---
title: "CF 1765M - LCM tối thiểu"
description: "Chúng ta được yêu cầu chia một số nguyên n cho trước thành hai phần dương a và b sao cho tổng của chúng không đổi ở n. Trong số tất cả các phép chia như vậy, chúng ta muốn cặp sao cho bội số chung nhỏ nhất của a và b càng nhỏ càng tốt."
date: "2026-06-09T13:16:04+07:00"
tags: ["codeforces", "competitive-programming", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1765
codeforces_index: "M"
codeforces_contest_name: "2022-2023 ICPC, NERC, Southern and Volga Russian Regional Contest (Online Mirror, ICPC Rules, Preferably Teams)"
rating: 1000
weight: 1765
solve_time_s: 86
verified: true
draft: false
---

[CF 1765M - LCM tối thiểu](https://codeforces.com/problemset/problem/1765/M) 

**Đánh giá:** 1000 
**Tags:** toán, lý thuyết số 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu chia một số nguyên nhất định`n`thành hai phần tích cực`a`Và`b`sao cho tổng của chúng cố định ở`n`. Trong số tất cả các phép chia như vậy, chúng ta muốn cặp nào tạo ra bội số chung nhỏ nhất của`a`Và`b`càng nhỏ càng tốt. 

Vì vậy, đầu vào chỉ là một chuỗi các ca kiểm thử, mỗi ca cung cấp một số duy nhất`n`. Đối với mỗi`n`, về mặt khái niệm, chúng tôi đặt một vết cắt để phá vỡ`n`thành hai số nguyên dương và chúng tôi đo lường mức độ “tương thích” của cặp này bằng cách sử dụng LCM. Mục tiêu là tìm ra sự phân chia trong đó biện pháp tương thích này được giảm thiểu. 

Ràng buộc`n ≤ 10^9`có nghĩa là chúng ta không thể thử tất cả các phân chia có thể có cho một trường hợp thử nghiệm một cách ngây thơ cho đến`n/2`và hy vọng nó sẽ vượt qua trong trường hợp xấu nhất nếu chúng ta làm bất cứ điều gì tốn kém cho mỗi lần chia tách. Quét tuyến tính cho mỗi trường hợp thử nghiệm sẽ ở mức 10^9 trên nhiều trường hợp, nhưng quan trọng hơn, nó báo hiệu rằng giải pháp phải giảm vấn đề xuống một số thứ như kiểm tra hệ số hoặc logarit. 

Một trường hợp khó nhận thấy là khi`n`nhỏ, đặc biệt là`n = 2`. Trong trường hợp đó, sự phân chia duy nhất là`1 + 1`, do đó, bất kỳ công thức nào dựa trên ước số hoặc ước số thực lớn nhất vẫn phải xử lý thực tế là "ước số tốt nhất" có thể là`1`. Một trường hợp cạnh khác là số lẻ như`n = 9`, trong đó trực giác có thể gợi ý sự phân chia cân bằng như`4 + 5`, nhưng những điều đó không nhất thiết phải tối ưu theo LCM; trên thực tế, các cặp có cấu trúc hơn như`3 + 6`thực hiện tốt hơn. Điều này cho thấy rằng chỉ tính đối xứng thôi thì không phải là phương pháp phỏng đoán chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là thử từng cặp`(a, b)`như vậy`a + b = n`, tính toán`lcm(a, b)`, và lấy giá trị nhỏ nhất Vì có khoảng`n`sự lựa chọn cho`a`, điều này dẫn đến`O(n)`làm việc cho mỗi trường hợp thử nghiệm. Với`n`lên đến`10^9`, điều này hoàn toàn không thể thực hiện được. 

Cấu trúc khóa xuất phát từ việc viết lại biểu thức LCM. Đối với bất kỳ cặp nào`(a, b)`, chúng ta có thể bày tỏ`a = g x`Và`b = g y`, Ở đâu`g = gcd(a, b)`Và`x`Và`y`là nguyên tố cùng nhau. Ràng buộc tổng trở thành`g(x + y) = n`, Vì thế`g`phải chia`n`. Điều này thay đổi vấn đề từ tìm kiếm trên tất cả các phần tách sang tìm kiếm trên các ước của`n`. 

Khi tổng được cố định theo`x + y = n / g`, sản phẩm`x * y`xác định LCM là`lcm(a, b) = g * x * y`. Đối với một khoản tiền cố định, sản phẩm`x * y`được giảm thiểu khi một giá trị`1`và cái còn lại là`k - 1`. Điều này đẩy LCM đến`g * (k - 1)`đơn giản hóa để`n - g`. 

Bây giờ mục tiêu trở nên cực kỳ đơn giản: tối đa hóa`g`dưới sự ràng buộc đó`g`là ước số thích hợp của`n`. Điều tốt nhất có thể`g`là ước số lớn nhất nhỏ hơn`n`, có được bằng cách chia`n`bởi thừa số nguyên tố nhỏ nhất của nó. 

Brute Force hoạt động bằng cách liệt kê tất cả các phần tách, nhưng không thành công vì nó bỏ qua cấu trúc số chia. Nhận xét rằng LCM phụ thuộc vào gcd và tổng làm giảm bài toán tìm một thừa số nguyên tố duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) mỗi lần kiểm tra | O(1) | Quá chậm | 
| Dựa trên hệ số nguyên tố | O(√n) mỗi lần kiểm tra | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Bây giờ chúng ta chuyển lý luận thành một thủ tục trực tiếp. 

1. Với mỗi test case, hãy đọc`n`. Mục tiêu là xác định sự phân chia trong đó cấu trúc gcd ngầm được tối đa hóa. 
2. Tìm thừa số nguyên tố nhỏ nhất`p`của`n`. Điều này là đủ vì chia`n`qua`p`cho ước số thực sự lớn nhất của`n`. 
3. Tính toán`g = n / p`. Giá trị này đại diện cho gcd tốt nhất có thể cho một cặp hợp lệ`(a, b)`. 
4. Xây dựng cặp như`a = g`Và`b = n - g`. Điều này đảm bảo rằng`a + b = n`và cấu trúc gcd phù hợp với hệ số tối ưu. 
5. Đầu ra`(a, b)`theo bất kỳ thứ tự nào. 

Lý do công trình này hoạt động là vì một khi chúng tôi sửa chữa`g`như một ước số của`n`, số tiền còn lại chia thành`(1, k - 1)`ở mức độ giảm các biến số, giúp tối thiểu hóa sản phẩm nội bộ và do đó giảm thiểu LCM. 

### Tại sao nó hoạt động 

Bất kỳ cặp hợp lệ nào cũng có thể được viết là`(g x, g y)`với`x + y = n / g`. LCM trở thành`g x y`. Để cố định`g`, giảm thiểu LCM giảm đến giảm thiểu`x y`, được giảm thiểu ở mức mất cân bằng cực độ`(1, k - 1)`. Sự tự do còn lại là lựa chọn`g`. Vì biểu thức cuối cùng đơn giản hóa thành`n - g`, tối đa hóa`g`đưa ra giải pháp tối ưu và ước số thực sự lớn nhất của`n`chính xác là`n`chia cho thừa số nguyên tố nhỏ nhất của nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def smallest_prime_factor(n: int) -> int:
    if n % 2 == 0:
        return 2
    d = 3
    while d * d <= n:
        if n % d == 0:
            return d
        d += 2
    return n

t = int(input())
for _ in range(t):
    n = int(input())
    p = smallest_prime_factor(n)
    g = n // p
    a = g
    b = n - g
    print(a, b)
```Hàm trợ giúp tính toán hệ số nguyên tố nhỏ nhất một cách hiệu quả bằng cách kiểm tra khả năng chia hết cho đến`sqrt(n)`. Khi chúng tôi tìm thấy nó, chúng tôi sẽ ngay lập tức tìm ra ứng cử viên gcd tối ưu`g`. 

Việc xây dựng`b = n - g`là có chủ ý: nó tránh bất kỳ sự tính toán lại nào của cấu trúc bổ sung và trực tiếp thực thi ràng buộc tổng. 

Một sai lầm phổ biến là quay trở lại`(g, g)`, nhưng điều đó chỉ có tác dụng khi`n`là số chẵn và`g = n/2`. Công thức đúng luôn bảo toàn điều kiện tổng thông qua phép trừ. 

## Ví dụ đã hoạt động 

Hãy xem xét`n = 9`. 

| Bước | n | thừa số nguyên tố nhỏ nhất p | g = n/p | một | b | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | 9 | - | - | - | - | 
| yếu tố | 9 | 3 | - | - | - | 
| tính toán | 9 | 3 | 3 | 3 | 6 | 

Điều này tạo ra`(3, 6)`. Các cách chia khác như`(4, 5)`hoặc`(1, 8)`mang lại giá trị LCM lớn hơn vì chúng tương ứng với cấu trúc gcd hiệu quả nhỏ hơn, làm tăng biểu thức`n - g`. 

Bây giờ hãy xem xét`n = 10`. 

| Bước | n | thừa số nguyên tố nhỏ nhất p | g = n/p | một | b | 
| --- | --- | --- | --- | --- | --- | 
| ban đầu | 10 | - | - | - | - | 
| yếu tố | 10 | 2 | - | - | - | 
| tính toán | 10 | 2 | 5 | 5 | 5 | 

Điều này mang lại sự phân chia đối xứng, tối ưu vì ước số thích hợp lớn nhất chính xác là`5`. 

Những ví dụ này xác nhận rằng thuật toán dịch chuyển một cách tự nhiên giữa các phần chia đối xứng và không đối xứng tùy thuộc vào việc liệu`n`có một thừa số nguyên tố nhỏ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(√n) mỗi lần kiểm tra | tìm kiếm thừa số nguyên tố nhỏ nhất lên tới √n | 
| Không gian | O(1) | chỉ có một vài biến được sử dụng | 

Các ràng buộc cho phép tối đa 100 trường hợp kiểm thử và mỗi trường hợp`n`nhiều nhất là`10^9`. Hệ số căn bậc hai cho mỗi trường hợp thử nghiệm dễ dàng đủ nhanh trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def spf(n):
        if n % 2 == 0:
            return 2
        d = 3
        while d * d <= n:
            if n % d == 0:
                return d
            d += 2
        return n

    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        p = spf(n)
        g = n // p
        out.append(f"{g} {n - g}")
    return "\n".join(out)

# provided samples
assert solve("4\n2\n9\n5\n10\n") == "1 1\n3 6\n1 4\n5 5"

# minimum case
assert solve("1\n2\n") == "1 1"

# prime n
assert solve("1\n7\n") == "1 6"

# even composite
assert solve("1\n12\n") in ["6 6"]

# power of two
assert solve("1\n16\n") == "8 8"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 | 1 1 | ranh giới nhỏ nhất | 
| 7 | 1 6 | hành vi chính | 
| 12 | 6 6 | tổng hợp chia đều | 
| 16 | 8 8 | lũy thừa hai đối xứng | 

## Vỏ cạnh 

cho`n = 2`, thuật toán tìm thừa số nguyên tố nhỏ nhất`2`, Vì thế`g = 1`và đầu ra`(1, 1)`, đây là phép chia hợp lệ duy nhất. 

Đối với một số nguyên tố`n`, nói`n = 7`, thừa số nguyên tố nhỏ nhất là`7`, Vì thế`g = 1`và kết quả trở thành`(1, 6)`. Điều này phù hợp với thực tế là không tồn tại cấu trúc ước số không tầm thường, do đó nghiệm suy biến đến mức mất cân bằng cực độ. 

Đối với sức mạnh của hai như`n = 16`, thừa số nguyên tố nhỏ nhất là`2`, Vì thế`g = 8`, sản xuất`(8, 8)`. Điều này cho thấy thuật toán phát hiện một cách tự nhiên khi nào phép chia đối xứng là tối ưu vì ước số thích hợp lớn nhất chính xác bằng một nửa`n`.
