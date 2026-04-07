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
