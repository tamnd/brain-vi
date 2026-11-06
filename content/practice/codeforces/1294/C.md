---
title: "CF 1294C - Tích của ba số"
description: "Với mỗi test, chúng ta được cấp một số nguyên n. Chúng ta phải xác định xem n có thể được viết dưới dạng tích của ba số nguyên riêng biệt a, b và c hay không, trong đó mỗi số nguyên ít nhất bằng 2. Nếu sự phân tách như vậy tồn tại, chúng ta in ra \"CÓ\" và một bộ ba hợp lệ."
date: "2026-06-11T18:40:37+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1294
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 615 (Div. 3)"
rating: 1300
weight: 1294
solve_time_s: 143
verified: true
draft: false
---

[CF 1294C - Tích của ba số](https://codeforces.com/problemset/problem/1294/C) 

**Đánh giá:** 1300 
**Tags:** tham lam, toán học, lý thuyết số 
**Thời gian giải:** 2m 23s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Đối với mỗi trường hợp thử nghiệm, chúng tôi được cung cấp một số nguyên duy nhất`n`. Chúng ta phải xác định liệu`n`có thể được viết dưới dạng tích của ba số nguyên **khác biệt**`a`,`b`, Và`c`, trong đó mỗi cái ít nhất`2`. 

Nếu sự phân rã như vậy tồn tại, chúng tôi in`"YES"`và một bộ ba hợp lệ. Nếu không có bộ ba như vậy tồn tại, chúng tôi in`"NO"`. 

Đầu vào chứa tối đa 100 trường hợp thử nghiệm và mỗi giá trị của`n`nhiều nhất là`10^9`. Một số cỡ này vẫn có thể được phân tích thành nhân tử bằng phép chia thử lên đến`√n`, bởi vì`√(10^9) ≈ 31623`. Ngay cả khi chúng tôi thực hiện công việc này cho mọi trường hợp thử nghiệm, tổng lượng tính toán vẫn thoải mái trong giới hạn thời gian. 

Khó khăn chính không phải là việc phân tích nhân tử. Thách thức là tìm ra ba yếu tố hoàn toàn khác nhau. 

Coi như`n = 64 = 2^6`. Có sự phân hủy:```
64 = 2 × 4 × 8
```Cả ba yếu tố đều khác biệt và ít nhất`2`. 

Bây giờ hãy xem xét`n = 32 = 2^5`. 

Các yếu tố có thể có bao gồm:```
32 = 2 × 2 × 8
32 = 2 × 4 × 4
```Mỗi phép phân tách đều lặp lại một thừa số, vì vậy câu trả lời đúng là`"NO"`. 

Một trường hợp tế nhị khác là khi`n`có đúng hai thừa số nguyên tố khác nhau nhưng một trong số chúng chỉ xuất hiện một lần. Ví dụ:```
n = 12 = 2^2 × 3
```Chúng ta có thể chọn`2`Và`3`, rời đi`2`là yếu tố còn lại. Các yếu tố trở thành:```
2, 3, 2
```vốn không khác biệt. Một giải pháp phải từ chối trường hợp này. 

Một ví dụ khác là:```
n = 72 = 2^3 × 3^2
```Lựa chọn`2`Và`3`lá:```
72 / (2 × 3) = 12
```cho:```
2, 3, 12
```đó là hợp lệ. 

Việc triển khai bất cẩn chỉ tìm thấy hai ước số bất kỳ và sử dụng số dư làm thừa số thứ ba có thể vô tình tạo ra các giá trị trùng lặp. 

## Phương pháp tiếp cận 

Một giải pháp vũ phu sẽ thử tất cả các bộ ba có thể`(a, b, c)`như vậy`a × b × c = n`. Một cách là lặp lại tất cả các ước số cho`a`, mọi ước số của`b`, và kiểm tra xem các dạng thương số còn lại có`c`. 

Ngay cả khi chúng ta hạn chế sử dụng ước số, cách tiếp cận này vẫn trở nên không thực tế. Không gian tìm kiếm tăng quá nhanh, đặc biệt khi được thực hiện cho tối đa 100 trường hợp thử nghiệm. 

Quan sát quan trọng là chúng ta không cần tất cả các yếu tố. Chúng ta chỉ cần **ba yếu tố riêng biệt**. 

Giả sử chúng ta trích xuất một ước số không tầm thường`a`từ`n`. Sau khi chia cho`a`, ta thu được số nhỏ hơn Nếu chúng ta có thể trích ra ước số thứ hai`b`điều đó khác với`a`, thì thương số còn lại tự động trở thành:```
c = n / (a × b)
```Vấn đề giảm xuống còn việc tìm hai ước số phân biệt trong khi đảm bảo thương số cuối cùng khác với cả hai. 

Hệ số nguyên tố làm cho việc này trở nên dễ dàng. Chúng tôi liên tục tìm kiếm ước số nhỏ nhất của số hiện tại. 

Ước số đầu tiên được tìm thấy sẽ trở thành`a`. 

Sau khi loại bỏ`a`, chúng tôi tìm kiếm lại. Số chia tiếp theo khác với`a`trở thành`b`. 

Mọi thứ còn lại được giao cho`c`. 

Nếu như`c`lớn hơn`1`và khác biệt với cả hai`a`Và`b`, chúng tôi có một câu trả lời hợp lệ. Nếu không thì không tồn tại bộ ba hợp lệ. 

Điều này có hiệu quả vì bất kỳ sự phân rã hợp lệ nào cuối cùng cũng phải xuất phát từ các thừa số nguyên tố của`n`. Việc trích xuất các thừa số một cách tham lam để lại càng nhiều khối lượng yếu tố càng tốt cho số cuối cùng, giúp dễ dàng thu được ba giá trị riêng biệt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) hoặc tệ hơn | O(1) | Quá chậm | 
| Tối ưu | O(√n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ giá trị ban đầu của`n`. 
2. Tìm ước số nhỏ nhất của`n`lớn hơn`1`. 

Đặt ước số này là`a`. 
3. Nếu không tồn tại ước số thì`n`là nguyên tố. 

Một số nguyên tố không thể viết dưới dạng tích của ba số nguyên lớn hơn`1`, vì vậy hãy in`"NO"`. 
4. Chia`n`qua`a`. 
5. Tìm ước số khác của số đã rút gọn. 

Số chia phải khác với`a`. 
6. Gọi số chia này là`b`. 
7. Nếu không tồn tại ước số đó, hãy in`"NO"`. 
8. Tính toán```
c = original_n / (a × b)
```9. Kiểm tra xem`c`khác với cả hai`a`Và`b`, và liệu`c > 1`. 
10. Nếu tất cả các bước kiểm tra đều đạt, hãy in`"YES"`và bộ ba`(a, b, c)`. Nếu không thì in`"NO"`. 

### Tại sao nó hoạt động 

Thuật toán trích xuất hai yếu tố không tầm thường riêng biệt từ`n`. Sau khi loại bỏ chúng, mọi thừa số nguyên tố còn lại sẽ được thu thập vào`c`. 

Nếu tồn tại một câu trả lời hợp lệ thì việc phân tích thành thừa số nguyên tố của`n`chứa đủ bội số và đủ thừa số nguyên tố phân biệt để tạo ra ba số phân biệt. Lấy hai thừa số đầu tiên một cách tham lam để lại phần dư lớn nhất có thể cho`c`, giúp tối đa hóa cơ hội`c`khác với những lựa chọn trước đó. 

Bất cứ khi nào thuật toán đưa ra bộ ba, kết quả sẽ chính xác`n`bởi vì:```
c = n / (a × b)
```và cả ba giá trị đều được kiểm tra là khác biệt và lớn hơn`1`. 

Bất cứ khi nào thuật toán thất bại, không có cách nào để xây dựng ba yếu tố riêng biệt. Cấu trúc nhân tố còn lại quá nhỏ khiến một trong các nhân tố bị lặp lại hoặc trở thành`1`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    n = int(input())
    original = n

    a = -1
    for d in range(2, int(n ** 0.5) + 1):
        if n % d == 0:
            a = d
            n //= d
            break

    if a == -1:
        print("NO")
        continue

    b = -1
    for d in range(2, int(n ** 0.5) + 1):
        if n % d == 0 and d != a:
            b = d
            n //= d
            break

    if b == -1:
        print("NO")
        continue

    c = original // (a * b)

    if c > 1 and c != a and c != b:
        print("YES")
        print(a, b, c)
    else:
        print("NO")
```Vòng lặp đầu tiên tìm kiếm ước số nhỏ nhất của số ban đầu. Điều này trở thành`a`. 

Sau khi chia cho`a`, vòng lặp thứ hai tìm kiếm một ước số khác khác với`a`. Điều này trở thành`b`. 

Thương số còn lại được tính từ số ban đầu chứ không phải từ giá trị đã sửa đổi của`n`. Cả hai cách tiếp cận đều có hiệu quả, nhưng việc sử dụng giá trị ban đầu sẽ làm cho công thức trở nên rõ ràng và tránh được những sai sót khi suy luận về mã. 

Việc kiểm tra tính khác biệt cuối cùng là cần thiết. Không có nó, những trường hợp như`12 = 2 × 3 × 2`sẽ được chấp nhận không chính xác. 

Tất cả số học đều vừa vặn thoải mái bên trong các số nguyên Python và không có nguy cơ tràn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
64
```| Bước | Hiện tại | một | b | Hành động | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 64 | - | - | Bắt đầu nhân tố hóa | 
| Tìm ước số thứ nhất | 32 | 2 | - | Chia cho 2 | 
| Tìm ước số thứ hai | 8 | 2 | 4 | Chia cho 4 | 
| Tính c | 8 | 2 | 4 | c = 64 / (2×4) | 

Bộ ba kết quả là:```
2, 4, 8
```Tất cả các giá trị đều khác biệt và tích của chúng bằng 64. 

### Ví dụ 2 

đầu vào:```
32
```| Bước | Hiện tại | một | b | Hành động | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 32 | - | - | Bắt đầu nhân tố hóa | 
| Tìm ước số thứ nhất | 16 | 2 | - | Chia cho 2 | 
| Tìm ước số thứ hai | 4 | 2 | 4 | Chia cho 4 | 
| Tính c | 4 | 2 | 4 | c = 32 / (2×4) | 

Bộ ba ứng cử viên trở thành:```
2, 4, 4
```Từ`b = c`, các thừa số không khác nhau nên đáp án là`"NO"`. 

Ví dụ này cho thấy tại sao không thể bỏ qua bài kiểm tra tính khác biệt cuối cùng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(√n) | Quét hai phép chia thử lên đến căn bậc hai của giá trị hiện tại | 
| Không gian | O(1) | Chỉ có một vài biến số nguyên được lưu trữ | 

Giá trị đầu vào lớn nhất có thể là`10^9`, căn bậc hai của nó bằng khoảng`31623`. Ngay cả với 100 trường hợp thử nghiệm, khối lượng công việc này vẫn có thể được xử lý dễ dàng trong thời gian giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        original = n

        a = -1
        for d in range(2, int(n ** 0.5) + 1):
            if n % d == 0:
                a = d
                n //= d
                break

        if a == -1:
            out.append("NO")
            continue

        b = -1
        for d in range(2, int(n ** 0.5) + 1):
            if n % d == 0 and d != a:
                b = d
                n //= d
                break

        if b == -1:
            out.append("NO")
            continue

        c = original // (a * b)

        if c > 1 and c != a and c != b:
            out.append("YES")
            out.append(f"{a} {b} {c}")
        else:
            out.append("NO")

    sys.stdout.write("\n".join(out))

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return result

# provided sample structure
out = run("5\n64\n32\n97\n2\n12345\n")
assert out.splitlines()[0] == "YES"
assert out.splitlines()[2] == "NO"
assert out.splitlines()[3] == "NO"
assert out.splitlines()[4] == "NO"

# prime number
assert run("1\n97\n").strip() == "NO"

# smallest input
assert run("1\n2\n").strip() == "NO"

# valid case with two prime factors
res = run("1\n72\n").splitlines()
assert res[0] == "YES"

# power of two that fails
assert run("1\n32\n").strip() == "NO"

# power of two that succeeds
res = run("1\n64\n").splitlines()
assert res[0] == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`97`|`NO`| Số nguyên tố không thể bị phân hủy | 
|`2`|`NO`| Giá trị đầu vào tối thiểu | 
|`72`|`YES`| Nhiều thừa số nguyên tố có lũy thừa lặp lại | 
|`32`|`NO`| Yếu tố trùng lặp xuất hiện trong mọi phân tách | 
|`64`|`YES`| Công suất lớn vẫn có thể tạo ra ba yếu tố riêng biệt | 

## Vỏ cạnh 

Hãy xem xét:```
n = 12
```Thuật toán chọn:```
a = 2
```Sau khi chia:```
n = 6
```Số chia phân biệt tiếp theo là:```
b = 3
```Phần còn lại trở thành:```
c = 12 / (2 × 3) = 2
```Từ`c = a`, bộ ba không phân biệt. Thuật toán in chính xác`"NO"`. 

Coi như:```
n = 64
```Hệ số hóa là:```
64 = 2^6
```Thuật toán trích xuất:```
a = 2
b = 4
c = 8
```Cả ba giá trị đều khác nhau nên nó in ra`"YES"`. 

Coi như:```
n = 97
```Không tìm thấy ước số trong lần tìm kiếm đầu tiên. Vì 97 là số nguyên tố nên không tồn tại sự phân tách thành ba số nguyên lớn hơn 1. Thuật toán in ngay lập tức`"NO"`. 

Coi như:```
n = 36
```Thuật toán tìm thấy:```
a = 2
```Sau đó:```
b = 3
```Cuối cùng:```
c = 36 / (2 × 3) = 6
```Bộ ba`(2, 3, 6)`là hợp lệ và thuật toán in`"YES"`. Trường hợp này chứng tỏ các số có nhiều thừa số nguyên tố tự nhiên tạo ra một phân tách hợp lệ như thế nào.
