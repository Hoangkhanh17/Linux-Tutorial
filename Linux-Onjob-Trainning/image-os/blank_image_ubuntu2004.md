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

<img src="https://imgur.com/4ZvH7Hg.png">

<img src="https://imgur.com/ocb6wi7.png">

<img src="https://imgur.com/oKoeSc8.png">

- Chọn `Use an entire disk` > `Done`. Lưu ý: KHÔNG DÙNG OPTION THIẾT LẬP DISK dạng LVM

<img src="https://imgur.com/onZ676t.png">

<img src="https://imgur.com/5nNakpR.png">

<img src="https://imgur.com/g7vKNdG.png">

- Điền các thông số và mật khẩu đăng nhập:

<img src="https://imgur.com/ww0zB0C.png">

- Lựa chọn cài đặt `Open SSH Server`:

<img src="https://imgur.com/uYDuSjv.png">

- Đợi OS được cài đặt sau đó tiến hành reboot:

<img src="https://imgur.com/cz2hwzO.png">

- Tiến hành Force Off máy và và gỡ ISO ra để Boot từ vda:

<img src="https://imgur.com/RaepcoU.png">

<img src="https://imgur.com/3dxDTCf.png">

- Kiểm tra lại Dịch vụ của SSH:

<img src="https://imgur.com/bFFC9aj.png">

- Kiểm tra lại cấu hình IP. Lưu ý: Cấu hình đúng dải IP Vlan đã chọn từ trước đó.

<img src="https://imgur.com/cdhPs0d.png">

<img src="https://imgur.com/gN4C11X.png">

- Tạo Snapshot trước khi tiến hành cài đặt các Dịch vụ

<img src="https://imgur.com/EZd4svq.png">

## Phần 2: Chuẩn bị Môi trường đóng Image Ubuntu 2004

### Bước 1: Thiết lập SSH cho user root

Login ssh với tài khoản ubuntu, chuyển user sudo

`sudo su`

Đổi mật khẩu cho User root

```
root@cloud:/home/ubuntu# passwd
New password:
Retype new password:
```

Cấu hình SSH cho User root

```
root@cloud:/home/ubuntu# sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/'g /etc/ssh/sshd_config
root@cloud:/home/ubuntu# systemctl restart sshd
```

Disable Firewall:

```
root@cloud:/home/ubuntu# systemctl disable ufw
Synchronizing state of ufw.service with SysV service script with /lib/systemd/systemd-sysv-install.
Executing: /lib/systemd/systemd-sysv-install disable ufw
Removed /etc/systemd/system/multi-user.target.wants/ufw.service.
root@cloud:/home/ubuntu# systemctl stop ufw
root@cloud:/home/ubuntu# systemctl status ufw
● ufw.service - Uncomplicated firewall
     Loaded: loaded (/lib/systemd/system/ufw.service; disabled; vendor preset: enabled)
     Active: inactive (dead)
       Docs: man:ufw(8)

Mar 17 04:08:53 cloud systemd[1]: Stopping Uncomplicated firewall...
Mar 17 04:08:53 cloud ufw-init[1140]: Skip stopping firewall: ufw (not enabled)
Mar 17 04:08:53 cloud systemd[1]: ufw.service: Succeeded.
Mar 17 04:08:53 cloud systemd[1]: Stopped Uncomplicated firewall.
```

Sau đó tiến hành Reboot lại VPS:

`init 6`

Kiểm tra lại SSH bằng User root:

<img src="https://imgur.com/4uoR69S.png">

Xóa user ubuntu đi, chỉ sử dụng user root cho quyền sudo

```
userdel ubuntu
rm -rf /home/ubuntu
```

### Bước 2: Điều chỉnh Timezone

Điều chỉnh Timezone về `Ho_Chi_Minh`:

`timedatectl set-timezone Asia/Ho_Chi_Minh`

Bổ sung `env locale`:

`echo "export LC_ALL=C" >>  ~/.bashrc`

### Bước 3: Disable IPv6

```
echo "net.ipv6.conf.all.disable_ipv6 = 1" >> /etc/sysctl.conf 
echo "net.ipv6.conf.default.disable_ipv6 = 1" >> /etc/sysctl.conf 
echo "net.ipv6.conf.lo.disable_ipv6 = 1" >> /etc/sysctl.conf
sysctl -p
```

Kiểm tra lại, kết quả trả về `1` là tắt thành công, `0` tức là IPv6 vẫn đang bật:

`root@cloud:~# cat /proc/sys/net/ipv6/conf/all/disable_ipv6`

### Bước 4: Kiểm tra và xóa phân vùng SWAP

```
root@cloud:~# cat /proc/swaps
Filename                                Type            Size    Used    Priority
/swap.img                               file            2067452 0       -2
```

Xóa phân vùng Swap:

```
root@cloud:~# swapoff -a
root@cloud:~# rm -rf /swap.img
```

Xóa cấu hình swap file trong file /etc/fstab:

`sed -Ei '/swap.img/d' /etc/fstab`

Kiểm tra lại:

```
root@cloud:~# free -m
              total        used        free      shared  buff/cache   available
Mem:           1987         129        1579           0         278        1706
Swap:             0           0           0
```