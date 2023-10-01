


I cant see nothing with cat.. so i had to search a tutorial or something, i cant get the flag while I finish the game

we need a flag from a server that we can get ...

So, it’s a pwn challenge. For those first-timers, basically you’re given a program that’s also being run on a server. The task is to find a flaw in that program so that you can retrieve the real flag from the server.

this retrieves info of the binary , using "file"

```
kali > file racecar


racecar: ELF 32-bit LSB pie executable, Intel 80386, version 1 (SYSV), dynamically linked, interpreter /lib/ld-linux.so.2, for GNU/Linux 3.2.0, BuildID[sha1]=c5631a370f7704c44312f6692e1da56c25c1863c, not stripped
```

It is an ELF 32 binary, and it is "not stripped"

This means, that the debugging information still there

```
readelf -s racecar
```

This is what readelf do: 
```
When working with different programs and compilers like the **gcc**, you often end up compiling the programs in a binary format that are executable. The object file generated is only understandable by the machine, and the only way that humans can work and understand its contents is by using the **readelf** command. With readelf, you can extract the information from the ELF (Executable and Linkable Format) files. The readelf program is almost similar to the **objdump**. But with readelf, you get more specific details and unlike objdump, it doesn’t rely on the **BFD** library.
```



This is all the info: 

```
Symbol table '.dynsym' contains 27 entries:
   Num:    Value  Size Type    Bind   Vis      Ndx Name
     0: 00000000     0 NOTYPE  LOCAL  DEFAULT  UND 
     1: 00000000     0 FUNC    GLOBAL DEFAULT  UND strcmp@GLIBC_2.0 (2)
     2: 00000000     0 FUNC    GLOBAL DEFAULT  UND read@GLIBC_2.0 (2)
     3: 00000000     0 NOTYPE  WEAK   DEFAULT  UND _ITM_deregisterT[...]
     4: 00000000     0 FUNC    GLOBAL DEFAULT  UND printf@GLIBC_2.0 (2)
     5: 00000000     0 FUNC    GLOBAL DEFAULT  UND fgets@GLIBC_2.0 (2)
     6: 00000000     0 FUNC    GLOBAL DEFAULT  UND time@GLIBC_2.0 (2)
     7: 00000000     0 FUNC    GLOBAL DEFAULT  UND sleep@GLIBC_2.0 (2)
     8: 00000000     0 FUNC    GLOBAL DEFAULT  UND alarm@GLIBC_2.0 (2)
     9: 00000000     0 FUNC    GLOBAL DEFAULT  UND __[...]@GLIBC_2.4 (3)
    10: 00000000     0 FUNC    WEAK   DEFAULT  UND [...]@GLIBC_2.1.3 (4)
    11: 00000000     0 FUNC    GLOBAL DEFAULT  UND malloc@GLIBC_2.0 (2)
    12: 00000000     0 FUNC    GLOBAL DEFAULT  UND puts@GLIBC_2.0 (2)
    13: 00000000     0 NOTYPE  WEAK   DEFAULT  UND __gmon_start__
    14: 00000000     0 FUNC    GLOBAL DEFAULT  UND exit@GLIBC_2.0 (2)
    15: 00000000     0 FUNC    GLOBAL DEFAULT  UND srand@GLIBC_2.0 (2)
    16: 00000000     0 FUNC    GLOBAL DEFAULT  UND strlen@GLIBC_2.0 (2)
    17: 00000000     0 FUNC    GLOBAL DEFAULT  UND __[...]@GLIBC_2.0 (2)
    18: 00000000     0 OBJECT  GLOBAL DEFAULT  UND stdin@GLIBC_2.0 (2)
    19: 00000000     0 FUNC    GLOBAL DEFAULT  UND setvbuf@GLIBC_2.0 (2)
    20: 00000000     0 FUNC    GLOBAL DEFAULT  UND fopen@GLIBC_2.1 (5)
    21: 00000000     0 FUNC    GLOBAL DEFAULT  UND putchar@GLIBC_2.0 (2)
    22: 00000000     0 FUNC    GLOBAL DEFAULT  UND rand@GLIBC_2.0 (2)
    23: 00000000     0 OBJECT  GLOBAL DEFAULT  UND stdout@GLIBC_2.0 (2)
    24: 00000000     0 FUNC    GLOBAL DEFAULT  UND atoi@GLIBC_2.0 (2)
    25: 00000000     0 NOTYPE  WEAK   DEFAULT  UND _ITM_registerTMC[...]
    26: 0000152c     4 OBJECT  GLOBAL DEFAULT   16 _IO_stdin_used

Symbol table '.symtab' contains 96 entries:
   Num:    Value  Size Type    Bind   Vis      Ndx Name
     0: 00000000     0 NOTYPE  LOCAL  DEFAULT  UND 
     1: 00000154     0 SECTION LOCAL  DEFAULT    1 .interp
     2: 00000168     0 SECTION LOCAL  DEFAULT    2 .note.ABI-tag
     3: 00000188     0 SECTION LOCAL  DEFAULT    3 .note.gnu.build-id
     4: 000001ac     0 SECTION LOCAL  DEFAULT    4 .gnu.hash
     5: 000001cc     0 SECTION LOCAL  DEFAULT    5 .dynsym
     6: 0000037c     0 SECTION LOCAL  DEFAULT    6 .dynstr
     7: 000004a8     0 SECTION LOCAL  DEFAULT    7 .gnu.version
     8: 000004e0     0 SECTION LOCAL  DEFAULT    8 .gnu.version_r
     9: 00000530     0 SECTION LOCAL  DEFAULT    9 .rel.dyn
    10: 00000580     0 SECTION LOCAL  DEFAULT   10 .rel.plt
    11: 00000618     0 SECTION LOCAL  DEFAULT   11 .init
    12: 00000640     0 SECTION LOCAL  DEFAULT   12 .plt
    13: 00000780     0 SECTION LOCAL  DEFAULT   13 .plt.got
    14: 00000790     0 SECTION LOCAL  DEFAULT   14 .text
    15: 00001514     0 SECTION LOCAL  DEFAULT   15 .fini
    16: 00001528     0 SECTION LOCAL  DEFAULT   16 .rodata
    17: 00001d7c     0 SECTION LOCAL  DEFAULT   17 .eh_frame_hdr
    18: 00001df8     0 SECTION LOCAL  DEFAULT   18 .eh_frame
    19: 00003e8c     0 SECTION LOCAL  DEFAULT   19 .init_array
    20: 00003e90     0 SECTION LOCAL  DEFAULT   20 .fini_array
    21: 00003e94     0 SECTION LOCAL  DEFAULT   21 .dynamic
    22: 00003f8c     0 SECTION LOCAL  DEFAULT   22 .got
    23: 00004000     0 SECTION LOCAL  DEFAULT   23 .data
    24: 00004010     0 SECTION LOCAL  DEFAULT   24 .bss
    25: 00000000     0 SECTION LOCAL  DEFAULT   25 .comment
    26: 00000000     0 FILE    LOCAL  DEFAULT  ABS crtstuff.c
    27: 000007e0     0 FUNC    LOCAL  DEFAULT   14 deregister_tm_clones
    28: 00000820     0 FUNC    LOCAL  DEFAULT   14 register_tm_clones
    29: 00000870     0 FUNC    LOCAL  DEFAULT   14 __do_global_dtors_aux
    30: 00004010     1 OBJECT  LOCAL  DEFAULT   24 completed.7283
    31: 00003e90     0 OBJECT  LOCAL  DEFAULT   20 __do_global_dtor[...]
    32: 000008c0     0 FUNC    LOCAL  DEFAULT   14 frame_dummy
    33: 00003e8c     0 OBJECT  LOCAL  DEFAULT   19 __frame_dummy_in[...]
    34: 00000000     0 FILE    LOCAL  DEFAULT  ABS racecar.c
    35: 00000000     0 FILE    LOCAL  DEFAULT  ABS crtstuff.c
    36: 00002018     0 OBJECT  LOCAL  DEFAULT   18 __FRAME_END__
    37: 00000000     0 FILE    LOCAL  DEFAULT  ABS 
    38: 00003e90     0 NOTYPE  LOCAL  DEFAULT   19 __init_array_end
    39: 00003e94     0 OBJECT  LOCAL  DEFAULT   21 _DYNAMIC
    40: 00003e8c     0 NOTYPE  LOCAL  DEFAULT   19 __init_array_start
    41: 00001d7c     0 NOTYPE  LOCAL  DEFAULT   17 __GNU_EH_FRAME_HDR
    42: 00003f8c     0 OBJECT  LOCAL  DEFAULT   22 _GLOBAL_OFFSET_TABLE_
    43: 000014f0     2 FUNC    GLOBAL DEFAULT   14 __libc_csu_fini
    44: 00000000     0 FUNC    GLOBAL DEFAULT  UND strcmp@@GLIBC_2.0
    45: 00000000     0 FUNC    GLOBAL DEFAULT  UND read@@GLIBC_2.0
    46: 00000000     0 NOTYPE  WEAK   DEFAULT  UND _ITM_deregisterT[...]
    47: 000007d0     4 FUNC    GLOBAL HIDDEN    14 __x86.get_pc_thunk.bx
    48: 00004000     0 NOTYPE  WEAK   DEFAULT   23 data_start
    49: 00000000     0 FUNC    GLOBAL DEFAULT  UND printf@@GLIBC_2.0
    50: 00000000     0 FUNC    GLOBAL DEFAULT  UND fgets@@GLIBC_2.0
    51: 00004010     0 NOTYPE  GLOBAL DEFAULT   23 _edata
    52: 00000000     0 FUNC    GLOBAL DEFAULT  UND time@@GLIBC_2.0
    53: 00000000     0 FUNC    GLOBAL DEFAULT  UND sleep@@GLIBC_2.0
    54: 00001352   143 FUNC    GLOBAL DEFAULT   14 menu
    55: 00001514     0 FUNC    GLOBAL DEFAULT   15 _fini
    56: 00000000     0 FUNC    GLOBAL DEFAULT  UND alarm@@GLIBC_2.0
    57: 00000000     0 FUNC    GLOBAL DEFAULT  UND __stack_chk_fail[...]
    58: 000008c9     0 FUNC    GLOBAL HIDDEN    14 __x86.get_pc_thunk.dx
    59: 00000000     0 FUNC    WEAK   DEFAULT  UND __cxa_finalize@@[...]
    60: 00000929   618 FUNC    GLOBAL DEFAULT   14 banner
    61: 00000000     0 FUNC    GLOBAL DEFAULT  UND malloc@@GLIBC_2.0
    62: 00004000     0 NOTYPE  GLOBAL DEFAULT   23 __data_start
    63: 00000000     0 FUNC    GLOBAL DEFAULT  UND puts@@GLIBC_2.0
    64: 00000000     0 NOTYPE  WEAK   DEFAULT  UND __gmon_start__
    65: 00000000     0 FUNC    GLOBAL DEFAULT  UND exit@@GLIBC_2.0
    66: 00004004     0 OBJECT  GLOBAL HIDDEN    23 __dso_handle
    67: 0000152c     4 OBJECT  GLOBAL DEFAULT   16 _IO_stdin_used
    68: 00000c02   143 FUNC    GLOBAL DEFAULT   14 race_type
    69: 00000000     0 FUNC    GLOBAL DEFAULT  UND srand@@GLIBC_2.0
    70: 00000000     0 FUNC    GLOBAL DEFAULT  UND strlen@@GLIBC_2.0
    71: 00000000     0 FUNC    GLOBAL DEFAULT  UND __libc_start_mai[...]
    72: 00001490    93 FUNC    GLOBAL DEFAULT   14 __libc_csu_init
    73: 00000c91  1009 FUNC    GLOBAL DEFAULT   14 car_menu
    74: 00000000     0 OBJECT  GLOBAL DEFAULT  UND stdin@@GLIBC_2.0
    75: 000008cd    92 FUNC    GLOBAL DEFAULT   14 read_int
    76: 00000000     0 FUNC    GLOBAL DEFAULT  UND setvbuf@@GLIBC_2.0
    77: 00000000     0 FUNC    GLOBAL DEFAULT  UND fopen@@GLIBC_2.1
    78: 00000000     0 FUNC    GLOBAL DEFAULT  UND putchar@@GLIBC_2.0
    79: 00004014     0 NOTYPE  GLOBAL DEFAULT   24 _end
    80: 00000790     0 FUNC    GLOBAL DEFAULT   14 _start
    81: 00001528     4 OBJECT  GLOBAL DEFAULT   16 _fp_hw
    82: 000011d2   384 FUNC    GLOBAL DEFAULT   14 car_info
    83: 00000000     0 FUNC    GLOBAL DEFAULT  UND rand@@GLIBC_2.0
    84: 00000000     0 OBJECT  GLOBAL DEFAULT  UND stdout@@GLIBC_2.0
    85: 00004010     0 NOTYPE  GLOBAL DEFAULT   24 __bss_start
    86: 000013e1   168 FUNC    GLOBAL DEFAULT   14 main
    87: 00004008     4 OBJECT  GLOBAL DEFAULT   23 coins
    88: 00001500    20 FUNC    GLOBAL HIDDEN    14 __stack_chk_fail[...]
    89: 0000400c     4 OBJECT  GLOBAL DEFAULT   23 check
    90: 00000000     0 FUNC    GLOBAL DEFAULT  UND atoi@@GLIBC_2.0
    91: 00004010     0 OBJECT  GLOBAL HIDDEN    23 __TMC_END__
    92: 00000000     0 NOTYPE  WEAK   DEFAULT  UND _ITM_registerTMC[...]
    93: 00000618     0 FUNC    GLOBAL DEFAULT   11 _init
    94: 00001082   336 FUNC    GLOBAL DEFAULT   14 info
    95: 00000b93   111 FUNC    GLOBAL DEFAULT   14 setup

```

OK, i used GHIDRA... it is a "debuger" and  it returned me this :

```
/* WARNING: Function: __x86.get_pc_thunk.bx replaced with injection: get_pc_thunk_bx */

void main(void)

{
  int iVar1;
  int iVar2;
  int in_GS_OFFSET;
  
  iVar1 = *(int *)(in_GS_OFFSET + 0x14);
  setup();
  banner();
  info();
  while (check != 0) {
    iVar2 = menu();
    if (iVar2 == 1) {
      car_info();
    }
    else if (iVar2 == 2) {
      check = 0;
      car_menu();
    }
    else {
      printf("\n%s[-] Invalid choice!%s\n",&DAT_00011548,&DAT_00011538);
    }
  }
  if (iVar1 != *(int *)(in_GS_OFFSET + 0x14)) {
    __stack_chk_fail_local();
  }
  return;
}

```

And this is the code of "car menu
```
void car_menu(void)

{
  int iVar1;
  int iVar2;
  uint __seed;
  int iVar3;
  size_t sVar4;
  char *__format;
  FILE *__stream;
  int in_GS_OFFSET;
  undefined *puVar5;
  undefined4 uVar6;
  undefined4 uVar7;
  uint local_54;
  char local_3c [44];
  int local_10;
  
  local_10 = *(int *)(in_GS_OFFSET + 0x14);
  uVar6 = 0xffffffff;
  uVar7 = 0xffffffff;
  do {
    printf(&DAT_00011948);
    iVar1 = read_int(uVar6,uVar7);
    if ((iVar1 != 2) && (iVar1 != 1)) {
      printf("\n%s[-] Invalid choice!%s\n",&DAT_00011548,&DAT_00011538);
    }
  } while ((iVar1 != 2) && (iVar1 != 1));
  iVar2 = race_type();
  __seed = time((time_t *)0x0);
  srand(__seed);
  if (((iVar1 == 1) && (iVar2 == 2)) || ((iVar1 == 2 && (iVar2 == 2)))) {
    iVar2 = rand();
    iVar2 = iVar2 % 10;
    iVar3 = rand();
    iVar3 = iVar3 % 100;
  }
  else if (((iVar1 == 1) && (iVar2 == 1)) || ((iVar1 == 2 && (iVar2 == 1)))) {
    iVar2 = rand();
    iVar2 = iVar2 % 100;
    iVar3 = rand();
    iVar3 = iVar3 % 10;
  }
  else {
    iVar2 = rand();
    iVar2 = iVar2 % 100;
    iVar3 = rand();
    iVar3 = iVar3 % 100;
  }
  local_54 = 0;
  while( true ) {
    sVar4 = strlen("\n[*] Waiting for the race to finish...");
    if (sVar4 <= local_54) break;
    putchar((int)"\n[*] Waiting for the race to finish..."[local_54]);
    if ("\n[*] Waiting for the race to finish..."[local_54] == '.') {
      sleep(0);
    }
    local_54 = local_54 + 1;
  }
  if (((iVar1 == 1) && (iVar2 < iVar3)) || ((iVar1 == 2 && (iVar3 < iVar2)))) {
    printf("%s\n\n[+] You won the race!! You get 100 coins!\n",&DAT_00011540);
    coins = coins + 100;
    puVar5 = &DAT_00011538;
    printf("[+] Current coins: [%d]%s\n",coins,&DAT_00011538);
    printf("\n[!] Do you have anything to say to the press after your big victory?\n> %s",
           &DAT_000119de);
    __format = (char *)malloc(0x171);
    __stream = fopen("flag.txt","r");
    if (__stream == (FILE *)0x0) {
      printf("%s[-] Could not open flag.txt. Please contact the creator.\n",&DAT_00011548,puVar5);
                    /* WARNING: Subroutine does not return */
      exit(0x69);
    }
    fgets(local_3c,0x2c,__stream);
    read(0,__format,0x170);
    puts(
        "\n\x1b[3mThe Man, the Myth, the Legend! The grand winner of the race wants the whole world  to know this: \x1b[0m"
        );
    printf(__format);
  }
  else if (((iVar1 == 1) && (iVar3 < iVar2)) || ((iVar1 == 2 && (iVar2 < iVar3)))) {
    printf("%s\n\n[-] You lost the race and all your coins!\n",&DAT_00011548);
    coins = 0;
    printf("[+] Current coins: [%d]%s\n",0,&DAT_00011538);
  }
  if (local_10 != *(int *)(in_GS_OFFSET + 0x14)) {
    __stack_chk_fail_local();
  }
  return;
}

```


On the code, this is vulnerable I was reading over there
```
 printf(__format);
 ```
the name is format string vulnerability

https://owasp.org/www-community/attacks/Format_string_attack


so when we play the game and win we can insert in an input something like this 

```
•”%x” Read data from the stack

•”%s” Read character strings from the process’ memory

•”%n” Write an integer to locations in the process’ memory
```


The guy that im searchin files and things made a script with the library "re" in python

```
# [`re`](https://docs.python.org/es/3/library/re.html#module-re "re: Regular expression operations.") — Operaciones con expresiones regulares[](https://docs.python.org/es/3/library/re.html#module-re "Enlazar permanentemente con este título")

**Source code:** [Lib/re/](https://github.com/python/cpython/tree/3.11/Lib/re/)

---

Este módulo proporciona operaciones de coincidencia de expresiones regulares similares a las encontradas en Perl.

Tanto los patrones como las cadenas de texto a buscar pueden ser cadenas de Unicode ([`str`](https://docs.python.org/es/3/library/stdtypes.html#str "str")) así como cadenas de 8 bits ([`bytes`](https://docs.python.org/es/3/library/stdtypes.html#bytes "bytes")). Sin embargo, las cadenas Unicode y las cadenas de 8 bits no se pueden mezclar: es decir, no se puede hacer coincidir una cadena Unicode con un patrón de bytes o viceversa; del mismo modo, al pedir una sustitución, la cadena de sustitución debe ser del mismo tipo que el patrón y la cadena de búsqueda.

```


And this is the script
```
#!/usr/bin/env python3
import re
raw_flag = input("Enter the raw data: ")
raw_flag = raw_flag.split()[::-1]
for i in range(len(raw_flag)):
    raw_flag[i] = re.findall('..', raw_flag[i])
    
flag = [] 
for chars in raw_flag:
     word = "" 
     for char in chars[::-1]:
        if char != '0x':
         word += chr(int(char, 16))
     flag.append(word) 
flag.reverse() 
print(''.join(flag))

```

So, this is the solution, first you run the script, firs you connect to the server, do the game and then you insert the raw data on the python script, and then it bring you back this :