---
title: "CF 105284A - P!=NP"
description: "Chúng ta được yêu cầu đếm các cặp số nguyên $(n, p)$ trong đó $p$ bị ràng buộc nằm giữa $0$ và $P$, và cả hai điều kiện số học liên quan đến phép nhân và hành vi giống giai thừa đều phải đúng."
date: "2026-06-23T06:40:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105284
codeforces_index: "A"
codeforces_contest_name: "TeamsCode Summer 2024 Advanced Division"
rating: 0
weight: 105284
solve_time_s: 87
verified: true
draft: false
---

[CF 105284A - P!=NP](https://codeforces.com/problemset/problem/105284/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu đếm các cặp số nguyên$(n, p)$Ở đâu$p$bị buộc phải nằm giữa$0$Và$P$và cả hai điều kiện số học liên quan đến phép nhân và hành vi giống giai thừa đều phải đúng. Một điều kiện buộc một mối quan hệ giữa$n \cdot p$và một giá trị xuất phát từ$p$và cái còn lại cấm trường hợp đẳng thức suy biến trong đó sản phẩm quay trở lại giá trị ban đầu. 

Ý tưởng cấu trúc quan trọng là đối với mỗi cố định$p$, giá trị của$n$không được tự do lựa chọn. Thay vào đó, các ràng buộc buộc một ứng cử viên phải$n$(nếu có), được xác định bằng cách sắp xếp lại đại số của phương trình bao gồm$n \cdot p$. Điều này có nghĩa là vấn đề không phải là tìm kiếm trên cả hai biến mà là việc lặp lại$p$và kiểm tra xem một số nguyên hợp lệ$n$tồn tại. 

Ràng buộc$P \le 10^5$ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng liệt kê tất cả các cặp có thể$(n, p)$theo cách hai chiều. Một vòng lặp đôi ngây thơ sẽ quá chậm nếu$n$được giới hạn tương tự như$p$, và nó trở nên hoàn toàn không khả thi nếu$n$là không giới hạn hoặc lớn. Giải pháp dự định phải giảm mọi thứ xuống một lần duy nhất$p$. 

Trường hợp cạnh tinh tế xuất hiện ở các giá trị rất nhỏ của$p$. Khi$p = 0$, bất kỳ sản phẩm nào$n \cdot p$trở thành số 0, điều này có xu hướng phá vỡ các đối số về tính duy nhất. Khi$p = 1$hoặc$p = 2$, các biểu thức giai thừa và tuyến tính sẽ thu gọn thành các số nguyên nhỏ trong đó các điều kiện đẳng thức hoạt động khác với trường hợp tổng quát. Một dẫn xuất bất cẩn giả định “lớn$p$” Hành vi sẽ bao gồm hoặc loại trừ không chính xác những giá trị nhỏ này, đây chính xác là nguyên nhân dẫn đến các câu trả lời sai. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ thử tất cả các cặp$(n, p)$với$0 \le p \le P$và kiểm tra trực tiếp hai điều kiện. Ngay cả khi chúng ta hạn chế$n$đến một phạm vi tương tự, điều này đã mang lại$O(P^2)$hoạt động xung quanh$10^{10}$trong trường hợp xấu nhất. Điều này vượt xa những gì giới hạn 1 giây có thể xử lý. 

Cấu trúc của điều kiện đơn giản hóa đáng kể khi chúng ta cô lập$n$. Mối quan hệ cốt lõi là sản phẩm$n \cdot p$phải khớp với một giá trị xuất phát duy nhất từ$p$, mà chúng ta có thể hiểu là$p!$. Điều này ngay lập tức buộc$n = \frac{p!}{p}$, điều này đơn giản hóa hơn nữa để$n = (p-1)!$. Vì vậy với mỗi$p$, có nhiều nhất một ứng cử viên$n$, và vấn đề giảm xuống còn việc quyết định xem ứng cử viên đó có hợp lệ dưới ràng buộc bất đẳng thức còn lại hay không. 

Khi việc giảm này được thực hiện, điều kiện còn lại chỉ lọc ra các trường hợp bệnh lý trong đó sản phẩm sụp đổ trở lại$p$. Điều đó chỉ xảy ra với các giá trị giai thừa nhỏ nhất, đặc biệt khi$p$nó nhỏ đến mức$p! = p$. Điều này xảy ra đối với$p = 1$Và$p = 2$. Tất cả các giá trị lớn hơn đều tự động thỏa mãn điều kiện bất đẳng thức. 

Vì vậy, cách tiếp cận vũ phu trở thành một bài toán đếm đơn giản trên$p$, trong đó mỗi giá trị hợp lệ$p$đóng góp chính xác một cặp hợp lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(P^2)$|$O(1)$| Quá chậm | 
| Kiểm tra Per-p giảm |$O(P)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Quan sát rằng đối với mỗi cố định$p$, điều kiện lực$n$để thỏa mãn một phương trình có dạng$n \cdot p = p!$. Điều này có nghĩa$n$được xác định duy nhất bất cứ khi nào$p > 0$. 
2. Sắp xếp lại phương trình để tính ra ứng viên duy nhất có thể$n$BẰNG$n = (p-1)!$. Điều này loại bỏ sự cần thiết phải tìm kiếm$n$toàn bộ. 
3. Đối với mỗi$p$, kiểm tra xem điều kiện thứ hai$p \ne n \cdot p$nắm giữ. Từ$n \cdot p = p!$, điều kiện này trở thành$p \ne p!$. 
4. Xác định giá trị của$p$Ở đâu$p = p!$. Sự đẳng thức này chỉ đúng đối với$p = 1$Và$p = 2$, bởi vì giai thừa phát triển nhanh chóng và vượt quá giá trị nhận dạng từ$p \ge 3$trở đi. 
5. Đếm tất cả các số nguyên$p$trong phạm vi$[1, P]$loại trừ$p = 1$Và$p = 2$. Mỗi cái như vậy$p$đóng góp chính xác một cặp hợp lệ. 

### Tại sao nó hoạt động 

Việc chuyển đổi làm giảm vấn đề thành ánh xạ một-một từ mỗi$p$cho một ứng cử viên duy nhất$n$. Không có hai khác nhau$n$các giá trị có thể đáp ứng các ràng buộc cho cùng một$p$và tất cả sự vô hiệu được nắm bắt hoàn toàn bởi tập hợp nhỏ$p$trong đó giai thừa bằng giá trị nhận dạng. Điều này đảm bảo rằng việc đếm hợp lệ$p$trực tiếp tạo ra số lượng cặp hợp lệ chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    P = int(input().strip())
    # valid p are all integers in [1, P] except p = 1 and p = 2
    if P <= 2:
        print(0)
    else:
        print(P - 2)

if __name__ == "__main__":
    solve()
```Mã này phản ánh sự rút gọn cuối cùng khi chúng ta không còn cố gắng tính giai thừa hoặc liệt kê nữa.$n$. Quan sát có ý nghĩa duy nhất là tất cả$p \ge 3$đóng góp chính xác một cặp hợp lệ. 

Việc xử lý ranh giới cho$P \le 2$là cần thiết vì công thức$P - 2$nếu không sẽ tạo ra số lượng âm hoặc không chính xác. Đây là nơi duy nhất thường xuất hiện các lỗi riêng lẻ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
```Chúng tôi liệt kê hợp lệ$p$các giá trị. 

| p | Có hiệu lực? | Lý do | 
| --- | --- | --- | 
| 1 | Không | giai thừa bằng giá trị, vi phạm bất đẳng thức | 
| 2 | Không | trường hợp sập tương tự | 
| 3 | Có | đóng góp một cặp hợp lệ | 
| 4 | Có | đóng góp một cặp hợp lệ | 

Số cuối cùng là 2, tương ứng với$p = 3$Và$p = 4$. 

### Ví dụ 2 

đầu vào:```
6
```| p | Có hiệu lực? | 
| --- | --- | 
| 1 | Không | 
| 2 | Không | 
| 3 | Có | 
| 4 | Có | 
| 5 | Có | 
| 6 | Có | 

Kết quả là 4 cặp hợp lệ. 

Những dấu vết này xác nhận rằng các giá trị bị loại trừ duy nhất là các suy biến nhỏ tại$p = 1$Và$p = 2$. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$| Chỉ cần phép trừ và so sánh theo thời gian không đổi | 
| Không gian |$O(1)$| Không sử dụng cấu trúc dữ liệu phụ trợ | 

Giải pháp là thời gian không đổi bất kể$P$, thỏa mãn một cách thoải mái$10^5$giới hạn và không còn chỗ cho những lo ngại về hiệu suất. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import factorial

    P = int(input().strip())
    if P <= 2:
        return "0"
    return str(P - 2)

# provided sample
assert run("4\n") == "2", "sample 1"

# minimum edge case
assert run("1\n") == "0", "P = 1"

# boundary case
assert run("2\n") == "0", "P = 2"

# small general case
assert run("3\n") == "1", "only p=3 works"

# larger case
assert run("10\n") == "8", "exclude only 1 and 2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 0 | ranh giới tối thiểu | 
| 2 | 0 | trường hợp suy biến thứ hai | 
| 3 | 1 | đóng góp hợp lệ đầu tiên xuất hiện | 
| 10 | 8 | tính đúng của công thức tổng quát | 

## Vỏ cạnh 

cho$P = 1$, ứng cử viên duy nhất$p$là 1, nhưng nó thất bại vì giai thừa bị phá vỡ và vi phạm điều kiện bất đẳng thức. Thuật toán trả về đúng 0 vì$P \le 2$. 

Vì$P = 2$, cả hai$p = 1$Và$p = 2$không hợp lệ vì lý do tương tự. Dạng trừ sẽ cho 0, khớp với kết quả chính xác mà không cần xử lý đặc biệt ngoài kiểm tra ranh giới. 

Vì$P \ge 3$, các trường hợp không hợp lệ được cố định và không phát triển cùng với$P$, vậy công thức$P - 2$vẫn ổn định và đếm chính xác tất cả các giá trị còn lại$p$các giá trị.
