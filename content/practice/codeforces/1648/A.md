---
title: "CF 1648A - Tổng lẻ"
description: "Chúng ta có một lưới 2D có kích thước $n nhân m$ trong đó mỗi ô chứa một màu được biểu thị bằng một số nguyên. Nhiệm vụ là tính tổng khoảng cách Manhattan giữa mỗi cặp ô có cùng màu."
date: "2026-06-10T03:58:43+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "data-structures", "geometry", "math", "matrices", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1648
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 775 (Div. 1, based on Moscow Open Olympiad in Informatics)"
rating: 1400
weight: 1648
solve_time_s: 72
verified: true
draft: false
---

[CF 1648A - Tổng kỳ lạ](https://codeforces.com/problemset/problem/1648/A) 

**Đánh giá:** 1400 
**Tags:** tổ hợp, cấu trúc dữ liệu, hình học, toán, ma trận, sắp xếp 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một lưới kích thước 2D$n \times m$trong đó mỗi ô chứa một màu được biểu thị bằng một số nguyên. Nhiệm vụ là tính tổng khoảng cách Manhattan giữa mỗi cặp ô có cùng màu. Khoảng cách Manhattan giữa hai ô$(r_1, c_1)$Và$(r_2, c_2)$là$|r_1 - r_2| + |c_1 - c_2|$, đếm số bước dọc và ngang cần thiết để di chuyển từ ô này sang ô khác. 

Đầu vào bao gồm$n$hàng với$m$mỗi số nguyên và tổng số ô không vượt quá 100.000. Điều này ngụ ý rằng chúng ta không thể có được giải pháp lặp lại trực tiếp trên tất cả các cặp ô, vì điều đó có thể dẫn đến kết quả gần như$(10^5)^2 / 2 = 5 \cdot 10^9$hoạt động, vượt xa giới hạn thời gian 2 giây. Một cách tiếp cận tuyến tính hoặc gần tuyến tính là cần thiết. 

Các trường hợp cạnh bao gồm các lưới trong đó tất cả các ô có cùng màu, lưới chỉ có một hàng hoặc cột hoặc lưới trong đó mỗi ô có một màu riêng biệt. Một phương pháp brute-force bất cẩn sẽ cố gắng tính tổng khoảng cách trên tất cả các cặp một cách mù quáng, điều này sẽ quá chậm đối với đầu vào lớn nhất. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản lặp lại từng màu, thu thập tất cả các vị trí của màu đó và tính tổng khoảng cách Manhattan trên tất cả các cặp. Nếu một màu xuất hiện$k$nhiều khi điều này đòi hỏi$O(k^2)$hoạt động cho mỗi màu. Trong trường hợp xấu nhất, nếu tất cả 100.000 ô có cùng màu thì phương pháp này yêu cầu$O(10^{10})$những hoạt động không thực tế. 

Thông tin chi tiết quan trọng là khoảng cách Manhattan có thể tách thành các phần đóng góp theo hàng và cột:$|r_1 - r_2| + |c_1 - c_2| = |r_1 - r_2| + |c_1 - c_2|$. Vì vậy, chúng ta có thể tính tổng hàng và tổng cột một cách độc lập. Nếu chúng ta sắp xếp tất cả các chỉ số hàng của một màu và tính tổng hiện có thì tổng khoảng cách do các hàng đóng góp có thể được tính theo thời gian tuyến tính tương ứng với số lần xuất hiện. Điều tương tự cũng áp dụng cho các chỉ số cột. Cụ thể, sau khi sắp xếp, đối với từng vị trí$i$, tổng khoảng cách đến tất cả các vị trí trước đó là$i \cdot \text{row}_i - \text{sum of previous rows}$. Tính tổng số này trên tất cả các vị trí sẽ cho ra tổng đóng góp của hàng và tương tự cho các cột. 

Điều này làm giảm độ phức tạp về thời gian từ$O(k^2)$mỗi màu để$O(k \log k)$để sắp xếp cộng$O(k)$để tính tổng. Cho rằng tổng của tất cả$k$trên tất cả các màu tối đa là 100.000, điều này hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((nm)^2) | O(nm) | Quá chậm | 
| Tối ưu | O(nm log nm) | O(nm) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo một từ điển ánh xạ từng màu vào hai danh sách: chỉ mục hàng và chỉ mục cột của tất cả các lần xuất hiện của màu đó. Điều này phân tách các đóng góp hàng và cột và sắp xếp các vị trí theo màu sắc. 
2. Đối với mỗi màu, hãy sắp xếp danh sách các chỉ số hàng. Việc sắp xếp là cần thiết vì công thức tính tổng khoảng cách hoạt động theo trình tự có thứ tự. 
3. Khởi tạo tổng khoảng cách cho các hàng. Lặp lại các chỉ số hàng được sắp xếp. Đối với chỉ số hiện tại$i$, thêm vào$i \cdot \text{row}_i - \text{sum of previous rows}$đến tổng số. Điều này tính toán tổng của$|r_i - r_j|$cho tất cả trước đó$j < i$trong thời gian tuyến tính. 
4. Lặp lại các bước 2-3 cho các chỉ số cột, tính tổng các phần đóng góp cho cùng một biến tổng. 
5. Sau khi xử lý tất cả các màu, hãy in tổng tích lũy. Đây là tổng khoảng cách Manhattan giữa tất cả các cặp ô có cùng màu. 

Tại sao nó hoạt động: Việc phân tách thành các hàng và cột thúc đẩy tính tuyến tính của khoảng cách Manhattan. Việc sắp xếp đảm bảo rằng chúng ta có thể tính toán sự khác biệt tích lũy một cách hiệu quả. Công thức$i \cdot x_i - \text{sum of previous}$được bắt nguồn từ sự quan sát rằng$\sum_{j=0}^{i-1} |x_i - x_j| = i \cdot x_i - \sum_{j=0}^{i-1} x_j$cho một chuỗi được sắp xếp, đảm bảo rằng mỗi cặp được tính chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import defaultdict

def main():
    n, m = map(int, input().split())
    color_positions = defaultdict(lambda: ([], []))  # maps color -> (rows, cols)
    
    for r in range(n):
        row = list(map(int, input().split()))
        for c, color in enumerate(row):
            color_positions[color][0].append(r)
            color_positions[color][1].append(c)
    
    total_sum = 0
    for rows, cols in color_positions.values():
        for indices in (rows, cols):
            indices.sort()
            acc = 0
            running_sum = 0
            for i, val in enumerate(indices):
                acc += i * val - running_sum
                running_sum += val
            total_sum += acc
    
    print(total_sum)

if __name__ == "__main__":
    main()
```Từ điển thu thập các chỉ số hàng và cột riêng biệt cho từng màu, cho phép tính tổng độc lập. Sắp xếp đảm bảo công thức được áp dụng chính xác.`running_sum`tích lũy tổng các chỉ số trước đó cho từng thứ nguyên, giúp tránh tính toán lại tổng nhiều lần. Phương pháp này xử lý tất cả các kích thước của lưới và màu sắc mà không vượt quá giới hạn về bộ nhớ hoặc thời gian. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
2 3
1 2 3
3 2 1
```| Màu sắc | Hàng | Cols | Hàng được sắp xếp | Đóng góp hàng | Cols được sắp xếp | Col đóng góp | Tổng đóng góp | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | [0,1] | [0,2] | [0,1] | 1 | [0,2] | 2 | 3 | 
| 2 | [0,1] | [1,1] | [0,1] | 1 | [1,1] | 0 | 1 | 
| 3 | [0,1] | [2,0] | [0,1] | 1 | [0,2] | 2 | 3 | 

Tổng cộng: 3 + 1 + 3 = 7, phù hợp với sản lượng dự kiến. 

Ví dụ thứ hai với một màu tô vào lưới:```
2 2
5 5
5 5
```| Màu sắc | Hàng | Cols | Đóng góp hàng | Col đóng góp | 
| --- | --- | --- | --- | --- | 
| 5 | [0,0,1,1] | [0,1,0,1] | 4 | 4 | 

Khoảng cách Manhattan giữa tất cả sáu cặp: tổng khoảng cách bằng 8, khớp với tính toán của chúng tôi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm log nm) | Sắp xếp chỉ số hàng và cột cho từng màu chiếm ưu thế; tính toán tổng là tuyến tính. | 
| Không gian | O(nm) | Lưu trữ vị trí cho mỗi màu yêu cầu O (số ô). | 

Với$nm \le 10^5$, việc sắp xếp chiếm ưu thế nhưng vừa vặn thoải mái trong vòng 2 giây. Việc sử dụng bộ nhớ vẫn ở dưới giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    main()
    return output.getvalue().strip()

# Provided sample
assert run("2 3\n1 2 3\n3 2 1\n") == "7", "sample 1"

# Minimum size
assert run("1 1\n1\n") == "0", "single cell"

# All cells same
assert run("2 2\n5 5\n5 5\n") == "8", "all same color"

# Row only
assert run("1 4\n1 2 1 2\n") == "4", "single row"

# Column only
assert run("4 1\n1\n2\n1\n2\n") == "4", "single column"

# Maximum distinct
assert run("2 3\n1 2 3\n4 5 6\n") == "0", "all distinct colors"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Lưới 1x1 | 0 | Ô đơn, không có cặp | 
| 2x2 đều giống nhau | 8 | Nhiều cặp, xác minh phép tính tổng | 
| 1x4 hỗn hợp | 4 | Khoảng cách ngang, đóng góp hàng | 
| 4x1 hỗn hợp | 4 | Khoảng cách dọc, đóng góp cột | 
| 2x3 tất cả đều khác biệt | 0 | Không có cặp, đảm bảo tổng bằng 0 | 

## Vỏ cạnh 

Đối với lưới trong đó tất cả các ô có cùng màu, chẳng hạn như:```
2 2
5 5
5 5
```Hàng: [0,0,1,1], Cols: [0,1,0,1]. Sắp xếp hàng và cột, thuật toán tính toán đóng góp hàng: 1_0-0=0, 2_0-0=0, 3*
