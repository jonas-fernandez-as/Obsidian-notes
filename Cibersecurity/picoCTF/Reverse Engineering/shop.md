
#### Description

Best Stuff - Cheap Stuff, Buy Buy Buy... Store Instance: [source](https://mercury.picoctf.net/static/bd84b0d8b57e043a028c36381910d0b7/source). The shop is open for business at `nc mercury.picoctf.net 11371`.

#### Hint

Always check edge cases when programming

### Solution


First I downloaded the file , and I inspected  with ghidra, I really don't understood nothing. That language was very strange for me. I used another decompilers online  and ..no I wasn't able to understand nothing right now

I used `readelf -s source` 
and `file source` 
And I see that it was writen on go

```
source.bin: ELF 32-bit LSB executable, Intel 80386, version 1 (SYSV), statically linked, Go BuildID=r5IKmnk_hVFErwy5ewa3/PyI570w85RI5Xa1aSnrW/RxmeFbAluXa5Hnisdodi/MJQIt60cZyLjm5Wta-r0, with debug_info, not stripped
```

So at this point because of my lack of skills I wasn't able to understand the source code. But at least I was able to get the flag with a basic method

You have to buy some item and put a negative number and then the app will give you money
For example buy -1 aple and you will get $15.
With this simple method you will be able to buy the flag

```
Flag is:  [112 105 99 111 67 84 70 123 98 52 100 95 98 114 111 103 114 97 109 109 101 114 95 98 56 100 55 50 55 49 102 125]
```
Then decode It to ascii and finally get read the flag

```
picoCTF{******************************}
```





### Assets 

```
### En cuanto a tu pregunta, los archivos binarios son una representación de código máquina que puede ser ejecutada directamente por la computadora. 

### No contienen código fuente legible por humanos, sino instrucciones en un formato que la máquina puede entender y ejecutar

[1](https://www.arsys.es/blog/que-es-el-codigo-fuente-como-ver-el-codigo-fuente)

### .

### Si quieres entender qué hace un archivo binario, tienes algunas opciones:

1. **Desensamblado**: Puedes usar un desensamblador para traducir el código máquina en un lenguaje de ensamblado más legible. Sin embargo, este proceso no te dará el código fuente original y puede ser difícil de entender si no estás familiarizado con el lenguaje de ensamblado.

2. **Depuración**: Puedes usar un depurador para ejecutar el programa paso a paso y ver qué está haciendo. Esto puede darte una idea de cómo funciona el programa, pero no te proporcionará el código fuente original.

3. **Ingeniería inversa**: Este es un proceso más avanzado que implica desensamblar y luego analizar el código para entender cómo funciona. Este proceso es técnicamente desafiante y puede ser ilegal dependiendo de las leyes locales y los términos de la licencia del software.

### Si solo quieres ver la representación del archivo binario, puedes usar herramientas como 

`   ### hexdump   `

###  o 

`   ### xxd   `

###  en Linux

[2](https://www.tremplin-numerique.org/es/c%C3%B3mo-ver-archivos-binarios-desde-la-l%C3%ADnea-de-comandos-de-Linux)

### . 

### También hay una extensión para VS Code llamada HexEditor que te permite editar archivos binarios viendo su representación en hexadecimal

[3](https://es.stackoverflow.com/questions/368791/ver-el-contenido-de-un-fichero-binario)

### .

### Por favor ten en cuenta que estos métodos no te permitirán ver o recuperar el código fuente original a partir de un archivo binario

[1](https://www.arsys.es/blog/que-es-el-codigo-fuente-como-ver-el-codigo-fuente)

### . Si tienes acceso al código fuente original, esa sería la mejor manera de entender qué hace el programa.
```