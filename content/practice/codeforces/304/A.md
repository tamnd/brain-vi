---
title: "CF 304A - Định lý Pytago II"
description: "Chúng ta cần đếm xem có bao nhiêu tam giác vuông có cạnh nguyên sao cho cả ba cạnh nhiều nhất là n. Một tam giác vuông có các cạnh (a, b, c) thỏa mãn phương trình Pythagore: $a^2+b^2=c^2$$a$$b$$c = sqrt{a^2 + b^2} xấp xỉ 21,21$$a^2 + b^2 = c^2 xấp xỉ 225,00 + 225,00 = 450."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "math"]
categories: ["algorithms"]
codeforces_contest: 304
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 183 (Div. 2)"
rating: 1200
weight: 304
solve_time_s: 120
verified: true
draft: false
---

[CF 304A - Định lý Pythagore II](https://codeforces.com/problemset/problem/304/A) 

**Đánh giá:** 1200 
**Tags:** sức mạnh vũ phu, toán học 
**Thời gian giải:** 2 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Ta cần đếm xem có bao nhiêu tam giác vuông có cạnh nguyên sao cho cả ba cạnh bằng nhau nhiều nhất`n`. 

Một tam giác vuông có các cạnh`(a, b, c)`thỏa mãn phương trình Pythagore:$a^2+b^2=c^2⟦PROTECT_24⟧b⟦PROTECT_25⟧a^2 + b^2 = c^2 \approx 225.00 + 225.00 = 450.00$Điều chỉnh a và b cập nhật tam giác và giá trị dẫn xuất của c.abc 

Bài toán hạn chế thứ tự các bên:```
1 ≤ a ≤ b ≤ c ≤ n
```Thứ tự quan trọng vì hình tam giác như`(3,4,5)`Và`(4,3,5)`nên được coi là cùng một hình tam giác. điều kiện`a ≤ b`tự động loại bỏ các bản sao. 

Đầu vào chứa một số nguyên duy nhất`n`. Đầu ra là số bộ ba hợp lệ`(a, b, c)`thỏa mãn phương trình và ràng buộc về thứ tự. 

Giới hạn trên chỉ là`10^4`. Điều đó ngay lập tức thay đổi bản chất của vấn đề. Thậm chí một`O(n^2)`thuật toán thực hiện xung quanh`10^8`các thao tác đơn giản trong trường hợp xấu nhất nếu được triển khai cẩn thận trong Python, vốn vẫn ở mức giới hạn nhưng thường được chấp nhận với số học nhẹ. MỘT`O(n^3)`giải pháp là hoàn toàn không thể bởi vì nó đòi hỏi khoảng`10^12`lần lặp lại. 

Thách thức chính là tránh việc liệt kê các bộ ba không cần thiết. 

Trường hợp cạnh tinh tế xuất hiện khi các hình tam giác trùng lặp được tính vô tình. 

Ví dụ: 

đầu vào:```
5
```Tam giác hợp lệ duy nhất là`(3,4,5)`. 

Câu trả lời đúng là:```
1
```Một lực lượng vũ phu bất cẩn lặp lại trên tất cả các cặp có thứ tự`(a,b)`mà không thực thi`a ≤ b`sẽ tính cả hai`(3,4,5)`Và`(4,3,5)`và in sai`2`. 

Một sai lầm dễ dàng khác là quên điều đó`c`nhiều nhất cũng phải thế`n`. 

Ví dụ: 

đầu vào:```
10
```Cặp đôi`(6,8)`sản xuất`c = 10`, hợp lệ. 

Nhưng`(8,15)`sản xuất`c = 17`, không được tính mặc dù nó thỏa mãn phương trình. 

Cạm bẫy thứ ba đến từ độ chính xác của dấu phẩy động. Một số triển khai tính toán:```
c = int(math.sqrt(a*a + b*b))
```và sau đó kiểm tra xem phương trình có đúng không. Làm tròn dấu phẩy động đôi khi có thể tạo ra giá trị không chính xác cho số lớn hơn. Cách tiếp cận an toàn là xác minh rõ ràng rằng:```
c * c == a * a + b * b
```trước khi đếm hình tam giác. 

## Phương pháp tiếp cận 

Giải pháp bạo lực trực tiếp nhất sẽ thử mọi cách có thể`(a,b,c)`. 

Chúng tôi lặp qua tất cả các giá trị:```
1 ≤ a ≤ b ≤ c ≤ n
```và kiểm tra xem:$a^2+b^2=c^2⟦PROTECT_26⟧b⟦PROTECT_27⟧a^2 + b^2 = c^2 \approx 225.00 + 225.00 = 450.00$Điều chỉnh a và b cập nhật tam giác và giá trị dẫn xuất của c.abc 

Điều này có tác dụng vì định nghĩa của một tam giác hợp lệ được kiểm tra chính xác. Không có gì bị bỏ sót và không có gì không hợp lệ được tính. 

Vấn đề là số lần lặp lại. Có khoảng:$n^3$có thể gấp ba lần. Với`n = 10^4`, điều đó trở thành khoảng`10^12`kiểm tra, vượt xa thời hạn. 

Quan sát quan trọng là một lần`a`Và`b`cố định, giá trị của`c`được xác định hoàn toàn. 

Từ phương trình:$c=\sqrt{a^2+b^2}⟦PROTECT_28⟧b⟦PROTECT_29⟧a^2 + b^2 = c^2 \approx 225.00 + 225.00 = 450.00$Điều chỉnh a và b cập nhật tam giác và giá trị dẫn xuất của c.abc 

Vì vậy, thay vì lặp qua ba biến, chúng ta chỉ lặp qua`(a,b)`và kiểm tra xem căn bậc hai thu được có phải là số nguyên hay không. 

Điều này làm giảm không gian tìm kiếm từ bậc ba sang bậc hai. 

Đối với mỗi cặp`(a,b)`với`a ≤ b`, chúng tôi tính toán:```
s = a*a + b*b
c = int(sqrt(s))
```Nếu như`c*c == s`Và`c ≤ n`, thì chúng ta đã tìm được một tam giác hợp lệ. 

Phương pháp bậc hai thành công vì ràng buộc`n ≤ 10^4`cho phép về`5 × 10^7`lặp lại nhẹ trong Python được tối ưu hóa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n³) | O(1) | Quá chậm | 
| Tối ưu | O(n²) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`n`. 
2. Khởi tạo`answer = 0`. 
3. Lặp lại`a`từ`1`ĐẾN`n`. 

Điều này chọn chân đầu tiên của tam giác. 
4. Lặp lại`b`từ`a`ĐẾN`n`. 

Bắt đầu từ`a`đảm bảo`a ≤ b`, do đó mỗi tam giác được tính đúng một lần. 
5. Tính toán:```
s = a*a + b*b
```Đây là hình vuông của cạnh huyền. 
6. Tính toán:```
c = int(s ** 0.5)
```Chúng ta chỉ cần kiểm tra xem`s`là một hình vuông hoàn hảo 
7. Xác minh cả hai điều kiện:```
c * c == s
c <= n
```Điều kiện đầu tiên xác nhận rằng`c`là một số nguyên. 

Thứ hai đảm bảo tam giác tôn trọng giới hạn vấn đề. 
8. Nếu cả hai điều kiện đều đúng, hãy tăng câu trả lời. 
9. In câu trả lời cuối cùng. 

### Tại sao nó hoạt động 

Thuật toán kiểm tra mọi cặp thứ tự có thể`(a,b)`thỏa mãn`1 ≤ a ≤ b ≤ n`. 

Với mỗi cặp, có đúng một giá trị có thể có của`c`có thể thỏa mãn phương trình Pythagore. Thuật toán tính toán ứng viên đó và kiểm tra xem đó có phải là số nguyên hợp lệ trong giới hạn hay không. 

Mỗi tam giác hợp lệ xuất hiện chính xác một lần vì vòng lặp thực thi`a ≤ b`. Không có tam giác không hợp lệ nào có thể được tính vì đẳng thức:$a^2+b^2=c^2⟦PROTECT_30⟧b⟦PROTECT_31⟧a^2 + b^2 = c^2 \approx 225.00 + 225.00 = 450.00$Điều chỉnh a và b cập nhật tam giác và giá trị dẫn xuất của c.abc 

được kiểm tra trực tiếp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())

answer = 0

for a in range(1, n + 1):
    for b in range(a, n + 1):
        s = a * a + b * b
        c = int(s ** 0.5)

        if c <= n and c * c == s:
            answer += 1

print(answer)
```Vòng ngoài chọn chặng đầu tiên`a`. Vòng lặp bên trong bắt đầu từ`a`thay vì`1`, tự động loại bỏ các thứ tự trùng lặp. 

Biến`s`cửa hàng:$a^2+b^2$Mã tính toán ứng cử viên căn bậc hai số nguyên`c`bằng cách sử dụng chuyển đổi căn bậc hai và số nguyên dấu phẩy động. Chi tiết quan trọng là bước xác nhận:```
c * c == s
```Nếu không có kiểm tra này, lỗi làm tròn có thể phân loại không chính xác các hình không phải là hình vuông thành hình vuông. 

Điều kiện:```
c <= n
```ngăn cản việc đếm các tam giác có cạnh huyền vượt quá giới hạn. 

Không cần thêm mảng hoặc cấu trúc dữ liệu nên mức sử dụng bộ nhớ không đổi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
```| một | b | s = a2+b2 | c | Có hiệu lực? | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | 1 | Không | 0 | 
| 1 | 2 | 5 | 2 | Không | 0 | 
| 1 | 3 | 10 | 3 | Không | 0 | 
| 1 | 4 | 17 | 4 | Không | 0 | 
| 1 | 5 | 26 | 5 | Không | 0 | 
| 2 | 2 | 8 | 2 | Không | 0 | 
| 2 | 3 | 13 | 3 | Không | 0 | 
| 2 | 4 | 20 | 4 | Không | 0 | 
| 2 | 5 | 29 | 5 | Không | 0 | 
| 3 | 4 | 25 | 5 | Có | 1 | 

Bộ ba hợp lệ duy nhất là`(3,4,5)`. Điều kiện đặt hàng tránh tính`(4,3,5)`riêng. 

### Ví dụ 2 

đầu vào:```
10
```| một | b | s | c | Có hiệu lực? | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 3 | 4 | 25 | 5 | Có | 1 | 
| 6 | 8 | 100 | 10 | Có | 2 | 
| Các cặp còn lại | - | - | - | Không | 2 | 

Các hình tam giác hợp lệ là`(3,4,5)`Và`(6,8,10)`. 

Ví dụ này chứng minh rằng nhiều phiên bản chia tỷ lệ của bộ ba Pythagore nguyên thủy được tính riêng biệt miễn là tất cả các cạnh vẫn nằm trong giới hạn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Hai vòng lặp lồng nhau`a`Và`b`| 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ | 

Với`n = 10^4`, thuật toán thực hiện khoảng 50 triệu lần lặp trong trường hợp xấu nhất. Mỗi lần lặp chỉ thực hiện một vài phép tính số học, phù hợp thoải mái trong giới hạn của Python. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())

    answer = 0

    for a in range(1, n + 1):
        for b in range(a, n + 1):
            s = a * a + b * b
            c = int(s ** 0.5)

            if c <= n and c * c == s:
                answer += 1

    print(answer)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()

    backup_stdout = sys.stdout
    sys.stdout = out

    solve()

    sys.stdout = backup_stdout
    return out.getvalue()

# provided sample
assert run("5\n") == "1\n", "sample 1"

# minimum input
assert run("1\n") == "0\n", "no triangle possible"

# first valid triangle appears
assert run("3\n") == "0\n", "3-4-5 not fully included"

# exact boundary inclusion
assert run("5\n") == "1\n", "3-4-5 counted"

# multiple triples
assert run("10\n") == "2\n", "3-4-5 and 6-8-10"

# larger case
assert run("25\n") == "8\n", "multiple primitive and scaled triples"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`0`| Ranh giới tối thiểu | 
|`3`|`0`| Cạnh huyền vượt quá giới hạn | 
|`5`|`1`| Bộ ba Pythagore hợp lệ đầu tiên | 
|`10`|`2`| Nhiều bộ ba hợp lệ | 
|`25`|`8`| Không gian tìm kiếm lớn hơn và hành vi mở rộng quy mô | 

## Vỏ cạnh 

Hãy xem xét đầu vào nhỏ nhất có thể:```
1
```Kiểm tra chỉ vòng lặp`(1,1)`. 

Thuật toán tính toán:```
1² + 1² = 2
```Từ`2`không phải là hình vuông hoàn hảo nên không có hình tam giác nào được tính. Đầu ra trở thành:```
0
```Điều này xác nhận thuật toán xử lý chính xác các trường hợp không tồn tại tam giác hợp lệ. 

Bây giờ hãy xem xét vấn đề đếm trùng lặp:```
5
```Một triển khai ngây thơ lặp lại độc lập trên tất cả`a`Và`b`sẽ tính cả hai:```
(3,4,5)
(4,3,5)
```Thuật toán hiện tại tránh điều này vì vòng lặp bên trong bắt đầu từ`b = a`. Một lần`(3,4)`được xử lý,`(4,3)`không bao giờ xuất hiện. 

Cuối cùng, hãy xem xét trường hợp cạnh huyền vượt quá giới hạn:```
10
```Cặp đôi`(8,15)`thỏa mãn phương trình Pythagore vì:$8^2+15^2=17^2$Nhưng`17 > 10`, do đó điều kiện:```
c <= n
```từ chối nó một cách chính xác. Nếu không có sự kiểm tra đó, thuật toán sẽ đếm quá số lượng các hình tam giác không hợp lệ.
