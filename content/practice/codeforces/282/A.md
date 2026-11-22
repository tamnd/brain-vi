---
title: "CF 282A - Bit++"
description: "Bài toán trình bày một ngôn ngữ lập trình đồ chơi chỉ có một biến x, ban đầu được đặt bằng 0. Mỗi dòng của chương trình là một câu lệnh tăng hoặc giảm biến này."
date: "2026-06-05T09:29:28+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 282
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 173 (Div. 2)"
rating: 800
weight: 282
solve_time_s: 76
verified: true
draft: false
---

[CF 282A - Bit++](https://codeforces.com/problemset/problem/282/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 16s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Bài toán trình bày một ngôn ngữ lập trình đồ chơi chỉ có một biến _x_, ban đầu được đặt bằng 0. Mỗi dòng của chương trình là một câu lệnh tăng hoặc giảm biến này. Thao tác có thể xuất hiện trước hoặc sau biến, vì vậy câu lệnh có thể là`++X`,`X++`,`--X`, hoặc`X--`. Nhiệm vụ của chúng ta là xác định giá trị cuối cùng của _x_ sau khi thực hiện tất cả các câu lệnh theo thứ tự. 

Đầu vào bắt đầu bằng số lượng câu lệnh`n`, theo sau là`n`dòng, mỗi dòng là một câu lệnh hợp lệ. Đầu ra là một số nguyên biểu thị giá trị cuối cùng của _x_. 

Với những hạn chế,`n`nhiều nhất là 150, một con số cực kỳ nhỏ đối với các máy tính hiện đại. Điều này có nghĩa là ngay cả một cách tiếp cận ngây thơ kiểm tra từng câu lệnh riêng lẻ cũng sẽ được áp dụng ngay lập tức. Không có lo ngại về kích thước đầu vào lớn, tràn số nguyên hoặc tối ưu hóa hiệu suất. 

Trường hợp cạnh không rõ ràng phát sinh từ thứ tự các ký hiệu. Vì thao tác có thể xuất hiện trước hoặc sau`X`, một kiểm tra ngây thơ cho`statement == "++X"`hoặc`statement == "X++"`có thể bỏ lỡ một hoạt động giảm dần như`X--`. Chúng ta phải phát hiện sự tăng và giảm bằng cách kiểm tra sự hiện diện của`++`hoặc`--`bất cứ nơi nào trong tuyên bố, không chỉ theo vị trí. Ví dụ: đầu vào:```
3
++X
X--
--X
```sẽ dẫn đến giá trị cuối cùng là`0`, bởi vì số đầu tiên tăng, số thứ hai giảm và số thứ ba lại giảm. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Khởi tạo bộ đếm`x`về không. Đối với mỗi câu lệnh, hãy kiểm tra chuỗi. Nếu nó chứa`++`, tăng`x`; nếu nó chứa`--`, giảm`x`. Điều này hiệu quả vì mỗi câu lệnh chứa chính xác một thao tác và một biến. Vì mỗi câu lệnh được kiểm tra riêng lẻ và chúng tôi chỉ thực hiện tìm kiếm chuỗi thời gian không đổi cho mỗi câu lệnh, nên tổng số thao tác tỷ lệ thuận với`n`. Vì`n = 150`, điều này là tầm thường. 

Không có thuật toán "nhanh hơn" có ý nghĩa ở đây vì phương pháp brute-force đã chạy theo thời gian tuyến tính so với số lượng câu lệnh. Thông tin chi tiết quan trọng là chúng ta không cần phân tích cú pháp câu lệnh theo vị trí hoặc mã hóa nó; sự hiện diện của toán tử hai ký tự`++`hoặc`--`là đủ. Quan sát này đơn giản hóa việc thực hiện và loại bỏ các trường hợp đặc biệt liên quan đến thứ tự các ký tự. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một biến`x`về không. Điều này đại diện cho giá trị hiện tại của biến duy nhất trong chương trình. 
2. Đọc số câu lệnh,`n`. 
3. Với mỗi câu lệnh trong chương trình, hãy kiểm tra xem nó có chứa chuỗi con không`++`. Nếu có thì tăng`x`. 
4. Nếu tuyên bố có chứa`--`, giảm`x`. 
5. Sau khi xử lý tất cả các câu lệnh, hãy in giá trị của`x`. 

Tại sao nó hoạt động: Thuật toán duy trì tính bất biến`x`luôn phản ánh hiệu quả ròng của tất cả các báo cáo được xử lý cho đến nay. Mỗi câu lệnh sửa đổi`x`bởi chính xác một, lên hoặc xuống và mọi câu lệnh được đảm bảo chứa chính xác một thao tác. Vì vậy, bằng cách quét tuần tự và cập nhật`x`theo hoạt động được phát hiện, giá trị cuối cùng được đảm bảo là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
x = 0

for _ in range(n):
    statement = input().strip()
    if "++" in statement:
        x += 1
    elif "--" in statement:
        x -= 1

print(x)
```Giải pháp đầu tiên đọc số lượng câu lệnh và khởi tạo`x`. Đối với mỗi câu lệnh, nó sẽ loại bỏ mọi dòng mới hoặc dấu cách ở cuối và kiểm tra sự hiện diện của toán tử tăng hoặc giảm. Chúng tôi sử dụng`in`hơn là kiểm tra vị trí vì toán tử có thể xuất hiện trước hoặc sau`X`. Điều này đảm bảo tính đúng đắn cho tất cả các hoán vị như`++X`hoặc`X++`. Cuối cùng, kết quả được in. 

## Ví dụ đã hoạt động 

Theo dõi thông qua đầu vào mẫu:```
1
++X
```| Bước | Tuyên bố | x trước | x sau | 
| --- | --- | --- | --- | 
| 1 | ++X | 0 | 1 | 

Điều này xác nhận rằng một mức tăng đơn lẻ hoạt động chính xác. 

Một ví dụ khác:```
3
++X
X--
--X
```| Bước | Tuyên bố | x trước | x sau | 
| --- | --- | --- | --- | 
| 1 | ++X | 0 | 1 | 
| 2 | X-- | 1 | 0 | 
| 3 | --X | 0 | -1 | 

Điều này thể hiện việc xử lý kết hợp các mức tăng và giảm theo bất kỳ thứ tự nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi câu lệnh được kiểm tra chính xác một lần, với tìm kiếm chuỗi con có độ dài cố định. | 
| Không gian | O(1) | Chỉ có một số nguyên duy nhất`x`được lưu trữ; không cần cấu trúc dữ liệu bổ sung. | 

Được cho`n <= 150`, giải pháp chạy trong thời gian không đáng kể và sử dụng bộ nhớ tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    x = 0
    for _ in range(n):
        statement = input().strip()
        if "++" in statement:
            x += 1
        elif "--" in statement:
            x -= 1
    return str(x)

# provided samples
assert run("1\n++X\n") == "1", "sample 1"

# custom cases
assert run("3\n++X\nX--\n--X\n") == "-1", "mixed increments and decrements"
assert run("5\n++X\nX++\n++X\nX++\n++X\n") == "5", "all increments"
assert run("4\n--X\nX--\n--X\nX--\n") == "-4", "all decrements"
assert run("1\nX--\n") == "-1", "single decrement"
assert run("2\nX++\n--X\n") == "0", "increment followed by decrement"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 mức tăng và giảm hỗn hợp | -1 | Xử lý chính xác nhiều thao tác theo thứ tự khác nhau | 
| 5 tăng tất cả | 5 | Xử lý chính xác tất cả các mức tăng | 
| 4 giảm tất cả | -4 | Xử lý chính xác tất cả các mức giảm | 
| Giảm đơn | -1 | Xử lý đầu vào tối thiểu | 
| Tăng rồi giảm | 0 | Hoạt động hủy bỏ chính xác | 

## Vỏ cạnh 

Trường hợp cạnh chính là thứ tự của các ký hiệu trong câu lệnh. Ví dụ,`X++`nên được đối xử giống hệt như`++X`. Thuật toán của chúng tôi xử lý việc này vì nó kiểm tra chuỗi con`++`hoặc`--`bất cứ nơi nào trong tuyên bố. 

đầu vào:```
2
X++
--X
```Dấu vết thực hiện: 

| Bước | Tuyên bố | x trước | x sau | 
| --- | --- | --- | --- | 
| 1 | X++ | 0 | 1 | 
| 2 | --X | 1 | 0 | 

Đầu ra là`0`, xác nhận thuật toán diễn giải chính xác vị trí của toán tử. Điều này đảm bảo không có lỗi xảy ra do vị trí của toán tử so với biến.
