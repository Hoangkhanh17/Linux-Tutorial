# Brute Force Password với Medusa

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