# SMTP protocol - Giao thức SMTP - Cách hoạt động của Email

## SMTP là gì

SMTP là một tiêu chuẩn Internet để gửi email trên internet.

Các thông số Kỹ thuật ban đầu của SMTP được xuất bản theo RFC 821 vào năm 1982. Sau đó, nhiều thông số kỹ thuật hơn được giới thiếu theo RFC 5321.

Tham khảo:

- RFC 821: https://tools.ietf.org/html/rfc821

- RFC 5321: https://tools.ietf.org/html/rfc5321

Thực sự để đọc một Document hàng trăm trang sẽ gây khó khăn và nhàm chán. Chúng ta sẽ giải thích cách hoạt động của Email và SMTP qua những ví dụ thực tế.

## Ví dụ thực tế

Lấy ví dụ: Mail được gửi đi từ Bob(Bob@gmail.com) đến người nhận là Alice (Alice@yahoo.com).

Bob muốn gửi email cho Alice. Anh ấy soạn tin nhắn của mình trên một ứng dụng để gửi/nhận mail (Outlook, Apple's Mail,...) và đã nhấn "Gửi".

Chúng ta sẽ tiến hành theo dõi hành trình của mail này từ Máy tính cá nhân của Bob gửi đi đến khi nó đến được Máy tính của Alice.

### 1. Bob's User Agent

Đây là một Ứng dụng được Bob sử dụng trên Máy tính cá nhân để Viết thư, trả lời thư và đọc các thư.

Bob sử dụng ứng dụng Apple's Mail làm công cụ gọi là **User Agent**

Nếu Bob muốn đọc Mail của mình, **User Agent** sẽ lấy lấy Mail từ Mail Server của anh ấy. Nếu Bob muốn gửi Mail đi, anh ấy sẽ viết mail trên **User Agent**, sau đó đẩy Mail đó đến Mail Server của anh ấy để có thể gửi được Mail đến đúng người nhận.

### 2. Bob's Mail Server

Bob có một tài khoản Gmail (Bob@gmail.com).

Điều đó có nghĩa là có một Máy chủ từ xa (remote machine) thuộc Miền gmail.com quản lý tất cả các Mails được gửi tới Bob. Máy chủ này phụ trách việc gửi Mails từ Bob tới những người dùng khác trên các Mail Server khác.

Máy chủ từ xa này (chính xác hơn, ứng dụng chạy trên máy chủ từ xa này) được gọi là Bob's Mail Server.

### 3. Alice's Mail Server

Cũng tương tự như Bob's Mail Server, nhưng Máy chủ này là Yahoo thay vì là Gmail của Bob (Vì Alice có tài khoản mail Alice@yahoo.com của Yahoo).

### 4. Alice's User Agent

Cũng tương tự như **User Agent** của Bob, đây là ứng dụng trên Máy tính cá nhân của Alice, giúp cô ấy tìm và lấy Mails từ Mail Server của cố ấy để đọc. Nó cũng cho phép Alice soạn Mail trên Máy tính cá nhân và gửi Mail đến Mail Server của cô ấy sau đó gửi đến đúng người nhận. Ứng dụng cô ấy đang sử dụng là Microsoft Outlook.

<img src="https://imgur.com/g3RqJgG.png">

## The email journey - Hành trình của một Email

Cùng tìm hiểu một Email sẽ chạy như thế nào từ Bob tới Alice.

1. Bob mở Ứng dụng Mail trên máy tính của anh ấy, nhập địa chỉ mail người nhận là Alice (Alice@yahoo.com), viết nội dung thư, Click chuột vào "Send" để gửi Mail đi.

2. Ứng dụng mail bắt đầu giao tiếp với Mail Server của Bob và bắt đầu đẩy mail đi đến nơi mà được lưu trữ rồi sau đó gửi đến mail của Alice (Alice@yahoo.com).

3. Mail Server của Bob thấy có một Mail đang chờ được gửi đến Alice@yahoo.com. Nó bắt đầu giao tiếp với Mail Server yahoo.com để cho phép Mail được gửi đi. Tại đây Giao thức SMTP bắt đầu vào việc. SMTP là giao thức quản lý việc giao tiếp giữa hai máy chủ Mail Server. Trong trường hợp cụ thể, Mail Server của Bob sẽ đóng vai trò là một SMTP Client trong khi Mail Server của Alice sẽ đóng vai trò là một SMTP Server.

4. Sau vài lần "bắt tay" giữa Gmail Mail Server và Yahoo Mail Server, SMTP Client gửi Mail của Bob tới Mail Server của Alice.

5. Mail Server của Alice nhận được Mail và đưa chúng vào trong Mailbox của cô ấy để cô ấy có thể đọc Mail.

6. Alice sử dụng Microsoft Outlook để lấy Mail từ Mailbox của cô ấy và bắt đầu đọc Mail của Bob.

## The SMTP Protocol - Giao thức SMTP

Hiện tại chúng ta sẽ tập trung vào việc giao tiếp giữa Gmail Mail Server (Bob) và Yahoo Mail Server (Alice).

Gmail Mail Server bắt đầu giao tiếp với Yahoo Mail Server để gửi Mail của Bob tới Alice.

SMTP chính là giao thức chi phối việc giao tiếp này.

Đây là sơ đồ tuần tự của tất cả các sự kiện xảy ra khi mọi thứ hoạt động chính xác:

<img src="https://imgur.com/OyuHbIK.png">

Giao thức SMTP là một giao thức văn bản bao gồm các lệnh và câu trả lời.

Trong ví dụ: Gmail Mail Server đóng vai trò là SMTP Client gửi các lệnh SMTP trong khi SMTP Server (Yahoo Mail Server) trả lời những lệnh này bằng các mã số.

Một vài ví dụ về các lệnh được sử dụng trong giao thức SMTP: **EHLO, MAIL FROM, RCPT TO, DATA, QUIT**.

## Three phases in SMTP Protocol

Có 3 phần trong giao thức SMTP:

### First. The SMTP handshake

Ban đầu, Gmail Mail Server (SMTP Client) thiết lập kết nối TCP tới Yahoo Mail Server (SMTP Server) nơi mà SMTP Server trả lời bằng **Mã Code 220**. (Bước này không xuất hiện trong sơ đồ tuần tự bên trên).

Sau khi SMTP Client nhận được **Mã Code 220**, việc "bắt tay" bắt đầu.

Mục đích chung của việc "bắt tay" là để Client và Server xác định bản thân, các dịch vụ được cung cấp, xác định danh tính của người gửi và người nhận.

Nó được bắt đầu bởi Gmail Mail Server gửi lệnh **EHLO** tới Yahoo Mail Server và xác định tên miền của nó. Ví dụ, Gmail Server sẽ gửi **"EHLO<gmail.com>"**.

Tưởng tượng lệnh **EHLO** như là một lời chào "hello" từ SMTP Client tới SMTP Server. Trên thực tế, nó được gọi là lệnh **HELO** trong RFC cũ, nhưng đã được sửa đổi sau đó trong RFC mới hơn để cho phép những tính năng vượt trội hơn. Xem thêm [tại đây](http://en.redinskala.com/what-is-the-difference-between-the-heloehlo-commands/)

SMTP Server xác nhận thông báo **EHLO** bằng cách trả lời bằng **Mã code 250** cùng với các dịch vụ mà SMTP có thể hỗ trợ. Điều quan trọng là SMTP Client và SMTP Server đều phải đồng ý về các dịch vụ và tính năng mà chúng có thể hỗ trợ khi quá trình chuyển thư bắt đầu.

Vậy là lời chào đã hoàn thành, bây giờ Client sẽ gửi thông tin của người gửi và người nhận.

SMTP Client tiếp tục bằng cách gửi lệnh **MAIL FROM** cùng với thông tin người gửi. Trong ví dụ là **"MAIL FROM:<Bob@gmail.com>"**

Khi SMTP Server nhận được lệnh, nó sẽ phản hồi lại cùng với **Mã code 250** để cho biết rằng không vấn đề gì khi nhận Mail từ User này.

Sau đó, SMTP Client gửi lệnh **RCPT TO** cùng với địa chỉ email của người nhận **RCPT TO:<Alice@yahoo.com>**.

Trong số đó, SMTP Server kiểm tra nếu có User "Alice" tồn tại thì sẽ gửi lại **Mã code 250** để xác nhận có thể chấp nhận Mail từ Bob tới Alice.

Vậy là kết thúc giai đoạn "bắt tay". Vậy hãy chuyển sang các chi tiết nhỏ hơn. Làm thế nào để Mail được chuyển từ SMTP Client sang SMTP Server ?

### Second. The Message transfer

Trước khi Mail thực sự được vận chuyển, SMTP Client gửi một câu lệnh nữa là **DATA** tới Server để chắc chắn rằng phía Server đã sẵn sàng.

SMTP Server gửi lại **Mã code 354** để thông báo rằng đã sẵn sàng nhận Mail.

Sau khi nhận được Mã code từ Server, Client đã sẵn sàng để gửi Mail.

Thực chất Mail đã được gửi đi từng dòng một. Server không thừa nhận từng dòng riêng lẻ, nó sẽ đợi tới khi dòng đặc biệt "End of mail" là dòng chỉ có "." (khoảng thời gian hoặc toàn bộ điểm dừng) bởi chính nó.

Khi Client gửi "." tới Server, điều này cho biết rằng Client đã hoàn tất việc gửi Mail. Nó cũng cho biết rằng Server có thể bắt đầu tiến trình xử lý Mail ngay bây giờ.

Sau khi SMTP Server nhận được ".", nó xác nhận đã nhận toàn bộ Mail bằng cách gửi **Mã code 250** lại cho Client.

Và thế là xong, đây là cách mà Mail được Bob soạn trên Máy tính của anh ấy kết thúc trên Yahoo Mail Server chờ đợi Alice tìm và đọc. Nhưng vẫn còn thiếu một thứ, đóng kết nối giữa SMTP Client và SMTP Server.

### Third. Closing the connection

Điều nãy rất đơn giản và dễ hiểu.

Gmail Mail Server gửi lệnh **QUIT** tới Yahoo Mail Server để chỉ ra ý định đóng kết nối và Yahoo Mail Server trả lời bằng **Mã Code 221**.

## User Agents

Trong ví dụ, Bob sử dụng User Agent để gửi Mail của anh ấy tới Mail Server. Alice sử dụng User Agent để lấy và đọc Mail của Bob. Trước tiên hãy nói về phía của Bob, User Agent gửi mail tới Mail Server là Gmail.

Hóa ra, User Agent của Bob cũng có thể dùng Giao thức SMTP để gửi Mail của Bob tới Gmail Mail Server.

Nó thực sự là một quá trình giống nhau, nhưng User Agent của Bob trở thành SMTP Client và Gmail Mail Server trở thành SMTP Server.

Với Alice thì khác. Alice không muốn đẩy mail lên Mail Server của cố ấy. Alice muốn lấy và đọc Mail đã có sẵn trên Yahoo Mailbox. Vì vậy có 2 giao thức phổ biến mà User Agent của Alice có thể sử dụng: Đó là POP và IMAP. Thành thực mà nói, đây không phải là 2 giao thức duy nhất để tương tác với Mail Server từ User Agent.

Thực tế hiện nay, chúng ta có thể đăng nhập trên các trình duyệt để làm User Agent. Các trình duyệt gửi/nhận Mail bằng HTTP nên sẽ không liên quan tới SMTP hoặc POP/IMAP. Tuy nhiên, việc giao tiếp giữa Gmail Mail Server và Yahoo Mail Server vẫn được điều chỉnh bới giao thức SMTP.

## Question

Như đã đề cập ở trên, SMTP Client sẽ gửi "." tại dòng cuối của Mail để thông báo rằng đã chuyển toàn bộ nội dung Mail.

Vậy điều gì sẽ xảy ra khi Bob tự viết "." tại dòng cuối ? 

Một số điều khoản về tính minh bạch của dữ liệu, "." để kết thúc Mail sẽ không được gửi bởi người dùng. Nhìn chung, người dùng không nhận thức được các chuỗi "Bị cấm". Để cho phép truyển tài Mail do người dùng soạn một cách minh bạch, có một số những thủ tục được sử dụng như sau:

- Trước khi gửi một dòng văn bản Mail, SMTP Client kiểm tra ký tự đầu tiên của dòng. Nếu đó là một khoảng trống, một bổ sung "." sẽ được thêm vào đầu dòng.

- Khi dòng văn bản Mail được SMTP Server nhận, nó sẽ kiểm tra dòng. Nếu dòng bao gồm "." duy nhất, nó sẽ được coi là chỉ báo cuối thư. Nếu kí tự đầu tiên là dấu chấm và có các ký tự khác trên dòng, ký tự đầu tiên sẽ bị xóa.

Tìm hiểu thêm [tại đây]: https://tools.ietf.org/html/rfc5321#section-4.5.2

## Tài liệu tham khảo

https://www.afternerd.com/blog/smtp/