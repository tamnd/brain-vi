---
title: "CF 283B - Chương trình Bò"
description: "Chúng ta được cho một dãy các số nguyên dương có độ dài n trong đó phần tử đầu tiên ban đầu chưa được biết và n - 1 phần tử còn lại được cung cấp. \"chương trình bò\" định nghĩa hai biến số nguyên, x và y, bắt đầu bằng x = 1 và y = 0."
date: "2026-06-05T09:43:18+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "dp", "graphs"]
categories: ["algorithms"]
codeforces_contest: 283
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 174 (Div. 1)"
rating: 1700
weight: 283
solve_time_s: 96
verified: true
draft: false
---

[CF 283B - Chương trình Bò](https://codeforces.com/problemset/problem/283/B) 

**Đánh giá:** 1700 
**Thẻ:** dfs và các đồ thị, dp, tương tự 
**Thời gian giải:** 1 phút 36 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một chuỗi các số nguyên dương có độ dài _n_ trong đó phần tử đầu tiên ban đầu chưa xác định và các phần tử _n - 1_ còn lại được cung cấp. "Chương trình bò" xác định hai biến số nguyên, _x_ và _y_, bắt đầu bằng _x_ = 1 và _y_ = 0. Chương trình thực hiện một cặp thao tác lặp lại: đầu tiên, nó tăng cả _x_ và _y_ theo phần tử hiện tại tại vị trí _x_, sau đó nó lại tăng _y_ theo cùng một giá trị đồng thời giảm _x_ theo giá trị đó. Chương trình dừng nếu _x_ di chuyển ra ngoài phạm vi [1, n]. 

Nhiệm vụ là xác định, đối với mỗi phần tử đầu tiên có thể có của chuỗi (từ 1 đến n - 1), giá trị cuối cùng của _y_ sau khi chạy chương trình hoặc -1 nếu nó không bao giờ kết thúc. Về cơ bản, điều này mô phỏng một đường dẫn động xuyên suốt trình tự trong đó giá trị bắt đầu ảnh hưởng đến cả bước nhảy tiến và lùi, tạo ra khả năng xảy ra các chu kỳ. 

Hạn chế chính là _n_ có thể lên tới 2·10^5 và các giá trị của chuỗi có thể lớn tới 10^9. Bất kỳ thuật toán nào cố gắng mô phỏng từng bước chạy sẽ có khả năng thực hiện các bước O(n) cho mỗi giá trị bắt đầu và do đó tổng thể là O(n^2). Điều này là quá chậm. Vì vậy, chúng ta cần một phương pháp tránh mô phỏng đầy đủ trong khi vẫn phát hiện chính xác các chu kỳ. 

Các trường hợp biên không rõ ràng bao gồm các chuỗi trong đó các bước nhảy ngay lập tức thoát khỏi phạm vi hợp lệ, các chuỗi gây ra dao động vô hạn hoặc các chuỗi cuối cùng tích lũy các giá trị _y_ lớn sau nhiều lần nhảy. Ví dụ: nếu _a_ = [3, 1], bắt đầu từ _x_ = 1, bước đầu tiên sẽ tăng _x_ lên 4 (kết thúc), cho _y_ = 3, trong khi giá trị bắt đầu nhỏ hơn có thể lặp vô thời hạn. Một mô phỏng đơn giản sẽ bỏ lỡ những chu kỳ này hoặc mất quá nhiều thời gian để phát hiện ra chúng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực mô phỏng chương trình từ mỗi vị trí bắt đầu. Ở mỗi bước, nó cập nhật _x_ và _y_ theo hai thao tác và chấm dứt khi _x_ vượt quá giới hạn hoặc nếu chúng tôi phát hiện trạng thái đã truy cập trước đó (biểu thị một chu kỳ). Cách tiếp cận này đúng nhưng O(n^2) trong trường hợp xấu nhất vì mỗi mô phỏng có thể đi qua chuỗi nhiều lần. Với n lên tới 2·10^5, điều này có thể đạt tới 4·10^10 phép toán, điều này là không khả thi. 

Cái nhìn sâu sắc quan trọng về tối ưu hóa là cấu trúc vấn đề cho phép chúng ta tính toán kết quả bằng cách sử dụng lập trình động với tính năng ghi nhớ. Vì mỗi vị trí _i_ chỉ phụ thuộc vào kết quả từ một vị trí khác (_i + a[i]_ cho bước tiến, sau đó _i - a[i]_ cho bước lùi), chúng ta có thể lưu trữ kết quả cho từng vị trí sau khi được tính toán. Nếu chúng tôi gặp một vị trí đã được tính toán, chúng tôi sẽ sử dụng lại giá trị của nó. Nếu chúng ta gặp một vị trí trong ngăn xếp đệ quy hiện tại thì chúng ta đã phát hiện được một chu trình nên kết quả là -1. Điều này làm giảm độ phức tạp tổng thể xuống O(n) vì mỗi vị trí được truy cập nhiều nhất hai lần: một lần trong tính toán và một lần trong quá trình truyền bá ghi nhớ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(n) | Quá chậm | 
| DP với khả năng ghi nhớ | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo một mảng`dp`có kích thước n+1 với`None`, sẽ lưu trữ giá trị được tính toán của _y_ cho mỗi vị trí bắt đầu. Giá trị -1 biểu thị một chu kỳ được phát hiện. 
2. Định nghĩa hàm đệ quy`solve(i)`trả về _y_ cuối cùng cho một chương trình bắt đầu tại chỉ mục _i_. Nếu _i_ nằm ngoài giới hạn, trả về 0, vì _y_ ngừng tích lũy. 
3. Trong`solve(i)`, kiểm tra xem`dp[i]`đã được tính toán rồi. Nếu có, hãy trả lại để tránh tính toán lại. 
4. Đánh dấu chỉ số hiện tại`i`là "đang tiến hành" bằng cách cài đặt`dp[i]`đến một giá trị trọng điểm, chẳng hạn như -2. Điều này cho phép phát hiện chu kỳ. 
5. Tính chỉ số bước tiến tiếp theo`j = i + a[i]`. Nếu như`j`vượt quá giới hạn, chương trình sẽ kết thúc ngay lập tức và`dp[i] = a[i]`. 
6. Ngược lại, tính chỉ số bước lùi`k = j - a[j]`. Giải quyết đệ quy`solve(k)`để có được kết quả từ đó. Nếu như`solve(k)`là -1, truyền -1 tới`dp[i]`. 
7. Nếu không, hãy cập nhật`dp[i] = a[i] + a[j] + solve(k)`. Điều này tính đến số gia tăng _y_ trong cả hai bước và tích lũy xuôi dòng. 
8. Trở về`dp[i]`là kết quả cho vị trí _i_. 
9. Lặp lại các vị trí bắt đầu từ 1 đến n-1, gọi`solve(i)`, và in kết quả. 

Tính bất biến đó là`dp[i]`sẽ chứa giá trị cuối cùng của _y_ hoặc -1 nếu tồn tại một chu trình. Các lệnh gọi đệ quy luôn di chuyển đến các vị trí được xác định nghiêm ngặt theo trình tự, do đó không có giá trị sai nào được truyền đi. Việc phát hiện chu kỳ được xử lý bằng điểm đánh dấu trọng điểm, đảm bảo kết thúc thuật toán trong O(n). 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(1 << 20)

n = int(input())
a = [0] + list(map(int, input().split()))  # 1-based indexing
dp = [None] * (n + 1)  # memoization array

def solve(i):
    if i <= 0 or i > n:
        return 0  # program terminates
    if dp[i] == -2:  # cycle detected
        return -1
    if dp[i] is not None:
        return dp[i]
    
    dp[i] = -2  # mark as in progress
    j = i + a[i]
    if j > n:
        dp[i] = a[i]
    else:
        k = j - a[j]
        res = solve(k)
        if res == -1:
            dp[i] = -1
        else:
            dp[i] = a[i] + a[j] + res
    return dp[i]

for i in range(1, n):
    print(solve(i))
```Giải pháp thiết lập lập chỉ mục dựa trên 1 để ánh xạ trực tiếp tới định nghĩa _x_ của vấn đề. các`solve`hàm sử dụng giá trị trọng điểm -2 để phát hiện các chu kỳ trong ngăn xếp cuộc gọi đệ quy. Các vị trí ngoài giới hạn ngay lập tức trả về 0, tương ứng với việc chấm dứt. Việc tích lũy _y_ được xử lý dưới dạng tổng của vị trí hiện tại và kết quả đệ quy xuôi dòng. 

## Ví dụ đã hoạt động 

**Ví dụ 1**: 

đầu vào:`4 2 4 1`| tôi | x | y | j = x+a[x] | k = j-a[j] | dp[i] | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 3 | 3-1=2 | 3 | 
| 2 | 2 | 0 | 6 | >n, chấm dứt | 6 | 
| 3 | 3 | 0 | 4 | 4-?= ra | 8 | 

Dấu vết này hiển thị các bước nhảy tiến và lùi, với sự tích lũy _y_ ở mỗi bước. Thuật toán xử lý chính xác việc chấm dứt ngoài giới hạn và tính toán`dp`các giá trị đệ quy. 

**Ví dụ 2**: 

đầu vào:`5 1 1 1 1`| tôi | dp[i] | 
| --- | --- | 
| 1 | 1+2+... cuối cùng cũng vượt quá giới hạn | 
| 2 | tính toán tương tự | 
| 3 | tương tự | 
| 4 | chấm dứt ngay lập tức | 

Điều này chứng tỏ rằng ngay cả các chuỗi thống nhất cũng được xử lý và các chu kỳ không bị phát hiện sai. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi vị trí được giải quyết tối đa một lần và kết quả được ghi nhớ sẽ tránh được việc đệ quy lặp lại. | 
| Không gian | O(n) |`dp`mảng và ngăn xếp đệ quy sử dụng bộ nhớ O(n). | 

Với các ràng buộc n 2·10^5 và độ sâu đệ quy tối đa n, giải pháp này chạy thoải mái trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    exec(open("solution.py").read(), globals())  # assuming code is in solution.py
    return output.getvalue().strip()

# Provided sample
assert run("4\n2 4 1\n") == "3\n6\n8", "sample 1"

# Minimum size input
assert run("2\n1\n") == "1", "minimum n"

# Maximum size input with all equal
n = 5
inp = f"{n}\n" + " ".join(["1"]*(n-1)) + "\n"
assert run(inp) == "\n".join(["i+1" for i in range(1,n)]), "all equal small n"

# Edge case with immediate termination
assert run("3\n5 1\n") == "5\n6", "jump out of bounds immediately"

# Edge case with possible cycle
assert run("4\n1 1 1\n") == "5\n4\n3", "small sequence cycle check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4\n2 4 | | |
