^[kroot@lvm:/home/vagrant^[\[root@lvm vagrant]# lsblk
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
^[kroot@lvm:/home/vagrant^[\[root@lvm vagrant]# lvmdiskscan
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
^[kroot@lvm:/home/vagrant^[\[root@lvm vagrant]# yum install xfsdump
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * base: mirror.reconn.ru
 * extras: mirror.sale-dedic.com
 * updates: mirror.reconn.ru
Resolving Dependencies
--> Running transaction check
---> Package xfsdump.x86_64 0:3.1.7-1.el7 will be installed
--> Processing Dependency: attr >= 2.0.0 for package: xfsdump-3.1.7-1.el7.x86_64
--> Running transaction check
---> Package attr.x86_64 0:2.4.46-13.el7 will be installed
--> Finished Dependency Resolution

Dependencies Resolved

================================================================================
 Package          Arch            Version                   Repository     Size
================================================================================
Installing:
 xfsdump          x86_64          3.1.7-1.el7               base          308 k
Installing for dependencies:
 attr             x86_64          2.4.46-13.el7             base           66 k

Transaction Summary
================================================================================
Install  1 Package (+1 Dependent package)

Total download size: 374 k
Installed size: 1.1 M
Is this ok [y/d/N]: y
Downloading packages:

(2/2): xfsdump-3.1.7-1.el7 0% [                 ]  0.0 B/s |    0 B   --:-- ETA

(1/2): attr-2.4.46-13.el7.x86_64.rpm                       |  66 kB   00:01

(2/2): xfsdump-3.1.7-1.el7.x86_64.rpm                      | 308 kB   00:01
--------------------------------------------------------------------------------
Total                                              263 kB/s | 374 kB  00:01
Running transaction check
Running transaction test
Transaction test succeeded
Running transaction

  Installing : attr-2.4.46-13.el7.x86_64 [                                ] 1/2
  Installing : attr-2.4.46-13.el7.x86_64 [#                               ] 1/2
  Installing : attr-2.4.46-13.el7.x86_64 [#####                           ] 1/2
  Installing : attr-2.4.46-13.el7.x86_64 [########                        ] 1/2
  Installing : attr-2.4.46-13.el7.x86_64 [#########                       ] 1/2
  Installing : attr-2.4.46-13.el7.x86_64 [############                    ] 1/2
  Installing : attr-2.4.46-13.el7.x86_64 [#############                   ] 1/2
  Installing : attr-2.4.46-13.el7.x86_64 [################                ] 1/2
  Installing : attr-2.4.46-13.el7.x86_64 [####################            ] 1/2
  Installing : attr-2.4.46-13.el7.x86_64 [#####################           ] 1/2
  Installing : attr-2.4.46-13.el7.x86_64 [######################          ] 1/2
  Installing : attr-2.4.46-13.el7.x86_64 [#######################         ] 1/2
  Installing : attr-2.4.46-13.el7.x86_64 [########################        ] 1/2
  Installing : attr-2.4.46-13.el7.x86_64 [#########################       ] 1/2
  Installing : attr-2.4.46-13.el7.x86_64 [##########################      ] 1/2
  Installing : attr-2.4.46-13.el7.x86_64 [###########################     ] 1/2
  Installing : attr-2.4.46-13.el7.x86_64 [#############################   ] 1/2
  Installing : attr-2.4.46-13.el7.x86_64 [##############################  ] 1/2
  Installing : attr-2.4.46-13.el7.x86_64 [############################### ] 1/2
  Installing : attr-2.4.46-13.el7.x86_64                                    1/2

  Installing : xfsdump-3.1.7-1.el7.x86_64 [                               ] 2/2
  Installing : xfsdump-3.1.7-1.el7.x86_64 [#                              ] 2/2
  Installing : xfsdump-3.1.7-1.el7.x86_64 [####                           ] 2/2
  Installing : xfsdump-3.1.7-1.el7.x86_64 [######                         ] 2/2
  Installing : xfsdump-3.1.7-1.el7.x86_64 [########                       ] 2/2
  Installing : xfsdump-3.1.7-1.el7.x86_64 [#########                      ] 2/2
  Installing : xfsdump-3.1.7-1.el7.x86_64 [###########                    ] 2/2
  Installing : xfsdump-3.1.7-1.el7.x86_64 [#############                  ] 2/2
  Installing : xfsdump-3.1.7-1.el7.x86_64 [###############                ] 2/2
  Installing : xfsdump-3.1.7-1.el7.x86_64 [#################              ] 2/2
  Installing : xfsdump-3.1.7-1.el7.x86_64 [###################            ] 2/2
  Installing : xfsdump-3.1.7-1.el7.x86_64 [#####################          ] 2/2
  Installing : xfsdump-3.1.7-1.el7.x86_64 [######################         ] 2/2
  Installing : xfsdump-3.1.7-1.el7.x86_64 [#######################        ] 2/2
  Installing : xfsdump-3.1.7-1.el7.x86_64 [########################       ] 2/2
  Installing : xfsdump-3.1.7-1.el7.x86_64 [##########################     ] 2/2
  Installing : xfsdump-3.1.7-1.el7.x86_64 [###########################    ] 2/2
  Installing : xfsdump-3.1.7-1.el7.x86_64 [#############################  ] 2/2
  Installing : xfsdump-3.1.7-1.el7.x86_64 [############################## ] 2/2
  Installing : xfsdump-3.1.7-1.el7.x86_64                                   2/2

  Verifying  : attr-2.4.46-13.el7.x86_64                                    1/2

  Verifying  : xfsdump-3.1.7-1.el7.x86_64                                   2/2

Installed:
  xfsdump.x86_64 0:3.1.7-1.el7

Dependency Installed:
  attr.x86_64 0:2.4.46-13.el7

Complete!
^[kroot@lvm:/home/vagrant^[\[root@lvm vagrant]# pvcreate /dev/d^H^[[Ksbd^H^[[K^H^[[Kdb
  Physical volume "/dev/sdb" successfully created.
^[kroot@lvm:/home/vagrant^[\[root@lvm vagrant]# vgcreate vg_root /dev/sdb
  Volume group "vg_root" successfully created
^[kroot@lvm:/home/vagrant^[\[root@lvm vagrant]#  lvcreate -n lv_root -l +100%FREE /dev/vg_root
  Logical volume "lv_root" created.
^[kroot@lvm:/home/vagrant^[\[root@lvm vagrant]# lvmdiskscan
  /dev/VolGroup00/LogVol00 [     <37.47 GiB]
  /dev/VolGroup00/LogVol01 [       1.50 GiB]
  /dev/sda2                [       1.00 GiB]
  /dev/vg_root/lv_root     [     <10.00 GiB]
  /dev/sda3                [     <39.00 GiB] LVM physical volume
  /dev/sdb                 [      10.00 GiB] LVM physical volume
  /dev/sdc                 [       2.00 GiB]
  /dev/sdd                 [       1.00 GiB]
  /dev/sde                 [       1.00 GiB]
  4 disks
  3 partitions
  1 LVM physical volume whole disk
  1 LVM physical volume
^[kroot@lvm:/home/vagrant^[\[root@lvm vagrant]# mkfs.xfs /dev/vg_root/lv_root
meta-data=/dev/vg_root/lv_root   isize=512    agcount=4, agsize=655104 blks
         =                       sectsz=512   attr=2, projid32bit=1
         =                       crc=1        finobt=0, sparse=0
data     =                       bsize=4096   blocks=2620416, imaxpct=25
         =                       sunit=0      swidth=0 blks
naming   =version 2              bsize=4096   ascii-ci=0 ftype=1
log      =internal log           bsize=4096   blocks=2560, version=2
         =                       sectsz=512   sunit=0 blks, lazy-count=1
realtime =none                   extsz=4096   blocks=0, rtextents=0
^[kroot@lvm:/home/vagrant^[\[root@lvm vagrant]# mount /dev/vg_root/lv_root /mnt

^[kroot@lvm:/home/vagrant^[\[root@lvm vagrant]# lsblk
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
^[kroot@lvm:/home/vagrant^[\[root@lvm vagrant]#
^[kroot@lvm:/home/vagrant^[\[root@lvm vagrant]# lvremove /dev/VolGroup00/LogVol$
Do you really want to remove active logical volume VolGroup00/LogVol00? [y/n]: y
  Logical volume "LogVol00" successfully removed
^[kroot@lvm:/home/vagrant^[\[root@lvm vagrant]# lvcreate -n VolGroup00/LogVol00$
WARNING: xfs signature detected on /dev/VolGroup00/LogVol00 at offset 0. Wipe i$
  Wiping xfs signature on /dev/VolGroup00/LogVol00.
  Logical volume "LogVol00" created.
^[kroot@lvm:/home/vagrant^[\[root@lvm vagrant]# mkfs.xfs /dev/VolGroup00/LogVol$
meta-data=/dev/VolGroup00/LogVol00 isize=512    agcount=4, agsize=524288 blks
         =                       sectsz=512   attr=2, projid32bit=1
         =                       crc=1        finobt=0, sparse=0
data     =                       bsize=4096   blocks=2097152, imaxpct=25
         =                       sunit=0      swidth=0 blks
naming   =version 2              bsize=4096   ascii-ci=0 ftype=1
log      =internal log           bsize=4096   blocks=2560, version=2
         =                       sectsz=512   sunit=0 blks, lazy-count=1
realtime =none                   extsz=4096   blocks=0, rtextents=0
^[kroot@lvm:/home/vagrant^[\[root@lvm vagrant]# mount /dev/VolGroup00/LogVol00 $
^[kroot@lvm:/home/vagrant^[\[root@lvm vagrant]# xfsdump -J - /dev/vg_root/lv_ro$
xfsdump: using file dump (drive_simple) strategy
xfsdump: version 3.1.7 (dump format 3.0)
xfsdump: level 0 dump of lvm:/
xfsdump: dump date: Sun Mar  3 12:08:54 2019
xfsdump: session id: 312e08cd-9a0f-4809-b7ec-5ea7df68b05c
xfsdump: session label: ""
xfsrestore: using file dump (drive_simple) strategy
xfsrestore: version 3.1.7 (dump format 3.0)
xfsrestore: searching media for dump
xfsdump: ino map phase 1: constructing initial dump list
xfsdump: ino map phase 2: skipping (no pruning necessary)
xfsdump: ino map phase 3: skipping (only one dump stream)
xfsdump: ino map construction complete
xfsdump: estimated dump size: 732319296 bytes
xfsdump: creating dump session media file 0 (media 0, file 0)
xfsdump: dumping ino map
xfsdump: dumping directories
xfsrestore: examining media file 0
xfsrestore: dump description:
xfsrestore: hostname: lvm
xfsrestore: mount point: /
xfsrestore: volume: /dev/mapper/vg_root-lv_root
xfsrestore: session time: Sun Mar  3 12:08:54 2019
xfsrestore: level: 0
xfsrestore: session label: ""
xfsrestore: media label: ""
xfsrestore: file system id: f4750918-9347-4ffc-a650-b6082bbcf9a8
xfsrestore: session id: 312e08cd-9a0f-4809-b7ec-5ea7df68b05c
xfsrestore: media id: ef2e2620-7356-4404-b17e-f5217e0b47e0
xfsrestore: searching media for directory dump
xfsrestore: reading directories
xfsdump: dumping non-directory files
xfsrestore: 2785 directories and 23851 entries processed
xfsrestore: directory post-processing
xfsrestore: restoring non-directory files
xfsdump: ending media file
xfsdump: media file size 709173592 bytes
xfsdump: dump size (non-dir files) : 695857224 bytes
xfsdump: dump complete: 31 seconds elapsed
xfsdump: Dump Status: SUCCESS
xfsrestore: restore complete: 32 seconds elapsed
xfsrestore: Restore Status: SUCCESS
^[kroot@lvm:/home/vagrant^[\[root@lvm vagrant]# for i in /proc/ /sys/ /dev/ /ru$
 /mnt/$i; done
^[kroot@lvm:/home/vagrant^[\[root@lvm vagrant]# chroot /mnt/
^[kroot@lvm:/^[\[root@lvm /]# grub2-mkconfig -o /boot/grub2/grub.cfg
Generating grub configuration file ...
Found linux image: /boot/vmlinuz-3.10.0-862.2.3.el7.x86_64
Found initrd image: /boot/initramfs-3.10.0-862.2.3.el7.x86_64.img
done
^[kroot@lvm:/^[\[root@lvm /]# cd /boot ; for i in `ls initramfs-*img`; do dracu$
sed "s/initramfs-//g; s/.img//g"` --force; done
Executing: /sbin/dracut -v initramfs-3.10.0-862.2.3.el7.x86_64.img 3.10.0-862.2$
dracut module 'busybox' will not be installed, because command 'busybox' could $
dracut module 'crypt' will not be installed, because command 'cryptsetup' could$
dracut module 'dmraid' will not be installed, because command 'dmraid' could no$
dracut module 'dmsquash-live-ntfs' will not be installed, because command 'ntfs$
dracut module 'multipath' will not be installed, because command 'multipath' co$
dracut module 'busybox' will not be installed, because command 'busybox' could $
dracut module 'crypt' will not be installed, because command 'cryptsetup' could$
dracut module 'dmraid' will not be installed, because command 'dmraid' could no$
dracut module 'dmsquash-live-ntfs' will not be installed, because command 'ntfs$
dracut module 'multipath' will not be installed, because command 'multipath' co$
*** Including module: bash ***
*** Including module: nss-softokn ***
*** Including module: i18n ***
*** Including module: drm ***
*** Including module: plymouth ***
*** Including module: dm ***
Skipping udev rule: 64-device-mapper.rules
Skipping udev rule: 60-persistent-storage-dm.rules
Skipping udev rule: 55-dm.rules
*** Including module: kernel-modules ***
Omitting driver floppy
*** Including module: lvm ***
Skipping udev rule: 64-device-mapper.rules
Skipping udev rule: 56-lvm.rules
Skipping udev rule: 60-persistent-storage-lvm.rules
*** Including module: qemu ***
*** Including module: resume ***
*** Including module: rootfs-block ***
*** Including module: terminfo ***
*** Including module: udev-rules ***
Skipping udev rule: 40-redhat-cpu-hotplug.rules
Skipping udev rule: 91-permissions.rules
*** Including module: biosdevname ***
*** Including module: systemd ***
*** Including module: usrmount ***
*** Including module: base ***
*** Including module: fs-lib ***
*** Including module: shutdown ***
*** Including modules done ***
*** Installing kernel module dependencies and firmware ***
*** Installing kernel module dependencies and firmware done ***
*** Resolving executable dependencies ***
*** Resolving executable dependencies done***
*** Hardlinking files ***
*** Hardlinking files done ***
*** Stripping files ***
*** Stripping files done ***
*** Generating early-microcode cpio image contents ***
*** No early-microcode cpio image needed ***
*** Store current command line parameters ***
*** Creating image file ***
*** Creating image file done ***
*** Creating initramfs image file '/boot/initramfs-3.10.0-862.2.3.el7.x86_64.im$
^[kroot@lvm:/boot^[\[root@lvm boot]# pvcreate /dev/sdc /dev/sdd
  Physical volume "/dev/sdc" successfully created.
  Physical volume "/dev/sdd" successfully created.
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
^[kroot@lvm:/boot^[\[root@lvm boot]# vgcreate vg_var /dev/sdc /dev/sdd
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
