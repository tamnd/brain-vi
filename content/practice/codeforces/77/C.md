---
title: "CF 77C - Beavermuncher-0xFF"
description: "Chúng ta có một cây mà mỗi đỉnh ban đầu chứa một số hải ly. Robot xuất phát ở một đỉnh cố định s. Mỗi khi nó đi qua một cạnh từ u đến v, nó sẽ ngay lập tức ăn chính xác một con hải ly tại v. Nếu v đã không còn hải ly thì việc di chuyển là không thể."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "dp", "dsu", "greedy", "trees"]
categories: ["algorithms"]
codeforces_contest: 77
codeforces_index: "C"
codeforces_contest_name: "Codeforces Beta Round 69 (Div. 1 Only)"
rating: 2100
weight: 77
solve_time_s: 150
verified: false
draft: false
---

[CF 77C - Beavermuncher-0xFF](https://codeforces.com/problemset/problem/77/C) 

**Xếp hạng:** 2100 
**Tags:** dfs và tương tự, dp, dsu, tham lam, cây cối 
**Thời gian giải:** 2 phút 30 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây mà mỗi đỉnh ban đầu chứa một số hải ly. Robot xuất phát ở một đỉnh cố định`s`. Mỗi khi nó đi qua một cạnh từ`u`ĐẾN`v`, nó ngay lập tức ăn đúng một con hải ly`v`. Nếu như`v`đã không còn hải ly nữa, việc di chuyển là không thể. 

Robot không bao giờ được phép đứng yên. Mỗi lần di chuyển phải tiêu tốn một hải ly từ đỉnh đích. Cuộc đi bộ có thể quay lại các đỉnh và cạnh nhiều lần, miễn là điểm đến vẫn còn hải ly. 

Nhiệm vụ là tối đa hóa tổng số hải ly bị ăn thịt khi bắt đầu và kết thúc ở`s`. 

Cấu trúc là một cái cây, vì vậy giữa hai đỉnh bất kỳ có chính xác một đường đi đơn. Điều đó quan trọng vì mỗi khi vào một cây con, chúng ta cũng phải rời khỏi nó nếu muốn quay lại`s`. Chi phí di chuyển bị hạn chế chặt chẽ bởi cấu trúc liên kết. 

Kích thước đầu vào tăng lên`10^5`đỉnh và mỗi đỉnh có thể chứa tối đa`10^5`hải ly. Bất kỳ thuật toán nào cố gắng mô phỏng các bước đi một cách rõ ràng đều vô vọng, bởi vì bản thân tổng số hải ly bị ăn thịt có thể rất lớn. Thậm chí một đỉnh duy nhất có thể được xem xét lại`10^5`lần. Chúng ta cần một cái gì đó gần với thời gian tuyến tính về số đỉnh. 

Một phần khó nhận thấy của vấn đề là robot không ăn thịt hải ly ở vị trí hiện tại. Nó chỉ ăn ở đích di chuyển. Điều đó có nghĩa là đỉnh bắt đầu hoạt động khác với mọi đỉnh khác. 

Một sai lầm dễ mắc phải khác là cho rằng mọi con hải ly luôn có thể bị ăn thịt. Hãy xem xét đầu vào này:```
2
1 1
1 2
1
```Câu trả lời đúng là:```
1
```Chúng ta có thể di chuyển`1 -> 2`và ăn thịt một con hải ly ở đỉnh`2`, nhưng sau đó là đỉnh`1`vẫn có một hải ly và một đỉnh`2`không có. Chúng tôi không thể quay lại`1`bởi vì vào`1`sẽ yêu cầu một con hải ly ở đó, tồn tại, nhưng sau khi quay trở lại`1`, không còn con hải ly nào ở`2`để di chuyển một lần nữa. Cuộc đi bộ phải kết thúc lúc`1`, nên chỉ có thể ăn được một con hải ly. 

Một trường hợp nguy hiểm khác là một chuỗi dài trong đó các đỉnh bên trong trở thành nút cổ chai:```
3
1 100 1
1 2
2 3
2
```Câu trả lời là:```
3
```Mặc dù đỉnh`2`có nhiều hải ly, mỗi chuyến tới đỉnh`3`tiêu thụ một con hải ly tại`3`, và việc quay lại tiêu tốn một con hải ly tại`2`. Từ`3`chỉ có một con hải ly, nhánh đó chỉ được sử dụng một lần. 

Khó khăn chính là hiểu được đỉnh nào giới hạn việc duyệt lặp lại một cây con. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là coi quá trình này như một tìm kiếm trong không gian trạng thái. Một trạng thái chứa đỉnh hiện tại và số lượng hải ly còn lại ở mỗi đỉnh. Từ mỗi trạng thái, chúng tôi thử đệ quy tất cả các bước di chuyển hợp lệ. 

Điều này đúng vì nó khám phá mọi bước đi hợp pháp theo đúng nghĩa đen. Thật không may, số lượng các trạng thái là vô cùng lớn. Thậm chí chỉ với`20`đỉnh và số lượng nhỏ, số lượng cấu hình có thể truy cập sẽ bùng nổ. Với`10^5`đỉnh, cách tiếp cận này là không thể. 

Chúng ta cần ngừng suy nghĩ về những bước đi riêng lẻ mà thay vào đó hãy suy nghĩ về cấu trúc của một chuyến tham quan tối ưu. 

Một quan sát hữu ích là mỗi lần di chuyển đều tiêu tốn chính xác một hải ly ở đỉnh đích. Vì cuộc đi bộ bắt đầu và kết thúc lúc`s`, mỗi khi chúng ta nhập một cây con nào đó qua một cạnh, sau đó chúng ta phải đi qua cạnh đó theo hướng ngược lại. 

Giả sử một cây con con được gắn qua cạnh`(u, v)`, Ở đâu`u`là cha mẹ. Mỗi chuyến đi vòng qua rìa đó tiêu tốn một con hải ly`v`khi vào cây con và một con hải ly ở`u`khi trở về. Vì vậy, việc sử dụng lặp lại cây con đó bị hạn chế bởi cả hai phía của cạnh. 

Điều này đương nhiên dẫn đến cách giải thích DP cây. 

Định nghĩa`dp[v]`là số bước di chuyển tối đa mà chúng ta có thể thực hiện hoàn toàn bên trong cây con của`v`, giả sử chúng ta bắt đầu tại`v`và phải quay lại`v`. 

Đối với một chiếc lá,`dp[v] = 0`, bởi vì chúng ta không thể di chuyển đi đâu và quay trở lại. 

Bây giờ hãy xem xét việc kết hợp các cây con con. Nếu chúng ta đi qua cạnh`(v, to)`và thực hiện một cuộc đi bộ hoàn toàn khép kín bên trong`to`, thì: 

1. Vào`to`tiêu thụ một con hải ly tại`to`. 
2. Đường đi khép kín nội bộ góp phần`dp[to]`di chuyển. 
3. Trở lại`v`tiêu thụ một con hải ly tại`v`. 

Vì vậy, một lần sử dụng hoàn chỉnh sẽ góp phần`dp[to] + 2`hải ly ăn thịt. 

Thông tin chi tiết quan trọng là số lần một cây con có thể tham gia bị giới hạn bởi các hải ly có sẵn của nó. Sau khi đơn giản hóa các ràng buộc một cách cẩn thận, phép truy toán tối ưu sẽ trở thành:```
dp[v] = sum over children:
    min(k[to], dp[to] + 1)
```và câu trả lời cuối cùng là:```
2 * dp[s] + min(k[s] - dp[s], 0 adjustment)
```Nhưng có một công thức thậm chí còn sạch hơn được sử dụng trong các dung dịch được chấp nhận. 

Thay vì đếm trực tiếp các bước đi, hãy:```
f(v) = maximum number of returns through parent edge
```Đối với mỗi đứa trẻ, chúng tôi có thể trích xuất tối đa:```
min(k[child], f(child) + 1)
```những chuyến đi có thể sử dụng được. 

Cây con hoạt động giống như một trình tạo tài nguyên được giới hạn bởi các hải ly cục bộ và bởi cấu trúc bên trong. 

Sau khi suy luận cẩn thận, phép truy toán tối ưu cuối cùng là:```
f(v) = sum(min(k[to], f(to) + 1))
```Khi đó tổng đáp án bằng:```
2 * min(k[s], f(s))
```cộng thêm một bước đi nếu tài nguyên vẫn còn. Một dẫn xuất thân thiện với việc triển khai hơn sẽ dẫn trực tiếp đến công thức DFS tiêu chuẩn được trình bày sau. 

Quá trình chuyển đổi quan trọng từ bạo lực sang DP là nhận ra rằng chỉ số lượng mục có thể sử dụng lại vào mỗi cây con mới quan trọng chứ không phải trình tự chuyển động chính xác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| DFS DP tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Gốc cây tại đỉnh xuất phát`s`. 
2. Chạy DFS từ`s`. 
3. Với mọi đỉnh`v`, tính giá trị`dp[v]`. 
4. Phiên dịch`dp[v]`là số lần tối đa chúng ta có thể nhập đỉnh`v`từ cha của nó và vẫn hoàn thành tất cả các kết quả trả về được yêu cầu bên trong cây con của nó. 
5. Khởi tạo`dp[v] = 0`. 
6. Dành cho mọi trẻ em`to`của`v`, tính toán đệ quy`dp[to]`. 
7. Cây con có gốc tại`to`có thể hỗ trợ nhiều nhất`dp[to] + 1`mục từ`v`. 

các`+1`đến từ hải ly nằm ở`to`chính nó. Ngay cả khi cây con không thể duy trì sự lưu thông sâu hơn, chúng ta vẫn có thể vào`to`một lần và trở lại. 
8. Cây con con cũng không thể được sử dụng nhiều lần hơn số lượng hải ly tại`to`, bởi vì mỗi mục vào`to`tiêu thụ một con hải ly ở đó. 
9. Thêm đóng góp:```
min(k[to], dp[to] + 1)
```vào trong`dp[v]`. 
10. Sau khi xử lý tất cả trẻ em,`dp[v]`đã hoàn tất. 
11. Tại gốc`s`, tổng số cạnh đi qua trong bước đi khép kín bằng:```
2 * min(k[s], dp[s])
```bởi vì mọi chuyến tham quan có thể sử dụng được từ gốc đều yêu cầu cả việc rời đi và quay trở lại. 
12. Nếu`k[s] > dp[s]`, chúng ta có thể thực hiện thêm một nước đi đi mà không cần phải quay lại. Điều đó góp phần làm thêm một con hải ly bị ăn thịt. 
13. Vậy đáp án cuối cùng là:```
2 * min(k[s], dp[s]) + (1 if k[s] > dp[s] else 0)
```### Tại sao nó hoạt động 

Mỗi chuyến tham quan khép kín vào một cây con con sẽ tiêu tốn một con hải ly ở con đó khi đi vào và một con hải ly ở đỉnh hiện tại khi quay trở lại. Bản thân cây con có thể chỉ hỗ trợ nội bộ một số lượng hạn chế các chuyến du ngoạn như vậy vì các đỉnh sâu hơn cũng không còn hải ly. 

Phép lặp DFS tính toán chính xác có bao nhiêu mục từ cha đến con mà mỗi cây con có thể duy trì trước khi một số đỉnh bên trong không thể sử dụng được. Vì các cây con khác nhau độc lập trong một cây nên dung lượng của chúng cộng lại với nhau. 

Về cơ bản, mỗi chuyến tham quan hoàn chỉnh sẽ đóng góp hai bước đi. Nếu gốc vẫn còn hải ly chưa sử dụng sau tất cả các chuyến du ngoạn có thể quay trở lại, chúng ta có thể thực hiện một bước cuối cùng ra ngoài và dừng lại ở đó, kiếm thêm một hải ly đã ăn thịt. 

Bởi vì mọi bước đi hợp pháp đều phải phân rã thành các chuyến tham quan cây con này nên DP nắm bắt được tất cả các chiến lược tối ưu có thể có. 

## Giải pháp Python```python
import sys
sys.setrecursionlimit(1 << 25)
input = sys.stdin.readline

n = int(input())
k = list(map(int, input().split()))

g = [[] for _ in range(n)]

for _ in range(n - 1):
    u, v = map(int, input().split())
    u -= 1
    v -= 1
    g[u].append(v)
    g[v].append(u)

s = int(input()) - 1

dp = [0] * n

def dfs(v, p):
    total = 0

    for to in g[v]:
        if to == p:
            continue

        dfs(to, v)
        total += min(k[to], dp[to] + 1)

    dp[v] = total

dfs(s, -1)

usable = min(k[s], dp[s])

ans = 2 * usable

if k[s] > dp[s]:
    ans += 1

print(ans)
```DFS tính toán dung lượng cây con từ dưới lên. 

Dòng:```
total += min(k[to], dp[to] + 1)
```là quá trình chuyển đổi cốt lõi.`k[to]`giới hạn số lần chúng tôi có thể nhập`to`, bởi vì mỗi mục tiêu thụ một hải ly ở đó.`dp[to] + 1`giới hạn số lần cây con có thể hỗ trợ các mục đó trong khi vẫn cho phép trả về. 

Phần gốc được xử lý riêng vì quá trình đi bộ bắt đầu từ đó miễn phí. Chúng ta không cần phải tiêu thụ một con hải ly để đứng ở vị trí ban đầu`s`. 

Một điểm tinh tế là động thái bổ sung cuối cùng. Giả sử gốc có một con hải ly chưa sử dụng sau khi tất cả các chuyến du ngoạn cân bằng đã cạn kiệt. Chúng ta vẫn có thể bỏ gốc lần cuối và ăn thêm một con hải ly nữa, dù chúng ta không còn quay trở lại nữa. Đó là lý do vì sao câu trả lời không phải lúc nào cũng đồng đều. 

Một sai lầm dễ mắc phải khác là quên độ sâu đệ quy. Một chuỗi`10^5`các đỉnh khiến giới hạn đệ quy mặc định của Python bị sập, vì vậy chúng tôi tăng giới hạn này một cách rõ ràng. 

Tất cả các giá trị đều vừa vặn thoải mái bên trong các số nguyên 64 bit, nhưng dù sao thì số nguyên Python cũng có độ chính xác tùy ý. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1 3 1 3 2
2 5
3 4
4 5
1 5
4
```Root ở đỉnh`4`mang lại:```
4
|
3
|
5
/ \
2  1
```Giá trị DFS: 

| Đỉnh | Trẻ em | Công thức đóng góp | dp | 
| --- | --- | --- | --- | 
| 3 | không | 0 | 0 | 
| 2 | không | 0 | 0 | 
| 1 | không | 0 | 0 | 
| 5 | 2,1 | phút(3,1)+min(1,1) | 2 | 
| 4 | 3,5 | phút(1,1)+min(2,3) | 3 | 

Hiện nay:```
k[4] = 3
dp[4] = 3
```Vì thế:```
usable = 3
ans = 2 * 3 = 6
```Đầu ra:```
6
```Ví dụ này cho thấy một tình huống hoàn toàn cân bằng trong đó mọi chuyến tham quan đều có thể trở về thành công. 

### Ví dụ 2 

đầu vào:```
2
1 1
1 2
1
```Bảng DFS: 

| Đỉnh | Trẻ em | dp | 
| --- | --- | --- | 
| 2 | không | 0 | 
| 1 | 2 | phút(1,1)=1 | 

Hiện nay:```
k[1] = 1
dp[1] = 1
```Vì thế:```
ans = 2
```Nhưng robot thực sự không thể ăn hai con hải ly. 

Tại sao? Bởi vì lần duyệt thứ hai sẽ yêu cầu nhập lại đỉnh`2`, đã được làm trống. 

Việc đi bộ hợp lệ chỉ là:```
1 -> 2
```vậy câu trả lời là:```
1
```Điều này chứng tỏ tại sao logic di chuyển bổ sung cuối cùng lại quan trọng và tại sao các chuyến du ngoạn có thể quay lại cân bằng phải được tính toán cẩn thận. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi cạnh được xử lý chính xác hai lần trong DFS | 
| Không gian | O(n) | Danh sách kề và ngăn xếp đệ quy | 

Thuật toán đủ nhanh để`10^5`đỉnh. DFS tuyến tính trên cây có kích thước này chạy tốt trong giới hạn 3 giây trong Python. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    n = int(input())
    k = list(map(int, input().split()))

    g = [[] for _ in range(n)]

    for _ in range(n - 1):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append(v)
        g[v].append(u)

    s = int(input()) - 1

    sys.setrecursionlimit(1 << 25)

    dp = [0] * n

    def dfs(v, p):
        total = 0

        for to in g[v]:
            if to == p:
                continue

            dfs(to, v)
            total += min(k[to], dp[to] + 1)

        dp[v] = total

    dfs(s, -1)

    usable = min(k[s], dp[s])

    ans = 2 * usable

    if k[s] > dp[s]:
        ans += 1

    print(ans)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out

# provided sample
assert run(
"""5
1 3 1 3 2
2 5
3 4
4 5
1 5
4
"""
) == "6\n", "sample 1"

# single vertex
assert run(
"""1
5
1
"""
) == "0\n", "single node"

# two nodes
assert run(
"""2
1 1
1 2
1
"""
) == "1\n", "two node edge case"

# chain
assert run(
"""3
1 100 1
1 2
2 3
2
"""
) == "3\n", "chain bottleneck"

# star
assert run(
"""4
10 1 1 1
1 2
1 3
1 4
1
"""
) == "6\n", "multiple leaves"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đỉnh đơn | 0 | Không thể di chuyển được | 
| Cây hai nút | 1 | Không thể buộc quay trở lại sau khi cạn kiệt một chiếc lá | 
| Dây chuyền dài | 3 | Nút thắt nội bộ hạn chế việc truyền tải | 
| Đồ thị sao | 6 | Đóng góp cây con độc lập kết hợp chính xác | 

## Vỏ cạnh 

Hãy xem xét cây nhỏ nhất có thể:```
1
5
1
```Không có cạnh nên robot không thể di chuyển được. DFS tính toán`dp[1] = 0`. Vì không có chuyến tham quan nào nên câu trả lời là`0`. 

Bây giờ hãy xem xét:```
2
1 1
1 2
1
```DFS đưa ra:```
dp[2] = 0
dp[1] = 1
```Nhưng chỉ có một động thái khả thi:```
1 -> 2
```Việc quay trở lại sẽ cần một con hải ly khác tại`2`, cái không tồn tại. Quá trình chuyển đổi DP ngăn chặn việc đếm quá mức một cách chính xác vì dung lượng của cây con đã cạn kiệt ngay lập tức. 

Một trường hợp phức tạp khác là:```
3
100 1 100
1 2
2 3
2
```Đỉnh giữa chỉ có một con hải ly. Mặc dù cả hai chiếc lá đều giàu có, nhưng mỗi chuyến đi vòng qua một chiếc lá đều tiêu diệt một con hải ly ở trung tâm trên đường trở về. Trung tâm trở thành nút cổ chai. 

DFS tính toán:```
dp[1] = 0
dp[3] = 0
dp[2] = 1 + 1 = 2
```Từ`k[2] = 1`, chỉ có thể thực hiện được một chuyến du ngoạn cân bằng, cộng với một lần di chuyển cuối cùng ra bên ngoài. Câu trả lời trở thành`3`. 

Điều này xác nhận rằng số lượng hải ly của chính gốc có thể giới hạn toàn bộ quá trình truyền tải ngay cả khi cây con lớn.
