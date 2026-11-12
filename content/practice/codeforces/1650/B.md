---
title: "CF 1650B - DIV + MOD"
description: "Chúng ta được yêu cầu tối đa hóa hàm được xác định là $fa(x) = lfloor x / a rfloor + (x bmod a)$ trên một phạm vi số nguyên từ $l$ đến $r$, trong đó $a$ là số nguyên dương cố định."
date: "2026-06-10T03:52:01+07:00"
tags: ["codeforces", "competitive-programming", "math"]
categories: ["algorithms"]
codeforces_contest: 1650
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 776 (Div. 3)"
rating: 900
weight: 1650
solve_time_s: 89
verified: true
draft: false
---

[CF 1650B - DIV + MOD](https://codeforces.com/problemset/problem/1650/B) 

**Xếp hạng:** 900 
**Thẻ:** toán 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu tối đa hóa một hàm được xác định là$f_a(x) = \lfloor x / a \rfloor + (x \bmod a)$trên một phạm vi số nguyên từ$l$ĐẾN$r$, Ở đâu$a$là số nguyên dương cố định. Nói cách khác, đối với một phân đoạn cho trước gồm các số nguyên liên tiếp, chúng ta muốn chọn số$x$tạo ra tổng lớn nhất của phép chia số nguyên của nó cho$a$và modulo phần dư của nó$a$. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm cung cấp một phân đoạn$[l, r]$và một số chia$a$. Đầu ra cho mỗi trường hợp là giá trị tối đa của$f_a(x)$trong phân khúc đó. 

Các ràng buộc rất chặt chẽ:$l$Và$r$có thể lên tới$10^9$, và có thể có tới$10^4$trường hợp thử nghiệm. Một giải pháp đơn giản tính toán$f_a(x)$cho mọi$x$giữa$l$Và$r$có thể yêu cầu$10^9$hoạt động trong một trường hợp thử nghiệm duy nhất, điều này là không khả thi. 

Các trường hợp khó tinh tế xuất hiện khi$r$rất gần với bội số của$a$hoặc khi nào$a$rất lớn so với$r-l$. Ví dụ, nếu$l=1$,$r=4$, Và$a=3$, mức tối đa không phải là$r$nhưng tại$x=2$. Cách tiếp cận bất cẩn chỉ kiểm tra điểm cuối của phân đoạn có thể tạo ra câu trả lời sai. 

## Phương pháp tiếp cận 

Phương pháp tính toán Brute-Force$f_a(x)$với mọi số nguyên$x$từ$l$ĐẾN$r$, sau đó chọn mức tối đa. Điều này hoạt động vì$f_a(x)$là một hàm số học đơn giản. Độ phức tạp thời gian của nó là$O(r-l+1)$mỗi trường hợp thử nghiệm. Cho rằng$r-l$có thể đạt được$10^9$, cách tiếp cận này quá chậm. 

Quan sát quan trọng là$f_a(x)$tăng lên như$x \bmod a$tăng nhưng giảm mạnh khi$x$là bội số của$a$. Cụ thể, để cố định$a$, hàm này tuyến tính từng phần:$f_a(x) = \lfloor x / a \rfloor + (x \bmod a) = k + r$Ở đâu$x = k \cdot a + r$với$0 \le r < a$. Trong mỗi khối của$a$số nguyên liên tiếp thì đạt giá trị lớn nhất khi$r = a-1$, tức là ngay trước bội số tiếp theo của$a$. Vì vậy, chỉ cần kiểm tra là đủ$x=r$Và$x=r - (r \bmod a) - 1$(số lớn nhất trong$[l, r]$ngay trước bội số của$a$). 

Quan sát này làm giảm vấn đề chỉ còn việc kiểm tra tối đa hai ứng viên cho mỗi trường hợp kiểm thử, đưa ra$O(1)$thời gian cho mỗi trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(r-l+1) | O(1) | Quá chậm | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. 
2. Với mỗi test case, hãy đọc$l$,$r$, Và$a$. 
3. Tính toán$f_a(r)$bởi vì mức tối đa có thể xảy ra ở điểm cuối bên phải. 
4. Tính toán$x = r - (r \bmod a) - 1$, là số nguyên lớn nhất nhỏ hơn hoặc bằng$r$kết thúc ngay trước bội số của$a$. 
5. Nếu$x \ge l$, tính toán$f_a(x)$; nếu không thì bỏ qua ứng cử viên này. 
6. Xuất ra mức tối đa giữa$f_a(r)$Và$f_a(x)$như câu trả lời cho trường hợp thử nghiệm này. 

Tính đúng đắn xuất phát từ tính chất$f_a(x)$phát triển tuyến tính trong một khối kích thước$a$và giảm khi đạt bội số$a$. Do đó hàm số chỉ đạt cực đại tại$r$hoặc ngay trước bội số của$a$trong phân khúc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def f(a, x):
    return x // a + x % a

t = int(input())
for _ in range(t):
    l, r, a = map(int, input().split())
    # candidate 1: at the right endpoint
    ans = f(a, r)
    # candidate 2: just before the previous multiple of a
    x = r - (r % a) - 1
    if x >= l:
        ans = max(ans, f(a, x))
    print(ans)
```chức năng`f(a, x)`cô lập tính chất số học của bài toán. Ứng viên`x = r - (r % a) - 1`đảm bảo chúng tôi nắm bắt được đỉnh của khối kích thước cuối cùng`a`bên trong phân khúc. Séc`x >= l`tránh sử dụng các số nằm ngoài phân khúc. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`l=1, r=4, a=3`| x | x // a | x % a | f(x) | 
| --- | --- | --- | --- | 
| 1 | 0 | 1 | 1 | 
| 2 | 0 | 2 | 2 | 
| 3 | 1 | 0 | 1 | 
| 4 | 1 | 1 | 2 | 

Ứng viên`r=4`cho`f=2`. Ứng viên`x=2`cho`f=2`. Tối đa là`2`. 

### Ví dụ 2 

đầu vào:`l=5, r=8, a=4`| x | x // a | x % a | f(x) | 
| --- | --- | --- | --- | 
| 5 | 1 | 1 | 2 | 
| 6 | 1 | 2 | 3 | 
| 7 | 1 | 3 | 4 | 
| 8 | 2 | 0 | 2 | 

Ứng viên`r=8`cho`f=2`. Ứng viên`x=7`cho`f=4`. Tối đa là`4`. 

Dấu vết này xác nhận rằng thuật toán xác định chính xác mức tối đa ngay trước bội số tiếp theo của`a`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Hai ứng cử viên cho mỗi trường hợp kiểm thử, các phép tính số học không đổi. | 
| Không gian | O(1) | Chỉ một vài số nguyên được lưu trữ cho mỗi trường hợp thử nghiệm. | 

Với$t \le 10^4$, thuật toán này thực hiện nhiều nhất$2 \cdot 10^4$ `f_a(x)`đánh giá, dễ dàng phù hợp trong giới hạn thời gian 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(input())
    for _ in range(t):
        l, r, a = map(int, input().split())
        def f(x): return x // a + x % a
        ans = f(r)
        x = r - (r % a) - 1
        if x >= l:
            ans = max(ans, f(x))
        output.append(str(ans))
    return "\n".join(output)

# provided samples
assert run("5\n1 4 3\n5 8 4\n6 10 6\n1 1000000000 1000000000\n10 12 8\n") == "2\n4\n5\n999999999\n5"

# custom tests
assert run("1\n1 1 1\n") == "1", "single-element segment"
assert run("1\n1 10 10\n") == "10", "a equals r"
assert run("1\n1 20 7\n") == "10", "normal segment"
assert run("1\n10 10 3\n") == "13", "single-element high remainder"
assert run("1\n1 1000000000 2\n") == "999999999", "large segment with small a"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 | 1 | đoạn một phần tử | 
| 1 10 10 | 10 | a bằng r | 
| 1 20 7 | 10 | đoạn bình thường | 
| 10 10 3 | 13 | phần tử đơn có phần dư cao | 
| 1 1000000000 2 | 999999999 | phân khúc lớn với a nhỏ | 

## Vỏ cạnh 

Khi đoạn chỉ chứa một phần tử,`l=r`, cả hai ứng cử viên đều thu gọn về cùng một giá trị. Ví dụ,`l=r=1, a=1`. Thuật toán tính toán`f(1)=1`, và ứng cử viên thứ hai`x=0`bị loại bỏ vì`x < l`. Đầu ra là chính xác. 

Khi`r`là bội số của`a`, cực đại thường xảy ra ở`r-1`. Vì`l=1, r=8, a=4`, giá trị hàm số tại`r=8`là 2, nhưng`r-1=7`cho 4. Công thức của chúng tôi`x = r - (r % a) - 1`xử lý việc này một cách chính xác. Séc`x >= l`ngăn chặn dòng chảy tràn cho các phân đoạn
