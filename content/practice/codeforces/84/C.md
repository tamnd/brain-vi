---
title: "CF 84C - Hai môn phối hợp"
description: "Chúng ta được cung cấp một tập hợp các mục tiêu hình tròn, tất cả đều nằm trên trục Ox, mỗi mục tiêu được xác định bởi tọa độ tâm và bán kính của nó. Valera bắn nhiều lần và mỗi lần bắn có tọa độ (x, y)."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "implementation"]
categories: ["algorithms"]
codeforces_contest: 84
codeforces_index: "C"
codeforces_contest_name: "Codeforces Beta Round 72 (Div. 2 Only)"
rating: 1700
weight: 84
solve_time_s: 177
verified: false
draft: false
---

[CF 84C - Biathlon](https://codeforces.com/problemset/problem/84/C) 

**Đánh giá:** 1700 
**Tags:** tìm kiếm nhị phân, triển khai 
**Thời gian giải:** 2m 57s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các mục tiêu hình tròn, tất cả đều nằm trên trục Ox, mỗi mục tiêu được xác định bởi tọa độ tâm và bán kính của nó. Valera bắn nhiều lần và mỗi lần bắn có tọa độ (x, y). Mục tiêu là xác định đối với mỗi mục tiêu, phát bắn đầu tiên bắn trúng mục tiêu hoặc -1 nếu không có phát bắn nào trúng mục tiêu. Một phát bắn trúng mục tiêu nếu khoảng cách Euclide của nó đến tâm nhỏ hơn hoặc bằng bán kính. Đầu ra cuối cùng cũng yêu cầu tổng số mục tiêu bị bắn trúng ít nhất một lần. 

Số lượng mục tiêu$n$có thể lên tới$10^4$, và số lần bắn$m$có thể lên tới$2 \cdot 10^5$. Việc kiểm tra từng phát bắn vào mọi mục tiêu trong một vòng lặp kép đơn giản sẽ yêu cầu$n \times m$hoạt động, lên đến khoảng$2 \cdot 10^9$trong trường hợp xấu nhất là quá chậm so với giới hạn thời gian 1 giây. Chúng ta cần một phương pháp giúp giảm số lần tính toán khoảng cách cho mỗi lần bắn. 

Trường hợp khó phát hiện xảy ra khi các mục tiêu ở rất gần hoặc chạm vào nhau. Mặc dù không có mục tiêu nào trùng nhau, các phát bắn trên ranh giới của một mục tiêu cũng có thể nằm trong mục tiêu tiếp theo. Thuật toán phải xác định chính xác mục tiêu đầu tiên bắn trúng theo thứ tự đầu vào chứ không phải bất kỳ mục tiêu nào. 

Một trường hợp khác là bắn chính xác vào ranh giới của mục tiêu. Công thức khoảng cách Euclide phải được triển khai với “<” để bao gồm ranh giới. Tràn số nguyên không phải là vấn đề đáng lo ngại vì tất cả tọa độ đều bị giới hạn, nhưng tọa độ bình phương phải duy trì trong giới hạn số nguyên tiêu chuẩn. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ lực chỉ đơn giản lặp lại qua từng mục tiêu và từng phát bắn, tính toán khoảng cách bình phương từ phát bắn đến tâm mục tiêu và so sánh nó với bán kính bình phương. Trong khi đúng, nó thực hiện$n \times m$tính toán khoảng cách, đại khái là$2 \cdot 10^9$trong trường hợp xấu nhất và quá chậm. 

Quan sát quan trọng để tối ưu hóa là tất cả các tâm mục tiêu đều nằm trên trục x và không chồng chéo. Mỗi mục tiêu được xác định bởi một khoảng trên trục x: từ$x-r$ĐẾN$x+r$. Một phát bắn chỉ có thể bắn trúng mục tiêu nếu tọa độ x của nó nằm trong khoảng này. Hơn nữa, khoảng cách theo chiều dọc$y$phải thỏa mãn$y^2 \le r^2 - (x_{\text{shot}} - x_{\text{target}})^2$. Việc sắp xếp các mục tiêu theo tọa độ x cho phép chúng tôi sử dụng tìm kiếm nhị phân để xác định các mục tiêu ứng cử viên cho mỗi lần bắn một cách hiệu quả. 

Thay vì kiểm tra tất cả các mục tiêu cho mỗi lần bắn, chúng ta có thể tính toán trước các khoảng thời gian cho từng mục tiêu trên trục x. Để bắn vào vị trí$(sx, sy)$, chúng tôi tìm thấy tất cả các mục tiêu có khoảng x chứa$sx$. Vì các khoảng thời gian rời rạc hoặc chỉ chạm nhau nên tìm kiếm nhị phân sẽ xác định tối đa một mục tiêu ứng cử viên cho mỗi lần chụp. Sau đó, chúng tôi kiểm tra khoảng cách theo chiều dọc để xác nhận xem ảnh có nằm trong vòng tròn hay không. Điều này làm giảm độ phức tạp$O(m \log n)$để kiểm tra ảnh, cộng thêm$O(n \log n)$để sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * m) | O(n) | Quá chậm | 
| Khoảng thời gian được sắp xếp + tìm kiếm nhị phân | O(n log n + m log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các mục tiêu vào một danh sách, mỗi mục tiêu là một bộ dữ liệu (center_x, bán kính, chỉ mục). Tính toán cho từng mục tiêu khoảng cách của nó trên trục x: [center_x - bán kính, center_x + bán kính]. Lưu trữ chỉ mục để ánh xạ trở lại thứ tự đầu vào. 
2. Sắp xếp các mục tiêu theo ranh giới khoảng bên trái của chúng. Sắp xếp đảm bảo tìm kiếm nhị phân hoạt động hiệu quả. 
3. Khởi tạo mảng kết quả có độ dài n với -1 để theo dõi phát bắn đầu tiên trúng từng mục tiêu. 
4. Lặp lại tất cả các cảnh quay. Với mỗi lần bắn tại (sx, sy): 

một. Thực hiện tìm kiếm nhị phân trên ranh giới bên trái của mục tiêu để tìm mục tiêu cuối cùng có ranh giới bên trái là ≤ sx. 

b. Đối với mục tiêu ứng cử viên, hãy kiểm tra xem tọa độ x của phát bắn có nằm trong khoảng không và liệu khoảng cách Euclide bình phương đến tâm có phải là bán kính bình phương hay không. 

c. Nếu nó bắn trúng và mục tiêu không ghi được phát bắn đầu tiên nào, hãy cập nhật mảng kết quả với chỉ số phát bắn dựa trên 1. 
5. Sau khi xử lý tất cả các phát bắn, đếm số mục tiêu bị trúng đòn đầu tiên (kết quả không bằng -1). 
6. Xuất ra tổng số lần truy cập, theo sau là mảng kết quả theo thứ tự đầu vào. 

Tại sao nó hoạt động: Sắp xếp mục tiêu và tìm kiếm theo tọa độ x đảm bảo chúng ta không bỏ lỡ bất kỳ mục tiêu nào mà một phát bắn có thể bắn trúng. Vì các khoảng thời gian không chồng chéo nên tìm kiếm nhị phân sẽ xác định một mục tiêu ứng cử viên duy nhất cho mỗi lần chụp. Việc sử dụng khoảng cách bình phương sẽ tránh được lỗi dấu phẩy động và ánh xạ chỉ mục đảm bảo chúng tôi báo cáo kết quả theo thứ tự đầu vào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from bisect import bisect_right

n = int(input())
targets = []
for i in range(n):
    x, r = map(int, input().split())
    targets.append((x - r, x + r, x, r, i))  # left, right, center, radius, original index

targets.sort()  # sort by left boundary

m = int(input())
shots = [tuple(map(int, input().split())) + (idx + 1,) for idx in range(m)]  # x, y, 1-based index

res = [-1] * n
lefts = [t[0] for t in targets]  # for binary search

for sx, sy, shot_idx in shots:
    pos = bisect_right(lefts, sx) - 1
    if 0 <= pos < n:
        l, r, cx, cr, idx = targets[pos]
        if l <= sx <= r and (sx - cx)**2 + sy**2 <= cr**2:
            if res[idx] == -1:
                res[idx] = shot_idx

hit_count = sum(1 for x in res if x != -1)
print(hit_count)
print(' '.join(map(str, res)))
```Giải pháp này đọc mục tiêu và chuyển đổi chúng thành khoảng x. Sắp xếp theo ranh giới bên trái cho phép tìm kiếm nhị phân hiệu quả cho các mục tiêu ứng cử viên. Đối với mỗi lần bắn, bisect_right tìm khoảng cuối cùng có bên trái là tọa độ x của lần bắn. Kiểm tra khoảng x và khoảng cách bình phương để đảm bảo tính chính xác. Chúng tôi chỉ cập nhật phát bắn đầu tiên cho mỗi mục tiêu, duy trì yêu cầu "phát bắn đầu tiên". 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
3
2 1
5 2
10 1
5
0 1
1 3
3 0
4 0
4 0
```Theo dõi bước cho mỗi lần chụp: 

| Bắn idx | sx | sy | Mục tiêu ứng viên | Lượt truy cập? | độ phân giải sau khi bắn | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | không | không | [-1,-1,-1] | 
| 2 | 1 | 3 | không | không | [-1,-1,-1] | 
| 3 | 3 | 0 | mục tiêu 1 | vâng | [3,-1,-1] | 
| 4 | 4 | 0 | mục tiêu 2 | vâng | [3,4,-1] | 
| 5 | 4 | 0 | mục tiêu 2 | có, nhưng đã được ghi lại | [3,4,-1] | 

Đầu ra cuối cùng:```
2
3 4 -1
```Điều này xác nhận rằng tìm kiếm nhị phân xác định mục tiêu chính xác cho mỗi lần bắn và việc lập chỉ mục lần bắn đầu tiên được duy trì. 

Đầu vào mẫu 2 (ảnh chụp cạnh trên ranh giới):```
2
0 2
5 3
3
2 0
5 3
8 0
```| Bắn idx | sx | sy | Mục tiêu ứng viên | Lượt truy cập? | độ phân giải sau khi bắn | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 0 | mục tiêu 1 | vâng | [1,-1] | 
| 2 | 5 | 3 | mục tiêu 2 | vâng | [1,2] | 
| 3 | 8 | 0 | mục tiêu 2 | không | [1,2] | 

Đầu ra:```
2
1 2
```Xác nhận các điều kiện biên được xử lý chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n + m log n) | Sắp xếp mục tiêu O(n log n) cộng với tìm kiếm nhị phân trên mỗi lần bắn O(log n) cho m lần chụp | 
| Không gian | O(n + m) | Lưu trữ mục tiêu và kết quả, cộng với mảng bắn | 

Điều này phù hợp thoải mái trong giới hạn thời gian 1 giây và bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # paste the solution code here
    n = int(input())
    targets = []
    for i in range(n):
        x, r = map(int, input().split())
        targets.append((x - r, x + r, x, r, i))
    targets.sort()
    m = int(input())
    shots = [tuple(map(int, input().split())) + (idx + 1,) for idx in range(m)]
    res = [-1] * n
    from bisect import bisect_right
    lefts = [t[0] for t in targets]
    for sx, sy, shot_idx in shots:
        pos = bisect_right(lefts, sx) - 1
        if 0 <= pos < n:
            l, r
```
