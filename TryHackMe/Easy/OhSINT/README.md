# OhSINT

**Platform:** TryHackMe  
**Difficulty:** Easy  
**Category:** Red Team  

## Overview

Are you able to use open source intelligence to solve this challenge?

## Download Task Files 

![Task File](screenshots/1.jpg)

The provided image did not contain any visible clues. Therefore, metadata analysis was performed to extract hidden information.
### Command Used 
```bash
exiftool WindowsXP_1551719014755.jpg
```
### Result
```ansi
ExifTool Version Number         : 13.50
File Name                       : WindowsXP_1551719014755.jpg
Directory                       : .
File Size                       : 234 kB
File Modification Date/Time     : 2026:03:28 13:32:27+05:30
File Access Date/Time           : 2026:03:28 13:32:27+05:30
File Inode Change Date/Time     : 2026:03:28 13:32:27+05:30
File Permissions                : -rw-r--r--
File Type                       : JPEG
File Type Extension             : jpg
MIME Type                       : image/jpeg
XMP Toolkit                     : Image::ExifTool 11.27
GPS Latitude                    : 54 deg 17' 41.27" N
GPS Longitude                   : 2 deg 15' 1.33" W
Copyright                       : OWoodflint
Image Width                     : 1920
Image Height                    : 1080
Encoding Process                : Baseline DCT, Huffman coding
Bits Per Sample                 : 8
Color Components                : 3
Y Cb Cr Sub Sampling            : YCbCr4:2:0 (2 2)
Image Size                      : 1920x1080
Megapixels                      : 2.1
GPS Latitude Ref                : North
GPS Longitude Ref               : West
GPS Position                    : 54 deg 17' 41.27" N, 2 deg 15' 1.33" W
```
### Analysis

The extracted metadata revealed:

- **GPS Coordinates** → `54°17'41.27"N, 2°15'1.33"W`
- **Author Name** → `OWoodflint`

The presence of GPS coordinates indicates that the image contains location data, which can be used to identify a physical location.

The username `OWoodflint` may be useful for further OSINT investigation across online platforms.

---

### Conclusion

The image metadata provided two critical pieces of information:

1. A specific geographic location  
2. A potential username for further reconnaissance  

These findings guide the next steps of the investigation.
