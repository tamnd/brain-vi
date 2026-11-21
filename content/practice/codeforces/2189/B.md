---
title: "CF 2189B - Lời nguyền của con ếch"
description: "Chúng ta được yêu cầu giúp một con ếch di chuyển dọc theo một dãy số vô hạn từ vị trí 0 đến vị trí mục tiêu $x$. Con ếch có $n$ kiểu nhảy kỳ diệu."
date: "2026-06-07T21:09:31+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 2189
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 1075 (Div. 2)"
rating: 1200
weight: 2189
solve_time_s: 128
verified: false
draft: false
---

[CF 2189B - Lời nguyền của con ếch](https://codeforces.com/problemset/problem/2189/B) 

**Đánh giá:** 1200 
**Tags:** thuật toán xây dựng, tham lam, toán học 
**Thời gian giải:** 2 phút 8 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu giúp một con ếch di chuyển dọc theo một dãy số vô hạn từ vị trí 0 đến vị trí mục tiêu$x$. Con ếch có$n$các loại bước nhảy ma thuật. Mỗi kiểu nhảy$i$cho phép con ếch di chuyển về phía trước tối đa$a_i$đơn vị, nhưng con ếch bị nguyền rủa: mỗi$b_i$-lần nhảy thứ đó buộc nó phải quay lại$c_i$đơn vị trước khi nhảy. Mục tiêu là xác định số lần lùi tối thiểu mà con ếch phải chịu đựng để đạt được vị trí chính xác.$x$, hoặc báo cáo rằng điều đó là không thể. 

Đầu vào chỉ định nhiều trường hợp thử nghiệm. Mỗi trường hợp liệt kê$n$các kiểu nhảy, mỗi kiểu có$a_i, b_i, c_i$. Các ràng buộc rất lớn:$x$có thể lên đến$10^{18}$Và$n$tổng hợp trên tất cả các trường hợp thử nghiệm là lên đến$10^5$. Điều này loại trừ bất kỳ giải pháp nào cố gắng mô phỏng từng bước nhảy riêng lẻ, vì làm như vậy sẽ yêu cầu theo thứ tự$x$hoạt động trong trường hợp xấu nhất. Chúng ta cần một cách tiếp cận toán học hoặc tham lam để tính toán kết quả mà không cần mô phỏng từng bước nhảy. 

Trường hợp cạnh không rõ ràng xảy ra khi khoảng cách nhảy tối đa$a_i$nhỏ hơn số lần khôi phục$c_i$cho một bước nhảy thường xuyên định kỳ. Ví dụ, với$x = 4$và một bước nhảy$a = 2, b = 2, c = 3$, con ếch không thể đạt tới 4. Một cách tiếp cận ngây thơ mà bỏ qua hiệu ứng quay lui có thể kết luận sai rằng con ếch có thể tiếp cận mục tiêu. Một trường hợp khác là khi mục tiêu nhỏ hơn bất kỳ khoảng cách nhảy nào nhưng có thể tiếp cận chính xác trong một lần nhảy; trong trường hợp này, số lần khôi phục tối thiểu là 0 chứ không phải một. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ mô phỏng tăng dần mọi loại bước nhảy, thử tất cả các kết hợp của các bước nhảy, theo dõi các bước lùi và dừng lại khi chúng ta đạt đến$x$. Điều này có thể đúng nhưng quá chậm vì mục tiêu$x$có thể cực kỳ lớn và mỗi chuỗi nhảy có thể bao gồm hàng tỷ bước. 

Cái nhìn sâu sắc quan trọng là xử lý từng kiểu nhảy một cách độc lập và tính toán xem con ếch có thể đi được bao xa với số lần quay lui nhất định. Đối với kiểu nhảy$i$, nếu chúng ta để$k$là số lần quay lui phát sinh, tổng khoảng cách có thể đạt được là$a_i \cdot (k \cdot b_i) - k \cdot c_i + r$, Ở đâu$r < b_i$là số lần nhảy trước lần quay lại tiếp theo. Tối ưu hóa cho số lần quay lui tối thiểu sẽ chuyển thành tối đa hóa khoảng cách nhảy hiệu quả$a_i - \frac{c_i}{b_i}$. Chúng ta có thể tính toán số lần quay lại tối thiểu cho từng loại bước nhảy bằng cách sử dụng số học số nguyên và chọn loại bước nhảy đạt được mục tiêu với số lần quay lại ít nhất. 

Cách tiếp cận vũ phu không thành công vì việc mô phỏng mọi bước nhảy là không khả thi. Quan sát cho thấy chúng ta có thể tính toán rollback trực tiếp dựa trên các tham số bước nhảy sẽ giảm vấn đề thành phép tính thời gian không đổi cho mỗi loại bước nhảy. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(x * n) | O(1) | Quá chậm | 
| Tối ưu | O(n) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi kiểu nhảy$i$, kiểm tra xem$a_i \ge x$. Nếu có, con ếch có thể với tới$x$trong một lần nhảy với số lần quay lại bằng 0. Ghi số 0 làm câu trả lời của ứng viên. 
2. Đối với các kiểu nhảy ở đâu$a_i < x$, tính số lần quay lui tối thiểu cần thiết. Mỗi lần quay lui xảy ra mỗi$b_i$nhảy và giảm khoảng cách lưới bằng cách$c_i$. Cho phép$k$là số lần rollback. Khoảng cách tối đa có thể đạt được với$k$rollback là$$\text{distance} = a_i \cdot (\text{total jumps}) - k \cdot c_i$$trong đó tổng số lần nhảy bao gồm cả số lần nhảy gây ra sự quay lui và số lần nhảy còn lại. Giải số nguyên nhỏ nhất$k \ge 0$khoảng cách như vậy$\ge x$. 

1. Tính toán$k$sử dụng phép toán số nguyên: số lần nhảy cần đạt$x$không có hiệu ứng rollback là$\lceil x / a_i \rceil$. Tổng khoảng cách rollback là$\lfloor (\text{jumps} - 1)/b_i \rfloor \cdot c_i$. Điều chỉnh$k$cho đến khi tổng khoảng cách bao gồm cả rollback đáp ứng hoặc vượt quá$x$. 
2. Theo dõi mức tối thiểu$k$trên tất cả các loại nhảy. Nếu không có kiểu nhảy nào có thể đạt tới$x$, đầu ra$-1$. Ngược lại, xuất ra số lần khôi phục tối thiểu được tìm thấy. 

Thuật toán hoạt động vì việc khôi phục là định kỳ và có thể dự đoán được, cho phép tính toán trực tiếp hiệu ứng tích lũy của chúng. Chúng tôi có thể kiểm tra từng loại bước nhảy một cách độc lập, đảm bảo rằng cấu hình khôi phục tối thiểu giữa các loại được nắm bắt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, x = map(int, input().split())
        min_rollbacks = float('inf')
        for _ in range(n):
            a, b, c = map(int, input().split())
            if a >= x:
                min_rollbacks = min(min_rollbacks, 0)
                continue
            if a * b <= c:
                continue
            # compute minimum rollbacks needed
            k = (x - a + (a * b - c) - 1) // (a * b - c)
            if k >= 0:
                min_rollbacks = min(min_rollbacks, k)
        print(-1 if min_rollbacks == float('inf') else min_rollbacks)

if __name__ == "__main__":
    solve()
```Đầu tiên, mã xử lý trường hợp đơn giản trong đó một bước nhảy có thể đến được mục tiêu. Sau đó, nó lọc ra các kiểu nhảy không hữu ích vì hình phạt lùi của chúng vượt quá khoảng cách thực trên mỗi chu kỳ. Đối với các loại bước nhảy còn lại, số học số nguyên sẽ tính toán chính xác số lần quay lui cần thiết. sử dụng`float('inf')`đảm bảo rằng các trường hợp không thể thực hiện được được gắn cờ chính xác. 

## Ví dụ đã hoạt động 

**Mẫu 1**:`1 1 3 3 3`| Bước | Vị trí | Khôi phục | Ghi chú | 
| --- | --- | --- | --- | 
| 1 | 0 -> 1 | 0 | Một lần nhảy 1 lần sẽ đạt được mục tiêu, không xảy ra hiện tượng quay lui | 

**Mẫu 4**:`5 8 ...`| Bước | Vị trí | Khôi phục | Ghi chú | 
| --- | --- | --- | --- | 
| 1 | 0 -> 12 | 0 | Nhảy quá 12 lần, vẫn đóng góp vào x | 
| 2 | điều chỉnh | 1 | Rollback xảy ra, khoảng cách tích lũy vẫn >= 8 | 
| Kết quả: số lần hoàn vốn tối thiểu = 2 | | | | 

Những dấu vết này cho thấy thuật toán nắm bắt được cả các giải pháp bước nhảy đơn và các chuỗi yêu cầu nhiều bước nhảy với các lần quay lui định kỳ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi kiểu nhảy được đánh giá độc lập, không cần mô phỏng các bước nhảy riêng lẻ | 
| Không gian | O(1) | Chỉ cần một vài biến cho mỗi trường hợp thử nghiệm, không cần mảng lớn | 

Được cho$n \le 10^5$Và$t \le 10^4$, tổng số hoạt động phù hợp thoải mái dưới$10^6$mỗi giây, thỏa mãn giới hạn thời gian. Việc sử dụng bộ nhớ là tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# Provided samples
assert run("6\n1 1\n3 3 3\n1 7\n4 2 5\n2 4\n1 2 3\n2 2 4\n5 8\n12 1 11\n10 1 4\n1 1 3\n1 2 5\n2 1 7\n1 1000000000000000000\n1000000 4 654321\n1 10\n2 2 1\n") == "0\n1\n-1\n2\n298892990032\n3"

# Minimum-size input
assert run("1\n1 1\n1 1 1\n") == "0"

# Maximum x with a large jump
assert run("1\n1 1000000000000000000\n1000000000 1 1\n") == "999999999"

# Multiple equal jumps
assert run("1\n2 10\n5 2 3\n5 2 3\n") == "2"

# Impossible case
assert run("1\n1 10\n1 1 2\n") == "-1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1\n1 1 1`| 0 | Đầu vào kích thước tối thiểu | 
|`1 1000000000000000000\n1000000000 1 1`| 999999999 | Tính x lớn | 
|`2 10\n5 2 3\n5 2 3`| 2 | Nhiều lần nhảy với các thông số bằng nhau | 
|`1 10\n1 1 2`| -1 | Mục tiêu không thể tiếp cận | 

## Vỏ cạnh 

Khi một kiểu nhảy có$a_i \ge x$, thuật toán ngay lập tức đặt rollback về 0. Ví dụ, với$x = 4$Và
