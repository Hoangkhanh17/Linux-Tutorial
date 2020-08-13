# Tìm hiểu về Logical Volume Manager - LVM

## Mục Lục

## Phần 1. Giới thiệu về LVM

1. LVM là gì ?

LVM là một công cụ để quản lý phân vùng logic được tạo và phân bổ từ các ổ đĩa vật lý. Với LVM bạn có thể dễ dàng tạo mới, thay đổi kích thước hoặc xóa bỏ phân vùng đã tạo.

Với kỹ thuật Logical Volume Manager (LVM), ta có thể thay đổi kích thước mà không cần phải sửa lại partition table của OS. Nếu muốn mở rộng dung lượng của nó, ta chỉ cần ấn định lại dung lượng mà không cần phân vùng lại, cũng không phải đối mặt với nguy cơ mất dữ liệu khi thay đổi dung lượng như khi thao tác trên Partition.

2. Cấu trúc LVM

<img src="https://imgur.com/VtGksx7.png">

- **Hard Drives:** Thiết bị lưu trữ dữ liệu. 

- **Partition:** Partitions là các phân vùng của Hard drives, mỗi Hard drives có 4 partition. Có 2 loại Partition:

    - **Primary partition:** Phân vùng chính, có thể khởi động , mỗi đĩa cứng có thể có tối đa 4 phân vùng này.

    - **Extended partition:** Phân vùng mở rộng.

- **Physical Volumes:** Một ổ đĩa vật lý có thể phân chia thành nhiều phân vùng vật lý gọi là Physical Volumes.

- **Volume Group:** Là một nhóm bao gồm nhiều Physical Volume trên 1 hoặc nhiều ổ đĩa khác nhau được kết hợp lại thành một Volume Group. Volume Group được sử dụng để tạo ra các Logical Volume, trong đó người dùng có thể tạo, thay đổi kích thước, lưu trữ, gỡ bỏ và sử dụng.

<img src="https://imgur.com/gHIAFxL.png">



- **Logical Volume:** Một Volume Group được chia nhỏ thành nhiều Logical Volume. Nó được dùng để mount tới hệ thống tập tin (File System) và được format với những chuẩn định dạng khác nhau như ext2, ext3, ext4…


<img scr="https://i.imgur.com/U2aX6X1.png">


## Tài liệu tham khảo

https://news.cloud365.vn/lvm-gioi-thieu-ve-logical-volume-manager/

https://blogd.net/linux/tao-va-quan-ly-lvm-trong-linux/

https://vinasupport.com/lvm-la-gi-tao-vao-quan-ly-logical-volume-manager/

https://www.ufsexplorer.com/articles/storage-technologies/lvm-data-organization.php

https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/logical_volume_manager_administration/lvm_examples