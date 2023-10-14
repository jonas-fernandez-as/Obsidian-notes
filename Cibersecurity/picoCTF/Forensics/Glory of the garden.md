
#### Description

This [garden](https://jupiter.challenges.picoctf.org/static/d0e1ffb10fc0017c6a82c57900f3ffe3/garden.jpg) contains more than it seems.

#### Hints

What is a hex editor?

### Solution

First download the file to your computer.

And the next step is use `strings`.

In my case I'm using linux so I did this

	`strings ./garden.jpg `

Then on the end of the output I was able to  see the flag

`picoCTF{FLAG}`



