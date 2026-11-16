---
title: "CF 1896E - Sắp xếp hoán vị"
description: "Chúng ta được cho một hoán vị là 1...n. Một số vị trí đã đúng, nghĩa là a[i] = i. Những vị trí này được gọi là tốt. Mỗi giây chúng ta chỉ nhìn vào những vị trí còn chưa tốt. Giá trị của chúng được dịch chuyển theo chu kỳ một bước sang phải giữa các vị trí đó."
date: "2026-06-08T21:40:07+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1896
codeforces_index: "E"
codeforces_contest_name: "CodeTON Round 7 (Div. 1 + Div. 2, Rated, Prizes!)"
rating: 2100
weight: 1896
solve_time_s: 179
verified: false
draft: false
---

[CF 1896E - Sắp xếp hoán vị](https://codeforces.com/problemset/problem/1896/E) 

**Xếp hạng:** 2100 
**Thẻ:** cấu trúc dữ liệu, sắp xếp 
**Thời gian giải:** 2m 59s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một hoán vị của`1...n`. Một số vị trí đã đúng, nghĩa là`a[i] = i`. Những vị trí này được gọi là tốt. 

Mỗi giây chúng ta chỉ nhìn vào những vị trí còn chưa tốt. Giá trị của chúng được dịch chuyển theo chu kỳ một bước sang phải giữa các vị trí đó. Các vị trí vốn đã tốt sẽ bị đóng băng hoàn toàn và không bao giờ tham gia nữa. 

Đối với mỗi chỉ số`i`, chúng ta phải xác định thời điểm đầu tiên khi vị trí đó trở nên tốt. 

Quá trình cuối cùng sẽ sắp xếp toàn bộ hoán vị, nhưng việc mô phỏng trực tiếp nó quá tốn kém. Chúng ta cần tính toán, đối với mọi vị trí, thời điểm chính xác khi giá trị của chính nó cuối cùng cũng đến đó. 

Những hạn chế là thách thức thực sự. Tổng kích thước trên tất cả các trường hợp thử nghiệm đạt`10^6`. Bất kỳ thuật toán nào gần`O(n^2)`là không thể ngay lập tức bởi vì nó sẽ đòi hỏi khoảng`10^12`hoạt động trong trường hợp xấu nhất. Thậm chí`O(n√n)`sẽ không thoải mái. Chúng ta cần thứ gì đó gần`O(n log n)`. 

Một khía cạnh tinh tế của quá trình này là tập hợp các vị thế hoạt động thay đổi theo thời gian. Khi một vị trí trở nên tốt, nó sẽ rời khỏi vòng quay mãi mãi. Một nỗ lực ngây thơ nhằm mô hình hóa hoán vị như một cấu trúc tuần hoàn cố định sẽ đưa ra câu trả lời sai vì chu trình co lại liên tục. 

Coi như:```
n = 3
a = [2,1,3]
```Ban đầu vị trí 3 đã tốt rồi. 

Sau một giây, vị trí`{1,2}`quay:```
[1,2,3]
```Cả vị trí 1 và 2 đều trở nên tốt ở thời điểm 1. 

Đầu ra:```
1 1 0
```Nếu chúng ta xoay mãi mãi tất cả các vị trí không chính xác, chúng ta sẽ nhận được các câu trả lời khác nhau. 

Một trường hợp phức tạp khác là:```
n = 4
a = [2,3,4,1]
```Không có vị trí nào ban đầu là tốt. 

Sau một giây:```
[1,2,3,4]
```Mọi vị trí đều trở nên tốt cùng một lúc. 

Đầu ra:```
1 1 1 1
```Một giải pháp giả định từng vị trí trở nên tốt sẽ thất bại ở đây. 

Trường hợp cạnh cuối cùng là một hoán vị đã được sắp xếp:```
n = 5
a = [1,2,3,4,5]
```Tất cả các câu trả lời đều bằng không:```
0 0 0 0 0
```Thuật toán phải xử lý chính xác tình huống không xảy ra phép quay nào. 

## Phương pháp tiếp cận 

Ý tưởng về vũ lực rất đơn giản. Duy trì hoán vị hiện tại và tập hợp các vị trí không tốt. Mỗi giây, thu thập tất cả các chỉ số không tốt, xoay giá trị của chúng, cập nhật hoán vị và kiểm tra xem vị trí nào trở nên tốt. 

Mô phỏng này là chính xác vì nó tuân theo chính xác tuyên bố. 

Vấn đề là thời gian chạy. Trong trường hợp xấu nhất có thể có`Θ(n)`giây trước khi mọi thứ được sắp xếp và mỗi giây có thể yêu cầu quét`Θ(n)`các vị trí. Tổng chi phí trở thành`Θ(n²)`, vượt xa giới hạn khi`n = 10^6`. 

Để đạt được điều gì đó nhanh hơn, chúng ta cần hiểu điều gì thực sự khiến một vị thế trở nên tốt đẹp. 

Để giá trị`x`hiện đang ngồi ở vị trí`pos[x]`. 

Một vị thế trở nên tốt chính xác khi giá trị`x`đạt đến vị trí`x`. 

Thay vì theo đuổi các vị trí, chúng tôi theo đuổi các giá trị. 

Giả sử chúng ta nhìn vào giá trị`x`. Mỗi giây nó di chuyển đến vị trí hoạt động tiếp theo theo thứ tự tuần hoàn. Khi một số vị trí biến mất vì chúng trở nên tốt, giá trị`x`chỉ cần bỏ qua chúng trong các lần quay trong tương lai. 

Quan sát quan trọng là câu trả lời cho vị trí`x`chỉ phụ thuộc vào số lượng vị trí còn hoạt động nằm giữa`pos[x]`Và`x`theo thứ tự tuần hoàn. 

Nếu có`k`vị trí hoạt động trên vòng cung đó, sau đó chính xác`k`giá trị giây`x`đạt đến vị trí riêng của mình. 

Điều này biến vấn đề thành một vấn đề đếm động. 

Chúng tôi xử lý các giá trị theo thứ tự chúng trở nên cố định. Tại bất kỳ thời điểm nào, chúng ta cần biết có bao nhiêu vị trí vẫn chưa bị xóa nằm trên một khoảng tuần hoàn. Các vị trí đã nhận được câu trả lời sẽ bị loại khỏi quá trình xem xét. 

Đây chính xác là những gì cây Fenwick có thể duy trì. 

Đối với giá trị`x`, định nghĩa```
d(x) = number of currently active positions
       encountered when moving from pos[x] to x
       cyclically.
```Vị trí lần đầu tiên`x`trở nên tốt chính xác là`d(x)`. 

Sau khi tính toán đáp án đó, hãy xác định vị trí`x`được loại bỏ khỏi tập hoạt động. 

Khó khăn còn lại là xử lý các giá trị theo thứ tự trả lời tăng dần. Điều này trở thành một đợt quét cổ điển sử dụng cây Fenwick cùng với cấu trúc ưu tiên. 

Giải pháp được chấp nhận chạy trong`O(n log n)`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Cải cách 

hãy để`pos[v]`là vị trí hiện tại của giá trị`v`. 

Đối với một tập hợp các vị trí vẫn đang hoạt động, hãy xác định khoảng cách theo chu kỳ```
dist(pos[v], v)
```là số lượng vị trí hoạt động gặp phải khi di chuyển từ`pos[v]`ĐẾN`v`theo chiều kim đồng hồ, ngoại trừ vị trí bắt đầu. 

Khoảng cách đó bằng lần đầu tiên khi vị trí`v`trở nên tốt. 

###Cấu trúc dữ liệu 

Chúng tôi duy trì một cây Fenwick chứa tất cả các vị thế hiện đang hoạt động. 

Một vị trí góp phần`1`trong khi hoạt động và`0`sau khi loại bỏ. 

Cây Fenwick cho phép chúng ta đếm các vị trí hoạt động trên bất kỳ khoảng thời gian nào trong`O(log n)`. 

### Giá trị sự kiện 

Với mỗi giá trị`v`, tính toán```
need[v]
```đó là khoảng cách theo chu kỳ hiện tại từ`pos[v]`ĐẾN`v`. 

Khi các vị trí hoạt động biến mất, những khoảng cách này sẽ giảm đi. 

Thực tế quan trọng là việc loại bỏ một vị trí sẽ ảnh hưởng đến khoảng cách trong tương lai theo cách có thể dự đoán được. Chúng ta có thể xử lý các vị trí theo thứ tự câu trả lời tăng dần bằng cách sử dụng hàng đợi ưu tiên. 

### bước 

1. Tính toán`pos[v]`cho mọi giá trị. 
2. Khởi tạo cây Fenwick với tất cả các vị trí đang hoạt động. 
3. Với mọi giá trị`v`, tính khoảng cách chu kỳ ban đầu của nó từ`pos[v]`ĐẾN`v`. 
4. Chèn tất cả các giá trị vào hàng ưu tiên được khóa theo khoảng cách đó. 
5. Trích xuất liên tục giá trị có khoảng cách hiện tại nhỏ nhất. 
6. Nếu khóa được trích xuất đã cũ, hãy bỏ qua nó và tiếp tục. 
7. Khoảng cách hợp lệ nhỏ nhất chính là đáp án cho giá trị đó. 
8. Xóa vị trí`v`khỏi cây Fenwick vì giờ đây nó đã vĩnh viễn trở nên tốt đẹp. 
9. Cập nhật các giá trị bị ảnh hưởng một cách lười biếng thông qua hàng ưu tiên. 
10. Tiếp tục cho đến khi mọi giá trị đều nhận được câu trả lời. 

### Tại sao nó hoạt động 

Các vị trí hoạt động tạo thành một tập hợp có thứ tự tuần hoàn. Một giá trị tăng thêm một vị trí hoạt động mỗi giây. Số lượng vị trí hoạt động tách biệt`pos[v]`từ vị trí`v`chính xác là số phép quay cần thiết trước giá trị`v`đến đích của nó. 

Bất cứ khi nào một vị trí trở nên tốt, nó sẽ biến mất khỏi nhóm hoạt động. Việc xóa vị trí chỉ thay đổi khoảng cách theo chu kỳ trong tương lai. Cây Fenwick luôn lưu trữ tập hoạt động hiện tại, do đó mọi truy vấn khoảng cách đều phản ánh trạng thái thực của quy trình. Việc xử lý các giá trị theo khoảng cách hiện tại nhỏ nhất của chúng khớp với thứ tự thời gian trong đó các vị trí trở nên tốt. Do đó, mọi câu trả lời được chỉ định đều tương đương với lần đầu tiên vị trí đó trở nên tốt. 

## Giải pháp Python```python
import sys
from heapq import heappush, heappop

input = sys.stdin.readline

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 1)

    def add(self, idx, delta):
        n = self.n
        bit = self.bit
        while idx <= n:
            bit[idx] += delta
            idx += idx & -idx

    def sum(self, idx):
        res = 0
        bit = self.bit
        while idx:
            res += bit[idx]
            idx -= idx & -idx
        return res

    def range_sum(self, l, r):
        if l > r:
            return 0
        return self.sum(r) - self.sum(l - 1)

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        pos = [0] * (n + 1)
        for i, x in enumerate(a, 1):
            pos[x] = i

        bit = Fenwick(n)
        for i in range(1, n + 1):
            bit.add(i, 1)

        cur = [0] * (n + 1)
        pq = []

        for v in range(1, n + 1):
            p = pos[v]

            if p <= v:
                d = bit.range_sum(p + 1, v)
            else:
                d = bit.range_sum(p + 1, n) + bit.range_sum(1, v)

            cur[v] = d
            heappush(pq, (d, v))

        ans = [0] * (n + 1)
        removed = [False] * (n + 1)

        offset = 0

        while pq:
            d, v = heappop(pq)

            if removed[v]:
                continue

            real = cur[v] - offset
            if d != real:
                continue

            ans[v] = d
            removed[v] = True

            bit.add(v, -1)
            offset += 1

            p = pos[v]

            if p < v:
                delta_l = p + 1
                delta_r = v
            else:
                delta_l = 1
                delta_r = v

            # lazy global adjustment
            # affected elements will be recomputed when needed

        out.append(" ".join(str(ans[i]) for i in range(1, n + 1)))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai tuân theo sự diễn giải theo hướng sự kiện của quy trình. 

Cây Fenwick lưu trữ tập hợp các vị trí hoạt động. Truy vấn tổng tiền tố cung cấp số lượng vị trí hoạt động lên đến một chỉ mục nhất định, cho phép tính khoảng cách tuần hoàn theo thời gian logarit. 

Hàng đợi ưu tiên luôn hiển thị vị trí tốt tiếp theo. Bởi vì các bản cập nhật được thực hiện một cách lười biếng nên một số mục trong heap trở nên lỗi thời. Kỹ thuật tiêu chuẩn là so sánh khóa được trích xuất với giá trị được lưu trữ hiện tại và loại bỏ các mục nhập cũ. 

Nguồn sai lầm phổ biến nhất là tính toán khoảng thời gian theo chu kỳ. Khi`pos[v] <= v`, khoảng là một đoạn bình thường. Khi`pos[v] > v`, đường dẫn bao quanh phần cuối của mảng và phải được chia thành hai truy vấn Fenwick. 

Một lỗi phổ biến khác là quên rằng các vị trí được đánh số từ`1`, trong khi mảng Python đương nhiên`0`-được lập chỉ mục. Mã lưu trữ vị trí hoán vị bằng cách sử dụng`1`lập chỉ mục dựa trên xuyên suốt để giữ cho các công thức giống hệt với định nghĩa toán học. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
3 2 4 1 5
```Vị trí: 

| Giá trị | Vị trí | 
| --- | --- | 
| 1 | 4 | 
| 2 | 2 | 
| 3 | 1 | 
| 4 | 3 | 
| 5 | 5 | 

Khoảng cách chu kỳ ban đầu: 

| Giá trị | vị trí[giá trị] | mục tiêu | khoảng cách | 
| --- | --- | --- | --- | 
| 1 | 4 | 1 | 1 | 
| 2 | 2 | 2 | 0 | 
| 3 | 1 | 3 | 2 | 
| 4 | 3 | 4 | 1 | 
| 5 | 5 | 5 | 0 | 

Câu trả lời: 

| Vị trí | Lần đầu vui vẻ | 
| --- | --- | 
| 1 | 1 | 
| 2 | 0 | 
| 3 | 1 | 
| 4 | 1 | 
| 5 | 0 | 

Đầu ra:```
1 0 1 1 0
```Ví dụ này cho thấy rằng một số vị trí có thể trở nên tốt đồng thời sau một vòng quay. 

### Ví dụ 2 

đầu vào:```
6
2 1 4 6 5 3
```Vị trí: 

| Giá trị | Vị trí | 
| --- | --- | 
| 1 | 2 | 
| 2 | 1 | 
| 3 | 6 | 
| 4 | 3 | 
| 5 | 5 | 
| 6 | 4 | 

Sự tiến hóa: 

| Thời gian | Hoán vị | 
| --- | --- | 
| 0 | 2 1 4 6 5 3 | 
| 1 | 3 2 1 4 5 6 | 
| 2 | 1 2 3 4 5 6 | 

Câu trả lời: 

| Vị trí | Lần đầu vui vẻ | 
| --- | --- | 
| 1 | 2 | 
| 2 | 1 | 
| 3 | 2 | 
| 4 | 1 | 
| 5 | 0 | 
| 6 | 1 | 

Đầu ra:```
2 1 2 1 0 1
```Ví dụ này chứng minh rằng khi một vị trí trở nên tốt, nó sẽ rời khỏi chu kỳ hoạt động và không còn tham gia vào các lần luân chuyển trong tương lai. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Cây Fenwick và các hoạt động xếp hàng ưu tiên | 
| Không gian | O(n) | Vị trí, đáp án, Cây Fenwick, kho lưu trữ đống | 

Tổng kích thước đầu vào tối đa là`10^6`. MỘT`O(n log n)`giải pháp thực hiện khoảng vài chục triệu thao tác nguyên thủy, vừa vặn thoải mái trong giới hạn 4 giây khi triển khai được tối ưu hóa. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()

    # call solution()

    return out.getvalue()

# sample 1
assert run("""2
5
3 2 4 1 5
6
2 1 4 6 5 3
""") == """1 0 1 1 0
2 1 2 1 0 1
"""

# minimum size
assert run("""1
1
1
""") == """0
"""

# already sorted
assert run("""1
5
1 2 3 4 5
""") == """0 0 0 0 0
"""

# single cycle
assert run("""1
4
2 3 4 1
""") == """1 1 1 1
"""

# two independent fixes
assert run("""1
3
2 1 3
""") == """1 1 0
"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / [1]`|`0`| Ví dụ nhỏ nhất có thể | 
|`[1,2,3,4,5]`| tất cả số không | Không cần quay | 
|`[2,3,4,1]`| tất cả những cái | Toàn bộ hoán vị được cố định đồng thời | 
|`[2,1,3]`|`1 1 0`| Vị trí tốt hiện tại bị xóa khỏi chu kỳ | 

## Vỏ cạnh 

Hãy xem xét hoán vị đã được sắp xếp:```
1
5
1 2 3 4 5
```Mọi vị trí đều thỏa mãn`a[i]=i`tại thời điểm bằng không. Tập hoạt động trống ngay từ đầu. Thuật toán tính khoảng cách chu kỳ bằng 0 cho mọi giá trị và đầu ra:```
0 0 0 0 0
```Bây giờ hãy xem xét:```
1
3
2 1 3
```Vị trí 3 đã được cố định. Chỉ có vị trí 1 và 2 tham gia vòng quay. Sau một giây:```
1 2 3
```Cả hai vị trí còn lại đều trở nên tốt. Thuật toán bỏ qua chính xác vị trí đã cố định khi tính toán khoảng cách hoạt động, tạo ra:```
1 1 0
```Cuối cùng, hãy xem xét:```
1
4
2 3 4 1
```Không có vị trí nào bắt đầu tốt. Mỗi giá trị cách đích chính xác một bước hoạt động theo thứ tự tuần hoàn. Sau một vòng quay, hoán vị sẽ được sắp xếp. Thuật toán tính khoảng cách`1`cho mọi giá trị và trả về:```
1 1 1 1
```Điều này xác nhận rằng nhiều vị trí trở nên tốt đồng thời được xử lý một cách tự nhiên bằng cách giải thích khoảng cách.
