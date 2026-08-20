---
title: "CF 102218F - Freddy Và Nhà Máy Sô Cô La"
description: "Chúng tôi có một dãy n ngày và giá trị nhu cầu cho mỗi ngày. Một bản cập nhật làm tăng nhu cầu của một ngày cụ thể. Trước khi bảo trì, nhà máy sản xuất b sôcôla mỗi ngày. Trong k ngày bảo trì nó không tạo ra gì và không thể bán được gì."
date: "2026-08-18T12:39:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102218
codeforces_index: "F"
codeforces_contest_name: "2019, XI Annual Programming Contest by ESCOM-IPN"
rating: 0
weight: 102218
solve_time_s: 511
verified: true
draft: false
---

[CF 102218F - Freddy và Nhà máy Sôcôla](https://codeforces.com/problemset/problem/102218/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 8m 31s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một loạt`n`ngày và giá trị nhu cầu cho mỗi ngày. Một bản cập nhật làm tăng nhu cầu của một ngày cụ thể. Trước khi bảo trì, nhà máy sản xuất`b`sôcôla mỗi ngày. Trong thời gian`k`ngày bảo trì nó không tạo ra gì và không thể bán được gì. Sau khi bảo trì kết thúc, nó tạo ra`a`sôcôla mỗi ngày. Ngày bắt đầu bảo trì`p`chỉ được chọn cho truy vấn loại 2, vì vậy mỗi truy vấn sẽ hỏi về một lịch trình sản xuất có thể khác nhau trong khi mảng nhu cầu hiện tại vẫn cố định. Vấn đề ban đầu sử dụng chính xác mô hình truy vấn và cập nhật này. 

Hạn chế quan trọng nhất là đơn đặt hàng trong ngày`i`không thể hài lòng trước ngày`i`, nhưng sôcôla không dùng đến có thể được cất giữ và bán sau. Do đó, nhà máy có thể tích lũy hàng tồn kho vào những ngày sản xuất vượt quá nhu cầu, sau đó dành số hàng tồn kho đó cho các đơn đặt hàng sau. 

Cho phép`D_i`là tổng nhu cầu trong ngày`1..i`, và để`P_i`là tổng sản lượng trong những ngày đó để bắt đầu bảo trì đã chọn. Nếu như`D_i > P_i`, thì ít nhất`D_i - P_i`đơn đặt hàng từ đầu tiên`i`ngày không bao giờ có thể được hoàn thành. Mức thâm hụt tiền tố lớn nhất như vậy xác định tổng nhu cầu chắc chắn sẽ bị mất bao nhiêu. 

Các ràng buộc làm cho mô phỏng trực tiếp không phù hợp. Với`n`lên đến`10^5`Và`q`lên đến`2 * 10^5`, một thuật toán lấy`O(n)`cho mọi truy vấn có thể thực hiện xung quanh`2 * 10^10`hoạt động trong trường hợp xấu nhất. Chúng tôi cần công việc logarit đại khái cho mỗi lần cập nhật và truy vấn. Thực tế là mọi cập nhật nhu cầu đều ảnh hưởng đến tất cả các tổng tiền tố kể từ ngày của nó trở đi hướng trực tiếp đến cây phân đoạn lười. 

Có một số ranh giới trong đó việc triển khai có thể âm thầm gặp trục trặc. Đầu tiên là bảo trì bắt đầu vào ngày`1`. Ví dụ,```
3 1 3 1 3
1 1 2
1 3 1
2 1
```Câu trả lời là`1`. Không có sản xuất hoặc bán hàng trong ngày`1`, thế là 2 đơn hàng ngày hôm đó bị mất. Sô-cô-la được sản xuất vào những ngày`2`Và`3`chỉ có thể giúp với một đơn hàng trong ngày`3`. Một giải pháp bất cẩn chỉ so sánh tổng nhu cầu với tổng sản lượng có thể cho rằng cả ba đơn hàng đều có thể bán được một cách không chính xác. 

Ranh giới còn lại là việc bảo trì kết thúc đúng ngày`n`. Coi như```
4 2 3 1 2
1 4 10
2 3
```Câu trả lời là`2`. Với`p = 3`, việc sản xuất chỉ diễn ra vào những ngày`1`Và`2`, tặng hai viên sôcôla, trong khi ngày`3`Và`4`đều là ngày bảo trì. Coi khoảng thời gian bảo trì như`[p, p+k]`thay vì`[p, p+k-1]`thay đổi lịch trình sản xuất và đưa ra kết quả sai. 

Khi`k = n`, toàn bộ nhà máy không có sẵn. Ví dụ,```
3 3 2 1 3
1 1 1
1 3 5
2 1
```có câu trả lời`0`. Không có hoạt động sản xuất hoặc bán hàng nào trong ngày, mặc dù tỷ lệ sản xuất sửa chữa danh nghĩa là lớn. Việc triển khai đánh giá một cách mù quáng phạm vi sau bảo trì có thể vô tình đưa vào sản xuất sau ngày này`n`. 

Cuối cùng, nhu cầu bằng 0 cần được xử lý đặc biệt. Vì```
2 1 2 1 1
2 1
```câu trả lời là`0`. Thâm hụt tiền tố tối đa được phép âm hoặc bằng 0, nhưng số lượng đơn hàng không được đáp ứng không thể âm. quên đi`max(0, ...)`trong công thức cuối cùng có thể đưa ra câu trả lời phủ định trong những trường hợp như vậy. 

## Phương pháp tiếp cận 

Một giải pháp đơn giản sẽ mô phỏng rõ ràng nhà máy cho mọi truy vấn loại 2. Đối với một người được chọn`p`, chúng ta có thể đi qua tất cả`n`ngày, duy trì lượng hàng tồn kho hiện tại, bổ sung sản lượng trong ngày khi sản lượng được phép, bán càng nhiều sôcôla càng tốt cho nhu cầu của ngày đó và tích lũy số lượng đã bán. Điều này đúng vì việc bán càng nhiều càng tốt mỗi ngày không bao giờ có thể gây tổn hại đến ngày mai: bất kỳ loại sô-cô-la nào không được bán hôm nay vẫn có sẵn sau này, trong khi việc đáp ứng một đơn đặt hàng có sẵn ngay lập tức không thể làm giảm số lượng cuối cùng có thể bán được. 

Vấn đề là chi phí. Một truy vấn loại 2 cần`O(n)`, và có thể có`q = 2 * 10^5`truy vấn. Trong trường hợp xấu nhất đây là`O(nq) = O(2 * 10^10)`hoạt động hàng ngày, vượt xa thời gian có sẵn. 

Quan sát hữu ích là chúng ta thực sự không cần phải mô phỏng hàng tồn kho. Đối với lịch trình sản xuất cố định, hãy xem xét bất kỳ tiền tố nào của ngày`1..i`. Nhiều nhất`P_i`sôcôla có thể đã được sản xuất vào thời điểm đó, nên ít nhất`max(0, D_i-P_i)`các đơn đặt hàng từ tiền tố đó phải bị mất. Sự thiếu hụt tiền tố tồi tệ nhất là thông tin duy nhất quan trọng về toàn bộ lịch trình. 

Giả sử thâm hụt tiền tố lớn nhất là`L`. Thế thì nhiều nhất`D_n-L`sôcôla có thể được bán. Giới hạn này có thể đạt được bằng cách bán tham lam bất cứ khi nào có đơn đặt hàng, vì vậy câu trả lời chính xác là`D_n - max(0, max_i(D_i-P_i))`. 

Nhiệm vụ còn lại là đánh giá mức tối đa đó một cách nhanh chóng trong khi nhu cầu đang được cập nhật. 

Để bắt đầu bảo trì`p`, sản xuất có ba hình thức đơn giản. Trước khi bảo trì,`P_i = bi`. Trong quá trình bảo trì,`P_i = b(p-1)`. Sau khi bảo trì, việc sản xuất sẽ được thực hiện`b(p-1) + a(i-p-k+1)`. 

Xác định ba mảng từ nhu cầu tiền tố:`F_i = D_i - bi`

`H_i = D_i`

`G_i = D_i - ai`. 

Khi đó mức thâm hụt lớn nhất trước khi bảo trì là mức tối đa`F_i`vì`i < p`. Trong thời gian bảo trì là tối đa`H_i`vì`p <= i <= p+k-1`, trừ`b(p-1)`. Sau bảo trì là tối đa`G_i`vì`i >= p+k`, cộng với hằng số chỉ phụ thuộc vào`p`. 

Cập nhật nhu cầu trong ngày`d`tăng mọi nhu cầu tiền tố`D_i`với`i >= d`. Do đó, nó thêm cùng một giá trị vào`F_i`,`H_i`, Và`G_i`trên hậu tố`[d,n]`. Do đó, chúng tôi cần một cấu trúc dữ liệu hỗ trợ bổ sung hậu tố và phạm vi truy vấn tối đa. Cây phân đoạn lười xử lý chính xác thao tác này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(nq)`|`O(n)`| Quá chậm | 
| Tối ưu |`O((n+q) log n)`|`O(n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng ba mảng khái niệm`F`,`H`, Và`G`giả định rằng mọi nhu cầu ban đầu đều bằng không. Tại vị trí`i`, giá trị của chúng là`-bi`,`0`, Và`-ai`. Đây chính xác là những biểu thức cần thiết cho ba giai đoạn sản xuất. 
2. Lưu trữ tối đa mỗi mảng trong số ba mảng trong mỗi nút cây phân đoạn. Một giá trị lười ghi lại một phần bổ sung đã được áp dụng cho toàn bộ nút nhưng chưa được đẩy tới các nút con của nó. 
3. Khi có bản cập nhật`(1, d, x)`đến, làm tăng tổng cầu lên`x`. Nhu cầu tiền tố`D_i`chỉ thay đổi đối với`i >= d`, vì vậy hãy thêm`x`cho cả ba giá trị cây phân đoạn trên hậu tố`[d,n]`. 
4. Đối với một truy vấn`(2, p)`, cho phép`m = p+k-1`. Chia ngày thành ba phạm vi`[1,p-1]`,`[p,m]`, Và`[m+1,n]`. Cây phân đoạn có thể tìm thấy mức tối đa có liên quan từ`F`,`H`, Và`G`cho các phạm vi này. 
5. Đối với những ngày trước khi bảo trì, hãy lấy`L = max(F_i)`qua`1 <= i < p`. Vì sản xuất là`bi`vào ngày`i`, đây chính xác là`max(D_i-P_i)`trong phạm vi đó. 
6. Đối với những ngày bảo trì, hãy lấy`M = max(H_i)`qua`p <= i <= m`. Sản lượng vào cuối bất kỳ ngày bảo trì nào là`b(p-1)`, do đó mức thâm hụt tương ứng là`M-b(p-1)`. 
7. Trong những ngày sau bảo trì, hãy lấy`R = max(G_i)`qua`m+1 <= i <= n`. Cho một ngày như vậy`i`, sản xuất là`b(p-1) + a(i-p-k+1)`. 

Sắp xếp lại biểu thức này cho`P_i = ai + (b-a)(p-1) - ak`. 

Kể từ đây`D_i-P_i = G_i + (a-b)(p-1) + ak`. 

Do đó, thâm hụt sau bảo trì là`R + (a-b)(p-1) + ak`. 
8. Lấy`L`,`M`, Và`R`cùng với số 0, vì lượng cầu bị mất không thể âm. Nếu như`bad`là mức tối đa của họ, câu trả lời là`D_n-bad`. 

### Tại sao nó hoạt động 

Đối với mỗi tiền tố kết thúc vào ngày`i`, không có chiến lược nào có thể bán được nhiều hơn`P_i`sôcôla cho các đơn hàng thuộc tiền tố đó. Như vậy ít nhất`D_i-P_i`đơn đặt hàng bị mất bất cứ khi nào số lượng này là dương. Do đó, mức thâm hụt tiền tố lớn nhất là giới hạn thấp hơn về số lượng đơn hàng bị mất. 

Chiến lược tham lam bán càng nhiều sôcôla càng tốt mỗi khi có đơn đặt hàng nhận ra chính xác giới hạn này. Bất cứ khi nào hàng tồn kho không đủ, mức thiếu hụt chính xác là nhu cầu tiền tố hiện tại trừ đi sản lượng tiền tố và sự thiếu hụt lớn nhất gặp phải là thâm hụt tiền tố tối đa. Khi quá trình sản xuất sau đó đến, nó có thể đáp ứng các đơn đặt hàng sau nhưng không thể sửa chữa đơn hàng đã bị bỏ lỡ. Do đó, mức thâm hụt tiền tố tối đa chính xác là khoản lỗ không thể tránh khỏi và trừ đi nó khỏi tổng nhu cầu sẽ có số lượng sôcôla bán được tối ưu. 

Cây phân đoạn luôn đại diện cho các giá trị hiện tại của`F_i`,`H_i`, Và`G_i`. Một bản cập nhật sẽ thêm cùng một lượng vào mỗi tổng tiền tố bị ảnh hưởng bởi nhu cầu đó, do đó, bản cập nhật hậu tố lười biếng sẽ giữ nguyên cả ba định nghĩa. Truy vấn phân chia các phần thiếu hụt tiền tố theo công thức sản xuất chính xác trong từng giai đoạn, do đó giá trị lớn nhất được cây trả về chính xác là phần thiếu hụt tiền tố lớn nhất có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

NEG = -(10 ** 30)

class SegmentTree:
    def __init__(self, n, a, b):
        self.n = n
        self.a = a
        self.b = b

        size = 4 * n + 5
        self.f = [0] * size
        self.h = [0] * size
        self.g = [0] * size
        self.lazy = [0] * size

        self._build(1, 1, n)

    def _build(self, v, l, r):
        if l == r:
            self.f[v] = -self.b * l
            self.h[v] = 0
            self.g[v] = -self.a * l
            return

        mid = (l + r) >> 1
        self._build(v << 1, l, mid)
        self._build(v << 1 | 1, mid + 1, r)
        self._pull(v)

    def _pull(self, v):
        left = v << 1
        right = left | 1

        self.f[v] = max(self.f[left], self.f[right])
        self.h[v] = max(self.h[left], self.h[right])
        self.g[v] = max(self.g[left], self.g[right])

    def _apply(self, v, x):
        self.f[v] += x
        self.h[v] += x
        self.g[v] += x
        self.lazy[v] += x

    def _push(self, v):
        x = self.lazy[v]
        if x:
            left = v << 1
            self._apply(left, x)
            self._apply(left | 1, x)
            self.lazy[v] = 0

    def add_suffix(self, start, x):
        self._add_suffix(1, 1, self.n, start, x)

    def _add_suffix(self, v, l, r, start, x):
        if r < start:
            return

        if start <= l:
            self._apply(v, x)
            return

        self._push(v)

        mid = (l + r) >> 1
        self._add_suffix(v << 1, l, mid, start, x)
        self._add_suffix(v << 1 | 1, mid + 1, r, start, x)

        self._pull(v)

    def query_phases(self, p, m):
        return self._query_phases(1, 1, self.n, p, m)

    def _query_phases(self, v, l, r, p, m):
        if r < p:
            return self.f[v], NEG, NEG

        if l > m:
            return NEG, NEG, self.g[v]

        if p <= l and r <= m:
            return NEG, self.h[v], NEG

        self._push(v)

        mid = (l + r) >> 1

        lf, lh, lg = self._query_phases(v << 1, l, mid, p, m)
        rf, rh, rg = self._query_phases(
            v << 1 | 1, mid + 1, r, p, m
        )

        return (
            max(lf, rf),
            max(lh, rh),
            max(lg, rg),
        )

def solve():
    n, k, a, b, q = map(int, input().split())

    seg = SegmentTree(n, a, b)
    total_demand = 0
    out = []

    for _ in range(q):
        query = list(map(int, input().split()))

        if query[0] == 1:
            d, x = query[1], query[2]

            total_demand += x
            seg.add_suffix(d, x)

        else:
            p = query[1]
            m = p + k - 1

            before, during, after = seg.query_phases(p, m)

            bad = before

            if during != NEG:
                bad = max(bad, during - b * (p - 1))

            if after != NEG:
                bad = max(
                    bad,
                    after + (a - b) * (p - 1) + a * k
                )

            bad = max(0, bad)
            out.append(str(total_demand - bad))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Cây phân đoạn chứa ba cực đại tại mỗi nút vì cùng một nhu cầu tiền tố được so sánh với ba công thức sản xuất khác nhau. Tất cả ba giá trị đều nhận được các phần bổ sung hậu tố giống hệt nhau, vì vậy một giá trị lười là đủ cho toàn bộ nút. 

Giá trị ban đầu vào ngày`i`là`-bi`vì`F`,`0`vì`H`, Và`-ai`vì`G`. Không cần có mảng nhu cầu rõ ràng. Cập nhật trong ngày`d`thay đổi mọi tổng tiền tố từ`D_d`trở đi, đó là lý do tại sao bản cập nhật trở thành phần bổ sung phạm vi hậu tố. 

Quy trình truy vấn chuyên biệt hơn một chút so với truy vấn tối đa phạm vi tiêu chuẩn. Hai ranh giới`p`Và`p+k-1`chia toàn bộ mảng thành ba giai đoạn sản xuất. Nếu một phân đoạn nằm hoàn toàn trong một pha thì giá trị tối đa được lưu trữ tương ứng có thể được trả về ngay lập tức. Chỉ các nút vượt qua một trong hai ranh giới mới cần lặp lại. Điều này giữ cho mỗi truy vấn logarit trong khi tránh được ba truy vấn phạm vi độc lập. 

các`NEG`giá trị chỉ được sử dụng cho các pha trống. Nó cố tình nhỏ hơn nhiều so với bất kỳ giá trị thực nào có thể có. Vì tổng nhu cầu nhiều nhất là`2 * 10^9`và sản lượng nhiều nhất là khoảng`10^10`, số nguyên Python không có mối lo ngại về tràn ở đây. 

Khoảng thời gian bảo trì là`[p, p+k-1]`, do đó điểm cuối bên phải của nó được tính là`m = p+k-1`. Phạm vi sau bảo trì bắt đầu vào lúc`m+1 = p+k`. Hai cách diễn đạt này là nơi xảy ra nhiều lỗi sai lầm nhất. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Sau ba lần cập nhật đầu tiên, mảng nhu cầu là`[2,1,0,0,3]`, vì vậy nhu cầu tiền tố là`[2,3,3,3,6]`. 

Vì`p=2`, bảo trì mất nhiều ngày`2`Và`3`. 

| Giai đoạn | Phạm vi chỉ số | Được lưu trữ tối đa | Điều chỉnh | Thâm hụt | 
| --- | --- | --- | --- | --- | 
| Trước |`1..1`|`max(D_i-i)=1`|`0`|`1`| 
| Trong |`2..3`|`max(D_i)=3`|`-1`|`2`| 
| Sau |`4..5`|`max(D_i-2i)=-4`|`+5`|`1`| 
| Tất cả các giai đoạn | | | |`max(0,1,2,1)=2`| 

Tổng nhu cầu là`6`, do đó hai đơn hàng chắc chắn sẽ bị mất và câu trả lời là`4`. 

Sau hai lần cập nhật tiếp theo, nhu cầu sẽ trở thành`[2,1,2,2,3]`, với tổng nhu cầu`10`và yêu cầu tiền tố`[2,3,5,7,10]`. 

Vì`p=1`, bảo trì mất nhiều ngày`1`Và`2`. 

| Giai đoạn | Phạm vi chỉ số | Được lưu trữ tối đa | Điều chỉnh | Thâm hụt | 
| --- | --- | --- | --- | --- | 
| Trước | trống |`NEG`|`0`| bỏ qua | 
| Trong |`1..2`|`max(D_i)=3`|`0`|`3`| 
| Sau |`3..5`|`max(D_i-2i)=0`|`+4`|`4`| 
| Tất cả các giai đoạn | | | |`4`| 

Câu trả lời là`10-4=6`. 

Vì`p=3`, bảo trì mất nhiều ngày`3`Và`4`. 

| Giai đoạn | Phạm vi chỉ số | Được lưu trữ tối đa | Điều chỉnh | Thâm hụt | 
| --- | --- | --- | --- | --- | 
| Trước |`1..2`|`max(D_i-i)=1`|`0`|`1`| 
| Trong |`3..4`|`max(D_i)=7`|`-2`|`5`| 
| Sau |`5..5`|`D_5-2*5=0`|`+6`|`6`| 
| Tất cả các giai đoạn | | | |`6`| 

Câu trả lời là`10-6=4`. 

Ba truy vấn này thực hiện cả phạm vi pha trống và sự thay đổi trong điều chỉnh sau bảo trì khi`p`di chuyển. 

### Ví dụ tùy chỉnh 

Hãy xem xét```
5 2 3 1 3
1 1 2
1 3 5
2 3
```Nhu cầu là`[2,0,5,0,0]`, với nhu cầu tiền tố`[2,2,7,7,7]`và tổng nhu cầu`7`. Bảo trì bắt đầu vào ngày`3`, thế là ngày`3`Và`4`không tạo ra gì cả. 

| Giai đoạn | Phạm vi chỉ số | Được lưu trữ tối đa | Điều chỉnh | Thâm hụt | 
| --- | --- | --- | --- | --- | 
| Trước |`1..2`|`max(D_i-i)=1`|`0`|`1`| 
| Trong |`3..4`|`max(D_i)=7`|`-2`|`5`| 
| Sau |`5..5`|`D_5-3*5=-8`|`+8`|`0`| 
| Tất cả các giai đoạn | | | |`5`| 

Câu trả lời là`7-5=2`. Năm đơn hàng tích lũy theo ngày`3`tất cả đều không thể hài lòng vì chỉ có hai viên sôcôla được sản xuất trong những ngày`1`Và`2`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n + q log n)`| Cây được xây dựng một lần, mỗi bản cập nhật là một phần bổ sung phạm vi hậu tố và mỗi truy vấn sẽ truy cập`O(log n)`các nút ranh giới có liên quan. | 
| Không gian |`O(n)`| Mảng cây phân đoạn lưu trữ ba giá trị cực đại và một giá trị lười cho mỗi nút. | 

Với`n <= 10^5`Và`q <= 2 * 10^5`, giải pháp chỉ thực hiện công việc logarit cho mỗi thao tác thay vì quét cả ngày cho mọi câu hỏi. Cây phân đoạn sử dụng bộ nhớ tuyến tính và duy trì thoải mái trong giới hạn 256 MB. 

## Trường hợp thử nghiệm 

Khai thác thử nghiệm sau đây giả định giải pháp đã gửi được lưu trữ dưới dạng`solution.py`và phơi bày`solve()`chức năng hiển thị ở trên.```python
import sys
import io

from solution import solve

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output

    try:
        solve()
        return output.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample
assert run(
    """\
5 2 2 1 8
1 1 2
1 5 3
1 2 1
2 2
1 4 2
1 3 2
2 1
2 3
"""
) == "4\n6\n4", "sample 1"

# Minimum-size input, maintenance covers the only day.
assert run(
    """\
1 1 2 1 2
1 1 3
2 1
"""
) == "0", "minimum-size case"

# Equal demand on every day, with queries at different boundaries.
assert run(
    """\
4 1 2 1 6
1 1 1
1 2 1
1 3 1
1 4 1
2 2
2 4
"""
) == "3\n3", "equal daily demands"

# Maintenance starts at the last possible day.
assert run(
    """\
4 2 3 1 3
1 4 10
2 3
2 1
"""
) == "2\n6", "last possible maintenance start"

# Maintenance covers the complete array.
assert run(
    """\
3 3 2 1 3
1 1 1
1 3 5
2 1
"""
) == "0", "maintenance covers all days"

# Maximum n, with 100000 operations.
# 99999 orders are all placed on day 1, followed by a query.
# Maintenance starts on day 1, so none of these orders can be sold.
parts = ["100000 1 100000 99999 100000"]
for _ in range(99999):
    parts.append("1 1 1")
parts.append("2 1")

assert run("\n".join(parts) + "\n") == "0", "maximum-n case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 |`4`,`6`,`4`| Trình tự cập nhật/truy vấn đầy đủ và thay đổi vị trí bảo trì | 
|`1 1 2 1 ...`|`0`| Kích thước tối thiểu và bảo trì chỉ trong ngày | 
| Four equal daily demands |`3`,`3`| Nhu cầu bình đẳng lặp đi lặp lại và ranh giới bảo trì khác nhau | 
| Bảo trì bắt đầu từ ngày`3`hoặc`1`|`2`,`6`| Last possible start and exact maintenance interval |
 |`k=n`|`0`| Không sản xuất hoặc bán trong suốt thời gian | 
|`n=100000`, 100000 hoạt động |`0`| Kích thước đầu vào lớn và cập nhật hậu tố lặp đi lặp lại | 

## Vỏ cạnh 

### Bảo trì bắt đầu vào ngày thứ nhất 

cho```
3 1 3 1 3
1 1 2
1 3 1
2 1
```chúng tôi có`D = [2,2,3]`. Từ`p=1`Và`k=1`, số lượng sản xuất là`[0,3,3]`. Các thiếu hụt tiền tố là`2`,`-1`, Và`-3`, do đó mức thâm hụt tối đa là`2`. Tổng nhu cầu là`3`, cho`3-2=1`. 

Trong truy vấn cây phân đoạn, phạm vi trước bảo trì trống, phạm vi bảo trì là ngày`1`và phạm vi sau bảo trì là ngày`2..3`. Phạm vi trống đóng góp`NEG`, vì vậy nó không thể vô tình trở thành câu trả lời. 

### Bảo trì kết thúc vào ngày thứ n 

cho```
4 2 3 1 2
1 4 10
2 3
```truy vấn có`p=3`Và`m=4`. Sản xuất là`1`vào những ngày`1`Và`2`, tiếp theo là không sản xuất vào những ngày`3`Và`4`. Nhà máy có sẵn chính xác hai viên sôcôla và đơn hàng duy nhất dành cho ngày`4`, vậy câu trả lời là`2`. 

Cây sử dụng phạm vi bảo trì`[3,4]`và một phạm vi trống sau bảo trì. Điều này trực tiếp kiểm tra rằng`p+k-1`, còn hơn là`p+k`, là ngày bảo trì cuối cùng. 

### Bảo trì hàng ngày 

cho```
3 3 2 1 3
1 1 1
1 3 5
2 1
```chúng tôi có`p=1`Và`k=3`, Vì thế`m=3=n`. Ngày nào cũng là ngày bảo trì. Mức bảo trì được lưu trữ tối đa là`D_3=6`, trong khi hai dải pha còn lại trống. Mức thâm hụt tối đa là`6`, bằng tổng cầu, nên câu trả lời là`0`. 

Việc thực hiện không bao giờ cần một trường hợp đặc biệt cho`k=n`. Các phạm vi trống trước và sau được biểu thị bằng`NEG`, trong khi dải giữa bao phủ toàn bộ cây phân đoạn một cách tự nhiên. 

### Không có nhu cầu 

cho```
2 1 2 1 1
2 1
```mọi giá trị nhu cầu đều bằng không. Bảo trì bắt đầu vào ngày`1`, các biểu thức tiền tố được lưu trữ đều không dương, do đó mức thâm hụt tối đa thô nhiều nhất là bằng 0. Áp dụng`max(0, bad)`chuyển đổi nó thành chính xác bằng 0 và câu trả lời là`D_n-bad = 0`. 

Đây là lý do tại sao kẹp cuối cùng về 0 nằm ngoài phép tính ba pha. Một tiền tố có thể có sản lượng dư thừa, nhưng sản xuất dư thừa không thể tạo ra số lượng đơn đặt hàng không được đáp ứng âm. 

### Cập nhật lặp đi lặp lại trong cùng một ngày 

Nếu một số truy vấn loại 1 nhắm mục tiêu vào cùng một ngày thì mỗi bản cập nhật sẽ bổ sung thêm vào nhu cầu hiện tại thay vì thay thế nó. Giả sử một ngày nhận được số tiền tăng thêm`4`, sau đó`7`. Nhu cầu của nó trở nên`11`và mọi tiền tố bắt đầu vào hoặc sau ngày đó sẽ tăng thêm`11`tổng cộng. Cây phân đoạn thực hiện hai phép bổ sung hậu tố, do đó các hiệu ứng sẽ tự động tích lũy. 

Đây cũng là lý do tại sao chỉ lưu trữ nhu cầu cuối cùng mỗi ngày sẽ không đủ trong quá trình xử lý. Mỗi bản cập nhật sẽ ngay lập tức thay đổi các biểu thức tiền tố được sử dụng bởi các truy vấn loại 2 sau này và cây lười sẽ giữ nguyên những thay đổi đó mà không cần xây dựng lại tổng tiền tố.
