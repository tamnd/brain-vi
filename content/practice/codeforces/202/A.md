---
title: "CF 202A - LLPS"
description: "Chúng ta được cung cấp một chuỗi chữ thường và phải chọn một số ký tự để tạo thành một dãy con vừa có tính chất palindrome vừa có kích thước từ điển lớn nhất có thể. Một dãy sau không cần phải tiếp giáp nhau."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "bitmasks", "brute-force", "greedy", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 202
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 127 (Div. 2)"
rating: 800
weight: 202
solve_time_s: 66
verified: true
draft: false
---

[CF 202A - LLPS](https://codeforces.com/problemset/problem/202/A) 

**Đánh giá:** 800 
**Tags:** tìm kiếm nhị phân, bitmasks, vũ phu, tham lam, triển khai, chuỗi 
**Thời gian giải:** 1 phút 6s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi chữ thường và phải chọn một số ký tự để tạo thành một dãy con vừa có tính chất palindrome vừa có kích thước từ điển lớn nhất có thể. 

Một dãy sau không cần phải tiếp giáp nhau. Chúng ta có thể bỏ qua các ký tự nhưng các ký tự còn lại phải giữ nguyên thứ tự ban đầu. 

Độ dài chuỗi tối đa là 10, rất nhỏ. Ngay cả các thuật toán hàm mũ cũng có thể thực hiện được về mặt kỹ thuật ở đây. Một tìm kiếm mạnh mẽ trên tất cả các chuỗi con sẽ kiểm tra nhiều nhất$2^{10} = 1024$ứng viên, dễ dàng phù hợp trong giới hạn. 

Phần thú vị của vấn đề không phải là hiệu suất mà là việc nhận ra cấu trúc ẩn bên trong trật tự từ điển. 

Một sai lầm phổ biến là cho rằng các palindrome dài hơn luôn tốt hơn. Thứ tự từ điển không hoạt động theo cách đó. Ví dụ: 

đầu vào:```
radar
```Các chuỗi palindromic có thể có bao gồm`"radar"`Và`"rr"`. 

Mặc dù`"radar"`dài hơn,`"rr"`lớn hơn về mặt từ điển vì ký tự khác nhau đầu tiên là`'r'`so với`'a'`, Và`'r' > 'a'`. 

Một sai lầm dễ mắc phải khác là cố gắng xây dựng dãy con xuôi ngược dài nhất trước rồi mới tối đa hóa thứ tự từ điển. Đây là những mục tiêu khác nhau. 

Ví dụ: 

đầu vào:```
abac
```Dãy con palindromic dài nhất là`"aba"`, nhưng câu trả lời đúng là:```
c
```Về mặt từ điển, bất kỳ chuỗi nào bắt đầu bằng`'c'`đánh bại mọi chuỗi bắt đầu bằng`'a'`. 

Một trường hợp tinh tế hơn xuất hiện khi ký tự lớn nhất xuất hiện nhiều lần. 

đầu vào:```
azbz
```Câu trả lời đúng là:```
zz
```Một giải pháp bất cẩn có thể chỉ mang lại kết quả`"z"`, nhưng nếu ký tự tối đa xuất hiện nhiều lần, việc lấy tất cả chúng sẽ tự động tạo thành một bảng màu và cho kết quả lớn hơn về mặt từ điển vì các chuỗi có tiền tố bằng nhau dài hơn sẽ lớn hơn. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản vì đầu vào quá nhỏ. 

Chúng ta có thể tạo ra mọi chuỗi con bằng cách sử dụng bitmask. Với mỗi dãy con, chúng ta kiểm tra xem nó có phải là một dãy palindrome hay không. Trong số tất cả các dãy con đối xứng, chúng ta giữ dãy con lớn nhất về mặt từ điển. 

Điều này hoạt động vì chỉ có$2^n$các chuỗi tiếp theo. Với$n \le 10$, tức là nhiều nhất là 1024 ứng viên. Ngay cả khi chúng ta dành$O(n)$thời gian xây dựng và kiểm tra từng chuỗi con, tổng công việc vẫn rất nhỏ. 

Phương pháp brute-force hoàn toàn có thể chấp nhận được cho vấn đề này. Tuy nhiên, có một quan sát đơn giản hơn nhiều. 

Giả sử ký tự lớn nhất xuất hiện trong chuỗi là`'z'`. Bất kỳ palindrome nào bắt đầu bằng một ký tự nhỏ hơn không bao giờ có thể đánh bại một palindrome bắt đầu bằng`'z'`, bất kể độ dài. 

Bây giờ hãy xem xét tất cả các lần xuất hiện của ký tự tối đa này. Nếu chúng ta lấy từng chữ một trong số chúng, thì chuỗi kết quả hoàn toàn bao gồm cùng một chữ cái, chữ cái này tự động trở thành một bảng màu. 

Ví dụ:```
abzczz
```Ký tự tối đa là`'z'`, xuất hiện ba lần. Trình tự tiếp theo`"zzz"`là một bảng màu và không có bảng màu nào bắt đầu bằng một ký tự nhỏ hơn có thể đánh bại nó về mặt từ điển. 

Điều này hoàn toàn giải quyết được vấn đề: 

Câu trả lời đơn giản là tất cả các lần xuất hiện của ký tự tối đa trong chuỗi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n \cdot n) | O(n) | Đã chấp nhận | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi đầu vào. 
2. Tìm ký tự tối đa trong chuỗi bằng Python`max()`. 

Ký tự này phải xuất hiện ở đầu bảng màu lớn nhất theo từ điển. Bất kỳ ký tự bắt đầu nhỏ hơn nào sẽ ngay lập tức bị thua khi so sánh từ điển. 
3. Duyệt lại chuỗi và thu thập mọi lần xuất hiện của ký tự tối đa đó. 

Một chuỗi được tạo hoàn toàn bằng một ký tự lặp lại luôn là một chuỗi palindrome. 
4. In chuỗi kết quả. 

### Tại sao nó hoạt động 

Đặt ký tự tối đa trong chuỗi là`c`. 

Bất kỳ dãy con đối xứng nào bắt đầu bằng một ký tự nhỏ hơn`c`về mặt từ điển sẽ tự động nhỏ hơn một dãy con bắt đầu bằng`c`. 

Vì vậy câu trả lời tối ưu phải bắt đầu bằng`c`. 

Khi ký tự đầu tiên được cố định thành`c`, thêm nhiều bản sao của`c`chỉ cải thiện kết quả. Nếu hai chuỗi có chung tiền tố thì chuỗi dài hơn sẽ lớn hơn về mặt từ điển. 

Vì tất cả các bản sao của`c`tạo thành một bảng màu hợp lệ, mỗi lần xuất hiện sẽ tạo ra câu trả lời lớn nhất có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()

mx = max(s)

answer = []

for ch in s:
    if ch == mx:
        answer.append(ch)

print("".join(answer))
```Lời giải trực tiếp theo quan sát đã được chứng minh ở trên.`max(s)`tìm ký tự lớn nhất về mặt từ điển xuất hiện trong chuỗi. 

Vòng lặp thứ hai tập hợp mọi lần xuất hiện của ký tự đó trong khi vẫn giữ nguyên trật tự. Vì tất cả các ký tự được chọn đều giống hệt nhau nên dãy con kết quả sẽ tự động là một bảng màu. 

Một điểm tinh tế là chúng ta phải chấp nhận tất cả các sự kiện xảy ra, không chỉ một sự kiện. Nếu ký tự tối đa xuất hiện nhiều lần thì chuỗi dài hơn của cùng một ký tự lặp lại sẽ lớn hơn về mặt từ điển. 

Ví dụ:```
"zz" > "z"
```bởi vì`"z"`là tiền tố của`"zz"`. 

Một chi tiết khác là bảo toàn thứ tự tiếp theo. Lặp lại từ trái sang phải tự nhiên giữ nguyên thứ tự ban đầu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
radar
```| Vị trí | Nhân vật | Ký tự tối đa | Đã thêm vào câu trả lời | Câu trả lời hiện tại | 
| --- | --- | --- | --- | --- | 
| 0 | r | r | Có | r | 
| 1 | một | r | Không | r | 
| 2 | d | r | Không | r | 
| 3 | một | r | Không | r | 
| 4 | r | r | Có | rr | 

Đầu ra:```
rr
```Ví dụ này cho thấy tại sao thứ tự từ điển lại chiếm ưu thế về độ dài palindrome.`"rr"`nhịp đập`"radar"`bởi vì ký tự khác nhau đầu tiên lớn hơn. 

### Ví dụ 2 

đầu vào:```
azbz
```| Vị trí | Nhân vật | Ký tự tối đa | Đã thêm vào câu trả lời | Câu trả lời hiện tại | 
| --- | --- | --- | --- | --- | 
| 0 | một | z | Không | | 
| 1 | z | z | Có | z | 
| 2 | b | z | Không | z | 
| 3 | z | z | Có | zz | 

Đầu ra:```
zz
```Dấu vết này chứng tỏ tại sao chúng ta phải bao gồm mọi lần xuất hiện của ký tự tối đa.`"zz"`về mặt từ điển lớn hơn`"z"`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lượt để tìm ký tự tối đa và một lượt để thu thập nó | 
| Không gian | O(n) | Câu trả lời có thể chứa tất cả các ký tự của chuỗi | 

Với$n \le 10$, chương trình sẽ chạy ngay lập tức. Ngay cả giải pháp vũ phu cũng có thể dễ dàng phù hợp, nhưng việc quan sát tham lam làm giảm việc thực hiện xuống còn một vài dòng. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    s = input().strip()

    mx = max(s)

    ans = []

    for ch in s:
        if ch == mx:
            ans.append(ch)

    print("".join(ans))

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue().strip()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out

# provided sample
assert run("radar\n") == "rr", "sample 1"

# custom cases
assert run("a\n") == "a", "single character"
assert run("aaaaa\n") == "aaaaa", "all equal characters"
assert run("abac\n") == "c", "largest character appears once"
assert run("azbz\n") == "zz", "largest character appears multiple times"
assert run("zyxwvutsrq\n") == "z", "strictly decreasing characters"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`a`|`a`| Đầu vào kích thước tối thiểu | 
|`aaaaa`|`aaaaa`| Tất cả các ký tự đều bằng nhau | 
|`abac`|`c`| Palindrome dài hơn không phải lúc nào cũng tốt hơn | 
|`azbz`|`zz`| Phải lấy tất cả ký tự tối đa | 
|`zyxwvutsrq`|`z`| Ký tự tối đa xuất hiện một lần | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
abac
```Ký tự tối đa là`'c'`. Thuật toán quét chuỗi và chỉ giữ lại`'c'`, sản xuất:```
c
```Thuật toán palindrome dài nhất sẽ tạo ra`"aba"`, điều này sai vì về mặt từ điển:```
c > aba
```Thuật toán thành công vì nó ưu tiên ký tự đầu tiên hơn tất cả. 

Bây giờ hãy xem xét:```
zzza
```Ký tự tối đa là`'z'`. Thuật toán thu thập mọi`'z'`và kết quả đầu ra:```
zzz
```Việc triển khai bất cẩn chỉ trả về một ký tự tối đa sẽ tạo ra`"z"`, nhỏ hơn vì`"z"`là tiền tố của`"zzz"`. 

Cuối cùng, hãy xem xét:```
bacab
```Ký tự tối đa là`'c'`, xuất hiện một lần Các đầu ra thuật toán:```
c
```Mặc dù`"bacab"`bản thân nó là một bảng màu, nó bắt đầu bằng`'b'`, do đó, nó mất về mặt từ điển đối với bất kỳ bảng màu nào bắt đầu bằng`'c'`.
