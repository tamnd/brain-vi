---
title: "CF 2051E - Giá Tốt Nhất"
description: "Bản thân thuật toán là chính xác. Triệu chứng là chương trình không xuất ra kết quả nào cả, không phải là xuất ra sai. Điều đó có nghĩa là lỗi không nằm trong logic để xác định đạt/không đạt. Nó nằm trong cấu trúc chương trình."
date: "2026-06-08T08:43:13+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "data-structures", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 2051
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 995 (Div. 3)"
rating: 1600
weight: 2051
solve_time_s: 175
verified: false
draft: false
---

[CF 2051E - Giá tốt nhất](https://codeforces.com/problemset/problem/2051/E) 

**Đánh giá:** 1600 
**Tags:** tìm kiếm nhị phân, sức mạnh vũ phu, cấu trúc dữ liệu, tham lam, sắp xếp 
**Thời gian giải:** 2m 55s 
**Đã xác minh:** không 

##Giải pháp 
Bản thân thuật toán là chính xác. Triệu chứng là chương trình **không có kết quả nào**, không phải kết quả đầu ra không chính xác. 

Điều đó có nghĩa là lỗi không nằm trong logic để xác định đạt/không đạt. Nó nằm trong cấu trúc chương trình. 

Một nguyên nhân rất phổ biến là giải pháp được viết bên trong một hàm như:```python
def solve():
    ...
```nhưng chức năng không bao giờ được gọi. Trong trường hợp đó, chương trình không đọc gì, không viết gì và thẩm phán thấy một kết quả trống:```
Expected:
0100
0000
1111
10

Actual:
```Truy tìm mẫu thông qua thuật toán dự định: 

### Trường hợp thử nghiệm 1```
n=4, m=4, k=3
known={1,3,4}
```Câu hỏi chưa biết:```
{2}
```Có đúng một câu hỏi chưa biết, đó là 2. 

Một danh sách có thể vượt qua nếu câu hỏi bị bỏ qua là 2.```
a = [1,2,3,4]
```Kết quả:```
0100
```### Trường hợp thử nghiệm 2```
n=5, k=3
known={1,3,4}
```Câu hỏi chưa biết:```
{2,5}
```Có ít nhất hai câu hỏi chưa biết. Loại bỏ một câu hỏi không thể loại bỏ cả hai. 

Kết quả:```
0000
```### Trường hợp thử nghiệm 3```
k=n
```Tất cả các câu hỏi đều được biết. 

Kết quả:```
1111
```### Trường hợp thử nghiệm 4```
n=2
known={2}
```Câu hỏi không xác định:```
{1}
```Chỉ có danh sách bỏ qua câu hỏi 1 là có thể vượt qua. 

Kết quả:```
10
```Vì vậy, thuật toán là tốt. Cách khắc phục chỉ đơn giản là cung cấp một giải pháp thực thi hoàn chỉnh và gọi`solve()`.```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    answers = []

    for _ in range(t):
        n, m, k = map(int, input().split())
        a = list(map(int, input().split()))
        q = list(map(int, input().split()))

        known = set(q)
        missing = n - k

        if missing == 0:
            answers.append("1" * m)
            continue

        if missing >= 2:
            answers.append("0" * m)
            continue

        # Exactly one unknown question.
        unknown = -1
        for x in range(1, n + 1):
            if x not in known:
                unknown = x
                break

        res = []
        for x in a:
            if x == unknown:
                res.append('1')
            else:
                res.append('0')

        answers.append(''.join(res))

    sys.stdout.write("\n".join(answers))

if __name__ == "__main__":
    solve()
```Điều này bảo tồn mục đích dự định$O(n+m)$theo cách tiếp cận trường hợp thử nghiệm và xử lý chính xác tất cả các trường hợp biên.
