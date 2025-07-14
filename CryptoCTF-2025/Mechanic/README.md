# Mechanic

The `mechanic.py` chains multiple layers of encryption on the original file based on the bits of a large random number, writing out each secret key. If the bit is 1, it encrypts and moves to the next layer. If the bit is 0, it just consumes and records a dummy key pair, continuing with the current file unchanged. the generated keys are stored in `output.raw`.

```python
kem = MLKEM_1024()
kry = KryptonKEM(MLKEM_1024)
pt = Path('/Mechanic/flag.png')
f = open('output.raw', 'wb')
m = randint(2 ** 39, 2 ** 40)
B, c = bin(m)[2:], 0
for b in B:
	if b == '1':
		pkey, skey = kem.keygen()
		ct = Path(f'/flag_{c}.enc')
		kry.encrypt(pkey, pt, ct)
		pt = ct
		c += 1
	else:
		pkey, skey = urandom(kem.param_sizes.pk_size), urandom(kem.param_sizes.sk_size)
	f.write(skey)
```

To decrypt the file, I load the secret keys from `output.raw` and decrypt the file layer by iterating over all stored secret keys in reverse. If the decryption fails, it skips the key since it's a random one.
```
$ python exploit.py
Loaded 40 skey candidates.
Successfully decrypted layer 22 with skey 40
Successfully decrypted layer 21 with skey 39
Successfully decrypted layer 20 with skey 38
...
Successfully decrypted layer 0 with skey 1
```

When I look at the final decrypted file, it's a PNG image that includes the flag.
```
$ file decrypted_layer_0.bin
decrypted_layer_0.bin: PNG image data, 1454 x 74, 8-bit/color RGBA, non-interlaced
```
