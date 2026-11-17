---
title: "CF 1978B - Tiệm Bánh Mới"
description: "Trong vấn đề này, Bob đang cố gắng tối đa hóa lợi nhuận từ việc bán bánh bằng một chương trình khuyến mãi đặc biệt. Anh ta có n cái bánh để bán, giá thông thường là a cho mỗi cái bánh, và thông số khuyến mãi b."
date: "2026-06-08T17:10:27+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "greedy", "math", "ternary-search"]
categories: ["algorithms"]
codeforces_contest: 1978
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 953 (Div. 2)"
rating: 800
weight: 1978
solve_time_s: 127
verified: true
draft: false
---

[CF 1978B - Tiệm bánh mới](https://codeforces.com/problemset/problem/1978/B) 

**Đánh giá:** 800 
**Tags:** tìm kiếm nhị phân, tham lam, toán học, tìm kiếm bậc ba 
**Thời gian giải:** 2m 7s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Trong vấn đề này, Bob đang cố gắng tối đa hóa lợi nhuận từ việc bán bánh bằng một chương trình khuyến mãi đặc biệt. Anh ấy có`n`bán bánh bao, giá bình thường`a`cho mỗi chiếc bánh và một thông số khuyến mại`b`. Khuyến mãi cho phép anh ta chọn một số nguyên`k`và bán cái đầu tiên`k`bánh bao với giá giảm từ`b`xuống tới`b - k + 1`. Số bánh còn lại nếu có sẽ bán theo giá thông thường`a`. Nhiệm vụ là xác định tổng lợi nhuận tối đa mà Bob có thể đạt được bằng cách chọn mức tối ưu.`k`. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm đưa ra`n`,`a`, Và`b`. Đầu ra là lợi nhuận tối đa cho mỗi trường hợp. Từ`n`,`a`, Và`b`có thể lớn như`10^9`và có tới`10^4`trường hợp thử nghiệm, bất kỳ giải pháp nào lặp lại trên từng bánh đều quá chậm. Chúng tôi cần một công thức thời gian không đổi cho mỗi trường hợp thử nghiệm để duy trì trong giới hạn thời gian. 

Một trường hợp cạnh tinh tế phát sinh khi`b`rất nhỏ hoặc rất lớn so với`a`Và`n`. Ví dụ, nếu`b <= a`, bán bánh với giá khuyến mại sẽ không tăng lợi nhuận. Ngược lại, nếu`b`là rất lớn, có thể sẽ tối ưu nếu bán được số lượng bánh bao mà chương trình khuyến mại cho phép. Giải pháp ngây thơ thử tất cả các giá trị của`k`sẽ hết thời gian chờ, vì vậy cần phải tính toán trực tiếp. 

## Phương pháp tiếp cận 

Cách tiếp cận ngây thơ lặp đi lặp lại`k`từ 0 đến`min(n, b)`, tính tổng của số đầu tiên`k`giảm giá, thêm`(n - k) * a`, và theo dõi mức tối đa. Điều này đúng nhưng đòi hỏi tới`10^9`hoạt động cho mỗi trường hợp thử nghiệm trong trường hợp xấu nhất là không khả thi. 

Cách tiếp cận tối ưu xuất phát từ quan sát rằng hàm tổng lợi nhuận xét theo`k`là bậc hai và lõm. Lợi nhuận cho`k`bánh được bán với giá khuyến mãi là`sum_{i=1}^k (b - i + 1) = k * b - k*(k-1)/2`. Những chiếc bánh còn lại cho`(n - k) * a`. Kết hợp những điều này, hàm tổng lợi nhuận là`profit(k) = k*b - k*(k-1)/2 + (n - k)*a`. Đây là hàm bậc hai lõm trong`k`, do đó cực đại xảy ra ở biên hoặc ở đỉnh. Đỉnh là tại`k = b - a`, Nhưng`k`bị giới hạn ở`[0, min(n, b)]`. Vì vậy, tối ưu`k`là`max(0, min(b - a, min(n, b)))`. 

Điều này làm giảm vấn đề xuống còn một phép tính duy nhất cho mỗi trường hợp thử nghiệm mà không có vòng lặp. Công thức số học có thể được đánh giá trực tiếp trong thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force (lặp k) | O(n) | O(1) | Quá chậm | 
| Công thức tính trực tiếp | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Với mỗi test case, hãy đọc`n`,`a`, Và`b`. 
3. Tính số lượng bánh khuyến mãi ứng viên`k`BẰNG`b - a`. Điều này xuất phát từ việc phân biệt hàm lợi nhuận bậc hai; nó là tối ưu không bị ràng buộc. 
4. Kẹp`k`trong phạm vi khả thi`[0, min(n, b)]`sử dụng`max(0, min(k, min(n, b)))`. 
5. Tính lợi nhuận lần đầu`k`bánh bao là tổng của giá giảm dần:`k * b - k * (k - 1) // 2`. 
6. Tính lợi nhuận từ số tiền còn lại`n - k`bánh bao ở mức giá`a`:`(n - k) * a`. 
7. Cộng hai thành phần lại để có được tổng lợi nhuận. 
8. In tổng lợi nhuận. 

Tại sao nó hoạt động: Hàm lợi nhuận xét theo`k`là một hàm bậc hai lõm nên cực đại xảy ra ở đỉnh hoặc ở các biên. Kẹp`k`đảm bảo chúng tôi tôn trọng số lượng bánh và giới hạn khuyến mãi. Việc tính tổng thông qua công thức sẽ tránh được vòng lặp và đảm bảo tính toán O(1) cho mỗi trường hợp kiểm thử. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    results = []

    for _ in range(t):
        n, a, b = map(int, input().split())
        k = b - a
        k = max(0, min(k, n, b))
        profit = k * b - k * (k - 1) // 2 + (n - k) * a
        results.append(str(profit))

    sys.stdout.write("\n".join(results) + "\n")

solve()
```### Giải thích 

Mã đọc tất cả các trường hợp thử nghiệm một cách hiệu quả bằng cách sử dụng`sys.stdin.readline`. Đối với mỗi trường hợp, nó tính toán số lượng bánh khuyến mại tối ưu bằng công thức dẫn xuất. Việc kẹp đảm bảo rằng`k`tôn trọng mọi ràng buộc. Tổng của số đầu tiên`k`giá giảm được tính bằng công thức chuỗi số học. Số bánh còn lại được nhân với giá thông thường. Kết quả được thu thập và in ở cuối để đạt hiệu quả. 

## Ví dụ đã hoạt động 

| Trường hợp thử nghiệm | n | một | b | k | Tính toán lợi nhuận | Tổng lợi nhuận | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 4 | 4 | 5 | 1 | 1_5 - 0 + 3_4 = 5 + 12 | 17 | 
| 2 | 5 | 5 | 9 | 4 | 4_9 - 6 + 1_5 = 36 - 6 + 5 | 35 | 
| 3 | 10 | 10 | 5 | 0 | 0 + 10*10 | 100 | 

Các bảng cho thấy rằng với mỗi đầu vào, việc tính toán`k`BẰNG`b - a`và việc kẹp nó mang lại lợi nhuận tối đa và tổng số học sẽ tính toán tổng một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) cho mỗi trường hợp thử nghiệm | Chỉ một vài phép tính số học cho mỗi trường hợp thử nghiệm | 
| Không gian | O(t) | Lưu trữ chuỗi đầu ra cho tất cả các trường hợp thử nghiệm | 

Giải pháp này phù hợp một cách thoải mái với các ràng buộc lên tới 10^4 trường hợp thử nghiệm và giới hạn thời gian 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    solve()
    return sys.stdout.getvalue().strip()

# Provided samples
assert run("7\n4 4 5\n5 5 9\n10 10 5\n5 5 11\n1000000000 1000000000 1000000000\n1000000000 1000000000 1\n1000 1 1000\n") == \
       "17\n35\n100\n45\n1000000000000000000\n1000000000000000000\n500500", "sample 1"

# Custom tests
assert run("1\n1 5 5\n") == "5", "single bun edge"
assert run("1\n10 1 10\n") == "55", "all buns promotional"
assert run("1\n10 10 1\n") == "100", "promotion worse than normal"
assert run("1\n1000000000 1 1000000000\n") == "500000000500000000", "large numbers"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 5 5 | 5 | Mép búi đơn | 
| 10 1 10 | 55 | Bán tất cả các loại bánh đang khuyến mãi | 
| 10 10 1 | 100 | Khuyến mãi tệ hơn giá bình thường | 
| 1e9 1 1e9 | 500000000500000000 | Tính đúng số học số lớn | 

## Vỏ cạnh 

Khi nào`b <= a`, được tính toán`k`trở nên tiêu cực. Việc kẹp về 0 đảm bảo rằng không có bánh nào được bán theo chương trình khuyến mãi, điều này là tối ưu. Ví dụ,`n=10, a=10, b=5`kết quả là`k = max(0, min(-5, 10, 5)) = 0`. Lợi nhuận là`10*10 = 100`, phù hợp với kết quả mong đợi. Tương tự, khi`b`vượt quá`n`, kẹp đảm bảo`k <= n`để tránh làm quá số lượng bánh. Điều này đảm bảo tính đúng đắn cho tất cả các điều kiện biên.
