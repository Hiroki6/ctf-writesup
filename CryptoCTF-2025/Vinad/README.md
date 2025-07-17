# Vinad

After some experiments, I found the `p` and `e` can have only two possible values based on the `R`.
So, I can reproduce the actual values of `p` and `e` to satisfy the condition so that I can decrypt the flag.
```python
while True:
    r = getRandomNBitInteger(512)
    p = vinad(r, R)
    q = n // p
    if isPrime(p):
        e = vinad(r + 0x10001, R)
        if GCD(e, (p - 1) * (q - 1)) == 1:
            break
```

```
$ python exploit.py
Flag: CCTF{s0lV1n9_4_Syst3m_0f_L1n3Ar_3qUaTi0n5_0vEr_7H3_F!3lD_F(2)!}
```
