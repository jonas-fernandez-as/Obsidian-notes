

#### Description

The [numbers](https://jupiter.challenges.picoctf.org/static/f209a32253affb6f547a585649ba4fda/the_numbers.png)... what do they mean?

### Hints

The flag is in the format PICOCTF{}

### Solution


We have this numbers first , in a picture:

16,9,3,15,3,20,6,20,8,5,14,21,13,2,5,18,19,13,1,19,15,14

The numbers are the index of the letter on the alphabet. For example.. a is 1 b is 2 , etc

I create a script on python and I copy the numbers in a variable

```
solution-numbers.py              
letters=["a","b","c","d","e","f","g","h","i","j","k","l","m","n","o","p","q","r","s","t","u","v","w","x","y","z"]

message=[16,9,3,15,3,20,6,20,8,5,14,21,13,2,5,18,19,13,1,19,15,14]

decode=""

for i in message:
    
    decode+=letters[i-1]


print(decode)

```

This script prints the solution 

`picoCTF{****************************}`