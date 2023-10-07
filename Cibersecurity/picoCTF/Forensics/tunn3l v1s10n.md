
#### Description


We found this [file](https://mercury.picoctf.net/static/09a86202e72dbdb5bf4d1b5d2c6a5b86/tunn3l_v1s10n). Recover the flag.

--------------------------------------------------

#### Hints

Weird that it won't display right...

-------------------------------



This file has no file extension. And when I used the command "file" with linux I get just 
```
tunn3l_v1s10n: data
```

The aproach that we have to do is go and inspect the bytes of the file , we need a hex editor

https://hexed.it/ (This tool has a file format identificator)

I imported it , and then i watched the first two characters(I think they're bytes) to check the  file type
```
42 4D 8E 86 2C 00 00 00
```

42 4D = BM

I googled what is BM

```  
The BMP format is an **uncompressed raster file designed to display high-quality images on Windows and store printable photos**.
````


so I add the .bmp extension to the file.. I wasnt able to see the content, but with this site: 
https://jumpshare.com/viewer/bmp I was able to see it


This was the image, I hope you can see it

https://jumpshare.com/s/0mIO8RwNiKJWXq2yDumc

Seems like the image was cutted. The size of this file is 2893454 bytes. Its very closer to 3 Mega. 

The next step is edit the height on the structure of bytes of the file , you can see on the assets that is telling something about the info of the headers, headers structures.

So you need to edit the 01 to 03 on the 0x17 offset fragment .
```
|Height|   |4 bytes|0016h|Vertical height of bitmap in pixels|
```

You will be able to see the full image with the real height. and you will be able to see the flag.


https://jumpshare.com/s/qgN1ebORKSz3AhIJdcPs


# Assets


https://www.ece.ualberta.ca/~elliott/ee552/studentAppNotes/2003_w/misc/bmp_file_format/bmp_file_format.htm


https://www.novell.com/documentation/ndsv8/usnds/c1help/novell_common/hexeditor.html


