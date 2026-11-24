---
title: "CF 914D - Bash và một câu đố toán học khó"
description: "Chúng tôi duy trì một mảng hỗ trợ hai loại hoạt động. Thao tác đầu tiên hỏi về phân đoạn [l, r] và giá trị x. Chúng tôi muốn biết liệu có thể sửa đổi nhiều nhất một phần tử bên trong phân đoạn đó để gcd của toàn bộ phân khúc trở thành chính xác x hay không."
date: "2026-06-13T01:23:46+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 914
codeforces_index: "D"
codeforces_contest_name: "Codecraft-18 and Codeforces Round 458 (Div. 1 + Div. 2, combined)"
rating: 1900
weight: 914
solve_time_s: 240
verified: true
draft: false
---

[CF 914D - Bash và một câu đố toán học hóc búa](https://codeforces.com/problemset/problem/914/D) 

**Xếp hạng:** 1900 
**Tas:** cấu trúc dữ liệu, lý thuyết số 
**Thời gian giải:** 4 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi duy trì một mảng hỗ trợ hai loại hoạt động. 

Thao tác đầu tiên hỏi về một phân đoạn`[l, r]`và một giá trị`x`. Chúng tôi muốn biết liệu có thể sửa đổi tối đa một phần tử bên trong phân đoạn đó để gcd của toàn bộ phân khúc trở nên chính xác hay không.`x`. 

Thao tác thứ hai cập nhật vĩnh viễn một vị trí mảng. 

Thách thức là cả kích thước mảng và số lượng truy vấn đều rất lớn. Mảng chứa tối đa`5 · 10^5`các phần tử và có thể có tới`4 · 10^5`hoạt động. Bất kỳ giải pháp nào quét toàn bộ phân đoạn cho mọi truy vấn đều không thể thực hiện được ngay lập tức. Độ dài đoạn trường hợp xấu nhất là`5 · 10^5`, do đó, việc quét tuyến tính cho mỗi truy vấn sẽ yêu cầu khoảng`2 · 10^11`hoạt động. 

Quan sát quan trọng được ẩn giấu bên trong cụm từ "thay đổi tối đa một phần tử". 

Giả sử chúng ta muốn gcd cuối cùng của phân khúc là`x`. Mọi phần tử không thay đổi khi đó phải chia hết cho`x`, vì gcd chia mọi phần tử của phân đoạn. Nếu hai vị trí khác nhau không chia hết cho`x`, một sửa đổi không thể khắc phục được cả hai. Điều này có nghĩa là một truy vấn hợp lệ chính xác khi phân đoạn chứa tối đa một phần tử không chia hết cho`x`. 

Một vài trường hợp cạnh rất dễ bị bỏ sót. 

Coi như:```
3
6 12 18
1
1 1 3 6
```Mỗi phần tử đều chia hết cho`6`, vậy câu trả lời là`YES`. Chúng tôi không cần phải sửa đổi bất cứ điều gì. Cụm từ "nhiều nhất một thay đổi" bao gồm không có thay đổi nào. 

Bây giờ hãy xem xét:```
3
6 10 15
1
1 1 3 5
```Chỉ một`6`không chia hết cho`5`. Sau khi thay đổi phần tử đơn lẻ đó thành`5`, đoạn này có thể trở thành`[5,10,15]`, gcd của ai là`5`. Câu trả lời là`YES`. 

Cuối cùng:```
3
6 10 14
1
1 1 3 5
```Cả hai`6`Và`14`không chia hết cho`5`. Một sửa đổi không thể làm cho cả hai chia hết cho`5`, vậy câu trả lời là`NO`. 

Một lỗi phổ biến là kiểm tra xem đoạn gcd hiện tại có thể được chuyển đổi thành`x`. Điều kiện thực tế phụ thuộc vào số lượng phần tử không chia hết cho`x`, không chỉ trên gcd hiện tại. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ xử lý một truy vấn`(l, r, x)`bằng cách quét từng phần tử trong đoạn đó và đếm xem có bao nhiêu phần tử không chia hết cho`x`. 

Logic là đúng. Nếu số đếm nhiều nhất là một, hãy trả lời`YES`; nếu không hãy trả lời`NO`. 

Vấn đề là tốc độ. Một đoạn có thể chứa`5 · 10^5`các phần tử và có thể có`4 · 10^5`truy vấn. Trong trường hợp xấu nhất, điều này trở nên đại khái`2 · 10^11`kiểm tra tính chia hết. 

Chúng ta cần một cách để đếm các phần tử "xấu" nhanh hơn nhiều. 

Quan sát quan trọng là sự chia hết cho`x`có thể được phát hiện thông qua gcds. Đối với bất kỳ phạm vi nào, nếu phạm vi gcd chia hết cho`x`, thì mọi phần tử trong phạm vi đó đều chia hết cho`x`. 

Điều này gợi ý một phạm vi lưu trữ cây phân đoạn gcds. 

Giả sử chúng ta kiểm tra một đoạn`[l, r]`. 

Nếu như`gcd(l, r)`chia hết cho`x`, thì mọi phần tử trong đoạn đều chia hết cho`x`, và câu trả lời là ngay lập tức`YES`. 

Ngược lại, ở đâu đó bên trong đoạn đó tồn tại một phần tử không chia hết cho`x`. 

Thay vì kiểm tra từng phần tử, chúng ta đi xuống cây phân đoạn. Bất cứ khi nào gcd của một nút chia hết cho`x`, chúng ta biết toàn bộ khoảng của nó là sạch và có thể bỏ qua. Chúng ta chỉ tiếp tục vào các nút có gcd không chia hết cho`x`. 

Chúng tôi dừng lại ngay khi phát hiện ra nhiều yếu tố vi phạm. 

Điều này biến vấn đề thành việc tìm xem số phần tử không chia hết cho`x`nhiều nhất là một. Cây phân đoạn cho phép chúng ta xác định vị trí các phần tử đó một cách hiệu quả vì các khoảng thời gian sạch lớn sẽ bị loại bỏ ngay lập tức. 

Cập nhật là cập nhật điểm cây phân đoạn tiêu chuẩn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) mỗi truy vấn | O(1) | Quá chậm | 
| Cây phân đoạn tối ưu | O(log2 n) cho mỗi truy vấn, cập nhật O(log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng cây phân đoạn trong đó mỗi nút lưu trữ gcd trong khoảng của nó. 
2. Đối với truy vấn cập nhật`(i, y)`, cập nhật lá tương ứng với vị trí`i`và tính toán lại các giá trị gcd trên đường trở về thư mục gốc. 
3. Đối với truy vấn kiểm tra`(l, r, x)`, xác định xem đoạn có chứa nhiều nhất một phần tử không chia hết cho`x`. 
4. Duyệt cây đoạn theo cách đệ quy. 
5. Nếu khoảng nút hiện tại không giao nhau`[l, r]`, trả về 0 phần tử xấu. 
6. Nếu nút hiện tại được chứa đầy đủ trong`[l, r]`và gcd được lưu trữ của nó chia hết cho`x`, trả về 0 phần tử xấu. 

Mọi phần tử trong khoảng đó đều chia hết cho`x`, vì vậy không có phần tử vi phạm nào có thể tồn tại ở đó. 
7. Nếu nút hiện tại đại diện cho một vị trí duy nhất và gcd của nó không chia hết cho`x`, trả về một phần tử xấu. 
8. Nếu không thì chia thành 2 con và tiếp tục tìm kiếm. 
9. Liên tục đếm các phần tử xấu. Khi số lượng vượt quá một, hãy ngừng khám phá và trả về bất kỳ giá trị nào lớn hơn một. 
10. Sau khi duyệt xong, hãy trả lời`YES`nếu số lượng nhiều nhất là một, nếu không thì trả lời`NO`. 

### Tại sao nó hoạt động 

Đối với bất kỳ khoảng nào, nếu gcd của nó chia hết cho`x`, thì mọi phần tử trong khoảng đó đều chia hết cho`x`. Khoảng như vậy không thể chứa phần tử xấu và có thể bị bỏ qua hoàn toàn. 

Bất cứ khi nào một nút gcd không chia hết cho`x`, ít nhất một phần tử trong khoảng đó không chia hết cho`x`, do đó việc đi xuống phần tử con của nó là cần thiết. 

Phép đệ quy cuối cùng sẽ đạt được chính xác những lá có giá trị không chia hết cho`x`. Thuật toán đếm chúng và dừng lại khi tìm thấy nhiều hơn một. 

Một phân đoạn có thể được chuyển đổi để có gcd`x`sử dụng nhiều nhất một sửa đổi khi và chỉ khi có nhiều nhất một phần tử không chia hết cho`x`. Nếu có hai hoặc nhiều phần tử như vậy thì mỗi phần tử phải được sửa đổi, điều này là không thể. Nếu không có hoặc có một phần tử như vậy, việc thay đổi phần tử đơn lẻ đó một cách thích hợp luôn cho phép toàn bộ phân đoạn gcd trở thành`x`. 

## Giải pháp Python```python
import sys
from math import gcd

input = sys.stdin.readline

class SegmentTree:
    def __init__(self, arr):
        self.n = len(arr)
        self.seg = [0] * (4 * self.n)
        self._build(1, 0, self.n - 1, arr)

    def _build(self, node, left, right, arr):
        if left == right:
            self.seg[node] = arr[left]
            return

        mid = (left + right) // 2
        self._build(node * 2, left, mid, arr)
        self._build(node * 2 + 1, mid + 1, right, arr)
        self.seg[node] = gcd(
            self.seg[node * 2],
            self.seg[node * 2 + 1]
        )

    def update(self, node, left, right, idx, value):
        if left == right:
            self.seg[node] = value
            return

        mid = (left + right) // 2

        if idx <= mid:
            self.update(node * 2, left, mid, idx, value)
        else:
            self.update(node * 2 + 1, mid + 1, right, idx, value)

        self.seg[node] = gcd(
            self.seg[node * 2],
            self.seg[node * 2 + 1]
        )

    def count_bad(self, node, left, right, ql, qr, x):
        if left > qr or right < ql:
            return 0

        if self.seg[node] % x == 0:
            return 0

        if left == right:
            return 1

        mid = (left + right) // 2

        res = self.count_bad(
            node * 2,
            left,
            mid,
            ql,
            qr,
            x
        )

        if res > 1:
            return res

        res += self.count_bad(
            node * 2 + 1,
            mid + 1,
            right,
            ql,
            qr,
            x
        )

        return min(res, 2)

def solve():
    n = int(input())
    arr = list(map(int, input().split()))

    st = SegmentTree(arr)

    q = int(input())
    ans = []

    for _ in range(q):
        query = list(map(int, input().split()))

        if query[0] == 1:
            _, l, r, x = query
            bad = st.count_bad(1, 0, n - 1, l - 1, r - 1, x)
            ans.append("YES" if bad <= 1 else "NO")
        else:
            _, idx, value = query
            st.update(1, 0, n - 1, idx - 1, value)

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Cây phân đoạn lưu trữ một giá trị cho mỗi khoảng, cụ thể là gcd của nó. Việc xây dựng cây mất thời gian tuyến tính. 

Hoạt động cập nhật là hoàn toàn tiêu chuẩn. Chúng ta sửa đổi một lá và tính toán lại các giá trị gcd trong khi quay về gốc. 

Phần thú vị là`count_bad`. Bất cứ khi nào một nút gcd chia hết cho`x`, toàn bộ khoảng được biết là sạch, do đó đệ quy dừng ngay lập tức. Việc cắt tỉa này là điều làm cho giải pháp được thực hiện nhanh chóng. 

Giá trị trả về được giới hạn ở`2`. Truy vấn chỉ quan tâm liệu số lượng phần tử vi phạm có`0`,`1`, hoặc ít nhất`2`. Không có lý do gì để tính nhiều hơn thế. 

Tất cả các chỉ số trong cây đều dựa trên 0, trong khi đầu vào là dựa trên một. Sự chuyển đổi`l - 1`,`r - 1`, Và`idx - 1`là điều cần thiết. 

Số nguyên Python dễ dàng xử lý các giá trị lên tới`10^9`, vì vậy tràn không bao giờ là một mối quan tâm. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3
2 6 3
4
1 1 2 2
1 1 3 3
2 1 9
1 1 3 2
```Truy vấn đầu tiên:`(1,2,2)`. 

| Khoảng thời gian | GCD | Chia hết cho 2? | Hành động | 
| --- | --- | --- | --- | 
| [1,2] | 2 | Có | Dừng lại | 
| Số xấu | 0 | | CÓ | 

Truy vấn thứ hai:`(1,3,3)`. 

| Khoảng thời gian | GCD | Chia hết cho 3? | Hành động | 
| --- | --- | --- | --- | 
| [1,3] | 1 | Không | Đi xuống | 
| [1,2] | 2 | Không | Đi xuống | 
| [1,1] | 2 | Không | Yếu tố xấu | 
| [2,2] | 6 | Có | Sạch | 
| [3,3] | 3 | Có | Sạch | 

Số xấu bằng`1`, vậy câu trả lời là`YES`. 

Sau khi cập nhật vị trí`1`ĐẾN`9`, mảng trở thành`[9,6,3]`. 

Truy vấn thứ ba:`(1,3,2)`. 

| Khoảng thời gian | GCD | Chia hết cho 2? | Hành động | 
| --- | --- | --- | --- | 
| [1,3] | 3 | Không | Đi xuống | 
| [1,1] | 9 | Không | Xấu | 
| [3,3] | 3 | Không | Xấu | 

Có ít nhất hai yếu tố xấu tồn tại nên câu trả lời là`NO`. 

Ví dụ này cho thấy tại sao chúng ta chỉ cần đếm các phần tử không chia hết cho`x`. 

### Ví dụ tùy chỉnh 

đầu vào:```
4
10 15 20 25
1
1 1 4 5
```| Khoảng thời gian | GCD | Chia hết cho 5? | Hành động | 
| --- | --- | --- | --- | 
| [1,4] | 5 | Có | Dừng lại | 

Khoảng nghiệm đã chứng tỏ rằng mọi phần tử đều chia hết cho`5`. 

Số xấu là`0`, vậy câu trả lời là`YES`. 

Điều này thể hiện trường hợp cắt bớt mạnh nhất, trong đó truy vấn kết thúc ở gốc mà không đi xuống. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log2 n) cho mỗi truy vấn loại 1, O(log n) cho mỗi lần cập nhật | Việc duyệt cây phân đoạn chỉ truy cập một số nút có liên quan theo logarit | 
| Không gian | O(n) | Lưu trữ cây phân đoạn | 

Với`n ≤ 5 · 10^5`Và`q ≤ 4 · 10^5`, các giới hạn này vừa vặn thoải mái trong giới hạn. Giải pháp Codeforces được chấp nhận sử dụng chính xác chiến lược gcd cây phân đoạn này. 

## Trường hợp thử nghiệm```python
import sys
import io
from math import gcd

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    class SegmentTree:
        def __init__(self, arr):
            self.n = len(arr)
            self.seg = [0] * (4 * self.n)
            self.build(1, 0, self.n - 1, arr)

        def build(self, p, l, r, arr):
            if l == r:
                self.seg[p] = arr[l]
                return
            m = (l + r) // 2
            self.build(p * 2, l, m, arr)
            self.build(p * 2 + 1, m + 1, r, arr)
            self.seg[p] = gcd(self.seg[p * 2], self.seg[p * 2 + 1])

        def update(self, p, l, r, idx, val):
            if l == r:
                self.seg[p] = val
                return
            m = (l + r) // 2
            if idx <= m:
                self.update(p * 2, l, m, idx, val)
            else:
                self.update(p * 2 + 1, m + 1, r, idx, val)
            self.seg[p] = gcd(self.seg[p * 2], self.seg[p * 2 + 1])

        def query(self, p, l, r, ql, qr, x):
            if l > qr or r < ql:
                return 0
            if self.seg[p] % x == 0:
                return 0
            if l == r:
                return 1
            m = (l + r) // 2
            res = self.query(p * 2, l, m, ql, qr, x)
            if res > 1:
                return res
            res += self.query(p * 2 + 1, m + 1, r, ql, qr, x)
            return min(res, 2)

    n = int(input())
    arr = list(map(int, input().split()))
    st = SegmentTree(arr)

    q = int(input())
    out = []

    for _ in range(q):
        t = list(map(int, input().split()))
        if t[0] == 1:
            _, l, r, x = t
            out.append(
                "YES"
                if st.query(1, 0, n - 1, l - 1, r - 1, x) <= 1
                else "NO"
            )
        else:
            _, i, y = t
            st.update(1, 0, n - 1, i - 1, y)

    return "\n".join(out)

# sample 1
assert run(
"""3
2 6 3
4
1 1 2 2
1 1 3 3
2 1 9
1 1 3 2
"""
) == "YES\nYES\nNO"

# minimum size
assert run(
"""1
7
1
1 1 1 7
"""
) == "YES"

# one bad element
assert run(
"""3
6 10 15
1
1 1 3 5
"""
) == "YES"

# two bad elements
assert run(
"""3
6 10 14
1
1 1 3 5
"""
) == "NO"

# update affects answer
assert run(
"""2
6 12
3
1 1 2 6
2 2 7
1 1 2 6
"""
) == "YES\nYES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mảng phần tử đơn | CÓ | Ranh giới tối thiểu | 
| 6 10 15 với x=5 | CÓ | Chính xác là một yếu tố vi phạm | 
| 6 10 14 với x=5 | KHÔNG | Hai yếu tố vi phạm | 
| Cập nhật rồi truy vấn | CÓ CÓ | Tuyên truyền cập nhật chính xác | 

## Vỏ cạnh 

Hãy xem xét:```
1
7
1
1 1 1 7
```Phần tử duy nhất đã chia hết cho`7`. Nút gốc gcd là`7`, chia hết cho`7`, do đó thuật toán ngay lập tức trả về 0 phần tử xấu và câu trả lời`YES`. 

Coi như:```
3
6 10 15
1
1 1 3 5
```Chỉ có giá trị`6`không chia hết cho`5`. Phép đệ quy cuối cùng xác định chính xác một lá xấu. Vì số đếm là`1`, thuật toán trả lời`YES`. Chúng ta có thể thay đổi phần tử đó thành`5`, làm cho đoạn gcd bằng`5`. 

Coi như:```
3
6 10 14
1
1 1 3 5
```Cả hai`6`Và`14`không chia hết được cho`5`. Quá trình đệ quy tìm thấy một lá xấu, tiếp tục, tìm lá xấu thứ hai và dừng lại. Số lượng vượt quá một, vì vậy câu trả lời là`NO`. 

Hãy xem xét trường hợp nhạy cảm với bản cập nhật:```
2
6 12
3
1 1 2 6
2 2 7
1 1 2 6
```Truy vấn đầu tiên thành công vì cả hai giá trị đều chia hết cho`6`. Sau khi cập nhật, mảng trở thành`[6,7]`. Chỉ có một phần tử không chia hết cho`6`, vì vậy truy vấn thứ hai vẫn thành công. Bản cập nhật cây phân đoạn truyền bá chính xác thông tin gcd mới, cho phép các truy vấn tiếp theo sử dụng trạng thái mảng đã sửa đổi.
