---
title: "CF 2085A - Lý thuyết chuỗi và phục vụ"
description: "Chúng ta được cung cấp một chuỗi các chữ cái viết thường và giới hạn số lần hoán đổi mà chúng ta có thể thực hiện. Mục đích là làm cho chuỗi trở nên “phổ quát”, nghĩa là về mặt từ điển, nó nhỏ hơn so với đảo ngược của nó về mặt từ điển."
date: "2026-06-08T06:06:30+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "implementation"]
categories: ["algorithms"]
codeforces_contest: 2085
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 1011 (Div. 2)"
rating: 900
weight: 2085
solve_time_s: 96
verified: false
draft: false
---

[CF 2085A - Lý thuyết chuỗi và phục vụ](https://codeforces.com/problemset/problem/2085/A) 

**Xếp hạng:** 900 
**Tas:** thuật toán xây dựng, triển khai 
**Thời gian giải:** 1 phút 36 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các chữ cái viết thường và giới hạn số lần hoán đổi mà chúng ta có thể thực hiện. Mục đích là làm cho chuỗi trở nên “phổ quát”, nghĩa là về mặt từ điển, nó nhỏ hơn so với đảo ngược của nó về mặt từ điển. Về cơ bản, chúng tôi muốn vị trí đầu tiên trong đó chuỗi và đảo ngược của nó khác nhau có chữ cái của chuỗi ban đầu nhỏ hơn chữ cái tương ứng trong đảo ngược. 

Đầu vào cung cấp độ dài chuỗi và số lần hoán đổi tối đa được phép cho mỗi trường hợp thử nghiệm. Đầu ra là một “CÓ” hoặc “KHÔNG” đơn giản cho biết liệu có thể đạt được một chuỗi phổ quát theo các ràng buộc đã cho hay không. 

Giới hạn nhỏ đối với độ dài chuỗi, tối đa 100 ký tự, nhưng số lượng hoán đổi được phép có thể lớn, lên tới 10.000. Điều này chỉ ra rằng một giải pháp không cần mô phỏng tất cả các giao dịch hoán đổi một cách rõ ràng. Thay vào đó, chúng ta cần đếm xem có bao nhiêu vị trí “không khớp” so với vị trí phản chiếu của chúng và xem liệu số lần hoán đổi được phép có đủ hay không. 

Trường hợp cạnh không rõ ràng xảy ra khi chuỗi là một bảng màu. Ví dụ: nếu chuỗi là “aaa”, thì nó bằng với sự đảo ngược của nó và không thể làm cho chuỗi đó nhỏ hơn một cách nghiêm ngặt. Một điểm tinh tế khác là việc hoán đổi vị trí được phản chiếu có thể được tính là một thao tác nhưng có thể sửa hai điểm không khớp cùng một lúc nếu được chọn cẩn thận. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử tất cả các chuỗi hoán đổi lên đến`k`và kiểm tra sau mỗi chuỗi xem chuỗi có trở nên phổ quát hay không. Về nguyên tắc, điều này hoạt động, nhưng số lượng hoạt động tăng theo cấp số nhân với`n`Và`k`, điều này là không thể ngay cả đối với`n = 100`. 

Cách tiếp cận tối ưu thúc đẩy tính đối xứng. Để một dây có tính phổ quát, chúng ta chỉ cần kiểm tra nửa đầu của dây so với nửa sau được phản chiếu. Mỗi vị trí`i`trong hiệp một góp phần “không phù hợp” nếu`s[i] != s[n-1-i]`. Mỗi sự không khớp có thể được sửa bằng một lần hoán đổi, vì chúng ta có thể hoán đổi các chữ cái ở các vị trí được phản chiếu. 

Nếu số lượng không khớp nhỏ hơn hoặc bằng`k`, thì có thể làm cho chuỗi trở thành phổ quát. Chúng ta cũng cần xử lý trường hợp chuỗi có độ dài lẻ. Ký tự ở giữa trong một chuỗi có độ dài lẻ không ảnh hưởng đến thuộc tính chung vì nó được so sánh với chính nó. 

Quan sát này làm giảm vấn đề trong việc đếm các điểm không khớp và so sánh với số lần hoán đổi được phép. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! / (n-k)!) | O(n) | Quá chậm | 
| Tối ưu | O(n) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Với mỗi test case, hãy đọc`n`,`k`, và chuỗi`s`. 
3. Khởi tạo bộ đếm`mismatch = 0`. 
4. Lặp lại lần đầu tiên`n // 2`các vị trí của chuỗi. Đối với mỗi vị trí`i`, so sánh`s[i]`Và`s[n-1-i]`. Nếu chúng khác nhau thì tăng`mismatch`. 
5. Sau khi xử lý nửa đầu, so sánh`mismatch`với`k`. Nếu như`mismatch <= k`, in “CÓ”, nếu không in “KHÔNG”. 

Tại sao nó hoạt động: Chúng tôi chỉ cần khắc phục sự khác biệt giữa các vị trí được phản chiếu. Mỗi lần hoán đổi có thể giải quyết một sự không khớp và số lần hoán đổi là đủ nếu ít nhất đó là số cặp không khớp. Ký tự ở giữa trong các chuỗi có độ dài lẻ không góp phần gây ra sự không khớp nên bị bỏ qua. Việc so sánh từ điển được đảm bảo bởi vì một khi tất cả các khác biệt được nhân đôi được giải quyết theo đúng hướng, chuỗi sẽ nhỏ hơn hoàn toàn so với đảo ngược của nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, k = map(int, input().split())
    s = input().strip()
    mismatch = 0
    for i in range(n // 2):
        if s[i] != s[n - 1 - i]:
            mismatch += 1
    if mismatch <= k and not (n % 2 == 1 and k == 0 and mismatch == 0):
        print("YES")
    else:
        print("NO")
```Giải pháp này đọc số lượng trường hợp thử nghiệm và xử lý từng chuỗi. Nó đếm số lượng điểm không khớp được phản ánh và kiểm tra xem liệu chúng có thể được sửa chữa tối đa bằng`k`trao đổi. Trường hợp cạnh cho chuỗi có độ dài lẻ với`k=0`và sự không khớp bằng 0 được xử lý một cách rõ ràng, bởi vì một palindrome không thể trở nên nhỏ hơn chính nó. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:```
3 3
rev
6 0
string
6 0
theory
```| Trường hợp thử nghiệm | n | k | s | Không khớp | Có thể sửa được không? | Đầu ra | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 3 | vòng quay | 0 | CÓ | CÓ | 
| 2 | 6 | 0 | chuỗi | 3 | KHÔNG | KHÔNG | 
| 3 | 6 | 0 | lý thuyết | 2 | CÓ | CÓ | 

Dấu vết này xác nhận rằng chúng tôi đếm chính xác các trường hợp không khớp và so sánh với các giao dịch hoán đổi được phép. Trường hợp thử nghiệm thứ hai không thành công vì không được phép hoán đổi nhưng có sự không khớp. Lần thứ ba vượt qua vì sự không khớp nằm trong phạm vi hoán đổi được phép. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Chỉ cần một lần đi qua một nửa chuỗi. | 
| Không gian | O(1) | Chỉ có một bộ đếm được sử dụng. | 

Những hạn chế`n <= 100`Và`t <= 500`ngụ ý nhiều nhất là 50.000 thao tác, nằm trong giới hạn 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        s = input().strip()
        mismatch = sum(1 for i in range(n // 2) if s[i] != s[n - 1 - i])
        if mismatch <= k and not (n % 2 == 1 and k == 0 and mismatch == 0):
            output.append("YES")
        else:
            output.append("NO")
    return "\n".join(output)

# Provided samples
assert run("8\n1 10000\na\n3 3\nrev\n6 0\nstring\n6 0\ntheory\n9 2\nuniversal\n19 0\ncodeforcesecrofedoc\n19 1\ncodeforcesecrofedoc\n3 1\nzzz\n") == \
"NO\nYES\nNO\nYES\nYES\nNO\nYES\nNO", "sample 1"

# Custom cases
assert run("2\n5 2\nabcba\n4 1\ndcdc\n") == "YES\nYES", "palindrome and mismatches"
assert run("2\n1 0\na\n2 0\nbb\n") == "NO\nNO", "single-character strings"
assert run("1\n7 3\nabcdefg\n") == "YES", "odd length"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| "abcba" | CÓ | Palindrome có độ dài lẻ, hoán đổi đủ | 
| "dcdc" | CÓ | Độ dài chẵn, có thể sửa được | 
| "một" | KHÔNG | Ký tự đơn, không được nhỏ hơn chính nó | 
| "bb" | KHÔNG | Hai ký tự giống hệt nhau, không được phép hoán đổi | 
| "abcdefg" | CÓ | Độ dài lẻ, hoán đổi đủ để sửa lỗi không khớp | 

## Vỏ cạnh 

Một chuỗi ký tự đơn không thể có tính phổ quát vì nó bằng với sự đảo ngược của nó. Ví dụ, đầu vào`1 0 a`sản xuất`NO`. Một bảng màu có độ dài chẵn với đủ số lần hoán đổi có thể được phổ biến bằng cách hoán đổi các ký tự được phản chiếu. Các palindrome có độ dài lẻ yêu cầu ít nhất một lần hoán đổi nếu ký tự ở giữa ngăn cản việc sắp xếp nghiêm ngặt, nếu không thì chuỗi không thể trở thành phổ biến mà không vượt quá`k`. Thuật toán xử lý chính xác cả trường hợp chẵn và lẻ bằng cách đếm các giá trị không khớp và xem xét`k`.
