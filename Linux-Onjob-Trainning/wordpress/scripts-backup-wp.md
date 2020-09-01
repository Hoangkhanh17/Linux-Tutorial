# Tạo Scripts Backup Source Code và Database WordPress

Để tạo Scripts Backup cho WordPress, ta nên tạo 2 file Scripts riêng biệt để Backup Source Code riêng và Backup cho Cơ Sở dữ liệu riêng.

Tạo thư mục làm nơi chứa các bản Backup:

`mkdir -p /backups/{backup-code,backup-db}`

Trong đó `/backup-code` để chứa Backup Source Code cho WordPress, `/backup-db` để chứa các bản Backup Cơ sở dữ liệu.

Để tạo Scripts Backup, ta tạo tệp có đuôi `.sh` để sử dụng

## 1. Backup Source WordPress

#### Bước 1: Tạo file Script

`vi /backups/backup-code/code-backup.sh`

Thêm nội dung dưới vào file vừa tạo:

```
#/bin/bash

SRCDIR="/var/www/html/" #Thư mục chứa Source Code
DESTDIR="/backups/backup-code/" #Thư mục ta sẽ lưu các bản Backup
FILENAME=wpsourcecode-$(date +%-Y%-m%-d)-$(date +%-T).tgz #Nén bản backup lại để tiết kiệm dung lượng
tar --create --gzip --file=$DESTDIR$FILENAME $SRCDIR
```

#### Bước 2: Gán quyền thực thi cho tệp Script vừa tạo

`chmod +x /backups/backup-code/code-backup.sh`

Sau khi tiến hành backup, file back up sẽ có dạng `wpsourcecode-202091-08:09:40.tgz` 

## 2. Backup Database

#### Bước 1: Tạo file Script cho Cơ sở dữ liệu

`vi /etc/backups/backup-db/db.sh`

Ta thêm nội dung sau:

```
NOW=$(date +"%Y-%m-%d-%H%M")
BACKUP_DIR="/backups/backup-db/"

DB_USER="root"
DB_PASS="Quanganh1234"
DB_NAME="wordpress"
DB_FILE="wordpress.$NOW.sql"

mysqldump -u$DB_USER -p$DB_PASS $DB_NAME > $BACKUP_DIR/$DB_FILE
```

#### Bước 2: Gán quyền thực thi cho tệp Script vừa tạo

`chmod +x chmod +x /backups/backup-db/db.sh`

## 3. Lưu vào Crontab để tự động chạy Scripts hàng ngày

`crontab -e`

```
0 8 * * * /backups/backup-code/code-backup.sh
0 8 * * * /backups/backup-db/db.sh
```

Hai Scripts vừa tạo sẽ được chạy tự động lúc 8 giờ sáng mỗi ngày.

### Chúc các bạn thành công !