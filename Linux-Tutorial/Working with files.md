# Luồng dữ liệu trong Linux

## 1. Các luồng dữ liệu

- ***Standard input/standard in/stdin***: Dữ liệu truyền vào để process xử lý. Có thể là dữ liệu nhập từ bàn phím, tập tin, hay output của process khác.

- ***Standard output/standard out/stdout***: Kết quả sau khi xử lý, thường là kết quả xuất ra màn hình (terminal). Hoặc có thể ghi vào tập tin, hoặc truyền cho process khác xử lý tiếp.

- ***Standard error/stderr***: Lỗi khi process xử lý, thường cũng xuất ra màn hình hoặc có thể ghi vào tập tin, hoặc truyền cho process khác xử lý.

<img src="https://i0.wp.com/trungquan710.com/wp-content/uploads/2015/07/linux_process_data_streams.png">


## 2. Thao tác điều khiển Output trong Linux

### **Ghi Output ra file (> hoặc 1>):**

```
root@ubuntu:/tmp# ls -l >outputtest.log
root@ubuntu:/tmp# cat outputtest.log
total 28
-rw-r--r-- 1 root root    0 Aug 12 20:50 outputtest.log
drwx------ 3 root root 4096 Aug 12 19:59 systemd-private-30439cce77574714bb38e03822fbf50b-systemd-networkd.service-KLsRPt
drwx------ 3 root root 4096 Aug 12 19:59 systemd-private-30439cce77574714bb38e03822fbf50b-systemd-resolved.service-QCT3T9
drwx------ 3 root root 4096 Aug 12 19:59 systemd-private-30439cce77574714bb38e03822fbf50b-systemd-timesyncd.service-mTRMmR
drwxrwxrwt 2 root root 4096 Aug 12 19:59 VMwareDnD
drwx------ 2 root root 4096 Aug 12 19:59 vmware-root
drwx------ 2 root root 4096 Aug 12 19:59 vmware-root_905-4013330159
-rw-r--r-- 1 root root  208 Aug 12 20:47 xyz.log
```

### **Thêm Output vào file (>> hoặc 1>>):**

```
root@ubuntu:/tmp# df -h >>outputtest.log
root@ubuntu:/tmp# cat outputtest.log
total 28
-rw-r--r-- 1 root root    0 Aug 12 20:50 outputtest.log
drwx------ 3 root root 4096 Aug 12 19:59 systemd-private-30439cce77574714bb38e03822fbf50b-systemd-networkd.service-KLsRPt
drwx------ 3 root root 4096 Aug 12 19:59 systemd-private-30439cce77574714bb38e03822fbf50b-systemd-resolved.service-QCT3T9
drwx------ 3 root root 4096 Aug 12 19:59 systemd-private-30439cce77574714bb38e03822fbf50b-systemd-timesyncd.service-mTRMmR
drwxrwxrwt 2 root root 4096 Aug 12 19:59 VMwareDnD
drwx------ 2 root root 4096 Aug 12 19:59 vmware-root
drwx------ 2 root root 4096 Aug 12 19:59 vmware-root_905-4013330159
-rw-r--r-- 1 root root  208 Aug 12 20:47 xyz.log
Filesystem      Size  Used Avail Use% Mounted on
udev            963M     0  963M   0% /dev
tmpfs           197M  836K  197M   1% /run
/dev/sda1        40G  3.5G   34G  10% /
tmpfs           985M     0  985M   0% /dev/shm
tmpfs           5.0M     0  5.0M   0% /run/lock
tmpfs           985M     0  985M   0% /sys/fs/cgroup
tmpfs           197M     0  197M   0% /run/user/1000
```

### **Ghi output ra file (>, >>, 1>, 1>>) và lỗi nếu có vào file khác (2> hoặc 2>>):**

```
root@ubuntu:/tmp# ls -l /tmp/abcde >testt.log 2>testerror.log
root@ubuntu:/tmp# cat testt.log
root@ubuntu:/tmp# cat testerror.log
ls: cannot access '/tmp/abcde': No such file or directory
```

### **Không ghi lỗi (2>/dev/null)**

```
root@ubuntu:/tmp# ls -l /tmp/acsavas
ls: cannot access '/tmp/acsavas': No such file or directory
root@ubuntu:/tmp# ls -l /tmp/acsavas 2>/dev/null
root@ubuntu:/tmp#
```

### **Ghi output ra cả màn hình và file (tee)**

```
root@ubuntu:/tmp# ls -l | tee logtest.log
total 44
-rw-r--r-- 1 root root 1029 Aug 12 20:57 outputtest.log
-rw-r--r-- 1 root root  868 Aug 12 21:08 outtput.log
drwx------ 3 root root 4096 Aug 12 19:59 systemd-private-30439cce77574714bb38e03822fbf50b-systemd-networkd.service-KLsRPt
drwx------ 3 root root 4096 Aug 12 19:59 systemd-private-30439cce77574714bb38e03822fbf50b-systemd-resolved.service-QCT3T9
drwx------ 3 root root 4096 Aug 12 19:59 systemd-private-30439cce77574714bb38e03822fbf50b-systemd-timesyncd.service-mTRMmR
-rw-r--r-- 1 root root   58 Aug 12 21:05 testerror.log
-rw-r--r-- 1 root root  376 Aug 12 21:02 testoutput.log
-rw-r--r-- 1 root root    0 Aug 12 21:05 testt.log
drwxrwxrwt 2 root root 4096 Aug 12 19:59 VMwareDnD
drwx------ 2 root root 4096 Aug 12 19:59 vmware-root
drwx------ 2 root root 4096 Aug 12 19:59 vmware-root_905-4013330159
-rw-r--r-- 1 root root  208 Aug 12 20:47 xyz.log
root@ubuntu:/tmp# cat logtest.log
total 44
-rw-r--r-- 1 root root 1029 Aug 12 20:57 outputtest.log
-rw-r--r-- 1 root root  868 Aug 12 21:08 outtput.log
drwx------ 3 root root 4096 Aug 12 19:59 systemd-private-30439cce77574714bb38e03822fbf50b-systemd-networkd.service-KLsRPt
drwx------ 3 root root 4096 Aug 12 19:59 systemd-private-30439cce77574714bb38e03822fbf50b-systemd-resolved.service-QCT3T9
drwx------ 3 root root 4096 Aug 12 19:59 systemd-private-30439cce77574714bb38e03822fbf50b-systemd-timesyncd.service-mTRMmR
-rw-r--r-- 1 root root   58 Aug 12 21:05 testerror.log
-rw-r--r-- 1 root root  376 Aug 12 21:02 testoutput.log
-rw-r--r-- 1 root root    0 Aug 12 21:05 testt.log
drwxrwxrwt 2 root root 4096 Aug 12 19:59 VMwareDnD
drwx------ 2 root root 4096 Aug 12 19:59 vmware-root
drwx------ 2 root root 4096 Aug 12 19:59 vmware-root_905-4013330159
-rw-r--r-- 1 root root  208 Aug 12 20:47 xyz.log
```

### **Ghi log lại toàn bộ thao tác:**

```
root@ubuntu:/tmp# script testxyt.log
Script started, file is testxyt.log
root@ubuntu:/tmp# helo

Command 'helo' not found, did you mean:

  command 'hello' from deb hello
  command 'hello' from deb hello-traditional

Try: apt install <deb name>

root@ubuntu:/tmp# df -h
Filesystem      Size  Used Avail Use% Mounted on
udev            963M     0  963M   0% /dev
tmpfs           197M  836K  197M   1% /run
/dev/sda1        40G  3.5G   34G  10% /
tmpfs           985M     0  985M   0% /dev/shm
tmpfs           5.0M     0  5.0M   0% /run/lock
tmpfs           985M     0  985M   0% /sys/fs/cgroup
tmpfs           197M     0  197M   0% /run/user/1000
root@ubuntu:/tmp# exit
exit
Script done, file is testxyt.log
```

## Tìm kiếm các tệp tin

Lệnh `locate` được sử dụng để thực hiện việc tìm kiếm hông qua cơ sở dữ liệu tệp và thư mục được xây dựng trước đó trên hệ thống của bạn, khớp với tất cả các mục nhập có chứa chuỗi ký tự được chỉ định.

**Ví dụ:** Muốn tìm kiếm các tệp tin hoặc thư mục có chứa `mysql`

```
root@ubuntu:~# locate mysql
/etc/apparmor.d/abstractions/mysql
/usr/share/bash-completion/completions/mysql
/usr/share/bash-completion/completions/mysqladmin
```

Đôi khi việc tìm kiếm sẽ cho ra danh sách kết quả rất dài. Ta có thể sử dụng thêm lệnh `grep` để tiến hành lọc kết quả mong muốn.

**Ví dụ:** Tìm kiếm các tệp và thư mục `zip` có chứa `bin` trong tên.

```
root@ubuntu:~# locate zip | grep bin
/bin/bunzip2
/bin/bzip2
/bin/bzip2recover
/bin/gunzip
/bin/gzip
/lib/firmware/qed/qed_init_values_zipped-8.10.10.0.bin
/lib/firmware/qed/qed_init_values_zipped-8.10.5.0.bin
/lib/firmware/qed/qed_init_values_zipped-8.15.3.0.bin
/lib/firmware/qed/qed_init_values_zipped-8.20.0.0.bin
/lib/firmware/qed/qed_init_values_zipped-8.33.1.0.bin
/lib/firmware/qed/qed_init_values_zipped-8.33.11.0.bin
/lib/firmware/qed/qed_init_values_zipped-8.37.2.0.bin
/lib/firmware/qed/qed_init_values_zipped-8.37.7.0.bin
/lib/firmware/qed/qed_init_values_zipped-8.4.2.0.bin
/lib/firmware/qed/qed_init_values_zipped-8.7.3.0.bin
/usr/lib/klibc/bin/gunzip
/usr/lib/klibc/bin/gzip
```

Tìm kiếm các tệp tên chứa các ký tự cụ thể:

|Wildcards| Results |
|---------|---------|
|    ?    |Tìm kiếm với bất kỳ ký tự đơn nào|
|    *    |Tìm kiếm với bất kỳ chuỗi ký tự nào|
|  [set]  |Tìm kiếm bất kỳ ký tự nào không có trong bộ ký tự|
|  [!set] |Tìm kiếm bất kỳ ký tự nào không có trong bộ ký tự|

Sử dụng lệnh `find` để tìm kiếm từ bất kỳ thư mục cụ thể nào và định vị các tệp phù hợp với các điều kiện được chỉ định. Lệnh mặc định là tại thư mục hiện hành.

```
[root@localhost ~]# find /var --help
Usage: find [-H] [-L] [-P] [-Olevel] [-D help|tree|search|stat|rates|opt|exec] [path...] [expression]

default path is the current directory; default expression is -print
expression may consist of: operators, options, tests, and actions:

operators (decreasing precedence; -and is implicit where no others are given):
      ( EXPR )   ! EXPR   -not EXPR   EXPR1 -a EXPR2   EXPR1 -and EXPR2
      EXPR1 -o EXPR2   EXPR1 -or EXPR2   EXPR1 , EXPR2

positional options (always true): -daystart -follow -regextype

normal options (always true, specified before other expressions):
      -depth --help -maxdepth LEVELS -mindepth LEVELS -mount -noleaf
      --version -xautofs -xdev -ignore_readdir_race -noignore_readdir_race

tests (N can be +N or -N or N): -amin N -anewer FILE -atime N -cmin N
      -cnewer FILE -ctime N -empty -false -fstype TYPE -gid N -group NAME
      -ilname PATTERN -iname PATTERN -inum N -iwholename PATTERN -iregex PATTERN
      -links N -lname PATTERN -mmin N -mtime N -name PATTERN -newer FILE
      -nouser -nogroup -path PATTERN -perm [-/]MODE -regex PATTERN
      -readable -writable -executable
      -wholename PATTERN -size N[bcwkMG] -true -type [bcdpflsD] -uid N
      -used N -user NAME -xtype [bcdpfls]
      -context CONTEXT


actions: -delete -print0 -printf FORMAT -fprintf FILE FORMAT -print
      -fprint0 FILE -fprint FILE -ls -fls FILE -prune -quit
      -exec COMMAND ; -exec COMMAND {} + -ok COMMAND ;
      -execdir COMMAND ; -execdir COMMAND {} + -okdir COMMAND ;
```

**Ví dụ:** Tìm kiếm các tệp tin trong thư mục `/var` có tên bất kỳ và đuôi `.log`

```
[root@localhost ~]# find /var -name *.log
/var/lib/mysql/tc.log
/var/log/tuned/tuned.log
/var/log/audit/audit.log
/var/log/anaconda/anaconda.log
/var/log/anaconda/X.log
/var/log/anaconda/program.log
/var/log/anaconda/packaging.log
/var/log/anaconda/storage.log
/var/log/anaconda/ifcfg.log
/var/log/anaconda/ks-script-KT6XZr.log
/var/log/anaconda/ks-script-WImM2h.log
/var/log/anaconda/journal.log
/var/log/boot.log
/var/log/vmware-vmsvc.log
/var/log/yum.log
/var/log/nginx/access.log
/var/log/nginx/error.log
/var/log/vmware-network.8.log
/var/log/vmware-network.7.log
/var/log/vmware-network.6.log
/var/log/vmware-network.5.log
/var/log/vmware-network.4.log
/var/log/vmware-network.3.log
/var/log/vmware-network.2.log
/var/log/vmware-network.1.log
/var/log/vmware-network.log
```


## Refrences - Special thanks !

https://trungquan710.com/linux/luong-du-lieu-doi-voi-linux-process.html

https://github.com/MinhKMA/Linux-Tutorial/blob/master/content/working_with_files.md