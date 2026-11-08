---
title: "CF 1415A - Vượt ngục"
description: "Chúng ta có một nhà tù hình chữ nhật với $n$ hàng và $m$ cột. Mỗi ô chứa chính xác một tù nhân và có một đường hầm thoát duy nhất trong một ô nằm ở hàng $r$ và cột $c$."
date: "2026-06-11T07:11:13+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "math"]
categories: ["algorithms"]
codeforces_contest: 1415
codeforces_index: "A"
codeforces_contest_name: "Technocup 2021 - Elimination Round 2"
rating: 800
weight: 1415
solve_time_s: 83
verified: true
draft: false
---

[CF 1415A - Vượt ngục](https://codeforces.com/problemset/problem/1415/A) 

**Đánh giá:** 800 
**Tags:** sức mạnh vũ phu, toán học 
**Thời gian giải:** 1 phút 23s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một nhà tù hình chữ nhật với$n$hàng và$m$cột. Mỗi ô chứa đúng một tù nhân và có một đường hầm thoát duy nhất trong ô nằm ở hàng$r$và cột$c$. Mỗi tù nhân có thể di chuyển đến bất kỳ phòng giam liền kề nào (lên, xuống, trái, phải) trong một giây hoặc giữ nguyên vị trí. Mục đích là xác định xem tù nhân cuối cùng sẽ mất bao lâu để đến được đường hầm vượt ngục nếu tất cả tù nhân đều di chuyển tối ưu. Kết quả là số giây tối thiểu mà sau đó tất cả tù nhân có thể tập trung ở lối ra. 

Đầu vào cung cấp nhiều trường hợp thử nghiệm, mỗi trường hợp đưa ra kích thước nhà tù và vị trí của đường hầm. Đầu ra của mỗi trường hợp thử nghiệm là một số nguyên biểu thị thời gian tối thiểu cần thiết để tất cả tù nhân đến được đường hầm. 

Những hạn chế là rất lớn:$n$Và$m$có thể lên đến$10^9$, và số lượng ca kiểm thử$t$có thể đạt được$10^4$. Điều này ngay lập tức loại trừ mọi mô phỏng hoặc lặp lại trên tất cả các ô. Chúng tôi không thể tính toán khoảng cách cho từng tù nhân riêng lẻ và cũng không thể lưu trữ đầy đủ$n \times m$ma trận trong bộ nhớ. Lời giải phải là một công thức tính trực tiếp khoảng cách tối đa. 

Các trường hợp cạnh không rõ ràng liên quan đến các đường hầm nằm ở các góc hoặc cạnh. Ví dụ: nếu đường hầm nằm ở góc trên bên trái của$1 \times 1$nhà tù, câu trả lời là 0. Nếu đường hầm ở$(1, 1)$trong một$10 \times 10$tù, tù nhân xa nhất ở góc đối diện$(10, 10)$, và thời gian là$18$. Một cách tiếp cận bất cẩn cho rằng đường hầm ở gần trung tâm hoặc khoảng cách trung bình sẽ tạo ra câu trả lời sai ở đây. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là tưởng tượng mỗi tù nhân là một điểm trên lưới và tính toán khoảng cách từ Manhattan đến đường hầm. Đối với mỗi tù nhân tại$(i, j)$, khoảng cách là$|i - r| + |j - c|$. Số giây tối thiểu để tất cả tù nhân đến được đường hầm khi đó sẽ là khoảng cách tối đa trên tất cả các phòng giam. Điều này hoạt động về mặt khái niệm, nhưng lặp lại trên mọi ô trong một$n \times m$lưới là không thể cho lớn$n$Và$m$. Đối với trường hợp xấu nhất, với$n = m = 10^9$, điều này sẽ yêu cầu$10^{18}$hoạt động hoàn toàn không thể thực hiện được. 

Cái nhìn sâu sắc quan trọng là chúng ta chỉ cần tù nhân ở xa đường hầm nhất. Trong lưới hình chữ nhật, ô xa nhất so với ô đã cho luôn là một trong bốn góc. Do đó, chúng ta có thể tính khoảng cách từ đường hầm đến mỗi góc và lấy giá trị lớn nhất. Điều này làm giảm vấn đề từ việc lặp lại hàng tỷ ô đến việc chỉ đánh giá bốn biểu thức. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n*m) | O(1) | Quá chậm | 
| Tối ưu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case,$t$. 
2. Đối với mỗi trường hợp thử nghiệm, hãy đọc kích thước$n$Và$m$và vị trí đường hầm$(r, c)$. 
3. Xác định 4 góc của lưới:$(1, 1)$,$(1, m)$,$(n, 1)$, Và$(n, m)$. 
4. Tính khoảng cách Manhattan từ đường hầm đến từng góc:$|r - 1| + |c - 1|$,$|r - 1| + |c - m|$,$|r - n| + |c - 1|$, Và$|r - n| + |c - m|$. 
5. Tận dụng tối đa bốn khoảng cách này. Đây là số giây tối thiểu cần thiết để tù nhân xa nhất có thể đến được đường hầm. 
6. In khoảng cách tối đa cho mỗi test. 

Tại sao nó hoạt động: Khoảng cách Manhattan thể hiện chính xác số lần di chuyển tối thiểu cần thiết trong lưới khi chuyển động bị hạn chế theo các hướng chính. Tù nhân xa đường hầm nhất phải ở một trong các góc, vì bất kỳ điểm nào bên trong lưới đều gần đường hầm hơn ít nhất một góc. Việc lấy khoảng cách tối đa đảm bảo rằng tất cả tù nhân, kể cả những người ở xa nhất, đều có thể đến được đường hầm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, m, r, c = map(int, input().split())
    dist1 = abs(r - 1) + abs(c - 1)
    dist2 = abs(r - 1) + abs(c - m)
    dist3 = abs(r - n) + abs(c - 1)
    dist4 = abs(r - n) + abs(c - m)
    print(max(dist1, dist2, dist3, dist4))
```Giải pháp đầu tiên là đọc số lượng trường hợp thử nghiệm. Đối với mỗi trường hợp thử nghiệm, nó đọc bốn số nguyên biểu thị kích thước nhà tù và tọa độ đường hầm. Khoảng cách đến bốn góc được tính toán bằng cách sử dụng sai số tuyệt đối, trực tiếp thực hiện khoảng cách Manhattan. Giá trị tối đa trong bốn giá trị này sẽ được in. sử dụng`abs`đảm bảo rằng những khác biệt âm không gây ra sai sót và việc tính toán từng khoảng cách một cách độc lập sẽ tránh được những sai sót riêng lẻ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:`10 10 1 1`| Góc | Khoảng cách | 
| --- | --- | 
| (1,1) | 0 | 
| (1,10) | 9 | 
| (10,1) | 9 | 
| (10,10) | 18 | 

Khoảng cách tối đa: 18. Điều này cho thấy tù nhân xa nhất ở góc dưới bên phải. 

### Mẫu 2 

đầu vào:`3 5 2 4`| Góc | Khoảng cách | 
| --- | --- | 
| (1,1) | 4 | 
| (1,5) | 3 | 
| (3,1) | 4 | 
| (3,5) | 2 | 

Khoảng cách tối đa: 4. Tù nhân xa nhất nằm ở góc trên bên trái hoặc dưới cùng bên trái. 

Những ví dụ này chứng minh rằng khoảng cách tối đa luôn đến từ một trong các góc và thuật toán xác định chính xác khoảng cách đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) cho mỗi trường hợp thử nghiệm | Chỉ có bốn phép tính khoảng cách được thực hiện | 
| Không gian | O(1) | Không có mảng hoặc lưới nào được lưu trữ | 

Được cho$t \le 10^4$, tổng số hoạt động là$4 \cdot t \le 4 \cdot 10^4$, dễ dàng phù hợp với giới hạn 1 giây. Việc sử dụng bộ nhớ là tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    t = int(input())
    for _ in range(t):
        n, m, r, c = map(int, input().split())
        dist1 = abs(r - 1) + abs(c - 1)
        dist2 = abs(r - 1) + abs(c - m)
        dist3 = abs(r - n) + abs(c - 1)
        dist4 = abs(r - n) + abs(c - m)
        print(max(dist1, dist2, dist3, dist4))
    return output.getvalue().strip()

assert run("3\n10 10 1 1\n3 5 2 4\n10 2 5 1\n") == "18\n4\n6", "Sample 1"
assert run("1\n1 1 1 1\n") == "0", "Single cell"
assert run("1\n5 5 3 3\n") == "4", "Tunnel in center"
assert run("1\n1000000000 1000000000 1 1\n") == "1999999998", "Maximum size corner"
assert run("1\n1000000000 1000000000 500000000 500000000\n") == "1000000000", "Maximum size center"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1 1 1 1\n`|`0`| Nhà tù đơn bào | 
|`1\n5 5 3 3\n`|`4`| Đường hầm ở trung tâm | 
|`1\n1000000000 1000000000 1 1\n`|`1999999998`| Lưới kích thước tối đa, đường hầm góc | 
|`1\n1000000000 1000000000 500000000 500000000\n`|`1000000000`| Lưới kích thước tối đa, đường hầm trung tâm | 

## Vỏ cạnh 

Đối với một$1 \times 1$nhà tù, đường hầm là phòng giam duy nhất. Thuật toán tính khoảng cách đến một góc$(1,1)$, bằng 0. Kết quả đầu ra đúng là 0. 

Đối với lưới có kích thước tối đa với đường hầm ở một góc,$(1,1)$trong một$10^9 \times 10^9$nhà tù, tù nhân xa nhất là ở$(10^9,10^9)$. Thuật toán tính toán`abs(1-10^9) + abs(1-10^9) = 1999999998`, phù hợp với mong đợi. 

Khi đường hầm ở giữa, khoảng cách đến các góc sẽ khác nhau. Thuật toán chọn chính xác mức tối đa, đảm bảo tính đến tù nhân xa nhất mà không cần lặp lại trên tất cả các ô
