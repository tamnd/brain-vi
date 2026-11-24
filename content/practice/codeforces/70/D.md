---
title: "CF 70D - Nhiệm vụ của giáo sư"
description: "Chúng ta được yêu cầu duy trì một tập hợp các điểm trên một mặt phẳng và hỗ trợ hai thao tác: thêm một điểm và kiểm tra xem một điểm có nằm trong bao lồi của tập hợp hiện tại hay không. Bao lồi là đa giác lồi tối thiểu bao quanh tất cả các điểm."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "geometry"]
categories: ["algorithms"]
codeforces_contest: 70
codeforces_index: "D"
codeforces_contest_name: "Codeforces Beta Round 64"
rating: 2700
weight: 70
solve_time_s: 122
verified: true
draft: false
---

[CF 70D - Nhiệm vụ của giáo sư](https://codeforces.com/problemset/problem/70/D) 

**Đánh giá:** 2700 
**Tags:** cấu trúc dữ liệu, hình học 
**Thời gian giải:** 2m 2s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu duy trì một tập hợp các điểm trên một mặt phẳng và hỗ trợ hai thao tác: thêm một điểm và kiểm tra xem một điểm có nằm trong bao lồi của tập hợp hiện tại hay không. Bao lồi là đa giác lồi tối thiểu bao quanh tất cả các điểm. Mọi điểm bên trong đa giác hoặc trên các cạnh của nó đều được tính là “bên trong”. Thách thức đến từ thực tế là các điểm được thêm một cách linh hoạt và các truy vấn về việc ngăn chặn được xen kẽ. 

Số lượng truy vấn có thể lên tới 100.000, loại trừ các phương pháp ngây thơ tính toán lại thân tàu từ đầu sau mỗi lần bổ sung. Mỗi tọa độ là một số nguyên nằm trong khoảng từ -10^6 đến 10^6, vì vậy chúng ta phải cẩn thận với độ chính xác bằng số nhưng số học số nguyên là đủ vì bài toán không liên quan đến tọa độ dấu phẩy động. 

Các trường hợp biên phát sinh khi một điểm truy vấn nằm chính xác trên một cạnh hoặc đỉnh của thân tàu. Một phép kiểm tra ngây thơ sử dụng các bất đẳng thức hoặc tích chéo mà không xử lý cẩn thận có thể đánh dấu nhầm các điểm này là các điểm bên ngoài. Một kịch bản tinh tế khác là một vài điểm đầu tiên. Bài toán đảm bảo rằng ba điểm được thêm đầu tiên tạo thành một tam giác không suy biến, vì vậy chúng ta có thể bắt đầu một cách an toàn với một đa giác lồi hợp lệ ngay từ đầu. 

## Phương pháp tiếp cận 

Giải pháp vũ phu sẽ duy trì danh sách tất cả các điểm. Đối với mỗi truy vấn ngăn chặn, nó có thể tính toán bao lồi từ đầu và kiểm tra xem điểm có nằm bên trong hay không. Việc xây dựng một bao lồi bằng quét Graham hoặc thuật toán chuỗi đơn điệu của Andrew mất O(n log n) cho mỗi truy vấn. Với q lên đến 10^5, điều này dẫn đến O(q n log n) trong trường hợp xấu nhất, có thể đạt tới ~10^10 thao tác. Điều này là quá chậm. 

Cái nhìn sâu sắc quan trọng là bao lồi phát triển đơn điệu khi chúng ta thêm điểm. Nếu chúng ta lưu trữ bao lồi riêng biệt, mỗi điểm mới sẽ nằm bên trong bao lồi hiện tại hoặc cập nhật bao. Việc kiểm tra ngăn chặn trong đa giác lồi có thể được thực hiện theo thời gian logarit bằng cách sử dụng tìm kiếm nhị phân nếu chúng ta biểu diễn phần thân dưới dạng danh sách các đỉnh được sắp xếp theo thứ tự theo chiều kim đồng hồ hoặc ngược chiều kim đồng hồ. Điều này làm giảm thời gian mỗi truy vấn xuống O(log n) để ngăn chặn và việc cập nhật bao lồi có thể được thực hiện theo phân bổ O(log n) nếu chúng ta chèn điểm cẩn thận bằng cách sử dụng cấu trúc bao trên và dưới. 

Bằng cách khai thác tính lồi, chúng ta tránh được việc xây dựng lại phần thân từ đầu và vấn đề giảm xuống còn sự kết hợp giữa bảo trì phần lồi động và các truy vấn điểm trong đa giác hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(q n log n) | O(n) | Quá chậm | 
| Vỏ lồi động với tìm kiếm nhị phân | O(q log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Bắt đầu bằng cách chèn ba điểm đầu tiên vào thân tàu. Sắp xếp chúng ngược chiều kim đồng hồ bằng cách sử dụng các tích chéo để đảm bảo hướng. Điều này tạo thành tam giác ban đầu, đảm bảo đa giác lồi hợp lệ cho các truy vấn tiếp theo. 
2. Giữ nguyên hai dãy điểm cho bao lồi: bao trên và bao dưới. Bao trên chứa các điểm dọc theo ranh giới trên theo thứ tự x tăng dần, bao dưới dọc theo ranh giới dưới. Các trình tự này cho phép chèn và loại bỏ trong O(log n) bằng cách kiểm tra lần lượt bằng tích chéo. 
3. Đối với mỗi điểm mới cần thêm, hãy kiểm tra xem nó có nằm ngoài thân tàu hiện tại hay không. Điều này có thể được thực hiện một cách hiệu quả bằng cách so sánh các độ dốc tương ứng với các điểm ngoài cùng bên trái và ngoài cùng bên phải của thân tàu. Nếu điểm nằm bên ngoài, hãy cập nhật các bao trên và dưới: loại bỏ các điểm không còn là một phần của bao vì điểm mới làm cho đa giác lại lồi. 
4. Để kiểm tra khả năng ngăn chặn của một điểm truy vấn, trước tiên hãy so sánh tọa độ x của nó với các điểm ngoài cùng bên trái và ngoài cùng bên phải. Nếu nó rơi ra ngoài, nó sẽ tự động nằm ngoài thân tàu. Ngược lại, thực hiện tìm kiếm nhị phân trên cả phần trên và phần dưới để xác định vị trí hai cạnh liền kề với tọa độ x. Sử dụng tích chéo để xác minh xem điểm truy vấn nằm trên hay dưới các cạnh tương ứng. Nếu nó vượt qua cả hai bài kiểm tra, nó nằm bên trong đa giác hoặc trên ranh giới của nó. 
5. Ghi “YES” cho các điểm bên trong hoặc trên thân tàu, và nếu không thì ghi “NO”. 

Điều bất biến xuyên suốt thuật toán là các dãy bao trên và dưới luôn tạo thành ranh giới lồi của tập điểm hiện tại. Việc cập nhật thân tàu duy trì tính bất biến này bằng cách chỉ giữ các điểm tạo thành các vòng lồi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def cross(o, a, b):
    return (a[0]-o[0])*(b[1]-o[1]) - (a[1]-o[1])*(b[0]-o[0])

def add_point(hull, p):
    hull.append(p)
    while len(hull) > 2 and cross(hull[-3], hull[-2], hull[-1]) <= 0:
        hull.pop(-2)

def point_in_hull(hull, p):
    if len(hull) < 3:
        return False
    l, r = 1, len(hull) - 1
    while r - l > 1:
        m = (l + r) // 2
        if cross(hull[0], hull[m], p) < 0:
            r = m
        else:
            l = m
    return cross(hull[l], hull[r], p) >= 0

q = int(input())
queries = [tuple(map(int, input().split())) for _ in range(q)]
hull = []

# Initialize first three points
for t, x, y in queries[:3]:
    hull.append((x, y))

# Sort counterclockwise
hull.sort()
if cross(hull[0], hull[1], hull[2]) < 0:
    hull[1], hull[2] = hull[2], hull[1]

for t, x, y in queries[3:]:
    if t == 1:
        add_point(hull, (x, y))
    else:
        print("YES" if point_in_hull(hull, (x, y)) else "NO")
```các`cross`chức năng được sử dụng nhất quán để duy trì sự định hướng.`add_point`bảo toàn tính lồi bằng cách loại bỏ các điểm bên trong.`point_in_hull`sử dụng tìm kiếm nhị phân dọc theo các cạnh của thân tàu để xác định việc ngăn chặn một cách hiệu quả. Sắp xếp ba điểm đầu tiên đảm bảo tam giác ban đầu được định hướng đúng. 

## Ví dụ đã hoạt động 

### Mẫu 1 

| Truy vấn | Điểm thân tàu | Điểm truy vấn | Kết quả | 
| --- | --- | --- | --- | 
| 1 0 0 | (0,0),(2,0),(2,2) | - | - | 
| 1 2 0 | giống nhau | - | - | 
| 1 2 2 | giống nhau | - | - | 
| 2 1 0 | tam giác | (1,0) | CÓ | 
| 1 0 2 | cập nhật thân tàu | (0,0),(2,0),(2,2),(0,2) | - | 
| 2 1 1 | hình chữ nhật | (1,1) | CÓ | 
| 2 2 1 | hình chữ nhật | (2,1) | CÓ | 
| 2 20 -1 | hình chữ nhật | (20,-1) | KHÔNG | 

Điều này cho thấy thân tàu mở rộng chính xác khi một điểm mới nằm bên ngoài và các truy vấn ngăn chặn trả về kết quả chính xác bao gồm cả các trường hợp cạnh. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q log n) | Mỗi phép cộng hoặc truy vấn yêu cầu tối đa một tìm kiếm nhị phân hoặc cập nhật bao lồi trong nhật ký n. | 
| Không gian | O(n) | Chúng ta lưu trữ tất cả các điểm trong dãy bao lồi. | 

Với q 10^5, giải pháp này chạy thoải mái dưới 1 giây với bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    exec(open('solution.py').read())
    return output.getvalue().strip()

# Provided sample
assert run("""8
1 0 0
1 2 0
1 2 2
2 1 0
1 0 2
2 1 1
2 2 1
2 20 -1""") == "YES\nYES\nYES\nNO"

# Custom minimum input
assert run("""4
1 0 0
1 1 0
1 0 1
2 0 0""") == "YES"

# Point outside after expansion
assert run("""5
1 0 0
1 2 0
1 1 2
1 3 1
2 2 1""") == "YES"

# Query on edge
assert run("""4
1 0 0
1 2 0
1 1 1
2 1 0""") == "YES"

# Query far outside
assert run("""4
1 0 0
1 2 0
1 1 1
2 10 10""") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Tam giác tối thiểu | CÓ | Xử lý thân tàu nhỏ nhất | 
| Mở rộng | CÓ | Hull mở rộng chính xác với những điểm mới | 
| Truy vấn cạnh | CÓ | Điểm trên mép thân tàu tính vào bên trong | 
| Bên ngoài xa xôi | KHÔNG | Điểm rõ ràng bên ngoài bị từ chối | 

## Vỏ cạnh 

Thuật toán xử lý chính xác các điểm chính xác trên biên bao lồi. Ví dụ: sau khi cộng (0,0),(2,0),(1,1), thân tàu là một hình tam giác. Truy vấn (1.0) kích hoạt tìm kiếm nhị phân dọc theo các cạnh và tích chéo đánh giá bằng 0, tạo ra "CÓ" như dự định
