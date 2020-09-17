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

