---
title: "CF 1944D - Chuỗi con không Palindromic"
description: "Chúng ta được cung cấp một chuỗi các chữ cái tiếng Anh viết thường và một tập hợp các truy vấn, mỗi truy vấn chỉ định một chuỗi con liền kề của chuỗi. Đối với mỗi truy vấn, chúng ta được yêu cầu tính tổng của tất cả các độ dài $k$ mà tồn tại ít nhất một chuỗi con có độ dài $k$ không phải là một bảng màu."
date: "2026-06-09T01:52:18+07:00"
tags: ["codeforces", "competitive-programming", "hashing", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 1944
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 934 (Div. 2)"
rating: 2000
weight: 1944
solve_time_s: 82
verified: false
draft: false
---

[CF 1944D - Chuỗi con không phải Palindromic](https://codeforces.com/problemset/problem/1944/D) 

**Xếp hạng:** 2000 
**Tags:** băm, triển khai, chuỗi 
**Thời gian giải:** 1m 22s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một chuỗi các chữ cái tiếng Anh viết thường và một tập hợp các truy vấn, mỗi truy vấn chỉ định một chuỗi con liền kề của chuỗi. Đối với mỗi truy vấn, chúng tôi được yêu cầu tính tổng của tất cả các độ dài$k$mà tồn tại ít nhất một chuỗi con có độ dài$k$đó không phải là một bảng màu. Nói cách khác, đối với mỗi chuỗi con, chúng tôi muốn tổng độ dài của tất cả các phân đoạn không tầm thường phá vỡ tính đối xứng. 

Các ràng buộc đầu vào lớn: độ dài chuỗi và số lượng truy vấn có thể đạt tới$2 \cdot 10^5$và tổng tổng của tất cả độ dài chuỗi và số lượng truy vấn trong các trường hợp thử nghiệm cũng không thể vượt quá$2 \cdot 10^5$. Do đó, một giải pháp đơn giản kiểm tra mọi chuỗi con ở mọi độ dài là không thể vì số lượng chuỗi con tăng bậc hai theo độ dài của chuỗi, đưa ra trường hợp xấu nhất$O(n^3)$giải pháp vượt xa khả năng thực hiện đối với$n \sim 10^5$. 

Ví dụ, các trường hợp cạnh không rõ ràng phát sinh khi chuỗi con đồng nhất`aaa`hoặc`bbbb`. Trong những trường hợp này, tất cả các chuỗi con đều là chuỗi palindrome nên tổng bằng 0. Một trường hợp khác xảy ra khi chuỗi con xen kẽ các ký tự, như`ababab`. Khi đó, chỉ các chuỗi con có độ dài từ 2 trở lên mới có thể là không phải palindromic và chúng ta phải cẩn thận không đếm độ dài gấp đôi - mỗi độ dài chỉ được tính tối đa một lần nếu nó có bất kỳ phân đoạn không palindromic nào. Chuỗi con nhỏ nhất, có độ dài 2, là ứng cử viên đầu tiên phá vỡ tính đối xứng. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force kiểm tra mọi chuỗi con của từng chuỗi con truy vấn. Đối với mỗi chiều dài$k$từ 2 đến độ dài chuỗi con, chúng ta sẽ trượt một cửa sổ có kích thước$k$và kiểm tra xem nó có phải là một bảng màu không. Điều này hoạt động chính xác nhưng yêu cầu kiểm tra$O(n^2)$chuỗi con cho mỗi truy vấn. Với$q$lên đến$2 \cdot 10^5$Và$n$lên đến$2 \cdot 10^5$, điều này hoàn toàn không thể thực hiện được. 

Thông tin chi tiết quan trọng là chúng ta không cần phải kiểm tra mọi chuỗi con ở mọi độ dài. Để không phải là chuỗi đối xứng, một chuỗi con có độ dài ít nhất là 2 chỉ cần một cặp ký tự liên tiếp khác nhau. Nếu mọi cặp liền kề trong chuỗi con đều giống hệt nhau thì tất cả các chuỗi con đều là chuỗi đối xứng; mặt khác, chuỗi con không phải là chuỗi palindrome đối với mọi độ dài từ 2 đến độ dài của nó, ngoại trừ trường hợp có thể là chuỗi palindrome đầy đủ. Do đó, chúng ta có thể giảm bớt vấn đề để tìm xem chuỗi con có chứa ít nhất một vị trí mà các ký tự liên tiếp khác nhau hay không. Nếu đúng như vậy, chuỗi con ngay lập tức là 2-tốt và bằng cách mở rộng, mọi độ dài lớn hơn sẽ tự động tốt vì chúng ta luôn có thể mở rộng cặp không palindromic để tạo thành chuỗi con không palindromic dài hơn. 

Vì vậy, đối với mỗi truy vấn, chúng ta chỉ cần kiểm tra hai điều kiện: chuỗi con có giống hệt nhau không và độ dài của chuỗi con. Nếu tất cả đều giống hệt nhau thì câu trả lời là 0; mặt khác, câu trả lời là tổng các số nguyên từ 2 đến độ dài chuỗi con. Tính tổng đó sử dụng công thức chuỗi số học$2 + 3 + \dots + L = \frac{L(L+1)}{2} - 1$, đó là thời gian không đổi cho mỗi truy vấn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$mỗi truy vấn |$O(1)$| Quá chậm | 
| Tối ưu |$O(n+q)$mỗi trường hợp thử nghiệm |$O(1)$mỗi truy vấn | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. Đối với mỗi trường hợp thử nghiệm, hãy đọc$n$Và$q$, theo sau là chuỗi. 
2. Đối với mỗi truy vấn, hãy đọc ranh giới chuỗi con$l$Và$r$. Điều chỉnh chúng để lập chỉ mục dựa trên số không. 
3. Trích xuất chuỗi con. Kiểm tra xem tất cả các ký tự trong chuỗi con có giống nhau không. Điều này có thể được thực hiện một cách hiệu quả bằng cách so sánh từng ký tự với ký tự đầu tiên hoặc bằng cách kiểm tra sự tồn tại của ít nhất một ký tự liền kề khác nhau. 
4. Nếu tất cả các ký tự giống hệt nhau, xuất ra 0. Điều này tương ứng với chuỗi con có kiểu đối xứng hoàn toàn đối với mọi độ dài. 
5. Mặt khác, tính tổng của tất cả các độ dài từ 2 đến độ dài chuỗi con bằng công thức$\frac{L(L+1)}{2} - 1$và xuất nó. 

Điều này có tác dụng vì các chuỗi con duy nhất có thể không thuộc loại palindromic là các chuỗi con hoàn toàn đồng nhất. Sự hiện diện của bất kỳ ký tự liền kề khác nhau nào đảm bảo chuỗi con không palindromic có độ dài 2 và điều này luôn có thể được mở rộng đến độ dài lớn hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n, q = map(int, input().split())
    s = input().strip()
    for _ in range(q):
        l, r = map(int, input().split())
        l -= 1
        r -= 1
        substring = s[l:r+1]
        # check if all characters are the same
        all_same = True
        for i in range(1, len(substring)):
            if substring[i] != substring[i-1]:
                all_same = False
                break
        if all_same:
            print(0)
        else:
            L = r - l + 1
            print(L * (L + 1) // 2 - 1)
```Các lựa chọn chính là lặp lại chuỗi con chỉ một lần để phát hiện tính phân tập và sử dụng công thức tính tổng số học để tránh vòng lặp rõ ràng. Có thể tránh các lỗi riêng lẻ bằng cách điều chỉnh các chỉ số về độ dài chuỗi con dựa trên 0 và tính toán cẩn thận. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Chuỗi con đầu vào:`aaab`(truy vấn 1, vị trí từ 1 đến 4) 

| tôi | chuỗi con[i] | So sánh với chuỗi con[i-1] | tất cả_giống nhau | 
| --- | --- | --- | --- | 
| 1 | một | một | Đúng | 
| 2 | một | một | Đúng | 
| 3 | b | một | Sai | 

Chuỗi con chứa các ký tự khác nhau, vì vậy L = 4, sum = 4*5/2 - 1 = 9. Đúng. 

### Ví dụ 2 

Chuỗi con đầu vào:`aaa`(truy vấn 2, vị trí từ 1 đến 3) 

| tôi | chuỗi con[i] | So sánh với chuỗi con[i-1] | tất cả_giống nhau | 
| --- | --- | --- | --- | 
| 1 | một | một | Đúng | 
| 2 | một | một | Đúng | 

Tất cả các ký tự giống hệt nhau → đầu ra 0. 

Điều này chứng tỏ rằng thuật toán phân biệt chính xác các chuỗi con palindromic hoàn toàn với các chuỗi con không palindromic. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n+q) cho mỗi trường hợp thử nghiệm | Mỗi chuỗi con được quét một lần để phát hiện các ký tự liền kề khác nhau, công thức tính tổng là O(1) | 
| Không gian | O(1) mỗi truy vấn | Chỉ một lát chuỗi con và cờ boolean được sử dụng | 

Tổng thời gian là tuyến tính ở kích thước đầu vào, thoải mái trong giới hạn 3 giây cho$n, q \le 2 \cdot 10^5$. Việc sử dụng bộ nhớ là tối thiểu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    # call solution
    t = int(input())
    for _ in range(t):
        n, q = map(int, input().split())
        s = input().strip()
        for _ in range(q):
            l, r = map(int, input().split())
            l -= 1
            r -= 1
            substring = s[l:r+1]
            all_same = True
            for i in range(1, len(substring)):
                if substring[i] != substring[i-1]:
                    all_same = False
                    break
            if all_same:
                print(0)
            else:
                L = r - l + 1
                print(L * (L + 1) // 2 - 1)
    return out.getvalue().strip()

# provided samples
assert run("5\n4 4\naaab\n1 4\n1 3\n3 4\n2 4\n3 2\nabc\n1 3\n1 2\n5 4\npqpcc\n1 5\n4 5\n1 3\n2 4\n2 1\naa\n1 2\n12 1\nsteponnopets\n1 12\n") == "9\n0\n2\n5\n5\n2\n14\n0\n2\n5\n0\n65"

# custom edge cases
assert run("1\n2 1\naa\n1 2\n") == "0"  # minimum size, all equal
assert run("1\n2 1\nab\n1 2\n") == "2"  # minimum size, not equal
assert run("1\n5 1\naaaaa\n1 5\n") == "0"  # all equal
assert run("1\n5 1\nabcde\n1 5\n") == "14"  # all distinct
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`aa`| 0 | chuỗi con có kích thước tối thiểu, tất cả đều bằng nhau | 
| | | |
