---
title: "CF 66A - Petya và Java"
description: "Chúng ta được cung cấp một số nguyên thập phân dưới dạng một chuỗi. Số có thể cực kỳ lớn, dài tới 100 chữ số, vì vậy nó có thể không vừa với các kiểu số nguyên thông thường trong nhiều ngôn ngữ lập trình. Nhiệm vụ là xác định loại số nguyên Java nhỏ nhất có thể lưu trữ giá trị này."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 66
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 61 (Div. 2)"
rating: 1300
weight: 66
solve_time_s: 93
verified: true
draft: false
---

[CF 66A - Petya và Java](https://codeforces.com/problemset/problem/66/A) 

**Đánh giá:** 1300 
**Tags:** triển khai, chuỗi 
**Thời gian giải:** 1m 33s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một số nguyên thập phân dưới dạng một chuỗi. Số có thể cực kỳ lớn, dài tới 100 chữ số, vì vậy nó có thể không vừa với các kiểu số nguyên thông thường trong nhiều ngôn ngữ lập trình. 

Nhiệm vụ là xác định loại số nguyên Java nhỏ nhất có thể lưu trữ giá trị này. Các loại có sẵn được kiểm tra theo thứ tự sau:`byte`,`short`,`int`,`long`, và cuối cùng`BigInteger`. Mỗi loại nguyên thủy có một phạm vi bao gồm cố định. Chúng ta phải in loại đầu tiên có phạm vi chứa số đã cho. 

Khó khăn chính đến từ kích thước đầu vào. Số nguyên 100 chữ số lớn hơn nhiều so với số nguyên có dấu 64 bit, do đó việc chuyển đổi trực tiếp đầu vào thành loại số thông thường là nguy hiểm trong các ngôn ngữ có số nguyên có chiều rộng cố định. Việc triển khai bất cẩn có thể bị tràn trước khi việc so sánh xảy ra. 

Các ràng buộc là rất nhỏ về kích thước đầu vào. Chúng tôi chỉ xử lý một chuỗi có độ dài tối đa là 100, do đó, ngay cả công việc theo thời gian tuyến tính cũng không đáng kể. Vấn đề không phải là tối ưu hóa, mà là so sánh an toàn và xử lý các giá trị biên một cách chính xác. 

Một số trường hợp phức tạp có thể âm thầm phá vỡ các giải pháp không chính xác. 

Hãy xem xét đầu vào:```
128
```Câu trả lời đúng là:```
short
```

`byte`kết thúc tại`127`, và các ranh giới là bao gồm. Một sự nghiêm khắc`<`so sánh thay vì`<=`sẽ từ chối`127`sai hoặc chấp nhận`128`không chính xác. 

Một trường hợp nguy hiểm khác là:```
9223372036854775808
```Câu trả lời đúng là:```
BigInteger
```Giá trị này chính xác lớn hơn một giá trị so với số nguyên 64 bit có dấu tối đa. Trong C++ hoặc Java, cố gắng phân tích cú pháp này trực tiếp thành một`long long`hoặc`long`sẽ tràn. Cách tiếp cận an toàn hơn là số học có độ chính xác tùy ý hoặc so sánh chuỗi trực tiếp. 

Một ví dụ nhỏ hơn nhưng tinh tế hơn là:```
2147483647
```Câu trả lời đúng là:```
int
```Đây chính xác là giới hạn trên của`int`, do đó thuật toán phải coi các giới hạn là bao hàm. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng phân tích số thành các loại số ngày càng lớn hơn và bắt lỗi. Trong Python, điều này hoạt động vì số nguyên Python có độ chính xác tùy ý, nhưng trong các ngôn ngữ có số nguyên có chiều rộng cố định, điều này trở nên rủi ro. Việc phân tích một giá trị 100 chữ số thành một loại số nguyên bình thường có thể bị tràn trước khi chúng ta so sánh nó. 

Một giải pháp kiểu vũ phu khác là so sánh chuỗi đầu vào với mọi số nguyên có thể có trong mỗi phạm vi, nhưng điều đó rõ ràng là không khả thi. Ngay cả`long`phạm vi chứa nhiều hơn$10^{18}$các giá trị. 

Quan sát hữu ích là chúng ta chỉ cần so sánh đầu vào với bốn giá trị biên cố định: 

- 127 
- 32767 
- 2147483647 
- 9223372036854775807 

Nếu số đó nhỏ hơn hoặc bằng một trong các giới hạn này thì loại đó có thể lưu trữ nó. 

Vì Python hỗ trợ các số nguyên có độ chính xác tùy ý một cách tự nhiên nên giải pháp đơn giản nhất được chấp nhận là chuyển đổi chuỗi thành số nguyên một lần, sau đó so sánh với các giới hạn đã biết theo thứ tự. Độ dài đầu vào chỉ có 100 chữ số nên việc phân tích cú pháp rất rẻ. 

Thuật toán trở thành một chuỗi so sánh từ loại nhỏ nhất trở lên. Phạm vi phù hợp đầu tiên sẽ đưa ra câu trả lời ngay lập tức. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Phân tích cú pháp lặp đi lặp lại/chuyển đổi chiều rộng cố định không an toàn | O(L) | O(1) | Rủi ro trong nhiều ngôn ngữ | 
| So sánh tuần tự với các giới hạn | O(L) | O(1) | Đã chấp nhận | 

Đây,$L$là số chữ số ở đầu vào. 

## Hướng dẫn thuật toán 

1. Đọc số đầu vào dưới dạng chuỗi. 
2. Chuyển đổi chuỗi thành số nguyên Python. 

Số nguyên Python có độ chính xác tùy ý, do đó, ngay cả giá trị 100 chữ số cũng được xử lý an toàn. 
3. So sánh giá trị với`byte`giới hạn trên`127`. 

Nếu giá trị lớn nhất`127`, in`"byte"`và dừng lại. 
4. Ngược lại so sánh với`short`giới hạn trên`32767`. 

Nếu giá trị phù hợp ở đây, hãy in`"short"`. 
5. Ngược lại so sánh với`int`giới hạn trên`2147483647`. 

Nếu giá trị phù hợp ở đây, hãy in`"int"`. 
6. Ngược lại so sánh với`long`giới hạn trên`9223372036854775807`. 

Nếu giá trị phù hợp ở đây, hãy in`"long"`. 
7. Nếu không có phạm vi nào trước đó hoạt động, hãy in`"BigInteger"`. 

### Tại sao nó hoạt động 

Các loại số nguyên được sắp xếp theo dung lượng tăng dần. Nếu một số vừa khít bên trong`byte`, đó tự động là loại hợp lệ nhỏ nhất vì mọi loại trước đó đều nhỏ hơn. Logic tương tự áp dụng cho`short`,`int`, Và`long`. 

Thuật toán kiểm tra các giới hạn theo đúng thứ tự này. So sánh thành công đầu tiên xác định loại hợp lệ nhỏ nhất, do đó câu trả lời luôn đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input().strip())

if n <= 127:
    print("byte")
elif n <= 32767:
    print("short")
elif n <= 2147483647:
    print("int")
elif n <= 9223372036854775807:
    print("long")
else:
    print("BigInteger")
```Đầu tiên, chương trình đọc đầu vào và chuyển đổi nó thành số nguyên Python. Điều này an toàn vì số nguyên Python không bị tràn đối với các giá trị lớn. 

Các so sánh được sắp xếp từ loại nhỏ nhất đến loại nguyên thủy lớn nhất. Thứ tự đó quan trọng. Nếu chúng tôi kiểm tra`long`đầu tiên, sau đó mọi giá trị nhỏ hơn cũng sẽ khớp`long`và chúng tôi sẽ mất yêu cầu trả lại loại phụ kiện nhỏ nhất. 

Việc so sánh sử dụng`<=`bởi vì các phạm vi được bao gồm. Các giá trị như`127`Và`2147483647`vẫn phải thuộc về`byte`Và`int`tương ứng. 

trận chung kết`else`xử lý mọi số lớn hơn số nguyên 64 bit có chữ ký tối đa. Những giá trị đó đòi hỏi`BigInteger`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
127
```| Bước | Đã kiểm tra tình trạng | Kết quả | 
| --- | --- | --- | 
| 1 |`127 <= 127`| Đúng | 
| 2 | In`"byte"`| Xong | 

Điều kiện đầu tiên đã thành công nên thuật toán ngay lập tức trả về loại hợp lệ nhỏ nhất. 

### Ví dụ 2 

đầu vào:```
2147483648
```| Bước | Đã kiểm tra tình trạng | Kết quả | 
| --- | --- | --- | 
| 1 |`2147483648 <= 127`| Sai | 
| 2 |`2147483648 <= 32767`| Sai | 
| 3 |`2147483648 <= 2147483647`| Sai | 
| 4 |`2147483648 <= 9223372036854775807`| Đúng | 
| 5 | In`"long"`| Xong | 

Dấu vết này cho thấy tại sao việc xử lý ranh giới lại quan trọng. Giá trị chính xác lớn hơn giá trị tối đa một đơn vị`int`, nên nó phải chuyển sang loại tiếp theo. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(L) | Phân tích số nguyên đầu vào mất thời gian tuyến tính theo số chữ số | 
| Không gian | O(1) | Chỉ có một vài biến được lưu trữ | 

Độ dài đầu vào tối đa là 100 chữ số, do đó, ngay cả việc xử lý tuyến tính cũng rất nhỏ. Giải pháp dễ dàng phù hợp trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    n = int(input().strip())

    if n <= 127:
        print("byte")
    elif n <= 32767:
        print("short")
    elif n <= 2147483647:
        print("int")
    elif n <= 9223372036854775807:
        print("long")
    else:
        print("BigInteger")

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    output = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return output

# provided sample
assert run("127\n") == "byte\n", "sample 1"

# boundary cases
assert run("128\n") == "short\n", "byte to short boundary"
assert run("32767\n") == "short\n", "maximum short"
assert run("32768\n") == "int\n", "short to int boundary"

# large boundaries
assert run("2147483647\n") == "int\n", "maximum int"
assert run("2147483648\n") == "long\n", "int to long boundary"

# beyond long
assert run("9223372036854775808\n") == "BigInteger\n", "requires BigInteger"

# minimum positive input
assert run("1\n") == "byte\n", "smallest valid input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`128`|`short`| Chuyển đổi đúng sau giới hạn byte | 
|`32767`|`short`| Bao gồm ranh giới trên | 
|`32768`|`int`| Lựa chọn loại tiếp theo | 
|`2147483648`|`long`| Vượt qua ranh giới int | 
|`9223372036854775808`|`BigInteger`| Lớn hơn phạm vi 64-bit đã ký | 
|`1`|`byte`| Đầu vào tích cực nhỏ nhất | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
127
```Thuật toán kiểm tra:```
127 <= 127
```Điều này đúng, vì vậy nó in:```
byte
```Điều này xác nhận rằng các ranh giới là bao gồm. Sử dụng một cách nghiêm ngặt`<`so sánh sẽ từ chối không chính xác giá trị này. 

Bây giờ hãy xem xét:```
128
```So sánh đầu tiên thất bại:```
128 <= 127
```Sự so sánh tiếp theo thành công:```
128 <= 32767
```Vì vậy, đầu ra trở thành:```
short
```Điều này xác minh sự chuyển đổi giữa các loại số nguyên liền kề. 

Trường hợp nguy hiểm hơn là:```
9223372036854775808
```Việc so sánh tiếp tục cho đến khi`long`kiểm tra:```
9223372036854775808 <= 9223372036854775807
```Điều này là sai nên thuật toán rơi vào nhánh cuối cùng và in ra:```
BigInteger
```Trường hợp này phát hiện các triển khai dựa vào phân tích cú pháp số nguyên có chiều rộng cố định và vô tình tràn trước khi so sánh.
