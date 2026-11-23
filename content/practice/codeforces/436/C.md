---
title: "CF 436C - Ngục tối và kẹo"
description: "Chúng tôi được cấp k cấp độ trò chơi. Mỗi cấp độ là một lưới ký tự n × m. Một cấp độ có thể được truyền theo hai cách khác nhau. Tùy chọn đầu tiên là gửi toàn bộ lưới từ đầu. Vì mọi tế bào đều phải được truyền đi nên chi phí là n m."
date: "2026-06-07T02:53:47+07:00"
tags: ["codeforces", "competitive-programming", "dsu", "graphs", "greedy", "trees"]
categories: ["algorithms"]
codeforces_contest: 436
codeforces_index: "C"
codeforces_contest_name: "Zepto Code Rush 2014"
rating: 1800
weight: 436
solve_time_s: 269
verified: true
draft: false
---

[CF 436C - Dungeon và Kẹo](https://codeforces.com/problemset/problem/436/C) 

**Đánh giá:** 1800 
**Tags:** dsu, đồ thị, tham lam, cây cối 
**Thời gian giải:** 4 phút 29s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được trao`k`cấp độ trò chơi. Mỗi cấp độ là một`n × m`lưới ký tự. Một cấp độ có thể được truyền theo hai cách khác nhau. 

Tùy chọn đầu tiên là gửi toàn bộ lưới từ đầu. Vì mọi tế bào đều phải được truyền đi nên chi phí là`n * m`. 

Tùy chọn thứ hai là chỉ gửi những khác biệt liên quan đến mức độ nào đó đã được truyền đi. Nếu hai cấp độ khác nhau về`d`tế bào, chi phí trở thành`d * w`. 

Thứ tự truyền không cố định. Chúng tôi có thể chọn bất kỳ cấp độ nào để gửi trước và mọi cấp độ sau có thể được xây dựng từ đầu hoặc được xây dựng lại từ một cấp độ đã truyền trước đó. 

Đầu ra yêu cầu hai điều. Đầu tiên, tổng chi phí truyền tải tối thiểu có thể. Thứ hai, một kế hoạch truyền tải thực tế. Đối với mọi cấp độ, chúng tôi phải xác định rằng nó được gửi từ đầu (`parent = 0`) hoặc nó được xây dựng lại từ mức được truyền trước đó. 

Các ràng buộc tiết lộ hướng dự định. Mỗi cấp độ chứa tối đa`10 × 10 = 100`tế bào. Có thể có tới`1000`cấp độ. Việc tính toán sự khác biệt giữa hai cấp độ tốn tối đa 100 so sánh ký tự, do đó, tất cả khoảng cách theo cặp có thể được tính trong khoảng`1000² × 100 = 10^8`hoạt động trong trường hợp xấu nhất. Con số này lớn nhưng vẫn có thể quản lý được trong C++ được tối ưu hóa và được chấp nhận cho vấn đề này. Khi đã biết chi phí theo cặp, chúng ta cần một thuật toán đồ thị chỉ trên 1000 đỉnh. 

Phần tinh tế là việc truyền một cấp độ từ đầu luôn được cho phép. Một sai lầm phổ biến là nghĩ rằng mọi cấp độ đều phải được kết nối với cấp độ khác. Coi như:```
1 1 2 100
A
B
```Chi phí chuyển đổi là`100`, trong khi gửi một cấp độ từ đầu chỉ tốn phí`1`. Câu trả lời tối ưu sẽ gửi cả hai cấp độ một cách độc lập với tổng chi phí`2`. 

Một sai lầm dễ mắc phải khác là quên rằng việc chuyển đổi chỉ hữu ích khi nó rẻ hơn việc xây dựng lại. Giả định:```
1 2 2 5
AA
AB
```Các mức khác nhau trong một ô, do đó chi phí chuyển đổi là`1 * 5 = 5`. Chỉ mất phí gửi từ đầu`2`. Giải pháp tối ưu bỏ qua hoàn toàn cạnh chuyển đổi. 

Cạm bẫy thứ ba liên quan đến trật tự truyền tải. Nếu cấp 3 được xây dựng lại từ cấp 1 thì cấp 1 phải xuất hiện sớm hơn trong đầu ra. Bất kỳ giải pháp hợp lệ nào cũng phải tạo ra thứ tự phù hợp với các phụ thuộc đã chọn. 

## Phương pháp tiếp cận 

Một quan điểm thô bạo là suy nghĩ trực tiếp về các lệnh truyền tải và sự lựa chọn của phụ huynh. Đối với mọi cấp độ, chúng tôi có thể quyết định xem nó được gửi từ đầu hay bắt nguồn từ cấp độ khác, đồng thời tôn trọng yêu cầu cấp độ cha mẹ xuất hiện sớm hơn trong trình tự truyền. 

Điều này rõ ràng là chính xác vì mọi chiến lược truyền tải hợp lệ đều có thể được mô tả bằng những quyết định như vậy. Thật không may, số lượng các khả năng có thể tăng theo cấp số nhân với`k`, khiến nó hoàn toàn không khả thi ngay cả đối với vài chục cấp độ. 

Quan sát quan trọng là vấn đề bí mật là một vấn đề đồ thị. 

Tạo một đỉnh cho mọi cấp độ. Đồng thời tạo thêm một đỉnh ảo đại diện cho "xây dựng từ đầu". Đánh số đỉnh này`0`. 

Cấp độ kết nối`i`trực tiếp đến đỉnh`0`có nghĩa là mức độ truyền tải`i`từ đầu. Chi phí của một lợi thế như vậy luôn luôn`n * m`. 

Đối với mỗi cặp cấp độ`i`Và`j`, tính xem có bao nhiêu ô khác nhau. Xây dựng lại một từ các chi phí khác`difference(i, j) * w`. 

Điều này trở thành trọng số của một cạnh giữa`i`Và`j`. 

Bây giờ hãy xem xét bất kỳ chiến lược truyền tải hợp lệ nào. Mọi cấp độ đều đến từ đầu hoặc từ chính xác một cấp độ trước đó. Điều đó có nghĩa là mọi cấp độ đều chọn chính xác một phụ huynh. Cấu trúc kết quả là một cây có gốc ở đỉnh`0`. 

Ngược lại, cây nào có gốc tại đỉnh`0`mô tả một chiến lược truyền tải hợp lệ. Một cấp độ được kết nối trực tiếp với`0`được gửi từ đầu. Một cấp độ kết nối với một cấp độ khác được xây dựng lại từ nó. 

Tổng chi phí truyền tải chính xác bằng tổng trọng số của các cạnh trong cây này. 

Bài toán đã được chuyển thành việc tìm cây có chi phí tối thiểu nối tất cả các đỉnh, chính xác là Cây bao trùm tối thiểu. 

Khi MST được tìm thấy, chúng tôi root nó ở đỉnh`0`và xuất ra cấp độ gốc của mọi cấp độ theo thứ tự DFS hoặc BFS. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu (MST) | O(k² · n · m + k² log k) | O(k²) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả`k`cấp độ và lưu trữ lưới của họ. 
2. Tạo đồ thị có các đỉnh`0...k`, ở đâu có đỉnh`0`là nút "xây dựng từ đầu" ảo. 
3. Dành cho mọi cấp độ`i`, thêm một cạnh`(0, i)`với trọng lượng`n * m`. 

Cạnh này thể hiện mức độ truyền`i`trực tiếp. 
4. Đối với mỗi cặp cấp độ`(i, j)`, đếm xem có bao nhiêu ô khác nhau. 

Nhân số này với`w`để có được chi phí chuyển đổi. 
5. Thêm một cạnh giữa`i`Và`j`với chi phí đó. 
6. Chạy thuật toán Kruskal trên tất cả các cạnh. 

MST chọn chính xác sự kết hợp rẻ nhất giữa truyền trực tiếp và biến đổi. 
7. Xây dựng danh sách lân cận của MST. 
8. Root MST ở đỉnh`0`và thực hiện DFS hoặc BFS. 
9. Mỗi lần chúng ta ghé thăm một đỉnh cùng mức: 

- Nếu cha mẹ của nó là`0`, đầu ra`(level, 0)`. 
- Nếu không thì xuất ra`(level, parent)`. 
10. Tính tổng trọng số của tất cả các cạnh MST và in tổng chi phí. 

### Tại sao nó hoạt động 

Mỗi gói truyền hợp lệ cung cấp cho mỗi cấp chính xác một nguồn, gốc ảo hoặc cấp khác. Cấu trúc đó là một đồ thị không tuần hoàn được kết nối bao trùm tất cả các đỉnh, do đó một cây khung có gốc tại đỉnh`0`. 

Chi phí của sơ đồ truyền tải bằng tổng trọng số của cạnh đã chọn. 

Ngược lại, mọi cây bao trùm đều có gốc tại đỉnh`0`xác định một kế hoạch truyền tải hợp lệ. Một cạnh tới đỉnh`0`có nghĩa là gửi một cấp độ từ đầu, trong khi ranh giới giữa hai cấp độ có nghĩa là truyền tải sự khác biệt của chúng. 

Vì có sự tương ứng một-một giữa các kế hoạch truyền tải hợp lệ và các cây bao trùm của biểu đồ này nên việc giảm thiểu chi phí truyền tải chính xác là bài toán Cây bao trùm tối thiểu. Thuật toán Kruskal trả về một cây bao trùm có tổng trọng số tối thiểu, do đó phương án truyền tải được tạo ra là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def find(x, parent):
    while parent[x] != x:
        parent[x] = parent[parent[x]]
        x = parent[x]
    return x

def union(a, b, parent, size):
    a = find(a, parent)
    b = find(b, parent)

    if a == b:
        return False

    if size[a] < size[b]:
        a, b = b, a

    parent[b] = a
    size[a] += size[b]
    return True

def main():
    n, m, k, w = map(int, input().split())

    levels = []
    for _ in range(k):
        grid = [input().strip() for _ in range(n)]
        levels.append(grid)

    edges = []

    build_cost = n * m

    for i in range(k):
        edges.append((build_cost, 0, i + 1))

    for i in range(k):
        for j in range(i + 1, k):
            diff = 0

            a = levels[i]
            b = levels[j]

            for r in range(n):
                row_a = a[r]
                row_b = b[r]
                for c in range(m):
                    if row_a[c] != row_b[c]:
                        diff += 1

            edges.append((diff * w, i + 1, j + 1))

    edges.sort()

    parent = list(range(k + 1))
    size = [1] * (k + 1)

    mst_cost = 0
    mst_adj = [[] for _ in range(k + 1)]

    used = 0

    for cost, u, v in edges:
        if union(u, v, parent, size):
            mst_cost += cost
            mst_adj[u].append(v)
            mst_adj[v].append(u)
            used += 1

            if used == k:
                break

    result = []
    stack = [(0, -1)]

    while stack:
        node, par = stack.pop()

        for nxt in mst_adj[node]:
            if nxt == par:
                continue

            result.append((nxt, 0 if node == 0 else node))
            stack.append((nxt, node))

    print(mst_cost)
    for x, y in result:
        print(x, y)

if __name__ == "__main__":
    main()
```Phần đầu tiên đọc và lưu trữ mọi cấp độ. Vì mỗi lưới chứa tối đa 100 ô nên việc so sánh hai cấp độ sẽ rẻ. 

Việc xây dựng đồ thị phản ánh chính xác các quy tắc truyền. Các cạnh từ gốc ảo thể hiện việc xây dựng lại từ đầu. Các ranh giới giữa các cấp độ thể hiện sự khác biệt trong việc truyền tải. 

Thuật toán của Kruskal là phù hợp tự nhiên vì đồ thị dày đặc. Có khoảng`k² / 2`các cạnh và Kruskal chỉ yêu cầu sắp xếp chúng một lần rồi thực hiện các hoạt động DSU gần như liên tục. 

Sau khi xây dựng MST, đồ thị được chuyển thành danh sách kề. Một DFS bắt nguồn từ đỉnh`0`xác định mối quan hệ cha mẹ được yêu cầu bởi định dạng đầu ra. Bởi vì một cây có chính xác một đường đi từ gốc tới mọi đỉnh nên cây cha được chọn là rõ ràng. 

Một chi tiết tinh tế là các cạnh MST không bị định hướng. Hướng DFS xác định điểm cuối nào sẽ trở thành điểm chính trong kế hoạch truyền tải. Vì mọi đỉnh đều có thể truy cập được từ gốc nên thứ tự kết quả luôn thỏa mãn yêu cầu rằng đỉnh cha được truyền trước các đỉnh con của nó. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2 3 3 2
A.A
...
A.a
..C
X.Y
...
```Chi phí theo cặp: 

| Cạnh | Chi phí | 
| --- | --- | 
| 0-1 | 6 | 
| 0-2 | 6 | 
| 0-3 | 6 | 
| 1-2 | 4 | 
| 1-3 | 2 | 
| 2-3 | 6 | 

Vụ hành quyết Kruskal: 

| Bước | Cạnh được chọn | Tổng chi phí | 
| --- | --- | --- | 
| 1 | (1,3) giá 2 | 2 | 
| 2 | (1,2) giá 4 | 6 | 
| 3 | (0,1) giá 6 | 12 | 

Chi phí MST là 12. Root tại nút 0 tạo ra: 

| Cấp độ | Phụ huynh | 
| --- | --- | 
| 1 | 0 | 
| 3 | 1 | 
| 2 | 1 | 

Điều này thể hiện ý tưởng trung tâm của vấn đề. Chỉ một cấp độ được truyền trực tiếp, trong khi các cấp độ khác tái sử dụng nó thông qua các biên khác biệt rẻ hơn. 

### Ví dụ được xây dựng 

đầu vào:```
1 1 2 100
A
B
```Các cạnh đồ thị: 

| Cạnh | Chi phí | 
| --- | --- | 
| 0-1 | 1 | 
| 0-2 | 1 | 
| 1-2 | 100 | 

Vụ hành quyết Kruskal: 

| Bước | Cạnh được chọn | Tổng chi phí | 
| --- | --- | --- | 
| 1 | (0,1) | 1 | 
| 2 | (0,2) | 2 | 

Cạnh chuyển đổi đắt tiền không bao giờ được sử dụng. 

Ví dụ này cho thấy tại sao các cạnh của gốc ảo lại quan trọng. Đôi khi việc xây dựng lại rẻ hơn so với việc sử dụng lại một cấp độ khác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k² · n · m + k² log k) | Sự khác biệt theo cặp cộng với phân loại Kruskal | 
| Không gian | O(k²) | Lưu trữ tất cả các cạnh đồ thị | 

Với`n, m ≤ 10`, mỗi mức so sánh tốn tối đa 100 lần kiểm tra ký tự. Yếu tố chi phối là khoảng 500.000 cạnh khi`k = 1000`. Cả thời gian và mức sử dụng bộ nhớ đều phù hợp thoải mái trong giới hạn của cuộc thi. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
# The exact MST may not be unique, so these tests focus on
# properties rather than exact output formatting.

import sys
import io

def run(inp: str) -> str:
    # paste solution implementation here
    pass

# minimum size
inp = """1 1 1 5
A
"""
# expected total cost = 1

# all levels equal
inp = """1 1 3 1
A
A
A
"""
# expected total cost = 1

# rebuilding cheaper than transforming
inp = """1 1 2 100
A
B
"""
# expected total cost = 2

# transformation cheaper
inp = """1 2 2 1
AA
AB
"""
# expected total cost = 3

# boundary-style chain
inp = """1 3 3 1
AAA
AAB
ABB
"""
# expected total cost = 5
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cấp độ đơn | Chi phí =`n*m`| Trường hợp hợp lệ nhỏ nhất | 
| Mọi cấp độ đều bình đẳng | Các phép biến đổi một gốc cộng với chi phí bằng 0 | Các cạnh không khác biệt | 
| Biến đổi đắt tiền | Xây dựng lại độc lập | Sử dụng root ảo đúng cách | 
| Biến đổi giá rẻ | Tái sử dụng cấp độ khác | Lựa chọn cạnh chuyển đổi | 
| Chuỗi điểm tương đồng | Cấu trúc MST đa cấp | Tái thiết cha mẹ | 

## Vỏ cạnh 

Hãy xem xét:```
1 1 2 100
A
B
```Chi phí chuyển đổi là 100 trong khi chi phí xây dựng lại là 1. Biểu đồ chứa các cạnh`(0,1)=1`,`(0,2)=1`,`(1,2)=100`. Kruskal chọn cả hai cạnh gốc. Đầu ra gửi cả hai cấp độ một cách độc lập với tổng chi phí là 2. Bất kỳ giải pháp nào luôn cố gắng kết nối các cấp độ với nhau đều sẽ sai. 

Coi như:```
1 1 3 1
A
A
A
```Tất cả chi phí chuyển đổi theo cặp là 0. MST chứa một cạnh gốc của chi phí 1 và hai cạnh có chi phí bằng 0 giữa các cấp. Tổng chi phí trở thành 1. Điều này xác nhận rằng các mức giống hệt nhau sẽ không bao giờ được xây dựng lại một cách không cần thiết. 

Coi như:```
1 2 2 5
AA
AB
```Các cấp độ khác nhau trong một ô. Chi phí chuyển đổi là 5, trong khi chi phí xây dựng lại là 2. Biểu đồ chứa các cạnh`(0,1)=2`,`(0,2)=2`,`(1,2)=5`. MST chọn hai cạnh gốc, đưa ra chi phí 4. Một quy tắc tham lam chẳng hạn như "luôn biến đổi khi chỉ có một vài ô khác nhau" sẽ không thành công vì hệ số nhân`w`làm thay đổi hoàn toàn nền kinh tế. 

Cuối cùng, hãy xem xét một cây trong đó cấp 3 phụ thuộc vào cấp 2 và cấp 2 phụ thuộc vào cấp 1. Các cạnh MST không bị định hướng, nhưng DFS từ gốc ảo định hướng chúng một cách chính xác. Cha mẹ luôn xuất hiện trước con cái trong quá trình truyền tải, tạo ra một lệnh truyền hợp lệ.
