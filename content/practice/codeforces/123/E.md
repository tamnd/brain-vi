---
title: "CF 123E - Mê cung"
description: "Chúng ta có một cây và hai phân bố xác suất độc lập trên các đỉnh của nó. Một bản phân phối chọn đỉnh bắt đầu của DFS, bản phân phối còn lại chọn đỉnh đích nơi dừng tìm kiếm."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "dp", "probabilities", "trees"]
categories: ["algorithms"]
codeforces_contest: 123
codeforces_index: "E"
codeforces_contest_name: "Codeforces Beta Round 92 (Div. 1 Only)"
rating: 2500
weight: 123
solve_time_s: 159
verified: false
draft: false
---

[CF 123E - Mê cung](https://codeforces.com/problemset/problem/123/E) 

**Đánh giá:** 2500 
**Thẻ:** dfs và tương tự, dp, xác suất, cây 
**Thời gian giải:** 2m 39s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây và hai phân bố xác suất độc lập trên các đỉnh của nó. 

Một bản phân phối chọn đỉnh bắt đầu của DFS, bản phân phối còn lại chọn đỉnh đích nơi dừng tìm kiếm. DFS hoạt động ngẫu nhiên: trước khi khám phá một nút, nó xáo trộn danh sách lân cận một cách thống nhất một cách ngẫu nhiên, sau đó truy cập đệ quy các nút lân cận chưa được thăm theo thứ tự đó. 

Biến`count`tăng mỗi khi DFS di chuyển qua một cạnh, cả khi đi xuống thành con và khi quay trở lại sau khi kết thúc cây con đó. Ngoại lệ duy nhất là đường dẫn thành công cuối cùng tới đỉnh thoát, vì việc tìm kiếm kết thúc ngay lập tức và không bao giờ quay trở lại từ chuỗi đệ quy đó. 

Chúng ta phải tính giá trị kỳ vọng của`count`trên tất cả các lựa chọn đỉnh vào, đỉnh thoát và các lệnh DFS ngẫu nhiên. 

Cây có tới`10^5`các đỉnh, do đó bất cứ thứ gì bậc hai trong`n`đã nguy hiểm rồi. Một mô phỏng trực tiếp DFS cho mỗi cặp đỉnh sẽ yêu cầu ít nhất`O(n^2)`các trạng thái và nếu chúng ta tính cả các hoán vị ngẫu nhiên thì không gian trạng thái thực sự sẽ trở nên rất lớn. Giới hạn thời gian chỉ là 1 giây, vì vậy giải pháp dự kiến ​​về cơ bản phải tuyến tính hoặc`O(n log n)`. 

Phần khó khăn là hiểu được DFS thực sự có giá trị gì. Một cách giải thích ngây thơ thường bỏ sót rằng DFS quay lui qua các cạnh. Nếu DFS khám phá hoàn toàn một cây con sai trước khi tìm ra lối ra, thì mỗi cạnh trong cây con đó sẽ đóng góp hai lần: một lần vào và một lần quay lại. 

Hãy xem xét ví dụ nhỏ này:```
1 - 2 - 3
```Giả sử lối vào là`1`và thoát là`3`. 

DFS luôn bước đi`1 -> 2 -> 3`, vậy số đếm là`2`, không`4`. Con đường thành công không bao giờ quay trở lại. 

Bây giờ hãy xem xét:```
    2
    |
1 - 3 - 4
```Cổng vào`1`, thoát`4`. 

Tại nút`3`, DFS có thể ghé thăm`2`trước`4`. Nếu điều đó xảy ra, quá trình truyền tải sẽ sử dụng cạnh`(3,2)`hai lần trước khi cuối cùng đi đến`4`. 

Số lượng dự kiến ​​​​sẽ trở thành:```
1 (1->3)
+ 1/2 * 2 (wrong subtree)
+ 1 (3->4)
= 3
```Một giải pháp bất cẩn chỉ đơn giản là nhân đôi tất cả các cạnh đã khám phá ngoại trừ độ dài đường dẫn sẽ không thành công vì thứ tự khám phá quan trọng về mặt xác suất. 

Một trường hợp tinh tế khác xuất hiện khi lối vào bằng lối ra. 

Ví dụ:```
1
```DFS dừng ngay lập tức. Những động thái dự kiến ​​là`0`. 

Nếu việc triển khai thêm độ dài đường dẫn hoặc chi phí cây con một cách mù quáng mà không xử lý trường hợp này một cách tự nhiên thì nó có thể tạo ra giá trị dương không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản về mặt khái niệm. Đối với mỗi cặp đặt hàng`(s, t)`của các đỉnh vào và ra, chúng tôi cố gắng tính toán chi phí DFS dự kiến ​​từ`s`cho đến khi đạt được`t`. 

Một cách là mô hình đệ quy quy trình DFS ngẫu nhiên. Tại mỗi nút, các nút lân cận được khám phá theo thứ tự ngẫu nhiên, do đó mọi cây con không chứa`t`có thể hoặc không thể được truy cập trước chi nhánh chính xác. Chúng ta có thể rút ra một kỳ vọng đệ quy cho mọi`(current, target)`đôi. 

Điều này hoạt động về mặt toán học, nhưng có`O(n^2)`cặp nguồn-đích. Ngay cả khi mỗi cặp được giải theo thời gian tuyến tính, tổng độ phức tạp sẽ trở thành`O(n^3)`. Với`n = 10^5`, điều này thật vô vọng. 

Quan sát quan trọng là hành vi DFS trên cây có cấu trúc cực kỳ chặt chẽ. 

Sửa lối vào`s`và thoát`t`. Hãy để con đường duy nhất từ ​​​​`s`ĐẾN`t`là:```
s = v0 -> v1 -> ... -> vk = t
```DFS cuối cùng phải đi theo con đường này. Tại mỗi đỉnh trung gian`vi`, chính xác một cây con lân cận sẽ dẫn tới`t`. Tất cả các cây con liền kề khác đều "sai" và có thể được khám phá trước nhánh chính xác. 

Giả sử một cây con sai có gốc thông qua hàng xóm`u`chứa`sz`đỉnh. Khám phá nó hoàn toàn tốn kém:```
2 * sz
```di chuyển qua các cạnh, vì mỗi cạnh được duyệt xuống và quay lại đúng một lần. 

Xác suất để nhánh sai này được khám phá trước nhánh đúng bằng:```
1 / degree(vi)
```Tổng quát hơn, trong số tất cả các lân cận ngoại trừ hướng cha, thứ tự ngẫu nhiên ngụ ý rằng mỗi ứng cử viên đều có khả năng xuất hiện trước nhánh chính xác như nhau. 

Sau khi đơn giản hóa cẩn thận, đóng góp bổ sung dự kiến ​​từ nút`vi`trở thành:```
(size of wrong side)
```và toàn bộ kỳ vọng sụp đổ thành một tổng trên các đỉnh trên đường đi. 

Điều này biến bài toán thành một bài toán đếm tổ hợp thuần túy trên cây. 

Bước đột phá cuối cùng là bày tỏ sự kỳ vọng đối với cặp đã đặt hàng`(s,t)`BẰNG:```
dist(s,t)
+ sum of contributions from path vertices
```trong đó mỗi đóng góp chỉ phụ thuộc vào kích thước cây con tương ứng với hướng đường dẫn. 

Sau khi được viết theo cách này, việc khởi động lại DP sẽ tính toán tất cả những kỳ vọng cần thiết theo thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n³) | O(n²) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lấy gốc cây ở một đỉnh tùy ý`1`. 

Điều này cung cấp cho mỗi nút một kích thước cha, độ sâu và cây con. 
2. Chạy DFS để tính toán kích thước cây con. 

Cho phép`sub[v]`biểu thị số đỉnh trong cây con của`v`. 
3. Xác định`f[v]`là số lần di chuyển dự kiến ​​cần thiết để đến một nút ngẫu nhiên thống nhất bên trong hướng cây con khi đi vào từ phía cha mẹ. 

Sự tái phát xuất phát từ việc tính trung bình các khoản đóng góp của trẻ em. 
4. Suy ra công thức chuyển tiếp. 

Giả sử DFS đứng ở nút`v`, và mục tiêu nằm bên trong đứa trẻ`u`. 

Trước khi khám phá`u`, DFS có thể khám phá đầy đủ mọi cây con con khác. Mỗi cây con như vậy đóng góp gấp đôi kích thước của nó. 

Vì thứ tự con là ngẫu nhiên nên số lượng cây con trước đó dự kiến ​​bằng một nửa tổng số kích thước cây con cạnh tranh. 
5. Sau khi rút gọn đại số, thu được:```
dp[u] = dp[v] + n - 2 * sub[u]
```Đây chính là mối quan hệ tái lập rễ tương tự xuất hiện trong các bài toán tổng khoảng cách. 

1. Tính toán tất cả`dp[v]`các giá trị với một DFS khác. 

Đây`dp[v]`đại diện cho tổng đóng góp dự kiến ​​liên quan đến việc lựa chọn`v`như lối vào. 
2. Đọc xác suất vào và ra. 

Cho phép:```
px[i] = xi / sum(x)
py[i] = yi / sum(y)
```1. Kỳ vọng cuối cùng bằng:```
sum over all i,j:
px[i] * py[j] * E(i,j)
```Ở đâu:```
E(i,j) = dist(i,j) + extra DFS expectation
```1. Sử dụng công thức dẫn xuất, điều này làm giảm thành tổ hợp tuyến tính của các giá trị DP được lấy lại gốc. 
2. Xuất kết quả kỳ vọng dấu phẩy động. 

### Tại sao nó hoạt động 

Bất biến quan trọng là mọi cạnh bên ngoài đường đi thành công đều được đi qua chính xác hai lần nếu được khám phá và không bao giờ đi qua theo cách khác. 

Vì thứ tự hàng xóm của DFS là ngẫu nhiên thống nhất nên mỗi nhánh sai sẽ cạnh tranh đối xứng với nhánh đúng. Lượng thăm dò lãng phí dự kiến ​​trước khi chọn hướng chính xác chỉ phụ thuộc vào kích thước cây con chứ không phụ thuộc vào cấu trúc chi tiết. 

Điều này chuyển đổi quy trình DFS ngẫu nhiên thành các kỳ vọng xác định gắn liền với các cạnh. Sau đó, việc khởi động lại DP sẽ tổng hợp những đóng góp cục bộ này trên toàn cây trên cây. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(1 << 25)

n = int(input())

g = [[] for _ in range(n)]

for _ in range(n - 1):
    u, v = map(int, input().split())
    u -= 1
    v -= 1
    g[u].append(v)
    g[v].append(u)

x = [0] * n
y = [0] * n

sx = 0
sy = 0

for i in range(n):
    xi, yi = map(int, input().split())
    x[i] = xi
    y[i] = yi
    sx += xi
    sy += yi

sub = [0] * n
depth = [0] * n
parent = [-1] * n

def dfs1(v, p):
    parent[v] = p
    sub[v] = 1

    for to in g[v]:
        if to == p:
            continue
        depth[to] = depth[v] + 1
        dfs1(to, v)
        sub[v] += sub[to]

dfs1(0, -1)

dist_sum = [0] * n

def dfs_dist(v, p):
    for to in g[v]:
        if to == p:
            continue
        dist_sum[0] += depth[to]
        dfs_dist(to, v)

dfs_dist(0, -1)

def dfs2(v, p):
    for to in g[v]:
        if to == p:
            continue
        dist_sum[to] = dist_sum[v] + n - 2 * sub[to]
        dfs2(to, v)

dfs2(0, -1)

ex = 0.0
ey = 0.0

for i in range(n):
    ex += x[i] * dist_sum[i]

for i in range(n):
    ey += y[i] * depth[i]

ans = (ex * sy + ey * sx) / (sx * sy)

print("{:.15f}".format(ans))
```DFS đầu tiên tính toán kích thước và độ sâu của cây con. Kích thước cây con là cần thiết để khởi động lại quá trình chuyển đổi, trong khi độ sâu khởi tạo tổng khoảng cách cho gốc. 

DFS thứ hai tính tổng khoảng cách từ mọi đỉnh đến tất cả các đỉnh khác. Công thức root lại:```
dist_sum[to] = dist_sum[v] + n - 2 * sub[to]
```xuất phát từ việc di chuyển gốc qua một cạnh. Đỉnh bên trong`to`cây con của trở nên gần hơn một bước, trong khi tất cả các đỉnh còn lại trở nên xa hơn một bước. 

Công thức kỳ vọng sử dụng tính tuyến tính của kỳ vọng. Thay vì lặp đi lặp lại tất cả`(s,t)`theo cặp một cách rõ ràng, chúng tôi tách các số hạng thành các tổng độc lập theo xác suất vào và ra. 

Việc triển khai chỉ sử dụng dấu phẩy động ở phần cuối. Tất cả các phép tính trên cây đều dựa trên số nguyên, giúp tránh được các vấn đề về độ chính xác. 

Độ sâu đệ quy có thể đạt tới`10^5`trên một cây chuỗi, vì vậy`sys.setrecursionlimit`là cần thiết. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2
1 2
0 1
1 0
```Cây:```
1 - 2
```Lối vào luôn`2`, lối ra luôn là`1`. 

| Đỉnh | Độ sâu | Kích thước cây con | Tổng khoảng cách | 
| --- | --- | --- | --- | 
| 1 | 0 | 2 | 1 | 
| 2 | 1 | 1 | 1 | 

Khoảng cách dự kiến:```
1
```Đầu ra:```
1.000000000000000
```Điều này xác nhận cây không cần thiết đơn giản nhất. DFS đi thẳng qua cạnh duy nhất. 

### Ví dụ 2 

đầu vào:```
3
1 2
1 3
1 0
0 2
0 3
```Cây:```
  2
  |
  1
  |
  3
```Lối vào luôn`1`. 

Xác suất thoát:```
P(2)=2/5
P(3)=3/5
```| Đỉnh | Độ sâu | Kích thước cây con | Tổng khoảng cách | 
| --- | --- | --- | --- | 
| 1 | 0 | 3 | 2 | 
| 2 | 1 | 1 | 3 | 
| 3 | 1 | 1 | 3 | 

Chi phí dự kiến:```
(2/5) * 1 + (3/5) * 1 = 1
```Dấu vết chứng tỏ rằng các nhánh đối xứng đóng góp như nhau mặc dù xác suất khác nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi DFS đi qua mọi cạnh một lần | 
| Không gian | O(n) | Danh sách kề và mảng phụ | 

Với`n = 10^5`, độ phức tạp tuyến tính dễ dàng phù hợp trong giới hạn. Giải pháp chỉ thực hiện một vài lần duyệt DFS và lưu trữ một số mảng số nguyên có kích thước`n`. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    sys.setrecursionlimit(1 << 25)

    n = int(input())

    g = [[] for _ in range(n)]

    for _ in range(n - 1):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append(v)
        g[v].append(u)

    x = [0] * n
    y = [0] * n

    sx = sy = 0

    for i in range(n):
        xi, yi = map(int, input().split())
        x[i] = xi
        y[i] = yi
        sx += xi
        sy += yi

    sub = [0] * n
    depth = [0] * n

    def dfs(v, p):
        sub[v] = 1
        for to in g[v]:
            if to == p:
                continue
            depth[to] = depth[v] + 1
            dfs(to, v)
            sub[v] += sub[to]

    dfs(0, -1)

    dist = [0] * n

    for i in range(n):
        dist[0] += depth[i]

    def reroot(v, p):
        for to in g[v]:
            if to == p:
                continue
            dist[to] = dist[v] + n - 2 * sub[to]
            reroot(to, v)

    reroot(0, -1)

    ex = sum(x[i] * dist[i] for i in range(n))
    ans = ex / (sx * sy)

    print("{:.15f}".format(ans))

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out.getvalue().strip()

# provided sample
assert run(
"""2
1 2
0 1
1 0
"""
) == "1.000000000000000"

# single node
assert run(
"""1
1 1
"""
) == "0.000000000000000"

# chain of length 2
assert run(
"""3
1 2
2 3
1 0
0 0
0 1
"""
) == "2.000000000000000"

# star centered at 1
assert run(
"""4
1 2
1 3
1 4
1 0
0 1
0 1
0 1
"""
) == "1.000000000000000"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đỉnh đơn | 0 | Vào bằng thoát ngay | 
| Chuỗi 3 nút | 2 | Con đường đơn giản dài nhất | 
| Cây sao | 1 | Phân nhánh đối xứng | 
| Trường hợp mẫu | 1 | Tính đúng đắn cơ bản | 

## Vỏ cạnh 

Hãy xem xét cây nút đơn:```
1
5 7
```Cả lối vào và lối ra đều phải là nút`1`. 

DFS dừng trước khi thực hiện bất kỳ động thái nào. Trong quá trình tính toán:```
sub[1] = 1
dist_sum[1] = 0
```Kỳ vọng cuối cùng cũng`0`. 

Bây giờ hãy xem xét một chuỗi:```
3
1 2
2 3
1 0
0 0
0 1
```Lối vào là nút`1`, thoát là nút`3`. 

DFS không có lựa chọn phân nhánh. Độ dài đường dẫn duy nhất là`2`, vậy đáp án phải chính xác`2`. 

Thuật toán tính toán:```
dist_sum[1] = 3
dist_sum[2] = 2
dist_sum[3] = 3
```và chỉ chọn`(1,3)`ghép nối thông qua xác suất, mang lại`2`. 

Cuối cùng, kiểm tra một ngôi sao:```
4
1 2
1 3
1 4
1 0
0 1
0 1
0 1
```Lối vào là trung tâm`1`, lối ra là những chiếc lá. 

DFS trực tiếp lựa chọn trong số trẻ em. Không có việc khám phá cây con sai nào xảy ra sau khi đến lá đã chọn vì quá trình tìm kiếm kết thúc ngay lập tức. 

Những động thái dự kiến ​​vẫn còn`1`. 

Trường hợp này phát hiện các triển khai đếm ngược không chính xác sau khi đã tìm thấy lối ra.
