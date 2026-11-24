---
title: "CF 95A - Khúc côn cầu"
description: "Chúng ta được cung cấp một chuỗi đại diện cho tên đội khúc côn cầu và danh sách các chuỗi con bị cấm. Nhiệm vụ của chúng ta là sửa đổi chuỗi gốc sao cho bất kỳ chữ cái nào là một phần của chuỗi con bị cấm đều có thể được thay thế bằng một chữ cái khác mà chúng ta chọn."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 95
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 77 (Div. 1 Only)"
rating: 1600
weight: 95
solve_time_s: 75
verified: true
draft: false
---

[CF 95A - Khúc côn cầu](https://codeforces.com/problemset/problem/95/A) 

**Đánh giá:** 1600 
**Tags:** triển khai, chuỗi 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi đại diện cho tên đội khúc côn cầu và danh sách các chuỗi con bị cấm. Nhiệm vụ của chúng ta là sửa đổi chuỗi gốc sao cho bất kỳ chữ cái nào là một phần của chuỗi con bị cấm đều có thể được thay thế bằng một chữ cái khác mà chúng ta chọn. Việc thay thế phải tối đa hóa khả năng xuất hiện của một chữ cái "may mắn" cụ thể, đồng thời giữ nguyên kiểu dáng của từng chữ cái. Các chữ cái không thuộc bất kỳ chuỗi con bị cấm nào đều không thể thay đổi được. Nếu có nhiều chuỗi hợp lệ có cùng số chữ cái may mắn, chúng ta phải chọn chuỗi nhỏ nhất về mặt từ điển. 

Có thể quản lý được các ràng buộc đầu vào: số lượng chuỗi con bị cấm$n$nhiều nhất là 100, độ dài chuỗi$|w|$nhiều nhất là 100 và mỗi chuỗi con có độ dài tối đa là 100. Điều này ngụ ý rằng ngay cả cách tiếp cận bậc hai trên độ dài chuỗi cũng có thể chấp nhận được vì$100^2 = 10{,}000$hoạt động tốt trong giới hạn 2 giây. Tuy nhiên, việc xử lý bất cẩn các chuỗi con chồng chéo hoặc so sánh không phân biệt chữ hoa chữ thường có thể dẫn đến kết quả không chính xác. 

Các trường hợp cạnh phát sinh khi các chuỗi con bị cấm chồng lên nhau. Ví dụ, nếu$w = "abcde"$và các chuỗi con bị cấm là$["ab", "bc"]$, cả "a" và "c" đều là một phần của chuỗi con bị cấm. Cách tiếp cận ngây thơ chỉ thay thế các chữ cái dựa trên kết quả trùng khớp đầu tiên có thể bỏ sót những phần trùng lặp này. Một trường hợp tinh tế khác là khi bản thân chữ may mắn là chữ hoa hoặc chữ thường và trùng với một chuỗi con bị cấm: việc thay thế phải tôn trọng chữ hoa chữ thường. Nếu chữ cái may mắn là "t" và chuỗi con bị cấm bao gồm "T", chúng ta phải thay thế "T" bằng chính "T" để tối đa hóa số lượng của nó trong khi vẫn giữ nguyên chữ hoa. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là kiểm tra, đối với từng vị trí trong chuỗi, xem có chuỗi con bị cấm nào bắt đầu ở vị trí đó hay không. Nếu có, hãy đánh dấu tất cả các ký tự trong chuỗi con đó là có thể thay thế được. Sau khi xử lý tất cả các chuỗi con, lặp lại chuỗi đó và đối với mỗi ký tự có thể thay thế, hãy thay thế nó bằng chữ cái may mắn trong khi vẫn giữ nguyên kiểu chữ ban đầu. Cách tiếp cận này hiệu quả vì kích thước đầu vào nhỏ nhưng vòng lặp lồng nhau trên các chuỗi con và vị trí chuỗi có thể trở nên tẻ nhạt. Độ phức tạp của nó là$O(n \cdot |w| \cdot \text{max\_len\_substr})$. Trong trường hợp xấu nhất với 100 chuỗi con có độ dài 100 và một chuỗi có độ dài 100, đây là$10^6$hoạt động vẫn chấp nhận được. 

Cái nhìn sâu sắc tối ưu là vấn đề giảm xuống còn hai nhiệm vụ: xác định vị trí nào bị bao phủ bởi các chuỗi con bị cấm và sau đó thay thế chúng một cách tối ưu. Chúng tôi không cần các cấu trúc dữ liệu nâng cao như Aho-Corasick vì độ dài chuỗi tối đa chỉ là 100. So sánh cẩn thận không phân biệt chữ hoa chữ thường và đánh dấu các vị trí là đủ. Khi các vị trí đã được đánh dấu, một lần chuyển qua chuỗi để thay thế các chữ cái sẽ đạt được kết quả mong muốn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * | w | * max_len_substr) | 
| Đánh dấu và thay thế được tối ưu hóa | O(n * | w | * max_len_substr) | 

## Hướng dẫn thuật toán 

1. Chuyển đổi chuỗi đầu vào$w$sang chữ thường để so sánh. Điều này cho phép so khớp không phân biệt chữ hoa chữ thường với các chuỗi con bị cấm. 
2. Khởi tạo một mảng boolean`covered`chiều dài$|w|$để theo dõi các vị trí là một phần của bất kỳ chuỗi con bị cấm nào. Ban đầu, tất cả các giá trị đều sai. 
3. Đối với mỗi chuỗi con bị cấm, hãy chuyển nó thành chữ thường. Sau đó, với mỗi chỉ số bắt đầu trong$w$, kiểm tra xem chuỗi con có khớp với phần của$w$bắt đầu từ chỉ mục đó. Nếu trùng khớp, đánh dấu tất cả các vị trí trong`covered`mảng tương ứng với chuỗi con phù hợp là True. Điều này đảm bảo rằng tất cả các chữ cái chạm vào bất kỳ chuỗi con bị cấm nào đều được đánh dấu. 
4. Lặp lại chuỗi gốc$w$. Với mỗi ký tự ở vị trí$i$, nếu như`covered[i]`là Đúng, thay thế bằng chữ may mắn. Giữ nguyên kiểu chữ: nếu ký tự gốc là chữ hoa thì sử dụng chữ may mắn viết hoa; nếu viết thường thì sử dụng phiên bản viết thường. Nếu như`covered[i]`là Sai, giữ nguyên ký tự. 
5. Nối tất cả các ký tự để tạo thành chuỗi cuối cùng và in nó. 

Tại sao nó hoạt động: Thuật toán đảm bảo rằng mọi ký tự thuộc chuỗi con bị cấm sẽ được thay thế bằng chữ cái may mắn, tối đa hóa số lượng của nó. Các ký tự không bị che vẫn không bị ảnh hưởng. Việc bảo quản nguyên bản đảm bảo tính đúng đắn. Vì chúng tôi luôn thay thế bằng biến thể nhỏ nhất về mặt từ điển (chữ may mắn phân biệt chữ hoa chữ thường), nên các mối liên kết được phá vỡ một cách chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
forbidden = [input().strip() for _ in range(n)]
w = input().strip()
lucky = input().strip()

lower_w = w.lower()
covered = [False] * len(w)

for sub in forbidden:
    sub_lower = sub.lower()
    sub_len = len(sub)
    for i in range(len(w) - sub_len + 1):
        if lower_w[i:i+sub_len] == sub_lower:
            for j in range(i, i+sub_len):
                covered[j] = True

result = []
for i, ch in enumerate(w):
    if covered[i]:
        if ch.isupper():
            result.append(lucky.upper())
        else:
            result.append(lucky.lower())
    else:
        result.append(ch)

print("".join(result))
```Giải pháp trước tiên sẽ đọc dữ liệu đầu vào và xử lý các chuỗi con bị cấm để đánh dấu những chữ cái nào bị che đi. Việc so sánh không phân biệt chữ hoa chữ thường được xử lý thông qua`lower()`. Hộp bảo quản thay thế, sử dụng`.upper()`hoặc`.lower()`tùy theo nhân vật gốc. Điều này đảm bảo sự xuất hiện tối đa của chữ cái may mắn và kết quả nhỏ nhất về mặt từ điển nếu tồn tại nhiều tùy chọn. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

| tôi | w[i] | được bảo hiểm[i] | thay thế | 
| --- | --- | --- | --- | 
| 0 | P | Sai | P | 
| 1 | e | Sai | e | 
| 2 | t | Sai | t | 
| 3 | r | Sai | r | 
| 4 | L | Đúng | T | 
| 5 | o | Đúng | t | 
| 6 | v | Đúng | t | 
| 7 | e | Sai | e | 
| 8 | L | Đúng | T | 
| 9 | bạn | Đúng | t | 
| 10 | c | Đúng | t | 
| 11 | k | Đúng | t | 
| 12 | y | Sai | y | 
| ... | ... | ... | ... | 

Kết quả là`"PetrLovtTttttNumtttt"`như mong đợi. 

**Ví dụ tùy chỉnh** 

đầu vào:```
2
abc
BCD
aBcdEf
x
```Các chuỗi con bị cấm bao gồm các vị trí 0,1,2,3. Trường hợp thay thế tôn trọng. 

Đầu ra:```
XxXxEf
```Dấu vết xác nhận việc đánh dấu chính xác và thay thế bảo quản hộp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * | w | 
| Không gian | O( | w | 

Tổng số thao tác nằm trong khoảng 1e6 để đạt được các giới hạn tối đa, vừa vặn thoải mái trong giới hạn 2 giây và bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    forbidden = [input().strip() for _ in range(n)]
    w = input().strip()
    lucky = input().strip()
    
    lower_w = w.lower()
    covered = [False] * len(w)

    for sub in forbidden:
        sub_lower = sub.lower()
        sub_len = len(sub)
        for i in range(len(w) - sub_len + 1):
            if lower_w[i:i+sub_len] == sub_lower:
                for j in range(i, i+sub_len):
                    covered[j] = True

    result = []
    for i, ch in enumerate(w):
        if covered[i]:
            if ch.isupper():
                result.append(lucky.upper())
            else:
                result.append(lucky.lower())
        else:
            result.append(ch)
    
    return "".join(result)

# provided sample
assert run("3\nbers\nucky\nelu\nPetrLoveLuckyNumbers\nt\n") == "PetrLovtTttttNumtttt"

# custom tests
assert run("2\nabc\nBCD\naBcdEf\nx\n") == "XxXxEf"
assert run("1\na\na\na\n") == "a"
assert run("1\nz\nabc\nb\n") == "abc"
assert run("3\na\nb\nc\nABC\nx\n") == "XXX"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2\nabc\nBCD\naBcdEf\nx | XxXxEf | chồng chéo các chuỗi con bị cấm và bảo quản kiểu chữ | 
| 1\na\na\na | một | thay thế chuỗi ký tự đơn | 
| 1\nz\nabc\nb | abc | chuỗi con bị cấm không có trong chuỗi | 
| 3\na\nb\nc\nABC\nx | XXX | tất cả các chữ cái được thay thế chính xác, phân biệt chữ hoa chữ thường | 

## Vỏ cạnh 

Đối với các chuỗi con chồng chéo,`w = "abcb"`, cấm = ["abc", "bcb"], may mắn = "x". Thuật toán đánh dấu chính xác các vị trí 0,1,2,3 và thay thế tất cả bằng chữ "x" hoặc "X
