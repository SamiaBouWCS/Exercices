```
samia@lvm:~$ sudo /sbin/vgdisplay
  --- Volume group ---

  VG Name               lvm-vg

  Format                lvm2

  VG Size               <19,52 GiB
  ```
samia@lvm:~$ sudo /sbin/vgdisplay 
  --- Volume group ---

  VG Name               lvm-vg

  Format                lvm2

  VG Size               <39,52 GiB
La création d'un snapshot du LV home
```
samia@lvm:~$ sudo lvcreate -L 12g -s -n lv_home /dev/lvm-vg/home

  Reducing COW size 12,00 GiB down to maximum usable size 11,80 GiB.

  Logical volume "lv_home" created.
Il y a bien création d'un dossier home-snap et montage du snapshot dans ce dossier
```
samia@lvm:~$ lsblk

NAME                  MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS

sda                     8:0    0   20G  0 disk 

├─sda1                  8:1    0  487M  0 part /boot

├─sda2                  8:2    0    1K  0 part 

└─sda5                  8:5    0 19,5G  0 part 

  ├─lvm--vg-root      254:0    0  6,8G  0 lvm  /

  ├─lvm--vg-swap_1    254:1    0  976M  0 lvm  [SWAP]

  └─lvm--vg-home-real 254:3    0 11,8G  0 lvm  

    ├─lvm--vg-home    254:2    0 11,8G  0 lvm  /home

    └─lvm--vg-lv_home 254:5    0 11,8G  0 lvm  /home-snap

sdb                     8:16   0   20G  0 disk 

└─lvm--vg-lv_home-cow 254:4    0 11,8G  0 lvm  

  └─lvm--vg-lv_home   254:5    0 11,8G  0 lvm  /home-snap

sr0                    11:0    1   51M  0 rom  
L'affichage des systèmes de fichiers actuellement montés n'affiche plus /home-snap
```
samia@lvm:~$ mount | grep home

/dev/mapper/lvm--vg-home on /home type ext4 (rw,relatime)
L'affichage des LV n'affiche plus le snapshot et le LV home n'est plus la source d'aucun snapshot
```
samia@lvm:~$ lsblk

NAME               MAJ:MIN RM  SIZE RO TYPE MOUNTPOINTS

sda                  8:0    0   20G  0 disk 

├─sda1               8:1    0  487M  0 part /boot

├─sda2               8:2    0    1K  0 part 

└─sda5               8:5    0 19,5G  0 part 

  ├─lvm--vg-root   254:0    0  6,8G  0 lvm  /

  ├─lvm--vg-swap_1 254:1    0  976M  0 lvm  [SWAP]

  └─lvm--vg-home   254:2    0 11,8G  0 lvm  /home

sdb                  8:16   0   20G  0 disk 

sr0                 11:0    1   51M  0 rom  
