---
title: "CF 1758A - SSeeeeeiingng DDoouubbllee"
description: "Chúng ta được cấp một chuỗi s. Nếu chúng ta \"nhân đôi\" chuỗi thì mỗi ký tự sẽ xuất hiện hai lần. Ví dụ: \"abc\" trở thành tập hợp nhiều ký tự trong \"aabbcc\". Nhiệm vụ của chúng ta không phải là tự xuất ra chuỗi nhân đôi."
date: "2026-06-09T14:37:51+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "strings"]
categories: ["algorithms"]
codeforces_contest: 1758
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 836 (Div. 2)"
rating: 800
weight: 1758
solve_time_s: 157
verified: false
draft: false
---

[CF 1758A - SSeeeeiinngg DDoouubbllee](https://codeforces.com/problemset/problem/1758/A) 

**Đánh giá:** 800 
**Tas:** thuật toán xây dựng, chuỗi 
**Thời gian giải:** 2m 37s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một chuỗi`s`. Nếu chúng ta "nhân đôi" chuỗi thì mỗi ký tự sẽ xuất hiện hai lần. Ví dụ,`"abc"`trở thành tập hợp nhiều ký tự trong`"aabbcc"`. 

Nhiệm vụ của chúng ta không phải là tự xuất ra chuỗi nhân đôi. Chúng ta có thể sắp xếp lại các ký tự nhân đôi đó theo bất kỳ thứ tự nào, miễn là chuỗi cuối cùng là một bảng màu và sử dụng chính xác các ký tự giống nhau. 

Một bảng màu đọc giống nhau từ trái sang phải và từ phải sang trái. Vì mỗi ký tự trong chuỗi nhân đôi xuất hiện với số lần chẵn nên luôn có thể sắp xếp theo kiểu đối xứng. 

Những hạn chế là rất nhỏ. Mỗi chuỗi có độ dài tối đa là 100 và có tối đa 1000 trường hợp kiểm thử. Thậm chí một$O(n^2)$giải pháp sẽ ổn ở đây. Thử thách thực sự là tìm ra cách xây dựng đơn giản nhưng luôn hiệu quả. 

Một sai lầm dễ mắc phải là suy nghĩ quá nhiều về việc sắp xếp lại và bắt đầu đếm tần số ký tự. Ví dụ:```
s = "abc"
```Chuỗi nhân đôi chứa hai bản sao của mỗi chữ cái. Giải pháp dựa trên tần số có hiệu quả nhưng không cần thiết. 

Một trường hợp tinh vi khác là khi tất cả các ký tự đều giống hệt nhau:```
s = "aaaa"
```Chuỗi nhân đôi là`"aaaaaaaa"`, vốn đã là một bảng màu. Bất kỳ cách xây dựng đúng nào cũng phải tạo ra một bảng màu hợp lệ. 

Trường hợp thứ ba là khi các ký tự lặp lại không đều:```
s = "abca"
```Một cách tiếp cận bất cẩn có thể trực tiếp tạo ra chuỗi nhân đôi:```
aabbccaa
```Đây không phải là một bảng màu. Chúng ta cần một cấu trúc đảm bảo tính đối xứng bất kể mẫu ký tự bên trong chuỗi gốc. 

## Phương pháp tiếp cận 

Quan điểm brute-force là tạo ra tất cả các sắp xếp lại của chuỗi nhân đôi và kiểm tra xem có chuỗi nào trong số chúng là palindrome hay không. 

Điều này rõ ràng là đúng vì mọi sự sắp xếp có thể đều được xem xét. Vấn đề là số lượng hoán vị. Nếu chuỗi nhân đôi có độ dài 200 thì số lần sắp xếp lại rất lớn, xấp xỉ$200!$. Ngay cả với chiều dài 20, điều này đã hoàn toàn không khả thi. 

Quan sát quan trọng xuất phát từ việc suy nghĩ trực tiếp về các palindrome thay vì nghĩ đến việc sắp xếp lại. 

Giả sử chúng ta đặt chuỗi gốc ở nửa bên trái của câu trả lời:```
s = abc
```Nếu chúng ta đặt chuỗi ngược lại của chuỗi gốc vào nửa bên phải:```
abc + cba = abccba
```kết quả tự động là một bảng màu. 

Tại sao điều này sử dụng chính xác các ký tự được yêu cầu? Mỗi nhân vật từ`s`xuất hiện một lần ở nửa bên trái và một lần ở nửa đảo ngược. Do đó, mỗi ký tự gốc xuất hiện tổng thể chính xác hai lần, đó chính xác là bộ ký tự nhiều ký tự của chuỗi nhân đôi. 

Điều này có nghĩa là chúng ta không cần đếm tần suất, sắp xếp hoặc xử lý đặc biệt. Câu trả lời luôn là:```
s + reverse(s)
```### So sánh 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O((2n)!)$|$O((2n)!)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi`s`. 
2. Tính ngược lại của nó. 
3. Nối chuỗi gốc và chuỗi đảo ngược của nó. 
4. Xuất chuỗi kết quả. 

Ý tưởng quan trọng là nửa đầu và nửa sau phản ánh chính xác lẫn nhau. Vì nửa sau đảo ngược với nửa đầu nên toàn bộ chuỗi đọc giống hệt nhau từ cả hai hướng. 

### Tại sao nó hoạt động 

Đặt chuỗi ban đầu là:```
s = c1 c2 ... cn
```Các đầu ra thuật toán:```
c1 c2 ... cn cn ... c2 c1
```Đối với mọi vị trí từ bên trái, vị trí tương ứng từ bên phải đều chứa cùng một ký tự. Do đó chuỗi này là một chuỗi palindrome. 

Mỗi ký tự của chuỗi gốc xuất hiện một lần ở nửa đầu và một lần ở nửa sau được phản chiếu. Do đó, mỗi ký tự xuất hiện chính xác hai lần, khớp với nhiều ký tự của chuỗi nhân đôi. Đầu ra vừa là một bảng màu vừa là sự sắp xếp lại hợp lệ của chuỗi nhân đôi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    s = input().strip()
    print(s + s[::-1])
```Giải pháp theo sau việc xây dựng trực tiếp. 

biểu hiện`s[::-1]`tạo ra chuỗi đảo ngược. Nối`s`ngược lại nó sẽ tạo ra một bảng màu ngay lập tức. 

Không có trường hợp ranh giới nào cần xử lý đặc biệt. Một chuỗi có độ dài một sẽ trở thành hai ký tự giống hệt nhau, vẫn là một bảng màu. Các ký tự lặp lại hoạt động một cách tự nhiên vì việc xây dựng không phụ thuộc vào tính duy nhất của ký tự. 

Việc triển khai chỉ thực hiện một lần đảo ngược và một lần nối cho mỗi trường hợp thử nghiệm, làm cho nó vừa đơn giản vừa hiệu quả. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Chuỗi đầu vào:```
sururu
```| Bước | Giá trị | 
| --- | --- | 
| Chuỗi gốc |`sururu`| 
| Đảo ngược |`ururus`| 
| Đầu ra |`sururuururus`| 

Kiểm tra tính đối xứng:```
sururuururus
||||||||||||
sururuururus
```Đọc từ một trong hai hướng cho cùng một chuỗi. Mỗi ký tự trong bản gốc xuất hiện chính xác hai lần. 

### Ví dụ 2 

Chuỗi đầu vào:```
errorgorn
```| Bước | Giá trị | 
| --- | --- | 
| Chuỗi gốc |`errorgorn`| 
| Đảo ngược |`nrogrorre`| 
| Đầu ra |`errorgornnrogrorre`| 

Nửa bên trái là chuỗi gốc và nửa bên phải là hình ảnh phản chiếu của nó. Điều này đảm bảo một bảng màu mà không cần bất kỳ phân tích tần số nào. 

Những ví dụ này chứng minh tính bất biến trung tâm: nửa sau luôn là nghịch đảo của nửa đầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Đảo ngược chuỗi và nối cả hai đều yêu cầu thời gian tuyến tính | 
| Không gian |$O(n)$| Mỗi chuỗi đầu ra và bản sao đảo ngược đều có kích thước tuyến tính | 

Đây,$n$là độ dài của chuỗi hiện tại. Từ$n \le 100$, thời gian chạy rất nhỏ. Ngay cả trên 1000 trường hợp thử nghiệm, giải pháp vẫn dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    t = int(input())
    for _ in range(t):
        s = input().strip()
        print(s + s[::-1])

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return out

# provided sample
assert run(
"""4
a
sururu
errorgorn
anutforajaroftuna
"""
) == (
"""aa
sururuururus
errorgornnrogrorre
anutforajaroftunaanutforajaroftuna
"""
)

# minimum size
assert run(
"""1
a
"""
) == (
"""aa
"""
), "single character"

# repeated characters
assert run(
"""1
aaaa
"""
) == (
"""aaaaaaaa
"""
), "all characters equal"

# distinct characters
assert run(
"""1
abc
"""
) == (
"""abccba
"""
), "basic palindrome construction"

# boundary length pattern
assert run(
"""1
abca
"""
) == (
"""abcaacba
"""
), "repeated and distinct characters mixed"
```### Tóm tắt phạm vi kiểm tra 

| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a`|`aa`| Chiều dài tối thiểu | 
|`aaaa`|`aaaaaaaa`| Tất cả các ký tự giống hệt nhau | 
|`abc`|`abccba`| Cấu tạo gương cơ bản | 
|`abca`|`abcaacba`| Hỗn hợp các ký tự lặp lại và độc đáo | 

## Vỏ cạnh 

Hãy xem xét đầu vào nhỏ nhất có thể:```
1
a
```Thuật toán tính toán:```
a + a = aa
```Kết quả là một bảng màu và chứa chính xác hai bản sao của ký tự gốc. 

Bây giờ hãy xem xét một chuỗi trong đó mọi ký tự đều giống nhau:```
1
aaaa
```Ngược lại vẫn vậy`"aaaa"`, do đó đầu ra trở thành:```
aaaaaaaa
```Mọi vị trí đều tự động khớp với vị trí được phản chiếu của nó. 

Cuối cùng, hãy xem xét một chuỗi hỗn hợp:```
1
abca
```Thuật toán thực hiện: 

| Bước | Giá trị | 
| --- | --- | 
| Bản gốc |`abca`| 
| Đảo ngược |`acba`| 
| Kết quả |`abcaacba`| 

Kiểm tra vị trí được nhân đôi:```
a b c a a c b a
a b c a a c b a
```Mọi cặp đối xứng đều khớp. Đầu ra chứa chính xác hai bản sao của mỗi ký tự từ chuỗi gốc, do đó đây là sự sắp xếp lại hợp lệ của chuỗi nhân đôi.
