##  Sanity Check 2

* **Category:** Misc
* **Points:** 50

### Challenge Description

A sanity check hidden in plain sight within the official PINK+ CTF Discord server.

### Methodology & Solution

When checking the `#rules` / `#announcements` channel on Discord, the list of competition guidelines had a curious formatting choice with oddly capitalized first words and punctuation:

* **P**lease do not brute force flags...
* **{**Flag} is NOT the flag format...
* **S**haring flags is strictly forbidden...
* **m**...
* **4**...
* **r**...
* **t**...
* **_**...
* **B**...
* **0**...
* **y**...
* **!**...
* **}**...

Reading the first character of each rule bullet vertically reveals a classic acrostic cipher:

```text
P + { + S + m + 4 + r + t + _ + B + 0 + y + ! + }
```

This reconstructs the flag:

```text
PINK{Sm4rt_B0y!}
```

### Flag

```text
PINK{Sm4rt_B0y!}
```

---
