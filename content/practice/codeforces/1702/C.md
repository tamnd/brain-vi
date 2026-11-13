---
title: "CF 1702C - Đào tạo và truy vấn"
description: "Chúng ta được cung cấp một tuyến tàu dọc theo các ga có chỉ số rất lớn, từ 1 đến $10^9$. Tàu di chuyển theo thứ tự chính xác của danh sách $u1, u2, dots, un$ và có thể quay lại các ga nhiều lần."
date: "2026-06-09T21:41:54+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1702
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 805 (Div. 3)"
rating: 1100
weight: 1702
solve_time_s: 157
verified: true
draft: false
---

[CF 1702C - Đào tạo và truy vấn](https://codeforces.com/problemset/problem/1702/C) 

**Đánh giá:** 1100 
**Tas:** cấu trúc dữ liệu, tham lam 
**Thời gian giải:** 2m 37s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tuyến tàu dọc theo các ga có chỉ số rất lớn, từ 1 đến$10^9$. Tàu đi theo đúng thứ tự của danh sách$u_1, u_2, \dots, u_n$và có thể xem lại các trạm nhiều lần. Đối với mỗi truy vấn bao gồm hai trạm$a_j$Và$b_j$, chúng ta cần xác định xem đoàn tàu có thể chở một hành khách từ$a_j$ĐẾN$b_j$dọc theo hướng phía trước của tuyến đường. 

Thách thức đến từ phạm vi rộng lớn của các chỉ số trạm kết hợp với nhiều truy vấn tiềm năng. Các ràng buộc cho phép lên đến$2 \cdot 10^5$các trạm và truy vấn cho mỗi trường hợp thử nghiệm và tổng của tất cả các trường hợp thử nghiệm cũng bị giới hạn tương tự. Điều này loại trừ bất kỳ thuật toán nào lặp lại tuyến đường cho mỗi truy vấn, vì thuật toán đó có thể đạt tới$2 \cdot 10^5 \times 2 \cdot 10^5 = 4 \cdot 10^{10}$hoạt động, quá lớn cho giới hạn 3 giây. 

Một sự tinh tế phát sinh với các trạm lặp đi lặp lại. Nếu như$u = [1, 2, 1]$và truy vấn từ 2 đến 1, cách tiếp cận ngây thơ “lần xuất hiện đầu tiên đến lần xuất hiện cuối cùng” có thể thất bại nếu bỏ qua thực tế là 1 xảy ra cả trước và sau 2. Giải pháp đúng phải tính đến lần xuất hiện đầu tiên của trạm bắt đầu và lần xuất hiện cuối cùng của trạm cuối để tôn trọng hướng chuyển tiếp. 

Một trường hợp đặc biệt khác là khi trạm bắt đầu hoặc trạm kết thúc không nằm trên tuyến đường. Nếu như$a_j$hoặc$b_j$không xuất hiện trong lộ trình, câu trả lời tự động là KHÔNG. 

## Phương pháp tiếp cận 

Phương pháp brute-force rất đơn giản: với mỗi truy vấn, hãy quét qua tuyến đường tàu từ trái sang phải để xem liệu bạn có thể tiếp cận được không$b_j$sau khi gặp phải$a_j$. Điều này hoạt động vì tuyến đường được xác định rõ ràng nhưng không thành công đối với đầu vào lớn. Đối với trường hợp xấu nhất của$n = k = 2 \cdot 10^5$, điều này đòi hỏi lên đến$O(n \cdot k) = 4 \cdot 10^{10}$hoạt động, điều đó là không thể thực hiện được. 

Cách tiếp cận tối ưu sử dụng thông tin chi tiết trước khi tính toán. Đối với mỗi chỉ mục trạm trong tuyến, chúng tôi lưu trữ lần đầu tiên nó xuất hiện và lần cuối cùng nó xuất hiện. Sau đó, đối với một truy vấn từ$a_j$ĐẾN$b_j$, câu trả lời là CÓ nếu lần xuất hiện đầu tiên của$a_j$là trước lần xuất hiện cuối cùng của$b_j$. Điều này có tác dụng vì tàu chỉ di chuyển về phía trước; cơ hội đầu tiên lên tàu$a_j$phải đến trước cơ hội cuối cùng để tiếp cận$b_j$. 

Điều này làm giảm mỗi truy vấn xuống$O(1)$tra cứu và xây dựng từ điển xuất hiện đầu tiên/cuối cùng là$O(n)$. Do đó, tổng độ phức tạp cho mỗi trường hợp thử nghiệm trở thành$O(n + k)$, phù hợp thoải mái trong thời gian giới hạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n*k) | O(1) | Quá chậm | 
| Tối ưu | O(n + k) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. Đối với mỗi trường hợp thử nghiệm, hãy đọc$n$Và$k$, độ dài tuyến đường và số lượng truy vấn. 
2. Đọc lộ trình tàu$u$. Khởi tạo hai từ điển:`first_occurrence`Và`last_occurrence`. Lặp lại qua$u$với chỉ mục$i$. Đối với mỗi trạm$x = u[i]$, nếu như$x$vẫn chưa vào`first_occurrence`, đặt nó thành$i$. Luôn cập nhật`last_occurrence[x] = i\`. 
3. Đối với mỗi truy vấn$a_j, b_j$, kiểm tra xem cả hai trạm có tồn tại trên tuyến không. Nếu thiếu một trong hai, hãy in NO. Mặt khác, so sánh các chỉ số: if`first_occurrence[a_j] <= last_occurrence[b_j]`, in CÓ; nếu không thì in ra NO. 

Tại sao nó hoạt động: thuật toán duy trì tính bất biến`first_occurrence[x]`là điểm sớm nhất mà bạn có thể lên ga$x$Và`last_occurrence[y]`là điểm muộn nhất bạn có thể xuống tại ga$y$. Nếu cơ hội đầu tiên lên tàu$a_j$đến sau cơ hội cuối cùng để tiếp cận$b_j$, đoàn tàu không thể chở một hành khách từ$a_j$ĐẾN$b_j$theo hướng phía trước. Nếu không thì có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    input()  # blank line
    n, k = map(int, input().split())
    u = list(map(int, input().split()))
    
    first_occurrence = {}
    last_occurrence = {}
    
    for i, station in enumerate(u):
        if station not in first_occurrence:
            first_occurrence[station] = i
        last_occurrence[station] = i
    
    for _ in range(k):
        a, b = map(int, input().split())
        if a not in first_occurrence or b not in last_occurrence:
            print("NO")
        elif first_occurrence[a] <= last_occurrence[b]:
            print("YES")
        else:
            print("NO")
```Giải pháp đầu tiên tính toán trước lần xuất hiện đầu tiên và cuối cùng cho mỗi trạm. Điều này tránh việc quét tuyến đường nhiều lần cho mỗi truy vấn. Việc kiểm tra sự tồn tại trong từ điển cũng xử lý trường hợp trạm không xuất hiện trên tuyến. 

## Ví dụ đã hoạt động 

**Trường hợp thử nghiệm 1:** 

Tuyến đường:`[3, 7, 1, 5, 1, 4]`Truy vấn:`(3, 5), (1, 7), (3, 10)`| Trạm | Đầu tiên | Cuối cùng | 
| --- | --- | --- | 
| 3 | 0 | 0 | 
| 7 | 1 | 1 | 
| 1 | 2 | 4 | 
| 5 | 3 | 3 | 
| 4 | 5 | 5 | 

Truy vấn 1:`3 -> 5`→ first_occ[3] = 0, Last_occ[5] = 3 → CÓ 

Truy vấn 2:`1 -> 7`→ first_occ[1] = 2, Last_occ[7] = 1 → KHÔNG 

Truy vấn 3:`3 -> 10`→ 10 không có trong tuyến → KHÔNG 

**Trường hợp thử nghiệm 2:** 

Tuyến đường:`[1, 2, 1]`Truy vấn:`(2, 1), (1, 2), (4, 5)`| Trạm | Đầu tiên | Cuối cùng | 
| --- | --- | --- | 
| 1 | 0 | 2 | 
| 2 | 1 | 1 | 

Truy vấn 1:`2 -> 1`→ first_occ[2] = 1, Last_occ[1] = 2 → CÓ 

Truy vấn 2:`1 -> 2`→ first_occ[1] = 0, Last_occ[2] = 1 → CÓ 

Truy vấn 3:`4 -> 5`→ thiếu 4 → KHÔNG 

Điều này cho thấy các trạm lặp lại được xử lý chính xác và các truy vấn về các trạm bị thiếu sẽ bị từ chối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + k) cho mỗi trường hợp thử nghiệm | Xây dựng bản đồ xuất hiện đầu tiên/cuối cùng mất O(n), trả lời k truy vấn là O(k) | 
| Không gian | O(n) | Chúng tôi lưu trữ lần xuất hiện đầu tiên và cuối cùng cho từng trạm duy nhất trong tuyến | 

Cho rằng tổng của n và k trên tất cả các trường hợp thử nghiệm không vượt quá$2 \cdot 10^5$, giải pháp dễ dàng chạy dưới 3 giây với chi phí bộ nhớ thấp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    exec(open("solution.py").read(), globals())
    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# Provided sample
assert run("3\n\n6 3\n3 7 1 5 1 4\n3 5\n1 7\n3 10\n\n3 3\n1 2 1\n2 1\n1 2\n4 5\n\n7 5\n2 1 1 1 2 4 4\n1 3\n1 4\n2 1\n4 1\n1 2") == \
"""YES
NO
NO
YES
YES
NO
NO
YES
YES
NO
YES"""

# Minimum size input
assert run("1\n\n1 1\n1\n1 1") == "YES"

# All equal stations
assert run("1\n\n4 2\n2 2 2 2\n2 2\n2 3") == "YES\nNO"

# Maximum k with repeated first/last
assert run("1\n\n5 3\n1 2 3 2 1\n1 3\n2 2\n3 1") == "YES\nYES\nNO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n\n1 1\n1\n1 1`| CÓ | Trường hợp cạnh đầu vào tối thiểu | 
| `1\n\n4 2\n2 2 2 2\n2 2\n2 | | |
