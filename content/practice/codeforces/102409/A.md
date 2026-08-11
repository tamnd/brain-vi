---
title: "CF 102409A - Toán dễ dàng"
description: "Với mỗi trường hợp thử nghiệm, chúng ta có ba số nguyên dương (A), (B) và (C). Đầu tiên chúng ta cộng (A) và (B), sau đó chia tổng đó cho (C). Đầu ra được yêu cầu là biểu diễn thập phân của kết quả với chính xác 50 chữ số sau dấu thập phân."
date: "2026-08-11T16:30:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102409
codeforces_index: "A"
codeforces_contest_name: "Semana i 2019"
rating: 0
weight: 102409
solve_time_s: 133
verified: true
draft: false
---

[CF 102409A - Toán dễ dàng](https://codeforces.com/problemset/problem/102409/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 13s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Với mỗi trường hợp thử nghiệm, chúng ta có ba số nguyên dương (A), (B) và (C). Đầu tiên chúng ta cộng (A) và (B), sau đó chia tổng đó cho (C). Đầu ra được yêu cầu là biểu diễn thập phân của kết quả với chính xác 50 chữ số sau dấu thập phân. Nếu việc mở rộng số thập phân chính xác tiếp tục vượt quá 50 chữ số đó thì mọi thứ sau chữ số thứ 50 sẽ bị loại bỏ thay vì làm tròn. 

Các số có thể chứa tối đa 50 chữ số thập phân, do đó, loại số nguyên cỡ máy thông thường là không đủ trong các ngôn ngữ có số nguyên có chiều rộng cố định. Đầu vào có thể chứa tối đa (10^4) trường hợp thử nghiệm, điều này cũng loại trừ các thuật toán có thời gian chạy phụ thuộc vào giá trị số của (A), (B) hoặc (C). Trong Python, các số nguyên có độ chính xác tùy ý làm cho kích thước của các con số có thể quản lý được, nhưng chúng tôi vẫn chỉ muốn một lượng công việc không đổi cho mỗi trường hợp thử nghiệm. Vì bản thân đầu ra luôn chứa 50 chữ số phân số nên thuật toán thực hiện các bước chia dài (O(50)) cho mỗi trường hợp là mục tiêu tự nhiên. 

Có một số trường hợp việc triển khai có vẻ hợp lý lại có thể thất bại. Đầu tiên là một kết quả số nguyên chính xác. Ví dụ, với đầu vào```
1
1 3 2
```câu trả lời là```
2.00000000000000000000000000000000000000000000000000
```Một chương trình chỉ in ra thương số nguyên mà quên rằng đầu ra phải luôn chứa chính xác 50 chữ số sau dấu thập phân. 

Vấn đề thứ hai là cắt bớt thay vì làm tròn. Ví dụ,```
1
1 1 6
```đại diện cho (1/3), vì vậy câu trả lời bắt buộc bắt đầu bằng 50 bản sao của chữ số 3:```
0.33333333333333333333333333333333333333333333333333
```Chuyển đổi dấu phẩy động theo sau là định dạng có thể làm tròn và về cơ bản hơn, các số dấu phẩy động không thể biểu thị chính xác các đầu vào số nguyên 50 chữ số hoặc 50 chữ số thập phân tùy ý. 

Vấn đề thứ ba là phần phân số có thể chứa các số 0 đứng đầu. Vì```
1
1 1 100
```giá trị là (0,02), do đó đầu ra phải là```
0.02000000000000000000000000000000000000000000000000
```Những số 0 đó là một phần của trường phân số 50 chữ số và không thể xóa được. 

Cuối cùng, chữ số thứ 50 phải được giữ lại. Với```
1
1 1 99999999999999999999999999999999999999999999999999
```tử số là 2 và mẫu số có đúng 50 số chín. Chữ số phân số khác 0 đầu tiên xuất hiện ở vị trí 50, do đó phần phân số kết thúc bằng`2`. Việc triển khai chỉ tạo ra 49 chữ số có lỗi từng chữ một. 

## Phương pháp tiếp cận 

Cách giải thích trực tiếp về phép chia thập phân sẽ liên tục trừ đi mẫu số để xác định từng chữ số thương. Đối với phần nguyên, việc tìm một chữ số thương có thể yêu cầu phép trừ lên tới (C). Vấn đề tương tự xảy ra với mọi chữ số phân số. Với 50 vị trí phân số, điều này có thể yêu cầu phép trừ (O(50C)) cho một trường hợp thử nghiệm. Vì (C) có thể gần bằng (10^{50}), nên trường hợp xấu nhất là theo thứ tự trừ (5 \times 10^{51}) cho một trường hợp, và với (10^4) trường hợp thì tổng số theo lý thuyết đạt khoảng (5 \times 10^{55}). Cách tiếp cận này có giá trị về mặt toán học, nhưng kích thước số của đầu vào khiến nó hoàn toàn không thực tế. 

Quan sát hữu ích là phép chia thập phân không yêu cầu tìm kiếm từng chữ số bằng phép trừ. Phép chia dài thông thường đã cho chúng ta biết chính xác phải làm gì. Đầu tiên tính thương số nguyên bằng phép chia số nguyên. Phần còn lại sau phép chia đó chứa tất cả thông tin cần thiết cho phần phân số. 

Giả sử số dư hiện tại là (r). Để thu được chữ số thập phân tiếp theo, nhân (r) với 10 và chia cho (C). Thương số chính xác là chữ số tiếp theo và phần dư mới là phần còn lại. Lặp lại quá trình này 50 lần sẽ tạo ra chính xác 50 chữ số đầu tiên sau dấu thập phân. 

Lý do điều này hoạt động cũng giống như lý do tại sao phép chia dài thủ công hoạt động. Nếu phần dư hiện tại đại diện cho phần chưa được giải của số, nhân nó với 10 dịch chuyển phần thập phân chưa được giải sang bên trái. Chia cho (C) sẽ trích chữ số tiếp theo mà không bao giờ sử dụng số học dấu phẩy động. 

Hai cách tiếp cận có thể được so sánh như sau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(50C)) | (O(1)) | Quá chậm | 
| Phân chia dài | (O(50)) phép tính số nguyên lớn cho mỗi trường hợp thử nghiệm | (O(1)) ngoài đầu ra | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc (A), (B), và (C) và tính (N=A+B). Kiểu số nguyên của Python xử lý chính xác giá trị kết quả, mặc dù nó có thể chứa tới 51 chữ số. 
2. Tính phần nguyên với`N // C`. Điều này cung cấp mọi thứ trước dấu thập phân. 
3. Tính số dư ban đầu với`N % C`. Danh tính phân chia 
[ 
N = (\lfloor N/C\rfloor)C + (N\bmod C) 
] 
cho chúng ta biết rằng phần dư này chính xác là phần vẫn cần thiết để tạo nên các chữ số phân số. 
4. Lặp lại thao tác sau chính xác 50 lần. Nhân số dư hiện tại với 10 và chia cho (C). Thương số là chữ số thập phân tiếp theo. Lưu trữ chữ số đó, sau đó thay số dư bằng số dư chia. 

Nếu phần còn lại bằng 0 thì mọi chữ số sau đó cũng bằng 0. Chúng tôi vẫn có thể thực hiện tất cả 50 lần lặp, giúp việc triển khai trở nên đơn giản và đảm bảo độ dài đầu ra cần thiết. 
5. Chuyển đổi 50 chữ số được tạo thành một chuỗi và nối phần nguyên, dấu thập phân và chuỗi phân số. Vì chính xác 50 chữ số đã được tạo nên đầu ra luôn có định dạng được yêu cầu. 

### Tại sao nó hoạt động 

Sau khi chia số nguyên, chúng ta có (N=qC+r), trong đó (q) là phần nguyên và (0\le r<C). Phần phân số là (r/C). Ở bất kỳ bước phân số nào, giả sử số dư hiện tại là (r). Sau đó 

\frac{\lfloor 10r/C\rfloor}{10} 
+ 
\frac{(10r\bmod C)}{10C}. 
] 

Thuật ngữ đầu tiên cho biết chữ số thập phân tiếp theo, trong khi thuật ngữ thứ hai có dạng giống hệt như bài toán phân số mà chúng ta đã bắt đầu, chỉ với một phần dư mới. Do đó, mỗi lần lặp đều bảo toàn một bất biến giống nhau: các chữ số được tạo chính xác là các chữ số thập phân đứng đầu của phân số ban đầu và phần còn lại được lưu trữ biểu thị mọi thứ chưa được tạo ra. Sau 50 lần lặp, các chữ số được lưu trữ chính xác là 50 chữ số phân số đầu tiên, chính xác là kết quả bị cắt bớt được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        a, b, c = map(int, input().split())
        n = a + b

        integer_part = n // c
        remainder = n % c

        digits = []

        for _ in range(50):
            remainder *= 10
            digit = remainder // c
            remainder %= c
            digits.append(str(digit))

        out.append(f"{integer_part}." + "".join(digits))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Hai phép tính số học đầu tiên triển khai phần nguyên và phần dư ban đầu từ bước 2 và 3. Chúng phải được thực hiện trước khi tạo các chữ số phân số vì phần dư sau khi chia (A+B) cho (C) là trạng thái bắt đầu chính xác cho phép chia dài. 

Bên trong vòng lặp 50 lần,`remainder *= 10`thực hiện phép dịch thập phân. Chia số nguyên cho`c`trích xuất chữ số tiếp theo và`% c`giữ phần dư mới cho lần lặp tiếp theo. Thứ tự quan trọng: chữ số phải được lấy từ phần dư nhân trước khi thay thế phần dư đó bằng`% c`. 

Không có thao tác dấu phẩy động nào xuất hiện ở bất kỳ đâu trong giải pháp. Điều này tránh được cả việc mất độ chính xác và làm tròn ngẫu nhiên. Python cũng không có vấn đề cố định tràn 32 bit hoặc 64 bit ở đây, vì vậy các giá trị gần (10^{50}) được xử lý chính xác. 

Vòng lặp chạy 50 lần ngay cả khi phần còn lại bằng 0. Đó là cố ý. Việc dừng sớm sẽ yêu cầu đệm riêng kết quả bằng các số 0, trong khi số lần lặp cố định sẽ cung cấp trực tiếp phần phân số 50 chữ số được yêu cầu. 

trận chung kết`sys.stdout.write`thu thập tất cả các câu trả lời và viết chúng lại với nhau. Với tối đa (10^4) trường hợp thử nghiệm, điều này tốt hơn là in liên tục từng kết quả một cách độc lập. 

## Ví dụ đã hoạt động 

Hãy xem xét trường hợp mẫu đầu tiên, (A=1), (B=3), (C=2). Tổng là 4 nên thương số nguyên là 2 và số dư ban đầu bằng 0. 

| Bước | Phần còn lại hiện tại | Sau khi nhân với 10 | Chữ số | Phần còn lại mới | 
| --- | --- | --- | --- | --- | 
| Phép chia số nguyên | 4 | | 2 | 0 | 
| 1 | 0 | 0 | 0 | 0 | 
| 2 | 0 | 0 | 0 | 0 | 
| 3 | 0 | 0 | 0 | 0 | 
| ... | 0 | 0 | 0 | 0 | 
| 50 | 0 | 0 | 0 | 0 | 

Do đó, phần phân số bao gồm toàn bộ số 0, tạo ra`2.`theo sau là đúng 50 số 0. Điều này xác nhận trường hợp cạnh phân chia chính xác. 

Bây giờ hãy xem xét trường hợp mẫu thứ ba, trong đó (A=99), (B=89) và (C=17). Tổng là 188. Phép chia số nguyên cho (188//17=11), với số dư (1). Một số lần lặp phân chia dài đầu tiên là: 

| Bước | Phần còn lại hiện tại | Sau khi nhân với 10 | Chữ số | Phần còn lại mới | 
| --- | --- | --- | --- | --- | 
| Phép chia số nguyên | 188 | | 11 | 1 | 
| 1 | 1 | 10 | 0 | 10 | 
| 2 | 10 | 100 | 5 | 15 | 
| 3 | 15 | 150 | 8 | 14 | 
| 4 | 14 | 140 | 8 | 4 | 
| 5 | 4 | 40 | 2 | 6 | 
| 6 | 6 | 60 | 3 | 9 | 
| 7 | 9 | 90 | 5 | 5 | 
| 8 | 5 | 50 | 2 | 16 | 
| 9 | 16 | 160 | 9 | 7 | 

Các chữ số được tạo bắt đầu bằng`058823529`, đưa ra mẫu`11.058823529...`. Tiếp tục lặp lại tương tự trong 50 lần lặp sẽ tạo ra kết quả cắt ngắn chính xác cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(T \cdot 50)) phép tính số học số nguyên lớn | Mỗi trường hợp thử nghiệm thực hiện một phép chia số nguyên và chính xác 50 bước chia dài cho phân số. | 
| Không gian | (O(50)) cho mỗi trường hợp thử nghiệm | Tối đa 50 ký tự chữ số phân số được lưu trữ trước khi đưa ra câu trả lời. | 

Có nhiều nhất (10^4) trường hợp thử nghiệm, do đó thuật toán chỉ thực hiện khoảng 500.000 lần lặp phân số. Các toán hạng chứa tối đa khoảng 51 chữ số thập phân trong quá trình tính toán, một con số rất nhỏ đối với số học số nguyên có độ chính xác tùy ý của Python. Tác phẩm đạt được vừa vặn thoải mái trong giới hạn 5 giây và mức sử dụng bộ nhớ thấp hơn nhiều so với 256 MB. 

## Trường hợp thử nghiệm```python
# The solution is organized so that solve_case can be tested directly.
import sys
import io

def solve_case(a, b, c):
    n = a + b
    integer_part = n // c
    remainder = n % c

    digits = []

    for _ in range(50):
        remainder *= 10
        digit = remainder // c
        remainder %= c
        digits.append(str(digit))

    return f"{integer_part}." + "".join(digits)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    try:
        sys.stdin = io.StringIO(inp)
        t = int(input())
        answers = []

        for _ in range(t):
            a, b, c = map(int, input().split())
            answers.append(solve_case(a, b, c))

        return "\n".join(answers)
    finally:
        sys.stdin = old_stdin

sample = (
    "3\n"
    "1 3 2\n"
    "10 25 5\n"
    "99 89 17\n"
)

sample_expected = (
    "2.00000000000000000000000000000000000000000000000000\n"
    "7.00000000000000000000000000000000000000000000000000\n"
    "11.05882352941176470588235294117647058823529411764705"
)

assert run(sample) == sample_expected, "sample 1"

assert run("1\n1 1 1\n") == (
    "2.00000000000000000000000000000000000000000000000000"
), "all values equal"

assert run("1\n1 1 100\n") == (
    "0.02000000000000000000000000000000000000000000000000"
), "leading fractional zero"

assert run("1\n1 1 99999999999999999999999999999999999999999999999999\n") == (
    "0.00000000000000000000000000000000000000000000000002"
), "50th fractional digit"

max_value = "9" * 49
assert run(f"1\n{max_value} {max_value} {max_value}\n") == (
    "2.00000000000000000000000000000000000000000000000000"
), "maximum-size integers"

assert run("1\n1 1 6\n") == (
    "0.33333333333333333333333333333333333333333333333333"
), "truncation rather than rounding"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1`|`2.`theo sau là 50 số 0 | Kết quả số nguyên chính xác và độ dài phân số cố định | 
|`1 1 100`|`0.02`theo sau là 48 số 0 | Các số 0 đứng đầu trong phần phân số | 
|`1 1`với mẫu số là 50 số chín |`0.`theo sau là 49 số 0 và`2`| Xử lý đúng chữ số thứ 50 | 
| Hai bản sao có giá trị tối đa 49 chữ số chia cho cùng một giá trị |`2.`theo sau là 50 số 0 | Số học số nguyên có kích thước tối đa | 
|`1 1 6`|`0.`tiếp theo là 50 phần ba | Cắt ngắn và số thập phân định kỳ | 

## Vỏ cạnh 

Đối với một phân chia chính xác như```
1
1 3 2
```chúng tôi nhận được (N=4),`integer_part = 2`, Và`remainder = 0`. Mỗi lần lặp phân số sẽ nhân 0 với 10 và trích ra chữ số 0. Kết quả là chính xác`2.00000000000000000000000000000000000000000000000000`. Không có trường hợp đặc biệt nào được yêu cầu vì bất biến còn lại xử lý nó một cách tự nhiên. 

Đối với một phân số có số 0 ở đầu,```
1
1 1 100
```chúng ta có (N=2), do đó phần nguyên bằng 0 và phần dư ban đầu là 2. Lần lặp đầu tiên tính toán (20//100=0), tạo ra chữ số phân số đầu tiên`0`. Phép tính thứ hai (200//100=2), tạo ra`2`. Số dư sau đó trở thành 0 nên 48 chữ số còn lại bằng 0. Câu trả lời cuối cùng là`0.02000000000000000000000000000000000000000000000000`. 

Đối với ranh giới liên quan đến chữ số thứ 50,```
1
1 1 99999999999999999999999999999999999999999999999999
```tử số là 2 và mẫu số là (10^{50}-1). Trong 49 lần lặp đầu tiên, nhân phần còn lại với 10 vẫn cho giá trị nhỏ hơn mẫu số nên mọi chữ số được trích ra đều bằng 0. Ở lần lặp 50, phần còn lại trở thành (2\cdot10^{49}) và nhân nó với 10 sẽ được (2\cdot10^{50}). Chia cho (10^{50}-1) tạo ra chữ số 2, với phần dư mới là 2. Thuật toán thực hiện chính xác 50 lần lặp, do đó kết quả cuối cùng`2`được giữ lại và mọi thứ sau khi nó được cắt ngắn một cách chính xác. 

Đối với một số thập phân định kỳ như```
1
1 1 6
```số dư ban đầu là 2. Mỗi lần lặp lại nhân nó với 10, trích ra (20//6=3) và để lại số dư 2. Trạng thái trả về chính xác phần còn lại như cũ, vì vậy mỗi một trong số 50 chữ số được tạo ra là 3. Kết quả là`0.33333333333333333333333333333333333333333333333333`, không có việc làm tròn diễn ra. 

Đối với các giá trị có kích thước tối đa, số nguyên có độ chính xác tùy ý của Python cho phép thực hiện các thao tác tương tự mà không bị tràn. Nếu cả (A) và (B) đều là số có 49 chữ số thì tổng của chúng có thể có 50 chữ số và các phép tính thương và số dư vẫn hoạt động chính xác. Thuật toán phụ thuộc vào số chữ số, không phụ thuộc vào việc lặp đi lặp lại đến giá trị số của (C), điều này làm cho nó phù hợp với các giới hạn đã cho.
