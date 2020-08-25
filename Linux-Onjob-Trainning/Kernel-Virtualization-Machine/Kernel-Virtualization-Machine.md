# Tìm hiểu về Kernel Virtualization Machine - KVM

## Mục Lục

[1. Công nghệ ảo hóa KVM](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob-Trainning/Kernel-Virtualization-Machine/Kernel-Virtualization-Machine.md#1-c%C3%B4ng-ngh%E1%BB%87-%E1%BA%A3o-h%C3%B3a-kvm)

[2. Cách thức hoạt động của KVM](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob-Trainning/Kernel-Virtualization-Machine/Kernel-Virtualization-Machine.md#2-c%C3%A1ch-th%E1%BB%A9c-ho%E1%BA%A1t-%C4%91%E1%BB%99ng-c%E1%BB%A7a-kvm)

[3. Ưu điểm và nhược điểm của KVM](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob-Trainning/Kernel-Virtualization-Machine/Kernel-Virtualization-Machine.md#3-%C6%B0u-%C4%91i%E1%BB%83m-v%C3%A0-nh%C6%B0%E1%BB%A3c-%C4%91i%E1%BB%83m-c%E1%BB%A7a-kvm)

- [a) Ưu điểm](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob-Trainning/Kernel-Virtualization-Machine/Kernel-Virtualization-Machine.md#a-%C6%B0u-%C4%91i%E1%BB%83m)

- [b) Nhược điểm](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob-Trainning/Kernel-Virtualization-Machine/Kernel-Virtualization-Machine.md#b-nh%C6%B0%E1%BB%A3c-%C4%91i%E1%BB%83m)

[Tài liệu tham khảo](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob-Trainning/Kernel-Virtualization-Machine/Kernel-Virtualization-Machine.md#t%C3%A0i-li%E1%BB%87u-tham-kh%E1%BA%A3o)

## 1. Công nghệ ảo hóa KVM

KVM là công nghệ ảo hóa mới cho phép ảo hóa thực sự trên nền tảng phần cứng. Nghĩa là OS (hệ điều hành) chính mô phỏng phần cứng cho các OS khác để chạy trên đó. Nó hoạt động tương tự như một người quản lý siêu việt chia sẻ công bằng các tài nguyên như disk (ổ đĩa), network IO và CPU.

KVM VPS không có tài nguyên dùng chung, tất cả đã được mặc định sẵn và chia sẻ. Điều đó có nghĩa là RAM của mỗi VPS KVM đã được phân bổ sẵn cho từng gói VPS để sở hữu, tận dụng hoàn toàn 100% và không bị chia sẻ ra ngoài. Đồng nghĩa với việc VPS KVM sẽ hoạt động ổn định hơn mà không bị ảnh hưởng bởi các VPS khác cùng hoạt động trên hệ thống. Tương tự điều đó với disk space, tài nguyên ổ cứng cũng đã được xác định sẵn và không bị vay mượn bởi các VPS khác.

## 2. Cách thức hoạt động của KVM

- KVM chuyển đổi một nhân Linux (Linux kernel) thành một bare metal hypervisor và thêm vào đó những đặc trưng riêng của các bộ xử lý Intel như Intel VT-X hay AMD như AMD-V.

- Khi đã trở thành một hypervisor, KVM hoàn toàn có thể setup các máy ảo với các hệ điều hành khác nhau và không phụ thuộc vào hệ điều hành của máy chủ vật lý.

- Trong kiến trúc của KVM, Virtual machine được thực hiện tương tự như là quy trình xử lý thông thường của Linux, được lập lịch hoạt động như các scheduler tiểu chuẩn của Linux.

- Trên thực tế, mỗi CPU ảo hoạt động như một tiến trình xử lý của Linux. Do đó, KVM được quyền thừa hưởng những ưu điểm từ các tính năng của nhân Linux.

<img src="https://blog.tinohost.com/wp-content/uploads/2019/02/kvm-kientruc.png">

## 3. Ưu điểm và nhược điểm của KVM

### a) Ưu điểm

- **Linh hoạt:** Tuy máy chủ gốc được cài đặt Linux, nhưng KVM hỗ trợ tạo máy chủ ảo có thể chạy cả Linux, Windows. Sử dụng kết hợp với QEMU, KVM có thể chạy Mac OS X. KVM cũng hỗ trợ cả x86 và x86-64 system.

- **Tính độc quyền cao:** Cấu hình từng gói VPS KVM sẽ chỉ một người sở hữu và toàn quyền sử dụng tài nguyên đó (CPU, RAM, diskspace…) mà không hề bị chia sẻ hay ảnh hưởng bởi các VPS khác trên cùng hệ thống.

- **Độ bảo mật chắc chắn:** Tích hợp các đặc điểm bảo mật của Linux như SELinux với các cơ chế bảo mật nhiều lớp, KVM bảo vệ các máy ảo tối đa và cách ly hoàn toàn.

- **Tiết kiệm chi phí, độ mở rộng cao:** Được phát triển trên nền tảng mã nguồn mở hoàn toàn miễn phí, được hỗ trợ từ cộng đồng và từ nhà sản xuất thiết bị, KVM ngày càng lớn mạnh và trở thành một lựa chọn hàng đầu cho doanh nghiệp với chi phí thấp, hiệu quả sử dụng cao.

### b) Nhược điểm

- **Yêu cầu cao về server/máy chủ:** Là công nghệ ảo hóa hoàn toàn phần cứng, VPS KVM yêu cầu cấu hình server vật lý khá cao. Thậm chí yêu cầu phải sử dụng các server của các thương hiệu lớn như IBM, Dell thì mới đảm bảo hoạt động tốt được.

## Tài liệu tham khảo

https://blog.tinohost.com/cong-nghe-ao-hoa-kvm-vmware/