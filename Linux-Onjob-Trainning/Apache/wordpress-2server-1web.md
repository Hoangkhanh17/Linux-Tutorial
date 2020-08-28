# Cài đặt tách WebServer và MySQL Server cho WordPress

## Mô hình

- Một máy chạy CentOS 7 cài đặt Aphache làm WebServer và cài đặt WordPress. Địa chỉ IP:

    - ens33: 172.16.2.184 có tác dụng làm địa chỉ để truy cập WordPress.

    - ens37: 192.168.20.132 NAT với card mạng MySQL Server.

- Một máy chạy CentOS 7 cài đặt MariaDB làm Máy chủ Database: Địa chỉ IP 192.168.20.132.

### Cấu hình máy chủ

<img src="https://imgur.com/25oYDt7.png">

### Mô hình mạng

<img src="https://imgur.com/iy7li4M.png">

## 1. Cài đặt MariaDB cho DB Server

#### Bước 1: Cài đặt MariaDB và sau đó khởi động:

`yum -y install mariadb`

`systemctl start mariadb`

#### Bước 2: Cài đặt Mật khẩu để bảo mật cho MariaDB

`mysql_secure_installation`

Tại đây các bước chọn (Y) và đặt mật khẩu cho MariaDB

#### Bước 3: Khởi tạo DB cho Wordpress

`mysql -u root -p`

Đăng nhập vào DB. Tiến hành tạo DB:

```
> CREATE DATABASE wordpress;

> CREATE USER admin@192.168.20.132 IDENTIFIED BY 'Admin@123';

> GRANT ALL PRIVILEGES ON wordpress.* to admin@192.168.20.132 IDENTIFIED BY 'Admin@123';

> FLUSH PRIVILEGES;

> EXIT;
```

**Chú ý**: Ta đây ta tạo user cho địa chỉ 192.168.20.132 chính là địa chỉ IP của WebServer cài Apache.

#### Bước 4: Khởi động lại MariaDB

`systemctl restart mariadb`

## 2. Cài đặt Apache và php cho WebServer

### 2.1. Cài đặt Apache

`yum -y install httpd`

```
systemctl start httpd

systemctl enable httpd
```

### 2.1. Cài đặt PHP

Ta nên cài đặt các bản PHP 7.x

#### Bước 1: Cài đặt REMI Repository:

```
yum install -y epel-release yum-utils

yum install -y http://rpms.remirepo.net/enterprise/remi-release-7.rpm
```

#### Bước 2: Cài đặt PHP 7.2

```
yum-config-manager --enable remi-php72

yum install -y php php-common php-opcache php-mcrypt php-cli php-gd php-curl php-mysqlnd php-xml
```

#### Bước 3: Khởi động lại Apache

`systemctl restart httpd`

## 3. Cài đặt WordPress trên WebServer

#### Bước 1: Tải WordPress

Trước tiên, ta truy cập vào thư mục /var/ww/html/ sau đó tiến hành download WordPres từ internet vào thư mục này để tránh việc phải sao chép lại thư mục wordpress vào đây.

```
cd /var/www/html

wget https://wordpress.org/latest.tar.gz
```

Giải nén WordPress:

`tar xzvf latest.tar.gz`

#### Bước 2: Di chuyển toàn bộ file cấu hình của WordPress vào /var/www/html

`mv wordpress/* /var/www/html/`

#### Bước 3: Truy cập vào thư mục WordPress vừa di chuyển để sao chép cấu hình

```
cd /var/www/html/

mv wp-config-sample.php wp-config.php
```

#### Bước 4: Thay đổi cấu hình cho Wordpress

`vi /var/www/html/wp-config.php`

Ở đây có một số thay đổi:

```
// ** MySQL settings - You can get this info from your web host ** //
/** The name of the database for WordPress */
define( 'DB_NAME', 'wordpress' );

/** MySQL database username */
define( 'DB_USER', 'admin' );

/** MySQL database password */
define( 'DB_PASSWORD', 'Admin@123' );

/** MySQL hostname */
define( 'DB_HOST', '192.168.20.131' );

/** Database Charset to use in creating database tables. */
define( 'DB_CHARSET', 'utf8' );

/** The Database Collate type. Don't change this if in doubt. */
define( 'DB_COLLATE', '' );
```

Trong đó bao gồm:

- DB_NAME: Tên DB ta vừa tạo ở trên là `wordpress`.

- DB_USER: `admin`.

- DB_PASSWORD: `Admin@123`.

- DB_HOST: Tại đây ta trỏ về IP của DB Server là 192.168.20.131.

#### Bước 5: Lưu cấu hình lại và khởi động lại httpd:

`systemcl restart httpd`

## Kiểm tra lại kết quả

Truy cập vào địa chỉ IP của WebServer: 172.16.2.184

<img src="https://imgur.com/9KlUHGT.png">

### Chúc các bạn thành công !

## Tài liệu tham khảo

https://news.cloud365.vn/cai-dat-wordpress-tren-2-server-bang-centos-7/