---
layout: post
title:  "CentOS 7 - Tăng dung lượng /dev/mapper/centos-root"
date:   2024-06-10
categories: [CentOS, CentOS 7, Linux, Disk, Size]
---

## Tài liệu

- [Sờ tách overflow](https://serverfault.com/questions/938268/how-to-increase-the-size-for-dev-mapper-centos-root){:target="_blank"}.

## Các bước

The first thing is to check if you have free extents in your volume group, to do that, you will use:

`vgdisplay`

which will return details on the VG, the important line you must check is the one that states Free PE / Size. There you will see the size available to create or extend logical volumes. For instance in my case I have a server that says:
```
Free PE / Size           3834 / 14.98 GiB
```
Given that you have the required free space you should use:

`lvextend /dev/mapper/centos-root -L +2G`

In the latter case I am extending the logical volume adding *2GB*. Note the +, if you give only the size, it will go to the specified size, I usually use this syntax because it is more transparent with the space you have available in the volume group.

After you successfully extended the volume (check with `lvscan`), you have to extend the file system, you can use:

`resize2fs /dev/mapper/centos-root`

As mentioned by @tinmarino and @y-melo in the comments. The command above will only work for ext2, ext3 and ext4. For xfs you should use `xfs_growfs /dev/mapper/centos-root`

Run `df` again to check that the available space has changed.

###
What if there's no space in the VG?

You have to first extend the volume group to be able to extend the logical volumes. For this matter you have to add a new disk. I am assuming that the CentOS box is a virtual machine because of the size of the disk, but of course this can be done on a physical server too, it is just that you have to physically add a disk.

Once you have the disk on the server, you have to create an LVM physical volume (PV), this can be created on a partition or even on the disk, I don't know the pros of doing it on the disk, but in my experience I have found it confusing as you won't be able to see a partition table, so I would recommend creating a partition first.

To create the PV over disk **`/dev/vdb'** partition 1 you do:

`pvcreate /dev/vdb1`

Once you have the PV, extend the VG (I don't know the name, I bet it is centos, check on your vgdisplay):

`vgextend centos /dev/vdb1`

###
TL;DR
For VG: vg0, LV:lv0 and new disk **/dev/sdb**. Extending 5GB

Check available space on the VG: `vgdisplay`. If enough go to 4

If you don't have space add a disk and create a PV: pvcreate /dev/sdb1

Extend the VG: `vgextend vg0 /dev/sdb1`

Extend the LV: `lvextend /dev/vg0/lv0 -L +5G`

Check: `lvscan`

Resize the file system: `resize2fs /dev/vg0/lv0` (or `xfs_growfs /dev/vg0/lv0`)

Check: `df -h | grep lv0`

