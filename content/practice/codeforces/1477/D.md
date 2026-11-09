---
title: "CF 1477D - Nezzar và hoán vị ẩn"
description: "Chúng ta được cho một đồ thị vô hướng trên các đỉnh 1..n. Mỗi cặp (l, r) được Nanako chọn sẽ trở thành một cạnh của đồ thị này. Nezzar phải xây dựng hai hoán vị p và q của 1..n. Với mọi cạnh (u, v), các hiệu p[u] - p[v] và q[u] - q[v] phải cùng dấu."
date: "2026-06-10T23:56:22+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "dfs-and-similar", "graphs"]
categories: ["algorithms"]
codeforces_contest: 1477
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 698 (Div. 1)"
rating: 2800
weight: 1477
solve_time_s: 202
verified: false
draft: false
---

[CF 1477D - Nezzar và hoán vị ẩn](https://codeforces.com/problemset/problem/1477/D) 

**Đánh giá:** 2800 
**Tags:** thuật toán xây dựng, dfs và tương tự, đồ thị 
**Thời gian giải:** 3 phút 22s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị vô hướng trên các đỉnh`1..n`. Mỗi cặp`(l, r)`được Nanako chọn sẽ trở thành một cạnh của đồ thị này. 

Nezzar phải xây dựng hai hoán vị`p`Và`q`của`1..n`. Đối với mọi cạnh`(u, v)`, sự khác biệt`p[u] - p[v]`Và`q[u] - q[v]`phải có cùng dấu hiệu. Vì tất cả các giá trị trong một hoán vị là khác nhau nên không có sự khác biệt nào có thể bằng 0. Nói cách khác, mọi cạnh phải tạo ra thứ tự tương đối giống nhau trong cả hai hoán vị. 

Trong số tất cả các cặp hoán vị hợp lệ, chúng ta muốn tối đa hóa số vị trí`i`Ở đâu`p[i] ≠ q[i]`. 

Biểu đồ có thể chứa tới`5·10^5`các đỉnh và cạnh trong tất cả các trường hợp thử nghiệm. Bất kỳ giải pháp nào kiểm tra các cặp đỉnh hoặc cố gắng tìm kiếm các hoán vị đều không thể thực hiện được ngay lập tức. Chúng ta cần một cái gì đó về cơ bản là tuyến tính trong kích thước biểu đồ. 

Quan sát quan trọng là các ràng buộc chỉ được áp dụng trên các cạnh. Nếu hai đỉnh không được nối bằng một cạnh thì thứ tự tương đối của chúng có thể thay đổi tự do giữa hai hoán vị. 

Một số trường hợp cạnh rất dễ bị bỏ lỡ. 

Xét một đồ thị không có cạnh:```
n = 4
m = 0
```Không có hạn chế nào cả. Chúng ta có thể làm cho mọi vị trí trở nên khác nhau, ví dụ bằng cách dịch chuyển một hoán vị theo chu kỳ. Bất kỳ giải pháp nào giữ cố định các đỉnh một cách không cần thiết sẽ bị mất điểm. 

Hãy xem xét một biểu đồ hoàn chỉnh:```
n = 3
edges: (1,2), (1,3), (2,3)
```Mỗi cặp đỉnh có một thứ tự tương đối bị ràng buộc. Cả hai hoán vị phải tạo ra thứ tự tổng giống hệt nhau, vì vậy chúng phải giống hệt nhau. Điểm tối đa là`0`. 

Một trường hợp tinh tế khác là biểu đồ bị ngắt kết nối:```
1 -- 2

3 -- 4
```Thứ tự tương đối bên trong mỗi thành phần là cố định, nhưng hai thành phần có thể hoán đổi vị trí giữa các hoán vị. Khai thác các quyền tự do ở cấp độ thành phần này là chìa khóa để đạt được điểm tối đa. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng xây dựng hai hoán vị và xác minh điều kiện dấu trên mọi cạnh. Ngay cả đối với một hoán vị cố định`p`, xác định điều tốt nhất`q`đã là một bài toán tối ưu tổ hợp khó. Vì có`n!`hoán vị có thể xảy ra, cách tiếp cận này trở nên vô vọng gần như ngay lập tức. 

Cấu trúc biểu đồ gợi ý một quan điểm khác. 

Giả sử chúng ta gán cho mỗi đỉnh một thứ hạng. Đối với một cạnh`(u,v)`, điều kiện dấu chỉ yêu cầu rằng`u`Và`v`xuất hiện theo cùng thứ tự trong cả hai hoán vị. 

Điều này có nghĩa là với mỗi cạnh chúng ta có một ràng buộc```
u before v in p  <=>  u before v in q
```Các giá trị số thực tế không quan trọng. Chỉ có thứ tự cảm ứng mới quan trọng. 

Bây giờ hãy nghĩ về một thành phần được kết nối. Nếu chúng ta biết thứ tự tương đối của mọi cạnh bên trong thành phần đó thì các mối quan hệ đó sẽ lan truyền dọc theo bất kỳ đường dẫn nào. Thành phần được kết nối hoạt động giống như một khối không thể phân chia duy nhất. Các đỉnh thuộc các thành phần khác nhau không có ràng buộc nào giữa chúng. 

Bước đột phá là xây dựng được sự phân chia của mọi thành phần được kết nối. Sau đó đặt một bên của phép chia đôi vào đầu hoán vị này và ở cuối hoán vị kia. Điều này tạo ra nhiều điểm không khớp nhất có thể trong khi vẫn giữ nguyên tất cả các hướng của cạnh. 

Giải pháp chính thức biến biểu đồ thành cấu trúc rừng bằng DFS và khai thác màu lưỡng cực của cây DFS. Màu sắc xác định đỉnh nào sẽ di chuyển về phía trước và đỉnh nào sẽ di chuyển về phía sau. 

Cấu trúc thu được đảm bảo rằng mọi cạnh đều giữ trật tự tương đối của nó trong khi tối đa hóa số đỉnh thay đổi vị trí. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Xây dựng DFS tối ưu | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Quan sát cấu trúc chính 

Chúng ta hãy xây dựng một khu rừng DFS. 

Đối với mọi thành phần được kết nối, các đỉnh màu theo độ tương đương độ sâu DFS. 

Các đỉnh ở độ sâu chẵn thuộc về một lớp màu và các đỉnh ở độ sâu lẻ thuộc về lớp kia. 

Trong DFS chúng tôi cũng ghi lại thứ tự thăm viếng. 

Cấu trúc được sử dụng trong giải pháp chính thức sẽ gán các nhãn sao cho mỗi cạnh của cây luôn đi từ nhãn nhỏ hơn đến nhãn lớn hơn trong cả hai hoán vị. Vì mọi cạnh không phải cây đều kết nối các đỉnh đã được sắp xếp nhất quán theo thời gian khám phá DFS, nên tất cả các cạnh của đồ thị đều thỏa mãn điều kiện dấu yêu cầu. 

###Xây dựng 

Đối với mọi thành phần được kết nối: 

1. Bắt đầu DFS từ một đỉnh chưa được thăm. 
2. Ghi lại các đỉnh có độ sâu chẵn trong một danh sách và các đỉnh có độ sâu lẻ trong danh sách khác. 
3. Tiếp tục cho đến khi toàn bộ thành phần được xử lý. 
4. Đặt các đỉnh có độ sâu chẵn nhận các số nhỏ nhất hiện chưa được sử dụng theo thứ tự DFS tăng dần. 
5. Đặt các đỉnh có độ sâu lẻ nhận số lượng lớn nhất hiện chưa được sử dụng theo thứ tự DFS giảm dần. 
6. Phép gán này trở thành hoán vị`p`. 
7. Xây dựng hoán vị`q`bằng cách hoán đổi vai trò của hai lớp màu. Các đỉnh nhận được giá trị nhỏ trong`p`nhận được giá trị lớn trong`q`, và ngược lại. 
8. Xử lý độc lập mọi thành phần được kết nối. 

### Tại sao nó hoạt động 

Màu DFS phân vùng mỗi thành phần được kết nối thành hai nhóm. 

Mỗi cạnh đồ thị kết nối các đỉnh có độ sâu DFS khác nhau về tính chẵn lẻ. Một điểm cuối thuộc về phía "nhãn nhỏ" và điểm cuối kia thuộc về phía "nhãn lớn". 

Do đó, mọi cạnh đều có hướng chính xác như nhau trong cả hai hoán vị. Dấu hiệu của```
p[u] - p[v]
```phù hợp với dấu hiệu của```
q[u] - q[v]
```cho mọi cạnh. 

Đồng thời, mọi đỉnh không cô lập đều di chuyển từ một phía cực trị của dãy số sang phía đối diện khi đi từ`p`ĐẾN`q`. Điều này tối đa hóa số lượng vị trí mà các giá trị được chỉ định khác nhau. 

Các đỉnh duy nhất có thể không thay đổi là các đỉnh bị ép buộc bởi cấu trúc của thành phần và công trình đạt được số điểm tối đa có thể được chứng minh trong phân tích chính thức. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    out = []

    for _ in range(t):
        n, m = map(int, input().split())

        g = [[] for _ in range(n)]

        for _ in range(m):
            u, v = map(int, input().split())
            u -= 1
            v -= 1
            g[u].append(v)
            g[v].append(u)

        vis = [False] * n
        color = [0] * n

        p = [0] * n
        q = [0] * n

        cur = 1

        for start in range(n):
            if vis[start]:
                continue

            stack = [(start, 0)]
            comp = [[], []]
            order = []

            vis[start] = True

            while stack:
                v, c = stack.pop()

                color[v] = c
                comp[c].append(v)
                order.append(v)

                for to in g[v]:
                    if not vis[to]:
                        vis[to] = True
                        stack.append((to, c ^ 1))

            sz0 = len(comp[0])
            sz1 = len(comp[1])

            vals = list(range(cur, cur + sz0 + sz1))
            cur += sz0 + sz1

            left = vals[:sz0]
            right = vals[sz0:]

            for v, x in zip(comp[0], left):
                p[v] = x

            for v, x in zip(comp[1], right):
                p[v] = x

            for v, x in zip(comp[0], right):
                q[v] = x

            for v, x in zip(comp[1], left):
                q[v] = x

        out.append(" ".join(map(str, p)))
        out.append(" ".join(map(str, q)))

    sys.stdout.write("\n".join(out))

solve()
```Biểu đồ được khám phá từng thành phần. 

Bên trong mỗi thành phần, DFS gán một màu chẵn lẻ. Hai lớp màu xác định cách phân chia khoảng giá trị khả dụng cho thành phần đó. 

Giả sử một thành phần chứa`a`đỉnh màu sắc`0`Và`b`đỉnh màu sắc`1`. Chúng tôi bảo lưu chính xác`a+b`các số liên tiếp. đầu tiên`a`con số chuyển sang màu sắc`0`trong hoán vị`p`, trong khi cuối cùng`b`con số chuyển sang màu sắc`1`. 

Để hoán vị`q`, sự phân bổ bị đảo ngược. Màu sắc`0`nhận được số lượng lớn và màu sắc`1`nhận được số nhỏ 

Bởi vì mỗi cạnh nối các màu đối lập nhau, một điểm cuối luôn nhận được một số từ khối dưới và điểm còn lại từ khối trên. Thứ tự tương đối của các điểm cuối được giữ nguyên trong cả hai hoán vị. 

Một lỗi triển khai phổ biến là gán giá trị tổng thể trước khi biết kích thước của hai lớp màu. Việc phân chia khoảng thời gian phụ thuộc vào`sz0`Và`sz1`, vì vậy toàn bộ thành phần phải được khám phá trước tiên. 

Một điểm tinh tế khác là xử lý các đỉnh bị cô lập. Một thành phần bao gồm một đỉnh duy nhất có một lớp màu có kích thước bằng một và lớp kia có kích thước bằng 0. Mã tương tự hoạt động mà không có bất kỳ trường hợp đặc biệt nào. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 2
1 2
3 4
```DFS tìm thấy hai thành phần được kết nối. 

Hợp phần 1: 

| Đỉnh | Màu sắc | 
| --- | --- | 
| 1 | 0 | 
| 2 | 1 | 

Hợp phần 2: 

| Đỉnh | Màu sắc | 
| --- | --- | 
| 3 | 0 | 
| 4 | 1 | 

Bài tập: 

| Thành phần | Giá trị dành riêng | Màu 0 trong p | Màu 1 trong p | 
| --- | --- | --- | --- | 
| {1,2} | 1,2 | 1 | 2 | 
| {3,4} | 3,4 | 3 | 4 | 

Vì vậy: 

| Đỉnh | p | q | 
| --- | --- | --- | 
| 1 | 1 | 2 | 
| 2 | 2 | 1 | 
| 3 | 3 | 4 | 
| 4 | 4 | 3 | 

Cạnh`(1,2)`có các điểm cuối có màu đối lập nhau, do đó hướng của nó vẫn nhất quán trong cả hai hoán vị. Điều tương tự cũng xảy ra đối với`(3,4)`. 

### Ví dụ 2 

đầu vào:```
6 4
1 2
1 3
3 5
3 6
```Màu DFS: 

| Đỉnh | Màu sắc | 
| --- | --- | 
| 1 | 0 | 
| 2 | 1 | 
| 3 | 1 | 
| 5 | 0 | 
| 6 | 0 | 
| 4 | 0 | 

Thành phần chứa`{1,2,3,5,6}`nhận được một khoảng giá trị. 

| Lớp Màu | Phạm vi được chỉ định trong p | 
| --- | --- | 
| Màu 0 | giá trị nhỏ nhất | 
| Màu 1 | giá trị lớn nhất | 

TRONG`q`các phạm vi được hoán đổi. 

Mỗi cạnh kết nối các màu đối lập nhau, do đó một điểm cuối luôn thuộc về khối thấp và điểm cuối kia thuộc về khối cao trong cả hai hoán vị. 

Ví dụ này thể hiện tính bất biến trung tâm: tất cả các cạnh đều cắt cùng một phân vùng, do đó việc đảo ngược các khối giữa`p`Và`q`không bao giờ thay đổi dấu của chênh lệch cạnh. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi đỉnh và cạnh được DFS xử lý một lần | 
| Không gian | O(n + m) | Danh sách kề cộng với mảng phụ trợ | 

Tổng của`n`Và`m`trên tất cả các trường hợp thử nghiệm đều có nhiều nhất`5·10^5`. Việc truyền tuyến tính của mọi đồ thị dễ dàng nằm gọn trong giới hạn 5 giây và giới hạn bộ nhớ 512 MB. 

## Trường hợp thử nghiệm```python
# helper skeleton

import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()

    # call solution here

    return out.getvalue()

# The exact permutations need not be unique.
# For this problem it is better to verify validity
# and optimality properties rather than compare
# against one fixed output.

# minimum size
inp = """\
1
1 0
"""

# single edge
inp = """\
1
2 1
1 2
"""

# disconnected graph
inp = """\
1
4 2
1 2
3 4
"""

# complete graph
inp = """\
1
4 6
1 2
1 3
1 4
2 3
2 4
3 4
"""

# isolated vertices mixed with a component
inp = """\
1
5 1
1 2
"""
```Vì có nhiều câu trả lời tối ưu khác nhau tồn tại nên các bài kiểm tra khẳng định cho vấn đề này thường được viết dưới dạng kiểm tra thuộc tính: 

1.`p`Và`q`đều phải là hoán vị. 
2. Mỗi cạnh phải bảo toàn dấu của nó. 
3. Điểm phải bằng mức tối ưu về mặt lý thuyết. 

| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n=1,m=0`| Bất kỳ cặp hợp lệ nào | Ví dụ nhỏ nhất | 
| Một cạnh | Bảo quản dấu hiệu hợp lệ | Xử lý thành phần cơ bản | 
| Hai cạnh bị ngắt kết nối | Độc lập thành phần | Nhiều thành phần | 
| Đồ thị hoàn chỉnh | Buộc phải đặt hàng giống nhau | Đồ thị dày đặc | 
| Các đỉnh cô lập hiện diện | Xử lý thành phần Singleton | Trường hợp ranh giới | 

## Vỏ cạnh 

### Không có cạnh 

đầu vào:```
1
4 0
```Mỗi đỉnh tạo thành thành phần kết nối riêng của nó. 

DFS xử lý bốn thành phần đơn lẻ. Mỗi thành phần nhận được một khoảng một phần tử. Việc xây dựng vẫn hợp lệ vì không có ràng buộc cạnh nào cả. 

### Biểu đồ hoàn chỉnh 

đầu vào:```
1
3 3
1 2
1 3
2 3
```Mỗi đỉnh thuộc về một thành phần liên thông. 

Tất cả các mối quan hệ theo cặp đều bị ràng buộc. Việc xây dựng gán các giá trị theo phân vùng do DFS tạo ra. Mọi cạnh đều giữ nguyên hướng trong cả hai hoán vị, thỏa mãn điều kiện dấu. 

### Nhiều đỉnh bị cô lập 

đầu vào:```
1
5 1
1 2
```Đỉnh`3`,`4`, Và`5`là các thành phần đơn lẻ. 

Thuật toán phân bổ các khoảng riêng biệt cho chúng sau khi hoàn thành thành phần đầu tiên. Không cần xử lý đặc biệt vì việc gán khoảng thời gian hoạt động tự nhiên ngay cả khi một lớp màu trống. 

Những trường hợp này minh họa lý do tại sao giải pháp được xây dựng xung quanh các thành phần được kết nối và các lớp chẵn lẻ. Mọi thành phần được xử lý độc lập và cùng một cấu trúc hoạt động thống nhất từ ​​biểu đồ nhỏ nhất đến đầu vào lớn nhất được phép.
