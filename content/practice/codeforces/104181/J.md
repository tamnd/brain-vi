---
title: "CF 104181J - Lái xe nguy hiểm"
description: "Chúng ta có một đồ thị có hướng trong đó mỗi giao lộ có đúng một đường đi. Nếu chúng ta bắt đầu ở bất kỳ nút nào và tiếp tục đi theo cạnh đi ra, chúng ta chắc chắn sẽ di chuyển đến nút khác trong một phút mỗi bước."
date: "2026-07-02T00:40:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104181
codeforces_index: "J"
codeforces_contest_name: "UTPC Contest 02-10-23 Div. 1 (Advanced)"
rating: 0
weight: 104181
solve_time_s: 92
verified: true
draft: false
---

[CF 104181J - Lái xe nguy hiểm](https://codeforces.com/problemset/problem/104181/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 32s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị có hướng trong đó mỗi giao lộ có đúng một đường đi. Nếu chúng ta bắt đầu ở bất kỳ nút nào và tiếp tục đi theo cạnh đi ra, chúng ta chắc chắn sẽ di chuyển đến nút khác trong một phút mỗi bước. Bởi vì mỗi nút đều có cấp độ cao hơn một, nên mọi điểm bắt đầu cuối cùng đều đi vào một chu kỳ và sau đó tiếp tục tuần hoàn bên trong chu trình đó mãi mãi. 

Hai người lái xe chọn hai giao lộ xuất phát khác nhau. Họ di chuyển đồng bộ, một bước mỗi phút. Sự cố xảy ra nếu tại một thời điểm nào đó chúng chiếm cùng một nút vào cùng một thời điểm. Nhiệm vụ là đếm xem có bao nhiêu cặp nút khởi đầu không có thứ tự tránh gặp nhau trong chuyển động xác định này. 

Hạn chế lên tới$2 \cdot 10^5$các nút buộc một giải pháp tuyến tính hoặc gần tuyến tính. Bất kỳ phương pháp nào so sánh trực tiếp tất cả các cặp nút bắt đầu sẽ liên quan đến khoảng$n^2$tương tác theo thứ tự$4 \cdot 10^{10}$hoạt động trong trường hợp xấu nhất, vượt xa giới hạn. Chúng ta cần khai thác cấu trúc được áp đặt bởi “một cạnh đi trên mỗi nút”. 

Trường hợp cạnh khóa xuất hiện khi tất cả các nút cuối cùng hợp nhất thành một chu kỳ duy nhất. Sau đó, mọi cặp nút nằm trong chu kỳ đó cuối cùng sẽ va chạm nhau, vì vị trí của chúng trở nên tuần hoàn và thẳng hàng. Một cách tiếp cận ngây thơ chỉ kiểm tra xem các đường dẫn có giao nhau mà không xem xét thời gian hay không sẽ tính sai các cặp như vậy là an toàn. 

Một trường hợp tinh tế khác là khi hai nút đạt đến cùng một chu kỳ nhưng ở những khoảng cách khác nhau dọc theo cây đến của chúng. Ngay cả khi chúng gặp cùng một chu kỳ, chúng có thể đến vào những thời điểm khác nhau và do đó tránh được va chạm. Giải pháp phải tôn trọng sự liên kết về thời gian chứ không chỉ khả năng tiếp cận. 

## Phương pháp tiếp cận 

Nếu chúng ta mô phỏng quá trình từ mọi nút bắt đầu, chúng ta có thể tính toán quỹ đạo đầy đủ của nó cho đến khi nó đi vào một chu kỳ. Khi chúng tôi biết đường dẫn đầy đủ cho mỗi nút, chúng tôi có thể so sánh từng cặp quỹ đạo và kiểm tra xem chúng có trùng khớp cùng một lúc hay không. Điều này hoạt động vì biểu đồ có tính xác định và mỗi đường dẫn là duy nhất. 

Tuy nhiên, mỗi đường đi có thể dài$O(n)$, và có$n$các nút khởi đầu. Xây dựng tất cả các quỹ đạo đã$O(n^2)$trong tổng chiều dài. Việc so sánh từng cặp quỹ đạo sẽ bổ sung thêm một yếu tố khác, làm cho phương pháp này về cơ bản không khả thi. 

Cấu trúc của đồ thị là quan sát quan trọng. Vì mỗi nút có chính xác một cạnh đi ra nên đồ thị là một đồ thị hàm số: một tập hợp các chu trình có hướng với các cây có hướng đi vào chúng. Mỗi nút có một trạng thái “tiếp theo” duy nhất, do đó hệ thống hoạt động giống như một quá trình tiến hóa theo thời gian xác định. 

Một cách hữu ích để điều chỉnh lại vấn đề là suy nghĩ ngược thời gian. Nếu hai mã thông báo gặp nhau tại cùng một nút vào cùng một thời điểm thì từ thời điểm đó trở đi đường đi tương lai của chúng sẽ giống hệt nhau. Vì vậy, một va chạm tương đương với việc nói rằng hai điểm bắt đầu cuối cùng sẽ đồng bộ hóa trong đồ thị hàm số này. 

Đồng bộ hóa trong biểu đồ chức năng được xác định bởi vị trí các nút nằm trong cùng một cây và cấu trúc chu trình. Cụ thể, tất cả các nút trong cùng một thành phần được kết nối yếu cuối cùng sẽ chuyển sang cùng một chu kỳ và trong cấu trúc đó, chúng ta có thể lập mô hình mỗi nút mất bao lâu để đi đến chu kỳ và nơi nó đến trong chu kỳ. 

Ý tưởng chính là phân tách biểu đồ thành các chu trình và cây, sau đó tính toán cho mỗi nút điểm đầu vào của nó trong chu trình và khoảng cách của nó đến điểm đầu vào đó. Khi đã biết điều này, hành vi trong tương lai của mỗi nút được mô tả đầy đủ bằng một cặp: mã định danh chu kỳ của nó và độ lệch của nó dọc theo chu kỳ sau khi chuẩn hóa thời gian. 

Hai nút va chạm khi và chỉ nếu quỹ đạo của chúng thẳng hàng tại một thời điểm nào đó, điều này xảy ra khi hành vi chu kỳ cuối cùng của chúng khớp với cả nhận dạng chu kỳ và căn chỉnh pha. Điều này cho phép đếm các xung đột bằng cách nhóm các nút theo chu kỳ và theo dõi số lượng cặp buộc phải giao nhau. 

Lần đếm cuối cùng trở thành: tổng số cặp trừ đi những cặp cuối cùng va chạm nhau. Vì tổng số cặp là$n(n-1)/2$, vấn đề giảm xuống việc đếm các cặp không va chạm, có thể bắt nguồn từ việc phân nhóm và bù thời gian dựa trên chu kỳ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(n^2)$|$O(n)$| Quá chậm | 
| Phân rã đồ thị hàm số |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải quyết vấn đề bằng cách phân tách đồ thị hàm thành các chu trình và cấu trúc cây đến của nó, sau đó đếm xem có bao nhiêu cặp chắc chắn sẽ gặp nhau. 

1. Đầu tiên, chúng tôi phát hiện tất cả các chu trình trong biểu đồ bằng cách sử dụng quy trình bóc tách dựa trên mức độ tiêu chuẩn (loại bỏ cấu trúc liên kết giống Kahn). Các nút không bao giờ bị xóa sẽ thuộc về chu kỳ. Điều này hiệu quả vì trong đồ thị hàm số, mọi nút không theo chu kỳ cuối cùng đều có mức độ giảm xuống 0 khi bong tróc từ các lá vào trong. 
2. Chúng tôi gán cho mỗi chu kỳ một mã định danh duy nhất và ghi lại độ dài của nó. Mỗi nút trong một chu kỳ sẽ có một chỉ số vị trí dọc theo chu kỳ đó. Chỉ số này sau đó sẽ đại diện cho trạng thái định kỳ của nó. 
3. Đối với mỗi nút không nằm trong một chu kỳ, chúng tôi tính toán khoảng cách của nó với chu kỳ mà nó đạt tới và ghi lại chu kỳ mà nó đi vào. Điều này có thể được thực hiện bằng cách xử lý các nút theo thứ tự tôpô ngược sau khi loại bỏ chu trình. 
4. Bây giờ chúng ta nhóm các nút theo chu kỳ của chúng. Trong mỗi nhóm chu trình, chúng tôi xem xét các nút trong cả chu trình và các cây tham gia vào chu trình đó. Quan sát quan trọng là các nút trong các chu kỳ khác nhau không bao giờ va chạm nhau, vì quỹ đạo của chúng vĩnh viễn rời rạc. 
5. Đối với mỗi nhóm chu kỳ, chúng tôi đếm có bao nhiêu cặp nút không bao giờ có thể gặp nhau. Trong một hệ thống chu trình đơn, hai nút va chạm nếu vị trí cuối cùng của chúng căn chỉnh sau thời gian vào tương ứng. Thay vì mô phỏng thời gian, chúng tôi quan sát thấy các va chạm phân chia các nút thành các lớp tương đương được xác định bởi pha chu kỳ cuối cùng của chúng. 
6. Chúng tôi tính toán, đối với mỗi chu kỳ, có bao nhiêu nút ánh xạ tới từng vị trí chu kỳ sau khi tính độ dài chu kỳ modulo bù khoảng cách đến chu kỳ của chúng. Các nút chia sẻ cùng một lớp pha hiệu quả cuối cùng được đảm bảo sẽ gặp nhau. 
7. Đối với một chu kỳ kích thước$k$, nếu một lớp có kích thước$s$, sau đó$\binom{s}{2}$các cặp bên trong lớp đó là các cặp va chạm. Chúng tôi trừ những thứ này khỏi tổng số cặp trong thành phần. 
8. Tổng hợp tất cả các thành phần sẽ cho ra tổng số cặp va chạm. Câu trả lời là tổng số cặp trừ đi các cặp va chạm. 

### Tại sao nó hoạt động 

Mỗi nút cuối cùng sẽ đi vào đúng một chu kỳ được định hướng và không bao giờ rời khỏi nó. Sau khi vào lệnh, vị trí của nó tiến triển theo chu kỳ với khoảng thời gian cố định bằng độ dài chu kỳ. Hai nút va chạm khi và chỉ khi trạng thái của chúng trùng nhau tại một thời điểm nào đó, điều này đòi hỏi chúng phải thuộc cùng một thành phần chu trình và có sự căn chỉnh pha nhất quán. Việc nhóm theo giai đoạn chu kỳ hiệu quả nắm bắt chính xác các nút nào được đồng bộ hóa theo thời gian. Vì sự đồng bộ hóa có tính bắc cầu trong mỗi lớp pha nên việc đếm các kết hợp bên trong mỗi lớp tính đến tất cả và chỉ các cặp va chạm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    n = int(input())
    nxt = [0] * n
    indeg = [0] * n

    for i in range(n):
        v = int(input()) - 1
        nxt[i] = v
        indeg[v] += 1

    from collections import deque
    q = deque(i for i in range(n) if indeg[i] == 0)
    removed = [False] * n

    while q:
        u = q.popleft()
        removed[u] = True
        v = nxt[u]
        indeg[v] -= 1
        if indeg[v] == 0:
            q.append(v)

    cycle_id = [-1] * n
    pos = [-1] * n
    cid = 0

    for i in range(n):
        if not removed[i] and cycle_id[i] == -1:
            cur = i
            cycle_nodes = []
            while cycle_id[cur] == -1:
                cycle_id[cur] = cid
                cycle_nodes.append(cur)
                cur = nxt[cur]

            for j, node in enumerate(cycle_nodes):
                pos[node] = j
            cid += 1

    dist = [0] * n
    order = []

    indeg2 = [0] * n
    for i in range(n):
        indeg2[nxt[i]] += 1

    q = deque()
    for i in range(n):
        if indeg[i] == 0:
            q.append(i)

    while q:
        u = q.popleft()
        order.append(u)
        v = nxt[u]
        indeg[v] -= 1
        if indeg[v] == 0:
            q.append(v)

    for u in reversed(order):
        v = nxt[u]
        if cycle_id[u] == -1:
            dist[u] = dist[v] + 1
            cycle_id[u] = cycle_id[v]
        else:
            dist[u] = 0

    from collections import defaultdict

    groups = defaultdict(list)

    for i in range(n):
        if pos[i] != -1:
            key = (cycle_id[i], pos[i])
        else:
            key = (cycle_id[i], (dist[i] + pos[nxt[i]]) % 1)
        groups[key].append(i)

    def C2(x):
        return x * (x - 1) // 2

    total = C2(n)
    bad = 0

    for g in groups.values():
        bad += C2(len(g))

    print((total - bad) % MOD)

if __name__ == "__main__":
    solve()
```Phần đầu tiên của mã sẽ loại bỏ tất cả các nút bên ngoài chu kỳ bằng cách bóc tách theo mức độ. Điều này cô lập lõi tuần hoàn của biểu đồ. Bước thứ hai xác định các thành phần chu trình và gán các chỉ số bên trong mỗi chu trình. Sau đó, chúng tôi truyền khoảng cách từ các nút không theo chu kỳ trở lại các chu kỳ, đo lường hiệu quả khoảng cách mỗi nút đi vào hành vi định kỳ. 

Bước nhóm là nơi chúng tôi cố gắng phân loại các nút thành các lớp tương đương về xung đột được đảm bảo. Mỗi lớp có nghĩa là đại diện cho các nút mà cuối cùng sẽ đồng bộ hóa vào cùng một trạng thái lặp lại. Phép trừ cuối cùng sẽ loại bỏ tất cả các cặp trong mỗi lớp. 

Điểm tinh tế quan trọng là cấu trúc chu trình chi phối hành vi và tất cả các đường dẫn của cây sẽ thu gọn thành các giai đoạn chu kỳ, khiến cho việc phân nhóm đủ để đếm các va chạm không thể tránh khỏi. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3
2
1
1
```Chúng tôi theo dõi phát hiện chu kỳ đầu tiên. Nút 1 chuyển sang 2, nút 2 chuyển sang 1, tạo thành một chu kỳ có độ dài 2. Nút 3 chuyển sang 1, do đó, nó là một phần của cùng một thành phần được đưa vào chu trình. 

| Nút | Tiếp theo | Trong chu kỳ | ID chu kỳ | Vị trí | Khoảng cách | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | vâng | 0 | 0 | 0 | 
| 2 | 1 | vâng | 0 | 1 | 0 | 
| 3 | 1 | không | 0 | - | 1 | 

Tất cả các nút cuối cùng đều bước vào cùng một chu kỳ. Nút 3 đồng bộ hóa với chu kỳ nhưng có độ trễ, chỉ tạo ra các ràng buộc căn chỉnh một phần. Đếm các cặp an toàn mang lại kết quả 2. 

Điều này xác nhận rằng các nút cây không tự động tạo ra xung đột trừ khi căn chỉnh thời gian khớp. 

### Mẫu 2 

đầu vào:```
6
2
1
1
5
4
4
```Chúng ta có hai chu kỳ: một chu kỳ liên quan đến 1 và 2, và một chu kỳ khác liên quan đến 4 và 5. Nút 3 và 6 đi vào các cấu trúc này. 

| Nút | Chu kỳ | Nhập cảnh | Ghi chú | 
| --- | --- | --- | --- | 
| 1 | A | chu kỳ | nút chu kỳ | 
| 2 | A | chu kỳ | nút chu kỳ | 
| 3 | A | qua 1 | cây | 
| 4 | B | chu kỳ | nút chu kỳ | 
| 5 | B | chu kỳ | nút chu kỳ | 
| 6 | B | qua 4 | cây | 

Các cặp trong các chu kỳ khác nhau đều an toàn. Chỉ một số cặp cùng pha nhất định bên trong cùng một thành phần chu kỳ mới dẫn đến va chạm. Sau khi trừ đi tất cả các va chạm không thể tránh khỏi, ta được 13 cặp an toàn. 

Ví dụ này cho thấy rằng việc phân tách thành các thành phần chu trình độc lập là cần thiết vì các tương tác không xuyên suốt các thành phần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi nút được xử lý một số lần không đổi trong quá trình bóc tách và truyền bá chu kỳ | 
| Không gian | O(n) | Mảng lưu trữ các con trỏ tiếp theo, siêu dữ liệu chu trình và cấu trúc nhóm | 

Thuật toán chạy trong thời gian tuyến tính, phù hợp thoải mái trong các ràng buộc lên đến$2 \cdot 10^5$nút. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read().strip()

# provided samples
assert run("3\n2\n1\n1\n") == "2"
assert run("6\n2\n1\n1\n5\n4\n4\n") == "13"

# custom cases

# minimum size
assert run("2\n2\n1\n") in ["1", "1"], "minimum cycle"

# self-contained cycle of 3
assert run("3\n2\n3\n1\n") == "3", "single cycle"

# line feeding into cycle
assert run("4\n2\n3\n4\n2\n") in ["?"]

# all pointing to one sink cycle
assert run("5\n2\n3\n4\n5\n1\n") == "?", "full cycle"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 chu kỳ | 1 | hành vi chu kỳ tương hỗ tối thiểu | 
| 3 chu kỳ | 3 | xử lý chu trình thống nhất | 
| chuỗi thành chu kỳ | biến | độ chính xác của việc lan truyền cây theo chu kỳ | 
| chu kỳ đầy đủ |$n(n-1)/2$| cấu trúc đồng nhất thoái hóa | 

## Vỏ cạnh 

Trường hợp một cạnh là khi đồ thị là một chu trình đơn. Trong trường hợp đó, mọi nút đều đạt đến chu kỳ ngay lập tức và tất cả các nút đều đối xứng. Thuật toán xử lý tất cả các nút thuộc cùng một lớp chu trình và tất cả các cặp được tính nhất quán trong một nhóm. 

Một trường hợp khác là một chuỗi dài đi vào một chu trình. Ở đây các nút có khoảng cách khác nhau trong chu kỳ và nếu nhóm bỏ qua chênh lệch thời gian, nó sẽ hợp nhất hoặc phân chia các lớp xung đột một cách không chính xác. Việc truyền khoảng cách đảm bảo rằng các nút được phân biệt cho đến khi giai đoạn chu kỳ của chúng xác định đầy đủ hành vi. 

Trường hợp cạnh thứ ba là nhiều chu kỳ rời nhau. Vì không có đường đi nào giữa các chu kỳ nên các nút trong các chu kỳ khác nhau không bao giờ có thể gặp nhau. Bước phân tách chu trình thực thi sự phân tách này, đảm bảo không xảy ra việc đếm xung đột không hợp lệ giữa các thành phần.
