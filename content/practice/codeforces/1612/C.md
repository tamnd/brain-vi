---
title: "CF 1612C - Chất Bản"
description: "Chúng tôi được yêu cầu mô phỏng một tình huống trò chuyện trong đó người dùng muốn gửi một \"tam giác biểu tượng cảm xúc\" bao gồm các tin nhắn với số lượng biểu cảm tăng dần rồi giảm dần."
date: "2026-06-10T06:57:15+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "math"]
categories: ["algorithms"]
codeforces_contest: 1612
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 117 (Rated for Div. 2)"
rating: 1300
weight: 1612
solve_time_s: 75
verified: true
draft: false
---

[CF 1612C - Lệnh cấm trò chuyện](https://codeforces.com/problemset/problem/1612/C) 

**Đánh giá:** 1300 
**Tags:** tìm kiếm nhị phân, toán học 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu mô phỏng một tình huống trò chuyện trong đó người dùng muốn gửi một "tam giác biểu tượng cảm xúc" bao gồm các tin nhắn với số lượng biểu cảm tăng dần rồi giảm dần. Đối với một kích thước nhất định$k$, tin nhắn đầu tiên có một biểu tượng cảm xúc, tin nhắn thứ hai có hai biểu tượng cảm xúc, tối đa$k$-tin nhắn thứ với$k$biểu tượng cảm xúc thì các tin nhắn sau sẽ giảm xuống còn một. Điều này tạo thành một chuỗi các$2k-1$tin nhắn trong đó số lượng biểu tượng cảm xúc trong tin nhắn$i$là$\min(i, 2k-i)$. 

Nền tảng trò chuyện có hệ thống kiểm duyệt sẽ cấm người dùng khi tổng số biểu tượng cảm xúc được gửi đạt hoặc vượt quá ngưỡng nhất định$x$. Nhiệm vụ là xác định xem người dùng có thể gửi bao nhiêu tin nhắn trước khi bị cấm. Nếu người dùng hoàn thành tam giác mà không vượt quá$x$, câu trả lời là$2k-1$. 

Các ràng buộc rất lớn:$k$có thể lên tới$10^9$Và$x$lên tới$10^{18}$. Một mô phỏng đơn giản tổng hợp các biểu tượng cảm xúc theo từng tin nhắn sẽ quá chậm vì nó có thể yêu cầu tối đa$2 \cdot 10^9$bổ sung cho mỗi trường hợp thử nghiệm. Điều này buộc một giải pháp hoạt động theo thời gian logarit hoặc không đổi cho mỗi trường hợp thử nghiệm. 

Các trường hợp cạnh không rõ ràng bao gồm nhỏ$k$và rất nhỏ$x$, ví dụ$k=1, x=1$. Hình tam giác chỉ có một tin nhắn và người dùng có thể bị cấm ngay lập tức. Một trường hợp cạnh khác là khi$x$lớn hơn tổng của cả tam giác. Vì$k=4$Và$x=20$, tổng của tam giác là 16. Trong trường hợp này, người dùng hoàn thành tất cả các tin nhắn mà không bị cấm. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản: tạo chuỗi tin nhắn, giữ tổng chạy và đếm tin nhắn cho đến khi tổng đạt hoặc vượt quá$x$. Điều này làm việc cho nhỏ$k$nhưng thất bại khi$k$lớn vì việc tổng hợp hàng tỷ tin nhắn là quá chậm. Số lượng hoạt động trong trường hợp xấu nhất là$O(k)$cho mỗi trường hợp thử nghiệm, điều này không thể chấp nhận được đối với$k$lên tới$10^9$Và$t$lên tới$10^4$. 

Quan sát quan trọng là số lượng biểu cảm ở mỗi nửa tam giác tạo thành một chuỗi số học đơn giản. Nửa đầu tăng từ 1 lên$k$và tính tổng thành$k(k+1)/2$. Nửa sau giảm từ$k-1$đến 1, tổng hợp thành$(k-1)k/2$. Tổng tổng của tam giác là$k^2$. Điều này cho phép chúng tôi nhanh chóng xác định liệu$x$vượt quá tổng tam giác, trong trường hợp đó người dùng viết tất cả$2k-1$tin nhắn. 

Nếu như$x$nhỏ hơn$k(k+1)/2$, người dùng bị cấm ở phần ngày càng tăng. Chúng ta có thể sử dụng công thức tính tổng của số thứ nhất$m$số tự nhiên và giải bất phương trình bậc hai$m(m+1)/2 \ge x$để tìm nhỏ nhất$m$. Nếu như$x$vượt quá$k(k+1)/2$nhưng ít hơn$k^2$, lệnh cấm xảy ra ở phần giảm dần. Chúng ta có thể giải quyết vấn đề: trừ tổng của nửa đầu từ$x$để tính các biểu cảm còn lại, sau đó áp dụng công thức tương tự cho dãy giảm dần. Sử dụng tìm kiếm nhị phân trên số lượng tin nhắn hoạt động hiệu quả ở cả hai nửa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k) | O(1) | Quá chậm đối với k lớn | 
| Tối ưu (Tìm kiếm nhị phân trên một nửa tam giác) | O(logk) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi trường hợp thử nghiệm, hãy tính tổng số biểu cảm trong nửa đầu$S_1 = k(k+1)/2$. So sánh$x$ĐẾN$S_1$. 
2. Nếu$x \le S_1$, lệnh cấm xảy ra ở phần ngày càng tăng. Sử dụng tìm kiếm nhị phân trong phạm vi từ 1 đến$k$để tìm số lượng tin nhắn tối thiểu$m$như vậy$m(m+1)/2 \ge x$. Trở lại$m$. 
3. Nếu$x > S_1$Và$x \le k^2$, lệnh cấm xảy ra ở phần giảm dần. Tính các biểu tượng cảm xúc còn lại$x' = x - S_1$. Phần giảm dần gồm$k-1$tin nhắn có biểu tượng cảm xúc$k-1, k-2, ..., 1$. Sử dụng tìm kiếm nhị phân trên$1..k-1$để tìm số lượng tin nhắn tối thiểu$m$như vậy$(k-1 + k-2 + ... + (k-m)) \ge x'$. Tương đương, tổng cuối cùng$m$số từ$k-1$xuống: tổng =$m(2k-m-1)/2$. Trở lại$k + m$. 
4. Nếu$x > k^2$, người dùng hoàn thành tất cả các tin nhắn mà không bị cấm. Trở lại$2k-1$. 

Tại sao nó hoạt động: Tam giác tăng dần rồi giảm dần. Việc sử dụng các công thức chuỗi số học đảm bảo rằng chúng ta tìm được điểm chính xác mà tổng tích lũy đạt tới$x$mà không lặp lại qua từng tin nhắn. Tìm kiếm nhị phân đảm bảo số lượng tin nhắn tối thiểu được tính toán chính xác. Các bất biến là giới hạn tổng tích lũy và tổng cấp số cộng chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def messages_before_ban(k, x):
    total = k * k
    if x >= total:
        return 2 * k - 1
    first_half = k * (k + 1) // 2
    if x <= first_half:
        # binary search in increasing part
        l, r = 1, k
        while l < r:
            m = (l + r) // 2
            if m * (m + 1) // 2 >= x:
                r = m
            else:
                l = m + 1
        return l
    else:
        # banned in decreasing part
        x_rem = x - first_half
        l, r = 1, k - 1
        while l < r:
            m = (l + r) // 2
            if m * (2 * k - m - 1) // 2 >= x_rem:
                r = m
            else:
                l = m + 1
        return k + l

t = int(input())
for _ in range(t):
    k, x = map(int, input().split())
    print(messages_before_ban(k, x))
```Giải pháp trước tiên sẽ kiểm tra xem người dùng có hoàn thành tam giác mà không vượt quá ngưỡng cấm hay không. Nó sử dụng công thức tính tổng số học để xử lý phần tăng và phần giảm riêng biệt. Tìm kiếm nhị phân được sử dụng để tìm một cách hiệu quả số lượng tin nhắn tối thiểu vượt quá ngưỡng, tránh các lỗi sai sót bằng cách bất biến vòng lặp cẩn thận. 

## Ví dụ đã hoạt động 

**Ví dụ 1:**$k = 4, x = 6$| Bước | tôi | r | m | tổng hợp | Quyết định | 
| --- | --- | --- | --- | --- | --- | 
| bắt đầu | 1 | 4 | 2 | 3 | tổng < x → l = 3 | 
| tiếp theo | 3 | 4 | 3 | 6 | tổng ≥ x → r = 3 | 

Đầu ra là 3. Người dùng bị cấm sau khi gửi ba tin nhắn. 

**Ví dụ 2:**$k = 3, x = 7$Tổng nửa đầu = 6 < x → cấm phần giảm dần. Còn lại x = 1. Dãy số giảm = 2,1. 

| Bước | tôi | r | m | tổng hợp | Quyết định | 
| --- | --- | --- | --- | --- | --- | 
| bắt đầu | 1 | 2 | 1 | 2 | tổng ≥ x → r = 1 | 

Đầu ra = k + l = 3 + 1 = 4. Người dùng bị cấm ở tin nhắn thứ 4. 

Những dấu vết này xác nhận tìm kiếm nhị phân xác định chính xác số lượng tin nhắn tối thiểu ở cả hai nửa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log k) cho mỗi trường hợp thử nghiệm | Tìm kiếm nhị phân tăng hoặc giảm một nửa với tối đa k phần tử | 
| Không gian | O(1) | Chỉ có các biến để tính toán số học, không cần mảng | 

Được cho$t \le 10^4$Và$k \le 10^9$,$O(t \log k)$được chấp nhận trong giới hạn thời gian 2 giây. Python xử lý các số nguyên lên tới$10^{18}$không tràn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    t = int(input())
    for _ in range(t):
        k, x = map(int, input().split())
        print(messages_before_ban(k, x))
    return output.getvalue().strip()

# provided samples
assert run("7\n4 6\n4 7\n1 2\n3 7\n2 5\n100 1\n1000000000 923456789987654321\n") == \
"3\n4\n1\n4\n3\n1\n1608737403", "sample 1"

# custom tests
assert run("2\n1 1\n1
```
