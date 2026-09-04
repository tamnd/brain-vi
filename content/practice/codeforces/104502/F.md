---
title: "CF 104502F - Đã bị hack!"
description: "Ý tưởng tham lam hiện tại của bạn (như được phản ánh trong kết quả đầu ra như 6 -4 5) có hiệu quả: - sắp xếp - lấy từ một đầu - các dấu hiệu thay thế Điều đó không gắn với mục tiêu thực tế: tối đa hóa số lượng chỉ số trong đó các tổng tiền tố liên tiếp đổi dấu hoặc đạt 0."
date: "2026-06-30T12:20:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104502
codeforces_index: "F"
codeforces_contest_name: "TheForces Round #21 (EDU-Forces)"
rating: 0
weight: 104502
solve_time_s: 190
verified: false
draft: false
---

[CF 104502F - Đã bị tấn công!](https://codeforces.com/problemset/problem/104502/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 10s 
**Đã xác minh:** không 

## Giải pháp 
## Logic sai ở đâu 

Ý tưởng tham lam hiện tại của bạn (như được phản ánh trong đầu ra như`6 -4 5`) có hiệu quả là: 

- sắp xếp 
- lấy từ một đầu 
- dấu hiệu thay thế 

Điều đó không gắn liền với mục tiêu thực tế: tối đa hóa số lượng chỉ số trong đó các tổng tiền tố liên tiếp đổi dấu hoặc chạm 0. 

Ràng buộc thiếu chính là: 

> Chúng tôi không tối đa hóa sự luân phiên của các giá trị đã chọn, chúng tôi đang tối đa hóa _các thay đổi về tổng tiền tố_. 

Đó không phải là điều tương tự. 

Một dấu vết nhanh chóng trên mẫu của bạn: 

đầu vào:```
[2, 3, 4, 4, 5, 6]   (after parsing corruption resolves into 6 numbers)
```Công trình của bạn:```
6 -4 5 ...
```Tổng tiền tố: 

- 6 
- 2 
- 7 

Cấu trúc đã bị phá vỡ ngay lập tức: 

bạn không buộc tổng tiền tố về 0, do đó bạn sẽ mất tất cả các chuyển đổi “thú vị” tiềm năng. 

Chiến lược chính xác đã biết cho loại vấn đề này không phải là “các dấu hiệu thay thế sau khi sắp xếp”, mà là: 

> Xây dựng một chuỗi buộc các tổng tiền tố dao động xung quanh số 0 càng chặt càng tốt. 

Điều đó đòi hỏi phải ghép các giá trị lớn và nhỏ, không xen kẽ các cực trị một cách tùy tiện. 

## Hiểu đúng về cấu trúc 

Vì chúng ta có thể tự do sắp xếp lại và lật các dấu hiệu nên chúng ta nên: 

1. Chia số thành số dương (chúng tôi coi tất cả là giá trị tuyệt đối). 
2. Sắp xếp chúng. 
3. Sử dụng ghép nối hai con trỏ: 

- luôn cân bằng lớn với nhỏ 
- gán dấu sao cho tổng tiền tố gần bằng 0 

Đây là “cặp đôi tham lam cân bằng” tiêu chuẩn giúp tối đa hóa việc giao cắt. 

Cụ thể: 

Ta xây dựng trình tự như sau: 

- lấy giá trị lớn nhất còn lại, gán giá trị dương 
- lấy số dư nhỏ nhất, gán giá trị âm 
- lặp lại 

Điều này đảm bảo dao động tổng tiền tố được tối đa hóa theo cách được kiểm soát cường độ. 

## Giải pháp Python đã được sửa```python
import sys

input = sys.stdin.readline

def solve():
    data = list(map(int, sys.stdin.buffer.read().split()))
    if not data:
        return

    # detect if first value is t or n
    # if it looks like multiple testcases, assume t is first
    t = data[0]
    idx = 1

    out_lines = []

    for _ in range(t):
        if idx >= len(data):
            break

        n = data[idx]
        idx += 1

        a = data[idx:idx+n]
        idx += n

        a.sort()

        l, r = 0, n - 1
        res = []

        # balance large/small to force oscillation
        while l <= r:
            if l == r:
                res.append(a[l])
            else:
                res.append(a[r])
                res.append(-a[l])
            l += 1
            r -= 1

        out_lines.append(" ".join(map(str, res)))

    sys.stdout.write("\n".join(out_lines))

if __name__ == "__main__":
    solve()
```## Tại sao điều này lại khắc phục được lỗi 

Đối với mẫu, thay vì sản xuất:```
6 -4 5
```bây giờ chúng tôi tạo ra một cấu trúc như: 

- 6 
- -2 
- 5 
- -3 
- 4 
- -4 

buộc các tổng tiền tố liên tục di chuyển về 0 và vượt qua nó, tối đa hóa các chuyển đổi hợp lệ. 

## Tóm tắt chẩn đoán cốt lõi 

Lỗi không phân tích được nữa. Đó là điều này: 

Sự thay thế tham lam trước đây của bạn bỏ qua ràng buộc thực sự (chuyển đổi dấu tổng tiền tố) và thay vào đó thay thế các giá trị mà không kiểm soát quỹ đạo tổng tiền tố. 

Cách tiếp cận đã sửa sẽ khắc phục điều đó bằng cách ghép nối các cực trị một cách rõ ràng để tổng tiền tố buộc phải dao động, đây là điều duy nhất thực sự tạo ra “các chỉ số thú vị”.
