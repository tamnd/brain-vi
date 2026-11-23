---
title: "CF 430A - Điểm và đoạn (dễ)"
description: "Chúng ta được yêu cầu tô màu một tập hợp các điểm trên trục số bằng hai màu đỏ và xanh lam, sao cho đối với mỗi đoạn cho trước, số điểm đỏ và xanh bên trong đoạn đó khác nhau tối đa một."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "sortings"]
categories: ["algorithms"]
codeforces_contest: 430
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 245 (Div. 2)"
rating: 1600
weight: 430
solve_time_s: 94
verified: false
draft: false
---

[CF 430A - Điểm và Đoạn (dễ)](https://codeforces.com/problemset/problem/430/A) 

**Đánh giá:** 1600 
**Tags:** thuật toán xây dựng, sắp xếp 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu tô màu một tập hợp các điểm trên trục số bằng hai màu đỏ và xanh lam, sao cho đối với mỗi đoạn cho trước, số điểm đỏ và xanh bên trong đoạn đó khác nhau tối đa một. Nói cách khác, mọi phân đoạn phải gần như cân bằng, với cùng số điểm của mỗi màu hoặc một màu vượt trội hơn một màu. Đầu vào cung cấp tọa độ của các điểm và điểm cuối của mỗi đoạn. Đầu ra phải gán màu cho từng điểm, được mã hóa thành 0 cho màu đỏ và 1 cho màu xanh lam. 

Các ràng buộc khá nhỏ: có tối đa 100 điểm và 100 đoạn và tọa độ nằm trong khoảng từ 0 đến 100. Điều này cho phép các thuật toán có độ phức tạp bậc hai tính bằng n hoặc m, như$100^2 = 10,000$hoạt động là tầm thường trong giới hạn thời gian 1 giây. Phạm vi tọa độ nhỏ cũng gợi ý rằng chúng ta có thể tận dụng tính năng nén tọa độ hoặc thậm chí là một mảng được lập chỉ mục theo vị trí nếu cần. 

Trường hợp cạnh tinh vi phát sinh khi hai đoạn chồng lên nhau theo cách mà một điểm thuộc về nhiều đoạn. Nếu chúng ta thử một mẫu xen kẽ đơn giản mà không xem xét sự chồng chéo của các phân đoạn, chúng ta có thể tạo ra tình huống trong đó lựa chọn màu của một điểm khiến một trong các phân đoạn chồng chéo vi phạm điều kiện cân bằng. Ví dụ, với điểm`[1, 2, 3]`và phân đoạn`[1, 2]`Và`[2, 3]`, tô màu đỏ cho tất cả các điểm có vẻ ổn đối với đoạn đầu tiên nhưng lại vi phạm sự cân bằng cho đoạn thứ hai. Nhận ra mô hình này là chìa khóa để xây dựng một giải pháp chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ xem xét mọi cách tô màu có thể có của các điểm. Với n điểm thì có$2^n$việc tô màu, điều này nhanh chóng trở nên không khả thi ngay cả với n = 20. Việc kiểm tra từng màu đối với tất cả các phân đoạn là đơn giản, nhưng tổng số thao tác tăng theo cấp số nhân, khiến cho lực lượng vũ phu không thực tế đối với n = 100. 

Quan sát quan trọng là các đoạn sẽ độc lập nếu bạn chỉ xem xét các điểm liền kề giữa các ranh giới của đoạn. Đối với bất kỳ đoạn [l, r] nào, miễn là các điểm trong đoạn đó được tô màu xen kẽ dọc theo trục x, điều kiện cân bằng |red - blue| 1 được tự động thỏa mãn. Điều này hoạt động vì các điểm khác biệt và được sắp xếp dọc theo trục OX. Điểm bên ngoài đoạn có màu gì không quan trọng vì các đoạn chỉ tính số điểm mà chúng chứa. Sắp xếp các điểm theo tọa độ x và sau đó tô màu chúng lần lượt đảm bảo rằng mọi đoạn có các điểm liên tiếp đều thỏa mãn yêu cầu cân bằng. 

Cái nhìn sâu sắc này làm giảm vấn đề thành một chiến lược tham lam đơn giản: sắp xếp các điểm theo tọa độ x và gán màu luân phiên, bắt đầu bằng màu đỏ. Không cần phải quay lại hoặc xử lý xung đột một cách rõ ràng vì sự luân phiên đảm bảo tất cả các phân đoạn có thể được cân bằng. Cách tiếp cận này là tuyến tính về số điểm, cộng với chi phí sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * m) | O(n) | Quá chậm | 
| Màu xen kẽ theo thứ tự X | O(n log n + n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc n, m, các điểm và điểm cuối của đoạn. 
2. Ghép nối từng điểm với chỉ mục ban đầu của nó vì việc sắp xếp sẽ sắp xếp lại chúng. 
3. Sắp xếp các điểm theo tọa độ x. 
4. Khởi tạo mảng màu có kích thước n. 
5. Gán màu xen kẽ dọc theo các điểm đã sắp xếp, bắt đầu bằng màu đỏ (0). Ví dụ: điểm đầu tiên là 0, điểm thứ hai là 1, điểm thứ ba là 0, v.v. 
6. Khôi phục thứ tự ban đầu bằng cách sử dụng các chỉ mục đã lưu và in mảng màu. 

Tại sao nó hoạt động: Bằng cách tô màu các điểm theo thứ tự xen kẽ dọc theo trục x, bất kỳ tập hợp con điểm liền kề nào (tạo thành một đoạn) sẽ có sự khác biệt về màu sắc nhiều nhất là một. Điều này thỏa mãn điều kiện cân bằng của bài toán cho tất cả các phân đoạn. Việc sắp xếp đảm bảo rằng chúng ta xử lý các điểm theo thứ tự từ trái sang phải và luân phiên đảm bảo sự cân bằng bất biến. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
points = list(map(int, input().split()))
segments = [tuple(map(int, input().split())) for _ in range(m)]

# keep original indices
indexed_points = list(enumerate(points))
indexed_points.sort(key=lambda x: x[1])

colors = [0] * n
for i, (idx, _) in enumerate(indexed_points):
    colors[idx] = i % 2

print(' '.join(map(str, colors)))
```Giải pháp bắt đầu bằng cách đọc dữ liệu đầu vào và giữ nguyên chỉ số ban đầu của điểm. Việc sắp xếp đảm bảo rằng chúng ta xen kẽ các màu một cách chính xác dọc theo trục số. Phân công`i % 2`đảm bảo sự luân phiên và cuối cùng, việc in ấn tôn trọng thứ tự điểm ban đầu. Việc sắp xếp theo tọa độ x là rất quan trọng vì các phân đoạn có thể chứa các điểm liên tiếp tùy ý và chỉ có màu liên tiếp mới giữ được sự cân bằng. 

## Ví dụ đã hoạt động 

**Mẫu 1**: 

Điểm đầu vào:`[3, 7, 14]`Phân đoạn:`[1,5], [6,10], [11,15]`| Chỉ mục được sắp xếp | Idx gốc | Điểm | Màu được chỉ định | 
| --- | --- | --- | --- | 
| 0 | 0 | 3 | 0 | 
| 1 | 1 | 7 | 1 | 
| 2 | 2 | 14 | 0 | 

Đoạn [1,5] chứa`[3]`→ một màu đỏ, không màu xanh → chênh lệch = 1 

Đoạn [6,10] chứa`[7]`→ một màu xanh, không đỏ → chênh lệch = 1 

Đoạn [11,15] chứa`[14]`→ một màu đỏ, không màu xanh → chênh lệch = 1 

Tất cả các phân đoạn đều đáp ứng yêu cầu. 

**Mẫu tùy chỉnh 2**: 

Điểm:`[2, 4, 5, 8]`Phân đoạn:`[2,5], [4,8]`| Chỉ mục được sắp xếp | Idx gốc | Điểm | Màu được chỉ định | 
| --- | --- | --- | --- | 
| 0 | 0 | 2 | 0 | 
| 1 | 1 | 4 | 1 | 
| 2 | 2 | 5 | 0 | 
| 3 | 3 | 8 | 1 | 

Đoạn [2,5] chứa`[2,4,5]`→ màu sắc`[0,1,0]`→ đỏ=2, xanh=1 → chênh lệch =1 

Đoạn [4,8] chứa`[4,5,8]`→ màu sắc`[1,0,1]`→ đỏ=1, xanh=2 → chênh lệch=1 

Mẫu xen kẽ bảo tồn tính bất biến trên các phân đoạn chồng chéo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n + m) | Sắp xếp các điểm là O(n log n), việc xử lý phân đoạn là ẩn | 
| Không gian | O(n + m) | Lưu trữ màu sắc và các điểm được lập chỉ mục, cộng với các phân đoạn | 

Giải pháp dễ dàng phù hợp với giới hạn: 100 điểm và 100 phân đoạn mang lại thời gian tính toán và mức sử dụng bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, m = map(int, input().split())
    points = list(map(int, input().split()))
    segments = [tuple(map(int, input().split())) for _ in range(m)]
    indexed_points = list(enumerate(points))
    indexed_points.sort(key=lambda x: x[1])
    colors = [0] * n
    for i, (idx, _) in enumerate(indexed_points):
        colors[idx] = i % 2
    return ' '.join(map(str, colors))

# Provided sample
assert run("3 3\n3 7 14\n1 5\n6 10\n11 15\n") in ["0 1 0", "0 0 0", "1 0 1"], "sample 1"

# Minimum input
assert run("1 1\n0\n0 0\n") == "0", "single point"

# Maximum input with alternating pattern
assert run("4 2\n1 2 3 4\n1 2\n3 4\n") in ["0 1 0 1", "1 0 1 0"], "overlapping segments"

# All points at same value (edge case)
assert run("3 1\n5 5 5\n5 5\n") in ["0 1 0", "1 0 1"], "single-point segment repeated"

# Non-overlapping segments
assert run("5 2\n1 3 5 7 9\n0 2\n6 8\n") in ["0 1 0 1 0", "1 0 1 0 1"], "non-overlapping segments"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1\n0\n0 0`|`0`| Trường hợp điểm đơn | 
|`4 2\n1 2 3 4\n1 2\n3 4`|`0 1 0 1`| Mô hình xen kẽ trên nhiều phân khúc | 
|`3 1\n5 5 5\n5 5`|`0 1 0`| Các điểm cùng tọa độ với đoạn một điểm | 
|`5 2\n1 3 5 7 9\n0 2\n6 8`|`0 1 0 1 0`| Phân đoạn không chồng chéo | 

## Vỏ cạnh 

Khi các điểm trùng với ranh giới của đoạn hoặc khi các đoạn chồng lên nhau, chiến lược luân phiên vẫn đảm bảo rằng bất kỳ đoạn nào cũng sẽ chứa số lượng điểm đỏ và điểm xanh bằng nhau hoặc khác nhau.
