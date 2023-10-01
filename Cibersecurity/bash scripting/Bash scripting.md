ls (means list)
ls -l (L MEANS LONG)
pwd means (Print Working Directory)


## BASICS: ##

This comand repeats the text that we writed

```r
cds@cds:~$ echo Hello
Hello

```

Read a file

```c
cds@cds:~$ cat textfile.txt 
hello world!
cds@cds:~$ 
```

run a script writed on bash

```c
cds@cds:~/bashcourses$ bash sheltest.sh 

Hello World !

```

It shows which is our predeterminated shell 
```c

cds@cds:~/bashcourses$ echo $SHELL

/bin/bash

```

With with the shebang we are telling that the code is interpreted with bash. 
#!                   this indicates that we are goig to name an interpreter
/bin/bash                  is the path of bash
```c
#!/bin/bash
Hello World !
```

now we can run the script with ./

```c
cds@cds:~/bashcourses$ ./sheltest.sh 
Hello World !

```

## VARIABLES: ##

Good and bad examples
it is bad because its writed twice /my/location/from and /my/location/to
you must evitate it
```c
#!/bin/bash

## NOT GOOD >>

cp /my/location/from /my/location/to

cp /my/location/from/here /my/location/to/there

##better

MY_LOCATION_FROM=/my/location/from
MY_LOCATION_TO=/my/location/to

cp $MY_LOCATION_FROM $MY_LOCATION_TO
cp "$MY_LOCATION_FROM_/here" "$MY_LOCATION_TO/there"

```
The variables are just declared 
```c
MY_NAME=JONAS
echo $MY_NAME
JONAS
```

This an an interactive option : 
```c
#!/bin/bash

echo What is your name?
read FIRST_NAME
echo What is your last name?
read LAST_NAME

echo Hello $FIRST_NAME $LAST_NAME
```

## POSITIONAL ARGUMENTS ##

At the first time i  put two empty values
```c
#!/bin/bash
#
echo Hello $1 $2

```

and in the second one I run the script with two words "ricardo ruben"

```c
cds@cds:~/bashcourses$ ./posargu.sh Ricardo Ruben
Hello Ricardo Ruben
```
Finally the scripts returns me HELLO RICARDO RUBEN

## OUTPUT / INPUT DIRECTION ##

### PIPPING ###
I know this ... |

```c
cds@cds:~/bashcourses$ ls -l /usr/bin | grep bash
-rwxr-xr-x 1 root   root     1265648 abr 23 23:23 bash
-rwxr-xr-x 1 root   root        6865 abr 23 23:23 bashbug
-rwxr-xr-x 1 root   root        4414 ago 28  2021 dh_bash-completion
lrwxrwxrwx 1 root   root           4 abr 23 23:23 rbash -> bash

```

### Output direction ###
symbols used
> symbol to write a file
> >to apend a file


```c
$ echo hello world! > hello.txt

$echo hello world >> hello.txt
```

example:
we created a new file called hello.txt with echo and we put hello ! 
and we append after the text "bueena!"
with echo >> hello.txt

If we were used echo > hello.txt we were overwriting everithing
```c
cds@cds:~/bashcourses$ echo hello! >> hello.txt
cds@cds:~/bashcourses$ cat hello.txt 
hello!
cds@cds:~/bashcourses$ echo bueeena !! >> hello.txt 
echo bueeena cat hello.txt  >> hello.txt 
cds@cds:~/bashcourses$ cat hello.txt 
hello!
bueeena cat hello.txt
```

word count

```c
cds@cds:~/bashcourses$ wc -w hello.txt 
4 hello.txt
```
we weant just the number of words only
```c
cds@cds:~/bashcourses$ wc -w < hello.txt 
4
```

APARENTEMENTE TE LEE TODO LO QUE ESTA ANTES DE EOF 

```c
cds@cds:~/bashcourses$ cat << EOF
> I will 
> write some
> text here
> EOF
I will 
write some
text here

```
This counts the number of words that im writing 
```c
wc -c <<< "Hello there wordcount!"
3
```


### TEST OPERATORS ###
## TEST OPERATORS ##
Because of this two commands are equal it returns a zero
```c
cds@cds:~/bashcourses$ [ hello = hello ]
cds@cds:~/bashcourses$ echo $?
0
```
This returns 1 

```c
cds@cds:~/bashcourses$ [ 1 = 0 ]
cds@cds:~/bashcourses$ echo $?
1
```
We can use another operators for this
the "minus EQ" operator for this.

```c
cds@cds:~/bashcourses$ [ 1 -eq 1 ]
cds@cds:~/bashcourses$ echo $?
0

```

## IF/ELIF/ELSE ##
