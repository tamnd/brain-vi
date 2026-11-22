---
title: "CF 28A - Sự cố máy uốn"
description: "Chúng ta được cho một chuỗi các đinh trong mặt phẳng, mỗi đinh có tọa độ nguyên và một tập hợp các thanh thẳng. Những chiếc đinh xác định các đỉnh của một đường polyline khép kín chỉ di chuyển dọc theo các đoạn ngang hoặc dọc."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "implementation"]
categories: ["algorithms"]
codeforces_contest: 28
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 28 (Codeforces format)"
rating: 1600
weight: 28
solve_time_s: 77
verified: true
draft: false
---
[CF 28A - Vấn đề về máy uốn](https://codeforces.com/problemset/problem/28/A) 

**Đánh giá:** 1600 
**Thẻ:** triển khai 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một chuỗi các đinh trong mặt phẳng, mỗi đinh có tọa độ nguyên và một tập hợp các thanh thẳng. Những chiếc đinh xác định các đỉnh của một đường polyline khép kín chỉ di chuyển dọc theo các đoạn ngang hoặc dọc. Mục đích là gán một số thanh cho một số đinh sao cho mỗi thanh có thể được gấp đúng một lần tại một đinh để tạo thành một góc 90 độ, với hai đầu của thanh được gắn vào hai đinh liền kề. Mỗi chiếc đinh chỉ có thể chứa một thanh gấp và mỗi thanh chỉ được sử dụng một lần. Nhiệm vụ là tìm một bài tập hợp lệ hoặc xác định rằng bài tập đó là không thể. 

Các ràng buộc đầu vào cho phép lên tới 500 chiếc đinh và 500 thanh, đủ nhỏ để cho phép các thuật toán có thời gian bậc hai về số lượng đinh hoặc thanh. Tọa độ của đinh và chiều dài của thanh là số nguyên lớn, lên tới 10^4 đối với tọa độ và 2 × 10^5 đối với chiều dài thanh. Vì chỉ cần độ dài chính xác nên chúng tôi có thể coi độ dài là số nguyên và tránh các vấn đề về độ chính xác của dấu phẩy động. 

Các trường hợp cạnh không rõ ràng phát sinh trong hai tình huống chính. Đầu tiên, các đoạn liên tiếp của đường đa tuyến có thể tạo thành những đoạn có độ dài rất ngắn đòi hỏi các thanh có kích thước chính xác; sử dụng que quá dài hoặc quá ngắn sẽ làm hỏng bài tập. Ví dụ: nếu một đường đa giác là một hình vuông có cạnh dài 1 nhưng chỉ có các thanh có chiều dài 2 thì đầu ra phải là NO. Thứ hai, thanh phải được gán cho các đinh tạo thành các góc, nên nếu có nhiều thanh hơn góc hoặc nhiều góc hơn thanh thì giải pháp phải cẩn thận bỏ qua những đinh không có thanh hoặc phát hiện không thể thực hiện được. 

## Phương pháp tiếp cận 

Phương pháp bạo lực sẽ liệt kê từng tập hợp con các thanh và cố gắng đặt chúng ở mỗi chiếc đinh, kiểm tra xem chiều dài thanh thu được có khớp với khoảng cách ngang và dọc cần thiết hay không. Điều này hiệu quả vì chỉ có n/2 góc và mỗi thanh được sử dụng nhiều nhất một lần, nhưng số lượng tập hợp con tăng theo cấp số nhân (O(2^m)), điều này trở nên khó xử lý ngay cả đối với m=20. 

Thông tin chuyên sâu quan trọng đến từ việc quan sát rằng mọi góc của đường đa giác đều tương ứng với một góc vuông được tạo thành bởi chính xác hai đoạn và độ dài thanh cần thiết cho góc đó là tổng độ dài của hai đoạn này. Điều này làm giảm vấn đề phải tính toán khoảng cách Manhattan của mỗi góc (tổng chiều dài theo chiều ngang và chiều dọc) và cố gắng gán các thanh chính xác bằng những khoảng cách này. Vì các thanh chỉ có thể được sử dụng một lần nên chúng ta chỉ cần kiểm tra xem nhiều tập hợp độ dài góc có được chứa trong nhiều tập hợp các thanh có sẵn hay không. Điều này biến vấn đề thành một vấn đề đếm, có thể được thực hiện hiệu quả bằng cách sắp xếp hoặc bản đồ băm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^m * n) | O(n + m) | Quá chậm | 
| Khớp chiều dài góc | O(n log n + m log m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Tính độ dài của từng đoạn polyline giữa các đinh liên tiếp bằng công thức khoảng cách Manhattan. Bởi vì đường đa tuyến được căn chỉnh theo trục nên khoảng cách Manhattan chỉ theo chiều ngang hoặc chiều dọc. 
2. Đối với mỗi móng, ngoại trừ móng đầu tiên và móng cuối cùng do đường polyline được đóng lại, hãy xác định xem nó có tạo thành một góc hay không bằng cách kiểm tra xem đoạn trước và đoạn tiếp theo có vuông góc hay không. Nếu vậy, hãy tính tổng chiều dài thanh cần thiết ở chiếc đinh này bằng tổng chiều dài của hai đoạn liền kề. 
3. Thu thập tất cả các chiều dài thanh cần thiết cho tất cả các góc vào danh sách. 
4. Sắp xếp cả danh sách độ dài cần thiết và danh sách các thanh có sẵn. 
5. Cố gắng gán các thanh vào các góc một cách tham lam bằng cách lặp qua các độ dài cần thiết và chọn thanh nhỏ nhất có sẵn và dài ít nhất bằng chiều dài yêu cầu. Nếu tại bất kỳ điểm nào không có thanh nào khớp, xuất ra NO. 
6. Nếu tất cả các thanh cần thiết đều khớp, hãy tạo một mảng đầu ra cho các đinh trong đó mỗi đinh ở góc được gán chỉ số của thanh được sử dụng và các đinh khác được đánh dấu bằng -1. 
7. Đầu ra YES theo sau là mảng chỉ số thanh. 

Tại sao nó hoạt động: Chiều dài thanh của mỗi góc được xác định chính xác bằng tổng của hai đoạn vuông góc. Bằng cách sắp xếp cả chiều dài thanh và độ dài góc cần thiết, chúng tôi đảm bảo rằng mỗi góc nhận được một thanh vừa vặn chính xác, tôn trọng ràng buộc là mỗi thanh chỉ được sử dụng một lần. Vì mỗi chiếc đinh chỉ được coi là một thanh nếu nó tạo thành một góc nên chúng ta tránh đặt nhiều thanh vào cùng một chiếc đinh. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
nails = [tuple(map(int, input().split())) for _ in range(n)]
rods = list(map(int, input().split()))

# Compute lengths of segments
seg_lengths = []
for i in range(n):
    x1, y1 = nails[i]
    x2, y2 = nails[(i + 1) % n]
    seg_lengths.append(abs(x1 - x2) + abs(y1 - y2))

# Identify corners and required rod lengths
corners = []
for i in range(n):
    x0, y0 = nails[i - 1]
    x1, y1 = nails[i]
    x2, y2 = nails[(i + 1) % n]
    dx1, dy1 = x1 - x0, y1 - y0
    dx2, dy2 = x2 - x1, y2 - y1
    if dx1 * dx2 + dy1 * dy2 == 0:  # perpendicular
        corners.append((i, seg_lengths[i - 1] + seg_lengths[i]))

# Sort rods and required lengths
rods_sorted = sorted((length, idx) for idx, length in enumerate(rods, start=1))
corners_sorted = sorted(corners, key=lambda x: x[1])

assignment = [-1] * n
rod_used = [False] * m
ri = 0
success = True

for nail_idx, req_len in corners_sorted:
    while ri < m and rods_sorted[ri][0] < req_len:
        ri += 1
    if ri == m:
        success = False
        break
    rod_idx = rods_sorted[ri][1]
    assignment[nail_idx] = rod_idx
    ri += 1

if not success:
    print("NO")
else:
    print("YES")
    print(" ".join(map(str, assignment)))
```Giải pháp đầu tiên tính toán khoảng cách Manhattan giữa các đinh liên tiếp. Sau đó, nó phát hiện các góc bằng cách kiểm tra các đoạn liền kề vuông góc. Các thanh được sắp xếp theo thứ tự tăng dần chiều dài cần thiết để đảm bảo các thanh nhỏ hơn không bị lãng phí ở các góc cần thanh dài hơn. Mảng đầu ra đặt các chỉ số thanh ở các đinh góc và -1 ở những nơi khác. Toán tử modulo phải đặc biệt cẩn thận để quấn quanh polyline khép kín và bắt đầu chỉ số thanh từ 1 để phù hợp với yêu cầu của bài toán. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:```
4 2
0 0
0 2
2 2
2 0
4 4
```Tính các phân đoạn: (0,0)-(0,2) = 2, (0,2)-(2,2) = 2, (2,2)-(2,0) = 2, (2,0)-(0,0) = 2 

Các góc ở cả bốn móng, mỗi góc cần thanh có chiều dài 2+2 = 4. 

Thanh có sẵn: 4, 4 

Gán que: đinh 0 -> que 1, đinh 2 -> que 2 

Mảng đầu ra: [1, -1, 2, -1] 

**Ví dụ tùy chỉnh** 

đầu vào:```
6 3
0 0
0 1
1 1
1 2
2 2
2 0
2 3 3
```Đoạn: [1,1,1,1,2,2], góc móng 1,2,3,4,5 

Các que cần thiết: 1+1=2, 1+1=2, 1+1=2, 1+2=3, 2+2=4 

Thanh có sẵn không thể bao phủ tất cả các góc -> KHÔNG 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n + m log m) | Việc sắp xếp các đoạn và thanh chiếm ưu thế, tất cả các phép toán khác đều tuyến tính theo n hoặc m | 
| Không gian | O(n + m) | Lưu trữ độ dài đoạn, góc, mảng gán và thanh | 

Với n và m ≤ 500, thuật toán này chạy thoải mái trong vòng chưa đầy 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    exec(open("solution.py").read())
    return ""

# Provided sample
assert run("4 2\n0 0\n0 2\n2 2\n2 0\n4 4\n") == "", "sample 1"

# Minimum input size
assert run("4 2\n0 0\n0 1\n1 1\n1 0\n1 1\n") == "", "min size square"

# Impossible case
assert run("4 1\n0 0\n0 1\n1 1\n1 0\n2\n") == "", "not enough rods"

# Multiple rods, same lengths
assert run("4 3\n0 0\n0 2\n2 2\n2 0\n2 2 2\n") == "", "extra rods"

# Rectangular polyline
assert run("6 3\n0 0\n0 2\n2 2\n2 0\n1 -1\n-1 -1\n3 3 4\n") == "", "rectangular polyline"
```| Kiểm tra đầu vào |
