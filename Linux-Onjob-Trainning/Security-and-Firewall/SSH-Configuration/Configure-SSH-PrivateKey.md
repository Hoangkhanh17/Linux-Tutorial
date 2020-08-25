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

### 2.1. Tạo khóa SSH key

Để tạo 1 SSH key, ta sử dụng câu lệnh

`ssh-keygen -t rsa`

Tại đây sẽ hiện ra thông báo gồm ba phần:

- Nơi lưu trữ key, ta có thể thay đổi hoặc ấn `enter` để lưu trữ vào thư mục mặc định.

- Nhập chuỗi mật khẩu để tăng tính bảo mật. Có thể sử dụng mật khẩu này để đăng nhập nếu không có mật khẩu của user.

- Nhập lại chuỗi mật khẩu ở trên.

Thông báo khởi tạo SSH key thành công:

```
[quanganh@localhost ~]$ ssh-keygen -t rsa
Generating public/private rsa key pair.
Enter file in which to save the key (/home/quanganh/.ssh/id_rsa):
Created directory '/home/quanganh/.ssh'.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /home/quanganh/.ssh/id_rsa.
Your public key has been saved in /home/quanganh/.ssh/id_rsa.pub.
The key fingerprint is:
SHA256:Ws52Kqokn7zYmdyhNLcJDTbwCnfG0hOwbZZCo3pn5NQ quanganh@localhost.localdomain
The key's randomart image is:
+---[RSA 2048]----+
|  +              |
| o = o           |
|o o O E          |
|.o X .           |
|o O X   S        |
|.= X . =         |
|o = + . + .      |
| X X +.. o       |
|. %o=. ..        |
+----[SHA256]-----+
```

Di chuyển và phân quyền key vừa tạo

- Di chuyển key đến vị trí mặc định của nó là `/.ssh/authorized_keys`

`mv /home/quanganh/.ssh/id_rsa.pub /home/quanganh/.ssh/authorized_keys`

- Phân quyền cho các thư mục và tệp tin chứa key:

```
chmod 600 /home/thuctap/.ssh/authorized_keys

chmod 700 .ssh
```

- Chỉnh sửa cấu hình SSH để khai báo key tại `/etc/ssh/sshd_config`

```
sudo sed -i 's|#LoginGraceTime 2m|LoginGraceTime 2m|' /etc/ssh/sshd_config

sudo sed -i 's|#StrictModes yes|StrictModes yes|' /etc/ssh/sshd_config

sudo sed -i 's|#PubkeyAuthentication yes|PubkeyAuthentication yes|' /etc/ssh/sshd_config
```

- Để tắt tính năng đăng nhập bằng mật khẩu mà chỉ đăng nhập được bằng SSH key, ta tiếp tục vào tệp tin cấu hình `/etc/ssh/sshd_config` và chỉnh sửa dòng `PasswordAuthentication yes` thành `no`.

- Khởi động lại dịch vụ SSH

`systemctl restart sshd`

### 2.2. Cài đặt SSH cho Client với public key

- Truy cập vào tệp tin chứa SSH Key, copy toàn bộ nội dung trong tệp.

`cat /home/quanganh/.ssh/id_rsat`

- Mở `Notepad` hoặc `Notepad++` và copy toàn bộ nội dung ở tệp trên vào sau đó lưu lại với phần mở rộng là `.ppk`. Tệp tin có đuôi `.ppk` là tệp tin có chức năng key.

- Mở ứng dụng **MobaXterm** để tiến hành tạo key SSH. **Menu -> Tools -> MobaKeyGen (SSH key generator)** để vào tính năng tạo key

- Chọn **Load** và trỏ đến tệp tin đuôi `.ppk` ta vừa tạo ở trên:

<img src="https://imgur.com/YsqBcis.png">

- Nhập chuỗi mật khẩu khi mà ta tạo SSH key từ ban đầu

<img src="https://imgur.com/wnbmE53.png">

- Chọn **Save private key** để lưu lại khóa key ta vừa tạo.

<img src="https://imgur.com/zLL36eS.png">

### 2.3. Kiểm tra đăng nhập bằng SSH

- Kiểm tra bằng SSH thông thường với mật khẩu, lúc này sẽ hiện lên thông báo không đăng nhập được như ảnh:

<img src="https://imgur.com/vcnr7Sl.png">

- Đăng nhập SSH bằng key vừa tạo, chọn `Use private key` và trỏ đến đường dẫn thư mục chứa Key

<img src="https://imgur.com/ahdH1hc.png">

- Lúc này sẽ hiện ra thông báo đăng nhập bằng chuỗi mật khẩu khi ta tạo SSH key ban đầu, nhập chuỗi mật khẩu này sẽ đăng nhập sẽ đăng nhập thành công.

<img src="https://imgur.com/tHb9BkP.png">

<img src="https://imgur.com/dNXtNnM.png">

## Kết luận

Việc đổi `port SSH` sẽ tăng tính bảo mật lên so với `port 22` thông thường. Tuy nhiên, hiện nay có rất nhiều cách thức để truy cập vào hệ thống. Chúng ta vẫn nên sử dụng những mật khẩu có độ phức tạp cao cũng như sử dụng các tường lửa để tăng thêm tính bảo mật cho hệ thống.

## Tài liệu tham khảo

https://news.cloud365.vn/ssh-phan1-huong-dan-tao-user-sudo-va-cau-hinh-disable-ssh-doi-voi-user-root/

https://news.cloud365.vn/ssh-phan-2-cau-hinh-dang-nhap-ssh-key-cho-user-tren-centos-7/

https://docs.vhost.vn/article/h%C6%B0%E1%BB%9Bng-d%E1%BA%ABn-t%E1%BA%A1o-v%C3%A0-s%E1%BB%AD-d%E1%BB%A5ng-ssh-key-450.html