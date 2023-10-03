#### Description

I wonder what this really is... [enc](https://mercury.picoctf.net/static/e47483f88b12f2ab0c46315afc12f64d/enc) `''.join([chr((ord(flag[i]) << 8) + ord(flag[i + 1])) for i in range(0, len(flag), 2)])`

#### Hints

You may find some decoders online

### Process ###

I tried with cyberchef and it doesn't work for me. The way that i was able to do it was this. With python3:

```
>>> decode = '灩捯䍔䙻ㄶ形楴獟楮獴㌴摟潦弸彥ㄴㅡて㝽'
>>> print(decode.encode('utf-16-be'))
b'picoCTF{******************************}'
```
