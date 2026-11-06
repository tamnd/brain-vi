---
title: "CF 1238F - Cây con tối đa"
description: "Chúng ta được cho một cái cây và cần tìm đồ thị con liên thông lớn nhất có thể được biểu diễn dưới dạng đồ thị giao nhau của các đoạn thẳng trên một đường thẳng."
date: "2026-06-11T22:11:05+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "dp", "graphs", "trees"]
categories: ["algorithms"]
codeforces_contest: 1238
codeforces_index: "F"
codeforces_contest_name: "Educational Codeforces Round 74 (Rated for Div. 2)"
rating: 2200
weight: 1238
solve_time_s: 116
verified: true
draft: false
---

[CF 1238F - Cây con tối đa](https://codeforces.com/problemset/problem/1238/F) 

**Đánh giá:** 2200 
**Thẻ:** dfs và tương tự, dp, đồ thị, cây 
**Thời gian giải:** 1 phút 56s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một cái cây và cần tìm đồ thị con liên thông lớn nhất có thể được biểu diễn dưới dạng đồ thị giao nhau của các đoạn thẳng trên một đường thẳng. 

Biểu đồ được xây dựng từ các đoạn bằng cách tạo một đỉnh trên mỗi đoạn và nối hai đỉnh bất cứ khi nào các đoạn tương ứng giao nhau. Một số cây có thể được biểu diễn theo cách này và một số thì không. Trong số tất cả các sơ đồ con được kết nối của cây đã cho, chúng ta phải tìm sơ đồ con có số đỉnh tối đa có thể biểu diễn được. 

Đầu vào chứa tối đa$1.5 \cdot 10^5$các trường hợp thử nghiệm, nhưng tổng số đỉnh trên tất cả các cây nhiều nhất là$3 \cdot 10^5$. Giới hạn tổng hợp này là một trong những vấn đề quan trọng. Bất kỳ giải pháp nào tệ hơn đáng kể so với tuyến tính trên mỗi cây sẽ thất bại. MỘT$O(n^2)$thuật toán sẽ yêu cầu khoảng$9 \cdot 10^{10}$trong trường hợp xấu nhất là hoàn toàn không thể thực hiện được. Thậm chí$O(n \log n)$thì ổn, nhưng giải pháp dự định thực sự là tuyến tính. 

Phần khó khăn không phải là việc xử lý cây. Thách thức ở đây là hiểu được cây nào có thể biểu diễn dưới dạng biểu đồ giao điểm khoảng. 

Một kết quả cổ điển nói rằng đồ thị khoảng chính xác là đồ thị giao nhau của các khoảng trên một đường thẳng. Cây là đồ thị khoảng có cấu trúc rất hạn chế. Bất kỳ chu trình nào cũng không thể thực hiện được vì đồ thị khoảng là hợp âm, và trong số các cây thì đồ thị khoảng duy nhất là những con sâu bướm. 

Sâu bướm là một cái cây trở thành một con đường đơn giản sau khi loại bỏ hết lá. 

Sự quan sát đó làm thay đổi hoàn toàn vấn đề. Chúng ta không còn tìm kiếm một cây con tốt tùy ý nữa. Chúng tôi đang tìm kiếm cây con được kết nối lớn nhất là sâu bướm. 

Một số trường hợp cạnh rất dễ bị bỏ lỡ. 

Hãy xem xét một ngôi sao:```
1
5
1 2
1 3
1 4
1 5
```Toàn bộ cây là một con sâu bướm. Loại bỏ các lá để lại một đỉnh duy nhất. Câu trả lời là 5. 

Cách tiếp cận dựa trên đường kính đơn giản sẽ chỉ trả về sai 2. 

Hãy xem xét một cây nhị phân cân bằng có chiều cao 2:```
      1
    / | \
   2  3  4
  / \ / \
 5 6 7 8
```Câu trả lời là 8, không phải 7. Con sâu bướm tối ưu sử dụng một đường đi trung tâm và có thể giữ các lá gắn liền với các đỉnh của đường đi đó. 

Một trường hợp tế nhị khác là một đường dẫn đơn giản:```
1-2-3-4-5
```Toàn bộ cái cây đã là một con sâu bướm. Câu trả lời là 5. 

Bất kỳ DP nào chỉ tính các đỉnh phân nhánh và quên điểm cuối của đường dẫn sẽ đánh giá thấp trường hợp này. 

## Phương pháp tiếp cận 

Quan điểm vũ phu rất đơn giản. Liệt kê mọi cây con được kết nối và kiểm tra xem nó có phải là sâu bướm hay không. 

Việc kiểm tra xem một cái cây có phải là sâu bướm hay không rất dễ dàng. Loại bỏ tất cả các lá và kiểm tra xem đồ thị còn lại có phải là đường dẫn hay không. Vấn đề là số lượng cây con được kết nối. Ngay cả một cây có vài trăm đỉnh cũng đã có một số lượng lớn các đồ thị con được kết nối. Cách tiếp cận này là vô vọng. 

Quan sát chính là cấu trúc. 

Một cây được biểu diễn dưới dạng đồ thị khoảng khi và chỉ nếu nó là một con sâu bướm. Một con sâu bướm bao gồm một đường xương sống, thường được gọi là cột sống, và các lá tùy ý gắn vào các đỉnh của đường đi đó. 

Thay vì suy nghĩ về cách biểu diễn khoảng, chúng ta có thể nghĩ đến việc chọn một đường đi. Mỗi đỉnh trên đường đi đó có thể đóng góp tất cả các lá lân cận của nó. Bất kỳ nhánh nào kéo dài hơn một cạnh so với đường đi sẽ vi phạm thuộc tính của sâu bướm. 

Giả sử một đỉnh thuộc về cột sống. Một cạnh sự cố được sử dụng để kết nối với cha mẹ của nó trên cột sống, một cạnh khác có thể tiếp tục cột sống đi xuống và mọi cạnh sự cố còn lại có thể đóng góp chính xác một đỉnh lá. 

Điều này tự nhiên gợi ý một cây DP. 

Hãy để một cây có rễ được cố định. Chúng tôi muốn một trạng thái mô tả con sâu bướm tốt nhất có cột sống bắt đầu từ đỉnh hiện tại và đi xuống. Khi cột sống đi qua một đỉnh, mỗi đứa trẻ không sử dụng sẽ đóng góp một lá. Một đứa trẻ có thể được chọn để tiếp tục cột sống. 

Điều này biến vấn đề thành một DP kiểu đường đi dài nhất trong đó mỗi đỉnh đóng góp```
(number of available leaves) + 1
```và một đứa trẻ có thể tiếp tục chuỗi. 

Câu trả lời cuối cùng có được bằng cách kết hợp hai phần mở rộng cột sống hướng xuống tốt nhất qua từng đỉnh, giống hệt đường kính DP. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Cây tối ưu DP | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Giả sử cây có gốc ở một đỉnh tùy ý, chẳng hạn như đỉnh 1. 

Xác định:`dp[v]`= kích thước tối đa của cây con sâu bướm có cột sống bắt đầu từ`v`và chỉ tiếp tục ở con cháu. 

Đối với một đỉnh`v`, cho phép```
base(v) = 1 + (deg(v) - 1)
```đối với các đỉnh không có gốc. 

các`1`đếm chính đỉnh đó. 

Trong số các cạnh tới của nó, một cạnh đi về phía cha mẹ của nó và không thể trở thành một chiếc lá. Mọi cạnh sự cố không được sử dụng khác đều có thể đóng góp một lá. 

Đối với gốc:```
base(root) = 1 + deg(root)
```vì không có cạnh cha. 

Bây giờ xử lý các đỉnh từ dưới lên. 

### 1. Tính phần đóng góp của mỗi đỉnh mà không kéo dài cột sống 

Nếu cột sống kết thúc ở`v`, tất cả các cạnh con đều trở thành lá. 

Giá trị chỉ đơn giản là`base(v)`. 

### 2. Thử kéo dài cột sống qua một đứa trẻ 

Giả sử cột sống tiếp tục xuyên suốt đứa trẻ`u`. 

Cạnh để`u`không còn có sẵn dưới dạng lá, vì vậy chúng tôi mất đi một lá đóng góp. 

Giá trị kết quả là```
base(v) - 1 + dp[u]
```Tận dụng tối đa tất cả trẻ em. 

Điều này mang lại`dp[v]`. 

### 3. Tính con sâu tốt nhất đi qua mỗi đỉnh 

Một điểm tối ưu toàn cục có thể có cột sống đi qua`v`và tiếp tục theo hai hướng trẻ khác nhau. 

Cho phép```
gain(u) = dp[u]
```cho mỗi đứa trẻ. 

Lấy hai lợi ích con lớn nhất. 

Con sâu bướm tập trung ở`v`có kích thước```
base(v) + best1 + best2 - 2
```Phép trừ 2 xuất phát từ việc sử dụng hai cạnh con làm cạnh gai thay vì cạnh lá. 

Đối với gốc, công thức hơi khác một chút vì không có cạnh cha nào bị loại trừ. 

Việc sử dụng cách triển khai hợp nhất bên dưới sẽ dễ dàng hơn và tránh được lý do trong trường hợp đặc biệt. 

### 4. Cập nhật câu trả lời chung 

Đối với mỗi đỉnh, hãy cập nhật câu trả lời với con sâu bướm tốt nhất có cột sống đi qua đỉnh đó. 

### 5. Xử lý tất cả các trường hợp kiểm thử 

Vì tổng số đỉnh nhiều nhất là$3 \cdot 10^5$, một DFS duy nhất cho mỗi cây là đủ. 

### Tại sao nó hoạt động 

Một con sâu bướm hoàn toàn được xác định bởi đường đi của cột sống. Mỗi đỉnh trên cột sống có thể sử dụng tối đa hai cạnh liên quan để duy trì trên cột sống. Tất cả các cạnh sự cố khác chỉ có thể đóng góp lá. 

Trạng thái DP đại diện cho con sâu bướm tối ưu có cột sống đi vào một đỉnh từ phía trên và tiếp tục đi xuống qua nhiều nhất một con. Khi kết hợp hai phần mở rộng con tốt nhất tại một đỉnh, chúng ta xây dựng mọi đường cột sống có thể có mà đỉnh cao nhất là đỉnh đó. Mỗi con sâu bướm có chính xác một đỉnh cột sống cao nhất như vậy trong cây có gốc, vì vậy mọi con sâu bướm hợp lệ đều được xem xét. DP luôn tính đến mọi đỉnh có thể trở thành một chiếc lá một cách hợp pháp và loại trừ mọi đỉnh sẽ tạo ra khoảng cách lớn hơn một so với cột sống. Do đó mức tối đa được tính toán chính xác là cây con sâu bướm lớn nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    anss = []

    for _ in range(t):
        n = int(input())

        g = [[] for _ in range(n)]

        for _ in range(n - 1):
            u, v = map(int, input().split())
            u -= 1
            v -= 1
            g[u].append(v)
            g[v].append(u)

        parent = [-1] * n
        order = [0]
        parent[0] = 0

        for v in order:
            for to in g[v]:
                if parent[to] == -1:
                    parent[to] = v
                    order.append(to)

        dp = [0] * n
        answer = 0

        for v in reversed(order):
            deg = len(g[v])

            if v == 0:
                base = deg + 1
            else:
                base = deg

            best1 = 0
            best2 = 0

            dp[v] = base

            for to in g[v]:
                if to == parent[v]:
                    continue

                cand = base - 1 + dp[to]
                if cand > dp[v]:
                    dp[v] = cand

                val = dp[to]
                if val > best1:
                    best2 = best1
                    best1 = val
                    best1_child = to
                elif val > best2:
                    best2 = val

            answer = max(answer, dp[v])

            cur = base

            if best1:
                cur += best1 - 1
            if best2:
                cur += best2 - 1

            answer = max(answer, cur)

        anss.append(str(answer))

    sys.stdout.write("\n".join(anss))

if __name__ == "__main__":
    solve()
```Cây được root lặp đi lặp lại để tránh các vấn đề về độ sâu đệ quy. Với tối đa$3 \cdot 10^5$các đỉnh, DFS đệ quy có nhiều rủi ro trong Python.`base`thể hiện sự đóng góp của đỉnh hiện tại nếu không có trẻ nào tiếp tục cột sống. Đối với đỉnh không phải gốc, một cạnh đã được dành riêng cho hướng gốc, do đó chỉ các cạnh liên quan còn lại mới có thể trở thành lá.`dp[v]`lưu trữ con sâu bướm tốt nhất có cột sống đi vào`v`từ trên cao và có thể tiếp tục thông qua một đứa trẻ. Việc mở rộng qua một phần tử con sẽ tiêu tốn một cạnh lá tiềm năng, đó là lý do tại sao quá trình chuyển đổi sử dụng`base - 1 + dp[child]`. 

Câu trả lời tổng thể cũng phải xét đến những con sâu bướm có cột sống đi qua một đỉnh và tiếp tục đi theo hai hướng con. Điều này tương tự với bước kết hợp đường kính cây tiêu chuẩn, vì vậy chúng tôi giữ hai giá trị DP con lớn nhất. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
5
1 2
1 3
1 4
1 5
```Cây là một ngôi sao. 

| Đỉnh | Bằng cấp | Căn cứ | DP trẻ em xuất sắc nhất | DP | 
| --- | --- | --- | --- | --- | 
| 2 | 1 | 1 | 0 | 1 | 
| 3 | 1 | 1 | 0 | 1 | 
| 4 | 1 | 1 | 1 | 1 | 
| 5 | 1 | 1 | 0 | 1 | 
| 1 | 4 | 5 | 1 | 5 | 

Câu trả lời trở thành 5. 

Điều này chứng tỏ rằng bản thân một ngôi sao cũng là một con sâu bướm. Tâm tạo thành một cột sống có chiều dài bằng một và mọi đỉnh còn lại là một chiếc lá. 

### Ví dụ 2 

đầu vào:```
1
5
1 2
2 3
3 4
4 5
```Một con đường đơn giản. 

| Đỉnh | Bằng cấp | Căn cứ | DP con | DP | 
| --- | --- | --- | --- | --- | 
| 5 | 1 | 1 | 0 | 1 | 
| 4 | 2 | 2 | 1 | 2 | 
| 3 | 2 | 2 | 2 | 3 | 
| 2 | 2 | 2 | 3 | 4 | 
| 1 | 1 | 2 | 4 | 5 | 

Câu trả lời là 5. 

Dấu vết này cho thấy cột sống kéo dài qua chính xác một đứa trẻ ở mỗi bước như thế nào, tái tạo lại toàn bộ con đường. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi cạnh được xử lý một số lần không đổi | 
| Không gian | O(n) | Danh sách kề, mảng cha, thứ tự truyền tải, mảng DP | 

Tổng của tất cả các đỉnh trong các trường hợp thử nghiệm nhiều nhất là$3 \cdot 10^5$. Một giải pháp tuyến tính chỉ xử lý vài triệu thao tác nguyên thủy, thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())

        g = [[] for _ in range(n)]

        for _ in range(n - 1):
            u, v = map(int, input().split())
            u -= 1
            v -= 1
            g[u].append(v)
            g[v].append(u)

        parent = [-1] * n
        order = [0]
        parent[0] = 0

        for v in order:
            for to in g[v]:
                if parent[to] == -1:
                    parent[to] = v
                    order.append(to)

        dp = [0] * n
        ans = 0

        for v in reversed(order):
            deg = len(g[v])

            base = deg + 1 if v == 0 else deg

            best1 = best2 = 0
            dp[v] = base

            for to in g[v]:
                if to == parent[v]:
                    continue

                dp[v] = max(dp[v], base - 1 + dp[to])

                val = dp[to]
                if val > best1:
                    best2 = best1
                    best1 = val
                elif val > best2:
                    best2 = val

            ans = max(ans, dp[v])

            cur = base
            if best1:
                cur += best1 - 1
            if best2:
                cur += best2 - 1

            ans = max(ans, cur)

        out.append(str(ans))

    return "\n".join(out)

# star
assert run("""1
5
1 2
1 3
1 4
1 5
""") == "5"

# path
assert run("""1
5
1 2
2 3
3 4
4 5
""") == "5"

# minimum tree
assert run("""1
2
1 2
""") == "2"

# balanced binary tree from statement style
assert run("""1
10
1 2
1 3
1 4
2 5
2 6
3 7
3 8
4 9
4 10
""") == "8"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cây hai nút | 2 | Kích thước hợp lệ tối thiểu | 
| Ngôi sao | 5 | Nhiều lá gắn vào một đỉnh cột sống | 
| Đường dẫn | 5 | Toàn bộ cây dùng làm cột sống | 
| Cây phân nhánh cân đối | 8 | Kết hợp hai bài tập kéo giãn cột sống tốt nhất | 

## Vỏ cạnh 

Xét cây tối thiểu:```
1
2
1 2
```Cả hai đỉnh tạo thành một đường đi, đường này tự động trở thành một con sâu bướm. Gốc được`base = 2`, chiếc lá được`base = 1`, và DP trả về 2. 

Hãy xem xét một ngôi sao:```
1
5
1 2
1 3
1 4
1 5
```Trung tâm tự đóng góp cộng với cả bốn người hàng xóm. Không cần mở rộng cột sống. Thuật toán đánh giá điều này trực tiếp thông qua`base`giá trị của trung tâm và trả về 5. 

Hãy xem xét một con đường dài:```
1
6
1 2
2 3
3 4
4 5
5 6
```Mỗi đỉnh bên trong kéo dài cột sống qua đúng một đứa trẻ. DP tích lũy thêm một đỉnh ở mỗi bước và đạt tới 6. Không cần đóng góp lá. 

Hãy xem xét một cái cây có nhiều nhánh, nơi có nhiều đứa trẻ tranh nhau giành lấy cột sống. Thuật toán giữ hai giá trị DP con lớn nhất. Mỗi gai sâu bướm đi qua một đỉnh có thể sử dụng nhiều nhất hai hướng, do đó chỉ giữ lại hai giá trị tốt nhất là đủ và không có giải pháp tối ưu nào bị loại bỏ.
