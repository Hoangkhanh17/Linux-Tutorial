# Cấu hình Virtual Host và Cài Let's Encrypt cho Nginx trên CentOS 7

## Mục lục

[1. Cấu hình Virtual Host](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob-Trainning/Nginx/Virtualhost-and-letsencrypt.md#1-c%E1%BA%A5u-h%C3%ACnh-virtual-host)

- [Bước 1: Tạo 2 thư mục cho 2 tên miền](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob-Trainning/Nginx/Virtualhost-and-letsencrypt.md#b%C6%B0%E1%BB%9Bc-1-t%E1%BA%A1o-2-th%C6%B0-m%E1%BB%A5c-cho-2-t%C3%AAn-mi%E1%BB%81n)

- [Bước 2: Tạo Virtual Host cho nginx](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob-Trainning/Nginx/Virtualhost-and-letsencrypt.md#b%C6%B0%E1%BB%9Bc-2-t%E1%BA%A1o-virtual-host-cho-nginx)

[2. Cài đặt SSL cho nginx Virtual hosts](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob-Trainning/Nginx/Virtualhost-and-letsencrypt.md#2-c%C3%A0i-%C4%91%E1%BA%B7t-ssl-cho-nginx-virtual-hosts)

- [Bước 1: Cài đặt Certbot](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob-Trainning/Nginx/Virtualhost-and-letsencrypt.md#b%C6%B0%E1%BB%9Bc-1-c%C3%A0i-%C4%91%E1%BA%B7t-certbot)

- [Bước 2: Tải về Certbot script](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob-Trainning/Nginx/Virtualhost-and-letsencrypt.md#b%C6%B0%E1%BB%9Bc-2-t%E1%BA%A3i-v%E1%BB%81-certbot-script)

- [Bước 3: Chuyển file certbot-auto mới tải về vào thư mục /usr/local/bin và cấp quyền cho file certbot-auto](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob-Trainning/Nginx/Virtualhost-and-letsencrypt.md#b%C6%B0%E1%BB%9Bc-3-chuy%E1%BB%83n-file-certbot-auto-m%E1%BB%9Bi-t%E1%BA%A3i-v%E1%BB%81-v%C3%A0o-th%C6%B0-m%E1%BB%A5c-usrlocalbin-v%C3%A0-c%E1%BA%A5p-quy%E1%BB%81n-cho-file-certbot-auto)

- [Bước 4: Thiết lập chứng chỉ miễn phí Let's Encrypt](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob-Trainning/Nginx/Virtualhost-and-letsencrypt.md#b%C6%B0%E1%BB%9Bc-4-thi%E1%BA%BFt-l%E1%BA%ADp-ch%E1%BB%A9ng-ch%E1%BB%89-mi%E1%BB%85n-ph%C3%AD-lets-encrypt)

- [Bước 5: Cấu hình Redirect cho tất cả các truy vấn tới HTTPS](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob-Trainning/Nginx/Virtualhost-and-letsencrypt.md#b%C6%B0%E1%BB%9Bc-5-c%E1%BA%A5u-h%C3%ACnh-redirect-cho-t%E1%BA%A5t-c%E1%BA%A3-c%C3%A1c-truy-v%E1%BA%A5n-t%E1%BB%9Bi-https)

[Tài liệu tham khảo](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob-Trainning/Nginx/Virtualhost-and-letsencrypt.md#t%C3%A0i-li%E1%BB%87u-tham-kh%E1%BA%A3o)

## 1. Cấu hình Virtual Host

### Bước 1: Tạo 2 thư mục cho 2 tên miền

```
mkdir -p /var/www/qatest.xyz/{public_html,logs,backup}
mkdir -p /var/www/congphan.xyz/{public_html,logs,backup}
```

Câu lệnh trên tạo ra 3 thư mục cùng lúc publich_html, logs, backup trong đường dẫn. Trong đó:

Trong đó: 

- public_html: bạn sẽ để sourcecode của trang web vào đây.

- logs: để chứa log của trang web.

- backup: chứa các file backup định kỳ hoặc các file cấu hình.

Phân quyền cho `nginx` quản lý thư mục:

`chown -R nginx:nginx /var/www`

### Bước 2: Tạo Virtual Host cho nginx

Ta tạo 2 file cấu hình để phục vụ cho việc tạo Virtual Host

```
vi /etc/nginx/conf.d/congphan.xyz.conf
vi /etc/nginx/conf.d/qatest.xyz.conf
```

Sau đó thêm cấu hình vào từng file như sau. Ví dụ dưới là file cấu hình của `/etc/nginx/conf.d/congphan.xyz.conf`, ta thay `congphan.xyz` bằng `qatest.xyz` cho file cấu hình `/etc/nginx/conf.d/congphan.xyz.conf`.

```
server {
    server_name  congphan.xyz www.congphan.xyz;

    root   /var/www/congphan.com/public_html;
    index index.php index.html index.htm;

    location / {
        try_files $uri $uri/ =404;
    }
    error_page 404 /404.html;
    error_page 500 502 503 504 /50x.html;

    location = /50x.html {
        root /usr/share/nginx/html;
    }

    location ~ \.php$ {
        try_files $uri =404;
        fastcgi_pass 127.0.0.1:9000;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }
}
```

Sau khi cài đặt xong ta tiến hành khởi động lại `nginx`

`systemctl restart nginx`

## 2. Cài đặt SSL cho nginx Virtual hosts

### Bước 1: Cài đặt Certbot

Certbot là một công cụ dòng lệnh miễn phí giúp đơn giản hóa quy trình lấy và gia hạn chứng chỉ SSL từ Let’s Encrypt và tự động kích hoạt HTTPS trên máy chủ.

Cài đặt các gói cần thiết:

```
yum -y install python36
yum -y install gcc mod_ssl python3-virtualenv redhat-rpm-config augeas-libs libffi-devel openssl-devel
```

### Bước 2: Tải về Certbot script

`curl -O https://dl.eff.org/certbot-auto`

### Bước 3: Chuyển file `certbot-auto` mới tải về vào thư mục `/usr/local/bin` và cấp quyền cho file `certbot-auto`

```
mv certbot-auto /usr/local/bin/certbot-auto
chmod 0755 /usr/local/bin/certbot-auto
```

### Bước 4: Thiết lập chứng chỉ miễn phí Let's Encrypt

`/usr/local/bin/certbot-auto --nginx`



```
Saving debug log to /var/log/letsencrypt/letsencrypt.log
Plugins selected: Authenticator nginx, Installer nginx
Enter email address (used for urgent renewal and security notices)
 (Enter 'c' to cancel): anhtq@nhanhoa.com.vn

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Please read the Terms of Service at
https://letsencrypt.org/documents/LE-SA-v1.2-November-15-2017.pdf. You must
agree in order to register with the ACME server at
https://acme-v02.api.letsencrypt.org/directory
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
(A)gree/(C)ancel: A

- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Would you be willing, once your first certificate is successfully issued, to
share your email address with the Electronic Frontier Foundation, a founding
partner of the Let's Encrypt project and the non-profit organization that
develops Certbot? We'd like to send you email about our work encrypting the web,
EFF news, campaigns, and ways to support digital freedom.
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
(Y)es/(N)o: Y

Which names would you like to activate HTTPS for?
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
1: congphan.xyz
2: www.congphan.xyz
3: qatest.xyz
4: www.qatest.xyz
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Select the appropriate numbers separated by commas and/or spaces, or leave input
blank to select all options shown (Enter 'c' to cancel):
```

Lúc này ta có thể lựa chọn số thứ tự tên miền muốn cài SSL hoặc ấn `Enter` để cài đặt SSL cho tất cả các tên miền.

**Lưu ý:** Ta nên cài đặt từng tên miền một để dễ dàng kiểm tra và theo dõi.

Thông báo đăng ký tên miền thành công:

```
Congratulations! You have successfully enabled https://qatest.xyz
- - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - - -
Subscribe to the EFF mailing list (email: anhtq@nhanhoa.com.vn).

IMPORTANT NOTES:
 - Congratulations! Your certificate and chain have been saved at:
   /etc/letsencrypt/live/qatest.xyz/fullchain.pem
   Your key file has been saved at:
   /etc/letsencrypt/live/qatest.xyz/privkey.pem
   Your cert will expire on 2020-11-18. To obtain a new or tweaked
   version of this certificate in the future, simply run certbot-auto
   again with the "certonly" option. To non-interactively renew *all*
   of your certificates, run "certbot-auto renew"
 - If you like Certbot, please consider supporting our work by:

   Donating to ISRG / Let's Encrypt:   https://letsencrypt.org/donate
   Donating to EFF:                    https://eff.org/donate-le

 - We were unable to subscribe you the EFF mailing list because your
   e-mail address appears to be invalid. You can try again later by
   visiting https://act.eff.org.
```

Ta quay lại file cấu hình của từng tên miền để kiểm tra:

`cat /etc/nginx/conf.d/congphan.xyz.conf`

```
server {
    server_name  congphan.xyz www.congphan.xyz;

    root   /var/www/congphan.com/public_html;
    index index.php index.html index.htm;

    location / {
        try_files $uri $uri/ =404;
    }
    error_page 404 /404.html;
    error_page 500 502 503 504 /50x.html;

    location = /50x.html {
        root /usr/share/nginx/html;
    }

    location ~ \.php$ {
        try_files $uri =404;
        fastcgi_pass 127.0.0.1:9000;
        fastcgi_index index.php;
        fastcgi_param SCRIPT_FILENAME $document_root$fastcgi_script_name;
        include fastcgi_params;
    }

    listen 443 ssl; # managed by Certbot
    ssl_certificate /etc/letsencrypt/live/www.congphan.xyz/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/www.congphan.xyz/privkey.pem; # managed by Certbot
    include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot


}
server {
    if ($host = www.congphan.xyz) {
        return 301 https://$host$request_uri;
    } # managed by Certbot


    if ($host = congphan.xyz) {
        return 301 https://$host$request_uri;
    } # managed by Certbot


    listen 80;
    server_name congphan.xyz www.congphan.xyz;
    return 404; # managed by Certbot


}
```

Kết quả cho thấy cấu hình đã được thay đổi khi được thêm cấu hình SSL vào.

### Bước 5: Cấu hình Redirect cho tất cả các truy vấn tới HTTPS

Truy cập vào file cấu hình trên và thêm cấu hình sau:

```
server {
   listen 80;
   server_name congphan.xyz www.congphan.xyz;
   return 301 https://www.congphan.xyz$request_uri;
 }
```

**Lưu ý:** Với mỗi tên miền ta chỉnh sửa lại cấu hình thêm vào ứng với tên miền đó.

Sau đó lưu lại và tiến hành `systemctl restart nginx`

Kiểm tra lại trên web đã nhận Https hay chưa:

<img src="https://imgur.com/qcmCgOu.png">

<img src="https://imgur.com/ztbssLJ.png">

Như vậy là đã cài đặt SSL thành công!

## Tài liệu tham khảo

[Cấu hình Virtual Hosts cho nginx](https://www.thuysys.com/server-vps/web-server/tao-virtual-host-tren-nginx-voi-centos-7.html)

[Cài đặt SSL cho nginx](https://news.cloud365.vn/ssl-cai-dat-ssl-mien-phi-cho-wordpress-website-voi-lets-encrypt-tren-nginx/)