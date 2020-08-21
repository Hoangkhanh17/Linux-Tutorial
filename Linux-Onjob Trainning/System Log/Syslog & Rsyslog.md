# Tổng quan về Syslog, Rsyslog và Log tập trung

## 1. Log là gì?

Log ghi lại liên tục các thông báo về hoạt động của cả hệ thống hoặc của các dịch vụ được triển khai trên hệ thống và file tương ứng. Log file thường là các file văn bản thông thường dưới dạng “clear text” tức là bạn có thể dễ dàng đọc được nó, vì thế có thể sử dụng các trình soạn thảo văn bản.

Các file log có thể nói cho bạn bất cứ thứ gì bạn cần biết, để giải quyết các rắc rối mà bạn gặp phải miễn là bạn biết ứng dụng nào. Mỗi ứng dụng được cài đặt trên hệ thống có cơ chế tạo log file riêng của mình để bất cứ khi nào bạn cần thông tin cụ thể thì các log file là nơi tốt nhất để tìm.

Các tập tin log được đặt trong thư mục /var/log. Bất kỳ ứng dụng khác mà sau này bạn có thể cài đặt trên hệ thống của bạn có thể sẽ tạo tập tin log của chúng tại /var/log.

## 2. Tổng quan Syslog

<img src="https://imgur.com/HGWS3w8.png">

### 2.1. Mục đích của Syslog

Syslog được sử dụng như một tiêu chuẩn, chuyển tiếp và thu thập log được sử dụng trên một phiên bản Linux. Syslog xác định mức độ nghiêm trọng (severity levels) cũng như mức độ cơ sở (facility levels) giúp người dùng hiểu rõ hơn về nhật ký được sinh ra trên máy tính của họ. 

Syslog có 4 khái niệm cơ bản:

- **Facility:** Giúp kiểm soát log đến dựa vào nguồn gốc được quy định như từ ứng dụng hay tiến trình nào. Syslog sử dụng facility để quy hoạch lại log như vậy có thể coi faicility là đại diện cho đối tượng tạo ra thông báo (kernel, process, apps,..). **Facility** sẽ "dán nhãn" các log ứng với ứng dụng hay tiến trình ghi lại log đó và ta chỉ việc đưa các `logs` vào các thư mục tương ứng để tiện việc sử dụng.

<img src="https://imgur.com/hVJOQ7Z.png">

- **Priority (level):** mức độ quan trọng của log message được chỉ định. Mức độ bao gồm từ gỡ lỗi (debug), thông báo thông tin (informational messages) đến mức khẩn cấp (emergency levels).

<img src="https://imgur.com/avxB3Xe.png">

- **Selector:** sự kết hợp giữa facilities và level (facility.level), tức khi một event xảy ra nó sẽ xem có match bất kì selector nào hay không, nếu có thì hành động (action) tương ứng khi cấu hình sẽ được thực thi.

- **Action:** đại diện cho địa chỉ của messages tương ứng với facility.level. action có thể là một tên file, một host name đứng trước ký tự @, hoặc một danh sách người dùng ngăn cách bằng dấu phảy, hoặc một dấu *.

### 2.2. Định dạng của Syslog

<img src="https://imgur.com/Evyp4ji.png">

Định dạng của 1 Syslog được chia thành ba phần, độ dài của một log không được vượt quá 1024 bytes:

- PRI : chi tiết các mức độ ưu tiên của tin nhắn (từ tin nhắn gỡ lỗi (debug) đến trường hợp khẩn cấp) cũng như các mức độ cơ sở (mail, auth, kernel).

- Header: bao gồm hai trường là TIMESTAMP và HOSTNAME, tên máy chủ là tên máy gửi nhật ký.

- MSG: phần này chứa thông tin thực tế về sự kiện đã xảy ra. Nó cũng được chia thành trường TAG và trường CONTENT.

#### a) PRI

Phần PRI hay Priority là một số được đặt trong ngoặc nhọn, thể hiện cơ sở sinh ra log hoặc mức độ nghiêm trọng, là một số gồm 8 bit:

- 3 bit đầu tiên thể hiện cho tính nghiêm trọng của thông báo.

- 5 bit còn lại đại diện cho sơ sở sinh ra thông báo.

<img src="https://imgur.com/hvfzAgS.png">

#### b) Hearder

Header bao gồm:

- **TIMESTAMP:** được định dạng trên định dạng của Mmm dd hh:mm:ss – Mmm, là ba chữ cái đầu tiên của tháng. Sau đó là thời gian mà thông báo được tạo ra giờ:phút:giây. Thời gian này được lấy từ thời gian hệ thống. **Chú ý:** nếu như thời gian của server và thời gian của client khác nhau thì thông báo ghi trên log được gửi lên server là thời gian của máy client.

- **HOSTNAME** (đôi khi có thể được phân giải thành địa chỉ IP). Nó thường được đưa ra khi bạn nhập lệnh tên máy chủ. Nếu không tìm thấy, nó sẽ được gán cả IPv4 hoặc IPv6 của máy chủ.

<img src="https://imgur.com/GP4FdQ9.png">

## 3. Tổng quan Rsyslog

Rsyslog là một phần mềm mã nguồn mở sử dụng trên Linux dùng để chuyển tiếp các log message đến một địa chỉ trên mạng (log receiver, log server) Nó thực hiện giao thức syslog cơ bản, đặc biệt là sử dụng TCP cho việc truyền tải log từ client tới server. Hiện nay rsyslog là phần mềm được cài đặt sẵn trên hầu hết hệ thống Unix và các bản phân phối của Linux như : Fedora, openSUSE, Debian, Ubuntu, Red Hat Enterprise Linux, FreeBSD…

- Nếu như sử dụng một bản phân phối Linux hiện đại (như máy Ubuntu, CentOS hoặc RHEL), máy chủ `syslog` mặc định được sử dụng là `rsyslog`.

- `Rsyslog` là một sự phát triển của syslog, cung cấp các khả năng như các mô đun có thể cấu hình, được liên kết với nhiều mục tiêu khác nhau (ví dụ chuyển tiếp nhật ký Apache đến một máy chủ từ xa).

- `Rsyslog` cũng cung cấp tính năng lọc riêng cũng như tạo khuôn mẫu để định dạng dữ liệu sang định dạng tùy chỉnh.

**Modules Rsyslog:** Rsyslog có thiết kế kiểu mô-đun. Điều này cho phép chức năng được tải động từ các mô-đun, cũng có thể được viết bởi bất kỳ bên thứ ba nào. Bản thân Rsyslog cung cấp tất cả các chức năng không cốt lõi như các mô-đun. Do đó, ngày càng có nhiều mô-đun. Có 6 modules cơ bản.

- [Output Modules](https://www.rsyslog.com/doc/v8-stable/configuration/modules/idx_output.html)

- [Input Modules](https://www.rsyslog.com/doc/v8-stable/configuration/modules/idx_input.html)

- [Parser Modules](https://www.rsyslog.com/doc/v8-stable/configuration/modules/idx_parser.html)

- [Message Modification Modules](https://www.rsyslog.com/doc/v8-stable/configuration/modules/idx_messagemod.html)

- [String Generator Modules](https://www.rsyslog.com/doc/v8-stable/configuration/modules/idx_stringgen.html)

- [Library Modules](https://www.rsyslog.com/doc/v8-stable/configuration/modules/idx_library.html)

### File cấu hình của rsyslog.conf

Ta có thể tìm file cấu hình rsyslog tại `/etc/rsyslog.conf`

```
authpriv.*                                              /var/log/secure
mail.*                                                  -/var/log/maillog
cron.*                                                  /var/log/cron
*.emerg                                                 :omusrmsg:*
uucp,news.crit                                          /var/log/spooler
local7.*                                                /var/log/boot.log
```

Cơ bản trên file rsyslog.conf mặc định cho chúng ta thấy nơi lưu trữ các log tiến trình của hệ thống.

Cấu hình trên được chia làm 2 trường:

- Trường 1: Seletor

    - Trường Seletor : Chỉ ra nguồn tạo ra log và mức cảnh bảo của log đó.

    - Trong trường seletor có 2 thành phần và được tách nhau bằng dấu “.“

- Trường 2: Action là trường để chỉ ra nơi lưu log của tiến trình đó. Có 2 loại là lưu tại file trong localhost hoặc gửi đến IP của máy chủ Log.


## Tài liệu tham khảo

https://news.cloud365.vn/log-ly-thuyet-tong-quan-ve-log-syslog-rsyslog/

https://cuongquach.com/syslog-phan-1-cac-khai-niem-ve-log-trong-linux.html