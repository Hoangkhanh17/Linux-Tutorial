# Tìm hiểu về Open-source Licenses

## Mục lục

## Giới thiệu chung

Phần mềm mã nguồn mở (Open Source) là những phần mềm được cung cấp dưới cả dạng mã và nguồn , không chỉ là miễn phí về giá mua mà chủ yếu là miễn phí về bản quyền : người dùng có quyền sửa đổi, cải tiến, phát triển, nâng cấp theo một số nguyên tắc chung quy định trong giấy phép phần mềm nguồn mở mà không cần xin phép ai, điều mà họ không được phép làm đối với các phần mềm nguồn đóng (tức là phần mềm thương mại)

Tuy nhiên, Giấy phép mã nguồn mở đặt ra các quy định buộc người sử dụng phần mềm đó phải tuân theo. Giấy phép mã nguồn mở được sử dụng cho các phần mềm nguồn mở.

## Phần 1. Giới thiệu về các giấy phép mã nguồn mở

**Tính pháp lý của Giấy phép mã nguồn mở:** Giấy phép mã nguồn mở vẫn duy trì xác nhận về bản quyền của tác giả gốc đối với phần mềm, tuy nhiên được đưa thêm các điều khoản để các hành vi phân phối, sửa đổi, sao chép… các phần mềm này trở thành hợp pháp. Vì vậy giấy phép này và các điều quy định trong nó có giá trị về mặt pháp lý (luật pháp được nhắc đến ở đây là luật của Hoa Kì)

**Phân loại:** Giấy phép mã nguồn mở có thể được chia thành 2 loại chính:

- Những giấy phép không quy định bất cứ sự hạn chế nào trong việc sử dụng mã nguồn (còn có thể gọi là các giấy phép không bảo hộ vì chúng không bảo vệ mã nguồn mở khỏi việc bị sử dụng trong các phần mềm không phải là mã nguồn mở).

    ***Các giấy phép thuộc loại này:*** Apache Software License v.1.1, BSD License, Intel Open Source License for CDSA/CSSM Implementation, MIT License, Sun Industry Standards Source License, W3C Software Notice and License…

- Những giấy phép quy định các hạn chế trong việc sử dụng mã nguồn (còn có thể gọi là các giấy phép bảo hộ vì chúng đảm bảo rằng các mã nguồn mở khi được sử dụng trong bất cứ tình huống nào sẽ vẫn được công khai/miễn phí.)

    ***Các giấy phép thuộc loại này:*** Apple Public Source License v.1.2, Common Public License v.1.0; GNU General Public License v.2.0, IBM Public License v.1.0, Mozilla Public License v.1.0 and v.1.1, Nokia Open Source License v.1.0a, Open Software License v.1.1, Python License; Python Software Foundation License v.2.1.1, Sun Public License v.1.0…

Người giữ bản quyền mã nguồn khi sử dụng loại giấy phép không bảo hộ sẽ giữ lại bản quyền của họ đối với mã nguồn, và cấp cho người được cấp bản quyền (có thể hiểu là người sử dụng sản phẩm, mã nguồn) tất cả các quyền thuộc về bản quyền của mã nguồn đó.

Người giữ bản quyền mã nguồn khi sử dụng loại giấy phép có bảo hộ giữ lại bản quyền của họ đối với mã nguồn, và cấp cho người được cấp bản quyền tất cả các quyền thuộc về bản quyền của mã nguồn đó, nhưng có ít nhất một điều kiện, thông thường là việc phân phối lại phần mềm/mã nguồn đó, dù đã được sửa đổi hay chưa, đều phải sử dụng cùng loại giấy phép ban đầu.

**Người viết giấy phép:** Giấy phép mã nguồn mở do một số công ty, tổ chức lập ra để quy định về trách nhiệm của người sử dụng đối với một phần mềm/mã nguồn mở. Hiện tại, công ty (tổ chức) OSI (Open Source Initiative) là người đưa ra định nghĩa về mã nguồn mở (OSD – Open Source Definition) được cộng đồng công nhận rộng rãi. Các giấy phép mã nguồn mở đa phần được xây dựng dựa trên OSD.

**Quy trình thông qua một giấy phép mã nguồn mở tại OSI:**

- Cộng đồng thẩm định giấy phép sẽ thảo luận trong ít nhất 30 ngày.

- Các ý kiến từ cộng đồng sẽ được tổng kết và đưa lên ban giám đốc OSI.

- Ban giám đốc OSI sẽ đưa ra quyết định cuối cùng, hoặc yêu cầu các thông tin bổ sung, trong lần họp định kì tháng sau.

- Cộng đồng thẩm định sẽ được thông báo về quyết định của ban giám đốc OSI. Nếu giấy phép đó được chấp thuận, nó sẽ được đưa lên website của OSI.

Chi tiết về quá trình thẩm định một giấy phép tại OSI, cùng các hướng dẫn dành cho những ai muốn tự soạn ra một giấy phép riêng của bản thân mà được OSI công nhận, có thể xem [tại đây](http://www.opensource.org/approval)

***Mục đích sử dụng:*** Các giấy phép mã nguồn mở được sử dụng để đảm bảo rằng các phần mềm, mã nguồn có sử dụng giấy phép này luôn là mã nguồn mở, phù hợp với OSD.

***Cách sử dụng giấy phép mã nguồn mở:*** Đối với nhà phát hành phần mềm, để có thể sử dụng một giấy phép mã nguồn mở có sẵn vào trong phần mềm của mình thì thông thường cần phải thực hiện các công việc sau:

- Đính kèm giấy phép vào trong phần mềm của mình (được hiểu là đưa nội dung bản giấy phép vào trong bộ cài đặt, hoặc vào một file văn bản đi kèm với các file của chương trình)

- Điền các thông tin cần thiết vào trong giấy phép: mỗi giấy phép đều có hướng dẫn việc làm thế nào để sử dụng chúng, thông thường là điền tên tác giả, năm phát hành, công ty … vào trong các trường tương ứng được quy định sẵn của giấy phép.

## Phần 2. Các loại giấy phép mã nguồn mở thông dụng

### 1. Apache License 2.0

Thời gian phát hành: 01/2004.

Giấy phép **Apache** là một giấy phép phần mềm tự do của **Quỹ phần mềm Apache (Apache Software Foundation – ASF)**. Giấy phép **Apache** trao cho người dùng phần mềm nguồn mở, quyền tự do sử dụng phần mềm với bất kì mục đích nào, phân phối chỉnh sửa, và phân phối bản sửa đổi của phần mềm, theo các điều khoản của giấy phép mà không lo vấn đề bàn quyền.

Các điều khoản của giấy phép:

- Giấy phép Apache cho phép người dùng tự do sử dụng phần mềm với bất kì mục đích nào, tự do phân phối, tự do sửa đổi, tự do phân phối bản sửa đổi mình làm.

- Giấy phép Apache không yêu cầu bản sửa đổi của phần mềm phải được phân phối dưới cùng giấy phép với bản gốc, cũng không yêu cầu bản sửa đổi phải được phân phối dưới dạng mã nguồn mở. Giấy phép Apache chỉ yêu cầu có một thông báo nhắc nhở người nhận rằng giấy phép Apache đã được sử dụng trong sản phẩm họ nhận được.

=> Người sử dụng phần mềm được quyền sử dụng chương trình và mã nguồn theo cách họ muốn, kể cả việc giữ lại mã nguồn cho riêng mình.

Giấy phép Apache không yêu cầu trích dẫn toàn bộ giấy phép vào sản phẩm hay tệp tin đính kèm bản phân phối, mà chỉ cần thêm vào phần thông báo có chứa đường link tới website chứa giấy phép với nội dung như sau:

```
Copyright [yyyy] [name of copyright owner]

Licensed under the Apache License, Version 2.0 (the “License”); you may not use this file except in compliance with the License. You may obtain a copy of the License at http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an “AS IS” BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License.
```

### 2. BSD 3-Clause “New” or “Revised” license

#### a. Nhà phát hành
Thời gian phát hành: 22/07/1999.

Giấy phép ***BSD (Berkeley Software Distribution License)*** có thể nói là lâu đời nhất trong các giấy phép nguồn mở, nó đã và đang tồn tại ở một số dạng kể từ những năm 1980.

#### b. Nội dung chính

***Điều khoản của giấy phép:*** Tái phân phối và sử dụng ở dạng mã nguồn và nhị phân , có hoặc không có sửa đổi mã nguồn , đều được cho phép miễn là các điều khoản sau được đáp ứng:

- Việc phân phối lại mã nguồn phải giữ lại thông báo bản quyền , danh sách các điều kiện và tuyên bố từ chối trách nhiệm.

- Việc phân phối lại dưới dạng nhị phân phải sao chép thông báo bản quyền , danh sách các điều kiện và tuyên bố từ chối trách nhiệm trong tài liệu và/hoặc các tài liệu khác được cung cấp bởi bản phân phối.

- Tên của người giữ bản quyền cũng như tên của những người đóng góp của nó có thể được sử dụng để xác nhận hoặc quảng cáo các sản phẩm có nguồn gốc từ phần mềm này mà không có sự cho phép trước bằng văn bản cụ thể.

Nội dung License: https://opensource.org/licenses/BSD-3-Clause

### 3. BSD 2-Clause “Simplified” or “FreeBSD” license

Ngày phát hành: 04/1999.

Giấy phép **BSD 2-Clause “Simplified” or “FreeBSD” license** về cơ bản giống với giấy phép **BSD 3-Clause**. Nhưng BSD 2-Clause yêu cầu *“Tên của những người đóng góp trước đó không được sử dụng để quảng cáo cho bất kỳ phiên bản phái sinh nào mà không có được sự cho phép bằng văn bản của họ”*. **BSD 3-Clause** đã giảm bớt sự phiền hà khi sử dụng phần mềm nguồn mở theo giấy phép BSD.

Nội dung License: https://opensource.org/licenses/BSD-2-Clause

### 4. GNU General Public License ( GPL )

#### a) Nhà phát hành
Giấy phép GNU hiện đang có 2 phiên bản được sử dụng phổ biến là GPL-2.0 và GPL-3.0.

Nội dung license: https://www.gnu.org/licenses/gpl.html

#### b) Nội dung chính

**Quyền lợi** khi sử dụng phần mềm áp dụng giấy phép GPL:

- Được sao chép và phân phối chương trình, được yêu cầu trả phí cho việc phân phối đó.

- Được thay đổi chương trình để sử dụng cho mục đích cá nhân.

- Được phân phối bản đã được thay đổi đó.

**Nghĩa vụ:**

- Khi sao chép và phân phối chương trình, phải đính kèm các thông báo về bản quyền gốc và không bảo hành(trừ trường hợp có văn bản thêm về quy định bảo hành)

- Khi phân phối bản đã được thay đổi bởi bản thân , phải chú thích rõ đó là bản đã được thay đổi, các thành phần được thay đổi, và áp dụng giấy phép GNU cho bản đã được thay đổi đó.

- Khi phát hành chương trình phải công khai mã nguồn của chương trình, đồng thời phải công bố mã nguồn của chương trình trong tối thiểu 3 năm mà không được đòi một khoản phí nào từ những người yêu cầu mã nguồn trừ chi phí vận chuyển hay tương đương.

### 5. GNU Library or “Lesser” General Public License (LGPL)

Thời gian phát hành: Phiên bản mới nhất **LGPL-3.0** phát hành năm 2007.

Nội dung License: https://www.gnu.org/copyleft/lesser.html

- **LGPL** là một phiên bản sửa đổi của **GPL**.

- Giấy phép này thường bị hạn chế đối với các thư viện phần mềm.

- Cung cấp sự bảo vệ ít hơn so với **GPL**.

- Điều này cho phép các chương trình không phải là Open source có thể truy cập hoặc liên kết tới các thư viện nguồn mở mà không phải công khai mã nguồn như giấy phép **GPL**.

### 6. MIT License

Được phát hành bởi **Massachusetts Institute of Technology (MIT)**

Nội dung License: https://opensource.org/licenses/MIT

Giấy phép MIT là loại giấy phép cho phép sử dụng mã nguồn tự do nhất , nó có thể kết hợp với các mã nguồn khác và đảm bảo tương thích theo điều kiện của mọi loại giấy phép khác.

Với giấy phép MIT bạn có thể sử dụng , sao chép , sửa đổi , hợp nhất , xuất bản , phân phối và/hoặc bán các bản sao của phần mềm mà không vi phạm bản quyền. Bạn chỉ cần tuân thủ điều kiện duy nhất sau:

- Thông báo bản quyền và thông báo cho phép của phần mềm gốc sử dụng giấy phép MIT sẽ phải bao gồm trong tất cả các bản sao hoặc phần quan trọng của phần mềm.

