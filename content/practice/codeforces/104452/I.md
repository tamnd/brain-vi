---
title: "CF 104452I - Pharaoh hEx"
description: "Mỗi trường hợp kiểm thử là: - N - mảng a có kích thước N - K - rồi K giá trị truy vấn Vì vậy cấu trúc nghiêm ngặt: Không có sự mơ hồ, không có nhóm ẩn, không có nhiều trường hợp kiểm thử."
date: "2026-06-30T14:46:47+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104452
codeforces_index: "I"
codeforces_contest_name: "ICPC Central Russia Regional Contest - 2020"
rating: 0
weight: 104452
solve_time_s: 200
verified: false
draft: false
---

[CF 104452I - Pharaoh hEx](https://codeforces.com/problemset/problem/104452/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 20s 
**Đã xác minh:** không 

## Giải pháp 
## 1. Đầu vào thực sự đảm bảo điều gì 

Mỗi trường hợp thử nghiệm là: 

-`N`- mảng`a`kích thước`N`-`K`- sau đó`K`giá trị truy vấn 

Vì vậy, cấu trúc là nghiêm ngặt:```
N
a1 a2 ... aN
K
L1
L2
...
LK
```Không có sự mơ hồ, không có nhóm ẩn, không có nhiều trường hợp thử nghiệm. 

## 2. Tại sao các phiên bản trước của bạn bị lỗi 

Mẫu lỗi điển hình tạo ra **đầu ra trống** là: 

### Trường hợp A: đọc sai tất cả dữ liệu đầu vào 

Sử dụng một cái gì đó như:```python
data = sys.stdin.read().split()
```nhưng sau đó quản lý không tốt việc nâng cao chỉ mục hoặc bỏ qua hoàn toàn vòng lặp truy vấn khi các chỉ mục nằm ngoài phạm vi. 

### Trường hợp B: đứt sớm do giới hạn vòng lặp không đúng 

Ví dụ:```python
for _ in range(n):   # wrong variable used instead of k
```Điều này dẫn đến: 

- không xử lý truy vấn 
- hoặc sử dụng sai phần đầu vào 
- dẫn đến sự chấm dứt im lặng 

### Trường hợp C: đường dẫn thực hiện in bị mờ hoặc thiếu 

Nếu logic nằm trong một điều kiện như:```python
if cnt > 0:
    print(...)
```sau đó khi tất cả các giá trị rơi vào nhánh đối diện, không có gì được in ra. 

Nhưng Mẫu 2 chứng minh điều đó cũng không nên xảy ra. 

Vì vậy, vấn đề thực sự là: 

> vòng lặp truy vấn không chạy chính xác do phân tích cú pháp không chính xác hoặc các biến bị ghi đè. 

## 3. Mô hình suy luận rõ ràng, đúng đắn 

Chúng tôi KHÔNG cần bất kỳ thủ thuật nào ngoài: 

- sắp xếp mảng 
- tổng tiền tố 
- tìm kiếm nhị phân cho mỗi truy vấn 

Logic ổn định. 

Điều duy nhất cần phải sửa là **xử lý đầu vào mạnh mẽ và cấu trúc vòng lặp**. 

## 4. Giải pháp đúng (phân tích cú pháp sạch + an toàn) 

Phiên bản này tránh mọi hoạt động lập chỉ mục dễ vỡ và đảm bảo thực thi chính xác:```python
import sys
input = sys.stdin.readline
from bisect import bisect_left

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    a.sort()

    pref = [0] * (n + 1)
    for i in range(n):
        pref[i + 1] = pref[i] + a[i]

    total = pref[n]

    k = int(input())
    for _ in range(k):
        L = int(input())
        idx = bisect_left(a, L)
        cnt = n - idx
        if cnt == 0:
            print(0)
            continue
        sum_ge = total - pref[idx]
        print(sum_ge - cnt * L)

if __name__ == "__main__":
    solve()
```## 5. Điều gì thực sự đã được sửa 

Sự điều chỉnh quan trọng không mang tính toán học mà mang tính cấu trúc: 

Giải pháp hiện nay đảm bảo: 

- chính xác một lần đọc cho`n`- chính xác một lần đọc cho mảng 
- chính xác một lần đọc cho`k`- chính xác`k`lần lặp lại 
- không có lỗi lập chỉ mục bộ đệm chia sẻ 
- không bỏ qua vòng lặp im lặng 

## 6. Tại sao cả hai mẫu đều hoạt động 

### Mẫu 1```
0 0 0 0
L = 0,1,2
```Mọi giá trị đều bằng 0, vì vậy: 

- với L = 0 → tất cả đóng góp 0 
- với L > 0 → vẫn 0 

Đầu ra là:```
0
0
0
```### Mẫu 2```
4 0 2 1 2
queries 0..6
```Đã sắp xếp:```
0 1 2 2 4
```Mỗi truy vấn đánh giá chính xác sự đóng góp của hậu tố bằng cách sử dụng tổng tiền tố, tạo ra:```
9 5 2 1 0 0 0
```## 7. Bài học rút ra 

Loại mẫu lỗi này hầu như không bao giờ liên quan đến thuật toán. 

Nó đến từ: 

> giới hạn vòng lặp không chính xác hoặc mức tiêu thụ đầu vào bị hỏng khiến vòng lặp tính toán không bao giờ được thực thi. 

Sau khi phân tích cú pháp đầu vào được căn chỉnh chặt chẽ với định dạng, giải pháp tìm kiếm nhị phân tổng tiền tố + nhị phân sẽ hoàn toàn ổn định.
