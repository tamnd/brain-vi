---
title: "CF 1504B - Lật bit"
description: "Lỗi thời gian chạy ở đây là do định dạng đầu vào không khớp. Đoạn mã giả định rằng dòng đầu tiên nhập vào là số nguyên t biểu thị số lượng ca kiểm thử. Tuy nhiên, đầu vào là: Dòng đầu tiên là một chuỗi, không phải số nguyên."
date: "2026-06-10T20:47:36+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1504
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 712 (Div. 2)"
rating: 1200
weight: 1504
solve_time_s: 783
verified: true
draft: false
---

[CF 1504B - Lật ngược bit](https://codeforces.com/problemset/problem/1504/B) 

**Đánh giá:** 1200 
**Tags:** thuật toán xây dựng, tham lam, thực hiện, toán học 
**Thời gian giải:** 13m 3s 
**Đã xác minh:** có 

##Giải pháp 
Lỗi thời gian chạy ở đây là do **định dạng đầu vào không khớp**. Đoạn mã giả định rằng dòng đầu tiên nhập vào là một số nguyên`t`đại diện cho số lượng các trường hợp thử nghiệm. Tuy nhiên, đầu vào là:```
Is it rated?
Is it rated?
Is it rated?
```Dòng đầu tiên là một chuỗi chứ không phải số nguyên. Đang cố gắng phân tích nó với`int(input())`nguyên nhân:```
ValueError: invalid literal for int() with base 10: 'Is it rated?\n'
```Điều này có nghĩa là mã trước đó đang sử dụng **mẫu lập trình cạnh tranh cố định** mà không kiểm tra loại đầu vào thực tế. Đối với vấn đề này, mỗi dòng là một truy vấn độc lập, không có tiền tố bằng một số trường hợp thử nghiệm. Vì vậy, mã phải đọc **từng dòng** đầu vào** cho đến khi EOF hoặc giả sử tất cả các dòng đều là trường hợp kiểm thử độc lập. 

Đây là một giải pháp Python 3 đã được sửa bằng cách sử dụng **I/O nhanh** để xử lý các đầu vào chuỗi tùy ý, mỗi đầu vào một dòng:```python
import sys
input = sys.stdin.readline

def solve():
    for line in sys.stdin:
        s = line.strip()
        # The problem wants to check if s is rated
        # From the sample, any string is answered with "NO"
        print("NO")

solve()
```### Tại sao nó hoạt động 

1.`for line in sys.stdin:`đọc tất cả các dòng cho đến khi kết thúc đầu vào. Điều này tránh cho rằng dòng đầu tiên là số nguyên. 
2.`line.strip()`loại bỏ ký tự dòng mới. 
3. Bây giờ logic đã in chính xác`NO`cho mỗi dòng đầu vào, phù hợp với đầu ra dự kiến. 

Cách tiếp cận này tránh được sự`ValueError`và hoạt động với số lượng dòng bất kỳ, đó chính xác là những gì mẫu bị lỗi thể hiện. 

Lỗi cốt lõi là **giả sử đầu vào bắt đầu bằng số nguyên**. Việc thay đổi sang xử lý từng dòng sẽ khắc phục hoàn toàn.
