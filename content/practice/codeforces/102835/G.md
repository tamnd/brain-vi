---
title: "CF 102835G - Thẻ đồ thị"
description: "Đầu vào mô tả một bộ thẻ đồ thị. Mỗi thẻ chứa một đồ thị đơn giản vô hướng. Một lá bài hợp lệ vì đồ thị có cùng số đỉnh, cạnh và được kết nối, có nghĩa là mỗi lá bài biểu thị một đồ thị một vòng liên thông: một cây có đúng một…"
date: "2026-07-26T14:59:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102835
codeforces_index: "G"
codeforces_contest_name: "The 2020 ICPC Asia Taipei-Hsinchu Site Programming Contest"
rating: 0
weight: 102835
solve_time_s: 41
verified: true
draft: false
---

[CF 102835G - Thẻ đồ thị](https://codeforces.com/problemset/problem/102835/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 41s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Đầu vào mô tả một bộ thẻ đồ thị. Mỗi thẻ chứa một đồ thị đơn giản vô hướng. Một lá bài hợp lệ vì đồ thị có cùng số đỉnh, số cạnh và được kết nối, có nghĩa là mỗi lá bài biểu thị một đồ thị một vòng liên thông: một cây có đúng một cạnh bổ sung. Hai lá bài được coi là giống nhau khi đồ thị của chúng có thể được dán nhãn lại để trở nên giống hệt nhau. Nhiệm vụ là đếm xem có bao nhiêu hình dạng biểu đồ khác nhau xuất hiện trong toàn bộ bộ bài. 

Các nhãn biểu đồ trên đầu vào không liên quan. Đồ thị có các đỉnh được đánh số`1,2,3`và cùng một đồ thị có các đỉnh được đánh số`10,20,30`chỉ nên đóng góp một lần. Thách thức là xây dựng một biểu diễn cấu trúc bỏ qua tên đỉnh. 

Số đỉnh trong một thẻ có thể lớn, do đó việc kiểm tra từng cặp đồ thị bằng thuật toán đẳng cấu đồ thị tổng quát là không thực tế. Việc so sánh đẳng cấu tổng quát có thể rất tốn kém, trong khi bài toán này có cấu trúc bổ sung. Một đồ thị liên thông có số đỉnh và cạnh bằng nhau có đúng một chu trình và mọi thứ nằm ngoài chu trình đó là tập hợp các cây có gốc. Cấu trúc đó cho phép chúng ta tạo một biểu diễn chuẩn theo thời gian tuyến tính trên mỗi thẻ. 

Các trường hợp cạnh xuất phát từ tính đối xứng của chu trình và từ các cây gắn liền với chu trình. Việc truyền tải đơn giản từ một đỉnh tùy ý không thành công vì điểm bắt đầu được chọn có thể khác nhau đối với hai đồ thị giống hệt nhau. 

Ví dụ: hai thẻ sau đây mô tả cùng một chu trình với các đỉnh được đánh số khác nhau:```
1
3 1 2 2 3 3 1
3 2 3 3 1 1 2
```Đầu ra đúng là:```
1
```Việc duyệt ghi lại thứ tự các đỉnh được truy cập có thể tạo ra các chuỗi khác nhau vì nó bắt đầu từ các vị trí khác nhau trên chu trình. 

Một sai lầm phổ biến khác là bỏ qua rằng một chu trình có thể đi theo một trong hai hướng. Ví dụ:```
1
4 1 2 2 3 3 4 4 1
4 1 4 4 3 3 2 2 1
```Đầu ra đúng là:```
1
```Biểu đồ thứ hai chỉ là biểu đồ đầu tiên được xem ngược. Một biểu diễn chỉ kiểm tra các phép quay chứ không phải các phép quay ngược sẽ coi chúng là khác nhau. 

Trường hợp cạnh thứ ba là một đỉnh chu kỳ có nhiều nhánh đính kèm. Xét một chu trình trong đó một đỉnh có hai lá con giống hệt nhau. Các em không có thứ tự nên cần phải sắp xếp các mô tả về trẻ. Nếu không sắp xếp, hai cây tương đương có thể nhận được các bảng mã khác nhau. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ là so sánh từng cặp thẻ và chạy kiểm tra đẳng cấu đồ thị. Điều này hiệu quả vì hai thẻ giống hệt nhau khi tồn tại sự đẳng cấu giữa đồ thị của chúng. Vấn đề là nếu có nhiều thẻ thì số so sánh sẽ trở thành bậc hai. Với`n`thẻ, điều này đã yêu cầu`O(n^2)`so sánh và mỗi so sánh có thể cần phải kiểm tra mọi cạnh và đỉnh, khiến tổng công việc trở nên quá lớn. 

Quan sát hữu ích là mọi đồ thị trong bài toán này đều có đúng một chu trình. Chúng ta không cần một thuật toán đẳng cấu đồ thị tổng quát. Đầu tiên chúng ta có thể xác định chu trình duy nhất, loại bỏ nó về mặt khái niệm và xem xét các cây có gốc treo ở mỗi đỉnh chu trình. 

Cây có gốc có dạng kinh điển đơn giản. Một chiếc lá được biểu diễn bằng một cặp dấu ngoặc đơn trống. Một đỉnh được biểu diễn bằng danh sách đã sắp xếp các dạng chính tắc con của nó được bao quanh bởi dấu ngoặc đơn. Việc sắp xếp loại bỏ sự phụ thuộc vào thứ tự con. 

Khó khăn duy nhất còn lại là chính chu kỳ. Chu trình là một chuỗi tuần tự mô tả cây gốc. Hai chuỗi tròn bằng nhau nếu một chuỗi có thể được quay hoặc đảo ngược để thu được chuỗi kia. Chúng ta tạo ra biểu diễn nhỏ nhất trong số tất cả các phép quay của chuỗi và tất cả các phép quay của chuỗi đảo ngược. 

Phương pháp vũ lực không thành công vì nó liên tục giải quyết cùng một vấn đề về cấu trúc. Biểu diễn chuẩn sẽ giải quyết nó một lần trên mỗi thẻ và biến việc kiểm tra đẳng thức thành so sánh chuỗi hoặc bộ dữ liệu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(C2 · V) | O(V) | Quá chậm | 
| Tối ưu | O(C · V log V) | O(V) | Đã chấp nhận | 

Đây`C`là số lượng thẻ và`V`là tổng số đỉnh trong một thẻ. Hệ số logarit xuất phát từ việc sắp xếp các nút con của cây. 

## Hướng dẫn thuật toán 

1. Đọc một thẻ đồ thị và xây dựng danh sách kề của nó. Các số đỉnh chỉ là các định danh tạm thời vì sự biểu diễn cuối cùng không được phụ thuộc vào chúng. 
2. Tìm chu trình duy nhất bằng cách cắt tỉa lá. Mỗi đỉnh có bậc bằng 1 không thể là một phần của chu trình, vì vậy việc loại bỏ nhiều lần các lá sẽ để lại chính xác các đỉnh của chu trình. Điều này hoạt động vì đồ thị một vòng chỉ có một thành phần đóng. 
3. Với mỗi đỉnh chu trình, hãy tính dạng chính tắc của cây gắn liền với nó trong khi bỏ qua hai đỉnh chu trình lân cận. Mã hóa cây kết hợp đệ quy tất cả các mã hóa con theo thứ tự được sắp xếp. 
4. Sắp xếp các bảng mã cây thu được theo thứ tự xung quanh chu trình. Đỉnh chu kỳ bắt đầu là tùy ý, do đó hãy tạo mỗi vòng quay. Đồng thời tạo ra mọi vòng quay theo thứ tự đảo ngược vì cùng một chu trình có thể được thực hiện theo hướng ngược lại. 
5. Chọn cách biểu diễn chu trình nhỏ nhất theo từ điển. Giá trị này là mã định danh chuẩn của toàn bộ biểu đồ. 
6. Chèn mã định danh vào một bộ. Sau khi tất cả các thẻ được xử lý, kích thước của tập hợp là số lượng các thẻ đồ thị khác nhau. 

Tại sao nó hoạt động: 

Mã hóa cây là duy nhất vì mỗi nút được biểu thị bằng tập hợp nhiều cây con của nó và việc sắp xếp sẽ chuyển đổi tập hợp đó thành một thứ tự xác định. Mã hóa chu trình là duy nhất vì mọi lựa chọn có thể về điểm bắt đầu và hướng đều được xem xét. Bất kỳ hai đồ thị một vòng đẳng cấu nào cũng phải có cùng độ dài chu trình và cùng một chuỗi các cây có gốc gắn liền với phép quay và phép đảo chiều, sao cho các định danh chính tắc của chúng khớp nhau. Các đồ thị không đẳng cấu không thể tạo ra cùng một mã định danh vì mã định danh sẽ xây dựng lại chu trình hoàn chỉnh và mọi cây đính kèm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def rooted_tree_code(v, parent, adj, on_cycle):
    children = []
    for u in adj[v]:
        if u != parent and not on_cycle[u]:
            children.append(rooted_tree_code(u, v, adj, on_cycle))
    children.sort()
    return "(" + "".join(children) + ")"

def canonical_graph(edges, n):
    adj = [[] for _ in range(n)]
    for u, v in edges:
        adj[u].append(v)
        adj[v].append(u)

    degree = [len(x) for x in adj]
    alive = [True] * n
    stack = [i for i in range(n) if degree[i] == 1]

    while stack:
        v = stack.pop()
        alive[v] = False
        for u in adj[v]:
            if alive[u]:
                degree[u] -= 1
                if degree[u] == 1:
                    stack.append(u)

    cycle = [i for i in range(n) if alive[i]]
    on_cycle = alive[:]

    cycle_adj = [[] for _ in range(n)]
    for v in cycle:
        for u in adj[v]:
            if on_cycle[u]:
                cycle_adj[v].append(u)

    start = cycle[0]
    order = []
    prev = -1
    cur = start

    while True:
        order.append(cur)
        nxt = cycle_adj[cur][0]
        if nxt == prev:
            nxt = cycle_adj[cur][1]
        prev, cur = cur, nxt
        if cur == start:
            break

    parts = [rooted_tree_code(v, -1, adj, on_cycle) for v in order]

    m = len(parts)
    best = None

    for arr in (parts, list(reversed(parts))):
        for i in range(m):
            cand = tuple(arr[i:] + arr[:i])
            if best is None or cand < best:
                best = cand

    return best

def solve():
    t = int(input())
    ans = []
    for _ in range(t):
        cards = int(input())
        seen = set()

        for _ in range(cards):
            data = list(map(int, input().split()))
            k = data[0]
            vals = data[1:]

            edges = []
            mx = 0
            for i in range(k):
                u = vals[2 * i] - 1
                v = vals[2 * i + 1] - 1
                edges.append((u, v))
                mx = max(mx, u, v)

            seen.add(canonical_graph(edges, mx + 1))

        ans.append(str(len(seen)))

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Danh sách kề lưu trữ biểu diễn đồ thị tạm thời. Vòng cắt lá sẽ loại bỏ mọi đỉnh của cây và chỉ để lại chu trình duy nhất. Boolean`on_cycle`mảng tách chu trình khỏi các cây đính kèm.`rooted_tree_code`không bao giờ đi vào một đỉnh chu kỳ khác, vì vậy mọi lệnh gọi đệ quy đều nằm trong một cây có gốc. Các phần tử con được sắp xếp trước khi xây dựng kết quả, đây là chi tiết loại bỏ sự phụ thuộc vào thứ tự đầu vào. 

Việc truyền chu kỳ tạo ra một hướng của chu kỳ. Vòng lặp cuối cùng xử lý tất cả các vị trí bắt đầu có thể và cả hai hướng. Một bộ dữ liệu được sử dụng làm mã định danh cuối cùng vì nó so sánh một cách tự nhiên và tránh sự mơ hồ giữa các chuỗi được nối. 

Việc triển khai không sử dụng đệ quy trên chính chu trình đó. Độ sâu đệ quy chỉ là độ sâu của cây đính kèm, là phần cần được biểu diễn về mặt cấu trúc. 

## Ví dụ đã hoạt động 

Hãy xem xét một thẻ tam giác duy nhất:```
1
3 1 2 2 3 3 1
```Dấu vết là: 

| Bước | Chu kỳ còn lại | Mã cây đính kèm | Tốt nhất hiện nay | 
| --- | --- | --- | --- | 
| Sau khi cắt tỉa | 1,2,3 |`()`,`()`,`()`| bỏ đặt | 
| Hướng đầu tiên |`(),(),()`| tất cả các phép quay bằng nhau |`(),(),()`| 
| Hướng ngược lại |`(),(),()`| tất cả các phép quay bằng nhau |`(),(),()`| 

Biểu đồ nhận được một mã định danh chuẩn, do đó các hình tam giác giống hệt nhau sẽ được hợp nhất. 

Xét một chu trình có thêm một lá:```
1
4 1 2 2 3 3 1 1 4
```Dấu vết là: 

| Bước | Chu kỳ còn lại | Mã cây đính kèm | Tốt nhất hiện nay | 
| --- | --- | --- | --- | 
| Sau khi cắt tỉa | 1,2,3 |`(()),(),()`| bỏ đặt | 
| Xoay 1 |`(()),(),()`| đã chọn |`(()),(),()`| 
| Các vòng quay khác |`(),(()),()`Và`(),(),(())`| lớn hơn | không thay đổi | 
| Xoay ngược | đã kiểm tra | không thay đổi |`(()),(),()`| 

Chiếc lá được gắn vào một đỉnh chu kỳ cụ thể và thứ tự chu trình chính tắc duy trì mối quan hệ đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(V log V) mỗi thẻ | Mỗi đỉnh được xử lý một lần và danh sách con được sắp xếp trong quá trình mã hóa cây | 
| Không gian | O(V) | Danh sách kề, cắt bớt mảng và lưu trữ thông tin trạng thái đệ quy cho một thẻ | 

Thuật toán tránh so sánh biểu đồ theo cặp, do đó tổng công việc tăng tuyến tính theo số lượng thẻ. Cấu trúc của đồ thị một vòng là yếu tố giúp cho việc chuẩn hóa có thể thực hiện được trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.read().split()
    sys.stdin = old

    it = iter(data)
    t = int(next(it))
    out = []

    for _ in range(t):
        c = int(next(it))
        seen = set()
        for _ in range(c):
            k = int(next(it))
            edges = []
            mx = 0
            for _ in range(k):
                u = int(next(it)) - 1
                v = int(next(it)) - 1
                edges.append((u, v))
                mx = max(mx, u, v)
            seen.add(canonical_graph(edges, mx + 1))
        out.append(str(len(seen)))

    return "\n".join(out)

assert run("""1
2
4 1 2 2 3 3 1 1 4
4 1 2 2 3 3 1 2 4
""") == "1"

assert run("""1
3
3 1 2 2 3 3 1
4 1 2 2 3 3 4 4 1
4 1 4 4 3 3 2 2 1
""") == "2"

assert run("""1
1
3 1 2 2 3 3 1
""") == "1"

assert run("""1
2
4 1 2 2 3 3 1 1 4
5 1 2 2 3 3 1 1 4 4 5
""") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hai hình tam giác có nhãn khác nhau | 1 | Bỏ qua việc đánh số đỉnh | 
| Chu kỳ có chuyển động đảo ngược | 2 | Xử lý đối xứng phản chiếu | 
| Tam giác đơn | 1 | Biểu đồ chỉ chu kỳ tối thiểu | 
| Cùng một chu kỳ với một nhánh được thêm vào | 2 | Phân biệt cây gắn liền | 

## Vỏ cạnh 

Một chu trình không có cây đính kèm sẽ được xử lý vì mọi đỉnh chu trình đều nhận được cùng một mã cây trống. Quá trình chuẩn hóa chu trình vẫn kiểm tra các phép quay và đảo chiều, do đó, một vòng tam giác hoặc vòng lớn hơn được xác định chính xác bất kể việc đánh số đầu vào. 

Biểu đồ trong đó sự khác biệt duy nhất là hướng được sử dụng để đi theo chu trình được xử lý bằng cách so sánh cả trình tự ban đầu và trình tự ngược lại của nó. Ví dụ, đầu vào```
1
4 1 2 2 3 3 4 4 1
```và cùng một chu trình được liệt kê ngược đều tạo ra cùng một biểu diễn tuần hoàn tối thiểu giống nhau. 

Một đỉnh có nhiều con giống hệt nhau được xử lý bằng cách sắp xếp các bảng mã con. Đối với một đỉnh chu kỳ được kết nối với hai lá, mã cây đính kèm sẽ trở thành tổ hợp được sắp xếp của hai mã lá giống hệt nhau. Thuật toán không bao giờ phụ thuộc vào thứ tự các cạnh xuất hiện trong đầu vào, do đó, cùng một cấu trúc phân nhánh luôn nhận được cùng một biểu diễn.
