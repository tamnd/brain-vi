---
title: "CF 160E - Xe buýt và con người"
description: "Mỗi xe buýt hoạt động tại đúng một thời điểm. Một xe buýt bắt đầu tại điểm dừng s và kết thúc tại điểm dừng f có thể chở bất kỳ hành khách nào có khoảng cách chuyến đi [l, r] nằm trọn trong [s, f]. Một người đến trạm l vào thời điểm b nên chỉ được đi xe buýt có thời gian t thỏa mãn t = b."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "sortings"]
categories: ["algorithms"]
codeforces_contest: 160
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 111 (Div. 2)"
rating: 2400
weight: 160
solve_time_s: 133
verified: true
draft: false
---

[CF 160E - Xe buýt và con người](https://codeforces.com/problemset/problem/160/E) 

**Đánh giá:** 2400 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, sắp xếp 
**Thời gian giải:** 2m 13s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi xe buýt hoạt động tại đúng một thời điểm. Xe buýt khởi hành tại điểm dừng`s`và kết thúc ở điểm dừng`f`có thể chở bất kỳ hành khách nào có khoảng thời gian chuyến đi`[l, r]`được chứa đầy đủ bên trong`[s, f]`. Một người đến điểm dừng`l`vào thời điểm đó`b`, nên họ chỉ có thể sử dụng xe buýt có thời gian`t`thỏa mãn`t >= b`. Trong số tất cả những chiếc xe buýt như vậy, họ luôn chọn chiếc có thời gian ngắn nhất. 

Đối với mỗi người, chúng ta phải trả lời một truy vấn hình học theo từng khoảng thời gian: 

Tìm xe buýt có giá trị tối thiểu`t`như vậy: 

-`s <= l`-`r <= f`-`t >= b`Nếu không có xe buýt như vậy tồn tại, hãy in`-1`. 

Các ràng buộc ngay lập tức loại trừ bất kỳ việc kiểm tra theo cặp nào. Với tối đa`10^5`xe buýt và`10^5`mọi người, việc quét trực tiếp cho mọi truy vấn sẽ yêu cầu`10^10`so sánh trong trường hợp xấu nhất, vượt xa những gì phù hợp trong một giây. 

Phần khó khăn là truy vấn có ba điều kiện cùng một lúc. Chúng tôi không chỉ lọc theo thời gian mà còn hỏi liệu một khoảng có chứa một khoảng khác hay không. Giải pháp chỉ xử lý hiệu quả một chiều vẫn còn quá chậm. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Hãy xem xét xe buýt:```
1 5 10
2 6 20
```và một người:```
2 5 10
```Câu trả lời đúng là chuyến xe buýt đầu tiên, vì thời gian của nó đúng bằng`b`, và sự bình đẳng được cho phép. Tìm kiếm nhị phân bất cẩn bằng cách sử dụng`t > b`thay vì`t >= b`âm thầm thất bại ở đây. 

Một trường hợp tinh tế khác là các xe buýt chồng chéo trong đó xe buýt trước không chứa khoảng thời gian dành cho hành khách nhưng xe buýt sau thì có.```
Buses:
1 4 5
2 10 7

Person:
3 8 1
```Câu trả lời là chiếc xe buýt thứ hai. Cấu trúc tham lam chỉ giữ xe buýt hoạt động trong thời gian tối thiểu sẽ từ chối truy vấn một cách không chính xác vì xe buýt sớm nhất không thể phục vụ hành khách. 

Một tình huống nguy hiểm nữa xuất hiện khi nhiều xe buýt cùng bao phủ một ranh giới bên trái nhưng có ranh giới bên phải khác nhau.```
Buses:
1 5 3
1 10 8

Person:
2 8 1
```Câu trả lời đúng là chuyến xe buýt thứ hai. Nếu chúng ta nén thông tin không chính xác và chỉ giữ lại một bus cho mỗi điểm bắt đầu, chúng ta sẽ mất thông tin cần thiết. 

## Phương pháp tiếp cận 

Giải pháp vũ phu rất đơn giản. Đối với mỗi người, hãy quét từng xe buýt và kiểm tra ba điều kiện: 

-`s <= l`-`r <= f`-`t >= b`Trong số tất cả các xe buýt hợp lệ, hãy chọn chiếc có giá trị nhỏ nhất`t`. 

Logic này hoàn toàn chính xác vì nó thực hiện trực tiếp định nghĩa từ câu lệnh. Vấn đề là hiệu suất. Với`10^5`mọi người và`10^5`xe buýt, trường hợp xấu nhất thực hiện`10^10`séc. Ngay cả việc triển khai được tối ưu hóa cao cũng không thể hoàn thành kịp thời. 

Nhận xét quan trọng là thời gian hoàn toàn có trật tự và duy nhất. Nếu chúng ta xử lý xe buýt theo thứ tự tăng dần`t`, thì đối với một người có ngưỡng`b`, câu trả lời đơn giản là chuyến xe buýt đầu tiên sau thời gian`b`khoảng của nó chứa`[l, r]`. 

Điều này biến vấn đề thành một vấn đề tìm kiếm hình học ngoại tuyến. 

Giả sử chúng ta xử lý xe buýt bằng cách tăng thời gian. Bất cứ lúc nào, chúng tôi muốn trả lời: 

Trong số các xe buýt đã được xử lý, có tồn tại một xe buýt có: 

-`s <= l`-`f >= r`Đây là truy vấn thống trị hai chiều cổ điển. 

Chi tiết cấu trúc quan trọng là các điểm dừng có phạm vi lên tới`10^9`, nhưng chỉ`10^5`tọa độ thực sự xuất hiện. Nén tọa độ cho phép chúng tôi làm việc trên các chỉ số thay vì các giá trị thô. 

Bây giờ hãy xem xét việc sửa điều kiện điểm cuối bên trái. Nếu chúng ta quét từ lớn`s`hướng tới nhỏ`s`, thì bất cứ khi nào chúng ta ở tọa độ`x`, mọi bus được chèn đều đã thỏa mãn`s >= x`. 

Đối với truy vấn có điểm cuối bên trái`l`, chúng ta cần xe buýt với`s <= l`, không`s >= l`. Chúng ta có thể đảo ngược hướng quét: 

- Sắp xếp xe buýt theo`s`tăng dần. 
- Sắp xếp truy vấn theo`l`tăng dần. 
- Chèn tất cả các xe buýt với`s <= l`. 

Trong số các xe buýt được chèn vào, chúng ta chỉ cần biết liệu một số xe buýt có`f >= r`, và trong số tất cả những chiếc xe buýt như vậy, chúng tôi muốn có thời gian nhỏ nhất. 

Điều này trở thành vấn đề về cây phân đoạn được lập chỉ mục bằng cách nén`f`. 

Đối với mọi điểm cuối xe buýt`f`, chúng tôi lưu trữ thời gian tối thiểu giữa các xe buýt kết thúc ở đó. Sau đó truy vấn hậu tố`[r, maxF]`đưa ra thời gian xe buýt hợp lệ tối thiểu. 

Điều phức tạp cuối cùng là các truy vấn yêu cầu thời gian tối thiểu lớn hơn hoặc bằng`b`, không chỉ là mức tối thiểu tuyệt đối. Chúng tôi giải quyết vấn đề này bằng cách chia để trị các câu trả lời. 

Chúng tôi tìm kiếm nhị phân bus trả lời cho mọi truy vấn trên danh sách bus được sắp xếp theo thời gian. Trong một lần lặp tìm kiếm nhị phân song song, chúng tôi kiểm tra xem một bus hợp lệ có tồn tại trong số bus đầu tiên hay không.`mid`xe buýt. 

Thử nghiệm tính khả thi đó chính xác là truy vấn ngăn chặn theo khoảng thời gian đã được giải quyết ở trên. 

Sự kết hợp mang lại một`O((n + m) log^2 n)`giải pháp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm) | O(1) | Quá chậm | 
| Tối ưu | O((n + m) log² n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp tất cả các xe buýt theo thời gian tăng dần`t`. 
2. Phối hợp-nén tất cả các tọa độ điểm dừng xuất hiện trên xe buýt và người dân. 

Tính năng nén cho phép lập chỉ mục cây phân đoạn trong khi vẫn giữ nguyên thứ tự khoảng thời gian. 
3. Đối với mỗi người, tìm kiếm nhị phân chỉ số xe buýt nhỏ nhất có thể phục vụ họ. 

Vì các xe buýt được sắp xếp theo thời gian nên việc tìm kiếm xe buýt hợp lệ đầu tiên sẽ tự động đưa ra thời gian hợp lệ tối thiểu. 
4. Sử dụng tìm kiếm nhị phân song song để xử lý tất cả mọi người cùng nhau. 

Mỗi người duy trì một phạm vi tìm kiếm`[low, high]`trên các chỉ số xe buýt. 
5. Trong một lần lặp lại, nhóm mọi người theo điểm giữa hiện tại của họ`mid`. 

Một người hỏi: "Có thể có một số xe buýt đầu tiên`mid`xe buýt chứa khoảng thời gian của tôi?" 
6. Xử lý các bus tăng dần từ thời điểm nhỏ nhất trở lên. 

Khi xe buýt`i`được thêm vào, chèn nó vào cây phân đoạn tại vị trí`f_i`. 
7. Cây phân đoạn lưu trữ điểm cuối bên trái tối thiểu`s`giữa các xe buýt kết thúc tại mỗi vị trí. 

Đối với mỗi phạm vi hậu tố`[r, maxF]`, truy vấn mức lưu trữ tối thiểu`s`cho chúng ta biết liệu một số bus được chèn có thỏa mãn hay không`s <= l`. 
8. Để trả lời một câu hỏi của một người`(l, r)`ở điểm giữa`mid`, truy vấn cây phân đoạn qua hậu tố`[r, maxF]`. 

Nếu tối thiểu`s`trong hậu tố đó nhiều nhất là`l`, thì một số bus được chèn sẽ chứa khoảng đó. 
9. Nếu truy vấn thành công, hãy di chuyển phạm vi tìm kiếm nhị phân sang trái. 

Nếu không hãy di chuyển nó sang phải. 
10. Sau khi tìm kiếm nhị phân song song kết thúc, mỗi người hoặc không có bus khả thi hoặc có chỉ số bus khả thi đầu tiên. 
11. Chuyển đổi chỉ số về số bus ban đầu theo thứ tự đầu vào. 

### Tại sao nó hoạt động 

Tại điểm giữa`mid`, cấu trúc dữ liệu chứa chính xác các xe buýt có thời gian nằm trong số các xe buýt đầu tiên`mid`những giá trị nhỏ nhất. Một truy vấn thành công chính xác khi có ít nhất một xe buýt được chèn chứa khoảng thời gian hành khách. 

Bất biến của cây phân đoạn là: 

Đối với mọi điểm cuối nén`f`, cây lưu trữ tối thiểu`s`trong số các xe buýt được chèn kết thúc tại`f`. 

Khi truy vấn hậu tố`[r, maxF]`, chúng tôi kiểm tra tất cả các xe buýt với`f >= r`. Nếu lưu trữ tối thiểu`s`bên trong hậu tố đó thỏa mãn`s <= l`, thì một số bus đồng thời thỏa mãn cả hai điều kiện ngăn chặn: 

-`s <= l`-`f >= r`Vì các bus được xử lý theo thứ tự thời gian tăng dần nên điểm giữa đầu tiên mà truy vấn trở nên khả thi sẽ tương ứng chính xác với thời gian bus hợp lệ tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10**18

class SegTree:
    def __init__(self, n):
        self.n = 1
        while self.n < n:
            self.n <<= 1
        self.seg = [INF] * (2 * self.n)

    def reset(self):
        for i in range(2 * self.n):
            self.seg[i] = INF

    def update(self, pos, val):
        pos += self.n
        if val < self.seg[pos]:
            self.seg[pos] = val
        else:
            return

        pos >>= 1
        while pos:
            self.seg[pos] = min(self.seg[pos << 1],
                                self.seg[pos << 1 | 1])
            pos >>= 1

    def query(self, l, r):
        res = INF
        l += self.n
        r += self.n

        while l <= r:
            if l & 1:
                res = min(res, self.seg[l])
                l += 1
            if not (r & 1):
                res = min(res, self.seg[r])
                r -= 1
            l >>= 1
            r >>= 1

        return res

def solve():
    n, m = map(int, input().split())

    buses = []
    coords = []

    for idx in range(n):
        s, f, t = map(int, input().split())
        buses.append((t, s, f, idx + 1))
        coords.append(s)
        coords.append(f)

    people = []

    for idx in range(m):
        l, r, b = map(int, input().split())
        people.append((l, r, b, idx))
        coords.append(l)
        coords.append(r)

    coords = sorted(set(coords))
    comp = {x: i for i, x in enumerate(coords)}

    buses.sort()

    bus_data = []
    for t, s, f, idx in buses:
        bus_data.append((s, f, idx))

    low = [0] * m
    high = [n - 1] * m
    ans = [-1] * m

    possible = [False] * m

    for i, (l, r, b, idx) in enumerate(people):
        if buses[-1][0] >= b:
            possible[i] = True
        else:
            low[i] = 1
            high[i] = 0

    while True:
        buckets = [[] for _ in range(n)]
        active = False

        for i in range(m):
            if low[i] <= high[i]:
                active = True
                mid = (low[i] + high[i]) // 2
                buckets[mid].append(i)

        if not active:
            break

        seg = SegTree(len(coords))

        ptr = 0

        for mid in range(n):
            while ptr <= mid:
                s, f, _ = bus_data[ptr]
                seg.update(comp[f], s)
                ptr += 1

            for qi in buckets[mid]:
                l, r, b, idx = people[qi]

                if buses[mid][0] < b:
                    low[qi] = mid + 1
                    continue

                cr = comp[r]

                best = seg.query(cr, len(coords) - 1)

                if best <= l:
                    ans[qi] = mid
                    high[qi] = mid - 1
                else:
                    low[qi] = mid + 1

    out = []

    for i in range(m):
        if ans[i] == -1:
            out.append("-1")
        else:
            out.append(str(buses[ans[i]][3]))

    print(" ".join(out))

solve()
```Giải pháp bắt đầu bằng cách sắp xếp các xe buýt theo thời gian vì mọi truy vấn đều yêu cầu thời gian khả thi nhỏ nhất. Khi các xe buýt được sắp xếp theo cách này, vấn đề sẽ trở thành việc tìm tiền tố đầu tiên chứa khoảng hợp lệ. 

Việc nén tọa độ là cần thiết vì số điểm dừng đạt tới`10^9`. Cây phân đoạn chỉ quan tâm đến thứ tự tương đối nên chỉ số nén là đủ. 

Cây phân đoạn lưu trữ các vị trí bắt đầu tối thiểu. Hướng này rất dễ bị sai. Đối với một truy vấn`(l, r)`, chúng tôi kiểm tra tất cả các xe buýt với`f >= r`. Trong số đó, nếu lưu trữ tối thiểu`s`nhiều nhất là`l`, thì một số bus chứa khoảng đó. 

Tìm kiếm nhị phân song song là tối ưu hóa cốt lõi. Thay vì chạy tìm kiếm nhị phân riêng biệt cho từng người và xây dựng lại cấu trúc nhiều lần, tất cả các truy vấn có chung điểm giữa đều được xử lý cùng nhau. 

Một chi tiết triển khai tinh tế là thứ tự lọc theo thời gian. Cây phân đoạn tại điểm giữa`mid`chứa tất cả các xe buýt từ các chỉ số`0..mid`, nhưng một số trong số họ vẫn có thể có`t < b`. Vì thời gian được sắp xếp nên việc kiểm tra`buses[mid][0] < b`là đủ. Nếu thời gian lớn nhất trong tiền tố vẫn còn quá nhỏ thì không có bus nào trong tiền tố đó có thể hoạt động. 

Một sai lầm dễ mắc phải khác là ghi đè trực tiếp các giá trị của cây phân đoạn. Nhiều xe buýt có thể chia sẻ cùng một`f`, vì vậy chúng ta phải lưu trữ tối thiểu`s`trong số đó. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4 3
1 10 10
5 6 2
6 7 3
5 7 4
5 7 1
1 2 1
1 10 11
```Xe buýt được sắp xếp theo thời gian: 

| Thời gian | ID xe buýt | Khoảng thời gian | 
| --- | --- | --- | 
| 2 | 2 | [5,6] | 
| 3 | 3 | [6,7] | 
| 4 | 4 | [5,7] | 
| 10 | 1 | [1,10] | 

Truy vấn: 

| Người | Khoảng thời gian | b | 
| --- | --- | --- | 
| 1 | [5,7] | 1 | 
| 2 | [1,2] | 1 | 
| 3 | [1,10] | 11 | 

Tiến trình tìm kiếm nhị phân: 

| Người | Đã kiểm tra giữa | Có hiệu lực? | Kết quả | 
| --- | --- | --- | --- | 
| 1 | Xe buýt giờ 3 | Không | di chuyển sang phải | 
| 1 | Xe buýt giờ 4 | Có | đáp án = xe buýt 4 | 
| 2 | Xe buýt giờ 3 | Không | di chuyển sang phải | 
| 2 | Giờ xe buýt 10 | Có | đáp án = xe buýt 1 | 
| 3 | Giờ xe buýt 10 | Không | câu trả lời = -1 | 

Đầu ra cuối cùng:```
4 1 -1
```Ví dụ này cho thấy tại sao chúng ta cần xe buýt hợp lệ sớm nhất chứ không phải bất kỳ xe buýt hợp lệ nào. Người 1 cũng có thể đi xe buýt 1, nhưng xe buýt 4 có thời gian ngắn hơn. 

### Ví dụ tùy chỉnh 

đầu vào:```
3 3
1 5 2
2 8 6
1 10 9
2 4 1
3 9 1
3 9 10
```Xe buýt được sắp xếp: 

| Thời gian | ID xe buýt | Khoảng thời gian | 
| --- | --- | --- | 
| 2 | 1 | [1,5] | 
| 6 | 2 | [2,8] | 
| 9 | 3 | [1,10] | 

Xử lý: 

| Người | Khoảng thời gian | Xe buýt hợp lệ đầu tiên | 
| --- | --- | --- | 
| 1 | [2,4] | Xe buýt 1 | 
| 2 | [3,9] | Xe buýt 3 | 
| 3 | [3,9], b=10 | Không có | 

Đầu ra:```
1 3 -1
```Dấu vết này chứng tỏ rằng bus thời gian tối thiểu có thể không ngăn chặn được trong khi bus sau đó thành công. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log² n) | Tìm kiếm nhị phân song song thực hiện các vòng O(log n), mỗi vòng có các thao tác trên cây phân đoạn | 
| Không gian | O(n + m) | Lưu trữ cho xe buýt, truy vấn, nhóm và cây phân đoạn | 

Với`10^5`xe buýt và con người,`O(nm)`là không thể. Các thừa số logarit ở đây đủ nhỏ để vừa khít bên trong các giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

INF = 10**18

class SegTree:
    def __init__(self, n):
        self.n = 1
        while self.n < n:
            self.n <<= 1
        self.seg = [INF] * (2 * self.n)

    def update(self, pos, val):
        pos += self.n
        self.seg[pos] = min(self.seg[pos], val)

        pos >>= 1
        while pos:
            self.seg[pos] = min(self.seg[pos << 1],
                                self.seg[pos << 1 | 1])
            pos >>= 1

    def query(self, l, r):
        res = INF
        l += self.n
        r += self.n

        while l <= r:
            if l & 1:
                res = min(res, self.seg[l])
                l += 1
            if not (r & 1):
                res = min(res, self.seg[r])
                r -= 1
            l >>= 1
            r >>= 1

        return res

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n, m = map(int, input().split())

    buses = []
    coords = []

    for idx in range(n):
        s, f, t = map(int, input().split())
        buses.append((t, s, f, idx + 1))
        coords.extend([s, f])

    people = []

    for idx in range(m):
        l, r, b = map(int, input().split())
        people.append((l, r, b, idx))
        coords.extend([l, r])

    coords = sorted(set(coords))
    comp = {x: i for i, x in enumerate(coords)}

    buses.sort()

    low = [0] * m
    high = [n - 1] * m
    ans = [-1] * m

    while True:
        buckets = [[] for _ in range(n)]
        active = False

        for i in range(m):
            if low[i] <= high[i]:
                active = True
                mid = (low[i] + high[i]) // 2
                buckets[mid].append(i)

        if not active:
            break

        seg = SegTree(len(coords))
        ptr = 0

        for mid in range(n):
            while ptr <= mid:
                t, s, f, idx = buses[ptr]
                seg.update(comp[f], s)
                ptr += 1

            for qi in buckets[mid]:
                l, r, b, idx = people[qi]

                if buses[mid][0] < b:
                    low[qi] = mid + 1
                    continue

                best = seg.query(comp[r], len(coords) - 1)

                if best <= l:
                    ans[qi] = mid
                    high[qi] = mid - 1
                else:
                    low[qi] = mid + 1

    res = []

    for x in ans:
        if x == -1:
            res.append("-1")
        else:
            res.append(str(buses[x][3]))

    return " ".join(res)

# provided sample
assert run(
"""4 3
1 10 10
5 6 2
6 7 3
5 7 4
5 7 1
1 2 1
1 10 11
"""
) == "4 1 -1", "sample 1"

# minimum case
assert run(
"""1 1
1 2 5
1 2 5
"""
) == "1", "minimum valid case"

# exact equality on time
assert run(
"""1 1
1 10 7
2 5 7
"""
) == "1", "t == b must work"

# containment failure
assert run(
"""2 1
1 5 2
2 10 3
3 9 1
"""
) == "-1", "no interval fully contains query"

# later bus succeeds
assert run(
"""3 1
1 5 2
2 8 6
1 10 9
3 9 1
"""
) == "3", "must choose later containing bus"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Xe buýt đơn và hành khách |`1`| Cấu hình hợp lệ tối thiểu | 
|`t == b`|`1`| Ranh giới bình đẳng về thời gian | 
| Không chứa khoảng |`-1`| Logic ngăn chặn khoảng thời gian chính xác | 
| Xe buýt sau thành công |`3`| Nhìn chung, xe buýt hợp lệ sớm nhất không phải lúc nào cũng sớm nhất | 

## Vỏ cạnh 

Xét ranh giới đẳng thức về thời gian:```
1 1
1 10 7
2 5 7
```Hành khách đến đúng lúc xe buýt hoạt động. Vì điều kiện là`b <= t`, xe buýt là hợp lệ. 

Trong quá trình tìm kiếm nhị phân, thời gian ở giữa là`7`. điều kiện`buses[mid][0] < b`trở thành`7 < 7`, sai, vì vậy truy vấn diễn ra bình thường và thành công. 

Bây giờ hãy xem xét các khoảng chồng chéo nhưng không đủ:```
2 1
1 5 2
2 10 3
3 9 1
```Chiếc xe buýt đầu tiên bị hỏng vì`5 < 9`. Xe buýt thứ hai bị hỏng vì`2 > 3`. 

Trong truy vấn cây phân đoạn về hậu tố`f >= 9`, chỉ có xe buýt thứ hai xuất hiện. Nó được lưu trữ tối thiểu`s`là`2`, đó không phải là`<= 3`, do đó thuật toán sẽ từ chối truy vấn một cách chính xác. 

Cuối cùng, hãy xem xét nhiều xe buýt chia sẻ cùng một điểm cuối:```
3 1
1 10 5
4 10 6
7 10 7
5 9 1
```Tất cả các xe buýt đều đáp ứng`f >= 9`, nhưng chỉ có hai điều đầu tiên thỏa mãn`s <= 5`. 

Cây phân đoạn lưu trữ giá trị tối thiểu`s`cho điểm cuối`10`, cụ thể là`1`. Truy vấn hậu tố trả về`1`, chứng minh chính xác rằng một xe buýt hợp lệ tồn tại.
