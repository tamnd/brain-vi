---
title: "CF 30C - Phòng trưng bày bắn súng"
description: "Chúng tôi được yêu cầu giúp King Copa tối đa hóa số lần bắn trúng dự kiến ​​của anh ấy trong một phòng tập bắn súng. Thư viện được thể hiện dưới dạng mặt phẳng 2D và mỗi mục tiêu xuất hiện tại một điểm cụ thể, chính xác vào một thời điểm cụ thể và biến mất ngay sau đó."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dp", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 30
codeforces_index: "C"
codeforces_contest_name: "Codeforces Beta Round 30 (Codeforces format)"
rating: 1800
weight: 30
solve_time_s: 60
verified: true
draft: false
---
[CF 30C - Thư viện bắn súng](https://codeforces.com/problemset/problem/30/C) 

**Đánh giá:** 1800 
**Tags:** dp, xác suất 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu giúp King Copa tối đa hóa số lần bắn trúng dự kiến của anh ấy trong một phòng tập bắn súng. Thư viện được thể hiện dưới dạng mặt phẳng 2D và mỗi mục tiêu xuất hiện tại một điểm cụ thể$(x_i, y_i)$chính xác tại một thời điểm cụ thể$t_i$và biến mất ngay sau đó. Nếu ống ngắm của nhà vua nhắm vào mục tiêu khi nó xuất hiện, ông ta có xác suất bắn trúng mục tiêu đó$p_i$. Ống ngắm của súng có thể di chuyển với tốc độ đơn vị, vì vậy hãy nhắm từ một điểm$(x_1, y_1)$đến người khác$(x_2, y_2)$, phải mất khoảng cách Euclide$\sqrt{(x_2-x_1)^2 + (y_2-y_1)^2}$đơn vị thời gian. Nhà vua biết trước tất cả các mục tiêu và thời gian của chúng, và ông ấy muốn lên kế hoạch cho một chuỗi các bước di chuyển để tối đa hóa tổng xác suất bắn trúng mục tiêu. 

Đầu vào bao gồm$n$mục tiêu ($1 \le n \le 1000$), mỗi cái có tọa độ, thời gian và xác suất. Đầu ra là số lần truy cập dự kiến ​​​​tối đa. 

Các ràng buộc cung cấp cho chúng tôi một số hướng dẫn. Với$n$lên tới 1000 và thời gian lên tới$10^9$, một O($n^2$) có thể chấp nhận được, nhưng mọi giải pháp tiếp cận O($2^n$) là quá chậm. Khoảng cách có thể âm, nhưng chúng tôi chỉ quan tâm đến chi phí di chuyển Euclide. Xác suất là số thực từ 0 đến 1. 

Các trường hợp có biên không rõ ràng bao gồm: một mục tiêu tại thời điểm 0 với xác suất nhỏ hơn 1, nhiều mục tiêu tập trung gần nhau về thời gian đến mức không thể di chuyển giữa chúng hoặc các mục tiêu có xác suất 0 nên bỏ qua. Ví dụ: với đầu vào:```
1
0 0 0 0.5
```Đầu ra là 0,5. Một thuật toán ngây thơ cho rằng vua chỉ có thể ghi được toàn bộ lượt truy cập (0 hoặc 1) sẽ thất bại ở đây. 

Một tình huống khó khăn khác là khi hai mục tiêu ở quá xa nhau để có thể tiếp cận kịp thời:```
2
0 0 1 1
1000 1000 2 1
```Mặc dù$t_2 > t_1$, khoảng cách quá lớn khiến vua không thể tiếp cận mục tiêu thứ hai kịp thời. Một giải pháp bất cẩn có thể thử cả hai và tính toán quá mức. 

## Phương pháp tiếp cận 

Giải pháp vũ phu là xem xét mọi hoán vị của mục tiêu, tính toán xem vua có thể di chuyển từ mục tiêu này sang mục tiêu tiếp theo kịp thời hay không và tính tổng các xác suất. Điều này có hiệu quả vì nó khám phá tất cả các chuỗi, nhưng số lượng hoán vị là$n!$, điều này hoàn toàn không thể thực hiện được đối với$n=1000$. 

Thông tin chi tiết quan trọng là việc bắn trúng mục tiêu phụ thuộc vào kỳ vọng, vì vậy chúng ta có thể mô hình hóa điều này như một dạng lập trình động. Nếu sắp xếp mục tiêu theo thời gian tăng dần, chúng ta chỉ cần xem xét giá trị kỳ vọng tốt nhất có thể đạt được nếu hoàn thành từng mục tiêu. Đối với mục tiêu$i$, chúng ta có thể xem xét tất cả các mục tiêu trước đó$j$có thể đạt được$i$đúng lúc$t_i - t_j \ge \text{distance}(i, j)$và tính:$$dp[i] = p_i + \max(dp[j] \text{ for all valid } j)$$Nếu không có mục tiêu trước đó có thể đạt được$i$, chúng ta có thể bắt đầu từ thời điểm 0 tại bất kỳ vị trí nào, điều này đơn giản hóa thành$dp[i] = p_i$. Điều này làm giảm vấn đề từ$O(n!)$ĐẾN$O(n^2)$, có thể chấp nhận được$n \le 1000$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các hoán vị) | Ồ (n!) | O(n) | Quá chậm | 
| Lập trình động với sắp xếp thời gian | O(n^2) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các mục tiêu và lưu trữ chúng dưới dạng bộ dữ liệu$(x_i, y_i, t_i, p_i)$. 
2. Sắp xếp mục tiêu theo thời gian xuất hiện$t_i$. Điều này đảm bảo rằng mọi mục tiêu trước đó mà chúng tôi xem xét trong DP đều được đảm bảo xuất hiện trước mục tiêu hiện tại, vì vậy chúng tôi chỉ nhìn về phía sau. 
3. Khởi tạo mảng DP`dp[i]`để lưu trữ số lần truy cập dự kiến ​​​​tối đa kết thúc tại mục tiêu$i$. 
4. Lặp lại các mục tiêu theo thứ tự thời gian tăng dần. Đối với mỗi mục tiêu$i$, giả sử ban đầu`dp[i] = p_i`, biểu thị việc bắt đầu trực tiếp tại mục tiêu này từ vị trí ban đầu tại thời điểm 0. 
5. Đối với mọi mục tiêu trước đó$j < i$, kiểm tra xem vua có thể di chuyển từ$j$ĐẾN$i$trong thời gian có sẵn. Tính khoảng cách Euclide$\text{dist} = \sqrt{(x_i - x_j)^2 + (y_i - y_j)^2}$. Nếu như$t_i - t_j \ge \text{dist}$, cập nhật`dp[i] = max(dp[i], dp[j] + p_i)`. 
6. Sau khi xử lý tất cả các mục tiêu, câu trả lời là giá trị lớn nhất trong mảng DP, vì chuỗi có thể kết thúc ở bất kỳ mục tiêu nào. 

Tại sao nó hoạt động: Bất biến DP là`dp[i]`là số lần bắn trúng dự kiến ​​tối ưu có thể đạt được nếu lần bắn trúng mục tiêu cuối cùng là$i$. Vì kỳ vọng có tính bổ sung và chúng tôi luôn xem xét các chuyển đổi khả thi từ các mục tiêu trước đó nên chúng tôi đảm bảo rằng mọi trình tự được xem xét đều hợp lệ và chúng tôi không bao giờ bỏ lỡ trình tự tốt hơn. 

## Giải pháp Python```python
import sys
import math
input = sys.stdin.readline

n = int(input())
targets = []
for _ in range(n):
    x, y, t, p = input().split()
    x = int(x)
    y = int(y)
    t = int(t)
    p = float(p)
    targets.append((t, x, y, p))

targets.sort()  # sort by time

dp = [0.0] * n

for i in range(n):
    t_i, x_i, y_i, p_i = targets[i]
    dp[i] = p_i  # start directly at this target
    for j in range(i):
        t_j, x_j, y_j, _ = targets[j]
        dist = math.hypot(x_i - x_j, y_i - y_j)
        if t_i - t_j >= dist:
            dp[i] = max(dp[i], dp[j] + p_i)

print(f"{max(dp):.10f}")
```Mã đọc và lưu trữ các mục tiêu dưới dạng bộ dữ liệu`(t, x, y, p)`vì vậy việc sắp xếp theo thời gian là chuyện nhỏ.`math.hypot`được sử dụng cho khoảng cách Euclide để tránh bình phương và căn bậc hai thủ công. Khởi tạo DP với`p_i`xử lý bắt đầu tại mục tiêu từ điểm gốc hoặc bất kỳ vị trí ban đầu nào. Kiểm tra tất cả`j < i`đảm bảo chúng tôi xem xét mọi vị trí hợp lệ trước đó. 

## Ví dụ đã hoạt động 

**Ví dụ 1:** 

đầu vào:```
1
0 0 0 0.5
```| tôi | t_i | x_i | y_i | p_i | dp[i] | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | 0 | 0,5 | 0,5 | 

Đầu ra:`0.5`. Xác nhận rằng một mục tiêu duy nhất tạo ra`dp[i] = p_i`. 

**Ví dụ 2:** 

đầu vào:```
3
0 0 1 0.5
1 0 2 0.5
0 1 2 0.5
```| tôi | t_i | x_i | y_i | p_i | dp[i] | chuyển tiếp được xem xét | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 0 | 0,5 | 0,5 | bắt đầu từ 0 | 
| 1 | 2 | 1 | 0 | 0,5 | 1.0 | từ 0: dist=1 <=1 | 
| 2 | 2 | 0 | 1 | 0,5 | 1.0 | từ 0: dist=1 <=1 | 

Đầu ra:`1.0`. Thể hiện nhiều chuyển đổi khả thi và DP chọn chính xác đường dẫn tối ưu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Đối với mỗi mục tiêu trong số n mục tiêu, chúng tôi xem xét tất cả các mục tiêu trước đó, do đó so sánh n*(n-1)/2 | 
| Không gian | O(n) | Mảng DP lưu trữ một float cho mỗi mục tiêu | 

Với n 1000, O(n^2) = 10^6 thao tác, phù hợp thoải mái trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io
import math

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    targets = []
    for _ in range(n):
        x, y, t, p = input().split()
        x = int(x)
        y = int(y)
        t = int(t)
        p = float(p)
        targets.append((t, x, y, p))
    targets.sort()
    dp = [0.0] * n
    for i in range(n):
        t_i, x_i, y_i, p_i = targets[i]
        dp[i] = p_i
        for j in range(i):
            t_j, x_j, y_j, _ = targets[j]
            if
```
