
# PINK{} Challenge Writeup

The challenge file `PINK_HQ.zip` contains a custom Among Us map file called `PINK+_HQ.lim2`, built with the LevelImposter map editor.

## File Format

A `.lim2` file has a header containing the length of a JSON manifest (`map.json`), followed by binary asset blobs. Each blob is indexed by a 36-byte UUID and a 4-byte length integer, then the raw payload. Most payloads are DXT1/DXT5 `.dds` textures, with some `.png` images and `.wav` audio.

## Flag Parts

The flag is split into 12 numbered parts hidden across map properties and asset files.

### Part 1: `PIN`

- **Location:** Texture asset `83a92fd6-1f6c-4c58-813b-9aa5977a7915` (background for the Haupthalle room)
- **Method:** Convert from DXT1 and flip vertically. Coordinates (179, 442) reveal `PIN`.

### Part 2: `K{`

- **Location:** Texture asset `46d07d42-0450-4a81-b37f-543d23d36ced` (keycard sprite for the horizontal door sabotage)
- **Method:** Flip vertically and enhance. Faint text on the card reads `K{`.

### Part 3: `Amo`

- **Location:** Texture asset `7d39d135-a118-46ab-b201-541e9ab651e2` (background for the Wires minigame)
- **Method:** Flip vertically. Handwriting on the left reads `Amo`.

### Part 4: `gus`

- **Location:** `map.json`, inside the `util-button1` object (Skeld/Polus emergency meeting button)
- **Method:** Inspect `customText.EmergencyDuringCrisis`:

```json
"EmergencyDuringCrisis": "EMERGENCY MEETINGS CANNOT BE CALLED DURING CRISIS\n4: gus"
```

### Part 5: `_su`

- **Location:** Texture asset `4651bbbb-c65b-4920-b263-2b2a5dbe264c` (Minimap sprite)
- **Method:** Flip vertically. Red lettering reads `_su`.

### Part 6: `s_p`

- **Location:** Audio asset `cf26471b-9769-439a-b68c-e9aa669bc487.wav` (custom sound on the vent in the Challenge Creation Room)
- **Method:** Listen or run speech analysis. A voice says `s_p`.

### Part 7: `ink`

- **Location:** Sprite `f0b2c7f9-2512-46e0-8ba8-684260b430d8` (Item 4 in the vending machine, the blue drink bottle)
- **Method:** Zoom into the black label on the bottle. It reads `ink`.

### Part 8: `_is`

- **Location:** `map.json` layer and room names: `ODogX2lz=`
- **Method:** Base64 decode:

```python
base64_decode("ODogX2lz=")  # -> "8: _is"
```

### Part 9: `_the`

- **Location:** Two sprites that join seamlessly:
  - Right side: `8d8af1d1-ff15-49aa-8723-0ac83062b894` (water tile in the `ODogX2lz=` room)
  - Left side: `370b6722-06a5-4b40-acdd-0628959d1eeb` (Polus vent sprite)
- **Method:** Place them side by side to form the Base64 string `OTogX3RoZQ==`:

```python
base64_decode("OTogX3RoZQ==")  # -> "9: _the"
```

### Part 10: `_im`

- **Location:** Sprite `8a713a3d-c7eb-476c-8889-d93d680af965` (floating Schwebender sprite in The Big One room)
- **Method:** Flip vertically. Red text reads `_im`.

### Part 11: `pasta_`

- **Location:** `map.json`, second collider of object `40c52fc0-1837-4b24-a89d-ec41d5503be6` in The Big One
- **Method:** Plot the 128 (x, y) coordinate points of the light-blocking collider. They spell `PASTA_`, which can also be read as `pasta_`.

### Part 12: `fr}`

- **Location:** Texture asset `5607d1b9-646d-4d25-8f6c-8f118ae3373a` (Leeres Objekt outside the map, seen by the camera "Outside???")
- **Method:** The texture is made of three groups of red squares, each group mapping to an ASCII code:

| Group | Squares | ASCII | Character |
|-------|---------|-------|-----------|
| 1     | 102     | 102   | `f`       |
| 2     | 114     | 114   | `r`       |
| 3     | 125     | 125   | `}`       |

## Reconstructed Flag

| Parts    | Fragment                |
|----------|-------------------------|
| 1 + 2    | `PIN` + `K{`            |
| 3 + 4    | `Amo` + `gus`           |
| 5 + 6 + 7| `_su` + `s_p` + `ink`   |
| 8        | `_is`                   |
| 9        | `_the`                  |
| 10 + 11  | `_im` + `pasta_`        |
| 12       | `fr}`                   |

Combined in order:

```
PINK{Amogus_sus_pink_is_the_impasta_fr}
```
