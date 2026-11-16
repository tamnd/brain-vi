---
title: "CF 1933C - Ngón tay rùa: Đếm giá trị của k"
description: "Chúng ta được yêu cầu đếm số lượng các giá trị riêng biệt $k$ sao cho chúng ta có thể biểu diễn một số $l$ cho trước dưới dạng $l = k cdot a^x cdot b^y$ đối với các số nguyên không âm $x$ và $y$."
date: "2026-06-08T18:13:34+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1933
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 929 (Div. 3)"
rating: 1100
weight: 1933
solve_time_s: 101
verified: true
draft: false
---

[CF 1933C - Ngón tay rùa: Đếm giá trị của k](https://codeforces.com/problemset/problem/1933/C) 

**Đánh giá:** 1100 
**Tags:** vũ lực, thực hiện, toán học, lý thuyết số 
**Thời gian giải:** 1 phút 41 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu đếm số lượng các giá trị khác nhau$k$sao cho chúng ta có thể đại diện cho một số nhất định$l$BẰNG$l = k \cdot a^x \cdot b^y$cho số nguyên không âm$x$Và$y$. Nói cách khác,$k$là hệ số dư của$l$sau khi tính ra sức mạnh của$a$Và$b$trong mọi sự kết hợp có thể. Mỗi trường hợp thử nghiệm cho ba số nguyên$a$,$b$, Và$l$, và chúng ta phải xuất ra số lượng khác nhau$k$các giá trị thỏa mãn sự phân tách này. 

Những ràng buộc chỉ ra rằng$a$Và$b$tương đối nhỏ ($\le 100$) trong khi$l$có thể lên tới$10^6$và số lượng ca kiểm thử có thể đạt tới$10^4$. Điều này ngụ ý rằng bất kỳ giải pháp nào lặp qua tất cả các số lên đến$l$sẽ là quá chậm. Chúng ta cần một cách tiếp cận chỉ khám phá cấu trúc nhân của$l$một cách hiệu quả. 

Một sự tinh tế quan trọng nảy sinh khi$a$Và$b$là các lũy thừa của cùng một số nguyên tố hoặc có chung thừa số nguyên tố. Ví dụ, nếu$a = 4$Và$b = 8$, sau đó nhân$a^x \cdot b^y$có thể tạo ra những đóng góp chồng chéo cho cùng một số mũ của 2. Một cách tiếp cận ngây thơ mà bỏ qua việc phân tích thừa số nguyên tố có thể tính hai lần hoặc bỏ lỡ các khả năng. Một trường hợp cạnh khác là khi$l$cũng không chia hết cho$a$hoặc$b$, điều này ngay lập tức mang lại$k = l$. 

## Phương pháp tiếp cận 

Một cách tiếp cận vũ phu sẽ thử tất cả các lũy thừa không âm$x$Và$y$như vậy$a^x \le l$Và$b^y \le l$, sau đó tính$k = l / (a^x \cdot b^y)$nếu phép chia là chính xác. Mặc dù đúng nhưng điều này có thể yêu cầu tới$(\log_a l) \cdot (\log_b l)$lặp lại cho mỗi trường hợp thử nghiệm, điều này khả thi đối với các trường hợp nhỏ$l$, nhưng có thể tốn kém với$t = 10^4$trường hợp thử nghiệm. 

Cái nhìn sâu sắc quan trọng là chúng tôi chỉ quan tâm đến các giá trị riêng biệt của$k$, tương ứng với các ước của$l$sau khi loại bỏ tất cả quyền hạn của$a$Và$b$. Điều này cho thấy chúng ta có thể sử dụng cấu trúc nhân của$l$để lặp lại quyền hạn một cách hiệu quả: 

1. Tính trước tất cả lũy thừa của$a$Và$b$điều đó không vượt quá$l$. 
2. Nhân mọi kết hợp$a^x \cdot b^y$và kiểm tra xem nó có chia không$l$. Nếu vậy,$k = l / (a^x \cdot b^y)$là hợp lệ. 
3. Cửa hàng$k$trong một bộ để tự động loại bỏ trùng lặp. 

Điều này làm giảm vấn đề xuống còn việc chỉ lặp qua các lũy thừa hợp lệ, nhỏ hơn nhiều so với việc lặp trên tất cả các số nguyên lên đến$l$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((log l / log a) * (log l / log b)) trên mỗi trường hợp thử nghiệm | O(1) | Quá chậm đối với t lớn | 
| Tối ưu | O((log l / log a) * (log l / log b)) trên mỗi trường hợp thử nghiệm | O(#distinct k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc$a$,$b$, Và$l$. 
2. Khởi tạo một tập hợp trống`k_values`để lưu trữ các kết quả riêng biệt. 
3. Tạo danh sách quyền hạn của$a$điều đó không vượt quá$l$. Bắt đầu bằng 1 và nhân liên tục với$a$cho đến khi vượt quá$l$. Lưu trữ trong`powers_a`. 
4. Tương tự, tạo danh sách quyền hạn của$b$điều đó không vượt quá$l$. Lưu trữ trong`powers_b`. 
5. Đối với mỗi$p\_a$TRONG`powers_a`Và$p\_b$TRONG`powers_b`, tính toán`product = p_a * p_b`. Nếu như`l % product == 0`, tính toán`k = l // product`và thêm nó vào`k_values`. 
6. After all combinations are checked, the size of`k_values`là câu trả lời cho trường hợp thử nghiệm này. Xuất nó ra. 
7. Lặp lại cho tất cả các trường hợp thử nghiệm. 

Tại sao nó hoạt động: Bằng cách lặp lại tất cả các lũy thừa khả thi của$a$Và$b$không vượt quá$l$, chúng tôi đảm bảo rằng mọi tiềm năng$k$được xem xét. Việc sử dụng phép chia số nguyên đảm bảo rằng chỉ có các hệ số chính xác mới đóng góp và tập hợp này sẽ loại bỏ các bản sao. Cách tiếp cận này tránh được sự lặp lại không cần thiết đối với những sự kết hợp không thể thực hiện được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        a, b, l = map(int, input().split())
        k_values = set()
        
        powers_a = []
        x = 1
        while x <= l:
            powers_a.append(x)
            if l // x < a:
                break
            x *= a
            
        powers_b = []
        y = 1
        while y <= l:
            powers_b.append(y)
            if l // y < b:
                break
            y *= b
        
        for p_a in powers_a:
            for p_b in powers_b:
                product = p_a * p_b
                if product > l:
                    continue
                if l % product == 0:
                    k_values.add(l // product)
        
        print(len(k_values))

if __name__ == "__main__":
    solve()
```Mã cẩn thận ngăn chặn tràn số nguyên bằng cách kiểm tra`l // x < a`Và`l // y < b`trước khi nhân, đảm bảo lũy thừa không vượt quá`l`. Sử dụng một bộ đảm bảo chúng tôi đếm từng`k`chỉ một lần. 

## Ví dụ đã hoạt động 

Đầu vào mẫu`2 5 20`: 

| x | y | a^x | b^y | sản phẩm | k = l/sản phẩm | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 1 | 1 | 1 | 20 | 
| 0 | 1 | 1 | 5 | 5 | 4 | 
| 1 | 0 | 2 | 1 | 2 | 10 | 
| 1 | 1 | 2 | 5 | 10 | 2 | 
| 2 | 0 | 4 | 1 | 4 | 5 | 
| 2 | 1 | 4 | 5 | 20 | 1 | 

Tất cả sáu`k`các giá trị được tính. Thuật toán xử lý hiệu quả các công suất vừa và nhỏ. 

Đầu vào mẫu`2 5 21`: 

| x | y | a^x | b^y | sản phẩm | k = l/sản phẩm | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 1 | 1 | 1 | 21 | 

Tất cả các quyền hạn khác của`a`hoặc`b`quá`l`hoặc không chia`l`. Thuật toán đếm đúng 1 giá trị. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t * (log_a l) * (log_b l)) | Mỗi trường hợp thử nghiệm tạo ra lũy thừa của a và b, lặp lại các kết hợp. Với a,b <= 100 và l <= 10^6, giới hạn log là nhỏ. | 
| Không gian | O(#distinct k) | Lưu trữ tất cả các giá trị k riêng biệt cho mỗi trường hợp thử nghiệm, tối đa là l nhưng thường nhỏ hơn nhiều. | 

Cho t <= 10^4 và l <= 10^6, thuật toán rất phù hợp trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# Provided samples
assert run("11\n2 5 20\n2 5 21\n4 6 48\n2 3 72\n3 5 75\n2 2 1024\n3 7 83349\n100 100 1000000\n7 3 2\n2 6 6\n17 3 632043\n") == "6\n1\n5\n12\n6\n11\n24\n4\n1\n3\n24"

# Custom cases
assert run("1\n2 2 1\n") == "1", "smallest l, a=b=2"
assert run("1\n2 3 6\n") == "4", "all factors count"
assert run("1\n5 5 25\n") == "3", "a=b same"
assert run("1\n100 100 1000000\n") == "4", "large a,b,l"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 2 1 | 1 | l nhỏ nhất, a=b | 
| 2 3 6 | 4 | kết hợp nhiều yếu tố | 
| 5 5 25 | 3 | kịch bản a=b | 
| 100 100 1000000 | 4 | số lượng lớn, hiệu quả | 

## Vỏ cạnh 

Khi nào`l`cũng không chia hết cho`a`hoặc`b`, chỉ `
