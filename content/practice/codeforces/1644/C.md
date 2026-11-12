---
title: "CF 1644C - Tăng tổng mảng con"
description: "Chúng ta được cho một mảng các số nguyên và một số $x$. Nhiệm vụ là tính toán, với mỗi $k$ có thể từ 0 đến $n$, tổng tối đa của một mảng con liền kề nếu chúng ta được phép thêm $x$ vào chính xác các phần tử $k$ trong mảng."
date: "2026-06-10T04:13:53+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dp", "greedy", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1644
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 123 (Rated for Div. 2)"
rating: 1400
weight: 1644
solve_time_s: 104
verified: true
draft: false
---

[CF 1644C - Tăng tổng mảng con](https://codeforces.com/problemset/problem/1644/C) 

**Đánh giá:** 1400 
**Tags:** bạo lực, dp, tham lam, thực hiện 
**Thời gian giải:** 1 phút 44s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên và một số$x$. Nhiệm vụ là tính toán cho mỗi khả năng$k$từ 0 đến$n$, tổng tối đa của một mảng con liền kề nếu chúng ta được phép thêm$x$chính xác$k$các phần tử trong mảng. Các phần tử cần thêm$x$to có thể được chọn tùy ý và bản thân mảng con không nhất thiết phải bao gồm tất cả các phần tử tăng lên. Một mảng con trống được cho phép, có tổng bằng 0. 

Các ràng buộc đủ nhỏ để$O(n^2)$giải pháp là khả thi. Tổng của$n$trên tất cả các trường hợp thử nghiệm tối đa là 5000, nghĩa là chúng tôi có thể thực hiện tổng cộng khoảng 25 triệu thao tác trong giới hạn thời gian 2 giây. Mỗi phần tử mảng và phần tăng dần$x$có thể lớn, vì vậy chúng ta phải tránh tràn số nguyên, nhưng Python xử lý việc đó một cách tự nhiên. 

Các trường hợp cạnh không rõ ràng bao gồm các mảng có tất cả các số âm, trong đó tổng tối đa có thể bằng 0 nếu không tồn tại mảng con tổng dương. Ví dụ, đối với mảng$[-5, -3, -2]$với$x = 4$, không chọn phần tử nào để tăng sẽ có tổng tối đa là 0, chọn một phần tử một cách khôn ngoan sẽ nhận được 2 và chọn cả ba sẽ nhận được 3. Một cách triển khai ngây thơ luôn chọn phần tử đầu tiên$k$các yếu tố có thể thất bại trong những trường hợp như vậy. 

Một trường hợp cạnh khác xảy ra khi$x = 0$. Sau đó, bài toán trở thành bài toán tổng mảng con tối đa cổ điển và thuật toán phải xử lý chính xác$k = 0$không giả định$x > 0$. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ là, đối với mỗi$k$, để tạo ra tất cả các tập hợp con có kích thước$k$để thêm$x$, sau đó với mỗi tập hợp con, hãy tính tổng mảng con tối đa. Tạo tất cả các tập hợp con là$O(\binom{n}{k})$và tính tổng mảng con tối đa là$O(n)$. Cách tiếp cận này đúng về nguyên tắc nhưng không khả thi vì$n$có thể lên tới 5000, vì vậy$\binom{5000}{k}$lớn về mặt thiên văn ngay cả đối với nhỏ$k$. 

Cái nhìn sâu sắc quan trọng là việc thêm$x$đối với các phần tử luôn có lợi, vì vậy chúng ta có thể tách rời việc lựa chọn mảng con và lựa chọn phần tử nào cần tăng. Đối với bất kỳ mảng con nào có độ dài$l$, lợi ích tối đa chúng ta có thể nhận được từ việc thêm$x$là$\min(l, k) \cdot x$, bởi vì chúng ta không thể tăng nhiều phần tử hơn số phần tử chứa trong mảng con. Do đó, chỉ cần tính toán trước, đối với mỗi độ dài mảng con, tổng mảng con tối đa của độ dài đó mà không tăng bất kỳ khoản nào. Sau đó với mỗi$k$, tổng lớn nhất là giá trị lớn nhất trên mọi độ dài$l$của$\text{max\_sum\_of\_length}[l] + \min(l, k) \cdot x$. 

Điều này làm giảm vấn đề từ cấp số nhân đến$O(n^2)$: tính tổng lớn nhất cho mọi độ dài$O(n^2)$và tính kết quả cho từng$k$mất$O(n^2)$tổng cộng trên tất cả các chiều dài. Với$n \le 5000$, điều này có thể chấp nhận được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * n) | O(n) | Quá chậm | 
| Tối ưu | O(n^2) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case và lặp lại từng test case. 
2. Đối với một mảng có độ dài nhất định$n$và tăng dần$x$, khởi tạo một mảng`max_sum_of_length`có độ dài (n + 1` để lưu trữ tổng tối đa cho các mảng con của mỗi độ dài. 
3. Đối với mỗi chỉ số bắt đầu$i$trong mảng, khởi tạo một tổng chạy`current_sum = 0`. Lặp lại chỉ mục kết thúc$j$từ$i$ĐẾN$n-1$. Tăng`current_sum`qua`a[j]`và cập nhật`max_sum_of_length[j-i+1]`với giá trị tối đa hiện tại và`current_sum`. Điều này nắm bắt tổng tối đa cho mỗi độ dài mảng con. 
4. Khởi tạo mảng đáp án`answer`chiều dài$n + 1$với số không. Cửa hàng này$f(k)$cho mỗi$k$. 
5. Với mỗi k có thể` from 0 to \(n, iterate over all subarray lengths `tôi` from 0 to ⟦PROTECT_46⟧ and update `câu trả lời[k]` as the maximum of its current value and `max_sum_of_length[l] + min(l, k) * x`. Điều này tính toán tổng tối ưu cho mỗi số phần tử tăng lên. 
6. In`answer`cho từng trường hợp thử nghiệm. 

Tại sao nó hoạt động: Bằng cách tính toán trước tổng số mảng con tối đa cho mỗi độ dài, chúng tôi đảm bảo rằng đối với bất kỳ lựa chọn nào về$k$, chúng ta có thể xem xét mảng con tối ưu cho mọi số phần tử có thể tăng lên. chức năng$\min(l, k) \cdot x$mô hình chính xác giới hạn mà chúng ta không thể tăng nhiều phần tử hơn số phần tử chứa trong mảng con. Việc lặp lại trên mọi độ dài đảm bảo rằng chúng ta không bao giờ bỏ lỡ mức tối đa toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, x = map(int, input().split())
    a = list(map(int, input().split()))

    max_sum_of_length = [0] * (n + 1)

    for i in range(n):
        current_sum = 0
        for j in range(i, n):
            current_sum += a[j]
            length = j - i + 1
            if current_sum > max_sum_of_length[length]:
                max_sum_of_length[length] = current_sum

    answer = [0] * (n + 1)
    for k in range(n + 1):
        best = 0
        for length in range(n + 1):
            best = max(best, max_sum_of_length[length] + min(length, k) * x)
        answer[k] = best

    print(*answer)
```Giải pháp đầu tiên xây dựng`max_sum_of_length`, đảm bảo rằng mọi tổng mảng con có thể đều được xem xét. Trong vòng lặp thứ hai,`min(length, k)`đảm bảo chúng tôi không bao giờ đếm quá số lượng phần tử mà chúng tôi có thể tăng lên. Việc sử dụng số 0 làm giá trị ban đầu đảm bảo rằng các mảng con trống được tính chính xác, tạo ra kết quả bằng 0 khi tất cả các số đều âm. 

## Ví dụ đã hoạt động 

**Mẫu 1:** 

đầu vào:`4 2`và mảng`[4, 1, 3, 2]`. 

| k | tối đa trên chiều dài l | l=1 | l=2 | l=3 | l=4 | 
| --- | --- | --- | --- | --- | --- | 
| 0 | tối đa(4,5,8,10) | 4 | 5 | 8 | 10 | 
| 1 | tối đa(6,7,10,12) | 6 | 7 | 10 | 12 | 
| 2 | tối đa(8,9,12,14) | 8 | 9 | 12 | 14 | 
| 3 | tối đa(10,11,14,16) | 10 | 11 | 14 | 16 | 
| 4 | tối đa(12,13,16,18) | 12 | 13 | 16 | 18 | 

Dấu vết này cho thấy cách thuật toán kết hợp các tổng mảng con với`k*x`để đạt được mức tối đa cho mỗi`k`. 

**Mẫu 2:** 

đầu vào:`3 5`và mảng`[-2, -7, -1]`. 

| k | tổng mảng con tốt nhất + min(length, k)*x | 
| --- | --- | 
| 0 | 0 | 
| 1 | tối đa(-2+5, -7+5, -1+5) = 4 | 
| 2 | tối đa(-2+10, -7+10, -1+10, (-2+-7)+10, ...) = 4 | 
| 3 | tổng([-2,-7,-1])+3*5 = 5 | 

Điều này thể hiện việc xử lý các số âm, mảng con trống và độ dài k > mảng con. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Vòng lặp bên ngoài trên chỉ mục bắt đầu, vòng lặp bên trong trên chỉ mục kết thúc tính toán tất cả các tổng của mảng con | 
| Không gian | O(n) | Mảng`max_sum_of_length`Và`answer`| 

Được cho$n \le 5000$và tổng của$n$trên tất cả các trường hợp thử nghiệm$\le 5000$,$O(n^2)$được chấp nhận trong vòng 2 giây. Mức sử dụng bộ nhớ thấp hơn nhiều so với giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    # invoke solution
    t = int(input())
    for _ in range(t):
        n, x = map(int, input().split())
        a = list(map(int, input().split()))
        max_sum_of_length = [0] * (n + 1)
        for i in range(n):
            s = 0
            for j in range(i, n):
                s += a[j]
                max_sum_of_length[j-i+1] = max(max_sum_of_length[j-i+1], s)
        ans = [0]*(n+1)
        for k in range(n+1
```
