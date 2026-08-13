---
title: "CF 102282E - \u041e \u0434\u0440\u0443\u0436\u0431\u0435"
description: "Đầu vào mô tả một đồ thị vô hướng có các đỉnh là người và các cạnh của nó biểu thị các cặp người là "bạn bè thực sự". Công ty được đảm bảo sẽ được kết nối, nghĩa là đối với mỗi cặp người đều có một chuỗi mối quan hệ bạn bè thực sự giữa họ."
date: "2026-08-13T09:08:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102282
codeforces_index: "E"
codeforces_contest_name: "2011, \u041e\u0442\u0431\u043e\u0440\u043e\u0447\u043d\u044b\u0439 \u043a\u043e\u043d\u0442\u0435\u0441\u0442 \u0421\u0413\u0410\u0423 \u043d\u0430 \u0447\u0435\u0442\u0432\u0435\u0440\u0442\u044c\u0444\u0438\u043d\u0430\u043b ACM ICPC"
rating: 0
weight: 102282
solve_time_s: 69
verified: true
draft: false
---

[CF 102282E - \u041e \u0434\u0440\u0443\u0436\u0431\u0435](https://codeforces.com/problemset/problem/102282/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 9 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Đầu vào mô tả một đồ thị vô hướng có các đỉnh là người và các cạnh của nó biểu thị các cặp người là "bạn bè thực sự". Công ty được đảm bảo sẽ được kết nối, nghĩa là đối với mỗi cặp người đều có một chuỗi mối quan hệ bạn bè thực sự giữa họ. 

Một con người chính là linh hồn của công ty khi loại bỏ đỉnh đó và tất cả các cạnh liên quan của nó khiến đồ thị còn lại bị ngắt kết nối. Trong thuật ngữ đồ thị tiêu chuẩn, một đỉnh như vậy là một điểm khớp nối hoặc đỉnh cắt. 

Chúng ta phải tìm mọi điểm khớp nối của đồ thị được kết nối và in số của chúng theo thứ tự tăng dần. Nếu không có thì dòng đầu tiên là`0`và dòng thứ hai trống. 

Các giới hạn là`n <= 10^4`Và`m <= 10^5`. Một thuật toán tỷ lệ thuận với kích thước đồ thị là dễ dàng phù hợp, trong khi thuật toán lặp lại việc duyệt đồ thị cho mọi đỉnh có khả năng xung quanh`10^4 * 10^5 = 10^9`hoạt động cạnh trong một đầu vào đủ dày đặc. Với giới hạn một giây, cách tiếp cận đó quá chậm. Mục tiêu phải ở gần`O(n + m)`. 

Có một số trường hợp khó xử lý. Hãy xem xét công ty nhỏ nhất có thể:```
2 1
1 2
```Loại bỏ một trong hai người để lại đúng một đỉnh, đỉnh này vẫn là đồ thị liên thông. Đầu ra đúng là:```
0
```Việc triển khai bất cẩn có thể phân loại gốc của DFS của nó như một điểm khớp nối chỉ vì nó có con. Gốc cần ít nhất hai con DFS trước khi nó trở thành điểm khớp nối. 

Một trường hợp quan trọng khác là một ngôi sao:```
5 4
1 2
1 3
1 4
1 5
```Đầu ra đúng là:```
1
1
```Loại bỏ đỉnh`1`để lại bốn đỉnh biệt lập, trong khi loại bỏ bất kỳ chiếc lá nào sẽ để lại một ngôi sao được kết nối. Chỉ kiểm tra xem một đỉnh có lân cận bậc thấp hay không sẽ không giải quyết được vấn đề. 

Trường hợp thứ ba là đồ thị chứa các chu trình:```
4 4
1 2
2 3
3 4
4 1
```Đầu ra đúng là:```
0
```Mỗi đỉnh đều có một lộ trình thay thế xung quanh chu trình. Bản thân cây DFS có thể trông giống như một chuỗi, nhưng cạnh không phải là cây phải được tính đến khi quyết định xem cây con có được tách khỏi phần còn lại của biểu đồ hay không. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp là thử xóa từng người một cách riêng biệt. Đối với mỗi đỉnh ứng cử viên, tạm thời bỏ qua nó, chạy DFS hoặc BFS từ bất kỳ đỉnh nào còn lại và đếm xem vẫn có thể đạt được bao nhiêu đỉnh. Nếu tất cả`n - 1`các đỉnh có thể tiếp cận được, ứng viên không phải là linh hồn. Nếu không thì đó là một điểm khớp nối. 

Điều này đúng vì định nghĩa về linh hồn chính xác là việc loại bỏ con người sẽ ngắt kết nối biểu đồ. Một lần duyệt sẽ kiểm tra khả năng kết nối sau một lần xóa cụ thể, do đó việc thử từng đỉnh sẽ kiểm tra mọi ứng cử viên có thể. 

Vấn đề là công việc lặp đi lặp lại. Chi phí một lần đi qua`O(n + m)`và chúng tôi thực hiện nó trong tối đa`n`đỉnh, cho`O(n(n + m))`. Ở giới hạn tối đa, điều này có thể đạt tới khoảng`10^4 * (10^4 + 10^5) = 1.1 * 10^9`thăm đỉnh và cạnh. Biểu đồ cũng chỉ được lưu trữ nhiều lần về mặt khái niệm, nhưng chỉ riêng chi phí thời gian đã khiến điều này không phù hợp. 

Quan sát quan trọng nhất là tất cả những thử nghiệm xóa bỏ này đều đặt ra những câu hỏi gần như giống nhau. Trong một DFS, hãy xem xét một đỉnh`u`và một trong các cây con DFS của nó có gốc tại`v`. Chúng ta chỉ cần biết liệu cây con đó có cạnh dẫn trở lại`u`hoặc tổ tiên của`u`. Nếu không được thì gỡ bỏ`u`tách toàn bộ cây con khỏi phần còn lại của biểu đồ. 

Điều này có thể được tóm tắt bằng các giá trị DFS tiêu chuẩn`tin[u]`Và`low[u]`.`tin[u]`là thời điểm khi`u`được ghé thăm lần đầu tiên.`low[u]`là thời gian khám phá nhỏ nhất có thể đạt được từ`u`bằng cách đi qua không hoặc nhiều cạnh cây DFS và sau đó sử dụng tối đa một cạnh không phải là cạnh cây. 

Đối với đỉnh không có gốc`u`, một đứa trẻ DFS`v`được tách ra khỏi tổ tiên của`u`chính xác khi nào`low[v] >= tin[u]`. Cây con như vậy không có cạnh nào có khả năng bỏ qua`u`, do đó xóa`u`ngắt kết nối cây con đó. 

Root DFS là một trường hợp đặc biệt. Nó không có tổ tiên nên`low`so sánh không thể hiện được tình trạng của nó. Gốc là một điểm khớp nối chính xác khi nó có ít nhất hai con DFS. Với một con, việc loại bỏ nó sẽ để lại toàn bộ cây DFS được kết nối thông qua một con đó. 

Brute-force hoạt động vì nó kiểm tra biểu đồ một cách rõ ràng sau mỗi lần xóa, nhưng không thành công khi cùng một thông tin kết nối được tính toán lại hàng nghìn lần. các`tin`Và`low`các giá trị cho phép một DFS trả lời đồng thời tất cả các câu hỏi xóa đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(n(n + m))`|`O(n + m)`| Quá chậm | 
| Tối ưu |`O(n + m)`|`O(n + m)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng danh sách kề cho đồ thị vô hướng. Mỗi cạnh đầu vào`(x, y)`được chèn vào cả hai`graph[x]`Và`graph[y]`, bởi vì tình bạn là đối xứng. 
2. Chạy DFS từ đỉnh`1`. Đầu vào đảm bảo rằng biểu đồ được kết nối, do đó một DFS chạm đến mọi đỉnh. Chỉ định thời gian khám phá ngày càng tăng`tin[u]`khi mỗi đỉnh được nhập lần đầu tiên. 
3. Trong khi xử lý một đỉnh`u`, khởi tạo`low[u] = tin[u]`. Điều này có nghĩa là trước khi xem xét bất kỳ cạnh nào khác,`u`có thể tự đến được lúc khám phá`tin[u]`. 
4. Đối với mọi người hàng xóm`v`của`u`, phân biệt các cạnh của cây với các cạnh đã được thăm. Nếu như`v`chưa được truy cập, hãy chạy DFS đệ quy trên`v`. Sau khi đệ quy kết thúc, hãy cập nhật`low[u]`với`low[v]`, bởi vì mọi đỉnh có thể tới được từ cây con của`v`cũng có thể truy cập từ`u`qua rìa cây DFS`u -> v`. 
5. Dành cho mọi trẻ em DFS`v`của một đỉnh không có gốc`u`, kiểm tra xem`low[v] >= tin[u]`. Nếu điều kiện này được giữ, hãy đánh dấu`u`như một điểm khớp nối. Bất đẳng thức nói rằng cây con của`v`không thể tiếp cận bất kỳ tổ tiên chặt chẽ nào của`u`, Vì thế`u`là kết nối duy nhất từ ​​cây con đó đến phần còn lại của cây DFS. 
6. Khi hàng xóm được thăm không phải là cạnh cha trực tiếp, hãy cập nhật`low[u]`sử dụng`tin[v]`. Một cạnh như vậy là cạnh sau của một đỉnh đã được phát hiện và thời gian khám phá của nó mô tả cây con hiện tại có thể thoát ra bao xa. 
7. Đếm số con DFS của mỗi gốc. Nếu gốc có ít nhất hai con, hãy đánh dấu nó là điểm khớp nối. Quy tắc riêng biệt là cần thiết vì gốc không có cha và do đó không có tổ tiên mà con của nó có thể bỏ qua. 
8. Cuối cùng, quét các đỉnh từ`1`bởi vì`n`. In mọi đỉnh được đánh dấu theo thứ tự đó, tự động đưa ra thứ tự tăng dần cần thiết. 

### Tại sao nó hoạt động 

Bất biến trung tâm là sau khi DFS xử lý xong một đỉnh`u`,`low[u]`là thời gian khám phá nhỏ nhất có thể đạt được từ`u`cây con của mà không đi qua cạnh cha của`u`. Hãy xem xét một đứa trẻ DFS`v`của`u`. Nếu như`low[v] < tin[u]`, cây con có gốc tại`v`có một cạnh đạt tới tổ tiên của`u`, do đó xóa`u`không cô lập cây con đó. Nếu như`low[v] >= tin[u]`, không tồn tại đường vòng như vậy nên mọi đường đi từ cây con đó tới các đỉnh bên ngoài nó đều phải đi qua`u`. Như vậy`u`là một điểm khớp nối chính xác khi điều kiện này đúng với một số con, ngoại trừ gốc, trong đó điều kiện đúng là có ít nhất hai con. 

Mỗi cạnh chỉ được kiểm tra thông qua danh sách kề trong DFS đơn này, do đó, cùng một quá trình duyệt đồng thời xác định tất cả các điểm khớp nối. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())

    graph = [[] for _ in range(n + 1)]

    for _ in range(m):
        u, v = map(int, input().split())
        graph[u].append(v)
        graph[v].append(u)

    tin = [0] * (n + 1)
    low = [0] * (n + 1)
    is_cut = [False] * (n + 1)

    timer = 0

    sys.setrecursionlimit(2 * n + 10)

    def dfs(u, parent):
        nonlocal timer

        timer += 1
        tin[u] = timer
        low[u] = timer

        children = 0

        for v in graph[u]:
            if v == parent:
                continue

            if tin[v] == 0:
                children += 1
                dfs(v, u)

                low[u] = min(low[u], low[v])

                if parent != 0 and low[v] >= tin[u]:
                    is_cut[u] = True
            else:
                low[u] = min(low[u], tin[v])

        if parent == 0 and children >= 2:
            is_cut[u] = True

    dfs(1, 0)

    answer = [u for u in range(1, n + 1) if is_cut[u]]

    print(len(answer))
    print(*answer)

if __name__ == "__main__":
    solve()
```Danh sách kề sử dụng chỉ số`1`bởi vì`n`, khớp với cách đánh số trong đầu vào. Chức vụ`0`không được sử dụng và cũng được sử dụng làm giá trị gốc đặc biệt cho gốc DFS. 

các`tin`mảng tăng gấp đôi như một mảng đã truy cập. Giá trị bằng 0 nghĩa là đỉnh đó chưa được khám phá. Sau khi được phân công,`tin[v]`là thời gian khám phá DFS cố định của nó, chính xác là giá trị cần thiết khi xử lý cạnh sau. 

Khi`v`là một hàng xóm không được thăm viếng, nó trở thành một đứa trẻ DFS. Cuộc gọi đệ quy phải kết thúc trước khi cập nhật`low[u]`, bởi vì`low[v]`chỉ được biết sau toàn bộ cây con của`v`đã được xử lý. 

Đối với một người hàng xóm đã ghé thăm, mã sử dụng`tin[v]`còn hơn là`low[v]`. Cạnh`(u, v)`là một cạnh trực tiếp không phải cây, do đó nó cung cấp một tuyến đường cụ thể đến đỉnh đã được phát hiện`v`; cho phép`low[v]`ở đây sẽ bao gồm thông tin không chính xác từ cây con khác. 

các`v == parent`kiểm tra cũng là cần thiết. Trong danh sách kề vô hướng, cạnh cây từ`u`đối với cha mẹ của nó xuất hiện như là hàng xóm của`u`. Coi cạnh đó là cạnh sau sẽ giảm không chính xác`low[u]`. 

Đệ quy Python là mối quan tâm triển khai chính. Một biểu đồ đường dẫn có thể chứa`10^4`các đỉnh, do đó độ sâu đệ quy DFS có thể đạt tới`10^4`. Việc tăng giới hạn đệ quy sẽ tránh được hạn chế về độ sâu đệ quy mặc định của Python. Không có vấn đề tràn số nguyên vì thời gian khám phá tối đa là`n`. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đồ thị đầu tiên gồm hai hình tam giác có chung đỉnh`5`. DFS có thể tạo thành cây`1 -> 3 -> 5 -> 2 -> 4`. Các cạnh phụ tạo ra các cạnh sau từ cây con xung quanh`5`quay lại phía sau`1`. 

Một dấu vết nhỏ gọn của các trạng thái DFS quan trọng là: 

| Hành động DFS |`u`|`tin[u]`|`low[u]`sau khi xử lý | Phụ huynh | Trạng thái cắt | 
| --- | --- | --- | --- | --- | --- | 
| Đi vào`1`| 1 | 1 | 1 | 0 | sai | 
| Đi vào`3`| 3 | 2 | 1 | 1 | sai | 
| Đi vào`5`| 5 | 3 | 1 | 3 | sai | 
| Đi vào`2`| 2 | 4 | 1 | 5 | sai | 
| Đi vào`4`| 4 | 5 | 1 | 2 | sai | 
| Hoàn thành`4`| 4 | 5 | 1 | 2 | sai | 
| Hoàn thành`2`| 2 | 4 | 1 | 5 | sai | 
| Hoàn thành`5`| 5 | 3 | 1 | 3 | đúng | 
| Hoàn thành`3`| 3 | 2 | 1 | 1 | sai | 
| Hoàn thành`1`| 1 | 1 | 1 | 0 | sai | 

Khi đỉnh`5`được xử lý, cây con của nó có thể đạt tới đỉnh`1`qua rìa`5-1`, Vì thế`low[5] = 1`. Tuy nhiên, đối với con`2`, cây con của nó không thể tới được tổ tiên của`5`, Và`low[2] = 1`vẫn lớn hơn hoặc bằng`tin[5] = 3`chỉ khi cấu trúc DFS thực sự cô lập nó. Trong biểu đồ này, vì`4-5`là một cạnh sau, thứ tự truyền tải chính xác rất quan trọng và điểm khớp nối thu được là`5`. 

Đầu ra là:```
1
5
```Ví dụ này chứng minh tại sao các chu trình phải được biểu diễn thông qua`low`các giá trị. Đang xóa`5`phá hủy kết nối chung của cả hai tam giác, trong khi loại bỏ bất kỳ đỉnh nào khác sẽ khiến biểu đồ được kết nối. 

### Mẫu 2 

Đồ thị thứ hai chứa một chu trình bao gồm các đỉnh`1, 2, 3, 4`, một chu trình khác bao gồm`4, 5, 6, 7`và một phần giống như đường dẫn được gắn qua`9`: đỉnh`4-9`,`9-8`, Và`9-10`. 

Cấu trúc khớp nối dễ nhìn thấy hơn từ các điều kiện DFS: 

| Đỉnh | Cây con con có liên quan |`tin[u]`| Đứa trẻ`low`| Điều kiện khớp nối | 
| --- | --- | --- | --- | --- | 
| 1 | cây con qua 2 | 1 | 1 | sai | 
| 2 | cây con đến 4 | 2 | 1 | sai | 
| 3 | cây con đến 4 | 3 | 1 | sai | 
| 4 | cây con đến 5 | 4 | 4 | đúng | 
| 4 | cây con đến 9 | 4 | 9 | đúng | 
| 9 | cây con đến 8 hoặc 10 | 9 | 10 | đúng | 

Chu kỳ đi qua`1, 2, 3, 4`cung cấp cho cây con DFS một cách để trở về tổ tiên của`4`, do đó các đỉnh đó không trở thành điểm khớp nối chỉ vì chúng là các đỉnh DFS bên trong. đỉnh`4`tuy nhiên, kết nối chu kỳ đầu tiên, chu kỳ thứ hai và nhánh chứa`9`. 

đỉnh`9`có cùng vai trò cấu trúc bên trong nhánh của nó. Đang xóa`9`ngăn cách các đỉnh`8`Và`10`từ phần còn lại của công ty. 

Đầu ra là:```
2
4 9
```Ví dụ này chứng tỏ rằng một số điểm khớp nối có thể xuất hiện trong một biểu đồ và chúng không cần phải liền kề nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n + m)`| Mỗi đỉnh được nhập một lần và mọi cạnh vô hướng được kiểm tra một số lần không đổi. | 
| Không gian |`O(n + m)`| Danh sách kề lưu trữ cả hai hướng của mọi cạnh, trong khi mảng DFS sử dụng`O(n)`không gian. | 

Với`n <= 10^4`Và`m <= 10^5`, thuật toán chỉ thực hiện một vài lần vượt qua`2m`các mục lân cận. Mức tiêu thụ bộ nhớ cũng thoải mái trong vòng 128 MB vì ​​biểu đồ và bốn`O(n)`mảng phụ trợ là cấu trúc quan trọng duy nhất. 

## Trường hợp thử nghiệm 

Các thử nghiệm sau đây sử dụng một phiên bản của giải pháp được hiển thị dưới dạng một hàm để mỗi đầu vào có thể được thực thi độc lập. Thử nghiệm kích thước tối đa sử dụng một chuỗi có`10^4`các đỉnh, buộc độ sâu DFS và thực hiện việc xử lý giới hạn đệ quy.```python
import sys
import io

def solve():
    input = sys.stdin.readline

    n, m = map(int, input().split())
    graph = [[] for _ in range(n + 1)]

    for _ in range(m):
        u, v = map(int, input().split())
        graph[u].append(v)
        graph[v].append(u)

    tin = [0] * (n + 1)
    low = [0] * (n + 1)
    is_cut = [False] * (n + 1)

    timer = 0
    sys.setrecursionlimit(2 * n + 10)

    def dfs(u, parent):
        nonlocal timer

        timer += 1
        tin[u] = timer
        low[u] = timer

        children = 0

        for v in graph[u]:
            if v == parent:
                continue

            if tin[v] == 0:
                children += 1
                dfs(v, u)
                low[u] = min(low[u], low[v])

                if parent != 0 and low[v] >= tin[u]:
                    is_cut[u] = True
            else:
                low[u] = min(low[u], tin[v])

        if parent == 0 and children >= 2:
            is_cut[u] = True

    dfs(1, 0)

    answer = [v for v in range(1, n + 1) if is_cut[v]]

    return str(len(answer)) + "\n" + " ".join(map(str, answer)) + "\n"

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    try:
        return solve()
    finally:
        sys.stdin = old_stdin

sample1 = """\
5 6
1 3
1 5
2 4
2 5
3 5
4 5
"""

sample2 = """\
10 11
1 2
1 3
2 4
3 4
4 5
4 6
5 7
6 7
4 9
8 9
9 10
"""

assert run(sample1) == "1\n5\n", "sample 1"
assert run(sample2) == "2\n4 9\n", "sample 2"

assert run("""\
2 1
1 2
""") == "0\n\n", "minimum graph has no articulation point"

assert run("""\
5 4
1 2
1 3
1 4
1 5
""") == "1\n1\n", "star center"

assert run("""\
4 6
1 2
1 3
1 4
2 3
2 4
3 4
""") == "0\n\n", "complete graph"

n = 10000
chain = [f"{n} {n - 1}"]
chain.extend(f"{i} {i + 1}" for i in range(1, n))
chain_input = "\n".join(chain) + "\n"

expected_vertices = list(range(2, n))
expected_max = "9998\n" + " ".join(map(str, expected_vertices)) + "\n"

assert run(chain_input) == expected_max, "maximum-size chain"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 1`có cạnh`1 2`|`0`| tối thiểu`n`, root với chính xác một con DFS | 
| Ngôi sao tập trung ở`1`|`1`, sau đó`1`| Quy tắc phát âm gốc | 
| Biểu đồ hoàn chỉnh trên`4`đỉnh |`0`| Nhiều cạnh sau và không có điểm khớp nối | 
| Chuỗi của`10000`đỉnh |`9998`, sau đó`2 3 ... 9999`| Kích thước tối đa, DFS sâu, đỉnh biên, điều kiện không phải gốc | 

## Vỏ cạnh 

Đối với đồ thị tối thiểu```
2 1
1 2
```DFS bắt đầu lúc`1`, khám phá`2`, và trả về. Gốc có đúng một con nên điều kiện gốc`children >= 2`là sai. đỉnh`2`cũng không phải là đỉnh bị cắt vì khi xóa nó chỉ để lại đỉnh`1`. Câu trả lời là`0`. 

Đối với ngôi sao```
5 4
1 2
1 3
1 4
1 5
```đỉnh`1`là gốc DFS và có bốn con. Điều kiện gốc ngay lập tức đánh dấu nó là điểm khớp nối. Mỗi lá không có con DFS nào, vì vậy không lá nào có thể thỏa mãn điều kiện không phải gốc. Kết quả chính xác là đỉnh`1`. 

Đối với biểu đồ hoàn chỉnh```
4 6
1 2
1 3
1 4
2 3
2 4
3 4
```Mỗi cây con DFS đều có cạnh sau của đỉnh trước đó. Do đó, đối với mọi ứng cử viên không phải là người gốc`u`, mỗi đứa trẻ có liên quan có`low[child] < tin[u]`. Không có đỉnh không phải gốc nào được đánh dấu và gốc chỉ có một con DFS vì các đỉnh còn lại đều đạt được thông qua con đầu tiên đó. Câu trả lời là`0`. 

Đối với chuỗi có kích thước tối đa, biểu đồ là```
10000 9999
1 2
2 3
3 4
...
9999 10000
```Không có cạnh sau nên với mọi đỉnh trong`u`, con của nó thỏa mãn`low[child] = tin[child]`, lớn hơn`tin[u]`. Mỗi đỉnh từ`2`bởi vì`9999`do đó là một điểm khớp nối. Đỉnh`1`Và`10000`thì không, vì việc xóa một trong hai điểm cuối sẽ để lại một chuỗi được kết nối. Việc triển khai làm tăng giới hạn đệ quy của Python vì độ sâu DFS đạt tới`10000`và lần quét cuối cùng sẽ phát ra các đỉnh theo thứ tự tăng dần.
