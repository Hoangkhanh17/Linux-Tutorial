# Phân vùng Swap trong Linux

## Mục lục

[Phần 1. Tìm hiểu về phân vùng Swap](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob%20Trainning/Ph%C3%A2n%20v%C3%B9ng%20swap.md#ph%E1%BA%A7n-1-t%C3%ACm-hi%E1%BB%83u-v%E1%BB%81-ph%C3%A2n-v%C3%B9ng-swap)

- [1. Phân vùng Swap là gì ?](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob%20Trainning/Ph%C3%A2n%20v%C3%B9ng%20swap.md#1-ph%C3%A2n-v%C3%B9ng-swap-l%C3%A0-g%C3%AC-)

- [2. Công dụng của Swap](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob%20Trainning/Ph%C3%A2n%20v%C3%B9ng%20swap.md#2-c%C3%B4ng-d%E1%BB%A5ng-c%E1%BB%A7a-swap)

- [3. Kích thước cho phân vùng Swap](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob%20Trainning/Ph%C3%A2n%20v%C3%B9ng%20swap.md#3-k%C3%ADch-th%C6%B0%E1%BB%9Bc-cho-ph%C3%A2n-v%C3%B9ng-swap)

[Phần 2. Thêm/xóa, thay đổi dung lượng một phân vùng Swap vào Ubuntu/CentOS](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob%20Trainning/Ph%C3%A2n%20v%C3%B9ng%20swap.md#ph%E1%BA%A7n-2-th%C3%AAmx%C3%B3a-m%E1%BB%99t-ph%C3%A2n-v%C3%B9ng-swap-v%C3%A0o-ubuntucentos)

- [1. Thêm một phân vùng Swap](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob%20Trainning/Ph%C3%A2n%20v%C3%B9ng%20swap.md#1-th%C3%AAm-m%E1%BB%99t-ph%C3%A2n-v%C3%B9ng-swap)

- [2. Xóa một phân vùng Swap](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob%20Trainning/Ph%C3%A2n%20v%C3%B9ng%20swap.md#2-x%C3%B3a-m%E1%BB%99t-ph%C3%A2n-v%C3%B9ng-swap)

- [3. Thay đổi dung lượng phân vùng /Swap](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob%20Trainning/Ph%C3%A2n%20v%C3%B9ng%20swap.md#3-thay-%C4%91%E1%BB%95i-dung-l%C6%B0%E1%BB%A3ng-ph%C3%A2n-v%C3%B9ng-swap)

## Phần 1. Tìm hiểu về phân vùng Swap

### 1. Phân vùng Swap là gì ?

**Swap** hay còn được gọi là **RAM** ảo được sử dụng để hỗ trợ lưu trữ dữ liệu khi **bộ nhớ vật lý (RAM)** đã đầy. Đôi khi **SWAP** cũng được dùng song song để tăng dung lượng bộ nhớ đệm.

Trường hợp đầy **RAM**, **Swap** sẽ được hệ thống dùng làm bộ nhớ thay thế. Tuy nhiên, tốc độ của nó chậm hơn rất nhiều so với ổ cứng vật lý.

### 2. Công dụng của Swap

Một trong những trường hợp quan trọng cần đến **Swap** là khi **RAM** đầy. Theo đó, **Swap** sẽ hạn chế các sự cố liên quan đến vấn đề bảo mật thông tin, nhất là trong hệ thống điều hành Linux.

**Swap** sẽ làm nhiệm vụ duy trì tất cả các hoạt động bình thường dù tốc độ có phần chậm hơn thay vì dừng cả hệ thống khiến thông tin dễ bị rò rỉ.

**Swap** được sử dụng khi:

- Khi dùng một phần mềm yêu cầu hệ thống có hỗ trợ bộ nhớ **Swap** trong phần cài đặt.

- Khi muốn hoạt động của hệ thống ổn định hơn, đặc biệt quan trọng đối với các hệ thống không có nhiều dung lượng RAM.

- Nếu đang dùng HĐH Ubuntu, hệ điều hành này sẽ yêu cầu Swap cho chế độ ngủ đông.

### 3. Kích thước cho phân vùng Swap

Việc cài đặt **Swap** là biện pháp dự phòng khi **RAM** bất ngờ bị hết. Cho nên chỉ cần cài **Swap** với dung lượng tối đa bằng một nửa dung lượng **RAM** thật.

Nếu phân vùng **Swap** được sử dụng quá nhiều, điều đó cảnh báo **VPS** hoặc **Server** cần nâng cấp **RAM** ngay lập tức.

## Phần 2. Thêm/xóa, thay đổi dung lượng một phân vùng Swap vào Ubuntu/CentOS

### 1. Thêm một phân vùng Swap

Để thêm một phân vùng Swap vào hệ thống, ta cần tài khoản root hoặc một user có quyền sudo.

#### Bước 1. Kiểm tra hệ thống đã được kích hoạt sử dụng Swap chưa

```
[root@localhost ~]# swapon -s
[root@localhost ~]# free -m
                total        used        free      shared  buff/cache   available
Mem:            972         166         664           7         141         663
Swap:             0           0           0
```

#### Bước 2. Tạo kích thước cho /swap

```
[root@localhost ~]# fallocate -l 512M /swapfile
```

512M là dung lượng tạo cho /swap, chúng ta có thể tùy chọn thay đổi theo mong muốn.

Nếu lệnh ``fallocate`` không có sẵn và báo lỗi **fallocate failed: Operation not supported**, ta sử dụng lệnh sau:

```
[root@localhost ~]# sudo dd if=/dev/zero of=/swapfile bs=1024 count=512
```
Có thể thay đổi dung lượng /swap tại `count=512` với dung lượng mong muốn.

Sau khi tạo xong, kiểm tra lại dung lượng file /swap:

```
[root@localhost ~]# ls -lh /swapfile
-rw-r--r--. 1 root root 512M Aug 13 14:48 /swapfile
```

#### Bước 3. Chỉ gắn quyền cho root có thể đọc và ghi file /swap

```
[root@localhost ~]# chmod 600 /swapfile
[root@localhost ~]# ls -lh /swapfile
-rw-------. 1 root root 512M Aug 13 14:48 /swapfile
```

#### Bước 4. Thiết lập phân vùng Swap

```
[root@localhost ~]# mkswap /swapfile
Setting up swapspace version 1, size = 524284 KiB
no label, UUID=df0ab48e-e652-4c5d-b1cf-207c60014647
```

#### Bước 5. Thiết lập swap tự động kích hoạt mỗi khi reboot

Ta cần thêm dòng lệnh `/swapfile swap swap defaults 0 0` vào `/etc/fstab`

```
[root@localhost ~]# vi /etc/fstab
[root@localhost ~]# cat /etc/fstab

#
# /etc/fstab
# Created by anaconda on Thu Aug 13 14:26:12 2020
#
# Accessible filesystems, by reference, are maintained under '/dev/disk'
# See man pages fstab(5), findfs(8), mount(8) and/or blkid(8) for more info
#
/dev/mapper/centos-root /                       ext4    defaults        1 1
UUID=fbb21271-d977-4e1e-8db8-abb96324d0fe /boot                   ext4    defaults        1 2
/swapfile swap swap defaults 0 0
```

#### Bước 6. Kiểm tra hệ thống đã báo cáo phân vùng /swap chưa

```
[root@localhost ~]# free -m
              total        used        free      shared  buff/cache   available
Mem:            972         169         659           7         142         660
Swap:             0           0           0
[root@localhost ~]# swapon -a
[root@localhost ~]# swapon -s
Filename                                Type            Size    Used    Priority
/swapfile                               file    524284  0       -2
[root@localhost ~]# free -m
              total        used        free      shared  buff/cache   available
Mem:            972         170         659           7         142         660
Swap:           511           0         511
```

### 2. Xóa một phân vùng Swap

#### Bước 1. Huỷ kích hoạt swap

```
root@ubuntu:~# swapoff /swapfile
```

#### Bước 2. Chỉnh sửa file `/etc/fstab` xoá dòng `/swapfile`

```
root@ubuntu:~# vi /etc/fstab
# /etc/fstab: static file system information.
#
# Use 'blkid' to print the universally unique identifier for a
# device; this may be used with UUID= as a more robust way to name devices
# that works even if disks are added and removed. See fstab(5).
#
# <file system> <mount point>   <type>  <options>       <dump>  <pass>
# / was on /dev/sda1 during installation
UUID=28fd8efc-aca4-4fde-a1a0-a6ded408d262 /               ext4    errors=remount-ro 0       1
/swapfile                       swap    defaults        0 0
/dev/fd0        /media/floppy0  auto    rw,user,noauto,exec,utf8 0       0
```

#### Bước 4. Xóa file **Swap**, sau đó kiểm tra lại đã thành công chưa

```
root@ubuntu:~# rm -rf /swapfile
root@ubuntu:~# free -m
               total        used        free      shared  buff/cache   available
Mem:           1969         139        1616           0         214        1678
Swap:             0           0           0
```

### 3. Thay đổi dung lượng phân vùng /Swap

#### Bước 1. Tắt phân vùng /swap

```
[root@localhost ~]# swapoff /swapfile
```

#### Bước 2. Thiết lập dung lượng mới cho /swap

```
[root@localhost ~]# sudo dd if=/dev/zero of=/swapfile bs=1M count=1024
1024+0 records in
1024+0 records out
1073741824 bytes (1.1 GB) copied, 1.48122 s, 725 MB/s
```

#### Bước 3. Thiết lập file vừa tạo thành phân vùng /swap

```
[root@localhost ~]# mkswap /swapfile
Setting up swapspace version 1, size = 1048572 KiB
no label, UUID=c4232459-abbe-4128-85bf-653a67178e80
```

#### Bước 4. Bật phân vùng /swap và kiểm tra

```
[root@localhost ~]# swapon /swapfile
[root@localhost ~]# free -m
              total        used        free      shared  buff/cache   available
Mem:            972         171          85           7         715         649
Swap:          1023           0        1023
```