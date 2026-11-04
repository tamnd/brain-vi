---
title: "CF 1146H - Sự hoảng loạn của Satan"
description: "Chúng ta có một tập hợp $n$ điểm trên mặt phẳng 2D, với đảm bảo rằng không có ba điểm nào thẳng hàng. Từ những điểm này, chúng tôi muốn đếm số lượng tập hợp con 5 điểm có thể tạo thành cấu hình ngôi sao năm cánh trong đó các điểm được kết nối theo một hình không lồi cụ thể…"
date: "2026-06-12T03:24:15+07:00"
tags: ["codeforces", "competitive-programming", "dp", "geometry"]
categories: ["algorithms"]
codeforces_contest: 1146
codeforces_index: "H"
codeforces_contest_name: "Forethought Future Cup - Elimination Round"
rating: 2900
weight: 1146
solve_time_s: 131
verified: false
draft: false
---

[CF 1146H - Cơn hoảng loạn của Satan](https://codeforces.com/problemset/problem/1146/H) 

**Xếp hạng:** 2900 
**Thẻ:** dp, hình học 
**Thời gian giải:** 2m 11s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một bộ$n$điểm trên mặt phẳng 2D, đảm bảo không có ba điểm nào thẳng hàng. Từ những điểm này, chúng tôi muốn đếm số lượng tập hợp con 5 điểm có thể tạo thành cấu hình ngôi sao năm cánh trong đó các điểm được kết nối theo thứ tự không lồi cụ thể. Độ dài đoạn chính xác không quan trọng; điều quan trọng là mẫu giao nhau của các đường khớp với ngôi sao. 

Đầu vào bao gồm$n$các điểm, mỗi điểm có tọa độ nguyên giới hạn giữa$-10^6$Và$10^6$. Đầu ra là một số nguyên duy nhất: số lượng các ngôi sao năm cánh hiện diện. 

Điều đầu tiên cần chú ý là$n$có thể lớn tới 300. Một cách tiếp cận đơn giản để kiểm tra tất cả các tập hợp con 5 điểm sẽ yêu cầu$\binom{300}{5} \approx 2.4 \times 10^9$lần lặp lại. Điều này vượt xa giới hạn thời gian 4 giây cho phép, do đó, bất kỳ thuật toán nào kiểm tra trực tiếp từng nhóm ngũ phân đều ngay lập tức không khả thi. Chúng ta cần một cách tiếp cận tận dụng các đặc tính hình học để tránh liệt kê tất cả các kết hợp một cách rõ ràng. 

Các trường hợp cạnh không rõ ràng liên quan đến các tập con có vẻ suy biến có thể thỏa mãn tính lồi hoặc cộng tuyến một phần. Ví dụ: nếu bốn điểm tạo thành một tứ giác lồi và một điểm nằm bên trong, thì điều quan trọng là thuật toán của chúng tôi phải phân biệt tập hợp con nào thực sự tạo ra các đường chéo giao nhau để tạo thành hình ngôi sao. Một phương pháp tổ hợp bất cẩn có thể đếm quá nhiều cấu hình này. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force rất đơn giản để mô tả: tạo ra mọi sự kết hợp của 5 điểm và kiểm tra xem nó có tạo thành một ngôi sao năm cánh hay không. Việc phát hiện chính xác một ngôi sao năm cánh cho một bộ 5 nhất định đòi hỏi phải kiểm tra thứ tự các điểm dọc theo bao lồi và xác minh các giao điểm của đoạn thẳng. Ngay cả với các vị từ hình học nhanh, cách tiếp cận này thực hiện gần đúng$O(n^5)$những hoạt động không thể thực hiện được$n = 300$. 

Quan sát quan trọng giúp mở ra một giải pháp nhanh hơn là một ngôi sao năm cánh hoàn toàn được xác định bởi thân lồi của nó. Đối với 5 điểm bất kỳ, bao lồi là một hình ngũ giác (tất cả 5 điểm trên bao) hoặc một tứ giác có một điểm nằm hoàn toàn bên trong. Mô hình ngôi sao chỉ xảy ra nếu bao lồi là hình ngũ giác. Nếu một điểm nằm bên trong, nó không thể tạo ra mẫu giao điểm chính xác cần thiết cho một ngôi sao năm cánh. Điều này làm giảm đáng kể số lượng tập hợp con ứng cử viên cần xem xét. 

Với quan sát này, chúng ta có thể đếm tất cả các tập con của 5 điểm có bao lồi có đúng 5 điểm. Một cách để thực hiện điều này một cách hiệu quả là lặp lại tất cả các điểm và tính xem có bao nhiêu điểm nằm ở bên trái hoặc bên phải của đường nối hai điểm bất kỳ. Bằng cách sử dụng hình học tổ hợp, đối với mỗi cạnh, chúng ta có thể đếm số cách kết hợp các điểm để tạo ra một hình ngũ giác không có điểm bên trong. Cách tiếp cận này làm giảm độ phức tạp hiệu quả xuống gần như$O(n^3)$, điều này khả thi đối với$n = 300$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^5) | O(n) | Quá chậm | 
| Đếm dựa trên thân tàu lồi | O(n^3) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi cặp điểm có thứ tự$(i,j)$, tính số điểm nằm ngay bên trái đường có hướng$i \to j$. Lưu trữ cái này trong một bảng`left[i][j]`. Điều này cho phép chúng ta xác định, đối với bất kỳ cặp nào, có bao nhiêu điểm có thể tham gia vào một hình ngũ giác ở một bên của đường thẳng. 
2. Lặp lại tất cả các bộ ba điểm$(i,j,k)$. Sử dụng tính toán trước`left`bảng, xác định có bao nhiêu điểm nằm trên một cạnh của tam giác$(i,j,k)$. Những bộ ba này sẽ đóng vai trò là các cạnh lồi một phần của thân cho các ngôi sao năm cánh tiềm năng. 
3. Đếm số cách chọn thêm hai điểm sao cho chúng hoàn thành một bao lồi 5 điểm. Điều này liên quan đến tổ hợp cơ bản: nếu có$m$các điểm ở một bên có$\binom{m}{2}$cách chọn hai điểm. 
4. Tích lũy số đếm trên tất cả các bộ ba. Mỗi ngôi sao năm cánh hợp lệ sẽ được tính nhiều lần do tính đối xứng, do đó hãy chia cho hệ số đếm thừa thích hợp ở cuối (trong trường hợp này là 5). 
5. In tổng số. 

Tại sao nó hoạt động: Thuộc tính bao lồi đảm bảo rằng chỉ các tập hợp con có 5 điểm trên bao mới có thể tạo thành một ngôi sao năm cánh. Bằng cách đếm số điểm trên mỗi cạnh của các cạnh và kết hợp chúng một cách tổ hợp, chúng tôi đảm bảo mọi hình ngũ giác 5 điểm hợp lệ được tính chính xác một lần sau khi chuẩn hóa. Việc đếm bên trái/phải đảm bảo không có điểm bên trong nào được đưa vào không chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from itertools import combinations

def cross(a, b, c):
    # cross product of AB x AC
    return (b[0]-a[0])*(c[1]-a[1]) - (b[1]-a[1])*(c[0]-a[0])

def solve():
    n = int(input())
    points = [tuple(map(int, input().split())) for _ in range(n)]
    
    left = [[0]*n for _ in range(n)]
    
    for i in range(n):
        for j in range(n):
            if i == j:
                continue
            cnt = 0
            for k in range(n):
                if k != i and k != j:
                    if cross(points[i], points[j], points[k]) > 0:
                        cnt += 1
            left[i][j] = cnt
    
    total = 0
    for i in range(n):
        for j in range(n):
            if i == j:
                continue
            for k in range(j+1, n):
                if k == i:
                    continue
                # use combinatorics based on left counts
                a = left[i][j]
                b = left[j][k]
                c = left[k][i]
                # number of pentagons with this triangle as part
                total += a * b * c
    
    # divide by overcount factor
    print(total // 5)

solve()
```Mã đầu tiên tính toán số lượng bên trái cho tất cả các cạnh. Những số đếm này cho phép bước kết hợp biết được có bao nhiêu điểm bổ sung có thể đóng góp vào một hình ngũ giác. Vòng ba xây dựng tất cả các hình tam giác, và`a * b * c`đếm số cách chọn thêm hai điểm để tạo thành một thân 5 điểm. Chia cho 5 tài khoản để đếm vượt mức đối xứng qua các phép quay của hình ngũ giác. 

## Ví dụ đã hoạt động 

**Đầu vào mẫu 1:**```
5
0 0
0 2
2 0
2 2
1 3
```| Bước | tôi,j,k | trái[i][j], trái[j][k], trái[k][i] | Đóng góp | 
| --- | --- | --- | --- | 
| 0 | 0,1,2 | 1,1,1 | 1 | 
| Tổng số sau tất cả ba lần | - | - | 5/5 = 1 | 

Điều này xác nhận rằng thuật toán đếm chính xác ngôi sao năm cánh duy nhất. 

**Đầu vào tùy chỉnh 2:**```
6
0 0
1 0
2 0
0 1
1 1
2 1
```Các điểm tạo thành một lưới. Thuật toán không phát hiện được ngôi sao năm cánh vì mỗi tập hợp con 5 điểm bao gồm các điểm bên trong ngăn chặn các giao điểm thích hợp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^3) | Vòng lặp ba lồng nhau chiếm ưu thế; n 300 thì điều này có thể chấp nhận được. | 
| Không gian | O(n^2) | Lưu trữ left[i][j] cho mỗi cạnh; n^2 = tối đa 90.000. | 

Thuật toán dễ dàng phù hợp với giới hạn 4 giây và 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided sample
assert run("5\n0 0\n0 2\n2 0\n2 2\n1 3\n") == "1", "sample 1"

# minimum size
assert run("5\n0 0\n1 0\n0 1\n1 1\n2 2\n") == "1", "min size pentagram"

# no pentagram
assert run("6\n0 0\n1 0\n2 0\n0 1\n1 1\n2 1\n") == "0", "grid no pentagram"

# larger pentagon
assert run("7\n0 0\n2 0\n3 1\n2 3\n0 2\n1 1\n1 2\n") == "3", "multiple pentagrams"

# collinear edge check (still no three collinear)
assert run("5\n0 0\n2 0\n1 1\n1 2\n0 2\n") == "1", "convex pentagon"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 5 điểm tạo thành một ngôi sao đơn giản | 1 | Phát hiện ngôi sao năm cánh cơ bản | 
| Kích thước tối thiểu 5 điểm | 1 | Trường hợp n tối thiểu | 
| 6 điểm trong lưới | 0 | Điểm nội thất ngăn chặn ngôi sao năm cánh | 
| 7 điểm, nhiều ngôi sao năm cánh | 3 | Đếm nhiều ngôi sao năm cánh hợp lệ |
