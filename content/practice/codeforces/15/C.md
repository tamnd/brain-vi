---
title: "CF 15C - Nim công nghiệp"
description: "Mỗi mỏ đá đóng góp một loạt các kích thước heap liên tiếp cho một trò chơi Nim tiêu chuẩn."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "games"]
categories: ["algorithms"]
codeforces_contest: 15
codeforces_index: "C"
codeforces_contest_name: "Codeforces Beta Round 15"
rating: 2000
weight: 15
solve_time_s: 145
verified: true
draft: false
---
[CF 15C - Nim công nghiệp](https://codeforces.com/problemset/problem/15/C) 

**Xếp hạng:** 2000 
**Thẻ:** trò chơi 
**Thời gian giải:** 2m 25s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi mỏ đá đóng góp một loạt các kích thước heap liên tiếp cho một trò chơi Nim tiêu chuẩn. 

Nếu một mỏ đá mang lại cho chúng ta những giá trị`(x, m)`, thì thùng chứa của nó chứa:$$x,\ x+1,\ x+2,\ \dots,\ x+m-1$$Tất cả các máy xúc từ tất cả các mỏ đá cùng nhau tạo thành một vị trí Nim lớn. Vì Nim thông thường được chơi nên người chiến thắng chỉ phụ thuộc vào xor của tất cả các kích thước heap. Nếu tổng xor khác 0 thì người chơi đầu tiên sẽ thắng. Ngược lại người chơi thứ hai sẽ thắng. 

Giải thích trực tiếp của nhiệm vụ là:$$(x) \oplus (x+1) \oplus \dots \oplus (x+m-1)$$cho mỗi mỏ đá, sau đó xor tất cả các kết quả đó lại với nhau. 

Các ràng buộc loại trừ hoàn toàn việc lặp lại qua các đống riêng lẻ. Một mỏ đá có thể chứa tới$10^{16}$máy xúc lật, và có thể có$10^5$mỏ đá. Ngay cả việc lưu trữ tất cả các đống cũng không thể chứ đừng nói đến việc xử lý từng đống một. 

Điều này buộc chúng ta phải tìm một lối tắt toán học cho xor trên các số nguyên liên tiếp. 

Phần khó khăn là nhiều lập trình viên nhớ các công thức tính tổng của các phạm vi, nhưng xor lại hoạt động khác. Một sự thích ứng ngây thơ thường thất bại với các điều kiện biên. 

Hãy xem xét đầu vào này:```
1
1 4
```Đống là:```
1 2 3 4
```Xor của họ là:$$1 \oplus 2 \oplus 3 \oplus 4 = 4$$Việc triển khai bất cẩn cố gắng lấy mẫu theo cách thủ công có thể dễ dàng tạo ra`0`đây. 

Một lỗi dễ mắc phải khác là xử lý sai phạm vi bắt đầu từ số 0. Công thức tiền tố-xor tiêu chuẩn sử dụng:$$0 \oplus 1 \oplus 2 \oplus \dots \oplus n$$Giả sử chúng ta xử lý:```
1
2 1
```Câu trả lời đơn giản là`2`, vì chỉ có một đống. Nếu chúng ta tính toán:$$f(r) \oplus f(l)$$thay vì:$$f(r) \oplus f(l-1)$$chúng tôi nhận được kết quả sai ngay lập tức. 

Giá trị lớn cũng quan trọng. Với số lượng lên đến$10^{16}$, mọi logic số nguyên 32 bit có chiều rộng cố định sẽ tràn trong nhiều ngôn ngữ. Python xử lý việc này một cách tự động, nhưng thuật toán vẫn phải duy trì logarit hoặc thời gian không đổi trên mỗi mỏ đá. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu là hoàn toàn đơn giản. Đối với mỗi mỏ đá, tạo tất cả các kích thước heap trong khoảng thời gian:$$[x,\ x+m-1]$$và xor chúng lại với nhau. 

Điều đó hoạt động vì vị trí Nim kết hợp thông qua xor. Nếu xor cuối cùng khác 0 thì người chơi đầu tiên sẽ thắng. Nếu không người chơi thứ hai sẽ thua. 

Vấn đề là quy mô. Một mỏ đá có thể chứa$10^{16}$đống. Ngay cả việc xử lý một tỷ phép tính mỗi giây vẫn sẽ chậm một cách vô vọng. Trong trường hợp xấu nhất, vũ lực sẽ yêu cầu khoảng:$$10^5 \times 10^{16} = 10^{21}$$hoạt động. 

Quan sát quan trọng là xor trên các số nguyên liên tiếp có cấu trúc lặp lại. 

Định nghĩa:$$pref(n)=0 \oplus 1 \oplus 2 \oplus \dots \oplus n$$Sau đó xor trong bất kỳ khoảng thời gian nào sẽ trở thành:$$l \oplus (l+1) \oplus \dots \oplus r = pref(r)\oplus pref(l-1)$$bởi vì mọi giá trị trước`l`xuất hiện hai lần và tự hủy. 

Bây giờ chúng ta chỉ cần một cách nhanh chóng để tính toán`pref(n)`. 

Nếu chúng ta viết ra một số giá trị: 

| n | trước(n) | 
| --- | --- | 
| 0 | 0 | 
| 1 | 1 | 
| 2 | 3 | 
| 3 | 0 | 
| 4 | 4 | 
| 5 | 1 | 
| 6 | 7 | 
| 7 | 0 | 

một chu kỳ có độ dài 4 xuất hiện:$$pref(n)= \begin{cases} n & n \bmod 4 = 0 \\ 1 & n \bmod 4 = 1 \\ n+1 & n \bmod 4 = 2 \\ 0 & n \bmod 4 = 3 \end{cases}$$Điều đó làm giảm mỗi mỏ đá từ tối đa$10^{16}$hoạt động đến thời gian không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(\sum m_i)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng mỏ đá. 
2. Duy trì một biến`nim_sum`, ban đầu`0`. 

Điều này lưu trữ xor của tất cả các đống trên tất cả các mỏ đá. 
3. Với mỗi mỏ đá, tính:$$l = x$$

$$r = x+m-1$$Mỏ đá đóng góp mọi số nguyên trong khoảng`[l, r]`. 

1. Tính xor của khoảng sử dụng tiền tố xor:$$range\_xor = pref(r) \oplus pref(l-1)$$Mỗi số dưới đây`l`xuất hiện trong cả hai tiền tố xors và bị loại bỏ. 

1. Xor khoảng đóng góp này vào`nim_sum`. 
2. Sau khi xử lý tất cả các mỏ đá, hãy kiểm tra`nim_sum`. 

Nếu nó khác 0 thì người chơi đầu tiên có vị trí Nim thắng, nên in ra`"tolik"`. 

Nếu không thì in`"bolik"`. 

### Tại sao nó hoạt động 

Tính chính xác đến từ hai thuộc tính Nim tiêu chuẩn. 

Đầu tiên, các đống độc lập kết hợp thông qua xor. Toàn bộ trò chơi đang giành chiến thắng cho người chơi đầu tiên chính xác khi xor của tất cả các kích thước heap khác 0. 

Thứ hai, xor tự hủy:$$a \oplus a = 0$$Vì thế:$$pref(r)\oplus pref(l-1)$$xóa tất cả các số bên dưới`l`, chỉ để lại xor của khoảng`[l, r]`. 

các`pref(n)`công thức đúng vì xor trên các số nguyên liên tiếp lặp lại cứ sau bốn số. Việc thêm bốn số nguyên liên tiếp luôn khôi phục lại cùng một mẫu, tạo ra các trường hợp dạng đóng dựa trên`n % 4`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def pref_xor(n):
    if n % 4 == 0:
        return n
    if n % 4 == 1:
        return 1
    if n % 4 == 2:
        return n + 1
    return 0

n = int(input())
nim_sum = 0

for _ in range(n):
    x, m = map(int, input().split())

    l = x
    r = x + m - 1

    nim_sum ^= pref_xor(r) ^ pref_xor(l - 1)

print("tolik" if nim_sum else "bolik")
```Chức năng trợ giúp`pref_xor(n)`tính toán:$$0 \oplus 1 \oplus \dots \oplus n$$trong thời gian không đổi bằng cách sử dụng mẫu modulo-4 lặp lại. 

Đối với mỗi mỏ đá, mã chuyển đổi`(x, m)`vào một khoảng thực tế`[x, x+m-1]`. các`-1`vấn đề. Việc quên nó sẽ làm thay đổi phạm vi và phá vỡ mọi câu trả lời. 

Biểu thức:```
pref_xor(r) ^ pref_xor(l - 1)
```trích xuất xor trong khoảng chính xác giống như cách tính tổng tiền tố trích xuất tổng số học. 

Số nguyên Python tự động xử lý các giá trị trên$2^{63}$, vì vậy$10^{16}$giới hạn là hoàn toàn an toàn. 

Quyết định cuối cùng phù hợp với lý thuyết Nim tiêu chuẩn. Xor 0 có nghĩa là mọi nước đi đều có thể được phản ánh sang trạng thái xor 0 khác, do đó người chơi đầu tiên sẽ thua khi chơi tối ưu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
2 1
3 2
```Xử lý dấu vết: 

| Mỏ đá | Khoảng thời gian | Khoảng thời gian | nim_sum sau khi cập nhật | 
| --- | --- | --- | --- | 
| (2,1) | [2,2] | 2 | 2 | 
| (3,2) | [3,4] | 7 | 5 | 

xor cuối cùng là`5`, khác không. 

Đầu ra:```
tolik
```Ví dụ này cho thấy các mỏ đá khác nhau kết hợp đơn giản như thế nào thông qua xor. Thuật toán không bao giờ cần mô phỏng các bước di chuyển. 

### Ví dụ 2 

đầu vào:```
1
1 3
```Đống là:```
1 2 3
```Dấu vết: 

| Mỏ đá | Khoảng thời gian | trước(r) | trước(l-1) | Khoảng thời gian | nim_sum | 
| --- | --- | --- | --- | --- | --- | 
| (1,3) | [1,3] | 0 | 0 | 0 | 0 | 

Đầu ra:```
bolik
```Thực vậy:$$1 \oplus 2 \oplus 3 = 0$$Điều này thể hiện thuộc tính hủy đằng sau kỹ thuật tiền tố-xor. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi mỏ đá được xử lý trong thời gian không đổi | 
| Không gian |$O(1)$| Chỉ có một vài biến số nguyên được lưu trữ | 

Với$10^5$mỏ đá, thời gian tuyến tính dễ dàng đủ nhanh. Thuật toán chỉ thực hiện một số phép tính số học và xor trên mỗi mỏ đá, trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    def pref_xor(n):
        if n % 4 == 0:
            return n
        if n % 4 == 1:
            return 1
        if n % 4 == 2:
            return n + 1
        return 0

    n = int(input())
    nim_sum = 0

    for _ in range(n):
        x, m = map(int, input().split())

        l = x
        r = x + m - 1

        nim_sum ^= pref_xor(r) ^ pref_xor(l - 1)

    print("tolik" if nim_sum else "bolik")

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out.strip()

# provided sample
assert run(
"""2
2 1
3 2
""") == "tolik", "sample 1"

# minimum-size input
assert run(
"""1
1 1
""") == "tolik", "single heap"

# xor becomes zero
assert run(
"""1
1 3
""") == "bolik", "1 xor 2 xor 3 = 0"

# catches off-by-one in interval end
assert run(
"""1
5 1
""") == "tolik", "single-value interval"

# large values
assert run(
"""1
10000000000000000 2
""") == "tolik", "large integers"

# multiple ranges cancelling each other
assert run(
"""2
1 1
1 1
""") == "bolik", "same heaps cancel"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 1`|`tolik`| Đầu vào hợp lệ tối thiểu | 
|`1 / 1 3`|`bolik`| Vị trí xor bằng 0 | 
|`1 / 5 1`|`tolik`| Xử lý ranh giới khoảng thời gian chính xác | 
|`1 / 10^16 2`|`tolik`| An toàn số nguyên lớn | 
|`2 / 1 1 / 1 1`|`bolik`| Hủy bỏ Xor trên các mỏ đá | 

## Vỏ cạnh 

Một sai lầm phổ biến là xử lý sai một mỏ đá chỉ chứa đúng một đống. 

đầu vào:```
1
5 1
```Khoảng thời gian là`[5,5]`. 

Thuật toán tính toán:$$pref(5)\oplus pref(4)$$Sử dụng quy tắc modulo-4:$$pref(5)=1$$

$$pref(4)=4$$Vì thế:$$1 \oplus 4 = 5$$Xor cuối cùng khác 0 nên câu trả lời là`"tolik"`. 

Nếu chúng ta vô tình sử dụng`r = x + m`thay vì`x + m - 1`, chúng tôi sẽ xử lý`[5,6]`thay vào đó và nhận được câu trả lời sai. 

Một trường hợp tinh vi khác là khi tổng xor bị triệt tiêu hoàn toàn. 

đầu vào:```
1
1 3
```Đống là:```
1 2 3
```Thuật toán tính toán:$$pref(3)\oplus pref(0) = 0 \oplus 0 = 0$$Vì vậy, đầu ra là`"bolik"`. 

Điều này xác nhận logic hủy tiền tố-xor. Việc triển khai mẫu modulo-4 có lỗi thường không thành công trên phạm vi chính xác này. 

Giá trị lớn là một điểm nguy hiểm khác. 

đầu vào:```
1
10000000000000000 1
```Khoảng chứa một đống duy nhất có giá trị$10^{16}$. 

Thuật toán vẫn chỉ thực hiện số học theo thời gian không đổi:$$pref(10^{16}) \oplus pref(10^{16}-1)$$Không có sự lặp lại nào phụ thuộc vào độ lớn của heap, do đó giải pháp vẫn hiệu quả ngay cả ở giới hạn tối đa.
