# Tìm hiểu về Crontab trên Linux

## Phần 1. Tổng quan về Crontab

### 1.1.  Crontab là gì ?

Crontab (CRON TABLE) là một tiện ích cho phép thực hiện các tác vụ một cách tự động theo định kỳ, ở chế độ nền của hệ thống. Crontab là một file chứa đựng bảng biểu (schedule) của các entries được chạy.

Bằng cách sử dụng các lệnh trong Linux Crontab ta có thể tạo những task chạy vào những giờ cụ thể đặt trước, như vào giờ nào trong ngày, vào giờ nào trong ngày vào thứ mấy trong tuần….

### 1.2. Cách thức hoạt động của Crontab

Một cron schedule đơn giản là một text file. Mỗi người dùng có một cron schedule riêng, file này thường nằm ở /var/spool/cron. Crontab files không cho phép bạn tạo hoặc chỉnh sửa trực tiếp với bất kỳ trình text editor nào, trừ phi bạn dùng lệnh crontab.

Một số lệnh thường dùng

```
crontab -e: tạo,  chỉnh sửa các crontab
crontab -l: Xem các Crontab đã tạo
crontab -r: xóa file crontab
```

### 1.3. Cài đặt Crontab

Hầu hết các Hệ điều hành Linux đều cài đặt sẵn Crontab. Cách kiểm tra Hệ điều hành Linux ta sử dụng đã cài đặt chưa:

`crontab -l` -> Kết quả trả về `-bash: crontab: command not found` thì ta sẽ cần cài đặt Crontab thủ công.

- Cài đặt Crontab:

`yum -y install cronie`

- Bật và cho phép Crontab khởi động cùng hệ thống:

```
service crond start
chkconfig crond on
```

## Phần 2. Cấu trúc của Crontab

### 2.1. Cấu trúc hoạt động của Crontab

Một crontab file có 5 trường xác định thời gian, cuối cùng là lệnh sẽ được chạy định kỳ, cấu trúc như sau:

```
*     *     *     *     *     command to be executed
-     -     -     -     -
|     |     |     |     |
|     |     |     |     +----- day of week (0 - 6) (Sunday=0)
|     |     |     +------- month (1 - 12)
|     |     +--------- day of month (1 - 31)
|     +----------- hour (0 - 23)
+------------- min (0 - 59)
```

Nếu một cột được gán ký tự *, nó có nghĩa là tác vụ sau đó sẽ được chạy ở mọi giá trị cho cột đó.

| Field | Giải thích       | Giá trị cho phép          |
|-------|------------------|---------------------------|
| MIN   | phút             | 0 to 59                   |
| HOUR  | Giờ              | 0 to 23                   |
| DOM   | Ngày trong tháng | 1-31                      |
| MON   | Tháng            | 1-12                      |
| DOW   | Ngày trong tuần  | 0-6                       |
| CMD   | Lệnh             | Các lệnh có thể thực hiện |

### 2.2. Một số ví dụ

- Chạy script 30 phút 1 lần:

`30 * * * * command`

- Chạy script vào 3 giờ sáng mỗi ngày

`0 3 * * * command`

- Tạo một tác vụ hoạt động vào một giờ cụ thể. Ví dụ: Backup dữ liệu vào ngày 07 tháng 10 lúc 08:11 AM

`11 08 10 07 * /backups/backup-code/code-backup.sh`

Trong đó: - 11 – phút 11

          - 08 – lúc 8 giờ

          - 10 – ngày mùng 10

          - 07 – tháng 07

- Tạo 1 tác vụ thực hiện 2 lần trong một ngày. Ví dụ: Backup dữ liệu 2 lần trong một ngày lúc 7:00 và 21:00 hàng ngày.

`00 07,21  * * * /backups/backup-code/code-backup.sh`

Trong đó: - 00 – phút 00

          - 07,21: 07 giờ sáng và 21 giờ tối

          - Hàng ngày

          - Hàng tháng

          - Tất cả các ngày trong tuần

### 2.2. Một số giá trị thời gian cho Crontab

| Keyword | Equivalent          |
|---------|---------------------|
| @yearly | 0 0 1 1 *           |
| @daily  | 0 0 * * *           |
| @hourly | 0 * * * *           |
| @reboot | chạy lúc khởi động. |

Ví dụ:

- Tạo một tác vụ chạy vào phút đầu tiên của năm:

`@yearly /backups/backup-code/code-backup.sh`

- Tạo một tác vụ chạy vào phút đầu tiên của tháng

`@monthly /backups/backup-code/code-backup.sh`

- Tạo một tác vụ chạy khi khởi động lại

`@reboot CMD`

## Tài liệu tham khảo

https://viblo.asia/p/tim-hieu-crontab-tren-linux-WApGx3DbM06y