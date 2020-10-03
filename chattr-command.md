# Giới thiệu lệnh chattr để bảo vệ sự toàn vẹn của file

## Mục lục

## 1. Lệnh Chattr là gì ?

`chattr` là viết tắt của Change Attribute. Đây là câu lệnh cho phép bạn thay đổi thuộc tính của file giúp bảo vệ file khỏi bị xóa hoặc ghi đè nội dung, dù cho bạn có đang là user root đi nữa.

```
chattr [operator] [flags] [filename]
```

Các operator:

- `+`: Thêm thuộc tính cho file

- `-`: Gỡ bỏ thuộc tính khỏi file

- `=`: Giữ nguyên thuộc tính của file

Các flag (hay các thuộc tính của file):

Một số flag thường dùng:

- `i`: Flag này khiến file không thể rename, không thể tạo symlink, không thể thực thi, không thể write. Chỉ có user root mới set và unset được thuộc tính này.

- `a`: Flag này khiến file không thể rename, không thể tạo symlink, không thể thực thi, chỉ có thể nối thêm nội dung vào file. Chỉ có user root mới set và unset được thuộc tính này.

- `d`: file có thuộc tính này sẽ không được backup khi tiến trình dump chạy

- `S`: Nếu một file có thuộc tính này bị sửa, thay đổi sẽ được cập nhật đồng bộ trên ổ cứng.

- `A`: Khi file có thuộc tính được truy cập, giá trị atime của file sẽ không thay đổi.

**Lưu ý**: Tất cả các lệnh dưới đây đều chạy dưới quyền user root.

## 2. Cách thêm thuộc tính trên file để bảo vệ file khỏi bị xóa

Chúng ta thêm thuộc tính i (immutable) cho file:

`chattr +i cloud365.txt`

Xem lệnh trên đã có hiệu lực chưa bằng cách dùng lệnh `lsattr`:

`----i----------- ./cloud365.txt`