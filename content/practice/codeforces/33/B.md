---
title: "CF 33B - Vấn đề về chuỗi"
description: "Chúng ta có hai chuỗi chữ thường có cùng độ dài. Chúng tôi có thể biến đổi nhiều lần các ký tự bằng cách sử dụng các quy tắc chuyển đổi có hướng dẫn. Quy tắc như a -> b với giá 5 có nghĩa là chúng ta có thể thay đổi một lần xuất hiện của a thành b bằng cách trả 5."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 33
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 33 (Codeforces format)"
rating: 1800
weight: 33
solve_time_s: 421
verified: true
draft: false
---
[CF 33B - Sự cố về chuỗi](https://codeforces.com/problemset/problem/33/B) 

**Đánh giá:** 1800 
**Thẻ:** con đường ngắn nhất 
**Thời gian giải:** 7 phút 1 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai chuỗi chữ thường có cùng độ dài. Chúng tôi có thể biến đổi nhiều lần các ký tự bằng cách sử dụng các quy tắc chuyển đổi có hướng dẫn. Một quy tắc như`a -> b`với chi phí`5`có nghĩa là chúng ta có thể thay đổi một lần xuất hiện của`a`vào trong`b`bằng cách trả tiền`5`. 

Mục tiêu không phải là chuyển đổi toàn bộ chuỗi này thành chuỗi khác một cách trực tiếp. Thay vào đó, đối với mọi vị trí, chúng tôi có thể thay đổi cả hai ký tự thành ký tự thứ ba nào đó nếu điều đó rẻ hơn. Sau tất cả các phép biến đổi, hai chuỗi kết quả phải giống hệt nhau và chúng tôi muốn tổng chi phí tối thiểu. 

Quan sát quan trọng là mọi vị trí đều độc lập. Nếu tại một vị trí chúng ta có các ký tự`x`Và`y`, chúng ta chỉ cần quyết định cả hai sẽ trở thành nhân vật cuối cùng nào. Tổng câu trả lời là tổng chi phí tối ưu trên tất cả các vị trí. 

Các chuỗi có thể có độ dài lên tới`10^5`, có nghĩa là bất kỳ thuật toán cho mỗi vị trí nào cũng phải cực kỳ nhỏ, thời gian lý tưởng là không đổi. Bảng chữ cái chỉ chứa các chữ cái tiếng Anh viết thường nên chỉ có`26`các ký tự có thể. Điều đó thay đổi hoàn toàn hình dạng của vấn đề. Mặc dù các chuỗi rất lớn nhưng đồ thị biến đổi lại rất nhỏ. 

Số lượng quy tắc chuyển đổi nhiều nhất là`500`. Chuyển đổi trực tiếp có thể không tối ưu vì các quy tắc chuỗi có thể rẻ hơn. Ví dụ:```
a -> b : 10
a -> c : 2
c -> b : 2
```Cách rẻ nhất từ`a`ĐẾN`b`thực sự là chi phí`4`. 

Điều này ngay lập tức gợi ý các đường đi ngắn nhất trên biểu đồ`26`nút. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Một trường hợp quan trọng là khi không tồn tại ký tự mục tiêu chung. 

Ví dụ:```
ab
cd
1
a c 5
```Không có cách nào để làm`b`Và`d`bằng mọi thứ, nên câu trả lời đúng là:```
-1
```Một giải pháp bất cẩn chỉ kiểm tra các phép biến đổi trực tiếp có thể bỏ sót điều không thể thực hiện được ở một số vị trí. 

Một trường hợp tinh tế khác là khi chuyển đổi cả hai ký tự thành ký tự thứ ba sẽ rẻ hơn so với chuyển đổi ký tự này sang ký tự kia. 

Ví dụ:```
ab
bc
4
a d 1
b d 1
b c 100
```Ở vị trí thứ hai, chuyển đổi`b`vào trong`c`chi phí`100`, nhưng cả hai đều có thể trở thành`d`cho tổng chi phí`2`. Chuỗi kết quả tối ưu không nhất thiết phải là một trong các chuỗi gốc. 

Các quy tắc lặp đi lặp lại là một cái bẫy khác. 

Ví dụ:```
a
b
2
a b 10
a b 3
```Biểu đồ phải giữ chi phí cạnh tối thiểu, nếu không chúng ta sẽ sử dụng không chính xác`10`thay vì`3`. 

Cuối cùng, các ký tự có thể đã khớp. Những vị trí đó sẽ đóng góp chi phí bằng 0 ngay cả khi có chu kỳ tồn tại trong biểu đồ. 

Ví dụ:```
aaa
aaa
1
a b 5
```Câu trả lời vẫn là con số không. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ xử lý từng vị trí một cách riêng biệt và thử tất cả các chuỗi biến đổi có thể có. Đối với một cặp nhân vật`(x, y)`, chúng ta có thể tìm kiếm qua tất cả các đường dẫn từ`x`Và`y`cho mọi nhân vật khác. Vì các phép biến đổi có thể xâu chuỗi tùy ý nên điều này nhanh chóng trở thành vấn đề tìm kiếm đồ thị. 

Một cách triển khai đơn giản sẽ chạy Dijkstra hoặc BFS từ đầu cho mọi vị trí và mọi nhân vật mục tiêu ứng cử viên. Với những chuỗi có độ dài`10^5`, thậm chí là khiêm tốn`O(26^2)`tính toán đồ thị cho mỗi vị trí trở nên tốn kém không cần thiết. 

Cấu trúc của vấn đề mang lại cơ hội mạnh mẽ hơn nhiều. Đồ thị chỉ có`26`đỉnh. Điều đó có nghĩa là chúng ta có thể tính toán trước chi phí rẻ nhất giữa mỗi cặp ký tự một lần, sau đó trả lời từng vị trí chuỗi trong thời gian không đổi. 

Các quy tắc chuyển đổi tạo thành một biểu đồ có trọng số có hướng:```
character = node
transformation = directed edge
cost = edge weight
```Nếu chúng ta biết khoảng cách đường đi ngắn nhất giữa mỗi cặp chữ cái thì với một vị trí`(s[i], t[i])`, chúng tôi chỉ đơn giản thử mọi ký tự cuối cùng có thể`c`. 

Tổng chi phí cho việc lựa chọn`c`là:```
dist[s[i]][c] + dist[t[i]][c]
```Chúng tôi chọn mức tối thiểu trên tất cả`26`sự lựa chọn. 

Bởi vì đồ thị rất nhỏ nên Floyd-Warshall hoàn hảo ở đây. Độ phức tạp của nó là`O(26^3)`, đó là hằng số thực tế. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n × 26 × tìm kiếm đồ thị) | O(26 + cạnh) | Quá chậm/không cần thiết | 
| Tối ưu | O(26^3 + 26 × n) | O(26^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo một`26 × 26`ma trận khoảng cách được khởi tạo ở mức vô cùng.`dist[i][j]`sẽ đại diện cho chi phí tối thiểu để biến đổi ký tự`i`thành nhân vật`j`. 
2. Đặt`dist[i][i] = 0`cho mỗi nhân vật. 

Một nhân vật luôn có thể không thay đổi với chi phí bằng 0. 
3. Đọc từng quy tắc chuyển đổi`(a, b, w)`và cập nhật:```
dist[a][b] = min(dist[a][b], w)
```Nhiều quy tắc có thể tồn tại giữa cùng một cặp, vì vậy chúng tôi chỉ giữ lại cạnh trực tiếp rẻ nhất. 
4. Chạy Floyd-Warshall trên`26`các chữ cái. 

Đối với mỗi ký tự trung gian`k`, hãy thử cải thiện từng cặp`(i, j)`sử dụng:```
dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j])
```Điều này tính toán chuyển đổi chuỗi rẻ nhất có thể. 
5. Nếu hai chuỗi có độ dài khác nhau, hãy in`-1`. 

Mọi thao tác chỉ thay đổi ký tự, không bao giờ thay đổi độ dài chuỗi. 
6. Xử lý vị trí chuỗi theo vị trí. 

Giả sử các ký tự hiện tại là`x`Và`y`. 
7. Hãy thử mọi ký tự cuối cùng có thể`c`từ`'a'`ĐẾN`'z'`. 

Tính toán:```
dist[x][c] + dist[y][c]
```Giữ nhân vật với tổng chi phí tối thiểu. 
8. Nếu không có ký tự hợp lệ cho một số vị trí, hãy in`-1`. 

Điều đó có nghĩa là không có nhân vật nào có thể truy cập được từ cả hai phía. 
9. Nếu không, hãy thêm ký tự đã chọn vào chuỗi câu trả lời và cộng giá trị của nó vào tổng số. 
10. Sau khi xử lý tất cả các vị trí, in tổng chi phí và chuỗi kết quả. 

### Tại sao nó hoạt động 

Floyd-Warshall đảm bảo rằng sau khi tiền xử lý,`dist[a][b]`bằng chi phí tối thiểu có thể để chuyển đổi`a`vào trong`b`sử dụng bất kỳ chuỗi quy tắc nào. 

Đối với mỗi vị trí chuỗi, ký tự cuối cùng độc lập với tất cả các vị trí khác. Bất kỳ giải pháp hợp lệ nào cũng phải chọn một số ký tự mục tiêu`c`cho vị trí đó. Cách rẻ nhất để đạt được sự lựa chọn đó chính xác là:```
dist[s[i]][c] + dist[t[i]][c]
```Vì thuật toán kiểm tra tất cả`26`các ký tự mục tiêu có thể có và chọn mức tối thiểu, nó sẽ tìm ra lựa chọn tối ưu toàn cục cho vị trí đó. Tổng các chi phí tối ưu độc lập này trên tất cả các vị trí sẽ cho tổng chi phí tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10**18

def solve():
    s = input().strip()
    t = input().strip()

    if len(s) != len(t):
        print(-1)
        return

    n = int(input())

    dist = [[INF] * 26 for _ in range(26)]

    for i in range(26):
        dist[i][i] = 0

    for _ in range(n):
        a, b, w = input().split()
        w = int(w)

        u = ord(a) - ord('a')
        v = ord(b) - ord('a')

        dist[u][v] = min(dist[u][v], w)

    for k in range(26):
        for i in range(26):
            for j in range(26):
                if dist[i][k] == INF or dist[k][j] == INF:
                    continue

                nd = dist[i][k] + dist[k][j]

                if nd < dist[i][j]:
                    dist[i][j] = nd

    total_cost = 0
    result = []

    for cs, ct in zip(s, t):
        u = ord(cs) - ord('a')
        v = ord(ct) - ord('a')

        best_cost = INF
        best_char = ''

        for c in range(26):
            if dist[u][c] == INF or dist[v][c] == INF:
                continue

            cur = dist[u][c] + dist[v][c]

            if cur < best_cost:
                best_cost = cur
                best_char = chr(c + ord('a'))

        if best_cost == INF:
            print(-1)
            return

        total_cost += best_cost
        result.append(best_char)

    print(total_cost)
    print(''.join(result))

solve()
```Phần đầu tiên xây dựng biểu đồ. Ma trận bắt đầu bằng vô cùng vì hầu hết các phép biến đổi ban đầu đều không thể thực hiện được. Việc đặt đường chéo thành 0 là điều cần thiết, nếu không các ký tự giống hệt nhau sẽ yêu cầu chuyển đổi không chính xác. 

Khi đọc các cạnh, mã sử dụng`min()`vì đầu vào có thể chứa các quy tắc trùng lặp. Thiếu chi tiết này âm thầm tạo ra các đường dẫn ngắn nhất sai. 

Pha Floyd-Warshall rất nhỏ vì kích thước đồ thị được cố định ở`26`. Việc kiểm tra chống lại`INF`tránh những phép cộng vô nghĩa như vô cùng cộng với một số. 

Vòng lặp cuối cùng xử lý các vị trí một cách độc lập. Đối với mỗi cặp ký tự, mã sẽ thử tất cả`26`có thể gặp gỡ các nhân vật và chọn nhân vật rẻ nhất có thể tiếp cận được. 

Một chi tiết triển khai tinh tế là ký tự kết quả không cần phải bằng ký tự gốc. Thuật toán xử lý việc này một cách tự nhiên vì nó tìm kiếm toàn bộ bảng chữ cái. 

Một chi tiết quan trọng khác là việc sử dụng`10**18`cho vô cùng. Mặc dù chi phí thực tế là nhỏ nhưng việc sử dụng số nguyên rất lớn sẽ ngăn ngừa lỗi logic kiểu tràn khi cộng khoảng cách. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
uayd
uxxd
3
a x 8
x y 13
d c 3
```Sau Floyd-Warshall, khoảng cách hữu ích là:```
a -> x = 8
x -> y = 13
a -> y = 21
d -> c = 3
```Bây giờ xử lý các vị trí. 

| Vị trí | s[i] | t[i] | Mục tiêu tốt nhất | Chi phí | 
| --- | --- | --- | --- | --- | 
| 0 | bạn | bạn | bạn | 0 | 
| 1 | một | x | x | 8 | 
| 2 | y | x | y | 13 | 
| 3 | d | d | d | 0 | 

Tổng chi phí trở thành`21`, và chuỗi kết quả là:```
uxyd
```Ví dụ này chứng minh tại sao đường đi ngắn nhất lại quan trọng. Sự chuyển đổi`a -> y`chỉ tồn tại thông qua`x`. 

### Ví dụ 2 

đầu vào:```
ab
bc
4
a d 1
b d 1
b c 100
c d 1
```Vị trí xử lý: 

| Vị trí | s[i] | t[i] | Mục tiêu ứng viên | Tổng chi phí | 
| --- | --- | --- | --- | --- | 
| 0 | một | b | d | 2 | 
| 1 | b | c | d | 2 | 

Kết quả:```
4
dd
```Vị trí thứ hai là vị trí thú vị. Chuyển đổi trực tiếp`b -> c`chi phí`100`, nhưng chuyển cả hai sang`d`chỉ chi phí`2`. 

Điều này xác nhận rằng ký tự mục tiêu tối ưu có thể khác hoàn toàn so với cả hai bản gốc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(26^3 + 26 × n) | Floyd-Warshall trên 26 chữ cái, sau đó 26 kiểm tra cho mỗi vị trí chuỗi | 
| Không gian | O(26^2) | Ma trận khoảng cách |`26^3`chỉ là`17576`hoạt động, về cơ bản là không đổi. Công việc chủ yếu là quét các chuỗi có chiều dài tuyến tính. Với chuỗi lên đến`10^5`, điều này thoải mái phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

INF = 10**18

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    s = input().strip()
    t = input().strip()

    if len(s) != len(t):
        return "-1"

    n = int(input())

    dist = [[INF] * 26 for _ in range(26)]

    for i in range(26):
        dist[i][i] = 0

    for _ in range(n):
        a, b, w = input().split()
        w = int(w)

        u = ord(a) - ord('a')
        v = ord(b) - ord('a')

        dist[u][v] = min(dist[u][v], w)

    for k in range(26):
        for i in range(26):
            for j in range(26):
                if dist[i][k] == INF or dist[k][j] == INF:
                    continue

                dist[i][j] = min(
                    dist[i][j],
                    dist[i][k] + dist[k][j]
                )

    total = 0
    ans = []

    for a, b in zip(s, t):
        u = ord(a) - ord('a')
        v = ord(b) - ord('a')

        best = INF
        ch = ''

        for c in range(26):
            if dist[u][c] == INF or dist[v][c] == INF:
                continue

            cur = dist[u][c] + dist[v][c]

            if cur < best:
                best = cur
                ch = chr(c + ord('a'))

        if best == INF:
            return "-1"

        total += best
        ans.append(ch)

    return f"{total}\n{''.join(ans)}"

# provided sample
assert run(
"""uayd
uxxd
3
a x 8
x y 13
d c 3
"""
) == "21\nuxyd", "sample 1"

# already equal
assert run(
"""aaa
aaa
1
a b 5
"""
) == "0\naaa", "equal strings"

# impossible transformation
assert run(
"""ab
cd
1
a c 5
"""
) == "-1", "impossible case"

# cheaper through intermediate
assert run(
"""a
c
2
a b 1
b c 1
"""
) == "2\nc", "shortest path chaining"

# duplicate edges
assert run(
"""a
b
2
a b 10
a b 3
"""
) == "3\nb", "minimum duplicate edge"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Chuỗi bằng nhau |`0`chi phí | Khởi tạo đường chéo | 
| Chuyển đổi không thể |`-1`| Phát hiện trạng thái không thể truy cập | 
| Con đường gián tiếp rẻ hơn | Sử dụng nút trung gian | Floyd-Warshall đúng đắn | 
| Các cạnh trùng lặp | Sử dụng cạnh tối thiểu | Xây dựng đồ thị phù hợp | 

## Vỏ cạnh 

Xét trường hợp biến đổi không thể:```
ab
cd
1
a c 5
```Đồ thị chỉ chứa một cạnh:```
a -> c
```Tại vị trí`0`, ký tự`a`Và`c`cả hai đều có thể trở thành`c`, vậy vị trí đó là hợp lệ. 

Tại vị trí`1`, ký tự`b`Và`d`không thể đạt tới bất kỳ ký tự chung nào. Trong thời gian`26`-quét ký tự, mọi ứng viên đều không thể truy cập được, vì vậy`best_cost`vẫn là vô hạn. Thuật toán in chính xác:```
-1
```Bây giờ hãy xem xét các cạnh trùng lặp:```
a
b
2
a b 10
a b 3
```Trong khi đọc dữ liệu đầu vào, thuật toán sẽ lưu trữ:```
dist[a][b] = min(10, 3) = 3
```Nếu không có bước này, Floyd-Warshall sẽ bắt đầu từ chi phí cạnh trực tiếp sai và đưa ra câu trả lời sai. 

Cuối cùng, hãy xem xét trường hợp ký tự thứ ba là tối ưu:```
ab
bc
4
a d 1
b d 1
c d 1
b c 100
```Ở vị trí thứ hai:```
b -> c = 100
b -> d = 1
c -> d = 1
```Thuật toán thử tất cả các ký tự đích và tính toán:```
target c : 100
target d : 2
```Thế là nó chọn đúng`d`. Một cách tiếp cận tham lam chỉ chuyển đổi ký tự này thành ký tự khác sẽ thất bại ở đây.
