---
title: "CF 1674C - Thay thế vô hạn"
description: "Bài toán cung cấp cho bạn hai chuỗi cho mỗi trường hợp kiểm thử: một chuỗi s bao gồm toàn bộ chữ cái 'a' và một chuỗi khác bao gồm các chữ cái viết thường tùy ý. Bạn được phép thay thế nhiều lần bất kỳ chữ 'a' nào trong s bằng chuỗi t."
date: "2026-06-10T01:12:59+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 1674
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 786 (Div. 3)"
rating: 1000
weight: 1674
solve_time_s: 89
verified: true
draft: false
---

[CF 1674C - Thay thế vô hạn](https://codeforces.com/problemset/problem/1674/C) 

**Đánh giá:** 1000 
**Tags:** tổ hợp, triển khai, chuỗi 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán cung cấp cho bạn hai chuỗi cho mỗi trường hợp thử nghiệm: một chuỗi`s`bao gồm toàn bộ chữ cái 'a' và một chuỗi khác`t`gồm các chữ cái viết thường tùy ý. Bạn được phép thay thế nhiều lần bất kỳ chữ 'a' nào trong`s`với chuỗi`t`. Sau bất kỳ số lần thay thế nào như vậy, bạn được yêu cầu đếm xem có thể thu được bao nhiêu chuỗi riêng biệt. Nếu quá trình này có thể tạo ra vô số chuỗi riêng biệt, bạn nên xuất ra -1. 

Những hạn chế là khiêm tốn. Mỗi chuỗi có độ dài tối đa là 50 và có thể có tới 10.000 trường hợp thử nghiệm. Điều này gợi ý rằng chúng ta cần một thuật toán tuyến tính theo kích thước của mỗi chuỗi, bởi vì một giải pháp cố gắng tạo ra tất cả các chuỗi có thể một cách rõ ràng sẽ bùng nổ về mặt tổ hợp, đặc biệt là khi`t`chứa các chữ cái khác ngoài 'a'. 

Có một số trường hợp đặc biệt đòi hỏi phải suy luận cẩn thận. Nếu như`t`chỉ bao gồm 'a' thì mọi sự thay thế đều thực sự là không hoạt động. Thay thế 'a' bằng`t`không thay đổi gì nên không có chuỗi mới nào được tạo ra ngoài chuỗi ban đầu`s`. Ngược lại, nếu`t`chứa ít nhất một chữ cái khác 'a', thì mỗi lần thay thế sẽ đưa ra một chữ cái không phải 'a'. Vì việc thay thế có thể tiếp tục vô thời hạn ở bất cứ nơi nào còn lại 'a, nên số lượng chuỗi có thể trở thành không giới hạn - không có giới hạn trên về độ dài hoặc chủng loại chuỗi. Cuối cùng, nếu`s`chỉ chứa một 'a' và`t`chứa các chữ cái khác 'a', bạn chỉ có thể tạo ra hai chuỗi: chuỗi gốc`s`và chuỗi`t`sau khi thay thế chữ 'a'. 

Việc triển khai ngây thơ cố gắng mô phỏng tất cả các thay thế sẽ thất bại với đầu vào khiêm tốn vì số lượng chuỗi có thể có thể tăng theo cấp số nhân hoặc vô hạn. Nhận biết cấu trúc của`t`do đó là chìa khóa cho một giải pháp đơn giản. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng liệt kê tất cả các trình tự thay thế có thể có. Đối với mỗi 'a' trong`s`, bạn có thể thay thế nó bằng`t`hoặc để nguyên, tạo các chuỗi mới một cách đệ quy cho đến khi không còn 'a'. Mặc dù điều này đúng về mặt khái niệm, nhưng số lượng chuỗi trong trường hợp xấu nhất sẽ theo cấp số nhân theo độ dài`s`. Cụ thể, đối với`n = 50`Và`t`chứa các chữ cái khác ngoài 'a', bạn có thể nhanh chóng vượt quá`2^50`các chuỗi có thể, vượt xa tính toán khả thi. 

Cách tiếp cận tối ưu dựa trên một quan sát đơn giản: số lượng các chuỗi riêng biệt được xác định hoàn toàn bằng việc liệu`t`chỉ chứa 'a' hoặc bất kỳ chữ cái nào khác. Nếu như`t`chứa một chữ cái khác với 'a', việc thay thế có thể tạo ra vô số chuỗi riêng biệt, bởi vì mỗi 'a' mới xuất hiện có thể được thay thế lại, làm chuỗi phát triển vô tận. Ngược lại, nếu`t`chỉ bao gồm 'a', không có sự thay thế nào thay đổi chuỗi, vì vậy chỉ có bản gốc`s`tồn tại dưới dạng một chuỗi riêng biệt. Đối với trường hợp`s`có độ dài 1 và`t`chứa các chữ cái không phải 'a', chỉ tồn tại hai chuỗi riêng biệt: 'a' ban đầu và chuỗi thay thế`t`. 

Điều này làm giảm vấn đề thành một kiểm tra đơn giản: quét`t`cho các chữ cái không phải 'a'. Nếu có, câu trả lời là -1 (vô hạn) trừ khi`s`có độ dài 1, trong trường hợp đó có đúng hai khả năng. Nếu tất cả các chữ cái của`t`là 'a', câu trả lời là 1, vì việc thay thế không tạo ra chuỗi mới. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * n) | O(n * 2^n) | Quá chậm | 
| Tối ưu | O( | t | ) cho mỗi trường hợp thử nghiệm | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`q`. Điều này kiểm soát vòng lặp chính trên tất cả các trường hợp thử nghiệm. 
2. Với mỗi test case, hãy đọc chuỗi`s`Và`t`. Chuỗi`s`sẽ chỉ chứa 'a' và`t`có thể chứa bất kỳ chữ cái viết thường nào. 
3. Kiểm tra xem`t`hoàn toàn bao gồm 'a'. Điều này có thể được thực hiện bằng cách so sánh tập hợp các ký tự trong`t`ĐẾN`{'a'}`. 
4. Nếu`t`chỉ chứa 'a', in 1. Không có sự thay thế nào làm thay đổi chuỗi, do đó chỉ tồn tại chuỗi gốc. 
5. Ngược lại, nếu`t`chứa bất kỳ chữ cái nào ngoài 'a', hãy kiểm tra độ dài của`s`. 
6. Nếu`s`có độ dài 1, in 2. Các chuỗi khác biệt duy nhất là chuỗi 'a' ban đầu và chuỗi thay thế`t`. 
7. Nếu`s`có độ dài lớn hơn 1, in -1. Quá trình thay thế có thể tạo ra vô số chuỗi vì mỗi chuỗi 'a' có thể tạo ra một cách đệ quy`t`chứa các chữ cái không phải 'a'. 

Tại sao nó hoạt động: điều bất biến là quá trình thay thế chỉ đưa ra các chữ cái mới nếu`t`chứa các chữ cái khác ngoài 'a'. Điều này ngay lập tức gây ra một tập hợp nhỏ hữu hạn các chuỗi có thể có hoặc một số lượng vô hạn, chỉ phụ thuộc vào nội dung của`t`và chiều dài của`s`. Không cần phải mô phỏng việc xây dựng chuỗi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

q = int(input())
for _ in range(q):
    s = input().strip()
    t = input().strip()
    
    if set(t) == {'a'}:
        print(1)
    elif len(s) == 1:
        print(2)
    else:
        print(-1)
```Giải pháp đầu tiên là đọc số lượng trường hợp thử nghiệm. Đối với mỗi trường hợp, nó đọc`s`Và`t`. các`set(t) == {'a'}`kiểm tra xác định xem tất cả các ký tự trong`t`khu vực'. Nếu vậy, các phép thay thế không làm thay đổi chuỗi nên chúng ta in ra 1. Nếu`t`chứa các chữ cái khác và`s`chỉ có một 'a', chuỗi có thể là chuỗi gốc hoặc chuỗi thay thế, vì vậy chúng tôi in 2. Nếu không, các thay thế có thể tiếp tục vô thời hạn và chúng tôi in -1. sử dụng`strip()`đảm bảo không có dòng mới nào cản trở việc so sánh chuỗi. 

## Ví dụ đã hoạt động 

Mẫu 1: 

| Bước | s | t | Đặt(t) | len(s) | Đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| 1 | aaa | một | {'a'} | 4 | 1 | 
| 2 | aa | abc | {'a','b','c'} | 2 | -1 | 
| 3 | một | b | {'b'} | 1 | 2 | 

Bảng xác nhận rằng logic phân biệt chính xác giữa các thay thế thuần túy 'a', s một chữ cái và s nhiều chữ cái với các thay thế không phải 'a'. 

Mẫu 2: 

đầu vào:```
2
a
a
aa
b
```| Bước | s | t | Đặt(t) | len(s) | Đầu ra | 
| --- | --- | --- | --- | --- | --- | 
| 1 | một | một | {'a'} | 1 | 1 | 
| 2 | aa | b | {'b'} | 2 | -1 | 

Điều này xác nhận rằng thuật toán xử lý các chuỗi có kích thước tối thiểu và các trường hợp nhiều 'a' một cách chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O( | t | 
| Không gian | O(1) | Chỉ một vài biến và tập hợp các ký tự từ t được lưu trữ cho mỗi trường hợp thử nghiệm. | 

Giải pháp thoải mái trong giới hạn thời gian và bộ nhớ. Tối đa 10^4 * 50 = 5×10^5 kiểm tra ký tự xảy ra, không đáng kể đối với các bộ xử lý hiện đại. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    q = int(input())
    for _ in range(q):
        s = input().strip()
        t = input().strip()
        if set(t) == {'a'}:
            print(1)
        elif len(s) == 1:
            print(2)
        else:
            print(-1)
    return output.getvalue().strip()

# provided samples
assert run("3\naaaa\na\naa\nabc\na\nb\n") == "1\n-1\n2", "sample 1"

# custom cases
assert run("2\na\na\naa\nb\n") == "1\n-1", "minimum and infinite"
assert run("1\na\nb\n") == "2", "single a, non-a t"
assert run("1\naaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa\na\n") == "1", "max length s, t all a"
assert run("1\naaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa\nb\n") == "-1", "max length s, t non-a"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2\na\na\naa\nb | 1\n-1 | chuỗi kích thước tối thiểu và tạo vô hạn | 
| 1\na\nb | 2 | đơn 'a' với sự thay thế không phải 'a' | 
| 1\na...a\na | 1 | độ dài tối đa s, t tất cả 'a' | 
| 1\na...a\nb | -1 | độ dài tối đa s, t chứa non-'a' | 

## Vỏ cạnh 

Nếu`s = "a"`Và`t = "b"`, thuật toán in ra 2. Dấu vết bước:`set(t) != {'a'}`,`len(s) == 1`,
