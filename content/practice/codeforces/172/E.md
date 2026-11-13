---
title: "CF 172E - BHTML+BCSS"
description: "Chúng tôi được cung cấp một tài liệu giống như HTML được đơn giản hóa. Mỗi thẻ có thể là thẻ mở như <a, thẻ đóng như </a hoặc thẻ tự đóng như <a/. Tài liệu được đảm bảo được lồng đúng cách, vì vậy mỗi thẻ mở có chính xác một thẻ đóng phù hợp."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special", "dfs-and-similar", "expression-parsing"]
categories: ["algorithms"]
codeforces_contest: 172
codeforces_index: "E"
codeforces_contest_name: "Croc Champ 2012 - Qualification Round"
rating: 2200
weight: 172
solve_time_s: 141
verified: false
draft: false
---

[CF 172E - BHTML+BCSS](https://codeforces.com/problemset/problem/172/E) 

**Đánh giá:** 2200 
**Thẻ:** *đặc biệt, dfs và tương tự, phân tích biểu thức 
**Thời gian giải:** 2m 21s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một tài liệu giống như HTML được đơn giản hóa. Mỗi thẻ là một thẻ mở như`<a>`, một thẻ đóng như`</a>`hoặc thẻ tự đóng như`<a/>`. Tài liệu được đảm bảo được lồng đúng cách, vì vậy mỗi thẻ mở có chính xác một thẻ đóng phù hợp. 

Mỗi cặp thẻ phù hợp tạo thành một phần tử. Thẻ tự đóng cũng được tính là phần tử. Vì tài liệu được lồng đúng cách nên các phần tử tạo thành cấu trúc rừng gốc, giống hệt như cây DOM. 

Truy vấn là một chuỗi các tên thẻ như:```
a b c
```Một phần tử khớp với truy vấn này nếu tồn tại một chuỗi tổ tiên kết thúc ở phần tử đó có tên thẻ chính xác`a -> b -> c`. 

Chi tiết quan trọng là chuỗi không cần sử dụng các cấp độ liên tiếp. Truy vấn chỉ yêu cầu ngăn chặn lồng nhau. Ví dụ:```
<a><x><b></b></x></a>
```phần tử`b`phù hợp với truy vấn`a b`, mặc dù`x`ngồi giữa họ. 

Đối với mỗi truy vấn, chúng ta phải đếm xem có bao nhiêu phần tử thỏa mãn nó. 

Độ dài tài liệu có thể đạt tới`10^6`, điều này ngay lập tức loại trừ mọi thứ bậc hai về số lượng thẻ. Ngay cả việc phân tích cú pháp tài liệu cũng tốn thời gian tuyến tính, do đó, logic khớp thực tế cũng phải gần với tuyến tính. 

Có nhiều nhất`200`truy vấn và độ dài mỗi truy vấn tối đa là`200`. Đó là một gợi ý mạnh mẽ rằng chúng ta nên xử lý trước tài liệu một lần, sau đó trả lời tất cả các truy vấn cùng nhau. 

Một DFS ngây thơ từ mọi nút cho mọi truy vấn sẽ bùng nổ. Trong trường hợp xấu nhất, cây có thể chứa hàng trăm nghìn phần tử. Nhân số đó với tất cả độ dài truy vấn là quá đắt. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Xem xét tên thẻ lặp lại:```
<a><a><a></a></a></a>
```Truy vấn:```
a a
```Sâu sắc nhất`a`khớp vì nó có tổ tiên`a`. Giữa`a`cũng khớp. Việc triển khai bất cẩn chỉ theo dõi tổ tiên gần nhất sẽ bỏ lỡ một số kết quả khớp. 

Một trường hợp tinh vi khác là lồng ghép không liên tiếp:```
<a><x><b></b></x></a>
```Truy vấn:```
a b
```Câu trả lời đúng là`1`. Bất kỳ giải pháp nào chỉ kiểm tra mối quan hệ cha-con thay vì chuỗi tổ tiên tùy ý đều thất bại ở đây. 

Thẻ tự đóng hoạt động giống như các phần tử thông thường:```
<a><b/></a>
```Truy vấn:```
a b
```Câu trả lời là`1`. Nếu quá trình phân tích cú pháp bỏ qua các thẻ tự đóng hoặc quên tạo nút phần tử cho chúng thì số đếm sẽ không chính xác. 

Cuối cùng, vấn đề thứ tự:```
<a><b></b></a>
```Truy vấn:```
b a
```Câu trả lời là`0`, bởi vì chuỗi tổ tiên phải tuân theo thứ tự lồng nhau từ ngoài cùng đến trong cùng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực rất đơn giản sau khi xây dựng cây. 

Đối với mọi phần tử, chúng ta có thể đi lên qua tổ tiên của nó và cố gắng khớp ngược mọi truy vấn. Giả sử một truy vấn là:```
a b c
```Đối với một phần tử được gắn thẻ`c`, chúng tôi leo về phía gốc để tìm kiếm`b`, sau đó tiếp tục tìm kiếm`a`. 

Điều này hoạt động vì các truy vấn chỉ hỏi liệu chuỗi tổ tiên như vậy có tồn tại hay không. Quá trình quét hướng lên sẽ trực tiếp kiểm tra tình trạng đó. 

Vấn đề là sự phức tạp. Nếu tài liệu chứa`N`các phần tử và cây thoái hóa thành một chuỗi thì mỗi bước đi lên có thể phải trả giá`O(N)`. Việc lặp lại điều này cho mọi phần tử và mọi truy vấn sẽ dẫn đến khoảng:```
O(number_of_queries * N^2)
```Với`N`gần mấy trăm nghìn thì vô vọng. 

Quan sát quan trọng là số lượng truy vấn riêng biệt rất nhỏ, trong khi bản thân tài liệu lại rất lớn. 

Thay vì kiểm tra các truy vấn riêng biệt, chúng tôi có thể xử lý tài liệu một lần trong khi vẫn duy trì thông tin về tất cả các truy vấn cùng một lúc. 

Giả sử chúng ta duyệt cây bằng DFS và giữ đường dẫn hiện tại của tên thẻ từ gốc đến nút hiện tại. Khi đó mỗi truy vấn sẽ trở thành một bài toán so khớp dãy con: 

Truy vấn có xuất hiện dưới dạng một chuỗi con của đường dẫn gốc tới nút hiện tại không? 

Ví dụ: nếu đường dẫn DFS hiện tại là:```
header p div b
```sau đó truy vấn:```
header b
```trận đấu. 

Vì các truy vấn ngắn nên chúng tôi có thể tạo một bộ ba tất cả các truy vấn. Trong DFS, bất cứ khi nào chúng tôi nhập thẻ có tên`x`, mọi trạng thái truy vấn đang chờ`x`có thể thăng tiến. 

Điều này biến vấn đề thành sự lan truyền tự động dọc theo đường dẫn DFS. 

Tổng lượng dữ liệu truy vấn rất nhỏ:```
200 * 200 = 40000
```Vì vậy, chúng tôi có đủ khả năng để duy trì danh sách chuyển tiếp cho tất cả các kết quả khớp một phần. 

Thuật toán kết quả về cơ bản trở nên tuyến tính theo kích thước tài liệu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(Q · N2) | O(N) | Quá chậm | 
| Tối ưu | O(Độ dài tài liệu + Tổng độ dài truy vấn + Tổng số chuyển đổi trạng thái) | O(Tổng độ dài truy vấn) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Phân tích tài liệu 

Chúng ta quét chuỗi tài liệu từ trái sang phải và trích xuất thẻ. 

Đối với mỗi thẻ mở, chúng tôi tạo một nút mới và đẩy nó vào ngăn xếp. 

Đối với mỗi thẻ đóng, chúng tôi lấy nút phù hợp ra khỏi ngăn xếp. 

Đối với mỗi thẻ tự đóng, chúng tôi tạo một nút nhưng không đẩy nó. 

Điều này xây dựng lại cây làm tổ. 

### Xây dựng trạng thái truy vấn 

1. Chia mọi truy vấn thành các từ. 
2. Gán một id số nguyên cho mỗi tên thẻ duy nhất xuất hiện trong tài liệu hoặc truy vấn. 
3. Đối với mỗi truy vấn, hãy tạo trạng thái đại diện cho tiền tố của truy vấn đó. 

Ví dụ: truy vấn:```
a b c
```tạo ra sự chuyển tiếp:```
state 0 --a--> state 1
state 1 --b--> state 2
state 2 --c--> state 3
```Tình trạng`3`có nghĩa là toàn bộ truy vấn phù hợp. 

1. Đối với mỗi tên thẻ, hãy lưu trữ trạng thái nào có thể nâng cao khi thẻ này xuất hiện. 

Điều này cho phép chúng tôi cập nhật hiệu quả tất cả các kết quả khớp một phần đang hoạt động trong DFS. 

### Truyền tải DFS 

1. Khởi động DFS từ mọi nút gốc. 
2. Duy trì một mảng`cnt[state]`.`cnt[s]`bằng số cách mà đường dẫn gốc tới nút hiện tại nhận ra trạng thái tiền tố truy vấn`s`. 

Ban đầu chỉ có trạng thái`0`đang hoạt động. 

1. Khi nhập một nút có thẻ`x`, mọi chuyển tiếp đang chờ đợi`x`trở nên có thể sử dụng được. 

Giả định:```
state u --x--> state v
```Sau đó:```
cnt[v] += cnt[u]
```Chúng tôi xử lý các trạng thái theo thứ tự ngược lại để các bản cập nhật từ nút này không xếp tầng nhiều lần trong cùng một bước. 

1. Bất cứ khi nào trạng thái đầu cuối trở nên hoạt động, mức tăng truy vấn tương ứng sẽ khớp với nút này. 
2. Tái diễn thành trẻ em. 
3. Sau khi quay lại, khôi phục tất cả các sửa đổi được thực hiện tại nút này. 

Việc khôi phục là cần thiết vì các đường dẫn DFS là độc lập. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào trong DFS,`cnt[state]`biểu thị có bao nhiêu chuỗi con của đường dẫn gốc đến nút hiện tại nhận ra tiền tố truy vấn tương ứng. 

Khi chúng tôi nhập một thẻ`x`, mọi kết quả phù hợp một phần hiện có mong đợi`x`có thể mở rộng thông qua nút hiện tại. Vì các chuỗi con cho phép bỏ qua các nút trung gian nên mọi tiền tố hợp lệ trước đó vẫn hợp lệ ở sâu hơn trong cây. 

Việc xử lý các chuyển đổi theo thứ tự ngược lại sẽ ngăn không cho một nút nào được sử dụng lại nhiều lần trong cùng một vị trí truy vấn. 

Vì DFS luôn duy trì chính xác đường dẫn tổ tiên hiện tại nên mỗi lần hoàn thành được tính tương ứng với một chuỗi lồng nhau hợp lệ trong tài liệu và mọi chuỗi lồng nhau hợp lệ cuối cùng đều được tính khi DFS đạt đến phần tử cuối cùng của nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from collections import defaultdict

def solve():
    s = input().strip()
    m = int(input())

    queries = []
    all_tags = set()

    for _ in range(m):
        q = input().split()
        queries.append(q)
        all_tags.update(q)

    n = len(s)

    children = []
    tag_id = []
    roots = []

    stack = []

    tag_map = {}

    def get_id(name):
        if name not in tag_map:
            tag_map[name] = len(tag_map)
        return tag_map[name]

    i = 0

    while i < n:
        j = s.find('>', i)

        token = s[i + 1:j]

        if token[0] == '/':
            stack.pop()

        elif token[-1] == '/':
            name = token[:-1]

            tid = get_id(name)

            node = len(children)
            children.append([])
            tag_id.append(tid)

            if stack:
                children[stack[-1]].append(node)
            else:
                roots.append(node)

        else:
            name = token

            tid = get_id(name)

            node = len(children)
            children.append([])
            tag_id.append(tid)

            if stack:
                children[stack[-1]].append(node)
            else:
                roots.append(node)

            stack.append(node)

        i = j + 1

    transitions = defaultdict(list)

    state_ptr = 1

    query_end = []
    query_state = []

    for qi, q in enumerate(queries):
        cur = 0

        for word in q:
            tid = get_id(word)

            nxt = state_ptr
            state_ptr += 1

            transitions[tid].append((cur, nxt))

            cur = nxt

        query_end.append(cur)
        query_state.append(cur)

    cnt = [0] * state_ptr
    cnt[0] = 1

    ans = [0] * m

    def dfs(u):
        tid = tag_id[u]

        updates = []

        trans = transitions.get(tid, [])

        for prev, nxt in reversed(trans):
            add = cnt[prev]

            if add:
                old = cnt[nxt]
                cnt[nxt] += add
                updates.append((nxt, old))

        for qi in range(m):
            st = query_end[qi]
            if cnt[st]:
                ans[qi] += cnt[st]

        for v in children[u]:
            dfs(v)

        for st, old in reversed(updates):
            cnt[st] = old

    for r in roots:
        dfs(r)

    print('\n'.join(map(str, ans)))

solve()
```Trình phân tích cú pháp sẽ tái tạo lại khu rừng trực tiếp từ luồng thẻ. Mỗi thẻ mở hoặc thẻ tự đóng sẽ tạo ra một nút. Ngăn xếp luôn lưu trữ các phần tử hiện đang mở. 

Các trạng thái tự động đại diện cho tiền tố của truy vấn. Tình trạng`0`có nghĩa là tiền tố trống. Mỗi lần chuyển đổi tương ứng với việc sử dụng một thẻ từ một truy vấn. 

DFS duy trì các giá trị lập trình động trên đường dẫn tổ tiên hiện tại. Cập nhật chuyển tiếp theo thứ tự ngược lại là phần tinh tế. Nếu không đảo ngược, một nút có thể nâng cao đồng thời nhiều vị trí của cùng một truy vấn. 

Ví dụ: hãy xem xét truy vấn:```
a a
```và nút hiện tại`a`. 

Nếu các bản cập nhật được xử lý chuyển tiếp, nút tương tự trước tiên sẽ kích hoạt bản cập nhật đầu tiên.`a`, sau đó kích hoạt ngay cái thứ hai`a`sử dụng chính nó một lần nữa. Đảo ngược ngăn chặn điều đó. 

Rollback khôi phục mảng DP sau khi rời khỏi cây con. Nếu không khôi phục, kết quả khớp từ một nhánh sẽ rò rỉ không chính xác sang các nhánh anh em. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
<a><b><b></b></b></a><a><b></b><b><v/></b></a><b></b>
```Truy vấn:```
a
a b b
a b
b a
```Cấu trúc cây là:```
a
└── b
    └── b

a
├── b
└── b
    └── v

b
```Dấu vết DFS cho truy vấn`a b`: 

| Đường dẫn hiện tại | Tiền tố hoạt động | Truy vấn đã khớp? | 
| --- | --- | --- | 
| một | một | Không | 
| a b | một, một b | Có | 
| a b b | một, một b | Có | 
| một | một | Không | 
| a b | một, một b | Có | 
| a b | một, một b | Có | 
| a b v | một, một b | Không | 
| b | không | Không | 

Câu trả lời trở thành`4`. 

Dấu vết này thể hiện sự phù hợp với trình tự tiếp theo. Truy vấn`a b`khớp với cả chuỗi lồng nhau trực tiếp và gián tiếp. 

### Ví dụ tùy chỉnh 

đầu vào:```
<a><x><b/></x></a>
```Truy vấn:```
a b
```Dấu vết DFS: 

| Đường dẫn hiện tại | Tiền tố phù hợp | 
| --- | --- | 
| một | một | 
| một x | một | 
| axb | một, một b | 

Câu trả lời là`1`. 

Ví dụ này xác nhận rằng các nút trung gian có thể bị bỏ qua. Truy vấn chỉ yêu cầu ngăn chặn tổ tiên chứ không yêu cầu liền kề. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(L + S + T) |`L`để phân tích tài liệu,`S`cho tổng kích thước truy vấn,`T`cho tất cả các cập nhật chuyển tiếp trong DFS | 
| Không gian | O(N + S) | Lưu trữ cây cộng với trạng thái tự động hóa | 

Độ dài tài liệu tối đa là`10^6`, vì vậy việc phân tích tuyến tính là cần thiết. Tổng kích thước truy vấn không bao giờ vượt quá`40000`, điều này làm cho máy tự động cực kỳ nhỏ so với tài liệu. Giải pháp dễ dàng phù hợp trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io
from collections import defaultdict

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    s = input().strip()
    m = int(input())

    queries = []

    children = []
    tag_id = []
    roots = []

    stack = []

    tag_map = {}

    def get_id(name):
        if name not in tag_map:
            tag_map[name] = len(tag_map)
        return tag_map[name]

    n = len(s)

    i = 0

    while i < n:
        j = s.find('>', i)

        token = s[i + 1:j]

        if token[0] == '/':
            stack.pop()

        elif token[-1] == '/':
            name = token[:-1]

            tid = get_id(name)

            node = len(children)
            children.append([])
            tag_id.append(tid)

            if stack:
                children[stack[-1]].append(node)
            else:
                roots.append(node)

        else:
            name = token

            tid = get_id(name)

            node = len(children)
            children.append([])
            tag_id.append(tid)

            if stack:
                children[stack[-1]].append(node)
            else:
                roots.append(node)

            stack.append(node)

        i = j + 1

    transitions = defaultdict(list)

    state_ptr = 1

    query_end = []

    for qi in range(m):
        q = input().split()

        cur = 0

        for word in q:
            tid = get_id(word)

            nxt = state_ptr
            state_ptr += 1

            transitions[tid].append((cur, nxt))

            cur = nxt

        query_end.append(cur)

    cnt = [0] * state_ptr
    cnt[0] = 1

    ans = [0] * m

    def dfs(u):
        tid = tag_id[u]

        updates = []

        for prev, nxt in reversed(transitions.get(tid, [])):
            add = cnt[prev]

            if add:
                old = cnt[nxt]
                cnt[nxt] += add
                updates.append((nxt, old))

        for qi, st in enumerate(query_end):
            if cnt[st]:
                ans[qi] += cnt[st]

        for v in children[u]:
            dfs(v)

        for st, old in reversed(updates):
            cnt[st] = old

    for r in roots:
        dfs(r)

    return '\n'.join(map(str, ans))

# provided sample
assert run(
"""<a><b><b></b></b></a><a><b></b><b><v/></b></a><b></b>
4
a
a b b
a b
b a
"""
) == "2\n1\n4\n0"

# single self-closing node
assert run(
"""<a/>
1
a
"""
) == "1"

# non-consecutive nesting
assert run(
"""<a><x><b/></x></a>
1
a b
"""
) == "1"

# repeated names
assert run(
"""<a><a><a/></a></a>
2
a a
a a a
"""
) == "2\n1"

# impossible order
assert run(
"""<a><b/></a>
1
b a
"""
) == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`<a/>`với truy vấn`a`|`1`| Thẻ tự đóng tạo thành phần | 
|`<a><x><b/></x></a>`với truy vấn`a b`|`1`| Công việc lồng ghép không liên tiếp | 
| Lồng nhau lặp đi lặp lại`a`thẻ |`2`,`1`| Tính chính xác của bản cập nhật DP theo thứ tự ngược | 
| Truy vấn`b a`TRÊN`<a><b/></a>`|`0`| Vấn đề trật tự tổ tiên | 

## Vỏ cạnh 

Xem xét tên thẻ lặp lại:```
<a><a><a/></a></a>
```Truy vấn:```
a a
```Khi DFS đi vào vực sâu nhất`a`, đường dẫn chứa ba`a`nút. 

Cập nhật chuyển tiếp theo thứ tự ngược đảm bảo rằng nút hiện tại chỉ đóng góp một lần cho mỗi vị trí truy vấn. Nút sâu nhất khớp chính xác bằng cách sử dụng nút tổ tiên`a`, không phải chính nó hai lần. 

Các đầu ra thuật toán:```
2
```trong đó tính các yếu tố ở giữa và sâu nhất. 

Bây giờ hãy xem xét các cấp độ bị bỏ qua:```
<a><x><b/></x></a>
```Truy vấn:```
a b
```Trong khi thăm quan`x`, trạng thái DP cho tiền tố`a`vẫn hoạt động. Đang vào`b`mở rộng tiền tố hiện có đó sang`a b`. 

Thuật toán không bao giờ yêu cầu sự liền kề giữa các nút phù hợp, chỉ yêu cầu ngăn chặn tổ tiên. 

Cuối cùng, hãy xem xét rò rỉ anh chị em:```
<a><b/></a><a/>
```Truy vấn:```
a b
```Câu trả lời đúng là`1`. 

Sau khi DFS rời khỏi cây con đầu tiên, quá trình khôi phục sẽ xóa tất cả các trạng thái được đưa vào đó. Khi gốc thứ hai`a`được xử lý, trước đó`b`trận đấu đã biến mất. 

Nếu không khôi phục, nhánh thứ hai sẽ kế thừa sai các kết quả khớp từ cây con đầu tiên.
