If you inspect the code source you will se a file with  importing code of js
"[rTEuOmSfG3.js](http://mercury.picoctf.net:60022/rTEuOmSfG3.js)"

```
./wasm2wat --generate-names script.wasm > script.wat
(This demo converts from the binary format to the text format.)



(module
  (type $t0 (func))
  (type $t1 (func (param i32 i32) (result i32)))
  (type $t2 (func (result i32)))
  (type $t3 (func (param i32 i32)))
  (func $__wasm_call_ctors (export "__wasm_call_ctors") (type $t0))
  (func $strcmp (export "strcmp") (type $t1) (param $p0 i32) (param $p1 i32) (result i32)
    (local $l2 i32) (local $l3 i32) (local $l4 i32) (local $l5 i32) (local $l6 i32) (local $l7 i32) (local $l8 i32) (local $l9 i32) (local $l10 i32) (local $l11 i32) (local $l12 i32) (local $l13 i32) (local $l14 i32) (local $l15 i32) (local $l16 i32) (local $l17 i32) (local $l18 i32) (local $l19 i32) (local $l20 i32) (local $l21 i32) (local $l22 i32) (local $l23 i32) (local $l24 i32) (local $l25 i32) (local $l26 i32) (local $l27 i32) (local $l28 i32) (local $l29 i32) (local $l30 i32) (local $l31 i32) (local $l32 i32) (local $l33 i32) (local $l34 i32) (local $l35 i32) (local $l36 i32) (local $l37 i32) (local $l38 i32) (local $l39 i32) (local $l40 i32) (local $l41 i32) (local $l42 i32) (local $l43 i32)
    (local.set $l2
      (global.get $g0))
    (local.set $l3
      (i32.const 32))
    (local.set $l4
      (i32.sub
        (local.get $l2)
        (local.get $l3)))
    (i32.store offset=24
      (local.get $l4)
      (local.get $p0))
    (i32.store offset=20
      (local.get $l4)
      (local.get $p1))
    (local.set $l5
      (i32.load offset=24
        (local.get $l4)))
    (i32.store offset=16
      (local.get $l4)
      (local.get $l5))
    (local.set $l6
      (i32.load offset=20
        (local.get $l4)))
    (i32.store offset=12
      (local.get $l4)
      (local.get $l6))
    (block $B0
      (loop $L1
        (local.set $l7
          (i32.load offset=16
            (local.get $l4)))
        (local.set $l8
          (i32.const 1))
        (local.set $l9
          (i32.add
            (local.get $l7)
            (local.get $l8)))
        (i32.store offset=16
          (local.get $l4)
          (local.get $l9))
        (local.set $l10
          (i32.load8_u
            (local.get $l7)))
        (i32.store8 offset=11
          (local.get $l4)
          (local.get $l10))
        (local.set $l11
          (i32.load offset=12
            (local.get $l4)))
        (local.set $l12
          (i32.const 1))
        (local.set $l13
          (i32.add
            (local.get $l11)
            (local.get $l12)))
        (i32.store offset=12
          (local.get $l4)
          (local.get $l13))
        (local.set $l14
          (i32.load8_u
            (local.get $l11)))
        (i32.store8 offset=10
          (local.get $l4)
          (local.get $l14))
        (local.set $l15
          (i32.load8_u offset=11
            (local.get $l4)))
        (local.set $l16
          (i32.const 255))
        (local.set $l17
          (i32.and
            (local.get $l15)
            (local.get $l16)))
        (block $B2
          (br_if $B2
            (local.get $l17))
          (local.set $l18
            (i32.load8_u offset=11
              (local.get $l4)))
          (local.set $l19
            (i32.const 255))
          (local.set $l20
            (i32.and
              (local.get $l18)
              (local.get $l19)))
          (local.set $l21
            (i32.load8_u offset=10
              (local.get $l4)))
          (local.set $l22
            (i32.const 255))
          (local.set $l23
            (i32.and
              (local.get $l21)
              (local.get $l22)))
          (local.set $l24
            (i32.sub
              (local.get $l20)
              (local.get $l23)))
          (i32.store offset=28
            (local.get $l4)
            (local.get $l24))
          (br $B0))
        (local.set $l25
          (i32.load8_u offset=11
            (local.get $l4)))
        (local.set $l26
          (i32.const 255))
        (local.set $l27
          (i32.and
            (local.get $l25)
            (local.get $l26)))
        (local.set $l28
          (i32.load8_u offset=10
            (local.get $l4)))
        (local.set $l29
          (i32.const 255))
        (local.set $l30
          (i32.and
            (local.get $l28)
            (local.get $l29)))
        (local.set $l31
          (local.get $l27))
        (local.set $l32
          (local.get $l30))
        (local.set $l33
          (i32.eq
            (local.get $l31)
            (local.get $l32)))
        (local.set $l34
          (i32.const 1))
        (local.set $l35
          (i32.and
            (local.get $l33)
            (local.get $l34)))
        (br_if $L1
          (local.get $l35)))
      (local.set $l36
        (i32.load8_u offset=11
          (local.get $l4)))
      (local.set $l37
        (i32.const 255))
      (local.set $l38
        (i32.and
          (local.get $l36)
          (local.get $l37)))
      (local.set $l39
        (i32.load8_u offset=10
          (local.get $l4)))
      (local.set $l40
        (i32.const 255))
      (local.set $l41
        (i32.and
          (local.get $l39)
          (local.get $l40)))
      (local.set $l42
        (i32.sub
          (local.get $l38)
          (local.get $l41)))
      (i32.store offset=28
        (local.get $l4)
        (local.get $l42)))
    (local.set $l43
      (i32.load offset=28
        (local.get $l4)))
    (return
      (local.get $l43)))
  (func $check_flag (export "check_flag") (type $t2) (result i32)
    (local $l0 i32) (local $l1 i32) (local $l2 i32) (local $l3 i32) (local $l4 i32) (local $l5 i32) (local $l6 i32) (local $l7 i32) (local $l8 i32) (local $l9 i32) (local $l10 i32)
    (local.set $l0
      (i32.const 0))
    (local.set $l1
      (i32.const 1072))
    (local.set $l2
      (i32.const 1024))
    (local.set $l3
      (call $strcmp
        (local.get $l2)
        (local.get $l1)))
    (local.set $l4
      (local.get $l3))
    (local.set $l5
      (local.get $l0))
    (local.set $l6
      (i32.ne
        (local.get $l4)
        (local.get $l5)))
    (local.set $l7
      (i32.const -1))
    (local.set $l8
      (i32.xor
        (local.get $l6)
        (local.get $l7)))
    (local.set $l9
      (i32.const 1))
    (local.set $l10
      (i32.and
        (local.get $l8)
        (local.get $l9)))
    (return
      (local.get $l10)))
  (func $copy_char (export "copy_char") (type $t3) (param $p0 i32) (param $p1 i32)
    (local $l2 i32) (local $l3 i32) (local $l4 i32) (local $l5 i32) (local $l6 i32) (local $l7 i32) (local $l8 i32) (local $l9 i32) (local $l10 i32) (local $l11 i32) (local $l12 i32) (local $l13 i32) (local $l14 i32) (local $l15 i32) (local $l16 i32) (local $l17 i32) (local $l18 i32)
    (local.set $l2
      (global.get $g0))
    (local.set $l3
      (i32.const 16))
    (local.set $l4
      (i32.sub
        (local.get $l2)
        (local.get $l3)))
    (i32.store offset=12
      (local.get $l4)
      (local.get $p0))
    (i32.store offset=8
      (local.get $l4)
      (local.get $p1))
    (local.set $l5
      (i32.load offset=12
        (local.get $l4)))
    (block $B0
      (br_if $B0
        (i32.eqz
          (local.get $l5)))
      (local.set $l6
        (i32.const 4))
      (local.set $l7
        (i32.load offset=8
          (local.get $l4)))
      (local.set $l8
        (i32.const 5))
      (local.set $l9
        (i32.rem_s
          (local.get $l7)
          (local.get $l8)))
      (local.set $l10
        (i32.sub
          (local.get $l6)
          (local.get $l9)))
      (local.set $l11
        (i32.load8_u offset=1067
          (local.get $l10)))
      (local.set $l12
        (i32.const 24))
      (local.set $l13
        (i32.shl
          (local.get $l11)
          (local.get $l12)))
      (local.set $l14
        (i32.shr_s
          (local.get $l13)
          (local.get $l12)))
      (local.set $l15
        (i32.load offset=12
          (local.get $l4)))
      (local.set $l16
        (i32.xor
          (local.get $l15)
          (local.get $l14)))
      (i32.store offset=12
        (local.get $l4)
        (local.get $l16)))
    (local.set $l17
      (i32.load offset=12
        (local.get $l4)))
    (local.set $l18
      (i32.load offset=8
        (local.get $l4)))
    (i32.store8 offset=1072
      (local.get $l18)
      (local.get $l17))
    (return))
  (table $T0 1 1 funcref)
  (memory $memory (export "memory") 2)
  (global $g0 (mut i32) (i32.const 66864))
  (global $input (export "input") i32 (i32.const 1072))
  (global $key (export "key") i32 (i32.const 1067))
  (global $__dso_handle (export "__dso_handle") i32 (i32.const 1024))
  (global $__data_end (export "__data_end") i32 (i32.const 1328))
  (global $__global_base (export "__global_base") i32 (i32.const 1024))
  (global $__heap_base (export "__heap_base") i32 (i32.const 66864))
  (global $__memory_base (export "__memory_base") i32 (i32.const 0))
  (global $__table_base (export "__table_base") i32 (i32.const 1))
  (data $d0 (i32.const 1024) "\9dn\93\c8\b2\b9A\8b\c5\c6\dda\93\c3\c2\da?\c7\93\c1\8b1\95\93\93\8eb\c8\94\c9\d5d\c0\96\c4\d97\93\93\c2\90\00\00")
  (data $d1 (i32.const 1067) "\f1\a7\f0\07\ed"))

```





With jsnice looks like this
```
'use strict';
const _0x143f = ["exports", "270328ewawLo", "instantiate", "1OsuamQ", "Incorrect!", "length", "copy_char", "value", "1512517ESezaM", "innerHTML", "check_flag", "result", "1383842SQRPPf", "924408cukzgO", "getElementById", "418508cLDohp", "input", "Correct!", "573XsMMHp", "arrayBuffer", "183RUQBDE", "38934oMACea"];
const _0x187e = function(url, whensCollection) {
  /** @type {number} */
  url = url - 285;
  let _0x143f7d = _0x143f[url];
  return _0x143f7d;
};
(function(data, oldPassword) {
  const toMonths = _0x187e;
  for (; !![];) {
    try {
      const userPsd = -parseInt(toMonths(290)) + -parseInt(toMonths(303)) + -parseInt(toMonths(294)) * -parseInt(toMonths(299)) + -parseInt(toMonths(306)) + parseInt(toMonths(292)) + -parseInt(toMonths(289)) * -parseInt(toMonths(287)) + parseInt(toMonths(304));
      if (userPsd === oldPassword) {
        break;
      } else {
        data["push"](data["shift"]());
      }
    } catch (_0x289152) {
      data["push"](data["shift"]());
    }
  }
})(_0x143f, 970828);
let exports;
(async() => {
  const edgeId = _0x187e;
  let rpm_traffic = await fetch("./qCCYI0ajpD");
  let m = await WebAssembly[edgeId(293)](await rpm_traffic[edgeId(288)]());
  let updatedEdgesById = m["instance"];
  exports = updatedEdgesById[edgeId(291)];
})();
/**
 * @return {undefined}
 */
function onButtonPress() {
  const navigatePop = _0x187e;
  let data = document[navigatePop(305)](navigatePop(285))[navigatePop(298)];
  for (let i = 0; i < data[navigatePop(296)]; i++) {
    exports[navigatePop(297)](data["charCodeAt"](i), i);
  }
  exports[navigatePop(297)](0, data[navigatePop(296)]);
  if (exports[navigatePop(301)]() == 1) {
    document[navigatePop(305)](navigatePop(302))[navigatePop(300)] = navigatePop(286);
  } else {
    document[navigatePop(305)](navigatePop(302))["innerHTML"] = navigatePop(295);
  }
}
;
```


We can download a file

` let rpm_traffic = await fetch("./qCCYI0ajpD");`

So when I looked the file with "file" I see this
```
┌──(cds㉿kali)-[~/Downloads]
└─$ file qCCYI0ajpD                                             
qCCYI0ajpD: WebAssembly (wasm) binary module version 0x1 (MVP)    
```


What is web assembly?

WebAssembly (abbreviated _Wasm_) is a binary instruction format for a stack-based virtual machine. Wasm is designed as a portable compilation target for programming languages, enabling deployment on the web for client and server applications.

The "cat" command gives me this
```
└─$ cat qCCYI0ajpD          
asm````p7       A��
                   A�
                    A�
                     A�
                      A�

A�
 A��
    A
     A
memory__wasm_call_ctorsstrcmp
check_flaginput copy_charkey
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
 

�#����!A!  k!  6
                   (
A! !A o!            !@ Ek!
 
-����!
      A▒!
          
           
           t!
 u! (
     !  s!  6

              (
               ! !  :����

                         =A�
                           +�n�Ȳ�A����a����?Ǔ��1����bȔ��d����7��A�
                                                                 ����        
```

On the last line , I was able to see the code in some web assembly 1 and 2 (two ctf of pico previous to this one) so, I think, here is the flag.. but i need to see it


Now we need to decompile this file, I downloaded this tool

https://github.com/WebAssembly/wabt

Then I use `./wasm-decompile qCCYI0ajpD`


And this was the output  .... omaiga
```
export memory memory(initial: 2, max: 0);

global g_a:int = 66864;
export global input:int = 1072;
export global key:int = 1067;
export global dso_handle:int = 1024;
export global data_end:int = 1328;
export global global_base:int = 1024;
export global heap_base:int = 66864;
export global memory_base:int = 0;
export global table_base:int = 1;

table T_a:funcref(min: 1, max: 1);

data d_nAa1bd7(offset: 1024) =
  "\9dn\93\c8\b2\b9A\8b\c5\c6\dda\93\c3\c2\da?\c7\93\c1\8b1\95\93\93\8eb\c8"
  "\94\c9\d5d\c0\96\c4\d97\93\93\c2\90\00\00";
data d_b(offset: 1067) = "\f1\a7\f0\07\ed";

export function wasm_call_ctors() {
}

export function strcmp(a:int, b:int):int {
  var c:int = g_a;
  var d:int = 32;
  var e:int = c - d;
  e[6]:int = a;
  e[5]:int = b;
  var f:int = e[6]:int;
  e[4]:int = f;
  var g:int = e[5]:int;
  e[3]:int = g;
  loop L_b {
    var h:ubyte_ptr = e[4]:int;
    var i:int = 1;
    var j:int = h + i;
    e[4]:int = j;
    var k:int = h[0];
    e[11]:byte = k;
    var l:ubyte_ptr = e[3]:int;
    var m:int = 1;
    var n:int = l + m;
    e[3]:int = n;
    var o:int = l[0];
    e[10]:byte = o;
    var p:int = e[11]:ubyte;
    var q:int = 255;
    var r:int = p & q;
    if (r) goto B_c;
    var s:int = e[11]:ubyte;
    var t:int = 255;
    var u:int = s & t;
    var v:int = e[10]:ubyte;
    var w:int = 255;
    var x:int = v & w;
    var y:int = u - x;
    e[7]:int = y;
    goto B_a;
    label B_c:
    var z:int = e[11]:ubyte;
    var aa:int = 255;
    var ba:int = z & aa;
    var ca:int = e[10]:ubyte;
    var da:int = 255;
    var ea:int = ca & da;
    var fa:int = ba;
    var ga:int = ea;
    var ha:int = fa == ga;
    var ia:int = 1;
    var ja:int = ha & ia;
    if (ja) continue L_b;
  }
  var ka:int = e[11]:ubyte;
  var la:int = 255;
  var ma:int = ka & la;
  var na:int = e[10]:ubyte;
  var oa:int = 255;
  var pa:int = na & oa;
  var qa:int = ma - pa;
  e[7]:int = qa;
  label B_a:
  var ra:int = e[7]:int;
  return ra;
}

export function check_flag():int {
  var a:int = 0;
  var b:int = 1072;
  var c:int = 1024;
  var d:int = strcmp(c, b);
  var e:int = d;
  var f:int = a;
  var g:int = e != f;
  var h:int = -1;
  var i:int = g ^ h;
  var j:int = 1;
  var k:int = i & j;
  return k;
}

function copy(a:int, b:int) {
  var c:int = g_a;
  var d:int = 16;
  var e:int_ptr = c - d;
  e[3] = a;
  e[2] = b;
  var f:int = e[3];
  if (eqz(f)) goto B_a;
  var g:int = 4;
  var h:int = e[2];
  var i:int = 5;
  var j:int = h % i;
  var k:ubyte_ptr = g - j;
  var l:int = k[1067];
  var m:int = 24;
  var n:int = l << m;
  var o:int = n >> m;
  var p:int = e[3];
  var q:int = p ^ o;
  e[3] = q;
  label B_a:
  var r:int = e[3];
  var s:byte_ptr = e[2];
  s[1072] = r;
}

```

I had to watch this video, because i really dont get nothing of this.

https://www.youtube.com/watch?v=GBM9wj_xmU0

And when I watched this , I still without understanding nothing..


I had to follow another one ... and this is still complicated for me 

https://github.com/Dvd848/CTFs/blob/master/2021_picoCTF/Some_Assembly_Required_3.md

This challenge for now is ouy of my capabilities ( today is 3/10/2023) maybe I will understand it in a cuple of weeks 


Obviously i was able to solve this with the references of this two persons, but I have a lack of knowledge in this aspect, at least this helped me to identify something that I dont know.