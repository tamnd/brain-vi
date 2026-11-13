---
title: "CF 1702A - Làm tròn giá xuống"
description: "Mỗi trường hợp thử nghiệm đưa ra giá hiện tại của một mặt hàng. Giá \"làm tròn\" là lũy thừa của mười: 1, 10, 100, 1000, v.v. Chúng tôi muốn giảm giá của mặt hàng đó cho đến khi nó trở thành số tròn lớn nhất không vượt quá giá gốc."
date: "2026-06-09T21:41:10+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms"]
categories: ["algorithms"]
codeforces_contest: 1702
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 805 (Div. 3)"
rating: 800
weight: 1702
solve_time_s: 114
verified: true
draft: false
---

[CF 1702A - Làm tròn giá xuống](https://codeforces.com/problemset/problem/1702/A) 

**Đánh giá:** 800 
**Tas:** thuật toán xây dựng 
**Thời gian giải:** 1 phút 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi trường hợp thử nghiệm đưa ra giá hiện tại của một mặt hàng. Giá "tròn" là lũy thừa của 10: 1, 10, 100, 1000, v.v. 

Chúng tôi muốn giảm giá của mặt hàng đó cho đến khi nó trở thành số tròn lớn nhất không vượt quá giá gốc. Câu trả lời là số tiền được loại bỏ khỏi giá. 

Ví dụ: nếu giá là 178 thì lũy thừa lớn nhất của 10 không lớn hơn 178 là 100. Mức giảm là 178 - 100 = 78. 

Giá nhiều nhất là$10^9$, và có thể có tới$10^4$trường hợp thử nghiệm. Những giới hạn này rất nhỏ đối với các phép tính số học. Thậm chí một$O(\log m)$giải pháp cho mỗi trường hợp thử nghiệm là đủ nhanh vì$m$có nhiều nhất 10 chữ số. Không cần cấu trúc dữ liệu nặng hoặc tiền xử lý. 

Phần khó khăn là xác định chính xác lũy thừa lớn nhất của mười không vượt quá giá trị đã cho. 

Trường hợp một cạnh xảy ra khi giá đã là lũy thừa của mười. 

đầu vào:```
1
1000
```Đầu ra:```
0
```Số tròn hợp lệ lớn nhất chính là 1000, vì vậy không cần phải xóa gì. Một giải pháp bất cẩn luôn chọn lũy thừa nhỏ hơn 10 sẽ cho kết quả sai là 900. 

Một trường hợp cạnh khác là giá trị nhỏ nhất có thể. 

đầu vào:```
1
1
```Đầu ra:```
0
```Vì 1 là$10^0$, câu trả lời lại là số không. 

Ví dụ hữu ích thứ ba là một số ngay dưới lũy thừa tiếp theo của mười. 

đầu vào:```
1
999999999
```Đầu ra:```
899999999
```lũy thừa lớn nhất của mười không vượt quá giá là$10^8 = 100000000$, không$10^9$, bởi vì$10^9$lớn hơn giá trị đã cho. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là tạo ra lũy thừa mười và tìm kiếm lũy thừa lớn nhất không vượt quá mức giá nhất định. Vì sức mạnh của số mười tăng lên nhanh chóng nên chỉ có một số ít ứng cử viên đạt được$10^9$: 1, 10, 100, ...,$10^9$. Kiểm tra tất cả chúng cho mọi trường hợp thử nghiệm đã đủ nhanh rồi. 

Quan sát thực tế là lũy thừa lớn nhất của mười không vượt quá một số được xác định hoàn toàn bằng số chữ số. Đối với số nguyên dương với$d$chữ số, lũy thừa lớn nhất của mười không vượt quá nó là$10^{d-1}$. 

Ví dụ: 178 có ba chữ số nên số làm tròn mong muốn là$10^2 = 100$. Tương tự, 987654321 có chín chữ số nên số làm tròn mong muốn là$10^8 = 100000000$. 

Một khi chúng ta biết sức mạnh của số mười này, câu trả lời chỉ đơn giản là:$$m - 10^{d-1}$$Ở đây, các cách tiếp cận bạo lực và tối ưu rất giống nhau vì các ràng buộc rất nhỏ. Việc quan sát số lượng chữ số mang lại cách thực hiện rõ ràng nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(log m) | O(1) | Đã chấp nhận | 
| Tối ưu | O(log m) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc giá$m$. 
2. Chuyển đổi$m$vào một chuỗi và tính độ dài của nó$d$. 

Độ dài trực tiếp cho chúng ta biết số đó có bao nhiêu chữ số. 
3. Tính lũy thừa lớn nhất của mười không vượt quá$m$BẰNG$10^{d-1}$. 

Bất kỳ số nguyên dương nào với$d$ít nhất là chữ số$10^{d-1}$và ít hơn$10^d$. 
4. Tính mức giảm cần thiết:$$\text{answer} = m - 10^{d-1}$$5. Xuất ra câu trả lời. 

### Tại sao nó hoạt động 

Với mọi số nguyên dương$m$với$d$chữ số,$$10^{d-1} \le m < 10^d$$Điều này có nghĩa$10^{d-1}$là số làm tròn hợp lệ không vượt quá$m$. Bất kỳ lũy thừa nào lớn hơn của mười sẽ là$10^d$, lớn hơn rất nhiều so với$m$. Như vậy$10^{d-1}$chính xác là số tròn lớn nhất mà bài toán cho phép. Trừ nó từ$m$mang lại mức giảm cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    m = int(input())
    d = len(str(m))
    round_price = 10 ** (d - 1)
    print(m - round_price)
```Chương trình xử lý từng trường hợp thử nghiệm một cách độc lập. 

Số chữ số thu được bằng cách sử dụng`len(str(m))`. Nếu một số có`d`chữ số, lũy thừa lớn nhất của mười không vượt quá nó là`10 ** (d - 1)`. 

Phép trừ`m - round_price`trực tiếp tạo ra mức giảm cần thiết. 

Không có lo ngại về tràn vì giá trị lớn nhất liên quan là$10^9$, điều này không quan trọng đối với số nguyên Python. 

Một sai lầm phổ biến là sử dụng lũy ​​thừa gần nhất của mười thay vì lũy thừa lớn nhất của mười để không vượt quá giá. Ví dụ: 9000 nên sử dụng 1000 chứ không phải 10000, vì 10000 lớn hơn giá gốc. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Giá đầu vào:```
178
```| m | Chữ số d | Sức mạnh hợp lệ lớn nhất của mười | Trả lời | 
| --- | --- | --- | --- | 
| 178 | 3 | 100 | 78 | 

Số này có ba chữ số nên giá làm tròn mục tiêu là$10^2 = 100$. Mức giảm là$178 - 100 = 78$. 

### Ví dụ 2 

Giá đầu vào:```
987654321
```| m | Chữ số d | Sức mạnh hợp lệ lớn nhất của mười | Trả lời | 
| --- | --- | --- | --- | 
| 987654321 | 9 | 100000000 | 887654321 | 

Số có chín chữ số. lũy thừa lớn nhất của mười không vượt quá nó là$10^8$. Trừ đi cho mức giảm cần thiết. 

Những ví dụ này minh họa tính bất biến của khóa: khi đã biết số chữ số, số làm tròn mục tiêu sẽ cố định. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log m) | Việc chuyển đổi số thành chuỗi cần có thời gian tỷ lệ thuận với số chữ số của nó | 
| Không gian | O(1) | Chỉ có một vài biến được lưu trữ | 

Từ$m \le 10^9$, mỗi số có nhiều nhất 10 chữ số. Ngay cả với$10^4$trường hợp thử nghiệm, tổng công việc rất nhỏ và dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    t = int(input())
    ans = []

    for _ in range(t):
        m = int(input())
        ans.append(str(m - 10 ** (len(str(m)) - 1)))

    return "\n".join(ans)

# provided sample
assert run(
"""7
1
2
178
20
999999999
9000
987654321
"""
) == "\n".join([
"0",
"1",
"78",
"10",
"899999999",
"8000",
"887654321"
]), "sample"

# minimum value
assert run(
"""1
1
"""
) == "0", "minimum input"

# already a power of ten
assert run(
"""1
1000
"""
) == "0", "already round"

# just below next power of ten
assert run(
"""1
999
"""
) == "899", "boundary below power"

# maximum constraint
assert run(
"""1
1000000000
"""
) == "0", "maximum value"

# mixed values
assert run(
"""4
10
11
99
100
"""
) == "\n".join([
"0",
"1",
"89",
"0"
]), "mixed boundary cases"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 | 0 | Giá trị nhỏ nhất có thể | 
| 1000 | 0 | Đã là sức mạnh của mười | 
| 999 | 899 | Số ngay dưới lũy thừa mười | 
| 1000000000 | 0 | Giá trị tối đa được phép | 
| 10, 11, 99, 100 | 0, 1, 89, 0 | Chuyển tiếp ranh giới giữa các độ dài chữ số | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
1
1
```Số có một chữ số nên thuật toán tính$10^{1-1} = 1$. Câu trả lời là$1 - 1 = 0$. Điều này xử lý chính xác giá trị nhỏ nhất có thể. 

Hãy xem xét đầu vào:```
1
1000
```Số có bốn chữ số. Thuật toán tính toán$10^3 = 1000$. Câu trả lời là$1000 - 1000 = 0$. Vì giá đã là số tròn nên không cần giảm. 

Hãy xem xét đầu vào:```
1
999999999
```Số có chín chữ số. Thuật toán tính toán$10^8 = 100000000$. Câu trả lời trở thành:$$999999999 - 100000000 = 899999999$$Sức mạnh tiếp theo của mười,$10^9$, quá lớn và không thể sử dụng được. Quy tắc đếm chữ số sẽ tự động chọn số làm tròn đúng. 

Hãy xem xét đầu vào:```
1
20
```Số có hai chữ số. Thuật toán chọn$10^1 = 10$. Câu trả lời là$20 - 10 = 10$. Điều này xác nhận rằng mục tiêu là lũy thừa lớn nhất của mười không vượt quá giá trị, không phải lũy thừa gần nhất của mười.
