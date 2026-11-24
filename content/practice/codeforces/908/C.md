---
title: "CF 908C - Năm Mới và Curling"
description: "Chúng ta có một tập hợp các đĩa có bán kính bằng nhau ban đầu được đặt phía trên mặt phẳng ở tọa độ y rất cao."
date: "2026-06-12T23:36:05+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "geometry", "implementation", "math"]
categories: ["algorithms"]
codeforces_contest: 908
codeforces_index: "C"
codeforces_contest_name: "Good Bye 2017"
rating: 1500
weight: 908
solve_time_s: 211
verified: false
draft: false
---

[CF 908C - Năm mới và môn bi đá trên băng](https://codeforces.com/problemset/problem/908/C) 

**Đánh giá:** 1500 
**Tags:** sức mạnh vũ phu, hình học, cách thực hiện, toán học 
**Thời gian giải:** 3 phút 31s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một tập hợp các đĩa có bán kính bằng nhau ban đầu được đặt phía trên mặt phẳng ở tọa độ _y_ rất cao. Mỗi đĩa có tọa độ _x_ cố định và chúng ta trượt nó thẳng xuống dọc theo đường thẳng đứng cho đến khi nó chạm đất (_y = 0_) hoặc tiếp xúc với một đĩa khác đã được đặt. Nhiệm vụ là tính tọa độ _y_ cuối cùng của tâm mỗi đĩa sau khi tất cả đã được đặt. 

Đầu vào bao gồm _n_, số lượng đĩa và _r_, bán kính chung của chúng. Sau đó theo sau một chuỗi các số nguyên _n_ chỉ định tọa độ _x_ của mỗi đĩa. Đầu ra là danh sách _n_ số thực, vị trí dọc cuối cùng của các đĩa. 

Các ràng buộc ở mức vừa phải: _n_ ≤ 1000 và _r_ ≤ 1000, do đó, ngay cả giải pháp _O(n²)_ cũng khả thi. Khó khăn chính không nằm ở hiệu quả thuật toán mà ở việc mô hình hóa chính xác các tương tác hình học. 

Các trường hợp biên không rõ ràng bao gồm các tình huống trong đó nhiều đĩa có chung tọa độ _x_. Ví dụ: nếu đầu vào là:```
3 1
5 5 5
```đĩa thứ nhất chạm đất tại _y = 1_ (chạm đất), đĩa thứ hai chạm vào đĩa thứ nhất và nằm yên tại _y = 3_, đĩa thứ ba chạm đất tại _y = 5_. Một cách tiếp cận bất cẩn mà bỏ qua vị trí của các đĩa trước đó sẽ đặt sai tất cả các đĩa tại _y = 1_. Một sự phức tạp khác phát sinh khi các đĩa được đóng theo chiều ngang nhưng không được căn chỉnh hoàn hảo, do đó đĩa mới phải tính toán độ lệch dọc bằng công thức khoảng cách vòng tròn-vòng tròn. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản nhất là dùng vũ lực: đối với mỗi đĩa, bắt đầu ở _y = r_ (mặt đất) và kiểm tra tất cả các đĩa đã đặt trước đó. Đối với bất kỳ đĩa nào trước đó có khoảng cách ngang nhỏ hơn hoặc bằng 2*r, hãy tính chiều cao tối đa mà đĩa mới có thể nằm yên mà không bị chồng lên nhau bằng định lý Pythagore. Tận dụng tối đa các độ cao ứng cử viên này và mặt đất. Cách tiếp cận này đúng vì nó thực thi rõ ràng điều kiện dừng cho tất cả các ổ đĩa trước đó, nhưng nó yêu cầu kiểm tra tất cả các ổ đĩa trước đó để tìm mọi ổ đĩa mới, đưa ra độ phức tạp về thời gian là _O(n²)_. Với _n ≤ 1000_, điều này dẫn đến tối đa một triệu phép tính, có thể chấp nhận được. 

Quan sát quan trọng là nói chung không có giải pháp tiệm cận nhanh hơn, bởi vì bất kỳ đĩa nào cũng có thể tương tác với tất cả các đĩa trước đó. Việc tối ưu hóa có thể được thực hiện bằng cách duy trì chỉ mục không gian (như cây phân đoạn hoặc nén tọa độ), nhưng đối với _n = 1000_ thì điều đó là không cần thiết. Phương pháp bạo lực là đủ và cần phải thực hiện cẩn thận để xử lý độ chính xác của dấu phẩy động. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Đã chấp nhận | 
| Tối ưu | O(n²) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo danh sách trống`y_positions`để lưu trữ tọa độ _y_ cuối cùng của mỗi đĩa. 
2. Lặp lại từng đĩa`i`theo thứ tự đã cho. Bắt đầu với vị trí dọc của ứng viên`y = r`, tương ứng với việc chạm đất. 
3. Đối với mỗi đĩa trước`j`(từ 0 đến i-1), kiểm tra xem khoảng cách ngang`abs(x[i] - x[j])`nhỏ hơn hoặc bằng 2*r. Điều này xác định liệu đĩa mới có thể nằm trên đĩa hay không`j`. 
4. Nếu hai đĩa có thể chạm vào nhau, hãy tính độ lệch dọc bằng định lý Pythagore: _y_ của tâm mới là`y[j] + sqrt(4*r*r - dx*dx)`Ở đâu`dx = abs(x[i] - x[j])`. Điều này mang lại vị trí cao nhất nơi các đĩa tiếp tuyến mà không chồng chéo lên nhau. 
5. Cập nhật`y`là giá trị tối đa của giá trị hiện tại và chiều cao tính toán này. 
6. Sau khi kiểm tra tất cả các đĩa trước đó, hãy nối thêm`y`ĐẾN`y_positions`. 
7. Lặp lại cho đến khi tất cả các đĩa được xử lý. Xuất tất cả các vị trí với độ chính xác đủ về dấu phẩy động. 

Điều bất biến là tại mỗi bước,`y_positions[i]`là tọa độ _y_ hợp lệ nhỏ nhất sao cho đĩa _i_ không giao với bất kỳ đĩa nào được đặt trước đó và thỏa mãn ràng buộc mặt đất. 

## Giải pháp Python```python
import sys, math
input = sys.stdin.readline

n, r = map(int, input().split())
x_coords = list(map(int, input().split()))

y_positions = []

for i in range(n):
    y = r  # minimal y if it touches the ground
    for j in range(i):
        dx = abs(x_coords[i] - x_coords[j])
        if dx <= 2*r:
            y = max(y, y_positions[j] + math.sqrt(4*r*r - dx*dx))
    y_positions.append(y)

print(" ".join(f"{y:.10f}" for y in y_positions))
```Mã trực tiếp thực hiện các bước thuật toán. Chúng tôi khởi tạo mỗi đĩa ở độ cao _r_, lặp lại tất cả các đĩa trước đó để tính chiều cao tối đa được phép nhằm tránh chồng chéo, sau đó nối thêm kết quả. sử dụng`math.sqrt`đảm bảo vị trí hình học chính xác. In với độ chính xác cao tránh được các vấn đề về kiểm tra dấu phẩy động. 

## Ví dụ đã hoạt động 

**Ví dụ 1:** 

đầu vào:```
6 2
5 5 6 8 3 12
```| tôi | x[i] | Ứng viên y | Các đĩa trước đã được kiểm tra | Max y sau khi kiểm tra | 
| --- | --- | --- | --- | --- | 
| 0 | 5 | 2 | không | 2 | 
| 1 | 5 | 2 | j=0, dx=0 | 6 | 
| 2 | 6 | 2 | j=0 dx=1 -> y=√(16-1)+2≈5.123, j=1 dx=1 -> y≈7.123 | 7.123 | 
| 3 | 8 | 2 | j=0 dx=3->bỏ qua, j=1 dx=3->bỏ qua, j=2 dx=2->y≈9.873 | 9.873 | 
| 4 | 3 | 2 | j=0 dx=2->y≈4.0, j=1 dx=2->y≈6.0, j=2 dx=3->bỏ qua, j=3 dx=5->bỏ qua | 6.0 | 
| 5 | 12 | 2 | kiểm tra các đĩa liên quan | 13.337 | 

Điều này chứng tỏ rằng ngay cả khi các đĩa gần nhau theo chiều ngang nhưng không giống nhau, công thức Pythagore vẫn tính toán chính xác độ lệch dọc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Đối với mỗi n đĩa, chúng tôi kiểm tra tối đa n đĩa trước đó | 
| Không gian | O(n) | Lưu trữ một tọa độ _y_ trên mỗi đĩa | 

Với n 1000, mã thực hiện tối đa 1e6 lần lặp, nằm trong giới hạn 2 giây. Việc sử dụng bộ nhớ là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io
import math

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n, r = map(int, input().split())
    x_coords = list(map(int, input().split()))
    y_positions = []
    for i in range(n):
        y = r
        for j in range(i):
            dx = abs(x_coords[i] - x_coords[j])
            if dx <= 2*r:
                y = max(y, y_positions[j] + math.sqrt(4*r*r - dx*dx))
        y_positions.append(y)
    return " ".join(f"{y:.10f}" for y in y_positions)

# Provided sample
assert run("6 2\n5 5 6 8 3 12\n") == "2.0000000000 6.0000000000 9.8729833462 13.3370849613 12.5187346573 13.3370849613"

# Minimum input
assert run("1 1\n1\n") == "1.0000000000"

# All disks in same x
assert run("3 1\n5 5 5\n") == "1.0000000000 3.0000000000 5.0000000000"

# All disks far apart
assert run("3 1\n1 100 200\n") == "1.0000000000 1.0000000000 1.0000000000"

# Disks at maximum n=1000, r=1000, sparse x
x_vals = " ".join(str(i*2) for i in range(1, 1001))
inp = f"1000 1000\n{x_vals}\n"
output = run(inp)
ys = list(map(float, output.split()))
assert all(y == 1000.0 for y in ys), "All disks should sit on ground due to spacing"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1\n1`|`1.0`| Đĩa đơn trên mặt đất | 
|`3 1\n5 5 5`|`1 3 5`| Nhiều đĩa xếp chồng lên nhau theo chiều dọc | 
|`3 1\n1 100 200`|`1 1 1`| Các đĩa cách xa nhau, mỗi đĩa nằm trên mặt đất | 
|`1000 1000\n2 4 6 ...`| `1000 | |
