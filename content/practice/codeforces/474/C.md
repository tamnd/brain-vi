---
title: "CF 474C - Thuyền Trưởng Marmot"
description: "Chúng tôi được yêu cầu giúp Đại úy Marmot tổ chức trung đoàn chuột chũi của anh ấy để mỗi nhóm bốn người tạo thành một hình vuông hoàn hảo trên một mặt phẳng. Mỗi nốt ruồi bắt đầu ở một vị trí (xi, yi) và có “nhà” tại (ai, bi)."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "geometry"]
categories: ["algorithms"]
codeforces_contest: 474
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 271 (Div. 2)"
rating: 2000
weight: 474
solve_time_s: 72
verified: true
draft: false
---

[CF 474C - Thuyền trưởng Marmot](https://codeforces.com/problemset/problem/474/C) 

**Xếp hạng:** 2000 
**Tags:** lực lượng vũ phu, hình học 
**Thời gian giải:** 1 phút 12s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu giúp Đại úy Marmot tổ chức trung đoàn chuột chũi của anh ấy để mỗi nhóm bốn người tạo thành một hình vuông hoàn hảo trên một mặt phẳng. Mỗi nốt ruồi bắt đầu ở một vị trí`(xi, yi)`và có một “ngôi nhà” tại`(ai, bi)`. Hoạt động duy nhất được phép là xoay nốt ruồi 90 độ ngược chiều kim đồng hồ quanh nhà của nó. Mỗi vòng quay được tính là một nước đi và mục tiêu là giảm thiểu tổng số nước đi cần thiết để tạo thành một hình vuông cho mỗi trung đoàn. 

Đầu vào bao gồm`n`trung đoàn, mỗi trung đoàn có 4 nốt ruồi, vậy tổng cộng chúng ta có`4*n`nốt ruồi. Tọa độ nằm trong khoảng từ`-10^4`ĐẾN`10^4`. Được cho`n`có thể lên tới 100, chúng ta có nhiều nhất là 400 nốt ruồi. Đầu ra là số lần di chuyển tối thiểu cho mỗi trung đoàn hoặc -1 nếu không thể tạo thành một hình vuông. 

Các trường hợp cạnh bao gồm các tình huống trong đó tất cả các nốt ruồi đều ở cùng một vị trí, không thể tạo thành một hình vuông không suy biến hoặc khi yêu cầu nhiều phép quay trên cùng một nốt ruồi. Một cách tiếp cận ngây thơ chỉ kiểm tra một phép quay cố định trên mỗi nốt ruồi có thể thất bại, bởi vì chúng ta cần khám phá tất cả các phép quay 0-3 cho mỗi nốt ruồi và tìm ra sự kết hợp tạo thành một hình vuông với những bước di chuyển tối thiểu. 

## Phương pháp tiếp cận 

Phương pháp vũ phu rất đơn giản: đối với mỗi trung đoàn, hãy thử tất cả các góc quay có thể có (0, 90, 180, 270 độ) cho cả bốn nốt ruồi. Vì mỗi nốt ruồi có 4 lựa chọn nên có 4^4 = 256 tổ hợp cho mỗi trung đoàn. Đối với mỗi kết hợp, chúng tôi kiểm tra xem bốn điểm kết quả có tạo thành hình vuông hay không. Điều này khả thi với ràng buộc n ≤ 100, vì tổng cộng có 256 * 100 = 25.600 lượt kiểm tra. 

Cái nhìn sâu sắc quan trọng để tối ưu hóa là nhận ra rằng 256 kết hợp trên mỗi trung đoàn là đủ nhỏ để không cần phải cắt tỉa thêm. Công việc chính là kiểm tra một cách hiệu quả xem bốn điểm có tạo thành một hình vuông hay không. Hình vuông có độ dài các cạnh bằng nhau và các đường chéo bằng nhau. Thay vì tính toán góc hoặc sử dụng dấu phẩy động, chúng ta có thể sử dụng khoảng cách bình phương để tránh sai sót về độ chính xác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n * 4^4) | O(1) | Đã chấp nhận | 
| Tối ưu | O(n * 4^4) | O(1) | Đã chấp nhận | 

Cách tiếp cận tối ưu về cơ bản là cách tiếp cận vũ phu được triển khai cẩn thận với số học số nguyên để kiểm tra các bình phương. 

## Hướng dẫn thuật toán 

1. Đọc số trung đoàn`n`. 
2. Đối với mỗi trung đoàn, hãy đọc vị trí và tọa độ nhà của bốn nốt ruồi. 
3. Đối với mỗi nốt ruồi, hãy tính toán trước bốn vị trí có thể có của nó sau 0, 1, 2 hoặc 3 lần quay quanh nhà của nó. Công thức xoay là`(x, y)`xoay quanh`(a, b)`90 độ ngược chiều kim đồng hồ:`(a - (y-b), b + (x-a))`. 
4. Tạo tất cả 4^4 = 256 tổ hợp phép quay, mỗi tổ hợp phép quay một nốt ruồi. Theo dõi tổng số bước di chuyển cho mỗi tổ hợp (tổng số phép quay được sử dụng). 
5. Đối với mỗi kết hợp, hãy kiểm tra xem bốn điểm thu được có tạo thành một hình vuông không suy biến hay không: 

- Tính toán tất cả các khoảng cách bình phương theo cặp. Phải có chính xác hai khoảng cách duy nhất: cạnh càng nhỏ, đường chéo càng lớn. 
- Đảm bảo khoảng cách đường chéo xảy ra chính xác hai lần, khoảng cách cạnh xảy ra đúng bốn lần và khoảng cách cạnh > 0. 
6. Theo dõi tổng số lần di chuyển tối thiểu trên tất cả các tổ hợp ô vuông hợp lệ. Nếu không có sự kết hợp hợp lệ nào, ghi -1 cho trung đoàn đó. 
7. In kết quả cho tất cả các trung đoàn. 

Tại sao nó hoạt động: Bằng cách tạo ra tất cả các phép quay có thể có và kiểm tra từng kết hợp, chúng tôi đảm bảo rằng nếu có thể có cấu hình hình vuông, chúng tôi sẽ tìm thấy nó. Việc sử dụng khoảng cách bình phương đảm bảo tính chính xác mà không có lỗi dấu phẩy động. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from itertools import product
from collections import Counter

def rotate_90(x, y, a, b):
    # rotate (x, y) 90 degrees CCW around (a, b)
    return a - (y - b), b + (x - a)

def is_square(points):
    dists = []
    for i in range(4):
        for j in range(i+1, 4):
            dx = points[i][0] - points[j][0]
            dy = points[i][1] - points[j][1]
            dists.append(dx*dx + dy*dy)
    count = Counter(dists)
    if len(count) != 2:
        return False
    side, diag = sorted(count.keys())
    return count[side] == 4 and count[diag] == 2 and side > 0

n = int(input())
results = []
for _ in range(n):
    moles = [tuple(map(int, input().split())) for _ in range(4)]
    rotations = []
    for x, y, a, b in moles:
        r = [(x, y)]
        for _ in range(3):
            x, y = rotate_90(x, y, a, b)
            r.append((x, y))
        rotations.append(r)
    min_moves = float('inf')
    for choice in product(range(4), repeat=4):
        points = [rotations[i][choice[i]] for i in range(4)]
        moves = sum(choice)
        if is_square(points):
            min_moves = min(min_moves, moves)
    results.append(-1 if min_moves == float('inf') else min_moves)

for res in results:
    print(res)
```Mã này trước tiên tính toán tất cả các vị trí có thể có cho mỗi nốt ruồi, sau đó lặp lại tất cả các kết hợp xoay bằng cách sử dụng`itertools.product`. các`is_square`đảm bảo chúng ta xác định chính xác các hình vuông mà không có lỗi chính xác bằng cách làm việc với khoảng cách bình phương. Các bước di chuyển tối thiểu được theo dõi cho mỗi trung đoàn và -1 được trả về nếu không có cấu hình nào hoạt động. 

## Ví dụ đã hoạt động 

Đối với đầu vào mẫu đầu tiên, các phép quay cho trung đoàn đầu tiên tạo ra các vị trí có thể có: 

| nốt ruồi | 0° | 90° | 180° | 270° | 
| --- | --- | --- | --- | --- | 
| 1 | (1,1) | (-1,1) | (-1,-1) | (1,-1) | 
| 2 | (-1,1) | (-1,-1) | (1,-1) | (1,1) | 
| 3 | (-1,1) | (-1,-1) | (1,-1) | (1,1) | 
| 4 | (1,-1) | (1,1) | (-1,1) | (-1,-1) | 

Đang thử kết hợp`(0,1,0,0)`mang lại điểm`(1,1)`,`(-1,-1)`,`(-1,1)`,`(1,-1)`, tạo thành một hình vuông. Tổng số nước đi là 1. Không có sự kết hợp nào mang lại ít nước đi hơn. 

Đối với trung đoàn thứ hai, không có sự kết hợp phép quay nào tạo ra bốn điểm tạo thành một hình vuông. Tất cả các lần thử đều mang lại điểm thẳng hàng hoặc cấu hình vùng bằng 0, do đó đầu ra là -1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * 4^4) | Mỗi trung đoàn có 4 nốt ruồi với 4 lựa chọn xoay → 256 tổ hợp; n 100, do đó 256*100 = 25.600 lượt kiểm tra. Kiểm tra khoảng cách mất O(6) cho mỗi kết hợp. | 
| Không gian | O(4*4) mỗi trung đoàn | Lưu trữ 4 vị trí trên mỗi nốt ruồi trong 4 lần quay; bộ nhớ bổ sung liên tục. | 

Với tổng số thao tác thấp (~ 25k) và cấu trúc dữ liệu nhỏ, điều này phù hợp thoải mái trong giới hạn thời gian 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io
def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline
    from itertools import product
    from collections import Counter

    def rotate_90(x, y, a, b):
        return a - (y - b), b + (x - a)

    def is_square(points):
        dists = []
        for i in range(4):
            for j in range(i+1, 4):
                dx = points[i][0] - points[j][0]
                dy = points[i][1] - points[j][1]
                dists.append(dx*dx + dy*dy)
        count = Counter(dists)
        if len(count) != 2:
            return False
        side, diag = sorted(count.keys())
        return count[side] == 4 and count[diag] == 2 and side > 0

    n = int(input())
    results = []
    for _ in range(n):
        moles = [tuple(map(int, input().split())) for _ in range(4)]
        rotations = []
        for x, y, a, b in moles:
            r = [(x, y)]
            for _ in range(3):
                x, y = rotate_90(x, y, a, b)
                r.append((x, y))
            rotations.append(r)
        min_moves = float('inf')
        for choice in product(range(4), repeat=4):
            points = [rotations[i][choice[i]] for i in range(4)]
            moves = sum(choice)
            if is_square(points):
                min_moves = min(min_moves, moves)
        results.append(-1 if min_moves == float('inf') else min_moves)

    return "\n".join(map(str, results))

# Provided sample
assert run("""4
1 1 0 0
-1 1 0 0
-1 1 0
```
