# Tìm hiểu về Query trong SQL

## 1. Query là gì?

Rất đơn giản, một query nghĩa là một yêu cầu truy vấn thông tin. Vậy, query trong ngôn ngữ lập trình là gì? Cũng vậy thôi – thông tin ở đây sẽ là thông tin trích xuất từ một database – cơ sở dữ liệu. Query dùng để thực hiện các thao tác lên dữ liệu đó (data manipulation) – thêm, xóa, thay đổi.

Tuy nhiên, bạn sẽ không thể nhận được bất kỳ thông tin, dữ liệu nào nếu chỉ thực hiện một lệnh truy vấn tùy ý. Query của bạn phải dựa trên một cấu trúc code được định sẵn mà cơ sở dữ liệu có thể hiểu được. Cấu trúc code này có thể xem như là ngôn ngữ truy vấn – query language.

Tiêu chuẩn hiện nay của ngôn ngữ truy vấn là Structured Query Language (SQL).

## 2. Hoạt đông của Query

Hãy nói về một truy vấn trong đời sống hằng ngày trước. Ví dụ bạn muốn mua một ly Cà Phê Sữa Đá trong Quán Cafe. Bạn sẽ đưa ra yêu cầu “Cho mình ly cà phê sữa đá?”. Nhân viên pha chế sẽ `hiểu` yêu cầu của bạn và thực hiện đơn hàng.

Một query hoạt động tương tự. Bạn sẽ sử dụng ngôn ngữ query để gửi yêu cầu bạn muốn. Bất kể bạn sử dụng SQL hay ngôn ngữ nào khác, miễn là cả database và bạn hiểu và sử dụng chung 1 ngôn ngữ bạn sẽ có thể thực hiện truy vấn và nhận kết quả đúng như mong muốn.

Chắc bạn tưởng thực hiện truy vấn là cách duy nhất để lấy dữ liệu. Không hẵn, trên thực tế có nhiều cách khác để thực hiện việc này miễn là database software. Chúng tôi tổng hợp các cách để lấy dữ liệu như sau:

- **Sử dụng tham số có sẵn**: Phần mềm mặc định có sẵn các tham số trong menu của nó. Người dùng có thể chọn, hệ thống sẽ hướng dẫn bạn cách để lấy kết quả mong muốn. Dễ thực hiện, nhưng không linh hoạt và có nhiều hạn chế về cách vận hành.

- **Sử dụng cấu trúc gợi ý**: Hệ thống sẽ hiển thị một bộ code cho bạn với các khoảng trống để điền vào, bạn có thể điền thêm giá trị là được.

- **Ngôn ngữ Query**: Bạn đã biết có nhiều ngôn ngữ query. Bạn sẽ phải viết truy vấn nếu muốn sử dụng dữ liệu. Phương pháp này đòi hỏi bạn có kiến thức về ngôn ngữ query đang được database software của bạn sử dụng. Mặc dù hơi phúc tạp nhưng nó cho bạn toàn quyền kiểm soát dữ liệu.

## 3. Ví dụ về một Query

Ví dụ bạn cần lấy một thông từ trong bảng khảo sát sau:

|ID|Name|Sex|Age|Occupation|
|--|----|---|---|----------|
|1 |John|Nam|17	|Student   |
|2 |Peter|Nam|26|Unemployed|
|3 |Margareth|Nu|34|Teacher|
|4 |Lea |Nu | 34|Unemployed|

- Chọn chỉ cột “Name” và “Occupation” từ bảng “participant”.

`SELECT Name, Occupation FROM Participant`

|Name|Occupation|
|----|----------|
|John|Student   |
|Peter|Unemployed|
|Margareth|Teacher|
|Lea |Unemployed|

- Xóa dữ liệu từ những người đang không đi làm.

`DELETE FROM Participant WHERE Occupation = ‘Unemployed’`

|ID|Name|Sex|Age|Occupation|
|--|----|---|---|----------|
|1 |John|Nam|17	|Student   |
|4 |Lea |Nu | 34|Unemployed|

- Thêm một dòng vào trong bảng một người có tên Mario, 67 tuổi, đã nghĩ hưu.

`INSERT INTO Participant (ID, Name, Sex, Age, Occupation) VALUES (‘5’, ‘Mario’, ‘Nam’, ‘67’, ‘Retired’)`

|ID|Name|Sex|Age|Occupation|
|--|----|---|---|----------|
|1 |John|Nam|17	|Student   |
|2 |Peter|Nam|26|Unemployed|
|3 |Margareth|Nu|34|Teacher|
|4 |Lea |Nu | 34|Unemployed|
|5 |Mario|Nam|67|Retired   |

## Kết luận

Một vài lệnh SQL, như những lệnh ở trên là tiêu biểu của những gì ngôn ngữ truy vấn có thể thực hiện. Nó giúp bạn xử lý dữ liệu hiệu quả. Hãy tưởng tượng bạn có hàng ngàn dòng dữ liệu. Kiểm soát dữ liệu như vậy không còn khó nữa. Hơn nữa, ngôn ngữ query rất dễ dùng, vì nó có ý nghĩa và dễ học sau khi bạn đã hiểu các luật cơ bản. 

Ta có thể hiểu cấu trúc, ý nghĩa của từng lệnh trong SQL [tại đây](https://quantrimang.com/13-cau-lenh-sql-quan-trong-programmer-nao-cung-can-biet-136595)

## Tài liệu tham khảo

https://mariadb.com/kb/vi/basic-sql-statements/

https://quantrimang.com/13-cau-lenh-sql-quan-trong-programmer-nao-cung-can-biet-136595

https://www.hostinger.vn/huong-dan/query-la-gi-giai-thich-ve-truy-van-database/