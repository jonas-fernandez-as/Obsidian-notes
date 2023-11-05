
#### Description

We found a brand new type of encryption, can you break the secret code? (Wrap with picoCTF{})
lkmjkemjmkiekeijiiigljlhilihliikiliginliljimiklligljiflhiniiiniiihlhilimlhijil
#### Hints ####
How does the cipher work if the alphabet isn't 26 letters?

Even though the letters are split up, the same paradigms still apply

### Solution ###

We start with this code that we can examine:

```
import string

LOWERCASE_OFFSET = ord("a")
ALPHABET = string.ascii_lowercase[:16]

def b16_encode(plain):
        enc = ""
        for c in plain:
                binary = "{0:08b}".format(ord(c))
                enc += ALPHABET[int(binary[:4], 2)]
                enc += ALPHABET[int(binary[4:], 2)]
        return enc

def shift(c, k):
        t1 = ord(c) - LOWERCASE_OFFSET
        t2 = ord(k) - LOWERCASE_OFFSET
        return ALPHABET[(t1 + t2) % len(ALPHABET)]

flag = "redacted"
key = "redacted"
assert all([k in ALPHABET for k in key])
assert len(key) == 1

b16 = b16_encode(flag)
enc = ""
for i, c in enumerate(b16):
        enc += shift(c, key[i % len(key)])

```

I did another one and so I can understand what this code was doing, and how encodes the message that we put on it. (note, this is in spanish, you can translate it on google and you will  be able to understand my notes)

```
import string
LOWERCASE_OFFSET = ord("a")
ALPHABET = string.ascii_lowercase[:16]
print(LOWERCASE_OFFSET)
c="c"
k="k"


message="pico"



#def b16_encode(plain):
#        enc = ""
#        for c in plain:
#                binary = "{0:08b}".format(ord(c))  #parece que lo transforma en binario
#                print(binary)
#                enc+= ALPHABET[int(binary[:4],2)]  #toma los primeros 4 caracteres en binario (base 2)
#                 print(enc)
#        print(enc)
#        return enc


#def shift(c, k):
#        t1 = ord(c) - LOWERCASE_OFFSET # transfroma "c" en ascii y lo resta por el valor de ascii "a"
#        print("ESTO ES c Y VALE 2 (99-97):")
#        print(t1) #2
#        t2 = ord(k) - LOWERCASE_OFFSET # transforma el valor de lo que vale "k" y el resta el valor ascii de "a"
#        print("ESTO ES k Y VALE 10 (107-97):")
#        print(t2) #10
#        result=ALPHABET[(t1 + t2) % len(ALPHABET)]
#        print("Lenght alphabet:")
#        print(len(ALPHABET))
#        print("c+k % len(ALPHABET) es:")
#        print(ALPHABET[(t1 + t2) % len(ALPHABET)]) #suma "c" y "k" y devuelve el resto de la division por 16
#        return ALPHABET[(t1 + t2) % len(ALPHABET)]
        
#shift(c,k)





flag = "redacted"
key = "redacted"
assert all([k in ALPHABET for k in key])
assert len(key) == 1


```

Basically we have to make the reverse to decode the code, but we need a "key" assert is validating the key and the length must be 1 , and must be on the alphabet.

```
assert all([k in ALPHABET for k in key])
assert len(key) == 1
```

So... I did another code that reverts this situation and decode it, doing the reverse process

```
import string

LOWERCASE_OFFSET = ord("a")
ALPHABET = string.ascii_lowercase[:16]

def shift_inverse(c, k):
    t1 = ALPHABET.index(c)
    t2 = ord(k) - LOWERCASE_OFFSET
    return ALPHABET[(t1 - t2) % len(ALPHABET)]

def b16_decode(enc):
    plain = ""
    for i in range(0, len(enc), 2):
        binary = "{0:04b}{1:04b}".format(ALPHABET.index(enc[i]), ALPHABET.index(enc[i+1]))
        plain += chr(int(binary, 2))
    return plain

enc = "lkmjkemjmkiekeijiiigljlhilihliikiliginliljimiklligljiflhiniiiniiihlhilimlhijil" 

for key in ALPHABET:
    b16 = ""
    for i in range(0, len(enc), 2):
        b16 += shift_inverse(enc[i], key)
        b16 += shift_inverse(enc[i+1], key)
    flag = b16_decode(b16)
    print("Key:", key, "Flag:", flag)

```

This is the output:

```
Key: a Flag: ºÉ¤ÉÊ¤¹·¸¸¹»¹···
Key: b Flag: ©¸¸¹sxwu¨¦zv§yzu|§¨{yªu¨t¦|w|wv¦z{¦xz
Key: c Flag: §§¨bgfdiehidkjhdckfkfeijgi
Key: d Flag: qQqVUSXTWXSZYWSRZUZUTXYVX
Key: e Flag: v`@`EDBusGCtFGBItuHFwBuAsIDIDCsGHsEG
Key: f Flag: et_tu?_431db62c5618cd75f1d0b83832b67b46
Key: g Flag: TcNcd.N#" SQ%!R$% 'RS&$U S/Q'"'"!Q%&Q#%
Key: h Flag: CR=RS=B@AABDB@@@
Key: i Flag: 2A,AB
???               ,1?00131
Key: j Flag: !01ûÿý .òþ/ñòýô/ óñ"ý ü.ôÿôÿþ.òó.ðò
Key: k Flag: /
/ ê
ïîìáíàáìãâàìëãîãîíáâïá
Key: l Flag: ùÙùÞÝÛ
ÑßÛÚ               ÐÜ
    ÒÝÒÝÜ
         ÐÑ
           ÞÐ
ÈèÍÌÊýûÏËüÎÏÊÁüýÀÎÿÊýÉûÁÌÁÌËûÏÀûÍÏ
Key: n Flag: íü×üý·×¼»¹ìê¾ºë½¾¹°ëì¿½î¹ì¸ê°»°»ºê¾¿ê¼¾
Key: o Flag: ÜëÆëì¦Æ«ª¨ÛÙ­©Ú¬­¨¯ÚÛ®¬Ý¨Û§Ù¯ª¯ª©Ù­®Ù«­
Key: p Flag: ËÚµÚÛµÊÈÉÉÊÊÈÈÈ

```

I tried this one :

`Key: f Flag: et_tu?_431db62c5618cd75f1d0b83832b67b46` 

And I did it :)