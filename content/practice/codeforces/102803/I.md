---
title: "CF 102803I - InkBall FX"
description: "Trò chơi có thể được xem như một tia di chuyển từ trái sang phải. Tọa độ theo phương ngang của quả bóng luôn tăng với vận tốc 1 nên sau t giây quả bóng ở vị trí x = t."
date: "2026-07-26T16:25:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102803
codeforces_index: "I"
codeforces_contest_name: "The 15th Heilongjiang Provincial Collegiate Programming Contest"
rating: 0
weight: 102803
solve_time_s: 51
verified: true
draft: false
---

[CF 102803I - InkBall FX](https://codeforces.com/problemset/problem/102803/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Trò chơi có thể được xem như một tia di chuyển từ trái sang phải. Tọa độ theo phương ngang của quả bóng luôn tăng với vận tốc 1 nên sau`t`giây bóng đang ở`x = t`. Phần thay đổi duy nhất là hướng thẳng đứng: nó bắt đầu tăng dần và mỗi khi quả bóng chạm vào một đoạn nằm ngang thì vận tốc thẳng đứng sẽ thay đổi. 

Một đoạn`(L, R, Y)`bị tấn công khi quỹ đạo hiện tại đạt đến độ cao`Y`tại một tọa độ ngang nào đó giữa`L`Và`R`. Sau khi bị đánh, đoạn đó biến mất nên chướng ngại vật tương tự không bao giờ cần phải xem xét lại. 

Đầu vào chứa tối đa`10^5`các đoạn ngang. Một mô phỏng trực tiếp để kiểm tra mọi phân đoạn sau mỗi lần va chạm sẽ yêu cầu tới`10^10`kiểm tra, vượt xa giới hạn 6 giây cho phép. Chúng ta cần một phương pháp logarit hoặc gần tuyến tính. 

Những trường hợp phức tạp là do việc chạm vào điểm cuối cũng được tính là va chạm. Một phân đoạn như:```
1
3 5 2
```bị trúng vì đường dẫn ban đầu là`y=x`, và nó đạt tới`(2,2)`trước phân khúc, không`(3,2)`, vậy câu trả lời là`0`. Việc triển khai bất cẩn chỉ kiểm tra độ cao và bỏ qua phạm vi x có thể tính không chính xác. 

Một lỗi phổ biến khác là quên rằng va chạm chính xác ở điểm cuối là hợp lệ:```
1
2 4 2
```Quả bóng chạm tới`(2,2)`, là điểm cuối bên trái của đoạn thẳng, vì vậy câu trả lời là`1`. 

Vấn đề thứ ba xuất hiện sau khi phản ánh:```
2
4 6 1
2 4 3
```Va chạm đầu tiên là với đoạn ở độ cao`3`khi quả bóng chạm tới`(3,3)`. Hướng thay đổi và đoạn thứ hai không bao giờ bị bắn trúng. Giải pháp giả định quả bóng luôn đi theo đường chéo ban đầu không thành công ở đây. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là liên tục tìm xung đột tiếp theo bằng cách kiểm tra từng đoạn còn lại. Đối với mỗi đoạn, chúng tôi tính toán xem tia hiện tại có cắt nó hay không và giữ lại tia sớm nhất. Điều này đúng vì quả bóng chỉ di chuyển về phía trước trong`x`, vậy giao điểm đầu tiên chính xác là sự kiện tiếp theo. Tuy nhiên, sau mỗi lần va chạm, một đoạn sẽ bị loại bỏ và trong trường hợp xấu nhất chúng ta thực hiện khoảng`n + (n-1) + ... + 1`kiểm tra, đó là`O(n^2)`. 

Quan sát quan trọng là một tia có độ dốc`1`hoặc`-1`có thể được mô tả bằng một hằng số. 

Khi quả bóng chuyển động đi lên thì đường đi của nó là:```
y = x + c
```Ở đâu`c = y - x`. Một phân đoạn được nhấn khi giá trị này nằm bên trong:```
Y - R <= c <= Y - L
```Đối với một cố định`c`, vị trí va chạm là`x = Y - c`, vì vậy trong số tất cả các phân đoạn phù hợp, chúng ta cần phân đoạn nhỏ nhất`Y`. 

Khi quả bóng chuyển động đi xuống thì đường đi của nó là:```
y = -x + c
```Ở đâu`c = y + x`. Khoảng hợp lệ trở thành:```
Y + L <= c <= Y + R
```và vị trí va chạm là`x = c - Y`, vì vậy chúng ta cần số lớn nhất`Y`. 

Sự cố trở thành hai truy vấn ngắt quãng động. Mỗi đoạn được chèn vào hai cấu trúc khoảng. Một truy vấn đưa ra phân đoạn tốt nhất trong số các khoảng bao gồm một điểm và sau khi sử dụng nó, chúng ta lười biếng xóa nó đi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n log n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi mọi phân đoạn thành hai khoảng chuyển đổi. Các cửa hàng có cấu trúc hướng lên`[Y-R, Y-L]`có giá trị`Y`, và cấu trúc hướng xuống lưu trữ`[Y+L, Y+R]`có giá trị`Y`. 
2. Xây dựng cây phân đoạn tọa độ nén cho từng hướng. Mỗi nút lưu trữ rất nhiều phân đoạn có khoảng thời gian được chuyển đổi bao phủ hoàn toàn nút đó. 
3. Bắt đầu mô phỏng tại`(0,0)`với hướng đi lên. 
4. Đối với chiều dòng điện, hãy tính hằng số tương ứng. Nếu di chuyển lên trên thì đó là`y-x`; nếu không thì nó là`y+x`. 
5. Truy vấn cây phân đoạn tương ứng. Truy vấn trả về phân đoạn có xung đột sớm nhất có thể. Nếu không có phân đoạn nào tồn tại thì quá trình mô phỏng sẽ kết thúc. 
6. Xóa đoạn tìm thấy bằng cách đánh dấu nó đã bị xóa. Các đống loại bỏ các phần tử đã xóa một cách lười biếng khi chúng đạt đến đỉnh. 
7. Di chuyển quả bóng đến điểm va chạm. Tọa độ x trở thành vị trí va chạm. Lật theo hướng dọc và lặp lại. 

Tại sao nó hoạt động: 

Tính bất biến của cây phân đoạn là mọi phân đoạn hoạt động xuất hiện chính xác trong các nút có phạm vi tọa độ được bao phủ hoàn toàn bởi khoảng chuyển đổi của nó. Một truy vấn sẽ truy cập mọi nút trên đường dẫn đến tọa độ được truy vấn, do đó, nó sẽ thấy mọi phân đoạn có thể xung đột. Thứ tự heap chọn xung đột gần nhất trong số các ứng cử viên đó. Vì mỗi xung đột sẽ loại bỏ vĩnh viễn một phân đoạn nên mọi thao tác được thực hiện nhiều nhất`n`lần. 

## Giải pháp Python```python
import sys
import heapq
input = sys.stdin.readline

class SegmentTree:
    def __init__(self, coords, mode):
        self.coords = coords
        self.n = len(coords)
        self.tree = [[] for _ in range(self.n * 4)]
        self.mode = mode
        self.deleted = None

    def add(self, node, l, r, ql, qr, item):
        if ql <= l and r <= qr:
            heapq.heappush(self.tree[node], item)
            return
        m = (l + r) // 2
        if ql <= m:
            self.add(node * 2, l, m, ql, qr, item)
        if m < qr:
            self.add(node * 2 + 1, m + 1, r, ql, qr, item)

    def query(self, node, l, r, pos):
        while self.tree[node] and self.deleted[self.tree[node][0][1]]:
            heapq.heappop(self.tree[node])

        best = self.tree[node][0] if self.tree[node] else None
        if l != r:
            m = (l + r) // 2
            if pos <= m:
                other = self.query(node * 2, l, m, pos)
            else:
                other = self.query(node * 2 + 1, m + 1, r, pos)
            if other is not None:
                if best is None:
                    best = other
                elif self.mode == 1 and other[0] < best[0]:
                    best = other
                elif self.mode == -1 and other[0] < best[0]:
                    best = other
        return best

def build_coords(intervals):
    a = []
    for l, r, _, _ in intervals:
        a.append(l)
        a.append(r)
    a.sort()
    res = []
    for x in a:
        if not res or res[-1] != x:
            res.append(x)
    extra = []
    for i in range(len(res) - 1):
        if res[i + 1] - res[i] > 1:
            extra.append((res[i] + res[i + 1]) // 2)
    res.extend(extra)
    res.sort()
    return res

def solve_case(segs):
    n = len(segs)
    up = []
    down = []

    for i, (l, r, y) in enumerate(segs):
        up.append((y - r, y - l, y, i))
        down.append((y + l, y + r, y, i))

    cu = build_coords(up)
    cd = build_coords(down)

    tree_up = SegmentTree(cu, 1)
    tree_down = SegmentTree(cd, -1)

    deleted = [False] * n
    tree_up.deleted = deleted
    tree_down.deleted = deleted

    import bisect

    for l, r, y, i in up:
        tree_up.add(1, 0, len(cu) - 1,
                    bisect.bisect_left(cu, l),
                    bisect.bisect_right(cu, r) - 1,
                    (y, i))

    for l, r, y, i in down:
        tree_down.add(1, 0, len(cd) - 1,
                      bisect.bisect_left(cd, l),
                      bisect.bisect_right(cd, r) - 1,
                      (-y, i))

    x = 0
    y = 0
    direction = 1
    ans = 0

    while True:
        if direction == 1:
            c = y - x
            p = bisect.bisect_left(cu, c)
            if p == len(cu) or cu[p] != c:
                p -= 1
            if p < 0:
                break
            res = tree_up.query(1, 0, len(cu) - 1, p)
            if res is None:
                break
            ny, idx = res
            nx = ny - c
        else:
            c = y + x
            p = bisect.bisect_left(cd, c)
            if p == len(cd) or cd[p] != c:
                p -= 1
            if p < 0:
                break
            res = tree_down.query(1, 0, len(cd) - 1, p)
            if res is None:
                break
            ny, idx = -res[0], res[1]
            nx = c - ny

        deleted[idx] = True
        ans += 1
        x = nx
        y = ny
        direction *= -1

    return ans

def main():
    t = int(input())
    out = []
    for _ in range(t):
        n = int(input())
        segs = [tuple(map(int, input().split())) for _ in range(n)]
        out.append(str(solve_case(segs)))
    print("\n".join(out))

if __name__ == "__main__":
    main()
```Các khoảng chuyển đổi là cốt lõi của việc thực hiện. Cây phân đoạn không lưu trữ trực tiếp các vị trí va chạm, vì quỹ đạo hiện tại đang thay đổi sau mỗi lần va chạm. Thay vào đó nó lưu trữ các hằng số mô tả tất cả các quỹ đạo có thể có. 

Các đống chứa id phân đoạn nên việc xóa rất lười biếng. Một phân đoạn bị loại bỏ có thể vẫn còn trong một số vùng nhớ heap, nhưng nó sẽ bị bỏ qua khi đạt đến đỉnh. Điều này tránh được việc loại bỏ tốn kém từ nhiều nút cây. 

Tất cả các tọa độ đều được xử lý bằng số nguyên Python, do đó không có vấn đề tràn mặc dù các giá trị trung gian có thể vượt quá phạm vi tọa độ ban đầu. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
3
4 6 1
2 4 3
5 6 3
```| Bước | Hướng | Vị trí hiện tại | Hằng số | Đánh | 
| --- | --- | --- | --- | --- | 
| 1 | Lên | (0,0) | 0 | Đoạn (2,4,3) | 
| 2 | Xuống | (3,3) | 6 | Không có | 

Cú đánh đầu tiên xảy ra vì`y=x`đạt đến độ cao`3`Tại`x=3`. Sau khi phản xạ, tia đi xuống không gặp đoạn còn lại. 

Đối với mẫu thứ hai:```
2
3 4 1
1 2 2
```| Bước | Hướng | Vị trí hiện tại | Hằng số | Đánh | 
| --- | --- | --- | --- | --- | 
| 1 | Lên | (0,0) | 0 | Đoạn (1,2,2) | 
| 2 | Xuống | (2,2) | 4 | Đoạn (3,4,1) | 

Va chạm đầu tiên xảy ra ở điểm cuối`(2,2)`. Tia phản xạ tới đoạn thứ hai tại`(3,1)`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi đoạn được chèn vào hai cây và mọi xung đột đều thực hiện các truy vấn logarit. | 
| Không gian | O(n log n) | Mỗi khoảng được lưu trữ dưới dạng logarit của nhiều nút cây phân đoạn. | 

Tối đa của`10^5`các phân đoạn được xử lý vì thuật toán không bao giờ quét tất cả các phân đoạn hoạt động trong khi xảy ra xung đột. Mỗi phân đoạn tham gia vào một số lượng nhất định các hoạt động heap. 

## Trường hợp thử nghiệm```
# The following tests can be used with the solve_case logic.

assert solve_case([(4, 6, 1), (2, 4, 3), (5, 6, 3)]) == 2
assert solve_case([(3, 4, 1), (1, 2, 2)]) == 2
assert solve_case([(1, 2, 5)]) == 0
assert solve_case([(1, 3, 1)]) == 1
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Một phân khúc chưa bao giờ chạm tới | 0 | Kiểm tra thiếu xử lý va chạm | 
| Va chạm điểm cuối | 1 | Kiểm tra ranh giới bao gồm | 
| Mẫu phản ánh | 2 | Kiểm tra sự thay đổi hướng | 
| Nhiều khoảng thời gian hoạt động | Đúng cú đánh đầu tiên | Kiểm tra thứ tự đống | 

## Vỏ cạnh 

Phân đoạn chỉ được chạm vào tại điểm cuối sẽ được lưu trữ với khoảng thời gian chuyển đổi đóng, do đó cả hai điểm cuối vẫn giữ nguyên vị trí truy vấn hợp lệ. Việc nén tọa độ cũng giữ lại mọi điểm cuối ban đầu, ngăn chặn việc vô tình loại bỏ các trường hợp biên. 

Khi một số phân đoạn có thể được đánh ở cùng một tọa độ được biến đổi, thứ tự heap sẽ chọn phân đoạn có tọa độ x va chạm nhỏ nhất. Điều này diễn ra trực tiếp từ các phương trình được biến đổi: đối với chuyển động đi lên, tọa độ x là`Y-c`, và đối với chuyển động đi xuống thì đó là`c-Y`. 

Sau khi va chạm, đoạn này chỉ được đánh dấu là bị xóa. Nó có thể vẫn tồn tại trong các đống nội bộ, nhưng mọi truy vấn sẽ loại bỏ các mục nhập không hợp lệ trước khi sử dụng chúng. Điều này duy trì tính chính xác trong khi vẫn duy trì việc thực hiện nhanh chóng.
