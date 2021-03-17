# Tài Liệu đóng Image Ubuntu 2004

## Thực hành trên WebvirtCloud

## Phần 1: Tạo mới VM Ubuntu 20.04 (WebvirtCloud)

### Bước 1: Tạo Disk mới

<img src="https://imgur.com/IZeTWld.png">

<img src="https://imgur.com/vBMt1mC.png">

#### Lưu ý: Với Linux ta nên đặt Disk có dung lượng nhỏ nhất là 10GB để cài Hệ điều hành trắng và có thể tăng dung lượng Disk lên nếu cần.

<img src="https://imgur.com/CctwvP4.png">

### Bước 2: Tạo Image Ubuntu 2004

<img src="https://imgur.com/SmVQrsm.png">

<img src="https://imgur.com/1wIyv5d.png">

<img src="https://imgur.com/DCKb9UE.png">

- Tại phần thông số RAM và CPU ta có thể để: 2GB Ram, 2 Cores CPU để tăng tốc độ Boot OS.

- Lựa chọn HDD với Disk đã tạo ở trên.

- Lựa chọn dải VLAN cung cấp địa chỉ IP

<img src="https://imgur.com/zS5rbSR.png">

### Lựa chọn file iso để cài đặt OS

<img src="https://imgur.com/b13WryY.png">

<img src="https://imgur.com/I45A5wH.png">

- Lựa chọn Boot theo thứ tự: 1:vda, 2:hda

<img src="https://imgur.com/ujEbyQd.png">

- Power On và Console vào cài đặt OS

<img src="https://imgur.com/Am4rTdm.png">

<img src="https://imgur.com/3YkMkDT.png">


### Bước 3: Cài đặt OS

#### Lưu ý: Do OS sẽ cài OS trắng nên sẽ bỏ hết các Options