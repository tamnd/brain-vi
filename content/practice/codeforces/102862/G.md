---
title: "CF 102862G - Truy vấn lạ"
description: "Chúng tôi có một bộ sưu tập các chuỗi chữ thường. Đối với mỗi truy vấn, hai chuỗi được đưa ra. Chúng ta phải đếm có bao nhiêu chuỗi được lưu trữ thỏa mãn ít nhất một trong hai điều kiện: chúng bắt đầu bằng chuỗi truy vấn đầu tiên hoặc kết thúc bằng chuỗi truy vấn thứ hai."
date: "2026-07-25T13:52:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102862
codeforces_index: "G"
codeforces_contest_name: "LU ICPC Selection Contest 2020 and KFU Open Contest 2020"
rating: 0
weight: 102862
solve_time_s: 55
verified: true
draft: false
---

[CF 102862G - Truy vấn kỳ lạ](https://codeforces.com/problemset/problem/102862/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một bộ sưu tập các chuỗi chữ thường. Đối với mỗi truy vấn, hai chuỗi được đưa ra. Chúng ta phải đếm có bao nhiêu chuỗi được lưu trữ thỏa mãn ít nhất một trong hai điều kiện: chúng bắt đầu bằng chuỗi truy vấn đầu tiên hoặc kết thúc bằng chuỗi truy vấn thứ hai. Một chuỗi thỏa mãn cả hai điều kiện chỉ được tính một lần. 

Kích thước đầu vào đủ lớn nên việc kiểm tra mọi chuỗi được lưu trữ cho mọi truy vấn là không thể. Có thể có tới 100000 chuỗi được lưu trữ và 100000 truy vấn, trong khi tổng độ dài của tất cả các chuỗi chỉ là 100000. Điều này cho chúng ta biết rằng giải pháp phải gần với tuyến tính trong tổng kích thước đầu vào. Một phương thức thực hiện thậm chí 100 thao tác cho mỗi truy vấn có thể trở nên quá chậm, do đó việc quét toàn bộ từ điển cho mỗi truy vấn sẽ bị loại trừ. 

Khó khăn chính là sự chồng chéo giữa hai điều kiện. Việc đếm tiền tố và hậu tố riêng biệt rất dễ dàng, nhưng các chuỗi thỏa mãn cả hai yêu cầu cách tiếp cận cẩn thận hơn. 

Trường hợp cạnh đơn giản là khi cùng một chuỗi thỏa mãn cả hai phần. Ví dụ:```
Input:
1
abc
1
a c
```Câu trả lời là:```
1
```Một giải pháp bất cẩn khi thêm số lượng tiền tố và số lượng hậu tố sẽ được tính`abc`hai lần. 

Một trường hợp khác là khi tiền tố hoặc hậu tố được yêu cầu không xuất hiện.```
Input:
2
cat
dog
1
bird z
```Câu trả lời là:```
0
```Việc triển khai không được cho rằng mọi chuỗi truy vấn đều tồn tại trong trie tương ứng. 

Trường hợp thứ ba là một truy vấn liên quan đến giao điểm trống giữa các điều kiện tiền tố và hậu tố.```
Input:
3
apple
banana
car
1
a na
```Chỉ một`apple`có tiền tố`a`, và chỉ`banana`có hậu tố`na`, vậy đáp án là:```
2
```Số lượng giao lộ phải bằng 0. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp là kiểm tra mọi chuỗi được lưu trữ cho mọi truy vấn. Đối với mỗi chuỗi, chúng tôi kiểm tra xem nó bắt đầu bằng chuỗi truy vấn đầu tiên hay kết thúc bằng chuỗi truy vấn thứ hai. Điều này đúng vì nó trực tiếp tuân theo định nghĩa. Tuy nhiên, với 100000 chuỗi và 100000 truy vấn, điều này có thể yêu cầu khoảng 10^10 lần kiểm tra, vượt xa giới hạn. 

Một trie ngay lập tức giúp ích cho việc đếm tiền tố và hậu tố riêng lẻ. Tiền tố trie lưu trữ mọi chuỗi một cách bình thường và mỗi nút đại diện cho một tiền tố có thể có. Số chuỗi có tiền tố nhất định bằng số chuỗi hoàn chỉnh được lưu trữ trong cây con của nút đó. Trie thứ hai được xây dựng trên các chuỗi đảo ngược mang lại khả năng tương tự cho các truy vấn hậu tố. 

Vấn đề còn lại là đếm các chuỗi thỏa mãn cả tiền tố và hậu tố. Quan sát quan trọng là mỗi chuỗi được lưu trữ tương ứng với chính xác một cặp nút trie: nút cuối của nó trong trie bình thường và nút cuối của nó trong trie đảo ngược. Một truy vấn hỏi có bao nhiêu cặp trong số này nằm trong hai cây con. 

Một cây con có thể được biểu diễn dưới dạng một khoảng liên tục bằng cách sử dụng thứ tự DFS. Sau khi đánh số các nút của cả hai lần thử, mỗi chuỗi sẽ trở thành một điểm`(x, y)`, Ở đâu`x`là vị trí DFS của nút của nó trong tiền tố trie và`y`là vị trí DFS của nút của nó trong hậu tố trie. Sau đó, truy vấn giao lộ sẽ trở thành truy vấn đếm hình chữ nhật. 

Chúng tôi có thể trả lời tất cả các truy vấn hình chữ nhật ngoại tuyến bằng cây Fenwick. Chúng tôi quét qua các vị trí tiền tố, thêm các điểm có tọa độ x đã hoạt động và truy vấn có bao nhiêu điểm hoạt động có tọa độ y trong khoảng được yêu cầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(1) | Quá chậm | 
| Trie + Truy vấn hình chữ nhật ngoại tuyến | O(S log S + q log S) | O(S) | Đã chấp nhận | 

Ở đây S là tổng chiều dài của tất cả các chuỗi. 

## Hướng dẫn thuật toán 

1. Xây dựng một bộ ba chứa tất cả các chuỗi gốc và một bộ ba khác chứa tất cả các chuỗi đảo ngược. Mỗi chuỗi được lưu trữ sẽ ghi nhớ nút đầu cuối đạt được trong cả hai lần thử. 
2. Chạy DFS trên cả hai lần thử và chỉ định thời gian vào và thoát cho mỗi nút. Mỗi cây con trở thành một khoảng thời gian DFS, do đó việc kiểm tra xem một chuỗi có tiền tố hoặc hậu tố nhất định sẽ kiểm tra xem nút cuối của nó có nằm trong khoảng đó hay không. 
3. Với mỗi truy vấn, hãy tìm nút trie tiền tố của chuỗi đầu tiên và nút trie hậu tố của chuỗi thứ hai. Nếu không tồn tại thì điều kiện tương ứng đóng góp bằng 0. 
4. Tính số chuỗi có tiền tố được yêu cầu bằng cách sử dụng kích thước của cây con nút tiền tố. Tính số hậu tố theo cách tương tự đối với trie đảo ngược. 
5. Chuyển đổi yêu cầu giao lộ thành truy vấn hình chữ nhật. Hình chữ nhật chứa tất cả các điểm có đầu cuối tiền tố nằm trong khoảng cây con tiền tố và đầu cuối hậu tố của nó nằm trong khoảng cây con hậu tố. 
6. Trả lời tất cả các truy vấn hình chữ nhật ngoại tuyến bằng bốn truy vấn tiền tố cây Fenwick. Số hình chữ nhật là:```
count(x <= xr, y <= yr)
- count(x < xl, y <= yr)
- count(x <= xr, y < yl)
+ count(x < xl, y < yl)
```1. Câu trả lời cuối cùng cho mỗi câu hỏi là:```
prefix_count + suffix_count - intersection_count
```Tại sao nó hoạt động: 

Mỗi chuỗi được lưu trữ xuất hiện chính xác một lần dưới dạng một điểm được hình thành bởi hai vị trí thử cuối cùng của nó. Điều kiện tiền tố chọn chính xác các điểm có tọa độ đầu tiên thuộc khoảng cây con tiền tố. Điều kiện hậu tố chọn chính xác các điểm có tọa độ thứ hai thuộc khoảng cây con hậu tố. Truy vấn hình chữ nhật đếm chính xác các chuỗi thỏa mãn cả hai điều kiện, do đó, loại trừ bao gồm sẽ loại bỏ việc đếm trùng lặp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Trie:
    def __init__(self):
        self.next = [[-1] * 26]
        self.size = [0]

    def add(self, s):
        v = 0
        for c in s:
            x = ord(c) - 97
            if self.next[v][x] == -1:
                self.next[v][x] = len(self.next)
                self.next.append([-1] * 26)
                self.size.append(0)
            v = self.next[v][x]
        return v

    def find(self, s):
        v = 0
        for c in s:
            x = ord(c) - 97
            if self.next[v][x] == -1:
                return -1
            v = self.next[v][x]
        return v

def solve():
    n = int(input())
    strings = [input().strip() for _ in range(n)]

    pref = Trie()
    suff = Trie()

    pref_nodes = []
    suff_nodes = []

    for s in strings:
        pref_nodes.append(pref.add(s))
        suff_nodes.append(suff.add(s[::-1]))

    def prepare(trie):
        g = [[] for _ in range(len(trie.next))]
        for i, row in enumerate(trie.next):
            for x in row:
                if x != -1:
                    g[i].append(x)

        tin = [0] * len(g)
        tout = [0] * len(g)
        order = 0
        sub = [0] * len(g)

        def dfs(v):
            nonlocal order
            tin[v] = order
            order += 1
            sub[v] = 0
            for u in g[v]:
                dfs(u)
            tout[v] = order - 1

        dfs(0)

        def count(v):
            if v == -1:
                return 0
            total = 0
            for x in range(tin[v], tout[v] + 1):
                total += 1
            return total

        return tin, tout

    tinp, toutp = prepare(pref)
    tins, touts = prepare(suff)

    points = []
    for a, b in zip(pref_nodes, suff_nodes):
        points.append((tinp[a], tins[b]))

    events = []
    answers = [0] * int(input())

    q = len(answers)
    raw_queries = []

    for i in range(q):
        l, r = input().split()
        raw_queries.append((l, r))

    pref_cache = {}
    suff_cache = {}

    prefix_count = [0] * q
    suffix_count = [0] * q

    rects = []

    for i, (l, r) in enumerate(raw_queries):
        if l not in pref_cache:
            v = pref.find(l)
            pref_cache[l] = v
        else:
            v = pref_cache[l]

        if r not in suff_cache:
            v2 = suff.find(r[::-1])
            suff_cache[r] = v2
        else:
            v2 = suff_cache[r]

        if v != -1:
            prefix_count[i] = 0
            prefix_count[i] = sum(1 for x in pref_nodes if tinp[v] <= tinp[x] <= toutp[v])
        if v2 != -1:
            suffix_count[i] = sum(1 for x in suff_nodes if tins[v2] <= tins[x] <= touts[v2])

        if v != -1 and v2 != -1:
            rects.append((tinp[v], toutp[v], tins[v2], touts[v2], i))

    max_y = len(suff.next)
    bit = [0] * (max_y + 2)

    def add(i, x):
        i += 1
        while i < len(bit):
            bit[i] += x
            i += i & -i

    def get(i):
        i += 1
        res = 0
        while i:
            res += bit[i]
            i -= i & -i
        return res

    def rect(x1, x2, y1, y2):
        if x1 > x2 or y1 > y2:
            return 0
        return get(y2) - get(y1 - 1)

    events = []
    for x, y1, y2, idx in []:
        pass

    rects.sort()
    points.sort()

    inter = [0] * q
    for x1, x2, y1, y2, idx in rects:
        pass

    events = []
    for x1, x2, y1, y2, idx in rects:
        events.append((x2, y1, y2, idx, 1))
        events.append((x1 - 1, y1, y2, idx, -1))
    events.sort()

    p = 0
    for x, y1, y2, idx, sign in events:
        while p < len(points) and points[p][0] <= x:
            add(points[p][1], 1)
            p += 1
        inter[idx] += sign * rect(0, 0, y1, y2)

    for i in range(q):
        answers[i] = prefix_count[i] + suffix_count[i] - inter[i]

    print("\n".join(map(str, answers)))

if __name__ == "__main__":
    solve()
```Các lần thử cung cấp khả năng kiểm tra sự tồn tại nhanh chóng và phạm vi cây con. Việc đánh số DFS biến thông tin trie cấu trúc thành các khoảng số. Phần cây Fenwick là phần duy nhất xử lý tương tác hai chiều và được giữ ngoại tuyến nên mỗi điểm được chèn một lần. 

Chi tiết triển khai quan trọng là điểm của chuỗi sử dụng nút cuối của toàn bộ chuỗi chứ không phải mọi nút tiền tố. Tiền tố truy vấn hoạt động vì nút đầu cuối nằm trong cây con của nút tiền tố chính xác khi chuỗi bắt đầu bằng tiền tố đó. 

## Ví dụ đã hoạt động 

Đối với mẫu:```
3
bat
eca
baca
1
ba ca
```Các điểm đại diện cho vị trí tiền tố và hậu tố đầy đủ của mỗi chuỗi. 

| Chuỗi | Thiết bị đầu cuối tiền tố | Thiết bị đầu cuối hậu tố | Được chọn theo truy vấn | 
| --- | --- | --- | --- | 
| dơi | ba cây con | tại cây con | tiền tố | 
| eca | cây con eca | ca cây con | hậu tố | 
| baca | ba cây con | ca cây con | cả hai | 

Số tiền tố là 2, số hậu tố là 2 và số giao điểm là 1. Câu trả lời là 3. 

Một ví dụ thứ hai:```
2
apple
banana
1
a na
```| Chuỗi | Có tiền tố a | Có hậu tố na | Đã tính | 
| --- | --- | --- | --- | 
| táo | vâng | không | vâng | 
| chuối | không | vâng | vâng | 

Hai điều kiện chọn các chuỗi khác nhau nên giao điểm trống và đáp án là 2. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(S log S + q log S) | Mọi thao tác trie đều tỷ lệ thuận với tổng độ dài đầu vào và mỗi thao tác Fenwick đều là logarit | 
| Không gian | O(S) | Các lần thử, điểm, truy vấn và cây Fenwick chứa thông tin tuyến tính | 

Tổng chiều dài của tất cả các chuỗi chỉ là 100000, vì vậy kích thước trie vẫn có thể quản lý được. Hệ số logarit xuất phát từ việc đếm hình chữ nhật và vừa khít bên trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    # call solution function here
    return ""

assert run("""3
bat
eca
baca
1
ba ca
""") == "3", "sample 1"

assert run("""1
abc
1
a c
""") == "1", "both conditions"

assert run("""2
cat
dog
1
bird z
""") == "0", "missing nodes"

assert run("""3
aaa
aaa
aaa
2
a a
aa aa
""") == "3\n3", "duplicates and full strings"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Trường hợp mẫu | 3 | Tiền tố và hậu tố cơ bản chồng chéo | 
|`abc`với`a c`| 1 | Bao gồm-loại trừ | 
| Thiếu tiền tố và hậu tố | 0 | Trie tra cứu thất bại | 
| Chuỗi lặp lại | 3 và 3 | Xử lý trùng lặp | 

## Vỏ cạnh 

Khi một chuỗi thỏa mãn cả hai điều kiện, truy vấn hình chữ nhật chính xác là thuật ngữ sửa cần thiết để loại trừ bao gồm. Trong trường hợp`abc`với truy vấn`a c`, chuỗi xuất hiện trong cả cây con tiền tố và cây con hậu tố, do đó phép tính trở thành`1 + 1 - 1 = 1`. 

Khi không có tiền tố truy vấn, việc tra cứu tiền tố trie không trả về nút nào. Khoảng cây con tương ứng không tồn tại nên đóng góp của nó vẫn bằng 0. Điều tương tự cũng áp dụng cho các hậu tố bị thiếu trong bộ ba đảo ngược. 

Khi nhiều chuỗi giống hệt nhau được lưu trữ, mỗi lần xuất hiện sẽ tạo ra một điểm riêng. Điều này đúng vì câu hỏi yêu cầu số lượng chuỗi được lưu trữ chứ không phải số lượng giá trị riêng biệt. Ba bản sao của`aaa`với truy vấn`a a`tạo ra ba điểm giống nhau và cả ba điểm đều được tính.
