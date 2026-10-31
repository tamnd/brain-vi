---
title: "CF 106386H - ​​Trượt băng nghệ thuật đỉnh cao"
description: "Chúng ta được cung cấp một đồ thị có hướng với các nút có trọng số. Mỗi nút đại diện cho một phần tử trượt băng và mỗi phần tử có một điểm. Cạnh có hướng từ $x$ đến $y$ có nghĩa là bạn được phép thực hiện phần tử $y$ ngay sau phần tử $x$."
date: "2026-06-25T10:15:00+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106386
codeforces_index: "H"
codeforces_contest_name: "UTPC Contest 2-25-26 (Advanced)"
rating: 0
weight: 106386
solve_time_s: 43
verified: true
draft: false
---

[CF 106386H - Trượt băng nghệ thuật đỉnh cao](https://codeforces.com/problemset/problem/106386/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một đồ thị có hướng với các nút có trọng số. Mỗi nút đại diện cho một phần tử trượt băng và mỗi phần tử có một điểm. Một cạnh định hướng từ$x$ĐẾN$y$có nghĩa là bạn được phép thực hiện phần tử$y$ngay sau phần tử$x$. Hiệu suất là một bước đi trên biểu đồ này trong đó mỗi bước đi theo một cạnh được định hướng và bước đi có thể bắt đầu tại bất kỳ nút nào. 

Điểm mấu chốt là việc lặp lại cùng một yếu tố sẽ không cộng thêm bất kỳ điểm nào sau lần xuất hiện đầu tiên của nó. Vì vậy, nếu chúng ta truy cập một nút nhiều lần thì chỉ lần truy cập đầu tiên mới đóng góp giá trị của nút đó; tất cả các lần truy cập tiếp theo đều không đóng góp. 

Nhiệm vụ là chọn một bước đi hợp lệ để tối đa hóa tổng các giá trị nút riêng biệt gặp phải dọc theo bước đi. 

Đồ thị có thể lớn, lên tới$10^5$nút và$2 \cdot 10^5$các cạnh. Bất kỳ giải pháp nào cố gắng liệt kê các đường đi hoặc thực hiện khám phá theo cấp số nhân qua các lần đi bộ sẽ ngay lập tức thất bại. Thậm chí$O(n^2)$các phương pháp tiếp cận quá lớn trong trường hợp xấu nhất vì biểu đồ có thể đủ dày đặc để các mô hình truyền tải lặp lại nhiều. 

Một khó khăn nhỏ là các chu kỳ được cho phép và trên thực tế, chúng là cách duy nhất để “xem lại” các nút. Tuy nhiên, việc xem lại một nút chỉ hữu ích nếu nó giúp tiếp cận các nút mới chưa được thu thập. Khi tất cả các nút có giá trị có thể truy cập được trong một khu vực đã được thu thập, việc tiếp tục lặp sẽ không mang lại lợi ích gì. 

Một sai lầm dễ mắc phải là cho rằng đây là vấn đề về đường đi dài nhất hoặc vấn đề về DAG DP. Điều đó không thành công vì chu kỳ được cho phép và hữu ích. Một lỗi phổ biến khác là nén các thành phần được kết nối mạnh nhưng sau đó xử lý biểu đồ kết quả không chính xác, đặc biệt nếu người ta quên rằng trong SCC, tất cả các nút đều có thể truy cập lẫn nhau và có thể được thu thập theo bất kỳ thứ tự nào mà không cần phải xem xét lại bất kỳ chi phí nào. 

Ví dụ: nếu chúng ta có một chu trình$1 \to 2 \to 3 \to 1$, hành vi đúng là bắt đầu từ bất kỳ đâu trong chu trình này cho phép thu thập cả ba giá trị. Một đường đi đơn giản DP có thể chỉ tính một hướng truyền tải và bỏ lỡ rằng toàn bộ thành phần thực sự có thể truyền tải tự do. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là mô phỏng tất cả các bước đi có thể bắt đầu từ mọi nút, duy trì một tập hợp các nút đã truy cập có giá trị đã được thu thập. Ở mỗi bước, chúng tôi di chuyển dọc theo bất kỳ cạnh đi nào và thêm giá trị của nút nếu nó chưa được nhìn thấy trước đó. Điều này đúng vì nó khớp trực tiếp với định nghĩa của quy trình. 

Điểm thất bại là không gian trạng thái. Chỉ riêng tập đã truy cập đã có kích thước$2^n$, và đối với mỗi tiểu bang có tới$n$những bước đi tiếp theo có thể xảy ra. Ngay cả khi giới hạn ở các trạng thái có thể truy cập, số lượng đường dẫn riêng biệt trong biểu đồ có chu kỳ là không bị giới hạn, vì các bước đi có thể lặp lại tùy ý nhiều lần trước khi quyết định tiếp tục. Điều này làm cho việc mô phỏng trực tiếp là không thể. 

Quan sát quan trọng là các chu kỳ bên trong biểu đồ không quan trọng trong nội bộ. Sau khi nhập một thành phần được kết nối mạnh, bạn có thể tự do di chuyển trong đó và thu thập mọi nút trong thành phần đó mà không cần lo lắng về thứ tự. Cấu trúc có ý nghĩa duy nhất là cách các thành phần này kết nối với nhau. 

Điều này làm giảm vấn đề về một đồ thị chu trình có hướng của các thành phần liên thông mạnh. Bên trong mỗi thành phần, chúng ta có thể tính tổng tất cả các giá trị nút, vì khi nhập nó, chúng ta có thể thu thập mọi thứ trong đó. Giữa các thành phần, chúng tôi bị hạn chế bởi DAG ngưng tụ, vì vậy chúng tôi đang chọn một đường dẫn trong DAG này một cách hiệu quả để tối đa hóa tổng trọng số thành phần. 

Điều này biến bài toán thành bài toán đường đi dài nhất trên DAG, trong đó trọng số nút là tổng của các giá trị SCC. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force qua các cuộc đi bộ | Hàm mũ | Hàm mũ | Quá chậm | 
| Nén SCC + DP trên DAG |$O(n + m)$|$O(n + m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính các thành phần liên thông mạnh của đồ thị. Mỗi nút được gán một id thành phần. Bước này nhóm các nút có thể truy cập được lẫn nhau, điều này quan trọng vì bên trong một nhóm như vậy, các ràng buộc về thứ tự không còn hạn chế tổng số bộ sưu tập. 
2. Đối với mỗi thành phần, hãy tính tổng giá trị của nó bằng cách tính tổng trọng số của tất cả các nút bên trong nó. Điều này hợp lệ vì khi bước đi đi vào thành phần, nó có thể đi ngang qua bên trong và cuối cùng truy cập tất cả các nút. 
3. Xây dựng biểu đồ thu gọn trong đó mỗi SCC trở thành một nút duy nhất. Đối với mọi cạnh ban đầu$u \to v$, nếu các thành phần khác nhau, hãy thêm một cạnh có hướng giữa các đại diện thành phần của chúng. 
4. Vì quá trình ngưng tụ SCC luôn tạo ra đồ thị không tuần hoàn có hướng, hãy tính thứ tự tôpô của các thành phần. 
5. Chạy lập trình động trên DAG. Cho phép$dp[c]$là tổng giá trị tối đa có thể đạt được kết thúc tại thành phần$c$. Khởi tạo tất cả$dp[c]$bằng tổng của chính thành phần đó, vì chúng ta có thể bắt đầu ở bất kỳ đâu. 
6. Duyệt các thành phần theo thứ tự tôpô. Đối với mỗi cạnh$c \to d$, cập nhật$dp[d]$BẰNG$dp[d] = \max(dp[d], dp[c] + value[d])$. Quá trình chuyển đổi này thể hiện việc di chuyển từ một vùng gồm các nút có thể truy cập lẫn nhau sang vùng khác và thu thập tất cả các nút trong thành phần đích khi đến. 
7. Câu trả lời là giá trị lớn nhất trong số tất cả các trạng thái dp. 

Tính chính xác dựa trên thực tế là khi một thành phần được nhập vào, tất cả các nút của nó có thể được thu thập mà không bị phạt và việc rời khỏi nó không thể cung cấp bất kỳ giá trị bổ sung nào ngoại trừ thông qua các cạnh đi ra. Vì việc thu hẹp SCC loại bỏ tất cả các ràng buộc thứ tự nội bộ nên mọi bước đi hợp lệ đều tương ứng chính xác với một đường dẫn trong DAG. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

def solve():
    n, m = map(int, input().split())
    val = list(map(int, input().split()))
    
    g = [[] for _ in range(n)]
    gr = [[] for _ in range(n)]
    
    for _ in range(m):
        x, y = map(int, input().split())
        x -= 1
        y -= 1
        g[x].append(y)
        gr[y].append(x)

    # Kosaraju: first pass order
    vis = [False] * n
    order = []

    def dfs1(u):
        vis[u] = True
        for v in g[u]:
            if not vis[v]:
                dfs1(v)
        order.append(u)

    for i in range(n):
        if not vis[i]:
            dfs1(i)

    # second pass: assign components
    comp = [-1] * n
    comps = []

    def dfs2(u, idx):
        comp[u] = idx
        comps[idx].append(u)
        for v in gr[u]:
            if comp[v] == -1:
                dfs2(v, idx)

    for u in reversed(order):
        if comp[u] == -1:
            comps.append([])
            dfs2(u, len(comps) - 1)

    k = len(comps)

    comp_sum = [0] * k
    for c in range(k):
        s = 0
        for u in comps[c]:
            s += val[u]
        comp_sum[c] = s

    dag = [set() for _ in range(k)]
    indeg = [0] * k

    for u in range(n):
        for v in g[u]:
            cu, cv = comp[u], comp[v]
            if cu != cv and cv not in dag[cu]:
                dag[cu].add(cv)
                indeg[cv] += 1

    topo = []
    stack = [i for i in range(k) if indeg[i] == 0]
    while stack:
        u = stack.pop()
        topo.append(u)
        for v in dag[u]:
            indeg[v] -= 1
            if indeg[v] == 0:
                stack.append(v)

    dp = comp_sum[:]

    for u in topo:
        for v in dag[u]:
            if dp[v] < dp[u] + comp_sum[v]:
                dp[v] = dp[u] + comp_sum[v]

    print(max(dp))

if __name__ == "__main__":
    solve()
```Nửa đầu của mã thực hiện thuật toán Kosaraju để xác định các thành phần được kết nối mạnh mẽ. DFS chuyển tiếp tạo ra thứ tự hoàn thiện, trong khi DFS đồ thị ngược chỉ định các thành phần. Điều này là cần thiết vì tính đối xứng về khả năng tiếp cận bên trong SCC là yếu tố cho phép thu gọn chu kỳ một cách an toàn. 

Nửa sau xây dựng đồ thị ngưng tụ. Một bộ được sử dụng cho mỗi thành phần để tránh các cạnh trùng lặp, vì nhiều cạnh gốc có thể kết nối cùng một cặp thành phần. Nếu không loại bỏ trùng lặp, quá trình chuyển đổi cấp độ và DP vẫn sẽ hoạt động nhưng sẽ lãng phí thời gian và có nguy cơ cập nhật dư thừa. 

Giai đoạn DP là quá trình tính toán đường đi dài nhất trên DAG, được khởi tạo bằng tổng thành phần vì mỗi thành phần đều có khả năng tự cung cấp làm điểm bắt đầu. Quá trình chuyển đổi sẽ thêm giá trị thành phần đích đầy đủ vì việc nhập nó cho phép thu thập đầy đủ. 

## Ví dụ đã hoạt động 

Hãy xem xét một biểu đồ nhỏ có chu trình và đuôi: 

đầu vào:```
5 5
1 10 100 1 50
1 2
2 3
3 1
3 4
4 5
```Phân tách SCC mang lại một thành phần$\{1,2,3\}$với tổng 111, và đơn vị 4 và 5. 

| Bước | Thành phần | dp trước | Chuyển tiếp | dp sau | 
| --- | --- | --- | --- | --- | 
| ban đầu | {1,2,3} | 111 | bắt đầu | 111 | 
| quá trình | 4 | 0 | 111 + 1 | 112 | 
| quá trình | 5 | 0 | 112 + 50 | 162 | 

Điều này cho thấy rằng khi chu trình được tham gia, tất cả các nút của nó sẽ được thu thập ngay lập tức và chỉ có cấu trúc bên ngoài mới quan trọng. 

Bây giờ hãy xem xét một biểu đồ phân nhánh: 

đầu vào:```
4 3
5 1 10 2
1 2
1 3
3 4
```Mỗi nút là SCC riêng của nó. 

| Bước | Nút | dp trước | Chuyển tiếp | dp sau | 
| --- | --- | --- | --- | --- | 
| bắt đầu | 1 | 5 | ban đầu | 5 | 
| 1 → 2 | 2 | 1 | 5 + 1 | 6 | 
| 1 → 3 | 3 | 10 | 5 + 10 | 15 | 
| 3 → 4 | 4 | 2 | 15 + 2 | 17 | 

Đường dẫn tốt nhất là 1 → 3 → 4, xác nhận rằng DP đang chọn tuyến đường tốt nhất trong DAG thay vì chỉ nút cao nhất ngay lập tức. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n + m)$| Kosaraju chạy theo thời gian tuyến tính, ngưng tụ SCC và DP cũng xử lý mỗi cạnh một lần | 
| Không gian |$O(n + m)$| danh sách kề, đồ thị ngược, bộ lưu trữ SCC và mảng DP | 

Các ràng buộc cho phép lên đến$3 \cdot 10^5$tổng kích thước đồ thị, do đó việc phân tách SCC theo thời gian tuyến tính và DAG DP vừa vặn thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    return sys.stdout.getvalue()

# NOTE: In actual use, run() should call solve() and capture output.
# Here we only illustrate structure.

# sample-style and custom tests would be placed when integrated
```Một dây nịt thích hợp sẽ chuyển hướng thiết bị xuất chuẩn và gọi trực tiếp cho người giải quyết; cấu trúc ở trên là một trình giữ chỗ phù hợp với các thiết lập thử nghiệm CF điển hình. 

| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đồ thị chu trình đơn | tổng của tất cả các nút | SCC sụp đổ đúng đắn | 
| chuỗi DAG | tính tổng dọc theo con đường tốt nhất | Độ chính xác DP trên DAG | 
| nhiều chi nhánh | lựa chọn đường dẫn tối đa | đúng tối ưu toàn cục | 

## Vỏ cạnh 

Đồ thị tuần hoàn đầy đủ là trường hợp góc quan trọng nhất. Nếu tất cả các nút đều có thể truy cập được lẫn nhau thì thuật toán sẽ nén mọi thứ vào một SCC và trả về tổng của tất cả các giá trị. Bất kỳ DP dựa trên đường dẫn nào không nén SCC sẽ coi lượt truy cập lại là không hợp lệ hoặc tính gấp đôi không đúng cách. 

Một trường hợp cạnh khác là biểu đồ có nhiều thành phần bị ngắt kết nối. Mỗi thành phần trở thành SCC của riêng nó và vì DP khởi tạo từng thành phần một cách độc lập nên thuật toán cho phép bắt đầu chính xác ở bất cứ đâu mà không cần các nút nguồn nhân tạo. 

Trường hợp khó phát hiện cuối cùng là khi đường đi tốt nhất đi vào một chu kỳ muộn và sau đó thoát ra qua một nút khác. Sự co lại của SCC đảm bảo rằng việc tham gia chu trình sẽ ngay lập tức cấp tất cả các giá trị, do đó không có lợi ích gì trong việc mô phỏng truyền tải một phần; DP tránh được việc đếm thiếu một cách chính xác bằng cách xử lý nguyên tử toàn bộ thành phần.
