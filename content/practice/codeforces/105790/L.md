---
title: "CF 105790L - Lango Mocos"
description: "Chúng ta có N loại đá. Một số cặp loại không tương thích, nghĩa là hai loại không thể được bảo quản trong cùng một túi. Đô Đô có đúng hai túi, mỗi loại đá phải được gán cho một trong số chúng."
date: "2026-06-26T08:54:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105790
codeforces_index: "L"
codeforces_contest_name: "UDESC Selection Contest 2024-1"
rating: 0
weight: 105790
solve_time_s: 36
verified: true
draft: false
---

[CF 105790L - Lango Mocos](https://codeforces.com/problemset/problem/105790/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 36s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có`N`các loại đá. Một số cặp loại không tương thích, nghĩa là hai loại không thể được bảo quản trong cùng một túi. Đô Đô có đúng hai túi, mỗi loại đá phải được gán cho một trong số chúng. Một nhiệm vụ hợp lệ là một nhiệm vụ trong đó mỗi cặp độc hại được chia thành các túi khác nhau. 

Đầu vào mô tả một đồ thị vô hướng. Mỗi loại đá là một đỉnh và mọi mối quan hệ độc hại đều là một cạnh. Câu hỏi đặt ra là liệu các đỉnh có thể được chia thành hai nhóm sao cho không có cạnh nào có cả hai điểm cuối trong cùng một nhóm hay không. Nếu có thể, chúng ta phải xuất ra hai nhóm. Ngược lại, chúng tôi báo cáo rằng việc phân tách không thể thực hiện được. 

Các giới hạn này đủ lớn để nghiệm phải tuyến tính. Với tối đa`100000`đỉnh và`100000`các cạnh, một thuật toán liên tục kiểm tra tất cả các cặp sẽ thực hiện xung quanh`10^10`hoạt động trong trường hợp xấu nhất, vượt xa giới hạn một giây cho phép. Chúng ta cần một giải pháp gần`O(N + M)`, trong đó mỗi đỉnh và cạnh chỉ được xử lý một số lần không đổi. 

Các trường hợp cạnh chính đến từ cấu trúc biểu đồ hơn là kích thước đầu vào. Một biểu đồ không có mối quan hệ độc hại là hợp lệ vì mọi hòn đá đều có thể đi vào một trong hai túi. Ví dụ:```
Input:
3 0

Output:
POSSIVEL
3 0
1 2 3
```Việc triển khai bất cẩn chỉ bắt đầu tìm kiếm từ đỉnh`1`sẽ bỏ lỡ các đỉnh bị cô lập trong các thành phần khác. 

Một đỉnh đơn là một trường hợp đặc biệt khác:```
Input:
1 0

Output:
POSSIVEL
1 0
1
```Không có xung đột cần giải quyết nên việc xếp loại duy nhất vào một túi là hợp lệ. Mã giả định cả hai túi phải không trống sẽ không thành công ở đây. 

Tam giác là trường hợp nhỏ nhất không thể xảy ra:```
Input:
3 3
1 2
2 3
3 1

Output:
IMPOSSIVEL
```Việc cố gắng xen kẽ các màu xung quanh chu kỳ sẽ buộc đỉnh đầu tiên và đỉnh cuối cùng nhận được cùng một màu mặc dù chúng được kết nối với nhau. Bất kỳ cách tiếp cận nào chỉ kiểm tra hàng xóm địa phương mà không phát hiện ra mâu thuẫn này sẽ chấp nhận nó một cách không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là thử mọi cách phân chia đá có thể thành hai túi. Vì mỗi hòn đá có hai sự lựa chọn, điều này tạo ra`2^N`các nhiệm vụ có thể. Đối với mỗi nhiệm vụ, chúng tôi có thể kiểm tra từng cặp độc hại và kiểm tra xem cả hai điểm cuối có nằm trong cùng một túi hay không. Phương pháp này đúng vì nó trực tiếp kiểm tra định nghĩa của một phép phân tách hợp lệ, nhưng nó không thể sử dụng được khi`N`đạt tới`100000`. Trường hợp xấu nhất cần phải kiểm tra về`2^100000`bài tập. 

Cấu trúc biểu đồ cung cấp một cách tốt hơn để suy nghĩ về vấn đề. Sự phân tách hai túi hợp lệ chính xác là sự chia đôi của biểu đồ. Theo thuật ngữ lý thuyết đồ thị, mỗi túi là một tập độc lập và một đồ thị vô hướng có thể được chia thành hai tập độc lập khi và chỉ nếu nó là lưỡng cực. 

Một đồ thị lưỡng cực có thể được nhận dạng bằng cách gán một trong hai màu cho mỗi đỉnh sao cho mỗi cạnh kết nối các màu khác nhau. Bắt đầu từ bất kỳ đỉnh nào chưa được thăm, chúng ta có thể tô màu nó và sử dụng phép truyền tải đồ thị để buộc các màu xen kẽ thông qua thành phần được kết nối của nó. Nếu chúng ta tìm thấy một cạnh nối hai đỉnh có cùng màu thì việc phân tách cần thiết là không thể. 

Giải pháp brute-force hoạt động vì nó khám phá mọi màu sắc có thể có. Quan sát cho thấy chỉ có hai màu quan trọng cho phép chúng ta xây dựng màu một cách trực tiếp thay vì tìm kiếm qua tất cả các khả năng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^N * M) | O(N) | Quá chậm | 
| Tối ưu | O(N + M) | O(N + M) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng danh sách kề cho đồ thị. Mỗi cặp độc hại tạo ra một cạnh vô hướng vì sự không tương thích diễn ra theo cả hai hướng. 
2. Giữ một mảng lưu trữ màu của mọi đỉnh. Sử dụng`-1`cho các đỉnh chưa được thăm và sử dụng`0`Và`1`cho hai túi có thể. 
3. Lặp lại qua mọi đỉnh. Nếu một đỉnh chưa được tô màu thì bắt đầu duyệt từ đỉnh đó và gán màu cho nó.`0`. Bước này là cần thiết vì biểu đồ có thể có nhiều thành phần bị ngắt kết nối. 
4. Trong quá trình duyệt, gán cho mỗi đỉnh lân cận một màu đối diện. Nếu một hàng xóm đã có cùng màu với đỉnh hiện tại thì biểu đồ chứa mâu thuẫn và câu trả lời là không thể. 
5. Sau khi tất cả các thành phần đã được xử lý, thu thập các đỉnh có màu`0`vào túi đầu tiên và các đỉnh có màu`1`vào túi thứ hai. Màu sắc đảm bảo rằng mọi cặp độc hại đều được phân chia giữa hai nhóm này. 

Tại sao nó hoạt động: tính bất biến được duy trì trong quá trình truyền tải là mọi cạnh được xử lý đều kết nối các đỉnh có màu khác nhau. Khi chúng ta gán cho hàng xóm không có màu màu đối diện, chúng ta bảo toàn đặc tính này cho cạnh đó. Nếu một hàng xóm đã được tô màu có màu sai thì không thể tồn tại phép gán hai màu hợp lệ vì cạnh đó yêu cầu cả hai màu bằng nhau và khác nhau cùng một lúc. Nếu quá trình duyệt kết thúc mà không có mâu thuẫn thì mọi cạnh đều giao nhau giữa hai nhóm màu nên hai túi hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())

    graph = [[] for _ in range(n)]

    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        graph[u].append(v)
        graph[v].append(u)

    color = [-1] * n
    bag0 = []
    bag1 = []

    for start in range(n):
        if color[start] != -1:
            continue

        color[start] = 0
        stack = [start]

        while stack:
            u = stack.pop()

            for v in graph[u]:
                if color[v] == -1:
                    color[v] = color[u] ^ 1
                    stack.append(v)
                elif color[v] == color[u]:
                    print("IMPOSSIVEL")
                    return

    for i in range(n):
        if color[i] == 0:
            bag0.append(i + 1)
        else:
            bag1.append(i + 1)

    print("POSSIVEL")
    print(len(bag0), len(bag1))
    print(*bag0)
    print(*bag1)

if __name__ == "__main__":
    solve()
```Danh sách kề chỉ lưu trữ các mối quan hệ độc hại hiện có, giúp giữ mức sử dụng bộ nhớ tỷ lệ thuận với kích thước biểu đồ thực tế. Quá trình truyền tải sử dụng một ngăn xếp rõ ràng thay vì đệ quy vì giới hạn đệ quy mặc định của Python quá nhỏ đối với một chuỗi các`100000`đỉnh. 

Vòng lặp bên ngoài trên tất cả các đỉnh xử lý các đồ thị bị ngắt kết nối. Một lỗi phổ biến là chỉ chạy DFS hoặc BFS từ đỉnh`1`, khiến các thành phần khác không được chọn. 

biểu hiện`color[u] ^ 1`lật giữa`0`Và`1`. Nó tránh việc kiểm tra tình trạng bổ sung và thực hiện việc phân công hai túi một cách trực tiếp. 

Bước thu thập cuối cùng chuyển đổi các chỉ mục dựa trên số 0 nội bộ trở lại cách đánh số dựa trên một được sử dụng bởi vấn đề. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 1
1 2
```Quá trình duyệt bắt đầu từ đỉnh`1`. 

| Đỉnh hiện tại | Màu hiện tại | Hàng xóm đã kiểm tra | Màu hàng xóm | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 2 | không màu | Gán màu 1 | 
| 2 | 1 | 1 | 0 | Cạnh hợp lệ | 

Các nhóm cuối cùng là: 

| Túi 0 | Túi 1 | 
| --- | --- | 
| 1 | 2 | 

Cạnh kết nối các màu khác nhau nên có thể tách rời. 

### Ví dụ 2 

đầu vào:```
6 5
1 2
2 3
3 1
4 5
5 6
```Thành phần đầu tiên đã tạo ra sự mâu thuẫn. 

| Đỉnh hiện tại | Màu hiện tại | Hàng xóm | Màu hàng xóm | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 2 | không màu | Gán màu 1 | 
| 1 | 0 | 3 | không màu | Gán màu 1 | 
| 2 | 1 | 3 | 1 | Xung đột | 

Đỉnh`1`,`2`, Và`3`tạo thành một hình tam giác. Việc truyền tải phát hiện các đỉnh đó`2`Và`3`sẽ cần các màu khác nhau vì chúng liền kề nhau, nhưng cả hai đều buộc phải tô màu`1`. Câu trả lời đúng là`IMPOSSIVEL`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N + M) | Mỗi đỉnh đi qua một lần và mọi cạnh được kiểm tra từ cả hai điểm cuối. | 
| Không gian | O(N + M) | Danh sách kề lưu trữ tất cả các cạnh và mảng lưu trữ màu sắc và dữ liệu truyền tải. | 

Những ràng buộc cho phép`100000`các đỉnh và các cạnh, do đó, việc duyệt đồ thị tuyến tính vừa vặn thoải mái trong giới hạn yêu cầu. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return result

assert run("""2 1
1 2
""") == """POSSIVEL
1 1
1
2
""", "sample 1"

assert run("""1 0
""") == """POSSIVEL
1 0
1

""", "sample 2"

assert run("""3 3
1 2
2 3
3 1
""") == """IMPOSSIVEL
""", "odd cycle"

assert run("""5 4
1 2
1 3
1 4
3 5
""") == """POSSIVEL
2 3
1 5
2 3 4
""", "tree shaped bipartite graph"

assert run("""4 0
""") == """POSSIVEL
4 0
1 2 3 4

""", "empty graph"

assert run("""4 4
1 2
2 3
3 4
4 1
""") == """POSSIVEL
2 2
1 3
2 4
""", "even cycle"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0`| Một chiếc túi có đỉnh duy nhất | Xử lý kích thước tối thiểu | 
|`4 0`| Tất cả các đỉnh có thể chia sẻ một túi | Xử lý đồ thị trống | 
| Đồ thị tam giác |`IMPOSSIVEL`| Phát hiện chu kỳ lẻ | 
| Đồ thị bốn chu kỳ | Hai nhóm xen kẽ | Đúng màu lưỡng cực | 
| Biểu đồ mẫu có nhánh | Phân vùng hợp lệ | Hành vi truyền tải chung | 

## Vỏ cạnh 

Đối với trường hợp đỉnh bị cô lập:```
Input:
1 0
```Thuật toán thăm đỉnh`1`, gán màu`0`, và kết thúc ngay lập tức vì không có hàng xóm nào. Bộ sưu tập cuối cùng đặt đỉnh`1`vào túi thứ nhất và để trống túi thứ hai. Đầu ra vẫn hợp lệ vì không có cặp độc hại. 

Đối với đồ thị bị ngắt kết nối:```
Input:
4 1
1 2
```Các đỉnh màu duyệt đầu tiên`1`Và`2`. Đỉnh`3`Và`4`không bao giờ đạt được từ thành phần đó, vì vậy vòng lặp bên ngoài bắt đầu các quá trình truyền tải mới cho chúng. Chúng nhận màu một cách độc lập và phân vùng cuối cùng bao gồm mọi đỉnh. 

Đối với một chu kỳ lẻ không thể:```
Input:
3 3
1 2
2 3
3 1
```Thuật toán tô màu đỉnh`1`BẰNG`0`, buộc các đỉnh`2`Và`3`trở thành`1`. Khi nó kiểm tra cạnh giữa`2`Và`3`, cả hai điểm cuối đều có cùng màu. Vì mọi cạnh phải kết nối các túi đối diện nhau nên không có phép gán hợp lệ nào tồn tại và thuật toán sẽ in chính xác`IMPOSSIVEL`.
