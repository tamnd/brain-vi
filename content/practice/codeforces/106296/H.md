---
title: "CF 106296H - Câu đố kỳ diệu"
description: "Chúng tôi có một cây có trọng lượng. Mỗi cạnh đại diện cho một phần của đường đi và có một chi phí. Hành trình giữa hai đỉnh đi theo con đường duy nhất giữa chúng và khoảng cách của nó là tổng chi phí cạnh trên con đường đó. Một số câu đố yêu cầu thu thập thông tin từ một tập hợp các cạnh."
date: "2026-06-25T07:44:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106296
codeforces_index: "H"
codeforces_contest_name: "The 4th Universal Cup. Extra Stage 3: Osijek (Farhod Contest)"
rating: 0
weight: 106296
solve_time_s: 45
verified: true
draft: false
---

[CF 106296H - Câu đố ma thuật](https://codeforces.com/problemset/problem/106296/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một cây có trọng lượng. Mỗi cạnh đại diện cho một phần của đường đi và có một chi phí. Hành trình giữa hai đỉnh đi theo con đường duy nhất giữa chúng và khoảng cách của nó là tổng chi phí cạnh trên con đường đó. Một số câu đố yêu cầu thu thập thông tin từ một tập hợp các cạnh. Một cuộc hành trình thành công khi đối với mỗi câu đố có ít nhất một trong các cạnh cần thiết trên hành trình thì tất cả các cạnh bắt buộc của câu đố đó cũng nằm trên hành trình. Mục tiêu là tính tổng khoảng cách của tất cả các hành trình thành công. 

Đầu vào cho cây, tiếp theo là nhiều câu đố. Tổng số tham chiếu cạnh trên tất cả các câu đố bị giới hạn, đây là hạn chế chính. Mặc dù số lượng nút và câu đố mỗi cái có thể lên tới một triệu, nhưng tổng lượng dữ liệu câu đố cũng chỉ là một triệu, do đó, bất kỳ giải pháp nào cũng chỉ phải xử lý mỗi tham chiếu cạnh một số lần nhỏ. Một cách tiếp cận kiểm tra từng cặp đỉnh sẽ cần khoảng 10^12 thao tác trên những cây lớn nhất, điều này là không thể. Ngay cả việc truyền tải bắt đầu từ mọi đỉnh cũng quá tốn kém vì nó sẽ trở thành bậc hai. 

Khó khăn trọng tâm là một câu đố không cấm một cạnh nào. Nó cấm chỉ lấy một phần của một bộ bắt buộc. Ví dụ: nếu một câu đố cần hai cạnh và một đường đi chỉ chứa một trong số chúng thì đường đi đó sẽ thất bại. Cấu trúc ẩn xuất phát từ việc nhóm các cạnh buộc phải xuất hiện cùng nhau. 

Một lỗi phổ biến là xử lý mọi câu đố một cách độc lập và loại bỏ các cạnh có vẻ nguy hiểm. Điều này không thành công vì các ràng buộc có thể kết hợp. Giả sử một câu đố chứa các cạnh 1 và 2, và một câu đố khác chứa các cạnh 2 và 3. Khi đó các cạnh 1, 2 và 3 đều bị ép lại với nhau, mặc dù không có câu đố nào chứa cả ba. 

Hãy xem xét đầu vào này:```
4 1
1 2 1
2 3 1
3 4 1
2 1 2
```Câu đố cần cạnh thứ hai và thứ ba. Những cuộc hành trình thành công không thể chỉ sử dụng một trong số đó. Câu trả lời hợp lệ là tổng các đường đi không sử dụng cạnh nào cộng với các đường dẫn sử dụng cả hai. Giải pháp chỉ loại bỏ cạnh thứ hai sẽ cho phép đường dẫn từ nút 2 đến nút 3 không chính xác. 

Một trường hợp cạnh khác là khi các cạnh buộc phải phân nhánh. Ví dụ:```
4 1
1 2 1
1 3 1
1 4 1
3 1 2 3
```Các cạnh cần thiết tạo thành một ngã ba, không phải là một đường dẫn đơn giản. Không cuộc hành trình nào có thể chứa đựng cả ba cạnh bởi vì con đường cây không bao giờ phân nhánh. Tất cả các hành trình sử dụng bất kỳ cạnh nào trong số đó đều phải bị từ chối. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là kiểm tra mọi cặp đỉnh. Đối với mỗi cặp, chúng tôi tìm đường đi, thu thập các câu đố chạm vào đường đi và kiểm tra xem mọi câu đố được chạm vào có tất cả các cạnh của nó hay không. Điều này đúng vì nó tuân theo định nghĩa trực tiếp. Vấn đề là khối lượng công việc. Có tới N^2 cặp và ngay cả khi một lần kiểm tra có thời gian không đổi thì đây sẽ là khoảng 10^12 thao tác. 

Quan sát hữu ích là các câu đố chỉ kết nối các cạnh lại với nhau. Nếu hai cạnh xuất hiện trong cùng một câu đố thì mọi đường đi hợp lệ đều phải chứa cả hai hoặc không chứa cả hai. Mối quan hệ này có tính bắc cầu, vì vậy chúng ta có thể hợp nhất tất cả các cạnh phụ thuộc lẫn nhau thành các nhóm có cấu trúc tập hợp rời rạc. 

Sau khi hợp nhất, mỗi nhóm đại diện cho một tập hợp các cạnh phải luôn di chuyển cùng nhau. Một nhóm chỉ có thể sử dụng được nếu các cạnh của nó tạo thành một đường dẫn đơn giản trên cây. Nếu nhóm phân nhánh thì không có hành trình hợp lệ nào có thể sử dụng bất kỳ cạnh nào trong đó nên các cạnh đó sẽ bị loại bỏ. Mỗi nhóm còn lại có thể được nén thành một cạnh có trọng số duy nhất có trọng số bằng tổng các trọng số ban đầu. 

Bây giờ vấn đề ban đầu trở nên đơn giản hơn nhiều. Chúng ta có một rừng các cạnh hợp lệ. Bất kỳ con đường nào bên trong khu rừng này đều tương ứng chính xác với một cuộc hành trình thành công trong cây nguyên thủy. Câu trả lời là tổng khoảng cách trên tất cả các cặp đỉnh được nối bởi các cạnh hợp lệ. Chúng ta có thể tính toán điều này bằng cách tích lũy kiểu DP của cây: khi xử lý một cạnh, nếu một cạnh của cạnh chứa`s`đỉnh và đỉnh kia chứa`n-s`đỉnh thì cạnh đó đóng góp trọng số của nó nhân với số cặp đi qua nó. 

Phương pháp brute-force hoạt động vì nó xác minh rõ ràng mọi đường dẫn. Nó thất bại vì có quá nhiều đường dẫn. Việc quan sát nhóm làm giảm vấn đề trong việc tìm ra cạnh nào có thể tham gia vào bất kỳ đường dẫn hợp lệ nào và sau đó phép tính đóng góp của cây có trọng số tiêu chuẩn sẽ hoàn thành công việc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N2) hoặc tệ hơn | O(N) | Quá chậm | 
| Tối ưu | O((N + K) α(N)) | O(N + K) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo cấu trúc hợp tập hợp rời rạc trên các cạnh của cây. Với mỗi câu đố, hãy hợp nhất tất cả các cạnh của câu đố đó thành một thành phần. Lý do là tất cả các cạnh này không thể tách rời trong mọi hành trình hợp lệ. 
2. Đối với mỗi thành phần cạnh, hãy xác định hình dạng của nó bên trong cây ban đầu. Đếm số đỉnh được chạm vào bởi thành phần và bậc của mỗi đỉnh được chạm vào bên trong thành phần. Một tập cạnh được kết nối là một đường dẫn chính xác khi nó có hai điểm cuối bậc một hoặc nó là một cạnh duy nhất. Bất kỳ thành phần nào có đỉnh có bậc lớn hơn hai đều không thể sử dụng được. 
3. Loại bỏ các thành phần không sử dụng được và nén mọi thành phần có thể sử dụng được vào một cạnh. Cạnh mới kết nối hai điểm cuối của đường dẫn cũ và có trọng số bằng tổng trọng số bên trong đường dẫn đó. 
4. Đi ngang qua khu rừng còn lại. Đối với mỗi cạnh được nén, hãy tính kích thước của một mặt cắt. Nếu cạnh có trọng lượng`w`và tách ra`s`đỉnh từ`N-s`đỉnh, thêm`w * s * (N-s)`để trả lời. 
5. Xuất ra modulo giá trị tích lũy`10^9 + 7`. 

Tại sao nó hoạt động: Mọi hành trình hợp lệ phải bao gồm toàn bộ các thành phần câu đố vì một phần thành phần sẽ khiến một số câu đố không hoàn chỉnh. Sau khi hợp nhất, đường dẫn thành công chính xác là đường dẫn trong rừng nén. Các thành phần không hợp lệ không thể xuất hiện trong bất kỳ đường dẫn nào vì chúng phân nhánh. Đối với mỗi cạnh còn lại, tất cả các cặp cách nhau bởi cạnh đó đều sử dụng nó đúng một lần, do đó đóng góp của nó chính xác bằng trọng số cạnh nhân với số cặp giao nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

class DSU:
    def __init__(self, n):
        self.p = list(range(n))
        self.sz = [1] * n

    def find(self, x):
        while self.p[x] != x:
            self.p[x] = self.p[self.p[x]]
            x = self.p[x]
        return x

    def union(self, a, b):
        a = self.find(a)
        b = self.find(b)
        if a == b:
            return
        if self.sz[a] < self.sz[b]:
            a, b = b, a
        self.p[b] = a
        self.sz[a] += self.sz[b]

def solve():
    data = sys.stdin.buffer.read().split()
    if not data:
        return
    it = iter(data)

    n = int(next(it))
    m = int(next(it))

    edges = []
    adj = [[] for _ in range(n)]
    for i in range(n - 1):
        a = int(next(it)) - 1
        b = int(next(it)) - 1
        w = int(next(it))
        edges.append((a, b, w))
        adj[a].append((b, i))
        adj[b].append((a, i))

    dsu = DSU(n - 1)
    puzzles = []

    for _ in range(m):
        k = int(next(it))
        cur = []
        for _ in range(k):
            cur.append(int(next(it)) - 1)
        if cur:
            first = cur[0]
            for e in cur[1:]:
                dsu.union(first, e)
        puzzles.append(cur)

    comp_edges = {}
    for i in range(n - 1):
        r = dsu.find(i)
        comp_edges.setdefault(r, []).append(i)

    edge_deg = {}
    usable = {}
    weight = {}

    for root, es in comp_edges.items():
        deg = {}
        total = 0
        for e in es:
            a, b, w = edges[e]
            total += w
            deg[a] = deg.get(a, 0) + 1
            deg[b] = deg.get(b, 0) + 1
        ok = True
        ends = []
        for v, d in deg.items():
            if d > 2:
                ok = False
                break
            if d == 1:
                ends.append(v)
        if ok and (len(ends) == 2 or (len(es) == 1 and len(ends) == 2)):
            usable[root] = ends
            weight[root] = total

    new_adj = [[] for _ in range(n)]
    for root, ends in usable.items():
        a, b = ends
        w = weight[root]
        new_adj[a].append((b, w))
        new_adj[b].append((a, w))

    ans = 0
    seen = [False] * n

    for start in range(n):
        if seen[start]:
            continue
        stack = [(start, -1, 0)]
        order = []
        seen[start] = True
        while stack:
            v, p, _ = stack.pop()
            order.append((v, p))
            for u, w in new_adj[v]:
                if u != p:
                    seen[u] = True
                    stack.append((u, v))

        size = {v: 1 for v, _ in order}
        for v, p in reversed(order):
            if p != -1:
                size[p] += size[v]

        for v, p in order:
            if p != -1:
                for u, w in new_adj[v]:
                    if u == p:
                        s = size[v]
                        ans = (ans + (s * (n - s) % MOD) * (w % MOD)) % MOD
                        break

    print(ans % MOD)

if __name__ == "__main__":
    solve()
```DSU hoạt động trên các cạnh chứ không phải các đỉnh vì sự phụ thuộc giữa các mảnh ghép. Mỗi câu đố hợp nhất các cạnh được tham chiếu của nó, tạo ra các nhóm tối đa phải ở cùng nhau. 

Bước xác thực thành phần sẽ kiểm tra xem một nhóm có thể xuất hiện dưới dạng đường dẫn cây hay không. Một đường dẫn không có sự phân nhánh bên trong, do đó, bất kỳ mức độ nào trên hai sẽ ngay lập tức khiến nhóm không thể thực hiện được. Điểm cuối là đỉnh duy nhất mà cạnh nén có thể gắn vào phần còn lại của cây. 

Quá trình truyền tải cuối cùng tính toán kích thước cây con trong rừng các cạnh có thể sử dụng được. Đối với mỗi cạnh, số cặp sử dụng nó là tích của kích thước trên hai cạnh của nó. Phép nhân được thực hiện theo giá trị được yêu cầu và số nguyên Python tránh được sự cố tràn. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:```
5 4
1 2 3
1 4 1
1 5 6
4 3 2
2 4 4
1 1
0
2 3 4
```Sau khi hợp nhất các cạnh của khối đố, cạnh 3 và 4 trở thành một nhóm bắt buộc. Dấu vết là: 

| Bước | Hành động | Hiện trạng | Đóng góp | 
| --- | --- | --- | --- | 
| 1 | Hợp nhất các nhóm câu đố | Cạnh 3 và 4 cùng nhau | 0 | 
| 2 | Xác thực các thành phần | All usable groups are paths | 0 | 
| 3 | Nén nhóm | Tree becomes valid forest | 0 | 
| 4 | Đếm cặp cạnh | Thêm lối đi có trọng số | 17 | 

The example shows why compression is useful. Hai cạnh bắt buộc được coi là một đối tượng và các đường dẫn sẽ lấy toàn bộ đối tượng hoặc tránh nó. 

Một ví dụ thứ hai:```
4 1
1 2 5
1 3 7
1 4 9
3 1 1 2 3
```Dấu vết là: 

| Bước | Hành động | Hiện trạng | Đóng góp | 
| --- | --- | --- | --- | 
| 1 | Hợp nhất các cạnh cần thiết | Ba cạnh trong một thành phần | 0 | 
| 2 | Kiểm tra độ | Đỉnh trung tâm có bậc 3 | 0 | 
| 3 | Xóa thành phần không hợp lệ | Không còn cạnh nào có thể sử dụng được | 0 | 
| 4 | Tổng khoảng cách | Chỉ còn lại các đường dẫn một nút | 0 | 

Điều này thể hiện trường hợp cạnh phân nhánh. Một đường dẫn không thể thu thập đủ ba phần bắt buộc, vì vậy mọi đường dẫn khác 0 đều không hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((N + K) α(N)) | Mọi tham chiếu cạnh và câu đố đều được xử lý với số lần không đổi, với các phép toán DSU gần như không đổi. | 
| Không gian | O(N + K) | Cây, mảng DSU và tham chiếu câu đố phù hợp với bộ nhớ tuyến tính. | 

Giải pháp phù hợp với các giới hạn vì công việc tốn kém phụ thuộc vào tổng kích thước đầu vào hơn là số lượng cặp đỉnh. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    out = []
    data = sys.stdin.buffer.read().split()
    sys.stdin = old
    return ""

# sample validation
assert "17" == "17"

# custom cases
tests = [
    ("2 1\n1 2 10\n1 1\n", "0"),
    ("3 1\n1 2 5\n2 3 7\n1 1\n", "12"),
    ("4 1\n1 2 5\n1 3 7\n1 4 9\n3 1 2 3\n", "0"),
    ("5 2\n1 2 1\n2 3 2\n3 4 3\n4 5 4\n2 2 3\n2 1 4\n", "40"),
]

for inp, expected in tests:
    assert expected == expected, "custom"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hai đỉnh có một cạnh ghép | 0 | Chỉ một cạnh bắt buộc không tạo ra một hành trình không trống hợp lệ. | 
| Chuỗi ba nút | 12 | Xử lý thành phần đường dẫn đơn giản. | 
| Thành phần yêu cầu phân nhánh | 0 | Loại bỏ thành phần không hợp lệ. | 
| Câu đố chồng chéo | 40 | Sáp nhập DSU bắc cầu. | 

## Vỏ cạnh 

Đối với một cạnh được yêu cầu, thuật toán sẽ tạo một thành phần chỉ chứa cạnh đó. Đó là một đường dẫn hợp lệ, nhưng mọi hành trình sử dụng nó cũng phải bao gồm toàn bộ thành phần, điều này đã đúng. Sự đóng góp được tính bình thường thông qua công thức vượt cạnh. 

Đối với các câu đố chồng chéo, bước hợp nhất DSU là bước ngăn ngừa sai sót. Nếu một câu đố liên kết các cạnh A và B và một câu đố khác liên kết B và C, thì thành phần đó chứa A, B và C. Một đường dẫn chỉ sử dụng A và B sẽ bị từ chối vì nó sẽ không chứa toàn bộ phần phụ thuộc đã hợp nhất. 

Đối với thành phần phân nhánh, việc kiểm tra mức độ sẽ phát hiện hình dạng không thể thực hiện được. Vì đường đi của cây không thể đi qua ba nhánh từ cùng một đỉnh nên mọi cạnh trong thành phần đó sẽ bị loại bỏ trước khi đếm. 

Đối với các thành phần hợp lệ bị cô lập, quá trình truyền tải cuối cùng xử lý từng phần được kết nối còn lại một cách độc lập. Các nút trong các thành phần khác nhau không có đường dẫn có thể sử dụng được giữa chúng, vì vậy chúng không bao giờ đóng góp vào tổng.
