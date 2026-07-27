---
title: "CF 102780D - Phát điện"
description: "Chúng ta cần tìm số nguyên dương x nhỏ nhất sao cho lũy thừa của a với số mũ x bằng lũy ​​thừa của x với số mũ b. Hai giá trị đầu vào là các cơ số liên quan đến phương trình này và câu trả lời là x hợp lệ nhỏ nhất không vượt quá 10^18."
date: "2026-07-27T20:07:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102780
codeforces_index: "D"
codeforces_contest_name: "ICPC Central Russia Regional Contest (CRRC 19)"
rating: 0
weight: 102780
solve_time_s: 71
verified: true
draft: false
---

[CF 102780D - Trò chơi mạnh mẽ](https://codeforces.com/problemset/problem/102780/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần tìm số nguyên dương x nhỏ nhất sao cho lũy thừa của a với số mũ x bằng lũy thừa của x với số mũ b. Hai giá trị đầu vào là các cơ số liên quan đến phương trình này và câu trả lời là x hợp lệ nhỏ nhất không vượt quá 10^18. Nếu không có số nguyên như vậy tồn tại, chúng ta in 0. 

Giá trị của a và b nhiều nhất là 10000, do đó không gian tìm kiếm cho chính đầu vào là rất nhỏ nhưng x có thể cực kỳ lớn. Không thể tìm kiếm trực tiếp trên các giá trị x có thể có vì câu trả lời có thể gần với giới hạn trên là 10^18. Lời giải phải sử dụng cấu trúc toán học của phương trình thay vì thử từng ứng viên một. 

Khó khăn tiềm ẩn chính là x xuất hiện cả dưới dạng cơ số và số mũ. Một giải pháp bất cẩn có thể cố gắng so sánh logarit dấu phẩy động, nhưng lỗi chính xác có thể biến phép kiểm tra đẳng thức thành kết quả không chính xác. Một sai lầm phổ biến khác là cho rằng giải pháp luôn tồn tại vì một số ví dụ nhỏ có hiệu quả. 

Ví dụ, đầu vào`2 6`không có giải pháp và đầu ra đúng là`0`. Một tìm kiếm chỉ kiểm tra một số lượng nhỏ các ứng cử viên có thể dừng quá sớm một cách không chính xác và bỏ lỡ thực tế là không có giá trị nào hoạt động. 

Một trường hợp cạnh khác là khi x hợp lệ không bằng a. Đối với đầu vào`2 4`, câu trả lời là`16`, bởi vì`2^16 = 16^4`. Đoán x từ cơ sở hoặc chỉ kiểm tra các giá trị x liên quan đến a sẽ bỏ sót giải pháp thực tế. 

Trường hợp thứ ba xuất hiện khi các thừa số nguyên tố của a có cấu trúc số mũ chung. Đối với đầu vào`100 20`, câu trả lời là`10`. Giải pháp đến từ việc giảm số mũ nguyên tố của a chứ không phải từ việc thử các giá trị gần với a. 

## Phương pháp tiếp cận 

Một giải pháp vũ phu sẽ kiểm tra mọi số nguyên x từ 1 đến 10^18 và kiểm tra xem`a^x = x^b`. Việc kiểm tra đẳng thức có thể được thực hiện bằng số học số nguyên, do đó phương pháp này sẽ đúng, nhưng số lượng ứng cử viên vượt xa những gì chương trình có thể xử lý. Ngay cả một thao tác cho mỗi ứng viên cũng sẽ cần khoảng 10^18 thao tác, trong khi giới hạn một giây chỉ cho phép số lượng thao tác nhỏ hơn nhiều. 

Quan sát hữu ích đến từ việc xem xét các thừa số nguyên tố. Giả sử hệ số nguyên tố của a là:```
a = p1^c1 * p2^c2 * ... * pk^ck
```Vế phải của phương trình là`x^b`, do đó mọi thừa số nguyên tố của x đều phải xuất hiện trong a. Nếu x chứa một số nguyên tố mới thì số nguyên tố đó sẽ xuất hiện ở bên phải nhưng không xuất hiện ở bên trái. 

Với mọi số nguyên tố pi, gọi số mũ của nó trong x là ei. So sánh số mũ nguyên tố ở cả hai vế cho:```
x * ci = b * ei
```Điều này có nghĩa là mọi số mũ ei đều được điều khiển bởi x. Trước tiên chúng ta loại bỏ ước số chung của b và tất cả các số mũ của a. Cho phép:```
g = gcd(b, c1, c2, ..., ck)
```Sau đó:```
b = g * b'
ci = g * ci'
```Các phương trình trở thành:```
ei = x * ci' / b'
```nên b' phải chia x. Viết:```
x = b' * n
```Thay thế điều này vào công thức số mũ sẽ cho:```
ei = n * ci'
```Vì thế:```
x = p1^(n*c1') * p2^(n*c2') * ... * pk^(n*ck')
```Điều này có thể được viết lại như sau:```
x = (p1^c1' * p2^c2' * ... * pk^ck')^n
```Cho phép:```
q = p1^c1' * p2^c2' * ... * pk^ck'
```Bây giờ toàn bộ vấn đề trở thành tìm số nguyên dương n thỏa mãn:```
q^n = b' * n
```Giá trị của q ít nhất là 2. Vì x = q^n không được vượt quá 10^18 nên n chỉ có thể ở khoảng 60. Chúng ta có thể đơn giản kiểm tra tất cả các giá trị n có thể có trong phạm vi nhỏ này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(10^18) | O(1) | Quá chậm | 
| Tối ưu | O(log(10^18) + sqrt(a)) | O(số thừa số nguyên tố của a) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích a và lưu trữ từng số mũ nguyên tố. Điều này mang lại các giá trị ci cần thiết để so sánh lũy thừa nguyên tố ở cả hai vế của phương trình. 
2. Tính toán`g`, ước chung lớn nhất của b và mọi số mũ từ việc phân tích thành nhân tử của a. Chia cho g sẽ loại bỏ phần chia sẻ không cần thiết và để lại các giá trị rút gọn b' và ci'. 
3. Xây dựng q sử dụng số mũ rút gọn:```
q = product of p^(ci/g)
```Giá trị này đại diện cho cơ số có lũy thừa có thể trở thành x sau khi rút gọn. 

1. Thử tăng giá trị của n bắt đầu từ 1. Với mỗi n, hãy tính`q^n`trong khi dừng lại nếu vượt quá 10^18. Nếu như:```
q^n == b' * n
```sau đó`q^n`là một x hợp lệ. Vì n được kiểm tra theo thứ tự tăng dần và q lớn hơn 1 nên câu trả lời đầu tiên tìm được là câu trả lời nhỏ nhất. 

1. Nếu tất cả các giá trị có thể có của n đều không đạt, xuất 0 vì không có x hợp lệ nào tồn tại trong phạm vi cho phép. 

Tại sao nó hoạt động: 

Việc so sánh thừa số nguyên tố biến phương trình ban đầu thành một phương trình nhỏ hơn nhiều. Mọi giải pháp khả thi đều phải có dạng`x = q^n`và mọi n hợp lệ phải thỏa mãn`q^n = b'n`. Thuật toán kiểm tra chính xác các giá trị có thể này, do đó nó không thể bỏ sót một giải pháp nào. Vì việc tìm kiếm bắt đầu từ n nhỏ nhất nên kết quả khớp đầu tiên sẽ cho kết quả x nhỏ nhất có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

LIMIT = 10**18

def factorize(x):
    factors = []
    d = 2
    while d * d <= x:
        if x % d == 0:
            cnt = 0
            while x % d == 0:
                x //= d
                cnt += 1
            factors.append((d, cnt))
        d += 1 if d == 2 else 2
    if x > 1:
        factors.append((x, 1))
    return factors

def solve():
    a, b = map(int, input().split())

    factors = factorize(a)

    g = b
    for _, c in factors:
        g = __import__("math").gcd(g, c)

    b_red = b // g

    q = 1
    for p, c in factors:
        q *= p ** (c // g)

    power = 1
    n = 1
    while True:
        power *= q
        if power > LIMIT:
            break

        if power == b_red * n:
            print(power)
            return

        n += 1

    print(0)

if __name__ == "__main__":
    solve()
```Hàm phân tích nhân tử trích xuất biểu diễn nguyên tố của a. Giới hạn đầu vào là 10000 giúp phân chia thử nghiệm đủ nhanh một cách dễ dàng. 

Phép tính ước chung lớn nhất làm giảm số mũ chính xác như được rút ra trong chứng minh. Biến`q`lưu trữ cơ số rút gọn, vì vậy mọi câu trả lời ứng cử viên đều có dạng được tạo bằng cách nhân liên tục với q. 

Vòng lặp bắt đầu với`power = q`, biểu thị n bằng 1. Mỗi lần lặp tăng n lên một và cập nhật q^n. Khi giá trị vượt quá 10^18, các giá trị sau này thậm chí còn lớn hơn vì q ít nhất bằng 2, do đó việc tìm kiếm có thể dừng lại một cách an toàn. 

Tất cả các phép tính đều sử dụng số nguyên Python nên không có rủi ro tràn dữ liệu. Việc kiểm tra giới hạn rõ ràng ngăn chặn sự phát triển không cần thiết của các quyền lực khổng lồ. 

## Ví dụ đã hoạt động 

Đối với đầu vào`2 4`, hệ số hóa là: 

| n | q^n | b' * n | Kết quả | 
| --- | --- | --- | --- | 
| 1 | 2 | 4 | Không bằng | 
| 2 | 4 | 8 | Không bằng | 
| 3 | 8 | 12 | Không bằng | 
| 4 | 16 | 16 | Tìm thấy | 

Đây`g = gcd(4, 1) = 1`, Vì thế`q = 2`Và`b' = 4`. Kết quả đầu tiên n là 4, cho x = 16. 

Đối với đầu vào`2 6`, phương trình rút gọn là: 

| n | q^n | b' * n | Kết quả | 
| --- | --- | --- | --- | 
| 1 | 2 | 6 | Không bằng | 
| 2 | 4 | 12 | Không bằng | 
| 3 | 8 | 18 | Không bằng | 
| 4 | 16 | 24 | Không bằng | 
| 5 | 32 | 30 | Không bằng | 
| 6 | 64 | 36 | Không bằng | 

Các lũy thừa của 2 cuối cùng tăng nhanh hơn biểu thức tuyến tính 6n và không có đẳng thức nào xuất hiện trước khi đạt đến giới hạn giá trị. Thuật toán in 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(sqrt(a) + log(10^18)) | Hệ số hóa kiểm tra các ước số lên tới sqrt(a) và tìm kiếm chỉ thử khoảng 60 giá trị của n | 
| Không gian | O(k) | Danh sách thừa số chỉ lưu trữ các thừa số nguyên tố của | 

Các ràng buộc trên a và b làm cho việc phân tích nhân tử trở nên không tốn kém. Không gian tìm kiếm được chuyển đổi rất nhỏ vì câu trả lời bị giới hạn bởi 10^18 và q ít nhất là 2, do đó thuật toán dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io
import math

def solution(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    a, b = map(int, input().split())

    def factorize(x):
        factors = []
        d = 2
        while d * d <= x:
            if x % d == 0:
                cnt = 0
                while x % d == 0:
                    x //= d
                    cnt += 1
                factors.append((d, cnt))
            d += 1 if d == 2 else 2
        if x > 1:
            factors.append((x, 1))
        return factors

    factors = factorize(a)
    g = b
    for _, c in factors:
        g = math.gcd(g, c)

    b_red = b // g
    q = 1
    for p, c in factors:
        q *= p ** (c // g)

    power = 1
    n = 1
    while True:
        power *= q
        if power > 10**18:
            break
        if power == b_red * n:
            return str(power)
        n += 1

    return "0"

assert solution("2 4") == "16", "sample 1"
assert solution("2 6") == "0", "sample 2"
assert solution("2 32") == "256", "sample 3"
assert solution("100 20") == "10", "sample 4"

assert solution("2 3") == "0", "no solution at small values"
assert solution("10000 9999") == "0", "maximum input size"
assert solution("8 4") == "0", "equal prime exponent pattern"
assert solution("4 8") == "16", "reduced exponent boundary"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 3`|`0`| Giá trị nhỏ nhất không có lời giải hợp lệ | 
|`10000 9999`|`0`| Giá trị đầu vào lớn và loại bỏ nhanh chóng | 
|`8 4`|`0`| Các trường hợp cấu trúc số mũ chung không tạo ra đáp án | 
|`4 8`|`16`| Rút gọn số mũ và tìm lời giải không hề tầm thường | 

## Vỏ cạnh 

cho`2 6`, thuật toán tìm`g = 1`,`q = 2`, Và`b' = 6`. Nó kiểm tra tất cả các giá trị n có thể có trong khi`2^n`vẫn ở dưới mức giới hạn. Không có giá trị thỏa mãn`2^n = 6n`, do đó kết quả là 0. Điều này xử lý tình huống trong đó một phương trình có vẻ hợp lệ không có nghiệm nguyên. 

Vì`2 4`, thuật toán không cho rằng câu trả lời gần với a. Nó xây dựng phương trình rút gọn`2^n = 4n`, tìm thấy n bằng 4 và trả về`2^4 = 16`. Điều này xác nhận rằng x được tạo có thể lớn hơn nhiều so với cơ sở ban đầu. 

Vì`100 20`, việc nhân tử hóa cho`100 = 2^2 * 5^2`. Gcd của b và số mũ là 2, tạo ra`q = 10`Và`b' = 10`. Việc kiểm tra đầu tiên cho`10^1 = 10 * 1`, do đó thuật toán trả về 10 ngay lập tức. Điều này bao gồm trường hợp việc giảm số mũ nguyên tố tạo ra nghiệm nhỏ. 

Tôi cũng có thể điều chỉnh bài xã luận này thành một phiên bản ngắn hơn theo phong cách Codeforces nếu bạn muốn có định dạng xuất bản cuộc thi nhỏ gọn hơn.
