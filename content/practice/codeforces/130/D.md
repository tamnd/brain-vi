---
title: "CF 130D - Hàm mũ"
description: "Chúng ta cần tính số dư khi chia $a^b$ cho $c$. Đầu vào cho ba số nguyên, mỗi số một dòng. Giá trị đầu tiên là cơ số, giá trị thứ hai là số mũ và giá trị thứ ba là mô đun. Việc giải thích trực tiếp là đơn giản."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special"]
categories: ["algorithms"]
codeforces_contest: 130
codeforces_index: "D"
codeforces_contest_name: "Unknown Language Round 4"
rating: 1500
weight: 130
solve_time_s: 111
verified: true
draft: false
---

[CF 130D - Hàm mũ](https://codeforces.com/problemset/problem/130/D) 

**Đánh giá:** 1500 
**Thẻ:** *đặc biệt 
**Thời gian giải:** 1 phút 51 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Ta cần tính số dư khi$a^b$được chia cho$c$. Đầu vào cho ba số nguyên, mỗi số một dòng. Giá trị đầu tiên là cơ số, giá trị thứ hai là số mũ và giá trị thứ ba là mô đun. 

Việc giải thích trực tiếp là đơn giản. Nếu đầu vào là:```
2
5
40
```sau đó chúng tôi tính toán$2^5 = 32$, rồi tính$32 \bmod 40 = 32$. 

Những hạn chế là rất nhỏ. Mọi giá trị đều nằm trong khoảng từ 1 đến 100. Ngay cả số mũ lớn nhất có thể cũng chỉ là 100, nghĩa là giá trị thô$100^{100}$là rất lớn về mặt toán học, nhưng Python tự động xử lý các số nguyên lớn tùy ý. Một vòng lặp đơn giản nhân số đó 100 lần là hoàn toàn an toàn trong giới hạn. 

Bởi vì số mũ nhiều nhất là 100, thậm chí là một$O(b)$giải pháp là tầm thường trong thực tế. Không có nguy cơ hết thời gian chờ khi thực hiện đơn giản. Tuy nhiên, vấn đề này là một cơ hội tốt để thảo luận về phép lũy thừa mô đun, bởi vì ý tưởng tương tự áp dụng cho các số mũ lớn bằng$10^{18}$. 

Trường hợp chính là quên áp dụng mô-đun vào đúng thời điểm. Coi như:```
10
2
7
```Câu trả lời đúng là:```
2
```bởi vì$10^2 = 100$, Và$100 \bmod 7 = 2$. 

Việc triển khai bất cẩn bằng các ngôn ngữ có số nguyên có kích thước cố định có thể bị tràn trước khi lấy mô-đun. Python tránh tràn, nhưng cách tiếp cận mô-đun vẫn là kỹ thuật chính xác về mặt toán học. 

Một trường hợp tinh vi khác là khi mô đun bằng 1:```
5
7
1
```Câu trả lời đúng là:```
0
```Mọi số nguyên modulo 1 đều bằng 0. Việc quên thuộc tính này có thể dẫn đến những giả định không chính xác về kết quả luôn dương. 

## Phương pháp tiếp cận 

Phương pháp tính toán brute-force$a^b$trực tiếp bằng cách nhân$a$tự nó$b$lần, sau đó lấy phần còn lại theo modulo$c$. 

Đối với những hạn chế này, giải pháp đó hoàn toàn có thể chấp nhận được. Số mũ nhiều nhất là 100, do đó vòng lặp thực hiện tối đa 100 phép nhân. Thậm chí lũy thừa trực tiếp bằng cách sử dụng`a ** b`hoạt động thoải mái trong Python. 

Điểm yếu của phương pháp brute-force xuất hiện khi số mũ trở nên rất lớn. Nếu như$b$đã từng$10^9$, vòng lặp thực hiện một tỷ phép nhân sẽ không thể thực hiện được trong thời gian giới hạn. 

Quan sát quan trọng là số học mô-đun cho phép chúng ta giảm các giá trị trung gian ngay lập tức:$$(x \cdot y) \bmod c = ((x \bmod c) \cdot (y \bmod c)) \bmod c$$Điều này có nghĩa là chúng ta không bao giờ cần lưu trữ toàn bộ giá trị của$a^b$. Chúng ta có thể giữ mọi kết quả trung gian nhỏ hơn$c$. 

Từ đó, lũy thừa nhị phân trở nên tự nhiên. Thay vì nhân$a$chính xác$b$nhiều lần, chúng tôi liên tục bình phương cơ số và xử lý số mũ từng chút một. Mỗi bước sẽ cắt số mũ làm đôi, giảm độ phức tạp từ$O(b)$ĐẾN$O(\log b)$. 

Mặc dù các ràng buộc không yêu cầu tối ưu hóa này nhưng đây là giải pháp tiêu chuẩn và có thể mở rộng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(b)$|$O(1)$| Đã chấp nhận | 
| Tối ưu |$O(\log b)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`a`,`b`, Và`c`. 
2. Khởi tạo`result = 1`. 

Biến này lưu trữ giá trị tích lũy của modulo lũy thừa`c`. 
3. Giảm phần đế ngay lập tức bằng`a %= c`. 

Bất kỳ bội số lớn hơn của`c`hoạt động giống hệt nhau theo số học modulo, vì vậy việc giữ cơ số nhỏ sẽ tránh được sự tăng trưởng không cần thiết. 
4. Trong khi`b > 0`, xử lý số mũ từng bit một. 
5. Nếu`b`là số lẻ, nhân kết quả hiện tại với cơ số hiện tại:$$result = (result \cdot a) \bmod c$$Số mũ lẻ có nghĩa là lũy thừa hiện tại đóng góp vào câu trả lời cuối cùng. 
6. Bình phương đáy:$$a = (a \cdot a) \bmod c$$Bình phương di chuyển từ sức mạnh như$a^1$ĐẾN$a^2$, sau đó$a^4$,$a^8$, vân vân. 
7. Chia số mũ cho 2 bằng phép chia số nguyên. 

Việc này sẽ loại bỏ bit vừa được xử lý. 
8. Sau khi vòng lặp kết thúc, in`result`. 

### Tại sao nó hoạt động 

Ở mỗi lần lặp, thuật toán duy trì tính bất biến này:$$result \cdot a^b \equiv \text{original } a^{\text{original } b} \pmod c$$Khi số mũ là số lẻ, một bản sao của cơ số hiện tại phải đóng góp vào đáp án cuối cùng, vì vậy chúng ta nhân nó thành`result`. Sau đó, chúng ta bình phương cơ số và chia đôi số mũ, giữ nguyên giá trị toán học ở dạng nén. 

Vì số mũ mất đi một chữ số nhị phân trong mỗi lần lặp nên vòng lặp cuối cùng sẽ đạt đến 0. Vào thời điểm đó, tất cả các quyền hạn cần thiết đã được tích hợp vào`result`, do đó nó bằng giá trị đúng của$a^b \bmod c$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

a = int(input())
b = int(input())
c = int(input())

result = 1
a %= c

while b > 0:
    if b % 2 == 1:
        result = (result * a) % c

    a = (a * a) % c
    b //= 2

print(result)
```Chương trình bắt đầu bằng cách đọc ba số nguyên từ đầu vào tiêu chuẩn. 

Biến`result`bắt đầu bằng 1 vì đẳng thức nhân bắt đầu từ 1. Dòng`a %= c`ngay lập tức giảm cơ sở vào phạm vi mô-đun hợp lệ. Điều này giữ cho các giá trị trung gian ở mức nhỏ và phù hợp với đặc tính toán học của phép nhân mô-đun. 

Vòng lặp xử lý số mũ ở dạng nhị phân. Khi`b`là số lẻ, bit nhị phân thấp nhất là 1, nghĩa là lũy thừa hiện tại của`a`phải được đưa vào câu trả lời. Sau đó, cơ số được bình phương để chuyển sang lũy ​​thừa tiếp theo của hai. 

dòng`b //= 2`dịch số mũ sang phải một bit. Đây là trung tâm của lũy thừa nhị phân. 

Thứ tự của các hoạt động quan trọng. Chúng ta phải kiểm tra xem`b`là số lẻ trước khi giảm một nửa, nếu không chúng ta sẽ mất thông tin về bit hiện tại. 

Số nguyên Python không bao giờ tràn, nhưng việc giữ modulo bên trong vòng lặp vẫn là chiến lược triển khai chính xác và rất cần thiết trong các ngôn ngữ có kiểu số nguyên có kích thước cố định. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
5
40
```Chúng tôi muốn tính toán$2^5 \bmod 40$. 

| Bước | b | kết quả | một | 
| --- | --- | --- | --- | 
| Bắt đầu | 5 | 1 | 2 | 
| b lẻ, nhân | 5 | 2 | 2 | 
| Hình vuông và một nửa | 2 | 2 | 4 | 
| Hình vuông và một nửa | 1 | 2 | 16 | 
| b lẻ, nhân | 1 | 32 | 16 | 
| Hình vuông và một nửa | 0 | 32 | 16 | 

Câu trả lời cuối cùng:```
32
```Dấu vết này cho thấy thuật toán chỉ nhân thành`result`khi bit số mũ hiện tại là 1. Công suất được sử dụng là$2^1$Và$2^4$, sản phẩm của ai là$2^5$. 

### Ví dụ 2 

đầu vào:```
10
2
7
```Chúng tôi muốn tính toán$10^2 \bmod 7$. 

| Bước | b | kết quả | một | 
| --- | --- | --- | --- | 
| Bắt đầu | 2 | 1 | 3 | 
| Hình vuông và một nửa | 1 | 1 | 2 | 
| b lẻ, nhân | 1 | 2 | 2 | 
| Hình vuông và một nửa | 0 | 2 | 4 | 

Câu trả lời cuối cùng:```
2
```Ví dụ này chứng tỏ tại sao việc giảm hệ số gốc sớm lại hữu ích. Từ$10 \bmod 7 = 3$, thuật toán không bao giờ hoạt động với các giá trị lớn không cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\log b)$| Số mũ giảm một nửa sau mỗi lần lặp | 
| Không gian |$O(1)$| Chỉ có một vài biến số nguyên được lưu trữ | 

Số mũ nhiều nhất là 100 nên vòng lặp chỉ thực hiện một vài lần. Giải pháp dễ dàng phù hợp với giới hạn thời gian 2 giây và giới hạn bộ nhớ 64 MB. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    a = int(input())
    b = int(input())
    c = int(input())

    result = 1
    a %= c

    while b > 0:
        if b % 2 == 1:
            result = (result * a) % c

        a = (a * a) % c
        b //= 2

    print(result)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdout = sys.__stdout__
    return out.getvalue()

# provided sample
assert run("2\n5\n40\n") == "32\n", "sample 1"

# minimum values
assert run("1\n1\n1\n") == "0\n", "minimum values"

# modulus equals 1
assert run("5\n7\n1\n") == "0\n", "modulo 1"

# all equal values
assert run("7\n7\n7\n") == "0\n", "all equal"

# larger exponent
assert run("3\n10\n13\n") == "3\n", "binary exponentiation correctness"

# maximum bounds
assert run("100\n100\n100\n") == "0\n", "maximum bounds"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1`|`0`| Đầu vào nhỏ nhất có thể | 
|`5 7 1`|`0`| Mọi số modulo 1 bằng 0 | 
|`7 7 7`|`0`| Cơ sở chia hết cho mô đun | 
|`3 10 13`|`3`| Xử lý đúng việc bình phương lặp đi lặp lại | 
|`100 100 100`|`0`| Hạn chế tối đa | 

## Vỏ cạnh 

Hãy xem xét trường hợp mô đun là 1:```
5
7
1
```Thuật toán bắt đầu bằng:```
a %= c
```Vì thế`a`trở thành`0`. 

Mọi phép nhân sau đó giữ nguyên`0`modulo`1`. Vòng lặp cuối cùng tạo ra:```
0
```điều này đúng về mặt toán học vì mọi số nguyên đều có số dư 0 khi chia cho 1. 

Bây giờ hãy xem xét trường hợp cơ số đã chia hết cho mô đun:```
7
7
7
```dòng`a %= c`một lần nữa biến căn cứ thành`0`. 

Lần đầu tiên thuật toán gặp bit số mũ lẻ, nó sẽ tính:$$result = (1 \cdot 0) \bmod 7 = 0$$Sau đó, kết quả không bao giờ có thể thay đổi. Câu trả lời cuối cùng là:```
0
```Điều này chứng tỏ tại sao việc giảm base trước khi vào vòng lặp vừa đúng vừa hiệu quả. 

Cuối cùng, hãy xem xét:```
10
2
7
```Việc triển khai đơn giản trước tiên có thể tính toán`10 * 10 = 100`và chỉ sau đó mới áp dụng mô-đun. Thay vào đó, thuật toán của chúng tôi giảm ngay lập tức:$$10 \bmod 7 = 3$$Sau đó, nó hoạt động hoàn toàn với các giá trị nhỏ:$$3^2 \bmod 7 = 2$$Đầu ra là:```
2
```Bất biến vẫn có giá trị vì phép nhân mô-đun bảo toàn tính tương đương ở mọi bước.
