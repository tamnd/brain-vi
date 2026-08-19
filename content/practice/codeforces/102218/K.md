---
title: "CF 102218K - Chữ số thiếu thứ K"
description: "Chúng ta có hai số nguyên thập phân rất lớn, A và B, và một chuỗi thập phân P phải bằng tích của chúng. Chính xác một chữ số của P đã được thay thế bằng . Chữ số bị thiếu đảm bảo là từ 1 đến 9 nên nhiệm vụ là phải tìm lại chữ số đó."
date: "2026-08-18T22:50:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102218
codeforces_index: "K"
codeforces_contest_name: "2019, XI Annual Programming Contest by ESCOM-IPN"
rating: 0
weight: 102218
solve_time_s: 251
verified: false
draft: false
---

[CF 102218K - Chữ số bị thiếu thứ K](https://codeforces.com/problemset/problem/102218/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4 phút 11s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai số nguyên thập phân rất lớn,`A`Và`B`, và một chuỗi thập phân`P`điều đó sẽ tương đương với sản phẩm của họ. Đúng một chữ số của`P`đã được thay thế bởi`*`. Chữ số bị thiếu được đảm bảo là từ`1`bởi vì`9`, vì vậy nhiệm vụ là khôi phục chữ số đó. 

Ba số đầu tiên,`a`,`b`, Và`p`, là số chữ số của`A`,`B`, Và`P`. Chúng không phải là giá trị số của những số nguyên đó. Hai số nguyên đầu vào, mỗi số có thể chứa gần một triệu chữ số, trong khi tích có thể chứa gần hai triệu chữ số. Sự cố ban đầu có giới hạn thời gian 0,5 giây và giới hạn bộ nhớ 32 MB. 

Những giới hạn đó ngay lập tức loại trừ việc điều trị`A`Và`B`như số nguyên máy thông thường. Ngay cả việc lưu trữ hoặc nhân các giá trị hàng triệu chữ số cũng đòi hỏi nhiều công việc hơn đáng kể so với phép tính số học liên tục. Giải pháp dự định phải xử lý trực tiếp các chuỗi thập phân và chỉ thực hiện một lượng công việc không đổi trên mỗi chữ số đầu vào, tạo ra độ phức tạp tuyến tính trong tổng kích thước đầu vào. 

Có một số trường hợp giải pháp bất cẩn có thể dẫn đến sai sót. Nếu ký tự bị thiếu là chữ số đầu tiên thì nó vẫn phải được đưa vào số học. Ví dụ,```
1 1 2
3
8
2*
```có sản phẩm`24`, vậy câu trả lời là`4`. Một giải pháp vô tình bỏ qua`*`vị trí khi tính tổng chữ số sẽ chỉ sử dụng chữ số hiển thị`2`và thu được dư lượng sai. 

Ký tự bị thiếu cũng có thể là chữ số cuối cùng. Ví dụ,```
1 1 2
7
8
5*
```có sản phẩm`56`, vậy câu trả lời là`6`. Việc coi ngôi sao như một dấu phân cách chứ không phải là một chữ số không xác định vẫn phải để lại phần đóng góp của nó vào tổng chữ số thập phân cần được phục hồi. 

Một trường hợp biên đặc biệt dễ xảy ra khi phần dư yêu cầu modulo 9 bằng 0. Vì bản thân số 0 bị cấm nên chữ số bị thiếu chính xác là`9`, không`0`. Ví dụ,```
1 1 1
9
1
*
```có sản phẩm`9`, vậy câu trả lời là`9`. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là thử mọi chữ số còn thiếu có thể từ`1`bởi vì`9`, thay thế`*`với chữ số đó và kiểm tra xem số kết quả có chính xác không`A * B`. Phương pháp này đúng vì một trong chín số thay thế là sản phẩm thật và câu lệnh đảm bảo rằng chữ số bị thiếu là khác 0. 

Vấn đề là kích thước của các con số. Trong trường hợp lớn nhất,`A`Và`B`mỗi người có tới`999999`chữ số và`P`có gần hai triệu chữ số. Việc xây dựng và nhân hai số nguyên khổng lồ vượt xa số học có chiều rộng cố định thông thường. Ngay cả khi chúng tôi đã có sản phẩm, việc kiểm tra tất cả chín ứng viên bằng cách quét sản phẩm sẽ yêu cầu tới`9p`, gần 18 triệu ký tự được kiểm tra trong đầu vào lớn nhất. Bản thân phép nhân sẽ đắt hơn nhiều so với việc thực hiện một cách ngây thơ. 

Quan sát quan trọng là các số thập phân bảo toàn giá trị modulo 9 thông qua tổng các chữ số của chúng. Với mọi số nguyên thập phân`X`, 

[ 
X \equiv \text{tổng các chữ số của }X \pmod 9. 
] 

Kể từ khi 

[ 
P=A\cdot B, 
] 

do đó chúng tôi có 

[ 
\operatorname{sumDigits}(P) 
\equiv 
\operatorname{sumDigits}(A)\operatorname{sumDigits}(B) 
\pmod 9. 
] 

Mỗi chữ số nhìn thấy được của`P`đóng góp một lượng đã biết vào tổng chữ số của nó. Nếu chữ số bị thiếu là`x`, sau đó 

(\text{tổng các chữ số của }A\bmod9) 
(\text{tổng các chữ số của }B\bmod9)\bmod9. 
] 

Vì vậy chúng ta chỉ cần tính ba số dư nhỏ trong khi đọc chuỗi. Không cần phép nhân số nguyên lớn nào cả. 

Chữ số bị thiếu được giới hạn ở`1`bởi vì`9`. Chín chữ số này có chín phần dư khác nhau theo modulo 9, do đó phần dư cần tìm xác định duy nhất câu trả lời. Đặc biệt, dư lượng`0`tương ứng với chữ số`9`. 

Ý tưởng Brute-Force hoạt động hiệu quả vì chỉ có chín chữ số, nhưng nó vẫn coi tích cực lớn là một số phải được xây dựng hoặc kiểm tra nhiều lần. Quan sát thấy rằng phép nhân và tổng chữ số thập phân tương tác rõ ràng theo modulo 9 loại bỏ hoàn toàn phép toán số lớn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(p) cộng với phép nhân số nguyên lớn | O(p) | Quá chậm | 
| Tối ưu | O(a + b + p) | O(a + b + p) cho chuỗi đầu vào | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số chữ số và ba chuỗi. Việc đếm không cần thiết cho việc tính toán vì bản thân các chuỗi cho chúng ta biết chính xác những chữ số nào phải được xử lý. 
2. Tính tổng các chữ số của`A`modulo 9. Chúng ta chỉ cần phần dư vì phương trình cuối cùng cũng là modulo 9. 
3. Tính tổng các chữ số của`B`modulo 9 vì lý do tương tự. 
4. Quét`P`và thêm mọi ký tự khác ngoài`*`đến tổng chữ số đang chạy theo modulo 9. Ngôi sao đóng góp một giá trị không xác định, vì vậy hãy tạm thời bỏ qua nó. 
5. Tính lượng cặn sản phẩm cần thiết 

[ 
r=(A\bmod9)(B\bmod9)\bmod9. 
] 

1. Giải quyết 

[ 
(\text{visibleSum}+x)\bmod9=r. 
] 

Ứng viên`x`là chữ số duy nhất từ`1`bởi vì`9`có phần dư theo modulo 9. Trong mã, điều này có thể được viết là 

[ 
x=(r-\text{visibleSum})\bmod9, 
] 

tiếp theo là thay đổi`0`ĐẾN`9`. 

1. In`x`. Tính duy nhất của phần dư trong số các chữ số được phép có nghĩa là không cần phải xây dựng sản phẩm hoặc kiểm tra nhiều ứng viên. 

### Tại sao nó hoạt động 

Điều bất biến là mọi chuỗi thập phân đã xử lý chỉ được biểu thị bằng tổng chữ số theo modulo 9, chính xác là phần dư giống như số nguyên được biểu thị bằng chuỗi đó. Vì`A`Và`B`, dư lượng của chúng có thể được nhân lên để thu được dư lượng của sản phẩm. Vì`P`, tất cả các chữ số nhìn thấy được đều cho một số dư đã biết và chữ số bị thiếu đóng góp chính xác`x`. Giá trị tính toán của`x`làm cho hai số dư bằng nhau và vì các chữ số`1`bởi vì`9`biểu diễn mọi dư lượng modulo 9 đúng một lần, đó`x`là chữ số duy nhất còn thiếu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def digit_sum_mod9(s):
    total = 0
    for c in s:
        if c != '\n':
            total += ord(c) - ord('0')
    return total % 9

def solve():
    a, b, p = map(int, input().split())
    A = input().strip()
    B = input().strip()
    P = input().strip()

    ra = digit_sum_mod9(A)
    rb = digit_sum_mod9(B)

    visible = 0
    for c in P:
        if c != '*':
            visible += ord(c) - ord('0')
    visible %= 9

    required = (ra * rb) % 9
    answer = (required - visible) % 9

    if answer == 0:
        answer = 9

    print(answer)

if __name__ == "__main__":
    solve()
```Người trợ giúp`digit_sum_mod9`xử lý từng ký tự chuỗi thập phân thay vì chuyển đổi nó thành số nguyên. Điều đó là cần thiết vì các số nguyên đầu vào có thể chứa gần một triệu chữ số, vượt xa kích thước số nguyên bình thường của máy. 

Vì`P`, mã chỉ bỏ qua`*`. Mỗi chữ số thực tế được bao gồm trong tổng chữ số hiển thị. Việc sử dụng`ord(c) - ord('0')`tránh tạo các đối tượng số nguyên tạm thời thông qua các chuyển đổi lặp đi lặp lại và giữ cho việc tính toán đơn giản. 

biểu hiện`(required - visible) % 9`mang lại một giá trị từ`0`bởi vì`8`. Nếu nó mang lại`1`bởi vì`8`, giá trị đó đã là chữ số bị thiếu tương ứng. Nếu nó mang lại`0`, chữ số còn thiếu phải là`9`, bởi vì`0`được loại trừ một cách rõ ràng bởi vấn đề. 

Không có vấn đề tràn số nguyên vì mỗi tổng đang chạy đều bị giảm modulo 9 sau mỗi chuỗi hoàn chỉnh và thậm chí cả số không bị giảm`visible`tổng số tiền tối đa khoảng 18 triệu đồng. Quan trọng hơn, mã không bao giờ xây dựng`A * B`. 

## Ví dụ đã hoạt động 

Đối với mẫu 1,```
1 1 2
3
8
2*
```những thay đổi trạng thái liên quan là: 

| Biến | Tiểu bang | 
| --- | --- | 
|`ra = digitSum(A) mod 9`| 3 | 
|`rb = digitSum(B) mod 9`| 8 | 
|`required = ra * rb mod 9`| 6 | 
|`visible = digitSum("2") mod 9`| 2 | 
|`answer = (6 - 2) mod 9`| 4 | 

Sản phẩm phải có tổng chữ số bằng`3 * 8 = 24`, đó là`6`modulo 9. Chữ số nhìn thấy được đóng góp`2`, do đó chữ số còn thiếu góp phần`4`. Sản phẩm hoàn thành là`24`, xác nhận kết quả. 

Đối với mẫu 2,```
2 2 3
10
10
*00
```tiểu bang là: 

| Biến | Tiểu bang | 
| --- | --- | 
|`ra = digitSum(A) mod 9`| 1 | 
|`rb = digitSum(B) mod 9`| 1 | 
|`required = ra * rb mod 9`| 1 | 
|`visible = digitSum("00") mod 9`| 0 | 
|`answer = (1 - 0) mod 9`| 1 | 

Ngôi sao là ký tự đầu tiên, vì vậy ví dụ này cũng xác minh rằng bản thân vị trí bị thiếu không cần xử lý đặc biệt. Sản phẩm hoàn thành là`100`, và câu trả lời là`1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(a + b + p) | Mỗi chữ số đầu vào được xử lý một lần. | 
| Không gian | O(a + b + p) | Các chuỗi đầu vào được lưu trữ, trong khi bản thân việc tính toán sử dụng không gian bổ sung O(1). | 

Với`a`Và`b`dưới một triệu và`p`dưới hai triệu, thuật toán chỉ thực hiện vài triệu thao tác ký tự đơn giản. Nó không bao giờ thực hiện phép nhân trên các số nguyên triệu chữ số, do đó thời gian chạy tỷ lệ tuyến tính với kích thước đầu vào và bộ nhớ làm việc bổ sung không đổi ngoài chuỗi đầu vào. 

Cuộc thi ban đầu sử dụng giới hạn 0,5 giây và giới hạn bộ nhớ 32 MB. Thuật toán cơ bản được thiết kế đặc biệt để tránh số học số lượng lớn, đây là yêu cầu thiết yếu cho các giới hạn đó. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline

    a, b, p = map(int, input().split())
    A = input().strip()
    B = input().strip()
    P = input().strip()

    ra = 0
    for c in A:
        ra = (ra + ord(c) - 48) % 9

    rb = 0
    for c in B:
        rb = (rb + ord(c) - 48) % 9

    visible = 0
    for c in P:
        if c != '*':
            visible = (visible + ord(c) - 48) % 9

    answer = (ra * rb - visible) % 9
    if answer == 0:
        answer = 9

    print(answer)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

assert run("""1 1 2
3
8
2*
""") == "4", "sample 1"

assert run("""2 2 3
10
10
*00
""") == "1", "sample 2"

assert run("""1 1 1
1
1
*
""") == "1", "minimum-size input"

assert run("""1 1 1
9
1
*
""") == "9", "residue zero must map to digit 9"

assert run("""1 1 2
7
8
5*
""") == "6", "missing digit at the end"

assert run("""2 2 3
11
11
1*1
""") == "2", "equal operands"

# Maximum-size valid construction:
# A = 1 followed by 999998 zeros
# B = 1 followed by 999998 zeros
# A * B = 1 followed by 1999996 zeros.
# The star replaces the leading 1.
n = 999999
A = "1" + "0" * (n - 1)
B = "1" + "0" * (n - 1)
P = "*" + "0" * (2 * n - 2)

max_case = f"{n} {n} {2 * n - 1}\n{A}\n{B}\n{P}\n"
assert run(max_case) == "1", "maximum-size input"

print("All tests passed.")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1 / 1 / 1 / *`|`1`| Tính toán dư lượng trực tiếp và đầu vào nhỏ nhất có thể | 
|`1 1 1 / 9 / 1 / *`|`9`| Phần dư modulo 9`0`phải sản xuất chữ số`9`| 
|`1 1 2 / 7 / 8 / 5*`|`6`| Thiếu chữ số ở vị trí cuối cùng | 
|`2 2 3 / 11 / 11 / 1*1`|`2`| Toán hạng bằng nhau và chữ số bên trong bị thiếu | 
| Quyền hạn kích thước tối đa của mười |`1`| Độ dài chuỗi gần giới hạn và chữ số bị thiếu ở đầu | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là thiếu chữ số hàng đầu. Vì```
2 2 3
10
10
*00
```chúng tôi nhận được`A mod 9 = 1`Và`B mod 9 = 1`, do đó sản phẩm phải`1`modulo 9. Phần nhìn thấy được`00`đóng góp bằng không, mang lại`x = 1`. Thuật toán không bao giờ giả định rằng ngôi sao ở đâu đó sau ký tự đầu tiên, vì vậy nó xử lý việc này một cách tự nhiên. 

Trường hợp cạnh thứ hai là thiếu chữ số cuối cùng. Vì```
1 1 2
7
8
5*
```sản phẩm là`56`. Dư lượng toán hạng là`7`Và`8`, cho`56 mod 9 = 2`. Chữ số nhìn thấy được`5`có dư lượng`5`, vậy 

[ 
x\equiv2-5\equiv6\pmod9. 
] 

Câu trả lời là`6`, và chuỗi hoàn thành là`56`. 

Trường hợp cạnh thứ ba là phần dư bắt buộc bằng 0. Vì```
1 1 1
9
1
*
```dư lượng sản phẩm là`9 * 1 mod 9 = 0`, trong khi tổng nhìn thấy được bằng không. Phép tính mang lại cho thí sinh`0`, nhưng số 0 không phải là chữ số được phép thiếu. Chữ số duy nhất được phép đồng dư với 0 modulo 9 là`9`, do đó thuật toán chuyển số 0 trung gian thành`9`. 

Trường hợp cạnh thứ tư là kích thước đầu vào tối đa. Đặt cả hai toán hạng là`1`theo sau là`999998`số không. Sản phẩm của họ là`1`theo sau là`1999996`số không. Thay thế cái đó trước`1`qua`*`cung cấp một chuỗi sản phẩm có độ dài`1999997`. Cả hai tổng chữ số toán hạng đều là`1`và mọi chữ số tích hiển thị đều bằng 0, do đó kết quả tính toán là ngay lập tức`1`. Thuật toán xử lý các chuỗi lớn một lần và không bao giờ cố gắng xây dựng tích của chúng dưới dạng số nguyên.
