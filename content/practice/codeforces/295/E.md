---
title: "CF 295E - Yaroslav và Điểm"
description: "Chúng ta có một tập hợp các điểm trên trục số. Mỗi điểm có một đặc điểm nhận dạng nên khi có bản cập nhật cho biết \"di chuyển điểm p theo d\", chỉ điểm cụ thể đó mới thay đổi vị trí. Có hai hoạt động."
date: "2026-06-05T17:48:14+07:00"
tags: ["codeforces", "competitive-programming", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 295
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 179 (Div. 1)"
rating: 2500
weight: 295
solve_time_s: 139
verified: true
draft: false
---

[CF 295E - Yaroslav và Điểm](https://codeforces.com/problemset/problem/295/E) 

**Đánh giá:** 2500 
**Thẻ:** cấu trúc dữ liệu 
**Thời gian giải:** 2m 19s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp các điểm trên trục số. Mỗi điểm có một đặc điểm nhận dạng, vì vậy khi có bản cập nhật thông báo "di chuyển điểm`p`qua`d`", chỉ có điểm cụ thể đó mới thay đổi vị trí. 

Có hai hoạt động. 

Thao tác đầu tiên sẽ xóa một điểm khỏi tọa độ hiện tại của nó và chèn nó vào tọa độ mới. Tuyên bố đảm bảo rằng tọa độ vẫn khác biệt sau mỗi lần cập nhật. 

Hoạt động thứ hai xem xét tất cả các điểm có tọa độ nằm trong một khoảng nhất định`[l, r]`. Trong số những điểm đó, chúng ta phải tính tổng khoảng cách trên mỗi cặp không có thứ tự. 

Nếu tọa độ được chọn là$$y_1 < y_2 < \dots < y_k,$$truy vấn yêu cầu$$\sum_{1 \le i < j \le k}(y_j-y_i).$$Những hạn chế là thách thức thực sự. Cả số điểm và số lượng truy vấn đều có thể đạt tới$10^5$. Một giải pháp quét tất cả các điểm cho mỗi truy vấn sẽ yêu cầu khoảng$10^{10}$trong trường hợp xấu nhất là hoàn toàn không thể thực hiện được. 

Bản thân tọa độ rất lớn, lên tới$10^9$về giá trị tuyệt đối và các bản cập nhật liên tục thay đổi chúng. Chúng ta không thể xây dựng một mảng trực tiếp được lập chỉ mục theo tọa độ. 

Một trường hợp tinh tế xuất hiện khi khoảng chứa 0 hoặc một điểm. Không có cặp nào nên câu trả lời phải bằng 0. 

Ví dụ:```
Points: 5

Query: [0, 10]
```Chỉ có một điểm được chọn nên đáp án là:```
0
```Một sai lầm dễ mắc phải khác là quên rằng các bản cập nhật sẽ di chuyển một điểm cụ thể chứ không phải giá trị tọa độ.```
Points: 1 10
Update: move point 1 by +20
```Tọa độ kết quả là:```
21 10
```không```
1 30
```Nguồn lỗi thứ ba là việc xử lý các ranh giới truy vấn. Nếu một điểm nằm chính xác tại`l`hoặc chính xác tại`r`, nó phải được bao gồm.```
Points: 0 5 10
Query: [0, 10]
```Cả ba điểm đều tham gia. 

Câu trả lời là:```
(5-0) + (10-0) + (10-5) = 20
```Sử dụng các bất đẳng thức nghiêm ngặt sẽ loại bỏ các điểm biên một cách không chính xác. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là xử lý từng truy vấn một cách độc lập. Đối với truy vấn loại 2, thu thập tất cả các điểm có tọa độ nằm trong`[l, r]`, sắp xếp chúng và tính tổng khoảng cách theo cặp. 

Điều này đúng vì định nghĩa chính xác là tổng của tất cả các cặp đã chọn. 

Thật không may, nó là quá chậm. Một truy vấn có thể chứa$10^5$điểm. Tính toán câu trả lời theo cách này tốn kém$O(n \log n)$cho mỗi truy vấn, dẫn đến khoảng$10^{10}$hoạt động tổng thể. 

Quan sát quan trọng là câu trả lời cho một tập hợp tọa độ có thứ tự có thể được hợp nhất từ ​​các phần nhỏ hơn. 

Giả sử chúng ta chia phạm vi tọa độ thành nửa bên trái và nửa bên phải. Mọi tọa độ ở nửa bên trái đều nhỏ hơn mọi tọa độ ở nửa bên phải. 

Đối với hai nhóm:$$L=\{x\},\qquad R=\{y\},$$đóng góp chéo là$$\sum (y-x).$$Khai triển biểu thức này mang lại$$|L|\cdot \sum R - |R|\cdot \sum L.$$Điều này chỉ phụ thuộc vào số điểm và tổng tọa độ ở mỗi nửa. 

Điều đó gợi ý một cây phân đoạn. Đối với mỗi nút chúng tôi lưu trữ:$$cnt = \text{number of active points}$$

$$sum = \text{sum of their coordinates}$$

$$ans = \text{sum of pairwise distances inside this segment}$$Khi sáp nhập hai đứa trẻ:$$cnt = cnt_L + cnt_R$$

$$sum = sum_L + sum_R$$

$$ans = ans_L + ans_R + cnt_L \cdot sum_R - cnt_R \cdot sum_L$$Số hạng cuối cùng chính xác là sự đóng góp của các cặp có điểm cuối nằm ở những đứa trẻ khác nhau. 

Cập nhật trở thành xóa và chèn điểm. Truy vấn trở thành truy vấn phạm vi cây phân đoạn thông thường. 

Các tọa độ rất lớn và động nên chúng tôi nén tất cả các tọa độ có thể xuất hiện. Vì mọi giá trị cập nhật đều được biết trước nên chúng tôi có thể mô phỏng chuỗi ngoại tuyến và thu thập mọi tọa độ sẽ tồn tại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(mn \log n)$|$O(n)$| Quá chậm | 
| Cây phân đoạn tối ưu + Nén tọa độ |$O((n+m)\log(n+m))$|$O(n+m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tọa độ ban đầu. 
2. Quét tất cả các truy vấn một lần trước khi xử lý chúng. 
3. Mô phỏng sự thay đổi tọa độ của mọi điểm và thu thập mọi tọa độ từng xuất hiện. 
4. Sắp xếp và loại bỏ các tọa độ đó. Điều này trở thành mảng nén tọa độ. 
5. Xây dựng cây phân đoạn trên tọa độ đã nén. 
6. Chèn mọi điểm ban đầu vào cây. Một cửa hàng lá:$$cnt=1,\quad sum=\text{coordinate},\quad ans=0.$$7. Đối với bản cập nhật loại 1: 

1. Xóa tọa độ cũ của điểm. 
2. Tính tọa độ mới. 
3. Chèn tọa độ mới. 
4. Cập nhật vị trí đã lưu của điểm đó. 
8. Đối với truy vấn loại 2: 

1. Tìm tọa độ nén thứ nhất không nhỏ hơn`l`. 
2. Tìm tọa độ nén cuối cùng không lớn hơn`r`. 
3. Truy vấn phạm vi cây phân đoạn đó. 
4. Xuất kết quả trả về`ans`. 
9. Trong mỗi lần hợp nhất, hãy tính:$$cross = cnt_L \cdot sum_R - cnt_R \cdot sum_L$$và thiết lập$$ans = ans_L + ans_R + cross.$$Lý do điều này có hiệu quả là vì mọi tọa độ ở con bên trái đều nhỏ hơn mọi tọa độ ở con bên phải. 

### Tại sao nó hoạt động 

Mỗi nút cây phân đoạn lưu trữ thông tin chính xác cần thiết để xây dựng lại tổng khoảng cách theo cặp trong khoảng của nó. 

Giá trị được lưu trữ`ans`đã chứa tất cả các cặp hoàn toàn bên trong con bên trái và tất cả các cặp hoàn toàn bên trong con bên phải. Các cặp còn thiếu duy nhất là những cặp có một điểm cuối ở mỗi đứa trẻ. 

Vì tọa độ được sắp xếp theo cây phân đoạn nên mỗi cặp chéo đều đóng góp`right - left`. Tổng hợp tất cả các cặp như vậy mang lại$$cnt_L \cdot sum_R - cnt_R \cdot sum_L.$$Do đó, mỗi cặp được tính chính xác một lần trong quá trình hợp nhất. Bằng cách quy nạp trên các nút cây phân đoạn, mọi truy vấn đều trả về tổng khoảng cách chính xác giữa tất cả các điểm đã chọn. 

## Giải pháp Python```python
import sys
from bisect import bisect_left, bisect_right

input = sys.stdin.readline

class SegTree:
    def __init__(self, coords):
        self.coords = coords
        self.n = len(coords)
        size = 4 * self.n + 5

        self.cnt = [0] * size
        self.sm = [0] * size
        self.ans = [0] * size

    def pull(self, p):
        lc = p * 2
        rc = lc + 1

        cnt_l = self.cnt[lc]
        cnt_r = self.cnt[rc]

        sum_l = self.sm[lc]
        sum_r = self.sm[rc]

        self.cnt[p] = cnt_l + cnt_r
        self.sm[p] = sum_l + sum_r

        cross = cnt_l * sum_r - cnt_r * sum_l
        self.ans[p] = self.ans[lc] + self.ans[rc] + cross

    def update(self, p, l, r, idx, delta):
        if l == r:
            self.cnt[p] += delta
            self.sm[p] += self.coords[idx] * delta
            self.ans[p] = 0
            return

        mid = (l + r) >> 1
        if idx <= mid:
            self.update(p * 2, l, mid, idx, delta)
        else:
            self.update(p * 2 + 1, mid + 1, r, idx, delta)

        self.pull(p)

    def query(self, p, l, r, ql, qr):
        if ql <= l and r <= qr:
            return (self.cnt[p], self.sm[p], self.ans[p])

        mid = (l + r) >> 1

        if qr <= mid:
            return self.query(p * 2, l, mid, ql, qr)

        if ql > mid:
            return self.query(p * 2 + 1, mid + 1, r, ql, qr)

        left = self.query(p * 2, l, mid, ql, qr)
        right = self.query(p * 2 + 1, mid + 1, r, ql, qr)

        cnt_l, sum_l, ans_l = left
        cnt_r, sum_r, ans_r = right

        cross = cnt_l * sum_r - cnt_r * sum_l

        return (
            cnt_l + cnt_r,
            sum_l + sum_r,
            ans_l + ans_r + cross
        )

def solve():
    n = int(input())
    x = list(map(int, input().split()))

    m = int(input())

    queries = []
    coords = x[:]

    cur = x[:]

    for _ in range(m):
        q = list(map(int, input().split()))
        queries.append(q)

        if q[0] == 1:
            p, d = q[1], q[2]
            cur[p - 1] += d
            coords.append(cur[p - 1])

    coords = sorted(set(coords))
    pos = {v: i for i, v in enumerate(coords)}

    seg = SegTree(coords)

    current = x[:]

    for v in current:
        seg.update(1, 0, seg.n - 1, pos[v], 1)

    out = []

    for q in queries:
        if q[0] == 1:
            p, d = q[1], q[2]
            p -= 1

            old = current[p]
            new = old + d

            seg.update(1, 0, seg.n - 1, pos[old], -1)
            seg.update(1, 0, seg.n - 1, pos[new], 1)

            current[p] = new

        else:
            lq, rq = q[1], q[2]

            L = bisect_left(coords, lq)
            R = bisect_right(coords, rq) - 1

            if L > R:
                out.append("0")
                continue

            _, _, ans = seg.query(1, 0, seg.n - 1, L, R)
            out.append(str(ans))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Lần đầu tiên vượt qua các truy vấn sẽ thực hiện nén tọa độ. Mọi tọa độ có thể xuất hiện đều được thu thập trước khi xây dựng cây phân đoạn. 

Mỗi lá tương ứng với một giá trị tọa độ nén. Vì tọa độ luôn khác biệt nên một lá chứa 0 hoặc một điểm hoạt động, nhưng việc triển khai vẫn hoạt động một cách tự nhiên ngay cả khi cho phép số lượng lớn hơn. 

Công thức hợp nhất là trung tâm của giải pháp. Cây phân đoạn không bao giờ liệt kê rõ ràng các cặp. Thay vào đó, nó giữ đủ thông tin tổng hợp để xây dựng lại tổng đóng góp của tất cả các cặp chéo trong thời gian không đổi. 

Truy vấn phạm vi trả về bộ ba`(count, sum, answer)`. Khi hai kết quả một phần được hợp nhất trong quá trình xử lý truy vấn, công thức hợp nhất giống hệt nhau sẽ được sử dụng như trong chính cây đó. 

Tất cả số học phải sử dụng số nguyên 64 bit. Trong Python việc này là tự động. 

## Ví dụ đã hoạt động 

### Ví dụ 1```
Points: 0 5 10
Query: [0,10]
```| Phân đoạn | Đếm | Tổng hợp | Tổng khoảng cách cặp | 
| --- | --- | --- | --- | 
| {0} | 1 | 0 | 0 | 
| {5} | 1 | 5 | 0 | 
| Hợp nhất | 2 | 5 | 5 | 
| {10} | 1 | 10 | 0 | 
| Hợp nhất cuối cùng | 3 | 15 | 20 | 

Câu trả lời cuối cùng là:$$(5-0)+(10-0)+(10-5)=20.$$Bảng này cho thấy cách đóng góp chéo tạo lại khoảng cách mà không cần liệt kê các cặp. 

### Ví dụ 2```
Points: 1 4 8
Update: point 2 += 3
Query: [0,10]
```Sau khi cập nhật tọa độ trở thành:```
1 7 8
```| Tọa độ hoạt động | Đếm | Tổng hợp | Trả lời | 
| --- | --- | --- | --- | 
| {1} | 1 | 1 | 0 | 
| {7} | 1 | 7 | 0 | 
| Hợp nhất | 2 | 8 | 6 | 
| {8} | 1 | 8 | 0 | 
| Hợp nhất cuối cùng | 3 | 16 | 14 | 

Câu trả lời là:$$(7-1)+(8-1)+(8-7)=14.$$Dấu vết này chứng tỏ rằng các bản cập nhật chỉ yêu cầu xóa một tọa độ và chèn một tọa độ khác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((n+m)\log(n+m))$| Mỗi cập nhật và truy vấn thực hiện một số lượng không đổi các thao tác trên cây phân đoạn | 
| Không gian |$O(n+m)$| Phối hợp nén và lưu trữ cây phân đoạn | 

Số tọa độ nén tối đa là tọa độ ban đầu cộng với một tọa độ mới cho mỗi lần cập nhật, tức là$O(n+m)$. Với$10^5$điểm và$10^5$truy vấn, hệ số logarit đủ nhỏ để vừa vặn trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    from bisect import bisect_left, bisect_right

    input_data = io.StringIO(inp)
    input = input_data.readline

    # paste solve() body here and return output
    return ""

# minimum size
# one point, one query
# answer must be 0

# assert run("1\n5\n1\n2 0 10\n") == "0"

# interval contains no points
# assert run("2\n0 10\n1\n2 20 30\n") == "0"

# boundary inclusion
# assert run("3\n0 5 10\n1\n2 0 10\n") == "20"

# update then query
# assert run(
# "3\n1 4 8\n2\n1 2 3\n2 0 10\n"
# ) == "14"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Điểm duy nhất | 0 | Không có cặp nào tồn tại | 
| Khoảng trống | 0 | Xử lý phạm vi chính xác | 
| Điểm chính xác trên ranh giới | 20 | Điểm cuối bao gồm | 
| Cập nhật rồi truy vấn | 14 | Logic loại bỏ và chèn | 

## Vỏ cạnh 

Hãy xem xét một khoảng truy vấn không chứa điểm:```
Points: 0 10
Query: [20,30]
```Tìm kiếm nhị phân tạo ra một phạm vi nén trống. Thuật toán ngay lập tức trở lại`0`. Không có truy vấn cây phân đoạn nào được thực hiện. 

Xét một khoảng chứa đúng một điểm:```
Points: 5
Query: [0,10]
```Phân đoạn được truy vấn trả về`cnt = 1`Và`ans = 0`. Vì không có cặp nào nên kết quả là đúng. 

Xét tọa độ biên:```
Points: 0 5 10
Query: [0,10]
```

`bisect_left`Và`bisect_right`bao gồm cả hai điểm cuối. Cả ba điểm đều được chọn và câu trả lời trở thành`20`. 

Xét một điểm di chuyển qua nhiều tọa độ khác:```
Points: 1 100
Update: point 1 += 200
```Tọa độ cũ được xóa khỏi lá của nó và tọa độ mới được chèn vào lá khác. Cây phân đoạn vẫn được sắp xếp theo giá trị tọa độ, do đó các phép tính đóng góp chéo trong tương lai vẫn tiếp tục hợp lệ. Đối số chính xác không bao giờ phụ thuộc vào khoảng cách di chuyển của một điểm.
