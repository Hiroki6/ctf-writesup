# Flag Hunters

After I played around with the program and analyzed the source code, I found when the `lib` variable get `0`, the program displays the flag.
In order to do that, somehow I need to inject a line `RETURN 0`.
```python
elif re.match(r"RETURN [0-9]+", line):
  lip = int(line.split()[1])
```

Since it splits the line by `;` in the loop, when I input `;xxx`, `xxx` becomes a new line.
Therefore, I get the flag by injecting `; RETURN 0`.
```python
for line in song_lines[lip].split(';'):
```
