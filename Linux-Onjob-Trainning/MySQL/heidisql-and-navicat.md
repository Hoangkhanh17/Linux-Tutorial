# Sử dụng HeidiSQL và Navicat để truy cập vào Cơ sở dữ liệu

## Mục lục

## 1. HediSQL

HeidiSQL là một công cụ quản trị nguồn mở và miễn phí cho MySQL, MariaDB và các nhánh của nó. Ngoài ra còn hỗ trợ Microsoft SQL Server và PostgreQuery. Sau đây là phần giới thiệu và hướng dẫn cài đặt quản lý cơ sở dữ liệu với phần mềm HeidiSQL.

Một số tính năng nổi bật HeidiSQL:

- HeidiSQL hỗ trợ nền tảng Windows XP, Vista, 7, 8, 10, Wine và Linux.

- Kết nối với máy chủ MariaDB, MySQL, MS SQL và PostgreSQL.

- Thiết lập kết nối SSH tới MariaDB hoặc MySQL.

- Sử dụng Command Line hoặc GUI.

- Các thao tác với bảng, khung nhìn, thủ tục, kích hoạt và sự kiện.

- Khởi chạy các câu truy vấn SQL.

- Nhập và xuất các tập tin SQL.

- Phiên bản HeidiSQL Portable.

### 1.1. Cài đặt HeidiSQL

Dowload phần mềm HediSQL tạ trang chủ https://www.heidisql.com/download.php.

Sau khi tải phần mềm HeidiSQL về máy bạn cần khởi chạy tập tin EXE:

- Nhấn vào tùy chọn I accept the agreeement và nhấn Next để qua màn hình kế tiếp.

<img src="https://i.ibb.co/4Knbv25/1.png">

- Tiếp theo bạn cần chọn đường dẫn thư mục lưu trữ trong Browse… hoặc có thể để mặc định.

<img src="https://i.ibb.co/SVZF7Bd/image.png">

- Bạn có thể chọn một số tùy chọn sau đó nhấn Next hoặc có thể để như mặc định.

<img src="https://i.ibb.co/37z9L7H/image.png">

- Bây giờ chỉ cần nhấn Install để bắt đầu cài đặt phần mềm HeidiSQL.

<img src="https://i.ibb.co/zbbdZ2T/image.png">

### 1.2. Kết nối HediSQL tới MySQL

Sau khi cài đặt xong thì bạn có thể sử dụng để kết nối với máy chủ cơ sở dữ liệu như MySQL, MariaDB, Microsoft SQL Server hoặc PostgreQuery.

- Để kết nối với SQL Server, ta phải sử dụng lệnh để cho phép kết nối với HeidiSQL. Truy cập vào Máy chủ SQL để sử dụng lệnh:

`mysql -u root -p`

`GRANT ALL PRIVILEGES ON wordpress.* TO root IDENTIFIED BY 'password';`

Câu lệnh có ý nghĩa là cho phép user `root` có mật khẩu là `password` truy cập vào Cơ sở dữ liệu `wordpress`.

- Đăng nhập vào HeidiSQL với các thông tin trên:

<img src="https://i.ibb.co/QdzdNF5/image.png">

Trong đó ta cần chú ý các thông tin:

- Network type: Loại kết nối ta muốn, ở đây ta chọn MySQL (TCP/IP)

- Hostname/IP: Tên Hostname hoặc địa chỉ IP của máy chủ SQL.

- User: User mà ta đã cho phép kết nối ở trên.

- Password: Mật khẩu ta cho phép kết nối ở trên.

- Port: Port mặc định của MySQL hoặc MariaDB là `3306`, ta có thể thay đổi nếu như máy chủ SQL thay đổi port.

<img src="https://i.ibb.co/5vLV3m4/image.png">

## 2. NaviCat

Navicat Premium là một công cụ quản trị cơ sở dữ liệu đa kết nối nâng cao cho phép bạn kết nối đồng thời với tất cả các loại cơ sở dữ liệu một cách dễ dàng. Navicat cho phép bạn kết nối với cơ sở dữ liệu MySQL, MariaDB, Oracle, PostgreSQL, SQLite và SQL Server từ một ứng dụng duy nhất, giúp quản trị cơ sở dữ liệu trở nên dễ dàng.

Các Tính Năng:

- Tạo kết nối dễ dàng đến gần như tất cả các loại Database.

- Tạo cơ sở dữ liệu.

- Mô hình dữ liệu hiện ngay trước mặt, thêm khóa, xóa khóa sử dụng kéo chuột.

- Tự động format biểu đồ ER Diagram.

- Viết câu lệnh SQL.

- Thiết kế các Mode.

So với HeidiSQL, NaviCat là ứng dụng trả phí, ta có thể sử dụng bản Trial để trải nghiệm trước.

### 2.1. Cài đặt NaviCat

Đăng nhập vào trang chủ https://www.navicat.com/en/products để tải NaviCat.

Ta chọn NaviCat Prenium để sử dụng với tất cả các hệ thống SQL mà NaviCat hỗ trợ.

- Sau khi dowload xong, bạn càn chạy tệp exe và cài đặt

<img src="https://i.ibb.co/HC3zpvG/image.png">

- Chọn `I Accept the agreement`

<img src="https://i.ibb.co/cYDYPfj/image.png">

- Các bước cài đặt tiếp theo cũng tương tự như HeidiSQL.

### 2.2. Kết nối NaviCat với Máy chủ Database

- Ta chọn vào mục `Connection` để tìm loại Cơ sở dữ liệu muốn kết nối.

<img src="https://i.ibb.co/tP8sT3v/image.png">

- Trong ví dụ ta sẽ chọn MariaDB. Tại đây sẽ hiện lên bảng điền thông tin Máy chủ ta cần điền. Các thông tin cần điền cũng tương tự HeidiSQL.

<img src="https://imgur.com/ZXozUBr.png">

- Chọn `Test Connection` để kiểm tra kết nối. Nếu có thông báo `Connection Successful` thì kết nối thành công.

- Giao diện về thông tin Database khi kết nối thành công:

<img src="https://imgur.com/QjXaYv9.png">

## Chúc các bạn thành công !