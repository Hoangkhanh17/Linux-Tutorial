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

### Bước 5: Cài đặt các gói Dịch vụ và Cập nhật OS

```
apt-get update -y 
apt-get upgrade -y 
apt-get dist-upgrade -y
apt-get autoremove 
```

### Bước 6: Cấu hình để instance báo log ra console, đổi tên Card mạng về eth* thay vì ens, eno

```
sed -i 's|GRUB_CMDLINE_LINUX=""|GRUB_CMDLINE_LINUX="net.ifnames=0 biosdevname=0 console=tty1 console=ttyS0"|g' /etc/default/grub
update-grub
```

### Bước 7: Tắt netplan và cài đặt ifupdown

Xóa Netplan:

```
apt-get --purge remove netplan.io -y
rm -rf /usr/share/netplan
rm -rf /etc/netplan
```

Cài đặt ifupdown:

```
apt-get update
apt-get install -y ifupdown
```

Tạo Snapshot:

## Phần 2: Cài đặt dịch vụ cần thiết cho Image Ubuntu 20.04

### Bước 1: Cấu hình netplug

Để sau khi boot máy ảo, có thể nhận đủ các NIC gắn vào:

```
apt-get install netplug -y

wget https://raw.githubusercontent.com/danghai1996/thuctapsinh/master/HaiDD/CreateImage/scripts/netplug_ubuntu -O netplug

mv netplug /etc/netplug/netplug

chmod +x /etc/netplug/netplug
```

### Bước 2: Disable snapd service

Kiểm tra Snap:

```
root@cloud:~# df -H
Filesystem      Size  Used Avail Use% Mounted on
udev            951M     0  951M   0% /dev
tmpfs           199M  916K  198M   1% /run
/dev/vda2       9.8G  2.4G  7.0G  26% /
tmpfs           994M     0  994M   0% /dev/shm
tmpfs           5.0M     0  5.0M   0% /run/lock
tmpfs           994M     0  994M   0% /sys/fs/cgroup
/dev/loop0       56M   56M     0 100% /snap/core18/1944
/dev/loop1       32M   32M     0 100% /snap/snapd/10707
/dev/loop2       70M   70M     0 100% /snap/lxd/19188
tmpfs           199M     0  199M   0% /run/user/0
```

```
root@cloud:~# snap list
Name    Version   Rev    Tracking       Publisher   Notes
core18  20201210  1944   latest/stable  canonical*  base
lxd     4.0.5     19188  4.0/stable/…   canonical*  -
snapd   2.48.2    10707  latest/stable  canonical*  snapd
```

Xóa Snap:

```
snap remove lxd
snap remove core18
snap remove snapd
```

Remove snapd package:

`apt purge snapd -y`

Xóa các thư mục snapd:

```
rm -rf ~/snap
rm -rf /snap
rm -rf /var/snap
rm -rf /var/lib/snapd
```

Kiểm tra lại:

```
root@cloud:~# df -H
Filesystem      Size  Used Avail Use% Mounted on
udev            998M     0  998M   0% /dev
tmpfs           209M  664k  208M   1% /run
/dev/vda2        11G  2.3G  7.8G  23% /
tmpfs           1.1G     0  1.1G   0% /dev/shm
tmpfs           5.3M     0  5.3M   0% /run/lock
tmpfs           1.1G     0  1.1G   0% /sys/fs/cgroup
tmpfs           209M     0  209M   0% /run/user/0
```

### Bước 3: Thiết lập Cloud-init

Cài đặt Cloud-init:

`apt-get install -y cloud-init`

Cấu hình user mặc định:

sed -i 's/name: ubuntu/name: root/g' /etc/cloud/cloud.cfg

Disable default config route:

`sed -i 's|link-local 169.254.0.0|#link-local 169.254.0.0|g' /etc/networks`

Cấu hình datasource, bỏ chọn mục NoCloud:

`dpkg-reconfigure cloud-init`

<img src="https://imgur.com/GeH9bA5.png">

Clean cấu hình và restart service:

```
cloud-init clean
systemctl restart cloud-init
systemctl enable cloud-init
systemctl status cloud-init
```

### Bước 4: Cài đặt qemu-agent

Chú ý: qemu-guest-agent là một daemon chạy trong máy ảo, giúp quản lý và hỗ trợ máy ảo khi cần (có thể cân nhắc việc cài thành phần này lên máy ảo)

Để có thể thay đổi password máy ảo bằng nova-set password thì phiên bản qemu-guest-agent phải `>= 2.5.0`

```
apt-get install software-properties-common -y
apt-get update -y
apt-get install qemu-guest-agent -y
service qemu-guest-agent start
```

Kiểm tra phiên bản qemu-ga bằng lệnh:

```
qemu-ga --version
service qemu-guest-agent status
```

### Bước 5: Cài đặt CMD Log

`curl -Lso- https://raw.githubusercontent.com/nhanhoadocs/ghichep-cmdlog/master/cmdlog.sh | bash`

### Bước 6: Dọn dẹp

```
apt-get clean all
rm -f /var/log/wtmp /var/log/btmp
history -c
> /var/log/cmdlog.log
```

### Bước 7: Tắt VM, Tạo Snapshot cho Ubuntu2004-Blank

<img src="https://imgur.com/zEf7cV7.png">