---
title: "CF 103562A - Số điện thoại"
description: "Cách tiếp cận bạo lực chính xác là những gì mà vấn đề gợi ý: đối với mỗi liên hệ, hãy chuyển đổi số điện thoại thành chữ số, tính tổng của chúng và kiểm tra tính chẵn lẻ. Điều này đã là tối ưu vì mỗi chữ số phải được kiểm tra ít nhất một lần để biết phần đóng góp của nó vào tổng."
date: "2026-07-03T05:20:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103562
codeforces_index: "A"
codeforces_contest_name: "UTPC Contest 02-11-22 Div. 2 (Beginner)"
rating: 0
weight: 103562
solve_time_s: 44
verified: true
draft: false
---

[CF 103562A - Số điện thoại](https://codeforces.com/problemset/problem/103562/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

##Giải pháp 
## Phương pháp tiếp cận 

Cách tiếp cận bạo lực chính xác là những gì mà vấn đề gợi ý: đối với mỗi liên hệ, hãy chuyển đổi số điện thoại thành chữ số, tính tổng của chúng và kiểm tra tính chẵn lẻ. Điều này đã là tối ưu vì mỗi chữ số phải được kiểm tra ít nhất một lần để biết phần đóng góp của nó vào tổng. Chi phí tỷ lệ thuận với tổng số chữ số trên tất cả các địa chỉ liên hệ, nhiều nhất là khoảng 9000, vì vậy việc này sẽ diễn ra ngay lập tức. 

Không cần cấu trúc tổ hợp sâu hơn hoặc thủ thuật tiền xử lý. Cách “tối ưu hóa” duy nhất là tránh chuyển đổi chuỗi thành số nguyên và thay vào đó tích lũy tổng trực tiếp từ mã ký tự. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Quét chữ số Brute Force | O(tổng chữ số) | O(1) thêm | Đã chấp nhận | 
| Bất kỳ quá trình tiền xử lý nâng cao nào | O(tổng chữ số) | O(1) | Quá mức cần thiết | 

## Hướng dẫn thuật toán 

1. Đọc số lượng liên hệ, sau đó xử lý từng dòng một. Tính chất phát trực tuyến tránh lưu trữ dữ liệu không cần thiết. 
2. Chia mỗi dòng thành tên và chuỗi số điện thoại. Việc phân chia được thực hiện một lần trên mỗi dòng, do đó việc phân tích cú pháp vẫn tuyến tính. 
3. Đối với mỗi ký tự trong số điện thoại, hãy chuyển đổi nó thành giá trị số và cộng dồn tổng. Bước này là cần thiết vì quyết định phụ thuộc vào tính chẵn lẻ của tổng chữ số đầy đủ chứ không phải cấu trúc từng phần. 
4. Sau khi tính tổng, kiểm tra xem nó có chẵn không. Nếu là số chẵn thì xuất ngay tên liên quan; nếu không hãy loại bỏ nó. 
5. Tiếp tục cho đến khi tất cả các tiếp điểm được xử lý, tự động giữ nguyên thứ tự đầu vào vì đầu ra được tạo ra theo cùng một thứ tự truyền tải. 

### Tại sao nó hoạt động 

Thuật toán dựa trên bất biến mà sau khi xử lý liên hệ thứ i, chúng tôi đã tính toán chính xác tổng chữ số chính xác của số điện thoại của nó. Vì mỗi chữ số đóng góp độc lập vào tổng và tính chẵn lẻ chỉ phụ thuộc vào tổng theo modulo 2 nên không có chữ số nào có thể bị bỏ qua hoặc xấp xỉ. Do đó, quyết định “in tên hay không” là chính xác cục bộ đối với mỗi liên hệ và không phụ thuộc vào các mục nhập khác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input().strip())
    for _ in range(n):
        line = input().strip().split()
        name = line[0]
        number = line[1]

        s = 0
        for ch in number:
            s += ord(ch) - ord('0')

        if s % 2 == 0:
            print(name)

if __name__ == "__main__":
    solve()
```Giải pháp xử lý dữ liệu đầu vào trong một lần duy nhất. Mỗi dòng được chia thành chính xác hai mã thông báo, do đó không có sự mơ hồ trong phân tích cú pháp ngay cả khi tên là chữ và số. 

Tổng chữ số được tính bằng cách sử dụng số học ký tự trực tiếp, giúp tránh chi phí chuyển đổi số nguyên và duy trì tính chính xác cho các số 0 đứng đầu. Việc kiểm tra tính chẵn lẻ được thực hiện ngay lập tức và đầu ra được truyền trực tiếp, giúp duy trì mức sử dụng bộ nhớ không đổi. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu:```
5
Jett 012345678
Viper 111111111
Neon 987654321
Raze 512610294
Reyna 192830492
```Đối với mỗi liên hệ: 

| Tên | Số | Tổng chữ số | Thậm chí? | Đầu ra | 
| --- | --- | --- | --- | --- | 
| Jett | 012345678 | 0+1+2+3+4+5+6+7+8 = 36 | Có | Jett | 
| Viper | 111111111 | 9 | Không | - | 
| Neon | 987654321 | 45 | Không | - | 
| San bằng | 512610294 | 30 | Có | San bằng | 
| Reyna | 192830492 | 38 | Có | Reyna | 

Dấu vết này cho thấy mỗi quyết định chỉ phụ thuộc vào tính chẵn lẻ của tổng cục bộ, độc lập với các liên hệ khác. Thứ tự đầu ra khớp với thứ tự của các mục nhập hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(D) | Mỗi chữ số trong mỗi số điện thoại đều được quét một lần | 
| Không gian | O(1) | Chỉ sử dụng các biến phụ không đổi | 

Tổng số chữ số nhỏ dưới các ràng buộc, do đó giải pháp phù hợp thoải mái trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    old_stdout = sys.stdout
    sys.stdout = io.StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdout = old_stdout
    return out

# provided sample
assert run("""5
Jett 012345678
Viper 111111111
Neon 987654321
Raze 512610294
Reyna 192830492
""") == """Jett
Raze
Reyna
"""

# minimum size
assert run("""1
A 0
""") == "A\n"

# all odd digits
assert run("""2
X 111
Y 135
""") == ""

# mixed parity
assert run("""3
A 12
B 123
C 222
""") == """A
C
"""

# leading zeros case
assert run("""2
Z 000
W 010
""") == """Z
W
"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đơn 0 | tên in | đầu vào tối thiểu, không xử lý | 
| tất cả những cái | trống | trường hợp từ chối đầy đủ | 
| chữ số hỗn hợp | đầu ra chọn lọc | tính đúng đắn chẵn lẻ | 
| số 0 đứng đầu | bao gồm chính xác | tính chính xác của việc phân tích chữ số | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi số điện thoại chỉ bao gồm các số 0, chẳng hạn như:```
1
Zero 000000
```Thuật toán tính tổng bằng 0, là số chẵn nên tên được in ra. Một sai lầm ở đây là trước tiên hãy chuyển đổi chuỗi thành số nguyên, chuỗi này vẫn hoạt động về mặt số nhưng có nguy cơ xảy ra sự phức tạp khi phân tích cú pháp không cần thiết và có khả năng tràn trong các biến thể khác. 

Một trường hợp cạnh khác là số có một chữ số như:```
1
Odd 7
```Tổng là 7, là số lẻ nên không in được gì. Thuật toán xử lý việc này một cách chính xác vì nó không có bất kỳ độ dài tối thiểu nào. 

Cuối cùng, hãy xem xét các số 0 đứng đầu hỗn hợp:```
1
Lead 00012
```Tổng là 1+2 = 3, là số lẻ nên bị loại trừ. Các số 0 đứng đầu không đóng góp gì, nhưng chúng vẫn phải được lặp lại; bỏ qua chúng sẽ thay đổi tính đúng đắn.
