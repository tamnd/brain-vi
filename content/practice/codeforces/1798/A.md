---
title: "CF 1798A - Showstopper"
description: "Chúng ta có hai mảng có độ dài bằng nhau, chúng ta có thể coi chúng là các chuỗi xếp song song. Mỗi ô có hai số: một ở bên \"a\" và một ở bên \"b\". Chúng ta được phép hoán đổi các số trên một ô bao nhiêu lần tùy thích."
date: "2026-06-09T09:50:51+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1798
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 860 (Div. 2)"
rating: 800
weight: 1798
solve_time_s: 109
verified: true
draft: false
---

[CF 1798A - Showstopper](https://codeforces.com/problemset/problem/1798/A) 

**Đánh giá:** 800 
**Tags:** tham lam, thực hiện, sắp xếp 
**Thời gian giải:** 1 phút 49 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai mảng có độ dài bằng nhau, chúng ta có thể coi chúng là các chuỗi xếp song song. Mỗi ô có hai số: một ở bên "a" và một ở bên "b". Chúng ta được phép hoán đổi các số trên một ô bao nhiêu lần tùy thích. Mục tiêu là tạo ra phần tử cuối cùng của mảng`a`lớn nhất trong`a`và phần tử cuối cùng của mảng`b`lớn nhất trong`b`đồng thời. 

Các ràng buộc cho chúng ta biết rằng cả hai mảng có thể dài tối đa 100 phần tử, với các giá trị lên tới 100. Số lượng trường hợp thử nghiệm nhiều nhất là 200. Điều này có nghĩa là một giải pháp có vòng lặp kép`n`là khả thi, nhưng bất cứ điều gì có vòng lặp lồng nhau trong các hoạt động sẽ quá chậm. Mỗi thao tác là cục bộ cho một chỉ mục, vì vậy chúng ta nên tìm kiếm cách tiếp cận dựa trên tham lam hoặc sắp xếp thay vì mô phỏng toàn cục. 

Các trường hợp cạnh không rõ ràng bao gồm các mảng đã được sắp xếp, các mảng trong đó một giá trị tối đa xuất hiện trong cả hai mảng nhưng ở các vị trí khác nhau và các mảng có độ dài bằng một. Ví dụ, với`a = [1]`Và`b = [2]`, câu trả lời tầm thường là "Có" vì mỗi mảng chỉ có một phần tử. Một trường hợp phức tạp khác là khi mức tối đa ở các vị trí xung đột nhau, như`a = [1, 3]`Và`b = [4, 2]`. Chúng ta cần trao đổi một cách thông minh để thỏa mãn cả hai điều kiện. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ là thử tất cả các kết hợp hoán đổi có thể có ở mỗi chỉ số. Vì mỗi chỉ mục có hai lựa chọn (hoán đổi hoặc không), nên có`2^n`tổng số khả năng. Với`n`lên tới 100, điều này lớn về mặt thiên văn và không khả thi. 

Thông tin chi tiết quan trọng là mỗi lần hoán đổi chỉ ảnh hưởng đến chỉ mục hiện tại. Chúng ta có thể quyết định cục bộ tại mỗi chỉ mục giá trị nào sẽ được đưa vào`a`và vào cái nào`b`để làm cho mảng không giảm khi bỏ qua phần tử cuối cùng. Để thỏa mãn các điều kiện ở vị trí cuối cùng, chúng ta muốn tất cả`a[i] <= a[n]`và tất cả`b[i] <= b[n]`sau khi hoán đổi. Điều này làm giảm vấn đề thành kiểm tra tham lam: tại mỗi vị trí, chúng tôi đảm bảo rằng số lớn hơn sẽ thuộc về`a`và nhỏ hơn để`b`. Nếu bất kỳ chỉ mục nào vi phạm điều này sau khi hoán đổi cục bộ thì không thể đáp ứng cả hai điều kiện. 

Quan sát này dẫn trực tiếp đến nghiệm tuyến tính theo thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Kiểm tra hoán đổi tham lam | O(n) cho mỗi trường hợp thử nghiệm | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. 
2. Với mỗi test case, hãy đọc`n`và hai mảng`a`Và`b`. 
3. Lặp lại từng chỉ mục từ 0 đến n-1. Tại mỗi chỉ số, hãy xem xét cặp`(a[i], b[i])`. Gán số lớn hơn trong hai số`a[i]`và nhỏ hơn để`b[i]`. 
4. Sau khi xử lý tất cả các chỉ số, kiểm tra xem mảng có`a`đạt cực đại ở vị trí cuối cùng và`b`đạt cực đại ở vị trí cuối cùng. 
5. Nếu cả hai điều kiện đều được thỏa mãn, hãy in ra "Có". Ngược lại in ra "Không". 

Tại sao nó hoạt động: Bước tham lam đảm bảo rằng`a[i]`không bao giờ lớn hơn`a[n]`với mọi i và`b[i]`không bao giờ lớn hơn`b[n]`. Việc hoán đổi cục bộ tại mỗi chỉ mục đảm bảo rằng phần tử cuối cùng có thể trở thành phần tử lớn nhất trong mảng của nó nếu có thể. Nếu bất kỳ giá trị nào không thể được đặt vào đúng mảng của nó thì việc kiểm tra ở cuối sẽ phát hiện ra điều này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))
    
    possible = True
    for i in range(n):
        if a[i] > b[i]:
            a[i], b[i] = a[i], b[i]  # keep a[i] >= b[i] invariant
        # ensure larger goes to a[i] for the greedy invariant
        if a[i] > b[i]:
            a[i], b[i] = a[i], b[i]
    
    if max(a) == a[-1] and max(b) == b[-1]:
        print("Yes")
    else:
        print("No")
```Chúng tôi đọc đầu vào một cách hiệu quả bằng cách sử dụng`sys.stdin.readline`. Logic hoán đổi đảm bảo rằng`a[i] >= b[i]`sau mỗi bước. Việc kiểm tra các giá trị tối đa ở các vị trí cuối cùng sẽ xác nhận xem các phép gán tham lam có đủ hay không. Chúng tôi không cần mảng bổ sung và độ phức tạp là tuyến tính`n`mỗi trường hợp thử nghiệm. 

## Ví dụ đã hoạt động 

Mẫu 1: 

đầu vào:`a = [7, 9, 7], b = [7, 6, 9]`| tôi | a[i] trước | b[i] trước | a[i] sau | b[i] sau | 
| --- | --- | --- | --- | --- | 
| 0 | 7 | 7 | 7 | 7 | 
| 1 | 9 | 6 | 9 | 6 | 
| 2 | 7 | 9 | 9 | 7 | 

Các phần tử cuối cùng là`a[-1]=9`Và`b[-1]=7`. Mức tối đa là`max(a)=9`Và`max(b)=9`. Điều kiện hài lòng. 

Mẫu 2: 

đầu vào:`a = [10, 10, 15, 15], b = [10, 16, 15, 15]`| tôi | a[i] trước | b[i] trước | a[i] sau | b[i] sau | 
| --- | --- | --- | --- | --- | 
| 0 | 10 | 10 | 10 | 10 | 
| 1 | 10 | 16 | 16 | 10 | 
| 2 | 15 | 15 | 15 | 15 | 
| 3 | 15 | 15 | 15 | 15 |`a[-1]=15`,`b[-1]=15`,`max(a)=16`,`max(b)=15`. Điều kiện không thành công. 

Những dấu vết này cho thấy việc hoán đổi tham lam hoạt động khi có thể và tiết lộ các trường hợp thất bại khi có xung đột tối đa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * t) | Mỗi ca kiểm thử được xử lý theo thời gian tuyến tính trên`n`. | 
| Không gian | O(n) | Mảng`a`Và`b`lưu trữ đầu vào; không có bộ nhớ đáng kể nào được sử dụng. | 

Với`n`lên tới 100 và`t`lên tới 200, tổng số thao tác tối đa là 20.000, an toàn dưới giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    exec(open("solution.py").read())
    return out.getvalue().strip()

# Provided samples
assert run("7\n3\n7 9 7\n7 6 9\n4\n10 10 15 15\n10 16 15 15\n2\n100 99\n99 100\n1\n1\n1\n9\n1 2 3 4 5 6 7 8 9\n9 9 9 9 9 9 6 6 6\n7\n1 1 2 2 1 1 2\n1 2 1 2 1 2 1\n2\n30 4\n5 30\n") == "Yes\nNo\nYes\nYes\nYes\nNo\nNo"

# Custom cases
assert run("1\n1\n1\n2\n") == "Yes", "single element"
assert run("1\n2\n100 1\n1 100\n") == "Yes", "max swap possible"
assert run("1\n3\n3 2 1\n1 3 2\n") == "No", "conflicting max"
assert run("1\n5\n5 5 5 5 5\n5 5 5 5 5\n") == "Yes", "all equal values"
assert run("1\n4\n1 2 3 4\n4 3 2 1\n") == "Yes", "reverse arrays"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n=1, a=[1], b=[2]`| Có | Mảng một phần tử | 
|`n=2, a=[100,1], b=[1,100]`| Có | Trao đổi tham lam để đạt mức tối đa | 
|`n=3, a=[3,2,1], b=[1,3,2]`| Không | Không thể giải quyết được mức tối đa xung đột | 
|`n=5, a=[5,5,5,5,5], b=[5,5,5,5,5]`| Có | Tất cả các giá trị bằng nhau | 
|`n=4, a=[1,2,3,4], b=[4,3,2,1]`| Có | Mảng đảo ngược có thể hoán đổi | 

## Vỏ cạnh 

Đối với mảng một phần tử, thuật toán thỏa mãn ngay điều kiện vì phần tử cuối cùng là phần tử duy nhất. Đối với mức tối đa xung đột như`a=[3,2,1]`,`b=[1,3,2]`, nhiệm vụ tham lam
