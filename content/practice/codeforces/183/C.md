---
title: "CF 183C - Nhuộm tuần hoàn"
description: "Chúng ta có một đồ thị có hướng trong đó mỗi cạnh thực thi một mối quan hệ chặt chẽ giữa màu sắc của các điểm cuối của nó. Nếu một đỉnh có màu c thì mọi đỉnh lân cận đều phải có màu c + 1, bao quanh modulo k."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar"]
categories: ["algorithms"]
codeforces_contest: 183
codeforces_index: "C"
codeforces_contest_name: "Croc Champ 2012 - Final"
rating: 2200
weight: 183
solve_time_s: 212
verified: true
draft: false
---

[CF 183C - Tô màu tuần hoàn](https://codeforces.com/problemset/problem/183/C) 

**Đánh giá:** 2200 
**Thẻ:** dfs và tương tự 
**Thời gian giải:** 3 phút 32s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị có hướng trong đó mỗi cạnh thực thi một mối quan hệ chặt chẽ giữa màu sắc của các điểm cuối của nó. Nếu một đỉnh có màu`c`, thì mọi hàng xóm đi ra đều phải có màu`c + 1`, bao quanh modulo`k`. 

Một cách khác để viết điều kiện là:$color(v) \equiv color(u)+1 \pmod{k}$Mỗi cạnh có hướng sẽ tăng màu đúng một bước trong hệ thống đánh số tuần hoàn. 

Nhiệm vụ là tìm số lớn nhất có thể`k`mà một nhiệm vụ như vậy tồn tại. 

Biểu đồ có thể chứa các vòng tự lặp và nhiều cạnh. Vòng tự đặc biệt bị hạn chế vì một cạnh từ một đỉnh đến chính nó yêu cầu:$color(u) \equiv color(u)+1 \pmod{k}$có nghĩa là`k = 1`. 

Các ràng buộc đủ lớn để chúng ta cần một giải pháp gần như tuyến tính. Với tối đa`10^5`đỉnh và cạnh, bất kỳ thuật toán nào thử tất cả`k`các giá trị và xác nhận chúng một cách độc lập sẽ trở nên quá chậm. Việc duyệt đồ thị bậc hai bị loại trừ ngay lập tức. Chúng ta nên mong đợi điều gì đó xung quanh`O(n + m)`hoặc`O((n + m) log n)`. 

Phần khó khăn là hiểu những hạn chế về màu sắc thực sự ngụ ý gì trên toàn cầu. 

Giả sử chúng ta đi dọc theo một con đường có chiều dài`d`. Mỗi cạnh tăng màu lên một, vì vậy sau khi đi qua đường dẫn, sự khác biệt về màu sắc giữa các điểm cuối buộc phải giảm đi.`d mod k`. 

Bây giờ hãy xem xét một chu trình có chiều dài`L`. Bắt đầu từ một đỉnh và quay trở lại đỉnh đó có nghĩa là:$L \equiv 0 \pmod{k}$Vì vậy, mọi độ dài chu kỳ có hướng phải chia hết cho`k`. 

Sự quan sát đó làm thay đổi hoàn toàn vấn đề. Thay vì suy nghĩ trực tiếp về màu sắc, chúng ta chỉ cần hiểu độ dài chu kỳ. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Hãy xem xét một vòng lặp tự:```
1 1
1 1
```Câu trả lời hợp lệ duy nhất là:```
1
```Việc triển khai bất cẩn mà chỉ xem xét các chu kỳ thông thường có thể bỏ lỡ chu kỳ có độ dài đó`1`lực lượng`k = 1`. 

Một trường hợp nguy hiểm khác là biểu đồ bị ngắt kết nối:```
4 2
1 2
3 4
```Không có chu kỳ định hướng nào cả. Trong DAG, chúng ta có thể sử dụng kích thước lớn tùy ý`k`, bởi vì không bao giờ có mâu thuẫn xuất hiện. Vì vấn đề đòi hỏi`k ≤ n`, câu trả lời đúng là`n = 4`. 

Việc triển khai chỉ tính toán gcds trong các chu kỳ hiện tại có thể vô tình quay trở lại`0`. 

Một ví dụ tinh tế hơn là:```
3 3
1 2
2 3
3 1
```Đồ thị này có một chu trình có độ dài`3`, vậy câu trả lời là`3`. 

Nhưng nếu chúng ta thêm một cạnh khác:```
3 4
1 2
2 3
3 1
1 3
```chúng tôi tạo ra một chu kỳ có độ dài`2`cũng như:`1 -> 3 -> 1`Bây giờ cả hai`2`Và`3`phải chia hết cho`k`, vậy chỉ:```
1
```hoạt động. 

Giải pháp chỉ kiểm tra một chu trình DFS tại một thời điểm có thể trả về không chính xác`3`. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Hãy thử mọi cách có thể`k`từ`n`xuống tới`1`và kiểm tra xem màu hợp lệ có tồn tại hay không. 

Đối với một cố định`k`, chúng ta có thể gán màu bằng DFS hoặc BFS. Mọi cạnh đều thực thi:```
color[v] = (color[u] + 1) mod k
```Nếu chúng ta lấy được hai màu khác nhau cho cùng một đỉnh thì`k`không hợp lệ. 

Việc xác minh này mất`O(n + m)`thời gian. Làm điều đó cho mọi`k`mang lại:```
O(n(n + m))
```Với`n = 10^5`, điều này trở nên quá lớn. 

Quan sát quan trọng là các ràng buộc biên tích lũy dọc theo các đường dẫn. Một con đường dài`d`buộc một sự khác biệt màu sắc của`d mod k`. Bất cứ khi nào hai đường đi khác nhau nối cùng một cặp đỉnh thì độ dài của chúng phải bằng modulo`k`. 

Giả sử DFS gán cho mỗi đỉnh một độ sâu. Đối với mọi cạnh`u -> v`, chúng tôi biết:```
depth[v] = depth[u] + 1
```dọc theo mép cây. 

Nếu chúng ta gặp một cạnh không phải cây thì:```
depth[u] + 1 - depth[v]
```đại diện cho sự khác biệt về độ dài giữa hai cách tiếp cận`v`. 

Sự khác biệt đó phải chia hết cho`k`. 

Việc thu thập các ràng buộc này trên toàn bộ biểu đồ sẽ mang lại:$k \mid (depth[u]+1-depth[v])$cho mọi cạnh. 

Vì vậy giá trị tối đa hợp lệ`k`chỉ đơn giản là gcd của tất cả các giá trị như vậy. 

Tại sao điều này làm việc? 

Mỗi chu kỳ có hướng đều đóng góp một trong những khác biệt này và gcd nắm bắt chính xác ước số chung được chia sẻ bởi tất cả các độ dài chu kỳ. 

Nếu đồ thị không có chu trình có hướng thì mọi khác biệt đều bằng 0, nghĩa là không có hạn chế nào cả. Trong trường hợp đó câu trả lời là`n`. 

Thuật toán cuối cùng thực hiện một DFS trên biểu đồ và tính toán gcd tăng dần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n(n + m)) | O(n) | Quá chậm | 
| Tối ưu | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng danh sách kề của đồ thị có hướng. 
2. Duy trì mảng độ sâu được khởi tạo thành`-1`. Một giá trị của`-1`có nghĩa là đỉnh chưa được thăm. 
3. Bắt đầu DFS từ mọi đỉnh chưa được thăm. Đặt độ sâu bắt đầu thành`0`. 
4. Trong DFS, với mọi cạnh`u -> v`:

Nếu như`v`chưa được thăm, chỉ định:```
depth[v] = depth[u] + 1
```và tiếp tục DFS. 
5. Nếu`v`đã được truy cập, tính toán:```
diff = depth[u] + 1 - depth[v]
```Giá trị này đại diện cho một ràng buộc chu kỳ. Bất kỳ hợp lệ`k`phải chia số này. 
6. Duy trì gcd của tất cả khác 0`diff`các giá trị gặp phải trong DFS. 
7. Sau khi xử lý toàn bộ đồ thị: 

Nếu gcd là`0`, biểu đồ không chứa ràng buộc chu kỳ, vì vậy bất kỳ`k`hoạt động. Câu trả lời tối đa được phép là`n`. 
8. Ngược lại, xuất giá trị tuyệt đối của gcd. 

### Tại sao nó hoạt động 

Độ sâu DFS mã hóa độ dài đường dẫn từ gốc DFS. 

Đối với mọi cạnh`u -> v`, biểu thức:```
depth[u] + 1 - depth[v]
```đo sự khác biệt giữa hai độ dài đường dẫn đến`v`. 

Một màu hợp lệ yêu cầu tất cả độ dài đường đi giữa các đỉnh giống nhau phải đồng nhất theo modulo`k`. Điều đó có nghĩa là mọi sự khác biệt như vậy phải chia hết cho`k`. 

Gcd của tất cả các khác biệt chính xác là số lớn nhất chia đồng thời cho mọi ràng buộc về độ dài chu kỳ. Bất kỳ số lượng lớn hơn sẽ vi phạm ít nhất một chu kỳ. 

Nếu tất cả sự khác biệt bằng 0 thì biểu đồ hoạt động giống như một DAG đối với các ràng buộc này và không có hạn chế nào về`k`tồn tại. 

## Giải pháp Python```python
import sys
from math import gcd

input = sys.stdin.readline

def solve():
    sys.setrecursionlimit(1 << 25)

    n, m = map(int, input().split())

    graph = [[] for _ in range(n)]

    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        graph[u].append(v)

    depth = [-1] * n
    g = 0

    def dfs(u):
        nonlocal g

        for v in graph[u]:
            if depth[v] == -1:
                depth[v] = depth[u] + 1
                dfs(v)
            else:
                diff = depth[u] + 1 - depth[v]
                g = gcd(g, abs(diff))

    for i in range(n):
        if depth[i] == -1:
            depth[i] = 0
            dfs(i)

    if g == 0:
        print(n)
    else:
        print(g)

solve()
```Danh sách kề lưu trữ tất cả các cạnh đi ra. Nhiều cạnh và vòng tự hoạt động tự nhiên mà không cần bất kỳ thao tác xử lý đặc biệt nào. 

các`depth`mảng phục vụ hai mục đích. Nó đánh dấu các đỉnh đã truy cập và ghi lại độ sâu DFS cần thiết để rút ra các ràng buộc chu trình. 

Khi DFS đi theo cạnh cây, độ sâu con sẽ lớn hơn độ sâu gốc. Điều này phù hợp với thực tế là mỗi cạnh được định hướng sẽ tăng màu lên một. 

Đối với một đỉnh đã được thăm, phép tính`diff`nắm bắt sự khác biệt giữa hai con đường. Lấy gcd tăng dần là đủ vì gcd có tính kết hợp:```
gcd(a, b, c) = gcd(gcd(a, b), c)
```Giá trị tuyệt đối là quan trọng. Tùy theo thứ tự di chuyển,`diff`có thể trở thành số âm, nhưng các ràng buộc về tính chia hết chỉ phụ thuộc vào độ lớn. 

Trường hợp đặc biệt`g == 0`xảy ra khi mọi sự khác biệt đều bằng không. Điều đó có nghĩa là không có ràng buộc chu kỳ không cần thiết nào được phát hiện. Trong tình huống này, đồ thị cho phép bất kỳ`k`, vậy giá trị cho phép lớn nhất là`n`. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4 4
1 2
2 1
3 4
4 3
```Truyền tải DFS: 

| Cạnh | độ sâu[u] | độ sâu[v] trước | khác biệt | gcd | 
| --- | --- | --- | --- | --- | 
| 1 → 2 | 0 | -1 | - | 0 | 
| 2 → 1 | 1 | 0 | 2 | 2 | 
| 3 → 4 | 0 | -1 | - | 2 | 
| 4 → 3 | 1 | 0 | 2 | 2 | 

gcd cuối cùng là`2`, vậy đáp án là:```
2
```Dấu vết này cho thấy cả hai thành phần được kết nối đều áp đặt cùng một hạn chế về độ dài chu kỳ. Mỗi độ dài chu kỳ được chia cho`2`. 

### Ví dụ 2 

đầu vào:```
3 3
1 2
2 3
3 1
```Truyền tải DFS: 

| Cạnh | độ sâu[u] | độ sâu[v] trước | khác biệt | gcd | 
| --- | --- | --- | --- | --- | 
| 1 → 2 | 0 | -1 | - | 0 | 
| 2 → 3 | 1 | -1 | - | 0 | 
| 3 → 1 | 2 | 0 | 3 | 3 | 

gcd cuối cùng là`3`. 

Vì vậy, câu trả lời là:```
3
```Chu kỳ có độ dài`3`, nghĩa là màu sắc phải lặp lại ba bước một lần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi đỉnh và cạnh được xử lý một lần | 
| Không gian | O(n + m) | Danh sách kề và trạng thái DFS | 

Kích thước đồ thị đạt`10^5`, vì vậy độ phức tạp tuyến tính chính xác là những gì chúng ta cần. Giải pháp dễ dàng phù hợp với cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string

import sys
import io
from math import gcd

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    sys.setrecursionlimit(1 << 25)

    n, m = map(int, input().split())

    graph = [[] for _ in range(n)]

    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        graph[u].append(v)

    depth = [-1] * n
    g = 0

    def dfs(u):
        nonlocal g

        for v in graph[u]:
            if depth[v] == -1:
                depth[v] = depth[u] + 1
                dfs(v)
            else:
                diff = depth[u] + 1 - depth[v]
                g = gcd(g, abs(diff))

    for i in range(n):
        if depth[i] == -1:
            depth[i] = 0
            dfs(i)

    return str(n if g == 0 else g)

# provided sample
assert run(
"""4 4
1 2
2 1
3 4
4 3
"""
) == "2", "sample 1"

# single self-loop
assert run(
"""1 1
1 1
"""
) == "1", "self-loop forces k = 1"

# DAG, no cycles
assert run(
"""4 2
1 2
3 4
"""
) == "4", "acyclic graph allows any k up to n"

# cycle of length 3
assert run(
"""3 3
1 2
2 3
3 1
"""
) == "3", "simple 3-cycle"

# mixed cycle lengths, gcd becomes 1
assert run(
"""3 4
1 2
2 3
3 1
1 3
"""
) == "1", "cycle lengths 2 and 3"

print("All tests passed.")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Tự lặp đơn | 1 | Lực tự vòng`k = 1`| 
| DAG với các thành phần bị ngắt kết nối | 4 | Không có ràng buộc về chu kỳ nghĩa là câu trả lời là`n`| 
| 3 chu kỳ đơn giản | 3 | Độ dài chu kỳ đơn xác định câu trả lời | 
| Ràng buộc hỗn hợp 2 chu kỳ và 3 chu kỳ | 1 | Logic GCD qua nhiều chu kỳ | 

## Vỏ cạnh 

Một vòng tự lặp ngay lập tức buộc câu trả lời phải`1`. 

đầu vào:```
1 1
1 1
```DFS bắt đầu ở đỉnh`1`với chiều sâu`0`. 

Đối với cạnh`1 -> 1`:```
diff = 0 + 1 - 0 = 1
```gcd trở thành`1`, vậy đáp án cuối cùng là`1`. 

Điều này hoạt động vì vòng lặp tự yêu cầu màu phải bằng màu tiếp theo của chính nó. 

Bây giờ hãy xem xét một đồ thị không có chu trình:```
4 2
1 2
3 4
```DFS chỉ định độ sâu tăng dần, nhưng mọi cạnh đều chuyển đến một nút chưa được thăm dò. Không có cạnh không phải cây nào xuất hiện, vì vậy gcd vẫn còn`0`. 

Đầu ra của thuật toán`n = 4`. 

Điều này phản ánh chính xác rằng đồ thị không theo chu kỳ không áp đặt hạn chế mô đun nào. 

Cuối cùng, hãy xem xét độ dài chu kỳ xung đột:```
3 4
1 2
2 3
3 1
1 3
```Đồ thị chứa cả 3 chu kỳ và 2 chu kỳ. 

DFS tạo ra các ràng buộc với gcd:```
gcd(3, 2) = 1
```Vì thế chỉ`k = 1`hoạt động. 

Thuật toán kết hợp chính xác tất cả thông tin về chu trình thay vì dựa vào một chu trình được phát hiện duy nhất.
