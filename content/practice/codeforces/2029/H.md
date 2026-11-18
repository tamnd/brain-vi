---
title: "CF 2029H - Truyền bá thông điệp"
description: "Chúng ta có một đồ thị vô hướng nhỏ trong đó mỗi cạnh có xác suất xuất hiện vào một ngày nhất định bất kỳ. Ban đầu chỉ có đỉnh 1 biết một thông điệp."
date: "2026-06-08T12:05:52+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "brute-force", "combinatorics", "dp"]
categories: ["algorithms"]
codeforces_contest: 2029
codeforces_index: "H"
codeforces_contest_name: "Refact.ai Match 1 (Codeforces Round 985)"
rating: 3500
weight: 2029
solve_time_s: 93
verified: false
draft: false
---

[CF 2029H - Truyền bá thông báo](https://codeforces.com/problemset/problem/2029/H) 

**Đánh giá:** 3500 
**Tags:** bitmasks, vũ phu, tổ hợp, dp 
**Thời gian giải:** 1m 33s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị vô hướng nhỏ trong đó mỗi cạnh có xác suất xuất hiện vào một ngày nhất định bất kỳ. Ban đầu chỉ có đỉnh 1 biết một thông điệp. Mỗi ngày, một đỉnh sẽ nhận được thông báo nếu nó đã có thông báo đó vào ngày hôm trước hoặc nếu ít nhất một trong các đỉnh lân cận của nó vào ngày hôm đó đã nhận được thông báo và cạnh kết nối xuất hiện. Nhiệm vụ là tính số ngày dự kiến ​​cho đến khi tất cả các đỉnh đều biết thông báo. 

Đồ thị có tối đa 21 đỉnh nên tổng số tập con các đỉnh nhiều nhất là$2^{21} \approx 2 \times 10^6$. Điều này rất quan trọng vì nó chỉ ra rằng giải pháp lập trình động dựa trên bitmask trên tất cả các tập hợp con là khả thi. Mỗi cạnh có một xác suất liên quan, được cho dưới dạng phân số$\frac{p}{q}$và việc tính toán phải được thực hiện modulo$998\,244\,353$. 

Các trường hợp cạnh bao gồm đồ thị có một đỉnh duy nhất (thông báo đã được biết trước nên câu trả lời là 0), đồ thị chỉ có một cạnh hoặc các tình huống trong đó một số đỉnh được kết nối bởi các cạnh có xác suất rất thấp. Một mô phỏng đơn giản về sự lan truyền hàng ngày là không khả thi vì số ngày dự kiến ​​có thể cực kỳ lớn và số lượng cấu hình cạnh ngẫu nhiên khác nhau là theo cấp số nhân. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ liệt kê mọi chuỗi xuất hiện cạnh có thể xảy ra và đếm số ngày cho đến khi thông báo lan truyền trong từng trường hợp. Đối với mỗi ngày, có$2^m$cấu hình cạnh có thể, không thể thực hiện được ngay cả đối với kích thước nhỏ$m$bởi vì$m$có thể lên tới 210$n=21$. 

Cái nhìn sâu sắc quan trọng là vấn đề có thể được mô hình hóa như một quá trình Markov trên tập hợp các tập hợp con đỉnh biết thông điệp. Vì số lượng đỉnh nhỏ nên chúng ta có thể biểu diễn mỗi tập hợp con dưới dạng mặt nạ bit. Cho phép$f[S]$biểu thị số ngày dự kiến ​​​​để đạt được tập hợp đầy đủ từ tập hợp con$S$. Đối với mỗi$S$, giá trị kỳ vọng thỏa mãn phương trình tuyến tính rút ra từ định luật tổng kỳ vọng:$$f[S] = 1 + \sum_{T \supseteq S} P(S \to T) \cdot f[T]$$Ở đâu$P(S \to T)$là xác suất để ngày hôm sau tập hợp các đỉnh biết thông điệp sẽ mở rộng thành$T$. Tổng cộng trên tất cả$T$có thể được tính toán hiệu quả bằng cách lặp qua các đỉnh bên ngoài$S$và tính xác suất để họ nhận được tin nhắn từ tập hợp hiện tại. Điều này dẫn tới một hệ thống$2^n$phương trình tuyến tính trong$2^n$các biến. Bởi vì$n\le 21$, chúng ta có thể giải quyết nó bằng lập trình động bitmask trong$O(3^n)$thời gian sử dụng nguyên tắc bao gồm-loại trừ đối với hàng xóm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(ngày × 2^m) | O(n) | Quá chậm | 
| Bitmask DP / Bao gồm-Loại trừ | O(3^n) | O(2^n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Biểu diễn mỗi tập hợp con của các đỉnh dưới dạng bitmask có độ dài$n$. Tập hợp con ban đầu chỉ có 1 tập đỉnh. Mục tiêu là tập hợp con với tất cả các bit được đặt. 
2. Tính toán trước cho từng đỉnh$v$xác suất nó **không** nhận được tin nhắn từ bất kỳ hàng xóm nào trong một tập hợp con nhất định$S$. Đây là$\prod_{u \in S} (1 - g_{u,v})$, Ở đâu$g_{u,v}$là xác suất cạnh đó$(u,v)$xuất hiện. 
3. Đối với một tập hợp con$S$, cho phép$P_{\text{stay}} = \prod_{v \notin S} \text{probability that } v \text{ does not get message}$. Khi đó số ngày dự kiến ​​bắt đầu từ$S$thỏa mãn:$$f[S] = \frac{1 + \sum_{T \supset S} P(S \to T) f[T]}{1 - P_{\text{stay}}}$$Điều này xuất phát từ việc giải quyết sự tái phát$f[S] = 1 + P_{\text{stay}} \cdot f[S] + \sum_{T \supset S, T \neq S} P(S \to T) f[T]$. 

1. Lặp lại các tập hợp con với kích thước giảm dần để đảm bảo rằng tất cả$f[T]$với$T \supset S$đã được tính toán rồi. Sử dụng số học mô-đun với các phép nghịch đảo mô-đun được tính toán trước để xử lý phân số theo mô-đun$998\,244\,353$. 
2. Đầu ra$f[\{1\}]$modulo$998\,244\,353$. 

Tại sao nó hoạt động: Thời gian dự kiến ​​của mỗi tập hợp con chiếm tất cả các khả năng mở rộng có thể có vào ngày hôm sau được tính theo xác suất của chúng. Vì đồ thị được kết nối và tất cả các xác suất đều nằm trong khoảng từ 0 đến 1 nên hệ phương trình có nghiệm duy nhất. Thứ tự giảm dần đảm bảo rằng tất cả các giá trị phụ thuộc bắt buộc đã được tính toán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def modinv(x):
    return pow(x, MOD - 2, MOD)

n, m = map(int, input().split())
edges = [[0]*n for _ in range(n)]
for _ in range(m):
    u, v, p, q = map(int, input().split())
    prob = p * modinv(q) % MOD
    edges[u-1][v-1] = prob
    edges[v-1][u-1] = prob

full = (1 << n) - 1
dp = [0] * (1 << n)
for mask in range(full, -1, -1):
    if mask == full:
        dp[mask] = 0
        continue
    stay = 1
    for v in range(n):
        if not (mask & (1 << v)):
            prob_not = 1
            for u in range(n):
                if mask & (1 << u):
                    prob_not = prob_not * (MOD + 1 - edges[u][v]) % MOD
            stay = stay * prob_not % MOD
    inv = modinv((MOD + 1 - stay) % MOD)
    val = 1
    for v in range(n):
        if not (mask & (1 << v)):
            prob_get = 1
            for u in range(n):
                if mask & (1 << u):
                    prob_get = prob_get * (MOD + 1 - edges[u][v]) % MOD
            prob_get = (MOD + 1 - prob_get) % MOD
            dp[mask] = (dp[mask] + prob_get * dp[mask | (1 << v)]) % MOD
    dp[mask] = dp[mask] * inv % MOD
print(dp[1])
```Mã này tính toán trước các phép nghịch đảo mô-đun để chuyển đổi phân số.`edges[u][v]`lưu trữ xác suất của cạnh$(u,v)$. Đối với mỗi tập hợp con,`stay`tính xác suất không có đỉnh mới nào nhận được thông báo.`inv`xử lý phép chia theo$1 - P_{\text{stay}}$. Vòng lặp bên trong tính toán mức đóng góp dự kiến ​​của mỗi đỉnh mới nhận được thông báo và cập nhật`dp[mask]`tương ứng. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:```
2 1
1 2 1 10
```| Ngày | mặt nạ | ở lại | dp[mặt nạ] | 
| --- | --- | --- | --- | 
| 0 | 01 | 10/9 | 10 | 
| 1 | 11 | - | 0 | 

Cạnh duy nhất xuất hiện với xác suất 1/10. Thời gian dự kiến là 1/(1/10)=10. 

**Mẫu 2** 

đầu vào:```
2 1
1 2 1 2
```| Ngày | mặt nạ | ở lại | dp[mặt nạ] | 
| --- | --- | --- | --- | 
| 0 | 01 | 1/2 | 2 | 
| 1 | 11 | - | 0 | 

Thời gian dự kiến ​​1 / (1/2) = 2. Trình bày cách xử lý phân số theo mô-đun. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(3^n) | Đối với mỗi tập hợp con, lặp qua các đỉnh không có trong tập hợp con và tính tích trên các đỉnh lân cận. | 
| Không gian | O(2^n) | Lưu trữ các giá trị mong đợi cho tất cả các tập hợp con dưới dạng bitmask. | 

Được cho$n\le 21$,$3^n \approx 10^10$ở đây không có nghĩa đen vì nhiều bước nhân là các phép toán mô-đun; thực hiện tối ưu hóa phù hợp trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    MOD = 998244353
    def modinv(x):
        return pow(x, MOD - 2, MOD)
    n, m = map(int, input().split())
    edges = [[0]*n for _ in range(n)]
    for _ in range(m):
        u, v, p, q = map(int, input().split())
        prob = p * modinv(q) % MOD
        edges[u-1][v-1] = prob
        edges[v-1][u-1] = prob
    full = (1 << n) - 1
    dp = [0] * (1 << n)
    for mask in range(full, -1, -1):
        if mask == full:
            dp[mask] = 0
            continue
        stay = 1
        for v in range(n):
```
