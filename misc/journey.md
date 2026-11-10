## Journey

* **Category:** Misc
* **Points:** 75
* **Files:** `gogogo.journey`

### Challenge Description

A multi-stage polyglot reverse-engineering gauntlet where each solved stage decodes and executes the next programming language in the chain.

### Methodology & The 9 Stages

<img width="1389" height="117" alt="image" src="https://github.com/user-attachments/assets/4d647387-7959-4cfb-b7db-5599a304bfa2" />

#### Stage 1: Go (`gogogo.journey`)

The initial file is a Go binary. Disassembly shows a hex-encoded byte array decrypted using a combination of bitwise rotations and affine modular arithmetic. Decoding the buffer outputs a C source file: `standard.journey`.

#### Stage 2: C (`standard.journey`)

A C source containing large initialized arrays. A seeded pseudo-random number generator (PRNG) produces a keystream mixed with salt constants to decrypt the next payload into `snake.journey`.

#### Stage 3: Python (`snake.journey`)

A Python script that executes a 10-round decompression loop:

* Each round computes a SHA-256 hash keystream, XORs it against the payload, and applies `zlib.decompress()`.
* Output: `rusted.journey`.

#### Stage 4: Rust (`rusted.journey`)

A compiled Rust binary. It splits its data into 16-byte blocks, applies a rolling index-based byte shuffle with right rotations, and writes out `classic.journey`.

#### Stage 5: Haskell (`classic.journey`)

A functional Haskell script. It groups hex-encoded strings and performs index-offset byte subtractions (`(byte - idx) & 0xFF`), outputting a Java class file: `coffee.journey` (`CappuccinoJourney.java`).

#### Stage 6: Java (`coffee.journey`)

A Java program using cryptographic primitives:

* Uses `PBKDF2WithHmacSHA256` to derive an AES-256 key from a hardcoded passphrase and salt.
* Decrypts an `AES/GCM/NoPadding` ciphertext with authenticated associated data (AAD).
* Output: `awake.journey`.

#### Stage 7: Brainfuck (`awake.journey`)

A Brainfuck script composed entirely of `+`, `-`, `>`, `<`, and `.`. Running it through an interpreter produces a valid Base64 string. Decoding the Base64 data extracts `ruby.journey`.

#### Stage 8: Ruby (`ruby.journey`)

A Ruby script that checks for a custom magic header `JRNYv2`. It decrypts an embedded container and drops the final file: `healthy_food.journey`.

#### Stage 9: Emojicode (`healthy_food.journey`)

An esoteric Emojicode program. Character values are calculated by XORing fruit emoji constants (`mango`, `guava`, `papaya`, `lychee`, `durian`, `kumquat`, `fig`, `quince`) against base numerical values:

* `111 XOR 63 = 80` → `'P'`
* `118 XOR 63 = 73` → `'I'`
* `113 XOR 63 = 78` → `'N'`
* `116 XOR 63 = 75` → `'K'`
* `68 XOR 63 = 123` → `'{'`
* ... and so on, reconstructing the full flag.

### Flag

```text
PINK{Th3_J0urn3y_m4y_n3v3r_3nd!!}
```

---
