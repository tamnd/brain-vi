---
title: "CF 2046C - Nhà thám hiểm"
description: "Mỗi thành phố là một điểm trên mặt phẳng. Chúng ta chọn một điểm phân chia $(x0,y0)$, điểm này chia mặt phẳng thành bốn vùng: $$begin{aligned} &x ge x0, y ge y0 &x < x0, y ge y0 &x ge x0, y < y0 &x < x0, y < y0 end{aligned}$$ Mỗi thành phố thuộc về chính xác một trong các vùng này, theo…"
date: "2026-06-08T09:08:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "greedy", "sortings", "ternary-search", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 2046
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 990 (Div. 1)"
rating: 2100
weight: 2046
solve_time_s: 199
verified: false
draft: false
---

[CF 2046C - Nhà thám hiểm](https://codeforces.com/problemset/problem/2046/C) 

**Xếp hạng:** 2100 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, tham lam, sắp xếp, tìm kiếm ba ngôi, hai con trỏ 
**Thời gian giải:** 3m 19s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Mỗi thành phố là một điểm trên mặt phẳng. Chúng tôi chọn điểm phân chia$(x_0,y_0)$, chia mặt phẳng thành bốn vùng:$$\begin{aligned} &x \ge x_0,\ y \ge y_0 \\ &x < x_0,\ y \ge y_0 \\ &x \ge x_0,\ y < y_0 \\ &x < x_0,\ y < y_0 \end{aligned}$$Mỗi thành phố thuộc về chính xác một trong những khu vực này, theo sự bất bình đẳng ở trên. 

Đối với điểm phân chia đã chọn, hãy tính bốn vùng$c_1,c_2,c_3,c_4$. Chúng tôi muốn tối đa hóa$$\min(c_1,c_2,c_3,c_4).$$Chúng ta phải xuất ra cả giá trị tối đa có thể đạt được và một điểm chia để đạt được giá trị đó. 

Tổng số điểm của tất cả các ca kiểm thử nhiều nhất là$10^5$. Bất kỳ giải pháp nào kiểm tra từng cặp ứng cử viên$x_0$Và$y_0$các giá trị sẽ yêu cầu$O(n^2)$hoặc tệ hơn là công việc trên mỗi trường hợp thử nghiệm là quá chậm. Chúng ta cần thứ gì đó xung quanh$O(n \log^2 n)$. 

Một điểm tinh tế là sự bất bình đẳng là không đối xứng. Thành phố có$x=x_0$thuộc về nửa bên phải và các thành phố có$y=y_0$thuộc về nửa trên. Một giải pháp vô tình coi việc phân chia là nghiêm ngặt ở cả hai bên sẽ tạo ra số lượng góc phần tư không chính xác. 

Một trường hợp phức tạp khác xảy ra khi nhiều thành phố có chung$x$-điều phối. Trong khi quét sạch sự phân chia theo chiều dọc có thể xảy ra, tất cả các thành phố có cùng$x$phải di chuyển cùng nhau. Việc chia tách bên trong một nhóm như vậy sẽ làm thay đổi ý nghĩa của bất đẳng thức và có thể tạo ra các trạng thái không hợp lệ. 

## Phương pháp tiếp cận 

Một giải pháp vũ phu sẽ thử mọi cách có thể$x_0$, mọi khả năng$y_0$, đếm bốn góc phần tư và giữ lại câu trả lời đúng nhất. Ngay cả khi chúng tôi hạn chế các ứng viên tọa độ xuất hiện trong đầu vào, điều này vẫn yêu cầu$O(n^2)$cặp ứng cử viên và$O(n)$làm việc để đánh giá từng người, dẫn đến$O(n^3)$. 

Quan sát quan trọng là chúng ta không cần tìm kiếm trực tiếp kích thước góc phần tư tối thiểu tốt nhất. Thay vào đó, giả sử chúng ta hỏi một câu hỏi đơn giản hơn: 

> Có thể làm cho mỗi góc phần tư chứa ít nhất$k$thành phố? 

Nếu chúng ta có thể trả lời câu hỏi này một cách hiệu quả thì câu trả lời cuối cùng có thể được tìm thấy bằng tìm kiếm nhị phân trên$k$. 

Đối với một cố định$k$, hãy tưởng tượng quét ranh giới dọc từ trái sang phải. Tại bất kỳ thời điểm nào, các thành phố được chia thành hai nhóm:$$L = \{x_i < x_0\}, \qquad R = \{x_i \ge x_0\}.$$Đối với ranh giới ngang được chọn$y_0$, chúng tôi cần$$\begin{aligned} \text{left-below} &\ge k,\\ \text{left-above} &\ge k,\\ \text{right-below} &\ge k,\\ \text{right-above} &\ge k. \end{aligned}$$Cho phép$P_L(y)$Và$P_R(y)$là số thành phố ở$L$Và$R$của ai$y$-tọa độ hoàn toàn nhỏ hơn$y$. 

Khi đó điều kiện trở thành$$k \le P_L(y_0) \le |L|-k,$$Và$$k \le P_R(y_0) \le |R|-k.$$Đối với mỗi bên, các giá trị hợp lệ của$y_0$tạo thành một khoảng liền kề vì số tiền tố là đơn điệu. Chúng ta chỉ cần xác định liệu khoảng thời gian cho$L$cắt khoảng cho$R$. 

Cây Fenwick cho phép chúng ta duy trì số lượng tiền tố trong khi quét$x$và cũng để tìm vị trí đầu tiên mà tiền tố đạt đến một giá trị nhất định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^3)$|$O(1)$| Quá chậm | 
| Tìm kiếm nhị phân + Quét + Fenwick |$O(n \log^2 n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Nén tọa độ 

Mọi thao tác chỉ phụ thuộc vào thứ tự tương đối của$y$-tọa độ. 

Nén tất cả khác biệt$y$-giá trị thành chỉ số$1 \ldots m$. 

### Kiểm tra tính khả thi của bản sửa lỗi$k$1. Đặt mọi điểm vào đúng cấu trúc$R$. 
2. Cấu trúc bên trái$L$bắt đầu trống rỗng. 
3. Quét rõ ràng$x$- tọa độ theo thứ tự tăng dần. 
4. Trước khi xử lý nhóm với tọa độ$x$, sự phân chia tương ứng với$x_0=x$. Mỗi điểm nhỏ hơn$x$đã ở trong rồi$L$, và mọi điểm với$x\ge x_0$vẫn còn trong$R$. 
5. Hãy để$tot_L$Và$tot_R$là kích thước hiện tại của hai bên. 
6. Nếu một trong hai bên có ít hơn$2k$điểm, bên đó không bao giờ có thể đóng góp ít nhất$k$điểm cả trên và dưới đường ngang. Bỏ qua vị trí quét này. 
7. Đối với$L$, tìm khoảng nén$y$-vị trí ở đâu$$k \le P_L \le tot_L-k.$$Vì số tiền tố là đơn điệu nên khoảng này có thể thu được bằng các truy vấn thống kê thứ tự Fenwick. 

1. Tính khoảng tương ứng cho$R$. 
2. Nếu hai khoảng giao nhau thì một giá trị hợp lệ$y_0$tồn tại và câu trả lời cho việc kiểm tra này là "có thể". 
3. Di chuyển toàn bộ dòng điện$x$-nhóm từ$R$vào trong$L$và tiếp tục cuộc càn quét. 

### Tìm kiếm nhị phân 

1. Câu trả lời không được vượt quá$\lfloor n/4 \rfloor$. 
2. Tìm kiếm nhị phân$k$trong phạm vi$[0,\lfloor n/4 \rfloor]$. 
3. Chạy kiểm tra tính khả thi. 
4. Giữ mức độ khả thi lớn nhất$k$. 
5. Chạy kiểm tra một lần nữa cho giá trị đó và lưu giá trị tương ứng$(x_0,y_0)$. 

### Tại sao nó hoạt động 

Đối với phân chia theo chiều dọc cố định, mọi phân chia theo chiều ngang hợp lệ phải đáp ứng hai ràng buộc độc lập:$$k \le P_L(y_0) \le |L|-k,$$Và$$k \le P_R(y_0) \le |R|-k.$$Vì cả hai hàm tiền tố đều đơn điệu nên mỗi ràng buộc xác định một khoảng liền kề của các khả năng có thể.$y_0$-vị trí. Sự phân chia theo chiều ngang tồn tại chính xác khi các khoảng đó giao nhau. 

Quá trình quét liệt kê mọi phân chia theo chiều dọc có ý nghĩa vì việc phân loại chỉ thay đổi khi chúng ta vượt qua một phân chia riêng biệt.$x$-điều phối. Tìm kiếm nhị phân là hợp lệ vì tính khả thi là đơn điệu: nếu mọi góc phần tư có thể chứa ít nhất$k$thành phố thì mọi giá trị nhỏ hơn cũng có thể đạt được. 

## Giải pháp Python```python
import sys
from bisect import bisect_left

input = sys.stdin.readline

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 1)

    def add(self, idx, val):
        n = self.n
        bit = self.bit
        while idx <= n:
            bit[idx] += val
            idx += idx & -idx

    def sum(self, idx):
        res = 0
        bit = self.bit
        while idx:
            res += bit[idx]
            idx -= idx & -idx
        return res

    def kth(self, k):
        idx = 0
        bitmask = 1 << (self.n.bit_length() - 1)
        while bitmask:
            nxt = idx + bitmask
            if nxt <= self.n and self.bit[nxt] < k:
                k -= self.bit[nxt]
                idx = nxt
            bitmask >>= 1
        return idx + 1

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        pts = [tuple(map(int, input().split())) for _ in range(n)]

        ys = sorted(set(y for _, y in pts))
        m = len(ys)

        data = []
        for x, y in pts:
            data.append((x, bisect_left(ys, y) + 1))

        data.sort()

        groups = []
        i = 0
        while i < n:
            j = i
            while j < n and data[j][0] == data[i][0]:
                j += 1
            groups.append((data[i][0], data[i:j]))
            i = j

        def get_interval(ft, total, k):
            if total < 2 * k:
                return None

            left = ft.kth(k)

            limit = total - k
            if limit == total:
                right = m
            else:
                pos = ft.kth(limit + 1)
                right = pos - 1

            if left > right:
                return None
            return (left, right)

        def check(k):
            L = Fenwick(m)
            R = Fenwick(m)

            for _, y in data:
                R.add(y, 1)

            left_cnt = 0
            right_cnt = n

            for x0, grp in groups:
                A = get_interval(L, left_cnt, k)
                B = get_interval(R, right_cnt, k)

                if A is not None and B is not None:
                    l = max(A[0], B[0])
                    r = min(A[1], B[1])

                    if l <= r:
                        y0 = ys[l - 1]
                        return True, x0, y0

                for _, y in grp:
                    R.add(y, -1)
                    L.add(y, 1)

                sz = len(grp)
                left_cnt += sz
                right_cnt -= sz

            return False, 0, 0

        lo, hi = 0, n // 4
        best = 0

        while lo <= hi:
            mid = (lo + hi) // 2
            ok, _, _ = check(mid)

            if ok:
                best = mid
                lo = mid + 1
            else:
                hi = mid - 1

        _, x0, y0 = check(best)

        out.append(str(best))
        out.append(f"{x0} {y0}")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log^2 n)$| Tìm kiếm nhị phân bật$k$, mỗi lần kiểm tra tính khả thi sẽ thực hiện quét các truy vấn thống kê thứ tự Fenwick | 
| Không gian |$O(n)$| Tọa độ nén, điểm được nhóm và cây Fenwick | 

Tổng số điểm trên tất cả các trường hợp thử nghiệm nhiều nhất là$10^5$, Vì thế$O(n \log^2 n)$dễ dàng phù hợp trong giới hạn. Việc sử dụng bộ nhớ là tuyến tính theo số điểm.
