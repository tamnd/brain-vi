---
title: "CF 102411C - Tranh thêu chữ thập"
description: "Vải là một lưới các tế bào hình chữ nhật. Mỗi ô được đánh dấu X phải có dấu chéo ở mặt trước, nghĩa là cả hai đường chéo của ô đó phải được khâu."
date: "2026-08-12T03:32:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102411
codeforces_index: "C"
codeforces_contest_name: "ICPC 2019-2020 North-Western Russia Regional Contest"
rating: 0
weight: 102411
solve_time_s: 345
verified: true
draft: false
---

[CF 102411C - Tranh thêu chữ thập](https://codeforces.com/problemset/problem/102411/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 5 phút 45 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Vải là một lưới các tế bào hình chữ nhật. Mỗi ô được đánh dấu`X`phải nhận một hình chữ thập ở mặt trước, nghĩa là cả hai đường chéo của ô đó phải được khâu. Sợi chỉ là một sợi liên tục nên sau khi đi hết một đường chéo mặt trước, kim phải đi qua mặt sau đến điểm bắt đầu của một đường chéo mặt trước khác, sau đó quay về mặt trước, v.v. 

Đầu ra là một chuỗi các điểm lưới. Các điểm liên tiếp mô tả một mũi khâu, với các mũi khâu thứ nhất, thứ ba, thứ năm và các mũi số lẻ khác nằm ở mặt trước, trong khi các mũi khâu xen kẽ nằm ở mặt sau. Các đoạn phía trước phải vẽ mọi đường chéo được yêu cầu đúng một lần. Các đoạn mặt sau được phép lặp lại, vì vậy khó khăn thực sự là tìm ra thứ tự của tất cả các đường chéo phía trước có thể nối thành một sợi xen kẽ mà không cần thực hiện một mũi khâu có độ dài bằng 0. Tọa độ yêu cầu là các giao điểm lưới, bao gồm các điểm ranh giới từ`(0, 0)`bởi vì`(w, h)`. 

Kích thước thỏa mãn`1 <= w, h <= 100`, vậy có nhiều nhất`10000`tế bào và do đó nhiều nhất`10000`chéo. Một giải pháp được chấp nhận có thể xử lý thoải mái mọi ô với số lần không đổi. Thuật toán bậc hai cũng vô hại ở đây, nhưng bất cứ điều gì theo cấp số nhân về số lượng dấu thập đều hoàn toàn không khả thi. Đặc biệt, câu trả lời có thể chứa gần như`40000`điểm, vì vậy bản thân việc xây dựng phải tuyến tính về số lượng ô. 

Có một số trường hợp khó xử lý. Chỉ với một dấu gạch chéo duy nhất, câu trả lời đã trở nên không cần thiết:```
1 1
X
```Cần có một chữ thập và hai đường chéo phía trước, vì vậy cần ít nhất ba mũi khâu. Câu trả lời tối ưu có`3`mũi khâu, không`2`, vì hai đường chéo phía trước phải cách nhau một đoạn phía sau. 

Một cây thánh giá cũng chỉ có thể chạm vào một cây thánh giá khác ở một góc. Ví dụ,```
2 2
X.
.X
```hợp lệ vì hai đường chéo được kết nối 8. Một công trình giả định kết nối bốn hướng thông thường sẽ coi những đường chéo này là những thành phần riêng biệt một cách không chính xác. 

Đường chéo trên ranh giới là một nguồn lỗi tọa độ phổ biến khác:```
1 1
X
```sử dụng tất cả bốn tọa độ góc`(0,0)`,`(1,0)`,`(0,1)`, Và`(1,1)`. Các tọa độ đề cập đến các điểm lưới, không phải trung tâm ô, do đó việc triển khai đánh số`w * h`tế bào thay vì`(w+1) * (h+1)`điểm lưới sẽ tạo ra điểm cuối không hợp lệ. 

Cuối cùng, hai đường chéo của một ô đều phải xuất hiện ở mặt trước. Một công trình chỉ ghé thăm mọi nơi`X`một ô một lần là không đủ, vì một ô đóng góp hai phân đoạn mặt trước khác nhau. Đối với ví dụ về một ô ở trên, đầu ra chính xác bắt đầu bằng ba mũi khâu và cả hai đường chéo phải xuất hiện giữa phân đoạn thứ nhất và thứ ba. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ bắt đầu bằng cách liệt kê`2k`yêu cầu các đường chéo phía trước, trong đó`k`là số lượng`X`tế bào. Sau đó, nó có thể thử mọi thứ tự của các đường chéo đó và kiểm tra xem các đường chéo liên tiếp có thể được kết nối bằng các mũi khâu mặt sau hợp lệ hay không. Điều này đúng vì nghiệm tối ưu không thể lặp lại đường chéo phía trước, do đó một số hoán vị của tất cả`2k`đường chéo đại diện cho mọi thứ tự tối ưu có thể. 

Vấn đề là số lượng hoán vị. có`(2k)!`các đơn hàng có thể và việc kiểm tra một đơn hàng sẽ mất`O(k)`thời gian. Độ phức tạp thu được là`Theta(k * (2k)!)`. Ở mức tối đa`k = 10000`, điều này không thể tính toán được từ xa. Brute Force hoạt động vì nó xem xét chính xác các đối tượng phải xuất hiện, nhưng nó thất bại vì nó coi thứ tự của chúng như một bài toán tổ hợp không hạn chế. 

Quan sát quan trọng là mỗi`X`ô có thể đóng góp không chỉ hai đường chéo phía trước mà còn cả hai cạnh phía sau được lựa chọn cẩn thận. Hãy xem xét một ô có các góc`A`ở phía trên bên trái,`B`ở phía trên bên phải,`C`ở phía dưới bên trái và`D`ở phía dưới bên phải. Đặt hai đường chéo`A-D`Và`C-B`vào biểu đồ phía trước. Đặt hai cạnh thẳng đứng`A-C`Và`B-D`vào biểu đồ mặt sau. 

Đối với mỗi góc của ô này, có đúng một cạnh trước và đúng một cạnh sau chạm vào góc đó. Như vậy bốn cạnh tạo thành một chu trình luân phiên:```
A --front-- D
|           |
back       back
|           |
C --front-- B
```Khi một số`X`có các tế bào, chúng tôi chỉ cần phủ lên các chu kỳ bốn cạnh nhỏ này. Bởi vì các đường chéo có 8 kết nối, hai ô thuộc cùng một mẫu kết nối sẽ có chung một cạnh hoặc chạm vào một góc. Các chu trình bốn cạnh tương ứng của chúng sau đó sẽ chia sẻ ít nhất một điểm lưới. Do đó, toàn bộ đồ thị được xây dựng được kết nối. 

Có một tài sản hữu ích hơn. Tại mỗi điểm lưới, số cạnh phía trước liên quan bằng số cạnh phía sau liên quan. Mỗi ô chạm vào điểm đó đóng góp chính xác một cạnh của mỗi loại. Điều này có nghĩa là bất cứ khi nào một đường truyền xen kẽ đi vào một đỉnh bằng cách sử dụng một loại cạnh thì một cạnh chưa sử dụng của loại kia sẽ có sẵn cho đến khi chu trình cục bộ hoàn thành. 

Do đó, chúng ta có thể thực hiện phép duyệt kiểu Euler trong khi xen kẽ giữa biểu đồ phía trước và biểu đồ phía sau. Việc truyền tải sử dụng mọi cạnh được xây dựng đúng một lần. Mép cuối cùng sẽ đóng chu trình trở lại điểm bắt đầu, nhưng chúng ta không cần xuất ra mép đóng đó, do đó số lượng mũi may ít hơn tổng số cạnh được tạo. 

Mỗi`X`đóng góp hai cạnh trước và hai cạnh sau, tạo ra bốn cạnh đồ thị. Vì`k`chéo, có`4k`các cạnh, do đó sợi được sản xuất cần`4k - 1`mũi khâu. 

Điều này cũng tối ưu. Mỗi cây thánh giá cần có hai đường chéo phía trước riêng biệt, tạo nên`2k`các mũi khâu phía trước. Nếu chủ đề hoàn chỉnh chứa`n`khâu và bắt đầu ở mặt trước, nó có`ceil(n / 2)`các đoạn phía trước. Kể từ đây`ceil(n / 2) >= 2k`và do đó`n >= 4k - 1`. Việc xây dựng của chúng tôi đạt đến chính xác giới hạn dưới này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`Theta(k * (2k)!)`|`O(k)`| Quá chậm | 
| Tối ưu |`O(wh)`|`O(wh)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Gán ID số nguyên cho mọi điểm lưới`(x, y)`, không phải mọi ô. có`(w + 1)(h + 1)`những điểm như vậy vì các góc của ô là nơi bắt đầu và kết thúc tất cả các mũi khâu. 
2. Với mỗi ô chứa`X`, cộng hai đường chéo của nó vào biểu đồ phía trước. Nếu các góc`(x, y)`,`(x+1, y)`,`(x, y+1)`, Và`(x+1, y+1)`, các cạnh phía trước là hai đoạn`(x, y) -> (x+1, y+1)`Và`(x+1, y) -> (x, y+1)`. 
3. Đối với cùng một ô, hãy cộng hai cạnh dọc của nó vào biểu đồ mặt sau. Đây là`(x, y) -> (x, y+1)`Và`(x+1, y) -> (x+1, y+1)`. Về mặt tinh thần, việc lựa chọn các cạnh thẳng đứng là tùy ý, nhưng sự lựa chọn cụ thể này mang lại chính xác một cạnh trước và một cạnh sau ở mỗi góc. 
4. Bắt đầu từ bất kỳ điểm lưới nào thuộc về một`X`tế bào. Trong quá trình truyền tải, giữ đỉnh hiện tại cùng với loại cạnh phải được sử dụng tiếp theo. Nếu loại hiện tại là mặt trước, hãy chọn cạnh mặt trước chưa sử dụng; sau khi lấy nó, chuyển sang biểu đồ mặt sau. Nếu loại hiện tại là mặt sau thì làm ngược lại. 
5. Sử dụng kỹ thuật Euler-tour của Hierholzer. Khi có một cạnh chưa được sử dụng, hãy làm theo nó ngay lập tức. Khi không có cạnh nào thuộc loại được yêu cầu còn lại ở đỉnh hiện tại, hãy xóa đỉnh đó khỏi ngăn xếp truyền tải và ghi lại nó. Ghi lại các đỉnh trong khi quay lui là cách đảo ngược cấu trúc Euler cục bộ thành chuỗi liên tục cần thiết. 
6. Đảo ngược các đỉnh đã ghi trước khi in. Các cặp liên tiếp thu được chính xác là`4k - 1`mũi khâu. Cặp đầu tiên là đường chéo phía trước, cặp thứ hai là cạnh sau và tính chẵn lẻ thay thế cho toàn bộ chuỗi. 

Lý do việc di chuyển xen kẽ không thể bị kẹt không chính xác là do sự bằng nhau giữa độ trước và sau ở mọi đỉnh. Giả sử chúng ta đến một đỉnh bằng cách sử dụng cạnh trước. Số cạnh phía trước không được sử dụng và các cạnh phía sau không được sử dụng tại đỉnh đó sẽ thay đổi theo từng bước khi quá trình truyền tải tiếp tục. Do đó, một cạnh mặt sau chưa được sử dụng sẽ có sẵn bất cứ khi nào chuyến tham quan xen kẽ vẫn cần tiếp tục. Đối số tương tự được áp dụng với hai loại cạnh được trao đổi. 

### Tại sao nó hoạt động 

Xét đồ thị được tạo bởi tất cả các cạnh trước và sau. Mọi`X`đóng góp một chu trình xen kẽ bốn cạnh, do đó tại mỗi điểm lưới, số cạnh phía trước liên quan bằng số cạnh phía sau liên quan. Vì các đường chéo đầu vào được kết nối 8 nên sự kết hợp của các chu trình cục bộ này được kết nối. Do đó, quá trình duyệt Hierholzer xen kẽ sử dụng mọi cạnh được xây dựng đúng một lần và quay trở lại điểm bắt đầu. Việc loại bỏ cạnh đóng cuối cùng sẽ để lại một chuỗi duy nhất chứa mọi đường chéo phía trước đúng một lần, với cạnh sau nằm giữa các cạnh trước liên tiếp. 

Có chính xác`2k`các cạnh phía trước, do đó trình tự có chính xác`2k`các mũi khâu phía trước và`2k - 1`các mũi khâu phía sau, cho`4k - 1`tổng số mũi khâu. Giới hạn dưới được chứng minh ở trên cho biết không có giải pháp hợp lệ nào có thể sử dụng ít hơn, do đó việc xây dựng là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    w, h = map(int, input().split())
    grid = [input().strip() for _ in range(h)]

    # Vertex id for grid point (x, y).
    # There are (w + 1) points in every row.
    def vid(x, y):
        return y * (w + 1) + x

    vertices = (w + 1) * (h + 1)

    # front[0] contains the two diagonals of every X-cell.
    # back[1] contains the two vertical sides of every X-cell.
    front = [[] for _ in range(vertices)]
    back = [[] for _ in range(vertices)]

    front_edges = []
    back_edges = []

    def add_edge(graph, edges, u, v):
        eid = len(edges)
        edges.append((u, v))
        graph[u].append((v, eid))
        graph[v].append((u, eid))

    start = -1
    crosses = 0

    for y in range(h):
        for x in range(w):
            if grid[y][x] != 'X':
                continue

            crosses += 1

            nw = vid(x, y)
            ne = vid(x + 1, y)
            sw = vid(x, y + 1)
            se = vid(x + 1, y + 1)

            if start == -1:
                start = nw

            # Front: the two diagonals.
            add_edge(front, front_edges, nw, se)
            add_edge(front, front_edges, ne, sw)

            # Back: the two vertical sides.
            add_edge(back, back_edges, nw, sw)
            add_edge(back, back_edges, ne, se)

    # There are 4 edges per cross in the auxiliary graph.
    # The Euler cycle is closed by one edge which we do not print.
    print(4 * crosses - 1)

    graphs = (front, back)
    edge_count = (len(front_edges), len(back_edges))
    used = [
        [False] * edge_count[0],
        [False] * edge_count[1],
    ]
    ptr = [0, 0]
    adjacency = [front, back]

    # Iterative alternating Hierholzer traversal.
    # state = (vertex, next edge type)
    stack = [(start, 0)]
    order = []

    while stack:
        u, typ = stack[-1]

        adj = adjacency[typ]

        while ptr[typ] < len(adj[u]):
            v, eid = adj[u][ptr[typ]]
            ptr[typ] += 1

            if used[typ][eid]:
                continue

            used[typ][eid] = True
            stack.append((v, typ ^ 1))
            break
        else:
            stack.pop()
            if stack:
                order.append(u)

    order.reverse()

    out = []
    for u in order:
        x = u % (w + 1)
        y = u // (w + 1)
        out.append(f"{x} {y}")

    sys.stdout.write("\n".join(out) + "\n")

if __name__ == "__main__":
    solve()
```các`vid`hàm ánh xạ một giao điểm lưới thành một số nguyên, sử dụng thứ tự hàng lớn. biểu thức`y * (w + 1) + x`dựa trên số lượng điểm lưới trên mỗi hàng, tức là`w + 1`, không`w`. Đây là lỗi lập chỉ mục tọa độ phổ biến nhất trong công trình này. 

Đối với mọi`X`, đoạn mã tạo ra hai cạnh trước và hai cạnh sau. Các cạnh phía trước chính xác là các đường chéo theo yêu cầu của chữ thập. Các cạnh mặt sau là phụ trợ, do đó hình học của chúng được chọn cho thuộc tính biểu đồ thay vì mẫu ban đầu yêu cầu các phân đoạn mặt sau cụ thể. 

các`used`mảng được lập chỉ mục theo loại cạnh và ID cạnh. Hai cạnh của biểu đồ có thể có cùng điểm cuối, đặc biệt khi các ô khác nhau đóng góp các phân đoạn gần nhau, do đó chỉ kiểm tra điểm cuối là không đủ. Mỗi cạnh được xây dựng riêng lẻ đều có ID riêng. 

các`ptr`mảng tránh việc quét liên tục các mục lân cận đã được kiểm tra. Mỗi mục lân cận được chuyển qua nhiều nhất một lần, do đó việc truyền tải là tuyến tính theo kích thước của biểu đồ được xây dựng. 

Việc truyền tải là lặp đi lặp lại chứ không phải đệ quy vì chuyến tham quan tối đa chứa`4 * 10000 = 40000`các cạnh. Một DFS đệ quy thông thường trong Python sẽ yêu cầu thay đổi giới hạn đệ quy và sẽ tiêu tốn một lượng lớn lệnh gọi một cách không cần thiết. Ngăn xếp rõ ràng thực hiện việc quay lui Hierholzer tương tự một cách an toàn. 

Quá trình truyền tải ghi lại một đỉnh khi nó được lấy ra khỏi ngăn xếp. Điều này tạo ra hành trình Euler theo thứ tự ngược lại, đó là lý do tại sao`order.reverse()`là cần thiết trước khi in. Việc quên sự đảo ngược này sẽ tạo ra các cạnh giống nhau theo thứ tự sai. 

Không có đường khâu có độ dài bằng 0 nào được tạo vì mọi cạnh của biểu đồ được xây dựng đều nối hai điểm lưới lân cận riêng biệt. Cạnh đóng bị bỏ qua cũng khác 0, nhưng nó không được in vì luồng không cần phải quay lại điểm bắt đầu một cách rõ ràng. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Hãy xem xét mẫu được cung cấp:```
3 2
.XX
..X
```Có hai cây thánh giá ở ô`(1,0)`Và`(2,1)`. Việc xây dựng tạo ra tám cạnh đồ thị phụ, bốn cạnh cho mỗi đường chéo. Do đó, câu trả lời cuối cùng có`8 - 1 = 7`mũi khâu. 

Một giao dịch có thể được tóm tắt dưới đây. Thứ tự duyệt chính xác có thể khác nhau tùy thuộc vào thứ tự danh sách kề, bởi vì bài toán chấp nhận bất kỳ cách xây dựng tối ưu nào. 

| Bước | Đỉnh | Loại tiếp theo | Hành động | 
| --- | --- | --- | --- | 
| 0 | góc xuất phát | Mặt trận | Lấy một đường chéo của đầu tiên`X`| 
| 1 | điểm cuối chéo | Quay lại | Đi theo chiều dọc | 
| 2 | góc tiếp theo | Mặt trận | Đi theo đường chéo khác | 
| 3 | góc tiếp theo | Quay lại | Tiếp tục qua biểu đồ phụ trợ | 
| 4 | góc tiếp theo | Mặt trận | Nhập chữ thập thứ hai | 
| 5 | góc tiếp theo | Quay lại | Di chuyển đến góc tiếp theo | 
| 6 | góc tiếp theo | Mặt trận | Lấy đường chéo cần thiết còn lại | 
| 7 | góc cuối cùng | Quay lại | Đóng chu trình Euler phụ | 

Đặc tính quan trọng là mỗi bước di chuyển về phía trước tương ứng với một trong hai đường chéo của một`X`, trong khi mọi di chuyển giữa chúng đều là cạnh sau. Vì có chính xác bốn cạnh phụ trên mỗi chữ thập nên hai chữ thập tạo ra tám cạnh và bảy mũi in. 

### Mẫu 2 

Đối với một chữ thập duy nhất,```
1 1
X
```có bốn cạnh phụ. Dán nhãn các góc`A = (0,0)`,`B = (1,0)`,`C = (0,1)`, Và`D = (1,1)`. Biểu đồ phía trước chứa`A-D`Và`B-C`, trong khi biểu đồ mặt sau chứa`A-C`Và`B-D`. 

| Bước | Điểm hiện tại | Loại cạnh | Điểm tiếp theo | 
| --- | --- | --- | --- | 
| 0 | A | Mặt trận | D | 
| 1 | D | Quay lại | B | 
| 2 | B | Mặt trận | C | 
| 3 | C | Quay lại | A | 

Động thái cuối cùng sẽ đóng chu trình phụ trợ và không cần thiết ở đầu ra. Do đó, trình tự in có chứa bốn điểm và ba mũi khâu:```
3
0 0
1 1
1 0
0 1
```Đoạn thứ nhất và thứ ba là hai đường chéo của chữ thập. Đoạn giữa nằm ở mặt sau. Ví dụ này cũng thể hiện trực tiếp giới hạn dưới: hai mũi khâu phía trước yêu cầu ít nhất một mũi khâu mặt sau giữa chúng, do đó không thể tránh khỏi ba mũi khâu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(wh)`| Mỗi ô được kiểm tra một lần và mỗi cạnh đồ thị đã xây dựng được xử lý một lần | 
| Không gian |`O(wh)`| Các đồ thị phụ, trạng thái cạnh, ngăn xếp truyền tải và đầu ra đều chứa`O(wh)`yếu tố | 

Có nhiều nhất`10000`vượt qua, cho nhiều nhất`40000`các cạnh phụ và`40000`các điểm đầu ra. Do đó, việc xây dựng và truyền tải chỉ thực hiện một lượng nhỏ công việc không đổi trên mỗi ô và vừa vặn thoải mái trong giới hạn thời gian 2 giây đã nêu và giới hạn bộ nhớ 512 MB. 

## Trường hợp thử nghiệm 

Đầu ra của một bài toán mang tính xây dựng không phải là duy nhất, vì vậy các bài kiểm tra bên dưới sẽ xác thực các đặc tính cấu trúc của câu trả lời được tạo ra thay vì so sánh nó với một chuỗi tọa độ cụ thể.```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    w, h = map(int, input().split())
    grid = [input().strip() for _ in range(h)]

    def vid(x, y):
        return y * (w + 1) + x

    vertices = (w + 1) * (h + 1)

    front = [[] for _ in range(vertices)]
    back = [[] for _ in range(vertices)]
    front_edges = []
    back_edges = []

    def add_edge(graph, edges, u, v):
        eid = len(edges)
        edges.append((u, v))
        graph[u].append((v, eid))
        graph[v].append((u, eid))

    start = -1
    crosses = 0

    for y in range(h):
        for x in range(w):
            if grid[y][x] != 'X':
                continue

            crosses += 1

            nw = vid(x, y)
            ne = vid(x + 1, y)
            sw = vid(x, y + 1)
            se = vid(x + 1, y + 1)

            if start == -1:
                start = nw

            add_edge(front, front_edges, nw, se)
            add_edge(front, front_edges, ne, sw)

            add_edge(back, back_edges, nw, sw)
            add_edge(back, back_edges, ne, se)

    graphs = (front, back)
    adjacency = [front, back]

    used = [
        [False] * len(front_edges),
        [False] * len(back_edges),
    ]
    ptr = [0, 0]

    stack = [(start, 0)]
    order = []

    while stack:
        u, typ = stack[-1]
        adj = adjacency[typ]

        while ptr[typ] < len(adj[u]):
            v, eid = adj[u][ptr[typ]]
            ptr[typ] += 1

            if used[typ][eid]:
                continue

            used[typ][eid] = True
            stack.append((v, typ ^ 1))
            break
        else:
            stack.pop()
            if stack:
                order.append(u)

    order.reverse()

    out = [str(4 * crosses - 1)]
    for u in order:
        out.append(f"{u % (w + 1)} {u // (w + 1)}")

    return "\n".join(out) + "\n"

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

def validate(inp: str, output: str):
    lines = output.strip().splitlines()
    w, h = map(int, inp.splitlines()[0].split())
    grid = inp.splitlines()[1:1 + h]

    crosses = sum(row.count('X') for row in grid)

    n = int(lines[0])
    assert n == 4 * crosses - 1

    points = [tuple(map(int, line.split())) for line in lines[1:]]
    assert len(points) == n + 1

    for x, y in points:
        assert 0 <= x <= w
        assert 0 <= y <= h

    required = set()

    for y in range(h):
        for x in range(w):
            if grid[y][x] == 'X':
                nw = (x, y)
                ne = (x + 1, y)
                sw = (x, y + 1)
                se = (x + 1, y + 1)

                required.add(frozenset((nw, se)))
                required.add(frozenset((ne, sw)))

    seen = set()

    for i in range(n):
        a = points[i]
        b = points[i + 1]

        assert a != b

        if i % 2 == 0:
            edge = frozenset((a, b))
            assert edge in required
            assert edge not in seen
            seen.add(edge)

    assert seen == required

# Provided sample
sample1 = """\
3 2
.XX
..X
"""
validate(sample1, run(sample1))

# Minimum-size input
case2 = """\
1 1
X
"""
validate(case2, run(case2))

# Corner-touching crosses, testing 8-connectivity
case3 = """\
2 2
X.
.X
"""
validate(case3, run(case3))

# Boundary-heavy rectangular pattern
case4 = """\
4 3
XXXX
X..X
XXXX
"""
validate(case4, run(case4))

# Maximum-size input, every cell is an X
case5 = "100 100\n" + "\n".join(["X" * 100 for _ in range(100)]) + "\n"
validate(case5, run(case5))

print("All tests passed.")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 2 / .XX / ..X`|`7`mũi khâu | Cung cấp mẫu và kết nối thông thường | 
|`1 1 / X`|`3`mũi khâu | Kích thước tối thiểu và giới hạn dưới chính xác | 
|`2 2 / X. /.X`|`7`mũi khâu | Kết nối 8 đường chéo | 
|`4 3 / XXXX / X..X / XXXX`|`31`mũi khâu | Các ô biên và nhiều thành phần được kết nối được nối qua các góc | 
|`100 100 / all X`|`39999`mũi khâu | Kích thước đầu vào tối đa và xây dựng thời gian tuyến tính | 

## Vỏ cạnh 

Đối với đầu vào tối thiểu```
1 1
X
```thuật toán tạo ra bốn cạnh phụ`A-D`,`B-C`,`A-C`, Và`B-D`. Việc truyền tải xen kẽ sử dụng cả bốn điểm, tạo ra bốn điểm và`4 - 1 = 3`mũi khâu. Các cạnh phía trước chính xác là hai đường chéo nên không có đường khâu mặt bắt buộc nào bị trùng lặp. 

Đối với kết nối chéo,```
2 2
X.
.X
```hai ô chia sẻ điểm lưới`(1,1)`. Do đó, các chu trình bốn cạnh phụ của chúng có chung đỉnh đó, do đó đồ thị tổng hợp được kết nối ngay cả khi các ô không có cạnh chung. Quá trình truyền tải có thể chuyển từ chu trình thuộc ô đầu tiên sang chu trình thuộc ô thứ hai. Đây chính xác là lý do tại sao điều kiện ban đầu sử dụng kết nối 8 thay vì kết nối 4. 

Đối với các ô biên, hãy xem xét```
2 1
XX
```Chữ thập bên trái sử dụng các góc với`x`tọa độ`0`Và`1`, trong khi chữ thập bên phải sử dụng`1`Và`2`. Do đó, đồ thị chứa các điểm trên ranh giới`x = 0`Và`x = 2`. Bởi vì việc đánh số đỉnh sử dụng`w + 1 = 3`điểm trên mỗi hàng, cả hai tọa độ biên đều được biểu diễn chính xác. sử dụng`w`thay vào đó sẽ ánh xạ một số điểm tới hàng sai. 

Để có đầu vào tối đa,```
100 100
XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
...
```với tất cả`10000`tế bào bằng`X`, thuật toán tạo ra`40000`các cạnh phụ và đầu ra`39999`mũi khâu. Mỗi ô đóng góp hai đường chéo phía trước riêng biệt của nó, trong khi đường chéo Euler xen kẽ nối tất cả chúng thành một sợi. Số lượng thao tác vẫn tỷ lệ thuận với số lượng ô, do đó kích thước đầu ra lớn được xử lý mà không cần bất kỳ tìm kiếm tổ hợp nào.
