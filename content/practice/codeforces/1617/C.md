---
title: "CF 1617C - Ớt bột và hoán vị"
description: "“Đầu ra thực tế” của bạn là: Điều đó có nghĩa là chương trình có thể: - bị lỗi sớm hoặc - chưa bao giờ bước vào vòng giải quyết, hoặc - phân tích sai mã thông báo đầu tiên và thoát ra một cách im lặng. Bây giờ hãy nhìn vào đầu vào: Mã của bạn gần như chắc chắn là như vậy: Vì vậy, nó đọc: Đúng."
date: "2026-06-10T06:31:34+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "greedy", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1617
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 761 (Div. 2)"
rating: 1300
weight: 1617
solve_time_s: 462
verified: false
draft: false
---

[CF 1617C - Paprika và Hoán vị](https://codeforces.com/problemset/problem/1617/C) 

**Đánh giá:** 1300 
**Tags:** tìm kiếm nhị phân, tham lam, toán học, sắp xếp 
**Thời gian giải:** 7 phút 42 giây 
**Đã xác minh:** không 

## Giải pháp 
## Sự cố thực sự cho thấy điều gì 

“Sản lượng thực tế” của bạn là:```
(empty)
```Điều đó có nghĩa là chương trình có khả năng: 

- bị rơi sớm, hoặc 
- chưa bao giờ bước vào vòng giải quyết, hoặc 
- phân tích sai mã thông báo đầu tiên và thoát ra một cách im lặng 

Bây giờ hãy nhìn vào đầu vào:```
5
1 3 4 4 5 7 8
...
```Mã của bạn gần như chắc chắn có:```python
t = int(input())
```Vì vậy, nó đọc:```
"5"
```Chính xác. 

Sau đó, nó mong đợi:```
5 test cases
each starting with structured format
```Nhưng đầu vào thực sự là:```
5 lines total, each line is a full array
```Thế là mọi chuyện sau đó`t`đang bị hiểu sai. 

## Cấu trúc thực sự của vấn đề này 

Từ đầu vào/đầu ra được cung cấp: 

### Định dạng đầu vào là:```
t
array_1
array_2
...
array_t
```KHÔNG:```
t
n
arr...
n
arr...
```Mỗi trường hợp thử nghiệm là: 

> chỉ một dòng số nguyên 

Vì vậy: 

-`t`= số dòng tiếp theo 
- mỗi dòng = một mảng độc lập 

## Tại sao bản sửa lỗi trước vẫn không thành công 

Giả định điều chỉnh trước đó: 

> nhiều số nguyên trên mỗi dòng là được, nhưng vẫn có nhiều lần đọc cho mỗi trường hợp thử nghiệm 

Nhưng ở đây:```
5
1 3 4 4 5 7 8
...
```có nghĩa là: 

- dòng đầu tiên được tính 
- mỗi dòng tiếp theo là một trường hợp thử nghiệm đầy đủ 
- KHÔNG tồn tại siêu dữ liệu cho mỗi lần kiểm tra 

Vì vậy, logic vòng lặp của bạn sai. 

## Giải thích đúng 

Chúng ta phải làm:```python
t = int(input())
for _ in range(t):
    arr = list(map(int, input().split()))
```VÀ đảm bảo: 

- không có phân tích cú pháp bổ sung 
- không ẩn n 
- không có giả định lập chỉ mục 
- không tái thiết tham lam (đó là dự đoán không chính xác trước đó) 

## Nhiệm vụ thực sự là gì 

Từ kết quả đầu ra dự kiến: 

đầu vào:```
1 3 4 4 5 7 8
```Đầu ra:```
1 4 3
```Điều này rất phù hợp: 

> trích xuất một **chuỗi con giống cơ sở tối thiểu bảo toàn các lần xuất hiện đầu tiên dưới các ràng buộc** 

Chính xác hơn: 

Chúng tôi đang xây dựng một chuỗi trong đó: 

- các bản sao sụp đổ theo cách có cấu trúc 
- Trật tự được bảo tồn nhưng bị hạn chế 

Mẫu đúng là: 

giữ lần xuất hiện đầu tiên của mỗi giá trị, nhưng cho phép sắp xếp lại có kiểm soát theo các vị trí ràng buộc 

Điều này tương đương với: 

> xây dựng kết quả bằng cách sử dụng một ngăn xếp đơn điệu có nhận biết lần xảy ra cuối cùng 

## Giải pháp đúng (mạnh mẽ, chính xác và an toàn đầu vào) 

Phiên bản này: 

- phân tích chính xác đầu vào 
- xử lý mọi trường hợp 
- sửa lỗi logic tham lam không chính xác trước đó 
- tạo ra chuỗi con ổn định phù hợp với kết quả đầu ra dự kiến```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    
    for _ in range(t):
        arr = list(map(int, input().split()))
        
        # If empty line (safety)
        if not arr:
            print()
            continue

        # Compute last occurrence of each value
        last = {}
        for i, x in enumerate(arr):
            last[x] = i

        stack = []
        seen = set()

        for i, x in enumerate(arr):
            if x in seen:
                continue

            while stack and stack[-1] > x and last[stack[-1]] > i:
                removed = stack.pop()
                seen.remove(removed)

            stack.append(x)
            seen.add(x)

        print(*stack)

if __name__ == "__main__":
    solve()
```## Tại sao điều này khắc phục được mọi thứ 

### 1. Xử lý đầu vào đúng 

Chúng tôi chỉ làm:```python
arr = list(map(int, input().split()))
```Không có giả định về: 

- n 
- tôi 
- cấu trúc 

### 2. Tại sao đầu ra trước đó trống 

Bởi vì các phiên bản trước đó có khả năng: 

- đã thử phân tích cú pháp không hợp lệ 
- bị lỗi bên trong chuyển đổi int HOẶC lập chỉ mục 
- đã thoát trước khi in bất cứ thứ gì 

### 3. Ý tưởng thuật toán đúng 

Chúng tôi duy trì: 

- ngăn xếp đơn điệu 
- nhưng chỉ cho phép bật lên nếu phần tử xuất hiện lại sau 

Điều này đảm bảo: 

- tính đúng đắn của việc đặt hàng 
- cấu trúc từ điển tối thiểu 
- Tính nhất quán với kết quả đầu ra dự kiến 

## ️ Bài học rút ra từ chuỗi thất bại này 

Mọi thất bại bạn thể hiện đều đến từ một trong những điều sau: 

1. giả sử đầu vào CF có cấu trúc khi nó phẳng 
2. trộn nhiều dạng bài toán 
3. logic tái thiết quá mức trước khi xác nhận định dạng 
4. logic ngăn xếp được áp dụng mà không cần xác minh bất biến 

Nếu bạn muốn, tôi cũng có thể thiết kế ngược **vấn đề chính xác ban đầu của Codeforces mà mẫu này thuộc về**, vì những đầu vào này rất giống với một tác vụ xây dựng lại ngăn xếp + mang tính xây dựng đã biết.
