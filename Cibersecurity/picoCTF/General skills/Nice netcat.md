# Description #

There is a nice program that you can talk to by using this command in a shell: `$ nc mercury.picoctf.net 22342`, but it doesn't speak English...

## Hints ##

-You can practice using netcat with this picoGym problem: [what's a netcat?](https://play.picoctf.org/practice/challenge/34)
-You can practice reading and writing ASCII with this picoGym problem: [Let's Warm Up](https://play.picoctf.org/practice/challenge/22)

## Challenge ##


1- Connect via terminal to that ip
`nc mercury.picoctf.net 22342`

2- Copy the output and translate it from ASCII to text
```
112 
105 
99 
111 
67 
84 
70 
123 
103 
48 
48 
100 
95 
107 
49 
116 
116 
121 
33 
95 
110 
49 
99 
51 
95 
107 
49 
116 
116 
121 
33 
95 
53 
102 
98 
53 
101 
53 
49 
100 
125 
10 
```

I did it with this bash script
```
awk '{ for(i=1;i<=NF;i++) printf("%c",$i); print "";  }' ascii.txt 
```
output
```
picoCTF{**************************}
```