---
title: "CF 2000C - Mẫu chuỗi số"
description: "Vấn đề yêu cầu chúng ta kiểm tra xem một chuỗi có khớp với mẫu số hay không. Mẫu là một mảng các số nguyên trong đó cùng một số phải tương ứng với cùng một chữ cái trong một chuỗi và cùng một chữ cái sẽ tương ứng với cùng một số."
date: "2026-06-08T14:11:26+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "strings"]
categories: ["algorithms"]
codeforces_contest: 2000
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 966 (Div. 3)"
rating: 1000
weight: 2000
solve_time_s: 156
verified: true
draft: false
---

[CF 2000C - Mẫu chuỗi số](https://codeforces.com/problemset/problem/2000/C) 

**Đánh giá:** 1000 
**Thẻ:** cấu trúc dữ liệu, chuỗi 
**Thời gian giải:** 2 phút 36 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Vấn đề yêu cầu chúng ta kiểm tra xem một chuỗi có khớp với mẫu số hay không. Mẫu là một mảng các số nguyên trong đó cùng một số phải tương ứng với cùng một chữ cái trong một chuỗi và cùng một chữ cái sẽ tương ứng với cùng một số. Nói cách khác, phải có ánh xạ một-một giữa các phần tử riêng biệt của mảng và các chữ cái riêng biệt của chuỗi. Mỗi trường hợp thử nghiệm cung cấp một mẫu số và một tập hợp các chuỗi và chúng ta cần xác định xem mỗi chuỗi có thỏa mãn sự tương ứng này hay không. 

Các ràng buộc cho chúng ta biết rằng tổng độ dài mảng và tổng độ dài chuỗi trên tất cả các trường hợp thử nghiệm nhiều nhất là 200.000. Điều này ngụ ý rằng chúng ta cần một giải pháp xử lý từng phần tử hoặc ký tự theo thời gian tuyến tính; bất kỳ giải pháp nào có độ phức tạp bậc hai về độ dài chuỗi sẽ quá chậm. Các trường hợp cạnh bao gồm các mảng có số lặp lại, chuỗi có các chữ cái lặp lại, mảng một phần tử hoặc mảng có số âm. Một cách tiếp cận ngây thơ có thể bỏ qua sự mâu thuẫn trong đó hai chữ cái ánh xạ tới cùng một số hoặc hai số ánh xạ tới cùng một chữ cái. 

Ví dụ, đối với một mảng`[3,5,2,1,3]`và chuỗi`"abfda"`,`3`xuất hiện hai lần và ánh xạ tới`'a'`cả hai lần và tất cả các số khác đều ánh xạ tới các chữ cái riêng biệt để nó khớp với nhau. Nhưng`"afbfa"`thất bại vì`'f'`xuất hiện ở các vị trí tương ứng với cả hai`5`Và`1`, phá vỡ ánh xạ một-một. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là thử tất cả các ánh xạ có thể có giữa các số và chữ cái cho mỗi chuỗi, nhưng điều này rõ ràng là không khả thi vì số lượng hoán vị của ánh xạ tăng theo cấp số nhân với số lượng số hoặc chữ cái riêng biệt. Cái nhìn sâu sắc quan trọng là chúng ta không cần thử ánh xạ; chúng ta chỉ cần kiểm tra tính nhất quán. Chúng ta có thể duy trì hai từ điển: một từ điển ánh xạ số thành chữ cái và một từ điển ánh xạ chữ cái thành số. Trong khi lặp lại đồng thời qua mẫu và chuỗi, chúng tôi kiểm tra xem mọi ánh xạ có nhất quán với các ánh xạ trước đó hay không. Nếu chúng tôi gặp phải sự không khớp thì chuỗi đó không khớp với mẫu. Nếu chúng ta đi đến cuối cùng mà không có sự mâu thuẫn thì nó sẽ phù hợp. Điều này hoạt động theo thời gian tuyến tính tương ứng với độ dài của chuỗi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k!) Trong đó k = số lượng các số phân biệt | O(k) | Quá chậm | 
| Kiểm tra bản đồ | O(n) mỗi chuỗi | O(n) mỗi chuỗi | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Với mỗi test case, hãy đọc`n`và mảng`a`. 
3. Đọc số chuỗi`m`. 
4. Đối với mỗi chuỗi`s`:

Một. Nếu chiều dài của`s`không khớp`n`, xuất ngay`"NO"`. 

b. Khởi tạo hai từ điển trống:`num_to_char`để ánh xạ số thành chữ cái và`char_to_num`từ chữ sang số. 

c. Lặp lại từng chỉ mục`i`từ`0`ĐẾN`n-1`. Đối với số hiện tại`a[i]`và tính cách`s[i]`:

Tôi. Nếu như`a[i]`đã có bản đồ trong`num_to_char`, kiểm tra xem nó có khớp không`s[i]`. Nếu không thì xuất`"NO"`và ngừng kiểm tra chuỗi này. 

ii. Nếu như`s[i]`đã có bản đồ trong`char_to_num`, kiểm tra xem nó có khớp không`a[i]`. Nếu không thì xuất`"NO"`và dừng lại. 

iii. Nếu cả hai ánh xạ đều nhất quán hoặc mới, hãy cập nhật từ điển. 

d. Nếu vòng lặp hoàn thành mà không có sự mâu thuẫn, xuất ra`"YES"`. 

**Tại sao nó hoạt động**: Từ điển duy trì tính bất biến là mỗi số ánh xạ tới chính xác một chữ cái và mỗi chữ cái ánh xạ tới chính xác một số. Bất kỳ vi phạm nào đối với bất biến này sẽ ngay lập tức xác định sự không khớp, đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    m = int(input())
    for _ in range(m):
        s = input().strip()
        if len(s) != n:
            print("NO")
            continue
        
        num_to_char = {}
        char_to_num = {}
        valid = True
        for ai, ci in zip(a, s):
            if ai in num_to_char and num_to_char[ai] != ci:
                valid = False
                break
            if ci in char_to_num and char_to_num[ci] != ai:
                valid = False
                break
            num_to_char[ai] = ci
            char_to_num[ci] = ai
        print("YES" if valid else "NO")
```Giải pháp đọc đầu vào hiệu quả bằng cách sử dụng`sys.stdin.readline`và lặp qua từng chuỗi, kiểm tra tính nhất quán của ánh xạ bằng hai từ điển. Việc sử dụng`zip(a, s)`đảm bảo rằng chúng ta so sánh trực tiếp các vị trí tương ứng. Việc sử dụng từ điển sẽ tránh việc quét lặp lại, duy trì độ phức tạp tuyến tính trên mỗi chuỗi. 

## Ví dụ đã hoạt động 

**Ví dụ 1** 

đầu vào: 

Mảng`a = [3,5,2,1,3]`, sợi dây`s = "abfda"`| Chỉ mục | một [tôi] | s[i] | num_to_char | char_to_num | Có hiệu lực? | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 3 | một | {3: a} | {a: 3} | Có | 
| 1 | 5 | b | {3: a,5: b} | {a:3,b:5} | Có | 
| 2 | 2 | f | {3:a,5:b,2:f} | {a:3,b:5,f:2} | Có | 
| 3 | 1 | d | {3:a,5:b,2:f,1:d} | {a:3,b:5,f:2,d:1} | Có | 
| 4 | 3 | một | nhất quán | nhất quán | Có | 

Đầu ra:`"YES"`**Ví dụ 2** 

Đầu vào: Mảng`[3,5,2,1,3]`, sợi dây`"afbfa"`Tại chỉ số 2,`s[2] = f`tương ứng với`a[2]=2`, nhưng sau đó`f`tại một vị trí tương ứng với một số khác, vi phạm ánh xạ, vì vậy`"NO"`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(tổng n + tổng độ dài của tất cả các chuỗi) | Mỗi phần tử của mảng và mỗi ký tự của mỗi chuỗi được xử lý một lần. | 
| Không gian | O(n) | Với mỗi chuỗi, từ điển có thể chứa tới`n`mục nhập. | 

Với những hạn chế, giải pháp này hiệu quả và sẽ hoạt động tốt trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        m = int(input())
        for _ in range(m):
            s = input().strip()
            if len(s) != n:
                output.append("NO")
                continue
            num_to_char = {}
            char_to_num = {}
            valid = True
            for ai, ci in zip(a, s):
                if ai in num_to_char and num_to_char[ai] != ci:
                    valid = False
                    break
                if ci in char_to_num and char_to_num[ci] != ai:
                    valid = False
                    break
                num_to_char[ai] = ci
                char_to_num[ci] = ai
            output.append("YES" if valid else "NO")
    return "\n".join(output)

# Provided sample
assert run("""3
5
3 5 2 1 3
2
abfda
afbfa
2
1 2
3
ab
abc
aa
4
5 -3 5 -3
4
aaaa
bcbc
aba
cbcb
""") == """YES
NO
YES
NO
NO
NO
YES
NO
YES""", "sample 1"

# Custom cases
assert run("""1
3
1 1 2
2
aab
aba
""") == """YES
NO""", "duplicate mapping check"

assert run("""1
4
1 2 1 2
2
abab
aabb
""") == """YES
NO""", "two-to-two mapping"

assert run("""1
1
100
1
x
""") == "YES", "single element mapping"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| [1,1,2], ["aab","aba"] | CÓ, KHÔNG | Đảm bảo bản đồ số trùng lặp một cách nhất quán | 
| [1,2,1,2], ["abab","aabb"] | CÓ, KHÔNG | Kiểm tra ánh xạ mẫu xen kẽ | 
| [100], ["x"] | CÓ | Xử lý mảng một phần tử một cách chính xác | 

## Vỏ cạnh 

Đối với các mảng có một phần tử, bất kỳ chuỗi ký tự đơn nào cũng khớp, tạo ra`"YES"`. Đối với các mảng có độ dài 1 nhưng chuỗi dài hơn 1, đầu ra là`"NO"`. Đối với các chuỗi trong đó một ký tự được ánh xạ tới nhiều số, mã sẽ phát hiện sự không nhất quán ngay lập tức và xuất ra`"NO"`. Số âm và số lớn được xử lý giống như bất kỳ số nguyên nào vì từ điển Python xử lý các khóa tùy ý một cách hiệu quả. Giải pháp xử lý chính xác tất cả các trường hợp khó khăn như vậy.
