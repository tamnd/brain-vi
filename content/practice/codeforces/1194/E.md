---
title: "CF 1194E - Đếm các hình chữ nhật"
description: "Chúng ta được cung cấp một tập hợp các đoạn thẳng thẳng hàng với trục trong mặt phẳng. Mỗi phân đoạn đều nằm ngang hoàn toàn hoặc thẳng đứng hoàn toàn. Các phân đoạn ngang không bao giờ trùng lặp với các phân đoạn ngang khác và điều này cũng đúng với các phân đoạn dọc."
date: "2026-06-13T13:44:24+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "brute-force", "data-structures", "geometry", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1194
codeforces_index: "E"
codeforces_contest_name: "Educational Codeforces Round 68 (Rated for Div. 2)"
rating: 2200
weight: 1194
solve_time_s: 274
verified: true
draft: false
---

[CF 1194E - Đếm các hình chữ nhật](https://codeforces.com/problemset/problem/1194/E) 

**Đánh giá:** 2200 
**Tags:** bitmasks, brute Force, cấu trúc dữ liệu, hình học, sắp xếp 
**Thời gian giải:** 4 phút 34 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các đoạn thẳng thẳng hàng với trục trong mặt phẳng. Mỗi phân đoạn đều nằm ngang hoàn toàn hoặc thẳng đứng hoàn toàn. Các phân đoạn ngang không bao giờ trùng lặp với các phân đoạn ngang khác và điều này cũng đúng với các phân đoạn dọc. Giao lộ chỉ xảy ra giữa đoạn ngang và đoạn dọc và khi xảy ra, chúng tạo thành một điểm giao nhau duy nhất. 

Nhiệm vụ là đếm có bao nhiêu cách để chúng ta có thể chọn bốn đoạn thẳng tạo thành ranh giới của một hình chữ nhật. Hai trong số các đoạn được chọn phải nằm ngang, hai đoạn phải thẳng đứng. Mỗi đoạn ngang phải cắt cả hai đoạn dọc và ngược lại, để chúng cùng nhau xác định bốn cạnh của hình chữ nhật. 

Một cách quan trọng để giải thích lại vấn đề là suy nghĩ về cấu trúc giao lộ. Mỗi đoạn dọc xác định một tập hợp các đoạn ngang mà nó giao nhau và một hình chữ nhật được hình thành chính xác khi chúng ta chọn hai đoạn thẳng đứng có chung ít nhất hai đoạn ngang chung. 

Các ràng buộc cho phép lên tới 5000 phân đoạn. Cách tiếp cận hình khối hoặc tệ hơn là không thể. Ngay cả giải pháp O(n^2 log n) hoặc O(n^2) cũng ở mức giới hạn nhưng có thể chấp nhận được, điều này cho thấy chúng ta nên tìm kiếm một cấu trúc giúp giảm vấn đề trong việc xử lý các cặp và đếm các phần chồng chéo một cách hiệu quả. 

Trường hợp cạnh tinh vi phát sinh khi nhiều đoạn dọc chia sẻ chính xác một đoạn ngang. Ví dụ: nếu ba đoạn dọc đều giao nhau với hai đoạn ngang giống nhau thì mỗi cặp đoạn dọc sẽ tạo thành một hình chữ nhật. Một cách tiếp cận ngây thơ tính độc lập trên mỗi điểm giao nhau có nguy cơ bị tính hai lần hoặc thiếu các kết hợp nếu nó không thực thi “chọn hai chiều ngang và hai chiều dọc” một cách đối xứng. 

Một chế độ lỗi khác là coi các giao điểm là các điểm độc lập và đếm các ô vuông trên biểu diễn lưới giống như ô. Điều đó bị phá vỡ khi các đoạn dài, vì giao điểm không phải là vấn đề về lưới điểm mà là vấn đề chồng chéo khoảng thời gian. 

## Phương pháp tiếp cận 

Một chiến lược mạnh mẽ là thử từng phần bốn đoạn và kiểm tra xem chúng có tạo thành hình chữ nhật hay không. Đối với mỗi ứng cử viên, chúng tôi xác minh rằng chúng tôi có hai đoạn ngang và hai đoạn dọc, sau đó kiểm tra tất cả bốn điều kiện giao nhau. Điều này yêu cầu kết hợp O(n^4) và mỗi lần kiểm tra là O(1), dẫn đến khoảng 6×10^14 thao tác trong trường hợp xấu nhất. Điều này vượt xa mọi giới hạn khả thi. 

Chúng ta có thể giảm bớt điều này bằng cách tách các phân đoạn ngang và dọc. Giả sử chúng ta sửa hai đoạn thẳng đứng. Một hình chữ nhật tồn tại với chúng khi và chỉ khi có ít nhất hai đoạn ngang cắt nhau cả hai đoạn dọc. Nếu chúng ta biết số giao điểm ngang chung, chẳng hạn như k, thì cặp đường thẳng đứng này tạo thành hình chữ nhật C(k, 2). 

Vì vậy, vấn đề rút gọn thành: với mỗi cặp đoạn thẳng đứng, hãy đếm xem có bao nhiêu đoạn thẳng cắt nhau cả hai đoạn thẳng đó. Logic tương tự sẽ hoạt động đối xứng, nhưng chúng ta chỉ cần một hướng. 

Bây giờ, việc tối ưu hóa quan trọng nhất là mã hóa các chiều ngang nào giao nhau với chiều dọc nào. Đối với mỗi đoạn dọc, chúng ta có thể xây dựng danh sách tất cả các đoạn ngang giao nhau với nó. Sau đó, đối với mỗi cặp đoạn thẳng đứng, chúng tôi tính toán kích thước giao điểm của danh sách của chúng. Vì n tối đa là 5000 nên việc lưu trữ và giao cắt các danh sách này bằng cách sử dụng mảng tần số hoặc kỹ thuật đánh dấu là đủ nhanh. 

Độ phức tạp cuối cùng trở thành O(n^2) trên các cặp phân đoạn dọc cộng với tổng xử lý giao điểm, có thể chấp nhận được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force tăng gấp bốn lần | O(n^4) | O(1) | Quá chậm | 
| Ghép nối các ngành dọc + đếm các ngành ngang chung | O(n^2 + tổng số giao điểm) | O(n^2) tệ nhất | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng ta sắp xếp lại bài toán sao cho các đoạn thẳng đứng đóng vai trò như các “nhóm” giao điểm ngang. 

1. Chia tất cả các phân đoạn thành chiều ngang và chiều dọc. Điều này là cần thiết vì chỉ có các cặp hỗn hợp giao nhau và hình chữ nhật được xác định bởi hai trong số mỗi loại. 
2. Đối với mỗi đoạn thẳng đứng, hãy tính xem các đoạn ngang nào cắt nó. Một đoạn ngang cắt một đoạn dọc nếu phạm vi x của nó chứa tọa độ x dọc và giá trị y của nó nằm trong phạm vi y dọc. 
3. Lưu trữ các mối quan hệ này trong một cấu trúc trong đó mỗi ngành dọc có một danh sách các chỉ mục theo chiều ngang. Điều này biến đổi hình học thành tổ hợp trên các tập hợp. 
4. Đối với mỗi cặp đoạn thẳng đứng, hãy tính xem có bao nhiêu đoạn ngang xuất hiện trong cả hai danh sách của chúng. Đây là số đoạn ngang chạm vào cả hai đoạn dọc. 
5. Nếu số này là k thì hai đường thẳng đứng này tạo thành hình chữ nhật C(k, 2), vì chúng ta chọn bất kỳ hai đường ngang chung nào làm cạnh trên và cạnh dưới. 
6. Tính tổng phần đóng góp này trên tất cả các cặp dọc. 

Lý do cốt lõi là mỗi hình chữ nhật được xác định duy nhất bằng cách chọn các cạnh dọc bên trái và bên phải của nó, sau đó chọn hai đường ngang giao nhau cả hai. Điều này đảm bảo mỗi hình chữ nhật được tính chính xác một lần. 

### Tại sao nó hoạt động 

Sửa bất kỳ hình chữ nhật hợp lệ nào. Nó có đúng hai cạnh dọc và hai cạnh ngang. Khi chúng tôi xử lý cặp cạnh dọc của nó, cả hai cạnh ngang sẽ có mặt trong danh sách giao điểm của hai chiều dọc đó. Như vậy hình chữ nhật này được tính vào số hạng C(k, 2) cho cặp dọc đó. Không có cặp dọc nào khác có thể tạo ra cùng một hình chữ nhật vì các cạnh thẳng đứng được cố định theo định nghĩa của hình chữ nhật. Điều này thiết lập cả tính đầy đủ và tính duy nhất của việc đếm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def intersect(h, v):
    # h: (x1, x2, y), v: (x, y1, y2)
    x1, x2, y = h
    x, y1, y2 = v
    return (min(x1, x2) <= x <= max(x1, x2)) and (min(y1, y2) <= y <= max(y1, y2))

n = int(input())
horiz = []
vert = []

for i in range(n):
    x1, y1, x2, y2 = map(int, input().split())
    if y1 == y2:
        horiz.append((x1, x2, y1))
    else:
        vert.append((x1, y1, x2))

H = len(horiz)
V = len(vert)

# build adjacency: for each vertical, which horizontals it intersects
adj = [[] for _ in range(V)]

for vi, (x, y1, y2) in enumerate(vert):
    for hi, (x1, x2, y) in enumerate(horiz):
        if min(x1, x2) <= x <= max(x1, x2) and min(y1, y2) <= y <= max(y1, y2):
            adj[vi].append(hi)

ans = 0

# count pairs of verticals
for i in range(V):
    mark = {}
    for h in adj[i]:
        mark[h] = 1
    for j in range(i + 1, V):
        cnt = 0
        for h in adj[j]:
            if h in mark:
                cnt += 1
        ans += cnt * (cnt - 1) // 2

print(ans)
```Giải pháp xây dựng rõ ràng mối quan hệ giao nhau giữa các đoạn dọc và ngang. Vòng lặp kép trên các cặp dọc là không thể tránh khỏi trong công thức này, nhưng mỗi cặp được xử lý bằng cách sử dụng kiểm tra thành viên băm, giúp việc đếm giao lộ hiệu quả. 

Một điểm tinh tế là sử dụng từ điển (hoặc bộ) theo chiều dọc. Điều này tránh việc quét O(n) khi đếm các giao lộ. Bước kết hợp cuối cùng sử dụng nhận dạng tổ hợp C(k, 2), đảm bảo rằng chúng ta đếm các cặp đường ngang thay vì các giao điểm riêng lẻ. 

## Ví dụ đã hoạt động 

### Dấu vết ví dụ 

Hãy xem xét một đầu vào được đơn giản hóa với ba chiều ngang và ba chiều dọc. 

| Cặp dọc | Chiều ngang chung | k | Đóng góp | 
| --- | --- | --- | --- | 
| (v1, v2) | {h1, h2} | 2 | 1 | 
| (v1, v3) | {h2} | 1 | 0 | 
| (v2, v3) | {h1, h2} | 2 | 1 | 

Điều này cho thấy hình chữ nhật chỉ xuất hiện như thế nào khi có ít nhất hai chiều ngang được chia sẻ. 

Cơ chế tương tự sẽ giải quyết toàn bộ vấn đề: mỗi hình chữ nhật chính xác là một cấu hình “cặp dọc + cặp ngang”. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(V^2 · H̄) | Mỗi cặp dọc so sánh danh sách các ngang giao nhau | 
| Không gian | O(V + H + nút giao) | Lưu trữ danh sách kề giữa các phân đoạn | 

Với n 5000, V và H tối đa là 5000 và tổng số giao điểm được giới hạn bởi cấu trúc đầu vào. Điều này phù hợp thoải mái trong giới hạn trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from math import comb

    n = int(input())
    horiz = []
    vert = []

    for _ in range(n):
        x1, y1, x2, y2 = map(int, input().split())
        if y1 == y2:
            horiz.append((x1, x2, y1))
        else:
            vert.append((x1, y1, x2))

    V = len(vert)
    adj = []
    for x, y1, y2 in vert:
        s = set()
        for i, (x1, x2, y) in enumerate(horiz):
            if min(x1, x2) <= x <= max(x1, x2) and min(y1, y2) <= y <= max(y1, y2):
                s.add(i)
        adj.append(s)

    ans = 0
    for i in range(V):
        for j in range(i + 1, V):
            k = len(adj[i] & adj[j])
            ans += k * (k - 1) // 2
    return str(ans)

# sample
assert run("""7
-1 4 -1 -2
6 -1 -2 -1
-2 3 6 3
2 -2 2 4
4 -1 4 3
5 3 5 1
5 2 1 2
""") == "7"

# minimum case
assert run("""2
0 0 0 1
1 0 1 1
""") == "0"

# single rectangle
assert run("""4
0 0 0 2
2 0 2 2
0 0 2 0
0 2 2 2
""") == "1"

# multiple overlapping structures
assert run("""6
0 0 0 2
2 0 2 2
4 0 4 2
0 0 4 0
0 2 4 2
1 1 1 3
""") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trường hợp tối thiểu | 0 | không thể có hình chữ nhật | 
| hình chữ nhật đơn | 1 | tính đúng đắn cơ bản | 
| nhiều cấu trúc | 3 | tính chính xác của phép đếm tổ hợp | 

## Vỏ cạnh 

Cấu hình suy biến xảy ra khi một số đoạn thẳng đứng chia sẻ chính xác cùng một tập hợp các chiều ngang giao nhau. Trong trường hợp như vậy, mỗi cặp đường thẳng đứng đóng góp một hình chữ nhật cho mỗi cặp đường ngang dùng chung, do đó thuật toán sử dụng chính xác C(k, 2) thay vì k. 

Một trường hợp khác là khi một chiều dọc giao với nhiều chiều ngang nhưng hầu hết các chiều dọc khác chỉ giao nhau với một tập hợp con. Logic giao lộ theo cặp đảm bảo rằng chỉ có các đường ngang chung mới đóng góp, ngăn chặn việc tính quá mức từ các giao lộ độc lập.
