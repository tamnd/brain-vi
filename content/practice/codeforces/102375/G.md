---
title: "CF 102375G - \u0415\u0441\u0442\u044c \u043b\u0438 \u0434\u0435\u043b\u0438\u0442\u0435\u043b\u044c?"
description: "Chúng ta được cung cấp một chuỗi thập phân khác rỗng, không có số 0 đứng đầu. Các chữ số không nhất thiết phải là biểu diễn thập phân của số chúng ta quan tâm. Thay vào đó, chúng ta có thể chọn cơ số (B) và sau đó diễn giải chính xác dãy chữ số giống nhau trong cơ số (B)."
date: "2026-08-15T07:12:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102375
codeforces_index: "G"
codeforces_contest_name: "\u041a\u0432\u0430\u043b\u0438\u0444\u0438\u043a\u0430\u0446\u0438\u043e\u043d\u043d\u044b\u0439 \u0440\u0430\u0443\u043d\u0434 \u0427\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442\u0430 \u0421\u0435\u0432\u0435\u0440\u043e-\u0417\u0430\u043f\u0430\u0434\u0430 \u0420\u043e\u0441\u0441\u0438\u0438 \u0438 \u041c\u043e\u0441\u043a\u0432\u044b ICPC 2019"
rating: 0
weight: 102375
solve_time_s: 517
verified: false
draft: false
---

[CF 102375G - \u0415\u0441\u0442\u044c \u043b\u0438 \u0434\u0435\u043b\u0438\u0442\u0435\u043b\u044c?](https://codeforces.com/problemset/problem/102375/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 8 phút 37 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi thập phân khác rỗng, không có số 0 đứng đầu. Các chữ số không nhất thiết phải là biểu diễn thập phân của số chúng ta quan tâm. Thay vào đó, chúng ta có thể chọn cơ số (B) và sau đó diễn giải chính xác dãy chữ số giống nhau trong cơ số (B). Nếu các chữ số là (a_0,a_1,\ldots,a_{n-1}), giá trị thu được là 

[ 
D=a_0B^{n-1}+a_1B^{n-2}+\cdots+a_{n-1}. 
] 

Cơ số phải nhiều hơn ít nhất một chữ số xuất hiện trong chuỗi. Chúng ta cần tìm một cơ số (2\le B\le10^9) và một ước số thích hợp (X), với (2\le X<D) và (X\le10^9). Nếu không có cặp nào như vậy tồn tại, chúng ta in ra (-1). Tuyên bố cuộc thi ban đầu đưa ra (n\le3\cdot10^6), giới hạn 2 giây và 512 MiB bộ nhớ. 

Độ dài là hạn chế chính. Với tối đa ba triệu chữ số, bất kỳ độ dài bậc hai nào đều không thể thực hiện được và ngay cả các thuật toán liên tục thao tác toàn bộ chuỗi nhiều lần cũng là điều không mong muốn. Chúng tôi muốn có số lần chuyển tuyến tính không đổi, lý tưởng nhất là chỉ một lần chuyển để thu thập một vài thuộc tính của các chữ số. Bản thân giá trị số (D) có thể có hàng triệu chữ số, do đó việc xây dựng nó dưới dạng số nguyên là hoàn toàn không cần thiết và không thể thực hiện được trong số học có chiều rộng cố định thông thường. 

Có một số trường hợp nhỏ khi việc xây dựng hấp dẫn không thành công. Đối với đầu vào`1`, số được biểu diễn luôn là (1), bất kể cơ số, nên không có ước số thích hợp và đáp án là chính xác`-1`. Đối với đầu vào`2`, giá trị luôn là (2), do đó việc chọn (X=2) không hợp lệ vì (X<D) là bắt buộc. Vấn đề tương tự xảy ra đối với các số nguyên tố có một chữ số`3`,`5`, Và`7`. Ngược lại, đầu vào`4`đã là hợp số rồi, vì vậy`10 2`hoạt động vì giá trị được biểu thị là (4) và ước số thích hợp của nó (2) là hợp lệ. 

Một trường hợp ranh giới khác là`10`. Tổng chữ số của nó là (1), do đó việc xây dựng dựa trên tổng chữ số không thể sử dụng (X=1). Một câu trả lời hợp lệ là`4 2`: chuỗi`10`trong cơ số (4) đại diện cho (4), chia hết cho (2). Cấu trúc đặc biệt giống nhau cho mọi chuỗi có dạng`100...0`có ít nhất hai chữ số. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ thử các cơ số bắt đầu từ cơ số pháp lý nhỏ nhất và đối với mỗi cơ số, bằng cách nào đó xác định xem số kết quả có phải là hợp số hay không. Người ta thậm chí có thể thử mọi ước số (X) có thể có từ (2) đến (10^9), đánh giá modulo số được biểu thị (X). Điều đó đúng vì việc tìm bất kỳ (X) nào có số dư bằng 0 sẽ ngay lập tức cung cấp chứng chỉ được yêu cầu, nhưng không gian tìm kiếm chứa tới (10^9) cơ sở và (10^9) ước số ứng viên, đưa ra (10^{18}) số chia trong trường hợp xấu nhất. Việc tính toán phần dư từ toàn bộ chuỗi ba triệu ký tự cho mọi ứng cử viên sẽ khiến cách tiếp cận trở nên tồi tệ hơn. 

Lực lượng vũ phu hoạt động vì tài sản duy nhất chúng ta cần là khả năng chia hết. Quan sát quan trọng là chúng tôi kiểm soát chính căn cứ đó. Giả sử chúng ta chọn 

[ 
B=X+1. 
] 

Khi đó (B\equiv1\pmod X), nên mọi lũy thừa của (B) cũng đồng dạng với (1\pmod X). Do đó, 

[ 
D 
=\sum a_iB^{n-1-i} 
\equiv\sum a_i 
\pmod X. 
] 

Số được biểu diễn modulo (X) chỉ đơn giản là tổng của các chữ số thập phân modulo (X). Điều này cho phép chúng ta chọn (X) thay vì tìm kiếm nó. 

Gọi (S) là tổng của tất cả các chữ số. Nếu (S\ge2), chọn 

[ 
X=S,\qquad B=S+1. 
] 

Sau đó (D\equiv S\equiv0\pmod X). Vì chữ số lớn nhất nhiều nhất là (S), nên cơ số (S+1) là hợp lệ. Ngoài ra, (S\le9n\le27\cdot10^6), nên cả (B) và (X) đều thấp hơn nhiều so với (10^9). 

Đối với một chuỗi có ít nhất hai chữ số, (D>X). Chữ số đứng đầu của nó là dương, vì vậy (D\ge B=S+1>X). Vậy số chia là thích hợp. 

Trường hợp duy nhất không thuộc (S\ge2) là (S=1). Vì chữ số đầu tiên khác 0 nên chuỗi phải là`1`hoặc`100...0`. Trường hợp một ký tự là giá trị (1), do đó không có câu trả lời nào tồn tại. Nếu có ít nhất hai ký tự, hãy chọn (B=4) và (X=2). Chuỗi đại diện cho (4^{n-1}), chia hết cho (2) và giá trị của nó ít nhất là (4), vì vậy (2) là ước số thực sự. 

Đối với đầu vào một chữ số có giá trị (d>1), việc thay đổi cơ số không có tác dụng gì cả. Chúng ta chỉ cần kiểm tra xem (d) có phải là hợp số hay không. Vì (d\le9), điều này có thể được xử lý trực tiếp bằng cách tìm ước số từ (2) đến (\sqrt d). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(10^{18}n)) trong tìm kiếm trực tiếp | (O(1)) ngoài đầu vào | Quá chậm | 
| Tối ưu | (O(n)) | (O(n)) cho chuỗi đầu vào | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi chữ số và tính tổng các chữ số của nó (S). Chúng ta chỉ cần tổng, bởi vì việc chọn một cơ số đồng dạng với (1) modulo (S) sẽ biến toàn bộ biểu diễn vị trí thành tổng chữ số modulo (S). 
2. Nếu chuỗi có độ dài bằng 1, hãy xử lý trực tiếp giá trị. Vì`1`, không có ước số hợp lệ. Vì`2`,`3`,`5`, Và`7`, số đó là số nguyên tố. Vì`4`,`6`,`8`, Và`9`, chọn cơ số (10) và ước số thích hợp của chữ số. 
3. Nếu chuỗi có ít nhất hai chữ số và (S=1) thì chuỗi đó nhất thiết phải là`100...0`. Chọn (B=4) và (X=2). Giá trị được biểu thị là (4^{n-1}), do đó nó chia hết cho (2) và vì (n\ge2) nên giá trị của nó ít nhất là (4). 
4. Nếu chuỗi có ít nhất hai chữ số và (S\ge2), hãy chọn (X=S) và (B=S+1). Vì mỗi chữ số nhiều nhất là (S), nên tất cả các chữ số đều hợp lệ ở cơ số (B). 
5. Lựa chọn ở bước trước cho kết quả (B\equiv1\pmod X). Do đó (B^k\equiv1\pmod X) với mọi (k) và số được biểu diễn thỏa mãn 
[ 
D\equiv S\equiv0\pmod X. 
] 
Vì chữ số đầu tiên là dương và có ít nhất hai chữ số, (D\ge B=S+1>X), nên (X) là ước số thực sự. 
6. In cặp đã tạo. Vì (S\le9\cdot3\cdot10^6=27\cdot10^6) nên cả (S) và (S+1) đều thỏa mãn giới hạn (10^9). 

Bất biến đằng sau việc xây dựng là sự đồng dư (B\equiv1\pmod X). Khi điều đó được giữ, biểu diễn vị trí sẽ mất tất cả sự phụ thuộc vào lũy thừa của (B) modulo (X), để lại chính xác tổng các chữ số của nó. Chọn (X) bằng tổng đó làm cho số được biểu diễn chia hết cho (X). Tình huống duy nhất mà số chia này sẽ là (1) là (S=1) và trường hợp đó được xử lý riêng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    s = input().strip()
    n = len(s)

    if n == 1:
        d = ord(s[0]) - ord('0')

        if d < 4:
            print(-1)
            return

        for x in range(2, int(d ** 0.5) + 1):
            if d % x == 0:
                print(10, x)
                return

        print(-1)
        return

    digit_sum = sum(ord(c) - ord('0') for c in s)

    if digit_sum == 1:
        print(4, 2)
        return

    x = digit_sum
    b = x + 1

    print(b, x)

if __name__ == "__main__":
    solve()
```Nhánh đầu tiên xử lý các chuỗi một chữ số vì cơ sở không thể thay đổi giá trị số của chúng. Đối với số có một chữ số (d), ước số hợp lệ tồn tại chính xác khi (d) là hợp số. Việc kiểm tra các ước số tối đa (\sqrt d) là quá đủ, mặc dù các giá trị này quá nhỏ nên ngay cả việc kiểm tra mã hóa cứng cũng có thể hoạt động. 

Đối với các chuỗi dài hơn, mã sẽ tính tổng các chữ số trong một lần chuyển. Số nguyên Python có thể lưu trữ số tiền này một cách thoải mái vì mức tối đa của nó chỉ là (27\cdot10^6). 

Khi tổng bằng (1), sự vắng mặt của bất kỳ chữ số khác 0 nào khác dẫn đến các chữ số không âm và thực tế là chữ số đầu tiên đã là (1). Việc xây dựng cơ sở (4), số chia (2) sau đó hoạt động mà không bao giờ xây dựng (D). 

Với tổng ít nhất (2), bộ mã`x = digit_sum`Và`b = x + 1`. Không cần phải đánh giá số lượng lớn được biểu thị bằng chuỗi. Chứng minh tính chia hết hoạt động hoàn toàn theo modulo (x). 

Cũng không có vấn đề tràn trong Python. Quan trọng hơn, việc triển khai không bao giờ tạo ra (B^{n-1}) hoặc (D), đây sẽ là chiến lược triển khai sai ngay cả trong ngôn ngữ có số nguyên có độ chính xác tùy ý vì (D) có thể chứa hàng triệu chữ số. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, đầu vào là`1`. 

| Biến | Giá trị | 
| --- | --- | 
|`s`|`1`| 
|`n`|`1`| 
|`d`|`1`| 
| Kết quả |`-1`| 

Giá trị duy nhất được biểu thị bằng một chữ số`1`là (1), độc lập với cơ số. Nó không có ước số (X>1) nên thuật toán sẽ loại bỏ nó một cách chính xác. 

Đối với Mẫu 2, đầu vào là`4`. 

| Biến | Giá trị | 
| --- | --- | 
|`s`|`4`| 
|`n`|`1`| 
|`d`|`4`| 
| Số chia được kiểm tra |`2`| 
| Kết quả |`10 2`| 

Giá trị đại diện là (4) trong mọi cơ sở. Số chia (2) là đúng và cơ số (10) là hợp lệ vì chữ số`4`có giá trị trong ký hiệu thập phân. Vì vậy, đầu ra là hợp lệ. 

Để hoàn thiện, Mẫu 3 minh họa cấu trúc chính. Tổng chữ số của nó là (1+9=10), do đó thuật toán chọn (X=10) và (B=11). Giá trị là 

[ 
D=1\cdot11+9=20, 
] 

và (20) chia hết cho (10). Đầu ra`11 10`khác với đầu ra mẫu`11 2`, nhưng cả hai đều hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n)) | Hoạt động duy nhất tùy thuộc vào độ dài đầu vào là chuyển tổng chữ số. | 
| Không gian | (O(n)) | Bản thân chuỗi đầu vào chiếm (O(n)) bộ nhớ; thuật toán sử dụng (O(1)) không gian bổ sung. | 

Với (n\le3\cdot10^6), một lần vượt qua ba triệu ký tự có thể dễ dàng nằm trong phạm vi dự kiến ​​của vấn đề. Thuật toán không thực hiện phân tích nhân tử của một số lớn và không bao giờ xây dựng giá trị được biểu thị bằng chuỗi chữ số. Tuyên bố chính thức chỉ định giới hạn thời gian 2 giây và giới hạn bộ nhớ 512 MiB và quá trình quét tuyến tính này được thiết kế cho những hạn chế đó. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    s = input().strip()
    n = len(s)

    if n == 1:
        d = ord(s[0]) - ord('0')

        if d < 4:
            print(-1)
            return

        for x in range(2, int(d ** 0.5) + 1):
            if d % x == 0:
                print(10, x)
                return

        print(-1)
        return

    digit_sum = sum(ord(c) - ord('0') for c in s)

    if digit_sum == 1:
        print(4, 2)
        return

    x = digit_sum
    b = x + 1
    print(b, x)

def run(inp: str) -> str:
    global input
    old_stdin = sys.stdin
    old_input = input

    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    try:
        solve()
        return sys.stdout.getvalue() if False else ""
    finally:
        sys.stdin = old_stdin
        input = old_input

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    old_input = input

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    input = sys.stdin.readline

    try:
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout
        input = old_input

# Provided samples.
assert run("1\n") == "-1", "sample 1"
assert run("4\n") == "10 2", "sample 2"

# Sample 3 has many valid answers. This implementation returns 11 10.
assert run("19\n") == "11 10", "sample 3"

# Custom: one-digit prime.
assert run("7\n") == "-1", "one-digit prime"

# Custom: one-digit composite.
assert run("9\n") == "10 3", "one-digit composite"

# Custom: digit sum is exactly one, with two digits.
assert run("10\n") == "4 2", "sum-one boundary"

# Custom: all digits equal.
assert run("999\n") == "28 27", "all equal digits"

# Custom: maximum length.
assert run("1" * 3_000_000 + "\n") == "3000001 3000000", "maximum length"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`7`|`-1`| Số nguyên tố một chữ số không thể được tạo thành tổng hợp bằng cách thay đổi cơ số. | 
|`9`|`10 3`| Xử lý tổng hợp một chữ số và tìm kiếm số chia. | 
|`10`|`4 2`| Cấu trúc đặc biệt (S=1) và điều kiện nghiêm ngặt (X<D). | 
|`999`|`28 27`| Tổng chữ số lớn và cách xây dựng (B=S+1). | 
|`111...111`với 3.000.000 chữ số |`3000001 3000000`| Độ dài đầu vào tối đa và giới hạn (10^9). | 

Trình trợ giúp kiểm tra phải nắm bắt đầu ra tiêu chuẩn vì giải pháp lập trình cạnh tranh ghi trực tiếp vào`stdout`. Mẫu với`19`cố tình kiểm tra đầu ra do việc triển khai cụ thể này tạo ra, vì tác vụ chấp nhận bất kỳ cặp hợp lệ nào thay vì yêu cầu cặp mẫu. 

## Vỏ cạnh 

Đối với đầu vào`1`, thuật toán sẽ nhập ngay vào nhánh một chữ số. Giá trị của nó là (1), do đó vòng lặp tìm ước số thậm chí không cần thiết và câu trả lời là`-1`. Điều này mắc phải lỗi cơ bản nhất là chọn (X) bằng chính chữ số đó mà không kiểm tra (X>1). 

Đối với đầu vào`2`, nhánh một chữ số tương tự nhận ra rằng số đó không phải là hợp số và in`-1`. Lựa chọn`B=3, X=2`bề ngoài trông có vẻ hấp dẫn, nhưng số được biểu thị vẫn là (2), do đó (X=D), vi phạm bất đẳng thức nghiêm ngặt. 

Đối với đầu vào`4`, thuật toán tìm (2) dưới dạng ước số và in`10 2`. Giá trị được biểu diễn vẫn là (4), do đó kết quả thỏa mãn (2<D). Điều này chứng tỏ tại sao đầu vào một chữ số cần được xử lý riêng biệt thay vì áp dụng cấu trúc tổng chữ số một cách máy móc. 

Đối với đầu vào`10`, tổng các chữ số là (1), không thể dùng làm ước số vì các ước số ít nhất phải bằng (2). Vì chuỗi có hai chữ số và tổng (1) nên nó phải chính xác`10`. Nhánh đặc biệt chọn (B=4), cho (D=4) và (X=2). Ở đây (2\mid4) và (2<4), do đó việc xây dựng là hợp lệ. 

Đối với đầu vào`1000`, lý luận tương tự cho (B=4), (X=2). Số được biểu thị là (4^3=64), chia hết cho (2). Thay vào đó, việc triển khai bất cẩn giả định tổng chữ số luôn ít nhất là (2) sẽ cố gắng xuất ra (X=1), điều này bị cấm. 

Đối với một đầu vào như`11`, tổng các chữ số là (2) nên thuật toán chọn (B=3) và (X=2). Giá trị được biểu thị là (1\cdot3+1=4) và (4\equiv0\pmod2). Bất đẳng thức chặt chẽ (X<D) cũng đúng. Đây là ví dụ không tầm thường nhỏ nhất của cách xây dựng chung. 

Đối với đầu vào có độ dài tối đa bao gồm ba triệu`9`chữ số thì tổng các chữ số là (27.000.000). Kết quả đầu ra của thuật toán (B=27.000.001) và (X=27.000.000). Cả hai đều nhỏ hơn nhiều so với (10^9) và (B) chắc chắn lớn hơn mọi chữ số. Giá trị được biểu thị bằng với tổng chữ số modulo (X), do đó nó chia hết cho (X), trong khi độ dài của nó đảm bảo rằng nó lớn hơn nhiều so với (X). Thuật toán không bao giờ tạo ra giá trị khổng lồ này, đó là lý do chính khiến giải pháp vẫn tuyến tính ở kích thước đầu vào.
