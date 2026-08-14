---
title: "CF 102307F - Công thức phân số"
description: "Mỗi dòng đầu vào là một biểu thức số học được xây dựng từ các phân số, phép cộng, phép trừ và dấu ngoặc đơn. Một phân số xuất hiện trong biểu thức có tử số từ 0 đến 100 và mẫu số dương từ 1 đến 20."
date: "2026-08-13T23:37:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102307
codeforces_index: "F"
codeforces_contest_name: "2019 ICPC Universidad Nacional de Colombia Programming Contest"
rating: 0
weight: 102307
solve_time_s: 187
verified: true
draft: false
---

[CF 102307F - Công thức phân số](https://codeforces.com/problemset/problem/102307/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 7s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi dòng đầu vào là một biểu thức số học được xây dựng từ các phân số, phép cộng, phép trừ và dấu ngoặc đơn. Một phân số xuất hiện trong biểu thức có tử số từ 0 đến 100 và mẫu số dương từ 1 đến 20. Dấu ngoặc đơn có thể được lồng vào nhau tùy ý, do đó, một biểu thức như`1/2-(3/4-(1/5+1/10))`là hợp lệ. 

Đối với mọi biểu thức, chúng ta cần giá trị hữu tỉ chính xác của nó và phải in giá trị đó dưới dạng phân số rút gọn có mẫu số dương. Bản thân kết quả có thể âm, mặc dù mọi phân số xuất hiện trong đầu vào đều có tử số không âm. 

Tổng số ký tự trên tất cả các dòng đầu vào tối đa là`2 * 10^5`. Điều đó ràng buộc loại trừ các thuật toán quét lại một biểu thức nhiều lần, vì thuật toán bậc hai có thể kiểm tra một cách đại khái`2 * 10^10`ký tự trong trường hợp xấu nhất. Quét tuyến tính là mục tiêu tự nhiên. Các mẫu số riêng lẻ nhiều nhất là 20, điều này mang lại cho chúng ta một thuộc tính mạnh hơn nhiều so với việc chỉ có các mã thông báo đầu vào nhỏ: mỗi mẫu số chia cho một mẫu số chung nhỏ cố định, bội số chung nhỏ nhất của`1, 2, ..., 20`. 

Một số trường hợp nguy hiểm có thể phá vỡ việc triển khai bất cẩn. Vì`1/5-2/10`, kết quả đúng là`0/1`. Giữ mẫu số không giảm sẽ tạo ra một cái gì đó như`0/10`, không thỏa mãn biểu diễn tối giản cần thiết. 

Vì`1/2-(1/2-2/1)`, kết quả đúng là`2/1`. Trình phân tích cú pháp xử lý dấu ngoặc đơn như cách nhóm thông thường mà không nhớ dấu hiệu đứng trước dấu ngoặc đơn mở có thể đánh giá biểu thức không chính xác như`-2/1`hoặc`-1/1`. 

Vì`1/2-3/2`, kết quả là`-1/1`. Việc triển khai giả định tử số cuối cùng phải không âm vì tất cả các phân số đầu vào đều không âm sẽ thất bại ở đây. Hạn chế áp dụng cho các phân số đầu vào, không áp dụng cho giá trị của biểu thức. 

Vì`100/20`, kết quả là`5/1`. Giới hạn mẫu số là bao gồm, do đó trình phân tích cú pháp phải chấp nhận 20 thay vì vô tình coi 19 là mẫu số lớn nhất có thể. 

## Phương pháp tiếp cận 

Một giải pháp brute-force đơn giản có thể liên tục xác định vị trí một biểu thức được đặt trong ngoặc đơn trong cùng, đánh giá nó, thay thế toàn bộ biểu thức bằng phân số kết quả của nó và tiếp tục cho đến khi không còn dấu ngoặc đơn nào. Mỗi phép thay thế đều đúng về mặt toán học vì biểu thức bên trong dấu ngoặc đơn được chọn độc lập với biểu thức xung quanh. 

Vấn đề là việc quét lặp đi lặp lại. Hãy xem xét một biểu thức chứa một chuỗi dài các dấu ngoặc đơn lồng nhau. Tìm kiếm đầu tiên quét gần như toàn bộ chuỗi, tìm kiếm tiếp theo quét gần như toàn bộ chuỗi rút ngắn, v.v. Với`n`nhân vật này cần`Theta(n^2)`kiểm tra nhân vật. Tại`n = 2 * 10^5`, một chuỗi có thể dẫn đến khoảng`n^2 / 2`, xung quanh`2 * 10^10`, vượt xa những gì giải pháp một giây có thể thực hiện được. 

Trình phân tích cú pháp đệ quy đã tốt hơn nhiều vì nó có thể đánh giá mọi phần của biểu thức một lần. Tuy nhiên, đệ quy Python ở đây không an toàn vì một biểu thức hợp lệ có thể chứa gần`2 * 10^5`dấu ngoặc đơn lồng nhau. Chúng ta có thể tránh đệ quy hoàn toàn bằng cách giữ trạng thái đánh giá rõ ràng trên một ngăn xếp. 

Ngoài ra còn có một sự đơn giản hóa số học hữu ích. Mỗi mẫu số đầu vào nằm trong khoảng từ 1 đến 20, vì vậy mọi mẫu số đều chia hết`L = lcm(1, 2, ..., 20) = 232792560`. 

Thay vì lưu trữ từng phân số dưới dạng tử số và mẫu số riêng biệt, hãy chuyển đổi từng phân số đầu vào`a/b`vào số nguyên`a * (L / b)`. 

Số nguyên này đại diện cho phân số ban đầu nhân với`L`. Vì phép cộng và phép trừ là tuyến tính nên toàn bộ biểu thức có thể được đánh giá chỉ bằng phép cộng và phép trừ số nguyên. Dấu ngoặc đơn chỉ ảnh hưởng đến số nguyên tích lũy nào nhận được dấu, do đó chúng có thể được xử lý bởi ngăn xếp. 

Sau khi biểu thức hoàn chỉnh đã được đánh giá, giả sử kết quả được chia tỷ lệ là`x`. Giá trị thực tế là`x/L`. Chia`x`Và`L`bằng ước số chung lớn nhất của chúng sẽ thu được phân số tối giản cần tìm. 

Phương pháp brute-force liên tục truy cập lại các ký tự giống nhau, trong khi phương pháp tối ưu xử lý từng ký tự một lần và trì hoãn tất cả việc giảm phân số cho đến hết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đặt`L = 232792560`, bội số chung nhỏ nhất của mọi mẫu số đầu vào có thể có. Mỗi phân số`a/b`có thể được biểu diễn dưới dạng số nguyên`a * (L/b)`, vì giá trị của nó chính xác là số nguyên chia cho`L`. 
2. Quét biểu thức từ trái sang phải. Duy trì`value`, giá trị biểu thức hiện tại tính bằng đơn vị`1/L`, Và`sign`, đó là`1`hoặc`-1`và cho chúng ta biết phân số tiếp theo sẽ được cộng như thế nào. 
3. Khi một phân số bắt đầu, hãy đọc trực tiếp tử số và mẫu số của nó từ chuỗi. Chuyển đổi nó thành mẫu số chung bằng cách sử dụng`numerator * (L // denominator)`, sau đó thêm`sign * scaled_fraction`ĐẾN`value`. Chúng ta không bao giờ cần số học dấu phẩy động nên không có lỗi làm tròn. 
4. Khi một`+`gặp phải, thiết lập`sign`ĐẾN`1`. Khi một`-`gặp phải, thiết lập`sign`ĐẾN`-1`. Dấu thuộc về số hạng tiếp theo, có thể là phân số hoặc toàn bộ biểu thức được đặt trong ngoặc đơn. 
5. Khi nào`(`gặp phải, đẩy dòng điện`value`Và`sign`lên một ngăn xếp. Sau đó bắt đầu một biểu thức mới bên trong dấu ngoặc đơn với`value = 0`Và`sign = 1`. Dấu hiệu đã lưu là cần thiết vì toàn bộ biểu thức bên trong có thể được đặt trước bởi`+`hoặc`-`. 
6. Khi nào`)`gặp phải thì biểu hiện bên trong đã xong. Bật giá trị bên ngoài đã lưu và dấu hiệu bên ngoài. Nếu kết quả bên trong là`inner`, thay thế trạng thái bên ngoài bằng`outer_value + outer_sign * inner`. Điều này coi toàn bộ biểu thức trong ngoặc đơn là một thuật ngữ, chính xác như ngữ pháp yêu cầu. 
7. Sau khi quét xong, biểu thức có giá trị`value / L`. Tính toán`g = gcd(abs(value), L)`và in`value // g`qua`L // g`. Việc sử dụng giá trị tuyệt đối trong gcd cũng xử lý chính xác các kết quả âm tính. 

### Tại sao nó hoạt động 

Điều bất biến là tại mọi điểm trong quá trình quét,`value / L`chính xác là giá trị của phần biểu thức hiện tại đã được sử dụng. các`sign`biến đại diện cho toán tử đang chờ được áp dụng cho thuật ngữ tiếp theo. Mở dấu ngoặc đơn sẽ lưu trạng thái bên ngoài hoàn chỉnh và bắt đầu một biểu thức bên trong độc lập. Việc đóng nó sẽ khôi phục trạng thái đó và thêm biểu thức bên trong được đánh giá với chính xác dấu đứng trước dấu ngoặc đơn mở. Vì mọi phân số đều được quy về cùng mẫu số`L`, tất cả các phép cộng và phép trừ đều giữ nguyên giá trị chính xác được biểu thị bằng`value / L`. Cuối cùng, giảm`value/L`bởi gcd của họ chỉ thay đổi cách biểu diễn chứ không phải giá trị toán học, vì vậy phân số được in ra chính xác là câu trả lời được yêu cầu. 

## Giải pháp Python```python
import sys
from math import gcd

input = sys.stdin.readline

L = 232792560

def evaluate(s):
    n = len(s)
    i = 0

    value = 0
    sign = 1

    # Each stack entry stores:
    # (value before '(', sign that preceded '(')
    stack = []

    while i < n:
        c = s[i]

        if c.isdigit():
            numerator = 0
            while i < n and s[i].isdigit():
                numerator = numerator * 10 + (ord(s[i]) - ord('0'))
                i += 1

            # Skip '/'
            i += 1

            denominator = 0
            while i < n and s[i].isdigit():
                denominator = denominator * 10 + (ord(s[i]) - ord('0'))
                i += 1

            scaled = numerator * (L // denominator)
            value += sign * scaled

        elif c == '+':
            sign = 1
            i += 1

        elif c == '-':
            sign = -1
            i += 1

        elif c == '(':
            stack.append((value, sign))
            value = 0
            sign = 1
            i += 1

        else:  # ')'
            outer_value, outer_sign = stack.pop()
            value = outer_value + outer_sign * value
            i += 1

    g = gcd(abs(value), L)
    return f"{value // g}/{L // g}"

def main():
    out = []

    for line in sys.stdin:
        s = line.strip()
        if s:
            out.append(evaluate(s))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    main()
```Hằng số`L`là tối ưu hóa số học trung tâm. Vì mỗi mẫu số lớn nhất là 20 nên nó chia bội số chung nhỏ nhất của`1`bởi vì`20`. Ví dụ,`1/6`trở thành`L/6`đơn vị, trong khi`3/10`trở thành`3L/10`đơn vị. Cả hai đều là số nguyên sử dụng chính xác cùng mẫu số`L`. 

Trình phân tích cú pháp đọc tử số đầy đủ trước khi bỏ qua`/`, sau đó đọc mẫu số. Không có dấu trừ đơn nhất trong ngữ pháp đầu vào, vì vậy dấu trừ luôn đóng vai trò là toán tử giữa hai công thức hợp lệ. 

Ngăn xếp lưu trữ hai giá trị ở mỗi dấu ngoặc đơn mở. Đầu tiên là mọi thứ được tính toán trước dấu ngoặc đơn và thứ hai là dấu đang chờ biểu thức trong ngoặc đơn. Đặt lại`value`về 0 mang lại cho biểu thức bên trong bộ tích lũy riêng của nó. 

Khi dấu ngoặc đơn đóng được xử lý, biểu thức bên trong nó đã được đánh giá đầy đủ. dòng`value = outer_value + outer_sign * value`chính xác là phép toán được biểu diễn bởi bối cảnh xung quanh. Sau thao tác đó, trình phân tích cú pháp có thể tiếp tục bình thường. 

Số nguyên Python có độ chính xác tùy ý, do đó không có rủi ro tràn. Quan trọng hơn, mẫu số chung là cố định nên tử số tích lũy vẫn ở mức khiêm tốn. Có nhiều nhất`O(n)`các phân số, mỗi phân số đóng góp nhiều nhất`100L`, cho tử số có độ lớn lớn nhất`O(nL)`, chứ không phải là một sản phẩm khổng lồ của tất cả các mẫu số đầu vào. 

Việc sử dụng gcd cuối cùng`abs(value)`bởi vì`value`có thể tiêu cực. Nếu như`value`bằng không,`gcd(0, L)`là`L`, do đó kết quả tự động trở thành`0/1`. 

## Ví dụ đã hoạt động 

Hãy xem xét biểu thức mẫu đầu tiên,`1/2+1/3`. Mẫu số chung là`L`, vậy hai phân số đóng góp`L/2`Và`L/3`. 

| Vị trí | Ký tự hoặc mã thông báo | giá trị | ký tên | Ngăn xếp | 
| --- | --- | --- | --- | --- | 
| 0 |`1/2`| 116396280 | 1 | trống | 
| 3 |`+`| 116396280 | 1 | trống | 
| 4 |`1/3`| 193993800 | 1 | trống | 
| kết thúc | giảm |`5/6`| | trống | 

Giá trị tỷ lệ tích lũy là`L/2 + L/3 = 5L/6`. Từ`gcd(5L/6, L) = L/6`, biểu diễn cuối cùng là`5/6`. 

Bây giờ hãy xem xét mẫu thứ ba,`1/2+(1/2-2/1)`. Ví dụ này thực hiện cả biểu thức con và phép trừ trong ngoặc đơn bên trong nó. 

| Vị trí | Ký tự hoặc mã thông báo | giá trị | ký tên | Ngăn xếp | 
| --- | --- | --- | --- | --- | 
| 0 |`1/2`| 116396280 | 1 | trống | 
| 3 |`+`| 116396280 | 1 | trống | 
| 4 |`(`| 0 | 1 |`(116396280, 1)`| 
| 5 |`1/2`| 116396280 | 1 |`(116396280, 1)`| 
| 8 |`-`| 116396280 | -1 |`(116396280, 1)`| 
| 9 |`2/1`| -116396280 | -1 |`(116396280, 1)`| 
| 12 |`)`| 0 | 1 | trống | 
| kết thúc | giảm |`-1/1`| | trống | 

Bên trong dấu ngoặc đơn,`1/2 - 2/1 = -3/2`. Biểu thức bên ngoài là`1/2 + (-3/2) = -1`, cung cấp yêu cầu`-1/1`. Ngăn xếp là thứ bảo tồn phần bên ngoài`+`trong khi biểu thức bên trong được đánh giá độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được quét một lần và gcd cuối cùng sử dụng các số nguyên có kích thước được giới hạn bởi tổng số phân số nhân với hằng số cố định`L`. | 
| Không gian | O(n) | Ngăn xếp dấu ngoặc đơn có thể chứa một mục nhập cho mỗi cấp độ lồng nhau. | 

Đây`n`là tổng chiều dài của tất cả các biểu thức đầu vào. Vì tổng số nhiều nhất là`2 * 10^5`, một lần truyền tuyến tính duy nhất trên tất cả đầu vào nằm trong giới hạn dự kiến. Ngăn xếp cũng sử dụng nhiều nhất bộ nhớ tuyến tính, dưới 256 MB. 

## Trường hợp thử nghiệm```python
import sys
import io
from math import gcd

L = 232792560

def evaluate(s):
    n = len(s)
    i = 0
    value = 0
    sign = 1
    stack = []

    while i < n:
        c = s[i]

        if c.isdigit():
            numerator = 0
            while i < n and s[i].isdigit():
                numerator = numerator * 10 + ord(s[i]) - ord('0')
                i += 1

            i += 1  # '/'

            denominator = 0
            while i < n and s[i].isdigit():
                denominator = denominator * 10 + ord(s[i]) - ord('0')
                i += 1

            value += sign * numerator * (L // denominator)

        elif c == '+':
            sign = 1
            i += 1

        elif c == '-':
            sign = -1
            i += 1

        elif c == '(':
            stack.append((value, sign))
            value = 0
            sign = 1
            i += 1

        else:
            outer_value, outer_sign = stack.pop()
            value = outer_value + outer_sign * value
            i += 1

    g = gcd(abs(value), L)
    return f"{value // g}/{L // g}"

def run(inp: str) -> str:
    old_stdin = sys.stdin
    try:
        sys.stdin = io.StringIO(inp)
        return "\n".join(
            evaluate(line.strip())
            for line in sys.stdin
            if line.strip()
        )
    finally:
        sys.stdin = old_stdin

# Provided samples
assert run("1/2+1/3\n") == "5/6", "sample 1"
assert run("1/5-2/10\n") == "0/1", "sample 2"
assert run("1/2+(1/2-2/1)\n") == "-1/1", "sample 3"

# Minimum-size input
assert run("0/1\n") == "0/1", "minimum fraction"

# Maximum numerator and denominator boundary
assert run("100/20\n") == "5/1", "maximum numerator and denominator"

# All equal values
assert run("1/2+1/2+1/2+1/2\n") == "2/1", "equal fractions"

# Deep nesting and subtraction through parentheses
assert run("1/1-(1/1-(1/1-(1/1)))\n") == "1/1", "nested parentheses"

# Negative result
assert run("1/20-100/1\n") == "-1999/20", "negative result"

# Force the expression length close to the maximum allowed.
terms = ["100/20"] * 28571
large_input = "+".join(terms)
assert len(large_input) <= 200000
assert run(large_input) == f"{5 * len(terms)}/1", "large input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0/1`|`0/1`| Tử số 0 và mẫu số tối thiểu | 
|`100/20`|`5/1`| Bao gồm các ranh giới tử số và mẫu số cộng với rút gọn | 
|`1/2+1/2+1/2+1/2`|`2/1`| Phân số bằng nhau lặp đi lặp lại và rút gọn cuối cùng | 
|`1/1-(1/1-(1/1-(1/1)))`|`1/1`| Dấu ngoặc đơn lồng nhau và truyền dấu | 
|`1/20-100/1`|`-1999/20`| Kết quả cuối cùng âm tính và mẫu số nhỏ | 
| 28571 bản sao của`100/20`tham gia bởi`+`|`142855/1`| Độ dài đầu vào gần tối đa và quét tuyến tính | 

## Vỏ cạnh 

biểu thức`1/5-2/10`kiểm tra giảm về không. Cả hai phân số đều trở thành số nguyên có mẫu số`L`và các giá trị tỷ lệ của chúng bằng nhau, do đó bộ tích lũy trở thành 0. gcd cuối cùng là`gcd(0, L) = L`, sản xuất`0/1`chứ không phải là không giảm`0/L`. 

biểu thức`1/2-(1/2-2/1)`kiểm tra một dấu được gắn vào toàn bộ biểu thức trong ngoặc đơn. Trước khi nhập dấu ngoặc đơn, ngăn xếp lưu giá trị bên ngoài`L/2`và ký tên`-1`. Biểu thức bên trong đánh giá thành`L/2 - 2L = -3L/2`. Đóng dấu ngoặc đơn sẽ khôi phục trạng thái bên ngoài và tính toán`L/2 - (-3L/2) = 2L`, làm giảm đến`2/1`. Cơ chế tương tự hoạt động với bất kỳ độ sâu lồng nhau nào. 

biểu thức`1/2-3/2`sản xuất`-1/1`. Bộ tích lũy đơn giản trở thành`L/2 - 3L/2 = -L`và việc rút gọn gcd giữ nguyên dấu âm ở tử số. Không cần có trường hợp đặc biệt nào cho biểu thức phủ định. 

biểu thức`100/20`kiểm tra trực tiếp các giá trị tối đa được phép. Từ`20`chia rẽ`L`, phần tỷ lệ là`100 * (L/20) = 5L`, và mức giảm cuối cùng mang lại`5/1`. Sự chuyển đổi`L // denominator`là chính xác vì mọi mẫu số cho phép đều là ước số của`L`. 

Một biểu thức lồng nhau sâu sắc như`1/1-(1/1-(1/1-(1/1)))`cũng giải thích tại sao ngăn xếp rõ ràng lại được ưu tiên hơn so với phân tích cú pháp đệ quy trong Python. Mỗi dấu ngoặc đơn mở thêm một mục nhập ngăn xếp có kích thước không đổi và mỗi dấu ngoặc đơn đóng sẽ loại bỏ mục nhập đó. Trình phân tích cú pháp không bao giờ phụ thuộc vào ngăn xếp cuộc gọi Python, do đó, ngay cả một biểu thức hợp lệ có lồng rất sâu vẫn an toàn. 

Cuối cùng, một biểu thức rất dài được tạo từ nhiều phân số sẽ kiểm tra giới hạn đầu vào thực tế. Trình phân tích cú pháp không liên tục xây dựng lại hoặc quét lại biểu thức, do đó công việc của nó tăng lên trực tiếp theo số lượng ký tự đầu vào. Mẫu số chung cố định cũng ngăn không cho mẫu số phân số tăng lên trong quá trình tính toán, giữ cho phép tính số học hiệu quả ngay cả ở gần`2 * 10^5`giới hạn ký tự.
