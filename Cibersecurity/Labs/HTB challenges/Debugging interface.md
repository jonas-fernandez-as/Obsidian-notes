
This is very easy on hack the box, but I really do not understand what is going on here.

First we have a zip file, when we unzip it, we got another one called "debugging_interface_signal.sal". If you put  the command 
```
file debugging_interface_signal.sal
```
You will see this:

```
debugging_interface_signal.sal: Zip archive data, at least v2.0 to extract, compression method=deflate
```
So I have to unzip it.. I didn't get the way to unzip it from the command line, this is not a zip file , or tar , or gzip.  

When you unzip it you get two files more 
```
1- digital-0.bin  
2- meta.json
```
The json file haves a lot of data, but i didn't get what to do with it .

The digital-0.bin I investigate it with the "strings" method , and I analyze the head of the file

```
kali> strings digital-0.bin | head
```

And this was the output
```
<SALEAE>
ffffff
~L@Y
L@LALALAL@LALAY
LAL@LAL@Y
LALALAL@LALAeDeDLAL@LALALAL@eEL@LALALA@
        LAY
LAL@LA~HLAL@LALAY
L@LAeDLALAeDL@LBL@LAY
LALAY

```
### About strings ###
**Muestra las cadenas de caracteres imprimibles que contengan los ficheros**, útil para visualizar ficheros que no sean, o que no se sepa que son de texto plano.

## About SALEAE ##
El Saleae Logic 8 **es un potente analizador lógico que permite grabar y mostrar señales en el circuito para que puedas depurarlo rápidamente**. Desde proyectos Arduino a sistemas de control aeronáuticos, más de 20.000 profesionales y entusiastas utilizan Logic cada mes para depurar y comprender sus diseños eléctricos.

### About logic analizers ###

Un **analizador lógico** se inicia cuando en el circuito digital a analizar se da una determinada condición **lógica**. En ese momento el **analizador** copia una gran cantidad de datos digitales del sistema al que está conectado. Más tarde será posible visualizar estos datos e incluso ver el **diagrama de** flujo del sistema.

So, it is a logic analizer, called "SALEAE". I need to download one and so I can get what It is.

![[Screenshot_2023-09-04_21_49_49.png]]

This is what i get from that app, and now I read on a website something like this

```
I calculated the bit rate by going to the start of the data. I hovered my mouse over the first block of data and saw a value of `32.02 µs`. However, that is microseconds, and SALEAE needs seconds. So, I divided 1,000,000 by 32.02 to get `31230 Bit/s`.
```


You need to put the Async Analyzer with the 31230 and the channel 0

![[Screenshot_2023-09-04_21_56_00.png]]

And this is the flag:

```
HTB{d38u991n9_1n732f4c35_c4n_83_f0und_1n_41m057_3v32y_3m83dd3d_d3v1c3!!52}
```
