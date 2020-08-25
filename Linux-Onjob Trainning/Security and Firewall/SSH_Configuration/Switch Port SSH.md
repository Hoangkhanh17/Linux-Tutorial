# Đổi port SSH tăng tính bảo mật cho CentOS 7

Thông thường port mặc định để chúng ta truy cập vào hệ thống là `port 22`. Điều này sẽ khiến cho hệ thống của chúng ta dễ bị quét và bị tấn công từ SSH.

Để tăng tính bảo mật cho hệ thống, bài viết này sẽ hướng dẫn cách đổi `port 22` mặc định của SSH sang port khác.

## Bước 1: Disable SElinux

Kiểm tra trạng thái hiện tại của SElinux:

`sestatus`

Nếu thấy SElinux đã bị tắt thì chuyển sang bước tiếp theo, nếu SElinux vẫn chạy ta tiến hành thay đổi cấu hình cho Selinux:

`vi /etc/selinux/config`

Đổi dòng `SELINUX=enforcing` thành `SELINUX=disabled`

## Bước 2: Thay đổi cấu hình của SSH

Chúng ta tiến hành truy cập vào tệp cấu hình của SSH bằng cách:

`vi /etc/ssh/sshd_config`

Tìm đến dòng `#Port 22`, bỏ dấu `#` và thay đổi giá trị `22` thành một giá trị khác, ở đây ta sẽ đặt là `2234`

<img src="https://imgur.com/6Pg1V6w.png">

## Bước 3: Cho phép port SSH mới thông qua firewall

```
firewall-cmd --add-port=3456/tcp --permanent
success
firewall-cmd --reload
success
```

Kiểm tra hoạt động của port mới thay đổi:

`netstat -ltnp`

<img src="https://imgur.com/fXBlmXN.png">

Sau khi thay đổi giá trị xong, ta khởi động lại SSH:

`systemctl restart sshd`


## Bước 4: Kiếm tra đăng nhập qua SSH

Kiểm tra đăng nhập vào `port 22` mặc định xem sự thay đổi:

<img src="https://imgur.com/xrbtdy1.png">

<img src="https://imgur.com/LY0hvQa.png">

Thông báo không thể SSH được.

Thay đổi sang `port 2234` để đăng nhập:

<img src="https://imgur.com/Xh6SjZu.png">

<img src="https://imgur.com/l2Pbkw2.png">

## Tài liệu tham khảo

https://kifarunix.com/how-to-configure-ssh-to-use-a-different-port-on-centos-7/