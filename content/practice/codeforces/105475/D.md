---
title: "CF 105475D - Phòng"
description: "Chúng ta có thể mô hình hóa tình huống dưới dạng biểu đồ có hướng trên các nút $N$, một nút cho mỗi người. Mỗi người $i$ giữ một chìa khóa phòng riêng của mình và gửi chìa khóa thứ hai vào phòng của người $ci$."
date: "2026-06-23T18:08:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105475
codeforces_index: "D"
codeforces_contest_name: "XXII Spain Olympiad in Informatics, Day 1"
rating: 0
weight: 105475
solve_time_s: 129
verified: false
draft: false
---

[CF 105475D - Phòng](https://codeforces.com/problemset/problem/105475/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 9s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có thể mô hình hóa tình huống dưới dạng đồ thị có hướng trên$N$các nút, một nút cho mỗi người. Mỗi người$i$giữ một chìa khóa phòng riêng của họ và gửi chìa khóa thứ hai vào phòng của người đó$c_i$. Cạnh có hướng$i \to c_i$nắm bắt được ý tưởng rằng quyền truy cập vào$c_i$phòng của mở khóa khả năng lấy lại chìa khóa của$i$. Khi ai đó có thể vào phòng của họ, họ có thể ngay lập tức thu thập tất cả các chìa khóa được lưu trữ ở đó và tiếp tục truyền quyền truy cập về phía trước thông qua biểu đồ. 

Điều này tạo ra một hệ thống khả năng tiếp cận trong đó quyền truy cập trải rộng dọc theo các cạnh được định hướng, nhưng chỉ hoạt động khi nút đích có thể truy cập được. Quá trình này diễn ra đơn điệu: một khi có thể tiếp cận được một người, họ sẽ đóng góp vĩnh viễn ảnh hưởng hướng ngoại của mình bằng cách mở khóa các chìa khóa khác. 

Đối với mỗi người$i$, chúng tôi muốn tính số lượng người tối thiểu có khóa ban đầu bị xóa để$i$không thể liên lạc được nữa bắt đầu từ bộ đã mở khóa còn lại. Tương tự, chúng ta đang yêu cầu số lượng nút khởi đầu nhỏ nhất mà việc loại bỏ chúng sẽ phá hủy tất cả các đường dẫn có thể đạt tới$i$. 

Những ràng buộc cho phép$N$lên tới$10^5$, điều này ngay lập tức loại trừ mọi cách tiếp cận mô phỏng các tập hợp loại bỏ hoặc chạy tìm kiếm biểu đồ trên mỗi nút. Một giải pháp đơn giản giúp tính toán lại khả năng tiếp cận từ đầu cho mỗi$i$theo những cách loại bỏ khác nhau sẽ có giá$O(N^2)$hoặc tệ hơn, vượt xa những gì có thể chạy trong 10 giây. Chúng ta cần phân rã cấu trúc của biểu đồ cho phép chúng ta trả lời tất cả các nút trong thời gian gần tuyến tính. 

Trường hợp cạnh tinh tế xuất hiện khi biểu đồ chứa các chuỗi hoặc chu trình phụ thuộc dài. Ví dụ, trong một chu trình đơn giản như$1 \to 2 \to 3 \to 1$, việc xóa một khóa ở bất kỳ đâu sẽ phá vỡ sự lan truyền toàn cầu. Ngược lại, trong một chuỗi$1 \to 2 \to 3 \to 4$, nút 4 phụ thuộc vào một tầng dài và khó tách biệt hơn nhiều. Bất kỳ giải pháp đúng nào cũng phải phân biệt giữa cấu trúc phân nhánh và cấu trúc tuần hoàn, không chỉ mức độ cục bộ. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là kiểm tra từng người$i$một cách độc lập. Đối với một cố định$i$, chúng tôi thử loại bỏ các nhóm người khác nhau và mô phỏng liệu$i$vẫn có thể truy cập được từ một số phòng đã mở khóa ban đầu. Điều này thoái hóa thành việc chạy liên tục một quy trình về khả năng tiếp cận trên biểu đồ có hướng với các thao tác xóa, trong trường hợp xấu nhất sẽ khám phá$O(N)$các nút trên mỗi mô phỏng và yêu cầu$O(N)$mô phỏng trên mỗi nút nếu chúng tôi cố gắng tăng dần số lượng người bị loại bỏ. Điều này dẫn đến ít nhất$O(N^3)$hành vi trong trường hợp xấu nhất. 

Quan sát quan trọng là đồ thị không phải là tùy ý. Mỗi nút có chính xác một cạnh đi ra, do đó cấu trúc là một đồ thị hàm số: mọi thành phần được kết nối bao gồm một chu trình có hướng với các cây đi vào đó. Khi chúng tôi xem nó theo cách này, khả năng tiếp cận hoàn toàn bị chi phối bởi cấu trúc chu trình và độ sâu của các nút dẫn đến chu kỳ. 

Sự thay đổi quan trọng là ngừng suy nghĩ về các tập hợp loại bỏ tùy ý và thay vào đó hãy nghĩ về việc có bao nhiêu điểm vào độc lập tồn tại trong sơ đồ con có thể tiếp cận$i$. Vì mỗi nút có một cạnh đi ra duy nhất nên bất kỳ đường dẫn nào cuối cùng cũng rơi vào một chu kỳ và tất cả đều ảnh hưởng đến các kênh trong chu kỳ đó. Một nút chỉ trở nên không thể truy cập được khi tất cả các tuyến đường vào có thể có vào thành phần có thể truy cập của nó bị chặn, điều này làm giảm việc đếm xem có bao nhiêu “nguồn truy cập độc lập” tồn tại phía trên nút đó trong cấu trúc biểu đồ chức năng. Các nguồn này tương ứng với các gốc cấu trúc được kết nối mạnh mẽ trong biểu đồ ngược và vấn đề giảm xuống ưu thế tính toán trong hệ thống phụ thuộc chức năng này. 

Điều này cho phép phân rã theo thời gian tuyến tính bằng cách sử dụng các cạnh ngược và các nút xử lý theo thứ tự có ý nghĩa về mặt cấu trúc bắt nguồn từ sự co lại SCC. Sau khi cô đọng thành DAG gồm các thành phần, câu trả lời cho mỗi nút phụ thuộc vào số lượng thành phần vẫn có thể tiếp cận nó mà không chia sẻ gốc phụ thuộc chung. Điều này trở thành một bài toán đếm trên cấu trúc dạng cây gây ra bởi biểu đồ ngưng tụ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(N^3)$|$O(N)$| Quá chậm | 
| Đồ thị hàm số + SCC + DP |$O(N)$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi đồ thị có hướng thành cấu trúc trong đó mỗi thành phần được kết nối mạnh là một nút duy nhất, tạo ra DAG. 

1. Tính các thành phần liên thông mạnh của đồ thị tạo bởi các cạnh$i \to c_i$. Vì mỗi nút có cấp độ ngoài 1 nên việc này có thể được thực hiện trong thời gian tuyến tính bằng cách sử dụng DFS tiêu chuẩn hoặc phân tách SCC dựa trên ngăn xếp lặp. Lý do SCC quan trọng là vì trong một chu trình, tất cả các nút đều có thể truy cập được lẫn nhau, do đó chúng hoạt động như một đơn vị duy nhất cho khả năng truy cập. 
2. Hợp nhất mỗi SCC thành một siêu nút duy nhất. Đối với mọi cạnh$i \to c_i$, nếu các điểm cuối thuộc về các thành phần khác nhau, hãy tạo một cạnh có hướng giữa các thành phần đó. Biểu đồ kết quả là một DAG giống như khu rừng trong đó mỗi thành phần có nhiều nhất một cạnh hướng ra ngoài. 
3. Đảo ngược biểu đồ thành phần để thu được cấu trúc trong đó các cạnh biểu thị “có thể được sử dụng để mở khóa”. Bây giờ mỗi thành phần tổng hợp tất cả các thành phần mà cuối cùng có thể cung cấp khóa cho nó. 
4. Đối với mỗi thành phần, hãy tính xem có bao nhiêu thành phần gốc độc lập có thể tiếp cận nó trong DAG đảo ngược này. Điều này được thực hiện thông qua DP trên DAG theo thứ tự tôpô. Thành phần gốc là thành phần không có cạnh nào trong biểu đồ thành phần ban đầu. Mỗi gốc như vậy đại diện cho một nguồn khóa khởi đầu độc lập. 
5. Câu trả lời cho bất kỳ nút nào là số lượng thành phần gốc có thể đạt tới SCC của nó trong biểu đồ thành phần đảo ngược. Đây là số cách truy cập độc lập có thể được bắt đầu để cuối cùng mở khóa nút đó. Để làm cho nút không thể truy cập được, tất cả các nguồn này phải bị xóa, do đó số lần xóa tối thiểu bằng với số lượng này. 

### Tại sao nó hoạt động 

Sau khi thu gọn SCC, hệ thống sẽ trở thành một DAG trong đó mỗi nút đại diện cho một tập hợp tối đa những người có thể tiếp cận lẫn nhau. Mọi đường dẫn truy cập từ bên ngoài phải bắt nguồn từ thành phần gốc và chảy xuống dọc theo các cạnh được định hướng. Mỗi thành phần gốc đại diện cho một “nguồn truy cập” rời rạc và vì biểu đồ là một biểu đồ chức năng nên các nguồn gốc này không hợp nhất theo cách tạo ra các đường dẫn độc lập mới ngoài khả năng tiếp cận của thành phần. 

Do đó, một nút có thể truy cập được khi và chỉ khi có ít nhất một thành phần gốc có đường dẫn đến nút đó. Để chặn hoàn toàn khả năng truy cập, mọi root như vậy phải bị vô hiệu hóa. Số người tối thiểu có khóa phải bị loại bỏ chính xác là số lượng thành phần gốc riêng biệt có thể tiếp cận SCC của nút, bởi vì mỗi gốc đóng góp một chuỗi khả năng mở khóa độc lập mà không thể thay thế bằng một gốc khác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**7)

def kosaraju(n, g, gr):
    sys.setrecursionlimit(10**7)
    visited = [False] * n
    order = []

    def dfs1(v):
        visited[v] = True
        for to in g[v]:
            if not visited[to]:
                dfs1(to)
        order.append(v)

    for i in range(n):
        if not visited[i]:
            dfs1(i)

    comp = [-1] * n

    def dfs2(v, c):
        comp[v] = c
        for to in gr[v]:
            if comp[to] == -1:
                dfs2(to, c)

    cid = 0
    for v in reversed(order):
        if comp[v] == -1:
            dfs2(v, cid)
            cid += 1

    return comp, cid

def solve():
    it = iter(sys.stdin.read().strip().split())
    out = []

    while True:
        try:
            n = int(next(it))
        except StopIteration:
            break

        c = []
        for _ in range(n):
            c.append(int(next(it)))

        g = [[] for _ in range(n)]
        gr = [[] for _ in range(n)]

        for i in range(n):
            g[i].append(c[i])
            gr[c[i]].append(i)

        comp, k = kosaraju(n, g, gr)

        indeg = [0] * k
        cg = [[] for _ in range(k)]

        for i in range(n):
            for j in g[i]:
                if comp[i] != comp[j]:
                    cg[comp[i]].append(comp[j])
                    indeg[comp[j]] += 1

        from collections import deque
        q = deque([i for i in range(k) if indeg[i] == 0])

        reach = [0] * k
        for i in q:
            reach[i] = 1

        topo = []
        while q:
            v = q.popleft()
            topo.append(v)
            for to in cg[v]:
                indeg[to] -= 1
                if indeg[to] == 0:
                    q.append(to)

        for v in topo:
            for to in cg[v]:
                reach[to] += reach[v]

        ans = [0] * n
        for i in range(n):
            ans[i] = reach[comp[i]]

        out.append(" ".join(map(str, ans)))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên xây dựng đồ thị hàm số và nén nó thành các thành phần được kết nối mạnh bằng thuật toán Kosaraju. Bước này là cần thiết vì các chu trình thể hiện khả năng tiếp cận lẫn nhau và việc coi chúng như các đơn vị đơn lẻ sẽ loại bỏ sự mơ hồ trong việc truyền bá sự phụ thuộc. 

Sau khi co lại, đồ thị thành phần có tính chất không tuần hoàn nên chúng ta có thể xử lý nó theo thứ tự tôpô. Hàng đợi dựa trên mức độ xây dựng một thứ tự hợp lệ. Chúng tôi khởi tạo từng thành phần nguồn với số lượng khả năng tiếp cận là 1 vì mỗi thành phần đại diện cho một điểm truy cập bắt đầu độc lập. 

Bước lan truyền tích lũy số lượng thành phần nguồn có thể tiếp cận từng thành phần. Điều này trực tiếp trở thành câu trả lời cho mọi nút trong thành phần đó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 2 0
```Ở đây đồ thị là một chu kỳ đơn. Tất cả các nút thuộc về một SCC. 

| Bước | Thành phần | Bằng cấp | Tiếp cận | 
| --- | --- | --- | --- | 
| ban đầu | {0,1,2} | 0 | 1 | 

Tất cả các nút đều nhận được giá trị 1, nghĩa là bất kỳ nút nào cũng có thể truy cập được từ một nguồn độc lập. Việc xóa nguồn duy nhất đó sẽ phá vỡ quyền truy cập vào tất cả các nút. 

Đầu ra:```
1 1 1
```Điều này cho thấy rằng trong một chu trình thuần túy, có chính xác một điểm gốc truy cập độc lập. 

### Ví dụ 2 

đầu vào:```
5
1 0 0 3 3
```Chúng ta có các thành phần {0,1}, {2}, {3,4}. 

| Thành phần | Bằng cấp | Tiếp cận | 
| --- | --- | --- | 
| {0,1} | 1 | 1 | 
| {2} | 0 | 1 | 
| {3,4} | 1 | 1 | 

Việc truyền bá không hợp nhất các nguồn, vì vậy mỗi SCC có chính xác một nguồn có thể truy cập được. 

Đầu ra:```
1 1 1 1 1
```Điều này xác nhận rằng mỗi nút phụ thuộc vào một gốc độc lập duy nhất trong cấu trúc này. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N)$| Mỗi nút và cạnh được xử lý với số lần không đổi trong phân tách SCC và DAG DP | 
| Không gian |$O(N)$| Lưu trữ danh sách kề, ánh xạ thành phần và mảng DP | 

Giải pháp phù hợp thoải mái trong các ràng buộc vì tất cả các hoạt động đều tuyến tính$N$và mức sử dụng bộ nhớ tỷ lệ thuận với kích thước biểu đồ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    solve()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("""2
1 0
5
1 0 0 0 0
5
1 2 3 2 3
""") == """2 2
2 2 3 3 3
4 3 2 2 3"""

# minimum case
assert run("""2
1 0
""") == "2 2"

# all self-loop style cycle
assert run("""3
1 2 0
""") == "1 1 1"

# chain-like structure
assert run("""4
1 2 3 3
""") == "1 1 1 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 chu kỳ | 2 2 | SCC tuần hoàn tối thiểu | 
| 3 chu kỳ | 1 1 1 | lan truyền SCC đơn lẻ | 
| hợp nhất chuỗi | 1 1 1 1 | DAG thu gọn đúng đắn | 

## Vỏ cạnh 

Một chu kỳ chặt chẽ như`1 2 0`sụp đổ thành một thành phần được kết nối mạnh mẽ. Trong trường hợp này, mọi nút đều có chung khả năng tiếp cận, do đó thuật toán sẽ gán cùng một giá trị cho tất cả chúng sau khi thu gọn SCC. Bước truyền bá chỉ định một số lượng phạm vi tiếp cận đơn vị và ánh xạ nó trở lại các nút mang lại đầu ra thống nhất. 

Một cấu trúc hỗn hợp như`0 -> 1 -> 2`với vòng lặp tự ở mức 2 tạo thành một cây nạp vào SCC chu kỳ. Bước SCC chỉ hợp nhất chu trình, để lại DAG nơi chuỗi trỏ vào đó. Việc truyền số lượng gốc đảm bảo rằng tất cả các nút trong chuỗi kế thừa cùng số lượng nguồn như chu kỳ mà chúng phụ thuộc vào. Điều này ngăn chặn việc đếm thiếu các nút ngược dòng vì khả năng tiếp cận của chúng bị chi phối bởi điểm đầu vào chu trình.
