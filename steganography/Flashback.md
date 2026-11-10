## Flashback

* **Category:** Steganography
* **Points:** 75
* **Files:** `flashback.swf`

### Challenge Description

An old-school Macromedia Flash game (`.swf`) consisting of a multi-stage maze game.

### Methodology

#### Step 1: The Red Herring

Playing through or disassembling the game reveals 5 maze levels. Beating level 5 displays a victory screen with the string:

`"THE MAZE WAS NEVER THE MESSAGE."`

Submitting this as the flag fails; as the author hinted, the game itself is merely a distraction for the real steganography hidden inside the Flash rendering engine.

#### Step 2: Reverse Engineering ActionScript Bytecode

Opening `flashback.swf` in JPEXS Free Flash Decompiler (`ffdec`), we inspected the tile drawing routine:

1. Inside `Maze.draw()`, the engine iterates through every coordinate `(x, y)` and calls `Maze.f3(color, x, y)`.
2. `f3` adds `+7` to the tile's Red color channel if `Maze.f2(x, y)` evaluates to false.
3. `f2(x, y)` reads a dynamic 89 × 89 bitmask matrix stored in `Maze.q4`.
4. In each of the 5 maze stages, `Maze.q4` is decrypted into memory using separate seeds and data arrays defined in `D7.c` and `D7.d`.

#### Step 3: Layer Analysis

Extracting the 5 bitmasks and comparing them:

* The active 1-bits across all 5 levels are **completely mutually exclusive** (zero bit collisions between levels).
* Combining all 5 layers with a bitwise OR reconstructs a single unified 89 × 89 binary matrix.

#### Step 4: Reconstructing the QR Code

An 89 × 89 grid is the exact dimension of a **Version 18 QR Code**:

**Dimension = 17 + 4 × Version = 17 + 4 × 18 = 89**

Examining the matrix confirmed standard Version 18 characteristics:

* Alignment pattern centers at coordinates (6, 30, 56, 82).
* Standard timing lines at row 6 and column 6.
* Format info indicators: Error Correction Level **H**, Mask Pattern **2** (`(row + col) % 3 == 0` / `col % 3 == 0`).
* The three standard 7 × 7 finder patterns in the corners had been removed by the author to prevent automated camera scanning.

We restored the three 7 × 7 finder patterns (with their 1-pixel separators), applied inverse Mask 2, de-interleaved the 13 QR codeword blocks, and decoded the 8-bit byte stream from Block 0.

### Decoder Script (Python)

```python
import numpy as np

# Reconstructed 89x89 matrix from the combined 5 ActionScript layers
# grid = np.load("combined_89x89.npy")

# Unmasking QR with Mask Pattern 2
for r in range(89):
    for c in range(89):
        if c % 3 == 0:
            grid[r, c] ^= 1

# Extracting data bits and decoding Byte Mode stream
# Block 0 yielded 34 ASCII bytes:
# Decoded: PINK{Fl4sh_Pl4y3r_escape_the_maze}
```

### Flag

```text
PINK{Fl4sh_Pl4y3r_escape_the_maze}
```

---
