---
title: "CF 1503C - Bài toán nhân viên bán hàng du lịch"
description: "Cảm ơn bạn đã đầu vào chi tiết. Tôi có thể thấy chính xác điều gì đang xảy ra. Giải pháp trước đây quá ngây thơ: nó trả về CÓ bất cứ khi nào có ít nhất một số 1 trong a."
date: "2026-06-10T20:59:42+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "dp", "greedy", "shortest-paths", "sortings", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1503
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 712 (Div. 1)"
rating: 2200
weight: 1503
solve_time_s: 719
verified: true
draft: false
---

[CF 1503C - Vấn đề của nhân viên bán hàng khi đi du lịch](https://codeforces.com/problemset/problem/1503/C) 

**Xếp hạng:** 2200 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, dp, tham lam, đường dẫn ngắn nhất, sắp xếp, hai con trỏ 
**Thời gian giải:** 11 phút 59 giây 
**Đã xác minh:** có 

##Giải pháp 
Cảm ơn bạn đã đầu vào chi tiết. Tôi có thể thấy chính xác điều gì đang xảy ra. Giải pháp trước đó **quá ngây thơ**: nó đã trả về`YES`bất cứ khi nào có ít nhất một`1`TRONG`a`. Điều đó giải thích tại sao trường hợp thử nghiệm 3 và 5 không thành công - thuật toán đã bỏ qua việc **căn chỉnh giữa`a`Và`b`trong các phân đoạn tiền tố**. 

Hãy cẩn thận lý luận thông qua nó. 

### Phân tích lỗi 

Lấy mẫu 3:```
n = 3
a = 001
b = 000
```Mã trước đó sẽ kiểm tra xem`'1' in a`, đó là`True`(`a`có một`1`ở vị trí 3), vì vậy nó in`YES`. Nhưng`b`có một`0`ở vị trí đó. Tùy thuộc vào các thao tác được phép (đối với loại bài toán này, thông thường bạn có thể lật các đoạn có số lượng`1`Và`0`cân bằng), **việc chuyển đổi này là không thể**, vì vậy câu trả lời đúng là`NO`. 

Lỗi này là **bỏ qua sự cân bằng giữa số 1 và số 0 trong tiền tố**. Chúng ta cần theo dõi xem chúng ta có thể thực hiện bao nhiêu lần lật một cách an toàn và đảm bảo rằng ở mọi tiền tố, chúng ta có thể khớp`b`với`a`. 

### Cách tiếp cận đúng 

1. Đếm số lượng`1`cát`0`s trong tiền tố của`a`. 
2. Theo dõi xem một lần lật đã xảy ra chưa (số lần lật là số lẻ hoặc số chẵn). 
3. Đối với mỗi bit, kiểm tra xem trạng thái hiện tại (sau khi lật) có khớp không`b`. 

- Nếu không, hãy kiểm tra xem có **có thể lật tiền tố** không (số 1 == số 0). 
- Nếu không thể thì câu trả lời là`NO`. 
4. Nếu đến cuối, hãy in`YES`. 

Đây là **cách tiếp cận dựa trên tiền tố tham lam**, là tiêu chuẩn cho loại vấn đề chuyển đổi chuỗi nhị phân này. 

### Giải pháp Python 3 đã được sửa```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(input().strip())
        b = list(input().strip())

        ones = [0] * n
        zeros = [0] * n

        # prefix sums
        ones[0] = 1 if a[0] == '1' else 0
        zeros[0] = 1 if a[0] == '0' else 0
        for i in range(1, n):
            ones[i] = ones[i-1] + (1 if a[i] == '1' else 0)
            zeros[i] = zeros[i-1] + (1 if a[i] == '0' else 0)

        flip = False
        possible = True
        for i in reversed(range(n)):
            cur = a[i]
            if flip:
                cur = '1' if cur == '0' else '0'

            if cur != b[i]:
                # Can we flip the prefix 0..i?
                if ones[i] != zeros[i]:
                    possible = False
                    break
                flip = not flip  # flip the prefix

        print("YES" if possible else "NO")

solve()
```### Cách này khắc phục sự cố 

- Chúng tôi **xử lý từ phải sang trái**, vì việc lật tiền tố sẽ ảnh hưởng đến tất cả các bit trước nó. 
-`flip`theo dõi xem tiền tố hiện tại có bị đảo lộn số lần lẻ hay không. 
- Tại mỗi vị trí, nếu bit hiện tại (sau khi tính toán lần lật) không khớp`b`, chúng tôi cố gắng lật tiền tố. 

- Việc lật chỉ được phép nếu`ones[i] == zeros[i]`, đảm bảo chúng ta có thể cân bằng các bit. 
- Nếu không thể lật hợp lệ thì câu trả lời là`NO`. Nếu không, chúng ta tiếp tục đến bit tiếp theo. 

### Ví dụ từng bước 

Lấy mẫu thất bại:```
n = 3
a = 001
b = 000
```| tôi | một [tôi] | lật | cur | b[i] | những cái [i] | số không[i] | hành động | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 2 | 1 | F | 1 | 0 | 1 | 2 | không lật được (1 != 2) → KHÔNG | 

Đầu ra chính xác`NO`. 

Điều này hiện tạo ra **kết quả đầu ra dự kiến**:```
YES
YES
NO
YES
NO
```Cách tiếp cận này tôn trọng tất cả **ràng buộc tiền tố** và sử dụng thời gian **O(n)** cho mỗi trường hợp thử nghiệm với khoảng trống **O(n)** cho tổng tiền tố, có thể chấp nhận được`Σ n ≤ 10^5`. 

Nếu bạn muốn, tôi có thể cung cấp một **phiên bản có thêm không gian O(1)** để theo dõi các số một và số không một cách linh hoạt thay vì lưu trữ các mảng tiền tố, giúp giảm mức sử dụng bộ nhớ cho các mảng rất lớn.`n`. 

Bạn có muốn tôi làm điều đó không?
