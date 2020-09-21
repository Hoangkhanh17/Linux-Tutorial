# Hệ thống Linux ngốn RAM như thế nào ?

## 1. Cơ chế sử dụng RAM

Linux mượn một số phần RAM không được sử dụng để làm bộ nhớ đệm cho hệ thống `(Disk Caching)`. Điều này có vẻ như đang chiếm dụng RAM nhưng thực tế thì không phải vậy.

Linux mượn phần RAM không sử dụng làm bộ nhớ đệm làm tăng cường tốc độ đọc ghi dữ liệu của ổ cưng . Thực tế thì cơ chế này đã rất hoàn thiện và không hề có điểm yếu, trừ việc gây khó hiểu cho người mới.

Nếu khi Ứng dụng cần RAM hoặc cài đặt thêm Ứng dụng, thì Linux lấy lại 1 phần RAM đã được chuyển đội làm bộ nhớ đệm. Bộ nhơ đệm sẽ luôn luôn trả lại cho app ngay lập tức khi chúng cần.

## 2. Cản trở cơ chế sử dụng RAM

Thực tế thì chúng ta không thể cản trở hay dùng nó lại. Lý do duy nhất khiến bất kì ai muốn loại bỏ cơ chế này đó là họ nghĩ rằng cơ chế này sẽ lấy đi memory của các app đang chạy. Cơ chế này giúp các application load nhanh hơn và mượt mà hơn mà thôi. Và chính vì thế chúng ta không có lý do gì để loại bỏ nó.

## 3. Tìm hiểu Lệnh Free

Ta thường sử dụng lệnh `free` để kiểm tra Memory mà hệ thống có và dung lượng đang sử dụng. Trong đó:

- used là phần memory đã bị các applications sử dụng.

- free là phần memory chưa sử dụng cho bất kì điều gì.

Tuy nhiên ta cần tính đến phần memory đã được sử dụng cho `thứ gì đó khác` nhưng vẫn có thể cấp phát cho một application khác khi chúng cần.

|Memory|Meaning|Linux Meaning|
|-------|-------|-------------|
|Sử dụng bởi các app|Used|Used|
|Đã sử dụng nhưng vẫn có thể cấp phát cho app khi cần|Free (or Available)|Used (and Available)|
|Không sử dụng|Free|Free|

Thứ gì đó khác ở đấy đó là thứ mà `free` gọi là `buff/cache`. Vì chúng ta và Linux có nhưng cách hiểu thuật ngũ là khác nhau nên khiến chúng ta nghĩ rằng mình sắp hết Memory còn thực tế thì không phải vậy.

## 4. Khi nào cần lo lắng về Memory

Sau khi đã hiểu khá khá thì chúng ta có thể dừng việc lo lắng khi:

- `free memory` tiến dần đến sát giá trị 0.

- `used memory` tiến dần đến sát giá trị total.

- `available memory` tiến dần đến sát giá trị max của nó (~ 80% total).

- `swap used` không thay đổi .

Chúng ta chỉ nên lo lắng khi:

- `available memory` tiến dần đến sát giá trị 0.

- `swap used` tăng liên tục đến gần sát total

- `dmesg | grep oom-killer` show ra thứ gì đó (Các Process bị dùng do hết RAM)

## Kết luận

Bài viết giúp chúng ta nắm rõ hơn về việc kiểm soát RAM cho Linux.

## Tài liệu tham khảo

https://viblo.asia/p/linux-ate-my-ram-OeVKBM9M5kW

https://www.linuxatemyram.com/

https://www.cloudways.com/blog/linux-ate-my-ram-memory-myth-busted/