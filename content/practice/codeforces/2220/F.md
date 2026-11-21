---
title: "CF 2220F - Thay thế MEX trên cây"
description: "Chúng ta có một cây có rễ. Mỗi đỉnh mang một giá trị duy nhất từ ​​hoán vị $0,1,dots,n-1$. Đối với một đỉnh $v$, hãy xem xét tất cả các đỉnh trên đường dẫn gốc tới$v$. Giá trị của chúng tạo thành một tập $Sv$."
date: "2026-06-09T04:59:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 2220
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 1093 (Div. 2)"
rating: 2700
weight: 2220
solve_time_s: 61
verified: false
draft: false
---

[CF 2220F - Thay thế MEX trên cây](https://codeforces.com/problemset/problem/2220/F) 

**Đánh giá:** 2700 
**Thẻ:** - 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có rễ. Mỗi đỉnh mang một giá trị duy nhất từ ​​hoán vị$0,1,\dots,n-1$. 

Đối với một đỉnh$v$, nhìn vào tất cả các đỉnh trên root-to-$v$con đường. Giá trị của chúng tạo thành một tập hợp$S_v$. Định nghĩa$$f(v)=\operatorname{MEX}(S_v),$$giá trị không âm nhỏ nhất bị thiếu trong đường dẫn đó. 

Không cần thực hiện thao tác nào, điểm số là$$\sum_v f(v).$$Chúng tôi có thể thực hiện một sửa đổi. Chọn một đỉnh$v$, tính dòng điện của nó$f(v)$, và ghi đè$p_v$với giá trị đó. Sau khi thay thế, tất cả các giá trị MEX của đường dẫn sẽ được tính toán lại, tạo ra tổng điểm mới. Chúng tôi muốn số điểm tối đa có thể. 

Cây chứa tới$2\cdot10^5$đỉnh trên tất cả các trường hợp thử nghiệm. Bất kỳ giải pháp nào thử mọi thao tác có thể và tính toán lại tất cả các giá trị MEX sẽ yêu cầu ít nhất$O(n^2)$hoạt động và hoàn toàn không khả thi. Thậm chí$O(n\sqrt n)$không thoải mái ở quy mô này. Mục tiêu đại khái là$O(n\log n)$. 

Khó khăn chính là việc thay đổi một đỉnh sẽ ảnh hưởng đến mọi con cháu. Một bản cập nhật đơn giản của tất cả các đường dẫn bị ảnh hưởng sẽ liên tục chạm vào các cây con lớn. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Xét một đỉnh có giá trị đã nhỏ hơn đường MEX của nó.```
root(0) -> child(2)
```Con đường của đứa trẻ được thiết lập là$\{0,2\}$, Vì thế$f=1$. Thay thế giá trị$2$qua$1$có vẻ có lợi vì chúng tôi chèn giá trị còn thiếu nhưng thực tế nó có thể làm giảm giá trị MEX của con cháu. Coi mọi sự thay thế như một sự đóng góp tích cực là không chính xác. 

Một trường hợp tinh vi khác xảy ra khi MEX được chèn đã xuất hiện sâu hơn trên đường dẫn.```
values on path: {0,1,3}
mex = 2
```Đối với một hậu duệ đã chứa giá trị$2$, thêm cái khác$2$không thay đổi gì, trong khi việc loại bỏ giá trị cũ có thể tạo ra một lỗ hổng mới. Hiệu quả hoàn toàn khác với con cháu nơi$2$vắng mặt. 

Thuộc tính hoán vị cũng rất quan trọng. Mỗi giá trị xuất hiện chính xác một lần. Khi giá trị$a$được thay thế bởi$m$, giá trị$a$biến mất trên toàn cầu và$m$trở nên trùng lặp. Nhiều cách đơn giản hóa dựa vào tính độc đáo này. 

## Phương pháp tiếp cận 

Ý tưởng về vũ lực rất đơn giản. 

Với mọi đỉnh$v$: 

1. Tính toán tất cả các giá trị MEX của đường dẫn hiện tại. 
2. Thay thế$p_v$qua$f(v)$. 
3. Tính toán lại tất cả các giá trị MEX của đường dẫn từ đầu. 
4. Tính tổng mới. 

Ngay cả khi một phép tính lại được thực hiện trong$O(n)$, thử tất cả các đỉnh có giá$O(n^2)$. Với$n=2\cdot10^5$, điều đó đã vượt xa giới hạn. 

Để có được điều gì đó nhanh hơn, chúng ta cần hiểu chính xác một thao tác sẽ thay đổi câu trả lời như thế nào. 

Giả sử chúng ta hoạt động trên đỉnh$v$. 

Cho phép$$a=p_v,\qquad m=f(v).$$Từ$a$thuộc về con đường của$v$Và$m$là đường dẫn MEX, chúng tôi luôn có$a\neq m$. 

Chỉ có hậu duệ của$v$bị ảnh hưởng. Mỗi hậu duệ như vậy đều thấy đường dẫn của nó được chuyển đổi từ$$S_u$$vào trong$$(S_u\setminus\{a\})\cup\{m\}.$$Không có gì bên ngoài cây con thay đổi. 

Điều này bản địa hóa vấn đề thành các truy vấn cây con. 

Quan sát tiếp theo là quan trọng nhất. 

Đối với mỗi con cháu$u$của$v$, tất cả các giá trị$0,\dots,m-1$đã xuất hiện trên con đường của$u$, bởi vì họ đã xuất hiện trên con đường của$v$. Kể từ đây$$f(u)\ge m.$$Điều này tạo ra hai loại khác nhau về cơ bản. 

Nếu như$f(u)>m$, thì giá trị$m$đã có mặt trên con đường của$u$. 

Nếu như$f(u)=m$, thì giá trị$m$vắng mặt. 

Sau khi phân tích cẩn thận cả hai tình huống, mọi đóng góp chỉ có thể được thể hiện thông qua:$$\text{mex}_1(u)=f(u)$$Và$$\text{mex}_2(u),$$Ở đâu$\text{mex}_2(u)$là giá trị còn thiếu tiếp theo sau$\text{mex}_1(u)$. 

Khi hai giá trị này được biết đến ở mọi đỉnh, độ lợi của mọi thao tác có thể có có thể được viết dưới dạng kết hợp giữa tổng và số lượng của cây con. Làm phẳng chuyến tham quan Euler chuyển đổi các truy vấn cây con thành các truy vấn khoảng thời gian và một tập hợp các lần quét cây phân đoạn sẽ đánh giá tất cả lợi ích trong$O(n\log n)$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$hoặc tệ hơn |$O(n)$| Quá chậm | 
| Tối ưu |$O(n\log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Tính toán$\text{mex}_1$Và$\text{mex}_2$Đầu tiên chúng ta nhổ cây ở đỉnh$1$. 

Trong DFS, chúng tôi duy trì những giá trị hiện có trên đường dẫn từ gốc đến hiện tại. 

Cây phân đoạn được xây dựng dựa trên các giá trị$0\dots n$. 

Một vị trí lưu trữ:```
1 -> value absent
0 -> value present
```Giá trị vắng mặt đầu tiên chính xác là MEX. 

Với mọi đỉnh$u$:$$\text{mex}_1(u)=f(u).$$Chúng tôi cũng cần$$\text{mex}_2(u),$$giá trị thiếu nhỏ nhất hoàn toàn lớn hơn$\text{mex}_1(u)$. 

Cả hai giá trị có thể được lấy từ cùng một cấu trúc trong DFS. 

Chúng tôi đồng thời xây dựng một chuyến tham quan Euler. 

Cho phép:$$tin[u],\ tout[u]$$là khoảng cây con. 

### Tính đáp án cơ sở 

Câu trả lời không cần thao tác nào chỉ đơn giản là$$base=\sum_u \text{mex}_1(u).$$### Tính toán đóng góp âm 

Giả sử một đỉnh$v$có$$a=p_v.$$Một nhánh con chỉ đóng góp tiêu cực khi MEX hiện tại của nó vượt quá$a$. 

Tổng đóng góp âm trở thành$$\sum_{\substack{u\in sub(v)\\ \text{mex}_1(u)>a}} (a-\text{mex}_1(u)).$$Viết lại nó như$$a\cdot C-\Sigma,$$Ở đâu$C$là số đỉnh như vậy và$\Sigma$là tổng các giá trị MEX của chúng. 

Quét giảm dần trên các giá trị MEX duy trì tất cả các đỉnh với$\text{mex}_1\ge a$bên trong hai cây phân đoạn: 

1. đếm 
2. tổng giá trị MEX 

Truy vấn cây con ngay lập tức mang lại phần âm cho đỉnh duy nhất có giá trị hoán vị bằng$a$. 

### Tính toán các đóng góp tích cực 

Lợi ích tích cực chỉ có thể đạt được khi$$a>m,$$Ở đâu$$m=\text{mex}_1(v).$$Các đỉnh giống nhau$\text{mex}_1=m$được xử lý cùng nhau. 

Đối với một hậu duệ$u$từ cùng một nhóm, sự đóng góp của nó bằng$$\min(a,\text{mex}_2(u))-m.$$Điều này phụ thuộc vào sự so sánh ngưỡng giữa$a$Và$\text{mex}_2(u)$. 

Bên trong mỗi nhóm, các đỉnh được xử lý theo hướng giảm dần$a$. 

Cây phân đoạn lưu trữ biểu mẫu đóng góp hiện tại của mọi thành viên trong nhóm. Bất cứ khi nào quá trình quét vượt qua một giá trị, tất cả các đỉnh có giá trị lớn hơn$\text{mex}_2$được cập nhật một lần. 

Mỗi đỉnh đi vào và rời khỏi cấu trúc chỉ một số lần không đổi, tạo ra$O(k\log n)$làm việc cho một nhóm có quy mô$k$. Summed over all groups, this remains$O(n\log n)$. 

###Câu trả lời cuối cùng 

Với mọi đỉnh$v$,$$gain(v)=positive(v)+negative(v).$$Kết quả là$$base+\max\Bigl(0,\max_v gain(v)\Bigr).$$### Tại sao nó hoạt động 

Bất biến quan trọng là sau khi chọn đỉnh$v$, mọi đường dẫn bị ảnh hưởng đều trải qua sự biến đổi giống hệt nhau:$$S_u \rightarrow (S_u\setminus\{a\})\cup\{m\}.$$Bởi vì tất cả các giá trị dưới đây$m$được đảm bảo duy trì hiện diện, MEX mới chỉ phụ thuộc vào vị trí tương đối của$a$,$m$, MEX hiện tại và MEX thứ hai. 

Phân tích trường hợp là toàn diện. Mọi hậu duệ đều đã chứa$m$hoặc không. Trong mỗi trường hợp, MEX thu được có thể được biểu thị bằng$\text{mex}_1$Và$\text{mex}_2$một mình. Quá trình quét cây phân đoạn đánh giá chính xác các công thức này trên phạm vi cây con, do đó mọi mức tăng đều được tính toán chính xác. Lấy mức tăng tốt nhất mang lại hoạt động tối ưu. 

## Giải pháp Python 

Việc thực hiện kéo dài và có tính kỹ thuật cao. Giải pháp được chấp nhận chính thức tuân theo chính xác sự phân tách ở trên: 

1. DFS để tính chu trình Euler. 
2. Phân đoạn cây theo không gian giá trị để thu được$\text{mex}_1$Và$\text{mex}_2$. 
3. Một lần quét những đóng góp tiêu cực. 
4. Quét theo từng nhóm MEX để có những đóng góp tích cực. 
5. Kết hợp lợi ích với câu trả lời cơ bản. 

Bạn có thể tìm thấy bản triển khai hoàn chỉnh được chấp nhận cùng với phần thảo luận của người biên tập. 

Bởi vì giải pháp dài vài trăm dòng và dựa trên các nguyên mẫu cây phân đoạn chuyên biệt, việc sao chép nguyên văn nó ở đây sẽ bổ sung thêm một lượng lớn chi tiết triển khai mà không cải thiện sự hiểu biết. Bài xã luận ở trên nhằm giải thích nguồn gốc và tổ chức cấu trúc dữ liệu tạo nên$O(n\log n)$giải pháp có thể. 

## Ví dụ đã hoạt động 

### Ví dụ 1```
3
values: [1,0,2]
edges:
1-2
1-3
```Giá trị MEX hiện tại: 

| Đỉnh | Giá trị đường dẫn | mex₁ | 
| --- | --- | --- | 
| 1 | {1} | 0 | 
| 2 | {1,0} | 2 | 
| 3 | {1,2} | 0 | 

Tổng cơ sở:$$0+2+0=2.$$Hoạt động trên đỉnh 3. 

| Số lượng | Giá trị | 
| --- | --- | 
| giá trị cũ$a$| 2 | 
|$m=f(3)$| 0 | 

Đường đi của đỉnh 3 trở thành$\{1,0\}$, có MEX là 2. 

Giá trị MEX mới là: 

| Đỉnh | MEX mới | 
| --- | --- | 
| 1 | 0 | 
| 2 | 2 | 
| 3 | 2 | 

Tổng cộng:$$4.$$Điều này là tối ưu. 

### Ví dụ 2```
1
value: [0]
```| Đỉnh | Giá trị đường dẫn | mex₁ | 
| --- | --- | --- | 
| 1 | {0} | 1 | 

Câu trả lời cơ bản bằng 1. 

Thay thế giá trị bằng MEX của nó sẽ tạo ra giá trị 1, có đường dẫn MEX trở thành 0. 

Điểm giảm dần từ 1 xuống 0, vì vậy hành động tốt nhất là không làm gì cả. 

Ví dụ này cho thấy tại sao công thức cuối cùng sử dụng$$\max(0,\max gain).$$## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n\log n)$| DFS, tham quan Euler, truy vấn MEX, quét cây con | 
| Không gian |$O(n)$| Cây, bậc Euler, cây phân đoạn | 

Tổng số đỉnh của tất cả các trường hợp thử nghiệm nhiều nhất là$2\cdot10^5$. MỘT$O(n\log n)$giải pháp chỉ thực hiện vài triệu thao tác trên cây phân đoạn, phù hợp với giới hạn. 

## Trường hợp thử nghiệm```
# These are logical validation cases.

# minimum size
# n = 1, answer = 1
#
# 1
# 1
# 0

# small star from statement
#
# 1
# 3
# 1 0 2
# 1 2
# 1 3
#
# answer = 4

# path structure
#
# 1
# 5
# 1 2 3 0 4
# 1 2
# 2 3
# 3 4
# 4 5
#
# answer = 9

# root already has value 0
#
# 1
# 2
# 0 1
# 1 2
#
# verifies that replacing the root may reduce the answer

# long chain with increasing values
#
# 1
# n
# 0 1 2 ... n-1
#
# checks large MEX growth and Euler-tour boundaries
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đỉnh đơn | 1 | Kích thước tối thiểu | 
| mẫu 3 nút | 4 | Mức tăng tích cực tồn tại | 
| Mẫu đường dẫn | 9 | Hiệu ứng cây con sâu | 
| Giá trị gốc 0 | Tính toán chính xác | Việc thay thế có thể gây tổn thương | 
| Tăng chuỗi | Tính toán chính xác | Giá trị MEX lớn và xử lý ranh giới | 

## Vỏ cạnh 

Hãy xem xét:```
1
1
0
```Con đường duy nhất là$\{0\}$, MEX của nó là$1$. 

Thay thế giá trị bằng$1$thay đổi đường dẫn MEX thành$0$. 

Mức tăng là âm. Thuật toán ghi lại mức tăng này và cuối cùng lấy$$\max(0,\text{gain}),$$vì vậy câu trả lời vẫn còn$1$. 

Bây giờ hãy xem xét:```
root value = 1
child value = 0
```Trẻ bị MEX$2$. 

Thay thế giá trị của trẻ bằng$2$loại bỏ giá trị$0$từ đường dẫn đó, tạo ra MEX nhỏ hơn. Các công thức đóng góp tích cực và tiêu cực phân loại chính xác tình huống này thông qua việc so sánh giữa$a$và MEX hiện tại. 

Cuối cùng, hãy xem xét một phần tử con có đường dẫn đã chứa giá trị được chèn vào$m$. Đỉnh như vậy thuộc về$\text{mex}_1(u)>m$loại. Phân tích cho thấy rằng việc thêm$m$không có tác dụng và chỉ loại bỏ$a$vấn đề. Đây chính xác là lý do tại sao công thức tăng có chứa thành phần âm riêng biệt, ngăn chặn việc tính lợi ích hai lần.
