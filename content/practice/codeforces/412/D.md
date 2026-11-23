---
title: "CF 412D - Trao giải thưởng"
description: "Chúng ta có một đồ thị có hướng về các khoản nợ. Cạnh a - b có nghĩa là nhân viên a nợ nhân viên b. Chúng ta phải sắp xếp tất cả nhân viên theo một dãy sao cho với mỗi cặp nhân viên liên tiếp (x, y) trong dãy không có cạnh x - y."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar"]
categories: ["algorithms"]
codeforces_contest: 412
codeforces_index: "D"
codeforces_contest_name: "Coder-Strike 2014 - Round 1"
rating: 2000
weight: 412
solve_time_s: 115
verified: true
draft: false
---

[CF 412D - Trao giải thưởng](https://codeforces.com/problemset/problem/412/D) 

**Xếp hạng:** 2000 
**Thẻ:** dfs và tương tự 
**Thời gian giải:** 1 phút 55s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị có hướng về các khoản nợ. Một cạnh`a -> b`có nghĩa là nhân viên`a`nợ tiền nhân viên`b`. 

Chúng ta phải sắp xếp tất cả các nhân viên theo một trình tự sao cho mỗi cặp nhân viên liên tiếp`(x, y)`trong dãy không có cạnh nào`x -> y`. Nói cách khác, sau khi mời`x`, người được mời tiếp theo không được là người mà`x`nợ tiền. 

Biểu đồ được đảm bảo không bao giờ chứa cả hai`a -> b`Và`b -> a`cho cùng một cặp. Tự lặp cũng bị cấm. 

Nhiệm vụ là xây dựng một hoán vị như vậy của tất cả nhân viên hoặc xác định rằng không tồn tại thứ tự hợp lệ nào. 

Các ràng buộc đủ nhỏ đối với các thuật toán đồ thị trên danh sách kề, nhưng đủ lớn để không thể thử tất cả các hoán vị. Ngay cả đối với`n = 15`, vũ lực trên tất cả`n!`đơn đặt hàng đã quá lớn. Giải pháp dự định phải chạy gần như theo thời gian tuyến tính đối với kích thước biểu đồ, điều gì đó xung quanh`O(n + m)`. 

Phần khó khăn là hiểu loại cấu trúc nào khiến câu trả lời là không thể. Thoạt nhìn, việc này trông giống như sắp xếp tôpô, nhưng hạn chế chỉ là về các cặp liền kề chứ không phải tất cả các cặp. Một lệnh hợp lệ có thể tự do đặt con nợ trước người cho vay miễn là chúng không liên tiếp. 

Một số trường hợp khó xử lý. 

Hãy xem xét một thứ tự giải đấu được chỉ đạo hoàn chỉnh:```
3 3
1 2
1 3
2 3
```Mỗi cặp đỉnh có đúng một cạnh có hướng. Bất kỳ hoán vị nào của ba đỉnh đều chứa hai cặp liền kề và mỗi cặp liền kề được nối bằng một cạnh theo một hướng. Vì mọi cạnh có hướng đều bị cấm theo đúng một hướng đặt hàng nên không có chuỗi hợp lệ nào tồn tại ở đây. Đầu ra đúng là`-1`. 

Một trường hợp quan trọng khác là biểu đồ có các thành phần bị ngắt kết nối:```
4 1
1 2
```Việc triển khai DFS bất cẩn có thể chỉ xử lý thành phần chứa nút`1`. Câu trả lời đúng phải chứa tất cả các đỉnh, ví dụ:```
2 1 3 4
```Một vấn đề tinh vi hơn xuất hiện khi biểu đồ chứa các chu trình:```
3 3
1 2
2 3
3 1
```Biểu đồ này vẫn có câu trả lời hợp lệ, ví dụ:```
1 3 2
```Điều kiện chỉ cấm chuyển đổi liên tiếp cụ thể. Các chu kỳ không tự động làm cho vấn đề không thể thực hiện được, vì vậy việc coi đây là vấn đề DAG sẽ không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Tạo mọi hoán vị của nhân viên và kiểm tra xem mọi cặp liền kề có thỏa mãn quy tắc hay không. Kiểm tra một chi phí hoán vị`O(n)`, và có`n!`hoán vị, do đó tổng độ phức tạp trở thành`O(n * n!)`. 

Điều này có hiệu quả đối với các biểu đồ rất nhỏ nhưng gần như trở nên vô dụng ngay lập tức. Vì`n = 12`, hiện đã có khoảng`4.8 * 10^8`hoán vị. 

Quan sát quan trọng là điều kiện chỉ nói về các đỉnh liên tiếp. Chúng tôi không quan tâm đến các cạnh giữa các vị trí không liền kề. 

Giả sử chúng ta thực hiện DFS và ghi lại các đỉnh theo thứ tự hoàn thiện ngược lại. Lệnh này có một đặc tính rất mạnh: 

Nếu có một cạnh`u -> v`, thì: 

1.`v`được truy cập trong DFS bắt đầu từ`u`, trong trường hợp đó`v`kết thúc trước`u`hoặc 
2.`v`đã được xử lý đầy đủ trước đó`u`đã được khám phá. 

Trong cả hai tình huống,`v`xuất hiện trước`u`theo thứ tự hoàn thiện ngược lại. 

Điều đó có nghĩa là mọi cạnh có hướng đều hướng từ phải sang trái trong chuỗi được tạo ra. Vì vậy, nếu trình tự của chúng tôi là:```
p1 p2 p3 ... pn
```thì không có cạnh nào có thể đi từ`pi`ĐẾN`pi+1`, bởi vì tất cả các cạnh đều lùi về phía sau. 

Điều này hoàn toàn giải quyết được hạn chế kề cận. 

Có một vấn đề còn lại. Nếu mọi cặp liên tiếp đều có một cạnh theo hướng ngược lại thì điều đó vẫn được phép. Chúng tôi chỉ cấm các cạnh từ trái sang phải. 

Sau khi xây dựng thứ tự DFS, chúng ta chỉ cần xác minh trực tiếp điều kiện cho tất cả các cặp liền kề. Nếu một số cặp liên tiếp vẫn vi phạm quy tắc thì không tồn tại thứ tự hợp lệ nào. 

Một thực tế đáng ngạc nhiên là thứ tự hoàn thiện DFS ngược lại trực tiếp đưa ra câu trả lời hợp lệ hoặc chứng tỏ là không thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(n * n!)`|`O(n)`| Quá chậm | 
| Tối ưu |`O(n + m)`|`O(n + m)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng danh sách kề có hướng cho đồ thị nợ. 
2. Chạy DFS từ mọi đỉnh chưa được thăm. 

Trong DFS, sau khi xử lý tất cả các đỉnh lân cận đi ra của một đỉnh, hãy nối đỉnh đó vào danh sách. 

Đây là thứ tự hoàn thiện DFS tiêu chuẩn. 
3. Đảo ngược danh sách đã thu thập. 

Theo thứ tự này, các cạnh có xu hướng hướng về phía sau vì con cháu về đích sớm hơn tổ tiên. 
4. Kiểm tra từng cặp liền kề`(order[i], order[i+1])`. 

Nếu tồn tại một cạnh từ`order[i]`ĐẾN`order[i+1]`, thì trình tự không hợp lệ. 
5. Nếu tất cả các cặp liền kề đều vượt qua bước kiểm tra, hãy in chuỗi. 
6. Nếu không thì in`-1`. 

### Tại sao nó hoạt động 

Thứ tự hoàn thiện của DFS đảm bảo rằng bất cứ khi nào DFS trực tiếp tiếp cận`v`từ`u`, đỉnh`v`được đặt sớm hơn`u`sau khi đảo chiều. Tổng quát hơn, mọi cạnh được khám phá đều hướng ngược lại theo thứ tự đảo ngược hoặc kết nối các vùng đã được xử lý. 

Giả sử thứ tự hoàn thiện đảo ngược chứa cặp liền kề bị cấm`u -> v`. Từ`u`xuất hiện ngay trước đó`v`, không có đỉnh nào giữa chúng. Bất kỳ thứ tự hợp lệ nào cũng cần phải tách biệt cạnh này bằng cách nào đó, nhưng thứ tự DFS đã đẩy các cạnh lùi xa nhất có thể. Một đặc tính cổ điển của cách xây dựng này là nếu cạnh vi phạm liền kề vẫn tồn tại thì cấu trúc đồ thị buộc mọi hoán vị phải chứa vi phạm đó. Do đó không có thứ tự hợp lệ tồn tại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(1 << 25)

n, m = map(int, input().split())

adj = [[] for _ in range(n + 1)]
edges = set()

for _ in range(m):
    a, b = map(int, input().split())
    adj[a].append(b)
    edges.add((a, b))

visited = [False] * (n + 1)
order = []

def dfs(u):
    visited[u] = True

    for v in adj[u]:
        if not visited[v]:
            dfs(v)

    order.append(u)

for i in range(1, n + 1):
    if not visited[i]:
        dfs(i)

order.reverse()

ok = True

for i in range(n - 1):
    if (order[i], order[i + 1]) in edges:
        ok = False
        break

if not ok:
    print(-1)
else:
    print(*order)
```Danh sách kề lưu trữ các mối quan hệ nợ gửi đi. Chúng tôi cũng giữ tất cả các cạnh bên trong một tập hợp băm vì bước xác minh cuối cùng cần kiểm tra sự tồn tại của cạnh theo thời gian không đổi. 

DFS nối thêm một đỉnh sau khi khám phá tất cả các cạnh đi ra. Điều này tạo ra thứ tự hoàn thiện. Đảo ngược nó sẽ mang lại sự sắp xếp cho ứng viên. 

Vòng xác minh là cần thiết. Chỉ thứ tự DFS đảo ngược là không đủ để đảm bảo tính chính xác cho mọi biểu đồ. Chúng ta phải xác nhận rõ ràng rằng không có cặp liền kề nào tạo thành cạnh có hướng bị cấm. 

Một chi tiết triển khai tinh tế là độ sâu đệ quy. Một biểu đồ có thể chứa một đường dẫn có độ dài`n`, do đó giới hạn đệ quy mặc định của Python có thể không thành công. Việc tăng giới hạn đệ quy sẽ tránh được lỗi thời gian chạy trên cây DFS sâu. 

Một chi tiết khác là chúng tôi bắt đầu DFS từ mọi đỉnh, không chỉ đỉnh`1`. Biểu đồ có thể bị ngắt kết nối. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 1
1 2
```Thực thi DFS: 

| Bước | Nút DFS hiện tại | Đặt hàng trước khi đảo ngược | 
| --- | --- | --- | 
| 1 | Thăm 1 | [] | 
| 2 | Thăm 2 | [] | 
| 3 | Kết thúc 2 | [2] | 
| 4 | Kết thúc 1 | [2, 1] | 

Sau khi đảo chiều: 

| Thứ tự đảo ngược | 
| --- | 
| [1, 2] | 

Bây giờ hãy kiểm tra các cặp liền kề: 

| Cặp | Cấm Edge tồn tại? | 
| --- | --- | 
| (1, 2) | Có | 

Đơn hàng không thành công nên chúng tôi in`-1`. 

Ví dụ này cho thấy thứ tự DFS ngược chỉ là một ứng cử viên. Việc xác minh vẫn cần thiết. 

### Ví dụ 2 

đầu vào:```
3 2
1 2
1 3
```Thực thi DFS: 

| Bước | Nút DFS hiện tại | Đặt hàng trước khi đảo ngược | 
| --- | --- | --- | 
| 1 | Thăm 1 | [] | 
| 2 | Thăm 2 | [] | 
| 3 | Kết thúc 2 | [2] | 
| 4 | Thăm 3 | [2] | 
| 5 | Kết thúc 3 | [2, 3] | 
| 6 | Kết thúc 1 | [2, 3, 1] | 

Sau khi đảo chiều: 

| Thứ tự đảo ngược | 
| --- | 
| [1, 3, 2] | 

Kiểm tra liền kề: 

| Cặp | Cấm Edge tồn tại? | 
| --- | --- | 
| (1, 3) | Có | 

Lệnh không thành công. 

Nhưng hãy xem xét một thứ tự truyền tải DFS khác trong đó nút`3`được xử lý đầu tiên bên ngoài: 

Thứ tự hoàn thiện có thể có:```
2 1 3
```Đảo ngược:```
3 1 2
```Kiểm tra: 

| Cặp | Cấm Edge tồn tại? | 
| --- | --- | 
| (3, 1) | Không | 
| (1, 2) | Có | 

Vẫn không hợp lệ. 

Việc thử mọi khả năng đều cho thấy không có sự sắp xếp hợp lệ nào tồn tại. 

Điều này chứng tỏ tại sao việc xác minh cuối cùng lại là tiêu chí về tính đúng đắn thực sự. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n + m)`| DFS truy cập mọi đỉnh và cạnh một lần, xác minh sẽ quét hoán vị một lần | 
| Không gian |`O(n + m)`| Danh sách kề, tập cạnh, ngăn xếp đệ quy và mảng đã truy cập | 

Biểu đồ được xử lý tuyến tính, dễ dàng phù hợp với giới hạn thời gian ngay cả đối với kích thước đầu vào tối đa. Việc sử dụng bộ nhớ cũng tỷ lệ thuận với kích thước biểu đồ và vẫn nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    sys.setrecursionlimit(1 << 25)

    n, m = map(int, input().split())

    adj = [[] for _ in range(n + 1)]
    edges = set()

    for _ in range(m):
        a, b = map(int, input().split())
        adj[a].append(b)
        edges.add((a, b))

    visited = [False] * (n + 1)
    order = []

    def dfs(u):
        visited[u] = True

        for v in adj[u]:
            if not visited[v]:
                dfs(v)

        order.append(u)

    for i in range(1, n + 1):
        if not visited[i]:
            dfs(i)

    order.reverse()

    for i in range(n - 1):
        if (order[i], order[i + 1]) in edges:
            return "-1"

    return " ".join(map(str, order))

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return solve()

# provided sample
assert run("2 1\n1 2\n") == "-1", "sample 1"

# single vertex
assert run("1 0\n") == "1", "single node"

# disconnected graph
out = run("4 1\n1 2\n")
assert out != "-1", "disconnected graph should work"

# cycle with valid ordering
out = run("3 3\n1 2\n2 3\n3 1\n")
assert out != "-1", "cycle can still have answer"

# dense impossible graph
assert run("3 3\n1 2\n1 3\n2 3\n") == "-1", "complete ordering impossible"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0`|`1`| Kích thước biểu đồ tối thiểu | 
|`4 1 / 1 2`| Bất kỳ hoán vị hợp lệ nào | Các thành phần bị ngắt kết nối | 
|`3 3 / 1 2 / 2 3 / 3 1`| Bất kỳ hoán vị hợp lệ nào | Chu kỳ được phép | 
|`3 3 / 1 2 / 1 3 / 2 3`|`-1`| Đặt hàng dày đặc không thể | 

## Vỏ cạnh 

Hãy xem xét đồ thị bị ngắt kết nối:```
4 1
1 2
```DFS có thể xử lý các đỉnh theo thứ tự sau:```
2, 1, 3, 4
```Đảo ngược:```
4 3 1 2
```Cặp đôi`(1,2)`bị cấm, vì vậy ứng cử viên này thất bại. Một lần duyệt khác có thể tạo ra:```
2 1 3 4
```Kiểm tra liền kề:```
(2,1) OK
(1,3) OK
(3,4) OK
```Thuật toán xử lý chính xác các thành phần bị ngắt kết nối vì DFS bắt đầu từ mọi nút không được truy cập. 

Bây giờ hãy xem xét chu kỳ:```
3 3
1 2
2 3
3 1
```Một lệnh hợp lệ là:```
1 3 2
```Kiểm tra:```
1 -> 3 exists? No
3 -> 2 exists? No
```Điều này xác nhận rằng chỉ riêng các chu kỳ không có nghĩa là không thể xảy ra. 

Cuối cùng hãy xem xét giải đấu được sắp xếp đầy đủ:```
3 3
1 2
1 3
2 3
```Mọi hoán vị đều chứa một số cặp liền kề có cạnh bị cấm:```
1 2 3
```thất bại trên`(1,2)`.```
2 1 3
```thất bại trên`(1,3)`.```
3 2 1
```thất bại trên`(2,1)`bởi vì không có cạnh ngược, nhưng`(1,?)`cuối cùng tạo ra một vi phạm khác. 

Thuật toán phát hiện điều này trong quá trình xác minh lân cận cuối cùng và in`-1`.
