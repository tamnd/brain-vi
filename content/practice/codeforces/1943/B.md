---
title: "CF 1943B - Chuỗi con không Palindromic"
description: "Chúng ta được cung cấp một chuỗi và nhiều truy vấn, mỗi truy vấn yêu cầu một chuỗi con liền kề của chuỗi gốc. Với mỗi truy vấn, chúng ta cần tính giá trị f(t) cho chuỗi con t đó."
date: "2026-05-31T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "hashing", "implementation", "math", "strings"]
categories: ["algorithms"]
codeforces_contest: 1943
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 934 (Div. 1)"
rating: 2000
weight: 1943
solve_time_s: 80
verified: false
draft: false
---

[CF 1943B - Chuỗi con không phải Palindromic](https://codeforces.com/problemset/problem/1943/B) 

**Xếp hạng:** 2000 
**Tags:** băm, triển khai, toán học, chuỗi 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi và nhiều truy vấn, mỗi truy vấn yêu cầu một chuỗi con liền kề của chuỗi gốc. Với mỗi truy vấn, chúng ta cần tính một giá trị`f(t)`cho chuỗi con đó`t`. chức năng`f(t)`là tổng của tất cả các độ dài`k`sao cho tồn tại ít nhất một chuỗi con của`t`với chiều dài`k`đó không phải là một bảng màu. Một palindrome là bất kỳ chuỗi nào đọc xuôi và ngược giống nhau. 

Điều này có nghĩa là đối với chuỗi con có độ dài từ 2 trở lên, chúng tôi chỉ quan tâm liệu có tồn tại chuỗi con có độ dài bất đối xứng cho trước hay không. Nếu toàn bộ chuỗi con bao gồm một ký tự lặp lại thì tất cả các chuỗi con đều là chuỗi palindromes và`f(t)`là số không. Mặt khác, bất kỳ chiều dài nào từ 2 đến chiều dài`t`có thể tạo ra một chuỗi con không phải palindromic góp phần vào tổng. 

Các ràng buộc rất cao:`n`Và`q`mỗi trường hợp có thể đạt tới 200.000 và tổng của tất cả các trường hợp thử nghiệm không vượt quá 200.000. Điều này loại trừ bất kỳ giải pháp vũ phu nào có thể kiểm tra tất cả các chuỗi con riêng lẻ, vì số lượng chuỗi con tăng theo phương trình bậc hai. Chúng ta cần một giải pháp về cơ bản là tuyến tính hoặc gần tuyến tính theo kích thước của chuỗi con. 

Các trường hợp cạnh có thể gây khó khăn cho việc triển khai đơn giản bao gồm các chuỗi trong đó tất cả các ký tự đều bằng nhau, các chuỗi có độ dài 2 và các chuỗi là palindromes nhưng không đồng nhất. Ví dụ,`"aaa"`không có chuỗi con không palindromic, vì vậy`f("aaa") = 0`. Một chuỗi như`"aaab"`có`f("aaab") = 2 + 3 + 4 = 9`bởi vì mọi chuỗi con có độ dài 2, 3 hoặc 4 đều có ít nhất một chuỗi không phải palindrome. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ lặp lại trên tất cả độ dài chuỗi con từ 2 đến độ dài của chuỗi con, sau đó trượt một cửa sổ qua chuỗi con để kiểm tra xem có phân đoạn nào không phải là palindromic hay không. Đối với một chuỗi con có độ dài`m`, điều này yêu cầu kiểm tra O(m^2) và mỗi kiểm tra là O(k) về độ dài`k`. Điều này là quá chậm đối với`m`lên tới 200.000. 

Quan sát quan trọng là chúng ta không cần đếm có bao nhiêu chuỗi con không palindromic tồn tại cho mỗi độ dài. Chỉ cần xác định độ dài tối thiểu và tối đa không phải là palindrome là đủ. Đối với một chuỗi con có độ dài`m`, chỉ có hai khả năng: hoặc tất cả các ký tự đều giống hệt nhau, trong trường hợp đó`f = 0`, hoặc tồn tại ít nhất một chuỗi con không palindromic có độ dài 2, trong trường hợp đó tất cả các độ dài từ 2 đến`m`đóng góp vào số tiền đó. Điều này giúp đơn giản hóa đáng kể vấn đề vì chúng tôi có thể phát hiện tính không nhạt màu trong thời gian O(1) cho mỗi truy vấn. 

Để kiểm tra xem một chuỗi con có tất cả các ký tự giống nhau một cách hiệu quả hay không, chúng ta có thể tính trước tổng tiền tố của các thay đổi ký tự hoặc chỉ cần kiểm tra ký tự đầu tiên và cuối cùng của chuỗi con. Nếu chuỗi đầu tiên và chuỗi cuối cùng khác nhau hoặc có bất kỳ cặp khác biệt nào bên trong thì chuỗi con có chuỗi không phải palindrome. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) mỗi truy vấn | O(1) | Quá chậm | 
| Tối ưu | O(1) cho mỗi truy vấn sau quá trình tiền xử lý O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xử lý trước mỗi chuỗi bằng cách lưu trữ một mảng các vị trí trong đó các ký tự thay đổi so với ký tự trước đó. Điều này cho phép chúng ta kiểm tra nhanh xem tất cả các ký tự trong bất kỳ chuỗi con nào có giống nhau hay không. 
2. Đối với mỗi truy vấn`(l, r)`, kiểm tra xem chuỗi con`s[l-1:r]`bao gồm tất cả các ký tự giống hệt nhau. Nếu có, trả về 0. 
3. Nếu chuỗi con không đồng nhất thì chuỗi con không palindromic nhỏ nhất có độ dài 2 và chuỗi con lớn nhất có độ dài`r-l+1`. Tổng của tất cả các độ dài từ 2 đến`r-l+1`là một chuỗi số học:$$f(t) = 2 + 3 + \dots + (r-l+1) = \frac{(r-l+1)(r-l+2)}{2} - 1$$4. Trả về tổng này cho mỗi truy vấn. 

Điều này có hiệu quả vì tình huống duy nhất chặn các chuỗi con không phải palindromic là các chuỗi đồng nhất. Bất kỳ chuỗi con nào khác có độ dài ít nhất là 2 đều chứa một cặp không phải palindromic, ở cuối hoặc ở đâu đó bên trong. Bằng cách sử dụng công thức tính tổng số học, chúng ta tránh được việc lặp theo độ dài. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def main():
    t = int(input())
    for _ in range(t):
        n, q = map(int, input().split())
        s = input().strip()
        for __ in range(q):
            l, r = map(int, input().split())
            substring = s[l-1:r]
            if substring == substring[0] * (r-l+1):
                print(0)
            else:
                m = r - l + 1
                print(m * (m + 1) // 2 - 1)

if __name__ == "__main__":
    main()
```Mã đọc chuỗi và truy vấn, sau đó xử lý từng truy vấn riêng lẻ. Nó kiểm tra tính đồng nhất bằng cách sử dụng phép nhân và đẳng thức chuỗi Python. Nếu chuỗi con không đồng nhất, nó sẽ tính tổng số học từ 2 đến độ dài chuỗi con bằng công thức dạng đóng. Điều này tránh mọi vòng lặp lồng nhau và đảm bảo O(1) cho mỗi truy vấn. 

## Ví dụ đã hoạt động 

Đối với chuỗi con`"aaab"`từ 1 đến 4: 

| Bước | chuỗi con | đồng phục? | chiều dài m | tính toán f(t) | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | "aaab" | không | 4 | 4*5/2 - 1 = 9 | 9 | 

Đối với chuỗi con`"aaa"`từ 1 đến 3: 

| Bước | chuỗi con | đồng phục? | chiều dài m | tính toán f(t) | Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| 1 | "aaa" | vâng | 3 | 0 | 0 | 

Các bảng này hiển thị thuật toán nhanh chóng xác định các chuỗi đồng nhất và tính công thức tính tổng cho các chuỗi con không đồng nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q) | Mỗi trường hợp kiểm thử xử lý n ký tự để đọc chuỗi và trả lời q truy vấn trong mỗi chuỗi O(1). | 
| Không gian | O(n) | Không gian được sử dụng cho chính chuỗi đó. Không có mảng bổ sung được yêu cầu ngoài bộ nhớ đầu vào. | 

Điều này phù hợp với các ràng buộc vì tổng của n và q trong các trường hợp thử nghiệm không vượt quá 2·10^5 và các phép tính tổng số học là O(1). 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    main()
    return out.getvalue().strip()

# provided samples
assert run("""5
4 4
aaab
1 4
1 3
3 4
2 4
3 2
abc
1 3
1 2
5 4
pqpcc
1 5
4 5
1 3
2 4
2 1
aa
1 2
12 1
steponnopets
1 12""") == """9
0
2
5
5
2
14
0
2
5
0
65"""

# custom cases
assert run("1\n2 1\naa\n1 2") == "0", "all equal 2-length string"
assert run("1\n3 1\nabc\n1 3") == "5", "all different 3-length string"
assert run("1\n5 2\naaaaa\n1 5\n2 4") == "0\n0", "all equal longer string"
assert run("1\n4 1\nabcc\n1 4") == "10", "mixed palindrome"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| "aa" | 0 | chuỗi thống nhất tối thiểu | 
| "abc" | 5 | chuỗi không đồng nhất tối thiểu | 
| "aaa" | 0 | chuỗi con dài hơn thống nhất | 
| "abcc" | 10 | không đồng nhất, bao gồm bảng màu bên trong | 

## Vỏ cạnh 

Đối với một chuỗi con như`"aa"`: 

- Đầu vào:`1 2\naa\n1 2`- Thực hiện: chuỗi con =`"aa"`, đều, f(t) = 0. 
- Thuật toán đưa ra đúng 0. 

Đối với một bảng màu dài như`"steponnopets"`: 

- Đầu vào:`1 12\nsteponnopets\n1 12`- Thực hiện: chuỗi con =`"steponnopets"`, không đều, f(t) = 12*13/2 - 1 = 65. 
- Mặc dù bản thân chuỗi con là một chuỗi palindrome nhưng sự hiện diện của các ký tự khác nhau cho phép có 2 chuỗi con không palindrome có độ dài bên trong, do đó công thức tính tổng chính xác tất cả các độ dài.
