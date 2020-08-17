# Cài đặt KVM trên CentOS 7

## Mục lục

[1. Chuẩn bị môi trường CentOS7](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob%20Trainning/Kernel%20Virtualization%20Machine/Install%20KVM%20CentOS7.md#1-chu%E1%BA%A9n-b%E1%BB%8B-m%C3%B4i-tr%C6%B0%E1%BB%9Dng-centos7)

[2. Cài đặt KVM](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob%20Trainning/Kernel%20Virtualization%20Machine/Install%20KVM%20CentOS7.md#2-c%C3%A0i-%C4%91%E1%BA%B7t-kvm)

[3. Cài đặt VM trên KVM](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob%20Trainning/Kernel%20Virtualization%20Machine/Install%20KVM%20CentOS7.md#3-c%C3%A0i-%C4%91%E1%BA%B7t-vm-tr%C3%AAn-kvm)

- [3.1. Tạo VM virt-manager](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob%20Trainning/Kernel%20Virtualization%20Machine/Install%20KVM%20CentOS7.md#31-t%E1%BA%A1o-vm-b%E1%BA%B1ng-virt-manager)

- [3.2. Tạo VM bằng virt-install](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob%20Trainning/Kernel%20Virtualization%20Machine/Install%20KVM%20CentOS7.md#32-t%E1%BA%A1o-vm-b%E1%BA%B1ng-virt-install)

    - [Tạo VM từ File ISO](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob%20Trainning/Kernel%20Virtualization%20Machine/Install%20KVM%20CentOS7.md#t%E1%BA%A1o-vm-t%E1%BB%AB-file-iso)

[Tài liệu tham khảo](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob%20Trainning/Kernel%20Virtualization%20Machine/Install%20KVM%20CentOS7.md#t%C3%A0i-li%E1%BB%87u-tham-kh%E1%BA%A3o)

## 1. Chuẩn bị môi trường CentOS7

Một máy chạy CentOS 7 hỗ trợ môi trường ảo hóa. Thông số:

- **CPU:** 2 nhân, 2 luồng.

- **RAM:** 2GB.

- **Disk:** 50GB.

- Nếu cài đặt trên VMware, ta có thể bật hỗ trợ ảo hóa trong **Virtual Machine Settings**

<img src="https://imgur.com/WlZ2vG2.png">

Kiểm tra xem máy chủ CentOS có hỗ trợ ảo hóa không:

```
[root@localhost ~]# egrep -c "svm|vmx" /proc/cpuinfo
4
```

Nếu giá trị trả về là `0` thì sẽ không hỗ trợ ảo hóa.

## 2. Cài đặt KVM

### Cài đặt các gói cần thiết

```
[root@localhost ~]# yum -y install qemu-kvm libvirt virt-install bridge-utils virt-manager
```

Trong đó:

- **qemu-kvm:** Phần phụ trợ cho KVM.

- **libvirt:** cung cấp libvirt mà bạn cần quản lý qemu và KVM bằng libvirt.

- **bridge-utils:** chứa một tiện ích cần thiết để tạo và quản lý các thiết bị bridge.

- **virt-manager:** cung cấp giao diện đồ họa để quản lý máy ảo.

- **virt-install:** Cung cấp lệnh để cài đặt máy ảo.

Sau khi cài đặt xong, kiểm tra lại các module KVM:

```
[root@localhost ~]# lsmod | grep kvm
kvm_intel             188688  0
kvm                   636969  1 kvm_intel
irqbypass              13503  1 kvm
```

Bật libvirt và kích hoạt khởi động cùng hệ thống

```
[root@localhost ~]# systemctl start libvirtd
[root@localhost ~]# systemctl enable libvirtd
```

Để sử dụng đồ họa cho virt-manager, ta cài đặt thêm gói X-window:

```
[root@localhost ~]# yum install "@X Window System" xorg-x11-xauth xorg-x11-fonts-* xorg-x11-utils -y
```

Sau khi cài đặt xong, ta `reboot` lại máy và kích hoạt Virtual Manager bằng lệnh `[root@localhost ~]# virt-manager`.

Để truy cập vào `virtual-manager` ta sử dụng Ứng dụng **Moba Xterm**, sau đó SSH vào máy chủ CentOS vừa cài đặt và sử dụng lệnh `virt-manager`, cửa sổ của Virtual Manager sẽ hiện lên:

<img src="https://imgur.com/pP9oovU.png">

## 3. Cài đặt VM trên KVM

Tạo thư mục để lưu trữ `file iso` cài đặt hệ điều hành. Chúng ta có thể `download`, `copy` các file iso để thuận tiện trong việc cài đặt VM. 

```
[root@localhost ~]# cd /var/lib/libvirt
[root@localhost libvirt]# mkdir file-iso
```

### 3.1. Tạo VM bằng virt-manager

#### Bước 1: File -> New Virtual Machine

<img src="https://imgur.com/pDZYZ5R.png">

#### Bước 2: Chọn Local install Media để cài đặt bằng file iso đã lưu trong máy chủ

<img src="https://imgur.com/IN3kMNi.png">

#### Bước 3: Chọn Use Iso image -> Browse để tiến hành chọn file iso

<img src="https://imgur.com/UCMXJPI.png">

#### Bước 4: Chọn Browse Local để tìm đến thư mục lưu file iso

<img src="https://imgur.com/mIyqmWa.png">

#### Bước 5: Filesytem root -> /var/lib/libvirt/file-iso để đến thư mục lưu file iso

<img src="https://imgur.com/cdm02Cw.png">

#### Bước 6: Chọn Iso thành công, chọn forward

<img src="https://imgur.com/UdxXlVV.png">

#### Bước 7: Lựa chọn dung lượng RAM, CPU, Disk

<img src="https://imgur.com/RjhMoxR.png">

<img src="https://imgur.com/EWtnaHT.png">

#### Bước 8: Review lại các thông số cấu hình và lựa chọn dải mạng

<img src="https://imgur.com/cosC7ET.png">

#### Bước 9: Tiến hành cài đặt hệ điều hành

<img src="https://imgur.com/rPRrgYk.png">

Kiểm tra trạng thái hoạt động của các máy ảo trên máy chủ cài KVM

```
[root@localhost file-iso]# virsh list --all
 Id    Name                           State
----------------------------------------------------
 1     centos7.0                      running
```

Sau khi cài đặt thành công, kiểm tra địa chỉ IP của VM vừa tạo khi gắn card mạng NAT. Địa chỉ IP nhận được là 192.168.100.197

<img src="https://imgur.com/O9fFQc4.png">

Tiến hành kiểm tra ping từ máy chủ CentOS cài KVM

```
[root@localhost file-iso]# ping 192.168.100.197
PING 192.168.100.197 (192.168.100.197) 56(84) bytes of data.
64 bytes from 192.168.100.197: icmp_seq=1 ttl=64 time=0.583 ms
64 bytes from 192.168.100.197: icmp_seq=2 ttl=64 time=0.715 ms
64 bytes from 192.168.100.197: icmp_seq=3 ttl=64 time=0.709 ms
^C
--- 192.168.100.197 ping statistics ---
3 packets transmitted, 3 received, 0% packet loss, time 2000ms
rtt min/avg/max/mdev = 0.583/0.669/0.715/0.060 ms
```

### 3.2. Tạo VM bằng virt-install

Có 3 cách tạo VM bằng `virt-install`: Tạo VM từ images, ISO và bằng lệnh tải về từ Internet.

#### Tạo VM từ File ISO

```
virt-install \
 -n myRHELVM1 \
 --description "Test VM with RHEL 6" \
 --os-type=Linux \
 --os-variant=rhel6 \
 --ram=2048 \
 --vcpus=2 \
 --disk path=/var/lib/libvirt/images/myRHELVM1.img,bus=virtio,size=10 \
 --graphics none \
 --cdrom /var/rhel-server-6.5-x86_64-dvd.iso \
 --network bridge:br0
```

Trong đó: 

- **-n** Tên VM muốn đặt.

- **-- descriptop** Mô tả về VM.

- **--os-type** Loại OS.

- **--os-variant** Tên OS.

- **--ram** dung lượng RAM ta đặt cho VM (theo MB).

- **-vcpus** số nhân CPU cho VM.

- **--disk path** Nơi lưu ổ cứng cho VM, `size` là dung lượng cấp cho ổ cứng.

- **--graphics** Đồ họa cấp cho VM.

- **--cdrom** Nơi lưu trữ file ISO để cài đặt.

- **--network bridge** Chọn Virtual Network cho VM.

## Tài liệu tham khảo

https://news.cloud365.vn/kvm-huong-dan-cai-dat-kvm-tren-centos-7/

https://github.com/niemdinhtrong/thuctapsinh/blob/master/NiemDT/KVM/docs/Mo-hinh-mang-KVM.md

https://github.com/thaonguyenvan/meditech-thuctap/blob/master/ThaoNV/KVM/tao-VM-KVM.md#virt-install