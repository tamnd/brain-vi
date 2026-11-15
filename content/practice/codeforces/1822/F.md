---
title: "CF 1822F - Những Người Bạn Làm Vườn"
description: "Chúng ta có một cây có gốc ở đỉnh 1, trong đó mọi cạnh đều có cùng độ dài. “Chi phí” của cây chỉ đơn giản là khoảng cách từ đỉnh đến gốc hiện tại là bao xa. Bởi vì tất cả các cạnh đều có trọng số giống nhau nên chi phí này chỉ bằng chiều cao của cây nhân với chiều dài cạnh."
date: "2026-06-09T07:50:31+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dfs-and-similar", "dp", "graphs", "trees"]
categories: ["algorithms"]
codeforces_contest: 1822
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 867 (Div. 3)"
rating: 1700
weight: 1822
solve_time_s: 74
verified: true
draft: false
---

[CF 1822F - Những người bạn làm vườn](https://codeforces.com/problemset/problem/1822/F) 

**Đánh giá:** 1700 
**Thẻ:** lực lượng vũ phu, dfs và tương tự, dp, đồ thị, cây cối 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có gốc ở đỉnh 1, trong đó mọi cạnh đều có cùng độ dài. “Chi phí” của cây chỉ đơn giản là khoảng cách từ đỉnh đến gốc hiện tại là bao xa. Bởi vì tất cả các cạnh đều có trọng số giống nhau nên chi phí này chỉ bằng chiều cao của cây nhân với chiều dài cạnh. 

Điều khó khăn là chúng ta được phép di chuyển gốc, nhưng chỉ dọc theo các cạnh, từng bước một. Mỗi lần di chuyển sẽ dịch gốc sang một đỉnh liền kề và tiêu tốn một số xu cố định. Sau bất kỳ số lần di chuyển nào như vậy, chúng tôi đánh giá lại chi phí của cây từ gốc mới và trừ đi tổng chi phí di chuyển. Mục tiêu là chọn một chuỗi dịch chuyển gốc để tối đa hóa lợi nhuận cuối cùng. 

Đầu vào là một cây, do đó không có chu trình và có chính xác một đường đi đơn giản giữa bất kỳ cặp đỉnh nào. Cấu trúc này rất quan trọng vì mỗi chuyển động của rễ thực sự là một bước đi trên cây này. 

Các ràng buộc buộc một giải pháp tuyến tính hoặc gần tuyến tính. Với tổng số lên tới 200.000 đỉnh trong các trường hợp thử nghiệm, bất kỳ giải pháp nào tính toán lại khoảng cách từ đầu cho mọi gốc có thể đều sẽ thất bại. Ngay cả BFS hoặc DFS cho mỗi nghiệm gốc ứng cử viên cũng dẫn đến hành vi bậc hai, vượt xa mức có thể chấp nhận được. 

Một vấn đề tế nhị xuất hiện khi suy nghĩ tham lam. Một ý tưởng ngây thơ là tiếp tục di chuyển rễ về phía những phần sâu hơn của cây để tăng chiều cao. Điều này không thành công vì mỗi bước di chuyển đều phải trả giá và việc tăng độ sâu không nhất thiết làm tăng độ lệch tâm tổng thể của gốc. 

Một cạm bẫy phổ biến khác là cho rằng nghiệm cuối cùng tốt nhất phải là trọng tâm hoặc điểm giữa của đường kính. Điều này là chưa đủ vì chúng tôi không chọn gốc tĩnh mà trả tiền để đi tới nó. 

Khó khăn thực sự là chi phí di chuyển phụ thuộc vào đường đi, trong khi chi phí của cây chỉ phụ thuộc vào gốc cuối cùng. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ là xem xét mọi chuỗi động tác gốc có thể xảy ra. Từ một gốc nhất định, chúng ta thử di chuyển đến bất kỳ hàng xóm nào, khám phá đệ quy tất cả các trạng thái và tính toán lợi nhuận thu được. Điều này ngay lập tức bùng nổ, bởi vì không gian trạng thái về cơ bản là tất cả các bước đi trên cây, có chiều sâu theo cấp số nhân. 

Ngay cả khi chúng ta giới hạn bản thân ở các vị trí gốc cuối cùng, chúng ta vẫn cần tính toán, cho mỗi nút, khoảng cách đến nút xa nhất (độ lệch tâm của nó). Phần đó đã là O(n) trên mỗi nút sử dụng BFS hoặc DFS, dẫn đến O(n²) trên mỗi trường hợp thử nghiệm. 

Quan sát quan trọng là chúng ta không bao giờ cần mô phỏng rõ ràng tất cả các đường chuyển động. Chi phí di chuyển gốc tỷ lệ thuận với số cạnh đi qua, vì vậy nếu chúng ta cố định một gốc cuối cùng, chi phí di chuyển chính xác là khoảng cách trong cây từ 1 đến gốc đó, nhân với c. 

Vì vậy, vấn đề giảm xuống còn việc chọn nút v để cực đại hóa: 

lợi nhuận(v) = độ lệch tâm(v) * k - dist(1, v) * c 

Đây hiện là tối ưu hóa kiểu DP của cây trên các nút, trong đó chúng tôi cần hai đại lượng cho mỗi nút: khoảng cách từ gốc ban đầu và độ lệch tâm của nó. 

Khoảng cách từ gốc 1 dễ dàng thông qua DFS hoặc BFS. Độ lệch tâm đòi hỏi thủ thuật cây cổ điển: hai lần duyệt BFS. Trước tiên, hãy tìm một điểm cuối của đường kính, sau đó từ đó tìm nút xa nhất và sau đó tính khoảng cách từ cả hai điểm cuối. Độ lệch tâm của bất kỳ nút nào là khoảng cách tối đa của nó đến hai điểm cuối đường kính. 

Khi các giá trị này được tính toán trước, việc đánh giá mọi nút sẽ trở nên tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các con đường | Hàm mũ | O(n) | Quá chậm | 
| Tính độ lệch tâm + đánh giá tất cả các nghiệm | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Chạy BFS/DFS từ nút 1 để tính dist1[v], khoảng cách từ gốc ban đầu đến mọi nút v. Điều này thể hiện tổng chi phí của việc di chuyển gốc đến v, vì mỗi lần di chuyển là một cạnh và chi phí là c. 
2. Tìm một điểm cuối của đường kính cây bằng cách chạy BFS từ bất kỳ nút nào, thường là nút 1. Gọi điểm cuối này là a. 
3. Chạy BFS từ a để tính khoảng cách da[v]. Sau đó tìm nút xa nhất từ ​​a, gọi nó là b. 
4. Chạy một BFS khác từ b để tính khoảng cách db[v]. 
5. Với mỗi nút v, hãy tính độ lệch tâm của nó là max(da[v], db[v]). Điều này hiệu quả vì trong cây, nút xa nhất so với v luôn là một trong những điểm cuối đường kính. 
6. Với mỗi nút v, hãy tính ứng viên lợi nhuận là ecc[v] * k - dist1[v] * c. 
7. Trả về giá trị tối đa trên tất cả các nút. 

Lý do chúng ta có thể hạn chế tính toán độ lệch tâm đối với các điểm cuối đường kính là vì trong một cây, tất cả các đường đi dài nhất đều đi qua đường kính, do đó, bất kỳ nút xa nhất nào cũng phải nằm ở một trong các đầu của nó. 

### Tại sao nó hoạt động 

Thuật toán tách biệt hai tác động cạnh tranh một cách rõ ràng: việc di chuyển gốc có chi phí tuyến tính dọc theo các cạnh của cây, trong khi lợi ích chỉ phụ thuộc vào khoảng cách giữa gốc mới và lá xa nhất. Cả hai đại lượng đều dựa trên đường dẫn và có thể được ghi lại hoàn toàn bằng khoảng cách BFS. Các điểm cuối đường kính cung cấp một cấu trúc tổng thể giới hạn tất cả các độ lệch tâm, đảm bảo rằng không có khoảng cách xa nhất của nút nào bị bỏ sót. Bởi vì mọi trạng thái cuối cùng có thể có đều được biểu thị duy nhất bởi một nút và mỗi nút như vậy được đánh giá chính xác một lần, nên giá trị tính toán tối đa là tối ưu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

def bfs(start, adj):
    n = len(adj) - 1
    dist = [-1] * (n + 1)
    q = deque([start])
    dist[start] = 0
    while q:
        u = q.popleft()
        for v in adj[u]:
            if dist[v] == -1:
                dist[v] = dist[u] + 1
                q.append(v)
    return dist

t = int(input())
for _ in range(t):
    n, k, c = map(int, input().split())
    adj = [[] for _ in range(n + 1)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        adj[u].append(v)
        adj[v].append(u)

    dist1 = bfs(1, adj)

    a = max(range(1, n + 1), key=lambda x: dist1[x])
    da = bfs(a, adj)
    b = max(range(1, n + 1), key=lambda x: da[x])
    db = bfs(b, adj)

    ans = 0
    for v in range(1, n + 1):
        ecc = max(da[v], db[v])
        ans = max(ans, ecc * k - dist1[v] * c)

    print(ans)
```BFS đầu tiên khắc phục chi phí để đạt được bất kỳ gốc cuối cùng tiềm năng nào. Phép tính BFS thứ hai và thứ ba xác định vị trí các điểm cuối đường kính, cho phép tính độ lệch tâm theo thời gian không đổi trên mỗi nút. 

Vòng lặp cuối cùng đánh giá trực tiếp mọi gốc có thể. Phép nhân với k và c được giữ nguyên ở dạng số học số nguyên, tránh các vấn đề về độ chính xác. 

Một chi tiết triển khai tinh tế là chúng tôi không tính toán lại BFS từ mọi nút, vì đó sẽ là phương trình bậc hai. Thay vào đó, chúng tôi sử dụng lại cấu trúc đường kính, giúp giảm tính toán độ lệch tâm về thời gian tuyến tính. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2 3
2 1
3 1
```Chúng tôi tính toán khoảng cách từ nút 1: 

| Nút | quận 1 | 
| --- | --- | 
| 1 | 0 | 
| 2 | 1 | 
| 3 | 1 | 

Điểm cuối đường kính là 2 và 3. Khoảng cách: 

| Nút | da (từ 2) | db (từ 3) | ecc | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 
| 2 | 0 | 2 | 2 | 
| 3 | 2 | 0 | 2 | 

Giá trị lợi nhuận: 

| Nút | ecc*k | dist1*c | lợi nhuận | 
| --- | --- | --- | --- | 
| 1 | 2 | 0 | 2 | 
| 2 | 4 | 3 | 1 | 
| 3 | 4 | 3 | 1 | 

Tối đa là 2 tại nút 1. 

Điều này cho thấy rằng mặc dù việc di chuyển làm tăng khoảng cách từ một số lá nhưng chi phí di chuyển lại chiếm ưu thế ngay lập tức. 

### Ví dụ 2 

Cây dài hơn:```
5 4 1
2 1
4 2
5 4
3 4
```Khoảng cách từ 1: 

| Nút | quận 1 | 
| --- | --- | 
| 1 | 0 | 
| 2 | 1 | 
| 4 | 2 | 
| 5 | 3 | 
| 3 | 3 | 

Điểm cuối đường kính là 5 và 3. Độ lệch tâm: 

| Nút | ecc | 
| --- | --- | 
| 1 | 3 | 
| 2 | 2 | 
| 4 | 2 | 
| 5 | 3 | 
| 3 | 3 | 

Lợi nhuận: 

| Nút | ecc*k | dist1*c | lợi nhuận | 
| --- | --- | --- | --- | 
| 1 | 12 | 0 | 12 | 
| 2 | 8 | 1 | 7 | 
| 4 | 8 | 2 | 6 | 
| 5 | 12 | 3 | 9 | 
| 3 | 12 | 3 | 9 | 

Tốt nhất là 12 tại nút 1. 

Điều này minh họa rằng mặc dù hiện có các nút có độ lệch tâm cao nhưng chi phí để tiếp cận chúng sẽ làm giảm lợi nhuận ròng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi BFS chạy theo thời gian tuyến tính và chúng tôi thực hiện ba lần duyệt BFS cộng với một lần quét tuyến tính | 
| Không gian | O(n) | Danh sách kề và mảng khoảng cách | 

Tổng của n trên tất cả các trường hợp thử nghiệm được giới hạn bởi 2e5, do đó tổng thời gian chạy nhìn chung vẫn giữ nguyên tuyến tính. Điều này thoải mái phù hợp trong các hạn chế. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import collections

    def solve():
        input = sys.stdin.readline

        def bfs(start, adj):
            n = len(adj) - 1
            dist = [-1] * (n + 1)
            q = collections.deque([start])
            dist[start] = 0
            while q:
                u = q.popleft()
                for v in adj[u]:
                    if dist[v] == -1:
                        dist[v] = dist[u] + 1
                        q.append(v)
            return dist

        t = int(input())
        out = []
        for _ in range(t):
            n, k, c = map(int, input().split())
            adj = [[] for _ in range(n + 1)]
            for _ in range(n - 1):
                u, v = map(int, input().split())
                adj[u].append(v)
                adj[v].append(u)

            dist1 = bfs(1, adj)
            a = max(range(1, n + 1), key=lambda x: dist1[x])
            da = bfs(a, adj)
            b = max(range(1, n + 1), key=lambda x: da[x])
            db = bfs(b, adj)

            ans = 0
            for v in range(1, n + 1):
                ecc = max(da[v], db[v])
                ans = max(ans, ecc * k - dist1[v] * c)

            out.append(str(ans))

        return "\n".join(out)

    return solve()

# provided samples
assert run("""4
3 2 3
2 1
3 1
5 4 1
2 1
4 2
5 4
3 4
6 5 3
4 1
6 1
2 6
5 1
3 2
10 6 4
1 3
1 9
9 7
7 6
6 4
9 2
2 8
8 5
5 10
""") == """2
12
17
32"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cây sao đơn | xử lý đường kính tối thiểu | tính đúng đắn của cấu trúc tầm thường | 
| cây xích | cấu trúc tuyến tính cực đoan | tính đúng đắn của độ lệch tâm | 
| cây cân bằng | nhiều nghiệm tối ưu bằng nhau | xử lý cà vạt | 

## Vỏ cạnh 

Cây có hình ngôi sao là trường hợp rõ ràng nhất mà trực giác có thể thất bại. Tâm có độ lệch tâm tối thiểu nhưng chi phí di chuyển bằng 0, trong khi các lá có độ lệch tâm tối đa nhưng chi phí di chuyển cao. Thuật toán đánh giá chính xác cả hai và lựa chọn dựa trên lợi nhuận ròng. 

Trong biểu đồ đường dẫn, mọi nút đều nằm trên đường kính, do đó độ lệch tâm bằng khoảng cách đến điểm cuối này hoặc điểm cuối khác. Phương thức BFS-từ-đường kính-kết thúc chỉ định chính xác cho mỗi nút khoảng cách xa nhất mà không cần BFS trên mỗi nút. 

Khi k nhỏ so với c, việc di chuyển gốc không bao giờ có lợi. Thuật toán vẫn xử lý việc này vì thuật ngữ dist1 * c chi phối tất cả mức tăng và mức tối đa tự nhiên xảy ra ở nút 1.
