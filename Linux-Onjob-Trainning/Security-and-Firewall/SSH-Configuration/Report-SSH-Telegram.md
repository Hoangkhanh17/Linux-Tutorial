# Cấu hình gửi report tới Telegram khi có truy cập SSH

## Mục lục

[1. Tạo một Telegram bot](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob-Trainning/Security-and-Firewall/SSH-Configuration/Report-SSH-Telegram.md#1-t%E1%BA%A1o-m%E1%BB%99t-telegram-bot)

- [Bước 1: Tạo 1 Telegram bot](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob-Trainning/Security-and-Firewall/SSH-Configuration/Report-SSH-Telegram.md#b%C6%B0%E1%BB%9Bc-1-t%E1%BA%A1o-1-telegram-bot)

- [Bước 2: Thêm bot vào group](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob-Trainning/Security-and-Firewall/SSH-Configuration/Report-SSH-Telegram.md#b%C6%B0%E1%BB%9Bc-2-th%C3%AAm-bot-v%C3%A0o-group)

- [Bước 3: Lấy chat_id](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob-Trainning/Security-and-Firewall/SSH-Configuration/Report-SSH-Telegram.md#b%C6%B0%E1%BB%9Bc-3-l%E1%BA%A5y-chat_id)

- [Bước 4: Kiểm tra cảnh báo tới Telegram qua API](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob-Trainning/Security-and-Firewall/SSH-Configuration/Report-SSH-Telegram.md#b%C6%B0%E1%BB%9Bc-3-l%E1%BA%A5y-chat_id)

## 1. Tạo một Telegram bot

### Bước 1: Tạo 1 Telegram bot

- Trên thanh tìm kiếm của **Telegram**, tìm đến người dùng **BotFather**

- Chat `/newbot` với BotFather.

- Đặt tên cho bot của bạn. Ở đây ví dụ đặt là `quanganh_bot`.

<img src="https://imgur.com/oqx7kn9.png">

- Sau khi tạo bot thành công sẽ có thông báo về một `token` cho bot. Ví dụ: `Part00000:Part1111111111111111-Part22222222222222`

### Bước 2: Thêm bot vào group

- Ta có thể thêm bot vào bất kỳ group nào ta muốn, ở đây ta sẽ thêm vào một group riêng chuyên gửi cảnh báo `SSH Report`.

- Chọn *Add member* -> @quanganh_bot.

- Khởi động bot bằng cách chat với bot trong nhóm: Ví dụ `/my_id @quanganh_bot`

### Bước 3: Lấy chat_id

- Để lấy được chat_id, ta sử dụng đường link sau:

https://api.telegram.org/bot[TOKEN]/getUpdates

- Tại ô [TOKEN] chính là Token nhận được khi tạo bot. Ví dụ là: https://api.telegram.org/botPart00000:Part1111111111111111-Part22222222222222/getUpdates

- Lúc này kết quả sẽ hiện ra, ta tìm đến đoạn `"chat":{"id":-xxxxxxxxx,"title":"SSH Report"`. Chat_id của bạn chính là chuỗi kí tự `-xxxxxxxxx`.

### Bước 4: Kiểm tra cảnh báo tới Telegram qua API

**Cách 1**: Ta sử dụng trên trình duyệt Web với method `GET` của http với cú pháp:

https://api.telegram.org/bot[TOKEN]/sendMessage?chat_id=[CHAT_ID]&text=[MY_MESSAGE_TEXT]

- [TOKEN]: Mã token ta nhận ở trên.

- [CHAT_ID]: chat_id ta vừa lấy.

- [MY_MESSAGE_TEXT]: Tin nhắn bất kỳ mà ta muốn.

Kết quả nhận được khi tin nhắn được chuyển tới group Telegram.

<img src="https://imgur.com/vRS8pKR.png">

**Cách 2**: Sử dụng lệnh curl để kiểm tra:

curl -X POST "https://api.telegram.org/bot[TOKEN]/sendMessage" -d "chat_id=[CHAT_ID]&text=[MY_MESSAGE_TEXT]"

Tương tự với [TOKEN], [CHAT_ID] và [MY_MESSAGE_TEXT]

Kết quả nhận được:

<img src="https://imgur.com/03sAUKM.png">

## 2. Gửi cảnh báo sử dụng `jq`

Với 2 cách trên, ta chỉ sử dụng để kiểm tra gửi cảnh báo về Telegram. Để thực hiện việc thông báo về đăng nhập SSH tới Telegram. Ta phải sử dụng script gửi cảnh báo để tự động hóa. Ở đây ta sẽ sử dụng ứng dụng `jq`

`jq` là một ứng dụng để  đọc thông tin file JSON trên linux. Để tìm hiểu ta có thể truy cập [tại đây](https://stedolan.github.io/jq/)

- Cài đặt jq:

```
yum install epel-release -y
yum install jq -y
```

- Tạo Script: Tạo một thư mục `/etc/profile.d/`. Để khi đăng nhập vào hệ thống thì script sẽ thực hiện ngay lập tức.

Tạo file script `ssh-telegram.sh`:

`vi /etc/profile.d/ssh-telegram.sh`

Nội dung script:

```
# ID chat Telegram
USERID="<target_user_id>"

# API Token bot
TOKEN="<bot_private_TOKEN>"

TIMEOUT="10"

# URL gửi tin nhắn của bot
URL="https://api.telegram.org/bot$TOKEN/sendMessage"

# Thời gian hệ thống
DATE_EXEC="$(date "+%d %b %Y %H:%M")"

# File temp
TMPFILE='/tmp/ipinfo.txt'

if [ -n "$SSH_CLIENT" ]; then
    IP=$(echo $SSH_CLIENT | awk '{print $1}')
    PORT=$(echo $SSH_CLIENT | awk '{print $3}')
    HOSTNAME=$(hostname -f)
    IPADDR=$(echo $SSH_CONNECTION | awk '{print $3}')

    # Lấy các thông tin từ IP người truy cập theo trang ipinfo.io
    curl http://ipinfo.io/$IP -s -o $TMPFILE
    CITY=$(cat $TMPFILE | jq '.city' | sed 's/"//g')
    REGION=$(cat $TMPFILE | jq '.region' | sed 's/"//g')
    COUNTRY=$(cat $TMPFILE | jq '.country' | sed 's/"//g')
    ORG=$(cat $TMPFILE | jq '.org' | sed 's/"//g')

    # Nội dung cảnh báo
    TEXT=$(echo -e "Thời gian: $DATE_EXEC\nUser: ${USER} logged in to $HOSTNAME($IPADDR) \nFrom $IP - $ORG - $CITY, $REGION, $COUNTRY on port $PORT")

    # Gửi cảnh báo
    curl -s -X POST --max-time $TIMEOUT $URL -d "chat_id=$USERID" -d text="$TEXT" > /dev/null

    # Xóa file temp khi script thực hiện xong
    rm $TMPFILE
fi
```

Trong đó, ta chỉ cần điền `chat_id` và `TOKEN` mà ta nhận được ở trên sau đó lưu lại là được. **Lưu ý**: tại `chat_id` phải có dấu `-` ở trước.

- Cấp quyền thực thi cho script:

`chmod +x /etc/profile.d/ssh-telegram.sh`

- Kiểm tra đăng nhập bằng SSH:

<img src="https://imgur.com/G8aD8az.png">

- Kiểm tra đăng nhập bằng SSH qua một máy khác:

<img src="https://imgur.com/sAnAGwB.png">

Cảnh báo được gửi về ngay sau khi đăng nhập SSH.

## Tài liệu tham khảo

https://dotrungquan.info/huong-dan-tao-bot-giam-sat-truy-cap-ssh-thong-qua-ung-dung-telegram/