# Description #

Files can always be changed in a secret way. Can you find the flag? [cat.jpg]

## Hints ##

-Look at the details of the file
-Make sure to submit the flag as picoCTF{XXXXX}


## Challenge ##

First you have to download a file .jpg that has an image of a cat, it's a picture.

Now you must see the metadata of the file with exiftool (in my case, because i'm using linux).

```
exiftool cat.jpg 
ExifTool Version Number         : 12.67
File Name                       : cat.jpg
Directory                       : .
File Size                       : 878 kB
File Modification Date/Time     : 2023:10:03 14:55:04-04:00
File Access Date/Time           : 2023:10:03 14:55:04-04:00
File Inode Change Date/Time     : 2023:10:03 14:55:04-04:00
File Permissions                : -rw-r--r--
File Type                       : JPEG
File Type Extension             : jpg
MIME Type                       : image/jpeg
JFIF Version                    : 1.02
Resolution Unit                 : None
X Resolution                    : 1
Y Resolution                    : 1
Current IPTC Digest             : 7a78f3d9cfb1ce42ab5a3aa30573d617
Copyright Notice                : PicoCTF
Application Record Version      : 4
XMP Toolkit                     : Image::ExifTool 10.80
License                         : cGljb0NURnt0aGVfbTN0YWRhdGFfMXNfbW9kaWZpZWR9
Rights                          : PicoCTF
Image Width                     : 2560
Image Height                    : 1598
Encoding Process                : Baseline DCT, Huffman coding
Bits Per Sample                 : 8
Color Components                : 3
Y Cb Cr Sub Sampling            : YCbCr4:2:0 (2 2)
Image Size                      : 2560x1598
Megapixels                      : 4.1

```

I the "license" field "cGljb0NURnt0aGVfbTN0YWRhdGFfMXNfbW9kaWZpZWR9" is the flag on base64. You must decode it and you will get the flag.

```
echo "cGljb0NURnt0aGVfbTN0YWRhdGFfMXNfbW9kaWZpZWR9" | base64 -d ; echo
```

`picoCTF{************************}`
