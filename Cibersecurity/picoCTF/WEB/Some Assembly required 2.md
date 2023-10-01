
First you need to inspect the code source and you will see something like this
`<script src="Y8splx37qY.js"></script>`

You need to go to the site and see the code
http://mercury.picoctf.net:48841/Y8splx37qY.js

The code was obfuscated first, so I searched a  beautify js platform 

```
const _0x6d8f = ['copy_char', 'value', '207aLjBod', '1301420SaUSqf', '233ZRpipt', '2224QffgXU', 'check_flag', '408533hsoVYx', 'instance', '278338GVFUrH', 'Correct!', '549933ZVjkwI', 'innerHTML', 'charCodeAt', './aD8SvhyVkb', 'result', '977AzKzwq', 'Incorrect!', 'exports', 'length', 'getElementById', '1jIrMBu', 'input', '615361geljRK'];
const _0x5c00 = function(_0x58505a, _0x4d6e6c) {
    _0x58505a = _0x58505a - 0xc3;
    let _0x6d8fc4 = _0x6d8f[_0x58505a];
    return _0x6d8fc4;
};
(function(_0x12fd07, _0x4e9d05) {
    const _0x4f7b75 = _0x5c00;
    while (!![]) {
        try {
            const _0x1bb902 = -parseInt(_0x4f7b75(0xc8)) * -parseInt(_0x4f7b75(0xc9)) + -parseInt(_0x4f7b75(0xcd)) + parseInt(_0x4f7b75(0xcf)) + parseInt(_0x4f7b75(0xc3)) + -parseInt(_0x4f7b75(0xc6)) * parseInt(_0x4f7b75(0xd4)) + parseInt(_0x4f7b75(0xcb)) + -parseInt(_0x4f7b75(0xd9)) * parseInt(_0x4f7b75(0xc7));
            if (_0x1bb902 === _0x4e9d05) break;
            else _0x12fd07['push'](_0x12fd07['shift']());
        } catch (_0x4f8a) {
            _0x12fd07['push'](_0x12fd07['shift']());
        }
    }
}(_0x6d8f, 0x4bb06));
let exports;
(async () => {
    const _0x835967 = _0x5c00;
    let _0x1adb5f = await fetch(_0x835967(0xd2)),
        _0x355961 = await WebAssembly['instantiate'](await _0x1adb5f['arrayBuffer']()),
        _0x5c0ffa = _0x355961[_0x835967(0xcc)];
    exports = _0x5c0ffa[_0x835967(0xd6)];
})();

function onButtonPress() {
    const _0x50ea62 = _0x5c00;
    let _0x5f4170 = document[_0x50ea62(0xd8)](_0x50ea62(0xda))[_0x50ea62(0xc5)];
    for (let _0x19d3ca = 0x0; _0x19d3ca < _0x5f4170['length']; _0x19d3ca++) {
        exports[_0x50ea62(0xc4)](_0x5f4170[_0x50ea62(0xd1)](_0x19d3ca), _0x19d3ca);
    }
    exports['copy_char'](0x0, _0x5f4170[_0x50ea62(0xd7)]), exports[_0x50ea62(0xca)]() == 0x1 ? document['getElementById'](_0x50ea62(0xd3))[_0x50ea62(0xd0)] = _0x50ea62(0xce) : document[_0x50ea62(0xd8)](_0x50ea62(0xd3))['innerHTML'] = _0x50ea62(0xd5);
}
```

I was able to see the code in a beauty way

jsnice.org

```
'use strict';
const _0x6d8f = ["copy_char", "value", "207aLjBod", "1301420SaUSqf", "233ZRpipt", "2224QffgXU", "check_flag", "408533hsoVYx", "instance", "278338GVFUrH", "Correct!", "549933ZVjkwI", "innerHTML", "charCodeAt", "./aD8SvhyVkb", "result", "977AzKzwq", "Incorrect!", "exports", "length", "getElementById", "1jIrMBu", "input", "615361geljRK"];
const _0x5c00 = function(url, whensCollection) {
  /** @type {number} */
  url = url - 195;
  let _0x6d8fc4 = _0x6d8f[url];
  return _0x6d8fc4;
};
(function(data, oldPassword) {
  const toMonths = _0x5c00;
  for (; !![];) {
    try {
      const userPsd = -parseInt(toMonths(200)) * -parseInt(toMonths(201)) + -parseInt(toMonths(205)) + parseInt(toMonths(207)) + parseInt(toMonths(195)) + -parseInt(toMonths(198)) * parseInt(toMonths(212)) + parseInt(toMonths(203)) + -parseInt(toMonths(217)) * parseInt(toMonths(199));
      if (userPsd === oldPassword) {
        break;
      } else {
        data["push"](data["shift"]());
      }
    } catch (_0x4f8a) {
      data["push"](data["shift"]());
    }
  }
})(_0x6d8f, 310022);
let exports;
(async() => {
  const edgeId = _0x5c00;
  let _0x1adb5f = await fetch(edgeId(210));
  let rpm_traffic = await WebAssembly["instantiate"](await _0x1adb5f["arrayBuffer"]());
  let updatedEdgesById = rpm_traffic[edgeId(204)];
  exports = updatedEdgesById[edgeId(214)];
})();
/**
 * @return {undefined}
 */
function onButtonPress() {
  const navigatePop = _0x5c00;
  let params = document[navigatePop(216)](navigatePop(218))[navigatePop(197)];
  for (let i = 0; i < params["length"]; i++) {
    exports[navigatePop(196)](params[navigatePop(209)](i), i);
  }
  exports["copy_char"](0, params[navigatePop(215)]);
  if (exports[navigatePop(202)]() == 1) {
    document["getElementById"](navigatePop(211))[navigatePop(208)] = navigatePop(206);
  } else {
    document[navigatePop(216)](navigatePop(211))["innerHTML"] = navigatePop(213);
  }
}
;
```

You need to go to the http://mercury.picoctf.net:48841./aD8SvhyVkb  and you will download a binary file.
```
asm````pA��
           A�
            A�
             A�

A�
 A��
    A
     A
      �
       memory__wasm_call_ctorsstrcmp
check_flaginput copy_char
                         __dso_handle
__global_base
__memory_base__heap_base
             __table_base
�
 �*#����!A !  k!  6▒  6 (▒!  6 (!  6
                                    @@ (!A j!           6 -!
  
:
  (
   !
    A!
       
        
6       j!
  
  -!  :
 -
 -!A�!  q!@ 
  !A�!  q! -
!A�!  q!  k!▒  ▒6

                  -
                   !A�!▒  ▒q!-
!A�!  q!  !    F!!A!" ! "q!# #
 -
  !$A�!% $ %q!& -
!'A�!( ' (q!) & )k!*  *6
                         (!+ +
                              L
                               A!A����!A����!  ����! ! !  G!A!  sA!             q!
 

g       #����!A!  k!  6
                          (
 (                         !@ E
  !!  s 6

          (
           !     !
 
        :����

             2A�
               +xakgK\Ns>j:<?m8>m;>k110<j?=88lj0l11:n;nmu     
```

I can guess that this is the flag , because another exercise called " Some assembly require 1"
```
+xakgK\Ns>j:<?m8>m;>k110<j?=88lj0l11:n;nmu
```

I was trying some things , tr with bash... is not base64 ... so I tied a bruteforce with XOR on Cyberchef . Modifying the 8 bite o something like this ( I think this is not the exact term) , I was able to see the flag.

https://gchq.github.io/CyberChef/#recipe=XOR_Brute_Force(1,100,0,'Standard',false,true,false,'')&input=K3hha2dLXE5zPmo6PD9tOD5tOz5rMTEwPGo/PTg4bGowbDExOm47bm11


```
picoCTF{***********************}
```