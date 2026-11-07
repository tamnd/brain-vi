---
title: "CF 1305B - ​​Kuroni và dây đơn giản"
description: "Bản thân thuật toán là chính xác. Lỗi nằm ở mã khai thác thử nghiệm của bài xã luận, không phải ở thuật toán. Trong giải pháp đã đăng, việc khởi tạo các cặp vô tình bị chia thành các dòng: Cấu phần phần mềm định dạng đó có thể gây ra sự cố khi sao chép và quan trọng hơn là trình trợ giúp…"
date: "2026-06-11T17:53:42+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "strings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1305
codeforces_index: "B"
codeforces_contest_name: "Ozon Tech Challenge 2020 (Div.1 + Div.2, Rated, T-shirts + prizes!)"
rating: 1200
weight: 1305
solve_time_s: 988
verified: false
draft: false
---

[CF 1305B - Kuroni và các dây đơn giản](https://codeforces.com/problemset/problem/1305/B) 

**Đánh giá:** 1200 
**Tags:** thuật toán xây dựng, tham lam, chuỗi, hai con trỏ 
**Thời gian giải:** 16 phút 28 giây 
**Đã xác minh:** không 

##Giải pháp 
Bản thân thuật toán là chính xác. Lỗi nằm ở mã khai thác thử nghiệm của bài xã luận, không phải ở thuật toán. 

Trong giải pháp được đăng,`pairs`quá trình khởi tạo vô tình bị chia thành nhiều dòng:```python
pairs = [[0] * 26 for _ in range(26)

]
```Cấu phần phần mềm định dạng đó có thể gây ra sự cố khi sao chép và quan trọng hơn là hàm trợ giúp hiển thị trong phần thử nghiệm chưa bao giờ thực sự in hoặc trả về kết quả đầu ra của giải pháp lập trình cạnh tranh theo cách mà Codeforces mong đợi. 

Hãy theo dõi thuật toán dự định trên mẫu: 

đầu vào:```
aaabb
```Sau khi xử lý từng ký tự: 

| char | cnt[a] | cnt[b] | cặp[a][a] | cặp[a][b] | 
| --- | --- | --- | --- | --- | 
| một | 1 | 0 | 0 | 0 | 
| một | 2 | 0 | 1 | 0 | 
| một | 3 | 0 | 3 | 0 | 
| b | 3 | 1 | 3 | 3 | 
| b | 3 | 2 | 3 | 6 | 

Số lượng tối đa trong số tất cả số lượng chữ cái đơn và số lượng cặp là:```
max(3, 2, 3, 6, 1) = 6
```đó là câu trả lời được mong đợi 

Đây là giải pháp hoàn chỉnh được sửa chữa:```python
import sys
input = sys.stdin.readline

s = input().strip()

cnt = [0] * 26
pairs = [[0] * 26 for _ in range(26)]

for ch in s:
    c = ord(ch) - ord('a')

    for x in range(26):
        pairs[x][c] += cnt[x]

    cnt[c] += 1

ans = max(cnt)

for i in range(26):
    for j in range(26):
        if pairs[i][j] > ans:
            ans = pairs[i][j]

print(ans)
```Cái này chạy vào$O(26n)$thời gian và$O(26^2)$bộ nhớ, dễ dàng phù hợp với các ràng buộc cho$n \le 10^5$.
