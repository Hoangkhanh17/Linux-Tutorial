# Cách thức Mail Server hoạt động

Dịch vụ email là một dịch vụ được xử dụng thường xuyên nhất trên toàn cầu . Ngày nay hầu như hầu hết mọi người đều có ít nhất một tài khoản email . Mặc dù việc gởi nhận mail rất đơn giản nhưng đằng sau đó rất nhiều sự kiện diễn ra sau hậu trường để đảm bảo rằng email đến đích cuối cùng của nó

Chức năng của mail server có thể được chia thành hai quy trình : gởi và nhận email . Hai giao thức sau đây giám sát thực hiện hai quá trình gởi nhận này:

- **Gởi Email** : Simple Mail Transfer Protocol (SMTP).

- **Nhận Email** : Post Office Protocol (POP) / Internet Message Access Protocol (IMAP).

## 1. Các thuật ngữ

- **Mail User Agent (MUA)**: MUA là một thành phần tương tác trực tiếp với người dùng cuối . Ví dụ về MUA là phần mềm Thunderbird, MS Outlook, Zimbra Desktop. Các giao diện web mail như Gmail và Yahoo! cũng là MUA.

- **Mail Transfer Agent (MTA)**: MTA chịu trách nhiệm chuyển email từ mail server của người gởi thư đến mail server của người nhận thư. Ví dụ về MTA là sendmail và postfix.

- **Mail Delivery Agent (MDA)**: Trong một mail server nhận thư , local MTA chấp nhận một email đến từ MTA của người gởi . Email sau đó được gửi đến hộp thư của người dùng bởi MDA.

- **POP / IMAP**: Giao thức POP và IMAP được sử dụng để tìm nạp email từ hộp thư của máy chủ người nhận tới MUA người nhận.

- **Mail Exchanger Record (MX)**: MX record có nhiệm vụ là chỉ đường cho email đi đến mail server của bạn (MX record thường đi kèm theo một A record sẽ trỏ về địa chỉ IP của mail server , và một thông số pref có giá trị số để chỉ ra mức độ ưu tiên của các mail server , giá trị pref càng nhỏ thì mức độ ưu tiên càng cao).

<img src="https://imgur.com/NYlxlWM.png">

Khi người gởi nhấp vào nút gởi , SMTP giúp đảm bảo chắc chắn là email được gởi từ server người gởi tới server người nhận . Khi email đến server người nhận MTA của server nhận sẽ tiếp nhận email của người gởi và chuyển nó cho MDA địa phương . MDA sau đó writes email vào mailbox của người nhận . Khi người nhận dùng MUA để kiểm tra email , MUA sẽ dùng giao thức POP hoặc IMAP để lấy mail về.

## 2. Cách thức hoạt động của Mail Server

Giả sử rằng userA@exampleA.tst đang cố gắng gửi một email tới userB@exampleB.tst

Các sự kiện sau đây sẽ xảy ra tuần tự khi người dùng gởi mail:

<img src="https://imgur.com/MkQgkL3.png">

1. MUA của người gửi khởi tạo kết nối tới mail server mail.exampleA.tst bằng giao thức SMTP (thường là TCP Port 25).

2. Mail server mail.exampleA.tst nhận email và biết rằng domain đích cần gởi email tới là exampleB.tst . Server mail.exampleA.tst tạo ra một truy vấn đến máy chủ DNS để hỏi về thông tin record MX của domain exampleB.tst. Giả sử rằng không có thông tin về domain exampleB.tst trong bộ nhớ cache của máy chủ DNS .

3. Máy chủ DNS lần lượt tạo ra một truy vấn đệ quy đối với máy chủ DNS có thẩm quyền và tìm hiểu về chi tiết các record MX của domain exampleB.tst. Thông tin này sẽ được trả về cho server mail.exampleA.tst .

4. Bây giờ server mail.exampleA.tst đã có địa chỉ IP của mail server đích , nó sẽ gửi email trực tiếp tới mail server mail.exampleB.tst thông qua Internet. SMTP được sử dụng để liên lạc giữa các mail server nguồn và đích.
5. Email đến được nhận bởi SMTP (MTA) cục bộ trên server mail.exampleB.tst. Sau khi nhận được email, nó được chuyển cho MDA, sau đó gửi thư đến hộp thư của người nhận được lưu trữ trong máy chủ. Máy chủ có các hộp thư riêng biệt cho mỗi người dùng.

6. Khi người nhận kiểm tra email qua giao thức POP hoặc IMAP, email được MUA lấy từ máy chủ về máy tính của user . Tùy thuộc vào cấu hình MUA, email có thể được tải xuống trong máy trạm, bản sao có thể được lưu giữ trong cả máy chủ và máy trạm, hoặc email giữa máy chủ và MUA được đồng bộ hóa , tuỳ thuộc vào bạn chọn giao thức POP hay IMAP.

## 3. Các giao thức Mail Server

Hiện nay có 3 giao thức cơ bản được ứng dụng trong mail server giúp hệ thống có thể hoạt động trơn tru và đảm bảo khả năng bảo mật cao nhất, bao gồm:

- **SMTP – Simple Mail Transfer Porotocol**: SMTP được sử dụng khi gửi từ một ứng dụng email như Postfix với một máy chủ email hoặc khi email được gửi từ một máy chủ email khác. Giao thức này sử dụng cổng TCP 25 với không bảo mật SSL và cổng 465/587 với bảo mật SSL/TLS.

- **POP3 – Post Office Porotocol version 3**: giao thức này được dùng để tải một email từ một máy chủ email. POP3 sử dụng cổng TCP 110 khi không có SSL, cổng 995 khi có SSL/TLS.

- **IMAP – Internet Message Access Protocol**: Đây là giao thức thế hệ mới của POP. IMAP sử dụng cổng TCP 143 (không có SSL) và 993 (Có SSL). Nó đặt sự kiểm soát email trên mail server. IMAP có thể hoạt động ở 3 chế độ: trực tuyến, ngoại tuyến và ngắt kết nối. Chế độ ngoại tuyến được thực hiện như sau: khi các email đã được chuyển tới máy client, nó sẽ bị xóa khỏa mail server và sau đó hệ thống bị ngắt. Lúc này người dùng có thể đọc, trả lời và làm một số việc khác ở chế độ ngoại tuyến. Tuy nhiên nếu họ muốn gửi thư, họ phải kết nối lại. Như vậy ở trong chế độ này, thông điệp được lưu tạm ở client server giúp người dùng có thể sử dụng bình thường và ở lần kết nối kế tiếp nó sẽ được cập nhập trở lại vào mail server.