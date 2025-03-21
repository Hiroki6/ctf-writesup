# Guess My Cheese (Part 1)

After I played around the server, I learned
- This server returns encrypted input text when I put cheese names such as cheddar and mozzallea. It's case insensitive.
- Each letter is always converted to a corresponding letter in the alphabet.
- The conversion always changes every time I connect to the server.

> The one that incorporates your affinity for linear equations???

Based on the hint, the cipher seems to be affine cipher.

Affine cipher satisfies the function.
```
E(x) = (a * x + b) mod m
```
In that case, m is 26, which is the number of letters in the alphabet.

For example, when `C`(3rd letter) becomes `Y`(25th letter), I need to find a and b to satisfy
```
24 = (a * 2 + b) % 26
```

So, the solution would be
1. put one cheese name and get the ciphertext
2. use the pair of cheese name and ciphertext to find `a` and `b`
3. decrypt the target ciphertext

I used [Z3](https://github.com/Z3Prover/z3) to find `a` and `b`.

```
$ python exploit.py
```
