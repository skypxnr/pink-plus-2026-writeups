The challenge file PINK_HQ.zip contains a custom Among Us map file called PINK+_HQ.lim2, built using the LevelImposter map editor.

A .lim2 file consists of a header with the length of a JSON manifest (map.json) followed by binary asset blobs (each indexed by a 36-byte UUID, a 4-byte length integer, and the raw payload—mostly DXT1/DXT5 .dds textures, .png images, and .wav audio).

The flag was split into 12 numbered parts hidden across different map properties and asset files:

Detailed Step-by-Step Breakdown of the Parts

Part 1 (PIN):

Location: Texture asset 83a92fd6-1f6c-4c58-813b-9aa5977a7915 (background for the Haupthalle room).
Method: When converted from DXT1 and flipped vertically, coordinates (179, 442) reveal:
1
:
 
𝑃
𝐼
𝑁
1: PIN

Part 2 (K{):

Location: Texture asset 46d07d42-0450-4a81-b37f-543d23d36ced (the keycard sprite for the horizontal door sabotage).
Method: When flipped vertically and enhanced, faint text on the card reads:
2
:
 
𝐾
{
2: K{

Part 3 (Amo):

Location: Texture asset 7d39d135-a118-46ab-b201-541e9ab651e2 (the background for the Wires minigame).
Method: When flipped vertically, handwriting on the left reads:
3
:
 
𝐴
𝑚
𝑜
3: Amo

Part 4 (gus):

Location: map.json inside the util-button1 object (Skeld / Polos-Knopf emergency meeting button).
Method: Inspecting customText.EmergencyDuringCrisis:
json
"EmergencyDuringCrisis": "EMERGENCY MEETINGS CANNOT BE CALLED DURING CRISIS\n4: gus"
4
:
 
𝑔
𝑢
𝑠
4: gus

Part 5 (_su):

Location: Texture asset 4651bbbb-c65b-4920-b263-2b2a5dbe264c (the Minimap sprite).
Method: When flipped vertically, red lettering reads:
5
:
 
_
𝑠
𝑢
5: _su

Part 6 (s_p):

Location: Audio asset cf26471b-9769-439a-b68c-e9aa669bc487.wav (the custom sound attached to the vent in the Challenge Creation Room).
Method: Listening / speech analysis reveals a voice saying:
6
:
 
𝑠
_
𝑝
6: s_p

Part 7 (ink):

Location: Sprite f0b2c7f9-2512-46e0-8ba8-684260b430d8 (Item 4 in the vending machine: the blue drink bottle).
Method: Zooming into the black label on the bottle reveals:
7
:
 
𝑖
𝑛
𝑘
7: ink

Part 8 (_is):

Location: map.json layer and room names: ODogX2lz=.
Method: Base64 decoding ODogX2lz=:
base64_decode
(
"ODogX2lz="
)
→
8
:
 
_
𝑖
𝑠
base64_decode("ODogX2lz=")→8: _is

Part 9 (_the):

Location: Two sprites that fit together seamlessly:
Right side on 8d8af1d1-ff15-49aa-8723-0ac83062b894 (the water tile in the ODogX2lz= room).
Left side on 370b6722-06a5-4b40-acdd-0628959d1eeb (the Polus vent sprite).
Method: When placed side-by-side, they form the Base64 string OTogX3RoZQ==:
base64_decode
(
"OTogX3RoZQ=="
)
→
9
:
 
_
𝑡
ℎ
𝑒
base64_decode("OTogX3RoZQ==")→9: _the

Part 10 (_im):

Location: Sprite 8a713a3d-c7eb-476c-8889-d93d680af965 (Schwebender Sprite floating in The Big One room).
Method: When flipped vertically, the red text reads:
10
:
 
_
𝑖
𝑚
10: _im

Part 11 (pasta_):

Location: map.json inside the second collider of object 40c52fc0-1837-4b24-a89d-ec41d5503be6 in The Big One.
Method: Plotting the 128 
(
𝑥
,
𝑦
)
(x,y) coordinate points of the light-blocking collider reveals:
11
:
 
𝑃
𝐴
𝑆
𝑇
𝐴
_
11: PASTA_
(or pasta_)

Part 12 (fr}):

Location: Texture asset 5607d1b9-646d-4d25-8f6c-8f118ae3373a (Leeres Objekt outside the map, viewed by the camera Outside???).
Method: The texture consists of three groups of red squares:
Group 1: 
102
 squares
→
ASCII 
102
=
’f’
102 squares→ASCII 102=’f’
Group 2: 
114
 squares
→
ASCII 
114
=
’r’
114 squares→ASCII 114=’r’
Group 3: 
125
 squares
→
ASCII 
125
=
’}’
125 squares→ASCII 125=’}’
Reconstructed Flag

Putting all 12 pieces together in sequence:

1 + 2: PIN + K{ 
→
→ PINK{
3 + 4: Amo + gus 
→
→ Amogus
5 + 6 + 7: _su + s_p + ink 
→
→ _sus_pink
8: _is
9: _the
10 + 11: _im + pasta_ 
→
→ _impasta_
12: fr}
text
PINK{Amogus_sus_pink_is_the_impasta_fr}

(Note: If case-sensitive with the A in lowercase or pasta in uppercase, the alternate variations are PINK{amogus_sus_pink_is_the_impasta_fr} and PINK{Amogus_sus_pink_is_the_imPASTA_fr})
