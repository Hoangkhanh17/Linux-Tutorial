# Phân vùng Swap trong Linux

## Mục lục

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

## Phần 2. Thêm/xóa một phân vùng Swap vào Ubuntu/CentOS

### 1. Thêm một phân vùng Swap

Để thêm một phân vùng Swap vào hệ thống, ta cần tài khoản root hoặc một user có quyền sudo.

#### Bước 1. Trước khi bắt đầu, hãy kiểm tra xem hệ thống đã được kích hoạt sử dụng Swap chưa:

```

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
