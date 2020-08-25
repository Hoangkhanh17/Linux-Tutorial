# Tìm hiểu về TCP Wrappers trên Linux

## Mục lục

[1. TCP Wrappers là gì ?](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob-Trainning/Security-and-Firewall/TCP-Wrappers.md#1-tcp-wrappers-l%C3%A0-g%C3%AC-)

[2.Cách cài đặt TCP Wrappers](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob-Trainning/Security-and-Firewall/TCP-Wrappers.md#2c%C3%A1ch-c%C3%A0i-%C4%91%E1%BA%B7t-tcp-wrappers)

- [2.1. Cách tiếp cận được đề xuất để bảo mật máy chủ](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob-Trainning/Security-and-Firewall/TCP-Wrappers.md#21-c%C3%A1ch-ti%E1%BA%BFp-c%E1%BA%ADn-%C4%91%C6%B0%E1%BB%A3c-%C4%91%E1%BB%81-xu%E1%BA%A5t-%C4%91%E1%BB%83-b%E1%BA%A3o-m%E1%BA%ADt-m%C3%A1y-ch%E1%BB%A7)

[Tài liệu tham khảo](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob-Trainning/Security-and-Firewall/TCP-Wrappers.md#t%C3%A0i-li%E1%BB%87u-tham-kh%E1%BA%A3o)

## 1. TCP Wrappers là gì ?

TCP Wrappers xuất hiện vào những năm 1990 trên các máy trạm Unix để bảo khỏi các cuộc tấn công mạng. TCP Wrappers là danh sách kiểm soát truy cập **Access list Control (ACL)**. Nó cho phép bạn từ chối và cho phép các kết nối dựa trên các đặc điểm của yêu cầu kết nối như địa chỉ IP hoặc tên máy chủ.

Ưu điểm của TCP Wrappers: So với tường lửa thông thường, TCP Wrappers hoạt động trong tầng 7 **Application** cho nên có thể lọc các truy vấn ngay cả khi sử dụng mã hóa.

## 2.Cách cài đặt TCP Wrappers

TCP Wrappers được cài đặt sẵn trên Ubuntu 18.04 LTS. Để cài đặt trên các hệ điều hành khác cũng khá dễ dàng.

- Cài đặt trên hệ điều hành Fedora/CentOS/RHEL:

`yum -y install tcp_wrappers`

- Cài đặt trên hệ điều hành Ubuntu/Debian:

`sudo apt-get install tcp_wrappers`

Sau khi cài đặt xong, TCP Wrappers thực hiện kiểm soát truy cập bằng file cấu hình: `/etc/hosts.allow` và `/etc/hosts.deny`. Hai tệp danh sách kiểm soát truy cập này quyết định liệu các máy khách cụ thể có được phép truy cập máy chủ Linux của bạn hay không.

- /etc/hosts.allow : Tệp này chứa tên của các máy chủ được phép sử dụng các dịch vụ mạng.

- /etc/hosts.deny : Tệp này chứa tên của máy chủ không thể sử dụng dịch vụ mạng.

- Nếu cùng một máy khách, người dùng hoặc ip được liệt kê trong cả hai tệp, hosts.allow có mức độ ưu tiên hơn `hosts.deny`.

Cú pháp sử dụng tệp `host.allow` và `host.deny`:

`list_of_service : list_of _ client [ : lệnh _ shell ]`

Trong đó:

- `service_list` là danh sáng các tiến trình ta muốn cho phép hoặc cấm truy cập.

- `client_list` là danh sách tên máy chủ, địa chỉ IP, mẫu đặc biệt hoặc ký tự đại diện sẽ được so sánh với từng máy khách được kết nối.

### 2.1. Cách tiếp cận được đề xuất để bảo mật máy chủ

Để bảo mật máy chủ Linux là chặn tất cả các kết nối và chỉ cho phép một vài máy chủ hoặc các mạng cụ thể. Để thực hiện, ta chỉnh sửa tại tệp `/etc/hosts.deny`

`sudo vi /etc/hosts.deny`

Ta thêm một dòng `ALL: ALL` để từ chối kết nối tất cả các dịch vụ và tất cả các mạng.

Thêm mạng cụ thể để kết nối, ta thêm chỉnh sửa tại `/etc/hosts.allow`:

`sshd : 192.168.184.140` 

Theo quy tắc trên, tất cả các kết nối đến sẽ bị từ chối cho tất cả các máy chủ ngoại trừ máy chủ 192.168.184.140 với giao thức SSH.

Ta có thể theo dõi `log` của máy chủ Linux tại `/var/log/secure`.


## Tài liệu tham khảo

https://news.cloud365.vn/tcp-wrappers-chung-la-gi-va-chung-duoc-su-dung-nhu-the-nao-trong-linux/

https://quantrimang.com/cach-bao-mat-may-chu-ssh-167262