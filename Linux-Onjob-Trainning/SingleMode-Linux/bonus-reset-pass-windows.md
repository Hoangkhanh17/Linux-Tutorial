# Hướng dẫn reset mật khẩu Quản trị cho Windows Server

## 1. Chuẩn bị

- Ta cần chuẩn bị một USB cài Boot Windows Server, USB từ 8GB trở lên để đáp ứng được dung lượng cả file ISO Windows Server.

- File ISO là bản cài Windows Server tương ứng với Windows Server hiện tại (2012, 2016,...)

- Phần mềm Rufus để cài USB boot, tham khảo [tại đây](https://www.keybanquyen.vn/2019/07/huong-dan-tao-usb-boot-cai-dat-windows-10-server-2016-2019.html)

## 2. Các bước thực hiện

#### Bước 1: Boot máy chủ bằng USB boot vừa tạo

<img src="https://imgur.com/EupPNN1.png">

#### Bước 2: Chọn "Next" và Chọn ”Repair your computer“

<img src="https://imgur.com/zFoSX6j.png">

#### Bước 3: Chọn "Trouble Shoot"

<img src="https://imgur.com/86HE6up.png">

#### Bước 4: Chọn "Command Prompt" để mở cửa sổ lệnh của Windows

<img src="https://imgur.com/qe5BnHT.png">

#### Bước 5: Xác định ổ chứa OS Windows Server

Ta phải xác định ổ chứa OS của Windows Server mà ta đã cài đặt, ví dụ trong bài này sẽ là ổ :/D

Các lệnh thực hiện:

```
x:\source>d: (D – là partition OS chứa các file system)

D:\>cd Windows\system32 (dùng lệnh ”cd ” truy cập vào đường dẫn Windows\system32)

D:\Windows\system32>ren Utilman.exe Utilman.exe.old  (dùng lệnh “ren” để rename lại file Utilman.exe thành file Ultilman.exe.old)

D:\Windows\system32>copy cmd.exe Utilman.exe ( dùng lệnh “copy” copy file “cmd.exe” đè lên “Utilman.exe” . Mục đích copy như vậy để khi login windows bấm phím Windows+U để mở cửa sổ Command Line)
```

Sau khi hoàn thành ta đóng cửa sổ Command Prompt và chọn "Continue"

<img src="https://imgur.com/WNDMIRC.png">

#### Bước 6: Để cho Windows Server khởi động bình thường, đến phần Login cho nhấn Tổ hợp phím Windows + U để mở cửa sổ Command Line

Lưu ý: Nếu bấm phím mà không mở ra cửa sổ CMD thì Click vào biểu tượng đồng hồ góc phải phía dưới như hình sẽ mở ra cửa sổ CMD.

<img src="https://imgur.com/d5ULnyn.png">

#### Bước 7: Sử dụng lệnh để thay đổi mật khẩu 

`net user administrator (password cần đặt)`

Thông báo thay đổi mật khẩu thành công:

`The command completed successfully`

Sau khi thay đổi mật khẩu thành công ta login lại với mật khẩu mới

### Chúc các bạn thành công !

## Tài liệu tham khảo

https://kb.pavietnam.vn/reset-password-windows-server-2016.html