---
title: "CF 102361E - Thoát hiểm"
description: "Chúng ta có một lưới n × m chứa các ô trống và các ô bị chặn. Mỗi robot bắt đầu ngay phía trên lưới trong một cột riêng biệt và ban đầu di chuyển xuống dưới. Mỗi lối ra nằm ngay dưới lưới trong một cột riêng biệt. Robot thường đi thẳng."
date: "2026-08-13T00:11:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102361
codeforces_index: "E"
codeforces_contest_name: "2019 China Collegiate Programming Contest Qinhuangdao Onsite"
rating: 0
weight: 102361
solve_time_s: 226
verified: true
draft: false
---

[CF 102361E - Thoát](https://codeforces.com/problemset/problem/102361/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 46s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một`n × m`lưới chứa các ô trống và các ô bị chặn. Mỗi robot bắt đầu ngay phía trên lưới trong một cột riêng biệt và ban đầu di chuyển xuống dưới. Mỗi lối ra nằm ngay dưới lưới trong một cột riêng biệt. 

Robot thường đi thẳng. Cách duy nhất để đổi hướng là đặt một trong bốn thiết bị quay hình góc trên một ô trống. Mỗi thiết bị kết nối chính xác một cạnh ngang của ô với một cạnh thẳng đứng, do đó về mặt hình học, nó hoạt động giống như một góc. Không thể vượt qua một thiết bị từ hướng không tương thích. 

Robot được phép chiếm giữ cùng một ô trống, vì vậy việc chiếm giữ ô thông thường không phải là hạn chế về năng lực. Khó khăn đến từ việc một thiết bị quay chiếm toàn bộ tế bào của nó. Robot không sử dụng thiết bị không thể đi thẳng qua ô đó. Điều này tạo ra thuộc tính phân tách hữu ích giữa các đường đi khác nhau của robot. 

Đối với mọi trường hợp thử nghiệm, đầu vào cung cấp lưới, cột bắt đầu của tất cả robot và cột của tất cả các lần thoát. Câu hỏi đặt ra là liệu chúng ta có thể đặt các thiết bị quay để mọi robot cuối cùng cũng đến được lối ra nào đó mà không chạm vào ô bị chặn hoặc sử dụng thiết bị từ hướng bất hợp pháp hay không. Chúng ta chỉ cần xuất ra`Yes`hoặc`No`. 

Kích thước lưới tối đa là`100 × 100`, vậy có nhiều nhất`10,000`tế bào. Có thể có nhiều nhất`100`robot. Một giải pháp gần như`O(nm)`hoặc`O(anm)`hoạt động có thể dễ dàng quản lý. Giải pháp liệt kê cấu hình của các ô là vô vọng, bởi vì ngay cả trước khi xem xét chuyển động của robot, mọi ô trống đều có năm khả năng: không có thiết bị hoặc một trong bốn hướng của thiết bị. Số lượng cấu hình có thể đạt`5^(nm)`, vốn đã lớn về mặt thiên văn đối với`nm = 10,000`. Danh sách cuộc thi ban đầu đưa ra giới hạn thời gian một giây, vì vậy giải pháp dự định cần khai thác cấu trúc lưới thay vì cấu hình tìm kiếm. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Hãy xem xét một ô bị chặn duy nhất:```
1
1 1 1 1
1
1
1
```Robot duy nhất bắt đầu phía trên ô duy nhất và lối ra duy nhất ở bên dưới ô đó. Vì ô bị chặn nên câu trả lời đúng là`No`. Việc triển khai kết nối trực tiếp mọi cột nguồn với nút lưới dọc của nó mà không kiểm tra xem ô đầu tiên có trống hay không sẽ chấp nhận nó một cách không chính xác. 

Vấn đề tương tự xảy ra ở phía dưới:```
1
1 1 1 1
1
1
1
```Ở đây lưới bị chặn nên không có đường dẫn đến lối ra. Tổng quát hơn, không thể tiếp cận được lối ra bên dưới ô phía dưới bị chặn. Biểu đồ chỉ được kết nối các lối ra với trạng thái dọc của các ô đáy trống. 

Mê cung một hàng cũng cần được chăm sóc vì robot có thể cần rẽ theo chiều ngang và sau đó lại quay xuống trong cùng một hàng:```
1
1 2 1 1
00
1
2
```Câu trả lời là`Yes`. Robot đi vào cột`1`theo chiều dọc, rẽ phải ở ô đầu tiên, rẽ xuống ở ô thứ hai và đi ra theo lối ra`2`. Một biểu đồ giả định mọi chuyển động thẳng đứng phải đi qua ít nhất hai hàng khác nhau sẽ bỏ lỡ cấu trúc hợp lệ này. 

Cuối cùng, hãy xem xét mẫu thứ hai chính thức:```
1
3 4 2 2
0000
0011
0000
3 4
2 4
```Câu trả lời là`No`. Robot xuất phát theo cột`3`và robot bắt đầu ở cột`4`cả hai có thể được thực hiện để tiếp cận lối ra`4`chỉ bằng cách chia sẻ một cấu trúc rẽ không thể bị chiếm giữ bởi hai đường dẫn khác nhau theo cách yêu cầu. Một kiểm tra khả năng tiếp cận đơn giản tính toán một đường dẫn cho mỗi robot một cách độc lập có thể nói`Yes`, bởi vì mỗi robot đều có một lối thoát riêng biệt. Vấn đề thực sự là liệu tất cả các tuyến đường có thể cùng tồn tại ở một vị trí đặt thiết bị hay không. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp là quyết định điều gì xảy ra trong mỗi ô trống. Mỗi ô có năm lựa chọn: để trống hoặc cài đặt một trong bốn thiết bị ở góc có thể. Đối với cấu hình, chúng ta có thể mô phỏng mọi robot trong khi ghi nhớ ô và hướng hiện tại của nó. Vì trạng thái là một cặp bao gồm một ô và một trong bốn hướng, nên một robot xác định có thể được mô phỏng nhiều nhất là`4nm`trạng thái trước khi thoát ra hoặc lặp lại một trạng thái. Đạt đến trạng thái lặp lại có nghĩa là robot bị mắc kẹt trong một chu kỳ. 

Nếu có`k`ô trống, số lượng cấu hình là`5^k`. Trong trường hợp xấu nhất`k = nm = 10,000`, vì vậy thậm chí chỉ cần liệt kê các cấu hình yêu cầu`5^10000`trường hợp. Thêm mô phỏng lên đến`a`robot sẽ cung cấp khoảng`O(5^(nm) · a · nm)`, điều đó hoàn toàn không thể thực hiện được. 

Quan sát hữu ích là hình học cho phép chúng ta quên hướng chính xác của thiết bị trong giây lát. Hãy suy nghĩ về một tế bào từ hai quan điểm khác nhau. Robot có thể di chuyển theo chiều dọc trong ô hoặc có thể di chuyển theo chiều ngang qua ô. Một thiết bị quay chính xác là thứ kết nối hai khả năng này. 

Đối với mỗi ô trống`(i,j)`, tạo hai đỉnh đồ thị. Cho phép`V(i,j)`biểu thị chuyển động theo chiều dọc trong ô và cho`H(i,j)`đại diện cho chuyển động ngang qua tế bào. 

Nếu hai ô liền kề theo chiều dọc đều trống, robot có thể tiếp tục theo chiều dọc giữa chúng`V`đỉnh. Tương tự, hai ô trống liền kề theo chiều ngang được kết nối giữa chúng`H`đỉnh. 

Bên trong một ô trống, kết nối`V(i,j)`Và`H(i,j)`. Sử dụng kết nối này có nghĩa là đặt một thiết bị quay ở đó. Hướng của dòng tương ứng xác định hướng nào trong bốn hướng góc là bắt buộc. Hai hướng ngược nhau của cùng một kết nối tương ứng với hai cách hợp pháp để đi qua cùng một góc. 

Có một quan sát hình học thứ hai giải thích tại sao dung lượng đơn vị là đủ. Hai robot không thể chia sẻ một đoạn thẳng hữu ích và sau đó tách ra. Nếu một thiết bị quay được đặt trên phân đoạn chung của chúng, robot không quay sẽ phải đi thẳng qua một ô có thiết bị quay chiếm giữ, điều này là bất hợp pháp. Nếu không có thiết bị như vậy tồn tại, hai robot vẫn đi trên cùng một quỹ đạo cho đến khi thoát ra và các lối thoát riêng biệt không thể khiến quỹ đạo chung đó trở thành hai tuyến đường khác nhau. Do đó, một giải pháp khả thi có thể được biểu diễn bằng các đường dẫn không cạnh tranh trên cùng một đường ngang hoặc dọc. Đây là lý do chính cho việc sử dụng công suất một trên các mép đường ray. Các giải pháp tiêu chuẩn cho vấn đề này sử dụng chính xác cấu trúc lưới hai lớp và lưu lượng tối đa. 

Nguồn kết nối với`V(1,p)`cho mọi robot bắt đầu ở cột`p`. Điều này mô hình thực tế là mọi robot đều đi vào hàng đầu tiên theo chiều dọc. Đối với mỗi cột thoát`e`, kết nối`V(n,e)`vào bồn rửa, vì chạm tới đáy ô đó trong khi di chuyển xuống dưới đồng nghĩa với việc robot có thể rời khỏi mê cung. 

Đồ thị kết quả chỉ có`O(nm)`các đỉnh và các cạnh. Sau đó chúng tôi hỏi liệu lưu lượng tối đa có bằng số lượng robot hay không. Nếu đúng như vậy, luồng sẽ phân tách thành các đường dẫn từ nguồn đến lối ra hợp lệ và mỗi kết nối giữa trạng thái ngang và dọc sẽ cho chúng ta biết vị trí đặt thiết bị quay. Nếu lưu lượng tối đa nhỏ hơn số lượng robot thì một số robot không thể được chỉ định đường dẫn tương thích. 

Việc xây dựng biên tập tiêu chuẩn thường được thực hiện bằng thuật toán Dinic. Vì mọi công suất liên quan là một và tổng lưu lượng mong muốn nhiều nhất là`a ≤ 100`, chúng ta có thể sử dụng cách triển khai luồng tối đa chuyên dụng thậm chí còn đơn giản hơn: chạy BFS liên tục để tìm một đường dẫn tăng cường và gửi một đơn vị qua đó. Nhiều nhất`a`sự tăng cường là cần thiết. Điều này mang lại một`O(aE)`ràng buộc, ở đây dễ dàng đủ nhỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(5^(nm) · a · nm)`|`O(nm)`| Quá chậm | 
| Tối ưu |`O(a · nm)`|`O(nm)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo hai đỉnh cho mỗi ô lưới, một đỉnh biểu thị chuyển động dọc và một đỉnh biểu thị chuyển động ngang. Các ô bị chặn không nhận được cạnh nào có thể sử dụng được. Việc phân chia là cần thiết vì một lượt rẽ chính xác là sự chuyển đổi từ kiểu chuyển động này sang kiểu chuyển động khác. 
2. Đối với mỗi cặp ô trống liền kề theo chiều dọc, nối các đỉnh dọc của chúng theo cả hai hướng bằng dung lượng`1`. Robot có thể di chuyển thẳng lên hoặc xuống dọc theo kết nối này. Công suất thể hiện thực tế là hai quỹ đạo khả thi của robot không thể cạnh tranh trên cùng một đường dẫn được định hướng. 
3. Đối với mỗi cặp ô trống liền kề theo chiều ngang, nối các đỉnh ngang của chúng theo cả hai hướng bằng dung lượng`1`. Đây là bước ngang tương đương với bước trước. 
4. Đối với mỗi ô trống, nối đỉnh ngang và đỉnh dọc của nó theo cả hai hướng bằng dung lượng`1`. Sử dụng cạnh này có nghĩa là ô chứa thiết bị quay. Ví dụ: một luồng đi vào trạng thái thẳng đứng từ phía trên và rời khỏi trạng thái nằm ngang tương ứng với một điều kiện thích hợp.`NE`hoặc`NW`thiết bị tùy thuộc vào hướng ngang của dòng chảy. Việc đảo ngược chuyển động sẽ mang lại quá trình di chuyển ngược hợp pháp tương ứng. 
5. Thêm một siêu nguồn và một siêu chìm. Đối với mỗi robot ở cột`p`, kết nối nguồn với`V(1,p)`với năng lực`1`, miễn là ô đầu tiên trống. Công suất một đại diện cho một robot bắt đầu từ đó. 
6. Đối với mỗi lối ra tại cột`e`, kết nối`V(n,e)`đến bồn rửa có dung tích`1`, miễn là ô dưới cùng trống. Robot phải di chuyển xuống dưới khi rời khỏi mê cung nên lối ra thuộc về lớp dọc. 
7. Tính lưu lượng tối đa từ nguồn đến bồn. Chúng tôi liên tục chạy BFS trên biểu đồ dư, khôi phục một đường dẫn tăng cường bằng các con trỏ gốc và tăng luồng thêm một. Vì mọi cạnh ban đầu đều có dung lượng bằng một và chúng ta chỉ cần nhiều nhất`a`đơn vị lưu lượng, không lớn hơn`a`sự tăng cường là cần thiết. 
8. So sánh luồng kết quả với`a`. Nếu luồng bằng số lượng robot, hãy in`Yes`. Nếu không thì in`No`. 

### Tại sao nó hoạt động 

Bất biến trung tâm là mọi đường dẫn luồng từ nguồn đến bồn mô tả một quỹ đạo robot hợp pháp. Di chuyển dọc theo cạnh thẳng đứng có nghĩa là tiếp tục theo chiều dọc qua các ô trống liền kề. Di chuyển dọc theo cạnh ngang có nghĩa là tiếp tục theo chiều ngang qua các ô trống liền kề. Chuyển đổi giữa hai lớp có nghĩa là đặt một thiết bị ở góc trong ô đó. Vì mỗi quá trình chuyển đổi như vậy đều có công suất đơn vị nên việc xây dựng không thể chỉ định cùng một nguồn rẽ có hướng hoặc đường thẳng cho hai đường dẫn robot không tương thích. 

Ngược lại, hãy sắp xếp hợp lý các thiết bị quay. Theo dõi từng robot từ điểm xuất phát đến điểm thoát và ghi lại xem nó đang di chuyển theo chiều dọc hay chiều ngang ở mỗi ô. Chuyển động thẳng sẽ trở thành một cạnh bên trong một lớp, trong khi mỗi lượt chuyển động hợp pháp sẽ trở thành một cạnh giữa hai lớp. Quỹ đạo của robot tạo thành các đường dẫn từ nguồn đến bồn. Thuộc tính phân tách hình học ngăn các đường dẫn xung đột yêu cầu cùng một tài nguyên dung lượng đơn vị. Do đó, mọi sự sắp xếp thoát hợp lệ đều mang lại một luồng giá trị`a`và mọi dòng giá trị`a`đưa ra một tập hợp các quỹ đạo robot có thể thực hiện được. Bằng cách mô tả đặc tính luồng cực đại, kiểm tra xem luồng cực đại có`a`đúng là quyết định cần thiết. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline
sys.setrecursionlimit(1_000_000)

def solve_case(n, m, a, b, grid, robots, exits):
    cells = n * m
    source = 2 * cells
    sink = source + 1
    vertex_count = sink + 1

    # Forward-star representation of the residual graph.
    head = [-1] * vertex_count
    to = []
    cap = []
    nxt = []

    def add_edge(u, v, c):
        idx = len(to)
        to.append(v)
        cap.append(c)
        nxt.append(head[u])
        head[u] = idx

        to.append(u)
        cap.append(0)
        nxt.append(head[v])
        head[v] = idx + 1

    def add_bidir(u, v):
        add_edge(u, v, 1)
        add_edge(v, u, 1)

    def hnode(i, j):
        return (i * m + j) * 2

    def vnode(i, j):
        return (i * m + j) * 2 + 1

    # Source to robots and bottom cells to sink.
    for p in robots:
        j = p - 1
        if grid[0][j] == '0':
            add_edge(source, vnode(0, j), 1)

    for e in exits:
        j = e - 1
        if grid[n - 1][j] == '0':
            add_edge(vnode(n - 1, j), sink, 1)

    # Grid graph.
    for i in range(n):
        for j in range(m):
            if grid[i][j] != '0':
                continue

            v = vnode(i, j)
            h = hnode(i, j)

            # A turn in this cell.
            add_bidir(h, v)

            # Continue vertically.
            if i > 0 and grid[i - 1][j] == '0':
                add_bidir(vnode(i - 1, j), v)

            # Continue horizontally.
            if j > 0 and grid[i][j - 1] == '0':
                add_bidir(hnode(i, j - 1), h)

    flow = 0

    # Since all useful capacities are one and a <= 100, repeatedly
    # finding one augmenting path is fast enough.
    while flow < a:
        parent = [-1] * vertex_count
        parent[source] = -2

        q = deque([source])

        while q and parent[sink] == -1:
            u = q.popleft()
            e = head[u]

            while e != -1:
                if cap[e] > 0:
                    v = to[e]
                    if parent[v] == -1:
                        parent[v] = e
                        if v == sink:
                            break
                        q.append(v)
                e = nxt[e]

        if parent[sink] == -1:
            break

        # Every augmenting path carries exactly one unit.
        v = sink
        while v != source:
            e = parent[v]
            cap[e] -= 1
            cap[e ^ 1] += 1
            v = to[e ^ 1]

        flow += 1

    return flow == a

def solve():
    t = int(input())

    answers = []

    for _ in range(t):
        n, m, a, b = map(int, input().split())
        grid = [input().strip() for _ in range(n)]
        robots = list(map(int, input().split()))
        exits = list(map(int, input().split()))

        answers.append(
            "Yes" if solve_case(n, m, a, b, grid, robots, exits)
            else "No"
        )

    sys.stdout.write("\n".join(answers))

if __name__ == "__main__":
    solve()
```Biểu đồ sử dụng hai ID số nguyên cho mỗi ô.`hnode(i, j)`là trạng thái nằm ngang và`vnode(i, j)`là trạng thái thẳng đứng. Giữ hai trạng thái riêng biệt là điều cho phép một ô duy nhất thể hiện cả khả năng chuyển động thẳng và khả năng rẽ. 

các`add_bidir`người trợ giúp xứng đáng được chú ý. Kết nối công suất một chiều hai chiều được triển khai dưới dạng hai cạnh công suất được định hướng độc lập. Mỗi cạnh có hướng cũng nhận được cạnh dư của chính nó. Điều này khác với việc thêm một cặp cạnh dư thông thường, vì sự chuyển động theo cả hai hướng là có thể thực hiện được. 

Nguồn chỉ kết nối với ô hàng đầu tiên trống. Điều này tránh việc vô tình cho phép robot đi vào ô bị chặn. Việc kiểm tra tương tự được thực hiện cho hàng dưới cùng trước khi kết nối lối ra với bồn rửa. 

Việc xây dựng biểu đồ chỉ kiểm tra ô phía trên và ô bên trái. Khi xử lý`(i,j)`, các cạnh tương ứng với các cạnh lân cận đó chưa được thêm vào trước đó, do đó, mỗi phần kề của lưới vô hướng được chèn chính xác một lần.`add_bidir`sau đó tạo ra cả hai hướng. 

Tìm kiếm luồng sử dụng BFS trên các cạnh dư.`parent[v]`lưu trữ cạnh được sử dụng để tiếp cận lần đầu`v`, cho phép xây dựng lại đường dẫn tăng cường hoàn chỉnh từ sink. Vì mỗi lần tăng thêm chính xác một đơn vị và có nhiều nhất`100`robot, vòng lặp sẽ kết thúc sau nhiều nhất`100`nâng cấp thành công. Không thể tràn số nguyên trong Python và trên thực tế, tất cả các khả năng và câu trả lời đều có nhiều nhất`100`. 

Mã sử ​​dụng tọa độ lưới dựa trên số 0 trong nội bộ. Các cột đầu vào dựa trên một cột, vì vậy mọi cột robot hoặc cột thoát đều được chuyển đổi bằng`p - 1`. Đây là chi tiết lập chỉ mục chính có xu hướng gây ra sai sót trong quá trình triển khai này. 

## Ví dụ đã hoạt động 

Mẫu chính thức chứa hai trường hợp thử nghiệm. Viết ở dạng nhiều dòng thông thường là:```
2
3 4 2 2
0000
0011
0000
1 4
2 4
3 4 2 2
0000
0011
0000
3 4
2 4
```Trong trường hợp đầu tiên, hai robot có thể sử dụng các cấu trúc quay riêng biệt. Một tuyến đường bắt đầu trong cột`1`, đi xuống hàng cuối cùng, rẽ phải và đi ra theo lối ra`2`. Cái khác bắt đầu trong cột`4`, rẽ trái ở gần đỉnh, đi xuống qua cột`3`, rẽ phải ở gần phía dưới và đi qua lối ra`4`. 

| Tăng cường | Robot nguồn | Lộ trình đồ thị chính | Dòng chảy | 
| --- | --- | --- | --- | 
| 1 | cột 1 | cột dọc 1 → hàng ngang 3 → cột dọc 2 → chìm | 1 | 
| 2 | cột 4 | cột dọc 4 → hàng ngang 1 → cột dọc 3 → hàng ngang 3 → cột dọc 4 → chìm | 2 | 

Sau lần tăng thứ hai, lưu lượng bằng`a = 2`, do đó thuật toán in`Yes`. Dấu vết này chứng minh tại sao robot có thể quay nhiều lần và tại sao biểu diễn hai lớp lại đủ để mã hóa tất cả bốn hướng của thiết bị. 

Đối với trường hợp thứ hai, robot bắt đầu theo cột`3`Và`4`. Robot từ cột`4`có thể được chuyển hướng tới lối ra`4`, nhưng robot từ cột`3`cũng cần cấu trúc quay thấp tương tự để đạt được lối ra`4`đồng thời tránh các ô bị chặn trong hàng`2`. Mạng còn lại chỉ có thể tìm thấy một đường dẫn từ nguồn đến đích tương thích. 

| Tăng cường | Robot nguồn | Kết quả | Dòng chảy | 
| --- | --- | --- | --- | 
| 1 | một trong các cột 3 hoặc 4 | đạt đến một lối thoát có sẵn | 1 | 
| 2 | robot còn lại | không còn đường dẫn tăng cường nào | 1 | 

Lưu lượng tối đa là`1`, nhỏ hơn`a = 2`, vậy câu trả lời là`No`. Điều này chứng tỏ tại sao việc kiểm tra khả năng tiếp cận riêng biệt cho từng robot là không đủ. Các đường dẫn phải cùng tồn tại trong cùng một ràng buộc về lưới và thiết bị. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(a · nm)`| Đồ thị có`O(nm)`các cạnh và nhiều nhất`a ≤ 100`việc tăng đơn vị được thực hiện. | 
| Không gian |`O(nm)`| có`O(nm)`các đỉnh và các cạnh dư của đồ thị. | 

Vì`n,m ≤ 100`, đồ thị chứa nhiều nhất`20,002`đỉnh. Số cạnh dư cũng tuyến tính theo số lượng ô. Vì nhiều nhất`100`cần tăng cường, việc triển khai đường dẫn tăng cường chuyên dụng chỉ thực hiện một số lượng nhỏ tìm kiếm trên biểu đồ đầy đủ. Điều này hoàn toàn thoải mái trong phạm vi phức tạp dự kiến ​​đối với các ràng buộc của cuộc thi. Danh sách cuộc thi chính thức đưa ra giới hạn thời gian một giây và 1024 MB bộ nhớ, trong khi các giải pháp được chấp nhận sử dụng cùng cấu trúc luồng tối đa hai lớp. 

## Trường hợp thử nghiệm 

Khai thác thử nghiệm sau đây giả định giải pháp đã gửi được lưu dưới dạng`solution.py`và phơi bày`solve()`chức năng hiển thị ở trên.```python
import sys
import io
import solution

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()
        solution.solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Official samples
sample = """\
2
3 4 2 2
0000
0011
0000
1 4
2 4
3 4 2 2
0000
0011
0000
3 4
2 4
"""

assert run(sample) == "Yes\nNo", "official sample"

# Minimum-size open maze.
assert run("""\
1
1 1 1 1
0
1
1
""") == "Yes", "single open cell"

# Minimum-size blocked maze.
assert run("""\
1
1 1 1 1
1
1
1
""") == "No", "single blocked cell"

# One-row maze requiring two turns.
assert run("""\
1
1 2 1 1
00
1
2
""") == "Yes", "horizontal detour in one row"

# Maximum-size empty maze.
grid = "\n".join(["0" * 100 for _ in range(100)])
max_case = (
    "1\n"
    "100 100 100 100\n"
    + grid + "\n"
    + " ".join(map(str, range(1, 101))) + "\n"
    + " ".join(map(str, range(1, 101))) + "\n"
)

assert run(max_case) == "Yes", "maximum all-empty case"

# Boundary case with a blocked bottom exit cell.
assert run("""\
1
2 2 1 1
00
01
1
2
""") == "No", "blocked exit cell"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 × 1`với lưới`0`|`Yes`| Kích thước tối thiểu và lối thoát thẳng đứng | 
|`1 × 1`với lưới`1`|`No`| Ô bắt đầu bị chặn | 
|`1 × 2`với lưới`00`, robot`1`, thoát`2`|`Yes`| Xoay ranh giới và chuyển động một hàng | 
|`100 × 100`tất cả số không với 100 robot và 100 lối thoát |`Yes`| Kích thước tối đa, giá trị luồng tối đa và xây dựng đồ thị lớn | 
|`2 × 2`, lưới`00 / 01`, thoát cột`2`|`No`| Bị chặn lối ra đáy và lập chỉ mục ranh giới | 

## Vỏ cạnh 

Trường hợp ô khởi đầu bị chặn sẽ được xử lý trước khi bất kỳ luồng nào có thể rời khỏi nguồn. Vì```
1
1 1 1 1
1
1
1
```ô lưới duy nhất bị chặn, do đó nguồn không nhận được cạnh nào cả. BFS ngay lập tức phát hiện ra rằng bồn rửa không thể truy cập được, luồng vẫn giữ nguyên`0`, và kết quả là`No`. 

Một ô thoát bị chặn hoạt động đối xứng. Coi như```
1
2 2 1 1
00
01
1
2
```Robot bắt đầu phía trên cột`1`, trong khi lối thoát duy nhất là ở bên dưới cột`2`. Mặc dù hàng đầu tiên mở nhưng ô dưới cùng trong cột`2`bị chặn. Việc thực hiện từ chối thêm cạnh`V(2,2) -> sink`, vì vậy không có đường dẫn nào có thể kết thúc ở lối ra đó. Lưu lượng tối đa là`0`, sản xuất`No`. 

Trường hợp ranh giới một hàng tinh tế hơn:```
1
1 2 1 1
00
1
2
```Nguồn kết nối với trạng thái thẳng đứng của ô`(1,1)`. Lượt đầu tiên sử dụng cạnh giữa`V(1,1)`Và`H(1,1)`, cạnh ngang di chuyển tới`H(1,2)`, và lượt thứ hai chuyển sang`V(1,2)`. Cuối cùng`V(1,2)`kết nối với bồn rửa. Dòng chảy là`1`, vậy câu trả lời là`Yes`. Không có trường hợp đặc biệt nào cho`n = 1`là cần thiết vì biểu diễn biểu đồ đã xử lý nó một cách tự nhiên. 

Trường hợp đường dẫn chung được lấy bởi mẫu thứ hai chính thức:```
1
3 4 2 2
0000
0011
0000
3 4
2 4
```Hai robot có các tuyến đường hợp lý riêng lẻ, nhưng các tuyến đường bắt buộc phải cạnh tranh nhau để giành được cùng một cấu trúc công suất đơn vị. Khi một tuyến đường tiêu thụ tài nguyên đó, nguồn thứ hai không còn đường dẫn nào đến lối ra. Lưu lượng tối đa dừng lại ở`1`, trong khi cần có hai đơn vị, do đó thuật toán trả về`No`. 

Trường hợp hoàn toàn trống có kích thước tối đa kiểm tra thái cực ngược lại:```
100 × 100 grid of zeros
100 robots in columns 1 through 100
100 exits in columns 1 through 100
```Mọi robot có thể tiếp tục đi xuống mà không cần sử dụng thiết bị quay. Đồ thị chứa tất cả`20,000`trạng thái chuyển động, nhưng luồng nguồn-tới-sink có giá trị chính xác`100`. Điều này xác nhận rằng việc triển khai không vô tình yêu cầu rẽ khi đã tồn tại một đường thẳng và biểu đồ vẫn có thể quản lý được ở các kích thước lớn nhất.
