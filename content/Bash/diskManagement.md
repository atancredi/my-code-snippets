## how to find the partition where Windows is
```bash
sudo fdisk -l | grep "Microsoft basic data" | awk '{print $1}'
```

&nbsp;
## mount the partition inside a folder
```bash
sudo mount {partition} {folder}
```

&nbsp;
## how to mount a partition automatically at startup
1. edit /etc/fstab file
2. add this line
    ```bash
    # remember to separate each column with a tab
    {partition} {folder} ntfs-3g defaults 0	
    ```
3. save and reboot


&nbsp;
## mount an external drive in a WS
```bash
sudo mount -t drvfs G: /mnt/g
```