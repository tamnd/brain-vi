---
title: "CF 219A - Chuỗi k"
description: "Chúng ta được cung cấp một chuỗi chữ thường và một số nguyên k. Chúng ta có thể sắp xếp lại các chữ cái theo cách chúng ta muốn. Mục tiêu là xây dựng một chuỗi mới bao gồm chính xác k khối giống hệt nhau được đặt nối tiếp nhau."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 219
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 135 (Div. 2)"
rating: 1000
weight: 219
solve_time_s: 82
verified: true
draft: false
---

[CF 219A - k-String](https://codeforces.com/problemset/problem/219/A) 

**Đánh giá:** 1000 
**Tags:** triển khai, chuỗi 
**Thời gian giải:** 1m 22s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi chữ thường và một số nguyên`k`. Chúng ta có thể sắp xếp lại các chữ cái theo cách chúng ta muốn. Mục tiêu là xây dựng một chuỗi mới bao gồm chính xác`k`các khối giống hệt nhau được đặt nối tiếp nhau. 

Ví dụ, nếu`k = 3`, thì chuỗi cuối cùng phải có dạng:```
t + t + t
```trong đó cả ba bản sao đều giống nhau`t`. 

Điều duy nhất chúng ta được phép thay đổi là thứ tự các ký tự. Mọi ký tự trong chuỗi gốc vẫn phải xuất hiện chính xác với số lần như nhau trong kết quả cuối cùng. 

Những hạn chế là rất nhỏ. Độ dài chuỗi tối đa là`1000`, và chỉ có`26`chữ cái tiếng Anh viết thường. Điều này ngay lập tức cho chúng ta biết rằng chúng ta không cần các thuật toán nâng cao. Thậm chí một`O(n^2)`giải pháp sẽ dễ dàng vượt qua. Điều quan trọng ở đây là hiểu được điều kiện toán học để có thể xây dựng được giá trị. 

Quan sát quan trọng đến từ việc đếm tần số. Giả sử khối lặp lại cuối cùng là`t`, và chúng tôi lặp lại nó`k`lần. Khi đó, mỗi số ký tự trong toàn bộ chuỗi phải chia hết cho`k`. 

Ví dụ:```
k = 3
string = aaabbbccc
```Mỗi chữ xuất hiện`3`lần, vì vậy chúng ta có thể tạo:```
abcabcabc
```Nhưng nếu chúng ta có:```
k = 2
string = aaabc
```nhân vật`'a'`xuất hiện`3`lần, không thể chia đều cho hai khối giống hệt nhau. Không có sự sắp xếp lại có thể khắc phục điều này. 

Có một số trường hợp đặc biệt thường phá vỡ việc triển khai không chính xác. 

Coi như:```
k = 4
s = aaaa
```Câu trả lời đúng là:```
aaaa
```Việc triển khai bất cẩn có thể cố gắng xây dựng khối cơ sở có chiều dài lớn hơn`1`, mặc dù khối hợp lệ nhỏ nhất chỉ là`"a"`. 

Một trường hợp phức tạp khác là:```
k = 3
s = aabbbb
```Các tần số là:```
a -> 2
b -> 4
```Cả hai số đều không chia hết cho`3`, vậy đáp án phải là`-1`. Một cách tiếp cận đơn giản chỉ kiểm tra xem tổng chiều dài có chia hết cho`k`sẽ nghĩ sai một giải pháp tồn tại bởi vì`6 % 3 == 0`. 

Một trường hợp tế nhị hơn là:```
k = 1
s = codeforces
```Mỗi chuỗi tự động là một`1`-sợi dây. Bản thân chuỗi gốc đã hợp lệ và mọi sự sắp xếp lại đều có tác dụng. Việc triển khai giả định mọi tần suất phải xuất hiện nhiều lần có thể vô tình từ chối trường hợp này. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là tạo ra các hoán vị của chuỗi và kiểm tra xem bất kỳ hoán vị nào có hợp lệ hay không`k`-sợi dây. 

Để kiểm tra hoán vị ứng cử viên, chúng ta có thể chia nó thành`k`phần bằng nhau và kiểm tra xem tất cả các phần có giống nhau không. 

Cách tiếp cận này đúng về mặt logic vì cuối cùng nó sẽ thử mọi cách sắp xếp có thể. Vấn đề là số lượng hoán vị. Một chuỗi có độ dài`1000`có tới`1000!`hoán vị, lớn đến mức không thể tưởng tượng được. Ngay cả đối với chiều dài`15`, vũ lực trở nên hoàn toàn không thực tế. 

Lý do khiến brute Force cảm thấy lãng phí là vì hầu hết các hoán vị chỉ khác nhau về thứ tự ký tự, trong khi điều kiện hợp lệ thực tế phụ thuộc hoàn toàn vào tần số ký tự. 

Quan sát đó dẫn đến cách tiếp cận tối ưu. 

Giả sử có nhân vật nào đó xuất hiện`cnt`lần. Trong một hợp lệ`k`-string, mỗi khối lặp lại phải chứa cùng số ký tự đó. Điều này chỉ có thể thực hiện được nếu`cnt`chia hết cho`k`. 

Khi mọi tần số đều chia hết cho`k`, việc xây dựng trở nên dễ dàng. Với mỗi ký tự, chúng ta đặt`cnt / k`sao chép vào một khối cơ sở. Lặp lại khối đó`k`lần tự động tạo lại tần số ban đầu. 

Ví dụ:```
k = 2
s = aazz
```Các tần số là:```
a -> 2
z -> 2
```Mỗi số đều chia hết cho`2`. 

Vì vậy, khối cơ sở trở thành:```
az
```Lặp lại nó hai lần sẽ cho:```
azaz
```Vấn đề giảm từ việc tìm kiếm giữa các hoán vị sang kiểm tra tính phân chia của tần số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! × n) | O(n) | Quá chậm | 
| Tối ưu | O(n + 26) | O(26) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`k`và chuỗi`s`. 
2. Đếm tần số của mỗi chữ cái viết thường. 
3. Kiểm tra xem mọi tần số có chia hết cho không`k`. 

Nếu một số ký tự không chia hết cho`k`, in`-1`và dừng lại. 

Điều kiện này là cần thiết vì các khối lặp lại giống hệt nhau phải phân bổ đều mọi ký tự. 
4. Xây dựng chuỗi khối cơ sở. 

Đối với mỗi ký tự, hãy thêm`frequency / k`bản sao của ký tự đó vào khối. 
5. Lặp lại chính xác khối cơ sở`k`lần. 
6. In chuỗi kết quả. 

### Tại sao nó hoạt động 

Thuật toán dựa trên một thuộc tính: trong một giá trị hợp lệ`k`-string, mỗi tần số ký tự phải được chia đều cho tất cả`k`bản sao. 

Nếu một nhân vật xuất hiện`cnt`tổng cộng lần thì mỗi khối phải chứa chính xác`cnt / k`bản sao. Điều này chỉ có thể thực hiện được khi`cnt`chia hết cho`k`. 

Khi tất cả các tần số đều thỏa mãn điều kiện này, việc xây dựng khối cơ sở sử dụng`cnt / k`bản sao đảm bảo rằng việc lặp lại khối`k`lần khôi phục lại số lượng ban đầu một cách chính xác. Vì tất cả các khối lặp lại đều giống hệt nhau nên chuỗi cuối cùng theo định nghĩa là hợp lệ`k`-sợi dây. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

k = int(input())
s = input().strip()

freq = [0] * 26

for ch in s:
    freq[ord(ch) - ord('a')] += 1

base = []

for i in range(26):
    if freq[i] % k != 0:
        print(-1)
        sys.exit()

    base.append(chr(i + ord('a')) * (freq[i] // k))

base_string = "".join(base)

print(base_string * k)
```Phần đầu tiên của mã đếm tần số ký tự bằng cách sử dụng một mảng có kích thước cố định`26`. Vì bảng chữ cái được giới hạn ở các chữ cái tiếng Anh viết thường nên việc này đơn giản và nhanh hơn so với việc sử dụng từ điển. 

Kiểm tra tính chia hết là điều kiện chính xác cốt lõi. Thời điểm chúng ta tìm thấy tần số không chia hết cho`k`, ta biết không có nghiệm nào tồn tại nên chương trình in ngay`-1`và thoát ra. 

Bước thi công tuy nhỏ gọn nhưng quan trọng. Nếu một chữ cái xuất hiện`cnt`tổng cộng lần thì mỗi khối lặp lại phải chứa chính xác`cnt // k`bản sao. Nối nhiều ký tự đó vào`base`xây dựng một khối hợp lệ. 

Cuối cùng, lặp lại khối cơ sở`k`lần tái tạo lại tổng tần số ban đầu trong khi vẫn đảm bảo tất cả các phần đều giống hệt nhau. 

Một chi tiết triển khai tinh tế là chỉ sử dụng phép chia số nguyên sau khi xác nhận tính chia hết. Nếu không, việc cắt bớt có thể âm thầm tạo ra số lượng ký tự sai. 

Một chi tiết nhỏ nữa là`strip()`khi đọc chuỗi. Nếu không có nó, ký tự dòng mới sẽ vô tình trở thành một phần của đầu vào. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
aazz
```Bảng tần số: 

| Nhân vật | Tần số | Chia hết cho 2? | Bản sao trong cơ sở | 
| --- | --- | --- | --- | 
| một | 2 | Có | 1 | 
| z | 2 | Có | 1 | 

Cấu trúc chuỗi cơ sở: 

| Bước | Chuỗi cơ sở | 
| --- | --- | 
| thêm 'a' | một | 
| thêm 'z' | az | 

Kết quả cuối cùng: 

| Căn cứ | k | Đầu ra | 
| --- | --- | --- | 
| az | 2 | azaz | 

Ví dụ này cho thấy trường hợp thành công tiêu chuẩn. Mỗi tần số được chia đều trên các khối lặp lại. 

### Ví dụ 2 

đầu vào:```
3
aaabbbb
```Bảng tần số: 

| Nhân vật | Tần số | Chia hết cho 3? | 
| --- | --- | --- | 
| một | 3 | Có | 
| b | 4 | Không | 

Từ`'b'`không chia hết cho`3`, thuật toán dừng ngay lập tức. 

Đầu ra:```
-1
```Ví dụ này thể hiện điều kiện không thể thực hiện được. Việc sắp xếp lại các ký tự không thể thay đổi tần số của chúng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + 26) | Một lần đếm ký tự, một lần vượt qua bảng chữ cái | 
| Không gian | O(26) | Mảng tần số có kích thước cố định | 

Kích thước đầu vào tối đa là`1000`, vì vậy giải pháp này thấp hơn nhiều so với thời hạn. Thuật toán chỉ quét chuỗi một lần và thực hiện công việc bổ sung liên tục cho bảng chữ cái. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    k = int(input())
    s = input().strip()

    freq = [0] * 26

    for ch in s:
        freq[ord(ch) - ord('a')] += 1

    base = []

    for i in range(26):
        if freq[i] % k != 0:
            print(-1)
            return

        base.append(chr(i + ord('a')) * (freq[i] // k))

    print("".join(base) * k)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    output = sys.stdout.getvalue().strip()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return output

# provided sample
assert run("2\naazz\n") == "azaz", "sample 1"

# minimum size
assert run("1\na\n") == "a", "single character"

# impossible because frequencies are not divisible
assert run("3\naabbbb\n") == "-1", "non-divisible frequencies"

# all characters equal
assert run("4\naaaa\n") == "aaaa", "all same letters"

# larger valid construction
assert run("3\naaabbbccc\n") == "abcabcabc", "repeated base block"

# boundary style case
assert run("2\nabcabc\n") == "abcabc", "already valid"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\na\n`|`a`| Đầu vào nhỏ nhất có thể | 
|`3\naabbbb\n`|`-1`| Phát hiện sự phân chia tần số không thể | 
|`4\naaaa\n`|`aaaa`| Ký tự lặp lại đơn | 
|`3\naaabbbccc\n`|`abcabcabc`| Xây dựng hợp lệ nhiều ký tự | 
|`2\nabcabc\n`|`abcabc`| Mẫu lặp lại đã hợp lệ | 

## Vỏ cạnh 

Hãy xem xét:```
k = 1
s = codeforces
```Mỗi tần số đều chia hết cho`1`, do đó thuật toán xây dựng toàn bộ chuỗi làm khối cơ sở và lặp lại nó một lần. Đầu ra hợp lệ vì mỗi chuỗi tự động là một`1`-sợi dây. 

Bây giờ hãy xem xét:```
k = 2
s = aaabc
```Các tần số là:```
a -> 3
b -> 1
c -> 1
```Thuật toán ngay lập tức thông báo rằng`3 % 2 != 0`và in`-1`. 

Điều này đúng vì không có sự sắp xếp lại nào có thể chia được ba bản sao của`'a'`đều thành hai khối giống hệt nhau. 

Một trường hợp cạnh quan trọng khác là:```
k = 4
s = aaaa
```Tần số của`'a'`là`4`, vì vậy mỗi khối nhận được một`'a'`. Khối cơ sở trở thành:```
a
```Lặp lại nó bốn lần sẽ cho:```
aaaa
```Điều này xác nhận thuật toán xử lý chính xác các trường hợp khối lặp lại có độ dài`1`. 

Cuối cùng, hãy xem xét:```
k = 2
s = abcabc
```Các tần số đều chia hết cho`2`:```
a -> 2
b -> 2
c -> 2
```Khối cơ sở trở thành`"abc"`, và lặp lại nó hai lần sẽ tạo ra`"abcabc"`. 

Điều này chứng tỏ rằng thuật toán hoạt động ngay cả khi đầu vào đã hợp lệ`k`-sợi dây.
