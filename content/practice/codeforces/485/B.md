---
title: "CF 485B - Tài nguyên có giá trị"
description: "Chúng tôi được yêu cầu xây dựng một thành phố hình vuông trên bản đồ Descartes 2D sao cho nó bao quanh tất cả các mỏ nhất định, được biểu thị dưới dạng các điểm có tọa độ nguyên. Các cạnh của hình vuông phải song song với các trục và mục tiêu của chúng ta là giảm thiểu diện tích của hình vuông."
date: "2026-05-31T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "greedy"]
categories: ["algorithms"]
codeforces_contest: 485
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 276 (Div. 2)"
rating: 1300
weight: 485
solve_time_s: 669
verified: false
draft: false
---

[CF 485B - Tài nguyên có giá trị](https://codeforces.com/problemset/problem/485/B) 

**Đánh giá:** 1300 
**Tags:** vũ phu, tham lam 
**Thời gian giải:** 11p 9s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu xây dựng một thành phố hình vuông trên bản đồ Descartes 2D sao cho nó bao quanh tất cả các mỏ nhất định, được biểu thị dưới dạng các điểm có tọa độ nguyên. Các cạnh của hình vuông phải song song với các trục và mục tiêu của chúng ta là giảm thiểu diện tích của hình vuông. Đầu vào bao gồm`n`điểm, mỗi điểm có tọa độ`(x_i, y_i)`và đầu ra là một số nguyên biểu thị diện tích tối thiểu của hình vuông. 

Những ràng buộc chỉ ra rằng`n`có thể lên tới 1000 và tọa độ có thể lớn tới ±10^9. Điều này ngụ ý rằng bất kỳ cách tiếp cận nào có độ phức tạp kém hơn`O(n)`hoặc`O(n log n)`có thể chấp nhận được vì 1000 điểm có thể dễ dàng quản lý được. Các phương pháp tiếp cận bạo lực cố gắng lặp lại trên tất cả các vị trí vuông có thể sẽ liên quan đến`O(n^2)`hoặc tệ hơn, điều này vẫn khả thi với 1000 điểm nhưng không cần thiết vì tồn tại một cách tiếp cận hình học trực tiếp hơn. 

Trường hợp cạnh phát sinh khi tất cả các điểm nằm trên một đường thẳng, ví dụ:```
2
1 1
1 5
```Ở đây, một hình vuông bao phủ đường thẳng đứng cũng phải mở rộng theo chiều ngang, do đó, cạnh của hình vuông được quyết định bởi độ rộng lớn nhất ở cả hai chiều. Một cách tiếp cận bất cẩn có thể chỉ lấy chênh lệch lớn nhất của x hoặc y mà không xem xét rằng cạnh bình phương phải lớn hơn trong hai hiệu. 

Một trường hợp tinh vi khác là khi tất cả các điểm đều giống hệt nhau:```
3
0 0
0 0
0 0
```Ở đây hình vuông phải có độ dài cạnh bằng 0 và diện tích bằng 0. Việc xử lý diện tích bằng 0 một cách chính xác là điều quan trọng. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là thử tất cả các cặp điểm trên cùng bên trái và dưới cùng bên phải có thể làm các góc của hình vuông, tính cạnh hình vuông cần thiết để che chúng và giữ lại diện tích tối thiểu được tìm thấy. Đối với mỗi cặp, chúng tôi sẽ kiểm tra tất cả các điểm để xem chúng có vừa với bên trong không. Điều này sẽ mang lại khoảng`O(n^3)`hoạt động và không hiệu quả ngay cả đối với`n = 1000`, có khả năng thực hiện 10^9 thao tác trong trường hợp xấu nhất. 

Quan sát quan trọng là các cạnh của hình vuông phải bao phủ tọa độ cực x và y của mỏ. Cho phép`min_x`Và`max_x`là tọa độ x tối thiểu và tối đa và`min_y`Và`max_y`tọa độ y tối thiểu và tối đa. Bất kỳ hình vuông nào bao phủ tất cả các điểm phải có chiều dài ít nhất`max_x - min_x`theo chiều ngang và`max_y - min_y`theo chiều dọc. Để thỏa mãn hình vuông, chúng ta lấy nhịp lớn hơn trong hai nhịp này làm chiều dài cạnh. Quan sát này làm giảm vấn đề thành quét tuyến tính đơn giản cho các giá trị tối thiểu/tối đa. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các cặp góc) | O(n^3) | O(n) | Quá chậm | 
| Giảm tối thiểu-tối đa (tối ưu) | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo bốn biến để theo dõi tọa độ cực trị:`min_x`,`max_x`,`min_y`, Và`max_y`. Bộ`min_x`Và`min_y`đến các giá trị rất lớn (vô cùng) và`max_x`Và`max_y`đến các giá trị rất nhỏ (-vô cực). 
2. Lặp lại tọa độ từng mỏ`(x_i, y_i)`. Đối với mỗi điểm, cập nhật`min_x`nếu như`x_i`nhỏ hơn,`max_x`nếu như`x_i`lớn hơn,`min_y`nếu như`y_i`nhỏ hơn và`max_y`nếu như`y_i`lớn hơn. Bước này tìm hình chữ nhật giới hạn của tất cả các mỏ. 
3. Tính độ dài cạnh cần tìm của hình vuông là`side = max(max_x - min_x, max_y - min_y)`. Điều này đảm bảo hình vuông có thể bao phủ hình chữ nhật được xác định bởi các cực trong khi vẫn duy trì các cạnh bằng nhau. 
4. Tính diện tích là`area = side * side`và xuất kết quả. 

Tại sao nó hoạt động: Hình chữ nhật giới hạn của các điểm xác định chiều rộng và chiều cao tối thiểu cần thiết để bao phủ tất cả các mỏ. Lấy kích thước lớn hơn đảm bảo rằng thành phố vẫn là hình vuông trong khi vẫn bao phủ tất cả các điểm. Không có hình vuông nhỏ hơn nào có thể bao quanh tất cả các mỏ vì ít nhất một cạnh sẽ quá ngắn để bao phủ các điểm cực trị trong một chiều. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
min_x = float('inf')
max_x = float('-inf')
min_y = float('inf')
max_y = float('-inf')

for _ in range(n):
    x, y = map(int, input().split())
    if x < min_x:
        min_x = x
    if x > max_x:
        max_x = x
    if y < min_y:
        min_y = y
    if y > max_y:
        max_y = y

side = max(max_x - min_x, max_y - min_y)
area = side * side
print(area)
```Đoạn mã trên phản ánh trực tiếp các bước thuật toán. Việc sử dụng`float('inf')`Và`float('-inf')`đảm bảo bất kỳ điểm đầu tiên nào cũng sẽ khởi tạo chính xác các điểm cực trị. Việc cập nhật các giá trị tối thiểu/tối đa riêng lẻ sẽ tránh được từng lỗi riêng lẻ, đặc biệt đối với tọa độ âm. 

## Ví dụ đã hoạt động 

**Mẫu 1:**```
Input:
2
0 0
2 2
```| Bước | phút_x | max_x | phút_y | max_y | 
| --- | --- | --- | --- | --- | 
| Ban đầu | thông tin | -inf | thông tin | -inf | 
| Sau (0,0) | 0 | 0 | 0 | 0 | 
| Sau (2,2) | 0 | 2 | 0 | 2 | 

Cạnh = max(2-0, 2-0) = 2, Diện tích = 4. Đúng. 

**Mẫu tùy chỉnh 2:**```
Input:
3
1 3
4 5
2 2
```| Bước | phút_x | max_x | phút_y | max_y | 
| --- | --- | --- | --- | --- | 
| Ban đầu | thông tin | -inf | thông tin | -inf | 
| Sau (1,3) | 1 | 1 | 3 | 3 | 
| Sau (4,5) | 1 | 4 | 3 | 5 | 
| Sau (2,2) | 1 | 4 | 2 | 5 | 

Cạnh = max(4-1, 5-2) = 3, Diện tích = 9. Đúng. 

Những dấu vết này xác nhận rằng thuật toán tính toán chính xác bình phương tối thiểu để bao phủ tất cả các điểm. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Chuyển một lần qua tất cả các điểm để tính tọa độ tối thiểu/tối đa | 
| Không gian | O(1) | Chỉ có bốn biến để theo dõi các cực trị, độc lập với n | 

Được cho`n`1000, thuật toán này thực thi gần như ngay lập tức. Việc sử dụng bộ nhớ là chuyện nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    min_x = float('inf')
    max_x = float('-inf')
    min_y = float('inf')
    max_y = float('-inf')

    for _ in range(n):
        x, y = map(int, input().split())
        min_x = min(min_x, x)
        max_x = max(max_x, x)
        min_y = min(min_y, y)
        max_y = max(max_y, y)

    side = max(max_x - min_x, max_y - min_y)
    return str(side * side)

# Provided sample
assert run("2\n0 0\n2 2\n") == "4", "sample 1"

# Minimum size input
assert run("2\n0 0\n0 0\n") == "0", "all points identical"

# Vertical line
assert run("2\n1 1\n1 5\n") == "16", "vertical line"

# Horizontal line
assert run("2\n3 7\n8 7\n") == "25", "horizontal line"

# Maximum spread
assert run("4\n-1000000000 -1000000000\n1000000000 1000000000\n0 0\n-500000000 500000000\n") == "4000000000000000000", "large coordinates"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 điểm giống nhau | 0 | Trường hợp không diện tích | 
| Đường dọc | 16 | Cạnh phải bằng sai phân y | 
| Đường ngang | 25 | Cạnh phải bằng sai phân x | 
| Tọa độ lớn | 4e18 | Xử lý các giá trị tọa độ cực cao mà không bị tràn | 

## Vỏ cạnh 

Đối với các điểm giống hệt nhau như`2\n0 0\n0 0`,`min_x = max_x = min_y = max_y = 0`, cạnh = 0, diện tích = 0. Thuật toán xử lý vấn đề này một cách chính xác vì nó sử dụng`max(max_x - min_x, max_y - min_y)`đó là 0. 

Đối với đường thẳng đứng`2\n1 1\n1 5`,`min_x = max_x = 1`,`min_y = 1`,`max_y = 5`, side = max(0, 4) = 4, diện tích = 16. Thuật toán chọn đúng kích thước lớn nhất là cạnh hình vuông. 

Đối với đường ngang`2\n3 7\n8 7`,`min_x = 3`,`max_x = 8`,`min_y = max_y = 7`, cạnh = max(5, 0) = 5, diện tích = 25. Một lần nữa, nó xác định chính xác cạnh hình vuông
