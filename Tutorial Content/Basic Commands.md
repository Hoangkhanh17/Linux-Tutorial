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

    Khi sử dụng lệnh rm để xóa file thì làm giảm đi một hard link. Khi số lượng hard link giảm còn 0 thì không thể truy cập tới nội dung của file được nữa
2. **Liên kết mềm (Symbolic Links)**

    Khi tạo Symbolic link với tên là `othername` thì hệ thống sẽ tạo ra một chỉ số inode khác tương ứng với tên file đó. Inode này sẽ tham chiếu đến một vùng nhớ khác chứa địa chỉ, địa chỉ này sẽ trỏ đến một vùng nhớ chứa dữ liệu lưu trữ đường dẫn đến file gốc `filename`.

    <img src="https://images.viblo.asia/bf8c7003-1a2f-487d-89e9-a4c9f2ac608c.png">

    Lệnh tạo liên kết tượng trưng như sau: `ln -s [file nguồn] [file đích]`

    ```
    # ln -s file2.txt file4.txt
    # ls -i file2.txt file4.txt
    1059736 file2.txt  1060252 file4.txt
    ```

    ```
    # echo "Hello World" > file2.txt
    # cat file2.txt
    Hello World
    # cat file4.txt
    Hello World
    ```
    
    Nếu xóa tệp tin file2.txt thì nội dung tại tệp file4.txt sẽ không còn:

    ```
    # rm file2.txt
    # cat file4.txt
    cat: file4.txt: No such file or directory
    ```

3. ** So sánh giữa Hard Links và Symbolic Links**

    | Hard Links | Symbolic Links |
    |------------|----------------|
    |Chỉ liên kết được tới file, không liên kết được tới thư mục| Có thể liên kết được tới thư mục|
    |Không tham chiếu được tới file trên ổ đĩa khác| Có thể tham chiếu tới file/thư mục khác ổ đĩa|
    |Liên kết tới một file vẫn còn ngay cả khi file đó đã được di chuyển	|Liên kết không còn tham chiếu được nữa nếu file được di chuyển|
    |Được liên kết với inode tham chiếu vật lý trên ổ cứng nơi chứa file	|Liên kết tham chiếu tên file/thư mục trừu tượng mà không phải địa chỉ vật lý. Chúng được cung cấp inode riêng của mình|
    |Có thể làm việc với mọi ứng dụng	|Một số ứng dụng không cho phép symbolic link