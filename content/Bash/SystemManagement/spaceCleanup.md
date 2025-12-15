

## See space taken by folders
```bash
sudo du -xh --max-depth=1 /
```

## Cleanup `/tmp` folder
deletes files older than 2 days
```bash
find /tmp -type f \( ! -user root \) -atime +2 -delete
```

## Cleanup `/tmp` folder
deletes files older than 31 days
```bash
find ~/.cache/ -depth -type f -atime +31 -delete
```

## Shrink WSL vdisk
```
wsl --shutdown
wsl.exe --list --verbose
diskpart
DISKPART> select vdisk file="<path to vhdx file>"
DISKPART> compact vdisk
```



