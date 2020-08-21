# Tìm hiểu về cấu hình Log trên Nginx Webserver

## Log trên nginx

Trên 1 Webserver gồm có 2 loại Log chính: `access log` và `error log`.

- `access log` ghi lại các thông tin người dùng truy cập vào website.

- `error log` ghi lại các cảnh báo các lỗi xảy ra với dịch vụ liên quan web server.

Theo mặc định Nginx Server lưu file log tại thư mục `/var/log/nginx`

```
[root@srv-thuctapanh ~]# ls /var/log/nginx/
access.log  error.log
```

## Access Log

Log này rất cần thiết đặc biệt khi website bị tấn công DDOS, nhờ nó ta có thể xác định được nguồn tần công, tần xuất và quy mô để có action phù hợp như chặn theo User Agent hay IP nguồn. Nginx cung cấp cho bạn khả năng tùy biến access log để xuất ra file theo định dạng mà bạn muốn.

