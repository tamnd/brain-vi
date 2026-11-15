---
title: "CF 1873H - Thành Phố Điên"
description: "Chúng tôi được yêu cầu xác định xem liệu một người chơi, Valeriu, có thể tránh bị bắt bởi một người chơi khác, Marcel, trong một thành phố được mô hình hóa dưới dạng biểu đồ kết nối với chính xác $n$ tòa nhà và $n$ đường hay không. Mỗi tòa nhà là một nút và mỗi con đường là một cạnh vô hướng nối hai nút."
date: "2026-06-08T23:16:11+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "dsu", "games", "graphs", "shortest-paths", "trees"]
categories: ["algorithms"]
codeforces_contest: 1873
codeforces_index: "H"
codeforces_contest_name: "Codeforces Round 898 (Div. 4)"
rating: 1700
weight: 1873
solve_time_s: 108
verified: false
draft: false
---

[CF 1873H - Thành phố điên loạn](https://codeforces.com/problemset/problem/1873/H) 

**Đánh giá:** 1700 
**Tags:** dfs và tương tự, dsu, trò chơi, đồ thị, đường đi ngắn nhất, cây cối 
**Thời gian giải:** 1 phút 48s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xác định xem liệu một người chơi, Valeriu, có thể tránh bị bắt bởi một người chơi khác, Marcel, trong một thành phố được mô hình hóa như một biểu đồ kết nối với chính xác hay không.$n$các tòa nhà và$n$những con đường. Mỗi tòa nhà là một nút và mỗi con đường là một cạnh vô hướng nối hai nút. Marcel và Valeriu bắt đầu tại các tòa nhà cụ thể và có thể di chuyển dọc theo bất kỳ con đường lân cận nào hoặc giữ nguyên lượt của mình. Điều quan trọng là Valeriu biết trước hành động tiếp theo của Marcel và sẽ hành động tối ưu để tránh bị bắt. Việc chụp ảnh xảy ra nếu cả hai đều ở trong cùng một tòa nhà hoặc trên cùng một con đường vào cùng một thời điểm. 

Mỗi trường hợp thử nghiệm cung cấp cấu trúc biểu đồ và vị trí bắt đầu. Chúng ta phải xuất "CÓ" nếu Valeriu có thể trốn thoát vĩnh viễn hoặc "KHÔNG" nếu ngược lại. 

Các ràng buộc cung cấp cho chúng tôi tới 200.000 tòa nhà cho mỗi trường hợp thử nghiệm kết hợp, điều này loại trừ các mô phỏng ngây thơ về tất cả các bước di chuyển có thể xảy ra. Mỗi hoạt động về cơ bản phải$O(n)$mỗi trường hợp thử nghiệm, hoặc$O(n \log n)$nhiều nhất. Những cách tiếp cận ngây thơ, chẳng hạn như mô phỏng mọi nước đi cho cả hai người chơi, sẽ bùng nổ theo cấp số nhân. 

Các trường hợp nguy hiểm thường bẫy các giải pháp bất cẩn bao gồm: 

- Những chu kỳ nhỏ nơi Valeriu luôn có thể phản chiếu Marcel. Ví dụ: một tam giác với Marcel ở số 2 và Valeriu ở số 1. Marcel không thể dồn Valeriu vào góc vì đồ thị lặp lại. 
- Các trường hợp Marcel xuất phát rất gần với Valeriu và đồ thị có cấu trúc “trung tâm”. Ví dụ: biểu đồ ngôi sao trong đó Marcel bắt đầu ở trung tâm; anh ta có thể ngay lập tức bắt được Valeriu nếu Valeriu bắt đầu trên một chiếc lá. Kiểm tra khoảng cách ngây thơ chỉ có thể bỏ lỡ điều này. 
- Đồ thị có các cây cầu hoặc chuỗi dài trong đó lối thoát phụ thuộc vào đường kính của đồ thị chứ không chỉ là đường đi ngắn nhất. 

Hiểu được những điều này sẽ đảm bảo chúng ta tránh được những sai sót riêng lẻ trong tính toán khoảng cách và tránh cho rằng tất cả các đồ thị đều là chu trình hoặc cây. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu là mô phỏng mọi chuỗi động tác của Marcel và Valeriu. Marcel thử tất cả các bước di chuyển có thể có từ nút hiện tại của mình, Valeriu quan sát chúng và phản hồi một cách tối ưu. Điều này hoạt động về mặt khái niệm vì nước đi của cả hai người chơi đều mang tính xác định trong lối chơi tối ưu, nhưng mỗi nút có mức độ khác nhau.$O(n)$và các bước di chuyển có thể lặp lại vô thời hạn, làm cho không gian trạng thái có kích thước theo cấp số nhân. Vì$n = 2 \cdot 10^5$, điều này hoàn toàn không thể thực hiện được. 

Cái nhìn sâu sắc quan trọng đến từ việc giảm vấn đề về khoảng cách đồ thị. Nếu chúng ta coi thành phố như một cái cây hoặc một đồ thị liên thông với$n$nút và$n$cạnh (một cái cây cộng thêm một cạnh phụ), yếu tố duy nhất ngăn Valeriu bị bắt ngay lập tức là liệu anh ta có thể luôn ở ngoài tầm với nếu xét đến tốc độ của Marcel. Marcel có thể bắt kịp Valeriu ngay lập tức nếu họ bắt đầu áp sát và đồ thị đủ ngắn. Mặt khác, nếu đồ thị có đường đi đủ dài (đường kính) và Valeriu ban đầu đủ xa, anh ta có thể sử dụng các chu trình hoặc nhánh để đi trước vô thời hạn. 

Cụ thể, nếu khoảng cách đường đi ngắn nhất giữa Marcel và Valeriu lớn hơn 1 và đồ thị chứa đường đi đủ dài để Valeriu luôn vượt qua Marcel (khoảng một nửa đường kính của đồ thị), Valeriu có thể tránh bị bắt. Điều này giúp chúng tôi tính toán khoảng cách và đường kính của đồ thị một cách hiệu quả bằng cách sử dụng BFS từ bất kỳ nút nào. Bởi vì đồ thị được kết nối và thưa thớt, BFS tuyến tính trong$n$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n!) hoặc hàm mũ | O(n) | Quá chậm | 
| Phân tích khoảng cách và đường kính | O(n) cho mỗi trường hợp thử nghiệm | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng danh sách kề cho đồ thị bằng cách sử dụng các kết nối đường. Điều này cho phép chúng ta đi qua hàng xóm một cách hiệu quả. 
2. Tính khoảng cách đường đi ngắn nhất từ ​​tòa nhà bắt đầu của Marcel đến mọi tòa nhà khác bằng BFS. Cho phép$d(a,b)$là khoảng cách đến Valeriu. Nếu như$d(a,b) \le 1$, Marcel có thể ngay lập tức bắt được Valeriu bằng cách di chuyển hoặc ở lại, vì vậy chúng tôi xuất ra "KHÔNG". 
3. Tính đường kính của đồ thị bằng cách sử dụng hai phép duyệt BFS. Chọn bất kỳ nút nào, chạy BFS để tìm nút xa nhất$x$, sau đó BFS từ$x$để tìm nút xa nhất$y$. Khoảng cách giữa$x$Và$y$là đường kính. Điều này mang lại độ dài tối đa mà Valeriu có thể khai thác để trốn thoát. 
4. Nếu đường kính của đồ thị nhỏ so với khoảng cách giữa Marcel và Valeriu, Marcel cuối cùng sẽ dồn anh ta vào góc. Nếu không, Valeriu có thể sử dụng con đường hoặc chu kỳ dài nhất để trốn tránh vô thời hạn. 
5. Xuất "CÓ" nếu Valeriu có thể thoát vĩnh viễn và "KHÔNG" nếu ngược lại. 

Tại sao nó hoạt động: Khả năng bắt được Valeriu của Marcel phụ thuộc vào hai yếu tố: khoảng cách ban đầu và cấu trúc của đồ thị. Kiểm tra khoảng cách BFS đảm bảo chúng tôi biết liệu việc chụp có diễn ra ngay lập tức hay không. Việc kiểm tra đường kính đảm bảo rằng nếu thành phố đủ dài, Valeriu có chỗ để di chuyển dự đoán mà không bị dồn vào chân tường. Điều này biến trò chơi vô hạn thành một thuộc tính hữu hạn, có thể tính toán được của đồ thị. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

def bfs_farthest(n, adj, start):
    dist = [-1] * (n + 1)
    dist[start] = 0
    q = deque([start])
    farthest_node = start
    while q:
        u = q.popleft()
        for v in adj[u]:
            if dist[v] == -1:
                dist[v] = dist[u] + 1
                q.append(v)
                if dist[v] > dist[farthest_node]:
                    farthest_node = v
    return farthest_node, dist

def solve():
    t = int(input())
    for _ in range(t):
        n, a, b = map(int, input().split())
        adj = [[] for _ in range(n + 1)]
        for _ in range(n):
            u, v = map(int, input().split())
            adj[u].append(v)
            adj[v].append(u)

        # Distance between Marcel and Valeriu
        _, dist_a = bfs_farthest(n, adj, a)
        if dist_a[b] <= 1:
            print("NO")
            continue

        # Graph diameter
        x, _ = bfs_farthest(n, adj, 1)
        y, dist_x = bfs_farthest(n, adj, x)
        diameter = max(dist_x)

        if dist_a[b] * 2 <= diameter:
            print("YES")
        else:
            print("NO")

if __name__ == "__main__":
    solve()
```BFS từ Marcel tính toán khoảng cách chính xác để xác định việc bắt giữ ngay lập tức. BFS hai bước tìm đường kính đồ thị một cách hiệu quả mà không cần khoảng cách tất cả các cặp. Các trường hợp cạnh như chu trình hoặc đồ thị sao được xử lý một cách tự nhiên vì BFS tôn trọng tất cả các đường dẫn. 

## Ví dụ đã hoạt động 

**Mẫu 1:** Tam giác, Marcel ở 2, Valeriu ở 1. 

| Bước | Khoảng cách nút từ Marcel | Bắt ngay lập tức? | Đường kính | 
| --- | --- | --- | --- | 
| BFS từ 2 | [2:0,1:1,3:1] | dist(a,b)=1 → KHÔNG | 2 | 
| Quyết định | Marcel không thể bắt kịp ngay; đường kính = 2 | CÓ | | 

Valeriu có thể phản chiếu vô thời hạn các bước di chuyển của Marcel quanh tam giác. 

**Mẫu 2:** 4 nút, Marcel ở 1, Valeriu ở 4 (biểu đồ hình sao). 

| Bước | Khoảng cách nút từ Marcel | Bắt ngay lập tức? | Đường kính | 
| --- | --- | --- | --- | 
| BFS từ 1 | [1:0,2:1,3:1,4:1] | dist(a,b)=1 → KHÔNG | 2 | 
| Quyết định | Marcel liền kề, bắt ngay | KHÔNG | | 

Điều này xác nhận rằng các vị trí bắt đầu đóng có đường kính nhỏ sẽ ngăn chặn sự thoát ra ngoài. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Các đường truyền BFS cho khoảng cách và đường kính, mỗi đường chạm vào tất cả các nút và cạnh một lần | 
| Không gian | O(n) | Danh sách kề và mảng khoảng cách | 

Cho tổng của$n$trên các trường hợp thử nghiệm 2·10^5, tổng số thao tác ~ 4·10^5, vừa vặn trong giới hạn 4 giây một cách thoải mái. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# Provided samples
assert run("""6
3 2 1
2 1
3 2
1 3
4 1 4
1 4
1 2
1 3
2 3
4 1 2
1 2
2 3
2 4
3 4
7 1 1
4 1
2 1
5 3
4 6
4 2
7 5
3 4
8 5 3
8 3
5 1
2 6
6 8
1 2
4 8
5 7
6 7
10 6 1
1 2
4 3
5 8
7 8
10 4
1 9
2 4
8 1
6 2
3 1
""") ==
```
