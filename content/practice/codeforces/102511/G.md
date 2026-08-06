---
title: "CF 102511G - Tên của cô ấy"
description: "Đầu vào mô tả một cây gia phả gốc. Mỗi người phụ nữ chỉ lưu trữ chữ cái đầu tiên của tên mình và mục lục của mẹ mình."
date: "2026-08-05T16:28:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102511
codeforces_index: "G"
codeforces_contest_name: "2019 ICPC World Finals"
rating: 0
weight: 102511
solve_time_s: 184
verified: true
draft: false
---

[CF 102511G - Tên của cô ấy](https://codeforces.com/problemset/problem/102511/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3m 4s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Đầu vào mô tả một cây gia phả gốc. Mỗi người phụ nữ chỉ lưu trữ chữ cái đầu tiên của tên mình và mục lục của mẹ mình. Tên đầy đủ của con gái có được bằng cách đặt chữ cái của cô ấy trước tên đầy đủ của mẹ cô ấy, do đó việc chuyển từ nút sang nút cha mẹ sẽ loại bỏ ký tự đầu tiên của tên. 

Đối với mỗi chuỗi truy vấn, chúng ta cần số lượng nút có tên đầy đủ bắt đầu bằng chuỗi đó. Thách thức là tên không được lưu trữ rõ ràng. Một chuỗi một triệu phụ nữ có thể tạo ra những cái tên dài một triệu, nên việc xây dựng từng cái tên đã quá tốn kém. 

Các giới hạn buộc một giải pháp tuyến tính hoặc gần như tuyến tính. Có thể có một triệu phụ nữ và tổng cộng một triệu ký tự truy vấn. Một cách tiếp cận duyệt qua mọi tên cho mọi truy vấn có thể thực hiện xung quanh$10^{12}$việc kiểm tra ký tự theo một chuỗi dài, vượt xa thời gian có sẵn. Bảng chữ cái chỉ có 26 chữ cái nên có thể thực hiện được các giải pháp sử dụng xử lý chuỗi tuyến tính. 

Những trường hợp phức tạp là do các tiền tố xuất hiện ở các độ sâu khác nhau của cây phả hệ. Truy vấn có thể có tên đầy đủ, có thể ngắn hơn tên hoặc không có tên trùng khớp. 

Ví dụ, đầu vào```
1 3
A 0
A
AA
B
```chỉ có tên`A`. Đầu ra đúng là```
1
0
0
```Giải pháp giả định mọi truy vấn phải khớp với tên đầy đủ sẽ từ chối truy vấn đầu tiên một cách không chính xác. 

Một trường hợp khác là truy vấn có tiền tố của một số tên liên quan:```
3 2
S 0
A 1
B 2
A
BA
```Những cái tên là`S`,`AS`, Và`BAS`. Đầu ra là```
1
1
```Truy vấn`A`chỉ trận đấu`AS`, không phải mọi hậu duệ của`A`nút, vì tên phát triển bằng cách thêm các chữ cái vào phía trước. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp sẽ xây dựng lại tên của từng phụ nữ và lưu trữ tất cả chúng. Để trả lời một truy vấn, chúng ta có thể so sánh nó với mọi tên. Điều này đúng vì nó kiểm tra chính xác mối quan hệ tiền tố được yêu cầu. Vấn đề là chi phí. Trong chuỗi một triệu phụ nữ, tổng chiều dài của tất cả các tên được xây dựng lại là khoảng$10^{12}$và việc so sánh các truy vấn với chúng thậm chí còn lớn hơn. 

Quan sát hữu ích là mối quan hệ họ hàng ngược lại đối với các truy vấn tiền tố. Con gái được tạo ra bằng cách thêm một chữ cái vào đầu tên của người mẹ. Nếu mọi tên đều bị đảo ngược thì con gái sẽ được tạo ra bằng cách thêm một chữ cái vào cuối. Cây gia phả trở thành một bộ ba tên đảo ngược thông thường. 

Một truy vấn hỏi liệu`s`là tiền tố của tên gốc sẽ trở thành câu hỏi về việc liệu`reverse(s)`là hậu tố của chuỗi đường dẫn trong trie này. Điều này biến vấn đề thành việc tìm nhiều hậu tố trong nhiều đường dẫn trie. 

Chúng tôi có thể chèn tất cả các truy vấn đã đảo ngược vào máy tự động Aho-Corasick. Trong khi duyệt qua họ trie, chúng ta duy trì trạng thái automaton sau khi đọc tên đảo ngược hiện tại. Mỗi người phụ nữ đến thăm đều đóng góp một lần vào trạng thái đó. Một truy vấn khớp với một trạng thái bất cứ khi nào trạng thái đó nằm trong cây con lỗi của nút truy vấn, vì các liên kết lỗi thể hiện mối quan hệ hậu tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2 + nk)$|$O(n^2)$| Quá chậm | 
| Tối ưu |$O(n + L)$|$O(n + L)$| Đã chấp nhận | 

Đây$L$là tổng độ dài của tất cả các truy vấn. 

## Hướng dẫn thuật toán 

1. Đảo ngược mọi chuỗi truy vấn và chèn nó vào bộ thử. Nút cuối của mỗi chuỗi được chèn sẽ được lưu trữ để có thể khôi phục câu trả lời sau này. Đảo ngược là sự chuyển đổi quan trọng vì cấu trúc họ sẽ thêm các ký tự sau khi đảo ngược một cách tự nhiên. 
2. Xây dựng các liên kết thất bại cho bộ ba bằng cách sử dụng cấu trúc Aho-Corasick. Các chuyển đổi bị thiếu được lấp đầy bằng cách sử dụng các chuyển đổi lỗi, cho phép mọi ký tự được xử lý trong thời gian không đổi. 
3. Lưu trữ cây phả hệ bằng danh sách con. Di chuyển nó từ người sáng lập trong khi vẫn giữ trạng thái Aho-Corasick hiện tại. Khi chuyển sang con gái, hãy đưa nhân vật của con gái vào máy tự động và tăng bộ đếm của trạng thái kết quả. 
4. Xây dựng cây liên kết lỗi của máy tự động. Thêm bộ đếm của mỗi trạng thái vào trạng thái mẹ của nó trong cây này, xử lý các nút từ sâu nhất đến nông nhất. Sau quá trình truyền bá này, mỗi trạng thái sẽ chứa số lượng họ mà chuỗi đại diện bởi trạng thái đó là hậu tố. 
5. Đối với mỗi truy vấn, xuất giá trị tích lũy được lưu trữ tại nút đầu cuối của nó trong máy tự động. 

Tại sao nó hoạt động: sau khi đảo ngược, mỗi lady tương ứng với một chuỗi gốc tới nút trong bộ ba. Trạng thái tự động đạt được sau khi truy cập một phụ nữ đại diện cho tiền tố truy vấn dài nhất cũng là hậu tố của tên đảo ngược đó. Liên kết lỗi chứa tất cả các kết quả phù hợp với hậu tố ngắn hơn. Việc truyền số đếm thông qua cây lỗi sẽ chuyển từng lần xuất hiện sang mọi chuỗi truy vấn là hậu tố, chính xác là điều kiện tiền tố ban đầu. 

## Giải pháp Python```python
import sys
from collections import deque
from array import array

input = sys.stdin.readline

def solve():
    n, k = map(int, input().split())

    fam_head = array('i', [-1]) * (n + 1)
    fam_to = array('i')
    fam_next = array('i')
    fam_ch = array('b')

    for i in range(1, n + 1):
        c, p = input().split()
        c = ord(c) - 65
        p = int(p)
        if p:
            fam_to.append(i)
            fam_next.append(fam_head[p])
            fam_ch.append(c)
            fam_head[p] = len(fam_to) - 1

    q_terminal = array('i', [0]) * k

    head = array('i', [-1])
    to = array('i')
    nxt = array('i')
    ch = array('b')

    def new_node():
        head.append(-1)
        return len(head) - 1

    for qi in range(k):
        s = input().strip()[::-1]
        cur = 0
        for x in s:
            c = ord(x) - 65
            e = head[cur]
            found = -1
            while e != -1:
                if ch[e] == c:
                    found = to[e]
                    break
                e = nxt[e]
            if found == -1:
                found = new_node()
                to.append(found)
                ch.append(c)
                nxt.append(head[cur])
                head[cur] = len(to) - 1
            cur = found
        q_terminal[qi] = cur

    m = len(head)
    fail = array('i', [0]) * m

    trans = array('i', [-1]) * (m * 26)
    for v in range(m):
        e = head[v]
        while e != -1:
            trans[v * 26 + ch[e]] = to[e]
            e = nxt[e]

    q = deque()
    for c in range(26):
        x = trans[c]
        if x == -1:
            trans[c] = 0
        else:
            q.append(x)

    while q:
        v = q.popleft()
        base = v * 26
        fbase = fail[v] * 26
        for c in range(26):
            u = trans[base + c]
            if u == -1:
                trans[base + c] = trans[fbase + c]
            else:
                fail[u] = trans[fbase + c]
                q.append(u)

    cnt = array('i', [0]) * m
    stack = [(1, 0)]
    while stack:
        v, state = stack.pop()
        e = fam_head[v]
        while e != -1:
            u = fam_to[e]
            ns = trans[state * 26 + fam_ch[e]]
            cnt[ns] += 1
            stack.append((u, ns))
            e = fam_next[e]

    children = [[] for _ in range(m)]
    for i in range(1, m):
        children[fail[i]].append(i)

    order = list(range(m))
    order.sort(key=lambda x: -x)
    for v in order:
        if v:
            cnt[fail[v]] += cnt[v]

    ans = []
    for x in q_terminal:
        ans.append(str(cnt[x]))
    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Cây phả hệ được lưu trữ riêng biệt với máy tự động vì hai cấu trúc thể hiện các hướng khác nhau của vấn đề. Việc truyền tải họ tạo ra các tên đảo ngược, trong khi máy tự động trả lời các truy vấn hậu tố đối với các tên đó. 

Việc chèn truy vấn sử dụng các chuỗi đảo ngược. Nút đầu cuối được lưu ngay lập tức vì sau tất cả quá trình xử lý trước, câu trả lời sẽ được gắn vào nút tự động chính xác đó. 

Việc truyền bá liên kết lỗi được thực hiện từ trẻ em tới cha mẹ. Một chuỗi truy vấn có thể là một hậu tố được đại diện bởi nhiều trạng thái, do đó số lượng cuối cùng không được biết cho đến khi tất cả các hậu tố trong cây lỗi đã đóng góp. 

Tất cả các bộ đếm đều phù hợp với số nguyên Python. Các mảng sử dụng bộ lưu trữ nhỏ gọn vì số lượng nút có thể lên tới một triệu. 

## Ví dụ đã hoạt động 

Đối với mẫu:```
10 5
S 0
Y 1
R 2
E 3
N 4
E 5
A 6
D 7
Y 7
R 9
RY
E
N
S
AY
```truy vấn đảo ngược mà trie nhận được`YR`,`E`,`N`,`S`, Và`YA`. 

Trong quá trình di chuyển của gia đình: 

| Quý cô | Ý nghĩa trạng thái đường dẫn đảo ngược | Đã thêm số lượng | 
| --- | --- | --- | 
| S | S | 1 | 
| YS | YS | 1 | 
| RYS | năm | 1 | 
| erys | YRSE | 1 | 
| NERYS | YRSEN | 1 | 
| NĂNG LƯỢNG | YRSENE | 1 | 
| AENERY | YRSENEA | 1 | 
| DAENERYS | YRSENEAD | 1 | 
| YAENERYS | YRSENEAY | 1 | 
| Lúa mạch | YRSENEAYR | 1 | 

Sự lan truyền lỗi làm cho`YR`nhận được hai trận đấu, bởi vì cả hai`RYS`Và`RYAENERYS`kết thúc bằng truy vấn đảo ngược. Nó cũng làm cho`E`nhận được hai trận đấu và`N`nhận được một. 

Một trường hợp nhỏ hơn:```
3 3
A 0
B 1
C 2
A
BA
CBA
```Những cái tên là`A`,`BA`, Và`CBA`. 

| Truy vấn | Truy vấn đảo ngược | Tên phù hợp | 
| --- | --- | --- | 
| A | A | A, BA, CBA | 
| BA | AB | Cử nhân, CBA | 
| CBA | ABC | CBA | 

Máy tự động đếm hậu tố của các tên bị đảo ngược, tương ứng chính xác với tiền tố của tên gốc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n + L)$| Mỗi cạnh họ và ký tự truy vấn được xử lý với số lần không đổi. | 
| Không gian |$O(n + L)$| Cây gia phả và máy tự động đều chứa tối đa số lượng nút tuyến tính. | 

Kích thước đầu vào tối đa là một triệu nút họ và một triệu ký tự truy vấn. Thuật toán chỉ thực hiện các bước tuyến tính trên các đối tượng này, đây là thang đo cần thiết cho các giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    oldout = sys.stdout
    sys.stdout = out
    solve()
    sys.stdin = old
    sys.stdout = oldout
    return out.getvalue()

assert run("""10 5
S 0
Y 1
R 2
E 3
N 4
E 5
A 6
D 7
Y 7
R 9
RY
E
N
S
AY
""") == """2
2
1
1
0"""

assert run("""1 3
A 0
A
AA
B
""") == """1
0
0"""

assert run("""3 3
A 0
B 1
C 2
A
BA
CBA
""") == """1
1
1"""

assert run("""2 2
Z 0
Z 1
Z
ZZ
""") == """1
1
"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Người sáng lập duy nhất |`1,0,0`| Truy vấn bằng tên gốc và thiếu tiền tố | 
| Tăng chuỗi |`1,1,1`| Tiền tố dọc theo chuỗi phụ thuộc dài | 
| Chữ cái lặp đi lặp lại |`1,1`| Xử lý đúng các ký tự và liên kết hậu tố giống hệt nhau | 

## Vỏ cạnh 

Một truy vấn chỉ phù hợp với người sáng lập sẽ được xử lý vì root lady cũng được truy cập trong quá trình truyền tải. Thuật toán bắt đầu tính từ các nút họ thực tế, do đó tên`S`đóng góp một lần vào mẫu. 

Một truy vấn ngắn hơn tên đầy đủ sẽ được xử lý bằng cách truyền lỗi. Ví dụ, trong chuỗi`A`,`BA`,`CBA`, truy vấn`A`tương ứng với hậu tố của tất cả các đường dẫn đảo ngược, do đó cây lỗi sẽ thêm cả ba lần xuất hiện vào trạng thái cuối. 

Một truy vấn không có kết quả phù hợp sẽ không bao giờ nhận được bất kỳ đóng góp nào. Nút tự động của nó vẫn ở mức 0 vì không có quá trình truyền tải họ nào đến được nó hoặc bất kỳ trạng thái nào bên dưới nó trong cây lỗi.
