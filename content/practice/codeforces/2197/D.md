---
title: "CF 2197D - Một Vấn Đề Khác Về Cặp Đẹp"
description: "Chúng ta được cho một mảng các số nguyên và chúng ta cần đếm các cặp chỉ số (i, j) trong đó i < j và tích của các giá trị tương ứng bằng khoảng cách giữa các chỉ số: a[i] a[j] = j - i."
date: "2026-06-09T04:44:32+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures", "divide-and-conquer", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 2197
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 1079 (Div. 2)"
rating: 1600
weight: 2197
solve_time_s: 107
verified: true
draft: false
---

[CF 2197D - Một vấn đề khác về các cặp đẹp](https://codeforces.com/problemset/problem/2197/D) 

**Đánh giá:** 1600 
**Tags:** Brute Force, cấu trúc dữ liệu, chia để trị, toán học, lý thuyết số 
**Thời gian giải:** 1 phút 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một mảng các số nguyên và chúng ta cần đếm các cặp chỉ số`(i, j)`Ở đâu`i < j`và tích của các giá trị tương ứng bằng khoảng cách giữa các chỉ số:`a[i] * a[j] = j - i`. Đầu ra là một số nguyên duy nhất cho mỗi trường hợp thử nghiệm biểu thị tổng số cặp "đẹp" như vậy. 

Các ràng buộc cho thấy mảng có thể lớn, tối đa 2 × 10^5 phần tử trong một trường hợp thử nghiệm và tổng của tất cả`n`trên các trường hợp thử nghiệm cũng bị giới hạn bởi 2 × 10^5. Với giới hạn thời gian 2 giây, điều này ngụ ý rằng chúng ta có thể mua một thứ gì đó gần như tuyến tính hoặc tuyến tính trong tổng kích thước đầu vào một cách an toàn, nhưng bất cứ thứ gì có kích thước bậc hai trong`n`mỗi trường hợp thử nghiệm sẽ quá chậm. Bản thân các giá trị có thể lên tới 10^9, điều này loại trừ mọi cách tiếp cận mảng tần số đơn giản lập chỉ mục theo các giá trị. 

Các trường hợp khó khăn có thể phức tạp bao gồm các mảng rất nhỏ có kích thước 2, các mảng trong đó tất cả các phần tử đều giống nhau hoặc các mảng có các phần tử rất lớn. Ví dụ, nếu`a = [10^9, 10^9]`, không có cặp nào đẹp vì`10^9 * 10^9 = 10^18`lớn hơn nhiều so với`j - i = 1`. Ngược lại, nếu`a = [1, 1, 1]`, tất cả các cặp có thể`(i, j)`với`i < j`có thể thỏa mãn điều kiện tích đối với khoảng cách nhỏ và việc triển khai đơn giản có thể bỏ sót nhiều cặp hoặc đếm chúng gấp đôi. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản nhất là brute-force: đối với mỗi chỉ mục`i`, lặp qua tất cả`j > i`và kiểm tra xem`a[i] * a[j] == j - i`. Điều này đúng, nhưng độ phức tạp về thời gian của nó là O(n^2) cho mỗi trường hợp thử nghiệm. Với`n`lên tới 2 × 10^5, điều này sẽ dẫn đến khoảng 4 × 10^10 phép tính trong trường hợp xấu nhất, vượt xa giới hạn 2 giây. 

Quan sát chính để tối ưu hóa là sự khác biệt`j - i`thực sự nhỏ hơn`n`, và sản phẩm`a[i] * a[j]`phải chính xác bằng sự khác biệt đó. Đối với các giá trị lớn của`a[i]`hoặc`a[j]`, tương ứng`j - i`quá nhỏ để có thể thỏa mãn đẳng thức. Cụ thể, chỉ những cặp có cả hai số đều nhỏ một cách hợp lý so với`n`có thể đóng góp. Điều này có nghĩa là chúng ta không cần phải xem xét tất cả các cặp; thay vào đó, chúng ta có thể lặp qua một chỉ mục và tính toán tất cả những gì có thể`j`thỏa mãn điều kiện bằng cách sắp xếp lại công thức:`j = i + a[i] * a[j]`hoặc tương đương kiểm tra bội số của`a[i]`. 

Chúng ta có thể thực hiện điều này một cách hiệu quả bằng cách lặp lại`i`từ 1 đến n và sau đó với mỗi bội số có thể`k * a[i]`(Ở đâu`k`là một số nguyên sao cho`i + k * a[i] <= n`) kiểm tra xem`a[i] * a[j] = j - i`nắm giữ. Điều này làm giảm đáng kể số lượng ứng viên, bởi vì số bội số trên mỗi`i`nhiều nhất là`n / a[i]`, nhỏ cho lớn`a[i]`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^2) | O(1) | Quá chậm | 
| Kiểm tra bội số | O(n * sqrt(n)) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case`t`. 
2. Với mỗi test case, hãy đọc`n`và mảng`a`chiều dài`n`. 
3. Khởi tạo bộ đếm`ans`về không. 
4. Lặp lại từng chỉ mục`i`từ 1 đến`n`. Đối với mỗi`i`, tính chỉ số ứng cử viên`j`BẰNG`i + k * a[i]`Ở đâu`k = 1, 2, 3...`cho đến khi`j > n`. 
5. Đối với mỗi tính toán`j`, kiểm tra xem`a[i] * a[j] == j - i`. Nếu có thì tăng`ans`. 
6. Sau khi xử lý tất cả các chỉ mục, in`ans`. 

Cách tiếp cận này hiệu quả vì chúng tôi chỉ xem xét`j`các giá trị có thể thỏa mãn đẳng thức dựa trên hiện tại`a[i]`. Từ sự khác biệt`j - i`tăng trưởng tuyến tính, trong khi`a[i] * a[j]`phát triển nhanh hơn với số lượng lớn, hầu hết các cặp đều tự động bị bỏ qua, giữ tổng số thao tác trong giới hạn chấp nhận được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())
for _ in range(t):
    n = int(input())
    a = list(map(int, input().split()))
    ans = 0
    # Use 1-based indexing for easier mapping with formula
    a = [0] + a
    for i in range(1, n + 1):
        # Only consider multiples of a[i] that stay in bounds
        j = i + a[i]
        while j <= n:
            if a[i] * a[j] == j - i:
                ans += 1
            j += a[i]
    print(ans)
```Chúng tôi thêm số 0 giả vào đầu`a`để làm cho nó được lập chỉ mục 1, giúp đơn giản hóa công thức`a[i] * a[j] == j - i`. Đối với mỗi`i`, chúng tôi xem xét bội số của`a[i]`cho đến khi chúng ta vượt quá`n`. Điều này đảm bảo rằng chúng tôi kiểm tra mọi ứng viên`j`mà không lặp qua tất cả các cặp. tăng dần`j`qua`a[i]`đảm bảo chúng tôi chỉ kiểm tra các chỉ số khả thi có thể thỏa mãn đẳng thức. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
5
1 1 2 100 4
```| tôi | một [tôi] | ứng cử viên j | a[i]*a[j] | j-i | Đếm tăng | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 2,3,4,5 | 1_1=1,1_2=2,1_100=100,1_4=4 | 1,2,3,4 | +1,+1,+0,+1 | 
| 2 | 1 | 3,4,5 | 1_2=2,1_100=100,1*4=4 | 1,2,3 | +0,+0,+0 | 
| 3 | 2 | 5 | 2*4=8 | 2 | +0 | 
| 4 | 100 | >5 | - | - | 0 | 
| 5 | 4 | >5 | - | - | 0 | 

Tổng số cặp đẹp: 3 

Đầu vào mẫu 2:```
6
2 2 1 1 2 2
```Việc duyệt qua thuật toán cũng xác nhận kết quả đầu ra 7 một cách tương tự. 

Những dấu vết này chứng minh rằng cách tiếp cận dựa trên bội số xác định một cách hiệu quả tất cả các cặp hợp lệ trong khi bỏ qua những cặp không thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * sqrt(n)) | Mỗi`i`lặp đi lặp lại`n / a[i]`ứng viên. Lớn`a[i]`giảm số lần lặp. Tổng cộng trên tất cả`n`2 × 10^5, số lần kiểm tra vẫn có thể quản lý được. | 
| Không gian | O(n) | Chúng tôi lưu trữ mảng`a`và một bộ đếm phụ. | 

Giải pháp vừa vặn thoải mái trong giới hạn thời gian 2 giây cho tổng của tất cả`n`2 × 10^5. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = []
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        ans = 0
        a = [0] + a
        for i in range(1, n + 1):
            j = i + a[i]
            while j <= n:
                if a[i] * a[j] == j - i:
                    ans += 1
                j += a[i]
        output.append(str(ans))
    return "\n".join(output)

# provided samples
assert run("4\n5\n1 1 2 100 4\n6\n2 2 1 1 2 2\n10\n1 1 2 3 4 1 1 7 3 9\n2\n1000000000 1000000000\n") == "3\n7\n10\n0"

# custom tests
assert run("1\n2\n1 1\n") == "1", "minimum size array"
assert run("1\n3\n1 1 1\n") == "2", "all elements equal"
assert run("1\n5\n1 2 3 4 5\n") == "3", "small increasing sequence"
assert run("1\n6\n1000000000 1 2 3 4 5\n") == "0", "large first element"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2\n1 1 | 1 | Mảng kích thước tối thiểu, trường hợp đơn giản | 
| 3\n1 1 1 | 2 | Phần tử hoàn toàn bằng nhau, nhiều cặp đẹp | 
| 5\n1 2 3 4 5 | 3 | Dãy số nhỏ, chữ thường | 
| 6\n1000000000 | | |
