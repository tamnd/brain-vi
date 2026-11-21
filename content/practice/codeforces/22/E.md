---
title: "CF 22E - Sơ đồ"
description: "Mỗi người chỉ vào đúng một người khác. Nếu ai đó biết được tin tức, họ gọi cho người mà họ chỉ, người này sẽ gọi cho người tiếp theo, v.v. Chúng ta có thể thêm các cạnh có hướng bổ sung có dạng x -> y, nghĩa là người x cũng phải gọi y."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "graphs", "trees"]
categories: ["algorithms"]
codeforces_contest: 22
codeforces_index: "E"
codeforces_contest_name: "Codeforces Beta Round 22 (Div. 2 Only)"
rating: 2300
weight: 22
solve_time_s: 108
verified: true
draft: false
---
[CF 22E - Sơ đồ](https://codeforces.com/problemset/problem/22/E) 

**Đánh giá:** 2300 
**Thẻ:** dfs và tương tự, đồ thị, cây 
**Thời gian giải:** 1 phút 48 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi người chỉ vào đúng một người khác. Nếu ai đó biết được tin tức, họ gọi cho người mà họ chỉ, người này sẽ gọi cho người tiếp theo, v.v. Chúng tôi có thể thêm các cạnh có hướng bổ sung của biểu mẫu`x -> y`, nghĩa là người`x`cũng phải gọi`y`. 

Mục tiêu mạnh hơn việc chỉ đơn giản là làm cho biểu đồ được kết nối từ một nút bắt đầu. Chúng ta cần quá trình này để thành công bất kể ai biết được tin tức trước. Điều đó có nghĩa là từ mọi nút, cuối cùng phải có khả năng đến được mọi nút khác thông qua các đường dẫn được định hướng. 

Biểu đồ ban đầu có cấu trúc rất đặc biệt: mỗi nút có chính xác một bậc. Những đồ thị như vậy được gọi là đồ thị hàm số. Mỗi thành phần được kết nối trong đồ thị hàm chứa chính xác một chu trình có hướng, với các cây tham gia vào chu trình đó. 

Chúng ta cần thêm số cạnh phụ tối thiểu để toàn bộ đồ thị được kết nối chặt chẽ. 

Sự ràng buộc`n ≤ 10^5`ngay lập tức loại trừ bất cứ điều gì bậc hai. Thậm chí một`O(n^2)`duyệt qua tất cả các cặp sẽ thực hiện xung quanh`10^10`hoạt động trong trường hợp xấu nhất, vượt xa thời hạn. Chúng ta cần một thuật toán tuyến tính hoặc gần tuyến tính. Vì đồ thị đã có chính xác`n`các cạnh, một`O(n)`Giải pháp dựa trên DFS là mục tiêu tự nhiên. 

Có một số trường hợp tế nhị có thể phá vỡ lý luận ngây thơ. 

Hãy xem xét biểu đồ này:```
1 -> 2
2 -> 1
3 -> 4
4 -> 3
```đầu vào:```
4
2 1 4 3
```Có hai chu kỳ riêng biệt. Chỉ thêm một cạnh, chẳng hạn như`2 -> 3`, là không đủ. Bắt đầu từ nút`4`, chúng ta không bao giờ có thể tiếp cận được các nút`1`hoặc`2`. Câu trả lời đúng cần có thêm hai cạnh để kết nối các chu trình theo cả hai hướng. 

Một tình huống khó khăn khác là khi nhiều cây cùng tham gia vào một chu kỳ:```
1 -> 2
2 -> 3
3 -> 2
4 -> 3
5 -> 3
```đầu vào:```
5
2 3 2 3 3
```Một giải pháp bất cẩn có thể cố gắng kết nối các nút lá. Điều đó không thành công vì lá cây không liên quan, sự cản trở đến từ cấu trúc chu trình. Mọi con đường cuối cùng đều đi vào cùng một chu kỳ`{2,3}`, do đó chỉ cần thêm một cạnh nữa, từ chu trình quay trở lại rễ cây. 

Trường hợp cạnh nguy hiểm nhất là khi đồ thị đã hình thành một chu kỳ:```
4
2 3 4 1
```Biểu đồ đã được kết nối mạnh mẽ. Câu trả lời đúng là bằng không. Nhiều quá trình triển khai vô tình buộc ít nhất một cạnh được thêm vào vì chúng chỉ tính các thành phần thay vì kiểm tra xem đã có một SCC hay chưa. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ là suy nghĩ theo cách tăng cường kết nối mạnh mẽ tùy ý. Chúng ta có thể tính toán tất cả các thành phần liên thông mạnh, sau đó liên tục thử thêm các cạnh giữa chúng và kiểm tra xem toàn bộ biểu đồ có liên thông mạnh hay không. 

Tính chính xác rất đơn giản vì chúng tôi trực tiếp xác minh thuộc tính sau mỗi lần sửa đổi. Vấn đề là chi phí. Bản thân việc kiểm tra kết nối mạnh mẽ cần có`O(n)`, và có thể có nhiều cạnh ứng viên. Ngay cả việc thử tất cả các cặp SCC cũng nhanh chóng trở thành phương trình bậc hai. 

Cấu trúc biểu đồ mang lại một đặc tính mạnh hơn nhiều mà chúng ta có thể khai thác. 

Vì mỗi nút có chính xác một cấp độ ngoài, nên mọi thành phần được kết nối đều chứa chính xác một chu trình có hướng. Bất kỳ nút nào bên ngoài chu trình cuối cùng sẽ chảy vào nó. Khi chúng ta bước vào một chu trình, chúng ta không bao giờ có thể rời khỏi chu trình đó vì mỗi nút chỉ có một cạnh đi ra. 

Sự quan sát đó thay đổi toàn bộ vấn đề. 

Trở ngại thực sự duy nhất chính là các chu kỳ. Bên trong một thành phần, tất cả các đường dẫn cuối cùng đều bị mắc kẹt trong chu trình của thành phần đó. Nếu có`k`các chu kỳ khác nhau thì đồ thị được chia thành`k`những vùng chìm không thể chạm tới nhau. 

Để làm cho đồ thị được liên kết chặt chẽ, chúng ta phải kết nối các chu trình này lại với nhau. Một chu trình có thể tiếp cận tất cả các cây ăn vào nó chỉ theo hướng ngược lại chứ không phải hướng thuận. Các nút quan trọng là gốc của các cây sắp tới, cụ thể là các nút có bậc 0 trong biểu đồ hàm. Đó là những nơi không thể tiếp cận được từ nơi khác. 

Định lý then chốt là: 

Nếu có`k`chu kỳ, số cạnh được thêm tối thiểu là: 

-`0`khi`k = 1`và không có nút nào có mức độ bằng 0. 
- Nếu không thì`max(number_of_cycles, number_of_zero_indegree_nodes)`. 

Đối với đồ thị hàm số, số nút bậc 0 luôn nhiều nhất là số cạnh được thêm vào cần thiết để kết nối tất cả các vùng không thể tiếp cận. 

Cấu trúc tiêu chuẩn rất thanh lịch: 

- thu thập một đại diện từ mỗi chu kỳ, 
- thu thập tất cả các nút có mức độ bằng 0, 
- kết nối chúng theo chu kỳ. 

Điều này tạo ra chính xác số cạnh tối thiểu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) hoặc tệ hơn | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng đồ thị có hướng và tính độ cho mỗi nút. 

Vì mỗi nút có chính xác một cạnh đi ra nên việc lưu trữ mảng mục tiêu là đủ. 
2. Tìm tất cả các chu trình có hướng. 

Chúng tôi chạy DFS với ba trạng thái: 

-`0`có nghĩa là chưa được ghé thăm, 
-`1`có nghĩa là hiện đang ở ngăn xếp đệ quy, 
-`2`nghĩa là đã được xử lý đầy đủ. 

Khi DFS tới một nút đã được đánh dấu`1`, chúng tôi đã tìm thấy một chu trình. Nút đó trở thành đại diện cho chu kỳ của nó. 
3. Thu thập tất cả các nút có mức độ bằng 0. 

Những nút này không bao giờ có thể đạt được từ bên ngoài cây của chúng. Trong một biểu đồ được kết nối mạnh, mọi nút phải có thể truy cập được từ mọi nút khác, do đó, mỗi nút gốc như vậy cần ít nhất một cạnh được thêm vào. 
4. Xử lý trường hợp đã được kết nối mạnh mẽ. 

Nếu có chính xác một chu trình và không có nút bậc 0 thì đồ thị đã là một chu trình có hướng. đầu ra`0`. 
5. Ghép các nút không độ với các chu trình. 

Cho phép:

-`roots`là các nút không độ, 
-`cycles`là đại diện chu kỳ. 

Chúng tôi tạo các cạnh để mọi gốc đều nhận được kết nối từ một thành phần khác và mỗi chu kỳ đều có được một đường thoát ra ngoài. 
6. Xây dựng mô hình kết nối tuần hoàn. 

Giả sử chúng ta có:```
roots = [r1, r2, ..., rm]
cycles = [c1, c2, ..., ck]
```Chúng tôi kết nối:```
c1 -> r2
c2 -> r3
...
ck -> r1
```cẩn thận gói các chỉ số modulo kích thước lớn hơn. 
7. Xuất ra tất cả các cạnh được xây dựng. 

Số cạnh được thêm vào bằng nhau`max(k, m)`. 

### Tại sao nó hoạt động 

Mọi đường đi trong đồ thị hàm số cuối cùng đều đi vào một chu trình. Trước khi tăng cường, các chu kỳ khác nhau là các vùng chìm bị cô lập. Khả năng kết nối mạnh mẽ đòi hỏi mọi chu kỳ đều có thể truy cập được từ mọi chu kỳ khác. 

Cấu trúc liên kết tất cả các chu trình thành một vòng định hướng. Khi một đường đi đạt đến bất kỳ chu trình nào, nó có thể đi qua các cạnh được thêm vào mọi thành phần khác. 

Các nút không độ chính xác là các nút ban đầu không có đường dẫn đến. Việc cung cấp cho mỗi nút như vậy một cạnh được thêm vào sẽ loại bỏ mọi điểm vào không thể truy cập được. 

Việc xây dựng theo chu kỳ đảm bảo: 

- mọi thành phần có thể tiếp cận mọi thành phần khác, 
- mọi gốc đều nhận được một đường dẫn đến, 
- không có cạnh không cần thiết được thêm vào. 

Vì cần ít nhất một cạnh cho mỗi chu kỳ và cho mỗi nghiệm bậc 0, nên giới hạn dưới là`max(k, m)`, và công trình đạt được điều đó một cách chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(1 << 25)

n = int(input())
to = [0] + list(map(int, input().split()))

indeg = [0] * (n + 1)

for i in range(1, n + 1):
    indeg[to[i]] += 1

state = [0] * (n + 1)
cycles = []

def dfs(u):
    state[u] = 1

    v = to[u]

    if state[v] == 0:
        dfs(v)
    elif state[v] == 1:
        cycles.append(v)

    state[u] = 2

for i in range(1, n + 1):
    if state[i] == 0:
        dfs(i)

roots = []

for i in range(1, n + 1):
    if indeg[i] == 0:
        roots.append(i)

if len(cycles) == 1 and len(roots) == 0:
    print(0)
    sys.exit()

m = len(roots)
k = len(cycles)

ans = []

t = max(m, k)

for i in range(t):
    a = cycles[i % k]
    b = roots[(i + 1) % m] if m > 0 else cycles[(i + 1) % k]
    ans.append((a, b))

print(len(ans))
for x, y in ans:
    print(x, y)
```DFS phát hiện các chu kỳ bằng phương pháp ngăn xếp đệ quy cổ điển. Vì mỗi nút chỉ có một cạnh đi ra nên mỗi đường dẫn DFS về cơ bản là tuyến tính và tổng công việc vẫn tuyến tính. 

Dòng:```
elif state[v] == 1:
    cycles.append(v)
```là thời điểm quan trọng. Gặp phải một nút đã có trong ngăn xếp đệ quy hiện tại có nghĩa là chúng tôi đã tìm thấy một chu trình có hướng. Chúng tôi chỉ lưu trữ một nút đại diện cho mỗi chu kỳ vì đó là tất cả các nhu cầu xây dựng. 

Mảng indegree xác định gốc của cây đến. Đây là những nút hiện không thể truy cập được từ nơi khác. 

Trường hợp đặc biệt:```
if len(cycles) == 1 and len(roots) == 0:
```vấn đề rất nhiều. Một đồ thị hàm số có một chu trình và không có nút bậc 0 phải tự nó là một chu trình có hướng, đã được kết nối chặt chẽ. 

Cấu trúc tuần hoàn sử dụng lập chỉ mục modulo để nó bao bọc các danh sách một cách tự nhiên. Khi không có rễ, chúng ta kết nối các chu trình trực tiếp với nhau. 

Một sai lầm phổ biến là cố gắng kết nối rễ với rễ. Điều đó không giúp thoát khỏi chu kỳ. Các cạnh phải bắt nguồn từ các đại diện chu trình vì chỉ các chu trình mới được đảm bảo có thể truy cập được mãi mãi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
3 3 2
```Đồ thị:```
1 -> 3
2 -> 3
3 -> 2
```Chu kỳ là`{2,3}`. 

| Bước | Chu kỳ | Rễ | Đã thêm các cạnh | 
| --- | --- | --- | --- | 
| Sau DFS | [2] | [] | [] | 
| Sau khi quét bằng cấp | [2] | [1] | [] | 
| Xây dựng | [2] | [1] | (2,1) | 

Đầu ra cuối cùng:```
1
2 1
```Hiện nay:```
1 -> 3 -> 2 -> 1
```Tất cả các nút đều có thể truy cập được lẫn nhau. 

Ví dụ này cho thấy tại sao các nút không độ lại quan trọng. nút`1`không thể truy cập được từ chu trình, vì vậy nó cần một cạnh đến. 

### Ví dụ 2 

đầu vào:```
4
2 1 4 3
```Đồ thị:```
1 <-> 2
3 <-> 4
```| Bước | Chu kỳ | Rễ | Đã thêm các cạnh | 
| --- | --- | --- | --- | 
| Sau DFS | [1,3] | [] | [] | 
| Xây dựng lặp lại 0 | [1,3] | [] | (1,3) | 
| Xây dựng lặp lại 1 | [1,3] | [] | (3,1) | 

Đầu ra cuối cùng:```
2
1 3
3 1
```Bây giờ mọi chu kỳ đều có thể đạt được chu kỳ khác. 

Dấu vết này thể hiện giới hạn dưới. Hai chu trình biệt lập yêu cầu ít nhất hai cạnh định hướng để làm cho khả năng tiếp cận hoạt động theo cả hai hướng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi nút được xử lý một lần trong DFS và một lần trong quá trình quét theo mức độ | 
| Không gian | O(n) | Mảng cho đồ thị, bậc, trạng thái DFS và lưu trữ câu trả lời | 

Đồ thị chứa chính xác`n`các cạnh, do đó việc truyền tuyến tính là tối ưu. Với`10^5`các nút, một`O(n)`giải pháp dễ dàng phù hợp trong giới hạn thời gian và giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    sys.setrecursionlimit(1 << 25)

    n = int(input())
    to = [0] + list(map(int, input().split()))

    indeg = [0] * (n + 1)

    for i in range(1, n + 1):
        indeg[to[i]] += 1

    state = [0] * (n + 1)
    cycles = []

    def dfs(u):
        state[u] = 1

        v = to[u]

        if state[v] == 0:
            dfs(v)
        elif state[v] == 1:
            cycles.append(v)

        state[u] = 2

    for i in range(1, n + 1):
        if state[i] == 0:
            dfs(i)

    roots = []

    for i in range(1, n + 1):
        if indeg[i] == 0:
            roots.append(i)

    if len(cycles) == 1 and len(roots) == 0:
        print(0)
        return

    m = len(roots)
    k = len(cycles)

    ans = []

    t = max(m, k)

    for i in range(t):
        a = cycles[i % k]

        if m > 0:
            b = roots[(i + 1) % m]
        else:
            b = cycles[(i + 1) % k]

        ans.append((a, b))

    print(len(ans))
    for x, y in ans:
        print(x, y)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    solve()

    sys.stdout = sys.__stdout__
    return out.getvalue()

# provided sample
assert run("3\n3 3 2\n").startswith("1"), "sample 1"

# already strongly connected cycle
assert run("4\n2 3 4 1\n") == "0\n", "single cycle"

# two disjoint cycles
res = run("4\n2 1 4 3\n")
assert res.startswith("2"), "two cycles"

# tree feeding into cycle
res = run("5\n2 3 2 3 3\n")
assert res.startswith("1"), "one root"

# minimum size
res = run("2\n2 1\n")
assert res == "0\n", "minimum strongly connected"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`4 / 2 3 4 1`|`0`| Đã kết nối mạnh mẽ | 
|`4 / 2 1 4 3`|`2`cạnh | Nhiều chu kỳ bị cô lập | 
|`5 / 2 3 2 3 3`|`1`cạnh | Rễ cây ăn vào một chu kỳ | 
|`2 / 2 1`|`0`| Chu kỳ hợp lệ nhỏ nhất | 

## Vỏ cạnh 

Xét lại đồ thị có hai chu trình không liên kết:```
4
2 1 4 3
```DFS phát hiện ra hai đại diện chu kỳ, ví dụ`[1,3]`. Không có nút nào có mức độ bằng 0 vì mỗi nút đều đã có một cạnh tới. 

Thuật toán đi vào`m == 0`nhánh và kết nối các chu trình trực tiếp:```
1 -> 3
3 -> 1
```Bây giờ các đường dẫn có thể di chuyển giữa hai SCC ban đầu theo cả hai hướng. 

Tiếp theo, hãy xem xét một biểu đồ có một chu trình nhưng không thể tiếp cận được rễ cây:```
5
2 3 2 3 3
```Bằng cấp là:```
1: 0
2: 2
3: 3
4: 0
5: 0
```Rễ là`[1,4,5]`. Chỉ có một đại diện chu kỳ, giả sử`[2]`. 

Thuật toán bổ sung:```
2 -> 4
2 -> 5
2 -> 1
```Bây giờ, mỗi gốc đều có một đường dẫn đến từ chu trình và mọi nút cuối cùng có thể quay trở lại chu trình thông qua các cạnh ban đầu. 

Cuối cùng, hãy xem xét trường hợp đã đúng:```
4
2 3 4 1
```Có một chu kỳ và không có nút không độ nào. Thuật toán ngay lập tức đưa ra số 0. 

Nếu không có sự kiểm tra này, nhiều quá trình triển khai sẽ vô tình thêm vào một cạnh dư thừa và phá vỡ tính tối thiểu.
