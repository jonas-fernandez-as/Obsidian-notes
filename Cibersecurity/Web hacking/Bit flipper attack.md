Traducción del inglés-Un ataque de cambio de bits es un ataque a un cifrado criptográfico en el que el atacante puede cambiar el texto cifrado de tal manera que resulte en un cambio predecible del texto sin formato, aunque el atacante no puede aprender el texto sin formato.


A **bit-flipping attack** is an attack on a [cryptographic](https://en.wikipedia.org/wiki/Cryptography "Cryptography") [cipher](https://en.wikipedia.org/wiki/Cipher "Cipher") in which the [attacker](https://en.wikipedia.org/wiki/Adversary_(cryptography) "Adversary (cryptography)") can change the [ciphertext](https://en.wikipedia.org/wiki/Ciphertext "Ciphertext") in such a way as to result in a predictable change of the [plaintext](https://en.wikipedia.org/wiki/Plaintext "Plaintext"), although the attacker is not able to learn the plaintext itself. Note that this type of attack is not—directly—against the cipher itself (as [cryptanalysis](https://en.wikipedia.org/wiki/Cryptanalysis "Cryptanalysis") of it would be), but against a particular message or series of messages. In the extreme, this could become a [Denial of service attack](https://en.wikipedia.org/wiki/Denial_of_service_attack "Denial of service attack") against all messages on a particular channel using that cipher.[[1]](https://en.wikipedia.org/wiki/Bit-flipping_attack#cite_note-1)

The attack is especially dangerous when the attacker knows the format of the message. In such a situation, the attacker can turn it into a similar message but one in which some important information is altered. For example, a change in the destination address might alter the message route in a way that will force re-encryption with a weaker cipher, thus possibly making it easier for an attacker to decipher the message.[[2]](https://en.wikipedia.org/wiki/Bit-flipping_attack#cite_note-2)

When applied to [digital signatures](https://en.wikipedia.org/wiki/Digital_signature "Digital signature"), the attacker might be able to change a [promissory note](https://en.wikipedia.org/wiki/Promissory_note "Promissory note") stating "I owe you $10.00" into one stating "I owe you $10,000".[[3]](https://en.wikipedia.org/wiki/Bit-flipping_attack#cite_note-3)

[Stream ciphers](https://en.wikipedia.org/wiki/Stream_cipher "Stream cipher"), such as [RC4](https://en.wikipedia.org/wiki/RC4_(cipher) "RC4 (cipher)"), are vulnerable to a bit-flipping attack, as are some [block cipher](https://en.wikipedia.org/wiki/Block_cipher "Block cipher") modes of operation. _See_ [stream cipher attack](https://en.wikipedia.org/wiki/Stream_cipher_attack "Stream cipher attack"). A keyed [message authentication code](https://en.wikipedia.org/wiki/Message_authentication_code "Message authentication code"), [digital signature](https://en.wikipedia.org/wiki/Digital_signature "Digital signature"), or other authentication mechanism allows the recipient to detect if any bits were flipped in transit.[[4]](https://en.wikipedia.org/wiki/Bit-flipping_attack#cite_note-4)



This starts like this, first we create an user on the loggin website , some similar to admin for example bmin

Once youre logged on as bmin you intercept the cookie session with burpsuit or zap.

When you visualize the cookie of bmin you look someting like this (this is a simplified cookie, not a real one)

aaaabbbbbbbbbbbbbb


the portion of "aaaaa" its for the character "a" of admin , and the rest "bbbbbbbbbb" for min.





On burpsuit you select the cookie of the proxy and then you send it to the intruder, and in "type of attack" select bit flipper attack

Then you select "literal value" and select all the bits 1 to 8

Disable the url encode option 


now go to options put "grep extract" -add

he copied something "youre currently logged as bdmin" and he want to se when it changes to admin.