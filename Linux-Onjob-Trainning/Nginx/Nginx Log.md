# Tìm hiểu về cấu hình Log trên Nginx Webserver

## Mục lục

[1. Log trên nginx](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob%20Trainning/Nginx/Nginx%20Log.md#1-log-tr%C3%AAn-nginx)

[2. Access Log](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob%20Trainning/Nginx/Nginx%20Log.md#2-access-log)

[3. Error log](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob%20Trainning/Nginx/Nginx%20Log.md#3-error-log)

[Tài liệu tham khảo](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob%20Trainning/Nginx/Nginx%20Log.md#t%C3%A0i-li%E1%BB%87u-tham-kh%E1%BA%A3o)

## 1. Log trên nginx

Trên 1 Webserver gồm có 2 loại Log chính: `access log` và `error log`.

- `access log` ghi lại các thông tin người dùng truy cập vào website.

- `error log` ghi lại các cảnh báo các lỗi xảy ra với dịch vụ liên quan web server.

Theo mặc định Nginx Server lưu file log tại thư mục `/var/log/nginx`

```
[root@srv-thuctapanh ~]# ls /var/log/nginx/
access.log  error.log
```

## 2. Access Log

Log này rất cần thiết đặc biệt khi website bị tấn công DDOS, nhờ nó ta có thể xác định được nguồn tần công, tần xuất và quy mô để có action phù hợp như chặn theo User Agent hay IP nguồn. Nginx cung cấp cho bạn khả năng tùy biến access log để xuất ra file theo định dạng mà bạn muốn.

Để thay đổi định dạng access log bạn chỉ cần làm việc với directive log_format, chỉ thị này mặc định nằm trong block http {…}. Để xem được log format bạn vào `/etc/nginx/nginx.conf` áp dụng cho cả hệ điều hành CentOS và Ubuntu. Mẫu log_format trong nginx:

```
log_format main '$remote_addr - $remote_user [$time_local] "$request" '
 '$status $body_bytes_sent "$http_referer" '
 '"$http_user_agent" "$http_x_forwarded_for"';
```

Chỉ thị `log_format` để định dạng `access log` cho nginx, ý nghĩa các biến như sau:

- main tên gọi của log format, bạn có thể tạo nhiều định dạng log khác nhau rồi gán cho nó một cái tên bất kỳ cũng được.

- $remote_addr địa chỉ IP truy cập web site của bạn.

- $remote_user ghi lại tài khoản truy cập web nếu trang của bạn có xác thực người dùng, đa số là không dùng bạn có thể bỏ đi.

- $time_local thời gian người dùng truy cập.

- $request đoạn đầu của request.

- $status trạng thái của response.

- $body_bytes_sent kích thước body mà server response.

- $http_referer URL được tham chiếu.

- $http_user_agent thông tin trình duyệt, hệ điều hành mà người dùng truy cập.

- $http_x_forwarded_for được ghi vào log nếu webserver detect người dùng truy cập qua proxy server.

## 3. Error log

Log này để ghi lại thông tin các lỗi cài đặt cấu hình hay đơn giản chỉ là những cảnh báo giữa Web Server Nginx và các dịch vụ Nginx.

`Error log` bao gồm `log_file` và `log_level`.

- `log_file` xác định đường dẫn của file log.

- `log_level` xác định mức độ nghiêm trọng của file log được ghi lại. Nếu ta không chỉ định `log_level` thì theo mặc định, hỉ ghi lại các sự kiện nhật ký có mức độ lỗi nghiêm trọng. Ví dụ:

```
http {
       	  ...
	  error_log  /var/log/nginx/error_log  crit;
	  ...
}
```

Có nhiều loại `log levels` bao gồm:

- `emerg log` ở level này mang tính khẩn cấp, dạng như server sắp sập đến nơi rồi.

- `alert` cảnh báo các vấn cần cần được xử lý ngay.

- `crit` các vấn đề quan trong nhưng không nhất thiết phải xử lý ngay lập tức, để theo dõi thêm.

- `error` ghi lại thông tin lỗi như đăng nhập hoặc cấu hình sai, mức độ thấp hơn crit.

- `warn` ở mức độ cảnh báo không phải lỗi.

- `notice` để thông báo cái gì đó.

- `info` ghi thông tin hệ thống, không có gì cả.

- `debug` ghi lại tất cả mọi thứ, dùng để dò lỗi.

Ở level debug, info, notice, warn thường ghi ra rất nhiều thông tin có thể không cần thiết. Ta nên để ở level emerg, alert, crit và error bởi vì nó ghi lại gần như tất cả các vấn đề ta cần để vận hành nginx rồi.

## Tài liệu tham khảo

https://www.thuysys.com/toi-uu/tim-hieu-cau-hinh-log-tren-webserver-nginx.html

https://www.journaldev.com/26756/nginx-access-logs-error-logs#what-is-nginx-access-log

http://nginx.org/en/docs/http/ngx_http_log_module.html#access_log