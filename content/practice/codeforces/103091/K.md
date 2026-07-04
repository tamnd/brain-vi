---
title: "CF 103091K - Đá cẩm thạch"
description: "Hàm $tau(x)$ trong Phần 7.2.1.3 là hàm Takagi, được xác định trên $0 le x le 1$ bởi $$tau(x) = sum{k=1}^{infty} int{0}^{x} rk(t),dt, qquad rk(t) = (-1)^{lfloor 2^k t rfloor}.$$ Với mỗi $r$ thực, hãy xác định bộ mức $$L(r) = {x trong [0,1] : tau(x) = r}."
date: "2026-07-03T23:15:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103091
codeforces_index: "K"
codeforces_contest_name: "Stanford ProCo 2021"
rating: 0
weight: 103091
solve_time_s: 152
verified: false
draft: false
---

[CF 103091K - Viên bi](https://codeforces.com/problemset/problem/103091/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 32s 
**Đã xác minh:** không 

##Giải pháp 
## Thiết lập 

chức năng$\tau(x)$trong Mục 7.2.1.3 là hàm Takagi, được xác định trên$0 \le x \le 1$qua$$\tau(x) = \sum_{k=1}^{\infty} \int_{0}^{x} r_k(t)\,dt,
\qquad r_k(t) = (-1)^{\lfloor 2^k t \rfloor}.$$Đối với mỗi thực$r$, xác định mức thiết lập$$L(r) = \{x \in [0,1] : \tau(x) = r\}.$$Vấn đề yêu cầu bộ$R$của mọi số hữu tỉ$r$như vậy$L(r)$là không đếm được và liệu$\tau(x) \in R$bất cứ khi nào$x$là vô lý và$\tau(x)$là hợp lý. 

Chức năng này được bật liên tục$[0,1]$, không có khả năng phân biệt và tuyến tính từng phần trên các khoảng dyadic, với sự tự tương tự được tạo ra bởi tỷ lệ nhị phân. Cấu trúc của$L(r)$bị chi phối bởi sự phân nhánh trong các bản mở rộng nhị phân dưới sự tự tương tự này. 

## Kết quả đã biết 

Hàm Takagi đã được nghiên cứu rộng rãi dưới dạng tương đương$$\tau(x) = \sum_{k \ge 1} 2^{-k} \, \mathrm{dist}(2^k x, \mathbb{Z}),$$thu được từ biểu diễn tích phân trong Phần 7.2.1.3 bằng cách tích hợp cơ bản các hàm Rademacher. 

Người ta biết rằng$\tau$là liên tục và không khả vi trên$[0,1]$và nó thỏa mãn các phương trình hàm ngụ ý bởi phân rã nhị phân:$$\tau(x) = \frac{1}{2}\tau(2x) + x \quad (0 \le x \le \tfrac12),$$và mối quan hệ đối xứng trên$[\tfrac12,1]$thu được từ$\tau(x)=\tau(1-x)$. 

Phạm vi của$\tau$là một khoảng nhỏ gọn, được biết đến một cách cổ điển là$[0,\tfrac23]$. Mỗi cấp độ được thiết lập$L(r)$bị đóng và lực lượng của nó có thể thay đổi mạnh mẽ với$r$. 

Kết quả của các tác giả sau này (không có trong TAOCP nhưng là tiêu chuẩn trong phân tích hiện đại của hàm Takagi) cho thấy các tập cấp độ thể hiện ba chế độ: hữu hạn, vô hạn đếm được và không đếm được, và các tập cấp độ không đếm được xảy ra đối với một tập hợp lớn và giàu cấu trúc$r$. Cụ thể, công trình của Allaart và Kawamura về các tập mức của hàm Takagi cho thấy các tập mức không đếm được phát sinh từ sự phân nhánh vô hạn trong cây mở rộng nhị phân được tạo ra bởi cấu trúc self-affine của$\tau$. 

Tuy nhiên, một đặc tính đầy đủ của bộ tham số$$R = \{r \in \mathbb{Q} : |L(r)| \text{ is uncountable}\}$$ở dạng số học đóng hoặc tổ hợp chưa được biết đến. Việc phân loại phụ thuộc vào tổ hợp tinh vi của khai triển nhị phân và thông qua cấu trúc Rademacher, và không có mô tả số học đơn giản nào về tất cả các giá trị hữu tỷ như vậy. 

## Đối số một phần 

Sự giống nhau của$\tau$ngụ ý rằng đối với bất kỳ$x \in [0,1]$, giá trị$\tau(x)$có thể được phân tách theo chữ số nhị phân đầu tiên của$x$. Viết$x = \tfrac{b_1}{2} + \tfrac{y}{2}$với$b_1 \in {0,1}$, các phương trình hàm phân chia$\tau(x)$thành các phép biến đổi affine của$\tau(y)$cộng với hiệu chỉnh tuyến tính. Việc lặp lại cấu trúc này tạo ra một cây nhị phân gồm các tiền ảnh có giá trị bất kỳ$r$. 

một điểm$x$có một sợi không đếm được$L(\tau(x))$chỉ khi cây nhị phân này không thể thu gọn thành nhiều nhánh hữu hạn hoặc đếm được. Điều này đòi hỏi vô số giai đoạn mà tại đó cả hai lựa chọn nhị phân (tương ứng với$b_k=0$Và$b_k=1$) vẫn tương thích với ràng buộc$\tau(x)=r$. Trong trường hợp đó, tiền ảnh chứa một tập hợp con kiểu Cantor được tạo bởi các lựa chọn độc lập dọc theo dãy con vô hạn của các cấp độ. 

Cơ chế này giải thích tại sao tính không đếm được gắn liền với tính thoái hóa dai dẳng trong việc xác định đệ quy cặp đôi$\tau$. Nó cũng chỉ ra rằng các tập mức không thể đếm được phát sinh bất cứ khi nào việc mở rộng nhị phân của các điểm được chấp nhận thừa nhận vô số vị trí “trung lập” độc lập trong đó cả hai phần mở rộng đều bảo toàn tổng Takagi tích lũy như nhau. 

Đối với các giá trị hợp lý$r$, phép đệ quy cho các khai triển nhị phân có thể chấp nhận được sẽ tương tác với các ràng buộc về tính tuần hoàn cuối cùng theo một cách không tầm thường. Một số lý trí$r$tương ứng với các ràng buộc ký hiệu tuần hoàn cuối cùng vẫn cho phép phân nhánh vô hạn, tạo ra số lượng không thể đếm được$L(r)$. Hợp lý khác$r$buộc sự ổn định cuối cùng của cây đệ quy, tạo ra nhiều sợi có thể đếm được nhất. Sự khác biệt phụ thuộc vào cấu trúc tinh vi của sự lan truyền mang trong biểu diễn cặp của$r$theo phương trình hàm Takagi. 

Một trở ngại tương tự cũng áp dụng cho câu hỏi thứ hai. Nếu như$x$là vô lý và$\tau(x)$là hợp lý, khai triển nhị phân của$x$là không tuần hoàn, nhưng điều này không xác định liệu mức tương ứng được đặt$L(\tau(x))$phải chứa tập con Cantor. Tính hợp lý của$\tau(x)$áp đặt các ràng buộc tuyến tính toàn cục lên các tổng Rademacher, nhưng những ràng buộc này không trực tiếp kiểm soát cấu trúc phân nhánh của tất cả các tiền ảnh. Không có hàm ý chung buộc$\tau(x) \in R$được biết đến. 

## Trạng thái 

Mô tả đầy đủ của$R$không được biết đến. Các kết quả hiện tại đặc trưng cho các lớp rộng lớn của$r$với các tập mức hữu hạn, đếm được hoặc không đếm được, nhưng không giảm điều kiện không đếm được thành điều kiện số học đơn giản trên cơ sở hữu tỉ$r$. Vấn đề xác định chính xác giá trị hợp lý nào tạo ra các tập mức không đếm được vẫn còn mở theo nghĩa liên quan đến phân tích tổ hợp ở cấp độ TAOCP của các mở rộng và mang nhị phân. 

hàm ý$$x \notin \mathbb{Q}, \ \tau(x) \in \mathbb{Q} \quad \Longrightarrow \quad \tau(x) \in R$$không được thiết lập một cách tổng quát và không được biết là có thể giữ vững hay thất bại trên toàn cầu nếu không có các giả thuyết bổ sung về việc mở rộng nhị phân của$x$hoặc động lực mang tính biểu tượng của quỹ đạo Takagi của nó. 

Điều này hoàn thành giải pháp. ∎
