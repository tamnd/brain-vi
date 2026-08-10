---
title: "CF 102700J - Kỳ thi Java"
description: "Mỗi học sinh chỉ vào chính xác một đối tác yêu thích. Nếu chúng ta vẽ một cạnh từ một học sinh đến bạn bạn yêu thích của họ, điều kiện phản đối xứng có nghĩa là không thể có một chu trình có hướng liên quan đến hai học sinh khác nhau."
date: "2026-08-10T05:58:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102700
codeforces_index: "J"
codeforces_contest_name: "2020 ICPC Universidad Nacional de Colombia Programming Contest"
rating: 0
weight: 102700
solve_time_s: 451
verified: true
draft: false
---

[CF 102700J - Kỳ thi Java](https://codeforces.com/problemset/problem/102700/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 7 phút 31 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi học sinh chỉ vào chính xác một đối tác yêu thích. Nếu chúng ta vẽ một cạnh từ một học sinh đến bạn bạn yêu thích của họ, điều kiện phản đối xứng có nghĩa là không thể có một chu trình có hướng liên quan đến hai học sinh khác nhau. Cho phép tự lặp, do đó cấu trúc là một tập hợp các cây có gốc, trong đó các đối tác yêu thích sau sẽ di chuyển từ một nút về phía gốc của nó. 

Một học sinh là đối tác đặc biệt của một học sinh khác chính xác khi học sinh đầu tiên nằm trong chuỗi đối tác yêu thích bắt đầu từ học sinh thứ hai. Trong ngôn ngữ cây, những đối tác đặc biệt là tổ tiên. Do đó, các điều kiện nhóm có cách giải thích cây rõ ràng. 

Giả sử chúng ta cần nhóm hợp lệ nhỏ nhất chứa sinh viên (X) và (Y). Gọi (L) là tổ tiên chung thấp nhất của chúng. Nhóm phải chứa mọi đỉnh trên đường đi từ (L) đến (X) và mọi đỉnh trên đường đi từ (L) đến (Y). Ngược lại, tập hợp các đường đi đó tự nó là một nhóm hợp lệ: mọi đỉnh không phải (L) đều có đối tác yêu thích của nó trong nhóm, trong khi (L) là tổ tiên của mọi thành viên. Do đó nhóm nhỏ nhất chính xác là đường đi của cây giữa (X) và (Y). 

Có một sự phức tạp do lớp học năng động gây ra. Học sinh có thể rời đi hoặc phụ huynh có thể đến muộn hơn. Một truy vấn chỉ có thể sử dụng những sinh viên có mặt tại thời điểm truy vấn. Nếu thiếu một đỉnh trên đường đi cần thiết thì nhóm không thể được hình thành. 

Đối với mỗi học sinh, hãy lưu trữ khoảng thời gian học sinh đó có mặt trong lớp. Sinh viên hiện tại ban đầu có thời gian bắt đầu (0). Một học sinh đến sự kiện (t) có thời gian bắt đầu (t), và một học sinh rời khỏi sự kiện (t) có thời gian kết thúc (t). Nếu học sinh không bao giờ rời đi, thời gian kết thúc có thể được coi là vô tận. Một học sinh có mặt tại thời điểm truy vấn (t) chính xác là khi nào 

[ 
bắt đầu[v] \le t < kết thúc[v]. 
] 

Do đó, mọi đỉnh trên một đường đi đều có mặt tại thời điểm (t) chính xác khi thời gian bắt đầu tối đa trên đường đi đó lớn nhất là (t) và thời gian kết thúc tối thiểu lớn hơn (t). 

Lớp có một đại diện hữu ích tương tự. Để một chủ đề được đảm bảo đúng dù giáo viên chọn thành viên nhóm nào thì mọi học sinh trong nhóm đều phải biết chủ đề đó. Do đó, điểm tối thiểu là số chủ đề chung cho mọi đỉnh trên đường đi. Nếu các chủ đề mà học sinh biết được biểu thị bằng mặt nạ bit thì câu trả lời chỉ đơn giản là AND theo bit của tất cả các mặt nạ trên đường dẫn, theo sau là số lượng phổ biến. 

Đầu vào có thể chứa (10^5) sinh viên hiện diện ban đầu và (10^5) sự kiện, trong khi mã định danh sinh viên là số nguyên tùy ý lên đến (10^9). Tổng số mã định danh riêng biệt xuất hiện ở bất kỳ đâu có thể là tuyến tính theo kích thước đầu vào, do đó cần phải nén tọa độ. Với giới hạn hai giây, việc quét toàn bộ cây cho mỗi truy vấn là không khả thi. Một chuỗi trong trường hợp xấu nhất gồm (10^5) đỉnh và (10^5) truy vấn sẽ yêu cầu khoảng (10^{10}) lượt truy cập đỉnh. Chúng tôi cần công việc logarit hoặc gần logarit cho mỗi truy vấn. 

Có một số trường hợp khó khăn có thể dễ dàng phá vỡ việc triển khai hợp lý. 

### Hai học sinh được hỏi giống nhau 

Một nhóm đơn lẻ có giá trị vì một học sinh về cơ bản là một đối tác đặc biệt của chính họ. Ví dụ,```
1 1
1 1
1 1
1
1 1 1
```có đầu ra```
1
```Giải pháp nhất quyết tìm hai đỉnh khác nhau hoặc giả sử đường đi có ít nhất một cạnh sẽ thất bại ở đây. 

### Hai học sinh ở hai cây khác nhau 

Hãy xem xét```
2 1
1 1
1 1
2 2
1 1
1
1 1 2
```Hai học sinh có gốc khác nhau nên không có tổ tiên chung và không có nhóm hợp lệ chứa cả hai. Đầu ra là```
-1
```Việc triển khai LCA bất cẩn giả định toàn bộ biểu đồ là một cây có thể trả về một gốc tùy ý và tạo ra một đường dẫn vô nghĩa. 

### Một học sinh trung cấp cần thiết đã rời đi 

Hãy xem xét```
3 2
1 2
2 1 2
2 3
2 1 2
3 3
2 1 2
3
1 1 3
0 2
1 1 3
```Truy vấn đầu tiên sử dụng đường dẫn (1,2,3), vì vậy điểm của nó là (2). Sinh viên (2) sau đó rời đi. Truy vấn thứ hai vẫn cần đường dẫn cây tương tự nhưng học sinh (2) vắng mặt nên đáp án là```
2
-1
```Chỉ kiểm tra xem (X) và (Y) có mặt hay không là chưa đủ. 

### Cha mẹ có thể đến sau con của nó 

Hãy xem xét```
1 1
1 2
1 1
3
1 1 2
2 2 2
1 1 2
```Ban đầu học sinh (1) chỉ vào học sinh (2), nhưng học sinh (2) không có mặt trong lớp. Truy vấn đầu tiên là không thể. Sau khi học sinh (2) đến, đường dẫn sẽ có thể sử dụng được và truy vấn thứ hai trả về (1). 

Đây là lý do tại sao cây phải được xây dựng từ toàn bộ lịch sử sự kiện thay vì chỉ từ những học sinh có mặt ban đầu. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp là coi mối quan hệ bạn tình yêu thích như một khu rừng và trả lời từng truy vấn bằng cách đi từ (X) và (Y) về phía tổ tiên chung của chúng. Khi đã biết đường dẫn, chúng ta có thể kiểm tra xem tất cả các đỉnh có hiện diện hay không và VÀ mặt nạ chủ đề của chúng. 

Cách tiếp cận bạo lực này là đúng vì nhóm nhỏ nhất chính xác là con đường giữa hai học sinh. Vấn đề của nó là độ dài đường dẫn. Một chuỗi gồm (10^5) học sinh có thể thực hiện một truy vấn duy nhất thực hiện (O(10^5)). Với (10^5) truy vấn, truy vấn đó sẽ trở thành (10^{10}) hoạt động trong trường hợp xấu nhất, vượt xa giới hạn thời gian. 

Một quan sát hữu ích là học sinh không thực sự thay đổi vị trí của mình trong rừng trong suốt giờ học. Chỉ có sự sẵn có của họ thay đổi. Vì mỗi học sinh đến nhiều nhất một lần và rời đi nhiều nhất một lần, nên trước tiên chúng ta có thể đọc toàn bộ chuỗi sự kiện và chỉ định cho mỗi học sinh một khoảng thời gian hoạt động cố định. Khu rừng bên dưới sau đó hoàn toàn tĩnh. 

Điều này biến phần động thành ba giá trị tĩnh được gắn vào mỗi đỉnh: mặt nạ chủ đề, thời gian đến và thời gian khởi hành. Một đường dẫn tổng hợp chỉ cần ba thao tác kết hợp: 

[ 
mặt nạ = mặt nạ_1 \mathbin{&} mặt nạ_2, 
] 

[ 
mới nhấtStart = \max(start_1,start_2), 
] 

[ 
sớm nhấtEnd = \min(end_1,end_2). 
] 

Chúng ta có thể trả lời các tập hợp đường dẫn này bằng cách sử dụng phân tách ánh sáng nặng. Một đường đi trong cây sẽ trở thành các khoảng (O(\log N)) liền kề nhau theo thứ tự đậm-nhẹ và mỗi khoảng có thể được truy vấn trong (O(\log N)) bằng một cây phân đoạn lặp. Độ phức tạp của truy vấn thu được là (O(\log^2 N)). 

Sự đơn giản hóa quan trọng là không có cập nhật cây phân đoạn. Tất cả các chuyến khởi hành và đến đã được chuyển đổi thành các khoảng thời gian cố định trước khi xây dựng cấu trúc dữ liệu cây. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O((N+Q)N)) trường hợp xấu nhất | (O(N)) | Quá chậm | 
| Tối ưu | (O((N+Q)\log N + Q\log^2 N)) | (O(N+Q)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc từng học sinh đầu tiên và mọi sự kiện. Nén mọi mã định danh sinh viên thành một chỉ mục số nguyên. Khi một học sinh lần đầu tiên chỉ xuất hiện với tư cách là đối tác yêu thích hoặc điểm cuối truy vấn, hãy tạo một đỉnh giữ chỗ cho họ. 

Phần giữ chỗ đại diện cho một học sinh có thể không bao giờ thực sự đến lớp. Khoảng thời gian mặc định của nó trống, vì vậy bất kỳ đường dẫn nào yêu cầu nó sẽ tự động trở thành không hợp lệ. 
2. Lưu trữ đối tác yêu thích của mọi đỉnh. Một đỉnh mà đối tác ưa thích của nó là một gốc. Nếu một đối tác yêu thích xuất hiện trong đầu vào nhưng không bao giờ trở thành sinh viên thì đối tác đó vẫn là một root không hoạt động. 

Điều kiện phản đối xứng đảm bảo rằng đồ thị có hướng thu được không có chu trình không tầm thường, do đó các thành phần này là cây có gốc. 
3. Chuyển các sự kiện trong lớp thành khoảng thời gian sẵn sàng cố định. Một sinh viên có mặt ban đầu nhận được`start = 0`. Một sinh viên đến sự kiện (t) nhận được`start = t`. Một khởi hành tại sự kiện (t) tập`end = t`. Những sinh viên không bao giờ rời đi sẽ nhận được thời gian kết thúc vô hạn, trong khi những sinh viên không bao giờ đến nơi sẽ có thời gian bắt đầu vô hạn và thời gian kết thúc bằng 0. 

Đối với một truy vấn tại thời điểm (t), một đường dẫn hoàn toàn hiện diện chính xác khi thời gian bắt đầu tối đa của nó lớn nhất là (t) và thời gian kết thúc tối thiểu của nó lớn hơn (t). 
4. Xây dựng biểu diễn rừng và tính toán độ sâu, kích thước cây con và trọng lượng con của từng đỉnh. 

Đứa trẻ nặng nhất là đứa trẻ có cây con lớn nhất. Theo sau các phần tử nặng tạo ra các chuỗi sao cho mọi đường dẫn từ gốc tới nút chỉ đi qua chuỗi (O(\log N)). 
5. Thực hiện phân tách ánh sáng nặng và gán cho mỗi đỉnh một vị trí trong một mảng tuyến tính. Tại vị trí của nó, lưu trữ mặt nạ chủ đề, thời gian bắt đầu và thời gian kết thúc. 
6. Xây dựng cây phân đoạn lặp trên mảng này. Mỗi phân đoạn lưu trữ ba tập hợp: AND của tất cả các mặt nạ chủ đề, thời gian bắt đầu tối đa và thời gian kết thúc tối thiểu. 

Phần tử nhận dạng là mặt nạ tất cả một cho AND, vô cực âm cho mức tối đa và vô cực dương cho mức tối thiểu. 
7. Đối với truy vấn liên quan đến (X) và (Y), trước tiên hãy kiểm tra xem chúng có thuộc cùng một cây hay không. Nếu không, hãy trả lời`-1`. 
8. Phân tách đường dẫn giữa (X) và (Y) thành các khoảng sáng-nặng. Luôn xử lý điểm cuối có đầu chuỗi sâu hơn, truy vấn đoạn chuỗi đó và di chuyển điểm cuối đến phần đầu chuỗi của nó. 

Mỗi khoảng thời gian được xử lý đóng góp mặt nạ, thời gian bắt đầu tối đa và thời gian kết thúc tối thiểu cho tổng hợp đường dẫn. 
9. Khi cả hai đỉnh đều nằm trên cùng một chuỗi nặng, hãy truy vấn khoảng thời gian cuối cùng giữa các vị trí của chúng. Khoảng này chứa tổ tiên chung thấp nhất đúng một lần. 
10. Đặt tổng hợp đường dẫn kết quả là`(commonMask, latestStart, earliestEnd)`. Nếu như`latestStart > queryTime`hoặc`earliestEnd <= queryTime`, ít nhất một học sinh được yêu cầu vắng mặt, do đó kết quả`-1`. 
11. Ngược lại, tồn tại nhóm hợp lệ nhỏ nhất và`commonMask`chứa chính xác các chủ đề được mọi thành viên trong nhóm đó biết. Câu trả lời là`commonMask.bit_count()`. 

### Tại sao nó hoạt động 

Mối quan hệ đối tác yêu thích làm cho mọi thành phần trở thành một cây có gốc, với việc di chuyển đến đối tác yêu thích tương ứng với việc di chuyển đến thành phần cha mẹ. Đối với hai đỉnh trong cùng một thành phần, bất kỳ nhóm hợp lệ nào chứa cả hai đều phải tiếp tục theo dõi các đối tác yêu thích cho đến khi đạt được tổ tiên chung. Tổ tiên thấp nhất như vậy là LCA của chúng, vì vậy nhóm nhỏ nhất có thể chính xác là con đường giữa chúng. Mỗi đỉnh không phải LCA đều có đỉnh gốc của nó trên đường đi đó và LCA là đỉnh tổ tiên của mọi đỉnh đường dẫn, do đó đường dẫn này thỏa mãn các quy tắc nhóm. 

Tổng hợp khoảng thời gian là chính xác vì một đỉnh xuất hiện tại thời điểm truy vấn (t) chính xác khi thời gian bắt đầu của nó lớn nhất là (t) và thời gian kết thúc của nó lớn hơn (t). Áp dụng mức tối đa cho tất cả thời gian bắt đầu và mức tối thiểu cho tất cả thời gian kết thúc sẽ mang lại chính xác hai điều kiện cần thiết để mọi đỉnh đường đi đều có mặt. 

Cuối cùng, một chủ đề góp phần đảm bảo điểm tối thiểu một cách chính xác khi mọi học sinh trong nhóm đều biết về chủ đề đó. Bitwise AND tính toán chính xác giao điểm của các bộ chủ đề này. Cây phân đoạn kết hợp các tập hợp liên kết này trên mỗi phân đoạn nặng-nhẹ, do đó tập hợp cuối cùng thể hiện chính xác đường dẫn được yêu cầu. 

## Giải pháp Python```python
import sys
from array import array

input = sys.stdin.readline

INF = 10**9

def solve():
    n, b = map(int, input().split())
    ALL = (1 << b) - 1

    ids = {}

    parent = []
    topic = []
    start = []
    end = []

    def get_id(x):
        v = ids.get(x)
        if v is not None:
            return v

        v = len(parent)
        ids[x] = v
        parent.append(v)
        topic.append(0)
        start.append(INF)
        end.append(0)
        return v

    def read_mask():
        a = list(map(int, input().split()))
        m = 0
        for t in a[1:]:
            m |= 1 << (t - 1)
        return m

    for _ in range(n):
        x_value, f_value = map(int, input().split())

        x = get_id(x_value)
        f = get_id(f_value)

        parent[x] = f
        topic[x] = read_mask()
        start[x] = 0
        end[x] = INF

    q = int(input())
    queries = []

    for t in range(1, q + 1):
        event = list(map(int, input().split()))
        typ = event[0]

        if typ == 0:
            x = get_id(event[1])
            end[x] = t

        elif typ == 1:
            x = get_id(event[1])
            y = get_id(event[2])
            queries.append((t, x, y))

        else:
            x = get_id(event[1])
            f = get_id(event[2])

            parent[x] = f
            topic[x] = read_mask()
            start[x] = t
            end[x] = INF

    N = len(parent)

    children = [[] for _ in range(N)]
    roots = []

    for v in range(N):
        p = parent[v]
        if p == v:
            roots.append(v)
        else:
            children[p].append(v)

    depth = [0] * N
    component = [0] * N
    order = []

    for root in roots:
        stack = [root]
        component[root] = root

        while stack:
            u = stack.pop()
            order.append(u)

            du = depth[u] + 1
            for v in children[u]:
                depth[v] = du
                component[v] = root
                stack.append(v)

    size = [1] * N
    heavy = [-1] * N

    for u in reversed(order):
        best_size = 0
        total = 1

        for v in children[u]:
            sv = size[v]
            total += sv
            if sv > best_size:
                best_size = sv
                heavy[u] = v

        size[u] = total

    head = [0] * N
    pos = [0] * N

    base_topic = [0] * N
    base_start = [0] * N
    base_end = [0] * N

    cur = 0
    stack = []

    for root in roots:
        stack.append((root, root))

        while stack:
            u, h = stack.pop()

            while u != -1:
                head[u] = h
                pos[u] = cur

                base_topic[cur] = topic[u]
                base_start[cur] = start[u]
                base_end[cur] = end[u]

                cur += 1

                hv = heavy[u]

                for v in children[u]:
                    if v != hv:
                        stack.append((v, v))

                u = hv

    size_tree = 1
    while size_tree < N:
        size_tree <<= 1

    seg_topic = array('i', [ALL]) * (2 * size_tree)
    seg_start = array('i', [-1]) * (2 * size_tree)
    seg_end = array('i', [INF]) * (2 * size_tree)

    for i in range(N):
        p = size_tree + i
        seg_topic[p] = base_topic[i]
        seg_start[p] = base_start[i]
        seg_end[p] = base_end[i]

    for p in range(size_tree - 1, 0, -1):
        left = p << 1
        right = left | 1

        seg_topic[p] = seg_topic[left] & seg_topic[right]
        seg_start[p] = max(seg_start[left], seg_start[right])
        seg_end[p] = min(seg_end[left], seg_end[right])

    def range_query(l, r):
        l += size_tree
        r += size_tree

        ans_topic = ALL
        ans_start = -1
        ans_end = INF

        while l < r:
            if l & 1:
                ans_topic &= seg_topic[l]
                s = seg_start[l]
                e = seg_end[l]

                if s > ans_start:
                    ans_start = s
                if e < ans_end:
                    ans_end = e

                l += 1

            if r & 1:
                r -= 1

                ans_topic &= seg_topic[r]
                s = seg_start[r]
                e = seg_end[r]

                if s > ans_start:
                    ans_start = s
                if e < ans_end:
                    ans_end = e

            l >>= 1
            r >>= 1

        return ans_topic, ans_start, ans_end

    def path_query(x, y):
        if component[x] != component[y]:
            return None

        ans_topic = ALL
        ans_start = -1
        ans_end = INF

        while head[x] != head[y]:
            if depth[head[x]] < depth[head[y]]:
                x, y = y, x

            h = head[x]

            a, s, e = range_query(pos[h], pos[x] + 1)

            ans_topic &= a
            if s > ans_start:
                ans_start = s
            if e < ans_end:
                ans_end = e

            x = parent[h]

        l = pos[x]
        r = pos[y]

        if l > r:
            l, r = r, l

        a, s, e = range_query(l, r + 1)

        ans_topic &= a
        if s > ans_start:
            ans_start = s
        if e < ans_end:
            ans_end = e

        return ans_topic, ans_start, ans_end

    out = []

    for t, x, y in queries:
        result = path_query(x, y)

        if result is None:
            out.append("-1")
            continue

        common_topic, latest_start, earliest_end = result

        if latest_start > t or earliest_end <= t:
            out.append("-1")
        else:
            out.append(str(common_topic.bit_count()))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Giai đoạn đầu vào trước tiên tạo chỉ mục số nguyên nén cho mọi mã định danh. Điều này là cần thiết vì bản thân số nhận dạng có thể lớn bằng (10^9), trong khi tất cả cấu trúc dữ liệu chỉ cần chỉ số từ (0) đến (N-1). 

Giai đoạn sự kiện không thực hiện truy vấn ngay lập tức. Thay vào đó, nó ghi lại từng truy vấn và hoàn tất việc tính toán khoảng thời gian hiện diện đầy đủ của mỗi học sinh. Đây là sự chuyển đổi ngoại tuyến quan trọng. Chuyến khởi hành chỉ thay đổi điểm cuối của một khoảng thời gian, trong khi chuyến đến cố định điểm bắt đầu. 

Rừng sau đó được xây dựng một lần. Việc truyền tải lặp đi lặp lại tránh đệ quy Python, điều này sẽ không an toàn trên chuỗi (10^5) trở lên. các`heavy`mảng xác định cây con lớn nhất và phân tách sẽ gán các vị trí liền kề cho các chuỗi nặng. 

Cây phân đoạn lưu trữ chính xác ba tập hợp đường dẫn cần thiết cho bằng chứng.`seg_topic`sử dụng bitwise AND,`seg_start`sử dụng tối đa và`seg_end`sử dụng tối thiểu. Cả ba phép toán đều có tính kết hợp, do đó các khoảng có thể được kết hợp theo bất kỳ thứ tự nào. 

Truy vấn đường dẫn luôn di chuyển đầu chuỗi nặng sâu hơn lên trên. Khi cả hai điểm cuối có cùng đầu chuỗi, đường đi còn lại của chúng là một đoạn liền kề. Tổ tiên chung thấp nhất được bao gồm chính xác một lần trong phạm vi truy vấn cuối cùng này. 

Việc so sánh khoảng thời gian sử dụng`latest_start > t`Và`earliest_end <= t`. Bất đẳng thức nghiêm ngặt ở cuối là cần thiết vì một học sinh rời khỏi sự kiện (t) sẽ không có mặt để trả lời truy vấn tại thời điểm (t). Ngược lại, một học sinh đến sự kiện (t) sẽ có mặt sau sự kiện đó, do đó thời gian bắt đầu của nó được chấp nhận khi truy vấn sau đó được xử lý. 

Không có vấn đề tràn số nguyên nào tồn tại trong Python. Trong cây phân đoạn,`array('i')`bộ nhớ an toàn vì mặt nạ chủ đề ở dưới (2^{20}) và thời gian sự kiện tối đa là (10^5). 

## Ví dụ đã hoạt động 

### Mẫu 1 

Có một học sinh, học sinh đó chỉ vào mình. Truy vấn duy nhất yêu cầu học sinh cùng với chính họ. 

| Thời gian sự kiện | Truy vấn | Đường dẫn | Bắt đầu mới nhất | Kết thúc sớm nhất | Mặt nạ thông thường | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | (1,1) | (1) | 0 | INF | 1 | 1 | 

Con đường này có một học sinh biết chủ đề duy nhất. Nhóm đơn là hợp lệ và AND của một mặt nạ của nó là`1`, cho điểm`1`. 

### Mẫu 2 

Rừng ban đầu là chuỗi 

[ 
1 \rightarrow 2 \rightarrow 3 \rightarrow 4 \rightarrow 5 \rightarrow 6 \rightarrow 7. 
] 

Học sinh (7) là một gốc. Tất cả học sinh ban đầu đều có mặt tại thời điểm 0. 

| Thời gian sự kiện | Sự kiện | Đường dẫn | Bắt đầu mới nhất | Kết thúc sớm nhất | Mặt nạ thông thường | Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | truy vấn (3,5) | (3,4,5) | 0 | INF |`1111`| 4 | 
| 2 | truy vấn (5,7) | (5,6,7) | 0 | INF |`1101`| 3 | 
| 3 | sinh viên 4 lá | | | | | | 
| 4 | truy vấn (3,5) | (3,4,5) | 0 | 3 |`1111`| -1 | 
| 5 | học sinh 8 đến, phụ huynh 4 | | | | | | 
| 6 | truy vấn (8,4) | (8,4) | 5 | 3 |`1111`| -1 | 
| 7 | truy vấn (8,8) | (8) | 5 | INF |`1111`| 4 | 
| 8 | truy vấn (1,1) | (1) | 0 | INF |`1000`| 1 | 

Đối với truy vấn đầu tiên, học sinh 3, 4 và 5 đều biết cả 4 chủ đề nên AND vẫn giữ nguyên`1111`. Đối với truy vấn thứ hai, học sinh 6 chỉ biết chủ đề 1, 2 và 4, do đó đường dẫn AND có ba bit được đặt. 

Sau khi học sinh 4 rời đi, đường đi giữa 3 và 5 vẫn tồn tại trong cây tĩnh, nhưng thời gian kết thúc sớm nhất của nó là 3. Tại thời điểm truy vấn 4,`earliest_end <= query_time`, vì vậy nhóm hiện không thể được thành lập. Việc học sinh 8 đến muộn không kích hoạt được học sinh 4, vì vậy truy vấn liên quan đến 8 và 4 vẫn không thể thực hiện được. Truy vấn đơn cho 8 là hợp lệ và cung cấp tất cả bốn chủ đề. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O((N+Q)\log N + Q\log^2 N)) | Tiền xử lý cây và xây dựng cây phân đoạn là tiền xử lý logarit gần tuyến tính và mọi đường đi đều đi qua chuỗi nặng (O(\log N)) với (O(\log N)) hoạt động trên mỗi phân đoạn | 
| Không gian | (O(N+Q)) | Sinh viên nén, truy vấn sự kiện, mảng HLD và cây phân đoạn đều tuyến tính ở kích thước đầu vào | 

Ở đây (N) là số lượng mã định danh học sinh riêng biệt xuất hiện ở bất kỳ đâu trong đầu vào. Nó là tuyến tính trong các sinh viên và sự kiện ban đầu. Phần quan trọng là đường dẫn có khả năng dài (10^5) không bao giờ được quét theo từng đỉnh. Sự phân rã mức độ nhẹ làm giảm nó thành nhiều phạm vi cây phân đoạn theo logarit, phù hợp với các ràng buộc đã định. 

## Trường hợp thử nghiệm 

Dây nịt sau đây giả định`solve()`chức năng từ giải pháp trên có sẵn trong cùng một mô-đun.```python
import sys
import io
from contextlib import redirect_stdout

# Use the solve() function from the solution above.
# For example:
# from solution import solve

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    output = io.StringIO()

    try:
        with redirect_stdout(output):
            solve()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

    return output.getvalue().strip()

sample1 = """\
1 1
1 2
1 1
1
1 1 1
"""

assert run(sample1) == "1", "sample 1"

sample2 = """\
7 4
1 2
1 4
2 3
1 1
3 4
4 1 2 3 4
4 5
4 3 1 2 4
5 6
4 1 4 2 3
6 7
3 1 2 4
7 7
4 4 1 2 3
8
1 3 5
1 5 7
0 4
1 3 5
2 8 4
4 3 4 1 2
1 8 4
1 8 8
1 1 1
"""

assert run(sample2) == "4\n3\n-1\n-1\n4\n1", "sample 2"

minimum_case = """\
1 1
1 1
1 1
1
1 1 1
"""

assert run(minimum_case) == "1", "minimum-size singleton"

disconnected_case = """\
2 20
1 1
1 20
2 2
1 20
1
1 1 2
"""

assert run(disconnected_case) == "-1", "different components and topic 20"

departure_case = """\
3 2
1 2
2 1 2
2 3
2 1 2
3 3
2 1 2
3
1 1 3
0 2
1 1 3
"""

assert run(departure_case) == "2\n-1", "inactive intermediate vertex"

arrival_case = """\
1 1
1 2
1 1
3
1 1 2
2 2 2
1 1
1 1 2
"""

assert run(arrival_case) == "-1\n1", "parent arrives later"

# Maximum-size stress case.
# 100000 initial students form one chain and there are 100000 queries.
n = 100000
q = 100000

parts = [f"{n} 1\n"]

for i in range(1, n):
    parts.append(f"{i} {i + 1}\n1 1\n")

parts.append(f"{n} {n}\n1 1\n")
parts.append(f"{q}\n")

for _ in range(q):
    parts.append(f"1 1 {n}\n")

maximum_case = "".join(parts)
expected = "\n".join(["1"] * q)

assert run(maximum_case) == expected, "maximum-size chain and query count"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Một học sinh chỉ vào mình |`1`| Đầu vào tối thiểu và đường dẫn đơn lẻ | 
| Hai gốc riêng biệt với chủ đề 20 |`-1`| Các thành phần khác nhau và chỉ số chủ đề cao nhất | 
| Chuỗi ba nút với học sinh cấp hai rời đi |`2`, sau đó`-1`| Đường đi sẵn có và ranh giới khởi hành | 
| Trẻ có mặt trước khi cha mẹ đến |`-1`, sau đó`1`| Khoảng thời gian đến và phụ huynh xuất hiện sau | 
| Chuỗi 100000 nút với 100000 truy vấn |`1`lặp đi lặp lại 100000 lần | Xử lý truy vấn và tiền xử lý ở quy mô tối đa | 

## Vỏ cạnh 

### Truy vấn đơn lẻ 

cho```
1 1
1 1
1 1
1
1 1 1
```học sinh là gốc rễ và hiện diện ngay từ đầu. Phân tách mức độ nhẹ chỉ định cho học sinh một vị trí và phạm vi cây phân đoạn cho truy vấn chứa chính xác vị trí đó. Tổng hợp là`mask = 1`,`latestStart = 0`, Và`earliestEnd = INF`. Tại thời điểm truy vấn 1, học sinh đang hoạt động nên câu trả lời là`1`. 

### Các thành phần khác nhau 

cho```
2 1
1 1
1 1
2 2
1 1
1
1 1 2
```học sinh 1 và 2 là gốc của các thành phần riêng biệt. Mã định danh thành phần của chúng khác nhau, vì vậy truy vấn đường dẫn ngay lập tức trả về`None`. Không có truy vấn cây phân đoạn nào được thử và câu trả lời là`-1`. 

###Học sinh trung cấp nghỉ học 

cho```
3 2
1 2
2 1 2
2 3
2 1 2
3 3
2 1 2
3
1 1 3
0 2
1 1 3
```tại thời điểm 1 đường dẫn là (1,2,3) và cả ba học sinh đều biết cả hai chủ đề. Tổng hợp là`11`, có hai bit được đặt. Tại thời điểm 2, học sinh 2 nhận được`end = 2`. Truy vấn thứ hai xảy ra ở thời điểm thứ 3, do đó tổng hợp đường dẫn có`earliestEnd = 2`. Từ`2 <= 3`, đường dẫn chứa một học sinh không còn hiện diện nữa và câu trả lời sẽ trở thành`-1`. 

### Phụ huynh đến sau con 

cho```
1 1
1 2
1 1
3
1 1 2
2 2 2
1 1
1 1 2
```Học sinh 2 tồn tại trong khu rừng nén ngay từ đầu vì nó xuất hiện với tư cách là đối tác yêu thích của học sinh 1, nhưng khoảng thời gian mặc định của nó trống. Vì vậy, truy vấn đầu tiên nhìn thấy`earliestEnd = 0`và thất bại. Tại sự kiện 2, học sinh 2 đến và nhận`start = 2`. Truy vấn cuối cùng xảy ra ở thời điểm thứ 3, vì vậy cả hai học sinh đều hoạt động và đường dẫn hợp lệ. 

### Học sinh ra về đúng thời điểm hỏi đáp 

Việc triển khai coi việc khởi hành tại thời điểm (t) là`end = t`. Điều kiện hoạt động là`t < end`, không`t <= end`. Do đó, nếu một học sinh rời đi ở sự kiện thứ 5 thì truy vấn ở thời điểm thứ 5 không thể sử dụng được học sinh đó. Ranh giới này được xử lý bởi`earliestEnd <= queryTime`. 

### Học sinh đến trước câu hỏi sau 

Một người đến vào thời điểm (t) nhận được`start = t`và truy vấn sau đó tại thời điểm (t+1) thỏa mãn`start <= queryTime`. Việc so sánh chặt chẽ chỉ cần thiết cho thời gian khởi hành, vì khoảng thời gian được biểu thị dưới dạng`[start, end)`. 

### Mã định danh được truy vấn không bao giờ xuất hiện 

Mã định danh như vậy vẫn được chèn vào biểu đồ nén. Khoảng thời gian mặc định của nó trống, với thời gian bắt đầu lớn hơn mọi truy vấn có thể và thời gian kết thúc bằng 0. Do đó, bất kỳ truy vấn nào có chứa học sinh đó đều không thể kiểm tra tính khả dụng thay vì gây ra lỗi tra cứu từ điển hoặc chỉ mục cây không hợp lệ. 

### Người bạn yêu thích không bao giờ xuất hiện khi còn là sinh viên 

Đối tác yêu thích vẫn được biểu diễn dưới dạng một đỉnh, nhưng nó vẫn không hoạt động. Nếu một đường dẫn cần đỉnh đó thì khoảng của nó sẽ làm cho truy vấn không hợp lệ. Điều này là cần thiết vì cha mẹ vắng mặt không thể âm thầm đóng vai trò là đối tác đặc biệt chung của một nhóm.
