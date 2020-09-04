# Tìm hiểu về /etc/proc - Process Information

## Mục lục

## 1. Hệ thống file /proc ( proc FS)

Chứa các thông tin về System Process.

Proc là hệ thống file ảo (pseudo file system), một hệ thống file thời gian thực (real time) và thường trú trong bộ nhớ (memory resident) để theo dõi các process đang chạy cùng với trạng thái của hệ thống.

Proc là hệ thống file ảo bởi vì trên thực tế nó không tồn tại trong bất kì phương tiện lưu trữ nào. Nó tồn tại dựa trên bộ nhớ ảo và dữ liệu luôn thay đổi động cùng với trạgn thái của hệ thống. Hầu hết các dữ liệu trong proc FS được cập nhật liên tục để phù hợp với trạng thái hiện tại của hệ điều hành. Nội dung của proc FS có thể được đọc bởi user có quyền thích hợp, trong đó một số phần chỉ dó thể đọc bởi owner của process và root. Nếu liệt kê thư mục root (/) ra bạn sẽ thấy

```
[root@anhtq-test ~]# ls -al /
total 72
dr-xr-xr-x.  19 root root  4096 Sep  4 14:14 .
dr-xr-xr-x.  19 root root  4096 Sep  4 14:14 ..
-rw-r--r--    1 root root     0 Oct 30  2019 .autorelabel
drwxr-xr-x    6 root root  4096 Sep  4 15:31 backup
lrwxrwxrwx.   1 root root     7 Oct 30  2019 bin -> usr/bin
dr-xr-xr-x.   5 root root  4096 Sep  4 15:10 boot
drwxr-xr-x   19 root root  2960 Sep  4 14:07 dev
drwxr-xr-x.  84 root root  4096 Sep  4 16:01 etc
drwxr-xr-x.   3 root root  4096 Sep  4 15:36 home
lrwxrwxrwx.   1 root root     7 Oct 30  2019 lib -> usr/lib
lrwxrwxrwx.   1 root root     9 Oct 30  2019 lib64 -> usr/lib64
drwx------.   2 root root 16384 Oct 30  2019 lost+found
drwxr-xr-x.   2 root root  4096 Apr 11  2018 media
drwxr-xr-x.   2 root root  4096 Apr 11  2018 mnt
drwxr-xr-x.   2 root root  4096 Sep  4 14:09 opt
dr-xr-xr-x  112 root root     0 Sep  4 14:07 proc
dr-xr-x---.   4 root root  4096 Sep  4 15:03 root
drwxr-xr-x   26 root root   740 Sep  4 16:07 run
lrwxrwxrwx.   1 root root     8 Oct 30  2019 sbin -> usr/sbin
drwxr-xr-x.   2 root root  4096 Apr 11  2018 srv
dr-xr-xr-x   13 root root     0 Sep  4 14:07 sys
drwxrwxrwt.   9 root root  4096 Sep  4 16:22 tmp
drwxr-xr-x.  13 root root  4096 Oct 30  2019 usr
drwxr-xr-x.  20 root root  4096 Oct 30  2019 var
```

Thư mục proc có kích thước luôn = 0 và thời điểm modify cuối cùng là thời điểm hiện tại.

## 2. Nội dung thư mục /proc

Sử dụng lệnh `ls /proc` sẽ thấy tập tin và các thư mục:

```
[root@anhtq-test ~]# ls /proc
1      17     21518  24     282  32   434  56    8312  9          diskstats    kallsyms    misc          slabinfo       version
10     17757  21544  247    285  33   436  574   8318  9951       dma          kcore       modules       softirqs       vmallocinfo
10040  18     21545  25     286  34   488  58    8319  acpi       driver       keys        mounts        stat           vmstat
11     19     21554  25284  29   35   495  6     8321  buddyinfo  execdomains  key-users   mtrr          swaps          zoneinfo
117    2      21555  25583  292  367  496  7     8322  bus        fb           kmsg        net           sys
12     21     21627  26     294  386  498  71    8323  cgroups    filesystems  kpagecount  pagetypeinfo  sysrq-trigger
12396  21106  21639  26209  30   4    5    8     871   cmdline    fs           kpageflags  partitions    sysvipc
1289   21414  21641  26225  300  40   51   8255  8869  consoles   interrupts   loadavg     sched_debug   timer_list
13     21511  21670  27     305  41   53   8285  8888  cpuinfo    iomem        locks       schedstat     timer_stats
14     21515  23     28     306  42   54   8286  8916  crypto     ioports      mdstat      scsi          tty
16     21516  233    281    31   43   558  8308  8917  devices    irq          meminfo     self          uptime
```

Một số nội dung và mô tả ngắn gọn:

-------------------------------------------------

file:/proc/cmdline == Kernel Command line (eg . “/rhgb”, “quite” etc.

file:/proc/cpuinfo == used cpus and various info about them

file:/proc/crypto == current password encription (such as MD5)

file:/proc/devices == available character(eg terminal,sound etc.) and block device name and corresponding number

file:/proc/diskstats == status about all diskdevice including ramdrive (such as free space,etc.)

file:/proc/dma == dma status (such as cascade)

file:/proc/execdomains == don’t know, maybe about kernel domain !

file:/proc/fb == maybe about any pending mails

file:/proc/filesystems == available filesystems (such as : ext3,vfat,etc.)

file:/proc/interrupts == available and usable interrupts and their number

file:/proc/iomem == kernel ram info (may be about available memory mapped io space)

file:/proc/ioports == available system io ports (such as timer,pci,vga+ , etc)

file:/proc/kcore == You can not access it as it is used by kernel itself

file:/proc/kmsg == Kernel Messages

file:/proc/loadavg == CPU Loads and uptime

file:/proc/locks

file:/proc/mdstat

file:/proc/meminfo == details about memory , including swap,cache,physical memory size and available free space

file:/proc/misc

file:/proc/modules == loded kernel modules (such as snd,alsa,hw,gameport etc.)

file:/proc/mounts ==mounted partitions

file:/proc/mtrr

file:/proc/partitions == Available Disk Partitions (eg. hda,hda1,…,hdb,.. etc.)

file:/proc/pci == pci devices and their corresponding Bus no.,devive no.,function no. ,irq no. etc.

file:/proc/slabinfo ==

file:/proc/stat == cpu usage info , Includes per cpu load (eg. cpu0,cpu1,…) , context switches , uptime etc.

file:/proc/swaps == Swap partition drive name and size etc.

file:/proc/sysrq-trigger

file:/proc/uptime == Uptime (times elapsed since boot) for individule cpus

file:/proc/version == Linux Kernel version and compiler used to build it

file:/proc/vmstat == Details Virtual memory Info ,including page faults,range,etc.

-------------------------------------------------

Các thư mục được đánh số như 1, 4567, 2385, 112, 40, 41 … chính là các Process ID (PID) của những process đang chạy trong hệ thống. Mỗi thư mục sẽ chứa thông tin về process đó. Bạn có thể dùng lệnh “ps –ef” để liệt kê các process đang chạy và so sánh với tên các thư mục trên để biết thư mục nào chứa thông tin của process nào.

Một số process khi sử dụng lệnh `ps -ef`:

<img src="https://imgur.com/zzAyB4l.png">

## Tài liệu tham khảo

https://tldp.org/LDP/Linux-Filesystem-Hierarchy/html/proc.html