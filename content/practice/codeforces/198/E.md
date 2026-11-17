---
title: "CF 198E - Câu Chuyện Hấp Dẫn"
description: "Mỗi dụng cụ kẹp được đặt tại một điểm cố định trong không gian. Tàu của Qwerty cũng đã được sửa. Một dụng cụ kẹp có thể kéo một dụng cụ kẹp khác vào tàu nếu có hai điều kiện đồng thời."
date: "2026-06-03T09:52:49+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "sortings"]
categories: ["algorithms"]
codeforces_contest: 198
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 125 (Div. 1)"
rating: 2400
weight: 198
solve_time_s: 86
verified: true
draft: false
---

[CF 198E - Câu chuyện hấp dẫn](https://codeforces.com/problemset/problem/198/E) 

**Đánh giá:** 2400 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, sắp xếp 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi dụng cụ kẹp được đặt tại một điểm cố định trong không gian. Tàu của Qwerty cũng đã được sửa. 

Một dụng cụ kẹp có thể kéo một dụng cụ kẹp khác vào tàu nếu có hai điều kiện đồng thời. Khối lượng của thiết bị kẹp mục tiêu không được vượt quá công suất của thiết bị kẹp mục tiêu và thiết bị kẹp mục tiêu phải nằm trong bán kính hoạt động của thiết bị kẹp mục tiêu tính từ tàu. 

Điều kiện thứ hai là quan sát quan trọng. Con tàu không bao giờ di chuyển và mọi thiết bị kẹp nằm rải rác vẫn giữ nguyên tọa độ ban đầu cho đến khi được thu thập. Khi một dụng cụ kẹp được thu thập hoạt động, nó vẫn hoạt động trên tàu. Điều đó có nghĩa là khoảng cách duy nhất quan trọng là khoảng cách từ con tàu đến mỗi tay kẹp. 

Đối với mỗi dụng cụ kẹp, chúng tôi có thể tính toán trước$$d_i = (x_i-x)^2 + (y_i-y)^2.$$Dụng cụ kẹp có bán kính$r$có thể tiếp cận chính xác các dụng cụ kẹp thỏa mãn$$d_i \le r^2.$$Sau khi thu thập một cái kẹp, chúng tôi có được một cặp mới$(p_i,r_i)$, có thể cho phép thu thập thêm dụng cụ kẹp. Chúng tôi muốn kích thước của toàn bộ tập hợp có thể truy cập. 

Ràng buộc$n \le 250000$loại trừ bất cứ điều gì bậc hai. Thậm chí$O(n\sqrt n)$sẽ không thoải mái ở quy mô này. Chúng ta cần thứ gì đó xung quanh$O(n \log n)$. 

Một điểm tinh tế là việc thu thập một chiếc kẹp có thể mở khóa một chiếc kẹp khác mà trước đây quá nặng và chiếc kẹp mới được thu thập đó có thể lần lượt mở khóa nhiều chiếc kẹp khác. Đây là vấn đề về khả năng tiếp cận chứ không phải vấn đề lọc một lần. 

Coi như:```
Ship: power=1 radius=10

A: mass=1 power=100 radius=10
B: mass=100 power=1 radius=10
```A có thể truy cập ban đầu. Sau khi thu thập A, B sẽ có thể truy cập được. Câu trả lời đúng là 2. Bất kỳ giải pháp nào chỉ kiểm tra khả năng tiếp cận từ dụng cụ kẹp ban đầu sẽ cho ra 1. 

Một sai lầm dễ mắc phải khác là nghĩ rằng hình học giữa các tay kẹp rất quan trọng.```
Ship at (0,0)

A at distance 5
B at distance 100
```Nếu A có bán kính 100 và đủ lực thì A có thể kéo B dù A và B ở xa nhau. Dụng cụ kẹp chủ động được lắp trên tàu nên tất cả bán kính được đo từ tàu chứ không phải từ vị trí ban đầu của dụng cụ kẹp. 

Một cạm bẫy cuối cùng là tràn số nguyên. Tọa độ và bán kính đạt$10^9$, do đó bình phương khoảng cách đạt tới$10^{18}$. Tất cả các tính toán khoảng cách phải sử dụng số nguyên 64 bit. 

## Phương pháp tiếp cận 

Quan điểm vũ phu rất đơn giản. Duy trì bộ dụng cụ kẹp đã thu thập. Bất cứ khi nào có dụng cụ kẹp mới, hãy quét tất cả các dụng cụ kẹp chưa được thu thập và thu thập từng cái đáp ứng cả yêu cầu về khối lượng và khoảng cách của dụng cụ kẹp hiện đang hoạt động. Lặp lại cho đến khi không còn thu được thêm dụng cụ kẹp nào nữa. 

Điều này đúng vì nó mô phỏng rõ ràng quá trình tiếp cận. Vấn đề là chi phí. Trong trường hợp xấu nhất chúng tôi có thể thu thập$n$dụng cụ kẹp và sau mỗi bộ sưu tập, chúng tôi quét tất cả$n$đồ vật nữa. Điều đó trở thành$O(n^2)$, đại khái$6 \cdot 10^{10}$hoạt động cho$n=250000$. 

Cấu trúc của truy vấn cụ thể hơn nhiều so với khả năng truy cập tùy ý. 

Đối với dụng cụ kẹp có nguồn điện$P$và bán kính$R$, chúng ta cần tất cả các dụng cụ kẹp không được thu thập thỏa mãn$$m_i \le P$$Và$$d_i \le R^2.$$Đây là truy vấn thống trị hai chiều đối với các thuộc tính$(m_i,d_i)$. 

Quan sát quan trọng là mỗi dụng cụ kẹp được thu thập nhiều nhất một lần. Chúng ta không cần phải liệt kê nhiều lần cùng một tập ứng cử viên. Chúng ta chỉ cần một cấu trúc dữ liệu có thể liên tục tìm thấy một số dụng cụ kẹp vẫn chưa được thu thập bên trong hình chữ nhật$$m_i \le P,\quad d_i \le R^2.$$Nếu có, chúng tôi sẽ thu thập, xóa vĩnh viễn và tiếp tục. 

Sắp xếp tất cả các dụng cụ kẹp theo khối lượng. Sau đó, một truy vấn trở thành: 

"Trong số tất cả các dụng cụ kẹp có chỉ số khoảng cách nằm trong tiền tố và vẫn còn hoạt động, hãy tìm khối lượng tối thiểu." 

Cây Fenwick có thể duy trì thông tin tiền tố theo khoảng cách. Mỗi nút Fenwick lưu trữ các dụng cụ kẹp được sắp xếp theo khối lượng. Tính năng xóa lười sẽ loại bỏ các dụng cụ kẹp đã được thu thập. 

Bất cứ khi nào khối lượng tối thiểu bên trong tiền tố khoảng cách có thể tiếp cận tối đa bằng công suất hiện tại, chúng tôi sẽ tìm thấy một dụng cụ kẹp sưu tập khác. 

Điều này mang lại một$O(n \log n)$giải pháp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n \log n)$|$O(n \log n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Giảm khoảng cách 

Với mỗi dụng cụ kẹp, hãy tính bình phương khoảng cách của nó với tàu:$$d_i=(x_i-x)^2+(y_i-y)^2.$$Thay thế mọi bán kính$r_i$qua$r_i^2$. 

Bây giờ hình học biến mất hoàn toàn. Mỗi dụng cụ kẹp chỉ được đại diện bởi:$$(d_i,m_i,p_i,r_i^2).$$### Nén tọa độ 

Thu thập tất cả các giá trị có thể xuất hiện dưới dạng ngưỡng khoảng cách:$$d_i,\quad r_i^2,\quad r^2.$$Sắp xếp và nén chúng. 

Cây Fenwick sẽ được xây dựng trên các tọa độ khoảng cách nén này. 

### Chuẩn bị cây Fenwick 

Sắp xếp tất cả các dụng cụ kẹp theo khối lượng. 

Đối với mỗi tay kẹp, hãy đưa nó vào cấu trúc Fenwick ở vị trí tương ứng với khoảng cách của nó. 

Mỗi nút Fenwick lưu trữ các dụng cụ kẹp theo thứ tự khối lượng tăng dần. 

Một nút sau này có thể chứa các phần tử đã bị xóa ở phía trước của nó, vì vậy chúng ta loại bỏ chúng một cách lười biếng.

 ### Khả năng tiếp cận BFS 

Hãy coi mỗi dụng cụ kẹp được thu thập như đang tạo ra một trạng thái mới$(p,r^2)$. 

Bắt đầu với dụng cụ kẹp ban đầu. 

Duy trì một hàng các trạng thái hoạt động. 

1. Bật một trạng thái$(P,R^2)$. 
2. Tìm chỉ số nén tương ứng với$R^2$. 
3. Truy vấn tiền tố Fenwick theo chỉ mục đó. 
4. Truy vấn trả về dụng cụ kẹp còn sống có khối lượng tối thiểu trong phạm vi khoảng cách có thể tiếp cận. 
5. Trong khi một dụng cụ kẹp như vậy tồn tại và khối lượng của nó tối đa là$P$: 

1. Đánh dấu nó đã được thu thập. 
2. Loại bỏ nó một cách lười biếng khỏi cấu trúc. 
3. Đẩy của riêng mình$(p_i,r_i^2)$vào hàng đợi. 
4. Tăng câu trả lời. 
5. Truy vấn lại. 

Quá trình dừng lại khi hàng đợi trống. 

### Tại sao nó hoạt động 

Đối với mọi trạng thái hoạt động$(P,R^2)$, một cái kẹp có thể được thu thập chính xác khi nó nằm bên trong hình chữ nhật$$m_i \le P,\quad d_i \le R^2.$$Truy vấn Fenwick luôn trả về bộ kẹp còn sống có khối lượng tối thiểu trong số tất cả các bộ kẹp thỏa mãn điều kiện khoảng cách. 

Nếu khối lượng tối thiểu đó đã vượt quá$P$, thì mọi dụng cụ kẹp còn lại trong phạm vi khoảng cách có thể tiếp cận cũng vượt quá$P$, vì vậy không có dụng cụ kẹp sưu tập nào tồn tại ở trạng thái này. 

Nếu khối lượng tối thiểu lớn nhất$P$, dụng cụ kẹp đó có thể được sưu tầm và phải thuộc nhóm có thể truy cập được. Việc thu thập nó ngay lập tức là an toàn vì việc thu thập dụng cụ kẹp không bao giờ khiến bất kỳ dụng cụ kẹp nào có thể tiếp cận trước đó trở nên không thể tiếp cận được. 

Mỗi dụng cụ kẹp được lắp vào một lần và được tháo ra một lần, do đó, thuật toán sẽ phát hiện chính xác điểm đóng của tất cả các dụng cụ kẹp có thể tiếp cận được. 

## Giải pháp Python```python
import sys
from collections import deque
from bisect import bisect_left, bisect_right

input = sys.stdin.readline

def solve():
    x, y, p0, r0, n = map(int, input().split())

    items = []
    coords = [r0 * r0]

    for idx in range(n):
        xi, yi, m, p, r = map(int, input().split())

        dx = xi - x
        dy = yi - y

        d = dx * dx + dy * dy
        rr = r * r

        items.append([d, m, p, rr, idx])

        coords.append(d)
        coords.append(rr)

    coords = sorted(set(coords))
    K = len(coords)

    def comp(v):
        return bisect_left(coords, v) + 1

    items_sorted = sorted(items, key=lambda z: (z[1], z[0]))

    bit = [[] for _ in range(K + 2)]

    for pos, item in enumerate(items_sorted):
        d, m, p, rr, original_id = item
        idx = comp(d)

        j = idx
        while j <= K:
            bit[j].append(pos)
            j += j & -j

    ptr = [0] * (K + 2)
    removed = [False] * n

    INF = (10**30, -1)

    def clean(node):
        arr = bit[node]
        p = ptr[node]

        while p < len(arr):
            pos = arr[p]
            oid = items_sorted[pos][4]
            if not removed[oid]:
                break
            p += 1

        ptr[node] = p

    def query(idx):
        best_mass = INF[0]
        best_pos = -1

        while idx > 0:
            clean(idx)

            p = ptr[idx]
            arr = bit[idx]

            if p < len(arr):
                pos = arr[p]
                mass = items_sorted[pos][1]

                if mass < best_mass:
                    best_mass = mass
                    best_pos = pos

            idx -= idx & -idx

        return best_pos

    ans = 0
    q = deque()
    q.append((p0, r0 * r0))

    while q:
        power, radius_sq = q.popleft()

        limit = bisect_right(coords, radius_sq)

        pos = query(limit)

        while pos != -1 and items_sorted[pos][1] <= power:
            d, m, p, rr, oid = items_sorted[pos]

            removed[oid] = True
            ans += 1

            q.append((p, rr))

            pos = query(limit)

    print(ans)

if __name__ == "__main__":
    solve()
```Giai đoạn tiền xử lý chuyển đổi mọi điều kiện hình học thành so sánh với khoảng cách bình phương. Đó là lý do tọa độ biến mất hoàn toàn khỏi cấu trúc dữ liệu. 

Cây Fenwick được xây dựng trên các giá trị khoảng cách nén. Mỗi nút chứa các bộ kẹp được sắp xếp theo khối lượng vì danh sách chung đã được sắp xếp theo khối lượng khi được chèn vào. 

các`ptr`mảng thực hiện việc xóa lười biếng. Khi một dụng cụ kẹp được thu thập, chúng tôi chỉ đánh dấu nó là đã bị loại bỏ. Lần tiếp theo khi một nút được truy cập, con trỏ của nó sẽ tiến qua các mục đã bị xóa. Điều này tránh được việc loại bỏ tốn kém khỏi nhiều nút Fenwick. 

Thao tác truy vấn trả về bộ kẹp còn sống có khối lượng nhỏ nhất trong số tất cả các khoảng cách bên trong tiền tố được yêu cầu. Nếu khối lượng nhỏ nhất đó đã quá lớn thì mọi ứng cử viên khác cũng quá lớn. 

Tất cả các ô vuông khoảng cách và bán kính được tính toán bằng số nguyên Python, xử lý an toàn các giá trị lên đến$10^{18}$. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
0 0 5 10 5
5 4 7 11 5
-7 1 4 7 8
0 2 13 5 6
2 -3 9 3 4
13 5 1 9 9
```| Bước | Sức mạnh hoạt động | Bán kính hoạt động² | Mới Được Thu Thập | 
| --- | --- | --- | --- | 
| 1 | 5 | 100 | Kẹp 2 | 
| 2 | 7 | 64 | Kẹp 1 | 
| 3 | 11 | 25 | Kẹp 4 | 
| 4 | 3 | 16 | Không có | 

Đáp án = 3. 

Dấu vết cho thấy hiệu ứng dây chuyền. Bộ kẹp ban đầu không thể trực tiếp thu thập bộ kẹp 1, nhưng việc thu thập bộ kẹp 2 sẽ tăng sức mạnh khả dụng và mở khóa nó. 

### Ví dụ tùy chỉnh```
0 0 1 10 2
1 0 1 100 10
2 0 100 1 10
```| Bước | Sức mạnh hoạt động | Bán kính hoạt động² | Mới Được Thu Thập | 
| --- | --- | --- | --- | 
| 1 | 1 | 100 | Dụng cụ kẹp đầu tiên | 
| 2 | 100 | 100 | Dụng cụ kẹp thứ hai | 
| 3 | 1 | 100 | Không có | 

Trả lời = 2. 

Điều này chứng tỏ tại sao một lần vượt qua là không đủ. Dụng cụ kẹp thứ hai chỉ có thể tiếp cận được sau khi có được dụng cụ kẹp thứ nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Mỗi kẹp được lắp vào và tháo ra một lần, mỗi thao tác đều chạm vào nút Fenwick | 
| Không gian |$O(n \log n)$| Dụng cụ kẹp được lưu trữ bên trong danh sách nút Fenwick | 

Với$n=250000$,$O(n \log n)$thoải mái trong giới hạn 4 giây. Việc sử dụng bộ nhớ nằm trong khoảng 512 MB vì ​​cấu trúc Fenwick lưu trữ tiêu chuẩn$n \log n$phân phối tài liệu tham khảo. 

## Trường hợp thử nghiệm```python
import sys, io
from collections import deque
from bisect import bisect_left, bisect_right

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    x, y, p0, r0, n = map(int, input().split())

    items = []
    coords = [r0 * r0]

    for idx in range(n):
        xi, yi, m, p, r = map(int, input().split())

        d = (xi - x) ** 2 + (yi - y) ** 2
        rr = r * r

        items.append([d, m, p, rr, idx])

        coords.append(d)
        coords.append(rr)

    coords = sorted(set(coords))
    K = len(coords)

    def comp(v):
        return bisect_left(coords, v) + 1

    items_sorted = sorted(items, key=lambda z: (z[1], z[0]))

    bit = [[] for _ in range(K + 2)]

    for pos, item in enumerate(items_sorted):
        idx = comp(item[0])

        j = idx
        while j <= K:
            bit[j].append(pos)
            j += j & -j

    ptr = [0] * (K + 2)
    removed = [False] * n

    def clean(node):
        while ptr[node] < len(bit[node]):
            pos = bit[node][ptr[node]]
            if not removed[items_sorted[pos][4]]:
                break
            ptr[node] += 1

    def query(idx):
        best = None

        while idx > 0:
            clean(idx)

            if ptr[idx] < len(bit[idx]):
                pos = bit[idx][ptr[idx]]

                if best is None or items_sorted[pos][1] < items_sorted[best][1]:
                    best = pos

            idx -= idx & -idx

        return best

    ans = 0
    q = deque([(p0, r0 * r0)])

    while q:
        power, rr = q.popleft()

        limit = bisect_right(coords, rr)

        pos = query(limit)

        while pos is not None and items_sorted[pos][1] <= power:
            removed[items_sorted[pos][4]] = True
            ans += 1

            q.append((items_sorted[pos][2], items_sorted[pos][3]))

            pos = query(limit)

    return str(ans)

# sample
assert run(
"""0 0 5 10 5
5 4 7 11 5
-7 1 4 7 8
0 2 13 5 6
2 -3 9 3 4
13 5 1 9 9
"""
) == "3"

# minimum size
assert run(
"""0 0 1 1 1
1 0 1 1 1
"""
) == "1"

# unreachable because of mass
assert run(
"""0 0 1 10 1
1 0 2 100 100
"""
) == "0"

# chain unlocking
assert run(
"""0 0 1 10 2
1 0 1 100 10
2 0 100 1 10
"""
) == "2"

# boundary distance exactly equal to radius
assert run(
"""0 0 10 5 1
3 4 10 1 1
"""
) == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 | 3 | Kịch bản chính thức | 
| Dụng cụ kẹp đơn có thể tiếp cận | 1 | Kích thước tối thiểu | 
| Kẹp nặng | 0 | Hạn chế khối lượng | 
| Mở khóa dây chuyền | 2 | Tuyên truyền khả năng tiếp cận | 
| Khoảng cách chính xác bằng bán kính | 1 | Ranh giới bao gồm | 

## Vỏ cạnh 

Hãy xem xét:```
0 0 1 10 2
1 0 1 100 10
2 0 100 1 10
```Trước tiên, thuật toán sẽ tìm bộ kẹp có khối lượng tối thiểu có thể tiếp cận được, thu thập nó và đẩy cặp bán kính công suất của nó vào hàng đợi. Khi trạng thái mới được xử lý, dụng cụ kẹp thứ hai sẽ có thể được sưu tập. Câu trả lời đúng là 2. 

Hãy xem xét:```
0 0 10 5 1
3 4 10 1 1
```Bình phương khoảng cách là$3^2+4^2=25$, chính xác bằng$5^2$. Truy vấn sử dụng tiền tố khoảng cách với điều kiện$d_i \le r^2$, do đó có bộ kẹp và câu trả lời là 1. 

Hãy xem xét:```
0 0 1 1000000000 1
1000000000 0 1 1 1
```Bình phương khoảng cách bằng$10^{18}$. Thuật toán lưu trữ tất cả các giá trị như số nguyên tỷ lệ 64 bit và so sánh trực tiếp số lượng bình phương. Không sử dụng số học dấu phẩy động nên câu trả lời vẫn đúng.
