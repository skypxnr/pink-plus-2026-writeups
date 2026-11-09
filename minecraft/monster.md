# Monster (Survey)

**Points:** 25
**Author:** QWERTZ
**Solves:** 1
**Upvotes:** 0

Thanks for participating in PINK+ CTF!

But... something isn't right???

[Challenge survey](https://docs.google.com/forms/d/e/1FAIpQLSdbwy6OPO6umTFXy9gLFX2yXmYPkxnnHPABuN7-vDlfNSAcAg/viewform)

> Please note that this challenge doesn't count as a tie breaker.

## Solution

### Step 1: The survey's hidden text

At the end of the survey, we are given this text:

```text
Thanks for filling out the survey... But why is this a Minecraft Challenge?
Rumors say the Guard protecting the Gate got knocked out!
And someone is trying to break the Gate? You should check on it!
You might need this spell against the Monster: "ANTIPINKMONSTER!"
```

This is the guard who was knocked out:

![Knocked-out guard](https://github.com/user-attachments/assets/fefd324b-ef7f-4ae8-9b41-33fee29afd12)

### Step 2: Reading the signs

Going through the gate and parkour-ing across various blocks, we find two wooden signs placed side by side on purpur pillars. They display a sequence of hexadecimal characters:

![Sign board](https://github.com/user-attachments/assets/5fb8da8a-3495-41cd-af7e-b8854a5e4908)

```text
[Left Sign]               [Right Sign]
E27D802A74858C4           E2A2837F71ABFD7
CBE44A750141244           26DCF3513E2926E
72DDAEAE661BF45           048683E4878146C
8786A2
```

### Step 3: Cryptographic analysis

**Key extraction.** The spell `ANTIPINKMONSTER!` is exactly 16 ASCII characters (16 bytes, or 128 bits):

```text
Key (hex): 414E544950494E4B4D4F4E5354455221
```

**Ciphertext formatting.** Reading the signs line by line across both boards gives 96 hex characters:

| Line | Left sign | Right sign | Total |
|------|-----------|------------|-------|
| 1 | `E27D802A74858C4` (15) | `E2A2837F71ABFD7` (15) | 30 |
| 2 | `CBE44A750141244` (15) | `26DCF3513E2926E` (15) | 30 |
| 3 | `72DDAEAE661BF45` (15) | `048683E4878146C` (15) | 30 |
| 4 | `8786A2` (6) | | 6 |

This totals 96 hex characters, or 48 bytes (3 × 16 bytes).

**Algorithm identification.** A 16-byte key paired with a 48-byte ciphertext and no Initialization Vector (IV) points to **AES-128-ECB**.

### Step 4: Decryption and OCR correction

Splitting the 96 hex characters into three 16-byte (32 hex character) blocks:

| Block | Hex | Decrypted (AES-ECB) |
|-------|-----|----------------------|
| 1 | `E27D802A74858C4E2A2837F71ABFD7CB` | `PINK{Th3_b1g_b4d` |
| 2 | `E44A7501412442BDCF3513E2926E72DD` | `_m0nst3r_1s_cr4z` |
| 3 | `AEAE661BF45048683E4878146C8786A2` | `y!}` + 13 bytes of `0x0d` padding |

**OCR correction:** A visual ambiguity in the default Minecraft pixel font makes `6` look like `B`. The right sign on line 2 should read `2BDCF...` instead of `26DCF...`, which gives the corrected block 2 hex shown above.

**Block 3 padding.** The final block decrypts to `y!}` followed by 13 bytes of PKCS#7 padding (`0x0d`), which is consistent with the flag's ending.

### Step 5: Solution script

```python
from Crypto.Cipher import AES
from Crypto.Util.Padding import unpad

key = b"ANTIPINKMONSTER!"
cipher = AES.new(key, AES.MODE_ECB)

ct_blocks = [
    "E27D802A74858C4E2A2837F71ABFD7CB",  # Block 1
    "E44A7501412442BDCF3513E2926E72DD",  # Block 2 (OCR-corrected: 6 -> B)
    "AEAE661BF45048683E4878146C8786A2",  # Block 3
]

plaintext = b"".join(cipher.decrypt(bytes.fromhex(b)) for b in ct_blocks)
flag = unpad(plaintext, AES.block_size).decode("utf-8")

print("Flag:", flag)
```

### Step 6: Flag

```text
PINK{Th3_b1g_b4d_m0nst3r_1s_cr4zy!}
```
