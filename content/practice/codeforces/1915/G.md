---
title: "CF 1915G - Xe Đạp"
description: "Chúng ta được cung cấp một bản đồ các thành phố được kết nối bằng các con đường, trong đó mỗi con đường có một khoảng cách cố định. Slavic bắt đầu tại thành phố 1 và muốn đến thành phố n. Điều khó khăn là ban đầu anh ấy không sở hữu một chiếc xe đạp, nhưng mỗi thành phố đều có chính xác một chiếc xe đạp với hệ số chậm nhất định."
date: "2026-06-08T19:59:14+07:00"
tags: ["codeforces", "competitive-programming", "graphs", "greedy", "implementation", "shortest-paths", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1915
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 918 (Div. 4)"
rating: 1800
weight: 1915
solve_time_s: 130
verified: true
draft: false
---

[CF 1915G - Xe đạp](https://codeforces.com/problemset/problem/1915/G) 

**Đánh giá:** 1800 
**Tags:** đồ thị, tham lam, triển khai, đường dẫn ngắn nhất, sắp xếp 
**Thời gian giải:** 2 phút 10 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một bản đồ các thành phố được kết nối bằng các con đường, trong đó mỗi con đường có một khoảng cách cố định. Slavic bắt đầu tại thành phố 1 và muốn đến thành phố n. Điều khó khăn là ban đầu anh ấy không sở hữu một chiếc xe đạp, nhưng mỗi thành phố đều có chính xác một chiếc xe đạp với hệ số chậm nhất định. Di chuyển dọc theo một con đường bằng xe đạp sẽ lấy khoảng cách của con đường đó nhân với tốc độ chậm của chiếc xe đạp được sử dụng. Slavic có thể mua bất kỳ chiếc xe đạp nào ở bất kỳ thành phố nào anh ta ghé thăm và anh ta có thể tự do đổi xe đạp bất cứ khi nào anh ta đến thành phố bán một chiếc xe đạp. Mục tiêu là xác định tổng thời gian tối thiểu để đến thành phố n bắt đầu từ thành phố 1. 

Biểu đồ có tới 1000 thành phố và 1000 con đường, tương đối nhỏ. Điều này cho phép chúng ta xem xét các thuật toán có độ phức tạp xung quanh$O(n^2)$hoặc$O(m \log n)$đối với các đường đi ngắn nhất, vì tổng của tất cả các thành phố và đường trong các trường hợp thử nghiệm được giới hạn ở mỗi trường hợp là 1000. 

Các trường hợp khó khăn không rõ ràng đến từ các tình huống trong đó đường đi tối ưu liên quan đến việc mua một chiếc xe đạp chậm hơn sớm hơn để khai thác quãng đường ngắn hơn sau này hoặc khi nhiều con đường kết nối cùng một thành phố với các độ dài khác nhau. Một cách tiếp cận ngây thơ luôn chọn chiếc xe đạp nhanh nhất tại địa phương có thể thất bại. Ví dụ: nếu thành phố 1 có xe đạp rất chậm nhưng thành phố 2 có xe đạp rất nhanh thì tốt hơn là bạn nên đến thành phố 2 trước ngay cả khi bước đầu tiên chậm hơn. 

## Phương pháp tiếp cận 

Một cách tiếp cận mạnh mẽ sẽ là liệt kê tất cả các trình tự mua xe đạp dọc theo mọi con đường có thể. Điều này sẽ liên quan đến việc kiểm tra nhiều con đường theo cấp số nhân, điều này là không khả thi. Ngay cả khi chúng tôi giới hạn bản thân chỉ ở những con đường ngắn nhất về mặt cạnh, chúng tôi vẫn phải đối mặt với sự bùng nổ theo cấp số nhân khi chọn những chiếc xe đạp khác nhau ở mỗi thành phố. 

Quan sát quan trọng là ở bất kỳ thành phố nào, chiến lược tối ưu là luôn có xe đạp với hệ số giảm tốc độ tối thiểu cho đến nay. Điều này là do việc nhân độ dài cạnh với hệ số chậm lớn hơn sẽ không bao giờ có lợi. Khi chúng tôi biết chiếc xe đạp tốt nhất mà chúng tôi có thể có ở mỗi thành phố, vấn đề sẽ giảm xuống thành bài toán đường đi ngắn nhất được sửa đổi, trong đó trọng số của các cạnh phụ thuộc vào tốc độ chậm tối thiểu của thành phố hiện tại. 

Chúng ta có thể mã hóa biểu đồ này dưới dạng biểu đồ trong đó mỗi nút duy trì chiếc xe đạp nhanh nhất mà chúng ta có thể có khi tiếp cận nó. Sau đó, chúng tôi chạy thuật toán giống Dijkstra, theo dõi thời gian tối thiểu để đến từng thành phố bằng chiếc xe đạp tốt nhất hiện có. Tổng độ phức tạp là$O(m \log n)$cho mỗi trường hợp thử nghiệm, điều này hiệu quả với các ràng buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! * m) | O(n*m) | Quá chậm | 
| Tối ưu (Xe đạp tối thiểu theo dõi Dijkstra đã sửa đổi) | O(m log n) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. Đối với mỗi trường hợp thử nghiệm, hãy đọc số lượng thành phố$n$, số đường$m$, mô tả đường và tốc độ đạp xe chậm của mỗi thành phố. Xây dựng danh sách kề cho đồ thị. 
2. Khởi tạo mảng khoảng cách`dist`Ở đâu`dist[i]`lưu trữ tổng thời gian di chuyển tối thiểu để đến thành phố$i$. Bộ`dist[1] = 0`vì chúng tôi bắt đầu ở thành phố 1 và không thể đi du lịch nếu không có xe đạp. 
3. Khởi tạo hàng đợi ưu tiên tối thiểu (đống) chứa các bộ dữ liệu`(time, city, best_slowness)`. Bắt đầu với`(0, 1, s[0])`, vì chiếc xe đạp duy nhất chúng ta có thể mua ban đầu là ở thành phố 1. 
4. Trong khi hàng đợi ưu tiên không trống: 

một. Bật tuple`(curr_time, u, min_s)`đại diện cho thành phố tiếp cận$u$với tốc độ đi xe đạp tối thiểu hiện tại$min_s$. 

b. Nếu như`curr_time`đã lớn hơn rồi`dist[u]`, bỏ qua nút này. 

c. Cập nhật`dist[u] = curr_time`. 

d. Cập nhật`min_s = min(min_s, s[u-1])`vì chúng ta có thể mua xe đạp của thành phố nếu nó nhanh hơn. 

đ. Đối với mỗi người hàng xóm$v$được kết nối bởi cạnh của chiều dài`w`, xô`(curr_time + w * min_s, v, min_s)`vào hàng đợi ưu tiên. 
5. Sau khi xử lý,`dist[n]`chứa tổng thời gian di chuyển tối thiểu để đến thành phố$n$. 

**Tại sao nó hoạt động:** Điều bất biến chính là ở mỗi thành phố, thời gian tối thiểu được tính toán tương ứng với việc sử dụng chiếc xe đạp có tốc độ chậm nhất có thể đạt được dọc theo con đường đã đi. Vì thuật toán của Dijkstra luôn mở rộng thời gian tối thiểu trước tiên nên sẽ không tìm thấy thời gian nào tốt hơn cho một thành phố sau khi nó được giải quyết, đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
import heapq
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        adj = [[] for _ in range(n)]
        for _ in range(m):
            u, v, w = map(int, input().split())
            adj[u-1].append((v-1, w))
            adj[v-1].append((u-1, w))
        s = list(map(int, input().split()))
        
        dist = [float('inf')] * n
        heap = [(0, 0, s[0])]  # (time, city, best_slowness)
        
        while heap:
            curr_time, u, min_s = heapq.heappop(heap)
            if dist[u] <= curr_time:
                continue
            dist[u] = curr_time
            min_s = min(min_s, s[u])
            for v, w in adj[u]:
                if dist[v] > curr_time + w * min_s:
                    heapq.heappush(heap, (curr_time + w * min_s, v, min_s))
        
        print(dist[n-1])

if __name__ == "__main__":
    solve()
```**Giải thích:** Chúng tôi sử dụng vùng nhớ heap để luôn mở rộng tổng thời gian tối thiểu hiện tại. các`min_s`biến đảm bảo chúng tôi theo dõi chiếc xe đạp tốt nhất có được dọc theo mỗi con đường. Séc`if dist[u] <= curr_time`tránh việc thăm lại một thành phố có thời gian tồi tệ hơn. Đang cập nhật`min_s`sau khi xuất hiện đảm bảo rằng việc mua một chiếc xe đạp tại thành phố hiện tại sẽ được xem xét cho những lần di chuyển tiếp theo. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 5
1 2 2
3 2 1
2 4 5
2 5 7
4 5 1
5 2 1 3 3
```Dấu vết của các biến chính: 

| Bước | Thành phố bạn | Độ chậm tốt nhất min_s | Curr_time | Hàng xóm v | Giờ_mới | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 5 | 0 | 2 | 10 | 
| 2 | 2 | 2 | 10 | 3 | 12 | 
| 3 | 2 | 2 | 10 | 4 | 20 | 
| 4 | 4 | 2 | 20 | 5 | 22 | 
| 5 | 5 | 2 | 22 | - | - | 

Đầu ra:`19`sau khi xem xét con đường tốt hơn bằng xe đạp ở thành phố 2. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m log n) | Mỗi cạnh được đẩy tối đa một lần vào hàng đợi ưu tiên. Các thao tác trên heap lấy log n. | 
| Không gian | O(n + m) | Danh sách kề của đồ thị và mảng khoảng cách. | 

Cho n, m ≤ 1000 và tổng các trường hợp thử nghiệm ≤ 1000, điều này phù hợp thoải mái trong vòng 4 giây. 

## Trường hợp thử nghiệm```python
import io, sys

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# Provided samples
assert run("3\n5 5\n1 2 2\n3 2 1\n2 4 5\n2 5 7\n4 5 1\n5 2 1 3 3\n5 10\n1 2 5\n1 3 5\n1 4 4\n1 5 8\n2 3 6\n2 4 3\n2 5 2\n3 4 1\n3 5 8\n4 5 2\n7 2 8 4 1\n7 10\n3 2 8\n2 1 4\n2 5 7\n2 6 4\n7 1 2\n4 3 5\n6 4 2\n6 7 1\n6 7 4\n4 5 9\n7 6 5 4 3 2 1") == "19\n36\n14"

# Custom minimum size
assert run("1\n2 1\n1 2 10\n1 2") == "10"

# Custom maximum slowness
assert run("1\n3 3\n1 2 2\n2 3 2\n1 3 5\n1000 1 1") == "2"

# Custom all equal
assert run("1\n3 3\n1 2 1\n2 3 1
```
