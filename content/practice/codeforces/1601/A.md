---
title: "CF 1601A - Loại bỏ mảng"
description: "\"Đầu ra thực tế\" được báo cáo hoàn toàn trống: Điều đó có nghĩa là lỗi không phải là lỗi thuật toán trong chính logic Sắp xếp ngược. Một lỗi thuật toán vẫn in ra thứ gì đó, chỉ là câu trả lời sai."
date: "2026-06-10T08:27:03+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "greedy", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1601
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 751 (Div. 1)"
rating: 1300
weight: 1601
solve_time_s: 520
verified: false
draft: false
---

[CF 1601A - Loại bỏ mảng](https://codeforces.com/problemset/problem/1601/A) 

**Đánh giá:** 1300 
**Tags:** bitmasks, tham lam, toán học, lý thuyết số 
**Thời gian giải:** 8 phút 40s 
**Đã xác minh:** không 

## Giải pháp 
"Đầu ra thực tế" được báo cáo hoàn toàn trống:```
Actual output:
```Điều đó có nghĩa là lỗi không phải là lỗi thuật toán trong logic Sắp xếp ngược. Một lỗi thuật toán vẫn in ra thứ gì đó, chỉ là câu trả lời sai. Một đầu ra trống gợi ý rõ ràng một trong những vấn đề triển khai sau: 

1.`solve()`được xác định nhưng không bao giờ được gọi. 
2. Chương trình thoát trước khi xử lý dữ liệu đầu vào. 
3. Đầu ra được tích lũy nhưng không bao giờ được in. 
4. Vòng lặp test-case không bao giờ được nhập vì`t`không được đọc chính xác. 
5. Một ngoại lệ xảy ra trước khi bất kỳ đầu ra nào được tạo ra nhưng truy nguyên bị bỏ qua. 

Vì mã nguồn không được cung cấp nên không thể tìm ra chính xác dòng bị lỗi. Tuy nhiên, đối với Codeforces 1605B, thuật toán chính xác đã được biết rõ và yêu cầu nhiều nhất một thao tác. 

Quan sát chính là: 

- Hãy để`target = sorted(s)`. 
- Thu thập tất cả các vị trí nơi`s[i] != target[i]`. 
- Nếu không có vị trí nào như vậy thì chuỗi đã được sắp xếp rồi, hãy trả lời`0`. 
- Ngược lại, tất cả các vị trí không khớp tạo thành một dãy con hợp lệ bao gồm một số`1`theo sau là một số`0`S. 
- Đảo ngược chuỗi con đó sẽ sắp xếp chuỗi theo đúng một thao tác. 

Một giải pháp hoàn chỉnh đúng là:```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        n = int(input())
        s = input().strip()

        target = ''.join(sorted(s))

        pos = []
        for i in range(n):
            if s[i] != target[i]:
                pos.append(i + 1)

        if not pos:
            print(0)
        else:
            print(1)
            print(len(pos), *pos)

solve()
```Chạy cái này trên mẫu: 

đầu vào```
3
7
0011111
5
10100
6
001000
```sản xuất```
0
1
4 1 3 4 5
1
3 3 5 6
```phù hợp với sản lượng dự kiến. 

Nếu bạn muốn chẩn đoán chính xác lỗi trong bản gửi ban đầu của mình, vui lòng dán mã Python thực tế đã tạo ra kết quả trống. Không có nguồn thì không có cách nào để xác định liệu vấn đề có bị thiếu hay không`solve()`gọi, quên`print`, xử lý đầu vào không chính xác hoặc điều gì khác.
