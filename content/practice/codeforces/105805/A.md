---
title: "CF 105805A - Đệ trình là tất cả những gì bạn cần"
description: "Chúng ta được cho một số nguyên dương $n$. Đối với mỗi trường hợp kiểm thử, chúng ta phải đếm có bao nhiêu số nguyên $x$ thỏa mãn $$x + (x bmod n) = n.$$ Dữ liệu đầu vào chứa tối đa $10^4$ trường hợp kiểm thử và mỗi giá trị của $n$ có thể lớn bằng $10^{18}$."
date: "2026-06-25T15:31:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105805
codeforces_index: "A"
codeforces_contest_name: "TheForces Round #41 (Magical-Forces)"
rating: 0
weight: 105805
solve_time_s: 45
verified: true
draft: false
---

[CF 105805A - Tất cả những gì bạn cần là gửi bài](https://codeforces.com/problemset/problem/105805/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số nguyên dương$n$. Với mỗi test case, chúng ta phải đếm xem có bao nhiêu số nguyên$x$thỏa mãn$$x + (x \bmod n) = n.$$Đầu vào chứa tối đa$10^4$trường hợp thử nghiệm và mỗi giá trị của$n$có thể lớn như$10^{18}$. Chúng ta chỉ cần xuất ra số nguyên hợp lệ$x$cho từng trường hợp thử nghiệm. 

Kích thước của$n$ngay lập tức loại trừ mọi cách tiếp cận thử từng giá trị ứng viên. Thậm chí kiểm tra tất cả các giá trị từ$0$ĐẾN$n$sẽ là không thể khi$n$có thể đạt được$10^{18}$. Lời giải phải đến từ việc xử lý phương trình đại số cho đến khi có thể tính được câu trả lời trong thời gian không đổi. 

Một vài trường hợp cạnh rất dễ bị bỏ sót. 

Ví dụ, khi$n = 2$,```
x = 1 -> 1 + (1 mod 2) = 2
x = 2 -> 2 + (2 mod 2) = 2
```Câu trả lời đúng là 2. Việc thực hiện bất cẩn chỉ có thể nhận thấy$x=n$và trả về 1. 

cho$n = 3$,```
x = 3 -> 3 + (3 mod 3) = 3
```Không có giá trị nào khác hoạt động, vì vậy câu trả lời là 1. Điều này cho thấy rằng giải pháp bổ sung chỉ xuất hiện cho các giá trị chẵn của$n$. 

Vì$n = 1$,```
x = 1 -> 1 + (1 mod 1) = 1
```Câu trả lời vẫn là 1. Bất kỳ công thức nào cũng phải xử lý chính xác dữ liệu đầu vào nhỏ nhất có thể. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là thử mọi cách có thể$x$, tính toán$x \bmod n$, và kiểm tra xem phương trình có đúng không. 

Tại sao chúng ta có thể hạn chế mình$x \le n$? Bởi vì$x \bmod n$không âm nên nếu$x > n$, sau đó$$x + (x \bmod n) > n,$$không bao giờ thỏa mãn phương trình. 

Phương pháp bạo lực này là đúng, nhưng đối với$n = 10^{18}$nó sẽ yêu cầu khoảng$10^{18}$kiểm tra, điều này hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là phép toán modulo trở nên đơn giản khi chúng ta viết$x$ở dạng thương-dư:$$x = qn + r,
\qquad 0 \le r < n.$$Sau đó$$x \bmod n = r.$$Thay vào phương trình sẽ cho$$qn + 2r = n.$$Bây giờ vấn đề hoàn toàn là số học. 

Từ$qn \le n$, thương số$q$chỉ có thể là 0 hoặc 1. 

Nếu$q = 1$, sau đó$$n + 2r = n,$$Vì thế$r = 0$, đưa ra giải pháp$$x = n.$$Nếu như$q = 0$, sau đó$$2r = n.$$Điều này chỉ có thể thực hiện được khi$n$là chẵn. Trong trường hợp đó,$$r = \frac{n}{2},$$Và$$x = \frac{n}{2}.$$Không có giá trị nào khác của$q$là có thể, vì vậy chúng tôi đã mô tả đầy đủ tất cả các giải pháp. 

Mọi$n$có giải pháp$x=n$. Các giá trị chẵn của$n$có một giải pháp bổ sung,$x=n/2$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Quá chậm | 
| Tối ưu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc giá trị của$n$. 
2. Kiểm tra xem$n$là số lẻ hoặc số chẵn. 
3. Nếu$n$là số lẻ, đầu ra 1. 

Giải pháp duy nhất là$x=n$. 
4. Nếu$n$chẵn, đầu ra 2. 

Các giải pháp là$x=n$Và$x=n/2$. 

### Tại sao nó hoạt động 

Viết$x$BẰNG$qn+r$biến đổi phương trình ban đầu thành$$qn+2r=n.$$Bởi vì$0 \le r < n$, thương không thể vượt quá 1. Trường hợp$q=1$luôn mang lại kết quả$x=n$. Vụ án$q=0$mang lại giải pháp thứ hai chính xác khi$n$là chẵn. Đây là những trường hợp duy nhất có thể xảy ra nên thuật toán tính mọi giải pháp hợp lệ và không bao giờ bỏ sót một giải pháp nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    n = int(input())
    if n % 2 == 0:
        print(2)
    else:
        print(1)
```Việc thực hiện tuân theo đặc tính toán học một cách trực tiếp. 

Hoạt động duy nhất chúng ta cần là kiểm tra tính chẵn lẻ của$n$. Khi$n$là số lẻ, tập lời giải chỉ chứa$x=n$. Khi$n$là chẵn,$x=n/2$trở thành một giải pháp hợp lệ thứ hai. 

Số nguyên Python hỗ trợ thoải mái các giá trị lên tới$10^{18}$, do đó không có mối lo ngại tràn. Thời gian chạy là không đổi cho mỗi trường hợp thử nghiệm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 3
```| Bước | Giá trị | 
| --- | --- | 
| Đọc n | 3 | 
| Kiểm tra tính chẵn lẻ | lẻ | 
| Trả lời | 1 | 

Giá trị hợp lệ duy nhất là$x=3$. Phương trình trở thành$$3 + (3 \bmod 3) = 3.$$Không có giải pháp thứ hai tồn tại bởi vì$3/2$không phải là số nguyên. 

### Ví dụ 2 

đầu vào:```
n = 8
```| Bước | Giá trị | 
| --- | --- | 
| Đọc n | 8 | 
| Kiểm tra tính chẵn lẻ | Thậm chí | 
| Trả lời | 2 | 

Hai giải pháp là: 

| x | x mod 8 | x + (x mod 8) | 
| --- | --- | --- | 
| 4 | 4 | 8 | 
| 8 | 0 | 8 | 

Ví dụ này minh họa giải pháp bổ sung xuất hiện bất cứ khi nào$n$là chẵn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) cho mỗi trường hợp thử nghiệm | Chỉ thực hiện kiểm tra tính chẵn lẻ | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu phụ trợ | 

Với nhiều nhất$10^4$trường hợp thử nghiệm, tổng công việc là rất nhỏ. Giải pháp dễ dàng phù hợp với giới hạn của cuộc thi. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())
        ans.append("2" if n % 2 == 0 else "1")

    sys.stdout.write("\n".join(ans))

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out.getvalue()

# provided sample
assert run("2\n1\n3\n") == "1\n1"

# minimum value
assert run("1\n1\n") == "1"

# smallest even number
assert run("1\n2\n") == "2"

# large odd value
assert run("1\n999999999999999999\n") == "1"

# large even value
assert run("1\n1000000000000000000\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1`|`1`| Đầu vào tối thiểu | 
|`1 / 2`|`2`| Số chẵn đầu tiên | 
|`1 / 999999999999999999`|`1`| Giá trị lẻ lớn | 
|`1 / 1000000000000000000`|`2`| Giá trị chẵn lớn | 
| Đầu vào mẫu |`1 1`| Ví dụ về câu lệnh khớp | 

## Vỏ cạnh 

Hãy xem xét:```
1
2
```Thuật toán phát hiện 2 là số chẵn và đưa ra 2. 

Hai giải pháp là:```
x = 1
x = 2
```Một giải pháp chỉ kiểm tra$x=n$sẽ trả về không chính xác 1. 

Bây giờ hãy xem xét:```
1
3
```Thuật toán phát hiện 3 là số lẻ và đưa ra 1. 

Sử dụng phương trình dẫn xuất$$qn + 2r = n,$$trường hợp$q=0$sẽ yêu cầu$2r=3$, không có nghiệm số nguyên. Chỉ một$x=3$vẫn còn. 

Cuối cùng, hãy xem xét:```
1
1
```Thuật toán đưa ra 1. 

Thật vậy,$$1 + (1 \bmod 1) = 1.$$Nhánh số chẵn không được kích hoạt, do đó đầu vào nhỏ nhất được xử lý chính xác.
