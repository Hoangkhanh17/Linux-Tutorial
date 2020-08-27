# Sử dụng Fail2ban để bảo mật máy chủ CentOS

## Mục lục

[1. Giới thiệu Fail2ban](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob-Trainning/Security-and-Firewall/Fail2ban-with-CentOS7.md#1-gi%E1%BB%9Bi-thi%E1%BB%87u-fail2ban)

[2. Cài đặt và cấu hình Fail2ban](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob-Trainning/Security-and-Firewall/Fail2ban-with-CentOS7.md#2-c%C3%A0i-%C4%91%E1%BA%B7t-v%C3%A0-c%E1%BA%A5u-h%C3%ACnh-fail2ban)

- [2.1. Cài đặt Fail2ban](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob-Trainning/Security-and-Firewall/Fail2ban-with-CentOS7.md#21-c%C3%A0i-%C4%91%E1%BA%B7t-fail2ban)

- [2.2. Cấu hình Fail2ban](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob-Trainning/Security-and-Firewall/Fail2ban-with-CentOS7.md#22-c%E1%BA%A5u-h%C3%ACnh-fail2ban)

[3. Giám sát cấu hình firewall và log fail2ban](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob-Trainning/Security-and-Firewall/Fail2ban-with-CentOS7.md#3-gi%C3%A1m-s%C3%A1t-c%E1%BA%A5u-h%C3%ACnh-firewall-v%C3%A0-log-fail2ban)

[4. Giám sát SSH login](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob-Trainning/Security-and-Firewall/Fail2ban-with-CentOS7.md#4-gi%C3%A1m-s%C3%A1t-ssh-login)

[Tài liệu tham khảo](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob-Trainning/Security-and-Firewall/Fail2ban-with-CentOS7.md#t%C3%A0i-li%E1%BB%87u-tham-kh%E1%BA%A3o)

## 1. Giới thiệu Fail2ban

Để kết nối với VPS chúng ta thường sử dụng port 22. Đây chính là lỗ hổng chết người các hacker có thể sử dụng để dò tìm password đăng nhập vào VPS.

Một biện pháp hạn chế việc này đó là thay đổi port SSH từ 22 sang một port khác. Tuy nhiên, việc này chỉ hạn chế một chút thôi vì nếu muốn, hacker có thể scan open port để biết được bạn đang sử dụng port nào để tấn công tiếp.

Giải pháp để chúng ta chấm dứt vấn đề này đó là sử dụng một công cụ tự động block IP khi VPS bị tấn công, đó là **Fail2Ban**.

**Fail2Ban** là một ứng dụng chạy nền theo dõi log file để phát hiện những địa chỉ IP đăng nhập sai password SSH nhiều lần. Sau đó, **Fail2Ban** sử dụng iptable firewall rules để block ngay địa chỉ IP với một khoảng thời gian định trước.

## 2. Cài đặt và cấu hình Fail2ban

### 2.1. Cài đặt Fail2ban

#### Bước 1: Đảm bảo hệ thống của bạn đã được cập nhật thời gian và cài đặt EPEL repository

`[root@srv-thuctapanh ~]# yum update && yum install epel-release`

#### Bước 2: Cài đặt fail2ban, sau đó bật fail2ban khởi động cùng hệ thống

```
[root@srv-thuctapanh ~]# yum -y install fail2ban
[root@srv-thuctapanh ~]# systemctl start fail2ban
[root@srv-thuctapanh ~]# systemctl enable fail2ban
Created symlink from /etc/systemd/system/multi-user.target.wants/fail2ban.service to /usr/lib/systemd/system/fail2ban.service.
```

Vậy là đã cài đặt Fail2ban thành công.

### 2.2. Cấu hình Fail2ban

#### Một số thông số đáng chú ý

Truy cập vào thư mục cấu hình của fail2ban:

`[root@srv-thuctapanh fail2ban]# cat /etc/fail2ban/jail.conf`

```
[DEFAULT]

# "ignoreip" can be an IP address, a CIDR mask or a DNS host. Fail2ban will not
# ban a host which matches an address in this list. Several addresses can be
# defined using space separator.
ignoreip = 127.0.0.1

# "bantime" is the number of seconds that a host is banned.
bantime = 600

# A host is banned if it has generated "maxretry" during the last "findtime"
# seconds.
findtime = 600

# "maxretry" is the number of failures before a host get banned.
maxretry = 3
```

Trong đó:

- **ignoreip:** không block những địa chỉ này, thường địa chỉ IP ở VN là địa chỉ động, nên chúng ta không sử dụng được option này.

- **bantime:** khoảng thời gian (giây) block IP.

- **findtime:** khoảng thời gian (giây) một IP phải login thành công.

- **maxretry:** số lần login false tối đa.

#### Cấu hình fail2ban cho SSH

Tạo file cấu hình:

`[root@srv-thuctapanh fail2ban]# vi /etc/fail2ban/jail.local`

```
[sshd]
enabled  = true
filter   = sshd
action   = iptables[name=SSH, port=ssh, protocol=tcp]
logpath  = /var/log/secure
maxretry = 5
bantime = 3600
```

Trong đó:

- **enabled:** kích hoạt bảo vệ

- **filter:** giữ mặc định để sử dụng file cấu hình /etc/fail2ban/filter.d/sshd.conf

- **action:** fail2ban sẽ ban địa chỉ IP nếu match filter /etc/fail2ban/action.d/iptables.conf

- **logpath:** đường dẫn file log fail2ban

- **maxretry:** số lần login false tối đa

- **bantime:** thời gian ban IP 3600 giây = 1 giờ, bạn có thể chỉnh nếu muốn.

Kiểm tra rules của fail2ban trên iptables

```
[root@srv-thuctapanh fail2ban]# iptables -L
Chain INPUT (policy ACCEPT)
target     prot opt source               destination
f2b-SSH    tcp  --  anywhere             anywhere             tcp dpt:ssh

Chain FORWARD (policy ACCEPT)
target     prot opt source               destination

Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination

Chain f2b-SSH (1 references)
target     prot opt source               destination
REJECT     all  --  218.92.0.190         anywhere             reject-with icmp-port-unreachable
REJECT     all  --  222.186.180.130      anywhere             reject-with icmp-port-unreachable
REJECT     all  --  222.186.30.112       anywhere             reject-with icmp-port-unreachable
RETURN     all  --  anywhere             anywhere
```

Trong đó `Chain f2b-SSH` là rules của Fail2ban, mặc định ban 3 IP của Trung Quốc và cho phép các IP còn lại truy cập.

#### Cấu hình chống DDOS cho Apache

Chúng ta cũng truy cập vào `vi /etc/fail2ban/jail.local` để thêm cấu hình:

```
# detect password authentication failures
[apache]
enabled  = true
filter   = apache-auth
port     = http,https
action   = iptables-multiport[name=auth, port="http,https"]
logpath  = /var/log/secure
bantime  = 3600
maxretry = 10
ignoreip = 117.5.255.125 42.115.206.150 127.0.0.1

# detect spammer robots crawling email addresses
[apache-badbots]
enabled  = true
filter   = apache-badbots
port     = http,https
action   = iptables-multiport[name=badbots, port="http,https"]
logpath  = /var/log/secure
bantime  = 3600
maxretry = 10
ignoreip = 117.5.255.125 42.115.206.150 127.0.0.1

# detect potential search for exploits
[apache-noscript]
enabled  = true
filter   = apache-noscript
port     = http,https
action   = iptables-multiport[name=noscript, port="http,https"]
logpath  = /var/log/secure
bantime  = 3600
maxretry = 10
ignoreip = 117.5.255.125 42.115.206.150 127.0.0.1

# detect Apache overflow attempts
[apache-overflows]
enabled  = true
filter   = apache-overflows
port     = http,https
action   = iptables-multiport[name=overflows, port="http,https"]
logpath  = /var/log/secure
bantime  = 3600
maxretry = 10
ignoreip = 117.5.255.125 42.115.206.150 127.0.0.1

##To stop DOS attack from remote host
[http-get-dos]
enabled  = true
port     = http,https
filter   = http-get-dos
logpath  = /var/log/secure
maxretry = 10
bantime  = 3600
ignoreip = 117.5.255.125 42.115.206.150 127.0.0.1
action   = iptables[name=HTTP, port=http, protocol=tcp
```

## 3. Giám sát cấu hình firewall và log fail2ban

- Kiểm tra trạng thái dịch vụ

`systemctl status fail2ban`

- Kiểm tra log của fail2ban:

`[root@srv-thuctapanh fail2ban]# sudo journalctl -b -u fail2ban`

- Kiểm tra log để biết các hành động gần đây của fail2ban

`[root@srv-thuctapanh fail2ban]# tail -F /var/log/fail2ban.log`

## 4. Giám sát SSH login

- Giám sát xem VPS/Server có bị tấn công hay chưa:

`[root@srv-thuctapanh fail2ban]# cat /var/log/secure | grep 'Failed password' | sort | uniq -c`

- Kiểm tra các IP bị banned bới Fail2ban:

```
[root@srv-thuctapanh fail2ban]# fail2ban-client status sshd
Status for the jail: sshd
|- Filter
|  |- Currently failed: 1
|  |- Total failed:     49
|  `- Journal matches:  _SYSTEMD_UNIT=sshd.service + _COMM=sshd
`- Actions
   |- Currently banned: 3
   |- Total banned:     3
   `- Banned IP list:   222.186.30.112 222.186.180.130 218.92.0.190
```

- Xóa IP khỏi danh sách banned:

`[root@srv-thuctapanh fail2ban]# fail2ban-client set sshd unbanip IPADDRESS`

## Tài liệu tham khảo

https://hocvps.com/cai-dat-fail2ban-tren-centos/

https://news.cloud365.vn/cach-su-dung-fail2ban-de-bao-mat-may-chu-centos/#1.-Gi%E1%BB%9Bi-thi%E1%BB%87u

