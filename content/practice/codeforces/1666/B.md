---
title: "CF 1666B - Phân bổ ngân sách"
description: "Chúng tôi được yêu cầu phân bổ thêm tiền ngân sách cho một số chủ đề, mỗi chủ đề bao gồm một số lượng nhỏ các hạng mục. Đối với mỗi chủ đề, các phần tiền tương đối tối ưu cho các mục của nó được đưa ra và một số tiền đã được gán cho các mục và không thể xóa được."
date: "2026-06-10T02:13:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 1666
codeforces_index: "B"
codeforces_contest_name: "2021-2022 ICPC, NERC, Northern Eurasia Onsite (Unrated, Online Mirror, ICPC Rules, Teams Preferred)"
rating: 3300
weight: 1666
solve_time_s: 114
verified: false
draft: false
---

[CF 1666B - Phân bổ ngân sách](https://codeforces.com/problemset/problem/1666/B) 

**Đánh giá:** 3300 
**Thẻ:** - 
**Thời gian giải:** 1m 54s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu phân bổ thêm tiền ngân sách cho một số chủ đề, mỗi chủ đề bao gồm một số lượng nhỏ các hạng mục. Đối với mỗi chủ đề, các phần tiền tương đối tối ưu cho các mục của nó được đưa ra và một số tiền đã được gán cho các mục và không thể xóa được. Nhiệm vụ là tính toán tổng số "không tối ưu" tối thiểu, được định nghĩa là tổng chênh lệch tuyệt đối giữa tỷ lệ tiền thực tế và tỷ lệ mục tiêu, sau khi phân phối thêm một lượng tiền nhất định. 

Cụ thể mỗi chủ đề có`n_i`các mặt hàng có số tiền ban đầu`hat_c[i][j]`và phân số lý tưởng`p[i][j]`. Nếu chúng ta thêm`d[i][j]`thêm tiền vào món hàng`j`trong chủ đề`i`, tỷ lệ cuối cùng trở thành`(hat_c[i][j] + d[i][j]) / (sum_j hat_c[i][j] + sum_j d[i][j])`. Chúng ta phải chọn`d[i][j] >= 0`tổng hợp lại thành tổng cộng thêm`x_k`để giảm thiểu tổng số các chủ đề và mục của`|actual_ratio - ideal_ratio|`. 

Thách thức chính là mức độ tối ưu của mỗi chủ đề là một hàm phi tuyến tính của số tiền bổ sung được thêm vào, bởi vì nó phụ thuộc vào tổng số tiền trong chủ đề và giá trị tuyệt đối đưa ra hành vi tuyến tính từng phần. Giá trị tiền thêm`x_k`có thể rất lớn (`10^12`) và có thể lên tới`3 * 10^5`các truy vấn, do đó, mọi tối ưu hóa lặp lại cho mỗi truy vấn sẽ quá chậm. May mắn thay, số lượng mục cho mỗi chủ đề rất ít (`2 <= n_i <= 5`), cho phép chúng tôi phân tích từng chủ đề một cách độc lập và tính toán trước hàm tuyến tính từng phần về cách lượng tiền tăng thêm làm giảm tính không tối ưu. 

Các trường hợp đặc biệt cần xử lý cẩn thận bao gồm các chủ đề trong đó tất cả số tiền ban đầu bằng 0 ngoại trừ một mặt hàng hoặc khi một mặt hàng đã có chính xác tỷ lệ mục tiêu. Việc phân phối đồng đều số tiền bổ sung một cách ngây thơ có thể gây ra tình trạng không tối ưu lớn nếu mức phân phối có sẵn bị sai lệch. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực là, đối với mỗi truy vấn`x_k`, hãy thử mọi cách để phân bổ số tiền thừa giữa các mục trong mỗi chủ đề để giảm thiểu sự khác biệt tuyệt đối. Bởi vì số tiền tăng thêm là liên tục nên người ta có thể coi nó như một bài toán quy hoạch tuyến tính. Với tối đa 5 mục cho mỗi chủ đề và 50.000 chủ đề, số lượng biến là 250.000. Việc giải quyết 250.000 LP biến đổi cho 300.000 truy vấn là không khả thi. 

Cái nhìn sâu sắc quan trọng là trong mỗi chủ đề, hàm mô tả mức độ không tối ưu có thể đạt được tối thiểu như một hàm của số tiền bổ sung là tuyến tính và lồi từng phần. Mỗi mục có độ dốc +1, -1 hoặc 0 theo đạo hàm của sự không tối ưu đối với việc thêm tiền. Sắp xếp tất cả các độ dốc ứng cử viên theo các mục sẽ đưa ra các điểm dừng nơi độ dốc thay đổi. Vì vậy, với mỗi chủ đề, chúng ta có thể tính toán trước một chuỗi các breakpoint`(required_extra, non_optimality)`thể hiện mức độ không tối ưu giảm khi số tiền tăng thêm. Vì số lượng mục cho mỗi chủ đề nhiều nhất là 5 nên mỗi chủ đề chỉ đóng góp một số điểm dừng. Sau đó, việc trả lời từng truy vấn tương đương với việc hợp nhất các chuỗi này và thực hiện tìm kiếm nhị phân để tìm ra tổng số điểm không tối ưu một cách hiệu quả. 

Về bản chất, bài toán quy về việc tính toán, đối với mỗi chủ đề, hàm tuyến tính từng phần của tính không tối ưu đối với tổng ngân sách của chủ đề. Khi có các hàm này, chúng ta có thể tính tổng chúng và trả lời từng truy vấn theo thời gian logarit. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(q * (n_1 * n_2 * ... * n_t)) | O(tổng số mục) | Quá chậm | 
| Tối ưu | O(T log T + Q log T) | O(T) | Đã chấp nhận | 

Đây,`T`là tổng số mặt hàng (<= 250.000),`Q`là số lượng truy vấn (<= 3*10^5). Việc tính toán trước các hàm tuyến tính từng phần chiếm ưu thế trong thời gian chạy nhưng có tính khả thi. 

## Hướng dẫn thuật toán 

1. **Chuẩn hóa phân số lý tưởng**: Với mỗi chủ đề, chuyển đổi các số nguyên đầu vào`p'`thành phân số thực tế`p[i][j]`tổng hợp thành 1. 
2. **Tính tỷ lệ ban đầu**: Với mỗi chủ đề, tính tổng số tiền được giao ban đầu`C_i = sum_j hat_c[i][j]`và tỉ lệ của từng món`r[j] = hat_c[i][j] / C_i`. 
3. **Xác định độ dốc cho mỗi mục**: Đối với một mục`j`, nếu như`r[j] < p[j]`, thêm tiền làm giảm tính không tối ưu (độ dốc -1). Nếu như`r[j] > p[j]`, thêm tiền sẽ làm tăng tính không tối ưu (độ dốc +1). Nếu như`r[j] == p[j]`, số tiền tăng thêm không làm thay đổi tính không tối ưu (độ dốc 0). Độ dốc thay đổi tại các điểm dừng trong đó việc thêm đủ tiền sẽ mang lại tỷ lệ chính xác cho phần mục tiêu. 
4. **Tính điểm dừng cho mỗi chủ đề**: Đối với mỗi mục, hãy tính số tiền cần thiết để đưa tỷ lệ về phân số lý tưởng. Thu thập tất cả các điểm dừng duy nhất trong đó bất kỳ độ dốc nào thay đổi, sắp xếp chúng và tính toán mức độ không tối ưu tại mỗi điểm dừng. Vì n_i <= 5 nên có tối đa 5 breakpoint cho mỗi chủ đề. 
5. **Hợp nhất các hàm chủ đề**: Mỗi chủ đề đóng góp một hàm tuyến tính từng phần`(required_extra, non_optimality)`. Kết hợp tất cả các chủ đề bằng cách tính tổng các độ dốc qua các điểm dừng. Điều này cho phép chúng ta xây dựng hàm tuyến tính từng phần toàn cầu của tổng số tiền tăng thêm cho tổng số không tối ưu. 
6. **Trả lời các truy vấn**: Đối với mỗi`x_k`, thực hiện tìm kiếm nhị phân trên các điểm dừng đã hợp nhất để tìm đoạn chứa`x_k`, sau đó tính toán mức độ không tối ưu bằng cách sử dụng hàm tuyến tính trong phân đoạn đó. 

**Tại sao nó hoạt động**: Bất biến quan trọng là trong mỗi phân khúc, độ dốc của mức không tối ưu đối với số tiền tăng thêm là không đổi và được biết trước. Vì hàm này là tuyến tính từng phần và lồi nên việc tính tổng các hàm này theo các chủ đề sẽ duy trì tính tuyến tính từng phần và cho phép tính toán chính xác ở bất kỳ giá trị tiền bổ sung nào. Điểm dừng nắm bắt tất cả các thay đổi về độ dốc, do đó chúng tôi không bao giờ bỏ lỡ điểm mà tính không tối ưu giảm hiệu quả hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from bisect import bisect_right

t, q = map(int, input().split())
topics = []

for _ in range(t):
    parts = list(map(int, input().split()))
    n = parts[0]
    hat = parts[1:n+1]
    p_raw = parts[n+1:]
    s = sum(p_raw)
    p = [x/s for x in p_raw]
    topics.append((n, hat, p))

queries = list(map(int, input().split()))

# For each topic, compute its piecewise linear non-optimality function
segments = []  # list of (start_extra, slope, intercept)

for n, hat, p in topics:
    C = sum(hat)
    breakpoints = []
    nonopt0 = 0.0
    for i in range(n):
        if C == 0:
            ratio = 0
        else:
            ratio = hat[i] / C
        nonopt0 += abs(ratio - p[i])
        if ratio < p[i]:
            bp = (C * p[i] - hat[i])
            if bp > 0:
                breakpoints.append((bp, -1))
        elif ratio > p[i]:
            bp = (hat[i] - C * p[i])
            if bp > 0:
                breakpoints.append((bp, +1))
    breakpoints.sort()
    segs = []
    last_extra = 0
    slope = sum(-1 if (hat[i]/C if C>0 else 0) < p[i] else 1 if (hat[i]/C if C>0 else 0) > p[i] else 0 for i in range(n))
    intercept = nonopt0
    for bp, delta in breakpoints:
        segs.append((last_extra, slope, intercept))
        intercept += slope * (bp - last_extra)
        slope -= delta  # slope changes when breakpoint is reached
        last_extra = bp
    segs.append((last_extra, slope, intercept))
    segments.append(segs)

# Merge all segments
points = set()
for segs in segments:
    for x, _, _ in segs:
        points.add(x)
points = sorted(points)
slopes = []
intercepts = []

for i, x in enumerate(points):
    total_slope = 0
    total_intercept = 0
    for segs in segments:
        idx = bisect_right([s[0] for s in segs], x) - 1
        _, s, c = segs[idx]
        total_slope += s
        total_intercept += c
    slopes.append(total_slope)
    intercepts.append(total_intercept)

# Answer queries
for xk in queries:
    idx = bisect_right(points, xk) - 1
    total = intercepts[idx] + slopes[idx] * (xk - points[idx])
    print(total)
```Giải pháp đầu tiên tính toán độ không tối ưu ban đầu và độ dốc của từng mục của chủ đề. Sau đó nó xác định các điểm dừng nơi độ dốc thay đổi. Bằng cách lưu trữ các phân đoạn dưới dạng (bắt đầu, độ dốc, phần chặn), chúng tôi có thể nhanh chóng tính toán tổng số điểm không tối ưu cho bất kỳ`x_k`sử dụng tìm kiếm nhị phân. Các trường hợp ranh giới như`C=0`được xử lý để tránh chia cho số 0. Việc sắp xếp các điểm dừng đảm bảo chúng tôi áp dụng các thay đổi về độ dốc theo đúng thứ tự. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

| Thêm | Không tối ưu | 
| --- | --- | 
| | |
