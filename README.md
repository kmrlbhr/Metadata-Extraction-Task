# Metadata Analysis & File Forensics WalkThrough

This repository demonstrates fundamental techniques for analyzing file metadata, extracting hidden data (steganography), and verifying file signatures using common command-line forensics tools.

## 🛠️ Tools Used

| Tool | Purpose |
|------|---------|
| `ExifTool` | Reads, writes, and edits meta information in a wide variety of files |
| `Hexeditor` | Used for inspecting the raw binary data of files |
| `Binwalk` | A tool for searching a given binary image for embedded files and executable code |
| `Strings` | Extracts printable, human-readable character sequences from binary files |
| `File` | Determines file types based on magic numbers/file signatures rather than extensions |

---
### 1. `ocean.jpg` - Metadata Extraction
We used `ExifTool` to analyze the metadata of this JPEG.

```bash
exiftool ocean.jpg
```
<img width="1919" height="934" alt="Ocean(Exiftool)" src="https://github.com/user-attachments/assets/1fbabeb7-e833-4f71-9408-ca7f2829d474" />
* Inspecting the output revealed a hidden flag planted within the image's `Comment` field.

**Result** :

```bash
THIS IS THE HIDDEN FLAG
```
### 2. `computer.jpg` - Hex Analysis
We used `hexeditor` to analyze this image.

```bash
hexeditor computer.jpg
```
<img width="1918" height="935" alt="computer(hexeditor)" src="https://github.com/user-attachments/assets/6d595416-cf6e-4eb9-b75e-b45e0c11ffa8" />

Try to look for Enf of File (EOF):JPEG files officially end with the hex bytes FF D9

<img width="1917" height="935" alt="computer(hexeditor)FF D9" src="https://github.com/user-attachments/assets/87528c69-e6ea-4568-9b17-272e7b0515d6" />

**Result** :

```bash
The image file was opened in a hex editor to inspect its raw hexadecimal and ASCII data structures
```

### 3. `dog.jpg` - Extracting Embedded Archives
We Used `Binwalk` to check if any other files were hidden within the image binary.

* Check for hidden files

  ```bash
  binwalk dog.jpg
  ```

  <img width="951" height="123" alt="first binwalk" src="https://github.com/user-attachments/assets/14596042-c200-433c-98aa-64a0d758669b" />

* It successfully detected the hidden ZIP archive

  * Extract the hidden ZIP archive

  ```bash
  binwalk -e dog.jpg
  ```
  <img width="939" height="116" alt="second binwalk" src="https://github.com/user-attachments/assets/e55d7f01-3597-436c-bcf3-f70980b339a5" />
* Go to the extracted folder and cat hidden file

  ```bash
  cd _dog.jpg.extracted/
  ls
  cat hidden_text.txt
  ```
  <img width="456" height="90" alt="third binwalk" src="https://github.com/user-attachments/assets/f31b6fba-e71c-41a1-8b1b-398d42d5581b" />
  <img width="456" height="45" alt="forth binwalk" src="https://github.com/user-attachments/assets/c089db6e-adf6-40a1-87a5-825d80318c15" />

 **Result** :
  
  ```bash
  THIS IS A HIDDEN FLAG
  ```

### 4. `computer.jpg` - String Extraction
We used `strings` to quickly look for readable text hidden inside the image file.

```bash
strings computer.jpg
```
<img width="1918" height="936" alt="computer(strings)1" src="https://github.com/user-attachments/assets/ecdd091c-dd39-492b-9721-fe6d5d7f38e5" />

We can also recuding the noise by using:

```bash
strings -n 10 computer.jpg
```
<img width="1918" height="934" alt="computer(strings)reducing the noise" src="https://github.com/user-attachments/assets/186ac086-71ef-4bc9-aff1-d61cf213bd3c" />


`-n 10`:Tells the tool to only print text strings that are at least 10 characters long.This significantly cleans up the output, making it much easier to manually skim through the text to spot a hidden message.

**Result** : 

```bash
Extracted ICC profile information and various standard background strings embedded in the file structure
```

### 5. `solitaire.exe` - File Signature Verification
Malicious or deceptive files often hide behind spoofed extensions. We used the `file` command to check the true identity of this executable.

* Check the true file type of the executable

  ```bash
  file solitaire.exe
  ```

  <img width="610" height="65" alt="solitaire(wrong one)" src="https://github.com/user-attachments/assets/2ae80ad1-a6f2-4a30-b7e9-9dfe443f1d88" />

**Result** :
  
  ```bash
  The file `solitaire.exe` was identified as `PNG image data, 640 x 449, 8-bit/color RGBA, non-interlaced`
  ```
  
* To fix the spoofed extension and view the file properly, it was renamed to its proper `.png` format and run again check the true file type of the image:

  ```bash
  mv solitaire.exe solitaire.png
  ```
  ```bash
  file solitaire.png
  ```
  
 <img width="609" height="106" alt="solitaire(corrected)" src="https://github.com/user-attachments/assets/d1f657cb-4a63-43da-9d8a-8b77a927ec99" />

  
  **Result** :

  ```bash
  The file `solitaire.png` was identified as `PNG image data, 640 x 449, 8-bit/color RGBA, non-interlaced`
  ```

### 6. `rubiks.jpg` - File Signature Verification
Similarly, we checked the true file type of this image file using the file command.

* Check the true file type of the image

  ```bash
  file rubiks.jpg
  ```

  <img width="592" height="70" alt="rubiks(wrong one)" src="https://github.com/user-attachments/assets/6a9fdb41-dbbb-421b-8c85-b07d1ce1d462" />

  
  **Result** :

  ```bash
  The file `rubiks.jpg` was also identified as `PNG image data, 609 x 640, 8-bit/color RGBA, non-interlaced`
  ```
* To correct the extension, it was renamed to its proper `.png` format and run again check the true file type of the image:

  ```bash
  mv rubiks.jpg rubiks.png
  ```
  ```bash
  file rubiks.png
  ```

 <img width="607" height="109" alt="rubiks(corrected)" src="https://github.com/user-attachments/assets/9fc68a7a-1a4c-458a-a0aa-c19d52508034" />

  
  **Result** :

  ```bash
  The file `rubiks.jpg` was also identified as `PNG image data, 609 x 640, 8-bit/color RGBA, non-interlaced`
  ```
