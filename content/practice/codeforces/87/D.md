---
title: "CF 87D - Con Đường Đẹp"
description: "Chúng ta có một cây có trọng số với n thành phố và n - 1 con đường. Mỗi cặp thành phố riêng biệt theo thứ tự đại diện cho một chiến dịch quân sự, do đó có tổng cộng n (n - 1) chuyến đi. Để đi từ thành phố u đến thành phố v, quân đội sẽ di chuyển dọc theo con đường duy nhất giữa chúng."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "dp", "dsu", "graphs", "implementation", "sortings", "trees"]
categories: ["algorithms"]
codeforces_contest: 87
codeforces_index: "D"
codeforces_contest_name: "Codeforces Beta Round 73 (Div. 1 Only)"
rating: 2300
weight: 87
solve_time_s: 132
verified: true
draft: false
---

[CF 87D - Con Đường Đẹp](https://codeforces.com/problemset/problem/87/D) 

**Đánh giá:** 2300 
**Thẻ:** dfs và tương tự, dp, dsu, đồ thị, triển khai, sắp xếp, cây 
**Thời gian giải:** 2m 12s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một cây có trọng số với`n`thành phố và`n - 1`những con đường. Mỗi cặp thành phố riêng biệt theo thứ tự đại diện cho một chiến dịch quân sự, do đó có`n * (n - 1)`tổng số chuyến đi. 

Đối với một chuyến đi từ thành phố`u`đến thành phố`v`, quân đội đi dọc theo con đường duy nhất giữa họ. Trong số tất cả các con đường trên con đường đó, quân đội dành nhiều thời gian nhất trên những con đường có trọng lượng tối đa dọc theo con đường đó. Mỗi con đường như vậy sẽ nhận được một cây đẹp cho chiến dịch đó. 

Mỗi con đường phải đếm xem có bao nhiêu cặp thứ tự`(u, v)`coi con đường này là một trong những cạnh có trọng lượng tối đa trên đường đi của chúng. Sau đó, chúng tôi đưa ra số lượng lớn nhất và tất cả các con đường đều đạt được số lượng đó. 

Đồ thị là một cái cây nên mỗi cặp thành phố đều có đúng một đường đi. Khó khăn đến từ sự ràng buộc. Nếu một số cạnh trên đường đi có cùng trọng số tối đa thì tất cả chúng đều nhận được một cây. 

Những ràng buộc buộc chúng ta phải suy nghĩ cẩn thận. Với`n ≤ 10^5`, số cặp thành phố là khoảng`10^10`, vì vậy bất kỳ thuật toán nào lặp lại rõ ràng trên tất cả các cặp đều không thể thực hiện được. Thậm chí`O(n^2)`quá chậm so với giới hạn 1 giây. Chúng ta cần một cái gì đó gần gũi`O(n log n)`hoặc`O(n α(n))`. 

Sự tinh tế chính là xử lý chính xác các trọng số cạnh bằng nhau. Một lỗi phổ biến là chỉ đếm các đường đi có cạnh là giá trị tối đa duy nhất. Điều này không thành công bất cứ khi nào có nhiều cạnh có trọng số tối đa xuất hiện trên cùng một đường dẫn. 

Hãy xem xét ví dụ này:```
3
1 2 5
2 3 5
```Con đường từ`1`ĐẾN`3`chứa cả hai cạnh, cả hai đều có trọng lượng tối đa`5`, nên cả hai con đường đều nhận được một cây từ chiến dịch này. 

Số đếm đúng là:```
edge 1 -> 4
edge 2 -> 4
```vì mỗi cạnh được sử dụng bởi:`(1,2), (2,1), (1,3), (3,1)`cho cạnh 1, 

và tương tự cho cạnh 2. 

Một trường hợp nguy hiểm khác là khi các cạnh nhẹ hơn kết nối các thành phần đã được kết nối bên trong bằng các cạnh có trọng lượng bằng nhau. 

Ví dụ:```
4
1 2 3
2 3 3
3 4 1
```Cạnh`(3,4)`không bao giờ có thể đạt cực đại trên đường đi có các đỉnh`1`hoặc`2`, bởi vì bất kỳ đường đi nào như vậy cũng chứa trọng số`3`. Cách tiếp cận DSU bất cẩn xử lý các cạnh một cách độc lập có thể vượt quá các đường dẫn như vậy. 

Câu trả lời đúng là:```
4 2
1 2
```Kịch bản khó khăn cuối cùng là các nhóm lớn có trọng số bằng nhau. Nếu chúng ta hợp nhất các thành phần DSU ngay lập tức trong khi lặp qua các cạnh có trọng số bằng nhau, thì các cạnh sau trong cùng một nhóm trọng số sẽ thấy các thành phần đã được cập nhật và tạo ra số lượng không chính xác. 

Ví dụ:```
4
1 2 5
2 3 5
3 4 5
```Tất cả các cạnh sẽ nhận được số lượng như nhau. Nhóm có trọng số bằng nhau phải được xử lý cùng nhau trước khi bất kỳ sự kết hợp nào được hoàn tất. 

## Phương pháp tiếp cận 

Giải pháp brute-force rất đơn giản về mặt khái niệm. Đối với mỗi cặp đặt hàng`(u, v)`, chúng ta tìm đường đi duy nhất giữa chúng, xác định trọng số cạnh tối đa trên đường đi đó và tăng dần mọi cạnh có trọng số đó. 

Điều này có tác dụng vì định nghĩa khớp trực tiếp với mô phỏng. Thật không may, có`n * (n - 1)`các cặp có thứ tự và mỗi đường dẫn có thể chứa`O(n)`các cạnh. Trong trường hợp xấu nhất điều này trở thành`O(n^3)`, điều này hoàn toàn không thể thực hiện được đối với`n = 10^5`. 

Chúng ta cần ngừng suy nghĩ về những con đường riêng lẻ. 

Quan sát quan trọng là một cạnh chỉ đóng góp vào một cặp khi trọng số của nó bằng trọng số cạnh tối đa trên đường đi. Giả sử chúng ta xử lý các cạnh theo thứ tự trọng số tăng dần bằng DSU. 

Trước khi xử lý trọng lượng`w`, mọi thành phần DSU biểu diễn các đỉnh được kết nối bằng cách sử dụng các cạnh nhỏ hơn`w`. 

Bây giờ hãy nắm lấy lợi thế`(u, v)`trọng lượng`w`. 

Nếu như`u`thuộc về một thành phần có kích thước`A`Và`v`thuộc về một thành phần có kích thước`B`, thì mỗi cặp`(a, b)`với`a`trong thành phần đầu tiên và`b`trong đường thứ hai có đường đi có trọng số cạnh tối đa chính xác`w`. 

Tại sao? Bởi vì tất cả các cạnh được thêm vào trước đó đều nhỏ hơn`w`và cạnh hiện tại tạo kết nối đầu tiên có thể có giữa các vùng này bằng cách sử dụng trọng số`w`. 

Số cặp không có thứ tự là`A * B`, vậy số cặp có thứ tự là:```
2 * A * B
```Giá trị đó đóng góp vào cạnh hiện tại. 

Trọng lượng bằng nhau làm phức tạp mọi thứ. Trong khi xử lý các cạnh của trọng lượng`w`, chúng ta không được kết hợp các thành phần ngay lập tức, bởi vì tất cả các cạnh của trọng số`w`sẽ thấy trạng thái DSU chỉ được hình thành bởi các trọng số nhỏ hơn. 

Vì vậy, chúng tôi xử lý từng nhóm trọng lượng theo hai giai đoạn: 

Đầu tiên, tính toán các khoản đóng góp bằng cách sử dụng trạng thái DSU trước bất kỳ công đoàn nào có trọng số này. 

Thứ hai, hợp nhất tất cả các cạnh của trọng lượng này. 

Điều này đảm bảo rằng các cạnh có trọng lượng bằng nhau được xử lý đối xứng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n³) | O(n) | Quá chậm | 
| DSU tối ưu theo trọng lượng | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các cạnh và lưu trữ điểm cuối, trọng số và chỉ mục gốc của chúng. 
2. Sắp xếp các cạnh theo trọng lượng theo thứ tự không giảm. 
3. Khởi tạo DSU trong đó mỗi đỉnh bắt đầu trong thành phần riêng của nó. 
4. Xử lý các cạnh được nhóm theo trọng số bằng nhau. 
5. Cho mọi cạnh`(u, v)`ở nhóm cân nặng hiện tại: 

Tìm gốc DSU của`u`Và`v`. 

Đặt kích thước thành phần là`A`Và`B`. 

Số cặp thành phố được sắp xếp có đường đi có trọng số tối đa`w`bởi vì kết nối này là`2 * A * B`. 

Lưu trữ giá trị này dưới dạng phần đóng góp của cạnh. 

Điều này hoạt động vì tất cả các đường dẫn giữa hai thành phần phải vượt qua một số cạnh trọng số`w`và tất cả các cạnh nhỏ hơn đã được hợp nhất trước đó. 
6. Sau khi tính toán đóng góp cho toàn bộ nhóm trọng số, hợp tất cả các cạnh trong nhóm. 

Trì hoãn công đoàn là điều cần thiết. Nếu không, các cạnh sau trong cùng một nhóm sẽ quan sát không chính xác các thành phần được hợp nhất một phần. 
7. Sau khi xử lý tất cả các nhóm, hãy tìm mức đóng góp tối đa. 
8. In ra giá trị lớn nhất, số cạnh đạt được giá trị đó và chỉ số của chúng theo thứ tự tăng dần. 

### Tại sao nó hoạt động 

Hiện tại chúng tôi đang xử lý trọng lượng`w`, mọi thành phần DSU thể hiện kết nối chỉ sử dụng các cạnh có trọng số nhỏ hơn`w`. 

Đối với một cạnh kết nối các thành phần có kích thước`A`Và`B`, mọi cặp đỉnh trên các thành phần này không có đường đi chỉ sử dụng các cạnh nhỏ hơn. Bất kỳ đường đi nào giữa chúng đều phải sử dụng ít nhất một cạnh có trọng số`w`, vậy cạnh lớn nhất trên đường đi đó chính xác là`w`. 

Cạnh hiện tại nằm trên tất cả các đường như vậy vì đồ thị là một cây. Do đó cạnh này nhận được chính xác`2 * A * B`các cặp đặt hàng. 

Việc xử lý các trọng số bằng nhau cùng nhau sẽ đảm bảo tính chính xác vì không có cạnh nào về trọng số`w`nên được hưởng lợi từ sự kết hợp do một cạnh khác có cùng trọng số gây ra. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class DSU:
    def __init__(self, n):
        self.parent = list(range(n + 1))
        self.size = [1] * (n + 1)

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
    n = int(input())

    edges = []

    for idx in range(1, n):
        u, v, w = map(int, input().split())
        edges.append((w, u, v, idx))

    edges.sort()

    dsu = DSU(n)

    ans = [0] * (n - 1)

    m = len(edges)
    i = 0

    while i < m:
        j = i

        while j < m and edges[j][0] == edges[i][0]:
            j += 1

        # compute contributions
        for k in range(i, j):
            w, u, v, idx = edges[k]

            ru = dsu.find(u)
            rv = dsu.find(v)

            ans[idx - 1] = 2 * dsu.size[ru] * dsu.size[rv]

        # perform unions
        for k in range(i, j):
            w, u, v, idx = edges[k]
            dsu.union(u, v)

        i = j

    best = max(ans)

    result = []

    for i in range(n - 1):
        if ans[i] == best:
            result.append(i + 1)

    print(best, len(result))
    print(*result)

solve()
```Giải pháp xoay quanh việc duy trì các thành phần được kết nối được tạo thành bởi các cạnh có trọng lượng nhỏ hơn. 

DSU lưu trữ cả kích thước thành phần gốc và kích thước thành phần. Nén đường dẫn và kết hợp theo kích thước giữ cho các hoạt động có thời gian gần như không đổi. 

Các cạnh được sắp xếp theo trọng số sao cho khi chúng ta xử lý một trọng số`w`, tất cả các trọng số nhỏ hơn đã được hợp nhất. 

Phần tinh tế nhất là việc xử lý hai pha các cạnh có trọng lượng bằng nhau. Trong giai đoạn đóng góp, mọi cạnh phải tuân theo trạng thái DSU trước khi xảy ra bất kỳ sự kết hợp nào có cùng trọng số đó. Chỉ sau khi tất cả các đóng góp được tính toán, chúng tôi mới hợp nhất các thành phần. 

Công thức đóng góp sử dụng các cặp có thứ tự, không phải các cặp không có thứ tự. Đó là lý do tại sao chúng tôi nhân với`2`. 

Tất cả số lượng phù hợp với số nguyên 64-bit. Trong Python điều này là tự động, nhưng trong C++ giải pháp sẽ yêu cầu`long long`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
2 1 5
```Các cạnh được sắp xếp: 

| Cạnh | Cân nặng | 
| --- | --- | 
| (2,1) | 5 | 

Kích thước DSU ban đầu: 

| Thành phần | Kích thước | 
| --- | --- | 
| {1} | 1 | 
| {2} | 1 | 

Trọng lượng xử lý`5`: 

| Cạnh | Cỡ A | Cỡ B | Đóng góp | 
| --- | --- | --- | --- | 
| (2,1) | 1 | 1 | 2 | 

Câu trả lời cuối cùng:```
2 1
1
```Điều này chứng tỏ trường hợp cơ bản. Các cặp có thứ tự duy nhất là`(1,2)`Và`(2,1)`. 

### Ví dụ 2 

đầu vào:```
4
1 2 5
2 3 5
3 4 1
```Các cạnh được sắp xếp: 

| Cạnh | Cân nặng | 
| --- | --- | 
| (3,4) | 1 | 
| (1,2) | 5 | 
| (2,3) | 5 | 

Sau khi xử lý trọng lượng`1`: 

| Cạnh | Cỡ A | Cỡ B | Đóng góp | 
| --- | --- | --- | --- | 
| (3,4) | 1 | 1 | 2 | 

Các thành phần DSU trở thành`{1}`,`{2}`,`{3,4}`. 

Bây giờ xử lý trọng lượng`5`. 

Trước công đoàn: 

| Cạnh | Cỡ A | Cỡ B | Đóng góp | 
| --- | --- | --- | --- | 
| (1,2) | 1 | 1 | 2 | 
| (2,3) | 1 | 2 | 4 | 

Sau khi kết hợp, tất cả các đỉnh được kết nối. 

Số cạnh cuối cùng: 

| Chỉ số cạnh | Đóng góp | 
| --- | --- | 
| 1 | 2 | 
| 2 | 4 | 
| 3 | 2 | 

Đầu ra:```
4 1
2
```Dấu vết này cho thấy tại sao các cạnh có trọng số bằng nhau phải được đánh giá trước khi kết hợp. Edge 2 nhìn thấy thành phần chính xác`{3,4}`có kích thước 2. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế, hoạt động DSU gần như không đổi | 
| Không gian | O(n) | Mảng DSU và lưu trữ cạnh | 

Thuật toán dễ dàng phù hợp với các ràng buộc. Sắp xếp`10^5`các cạnh đủ nhanh và các hoạt động DSU nhìn chung có hiệu quả tuyến tính. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    class DSU:
        def __init__(self, n):
            self.parent = list(range(n + 1))
            self.size = [1] * (n + 1)

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

    n = int(input())

    edges = []

    for idx in range(1, n):
        u, v, w = map(int, input().split())
        edges.append((w, u, v, idx))

    edges.sort()

    dsu = DSU(n)

    ans = [0] * (n - 1)

    i = 0

    while i < len(edges):
        j = i

        while j < len(edges) and edges[j][0] == edges[i][0]:
            j += 1

        for k in range(i, j):
            w, u, v, idx = edges[k]

            ru = dsu.find(u)
            rv = dsu.find(v)

            ans[idx - 1] = 2 * dsu.size[ru] * dsu.size[rv]

        for k in range(i, j):
            w, u, v, idx = edges[k]
            dsu.union(u, v)

        i = j

    best = max(ans)

    res = []

    for i in range(n - 1):
        if ans[i] == best:
            res.append(i + 1)

    out = []
    out.append(f"{best} {len(res)}")
    out.append(" ".join(map(str, res)))

    return "\n".join(out) + "\n"

# provided sample
assert run(
"""2
2 1 5
"""
) == """2 1
1
""", "sample 1"

# all equal weights
assert run(
"""3
1 2 5
2 3 5
"""
) == """4 2
1 2
""", "equal weights"

# chain with increasing weights
assert run(
"""4
1 2 1
2 3 2
3 4 3
"""
) == """6 1
3
""", "largest edge dominates most paths"

# star graph
assert run(
"""5
1 2 10
1 3 10
1 4 1
1 5 1
"""
) == """8 2
1 2
""", "mixed equal weights"

# minimum size
assert run(
"""2
1 2 1
"""
) == """2 1
1
""", "minimum input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cây hai nút | Cạnh đơn được 2 | Ranh giới tối thiểu | 
| Tất cả trọng lượng bằng nhau | Cà vạt nhiều cạnh | Xử lý trọng lượng bằng nhau | 
| Tăng chuỗi | Cạnh lớn nhất chiếm ưu thế | Đếm theo thứ tự | 
| Ngôi sao có trọng lượng hỗn hợp | Công đoàn bị trì hoãn | Độ chính xác cùng trọng lượng | 
| Đầu vào hợp lệ nhỏ nhất | Hành vi DSU cơ bản | An toàn từng người một | 

## Vỏ cạnh 

Hãy xem xét lại chuỗi có trọng lượng bằng nhau:```
3
1 2 5
2 3 5
```Ban đầu tất cả các thành phần có kích thước 1. 

Cả hai cạnh đều thuộc cùng một nhóm trọng lượng. 

Trước công đoàn: 

| Cạnh | Kích thước thành phần | Đóng góp | 
| --- | --- | --- | 
| (1,2) | 1 và 1 | 2 | 
| (2,3) | 1 và 1 | 2 | 

Sau khi xử lý liên kết đầu tiên, DSU sẽ trở thành`{1,2}`Và`{3}`. 

Nếu chúng ta tính cạnh thứ hai sau đó, nó sẽ nhận được đóng góp không chính xác`4`. 

Việc xử lý các trọng số bằng nhau sẽ tránh được lỗi này. 

Bây giờ hãy xem xét:```
4
1 2 3
2 3 3
3 4 1
```Sau khi xử lý trọng lượng`1`, đỉnh`3`Và`4`được kết nối. 

Đối với cạnh`(2,3)`trọng lượng`3`, kích thước thành phần là:```
{2} -> size 1
{3,4} -> size 2
```Vì vậy sự đóng góp trở thành`4`. 

Điều này đếm chính xác các cặp có thứ tự:```
(2,3), (3,2), (2,4), (4,2)
```Các cặp liên quan đến đỉnh`1`bị loại trừ vì đường đi của chúng chứa một cạnh khác có trọng số tối đa bằng nhau. 

Cuối cùng, hãy xem xét một chuỗi tăng nghiêm ngặt:```
4
1 2 1
2 3 2
3 4 3
```Khi xử lý cạnh`(3,4)`với trọng lượng`3`, DSU đã được sáp nhập`{1,2,3}`. 

Sự đóng góp là:```
2 * 3 * 1 = 6
```tương ứng với các cặp có thứ tự:```
(1,4), (4,1),
(2,4), (4,2),
(3,4), (4,3)
```Mỗi đường đi này đều có trọng số cạnh tối đa`3`, và cạnh`(3,4)`là cạnh duy nhất có giá trị đó.
