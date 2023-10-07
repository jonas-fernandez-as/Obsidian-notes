
#### Description

Matryoshka dolls are a set of wooden dolls of decreasing size placed one inside another. What's the final one? Image: [this](https://mercury.picoctf.net/static/205adad23bf9d8303081a0e71c9beab8/dolls.jpg)

### Hints

-Wait, you can hide files inside files? But how do you find them?

-Make sure to submit the flag as picoCTF{XXXXX}


# Solution #

FIrst you start downloading the image. 

I use exiftools 


```
ExifTool Version Number         : 12.67
File Name                       : dolls.jpg
Directory                       : .
File Size                       : 652 kB
File Modification Date/Time     : 2023:10:05 20:52:26-04:00
File Access Date/Time           : 2023:10:05 21:52:59-04:00
File Inode Change Date/Time     : 2023:10:05 21:48:15-04:00
File Permissions                : -rwxr--r--
File Type                       : PNG
File Type Extension             : png
MIME Type                       : image/png
Image Width                     : 594
Image Height                    : 1104
Bit Depth                       : 8
Color Type                      : RGB with Alpha
Compression                     : Deflate/Inflate
Filter                          : Adaptive
Interlace                       : Noninterlaced
Profile Name                    : ICC Profile
Profile CMM Type                : Apple Computer Inc.
Profile Version                 : 2.1.0
Profile Class                   : Display Device Profile
Color Space Data                : RGB
Profile Connection Space        : XYZ
Profile Date Time               : 2020:06:09 12:08:45
Profile File Signature          : acsp
Primary Platform                : Apple Computer Inc.
CMM Flags                       : Not Embedded, Independent
Device Manufacturer             : Apple Computer Inc.
Device Model                    : 
Device Attributes               : Reflective, Glossy, Positive, Color
Rendering Intent                : Perceptual
Connection Space Illuminant     : 0.9642 1 0.82491
Profile Creator                 : Apple Computer Inc.
Profile ID                      : 0
Profile Description             : Display
Profile Description ML (hr-HR)  : LCD u boji
Profile Description ML (ko-KR)  : 컬러 LCD
Profile Description ML (nb-NO)  : Farge-LCD
Profile Description ML (hu-HU)  : Színes LCD
Profile Description ML (cs-CZ)  : Barevný LCD
Profile Description ML (da-DK)  : LCD-farveskærm
Profile Description ML (nl-NL)  : Kleuren-LCD
Profile Description ML (fi-FI)  : Väri-LCD
Profile Description ML (it-IT)  : LCD colori
Profile Description ML (es-ES)  : LCD color
Profile Description ML (ro-RO)  : LCD color
Profile Description ML (fr-CA)  : ACL couleur
Profile Description ML (uk-UA)  : Кольоровий LCD
Profile Description ML (he-IL)  : LCD צבעוני
Profile Description ML (zh-TW)  : 彩色LCD
Profile Description ML (vi-VN)  : LCD Màu
Profile Description ML (sk-SK)  : Farebný LCD
Profile Description ML (zh-CN)  : 彩色LCD
Profile Description ML (ru-RU)  : Цветной ЖК-дисплей
Profile Description ML (en-GB)  : Colour LCD
Profile Description ML (fr-FR)  : LCD couleur
Profile Description ML (hi-IN)  : रगन LCD
Profile Description ML (th-TH)  : LCD ส
Profile Description ML (ca-ES)  : LCD en color
Profile Description ML (en-AU)  : Colour LCD
Profile Description ML (es-XL)  : LCD color
Profile Description ML (de-DE)  : Farb-LCD
Profile Description ML          : Color LCD
Profile Description ML (pt-BR)  : LCD Colorido
Profile Description ML (pl-PL)  : Kolor LCD
Profile Description ML (el-GR)  : Έγχρωμη οθόνη LCD
Profile Description ML (sv-SE)  : Färg-LCD
Profile Description ML (tr-TR)  : Renkli LCD
Profile Description ML (pt-PT)  : LCD a Cores
Profile Description ML (ja-JP)  : カラーLCD
Profile Copyright               : Copyright Apple Inc., 2020
Media White Point               : 0.94955 1 1.08902
Red Matrix Column               : 0.51099 0.23955 -0.00104
Green Matrix Column             : 0.29517 0.69981 0.04224
Blue Matrix Column              : 0.15805 0.06064 0.78369
Red Tone Reproduction Curve     : (Binary data 2060 bytes, use -b option to extract)
Video Card Gamma                : (Binary data 48 bytes, use -b option to extract)
Native Display Info             : (Binary data 62 bytes, use -b option to extract)
Chromatic Adaptation            : 1.04861 0.02332 -0.05034 0.03018 0.99002 -0.01714 -0.00922 0.01503 0.75172
Make And Model                  : (Binary data 40 bytes, use -b option to extract)
Blue Tone Reproduction Curve    : (Binary data 2060 bytes, use -b option to extract)
Green Tone Reproduction Curve   : (Binary data 2060 bytes, use -b option to extract)
Exif Byte Order                 : Big-endian (Motorola, MM)
X Resolution                    : 144
Y Resolution                    : 144
Resolution Unit                 : inches
User Comment                    : Screenshot
Exif Image Width                : 594
Exif Image Height               : 1104
Pixels Per Unit X               : 5669
Pixels Per Unit Y               : 5669
Pixel Units                     : meters
XMP Toolkit                     : XMP Core 5.4.0
Apple Data Offsets              : (Binary data 28 bytes, use -b option to extract)
Warning                         : [minor] Trailer data after PNG IEND chunk
Image Size                      : 594x1104
Megapixels                      : 0.656
```
I used another tool, because the hint was telling me that here is a hidden file.
binwalk ./doll.jpg. 

And this was the output
```
DECIMAL       HEXADECIMAL     DESCRIPTION
--------------------------------------------------------------------------------
0             0x0             PNG image, 594 x 1104, 8-bit/color RGBA, non-interlaced
3226          0xC9A           TIFF image data, big-endian, offset of first image directory: 8
272492        0x4286C         Zip archive data, at least v2.0 to extract, compressed size: 378952, uncompressed size: 383937, name: base_images/2_c.jpg
651610        0x9F15A         End of Zip archive, footer length: 22

```

Look at this line

`size: 378952, uncompressed size: 383937, name: base_images/2_c.jpg`

We have a folder called base_image, and inside a file called 2_c.jpg


Now ,based on the hints I just changed the extension of the file from .jpg to .zip  and I was able to unzip the file. 

`unzip dolls.zip`

Then i had to repeat the process four times, and finally I get the flag.

`flag.txt`

picoCTF{96fac089316e094d41ea046900197662} 








# Assets #

1. [**Comando strings**: Este comando imprimirá cualquier cadena imprimible en un archivo, lo que podría indicar algunos archivos ocultos, mensajes o contenido](https://superuser.com/questions/803225/how-to-find-detect-hidden-files-inside-jpeg-file)[1](https://superuser.com/questions/803225/how-to-find-detect-hidden-files-inside-jpeg-file). Para usar este comando, simplemente escribe “strings” seguido de la ruta al archivo de imagen. [Por ejemplo, si el archivo de imagen se encuentra en el directorio actual, escribirías "strings ./image.jpg"](https://www.systranbox.com/how-to-extract-hidden-files-from-jpg-linux/)[2](https://www.systranbox.com/how-to-extract-hidden-files-from-jpg-linux/).
    
2. [**Comando binwalk**: Este comando está específicamente diseñado para extraer archivos de imágenes binarias](https://www.systranbox.com/how-to-extract-hidden-files-from-jpg-linux/)[2](https://www.systranbox.com/how-to-extract-hidden-files-from-jpg-linux/). Para usar este comando, simplemente escribe “binwalk” seguido de la ruta al archivo de imagen. [Por ejemplo, si el archivo de imagen se encuentra en el directorio actual, escribirías "binwalk ./image.jpg"](https://www.systranbox.com/how-to-extract-hidden-files-from-jpg-linux/)[2](https://www.systranbox.com/how-to-extract-hidden-files-from-jpg-linux/).
    
3. [**Comando foremost**: Este comando también está diseñado para extraer archivos de imágenes binarias](https://www.systranbox.com/how-to-extract-hidden-files-from-jpg-linux/)[2](https://www.systranbox.com/how-to-extract-hidden-files-from-jpg-linux/). Para usar este comando, simplemente escribe “foremost” seguido de la ruta al archivo de imagen. [Por ejemplo, si el archivo de imagen se encuentra en el directorio actual, escribirías "foremost ./image.jpg"](https://www.systranbox.com/how-to-extract-hidden-files-from-jpg-linux/)[2](https://www.systranbox.com/how-to-extract-hidden-files-from-jpg-linux/).