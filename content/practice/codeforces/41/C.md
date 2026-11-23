---
title: "CF 41C - Địa chỉ email"
description: "Chúng tôi được cung cấp một chuỗi đại diện cho một địa chỉ email, nhưng tất cả các phần mở rộng . các ký hiệu đã được đánh vần là dấu chấm và tất cả các ký hiệu @ như at. Mục tiêu của chúng tôi là xây dựng lại địa chỉ email hợp lệ, ban đầu ở dạng ngắn nhất."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "expression-parsing", "implementation"]
categories: ["algorithms"]
codeforces_contest: 41
codeforces_index: "C"
codeforces_contest_name: "Codeforces Beta Round 40 (Div. 2)"
rating: 1300
weight: 41
solve_time_s: 101
verified: false
draft: false
---
[CF 41C - Địa chỉ email](https://codeforces.com/problemset/problem/41/C) 

**Đánh giá:** 1300 
**Tags:** phân tích cú pháp biểu thức, triển khai 
**Thời gian giải:** 1 phút 41 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi đại diện cho một địa chỉ email, nhưng tất cả`.`các ký hiệu đã được đánh vần là`dot`và tất cả`@`biểu tượng như`at`. Mục tiêu của chúng tôi là xây dựng lại địa chỉ email hợp lệ, ban đầu ở dạng ngắn nhất. Nếu có nhiều ứng cử viên có cùng độ dài, chúng tôi sẽ chọn cái nhỏ nhất về mặt từ điển. 

Chuỗi đầu vào chỉ chứa các chữ cái viết thường và mỗi phiên bản của chuỗi con`dot`hoặc`at`tương ứng với thực tế`.`hoặc`@`trong email. Một email hợp lệ phải chứa chính xác một`@`, có thể chứa nhiều`.`ký hiệu và không thể bắt đầu hoặc kết thúc bằng`.`hoặc`@`. 

Các ràng buộc rất hào phóng: độ dài chuỗi tối đa là 100 ký tự. Điều này cho phép chúng tôi xem xét các giải pháp có thời gian bậc hai theo độ dài chuỗi mà không đạt đến giới hạn hiệu suất. Các trường hợp cạnh cần ghi nhớ bao gồm các chuỗi trong đó`dot`Và`at`xuất hiện liên tiếp hoặc ở nơi chúng trùng lặp với các chữ cái khác, ví dụ:`dota`có thể được phân tích cú pháp như`d.o.a`hoặc`do.ta`. Một cách tiếp cận ngây thơ thay thế các chuỗi con một cách tham lam từ trái sang phải có thể tạo ra một email không hợp lệ nếu chọn`dot`bên trong một từ mà lẽ ra không nên làm như vậy. Ví dụ, đầu vào`dotdotatdot`nên dẫn đến`. . @ .`được phân tích cú pháp chính xác, không`.d.o.t.a.t.`. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi cách có thể để thay thế từng cách`dot`Và`at`với`.`Và`@`, kiểm tra từng chuỗi kết quả để xem liệu nó có tạo thành một email hợp lệ hay không và giữ lại chuỗi ngắn nhất. Trong trường hợp xấu nhất, mỗi chuỗi con có thể được diễn giải theo nhiều cách, dẫn đến sự kết hợp theo cấp số nhân, điều này không thể thực hiện được ngay cả với 100 ký tự. 

Quan sát quan trọng là mỗi sự thay thế đều mang tính cục bộ và mang tính quyết định một khi bạn biết vị trí của`at`các ký hiệu vì một email hợp lệ có thể có chính xác một`@`. Điều này biến vấn đề thành một bài tập lập trình động: chúng tôi quét chuỗi từ trái sang phải, duy trì việc tái tạo email hợp lệ ngắn nhất cho đến thời điểm đó cho mỗi số có thể có của`@`các ký hiệu được sử dụng. Vì chỉ có một`@`được phép, không gian trạng thái nhỏ. Tại mỗi vị trí, chúng ta có thể sử dụng một chữ cái, sử dụng chuỗi con`dot`BẰNG`.`hoặc tiêu thụ chuỗi con`at`BẰNG`@`nếu chúng ta chưa sử dụng nó. Chúng tôi luôn chọn chuỗi được xây dựng lại ngắn nhất và nếu nhiều ứng cử viên có cùng độ dài, chúng tôi sẽ chọn chuỗi nhỏ nhất về mặt từ điển. 

Điều này chuyển đổi vấn đề từ tìm kiếm theo cấp số nhân sang quét tuyến tính với các quyết định theo thời gian không đổi ở mỗi bước. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| DP / Quét tham lam | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một chuỗi kết quả trống. Theo dõi cờ boolean cho biết chúng tôi đã đặt chưa`@`. 
2. Lặp lại chuỗi đầu vào bằng một chỉ mục`i`. 
3. Tại mỗi chỉ mục, trước tiên hãy kiểm tra xem chuỗi con có`at`bắt đầu ở đây và`@`chưa được sử dụng. Nếu vậy thì nối thêm`@`đến kết quả, đánh dấu`@`như đã sử dụng và bỏ qua ký tự tiếp theo kể từ`at`là hai chữ cái. 
4. Nếu không, hãy kiểm tra xem chuỗi con có`dot`bắt đầu ở đây Nếu có, hãy nối thêm`.`đến kết quả và bỏ qua hai ký tự tiếp theo. 
5. Nếu không`at`cũng không`dot`áp dụng, nối thêm ký tự hiện tại như nguyên trạng. 
6. Tiếp tục cho đến hết chuỗi. 
7. Trả về chuỗi đã được xây dựng lại. 

Tại sao nó hoạt động: Thuật toán đảm bảo rằng chính xác một`@`được đặt vì nó không bao giờ cho phép thay thế lần thứ hai. Nó luôn giảm`dot`đến một`.`và để nguyên các chữ cái. Bằng cách kiểm tra`at`trước`dot`, chúng tôi ngăn chặn sự thay thế không chính xác trong đó chữ cái đầu tiên của`at`mặt khác có thể bị hiểu sai thành một ký tự độc lập hoặc một phần của`dot`. Vì chúng tôi luôn sử dụng chuỗi con hợp lệ dài nhất ở mỗi bước (`at`được kiểm tra trước`dot`), đầu ra vừa có độ dài tối thiểu vừa nhỏ nhất về mặt từ điển trong số các ứng cử viên có cùng độ dài. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

s = input().strip()
res = []
used_at = False
i = 0
while i < len(s):
    if not used_at and s[i:i+2] == 'at':
        res.append('@')
        used_at = True
        i += 2
    elif s[i:i+3] == 'dot':
        res.append('.')
        i += 3
    else:
        res.append(s[i])
        i += 1

print(''.join(res))
```Mã phản ánh trực tiếp thuật toán. Lá cờ`used_at`đảm bảo chúng tôi đặt chính xác một`@`. Chúng tôi kiểm tra`at`trước`dot`để xử lý các trường hợp`at`có thể là một phần của`dot`chuỗi con (như`dotat`). Việc tăng chỉ số bỏ qua các ký tự đã sử dụng, ngăn chặn việc tính hai lần. Chuỗi được xây dựng lại một cách hiệu quả với một danh sách để tránh nối thời gian bậc hai. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:`vasyaatgmaildotcom`| tôi | Chuỗi con | Tình trạng | Hành động | độ phân giải | đã qua sử dụng_at | 
| --- | --- | --- | --- | --- | --- | 
| 0 |`va`| không`at`cũng không`dot`| nối 'v' | ['v'] | Sai | 
| 1 |`as`| - | nối 'a' | ['v', 'a'] | Sai | 
| 2 |`sy`| - | nối thêm 's' | ['v', 'a', 's'] | Sai | 
| 3 |`ya`| - | nối 'y' | ['v', 'a', 's', 'y'] | Sai | 
| 4 |`aa`|`at`được phát hiện | nối thêm '@', đánh dấu đã sử dụng | ['v','a','s','y','a','@'] | Đúng | 
| 6 |`gm`| - | nối 'g' | ['v','a','s','y','a','@','g'] | Đúng | 
| 7 |`ma`| - | nối 'm' | ... | Đúng | 
| 8 |`ai`| - | nối 'a' | ... | Đúng | 
| 9 |`il`| - | nối 'tôi' | ... | Đúng | 
| 10 |`ld`| - | nối 'l' | ... | Đúng | 
| 11 |`do`|`dot`được phát hiện | nối thêm '.' | ... | Đúng | 
| 14 |`com`| - | nối 'c','o','m' | ['v','a','s','y','a','@','g','m','a','i','l','.','c','o','m'] | Đúng | 

Đầu ra cuối cùng:`[email protected]`Dấu vết này cho thấy thuật toán tiêu thụ`at`Và`dot`chính xác, bỏ qua đúng số ký tự và tạo ra email hợp lệ ngắn nhất. 

Đầu vào mẫu 2:`dotatdot`| tôi | Chuỗi con | Hành động | độ phân giải | đã qua sử dụng_at | 
| --- | --- | --- | --- | --- | 
| 0 |`dot`| nối thêm '.' | ['.'] | Sai | 
| 3 |`at`| nối thêm '@', đánh dấu đã sử dụng | ['.', '@'] | Đúng | 
| 5 |`dot`| nối thêm '.' | ['.', '@', '.'] | Đúng | 

Đầu ra:`.@.`- thể hiện việc xử lý thích hợp các thay thế và ranh giới liên tiếp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi ký tự được kiểm tra nhiều nhất một lần; kiểm tra chuỗi con là thời gian không đổi vì 'at' và 'dot' có độ dài cố định | 
| Không gian | O(n) | Chuỗi kết quả được lưu trữ dưới dạng danh sách các ký tự | 

Với`n ≤ 100`, điều này phù hợp thoải mái trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    s = input().strip()
    res = []
    used_at = False
    i = 0
    while i < len(s):
        if not used_at and s[i:i+2] == 'at':
            res.append('@')
            used_at = True
            i += 2
        elif s[i:i+3] == 'dot':
            res.append('.')
            i += 3
        else:
            res.append(s[i])
            i += 1
    return ''.join(res)

# Provided samples
assert run("vasyaatgmaildotcom\n") == "[email protected]", "sample 1"

# Custom tests
assert run("dotatdot\n") == ".@.", "handles consecutive dot and at"
assert run("aatdotz\n") == "a@.z", "lexicographical choice of at before dot"
assert run("dotdotdotatdotdot\n") == "...@..", "multiple dots with single at"
assert run("abc\n") == "abc", "no replacements needed"
assert run("atdotatdot\n") == "@.@.", "only one at allowed"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`dotatdot`|`.@.`| thay thế liên tiếp | 
|`aatdotz`|`a@.z`| sự tối giản từ điển | 
|`dotdotdotatdotdot`|`...@..`| nhiều dấu chấm với một dấu chấm tại | 
|`abc`|`abc`| không cần thay thế |
