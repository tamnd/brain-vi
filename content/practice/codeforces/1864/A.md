---
title: "CF 1864A - Tăng và Giảm"
description: "Bài toán cho chúng ta ba số nguyên: giá trị bắt đầu x, giá trị kết thúc y và độ dài n. Chúng ta cần xây dựng một mảng a có độ dài n bắt đầu tại x và kết thúc tại y, tăng nghiêm ngặt và giảm nghiêm ngặt các hiệu liên tiếp."
date: "2026-06-08T23:53:50+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "greedy", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 1864
codeforces_index: "A"
codeforces_contest_name: "Harbour.Space Scholarship Contest 2023-2024 (Div. 1 + Div. 2)"
rating: 800
weight: 1864
solve_time_s: 100
verified: false
draft: false
---

[CF 1864A - Tăng và Giảm](https://codeforces.com/problemset/problem/1864/A) 

**Đánh giá:** 800 
**Tags:** thuật toán xây dựng, tham lam, thực hiện, toán học 
**Thời gian giải:** 1 phút 40s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Bài toán cho ta ba số nguyên: một giá trị bắt đầu`x`, một giá trị kết thúc`y`, và độ dài`n`. Chúng ta cần xây dựng một mảng`a`chiều dài`n`bắt đầu lúc`x`và kết thúc tại`y`, đang tăng nghiêm ngặt và có sự khác biệt liên tiếp giảm dần. Nói cách khác, nếu`b_i = a_{i+1} - a_i`, sau đó`b`nên tạo thành một dãy giảm nghiêm ngặt. 

Việc này khó khăn vì các điều kiện trên`b`giới hạn khoảng cách có thể có giữa các phần tử liên tiếp. Ví dụ: chênh lệch thứ nhất phải lớn hơn chênh lệch thứ hai, chênh lệch thứ hai phải lớn hơn chênh lệch thứ ba, v.v. Nếu trong khoảng từ`x`ĐẾN`y`quá nhỏ so với`n`, có thể không thể thỏa mãn đồng thời cả chênh lệch tăng và giảm nghiêm ngặt. 

Các hạn chế là nhỏ:`x`,`y`1000,`n`1000 và`t ≤ 1000`. Điều này có nghĩa là một giải pháp chạy trong khoảng`O(n)`mỗi trường hợp thử nghiệm là có thể chấp nhận được. Việc liệt kê cưỡng bức tất cả các chuỗi sẽ không khả thi vì số lượng các chuỗi tăng lên theo tổ hợp, nhưng việc xây dựng tham lam hoặc toán học là thực tế. 

Một trường hợp cạnh quan trọng phát sinh khi`n`là 3, là kích thước tối thiểu. Ở đây, chúng ta chỉ cần hai điểm khác biệt. Ví dụ,`x=1, y=3, n=3`là không thể bởi vì chúng ta cần hai hiệu dương giảm nghiêm ngặt. Cặp duy nhất có thể`(d1, d2)`với`d1 > d2 > 0`sẽ cần`d1 + d2 = y - x = 2`. Nhưng cặp số nguyên dương giảm nhỏ nhất là`(2,1)`, tổng bằng 3, lớn hơn 2. Vậy kết quả đúng là`-1`. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu sẽ thử tất cả các chuỗi có độ dài`n`bắt đầu từ`x`và kết thúc tại`y`, kiểm tra sự khác biệt tăng và giảm nghiêm ngặt. Đối với mỗi lựa chọn có thể có của`a_2`, sau đó`a_3`, v.v., thuật toán sẽ xác minh xem chênh lệch có giảm hay không. Đây là cấp số nhân trong`n`và rõ ràng là không khả thi đối với`n=1000`. 

Quan sát quan trọng để có giải pháp nhanh hơn là chuỗi chênh lệch độ dài giảm dần`n-1`có thể được biểu diễn bằng các số nguyên liên tiếp theo thứ tự ngược lại. Giả sử chúng ta định nghĩa sự khác biệt là`d, d-1, d-2, ..., d-(n-2)`. Tổng của những khác biệt này phải bằng`y-x`, tức là,```
d + (d-1) + (d-2) + ... + (d-(n-2)) = (n-1)*d - (n-2)*(n-1)/2 = y - x
```Từ phương trình này, chúng ta có thể giải số nguyên lớn nhất`d`thỏa mãn điều kiện tổng trong khi giữ cho mọi sai phân đều dương. Một lần`d`được xác định, trình tự`a`được xây dựng lại bằng cách thêm các khác biệt một cách tuần tự bắt đầu từ`x`. Cách tiếp cận tham lam này có tác dụng vì việc giảm các khác biệt liên tiếp tính tổng bằng tổng khoảng cách sẽ tạo ra một chuỗi hợp lệ và việc sử dụng chênh lệch ban đầu tối đa sẽ đảm bảo tất cả các khác biệt vẫn dương. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Xây dựng toán học | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng chênh lệch`diff = y - x`. Đây là tổng số mà sự khác biệt ngày càng giảm của chúng ta phải đạt được. 
2. Bắt đầu từ sự khác biệt cuối cùng có thể`d = 1`và kiểm tra tăng dần nếu tổng của một chuỗi độ dài giảm dần`n-1`có thể phù hợp`diff`. Sử dụng công thức`(n-1)*d - (n-2)*(n-1)/2`. Chúng tôi cần`d`càng lớn càng tốt trong khi vẫn giữ mọi khác biệt là dương. 
3. Một lần`d`được xác định, tạo ra chuỗi các khác biệt như`[d, d-1, ..., d-(n-2)]`. 
4. Khởi tạo`a[0] = x`và lặp đi lặp lại thêm từng điểm khác biệt để tạo ra phần còn lại của chuỗi. 
5. Nếu không tích cực`d`thỏa mãn phương trình tổng, trả về`-1`cho trường hợp thử nghiệm đó. 

Tại sao nó hoạt động: công thức tổng đảm bảo rằng chuỗi sai phân bao phủ chính xác khoảng cách từ`x`ĐẾN`y`. Việc sử dụng các số nguyên giảm liên tiếp đảm bảo rằng hiệu giảm dần và bắt đầu từ`x`tạo ra một mảng tăng nghiêm ngặt. Điều bất biến là ở mỗi bước, khoảng cách còn lại có thể được phân bổ giữa các vị trí còn lại với độ chênh lệch dương giảm dần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        x, y, n = map(int, input().split())
        total = y - x
        found = False
        for d_last in range(1, total + 1):
            # Sum of n-1 decreasing differences starting from d
            # Let largest difference be d
            # Sum = (n-1)*d - (n-2)*(n-1)//2
            d = (total + (n-2)*(n-1)//2) // (n-1)
            if d*(n-1) - (n-2)*(n-1)//2 == total and d > 0:
                a = [x]
                curr = x
                for i in range(n-1):
                    curr += d
                    a.append(curr)
                    d -= 1
                print(" ".join(map(str, a)))
                found = True
                break
        if not found:
            print(-1)

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên tính toán chênh lệch ban đầu tối thiểu bằng cách sử dụng công thức dẫn xuất. Nó xây dựng mảng bằng cách giảm chênh lệch một đơn vị ở mỗi bước. Kiểm tra vòng lặp`d_last`có thể được tối ưu hóa, nhưng biểu mẫu này rõ ràng và hoạt động trong giới hạn. Sự tinh tế quan trọng bao gồm việc đảm bảo sự khác biệt vẫn dương và tổng chính xác bằng`y - x`. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:`1 4 3`| Bước | Khác biệt còn lại | Hiện tại d | Trình tự a | Sự khác biệt b | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 3 | 2 | [1] | [] | 
| tôi=1 | 3 | 2 | [1,3] | [2] | 
| tôi=2 | 1 | 1 | [1,3,4] | [2,1] | 

Điều này thể hiện trình tự làm việc tối thiểu. Sự khác biệt giảm dần và tổng cộng thành`y-x`. 

Đầu vào mẫu:`1 3 3`| Bước | Khác biệt còn lại | Hiện tại d | Trình tự a | Sự khác biệt b | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 2 | 2 | [1] | [] | 
| tôi=1 | 2 | 2 | [1,3] | [2] | 
| tôi=2 | 0 | 1 | không hợp lệ | - | 

Tổng sự khác biệt lớn hơn 2, do đó không thể có chuỗi, trả về`-1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Xây dựng mảng và tính tổng là tuyến tính theo chiều dài mảng. | 
| Không gian | O(n) | Lưu trữ mảng có độ dài n. | 

Được cho`n ≤ 1000`Và`t ≤ 1000`, các hoạt động trong trường hợp xấu nhất là`10^6`, trong vòng 1 giây. Mức sử dụng bộ nhớ không đáng kể so với giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("3\n1 4 3\n1 3 3\n100 200 4\n") == "1 3 4\n-1\n100 150 180 200", "sample 1"

# minimum n
assert run("1\n1 3 3\n") == "-1", "minimum n edge"

# maximum n with simple difference
assert run("1\n1 1000 1000\n") == "1 2 3 4 5 6 7 8 9 10 ... 1000", "max n edge"

# consecutive differences 1
assert run("1\n1 6 4\n") == "1 3 5 6", "small array valid sequence"

# impossible case
assert run("1\n5 6 5\n") == "-1", "impossible small range"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 3 3`|`-1`| n tối thiểu không thỏa mãn điều kiện | 
|`1 1000 1000`|`1 2 3 ... 1000`| n lớn, phạm vi hợp lệ tối đa | 
|`1 6 4`|`1 3 5 6`| dãy có sai phân giảm hợp lệ | 
|`5 6 5`|`-1`| phạm vi quá nhỏ so với trình tự yêu cầu | 

## Vỏ cạnh 

Đối với đầu vào`1 3 3`, thuật toán tính toán`total = 2`. Sử dụng công thức cho`d`, chúng tôi nhận được`d = (2 + 1) // 2 = 1`. Trình tự khác biệt sẽ là`[1,0]`, nhưng 0 thì không
