---
title: "CF 102222L - Khoảng thời gian liên tục"
description: "Đối với một mảng con (al,ldots,ar), hãy sắp xếp các giá trị của nó và xem xét các giá trị liên tiếp theo thứ tự được sắp xếp đó. Mảng con hợp lệ chính xác khi không có khoảng cách giữa hai giá trị phân biệt lân cận vượt quá (1). Cho phép các giá trị bằng nhau, do đó, một mảng con như ([1,1,2,2]) là hợp lệ."
date: "2026-08-17T22:18:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102222
codeforces_index: "L"
codeforces_contest_name: "2018-2019 ACM-ICPC, China Multi-Provincial Collegiate Programming Contest"
rating: 0
weight: 102222
solve_time_s: 143
verified: true
draft: false
---

[CF 102222L - Khoảng thời gian liên tục](https://codeforces.com/problemset/problem/102222/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 23s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Đối với một mảng con (a_l,\ldots,a_r), hãy sắp xếp các giá trị của nó và xem xét các giá trị liên tiếp theo thứ tự được sắp xếp đó. Mảng con hợp lệ chính xác khi không có khoảng cách giữa hai giá trị phân biệt lân cận vượt quá (1). Cho phép các giá trị bằng nhau, do đó, một mảng con như ([1,1,2,2]) là hợp lệ. 

Một cách hữu ích để thể hiện điều kiện là bỏ qua các bản sao lặp lại có cùng giá trị. Đặt (mx) là mức tối đa, (mn) mức tối thiểu và (cnt) số lượng giá trị riêng biệt trong mảng con. Vì mọi giá trị đều là số nguyên nằm giữa (mn) và (mx), nên có thể có (mx-mn+1) giá trị nguyên trong phạm vi đó. Mảng con liên tục chính xác khi mọi giá trị đó xuất hiện, điều này mang lại 

[ 
mx-mn+1=cnt. 
] 

Tương đương, 

[ 
mx-mn-cnt=-1. 
] 

Vì vậy, vấn đề trở thành việc đếm các mảng con có số lượng (mx-mn-cnt) bằng (-1). Đây là phép biến đổi trung tâm được sử dụng bởi giải pháp tiêu chuẩn. 

Có tối đa (10^5) phần tử trong một trường hợp thử nghiệm và tổng cộng (10^6) phần tử. Một giải pháp (O(n^2)) thực hiện các phần mở rộng khoảng (n(n+1)/2), tức là khoảng (5\cdot10^9) thao tác khi (n=10^5). Ngay cả với các bản cập nhật liên tục, điều đó vẫn vượt xa giới hạn cuộc thi 10 giây có thể chịu đựng được. Chúng ta cần một cách tiếp cận (O(n\log n)), chỉ tính tuyến tính hoặc logarit cho mỗi phần tử. 

Trường hợp cạnh đầu tiên là sự lặp lại. Đối với đầu vào```
1
2
1 1
```câu trả lời đúng là (3), vì cả ba mảng con đều liên tục. Một giải pháp bất cẩn có thể so sánh độ dài mảng con với (mx-mn+1), coi ([1,1]) là có hai phần tử trong một phạm vi chỉ chứa một số nguyên. Sự lặp lại không tạo ra khoảng trống, vì vậy số lượng chính xác là số giá trị riêng biệt chứ không phải độ dài. 

Trường hợp cạnh thứ hai là một số nguyên bị thiếu. Vì```
1
2
1 3
```câu trả lời là (2). Các khoảng đơn là hợp lệ, nhưng ([1,3]) thì không, vì các giá trị được sắp xếp có khoảng cách là (2). Chỉ kiểm tra xem mức tối thiểu và tối đa có gần với điểm cuối của một phạm vi nào đó hay không là không đủ. Đẳng thức đếm khác biệt phát hiện chính xác giá trị còn thiếu. 

Trường hợp cạnh thứ ba là khoảng chứa các giá trị trùng lặp và tất cả các giá trị bắt buộc. Vì```
1
4
1 2 2 3
```mỗi một trong mười mảng con đều liên tục, nên câu trả lời là (10). Khoảng đầy đủ có (mx=3), (mn=1) và (cnt=3), cho (3-1+1=3). Bản sao (2) không thay đổi (cnt). 

Trường hợp khó triển khai cuối cùng xuất phát từ thực tế là câu trả lời có thể lớn. Khi tất cả (10^5) phần tử bằng nhau thì mọi mảng con đều hợp lệ và câu trả lời là 

[ 
\frac{10^5\cdot100001}{2}=5.000.050.000. 
] 

Số nguyên có dấu 32 bit là không đủ. Số nguyên Python tự động xử lý việc này, nhưng thuật toán tương tự trong C++ cần câu trả lời 64 bit. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sửa điểm cuối bên trái và mở rộng điểm cuối bên phải. Trong khi mở rộng, chúng ta có thể duy trì mức tối thiểu, tối đa hiện tại và một tập hợp các giá trị riêng biệt. Sau đó, mỗi phần mở rộng sẽ thực hiện công việc mong đợi (O(1)) cho tập hợp và công việc không đổi cho cực trị. Điều này cho ra tổng thời gian (O(n^2)) và nó đúng vì mỗi mảng con được kiểm tra chính xác một lần. 

Vấn đề là số lượng mảng con. Có (n(n+1)/2) trong số chúng, tức là khoảng (5\cdot10^9) cho (n=10^5). Mặc dù mỗi tiện ích mở rộng riêng lẻ đều rẻ nhưng tổng công việc thì không. 

Cách tiếp cận nhanh hơn sẽ sửa điểm cuối bên phải (r) và xem xét đồng thời mọi điểm cuối bên trái có thể có. Xác định 

[ 
F_r(l)=\max(a_l,\ldots,a_r)-\min(a_l,\ldots,a_r)-\operatorname{distinct}(a_l,\ldots,a_r). 
] 

Chúng ta cần số lượng điểm cuối bên trái với (F_r(l)=-1). 

Khi (a_r) được thêm vào, những thay đổi tối đa chỉ dành cho tập hợp các điểm cuối bên trái liền kề. Ngăn xếp giảm dần đơn điệu cho chúng ta biết chính xác điểm cuối bên trái nào được thay thế mức tối đa bằng (a_r). Tương tự, ngăn xếp tăng đơn điệu cho chúng ta biết điểm cuối bên trái nào được thay thế mức tối thiểu bằng (a_r). 

Phần riêng biệt có bản cập nhật đặc biệt rõ ràng. Giả sử lần xuất hiện trước đó của (a_r) là ở vị trí (p). Đối với một mảng con kết thúc tại (r), giá trị mới (a_r) là một giá trị riêng biệt mới chính xác khi điểm cuối bên trái của nó nằm trong ([p+1,r]). Do đó, chúng tôi trừ (1) từ (F_r(l)) trên phạm vi chính xác đó. 

Tất cả ba thao tác đều là phép cộng phạm vi cho một mảng được lập chỉ mục bởi điểm cuối bên trái. Chúng ta cần duy trì giá trị tối thiểu của mảng đó và có bao nhiêu vị trí đạt được mức tối thiểu đó. Cây phân đoạn lười biếng hỗ trợ chính xác thao tác này. 

Ngoài ra còn có một bất đẳng thức hữu ích: 

[ 
\operatorname{distinct}\leq \max-\min+1. 
] 

Do đó 

[ 
F_r(l)=\max-\min-\operatorname{distinct}\geq -1. 
] 

Khoảng đơn ([r,r]) luôn có (F_r(r)=-1). Do đó, sau khi xử lý vị trí (r), mức tối thiểu toàn cục của cây phân đoạn luôn chính xác (-1) và số vị trí đạt mức tối thiểu đó chính xác là số mảng con hợp lệ kết thúc tại (r). Chúng tôi thậm chí không cần một truy vấn riêng trên ([1,r]). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n^2)) | (O(n)) | Quá chậm | 
| Tối ưu | (O(n\log n)) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Tạo cây phân đoạn có lá (l) đại diện cho giá trị (F(l)) cho điểm cuối bên phải hiện tại. Ban đầu mọi giá trị đều là (0), vì chưa có điểm cuối bên phải nào được xử lý. Mỗi nút cây lưu trữ giá trị tối thiểu trong phạm vi của nó, số lượng lá đạt mức tối thiểu đó và phép cộng lười biếng. 
2. Duy trì ngăn xếp giảm dần đơn điệu để có giá trị tối đa. Mỗi mục ngăn xếp lưu trữ một giá trị và vị trí gần đây nhất của nó. Trước khi chèn (a_r), các điểm cuối bên trái sau đỉnh ngăn xếp trước đó đều lấy (a_r) làm mức tối đa mới, vì vậy hãy thêm (a_r) vào phạm vi đó. 
3. Xóa các mục ngăn xếp có giá trị nhỏ hơn hoặc bằng (a_r). Khi một mục nhập ((v,p)) bị xóa, mức đóng góp tối đa trên phạm vi điểm cuối bên trái giữa vị trí ngăn xếp còn sót lại trước đó và (p) sẽ thay đổi từ (v) thành (a_r). Thêm (a_r-v) vào phạm vi đó. Các giá trị bằng nhau cũng được hiển thị vì lần xuất hiện mới nhất sẽ đưa ra ranh giới chính xác cho các bản cập nhật trong tương lai. 
4. Đẩy ((a_r,r)) vào ngăn xếp tối đa. Mỗi vị trí mảng vào và rời khỏi ngăn xếp này nhiều nhất một lần, vì vậy tất cả các cập nhật tối đa cùng nhau chỉ yêu cầu các thao tác cập nhật phạm vi (O(n)). 
5. Lặp lại cách xây dựng tương tự với ngăn xếp tăng dần đơn điệu để có giá trị tối thiểu. Đóng góp tối thiểu có dấu âm trong (F), vì vậy khi (a_r) trở thành mức tối thiểu mới, đóng góp trực tiếp là (-a_r). Khi mức tối thiểu cũ (v) được thay thế bằng (a_r), hãy thêm (v-a_r). 
6. Tra cứu lần xuất hiện trước (p) của (a_r), sử dụng từ điển. Thêm (-1) vào phạm vi ([p+1,r]). Đó chính xác là các điểm cuối bên trái mà (a_r) chưa có trong mảng con trước đó. Sau đó lưu trữ (r) như lần xuất hiện mới trước đó. 
7. Đọc gốc của cây phân đoạn. Giá trị tối thiểu của nó là (-1) và số lượng của nó là số khoảng hợp lệ kết thúc tại (r). Thêm số đó vào câu trả lời. 
8. Sau khi tất cả các điểm cuối phù hợp đã được xử lý, hãy in câu trả lời tích lũy theo yêu cầu`Case #x: y`định dạng. 

### Tại sao nó hoạt động 

Đối với mỗi điểm cuối bên phải cố định (r), lá cây phân đoạn (l) biểu diễn chính xác 

[ 
F_r(l)=\max(a_l,\ldots,a_r)-\min(a_l,\ldots,a_r)-\operatorname{distinct}(a_l,\ldots,a_r). 
] 

Các bản cập nhật ngăn xếp tối đa duy trì thuật ngữ tối đa cho mọi điểm cuối bên trái, bởi vì các phân vùng ngăn xếp bên trái điểm cuối theo số lần xuất hiện hiện tại là lớn nhất. Ngăn xếp tối thiểu thực hiện tương tự đối với thời hạn tối thiểu. Bản cập nhật lần xuất hiện trước trừ chính xác một lần cho mỗi giá trị riêng biệt trong mỗi mảng con, bởi vì một giá trị góp phần vào số lượng riêng biệt cho các điểm cuối bên trái sau lần xuất hiện trước đó. 

Do đó, mỗi lá đều có (F_r(l)) chính xác sau khi vị trí (r) được xử lý. Một mảng con liên tục chính xác khi (F_r(l)=-1). Vì mọi (F_r(l)\geq-1) và khoảng đơn đều cho kết quả bằng nhau nên giá trị tối thiểu của cây phân đoạn là (-1) và số lượng tối thiểu của nó chính xác là số khoảng liên tục kết thúc tại (r). Tính tổng số này trên tất cả (r) sẽ đếm mọi mảng con hợp lệ chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(1_000_000)

class SegmentTree:
    def __init__(self, n):
        self.n = n
        size = 4 * n + 5
        self.mn = [0] * size
        self.cnt = [0] * size
        self.lazy = [0] * size
        self._build(1, 1, n)

    def _build(self, p, l, r):
        self.cnt[p] = r - l + 1
        if l == r:
            return
        m = (l + r) >> 1
        self._build(p << 1, l, m)
        self._build(p << 1 | 1, m + 1, r)

    def _push(self, p):
        z = self.lazy[p]
        if z:
            lc = p << 1
            rc = lc | 1

            self.mn[lc] += z
            self.mn[rc] += z
            self.lazy[lc] += z
            self.lazy[rc] += z

            self.lazy[p] = 0

    def _pull(self, p):
        lc = p << 1
        rc = lc | 1

        if self.mn[lc] < self.mn[rc]:
            self.mn[p] = self.mn[lc]
            self.cnt[p] = self.cnt[lc]
        elif self.mn[lc] > self.mn[rc]:
            self.mn[p] = self.mn[rc]
            self.cnt[p] = self.cnt[rc]
        else:
            self.mn[p] = self.mn[lc]
            self.cnt[p] = self.cnt[lc] + self.cnt[rc]

    def _add(self, p, l, r, ql, qr, value):
        if ql <= l and r <= qr:
            self.mn[p] += value
            self.lazy[p] += value
            return

        self._push(p)

        m = (l + r) >> 1
        if ql <= m:
            self._add(p << 1, l, m, ql, qr, value)
        if qr > m:
            self._add(p << 1 | 1, m + 1, r, ql, qr, value)

        self._pull(p)

    def add(self, l, r, value):
        if l <= r:
            self._add(1, 1, self.n, l, r, value)

    @property
    def minimum(self):
        return self.mn[1]

    @property
    def minimum_count(self):
        return self.cnt[1]

def count_intervals(a):
    n = len(a)
    seg = SegmentTree(n)

    big = []
    small = []
    last = {}

    answer = 0

    for r, x in enumerate(a, 1):
        # Add the contribution of the new maximum.
        left = big[-1][1] + 1 if big else 1
        seg.add(left, r, x)

        # Replace old maxima by x.
        while big and big[-1][0] <= x:
            value, pos = big.pop()
            left = big[-1][1] + 1 if big else 1
            seg.add(left, pos, x - value)

        big.append((x, r))

        # Add the contribution of the new minimum.
        left = small[-1][1] + 1 if small else 1
        seg.add(left, r, -x)

        # Replace old minima by x.
        while small and small[-1][0] >= x:
            value, pos = small.pop()
            left = small[-1][1] + 1 if small else 1
            seg.add(left, pos, value - x)

        small.append((x, r))

        # Count x as a distinct value exactly for left endpoints
        # after its previous occurrence.
        previous = last.get(x, 0)
        seg.add(previous + 1, r, -1)
        last[x] = r

        # The minimum is always -1 because [r, r] is valid.
        answer += seg.minimum_count

    return answer

def solve():
    t = int(input())
    out = []

    for case_id in range(1, t + 1):
        n = int(input())
        a = list(map(int, input().split()))

        out.append(
            f"Case #{case_id}: {count_intervals(a)}"
        )

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Cây phân đoạn được khởi tạo với tất cả các giá trị bằng 0. các`cnt`mảng ghi lại số lượng lá thuộc về mỗi nút, do đó mức tối thiểu ban đầu bằng 0 và mọi vị trí đều đóng góp vào số lượng của nó. 

các`_add`phương thức thực hiện phép cộng phạm vi lười tiêu chuẩn. Nút được che phủ hoàn toàn sẽ nhận giá trị trực tiếp, trong khi các nút được che phủ một phần sẽ đẩy giá trị lười đang chờ xử lý của chúng sang nút con, lặp lại và sau đó tính toán lại số lượng tối thiểu và tối thiểu của chúng. 

Ngăn xếp tối đa lưu trữ các cặp`(value, position)`theo thứ tự giá trị giảm dần. Phép cộng phạm vi đầu tiên gán giá trị mới cho các điểm cuối bên trái sau đỉnh ngăn xếp hiện tại. Mỗi mục nhập được bật lên đại diện cho một phạm vi có mức tối đa cũ được thay thế bằng giá trị mới, do đó đóng góp của nó thay đổi theo`x - value`. 

Ngăn xếp tối thiểu là đối xứng, nhưng đóng góp của nó có dấu ngược lại. Số tiền thay thế của nó là`value - x`. 

Từ điển`last`được khóa bởi giá trị mảng thực tế, vì vậy các giá trị lên tới (10^9) không yêu cầu nén tọa độ. Nếu như`previous`là lần xuất hiện cuối cùng của`x`, thì chính xác các điểm cuối bên trái từ`previous + 1`bởi vì`r`nhìn thấy`x`lần đầu tiên. 

Câu trả lời được cập nhật bằng cách sử dụng`seg.minimum_count`sau tất cả ba loại thay đổi đối với điểm cuối bên phải hiện tại. Singleton ([r,r]) luôn tạo giá trị tối thiểu bằng (-1), do đó không cần phạm vi truy vấn hoặc xử lý đặc biệt đối với các lá không hoạt động. 

Số nguyên Python cũng loại bỏ mọi lo ngại về tràn cho câu trả lời cuối cùng. Độ sâu của cây phân đoạn đệ quy chỉ là (O(\log n)), trong khi`sys.setrecursionlimit`cung cấp đủ chỗ cho việc thực hiện đệ quy. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên, mảng là ([1,2,1,2]). Mọi mảng con đều liên tục vì các giá trị duy nhất có thể xuất hiện là (1) và (2), là các giá trị liên tiếp. 

| Điểm cuối bên phải (r) | (a_r) | Điểm cuối bên trái hợp lệ | Tối thiểu | Số lượng tối thiểu | Chạy câu trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | -1 | 1 | 1 | 
| 2 | 2 | 1, 2 | -1 | 2 | 3 | 
| 3 | 1 | 1, 2, 3 | -1 | 3 | 6 | 
| 4 | 2 | 1, 2, 3, 4 | -1 | 4 | 10 | 

Điểm quan trọng ở đây là các bản sao không làm tăng số lượng khác biệt. Ví dụ: tại (r=3), khoảng ([1,2,1]) có giá trị lớn nhất (2), giá trị nhỏ nhất (1) và hai giá trị phân biệt, do đó (2-1-2=-1). Ba khoảng kết thúc ở vị trí (3) đều được tính. 

Đối với mẫu thứ hai, mảng là ([1,3,2,4]). 

| Điểm cuối bên phải (r) | (a_r) | Điểm cuối bên trái hợp lệ | Tối thiểu | Số lượng tối thiểu | Chạy câu trả lời | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | -1 | 1 | 1 | 
| 2 | 3 | 2 | -1 | 1 | 2 | 
| 3 | 2 | 1, 2, 3 | -1 | 3 | 5 | 
| 4 | 4 | 2, 3, 4 | -1 | 3 | 8 | 

Tại (r=2), khoảng ([1,3]) bị từ chối vì mức tối đa của nó là (3), mức tối thiểu của nó là (1) và nó chứa hai giá trị riêng biệt. Giá trị của nó là (3-1-2=0), không phải (-1). Tại (r=3), việc thêm (2) sẽ điền số nguyên còn thiếu vào ([1,3]), do đó ([1,3,2]) trở thành hợp lệ. Tại (r=4), ([3,2,4]) hợp lệ vì các giá trị được sắp xếp của nó là (2,3,4), trong khi ([2,4]) vẫn không hợp lệ vì thiếu (3). 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n\log n)) | Mỗi phần tử đi vào và rời khỏi mỗi ngăn xếp đơn điệu một lần, tạo ra các phép cộng phạm vi (O(n)) và chi phí bổ sung mỗi phạm vi (O(\log n)). | 
| Không gian | (O(n)) | Cây phân đoạn, hai ngăn xếp đơn điệu và từ điển xuất hiện lần cuối đều sử dụng bộ nhớ (O(n)). | 

Trên tất cả các trường hợp thử nghiệm, tổng (n) tối đa là (10^6). Thuật toán thực hiện một số tuyến tính các thao tác ngăn xếp và cập nhật phạm vi cho mỗi phần tử, với chi phí cây phân đoạn logarit, mang lại công tiệm cận (O(10^6\log 10^5)). Mức tiêu thụ bộ nhớ là tuyến tính trong trường hợp thử nghiệm lớn nhất, phù hợp với giới hạn 256 MB đã nêu cho việc triển khai dự kiến. 

## Trường hợp thử nghiệm 

Các thử nghiệm sau đây dự kiến sẽ được thêm vào sau mã giải pháp ở trên. Trình trợ giúp chuyển hướng đầu vào và đầu ra tiêu chuẩn và đặt lại`input`để mỗi lệnh gọi hoạt động giống như một cuộc thi riêng biệt.```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()

        global input
        input = sys.stdin.readline

        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout
        input = sys.stdin.readline

# Provided samples
sample = """\
2
4
1 2 1 2
4
1 3 2 4
"""
assert run(sample) == "Case #1: 10\nCase #2: 8", "provided samples"

# Minimum-size input
assert run("""\
1
1
7
""") == "Case #1: 1", "single element"

# All values equal: every subarray is continuous.
assert run("""\
1
5
9 9 9 9 9
""") == "Case #1: 15", "all equal values"

# Missing integer in the pair, then restored by the third value.
assert run("""\
1
3
1 3 2
""") == "Case #1: 5", "missing integer"

# Large value gap. Only the singleton intervals are valid.
assert run("""\
1
2
1 1000000000
""") == "Case #1: 2", "boundary values"

# Duplicates must not be counted as additional distinct values.
assert run("""\
1
4
1 2 2 3
""") == "Case #1: 10", "duplicates"

# Maximum-size case, also checks that the answer exceeds 32-bit range.
n = 100000
maximum_case = "1\n" + str(n) + "\n" + ("7 " * (n - 1)) + "7\n"
assert run(maximum_case) == "Case #1: 5000050000", "maximum-size all-equal case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 / 7`|`Case #1: 1`| Xử lý đầu vào và đơn lẻ tối thiểu | 
|`1 / 5 / 9 9 9 9 9`|`Case #1: 15`| Mọi mảng con vẫn hợp lệ khi tất cả các giá trị đều bằng nhau | 
|`1 / 3 / 1 3 2`|`Case #1: 5`| Một số nguyên bị thiếu có thể làm cho một khoảng không hợp lệ, sau đó hợp lệ sau phần mở rộng | 
|`1 / 2 / 1 1000000000`|`Case #1: 2`| Khoảng cách giá trị rất lớn | 
|`1 / 4 / 1 2 2 3`|`Case #1: 10`| Xử lý đúng các bản sao | 
| (n=100000), tất cả các giá trị`7`|`Case #1: 5000050000`| Kích thước đầu vào tối đa và câu trả lời lớn | 

## Vỏ cạnh 

Đối với trường hợp giá trị trùng lặp```
1
2
1 1
```vị trí đầu tiên tạo ra (F(1,1)=1-1-1=-1). Ở vị trí thứ hai, mức tối đa và tối thiểu không thay đổi. Sự xuất hiện trước đó của (1) là vị trí (1), do đó, việc cập nhật số lượng khác biệt chỉ ảnh hưởng đến điểm cuối bên trái (2). Do đó, cả hai lá đều có giá trị (-1) và cây phân đoạn báo cáo hai khoảng hợp lệ kết thúc ở vị trí (2). Cùng với singleton đầu tiên, câu trả lời là (3). 

Đối với trường hợp thiếu giá trị```
1
2
1 3
```khoảng kết thúc ở vị trí (2) với điểm cuối bên trái (1) có (mx=3), (mn=1) và (cnt=2), cho ra (F=0). Đơn vị ([3]) có (F=-1). Do đó, mức tối thiểu của cây phân đoạn vẫn là (-1), nhưng chỉ có một lá đạt được. Tổng đáp án là (1+1=2). 

Đối với phạm vi được điền trùng lặp```
1
4
1 2 2 3
```khoảng đầy đủ có (mx=3), (mn=1) và (cnt=3), vì vậy giá trị của nó là (-1). Việc lặp lại (2) không thay đổi số lượng tối thiểu cũng như số lượng riêng biệt. Lý do tương tự áp dụng cho mọi khoảng thời gian ngắn hơn, đưa ra mười mảng con hợp lệ. 

Đối với trường hợp giá trị bằng kích thước tối đa, mỗi mảng con có (mx=mn) và (cnt=1), do đó (F=-1) cho mọi điểm cuối bên trái. Tại mỗi điểm cuối bên phải (r), số lượng tối thiểu của cây phân đoạn chính xác là (r). Tính tổng (1+2+\cdots+100000) cho (5.000.050.000), xác nhận cả logic đếm và sự cần thiết của loại số nguyên có khả năng chứa kết quả đầy đủ.
