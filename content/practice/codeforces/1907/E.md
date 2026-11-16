---
title: "CF 1907E - Bộ ba tốt"
description: "Chúng ta cần đếm bộ ba số nguyên không âm $(a,b,c)$ có tổng chính xác là $n$. Phần bất thường là điều kiện tổng chữ số: $$text{digsum}(a)+text{digsum}(b)+text{digsum}(c)=text{digsum}(n).$$ Bộ ba chỉ được tính nếu cả hai điều kiện đều đúng."
date: "2026-06-08T20:38:52+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "combinatorics", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1907
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 913 (Div. 3)"
rating: 1600
weight: 1907
solve_time_s: 115
verified: true
draft: false
---

[CF 1907E - Bộ ba tốt](https://codeforces.com/problemset/problem/1907/E) 

**Đánh giá:** 1600 
**Tags:** lực lượng vũ phu, tổ hợp, lý thuyết số 
**Thời gian giải:** 1 phút 55s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta cần đếm bộ ba số nguyên không âm$(a,b,c)$tổng của nó chính xác là$n$. Phần bất thường là điều kiện tổng chữ số:$$\text{digsum}(a)+\text{digsum}(b)+\text{digsum}(c)=\text{digsum}(n).$$Bộ ba chỉ được tính nếu cả hai điều kiện đều được giữ nguyên. Vì trật tự rất quan trọng,$(1,2,3)$Và$(3,2,1)$là những bộ ba khác nhau. 

Giá trị của$n$có thể lớn như$10^7$, và có thể có$10^4$trường hợp thử nghiệm. Bất kỳ thuật toán nào lặp lại các giá trị có thể có của$a$,$b$, Và$c$ngay lập tức là không thể. Thậm chí lặp lại trên tất cả các cặp$(a,b)$sẽ yêu cầu khoảng$10^{14}$hoạt động trong trường hợp xấu nhất. 

Ràng buộc chính không phải là kích thước của$n$, nhưng thực tế là$n$có nhiều nhất 8 chữ số thập phân. Điều này gợi ý rõ ràng rằng câu trả lời phụ thuộc vào từng chữ số biểu diễn thập phân thay vì vào chính giá trị số. 

Có một số trường hợp khó khăn có thể dễ dàng đánh lừa một giải pháp ngây thơ. 

Vì$n=0$, bộ ba hợp lệ duy nhất là$(0,0,0)$. Câu trả lời là$1$. Một công thức giả định mỗi chữ số đóng góp nhiều khả năng sẽ trả về giá trị lớn hơn một cách không chính xác. 

Vì$n=10$, các bộ ba hợp lệ không phải là tất cả nghiệm của$a+b+c=10$. Ví dụ,$(5,5,0)$có tổng chữ số$5+5+0=10$, trong khi$\text{digsum}(10)=1$, vì vậy nó không hợp lệ. Thực hiện hoạt động quan trọng. 

Vì$n=10000000$, hầu hết các chữ số đều bằng 0. Một chữ số DP bất cẩn có thể vô tình cho phép mang giữa các vị trí và đếm nhiều cấu hình bổ sung. Câu trả lời đúng chỉ có$3$, tương ứng với việc đặt toàn bộ giá trị vào đúng một trong ba số. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ liệt kê tất cả các bộ ba$(a,b,c)$với$a+b+c=n$, sau đó kiểm tra điều kiện tổng chữ số. Từ$c$được xác định bởi$a$Và$b$, điều này đòi hỏi khoảng$$\sum_{a=0}^{n}(n-a+1)=O(n^2)$$tiểu bang. 

Vì$n=10^7$, đó là khoảng$10^{14}$lặp đi lặp lại, hoàn toàn không khả thi. 

Quan sát quan trọng xuất phát từ tính chất tiêu chuẩn của phép cộng thập phân. Bất cứ khi nào xảy ra hiện tượng nhớ, tổng các tổng chữ số sẽ giảm theo bội số của$9$. 

Với mọi số nguyên không âm,$$\text{digsum}(x)+\text{digsum}(y)-\text{digsum}(x+y)$$bằng$9$lần số lượng hạt mang được tạo ra trong quá trình cộng. 

Áp dụng điều này nhiều lần,$$\text{digsum}(a)+\text{digsum}(b)+\text{digsum}(c)-\text{digsum}(n)$$là$9$nhân với tổng số lần mang xảy ra khi cộng ba số đó. 

Điều kiện của chúng tôi yêu cầu sự khác biệt này phải chính xác bằng không. Vì việc mang chỉ có thể làm cho phía bên trái lớn hơn nên khả năng duy nhất là **không xảy ra việc mang ở bất cứ đâu**. 

Điều đó hoàn toàn thay đổi vấn đề. Thay vì nghĩ về những con số, chúng ta nghĩ về những con số. 

Giả sử một chữ số của$n$là$d$. Vì không được phép mang theo nên các chữ số tương ứng$x,y,z$của$a,b,c$phải thỏa mãn$$x+y+z=d.$$Các vị trí chữ số khác nhau trở nên độc lập. Số bộ ba hợp lệ chỉ đơn giản là tích của số bộ ba chữ số ứng với mỗi chữ số của$n$. 

Đối với một chữ số cố định$d$, chúng ta cần số nghiệm không âm của$$x+y+z=d.$$Từ$d\le 9$, giới hạn chữ số$x,y,z\le 9$được tự động thỏa mãn. 

Bởi các ngôi sao và thanh,$$\#(x+y+z=d)=\binom{d+2}{2}.$$Mỗi chữ số đóng góp độc lập nên câu trả lời cuối cùng là$$\prod_{\text{digits } d \text{ of } n} \binom{d+2}{2}.$$| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(\text{digits}(n))$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước các giá trị$$f[d]=\binom{d+2}{2}$$cho chữ số$d=0$bởi vì$9$. 
2. Với mỗi test case, hãy chuyển đổi$n$sang biểu diễn thập phân của nó. 
3. Khởi tạo câu trả lời dưới dạng$1$. 
4. Xử lý từng chữ số$d$của$n$. 
5. Nhân câu trả lời với$f[d]$. 

Điều này đếm tất cả các chữ số ba$(x,y,z)$tổng của nó bằng$d$ở vị trí đó. 
6. Sau khi tất cả các chữ số được xử lý, xuất kết quả. 

### Tại sao nó hoạt động 

Điều kiện tổng các chữ số tương đương với việc nói rằng không có số nhớ nào xảy ra trong quá trình cộng$a+b+c=n$. Nếu thậm chí một lần xuất hiện, số lượng$$\text{digsum}(a)+\text{digsum}(b)+\text{digsum}(c)$$trở nên lớn hơn nghiêm ngặt so với$\text{digsum}(n)$bằng bội số của$9$. 

Khi không có số mang, mỗi vị trí thập phân là độc lập. Ở vị trí chữ số nơi$n$chứa chữ số$d$, các chữ số được chọn cho$a$,$b$, Và$c$chỉ cần thỏa mãn$x+y+z=d$. Số bộ ba như vậy là$\binom{d+2}{2}$. Vì các lựa chọn ở các vị trí khác nhau không tương tác với nhau nên nguyên tắc nhân cho kết quả trên tất cả các chữ số. Do đó, mỗi bộ ba hợp lệ sẽ được tính chính xác một lần và mọi cấu hình được tính sẽ tạo ra một bộ ba hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

# f[d] = C(d + 2, 2)
ways = [(d + 2) * (d + 1) // 2 for d in range(10)]

t = int(input())

for _ in range(t):
    n = input().strip()

    ans = 1
    for ch in n:
        ans *= ways[ord(ch) - ord('0')]

    print(ans)
```Mảng`ways`lưu trữ số nghiệm không âm của$x+y+z=d$cho mỗi chữ số$d$. Từ$d\le 9$, công thức sao và thanh được áp dụng trực tiếp. 

Đối với mỗi trường hợp thử nghiệm, chúng tôi đọc số dưới dạng chuỗi. Điều này tránh việc trích xuất nhiều lần các chữ số có phép chia và xử lý các giá trị lên đến$10^7$. 

Câu trả lời bắt đầu lúc$1$. Mỗi chữ số đóng góp một hệ số nhân độc lập nên chúng ta nhân với`ways[d]`cho mỗi chữ số. 

Các số nguyên Python tự động mở rộng đến kích thước tùy ý, điều này rất hữu ích vì các câu trả lời chẳng hạn như câu trả lời cho$9999999$vượt quá giới hạn 32-bit và 64-bit. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$n=11$Các chữ số là$1$Và$1$. 

Đối với chữ số$1$,$$\binom{1+2}{2}=3.$$| Chữ số | Đóng góp | Chạy Trả lời | 
| --- | --- | --- | 
| 1 | 3 | 3 | 
| 1 | 3 | 9 | 

Câu trả lời cuối cùng:$9$. 

Những điều này tương ứng chính xác với việc chọn, độc lập ở mỗi vị trí chữ số, một trong ba khả năng:$$(1,0,0),\ (0,1,0),\ (0,0,1).$$### Ví dụ 2:$n=3141$| Chữ số | Sự đóng góp$\binom{d+2}{2}$| Chạy Trả lời | 
| --- | --- | --- | 
| 3 | 10 | 10 | 
| 1 | 3 | 30 | 
| 4 | 15 | 450 | 
| 1 | 3 | 1350 | 

Câu trả lời cuối cùng:$1350$. 

Ví dụ này thể hiện tính độc lập của vị trí chữ số. Tổng số chỉ đơn giản là tích của số đếm cho mỗi chữ số. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\text{digits}(n))$| Một phép nhân cho mỗi chữ số thập phân | 
| Không gian |$O(1)$| Chỉ có một bảng tra cứu có kích thước cố định nhỏ | 

Từ$n \le 10^7$, mỗi test chứa tối đa 8 chữ số. Ngay cả với$10^4$trường hợp thử nghiệm, tổng công việc chỉ xoay quanh$8 \times 10^4$hoạt động chữ số, thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline
    ways = [(d + 2) * (d + 1) // 2 for d in range(10)]

    t = int(input())
    out = []

    for _ in range(t):
        n = input().strip()
        ans = 1
        for ch in n:
            ans *= ways[int(ch)]
        out.append(str(ans))

    return "\n".join(out)

# provided sample
assert run(
"""12
11
0
1
2
3
4
5
3141
999
2718
9999999
10000000
"""
) == (
"""9
1
3
6
10
15
21
1350
166375
29160
1522435234375
3"""
), "sample"

# minimum value
assert run(
"""1
0
"""
) == "1", "only (0,0,0)"

# single digit maximum
assert run(
"""1
9
"""
) == "55", "C(11,2)"

# power of ten
assert run(
"""1
1000
"""
) == "3", "only one nonzero digit"

# all digits equal to 9
assert run(
"""1
999
"""
) == "166375", "55^3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0`|`1`| Giá trị nhỏ nhất có thể | 
|`9`|`55`| Đóng góp một chữ số lớn nhất | 
|`1000`|`3`| Nhiều chữ số 0, xử lý mang theo | 
|`999`|`166375`| Câu trả lời nhân lớn | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
1
0
```Thuật toán xử lý một chữ số$0$. Đóng góp của nó là$$\binom{2}{2}=1.$$Câu trả lời cuối cùng là$1$, tương ứng với bộ ba duy nhất$(0,0,0)$. 

Hãy xem xét đầu vào:```
1
10
```Các chữ số là$1$Và$0$. Những đóng góp của họ là$3$Và$1$.$$3 \times 1 = 3.$$Bộ ba hợp lệ chính xác là$(10,0,0)$,$(0,10,0)$, Và$(0,0,10)$. Bất kỳ bộ ba nào như$(5,5,0)$tạo ra dấu ở vị trí đơn vị và vi phạm điều kiện tổng chữ số. Thuật toán tự động loại trừ nó vì nó chỉ tính các phép gán chữ số không mang. 

Hãy xem xét đầu vào:```
1
10000000
```Các đóng góp chữ số là$$3 \times 1 \times 1 \times 1 \times 1 \times 1 \times 1 \times 1 = 3.$$Chỉ một số có thể chứa số đứng đầu$1$, trong khi hai số còn lại phải bằng 0. Thuật toán trả về chính xác$3$. 

Hãy xem xét đầu vào:```
1
9999999
```Mỗi chữ số góp phần$$\binom{11}{2}=55.$$Câu trả lời trở thành$$55^7=1522435234375.$$Điều này kiểm tra xem việc triển khai có xử lý chính xác các câu trả lời rất lớn hay không và dựa vào các số nguyên có độ chính xác tùy ý của Python thay vì số học có chiều rộng cố định.
