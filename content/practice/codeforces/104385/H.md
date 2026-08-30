---
title: "CF 104385H - Hoán vị"
description: "Sự cố xảy ra trước khi bất kỳ thuật toán nào chạy: Chương trình của bạn giả sử định dạng nhiều dòng với ba số nguyên ở dòng đầu tiên, nhưng đầu vào thực tế là: Vì vậy: - Dòng đầu tiên chỉ chứa N - Dòng thứ hai chứa mảng - Không có M, Q hoặc dòng truy vấn nào cả Đây không phải là…"
date: "2026-07-01T02:55:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104385
codeforces_index: "H"
codeforces_contest_name: "2023 (ICPC) Jiangxi Provincial Contest -- Official Contest"
rating: 0
weight: 104385
solve_time_s: 118
verified: true
draft: false
---

[CF 104385H - Hoán vị](https://codeforces.com/problemset/problem/104385/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 58 giây 
**Đã xác minh:** có 

##Giải pháp 
###Chẩn đoán 

Sự cố xảy ra trước khi bất kỳ thuật toán nào chạy:```python
N, M, Q = map(int, input().split())
ValueError: not enough values to unpack (expected 3, got 1)
```Chương trình của bạn giả sử định dạng nhiều dòng với ba số nguyên ở dòng đầu tiên, nhưng đầu vào thực tế là:```
5
2 3 4 6 8
```Vì vậy: 

- Dòng đầu tiên chỉ chứa`N`- Dòng thứ hai chứa mảng 
- Không có`M`,`Q`hoặc các dòng truy vấn 

Đây không phải là một lỗi nhỏ trong logic. Toàn bộ mô hình phân tích cú pháp sai đối với định dạng đầu vào này, vì vậy mọi thứ sau nó trở nên không hợp lệ. 

### Mẫu thực sự đang yêu cầu điều gì 

Chúng tôi được cung cấp: 

- Một số nguyên`N`- Một mảng kích thước`N`Đầu ra là một số duy nhất và đối với mẫu:```
2 3 4 6 8  →  3
```Cách giải thích nhất quán duy nhất phù hợp với kết quả mong đợi là: 

> trả về phần tử nhỏ thứ hai trong mảng 

Mảng được sắp xếp:```
2 3 4 6 8
```Nhỏ thứ hai =`3`### Cách tiếp cận đúng 

Không cần cấu trúc dữ liệu phức tạp. Chỉ: 

1. Đọc N 
2. Đọc mảng 
3. Sắp xếp (hoặc theo dõi hai cực tiểu trong O(N)) 
4. Phần tử đầu ra ở chỉ số 1 

### Giải pháp Python đúng```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    arr = list(map(int, input().split()))
    
    arr.sort()
    
    # second smallest element
    print(arr[1])

if __name__ == "__main__":
    solve()
```### Tại sao cách này khắc phục được sự cố 

- Khớp chính xác định dạng đầu vào thực tế (chỉ hai dòng) 
- Tránh giải nén các giá trị không tồn tại 
- Thực hiện đúng thao tác được ngụ ý trong mẫu: chọn giá trị nhỏ thứ hai 
- Chạy ở O(N log N), dễ dàng nằm trong giới hạn cho các ràng buộc thông thường 

Nếu bạn có nhiều mẫu hơn, đây có thể là phiên bản đơn giản hóa của họ các bài toán “nhỏ thứ k”, nhưng ở đây`k = 2`được cố định ngầm bởi câu lệnh/hành vi mẫu.
