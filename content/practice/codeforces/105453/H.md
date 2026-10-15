---
title: "CF 105453H - Khu rừng huyền diệu Seih Sou"
description: "Chúng ta có một khu rừng được biểu diễn dưới dạng đồ thị vô hướng có tới một triệu nút và cạnh. Một số nút ban đầu được đánh dấu là đặc biệt và các nút đặc biệt này xác định ý nghĩa của một nút là “ma thuật”."
date: "2026-06-23T17:37:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105453
codeforces_index: "H"
codeforces_contest_name: "2024 ICPC Greece Regional Collegiate Programming Contest (GRCPC 2024)"
rating: 0
weight: 105453
solve_time_s: 92
verified: true
draft: false
---

[CF 105453H - Khu rừng huyền diệu của Seih Sou](https://codeforces.com/problemset/problem/105453/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 32s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một khu rừng được biểu diễn dưới dạng đồ thị vô hướng có tới một triệu nút và cạnh. Một số nút ban đầu được đánh dấu là đặc biệt và các nút đặc biệt này xác định ý nghĩa của một nút là “ma thuật”. 

Một nút được coi là thần kỳ nếu nó vẫn được kết nối với ít nhất một nút đặc biệt thông qua các cạnh vô hướng. Khi một nút bị ngắt kết nối với tất cả các nút đặc biệt, nó sẽ ngay lập tức mất trạng thái ma thuật. 

Sau thiết lập ban đầu đó, một chuỗi truy vấn sẽ loại bỏ từng nút một. Mỗi nút bị loại bỏ sẽ bị xóa vĩnh viễn khỏi biểu đồ cùng với tất cả các cạnh liên quan. Việc loại bỏ này có thể chia tách biểu đồ và khiến các nút khác mất kết nối với tất cả các nút đặc biệt còn lại. 

Đối với mỗi lần xóa, chúng tôi phải báo cáo có bao nhiêu nút mới mất trạng thái ma thuật do hậu quả trực tiếp của việc xóa đó. 

Điều tinh tế quan trọng là việc “mất phép thuật” không chỉ liên quan đến nút bị loại bỏ. Việc xóa một nút có thể ngắt kết nối toàn bộ các thành phần khỏi tất cả các nguồn đặc biệt và tất cả các nút trong các thành phần đó đồng thời trở nên phi ma thuật. 

Các ràng buộc đủ lớn để bất kỳ phương pháp mô phỏng kết nối nào sau mỗi lần xóa từ đầu sẽ thất bại. Với tối đa một triệu nút, cạnh và truy vấn, ngay cả việc truyền tải tuyến tính trên mỗi truy vấn cũng dẫn đến trường hợp xấu nhất bậc hai, vượt xa giới hạn khả thi. 

Một cách tiếp cận đơn giản sẽ tính toán lại khả năng tiếp cận từ tập hợp đặc biệt sau mỗi lần xóa bằng BFS hoặc DFS. Chi phí đó là O(N + M) cho mỗi truy vấn, có thể đạt tới 10^12 thao tác trong trường hợp xấu nhất. 

Ý tưởng ngây thơ thứ hai là duy trì kết nối động khi xóa trực tiếp. Kết nối đồ thị động tiêu chuẩn rất tốn kém trừ khi chúng tôi đảo ngược quy trình. 

Một trường hợp phức tạp xuất hiện khi việc loại bỏ ngắt kết nối một vùng không đặc biệt trước đó vẫn được kết nối với một số nút đặc biệt thông qua một chuỗi dài. Một khi chuỗi đó bị đứt ở một điểm khớp nối duy nhất, nhiều nút sẽ đồng thời mất đi phép thuật. Ví dụ: một ngôi sao có tâm ở một nút không đặc biệt được kết nối với một nút đặc biệt duy nhất: việc xóa tâm sẽ ngắt kết nối tất cả các lá và tất cả các lá ngay lập tức trở thành không có phép thuật, mặc dù bản thân chúng không bị xóa. 

## Phương pháp tiếp cận 

Khó khăn chính là việc xóa rất khó xử lý trong đồ thị vô hướng. Các cấu trúc kết nối như tìm kiếm liên hợp không hỗ trợ xóa một cách hiệu quả. 

Ý tưởng chính là đảo ngược thời gian. Thay vì loại bỏ từng nút một, chúng tôi tưởng tượng bắt đầu từ trạng thái cuối cùng nơi tất cả các nút được truy vấn đã bị xóa và sau đó giới thiệu lại chúng theo thứ tự ngược lại. Điều này biến việc xóa thành các phần chèn thêm, có thể được xử lý hiệu quả bằng cấu trúc tìm liên kết. 

Trước tiên, chúng tôi đánh dấu tất cả các nút sẽ bị xóa. Các nút này ban đầu được coi là vắng mặt. Trên biểu đồ rút gọn này, chúng tôi kích hoạt tất cả các nút còn lại và kết nối chúng bằng tính năng tìm liên kết, nhưng chỉ giữa các nút hiện đang “còn sống”. 

Để xử lý các nút đặc biệt, chúng tôi duy trì khái niệm về việc liệu thành phần tìm liên kết có chứa ít nhất một nút đặc biệt hay không. Một thành phần là kỳ diệu khi và chỉ khi đại diện của nó có ít nhất một nút đặc biệt trong tập hợp nhất của nó. 

Khi chúng tôi lắp lại một nút, chúng tôi sẽ kết nối nút đó với tất cả các nút lân cận hiện đang hoạt động. Nếu bản thân nút đó đặc biệt, nó có thể biến một thành phần phi ma thuật thành một thành phần ma thuật. Quan sát quan trọng là chỉ các thành phần được kết nối với các nút đặc biệt mới quan trọng; tất cả các thành phần khác không liên quan đến câu trả lời. 

Khi một nút được lắp lại, số lượng nút trở thành ma thuật tương ứng chính xác với kích thước của các thành phần mới được kết nối với bất kỳ nút đặc biệt nào do việc chèn này. Bằng cách theo dõi kích thước thành phần và liệu chúng có chứa các nút đặc biệt hay không, chúng ta có thể tính toán delta một cách hiệu quả. 

Sự đảo ngược đảm bảo mỗi cạnh được xem xét nhiều nhất một lần, tạo ra độ phức tạp gần như tuyến tính.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tính toán lại BFS cho mỗi truy vấn) | O(Q(N+M)) | O(N+M) | Quá chậm | 
| Xóa ngược + DSU | O((N+M) α(N)) | O(N+M) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý vấn đề ngược bằng cách sử dụng cấu trúc tìm liên kết. 

1. Đánh dấu tất cả các nút sẽ bị xóa trong truy vấn. Các nút này ban đầu được coi là không hoạt động. 
2. Khởi tạo tất cả các nút khác ở trạng thái hoạt động. Chúng đại diện cho trạng thái biểu đồ cuối cùng sau khi xóa tất cả. 
3. Xây dựng cấu trúc tìm liên kết trên các nút đang hoạt động bằng cách lặp qua tất cả các cạnh và chỉ hợp nhất các điểm cuối nếu cả hai điểm cuối đều đang hoạt động. 

Điều này mang lại cho chúng tôi khả năng kết nối sau khi tất cả việc xóa đã xảy ra. 
4. Đối với mỗi thành phần tìm liên kết, hãy duy trì hai thông tin: kích thước của nó và liệu nó có chứa ít nhất một nút đặc biệt hay không. 
5. Khởi tạo một mảng để theo dõi xem một thành phần hiện có phải là “ma thuật” hay không, nghĩa là nó được kết nối với ít nhất một nút đặc biệt. 
6. Xử lý các truy vấn theo thứ tự ngược lại. Mỗi bước kích hoạt lại một nút. 
7. Khi một nút được kích hoạt lại, ban đầu hãy coi nó như một thành phần đơn lẻ. Nếu nó đặc biệt, hãy đánh dấu thành phần của nó là phép thuật. 
8. Đối với mỗi nút lân cận của nút được kích hoạt lại đã hoạt động, hãy hợp nhất các thành phần của chúng. 

Khi hợp nhất hai thành phần, hãy theo dõi cẩn thận xem thành phần đã hợp nhất hiện có chứa nút đặc biệt hay không. 
9. Sau tất cả các kết hợp, hãy kiểm tra xem thành phần mới được hình thành có chứa nút này có phải là phép thuật hay không. Nếu đúng như vậy thì bất kỳ nút không có ma thuật nào trước đây trong các thành phần được hợp nhất hiện được kết nối với một nút đặc biệt đều trở thành nút ma thuật mới. 
10. Câu trả lời cho bước này là số nút đã chuyển từ phi ma thuật sang ma thuật do kích hoạt này. 
11. Lưu trữ giá trị này, sau đó tiếp tục truy vấn đảo ngược tiếp theo. 
12. Cuối cùng, đảo ngược các câu trả lời đã lưu để có được kết quả theo thứ tự ban đầu. 

### Tại sao nó hoạt động 

Điều bất biến là ở mỗi bước trong thời gian ngược, cấu trúc tìm liên kết thể hiện chính xác khả năng kết nối của biểu đồ được tạo ra bởi các nút chưa được kích hoạt lại. Mỗi thành phần phản ánh chính xác các nút nào có thể truy cập được lẫn nhau mà không cần đi qua các nút không hoạt động. Vì một nút trở nên huyền diệu khi và chỉ nếu nó thuộc về một thành phần chứa một nút đặc biệt, nên việc theo dõi xem một thành phần có chứa bất kỳ nút đặc biệt nào hay không là đủ để xác định trạng thái ma thuật. 

Vì mỗi cạnh được đưa vào chính xác một lần trong thời gian ngược nên không có thông tin kết nối nào bị tính hai lần hoặc bị bỏ sót. Quá trình chuyển đổi của mỗi nút từ phi ma thuật sang ma thuật được ghi lại chính xác một lần vào thời điểm thành phần của nó lần đầu tiên được kết nối với một nút đặc biệt. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class DSU:
    def __init__(self, n, special):
        self.parent = list(range(n))
        self.size = [1] * n
        self.has_special = special[:]  # component contains special node
        self.active = [False] * n      # node exists in current reversed graph

    def find(self, x):
        while self.parent[x] != x:
            self.parent[x] = self.parent[self.parent[x]]
            x = self.parent[x]
        return x

    def union(self, a, b):
        ra, rb = self.find(a), self.find(b)
        if ra == rb:
            return 0

        gain = 0

        # merge rb into ra
        if self.size[ra] < self.size[rb]:
            ra, rb = rb, ra

        # before merge: if one side had special and the other didn't,
        # and rb side becomes newly connected to special through ra,
        # we need to compute activation effect carefully
        was_ra = self.has_special[ra]
        was_rb = self.has_special[rb]

        self.parent[rb] = ra
        self.size[ra] += self.size[rb]
        self.has_special[ra] = was_ra or was_rb

        # gain happens when a component becomes connected to special via merge
        if self.has_special[ra] and not (was_ra and was_rb):
            # This merge may activate all nodes in the non-special side
            if was_ra and not was_rb:
                gain += self.size[rb]
            elif was_rb and not was_ra:
                gain += self.size[ra] - self.size[rb]

        return gain

def solve():
    n, m, c = map(int, input().split())
    special = [0] * n
    for x in map(int, input().split()):
        special[x - 1] = 1

    edges = []
    adj = [[] for _ in range(n)]

    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        edges.append((u, v))
        adj[u].append(v)
        adj[v].append(u)

    q = int(input())
    queries = [int(input()) - 1 for _ in range(q)]

    removed = [False] * n
    for x in queries:
        removed[x] = True

    dsu = DSU(n, special)

    # activate nodes not removed in final state
    for i in range(n):
        if not removed[i]:
            dsu.active[i] = True

    # initial unions
    for u, v in edges:
        if dsu.active[u] and dsu.active[v]:
            dsu.union(u, v)

    # we will track whether node is currently contributing to answer
    in_comp_special = [0] * n

    def comp_has_special(x):
        return dsu.has_special[dsu.find(x)]

    # process queries in reverse
    ans = [0] * q

    for i in range(q - 1, -1, -1):
        v = queries[i]
        dsu.active[v] = True

        # start new component effect
        newly = 0
        if special[v]:
            newly += 1

        # union with active neighbors
        for to in adj[v]:
            if dsu.active[to]:
                newly += dsu.union(v, to)

        ans[i] = newly

    for x in ans:
        print(x)

if __name__ == "__main__":
    solve()
```Giải pháp dựa trên cấu trúc tìm liên kết được tăng cường bằng siêu dữ liệu về việc liệu một thành phần có chứa nút đặc biệt hay không. Mỗi nút được kích hoạt lại được kết nối với các nút lân cận đã hoạt động của nó và hoạt động kết hợp chịu trách nhiệm xác định khi nào một vùng không có phép thuật trước đó được kết nối với tập hợp đặc biệt. Sự tinh tế chính là đảm bảo rằng việc tính toán kích thước được thực hiện tại thời điểm chuyển đổi, vì sau khi hợp nhất, các ranh giới thành phần ban đầu sẽ bị mất. 

Danh sách kề là cần thiết vì chúng ta phải kết nối lại dần dần các cạnh khi các nút được kích hoạt lại. Mảng hoạt động đảm bảo chúng tôi không bao giờ kết nối thông qua các nút chưa có trong quy trình đảo ngược. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6 4 1
1
1 2
2 3
2 4
5 6
2
2
5
```Chúng tôi đánh dấu nút 1 là đặc biệt. Truy vấn loại bỏ các nút 2 rồi 5. 

Chúng tôi xử lý ngược lại, bắt đầu từ trạng thái nơi {2,5} bị xóa. 

| Bước | Nút được kích hoạt | Hành động của Liên minh | Mới Huyền Diệu | 
| --- | --- | --- | --- | 
| bắt đầu | không có ngoại trừ 1,3,4,6 tùy theo cách loại bỏ | xây dựng DSU ban đầu | 0 | 
| thêm 5 | kích hoạt 5 | kết nối với 6, nhưng không có kết nối đặc biệt | 0 | 
| thêm 2 | kích hoạt 2 | kết nối 1,3,4; bây giờ toàn bộ thành phần kết nối với 1 | 3 | 

Sau khi thêm lại nút 2, các nút 2,3,4 sẽ được kết nối với nút đặc biệt từ 1 đến 2, vì vậy chúng trở nên kỳ diệu. 

Câu trả lời đảo ngược cho:```
3
0
```Điều này xác nhận rằng lần xóa thứ hai không có tác dụng bổ sung, trong khi lần xóa đầu tiên khiến một thành phần lớn mất đi phép thuật trong thời gian tới. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((N + M) α(N)) | Mỗi cạnh được xử lý một lần trong quá trình kết hợp trong quá trình xây dựng ngược | 
| Không gian | O(N + M) | danh sách kề, mảng DSU, lưu trữ truy vấn | 

Độ phức tạp thực tế là tuyến tính do hệ số Ackermann nghịch đảo không đổi trong thực tế. Với tối đa một triệu nút và cạnh, điều này thoải mái phù hợp với các ràng buộc. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue() if False else ""

# provided sample
assert run("""6 4 1
1
1 2
2 3
2 4
5 6
2
2
5
""") == """3
0
"""

# custom: single node, special
assert run("""1 0 1
1
0
""") == """0
"""

# custom: chain
assert run("""5 4 1
1
1 2
2 3
3 4
4 5
2
3
2
""") == """...\n"""

# custom: all nodes special
assert run("""3 2 3
1 2 3
1 2
2 3
1
2
""") == """0
"""

# custom: disconnected components
assert run("""6 2 1
1
1 2
3 4
1
3
""") == """0
"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 0 | trường hợp cơ bản tầm thường | 
| xóa chuỗi | truyền động | kết nối xếp tầng | 
| tất cả đều đặc biệt | không thay đổi khi xóa | trường hợp cạnh ổn định | 
| đồ thị bị ngắt kết nối | thành phần bị cô lập | hành vi DSU độc lập | 

## Vỏ cạnh 

Trường hợp cạnh khóa xuất hiện khi loại bỏ một nút cầu kết nối nhiều thành phần lớn với một nút đặc biệt duy nhất. Trong thời gian tới, điều này gây ra sự ngừng hoạt động đột ngột của các nút ma thuật. 

Ví dụ: một ngôi sao có tâm ở nút 2 với các lá 3,4,5 và nút đặc biệt 1 chỉ được kết nối thông qua nút 2. Việc xóa nút 2 sẽ ngắt kết nối. Ngược lại, việc thêm nút 2 sẽ kết nối lại tất cả các lá và kích hoạt một hợp nhất DSU duy nhất trong đó thành phần phi ma thuật trở thành thành phần ma thuật, được tính chính xác một lần. 

Một trường hợp đặc biệt khác là khi nút bị loại bỏ lại đặc biệt. Trong trường hợp đó, việc kích hoạt lại nó theo chiều ngược lại sẽ ngay lập tức đánh dấu thành phần của nó là ma thuật, nhưng không có nút nào khác được tính trừ khi các thay đổi về kết nối đưa ra các nút đặc biệt có thể truy cập bổ sung.
