
# Cyber Challenge Writeup

**Category:** OSINT
**Points:** 100
**CTF:** PINK+ CTF

The challenge description, "If we don't step on the gas, then it'll simply be too late," quotes line 8 of Strophe 1 of *Cyber Gangsta's Paradise*:

> Wenn wir nicht bald Gas geb'n, dann is' es halt zu spät!

The challenge references two cybersecurity awareness songs by Prof. Dr. Dominik Merli, Professor for IT Security at the Technical University of Applied Sciences Augsburg (THA), Bavaria:

- **Gangsta:** *Cyber Gangsta's Paradise*, a parody of Coolio's *Gangsta's Paradise* featuring his alter ego MC BlackHat.
- **Stark:** *Cyberstark*, Prof. Merli versus MC BlackHat, focusing on human factors and cyber resilience. It includes the line "Cyberstarke Menschen machen Organisationen resilient."

## Coordinate Scheme

Each coordinate has the format:

```

[Song] : [Section] : [Line Number] : [Character Index (1-based)]
```

- **Sections:** `1`, `2`, `3` = Strophe 1, 2, 3; `R` = Refrain; `B` = Bridge
- **Hyphens:** Characters are counted with hyphens included, unless noted otherwise.

## Decoding

| Coordinate | Line Text (excerpt) | Char |
|------------|---------------------|------|
| `Stark:1:3:1` | Phishing, Smishing, Quishing und mehr, | `P` |
| `Gangsta:1:7:1` | Ist das wirklich Qualität, ist das alles was hier geht? | `I` |
| `Stark:B:4:7` | WIR SIND CYBERSTARK! WIR GEBEN NIEMALS AUF! | `N` |
| `Gangsta:1:9:1` | Kriminelle haben Dollarzeichen in den Augen, | `K` |
| `{` | | `{` |
| `Stark:R:2:1` | Wir sind cyberstark - haben Cyber-Mut! | `W` |
| `Gangsta:3:2:16` | Black Hats machen Business, wir sind ja keine Zocker. | `e` |
| `_` | | `_` |
| `Gangsta:B:1:2` | Warum sind wir blind, während Zeit verrinnt, | `a` |
| `Gangsta:R:3:14` | Letztlich führt nur Mut und Fleiß raus aus Cyber... | `r` |
| `Stark:2:2:5` | An mein Laptop lass ich niemand ran! | `e` |
| `_` | | `_` |
| `Stark:B:4:10` | WIR SIND CYBERSTARK! WIR GEBEN NIEMALS AUF! | `C` |
| `Stark:R:2:30` | Wir sind cyberstark - haben Cyber-Mut! | `y` |
| `Gangsta:3:4:2` | aber es trifft nur Deine Kunden - Du hast nicht... | `b` |
| `Gangsta:2:1:2` | Wer weiß bei Euch Bescheid, über Cybersicherheit, | `e` |
| `Stark:B:3:3` | Ihr seid angreifbar! - Wir die Katze, ihr die Maus! | `r` |
| `Gangsta:1:11:3` | Wir müssen jetzt was tun, wir dürfen nicht mehr ruh'n, | `r` |
| `Gangsta:B:1:37` | Warum sind wir blind, während Zeit verrinnt, | `e` |
| `Stark:1:1:9` | Ha! Erwischt! So sieht's aus! | `s` |
| `Stark:2:6:6` | USB-Sticks steck ich niemals an. | `i` (hyphen ignored) |
| `Stark:B:1:34` | Ihr seid - Ha? - Das ist doch wohl ein Witz! | `l` |
| `Gangsta:2:12:10` | Dann schließt sie! Und macht den Elefanten so... | `i` |
| `Gangsta:1:1:42` | Jeden Tag, in der Zeitung und auf heise.de, | `e` |
| `Gangsta:R:1:12` | IoT um jeden Preis führt zu Cyber Gangsta's Paradise | `n` |
| `Stark:2:2:12` | An mein Laptop lass ich niemand ran! | `t` |
| `Stark:B:2:45` | Ihr seid - Was? - Tatsächlich checkt Ihr nix! | `!` |
| `}` | | `}` |

## Assembling the Flag

Reading the characters in order:

```

P I N K { W e _ a r e _ C y b e r s i l i e n t ! }
```

```
PINK{We_are_Cyberresilient!}
```

**Note on `USB-Sticks`:** Counting the hyphen gives `t` at index 6. Ignoring the hyphen gives `i`. The flag uses `i`, so the hyphen must be ignored. If the platform checks literal raw indexing, the result is `PINK{We_are_Cyberrestlient!}`.

## Flag

```
PINK{We_are_Cyberresilient!}
```
