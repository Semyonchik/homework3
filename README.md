<b>Уменьшение тома под /</b>
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
    mkfs.xfs /dev/vg_root/lv_root
    xfsdump -J - /dev/VolGroup00/LogVol00 | xfsrestore -J - /mnt

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

Уменьшил исходный том до 8Gb. Удалил его и создал с новыми параметрами. Создал на нем ФС, смонтировал и перекинул обратно все файлы. 

    lvremove /dev/VolGroup00/LogVol$
    lvcreate -n VolGroup00/LogVol00$
    mkfs.xfs /dev/VolGroup00/LogVol$
    mount /dev/VolGroup00/LogVol00 $
    xfsdump -J - /dev/vg_root/lv_ro$
    
Далее повторил все действия для изменения загрузчика.

	mount --bind /proc /mnt/proc
	mount --bind /dev /mnt/dev
	mount --bind /sys /mnt/sys
	mount --bind /run /mnt/run
	mount --bind /boot /mnt/boot
	chroot /mnt/
	grub2-mkconfig -o /boot/grub2/grub.cfg
	cd /boot ; for i in `ls initramfs-*img`; do dracut -v $i `echo $i|sed "s/initramfs-//g;s/.img//g"` --force; done

<b>Создание LVM raid1 для /var</b>
Из двух дисков собрал LVM raid1.

	pvcreate /dev/sdc /dev/sdd
	vgcreate vg_var /dev/sdc /dev/sdd
	lvcreate -L 950M -m1 -n lv_var vg_var

Создал на массиве ФС ext4.

	mkfs.ext4 /dev/vg_var/lv_var

Примонтировал массив в папку /mnt, скопировал туда содержимое /var, очистил /var и перемонтировал рейд как /var.

	mount /dev/vg_var/lv_var /mnt
	cp -aR /var/* /mnt/ # rsync -avHPSAX /var/$
	rm -fr /var/*
	umount /mnt
	mount /dev/vg_var/lv_var /var

Добавил в fstab автомонтирование /var.

	 echo "`blkid | grep var: | awk '{print $2}'` /var ext4 defaults 0 0" >> /etc/fstab
	 
После перезагрузки удалил временный том.

	lvremove /dev/vg_root/lv_root
	vgremove /dev/vg_root
	pvremove /dev/sdb

<b>Создание LVM тома под /var</b>
На VolGroup00 создал том для /home, создал ФС и примонтировал в /mnt.

	lvcreate -n LogVol_Home -L 2G /dev/VolGroup00
	mkfs.xfs /dev/VolGroup00/LogVol_Home
	mount /dev/VolGroup00/LogVol_Home /mnt/
	
Скопировал содержимое /home в /mnt, очистил /home и перемонтировал новый том в /home.

	cp -aR /home/* /mnt/
	rm -rf /home/*
	umount /mnt
	mount /dev/VolGroup00/LogVol_Home /home/
	
Добавил в fstab монтирование /home

	echo "`blkid | grep Home | awk '{print $2}'` /home xfs defaults 0 0" >> /etc/fstab

<b>Создание спапшота и восстановление</b>
Создал в /home/ файлы

	touch /home/file{1..25}
	
Снял снапшот

	lvcreate -L 100MB -s -n home_snap /dev/VolGroup00/LogVol_Home

Удалил несколько файлов

	rm -f /home/file{9..21}
	
Восстановил из снапшота.

	umount /home
	lvconvert --merge /dev/VolGroup00/home_snap
	mount /home

<details> <summary>Вывод lsblk после всех действий:</summary>
	
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

</details>
