---
title: "CF 29C - Tem Thư"
description: "Mỗi con tem mô tả sự chuyển giao trực tiếp giữa hai thành phố. Nếu phong bì chứa n tem thì lá thư lần lượt được chuyển qua đúng n + 1 thành phố. Tuyến đường không bao giờ quay lại thành phố nên toàn bộ hành trình tạo thành một con đường đơn giản."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dfs-and-similar", "graphs", "implementation"]
categories: ["algorithms"]
codeforces_contest: 29
codeforces_index: "C"
codeforces_contest_name: "Codeforces Beta Round 29 (Div. 2, Codeforces format)"
rating: 1700
weight: 29
solve_time_s: 97
verified: false
draft: false
---
[CF 29C - Tem Thư](https://codeforces.com/problemset/problem/29/C) 

**Đánh giá:** 1700 
**Thẻ:** cấu trúc dữ liệu, dfs và tương tự, đồ thị, triển khai 
**Thời gian giải:** 1 phút 37s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Mỗi con tem mô tả sự chuyển giao trực tiếp giữa hai thành phố. Nếu phong bì chứa`n`tem, sau đó lá thư được chuyển đi chính xác`n + 1`các thành phố theo thứ tự Tuyến đường không bao giờ quay lại thành phố nên toàn bộ hành trình tạo thành một con đường đơn giản. 

Các con tem không có thứ tự. Chúng tôi chỉ biết cặp thành phố nào liên tiếp trong quá trình phân phối. Nhiệm vụ của chúng ta là xây dựng lại một thứ tự hợp lệ của tất cả các thành phố trên tuyến đường. 

Đây tự nhiên là một vấn đề đồ thị. Mỗi thành phố là một đỉnh, mỗi con tem là một cạnh vô hướng và đồ thị cuối cùng được đảm bảo là một đường đi đơn giản. Một con đường với`n`các cạnh luôn có đúng hai đỉnh bậc`1`, các điểm cuối, trong khi mọi đỉnh trong đều có bậc`2`. 

Các ràng buộc đủ lớn nên việc kiểm tra hoán vị cưỡng bức là không thể. Với`n`lên tới`10^5`, lời giải phải gần với thời gian tuyến tính. Bất kỳ cách tiếp cận nào liên tục quét biểu đồ hoặc thử nhiều tuyến đường ứng cử viên sẽ nhanh chóng trở nên quá chậm. MỘT`O(n)`hoặc`O(n log n)`giải pháp hoàn toàn an toàn. 

Có một vài sai lầm dễ dàng âm thầm phá vỡ quá trình tái thiết. 

Một lỗi phổ biến là bắt đầu truyền tải từ một nút tùy ý thay vì điểm cuối. Coi như:```
3
1 2
2 3
3 4
```Nếu chúng ta bắt đầu từ`2`, chúng ta có thể chuyển đến`1`và bị mắc kẹt, lỡ mất nửa chặng đường. Việc truyền tải chính xác phải bắt đầu từ một mức độ`1`nút, hoặc`1`hoặc`4`. 

Một vấn đề tế nhị khác là giả sử nhãn thành phố là số nguyên nhỏ. Vấn đề cho phép các giá trị lên đến`10^9`.```
2
1000000000 7
7 42
```Việc sử dụng một mảng được lập chỉ mục theo số thành phố sẽ lãng phí một lượng lớn bộ nhớ hoặc gây ra sự cố hoàn toàn. Cần có bản đồ băm. 

Sai lầm thứ ba là quên rằng đồ thị là vô hướng. Ví dụ:```
2
1 100
100 2
```Các tuyến đường hợp lệ là:```
1 100 2
2 100 1
```Việc coi tem là các cạnh được định hướng sẽ từ chối một trong những câu trả lời này một cách không chính xác. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Chúng ta có thể thử mọi thứ tự có thể có của các thành phố và kiểm tra xem các cặp liên tiếp có tương ứng với tem hay không. Vì có`n + 1`thành phố, điều này đòi hỏi phải kiểm tra`(n + 1)!`hoán vị. 

Ngay cả đối với`n = 15`, điều này trở nên to lớn. Vì`n = 10^5`, điều đó là hoàn toàn không thể. 

Lực lượng vũ phu tốt hơn một chút sẽ xây dựng tuyến đường dần dần. Chọn thành phố bắt đầu, liên tục chọn một cạnh liền kề chưa được sử dụng và quay lại bất cứ khi nào đường dẫn không hợp lệ. Điều này hiệu quả vì đồ thị được đảm bảo là một đường dẫn, nhưng nếu không khai thác trực tiếp cấu trúc đó, thuật toán vẫn lãng phí thời gian để khám phá các trạng thái không cần thiết. 

Quan sát quan trọng là biểu đồ đầu vào đã là một đường dẫn đơn giản. Mỗi thành phố nội bộ kết nối với chính xác hai hàng xóm và chỉ có hai điểm cuối kết nối với một hàng xóm. 

Khi chúng tôi xác định được một điểm cuối, việc tái thiết sẽ trở nên đơn giản. Từ thành phố hiện tại, có nhiều nhất một người hàng xóm chưa ghé thăm mà chúng ta có thể chuyển đến tiếp theo. Tuyến đường bị ép buộc. 

Điều này biến vấn đề thành một phép duyệt đồ thị đơn giản trên đồ thị đường dẫn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((n+1)!) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng danh sách kề cho đồ thị. 

Đối với mỗi con tem`(u, v)`, thêm vào`v`vào danh sách hàng xóm của`u`, và thêm`u`vào danh sách hàng xóm của`v`. 
2. Tìm một điểm cuối của đường dẫn. 

Trong một đường đi đơn giản, điểm cuối chính xác là các đỉnh có bậc`1`. Chúng tôi quét tất cả các thành phố và chọn bất kỳ thành phố nào có danh sách lân cận có kích thước`1`. 
3. Bắt đầu di chuyển ngang từ điểm cuối đó. 

Thêm thành phố bắt đầu vào câu trả lời. 
4. Liên tục di chuyển đến người hàng xóm chưa ghé thăm tiếp theo. 

Vì đồ thị là một đường dẫn nên mỗi nút bên trong có chính xác hai nút lân cận. Một là thành phố trước đây chúng tôi đã đến và thành phố còn lại là thành phố tiếp theo trên tuyến đường. 
5. Theo dõi thành phố trước đó. 

Ở mỗi bước, hãy kiểm tra các vùng lân cận của thành phố hiện tại. Di chuyển đến hàng xóm khác với thành phố trước đó. 
6. Dừng lại khi không còn động thái nào nữa. 

Điều này xảy ra ở điểm cuối khác, nơi hàng xóm duy nhất là thành phố trước đó. 

### Tại sao nó hoạt động 

Đồ thị được hình thành bởi các dấu được đảm bảo là một đường đi đơn giản. Một đường đi đơn giản có chính xác hai điểm cuối và không có chu trình. 

Bắt đầu từ điểm cuối đảm bảo rằng mỗi bước tiến đều được xác định duy nhất. Tại bất kỳ đỉnh bên trong nào, một thành phố liền kề là thành phố mà chúng ta đã xuất phát, trong khi thành phố còn lại là thành phố tiếp theo trên tuyến đường. Vì không có thành phố nào lặp lại nên việc chọn hàng xóm không được sử dụng luôn đi theo con đường ban đầu. 

Quá trình duyệt truy cập vào mọi cạnh chính xác một lần và mọi đỉnh đúng một lần, do đó chuỗi được tạo ra là một tuyến đường hợp lệ chứa tất cả các tem. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from collections import defaultdict

def solve():
    n = int(input())

    graph = defaultdict(list)

    for _ in range(n):
        u, v = map(int, input().split())
        graph[u].append(v)
        graph[v].append(u)

    start = -1

    for node in graph:
        if len(graph[node]) == 1:
            start = node
            break

    ans = []

    prev = None
    curr = start

    while True:
        ans.append(curr)

        next_node = None

        for nei in graph[curr]:
            if nei != prev:
                next_node = nei
                break

        if next_node is None:
            break

        prev, curr = curr, next_node

    print(*ans)

solve()
```Danh sách lân cận lưu trữ tất cả các thành phố lân cận cho mỗi thành phố. MỘT`defaultdict(list)`là lý tưởng vì nhãn thành phố có thể lớn bằng`10^9`, vì vậy chúng tôi không thể lập chỉ mục mảng trực tiếp theo số thành phố. 

Quá trình quét nút bắt đầu dựa vào thuộc tính xác định của biểu đồ đường dẫn: điểm cuối có mức độ`1`. Điểm cuối nào cũng được chấp nhận vì tuyến đường ngược lại cũng hợp lệ. 

Quá trình duyệt sử dụng hai biến,`prev`Và`curr`. Điều này tránh cần một bộ đã truy cập. Vì mỗi nút có nhiều nhất là bậc`2`, loại trừ thành phố trước đó sẽ ngay lập tức xác định chính xác thành phố tiếp theo. 

Điều kiện dừng là tinh tế. Tại điểm cuối cuối cùng, mọi hàng xóm đều bằng`prev`, vì vậy không hợp lệ`next_node`tồn tại. Vào thời điểm đó, tuyến đường đã hoàn tất. 

Một lỗi triển khai phổ biến đang cập nhật`prev`Và`curr`theo thứ tự sai. Nhiệm vụ đồng thời:```
prev, curr = curr, next_node
```giữ cả hai giá trị đúng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
1 100
100 2
```Danh sách lân cận:```
1 -> [100]
100 -> [1, 2]
2 -> [100]
```Các điểm cuối là`1`Và`2`. Giả sử chúng ta bắt đầu từ`1`. 

| Bước | trước | hiện tại | next_node | trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | Không có | 1 | 100 | [1] | 
| 2 | 1 | 100 | 2 | [1, 100] | 
| 3 | 100 | 2 | Không có | [1, 100, 2] | 

Đầu ra:```
1 100 2
```Dấu vết này cho thấy lý do tại sao việc truyền tải trở nên xác định sau khi chọn điểm cuối. 

### Ví dụ 2 

đầu vào:```
4
10 20
30 40
20 30
40 50
```Danh sách lân cận:```
10 -> [20]
20 -> [10, 30]
30 -> [40, 20]
40 -> [30, 50]
50 -> [40]
```Điểm cuối là`10`Và`50`. 

| Bước | trước | hiện tại | next_node | trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | Không có | 10 | 20 | [10] | 
| 2 | 10 | 20 | 30 | [10, 20] | 
| 3 | 20 | 30 | 40 | [10, 20, 30] | 
| 4 | 30 | 40 | 50 | [10, 20, 30, 40] | 
| 5 | 40 | 50 | Không có | [10, 20, 30, 40, 50] | 

Đầu ra:```
10 20 30 40 50
```Ví dụ này chứng minh rằng các tem có thể đến theo thứ tự tùy ý, nhưng cấu trúc đồ thị vẫn xác định đường dẫn duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi cạnh và đỉnh được xử lý một số lần không đổi | 
| Không gian | O(n) | Danh sách kề lưu trữ tất cả các cạnh | 

Đồ thị chứa chính xác`n`các cạnh và`n + 1`đỉnh. Mọi thao tác trong thuật toán đều tuyến tính theo kích thước đồ thị, dễ dàng phù hợp với các giới hạn cho`n = 10^5`. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io
from collections import defaultdict

def solve():
    input = sys.stdin.readline

    n = int(input())

    graph = defaultdict(list)

    for _ in range(n):
        u, v = map(int, input().split())
        graph[u].append(v)
        graph[v].append(u)

    start = -1

    for node in graph:
        if len(graph[node]) == 1:
            start = node
            break

    ans = []

    prev = None
    curr = start

    while True:
        ans.append(curr)

        nxt = None

        for nei in graph[curr]:
            if nei != prev:
                nxt = nei
                break

        if nxt is None:
            break

        prev, curr = curr, nxt

    print(*ans)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    return sys.stdout.getvalue().strip()

# provided sample
out = run("2\n1 100\n100 2\n")
assert out in ["1 100 2", "2 100 1"], "sample 1"

# minimum size
out = run("1\n5 9\n")
assert out in ["5 9", "9 5"], "minimum case"

# long chain
out = run("4\n10 20\n30 40\n20 30\n40 50\n")
assert out in ["10 20 30 40 50", "50 40 30 20 10"], "unordered edges"

# large labels
out = run("2\n1000000000 7\n7 42\n")
assert out in [
    "1000000000 7 42",
    "42 7 1000000000"
], "large city ids"

# negative traversal direction
out = run("3\n1 2\n2 3\n3 4\n")
assert out in [
    "1 2 3 4",
    "4 3 2 1"
], "path traversal"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 5 9`|`5 9`hoặc`9 5`| Đồ thị hợp lệ nhỏ nhất | 
| Chuỗi không có thứ tự | Lộ trình đặt hàng đầy đủ | Tái thiết độc lập với thứ tự đầu vào | 
| Nhãn thành phố lớn | Đường dẫn đúng | Xử lý bản đồ băm cho các giá trị thưa thớt | 
| Đường dẫn thẳng 4 nút | Hoặc là hướng | Truyền tải dựa trên điểm cuối | 

## Vỏ cạnh 

Hãy xem xét trường hợp quá trình truyền tải bắt đầu ở giữa thay vì ở điểm cuối. 

đầu vào:```
3
1 2
2 3
3 4
```Đồ thị là:```
1 - 2 - 3 - 4
```Thuật toán của chúng tôi trước tiên sẽ quét độ và tìm thấy một trong hai`1`hoặc`4`. Giả sử nó chọn`1`. 

Truyền tải:```
1 -> 2 -> 3 -> 4
```Tại mỗi nút nội bộ, thành phố trước đó bị loại trừ, chỉ để lại chính xác một thành phố tiếp theo hợp lệ. Thuật toán không bao giờ bị mắc kẹt sớm. 

Bây giờ hãy xem xét các định danh thành phố rất lớn. 

đầu vào:```
2
1000000000 7
7 42
```Cấu trúc liền kề trở thành:```
1000000000 -> [7]
7 -> [1000000000, 42]
42 -> [7]
```Vì việc triển khai sử dụng từ điển thay vì mảng được lập chỉ mục nên bản thân các giá trị thành phố không ảnh hưởng đến việc sử dụng bộ nhớ. 

Cuối cùng, hãy xem xét đường dẫn hợp lệ nhỏ nhất có thể. 

đầu vào:```
1
8 9
```Cả hai thành phố đều có bằng cấp`1`. Bắt đầu từ một trong hai điểm cuối ngay lập tức tạo ra:```
8 9
```hoặc```
9 8
```Quá trình truyền dừng chính xác sau một cạnh vì thành phố thứ hai không còn hàng xóm nào chưa sử dụng.
