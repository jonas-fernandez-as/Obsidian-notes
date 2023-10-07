#### Description

Can you look at the data in this binary: [static](https://mercury.picoctf.net/static/ec4dbd8898ade34e1d60d5b70c1b8c8c/static)? This [BASH script](https://mercury.picoctf.net/static/ec4dbd8898ade34e1d60d5b70c1b8c8c/ltdis.sh) might help!

# Solution #

First of all we need to know what is a static binary:

[A static binary is a compiled version of a program that has been statically linked against libraries](https://en.wikipedia.org/wiki/Static_build)[1](https://en.wikipedia.org/wiki/Static_build).

When you statically link a file into an executable, the contents of that file are included at link time. [In other words, the contents of the file are physically inserted into the executable that you will run](https://en.wikipedia.org/wiki/Static_build)[2](https://stackoverflow.com/questions/311882/what-do-statically-linked-and-dynamically-linked-mean).

This is different from dynamic linking, where a pointer to the file being linked in (the file name of the file, for example) is included in the executable and the contents of said file are not included at link time. [It’s only when you later run the executable that these dynamically linked files are brought in and they’re only brought into the in-memory copy of the executable, not the one on disk](https://stackoverflow.com/questions/311882/what-do-statically-linked-and-dynamically-linked-mean)[2](https://stackoverflow.com/questions/311882/what-do-statically-linked-and-dynamically-linked-mean).

[Static binaries have their use in special cases (like the tools in your initial ramdisk that doesn’t have the infrastructure to support dynamic libraries) but other than that are not really in use and pretty much impossible to create for more complex programs due to plenty of libraries not supporting static linking](https://www.reddit.com/r/linux/comments/6pkzf5/static_and_dynamic_binaries/)[3](https://www.reddit.com/r/linux/comments/6pkzf5/static_and_dynamic_binaries/).

We have the binary and when you execute it you're not able to see the output.. you can see just a piece of information.

But we have a bash script

```
#!/bin/bash



echo "Attempting disassembly of $1 ..."


#This usage of "objdump" disassembles all (-D) of the first file given by 
#invoker, but only prints out the ".text" section (-j .text) (only section
#that matters in almost any compiled program...

objdump -Dj .text $1 > $1.ltdis.x86_64.txt


#Check that $1.ltdis.x86_64.txt is non-empty
#Continue if it is, otherwise print error and eject

if [ -s "$1.ltdis.x86_64.txt" ]
then
        echo "Disassembly successful! Available at: $1.ltdis.x86_64.txt"

        echo "Ripping strings from binary with file offsets..."
        strings -a -t x $1 > $1.ltdis.strings.txt
        echo "Any strings found in $1 have been written to $1.ltdis.strings.txt with file offset"



else
        echo "Disassembly failed!"
        echo "Usage: ltdis.sh <program-file>"
        echo "Bye!"
fi

```

So, the solution is very friendly for me , I just use the bash script and I put the static binary as argument, like this:


`./ltdis.sh static`

The script will make two .txt  files, I `cat`  the `static.ltdis.strings.txt` and I was able to see the flag.

`picoCTF{**********************}` 
