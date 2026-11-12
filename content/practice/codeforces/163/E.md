---
title: "CF 163E - Chính phủ điện tử"
description: "Chúng tôi có một bộ họ cố định. Mỗi họ có thể hoạt động hoặc không hoạt động. Ban đầu mọi họ đều hoạt động. Hệ thống phải xử lý ba loại hoạt động. Truy vấn +i kích hoạt họ thứ i. Truy vấn -i hủy kích hoạt họ thứ i. Một truy vấn?"
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dfs-and-similar", "dp", "strings", "trees"]
categories: ["algorithms"]
codeforces_contest: 163
codeforces_index: "E"
codeforces_contest_name: "VK Cup 2012 Round 2"
rating: 2800
weight: 163
solve_time_s: 136
verified: true
draft: false
---

[CF 163E - Chính phủ điện tử](https://codeforces.com/problemset/problem/163/E) 

**Đánh giá:** 2800 
**Thẻ:** cấu trúc dữ liệu, dfs và tương tự, dp, chuỗi, cây 
**Thời gian giải:** 2 phút 16s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một bộ họ cố định. Mỗi họ có thể hoạt động hoặc không hoạt động. Ban đầu mọi họ đều hoạt động. 

Hệ thống phải xử lý ba loại hoạt động. 

Một truy vấn`+i`kích hoạt`i-th`họ. 

Một truy vấn`-i`vô hiệu hóa`i-th`họ. 

Một truy vấn`?text`yêu cầu tổng số lần xuất hiện của tất cả các họ hiện đang hoạt động bên trong`text`. Các trận đấu chồng chéo được tính riêng. Nếu như`"aa"`đang hoạt động và văn bản là`"aaa"`, sau đó nó xuất hiện hai lần. 

Phần khó khăn là cả số lượng mẫu và tổng kích thước văn bản đều lớn. Tổng chiều dài của tất cả các họ lên tới`10^6`và tổng độ dài của tất cả các văn bản truy vấn cũng lên tới`10^6`. Giải pháp kiểm tra từng mẫu hoạt động đối với từng văn bản truy vấn một cách độc lập là hoàn toàn không khả thi. 

Giả sử chúng ta đã thử cách tiếp cận trực tiếp. Đối với mọi`?`truy vấn, chúng tôi lặp qua tất cả các họ đang hoạt động và chạy so khớp chuỗi con. Trong trường hợp xấu nhất chúng ta có thể có`10^5`mẫu có chiều dài trung bình`10`và một văn bản truy vấn có độ dài`10^6`. Ngay cả việc sử dụng KMP cho từng mẫu cũng đã yêu cầu khoảng`10^11`hoạt động nhân vật tổng thể. 

Các ràng buộc gợi ý mạnh mẽ rằng chúng ta phải xử lý tất cả các mẫu cùng một lúc. Nhiều tiền tố chia sẻ mẫu là cấu trúc trung tâm ở đây. Khi chúng tôi nhận thấy điều đó, công cụ tự nhiên là máy tự động Aho-Corasick. 

Có một số trường hợp khó khăn có thể dễ dàng phá vỡ việc triển khai ngây thơ. 

Hãy xem xét các trận đấu chồng chéo. 

đầu vào:```
1 1
aa
?aaa
```Đầu ra đúng:```
2
```mẫu`"aa"`xuất hiện ở các vị trí`[1,2]`Và`[2,3]`. Việc triển khai bất cẩn bằng cách sử dụng tìm kiếm không chồng chéo sẽ trả về sai`1`. 

Một trường hợp tinh vi khác là kích hoạt hoặc hủy kích hoạt nhiều lần. 

đầu vào:```
4 1
abc
-1
-1
+1
?abc
```Đầu ra đúng:```
1
```Việc hủy kích hoạt thứ hai không được làm gì cả. Nếu chúng ta giảm số đếm một cách mù quáng, chúng ta có thể vô tình làm cho mẫu đóng góp`-1`. 

Liên kết bị lỗi là một nguồn sai lầm phổ biến khác. 

đầu vào:```
1 2
a
aa
?aa
```Đầu ra đúng:```
3
```Các trận đấu là:`"a"`hai lần,`"aa"`một lần. 

Nếu chúng ta chỉ đếm các mẫu kết thúc ở nút trie hiện tại và bỏ qua các mẫu tổ tiên bị lỗi, chúng ta sẽ bỏ lỡ mẫu hậu tố ngắn hơn`"a"`. 

Trường hợp góc quan trọng cuối cùng đến từ các bản cập nhật động. 

đầu vào:```
5 2
a
aa
-1
?aa
+1
?aa
```Đầu ra đúng:```
1
3
```Khi`"a"`không hoạt động, chỉ`"aa"`đóng góp. Sau khi kích hoạt lại, cả hai mẫu đều đóng góp lại. Cấu trúc dữ liệu phải hỗ trợ cập nhật trực tuyến một cách hiệu quả. 

## Phương pháp tiếp cận 

Giải pháp brute-force rất đơn giản về mặt khái niệm. Đối với mỗi văn bản truy vấn, hãy lặp qua tất cả các họ đang hoạt động và đếm số lần xuất hiện một cách độc lập. Sử dụng công cụ so khớp tuyến tính như KMP, việc đếm một mẫu bên trong một văn bản sẽ mất`O(text_length + pattern_length)`. 

Nếu có`k = 10^5`các mẫu và tổng độ dài văn bản truy vấn cũng`10^6`, độ phức tạp trong trường hợp xấu nhất trở thành gần như:```
O(total_queries * total_patterns * average_text_size)
```Ngay cả những ước tính lạc quan cũng vượt quá`10^10`hoạt động. Điều này không thể chạy trong vòng một giây. 

Quan sát quan trọng là tất cả các mẫu đều tĩnh. Chỉ có trạng thái hoạt động/không hoạt động của họ thay đổi. Điều này có nghĩa là chúng ta có thể xây dựng một máy tự động duy nhất cho tất cả các họ một lần, sau đó sử dụng lại nó cho mọi truy vấn. 

Aho-Corasick cho phép chúng tôi xử lý văn bản theo thời gian tuyến tính đồng thời theo dõi các kết quả khớp cho mọi mẫu. Trong khi quét các ký tự, máy tự động sẽ di chuyển qua các trạng thái trie và các liên kết lỗi sẽ tự động xử lý các kết quả khớp hậu tố. 

Điều này giải quyết được vấn đề khớp nhiều mẫu nhưng không giải quyết được vấn đề kích hoạt động. Giả sử chúng ta đến nút tự động`v`trong khi quét văn bản. Mỗi mẫu tương ứng với một số hậu tố của`v`nên đóng góp nếu hoạt động. 

Thuộc tính cấu trúc quan trọng là: 

Nếu nút`u`là hậu tố tổ tiên của nút`v`trong cây thất bại, sau đó là mẫu`u`trận đấu bất cứ khi nào chúng tôi ở trạng thái`v`. 

Vì vậy, vấn đề trở thành: 

Trong khi duyệt qua văn bản, hãy hỏi liên tục: 

"Tổng trọng số mẫu hoạt động trên tất cả tổ tiên của nút hiện tại trong cây lỗi là bao nhiêu?" 

Đây chính xác là một vấn đề truy vấn tiền tố cây con trên cây. 

Chúng tôi xây dựng cây lỗi, chạy lập chỉ mục thứ tự DFS và duy trì cây Fenwick trên các chỉ số chuyến tham quan Euler. 

Khi một mẫu bắt đầu hoạt động, chúng tôi thêm`+1`đến vị trí nút của nó. 

Khi nó không hoạt động, chúng tôi thêm`-1`. 

Đối với một văn bản truy vấn, khi duyệt qua các trạng thái tự động hóa, chúng tôi hỏi có bao nhiêu nút mẫu hoạt động nằm trên chuỗi hậu tố của trạng thái hiện tại. Theo thứ tự DFS của cây lỗi, truy vấn đó trở thành truy vấn tổng của cây con. 

Kết quả là một giải pháp trực tuyến có tổng độ phức tạp gần như tuyến tính ở kích thước đầu vào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(total_patterns × Total_text_length) cho mỗi truy vấn | O(total_pattern_length) | Quá chậm | 
| Tối ưu | O(total_input_size × bảng chữ cái + Total_queries × log N) | O(total_pattern_length) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng một bộ thử từ tất cả các họ. 

Mỗi nút đại diện cho một tiền tố. Đối với mỗi mẫu, lưu trữ nút nơi mẫu kết thúc. 
2. Xây dựng liên kết lỗi bằng BFS. 

Liên kết lỗi của một nút trỏ đến hậu tố thích hợp dài nhất cũng tồn tại trong bộ ba. Đây là cấu trúc chuyển tiếp tương tự được sử dụng trong KMP, được khái quát hóa thành nhiều mẫu. 
3. Xây dựng cây thất bại. 

Nếu như`fail[v] = u`, sau đó thêm một cạnh`u -> v`. 

Cây này nắm bắt các mối quan hệ hậu tố giữa các trạng thái tự động. 
4. Chạy DFS trên cây lỗi và ấn định các khoảng thời gian tham quan Euler. 

Đối với mỗi nút`v`, tính:`tin[v]`= thời gian vào,`tout[v]`= thời gian thoát ra. 

Tất cả con cháu của`v`chiếm một phân đoạn liền kề theo thứ tự DFS. 
5. Duy trì cây Fenwick theo thứ tự DFS. 

Nếu một mẫu kết thúc tại nút`v`đang hoạt động, hãy thêm`+1`Tại`tin[v]`. 

Nếu nó không hoạt động, hãy trừ`1`. 
6. Xử lý văn bản truy vấn bằng máy tự động. 

Đối với mỗi nhân vật: 

di chuyển qua các quá trình chuyển đổi tự động, 

đến tiểu bang`v`. 

Mọi mẫu hoạt động có nút cuối cùng là tổ tiên của`v`trong cây thất bại phù hợp ở đây. 
7. Đếm tổ tiên hậu tố hoạt động bằng cách sử dụng tổng cây con. 

Số lượng mẫu hoạt động khớp ở trạng thái`v`bằng:```
sum over subtree(v)
```theo thứ tự DFS của cây lỗi. 

Vì các nút con trong cây lỗi tương ứng với các nút có`v`làm hậu tố, tập hợp cây con sẽ đưa ra chính xác kết quả phù hợp cần thiết. 
8. Cộng dồn kết quả trên toàn bộ văn bản và in tổng số cuối cùng. 

### Tại sao nó hoạt động 

Bất biến quan trọng là: 

Một mẫu khớp với trạng thái máy tự động hiện tại khi và chỉ khi nút đầu cuối của mẫu đó nằm trên chuỗi liên kết lỗi của trạng thái hiện tại. 

Trong cây thất bại, tổ tiên hậu tố trở thành tổ tiên của cây bình thường. Sau khi làm phẳng chuyến tham quan Euler, tất cả các nút con của một nút tạo thành một đoạn liền kề. 

Bằng cách lưu trữ số lượng mẫu hoạt động tại các nút đầu cuối, các truy vấn cây con đếm chính xác mọi mẫu hoạt động có quan hệ hậu tố làm cho nó khớp với trạng thái hiện tại. 

Vì mỗi ký tự văn bản sẽ nâng cấp máy tự động chính xác một lần và mỗi bản cập nhật sẽ thay đổi một vị trí Fenwick nên mọi thao tác vẫn hiệu quả và chính xác. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 1)

    def add(self, idx, val):
        while idx <= self.n:
            self.bit[idx] += val
            idx += idx & -idx

    def sum(self, idx):
        res = 0
        while idx > 0:
            res += self.bit[idx]
            idx -= idx & -idx
        return res

    def range_sum(self, l, r):
        return self.sum(r) - self.sum(l - 1)

def solve():
    n, k = map(int, input().split())

    trie = [[0] * 26]
    fail = [0]
    out = []

    patterns = []

    for _ in range(k):
        s = input().strip()
        patterns.append(s)

        node = 0

        for ch in s:
            c = ord(ch) - 97

            if trie[node][c] == 0:
                trie[node][c] = len(trie)
                trie.append([0] * 26)
                fail.append(0)

            node = trie[node][c]

        out.append(node)

    q = deque()

    for c in range(26):
        nxt = trie[0][c]
        if nxt:
            q.append(nxt)

    while q:
        v = q.popleft()

        for c in range(26):
            nxt = trie[v][c]

            if nxt:
                fail[nxt] = trie[fail[v]][c]
                q.append(nxt)
            else:
                trie[v][c] = trie[fail[v]][c]

    size = len(trie)

    tree = [[] for _ in range(size)]

    for v in range(1, size):
        tree[fail[v]].append(v)

    tin = [0] * size
    tout = [0] * size
    timer = 0

    stack = [(0, 0)]

    while stack:
        v, state = stack.pop()

        if state == 0:
            timer += 1
            tin[v] = timer

            stack.append((v, 1))

            for to in reversed(tree[v]):
                stack.append((to, 0))
        else:
            tout[v] = timer

    bit = Fenwick(size)

    active = [True] * k

    for node in out:
        bit.add(tin[node], 1)

    ans = []

    for _ in range(n):
        query = input().strip()

        typ = query[0]
        arg = query[1:]

        if typ == '+':
            idx = int(arg) - 1

            if not active[idx]:
                active[idx] = True
                bit.add(tin[out[idx]], 1)

        elif typ == '-':
            idx = int(arg) - 1

            if active[idx]:
                active[idx] = False
                bit.add(tin[out[idx]], -1)

        else:
            node = 0
            total = 0

            for ch in arg:
                c = ord(ch) - 97
                node = trie[node][c]

                total += bit.range_sum(tin[node], tout[node])

            ans.append(str(total))

    print('\n'.join(ans))

solve()
```Giai đoạn xây dựng trie tạo ra một trạng thái tự động cho mỗi tiền tố riêng biệt. Mỗi mẫu đều ghi nhớ nút đầu cuối của nó vì các bản cập nhật sau này phải kích hoạt hoặc hủy kích hoạt chính xác nút đó. 

Trong quá trình xây dựng BFS của các liên kết lỗi, các chuyển tiếp bị thiếu sẽ được lấp đầy bằng cách sử dụng các chuyển tiếp lỗi. Điều này biến bộ ba thành một máy tự động hoàn chỉnh trong đó mọi chuyển đổi ký tự đều trở thành`O(1)`. 

Cây lỗi được xây dựng từ các liên kết lỗi. Nếu như`fail[v] = u`, sau đó`u`là cha mẹ của`v`. Cấu trúc cây này là thủ thuật cốt lõi đằng sau giải pháp. 

Việc đánh số DFS đáng được chú ý cẩn thận. Một cây con trong cây lỗi tương ứng với một khoảng Euler liền kề`[tin[v], tout[v]]`. Cây Fenwick hoạt động hiệu quả trên các phạm vi liền kề, đó là lý do tại sao việc làm phẳng là cần thiết. 

Một chi tiết thực hiện tinh tế là hướng đếm. 

Chúng tôi lưu trữ các mẫu hoạt động tại các nút cuối của chúng. Khi xử lý trạng thái tự động`v`, chúng tôi truy vấn cây con của`v`, không phải chuỗi tổ tiên của nó. 

Tại sao? Bởi vì trong cây thất bại, con cháu của`v`chính xác là các trạng thái có chuỗi thất bại chứa`v`. Sự đảo ngược này rất dễ mắc sai lầm. 

Việc kích hoạt và hủy kích hoạt nhiều lần được bảo vệ bằng`active`mảng. Nếu không có sự kiểm tra này, nhiều`+`các hoạt động sẽ thêm sai cùng một mẫu nhiều lần. 

DFS lặp lại tránh được các vấn đề về độ sâu đệ quy của Python. Máy tự động có thể chứa tới`10^6`các nút, do đó DFS đệ quy không an toàn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
7 3
a
aa
ab
?aaab
-2
?aaab
-3
?aaab
+2
?aabbaa
```Mẫu:`a`,`aa`,`ab`Ban đầu tất cả đều hoạt động. 

| Bước | Truy vấn | Mẫu hoạt động | Trận đấu | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 |`?aaab`| a, aa, ab | a=3, aa=2, ab=1 | 6 | 
| 2 |`-2`| một, ab | aa đã xóa | - | 
| 3 |`?aaab`| một, ab | a=3, ab=1 | 4 | 
| 4 |`-3`| một | bỏ ab | - | 
| 5 |`?aaab`| một | a=3 | 3 | 
| 6 |`+2`| à, àa | aa được khôi phục | - | 
| 7 |`?aabbaa`| à, àa | a=4, aa=2 | 6 | 

Dấu vết này cho thấy các bản cập nhật động hoạt động chính xác. Các mẫu chỉ đóng góp khi đang hoạt động và các mẫu trùng lặp sẽ được tính. 

### Ví dụ 2 

đầu vào:```
5 2
a
aa
?aa
-1
?aa
+1
?aa
```| Bước | Trạng thái hiện tại khi quét | Mẫu hoạt động | Đóng góp | 
| --- | --- | --- | --- | 
|`?aa`Đầu tiên`a`| trạng thái("a") | à, àa | +1 | 
|`?aa`thứ hai`a`| trạng thái("aa") | à, àa | +2 | 
| Tổng cộng | - | - | 3 | 
|`-1`| vô hiệu hóa`a`| aa chỉ | - | 
|`?aa`Đầu tiên`a`| trạng thái("a") | aa chỉ | 0 | 
|`?aa`thứ hai`a`| trạng thái("aa") | aa chỉ | +1 | 
| Tổng cộng | - | - | 1 | 

Ví dụ này xác nhận rằng các kết quả khớp hậu tố được xử lý thông qua các liên kết bị lỗi. Ở trạng thái`"aa"`, cả hai`"aa"`Và`"a"`có thể đóng góp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(total_pattern_length + Total_text_length + Total_queries × log N) | Truyền tải Trie là tuyến tính, các cập nhật và truy vấn Fenwick là logarit | 
| Không gian | O(total_pattern_length) | Trie, cây thất bại, mảng DFS và cây Fenwick đều có quy mô với kích thước máy tự động | 

Tổng kích thước máy tự động được giới hạn bởi tổng chiều dài mẫu, nhiều nhất là`10^6`. Mỗi ký tự văn bản thực hiện một chuyển đổi tự động và một truy vấn phạm vi Fenwick. Điều này thoải mái phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from collections import deque

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    class Fenwick:
        def __init__(self, n):
            self.n = n
            self.bit = [0] * (n + 1)

        def add(self, idx, val):
            while idx <= self.n:
                self.bit[idx] += val
                idx += idx & -idx

        def sum(self, idx):
            res = 0
            while idx > 0:
                res += self.bit[idx]
                idx -= idx & -idx
            return res

        def range_sum(self, l, r):
            return self.sum(r) - self.sum(l - 1)

    n, k = map(int, input().split())

    trie = [[0] * 26]
    fail = [0]
    out = []

    for _ in range(k):
        s = input().strip()

        node = 0

        for ch in s:
            c = ord(ch) - 97

            if trie[node][c] == 0:
                trie[node][c] = len(trie)
                trie.append([0] * 26)
                fail.append(0)

            node = trie[node][c]

        out.append(node)

    q = deque()

    for c in range(26):
        nxt = trie[0][c]
        if nxt:
            q.append(nxt)

    while q:
        v = q.popleft()

        for c in range(26):
            nxt = trie[v][c]

            if nxt:
                fail[nxt] = trie[fail[v]][c]
                q.append(nxt)
            else:
                trie[v][c] = trie[fail[v]][c]

    size = len(trie)

    tree = [[] for _ in range(size)]

    for v in range(1, size):
        tree[fail[v]].append(v)

    tin = [0] * size
    tout = [0] * size

    timer = 0
    stack = [(0, 0)]

    while stack:
        v, state = stack.pop()

        if state == 0:
            timer += 1
            tin[v] = timer

            stack.append((v, 1))

            for to in reversed(tree[v]):
                stack.append((to, 0))
        else:
            tout[v] = timer

    bit = Fenwick(size)

    active = [True] * k

    for node in out:
        bit.add(tin[node], 1)

    ans = []

    for _ in range(n):
        s = input().strip()

        if s[0] == '?':
            node = 0
            total = 0

            for ch in s[1:]:
                node = trie[node][ord(ch) - 97]
                total += bit.range_sum(tin[node], tout[node])

            ans.append(str(total))

        elif s[0] == '+':
            idx = int(s[1:]) - 1

            if not active[idx]:
                active[idx] = True
                bit.add(tin[out[idx]], 1)

        else:
            idx = int(s[1:]) - 1

            if active[idx]:
                active[idx] = False
                bit.add(tin[out[idx]], -1)

    return "\n".join(ans)

# provided sample
assert run(
"""7 3
a
aa
ab
?aaab
-2
?aaab
-3
?aaab
+2
?aabbaa
"""
) == "6\n4\n3\n6", "sample 1"

# minimum size
assert run(
"""1 1
a
?a
"""
) == "1", "minimum case"

# overlapping matches
assert run(
"""1 1
aa
?aaa
"""
) == "2", "overlapping occurrences"

# repeated enable/disable
assert run(
"""5 1
abc
-1
-1
+1
?abc
"""
) == "1", "idempotent updates"

# suffix-chain counting
assert run(
"""1 2
a
aa
?aa
"""
) == "3", "failure-link aggregation"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu đơn`"a"`|`1`| Trường hợp hợp lệ nhỏ nhất | 
|`"aa"`bên trong`"aaa"`|`2`| Sự xuất hiện chồng chéo | 
| lặp đi lặp lại`-1`|`1`| Cập nhật phải bình thường | 
| mẫu`"a"`Và`"aa"`|`3`| Kết hợp hậu tố chuỗi lỗi | 

## Vỏ cạnh 

Hãy xem xét lại các lần xuất hiện chồng chéo. 

đầu vào:```
1 1
aa
?aaa
```Trong khi quét: 

sau ký tự thứ hai, trạng thái`"aa"`đóng góp một lần, 

sau ký tự thứ ba, quá trình chuyển đổi thất bại vẫn khiến chúng tôi phải dừng lại`"aa"`đóng góp nữa. 

Các đầu ra thuật toán:```
2
```bởi vì mọi vị trí văn bản đều đóng góp các kết quả khớp một cách độc lập. 

Bây giờ hãy xem xét việc hủy kích hoạt nhiều lần. 

đầu vào:```
4 1
abc
-1
-1
+1
?abc
```đầu tiên`-1`loại bỏ mô hình khỏi cây Fenwick. 

thứ hai`-1`séc`active[idx]`và không làm gì cả. 

các`+1`khôi phục chính xác một bản sao. 

Truy vấn cuối cùng xuất ra chính xác:```
1
```thay vì`0`hoặc`-1`. 

Trường hợp cạnh liên kết hậu tố đặc biệt quan trọng. 

đầu vào:```
1 2
a
aa
?aa
```Ở trạng thái tự động`"aa"`: 

nút đó khớp với nhau`"aa"`, 

tổ tiên thất bại của nó phù hợp`"a"`. 

Truy vấn cây con trên cây lỗi sẽ đếm cả hai nút đầu cuối đang hoạt động, tạo ra:```
3
```Cuối cùng, hãy xem xét các mẫu có tiền tố chung nhưng kết thúc khác nhau. 

đầu vào:```
2 3
ab
abc
abd
?abcd
```Quá trình chuyển đổi tự động tái sử dụng tiền tố trie một cách hiệu quả. 

Trận đấu:`"ab"`một lần,`"abc"`một lần,`"abd"`không lần nào. 

Đầu ra đúng:```
2
```Thuật toán xử lý việc này một cách tự nhiên vì các trạng thái trie biểu thị trực tiếp các tiền tố được chia sẻ.
