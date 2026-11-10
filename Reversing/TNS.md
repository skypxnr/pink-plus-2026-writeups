Analysis & Reverse Engineering Walkthrough

The challenge TNS 2 (Category: calculator, Author: admin5559, Description: "I did some Painting... on my calculator") provides the file tns2_pink.tns.

1. File Container & Method 13 Architecture

TI-Nspire .tns Container:

Inspecting the header reveals the magic bytes *TIMLP0500 and end-of-directory signature TIPD.
Underneath, the file is a modified PK-ZIP archive containing two entries:
Document.xml
Problem1.xml
Both entries use compression method 0x000D (Method 13).

TI-Nspire Method 13 Unpacking: Method 13 consists of a four-stage layered construct:

40-byte Header: Encrypted using 3DES-ECB with a fixed TI master key: 79 c4 e0 f4 5e ef 7a 5b 70 13 7a 57 c2 fd 3d 2c c2 70 7c c1 ad 2f 15 75
Decrypting the header yields TIEN0100, block size 0x400, counter seed, and 21 bytes of packed 3DES key material.
Payload Stream: Encrypted with a 3DES keystream counter XOR mode (\x00\x00\x00\x00 + seed + block_index).
Compression: Raw DEFLATE stream (zlib.decompress(data, -15)).
Token Stream: Tokenized XML stream starting with TIXC0100-1.0?>.

Decompressing to XML: Decoding the TIXC0100 token grammar reconstructs the exact original Problem1.xml (57,640 bytes).

2. The Red Herring vs. The Real "Painting"

In Problem1.xml, there is an off-screen text object:

xml
<gg:text><gg:id>132</gg:id><gg:chrs>... PINK{th1s_15_4_f4k3_fl4g}</gg:chrs></gg:text>

This is a fake flag planted to trap automated string dumpers.

The challenge prompt states: "I did some Painting... on my calculator". Inside the <wdgt xmlns:gg="urn:TI.GeoGrapher"> widget, the author drew geometric vectors using 63 <gg:point> vertices, 45 <gg:segment> lines, and inline text labels:

Row 1:   [ P ]  [ I ]  [ N ]  [ K ]  [ { ]  [ f ]  [ L ]
Row 2:   [ A ]  "g_"   [ t ]  [ n ]  "S_ca" [ l ]  [ c ]  [ } ]
Drawn segments:
P, I, N, K, {
f (vertical stem with crossbar extending across both sides)
L (vertical with bottom horizontal bar)
A (slanted legs with middle crossbar)
t (vertical with bottom hook and crossbar)
n (stem with arch)
l (vertical stroke with bottom serif)
c (half-height square bracket shape)
} (closing brace)
Text labels embedded:
g_
S_ca

Combining the segments and text in order:
PINK
+
{
+
fLA
+
g_
+
tn
+
S_ca
+
lc
+
}
PINK+{+fLA+g_+tn+S_ca+lc+}

Flag
text
PINK{fLAg_tnS_calc}

(If the platform treats the drawn f as uppercase: PINK{FLAg_tnS_calc}, or case-insensitive: PINK{flag_tns_calc}).
