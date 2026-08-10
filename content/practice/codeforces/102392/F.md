---
title: "CF 102392F - Trò chơi trên cây"
description: "Gốc cây đã cho ở đỉnh 1. Trò chơi có thể được xem một cách tự nhiên hơn như một trò chơi trên một đồ thị khác. Tạo một đồ thị có các đỉnh là các đỉnh của cây và nối hai đỉnh bất cứ khi nào một đỉnh là tổ tiên của đỉnh kia trong cây có gốc."
date: "2026-08-10T19:33:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102392
codeforces_index: "F"
codeforces_contest_name: "2019-2020 ICPC Southeastern European Regional Programming Contest (SEERC 2019)"
rating: 0
weight: 102392
solve_time_s: 85
verified: true
draft: false
---

[CF 102392F - Trò chơi trên cây](https://codeforces.com/problemset/problem/102392/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Gốc cây đã cho tại đỉnh`1`. Trò chơi có thể được xem một cách tự nhiên hơn như một trò chơi trên một biểu đồ khác. Tạo một đồ thị có các đỉnh là các đỉnh của cây và nối hai đỉnh bất cứ khi nào một đỉnh là tổ tiên của đỉnh kia trong cây có gốc. Một nước đi trong trò chơi gốc chính xác là một nước đi dọc theo một trong các cạnh này tới một đỉnh chưa từng được ghé thăm trước đó. Các cạnh của cây ban đầu chỉ là một cách để mô tả mối quan hệ tổ tiên này và một bước di chuyển có thể vượt qua nhiều cấp độ. 

Trước tiên, Alice chọn bất kỳ đỉnh nào, tương đương với việc chọn đỉnh bắt đầu của trò chơi địa lý đỉnh trên biểu đồ tổ tiên này. Mỗi nước đi tiếp theo đều đánh dấu một đỉnh chưa được sử dụng trước đó và người chơi không có hàng xóm hợp pháp chưa sử dụng sẽ thua. Đầu ra cần thiết là`Alice`nếu người chơi đầu tiên có chiến lược chiến thắng và`Bob`nếu không thì. 

Có tới`100000`đỉnh và`99999`các cạnh. Với giới hạn thời gian một giây, một thuật toán có công việc phát triển theo phương trình bậc hai đã quá đắt trong Python và bất kỳ thuật toán nào có hàm mũ đều hoàn toàn nằm ngoài tầm với. Bản thân cây có thể được xử lý theo thời gian tuyến tính, vì vậy mục tiêu là`O(n)`thời gian và`O(n)`ký ức. Thách thức chính là đồ thị tổ tiên tiềm ẩn có thể có`Theta(n^2)`các cạnh, vì vậy việc xây dựng nó một cách rõ ràng không phải là một lựa chọn. 

Trường hợp cạnh đầu tiên là một đỉnh duy nhất.```
1
```Câu trả lời đúng là`Alice`. Alice thực hiện nước đi đầu tiên duy nhất có thể có, sau đó Bob không được di chuyển nữa. Việc triển khai chỉ bắt đầu từ rìa cây hoặc giả sử mọi trò chơi đều có ít nhất một nước đi sau vị trí ban đầu có thể xử lý sai trường hợp này. 

Trường hợp cạnh thứ hai là đường đi có số đỉnh chẵn.```
4
1 2
2 3
3 4
```Câu trả lời là`Bob`. Mỗi cặp đỉnh đều có thể so sánh được trên một đường đi có gốc, vì vậy sau lựa chọn đầu tiên của Alice, ba đỉnh còn lại đều có thể truy cập được. Do đó, người chơi sẽ đi thăm tất cả bốn đỉnh, trong đó Bob thực hiện nước đi cuối cùng. Một giải pháp chỉ dựa trên tính chẵn lẻ là hấp dẫn, nhưng nó thất bại đối với các cây phân nhánh, bởi vì các đỉnh không thể so sánh được không thể được sử dụng làm các bước di chuyển liên tiếp. 

Trường hợp cạnh thứ ba đang phân nhánh với số đỉnh chẵn.```
4
1 2
1 3
1 4
```Câu trả lời là`Alice`. Alice có thể bắt đầu từ một chiếc lá. Bob có thể di chuyển đến gốc, sau đó Alice chuyển sang lá khác và Bob không có động thái hợp pháp. Việc coi mọi cặp đỉnh của cây là liền kề nhau sẽ biến biểu đồ này thành một biểu đồ hoàn chỉnh một cách không chính xác và đưa ra sai người chiến thắng. 

Cuối cùng, tổ tiên không có nghĩa là cha mẹ hoặc con cái trực tiếp. Ví dụ,```
4
1 2
2 3
3 4
```cho phép đỉnh`1`để di chuyển trực tiếp đến đỉnh`4`, bởi vì họ là tổ tiên và con cháu. Một thuật toán chỉ xây dựng các cạnh cây ban đầu sẽ giải được một trò chơi khác và có thể thất bại trong các bài kiểm tra ẩn. 

## Phương pháp tiếp cận 

Một giải pháp brute-force trực tiếp có thể biểu diễn tập hợp các đỉnh đen và thử đệ quy mọi đỉnh hợp lệ tiếp theo. Điều này đúng vì trò chơi là hữu hạn nên minimax có thể kiểm tra mọi khả năng tiếp tục diễn ra và xác định xem vị trí hiện tại có thắng hay không. Tuy nhiên, trên một đường đi, mọi đỉnh đều có thể so sánh được với mọi đỉnh khác. Khi đỉnh đầu tiên được chọn, mọi đỉnh còn lại đều có thể được chọn tiếp theo, do đó trò chơi chứa tất cả`n!`có thể hoàn thành các đơn đặt hàng thăm viếng. Do đó, một tìm kiếm đệ quy đơn giản có`Theta(n!)`các nhánh đầu cuối và việc quét các chuyển động có thể có ở mỗi trạng thái có thể đẩy công việc đến`O(n * n!)`. Ngay cả với việc ghi nhớ, có thể có`Theta(n * 2^n)`những trạng thái vẫn còn vô vọng đối với`n = 100000`. 

Nhận xét hữu ích là trò chơi này có đặc điểm phù hợp tiêu chuẩn. Đối với bất kỳ đồ thị vô hướng nào, nếu có sự khớp hoàn hảo, người chơi thứ hai có thể trả lời mọi nước đi bằng cách di chuyển đến đỉnh khớp với đỉnh vừa chọn. Đỉnh phù hợp được đảm bảo không được sử dụng vì đối tác của nó là đỉnh duy nhất có thể sử dụng nó trước đó. 

Hướng ngược lại cũng hữu ích. Giả sử kết quả khớp tối đa không hoàn hảo và để Alice bắt đầu ở một đỉnh chưa từng có. Bob không thể di chuyển từ đỉnh bắt đầu của Alice sang một đỉnh chưa khớp khác, bởi vì việc di chuyển như vậy sẽ tạo ra một đường dẫn tăng cường cho kết quả khớp tối đa. Do đó, mọi đỉnh Bob có thể chạm tới đều trùng khớp. Alice có thể trả lời bằng cách sử dụng đối tác phù hợp của mình, chính xác như chiến lược của người chơi thứ hai hoạt động trong trường hợp kết hợp hoàn hảo. Điều này mang lại cho Alice chiến lược chiến thắng. Đặc tính phù hợp này là sự rút gọn trọng tâm của lý thuyết trò chơi. 

Vì vậy, trò chơi được rút gọn thành một câu hỏi: đồ thị tổ tiên có khớp hoàn hảo không? 

Chúng ta vẫn chưa thể xây dựng được đồ thị tổ tiên vì nó có thể chứa nhiều cạnh bậc hai. Cấu trúc đặc biệt của cây có rễ đã cứu chúng ta. Bên trong cây con của`u`, các đỉnh thuộc hai cây con khác nhau là không thể so sánh được nên chúng không bao giờ có thể ghép đôi với nhau. Đỉnh duy nhất trong`u`cây con của có thể kết nối các cây con con khác nhau là`u`chính nó. Điều này làm cho cây DP rất nhỏ có thể thực hiện được. 

Cho phép`dp[u]`là số đỉnh tối thiểu còn lại chưa được so sánh sau khi lấy kết quả khớp tối đa hoàn toàn bên trong cây con của`u`. Nếu bọn trẻ cùng nhau rời đi`s`các đỉnh không trùng nhau thì khi`s = 0`,`u`bản thân nó không có ai ở dưới nó để ghép nối, vì vậy vẫn còn một đỉnh chưa từng có. Khi`s > 0`,`u`có thể được ghép nối với một trong những hậu duệ chưa từng có đó, để lại`s - 1`. Như vậy 

dp[u]={ 1, s−1, ​ s=0, s>0, ​ 

ở đâu 

s= v con của bạn ∑ ​ dp[v]. 

Đây chính xác là cây DP được sử dụng cho công thức so khớp tổ tiên-con cháu của bài toán. 

Gốc có sự kết hợp hoàn hảo chính xác khi`dp[1] = 0`. Nếu nó bằng 0 thì Bob thắng. Ngược lại, Alice sẽ thắng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(n · n!)`không ghi nhớ |`O(n)`đệ quy/trạng thái | Quá chậm | 
| Tối ưu |`O(n)`|`O(n)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng cây gốc dưới dạng danh sách kề. Chúng ta không bao giờ xây dựng một cách rõ ràng các cạnh tổ tiên-con cháu, bởi vì có thể có`Theta(n^2)`của họ. 
2. Gốc cây tại đỉnh`1`và tính toán mảng cha cùng với thứ tự duyệt. Việc xử lý các đỉnh theo thứ tự truyền tải ngược sẽ tạo ra cây DP từ dưới lên mà không cần dựa vào độ sâu đệ quy của Python. 
3. Với mọi đỉnh`u`, tính toán 

s=∑dp[v] 

trên con cái của nó`v`. Mỗi nút con đóng góp số đỉnh tối thiểu chưa từng có mà cây con của nó phải hiển thị. 
4. Nếu`s = 0`, bộ`dp[u] = 1`. Mọi cây con con đều có thể được so khớp hoàn hảo bên trong, do đó không có sẵn cây con nào cho`u`để ghép nối với. đỉnh`u`bản thân nó phải vẫn chưa từng có. 
5. Nếu`s > 0`, bộ`dp[u] = s - 1`. Ít nhất một đỉnh chưa từng tồn tại ở đâu đó bên dưới`u`, và mọi đỉnh như vậy đều là con cháu của`u`, Vì thế`u`có thể được kết hợp với một trong số họ. Điều này tiêu thụ chính xác một đỉnh chưa từng có trước đó. 
6. Sau khi xử lý root xong hãy kiểm tra`dp[1]`. Giá trị bằng 0 có nghĩa là toàn bộ cây có thể được phân chia thành các cặp tổ tiên-con cháu, đây là sự kết hợp hoàn hảo của biểu đồ trò chơi. Bob thắng trong trường hợp đó. Bất kỳ giá trị dương nào có nghĩa là kết quả khớp tối đa để lại ít nhất một đỉnh chưa khớp, vì vậy Alice bắt đầu ở đỉnh chưa khớp đó và thắng. 

Tại sao nó hoạt động: DP duy trì tính bất biến`dp[u]`là số đỉnh không khớp tối thiểu trong số tất cả các kết quả phù hợp chứa hoàn toàn trong cây con của`u`. Các cây con con không thể tương tác với nhau vì các đỉnh từ các cây con khác nhau là không thể so sánh được. Do đó, cải tiến duy nhất có thể có sau khi kết hợp các phần tử con là sử dụng`u`để phù hợp với một hậu duệ được tiếp xúc. Phép truy toán xem xét chính xác hai khả năng đó và do đó tính toán mức tối thiểu thực sự. Ở gốc, không có đỉnh nào chưa khớp tương đương với một kết hợp hoàn hảo. Đối số trò chơi phù hợp sau đó sẽ xác định người chiến thắng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())

    graph = [[] for _ in range(n)]

    for _ in range(n - 1):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        graph[u].append(v)
        graph[v].append(u)

    # Root the tree at 0.
    parent = [-1] * n
    order = [0]
    parent[0] = -2

    for u in order:
        for v in graph[u]:
            if v == parent[u]:
                continue
            parent[v] = u
            order.append(v)

    # Process children before parents.
    dp = [0] * n

    for u in reversed(order):
        total = 0

        for v in graph[u]:
            if parent[v] == u:
                total += dp[v]

        if total == 0:
            dp[u] = 1
        else:
            dp[u] = total - 1

    print("Bob" if dp[0] == 0 else "Alice")

if __name__ == "__main__":
    solve()
```Danh sách kề chỉ lưu trữ`n - 1`mép cây ban đầu. các`parent`mảng bắt rễ cây ở đỉnh`1`, được biểu thị bằng chỉ số`0`bằng Python. các`order`mảng là một quá trình truyền tải từ gốc, vì vậy việc đảo ngược nó đảm bảo rằng mọi đứa trẻ đều đã nhận được`dp`giá trị khi cha của nó được xử lý. 

Bản thân DP có chủ ý nhỏ. Đối với mỗi đỉnh,`total`là tổng các giá trị của các con của nó. biểu thức`total == 0`là trường hợp ranh giới duy nhất trong phép truy toán. Khi nó bằng 0, đỉnh hiện tại đóng góp một đỉnh chưa khớp. Nếu không, nó sẽ tiêu thụ một hậu duệ chưa từng có. 

Việc thực hiện là lặp đi lặp lại chứ không phải đệ quy. Một con đường với`100000`các đỉnh sẽ tạo độ sâu đệ quy xung quanh`100000`, điều này không an toàn trong Python ngay cả khi giới hạn đệ quy được tăng theo cách thủ công. Không thể xảy ra tràn số nguyên vì số nguyên Python có độ chính xác tùy ý và trên thực tế mọi`dp[u]`nhiều nhất là kích thước của cây con của nó. 

Thứ tự của các hoạt động quan trọng. Việc tính toán cạnh cha trước khi đẩy hàng xóm sẽ ngăn cản cạnh vô hướng trở lại cha mẹ khỏi bị coi là cạnh con. điều kiện`parent[v] == u`trong giai đoạn DP sau đó xác định chính xác các phần tử con của`u`. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Cây là một con đường:```
1 - 2 - 3 - 4
```Mỗi đỉnh có nhiều nhất một con sau khi root tại`1`. 

| Đỉnh | Trẻ em | Tổng con |`dp`| 
| --- | --- | --- | --- | 
| 4 | không | 0 | 1 | 
| 3 | 4 | 1 | 0 | 
| 2 | 3 | 0 | 1 | 
| 1 | 2 | 1 | 0 | 

Gốc có`dp[1] = 0`, do đó đồ thị tổ tiên có sự kết hợp hoàn hảo. Một sự phù hợp như vậy là`(1,2), (3,4)`. Bob luôn có thể phản hồi bằng cách sử dụng đối tác phù hợp với nước đi mới nhất của Alice, vì vậy kết quả đầu ra là`Bob`. 

### Mẫu 2 

Sau khi root tại`1`, đỉnh`1`có con`2`Và`3`. đỉnh`2`có con`4`,`5`,`6`, Và`7`, trong khi`3`là một chiếc lá. 

| Đỉnh | Trẻ em | Tổng con |`dp`| 
| --- | --- | --- | --- | 
| 4 | không | 0 | 1 | 
| 5 | không | 0 | 1 | 
| 6 | không | 0 | 1 | 
| 7 | không | 0 | 1 | 
| 2 | 4, 5, 6, 7 | 4 | 3 | 
| 3 | không | 0 | 1 | 
| 1 | 2, 3 | 4 | 3 | 

Gốc có`dp[1] = 3`, do đó ba đỉnh vẫn chưa được so sánh sau khi kết hợp tốt nhất có thể. Không có sự kết hợp hoàn hảo nào và Alice thắng. Điều này phù hợp với chiến lược mẫu, trong đó Alice bắt đầu ở đỉnh`3`và sử dụng cấu trúc phân nhánh để ngăn Bob duy trì phản hồi trùng khớp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n)`| Mỗi cạnh của cây được kiểm tra một số lần không đổi. | 
| Không gian |`O(n)`| Danh sách kề, mảng cha, thứ tự truyền tải và mảng DP đều sử dụng không gian tuyến tính. | 

Đầu vào chứa tối đa`100000`các đỉnh, do đó việc xử lý tuyến tính nằm trong giới hạn dự kiến. Thuật toán không bao giờ xây dựng đồ thị tổ tiên có kích thước bậc hai và tránh các vấn đề về độ sâu DFS đệ quy trên đường dẫn suy biến. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline

    n = int(input())
    graph = [[] for _ in range(n)]

    for _ in range(n - 1):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        graph[u].append(v)
        graph[v].append(u)

    parent = [-1] * n
    parent[0] = -2
    order = [0]

    for u in order:
        for v in graph[u]:
            if v == parent[u]:
                continue
            parent[v] = u
            order.append(v)

    dp = [0] * n

    for u in reversed(order):
        total = 0
        for v in graph[u]:
            if parent[v] == u:
                total += dp[v]

        dp[u] = 1 if total == 0 else total - 1

    print("Bob" if dp[0] == 0 else "Alice")

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_input = globals()["input"]

    sys.stdin = io.StringIO(inp)
    globals()["input"] = sys.stdin.readline

    try:
        from io import StringIO
        old_stdout = sys.stdout
        sys.stdout = StringIO()

        solve()
        result = sys.stdout.getvalue().strip()

        sys.stdout = old_stdout
        return result
    finally:
        sys.stdin = old_stdin
        globals()["input"] = old_input

# Provided samples
assert run(
    """4
1 2
2 3
3 4
"""
) == "Bob", "sample 1"

assert run(
    """7
2 1
2 6
1 3
2 5
7 2
2 4
"""
) == "Alice", "sample 2"

# Minimum-size tree
assert run(
    """1
"""
) == "Alice", "single vertex"

# Small branching tree
assert run(
    """4
1 2
1 3
1 4
"""
) == "Alice", "star with three leaves"

# Perfect matching in the ancestor graph
assert run(
    """6
1 2
1 3
2 4
3 5
4 6
"""
) == "Bob", "perfect ancestor matching"

# Maximum-size path, n is even
n = 100000
edges = "\n".join(f"{i} {i + 1}" for i in range(1, n))
assert run(f"{n}\n{edges}\n") == "Bob", "maximum-size path"

# Maximum-size star, repeated identical leaf structure
n = 100000
edges = "\n".join(f"1 {i}" for i in range(2, n + 1))
assert run(f"{n}\n{edges}\n") == "Alice", "maximum-size star"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`Alice`| Trường hợp ranh giới kích thước tối thiểu và tái phát lá | 
|`1-2, 1-3, 1-4`|`Alice`| Phân nhánh ở nơi chỉ tính chẵn lẻ là không đủ | 
|`1-2, 1-3, 2-4, 3-5, 4-6`|`Bob`| Một sự kết hợp hoàn hảo không cần thiết | 
| Đường dẫn với`100000`đỉnh |`Bob`| Độ sâu tối đa và di chuyển lặp đi lặp lại | 
| Sao với`100000`đỉnh |`Alice`| Mức độ tối đa và các trạng thái con giống hệt nhau lặp đi lặp lại | 

Cụm từ "các giá trị hoàn toàn bằng nhau" không tương ứng với đặc điểm đầu vào ở đây vì bài toán không có giá trị đỉnh. Ngôi sao có kích thước tối đa phục vụ cùng một mục đích thử nghiệm về mặt cấu trúc: mọi lá đều có trạng thái cục bộ giống hệt nhau, do đó, nó kiểm tra xem các khoản đóng góp DP giống hệt nhau lặp đi lặp lại có được tích lũy chính xác hay không. 

## Vỏ cạnh 

Đối với trường hợp một đỉnh,```
1
```quá trình duyệt chỉ chứa gốc. Tổng con của nó bằng 0, vì vậy`dp[1] = 1`. Gốc không thể được so khớp hoàn hảo, có nghĩa là biểu đồ tổ tiên không có kết quả khớp hoàn hảo. Alice thắng ngay lập tức và thuật toán in ra`Alice`. 

Đối với đường đi bốn đỉnh,```
4
1 2
2 3
3 4
```các giá trị từ dưới lên là`dp[4] = 1`,`dp[3] = 0`,`dp[2] = 1`, Và`dp[1] = 0`. Số 0 ở gốc biểu thị sự phù hợp`(1,2), (3,4)`. Bob có thể sử dụng những cặp này làm câu trả lời, đưa ra`Bob`. Trường hợp này cũng xác nhận rằng DP không chỉ đơn giản là đếm các đỉnh theo modulo hai. 

Đối với cây phân nhánh,```
4
1 2
1 3
1 4
```mỗi chiếc lá góp phần`1`, do đó gốc nhận được tổng con của`3`và thu được`dp[1] = 2`. Không có sự kết hợp hoàn hảo. Alice có thể bắt đầu từ một chiếc lá, sau đó Bob bị buộc phải đi đến gốc và Alice lấy một chiếc lá khác. Bob sau đó không có động thái hợp pháp. Thuật toán in chính xác`Alice`. 

Đối với trường hợp kết hợp hoàn hảo không tầm thường,```
6
1 2
1 3
2 4
3 5
4 6
```các giá trị là`dp[6] = 1`,`dp[4] = 0`,`dp[2] = 1`,`dp[5] = 1`,`dp[3] = 0`, và cuối cùng`dp[1] = 0`. Sự kết hợp hoàn hảo là`(4,6), (2,1), (3,5)`. Các cặp không nhất thiết phải là cạnh cây gốc nói chung, chỉ cần các cặp tổ tiên-con cháu. Vì việc so khớp bao trùm mọi đỉnh nên Bob có phản ứng với mọi nước đi của Alice. 

Đối với đường dẫn có kích thước tối đa, việc triển khai không bao giờ đệ quy đi xuống`100000`xếp chồng các khung. Nó xây dựng trật tự cha một cách lặp đi lặp lại, xử lý thứ tự ngược lại và thu được`dp[1] = 0`bởi vì một đường dẫn có độ dài chẵn có sự kết hợp hoàn hảo. Kết quả là`Bob`. 

Đối với ngôi sao có kích thước tối đa, mỗi chiếc lá đều đóng góp`1`. Gốc nhìn thấy`99999`con cháu vô song và tiêu thụ một trong số họ, để lại`99998`, Vì thế`dp[1] > 0`. Kết quả là`Alice`. Điều này cũng thực hiện mức độ lớn nhất có thể và xác nhận rằng các khoản đóng góp con phải được tính tổng thay vì giảm xuống trạng thái Boolean.
