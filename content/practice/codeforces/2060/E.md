---
title: "CF 2060E - Thành phần đồ thị"
description: "Cho hai đồ thị vô hướng có cùng một tập đỉnh. Một biểu đồ, gọi là biểu đồ làm việc, là cấu trúc mà chúng ta được phép sửa đổi. Biểu đồ thứ hai là một cấu trúc tham chiếu cố định mà cuối cùng chúng ta phải “khớp” về mặt kết nối."
date: "2026-06-08T10:40:52+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "dsu", "graphs", "greedy"]
categories: ["algorithms"]
codeforces_contest: 2060
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 998 (Div. 3)"
rating: 1500
weight: 2060
solve_time_s: 74
verified: true
draft: false
---

[CF 2060E - Thành phần biểu đồ](https://codeforces.com/problemset/problem/2060/E) 

**Đánh giá:** 1500 
**Tags:** dfs và tương tự, dsu, đồ thị, tham lam 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Cho hai đồ thị vô hướng có cùng một tập đỉnh. Một biểu đồ, gọi là biểu đồ làm việc, là cấu trúc mà chúng ta được phép sửa đổi. Biểu đồ thứ hai là một cấu trúc tham chiếu cố định mà cuối cùng chúng ta phải “khớp” về mặt kết nối. 

Thao tác được phép rất mạnh: chúng ta có thể tự do chèn bất kỳ cạnh nào bị thiếu vào biểu đồ đang làm việc hoặc xóa bất kỳ cạnh nào hiện có. Mỗi sửa đổi như vậy tốn một thao tác. Sau khi thực hiện một số chuỗi chỉnh sửa này, chúng tôi không quan tâm đến các cạnh chính xác nữa mà chỉ quan tâm đến các thành phần được kết nối. Mục tiêu là hai đỉnh được kết nối trong biểu đồ đang hoạt động khi và chỉ khi chúng được kết nối trong biểu đồ tham chiếu. 

Vì vậy, nhiệm vụ cơ bản là chuyển đổi một phân vùng gồm các đỉnh thành một phân vùng khác, trong đó mỗi phân vùng được xác định bởi các thành phần được kết nối của biểu đồ. 

Các ràng buộc rất chặt chẽ: tối đa 2×10^5 đỉnh và cạnh trong tất cả các trường hợp thử nghiệm. Điều này ngay lập tức loại trừ bất kỳ phương pháp nào tính toán lại kết nối nhiều lần trên mỗi cạnh hoặc mô phỏng các thay đổi một cách đơn giản. Về cơ bản, bất kỳ giải pháp nào cũng phải giảm vấn đề xuống một vài bước tuyến tính hoặc gần như tuyến tính cho mỗi trường hợp thử nghiệm, thường sử dụng DSU hoặc DFS. 

Trường hợp cạnh tinh tế phát sinh khi hai biểu đồ đã xác định kết nối giống hệt nhau nhưng có tập hợp cạnh rất khác nhau. Ví dụ: nếu cả hai biểu đồ được kết nối nhưng một biểu đồ là cây và biểu đồ kia dày đặc thì không cần thực hiện thao tác nào vì kết nối đã khớp nhau, mặc dù các bộ cạnh khác nhau đáng kể. Một cách tiếp cận ngây thơ cố gắng “căn chỉnh các cạnh” thay vì các thành phần sẽ tính sai các chỉnh sửa không cần thiết. 

Một trường hợp cạnh khác xảy ra khi cả hai đồ thị bị ngắt kết nối hoàn toàn nhưng các đỉnh được phân chia khác nhau. Ví dụ: nếu F là một thành phần và G chia mọi thứ thành các phần tử đơn thì chúng ta phải xóa đủ cạnh để cô lập mọi đỉnh trong F. Bất kỳ cách tiếp cận nào chỉ tập trung vào sự khác biệt của các cạnh mà không xem xét cấu trúc thành phần sẽ bị đếm thừa hoặc đếm thiếu trong những trường hợp như vậy. 

## Phương pháp tiếp cận 

Phối cảnh vũ phu sẽ cố gắng biến đổi trực tiếp F thành một số biểu đồ mục tiêu có cùng các thành phần được kết nối với G. Người ta có thể tưởng tượng việc kiểm tra liên tục các cặp đỉnh và quyết định xem có cần một cạnh hay phải loại bỏ tùy thuộc vào việc liệu chúng có nên kết nối hay không. Tuy nhiên, mỗi thao tác sẽ thay đổi khả năng kết nối trên toàn cầu, do đó, việc tính toán lại các thành phần được kết nối sau mỗi lần chỉnh sửa sẽ dẫn đến việc duyệt biểu đồ lặp lại tốn kém. Ngay cả khi chúng ta lựa chọn cẩn thận các cạnh, việc duy trì kết nối động dưới sự bổ sung và xóa tùy ý sẽ đẩy chúng ta vào những cấu trúc nặng nề hơn mức cần thiết. 

Sự đơn giản hóa chính là chúng ta thực sự không bao giờ cần khớp các cạnh, chỉ các thành phần được kết nối. Một khi điều này được nhận ra, vấn đề sẽ giảm xuống còn việc so sánh hai phần của tập đỉnh: các thành phần của F với các thành phần của G. 

Bên trong mỗi thành phần liên thông của G, tất cả các đỉnh phải kết thúc liên thông trong đồ thị cuối cùng. Điều này có nghĩa là trong mỗi thành phần G, trạng thái cuối cùng phải là một thành phần được kết nối duy nhất. Ngược lại, các đỉnh từ các thành phần G khác nhau phải bị ngắt kết nối trong biểu đồ cuối cùng, do đó, bất kỳ cạnh nào của F giao nhau giữa các thành phần G khác nhau về cơ bản là “sai” và phải bị loại bỏ. 

Bây giờ cấu trúc trở nên sạch sẽ. Đầu tiên chúng ta buộc F tôn trọng phân vùng của G bằng cách loại bỏ tất cả các cạnh trong F kết nối các thành phần G khác nhau. Sau quá trình dọn dẹp này, mỗi thành phần còn lại của F nằm hoàn toàn bên trong một số thành phần của G. Trong mỗi thành phần G, khi đó chúng ta cần kết nối nhiều thành phần F thành một. Mỗi lần hợp nhất hai thành phần yêu cầu thêm chính xác một cạnh và điều tốt nhất chúng ta có thể làm là kết nối các thành phần một cách tham lam, do đó chi phí là số thành phần trừ đi một thành phần trên mỗi thành phần G.

Điều này dẫn đến giải pháp dựa trên DSU: chúng tôi sử dụng DSU cho G để xác định các nhóm, sau đó mô phỏng các cạnh F hợp lệ trong các nhóm đó, đếm các thành phần kết quả trên mỗi nhóm và tính toán số lượng hợp nhất cần thiết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tính toán lại lực lượng vũ phu | O(n·(m₁+m₂)) | O(n+m) | Quá chậm | 
| Hợp nhất thành phần DSU | O((n+m₁+m₂) α(n)) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi sử dụng các cấu trúc tập hợp rời rạc hai lần theo cách khái niệm, nhưng chỉ cần một DSU khi triển khai. 

1. Xây dựng DSU cho đồ thị G để tính các thành phần liên thông của nó. Mỗi đỉnh được gán một đại diện để xác định thành phần cuối cùng mà nó thuộc về. Bước này chuyển đổi vấn đề từ dựa trên biểu đồ sang dựa trên thành phần. 
2. Khởi tạo một DSU khác cho các đỉnh, nhưng chỉ cho phép các tập hợp nằm trong cùng một thành phần G. Điều này đảm bảo chúng ta không bao giờ vô tình hợp nhất các thành phần phải tách biệt trong câu trả lời cuối cùng. 
3. Lặp qua tất cả các cạnh của F. Với mỗi cạnh (u, v), kiểm tra xem u và v có thuộc cùng một thành phần G hay không. Nếu không, cạnh này không hợp lệ trong cấu trúc cuối cùng và đóng góp một lần xóa bắt buộc. Nếu đúng như vậy, hợp u và v trong DSU bị hạn chế của F. 
4. Sau khi xử lý tất cả các cạnh hợp lệ, bây giờ chúng ta có một phân vùng của mỗi thành phần G thành nhiều phần được kết nối được tạo thành bởi các cạnh cho phép của F. 
5. Với mỗi thành phần G, hãy đếm xem có bao nhiêu nghiệm DSU giữa các đỉnh của nó. Nếu một thành phần có k phần như vậy, chúng ta cần chính xác k−1 thao tác để kết nối chúng thành một thành phần được kết nối duy nhất. 
6. Tính tổng tất cả các thành phần G các phép hợp nhất được yêu cầu và thêm tất cả các phần xóa của các cạnh thành phần chéo. Tổng số này là câu trả lời. 

### Tại sao nó hoạt động 

Tính đúng đắn đến từ việc tách vấn đề thành hai ràng buộc độc lập: loại bỏ các cạnh vi phạm sự ngắt kết nối bắt buộc và chỉ thêm các cạnh để khôi phục kết nối bên trong mỗi nhóm được yêu cầu. Khi các cạnh của thành phần chéo bị loại bỏ, mỗi thành phần G sẽ trở thành một bài toán con bị cô lập. Bên trong một bài toán con như vậy, bất kỳ cấu trúc liên thông nào có k thành phần đều yêu cầu ít nhất k−1 cạnh để trở nên liên thông và chúng ta luôn có thể đạt được điều đó vì bất kỳ cạnh nào bị thiếu bên trong thành phần đều được phép thêm vào. 

Điều này mang lại cả giới hạn dưới và cách mang tính xây dựng để đạt được nó. 

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
        a = self.find(a)
        b = self.find(b)
        if a == b:
            return
        if self.size[a] < self.size[b]:
            a, b = b, a
        self.parent[b] = a
        self.size[a] += self.size[b]

def solve():
    t = int(input())
    for _ in range(t):
        n, m1, m2 = map(int, input().split())

        g_dsu = DSU(n)

        edges_f = []
        for _ in range(m1):
            u, v = map(int, input().split())
            edges_f.append((u - 1, v - 1))

        for _ in range(m2):
            u, v = map(int, input().split())
            g_dsu.union(u - 1, v - 1)

        comp_vertices = [[] for _ in range(n)]
        for i in range(n):
            comp_vertices[g_dsu.find(i)].append(i)

        f_dsu = DSU(n)

        deletions = 0

        for u, v in edges_f:
            if g_dsu.find(u) != g_dsu.find(v):
                deletions += 1
            else:
                f_dsu.union(u, v)

        answer = deletions

        for comp in comp_vertices:
            roots = set()
            for v in comp:
                roots.add(f_dsu.find(v))
            if len(comp) > 0:
                answer += len(roots) - 1

        print(answer)

if __name__ == "__main__":
    solve()
```Mã đầu tiên nén đồ thị G thành các thành phần bằng DSU. Sau đó, nó lọc các cạnh của F dựa trên việc chúng có tôn trọng các thành phần đó hay không. Bất kỳ thành phần cắt cạnh nào đều được tính ngay lập tức là yêu cầu xóa. 

Bên trong mỗi thành phần G hợp lệ, chúng tôi theo dõi cách các cạnh được phép của F chia các đỉnh thành các thành phần phụ. Mỗi gốc DSU riêng biệt tương ứng với một phần và chúng tôi tính toán số lượng hợp nhất cần thiết để thống nhất chúng. 

Một chi tiết triển khai tinh tế là chúng tôi không bao giờ cố gắng duy trì kết nối toàn cầu trong F. Chúng tôi chỉ dựa vào các nhãn đại diện vì cấu trúc nhiều thành phần sẽ không liên quan sau khi được lọc. 

## Ví dụ đã hoạt động 

Chúng ta theo dõi một trường hợp nhỏ trong đó G chia các đỉnh khác với F. 

Xét trường hợp có n = 4. 

Các cạnh F: (1,2), (2,3) 

Các cạnh G: (1,2), (3,4) 

Các thành phần G là {1,2} và {3,4}. 

### Dấu vết 

| Bước | Hành động | thành phần DSU(G) | DSU(F hạn chế) | xóa | 
| --- | --- | --- | --- | --- | 
| ban đầu | xây dựng G | {1,2}, {3,4} | tất cả riêng biệt | 0 | 
| quá trình (1,2) | công đoàn được phép | {1,2}, {3,4} | {1,2} | 0 | 
| quá trình (2,3) | chéo | {1,2}, {3,4} | {1},{2},{3},{4} | 1 | 

Bây giờ bên trong các thành phần G: 

Đối với {1,2}, F có một thành phần nên có giá 0. 

Với {3,4}, F có hai đơn vị nên có giá 1. 

Tổng số câu trả lời là 2. 

Dấu vết này cho thấy cách thuật toán tách biệt rõ ràng chi phí xóa khỏi chi phí hợp nhất nội bộ, phù hợp với việc phân tách các ràng buộc. 

Trường hợp thứ hai chứng tỏ một G được kết nối đầy đủ. 

Cạnh F: không có 

Cạnh G: tạo thành chuỗi 1-2-3-4 

Tất cả các đỉnh đều nằm trong một thành phần G nên chúng ta chỉ tính toán khả năng kết nối bên trong F. Mỗi đỉnh đều bị cô lập nên chúng ta có 4 thành phần và cần 3 lần hợp nhất. Thuật toán tự nhiên đưa ra 3, phù hợp với ý tưởng rằng chúng ta chỉ cần xây dựng một cây bao trùm bên trong một nhóm duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m₁ + m₂) | Hoạt động DSU và chuyển một lần qua các cạnh và đỉnh | 
| Không gian | O(n) | Mảng DSU và lưu trữ nhóm | 

Các ràng buộc cho phép tổng số phần tử lên tới 2×10^5, do đó, các hoạt động DSU tuyến tính hoặc gần tuyến tính nằm trong giới hạn, đặc biệt là với tính năng nén đường dẫn và liên kết theo kích thước, đảm bảo thời gian khấu hao gần như không đổi cho mỗi hoạt động. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
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
            a = self.find(a)
            b = self.find(b)
            if a == b:
                return
            if self.size[a] < self.size[b]:
                a, b = b, a
            self.parent[b] = a
            self.size[a] += self.size[b]

    t = int(input())
    out = []

    for _ in range(t):
        n, m1, m2 = map(int, input().split())

        g = DSU(n)

        edges = []
        for _ in range(m1):
            u, v = map(int, input().split())
            edges.append((u - 1, v - 1))

        for _ in range(m2):
            u, v = map(int, input().split())
            g.union(u - 1, v - 1)

        f = DSU(n)

        del_cost = 0

        for u, v in edges:
            if g.find(u) != g.find(v):
                del_cost += 1
            else:
                f.union(u, v)

        comp = {}
        for i in range(n):
            r = g.find(i)
            comp.setdefault(r, []).append(i)

        ans = del_cost
        for nodes in comp.values():
            roots = set(f.find(x) for x in nodes)
            ans += len(roots) - 1

        out.append(str(ans))

    return "\n".join(out)

# provided samples
assert run("""5
3 2 1
1 2
2 3
1 3
2 1 1
1 2
1 2
3 2 0
3 2
1 2
1 0 0
3 3 1
1 2
1 3
2 3
1 2
""") == """3
0
2
0
2"""

# custom cases
assert run("""1
4 0 0
1
""") == "0", "empty graphs"

assert run("""1
4 3 1
1 2
2 3
3 4
1 2
""") == "2", "chain mismatch"

assert run("""1
3 3 1
1 2
2 3
1 3
1 2
""") == "2", "triangle vs edge"

assert run("""1
5 0 4
1 2
2 3
3 4
4 5
""") == "4", "build full connectivity"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đồ thị trống | 0 | căn chỉnh tầm thường | 
| chuỗi không khớp | 2 | xóa cộng với sáp nhập | 
| tam giác vs cạnh | 2 | các cạnh thừa trong F | 
| xây dựng kết nối đầy đủ | 4 | chi phí xây dựng thuần túy | 

## Vỏ cạnh 

Trường hợp F được kết nối đầy đủ nhưng G chia các đỉnh thành nhiều thành phần cho thấy tại sao việc đếm xóa là cần thiết. Nếu tất cả các cạnh trong F kết nối các thành phần G khác nhau thì mọi cạnh như vậy phải bị loại bỏ, nếu không đồ thị cuối cùng sẽ kết nối không chính xác các thành phần phải tách biệt. 

Một trường hợp khác trong đó G được kết nối đầy đủ nhưng F có nhiều thành phần cho thấy tác dụng ngược lại. Không cần xóa, nhưng mọi phần bị ngắt kết nối bên trong F phải được kết nối với đúng một cạnh ít hơn số lượng của nó. Thuật toán xử lý điều này một cách tự nhiên vì tất cả các đỉnh đều thuộc một nhóm G, do đó chỉ có cấu trúc DSU bên trong mới xác định được câu trả lời.
