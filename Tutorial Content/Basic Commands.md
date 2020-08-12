# Xác định thư mục Dịch vụ

Một số thư mục nên được sử dụng để lưu trữ các chương trình thực thi:

```
/bin
/usr/bin
/sbin
/usr/sbin
/opt.
```

Để tìm đường dẫn đến thự mục chứa ứng dụng, ta dùng lệnh `which`. Ví dụ:
```
$ which httpd
/usr/sbin/httpd
```

Nếu lệnh `which` không có tác dụng, ta có thể sử dụng lệnh `whereis`. Lệnh này có thể tìm kiếm các gói liên quan trong phạm vi rộng hơn.

```
whereis httpd
httpd: /usr/sbin/httpd /usr/lib64/httpd /etc/httpd /usr/share/httpd /usr/share/man/man8/httpd.8.gz
```

# Truy cập các thư mục

Để truy cập các thư mục trong Linux. Ta sử dụng các lệnh:

| Câu lệnh |   Kết quả   |
|----------|-------------|
| cd       | Chuyển đến thư mục chính  |
| cd ..     | Chuyển đến thư mục mẹ     |
| cd -     | Chuyển đến thư mục ngay trước khi ta sử dụng câu lệnh                |
| cd /     | Chuyển đến thư mục /   |

# Kiểm tra hệ thống tệp tin

Các lệnh sau có thể giúp ta kiểm tra hệ thống tệp tin

| Câu lệnh | Kết quả              |
|----------|----------------------|
| ls       | Liệt kê các nội dung của thư mục hiện hành |
| ls -a    | Liệt kê các nội dung bao gồm cả các thư mục và tệp tin ẩn của thư mục hiện hành   |
| tree     | Hiển thị sơ đồ cây các nội dung của thư mục hiện hành       |
| tree -d  | Chỉ hiện thị sơ đồ cây các thư mục của thư mục hiện hành |