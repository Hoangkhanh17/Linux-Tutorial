# Tìm hiểu về Logical Volume Manager - LVM

## Mục Lục

[Phần 1. Giới thiệu về LVM](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob%20Trainning/Logical%20Volume%20Manager.md#ph%E1%BA%A7n-1-gi%E1%BB%9Bi-thi%E1%BB%87u-v%E1%BB%81-lvm)

[]

## Phần 1. Giới thiệu về LVM

### 1. LVM là gì ?

LVM là một công cụ để quản lý phân vùng logic được tạo và phân bổ từ các ổ đĩa vật lý. Với LVM bạn có thể dễ dàng tạo mới, thay đổi kích thước hoặc xóa bỏ phân vùng đã tạo.

Với kỹ thuật Logical Volume Manager (LVM), ta có thể thay đổi kích thước mà không cần phải sửa lại partition table của OS. Nếu muốn mở rộng dung lượng của nó, ta chỉ cần ấn định lại dung lượng mà không cần phân vùng lại, cũng không phải đối mặt với nguy cơ mất dữ liệu khi thay đổi dung lượng như khi thao tác trên Partition.

### 2. Cấu trúc LVM

<img src="https://imgur.com/VtGksx7.png">

- **Hard Drives:** Thiết bị lưu trữ dữ liệu. 

- **Partition:** Partitions là các phân vùng của Hard drives, mỗi Hard drives có 4 partition. Có 2 loại Partition:

    - **Primary partition:** Phân vùng chính, có thể khởi động , mỗi đĩa cứng có thể có tối đa 4 phân vùng này.

    - **Extended partition:** Phân vùng mở rộng.

- **Physical Volumes:** Một ổ đĩa vật lý có thể phân chia thành nhiều phân vùng vật lý gọi là Physical Volumes.

- **Volume Group:** Là một nhóm bao gồm nhiều Physical Volume trên 1 hoặc nhiều ổ đĩa khác nhau được kết hợp lại thành một Volume Group. Volume Group được sử dụng để tạo ra các Logical Volume, trong đó người dùng có thể tạo, thay đổi kích thước, lưu trữ, gỡ bỏ và sử dụng.

<img src="https://imgur.com/gHIAFxL.png">

- **Logical Volume:** Một Volume Group được chia nhỏ thành nhiều Logical Volume. Nó được dùng để mount tới hệ thống tập tin (File System) và được format với những chuẩn định dạng khác nhau như ext2, ext3, ext4…

<img src="https://i.imgur.com/UNmyHQI.png">

## Phần 2. Tạo và quản lý LVM

### 1. Tạo LVM

#### Bước 1. Tạo Physical Volume

Để tạo Physical Volume, ta sử dụng lệnh `pvcreate`

```
[root@localhost ~]# pvcreate /dev/sdb /dev/sdc /dev/sdd
  Physical volume "/dev/sdb" successfully created.
  Physical volume "/dev/sdc" successfully created.
  Physical volume "/dev/sdd" successfully created.
```

- Chúng ta có thể liệt kê các danh sách các Physical Volume bằng lệnh `pvs`

- Để xem thông tin chi tiết cho từng Physical Volume, ta dùng lệnh `pvdisplay`. Ví dụ: `pvdisplay /dev/sdb`

#### Bước 2. Tạo Volume Group

Để tạo volume group với tên vg0 bằng cách sử dụng /dev/sdb và /dev/sdc. Chúng ta thực hiện như sau:

```
$ vgcreate new_vol_group /dev/sdd
Volume group "new_vol_group" successfully created
```

Để kiểm tra thông tin của Volume group vừa tạo, ta dùng lệnh `vgdisplay vg0`.

Ý nghĩa các thông tin của Volume group khi chạy lệnh `vgdisplay`:

- VG Name: Tên Volume Group.

- Format: Kiến trúc LVM được sử dụng.

- VG Access: Volume Group có thể đọc/ghi và sẵn sàng để sử dụng.

- VG Status: Volume Group có thể được định cỡ lại, chúng ta có thể mở rộng thêm nếu cần thêm dung lượng.

- PE Size: Mở rộng Physical, Kích thước cho đĩa có thể được xác định bằng kích thước PE hoặc GB, 4MB là kích thước PE mặc định của LVM

- Total PE: Dung lượng Volume Group có

- Alloc PE: Tổng PE đã sử dụng

- Free PE: Tổng PE chưa được sử dụng

Để kiểm tra số lượng Physical Volume dùng để tạo Volume Group, ta dùng lệnh `vgs`.

```
[root@localhost ~]# vgs
  VG   #PV #LV #SN Attr   VSize   VFree
  cl     1   2   0 wz--n- <19.00g     0
  vg0   2   0   0 wz--n-  19.99g 19.99g
```

Trong đó:

- VG: Tên Volume Group

- #PV: Physical Volume sử dụng trong Volume Group

- VFree: Hiển thị không gian trống có sẵn trong Volume Group

- VSize: Tổng kích thước của Volume Group

- #LV: Logical Volume nằm trong volume group

- #SN: Số lượng Snapshot của volume group

- Attr: Trạng thái của Volume group có thể ghi, có thể đọc, có thể thay đổi.

#### Bước 3. Tạo Logical Volume

Chúng ta sẽ tạo 2 Logical Volume với tên `projects` có dung lượng là 10GB và `backups` sử dụng toàn bộ dung lượng còn lại của volume group.

```
[root@localhost ~]# lvcreate -n projects -L 10G vg0
  Logical volume "projects" created.
[root@localhost ~]# lvcreate -n backups -l 100%FREE vg0
  Logical volume "backups" created.
```

Trong đó:

- n: Sử dụng chỉ ra tên của logical volume cần tạo.

- L: Sử dụng chỉ một kích thước cố định.

- l: Sử dụng chỉ phần trăm của không gian còn lại trong volume group.

Kiểm tra danh sách Logical Volume vừa được tạo:

```
[root@localhost ~]# lvs
  LV           VG   Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  root         cl   -wi-ao---- <17.00g
  swap         cl   -wi-a-----   2.00g
  backups      vg0  -wi-a-----   9.99g
  projects     vg0  -wi-a-----  10.00g
```

Để hiển thị thông tin chi tiết cho các Logical Volume, ta dùng lệnh `lvdisplay vg0/projects`

#### Bước 4. Tạo File System cho Logical Volume

```
[root@localhost ~]# mkfs.ext4 /dev/vg0/projects
mke2fs 1.42.9 (28-Dec-2013)
Filesystem label=
OS type: Linux
Block size=4096 (log=2)
Fragment size=4096 (log=2)
Stride=0 blocks, Stripe width=0 blocks
655360 inodes, 2621440 blocks
131072 blocks (5.00%) reserved for the super user
First data block=0
Maximum filesystem blocks=2151677952
80 block groups
32768 blocks per group, 32768 fragments per group
8192 inodes per group
Superblock backups stored on blocks:
	32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632

Allocating group tables: done
Writing inode tables: done
Creating journal (32768 blocks): done
Writing superblocks and filesystem accounting information: done

[root@localhost ~]# mkfs.ext4 /dev/vg0/backups
mke2fs 1.42.9 (28-Dec-2013)
Filesystem label=
OS type: Linux
Block size=4096 (log=2)
Fragment size=4096 (log=2)
Stride=0 blocks, Stripe width=0 blocks
655360 inodes, 2619392 blocks
130969 blocks (5.00%) reserved for the super user
First data block=0
Maximum filesystem blocks=2151677952
80 block groups
32768 blocks per group, 32768 fragments per group
8192 inodes per group
Superblock backups stored on blocks:
	32768, 98304, 163840, 229376, 294912, 819200, 884736, 1605632

Allocating group tables: done
Writing inode tables: done
Creating journal (32768 blocks): done
Writing superblocks and filesystem accounting information: done
```

### 2. Mở rộng Volume Group và thay đổi kích thước các Logical Volume

Trong ví dụ dưới đây chúng ta sẽ thêm một physical volume có tên `/dev/sdd` với kích thước 10GB vào volume group vg0, sau đó chúng ta sẽ tăng kích thước của logical volume `/projects` lên 10GB thực hiện như sau:

- Tạo điểm gắn kết:

```
[root@localhost ~]# mkdir /projects
[root@localhost ~]# mkdir /backups
```

- Mount các Logical Volume vừa tạo:

```
[root@localhost ~]# mount /dev/vg0/projects /projects/
[root@localhost ~]# mount /dev/vg0/backups /backups/
```

- Kiểm tra việc sử dụng không gian ổ đĩa

```
[root@localhost ~]# df -TH
Filesystem               Type      Size  Used Avail Use% Mounted on
/dev/mapper/cl-root      xfs        19G  1.1G   18G   6% /
devtmpfs                 devtmpfs  501M     0  501M   0% /dev
tmpfs                    tmpfs     512M     0  512M   0% /dev/shm
tmpfs                    tmpfs     512M  7.1M  505M   2% /run
tmpfs                    tmpfs     512M     0  512M   0% /sys/fs/cgroup
/dev/sda1                xfs       1.1G  145M  919M  14% /boot
tmpfs                    tmpfs     103M     0  103M   0% /run/user/0
/dev/mapper/vg0-projects ext4      11G   38M   9.9G   1% /projects
/dev/mapper/vg0-backups  ext4      11G   38M   9.9G   1% /backups
```

- Thêm `/dev/sdd` vào Volume Group vg0:

```
[root@localhost ~]# vgextend vg0 /dev/sdd
  Volume group "vg0" successfully extended
```

- Tăng kích thước cho dung lượng Logical Volume `/projects`

```
[root@localhost ~]# lvextend -l +2000 /dev/vg0/projects
  Size of logical volume vg0/projects changed from 10.00 GiB (1920 extents) to 17.81 GiB (4560 extents).
  Logical volume vg0/projects successfully resized.
```

- Sau khi chạy lệnh trên chúng ta cần thay đổi kích thước hệ thống tệp, vì thế chúng ta phải chạy lệnh sau để resize:
    
    - Đối với file system (ext2, ext3, ext 4): `resize2fs`.

    - Đối với file system (xfs): `xfs_growfs`.

```
[root@localhost ~]# resize2fs /dev/vg0/projects
resize2fs 1.42.9 (28-Dec-2013)
Filesystem at /dev/vg0/projects is mounted on /projects; on-line resizing required
old_desc_blocks = 1, new_desc_blocks = 2
The filesystem on /dev/vg0/projects is now 4669440 blocks long.

[root@localhost ~]# df -TH
Filesystem               Type      Size  Used Avail Use% Mounted on
/dev/mapper/cl-root      xfs        19G  1.1G   18G   6% /
devtmpfs                 devtmpfs  501M     0  501M   0% /dev
tmpfs                    tmpfs     512M     0  512M   0% /dev/shm
tmpfs                    tmpfs     512M  7.1M  505M   2% /run
tmpfs                    tmpfs     512M     0  512M   0% /sys/fs/cgroup
/dev/sda1                xfs       1.1G  145M  919M  14% /boot
tmpfs                    tmpfs     103M     0  103M   0% /run/user/0
/dev/mapper/vg0-projects ext4       19G   47M   18G   1% /projects
/dev/mapper/vg0-backups   ext4       11G   38M  9.9G   1% /backups
```

### 3. Giảm kích cỡ Logical Volume

Khi chúng ta muốn giảm dung lượng các Logical Volume. Chúng ta cần phải chú ý vì nó rất quan trọng và có thể bị lỗi trong khi chúng ta giảm dung lượng Logical Volume. Để đảm bảo an toàn khi giảm Logical Volume cần thực hiện các bước sau:

- Trước khi bắt đầu, cần sao lưu dữ liệu vì vậy sẽ không phải đau đầu nếu có sự cố xảy ra.

- Để giảm dung lượng Logical Volume, cần thực hiện đầy đủ và cẩn thận 5 bước cần thiết:

    - Ngắt kết nối file system.

    - Kiểm tra file system sau khi ngắt kết nối.

    - Giảm file system.

    - Giảm kích thước Logical Volume hơn kích thước hiện tại.

    - Kiểm tra lỗi cho file system.

    - Mount lại file system và kiểm tra kích thước của nó.

- Khi giảm dung lượng Logical Volume chúng ta phải ngắt kết nối hệ thống tệp trước khi giảm.

Ví dụ: Giảm Logical Volume `project` với kích thước từ 17.81GB giảm xuống còn 10GB mà không làm mất dữ liệu. Chúng ta thực hiện các bước như sau:

Kiểm tra dung lượng của Logical Volume và kiểm tra file system trước khi thực hiện giảm:

```
[root@localhost ~]# lvs
  LV       VG  Attr       LSize  Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  root     cl  -wi-ao---- 17.00g
  swap     cl  -wi-ao----  2.00g
  backups  vg0 -wi-ao---- 10.00g
  projects vg0 -wi-ao---- 17.81g
[root@localhost ~]# df -TH
Filesystem               Type      Size  Used Avail Use% Mounted on
/dev/mapper/cl-root      xfs        19G  6.8G   12G  37% /
devtmpfs                 devtmpfs  501M     0  501M   0% /dev
tmpfs                    tmpfs     512M     0  512M   0% /dev/shm
tmpfs                    tmpfs     512M  7.1M  505M   2% /run
tmpfs                    tmpfs     512M     0  512M   0% /sys/fs/cgroup
/dev/sda1                xfs       1.1G  145M  919M  14% /boot
tmpfs                    tmpfs     103M     0  103M   0% /run/user/0
/dev/mapper/vg0-projects ext4       19G  4.4G   14G  25% /projects
/dev/mapper/vg0-backups  ext4       11G  4.4G  5.6G  44% /backups
```

#### Bước 1. Unount File system

```
[root@localhost ~]# umount /projects/
[root@localhost ~]# df -TH
Filesystem              Type      Size  Used Avail Use% Mounted on
/dev/mapper/cl-root     xfs        19G  6.8G   12G  37% /
devtmpfs                devtmpfs  501M     0  501M   0% /dev
tmpfs                   tmpfs     512M     0  512M   0% /dev/shm
tmpfs                   tmpfs     512M  7.1M  505M   2% /run
tmpfs                   tmpfs     512M     0  512M   0% /sys/fs/cgroup
/dev/sda1               xfs       1.1G  145M  919M  14% /boot
tmpfs                   tmpfs     103M     0  103M   0% /run/user/0
/dev/mapper/vg0-backups ext4       11G  4.4G  5.6G  44% /backups
```

#### Bước 2. Kiểm tra lỗi file system bằng lệnh `e2fsck`

```
[root@localhost ~]# e2fsck -f /dev/vg0/projects
e2fsck 1.42.9 (28-Dec-2013)
Pass 1: Checking inodes, blocks, and sizes
Pass 2: Checking directory structure
Pass 3: Checking directory connectivity
Pass 4: Checking reference counts
Pass 5: Checking group summary information
/dev/vg0/projects: 11/1171456 files (0.0% non-contiguous), 117573/4669440 blocks
```

#### Bước 3. Giảm kích thước của `projects` theo kích thước từ 17.81GB giảm xuống còn 10GB chúng ta thực hiện như sau:

```
[root@localhost ~]# resize2fs /dev/vg0/projects 10G
resize2fs 1.42.9 (28-Dec-2013)
Resizing the filesystem on /dev/vg0/projects to 2621440 (4k) blocks.
The filesystem on /dev/vg0/projects is now 2621440 blocks long.
```

#### Bước 4. Giảm kích thước `projects` bằng lệnh `lvreduce`

```
[root@localhost ~]# lvreduce -L 10G /dev/vg0/projects
  WARNING: Reducing active logical volume to 10.00 GiB.
  THIS MAY DESTROY YOUR DATA (filesystem etc.)
Do you really want to reduce vg0/projects? [y/n]: y
  Size of logical volume vg0/projects changed from 17.81 GiB (4560 extents) to 10.00 GiB (2560 extents).
  Logical volume vg0/projects successfully resized.
```

#### Bước 5. Để đảm bảo an toàn, kiểm tra lỗi file system đã giảm

```
[root@localhost ~]# e2fsck -f /dev/vg0/projects
e2fsck 1.42.9 (28-Dec-2013)
Pass 1: Checking inodes, blocks, and sizes
Pass 2: Checking directory structure
Pass 3: Checking directory connectivity
Pass 4: Checking reference counts
Pass 5: Checking group summary information
/dev/vg0/projects: 11/655360 files (0.0% non-contiguous), 83137/2621440 blocks
```

#### Bước 6. Gắn kết file system và kiểm tra kích thước của nó.

```
[root@localhost ~]# mount /dev/vg0/projects /projects/
[root@localhost ~]# lvs
  LV       VG  Attr       LSize  Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  root     cl  -wi-ao---- 17.00g
  swap     cl  -wi-ao----  2.00g
  backups  vg0 -wi-ao---- 10.00g
  projects vg0 -wi-ao---- 10.00g
[root@localhost ~]# df -TH
Filesystem               Type      Size  Used Avail Use% Mounted on
/dev/mapper/cl-root      xfs        19G  6.8G   12G  37% /
devtmpfs                 devtmpfs  501M     0  501M   0% /dev
tmpfs                    tmpfs     512M     0  512M   0% /dev/shm
tmpfs                    tmpfs     512M  7.1M  505M   2% /run
tmpfs                    tmpfs     512M     0  512M   0% /sys/fs/cgroup
/dev/sda1                xfs       1.1G  145M  919M  14% /boot
tmpfs                    tmpfs     103M     0  103M   0% /run/user/0
/dev/mapper/vg0-backups  ext4       11G  4.4G  5.6G  44% /backups
/dev/mapper/vg0-projects ext4       11G  4.4G  5.6G  44% /projects
```

## Tài liệu tham khảo

https://news.cloud365.vn/lvm-gioi-thieu-ve-logical-volume-manager/

https://blogd.net/linux/tao-va-quan-ly-lvm-trong-linux/

https://vinasupport.com/lvm-la-gi-tao-vao-quan-ly-logical-volume-manager/

https://www.ufsexplorer.com/articles/storage-technologies/lvm-data-organization.php

https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/logical_volume_manager_administration/lvm_examples