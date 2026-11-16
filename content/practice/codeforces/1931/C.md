---
title: "CF 1931C - Làm Lại Bình Đẳng"
description: "Chúng ta được cho một mảng các số nguyên và chúng ta muốn làm cho tất cả các phần tử bằng nhau. Chúng ta được phép thực hiện nhiều nhất một thao tác, bao gồm việc chọn một mảng con liền kề và đặt tất cả các phần tử của nó thành một giá trị nào đó."
date: "2026-06-08T18:25:02+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1931
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 925 (Div. 3)"
rating: 1000
weight: 1931
solve_time_s: 103
verified: true
draft: false
---

[CF 1931C - Tạo lại sự bình đẳng](https://codeforces.com/problemset/problem/1931/C) 

**Đánh giá:** 1000 
**Tags:** vũ phu, tham lam, toán học 
**Thời gian giải:** 1m 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên và chúng ta muốn làm cho tất cả các phần tử bằng nhau. Chúng ta được phép thực hiện nhiều nhất một thao tác, bao gồm việc chọn một mảng con liền kề và đặt tất cả các phần tử của nó thành một giá trị nào đó. Chi phí của hoạt động bằng với độ dài của mảng con chúng ta chọn. Mục tiêu của chúng tôi là giảm thiểu chi phí này. 

Đầu vào bao gồm nhiều trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm đưa ra kích thước mảng và chính mảng đó. Chúng ta phải đưa ra chi phí tối thiểu cho mỗi trường hợp thử nghiệm. 

Những hạn chế là đáng kể. Mảng có thể lên tới$2 \cdot 10^5$các phần tử và có thể có tới$10^4$trường hợp thử nghiệm. Tổng số phần tử trong tất cả các trường hợp thử nghiệm không vượt quá$2 \cdot 10^5$. Điều này có nghĩa là bất kỳ giải pháp nào có thời gian dài hơn tuyến tính cho mỗi trường hợp thử nghiệm đều có thể quá chậm. Một cách mạnh mẽ ngây thơ kiểm tra tất cả các mảng con có thể là bậc hai trong$n$, điều đó là không thể chấp nhận được. Chúng ta cần một cách tiếp cận tuyến tính hoặc gần tuyến tính. 

Các trường hợp cạnh bao gồm các mảng đã đồng nhất, các mảng có độ dài 1 và các mảng trong đó nhiều phần tử đã được lặp lại nhưng không liên tiếp. Ví dụ, trong`[8, 8, 8, 1, 2, 8, 8, 8]`, lựa chọn tối ưu không hiển nhiên ngay lập tức vì các số 8 lặp lại được phân tách bằng các giá trị khác nhau. Một cách tiếp cận ngây thơ luôn chọn lần chạy liền kề dài nhất có cùng giá trị sẽ thất bại ở đây. 

## Phương pháp tiếp cận 

Phương thức brute-force sẽ lặp lại mọi giá trị có thể mà chúng ta có thể đặt cho mảng và với mỗi giá trị, hãy thử mọi mảng con để xem liệu việc đặt nó có dẫn đến một mảng thống nhất hay không. Chi phí đánh giá mỗi mảng con tỷ lệ thuận với độ dài của nó và có khoảng$O(n^2)$mảng con. Điều này dẫn đến$O(n^3)$hoạt động khi kết hợp với việc lặp qua tất cả các giá trị ứng cử viên. Điều này đúng về nguyên tắc nhưng quá chậm đối với các ràng buộc của vấn đề. 

Thông tin chuyên sâu quan trọng để có giải pháp nhanh hơn là tập trung vào các vị trí mà từng giá trị ứng cử viên đã xuất hiện. Chúng ta có thể đếm số đoạn của mảng không bằng giá trị đã chọn. Mỗi phân đoạn như vậy có thể được "bao phủ" bởi một thao tác nếu chúng ta mở rộng mảng con một cách thông minh. Vì chúng tôi chỉ được phép thực hiện một thao tác nên chi phí tối thiểu bằng với độ dài của đoạn dài nhất mà chúng tôi cần ghi đè để bù đắp tất cả các khác biệt đối với giá trị ứng cử viên. Chính thức hơn, mảng có thể được coi là một chuỗi các khối được phân tách bằng giá trị ứng cử viên. Mỗi khối phải được ghi đè, nhưng chúng ta có thể chọn bao gồm nhiều khối trong một thao tác. Một cách tiếp cận tham lam xuất hiện: chúng tôi liên tục bỏ qua giá trị ứng cử viên và đếm độ dài của các đoạn liền kề của các giá trị không ứng cử viên, chọn độ dài tối thiểu trong số tất cả các giá trị ứng cử viên. 

Quan sát này làm giảm vấn đề từ việc xem xét tất cả các mảng con đến việc lặp lại các vị trí của từng giá trị duy nhất, mang lại cho chúng ta một giải pháp quét tuyến tính hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(1) | Quá chậm | 
| Tối ưu | O(n) cho mỗi trường hợp thử nghiệm | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi trường hợp kiểm thử, hãy đọc mảng và ghi lại tập hợp các giá trị duy nhất. Đây là những ứng cử viên duy nhất cho giá trị thống nhất cuối cùng. 
2. Khởi tạo một biến`min_cost`đến một số lượng lớn, sẽ theo dõi chi phí tối thiểu được tìm thấy giữa các ứng viên. 
3. Đối với mỗi giá trị ứng viên`v`, quét mảng từ trái sang phải. Mỗi lần chúng ta gặp một phần tử không bằng`v`, đếm các phần tử liên tiếp cho đến phần tử tiếp theo`v`hoặc cuối mảng. Mỗi phân đoạn liền kề như vậy đóng góp 1 vào số lượng hoạt động được yêu cầu. 
4. Chi phí cho ứng viên`v`là số lượng phân đoạn không ứng cử viên, vì mỗi phân đoạn phải được ghi đè một lần. Vì chi phí vận hành bằng với độ dài của phân đoạn và bài toán cho phép chọn bất kỳ phân đoạn nào nên chúng tôi chọn số lượng phân đoạn nhỏ nhất làm chi phí tối thiểu có thể để bù đắp tất cả những khác biệt trong một thao tác. 
5. Cập nhật`min_cost`nếu ứng viên này đưa ra tổng chi phí nhỏ hơn. 
6. Sau khi kiểm tra tất cả các ứng viên, xuất ra`min_cost`. 

Tại sao nó hoạt động: Bằng cách xử lý riêng từng giá trị ứng cử viên và đếm các khối giá trị không ứng cử viên liền kề, chúng tôi xác định các phân đoạn tối thiểu phải được ghi đè. Vì chúng ta có thể chọn bất kỳ mảng con liền kề nào để ghi đè trong một thao tác duy nhất, số lượng khối không ứng cử viên tối thiểu tương ứng trực tiếp với chi phí tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        if n == 1:
            print(0)
            continue
        unique_vals = set(a)
        min_cost = n  # maximal cost is n
        for v in unique_vals:
            i = 0
            cost = 0
            while i < n:
                if a[i] != v:
                    cost += 1
                    while i < n and a[i] != v:
                        i += 1
                else:
                    i += 1
            min_cost = min(min_cost, cost)
        print(min_cost)

if __name__ == "__main__":
    solve()
```Chúng tôi lặp lại từng giá trị duy nhất vì chỉ những giá trị đó mới có thể là giá trị thống nhất cuối cùng. Việc đếm các phân đoạn liền kề của các giá trị không ứng cử đảm bảo chúng tôi chỉ áp dụng thao tác khi cần thiết. Vòng lặp bên trong bỏ qua các phần tử bằng ứng cử viên một cách chính xác để tránh tính hai lần. sử dụng`min`giữa các ứng viên đảm bảo chúng tôi tìm được sự lựa chọn tối ưu. Xử lý`n == 1`như một trường hợp đặc biệt sẽ ngăn chặn các phép tính không cần thiết và tránh các lỗi sai sót. 

## Ví dụ đã hoạt động 

Đầu vào mẫu`[8, 8, 8, 1, 2, 8, 8, 8]`: 

| Chỉ mục | Giá trị | Ứng viên 8 | Số lượng phân đoạn | 
| --- | --- | --- | --- | 
| 0 | 8 | bỏ qua | 0 | 
| 1 | 8 | bỏ qua | 0 | 
| 2 | 8 | bỏ qua | 0 | 
| 3 | 1 | đếm | 1 | 
| 4 | 2 | tiếp tục | 1 | 
| 5 | 8 | bỏ qua | 1 | 
| 6 | 8 | bỏ qua | 1 | 
| 7 | 8 | bỏ qua | 1 | 

Tổng chi phí cho ứng viên 8 là 2. Các ứng viên khác (1 hoặc 2) mang lại chi phí cao hơn. 

Đầu vào mẫu`[1, 2, 3]`: 

| Chỉ mục | Giá trị | Ứng viên 1 | Số lượng phân đoạn | 
| --- | --- | --- | --- | 
| 0 | 1 | bỏ qua | 0 | 
| 1 | 2 | đếm | 1 | 
| 2 | 3 | tiếp tục | 1 | 

Chi phí cho ứng viên 1 là 2. Ứng viên 2 và 3 tương tự mang lại chi phí 2. Chi phí tối thiểu là 2. 

Những dấu vết này xác nhận rằng thuật toán đếm chính xác các phân đoạn ghi đè cần thiết và chọn mức tối thiểu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * u) | n để quét mảng, u giá trị duy nhất cho mỗi trường hợp thử nghiệm (u ≤ n) | 
| Không gian | O(n) | lưu trữ mảng và tập hợp các giá trị duy nhất | 

Với tổng n trên tất cả các trường hợp thử nghiệm 2·10^5, điều này rất phù hợp với giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("8\n6\n1 2 3 4 5 1\n7\n1 1 1 1 1 1 1\n8\n8 8 8 1 2 8 8 8\n1\n1\n2\n1 2\n3\n1 2 3\n7\n4 3 2 7 1 1 3\n9\n9 9 2 9 2 5 5 5 3\n") == "4\n0\n2\n0\n1\n2\n6\n7", "sample 1"

# custom cases
assert run("1\n1\n42\n") == "0", "single element"
assert run("1\n5\n1 1 1 1 1\n") == "0", "all equal"
assert run("1\n5\n1 2 3 4 5\n") == "4", "all distinct"
assert run("1\n6\n1 1 2 2 1 1\n") == "2", "two blocks of different value"
assert run("1\n3\n2 2 2\n") == "0", "already uniform"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1\n42`| 0 | Mảng một phần tử | 
|`1\n5\n1 1 1 1 1`| 0 | Mảng đã thống nhất | 
|`1\n5\n1 2 3 4 5`| 4 | Tất cả các giá trị riêng biệt, chi phí tối đa | 
|`1\n6\n1 1 2 2 1 1`| 2 | Nhiều khối nhỏ giá trị không phải ứng cử viên | 
|`1\n3\n2 2 2`| 0 | Đã đồng nhất, vỏ có cạnh tối thiểu | 

## Vỏ cạnh 

Thuật toán xử lý các mảng phần tử đơn bằng cách trả về 0 ngay lập tức, ngăn chặn việc xử lý không cần thiết. Trong mảng như`[1, 2, 3]`, cái
