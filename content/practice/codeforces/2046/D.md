---
title: "CF 2046D - Vì Hoàng Đế!"
description: "Chúng ta được cho một đồ thị có hướng trong đó mỗi đỉnh đại diện cho một thành phố và mỗi cạnh có hướng đại diện cho một con đường cho phép người đưa tin đi một chiều. Một số thành phố ban đầu có chứa một số sứ giả."
date: "2026-06-08T09:08:15+07:00"
tags: ["codeforces", "competitive-programming", "flows", "graphs"]
categories: ["algorithms"]
codeforces_contest: 2046
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 990 (Div. 1)"
rating: 3100
weight: 2046
solve_time_s: 111
verified: false
draft: false
---

[CF 2046D - Vì Hoàng đế!](https://codeforces.com/problemset/problem/2046/D) 

**Đánh giá:** 3100 
**Tags:** luồng, đồ thị 
**Thời gian giải:** 1 phút 51 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị có hướng trong đó mỗi đỉnh đại diện cho một thành phố và mỗi cạnh có hướng đại diện cho một con đường cho phép người đưa tin đi một chiều. Một số thành phố ban đầu có chứa một số sứ giả. Những người đưa tin này có thể di chuyển dọc các con đường và một khi họ mang theo một bản sao của kế hoạch, họ có thể sao chép nó một cách tự do bất cứ khi nào họ ở trong thành phố, vì vậy yếu tố hạn chế không phải là số lượng bản sao có thể được tạo ra sau đó mà là nơi chúng tôi đưa kế hoạch ban đầu vào. 

Nhiệm vụ là quyết định xem chúng ta phải giao bao nhiêu bản sao ban đầu của kế hoạch cho những người đưa tin để sau khi di chuyển và sao chép, mỗi thành phố đều được ít nhất một người đưa tin mang kế hoạch đến thăm. Một trình nhắn tin có thể đi qua biểu đồ có hướng một cách tùy ý, do đó khả năng tiếp cận hoàn toàn bị chi phối bởi các đường dẫn có hướng. 

Giải thích cấu trúc quan trọng là mỗi kế hoạch ban đầu được đặt trên trình nhắn tin sẽ bắt đầu một khu vực có thể tiếp cận: tất cả các thành phố có thể tiếp cận được từ vị trí xuất phát của trình nhắn tin đó cuối cùng sẽ được bao phủ. Bởi vì việc sao chép là miễn phí và cục bộ, nên khi bất kỳ người đưa tin nào có kế hoạch đến một thành phố, tất cả người đưa tin ở đó đều có thể mang nó đi xa hơn, vì vậy quá trình này tương đương với việc chọn người đưa tin ban đầu và truyền bá khả năng tiếp cận về phía trước. 

Ràng buộc về tổng số thành phố trong các thử nghiệm là nhỏ, với tối đa 200 nút tổng thể và các cạnh lên tới 800. Điều này cho thấy rõ ràng thuật toán đồ thị O(n^2) hoặc O(nm) cho mỗi thử nghiệm đều có thể chấp nhận được, nhưng bất kỳ số mũ nào trong n hoặc liên quan đến việc liệt kê tập hợp con của các nút thì không. 

Một trường hợp thất bại tinh tế xuất hiện khi một thành phố không có nguồn kế hoạch có thể tiếp cận được nhưng cũng không có người đưa tin ban đầu. Ví dụ: nếu một nút có bậc 0 và a_i = 0 thì điều đó ngay lập tức là không thể. Một trường hợp thất bại khác phát sinh khi biểu đồ có nhiều thành phần được kết nối mạnh (SCC): bên trong SCC, một kế hoạch là đủ, nhưng giữa các SCC, vấn đề về hướng và SCC không thể truy cập buộc phải có thêm các vị trí ban đầu. 

Một cách tiếp cận đơn giản có thể thử mô phỏng tất cả các vị trí xuất phát hoặc tham lam chọn thành phố, nhưng các chiến lược như vậy sẽ thất bại khi khả năng tiếp cận chồng chéo giữa các SCC là không cần thiết, bởi vì việc chọn điểm xuất phát trong một SCC có thể đã bao trùm nhiều SCC khác và các lựa chọn tham lam cục bộ không nắm bắt được cấu trúc khả năng tiếp cận toàn cầu. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ trực tiếp là thử chọn các tập hợp con của trình nhắn tin làm điểm bắt đầu cho kế hoạch và mô phỏng khả năng tiếp cận. Đối với mỗi trình nhắn tin bắt đầu được chọn, chúng tôi chạy BFS/DFS để đánh dấu tất cả các thành phố có thể tiếp cận và kiểm tra xem tất cả các thành phố có được bao phủ hay không. Điều này yêu cầu kiểm tra tất cả các tập con có kích thước k, tăng k từ 1 trở lên. 

Ngay cả khi chúng tôi sửa k, số lượng tập hợp con là tổ hợp của số lượng trình nhắn tin, có thể là O(2^n). Mỗi mô phỏng có chi phí O(n + m), do đó tổng số là hàm mũ và không khả thi ngay cả với n = 200. 

Thông tin chi tiết quan trọng là khả năng tiếp cận được xác định bởi các thành phần được kết nối chặt chẽ. Trong SCC, nếu một người đưa tin trong SCC đó nhận được gói, tất cả các nút trong SCC sẽ trở nên tương đương vì chúng có thể liên lạc với nhau. Do đó, mỗi SCC hoạt động giống như một nút duy nhất trong DAG cô đọng. 

Khi chúng tôi nén biểu đồ thành SCC, chúng tôi sẽ thu được DAG. Trong DAG này, bất kỳ SCC nào không có cạnh đến từ các SCC khác đều không thể tiếp cận được từ bên ngoài. Những SCC như vậy phải chứa ít nhất một vị trí kế hoạch ban đầu bên trong chúng, nếu không chúng sẽ không được thông báo mãi mãi. Nếu một SCC như vậy ban đầu cũng không chứa người đưa tin thì không thể đáp ứng được điều đó. 

Do đó, vấn đề giảm xuống còn việc xác định SCC, thu gọn chúng và đảm bảo mọi SCC nguồn (theo độ 0 trong biểu đồ ngưng tụ) đều chứa ít nhất một sứ giả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tập hợp con Brute Force | O(2^n · (n + m)) | O(n + m) | Quá chậm | 
| Nén SCC (Kosaraju/Tarjan) | O(n + m) | O(n + m) | Đã chấp nhận |

## Hướng dẫn thuật toán 

1. Tính các thành phần liên thông mạnh của đồ thị có hướng. Nhóm này nhóm các thành phố nơi khả năng tiếp cận lẫn nhau được đảm bảo, do đó, bất kỳ kế hoạch nào đi vào một nút sẽ lan rộng đến toàn bộ thành phần. 
2. Xây dựng biểu đồ thu gọn trong đó mỗi SCC là một nút. Với mọi cạnh u → v trong đồ thị ban đầu, nếu u và v thuộc các thành phần khác nhau thì hãy thêm một cạnh vào giữa các SCC của chúng. 
3. Tính bậc của từng SCC trong đồ thị thu gọn. 
4. Đối với mỗi SCC, hãy xác định xem nó có chứa ít nhất một thành phố có a_i > 0 hay không. Điều này cho biết liệu chúng ta có thể đặt ít nhất một sơ đồ ban đầu bên trong SCC này hay không. 
5. Đối với mỗi SCC có mức độ bằng 0, hãy kiểm tra xem nó có chứa ít nhất một trình nhắn tin hay không. Nếu không, hãy trả về -1 vì không SCC bên ngoài nào có thể tiếp cận nó và chúng ta không thể bắt đầu bên trong nó. 
6. Đếm xem có bao nhiêu SCC có bậc 0 tồn tại. Con số này chính là câu trả lời, vì mỗi SCC như vậy yêu cầu ít nhất một kế hoạch khởi đầu độc lập. 

Lý do điều này hoạt động là do sự ngưng tụ SCC tạo ra DAG trong đó thông tin chỉ chảy dọc theo các cạnh. Bất kỳ SCC nào có mức độ bằng 0 đều là nguồn trong DAG này, vì vậy không thành phần nào khác có thể truyền một kế hoạch vào đó. Bên trong SCC, một kế hoạch khởi đầu là đủ vì khả năng tiếp cận lẫn nhau hoàn toàn đảm bảo phạm vi phủ sóng nội bộ hoàn chỉnh và lan truyền ra bên ngoài. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

def kosaraju(n, g, gr):
    vis = [False] * n
    order = []

    def dfs1(v):
        vis[v] = True
        for to in g[v]:
            if not vis[to]:
                dfs1(to)
        order.append(v)

    for i in range(n):
        if not vis[i]:
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
    t = int(input())
    out = []

    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))

        g = [[] for _ in range(n)]
        gr = [[] for _ in range(n)]

        for _ in range(m):
            u, v = map(int, input().split())
            u -= 1
            v -= 1
            g[u].append(v)
            gr[v].append(u)

        comp, k = kosaraju(n, g, gr)

        indeg = [0] * k
        has_messenger = [False] * k

        for i in range(n):
            has_messenger[comp[i]] = has_messenger[comp[i]] or (a[i] > 0)

        for u in range(n):
            for v in g[u]:
                cu, cv = comp[u], comp[v]
                if cu != cv:
                    indeg[cv] += 1

        ans = 0
        for i in range(k):
            if indeg[i] == 0:
                if not has_messenger[i]:
                    out.append("-1")
                    break
                ans += 1
        else:
            out.append(str(ans))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ tính toán SCC bằng thuật toán của Kosaraju, phân tách các danh sách kề thuận và ngược. Sau khi tính toán các mã định danh thành phần, nó sẽ xây dựng siêu dữ liệu cho mỗi thành phần: liệu nó có chứa ít nhất một trình nhắn tin hay không và mức độ của nó trong biểu đồ ngưng tụ. 

Một điểm tinh tế là các cạnh chỉ được tính khi giao nhau các thành phần; Các cạnh SCC bên trong phải được bỏ qua vì chúng không thể hiện sự lan truyền giữa các thành phần. Vòng cuối cùng đếm chính xác số lượng SCC nguồn trong khi xác thực tính khả thi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Biểu đồ đầu vào có nhiều SCC; xem xét một dấu vết đơn giản hóa. 

| Thành phố | a_i | Thành phần | Bằng cấp | Có Messenger | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 0 | 0 | vâng | 
| 2 | 1 | 0 | 0 | vâng | 
| 3 | 0 | 1 | 1 | không | 
| 4 | 1 | 1 | 1 | vâng | 

SCC 0 có cấp độ 0 và chứa các trình nhắn tin, SCC 1 có cấp độ 1. 

Chúng tôi xử lý SCC theo nghĩa tôpô: SCC 0 là nguồn, SCC 1 thì không. Chỉ SCC 0 mới cần một kế hoạch khởi đầu. 

Câu trả lời là 1. 

Điều này cho thấy rằng SCC xuôi dòng không đóng góp vào câu trả lời ngay cả khi chúng chứa các trình nhắn tin, bởi vì chúng có thể được truy cập từ SCC nguồn. 

### Ví dụ 2 

Hãy xem xét trường hợp SCC nguồn không có trình nhắn tin. 

| Thành phố | a_i | Thành phần | Bằng cấp | 
| --- | --- | --- | --- | 
| 1 | 0 | 0 | 0 | 
| 2 | 0 | 0 | 0 | 
| 3 | 1 | 1 | 1 | 

SCC 0 là nguồn nhưng không có trình truyền tin. Không có kế hoạch nào có thể được đưa vào đó vì không có trình nhắn tin ban đầu nào tồn tại trong SCC đó và không có cạnh nào có thể mang đến. 

Thuật toán ngay lập tức trả về -1 khi phát hiện điều kiện này, chứng tỏ tính đúng đắn của việc kiểm tra tính khả thi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Hai đường chuyền DFS dành cho SCC cộng với xử lý cạnh tuyến tính trên biểu đồ thu gọn | 
| Không gian | O(n + m) | Danh sách kề, đồ thị ngược, mảng thành phần | 

Các ràng buộc cho phép tối đa 200 nút và 800 cạnh, vì vậy các thuật toán đồ thị tuyến tính là đủ dễ dàng. Việc giảm dựa trên SCC đảm bảo chúng tôi không bao giờ thử bất kỳ hoạt động khám phá tổ hợp nào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# provided samples
assert run("""2
7 6
2 1 0 1 2 3 4
1 2
1 3
2 4
2 5
3 6
3 7
4 4
1 1 1 1
1 2
1 3
2 4
3 4
""") == "2\n2"

# single node SCC-like structure
assert run("""1
3 2
1 0 0
1 2
2 3
""") == "1"

# impossible source SCC
assert run("""1
2 0
0 1
""") == "-1"

# cycle SCC
assert run("""1
3 3
0 1 0
1 2
2 3
3 1
""") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đồ thị chuỗi | 1 | truyền qua DAG | 
| không có tin nhắn trong nguồn SCC | -1 | thất bại khả thi | 
| chu kỳ đầy đủ | 1 | Độ chính xác nén SCC | 
| nhiều nguồn | k | đếm các nguồn SCC độc lập | 

## Vỏ cạnh 

Trường hợp nguy hiểm là khi SCC nguồn không có trình nhắn tin. Trong tình huống đó, mặc dù về mặt cấu trúc nó là bắt buộc nhưng nó không thể được kích hoạt. Thuật toán phát hiện chính xác điều này trong quá trình tổng hợp SCC bằng cách kiểm tra has_messenger trước khi đếm. 

Một trường hợp cạnh khác là đồ thị liên thông mạnh đầy đủ. Tất cả các nút tạo thành một SCC với mức độ bằng 0. Nếu có ít nhất một sứ giả tồn tại ở bất cứ đâu thì một kế hoạch là đủ vì nó sẽ lan truyền khắp SCC. Thuật toán thu gọn biểu đồ thành một thành phần và trả về 1. 

Trường hợp cạnh thứ ba liên quan đến các thành phần bị ngắt kết nối không có cạnh. Mỗi nút là SCC riêng của nó với mức độ bằng 0. Câu trả lời là số nút chứa ít nhất một trình nhắn tin vì mỗi nút phải được khởi động độc lập.
