---
title: "CF 14A - Thư"
description: "Chúng ta có một lưới hình chữ nhật biểu thị một tờ giấy vẽ đồ thị có _n_ hàng và _m_ cột. Một số ô vuông được tô bóng, đánh dấu *, còn lại không tô bóng, đánh dấu bằng .."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 14
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 14 (Div. 2)"
rating: 800
weight: 14
solve_time_s: 68
verified: true
draft: false
---
[CF 14A - Thư](https://codeforces.com/problemset/problem/14/A) 

**Đánh giá:** 800 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một lưới hình chữ nhật biểu thị một tờ giấy vẽ đồ thị có _n_ hàng và _m_ cột. Một số hình vuông được tô bóng, đánh dấu bằng`*`, và phần còn lại không được tô bóng, được đánh dấu bằng`.`. Bob muốn cắt hình chữ nhật nhỏ nhất chứa tất cả các hình vuông được tô bóng để anh ấy có thể gửi nó với chi phí tối thiểu. Đầu ra chỉ đơn giản là nội dung của hình chữ nhật này, giữ nguyên kiểu tô bóng ban đầu. 

Vì cả hai chiều đều được giới hạn bởi 50 nên lưới nhỏ. Điều đó có nghĩa là bất kỳ thuật toán nào chạm vào từng ô một vài lần đều đủ nhanh. Vấn đề cơ bản là tìm hộp giới hạn nhỏ nhất xung quanh tất cả`*`nhân vật. 

Các trường hợp cạnh không rõ ràng bao gồm các tình huống trong đó tất cả các ô được tô bóng nằm trên cùng một hàng, trên cùng một cột hoặc tập trung ở một góc. Ví dụ: nếu đầu vào là:```
3 3
*..
...
...
```Đầu ra phải chỉ`*`, không phải toàn bộ hàng hoặc cột. Ví dụ: việc triển khai bất cẩn luôn in đầy đủ các hàng hoặc cột có thể bao gồm quá mức các ô trống. Tương tự, một hình chữ nhật chỉ có một hàng hoặc một cột sẽ được xử lý chính xác mà không tạo ra lỗi chỉ mục. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ là lặp lại tất cả các hình chữ nhật có thể có trong lưới, kiểm tra từng hình chữ nhật xem nó có chứa tất cả`*`và chọn hình có diện tích nhỏ nhất. Điều này hiệu quả vì tính chính xác được đảm bảo: bất kỳ hình chữ nhật nào chứa tất cả các ngôi sao đều là ứng cử viên. Tuy nhiên, số lượng hoạt động là khoảng`(n * m)^2 * (n * m)`để kiểm tra từng hình chữ nhật, có khoảng 15 triệu lần lặp cho`n = m = 50`. Mặc dù điều này vẫn có thể chạy dưới một giây trong Python nhưng nó không hiệu quả và không cần thiết. 

Cách tiếp cận tối ưu thúc đẩy cấu trúc vấn đề: chúng ta chỉ cần trên cùng, dưới cùng, ngoài cùng bên trái và ngoài cùng bên phải`*`tế bào. Nếu chúng ta ghi lại các chỉ số hàng và chỉ số cột nhỏ nhất và lớn nhất trong đó`*`xuất hiện, bốn giá trị này ngay lập tức xác định hình chữ nhật giới hạn tối thiểu. Điều này làm giảm công việc quét từng ô chính xác một lần và xuất ra lưới con được xác định bởi các ranh giới này. Quan sát quan trọng là hình chữ nhật tối thiểu phải thẳng hàng với tọa độ cực trị của các ô được tô bóng, do đó không cần phải kiểm tra các hình chữ nhật khác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((nm)^3) | O(nm) | Quá chậm | 
| Tối ưu | O(nm) | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo các biến để theo dõi các hàng tối thiểu và tối đa (`top`,`bottom`) và cột (`left`,`right`) chứa`*`. Bộ`top`Và`left`với số lượng rất lớn và`bottom`Và`right`đến -1. 
2. Lặp lại qua từng ô`(i, j)`trong lưới. Khi một ô chứa`*`, cập nhật`top`ĐẾN`min(top, i)`,`bottom`ĐẾN`max(bottom, i)`,`left`ĐẾN`min(left, j)`, Và`right`ĐẾN`max(right, j)`. Bước này xác định các điểm cực trị của vùng bóng mờ. 
3. Sau khi quét,`top`ĐẾN`bottom`đưa ra phạm vi hàng và`left`ĐẾN`right`đưa ra phạm vi cột của hình chữ nhật tối thiểu chứa tất cả các ngôi sao. 
4. Lặp lại các hàng`top`ĐẾN`bottom`và với mỗi hàng, in chuỗi con từ cột`left`ĐẾN`right`. Điều này tạo ra hình chữ nhật tối thiểu với kiểu tô bóng ban đầu còn nguyên vẹn. 

Tại sao nó hoạt động: Thuật toán duy trì tính bất biến mà`top`,`bottom`,`left`, Và`right`luôn bao hàm tất cả`*`thấy cho đến nay. Khi tất cả các ô đã được quét, hình chữ nhật được xác định bởi các ranh giới này là nhỏ nhất có thể, bởi vì bất kỳ hình chữ nhật nhỏ hơn nào cũng sẽ thiếu ít nhất một ô.`*`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
grid = [input().strip() for _ in range(n)]

top, bottom = n, -1
left, right = m, -1

for i in range(n):
    for j in range(m):
        if grid[i][j] == '*':
            top = min(top, i)
            bottom = max(bottom, i)
            left = min(left, j)
            right = max(right, j)

for i in range(top, bottom + 1):
    print(grid[i][left:right + 1])
```Mã đọc lưới, theo dõi các vị trí cực trị của các ô được tô bóng và cắt lưới tương ứng. sử dụng`strip()`đảm bảo không có ký tự dòng mới nào can thiệp. Có thể tránh các lỗi riêng lẻ bằng cách bao gồm cả hai điểm cuối khi cắt bằng`right + 1`Và`bottom + 1`. 

## Ví dụ đã hoạt động 

**Ví dụ 1**: 

đầu vào:```
6 7
.......
..***..
..*....
..***..
..*....
..***..
```Các biến theo dõi: 

| Hàng | vị trí '*' | hàng đầu | dưới cùng | trái | đúng | 
| --- | --- | --- | --- | --- | --- | 
| 0 | không | 6 | -1 | 7 | -1 | 
| 1 | 2,3,4 | 1 | 1 | 2 | 4 | 
| 2 | 2 | 1 | 2 | 2 | 4 | 
| 3 | 2,3,4 | 1 | 3 | 2 | 4 | 
| 4 | 2 | 1 | 4 | 2 | 4 | 
| 5 | 2,3,4 | 1 | 5 | 2 | 4 | 

Trích xuất hình chữ nhật: hàng 1-5, cột 2-4 

Đầu ra:```
***
*..
***
*..
***
```**Ví dụ 2 (ô đơn)**: 

đầu vào:```
3 3
...
.*.
...
```Các biến theo dõi: 

| Hàng | vị trí '*' | hàng đầu | dưới cùng | trái | đúng | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 1 | 1 | 

Đầu ra:```
*
```Điều này chứng tỏ rằng ngay cả một ô được tô bóng cũng được xử lý chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) | Chúng tôi quét từng ô chính xác một lần và chỉ cắt các hàng để lấy đầu ra. | 
| Không gian | O(nm) | Bản thân lưới được lưu trữ; các biến phụ là O(1). | 

Được cho`n, m ≤ 50`,`nm`nhiều nhất là 2500, nằm trong giới hạn 1 giây. Việc sử dụng bộ nhớ là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    n, m = map(int, input().split())
    grid = [input().strip() for _ in range(n)]

    top, bottom = n, -1
    left, right = m, -1

    for i in range(n):
        for j in range(m):
            if grid[i][j] == '*':
                top = min(top, i)
                bottom = max(bottom, i)
                left = min(left, j)
                right = max(right, j)

    for i in range(top, bottom + 1):
        print(grid[i][left:right + 1])

    return sys.stdout.getvalue().strip()

# Provided sample
assert run("6 7\n.......\n..***..\n..*....\n..***..\n..*....\n..***..") == "***\n*..\n***\n*..\n***", "sample 1"

# Single star
assert run("3 3\n...\n.*.\n...") == "*", "single star"

# Full rectangle
assert run("2 2\n**\n**") == "**\n**", "full 2x2 rectangle"

# Row-only rectangle
assert run("3 5\n.....\n*****\n.....") == "*****", "row only"

# Column-only rectangle
assert run("4 3\n.*.\n.*.\n.*.\n.*.") == "*\n*\n*\n*", "column only"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 3\n...\n.*.\n...`|`*`| hình chữ nhật ô đơn | 
|`2 2\n**\n**`|`**\n**`| hình chữ nhật nhỏ đầy đủ | 
|`3 5\n.....\n*****\n.....`|`*****`| hình chữ nhật một hàng | 
|`4 3\n.*.\n.*.\n.*.\n.*.`|`*\n*\n*\n*`| hình chữ nhật cột đơn | 

## Vỏ cạnh 

Nếu tất cả các ngôi sao nằm thành một hàng,`top`Và`bottom`đều bình đẳng. Vòng lặp của chúng tôi chỉ in chính xác hàng đó. Tương tự, nếu tất cả các ngôi sao nằm trong một cột,`left`Và`right`bằng nhau, tạo thành hình chữ nhật một cột. Nếu ngôi sao nằm ở góc lưới, chẳng hạn như`(0,0)`,`min`Và`max`logic vẫn tính toán hình chữ nhật giới hạn chính xác. Thuật toán này xử lý các đầu vào tối thiểu và tối đa một cách liền mạch.
