# Tấn công máy chủ bằng Scan password - Brute Force Password

## Mục lục

[Phần 1. Giới thiệu về Brute Force Password](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob%20Trainning/Security%20and%20Firewall/Brute%20Force/Brute%20Force%20Attack.md#ph%E1%BA%A7n-1-gi%E1%BB%9Bi-thi%E1%BB%87u-v%E1%BB%81-brute-force-password)

- [1. Brute Force là gì ?](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob%20Trainning/Security%20and%20Firewall/Brute%20Force/Brute%20Force%20Attack.md#1-brute-force-l%C3%A0-g%C3%AC-)

- [2. Khả năng của Brute Force](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob%20Trainning/Security%20and%20Firewall/Brute%20Force/Brute%20Force%20Attack.md#2-kh%E1%BA%A3-n%C4%83ng-c%E1%BB%A7a-brute-force)

- [3. Cách phòng chống Brute Force Attack](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob%20Trainning/Security%20and%20Firewall/Brute%20Force/Brute%20Force%20Attack.md#3-c%C3%A1ch-ph%C3%B2ng-ch%E1%BB%91ng-brute-force-attack)

[Phần 2. Brute Force Password](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob%20Trainning/Security%20and%20Firewall/Brute%20Force/Brute%20Force%20Attack.md#ph%E1%BA%A7n-2-brute-force-password)

## Phần 1. Giới thiệu về Brute Force Password

### 1. Brute Force là gì ?

Brute Force là một loại tấn công mạng, trong đó bạn có một phần mềm, xoay vòng các ký tự khác nhau, kết hợp để tạo ra một mật khẩu đúng. Phần mềm Brute Force Attack password cracker đơn giản sẽ sử dụng tất cả các kết hợp có thể để tìm ra mật khẩu cho máy tính hoặc máy chủ mạng.

Nó rất đơn giản và không sử dụng bất kỳ kỹ thuật thông minh nào. Vì phương pháp này chủ yếu dựa trên toán học, phải mất ít thời gian hơn để crack mật khẩu, bằng cách sử dụng các ứng dụng brute force thay vì tìm ra chúng theo cách thủ công.

Tấn công Brute Force là tốt hay xấu tùy thuộc vào người sử dụng nó. Nó có thể được bọn tội phạm mạng cố gắng sử dụng để hack vào một máy chủ mạng, hoặc nó có thể được một quản trị viên mạng dùng để xem mạng của mình được bảo mật có tốt không. Một số người dùng máy tính cũng sử dụng các ứng dụng brute force để khôi phục mật khẩu đã quên.

### 2. Khả năng của Brute Force

Nếu mật khẩu của bạn đang sử dụng tất cả các chữ cái thường và không có ký tự đặc biệt hoặc chữ số, chỉ mất 2-10 phút là một cuộc tấn công brute force có thể crack mật khẩu này. Ngược lại, một mật khẩu có sự kết hợp của cả chữ hoa và chữ thường cùng với một vài chữ số (giả sử có 8 chữ số) sẽ mất hơn 14-15 năm để bị crack.

Chúng ta có thể kiểm tra mức độ mạnh của mật khẩu mà ta đã đặt [tại đây](https://www.grc.com/haystack.htm). Kết quả tại trang web trên là tương đối, chúng ta vẫn nên đặt một mật khẩu phức tạp để tránh bị Brute Force.

**Brute Force SSH:** Việc đặt mặc định `port 22` cho SSH sẽ khiến cho hacker dễ dàng dò ra được thông tin tài khoản để SSH vào máy chủ. Nên việc sử dụng Brute Force SSH cũng khá dễ dàng.

### 3. Cách phòng chống Brute Force Attack

Cách dễ dàng nhất là đặt mật khẩu có mức độ phức tạp cao. Ví dụ:

- Có ít nhất một chữ hoa.

- Có ít nhất một chữ số.

- Có ít nhất một ký tự đặc biệt.

- Mật khẩu phải có tối thiểu 8-10 ký tự.

- Bao gồm ký tự ASCII, nếu bạn muốn.

Sử dụng các phần mềm chống tấn công để xác định IP đăng nhập sai nhiều lần và block IP đó. Ví dụ: Tham khảo phần mềm Fail2ban [tại đây](https://github.com/quanganh1996111/Linux-Tutorial/blob/master/Linux-Onjob%20Trainning/Security%20and%20Firewall/Fail2ban_with_CentOS7.md).

Đối với **Brute Force SSH**, ta có thể thay đổi `port 22` mặc định sang `port` khác để tránh bị scan SSH hoặc nâng cấp bảo mật cho SSH bằng cách sử dụng các key gen hoặc xác thực 2 bước.

## Phần 2. Brute Force Password

Để tiến hành Brute Force một máy chủ nào đó, ta có thể sử dung các phần mềm trên mạng hoặc tự tạo một Tool để quét.

Một số phần mềm phục vụ việc Brute Force hiện nay: Medusa, NCrack, Hydra,...

### Hydra

Hydra là tool được sử dụng phổ biến, rất mạnh mẽ. Nếu bạn sử dụng hệ điều hành Kali Linux, thì tool này đã được cài đặt sẵn. Các hệ điều hành Debian có thể cài đặt tool này cũng khá dễ dàng.

Có thể tham khảo demo Brute Force bằng Hydra [tại đây](https://tuanhdmsp.wordpress.com/2017/11/28/huong-dan-tan-cong-password-windows-su-dung-hydra-kali-linux/#:~:text=C%C3%B4ng%20c%E1%BB%A5%20Hydra%20s%E1%BB%AD%20d%E1%BB%A5ng,%C4%91%E1%BB%83%20t%C3%ACm%20ra%20m%E1%BA%ADt%20kh%E1%BA%A9u.)

### Ncrack

Ncrack cũng được cài đặt sẵn trong hệ điều hành Kali Linux. Việc cài đặt cho hệ điều hành Debian cũng tương tự Hydra.

### Medusa

Tham khảo [tại đây](https://news.cloud365.vn/brute-force-password-su-dung-medusa/)


## Kết luận

Nhìn chung các Tools hoặc việc tự tạo Tool riêng để Brute Force đều dựa trên việc truy quét các mật khẩu yếu, dễ bị quét. Chúng ta nên cảnh giác và sử dụng mật khẩu mạnh cũng như các phần mềm bảo mật để bảo vệ máy chủ của mình.

## Tài liệu tham khảo

https://quantrimang.com/cuoc-tan-cong-brute-force-la-gi-157987

https://anninhmang.edu.vn/thu-thap-ssh-username-password-voi-brute-force-attack/

https://news.cloud365.vn/brute-force-password-su-dung-medusa/