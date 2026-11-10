
# PINK{} Challenge Writeup: Moskau

This challenge is based on the German song "Moskau" by Dschinghis Khan, known for the line "Kosaken hey, hey, hey, hebt die Gläser...".

The emojis translate the song's lyrics line by line, but some intruder emojis do not belong to the lyrics. Taking the first letter (in English) of each intruder emoji reveals the flag.

## Step-by-Step Breakdown

### Prefix / Opening Brace

| Emoji | Meaning | Letter |
|-------|---------|--------|
| 🍕 | Pizza | `P` |
| 🇮🇹 | Italy | `I` |
| 👃 | Nose | `N` |
| 🔑 | Key | `K` |
| 🫲 | Left-facing cupped hand | `{` |

### Word 1

| Emoji | Meaning | Letter |
|-------|---------|--------|
| 🍋 | Lemon | `L` |
| 🐘 | Elephant | `E` |
| 🦅 | Eagle | `E` |
| 🌈 | Rainbow | `R` |
| 🐯 | Tiger | `T` |
| ➖ | Minus/Dash | `_` |

### Word 2

| Emoji | Meaning | Letter |
|-------|---------|--------|
| 🐶 | Dog | `D` |
| ℹ️ | Info | `I` |
| ✉️ | Envelope / E-mail | `E` |
| ➖ | Minus/Dash | `_` |

### Word 3 & Closing Brace

| Emoji | Meaning | Letter |
|-------|---------|--------|
| 🍇 | Grape | `G` |
| 🦁 | Lion | `L` |
| 🍎 | Apple | `A` |
| 🌲 | Evergreen | `E` |
| 🐍 | Snake | `S` |
| 👀 | Eyes | `E` |
| 🐇 | Rabbit | `R` |
| 🫱 | Right-facing cupped hand | `}` |

## Flag

The hidden German phrase is "LEERT DIE GLÄSER" ("Empty the glasses"):

```

pink{LEERT_DIE_GLAESER}
```

If the platform is case-sensitive and rejects uppercase, try:

```
pink{leert_die_glaeser}
```
