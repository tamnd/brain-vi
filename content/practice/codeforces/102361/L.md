---
title: "CF 102361L - MUV LUV THAY THẾ"
description: "Cabin là một lưới hình chữ nhật, nhưng hầu hết các ô đều không hữu ích như nhau. Có hai hành lang dọc, mỗi hành lang rộng một ô. Hành lang đầu tiên nằm ngay sau khu vực ngồi bên trái và hành lang thứ hai nằm ngay sau khu vực ngồi giữa."
date: "2026-08-13T00:22:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102361
codeforces_index: "L"
codeforces_contest_name: "2019 China Collegiate Programming Contest Qinhuangdao Onsite"
rating: 0
weight: 102361
solve_time_s: 284
verified: true
draft: false
---

[CF 102361L - MUV LUV THAY THẾ](https://codeforces.com/problemset/problem/102361/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4 phút 44 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Cabin là một lưới hình chữ nhật, nhưng hầu hết các ô đều không hữu ích như nhau. Có hai hành lang dọc, mỗi hành lang rộng một ô. Hành lang đầu tiên nằm ngay sau khu vực ngồi bên trái và hành lang thứ hai nằm ngay sau khu vực ngồi giữa. Chỉ các ô hành lang mới cho phép di chuyển theo chiều dọc. Mỗi người lính bắt đầu ở một trong ba khu vực ngồi, nơi ban đầu chuyển động theo chiều ngang. 

Người lính từ khu vực bên trái cuối cùng phải sử dụng hành lang bên trái và rời khỏi lối ra đó. Người lính từ khu vực bên phải phải sử dụng hành lang bên phải. Người lính từ khu vực giữa có thể chọn một trong hai hành lang. Khi một người lính đến một hành lang, nó có thể di chuyển xuống một hàng trong một đơn vị thời gian và cuối cùng đến được lối ra tương ứng. 

Đầu vào cung cấp số hàng, chiều rộng của ba khu vực ngồi và số lượng binh sĩ. Mỗi người lính được mô tả theo khu vực cũng như hàng và cột đầu tiên của nó. Đầu ra được yêu cầu là số nguyên thời gian tối thiểu mà mọi người lính có thể ở bên ngoài cabin. 

Giá trị tham số lớn nhất là khoảng (10^9), trong khi số lượng binh lính có thể đạt tới (10^5). Chúng ta không thể xây dựng lưới, mô phỏng từng ô hoặc mô phỏng từng bước chuyển động. Ngay cả một mô phỏng xử lý mỗi người lính một lần trong một khoảnh khắc cũng có thể yêu cầu khoảng (10^{14}) thông tin cập nhật về người lính trên một phiên bản lớn. Giải pháp phải phụ thuộc vào bản ghi đầu vào (10^5) thay vì dựa trên hình học có kích thước (10^9). 

Có một số trường hợp ranh giới bộc lộ sai sót trong mô hình. Với một người lính trong cabin nhỏ nhất có thể, câu trả lời không phải là một vì người lính bắt đầu trong phòng giam có chỗ ngồi và trước tiên phải đi vào hành lang. Ví dụ, với`1 1 1 1 1`và người lính ở`(1,1)`, hành lang bên trái là cột 2 nên quân lính cần một nước đi ngang và một nước đi xuống. Câu trả lời là`2`. Giải pháp xử lý lối ra hành lang như thể nó nằm liền kề theo chiều ngang với ô bắt đầu sẽ quay trở lại không chính xác`1`. 

Một trường hợp khó phát hiện khác là khi nhiều người lính có thời gian xuất cảnh sớm nhất như nhau. Ví dụ, với`1 1 1 1 3`, đưa lính vào`(1,1)`,`(1,3)`, Và`(1,5)`, lần lượt thuộc các vùng bên trái, giữa và bên phải. Mỗi người lính có thể đến hành lang đã chọn và thoát ra theo hai bước riêng lẻ, nhưng ba người lính không thể cùng sử dụng hành lang của mình với thời gian thoát ra là hai khi người lính ở giữa chọn một bên. Hai người lính phải đi chung một hành lang nên một trong số họ cần có thời gian thoát ra muộn hơn. Câu trả lời đúng là`3`. Xử lý độc lập con đường ngắn nhất của mỗi người lính sẽ đưa ra câu trả lời sai`2`. 

Sai lầm phổ biến thứ ba là quên rằng người lính có thể chờ đợi. Hai người lính tiếp cận cùng một hành lang từ hai phía đối diện có thể tránh va chạm bằng cách trì hoãn một trong số họ trước khi tiến vào hành lang. Ràng buộc liên quan không phải là thời gian tham gia của họ phải khác biệt mà là thời gian cuối cùng của họ đi qua hành lang đó phải khác biệt. 

## Phương pháp tiếp cận 

Lực lượng vũ phu trực tiếp nhất là quyết định lối ra nào mà mỗi người lính ở khu vực giữa sử dụng, sau đó giải quyết vấn đề di chuyển cho hai nhóm kết quả. Nếu có (các) lính khu vực giữa thì có (2^s) nhiệm vụ. Với (s=100000), điều này có nghĩa là (2^{100000}) bài tập khác nhau trước khi xem xét chi phí kiểm tra dù chỉ một bài tập. Một mô phỏng theo nghĩa đen hơn cũng là vô vọng. Nếu mỗi khoảnh khắc quét tất cả (10^5) binh sĩ và việc sơ tán diễn ra theo thứ tự (10^9) khoảnh khắc thì mô phỏng sẽ thực hiện xung quanh (10^{14}) thông tin cập nhật về binh lính. 

Quan sát quan trọng là ngừng suy nghĩ về toàn bộ hành lang và thay vào đó hãy mô tả một người lính vào thời điểm nó rời khỏi lối ra. Giả sử một người lính bước vào hành lang vào thời điểm (s), bắt đầu từ hàng (x). Thời gian thoát của nó là (s+x). Nếu chúng ta quy định thời gian thoát của nó là (e) thì đường đi của nó bên trong hành lang được xác định hoàn toàn: tại thời điểm (t), hàng của nó là (e-t). Hai người lính sử dụng cùng một hành lang va chạm vào nhau khi họ có cùng thời gian thoát ra. Do đó, yêu cầu duy nhất bên trong một hành lang là tất cả binh lính được phân công đều nhận được thời gian thoát là số nguyên riêng biệt. 

Đối với một người lính, gọi (p_L) là thời gian sớm nhất có thể thoát ra qua lối ra bên trái và (p_R) là thời gian sớm nhất có thể thoát ra qua lối ra bên phải. Một người lính được chỉ định ở hành lang bên trái có thể sử dụng bất kỳ số nguyên nào thời gian thoát ra trong khoảng ([p_L,T]), trong đó (T) là thời gian cuối cùng được đề xuất. Điều tương tự cũng xảy ra với hành lang bên phải. 

Điều này biến bài toán hình học thành bài toán so khớp. Có (T) khoảng thời gian thoát có thể có trên mỗi hành lang. Một người lính được kết nối với hậu tố của các ô bên trái và hậu tố của các ô bên phải. Đảo ngược thứ tự thời gian biến những hậu tố đó thành tiền tố, điều này mang lại một dạng điều kiện Hall đặc biệt thuận tiện. 

Đối với (T) cố định, hãy xác định 

[ 
d_L=\max(0,T-p_L+1), \qquad d_R=\max(0,T-p_R+1). 
] 

Giá trị (d_L) là số khe thời gian đảo ngược dành cho người lính ở hành lang bên trái. Giá trị bằng 0 nghĩa là người lính không thể sử dụng hành lang đó theo thời gian (T). 

Hãy xem xét lấy (a) khe thời gian đảo ngược đầu tiên ở bên trái và (b) đầu tiên ở bên phải. Một người lính bị buộc phải vào tập hợp này một cách chính xác khi (d_L\le a) và (d_R\le b). Những người lính như vậy đều phải vừa với các ô (a+b). Do đó điều kiện khả thi là 

[ 
#{i:d_{L,i}\le a,\ d_{R,i}\le b}\le a+b 
] 

với mọi (a,b). 

Bởi vì các vị trí có sẵn của mỗi người lính đều là tiền tố nên việc kiểm tra các cặp tiền tố này là đủ theo định lý Hall. Chúng ta có thể quét (a) từ nhỏ đến lớn. Khi một người lính bắt đầu hoạt động, nó đóng góp một phần vào mọi (b) thỏa mãn (b\ge d_R). Như vậy số lượng 

[ 
#{d_R\le b}-b 
] 

nhận được một bổ sung phạm vi trên một hậu tố. Cây phân đoạn có thể duy trì mức tối đa của nó. 

Có một phép biến đổi hữu ích giúp tránh việc xây dựng rõ ràng tất cả các giá trị (d_R) cho mỗi lần lặp tìm kiếm nhị phân. đặt 

[ 
q=T+1-b. 
] 

Khi đó (d_R\le b) tương đương với (p_R\ge q), kể cả trường hợp người lính không thể sử dụng hành lang bên phải bằng cách coi thời gian sớm nhất bên phải của nó là vô cùng. Điều kiện Hall trở thành 

[ 
q+#{p_R\ge q}\le T+1+a. 
] 

Đối với mỗi người lính tại ngũ, việc chèn nó sẽ thêm một vào mỗi ngưỡng (q\le p_R). Chúng tôi duy trì giá trị tối đa của (q+\text{count}(p_R\ge q)) với cây phân đoạn hỗ trợ bổ sung phạm vi tiền tố. 

Thuật toán kết quả thực hiện tìm kiếm nhị phân trên (T). Mỗi lần kiểm tra tính khả thi cần (O(k\log k)), do đó độ phức tạp hoàn toàn là (O(k\log k\log C)), trong đó (C) là phạm vi số của câu trả lời. Với (C) xung quanh (10^9), hệ số logarit bổ sung chỉ khoảng 31.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force qua nhiệm vụ giữa | (O(2^k\cdot k\log k)) | (O(k)) | Quá chậm | 
| Mô phỏng bước thời gian | (O(kT)) | (O(k)) | Quá chậm | 
| Kiểm tra điều kiện Hall tối ưu bằng tìm kiếm nhị phân | (O(k\log k\log C)) | (O(k)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi từng người lính thành thời gian thoát ra sớm nhất có thể cho mỗi hành lang. Đặt hành lang bên trái là cột (c_1=l_1+1) và hành lang bên phải là cột (c_2=l_1+l_2+2). 

Lính bên trái chỉ có giá trị bên trái 

[ 
p_L=x+c_1-y. 
] 

Lính bên phải chỉ có giá trị phù hợp 

[ 
p_R=x+y-c_2. 
] 

Một người lính ở khu vực giữa có cả hai 

[ 
p_L=x+y-c_1, 
\qquad 
p_R=x+c_2-y. 
] 

Đối với hành lang mà người lính bị cấm sử dụng, hãy lưu trữ một giá trị trọng điểm rất lớn. 
2. Cho trước thời gian sơ tán của ứng viên (T), hãy chuyển đổi mỗi thời gian thoát sớm nhất thành thời hạn theo thời gian đảo ngược. 

Trong thời gian sớm nhất hữu hạn (p), số lượng khe thời gian đảo ngược có thể sử dụng là 

[ 
d=\max(0,T-p+1). 
] 

Một người lính có thể sử dụng hành lang khi và chỉ khi (d>0). 
3. Sử dụng định lý Hall để mô tả tính khả thi. Đối với bất kỳ (a) ô bên trái và (b) ô bên phải nào khi bắt đầu thời gian đảo ngược, mọi người lính thỏa mãn (d_L\le a) và (d_R\le b) đều có vùng lân cận hoàn chỉnh bên trong các ô (a+b) đó. Vì vậy những người lính như vậy không được lớn hơn (a+b). 

Vì các vùng lân cận là tiền tố nên các cặp tiền tố này đủ để kiểm tra sự so khớp lưỡng cực hoàn chỉnh. 
4. Quét giá trị (a) từ nhỏ đến lớn. Khi (a) đạt tới (d_L của một người lính), người lính đó sẽ hoạt động. Đối với tập hoạt động hiện tại, chúng ta cần 

[ 
\max_b\left(#{d_R\le b}-b\right)\le a. 
] 

Mỗi người lính mới hoạt động với đúng thời hạn (d_R) sẽ tăng số lượng cho mỗi (b\ge d_R). Đây chính xác là một bổ sung phạm vi hậu tố. 
5. Viết lại biểu thức bên phải bằng cách sử dụng (q=T+1-b). Điều kiện trở thành 

[ 
\max_q\left(q+#{p_R\ge q}\right)\le T+1+a. 
] 

Một người lính mới hoạt động với thời gian sớm nhất (p_R) sẽ tăng giá trị duy trì cho mỗi (q\le p_R), vì vậy chúng ta cần bổ sung phạm vi tiền tố và truy vấn tối đa toàn cầu. 

Chỉ các ngưỡng bằng với hữu hạn hiện có (p_R), cùng với (T+1), mới có thể phù hợp. Giữa hai ngưỡng liên tiếp, số đếm không đổi trong khi (q) tăng, do đó điểm cuối lớn nhất luôn là đủ. 
6. Sắp xếp binh lính theo thứ tự giảm dần (p_L). Vì (T) cố định nên điều này giống như quá trình xử lý tăng dần (d_L). Những binh sĩ có (p_L>T), bao gồm cả những binh sĩ bị cấm ở hành lang bên trái, đều có (d_L=0) và được đưa vào trước. Sau đó, các giá trị hữu hạn (p_L) bằng nhau được xử lý thành một nhóm. 
7. Đối với mỗi nhóm đang hoạt động, hãy cập nhật cây phân đoạn theo ngưỡng bên phải của nó. Nếu (p_R>T), người lính không thể sử dụng hành lang bên phải, do đó, nó đóng góp vào mọi ngưỡng hợp lệ lên tới (T+1). Ngược lại, nó đóng góp vào tất cả các ngưỡng (q\le p_R). 
8. Sau mỗi nhóm, so sánh giá trị lớn nhất của cây phân đoạn với (T+1+a). Nếu cực đại lớn hơn thì điều kiện Hall bị vi phạm và (T) là không thể. 
9. Đạt được giới hạn trên bằng cách phân công từng binh sĩ khu vực giữa vào hành lang bên trái, lên lịch cho tất cả binh sĩ khu vực bên trái và khu vực giữa cùng nhau và lên lịch độc lập cho các binh sĩ khu vực bên phải. Đối với một hành lang, hãy sắp xếp tất cả thời gian thoát sớm nhất và ấn định thời gian thoát riêng biệt sớm nhất có thể. Tìm kiếm nhị phân giữa số 0 và giới hạn trên khả thi được đảm bảo này. 

### Tại sao nó hoạt động 

Bất biến trung tâm là lịch trình hành lang được đặc trưng hoàn toàn bởi thời gian thoát là số nguyên riêng biệt. Một người lính có thời gian thoát sớm nhất có thể (p) có thể nhận được chính xác bất kỳ thời gian thoát nào ít nhất (p), do đó, đối với một (T) cố định, các vị trí có thể có của nó tạo thành một hậu tố. Đảo ngược thời gian biến mọi vị trí có thể được đặt thành tiền tố.

Định lý Hall nói rằng sự trùng khớp tồn tại chính xác khi mỗi bộ khe chứa đủ sức chứa cho tất cả những người lính có toàn bộ vùng lân cận nằm bên trong nó. Bởi vì mỗi vùng lân cận là sự kết hợp của hai tiền tố, nên các tập hợp vị trí có khả năng chặt chẽ có thể được biểu diễn bằng cách lấy tiền tố của hành lang bên trái và tiền tố của hành lang bên phải. Cây phân đoạn sẽ kiểm tra đồng thời tất cả các cặp như vậy. 

Tìm kiếm nhị phân là hợp lệ vì nếu có thể sơ tán trong thời điểm (T), thì vẫn có thể thực hiện được trong bất kỳ số lượng thời điểm lớn hơn nào. Do đó, vị từ khả thi là đơn điệu và khả thi nhỏ nhất (T) chính xác là câu trả lời được yêu cầu. 

## Giải pháp Python```python
import sys
from bisect import bisect_right

input = sys.stdin.readline

INF = 10**30
NEG = -10**30

class SegTree:
    def __init__(self, values):
        n = len(values)
        size = 1
        while size < n:
            size <<= 1

        self.size = size
        self.mx = [NEG] * (size << 1)
        self.lazy = [0] * (size << 1)

        base = size
        for i, v in enumerate(values):
            self.mx[base + i] = v

        for i in range(size - 1, 0, -1):
            self.mx[i] = max(self.mx[i << 1], self.mx[i << 1 | 1])

    def _add(self, node, left, right, ql, qr, value):
        if ql <= left and right <= qr:
            self.mx[node] += value
            self.lazy[node] += value
            return

        mid = (left + right) >> 1

        if ql <= mid:
            self._add(node << 1, left, mid, ql, qr, value)
        if mid < qr:
            self._add(node << 1 | 1, mid + 1, right, ql, qr, value)

        self.mx[node] = (
            self.lazy[node]
            + max(self.mx[node << 1], self.mx[node << 1 | 1])
        )

    def add_prefix(self, right, value):
        if right < 0:
            return
        if right >= self.size:
            right = self.size - 1
        self._add(1, 0, self.size - 1, 0, right, value)

    @property
    def maximum(self):
        return self.mx[1]

def corridor_cost(values):
    if not values:
        return 0

    values.sort()

    current = 0
    for p in values:
        current = max(p, current + 1)

    return current

def solve():
    n, l1, l2, l3, k = map(int, input().split())

    c1 = l1 + 1
    c2 = l1 + l2 + 2

    jobs = []
    left_fixed = []
    right_fixed = []
    middle = []

    for _ in range(k):
        a, x, y = map(int, input().split())

        if a == 1:
            p_l = x + c1 - y
            p_r = INF
            left_fixed.append(p_l)
        elif a == 2:
            p_l = x + y - c1
            p_r = x + c2 - y
            middle.append((p_l, p_r))
        else:
            p_l = INF
            p_r = x + y - c2
            right_fixed.append(p_r)

        jobs.append((p_l, p_r))

    # A guaranteed feasible solution:
    # send every middle-zone soldier to the left corridor.
    upper = max(
        corridor_cost(left_fixed + [p_l for p_l, _ in middle]),
        corridor_cost(right_fixed[:])
    )

    finite_right = sorted({p_r for _, p_r in jobs if p_r < INF})
    right_rank = {v: i for i, v in enumerate(finite_right)}

    jobs_by_left = sorted(jobs, key=lambda z: z[0], reverse=True)

    def feasible(T):
        # Only p_R values <= T are useful coordinates.
        m = bisect_right(finite_right, T)

        # q values are all useful p_R thresholds plus q = T + 1.
        coords = finite_right[:m]
        coords.append(T + 1)

        seg = SegTree(coords)

        idx = 0
        total = len(jobs_by_left)

        # All jobs with p_L > T, and all jobs forbidden from the
        # left corridor, have d_L = 0.
        while idx < total and jobs_by_left[idx][0] > T:
            p_r = jobs_by_left[idx][1]

            if p_r >= INF or p_r > T:
                seg.add_prefix(m, 1)
            else:
                seg.add_prefix(right_rank[p_r], 1)

            idx += 1

        # At a = 0, all d_L = 0 jobs are active.
        if seg.maximum > T + 1:
            return False

        # For finite p_L <= T, equal p_L values have the same d_L.
        while idx < total:
            p_l = jobs_by_left[idx][0]
            a = T - p_l + 1

            j = idx
            while j < total and jobs_by_left[j][0] == p_l:
                p_r = jobs_by_left[j][1]

                if p_r >= INF or p_r > T:
                    seg.add_prefix(m, 1)
                else:
                    seg.add_prefix(right_rank[p_r], 1)

                j += 1

            if seg.maximum > T + 1 + a:
                return False

            idx = j

        return True

    lo = 0
    hi = upper

    while lo < hi:
        mid = (lo + hi) >> 1
        if feasible(mid):
            hi = mid
        else:
            lo = mid + 1

    print(lo)

if __name__ == "__main__":
    solve()
```Phần đầu tiên của quá trình triển khai sẽ chuyển đổi tọa độ hình học thành hai thời điểm thoát sớm nhất. Các cột hành lang được`c1 = l1 + 1`Và`c2 = l1 + l2 + 2`, do đó khoảng cách theo chiều ngang được lấy trực tiếp từ hiệu cột.`INF`đại diện cho một lối ra mà người lính không được phép sử dụng. Nó lớn hơn nhiều so với mọi thời gian thực có thể có, vì vậy, trong quá trình kiểm tra tính khả thi, một người lính như vậy sẽ không nhận được chỗ trống nào trên hành lang đó. 

các`corridor_cost`hàm chỉ được sử dụng cho giới hạn trên ban đầu. Nếu thời gian thoát sớm nhất trên một hành lang được sắp xếp theo (p_1\le p_2\le\cdots), thì thời gian thoát sớm nhất có thể thu được bằng cách lấy liên tục`max(p_i, previous + 1)`. Điều này cũng chứng tỏ tại sao các va chạm bên trong một hành lang lại giảm được thời gian thoát ra rõ rệt. 

Cây phân đoạn lưu trữ các giá trị có dạng 

[ 
q+#{p_R\ge q}. 
] 

Lá của nó bắt đầu bằng ngưỡng`q`. Kích hoạt một người lính sẽ thêm một người vào mỗi ngưỡng không lớn hơn ngưỡng đó`p_R`, đây chính xác là phép cộng phạm vi tiền tố. Thư mục gốc lưu trữ mức tối đa trên tất cả các ngưỡng. 

Ngưỡng đặc biệt`T + 1`đại diện cho (b=0), nghĩa là không có sẵn khe thời gian đảo ngược hành lang bên phải. Nó cần thiết cho những người lính bị buộc phải di chuyển sang hành lang bên trái. Giá trị của`p_R`lớn hơn`T`có tác dụng tương tự nên chúng cập nhật toàn bộ phạm vi ngưỡng hợp lệ. 

Thứ tự quét đang giảm dần`p_L`. Kể từ khi 

[ 
d_L=\max(0,T-p_L+1), 
] 

giảm dần`p_L`chính xác là đang tăng lên`d_L`. Tất cả các giá trị với`p_L>T`được xử lý cùng nhau tại (a=0). hữu hạn bằng nhau`p_L`các giá trị cũng được xử lý cùng nhau vì chúng có cùng tham số Hall (a). 

Số nguyên Python có độ chính xác tùy ý, do đó tọa độ có kích thước (10^9) và tất cả thời gian lập lịch tích lũy đều an toàn. Cái lớn`INF`Và`NEG`các giá trị nằm ngoài phạm vi câu trả lời có thể có và chỉ đóng vai trò là lính canh. 

## Ví dụ đã hoạt động 

Đối với mẫu được cung cấp, hai cột hành lang là (3) và (6). 

Hai lính bên trái có thời gian ra trái sớm nhất (4) và (3). Lính ở giữa có thời gian thoát ra bên trái sớm nhất (3) và thời gian thoát ra bên phải sớm nhất (4). 

| Người Lính | Khu | (p_L) | (p_R) | 
| --- | --- | --- | --- | 
| A | 1 | 4 | INF | 
| B | 1 | 3 | INF | 
| C | 2 | 3 | 4 | 

Với (T=3), thời hạn đảo ngược là 

| Người Lính | (d_L) | (d_R) | 
| --- | --- | --- | 
| A | 0 | 0 | 
| B | 1 | 0 | 
| C | 1 | 0 | 

Lấy (a=1,b=0), cả ba binh sĩ đều thỏa mãn (d_L\le1) và (d_R\le0) nên tất cả sẽ phải nhét vừa vào một ô bên trái. Bất đẳng thức Hall trở thành (3\le1), không thành công. 

Với (T=4), thời hạn trở thành 

| Người Lính | (d_L) | (d_R) | 
| --- | --- | --- | 
| A | 1 | 0 | 
| B | 2 | 0 | 
| C | 2 | 1 | 

Các điều kiện chặt chẽ hiện đã được thỏa mãn. Ví dụ: (a=2,b=1) chứa cả ba người lính và có sẵn ba vị trí, cho ra (3\le3). Do đó (T=4) là khả thi, trong khi (T=3) là không khả thi, vì vậy câu trả lời là`4`. 

Điều này phù hợp với lịch trình di chuyển thực tế. Hai quân bên trái có thể rời đi vào thời điểm (3) và (4), trong khi quân ở giữa sử dụng hành lang bên phải và rời đi vào thời điểm (4). 

Đối với ví dụ thứ hai, hãy xem xét```
1 1 1 1 3
1 1 1
2 1 3
3 1 5
```Hành lang là cột (2) và (4). Mỗi người lính có thời gian xuất cảnh sớm nhất có thể là (2). 

| Người Lính | Khu | (p_L) | (p_R) | 
| --- | --- | --- | --- | 
| A | 1 | 2 | INF | 
| B | 2 | 2 | 2 | 
| C | 3 | INF | 2 | 

Cố gắng (T=2) cho 

| Người Lính | (d_L) | (d_R) | 
| --- | --- | --- | 
| A | 1 | 0 | 
| B | 1 | 1 | 
| C | 0 | 1 | 

Với (a=1,b=1), cả ba người lính đều thỏa mãn hai điều kiện về thời hạn, nhưng chỉ có hai suất. Bất đẳng thức (3\le2) không thành công. 

Tại (T=3), thời hạn trở thành 

| Người Lính | (d_L) | (d_R) | 
| --- | --- | --- | 
| A | 2 | 0 | 
| B | 2 | 2 | 
| C | 0 | 2 | 

Mọi điều kiện Hall đều được thỏa mãn. Một nhiệm vụ khả thi sẽ đưa A và B qua hành lang bên trái và C qua hành lang bên phải. Hành lang bên trái sử dụng thời gian thoát (2) và (3), trong khi hành lang bên phải sử dụng thời gian thoát (2). Câu trả lời là`3`. 

Ví dụ này nắm bắt thời gian thoát sớm nhất trùng lặp. Các lối đi ngắn nhất riêng lẻ đều có chiều dài bằng hai, nhưng hai người lính sử dụng cùng một hành lang cần thời gian thoát ra khác nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(k\log k\log C)) | Quá trình sắp xếp thực hiện (O(k\log k)), mỗi lần kiểm tra tính khả thi sử dụng (O(k\log k)) và tìm kiếm nhị phân thực hiện (O(\log C)) kiểm tra | 
| Không gian | (O(k)) | Các binh sĩ, ngưỡng bên phải được nén và cây phân đoạn đều sử dụng không gian tuyến tính | 

Với (k\le100000), thuật toán không bao giờ phụ thuộc vào kích thước lưới. Các giá trị như (n,l_1,l_2,l_3) có thể là (10^9) mà không làm tăng kích thước cấu trúc dữ liệu. Yếu tố số duy nhất là tìm kiếm nhị phân trên câu trả lời, cần khoảng 31 lần lặp khi câu trả lời dưới vài tỷ. 

## Trường hợp thử nghiệm```python
# The solve() function from the previous section is assumed to be defined.

import sys
import io

def run(inp: str) -> str:
    global input

    old_stdin = sys.stdin
    old_input = input

    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    try:
        solve()

        # solve() writes directly to stdout, so this helper is intended
        # to be adapted with an output capture in an actual test harness.
        # A convenient contest-style version is shown below instead.
    finally:
        sys.stdin = old_stdin
        input = old_input

    return ""

# For a fully executable harness, redirect stdout as well.

def run(inp: str) -> str:
    global input

    old_stdin = sys.stdin
    old_stdout = sys.stdout
    old_input = input

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    input = sys.stdin.readline

    try:
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout
        input = old_input

# Provided sample
assert run(
    """4 2 2 2 3
1 2 1
1 2 2
2 2 4
"""
) == "4", "provided sample"

# Minimum-size cabin, one soldier in the left zone.
assert run(
    """1 1 1 1 1
1 1 1
"""
) == "2", "minimum cabin"

# One soldier on each side. Both can leave at time 2.
assert run(
    """1 1 1 1 2
1 1 1
3 1 5
"""
) == "2", "independent corridors"

# Three soldiers with identical earliest exit time.
# Two of them must share one corridor, so the answer is 3.
assert run(
    """1 1 1 1 3
1 1 1
2 1 3
3 1 5
"""
) == "3", "duplicate earliest exit times"

# Boundary case around both corridor columns.
assert run(
    """1 2 1 2 2
1 1 2
2 1 4
"""
) == "3", "corridor boundary"

# Large coordinates and maximum number of soldiers.
# All soldiers are in the left zone at row 1.
# Their earliest times are consecutive and the largest is 1000000001.
parts = ["1000000000 1000000000 1 1 100000"]
for y in range(1, 100001):
    parts.append(f"1 1 {y}")

assert run("\n".join(parts) + "\n") == "1000000001", "large k and coordinates"
```Trường hợp tùy chỉnh đầu tiên xác nhận các kích thước tối thiểu có thể có và ranh giới một bước di chuyển ngang cộng với một bước di chuyển dọc. 

Trường hợp thứ hai khẳng định hai hành lang độc lập khi quân lính không tranh giành cùng một hành lang. 

Trường hợp thứ ba kiểm tra rằng thời gian thoát sớm nhất bằng nhau không thể được chỉ định đồng thời cho cùng một hành lang. 

Trường hợp thứ tư đặt quân lính ngay cạnh cả hai ranh giới hành lang và mắc lỗi trong định nghĩa (c_1) và (c_2). 

Trường hợp cuối cùng sử dụng (100000) lính và tọa độ gần (10^9). Nó xác nhận cả hành vi tiệm cận và việc sử dụng số học số nguyên có kích thước tùy ý. 

| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1 1 1`với một người lính bên trái |`2`| Kích thước tối thiểu và xử lý từng cái một | 
|`1 1 1 1 2`với một người lính trái và một người lính phải |`2`| Lịch trình hành lang độc lập | 
|`1 1 1 1 3`với quân trái, giữa, phải |`3`| Thời gian thoát trùng lặp và năng lực hành lang | 
|`1 2 1 2 2`với quân giáp ranh |`3`| Công thức cột hành lang chính xác | 
|`1000000000 1000000000 1 1 100000`với 100000 quân trái |`1000000001`| Tối đa (k), tọa độ lớn và số học số nguyên | 

## Vỏ cạnh 

Đối với cabin tối thiểu```
1 1 1 1 1
1 1 1
```hành lang bên trái là cột (2). Người lính bắt đầu ở cột (1), vì vậy (p_L=1+(2-1)=2). Chỉ có một người lính nên không thể xảy ra va chạm ở hành lang. Tìm kiếm nhị phân cho thấy (T=1) là không khả thi và (T=2) là khả thi. 

Đối với thời gian thoát sớm nhất trùng lặp,```
1 1 1 1 3
1 1 1
2 1 3
3 1 5
```cả ba người lính đều có thời gian xuất cảnh sớm nhất (2). Tại (T=2), lính bên trái chỉ có ô bên trái tại thời điểm đảo ngược (1), lính ở giữa có thể sử dụng một trong hai ô của hành lang và lính bên phải chỉ có ô bên phải. Điều kiện của Hall cho một ô tiền tố trên mỗi hành lang nhìn thấy cả ba người lính nhưng chỉ có hai ô, do đó (T=2) không thành công. Tại (T=3), mỗi hành lang có đủ thời gian thoát riêng biệt và đáp án là (3). 

Đối với một người lính ở gần hành lang bên trái,```
1 2 1 2 2
1 1 2
2 1 4
```hành lang bên trái là cột (3), còn hành lang bên phải là cột (6). Lính bên trái có (p_L=2), còn lính giữa ở cột (4) có (p_L=2) và (p_R=3). Cả hai có thể rời đi riêng lẻ qua phía bên trái vào thời điểm (2), nhưng họ sẽ cần thời gian ra hành lang riêng biệt nếu cả hai đều sử dụng hành lang đó. Đưa lính trung gian sang bên phải sẽ có thời gian thoát ra (2) và (3), nên đáp án là`3`. Điều này mắc phải lỗi phổ biến khi coi hai cột hành lang như thể vị trí của chúng là (l_1) và (l_1+l_2+1) chứ không phải là (l_1+1) và (l_1+l_2+2). 

Đối với một trường hợp rất lớn,```
1000000000 1000000000 1 1 100000
```với tất cả (100000) binh sĩ ở hàng (1) của khu vực bên trái, thời gian thoát sớm nhất tạo thành dãy liên tiếp từ (999900002) đến (1000000001). Bởi vì thời gian sớm nhất liên tiếp đã có sẵn các khe thoát riêng biệt nên người lính cuối cùng sẽ rời đi vào thời điểm (1000000001). Thuật toán xử lý việc này mà không cần xây dựng lưới rộng (10^9).
