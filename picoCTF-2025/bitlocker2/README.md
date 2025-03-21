# Bitlocker-2

The disk image is encrypted.
```
$ dislocker-metadata -V bitlocker-2.dd
...
[INFO]     Encryption Type: AES-XTS-128 (0x8004)
...
```

I tried to crack the password the same as [bitlocker-1](../bitlocker/README.md). However, there was no luck.

Since a memory dump is attached to the challenge, I have to find the password from the memory dump. After googling, I found [Volatility-BitLocker](https://github.com/breppo/Volatility-BitLocker) plugin to retrieve the FVEK (File Volume Encryption Key) from memory dump.
```
$ vol.py -f memdump.mem --profile=Win10x64_19041 bitlocker --dump-dir fvek-dir

[FVEK] Address : 0x8087865bead0
[FVEK] Cipher  : AES-XTS 128 bit (Win 10+)
[FVEK] FVEK: 4f79d4a00d5e9b25965b89581a6a599c
[DUMP] FVEK dumped to file: fvek-dir/0x8087865bead0.fvek
...
```

The plugin found some keys.
> This will print the potential found FVEKs. The first returned should be the one as the plugin goes from the current Windows versions to the oldest.

According to the description, I tried the first key to mount the disk image.
```
$ dislocker r -V bitlocker-2.dd -k fvek-dir/0x8087865bead0.fvek -- /mnt/bitlocker
$ file /mnt/bitlocker/dislocker-file
/mnt/bitlocker/dislocker-file: DOS/MBR boot sector, code offset 0x52+2, OEM-ID "NTFS    ", sectors/cluster 8, Media descriptor 0xf8, sectors/track 63, heads 255, hidden sectors 41531392, dos < 4.0 BootSector (0x80), FAT (1Y bit by descriptor); NTFS, sectors/track 63, sectors 262143, $MFT start cluster 16981, $MFTMirror start cluster 2, bytes/RecordSegment 2^(-1*246), clusters/index block 1, serial number 0ec6ee03e6ee002e6; contains bootstrap BOOTMGR

$ mount -t ntfs-3g -o loop /mnt/bitlocker/dislocker-file /mnt/bitlocker-2
```

`flag.txt` was found.
```
$ ls /mnt/bitlocker-2
'$RECYCLE.BIN'   flag.txt  'System Volume Information'
```