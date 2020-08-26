# Cài đặt MySQL-MariaDB và PHP với phiên bản mong muốn

## Mục lục

[1. Cài đặt chỉ định phiên bản MySQL](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Install-MySQL-MariaDB-PHP-with-version.md#1-c%C3%A0i-%C4%91%E1%BA%B7t-ch%E1%BB%89-%C4%91%E1%BB%8Bnh-phi%C3%AAn-b%E1%BA%A3n-mysql)

[2. Cài đặt chỉ định phiên bản MariaDB](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Install-MySQL-MariaDB-PHP-with-version.md#2-c%C3%A0i-%C4%91%E1%BA%B7t-ch%E1%BB%89-%C4%91%E1%BB%8Bnh-phi%C3%AAn-b%E1%BA%A3n-mariadb)

[3. Cài đặt chỉ định phiên bản PHP]

## 1. Cài đặt chỉ định phiên bản MySQL

Để cài đặt phiên bản MySQL mà ta mong muốn, trước tiên ta phải kích hoạt kho lưu trữ (Repository) đúng với phiên bản mà ta muốn

### Ví dụ 1: Cài đặt MySQL version 8.0

- Kích hoạt kho dữ liệu MySQL 8.0:

`yum localinstall https://dev.mysql.com/get/mysql80-community-release-el7-1.noarch.rpm -y`

- Cài đặt MySQL 8.0:

`yum install mysql-community-server -y`

### Ví dụ 2: Cài đặt MySQL 5.7

- Kích hoạt kho lưu trữ MySQL 5.7:

`yum localinstall https://dev.mysql.com/get/mysql57-community-release-el7-11.noarch.rpm -y`

- Cài đặt MySQL 5.7:

`yum install mysql-community-server -y`

## 2. Cài đặt chỉ định phiên bản MariaDB

Trên hệ điều hành CentOS 7, phiên bản mặc định của MariaDB là 5.5. Để cài đặt các phiên bản khác, ta làm theo các bước sau

### Bước 1: Tạo Kho lưu trữ MariaDB

`vi /etc/yum.repos.d/MariaDB.repo`

```
# MariaDB 10.3 CentOS repository list - created 2018-05-25 19:02 UTC
# http://downloads.mariadb.org/mariadb/repositories/
[mariadb]
name = MariaDB
baseurl = http://yum.mariadb.org/10.3/centos7-amd64
gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
gpgcheck=1
```

Tại `baseurl`, ta truy cập vào http://yum.mariadb.org/ để tìm đến phiên bản mà ta muốn, sau đó sao chép đường dẫn URL của phiên bản copy vào `baseurl`. Ví dụ trên là phiên bản MariaDB 10.3.

### Bước 2: Cài đặt MariaDB

Lúc này chúng ta đã chỉ định phiên bản cài đặt cho MariaDB, ta sử dụng lệnh sau để cài đặt:

`yum -y install MariaDB-server MariaDB-client`

## 3. Cài đặt chỉ định phiên bản PHP

Trong kho phần mềm chính thức của CentOS 7 thì PHP 5.4 đã cũ và không còn được các nhà phát triển hỗ trợ, duy trì và cập nhật bản vá lỗi.

Để hỗ trợ cho những tính năng mới cũng như tăng cường khả năng bảo mật, ta cần phiên bản PHP mới hơn trên hệ thống CentOS 7.

Ta sẽ cài đặt các phiên bản PHP 7.

### Bước 1: Kích hoạt EPEL và Remi repository trên hệ thống

- Cài đặt EPEL repository:

`yum -y install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm`

- Cài đặt Remi repository:

`yum -y install https://rpms.remirepo.net/enterprise/remi-release-7.rpm`

### Bước 2: Cấu chỉ định phiên bản PHP

Server đã được thiết lập để cài đặt PHP từ yum repository. Sử dụng một trong những lệnh bên dưới để cấu hình cài đặt các phiên bản PHP 7.0, PHP 7.1, PHP 7.2, PHP 7.3 hoặc PHP 7.4:

```
## Install PHP 7.4
# yum-config-manager --enable remi-php74

## Install PHP 7.3 
# yum-config-manager --enable remi-php73

## Install PHP 7.2 
# yum-config-manager --enable remi-php72

## Install PHP 7.1 
# yum-config-manager --enable remi-php71

## Install PHP 7.0 
# yum-config-manager --enable remi-php70
```

### Bước 3: Cài đặt PHP

`yum -y install php`

Chúng ta có thể cài đặt thêm các Modules cho PHP, để tìm các modules ta sử dụng lệnh:

`yum search php | more`

## Tài liệu tham khảo

https://blog.hostvn.net/chia-se/huong-dan-cai-dat-mysql-tren-centos-7.html

https://linuxize.com/post/install-mariadb-on-centos-7/

https://wiki.matbao.net/kb/huong-dan-cai-dat-php-7-x-tren-centos-7/