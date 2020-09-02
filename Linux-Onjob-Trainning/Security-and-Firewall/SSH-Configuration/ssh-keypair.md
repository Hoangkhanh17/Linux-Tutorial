Cấu hình SSH Key Pair cho Linux

## Mục lục

[1. Mô hình hoạt động](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob-Trainning/Security-and-Firewall/SSH-Configuration/ssh-keypair.md#1-m%C3%B4-h%C3%ACnh-ho%E1%BA%A1t-%C4%91%E1%BB%99ng)

[2. Các bước cấu hình](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob-Trainning/Security-and-Firewall/SSH-Configuration/ssh-keypair.md#2-c%C3%A1c-b%C6%B0%E1%BB%9Bc-c%E1%BA%A5u-h%C3%ACnh)

- [2.1. Cấu hình trên máy Master](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob-Trainning/Security-and-Firewall/SSH-Configuration/ssh-keypair.md#21-c%E1%BA%A5u-h%C3%ACnh-tr%C3%AAn-m%C3%A1y-master)

- [2.2. Cấu hình trên máy Slave](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob-Trainning/Security-and-Firewall/SSH-Configuration/ssh-keypair.md#22-c%E1%BA%A5u-h%C3%ACnh-tr%C3%AAn-m%C3%A1y-slave)

[3. Kiểm tra lại cấu hình](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob-Trainning/Security-and-Firewall/SSH-Configuration/ssh-keypair.md#3-ki%E1%BB%83m-tra-l%E1%BA%A1i-c%E1%BA%A5u-h%C3%ACnh)

[4. Cấu hình Disable đăng nhập bằng Password cho các máy Slave](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob-Trainning/Security-and-Firewall/SSH-Configuration/ssh-keypair.md#4-c%E1%BA%A5u-h%C3%ACnh-disable-%C4%91%C4%83ng-nh%E1%BA%ADp-b%E1%BA%B1ng-password-cho-c%C3%A1c-m%C3%A1y-slave)

[Tài liệu tham khảo](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob-Trainning/Security-and-Firewall/SSH-Configuration/ssh-keypair.md#t%C3%A0i-li%E1%BB%87u-tham-kh%E1%BA%A3o)

## 1. Mô hình hoạt động

<img src="https://imgur.com/MhwzU0q.png">

Sử dụng mô hình bao gồm 1 Máy **Master** chạy CentOS 7 và các máy **Slave** sử dụng các hệ điều hành Linux. Mô hình mô tả việc chỉ có máy **Master** mới có khả năng SSH được vào các máy **Slave**.

## 2. Các bước cấu hình

### 2.1. Cấu hình trên máy Master

#### Bước 1: Khởi tạo SSH Key pair

Ta sử dụng lệnh:

`ssh-keygen -t rsa`

Nếu ta muốn tăng thêm tính bảo mật, ta có thể tạo khóa **4096-bit** bằng lệnh:

`ssh-keygen -t rsa -b 4096`

#### Bước 2: Lưu khóa SSH vừa tạo

Sau khi sử dụng lệnh trên, một thông báo sẽ hiện lên để thông báo thư mục lưu khóa SSH vừa khởi tạo:

```
Generating public/private rsa key pair.
Enter file in which to save the key (/root/.ssh/id_rsa):
```

Ấn Enter để lưu vào thư mục mặc định.

#### Bước 3: Tạo mật khẩu Passphrase cho khóa SSH

```
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
```

#### Bước 4: Thông báo khóa Key được tạo thành công

```
Your identification has been saved in home/root/.ssh/id_rsa.
Your public key has been saved in home/root/.ssh/id_rsa.pub.
The key fingerprint is:
KYg355:gKotTeU5NQ-5m296q55Ji57F8iO6c0K6GUr5:PO1iRk
root@hostname
The key's randomart image is:
+------[RSA 3072]-------+
|       .oo.            |
|        +o+.           |
|      + +.+            |
| o  +          S .     |
|      .    E  .   . =.o|
|    .  +       .   B+@o|
|        +   .     oo*=O|
|   oo            . .+o+|
|                 o=ooo=|
+------ [SHA256] ------+
```

Ta sẽ sử dụng Public Key để lưu sang cho các máy **Slave** tại thư mục `home/root/.ssh/id_rsa.pub` (Lưu ý: Thư mục sẽ thay đổi tùy vào tài khoản mà ta muốn tạo khóa. Ta có thể tạo khóa cho User có quyền root. Tham khảo cách tạo User Root [tại đây](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob-Trainning/Security-and-Firewall/SSH-Configuration/create-root-user.md))

### 2.2. Cấu hình trên máy Slave

#### Bước 1: Tìm thông tin khóa Public Key trên máy Master

Để xem thông tin khóa Public Key, ta sử dụng lệnh:

`cat home/root/.ssh/id_rsa.pub`

Ta sao chép thông tin trong tệp tin này để chuẩn bị chuyển sang cho các máy **Slave**.

#### Bước 2: Truy cập vào máy Slave

Ví dụ: Truy cập vào máy **Slave** có địa chỉ IP 192.168.1.140. Ta sử dụng MobaXterm hoặc Putty để truy cập.

#### Bước 3: Khởi tạo thư mục SSH và tệp tin lưu trữ khóa Public key

`mkdir -p ~/.ssh && touch ~/.ssh/authorized_keys`

#### Bước 4: Phân quyền cho các thư mục và tệp tin trên

`chmod 700 ~/.ssh && chmod 600 ~/.ssh/authorized_keys`

#### Bước 5: Truy cập vào tên tin vừa tạo và sao chép thông tin khóa Public Key

`vi ~/.ssh/authorized_keys`

Ta sao chép thông tin trong tệp tại Bước 1.

Tại đây ta đã hoàn thành việc cấu hình để truy cập bằng Public Key.

## 3. Kiểm tra lại cấu hình

- Trên máy **Master**, ta tiến hành truy cập vào SSH vào máy **Slave**

`ssh root@192.168.1.140`

- Thông báo đăng nhập SSH lần đầu tiên:

```
The authenticity of host '192.168.1.140 (192.168.1.140)' can't be established.
ECDSA key fingerprint is fd:fd:d4:f9:77:fe:73:84:e1:55:00:ad:d6:6d:22:fe.
Are you sure you want to continue connecting (yes/no)?
```

Ta gõ `yes` để truy cập vào máy **Slave**

- Thông báo đăng nhập bằng Passphrase:

`Enter passphrase for key '/root/.ssh/id_rsa':`

Sau khi gõ Passphrase thành công ta đã thành công việc đăng nhập bằng Public Key.

## 4. Cấu hình Disable đăng nhập bằng Password cho các máy Slave

Việc tắt đăng nhập bằng Password sẽ tăng khả năng bảo mật cho các máy **Slave**. Không những thế, nếu không tắt đăng nhập bằng Password, ta vẫn có thể đăng nhập SSH vào các máy **Slave** bằng Password mặc dù đã cấu hình cài đặt đăng nhập bằng SSH Key pair ở trên.

- Truy cập vào cấu hình của SSH:

`vi /etc/ssh/sshd_config`

Tìm đến dòng `PasswordAuthentication yes` sau đó sửa giá trị `yes` thành `no`.

- Khởi động lại SSH:

`systemctl restart sshd`

- Kiểm tra đăng nhập bằng Password, ta sẽ thấy thông báo đăng nhập đòi Key Pair:

<img src="https://imgur.com/2dVmwdR.png">

- Kiểm tra đăng nhập thông qua máy **Master**:

<img src="https://imgur.com/VCyCjk1.png">

### Chúc các bạn thành công !

Các máy **Slave** khác cũng cấu hình tương tự.

## Tài liệu tham khảo

https://phoenixnap.com/kb/how-to-generate-ssh-key-centos-7

https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys-on-centos7