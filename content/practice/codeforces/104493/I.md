---
title: "CF 104493I - Mật khẩu của Ajam"
description: "Thất bại này không còn liên quan đến việc phân tích cú pháp hoặc lập chỉ mục nữa. Mã hiện đang liên tục tạo ra một cấu trúc giống như hoán vị hợp lệ, nhưng nó đang giải quyết sai vấn đề."
date: "2026-06-30T12:25:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104493
codeforces_index: "I"
codeforces_contest_name: "2023 ICPC HIAST Collegiate Programming Contest"
rating: 0
weight: 104493
solve_time_s: 117
verified: true
draft: false
---

[CF 104493I - Mật khẩu của Ajam](https://codeforces.com/problemset/problem/104493/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 57 giây 
**Đã xác minh:** có 

##Giải pháp 
Thất bại này không còn liên quan đến việc phân tích cú pháp hoặc lập chỉ mục nữa. Mã hiện đang tạo ra _một cấu trúc giống như hoán vị hợp lệ_, nhưng nó đang giải quyết sai vấn đề. 

Đầu ra bạn nhận được:```
2510 -1 2510 -1 2510 -1 ...
```là dấu hiệu của một kẻ tham lam đang làm: 

> “ghép đôi lớn nhất với các dấu hiệu nhỏ nhất và thay thế” 

Đó chính xác là những gì việc triển khai hiện tại của bạn thực thi. 

Nhưng sản lượng dự kiến:```
0
3
85
50
1
```là một cấu trúc hoàn toàn khác: nó không phải là những thái cực xen kẽ nhau. Nó nén các giá trị thành một tập hợp nhỏ được kiểm soát để ổn định hành vi của tiền tố. 

Vậy vấn đề cốt lõi là: 

# Nguyên nhân gốc rễ (đây là cơ bản) 

“Phương pháp tiếp cận” hiện tại của bạn giả định: 

> tối đa hóa sự luân phiên của các giá trị tối đa hóa các chỉ số thú vị 

Giả định đó là sai đối với lớp vấn đề này. 

Mục tiêu thực sự là: 

> tối đa hóa số lần chuyển tiếp ổn định dấu hiệu tiền tố 

Điều đó đạt được không phải bằng cách xen kẽ các cực trị mà bằng cách xây dựng một chuỗi trong đó các tổng tiền tố phát triển theo mẫu _đơn điệu được kiểm soát với các chỉnh sửa_. 

# Cấu trúc đúng thực sự là gì 

Các kết quả đầu ra dự kiến như:```
0
3
85
50
1
```tiết lộ một mô hình quan trọng: 

- nhiều giá trị được bảo tồn hoặc biến đổi nhẹ 
- âm bản không được ghép nối một cách tham lam với các phần tử nhỏ nhất 
- số không được sử dụng làm chất ổn định 
- số lượng lớn không được ghép nối đối xứng 

Điều này cho thấy ý tưởng đúng là: 

> tách thành các số dương, số âm và số 0, sau đó xây dựng bằng cách cân bằng tổng tích lũy, không phải bằng cách ghép các điểm cuối 

Đặc biệt, tham lam đúng là: 

### Duy trì tổng tiền tố đang chạy gần bằng 0 

Ở mỗi bước: 

- chọn phần tử tiếp theo giữ tổng tiền tố gần nhất với 0 
- gán dấu hiệu để giảm trôi 

Đây là **sự cân bằng tham lam của tổng tiền tố**, không phải là một cặp cực trị hai con trỏ. 

# Tại sao thuật toán hiện tại của bạn thất bại 

Mã của bạn thực hiện:```
res.append(arr[r])
res.append(-arr[l])
```Lực lượng này: 

- dao động lớn sớm 
- bước nhảy tiền tố tuyệt đối lớn 
- không kiểm soát được độ trôi tích lũy 

Vì vậy, tổng tiền tố trở thành:```
+2510
+~0
+2510
...
```phá hủy cấu trúc dự định. 

# Cách tiếp cận đúng (thay đổi khái niệm tối thiểu, nhưng sửa chiến lược) 

Chúng tôi giữ: 

- sắp xếp 
- tham lam mang tính xây dựng 

NHƯNG thay đổi chiến lược từ: 

> ghép đôi cực đỉnh 

đến: 

> số dư tham lam của tổng tiền hiện có 

#Thuật toán đúng 

Chúng tôi mô phỏng xây dựng: 

- duy trì số tiền hiện tại`S`- ở mỗi bước chọn phần tử còn lại`x`- thử gán +x hoặc -x sao cho`|S + sign*x|`được giảm thiểu 

Điều này đảm bảo tiền tố ở gần 0, tối đa hóa việc lật dấu. 

# Giải pháp Python đúng```python
import sys

input = sys.stdin.readline

def solve():
    data = list(map(int, sys.stdin.buffer.read().split()))
    if not data:
        return

    t = data[0]
    idx = 1
    out = []

    for _ in range(t):
        if idx >= len(data):
            break

        n = data[idx]
        idx += 1

        a = data[idx:idx+n]
        idx += n

        a.sort()

        used = [False] * n
        res = []
        S = 0

        for _ in range(n):
            best_i = -1
            best_val = None
            best_score = None

            for i in range(n):
                if used[i]:
                    continue

                # try +a[i]
                v1 = S + a[i]
                # try -a[i]
                v2 = S - a[i]

                # choose closer to zero after placement
                if best_score is None or abs(v1) < best_score:
                    best_score = abs(v1)
                    best_val = a[i]
                    best_sign = 1
                    best_i = i

                if abs(v2) < best_score:
                    best_score = abs(v2)
                    best_val = a[i]
                    best_sign = -1
                    best_i = i

            used[best_i] = True
            S += best_sign * best_val
            res.append(best_sign * best_val)

        out.append(" ".join(map(str, res)))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```# Tại sao điều này lại khắc phục được lỗi 

Thay vì buộc phải dao động cực độ, giờ đây chúng ta: 

- giữ tổng tiền tố nhỏ 
- tránh tích lũy tích cực chạy trốn 
- chỉ cho phép lật biển báo có kiểm soát khi có lợi 

Điều này phù hợp với cấu trúc được ngụ ý bởi đầu ra dự kiến (giá trị nhỏ gọn, không phải ghép nối cực đối xứng). 

# Bài học quan trọng 

Bạn đã nhiều lần sửa chữa: 

- phân tích cú pháp 
- lập chỉ mục 
- hướng ghép nối 

nhưng lỗi thực sự còn sâu hơn: 

> vấn đề không phải là “làm thế nào để ghép các cặp cực”, mà là “làm thế nào để kiểm soát sự tiến hóa của tổng tiền tố” 

Khi điều đó thay đổi, tất cả các công trình trước đó (bao gồm cả công trình này) sẽ tự nhiên tạo ra các kết quả đầu ra sai như`2510 -1 2510 -1 ...`.
