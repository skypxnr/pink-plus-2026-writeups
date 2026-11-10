
# TNS 2 Challenge Writeup

**Category:** Calculator
**Author:** admin5559
**Description:** "I did some Painting... on my calculator"

The challenge provides a file named `tns2_pink.tns`.

## 1. File Container and Method 13 Architecture

### TI-Nspire `.tns` Container

The header begins with the magic bytes `*TIMLP0500`, and the end-of-directory signature is `TIPD`. Underneath, the file is a modified PK-ZIP archive with two entries:

- `Document.xml`
- `Problem1.xml`

Both entries use compression method `0x000D` (Method 13).

### Method 13 Unpacking

Method 13 is a four-stage layered construction:

1. **40-byte header:** Encrypted with 3DES-ECB using the fixed TI master key:
   `79 c4 e0 f4 5e ef 7a 5b 70 13 7a 57 c2 fd 3d 2c c2 70 7c c1 ad 2f 15 75`
   Decrypting the header yields the magic `TIEN0100`, a block size of `0x400`, a counter seed, and 21 bytes of packed 3DES key material.
2. **Payload stream:** Encrypted with a 3DES keystream in counter XOR mode, using `\x00\x00\x00\x00` + seed + block index as the counter.
3. **Compression:** Raw DEFLATE stream, decompressed with `zlib.decompress(data, -15)`.
4. **Token stream:** A tokenized XML stream beginning with `TIXC0100-1.0?>`.

Decoding the `TIXC0100` token grammar reconstructs the original `Problem1.xml` (57,640 bytes).

## 2. The Red Herring vs. The Real "Painting"

`Problem1.xml` contains an off-screen text object:

```xml
<gg:text><gg:id>132</gg:id><gg:chrs>... PINK{th1s_15_4_f4k3_fl4g}</gg:chrs></gg:text>
```

This is a decoy planted to trap automated string dumpers.

The prompt says "I did some Painting... on my calculator." Inside the `<wdgt xmlns:gg="urn:TI.GeoGrapher">` widget, the author drew geometry using 63 `<gg:point>` vertices, 45 `<gg:segment>` lines, and inline text labels.

### Layout

```
Row 1:   [ P ]  [ I ]  [ N ]  [ K ]  [ { ]  [ f ]  [ L ]
Row 2:   [ A ]  "g_"   [ t ]  [ n ]  "S_ca" [ l ]  [ c ]  [ } ]
```

### Drawn Glyphs

| Glyph | Description |
|-------|-------------|
| `P`, `I`, `N`, `K`, `{` | Drawn directly |
| `f` | Vertical stem with a crossbar extending across both sides |
| `L` | Vertical stroke with a bottom horizontal bar |
| `A` | Slanted legs with a middle crossbar |
| `t` | Vertical stroke with a bottom hook and crossbar |
| `n` | Stem with an arch |
| `l` | Vertical stroke with a bottom serif |
| `c` | Half-height square bracket shape |
| `}` | Closing brace |

### Text Labels

- `g_`
- `S_ca`

## 3. Assembling the Flag

Combining the segments and text labels in order:

```
PINK + { + fLA + g_ + tn + S_ca + lc + }
```

Result:

```
PINK{fLAg_tnS_calc}
```

If the platform treats the drawn `f` as uppercase, the flag is `PINK{FLAg_tnS_calc}`. If it is case-insensitive, the flag is `PINK{flag_tns_calc}`.

## Flag

```
PINK{fLAg_tnS_calc}
```