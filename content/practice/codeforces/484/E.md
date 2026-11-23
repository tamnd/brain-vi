---
title: "CF 484E - Hàng rào ký tên"
description: "Chúng ta có một hàng rào được thể hiện bằng một dãy độ cao. Biển hiệu hình chữ nhật phải được đặt phía trên một số tấm biển liên tiếp. Biển có chiều rộng cố định w nên phải che đúng w tấm hàng rào liên tiếp."
date: "2026-06-07T17:25:15+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "constructive-algorithms", "data-structures"]
categories: ["algorithms"]
codeforces_contest: 484
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 276 (Div. 1)"
rating: 2500
weight: 484
solve_time_s: 199
verified: true
draft: false
---

[CF 484E - Đăng nhập vào hàng rào](https://codeforces.com/problemset/problem/484/E) 

**Đánh giá:** 2500 
**Tags:** tìm kiếm nhị phân, thuật toán xây dựng, cấu trúc dữ liệu 
**Thời gian giải:** 3m 19s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một hàng rào được thể hiện bằng một dãy độ cao. Biển hiệu hình chữ nhật phải được đặt phía trên một số tấm biển liên tiếp. Biển hiệu có chiều rộng cố định`w`, do đó nó phải bao phủ chính xác`w`tấm hàng rào liên tiếp. Chiều cao của nó không được vượt quá tấm bảng ngắn nhất trong số những tấm được che phủ, nếu không một phần của biển hiệu sẽ nhô ra phía trên hàng rào. 

Đối với một truy vấn`(l, r, w)`, chúng ta có thể chọn bất kỳ khối liên tiếp nào có chính xác`w`bảng hoàn toàn chứa bên trong khoảng`[l, r]`. Nếu chúng ta đặt dấu hiệu trên một khối`[x, x+w-1]`, chiều cao ký hiệu tối đa có thể bằng$$\min(h_x,h_{x+1},\ldots,h_{x+w-1}).$$Trong số tất cả các vị trí hợp lệ bên trong`[l,r]`, chúng tôi muốn giá trị lớn nhất như vậy. 

Tương tự, mỗi truy vấn sẽ hỏi:$$\max_{x \in [l,r-w+1]} \min(h_x,\ldots,h_{x+w-1}).$$Hàng rào chứa tới$10^5$bảng điều khiển và có tới$10^5$truy vấn. Bất kỳ thuật toán nào quét trực tiếp khoảng thời gian truy vấn đều vô vọng. Thậm chí một$O((r-l+1)\log n)$giải pháp cho mỗi truy vấn có thể yêu cầu khoảng$10^{10}$hoạt động trong trường hợp xấu nhất. 

Các ràng buộc gợi ý rằng tất cả các truy vấn phải được xử lý cùng nhau. Xung quanh sự phức tạp$O((n+m)\log^2 n)$hoặc$O((n+m)\log n)$là phù hợp. 

Một khía cạnh tinh tế là câu trả lời không phải là chiều cao tối thiểu trong toàn bộ khoảng thời gian. Chúng ta có thể tự do lựa chọn nơi có chiều rộng-`w`dấu hiệu được đặt. 

Coi như:```
heights = [5,1,5]
query = (1,3,1)
```Câu trả lời là`5`, vì chiều rộng`1`cho phép chọn một trong hai điểm cuối. Một giải pháp chỉ đơn giản là tận dụng tối thiểu`[1,3]`sẽ quay lại không chính xác`1`. 

Một lỗi phổ biến khác là quên rằng chiều rộng biển báo là cố định.```
heights = [4,4,1,4]
query = (1,4,3)
```Các cửa sổ hợp lệ là`[4,4,1]`Và`[4,1,4]`, cả hai đều có tối thiểu`1`. Câu trả lời là`1`, mặc dù tồn tại một cửa sổ ngắn hơn với mức tối thiểu`4`. 

Trường hợp cạnh thứ ba xảy ra khi phải sử dụng toàn bộ khoảng.```
heights = [7,3,5]
query = (1,3,3)
```Chỉ có một vị trí tồn tại, vì vậy câu trả lời là khoảng thời gian tối thiểu, cụ thể là`3`. 

Giải pháp tối ưu phải xử lý chính xác tất cả các tình huống này. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất sẽ đánh giá mọi vị trí có thể đặt biển báo. Đối với một truy vấn`(l,r,w)`, chúng tôi kiểm tra tất cả các vị trí bắt đầu từ`l`ĐẾN`r-w+1`. Đối với mỗi chiều rộng-`w`window chúng ta tính toán chiều cao tối thiểu của nó và lấy giá trị tối đa. 

Điều này đúng vì nó kiểm tra rõ ràng mọi vị trí đặt biển báo hợp pháp. 

Vấn đề là thời gian chạy. Một khoảng truy vấn có thể chứa$O(n)$cửa sổ ứng viên. Ngay cả khi cấu trúc phạm vi tối thiểu cung cấp cho mỗi cửa sổ mức tối thiểu$O(1)$, một truy vấn vẫn có giá$O(n)$. Với$10^5$truy vấn, trường hợp xấu nhất sẽ trở thành$10^{10}$hoạt động. 

Quan sát quan trọng là ngừng suy nghĩ trực tiếp về câu trả lời và thay vào đó hãy đặt câu hỏi quyết định: 

Đối với ngưỡng chiều cao$H$, liệu có tồn tại một chiều rộng-`w`chặn bên trong`[l,r]`mỗi bảng có chiều cao ít nhất$H$? 

Nếu chúng ta đánh dấu mọi vị trí có chiều cao ít nhất$H$khi hoạt động, câu hỏi sẽ trở thành: 

Có phân đoạn hoạt động liên tiếp bên trong`[l,r]`có chiều dài ít nhất`w`? 

Điều này biến bài toán tối ưu ban đầu thành bài toán quyết định đơn điệu. Nếu chiều cao$H$hoạt động, mọi chiều cao nhỏ hơn cũng hoạt động. Sự đơn điệu đó gợi ý tìm kiếm nhị phân. 

Tuy nhiên, việc tìm kiếm nhị phân một cách độc lập cho mỗi truy vấn vẫn sẽ quá tốn kém. Bí quyết tiêu chuẩn là tìm kiếm nhị phân song song. 

Đối với một ngưỡng cố định$H$, chúng tôi kích hoạt tất cả các vị trí có chiều cao ít nhất$H$. Sau đó, chúng ta cần một cấu trúc dữ liệu có thể trả lời: 

Khoảng bên trong`[l,r]`, độ dài tối đa của một đoạn hoạt động liên tiếp là bao nhiêu? 

Điều này có thể được duy trì bằng cách lưu trữ cây phân đoạn: 

- tiền tố hoạt động dài nhất 
- hậu tố hoạt động dài nhất 
- đoạn hoạt động dài nhất 
- độ dài khoảng 

Khi độ cao được kích hoạt theo thứ tự giảm dần, các cập nhật sẽ là phép gán điểm. 

Các nhóm tìm kiếm nhị phân song song cùng nhau tất cả các truy vấn hiện đang kiểm tra cùng thứ hạng chiều cao. Trong một lần quét độ cao, chúng tôi kích hoạt bảng điều khiển và đánh giá tất cả các truy vấn đó. Mỗi truy vấn chỉ cần$O(\log n)$cây phân đoạn hoạt động trên mỗi vòng tìm kiếm nhị phân. 

Sự phức tạp kết quả trở thành$O((n+m)\log^2 n)$, vừa vặn thoải mái. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(mn)$|$O(1)$| Quá chậm | 
| Tối ưu |$O((n+m)\log^2 n)$|$O(n+m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đầu tiên nén các câu trả lời của ứng viên. Câu trả lời cho mọi truy vấn phải bằng một trong các chiều cao của hàng rào, vì việc tăng chiều cao biển báo chỉ thay đổi tính khả thi khi vượt qua chiều cao của bảng điều khiển. 

Hãy sắp xếp các độ cao khác nhau theo thứ tự giảm dần. 

### 1. Khởi tạo phạm vi tìm kiếm nhị phân cho mọi truy vấn 

Đối với mỗi truy vấn duy trì các chỉ số`low`Và`high`thành các độ cao khác nhau được sắp xếp. 

Ban đầu mọi độ cao đều có thể thực hiện được, vì vậy:`low = 0`

`high = number_of_distinct_heights - 1`### 2. Thực hiện lặp lại một vòng tìm kiếm nhị phân song song 

Đối với mỗi truy vấn chưa được giải quyết, hãy tính toán$$mid = \frac{low+high}{2}.$$Nhóm các truy vấn theo điểm giữa hiện tại của chúng. 

Tất cả các truy vấn trong cùng một nhóm đều hỏi liệu một ngưỡng chiều cao cụ thể có khả thi hay không. 

### 3. Quét chiều cao từ lớn nhất đến nhỏ nhất 

Sắp xếp vị trí hàng rào theo chiều cao giảm dần. 

Xử lý các nhóm điểm giữa theo thứ tự ngưỡng giảm dần. 

Bất cứ khi nào ngưỡng hiện tại giảm, hãy kích hoạt tất cả các vị trí hàng rào có chiều cao ít nhất bằng ngưỡng đó. 

Tập hoạt động thể hiện chính xác các bảng có thể hỗ trợ ký hiệu có chiều cao đó. 

### 4. Duy trì các phân đoạn đang hoạt động trong cây phân đoạn 

Mỗi lá tương ứng với một bảng. 

Một cửa hàng bảng điều khiển không hoạt động: 

- tiền tố = 0 
- hậu tố = 0 
- tốt nhất = 0 

Một bảng điều khiển hoạt động lưu trữ: 

- tiền tố = 1 
- hậu tố = 1 
- tốt nhất = 1 

Khi kết hợp hai con:$$prefix = \begin{cases} left.length + right.prefix & \text{if left is completely active}\\ left.prefix & \text{otherwise} \end{cases}$$Hậu tố được tính toán đối xứng. 

Phân đoạn tốt nhất là tối đa: 

- left.best 
- đúng.tốt nhất 
- left.suffix + right.prefix 

### 5. Trả lời các câu hỏi về tính khả thi 

Đối với truy vấn`(l,r,w)`lấy thông tin cây phân đoạn trên`[l,r]`. 

cánh đồng`best`bằng với lần chạy hoạt động liên tiếp dài nhất trong khoảng thời gian đó. 

Ngưỡng này khả thi chính xác khi$$best \ge w.$$Nếu khả thi, hãy di chuyển tìm kiếm nhị phân lên tầm cao hơn. 

Nếu không hãy di chuyển nó về phía độ cao nhỏ hơn. 

### 6. Tiếp tục cho đến khi tất cả tìm kiếm kết thúc 

Sau khoảng`log(number_of_distinct_heights)`làm tròn, mọi truy vấn sẽ hội tụ về một giá trị độ cao. 

### 7. Xuất ra chiều cao tương ứng 

Vị trí tìm kiếm nhị phân cuối cùng xác định ngưỡng khả thi lớn nhất, đó chính xác là câu trả lời. 

### Tại sao nó hoạt động 

Sửa một ngưỡng$H$. Chiều rộng-`w`ít nhất là dấu hiệu của chiều cao$H$tồn tại bên trong`[l,r]`nếu và chỉ nếu có`w`các tấm liên tiếp trong khoảng đó có chiều cao ít nhất là$H$. 

Sau khi kích hoạt chính xác những tấm có chiều cao tối thiểu$H$, dấu hiệu như vậy tồn tại khi và chỉ nếu khoảng chứa một chuỗi hoạt động có độ dài ít nhất`w`. 

Cây phân đoạn tính toán lần chạy hoạt động dài nhất trong mỗi khoảng thời gian được truy vấn, do đó việc kiểm tra tính khả thi là chính xác. 

Tính khả thi là đơn điệu. Nếu một ngưỡng$H$là khả thi thì mọi ngưỡng nhỏ hơn cũng khả thi vì việc kích hoạt nhiều bảng hơn không thể phá hủy lượt chạy hợp lệ hiện có. Do đó, tìm kiếm nhị phân tìm thấy ngưỡng khả thi lớn nhất. Tìm kiếm nhị phân song song đánh giá đồng thời nhiều tìm kiếm như vậy trong khi sử dụng lại cùng một quá trình quét kích hoạt. Do đó, mọi giá trị được báo cáo chính xác là chiều cao ký hiệu tối đa có thể đạt được. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SegTree:
    def __init__(self, n):
        self.n = n
        self.pref = [0] * (4 * n)
        self.suff = [0] * (4 * n)
        self.best = [0] * (4 * n)
        self.length = [0] * (4 * n)
        self._build(1, 0, n - 1)

    def _build(self, v, tl, tr):
        self.length[v] = tr - tl + 1
        if tl == tr:
            return
        tm = (tl + tr) // 2
        self._build(v * 2, tl, tm)
        self._build(v * 2 + 1, tm + 1, tr)

    def _pull(self, v):
        lc = v * 2
        rc = lc + 1

        self.pref[v] = self.pref[lc]
        if self.pref[lc] == self.length[lc]:
            self.pref[v] += self.pref[rc]

        self.suff[v] = self.suff[rc]
        if self.suff[rc] == self.length[rc]:
            self.suff[v] += self.suff[lc]

        self.best[v] = max(
            self.best[lc],
            self.best[rc],
            self.suff[lc] + self.pref[rc]
        )

    def update(self, v, tl, tr, pos):
        if tl == tr:
            self.pref[v] = 1
            self.suff[v] = 1
            self.best[v] = 1
            return

        tm = (tl + tr) // 2
        if pos <= tm:
            self.update(v * 2, tl, tm, pos)
        else:
            self.update(v * 2 + 1, tm + 1, tr, pos)

        self._pull(v)

    def query(self, v, tl, tr, l, r):
        if l == tl and r == tr:
            return (
                self.pref[v],
                self.suff[v],
                self.best[v],
                self.length[v]
            )

        tm = (tl + tr) // 2

        if r <= tm:
            return self.query(v * 2, tl, tm, l, r)

        if l > tm:
            return self.query(v * 2 + 1, tm + 1, tr, l, r)

        left = self.query(v * 2, tl, tm, l, tm)
        right = self.query(v * 2 + 1, tm + 1, tr, tm + 1, r)

        lp, ls, lb, ll = left
        rp, rs, rb, rl = right

        pref = lp
        if lp == ll:
            pref += rp

        suff = rs
        if rs == rl:
            suff += ls

        best = max(lb, rb, ls + rp)

        return (pref, suff, best, ll + rl)

def solve():
    n = int(input())
    h = list(map(int, input().split()))

    m = int(input())

    queries = []
    for _ in range(m):
        l, r, w = map(int, input().split())
        queries.append((l - 1, r - 1, w))

    vals = sorted(set(h), reverse=True)
    k = len(vals)

    low = [0] * m
    high = [k - 1] * m

    positions = sorted(
        [(h[i], i) for i in range(n)],
        reverse=True
    )

    while True:
        buckets = [[] for _ in range(k)]
        active = False

        for i in range(m):
            if low[i] < high[i]:
                active = True
                mid = (low[i] + high[i]) // 2
                buckets[mid].append(i)

        if not active:
            break

        seg = SegTree(n)

        ptr = 0

        for mid in range(k):
            threshold = vals[mid]

            while ptr < n and positions[ptr][0] >= threshold:
                seg.update(1, 0, n - 1, positions[ptr][1])
                ptr += 1

            for qi in buckets[mid]:
                l, r, w = queries[qi]
                best = seg.query(1, 0, n - 1, l, r)[2]

                if best >= w:
                    high[qi] = mid
                else:
                    low[qi] = mid + 1

    ans = [str(vals[low[i]]) for i in range(m)]
    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Cây phân đoạn lưu trữ chính xác thông tin cần thiết để hợp nhất các lần chạy hoạt động liên tiếp. các`best`giá trị là phân đoạn hoạt động dài nhất trong khoảng thời gian của nút. Độ dài tiền tố và hậu tố cho phép các đường chạy qua điểm giữa được xây dựng lại. 

Tìm kiếm nhị phân được thực hiện trên các độ cao khác nhau thay vì trên toàn bộ phạm vi lên tới$10^9$. Điều này làm giảm độ sâu tìm kiếm xuống còn khoảng`log2(n)`. 

Một lỗi triển khai phổ biến là lấy ngược hướng tìm kiếm nhị phân. Mảng chiều cao`vals`được sắp xếp theo thứ tự giảm dần. Các chỉ số nhỏ hơn tương ứng với độ cao lớn hơn. Khi một ngưỡng khả thi, chúng ta tiếp tục tìm kiếm theo những độ cao lớn hơn, nghĩa là di chuyển giới hạn trên tới`mid`. 

Một điểm tinh tế khác là xây dựng lại cây phân đoạn sau mỗi vòng tìm kiếm nhị phân song song. Mỗi vòng đại diện cho một lần quét mới qua các ngưỡng, do đó tập hoạt động phải được xây dựng lại từ đầu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1 2 2 3 3
1
2 5 3
```Độ cao khác biệt là`[3,2,1]`. 

| Chiều cao trung bình | Vị trí hoạt động | Khoảng thời gian truy vấn | Chạy hoạt động dài nhất | Khả thi | 
| --- | --- | --- | --- | --- | 
| 2 | 2,3,4,5 | [2,5] | 4 | Có | 
| 3 | 4,5 | [2,5] | 2 | Không | 

Tìm kiếm nhị phân trước tiên xác minh chiều cao đó`2`hoạt động. Sau đó nó kiểm tra chiều cao`3`, không thành công vì chỉ có ít nhất hai tấm có chiều cao liên tiếp`3`. Câu trả lời là`2`. 

Điều này thể hiện tính chất đơn điệu. Một lần`3`thất bại, mọi ngưỡng lớn hơn cũng phải thất bại. 

### Ví dụ 2 

đầu vào:```
4
5 1 5 5
1
1 4 2
```Độ cao khác biệt là`[5,1]`. 

| Chiều cao trung bình | Vị trí hoạt động | Khoảng thời gian truy vấn | Chạy hoạt động dài nhất | Khả thi | 
| --- | --- | --- | --- | --- | 
| 5 | 1,3,4 | [1,4] | 2 | Có | 

Cuộc chạy tích cực`[3,4]`có chiều dài`2`, do đó, dấu hiệu có chiều rộng-2 có thể được đặt với chiều cao`5`. 

Ví dụ này cho thấy lý do tại sao chúng ta cần thời gian chạy liên tiếp dài nhất thay vì chỉ đếm các bảng đang hoạt động. Có ba bảng đang hoạt động, nhưng chỉ có hai bảng liền kề. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((n+m)\log^2 n)$| Về$\log n$các vòng tìm kiếm nhị phân song song, mỗi vòng thực hiện$O(n+m)$hoạt động cây phân đoạn | 
| Không gian |$O(n+m)$| Cây phân đoạn, truy vấn, trạng thái tìm kiếm nhị phân | 

Với$n,m \le 10^5$,$\log n$là khoảng 17. Số lượng thao tác thu được nằm trong giới hạn 4 giây và vừa vặn thoải mái trong giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    class SegTree:
        def __init__(self, n):
            self.n = n
            self.pref = [0] * (4 * n)
            self.suff = [0] * (4 * n)
            self.best = [0] * (4 * n)
            self.length = [0] * (4 * n)
            self.build(1, 0, n - 1)

        def build(self, v, l, r):
            self.length[v] = r - l + 1
            if l == r:
                return
            m = (l + r) // 2
            self.build(v * 2, l, m)
            self.build(v * 2 + 1, m + 1, r)

        def pull(self, v):
            lc = v * 2
            rc = lc + 1

            self.pref[v] = self.pref[lc]
            if self.pref[lc] == self.length[lc]:
                self.pref[v] += self.pref[rc]

            self.suff[v] = self.suff[rc]
            if self.suff[rc] == self.length[rc]:
                self.suff[v] += self.suff[lc]

            self.best[v] = max(
                self.best[lc],
                self.best[rc],
                self.suff[lc] + self.pref[rc]
            )

        def update(self, v, l, r, pos):
            if l == r:
                self.pref[v] = self.suff[v] = self.best[v] = 1
                return
            m = (l + r) // 2
            if pos <= m:
                self.update(v * 2, l, m, pos)
            else:
                self.update(v * 2 + 1, m + 1, r, pos)
            self.pull(v)

        def query(self, v, l, r, ql, qr):
            if ql == l and qr == r:
                return (self.pref[v], self.suff[v], self.best[v], self.length[v])

            m = (l + r) // 2

            if qr <= m:
                return self.query(v * 2, l, m, ql, qr)
            if ql > m:
                return self.query(v * 2 + 1, m + 1, r, ql, qr)

            A = self.query(v * 2, l, m, ql, m)
            B = self.query(v * 2 + 1, m + 1, r, m + 1, qr)

            ap, asf, ab, al = A
            bp, bsf, bb, bl = B

            pref = ap + bp if ap == al else ap
            suff = bsf + asf if bsf == bl else bsf
            best = max(ab, bb, asf + bp)

            return (pref, suff, best, al + bl)

    out = []

    n = int(input())
    h = list(map(int, input().split()))
    m = int(input())

    qs = [tuple(map(int, input().split())) for _ in range(m)]

    vals = sorted(set(h), reverse=True)
    k = len(vals)

    lo = [0] * m
    hi = [k - 1] * m

    pos = sorted([(h[i], i) for i in range(n)], reverse=True)

    while True:
        buckets = [[] for _ in range(k)]
        alive = False

        for i in range(m):
            if lo[i] < hi[i]:
                alive = True
                md = (lo[i] + hi[i]) // 2
                buckets[md].append(i)

        if not alive:
            break

        seg = SegTree(n)
        ptr = 0

        for md in range(k):
            thr = vals[md]

            while ptr < n and pos[ptr][0] >= thr:
                seg.update(1, 0, n - 1, pos[ptr][1])
                ptr += 1

            for qi in buckets[md]:
                l, r, w = qs[qi]
                best = seg.query(1, 0, n - 1, l - 1, r - 1)[2]

                if best >= w:
                    hi[qi] = md
                else:
                    lo[qi] = md + 1

    return "\n".join(str(vals[lo[i]]) for i in range(m))

# provided sample
assert run(
"""5
1 2 2 3 3
3
2 5 3
2 5 2
1 5 5
"""
) == "2\n3\n1"

# minimum size
assert run(
"""1
7
1
1 1 1
"""
) == "7"

# all equal
assert run(
"""4
5 5 5 5
2
1 4 2
2 3 1
"""
) == "5\n5"

# width equals interval length
assert run(
"""3
7 3 5
1
1 3 3
"""
) == "3"

# separated tall panels
assert run(
"""4
5 1 5 5
1
1 4 2
"""
) == "5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Bảng đơn | 7 | Trường hợp pháp lý nhỏ nhất | 
| Mọi chiều cao đều bằng nhau | 5, 5 | Hành vi có chiều cao đồng đều | 
| Chiều rộng bằng khoảng cách | 3 | Chỉ có một vị trí có thể | 
|`5 1 5 5`có chiều rộng 2 | 5 | Chạy logic liên tục thay vì đếm bảng hoạt động | 

## Vỏ cạnh 

Hãy xem xét:```
3
5 1 5
1
1 3 1
```Câu trả lời là`5`. Trong ngưỡng`5`, vị trí 1 và 3 đang hoạt động. Hoạt động lâu nhất bên trong`[1,3]`là`1`, đã thỏa mãn chiều rộng`1`. Tìm kiếm nhị phân trả về chính xác`5`. Một giải pháp sử dụng khoảng thời gian tối thiểu sẽ trả về không chính xác`1`. 

Coi như:```
4
4 4 1 4
1
1 4 3
```Đối với ngưỡng`4`, các lần chạy đang hoạt động có độ dài`2`Và`1`. Thời gian chạy dài nhất chỉ là`2`, nhỏ hơn chiều rộng yêu cầu`3`, do đó ngưỡng không thành công. Đối với ngưỡng`1`, tất cả các vị trí sẽ hoạt động và thời gian dài nhất sẽ trở thành`4`, vậy câu trả lời là`1`. Thuật toán thực thi chính xác yêu cầu về chiều rộng cố định. 

Coi như:```
3
7 3 5
1
1 3 3
```Ở ngưỡng`5`, các vị trí hoạt động là`{1,3}`và thời gian dài nhất là`1`, do đó tính khả thi không thành công. Ở ngưỡng`3`, mọi vị trí đều hoạt động và thời gian chạy dài nhất sẽ trở thành`3`, khớp chính xác với chiều rộng yêu cầu. Câu trả lời là`3`, đó là chiều cao tối thiểu của toàn bộ khoảng.
