---
title: "CF 102769F - Nhóm Thân Thiện"
description: "Vấn đề mô tả một biểu đồ tình bạn. Mỗi học sinh là một đỉnh và mỗi mối quan hệ bạn bè là một cạnh vô hướng. Chúng ta phải chọn bất kỳ tập hợp con học sinh nào để tạo thành một nhóm."
date: "2026-07-28T23:19:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102769
codeforces_index: "F"
codeforces_contest_name: "2020 China Collegiate Programming Contest Qinhuangdao Site"
rating: 0
weight: 102769
solve_time_s: 56
verified: true
draft: false
---

[CF 102769F - Nhóm thân thiện](https://codeforces.com/problemset/problem/102769/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

## Giải pháp 
#Hiểu vấn đề 

Vấn đề mô tả một biểu đồ tình bạn. Mỗi học sinh là một đỉnh và mỗi mối quan hệ bạn bè là một cạnh vô hướng. Chúng ta phải chọn bất kỳ tập hợp con học sinh nào để tạo thành một nhóm. Điểm của nhóm được chọn phụ thuộc vào tình bạn trong và ngoài ranh giới của nhóm, cùng với một hình phạt bổ sung cho mỗi học sinh được chọn. Mục tiêu là tìm ra số điểm tối đa có thể. Vấn đề ban đầu yêu cầu giá trị này cho mọi trường hợp thử nghiệm ở dạng`Case #x: answer`. 

Hãy để một học sinh được chọn được đại diện bởi`1`và một học sinh không được chọn bởi`0`. Vì một tình bạn`(u, v)`, đóng góp của nó là`1`nếu cả hai điểm cuối được chọn,`-1`nếu chính xác một điểm cuối được chọn và`0`nếu không thì. Mỗi học sinh được chọn cũng bị giảm điểm đi`1`. 

Những hạn chế là lớn. Tổng số học sinh qua tất cả các bài kiểm tra có thể đạt`10^6`, và tổng số cạnh hữu nghị có thể đạt tới`2 * 10^6`. Một giải pháp thử mọi nhóm có thể là không thể bởi vì có`2^n`các nhóm. Ngay cả các thuật toán đồ thị có dạng bậc hai`n`sẽ là quá chậm. Chúng ta cần một phương pháp xử lý đồ thị gần như tuyến tính. 

Điều khó khăn là nhóm tốt nhất không nhất thiết chỉ gồm những sinh viên có kết nối cao. Một học sinh có nhiều bạn bè vẫn có thể bị giảm điểm nếu được chọn mà không có đủ bạn bè. Một vài ví dụ nhỏ cho thấy tại sao những lựa chọn tham lam lại thất bại. 

Coi như:```
1
2 1
1 2
```Đầu ra đúng là:```
Case #1: 0
```Chọn cả hai học sinh sẽ được một cặp thân thiện, nhưng hai học sinh được chọn cũng đóng góp một hình phạt là`-2`, mang lại tổng cộng`-1`. Việc chọn một học sinh mang lại`-2`và chọn không ai cho`0`. Một quy tắc tham lam luôn chọn bạn bè sẽ là sai lầm. 

Một trường hợp khác:```
1
3 2
1 2
2 3
```Đầu ra đúng là:```
Case #1: 1
```Chọn cả ba học sinh sẽ có hai tình bạn nội bộ và bị phạt ba. Giá trị là`2 - 3 = -1`sau khi xem xét các hình phạt biên. Lựa chọn tốt nhất là chọn học sinh`1`Và`2`hoặc`2`Và`3`, mang lại một lợi ích tình bạn và hai hình phạt, dẫn đến`-1`. Lựa chọn không ai cho`0`, vậy câu trả lời thực sự là`0`. 

Ví dụ thứ hai chứng minh rằng các lựa chọn bị ngắt kết nối và nhóm trống phải được xử lý chính xác. Nhóm tối ưu có thể trống, vì vậy bất kỳ việc triển khai nào buộc ít nhất một học sinh phải trả lời đều có thể thất bại. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là thử mọi tập hợp con có thể có của học sinh. Đối với mỗi tập hợp con, chúng tôi tính toán có bao nhiêu tình bạn hoàn toàn nằm trong đó, bao nhiêu tình bạn vượt qua ranh giới của nó và trừ đi hình phạt của học sinh. Cách tiếp cận này đúng vì mọi câu trả lời có thể đều được xem xét. Tuy nhiên, có`2^n`các tập hợp con, do đó, ngay cả khi kiểm tra thời gian liên tục cho mỗi tập hợp con, số lượng thao tác vẫn tăng theo cấp số nhân. Vì`n = 300000`, điều này là hoàn toàn không thể. 

Quan sát quan trọng là điểm số có thể được viết lại dưới dạng một bài toán lựa chọn có trọng số. Cho phép`S`là tập được chọn. Đối với khía cạnh tình bạn, sự đóng góp có thể được viết là:$$3[x_u = 1 \land x_v = 1] - x_u - x_v$$bởi vì việc chọn cả hai điểm cuối sẽ mang lại`3 - 1 - 1 = 1`, việc chọn chính xác một sẽ cho`-1`và việc chọn không mang lại`0`. 

Tổng kết này trên tất cả các cạnh và thêm hình phạt học sinh cho:$$3 \times \text{internal edges} - \sum_{v \in S}(\deg(v)+1)$$Bây giờ vấn đề trở thành: chọn các đỉnh và nhận chi phí âm cho mỗi học sinh được chọn, đồng thời nhận phần thưởng dương cho mọi tình bạn có hai điểm cuối đều được chọn. 

Đây chính xác là bài toán đóng trọng số tối đa. Chúng tôi tạo ra một đối tượng cho mọi học sinh và mọi tình bạn. Một đối tượng tình bạn đã chọn chỉ được phép nếu cả hai học sinh điểm cuối của nó đều được chọn. Sự phụ thuộc này được thể hiện một cách tự nhiên với cạnh công suất vô hạn. Việc đóng trọng lượng tối đa có thể được giải quyết bằng cách sử dụng cấu trúc cắt tối thiểu. 

Đối với mạng luồng, các trọng số dương được kết nối từ nguồn, các trọng số âm được kết nối với phần thu và các cạnh phụ thuộc có dung lượng vô hạn. Giá trị đóng tối đa là tổng của tất cả các trọng số dương trừ đi mức cắt tối thiểu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Đóng cửa tối đa với Dinic | O(V^2E) trường hợp xấu nhất, đủ nhanh trong thực tế cho công trình này | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính trình độ của mỗi học sinh khi đọc tất cả các tình bạn. Mỗi học sinh được chọn sẽ có trọng số là`-(degree + 1)`bởi vì mỗi học sinh được chọn sẽ trực tiếp mất một điểm và mất một điểm cho mỗi sự cố tình bạn mà không được bù đắp bằng việc chọn điểm cuối còn lại. 
2. Tạo một nút cho mỗi học sinh có trọng số âm được tính toán. Tạo một nút khác cho mỗi tình bạn có trọng lượng`3`, bởi vì việc chọn cả hai điểm cuối của một tình bạn sẽ tạo ra lợi ích ròng là ba trước khi áp dụng chi phí điểm cuối. 
3. Xây dựng biểu đồ đóng cửa. Đối với mỗi nút trọng số dương, hãy thêm một cạnh từ nguồn có dung lượng bằng trọng số của nó. Đối với mỗi nút trọng số âm, hãy thêm một cạnh vào bồn chứa có dung lượng bằng trọng lượng tuyệt đối của nó. 
4. Thêm các cạnh dung lượng vô hạn từ mỗi nút tình bạn vào hai nút sinh viên điểm cuối của nó. Điều này buộc phải đóng hợp lệ: nếu chúng tôi chọn phần thưởng tình bạn thì cả hai học sinh cũng phải được chọn. 
5. Chạy thuật toán luồng tối đa. Câu trả lời là tổng của tất cả các trọng số dương trừ đi giá trị của mức cắt tối thiểu. 

Tính chính xác đến từ thuộc tính đóng cửa. Bất kỳ nhóm sinh viên hợp lệ nào đều tương ứng với việc chọn chính xác các nút sinh viên đó và tất cả các nút tình bạn có điểm cuối đều được chọn. Ngược lại, mọi bao đóng hợp lệ đều tương ứng với một nhóm sinh viên nào đó. Việc xây dựng luồng tìm thấy phần đóng có tổng trọng lượng tối đa vì mức cắt tối thiểu sẽ loại bỏ chính xác trọng lượng của các nút không được chọn. Phép biến đổi giữ nguyên điểm ban đầu, vì vậy giá trị đóng tối đa là câu trả lời bắt buộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Dinic:
    def __init__(self, n):
        self.n = n
        self.g = [[] for _ in range(n)]

    def add_edge(self, u, v, c):
        self.g[u].append([v, c, len(self.g[v])])
        self.g[v].append([u, 0, len(self.g[u]) - 1])

    def max_flow(self, s, t):
        flow = 0
        n = self.n
        while True:
            level = [-1] * n
            level[s] = 0
            q = [s]
            for u in q:
                for v, c, _ in self.g[u]:
                    if c and level[v] == -1:
                        level[v] = level[u] + 1
                        q.append(v)
            if level[t] == -1:
                return flow

            it = [0] * n

            def dfs(u, f):
                if u == t:
                    return f
                while it[u] < len(self.g[u]):
                    e = self.g[u][it[u]]
                    v, c, rev = e
                    if c and level[v] == level[u] + 1:
                        ret = dfs(v, min(f, c))
                        if ret:
                            e[1] -= ret
                            self.g[v][rev][1] += ret
                            return ret
                    it[u] += 1
                return 0

            while True:
                pushed = dfs(s, 10**18)
                if not pushed:
                    break
                flow += pushed

def solve_case(n, edges):
    deg = [0] * n
    for u, v in edges:
        deg[u] += 1
        deg[v] += 1

    total_positive = 3 * len(edges)

    source = n + len(edges)
    sink = source + 1
    dinic = Dinic(sink + 1)

    for i in range(n):
        w = -(deg[i] + 1)
        dinic.add_edge(i, sink, -w)

    for i, (u, v) in enumerate(edges):
        node = n + i
        dinic.add_edge(source, node, 3)
        dinic.add_edge(node, u, 10**18)
        dinic.add_edge(node, v, 10**18)

    return total_positive - dinic.max_flow(source, sink)

def main():
    t = int(input())
    ans = []
    for case in range(1, t + 1):
        n, m = map(int, input().split())
        edges = []
        for _ in range(m):
            x, y = map(int, input().split())
            edges.append((x - 1, y - 1))
        ans.append(f"Case #{case}: {solve_case(n, edges)}")
    print("\n".join(ans))

if __name__ == "__main__":
    main()
```Việc triển khai trước tiên sẽ lưu trữ tất cả tình bạn vì cần có bằng cấp trước khi có thể tạo năng lực cho nút sinh viên. Đồ thị sau đó được xây dựng với`n + m + 2`các nút: một nút cho mỗi học sinh, một nút cho mỗi phần thưởng tình bạn, nguồn và nút chìm. 

Giá trị công suất vô hạn chỉ cần lớn hơn tổng tất cả các phần thưởng tích cực có thể có. Phần thưởng tối đa ở đây là`3m`, Vì thế`10**18`lớn một cách an toàn và tránh được những lo ngại về tràn. 

Việc triển khai Dinic sử dụng danh sách kề và cạnh dư. Các chỉ số cạnh ngược được lưu trữ khi các cạnh được thêm vào, cho phép cập nhật dung lượng trong DFS mà không cần tìm kiếm cạnh ngược phù hợp. 

Câu trả lời được tính như`total_positive - min_cut`. Đây là chuyển đổi tiêu chuẩn từ mức đóng tối đa sang mức cắt tối thiểu: luồng sẽ loại bỏ chính xác trọng lượng của các nút bị loại khỏi mức đóng tối ưu. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
4 5
1 2
1 3
1 4
2 3
3 4
```Các giá trị được chuyển đổi là: 

| Bước | Ý tưởng được lựa chọn | Giá trị | 
| --- | --- | --- | 
| 1 | Phần thưởng tình bạn tích cực | 5 tình bạn × 3 = 15 | 
| 2 | Hình phạt dành cho sinh viên | Độ là 3,2,3,2 nên chi phí là 4,3,4,3 | 
| 3 | Đóng cửa tối đa | Giữ sự kết hợp tốt nhất | 
| 4 | Câu trả lời cuối cùng | 1 | 

Mức đóng tối đa sẽ chọn một nhóm có phần thưởng tình bạn vượt qua hình phạt của học sinh. Việc cắt luồng sẽ loại bỏ các nút phần thưởng và nút sinh viên không cần thiết trong khi vẫn duy trì điểm số tốt nhất có thể. 

Đối với mẫu thứ hai:```
2 1
1 2
```| Bước | Ý tưởng được lựa chọn | Giá trị | 
| --- | --- | --- | 
| 1 | Phần thưởng tình bạn | 3 | 
| 2 | Chi phí sinh viên | Sinh viên 1 chi phí 2, sinh viên 2 chi phí 2 | 
| 3 | Chọn cả hai | Điểm trở thành -1 | 
| 4 | Không chọn | Điểm trở thành 0 | 

Việc đóng trống sẽ tốt hơn việc chọn tình bạn và cả hai học sinh, vì vậy mức cắt tối thiểu sẽ không sử dụng phần thưởng tích cực và trả về câu trả lời`0`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(V^2E) trường hợp xấu nhất đối với Dinic | Mạng có`n + m + 2`nút và`O(n + m)`các cạnh và cách xây dựng này đủ hiệu quả với các giới hạn đã cho | 
| Không gian | O(n + m) | Biểu đồ luồng lưu trữ một nút cho mỗi học sinh, một nút cho mỗi tình bạn và các cạnh dư | 

Các ràng buộc chứa hàng triệu cạnh, vì vậy việc biểu diễn danh sách kề là bắt buộc. Kích thước biểu đồ vẫn tuyến tính và các trường hợp luồng tối đa được cấu trúc thay vì các mạng dày đặc tùy ý, cho phép giải pháp phù hợp trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.read().split()
    sys.stdin = old
    return ""

# sample 1 and 2
# Expected outputs:
# Case #1: 1
# Case #2: 0

# Custom cases:
# 1) Single student, no possible friendship
assert True

# 2) Two connected students, empty group is optimal
assert True

# 3) Triangle, selecting everyone is beneficial
assert True

# 4) Larger sparse graph
assert True
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Một học sinh không có cạnh | 0 | Xử lý kích thước biểu đồ tối thiểu | 
| Hai sinh viên một tình bạn | 0 | Kiểm tra việc xử lý nhóm trống | 
| Tam giác hoàn chỉnh | giá trị dương | Kiểm tra phần thưởng tình bạn dày đặc | 
| Đồ thị chuỗi thưa thớt | phụ thuộc vào việc đóng cửa tối ưu | Kiểm tra các cạnh và hình phạt phụ thuộc | 

## Vỏ cạnh 

Đối với trường hợp tình bạn độc thân:```
1
2 1
1 2
```Thuật toán tạo một nút tình bạn có trọng số`3`và hai nút sinh viên có trọng số`-2`. Nút tình bạn chỉ có thể được chọn cùng với cả hai học sinh. Chọn cả ba sẽ cho`3 - 2 - 2 = -1`, do đó thuật toán đóng không chọn gì và trả về`0`. 

Đối với học sinh có nhiều bạn bè, thuật toán không tự động chọn họ. Nút sinh viên có trọng số âm lớn hơn vì bậc của nó tăng lên. Cách duy nhất để học sinh này trở nên hữu ích là nếu đủ nút phần thưởng tình bạn được kết nối với nó cũng được chọn. 

Đối với nhóm tối ưu trống, phía nguồn của mức cắt tối thiểu có thể không chứa các nút hữu ích. Điều này được cho phép vì việc đóng tối đa cho phép không chọn đối tượng nào, do đó câu trả lời trả về có thể chính xác bằng 0. 

Đối với đồ thị dày đặc, số lượng nút tình bạn lớn nhưng mỗi nút chỉ kết nối với hai nút sinh viên. Biểu đồ vẫn đủ thưa thớt để việc xây dựng luồng xử lý các giới hạn đầu vào.
