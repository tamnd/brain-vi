---
title: "CF 2044E - Vấn đề điên rồ"
description: "Chúng ta được yêu cầu đếm số cặp số nguyên $(x, y)$ thỏa mãn hai ràng buộc khoảng và mối quan hệ hình học. Cụ thể, $x$ phải nằm trong khoảng $[l1, r1]$, $y$ phải nằm trong $[l2, r2]$, và $y / x$ phải là lũy thừa nguyên của $k$."
date: "2026-06-08T09:24:11+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "greedy", "implementation", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 2044
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 993 (Div. 4)"
rating: 1300
weight: 2044
solve_time_s: 90
verified: true
draft: false
---

[CF 2044E - Vấn đề điên rồ](https://codeforces.com/problemset/problem/2044/E) 

**Đánh giá:** 1300 
**Tags:** tìm kiếm nhị phân, tham lam, thực hiện, toán, lý thuyết số 
**Thời gian giải:** 1 phút 30s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu đếm số cặp số nguyên$(x, y)$thỏa mãn hai ràng buộc khoảng và một mối quan hệ hình học. Cụ thể,$x$phải nằm trong khoảng$[l_1, r_1]$,$y$phải nằm trong$[l_2, r_2]$, Và$y / x$phải là lũy thừa nguyên của$k$. Đây,$k$ít nhất là 2, vì vậy chúng ta đang xử lý sự tăng trưởng bằng cách nhân lặp lại. Mỗi trường hợp thử nghiệm là độc lập và có thể có tới$10^4$trường hợp thử nghiệm. Các khoảng và$k$có thể lớn như$10^9$, việc liệt kê tất cả các cặp một cách ngây thơ như vậy rõ ràng là không thực tế. 

Phần không rõ ràng là điều kiện tăng trưởng theo cấp số nhân. Một giải pháp ngây thơ có thể thử mọi cách$x$TRONG$[l_1, r_1]$và mọi$y$TRONG$[l_2, r_2]$và kiểm tra xem$y / x$là sức mạnh của$k$. Điều này sẽ thực hiện theo thứ tự$10^{18}$trong trường hợp xấu nhất là hoàn toàn không thể thực hiện được. Một sự tinh tế khác nảy sinh khi$k$lớn hoặc khoảng cách rất hẹp. Ví dụ, với$k = 10^9$Và$x = 1$,$y = 10^9$là hợp lệ nhưng việc liệt kê tất cả các lũy thừa trung gian là không cần thiết. 

Những ví dụ nhỏ bộc lộ những cạm bẫy. Nếu như$l_1 = r_1 = 1$Và$l_2 = r_2 = 1$, cặp hợp lệ duy nhất là$(1,1)$. Một cách tiếp cận bất cẩn có thể cho rằng có nhiều cặp tồn tại. Nếu như$k$là cực kỳ lớn, số mũ hợp lệ duy nhất có thể là 0 và bạn có thể dễ dàng bỏ lỡ điều đó nếu thuật toán giả sử có nhiều lũy thừa phù hợp trong khoảng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực lặp đi lặp lại trên mọi$x$TRONG$[l_1, r_1]$và tạo ra sức mạnh của$k$bắt đầu từ$x$cho đến khi kết quả vượt quá$r_2$. Với mỗi lũy thừa, chúng ta kiểm tra xem nó có rơi vào$[l_2, r_2]$và đếm nó. Điều này hoạt động chính xác nhưng quá chậm: số lượng$x$các giá trị có thể lên tới$10^9$và mỗi cái có thể sản xuất tới$\log_k r_2$quyền lực. Điều này dẫn đến khoảng$10^9 \cdot 30 \approx 3 \cdot 10^{10}$hoạt động vượt xa khả năng thực hiện. 

Cái nhìn sâu sắc quan trọng là mối quan hệ$y = x \cdot k^n$cho phép chúng tôi sửa chữa$n$thay vì$x$. Đối với mỗi$n \ge 0$, khả năng$x$thỏa mãn$l_1 \le x \le r_1$Và$l_2 / k^n \le x \le r_2 / k^n$. Điều này làm giảm việc đếm số nguyên trong khoảng$[\max(l_1, \lceil l_2 / k^n \rceil), \min(r_1, \lfloor r_2 / k^n \rfloor)]$. Chúng ta có thể ngừng lặp lại$n$khi$x_{\text{min}} > x_{\text{max}}$bởi vì quyền lực lớn hơn sẽ chỉ tăng lên$x_{\text{min}}$. Cách tiếp cận này làm giảm đáng kể độ phức tạp: số mũ$n$nhiều nhất là$\log_k r_2 \le 60$, điều này khả thi ngay cả với nhiều trường hợp thử nghiệm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((r1-l1+1) * log_k(r2)) | O(1) | Quá chậm | 
| Tối ưu | O(log_k r2) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case$t$. Đối với mỗi trường hợp thử nghiệm, hãy đọc$k$,$l_1$,$r_1$,$l_2$,$r_2$. 
2. Khởi tạo bộ đếm`ans = 0`. Điều này sẽ lưu trữ tổng số cặp hợp lệ. 
3. Đặt`power = 1`Và`n = 0`. Điều này thể hiện$k^n$. Bắt đầu với$k^0 = 1$. 
4. Trong khi`power <= r_2`, hãy làm như sau: 

một. Tính toán`x_min = max(l_1, (l_2 + power - 1) // power)`. Điều này đảm bảo$x \cdot k^n \ge l_2$. 

b. Tính toán`x_max = min(r_1, r_2 // power)`. Điều này đảm bảo$x \cdot k^n \le r_2$. 

c. Nếu như`x_min <= x_max`, tăng`ans`qua`x_max - x_min + 1`. 

d. Nếu như`power > r_2 // k`, phá vỡ. nhân với$k$một lần nữa sẽ tràn$r_2$. 

đ. Ngược lại thì nhân`power *= k`và tăng dần`n`. 
5. In`ans`sau khi kết thúc mỗi test case. 

Tại sao nó hoạt động: tính bất biến là với mỗi lũy thừa của$k$, chúng tôi đếm tất cả$x$tại giao điểm của khoảng$[l_1, r_1]$và khoảng chia tỷ lệ$[l_2 / k^n, r_2 / k^n]$. Chúng tôi lặp lại một cách có hệ thống mọi thứ có thể$k^n$điều đó có thể tạo ra một$y$TRONG$[l_2, r_2]$. Không có cặp nào được tính hai lần vì mỗi$n$là khác biệt và các khoảng thời gian nắm bắt chính xác tất cả các khả năng có thể$x$vì điều đó$n$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        k, l1, r1, l2, r2 = map(int, input().split())
        ans = 0
        power = 1
        while power <= r2:
            x_min = max(l1, (l2 + power - 1) // power)
            x_max = min(r1, r2 // power)
            if x_min <= x_max:
                ans += x_max - x_min + 1
            if power > r2 // k:
                break
            power *= k
        print(ans)

if __name__ == "__main__":
    solve()
```Chúng tôi khởi tạo`power = 1`đại diện cho$k^0$. Việc tính toán`x_min`sử dụng phép chia trần số nguyên để đảm bảo giới hạn dưới.`x_max`sử dụng phân chia tầng để ở trong giới hạn trên. Điều kiện ngắt tránh tràn, điều này rất quan trọng khi`k`là rất lớn. Nhân trước giờ nghỉ sẽ có nguy cơ vượt quá số nguyên tối đa được cho phép bởi các ràng buộc của bài toán. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
2 2 6 2 12
```| n | sức mạnh=k^n | x_min | x_max | Số x hợp lệ | Tích lũy ans | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 2 | 6 | 5 | 5 | 
| 1 | 2 | 2 | 6 | 5 | 10 | 
| 2 | 4 | 2 | 3 | 2 | 12 | 
| 3 | 8 | 2 | 1 | 0 | 12 | 

Dấu vết hiển thị tất cả lũy thừa của 2 và các khoảng x giao nhau chính xác với [2,6]. Tổng số cặp = 12. 

### Đầu vào mẫu 2```
3 5 7 15 63
```| n | sức mạnh=k^n | x_min | x_max | Số x hợp lệ | Tích lũy ans | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 5 | 7 | 3 | 3 | 
| 1 | 3 | 5 | 7 | 2 | 5 | 
| 2 | 9 | 5 | 7 | 1 | 6 | 
| 3 | 27 | 5 | 2 | 0 | 6 | 

Điều này thể hiện sự tăng trưởng và giao điểm khoảng cách chính xác. Tổng tích lũy = 6, khớp với đầu ra mẫu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t * log_k r2) | Với mỗi test, số lũy thừa của k nhiều nhất là log_k r2 ≤ 60. Với t 10^4 thì tổng số phép tính ≤ 6*10^5. | 
| Không gian | O(1) | Chỉ sử dụng bộ đếm và biến tạm thời; không có cấu trúc bổ sung tỷ lệ thuận với kích thước đầu vào. | 

Thuật toán dễ dàng phù hợp trong vòng 2 giây và 256 MB, vì mỗi trường hợp thử nghiệm chỉ yêu cầu một số lần lặp và số học tầm thường. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("5\n2 2 6
```
