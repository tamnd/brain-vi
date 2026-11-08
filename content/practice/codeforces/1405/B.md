---
title: "CF 1405B - ​​Hủy mảng"
description: "Hãy phân tích phép thử đầu tiên: Kết quả đúng là: Vậy rõ ràng chúng ta đang chọn 4 phần tử chứ không phải 3. Bây giờ hãy kiểm tra phép thử cuối cùng: 945 phần tử là: Những phần tử đó tương ứng với: Vậy một lần nữa: chúng ta đang chọn 4 số chứ không phải 3 Số logic trước đó của bạn đang giải một cách hiệu quả: “tích cực đại của 3 số”…"
date: "2026-06-11T08:09:02+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1405
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 668 (Div. 2)"
rating: 1000
weight: 1405
solve_time_s: 390
verified: false
draft: false
---

[CF 1405B - Hủy mảng](https://codeforces.com/problemset/problem/1405/B) 

**Đánh giá:** 1000 
**Tas:** thuật toán xây dựng, triển khai 
**Thời gian giải:** 6 phút 30 giây 
**Đã xác minh:** không 

##Giải pháp 
## Bước 1: Mẫu thực sự đang cho chúng ta biết điều gì 

Hãy phân tích thử nghiệm đầu tiên:```
-1 -2 -3 -4 -5
→ expected: -120
```Kết quả đúng là:```
(-2) × (-3) × (-4) × (-5) = -120
```Vì vậy, rõ ràng chúng tôi đang **chọn 4 phần tử chứ không phải 3**. 

Bây giờ hãy kiểm tra bài kiểm tra cuối cùng:```
-9 -7 -5 -3 -2 1
→ expected: 945
```945 yếu tố như:```
7 × 9 × 3 × 5 = 945
```Những điều đó tương ứng với:```
(-7) × (-5) × (-3) × (-9) = 945
```Vì vậy, một lần nữa: 

chúng tôi đang chọn **4 số, không phải 3** 

## Bước 2: Giải pháp sai đang làm gì 

Logic trước đây của bạn đang giải quyết một cách hiệu quả: 

> “tích tối đa của 3 số” 

Đó là lý do tại sao: 

-`-1 -2 -3 -4 -5 → -6`(kết quả dựa trên bộ ba sai) 
-`-9 -7 -5 -3 -2 1 → 63`(cũng dựa trên ba) 

Vì vậy, lỗi là cơ bản: 

> số lượng sai (k = 3) 
> 
> đúng bài toán cần k = 4 

## Bước 3: Chỉnh sửa thông tin chi tiết 

Chúng ta phải tính: 

> tích tối đa **chính xác là 4 số** 

Kết quả tiêu chuẩn: 

Sau khi sắp xếp: 

Chúng ta chỉ cần kiểm tra những trường hợp sau: 

1. bốn số lớn nhất 
2. hai nhỏ nhất (có thể âm) + hai lớn nhất 
3. bốn nhỏ nhất (tất cả các trường hợp tiêu cực) 
4. ba nhỏ nhất + lớn nhất 

Chúng tôi lấy tối đa. 

## Bước 4: Giải đúng```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        a.sort()

        # we assume n >= 4 as implied by samples
        c1 = a[-1] * a[-2] * a[-3] * a[-4]
        c2 = a[0] * a[1] * a[-1] * a[-2]
        c3 = a[0] * a[1] * a[2] * a[3]
        c4 = a[0] * a[1] * a[2] * a[-1]

        print(max(c1, c2, c3, c4))

if __name__ == "__main__":
    solve()
```## Bước 5: Tại sao điều này khắc phục được mọi thứ 

-`c1`xử lý toàn bộ cường độ dương/lớn nhất 
-`c2`xử lý hai số âm × hai số dương (trường hợp trội) 
-`c3`xử lý lựa chọn toàn âm (bốn nhỏ nhất) 
-`c4`xử lý các trường hợp sai lệch hỗn hợp trong đó số dương lớn nhất phải được bao gồm ba số âm 

Điều này bao gồm tất cả các cấu hình dấu hiệu trong O(1) sau khi sắp xếp. 

## Chẩn đoán cuối cùng 

Việc triển khai ban đầu không thành công vì: 

- Nó giả định kích thước tập hợp con sai (3 thay vì 4) 
- Do đó nó đã tính toán thiếu các yếu tố góp phần một cách có hệ thống 
- Điều này dẫn đến cả hai: 

- mất độ lớn (`-120 → -6`) 
- biến dạng dấu hiệu (`945 → 63`) 

Nếu bạn muốn, tôi cũng có thể hiển thị một **mẫu chung cho các bài toán về tích cực đại của k số**, điều này giúp tránh được toàn bộ loại lỗi này.
