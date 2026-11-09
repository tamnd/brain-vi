---
title: "CF 1477B - Chuỗi Nezzar và nhị phân"
description: "Chúng ta bắt đầu bằng một chuỗi nhị phân mà cuối cùng phải trở thành một chuỗi nhị phân khác sau một chuỗi thao tác. Mỗi ngày, một đoạn cố định được kiểm tra. Nếu đoạn đó chứa cả 0 và 1 thì quá trình sẽ thất bại ngay lập tức."
date: "2026-06-10T23:53:23+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1477
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 698 (Div. 1)"
rating: 1900
weight: 1477
solve_time_s: 145
verified: true
draft: false
---

[CF 1477B - Chuỗi Nezzar và nhị phân](https://codeforces.com/problemset/problem/1477/B) 

**Xếp hạng:** 1900 
**Tas:** cấu trúc dữ liệu, tham lam 
**Thời gian giải:** 2m 25s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu bằng một chuỗi nhị phân mà cuối cùng phải trở thành một chuỗi nhị phân khác sau một chuỗi thao tác. 

Mỗi ngày, một đoạn cố định được kiểm tra. Nếu đoạn đó chứa cả hai`0`Và`1`, quá trình này ngay lập tức thất bại. Điều đó có nghĩa là trước mỗi lần kiểm tra, phần được kiểm tra phải được`0`s hoặc hoàn toàn`1`S. 

Sau khi kiểm tra, trong đêm, chúng tôi được phép sửa đổi ít hơn một nửa số ký tự trong cùng đoạn đó. 

Đầu vào cho chuỗi gốc`s`, chuỗi cuối cùng mong muốn`f`và trình tự các phân đoạn được kiểm tra. Chúng ta phải xác định liệu có tồn tại một số chuỗi sửa đổi hợp lệ hàng đêm làm biến đổi`s`vào trong`f`đồng thời giữ mọi phân đoạn được kiểm tra thống nhất tại thời điểm nó được kiểm tra. 

Những ràng buộc là điều đầu tiên gợi ý hướng đi dự định. Trên tất cả các trường hợp thử nghiệm, tổng chiều dài chuỗi và tổng số truy vấn đều cao nhất`2 · 10^5`. Một giải pháp chạm tới mọi vị trí của một phân đoạn cho mọi truy vấn sẽ yêu cầu khoảng`O(nq)`hoạt động, có thể đạt được khoảng`4 · 10^10`trong trường hợp xấu nhất. Điều đó hoàn toàn không thể thực hiện được. Chúng ta cần thứ gì đó xung quanh`O((n+q) log n)`. 

Khía cạnh tinh vi nhất của vấn đề là các thao tác được mô tả trước theo thời gian, nhưng chuỗi cuối cùng đã được biết trước. Việc mô phỏng chuyển tiếp rất khó vì mỗi đêm có thể có nhiều cách để sửa đổi phân đoạn. 

Một số trường hợp đặc biệt có thể gây ra suy luận sai nếu chúng ta không cẩn thận. 

Hãy xem xét một đoạn có độ dài chẵn và hiện chứa chính xác một nửa số 0 và một nửa số một.```
length = 4
segment = 0011
```Để làm cho phân đoạn thống nhất, chúng ta cần thay đổi chính xác hai ký tự. Quy định chỉ cho phép thay đổi ít hơn một nửa đoạn nên việc thay đổi hai ký tự là bị cấm. Bất kỳ giải pháp nào coi "nhiều nhất là một nửa" là hợp lệ sẽ chấp nhận những trường hợp như vậy một cách không chính xác. 

Một trường hợp nguy hiểm khác là phân khúc đã có đa số nghiêm ngặt.```
segment = 0001
```Vì ít hơn một nửa số vị trí có thể được thay đổi nên trạng thái thống nhất duy nhất có thể có trước ngày hôm trước là tất cả số không. Biến nó thành tất cả sẽ yêu cầu thay đổi ba vị trí, vượt quá giới hạn. Việc triển khai bất cẩn cho phép giá trị thống nhất sẽ là sai. 

Cuối cùng, khi không có truy vấn nào cả thì không thể thực hiện được phép biến đổi nào.```
n = 3, q = 0
s = 101
f = 111
```Câu trả lời là`NO`bởi vì chuỗi không bao giờ thay đổi. Bất kỳ thuật toán nào cũng phải xác minh rõ ràng rằng trạng thái ban đầu được xây dựng lại bằng`s`. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng mô phỏng quá trình. Bắt đầu từ`s`, đối với mỗi truy vấn, chúng tôi sẽ xác định xem phân đoạn được kiểm tra có thống nhất hay không, sau đó xem xét các sửa đổi có thể có hàng đêm. Khó khăn là có thể truy cập được nhiều chuỗi trung gian khác nhau. Khám phá tất cả các khả năng nhanh chóng trở thành cấp số nhân. 

Ngay cả khi bằng cách nào đó chúng tôi tránh được việc phân nhánh và chỉ theo dõi một trạng thái thì việc cập nhật toàn bộ phân đoạn cho mỗi truy vấn sẽ tốn kém.`O(nq)`thời gian. Với cả hai giá trị đạt`2 · 10^5`, điều đó đã vượt xa giới hạn. 

Quan sát quan trọng là chuỗi cuối cùng`f`được biết đến. Thay vì hỏi làm thế nào chúng ta có thể tiếp cận`f`, chúng tôi hỏi liệu`f`có thể phát sinh từ một chuỗi các hoạt động hợp lệ. 

Giả sử chúng ta đang xem xét một phân đoạn truy vấn`[l,r]`và chúng tôi biết chuỗi ngay sau khi sửa đổi đêm đó. Phân khúc đó có thể trông như thế nào ngay trước khi sửa đổi? 

Gọi độ dài đoạn đó là`len`và để nó hiện chứa`ones`những cái đó. 

Trước khi vận hành ban đêm, đoạn đường này phải hoàn toàn đồng nhất vì mới được kiểm tra. 

Nếu giá trị thống nhất là`0`, sau đó hoạt động ban đêm thay đổi chính xác`ones`các vị trí. 

Nếu giá trị thống nhất là`1`, sau đó hoạt động ban đêm thay đổi chính xác`len - ones`các vị trí. 

Bởi vì có ít hơn một nửa số vị trí có thể bị thay đổi nên chỉ một trong những khả năng này có thể đúng. 

Nếu như`ones < len/2`, thì đoạn đó trước đó phải có toàn số 0. 

Nếu như`ones > len/2`, thì phân đoạn trước đó phải là tất cả một. 

Nếu như`ones = len/2`, cả hai khả năng đều không hiệu quả vì cả hai đều yêu cầu thay đổi chính xác một nửa vị trí. 

Điều này hoàn toàn loại bỏ sự mơ hồ. 

Bắt đầu từ`f`, chúng tôi xử lý các truy vấn theo thứ tự ngược lại. Đối với mỗi phân đoạn, chúng tôi đếm xem nó hiện có bao nhiêu phân đoạn. 

Nếu số đếm chính xác bằng một nửa độ dài đoạn thì không thể xây dựng lại được. 

Nếu không, chúng tôi sẽ ghi đè toàn bộ phân đoạn bằng giá trị đa số bắt buộc và tiếp tục. 

Sau khi đảo ngược tất cả các truy vấn, chúng ta thu được ứng cử viên duy nhất có thể có cho chuỗi gốc. Nếu nó khớp`s`, câu trả lời là`YES`; nếu không thì`NO`. 

Thách thức còn lại là hỗ trợ các truy vấn đếm phạm vi và cập nhật gán phạm vi một cách hiệu quả. Đây là một vấn đề về cây phân đoạn lan truyền lười biếng cổ điển. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) hoặc tệ hơn | O(n) | Quá chậm | 
| Tối ưu | O((n + q) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng cây phân đoạn từ chuỗi cuối cùng`f`. 

Mỗi nút lưu trữ số lượng nút trong khoảng của nó. 
2. Xử lý các truy vấn theo thứ tự ngược lại. 

Việc đảo ngược biến đổi vấn đề từ "nhiều trạng thái có thể xảy ra trong tương lai" thành "nhiều nhất một trạng thái có thể xảy ra trước đó". 
3. Đối với phân khúc hiện tại`[l,r]`, truy vấn số lượng cái bên trong nó. 

Cho phép`len = r - l + 1`. 
4. Nếu`2 * ones == len`, quay lại ngay`NO`. 

Phân đoạn thống nhất trước đó không thể tạo ra trạng thái có chính xác nửa số một và nửa số 0 trong khi thay đổi hoàn toàn ít hơn một nửa số vị trí. 
5. Nếu`2 * ones < len`, gán toàn bộ phân đoạn về 0. 

Phân đoạn phải có toàn số 0 trước thao tác này. 
6. Nếu không thì chỉ định toàn bộ phân đoạn thành một. 

Phân đoạn này phải là tất cả các phân đoạn trước thao tác này. 
7. Tiếp tục cho đến khi tất cả các truy vấn đảo ngược được xử lý. 
8. Sau khi quá trình tái tạo kết thúc, hãy so sánh mọi vị trí với chuỗi gốc`s`. 

Nếu có bất kỳ vị trí nào khác, hãy quay lại`NO`. 
9. Nếu chuỗi được xây dựng lại khớp`s`, trở lại`YES`. 

### Tại sao nó hoạt động 

Bất biến quan trọng là trong khi xử lý các truy vấn ngược lại, cây phân đoạn luôn biểu thị trạng thái duy nhất của chuỗi ngay sau thao tác chuyển tiếp thời gian tương ứng. 

Hãy xem xét một phân đoạn truy vấn đảo ngược. Gọi số hàng đơn vị hiện tại của nó là`ones`. 

Nếu như`ones < len/2`, để có được trạng thái này từ một phân đoạn tất cả sẽ yêu cầu thay đổi hơn một nửa vị trí. Phân đoạn trước hợp lệ duy nhất là phân đoạn hoàn toàn bằng 0. 

Nếu như`ones > len/2`, đối số đối xứng cho thấy rằng phần trước hợp lệ duy nhất là phân đoạn tất cả một. 

Nếu như`ones = len/2`, cả hai ứng cử viên tiền nhiệm đều yêu cầu thay đổi chính xác một nửa số vị trí, điều này bị cấm. 

Do đó, mọi bước đảo ngược đều được xác định duy nhất hoặc không thể thực hiện được. Thuật toán xây dựng lại chính xác tập hợp các trạng thái có thể tồn tại trong một quy trình chuyển tiếp hợp lệ. Nếu việc xây dựng lại đạt đến một chuỗi khác với`s`, thì không có chuỗi hợp lệ nào bắt đầu từ`s`. Nếu việc tái thiết đạt tới`s`, việc thực hiện lại phép suy luận ngược về phía trước sẽ mang lại một chuỗi các phép toán hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class SegmentTree:
    def __init__(self, arr):
        self.n = len(arr)
        self.tree = [0] * (4 * self.n)
        self.lazy = [-1] * (4 * self.n)
        self._build(1, 0, self.n - 1, arr)

    def _build(self, node, l, r, arr):
        if l == r:
            self.tree[node] = arr[l]
            return

        mid = (l + r) // 2
        self._build(node * 2, l, mid, arr)
        self._build(node * 2 + 1, mid + 1, r, arr)
        self.tree[node] = self.tree[node * 2] + self.tree[node * 2 + 1]

    def _apply(self, node, l, r, val):
        self.tree[node] = (r - l + 1) * val
        self.lazy[node] = val

    def _push(self, node, l, r):
        if self.lazy[node] == -1 or l == r:
            return

        mid = (l + r) // 2
        val = self.lazy[node]

        self._apply(node * 2, l, mid, val)
        self._apply(node * 2 + 1, mid + 1, r, val)

        self.lazy[node] = -1

    def update(self, ql, qr, val):
        self._update(1, 0, self.n - 1, ql, qr, val)

    def _update(self, node, l, r, ql, qr, val):
        if qr < l or r < ql:
            return

        if ql <= l and r <= qr:
            self._apply(node, l, r, val)
            return

        self._push(node, l, r)

        mid = (l + r) // 2
        self._update(node * 2, l, mid, ql, qr, val)
        self._update(node * 2 + 1, mid + 1, r, ql, qr, val)

        self.tree[node] = self.tree[node * 2] + self.tree[node * 2 + 1]

    def query(self, ql, qr):
        return self._query(1, 0, self.n - 1, ql, qr)

    def _query(self, node, l, r, ql, qr):
        if qr < l or r < ql:
            return 0

        if ql <= l and r <= qr:
            return self.tree[node]

        self._push(node, l, r)

        mid = (l + r) // 2
        return (
            self._query(node * 2, l, mid, ql, qr) +
            self._query(node * 2 + 1, mid + 1, r, ql, qr)
        )

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n, q = map(int, input().split())
        s = input().strip()
        f = input().strip()

        queries = []
        for _ in range(q):
            l, r = map(int, input().split())
            queries.append((l - 1, r - 1))

        st = SegmentTree([int(c) for c in f])

        ok = True

        for l, r in reversed(queries):
            ones = st.query(l, r)
            length = r - l + 1

            if ones * 2 == length:
                ok = False
                break

            if ones * 2 < length:
                st.update(l, r, 0)
            else:
                st.update(l, r, 1)

        if not ok:
            ans.append("NO")
            continue

        for i in range(n):
            if st.query(i, i) != int(s[i]):
                ok = False
                break

        ans.append("YES" if ok else "NO")

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Cây phân đoạn chỉ lưu trữ một phần thông tin, số lượng thông tin trong mỗi khoảng. Đó chính xác là những gì quá trình ngược lại cần để quyết định trạng thái tiền thân. 

Tuyên truyền lười biếng là điều cần thiết vì mỗi bước đảo ngược sẽ ghi đè lên toàn bộ phân đoạn bằng tất cả số 0 hoặc tất cả số 1. Nếu không có sự lan truyền lười biếng, một bản cập nhật duy nhất có thể tiêu tốn thời gian tuyến tính. 

Sự so sánh chống lại`s`chỉ được thực hiện sau khi tất cả các thao tác ngược lại đã được áp dụng. Tại thời điểm đó, cây phân đoạn đại diện cho chuỗi ban đầu được xây dựng lại. 

Việc kiểm tra đẳng thức sử dụng`ones * 2 == length`thay vì chia dấu phẩy động. Điều này tránh các vấn đề về độ chính xác và thể hiện trực tiếp điều kiện nửa nghiêm ngặt từ câu lệnh. 

## Ví dụ đã hoạt động 

### Ví dụ 1```
n = 5
s = 00000
f = 00111

queries:
[1,5]
[1,3]
```Thứ tự ngược lại là`[1,3]`, sau đó`[1,5]`. 

| Bước | Phân đoạn | Những cái | Chiều dài | Hành động | Tiểu bang | 
| --- | --- | --- | --- | --- | --- | 
| Bắt đầu | - | - | - | Ban đầu`f`| 00111 | 
| 1 | [1,3] | 1 | 3 | Đa số 0, điền 0 | 00011 | 
| 2 | [1,5] | 2 | 5 | Đa số 0, điền 0 | 00000 | 

Chuỗi được xây dựng lại cuối cùng là`00000`, bằng`s`. 

Trả lời:`YES`. 

Dấu vết này cho thấy ý tưởng trung tâm của việc tái thiết ngược. Mỗi phân khúc có đa số nghiêm ngặt, do đó phân khúc trước được xác định duy nhất. 

### Ví dụ 2```
n = 2
s = 00
f = 01

query:
[1,2]
```Xử lý ngược: 

| Bước | Phân đoạn | Những cái | Chiều dài | Hành động | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | - | - | - | Bang = 01 | 
| 1 | [1,2] | 1 | 2 | Không thể | 

Đoạn này chứa chính xác một số 0 và một số một. Để đạt được trạng thái như vậy từ một phân khúc đồng nhất sẽ yêu cầu thay đổi chính xác một nửa số vị trí. 

Trả lời:`NO`. 

Ví dụ này thể hiện điều kiện từ chối quan trọng nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) log n) | Mỗi truy vấn thực hiện một truy vấn phạm vi và một phép gán phạm vi | 
| Không gian | O(n) | Cây phân đoạn và mảng lười | 

Tổng giá trị của`n`Và`q`trên tất cả các trường hợp thử nghiệm nhiều nhất là`2 · 10^5`. Với các phép toán logarit trên cây phân đoạn, tổng công việc nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    from io import StringIO
    sys.stdin = StringIO(inp)
    out = StringIO()

    input = sys.stdin.readline

    class SegmentTree:
        def __init__(self, arr):
            self.n = len(arr)
            self.tree = [0] * (4 * self.n)
            self.lazy = [-1] * (4 * self.n)
            self.build(1, 0, self.n - 1, arr)

        def build(self, node, l, r, arr):
            if l == r:
                self.tree[node] = arr[l]
                return
            m = (l + r) // 2
            self.build(node * 2, l, m, arr)
            self.build(node * 2 + 1, m + 1, r, arr)
            self.tree[node] = self.tree[node * 2] + self.tree[node * 2 + 1]

        def apply(self, node, l, r, v):
            self.tree[node] = (r - l + 1) * v
            self.lazy[node] = v

        def push(self, node, l, r):
            if self.lazy[node] == -1 or l == r:
                return
            m = (l + r) // 2
            v = self.lazy[node]
            self.apply(node * 2, l, m, v)
            self.apply(node * 2 + 1, m + 1, r, v)
            self.lazy[node] = -1

        def update(self, node, l, r, ql, qr, v):
            if qr < l or r < ql:
                return
            if ql <= l and r <= qr:
                self.apply(node, l, r, v)
                return
            self.push(node, l, r)
            m = (l + r) // 2
            self.update(node * 2, l, m, ql, qr, v)
            self.update(node * 2 + 1, m + 1, r, ql, qr, v)
            self.tree[node] = self.tree[node * 2] + self.tree[node * 2 + 1]

        def query(self, node, l, r, ql, qr):
            if qr < l or r < ql:
                return 0
            if ql <= l and r <= qr:
                return self.tree[node]
            self.push(node, l, r)
            m = (l + r) // 2
            return (
                self.query(node * 2, l, m, ql, qr)
                + self.query(node * 2 + 1, m + 1, r, ql, qr)
            )

    t = int(input())
    ans = []

    for _ in range(t):
        n, q = map(int, input().split())
        s = input().strip()
        f = input().strip()

        qs = []
        for _ in range(q):
            l, r = map(int, input().split())
            qs.append((l - 1, r - 1))

        st = SegmentTree([int(c) for c in f])

        ok = True

        for l, r in reversed(qs):
            ones = st.query(1, 0, n - 1, l, r)
            ln = r - l + 1

            if ones * 2 == ln:
                ok = False
                break

            st.update(1, 0, n - 1, l, r, 1 if ones * 2 > ln else 0)

        if ok:
            for i in range(n):
                if st.query(1, 0, n - 1, i, i) != int(s[i]):
                    ok = False
                    break

        ans.append("YES" if ok else "NO")

    return "\n".join(ans)

assert run("""1
1 0
0
0
""") == "YES"

assert run("""1
1 0
0
1
""") == "NO"

assert run("""1
2 1
00
01
1 2
""") == "NO"

assert run("""1
5 2
00000
00111
1 5
1 3
""") == "YES"

assert run("""1
5 1
11111
11111
1 5
""") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n=1, q=0, s=f`| CÓ | Ví dụ tối thiểu | 
|`n=1, q=0, s≠f`| KHÔNG | Không có hoạt động nào | 
|`00 → 01`có độ dài đoạn 2 | KHÔNG | Chính xác một nửa không thể | 
| Trường hợp mẫu đầu tiên | CÓ | Tái thiết ngược thành công | 
| Tất cả những truy vấn có dư thừa | CÓ | Phân đoạn thống nhất vẫn hợp lệ | 

## Vỏ cạnh 

Hãy xem xét:```
1
2 1
00
01
1 2
```Độ dài đoạn là`2`và chứa đúng một`1`. Trong quá trình xử lý ngược, chúng tôi thu được`ones = 1`,`length = 2`, Vì thế`2 * ones = length`. Thuật toán ngay lập tức từ chối. Điều này phù hợp với quy luật vì việc thay đổi chính xác một trong hai vị trí không hoàn toàn ít hơn một nửa. 

Coi như:```
1
4 1
0000
0001
1 4
```Đoạn này chứa một`1`và ba`0`S. Quá trình xử lý ngược nhìn thấy đa số bằng 0 nghiêm ngặt và xây dựng lại phần trước đó như`0000`. Chuỗi được xây dựng lại cuối cùng bằng`s`, vậy câu trả lời là`YES`. Thuật toán xác định chính xác người tiền nhiệm duy nhất có thể. 

Coi như:```
1
3 0
101
111
```Không có truy vấn nào. Pha ngược lại không thực hiện công. Chuỗi được xây dựng lại vẫn còn`111`, không bằng chuỗi gốc`101`. Thuật toán trả về`NO`, nhận biết chính xác rằng không thể sửa đổi được. 

Coi như:```
1
3 0
101
101
```Một lần nữa không có truy vấn nào, nhưng bây giờ chuỗi được xây dựng lại đã khớp`s`. Thuật toán trả về`YES`, đó chính xác là hành vi dự định khi không cần thay đổi.
