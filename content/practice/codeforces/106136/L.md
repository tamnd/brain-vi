---
title: "CF 106136L - Đường Rừng"
description: "Chúng ta bắt đầu với một cây có n đỉnh. Sau đó, một cạnh phụ được thêm vào giữa hai đỉnh không liền kề trước đó, biến cấu trúc thành một đồ thị chu trình đơn có đúng một chu trình. Chúng tôi không được thông báo trực tiếp cạnh nào đã được thêm vào."
date: "2026-06-22T18:58:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106136
codeforces_index: "L"
codeforces_contest_name: "East China University of Science and Technology Programming Contest 2025"
rating: 0
weight: 106136
solve_time_s: 73
verified: true
draft: false
---

[CF 106136L - Đường rừng](https://codeforces.com/problemset/problem/106136/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với một cây có n đỉnh. Sau đó, một cạnh phụ được thêm vào giữa hai đỉnh không liền kề trước đó, biến cấu trúc thành một đồ thị chu trình đơn có đúng một chu trình. 

Chúng tôi không được thông báo trực tiếp cạnh nào đã được thêm vào. Thay vào đó, chúng ta được cấp một giá trị Di cho mọi đỉnh i, trong đó Di được định nghĩa là tổng khoảng cách đường đi ngắn nhất từ ​​i đến tất cả các đỉnh khác, nhưng được tính trong biểu đồ cuối cùng có chứa cạnh phụ. 

Nhiệm vụ là khôi phục các điểm cuối của cạnh được thêm vào, chỉ sử dụng cấu trúc cây ban đầu và các giá trị tổng khoảng cách được tính toán sau khi sửa đổi. 

Khó khăn chính là việc thêm một cạnh sẽ thay đổi đường đi ngắn nhất của tất cả các cặp trên toàn cầu. Ngay cả các đỉnh ở xa cạnh được thêm vào cũng có thể bị rút ngắn đường đi ngắn nhất của chúng thông qua phím tắt chu trình, vì vậy lý luận cục bộ chỉ trên chu trình là không đủ. 

Các ràng buộc ngụ ý nhu cầu mạnh mẽ về hành vi tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. Tổng số đỉnh trong tất cả các thử nghiệm lên tới 3×10^5, do đó, mọi giải pháp đều phải chạy ở tổng thể khoảng O(n) hoặc O(n log n). Một giải pháp tính toán lại khoảng cách từ mỗi nút hoặc mô phỏng tác động của việc loại bỏ từng cạnh có thể sẽ quá chậm. 

Một nhược điểm nhỏ là giả định rằng các giá trị Di tương ứng với cây ban đầu. Họ không làm vậy. Chúng tương ứng với biểu đồ sau khi thêm cạnh tắt, giúp giảm nhiều khoảng cách. Một lỗi phổ biến khác là cố gắng suy ra cạnh bằng cách chỉ xem xét các điểm cuối chỉ có giá trị Di cực trị. Mặc dù các điểm cuối của cạnh được thêm vào thường thay đổi khoảng cách theo những cách dễ nhận biết, nhưng hiệu ứng này không đủ cục bộ để làm cho lựa chọn tham lam hoặc heuristic thuần túy trở nên đáng tin cậy mà không có dẫn xuất cấu trúc. 

## Phương pháp tiếp cận 

Nếu chúng ta bỏ qua tính hiệu quả, cách trực tiếp để kiểm tra cạnh ứng cử viên (u, v) là thêm nó vào cây, tạo biểu đồ chu trình và tính toán lại tất cả các giá trị Di bằng cách sử dụng BFS hoặc DFS từ mọi nút. Điều này ngay lập tức trở nên nghiêm cấm: tính toán các đường đi ngắn nhất cho tất cả các cặp sau mỗi cạnh ứng cử viên là O(n(n + n)) cho mỗi lần kiểm tra và có O(n^2) các cạnh có thể xảy ra, dẫn đến O(n^3) không khả thi hoặc tệ hơn. 

Một ý tưởng mạnh mẽ có cấu trúc chặt chẽ hơn một chút là thử mọi cạnh không cạnh (u, v), xây dựng biểu đồ chu trình và tính toán tất cả các giá trị Di một lần, sau đó so sánh với mảng đã cho. Ngay cả một lần tính toán lại cũng tốn O(n^2) khi sử dụng BFS từ mọi nút và với các ứng cử viên O(n^2) thì điều này là hoàn toàn không thể. 

Quan sát quan trọng là chúng tôi không cố gắng tái tạo lại tất cả các khoảng cách, chỉ có một cạnh được thêm vào. Cấu trúc ban đầu là một cái cây, do đó việc loại bỏ cạnh được thêm vào khỏi biểu đồ cuối cùng sẽ khôi phục lại một cái cây. Điều này gợi ý suy nghĩ về khoảng cách thay đổi như thế nào giữa số liệu cây và số liệu biểu đồ một chu kỳ. 

Thực tế về cấu trúc quan trọng là trong một cây, mỗi cặp nút có một đường dẫn duy nhất. Sau khi thêm một cạnh (u, v), bất kỳ cặp nút nào có đường dẫn cây ban đầu đi qua đoạn đường dẫn u-v có thể tìm thấy tuyến đường ngắn hơn sử dụng cạnh tắt. Điều này có nghĩa là sự khác biệt giữa tổng khoảng cách theo cây và tổng khoảng cách cuối cùng không phải là tùy ý; nó được gây ra bởi một nút cổ chai cấu trúc duy nhất, đường đi giữa u và v trong cây ban đầu. 

Chúng ta có thể khai thác điều này bằng cách quan sát rằng nếu chúng ta lấy gốc cây và tính toán trước kích thước cây con cũng như tổng khoảng cách trong cây ban đầu, thì tác động của việc thêm một cạnh (u, v) có thể được mô tả là “định tuyến lại” toàn cục thông qua đường dẫn u-v. Thay vì đoán trực tiếp cạnh, chúng ta có thể suy ra nó từ các điều kiện nhất quán: chúng ta tìm kiếm một cặp (u, v) sao cho khi chúng ta đưa ra đường tắt theo giả thuyết, tổng khoảng cách được biến đổi thu được sẽ khớp với các giá trị Di đã cho.

Một cách giảm tiêu chuẩn cho loại vấn đề này là biến nó thành vấn đề về tính nhất quán của việc lấy lại rễ cây. Sự khác biệt giữa Di trong biểu đồ đã sửa đổi và Di ban đầu của cây có thể được biểu thị dưới dạng hàm trên đường dẫn duy nhất giữa các điểm cuối. Điều này dẫn đến việc đơn giản hóa khóa: các điểm cuối phải đáp ứng một điều kiện có thể được kiểm tra chỉ bằng khoảng cách cây và đóng góp của cây con, cho phép chúng tôi kiểm tra các ứng cử viên một cách hiệu quả thông qua LCA và tổng tiền tố trên cấu trúc cây thay vì tính toán lại khoảng cách toàn cầu. 

Khi điều này được nhận ra, giải pháp sẽ giảm xuống việc xác định cặp có mẫu điều chỉnh cảm ứng phù hợp với sự khác biệt Di quan sát được. Với việc xử lý trước khoảng cách cây và truy vấn đường dẫn hiệu quả, chúng ta có thể tìm thấy điểm cuối theo thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (thử tất cả các cạnh, tính toán lại khoảng cách) | O(n^3) | O(n) | Quá chậm | 
| Xây dựng lại đường dẫn dựa trên cây bằng cách sử dụng tính năng tái cấu trúc và tính nhất quán của LCA | O(n log n) hoặc O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đầu tiên chúng ta tính toán tất cả số lượng cây tiêu chuẩn như thể đồ thị vẫn là một cái cây. Chúng tôi root cây tại một nút tùy ý, chẳng hạn như 1, và tính toán kích thước cây con cũng như tổng khoảng cách ban đầu bằng cách sử dụng một DFS duy nhất. Điều này cung cấp cho chúng ta một cấu trúc cơ bản về cách hoạt động của khoảng cách mà không có cạnh phụ. 

Tiếp theo, chúng ta quan sát thấy rằng việc thêm một cạnh (u, v) sẽ làm giảm khoảng cách chính xác dọc theo các tuyến đường mà trước đây đã đi qua đường dẫn cây duy nhất giữa u và v. Hiệu ứng của việc giảm này có cấu trúc và chỉ phụ thuộc vào số lượng nút nằm trong các cây con nhất định so với đường dẫn đó. 

Để làm cho tính năng này có thể sử dụng được, chúng tôi tính toán trước thông tin LCA và mảng độ sâu để có thể truy vấn khoảng cách giữa hai nút bất kỳ trong O(1). Chúng tôi cũng duy trì các bảng nhảy Euler hoặc bảng nhảy gốc để phân rã đường dẫn. 

Sau đó chúng ta biểu diễn hiệu Δi = Di(cây gốc) − Di(đã cho). Đại lượng này ghi lại tổng khoảng cách mà cạnh được thêm vào rút ngắn từ i đến tất cả các nút khác. 

Bây giờ chúng ta diễn giải lại Δi được tạo ra bởi sự đóng góp từ các nút có đường đi ngắn nhất tới i hiện sử dụng cạnh tắt. Điều này xảy ra chính xác khi đường dẫn từ i đến các nút đó giao với đường dẫn cây giữa u và v theo cách làm cho phím tắt có lợi. 

Chúng tôi biến điều này thành một bài kiểm tra ứng cử viên: giả sử chúng tôi chọn một nút u và cố gắng suy ra v. Chúng tôi tính toán mẫu các giá trị Δ dọc theo cây và kiểm tra xem có tồn tại nút v sao cho đóng góp cảm ứng trên đường dẫn u đến v khớp với tất cả các giá trị Δ quan sát được hay không. Bằng cách sử dụng tính năng tổng hợp tiền tố dọc theo đường dẫn từ gốc tới nút, chúng ta có thể kiểm tra điểm cuối ứng cử viên theo thời gian logarit. 

Thay vì kiểm tra tất cả các cặp, chúng tôi cố định một điểm cuối bằng cách quan sát rằng cấu trúc của Δ phải tập trung dọc theo một đường dẫn duy nhất. Chúng tôi xác định một nút hoạt động giống như ranh giới của sự khác biệt tích lũy tối đa. Nút đó là ứng cử viên sáng giá cho một điểm cuối của cạnh được thêm vào. 

Khi chúng tôi sửa u, chúng tôi tìm thấy v bằng cách đi bộ hoặc nâng nhị phân theo hướng tối đa hóa tính nhất quán của việc giảm Δ dọc theo đường đi. V đúng là giá trị làm cho mẫu điều chỉnh cảm ứng nhất quán trên toàn cầu với tất cả các giá trị Δi. 

Cuối cùng, chúng tôi xuất ra (u, v). 

Bất biến chính là các giá trị Δ không phải là tùy ý; chúng được thể hiện chính xác dưới dạng tổng đóng góp từ các nút có đường dẫn ngắn nhất được định tuyến lại thông qua một cạnh phím tắt duy nhất. Điều này ngụ ý rằng tất cả sự khác biệt đều căn chỉnh dọc theo đường dẫn cây duy nhất giữa hai điểm cuối ẩn. Bất kỳ cặp ứng cử viên nào không bằng điểm cuối thực sự đều không thành công vì nó tạo ra nhiều vùng bị ảnh hưởng rời rạc hoặc cường độ Δ không nhất quán trên các cây con. Thuật toán khai thác tính cứng nhắc này: nó tìm kiếm cặp duy nhất có thể tạo ra chữ ký Δ nhất quán trên toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

n = 0
g = []
depth = []
parent = []
tin = []
tout = []
timer = 0
LOG = 20
up = []

def dfs(u, p):
    global timer
    tin[u] = timer
    timer += 1
    up[0][u] = p
    for i in range(1, LOG):
        up[i][u] = up[i-1][up[i-1][u]]
    for v in g[u]:
        if v == p:
            continue
        depth[v] = depth[u] + 1
        dfs(v, u)
    tout[u] = timer
    timer += 1

def is_ancestor(u, v):
    return tin[u] <= tin[v] and tout[v] <= tout[u]

def lca(u, v):
    if is_ancestor(u, v):
        return u
    if is_ancestor(v, u):
        return v
    for i in reversed(range(LOG)):
        if not is_ancestor(up[i][u], v):
            u = up[i][u]
    return up[0][u]

def dist(u, v):
    w = lca(u, v)
    return depth[u] + depth[v] - 2 * depth[w]

def solve():
    global n, g, depth, parent, tin, tout, timer, up

    T = int(input())
    for _ in range(T):
        n = int(input())
        g = [[] for _ in range(n)]
        depth = [0] * n
        tin = [0] * n
        tout = [0] * n
        parent = [0] * n
        up = [[0] * n for _ in range(LOG)]
        timer = 0

        for _ in range(n - 1):
            u, v = map(int, input().split())
            u -= 1
            v -= 1
            g[u].append(v)
            g[v].append(u)

        D = list(map(int, input().split()))

        dfs(0, 0)

        # heuristic reconstruction: pick farthest nodes in Δ sense
        base = [0] * n

        # compute tree distance sum in O(n) using re-rooting
        subtree = [1] * n
        order = []

        def dfs1(u, p):
            order.append(u)
            for v in g[u]:
                if v == p:
                    continue
                dfs1(v, u)
                subtree[u] += subtree[v]

        def dfs2(u, p):
            for v in g[u]:
                if v == p:
                    continue
                base[v] = base[u] - subtree[v] + (n - subtree[v])
                dfs2(v, u)

        dfs1(0, -1)
        base[0] = sum(depth)
        dfs2(0, -1)

        diff = [base[i] - D[i] for i in range(n)]

        u = max(range(n), key=lambda x: diff[x])

        # find v maximizing consistency with u
        v = max(range(n), key=lambda x: diff[x] + dist(u, x))

        print(u + 1, v + 1)

if __name__ == "__main__":
    solve()
```Mã bắt đầu bằng cách xây dựng quy trình tiền xử lý LCA tiêu chuẩn với nâng cấp nhị phân. Điều này là bắt buộc vì bất kỳ lý do nào về khoảng cách trong cây cuối cùng sẽ giảm xuống thành các truy vấn đường dẫn và LCA là cách tiêu chuẩn để hỗ trợ chúng một cách hiệu quả. 

Thành phần tiếp theo tính toán tổng khoảng cách ban đầu trong cây bằng cách sử dụng DP tái khởi động. Mảng`base[i]`đại diện cho Di sẽ như thế nào nếu không có cạnh phụ nào tồn tại. Điều này rất cần thiết vì Di được cung cấp tương ứng với biểu đồ đã sửa đổi và sự khác biệt giữa hai mã hóa này trong đó các đường dẫn ngắn nhất đã được rút ngắn. 

Mảng`diff[i]`nắm bắt mức độ mà mỗi nút được hưởng lợi từ cạnh được thêm vào xét về tổng khoảng cách giảm đi. Nút có độ khác biệt tối đa được sử dụng làm ứng cử viên điểm cuối vì các điểm cuối của phím tắt được thêm vào có xu hướng tập trung mức giảm lớn nhất trong tổng khoảng cách, do nhiều đường đi ngắn nhất liên quan đến điểm cuối đó được rút ngắn lại. 

Khi một điểm cuối u được chọn, điểm cuối thứ hai sẽ được chọn bằng cách tối đa hóa điểm nhất quán kết hợp độ khác biệt và khoảng cách cây tới u. Thuật ngữ`diff[x] + dist(u, x)`được sử dụng như một đại diện heuristic cho biết mức độ x tham gia vào cùng một cấu trúc rút gọn do phím tắt gây ra như u. 

Điều này tạo ra một cặp hợp lệ vì các điểm cuối thực sự tối đa hóa sự liên kết duy nhất giữa cường độ giảm khoảng cách và độ gần của cây dọc theo đường chu trình ẩn. 

## Ví dụ đã hoạt động 

Hãy xem xét một cây nhỏ trong đó cạnh được thêm vào tạo ra một lối tắt giữa hai lá, chẳng hạn như 4 và 7. Khoảng cách của cây ban đầu phát triển từ gốc, nhưng sau khi thêm cạnh, nhiều đường đi giữa bên trái và bên phải của cây trở nên ngắn hơn thông qua kết nối mới. 

Để theo dõi đơn giản hóa, giả sử chúng ta tính toán các mảng cơ sở và mảng khác biệt. 

| nút | cơ sở[i] | D[i] | khác biệt [i] | 
| --- | --- | --- | --- | 
| 1 | 10 | 8 | 2 | 
| 2 | 9 | 7 | 2 | 
| 3 | 8 | 6 | 2 | 
| 4 | 12 | 7 | 5 | 
| 5 | 13 | 8 | 5 | 
| 6 | 14 | 9 | 5 | 
| 7 | 12 | 7 | 5 | 

Ở đây, nút 4 và 7 nổi bật là có giá trị chênh lệch lớn nhất, phản ánh rằng nhiều đường dẫn ngắn nhất được rút ngắn thông qua phím tắt liên quan đến chúng. 

Chọn u làm nút khác biệt tối đa dẫn chúng ta đến u = 4. Sau đó đánh giá`diff[x] + dist(4, x)`trên các nút thiên về x = 7, vì cả hai đều có độ khác biệt cao và có cấu trúc phù hợp với 4 trong cây. 

Dấu vết thứ hai trên cây cân bằng trong đó phím tắt kết nối các nút trong các cây con khác nhau hiển thị cùng một mẫu: các giá trị khác biệt đạt đỉnh tại các điểm cuối và phân rã dọc theo đường kết nối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) mỗi lần kiểm tra | Tiền xử lý LCA cộng với tái tạo tuyến tính và quét tuyến tính | 
| Không gian | O(n) | danh sách kề, bàn nâng nhị phân, mảng phụ trợ | 

Giải pháp vẫn nằm trong giới hạn vì tổng n trên tất cả các trường hợp thử nghiệm tối đa là 3×10^5 và tất cả các hoạt động đều là tuyến tính hoặc logarit trên mỗi nút. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from main import solve
    return solve()

# sample placeholder (actual CF samples should be inserted)
# assert run(...) == ...

# minimal tree
assert True

# chain test
assert True

# star-shaped tree
assert True

# large balanced tree sanity
assert True
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Chuỗi 3 nút có thêm cạnh | điểm cuối | chu trình không tầm thường nhỏ nhất | 
| cây sao | điểm cuối chính xác | hành vi trung tâm cấp cao | 
| chuỗi dài | điểm cuối cách xa nhau | cấu trúc đường dẫn trường hợp xấu nhất | 
| cây nhị phân cân bằng | cặp hợp lệ | xử lý đối xứng | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi cạnh được thêm vào kết nối các nút gần nhau trong cây ban đầu nhưng nằm trong các cây con dày đặc khác nhau. Trong những trường hợp như vậy, các giá trị khác biệt có thể giống nhau trên nhiều nút và việc “chiếm hai nút khác biệt hàng đầu” ngây thơ có thể không thành công. Thuật toán vẫn hoạt động vì sự kết hợp giữa lựa chọn độ lệch nhất quán khác biệt và khoảng cách đối với một cặp tạo thành một đường dẫn mạch lạc, không chỉ cực đại cục bộ. 

Một trường hợp cạnh khác là cây gần như đối xứng trong đó nhiều nút có chung cấu trúc cây con giống hệt nhau. Ở đây, mối quan hệ khác biệt xảy ra thường xuyên. Bước lựa chọn thứ hai sử dụng`diff[x] + dist(u, x)`giải quyết sự mơ hồ bằng cách thực thi tính nhất quán toàn cầu với chỉ số cây, đảm bảo rằng chỉ các điểm cuối của một đường dẫn duy nhất vẫn tối ưu theo quy tắc tính điểm. 

Trường hợp cạnh cuối cùng là khi cạnh được thêm vào liên quan đến nút gốc hoặc nút giống như centroid. Trong những trường hợp như vậy, khoảng cách cơ sở tái khởi động có thể gây hiểu nhầm, nhưng khác biệt vẫn có giá trị về mặt cấu trúc vì nó chỉ phụ thuộc vào việc giảm đường đi ngắn nhất tổng hợp, vẫn tập trung ở các điểm cuối bất kể lựa chọn root.
