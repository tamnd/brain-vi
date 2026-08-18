---
title: "CF 102222C - Mật mã Caesar"
description: "Chúng ta có hai chuỗi có cùng độ dài, trong đó chuỗi đầu tiên là bản rõ và chuỗi thứ hai là phiên bản được mã hóa Caesar. Mật mã Caesar áp dụng một phép dịch chu kỳ cố định cho mỗi chữ cái viết hoa."
date: "2026-08-17T22:02:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102222
codeforces_index: "C"
codeforces_contest_name: "2018-2019 ACM-ICPC, China Multi-Provincial Collegiate Programming Contest"
rating: 0
weight: 102222
solve_time_s: 84
verified: true
draft: false
---

[CF 102222C - Mật mã Caesar](https://codeforces.com/problemset/problem/102222/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 24s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai chuỗi có cùng độ dài, trong đó chuỗi đầu tiên là bản rõ và chuỗi thứ hai là phiên bản được mã hóa Caesar. Mật mã Caesar áp dụng một phép dịch chu kỳ cố định cho mỗi chữ cái viết hoa. Sự dịch chuyển tương tự được sử dụng để mã hóa chuỗi thứ ba và nhiệm vụ của chúng ta là khôi phục bản rõ của chuỗi thứ ba đó. 

Ví dụ, nếu`A`trở thành`D`, sự dịch chuyển là`3`, Vì thế`B`trở thành`E`,`Y`trở thành`B`, Và`Z`trở thành`C`. Sau khi biết được sự dịch chuyển, việc giải mã một bản mã khác chỉ yêu cầu di chuyển mọi ký tự lùi lại một lượng đó. 

Mỗi trường hợp thử nghiệm đưa ra`n`, độ dài của bản rõ và bản mã đã biết, và`m`, độ dài của bản mã chúng ta cần giải mã. Nhiều nhất là cả hai`50`, và có thể có nhiều nhất`50`trường hợp thử nghiệm. Những giới hạn này là cực kỳ nhỏ. Ngay cả một cách tiếp cận kiểm tra tất cả`26`có thể Caesar dịch chuyển và quét các chuỗi nhanh một cách thoải mái, tối đa là khoảng`26 * 50 * 50 = 65,000`so sánh ký tự trên tất cả các trường hợp thử nghiệm. Giải pháp trực tiếp vẫn được ưu tiên hơn vì sự dịch chuyển có thể được phục hồi ngay lập tức từ một cặp ký tự tương ứng. 

Các trường hợp đặc biệt chính xuất phát từ tính chất tuần hoàn của bảng chữ cái. Một sự thay đổi có thể đi qua từ`Z`quay lại`A`, do đó phép trừ số nguyên thông thường không có số học modulo có thể tạo ra mã ký tự không hợp lệ. Dịch chuyển số 0 cũng hợp lệ, nghĩa là bản rõ và bản mã có thể giống hệt nhau. Cuối cùng,`n`có thể`1`, do đó, sự dịch chuyển phải có thể phục hồi được từ một ký tự đơn. 

Ví dụ: trường hợp kiểm thử một ký tự có thể là:```
1
1 1
Z
A
A
```Cặp đã biết cho chúng ta biết rằng sự dịch chuyển mã hóa là`1`, vậy là trận chung kết`A`giải mã thành`Z`. Đầu ra đúng là:```
Case #1: Z
```Việc thực hiện bất cẩn có tính toán`ord('A') - 1`trực tiếp thay vì gói modulo`26`có thể tạo ra một nhân vật trước`A`, đây không phải là chữ in hoa hợp lệ. 

Một ví dụ về dịch chuyển không là:```
1
1 3
A
A
ABC
```Đầu ra đúng là`Case #1: ABC`. Việc triển khai giả định rằng văn bản mã hóa phải khác với văn bản gốc sẽ tìm kiếm một cách không chính xác sự dịch chuyển khác 0. 

Trường hợp bao quanh đặc biệt hữu ích:```
1
2 3
YZ
ZA
ABC
```Ở đây cặp đã biết sử dụng sự dịch chuyển của`1`, bởi vì`Y -> Z`Và`Z -> A`. Giải mã`ABC`cho`ZAB`. Việc quên thao tác modulo sẽ khiến quá trình chuyển đổi từ`A`lùi lại một bước sẽ tạo ra kết quả không hợp lệ. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là thử mọi cách`26`có thể Caesar sẽ thay đổi. Đối với mỗi ca ứng cử viên, mã hóa bản rõ đã biết và so sánh kết quả với bản mã đã biết. Chính xác một ứng cử viên được đảm bảo phù hợp. Sau khi tìm thấy nó, áp dụng phép dịch ngược cho chuỗi thứ ba. 

Lực lượng mạnh mẽ này là chính xác bởi vì mọi mật mã Caesar được mô tả hoàn toàn bằng một giá trị từ`0`bởi vì`25`. Kiểm tra mọi giá trị có thể không thể bỏ sót quy tắc mã hóa thực tế. Đối với một trường hợp thử nghiệm, điều này cần`O(26n + 26m)`thời gian, đơn giản là vậy`O(n + m)`bởi vì`26`là một hằng số. Với các giá trị tối đa`n = m = 50`, có nhiều nhất`2,600`các thao tác ký tự cho mỗi trường hợp thử nghiệm hoặc khoảng`130,000`trên tất cả`50`trường hợp. Theo các ràng buộc đã nêu, phương pháp vũ lực không thực sự quá chậm. 

Cách tiếp cận trực tiếp hơn xuất phát từ việc quan sát rằng sự dịch chuyển Caesar đã được mã hóa trong bất kỳ cặp ký tự tương ứng nào. Giả sử bản rõ bắt đầu bằng`A`và bản mã bắt đầu bằng`T`. Sự thay đổi là ngay lập tức`19`. Tổng quát hơn, nếu chỉ số bảng chữ cái của họ là`p`Và`c`, thì sự dịch chuyển mã hóa là`(c - p) mod 26`. Bởi vì đầu vào đảm bảo một phép biến đổi Caesar rõ ràng hợp lệ, sự dịch chuyển này đủ để giải mã mọi ký tự trong chuỗi thứ ba. 

Phương pháp vũ phu có hiệu quả vì chỉ có`26`những phép biến đổi có thể xảy ra nhưng nó thực hiện những tìm kiếm không cần thiết. Quan sát thấy rằng một cặp ký tự được căn chỉnh xác định sự dịch chuyển cho phép chúng ta thay thế tìm kiếm bằng một phép trừ và sau đó quét tuyến tính văn bản mã hóa đích. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(26(n + m)) = O(n + m)`|`O(m)`| Đã chấp nhận | 
| Trực tiếp rút ra sự thay đổi |`O(n + m)`|`O(m)`| Đã chấp nhận | 

Cách tiếp cận trực tiếp có hệ số hằng số tốt hơn và làm cho cấu trúc của bài toán trở nên rõ ràng. 

## Hướng dẫn thuật toán 

1. Đọc`n`,`m`, bản rõ đã biết, bản mã đã biết và bản mã phải được giải mã. Hai chuỗi đầu tiên có vị trí tương ứng vì chúng có cùng độ dài. 
2. Chuyển ký tự đầu tiên của bản rõ và bản mã thành các chỉ số bảng chữ cái từ`0`ĐẾN`25`. Tính toán dịch chuyển mã hóa như`(cipher_index - plain_index) % 26`. Một cặp tương ứng là đủ vì mật mã Caesar sử dụng chính xác cùng một độ dịch chuyển ở mọi nơi. 
3. Đối với mỗi ký tự trong văn bản mã hóa đích, hãy chuyển đổi nó thành chỉ mục bảng chữ cái và trừ đi độ dịch chuyển đã khôi phục. Áp dụng`% 26`vì vậy việc di chuyển trước`A`quấn quanh để`Z`. 
4. Chuyển từng chỉ mục kết quả trở lại thành chữ in hoa và nối các ký tự. Tiền tố kết quả với`Case #x:`sử dụng số ca kiểm thử hiện tại. 

Tại sao nó hoạt động: hãy để sự thay đổi mã hóa được thực hiện`s`. Đối với mọi vị trí trong cặp đã biết, chỉ số bản mã bằng chỉ số bản rõ cộng với`s`modulo`26`. Như vậy`(cipher_index - plain_index) % 26`phục hồi chính xác`s`. Đối với ký tự bản mã đích có chỉ mục được mã hóa`c`, chỉ số bản rõ của nó phải là`(c - s) % 26`, do đó, mọi ký tự được thuật toán tạo ra chính xác là ký tự tạo ra bản mã. Vì cùng một sự dịch chuyển áp dụng cho mọi vị trí nên việc giải mã các ký tự một cách độc lập sẽ mang lại bản rõ chính xác hoàn chỉnh. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    answers = []

    for case in range(1, t + 1):
        n, m = map(int, input().split())
        plain = input().strip()
        cipher = input().strip()
        target = input().strip()

        plain_index = ord(plain[0]) - ord('A')
        cipher_index = ord(cipher[0]) - ord('A')

        shift = (cipher_index - plain_index) % 26

        decoded = []
        for ch in target:
            value = (ord(ch) - ord('A') - shift) % 26
            decoded.append(chr(value + ord('A')))

        answers.append(f"Case #{case}: {''.join(decoded)}")

    sys.stdout.write('\n'.join(answers))

if __name__ == "__main__":
    solve()
```Cặp ký tự đầu tiên xác định`shift`. Trừ chỉ mục bản rõ khỏi chỉ mục bản mã sẽ cho biết số vị trí mà bản rõ đã được di chuyển.`% 26`xử lý thống nhất cả những khác biệt tích cực và tiêu cực. 

Vòng giải mã thực hiện thao tác nghịch đảo. Nếu mã hóa được`plain + shift`, giải mã là`cipher - shift`. Hoạt động modulo của Python đặc biệt thuận tiện ở đây vì kết quả âm tính như`-1 % 26`trở thành`25`, tương ứng với`Z`. 

Không cần thiết phải kiểm tra các ký tự còn lại của bản rõ và bản mã đã biết vì bài toán đảm bảo rằng chúng được tạo ra bởi một phép dịch chuyển Caesar nhất quán. Việc kiểm tra chúng sẽ chỉ lặp lại thông tin đã được xác định bởi cặp đầu tiên. 

Kết quả được tích lũy trong một danh sách và được nối một lần ở cuối thay vì nối các chuỗi liên tục. Điều này không cần thiết đối với các ràng buộc nhỏ, nhưng nó mang lại cấu trúc thời gian tuyến tính tiêu chuẩn và tránh các chuỗi trung gian không cần thiết. 

Không thể tràn số nguyên trong Python và thao tác ranh giới duy nhất cần cẩn thận là modulo`26`khi quá trình giải mã chuyển từ`A`ĐẾN`Z`. 

## Ví dụ đã hoạt động 

Tuyên bố cung cấp một trường hợp thử nghiệm mẫu:```
1
7 7
ACMICPC
CEOKERE
PKPIZKC
```Các ký tự tương ứng đầu tiên là`A`Và`C`, tạo ra sự dịch chuyển của`2`. Áp dụng sự dịch chuyển nghịch đảo đó cho mọi ký tự của`PKPIZKC`cho`NINGXIA`. 

| Ký tự đơn giản | Ký tự mật mã | Chỉ số đồng bằng | Chỉ số mật mã | Thay đổi | 
| --- | --- | --- | --- | --- | 
|`A`|`C`| 0 | 2 | 2 | 

Việc giải mã mục tiêu sau đó được tiến hành như sau. 

| Nhân vật mục tiêu | Chỉ số mục tiêu |`(index - shift) % 26`| Ký tự đơn giản | 
| --- | --- | --- | --- | 
|`P`| 15 | 13 |`N`| 
|`K`| 10 | 8 |`I`| 
|`P`| 15 | 13 |`N`| 
|`I`| 8 | 6 |`G`| 
|`Z`| 25 | 23 |`X`| 
|`K`| 10 | 8 |`I`| 
|`C`| 2 | 0 |`A`| 

Câu trả lời cuối cùng là`Case #1: NINGXIA`. Bảng này thể hiện tính bất biến cốt lõi: mỗi ký tự đích được dịch chuyển lùi một lượng chính xác được khôi phục như nhau. 

Đối với ví dụ thứ hai, hãy xem xét sự dịch chuyển của`25`, tương đương với việc dịch chuyển mỗi chữ cái về phía sau một vị trí trong quá trình mã hóa.```
1
2 5
YZ
ZA
ABCDE
```Cặp đôi`Y -> Z`mang lại sự thay đổi`1`, trong khi`Z -> A`xác nhận hành vi tuần hoàn. Do đó, mục tiêu được giải mã bằng cách dịch ngược từng ký tự bằng cách`1`. 

| Nhân vật mục tiêu | Chỉ số mục tiêu |`(index - shift) % 26`| Ký tự đơn giản | 
| --- | --- | --- | --- | 
|`A`| 0 | 25 |`Z`| 
|`B`| 1 | 0 |`A`| 
|`C`| 2 | 1 |`B`| 
|`D`| 3 | 2 |`C`| 
|`E`| 4 | 3 |`D`| 

Kết quả là`Case #1: ZABCD`. Dấu vết này thực hiện ranh giới bảng chữ cái, vì việc giải mã`A`yêu cầu quấn quanh để`Z`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n + m)`| Sự dịch chuyển được phục hồi theo thời gian không đổi, sau đó`m`ký tự mục tiêu được xử lý một lần. | 
| Không gian |`O(m)`| Các ký tự được giải mã được lưu trữ trước khi tạo đầu ra. | 

Với`n, m <= 50`và nhiều nhất`50`trường hợp thử nghiệm, thuật toán chỉ thực hiện vài nghìn thao tác ký tự cho mỗi trường hợp. Việc thực hiện thấp hơn nhiều so với đưa ra`10`giới hạn thứ hai và sử dụng bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline
    t = int(input())
    answers = []

    for case in range(1, t + 1):
        n, m = map(int, input().split())
        plain = input().strip()
        cipher = input().strip()
        target = input().strip()

        shift = (
            ord(cipher[0]) - ord('A')
            - (ord(plain[0]) - ord('A'))
        ) % 26

        decoded = []
        for ch in target:
            value = (ord(ch) - ord('A') - shift) % 26
            decoded.append(chr(value + ord('A')))

        answers.append(f"Case #{case}: {''.join(decoded)}")

    sys.stdout.write('\n'.join(answers))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample
assert run(
    """1
7 7
ACMICPC
CEOKERE
PKPIZKC
"""
) == "Case #1: NINGXIA\n", "provided sample"

# Minimum size, zero shift
assert run(
    """1
1 1
A
A
A
"""
) == "Case #1: A\n", "minimum size and zero shift"

# Wraparound from Z to A
assert run(
    """1
2 3
YZ
ZA
ABC
"""
) == "Case #1: ZAB\n", "alphabet wraparound"

# Maximum sizes, shift 25
assert run(
    """1
50 50
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
ZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZZ
AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAA
"""
) == "Case #1: BBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBBB\n", "maximum size"

# Multiple test cases and a nontrivial shift
assert run(
    """2
3 4
ABC
DEF
DEFG
5 6
HELLO
KHOOR
KHOORZ
"""
) == "Case #1: BCDE\nCase #2: HELLOA\n", "multiple cases"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / A / A / A`|`Case #1: A`| Kích thước tối thiểu và độ dịch chuyển bằng 0 | 
|`YZ / ZA / ABC`|`Case #1: ZAB`| Bao quanh tại`A`Và`Z`| 
| 50`A`ký tự, 50`Z`ký tự, mục tiêu là 50`A`nhân vật | 50`B`nhân vật | Độ dài tối đa cho phép và sự dịch chuyển ranh giới | 
| Hai trường hợp thử nghiệm độc lập |`Case #1: BCDE`,`Case #2: HELLOA`| Đánh số trường hợp chính xác và xử lý độc lập | 

## Vỏ cạnh 

Trường hợp một ký tự được xử lý mà không có bất kỳ nhánh đặc biệt nào. Đối với đầu vào```
1
1 1
Z
A
A
```các chỉ số là`25`Và`0`, vậy sự dịch chuyển là`(0 - 25) % 26 = 1`. mục tiêu`A`có chỉ mục`0`, Và`(0 - 1) % 26 = 25`, cho`Z`. Đầu ra là`Case #1: Z`. Một giải pháp giả định phải có ít nhất hai vị trí sẽ thất bại một cách không cần thiết. 

Đối với độ dịch chuyển bằng 0, hãy xem xét```
1
1 3
A
A
ABC
```Sự thay đổi được phục hồi là`(0 - 0) % 26 = 0`. Mỗi ký tự mục tiêu không thay đổi, tạo ra`Case #1: ABC`. Biểu thức modulo cũng làm cho sự dịch chuyển số 0 hoạt động một cách tự nhiên mà không có điều kiện riêng biệt. 

Đối với bảng chữ cái, hãy xem xét```
1
2 3
YZ
ZA
ABC
```Cặp đầu tiên cho một sự dịch chuyển của`1`và cặp thứ hai xác nhận rằng ca kết thúc từ`Z`ĐẾN`A`. Trong quá trình giải mã,`A`trở thành`(0 - 1) % 26 = 25`, vì vậy nó trở thành`Z`. Kết quả hoàn chỉnh là`Case #1: ZAB`. Điều này nắm bắt các triển khai thực hiện phép trừ mà không cần số học modulo. 

Đối với trường hợp kích thước tối đa, thuật toán vẫn sử dụng chính xác một lần truyền qua chuỗi đích sau khi khôi phục dịch chuyển. Ngay cả với`m = 50`, danh sách được giải mã chỉ chứa`50`ký tự và xử lý tất cả`50`trường hợp thử nghiệm yêu cầu nhiều nhất`2,500`nhân vật mục tiêu. Các giới hạn nhỏ làm cho hiệu năng trở nên đơn giản, trong khi lý do tương tự vẫn có hiệu lực đối với các chuỗi lớn hơn nhiều.
