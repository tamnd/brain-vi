---
title: "CF 102428I - Cải thiện SPAM"
description: "Hãy coi mỗi danh sách gửi thư như một đỉnh trong biểu đồ có hướng. Khi danh sách gửi thư i chứa danh sách gửi thư j, hãy vẽ một cạnh từ i đến j. Email của khách hàng là đỉnh cuối cùng."
date: "2026-08-12T07:20:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102428
codeforces_index: "I"
codeforces_contest_name: "2019-2020 ACM-ICPC Latin American Regional Programming Contest"
rating: 0
weight: 102428
solve_time_s: 107
verified: true
draft: false
---

[CF 102428I - Cải thiện SPAM](https://codeforces.com/problemset/problem/102428/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Hãy coi mỗi danh sách gửi thư như một đỉnh trong biểu đồ có hướng. Khi gửi danh sách gửi thư`i`chứa danh sách gửi thư`j`, vẽ một cạnh từ`i`ĐẾN`j`. Email của khách hàng là đỉnh cuối cùng. Gửi tin nhắn đến danh sách gửi thư có nghĩa là phải tuân theo mọi lựa chọn gửi đi, do đó có thể tiếp cận cùng một khách hàng bằng nhiều con đường khác nhau. 

Quy tắc tạo mang lại cho biểu đồ này một thuộc tính quan trọng: danh sách gửi thư tạo thành DAG. Một danh sách chỉ có thể được chèn vào các danh sách đã tồn tại và các danh sách được tạo mỗi lần một danh sách, do đó, các cạnh của danh sách trong danh sách luôn di chuyển tiến hoặc lùi trong thời gian tạo mà không bao giờ quay lại danh sách đã tồn tại. Địa chỉ số của danh sách không biểu thị thời gian tạo danh sách, vì vậy chúng ta không thể giả định rằng một cạnh đi từ chỉ mục nhỏ hơn đến chỉ mục lớn hơn. 

Câu trả lời đầu tiên,`B`, tính riêng từng lần giao hàng. Nếu có thể tiếp cận một khách hàng thông qua ba con đường khác nhau thì khách hàng đó sẽ đóng góp ba con đường vào`B`. Câu trả lời thứ hai,`A`, chỉ tính mỗi khách hàng có thể tiếp cận một lần. Như vậy`A`chỉ đơn giản là số lượng địa chỉ khách hàng riêng biệt có thể truy cập được từ danh sách gửi thư`1`. 

Có nhiều nhất`1000`danh sách gửi thư và`2000`tổng số địa chỉ Một danh sách có thể chứa hầu hết mọi địa chỉ, vì vậy tổng số mục thành viên có thể lên tới`L(N-1)`, chỉ dưới hai triệu. Điều đó loại trừ các thuật toán liên tục mở rộng các danh sách con giống nhau hoặc liệt kê tất cả các đường dẫn có thể. Thuật toán đồ thị tuyến tính hoặc gần tuyến tính phù hợp với các giới hạn này. 

Có một số trường hợp dễ xảy ra khi việc thực hiện bất cẩn sẽ dẫn đến kết quả sai. Đầu tiên, cùng một khách hàng có thể được tiếp cận thông qua hai chi nhánh khác nhau. Ví dụ,```
4 3
2 2 3
1 4
1 4
```Ở đây danh sách gửi thư`1`tiếp cận khách hàng`4`thông qua cả hai danh sách`2`Và`3`. Hệ thống thông thường gửi hai tin nhắn, trong khi hệ thống cải tiến gửi một tin nhắn, vì vậy câu trả lời là`2 1`. Một giải pháp chỉ đếm các địa chỉ riêng biệt xuất hiện trực tiếp trong danh sách`1`bỏ lỡ bản sao. 

Cái bẫy thứ hai là một khách hàng có thể xảy ra cả trực tiếp lẫn thông qua một danh sách khác:```
4 2
2 2 3
1 3
```Danh sách`1`chứa danh sách`2`và khách hàng`3`, trong khi danh sách`2`cũng chứa khách hàng`3`. Số lượng giao hàng thông thường là`2`, nhưng chỉ tồn tại một khách hàng riêng biệt, nên câu trả lời là`2 1`. Giải pháp chỉ loại bỏ các mục trùng lặp trong mỗi danh sách riêng lẻ vẫn bị tính quá mức. 

Bẫy thứ ba là giả sử các chỉ mục danh sách mô tả thứ tự tạo. Coi như```
3 2
1 2
1 3
```Danh sách`1`chứa danh sách`2`, và liệt kê`2`chứa khách hàng`3`. Câu trả lời đúng là`1 1`. Một DP xử lý danh sách theo thứ tự số sẽ cố gắng tính toán danh sách`1`trước danh sách`2`, mặc dù danh sách`1`phụ thuộc vào nó. Thay vào đó, đồ thị phải được sắp xếp theo thứ tự tôpô. 

## Phương pháp tiếp cận 

Mô phỏng lực lượng vũ phu trực tiếp tuân theo hệ thống phân cấp danh sách gửi thư chính xác như hệ thống thực. Bất cứ khi nào nó gặp một khách hàng, nó sẽ tăng số lượng gửi và bất cứ khi nào nó gặp một danh sách gửi thư khác, nó sẽ xử lý đệ quy danh sách đó. Điều này đúng vì mỗi lần xuất hiện của danh sách gửi thư đều thể hiện một lần truyền tải độc lập khác, chính xác như trong hệ thống ban đầu. 

Vấn đề là cùng một đồ thị con có thể được mở rộng nhiều lần. Xem xét các danh sách gửi thư được sắp xếp dưới dạng một DAG hoàn chỉnh, với danh sách`1`chỉ vào mọi danh sách sau này, danh sách`2`chỉ vào mọi danh sách sau này, v.v. Đặt một email khách hàng vào danh sách cuối cùng. Mỗi tập hợp con của danh sách trung gian đưa ra một đường dẫn khác với danh sách`1`tới email cuối cùng đó. Với`L`danh sách, điều này tạo ra`2^(L-2)`giao hàng cho cùng một khách hàng. Tại`L = 1000`, đó là`2^998`thăm lá đệ quy, vượt xa bất cứ điều gì có thể được thực thi. 

Quan sát khắc phục điều này là biểu đồ là DAG, do đó, hiệu quả của mọi danh sách gửi thư có thể được tính toán một lần và được sử dụng lại. Định nghĩa`ways[i]`như số lượng giao hàng của khách hàng được tạo ra khi gửi danh sách gửi thư`i`được xử lý. Đối với khách hàng trực tiếp trong danh sách, chúng tôi sẽ thêm một khách hàng. Đối với một danh sách gửi thư khác`j`, chúng tôi thêm`ways[j]`. Khi tất cả trẻ em đã được tính toán,`ways[i]`được biết đến bất kể cuối cùng có bao nhiêu con đường khác nhau đi tới`i`. 

Câu trả lời không trùng lặp thậm chí còn đơn giản hơn. Chúng tôi không cần phải xây dựng tập hợp khách hàng có thể tiếp cận cho mọi danh sách. Bắt đầu từ danh sách`1`, chạy duyệt đồ thị và đánh dấu mọi địa chỉ ngay khi gặp nó. Danh sách gửi thư chỉ được xử lý khi nó được tiếp cận lần đầu tiên và khách hàng chỉ được tính khi nó được tiếp cận lần đầu tiên. Điều này trực tiếp mô hình hóa hành vi được cải thiện. 

Chi tiết cấu trúc duy nhất chúng ta cần trước DP là thứ tự tôpô của biểu đồ danh sách gửi thư. Thuật toán Kahn đưa ra mệnh lệnh từ cha mẹ đối với con cái. Đảo ngược thứ tự đó đảm bảo rằng mọi danh sách được tham chiếu bởi`i`đã được đánh giá khi chúng tôi tính toán`ways[i]`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(số lượng đường dẫn mở rộng) | Độ sâu đệ quy O(L) | Quá chậm | 
| Tối ưu | O(N + E) | O(N + E) | Đã chấp nhận | 

Đây`E`biểu thị tổng số mục thành viên. Độ phức tạp brute-force không phải là đa thức vì số lượng đường dẫn có thể là hàm mũ trong`L`. 

## Hướng dẫn thuật toán 

1. Đọc mọi danh sách gửi thư và lưu trữ các thành viên trong đó. Đối với mỗi thành viên là một danh sách gửi thư khác, hãy thêm một cạnh được định hướng từ danh sách hiện tại vào thành viên đó và tăng mức độ của thành viên đó. Việc giữ các danh sách thành viên đầy đủ là rất hữu ích vì sau này cả DP và truyền tải không trùng lặp sẽ sử dụng cùng một cách biểu diễn giống nhau. 
2. Chạy phương pháp sắp xếp tôpô của Kahn trên biểu đồ danh sách gửi thư. Bắt đầu với mọi danh sách có mức độ bằng 0, liên tục loại bỏ một danh sách như vậy và giảm mức độ của các danh sách con của nó. Quy tắc tạo đảm bảo rằng biểu đồ có tính tuần hoàn, do đó mọi danh sách gửi thư cuối cùng đều xuất hiện theo thứ tự. 
3. Đảo ngược thứ tự tôpô. Nếu như`i -> j`có nghĩa là danh sách đó`i`chứa danh sách`j`, sau đó`j`phải được đánh giá trước`i`. Thứ tự đảo ngược đưa ra chính xác hướng phụ thuộc đó. 
4. Xử lý danh sách gửi thư theo thứ tự đảo ngược và tính toán`ways[i]`. Đối với mọi khách hàng trực tiếp trong danh sách`i`, thêm vào`1`. Đối với mỗi danh sách gửi thư`j`trong danh sách`i`, thêm vào`ways[j]`. Lấy kết quả modulo`10^9 + 7`. Vì mọi danh sách tham chiếu đều đã được xử lý nên không cần mở rộng đệ quy. 
5. Bắt đầu từ danh sách gửi thư`1`, thực hiện DFS hoặc BFS trên tất cả các địa chỉ. Duy trì một`seen`mảng bao gồm cả danh sách gửi thư và địa chỉ khách hàng. Khi tiếp cận được một khách hàng chưa từng thấy trước đó, hãy đánh dấu nó và tăng dần`A`. Khi đạt đến danh sách gửi thư chưa từng thấy trước đó, hãy đánh dấu nó và đưa nó vào hàng đợi truyền tải. 
6. Đầu ra`ways[1]`và số lượng khách hàng riêng biệt mà quá trình truyền tải đạt được. Giá trị đầu tiên đếm từng đường dẫn riêng biệt, trong khi giá trị thứ hai đếm từng địa chỉ nhiều nhất một lần. 

### Tại sao nó hoạt động 

Đối với câu trả lời đầu tiên, bất biến là sau khi xử lý danh sách`i`,`ways[i]`bằng chính xác số lượng đường dẫn phân phối máy khách bắt đầu từ`i`. Mỗi khách hàng trực tiếp đóng góp một đường dẫn và mỗi danh sách gửi thư được chứa sẽ đóng góp tất cả các đường dẫn bắt đầu từ danh sách con đó. Vì thứ tự tôpô đảo ngược xử lý mọi phần tử con trước tiên nên phép lặp tính toán cho mọi lần phân phối có thể xảy ra chính xác một lần. 

Đối với câu trả lời thứ hai, điều bất biến là một địa chỉ được xử lý nhiều nhất một lần. Mọi khách hàng có thể truy cập từ danh sách`1`cuối cùng sẽ gặp phải quá trình truyền tải, vì vậy mỗi khách hàng có thể tiếp cận đều đóng góp chính xác một cho`A`. Nếu có nhiều đường dẫn đến cùng một máy khách,`seen`mục nhập đã được đặt khi các đường dẫn sau gặp nó, do đó không có bản sao nào được tính. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 1_000_000_007

def solve():
    input = sys.stdin.readline

    n, l = map(int, input().split())

    members = [[] for _ in range(l)]
    graph = [[] for _ in range(l)]
    indegree = [0] * l

    for i in range(l):
        data = list(map(int, input().split()))
        k = data[0]
        cur = [x - 1 for x in data[1:k + 1]]
        members[i] = cur

        for x in cur:
            if x < l:
                graph[i].append(x)
                indegree[x] += 1

    # Topological order of the mailing-list DAG.
    queue = []
    head = 0

    for i in range(l):
        if indegree[i] == 0:
            queue.append(i)

    topo = []

    while head < len(queue):
        u = queue[head]
        head += 1
        topo.append(u)

        for v in graph[u]:
            indegree[v] -= 1
            if indegree[v] == 0:
                queue.append(v)

    # Children must be evaluated before parents.
    ways = [0] * l

    for u in reversed(topo):
        total = 0

        for x in members[u]:
            if x < l:
                total += ways[x]
            else:
                total += 1

        ways[u] = total % MOD

    # Count distinct client emails reachable from list 1.
    seen = [False] * n
    seen[0] = True

    queue = [0]
    head = 0
    distinct_clients = 0

    while head < len(queue):
        u = queue[head]
        head += 1

        for x in members[u]:
            if seen[x]:
                continue

            seen[x] = True

            if x < l:
                queue.append(x)
            else:
                distinct_clients += 1

    return f"{ways[0]} {distinct_clients}"

if __name__ == "__main__":
    print(solve())
```Đầu vào được lưu trữ trong`members`, sử dụng địa chỉ dựa trên số không. Một giá trị nhỏ hơn`l`là một danh sách gửi thư, trong khi ít nhất một giá trị`l`là một địa chỉ khách hàng. Đây là lý do tại sao điều kiện là`x < l`, còn hơn là`x <= l`. 

các`graph`mảng chỉ chứa các cạnh danh sách. Nó tách biệt với`members`bởi vì việc sắp xếp cấu trúc liên kết chỉ cần các cạnh đó, trong khi DP cần phân biệt cả thành viên danh sách và thành viên khách hàng. 

Mảng vô cấp thuộc thuật toán Kahn. Sau khi thứ tự tôpô được xây dựng, nó không còn cần thiết nữa nên bộ nhớ tương tự không bị trùng lặp. 

DP lặp đi lặp lại`reversed(topo)`. Hướng này rất dễ bị sai. Nếu danh sách`u`chứa danh sách`v`, đồ thị chứa`u -> v`, do đó trật tự tôpô thông thường đặt`u`trước`v`. DP cần thứ tự ngược lại, với`v`trước`u`. 

Số nguyên trong Python không bị tràn nhưng phép toán modulo vẫn cần thiết vì đáp án bắt buộc là modulo`10^9 + 7`. Modulo được thực hiện một lần sau khi tính toán từng danh sách, điều này là đủ vì tổng trung gian bị giới hạn bởi số lần thành viên`MOD`. 

Để truyền tải không trùng lặp, một`seen`mảng là đủ cho cả hai loại địa chỉ. Danh sách gửi thư đã xem sẽ không bao giờ được mở rộng nữa và một khách hàng đã xem sẽ không bao giờ được tính lại. Đây chính xác là sự khác biệt giữa các đường đếm cho`B`và đếm các đỉnh có thể tiếp cận được cho`A`. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Các cạnh của danh sách gửi thư là`1 -> 2`,`1 -> 3`, Và`3 -> 2`. Thuật toán Kahn có thể tạo ra thứ tự`1, 3, 2`, do đó quá trình DP`2, 3, 1`. 

| Danh sách | Thành viên |`ways`sau khi xử lý | Khách hàng có thể tiếp cận | 
| --- | --- | --- | --- | 
| 2 | 4, 5 | 2 | 4, 5 | 
| 3 | 4, 2 | 3 | 4, 5 | 
| 1 | 2, 3 | 5 | 4, 5 | 

Danh sách`2`gửi trực tiếp cho khách hàng`4`Và`5`, cho`ways[2] = 2`. Danh sách`3`gửi trực tiếp đến`4`và sau đó xử lý danh sách`2`, Vì thế`ways[3] = 1 + 2 = 3`. Cuối cùng, liệt kê`1`xử lý cả hai danh sách, đưa ra`2 + 3 = 5`. 

Việc truyền tải khả năng tiếp cận nhìn thấy khách hàng`4`Và`5`mỗi người một lần. Khách hàng`4`được gặp qua cả hai danh sách`2`Và`3`, Nhưng`seen[4]`ngăn chặn cuộc gặp gỡ thứ hai làm tăng câu trả lời. Kết quả là`5 2`. 

### Mẫu 2 

Các cạnh của danh sách gửi thư có liên quan là`1 -> 6`,`3 -> 6`,`3 -> 4`,`3 -> 5`,`5 -> 4`,`6 -> 5`, Và`6 -> 4`. Một trật tự tôpô hợp lệ là`1, 2, 3, 6, 5, 4`, do đó DP sử dụng thứ tự ngược lại. 

| Danh sách | Các thành viên liên quan đến DP |`ways`| 
| --- | --- | --- | 
| 4 | 14, 15 | 2 | 
| 5 | 6? không, thực ra là 4, 14 | 3 | 
| 6 | 5, 4 | 5 | 
| 3 | 6, 14, 4, 5, 15 | 12 | 
| 2 | 10, 11, 12, 13, 9, 7, 8 | 7 | 
| 1 | 6 | 5 | 

Danh sách`4`trực tiếp chứa khách hàng`14`Và`15`, vì vậy nó tạo ra hai thông báo. Danh sách`5`chứa danh sách`4`và khách hàng`14`, sản xuất`2 + 1 = 3`. Danh sách`6`chứa danh sách`5`Và`4`, sản xuất`3 + 2 = 5`. Kể từ danh sách`1`chỉ chứa danh sách`6`,`B = 5`. 

Đối với hệ thống được cải tiến, duyệt từ danh sách`1`đạt danh sách`6`, sau đó liệt kê`5`Và`4`và cuối cùng chỉ có khách hàng`14`Và`15`. Cả hai đường dẫn đến những khách hàng đó đều thu gọn thành một lần gửi cho mỗi địa chỉ, mang lại`A = 2`. 

## Phân tích độ phức tạp 

hãy để`E`là tổng số mục thành viên trên tất cả các danh sách gửi thư. 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N + E) | Phân loại cấu trúc liên kết, DP và khả năng truyền tải khả năng tiếp cận đều kiểm tra mọi thành viên được lưu trữ với số lần không đổi | 
| Không gian | O(N + E) | Danh sách thành viên, biểu đồ danh sách gửi thư, mức độ, giá trị DP, hàng đợi và mảng đã truy cập đều tuyến tính ở kích thước đầu vào | 

Tối đa có thể`E`ở dưới`L(N-1)`, dưới hai triệu theo các ràng buộc nhất định. Mỗi thành viên chỉ được kiểm tra một số lần không đổi, do đó thuật toán vẫn nằm trong phạm vi dự định ngay cả đối với cấu trúc danh sách gửi thư dày đặc. 

## Trường hợp thử nghiệm```python
import sys
import io

MOD = 1_000_000_007

def solve():
    input = sys.stdin.readline

    n, l = map(int, input().split())

    members = [[] for _ in range(l)]
    graph = [[] for _ in range(l)]
    indegree = [0] * l

    for i in range(l):
        data = list(map(int, input().split()))
        k = data[0]
        cur = [x - 1 for x in data[1:k + 1]]
        members[i] = cur

        for x in cur:
            if x < l:
                graph[i].append(x)
                indegree[x] += 1

    queue = []
    head = 0

    for i in range(l):
        if indegree[i] == 0:
            queue.append(i)

    topo = []

    while head < len(queue):
        u = queue[head]
        head += 1
        topo.append(u)

        for v in graph[u]:
            indegree[v] -= 1
            if indegree[v] == 0:
                queue.append(v)

    ways = [0] * l

    for u in reversed(topo):
        total = 0
        for x in members[u]:
            if x < l:
                total += ways[x]
            else:
                total += 1
        ways[u] = total % MOD

    seen = [False] * n
    seen[0] = True

    queue = [0]
    head = 0
    distinct_clients = 0

    while head < len(queue):
        u = queue[head]
        head += 1

        for x in members[u]:
            if seen[x]:
                continue

            seen[x] = True

            if x < l:
                queue.append(x)
            else:
                distinct_clients += 1

    return f"{ways[0]} {distinct_clients}"

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    try:
        return solve()
    finally:
        sys.stdin = old_stdin

sample1 = """\
5 3
2 2 3
2 4 5
2 4 2
"""

sample2 = """\
15 6
1 6
7 10 11 12 13 9 7 8
5 6 14 4 5 15
2 14 15
2 4 14
2 5 4
"""

sample3 = """\
10 5
4 8 9 10 3
3 9 10 6
3 8 9 7
6 2 3 6 7 8 10
5 9 10 3 1 7
"""

assert run(sample1) == "5 2", "sample 1"
assert run(sample2) == "5 2", "sample 2"
assert run(sample3) == "6 4", "sample 3"

# Minimum-size instance.
assert run("""\
2 1
1 2
""") == "1 1", "minimum-size case"

# Same client reached through two different lists.
assert run("""\
4 3
2 2 3
1 4
1 4
""") == "2 1", "duplicate through two branches"

# Same client appears directly and through a nested list.
assert run("""\
4 2
2 2 3
1 3
""") == "2 1", "direct plus indirect duplicate"

# Many paths to the same client.
assert run("""\
8 4
4 2 3 4 8
3 3 4 8
2 4 8
1 8
""") == "8 1", "exponential path structure"

# Maximum N and maximum L, with the boundary client N appearing.
n = 2000
l = 1000
lines = [f"{n} {l}"]
for i in range(l):
    lines.append(f"1 {l + i + 1}")

max_case = "\n".join(lines) + "\n"
assert run(max_case) == "1 1", "maximum-size boundary case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 1 / 1 2`|`1 1`| Giá trị tối thiểu và địa chỉ khách hàng đầu tiên | 
|`4 3 / 2 2 3 / 1 4 / 1 4`|`2 1`| Cùng một khách hàng được tiếp cận thông qua các chi nhánh riêng biệt | 
|`4 2 / 2 2 3 / 1 3`|`2 1`| Một khách hàng đã tiếp cận cả trực tiếp và gián tiếp | 
|`8 4 / 4 2 3 4 8 / 3 3 4 8 / 2 4 8 / 1 8`|`8 1`| Nhiều đường dẫn riêng biệt kết thúc tại một khách hàng | 
|`N=2000, L=1000`, mỗi danh sách chứa ứng dụng khách ranh giới riêng của nó |`1 1`| Kích thước và địa chỉ tối đa`N`| 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là khả năng tiếp cận trùng lặp thông qua các nhánh khác nhau. Vì```
4 3
2 2 3
1 4
1 4
```DP topo tính toán`ways[2] = 1`Và`ways[3] = 1`, Vì thế`ways[1] = 2`. Trong quá trình duyệt khả năng tiếp cận, hãy liệt kê`2`khám phá khách hàng`4`Đầu tiên. Khi danh sách`3`được xử lý,`seen[4]`đã đúng nên khách hàng không được tính lại. Đầu ra là`2 1`. 

Trường hợp cạnh thứ hai là trùng lặp do tư cách thành viên trực tiếp và lồng nhau gây ra. Vì```
4 2
2 2 3
1 3
```danh sách`2`thực hiện một lần giao hàng cho khách hàng`3`. Danh sách`1`cũng chứa khách hàng`3`trực tiếp, vì vậy`ways[1] = 2`. Việc đi qua đạt`3`trực tiếp từ danh sách`1`và sau đó gặp lại nó thông qua danh sách`2`, nhưng lần xuất hiện thứ hai bị bỏ qua. Đầu ra là`2 1`. 

Trường hợp cạnh thứ ba là sự không khớp giữa thứ tự địa chỉ số và thứ tự phụ thuộc:```
3 2
1 2
1 3
```Đồ thị chứa`1 -> 2`, và liệt kê`2`chứa khách hàng`3`. Danh sách địa điểm theo thuật toán của Kahn`1`trước danh sách`2`, sau đó thứ tự đảo ngược đánh giá danh sách`2`Đầu tiên. Như vậy`ways[2] = 1`, theo sau là`ways[1] = 1`. Việc duyệt từ danh sách`1`cũng đến tay khách hàng`3`, cho`1 1`. DP chỉ mục số sẽ thất bại vì nó cố gắng sử dụng`ways[2]`trước khi tính toán nó. 

Trường hợp cạnh cấu trúc cuối cùng là một biểu đồ có nhiều đường dẫn theo cấp số nhân đến cùng một máy khách:```
8 4
4 2 3 4 8
3 3 4 8
2 4 8
1 8
```Có tám đường dẫn khác nhau từ danh sách`1`cho khách hàng`8`, Vì thế`B = 8`. Việc truyền tải được cải thiện nhìn thấy khách hàng`8`một lần và cho`A = 1`. DP xử lý tất cả tám đường dẫn bằng cách tính toán từng danh sách một lần, thay vì duyệt qua tám đường dẫn đó một cách rõ ràng. Đây là thuộc tính làm cho thuật toán mở rộng đến trường hợp tối đa lớn hơn nhiều.
