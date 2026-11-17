---
title: "CF 1996D - Vui Vẻ"
description: "Chúng ta cần đếm bộ ba số nguyên dương $(a,b,c)$ thỏa mãn hai ràng buộc độc lập. Hạn chế đầu tiên giới hạn các tích theo cặp: $$ab + ac + bc le n$$ Hạn chế thứ hai giới hạn tổng: $$a + b + c le x$$ Từ \"ordered\" quan trọng."
date: "2026-06-08T14:43:23+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "combinatorics", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1996
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 962 (Div. 3)"
rating: 1500
weight: 1996
solve_time_s: 137
verified: true
draft: false
---

[CF 1996D - Vui vẻ](https://codeforces.com/problemset/problem/1996/D) 

**Đánh giá:** 1500 
**Tags:** tìm kiếm nhị phân, lực lượng vũ phu, tổ hợp, toán học, lý thuyết số 
**Thời gian giải:** 2m 17s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần đếm bộ ba số nguyên dương theo thứ tự$(a,b,c)$thỏa mãn hai ràng buộc độc lập. 

Hạn chế đầu tiên giới hạn các sản phẩm theo cặp:$$ab + ac + bc \le n$$Hạn chế thứ hai giới hạn tổng:$$a + b + c \le x$$Từ "ra lệnh" quan trọng. Nếu như$(1,1,2)$là hợp lệ thì$(1,2,1)$Và$(2,1,1)$được tính riêng. 

Đầu vào chứa nhiều trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm chúng tôi nhận được$n$Và$x$và phải xuất ra số bộ ba hợp lệ. 

Những hạn chế là đầu mối thực sự. Một bài kiểm tra duy nhất có thể có$n,x$lớn như$10^6$, nhưng tổng của tất cả$n$giá trị trên toàn bộ đầu vào nhiều nhất là$10^6$, và tổng của tất cả$x$giá trị cũng nhiều nhất$10^6$. 

Tìm kiếm bậc ba trên tất cả các giá trị có thể có của$a,b,c$là hoàn toàn không thể. Ngay cả một tìm kiếm bậc hai trên tất cả các giá trị lên đến$10^6$sẽ là quá lớn. Chúng ta cần khai thác cấu trúc của$$ab + ac + bc$$để hạn chế đáng kể không gian tìm kiếm. 

Một sai lầm dễ mắc phải là quên rằng các biến phải hoàn toàn dương. Ví dụ:```
n = 1, x = 10
```Không có bộ ba hợp lệ vì giá trị nhỏ nhất có thể có của$$ab+ac+bc$$với số nguyên dương là$1+1+1=3$. 

Một trường hợp tinh vi khác xuất hiện khi tổng giới hạn rất lớn và thực sự không liên quan:```
n = 7, x = 1000
```Câu trả lời không được xác định bởi$x$. Mọi bộ ba hợp lệ chỉ xuất phát từ bất đẳng thức tích. Một giải pháp lặp lại các giá trị lên tới$x$sẽ lãng phí rất nhiều thời gian. 

Lỗi phổ biến thứ ba là coi các hoán vị là giống hệt nhau. Vì```
n = 7, x = 4
```bộ ba hợp lệ là$$(1,1,1), (1,1,2), (1,2,1), (2,1,1)$$đưa ra câu trả lời$4$, không$2$. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là vũ lực. Liệt kê mọi điều tích cực$a$, mọi tích cực$b$, mọi tích cực$c$, kiểm tra cả hai bất đẳng thức và đếm các bộ ba hợp lệ. 

Điều này rõ ràng là đúng vì mỗi ứng viên đều được kiểm tra đúng một lần. 

Thật không may, ngay cả khi chúng ta chỉ xem xét các giá trị lên tới$10^6$, không gian tìm kiếm sẽ có thứ tự$$10^{18}$$gấp ba lần. Điều đó gần như không thể thực hiện được. 

Quan sát quan trọng là một lần$a$Và$b$được cố định, cả hai ràng buộc đều trở thành giới hạn trên đơn giản trên$c$. 

Từ$$ab + ac + bc \le n$$chúng tôi có được$$c(a+b) \le n-ab$$và do đó$$c \le \frac{n-ab}{a+b}.$$Từ$c$phải là số nguyên,$$c \le \left\lfloor \frac{n-ab}{a+b} \right\rfloor.$$Giới hạn tổng mang lại$$c \le x-a-b.$$Vì vậy đối với một cặp cố định$(a,b)$, số lượng giá trị hợp lệ của$c$đơn giản là$$\min\!\left( \left\lfloor \frac{n-ab}{a+b} \right\rfloor, x-a-b \right)$$miễn là đại lượng này dương. 

Bây giờ vấn đề trở thành đếm theo cặp$(a,b)$. 

Chúng ta vẫn cần tránh lặp lại tất cả$10^{12}$các cặp có thể. Bản thân sự bất bình đẳng về sản phẩm mang lại một ràng buộc chặt chẽ. Từ$c\ge1$,$$ab+a+b \le n.$$Sắp xếp lại,$$b(a+1) \le n-a.$$Như vậy$$b \le \frac{n-a}{a+1}.$$Đối với lớn$a$, giới hạn trên này nhanh chóng trở nên nhỏ. Tổng số cặp được kiểm tra là khoảng$$\sum_{a=1}^{n} \frac{n}{a} = O(n\log n).$$Bởi vì tổng của tất cả$n$các giá trị trên đầu vào chỉ là$10^6$, độ phức tạp của chuỗi hài này dễ dàng đủ nhanh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^3)$hoặc tệ hơn |$O(1)$| Quá chậm | 
| Tối ưu |$O(n \log n)$mỗi đầu vào tổng hợp |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo câu trả lời về 0. 
2. Lặp lại tất cả các giá trị dương của$a$. 
3. Đối với hiện tại$a$, tính giá trị lớn nhất có thể$b$điều đó vẫn có thể cho phép một số điều tích cực$c$. 

Từ$ab+a+b\le n$,$$b \le \left\lfloor \frac{n-a}{a+1} \right\rfloor.$$Từ$a+b+c\le x$với$c\ge1$,$$b \le x-a-1.$$Lấy mức tối thiểu của hai giới hạn này. 
4. Lặp lại$b$từ$1$tới giá trị tối đa đó. 
5. Tính giới hạn theo sản phẩm trên$c$:$$c_1= \left\lfloor \frac{n-ab}{a+b} \right\rfloor.$$6. Tính giới hạn tính tổng trên$c$:$$c_2=x-a-b.$$7. Mọi số nguyên$c$trong phạm vi$$1 \le c \le \min(c_1,c_2)$$là hợp lệ, vì vậy hãy thêm$$\min(c_1,c_2)$$để trả lời. 
8. Sau tất cả các cặp$(a,b)$đã được xử lý, xuất ra câu trả lời tích lũy. 

### Tại sao nó hoạt động 

Đối với một cặp cố định$(a,b)$, cả hai bất đẳng thức ban đầu đều độc lập tạo ra giới hạn trên của$c$. Một giá trị của$c$có giá trị chính xác khi nó thỏa mãn cả hai giới hạn cùng một lúc. Cho phép lớn nhất$c$do đó là giá trị nhỏ nhất của hai giới hạn. 

Mỗi bộ ba hợp lệ tương ứng với chính xác một cặp$(a,b)$và một giá trị được tính của$c$. Mỗi giá trị đếm được của$c$thỏa mãn cả hai bất đẳng thức ban đầu bằng cách xây dựng. Điều này thiết lập sự tương ứng một-một giữa các đối tượng được đếm và bộ ba hợp lệ, do đó thuật toán đếm không quá nhiều cũng không quá ít. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n, x = map(int, input().split())

        total = 0

        a = 1
        while True:
            b1 = (n - a) // (a + 1)
            b2 = x - a - 1
            bmax = min(b1, b2)

            if bmax < 1:
                if b1 < 1:
                    break
                a += 1
                continue

            for b in range(1, bmax + 1):
                c1 = (n - a * b) // (a + b)
                c2 = x - a - b
                total += min(c1, c2)

            a += 1

        ans.append(str(total))

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Vòng lặp bên ngoài lặp qua các giá trị có thể có của$a$. biểu hiện```
(n - a) // (a + 1)
```xuất phát trực tiếp từ điều kiện cần$ab+a+b\le n$, điều này đảm bảo rằng ít nhất một giá trị dương của$c$có thể tồn tại. 

Giới hạn thứ hai```
x - a - 1
```xuất phát từ việc cần có chỗ cho sự tích cực$c$trong giới hạn tổng. 

Đối với mỗi cặp hợp lệ$(a,b)$, mã tính toán hai giới hạn trên độc lập trên$c$. Số lượng giá trị được chấp nhận chính xác là giá trị nhỏ hơn trong hai giới hạn. 

Một sai lầm thường gặp là quên điều đó$c$phải tích cực. Giới hạn cho$b$có nguồn gốc giả định$c\ge1$, đó là lý do tại sao các công thức chứa phần bổ sung$+a+b$điều khoản thay vì chỉ$ab$. 

Số nguyên Python tự động xử lý các câu trả lời lớn, có thể vượt quá$2^{31}$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 7, x = 4
```| một | b | c từ sản phẩm | c từ tổng | đã thêm | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | 2 | 2 | 
| 1 | 2 | 1 | 1 | 1 | 
| 2 | 1 | 1 | 1 | 1 | 

Tổng số câu trả lời = 4. 

Bộ ba đếm được là:$$(1,1,1), (1,1,2), (1,2,1), (2,1,1).$$Ví dụ này cho thấy một cặp đơn lẻ$(a,b)$có thể đóng góp nhiều giá trị của$c$. 

### Ví dụ 2 

đầu vào:```
n = 10, x = 5
```| một | b | c từ sản phẩm | c từ tổng | đã thêm | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 4 | 3 | 3 | 
| 1 | 2 | 2 | 2 | 2 | 
| 1 | 3 | 1 | 1 | 1 | 
| 2 | 1 | 2 | 2 | 2 | 
| 2 | 2 | 1 | 1 | 1 | 
| 3 | 1 | 1 | 1 | 1 | 

Tổng số câu trả lời = 10. 

Dấu vết này chứng tỏ rằng bất đẳng thức nào cũng có thể trở thành hạn chế tích cực. Đôi khi giới hạn tổng nhỏ hơn, đôi khi giới hạn sản phẩm nhỏ hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O\!\left(\sum \frac{n}{a}\right)=O(n\log n)$| Số lượng chuỗi hài của các cặp được kiểm tra | 
| Không gian |$O(1)$| Chỉ có một vài biến số nguyên được lưu trữ | 

Một thực tế quan trọng là tổng$n$trên tất cả các trường hợp thử nghiệm là nhiều nhất$10^6$. Số lần lặp chuỗi hài hòa là khoảng$n\log n$, nằm trong khoảng thời gian thoải mái cho kích thước đầu vào tổng hợp này. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline
    t = int(input())
    out = []

    for _ in range(t):
        n, x = map(int, input().split())

        total = 0
        a = 1

        while True:
            b1 = (n - a) // (a + 1)
            b2 = x - a - 1
            bmax = min(b1, b2)

            if bmax < 1:
                if b1 < 1:
                    break
                a += 1
                continue

            for b in range(1, bmax + 1):
                c1 = (n - a * b) // (a + b)
                c2 = x - a - b
                total += min(c1, c2)

            a += 1

        out.append(str(total))

    return "\n".join(out)

# provided sample
assert run(
"""4
7 4
10 5
7 1000
900000 400000
"""
) == """4
10
7
1768016938"""

# minimum values
assert run(
"""1
1 1
"""
) == "0"

# exactly one valid triple
assert run(
"""1
3 3
"""
) == "1"

# sum bound dominates
assert run(
"""1
100 3
"""
) == "1"

# small manual case
assert run(
"""1
4 10
"""
) == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`0`| Không tồn tại bộ ba dương | 
|`3 3`|`1`| Bộ ba hợp lệ nhỏ nhất khả thi | 
|`100 3`|`1`| Ràng buộc tổng rất chặt chẽ | 
|`4 10`|`1`| Giới hạn số tiền lớn, ràng buộc sản phẩm chiếm ưu thế | 

## Vỏ cạnh 

Hãy xem xét:```
1
1 1
```Giá trị nhỏ nhất có thể có của$ab+ac+bc$với số nguyên dương là$3$. Vòng lặp tính toán$$b_{\max}=\left\lfloor\frac{1-1}{2}\right\rfloor=0,$$vậy là không có cặp$(a,b)$được xử lý và câu trả lời vẫn còn$0$. 

Bây giờ hãy xem xét:```
1
7 1000
```Ràng buộc tổng thực sự không liên quan. Vì$(a,b)=(1,1)$, sản phẩm bị ràng buộc mang lại$c\le2$, trong khi tổng giới hạn cho$c\le998$. Thuật toán lấy đúng giá trị nhỏ hơn và chỉ tính giá trị khả thi$c$'S. 

Cuối cùng hãy xem xét:```
1
7 4
```Bộ ba$(1,1,2)$,$(1,2,1)$, Và$(2,1,1)$tất cả đều khác biệt vì các vòng lặp lặp theo các cặp có thứ tự$(a,b)$. Không thực hiện giảm đối xứng, do đó các bộ ba có thứ tự được tính chính xác theo yêu cầu.
