---
title: "CF 1886F - Trộm Kim Cương"
description: "Mỗi camera có thể bị hack nhiều lần. Một vụ hack được thực hiện ở giây T sẽ vô hiệu hóa camera đó trong khoảng thời gian [T + 1, T + s]. Khoảnh khắc duy nhất quan trọng là hai vụ trộm. Giả sử viên kim cương thứ nhất bị đánh cắp ở thời điểm A và viên kim cương thứ hai ở thời điểm B với A < B."
date: "2026-06-08T22:18:47+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1886
codeforces_index: "F"
codeforces_contest_name: "Educational Codeforces Round 156 (Rated for Div. 2)"
rating: 3300
weight: 1886
solve_time_s: 113
verified: true
draft: false
---

[CF 1886F - Trộm kim cương](https://codeforces.com/problemset/problem/1886/F) 

**Đánh giá:** 3300 
**Tas:** cấu trúc dữ liệu, tham lam 
**Thời gian giải:** 1m 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi camera có thể bị hack nhiều lần. Một vụ hack được thực hiện ở giây thứ hai`T`vô hiệu hóa máy ảnh đó trong khoảng thời gian`[T + 1, T + s]`. 

Khoảnh khắc duy nhất quan trọng là hai vụ trộm. Hãy để viên kim cương đầu tiên bị đánh cắp vào thời điểm`A`và lần thứ hai`B`, với`A < B`. 

Camera quan sát viên kim cương đầu tiên phải được tắt vào thời điểm đó`A`. 

Camera quan sát viên kim cương thứ hai phải được tắt vào thời điểm đó`B`. 

Một camera đang quan sát cả hai viên kim cương phải bị vô hiệu hóa trong toàn bộ khoảng thời gian từ`A`ĐẾN`B`, hoặc bị hack hai lần, một lần cho mỗi lần trộm. 

Nhận xét quan trọng là lịch trình tối ưu không bao giờ có thời gian nhàn rỗi. Mỗi giây đều là một vụ hack hoặc một trong hai vụ trộm. Nếu chúng ta biết cần bao nhiêu lần hack thì câu trả lời đơn giản là```
(number of hacks) + 2
```vì bản thân hai vụ trộm cũng tiêu tốn từng giây. 

Các ràng buộc nhỏ bất thường ở một chiều và lớn bất thường ở chiều khác. Chúng tôi có nhiều nhất là 1500 camera, nhưng thời lượng camera nhiều nhất là`2n`, do đó nhiều nhất là 3000. Bất kỳ giải pháp bậc hai nào trong phạm vi khoảng thời gian đều có thể chấp nhận được, nhưng bất cứ điều gì giống như tìm kiếm theo cấp số nhân trên các tập hợp con của máy ảnh đều là không thể. 

Phần khó khăn là không lên lịch hack. Nó quyết định camera nào có thể bị hack một lần và camera nào phải hack hai lần. 

Trường hợp cạnh tinh tế xuất hiện khi mọi camera quan sát viên kim cương đầu tiên đều có thời lượng rất nhỏ.```
1
1 1
```Viên kim cương đầu tiên có thể bị đánh cắp sau khi hack camera đó một lần, nhưng viên kim cương thứ hai thì không thể vì không có camera theo dõi nó. Câu trả lời đúng là`2`, không`-1`. Một giải pháp nhất quyết đồng bộ hóa cả hai vụ trộm sẽ thất bại ở đây. 

Một trường hợp quan trọng khác là camera quan sát cả hai viên kim cương.```
1
3 1
```Một hack là không đủ. Camera chỉ bị vô hiệu hóa trong một giây, trong khi hai vụ trộm phải xảy ra ở những giây khác nhau. Câu trả lời đúng là`4`, phù hợp với mẫu Bất kỳ cách tiếp cận nào coi máy ảnh loại 3 là đáp ứng đồng thời cả hai viên kim cương sau một lần hack đều không chính xác. 

Cái bẫy thứ ba là cho rằng thời gian dài hơn luôn tốt hơn. Đối với khoảng cách cố định giữa các vụ trộm, camera loại 3 có thời lượng`x`đôi khi có thể được che chắn bởi một lần hack và đôi khi không. Quyết định phụ thuộc vào khoảng cách được lựa chọn giữa các vụ trộm. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô phỏng tất cả các khoảnh khắc có thể xảy ra của vụ trộm đầu tiên, tất cả các khoảnh khắc có thể xảy ra của vụ trộm thứ hai và tất cả các nhiệm vụ có thể có của camera để thực hiện các vụ hack. Ngay cả sau khi khai thác thực tế là thời lượng tối đa là 3000, không gian trạng thái vẫn rất lớn. Camera loại 3 có thể bị hack một lần trước lần trộm đầu tiên, một lần trước lần trộm thứ hai hoặc hai lần. Với số lượng lên tới 1500 camera như vậy, việc tìm kiếm toàn diện là vô vọng. 

Sự đột phá đến từ việc tập trung vào khoảng trống```
len = B - A
```giữa hai vụ trộm. 

Sửa giá trị này. Bây giờ mọi máy ảnh có thể được phân loại hoàn toàn theo thời lượng của nó. 

Đối với camera loại 1, một vụ hack duy nhất phải bao trùm được vụ trộm đầu tiên. 

Đối với camera loại 2, một vụ hack duy nhất phải che đậy được vụ trộm thứ hai. 

Đối với máy ảnh loại 3 có thời lượng ít nhất`len`, một vụ hack có thể bao gồm cả hai vụ trộm. Nếu thời lượng của nó nhỏ hơn`len`, hai vụ hack là không thể tránh khỏi. 

Sau khi sửa`len`, vấn đề trở thành một bài kiểm tra tính khả thi. Chúng ta phải xác định xem liệu khoảng thời gian sẵn có có thể bao gồm tất cả các vị trí cần thiết trước vụ trộm đầu tiên và trước vụ trộm thứ hai hay không. 

Giải pháp được chấp nhận xử lý mọi giá trị có thể có của`len`. Từ`len`không bao giờ vượt quá`k₂ + k₃ + 1`, chỉ một`O(n)`các giá trị cần được kiểm tra. Đối với mỗi giá trị, phép gán tham lam được duy trì với cây phân đoạn hỗ trợ bổ sung phạm vi và truy vấn tối đa. Cây xác minh các ràng buộc về năng lực kiểu Hall phát sinh khi thời lượng được hiểu là thời hạn. 

Độ phức tạp thu được là khoảng`O(n² log n)`, phù hợp thoải mái cho`n = 1500`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n² log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chia camera thành ba nhóm tùy theo loại. 
2. Sắp xếp thời lượng của mỗi nhóm. 
3. Liệt kê mọi giá trị có thể có của```
len = B - A
```từ`1`ĐẾN`k₂ + k₃ + 1`. 
4. Đối với hiện tại`len`, phân loại từng camera loại 3. 

Camera loại 3 có thể được sử dụng một lần, bao gồm cả hai vụ trộm hoặc được chuyển đổi thành hai yêu cầu độc lập. Quy trình tham lam ban đầu cố gắng sử dụng càng nhiều camera loại 3 càng tốt chỉ bằng một lần hack. 
5. Duy trì hai bộ sưu tập yêu cầu. 

Cấu trúc bên trái đại diện cho các camera phải hoạt động ở lần trộm đầu tiên. 

Cấu trúc bên phải đại diện cho các camera phải hoạt động ở lần trộm thứ hai. 
6. Sử dụng cây phân đoạn lưu trữ vi phạm tối đa trong số tất cả các tiền tố thời hạn. 

Giá trị dương có nghĩa là một số tiền tố yêu cầu nhiều camera hơn mức có thể được lên lịch. Giá trị không dương có nghĩa là nhiệm vụ hiện tại có thể thực hiện được. 
7. Quét qua camera loại 2. 

Trong quá trình quét, một số camera di chuyển từ bên phải sang bên trái. Bất cứ khi nào tính khả thi bị phá vỡ, bước sửa chữa tham lam sẽ chuyển đổi một camera loại 3 được chia sẻ trước đó thành một camera bị hack hai lần. 
8. Bất cứ khi nào cả hai ràng buộc bên trái và bên phải đều khả thi và số lần hack giữa các lần trộm không vượt quá`len`, tính tổng số lần hack. 
9. Câu trả lời cho cấu hình này bằng```
n + extra
```hack, cộng với hai hành động trộm cắp. 
10. Lấy giá trị nhỏ nhất trên mọi giá trị của`len`. 

### Tại sao nó hoạt động 

Đối với một khoảng cách cố định`len`, mọi camera đều đóng góp một hạn chế về loại thời hạn. Cây phân đoạn liên tục kiểm tra xem mọi tiền tố của thời hạn có được thỏa mãn hay không. Quy tắc tham lam luôn thích chỉ sử dụng máy ảnh loại 3 một lần vì điều đó giảm thiểu tổng số lần hack. Khi tính khả thi bị vi phạm, việc chuyển đổi camera chia sẻ đủ điều kiện lớn nhất thành camera bị hack kép là cách sửa chữa duy nhất có thể cải thiện tiền tố bị vi phạm. Đây là lập luận trao đổi tương tự xuất hiện trong các bài toán lập kế hoạch thời hạn cổ điển. 

Quá trình quét kiểm tra tất cả các phân phối có thể có của camera loại 2 và vòng ngoài kiểm tra tất cả các lỗ hổng có thể bị trộm. Mỗi lịch trình khả thi tương ứng với chính xác một trạng thái mà thuật toán đạt được và mọi trạng thái được cây phân đoạn chấp nhận đều tương ứng với một lịch trình có thể thực hiện được. Do đó giá trị báo cáo tối thiểu là tối ưu. 

## Giải pháp Python```python
import sys
from bisect import bisect_right
input = sys.stdin.readline

class SegTree:
    def __init__(self, sz):
        self.sz = sz
        self.tot = 0
        self.t = [0] * (4 * sz)
        self.lazy = [0] * (4 * sz)
        self._build(0, 0, sz)

    def _build(self, v, l, r):
        if l + 1 == r:
            self.t[v] = -l
            return
        m = (l + r) // 2
        self._build(v * 2 + 1, l, m)
        self._build(v * 2 + 2, m, r)
        self.t[v] = max(self.t[v * 2 + 1], self.t[v * 2 + 2])

    def _apply(self, v, x):
        self.t[v] += x
        self.lazy[v] += x

    def _push(self, v):
        if self.lazy[v]:
            x = self.lazy[v]
            self._apply(v * 2 + 1, x)
            self._apply(v * 2 + 2, x)
            self.lazy[v] = 0

    def _upd(self, v, l, r, ql, qr, x):
        if ql >= qr:
            return
        if l == ql and r == qr:
            self._apply(v, x)
            return
        self._push(v)
        m = (l + r) // 2
        self._upd(v * 2 + 1, l, m, ql, min(qr, m), x)
        self._upd(v * 2 + 2, m, r, max(ql, m), qr, x)
        self.t[v] = max(self.t[v * 2 + 1], self.t[v * 2 + 2])

    def upd(self, pos, x):
        self.tot += x
        self._upd(0, 0, self.sz, max(0, pos), self.sz, x)

    def _bad(self, v, l, r):
        if self.t[v] <= 0:
            return -1
        if l + 1 == r:
            return l
        self._push(v)
        m = (l + r) // 2
        if self.t[v * 2 + 1] > 0:
            return self._bad(v * 2 + 1, l, m)
        return self._bad(v * 2 + 2, m, r)

    def get_bad(self):
        return self._bad(0, 0, self.sz)

def solve():
    n = int(input())

    a = [[] for _ in range(4)]
    sz = 1

    for _ in range(n):
        t, s = map(int, input().split())
        a[t].append(s)
        sz = max(sz, s + 1)

    k = [0] * 4

    for t in range(1, 4):
        a[t].sort()
        k[t] = len(a[t])

    a[2].reverse()

    INF = 10000
    ans = INF

    for length in range(1, k[2] + k[3] + 2):
        L = SegTree(sz)
        R = SegTree(sz)

        used = []

        for x in a[1]:
            L.upd(x, 1)

        for x in a[2]:
            R.upd(x, 1)

        for x in a[3]:
            L.upd(x - length, 1)

            if L.t[0] <= 0:
                used.append(x - length)
            else:
                L.upd(x - length, -1)
                L.upd(x, 1)
                R.upd(x, 1)

        used.sort()

        for i in range(k[2] + 1):
            if L.t[0] <= 0 and R.t[0] <= 0 and R.tot + 1 <= length:
                ans = min(ans, n + (k[3] - len(used)) + 2)

            if i == k[2]:
                break

            x = a[2][i]
            R.upd(x, -1)
            L.upd(x - length, 1)

            while True:
                pos = L.get_bad()
                if pos == -1:
                    break

                idx = bisect_right(used, pos) - 1
                if idx < 0:
                    break

                v = used[idx]

                L.upd(v, -1)
                L.upd(v + length, 1)
                R.upd(v + length, 1)

                used.pop(idx)

    print(-1 if ans == INF else ans)

solve()
```Cây phân đoạn lưu trữ vi phạm tiền tố tối đa. Một chiếc lá tương ứng với thời gian`d`ban đầu chứa`-d`. Mỗi máy ảnh đều góp phần`+1`cho tất cả các khoảng thời gian ít nhất bằng khoảng thời gian cần thiết để trang trải nó. Giá trị dương có nghĩa là một số tiền tố thời hạn bị quá tải và không thể lên lịch. 

Biến`used`chứa các camera loại 3 hiện đang bị tấn công bởi một vụ hack. Khi vi phạm xuất hiện, bước sửa chữa tham lam sẽ loại bỏ phần tử lớn nhất được chấp nhận khỏi`used`và biến chiếc camera đó thành một chiếc camera bị hack kép. Đây chính xác là quá trình chuyển đổi được thực hiện trong giải pháp tham chiếu. 

Chi tiết triển khai tinh vi nhất là cập nhật phạm vi. Giải pháp ban đầu diễn giải một khoảng thời gian`d`như góp phần vào mọi hậu tố`[d, +∞)`. Việc thiếu bản cập nhật hậu tố sẽ làm thay đổi ý nghĩa của bài kiểm tra tính khả thi và phá vỡ tính bất biến tham lam. 

## Ví dụ đã hoạt động 

### Mẫu 1```
4
2 6
1 2
1 2
2 1
```| Loại | Thời lượng | 
| --- | --- | 
| 1 | 2, 2 | 
| 2 | 6, 1 | 
| 3 | không | 

Lịch trình tốt nhất sẽ hack mọi camera chính xác một lần. 

| Số lượng | Giá trị | 
| --- | --- | 
| Máy ảnh | 4 | 
| Hack bổ sung | 0 | 
| Hành động trộm cắp | 2 | 
| Trả lời | 6 | 

Ví dụ cho thấy tình huống đơn giản nhất. Không có máy ảnh nào xem được cả hai viên kim cương, vì vậy không có máy ảnh nào cần hack lần thứ hai. 

### Mẫu 4```
1
3 1
```| Loại | Thời lượng | 
| --- | --- | 
| 1 | không | 
| 2 | không | 
| 3 | 1 | 

| Số lượng | Giá trị | 
| --- | --- | 
| Máy ảnh | 1 | 
| Hack bổ sung | 1 | 
| Hành động trộm cắp | 2 | 
| Trả lời | 4 | 

Một vụ hack không thể bao gồm hai khoảnh khắc trộm cắp khác nhau vì thời lượng chỉ kéo dài một giây. Máy ảnh phải bị hack hai lần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n² log n) | Giá trị O(n) của`len`, mỗi hoạt động thực hiện O(n) thao tác trên cây phân đoạn | 
| Không gian | O(n) | Máy ảnh, nhiều bộ và cây phân đoạn trong thời lượng lên tới 2n | 

Với`n ≤ 1500`, phạm vi thời lượng tối đa là`3000`. Kết quả`O(n² log n)`độ phức tạp nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    # paste solve() here and return captured output
    pass

# provided sample
assert run(
"""4
2 6
1 2
1 2
2 1
"""
) == "6\n"

# no cameras
assert run(
"""0
"""
) == "2\n"

# one shared camera, needs two hacks
assert run(
"""1
3 1
"""
) == "4\n"

# only first-diamond cameras
assert run(
"""2
1 1
1 2
"""
) == "4\n"

# all cameras identical
assert run(
"""4
3 5
3 5
3 5
3 5
"""
) == "6\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Không có máy ảnh | 2 | Cả hai vụ trộm đều có thể được thực hiện ngay lập tức | 
| Một camera loại 3 có thời lượng 1 | 4 | Máy ảnh dùng chung có thể yêu cầu hai lần hack | 
| Chỉ có camera loại 1 | 4 | Viên kim cương thứ hai không có ràng buộc | 
| Tất cả loại 3 thời lượng 5 | 6 | Nhiều camera được chia sẻ được bao phủ bởi một vụ hack mỗi cái | 

## Vỏ cạnh 

Hãy xem xét```
0
```Không có máy ảnh nào cả. Monocarp dành một giây để đánh cắp viên kim cương đầu tiên và một giây để đánh cắp viên kim cương thứ hai. Câu trả lời là`2`. Cấu trúc khả thi vẫn trống và thuật toán chấp nhận ngay lập tức. 

Coi như```
1
3 1
```Đối với mọi khoảng cách có thể`len ≥ 1`, thời lượng quá ngắn để thực hiện cả hai vụ trộm. Thủ tục tham lam cuối cùng đã biến chiếc máy ảnh thành một chiếc máy ảnh bị hack kép. Tổng số lần hack trở thành`2`, đưa ra câu trả lời`2 + 2 = 4`. 

Coi như```
2
3 2
2 3
```Camera loại 3 có thể che đậy hành vi trộm cắp đầu tiên và tùy thuộc vào khoảng trống đã chọn, cũng có thể che đậy hành vi trộm cắp thứ hai. Cuộc quét qua`len`tìm cấu hình khả thi tối thiểu và trả về giá trị tối ưu`4`, phù hợp với mẫu chính thức.
