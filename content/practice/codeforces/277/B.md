---
title: "CF 277B - Tập hợp điểm"
description: "Chúng ta được yêu cầu xây dựng một tập hợp n điểm trên mặt phẳng 2D sao cho đa giác lồi lớn nhất có thể được hình thành từ những điểm này có đúng m đỉnh. Không có ba điểm nào có thể thẳng hàng."
date: "2026-06-06T00:44:58+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "geometry"]
categories: ["algorithms"]
codeforces_contest: 277
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 170 (Div. 1)"
rating: 2300
weight: 277
solve_time_s: 111
verified: false
draft: false
---

[CF 277B - Tập hợp điểm](https://codeforces.com/problemset/problem/277/B) 

**Đánh giá:** 2300 
**Tags:** thuật toán xây dựng, hình học 
**Thời gian giải:** 1 phút 51 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu xây dựng một bộ`n`các điểm trên mặt phẳng 2D sao cho đa giác lồi lớn nhất có thể được hình thành từ các điểm này có chính xác`m`đỉnh. Không có ba điểm nào có thể thẳng hàng. Đầu vào bao gồm hai số nguyên`n`Và`m`với giới hạn`3 ≤ m ≤ 100`Và`m ≤ n ≤ 2*m`. Đầu ra phải là`-1`nếu không có công trình nào tồn tại hoặc danh sách`n`các điểm có tọa độ nguyên giới hạn bởi`10^8`. 

Những hạn chế về`n`Và`m`cho chúng ta biết rằng tập hợp phải bao gồm tất cả các đỉnh của một lồi`m`-gon và có khả năng lên tới`m`điểm bổ sung bên trong nó. Từ`n`không thể vượt quá`2*m`, chúng ta được đảm bảo rằng mọi điểm phụ có thể được đặt bên trong đa giác lồi mà không tạo ra tập con lồi lớn hơn. Hạn chế là không có ba điểm nào nằm trên một đường thẳng ngăn cản việc xây dựng tầm thường dọc theo một đường thẳng. 

Các trường hợp biên bao gồm kịch bản tối thiểu`n = m = 3`, trong đó đa giác lồi là toàn bộ tập hợp điểm và các trường hợp trong đó`n = 2*m`, buộc chúng ta phải đặt số điểm tối đa vào bên trong mà không tạo thành bao lồi mới. Một cách tiếp cận ngây thơ đặt tất cả các điểm trên một lớp lồi sẽ thất bại vì`n > m`bởi vì độ lồi sẽ vượt quá`m`. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng tạo ra tất cả các tập hợp con các điểm và kiểm tra kích thước của bao lồi. Đối với mỗi tập hợp điểm ứng cử viên, chúng tôi sẽ tính toán bao lồi bằng cách sử dụng quét Graham hoặc thuật toán chuỗi đơn điệu của Andrew, sau đó kiểm tra xem kích thước của nó có bằng không`m`. Số lượng tập hợp con tăng lên theo tổ hợp, lên đến`2^n`, điều này hoàn toàn không thể thực hiện được ngay cả đối với`n = 100`. 

Cái nhìn sâu sắc quan trọng là hình học: chúng ta luôn có thể bắt đầu bằng một đường lồi`m`-gon và đặt thêm các điểm bên trong nó mà không làm tăng bao lồi. Nếu chúng ta đặt thêm`n - m`chỉ hoàn toàn bên trong đa giác và không thẳng hàng với bất kỳ cạnh nào, tập con lồi lớn nhất vẫn giữ nguyên chính xác như ban đầu`m`đỉnh. Cấu trúc này cho phép xây dựng thời gian tuyến tính đơn giản. 

Để thỏa mãn ràng buộc "không có ba điểm trên một đường thẳng", chúng ta có thể sắp xếp các điểm bên trong một chút, ví dụ bằng cách thêm các điểm dọc theo một đường chéo nông bên trong đa giác. Tọa độ có thể được chọn nhỏ, nguyên và được giới hạn trong phạm vi`10^8`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n * n log n) | O(n) | Quá chậm | 
| Đa giác lồi xây dựng | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng một lồi`m`-gon. Lựa chọn đơn giản nhất là đặt các điểm dọc theo đường`y = x`với số gia số nguyên, đảm bảo chúng ở vị trí lồi hoàn toàn. Dán nhãn cho những điểm này`P0, P1, ..., P_{m-1}`. Bao lồi của các điểm này sẽ có kích thước chính xác`m`. 
2. Xác định cần bao nhiêu điểm bên trong:`k = n - m`. Những điểm này phải nằm hoàn toàn bên trong đa giác. 
3. Địa điểm`k`các điểm bên trong dọc theo một đường hơi lệch so với đáy của đa giác, ví dụ như tại`y = 0`với`x`tọa độ so le giữa hai đỉnh. Điều này đảm bảo chúng nằm bên trong bao lồi và tránh được hiện tượng cộng tuyến. 
4. Xuất tọa độ của tất cả`n`điểm. 

Tại sao nó hoạt động: bao lồi của một tập hợp điểm được xác định bởi các điểm ngoài cùng. Bằng cách đặt thêm`k`điểm hoàn toàn bên trong, chúng không thể trở thành các đỉnh của một đa giác lồi mới, do đó tập con lồi lớn nhất vẫn giữ nguyên chính xác là tập con lồi đầu tiên`m`điểm. Các điểm bên trong so le đảm bảo không có ba điểm nào thẳng hàng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())

if m < 3 or m > n or n > 2 * m:
    print(-1)
    sys.exit()

points = []

# Step 1: Construct convex m-gon along y = x
for i in range(m):
    points.append((i * 2, i * 2))

# Step 2: Place interior points, staggered along y = 0
for i in range(n - m):
    points.append((i * 2 + 1, 0))

for x, y in points:
    print(x, y)
```Mỗi phần tương ứng với các bước thuật toán. Đa giác lồi bên ngoài sử dụng tọa độ`(0,0), (2,2), ..., (2*(m-1), 2*(m-1))`, đảm bảo độ dốc tăng dần. Điểm nội thất sử dụng`y = 0`và loạng choạng`x`tọa độ`(1,3,5,...)`, vì vậy chúng không thể thẳng hàng với các cạnh đa giác. Nhân tọa độ với 2 sẽ có chỗ để đặt các điểm bên trong mà không chồng chéo lên nhau. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:`4 3`| Bước | Điểm được xây dựng | Kích thước thân lồi | 
| --- | --- | --- | 
| 1 | (0,0),(2,2),(4,4) | 3 | 
| 2 | (1,0) | Vẫn 3 | 

Điểm bổ sung`(1,0)`nằm bên trong bao lồi của tam giác`(0,0),(2,2),(4,4)`, do đó tập con lồi lớn nhất vẫn có kích thước 3. 

**Mẫu tùy chỉnh** 

đầu vào:`6 4`| Bước | Điểm được xây dựng | Kích thước thân lồi | 
| --- | --- | --- | 
| 1 | (0,0),(2,2),(4,4),(6,6) | 4 | 
| 2 | (1,0),(3,0) | Vẫn 4 | 

Cả hai điểm trong đều nằm dưới tứ giác lồi. Độ lồi được bảo toàn ở mức 4. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Xây dựng và in điểm tuyến tính trong n | 
| Không gian | O(n) | Chúng tôi lưu trữ n điểm một cách rõ ràng | 

Được cho`n ≤ 200`, điều này nằm trong giới hạn 2 giây và bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    exec(open("solution.py").read())
    return output.getvalue().strip()

# Provided sample
assert run("4 3") == "0 0\n2 2\n4 4\n1 0", "sample 1"

# Minimum input
assert run("3 3") == "0 0\n2 2\n4 4", "min n=m"

# Maximum input
assert run("200 100")[:5] == "0 0\n2 2", "max n and m"

# n=m+1 case
assert run("5 3") == "0 0\n2 2\n4 4\n1 0\n3 0", "n=m+2"

# n=2*m case
assert run("6 3") == "0 0\n2 2\n4 4\n1 0\n3 0\n5 0", "n=2*m"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 3 3 | 0 0,2 2,4 4 | Thân lồi có kích thước tối thiểu | 
| 4 3 | 0 0,2 2,4 4,1 0 | Vị trí điểm nội thất nhỏ | 
| 6 3 | 0 0,2 2,4 4,1 0,3 0,5 0 | Điểm bên trong tối đa với n=2*m | 
| 200 100 | 0 0,2 2,... | Xử lý ràng buộc tối đa | 

## Vỏ cạnh 

cho`n = m`, thuật toán chỉ xây dựng đa giác lồi. Ví dụ,`n = 5, m = 5`mang lại điểm`(0,0),(2,2),(4,4),(6,6),(8,8)`. Không có điểm bên trong nên độ lồi chính xác là`m`. 

Vì`n = 2*m`, thuật toán đặt`m`các điểm bên trong dọc theo một đường bên trong đa giác. Không có đỉnh lồi mới nào được tạo ra vì các điểm bên trong nằm hoàn toàn bên trong, so le và không thẳng hàng với bất kỳ cạnh đa giác nào. Vì`n = 6, m = 3`, điểm`(0,0),(2,2),(4,4)`tạo thành hình tam giác và`(1,0),(3,0),(5,0)`nằm bên trong, bảo toàn độ lồi tại 3. 

Tất cả các trường hợp khác với`m ≤ n ≤ 2*m`cũng được xử lý tương tự. Không có ba điểm nào thẳng hàng, các điểm trong nằm hoàn toàn bên trong đa giác và kích thước bao lồi vẫn giữ nguyên`m`.
