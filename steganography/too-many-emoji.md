# Too many Emoji: CTF Writeup

| | |
|---|---|
| **CTF** | PINK+ CTF |
| **Category** | Steganography |
| **Points** | 25 |
| **Authors** | Poris & QWERTZ |

## Challenge

The challenge shows only a large block of emojis and a flag submission box. No other text or files are given.

## Solution

1. Copy the emojis from the page as text. A photo or screenshot is too blurry to read reliably.
2. A long run of the same ten emojis keeps repeating between the other groups, so it looks like a separator:

   ```
   ☂🔩🐬🦅🚀🐍🐈🐙🚀🦅
   ```

3. Take the **Unicode name** of each emoji and keep its **first letter**.
4. The repeated run spells **UNDERSCORE**, so it stands for `_`. Each group between runs is one word of the flag.

## Key

Each emoji stands for the first letter of its official Unicode name.

| Emoji | Unicode name | Letter |
|---|---|---|
| 👛 | PURSE | P |
| 🏮 | IZAKAYA LANTERN | I |
| 🔩 | NUT AND BOLT | N |
| 👘 | KIMONO | K |
| 🐈 | CAT | C |
| ☂ | UMBRELLA | U |
| 🚀 | ROCKET | R |
| 🦁 | LION | L |
| 🐙 | OCTOPUS | O |
| 🦅 | EAGLE | E |
| 🐍 | SNAKE | S |
| 🐬 | DOLPHIN | D |

## Decoding

| Emoji group | Decoded |
|---|---|
| 👛🏮🔩👘 | PINK |
| 🐈☂🚀🦁💛🐻🚀⚓🐈🦅 + 🐙🐼🦅🔩 | CURLYBRACE + OPEN → `{` |
| 🍍💿🎀👿🎷 | PORIS |
| 🍭📥🐨💡🌱 | LIKES |
| 👽🎤🍢🎃🍨🎒 | EMOJIS |
| 🎁📖🐋 | WOW |
| 🍓😒🍫🎧 | SUCH |
| 🏺 | A |
| 🌵🐙💿🐆 | COOL |
| 🍒🌺👾💄📒 | CHALL |
| 🐈☂🚀🦁💛🐻🚀⚓🐈🦅 + 🐈🦁🐙🐍🦅 | CURLYBRACE + CLOSE → `}` |

Between every word, the repeated `☂🔩🐬🦅🚀🐍🐈🐙🚀🦅` run decodes to UNDERSCORE (`_`).

## Flag

```
PINK{PORIS_LIKES_EMOJIS_WOW_SUCH_A_COOL_CHALL}
```

> The case is not encoded in the emojis. If the platform rejects the uppercase version, try lowercase.

## Full emoji string

```
👛🏮🔩👘🐈☂🚀🦁💛🐻🚀⚓🐈🦅🐙🐼🦅🔩🍍💿🎀👿🎷☂🔩🐬🦅🚀🐍🐈🐙🚀🦅🍭📥🐨💡🌱☂🔩🐬🦅🚀🐍🐈🐙🚀🦅👽🎤🍢🎃🍨🎒☂🔩🐬🦅🚀🐍🐈🐙🚀🦅🎁📖🐋☂🔩🐬🦅🚀🐍🐈🐙🚀🦅🍓😒🍫🎧☂🔩🐬🦅🚀🐍🐈🐙🚀🦅🏺☂🔩🐬🦅🚀🐍🐈🐙🚀🦅🌵🐙💿🐆☂🔩🐬🦅🚀🐍🐈🐙🚀🦅🍒🌺👾💄📒🐈☂🚀🦁💛🐻🚀⚓🐈🦅🐈🦁🐙🐍🦅
```

## Quick solver (Python)

```python
import unicodedata

emojis = "<paste the emoji string here>"
sep = "☂🔩🐬🦅🚀🐍🐈🐙🚀🦅"  # decodes to UNDERSCORE

def decode(group):
    return "".join(unicodedata.name(c)[0] for c in group)

words = [decode(g) for g in emojis.split(sep)]
print("_".join(words))
# Replace CURLYBRACEOPEN with { and CURLYBRACECLOSE with }
```
