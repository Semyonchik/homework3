<details> <summary>Вывод lsblk и lvmdiskscan при загрузке:</summary>
	
		NAME                    MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
    sda                       8:0    0   40G  0 disk
    ├─sda1                    8:1    0    1M  0 part
    ├─sda2                    8:2    0    1G  0 part /boot
    └─sda3                    8:3    0   39G  0 part
      ├─VolGroup00-LogVol00 253:0    0 37.5G  0 lvm  /
      └─VolGroup00-LogVol01 253:1    0  1.5G  0 lvm  [SWAP]
    sdb                       8:16   0   10G  0 disk
    sdc                       8:32   0    2G  0 disk
    sdd                       8:48   0    1G  0 disk
    sde                       8:64   0    1G  0 disk


    /dev/VolGroup00/LogVol00 [     <37.47 GiB]
    /dev/VolGroup00/LogVol01 [       1.50 GiB]
    /dev/sda2                [       1.00 GiB]
    /dev/sda3                [     <39.00 GiB] LVM physical volume
    /dev/sdb                 [      10.00 GiB]
    /dev/sdc                 [       2.00 GiB]
    /dev/sdd                 [       1.00 GiB]
    /dev/sde                 [       1.00 GiB]
    4 disks
    3 partitions
    0 LVM physical volume whole disks
    1 LVM physical volume
  </details>
 
 Установил xfsdump для снятия бекапа.
 
    yum install xfsdump
    
Создал временный том для бекапа корневого раздела, создал на нем ФС, смонтировал и сделал бекап.

    pvcreate /dev/sdb
    vgcreate vg_root /dev/sdb
    lvcreate -n lv_root -l +100%FREE /dev/vg_root
    mkfs.xfs /dev/vg_root/lv_root
    mount /dev/vg_root/lv_root /mnt
    mkfs.xfs /dev/VolGroup00/LogVol$
    xfsdump -J - /dev/vg_root/lv_ro$

Смонтировал системные директории в /mnt/ и сделал /mnt/ корнем

	  mount --bind /proc /mnt/proc
	  mount --bind /dev /mnt/dev
	  mount --bind /sys /mnt/sys
	  mount --bind /run /mnt/run
    mount --bind /boot /mnt/boot
	  chroot /mnt/
  
Создал новый конфиг grub и изменил в нем rd.lvm.lv=VolGroup00/LogVol00 на rd.lvm.lv=vg_root/lv_root
    
     grub2-mkconfig -o /boot/grub2/grub.cfg
     
Обновил образ initrd согласно методичке

    cd /boot ; for i in `ls initramfs-*img`; do dracut -v $i `echo $i|sed "s/initramfs-//g;s/.img//g"` --force; done

Пеерезагрузился с нового диска.

<details> <summary>Вывод lsblk после перезагрузки:</summary>
	
		NAME                    MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
    sda                       8:0    0   40G  0 disk
    ├─sda1                    8:1    0    1M  0 part
    ├─sda2                    8:2    0    1G  0 part /boot
    └─sda3                    8:3    0   39G  0 part
      ├─VolGroup00-LogVol01 253:1    0  1.5G  0 lvm  [SWAP]
      └─VolGroup00-LogVol00 253:2    0 37.5G  0 lvm
    sdb                       8:16   0   10G  0 disk
    └─vg_root-lv_root       253:0    0   10G  0 lvm  /
    sdc                       8:32   0    2G  0 disk
    sdd                       8:48   0    1G  0 disk
    sde                       8:64   0    1G  0 disk
</details>

Уменьшил исходный том до 8Gb. Удалил его и создал с новыми параметрами.

    pvcreate /dev/sdc /dev/sdd
    vgcreate vg_var /dev/sdc /dev/sdd
  Volume group "vg_var" successfully created
^[kroot@lvm:/boot^[\[root@lvm boot]# lsblk
NAME                    MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda                       8:0    0   40G  0 disk
├─sda1                    8:1    0    1M  0 part
├─sda2                    8:2    0    1G  0 part /boot
└─sda3                    8:3    0   39G  0 part
  ├─VolGroup00-LogVol01 253:1    0  1.5G  0 lvm  [SWAP]
  └─VolGroup00-LogVol00 253:2    0    8G  0 lvm  /
sdb                       8:16   0   10G  0 disk
└─vg_root-lv_root       253:0    0   10G  0 lvm
sdc                       8:32   0    2G  0 disk
sdd                       8:48   0    1G  0 disk
sde                       8:64   0    1G  0 disk
^[kroot@lvm:/boot^[\[root@lvm boot]# lvcreate -L 950M -m1 -n lv_var vg_var
  Rounding up size to full physical extent 952.00 MiB
  Logical volume "lv_var" created.
^[kroot@lvm:/boot^[\[root@lvm boot]# lsblk
NAME                     MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda                        8:0    0   40G  0 disk
├─sda1                     8:1    0    1M  0 part
├─sda2                     8:2    0    1G  0 part /boot
└─sda3                     8:3    0   39G  0 part
  ├─VolGroup00-LogVol01  253:1    0  1.5G  0 lvm  [SWAP]
  └─VolGroup00-LogVol00  253:2    0    8G  0 lvm  /
sdb                        8:16   0   10G  0 disk
└─vg_root-lv_root        253:0    0   10G  0 lvm
sdc                        8:32   0    2G  0 disk
├─vg_var-lv_var_rmeta_0  253:3    0    4M  0 lvm
│ └─vg_var-lv_var        253:7    0  952M  0 lvm
└─vg_var-lv_var_rimage_0 253:4    0  952M  0 lvm
  └─vg_var-lv_var        253:7    0  952M  0 lvm
sdd                        8:48   0    1G  0 disk
├─vg_var-lv_var_rmeta_1  253:5    0    4M  0 lvm
│ └─vg_var-lv_var        253:7    0  952M  0 lvm
└─vg_var-lv_var_rimage_1 253:6    0  952M  0 lvm
  └─vg_var-lv_var        253:7    0  952M  0 lvm
sde                        8:64   0    1G  0 disk
^[kroot@lvm:/boot^[\[root@lvm boot]# mkfs.ext4 /dev/vg_var/lv_var
mke2fs 1.42.9 (28-Dec-2013)
Filesystem label=
OS type: Linux
Block size=4096 (log=2)
Fragment size=4096 (log=2)
Stride=0 blocks, Stripe width=0 blocks
60928 inodes, 243712 blocks
12185 blocks (5.00%) reserved for the super user
First data block=0
Maximum filesystem blocks=249561088
8 block groups
32768 blocks per group, 32768 fragments per group
7616 inodes per group
Superblock backups stored on blocks:
        32768, 98304, 163840, 229376

Allocating group tables: 0/8^H^H^H   ^H^H^Hdone
Writing inode tables: 0/8^H^H^H   ^H^H^Hdone
Creating journal (4096 blocks): done
Writing superblocks and filesystem accounting information: 0/8^H^H^H   ^H^H^Hdo$

^[kroot@lvm:/boot^[\[root@lvm boot]# mount /dev/vg_var/lv_var /mnt
^[kroot@lvm:/boot^[\[root@lvm boot]# cp -aR /var/* /mnt/ # rsync -avHPSAX /var/$
^[kroot@lvm:/boot^[\[root@lvm boot]# mkdir /tmp/oldvar && mv /var/* /tmp/oldvar
^[kroot@lvm:/boot^[\[root@lvm boot]#  umount /mnt^M^H^H^[[K
^[kroot@lvm:/boot^[\[root@lvm boot]# mount /dev/vg_var/lv_var /var
^[kroot@lvm:/boot^[\[root@lvm boot]# ^C
^[kroot@lvm:/boot^[\[root@lvm boot]# ^V^H^H^[[Kecho "`blkid | grep var: | awk '$
 0 0" >> /etc/fstab
^[kroot@lvm:/boot^[\[root@lvm boot]# telinit 6
Running in chroot, ignoring request.
^[kroot@lvm:/boot^[\[root@lvm boot]# reboot
Running in chroot, ignoring request.
^[kroot@lvm:/boot^[\[root@lvm boot]# cd /
^[kroot@lvm:/^[\[root@lvm /]# telinit 6
Running in chroot, ignoring request.
^[kroot@lvm:/^[\[root@lvm /]# R^H^[[K
^[[8@(reverse-i-search)`':^[[C^G
^[[8P[root@lvm /]#^[[C^C
^[kroot@lvm:/^[\[root@lvm /]# k^G^G^G^G^G^G^G^G^G^G^G^G^Gkk^G^G^G^G^G^G^G^G^G^G$
bash: kkk: command not found
^[kroot@lvm:/^[\[root@lvm /]# ^G^G^G^Gk^G^H^[[K^G
[root@otuslinux ~]# lvremove /dev/vg_root/lv_root
[root@otuslinux ~]# vgremove /dev/vg_root
[root@otuslinux ~]# pvremove /dev/sdb
Выделить том под /home
Вýделāем том под /home по тому же принципу что делали длā /var:
[root@otuslinux ~]# lvcreate -n LogVol_Home -L 2G /dev/VolGroup00
[root@otuslinux ~]# mkfs.xfs /dev/VolGroup00/LogVol_Home
[root@otuslinux ~]# mount /dev/VolGroup00/LogVol_Home /mnt/
[root@otuslinux ~]# cp -aR /home/* /mnt/
[root@otuslinux ~]# rm -rf /home/*
[root@otuslinux ~]# umount /mnt
[root@otuslinux ~]# mount /dev/VolGroup00/LogVol_Home /home/
Правим fstab длā автоматического монтированиā /home
[root@otuslinux ~]# echo "`blkid | grep Home | awk '{print $2}'` /home xfs defaults 0 0" >> /etc/fstab
/home - сделать том для снапшотов
Сгенерируем файлý в /home/:
[root@otuslinux ~]# touch /home/file{1..20}
Снāтþ снапшот:
[root@otuslinux ~]# lvcreate -L 100MB -s -n home_snap /dev/VolGroup00/LogVol_Home
Удалитþ частþ файлов:
[root@otuslinux ~]# rm -f /home/file{11..20}
Процесс восстановлениā со снапшота:
[root@otuslinux ~]# umount /home
[root@otuslinux ~]# lvconvert --merge /dev/VolGroup00/home_snap
[root@otuslinux ~]# mount /home


NAME                       MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
sda                          8:0    0   40G  0 disk
├─sda1                       8:1    0    1M  0 part
├─sda2                       8:2    0    1G  0 part /boot
└─sda3                       8:3    0   39G  0 part
  ├─VolGroup00-LogVol00    253:0    0    8G  0 lvm  /
  ├─VolGroup00-LogVol01    253:1    0  1.5G  0 lvm  [SWAP]
  └─VolGroup00-LogVol_Home 253:2    0    2G  0 lvm  /home
sdb                          8:16   0   10G  0 disk
sdc                          8:32   0    2G  0 disk
├─vg_var-lv_var_rmeta_0    253:3    0    4M  0 lvm
│ └─vg_var-lv_var          253:7    0  952M  0 lvm  /var
└─vg_var-lv_var_rimage_0   253:4    0  952M  0 lvm
  └─vg_var-lv_var          253:7    0  952M  0 lvm  /var
sdd                          8:48   0    1G  0 disk
├─vg_var-lv_var_rmeta_1    253:5    0    4M  0 lvm
│ └─vg_var-lv_var          253:7    0  952M  0 lvm  /var
└─vg_var-lv_var_rimage_1   253:6    0  952M  0 lvm
  └─vg_var-lv_var          253:7    0  952M  0 lvm  /var
sde                          8:64   0    1G  0 disk
