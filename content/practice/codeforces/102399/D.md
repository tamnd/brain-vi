---
title: "CF 102399D - \u0414\u043e\u0440\u043e\u0433\u0438 \u0432 \u0441\u0442\u0440\u0430\u043d\u0435"
description: "Chúng tôi có một biểu đồ trọng số có hướng của các thành phố. Thành phố 1 là thủ đô và mọi con đường có hướng u - v đều có tải trọng w, nghĩa là một hàng hóa có trọng lượng tối đa w có thể đi qua con đường đó. Đối với một thành phố cố định s, tuyến đường từ s đến thủ đô có thể sử dụng nhiều con đường."
date: "2026-08-11T23:35:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102399
codeforces_index: "D"
codeforces_contest_name: "2019 \u041c\u043e\u0441\u043a\u043e\u0432\u0441\u043a\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432, \u043b\u0438\u0433\u0430 A"
rating: 0
weight: 102399
solve_time_s: 185
verified: true
draft: false
---

[CF 102399D - \u0414\u043e\u0440\u043e\u0433\u0438 \u0432 \u0441\u0442\u0440\u0430\u043d\u0435](https://codeforces.com/problemset/problem/102399/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3m 5s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một biểu đồ trọng số có hướng của các thành phố. Thành phố`1`là thủ đô, và mọi con đường dẫn lối`u -> v`có năng lực`w`, nghĩa là một hàng hóa có trọng lượng tối đa`w`có thể đi qua con đường đó 

Đối với thành phố cố định`s`, đường đi từ`s`đến thủ đô có thể sử dụng nhiều con đường. Hàng hóa giống nhau phải tồn tại trên mọi con đường trên tuyến, vì vậy trọng lượng hàng hóa tối đa được hỗ trợ bởi tuyến đường cụ thể đó là sức chứa tối thiểu dọc theo tuyến đường đó. Chúng ta cần con đường tốt nhất có thể, vì vậy câu trả lời cho`s`là mức tối đa, trên tất cả các tuyến đường từ`s`ĐẾN`1`, về dung lượng biên tối thiểu trên tuyến đường đó. Nếu không có đường đến thủ đô thì câu trả lời là`-1`. 

Các giới hạn chính thức là`n <= 10^5`Và`m <= 10^6`, với giới hạn thời gian 2 giây và giới hạn bộ nhớ 512 MB. Số lượng đường đủ lớn để một thuật toán có thể thực hiện bất cứ điều gì gần với`O(nm)`là không thể. Thậm chí`O(n^2)`quá lớn khi`n`đạt tới`10^5`. Chúng ta cần xử lý đồ thị gần như tuyến tính hoặc`m log n`thời gian. Công suất biên nhiều nhất là`10^9`, vì vậy số nguyên Python xử lý chúng trực tiếp mà không cần lo lắng về lỗi tràn. 

Hướng của các con đường tạo ra cái bẫy chung đầu tiên. Chúng tôi đang hỏi về những con đường kết thúc tại thành phố`1`, không phải đường dẫn bắt đầu từ đó. Ví dụ:```
2 1
2 1 5
```Câu trả lời là`5`. Nếu chúng tôi vô tình thực hiện một tìm kiếm thông thường từ thành phố`1`trên biểu đồ ban đầu, thành phố`2`không thể tới được vì con đường duy nhất hướng tới`1`. 

Cái bẫy thứ hai là chiếm lợi thế lớn nhất trên tuyến đường thay vì lợi thế nhỏ nhất. Coi như:```
3 2
3 2 10
2 1 3
```Câu trả lời là:```
3
```Con đường năng lực`10`không giúp vượt quá khả năng`3`đường. Năng lực của một tuyến đường là điểm nghẽn của nó. 

Cái bẫy thứ ba là chấp nhận con đường đầu tiên được tìm thấy. Coi như:```
4 4
2 1 2
2 3 8
3 4 7
4 1 7
```Câu trả lời cho thành phố`2`là:```
7
7
-1
```Một DFS ngay lập tức có`2 -> 1`sẽ ghi lại`2`, mặc dù`2 -> 3 -> 4 -> 1`hỗ trợ trọng lượng`7`. Bài toán yêu cầu tìm đường đi tốt nhất chứ không chỉ đơn thuần là đường đi có thể tiếp cận được. 

Cuối cùng, những thành phố không thể tiếp cận vẫn phải tồn tại`-1`. Ví dụ:```
3 1
2 1 4
```Đầu ra là:```
4
-1
```Một khởi tạo bất cẩn như`answer[v] = 0`có thể khiến một thành phố không thể tiếp cận trông như thể có thể vận chuyển một chuyến hàng không trọng lượng, mặc dù câu trả lời bắt buộc là rõ ràng`-1`. 

## Phương pháp tiếp cận 

Giải pháp mạnh mẽ trực tiếp là liệt kê các con đường từ mọi thành phố đến thủ đô. Đối với mỗi đường dẫn, hãy tính toán nút thắt cổ chai bằng cách lấy công suất cạnh tối thiểu, sau đó giữ nút cổ chai lớn nhất được thấy cho thành phố xuất phát đó. Điều này đúng vì mọi tuyến đường có thể đều được kiểm tra. 

Vấn đề là số lượng đường dẫn. Trong một đồ thị có hướng hoàn chỉnh, một DFS liệt kê các đường dẫn đơn giản từ một nguồn có thể gặp phải`P(n-1,1) + P(n-1,2) + ... + P(n-1,n-1)`những con đường khác nhau, nơi`P(a,b) = a! / (a-b)!`. Trên tất cả các thành phố bắt đầu, điều này theo thứ tự`n!`, vượt xa những gì có thể được xử lý`n = 10^5`. Các chu kỳ làm cho việc liệt kê đường dẫn bất cẩn thậm chí còn tồi tệ hơn trừ khi việc xử lý trạng thái đã truy cập được thêm vào. 

Brute-force hoạt động vì câu trả lời được xác định bởi dung lượng cạnh tối thiểu của đường dẫn, nhưng nó không thành công vì nó xử lý riêng từng đường dẫn có thể. Quan sát hữu ích là giá trị của một đường dẫn có thể được tóm tắt bằng một số duy nhất, nút cổ chai của nó. Khi chúng tôi biết điểm thắt cổ chai tốt nhất có thể đạt được từ một thành phố đến thủ đô, việc mở rộng đường đi bằng một con đường nữa chỉ làm thay đổi giá trị đó thông qua một hoạt động tối thiểu. 

Ngoài ra còn có vấn đề về phương hướng. Vì mọi câu trả lời đều mô tả một lộ trình hướng tới thành phố`1`, đảo ngược mọi con đường. Một con đường nguyên bản`u -> v`năng lực`w`trở thành một cạnh đảo ngược`v -> u`với cùng công suất. Bây giờ một tuyến đường từ thủ đô đến`u`trong biểu đồ đảo ngược tương ứng chính xác với tuyến đường từ`u`đến thủ đô trong biểu đồ ban đầu. 

Điều này biến bài toán thành bài toán đường dẫn cổ chai tối đa bắt đầu từ thành phố`1`. Chúng ta có thể giải quyết nó bằng thuật toán tham lam kiểu Dijkstra. Thay vì chọn khoảng cách dự kiến ​​nhỏ nhất, chúng tôi liên tục chọn thành phố chưa được xử lý có nút cổ chai lớn nhất hiện được biết đến. Nếu thành phố hiện tại có giá trị`best[v]`và một cạnh đảo ngược`v -> u`có năng lực`w`, sau đó đạt`u`bởi vì`v`hỗ trợ`min(best[v], w)`. 

Nếu giá trị đó được cải thiện`best[u]`, chúng ta sẽ đẩy giá trị mới vào vùng heap tối đa. Thứ tự tham lam tương tự khiến Dijkstra hoạt động cũng hợp lệ ở đây vì mọi tiện ích mở rộng đều được áp dụng`min`, không bao giờ có thể tăng giá trị của một đường dẫn đã biết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | số mũ,`O(n!)`trong đồ thị dày đặc |`O(n + m)`cộng với trạng thái đệ quy/đường dẫn | Quá chậm | 
| Tối ưu |`O((n + m) log n)`|`O(n + m)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng biểu đồ đảo ngược. Đối với mọi con đường ban đầu`u -> v`với năng lực`w`, lưu trữ một cạnh`v -> u`với cùng công suất. Điều này làm cho việc di chuyển ra khỏi thành phố`1`trong biểu đồ mới tương đương với việc di chuyển về phía thành phố`1`trong biểu đồ gốc. 
2. Tạo`best[v]`, năng lực tắc nghẽn lớn nhất hiện nay được biết đến để tiếp cận thành phố`v`từ thành phố`1`trong đồ thị đảo ngược. Bộ`best[1]`đến vô cùng vì chưa có con đường nào được sử dụng và đặt mọi giá trị khác thành`-1`bởi vì những thành phố đó vẫn chưa tới được. 
3. Đặt`(infinity, 1)`vào hàng đợi có mức độ ưu tiên tối đa. của Python`heapq`là một đống tối thiểu, vì vậy hãy lưu trữ dung lượng dưới dạng âm và sử dụng`(-capacity, vertex)`. 
4. Loại bỏ thành phố có nút thắt cổ chai lớn nhất được biết đến khỏi đống. Nếu mục nhập heap đã cũ vì`best[v]`đã trở nên lớn hơn, hãy loại bỏ nó. Đây là kỹ thuật xóa lười tương tự thường được sử dụng với thuật toán Dijkstra. 
5. Đối với mọi cạnh đảo ngược`v -> u`với năng lực`w`, tính toán nút cổ chai thu được bằng cách đạt`v`và sau đó vượt qua cạnh này:```
candidate = min(best[v], w)
```Nếu như`candidate > best[u]`, cập nhật`best[u]`và đẩy giá trị mới vào heap. Mức tối thiểu là cần thiết vì hàng hóa phải đáp ứng cả đoạn đường đã đi qua và đoạn đường mới. 

1. Tiếp tục cho đến khi heap trống. Tại thời điểm đó, mọi thành phố có thể đến được từ thủ đô trong biểu đồ đảo ngược đều có mức tắc nghẽn tối đa có thể xảy ra. Mọi thành phố vẫn bình đẳng`-1`không thể đạt được vốn trong biểu đồ ban đầu. 
2. Đầu ra`best[2]`,`best[3]`, ...,`best[n]`. Thành phố`1`bản thân nó không được yêu cầu. 

### Tại sao nó hoạt động 

Điều bất biến là bất cứ khi nào một thành phố`v`được giải quyết với nút cổ chai lớn nhất hiện tại thì giá trị của nó đã là tối ưu. Giả sử có một con đường tốt hơn để`v`. Hãy xem xét đỉnh đầu tiên trên đường đi tốt hơn chưa được trích xuất. Tiền thân của nó trên cùng một đường dẫn chắc chắn đã được trích xuất rồi, bởi vì vốn đã được trích xuất trước tiên và đường dẫn tiến dần ra ngoài trong biểu đồ đảo ngược. Khi tiền thân đó được xử lý, thuật toán sẽ chèn một ứng cử viên cho đỉnh tiếp theo bằng với điểm nghẽn của tiền tố của đường dẫn tốt hơn. Ứng cử viên đó ít nhất là nút thắt cổ chai được yêu cầu`v`. Vì hàng đợi luôn trích xuất ứng viên lớn nhất trước tiên,`v`không thể được trích xuất với giá trị nhỏ hơn. Vì vậy mọi giá trị cuối cùng đều tối ưu. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())

    # rev[v] contains (u, w) for every original edge u -> v with weight w.
    rev = [[] for _ in range(n)]

    for _ in range(m):
        u, v, w = map(int, input().split())
        u -= 1
        v -= 1
        rev[v].append((u, w))

    best = [-1] * n
    best[0] = 10**18

    # Python has a min-heap, so store negative bottlenecks.
    pq = [(-best[0], 0)]

    while pq:
        neg_value, v = heapq.heappop(pq)
        value = -neg_value

        # Ignore an outdated heap entry.
        if value != best[v]:
            continue

        for u, w in rev[v]:
            candidate = min(value, w)

            if candidate > best[u]:
                best[u] = candidate
                heapq.heappush(pq, (-candidate, u))

    sys.stdout.write("\n".join(map(str, best[1:])))

if __name__ == "__main__":
    solve()
```Vòng lặp đầu vào chuyển đổi mỗi con đường thành một mục lân cận đảo ngược. Nếu đường ban đầu là`u -> v`, đạt`v`trong biểu đồ đảo ngược cho phép chúng ta chuyển sang`u`, vì vậy việc lưu trữ nó trong`rev[v]`chính xác là sự chuyển đổi cần thiết.`best[0]`được khởi tạo thành`10**18`, lớn hơn một cách an toàn so với mọi dung lượng biên có thể có. Nó cũng có thể được khởi tạo ở mức vô cùng tượng trưng, ​​nhưng việc sử dụng số nguyên sẽ giúp việc tính toán nút cổ chai trở nên đơn giản. Quá trình chuyển đổi đầu tiên sau đó trở thành`min(10**18, w) = w`. 

Hàng đợi ưu tiên chứa các giá trị âm vì`heapq`thực hiện một heap tối thiểu. Do đó, đỉnh có nút cổ chai thực tế lớn nhất có số âm được lưu trữ nhỏ nhất và được trích xuất đầu tiên. 

Một đỉnh có thể vào heap nhiều lần. Ví dụ, nó có thể đạt được trước tiên với dung lượng`3`và sau đó được cải tiến thành`7`. Thay vì tìm kiếm trong đống để xóa mục nhập cũ, mã sẽ để nó ở đó. Khi cái cũ`(3, v)`mục nhập được bật lên,`value != best[v]`, nên nó bị bỏ qua. Điều này giữ cho việc thực hiện đơn giản và duy trì kết quả mong đợi`O((n+m) log n)`ràng buộc. 

Sự so sánh là`candidate > best[u]`, không`>=`. Các giá trị bằng nhau không cải thiện được điều gì và việc tránh các mục nhập heap bằng nhau trùng lặp sẽ hữu ích trên các biểu đồ có nhiều đường có dung lượng giống nhau. 

Không có hoạt động lập chỉ mục dựa trên một bên trong thuật toán biểu đồ. Các thành phố được chuyển đổi từ`1..n`ĐẾN`0..n-1`ngay lập tức, vậy thành phố`1`là đỉnh`0`. Miếng cuối cùng`best[1:]`loại bỏ chính xác vốn từ đầu ra. 

Số nguyên Python không bị tràn và tất cả các dung lượng đều là dương và nhiều nhất là`10^9`. phần bổ sung`10**18`canh gác chỉ được sử dụng cho thủ đô và không bao giờ xuất hiện ở đầu ra. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Những con đường ban đầu tạo thành vòng tròn```
1 -> 2 -> 3 -> 4 -> 1
```với năng lực`3, 6, 2, 1`. Đảo ngược chúng mang lại```
1 -> 4 -> 3 -> 2 -> 1
```Hàng đợi liên quan và`best`các trạng thái là: 

| Thành phố được trích xuất | Giá trị được trích xuất | Thành phố thư thái | Công suất cạnh | Ứng viên | Đã cập nhật`best`| 
| --- | --- | --- | --- | --- | --- | 
| 1 |`∞`| 4 | 1 | 1 |`best[4] = 1`| 
| 4 | 1 | 3 | 2 | 1 |`best[3] = 1`| 
| 3 | 1 | 2 | 6 | 1 |`best[2] = 1`| 
| 2 | 1 | 1 | 3 | 1 | không thay đổi | 

Con đường đầu tiên rời thủ đô trong đồ thị đảo ngược có sức chứa`1`, nên mọi thành phố xa hơn trên tuyến đường này đều thừa hưởng nút cổ chai`1`. Các câu trả lời kết quả là`1, 1, 1`, phù hợp với mẫu 

### Mẫu 2 

Những con đường ban đầu bao gồm`2 -> 3`với năng lực`3`,`3 -> 1`với năng lực`7`,`2 -> 1`với năng lực`2`,`2 -> 4`với năng lực`9`, Và`4 -> 1`với năng lực`1`. 

Sau khi đảo ngược, TP.`1`có thể đạt được`3`với năng lực`7`,`2`với năng lực`2`, Và`4`với năng lực`1`. 

| Thành phố được trích xuất | Giá trị được trích xuất | Thành phố thư thái | Công suất cạnh | Ứng viên | Đã cập nhật`best`| 
| --- | --- | --- | --- | --- | --- | 
| 1 |`∞`| 3 | 7 | 7 |`best[3] = 7`| 
| 1 |`∞`| 2 | 2 | 2 |`best[2] = 2`| 
| 1 |`∞`| 4 | 1 | 1 |`best[4] = 1`| 
| 3 | 7 | 2 | 3 | 3 |`best[2] = 3`| 
| 2 | 3 | không cải thiện | | | không thay đổi | 
| 4 | 1 | không cải thiện | | | không thay đổi | 

Cập nhật quan trọng là thành phố`2`. Tuyến đường trực tiếp đến thủ đô có sức chứa`2`, nhưng đường đi ngược lại`1 -> 3 -> 2`có nút thắt cổ chai`min(7, 3) = 3`. Thành phố trích xuất heap tối đa`3`trước thành phố`2`, cho phép con đường tốt hơn này được khám phá. Đầu ra cuối cùng là`3, 7, 1`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O((n + m) log n)`| Mỗi lần thư giãn thành công sẽ chèn một mục nhập heap và mỗi cạnh được kiểm tra một lần từ danh sách kề kề đảo ngược của nó. | 
| Không gian |`O(n + m)`| Biểu đồ đảo ngược lưu trữ tất cả`m`đường, trong khi`best`và việc sử dụng đống`O(n + m)`thêm không gian trong trường hợp xấu nhất. | 

Với`n = 10^5`Và`m = 10^6`, thuật toán thực hiện quét tuyến tính trên các con đường cộng với các thao tác heap, thay vì liệt kê các đường dẫn. Đây là thang đo phù hợp với giới hạn nhất định là 2 giây và 512 MB. 

## Trường hợp thử nghiệm 

Khai thác kiểm tra sau đây sử dụng cùng một thuật toán trong một cuộc gọi có thể`solve()`chức năng. Trường hợp được tạo lớn có số lượng thành phố và đường tối đa được phép, do đó, nó cũng kiểm tra xem việc triển khai có xử lý được quy mô đầu vào cao hơn hay không.```python
import sys
import io
import heapq

def solve():
    input = sys.stdin.readline
    n, m = map(int, input().split())

    rev = [[] for _ in range(n)]

    for _ in range(m):
        u, v, w = map(int, input().split())
        u -= 1
        v -= 1
        rev[v].append((u, w))

    best = [-1] * n
    best[0] = 10**18

    pq = [(-best[0], 0)]

    while pq:
        neg_value, v = heapq.heappop(pq)
        value = -neg_value

        if value != best[v]:
            continue

        for u, w in rev[v]:
            candidate = min(value, w)
            if candidate > best[u]:
                best[u] = candidate
                heapq.heappush(pq, (-candidate, u))

    sys.stdout.write("\n".join(map(str, best[1:])))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Sample 1
assert run(
    """4 4
1 2 3
2 3 6
3 4 2
4 1 1
"""
) == "1\n1\n1", "sample 1"

# Sample 2
assert run(
    """4 5
2 3 3
3 1 7
2 1 2
2 4 9
4 1 1
"""
) == "3\n7\n1", "sample 2"

# Sample 3
assert run(
    """3 2
2 1 2
1 3 2
"""
) == "2\n-1", "sample 3"

# Minimum-size graph, direct road only.
assert run(
    """2 1
2 1 1000000000
"""
) == "1000000000", "minimum size and maximum weight"

# All capacities equal, including a longer route.
assert run(
    """5 5
2 3 7
3 4 7
4 1 7
5 4 7
2 1 7
"""
) == "7\n7\n7\n7", "all equal capacities"

# Boundary case with a tempting direct route that is worse.
assert run(
    """4 4
2 1 2
2 3 8
3 4 7
4 1 7
"""
) == "7\n7\n7", "better indirect bottleneck"

# Maximum n and maximum m.
# The first 1000 cities form a complete directed graph with weight 5:
# 1000 * 999 = 999000 edges.
# Another 1000 edges connect cities 1001..2000 to the reachable component.
# The remaining cities are unreachable.
n = 100000
parts = [f"{n} 1000000"]

for u in range(1, 1001):
    for v in range(1, 1001):
        if u != v:
            parts.append(f"{u} {v} 5")

for u in range(1001, 2001):
    parts.append(f"{u} {u - 1} 5")

large_input = "\n".join(parts) + "\n"
large_output = run(large_input)

expected_large = "\n".join(
    ["5"] * 1999 + ["-1"] * (n - 2000)
)

assert large_output == expected_large, "maximum-size graph"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 1`, một đường công suất`10^9`|`1000000000`| Kích thước đồ thị tối thiểu và dung lượng cạnh tối đa | 
| Năm thành phố với mọi con đường hữu ích đều có năng lực`7`| Bốn dòng chứa`7`| Năng lực ngang nhau và các tuyến đường dài hơn | 
| Thành phố`2`có năng lực trực tiếp`2`tuyến đường và năng lực gián tiếp`7`tuyến đường |`7`,`7`,`7`| Chọn nút cổ chai tối đa thay vì con đường đầu tiên | 
|`n = 100000`,`m = 1000000`|`5`cho các thành phố`2..2000`, sau đó`-1`| Tỷ lệ đầu vào tối đa, biểu đồ dày đặc và các đỉnh không thể tiếp cận | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên có hướng đảo ngược. Vì```
2 1
2 1 5
```đồ thị đảo ngược chứa`1 -> 2`với năng lực`5`. Thuật toán bắt đầu tại thành phố`1`, thành phố thư giãn`2`với`min(infinity, 5) = 5`và đầu ra`5`. Tìm kiếm biểu đồ gốc từ thành phố`1`sẽ thất bại, đó chính xác là lý do tại sao việc đảo ngược là cần thiết. 

Trường hợp cạnh thứ hai là tính toán nút cổ chai. Vì```
3 2
3 2 10
2 1 3
```đồ thị đảo ngược chứa`1 -> 2`với năng lực`3`Và`2 -> 3`với năng lực`10`. Thành phố`2`nhận được giá trị`3`, rồi thành phố`3`nhận được`min(3, 10) = 3`. Đầu ra là`3`, mặc dù một con đường có thể chở`10`. 

Trường hợp cạnh thứ ba là các tuyến đường cạnh tranh. Vì```
4 4
2 1 2
2 3 8
3 4 7
4 1 7
```biểu đồ đảo ngược bắt đầu với các ứng cử viên`2`cho thành phố`2`Và`7`cho thành phố`4`. Thành phố`4`được trích xuất đầu tiên bởi vì`7 > 2`. Nó nằm cạnh thành phố`3`có năng lực`7`, vậy thành phố`3`nhận được`7`. Thành phố xử lý`3`sau đó cung cấp cho thành phố`2`ứng cử viên`min(7, 8) = 7`, thay thế giá trị trước đó của nó`2`. Kết quả cuối cùng là`7, 7, 7`. 

Trường hợp cạnh thứ tư là một đỉnh không thể chạm tới:```
3 1
2 1 4
```Sau khi đảo ngược, chỉ có thành phố`2`có thể đến được từ thành phố`1`. Heap không bao giờ khám phá thành phố`3`, Vì thế`best[3]`ở lại`-1`. Đầu ra là`4`theo sau là`-1`. Đây là lý do tại sao`-1`là trạng thái ban đầu hữu ích hơn là`0`.
