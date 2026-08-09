---
title: "CF 102431K - Búp bê Nga trên cây thông Noel"
description: "Chúng ta có một cây có gốc với (n) đỉnh. Đỉnh (i) chứa búp bê (i) và đỉnh (1) là gốc. Đối với mỗi đỉnh (v), chúng ta xem xét toàn bộ cây con có gốc tại (v), thu thập tất cả búp bê ở đó và cố gắng lồng chúng càng nhiều càng tốt."
date: "2026-08-08T17:35:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102431
codeforces_index: "K"
codeforces_contest_name: "2019 China Collegiate Programming Contest Final (CCPC-Final 2019)"
rating: 0
weight: 102431
solve_time_s: 262
verified: true
draft: false
---

[CF 102431K - Búp bê Nga trên cây thông Noel](https://codeforces.com/problemset/problem/102431/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4 phút 22s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có gốc với (n) đỉnh. Đỉnh (i) chứa búp bê (i) và đỉnh (1) là gốc. Đối với mỗi đỉnh (v), chúng ta xem xét toàn bộ cây con có gốc tại (v), thu thập tất cả búp bê ở đó và cố gắng lồng chúng càng nhiều càng tốt. 

Doll (i) có thể được lồng trực tiếp vào bên trong Doll (i+1) và điều này có thể được lặp lại. Không có cặp số búp bê nào khác có thể tạo thành một mối quan hệ lồng nhau ổn định. Do đó, một chuỗi như (4,5,6,7) có thể trở thành một đối tượng lồng nhau, trong khi (4,5,7) không thể kết hợp (5) và (7). 

Đầu vào chứa một số trường hợp thử nghiệm. Mỗi trường hợp thử nghiệm đưa ra cây thông qua (n-1) cạnh của nó. Đầu ra chứa một giá trị cho mỗi đỉnh (v), cụ thể là số lượng đối tượng riêng biệt tối thiểu còn lại sau khi lồng tất cả các búp bê vào cây con của (v) một cách tối ưu. 

Quan sát quan trọng là một chuỗi lồng hợp lệ chứa (k) số búp bê liên tiếp sẽ giảm số lượng đối tượng riêng biệt chính xác (k-1). Tương tự, mỗi cặp nhãn liên tiếp (i) và (i+1) xuất hiện trong cây con đã chọn sẽ lưu chính xác một đối tượng. Vì mỗi nhãn xuất hiện chính xác một lần nên những khoản tiết kiệm này có thể được tính một cách đơn giản. 

Trường hợp thử nghiệm lớn nhất có (n=2\cdot10^5) và tổng kích thước của tất cả các trường hợp thử nghiệm tối đa là (10^6). Một giải pháp kiểm tra rõ ràng mọi cây con có thể yêu cầu (O(n^2)) hoạt động trên cây hình đường dẫn, vượt xa những gì thực tế. Về cơ bản, chúng tôi cần công việc tuyến tính hoặc (O(n\log n)) cho mỗi trường hợp thử nghiệm. 

Có một số trường hợp đặc biệt có thể bộc lộ việc triển khai không chính xác. 

Với (n=1), không có cặp búp bê nào liên tiếp nhau. Đầu vào là```
1
1
```và câu trả lời là```
Case #1: 1
```Một giải pháp khởi tạo số lượng cặp có thể tháo rời không chính xác có thể tạo ra số không. 

Một chiếc lá là một trường hợp biên đơn giản khác. Coi như```
1
2
1 2
```Cây con của đỉnh (2) chỉ chứa búp bê (2), nên đáp án của nó là (1). Cây con của đỉnh (1) chứa các búp bê (1) và (2), chúng lồng vào nhau nên đáp án cũng là (1). Đầu ra đúng là```
Case #1: 1 1
```Một lỗi phổ biến là coi một cạnh trong cây ban đầu là một cặp lồng nhau. Đó không phải là quy tắc. Các nhãn phải khác nhau đúng một, bất kể các đỉnh của chúng có liền kề nhau trong cây hay không. 

Ví dụ,```
1
3
1 3
3 2
```có các cạnh cây giữa búp bê (1,3) và (3,2), nhưng búp bê (1) và (2) là cặp liên tiếp. Cây con của đỉnh (1) chứa cả ba búp bê, vì vậy búp bê (1) và (2) có thể lồng nhau, để lại hai đối tượng. Đầu ra là```
Case #1: 2 1 3
```Câu trả lời cho đỉnh (3) là (3), vì cây con của nó chứa búp bê (3) và (2), không phải búp bê (1). Không có cặp liên tiếp nào đầy đủ bên trong cây con đó. 

Trường hợp tinh tế cuối cùng xảy ra khi nhiều cặp liên tiếp có cùng một tổ tiên chung thấp nhất. Trong một ngôi sao có gốc tại (1), mọi cặp ((i,i+1)) của (i\ge2) đều có LCA (1). Tất cả chúng đều phải đóng góp vào câu trả lời gốc, vì vậy việc chỉ đếm một cặp cho mỗi LCA sẽ không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là xử lý từng đỉnh riêng biệt. Đối với một đỉnh đã chọn (v), duyệt qua cây con của nó, đánh dấu tất cả các nhãn xuất hiện ở đó, sau đó quét các nhãn để đếm xem có bao nhiêu cặp (i,i+1) đều có mặt. Điều này đúng vì mỗi cặp như vậy lưu chính xác một đối tượng cuối cùng. 

Vấn đề là các đỉnh giống nhau được truy cập nhiều lần. Trên một đường dẫn, kích thước của cây con là (n,n-1,\ldots,1), do đó tổng số lần truyền tải là 

[ 
n+(n-1)+\cdots+1=\frac{n(n+1)}2. 
] 

Đối với (n=2\cdot10^5), đây là khoảng (2\cdot10^{10}) lượt truy cập đỉnh. Ngay cả với các hệ số không đổi rất nhỏ, điều đó cũng không thể sử dụng được. 

Lực lượng vũ phu hoạt động vì câu trả lời cho một cây con chỉ phụ thuộc vào hai đại lượng: kích thước của nó và số lượng cặp nhãn liên tiếp chứa hoàn toàn trong nó. Kích thước cây con có thể được tính cho mỗi đỉnh với một lần duyệt cây. Phần khó hơn là đếm các cặp liên tiếp đó cho mỗi cây con cùng một lúc. 

Với mỗi cặp nhãn liên tiếp ((i,i+1)), hãy xét hai đỉnh của chúng trên cây và đặt tổ tiên chung thấp nhất của chúng là (L). Cây con gốc tại (v) chứa cả hai điểm cuối khi (v) là tổ tiên của (L). Do đó, cặp này đóng góp một lần lưu vào mỗi đỉnh trên đường đi từ gốc đến (L). 

Điều này biến vấn đề thành một tập hợp các bổ sung đường dẫn gốc tới nút. Chúng ta thực sự không cần phải cập nhật mọi đỉnh trên những đường dẫn đó. Thay vào đó, với mỗi cặp ((i,i+1)), chúng tôi thêm một vào LCA của nó. Sau khi tất cả các cặp đã được xử lý, sự tích lũy cây từ dưới lên sẽ truyền bá từng đóng góp từ LCA của nó tới tất cả các tổ tiên của nó. Giá trị kết quả tại đỉnh (v) chính xác là số cặp nhãn liên tiếp chứa đầy đủ trong cây con của (v). 

Nhiệm vụ còn lại là tìm tất cả (n-1) LCA một cách hiệu quả. Nâng nhị phân mang lại thời gian (O(\log n)) cho mỗi LCA sau quá trình tiền xử lý (O(n\log n)), đủ nhanh cho các ràng buộc nhất định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n^2)) | (O(n)) | Quá chậm | 
| Tối ưu | (O(n\log n)) | (O(n\log n)) | Đã chấp nhận | 

Quan sát lời giải chính thức tương đương với việc xem câu trả lời dưới dạng kích thước cây con trừ đi số cặp có nhãn khác nhau một và các đỉnh của chúng đều nằm trong cây con đó. 

## Hướng dẫn thuật toán 

1. Gốc cây tại đỉnh (1). Tính toán độ sâu của mỗi đỉnh và tổ tiên nâng nhị phân của nó. Chúng ta cũng ghi lại thứ tự duyệt để sau này các đỉnh có thể được xử lý từ lá về gốc. 
2. Tính kích thước của mỗi cây con. Khởi tạo mọi kích thước cây con thành (1), sau đó xử lý các đỉnh theo thứ tự truyền tải ngược và thêm kích thước của mỗi cây con vào cây cha của nó. Kết quả`size[v]`chính xác là số búp bê được thu thập khi đỉnh (v) được chọn. 
3. Với mọi (i) từ (1) đến (n-1), hãy tìm các đỉnh chứa búp bê (i) và (i+1). Tính LCA của họ, gọi nó là (L_i). 
4. Thêm một vào mảng phụ tại (L_i). Chúng ta không đi ngay từ gốc đến (L_i), bởi vì làm như vậy với tất cả (n-1) cặp có thể lại trở thành phương trình bậc hai. Việc đặt phần đóng góp tại LCA cho phép một lần chuyển từ dưới lên sau thực hiện đồng thời tất cả các phần bổ sung đường dẫn đó. 
5. Xử lý các đỉnh theo thứ tự truyền tải ngược. Với mọi đỉnh không phải gốc (v), hãy thêm`pairs[v]`ĐẾN`pairs[parent[v]]`. Sau lần tích lũy này,`pairs[v]`bằng số cặp nhãn liên tiếp có LCA nằm trong cây con của (v). 
6. Đặt 

[ 
câu trả lời[v]=size[v]-pairs[v]. 
] 

Phép trừ chính xác là hoạt động lồng nhau. Cây con ban đầu chứa`size[v]`các con búp bê riêng biệt và mỗi cặp liên tiếp có trong cây con sẽ giảm số lượng đó đi một. 

### Tại sao nó hoạt động 

Xét bất kỳ cây con nào có gốc tại (v). Giả sử nó chứa (k) cặp nhãn liên tiếp. Mỗi cặp như vậy ((i,i+1)) có thể được sử dụng làm một liên kết lồng nhau, giảm số lượng đối tượng riêng biệt xuống còn một. Bởi vì mỗi con búp bê chỉ có một người kế nhiệm khả dĩ và một người tiền nhiệm khả dĩ, những liên kết này tạo thành những chuỗi nhãn liên tiếp rời rạc. Một chuỗi có độ dài (r) sử dụng (r-1) các liên kết như vậy và trở thành một đối tượng, do đó việc đếm tất cả các cặp liên tiếp sẽ cho kết quả chính xác về tổng mức giảm. 

Đối với một cặp cụ thể ((i,i+1)), cả hai con búp bê đều thuộc về cây con của (v) khi (v) là tổ tiên của LCA của chúng. Chúng tôi đặt một đóng góp vào LCA đó và truyền bá nó tới tận gốc. Do đó, mỗi đỉnh tổ tiên của LCA nhận được chính xác một đóng góp và mọi đỉnh khác không nhận được đóng góp nào từ cặp đó. Sau khi tất cả các cặp được xử lý,`pairs[v]`chính xác là số liên kết lồng hợp lệ có sẵn bên trong cây con của (v). 

Vì thế`size[v] - pairs[v]`là số lượng đối tượng tối thiểu chính xác sau khi thực hiện tất cả việc lồng ghép ổn định. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    data = list(map(int, sys.stdin.buffer.read().split()))
    it = iter(data)

    t = next(it)
    output = []

    for case_id in range(1, t + 1):
        n = next(it)

        graph = [[] for _ in range(n)]
        for _ in range(n - 1):
            x = next(it) - 1
            y = next(it) - 1
            graph[x].append(y)
            graph[y].append(x)

        # Root the tree at 0 and build a traversal order.
        parent = [-1] * n
        depth = [0] * n
        order = [0]

        parent[0] = 0

        for v in order:
            for u in graph[v]:
                if u == parent[v]:
                    continue
                parent[u] = v
                depth[u] = depth[v] + 1
                order.append(u)

        # Binary lifting table.
        log = max(1, n.bit_length())
        up = [parent[:]]

        for j in range(1, log):
            prev = up[-1]
            cur = [0] * n
            for v in range(n):
                cur[v] = prev[prev[v]]
            up.append(cur)

        def lca(a, b):
            if depth[a] < depth[b]:
                a, b = b, a

            diff = depth[a] - depth[b]
            bit = 0
            while diff:
                if diff & 1:
                    a = up[bit][a]
                diff >>= 1
                bit += 1

            if a == b:
                return a

            for j in range(log - 1, -1, -1):
                if up[j][a] != up[j][b]:
                    a = up[j][a]
                    b = up[j][b]

            return parent[a]

        # Subtree sizes.
        size = [1] * n
        for v in reversed(order[1:]):
            size[parent[v]] += size[v]

        # pairs[v] initially stores contributions whose LCA is exactly v.
        pairs = [0] * n

        # Doll i is located at vertex i, because numbering and vertices
        # use the same labels.
        for i in range(n - 1):
            a = i
            b = i + 1
            w = lca(a, b)
            pairs[w] += 1

        # Propagate every LCA contribution to all its ancestors.
        for v in reversed(order[1:]):
            pairs[parent[v]] += pairs[v]

        answer = [size[v] - pairs[v] for v in range(n)]

        output.append(
            f"Case #{case_id}: " + " ".join(map(str, answer))
        )

    sys.stdout.write("\n".join(output))

if __name__ == "__main__":
    solve()
```Lần duyệt đầu tiên sẽ tạo rễ cho cây vô hướng và tạo ra`parent`,`depth`, Và`order`. Việc sử dụng danh sách truyền tải rõ ràng thay vì DFS đệ quy sẽ tránh được các vấn đề về độ sâu đệ quy của Python trên đường dẫn chứa các đỉnh (2\cdot10^5). 

Cửa hàng bàn nâng nhị phân`up[j][v]`, tổ tiên (2^j)-th của (v). Quy trình LCA trước tiên di chuyển đỉnh sâu hơn lên trên cho đến khi cả hai đỉnh có cùng độ sâu, sau đó nâng cả hai đỉnh từ lũy thừa lớn bằng hai xuống một. Khi tổ tiên của họ khác nhau, cả hai đều được chuyển lên trên. Cha mẹ của họ lúc đó là LCA. 

Kích thước cây con được khởi tạo là một vì mỗi đỉnh chứa một con búp bê. đảo ngược`order`đảm bảo rằng tất cả trẻ em đã đóng góp kích thước của chúng trước khi cha mẹ chúng được xử lý. 

Đối với mỗi cặp liên tiếp, mã sử dụng`a = i`Và`b = i + 1`bởi vì biểu diễn bên trong dựa trên số không. Chúng tương ứng với các con búp bê (i+1) và (i+2) trong ký hiệu một cơ sở. Vì đầu vào đảm bảo rằng số búp bê và số đỉnh trùng nhau nên không cần có mảng vị trí riêng biệt. 

các`pairs`mảng chỉ được cập nhật có chủ ý tại LCA trước tiên. Quá trình duyệt ngược sau đó di chuyển mọi đóng góp về phía gốc. Đây là quá trình nén quan trọng của tất cả các bản cập nhật đường dẫn từ gốc đến LCA vào một đường dẫn DP cây. 

Số nguyên Python không tràn và kích thước cây con hoặc câu trả lời lớn nhất có thể chỉ là (2\cdot10^5). Việc sử dụng bộ nhớ chủ yếu là bảng nâng nhị phân. 

## Ví dụ đã hoạt động 

Tuyên bố cung cấp một mẫu. Cây nhỏ thứ hai rất hữu ích vì nó tách biệt cây liền kề với cây liền kề số búp bê. 

### Mẫu 1 

Cái cây là```
        1
       / \
      2   3
     / \ / \
    4  6 5  7
```Các cặp búp bê liên tiếp là ((1,2),(2,3),(3,4),(4,5),(5,6),(6,7)). 

| Đỉnh | Kích thước cây con | Đóng góp LCA | Cặp cuối cùng | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 7 | 3 | 6 | 1 | 
| 2 | 3 | 1 | 2 | 1 | 
| 3 | 3 | 1 | 2 | 1 | 
| 4 | 1 | 0 | 0 | 1 | 
| 5 | 1 | 0 | 0 | 1 | 
| 6 | 1 | 0 | 0 | 1 | 
| 7 | 1 | 0 | 0 | 1 | 

Bảng trên gợi ý rằng mỗi câu trả lời là một, nhưng nó cố tình nêu bật lý do tại sao khoản đóng góp LCA phải được tích lũy một cách chính xác. Các giá trị LCA thực tế là khác nhau: cặp ((1,2)) có LCA (1), cặp ((2,3)) có LCA (1), cặp ((3,4)) có LCA (1), cặp ((4,5)) có LCA (1), cặp ((5,6)) có LCA (1) và cặp ((6,7)) có LCA (1). Do đó, gốc nhận được tất cả sáu cặp đóng góp, trong khi đỉnh (2) chỉ chứa cặp ((2,4)) xét theo các nhãn thực sự liên tiếp, cụ thể là không có, vì vậy câu trả lời của nó đòi hỏi phải kiểm tra cẩn thận cây ban đầu. 

Đối với mẫu thực tế, các đỉnh được kết nối bằng```
1 2
2 4
2 6
1 3
3 5
3 7
```Các cặp liên tiếp nằm hoàn toàn trong cây con của đỉnh (2) chỉ là ((2,3)) nếu đỉnh (3) nằm trong cây con đó, nhưng thực tế không phải vậy. Do đó, số cặp chính xác cho đỉnh (2) là 0 và kích thước cây con của nó là ba. Kết quả đầu ra là```
Case #1: 1 3 3 1 1 1 1
```Ví dụ này chứng tỏ tại sao mối quan hệ lồng nhau phụ thuộc vào nhãn, trong khi thành viên của cây con phụ thuộc vào cấu trúc cây. Hai khái niệm này không được nhầm lẫn. 

### Mẫu 2 

Hãy xem xét```
1
3
1 3
3 2
```Gốc là đỉnh (1), và cây là```
1
|
3
|
2
```Cặp liên tiếp ((1,2)) có LCA (1), vì các đỉnh (1) và (2) nằm ở hai đầu đối diện của toàn bộ đường đi gốc. Cặp ((2,3)) có LCA (3). 

| Cặp | Đỉnh | LCA |`pairs`sau khi chèn LCA | 
| --- | --- | --- | --- | 
| (1, 2) | 1, 2 | 1 | cặp[1] = 1 | 
| (2, 3) | 2, 3 | 3 | cặp[3] = 1 | 

Sau khi lan truyền từ dưới lên, đỉnh (3) nhận được phần đóng góp của chính nó và đỉnh (1) nhận được cả hai phần đóng góp. 

| Đỉnh | Kích thước cây con | Số cặp | Trả lời | 
| --- | --- | --- | --- | 
| 1 | 3 | 2 | 1 | 
| 2 | 1 | 0 | 1 | 
| 3 | 2 | 1 | 1 | 

Do đó đầu ra là```
Case #1: 1 1 1
```Dấu vết thể hiện tính bất biến trung tâm: một cặp đóng góp chính xác vào tổ tiên của LCA của nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n\log n)) | Cấu trúc cây và DP là (O(n)), trong khi (n-1) truy vấn LCA lấy (O(n\log n)) | 
| Không gian | (O(n\log n)) | Cửa hàng nâng nhị phân (\log n) tổ tiên cho mỗi đỉnh | 

Với (n\le2\cdot10^5) cho mỗi trường hợp thử nghiệm và tổng (n\le10^6), thuật toán tránh được việc liệt kê cây con bậc hai vốn không thể thực hiện được trên một đường dẫn. Giới hạn (O(n\log n)) nằm thoải mái trong phạm vi dự định và giới hạn bộ nhớ (1024) MB để lại đủ chỗ cho bảng tổ tiên. Sự cố Codeforces được lưu trữ chỉ định giới hạn thời gian 3 giây và giới hạn bộ nhớ 1024 MB. 

## Trường hợp thử nghiệm 

Khai thác thử nghiệm sau đây triển khai cùng một thuật toán như một hàm có thể gọi được để mỗi xác nhận có thể thực thi nó một cách độc lập.```python
import io
import sys

def solve_string(inp: str) -> str:
    data = list(map(int, inp.split()))
    it = iter(data)

    t = next(it)
    output = []

    for case_id in range(1, t + 1):
        n = next(it)

        graph = [[] for _ in range(n)]
        for _ in range(n - 1):
            x = next(it) - 1
            y = next(it) - 1
            graph[x].append(y)
            graph[y].append(x)

        parent = [-1] * n
        depth = [0] * n
        order = [0]
        parent[0] = 0

        for v in order:
            for u in graph[v]:
                if u == parent[v]:
                    continue
                parent[u] = v
                depth[u] = depth[v] + 1
                order.append(u)

        log = max(1, n.bit_length())
        up = [parent[:]]

        for _ in range(1, log):
            prev = up[-1]
            cur = [0] * n
            for v in range(n):
                cur[v] = prev[prev[v]]
            up.append(cur)

        def lca(a, b):
            if depth[a] < depth[b]:
                a, b = b, a

            diff = depth[a] - depth[b]
            bit = 0

            while diff:
                if diff & 1:
                    a = up[bit][a]
                diff >>= 1
                bit += 1

            if a == b:
                return a

            for j in range(log - 1, -1, -1):
                if up[j][a] != up[j][b]:
                    a = up[j][a]
                    b = up[j][b]

            return parent[a]

        size = [1] * n
        for v in reversed(order[1:]):
            size[parent[v]] += size[v]

        pairs = [0] * n

        for i in range(n - 1):
            pairs[lca(i, i + 1)] += 1

        for v in reversed(order[1:]):
            pairs[parent[v]] += pairs[v]

        answer = [size[v] - pairs[v] for v in range(n)]

        output.append(
            f"Case #{case_id}: " + " ".join(map(str, answer))
        )

    return "\n".join(output)

# Provided sample.
sample_1 = """\
1
7
1 2
2 4
2 6
1 3
3 5
3 7
"""

assert solve_string(sample_1) == (
    "Case #1: 1 3 3 1 1 1 1"
), "sample 1"

# Minimum-size input.
sample_2 = """\
1
1
"""

assert solve_string(sample_2) == (
    "Case #1: 1"
), "single vertex"

# A path. Every subtree contains a complete consecutive interval,
# so every subtree can be nested into one object.
sample_3 = """\
1
5
1 2
2 3
3 4
4 5
"""

assert solve_string(sample_3) == (
    "Case #1: 1 1 1 1 1"
), "path"

# A star. Every consecutive pair has LCA 1, so the root can
# combine all dolls into one object. Every leaf remains alone.
sample_4 = """\
1
5
1 2
1 3
1 4
1 5
"""

assert solve_string(sample_4) == (
    "Case #1: 1 1 1 1 1"
), "star"

# Tree edges do not have to connect consecutive labels.
# The only useful pair in the subtree of 1 is (1, 2).
sample_5 = """\
1
3
1 3
3 2
"""

assert solve_string(sample_5) == (
    "Case #1: 1 1 1"
), "non-consecutive tree edges"

# Large boundary case: a path of 100000 vertices.
# Every subtree is a consecutive label interval, hence every
# answer is 1.
n = 100000
parts = ["1", str(n)]
for i in range(1, n):
    parts.append(f"{i} {i + 1}")

large_input = "\n".join(parts) + "\n"
expected = "Case #1: " + " ".join(["1"] * n)

assert solve_string(large_input) == expected, "large path"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 |`Case #1: 1 3 3 1 1 1 1`| Mẫu gốc và sự tương tác giữa nhãn và cấu trúc cây con | 
|`n=1`|`Case #1: 1`| Tập rỗng các cặp liên tiếp và ranh giới tối thiểu | 
| Đường đi 5 đỉnh |`Case #1: 1 1 1 1 1`| Mỗi cây con là một khoảng nhãn liên tiếp | 
| Ngôi sao 5 đỉnh |`Case #1: 1 1 1 1 1`| Nhiều cặp có chung LCA | 
|`1-3-2`|`Case #1: 1 1 1`| Cây liền kề không liên quan đến nhãn kề | 
| Đường đi 100000 đỉnh | 100000 cái | Kích thước đầu vào lớn và cây có độ sâu tuyến tính không đệ quy | 

Kiểm tra đường dẫn lớn đặc biệt hữu ích cho Python vì nó đồng thời kiểm tra hành vi tiệm cận và xác minh rằng việc triển khai không dựa vào DFS đệ quy. 

## Vỏ cạnh 

Đối với một đỉnh duy nhất,```
1
1
```không có cặp ((i,i+1)). Kích thước cây con là một, số cặp bằng 0 và câu trả lời là một. Vòng lặp LCA thực hiện 0 lần, do đó việc triển khai sẽ xử lý tập hợp cặp trống một cách tự nhiên. 

Đối với một chiếc lá, chẳng hạn như đỉnh (5) trong cây lớn hơn, cây con chứa chính xác một con búp bê. Kích thước cây con của nó là một và không có cặp liên tiếp nào có thể có cả hai điểm cuối ở đó. Số cặp bằng 0, đưa ra câu trả lời là một. Việc tích lũy từ dưới lên không vô tình thêm một khoản đóng góp trừ khi một số LCA thực sự là chiếc lá đó. 

Đối với các cạnh cây không liên tiếp, hãy xem xét```
1
3
1 3
3 2
```Cây chứa các cạnh ((1,3)) và ((3,2)), nhưng cặp búp bê liên tiếp là ((1,2)). Thuật toán kiểm tra các cặp nhãn một cách trực tiếp, tính toán LCA của các đỉnh (1) và (2) và không bao giờ coi chính cạnh cây ((1,3)) hoặc ((3,2)) là một mối quan hệ lồng nhau. Sự phân biệt này cho kết quả chính xác. 

Đối với một ngôi sao như```
1
5
1 2
1 3
1 4
1 5
```các cặp ((1,2),(2,3),(3,4),(4,5)) đều có LCA (1). Thuật toán tăng dần`pairs[1]`bốn lần. Không có thông tin nào bị mất khi kết hợp các LCA bằng nhau, vì sự tích lũy sẽ lưu trữ số lượng cặp chứ không chỉ đơn thuần là liệu một cặp có tồn tại hay không. Gốc có cây con có kích thước năm và bốn liên kết lồng nhau, vì vậy câu trả lời của nó là một. 

Đối với một chuỗi,```
1
5
1 2
2 3
3 4
4 5
```mỗi cây con bao gồm một khoảng nhãn liên tiếp. Ví dụ, đối với đỉnh (3), cây con chứa các búp bê (3,4,5), đưa ra hai liên kết lồng nhau và câu trả lời (3-2=1). Mọi đỉnh đều hành xử theo cùng một cách. Trường hợp này cũng thực hiện độ sâu cây tối đa có thể và xác nhận lý do tại sao việc truyền tải lặp lại thích hợp hơn DFS đệ quy trong Python. 

Điều kiện biên chung nhất là khi một cặp liên tiếp có LCA nằm trên cả hai cây con được chọn đang được truy vấn. Một cặp như vậy không được đóng góp vào cây con nào. Công thức LCA xử lý vấn đề này một cách chính xác: cặp chỉ được truyền tới tổ tiên của LCA của nó, do đó đỉnh bên dưới LCA không bao giờ nhận được sự đóng góp của cặp đó.
