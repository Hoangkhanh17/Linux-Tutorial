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

# Liên kết cứng và liên kết mềm

## Hard links and Symbolic Links

1. **Liên kết cứng (Hard Links)**

    Trong một hệ thống Linux, liên kết cứng là các liên kết cấp thấp ( low-level links) mà hệ thống sử dụng để tạo các thành phần của chính hệ thống file, chẳng hạn như file và thư mục. Liên kết cứng sẽ tạo một liên kết trong cùng hệ thống tập tin với 2 inode entry tương ứng trỏ đến cùng một nội dung vật lý (cùng số inode vì chúng trỏ đến cùng dữ liệu).

    Tất cả các hệ thống tệp tin dựa trên thư mục phải có ít nhất một liên kết cứng (link counts từ 1 trở lên) cung cấp tên gốc cho mỗi tệp tin.

    <img src="https://images.viblo.asia/854df42c-5097-49cf-8c32-23fdd8be3484.png">

    Lệnh tạo Hard Links: `ln [file nguồn] [file đích]`. Ví dụ:

    ```
    # ls -li file1.txt file2.txt
    1059736 -rw-r--r-- 2 root root 16 Aug 12 02:24 file1.txt
    1059736 -rw-r--r-- 2 root root 16 Aug 12 02:24 file2.txt

    # echo "Hello" > file1.txt
    # cat file1.txt
    Hello
    # cat file2.txt
    Hello
    ```
    
    Hai tệp tin file1.txt và file2.txt có số inode giống nhau 1059736. Xóa file1.txt thì nội dung tại file2.txt vẫn còn.

    ```
    # rm file1.txt
    # cat file2.txt
    Hello
    ```
