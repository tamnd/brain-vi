---
title: "CF 486D - Bộ hợp lệ"
description: "Chúng ta được cho một cây trong đó mọi đỉnh đều có một giá trị nguyên. Một tập hợp các đỉnh được coi là hợp lệ nếu nó không trống, tạo thành một sơ đồ con liên thông của cây và chênh lệch giữa giá trị lớn nhất và nhỏ nhất bên trong tập hợp không vượt quá d."
date: "2026-06-07T17:28:51+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "dp", "math", "trees"]
categories: ["algorithms"]
codeforces_contest: 486
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 277 (Div. 2)"
rating: 2100
weight: 486
solve_time_s: 118
verified: true
draft: false
---

[CF 486D - Bộ hợp lệ](https://codeforces.com/problemset/problem/486/D) 

**Đánh giá:** 2100 
**Tags:** dfs và tương tự, dp, toán, cây 
**Thời gian giải:** 1 phút 58 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một cây trong đó mọi đỉnh đều có một giá trị nguyên. Một tập hợp các đỉnh được coi là hợp lệ nếu nó không trống, tạo thành một sơ đồ con liên thông của cây và chênh lệch giữa giá trị lớn nhất và nhỏ nhất trong tập hợp đó không vượt quá`d`. 

Nhiệm vụ là đếm xem tồn tại bao nhiêu tập hợp đỉnh được kết nối như vậy. 

Cây chứa tối đa 2000 đỉnh và mọi giá trị nhiều nhất là 2000. Giới hạn 2000 đỉnh đủ nhỏ để một`O(n²)`hoặc`O(n² log n)`lời giải là thực tế, nhưng bất kỳ số mũ nào về số đỉnh đều không thể xảy ra. Vì một cây có 2000 đỉnh có 1999 cạnh nên việc chạy DFS từ mọi đỉnh là hoàn toàn khả thi. 

Phần khó khăn là tránh tính hai lần. Một tập hợp liên thông có thể chứa nhiều đỉnh có cùng giá trị nhỏ nhất. Nếu chúng ta chỉ liệt kê các tập hợp con liên thông thỏa mãn giới hạn giá trị thì có thể phát hiện cùng một tập hợp nhiều lần. 

Hãy xem xét một ví dụ nhỏ:```
d = 0

1 -- 2
values: [5, 5]
```Các bộ hợp lệ là`{1}`,`{2}`, Và`{1,2}`. 

Nếu chúng ta cố gắng đếm các tập con liên thông có gốc ở mọi đỉnh có giá trị nhỏ nhất thì`{1,2}`sẽ được tính một lần từ đỉnh 1 và một lần nữa từ đỉnh 2. 

Một trường hợp tinh vi khác xuất hiện khi tồn tại một số cực tiểu bằng nhau bên trong một thành phần được kết nối lớn hơn:```
1 -- 2 -- 3
values: [1, 1, 2]
d = 1
```bộ`{1,2,3}`là hợp lệ. Bất kỳ sơ đồ đếm nào không thiết lập đỉnh tối thiểu đại diện duy nhất sẽ tính tập hợp này nhiều lần. 

Trường hợp cạnh thứ ba là`d = 0`. Khi đó mọi tập hợp lệ phải bao gồm toàn bộ các đỉnh có cùng giá trị. Thuật toán phải giảm một cách tự nhiên sang việc đếm các tập hợp con đơn sắc được kết nối. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ liệt kê mọi tập hợp con của các đỉnh, kiểm tra xem nó có được kết nối hay không và xác minh rằng giá trị tối đa trừ đi giá trị tối thiểu là nhiều nhất`d`. 

có`2^n`tập hợp con. Ngay cả đối với`n = 40`, con số này đã quá lớn, trong khi giới hạn thực tế là 2000. Cách tiếp cận vũ phu là hoàn toàn không khả thi. 

Cấu trúc cây gợi ý một góc nhìn khác. Thay vì liệt kê trực tiếp các tập con, giả sử chúng ta chọn một đỉnh`r`sẽ đóng vai trò là đỉnh có giá trị nhỏ nhất của tập hợp. 

Bất kỳ tập hợp lệ nào được tính thông qua`r`phải thỏa mãn hai điều kiện. 

Đầu tiên, mọi đỉnh trong tập hợp phải có giá trị giữa`a[r]`Và`a[r] + d`. 

Thứ hai, trong số các đỉnh có giá trị chính xác`a[r]`, chúng ta cần một quy tắc ràng buộc để mỗi tập hợp lệ được tính chính xác một lần. 

Một kỹ thuật rất phổ biến trên cây là đếm các sơ đồ con được kết nối chứa một gốc phân biệt. Khi gốc đã được cố định, DP của cây có thể đếm tất cả các tập hợp con được kết nối có chứa gốc đó. 

Đối với một gốc được chọn`r`, chúng ta chỉ cho phép các đỉnh có giá trị nằm trong khoảng```
[a[r], a[r] + d]
```và ngoài ra còn cấm bất kỳ đỉnh nào có```
a[v] = a[r] and v < r.
```Quy tắc ràng buộc này làm cho`r`đỉnh có chỉ số nhỏ nhất trong số tất cả các đỉnh có giá trị nhỏ nhất. 

Bây giờ mọi tập hợp lệ đều có chính xác một nghiệm đại diện: 

Nếu một bộ hợp lệ có giá trị tối thiểu`m`, xét tất cả các đỉnh bên trong tập hợp có giá trị bằng`m`. Cho phép`r`là chỉ số nhỏ nhất trong số đó. Bộ này sẽ được tính khi xử lý`r`và nó sẽ bị từ chối đối với mọi đỉnh có giá trị tối thiểu khác do hạn chế ràng buộc. 

Sau khi cố định các đỉnh cho phép, chúng ta cần đếm các tập con liên thông chứa`r`bên trong cây cảm ứng các đỉnh cho phép. 

Đây là cây DP tiêu chuẩn. Với mọi đỉnh`u`, định nghĩa`dp[u]`là số tập hợp con được kết nối bên trong cây con DFS của`u`chứa`u`. 

Đối với mỗi đứa trẻ`v`, chúng ta có thể: 

1. Đừng lấy gì từ phía đứa trẻ đó. 
2. Đính kèm bất kỳ tập hợp con kết nối nào được tính bằng`dp[v]`. 

Do đó sự đóng góp của trẻ em là`(dp[v] + 1)`, Và```
dp[u] = ∏ (dp[v] + 1).
```Câu trả lời được đóng góp bởi root`r`là`dp[r]`. 

Lặp lại DFS này cho mọi đỉnh sẽ cho câu trả lời cuối cùng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n · n) | O(n) | Quá chậm | 
| Tối ưu | O(n²) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc cây và các giá trị. 
2. Lặp lại mọi đỉnh`r`và coi nó là đỉnh tối thiểu phân biệt của một tập hợp lệ. 
3. Trong khi xử lý`r`, một đỉnh`v`được phép nếu: 

-`a[v] >= a[r]`-`a[v] <= a[r] + d`- nếu như`a[v] == a[r]`, sau đó`v >= r`Điều kiện cuối cùng là luật hòa. 
4. Chạy DFS bắt đầu từ`r`. 
5. Đối với mỗi đỉnh được thăm`u`, tính số lượng tập hợp con được kết nối có chứa`u`và nằm hoàn toàn bên trong cây con DFS. 
6. Đối với mọi trẻ em được phép`v`, tính toán đệ quy`dp[v]`. 
7. Khi kết hợp một tập con, bỏ qua nhánh đó hoặc đính kèm một trong các tập con được kết nối được tính bằng`dp[v]`. Nhân kết quả hiện tại với`(dp[v] + 1)`. 
8. Bảo quản sản phẩm thu được dưới dạng`dp[u]`. 
9. Sau khi DFS kết thúc, hãy thêm`dp[r]`cho câu trả lời toàn cầu. 
10. Lặp lại cho tất cả các nghiệm và in ra tổng modulo`10^9 + 7`. 

### Tại sao nó hoạt động 

Sửa mọi bộ kết nối hợp lệ`S`. 

Cho phép`m`là giá trị tối thiểu của nó. Giữa các đỉnh của`S`có giá trị`m`, chọn chỉ số nhỏ nhất`r`. 

Khi xử lý root`r`, mọi đỉnh của`S`thỏa mãn```
m ≤ a[v] ≤ m + d,
```và không có đỉnh giá trị`m`có chỉ số nhỏ hơn`r`. Như vậy mọi đỉnh của`S`được cho phép. 

Từ`S`được kết nối và chứa`r`, DFS DP có chính xác một cách để xây dựng nó. 

Bây giờ hãy xem xét bất kỳ gốc nào khác`x`. 

Nếu như`a[x] > m`, sau đó`x`không thể là giá trị nhỏ nhất của`S`. 

Nếu như`a[x] = m`Và`x > r`, sau đó`r`bị cấm bởi quy tắc ràng buộc trong khi xử lý`x`, Vì thế`S`không thể hình thành được. 

Do đó, mọi bộ hợp lệ đều được tính chính xác một lần và mọi bộ được tính đều hợp lệ. Điều này thiết lập tính đúng đắn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 1000000007

d, n = map(int, input().split())
a = list(map(int, input().split()))

g = [[] for _ in range(n)]
for _ in range(n - 1):
    u, v = map(int, input().split())
    u -= 1
    v -= 1
    g[u].append(v)
    g[v].append(u)

sys.setrecursionlimit(10000)

answer = 0

for root in range(n):
    base = a[root]

    def dfs(u, parent):
        res = 1

        for v in g[u]:
            if v == parent:
                continue

            if a[v] < base or a[v] > base + d:
                continue

            if a[v] == base and v < root:
                continue

            res = (res * (dfs(v, u) + 1)) % MOD

        return res

    answer = (answer + dfs(root, -1)) % MOD

print(answer)
```Vòng lặp bên ngoài chọn đỉnh tối thiểu đại diện. Biến`base`lưu trữ giá trị của nó. 

DFS chỉ đi qua các đỉnh thỏa mãn ràng buộc khoảng giá trị. Các đỉnh có cùng giá trị với gốc nhưng có chỉ số nhỏ hơn cũng bị loại trừ. Đây là điều kiện ràng buộc quan trọng giúp ngăn ngừa việc đếm trùng lặp. 

Giá trị trả về của`dfs(u, parent)`là số tập con liên thông có chứa`u`và chỉ sử dụng các đỉnh từ cây con DFS bên dưới`u`. 

Với mỗi đứa trẻ, có đúng hai lựa chọn. Hoặc là chúng ta bỏ qua toàn bộ chi nhánh đó, góp phần`1`hoặc chúng ta kết nối một trong các tập hợp con được kết nối hợp lệ có chứa phần tử con, góp phần`dp[child]`. Nhân những lựa chọn này cho trẻ em sẽ có tổng số. 

Vì mọi phép nhân đều được thực hiện modulo`10^9 + 7`, tránh được tràn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
1 4
2 1 3 2
1 2
1 3
3 4
```Đang xử lý gốc 2 (giá trị 1): 

| Đỉnh | Được phép | dp | 
| --- | --- | --- | 
| 4 | Có | 1 | 
| 3 | Không (giá trị 3 > 2) | - | 
| 1 | Có | 1 | 
| 2 | Có | 2 | 

Sự đóng góp là`2`, tương ứng với:```
{2}
{1,2}
```Xử lý tất cả các gốc tạo ra câu trả lời tổng thể`8`. 

Ví dụ này minh họa cách cửa sổ giá trị`[a[root], a[root]+d]`hạn chế các đỉnh có thể tham gia. 

### Ví dụ 2 

đầu vào:```
0 3
5 5 5
1 2
2 3
```Xử lý gốc 1: 

| Đỉnh | Được phép | dp | 
| --- | --- | --- | 
| 3 | Có | 1 | 
| 2 | Có | 2 | 
| 1 | Có | 3 | 

Sự đóng góp:`3`. 

Xử lý gốc 2: 

Đỉnh 1 bị cấm vì nó có cùng giá trị và chỉ số nhỏ hơn. 

| Đỉnh | Được phép | dp | 
| --- | --- | --- | 
| 3 | Có | 1 | 
| 2 | Có | 2 | 

Sự đóng góp:`2`. 

Xử lý gốc 3: 

| Đỉnh | Được phép | dp | 
| --- | --- | --- | 
| 3 | Có | 1 | 

Sự đóng góp:`1`. 

Tổng cộng:```
3 + 2 + 1 = 6
```Sáu tập hợp con được kết nối hợp lệ là:```
{1}
{2}
{3}
{1,2}
{2,3}
{1,2,3}
```Dấu vết này cho thấy quy tắc ràng buộc phân phối các tập hợp con tối thiểu bằng nhau giữa các nghiệm khác nhau mà không bị trùng lặp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | Một DFS được thực thi cho mỗi gốc và mỗi DFS truy cập tối đa O(n) đỉnh | 
| Không gian | O(n) | Danh sách kề, ngăn xếp đệ quy và trạng thái DFS | 

Với`n ≤ 2000`, MỘT`O(n²)`giải pháp thực hiện khoảng bốn triệu lần truyền tải cạnh trong trường hợp xấu nhất, phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    MOD = 1000000007

    input = sys.stdin.readline
    d, n = map(int, input().split())
    a = list(map(int, input().split())

    )
    g = [[] for _ in range(n)]

    for _ in range(n - 1):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append(v)
        g[v].append(u)

    sys.setrecursionlimit(10000)

    ans = 0

    for root in range(n):
        base = a[root]

        def dfs(u, p):
            res = 1

            for v in g[u]:
                if v == p:
                    continue
                if a[v] < base or a[v] > base + d:
                    continue
                if a[v] == base and v < root:
                    continue

                res = (res * (dfs(v, u) + 1)) % MOD

            return res

        ans = (ans + dfs(root, -1)) % MOD

    print(ans)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue().strip()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out

# sample 1
assert run(
"""1 4
2 1 3 2
1 2
1 3
3 4
"""
) == "8"

# minimum size
assert run(
"""0 1
7
"""
) == "1"

# all equal values on a chain
assert run(
"""0 3
5 5 5
1 2
2 3
"""
) == "6"

# no pair can coexist
assert run(
"""0 2
1 2
1 2
"""
) == "2"

# star, all values inside range
assert run(
"""10 3
1 2 3
1 2
1 3
"""
) == "6"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đỉnh đơn | 1 | Cây nhỏ nhất có thể | 
| Chuỗi có giá trị bằng nhau | 6 | Hòa giữa các mức tối thiểu bằng nhau | 
| Hai đỉnh, d=0, có giá trị khác nhau | 2 | Lọc hạn chế giá trị | 
| Ngôi sao nhỏ với d lớn | 6 | Đếm tất cả các tập hợp con được kết nối | 

## Vỏ cạnh 

Hãy xem xét:```
0 2
5 5
1 2
```Các bộ hợp lệ là:```
{1}
{2}
{1,2}
```Khi root`1`được xử lý, cả hai đỉnh đều được phép và`{1,2}`được tính. 

Khi root`2`được xử lý, đỉnh`1`bị loại trừ vì nó có cùng giá trị và chỉ số nhỏ hơn. bộ`{1,2}`không thể xuất hiện lại được. Câu trả lời cuối cùng là chính xác`3`. 

Coi như:```
0 2
1 2
1 2
```Các bộ hợp lệ duy nhất là`{1}`Và`{2}`bởi vì sự khác biệt giá trị bên trong`{1,2}`bằng`1`, vượt quá`d=0`. 

Đối với gốc`1`, đỉnh`2`nằm ngoài khoảng cho phép`[1,1]`. 

Đối với gốc`2`, đỉnh`1`nằm ngoài khoảng cho phép`[2,2]`. 

Mỗi DFS đóng góp chính xác một bộ đơn, tạo ra câu trả lời`2`. 

Coi như:```
1 3
1 2 2
1 2
2 3
```Các bộ kết nối hợp lệ là:```
{1}
{2}
{3}
{1,2}
{2,3}
{1,2,3}
```Giới hạn khoảng cho phép tất cả các đỉnh khi root`1`được chọn. DP tạo chính xác mọi tập hợp kết nối có chứa gốc đó. Rễ`2`Và`3`chỉ đóng góp các bộ có giá trị tối thiểu là`2`. Mỗi tập hợp lệ xuất hiện đúng một lần vì đỉnh tối thiểu đại diện là duy nhất.
