# Tìm hiểu về SSL

## Mục lục

## 1. SSL là gì?

SSL là chữ viết tắt của Secure Sockets Layer (Lớp socket bảo mật). Một loại bảo mật giúp mã hóa liên lạc giữa website và trình duyệt. Công nghệ này đang lỗi thời và được thay thế hoàn toàn bởi TLS.

TLS là chữ viết tắt của Transport Layer Security, nó cũng giúp bảo mật thông tin truyền giống như SSL. Nhưng vì SSL không còn được phát triển nữa, nên TLS mới là thuật ngữ đúng nên dùng.

HTTPS là phần mở rộng bảo mật của HTTP. Website được cài đặt chứng chỉ SSL/TLS có thể dùng giao thức HTTPS để thiết lập kênh kết nối an toàn tới server.

- Mục tiêu của SSL/TLS là bảo mật các thông tin nhạy cảm trong quá trình truyền trên internet như, thông tin cá nhân, thông tin thanh toán, thông tin đăng nhập.

- Nó là giải pháp thay thế cho phướng pháp truyền thông tin văn bản dạng plain text, văn bản loại này khi truyền trên internet sẽ không được mã hóa, nên việc áp dụng mã hóa vào sẽ khiến cho các bên thứ 3 không xâm nhập được bào thông tin của bạn, không đánh cắp hay chỉnh sửa được các thông tin đó.

- Ta có thể biết được website có đang dùng chứng chỉ bảo mật SSL/TLS hay không đơn giản bằng cách nhìn vào icon trong URL ngay trong thanh địa chỉ.

## 2. Cách thức hoạt động

Chứng chỉ SSL/TLS hoạt động bằng cách tích hợp key mã hóa vào thông tin định danh công ty. Nó sẽ giúp công ty mã hóa mọi thông tin được truyền mà không bị ảnh hưởng hoặc chỉnh sửa bởi các bên thứ 3.

<img src="https://imgur.com/HwLD0ZL.png">

SSL/TLS hoạt động bằng cách sử dụng public và private key, đồng thời các khóa duy nhất của mỗi phiên giao dịch. Mỗi khi khách truy cập điền vào thanh địa chỉ SSL thông tin web browser hoặc chuyển hướng tới trang web được bảo mật, trình duyệt và web server đã thiết lập kết nối.

Trong phiên kết nối ban đầu, public và private key được dùng để tạo session key, vốn được dùng để mã hóa và giải mã dữ liệu được truyền đưa. Session key sẽ được sử dụng trong một khoảng thời gian nhất định và chỉ có thể dùng cho phiên giao dịch này.

Nếu có khóa màu xanh ngay đầu địa chỉ web thì tức là website đã thiết lập đúng SSL/TLS. Bạn có thể nhấn vào nút màu xanh đó để xem ai là người giữ chứng chỉ này.

<img src="https://imgur.com/ps2Ln99.png">

### SSL/TLS và HTTPS

Khi bạn thiết lập chứng chỉ SSL, bạn sẽ cần cấu hình nó truyền dữ liệu qua HTTPS. 2 công nghệ này đi đôi với nhau mà bạn không thể chỉ dùng 1 trong 2.

URLs sử dụng 1 trong 2 giao thức là HTTP (Hypertext Transfer Protocol) hay HTTPS (Hypertext Transfer Protocol Secure). Giao thức này sẽ quyết định lượng dữ liệu được truyền đi và ghi nhận như thế nào.

<img src="https://imgur.com/KAjOJ5U.png">

## 3. Các loại chứng chỉ SSL

### 3.1. Các loại chứng chỉ SSL dựa trên mức độ xác thực

#### 3.1.1. Domain Validation

Đối với loại chứng chỉ SSL này bạn cần phải xác nhận quyền sở hữu tên miền đó là của mình, cách xác nhận được thực hiện bằng email hoặc qua hồ sơ DNS.

Loại chứng nhận này được cấp khá nhanh chỉ trong vài phút hoặc 1 vài giờ. Và nó thích hợp với các cá nhân không thuộc tổ chức và không quan tâm mấy đến vấn đề bảo mật.
Đây là loại chứng chỉ SSL rẻ nhất và thích hợp với các trang blog cá nhân.

<img src="https://imgur.com/DCE6xxj.png">

#### 3.1.2 Organization Validation

Loại chứng chỉ này sẽ được cấp trong vòng 2 đến 3 ngày làm việc.

Nó thích hợp cho các cổng thông tin thương mại điện tử.

Sự khác biệt lớn nhất giữa DV và OV là việc xác thực công ty được thực hiện bởi các nhà cung cấp chứng chỉ. Nó không lớn như EV nhưng có khả năng tốt hơn DV.

#### 3.1.3 Extended Validation

Đây là loại chứng chỉ được đánh giá cao cho các trang web với hoạt động giao dịch trực tuyến.

Khác với 2 chứng chỉ trên, thì chứng chỉ EV đòi hỏi một quy trình xác thực nghiêm ngặt. Và mất khoảng 7-10 ngày để kích hoạt.

Đây là loại chứng chỉ hiển thị các tổ chức mà chứng chỉ được cấp cho trong trình duyệt. Thích hợp với các trang ngân hàng, tài chính và thương mại điện tử

Chứng chỉ EV sẽ cung cấp trên thanh địa chỉ HTTPS màu xanh lá cây khá uy tín.

### 3.2. Các chứng chỉ SSL dựa trên số lượng tên miền

#### 3.2.1. Single Name SSL Certificate

Đối với SSL này sẽ chỉ có 1 tên miền được bảo đảm.

#### 3.2.2. Chứng chỉ Wildcard SSL

Đối với chứng chỉ này sẽ đảm bảo sự không giới hạn các sub-domain( các tên miền phụ) và một tên miền duy nhất.

#### 3.2.3. Chứng Chỉ SSL Multi-domain

Một chứng Chỉ SSL Multi-domain hỗ trợ tất cả các loại tên miền và subdomain khác nhau.

SSL Multi domain được đề xuất cho những người có nhiều tên miền và subdomain.

#### 3.2.4. Chứng Chỉ Unified Communications (UCC)

UCCs cho phép khách hàng bảo vệ lên đến 100 tên miền bằng cách sử dụng cùng một chứng chỉ.

Chứng Chỉ UCC được thiết kế đặc biệt để bảo đảm Microsoft Exchange và các Office Communication Server.

## 4. Một số loại chứng chỉ SSL Miễn phí

### 4.1. Let's Encrypt

#### 4.1.1. Khái niệm

Let’s Encrypt là một nhà cung cấp chứng chỉ số SSL (Certificate Authority) hoàn toàn miễn phí nhằm cung cấp chứng nhận SSL cho cộng đồng nói chung. Đây là một dự án của Internet Research Group, một hiệp hội về dịch vụ cộng đồng.

Let’s Encrypt được tài trợ bởi nhiều công ty bao gồm Google, Facebook, Sucuri, Mozilla, Cisco, v.v…

Lợi ích của Let's Encrypt

- **Hoàn toàn miễn phí**: Chỉ cần sở hữu một tên miền, bạn có thể sử dụng Let’s Encrypt để có được chứng chỉ tin cậy mà không phải bỏ ra bất kì chi phí nào.

- **Dễ dàng cài đặt** thông qua các control panel phổ biến như cPanel, DirectAdmin và Plesk.

- **Không yêu cầu** chứng thực qua email..

- Không yêu cầu sử dụng trên **địa chỉ IP riêng** (có phát sinh phí khi đăng ký thêm IP).

- **Mở rộng hợp tác không hạn chế**: Với tính chất mở, giao thức phát hành và gia hạn tự động sẽ được công bố như một tiêu chuẩn công khai và người khác có thể áp dụng. Giống như những giao thức Internet cơ bản khác, Let’s Encrypt nỗ lực để mang lại lợi ích cho cộng đồng và không nằm dưới sự kiểm soát của bất kỳ một tổ chức nào.

- **Được tin cậy bởi các trình duyệt**: Hoạt động như một nền tảng thúc đẩy những TLS tốt nhất, cả về phía CA (Certificate Authority), Let’s Encrypt giúp các nhà khai thác trang web đảm bảo an toàn cho máy chủ một cách đúng đắn.

- **Độ rõ ràng, minh bạch**: Tất các các chứng chỉ được ban hành hoặc thu hồi sẽ được ghi công khai và bất cứ ai cũng có thể kiểm tra.

- **Tính tự động cao**: Phần mềm chạy trên máy chủ web có thể tương tác với Let’s Encrypt để có được chứng chỉ một cách nhanh chóng, cấu hình an toàn để sẵn sàng sử dụng và tự động thay mới khi cần. Song song đó, chứng chỉ SSL của Let’s Encrypt theo chuẩn Domain Validation, do đó bạn chỉ cần tên miền và có thể sử dụng chúng cho bất kỳ máy chủ nào.

#### 4.1.2. Cách thức xác nhận tên miền của Let's Encrypt

Let’s Encrypt xác định quyền quản trị máy chủ bằng khóa công khai.

Lần đầu tiên, phần mềm quản lý tương tác với Let’s Encrypt, nó tạo ra một cặp khóa mới và chứng minh với Let’s Encrypt CA rằng máy chủ đang kiểm soát một hoặc vài tên miền.

Để khởi động quá trình này, trình quản lý yêu cầu Let’s Encrypt CA cung cấp thông tin cần thiết để chứng minh rằng nó đang kiểm soát example.com. Let’s Encrypt sẽ xem xét và đưa ra các yêu cầu, bạn cần hoàn thành nó để chứng minh mình có quyền kiểm soát tên miền. Ta có 2 lựa chọn:

- Cung cấp một bản ghi DNS dưới tên example.com

- Cung cấp nguồn HTTP dưới URL được biết đến trên https://example.com/

<img src="https://imgur.com/MILnnsm.png">

Sau khi hoàn thành các yêu cầu, Let’s Encrypt sẽ cung cấp cho trình quản lý chứng chỉ một cặp khóa riêng để chứng minh rằng nó kiểm soát cặp khóa.

Đến đây, trình quản lý đặt một tập tin trên đường dẫn được chỉ định trên trang web https://example.com. Trình quản lý cũng ký một khóa riêng, sau khi hoàn thành sẽ thông báo cho CA rằng nó đã hoàn thành xác nhận.

### 4.2. Open SSL

Open SSL là một thư viện nguồn mở được sử dụng để mã hóa dữ liệu và triển khai các giao thức mạng. Được phát hành lần đầu tiên vào năm 1998, nó có sẵn trong các hệ thống Linux, Windows, macOS và BSD. OpenSSL cho phép người dùng thực hiện các tác vụ liên quan đến SSL khác nhau, bao gồm CSR (Yêu cầu ký chứng chỉ), tạo khóa riêng và cài đặt chứng chỉ SSL.

### 4.3. CloudFlare

- **Flexible SSL**: Kiểu này CloudFlare sẽ hỗ trợ người truy cập vào website của bạn thông qua giao thức HTTPS, nhưng dữ liệu gửi từ CloudFlare về máy chủ sẽ không được mã hóa. Và bạn không cần cài chứng chỉ SSL bên trong server. Có thể sử dụng bất cứ website nào, từ Shared Host đến máy chủ riêng và không cần thiết lập gì thêm.

- **Full SSL**: Kiểu này CloudFlare sẽ hỗ trợ người truy cập vào website thông qua giao thức HTTPS và dữ liệu từ CloudFlare gửi về máy chủ cũng sẽ được mã hóa. Tuy nhiên bạn phải có một chứng chỉ SSL, nhưng CloudFlare sẽ không xác thực chứng chỉ này nên bạn có thể sử dụng chứng chỉ tự ký, hoặc tạo chứng chỉ của CloudFlare. Và tài khoản bạn phải là tài khoản Pro mới có thể sử dụng chứng chỉ riêng trên CloudFlare.

- **Full SSL (strict)**: Giống như kiểu Full SSL nhưng CloudFlare sẽ xác thực chứng chỉ này, chứng chỉ của bạn phải mua hoặc sử dụng Let’s Encrypt. Và tài khoản của bạn phải là Pro mới có thể sử dụng chứng chỉ riêng.
Ở phần dưới mình sẽ hướng dẫn chi tiết cách sử dụng từng loại.