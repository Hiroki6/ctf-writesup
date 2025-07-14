# Vainrat

Based on `vainrat.py`, this server prints `y` only after the user inputs `c` more than `randint(12, 19)` times.
```python
if ans == 'c':
	x, y = rat(x0, y0)
	x0, y0 = x, y
	c += 1
	if c <= randint(12, 19):
		pr(border, f'Unfortunately, the rat got away :-(')
	else: pr(border, f'y = {y}')
```

In this case, on the 14th and 15th attempts, the server revealed `y`.
```
┃ We know y0 = 0.821678202592029840640488834626560793324753326620256635180524944323511676443467897250373271205925559084872165722454754934788227497666
| Options: 
|       [C]atch the rat 
|       [Q]uit
...
┃ y = 0.773365245110229560369109516556336837413347439447480207344363740888993277142640995641465266908673913897758285634622864886689616731191
| Options: 
|       [C]atch the rat 
|       [Q]uit
C
┃ y = 0.773365244980878269659723009287896063454399827068809910292134194830046587885268911767353965271126936475827362832564262574076965973911
| Options: 
|       [C]atch the rat 
|       [Q]uit
```

Notice that the `y` from the 14th attempt is used as `y0` for the 15th computation.
This means we can compute the original `x0` and `x` at that step using:
```python
def get_x0_x(y0, y):
    x = (y ** 2) / y0
    x0 = 2 * x - y0
    return mp.mpf(x0), mp.mpf(x)

x0, x = reverse(y0, y)
```

Next, I reconstruct the initial values by reversing the rat function 14 times:
```python
def reverse_rat(x, y):
    y0 = (y ** 2) / x
    x0 = 2 * x - y0
    return mp.mpf(x0), mp.mpf(y0)

x_current = x0
y_current = y0
for i in range(14):
    x_current, y_current = reverse_rat(mp.mpf(x_current), mp.mpf(y_current))
```

Finally, I brute-force to recover the flag by trying different decimal shifts of the reconstructed `x`:
```
for d in range(1, len(str(original_x))):
    m_candidate_int = int(mp.nint(original_x * mp.power(10, d)))
    if len(str(m_candidate_int)) == d:
        try:
            message_bytes = long_to_bytes(m_candidate_int)
            print(f"Flag: {message_bytes.decode()}")
        except Exception:
            continue
```

```
$ python exploit.py
Flag: 
Flag: D
Flag:   
Flag: 
ZL
Flag: CCTF{h3Ur1s7!c5_anD_iNv4rIanTs_iN_CryptoCTF_2025!}
```
