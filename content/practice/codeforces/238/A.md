---
title: "CF 238A - Không phải chuỗi len"
description: "Chúng tôi làm việc với các mảng có độ dài n, trong đó mỗi phần tử là số nguyên từ 0 đến 2^m - 1. Một chuỗi được gọi là \"wool\" nếu tồn tại một số mảng con liền kề có xor bằng 0. Chúng tôi được yêu cầu đếm xem có bao nhiêu chuỗi không phải là len, nghĩa là mọi mảng con liền kề đều có xor khác 0."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "math"]
categories: ["algorithms"]
codeforces_contest: 238
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 148 (Div. 1)"
rating: 1300
weight: 238
solve_time_s: 318
verified: true
draft: false
---

[CF 238A - Chuỗi không phải len](https://codeforces.com/problemset/problem/238/A) 

**Đánh giá:** 1300 
**Tags:** thuật toán xây dựng, toán học 
**Thời gian giải:** 5 phút 18s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi làm việc với các mảng có độ dài`n`, trong đó mỗi phần tử là một số nguyên từ`0`ĐẾN`2^m - 1`. 

Một chuỗi được gọi là "len" nếu tồn tại một số mảng con liền kề có xor là`0`. Chúng ta được yêu cầu đếm xem có bao nhiêu chuỗi _not_ len, nghĩa là mọi mảng con liền kề đều có xor khác 0. 

Câu trả lời phải được tính theo modulo`10^9 + 9`. 

Điều đầu tiên cần chú ý là xor trên một mảng con được kết nối tự nhiên với xors tiền tố. Nếu chúng ta định nghĩa$$p_i = a_1 \oplus a_2 \oplus \cdots \oplus a_i$$với`p_0 = 0`, sau đó là xor của mảng con`[l, r]`bằng$$p_r \oplus p_{l-1}$$Một mảng con xor trở thành 0 chính xác khi hai xor tiền tố bằng nhau. 

Vì vậy vấn đề đang bí mật hỏi: 

"Có bao nhiêu mảng tạo ra các xor tiền tố riêng biệt theo cặp`p_0, p_1, ..., p_n`?" 

Sự cải cách đó là toàn bộ vấn đề. 

Các ràng buộc đủ lớn nên việc liệt kê lực lượng vũ phu là không thể. Cả hai`n`Và`m`có thể đạt được`10^5`. Số mảng là$$(2^m)^n = 2^{mn}$$lớn về mặt thiên văn ngay cả đối với các giá trị vừa phải. Bất kỳ giải pháp nào lặp lại các mảng, tập hợp con hoặc trạng thái xor cho mỗi vị trí theo kiểu bậc hai sẽ thất bại. 

Độ phức tạp của mục tiêu gần như tuyến tính hoặc logarit trong`n`Và`m`. 

Có một số trường hợp khó bỏ sót. 

Nếu như`n >= 2^m`, câu trả lời tự động là 0. chỉ có`2^m`có thể có các giá trị tiền tố xor, nhưng chúng ta cần`n + 1`các giá trị riêng biệt vì`p_0`. Theo nguyên tắc chuồng bồ câu, sự lặp lại là không thể tránh khỏi. 

Ví dụ:```
n = 2, m = 1
```Các giá trị được phép là`{0, 1}`. 

Chỉ có hai xor tiền tố khả thi, nhưng chúng ta cần ba giá trị riêng biệt:$$p_0, p_1, p_2$$Không thể, vậy đáp án đúng là`0`. 

Việc triển khai bất cẩn có thể cố gắng tính giai thừa giảm dần với các số hạng âm và vô tình tạo ra rác thay vì 0. 

Một trường hợp tinh vi khác là mảng chứa số 0. Bất kỳ phần tử nào`a_i = 0`ngay lập tức tạo ra một mảng con zero-xor có độ dài bằng một. 

Ví dụ:```
n = 1, m = 3
```Số được phép là`0..7`. 

Giá trị duy nhất`0`bị cấm nên câu trả lời là`7`. 

Một dẫn xuất lỗi mà quên mất`p_i != p_{i-1}`sẽ đếm không chính xác tất cả`8`mảng. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Tạo mọi mảng có độ dài`n`, tính xor cho mọi mảng con và kiểm tra xem có xor nào bằng 0 hay không. 

có`2^{mn}`mảng. Ngay cả khi chúng ta tối ưu hóa tính toán xor mảng con bằng cách sử dụng xor tiền tố, mỗi mảng vẫn yêu cầu kiểm tra tất cả`O(n^2)`mảng con. 

Tổng độ phức tạp trở thành$$O(2^{mn} \cdot n^2)$$điều này là vô vọng ngay cả đối với những đầu vào nhỏ. 

Vấn đề sẽ có thể giải quyết được khi chúng ta viết lại điều kiện bằng tiền tố xors. 

Định nghĩa:$$p_0 = 0$$

$$p_i = a_1 \oplus a_2 \oplus \cdots \oplus a_i$$Sau đó:$$a_i = p_i \oplus p_{i-1}$$Mỗi lựa chọn xor tiền tố sẽ xác định duy nhất mảng. 

Một mảng con`[l, r]`có xor 0 chính xác khi:$$p_r = p_{l-1}$$Vì vậy, một chuỗi không chính xác khi tất cả các tiền tố xors đều khác biệt. 

Bây giờ bài toán trở thành tổ hợp. 

có`2^m`giá trị xor có thể. 

Chúng tôi đã sửa rồi`p_0 = 0`. 

Để chọn`p_1`, chúng ta có thể sử dụng bất kỳ giá trị nào ngoại trừ`0`, cho:$$2^m - 1$$sự lựa chọn. 

Vì`p_2`, chúng ta phải tránh hai giá trị được sử dụng:$$2^m - 2$$sự lựa chọn. 

Tiếp tục quá trình này:$$(2^m - 1)(2^m - 2)\cdots(2^m - n)$$Đây chỉ là một giai thừa rơi xuống. 

Nếu như`n >= 2^m`, một thừa số trở thành 0, vì vậy câu trả lời là 0. 

Toàn bộ vấn đề được rút gọn thành phép nhân mô-đun. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^{mn} \cdot n^2)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n + \log m)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`Và`m`. 
2. Tính toán$$K = 2^m \pmod{10^9+9}$$sử dụng lũy ​​thừa mô đun nhanh. 

1. Nếu`n >= K`trong số học số nguyên thông thường, câu trả lời phải bằng 0 vì chúng ta cần`n + 1`các giá trị tiền tố xor riêng biệt nhưng chỉ`K`các giá trị tồn tại. 

Từ`2^m`có thể rất lớn, chúng tôi không tính toán trực tiếp. Chúng ta chỉ cần biết liệu`2^m <= n`. 

Bởi vì`n ≤ 10^5`, chúng ta có thể kiểm tra một cách an toàn: 

- nếu`m >= 17`, sau đó`2^m > 10^5`, Vì thế`n < 2^m`tự động 
- ngược lại tính toán`1 << m`1. Khởi tạo`answer = 1`. 
2. Đối với mọi`i`từ`1`ĐẾN`n`, nhân:$$answer \gets answer \times (K - i)$$modulo`10^9 + 9`. 

Điều này tương ứng với việc chọn một tiền tố xor riêng biệt mới ở mỗi bước. 

1. In câu trả lời cuối cùng. 

### Tại sao nó hoạt động 

Điều bất biến là sau khi xử lý vị trí`i`, các giá trị tiền tố xor$$p_0, p_1, \dots, p_i$$đều khác biệt. 

Ở bước`i`, chính xác`i`giá trị xor đã được sử dụng, vì vậy có:$$2^m - i$$những lựa chọn hợp lệ cho`p_i`. 

Mỗi chuỗi xor tiền tố riêng biệt hợp lệ tương ứng với chính xác một mảng vì:$$a_i = p_i \oplus p_{i-1}$$và mỗi mảng tạo ra chính xác một chuỗi xor tiền tố. 

Vì vậy, việc đếm các mảng hợp lệ cũng giống như việc đếm các cách chọn các giá trị xor tiền tố riêng biệt từng bước. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 9

def solve():
    n, m = map(int, input().split())

    # Check whether 2^m <= n without overflow
    if m < 17 and (1 << m) <= n:
        print(0)
        return

    total = pow(2, m, MOD)

    ans = 1

    for i in range(1, n + 1):
        ans = (ans * (total - i)) % MOD

    print(ans)

solve()
```Chi tiết quan trọng đầu tiên là kiểm tra số 0 sớm. 

Điều kiện toán học là:$$n \ge 2^m$$nhưng tính toán trực tiếp`2^m`có thể trở nên lớn một cách không cần thiết. Từ`n`nhiều nhất là`100000`, bất kì`m >= 17`tự động thỏa mãn:$$2^m > 100000$$vì vậy câu trả lời không thể trở thành số 0 theo nguyên lý chuồng bồ câu. Chỉ nhỏ`m`các giá trị cần kiểm tra rõ ràng. 

Biến`total`cửa hàng:$$2^m \bmod MOD$$điều này là đủ cho tất cả các phép nhân sau này vì mọi phép toán đều có modulo`MOD`. 

Vòng lặp nhân lên:$$(2^m - 1)(2^m - 2)\cdots(2^m - n)$$khớp chính xác với đạo hàm tổ hợp. 

Phép trừ phải xảy ra trước phép nhân modulo. Việc quên dấu ngoặc đơn ở đây là nguyên nhân phổ biến gây ra lỗi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2
```Chúng tôi có:$$2^m = 4$$Việc tính toán tiến hành như sau. 

| Bước | Yếu tố | Trả lời | 
| --- | --- | --- | 
| Bắt đầu | - | 1 | 
| tôi = 1 | 4 - 1 = 3 | 3 | 
| tôi = 2 | 4 - 2 = 2 | 6 | 
| tôi = 3 | 4 - 3 = 1 | 6 | 

Câu trả lời cuối cùng:```
6
```Điều này phù hợp với mẫu. 

Dấu vết thể hiện ý tưởng đếm cốt lõi. Ở mỗi bước, chúng tôi chọn một tiền tố xor mới chưa xuất hiện trước đó. 

### Ví dụ 2 

đầu vào:```
2 1
```Giá trị xor được phép là`{0, 1}`. 

Chúng ta cần ba xor tiền tố riêng biệt:$$p_0, p_1, p_2$$điều đó là không thể. 

| Bước | Giá trị | 
| --- | --- | 
|$2^m$| 2 | 
| Bắt buộc phải có tiền tố riêng biệt xors | 3 | 
| Khả thi? | Không | 

Câu trả lời cuối cùng:```
0
```Ví dụ này minh họa trường hợp nguyên lý cạnh của chuồng bồ câu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n + \log m)$| lũy thừa mô-đun cộng với một vòng lặp`n`| 
| Không gian |$O(1)$| chỉ một vài biến số nguyên được lưu trữ | 

Vòng lặp chạy nhiều nhất`100000`lặp đi lặp lại, đó là tầm thường trong thời gian giới hạn. Việc sử dụng bộ nhớ là không đổi. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

MOD = 10**9 + 9

def solve():
    input = sys.stdin.readline

    n, m = map(int, input().split())

    if m < 17 and (1 << m) <= n:
        print(0)
        return

    total = pow(2, m, MOD)

    ans = 1

    for i in range(1, n + 1):
        ans = (ans * (total - i)) % MOD

    print(ans)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out.getvalue().strip()

# provided sample
assert run("3 2\n") == "6", "sample 1"

# minimum input
assert run("1 1\n") == "1", "single nonzero value"

# impossible because n >= 2^m
assert run("2 1\n") == "0", "pigeonhole principle"

# another small manual case
assert run("1 3\n") == "7", "all values except zero"

# boundary where n = 2^m - 1
assert run("3 2\n") == "6", "largest possible nonzero case"

# large m where zero condition should not trigger
res = int(run("100000 100000\n"))
assert 0 <= res < MOD, "large limits"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`1`| trường hợp không tầm thường nhỏ nhất | 
|`2 1`|`0`| kịch bản tiền tố khác biệt không thể | 
|`1 3`|`7`| phần tử 0 phải được loại trừ | 
|`3 2`|`6`| tính toán giai thừa rơi | 
|`100000 100000`| giá trị modulo hợp lệ | hiệu suất ở giới hạn tối đa | 

## Vỏ cạnh 

Hãy xem xét:```
2 1
```Chỉ có hai trạng thái xor:`0`Và`1`. 

Thuật toán kiểm tra:$$2^1 = 2$$Từ`n = 2`, chúng tôi cần`3`xors tiền tố riêng biệt, điều này là không thể. Tình trạng ban đầu trở lại`0`. 

Điều này tránh việc vô tình tính toán:$$(2-1)(2-2)=0$$thông qua số học mô-đun sau nhiều thao tác không cần thiết. 

Bây giờ hãy xem xét:```
1 3
```Các giá trị được phép là`0..7`. 

Thuật toán tính toán: 

| Bước | Yếu tố | Trả lời | 
| --- | --- | --- | 
| Bắt đầu | - | 1 | 
| tôi = 1 | 8 - 1 = 7 | 7 | 

Mảng bị cấm duy nhất là`(0)`bởi vì một số 0 đã tạo thành một mảng con zero-xor. 

Thuật toán xử lý việc này một cách tự động vì`p_1`không thể bằng`p_0 = 0`. 

Cuối cùng hãy xem xét:```
3 1
```Đây:$$2^1 = 2$$nhưng chúng ta sẽ cần bốn xor tiền tố riêng biệt. 

Việc kiểm tra sớm ngay lập tức trở lại`0`. 

Nếu không có sự kiểm tra đó, việc triển khai bất cẩn có thể tiếp tục nhân các giá trị âm theo modulo`MOD`, tạo ra một câu trả lời khác 0 vô nghĩa.
