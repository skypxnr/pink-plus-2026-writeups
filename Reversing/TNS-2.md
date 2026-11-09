# Analysis & Reverse Engineering Walkthrough

The challenge **TNS 2** (Category: calculator, Author: admin5559, 50 points, Description: *"I did some Painting... on my calculator"*) provides one file: `tns2_pink.tns`.

## 1. File Container & Method 13 Architecture

### TI-Nspire `.tns` container

- The header starts with the magic `*TIMLP0500`, and the directory ends with the signature `TIPD`.
- Underneath, it is a modified PK-ZIP archive with two entries: `Document.xml` and `Problem1.xml`.
- Both entries use compression method `0x000D` (**Method 13**), a TI-specific method, so a normal unzip fails.

### Method 13 unpacking

Method 13 is a layered construct:

1. **40-byte header:** encrypted with 3DES-ECB using a fixed TI master key:
   `79 c4 e0 f4 5e ef 7a 5b 70 13 7a 57 c2 fd 3d 2c c2 70 7c c1 ad 2f 15 75`.
   Decrypting it yields the marker `TIEN0100`, a block size of `0x400`, a counter seed and 21 bytes of packed key material (three 7-byte chunks, expanded to a 24-byte 3DES key with DES parity bits).
2. **Payload stream:** encrypted in a counter mode. For each 8-byte block, the keystream is `3DES(key, 00 00 00 00 || (seed + block_index % 0x400))`, XORed with the data.
3. **Compression:** the result is a raw DEFLATE stream (`zlib.decompress(data, -15)`).
4. **Token stream:** the inflated data starts with the marker `TIXC` and is a tokenized XML format, not plain text.

```python
import zlib, struct
from cryptography.hazmat.primitives.ciphers import Cipher, algorithms, modes

HK = bytes([0x79,0xc4,0xe0,0xf4,0x5e,0xef,0x7a,0x5b,0x70,0x13,0x7a,0x57,
            0xc2,0xfd,0x3d,0x2c,0xc2,0x70,0x7c,0xc1,0xad,0x2f,0x15,0x75])

def tdes(k):
    c = Cipher(algorithms.TripleDES(k), modes.ECB())
    return c.encryptor(), c.decryptor()

def parity(b):
    r = b & 0xfe
    return r | 1 if bin(r).count("1") % 2 == 0 else r

def expand(b):                       # 7 bytes -> 8 key bytes (DES parity)
    raw = [b[0]&0xfe, ((b[1]>>1)&0x7e)|((b[0]<<7)&0xff), ((b[2]>>2)&0x3e)|((b[1]<<6)&0xff),
           ((b[3]>>3)&0x1e)|((b[2]<<5)&0xff), ((b[4]>>4)&0x0e)|((b[3]<<4)&0xff),
           ((b[5]>>5)&0x06)|((b[4]<<3)&0xff), ((b[6]>>6)&0x02)|((b[5]<<2)&0xff), (b[6]<<1)&0xff]
    return bytes(parity(x) for x in raw)

def unpack_method13(blob):
    h = tdes(HK)[1].update(blob[:40])
    assert h[:8] == b"TIEN0100"
    seed = struct.unpack("<I", h[12:16])[0]
    km = h[16:37]
    key = b"".join(expand(km[i*7:i*7+7]) for i in range(3))
    enc = tdes(key)[0]
    body = bytearray(blob[40:])
    for bi in range(0, len(body), 8):
        ctr = b"\0"*4 + struct.pack("<I", (seed + (bi//8) % 0x400) & 0xffffffff)
        ks = enc.update(ctr)
        for j in range(min(8, len(body)-bi)):
            body[bi+j] ^= ks[j]
    return zlib.decompress(bytes(body), -15)      # -> TIXC token stream
```

The raw bytes of each Method 13 entry come out of the ZIP local header (compression method 13, `Problem1.xml` is the interesting one).

### Decoding the `TIXC` tokens

The TIXC grammar was decoded with an open-source implementation of the tokenizer (`tixc.rs` from the `rust-tns` project, compiled stand-alone). That reconstructs the original `Problem1.xml`, a TI-Nspire **Graphs & Geometry** document (`xmlns:gg="urn:TI.GeoGrapher"`).

## 2. The Red Herring vs. The Real "Painting"

In `Problem1.xml`, an off-screen text object contains:

```xml
<gg:text> ... <gg:chrs>PINK{th1s_15_4_f4k3_fl4g}</gg:chrs> ... </gg:text>
```

This is a **fake flag** meant to trap anyone who just dumps strings from the decoded file.

The prompt says *"I did some Painting... on my calculator"*. The geometry widget holds:

- **63** `<gg:point>` vertices
- **45** `<gg:segment>` lines (each references two point ids)
- **2** `<gg:text>` labels that are part of the real picture: `g_` and `S_ca`

Plotting every segment (point id -> `x`,`y`) in matplotlib shows hand-drawn letters:

```
Row 1:   [ P ] [ I ] [ N ] [ K ] [ { ] [ f ] [ L ]
Row 2:   [ A ]  "g_"  [ t ] [ n ]  "S_ca"  [ l ] [ c ] [ } ]
```

The two text labels are placed **between** the drawn letters. Overlaying their coordinates on the plot puts `g_` right after the `A` and `S_ca` right after the `n`.

Reading the whole thing in order:

```
P I N K {  f L A  g_  t n  S_ca  l c  }
-> PINK{fLAg_tnS_calc}
```

## Flag

```text
PINK{fLAg_tnS_calc}
```

(The `S` of the text label `S_ca` is uppercase in the file. If the platform rejects it, try the all-lowercase variant `PINK{fLAg_tns_calc}`.)

---

*Challenge file (`tns2_pink.tns`) belongs to the PINK+ CTF authors and is not included. Writeup published after the competition, in line with the event's rules. Challenge by admin5559, PINK+ CTF 2026 (`ctf.pink.bayern`).*
