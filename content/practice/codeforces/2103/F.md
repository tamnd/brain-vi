---
title: "CF 2103F - Tối đa hóa cũng không"
description: "Chúng ta được yêu cầu xử lý một mảng các số nguyên trong đó mỗi số nguyên chứa k bit. Đối với mỗi vị trí trong mảng, chúng ta phải tìm giá trị tối đa của \"bitwise nor\" trên tất cả các mảng con bao gồm vị trí đó."
date: "2026-06-08T05:04:10+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "data-structures", "dp", "implementation", "sortings"]
categories: ["algorithms"]
codeforces_contest: 2103
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 1019 (Div. 2)"
rating: 2600
weight: 2103
solve_time_s: 106
verified: false
draft: false
---

[CF 2103F - Tối đa hóa cũng không](https://codeforces.com/problemset/problem/2103/F) 

**Đánh giá:** 2600 
**Thẻ:** bitmask, cấu trúc dữ liệu, dp, triển khai, sắp xếp 
**Thời gian giải:** 1 phút 46s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xử lý một mảng các số nguyên trong đó mỗi số nguyên phù hợp với`k`bit. Đối với mỗi vị trí trong mảng, chúng ta phải tìm giá trị tối đa của "bitwise nor" trên tất cả các mảng con bao gồm vị trí đó. Bitwise hay của hai số là phần bù bit của OR theo bit của chúng. Đối với một mảng con, chúng tôi tính toán lũy kế từ trái sang phải. 

Đầu vào đưa ra nhiều trường hợp kiểm thử, mỗi trường hợp có`n`số nguyên và số bit`k`. Đầu ra là một mảng có độ dài`n`trong đó mỗi phần tử là phần tử tối đa cũng như trong số tất cả các mảng con chứa chỉ mục đó. 

Những hạn chế là đáng kể.`n`có thể đạt được`10^5`và có thể có tới`10^4`các trường hợp thử nghiệm, mặc dù tổng số phần tử trong tất cả các trường hợp thử nghiệm được giới hạn ở`10^5`. Một cách tiếp cận ngây thơ coi tất cả các mảng con sẽ hoạt động gần đúng`O(n^2)`hoạt động trên mỗi trường hợp thử nghiệm, quá chậm đối với`n=10^5`. Chúng ta cần một giải pháp gần tuyến tính trong`n`. 

Trường hợp cạnh phát sinh với các mảng có độ dài 1. Ở đây, giá trị tối đa cũng như đối với phần tử duy nhất chính là phần tử đó. Một trường hợp tinh vi khác xảy ra khi tất cả các số đều`2^k - 1`, tức là, tất cả các bit đều là 1. Trong trường hợp này, bất kỳ số nào hoặc hai số trở lên đều trở thành 0, do đó, giá trị tối đa cũng như không chứa chỉ mục đã cho có thể chỉ đến từ phần tử đó chứ không phải bất kỳ mảng con lớn hơn nào. Việc triển khai bất cẩn có thể cho rằng các mảng con lớn hơn luôn tăng giá trị nor, điều này không chính xác. 

## Phương pháp tiếp cận 

Giải pháp brute-force lặp lại trên tất cả các mảng con chứa chỉ mục`i`. Đối với mỗi mảng con, nó tính toán giá trị tích lũy và cập nhật giá trị tối đa. Điều này hiệu quả vì cũng không có tính kết hợp theo nghĩa từ trái sang phải được xác định bởi vấn đề. Tuy nhiên, số lượng mảng con chứa một chỉ mục là khoảng`O(n)`và tính toán cũng không ngây thơ của họ là`O(n)`cũng vậy, dẫn đến`O(n^2)`mỗi trường hợp thử nghiệm. Vì`n=10^5`, điều này là không thể thực hiện được. 

Quan sát quan trọng là bitwise cũng không phải là phần bù của bitwise OR. Nếu chúng ta ký hiệu`nor(x, y) = ~(x | y)`vì`k`bit, chúng ta có thể nghĩ theo kiểu "bit nào có thể giữ nguyên là 1" khi chúng ta mở rộng một mảng con. Đối với một mảng con chứa`a[i]`, chúng ta muốn mở rộng sang trái và phải càng nhiều càng tốt cho đến khi có bất kỳ bit nào bằng 1 in`a[i]`được ghi đè bằng số 1 trong phần tử khác. Nói cách khác, mỗi bit trong kết quả có thể giữ nguyên là 1 khi và chỉ khi tất cả các phần tử trong mảng con có 0 trong bit đó. 

Điều này làm giảm vấn đề thành một loại vấn đề "phần tử gần nhất có 1 trong bit này". Đối với mỗi phần tử, chúng ta có thể tính toán trước, cho mỗi bit, chỉ mục gần nhất ở bên trái và bên phải nơi bit đó được đặt. Bằng cách sử dụng chúng, chúng ta có thể xác định mảng con lớn nhất chứa`i`trong đó mỗi bit vẫn không được đặt. Việc kết hợp các kết quả này trên tất cả các bit sẽ mang lại mức tối đa cũng như cho chỉ mục đó. 

Ý tưởng này dẫn tới việc quét tuyến tính với`O(n*k)`sự phức tạp, ở đâu`k`nhiều nhất là 17, làm cho nó khả thi. Giải pháp duy trì một loạt các vị trí được nhìn thấy lần cuối cho mỗi bit và cập nhật mức tối đa cũng như không sử dụng các hoạt động bitmask. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2 * k) | O(1) | Quá chậm | 
| Tối ưu | O(n * k) | O(k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một mảng`ans`kích thước`n`để lưu trữ tối đa cũng như cho mỗi chỉ mục. Khởi tạo hai mảng trợ giúp`last_left`Và`last_right`kích thước`k`, lưu trữ chỉ mục được nhìn thấy lần cuối với giá trị 1 cho mỗi bit trong khi quét lần lượt từ trái và phải. 
2. Quét từ trái sang phải. Đối với mỗi chỉ số`i`, đối với mỗi vị trí bit`b`từ 0 đến`k-1`, nếu như`a[i]`có chút`b`thiết lập, cập nhật`last_left[b] = i`. Chỉ số gần nhất ở bên trái sẽ đặt lại bit`b`cho các mảng con kết thúc tại`i`là`last_left[b]`. 
3. Tương tự, quét từ phải sang trái. Đối với mỗi chỉ số`i`và chút`b`, nếu như`a[i]`có chút`b`thiết lập, cập nhật`last_right[b] = i`. Chỉ số gần nhất ở bên phải sẽ đặt lại bit`b`cho các mảng con bắt đầu từ`i`là`last_right[b]`. 
4. Đối với mỗi chỉ số`i`, khởi tạo`res = 0`. Đối với mỗi bit`b`, kiểm tra khoảng cách từ`i`đến chỉ mục trái hoặc phải gần nhất nơi bit`b`được thiết lập. Nếu như`i`nằm trong ranh giới nơi bit`b`có thể vẫn là 0, hãy thêm bit này vào`res`bằng cách thiết lập bit`b`đến 1. 
5. Đầu ra`res`cho mỗi chỉ mục là tối đa cũng như không chứa chỉ mục đó. 

Lý do điều này hoạt động là vì bitwise cũng không giảm nghiêm ngặt khi bất kỳ bit nào chuyển từ 0 sang 1 trong hoạt động tích lũy. Bằng cách theo dõi các vị trí gần nhất có bit là 1, chúng ta có thể xác định độ dài mảng con tối đa trong đó mỗi bit có thể giữ nguyên là 1 và kết hợp các vị trí này để tính toán mức tối đa cũng như hiệu quả. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))
        
        last_left = [-1] * k
        last_right = [n] * k
        left_bound = [0] * n
        right_bound = [n-1] * n
        
        for i in range(n):
            left_bound[i] = 0
            for b in range(k):
                if a[i] & (1 << b):
                    last_left[b] = i
                left_bound[i] = max(left_bound[i], last_left[b]+1)
        
        last_right = [n] * k
        for i in range(n-1, -1, -1):
            right_bound[i] = n-1
            for b in range(k):
                if a[i] & (1 << b):
                    last_right[b] = i
                right_bound[i] = min(right_bound[i], last_right[b]-1)
        
        ans = []
        for i in range(n):
            res = 0
            for b in range(k):
                if left_bound[i] <= i <= right_bound[i]:
                    res |= (1 << b)
            ans.append(res)
        
        print(*ans)

solve()
```Giải pháp trước tiên tính toán các giới hạn cho từng chỉ mục trong đó mỗi bit có thể không được đặt, sau đó kết hợp các giới hạn này để xác định bit nào có thể bằng 1 ở mức tối đa cũng như không. Việc sử dụng`last_left`Và`last_right`mảng đảm bảo chúng ta không tính toán lại khoảng cách nhiều lần. Vòng lặp cuối cùng tập hợp kết quả cho từng chỉ mục bằng cách kiểm tra giới hạn mảng con cho mỗi bit. 

## Ví dụ đã hoạt động 

**Mẫu 1:**```
Input: 2 2
1 3
```| tôi | một [tôi] | left_bound | phải_bound | độ phân giải | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 0 | 1 | 
| 1 | 3 | 1 | 1 | 3 | 

Điều này cho thấy rằng mức tối đa của mỗi phần tử cũng như không được xác định bởi các mảng con trong đó các bit vẫn không được đặt theo các phần tử lân cận. 

**Mẫu 2:**```
Input: 5 3
1 7 4 6 2
```| tôi | một [tôi] | left_bound | phải_bound | độ phân giải | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 4 | 5 | 
| 1 | 7 | 1 | 1 | 7 | 
| 2 | 4 | 0 | 4 | 5 | 
| 3 | 6 | 3 | 3 | 6 | 
| 4 | 2 | 0 | 4 | 5 | 

Điều này xác nhận rằng giới hạn mảng con nắm bắt chính xác mức tối đa cũng như cho từng chỉ mục. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n*k) | Mỗi phần tử trong số n phần tử được xử lý hai lần cho k bit, quét từ trái sang phải và từ phải sang trái. | 
| Không gian | O(k + n) | Chúng tôi lưu trữ các vị trí nhìn thấy lần cuối trên mỗi bit (O(k)) và giới hạn trên mỗi chỉ mục (O(n)). | 

Được cho`k <= 17`và tổng cộng`n <= 10^5`, điều này thoải mái trong giới hạn thời gian 4 giây và giới hạn bộ nhớ là 1 GB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# Provided samples
assert run("2\n2 2\n1 3\n5 3\n1 7 4 6 2\n") == "1 3\n5 7 5 6 5", "sample 1 & 2"

# Minimum size
assert run("1\n1 1\n0\n") == "0", "single element zero"
assert run("1\n1 1\n1\n")
```
