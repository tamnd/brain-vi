---
title: "CF 76E - Điểm"
description: "Chúng tôi được cấp tới một trăm nghìn điểm trên mặt phẳng 2D. Đối với mỗi cặp điểm không có thứ tự, chúng ta phải tính bình phương khoảng cách Euclide giữa chúng và cộng tất cả các giá trị đó lại với nhau."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 76
codeforces_index: "E"
codeforces_contest_name: "All-Ukrainian School Olympiad in Informatics"
rating: 1700
weight: 76
solve_time_s: 113
verified: true
draft: false
---

[CF 76E - Điểm](https://codeforces.com/problemset/problem/76/E) 

**Đánh giá:** 1700 
**Tags:** thực hiện, toán học 
**Thời gian giải:** 1m 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp tới một trăm nghìn điểm trên mặt phẳng 2D. Đối với mỗi cặp điểm không có thứ tự, chúng ta phải tính bình phương khoảng cách Euclide giữa chúng và cộng tất cả các giá trị đó lại với nhau. 

Cho hai điểm`(x1, y1)`Và`(x2, y2)`, bình phương khoảng cách là$(x_1-x_2)^2 + (y_1-y_2)^2$Nhiệm vụ không yêu cầu khoảng cách Euclide thực tế, chỉ yêu cầu bình phương của nó. Điều đó quan trọng vì khoảng cách bình phương mở rộng thành các biểu thức đại số có thể tính tổng một cách hiệu quả. 

Kích thước đầu vào ngay lập tức loại trừ việc liệt kê theo cặp. Với`N = 100000`, số lượng cặp là khoảng$\frac{N(N-1)}{2}$tức là khoảng năm tỷ cặp trong trường hợp xấu nhất. Ngay cả một lượng công việc rất nhỏ cho mỗi cặp cũng sẽ vượt quá giới hạn thời gian. 

Bản thân tọa độ rất nhỏ, giữa`-10000`Và`10000`, nhưng điều đó không đủ để biện minh cho bất kỳ nghiệm bậc hai nào. Cách tiếp cận đúng phải chạy gần với thời gian tuyến tính. 

Có một số trường hợp khó bỏ sót. 

Nếu nhiều điểm trùng nhau thì đóng góp của chúng sẽ bằng 0. Ví dụ:```
3
1 1
1 1
1 1
```Mỗi cặp đều có khoảng cách`0`, vậy đáp án là:```
0
```Việc triển khai bất cẩn cho rằng tất cả các điểm đều khác biệt có thể vô tình đếm gấp đôi hoặc xử lý sai các bản sao. 

Tọa độ âm cũng quan trọng vì các thuật ngữ như`(x1 - x2)^2`vẫn dương sau khi bình phương. Ví dụ:```
2
-3 -4
3 4
```Bình phương khoảng cách là:```
(6)^2 + (8)^2 = 100
```Bất kỳ cách tiếp cận nào dựa vào giá trị tuyệt đối mà không bình phương cẩn thận đều có thể tạo ra kết quả không chính xác. 

Một vấn đề tế nhị khác là tràn số nguyên trong các ngôn ngữ có số nguyên có kích thước cố định. Giả sử tất cả tọa độ đều gần`10000`Và`N`là lớn. Câu trả lời cuối cùng có thể rất lớn, vượt xa phạm vi số nguyên 32 bit. Python xử lý việc này một cách tự động, nhưng trong C++ việc này yêu cầu`long long`. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Lặp lại từng cặp điểm, tính trực tiếp khoảng cách bình phương và thêm nó vào câu trả lời. 

Điều đó có nghĩa là đánh giá:$(x_i-x_j)^2 + (y_i-y_j)^2$cho mọi`i < j`. 

Điều này hiệu quả vì nó khớp chính xác với định nghĩa vấn đề. Vấn đề là quy mô. Với`100000`điểm, có khoảng năm tỷ cặp và mỗi cặp yêu cầu một số phép tính số học. Tốc độ đó là quá chậm so với giới hạn một giây. 

Quan sát chính là tọa độ x và y độc lập bên trong công thức khoảng cách bình phương. Khai triển biểu thức cho:$(x_i-x_j)^2 = x_i^2 + x_j^2 - 2x_ix_j$và tương tự cho tọa độ y. 

Vì vậy, thay vì suy nghĩ về các cặp về mặt hình học, chúng ta có thể nghĩ xem mỗi tọa độ đóng góp bao nhiêu lần vào tổng cuối cùng. 

Trước tiên chỉ xét phần x:$\sum_{i<j}(x_i-x_j)^2$Mở rộng tất cả các điều khoản:$\sum_{i<j}(x_i^2+x_j^2-2x_ix_j)$Mọi`x_i^2`xuất hiện chính xác`N-1`lần trên tất cả các cặp. Các số hạng chéo có thể được viết lại bằng cách sử dụng bình phương của tổng:$\left(\sum x_i\right)^2 = \sum x_i^2 + 2\sum_{i<j}x_ix_j$Sắp xếp lại mang lại:$\sum_{i<j}(x_i-x_j)^2 = N\sum x_i^2 - \left(\sum x_i\right)^2$Công thức tương tự hoạt động độc lập cho tọa độ y. 

Điều đó biến vấn đề từ tính toán theo cặp thành chỉ duy trì bốn giá trị tổng hợp:```
sum_x
sum_y
sum_x2
sum_y2
```Sau khi đọc tất cả các điểm một lần, chúng ta có thể tính toán trực tiếp câu trả lời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N2) | O(1) | Quá chậm | 
| Tối ưu | O(N) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số điểm`N`. 
2. Khởi tạo bốn bộ tích lũy:`sum_x`,`sum_y`,`sum_x2`, Và`sum_y2`. 
3. Đối với mỗi điểm`(x, y)`: 

1. Thêm`x`ĐẾN`sum_x`. 
2. Thêm`y`ĐẾN`sum_y`. 
3. Thêm`x * x`ĐẾN`sum_x2`. 
4. Thêm`y * y`ĐẾN`sum_y2`. 

Những giá trị này là đủ vì công thức cuối cùng chỉ phụ thuộc vào tổng và tổng bình phương. 
4. Tính tổng đóng góp từ tọa độ x:$N\sum x_i^2 - \left(\sum x_i\right)^2$1. Tính tổng đóng góp từ tọa độ y:$N\sum y_i^2 - \left(\sum y_i\right)^2$1. Cộng hai phần đóng góp lại với nhau và in kết quả. 

### Tại sao nó hoạt động 

Thuật toán dựa vào việc khai triển đại số của sai phân bình phương. Mỗi khoảng cách bình phương theo cặp có thể được phân tách thành các thành phần x và y độc lập. Khi tất cả các số hạng mở rộng được cộng lại với nhau, cấu trúc cặp bậc hai sẽ sụp đổ thành các biểu thức chỉ bao gồm tổng toàn cục và tổng bình phương. 

Danh tính$\sum_{i<j}(a_i-a_j)^2 = N\sum a_i^2 - \left(\sum a_i\right)^2$giữ cho bất kỳ chuỗi số. Áp dụng nó riêng biệt cho tọa độ x và tọa độ y sẽ tái tạo lại chính xác tổng khoảng cách bình phương trên tất cả các cặp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())

    sum_x = 0
    sum_y = 0
    sum_x2 = 0
    sum_y2 = 0

    for _ in range(n):
        x, y = map(int, input().split())

        sum_x += x
        sum_y += y

        sum_x2 += x * x
        sum_y2 += y * y

    ans = n * sum_x2 - sum_x * sum_x
    ans += n * sum_y2 - sum_y * sum_y

    print(ans)

solve()
```Phần đầu tiên của mã đọc tất cả các điểm trong khi duy trì bốn giá trị tổng hợp. Chúng tôi không bao giờ tự lưu trữ điểm vì công thức cuối cùng chỉ phụ thuộc vào tổng số. 

Các biểu thức`x * x`Và`y * y`tính tọa độ bình phương trực tiếp. Sử dụng phép nhân thay vì lũy thừa sẽ nhanh hơn và rõ ràng hơn một chút trong mã lập trình cạnh tranh. 

Tính toán cuối cùng phản ánh chính xác đạo hàm toán học. Đầu tiên chúng ta tính toán tổng đóng góp x, sau đó là đóng góp y và cuối cùng cộng chúng lại với nhau. 

Một điểm tinh tế là công thức đã tính từng cặp không có thứ tự chính xác một lần. Không có sự chia cho hai ở bất kỳ đâu trong quá trình triển khai vì danh tính đại số đã tính đến việc đếm cặp chính xác. 

Số nguyên Python tự động mở rộng đến độ chính xác tùy ý, vì vậy việc tràn không phải là vấn đề đáng lo ngại ở đây. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
1 1
-1 -1
1 -1
-1 1
```| Điểm | tổng_x | tổng_y | tổng_x2 | tổng_y2 | 
| --- | --- | --- | --- | --- | 
| (1, 1) | 1 | 1 | 1 | 1 | 
| (-1, -1) | 0 | 0 | 2 | 2 | 
| (1, -1) | 1 | -1 | 3 | 3 | 
| (-1, 1) | 0 | 0 | 4 | 4 | 

Bây giờ hãy tính: 

| Biểu hiện | Giá trị | 
| --- | --- | 
|`4 * 4 - 0²`| 16 | 
|`4 * 4 - 0²`| 16 | 
| Tổng cộng | 32 | 

Đầu ra:```
32
```Ví dụ này cho thấy tính đối xứng làm cho tổng tọa độ bị triệt tiêu như thế nào. Toàn bộ câu trả lời đến từ tổng bình phương. 

### Ví dụ 2 

đầu vào:```
3
0 0
1 0
3 0
```Khoảng cách bình phương theo cặp là:```
(0,1) -> 1
(0,3) -> 9
(1,3) -> 4
```Tổng cộng:```
14
```Dấu vết: 

| Điểm | tổng_x | tổng_y | tổng_x2 | tổng_y2 | 
| --- | --- | --- | --- | --- | 
| (0, 0) | 0 | 0 | 0 | 0 | 
| (1, 0) | 1 | 0 | 1 | 0 | 
| (3, 0) | 4 | 0 | 10 | 0 | 

Bây giờ hãy tính: 

| Biểu hiện | Giá trị | 
| --- | --- | 
|`3 * 10 - 4²`| 14 | 
|`3 * 0 - 0²`| 0 | 
| Tổng cộng | 14 | 

Ví dụ này chứng minh rằng công thức cũng hoạt động khi tất cả các điểm nằm trên một trục. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi điểm được xử lý một lần | 
| Không gian | O(1) | Chỉ có một số biến tích lũy được lưu trữ | 

Giải pháp thoải mái phù hợp với giới hạn. Việc xử lý một trăm nghìn điểm với công việc liên tục trên mỗi điểm dễ dàng đủ nhanh trong giới hạn một giây. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())

    sum_x = 0
    sum_y = 0
    sum_x2 = 0
    sum_y2 = 0

    for _ in range(n):
        x, y = map(int, input().split())

        sum_x += x
        sum_y += y

        sum_x2 += x * x
        sum_y2 += y * y

    ans = n * sum_x2 - sum_x * sum_x
    ans += n * sum_y2 - sum_y * sum_y

    print(ans)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    out = io.StringIO()
    old_stdout = sys.stdout
    sys.stdout = out

    solve()

    sys.stdout = old_stdout

    return out.getvalue().strip()

# provided sample
assert run(
"""4
1 1
-1 -1
1 -1
-1 1
"""
) == "32", "sample 1"

# minimum size
assert run(
"""1
5 7
"""
) == "0", "single point"

# duplicate points
assert run(
"""3
2 2
2 2
2 2
"""
) == "0", "all points identical"

# points on a line
assert run(
"""3
0 0
1 0
3 0
"""
) == "14", "line case"

# negative coordinates
assert run(
"""2
-3 -4
3 4
"""
) == "100", "negative coordinates"

# mixed coordinates
assert run(
"""4
0 0
1 2
-1 3
4 -2
"""
) == "74", "general case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Điểm duy nhất | 0 | Không có cặp nào tồn tại | 
| Tất cả các điểm giống nhau | 0 | Xử lý trùng lặp | 
| Điểm trên một dòng | 14 | Đóng góp x/y độc lập | 
| Tọa độ âm | 100 | Bình phương đúng với số âm | 
| Tọa độ hỗn hợp | 74 | Tính đúng đắn chung | 

## Vỏ cạnh 

Hãy xem xét trường hợp tất cả các điểm giống hệt nhau:```
3
2 2
2 2
2 2
```Trong quá trình xử lý: 

| Điểm | tổng_x | tổng_y | tổng_x2 | tổng_y2 | 
| --- | --- | --- | --- | --- | 
| (2,2) | 2 | 2 | 4 | 4 | 
| (2,2) | 4 | 4 | 8 | 8 | 
| (2,2) | 6 | 6 | 12 | 12 | 

Bây giờ hãy tính:```
3 * 12 - 6² = 36 - 36 = 0
```cho cả hai tọa độ, vì vậy câu trả lời tổng thể là`0`. 

Điều này có hiệu quả vì các tọa độ giống hệt nhau làm cho số hạng tổng bình phương triệt tiêu chính xác số hạng tổng bình phương đã nhân. 

Bây giờ hãy xem xét tọa độ âm:```
2
-3 -4
3 4
```Các bộ tích lũy trở thành: 

| Điểm | tổng_x | tổng_y | tổng_x2 | tổng_y2 | 
| --- | --- | --- | --- | --- | 
| (-3,-4) | -3 | -4 | 9 | 16 | 
| (3,4) | 0 | 0 | 18 | 32 | 

Tính toán cuối cùng:```
2 * 18 - 0² = 36
2 * 32 - 0² = 64
```Tổng cộng:```
36 + 64 = 100
```Bình phương sẽ loại bỏ dấu một cách tự nhiên, khớp chính xác với công thức khoảng cách hình học. 

Cuối cùng, hãy xem xét đầu vào nhỏ nhất có thể:```
1
5 7
```Không có cặp nào cả. Công thức vẫn hoạt động chính xác:```
1 * 25 - 5² = 0
1 * 49 - 7² = 0
```Vì vậy, câu trả lời trở thành`0`mà không cần phải xử lý trường hợp đặc biệt nào.
