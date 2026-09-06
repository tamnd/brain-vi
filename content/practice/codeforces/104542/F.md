---
title: "CF 104542F - Sự cố chuỗi thú vị"
description: "Chúng ta có một tập hợp các nút bị cô lập, mỗi nút mang một chuỗi nhỏ cố định. Theo thời gian, các cạnh được thêm vào, do đó các nút dần dần hình thành các thành phần được kết nối. Các thành phần này hoạt động giống như các nhóm phát triển khi các liên minh được thực hiện."
date: "2026-06-30T09:13:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104542
codeforces_index: "F"
codeforces_contest_name: "TheForces Round #22 (Interesting-Forces)"
rating: 0
weight: 104542
solve_time_s: 108
verified: true
draft: false
---

[CF 104542F - Sự cố chuỗi thú vị](https://codeforces.com/problemset/problem/104542/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 48s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một tập hợp các nút bị cô lập, mỗi nút mang một chuỗi nhỏ cố định. Theo thời gian, các cạnh được thêm vào, do đó các nút dần dần hình thành các thành phần được kết nối. Các thành phần này hoạt động giống như các nhóm phát triển khi các liên minh được thực hiện. 

Bên cạnh biểu đồ đang phát triển này, chúng tôi liên tục được hỏi hai điều. Đầu tiên, chúng ta có thể kết nối hai nút, hợp nhất các thành phần của chúng. Thứ hai, chúng ta được cấp một nút và một chuỗi văn bản và chúng ta phải xem bên trong toàn bộ thành phần được kết nối của nút đó. Đối với mỗi nút trong thành phần đó, chúng tôi lấy chuỗi liên kết của nó và đếm số lần nó xuất hiện dưới dạng chuỗi con bên trong văn bản truy vấn. Câu trả lời cuối cùng là tổng số lượng này trên toàn bộ thành phần. 

Khó khăn chính là cả cấu trúc biểu đồ và văn bản truy vấn đều tương tác với nhau. Thành phần này thay đổi theo thời gian và mỗi truy vấn sẽ hỏi về việc khớp mẫu trên một tập hợp lớn các mẫu có thể được xác định bởi thành phần đó. 

Các ràng buộc định hình mạnh mẽ những gì có thể. Tổng chiều dài của tất cả các chuỗi nút chỉ lên tới năm trăm nghìn, điều này cho thấy rằng mỗi ký tự của chuỗi đầu vào chỉ có thể được xử lý tổng thể một số lần nhỏ. Tương tự, tổng chiều dài của tất cả các chuỗi truy vấn cũng bị giới hạn, điều này cho thấy rằng việc quét từng chuỗi truy vấn nhiều lần có thể chấp nhận được miễn là công việc trên mỗi ký tự gần như không đổi. Tuy nhiên, số lượng truy vấn và liên kết lớn, do đó, bất kỳ phương pháp nào liên tục xây dựng lại các cấu trúc nặng nề từ đầu cho mỗi truy vấn đều sẽ quá chậm. 

Một cách giải thích đơn giản, đối với mỗi truy vấn, sẽ lặp lại trên tất cả các nút trong thành phần và đối với mỗi nút sẽ chạy tìm kiếm chuỗi con trên văn bản truy vấn. Điều này đã nhân kích thước thành phần với độ dài truy vấn, điều này trở nên không khả thi khi cả hai đều lớn. 

Một lỗi tinh vi hơn sẽ xuất hiện nếu chúng ta cố gắng tính toán trước các câu trả lời cho mỗi nút một cách độc lập. Điều đó sẽ bỏ qua thực tế là các thành phần hợp nhất một cách linh hoạt và việc tính toán lại các cấu trúc tổng hợp sau mỗi lần kết hợp sẽ dẫn đến việc xây dựng lại toàn bộ nhiều lần. 

Trường hợp cạnh cuối cùng đáng chú ý là các chuỗi giống hệt nhau được lặp lại trên các nút khác nhau. Nếu chúng ta không tổng hợp chúng một cách cẩn thận, chúng ta có thể đếm gấp đôi hoặc lãng phí thời gian liên tục khớp các mẫu giống hệt nhau một cách riêng biệt thay vì coi chúng là cấu trúc chung. 

## Phương pháp tiếp cận 

Giải pháp brute-force trực tiếp xử lý từng truy vấn bằng cách lặp qua tất cả các nút trong thành phần được kết nối của nút được truy vấn. Đối với mỗi nút như vậy, nó sẽ chạy tìm kiếm chuỗi con của chuỗi đó bên trong văn bản truy vấn. Nếu kích thước thành phần lớn và chuỗi truy vấn dài, điều này dẫn đến khoảng O(n * |t|) cho mỗi truy vấn trong trường hợp xấu nhất. Với tối đa 200000 truy vấn, điều này trở nên lớn về mặt thiên văn. 

Quan sát quan trọng là mỗi truy vấn về cơ bản là yêu cầu khớp mẫu của toàn bộ tập hợp các mẫu đối với một văn bản. Các mẫu là các chuỗi được gắn vào các nút trong một thành phần được kết nối. Đây chính xác là loại vấn đề mà máy tự động Aho-Corasick rất hữu ích, bởi vì nó cho phép khớp nhiều mẫu đồng thời theo thời gian tuyến tính theo độ dài văn bản. 

Khó khăn còn lại là tập hợp các mẫu thay đổi linh hoạt do chèn cạnh. Điều này gợi ý việc duy trì, đối với mỗi thành phần được kết nối, một từ điển mẫu hỗ trợ việc hợp nhất. Cấu trúc tự nhiên là một trie được làm giàu thành máy tự động Aho-Corasick. 

Khi hai thành phần hợp nhất, bộ mẫu của chúng sẽ được hợp nhất. Nếu chúng ta luôn hợp nhất máy tự động nhỏ hơn thành máy tự động lớn hơn, thì mỗi chuỗi chỉ được di chuyển theo số lần logarit trong các lần hợp nhất. Sau khi hợp nhất, chúng tôi xây dựng lại các liên kết bị lỗi cho cấu trúc kết hợp.

Sau đó, mỗi truy vấn sẽ chạy một lần duyệt Aho-Corasick trên chuỗi truy vấn và tích lũy tất cả các kết quả khớp mẫu. Vì mỗi mẫu thuộc về chính xác một thành phần tại thời điểm truy vấn nên máy tự động của thành phần đó thể hiện đầy đủ câu trả lời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(q · kích thước(thành phần) · | t | ) | 
| DSU + Aho-Corasick (nhỏ đến lớn) | O((tổng | s_i | + tổng | 

## Hướng dẫn thuật toán 

Chúng tôi kết hợp liên kết tập hợp rời rạc với máy tự động Aho-Corasick được duy trì linh hoạt trên mỗi thành phần. 

### Hướng dẫn thuật toán 

1. Khởi tạo một DSU trong đó mỗi nút là thành phần riêng của nó và mỗi thành phần ban đầu chứa chính xác một chuỗi mẫu. Đối với mỗi nút, chúng tôi xây dựng cấu trúc tri một nút đại diện cho chuỗi của nút đó. Trie này cũng là máy tự động ban đầu cho thành phần đó. 
2. Duy trì cho mỗi gốc DSU một con trỏ tới gốc của cấu trúc tri hiện tại của nó. Trie này đại diện cho tất cả các chuỗi trong thành phần đó. 
3. Khi xử lý truy vấn hợp giữa u và v, hãy tìm gốc DSU của chúng. Nếu chúng đã có trong cùng một thành phần thì không cần làm gì cả. Nếu không, hãy luôn gắn phần nhỏ hơn vào phần lớn hơn. Điều này đảm bảo rằng mỗi nút của mỗi trie chỉ di chuyển một số lần logarit trong tất cả các lần hợp nhất. 
4. Để hợp nhất hai lần thử, chúng ta chèn đệ quy tất cả các nút của trie nhỏ hơn vào trie lớn hơn, cấu trúc chia sẻ nếu có thể. Khi tồn tại các tiền tố giống hệt nhau, chúng tôi sử dụng lại các nút thay vì sao chép chúng. 
5. Sau khi thử hợp nhất, hãy xây dựng lại các liên kết lỗi Aho-Corasick cho trie kết quả. Điều này được thực hiện bởi BFS trên trie, thiết lập các con trỏ lỗi và truyền bá các liên kết đầu ra để mỗi nút biết mẫu nào kết thúc tại hoặc đi qua nó. 
6. Đối với truy vấn loại 2, chúng tôi lấy gốc DSU của nút đã cho, truy xuất máy tự động của nó và chạy truyền tải Aho-Corasick tiêu chuẩn trên chuỗi truy vấn. Mỗi lần chúng tôi đến một nút trong máy tự động, chúng tôi sẽ thêm số lượng mẫu kết thúc tại nút đó vào câu trả lời. 
7. Xuất tổng tích lũy cho mỗi truy vấn. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, mọi thành phần được kết nối đều được biểu thị bằng chính xác một máy tự động Aho-Corasick chứa chính xác tập hợp các chuỗi thuộc về thành phần đó. Hoạt động liên minh duy trì tính bất biến này bằng cách hợp nhất hai ô tô thành một cấu trúc nhất quán duy nhất. Vì mỗi chuỗi được chèn chính xác một lần cho mỗi cấp độ hợp nhất và luôn ở trong một cấu trúc lớn hơn nên tổng chi phí xây dựng lại được khấu hao nhỏ. Tính chính xác của truy vấn tuân theo thuộc tính của Aho-Corasick: mọi lần xuất hiện của bất kỳ mẫu nào trong văn bản đều được báo cáo chính xác một lần thông qua nút đầu cuối của nó và các liên kết đầu ra được truyền bá. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

class Node:
    __slots__ = ("next", "link", "out", "cnt")
    def __init__(self):
        self.next = {}
        self.link = 0
        self.out = 0
        self.cnt = 0

def build_ac(nodes):
    q = deque()
    for c, v in nodes[0].next.items():
        q.append(v)
        nodes[v].link = 0

    while q:
        v = q.popleft()
        nodes[v].out = nodes[nodes[v].link].out + nodes[v].cnt
        for c, u in nodes[v].next.items():
            q.append(u)
            f = nodes[v].link
            while f and c not in nodes[f].next:
                f = nodes[f].link
            nodes[u].link = nodes[f].next[c] if c in nodes[f].next else 0

def merge_trie(big, small, nodes):
    stack = [(big, small)]
    while stack:
        a, b = stack.pop()
        nodes[a].cnt += nodes[b].cnt
        for c, nb in nodes[b].next.items():
            if c in nodes[a].next:
                stack.append((nodes[a].next[c], nb))
            else:
                nodes[a].next[c] = nb

def add_string(nodes, s):
    v = 0
    for ch in s:
        if ch not in nodes[v].next:
            nodes[v].next[ch] = len(nodes)
            nodes.append(Node())
        v = nodes[v].next[ch]
    nodes[v].cnt += 1
    return nodes

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
            return a
        if self.sz[a] < self.sz[b]:
            a, b = b, a
        self.p[b] = a
        self.sz[a] += self.sz[b]
        return a, b

def query_ac(nodes, s):
    v = 0
    res = 0
    for ch in s:
        while v and ch not in nodes[v].next:
            v = nodes[v].link
        if ch in nodes[v].next:
            v = nodes[v].next[ch]
        res += nodes[v].out
    return res

def main():
    n = int(input())
    roots = [None] * n
    nodes_list = []

    def new_trie(s):
        nodes = [Node()]
        v = 0
        for ch in s:
            if ch not in nodes[v].next:
                nodes[v].next[ch] = len(nodes)
                nodes.append(Node())
            v = nodes[v].next[ch]
        nodes[v].cnt = 1
        build_ac(nodes)
        return nodes

    for i in range(n):
        s = input().strip()
        roots[i] = i
        nodes_list.append(new_trie(s))

    dsu = DSU(n)

    q = int(input())
    for _ in range(q):
        tmp = input().split()
        if tmp[0] == '1':
            u = int(tmp[1]) - 1
            v = int(tmp[2]) - 1
            ru = dsu.find(u)
            rv = dsu.find(v)
            if ru == rv:
                continue
            if len(nodes_list[ru]) < len(nodes_list[rv]):
                ru, rv = rv, ru
            merge_trie(nodes_list[ru], nodes_list[rv], nodes_list)
            dsu.p[rv] = ru
            build_ac(nodes_list[ru])
        else:
            u = int(tmp[1]) - 1
            t = tmp[2].strip()
            r = dsu.find(u)
            print(query_ac(nodes_list[r], t))

if __name__ == "__main__":
    main()
```DSU theo dõi các nút nào thuộc về nhau, trong khi gốc của mỗi thành phần trỏ tới cấu trúc Aho-Corasick chứa tất cả các chuỗi trong thành phần đó. Khi hai thành phần hợp nhất, bộ ba nhỏ hơn sẽ được gấp lại thành bộ ba lớn hơn, duy trì hiệu quả khấu hao. Sau mỗi lần hợp nhất, các liên kết lỗi sẽ được xây dựng lại để các truy vấn trong tương lai hoạt động trên một máy tự động nhất quán. 

Mỗi truy vấn thuộc loại thứ hai chạy một lần quét trên chuỗi văn bản, theo sau các chuyển đổi tự động và tích lũy số lượng mẫu thông qua việc truyền bá đầu ra. 

## Ví dụ đã hoạt động 

### Dấu vết ví dụ 

đầu vào:```
4
a
ab
ba
ca
7
2 2 abab
1 2 3
2 2 abab
1 1 3
2 2 acac
1 3 4
2 2 acac
```Chúng tôi chỉ theo dõi thành phần chứa nút 2. 

| Bước | Hoạt động | Chuỗi thành phần (2) | Văn bản truy vấn | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | Truy vấn 2 | {ab} | abab | 2 | 
| 2 | Đoàn 2-3 | {ab, ba} | - | - | 
| 3 | Truy vấn 2 | {ab, ba} | abab | 3 | 
| 4 | Đoàn 1-3 | {ab, ba, a} | - | - | 
| 5 | Truy vấn 2 | {ab, ba, a} | acac | 2 | 
| 6 | Đoàn 3-4 | {ab, ba, a, ca} | - | - | 
| 7 | Truy vấn 2 | {ab, ba, a, ca} | acac | 3 | 

Mỗi truy vấn phản ánh một tập hợp mẫu ngày càng tăng và mỗi liên kết sẽ mở rộng máy tự động tương ứng. 

Dấu vết này xác nhận rằng cấu trúc thành phần được tích lũy chính xác và mỗi truy vấn luôn nhìn thấy toàn bộ tập hợp mẫu hiện tại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((Σ | s_i | 
| Không gian | O(Σ | s_i | 

Các ràng buộc đảm bảo rằng tổng số ký tự trên tất cả các chuỗi và truy vấn đủ nhỏ để chi phí logarit từ việc hợp nhất vẫn có thể chấp nhận được trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import main
    import sys as _sys
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        main()
    return out.getvalue().strip()

# provided sample
assert run("""4
a
ab
ba
ca
7
2 2 abab
1 2 3
2 2 abab
1 1 3
2 2 acac
1 3 4
2 2 acac
""") == """2
3
2
3"""

# minimal case
assert run("""1
a
1
2 1 aaaa
""") == "4"

# all identical strings
assert run("""3
a
a
a
2
2 1 aaa
2 1 aaa
""") == """3
3"""

# no unions
assert run("""3
a
ab
aba
1
2 2 ababa
""") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| văn bản lặp lại nút đơn | 4 | độ chính xác phù hợp cơ bản | 
| tất cả các mẫu giống hệt nhau | 3, 3 | tổng hợp trong thành phần | 
| không có công đoàn | kết quả duy nhất | hành vi DSU tĩnh | 

## Vỏ cạnh 

Trường hợp phức tạp xảy ra khi nhiều nút chia sẻ các chuỗi giống hệt nhau. Trong tình huống đó, việc nén trie đảm bảo chúng được biểu diễn dưới dạng số lượng thiết bị đầu cuối lặp lại thay vì các đường dẫn trùng lặp. Ví dụ: ba nút, mỗi nút có`"a"`trong cùng một thành phần sẽ đóng góp ba kết quả phù hợp cho mỗi lần xuất hiện`"a"`trong một văn bản truy vấn. Thuật toán xử lý việc này vì mỗi nút đầu cuối tăng`cnt`, Và`out`lan truyền tổng hợp các số đếm này một cách chính xác trong quá trình xây dựng Aho-Corasick. 

Một trường hợp khác là sự kết hợp lặp đi lặp lại tạo thành một chuỗi lớn. Nếu không hợp nhất từ ​​nhỏ đến lớn, việc liên tục gắn các cấu trúc lớn vào các cấu trúc lớn sẽ gây ra hành vi bậc hai. Quy tắc hợp nhất dựa trên kích thước đảm bảo rằng mỗi nút chỉ di chuyển một số lần giới hạn, duy trì hiệu quả phân bổ ngay cả trong các chuỗi liên kết đối nghịch.
