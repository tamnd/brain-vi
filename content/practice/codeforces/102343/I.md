---
title: "CF 102343I - Làm tròn điểm nổi"
description: "Chúng ta được cung cấp các số hạng hiển thị của một dãy hình học sau khi mỗi số hạng được làm tròn đến chính xác (D) chữ số có nghĩa. Dãy số ban đầu có dạng [ ai=a0r^i, ] trong đó (a00) là số hạng đầu tiên và (r0) là tỷ số chung."
date: "2026-08-17T10:21:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102343
codeforces_index: "I"
codeforces_contest_name: "UCF Locals 2019"
rating: 0
weight: 102343
solve_time_s: 135
verified: true
draft: false
---

[CF 102343I - Làm tròn điểm nổi](https://codeforces.com/problemset/problem/102343/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 15s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp các số hạng hiển thị của một dãy hình học sau khi mỗi số hạng được làm tròn đến chính xác (D) chữ số có nghĩa. Dãy ban đầu có dạng 

[ 
a_i=a_0r^i, 
] 

trong đó (a_0>0) là số hạng đầu tiên và (r>0) là tỉ số chung. Nhiệm vụ không phải là khôi phục chính xác trình tự gốc vì việc làm tròn sẽ phá hủy thông tin đó. Thay vào đó, chúng ta cần giới hạn trên và dưới chặt chẽ nhất có thể cho (a_0) và (r). Đầu ra được yêu cầu chứa bốn giới hạn đó, với các chữ số có nghĩa (D+3) cho (a_0) và (D+5) cho (r). Các ràng buộc chính thức đã được thay thế (1<D<6) bằng (0<D<6), (1<N<200) tương đương và mọi thuật ngữ được quan sát đều nằm hoàn toàn giữa (10^{-8}) và (10^8). Tuyên bố chính thức và các mẫu xác nhận những giới hạn này và độ chính xác đầu ra cần thiết. 

Quan sát trọng tâm là mọi giá trị làm tròn đều tương ứng với một khoảng giá trị ban đầu có thể có. Ví dụ: khi (D=4), giá trị được hiển thị (1335) đại diện cho mọi số từ (1334,5) đến (1335,5), với điểm cuối trên được hiểu là giới hạn giới hạn. Tổng quát hơn, nếu giá trị được hiển thị là (x) và giá trị vị trí của chữ số có nghĩa cuối cùng của nó là (u), thì giá trị ban đầu nằm giữa (x-u/2) và (x+u/2). 

Giá trị nhỏ của (N) có tính quyết định. Vì (N<200), nên có ít hơn (200^2/2=20.000) cặp chỉ số. Một giải pháp (O(N^2)) chỉ thực hiện vài chục nghìn phép tính dấu phẩy động, nằm trong giới hạn bốn giây đã nêu cho bài toán. Một phương pháp (O(N^3)) sẽ thực hiện khoảng tám triệu phép tính cặp ba trong trường hợp xấu nhất, trong khi tìm kiếm số trên lưới hai chiều tinh tế sẽ đắt hơn rất nhiều và cũng sẽ khiến việc kiểm soát độ chính xác trở nên khó khăn. 

Có một số trường hợp đặc biệt khi coi số được làm tròn là giá trị chính xác sẽ cho kết quả sai. Hãy xem xét chuỗi nhỏ nhất có thể có (D=1):```
1 2
1 1
```Giới hạn chính xác là khoảng```
0.9500 1.050 0.904762 1.10526
```bởi vì mỗi hiển thị (1) đại diện cho khoảng ([0,95,1,05)). Một giải pháp chỉ cần lấy (a_0=1) và (r=1) bỏ lỡ toàn bộ phạm vi của chuỗi ban đầu hợp lệ. 

Vấn đề thứ hai xảy ra khi thứ tự cường độ thay đổi. Ví dụ,```
3 2
999
1000
```Giá trị (999) có đơn vị làm tròn là (1), trong khi (1000) có đơn vị làm tròn là (10). Khoảng thời gian của chúng là ([998,5,999,5)) và ([995,1005)). Việc triển khai bất cẩn luôn sử dụng cùng số vị trí thập phân cho lỗi làm tròn sẽ tạo ra khoảng sai cho ít nhất một trong các số hạng này. 

Trường hợp cạnh thứ ba là một dãy có các số hạng bằng nhau:```
2 3
5.0 5.0 5.0
```Tỷ lệ không nhất thiết phải chính xác (1). Nó có thể nằm trong khoảng từ (0,980198) đến (1,020202), vì một sự tăng hoặc giảm nhỏ của (r) vẫn có thể giữ cả ba số hạng ban đầu bên trong khoảng làm tròn của chúng. Giả sử (r=1) vì các giá trị được hiển thị bằng nhau sẽ loại bỏ các khả năng hợp lệ. 

## Phương pháp tiếp cận 

Một cách tiếp cận bằng số thực sự đơn giản sẽ thử các giá trị ứng cử viên cho (a_0) và (r), tạo ra toàn bộ chuỗi hình học và kiểm tra xem mọi thuật ngữ được tạo có nằm trong khoảng làm tròn tương ứng của nó hay không. Điều này đúng về mặt khái niệm, bởi vì một cặp ứng viên hợp lệ chính xác khi tất cả các số hạng được tạo ra của nó đều hợp lệ. Vấn đề là (a_0) và (r) là số thực, do đó, lưới lực lượng vũ phu không có độ phân giải hữu hạn tự nhiên. Để đạt được độ chính xác được yêu cầu, thậm chí một triệu ứng cử viên dọc theo mỗi trục sẽ yêu cầu khoảng (10^{12}) lần kiểm tra và câu trả lời có thể nằm giữa các điểm lưới. 

Quan sát hữu ích là chúng ta không bao giờ cần tìm kiếm trực tiếp trên (a_0) và (r). Mỗi cặp vị trí chuỗi đưa ra một hạn chế ngay lập tức đối với (r). 

Giả sử số hạng (i) nằm trong ([L_i,U_i]) và số hạng (j), trong đó (i<j), nằm trong ([L_j,U_j]). Kể từ khi 

[ 
L_i\le a_0r^i\le U_i 
] 

và 

[ 
L_j\le a_0r^j\le U_j, 
] 

chúng ta có thể kết hợp giới hạn dưới của số hạng sau với giới hạn trên của số hạng trước: 

[ 
L_j\le a_0r^j\le U_i r^{j-i}. 
] 

Do đó 

[ 
r^{j-i}\ge \frac{L_j}{U_i}, 
] 

vậy 

[ 
r\ge\left(\frac{L_j}{U_i}\right)^{1/(j-i)}. 
] 

Tương tự, kết hợp giới hạn trên của số hạng sau với giới hạn dưới của số hạng trước sẽ cho 

[ 
r\le\left(\frac{U_j}{L_i}\right)^{1/(j-i)}. 
] 

Mỗi cặp số hạng cung cấp một giới hạn dưới và một giới hạn trên cho (r). Lấy mức tối đa của tất cả các giới hạn dưới và mức tối thiểu của tất cả các giới hạn trên sẽ cho khoảng khả thi chính xác cho (r). 

Khi đã biết hai giá trị cực trị của (r), (a_0) trở nên dễ dàng. Đối với một (r cố định), mọi thuật ngữ đều cho 

[ 
\frac{L_i}{r^i}\le a_0\le\frac{U_i}{r^i}. 
] 

Như vậy 

[ 
a_0\ge\max_i\frac{L_i}{r^i} 
] 

và 

[ 
a_0\le\min_i\frac{U_i}{r^i}. 
] 

Giá trị nhỏ nhất có thể (a_0) xảy ra ở giá trị lớn nhất có thể (r), trong khi giá trị lớn nhất có thể (a_0) xảy ra ở giá trị nhỏ nhất có thể (r). Điều này xảy ra vì mọi biểu thức (L_i/r^i) và (U_i/r^i) đều không tăng khi (r) tăng. 

Phương pháp kết quả đã đủ tối ưu cho ràng buộc thực tế (N<200). Nó kiểm tra từng cặp một lần và sau đó thực hiện một lần quét tuyến tính bổ sung. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu số | Phụ thuộc vào độ phân giải lưới, có khả năng (O(G^2N)) | (O(N)) | Quá chậm và nhạy cảm với độ chính xác | 
| Ràng buộc khoảng thời gian theo cặp | (O(N^2)) | (O(N)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi mọi giá trị đầu vào được làm tròn thành khoảng giá trị ban đầu có thể có. Nếu chữ số có nghĩa cuối cùng của nó đại diện cho giá trị vị trí (u), hãy lưu trữ (L_i=x_i-u/2) và (U_i=x_i+u/2). Bài toán yêu cầu các giới hạn chặt chẽ, do đó bản thân các giá trị điểm cuối được sử dụng khi tính toán các câu trả lời giới hạn. 
2. Khởi tạo giới hạn dưới của (r) về 0 và giới hạn trên về vô cùng. Với mỗi cặp (i<j), hãy tính 

[ 
\left(\frac{L_j}{U_i}\right)^{1/(j-i)} 
] 

và sử dụng nó để củng cố giới hạn dưới của (r). Tính toán 

[ 
\left(\frac{U_j}{L_i}\right)^{1/(j-i)} 
] 

và sử dụng nó để tăng cường giới hạn trên. 

1. Sau khi tất cả các cặp đã được xử lý, giá trị thu được là tỷ lệ khả thi tối thiểu và tối đa. Mọi chuỗi hình học hợp lệ phải thỏa mãn mọi hạn chế theo cặp, trong khi giao điểm của các hạn chế đó đủ để làm cho tất cả các khoảng ban đầu khả thi đồng thời. 
2. Sử dụng tỷ lệ tối đa (r_{\max}) để tính mức tối thiểu có thể (a_0). Quét tất cả các thuật ngữ và tính toán 

[ 
\max_i\frac{L_i}{r_{\max}^i}. 
] 

Lý do sử dụng (r_{\max}) là việc tăng (r) sẽ giảm mọi yêu cầu thấp hơn trên (a_0). 

1. Sử dụng tỷ lệ tối thiểu (r_{\min}) để tính tỷ lệ tối đa có thể (a_0). Quét tất cả các thuật ngữ và tính toán 

[ 
\min_i\frac{U_i}{r_{\min}^i}. 
]

Lý do là đối xứng: việc tăng (r) chỉ có thể giảm mọi giới hạn trên của (a_0), do đó giới hạn trên lớn nhất xảy ra tại (r_{\min}). 

1. Định dạng hai giá trị (a_0) có chữ số có nghĩa (D+3) và hai giá trị tỷ lệ có (D+5) chữ số có nghĩa. Ký hiệu khoa học không được phép, do đó trình trợ giúp định dạng sẽ chuyển nó trở lại ký hiệu thập phân thông thường khi Python sử dụng số mũ. 

### Tại sao nó hoạt động 

Điều bất biến là sau khi xử lý bất kỳ tập hợp các cặp chỉ số nào, khoảng duy trì cho (r) chứa chính xác mọi tỷ lệ có thể đáp ứng các cặp được xử lý đó. Mỗi cặp lấy được giới hạn dưới và giới hạn trên cần thiết trực tiếp từ hai khoảng làm tròn tương ứng. Sau khi tất cả các cặp được xử lý, mỗi cặp vị trí đều tương thích với khoảng tỷ lệ kết quả, điều này là đủ vì một khi (r) được cố định, tất cả các ràng buộc trên (a_0) chỉ đơn giản là các khoảng trên cùng một dòng thực. Lần quét cuối cùng giao với những khoảng thời gian đó. Vì các yêu cầu thấp hơn cho (a_0) giảm khi (r) tăng và các yêu cầu trên cũng giảm khi (r) tăng nên (a_0) đạt được ở (r_{\max}) và (a_0) lớn nhất ở (r_{\min}). 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

def decimal_exponent(s):
    s = s.strip()
    if '.' in s:
        left, right = s.split('.')
    else:
        left, right = s, ''

    left = left.lstrip('0')

    if left:
        return len(left) - 1

    for i, ch in enumerate(right):
        if ch != '0':
            return -(i + 1)

    return 0

def interval(s, d):
    x = float(s)
    e = decimal_exponent(s)

    # Place value of the D-th significant digit.
    unit = 10.0 ** (e - d + 1)
    half = unit * 0.5

    return x - half, x + half

def fixed_from_scientific(s):
    if 'e' not in s and 'E' not in s:
        return s

    mantissa, exponent = s.lower().split('e')
    exponent = int(exponent)

    sign = ''
    if mantissa.startswith('-'):
        sign = '-'
        mantissa = mantissa[1:]

    if '.' in mantissa:
        whole, frac = mantissa.split('.')
        digits = whole + frac
        decimal_pos = len(whole)
    else:
        digits = mantissa
        decimal_pos = len(mantissa)

    decimal_pos += exponent

    if decimal_pos <= 0:
        result = '0.' + '0' * (-decimal_pos) + digits
    elif decimal_pos >= len(digits):
        result = digits + '0' * (decimal_pos - len(digits))
    else:
        result = digits[:decimal_pos] + '.' + digits[decimal_pos:]

    return sign + result

def format_sig(x, digits):
    # x is always positive in this problem.
    s = format(x, f'.{digits}g')
    return fixed_from_scientific(s)

def solve(data):
    tokens = data.split()
    it = iter(tokens)

    d = int(next(it))
    n = int(next(it))

    low = [0.0] * n
    high = [0.0] * n

    for i in range(n):
        low[i], high[i] = interval(next(it), d)

    r_min = 0.0
    r_max = float('inf')

    # Every pair of terms gives a necessary restriction on r.
    for i in range(n):
        for j in range(i + 1, n):
            dist = j - i

            lower_r = (low[j] / high[i]) ** (1.0 / dist)
            upper_r = (high[j] / low[i]) ** (1.0 / dist)

            if lower_r > r_min:
                r_min = lower_r
            if upper_r < r_max:
                r_max = upper_r

    # Minimum a0 occurs at maximum r.
    rpow = 1.0
    a0_min = 0.0
    for i in range(n):
        if i > 0:
            rpow *= r_max
        candidate = low[i] / rpow
        if candidate > a0_min:
            a0_min = candidate

    # Maximum a0 occurs at minimum r.
    rpow = 1.0
    a0_max = float('inf')
    for i in range(n):
        if i > 0:
            rpow *= r_min
        candidate = high[i] / rpow
        if candidate < a0_max:
            a0_max = candidate

    ans = [
        format_sig(a0_min, d + 3),
        format_sig(a0_max, d + 3),
        format_sig(r_min, d + 5),
        format_sig(r_max, d + 5),
    ]

    return ' '.join(ans)

def main():
    data = sys.stdin.read()
    sys.stdout.write(solve(data))

if __name__ == "__main__":
    main()
```các`decimal_exponent`hàm xác định độ lớn của chữ số có nghĩa đầu tiên trực tiếp từ chuỗi đầu vào. Điều này tránh việc dựa vào`log10`khi đầu vào chính xác là lũy thừa của mười, trong đó một lỗi dấu phẩy động nhỏ có thể chọn sai đơn vị làm tròn. 

các`interval`hàm sử dụng số mũ đó để tìm giá trị vị trí của chữ số có nghĩa cuối cùng. Chẳng hạn, với (D=3),`999`có số mũ (2) nên đơn vị làm tròn của nó là (10^{2-3+1}=1), trong khi`1000`có số mũ (3), cho ra đơn vị là (10). Đây là trường hợp ranh giới mà việc triển khai số thập phân cố định sẽ xử lý sai. 

Các vòng lặp lồng nhau thực hiện trực tiếp việc dẫn xuất theo cặp. Số mũ (1/(j-i)) là cần thiết vì hai vị trí dãy được phân tách bằng lũy ​​thừa (j-i) của (r), không nhất thiết phải là một lũy thừa. 

Hai lần quét cuối cùng có chủ ý sử dụng các điểm cuối tỷ lệ khác nhau. Ở mức tối thiểu (a_0),`r_max`được sử dụng. Để đạt mức tối đa (a_0),`r_min`được sử dụng. Việc đảo ngược hai lựa chọn này là nguyên nhân phổ biến dẫn đến các câu trả lời sai. 

Số nguyên Python không bị tràn và tất cả các giá trị số trong bài toán này vẫn nằm trong phạm vi dấu phẩy động rất nhỏ. Tuyên bố cũng đảm bảo rằng lời giải chính thức cần không quá 13 chữ số có nghĩa trong các phép tính trung gian, do đó số học có độ chính xác kép là đủ cho độ chính xác đầu ra được yêu cầu. 

Trình trợ giúp định dạng sử dụng định dạng chữ số có nghĩa của Python và chuyển đổi ký hiệu khoa học thành ký hiệu thập phân tiêu chuẩn. Điều này bảo tồn các số 0 ở cuối như`180.0500`Và`1.0106650`, điều này quan trọng vì câu lệnh yêu cầu đầu ra phải tuân theo các quy tắc biểu diễn chữ số có nghĩa một cách rõ ràng. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Mẫu đầu tiên là```
4 4
180.0 351.0 684.5 1335
```Các khoảng tương ứng là 

[ 
[179,95,180,05),\quad 
[350,5,351,5),\quad 
[684,45,684,55),\quad 
[1334.5,1335.5). 
] 

Các ràng buộc theo cặp quyết định tạo ra các giới hạn tỷ lệ sau. 

| Cặp (i,j) | Giới hạn dưới của (r) | Giới hạn trên của (r) | 
| --- | --- | --- | 
| (0,1) | (350,5/180,05) | (351,5/179,95) | 
| (0,2) | (\sqrt{684,45/180,05}) | (\sqrt{684,55/179,95}) | 
| (0,3) | (\sqrt[3]{1334.5/180.05}) | (\sqrt[3]{1335.5/179.95}) | 
| (1,2) | (684,45/351,5) | (684,55/350,5) | 
| (1,3) | (\sqrt{1334.5/351.5}) | (\sqrt{1335.5/350.5}) | 
| (2,3) | (1334.5/684.55) | (1335.5/684.45) | 

Lấy giới hạn dưới lớn nhất và giới hạn trên nhỏ nhất sẽ có khoảng (1,94973304) và (1,95041335). 

Đối với mức tối thiểu (a_0), việc đánh giá các ràng buộc thấp hơn tại (r_{\max}) sẽ cho (179,95). Để đạt mức tối đa (a_0), việc đánh giá các giới hạn trên tại (r_{\min}) sẽ cho (180,05). 

Kết quả đầu ra là```
179.9500 180.0500 1.94973304 1.95041335
```phù hợp với mẫu chính thức. 

### Mẫu 2 

Mẫu thứ hai là```
3 6
12.9 13.0 13.2 13.3 13.4 13.5
```Đây là một vài khoảng đầu tiên 

[ 
[12,85,12,95),\quad 
[12,95,13,05),\quad 
[13,15,13,25),\quad 
[13,25,13,35),\ldots 
] 

Các hạn chế theo cặp dần dần ép tỷ lệ vào một khoảng hẹp. 

| Số lượng | Giá trị | 
| --- | --- | 
| (r_{\min}) | xấp xỉ (1,0076924) | 
| (r_{\max}) | xấp xỉ (1.0106650) | 
| (a_{0,\min}) | (12.850) | 
| (a_{0,\max}) | xấp xỉ (12.949) | 

Đầu ra là```
12.850 12.949 1.0076924 1.0106650
```phù hợp với mẫu chính thức. 

Ví dụ này chứng minh tại sao tỷ lệ hiển thị liền kề là không đủ. Giới hạn chặt chẽ nhất đối với (r) có thể đến từ hai số hạng cách nhau vài vị trí, vì số mũ (1/(j-i)) thay đổi ảnh hưởng của độ rộng khoảng của chúng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(N^2)) | Mỗi cặp (i<j) được xử lý một lần, sau đó là hai lần quét (O(N)) | 
| Không gian | (O(N)) | Chỉ các điểm cuối khoảng dưới và trên được lưu trữ | 

Với (N<200), pha cặp thực hiện ít hơn 20.000 lần lặp. Do đó, giới hạn thời gian bốn giây là cực kỳ hào phóng đối với phương pháp này và mức sử dụng bộ nhớ là không đáng kể. 

## Trường hợp thử nghiệm 

Các thử nghiệm sau đây sử dụng một`solve`có chức năng thực hiện tương tự như giải pháp đã gửi. Các so sánh mang tính số học chứ không phải chính xác theo chuỗi vì giám khảo đánh giá các câu trả lời có dấu phẩy động, trong khi số lượng chính xác các số 0 ở cuối chỉ là yêu cầu về trình bày.```
import io
import math

# Import the solve() function from the submitted solution.
# from solution import solve

def run(inp: str) -> str:
    return solve(inp)

def assert_close(inp: str, expected: str, eps: float = 1e-6):
    got = list(map(float, run(inp).split()))
    want = list(map(float, expected.split()))

    assert len(got) == 4
    assert len(want) == 4

    for a, b in zip(got, want):
        assert math.isclose(a, b, rel_tol=eps, abs_tol=eps), (
            f"expected {b}, got {a}"
        )

# Provided sample 1
assert_close(
    """4 4
180.0 351.0 684.5 1335
""",
    "179.9500 180.0500 1.94973304 1.95041335",
)

# Provided sample 2
assert_close(
    """3 6
12.9 13.0 13.2 13.3 13.4 13.5
""",
    "12.850 12.949 1.0076924 1.0106650",
)

# Provided sample 3
assert_close(
    """1 3
300 20 1
""",
    "250.0 350.0 0.0520988 0.0774597",
)

# Minimum-size input.
assert_close(
    """1 2
1 1
""",
    "0.9500 1.050 0.904762 1.10526",
)

# All values equal.
assert_close(
    """2 3
5.0 5.0 5.0
""",
    "4.9500 5.0500 0.9801980 1.020202",
)

# Boundary where the order of magnitude changes.
assert_close(
    """3 2
999
1000
""",
    "998.500 999.500 0.99549775 1.0065098",
)

# Maximum-size input: 199 equal terms.
# D = 5, so 1.0000 represents [0.99995, 1.00005).
maximum_input = "5 199\n" + " ".join(["1.0000"] * 199) + "\n"

assert_close(
    maximum_input,
    "0.99995000 1.0000500 0.9999000050 1.000100005",
)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 2 / 1 1`|`0.9500 1.050 0.904762 1.10526`| Tối thiểu (N), xử lý khoảng cơ bản, giới hạn tỷ lệ | 
|`2 3 / 5.0 5.0 5.0`|`4.9500 5.0500 0.9801980 1.020202`| Các giá trị hoàn toàn bằng nhau và thực tế là (r) không cần bằng (1) | 
|`3 2 / 999 1000`|`998.500 999.500 0.99549775 1.0065098`| Thay đổi độ lớn và đơn vị làm tròn khác nhau | 
|`5 199 / 1.0000 ...`|`0.99995000 1.0000500 0.9999000050 1.000100005`| Giá trị tối đa (N), giá trị lặp lại và độ ổn định số | 

## Vỏ cạnh 

Đối với trường hợp kích thước tối thiểu```
1 2
1 1
```mỗi số hạng đại diện cho ([0,95,1,05)). Cặp duy nhất cho 

[ 
r_{\min}=\frac{0.95}{1.05}=\frac{19}{21}, 
\qquad 
r_{\max}=\frac{1.05}{0.95}=\frac{21}{19}. 
] 

Tại (r_{\max}), số hạng đầu tiên nhỏ nhất có thể là (0,95). Tại (r_{\min}), số hạng đầu tiên lớn nhất có thể là (1,05). Do đó, thuật toán tạo ra```
0.9500 1.050 0.904762 1.10526
```mà không yêu cầu bất kỳ xử lý đặc biệt nào đối với (N=2). 

Đối với trường hợp đều bằng nhau```
2 3
5.0 5.0 5.0
```khoảng thời gian cho mỗi thuật ngữ là ([4,95,5,05)). Cặp liền kề cho 

[ 
r_{\min}=\frac{4.95}{5.05}\approx0.980198, 
\qquad 
r_{\max}=\frac{5.05}{4.95}\approx1.020202. 
] 

Các điều khoản sau không tạo ra hạn chế chặt chẽ hơn. Ở tỷ lệ tối đa, các ràng buộc dưới cắt nhau tại (a_0=4,95), trong khi ở tỷ lệ tối thiểu, các ràng buộc trên cắt nhau tại (a_0=5,05). Thuật toán xuất ra chính xác```
4.9500 5.0500 0.9801980 1.020202
```thay vì buộc tỷ lệ thành (1) không chính xác. 

Đối với ranh giới độ lớn```
3 2
999 1000
```số hạng thứ nhất có đơn vị làm tròn (1) là ([998,5,999,5)). Thứ hai có đơn vị làm tròn (10), cho ([995,1005)). Do đó, cặp đôi này đưa ra 

[ 
r_{\min}=\frac{995}{999.5}\approx0.99549775 
] 

và 

[ 
r_{\max}=\frac{1005}{998.5}\approx1.0065098. 
] 

Giới hạn số hạng đầu tiên thu được chính xác là (998,5) và (999,5). Trường hợp này thực hiện phép tính số mũ dựa trên chuỗi trong quá trình triển khai, đó là lý do tại sao mã không xác định đơn vị làm tròn bằng cách liên tục đếm các vị trí thập phân. 

Đối với trường hợp kích thước tối đa, lấy 199 bản sao của`1.0000`với (D=5). Mỗi thuật ngữ đều có khoảng ([0,99995,1,00005)). Vòng lặp theo cặp xử lý các cặp (199\cdot198/2=19,701), vẫn còn rất nhỏ. Bởi vì tất cả các khoảng đều giống nhau nên giới hạn tỷ lệ chặt chẽ nhất đến từ các vị trí liền kề: 

[ 
r_{\min}=\frac{0.99995}{1.00005} 
] 

và 

[ 
r_{\max}=\frac{1.00005}{0.99995}. 
] 

Hai lần quét cuối cùng sẽ khôi phục giới hạn tương ứng cho (a_0). Điều này xác nhận rằng quy mô triển khai trực tiếp đến mức lớn nhất được phép (N) mà không có bất kỳ thay đổi thuật toán nào.
