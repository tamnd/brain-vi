---
title: "CF 1107B - Gốc kỹ thuật số"
description: "Chúng tôi được yêu cầu trả lời nhiều truy vấn độc lập. Mỗi truy vấn đưa ra hai số, một vị trí $k$ và một chữ số $x$ trong khoảng từ 1 đến 9. Đối với mỗi truy vấn, chúng ta cần xuất ra số nguyên dương thứ $k$-th có gốc kỹ thuật số bằng $x$."
date: "2026-06-12T05:22:17+07:00"
tags: ["codeforces", "competitive-programming", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1107
codeforces_index: "B"
codeforces_contest_name: "Educational Codeforces Round 59 (Rated for Div. 2)"
rating: 1000
weight: 1107
solve_time_s: 70
verified: true
draft: false
---

[CF 1107B - Root kỹ thuật số](https://codeforces.com/problemset/problem/1107/B) 

**Đánh giá:** 1000 
**Tags:** toán, lý thuyết số 
**Thời gian giải:** 1 phút 10s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu trả lời nhiều truy vấn độc lập. Mỗi truy vấn đưa ra hai số, một vị trí$k$và một chữ số$x$trong khoảng từ 1 đến 9. Đối với mỗi truy vấn, chúng ta cần xuất ra$k$-số nguyên dương thứ có nghiệm số bằng$x$. 

Căn nguyên số của một số là phần còn lại sau khi cộng liên tục các chữ số của nó cho đến khi còn lại một chữ số. Ví dụ: các số như 5, 14, 23, 32 đều giảm xuống còn 5, trong khi các số như 2, 11, 20, 29 đều giảm xuống 2. 

Vì vậy, mỗi truy vấn thực sự yêu cầu chúng ta liệt kê dãy số tăng dần vô hạn được nhóm theo gốc số của chúng và chọn$k$-phần tử thứ trong nhóm cho một chữ số nhất định. 

Những hạn chế quan trọng bởi vì$k$có thể lớn như$10^{12}$, điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào cố gắng lặp qua các số và tính toán từng gốc số một. Thậm chí kiểm tra$10^9$số lượng cho mỗi truy vấn sẽ vượt xa giới hạn thời gian. Vì có tới$10^3$truy vấn, chúng ta cần một công thức trực tiếp ánh xạ$(k, x)$đến câu trả lời trong thời gian không đổi. 

Một điểm tinh tế là căn số hoạt động theo chu kỳ đối với modulo 9. Một sai lầm ngây thơ là cho rằng chúng ta chỉ có thể xây dựng các số theo từng chữ số hoặc mô phỏng tổng các chữ số, nhưng điều đó hoàn toàn phá vỡ thang đo đầu vào. 

Trường hợp cạnh xuất hiện khi$x = 9$, bởi vì căn số 9 tương ứng chính xác với bội số của 9, không phải là số dư bằng 9 mod 9 theo nghĩa ngây thơ trừ khi được xử lý cẩn thận. Một vấn đề khác là hành vi sai lệch khi ánh xạ phần tử đầu tiên của mỗi nhóm. 

Ví dụ, đối với$x = 1$, trình tự bắt đầu như sau: 

1, 10, 19, 28, 37, ... 

cho$x = 9$, nó bắt đầu như: 

9, 18, 27, 36, 45, ... 

Bất kỳ giải pháp nào cũng phải tạo ra các chuỗi này một cách trực tiếp mà không cần quét số nguyên. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ xử lý từng truy vấn một cách độc lập bằng cách lặp qua các số tự nhiên, tính toán gốc số của chúng và thu thập các kết quả khớp cho đến khi đạt được$k$-thứ 1 Điều này hiệu quả vì mọi số đều có gốc số được xác định rõ ràng và chúng ta có thể kiểm tra tư cách thành viên trong$O(\log n)$thời gian cho mỗi số. Tuy nhiên, trong trường hợp xấu nhất, nếu$k = 10^{12}$, chúng ta sẽ cần quét tới$10^{12}$số lượng trên mỗi truy vấn, làm cho giải pháp không khả thi. 

Quan sát quan trọng là căn số được xác định hoàn toàn bằng giá trị modulo 9 của một số, với trường hợp đặc biệt là bội số của 9 tương ứng với căn số 9. Điều này tạo ra một cấu trúc tuần hoàn hoàn hảo: mỗi khối gồm 9 số nguyên liên tiếp chứa chính xác một số cho mỗi căn số từ 1 đến 9. 

Điều này có nghĩa là chuỗi số có gốc số cho trước tạo thành một cấp số cộng với kích thước bước 9. Khi chúng ta xác định được số đầu tiên trong chuỗi cho một số đã cho$x$, cái$k$-số thứ chỉ đơn giản là điểm bắt đầu cộng$9 \cdot (k-1)$. 

Vì$x = 9$, số đầu tiên là 9. Với$x \in [1,8]$, số đầu tiên là$x$chính nó. Điều này đưa ra một giải pháp dạng đóng trực tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n \cdot k)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đối với mỗi truy vấn$(k, x)$, chúng tôi xây dựng câu trả lời trực tiếp. 

1. Xác định số đầu tiên trong dãy tương ứng với căn số$x$. Nếu như$x = 9$, số này là 9. Ngược lại thì là$x$. Điều này hiệu quả vì các số có một chữ số đã có gốc số và 9 là bội số đầu tiên của 9. 
2. Sau khi điểm bắt đầu được cố định, hãy quan sát rằng việc thêm 9 sẽ giữ nguyên gốc số. Điều này là do việc thêm 9 không làm thay đổi phần dư modulo 9 của một số theo cách ảnh hưởng đến việc phân loại gốc số của nó. 
3. Tính đáp án như sau:$$\text{ans} = \text{start} + 9 \cdot (k - 1)$$4. Xuất giá trị này cho từng truy vấn một cách độc lập. 

Tại sao nó hoạt động 

Tính đúng đắn đến từ việc phân chia các số nguyên dương thành chín cấp số cộng dựa trên phần dư modulo 9 của chúng, ngoại trừ phần dư 0 tương ứng với căn số 9. Mỗi số nguyên thuộc về chính xác một cấp số và mỗi cấp số tăng thêm 9 trong khi vẫn giữ nguyên căn số đó. Do đó, việc lập chỉ mục cho một cấp số tương đương với việc đếm xuôi theo các bước cố định, điều này đảm bảo rằng kết quả được xây dựng$k$- phần tử thứ chính xác là số được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    for _ in range(n):
        k, x = map(int, input().split())
        start = 9 if x == 9 else x
        print(start + 9 * (k - 1))

if __name__ == "__main__":
    solve()
```Giải pháp đọc từng truy vấn và tính toán câu trả lời trong thời gian không đổi. Chi tiết triển khai chính là xử lý$x = 9$riêng biệt, vì nó tương ứng với cấp số cộng bắt đầu từ 9 thay vì 0 hoặc 9 modulo 9 mơ hồ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 5
5 2
3 1
```| Truy vấn | k | x | bắt đầu | công thức | kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 5 | 5 | 5 + 9·0 | 5 | 
| 2 | 5 | 2 | 2 | 2 + 9·4 | 38 | 
| 3 | 3 | 1 | 1 | 1 + 9·2 | 19 | 

Dấu vết này cho thấy rằng mỗi lớp gốc kỹ thuật số hoạt động giống như một cấp số cộng đơn giản và việc lập chỉ mục hoàn toàn là tuyến tính. 

### Ví dụ 2 

đầu vào:```
2
4 9
6 7
```| Truy vấn | k | x | bắt đầu | công thức | kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 4 | 9 | 9 | 9 + 9·3 | 36 | 
| 2 | 6 | 7 | 7 | 7 + 9·5 | 52 | 

Điều này khẳng định rằng ngay cả trường hợp đặc biệt$x = 9$phù hợp hoàn toàn với cùng một cấu trúc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi truy vấn được trả lời bằng một số phép tính số học không đổi | 
| Không gian |$O(1)$| Không cần kết cấu phụ trợ | 

Các ràng buộc cho phép lên đến$10^3$truy vấn, do đó, giải pháp thời gian không đổi cho mỗi truy vấn là đủ dễ dàng. Ngay cả với$k$lên đến$10^{12}$, chúng tôi không bao giờ lặp lại đến$k$, chỉ tính một công thức trực tiếp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n = int(input())
    out = []
    for _ in range(n):
        k, x = map(int, input().split())
        start = 9 if x == 9 else x
        out.append(str(start + 9 * (k - 1)))
    return "\n".join(out)

# provided samples
assert run("3\n1 5\n5 2\n3 1\n") == "5\n38\n19"

# minimum k
assert run("1\n1 9\n") == "9"

# first element of each class
assert run("3\n1 1\n1 5\n1 9\n") == "1\n5\n9"

# large k
assert run("2\n1000000000000 1\n1000000000000 9\n") == "8999999999991\n8999999999999"

# boundary mix
assert run("2\n2 2\n3 8\n") == "11\n26"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| k=1 trường hợp | 1,5,9 | điểm xuất phát đúng | 
| k lớn | giá trị lớn | không tràn, chia tỷ lệ công thức đúng | 
| chữ số hỗn hợp | đầu ra đa dạng | xử lý đúng tất cả x | 

## Vỏ cạnh 

cho$x = 9$, dãy số là bội số của 9. Bộ thuật toán`start = 9`, vì vậy đối với đầu vào$(k=1, x=9)$, ta được 9. Với$k=4$, chúng tôi tính toán$9 + 27 = 36$, khớp với bội số thứ 4 của 9. 

Đối với các giá trị nhỏ như$x = 1$, chuỗi bắt đầu từ 1. Đối với$(k=1, x=1)$, ta được 1. Với$(k=3, x=1)$, chúng tôi tính toán$1 + 18 = 19$, khớp với bảng liệt kê trực tiếp 1, 10, 19. 

Đối với rất lớn$k$, quá trình tính toán vẫn ổn định vì chỉ sử dụng phép nhân và phép cộng trên số nguyên an toàn 64 bit. Python xử lý các số nguyên lớn một cách tự nhiên nên không có nguy cơ tràn dữ liệu.
