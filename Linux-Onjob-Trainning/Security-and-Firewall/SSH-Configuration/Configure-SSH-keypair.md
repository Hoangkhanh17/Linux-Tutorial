# Tạo tài khoản User được cấp quyền sudo và tạo khóa SSH key

## Mục lục

[1. Tạo User được cấp quyền sudo và Tắt tính năng SSH cho User root](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob-Trainning/Security-and-Firewall/SSH-Configuration/Configure-SSH-PrivateKey.md#1-t%E1%BA%A1o-user-%C4%91%C6%B0%E1%BB%A3c-c%E1%BA%A5p-quy%E1%BB%81n-sudo-v%C3%A0-t%E1%BA%AFt-t%C3%ADnh-n%C4%83ng-ssh-cho-user-root)

- [1.1. Tạo User với quyền sudo](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob-Trainning/Security-and-Firewall/SSH-Configuration/Configure-SSH-PrivateKey.md#11-t%E1%BA%A1o-user-v%E1%BB%9Bi-quy%E1%BB%81n-sudo)

- [1.2. Tắt tính năng SSH cho user root](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob-Trainning/Security-and-Firewall/SSH-Configuration/Configure-SSH-PrivateKey.md#12-t%E1%BA%AFt-t%C3%ADnh-n%C4%83ng-ssh-cho-user-root)

[2. Tạo khóa SSH key cho user](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob-Trainning/Security-and-Firewall/SSH-Configuration/Configure-SSH-PrivateKey.md#2-t%E1%BA%A1o-kh%C3%B3a-ssh-key-cho-user)

- [2.1. Tạo khóa SSH key](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob-Trainning/Security-and-Firewall/SSH-Configuration/Configure-SSH-PrivateKey.md#21-t%E1%BA%A1o-kh%C3%B3a-ssh-key)

- [2.2. Cài đặt SSH cho Client với public key](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob-Trainning/Security-and-Firewall/SSH-Configuration/Configure-SSH-PrivateKey.md#22-c%C3%A0i-%C4%91%E1%BA%B7t-ssh-cho-client-v%E1%BB%9Bi-public-key)

- [2.3. Kiểm tra đăng nhập bằng SSH](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob-Trainning/Security-and-Firewall/SSH-Configuration/Configure-SSH-PrivateKey.md#23-ki%E1%BB%83m-tra-%C4%91%C4%83ng-nh%E1%BA%ADp-b%E1%BA%B1ng-ssh)

[Kết luận](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob-Trainning/Security-and-Firewall/SSH-Configuration/Configure-SSH-PrivateKey.md#k%E1%BA%BFt-lu%E1%BA%ADn)

[Tài liệu tham khảo](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob-Trainning/Security-and-Firewall/SSH-Configuration/Configure-SSH-PrivateKey.md#t%C3%A0i-li%E1%BB%87u-tham-kh%E1%BA%A3o)

## 1. Tạo User được cấp quyền sudo và Tắt tính năng SSH cho User root

### 1.1. Tạo User với quyền sudo

- Tạo tài khoản có tên là `quanganh`:

`adduser quanganh`

- Tạo mật khẩu cho user `quanganh`:

`passwd quanganh`

- Cấp quyền sudo cho user `quanganh`. Theo mặc định, server sẽ có 1 group có tên là wheel, các thành viên được add vào nhóm wheel mặc đính sẽ có đặc quyền sudo.

`usermod -aG wheel quanganh`

- Kiểm tra lại quyền sudo cho user `quanganh`:

```
cat /etc/group | grep quanganh
wheel:x:10:quanganh
quanganh:x:1000:
```

### 1.2. Tắt tính năng SSH cho user root

Để tắt tính năng SSH cho user `root`. Ta phải đảm bảo có một user khác có quyền sudo trước khi tắt tính năng SSH bằng user `root`. Ta đã tạo user `quanganh` có quyền sudo ở trên.

- Để vô hiệu hóa tính năng SSH với user `root`. Ta vào tệp cấu hình của ssh tại `/etc/ssh/sshd_config`

`vi /etc/ssh/sshd_config`

- Tìm đến dòng chứa nội dung `#PermitRootLogin yes` và bỏ dấu comment, chỉnh sửa `yes` thành `no`:

`PermitRootLogin no`

- Lưu cấu hình lại và restart lại SSH

`systemctl restart sshd`

- Kiểm tra lại đăng nhập SSH bằng user `root`:

<img src="https://imgur.com/mZSKA4d.png">

- Kiểm tra đăng nhập bằng user `quanganh`:

<img src="https://imgur.com/xdyUUyZ.png">

## 2. Tạo khóa SSH key cho user

Việc tạo SSH key sẽ tăng tính bảo mật cho hệ thống của bạn, sẽ tránh bị quét tài khoản `brute force password`.

### 2.1. Tạo khóa SSH Public Key

Ta nên tạo khóa Public Key để tránh Private Key của hệ thống bị truyền ra bên ngoài.

Để tạo khóa Public Key, ta sử dụng ứng dụng `MobaXterm SSH Key Generator` hoặc `PuttyGen` để tạo Public key. Trong ví dụ này chúng ta sẽ sử dụng `MobaXterm SSH Key Generator`.

#### Bước 1: Chọn Generate để tạo Public key

<img src="https://imgur.com/yYVlmAQ.png">

#### Bước 2: Sau khi Generate xong sẽ hiện lên như sau

Ta thêm Passphrase vào để tránh trường hợp chẳng may bị đánh cắp Public Key.

<img src="https://imgur.com/DJJ9U2o.png">

### 2.2. Tạo khóa SSH key cho User được tạo

Để tạo Public key cho user `quanganh` được tạo ở trên. Ta thực hiện các bước sau:

#### Bước 1: Tạo thư mục chứa Public Key trên thư mục của User và phân quyền cho thư mục

```
mkdir -p /home/quanganh/.ssh

sudo chmod 700 /home/quanganh/.ssh

touch /home/quanganh/.ssh/authorized_keys

sudo chmod 600 /home/quanganh/.ssh/authorized_keys
```

#### Bước 2: Truy cập vào tệp ta tạo ở trên và copy Public vừa tạo

Chuỗi kí tự ta sẽ copy khi tạo Public Key:

<img src="https://imgur.com/beiJI4y.png">

Truy cập vào `/home/quanganh/.ssh/authorized_keys` và copy toàn bộ Chuỗi kí tự trên.

#### Bước 3: Khởi động lại SSH

`systemctl restart sshd`

#### Bước 4: Tiến hành lưu Public Key vừa tạo

Ta chọn vào `Save Private Key` để được lưu tệp Public Key vừa tạo thành tệp có đuôi `.ppk` để dễ dàng sử dụng.

<img src="https://imgur.com/H82P6dy.png">

#### Bước 5: Truy cập vào MobaXterm để thêm Public Key

Ta tích chọn `Use Private key` và trỏ đến đường dẫn lưu tệp `.ppk` vừa lưu.

<img src="https://imgur.com/Nf60JO2.png">

Kiểm tra truy cập:

<img src="https://imgur.com/ROdWBfh.png">

Yêu cầu đăng nhập `Passphrase`, ta gõ đúng Mật khẩu `Passphrase` ta tạo ở trên là đăng nhập thành công.

## Chúc các bạn thành công