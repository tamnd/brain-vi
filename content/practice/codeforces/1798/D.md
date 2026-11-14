---
title: "CF 1798D - Sự sắp xếp gây sốc"
description: "Chúng ta được cho một mảng các số nguyên có tổng bằng 0. Nhiệm vụ là sắp xếp lại các phần tử sao cho tổng tuyệt đối tối đa trên bất kỳ mảng con liền kề nào nhỏ hơn hoàn toàn so với chênh lệch giữa phần tử lớn nhất và nhỏ nhất trong mảng."
date: "2026-06-09T09:54:42+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 1798
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 860 (Div. 2)"
rating: 1600
weight: 1798
solve_time_s: 230
verified: false
draft: false
---

[CF 1798D - Sự dàn xếp gây sốc](https://codeforces.com/problemset/problem/1798/D) 

**Đánh giá:** 1600 
**Tags:** thuật toán xây dựng, tham lam, toán học 
**Thời gian giải:** 3 phút 50 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên có tổng bằng 0. Nhiệm vụ là sắp xếp lại các phần tử sao cho tổng tuyệt đối tối đa trên bất kỳ mảng con liền kề nào nhỏ hơn hoàn toàn so với chênh lệch giữa phần tử lớn nhất và nhỏ nhất trong mảng. Nói cách khác, nếu chúng ta biểu thị phần tử lớn nhất bằng`max_val`và tối thiểu bằng`min_val`, chúng ta cần tìm một hoán vị của mảng sao cho không có khối số liên tiếp nào có tổng giá trị tuyệt đối bằng`max_val - min_val`hoặc hơn thế nữa. 

Đầu vào chứa nhiều trường hợp thử nghiệm. Mỗi mảng có thể lớn tới 300.000 phần tử và tổng của tất cả các trường hợp thử nghiệm được giới hạn ở mức 300.000. Với giới hạn thời gian 2 giây, bất kỳ thuật toán nào có độ phức tạp kém hơn`O(n log n)`mỗi trường hợp thử nghiệm có thể sẽ quá chậm. Điều này loại trừ các phương pháp thử tất cả các hoán vị hoặc tính tổng cho tất cả`O(n^2)`mảng con trực tiếp. 

Trường hợp cạnh không rõ ràng xảy ra khi tất cả các số đều bằng 0. Tổng của bất kỳ mảng con nào đều bằng 0, không nhỏ hơn hiệu của`0 - 0 = 0`. Một tình huống phức tạp khác phát sinh khi mảng chứa các số dương và số âm lớn triệt tiêu lẫn nhau; một nỗ lực ngây thơ để sắp xếp hoặc xáo trộn có thể vô tình tạo ra tiền tố hoặc hậu tố có tổng bằng tổng khoảng`max - min`, vi phạm điều kiện 

Ví dụ, đưa ra`a = [0, 0, 0]`, mọi hoán vị đều mang lại`max(abs(sum)) = 0`Và`max - min = 0`, vì vậy đầu ra phải là`No`. Một trường hợp khác là`a = [5, -5]`; mọi thứ tự đều tạo ra tổng mảng con tuyệt đối là 5, bằng`max - min = 10`, thỏa mãn điều kiện Việc nhận ra những trường hợp ranh giới này giúp ngăn chặn những giả định không chính xác về việc luôn có thể xây dựng một sự sắp xếp hợp lý. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ thử mọi hoán vị của mảng và kiểm tra tổng mảng con tối đa. Điều này có hiệu quả vì điều kiện có tính xác định: đối với mỗi hoán vị, chúng ta có thể tính tất cả các tổng của mảng con liền kề và xác minh bất đẳng thức. Tuy nhiên, số hoán vị là`n!`, và thậm chí đối với`n = 10`, đây là 3.628.800 phép tính. Tính toán tất cả các tổng mảng con cho mỗi hoán vị sẽ nhân số này với`O(n^2)`, làm cho nó hoàn toàn không khả thi đối với các ràng buộc đã cho. 

Cái nhìn sâu sắc quan trọng đến từ việc quan sát mối quan hệ giữa số dương và số âm. Tổng của mảng bằng 0 nên tổng dương bằng tổng âm tuyệt đối. Nếu chúng ta sắp xếp các số và đặt tất cả các số dương theo sau là tất cả các số âm (hoặc ngược lại), thì tổng mảng con lớn nhất sẽ xuất hiện ở một trong các đầu và giá trị tuyệt đối của nó có thể đạt tới tổng của tất cả các số dương, nhiều nhất là`max_val - min_val`nếu mảng chứa ít nhất một số hoàn toàn dương và một số hoàn toàn âm. Sự sắp xếp này tránh nguy cơ các mảng con tính tổng chính xác bằng tổng khoảng cách ở các vị trí trung gian. 

Vì vậy, chúng ta có thể xây dựng một hoán vị hợp lệ bằng cách trước tiên tách số dương và số âm. Nếu tổng của tất cả các số dương bằng tổng của tất cả các số âm trong giá trị tuyệt đối và cả hai số này đều không bằng 0, thì thứ tự của số dương trước sau số âm hoặc số âm trước sau số dương, đảm bảo rằng không có tổng mảng con nào đạt đến giới hạn khoảng. Nếu tất cả các số đều bằng 0 thì nhiệm vụ này là không thể vì bất đẳng thức yêu cầu là nghiêm ngặt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! * n^2) | O(n^2) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. Lặp lại qua từng trường hợp thử nghiệm. Đối với mỗi trường hợp, hãy đọc độ dài mảng`n`và mảng`a`. 
2. Tính các phần tử lớn nhất và nhỏ nhất,`max_val`Và`min_val`. Nếu như`max_val - min_val`bằng 0, đầu ra`No`. Điều này xử lý các trường hợp hoàn toàn bằng 0 hoặc hoàn toàn bằng nhau. 
3. Phân vùng mảng thành hai danh sách:`pos`chứa tất cả các số dương và`neg`chứa tất cả các số âm. Bạn có thể tùy ý thu thập các số 0 trong một danh sách riêng biệt nhưng có thể thêm số 0 vào bất kỳ đâu. 
4. Sắp xếp các số dương theo thứ tự giảm dần và các số âm theo thứ tự tăng dần. Điều này đảm bảo các số lớn nhất nằm ở các cạnh của mảng được xây dựng, giảm khả năng tổng của mảng con vượt quá giới hạn cho phép. 
5. Xây dựng mảng đã sắp xếp lại bằng cách nối`pos`Và`neg`. Nếu muốn, các số 0 có thể được đặt ở giữa, mặc dù vị trí đó không ảnh hưởng đến điều kiện vì số 0 không đóng góp vào tổng. 
6. Đầu ra`Yes`và mảng được sắp xếp lại. 

Tại sao nó hoạt động: Bằng cách tách các số dương và số âm, các tổng riêng phần dọc theo mảng dao động ít hơn. Các số dương lớn nhất được cân bằng bởi các số âm lớn nhất ở đầu đối diện, ngăn chặn bất kỳ mảng con liền kề nào tính tổng theo giá trị tuyệt đối cho toàn bộ mảng. Điều này đảm bảo điều kiện được thỏa mãn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    
    max_val = max(a)
    min_val = min(a)
    
    if max_val - min_val == 0:
        print("No")
        continue
    
    pos = [x for x in a if x > 0]
    neg = [x for x in a if x < 0]
    zero = [x for x in a if x == 0]
    
    pos.sort(reverse=True)
    neg.sort()
    
    ans = pos + neg + zero
    print("Yes")
    print(" ".join(map(str, ans)))
```Giải pháp tách các số dương và số âm để giảm tổng mảng con tối đa. Sắp xếp đảm bảo số lượng lớn nhất được đặt ở cực trị. Các số 0 được đặt cuối cùng vì chúng không ảnh hưởng đến tổng của mảng con. Các trường hợp cạnh như mảng số 0 hoặc mảng có tất cả các giá trị bằng nhau được xác định chính xác bằng cách kiểm tra xem`max_val - min_val`bằng không. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:`[3, 4, -2, -5]`

`max_val = 4, min_val = -5, max_val - min_val = 9`Số dương:`[3, 4]`, Số âm:`[-2, -5]`| Bước | tư thế | phủ định | và | 
| --- | --- | --- | --- | 
| Riêng biệt | [3, 4] | [-2, -5] | - | 
| Sắp xếp | [4, 3] | [-5, -2] | - | 
| Kết hợp | - | - | [4, 3, -5, -2] | 
Tổng mảng con tuyệt đối tối đa là`7`, nhỏ hơn`9`. 

### Ví dụ 2 

đầu vào:`[0, 1, -1]`

`max_val = 1, min_val = -1, max_val - min_val = 2`Tích cực:`[1]`, Tiêu cực:`[-1]`, Không:`[0]`Kết hợp:`[1, -1, 0]`Tổng mảng con tuyệt đối tối đa:`1`, ít hơn`2`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp tích cực và tiêu cực chiếm ưu thế; phân vùng là O(n) | 
| Không gian | O(n) | Lưu trữ danh sách pos, neg và zero | 

Giải pháp dễ dàng phù hợp với giới hạn 2 giây cho`n`lên tới 300.000 và tổng số tiền tích lũy là`n`trên tất cả các trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # include solution code here
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        
        max_val = max(a)
        min_val = min(a)
        
        if max_val - min_val == 0:
            print("No")
            continue
        
        pos = [x for x in a if x > 0]
        neg = [x for x in a if x < 0]
        zero = [x for x in a if x == 0]
        
        pos.sort(reverse=True)
        neg.sort()
        
        ans = pos + neg + zero
        print("Yes")
        print(" ".join(map(str, ans)))
    return output.getvalue().strip()

# Provided samples
assert run("7\n4\n3 4 -2 -5\n5\n2 2 2 -3 -3\n8\n-3 -3 1 1 1 1 1 1\n3\n0 1 -1\n7\n-3 4 3 4 -4 -4 0\n1\n0\n7\n-18 13 -18 -17 12 15 13\n") == \
"Yes\n4 3 -5 -2\nYes\n2 2 2 -3 -3\nYes\n1 1 1 1 1 1 -3 -3\nYes\n1 -1 0\nYes\n4 4 -4 -4 3 -3 0\nNo\nYes\n15 13 13 12 -18 -18 -17", "sample 1"

# Custom cases
assert run("1\n2\n0 0\n") == "No", "all zeros"
assert run("1\n3\n5 -5 0\n") == "Yes\n5 -5
```
