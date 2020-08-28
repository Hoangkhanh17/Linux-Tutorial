# Cách sử dụng file .htaccess trong Apache

Mặc định trong Apache sẽ có một file tên là `.htaccess`. File `.htaccess` này có tác dụng thay đổi truy cập mà không làm ảnh hưởng đến cấu hình.

Nếu như trong Apache không có sẵn `.htaccess` ta cũng có thể tạo một file

## 1. Cấu hình từ non-www sang www

```
RewriteEngine On
RewriteCond %{HTTP_HOST} ^quanganh.abc [NC]
RewriteRule ^(.*)$ http://www.quanganh.abc/$1 [L,R=301]
```

Sau đó lưu lại, sử dụng lệnh `curl` để kiểm tra:

```
[root@localhost html]# curl -I http://quanganh.abc
HTTP/1.1 301 Moved Permanently
Date: Fri, 28 Aug 2020 07:36:32 GMT
Server: Apache/2.4.6 (CentOS) PHP/7.2.33
X-Powered-By: PHP/7.2.33
X-Redirect-By: WordPress
Location: http://www.quanganh.abc/
Content-Type: text/html; charset=UTF-8
```

Ta thấy `Location: http://www.quanganh.abc/` là kết quả mong muốn

## 2. Cấu hình từ www sang non-www

Cấu hình này sẽ ngược lại với cấu hình ở Mục 1, ta thay đổi cấu hình tại file `.htaccess`

```
RewriteEngine On
RewriteCond % ^www.quanganh.abc
RewriteRule (.*) http://tenmiencuaban.com/$1 [R=301,L]
```

Sau đó lưu lại và kiểm tra. Kết quả:

```
[root@localhost html]# curl -I http://www.quanganh.abc
HTTP/1.1 301 Moved Permanently
Date: Fri, 28 Aug 2020 07:36:32 GMT
Server: Apache/2.4.6 (CentOS) PHP/7.2.33
X-Powered-By: PHP/7.2.33
X-Redirect-By: WordPress
Location: http://quanganh.abc/
Content-Type: text/html; charset=UTF-8
```