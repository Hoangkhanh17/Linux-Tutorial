# Tạo tài khoản User được cấp quyền sudo và tạo khóa SSH key

## Mục lục

- [1. Tạo User với quyền sudo](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob-Trainning/Security-and-Firewall/SSH-Configuration/create-root-user.md#1-t%E1%BA%A1o-user-v%E1%BB%9Bi-quy%E1%BB%81n-sudo)

- [2. Tắt tính năng SSH cho user root](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob-Trainning/Security-and-Firewall/SSH-Configuration/Configure-SSH-keypair.md#12-t%E1%BA%AFt-t%C3%ADnh-n%C4%83ng-ssh-cho-user-root)

## Tạo User được cấp quyền sudo và Tắt tính năng SSH cho User root

### 1. Tạo User với quyền sudo

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

### 2. Tắt tính năng SSH cho user root

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


## Chúc các bạn thành công