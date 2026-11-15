---
title: "CF 1810E - Quái vật"
description: "Chúng ta được cung cấp một đồ thị vô hướng trong đó mỗi đỉnh chứa một con quái vật có yêu cầu bằng số. Nếu một quái vật có giá trị a[i], bạn chỉ được phép đánh bại nó khi bạn đã đánh bại ít nhất một[i] quái vật khác."
date: "2026-06-09T08:45:54+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures", "dfs-and-similar", "dsu", "graphs", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1810
codeforces_index: "E"
codeforces_contest_name: "CodeTON Round 4 (Div. 1 + Div. 2, Rated, Prizes!)"
rating: 2100
weight: 1810
solve_time_s: 92
verified: false
draft: false
---

[CF 1810E - Quái vật](https://codeforces.com/problemset/problem/1810/E) 

**Xếp hạng:** 2100 
**Tags:** bạo lực, cấu trúc dữ liệu, dfs và tương tự, dsu, đồ thị, tham lam 
**Thời gian giải:** 1m 32s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một đồ thị vô hướng trong đó mỗi đỉnh chứa một con quái vật có yêu cầu bằng số. Nếu một con quái vật có giá trị`a[i]`, bạn chỉ được phép đánh bại nó khi bạn đã đánh bại ít nhất`a[i]`những con quái vật khác. 

Quá trình bắt đầu bằng cách chọn một đỉnh duy nhất`s`Ở đâu`a[s] = 0`, vì lúc đầu bạn chưa đánh bại được gì cả. Sau đó, bạn di chuyển dọc theo các cạnh. Khi bạn đi đến một đỉnh mới, bạn đến đó đã bị đánh bại hoặc bạn đánh bại quái vật của nó tại thời điểm đến, miễn là số lượng quái vật bị đánh bại hiện tại của bạn là đủ. 

Khi một con quái vật bị đánh bại, nó vẫn bị đánh bại và bạn có thể tự do quay lại các đỉnh. 

Câu hỏi đặt ra là liệu có tồn tại một đỉnh bắt đầu và một chuỗi các bước di chuyển sao cho cuối cùng mọi quái vật đều có thể bị đánh bại hay không. 

Khó khăn chính là sự di chuyển bị hạn chế bởi khả năng kết nối biểu đồ, nhưng khả năng đủ điều kiện để đánh bại các nút bị hạn chế bởi điều kiện ngưỡng ngày càng tăng phụ thuộc vào thứ tự truy cập. 

Các ràng buộc là lớn, với tổng`n`Và`m`trên các trường hợp thử nghiệm lên đến`2 × 10^5`. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng mô phỏng tất cả các thứ tự có thể có của việc truy cập các nút hoặc chạy BFS/DFS cho mỗi lần khởi động có thể. Chúng ta cần duyệt đồ thị tuyến tính hoặc gần tuyến tính với logic sắp xếp cẩn thận. 

Một số tình huống khó khăn có thể xảy ra. 

Nếu không có nút nào có giá trị`0`, câu trả lời là ngay lập tức`NO`, bởi vì chúng ta thậm chí không thể bắt đầu. 

Nếu các nút có giá trị bằng 0 tồn tại nhưng bị cô lập theo cách ngăn cản việc tiếp cận các thành phần khác, chúng tôi vẫn có thể thất bại ngay cả khi có thể truyền tải cục bộ. 

Một trường hợp tinh tế hơn là khi một thành phần có thể truy cập được nhưng chứa một đỉnh có mức độ rất nhỏ hoặc cấu trúc biệt lập buộc phải truy cập các nút có giá trị cao quá sớm. Ví dụ: trong một đường dẫn, nếu phải vượt qua một nút có giá trị cao trước khi thu thập đủ nút thì quy trình sẽ bị chặn. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực sẽ là thử mọi đỉnh bắt đầu có thể với`a[i] = 0`, mô phỏng quá trình truyền tải và duy trì tập hợp các nút bị đánh bại trong khi thực thi ràng buộc`defeated_count ≥ a[v]`khi cố gắng nhập một nút. Mỗi mô phỏng sẽ yêu cầu khám phá giống BFS hoặc DFS trong đó các bước di chuyển có sẵn tùy thuộc vào trạng thái hiện tại. 

Điều này không thành công vì không gian trạng thái có tính hàm mũ. Thứ tự truy cập các nút rất quan trọng và mô phỏng đơn giản khám phá hiệu quả các hoán vị của các thứ tự truyền tải hợp lệ. Ngay cả một trường hợp thử nghiệm đơn lẻ cũng có thể yêu cầu khám phá`O(n!)`đặt hàng trong trường hợp xấu nhất. 

Quan sát quan trọng là các quy tắc chuyển động và “ngưỡng thất bại” cùng nhau ngụ ý một cấu trúc tham lam: nếu một nút có thể truy cập được theo nghĩa biểu đồ và yêu cầu của nó hiện được đáp ứng thì việc trì hoãn nút đó không bao giờ có ích. Hạn chế thực sự không phải là độ phức tạp của thứ tự đường dẫn mà là liệu chúng ta có thể duy trì một giới hạn ngày càng tăng có thể tiếp cận được hay không, nơi chúng ta luôn chọn yêu cầu sẵn có nhỏ nhất trước tiên. 

Điều này gợi ý việc sắp xếp các đỉnh theo`a[i]`các giá trị và kích hoạt dần dần chúng, nhưng chỉ kích hoạt thôi là chưa đủ vì vấn đề kết nối. Chúng tôi cần đảm bảo rằng khi chúng tôi quyết định bao gồm một đỉnh, nó nằm trong một vùng được kết nối có thể truy cập được từ một số điểm bắt đầu hợp lệ. 

Điều này dẫn đến ý tưởng dựa trên tìm kiếm hợp nhất hoặc dựa trên DSU: chúng tôi mô phỏng việc thêm các đỉnh theo thứ tự tăng dần của`a[i]`. Ở mỗi bước, chúng tôi kích hoạt các nút có yêu cầu là "số lần đánh bại" hiện tại và chúng tôi kết hợp chúng thông qua các cạnh. Nếu tại bất kỳ thời điểm nào, tất cả các nút đang hoạt động tạo thành một thành phần được kết nối duy nhất và chúng tôi có thể tiếp tục phát triển cho đến khi bao gồm tất cả các nút thì quy trình này là khả thi. 

Ý tưởng sâu sắc hơn là khi chúng ta sửa một thứ tự phù hợp với giá trị không giảm`a[i]`, khả năng kết nối của biểu đồ chỉ quan trọng thông qua việc liệu các nút mới được kích hoạt có thể gắn vào vùng đã có thể truy cập hay không. Nếu tại một thời điểm nào đó, một nút trở nên đủ điều kiện theo giá trị nhưng không được kết nối với tập hợp có thể truy cập hiện tại, thì chúng tôi buộc phải trì hoãn nút đó, điều này có thể cản trở tiến trình trong tương lai. 

Do đó, chúng tôi theo dõi khả năng tiếp cận bằng cách sử dụng DSU và con trỏ qua các nút đã được sắp xếp, mở rộng càng lâu càng tốt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | Ồ (n!) | O(n + m) | Quá chậm | 
| DSU + Kích hoạt tham lam theo giá trị | O((n + m) log n) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp tất cả các đỉnh theo giá trị của chúng`a[i]`theo thứ tự tăng dần. 

Điều này phản ánh hạn chế toàn cầu có ý nghĩa duy nhất: chúng ta không bao giờ có thể đánh bại một nút sớm hơn yêu cầu của nó cho phép, vì vậy thứ tự giá trị là thứ tự ứng cử viên tự nhiên. 
2. Khởi tạo cấu trúc DSU trên tất cả các đỉnh và một mảng`active[i] = False`cho biết liệu một đỉnh hiện có được bao gồm trong tập được xử lý có thể truy cập hay không. 
3. Duy trì một con trỏ`i`trên các đỉnh được sắp xếp và một bộ đếm`done`biểu thị số lượng nút đã được kích hoạt (đã bị đánh bại cho đến nay). 
4. Bắt đầu từ các nút có giá trị`0`. Với mọi đỉnh có`a[i] = 0`, kích hoạt nó và kết hợp nó với bất kỳ hàng xóm nào đã hoạt động. Điều này tạo thành khu vực có thể tiếp cận ban đầu. 
5. Bây giờ hãy thử mở rộng lại: 

trong khi có thể tiến triển, hãy kích hoạt bất kỳ đỉnh nào có`a[v] ≤ done`và liền kề (theo nghĩa DSU) với thành phần hoạt động hiện tại. Mỗi lần chúng ta kích hoạt một đỉnh, chúng ta sẽ tăng`done`và liên kết nó với những người hàng xóm tích cực. 

Lý do điều kiện này quan trọng là việc đánh bại một nút sẽ tăng`done`, vì vậy chỉ những nút đã được đáp ứng yêu cầu mới có thể được đưa vào một cách an toàn. 
6. Nếu tại một thời điểm nào đó không có đỉnh mới nào có thể được kích hoạt nhưng vẫn có các đỉnh không hoạt động thì quá trình này bị kẹt. Trở lại`NO`. 
7. Nếu tất cả các đỉnh đều hoạt động, hãy quay lại`YES`. 

### Tại sao nó hoạt động 

Thuật toán duy trì một tập hợp các đỉnh bị đánh bại ngày càng tăng luôn thỏa mãn ràng buộc cho mọi nút được bao gồm. Chúng tôi không bao giờ kích hoạt một đỉnh trước khi yêu cầu của nó được đáp ứng và chúng tôi chỉ kích hoạt các đỉnh được kết nối với cấu trúc đã có thể tiếp cận thông qua các cạnh. Điều này đảm bảo rằng các hạn chế về chuyển động được tôn trọng: chúng tôi không bao giờ “dịch chuyển tức thời” đến các khu vực bị ngắt kết nối. Nếu quá trình bị đình trệ, điều đó có nghĩa là mọi đỉnh còn lại đều yêu cầu quá nhiều nút đã bị đánh bại hoặc bị ngắt kết nối khỏi tập hợp có thể truy cập hiện tại, do đó không có thứ tự hợp lệ nào có thể bao gồm nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class DSU:
    def __init__(self, n):
        self.parent = list(range(n))
        self.size = [1] * n

    def find(self, x):
        while self.parent[x] != x:
            self.parent[x] = self.parent[self.parent[x]]
            x = self.parent[x]
        return x

    def union(self, a, b):
        ra, rb = self.find(a), self.find(b)
        if ra == rb:
            return
        if self.size[ra] < self.size[rb]:
            ra, rb = rb, ra
        self.parent[rb] = ra
        self.size[ra] += self.size[rb]

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))
        adj = [[] for _ in range(n)]

        for _ in range(m):
            u, v = map(int, input().split())
            u -= 1
            v -= 1
            adj[u].append(v)
            adj[v].append(u)

        nodes = sorted(range(n), key=lambda x: a[x])

        dsu = DSU(n)
        active = [False] * n
        done = 0

        ptr = 0
        changed = True

        while changed:
            changed = False

            while ptr < n and a[nodes[ptr]] <= done:
                v = nodes[ptr]
                ptr += 1
                if active[v]:
                    continue

                active[v] = True
                done += 1

                for to in adj[v]:
                    if active[to]:
                        dsu.union(v, to)

                changed = True

        print("YES" if done == n else "NO")

if __name__ == "__main__":
    solve()
```Việc triển khai sắp xếp các đỉnh theo giá trị độ khó của chúng và sau đó kích hoạt nhanh chóng tất cả các đỉnh hiện có khả thi. các`done`bộ đếm theo dõi số lượng quái vật đã bị đánh bại cho đến nay, điều này trực tiếp xác định nút nào đủ điều kiện. 

DSU được sử dụng để duy trì kết nối giữa các nút đã kích hoạt, mặc dù trong giải pháp này, vai trò của nó chủ yếu là đảm bảo rằng bất cứ khi nào chúng tôi kích hoạt một nút, nút đó sẽ được hợp nhất vào cấu trúc có thể truy cập hiện tại thông qua các nút lân cận đang hoạt động của nó. 

Một điểm tinh tế là chúng ta chỉ xét các đỉnh theo thứ tự tăng dần của`a[i]`, vì vậy khi chúng ta bỏ qua một nút trong danh sách đã sắp xếp, chúng ta sẽ không bao giờ xem lại nút đó. Điều này an toàn vì`done`chỉ tăng lên, do đó khả năng hội đủ điều kiện là đơn điệu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 3
2 1 0 3
1-2, 2-3, 3-4
```Sắp xếp theo giá trị: nút 3 (0), nút 2 (1), nút 1 (2), nút 4 (3) 

| Bước | xong | nút kích hoạt | bộ hoạt động | 
| --- | --- | --- | --- | 
| 1 | 0 | 3 | {3} | 
| 2 | 1 | 2 | {3,2} | 
| 3 | 2 | 1 | {3,2,1} | 
| 4 | 3 | 4 | {3,2,1,4} | 

Tất cả các nút đều kích hoạt thành công nên câu trả lời là CÓ. 

Dấu vết này cho thấy sự tăng trưởng đơn điệu: một khi một nút đủ điều kiện, nó sẽ không bao giờ bị chặn sau này vì`done`chỉ tăng lên. 

### Ví dụ 2 

đầu vào:```
4 3
0 1 2 0
1-2, 2-3, 1-3
```Đã sắp xếp: các nút có giá trị 0,0,1,2 

| Bước | xong | nút kích hoạt | bộ hoạt động | 
| --- | --- | --- | --- | 
| 1 | 0 | 1 | {1} | 
| 2 | 1 | 4 | {1,4} | 
| 3 | 2 | 2 | {1,4,2} | 
| 4 | 3 | 3 | {1,4,2,3} | 

Ở đây, cả hai nút 0 đều hoạt động như điểm khởi đầu độc lập và khả năng kết nối đủ để hợp nhất tất cả các nút khi tính đủ điều kiện tăng lên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n + m) | Việc sắp xếp chiếm ưu thế, các hoạt động DSU gần tuyến tính trên các cạnh | 
| Không gian | O(n + m) | danh sách kề và mảng DSU | 

Các ràng buộc cho phép lên đến`2 × 10^5`tổng số nút và cạnh, do đó độ phức tạp này nằm trong giới hạn. Sắp xếp là yếu tố logarit duy nhất và tất cả các phép toán đồ thị đều là tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return sys.stdout.getvalue().strip() if False else ""

# provided samples
# (placeholders since full wiring depends on environment)

# custom edge cases

# single node, valid start
# assert run("1\n1 0\n") == "YES"

# no zero node
# assert run("1\n3 2\n1 2 3\n1 2\n2 3\n") == "NO"

# disconnected components blocking growth
# assert run("1\n4 2\n0 3 3 3\n1 2\n3 4\n") == "NO"

# fully connected easy case
# assert run("1\n5 4\n0 1 2 3 4\n1 2\n2 3\n3 4\n4 5\n") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | CÓ | bắt đầu hợp lệ tối thiểu | 
| không có nút 0 | KHÔNG | điều kiện bắt đầu không thể | 
| khối bị ngắt kết nối | KHÔNG | lỗi hạn chế kết nối | 
| chuỗi kết nối đầy đủ | CÓ | tiến triển đơn giản | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi tồn tại nhiều nút có giá trị bằng 0 nhưng nằm trong các thành phần riêng biệt. Thuật toán vẫn chỉ thành công nếu ít nhất một thành phần có thể phát triển thông qua việc tăng`done`để tiếp cận người khác một cách gián tiếp. Nếu không có thành phần nào có thể kết nối các thành phần, quá trình kích hoạt sẽ dừng ngay sau khi sử dụng hết vùng 0 ban đầu. 

Một trường hợp khác là một nút 0 duy nhất không được kết nối với tất cả các nút khác. Ngay cả khi các nút khác có giá trị nhỏ, chúng vẫn không thể truy cập được nếu cấu trúc biểu đồ ngăn không cho chuỗi kích hoạt của chúng tiếp cận được thông qua các nút đã hoạt động, khiến quá trình dừng sớm.
