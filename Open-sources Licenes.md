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

    Copyright [yyyy] [name of copyright owner]

    Licensed under the Apache License, Version 2.0 (the “License”); you may not use this file except in compliance with the License. You may obtain a copy of the License at http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software distributed under the License is distributed on an “AS IS” BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied. See the License for the specific language governing permissions and limitations under the License.

