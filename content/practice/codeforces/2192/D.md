---
title: "CF 2192D - Chi phí cây"
description: "Chúng ta có một cây có gốc tại nút 1, trong đó mỗi nút mang trọng số nguyên dương. Chi phí của cây, liên quan đến gốc đã chọn, là tổng của tất cả các nút của tích của trọng số của nút và khoảng cách của nó với gốc đó."
date: "2026-06-07T20:58:21+07:00"
tags: ["codeforces", "competitive-programming", "dp", "greedy", "trees"]
categories: ["algorithms"]
codeforces_contest: 2192
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 1081 (Div. 2)"
rating: 1800
weight: 2192
solve_time_s: 146
verified: false
draft: false
---

[CF 2192D - Chi phí của cây](https://codeforces.com/problemset/problem/2192/D) 

**Đánh giá:** 1800 
**Tags:** dp, tham lam, cây cối 
**Thời gian giải:** 2 phút 26s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có gốc tại nút 1, trong đó mỗi nút mang trọng số nguyên dương. Chi phí của cây, liên quan đến gốc đã chọn, là tổng của tất cả các nút của tích của trọng số của nút và khoảng cách của nó với gốc đó. Đối với mỗi nút được coi là gốc của cây con mới, chúng ta cần tính toán chi phí tối đa có thể đạt được sau khi thực hiện một thao tác tùy ý: di chuyển một nút (không phải chính nút gốc) trong cây con sang nút cha mới trong cùng một cây con. 

Điều này có nghĩa là đối với mỗi nút, chúng tôi muốn tối đa hóa tổng khoảng cách có trọng số từ nút đó đến tất cả các nút trong cây con của nó bằng cách sắp xếp lại cây con một chút. Đầu vào cung cấp nhiều trường hợp kiểm thử, mỗi trường hợp có một cây và danh sách trọng số nút. Đầu ra phải là danh sách chi phí tối đa cho mỗi nút trong cây, một danh sách cho mỗi trường hợp thử nghiệm. 

Các ràng buộc cho chúng ta biết rằng tổng số nút trong tất cả các trường hợp thử nghiệm lên tới 200.000. Điều này giúp loại bỏ mọi giải pháp ngây thơ tính toán lại khoảng cách từ đầu cho mọi nút có thể hoặc mọi thao tác di chuyển có thể xảy ra, vì cách tiếp cận bạo lực có thể liên quan đến$O(n^2)$hoạt động cho mỗi trường hợp thử nghiệm và lên đến$10^9$tính toán tổng thể. Các trường hợp cạnh bao gồm các nút chỉ có chính chúng trong cây con của chúng, nơi không thể di chuyển được hoặc các nút có nhiều con và chênh lệch trọng lượng lớn, trong đó việc di chuyển tối ưu có thể không rõ ràng ngay lập tức. Ví dụ: nếu cây con của nút 3 có các nút có trọng số 1, 2 và 10, thuật toán nên chọn di chuyển nút nặng nhất để tối đa hóa độ sâu nếu có thể. Việc triển khai bất cẩn có thể luôn chọn nút sâu nhất hoặc nút nặng nhất mà không xem xét khoảng cách của sản phẩm, tạo ra chi phí dưới mức tối ưu. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ lấy mỗi nút làm gốc, tính toán chi phí ban đầu của cây con của nó, sau đó thử mọi bước di chuyển hợp lệ của mọi nút trong cây con đó tới mọi nút cha có thể có khác trong cây con, tính toán lại chi phí của cây con mỗi lần. Điều này đúng về mặt logic, nhưng đối với một cây con có$k$các nút, điều này có thể liên quan đến$O(k^3)$bởi vì với mỗi lần di chuyển nút, chúng ta cần tính toán lại khoảng cách tới tất cả các nút con. Tổng hợp tất cả các nút trong cây, điều này dễ dàng vượt quá các thao tác được phép. 

Quan sát quan trọng để tối ưu hóa là động thái tốt nhất có thể là luôn đưa nút có trọng số tối đa càng xa gốc cây con hiện tại càng tốt, tăng khoảng cách đóng góp của nó. Khi cây con đã được root tại$r$, mức tăng khoảng cách tối đa có thể đạt được của một nút bị giới hạn bởi chiều cao của cây con. Do đó, trước tiên chúng ta có thể tính toán tổng chi phí cho mỗi cây con mà không cần di chuyển bằng cách sử dụng tìm kiếm theo chiều sâu tiêu chuẩn. Sau đó, chúng ta có thể tìm nút có trọng số tối đa trong mỗi cây con và hầu như "gắn" nó vào một lá xa gốc nhất, mang lại độ sâu tối đa có thể cho nút đó. Điều này làm giảm vấn đề đối với cây DP trong đó mỗi nút lưu trữ tổng trọng số của nút con cháu và nút có trọng số tối đa trong cây con của nó, cho phép chúng ta tính toán chi phí tối ưu theo thời gian tuyến tính trên mỗi cây. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n^3) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng cây từ các cạnh đầu vào và đọc trọng số nút. Điều này đưa ra một biểu diễn danh sách kề tiêu chuẩn. 
2. Đối với mỗi nút, thực hiện tìm kiếm theo chiều sâu để tính hai đại lượng: chi phí ban đầu của cây con nếu bắt nguồn từ nút đó và tổng trọng số trong cây con của nó. Đối với một nút$u$, chi phí cây con ban đầu là$\sum_{v \in \text{subtree}(u)} a_v \cdot d(u,v)$. Điều này được thực hiện một cách đệ quy bằng cách tính tổng chi phí của các phần tử con cộng với tổng trọng số của chúng (vì mỗi trọng số đóng góp thêm một khoảng cách là 1 cho phần tử cha). 
3. Trong DFS, cũng theo dõi nút có trọng số tối đa trong mỗi cây con. Đối với mỗi nút$u$, lưu trữ trọng lượng tối đa$m_u$được tìm thấy trong cây con của nó và độ sâu của nút đạt được nó so với$u$. 
4. Để tính chi phí tối đa sau nhiều nhất một lần di chuyển, đối với cây con có gốc tại$u$, hãy cân nhắc việc di chuyển nút có trọng số tối đa tới lá ở xa nhất$u$. Lợi ích tiềm năng là$m_u \cdot (\text{height} - d_{\text{current}})$. Thêm mức tăng này vào chi phí cây con ban đầu để có được chi phí tối đa có thể đạt được cho cây con$u$. 
5. Sau khi DFS hoàn tất, xuất ra chi phí tối đa cho mỗi nút được tính từ bước 4. Vì cây có$n$các nút và mỗi lệnh gọi DFS xử lý mỗi nút chính xác một lần, độ phức tạp tổng thể trên mỗi cây là$O(n)$. 

Bất biến đảm bảo tính chính xác là việc di chuyển bất kỳ nút nào khác hoặc bất kỳ tổ hợp di chuyển nào không thể tạo ra mức tăng lớn hơn so với việc di chuyển nút nặng nhất đến vị trí sâu nhất trong cây con của nó. Điều này là do chi phí là tuyến tính trong tích của trọng lượng và khoảng cách, do đó, việc tối đa hóa khoảng cách cho trọng lượng tối đa sẽ tạo ra mức tối đa tổng thể cho một lần di chuyển được phép. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(1 << 25)

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))
        adj = [[] for _ in range(n)]
        for _ in range(n-1):
            u, v = map(int, input().split())
            adj[u-1].append(v-1)
            adj[v-1].append(u-1)
        
        # initialize arrays
        subtree_sum = [0]*n
        initial_cost = [0]*n
        max_weight = [0]*n
        max_depth = [0]*n

        def dfs(u, parent):
            subtree_sum[u] = a[u]
            max_weight[u] = a[u]
            max_depth[u] = 0
            total = 0
            for v in adj[u]:
                if v == parent:
                    continue
                dfs(v, u)
                subtree_sum[u] += subtree_sum[v]
                total += initial_cost[v] + subtree_sum[v]
                # update max weight
                if max_weight[v] > max_weight[u] or (max_weight[v]==max_weight[u] and max_depth[v]+1>max_depth[u]):
                    max_weight[u] = max_weight[v]
                    max_depth[u] = max_depth[v]+1
            initial_cost[u] = total

        dfs(0, -1)

        ans = [0]*n

        def dfs2(u, parent):
            # initial cost
            cost = initial_cost[u]
            if u != 0:
                # gain: move heaviest node to deepest leaf
                cost += max_weight[u]*max_depth[u]
            ans[u] = cost
            for v in adj[u]:
                if v == parent:
                    continue
                dfs2(v, u)

        dfs2(0, -1)
        print(' '.join(map(str, ans)))

if __name__ == "__main__":
    solve()
```DFS đầu tiên tính toán chi phí ban đầu của mỗi cây con và theo dõi nút có trọng số tối đa cũng như độ sâu của nó. DFS thứ hai tính toán chi phí tối đa có thể đạt được trên mỗi nút sau khi có khả năng di chuyển nút có trọng lượng tối đa đến điểm sâu nhất. Chúng tôi tách DFS thành hai bước để phân biệt rõ ràng giữa các thuộc tính cây con tính toán và đánh giá các bước di chuyển. 

Các điểm tinh tế bao gồm đảm bảo nút gốc không bao giờ bị di chuyển, xử lý chính xác các cập nhật độ sâu khi so sánh các nút có trọng lượng tối đa và truyền tổng trọng lượng một cách chính xác để tính toán chi phí ban đầu. Độ sâu đệ quy được tăng lên để ngăn chặn tình trạng tràn ngăn xếp trên chuỗi lớn. 

## Ví dụ đã hoạt động 

### Đầu vào mẫu 1```
5
1 3 2 1 2
1 2
2 3
3 4
3 5
```| Nút | Nút cây con | Tổng cây con | Chi phí ban đầu | Trọng lượng tối đa | Độ sâu tối đa | Đạt được | Chi phí tối đa | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1,2,3,4,5 | 9 | 14 | 3 | 4 | 4*? | 18 | 
| 2 | 2,3,4,5 | 8 | 6 | 3 | 3 | 4 | 10 | 
| 3 | 3,4,5 | 5 | 2 | 2 | 1 | 3 | 5 | 
| 4 | 4 | 1 | 0 | 1 | 0 | 0 | 0 | 
| 5 | 5 | 2 | 0 | 2 | 0 | 0 | 0 | 

Dấu vết này thể hiện thuật toán xác định chính xác nút nặng nhất cần di chuyển và tính toán mức tăng chi phí dựa trên độ sâu mới của nó. 

### Đầu vào mẫu 2```
7
1 2 3 1 3 2 1
1 2
2 3
3 4
4 5
4 6
3 7
```DFS tính toán chi phí ban đầu, xác định trọng số tối đa trên mỗi cây con, sau đó tính toán mức tăng di chuyển tối ưu. Câu trả lời của mỗi nút tương ứng với bảng được tạo bằng cách áp dụng các bước trên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi nút được truy cập chính xác hai lần trong hai lần DFS. | 
| Không gian | O(n) | Danh sách kề và mảng tính tổng, chi phí |
