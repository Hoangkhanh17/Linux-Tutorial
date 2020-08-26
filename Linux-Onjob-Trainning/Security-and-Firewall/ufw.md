# Tìm hiểu về UFW (Uncomplicated Firewall)

## Mục lục

## 1. Giới thiếu về UFW

Firewall của máy lá tính có thể là phần cứng hay phần mềm quản lý inbound và outbound traffic (dữ liệu truyền đưa). Nó là một yếu tố bảo mật máy tính.

Kể cả nếu Linux được cho là có nhiều tính năng bảo mật sẵn rồi, sẽ không bao giờ là thừa khi thiết lập bảo mật hơn.

Trong trường hợp này, Ubuntu Firewall là ứng dụng tường lửa trong hệ điều hành Ubuntu. Nó là UFW (Uncomplicated Firewall – tường lửa không phức tạp), với Iptables làm làm front-end.

## 2. Cấu hình Ubuntu Firewall với UFW

Trên Ubuntu 18.04 đã được cài sẵn UFW nhưng chưa kích hoạt nên điều đầu tiên cần làm là kích hoạt UFW.

`sudo ufw enable`

- Nếu xuất hiện lỗi, ta có thể cài đặt lại UFW:

`sudo apt-get install ufw`

- Kiểm tra lại hoạt động của UFW:

`sudo ufw status`

Mặc định, UFW chặn tất cả kết nối incoming và cho phép mọi kết nối outgoing. Nhiều người chỉ cần vậy là được, nhưng nếu bạn có dịch vụ mạng hay ứng dụng đặc biệt, bạn cần thiết lập một vài rules (quy tắc).

## 3. Thiết lập UFW 

### 3.1. Mở và đóng port bằng UFW

#### Mở hoặc đóng một port

`sudo ufw allow/deny [port/protocol]`

Trong đó:

- allow là cho phép mở port, deny là đóng port.

- port: port ta muốn mở.

- protocol: giao thức tcp hoặc udp.

Ví dụ: Mở hoặcđóng port 56 với giao thức tcp

`sudo ufw allow/deny 56/tcp`

#### Mở hoặc đóng một loạt port cùng lúc

Trong cùng một dòng lệnh, mở và chặn một loạt port. Rất tiết kiệm thời gian và cấu trúc đơn giản như sau:

`sudo ufw allow/deny [Starting_port:Ending_port]/protocol`

Ví dụ:

`sudo ufw allow/deny 300:310/tcp`

### 3.2. Làm việc với dịch vụ trên UFW

Có nhiều dịch vụ mạng mà UFW có thể thiết lập. Cách quản lý chúng là biết port nào chúng đang dùng để kết nối tới server.

Ví dụ, HTTP đòi hỏi port 80 có sẵn và HTTPS port 443 .

Vậy, chúng ta cần chạy lệnh sau cho HTTP:

`sudo ufw allow http`

Lệnh này đồng nghĩa với việc mở port 80.

Nên bạn chỉ cần biết dịch vụ nào chạy port nào là có thể quản lý được.

### 3.3. Chặn hoặc cho phép địa chỉ IP

- Ta cũng có thể chặn một địa chỉ IP nhất định với lệnh sau:

`sudo ufw deny from IPADRESS`

Ví dụ: Chặn địa chỉ IP `192.168.10.100`

`sudo ufw deny from 192.168.10.100`

- Cho phép một địa chỉ IP kết nối:

`sudo ufw allow from 192.168.1.3`

- Cho phép một địa IP kết nối với một port nhất định:

`sudo ufw allow from [IP_ADDRES] to any port [PORT]`

### 3.4. Xóa một rule nhất định trên UFW

Trước khi xóa một rule nào đó, ta cần liệt kê danh sách rules đã thiết lập. Mỗi một dòng lệnh để thiết lập rule là một dòng trong danh sách:

`sudo ufw status numbered`

Xóa một rule bất kỳ, ta cần xem rule đó ở dòng lệnh thứ mấy. Ví dụ: Xóa rule số 4

`sudo ufw delete 4`

## Tài liệu tham khảo

https://www.hostinger.vn/huong-dan/cau-hinh-ubuntu-firewall/