---
title: "CF 1881F - Khoảng cách tối đa tối thiểu"
description: "Chúng ta được cho một cây và một tập con các đỉnh của nó được đánh dấu là đặc biệt. Với mọi đỉnh $v$, hãy xác định $f(v)$ là khoảng cách từ $v$ đến đỉnh được đánh dấu xa nhất. Chúng ta muốn chọn một đỉnh có đỉnh được đánh dấu xa nhất càng gần càng tốt."
date: "2026-06-08T22:42:23+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "dp", "graphs", "shortest-paths", "trees"]
categories: ["algorithms"]
codeforces_contest: 1881
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 903 (Div. 3)"
rating: 1700
weight: 1881
solve_time_s: 140
verified: true
draft: false
---

[CF 1881F - Khoảng cách tối đa tối thiểu](https://codeforces.com/problemset/problem/1881/F) 

**Đánh giá:** 1700 
**Thẻ:** dfs và tương tự, dp, đồ thị, đường đi ngắn nhất, cây 
**Thời gian giải:** 2 phút 20s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một cây và một tập con các đỉnh của nó được đánh dấu là đặc biệt. 

Với mọi đỉnh$v$, định nghĩa$f(v)$như khoảng cách từ$v$đến đỉnh được đánh dấu xa nhất. Chúng ta muốn chọn một đỉnh có đỉnh được đánh dấu xa nhất càng gần càng tốt. Đáp án là giá trị nhỏ nhất của$f(v)$trên tất cả các đỉnh. 

Nhìn theo cách khác, hãy tưởng tượng tất cả các đỉnh được đánh dấu là những vị trí quan trọng. Đối với mỗi đỉnh ứng cử viên, hãy xem khoảng cách tồi tệ nhất tới bất kỳ đỉnh được đánh dấu nào. Chúng ta cần đỉnh có thể giảm thiểu khoảng cách trong trường hợp xấu nhất này. 

Cây có thể chứa tới$2 \cdot 10^5$đỉnh trên tất cả các trường hợp thử nghiệm. Bất kỳ thuật toán nào thực hiện việc truyền tải từ mọi đỉnh sẽ yêu cầu$O(n^2)$hoạt động trong trường hợp xấu nhất, vượt xa những gì phù hợp trong hai giây. Chúng tôi cần một giải pháp gần với thời gian tuyến tính cho mỗi trường hợp thử nghiệm. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Xét một cây chỉ có một đỉnh được đánh dấu:```
1
5 1
3
1 2
2 3
3 4
4 5
```Câu trả lời đúng là`0`, bởi vì việc chọn đỉnh 3 sẽ cho khoảng cách 0 tới đỉnh được đánh dấu duy nhất. Giải pháp giả định hai điểm cuối luôn tồn tại sẽ thất bại ở đây. 

Hãy xem xét một đường dẫn trong đó mọi đỉnh đều được đánh dấu:```
1
4 4
1 2 3 4
1 2
2 3
3 4
```Câu trả lời là`2`. Các đỉnh tốt nhất là 2 và 3, có đỉnh được đánh dấu xa nhất ở khoảng cách 2. Chỉ nhìn vào các đỉnh được đánh dấu gần đó sẽ bỏ lỡ ảnh hưởng của các điểm cuối. 

Một trường hợp tinh vi khác là khi các đỉnh được đánh dấu tạo thành một cụm nhỏ bên trong một cây lớn:```
1
7 2
3 4
1 2
2 3
3 4
4 5
5 6
6 7
```Câu trả lời là`1`. Các đỉnh tối ưu là 3 và 4. Một cách tiếp cận bất cẩn cố gắng cân bằng khoảng cách đến tất cả các đỉnh thay vì chỉ các đỉnh được đánh dấu sẽ tạo ra giá trị lớn hơn. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp là tính toán$f(v)$cho mỗi đỉnh riêng biệt. 

Đối với một đỉnh cố định$v$, chúng ta có thể chạy BFS hoặc DFS để lấy khoảng cách tới tất cả các đỉnh, sau đó lấy khoảng cách tối đa giữa các đỉnh được đánh dấu. Lặp lại điều này cho mọi đỉnh sẽ cho$O(n^2)$thời gian trên cây. Với$n = 2 \cdot 10^5$, điều này sẽ yêu cầu khoảng$4 \cdot 10^{10}$hoạt động trong trường hợp xấu nhất. 

Quan sát quan trọng là chúng ta không quan tâm đến tất cả các đỉnh được đánh dấu riêng lẻ. Chúng tôi chỉ quan tâm đến cái xa nhất. 

Giả sử chúng ta chỉ nhìn vào các đỉnh được đánh dấu và tìm thấy hai đỉnh được đánh dấu có khoảng cách lớn nhất trong số tất cả các cặp được đánh dấu. Gọi cho họ$A$Và$B$. Chúng tạo thành đường kính của bộ được đánh dấu. 

Bây giờ hãy xem xét bất kỳ đỉnh nào$v$. Cho phép$$d_A(v)=\operatorname{dist}(v,A)$$Và$$d_B(v)=\operatorname{dist}(v,B).$$Một tính chất quan trọng của cây là với mọi đỉnh được đánh dấu$x$,$$\operatorname{dist}(v,x) \le \max(d_A(v), d_B(v)).$$Điều này có nghĩa là đỉnh được đánh dấu xa nhất tính từ$v$luôn là một trong hai điểm cuối đường kính. 

Vì thế$$f(v)=\max(d_A(v), d_B(v)).$$Khi đã biết điểm cuối của đường kính, chúng ta chỉ cần khoảng cách từ$A$và từ$B$. Sau đó, chúng tôi đánh giá biểu thức trên cho mọi đỉnh và lấy mức tối thiểu. 

Câu hỏi còn lại là làm thế nào để tìm được đường kính đã đánh dấu. Đây là thủ thuật DFS kép tiêu chuẩn, được sửa đổi một chút. 

Bắt đầu từ bất kỳ đỉnh được đánh dấu nào và tìm đỉnh được đánh dấu xa nhất$A$. Sau đó bắt đầu từ$A$và tìm đỉnh được đánh dấu xa nhất$B$. Con đường giữa$A$Và$B$là đường kính giữa các đỉnh được đánh dấu. 

Sau đó, một DFS từ$A$và một DFS từ$B$cung cấp tất cả các khoảng cách cần thiết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n)$mỗi trường hợp thử nghiệm |$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc cây và danh sách các đỉnh đã đánh dấu. 
2. Chọn bất kỳ đỉnh nào được đánh dấu làm điểm bắt đầu. 
3. Chạy DFS từ đỉnh này và tính khoảng cách đến mọi đỉnh. 
4. Trong số tất cả các đỉnh được đánh dấu, hãy chọn đỉnh có khoảng cách lớn nhất. Gọi nó$A$. 

Đỉnh này phải là điểm cuối của đường kính được đánh dấu. 
5. Chạy một DFS khác từ$A$. 
6. Trong số tất cả các đỉnh được đánh dấu, hãy chọn đỉnh xa nhất$A$. Gọi nó$B$. 

Cặp đôi$(A,B)$tạo thành đường kính của các đỉnh được đánh dấu. 
7. Lưu giữ mọi khoảng cách từ$A$. Gọi mảng này`distA`. 
8. Chạy DFS thứ ba từ$B$và lưu trữ tất cả khoảng cách trong`distB`. 
9. Với mọi đỉnh$v$, tính toán$$\max(\text{distA}[v], \text{distB}[v]).$$Điều này bằng khoảng cách từ$v$đến đỉnh được đánh dấu xa nhất của nó. 
10. Lấy giá trị nhỏ nhất trong số tất cả các đỉnh và xuất ra. 

### Tại sao nó hoạt động 

hãy để$A$Và$B$là điểm cuối của đường kính các đỉnh được đánh dấu. 

Đối với bất kỳ đỉnh được đánh dấu$x$, hàm ý đường kính cực đại$$\operatorname{dist}(A,x)\le \operatorname{dist}(A,B)$$Và$$\operatorname{dist}(B,x)\le \operatorname{dist}(A,B).$$Thuộc tính đường kính cây tiêu chuẩn cho biết rằng với mỗi đỉnh$v$, đỉnh được đánh dấu xa nhất tính từ$v$phải là một trong những điểm cuối đường kính. Do đó,$$\max_{x \text{ marked}} \operatorname{dist}(v,x) = \max( \operatorname{dist}(v,A), \operatorname{dist}(v,B) ).$$Thuật toán tính toán chính xác hai khoảng cách này cho mỗi đỉnh và giảm thiểu mức tối đa của chúng. Vì mọi giá trị ứng cử viên đều được tính toán chính xác nên giá trị tối thiểu cũng chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        n, k = map(int, input().split())
        marked = list(map(int, input().split()))

        g = [[] for _ in range(n + 1)]

        for _ in range(n - 1):
            u, v = map(int, input().split())
            g[u].append(v)
            g[v].append(u)

        def distances(start):
            dist = [-1] * (n + 1)
            dist[start] = 0

            stack = [start]

            while stack:
                v = stack.pop()

                for to in g[v]:
                    if dist[to] == -1:
                        dist[to] = dist[v] + 1
                        stack.append(to)

            return dist

        start = marked[0]

        dist0 = distances(start)

        A = max(marked, key=lambda x: dist0[x])

        distA = distances(A)

        B = max(marked, key=lambda x: distA[x])

        distB = distances(B)

        ans = min(max(distA[v], distB[v]) for v in range(1, n + 1))

        print(ans)

solve()
```Chức năng trợ giúp`distances()`thực hiện một DFS lặp và trả về khoảng cách từ một nguồn đến mọi đỉnh. Truyền tải lặp lại an toàn hơn DFS đệ quy vì cây có thể chứa tới$2 \cdot 10^5$các đỉnh sẽ vượt quá độ sâu đệ quy của Python. 

Lần duyệt đầu tiên tìm thấy một đỉnh được đánh dấu nằm ở một đầu của đường kính được đánh dấu. Lần duyệt thứ hai xác định điểm cuối đối diện. Đây chính xác là các bước tương tự được sử dụng để tìm đường kính cây, ngoại trừ đỉnh xa nhất chỉ được chọn trong số các đỉnh được đánh dấu. 

Lần duyệt thứ ba là cần thiết vì chúng ta cần khoảng cách từ cả hai điểm cuối đường kính cùng một lúc. Một lần`distA`Và`distB`có sẵn, biểu thức`max(distA[v], distB[v])`đưa ra khoảng cách được đánh dấu xa nhất cho đỉnh`v`. 

Một sai lầm phổ biến là giảm thiểu`distA[v] + distB[v]`hoặc một số sự kết hợp khác. Bài toán yêu cầu khoảng cách được đánh dấu tệ nhất, do đó biểu thức đúng là khoảng cách lớn nhất của hai điểm cuối. 

## Ví dụ đã hoạt động 

### Ví dụ 1```
7 3
Marked: 2 6 7
```Cây đó là:```
    1
   / \
  2   3
 / \ / \
4  5 6  7
```Điểm cuối đường kính được đánh dấu là 2 và 6. 

| Đỉnh | distA (từ 2) | distB (từ 6) | tối đa | 
| --- | --- | --- | --- | 
| 1 | 1 | 2 | 2 | 
| 2 | 0 | 3 | 3 | 
| 3 | 2 | 1 | 2 | 
| 4 | 1 | 4 | 4 | 
| 5 | 1 | 4 | 4 | 
| 6 | 3 | 0 | 3 | 
| 7 | 3 | 2 | 3 | 

Giá trị nhỏ nhất ở cột cuối cùng là`2`, đạt được ở đỉnh 1 và 3. 

Ví dụ này cho thấy thuộc tính trung tâm của giải pháp. Khi đã biết điểm cuối của đường kính, câu trả lời của mọi đỉnh chỉ xuất phát từ hai khoảng cách đó. 

### Ví dụ 2```
4 4
Marked: 1 2 3 4
1-2-3-4
```Điểm cuối đường kính được đánh dấu là 1 và 4. 

| Đỉnh | quậnA | quận B | tối đa | 
| --- | --- | --- | --- | 
| 1 | 0 | 3 | 3 | 
| 2 | 1 | 2 | 2 | 
| 3 | 2 | 1 | 2 | 
| 4 | 3 | 0 | 3 | 

Giá trị tối thiểu là`2`. 

Điều này chứng tỏ rằng khi mỗi đỉnh được đánh dấu, bài toán giảm xuống còn việc tìm tâm của đường kính. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Ba lần duyệt cây đầy đủ và quét tuyến tính | 
| Không gian |$O(n)$| Danh sách kề và mảng khoảng cách | 

Tổng của tất cả$n$giá trị trên các trường hợp thử nghiệm là nhiều nhất$2 \cdot 10^5$. Xử lý tuyến tính cho mỗi trường hợp thử nghiệm có nghĩa là tổng công việc cũng tuyến tính trong kích thước đầu vào tổng thể, dễ dàng nằm gọn trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline
    out = []

    t = int(input())

    for _ in range(t):
        n, k = map(int, input().split())
        marked = list(map(int, input().split()))

        g = [[] for _ in range(n + 1)]

        for _ in range(n - 1):
            u, v = map(int, input().split())
            g[u].append(v)
            g[v].append(u)

        def distances(start):
            dist = [-1] * (n + 1)
            dist[start] = 0
            stack = [start]

            while stack:
                v = stack.pop()
                for to in g[v]:
                    if dist[to] == -1:
                        dist[to] = dist[v] + 1
                        stack.append(to)

            return dist

        d0 = distances(marked[0])
        A = max(marked, key=lambda x: d0[x])

        dA = distances(A)
        B = max(marked, key=lambda x: dA[x])

        dB = distances(B)

        out.append(str(min(max(dA[i], dB[i]) for i in range(1, n + 1))))

    return "\n".join(out)

# provided sample
assert run(
"""6
7 3
2 6 7
1 2
1 3
2 4
2 5
3 6
3 7
4 4
1 2 3 4
1 2
2 3
3 4
5 1
1
1 2
1 3
1 4
1 5
5 2
4 5
1 2
2 3
1 4
4 5
10 8
1 2 3 4 5 8 9 10
2 10
10 5
5 3
3 1
1 7
7 4
4 9
8 9
6 1
10 9
1 2 4 5 6 7 8 9 10
1 3
3 9
9 4
4 10
10 6
6 7
7 2
2 5
5 8
"""
) == "2\n2\n0\n1\n4\n5"

# single vertex
assert run(
"""1
1 1
1
"""
) == "0"

# path with one marked endpoint
assert run(
"""1
5 1
5
1 2
2 3
3 4
4 5
"""
) == "0"

# two marked endpoints of a path
assert run(
"""1
5 2
1 5
1 2
2 3
3 4
4 5
"""
) == "2"

# star centered at marked vertex
assert run(
"""1
5 1
1
1 2
1 3
1 4
1 5
"""
) == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cây đỉnh đơn | 0 | Đầu vào nhỏ nhất có thể | 
| Một điểm cuối được đánh dấu trên đường dẫn | 0 | Xử lý$k=1$| 
| Đường dẫn có điểm cuối được đánh dấu | 2 | Hành vi đường kính-tâm | 
| Ngôi sao được đánh dấu ở giữa | 0 | Khoảng cách từ nút được đánh dấu duy nhất | 

## Vỏ cạnh 

Hãy xem xét một đỉnh được đánh dấu duy nhất.```
1
5 1
3
1 2
2 3
3 4
4 5
```Tìm kiếm đường kính đầu tiên tìm thấy đỉnh 3. Tìm kiếm thứ hai cũng tìm thấy đỉnh 3. Do đó$A=B=3$. Mỗi đỉnh sử dụng`max(distA, distB)`, đơn giản là khoảng cách của nó tới đỉnh 3. Điểm cực tiểu xảy ra ở đỉnh 3 và bằng 0. 

Xem xét tất cả các đỉnh được đánh dấu trên một đường dẫn.```
1
4 4
1 2 3 4
1 2
2 3
3 4
```Đường kính được đánh dấu là toàn bộ đường đi, có điểm cuối 1 và 4. Khoảng cách từ các điểm cuối đó xác định chính xác đỉnh 2 và 3 là tâm. Câu trả lời trở thành 2. 

Hãy xem xét các đỉnh được đánh dấu tập trung ở một khu vực.```
1
7 2
3 4
1 2
2 3
3 4
4 5
5 6
6 7
```Điểm cuối đường kính là 3 và 4. Khoảng cách từ các điểm cuối này cho: 

| Đỉnh | max(dist(3), dist(4)) | 
| --- | --- | 
| 1 | 3 | 
| 2 | 2 | 
| 3 | 1 | 
| 4 | 1 | 
| 5 | 2 | 
| 6 | 3 | 
| 7 | 4 | 

Tối thiểu là 1, đạt được ở đỉnh 3 và 4. Thuật toán chỉ tập trung vào các đỉnh được đánh dấu, chính xác theo yêu cầu của định nghĩa bài toán.
