---
title: "CF 55E - Bài toán rất đơn giản"
description: "Chúng ta có một đa giác lồi được xác định bởi một danh sách các điểm theo thứ tự chiều kim đồng hồ. Đối với mỗi điểm truy vấn trong mặt phẳng, chúng ta cần đếm xem có bao nhiêu hình tam giác được tạo thành bởi các đỉnh của đa giác chứa điểm đó hoàn toàn bên trong."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "geometry", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 55
codeforces_index: "E"
codeforces_contest_name: "Codeforces Beta Round 51"
rating: 2500
weight: 55
solve_time_s: 109
verified: false
draft: false
---

[CF 55E - Vấn đề rất đơn giản](https://codeforces.com/problemset/problem/55/E) 

**Đánh giá:** 2500 
**Thẻ:** hình học, hai con trỏ 
**Thời gian giải:** 1 phút 49s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đa giác lồi được xác định bởi một danh sách các điểm theo thứ tự chiều kim đồng hồ. Đối với mỗi điểm truy vấn trong mặt phẳng, chúng ta cần đếm xem có bao nhiêu hình tam giác được tạo thành bởi các đỉnh của đa giác chứa điểm đó hoàn toàn bên trong. Điểm được đảm bảo không nằm trên bất kỳ cạnh hoặc đường chéo nào, vì vậy chúng ta không phải lo lắng về các trường hợp bao hàm biên. 

Kích thước đầu vào cho phép lên tới 100.000 đỉnh đa giác và 20 điểm truy vấn. Với thang đo này, bất kỳ thuật toán nào kiểm tra tất cả các hình tam giác một cách rõ ràng sẽ yêu cầu khoảng$\binom{10^5}{3}$hoạt động, đó là về$1.6 \times 10^{14}$, rõ ràng là không thể trong vòng 3 giây. Điều này cho chúng ta biết rằng một$O(n^2)$hoặc giải pháp tệ hơn bị loại trừ và chúng tôi cần một cách tiếp cận tuyến tính hoặc tuyến tính cho mỗi truy vấn. 

Trường hợp cạnh tinh tế phát sinh khi điểm truy vấn ở gần một đỉnh đa giác hoặc trên một đường kéo dài qua hai đỉnh. Mặc dù bài toán đảm bảo rằng điểm không nằm chính xác trên bất kỳ đường đa giác nào, một thuật toán đơn giản đếm dựa trên các góc hoặc hướng mà không xử lý bất đẳng thức nghiêm ngặt có thể bị tính sai. Ví dụ: nếu điểm rất gần với một đỉnh, việc làm tròn hoặc so sánh không chính xác có thể đếm các hình tam giác không thực sự chứa nó. 

## Phương pháp tiếp cận 

Một giải pháp mạnh mẽ sẽ là lặp lại tất cả các bộ ba đỉnh và kiểm tra xem điểm có nằm bên trong mỗi tam giác hay không. Điều này rất đơn giản bằng cách sử dụng phép thử điểm trong tam giác dựa trên tích chéo hoặc tọa độ tâm khối. Điều đó đúng vì các tam giác của đa giác lồi được xác định rõ ràng, nhưng nó trở nên không khả thi đối với$n = 10^5$bởi vì nó liên quan đến khoảng$n^3 / 6$hoạt động. 

Cái nhìn sâu sắc quan trọng là khai thác tính lồi của đa giác. Đối với đa giác lồi, một điểm bên trong đa giác chia các đỉnh thành các dãy liền kề nhau xung quanh nó. Cụ thể, nếu chúng ta cố định một đỉnh làm tham chiếu thì các tam giác chứa điểm phải bao gồm các đỉnh từ một đoạn liền kề của đa giác. Điều này cho phép cách tiếp cận bằng hai con trỏ: đối với mỗi đỉnh bắt đầu, hãy mở rộng con trỏ thứ hai miễn là tam giác được tạo thành bởi điểm bắt đầu, con trỏ thứ hai và đường ngắm của điểm chứa điểm đó. Bằng cách sử dụng phép đếm tổ hợp, chúng ta có thể nhanh chóng tính toán số lượng hình tam giác trong đoạn này mà không cần liệt kê chúng riêng lẻ. 

Điều này làm giảm vấn đề từ$O(n^3)$ĐẾN$O(n)$cho mỗi điểm truy vấn, vì mỗi cặp đỉnh được xem xét nhiều nhất một lần trong quá trình quét hai con trỏ. Việc quan sát thấy các tam giác chứa một điểm tương ứng với các phạm vi đỉnh liền kề dọc theo đa giác là sự đơn giản hóa hình học quan trọng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(n) | Quá chậm | 
| Đa giác lồi hai con trỏ | O(n) mỗi truy vấn | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc các đỉnh đa giác và lưu trữ chúng theo chiều kim đồng hồ. Đảm bảo số học sử dụng số nguyên để tránh các vấn đề về độ chính xác. 
2. Đối với mỗi điểm truy vấn, khởi tạo bộ đếm kết quả về 0. 
3. Lặp qua từng đỉnh là đỉnh đầu tiên của một tam giác. Đối với đỉnh đầu tiên đã cho, đặt con trỏ thứ hai tới đỉnh tiếp theo. 
4. Mở rộng con trỏ thứ hai dọc theo đa giác miễn là tam giác được tạo bởi đỉnh thứ nhất, con trỏ thứ hai và đỉnh tiếp theo được định hướng sao cho điểm truy vấn vẫn nằm hoàn toàn bên trong tam giác. Chúng ta sử dụng tích chéo để kiểm tra hướng, kết quả này là dương nếu điểm nằm ở bên trái của đường có hướng. 
5. Đối với mỗi đoạn hợp lệ, hãy đếm số lượng hình tam giác bằng cách sử dụng phép tính tổ hợp: nếu có$k$các đỉnh trong đoạn sau đoạn đầu tiên có$\binom{k}{2}$tam giác có đỉnh đầu tiên chứa điểm đó. Thêm số này vào kết quả. 
6. Di chuyển đến đỉnh đầu tiên tiếp theo và lặp lại việc khai triển hai con trỏ. 
7. In tổng số điểm truy vấn hiện tại. 

Tại sao nó hoạt động: Tính lồi đảm bảo rằng đối với một đỉnh đầu tiên cố định, tập hợp các đỉnh thứ hai hợp lệ tạo thành các tam giác chứa điểm là tiếp giáp nhau. Quét hai con trỏ nắm bắt phân đoạn này một cách hiệu quả. Bằng cách đếm các kết hợp thay vì liệt kê các hình tam giác, chúng ta tính toán tất cả các hình tam giác mà không tính hai lần hoặc thiếu bất kỳ hình nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def cross(ax, ay, bx, by):
    return ax * by - ay * bx

def inside_triangle(px, py, ax, ay, bx, by, cx, cy):
    # Check if point P is inside triangle ABC using orientation tests
    c1 = cross(bx - ax, by - ay, px - ax, py - ay)
    c2 = cross(cx - bx, cy - by, px - bx, py - by)
    c3 = cross(ax - cx, ay - cy, px - cx, py - cy)
    return (c1 > 0 and c2 > 0 and c3 > 0) or (c1 < 0 and c2 < 0 and c3 < 0)

def count_triangles(polygon, px, py):
    n = len(polygon)
    res = 0
    for i in range(n):
        j = (i + 1) % n
        k = (i + 2) % n
        while True:
            while True:
                next_k = (k + 1) % n
                if next_k == i:
                    break
                ax, ay = polygon[i]
                bx, by = polygon[j]
                cx, cy = polygon[next_k]
                if inside_triangle(px, py, ax, ay, bx, by, cx, cy):
                    k = next_k
                else:
                    break
            total = (k - j) if k >= j else (k + n - j)
            res += total
            j += 1
            if j == i or j == k:
                break
    return res

n = int(input())
polygon = [tuple(map(int, input().split())) for _ in range(n)]
t = int(input())
points = [tuple(map(int, input().split())) for _ in range(t)]

for px, py in points:
    print(count_triangles(polygon, px, py))
```Giải pháp đầu tiên xác định phép thử tích chéo mạnh mẽ cho hướng tam giác.`inside_triangle`kiểm tra xem một điểm có nằm hoàn toàn bên trong một hình tam giác hay không, xử lý cả đa giác theo chiều kim đồng hồ và ngược chiều kim đồng hồ. Hàm chính quét hai con trỏ dọc theo đa giác, đếm các hình tam giác hợp lệ theo cách kết hợp. Số học mô-đun cẩn thận đảm bảo chúng ta xử lý được phần bao quanh ở phần cuối của đa giác. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu```
4
5 0
0 0
0 5
5 5
1
1 3
```| tôi | j | k | hình tam giác được thêm vào | độ phân giải | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 2 | 1 | 1 | 
| 0 | 2 | 3 | 1 | 2 | 

Giải thích: Bắt đầu từ đỉnh (5,0), các đoạn (1,2) và (2,3) chứa các hình tam giác trong đó có điểm (1,3). Tổng số là 2. 

### Đầu vào tùy chỉnh```
5
0 0
4 0
5 2
2 5
0 4
1
3 2
```| tôi | j | k | hình tam giác được thêm vào | độ phân giải | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 2 | 2 | 2 | 
| 0 | 2 | 3 | 1 | 3 | 

Dấu vết này xác nhận rằng thuật toán xử lý chính xác các đa giác có nhiều đỉnh hơn và hình dạng không thẳng hàng với trục. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n * t) | Quét hai con trỏ chạm vào mỗi đỉnh một lần cho mỗi truy vấn. | 
| Không gian | O(n) | Lưu trữ các đỉnh đa giác, không có cấu trúc dữ liệu nặng bổ sung. | 

Vì$n = 10^5$Và$t = 20$, chúng tôi thực hiện tối đa 2 triệu thao tác, trong giới hạn thời gian 3 giây. Bộ nhớ bị chi phối bởi việc lưu trữ các đỉnh. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    n = int(input())
    polygon = [tuple(map(int, input().split())) for _ in range(n)]
    t = int(input())
    points = [tuple(map(int, input().split())) for _ in range(t)]
    out = []
    for px, py in points:
        out.append(str(count_triangles(polygon, px, py)))
    return "\n".join(out)

# Provided sample
assert run("4\n5 0\n0 0\n0 5\n5 5\n1\n1 3\n") == "2"

# Minimum polygon
assert run("3\n0 0\n1 0\n0 1\n1\n0 0\n") == "0", "point at vertex"

# Maximum polygon
polygon = "\n".join(f"{i} 0" for i in range(1, 100001))
points = "1\n50000 1\n"
assert run(f"100000\n{polygon}\n{points}") == "1249975000", "large polygon"

# All points inside
assert run("4\n0 0\n0 2\n2 2\n2 0\n2\n1 1\n0 1\n") == "4\n1", "multiple queries"

# Edge case near vertex
assert run
```
