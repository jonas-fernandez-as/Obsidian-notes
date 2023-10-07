
# Description #

In RSA, a small `e` value can be problematic, but what about `N`? Can you decrypt this? [values](https://mercury.picoctf.net/static/12d820e355a7775a2c9129b2622a7eb6/values)

# Hints#

Bits are expensive, I used only a little bit over 100 to save money



# Process #


This is the file that we have at the start:
```
Decrypt my super sick RSA: 

c: 843044897663847841476319711639772861390329326681532977209935413827620909782846667 n: 1422450808944701344261903748621562998784243662042303391362692043823716783771691667 e: 65537
```

In RSA, n is the modulus for both the public key and the private key e is the public key of exponent c is the ciphertext obtained by encrypting the original message.

To decrypt a message in RSA, you need the private key which is not provided here. The private key in RSA is a number d such that (e * d) % phi(n) = 1, where phi(n) is the Euler2 totient function. However, computing d without knowing the prime factors of n is a difficult problem to solve, and that is the basis of security in RSA2.

That said, in the context of a CTF challenge like picoCTF, there are sometimes ways to break RSA if it is configured incorrectly or if certain conditions are met. For example, if n is small enough, you could factor it out and calculate d, or if the same n is used to encrypt many messages and e is small, you could use a low message broadcast attack.


```
```python
from sympy import *

# Valores dados
e = 65537
N = 1422450808944701344261903748621562998784243662042303391362692043823716783771691667
c = 843044897663847841476319711639772861390329326681532977209935413827620909782846667

# Factorización de N
p, q = factorint(N)

# Cálculo de d
d = mod_inverse(e, (p-1)*(q-1))

# Descifrado del mensaje
m = pow(c, d, N)

print(m)
```




Chat con gpt4:

En RSA, N es el producto de dos números primos, p y q. Si N solo tiene un poco más de 100 bits, entonces p y q deben ser números primos de aproximadamente 50 bits cada uno. Esto es mucho más pequeño que los tamaños típicos utilizados en RSA, que suelen ser de al menos 512 bits cada uno.

Dado que p y q son mucho más pequeños de lo habitual, podrías intentar factorizar N utilizando un algoritmo de factorización para números pequeños. Si eso no funciona, podrías intentar buscar en una base de datos de números primos para ver si puedes encontrar p y q.

Si aún así no puedes factorizar N, entonces es posible que la implementación de RSA en este desafío tenga alguna otra debilidad que puedas explotar. Por ejemplo, si e es pequeño y se usa el mismo N para cifrar muchos mensajes, podrías intentar un ataque de transmisión baja.

Yo
sabes, disculpa mi ignorancia pero, no entiendo mucho que es p, q a que te refieres con eso ? hay una parte que dice (e,(p-1)*(q-1) ) podrias explicarmelo como si fuera un niño pequeño ?



GPT4

¡Por supuesto! No hay problema, te lo explicaré de la manera más sencilla posible.

Imagina que tienes una caja fuerte con un candado especial. Para abrir este candado, necesitas una llave especial. En el caso de RSA, esta llave especial se llama “clave privada”, y está compuesta por dos partes: N y d.

Ahora, ¿cómo se crea esta clave privada? Bueno, aquí es donde entran en juego los números p y q. Piensa en p y q como dos números secretos que solo tú conoces. Multiplicas p y q para obtener N. Luego, eliges otro número, e, de tal manera que e y (p-1)_(q-1) sean primos entre sí (es decir, no tienen otros divisores comunes aparte de 1). Finalmente, calculas d como el inverso multiplicativo de e modulo (p-1)_(q-1). Esto significa que e * d es igual a 1 cuando se divide por (p-1)*(q-1).

Entonces, para resumir:

- p y q son dos números primos secretos que eliges.
- N es el producto de p y q.
- e es un número que eliges de tal manera que e y (p-1)*(q-1) sean primos entre sí.
- d es el inverso multiplicativo de e modulo (p-1)*(q-1).

Espero que esto te ayude a entender mejor cómo funciona RSA. Si tienes más preguntas, no dudes en preguntar.




N= multiplicacion de dos numeros primos = llave publica

p , q = dos numeros primos secretos 

d= llave privada

e= otra parte de la clave publica (??)




aux :


HOla, yo te mande este mensaje !!:

me podrias ayudar en un CTF de picoCTF ? Mira el problema es asi, aqui esta la descripcion : In RSA, a small e value can be problematic, but what about N? Can you decrypt this? ... los valores son estos : Decrypt my super sick RSA: c: 843044897663847841476319711639772861390329326681532977209935413827620909782846667 n: 1422450808944701344261903748621562998784243662042303391362692043823716783771691667 e: 65537 y la unica pista que tengo es esta: Bits are expensive, I used only a little bit over 100 to save money . Tu me habias dicho ago asi "That said, in the context of a CTF challenge like picoCTF, there are sometimes ways to break RSA if it is configured incorrectly or if certain conditions are met. For example, if n is small enough, you could factor it out and calculate d, or if the same n is used to encrypt many messages and e is small, you could use a low message broadcast attack." Pero anteriormente no te habia proporcionado tantos datos

debatimos un poco y me dijiste esto:

last msg 

Sí, tienes razón. En el contexto de RSA, **d** es una parte de la **clave privada**. La clave privada en RSA está compuesta por dos partes: **N** y **d**.

- **N** es el producto de dos números primos, p y q. Es conocido públicamente y es parte de la clave pública y privada.
- **d** es un número especial que se calcula a partir de e, p y q. Este número es mantenido en secreto y es la otra parte de la clave privada.

Por lo tanto, puedes pensar en **d** como la llave que abre la caja fuerte **N**. Solo alguien con la clave privada (es decir, alguien que conoce **d**) puede abrir la caja fuerte y leer el mensaje cifrado.

necesito saber que es "e"  entiendo que p y q son dos numeros primos, pero que es "e"




Ok with this one my mind is blowing !!!!

Another answer

El título “Mind your Ps and Qs” es una pista que se refiere a los valores **p** y **q** en RSA. En RSA, **p** y **q** son números primos que se utilizan para generar las claves pública y privada.

El título sugiere que debes prestar atención a cómo se utilizan estos valores en el problema. En el contexto de este desafío, podría implicar que necesitas encontrar una manera de determinar los valores de **p** y **q** a partir del valor de **N** dado, ya que **N** es el producto de **p** y **q**. Una vez que tienes **p** y **q**, puedes calcular **d**, la clave privada, que se utiliza para descifrar el mensaje.

Por lo tanto, “Mind your Ps and Qs” es un juego de palabras que te indica que debes prestar atención a cómo se utilizan los valores de **p** y **q** en este problema de RSA.




# SOLUTION #


Speaking with an IA i get some confusion but I can get superficially how does this work. Fortunately there is a website, beautiful site, where you just put the values that you have and then the site decrypt the key.

https://www.dcode.fr/rsa-cipher

Add the values to the site and you will get your Key

```
picoCTF{******************************}
```