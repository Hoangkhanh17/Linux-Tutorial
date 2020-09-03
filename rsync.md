# Tìm hiểu về công cụ Rsync

## Mục lục

[1. Rsync là gì?](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/rsync.md#1-rsync-l%C3%A0-g%C3%AC-)

[2. Cài đặt Rsync](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/rsync.md#2-c%C3%A0i-%C4%91%E1%BA%B7t-rsync)

[3. Cách sử dụng Rsync](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/rsync.md#3-c%C3%A1ch-s%E1%BB%AD-d%E1%BB%A5ng-rsync)

- [3.1. Copy file và thư mục giữa các Server](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/rsync.md#31-copy-file-v%C3%A0-th%C6%B0-m%E1%BB%A5c-gi%E1%BB%AFa-c%C3%A1c-server)

- [3.2. Rsync qua SSH](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/rsync.md#32-rsync-qua-ssh)

[Tài liệu tham khảo](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/rsync.md#t%C3%A0i-li%E1%BB%87u-tham-kh%E1%BA%A3o)

## 1. Rsync là gì ?

**Rsync (Remote Sync)** là một công cụ dùng để sao chép và đồng bộ file/thư mục được dùng rất phổ biến. Với sự trợ giúp của rsync, bạn có thể đồng bộ dữ liệu trên local hoặc giữa các server với nhau một cách dễ dàng.

Tính năng của Rsync:

- Rsync hỗ trợ copy giữ nguyên thông số của files/folder như Symbolic links, Permissions, TimeStamp, Owner và Group.

- Rsync nhanh hơn scp vì Rsync sử dụng giao thức remote-update, chỉ transfer những dữ liệu thay đổi mà thôi.

- Rsync tiết kiệm băng thông do sử dụng phương pháp nén và giải nén khi transfer.

- Rsync không yêu cầu quyền super-user.

## 2. Cài đặt Rsync

- Trên Red Hat/CentOS:

`yum install rsync -y`

- Trên Debian/Ubuntu:

`apt-get install rsysnc`

## 3. Cách sử dụng Rsync

Câu lệnh cơ bản:

`rsync options source destination`

Trong đó:

- Source: dữ liệu nguồn

- Destination: dữ liệu đích

- Options: một số tùy chọn thêm

Các tham số cần biết khi dùng Rsync:

- -v: hiển thị trạng thái kết quả

- -r: copy dữ liệu recursively, nhưng không đảm bảo thông số của file và thư mục

- -a: cho phép copy dữ liệu recursively, đồng thời giữ nguyên được tất cả các thông số của thư mục và file

- -z: nén dữ liệu khi transfer, tiết kiệm băng thông tuy nhiên tốn thêm một chút thời gian

- -h: human-readable, output kết quả dễ đọc

- --delete: xóa dữ liệu ở destination nếu source không tồn tại dữ liệu đó.

- --exclude: loại trừ ra những dữ liệu không muốn truyền đi, nếu bạn cần loại ra nhiều file hoặc folder ở nhiều đường dẫn khác nhau thì mỗi cái bạn phải thêm --exclude tương ứng.

Rsync không tự động chạy nên thường được dùng kết hợp với [crontab](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Crontab.md)

Khi lần đầu chạy rsync, toàn bộ dữ liệu nguồn sẽ được copy đến server đích, từ lần chạy sau trở đi chỉ những dữ liệu chưa được copy mới được transfer – đây là quá trình đồng bộ dữ liệu. Do đó, bạn có thể hiểu rsync thực hiện việc copy hoặc đồng bộ đều đúng.

### 3.1. Copy file và thư mục giữa các Server

- Copy thư mục từ Local lên Remote Server:

`rsync -avz rpmpkgs/ root@192.168.0.101:/home/`

Lệnh trên copy thư mục `rpmpkgs` từ Local lên Remote Server có IP `192.168.0.101`, lưu ở thư mục `/home/`.

- Copy thư mục từ Remote Server về Local

`rsync -avzh root@192.168.0.100:/home/tarunika/rpmpkgs /tmp/myrpms`

Lệnh trên sẽ copy dữ liệu ở thư mục `/home/tarunika/rpmpkgs` trên Remote Server `192.168.0.100` về máy Local lưu ở thư mục `/tmp/myrpms`

### 3.2. Rsync qua SSH

Với Rsync, bạn có thể transfer qua giao thức SSH, qua đó dữ liệu được bảo mật an toàn hơn.

- Copy file từ Remote Server về Local Server qua SSH:

Để xác định giao thức sẽ sử dụng với rsync, bạn cần thêm tùy chọn `-e` cùng với tên giao thức, ở đây là `ssh`.

`rsync -avzhe ssh root@192.168.0.100:/root/install.log /tmp/`

Lệnh trên copy file `/root/install.log` trên Remote Server `192.168.0.100` về thư mục `/tmp/` trên máy Local.

- Copy file từ Local lên Remote Server qua SSH:

`rsync -avzhe ssh backup.tar root@192.168.0.100:/backups/`

**Lưu ý**: Nếu sử dụng port SSH custom, không phải port tiêu chuẩn 22, bạn cần chỉ rõ port muốn dùng trong câu lệnh. Ví dụ port `2222`:

`rsync -avzhe "ssh -p 2222" root@192.168.0.100:/root/install.log /tmp/`

## Tài liệu tham khảo

https://hocvps.com/rsync/

https://viblo.asia/p/rsync-command-dong-bo-du-lieu-tren-linux-djeZ1R7YlWz