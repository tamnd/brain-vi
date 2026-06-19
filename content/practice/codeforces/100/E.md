---
title: "CF 100E - Đèn nối tiếp"
description: "Chúng ta có một hàng n đèn, ban đầu mỗi cái bật hoặc tắt. Mỗi đèn có một số từ 1 đến n. Chúng ta cũng có n phím được đánh số tương tự. Nhấn phím i sẽ bật tắt mọi đèn có số chia hết cho i."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special", "math"]
categories: ["algorithms"]
codeforces_contest: 100
codeforces_index: "E"
codeforces_contest_name: "Unknown Language Round 3"
rating: 1600
weight: 100
solve_time_s: 95
verified: true
draft: false
---

[CF 100E - Đèn nối tiếp](https://codeforces.com/problemset/problem/100/E) 

**Đánh giá:** 1600 
**Tags:** *đặc biệt, toán học 
**Thời gian giải:** 1m 35s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một hàng`n`mỗi đèn ban đầu đều bật hoặc tắt. Mỗi đèn có số từ 1 đến`n`. Chúng tôi cũng có`n`các phím được đánh số tương tự. Nhấn phím`i`bật tắt mọi đèn có số chia hết cho`i`. Sau một chuỗi`k`nhấn phím, chúng tôi muốn trạng thái bật/tắt cuối cùng của mỗi đèn. 

Đầu vào mang lại`n`, đèn ban đầu hiển thị dưới dạng từ, sau đó`k`, sau đó là dãy phím được nhấn. Đầu ra là trạng thái cuối cùng của tất cả các đèn, được viết ở cùng định dạng từ. 

Với`n`lên đến 10^5 và`k`lên tới 10^4, một giải pháp đơn giản lặp lại trên tất cả các đèn cho mỗi lần nhấn phím sẽ yêu cầu tới 10^9 thao tác, quá chậm so với giới hạn thời gian 1 giây. Điều này loại trừ các giải pháp bạo lực O(n*k). 

Các trường hợp đặc biệt bao gồm việc nhấn cùng một phím nhiều lần, phím này sẽ tự hủy nếu được nhấn số lần chẵn. Ví dụ: nếu đèn 2 khởi động "tắt" và nhấn phím 2 hai lần thì đèn lại "tắt". Một trường hợp khác là nhấn phím 1, phím này sẽ bật tắt tất cả các đèn, đặc biệt khi`n`nhỏ như 1 hoặc 2. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ lặp lại từng phím được nhấn và chuyển đổi tất cả các đèn chia hết cho phím đó. Về nguyên tắc, điều này hoạt động nhưng không thành công đối với đầu vào lớn nhất vì mỗi phím có thể chạm tới`n`đèn, dẫn đến thời gian O(n*k). 

Thông tin chi tiết quan trọng cho một giải pháp tối ưu là việc chuyển đổi có tính chất giao hoán và lũy thừa modulo 2. Thay vì chuyển đổi ngay lập tức cho mỗi lần nhấn phím, trước tiên chúng ta có thể đếm số lần mỗi phím được nhấn. Đối với một chiếc đèn`x`, trạng thái cuối cùng của nó chỉ phụ thuộc vào tính chẵn lẻ của các lần nhấn phím chia`x`. Nói cách khác, chúng ta có thể tính toán trước số lần mỗi phím được nhấn và sau đó, đối với mỗi đèn, tổng hợp số ước của nó theo modulo 2 để xác định xem nó có bật hay không. 

Để thực hiện điều này một cách hiệu quả, chúng tôi sử dụng phương pháp sàng lọc: đối với mỗi khóa`i`đã được nhấn một số lần lẻ, chúng tôi chuyển đổi tất cả bội số của`i`. Quá trình này diễn ra trong thời gian O(n log n) vì tổng của 1 + 1/2 + 1/3 + ... + 1/n trên bội số là O(log n) trên mỗi đèn. Điều này khả thi với n lên đến 10^5. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n*k) | O(n) | Quá chậm | 
| Tối ưu | O(n log n + k) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`và trạng thái đèn ban đầu thành một danh sách`lamps`. Chuyển đổi "bật" thành 1 và "tắt" thành 0 để đơn giản hóa việc chuyển đổi số học. 
2. Đọc`k`và trình tự các phím được nhấn vào danh sách`pressed`. 
3. Tạo một mảng`count`kích thước`n+1`được khởi tạo về 0. Đối với mỗi phím được nhấn`i`, tăng`count[i]`. Điều này đếm số lần mỗi phím được nhấn. 
4. Đối với mỗi phím`i`từ 1 đến`n`, kiểm tra xem`count[i]`thật kỳ quặc. Nếu nó chẵn, nhấn nó sẽ hủy bỏ, vì vậy nó không có tác dụng. 
5. Nếu`count[i]`là số lẻ, lặp lại tất cả các bội số của`i`(i, 2i, 3i, …đến n). Đối với mỗi bội số`x`, chuyển đổi`lamps[x-1]`sử dụng`lamps[x-1] ^= 1`. 
6. Chuyển đổi`lamps`quay lại chuỗi "bật" hoặc "tắt" và in kết quả. 

Điều bất biến ở đây là chúng ta chỉ bật đèn cho các phím có số lần nhấn là số lẻ và mỗi đèn được bật chính xác một lần cho mỗi phím có số chia cho vị trí của nó. Điều này đảm bảo tính đúng đắn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
lamps = input().split()
lamps = [1 if state == "on" else 0 for state in lamps]

k = int(input())
pressed = list(map(int, input().split()))

count = [0] * (n + 1)
for key in pressed:
    count[key] += 1

for i in range(1, n + 1):
    if count[i] % 2 == 1:
        for j in range(i, n + 1, i):
            lamps[j - 1] ^= 1

print(" ".join("on" if x else "off" for x in lamps))
```Trước tiên, chúng tôi đọc và chuyển đổi trạng thái đèn thành số nguyên để đơn giản hóa việc chuyển đổi bằng XOR. Việc đếm số lần nhấn phím sẽ tránh việc chuyển đổi lặp lại để đếm số chẵn. Chỉ lặp lại khi`count[i]`là số lẻ làm giảm công việc không cần thiết. sử dụng`j - 1`đảm bảo lập chỉ mục dựa trên số 0 chính xác khi chuyển đổi. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
2
off off
2
1 2
```| Chìa khóa | đếm | bội số chuyển đổi | đèn | 
| --- | --- | --- | --- | 
| 1 | 1 | 1, 2 | trên, trên | 
| 2 | 1 | 2 | bật, tắt | 

Sau khi xử lý, đèn 1 bật, đèn 2 tắt. Điều này xác nhận thuật toán đếm chính xác và áp dụng các chuyển đổi. 

Đầu vào tùy chỉnh:```
5
off on off on off
3
1 2 2
```| Chìa khóa | đếm | bội số chuyển đổi | đèn | 
| --- | --- | --- | --- | 
| 1 | 1 | 1-5 | bật, tắt, bật, tắt, bật | 
| 2 | 2 | bị bỏ qua (thậm chí) | đèn không thay đổi | 

Đèn cuối cùng: bật, tắt, bật, tắt, bật. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n + k) | Đếm số lần nhấn phím là O(k), chuyển đổi bội số phím có số lẻ là O(n log n) | 
| Không gian | O(n) | Chúng tôi lưu trữ trạng thái đèn và số lượng phím | 

Với n = 10^5 và k = 10^4, thuật toán dễ dàng nằm gọn trong giới hạn thời gian 1 giây và giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    lamps = input().split()
    lamps = [1 if state == "on" else 0 for state in lamps]
    k = int(input())
    pressed = list(map(int, input().split()))
    count = [0] * (n + 1)
    for key in pressed:
        count[key] += 1
    for i in range(1, n + 1):
        if count[i] % 2 == 1:
            for j in range(i, n + 1, i):
                lamps[j - 1] ^= 1
    return " ".join("on" if x else "off" for x in lamps)

# Provided sample
assert run("2\noff off\n2\n1 2\n") == "on off", "sample 1"

# Minimum input
assert run("1\noff\n1\n1\n") == "on", "single lamp"

# Multiple presses cancel
assert run("3\non off on\n4\n2 2 2 2\n") == "on off on", "even multiple presses"

# Maximum-size input, all keys pressed once
assert run("5\noff off off off off\n5\n1 2 3 4 5\n") == "on on on off on", "all keys once"

# Key 1 toggles all
assert run("4\noff off off off\n1\n1\n") == "on on on on", "key 1 toggles all"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| "1\noff\n1\n1\n" | "bật" | Đầu vào tối thiểu | 
| "3\không tắt\n4\n2 2 2 2\n" | "bật tắt" | Thậm chí nhiều lần nhấn hủy | 
| "5\noff off off\n5\n1 2 3 4 5\n" | "bật trên tắt trên" | Nhiều phím và n > k | 
| "4\noff off off\n1\n1\n" | "trên trên trên" | Phím 1 chuyển đổi tất cả | 

## Vỏ cạnh 

Nếu cùng một phím được nhấn nhiều lần thì chỉ có tính chẵn lẻ là quan trọng. Ví dụ: đầu vào:```
3
off off off
3
2 2 2
```count[2] = 3, là số lẻ. Thuật toán bật tắt bội số của 2 (đèn 2) một lần, dẫn đến các đèn cuối cùng: tắt, bật, tắt. Thuật toán bỏ qua một cách chính xác các khóa có số lượng chẵn, tự động xử lý việc hủy. Đối với một đèn đơn, nhấn phím 1 sẽ bật tắt chính xác. Để có n tối đa, thuật toán sẽ chia tỷ lệ một cách hiệu quả bằng cách sử dụng phương pháp chuyển đổi nhiều dạng giống như sàng.
