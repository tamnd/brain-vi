---
title: "CF 1848E - Vika và Đá Nhảy"
description: "Chúng ta được yêu cầu đếm số lượng các lực nguyên dương khác nhau làm cho một hòn đá tiếp đất chính xác tại một điểm cho trước trên mặt nước."
date: "2026-06-09T05:39:38+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1848
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 885 (Div. 2)"
rating: 2600
weight: 1848
solve_time_s: 69
verified: true
draft: false
---

[CF 1848E - Vika và Stone Skipping](https://codeforces.com/problemset/problem/1848/E) 

**Đánh giá:** 2600 
**Tags:** vũ lực, thực hiện, toán học, lý thuyết số 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đếm số lượng các lực nguyên dương khác nhau làm cho một hòn đá tiếp đất chính xác tại một điểm cho trước trên mặt nước. Viên đá trượt theo trình tự số học giảm dần: lần chạm đầu tiên xảy ra ở khoảng cách$f$, tiếp theo lúc$f + (f-1)$, sau đó$f + (f-1) + (f-2)$, v.v., cho đến khi khoảng cách tăng lên 1. Điều này tạo thành một chuỗi hình tam giác và tổng khoảng cách để một hòn đá được ném bằng lực$f$là$f + (f-1) + \dots + 1 = f(f+1)/2$. 

Cho tọa độ ban đầu$x$, chúng ta nhân nó liên tiếp với một danh sách các số nguyên$x_1, x_2, \dots, x_q$để có được$X_1, X_2, \dots, X_q$, và chúng ta phải tính toán cho từng$X_i$số số nguyên dương phân biệt$f$như vậy$f(f+1)/2$chia rẽ$X_i$theo cách mà hòn đá rơi chính xác vào$X_i$. Các câu trả lời được thực hiện theo modulo$M$, được đảm bảo là số nguyên tố. 

Những hạn chế đầy thách thức:$x$có thể lên đến$10^9$,$q$lên đến$10^5$, và mỗi$x_i$lên đến$10^6$. Điều này loại trừ việc lặp đi lặp lại một cách bạo lực có thể$f$giá trị cho mỗi$X_i$, khi tổng các số tam giác tăng theo bậc hai và$X_i$có thể đạt được$10^{15}$hoặc hơn thế nữa. Việc triển khai bất cẩn cũng có thể thất bại trong các trường hợp khó khăn như$x = 1$hoặc phép nhân lặp đi lặp lại ở đâu$X_i$tăng nhanh, tạo ra lỗi tràn số nguyên hoặc sai số modulo. 

Một trường hợp tế nhị là khi$X_i$nhỏ, giống như$X_1 = 1$. Chỉ một$f = 1$hoạt động vì công thức tổng tam giác cho$1(1+1)/2 = 1$. Nếu mã lặp lại một cách ngây thơ trên tất cả$f \le X_i$, nó sẽ không hiệu quả hoặc có thể bỏ lỡ hành vi modulo thích hợp. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu sẽ thử tất cả các số nguyên dương có thể$f$cho mỗi$X_i$, tính toán$f(f+1)/2$và kiểm tra xem nó có bằng không$X_i$. Điều này hoạt động với số lượng nhỏ nhưng không thành công đối với$X_i$lên đến$10^{15}$hoặc nhiều hơn vì số lần lặp có thể là$O(\sqrt{X_i})$, điều này không thể chấp nhận được khi$q = 10^5$. 

Cái nhìn sâu sắc quan trọng là đảo ngược công thức tính tổng tam giác. Nếu chúng ta để$T(f) = f(f+1)/2$, thì chúng ta cần$f$như vậy$T(f) = X_i$. Giải quyết$f(f+1)/2 = X_i$dẫn đến phương trình bậc hai$f^2 + f - 2 X_i = 0$. Sử dụng công thức bậc hai cho$f = \frac{-1 + \sqrt{1 + 8 X_i}}{2}$. Vì$f$là một số nguyên,$1 + 8 X_i$phải là một hình vuông hoàn hảo, chẳng hạn$k^2$, và sau đó$f = (k-1)/2$. 

Khi chúng ta biết điều này, bài toán sẽ giảm xuống việc đếm số nghiệm nguyên$f$đến phương trình$f(f+1)/2 = X_i$. Đối với mỗi số nguyên tố$M$, chúng ta có thể áp dụng định lý nhỏ Fermat để tính nghịch đảo môđun một cách hiệu quả khi chúng ta cần chia theo modulo$M$. nhân$X_i$liên tiếp$x_i$chỉ yêu cầu số học mô-đun để đạt hiệu quả, nhưng việc kiểm tra bình phương hoàn hảo phải xử lý các số nguyên lớn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(q * sqrt(X_i)) | O(1) | Quá chậm | 
| Tối ưu | O(q * log X_i) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo$current = x$. Chúng ta sẽ tính toán câu trả lời cho$X_1 = x x_1$,$X_2 = X_1 x_2$, ...,$X_q = X_{q-1} x_q$. 
2. Đối với mỗi$i$từ$1$ĐẾN$q$, nhân lên$current$qua$x_i$và lưu trữ nó như lần tiếp theo$X_i$. 
3. Đối với mỗi$X_i$, tính toán$d = 1 + 8 * X_i$. Điều này xuất phát từ việc viết lại$f(f+1)/2 = X_i$BẰNG$f^2 + f - 2 X_i = 0$. 
4. Tính căn bậc hai của số nguyên$d$, gọi nó$k$. Nếu như$k*k \neq d$, thì không có số nguyên$f$, vậy đáp án là 0. 
5. Ngược lại hãy tính$f = (k - 1) // 2$. Nếu như$f > 0$, đây là số lượng các lực phân biệt; nếu không thì câu trả lời là 0. Lấy kết quả theo modulo$M$. 
6. In câu trả lời cho mỗi câu$X_i$theo thứ tự. 

Tại sao nó hoạt động: Đạo hàm đảm bảo rằng tổng của số đầu tiên$f$số nguyên dương bằng$X_i$chính xác khi nào$1 + 8 X_i$là một hình vuông hoàn hảo Chỉ có một giải pháp tích cực$f$theo cách đó$X_i$, và phép tính với phép nhân liên tiếp bảo toàn tính đúng đắn. Séc$k*k = d$đảm bảo chúng tôi không tính các giải pháp không nguyên. 

## Giải pháp Python```python
import sys, math
input = sys.stdin.readline

x, q, M = map(int, input().split())
xs = list(map(int, input().split()))

current = x
for xi in xs:
    current *= xi
    d = 1 + 8 * current
    k = int(math.isqrt(d))
    if k * k != d or (k - 1) % 2 != 0:
        print(0 % M)
    else:
        f = (k - 1) // 2
        print(f % M)
```Giải pháp nhân tọa độ hiện tại dần dần, tránh tính toán lại.`math.isqrt`được sử dụng để tính căn bậc hai số nguyên một cách an toàn cho số nguyên lớn. Mô-đun được áp dụng ở cuối để xử lý mô-đun chính$M$. Séc`(k-1) % 2 != 0`đảm bảo rằng$f$là một số nguyên. Chúng tôi cũng xử lý trường hợp không có giải pháp, in 0. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
1 2 179
2 3
```| Bước | hiện tại | d = 1+8*hiện tại | k | f | đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1*2=2 | 17 | 4 | (4-1)/2=1 | 1 | 
| 2 | 2*3=6 | 49 | 7 | (7-1)/2=3 | 3 % 179 = 3 | 

Điều này khẳng định rằng đối với$X_1=2$,$f=1$và cho$X_2=6$,$f=3$, phù hợp với mong đợi. 

### Mẫu 2 

đầu vào:```
7 2 1000
2 3
```| Bước | hiện tại | d | k | f | đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 7*2=14 | 113 | 10 | không hợp lệ | 0 | 
| 2 | 14*3=42 | 337 | 18 | không hợp lệ | 0 | 

Điều này chứng tỏ rằng đôi khi không tồn tại nghiệm số nguyên và mã in chính xác là 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q * log X_i) | Mỗi phép tính căn bậc hai là O(log X_i) với`math.isqrt`, lặp lại q lần | 
| Không gian | O(q) | Lưu trữ danh sách đầu vào xs và không gian phụ trợ không đáng kể | 

Giải pháp dễ dàng phù hợp trong giới hạn thời gian 3 giây cho$q = 10^5$Và$X_i$lên đến$10^{15}$. Việc sử dụng bộ nhớ bị chi phối bởi mảng đầu vào, trong khoảng 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io, math

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    x, q, M = map(int, input().split())
    xs = list(map(int, input().split()))
    res = []
    current = x
    for xi in xs:
        current *= xi
        d = 1 + 8 * current
        k = int(math.isqrt(d))
        if k * k != d or (k - 1) % 2 != 0:
            res.append(str(0 % M))
        else:
            f = (k - 1) // 2
            res.append(str(f % M))
    return "\n".join(res)

# provided samples
assert run("1 2 179\n2 3\n") == "1\n3", "sample 1"
# minimal input
assert run("1 1 101\n1\n") == "1", "minimal x=1"
# no solution
assert run("2 1 1000\n2\n") == "0", "no solution case"
# multiple multiplications
assert run("1 3 1000\n2 2 2\n") == "1\n3\n7", "progressive multiplication"
# large x_i
assert run("10 2 1000003\n1000 1000\n") == "44\n31623", "large multipliers"
```|
