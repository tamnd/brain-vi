---
title: "CF 474E - Trụ cột"
description: "Chúng ta có một dãy các cột được sắp xếp từ trái qua phải. Cột i có chiều cao h[i]. Một bước nhảy hợp lệ chỉ có thể đi tiếp, từ chỉ số nhỏ hơn đến chỉ số lớn hơn và chênh lệch độ cao giữa hai cột ít nhất phải là d."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "dp", "sortings", "trees"]
categories: ["algorithms"]
codeforces_contest: 474
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 271 (Div. 2)"
rating: 2000
weight: 474
solve_time_s: 121
verified: true
draft: false
---

[CF 474E - Trụ cột](https://codeforces.com/problemset/problem/474/E) 

**Xếp hạng:** 2000 
**Thẻ:** tìm kiếm nhị phân, cấu trúc dữ liệu, dp, sắp xếp, cây 
**Thời gian giải:** 2m 1s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một dãy các cột được sắp xếp từ trái qua phải. Trụ cột`i`có chiều cao`h[i]`. Một bước nhảy hợp lệ chỉ có thể đi tiếp, từ chỉ số nhỏ hơn đến chỉ số lớn hơn và chênh lệch độ cao giữa hai cột ít nhất phải là`d`. 

Nhiệm vụ là chọn một chuỗi các chỉ số trụ cột`i1 < i2 < ... < ik`sao cho mỗi cặp liên tiếp tạo thành một bước nhảy hợp lệ. Trong số tất cả các chuỗi như vậy, chúng ta cần một chuỗi có độ dài tối đa có thể và phải in các chỉ số thực tế. 

Cấu trúc ngay lập tức gợi ý một vấn đề lập trình động kiểu đường dẫn dài nhất. Nếu chúng ta định nghĩa`dp[i]`là chuỗi hợp lệ dài nhất kết thúc ở cột`i`, sau đó`dp[i] = 1 + max(dp[j])`tổng thể`j < i`thỏa mãn`|h[i] - h[j]| ≥ d`. 

Thách thức là tìm ra điều đó một cách hiệu quả nhất. 

Những ràng buộc là điều làm cho vấn đề trở nên thú vị. Có thể có tới`100000`cột và chiều cao có thể lớn bằng`10^15`. Một thuật toán bậc hai sẽ kiểm tra đại khái`10^10`cặp trong trường hợp xấu nhất, vượt xa những gì có thể chạy trong một giây. Chúng ta cần thứ gì đó xung quanh`O(n log n)`. 

Một số trường hợp đáng chú ý. 

Khi`d = 0`, mọi bước nhảy về phía trước đều được phép. Ví dụ:```
3 0
5 5 5
```Câu trả lời là cả ba trụ cột. Bất kỳ giải pháp nào vô tình loại trừ độ cao bằng nhau khi`d = 0`sẽ mất các chuyển đổi hợp lệ. 

Khi nhiều cột có cùng chiều cao và`d > 0`, việc nhảy giữa chúng bị cấm.```
4 1
7 7 7 7
```Không thể nhảy được vì mọi chênh lệch độ cao đều bằng không. Câu trả lời đúng có độ dài`1`. 

Một trường hợp tinh tế khác xảy ra khi cả chiều cao nhỏ hơn và chiều cao lớn hơn đều là giá trị tiền thân hợp lệ.```
4 3
10 1 20 8
```Đối với trụ cột`8`, cả chiều cao`1`và chiều cao`20`thỏa mãn ràng buộc vì điều kiện sử dụng sai phân tuyệt đối. Giải pháp chỉ tìm kiếm một phía của phạm vi độ cao sẽ bỏ lỡ các chuyển đổi hợp lệ. 

Giới hạn chiều cao rất lớn cũng có nghĩa là chúng ta không thể xây dựng cây phân đoạn trực tiếp trên các giá trị chiều cao. Chiều cao trước tiên phải được nén thành cấp bậc. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là lập trình động trên tất cả các cặp trụ. 

Cho phép`dp[i]`là độ dài chuỗi tối đa kết thúc ở cột`i`. Đối với mỗi trụ cột`i`, chúng tôi kiểm tra mọi trụ cột trước đó`j`. Nếu như`|h[i] - h[j]| ≥ d`, thì chúng ta có thể mở rộng chuỗi kết thúc tại`j`.```
dp[i] = max(dp[i], dp[j] + 1)
```Phép lặp này đúng vì mọi chuỗi hợp lệ đều kết thúc tại`i`phải đến từ một số tiền thân hợp lệ trước đó. Thật không may, có`O(n²)`cặp. Với`n = 100000`, điều này có nghĩa là khoảng mười tỷ so sánh. 

Điểm nghẽn đang được tìm thấy```
max(dp[j])
```trong số các cột trước đó có chiều cao thuộc một trong hai vùng:```
h[j] ≤ h[i] - d
```hoặc```
h[j] ≥ h[i] + d
```Quan sát chính là điều kiện chỉ phụ thuộc vào chiều cao chứ không phụ thuộc vào vị trí khi chúng ta đã xử lý các cột từ trái sang phải. 

Giả sử chúng ta xử lý các cột theo thứ tự chỉ mục. Khi chúng tôi đến được cột trụ`i`, tất cả các phiên bản trước hợp lệ đã được xử lý. Trong số các trụ được xử lý đó, chúng ta cần giá trị DP tối đa trong hai khoảng chiều cao. 

Điều này trở thành một vấn đề truy vấn phạm vi tối đa trên độ cao. 

Độ cao có thể được nén thành các tọa độ duy nhất được sắp xếp. Sau đó, chúng tôi duy trì cây phân đoạn được lập chỉ mục theo chiều cao được nén. Mỗi vị trí cây lưu trữ tốt nhất`(dp value, pillar index)`được nhìn thấy cho đến nay với chiều cao đó. 

Đối với trụ cột`i`, chúng tôi thực hiện hai truy vấn cây phân đoạn: 

Khoảng đầu tiên:```
(-∞, h[i] - d]
```Khoảng thứ hai:```
[h[i] + d, +∞)
```Câu trả lời tốt hơn trong hai câu trả lời đó mang lại cho câu trả lời trước đó tạo ra chuỗi dài nhất kết thúc tại`i`. 

Sau khi tính toán`dp[i]`, chúng ta cập nhật cây phân đoạn tại vị trí nén của`h[i]`. 

Điều này làm giảm công việc trên mỗi trụ cột xuống còn một vài tìm kiếm nhị phân và các thao tác trên cây phân đoạn, mang lại`O(n log n)`thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả chiều cao của cột. 
2. Phối hợp-nén độ cao bằng cách sắp xếp các giá trị riêng biệt và gán cho mỗi độ cao một vị trí trong danh sách đã sắp xếp đó. 
3. Tạo cây phân đoạn trong đó mỗi nút lưu trữ một cặp`(best_dp, pillar_index)`đại diện cho trình tự mạnh nhất trong số các cột được xử lý trong phạm vi chiều cao đó. 
4. Xử lý các trụ từ trái sang phải. 
5. Đối với chiều cao hiện tại`h[i]`, tìm tọa độ nén cuối cùng có giá trị lớn nhất`h[i] - d`. 
6. Truy vấn cây phân đoạn trên phạm vi tiền tố đó. Điều này tìm thấy người tiền nhiệm tốt nhất với chiều cao đủ nhỏ hơn. 
7. Tìm tọa độ nén đầu tiên có giá trị nhỏ nhất`h[i] + d`. 
8. Truy vấn cây phân đoạn trên phạm vi hậu tố đó. Điều này tìm thấy người tiền nhiệm tốt nhất với chiều cao đủ lớn hơn. 
9. Lấy kết quả tốt hơn từ hai truy vấn. Nếu cả hai phạm vi đều không chứa cột đã được xử lý, hãy bắt đầu một chuỗi độ dài mới`1`. 
10. Đặt```
dp[i] = predecessor_dp + 1
```và lưu trữ chỉ mục tiền thân để xây dựng lại. 

1. Cập nhật cây đoạn tại vị trí nén của`h[i]`với cặp`(dp[i], i)`nếu nó cải thiện giá trị được lưu trữ. 
2. Sau khi xử lý xong tất cả các cột, hãy tìm cột có giá trị lớn nhất`dp`. 
3. Đi ngược lại các con trỏ trước đó để xây dựng lại trình tự tối ưu. 
4. Đảo ngược danh sách đã được xây dựng lại và in nó. 

### Tại sao nó hoạt động 

Khi xử lý trụ`i`, cây phân đoạn chỉ chứa thông tin về các trụ có chỉ số nhỏ hơn, chính xác là tập hợp các trụ trước đó hợp pháp. Mọi người tiền nhiệm hợp lệ phải có chiều cao tối đa`h[i] - d`hoặc ít nhất`h[i] + d`. Hai truy vấn phạm vi kiểm tra chính xác các độ cao đó và trả về giá trị DP tối đa trong số chúng. 

Sự truy hồi giống hệt với chương trình động bậc hai, nhưng cây phân đoạn tính toán mức tối đa cần thiết một cách hiệu quả. Vì mọi trạng thái DP được tính toán từ tất cả các trạng thái tiền nhiệm hợp lệ và không có trạng thái tiền nhiệm không hợp lệ nào được xem xét, kết quả là`dp[i]`là tối ưu. Việc xây dựng lại thông qua các chỉ số tiền thân được lưu trữ mang lại một chuỗi có độ dài tối đa thực tế. 

## Giải pháp Python```python
import sys
from bisect import bisect_left, bisect_right

input = sys.stdin.readline

n, d = map(int, input().split())
h = list(map(int, input().split()))

vals = sorted(set(h))
m = len(vals)

NEG = (0, -1)

class SegmentTree:
    def __init__(self, n):
        self.n = 1
        while self.n < n:
            self.n <<= 1
        self.seg = [NEG] * (2 * self.n)

    def _better(self, a, b):
        if a[0] >= b[0]:
            return a
        return b

    def update(self, pos, value):
        p = pos + self.n
        if value[0] > self.seg[p][0]:
            self.seg[p] = value
        else:
            return

        p >>= 1
        while p:
            self.seg[p] = self._better(self.seg[p * 2],
                                       self.seg[p * 2 + 1])
            p >>= 1

    def query(self, l, r):
        if l > r:
            return NEG

        l += self.n
        r += self.n

        left_res = NEG
        right_res = NEG

        while l <= r:
            if l & 1:
                left_res = self._better(left_res, self.seg[l])
                l += 1

            if not (r & 1):
                right_res = self._better(self.seg[r], right_res)
                r -= 1

            l >>= 1
            r >>= 1

        return self._better(left_res, right_res)

st = SegmentTree(m)

dp = [0] * n
parent = [-1] * n

for i in range(n):
    best = NEG

    right_small = bisect_right(vals, h[i] - d) - 1
    if right_small >= 0:
        best = st.query(0, right_small)

    left_large = bisect_left(vals, h[i] + d)
    if left_large < m:
        cand = st.query(left_large, m - 1)
        if cand[0] > best[0]:
            best = cand

    dp[i] = best[0] + 1
    parent[i] = best[1]

    pos = bisect_left(vals, h[i])
    st.update(pos, (dp[i], i))

end = max(range(n), key=lambda x: dp[x])

answer = []
cur = end

while cur != -1:
    answer.append(cur + 1)
    cur = parent[cur]

answer.reverse()

print(len(answer))
print(*answer)
```Bước nén tọa độ sẽ chuyển đổi độ cao lên tới`10^15`thành các chỉ số giữa`0`Và`m - 1`, làm cho việc lưu trữ cây phân đoạn trở nên khả thi. 

Cây phân đoạn lưu trữ các cặp`(dp value, index)`thay vì chỉ có giá trị DP. Giá trị DP là cần thiết để tối ưu hóa, trong khi sau này cần có chỉ mục để tái thiết. 

Hai tìm kiếm nhị phân xác định hai vùng chiều cao hợp lệ.`bisect_right(vals, h[i] - d) - 1`đưa ra tọa độ nén lớn nhất vẫn thuộc khoảng thấp hơn.`bisect_left(vals, h[i] + d)`đưa ra tọa độ đầu tiên thuộc khoảng trên. 

Một lỗi phổ biến là cập nhật cây phân đoạn trước khi truy vấn nó. Làm như vậy sẽ cho phép một trụ cột sử dụng chính nó như một trụ cột trước đó. Việc xử lý nghiêm ngặt theo thứ tự từ trái sang phải và truy vấn trước khi cập nhật sẽ ngăn chặn vấn đề đó. 

Một điểm tinh tế khác là xử lý các phạm vi trống. Khi không có chiều cao nào thuộc khoảng được truy vấn, truy vấn cây phân đoạn sẽ bị bỏ qua và kết quả vẫn giữ nguyên giá trị trung tính`(0, -1)`. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5 2
1 3 6 7 4
```Chiều cao nén:```
[1, 3, 4, 6, 7]
```| tôi | chiều cao | người tiền nhiệm tốt nhất dp | dp[i] | cha mẹ | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 1 | -1 | 
| 2 | 3 | 1 | 2 | 1 | 
| 3 | 6 | 2 | 3 | 2 | 
| 4 | 7 | 3 | 4 | 3 | 
| 5 | 4 | 3 | 4 | 3 | 

Giá trị DP tối đa là`4`. Một bản tái thiết hợp lệ là:```
1 2 3 5
```Dấu vết này cho thấy cách mỗi trụ có được tiền thân tốt nhất thông qua các truy vấn theo phạm vi độ cao thay vì kiểm tra từng trụ trước đó một cách riêng lẻ. 

### Ví dụ 2 

đầu vào:```
4 1
7 7 7 7
```| tôi | chiều cao | người tiền nhiệm tốt nhất dp | dp[i] | cha mẹ | 
| --- | --- | --- | --- | --- | 
| 1 | 7 | 0 | 1 | -1 | 
| 2 | 7 | 0 | 1 | -1 | 
| 3 | 7 | 0 | 1 | -1 | 
| 4 | 7 | 0 | 1 | -1 | 

Vì chênh lệch độ cao giữa bất kỳ cặp nào đều bằng 0 nên việc nhảy không được coi là hợp pháp. Mỗi trụ cột bắt đầu trình tự riêng của nó. 

Câu trả lời cuối cùng có độ dài`1`và bất kỳ chỉ mục nào đều hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi trụ cột thực hiện một số lượng tìm kiếm nhị phân và hoạt động trên cây phân đoạn không đổi | 
| Không gian | O(n) | Mảng DP, mảng cha, tọa độ nén và cây phân đoạn | 

Với`n = 100000`, MỘT`O(n log n)`giải pháp chỉ thực hiện vài triệu thao tác, phù hợp thoải mái trong thời gian giới hạn. Việc sử dụng bộ nhớ là tuyến tính và thấp hơn nhiều so với mức 256 MB có sẵn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from bisect import bisect_left, bisect_right

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n, d = map(int, input().split())
    h = list(map(int, input().split()))

    vals = sorted(set(h))
    m = len(vals)

    NEG = (0, -1)

    class SegmentTree:
        def __init__(self, n):
            self.n = 1
            while self.n < n:
                self.n <<= 1
            self.seg = [NEG] * (2 * self.n)

        def better(self, a, b):
            return a if a[0] >= b[0] else b

        def update(self, pos, value):
            p = pos + self.n
            if value[0] <= self.seg[p][0]:
                return
            self.seg[p] = value
            p >>= 1
            while p:
                self.seg[p] = self.better(
                    self.seg[p * 2],
                    self.seg[p * 2 + 1]
                )
                p >>= 1

        def query(self, l, r):
            if l > r:
                return NEG
            l += self.n
            r += self.n
            res = NEG
            while l <= r:
                if l & 1:
                    res = self.better(res, self.seg[l])
                    l += 1
                if not (r & 1):
                    res = self.better(res, self.seg[r])
                    r -= 1
                l >>= 1
                r >>= 1
            return res

    st = SegmentTree(m)

    dp = [0] * n
    par = [-1] * n

    for i in range(n):
        best = NEG

        p = bisect_right(vals, h[i] - d) - 1
        if p >= 0:
            best = st.query(0, p)

        q = bisect_left(vals, h[i] + d)
        if q < m:
            cand = st.query(q, m - 1)
            if cand[0] > best[0]:
                best = cand

        dp[i] = best[0] + 1
        par[i] = best[1]

        st.update(bisect_left(vals, h[i]), (dp[i], i))

    ans = max(dp)
    return str(ans)

# provided sample
assert run("5 2\n1 3 6 7 4\n") == "4"

# minimum size
assert run("1 5\n10\n") == "1"

# all equal heights, d > 0
assert run("4 1\n7 7 7 7\n") == "1"

# d = 0, every jump allowed
assert run("5 0\n2 2 2 2 2\n") == "5"

# alternating valid jumps
assert run("5 10\n1 20 2 30 3\n") == "5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 5 / 10`|`1`| Kích thước đầu vào tối thiểu | 
|`4 1 / 7 7 7 7`|`1`| Chiều cao bằng nhau với các bước nhảy bị cấm | 
|`5 0 / 2 2 2 2 2`|`5`| Trường hợp đặc biệt mà mọi bước nhảy đều hợp pháp | 
|`5 10 / 1 20 2 30 3`|`5`| Xích dài sử dụng cả phạm vi chiều cao thấp và cao | 

## Vỏ cạnh 

Hãy xem xét:```
4 1
7 7 7 7
```Đối với mỗi cột, phạm vi chiều cao hợp lệ là`≤ 6`Và`≥ 8`. Cả hai phạm vi đều không chứa bất kỳ trụ cột được xử lý nào. Cả hai truy vấn cây phân đoạn đều trả về giá trị trung tính, do đó mọi truy vấn`dp[i]`trở thành`1`. Thuật toán báo cáo chính xác rằng không thể nhảy được. 

Coi như:```
5 0
2 2 2 2 2
```Bây giờ mọi trụ cột trước đó đều là tiền thân hợp lệ. Khoảng thấp hơn trở thành`≤ 2`, vốn đã chứa tất cả các cột được xử lý. Mỗi trụ cột mới mở rộng chuỗi tốt nhất được tìm thấy cho đến nay, tạo ra các giá trị DP`1, 2, 3, 4, 5`. Câu trả lời là trình tự đầy đủ. 

Coi như:```
4 3
10 1 20 8
```Về chiều cao`8`, những người tiền nhiệm hợp lệ đều có chiều cao`1`và chiều cao`20`, vì điều kiện dựa trên sự khác biệt tuyệt đối. Thuật toán thực hiện hai truy vấn phạm vi riêng biệt, một cho độ cao nhỏ hơn và một cho độ cao lớn hơn, sau đó đưa ra kết quả tốt hơn. Điều này tránh được lỗi phổ biến khi chỉ xem xét một phía của phổ chiều cao. 

Coi như:```
3 1000000000
1 1000000000000000 2
```Độ cao là rất lớn, nhưng nén phối hợp ánh xạ chúng tới một phạm vi chỉ số nhỏ. Cây phân đoạn không bao giờ phụ thuộc vào giá trị độ cao thô, do đó thuật toán xử lý toàn bộ`10^15`phạm vi mà không tăng mức sử dụng bộ nhớ.
