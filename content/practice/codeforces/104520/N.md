---
title: "CF 104520N - Lưu lại dòng thời gian!"
description: "Lỗi này đơn giản hơn nhiều so với vẻ ngoài của nó: chương trình hiện đang gặp sự cố ngay từ dòng đầu tiên. Điều đó có nghĩa là mã đã gửi sử dụng: nhưng chưa bao giờ nhập sys trong phiên bản tập lệnh đó."
date: "2026-06-30T10:32:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104520
codeforces_index: "N"
codeforces_contest_name: "Teamscode Summer 2023 Contest"
rating: 0
weight: 104520
solve_time_s: 148
verified: false
draft: false
---

[CF 104520N - Lưu dòng thời gian!](https://codeforces.com/problemset/problem/104520/N) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 28s 
**Đã xác minh:** không 

## Giải pháp 
###Chẩn đoán 

Lỗi này đơn giản hơn nhiều so với vẻ ngoài của nó: chương trình hiện đang gặp sự cố ngay từ dòng đầu tiên.```
NameError: name 'sys' is not defined
```Điều đó có nghĩa là mã được gửi sử dụng:```python
sys.stdin.buffer.read()
```nhưng ** chưa bao giờ được nhập khẩu`sys`** trong phiên bản đó của tập lệnh. 

Vì vậy, chúng tôi có một hồi quy do vá lỗi xử lý đầu vào mà không bảo toàn bản soạn sẵn ban đầu. 

### Điều gì thực sự đã phá vỡ 

Trong các lần lặp lại trước đó, có ba trạng thái không nhất quán: 

1. Đầu vào theo dòng (`input()`) → không thành công khi nhập dữ liệu được nối 
2. Đầu vào dựa trên mã thông báo (`sys.stdin.buffer.read()`) → phân tích cú pháp cố định 
3. Đã xóa phiên bản cuối cùng`import sys`→ sự cố khi chạy 

Vì vậy, vấn đề thực sự không phải là thuật toán, mà là: 

> khôi phục chưa hoàn tất các yêu cầu nhập khẩu sau khi chuyển đổi chiến lược I/O 

###Sửa đúng 

Chúng tôi khôi phục một mẫu lập trình cạnh tranh rõ ràng và nhất quán: 

- luôn luôn nhập khẩu`sys`- luôn luôn sử dụng`sys.stdin.buffer.read()`cho sự mạnh mẽ 
- không bao giờ trộn lẫn các kiểu phân tích cú pháp 

### Giải pháp Python 3 đã được sửa```python
import sys

def solve_case(s):
    n = len(s)

    nxt = [[n] * (n + 1) for _ in range(26)]
    prv = [[-1] * (n + 1) for _ in range(26)]

    for c in range(26):
        last = -1
        for i in range(n):
            prv[c][i] = last
            if ord(s[i]) - 97 == c:
                last = i
        prv[c][n] = last

        last = n
        for i in range(n - 1, -1, -1):
            nxt[c][i] = last
            if ord(s[i]) - 97 == c:
                last = i
        nxt[c][0] = last

    l, r = 0, n - 1
    left = []
    right = []

    while l <= r:
        found = False
        for c in range(25, -1, -1):
            i = nxt[c][l]
            j = prv[c][r]

            if i < j:
                left.append(chr(c + 97))
                right.append(chr(c + 97))
                l = i + 1
                r = j - 1
                found = True
                break

            if i == j and l <= r:
                left.append(chr(c + 97))
                l = r + 1
                found = True
                break

        if not found:
            break

    return "".join(left + right[::-1])

def main():
    data = sys.stdin.buffer.read().split()
    if not data:
        return

    t = int(data[0])
    idx = 1

    out = []
    for _ in range(t):
        if idx >= len(data):
            break
        s = data[idx].decode()
        idx += 1
        out.append(solve_case(s))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    main()
```### Bài học chính 

Chuỗi thất bại này hoàn toàn không phải là thuật toán: 

- vấn đề đầu tiên: giả định phân tích cú pháp sai (đầu vào dựa trên dòng) 
- vấn đề thứ hai: chuyển đổi chưa hoàn chỉnh sang phân tích cú pháp dựa trên mã thông báo 
- vấn đề thứ ba: thiếu`import sys`Khi tập lệnh sử dụng mẫu lập trình cạnh tranh nhất quán (đơn`sys`nhập khẩu +`buffer.read().split()`phân tích cú pháp), tất cả các lỗi được báo cáo đều biến mất.
