# Tap into Hash

When I saw the source code, it looked bit complicated.
However, the `encrypt` function puts the `inner_txt` in the middle of the `plaintext`, then xor per block with the hash of the `key`.
I guess the `inner_txt` is the flag.
```python
def encrypt(plaintext, inner_txt, key):
    midpoint = len(plaintext) // 2

    first_part = plaintext[:midpoint]
    second_part = plaintext[midpoint:]
    modified_plaintext = first_part + inner_txt + second_part
    block_size = 16
    plaintext = pad(modified_plaintext, block_size)
    key_hash = hashlib.sha256(key).digest()

    ciphertext = b''

    for i in range(0, len(plaintext), block_size):
        block = plaintext[i:i + block_size]
        cipher_block = xor_bytes(block, key_hash)
        ciphertext += cipher_block

    return ciphertext
```

Therefore, I just tried to xor the ciphertext with the hash of the key, then the flag came out.
```
$ python exploit.py
Decrypted Blockchain: 5410603d236160940efdcaf26beca4ddfaf5327aaf41b730645f77d4ccfdded5-00118a79e0fbdbba6bfc52384735078d69073d211d4efbc15b35ce5a168ad59a-00f7f88f01862b79cff1f05cc3e3dc12picoCTF{block_3SRhViRbT1qcX_XUjM0r49cH_qCzmJZzBK_41c10331}1123443252a07cca666af8e7ace2495f-000f669f06d71a4263f0353d23bc3968d6ba3e3a123ff8a4581d730ddb5cedde-009df6f0bf347f93ff88a7ff8b381550eeb65f198479a008635eeb691cf34f2c
```