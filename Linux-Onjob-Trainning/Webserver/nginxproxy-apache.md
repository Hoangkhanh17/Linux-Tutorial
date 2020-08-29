# Cấu hình làm Nginx Reverse Poxy cho Apache Webserver

## 1. Mô hình hệ thống

<img src="https://imgur.com/qylnsBG.png">

Mô hình cài đặt LAB sẽ triển khai như sau:

- Cài đặt máy webs server sử dụng Apache, sau đó up các file ảnh hoặc tạo ra một site html đơn giản (có ảnh hoặc file js)

- Cài đặt máy Nginx làm chức năng reverse và caching.

- Cấu hình file host ở các máy của người dùng với domain `quanganh123.abc` trỏ về IP của máy chủ nginx.

- Người dùng mở trình duyệt hoặc dùng lệnh curl với tùy chọn -I để kiểm tra xem proxy có hoạt động hay không.

- Người dùng truy cập nhiều lần vào web hoặc dùng curl để kiểm tra xem đã cache được hay chưa?

## 2. Cài đặt cấu hình cho máy chủ

### 2.1. Cài đặt WebServer Apache

- Các gói cài đặt và cập nhật:

```
yum update -y

yum install -y epel-release

yum install -y wget byobu
```

- Cài đặt Apache:

`yum install -y httpd`

- Khởi động Apache và kích hoạt khởi động cùng hệ thống:

```
systemctl start httpd

systemctl enable httpd
```

- Thêm một đoạn code html và ảnh để web thêm đặc sắc:

`wget -O /var/www/html/index.html https://gist.githubusercontent.com/congto/359e04f735162a987daf58d3f8d44fb6/raw/51ccab89265bff5717084af1212640dae6bbfa92/indext.html`

- Kiểm tra truy cập trên web với địa chỉ http://172.16.2.63/ :

<img src="https://imgur.com/Jiz5lz7.png">

### 2.2. Cấu hình cài đặt Nginx

Các bước cũng tương tự như cài đặt Apache:

- Các gói cài đặt và cập nhật:

```
yum update -y

yum install -y epel-release

yum install -y wget byobu
```

- Cài đặt nginx:

`yum -y install nginx`

- Khởi động Nginx và kích hoạt khởi động cùng hệ thống:

```
systemctl start nginx

systemctl enable nginx
```

### 2.3. Cấu hình Nginx làm Proxy

Khai báo cấu hình cho Nginx có chức năng Reverse Proxy cho Webserver. Khi người dùng truy cập vào IP hoặc Domain trỏ về địa chỉ IP của Nginx, lúc này Nginx sẽ có nhiệm vụ điều phối truy cập vào máy Apache Webserver để lấy nội dung trang web.

- Sao lưu cấu hình mặc định của nginx:

`cp /etc/nginx/nginx.conf /etc/nginx/nginx.conf.bka`

- Tạo file cấu hình làm nhiệm vụ reverse proxy:

`vi /etc/nginx/conf.d/proxy.conf`

- Thêm cấu hình vào file cấu hình trên:

```
server {
    listen 80;
    server_name quanganh123.abc;
    access_log /var/log/nginx/access.log;
    error_log /var/log/nginx/error.log;

    location / {
       proxy_pass http://172.16.2.63:80/;
       # Input any other settings you may need that are not already contained in the default snippets.
    }
}
```

- Lưu ý trong dòng proxy_pass ta sẽ khai báo về địa chỉ của máy cài httpd.

- Kiểm tra lại hoạt động của nginx:

```
nginx -t

nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```

- Khởi động lại nginx:

`systemctl restart nginx`

- Tiến hành trỏ domain `quanganh123.abc` về IP của Nginx để kiểm tra:

`172.16.2.64 quanganh123.abc`

- Kiểm tra nội dung trang web, ta thấy nội dung của Webserver Apache đã hiện lên với địa chỉ IP của Nginx.

<img src="https://imgur.com/OFdMvAH.png">

### 2.4. Cấu hình Caching cho nginx

Khai báo thêm các tùy chọn để biến Nginx thành máy chủ Cache

- Tạo file cần lưu cache và gán quyền cho nginx:

```
sudo mkdir -p /var/lib/nginx/cache
sudo chown nginx /var/lib/nginx/cache
sudo chmod 700 /var/lib/nginx/cache
```

- Truy cập vào file cấu hình Nginx ở trên để thay đổi cấu hình như sau:

```
proxy_cache_path /var/lib/nginx/cache levels=1:2 keys_zone=backcache:8m max_size=50m;
proxy_cache_key "$scheme$request_method$host$request_uri$is_args$args";
proxy_cache_valid 200 302 10m;
proxy_cache_valid 404 1m;

server {
    listen 80;
    server_name quanganh123.abc;
    access_log /var/log/nginx/access.log;
    error_log /var/log/nginx/error.log;

    location / {
       proxy_cache backcache;
       add_header X-Proxy-Cache $upstream_cache_status;

       proxy_pass http://172.16.2.63:80/;
       # Input any other settings you may need that are not already contained in the default snippets.
    }
}
```

- Kiểm tra lại hoạt động của nginx:

`nginx -t`

- Nếu hoạt đông của nginx bình thường ta tiến hành khởi động lại Nginx:

`systemctl restart nginx`

- Kiểm tra lại Cache trên máy Client bằng lệnh `curl`:

<img src="https://imgur.com/ADffQmN.png">

Ta thấy dòng `X-Proxy-Cache: ` của lần lệnh `curl` đầu tiên báo `MISS` và tiếp theo báo `HIT`. Như vậy là ta đã cấu hình thành công.

- Kiểm tra lại việc truy cập bằng Web:

<img src="https://imgur.com/qOfqpLR.png">

<img src="https://imgur.com/1f7JT2R.png">

### Chúc các bạn thành công

Tài liệu tham khảo

https://gist.github.com/congto/bb0a2c37348ced72284659a86cd24052