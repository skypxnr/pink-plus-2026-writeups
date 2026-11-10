To solve the TNS challenge, we examine the provided TI-Nspire calculator files (lib_pink.tns and pink.tns).

Analysis & Solution

Format Identification: TI-Nspire .tns document files are ZIP archives using custom header signatures (*TIMLP... instead of PK\x03\x04 and TIPD instead of PK\x05\x06).

Extracting the Archive Contents: Inside the archives, we find Python scripts used by the TI-Nspire Python app:

lib_pink.tns contains lib_pink.py:
python
alphabet = "jXOsE_Q7YB32KVqRGFUx15IcltpCNg8{HMz49baWkiTud60vmhnLAoSZDyewr}PJf"
def get(list):
  if list == "alphabet":
    return alphabet
   - **`pink.tns`** contains **`pink.py`**:
     ```python
     import lib_pink as pinkctf
     e = pinkctf.get("alphabet")
     c = "62 22 28 12 31 0 43 3 25 5 35 5 3 20 48 26 24 10 5 26 60 46 29 60 35 48 5 20 5 59 60 46 25 10 5 46 50 5 48 57 5 50 10 59 5 23 35 24 23 43 24 35 25 46 60 61"
     if c == "":
       c = input("Ciphertext: ")
     f = ""
     for n in c.split():
       f += e[int(n)]
     print(f)
     ```
3. **Decoding the Ciphertext:**
   Mapping each index in `c` to the character in `alphabet`:
python
alphabet = "jXOsE_Q7YB32KVqRGFUx15IcltpCNg8{HMz49baWkiTud60vmhnLAoSZDyewr}PJf"
c = "62 22 28 12 31 0 43 3 25 5 35 5 3 20 48 26 24 10 5 26 60 46 29 60 35 48 5 20 5 59 60 46 25 10 5 46 50 5 48 57 5 50 10 59 5 23 35 24 23 43 24 35 25 46 60 61"
flag = "".join(alphabet[int(i)] for i in c.split())
print(flag)
Flag
text
PINK{just_4_s1mpl3_pr0gr4m_1_wr0t3_0n_my_n3w_c4lcul4t0r}
