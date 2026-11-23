---
title: "CF 41A - Dịch thuật"
description: "Chúng tôi được cung cấp hai chuỗi chữ thường. Chuỗi đầu tiên là một từ trong một ngôn ngữ và chuỗi thứ hai được cho là bản dịch của nó sang ngôn ngữ khác trong đó mọi từ được viết ngược."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 41
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 40 (Div. 2)"
rating: 800
weight: 41
solve_time_s: 74
verified: true
draft: false
---
[CF 41A - Bản dịch](https://codeforces.com/problemset/problem/41/A) 

**Đánh giá:** 800 
**Tags:** triển khai, chuỗi 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp hai chuỗi chữ thường. Chuỗi đầu tiên là một từ trong một ngôn ngữ và chuỗi thứ hai được cho là bản dịch của nó sang ngôn ngữ khác trong đó mọi từ được viết ngược. 

Nhiệm vụ chỉ đơn giản là kiểm tra xem chuỗi thứ hai có chính xác là đảo ngược của chuỗi thứ nhất hay không. Nếu có, chúng tôi in`YES`. Ngược lại, chúng tôi in`NO`. 

Độ dài tối đa của mỗi chuỗi chỉ là 100 ký tự. Với giới hạn nhỏ như vậy, ngay cả những phương pháp tiếp cận kém hiệu quả cũng sẽ được thực hiện thoải mái trong thời gian giới hạn. Giải pháp quét chuỗi một lần là quá đủ. Không cần cấu trúc dữ liệu nâng cao hoặc thủ thuật tối ưu hóa. 

Mối nguy hiểm chính trong vấn đề này không phải là hiệu suất mà là tính chính xác. Việc thực hiện bất cẩn có thể vô tình chấp nhận các trường hợp không hợp lệ. 

Một lỗi dễ mắc phải là quên so sánh chuỗi đảo ngược đầy đủ. Ví dụ:```
abc
ab
```Đầu ra đúng là:```
NO
```Chuỗi thứ hai ngắn hơn nên không thể đảo ngược chuỗi thứ nhất. Bất kỳ triển khai nào chỉ so sánh các vị trí khớp cho đến khi một chuỗi kết thúc sẽ chấp nhận trường hợp này một cách không chính xác. 

Một lỗi phổ biến khác là đảo ngược không chính xác bằng cách so sánh các ký tự theo cùng một hướng. Coi như:```
abc
cba
```Đầu ra đúng là:```
YES
```Nếu chúng ta so sánh`s[i]`với`t[i]`trực tiếp thay vì`t[n - 1 - i]`, chúng tôi sẽ từ chối một bản dịch hợp lệ. 

Trường hợp cạnh cuối cùng là một chuỗi ký tự đơn:```
a
a
```Đầu ra đúng là:```
YES
```Chuỗi một ký tự bằng chuỗi đảo ngược của chính nó nên câu trả lời vẫn phải được chấp nhận. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là xây dựng phiên bản đảo ngược của chuỗi đầu tiên theo cách thủ công. Chúng ta có thể lặp lại từ ký tự cuối cùng của`s`vào đầu tiên và nối từng ký tự vào một chuỗi mới. Sau khi xây dựng chuỗi đảo ngược, chúng ta so sánh nó với`t`. 

Điều này hoạt động vì một từ đảo ngược được xác định duy nhất. Nếu mọi ký tự xuất hiện theo thứ tự ngược lại thì bản dịch là chính xác. 

Hạn chế của phương pháp này là việc nối chuỗi lặp đi lặp lại có thể trở nên kém hiệu quả trong các ngôn ngữ mà chuỗi là bất biến. Mỗi phần bổ sung có thể tạo ra một chuỗi mới, có thể dẫn đến hành vi bậc hai trong trường hợp xấu nhất. Với độ dài tối đa chỉ 100, điều này vẫn dễ dàng vượt qua, nhưng đây không phải là cách triển khai rõ ràng nhất. 

Quan sát quan trọng là sự cố đã khớp trực tiếp với thao tác tích hợp sẵn. Thay vì phải xây dựng lại chuỗi ngược theo cách thủ công, chúng ta có thể đảo ngược chuỗi trong một bước và so sánh kết quả ngay lập tức. 

Trong Python,`s[::-1]`tạo ra chuỗi đảo ngược. Khi chúng tôi tính toán nó, vấn đề sẽ giảm xuống còn một kiểm tra đẳng thức duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Đã chấp nhận | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi đầu tiên`s`. 
2. Đọc chuỗi thứ hai`t`. 
3. Đảo ngược`s`sử dụng cắt lát. 
4. So sánh chuỗi đảo ngược với`t`. 
5. Nếu chúng bằng nhau thì in ra`YES`. Ngược lại, in`NO`. 

Việc so sánh có hiệu quả vì việc đảo ngược sẽ bảo toàn mọi ký tự trong khi chỉ thay đổi thứ tự. Hai chuỗi khớp chính xác khi và chỉ khi bản dịch được thực hiện chính xác. 

### Tại sao nó hoạt động 

Thuật toán kiểm tra trực tiếp thuộc tính xác định của bản dịch. Một từ được dịch hợp lệ phải chứa chính xác các ký tự giống như từ gốc nhưng theo thứ tự ngược lại. 

Nếu như`reverse(s) == t`, mọi vị trí đều khớp với ký tự đảo ngược được yêu cầu, do đó bản dịch là chính xác. 

Nếu thậm chí một ký tự khác nhau hoặc độ dài khác nhau thì việc kiểm tra tính bằng nhau sẽ tự động thất bại, do đó bản dịch không hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()
t = input().strip()

if s[::-1] == t:
    print("YES")
else:
    print("NO")
```Chương trình bắt đầu bằng cách đọc cả hai chuỗi từ đầu vào tiêu chuẩn. sử dụng`.strip()`loại bỏ ký tự dòng mới ở cuối được tạo ra bằng cách đọc đầu vào. 

biểu hiện`s[::-1]`tạo một bản sao đảo ngược của chuỗi. Tính năng cắt Python xử lý việc đảo ngược nội bộ, vì vậy chúng ta không cần vòng lặp thủ công. 

Kiểm tra đẳng thức sẽ tự động so sánh cả độ dài và thứ tự ký tự. Điều này tránh các lỗi tinh vi trong đó các kết quả khớp một phần có thể vượt qua không chính xác. 

Việc thực hiện ngắn gọn vì bản thân vấn đề về cơ bản là đơn giản. Ý tưởng chính là nhận ra rằng việc đảo ngược một chuỗi là thao tác chính xác cần có. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
code
edoc
```| Bước | Giá trị | 
| --- | --- | 
| Chuỗi gốc`s`|`code`| 
| Đảo ngược`s`|`edoc`| 
| Chuỗi mục tiêu`t`|`edoc`| 
| Kết quả so sánh | Bằng | 
| Đầu ra |`YES`| 

Phiên bản đảo ngược của`code`chính xác là`edoc`, vì vậy bản dịch là hợp lệ. 

### Ví dụ 2 

đầu vào:```
hello
ollehh
```| Bước | Giá trị | 
| --- | --- | 
| Chuỗi gốc`s`|`hello`| 
| Đảo ngược`s`|`olleh`| 
| Chuỗi mục tiêu`t`|`ollehh`| 
| Kết quả so sánh | Không bằng | 
| Đầu ra |`NO`| 

Ví dụ này chứng tỏ rằng các tiền tố phù hợp là chưa đủ. Các chuỗi phải khớp hoàn toàn, bao gồm cả độ dài. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Đảo ngược và so sánh cả hai quét chuỗi một lần | 
| Không gian | O(n) | Chuỗi đảo ngược được lưu trữ tạm thời | 

Độ dài chuỗi tối đa chỉ là 100, vì vậy giải pháp này dễ dàng phù hợp với cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    import sys
    input = sys.stdin.readline

    s = input().strip()
    t = input().strip()

    if s[::-1] == t:
        print("YES")
    else:
        print("NO")

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    output = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return output.strip()

# provided sample
assert run("code\nedoc\n") == "YES", "sample 1"

# minimum-size input
assert run("a\na\n") == "YES", "single character"

# different lengths
assert run("abc\nab\n") == "NO", "length mismatch"

# same letters but wrong order
assert run("abcd\nabdc\n") == "NO", "incorrect reversal"

# maximum-size style case
s = "a" * 100
t = "a" * 100
assert run(f"{s}\n{t}\n") == "YES", "maximum equal strings"

# palindrome
assert run("level\nlevel\n") == "YES", "palindrome case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a / a`|`YES`| Đầu vào kích thước tối thiểu | 
|`abc / ab`|`NO`| Xử lý độ dài không khớp | 
|`abcd / abdc`|`NO`| Thứ tự ký tự sai | 
| 100`'a'`nhân vật |`YES`| Độ dài tối đa cho phép | 
|`level / level`|`YES`| Palindrome không thay đổi khi đảo ngược | 

## Vỏ cạnh 

Hãy xem xét trường hợp các chuỗi có độ dài khác nhau:```
abc
ab
```Thuật toán tính toán:```
reverse("abc") = "cba"
```Sau đó nó so sánh`"cba"`với`"ab"`. 

Các chuỗi khác nhau, do đó đầu ra trở thành:```
NO
```Điều này xác nhận rằng thuật toán không vô tình chấp nhận kết quả khớp một phần. 

Bây giờ hãy xem xét một bảng màu:```
level
level
```Thuật toán tính toán:```
reverse("level") = "level"
```Vì chuỗi đảo ngược giống hệt chuỗi gốc nên việc so sánh thành công và kết quả là:```
YES
```Điều này xác minh rằng các chuỗi bằng đảo ngược của chính chúng được xử lý chính xác. 

Cuối cùng, hãy xem xét trường hợp các ký tự khớp nhau nhưng thứ tự không khớp:```
abcd
abdc
```Thuật toán tính toán:```
reverse("abcd") = "dcba"
```So sánh`"dcba"`với`"abdc"`thất bại ngay lập tức, tạo ra:```
NO
```Điều này cho thấy giải pháp kiểm tra thứ tự đảo ngược chính xác chứ không chỉ bộ ký tự.
