
We have this on the source code
```
|<html>|
||<head>|
||<meta charset="UTF-8">|
||<script src="[G82XCw5CX3.js](http://mercury.picoctf.net:15472/G82XCw5CX3.js)"></script>|
||</head>|
||<body>|
||<h4>Enter flag:</h4>|
||<input type="text" id="input"/>|
||<button onclick="onButtonPress()">Submit</button>|
||<p id="result"></p>|
||</body>|
||</html>|
```

As you can see, here we see a  G82XCw5CX3.js, so it you look at this you can see some code , looks like hexadecimal

http://mercury.picoctf.net:15472/G82XCw5CX3.js

```
const _0x402c=['value','2wfTpTR','instantiate','275341bEPcme','innerHTML','1195047NznhZg','1qfevql','input','1699808QuoWhA','Correct!','check_flag','Incorrect!','./JIFxzHyW8W','23SMpAuA','802698XOMSrr','charCodeAt','474547vVoGDO','getElementById','instance','copy_char','43591XxcWUl','504454llVtzW','arrayBuffer','2NIQmVj','result'];const _0x4e0e=function(_0x553839,_0x53c021){_0x553839=_0x553839-0x1d6;let _0x402c6f=_0x402c[_0x553839];return _0x402c6f;};(function(_0x76dd13,_0x3dfcae){const _0x371ac6=_0x4e0e;while(!![]){try{const _0x478583=-parseInt(_0x371ac6(0x1eb))+parseInt(_0x371ac6(0x1ed))+-parseInt(_0x371ac6(0x1db))*-parseInt(_0x371ac6(0x1d9))+-parseInt(_0x371ac6(0x1e2))*-parseInt(_0x371ac6(0x1e3))+-parseInt(_0x371ac6(0x1de))*parseInt(_0x371ac6(0x1e0))+parseInt(_0x371ac6(0x1d8))*parseInt(_0x371ac6(0x1ea))+-parseInt(_0x371ac6(0x1e5));if(_0x478583===_0x3dfcae)break;else _0x76dd13['push'](_0x76dd13['shift']());}catch(_0x41d31a){_0x76dd13['push'](_0x76dd13['shift']());}}}(_0x402c,0x994c3));let exports;(async()=>{const _0x48c3be=_0x4e0e;let _0x5f0229=await fetch(_0x48c3be(0x1e9)),_0x1d99e9=await WebAssembly[_0x48c3be(0x1df)](await _0x5f0229[_0x48c3be(0x1da)]()),_0x1f8628=_0x1d99e9[_0x48c3be(0x1d6)];exports=_0x1f8628['exports'];})();function onButtonPress(){const _0xa80748=_0x4e0e;let _0x3761f8=document['getElementById'](_0xa80748(0x1e4))[_0xa80748(0x1dd)];for(let _0x16c626=0x0;_0x16c626<_0x3761f8['length'];_0x16c626++){exports[_0xa80748(0x1d7)](_0x3761f8[_0xa80748(0x1ec)](_0x16c626),_0x16c626);}exports['copy_char'](0x0,_0x3761f8['length']),exports[_0xa80748(0x1e7)]()==0x1?document[_0xa80748(0x1ee)](_0xa80748(0x1dc))[_0xa80748(0x1e1)]=_0xa80748(0x1e6):document[_0xa80748(0x1ee)](_0xa80748(0x1dc))[_0xa80748(0x1e1)]=_0xa80748(0x1e8);}
```

Seems to be some folder here /JIFxzHyW8W


When I go to that folder, I downloaded a file called JIFxzHyW8W and when I see inside of it the ony thing that I was able to read was :
```
picoCTF{c733fda95299a16681f37b3ff09f901c} 
```


Just submit the flag and you will complete the challenge





I was trying to understand the code, but I get into a rabbit hole jhahahaha...


This was the type of file
```
┌──(cds㉿kali)-[~/Downloads]
└─$ file JIFxzHyW8W                                                       
JIFxzHyW8W: WebAssembly (wasm) binary module version 0x1 (MVP)
```

With a beautify I was able to see like this the code 

```
const _0x402c = ['value', '2wfTpTR', 'instantiate', '275341bEPcme', 'innerHTML', '1195047NznhZg', '1qfevql', 'input', '1699808QuoWhA', 'Correct!', 'check_flag', 'Incorrect!', './JIFxzHyW8W', '23SMpAuA', '802698XOMSrr', 'charCodeAt', '474547vVoGDO', 'getElementById', 'instance', 'copy_char', '43591XxcWUl', '504454llVtzW', 'arrayBuffer', '2NIQmVj', 'result'];
const _0x4e0e = function(_0x553839, _0x53c021) {
    _0x553839 = _0x553839 - 0x1d6;
    let _0x402c6f = _0x402c[_0x553839];
    return _0x402c6f;
};
(function(_0x76dd13, _0x3dfcae) {
    const _0x371ac6 = _0x4e0e;
    while (!![]) {
        try {
            const _0x478583 = -parseInt(_0x371ac6(0x1eb)) + parseInt(_0x371ac6(0x1ed)) + -parseInt(_0x371ac6(0x1db)) * -parseInt(_0x371ac6(0x1d9)) + -parseInt(_0x371ac6(0x1e2)) * -parseInt(_0x371ac6(0x1e3)) + -parseInt(_0x371ac6(0x1de)) * parseInt(_0x371ac6(0x1e0)) + parseInt(_0x371ac6(0x1d8)) * parseInt(_0x371ac6(0x1ea)) + -parseInt(_0x371ac6(0x1e5));
            if (_0x478583 === _0x3dfcae) break;
            else _0x76dd13['push'](_0x76dd13['shift']());
        } catch (_0x41d31a) {
            _0x76dd13['push'](_0x76dd13['shift']());
        }
    }
}(_0x402c, 0x994c3));
let exports;
(async () => {
    const _0x48c3be = _0x4e0e;
    let _0x5f0229 = await fetch(_0x48c3be(0x1e9)),
        _0x1d99e9 = await WebAssembly[_0x48c3be(0x1df)](await _0x5f0229[_0x48c3be(0x1da)]()),
        _0x1f8628 = _0x1d99e9[_0x48c3be(0x1d6)];
    exports = _0x1f8628['exports'];
})();

function onButtonPress() {
    const _0xa80748 = _0x4e0e;
    let _0x3761f8 = document['getElementById'](_0xa80748(0x1e4))[_0xa80748(0x1dd)];
    for (let _0x16c626 = 0x0; _0x16c626 < _0x3761f8['length']; _0x16c626++) {
        exports[_0xa80748(0x1d7)](_0x3761f8[_0xa80748(0x1ec)](_0x16c626), _0x16c626);
    }
    exports['copy_char'](0x0, _0x3761f8['length']), exports[_0xa80748(0x1e7)]() == 0x1 ? document[_0xa80748(0x1ee)](_0xa80748(0x1dc))[_0xa80748(0x1e1)] = _0xa80748(0x1e6) : document[_0xa80748(0x1ee)](_0xa80748(0x1dc))[_0xa80748(0x1e1)] = _0xa80748(0x1e8);
}
```

Speaking with a IA , helped me to understand the text, he says that is

```

const flag = "2wfTpTR";

async function checkFlag(input) {
  // Carga el módulo WebAssembly.
  const wasmModule = await fetch("./JIFxzHyW8W").then(response => response.arrayBuffer());

  // Instancia el módulo WebAssembly.
  const instance = await WebAssembly.instantiate(wasmModule);

  // Obtiene los exports del módulo WebAssembly.
  const exports = instance.exports;

  // Itera sobre cada carácter de la entrada.
  for (let i = 0; i < input.length; i++) {
    // Copia el carácter de la entrada al módulo WebAssembly.
    exports.copy_char(input.charCodeAt(i), i);
  }

  // Copia el número de caracteres de la entrada al módulo WebAssembly.
  exports.copy_char(0, input.length);

  // Devuelve el resultado de la comprobación de la bandera.
  return exports.check_flag() === 1;
}

function onButtonPress() {
  // Obtiene la entrada del usuario.
  const input = document.getElementById("input").value;

  // Llama a la función de comprobación de la bandera.
  const isCorrectFlag = await checkFlag(input);

  // Actualiza el elemento DOM con el resultado.
  if (isCorrectFlag) {
    document.getElementById("result").innerHTML = "Correcto!";
  } else {
    document.getElementById("result").innerHTML = "Incorrecto!";
  }
}
```

Utiliza el código con precaución. [Más información](https://bard.google.com/faq#coding)

content_copy

Esta traducción es más fácil de entender porque:

- Las variables no descriptivas han sido reemplazadas por nombres más descriptivos.
- El código ha sido reestructurado para que sea más fácil de seguir.

La función `checkFlag()` funciona de la siguiente manera:

1. Carga el módulo WebAssembly desde un archivo local.
2. Instancia el módulo WebAssembly.
3. Obtiene los exports del módulo WebAssembly.
4. Itera sobre cada carácter de la entrada.
5. Copia el carácter de la entrada al módulo WebAssembly.
6. Copia el número de caracteres de la entrada al módulo WebAssembly.
7. Devuelve el resultado de la comprobación de la bandera.

La función `onButtonPress()` funciona de la siguiente manera:

1. Obtiene la entrada del usuario.
2. Llama a la función `checkFlag()` para comprobar la entrada.
3. Actualiza el elemento DOM con el resultado.

Espero que esto ayude.



YO : 
 tengo una dudda, por que cuando yo me fijo en tras plataformas paso de hexadecimal : _0x402c a ascii :5f 30 78 34 30 32 63 y de ascii a texto plano me da esto :N" ? , Yo no puedo leer la palabra flag ahi

IA

  
La razón por la que no puedes leer la palabra `flag` en la cadena `5f 30 78 34 30 32 63` es porque la cadena está codificada en UTF-8. UTF-8 es un estándar de codificación de caracteres que utiliza un byte para representar caracteres ASCII, y dos o más bytes para representar caracteres Unicode.

En este caso, la cadena `5f 30 78 34 30 32 63` representa la cadena `flag` en Unicode. El primer byte, `5f`, representa el carácter `_` en Unicode. El segundo byte, `30`, representa el carácter `f` en Unicode. Y así sucesivamente.

Para convertir la cadena `5f 30 78 34 30 32 63` de UTF-8 a ASCII, puedes utilizar la función `decode()` de la biblioteca estándar de Python. Esta función devuelve una cadena de caracteres ASCII que representa la cadena UTF-8.


Python

```
import codecs

def decode_utf8(string):
    return codecs.decode(string, "utf-8")

print(decode_utf8("5f 30 78 34 30 32 63"))
```

Utiliza el código con precaución. [Más información](https://bard.google.com/faq#coding)

content_copy

Este código devuelve la siguiente salida:

```
flag
```

Por lo tanto, la cadena `5f 30 78 34 30 32 63` representa la cadena `flag` en ASCII.

Aquí hay una explicación más detallada de cómo funciona la codificación UTF-8:

- Los caracteres ASCII se representan utilizando un byte.
- Los caracteres Unicode que se encuentran en el rango U+0000-U+007F también se representan utilizando un byte.
- Los caracteres Unicode que se encuentran en el rango U+0080-U+07FF se representan utilizando dos bytes.
- Los caracteres Unicode que se encuentran en el rango U+0800-U+FFFF se representan utilizando tres bytes.
- Los caracteres Unicode que se encuentran en el rango U+10000-U+10FFFF se representan utilizando cuatro bytes.

En este caso, la cadena `flag` se encuentra en el rango U+0000-U+007F, por lo que se representa utilizando un byte por carácter.