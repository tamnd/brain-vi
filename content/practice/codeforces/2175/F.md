---
title: "CF 2175F - Thông Điệp Bí Mật"
description: "Chúng ta được cho một đồ thị vô hướng có trọng số. Chúng ta phải chọn chính xác các cạnh $n-1$. Nếu các cạnh $n-1$ đó tạo thành một cây bao trùm thì sự lựa chọn bị cấm. Chúng ta muốn tổng trọng số tối thiểu có thể có trong số tất cả các lựa chọn của các cạnh $n-1$ không tạo thành cây."
date: "2026-06-09T04:34:41+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "implementation", "trees"]
categories: ["algorithms"]
codeforces_contest: 2175
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 1069 (Div. 2)"
rating: 3400
weight: 2175
solve_time_s: 137
verified: false
draft: false
---

[CF 2175F - Thông điệp bí mật](https://codeforces.com/problemset/problem/2175/F) 

**Đánh giá:** 3400 
**Tas:** cấu trúc dữ liệu, triển khai, cây 
**Thời gian giải:** 2m 17s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị vô hướng có trọng số. Chúng ta phải lựa chọn chính xác$n-1$các cạnh. 

Nếu những$n-1$các cạnh tạo thành một cây bao trùm, sự lựa chọn bị cấm. Chúng tôi muốn tổng trọng lượng tối thiểu có thể có trong số tất cả các lựa chọn về$n-1$các cạnh **không** tạo thành cây. 

Một biểu đồ trên$n$các đỉnh có chính xác$n-1$các cạnh là một cây khi và chỉ nếu nó được kết nối. Quan sát đó ngay lập tức đơn giản hóa vấn đề: chúng ta đang tìm tập trọng số tối thiểu của$n-1$các cạnh có đồ thị cảm ứng bị ngắt kết nối. 

Kích thước biểu đồ lớn. Trong tất cả các trường hợp thử nghiệm, cả tổng số đỉnh và tổng số cạnh đều lớn nhất$2 \cdot 10^5$. Bất kỳ thuật toán nào liệt kê các tập hợp con, cây bao trùm hoặc thậm chí tất cả các cặp tập hợp cạnh đều hoàn toàn không thể thực hiện được. Chúng ta cần một cái gì đó gần gũi$O(m \log m)$mỗi tập tin thử nghiệm. 

Trường hợp cạnh tinh tế xuất hiện khi nhẹ nhất trên toàn cầu$n-1$các cạnh đã bị ngắt kết nối. Ví dụ:```
n = 4
edges: 1, 2, 3, 100
```Việc chọn ba cạnh nhẹ nhất đã cho ra một biểu đồ không liên kết, vì vậy câu trả lời đơn giản là$1+2+3=6$. Giải pháp bắt đầu bằng cách xây dựng MST và sau đó buộc sửa đổi sẽ bỏ lỡ trường hợp này. 

Một trường hợp quan trọng khác là khi nhẹ nhất$n-1$các cạnh tạo thành một cây. Khi đó câu trả lời không nhất thiết phải có được bằng cách thay thế một cạnh cây tùy ý. Việc thay thế phải ngắt kết nối đồ thị. Ví dụ: nếu một cạnh phụ tạo ra một chu trình, việc loại bỏ một cạnh khỏi chu trình đó sẽ giữ cho đồ thị được kết nối và vẫn cho ra một cây, điều này bị cấm. 

Trường hợp góc cuối cùng xảy ra khi$m=n-1$. Đồ thị chứa đúng một tập hợp$n-1$các cạnh. Nếu tập hợp đó là cây thì không tồn tại câu trả lời hợp lệ và chúng ta phải in$-1$. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ kiểm tra mọi tập hợp con của$n-1$các cạnh, kiểm tra xem nó có tạo thành cây hay không và giữ trọng lượng tối thiểu trong số những cây không hợp lệ. có$$\binom{m}{n-1}$$những tập hợp con như vậy, lớn về mặt thiên văn ngay cả đối với các đồ thị có vài chục cạnh. 

Quan sát chính xuất phát từ việc sắp xếp tất cả các cạnh theo trọng số. 

Cho phép$r=n-1$. Hãy xem xét điều đầu tiên$r$các cạnh theo thứ tự trọng lượng không giảm. Gọi bộ này$P$. 

Mỗi bộ$r$các cạnh có trọng lượng ít nhất bằng trọng lượng của$P$, bởi vì$P$chứa nhẹ nhất toàn cầu$r$các cạnh. 

Nếu như$P$đã bị ngắt kết nối, thì nó sẽ tối ưu ngay lập tức và chúng ta đã hoàn thành. 

Trường hợp thú vị là khi$P$tạo thành một cái cây. 

Giả sử chúng ta thêm một cạnh$e$ngoài$P$. Từ$P$là một cái cây, thêm$e$tạo ra đúng một chu kỳ. Nếu chúng ta loại bỏ một cạnh khỏi chu trình đó, đồ thị vẫn được kết nối và chúng ta vẫn có một cây. Điều đó không giúp ích gì. 

Để thu được đồ thị rời rạc, chúng ta phải loại bỏ một cạnh **không** trên chu trình được tạo bởi$e$. 

Đối với cạnh ngoài cố định$e$, bộ ngắt kết nối rẻ nhất có thể đạt được bằng một lần hoán đổi bằng cách loại bỏ cạnh nặng nhất của$P$không nằm trên đường đi giữa các điểm cuối của$e$. 

Sự thật đáng ngạc nhiên là mọi câu trả lời tối ưu đều có thể được biểu diễn bằng một phép hoán đổi duy nhất như vậy. Bất kỳ giải pháp nào khác với$P$bởi một số bổ sung và loại bỏ có trọng số tăng ít nhất bằng mức hoán đổi đơn hợp lệ tốt nhất. 

Điều này làm giảm toàn bộ vấn đề trong việc tìm kiếm, đối với mọi cạnh bên ngoài tiền tố, cạnh có trọng số tối đa của cây tiền tố không nằm trên đường dẫn cây tương ứng. 

Điều đó trở thành vấn đề về cấu trúc dữ liệu cây. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu |$O(m \log^2 n)$|$O(n \log n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Xây dựng tiền tố quan trọng 

1. Sắp xếp tất cả các cạnh theo trọng lượng. 
2. Hãy để$r=n-1$. Lấy cái đầu tiên$r$các cạnh và gọi tập hợp này$P$. 
3. Tính tổng trọng lượng của chúng, ký hiệu là$S$. 
4. Sử dụng DSU trên các cạnh của$P$. 
5. Nếu$P$bị ngắt kết nối, đầu ra$S$. Không có bộ nào khác$r$các cạnh có thể có trọng lượng nhỏ hơn. 
6. Nếu$m=r$, sau đó$P$là tập hợp duy nhất có thể có của$r$các cạnh. Vì nó là một cái cây nên không có câu trả lời hợp lệ nào tồn tại, vì vậy kết quả đầu ra$-1$. 

### Khi tiền tố là cây 

1. Điều trị$P$như một cái cây. 
2. Root cây và xây dựng bảng nâng nhị phân. 
3. Đối với mỗi lần nhảy tổ tiên, hãy lưu trữ hai giá trị: 

- trọng lượng cạnh tối đa trong lần nhảy đó, 
- trọng lượng cạnh tối đa thứ hai trong lần nhảy đó. 
4. Cho mọi cạnh$e=(u,v,w)$ngoài$P$, truy vấn đường dẫn từ$u$ĐẾN$v$bên trong cây. 
5. Hãy để$M$là trọng lượng cạnh tối đa trên ent
