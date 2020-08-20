# Cấu hình Virtual cho Apache chạy nhiều trang Web CentOS7

## 1. Cài đặt Aphache

#### Bước 1: Cập nhật lại máy chủ CentOS 7

`# yum -y update`

#### Bước 2: Cài đặt Apache và khởi động cùng Hệ thống

`# yum -y install httpd`

```
# systemctl start httpd
# systemctl enable httpd
```

## 2. Cấu hình Virtual Host trên Apache

Để chạy nhiều website trên máy chủ, ta phải tạo Virtual Hosts cho Apache.

### 2.1. Tạo thư mục và phân quyền

Tạo thư mục đặt tên miền mà ta muốn sử dụng để dễ dàng quản lý, ở đây ta sẽ tạo 2 thư mục cho 2 tên miền là `qatest.xyz` và `congphan.xyz`

```
# mkdir -p /var/www/qatest.xyz/public_html
# mkdir -p /var/www/qatest.xyz/logs
# mkdir -p /var/www/qatest.xyz/backup
```

```
# mkdir -p /var/www/congphan.xyz/public_html
# mkdir -p /var/www/congphan.xyz/logs
# mkdir -p /var/www/congphan.xyz/backup
```

Trong đó: 

- public_html: bạn sẽ để sourcecode của trang web vào đây.

- logs: để chứa log của trang web.

- backup: chứa các file backup định kỳ hoặc các file cấu hình.

Gán quyền truy cập cho Apache vào các thư mục trên để khi chạy tránh gặp lỗi

```
# chown -R apache:apache /var/www/qatest.xyz/
# chown -R apache:apache /var/www/congphan.xyz/
```

### 2.2. Tạo Virtual Hosts cho Apache

#### Bước 1: Thay đổi cấu hình cho Apache

Để tạo Virtual Hosts, ta cần khai báo Virtual Hosts vào file cấu hình của Apache tại đường dẫn `vi /etc/httpd/conf/httpd.conf`.

Thêm dòng `NameVirtualHost *:80` vào cuối file cấu hình và lưu lại.

#### Bước 2: Tạo cấu hình Virtual Hosts

Tạo một file để lưu cấu hình Virtual Hosts bao gồm 2 thư mục 2 trang Web ta tạo bên trên.

`# vi /etc/httpd/conf.d/vhost.conf`

```
<VirtualHost *:80>
ServerAdmin anhtq@nhanhoa.com.vn
ServerName qatest.xyz
ServerAlias www.qatest.xyz
DocumentRoot /var/www/qatest.xyz/public_html/
ErrorLog /var/www/qatest.xyz/logs/error.log
CustomLog /var/www/qatest.xyz/logs/access.log combined
</VirtualHost>

<VirtualHost *:80>
ServerAdmin congpv@nhanhoa.com.vn
ServerName congphanxyz
ServerAlias www.congphan.xyz
DocumentRoot /var/www/congphantest.com/public_html/
ErrorLog /var/www/congphantest.com/logs/error.log
CustomLog /var/www/congphantest.com/logs/access.log combined
</VirtualHost>
```

Trong đó:

- VirtualHost *:80: port dùng cho trang web có domain được mô tả trong ServerAlias.

- DocumentRoot: đường dẫn đến thư mục chứa sourcecode.

- ServerAdmin: đơn giản chỉ là mail quản trị viện.

- ServerName: root domain của virtual host được người dùng gõ trên trình duyệt.

- ServerAlias: tên gọi khác của root domain, còn được dùng để cấu hình www và non www .

- ErrorLog: ghi lại log các lỗi phát sinh.

- CustomLog: ghi lại log các truy cập.

Sau đó khởi động lại Apache

`# systemctl restart httpd`

#### Bước 3: Tạo một file html để kiểm tra lại cấu hình

Ta tạo cho mỗi thư mục một file riêng để kiểm tra

`# vi /var/www/qatest.xyz/public_html/index.html`

`# vi /var/www/congphan.xyz/public_html/index.html`

```
<html>
 <head>
 <title>Welcome</title>
 </head>
 <body>
 <h1>Cau hinh LAMP VirtualHost thanh cong</h1>
 </body>
</html>
```

Sau đó lưu lại và restart Apache.

#### Bước 4: Kiểm tra cấu hình Apache bằng cách truy cập vào 2 trang web vừa tạo

```
[root@srv-thuctapanh ~]# curl -I http://qatest.xyz
HTTP/1.1 200 OK
Date: Thu, 20 Aug 2020 04:08:11 GMT
Server: Apache/2.4.6 (CentOS)
Last-Modified: Thu, 20 Aug 2020 01:53:07 GMT
ETag: "78-5ad4561be2fe0"
Accept-Ranges: bytes
Content-Length: 120
Content-Type: text/html; charset=UTF-8

[root@srv-thuctapanh ~]# curl -I http://congphan.xyz
HTTP/1.1 200 OK
Date: Thu, 20 Aug 2020 04:08:19 GMT
Server: Apache/2.4.6 (CentOS)
Last-Modified: Thu, 20 Aug 2020 01:53:07 GMT
ETag: "78-5ad4561be2fe0"
Accept-Ranges: bytes
Content-Length: 120
Content-Type: text/html; charset=UTF-8
```