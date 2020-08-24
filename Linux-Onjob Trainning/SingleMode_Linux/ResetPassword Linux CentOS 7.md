# Reset Password Root CentOS 7

## Mục lục

Việc quên password là việc xảy ra thường xuyên trong việc đăng nhập vào bất kỳ hệ thống cũng như ứng dụng nào. Hệ thống Linux cũng vậy, để reset lại mật khẩu cho `root` cũng không quá phức tạp.

### Bước 1: Khởi động lại hệ thống

Để reset mật khẩu root, trước tiên chúng ta khởi động lại máy chủ.

Khi hệ thống boot lên màn hình GRUB2, ta gõ phím `e` để hiện lên màn hình chỉnh sửa.

<img src="https://imgur.com/0K0c9h2.png">

### Bước 2: Chỉnh sửa các thông số cần thiết

- Tìm đến dòng `Linux 16`, xoá thông số “rhgb quiet” để kích hoạt log message hệ thống khi thực hiện đổi mật khẩu root, sau đó thêm vào dòng `init=/bin/bash/`

<img src="https://imgur.com/PwJKoue.png">

- Sau đó Ctrl X để lưu và tự động boot vào môi trường initramfs.

### 3. Bước 3: remount filesystem và chuyển chế độ chroot

- Hệ thống filesystem hiện tại đang ở chế độ “read only” được mount ở thư mục /sysroot/, để thực hiện khôi phục mật khẩu root thì ta cần thêm quyền ghi (write) trên filesystem. Ta sẽ tiến hành remount lại filesystem /sysroot/ với quyền đọc-ghi (read-write).

- Kiểm tra lại xem filesystem đã được remount quyền đọc-ghi hay chưa.

`mount -o remount, rw /sysroot`

`mount | grep -w “/sysroot“`

- Lúc này filesystem đã được mount và ta sẽ chuyển đổi sang môi trường filesystem .

`chroot /sysroot`
 
- Tiến hành reset password user root.

`passwd root`

- Chạy lệnh sau để update lại các thông số cấu hình SELINUX, nếu bạn có sử dụng SELINUX. Nguyên nhân khi ta update file /etc/passwd chứa mật khẩu thì các thông số SELINUX security contex sẽ khác nên cần update lại.

`touch /.autorelabel`

`touch /.autorelabel`

- Remount filesystem “/“ ở chế độ read-only.

`mount -o remount, ro /`

- Thoát môi trường chroot và Khởi động lại hệ thống.

`exit`

`exec /sbin/reboot`
 
- Bạn sẽ thấy hệ thống reboot và chậm hơn bình thường , do hệ thống đang tiến hành hoạt động SELINUX relabel. Sau khi boot vào hệ thống prompt console thành công thì bạn có thể đăng nhập bằng mật khẩu mới.