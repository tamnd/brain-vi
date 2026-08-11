---
title: "CF 102420D - Chính tả"
description: "Chúng ta có hai số nguyên dương, a và b, được cho dưới dạng chuỗi thập phân và chúng ta xem xét mọi số nguyên từ a đến b. Chúng ta nhân tất cả chúng lại với nhau, sau đó liên tục thay số kết quả bằng tổng các chữ số thập phân của nó cho đến khi chỉ còn lại một chữ số."
date: "2026-08-12T00:40:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102420
codeforces_index: "D"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2019-2020, \u0422\u0440\u0435\u0442\u044c\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430, \u0443\u0441\u043b\u043e\u0436\u043d\u0435\u043d\u043d\u0430\u044f \u043d\u043e\u043c\u0438\u043d\u0430\u0446\u0438\u044f"
rating: 0
weight: 102420
solve_time_s: 182
verified: true
draft: false
---

[CF 102420D - Chính tả](https://codeforces.com/problemset/problem/102420/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3m 2s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Ta có hai số nguyên dương`a`Và`b`, được cho dưới dạng chuỗi thập phân và chúng tôi xem xét mọi số nguyên từ`a`bởi vì`b`. Chúng ta nhân tất cả chúng lại với nhau, sau đó liên tục thay số kết quả bằng tổng các chữ số thập phân của nó cho đến khi chỉ còn lại một chữ số. Đầu ra được yêu cầu là một chữ số cuối cùng. 

Khó khăn chính đó là`a`Và`b`có thể chứa tới 100.000 chữ số. Chúng tôi không thể xây dựng sản phẩm và thậm chí chúng tôi không thể chuyển đổi điểm cuối thành số nguyên máy thông thường một cách an toàn. Một giải pháp phải làm việc trực tiếp với các biểu diễn thập phân và khai thác thuộc tính của chữ số cuối cùng thay vì thực hiện số học đã nêu theo nghĩa đen. 

Tổng chữ số lặp lại là gốc kỹ thuật số. Với mọi số nguyên dương`x`, gốc số của nó được xác định hoàn toàn bởi`x mod 9`: nếu số dư khác 0 thì đáp án là số dư đó, còn số dư 0 tương ứng với chữ số`9`. Do đó, sản phẩm khổng lồ chỉ có ý nghĩa modulo`9`. 

Có hai trường hợp đặc biệt thường gây ra giải pháp không chính xác. Đầu tiên, một sản phẩm có thể được chia cho`9`, cho phần dư`0`, nhưng chữ số cuối cùng là`9`, không`0`. Ví dụ, với đầu vào`9`Và`9`, sản phẩm là`9`, vậy câu trả lời là`9`. Một giải pháp đơn giản là in`product % 9`sẽ in`0`. 

Thứ hai, khoảng có thể rất lớn mặc dù điểm cuối của nó là những chuỗi thập phân khổng lồ. Ví dụ,`1`Và`100000000000000000000000000000`đại diện cho một khoảng thời gian có quá nhiều yếu tố để liệt kê. Một giải pháp cố gắng nhân mọi số nguyên không chỉ chậm mà về cơ bản nó không tương thích với kích thước đầu vào. 

Ngoài ra còn có một ranh giới hữu ích xung quanh các khoảng chứa năm hoặc ít hơn sự khác biệt liên tiếp. Ví dụ,`1`bởi vì`5`cho`120`, có gốc kỹ thuật số là`3`, trong khi`1`bởi vì`6`cho`720`, có gốc kỹ thuật số là`9`. Quá trình chuyển đổi xảy ra vì sáu số nguyên liên tiếp nhất thiết phải chứa hai bội số của`3`, làm cho tích của họ chia hết cho`9`. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp tuân theo tuyên bố chính xác. Bắt đầu bằng một sản phẩm của`1`, chúng ta sẽ nhân với mọi số nguyên từ`a`bởi vì`b`, xây dựng số nguyên khổng lồ thu được, tính tổng các chữ số của nó và lặp lại cho đến khi còn lại một chữ số. Điều này đúng vì nó thực hiện chính xác phép chuyển đổi được yêu cầu. 

Vấn đề là kích thước phạm vi. Vì`a = 1`Và`b = 10^100000 - 1`, có chính xác`10^100000 - 1`các yếu tố, do đó, ngay cả khi bỏ qua chi phí nhân các số nguyên khổng lồ, thuật toán vẫn thực hiện`10^100000 - 2`các thao tác nhân. Bản thân sản phẩm cũng có một số lượng lớn các chữ số. Điều này làm cho vũ lực không thể thực hiện được. 

Quan sát giải quyết được vấn đề là tổng các chữ số bảo toàn một số modulo`9`. Nếu hai số nguyên dương có cùng phần dư modulo`9`, chúng có cùng gốc kỹ thuật số, với phần còn lại`0`đại diện bởi`9`. Do đó, chúng ta chỉ cần tích modulo`9`. 

Điều đó dường như vẫn còn nhiều yếu tố, nhưng modulo`9`cho chúng ta một lối tắt mạnh mẽ hơn nhiều. Sáu số nguyên liên tiếp bất kỳ đều chứa hai bội số của`3`. Do đó, sản phẩm của họ được chia cho`3 * 3 = 9`, do đó mọi khoảng chứa ít nhất sáu số liên tiếp đều có đáp số cuối cùng`9`. Về mặt điểm cuối, điều này có nghĩa là bất cứ khi nào`b - a >= 5`, câu trả lời là ngay lập tức`9`. 

Chỉ còn lại các khoảng có tối đa năm số. Chúng tôi có thể xử lý chúng trực tiếp theo modulo`9`, vì có nhiều nhất năm yếu tố. Chúng ta không cần giá trị thực của chúng, chỉ cần phần dư của chúng theo modulo`9`. Modulo dư của chuỗi thập phân`9`chỉ là tổng các chữ số của nó theo modulo`9`. 

Vấn đề kỹ thuật còn lại là tìm ra sự khác biệt nhỏ`b - a`mà không chuyển đổi chuỗi 100.000 chữ số thành số nguyên. Vì chúng ta đã biết rằng chỉ có sự khác biệt so với`0`bởi vì`5`vấn đề, chúng ta có thể thêm từng giá trị nhỏ này vào chuỗi thập phân`a`và so sánh kết quả với`b`. Việc này chỉ thực hiện một số lần quét tuyến tính không đổi trên đầu vào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ω(10^100000) các phép toán nhân tố trong trường hợp xấu nhất | Thiên văn cho sản phẩm | Quá chậm | 
| Tối ưu | O( | a | + | b | ) | O( | a | + | b | ) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`a`Và`b`dưới dạng dây. Chúng tôi cố tình giữ chúng dưới dạng chuỗi vì điểm cuối có thể có 100.000 chữ số. 
2. Tính toán`a mod 9`bằng cách quét các chữ số của nó và tích lũy phần còn lại. Điều này có thể thực hiện được vì số thập phân thỏa mãn`10 ≡ 1 (mod 9)`, do đó toàn bộ số này bằng tổng các chữ số của nó. 
3. Kiểm tra xem`b >= a + 5`. Chúng ta có thể tính toán`a + 5`sử dụng phép cộng chuỗi thập phân thông thường và so sánh nó với`b`. Nếu điều kiện này đúng thì khoảng chứa ít nhất sáu số nguyên, do đó nó chứa ít nhất hai bội số của`3`. Sản phẩm của họ được chia cho`9`và gốc kỹ thuật số cuối cùng là`9`. 
4. Nếu`b < a + 5`, khoảng chứa tối đa năm số nguyên. Tìm sự khác biệt chính xác`d`bằng cách xây dựng`a`,`a + 1`, ...,`a + 4`dưới dạng chuỗi thập phân và so sánh chúng với`b`. Từ`a <= b`, chính xác một trong các giá trị này phải bằng`b`. 
5. Nhân số dư của`a, a + 1, ..., a + d`modulo`9`. Bởi vì`d <= 4`, đây nhiều nhất là năm phép nhân mô-đun. 
6. Chuyển đổi phần còn lại thành gốc kỹ thuật số. Phần còn lại từ`1`bởi vì`8`đã là câu trả lời rồi. Phần còn lại của`0`có nghĩa là tích là bội số dương của`9`, vậy câu trả lời là`9`. 

Tại sao nó hoạt động: trong suốt thuật toán, thông tin duy nhất bị loại bỏ khỏi sản phẩm là thông tin không thể ảnh hưởng đến gốc kỹ thuật số của nó. Modulo đồng dư`9`được bảo toàn bằng phép nhân nên tích của phần dư có số dư bằng chính tích của sản phẩm thật. Đối với các khoảng có ít nhất sáu số, tích nhất thiết phải chứa hai thừa số chia hết cho`3`, do đó phần dư của nó được đảm bảo bằng 0. Trong những khoảng thời gian ngắn hơn, chúng tôi nhân số dư của mọi yếu tố một cách rõ ràng. Do đó, mọi khoảng thời gian có thể được xử lý bằng một modulo chính xác`9`tính toán và chuyển đổi cuối cùng từ số 0 còn lại sang`9`cho kết quả tổng các chữ số lặp lại một cách chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def add_small(s, x):
    """Return the decimal string representing int(s) + x, where x <= 5."""
    digits = list(s)
    i = len(digits) - 1
    carry = x

    while i >= 0 and carry:
        value = (ord(digits[i]) - 48) + carry
        digits[i] = chr(value % 10 + 48)
        carry = value // 10
        i -= 1

    if carry:
        digits.insert(0, chr(carry + 48))

    return ''.join(digits)

def mod9(s):
    r = 0
    for ch in s:
        r = (r + ord(ch) - 48) % 9
    return r

def solve():
    a = input().strip()
    b = input().strip()

    # Six consecutive integers contain two multiples of 3,
    # so their product is divisible by 9.
    if b >= add_small(a, 5):
        print(9)
        return

    # Here b - a <= 4, so we can find the exact difference
    # by checking only five possibilities.
    diff = 0
    for d in range(5):
        if add_small(a, d) == b:
            diff = d
            break

    result = 1
    current_mod = mod9(a)

    for d in range(diff + 1):
        result = (result * ((current_mod + d) % 9)) % 9

    print(9 if result == 0 else result)

solve()
```các`mod9`chức năng thực hiện quan sát trung tâm. Nó không bao giờ xây dựng số nguyên khổng lồ được biểu thị bằng chuỗi. Mỗi chữ số có thể được xử lý độc lập và phần còn lại luôn nằm trong khoảng`0`Và`8`. 

các`add_small`Hàm chỉ được sử dụng với các giá trị từ`0`bởi vì`5`. Nó thực hiện phép cộng thập phân thông thường từ phải sang trái, do đó, ngay cả điểm cuối 100.000 chữ số cũng được xử lý mà không cần chuyển đổi số nguyên. Việc mang có thể lan truyền qua toàn bộ chuỗi, đó là lý do tại sao vòng lặp phải tiếp tục trong khi`carry`là khác không. Ví dụ, thêm`5`ĐẾN`999`sản xuất chính xác`1004`. 

Sự so sánh`b >= add_small(a, 5)`xử lý trường hợp khoảng thời gian lớn mà không cần tính toán`b - a`. Vì các chuỗi đầu vào không có số 0 đứng đầu nên việc so sánh từ điển thông thường là đủ khi độ dài của chúng khác nhau hoặc khi độ dài của chúng bằng nhau. 

Khi khoảng được biết là chứa tối đa năm số, vòng lặp sẽ kiểm tra điểm cuối chính xác thay vì cố gắng tính một phép trừ lớn chung. Đây là việc sử dụng có chủ ý giới hạn toán học: chỉ có năm ứng cử viên, vì vậy một số phép toán chuỗi không đổi là đủ. 

Vòng lặp nhân sử dụng`(current_mod + d) % 9`cho các giá trị liên tiếp Kể từ khi thêm`d`đến phần dư của`a`mang lại dư lượng của`a + d`modulo`9`, các số nguyên lớn thực tế không bao giờ cần tồn tại. 

Các số nguyên có độ chính xác tùy ý của Python sẽ không tràn đối với các giá trị modulo nhỏ được sử dụng ở đây mà dựa vào Python để phân tích chuỗi thập phân 100.000 chữ số với`int()`là không cần thiết và cũng có thể đạt giới hạn chuyển đổi thập phân của Python. Việc giữ các điểm cuối dưới dạng chuỗi sẽ tránh hoàn toàn vấn đề đó. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, khoảng là từ`1`ĐẾN`5`. Độ dài của nó là năm nên không áp dụng phím tắt sáu số. 

| Bước |`d`| Dư lượng hiện tại | Dư lượng sản phẩm | 
| --- | --- | --- | --- | 
| Bắt đầu | 0 | 1 | 1 | 
| Nhân với`1`| 0 | 1 | 1 | 
| Nhân với`2`| 1 | 2 | 2 | 
| Nhân với`3`| 2 | 3 | 6 | 
| Nhân với`4`| 3 | 4 | 6 | 
| Nhân với`5`| 4 | 5 | 3 | 

Sản phẩm phù hợp với`3 mod 9`, vậy gốc kỹ thuật số của nó là`3`. Điều này phù hợp với tính toán trực tiếp`1 * 2 * 3 * 4 * 5 = 120`, có tổng chữ số là`3`. 

Đối với Mẫu 2, khoảng thời gian là`6`bởi vì`8`. Một lần nữa chỉ có ba yếu tố. 

| Bước |`d`| Dư lượng hiện tại | Dư lượng sản phẩm | 
| --- | --- | --- | --- | 
| Bắt đầu | 0 | 6 | 1 | 
| Nhân với`6`| 0 | 6 | 6 | 
| Nhân với`7`| 1 | 7 | 6 | 
| Nhân với`8`| 2 | 8 | 3 | 

Phần dư cuối cùng là`3`, vậy câu trả lời là`3`. Sản phẩm thực tế là`6 * 7 * 8 = 336`, Và`3 + 3 + 6 = 12`, theo sau là`1 + 2 = 3`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O( | a | + | b | ) | Chỉ thực hiện một số lần quét và cộng số thập phân không đổi, cộng với tối đa năm phép nhân mô-đun. | 
| Không gian | O( | a | + | b | ) | Các chuỗi và số lượng chuỗi thập phân tạm thời không đổi được lưu trữ. | 

Với tối đa 100.000 chữ số cho mỗi điểm cuối, việc quét tuyến tính là hoàn toàn thiết thực. Thuật toán không bao giờ phụ thuộc vào độ lớn số của`a`hoặc`b`, chỉ trên số chữ số thập phân và nó chỉ thực hiện công việc liên tục ngoài những lần quét đó. 

## Trường hợp thử nghiệm```python
import io
import sys

def add_small(s, x):
    digits = list(s)
    i = len(digits) - 1
    carry = x

    while i >= 0 and carry:
        value = (ord(digits[i]) - 48) + carry
        digits[i] = chr(value % 10 + 48)
        carry = value // 10
        i -= 1

    if carry:
        digits.insert(0, chr(carry + 48))

    return ''.join(digits)

def mod9(s):
    r = 0
    for ch in s:
        r = (r + ord(ch) - 48) % 9
    return r

def solve_one(a, b):
    if b >= add_small(a, 5):
        return "9"

    diff = 0
    for d in range(5):
        if add_small(a, d) == b:
            diff = d
            break

    result = 1
    base = mod9(a)

    for d in range(diff + 1):
        result = result * ((base + d) % 9) % 9

    return str(9 if result == 0 else result)

def run(inp: str) -> str:
    data = inp.strip().split()
    a, b = data
    return solve_one(a, b) + "\n"

# Provided samples
assert run("1\n5\n") == "3\n", "sample 1"
assert run("6\n8\n") == "3\n", "sample 2"

# Minimum-size input
assert run("1\n1\n") == "1\n", "minimum input"

# All-equal values where the value is divisible by 9
assert run("9\n9\n") == "9\n", "zero remainder must become digital root 9"

# Boundary just below six consecutive numbers
assert run("4\n8\n") == "6\n", "five-number interval"

# Boundary at six consecutive numbers
assert run("4\n9\n") == "9\n", "six-number interval"

# Large endpoint, same value, without converting it to an ordinary integer
big = "9" * 100000
assert run(big + "\n" + big + "\n") == "9\n", "100000-digit endpoint"

# Large endpoint with difference exactly 5
large_a = "1" + "0" * 99999
large_b = add_small(large_a, 5)
assert run(large_a + "\n" + large_b + "\n") == "9\n", "large boundary difference"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1`|`1`| Xử lý điểm cuối và khoảng thời gian nhỏ nhất có thể | 
|`9 / 9`|`9`| Phân biệt root kỹ thuật số`9`từ kết quả modulo`0`| 
|`4 / 8`|`6`| Kích thước khoảng tối đa không kích hoạt phím tắt sáu số | 
|`4 / 9`|`9`| Ranh giới chính xác trong đó sáu số liên tiếp buộc phải chia hết cho`9`| 
|`9...9 / 9...9`với 100.000 chữ số |`9`| Độ dài điểm cuối tối đa và tính toán modulo dựa trên chuỗi | 
|`10^99999 / (10^99999 + 5)`|`9`| Những chuỗi lớn kết hợp với sự chính xác`b - a = 5`ranh giới | 

## Vỏ cạnh 

Khi nào`a = b`, tích chứa đúng một số nên đáp án đơn giản là gốc số của số đó. Đối với đầu vào`9`Và`9`,`mod9("9")`là số không. Thuật toán không in số 0, nó chuyển số dư 0 thành`9`, cho kết quả đúng`9`. 

Khi khoảng chứa đúng năm số thì không được áp dụng phím tắt. Đối với đầu vào`4`Và`8`, các yếu tố là`4, 5, 6, 7, 8`. Sản phẩm của họ là`6720`, có số dư`6`modulo`9`, vậy câu trả lời là`6`. Thuật toán so sánh`b`với`a + 5 = 9`, thấy thế`8 < 9`và xử lý rõ ràng tất cả năm yếu tố. 

Khi khoảng chứa đúng sáu số thì phải áp dụng phím tắt. Đối với đầu vào`4`Và`9`, các yếu tố bao gồm cả`6`Và`9`. Sản phẩm của họ đã đóng góp hai yếu tố`3`, do đó sản phẩm hoàn chỉnh có thể chia hết cho`9`. Thuật toán tính toán`a + 5 = 9`, quan sát`b >= 9`, và ngay lập tức quay trở lại`9`. 

Điểm cuối có thể có 100.000 chữ số, vì vậy hãy chuyển đổi chúng bằng`int()`không phải là một phần của giải pháp. Đối với điểm cuối bao gồm 100.000 số chín, thuật toán sẽ quét các chữ số của nó để thu được modulo`9`dư lượng và chỉ sử dụng phép cộng chuỗi thập phân khi so sánh các giá trị gần đó. Công việc vẫn tuyến tính về số lượng chữ số. 

Vụ án`b - a = 5`đặc biệt hữu ích để bắt từng lỗi một. Sáu số có nghĩa là năm đơn vị chênh lệch nên sản phẩm đảm bảo chia hết cho`9`. Một triển khai kiểm tra`b - a > 5`thay vì`b - a >= 5`sẽ cố gắng xử lý trường hợp này một cách không chính xác như một khoảng thời gian ngắn. Ví dụ,`4`Và`9`phải sản xuất`9`.
