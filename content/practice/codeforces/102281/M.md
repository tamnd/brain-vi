---
title: "CF 102281M - \u0410\u043d\u0442\u0438\u043d\u0430\u0443\u0447\u043d\u0430\u044f \u0437\u0430\u0434\u0430\u0447\u0430"
description: "Các lỗ sâu tạo thành một đồ thị có hướng. Mỗi quá trình chuyển đổi đã biết sẽ đi từ lỗ sâu đục này sang lỗ sâu đục khác và có một trong hai chi phí. Một quá trình siêu chuyển đổi tốn một ant giờ, trong khi một quá trình chuyển đổi rỗng có giá bằng 0. Con tàu xuất phát từ hố sâu số 1 và phải tới hố sâu n."
date: "2026-08-13T09:32:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102281
codeforces_index: "M"
codeforces_contest_name: "2011, IV \u0421\u0430\u043c\u0430\u0440\u0441\u043a\u0430\u044f \u043e\u0431\u043b\u0430\u0441\u0442\u043d\u0430\u044f \u043c\u0435\u0436\u0432\u0443\u0437\u043e\u0432\u0441\u043a\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e"
rating: 0
weight: 102281
solve_time_s: 74
verified: true
draft: false
---

[CF 102281M - \u0410\u043d\u0442\u0438\u043d\u0430\u0443\u0447\u043d\u0430\u044f \u0437\u0430\u0434\u0430\u0447\u0430](https://codeforces.com/problemset/problem/102281/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Các lỗ sâu tạo thành một đồ thị có hướng. Mỗi quá trình chuyển đổi đã biết sẽ đi từ lỗ sâu đục này sang lỗ sâu đục khác và có một trong hai chi phí. Một quá trình siêu chuyển đổi tốn một ant giờ, trong khi một quá trình chuyển đổi rỗng có giá bằng 0. Con tàu xuất phát từ hố sâu số 1 và phải tới hố sâu n. 

Chúng ta cần một tuyến đường với tổng chi phí tối thiểu. Có một hạn chế bổ sung: một tuyến đường không thể truy cập cùng một lỗ sâu đục hai lần. Đầu ra phải chứa số lượng kiến ​​giờ tối thiểu, theo sau là số lượng lỗ sâu trong tuyến và chính tuyến đó. Nếu wormhole n không thể truy cập được từ wormhole 1, thì kết quả đầu ra là`IMPOSSIBLE`. 

Biểu đồ có thể chứa tới 100.000 lỗ sâu đục và 100.000 chuyển tiếp có hướng. Với kích thước đó, việc liệt kê các tuyến đường có thể là hoàn toàn không khả thi. Ngay cả các thuật toán kiểm tra tất cả các cặp đỉnh, chẳng hạn như Floyd-Warshall với thời gian (O(n^3)), cũng vượt xa giới hạn. Chúng ta cần một thuật toán đồ thị có thời gian chạy về cơ bản là tuyến tính theo số đỉnh và chuyển tiếp. Thực tế là mọi chuyển đổi đều có giá bằng 0 hoặc bằng 1 là đặc tính cấu trúc mang lại cho chúng ta một thuật toán như vậy. 

Có một số trường hợp đặc biệt có thể khiến việc triển khai bất cẩn không thành công. 

Hãy xem xét đồ thị nhỏ nhất có thể:```
1 0
```Con tàu đã tới đích rồi. Con đường chính xác là lỗ sâu đục duy nhất`1`, với chi phí bằng 0:```
0 1
1
```Việc triển khai luôn mong đợi đi qua ít nhất một cạnh có thể báo cáo không chính xác`IMPOSSIBLE`hoặc tạo ra một tuyến đường trống. 

Trường hợp thứ hai là một đích đến không thể truy cập được:```
2 0
```Không có sự chuyển đổi từ lỗ sâu 1 sang lỗ sâu 2, vì vậy kết quả đầu ra đúng duy nhất là:```
IMPOSSIBLE
```Việc triển khai tìm kiếm phải phân biệt một đỉnh chưa được chạm tới với một đỉnh có khoảng cách bằng 0. 

Chu kỳ chi phí bằng 0 là một trường hợp tế nhị khác:```
3 3
1 2 0
2 1 0
2 3 1
```Lộ trình tối ưu là`1 2 3`, với chi phí một. chu kỳ`1 -> 2 -> 1`không đóng góp thời gian nhưng vẫn bị cấm vì lỗ sâu đục 1 sẽ được viếng thăm hai lần. Thuật toán khoảng cách ngắn nhất có thể gặp phải một chu trình như vậy trong nội bộ, nhưng đường đi ngắn nhất được xây dựng lại không được chứa chu trình đó. 

Cuối cùng, một số tuyến đường ngắn nhất có thể có cùng chi phí. Ví dụ:```
4 4
1 2 1
1 3 1
2 4 0
3 4 0
```Cả hai`1 2 4`Và`1 3 4`có chi phí một. Vấn đề chấp nhận một trong hai. Một giải pháp không được dựa vào một con đường ngắn nhất cụ thể hiện có. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là liệt kê các tuyến đường từ lỗ sâu đục 1, theo dõi các lỗ sâu đục đã ghé thăm sao cho một đỉnh không thể xuất hiện hai lần. Bất cứ khi nào đến đích, chúng tôi có thể so sánh chi phí của tuyến đường với câu trả lời tốt nhất được tìm thấy cho đến nay. 

Điều này đúng vì mọi con đường hợp pháp đều được xem xét rõ ràng. Vấn đề là số lượng tuyến đường. Một đồ thị có nhiều chuyển tiếp phân nhánh có thể có nhiều đường đi đơn giản theo cấp số nhân. Trong một biểu đồ đủ dày đặc, số lượng đường đi đơn giản có thể vào thứ tự (n!), do đó, ngay cả khi chỉ có 100.000 đỉnh, việc tìm kiếm sẽ không thể thực hiện được rất lâu trước khi đầu vào đạt kích thước tối đa. Việc hạn chế đối với các đỉnh lặp lại làm cho việc liệt kê lực lượng thậm chí còn phức tạp hơn, bởi vì trạng thái không chỉ là đỉnh hiện tại mà còn là tập hợp đầy đủ các đỉnh đã truy cập trước đó. 

Cách tiếp cận bạo lực có hiệu quả vì nó trực tiếp thể hiện định nghĩa về con đường hợp pháp, nhưng nó thất bại vì có quá nhiều con đường để kiểm tra. 

Quan sát quan trọng là hạn chế về các lỗ sâu lặp đi lặp lại không thực sự yêu cầu một thuật toán đường đi ngắn nhất đặc biệt. Tất cả chi phí chuyển đổi đều không âm, vì vậy nếu một bước đi chứa một đỉnh lặp lại thì phần giữa hai lần xuất hiện liên tiếp của đỉnh đó là một chu trình. Việc loại bỏ chu trình đó không thể làm tăng tổng chi phí. Nếu chu trình có chi phí dương thì lộ trình sẽ trở nên rẻ hơn. Nếu nó có chi phí bằng 0 thì tuyến đường đó sẽ giữ nguyên chi phí. Việc lặp lại quá trình này sẽ loại bỏ mọi đỉnh lặp lại và tạo ra một đường đi đơn giản với chi phí không lớn hơn. 

Do đó, luôn có một lộ trình tối ưu và đơn giản. Chúng ta có thể giải bài toán đường đi ngắn nhất thông thường và xây dựng lại một đường đi ngắn nhất. Trọng số cạnh đặc biệt, được giới hạn ở mức 0 và 1, cho phép chúng tôi sử dụng BFS 0-1. 

0-1 BFS về cơ bản là thuật toán Dijkstra chuyên dùng cho trọng số 0 và 1. Thay vì hàng đợi ưu tiên, nó sử dụng deque. Khi đi qua một cạnh có chi phí bằng 0 sẽ cải thiện khoảng cách, đỉnh mới được đặt ở phía trước vì khoảng cách của nó giống với đỉnh hiện tại. Khi đi qua cạnh một chi phí sẽ cải thiện khoảng cách, đỉnh mới sẽ lùi về phía sau vì khoảng cách của nó chính xác là lớn hơn một. 

Điều này giữ cho các đỉnh được xử lý theo thứ tự khoảng cách không giảm trong khi chỉ yêu cầu các phép toán deque trong thời gian không đổi cho mỗi lần thư giãn thành công. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ trong trường hợp xấu nhất | (O(n+m)) cộng với trạng thái tuyến đường | Quá chậm | 
| Tối ưu, 0-1 BFS | (O(n+m)) | (O(n+m)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng danh sách lân cận có hướng. Đối với mỗi lần chuyển đổi`a -> b`với loại`t`, cửa hàng`(b, t)`. Hướng quan trọng vì quá trình chuyển đổi có thể là một chiều. 
2. Khởi tạo`dist[1] = 0`và tất cả các khoảng cách khác đến vô cùng. Cũng lưu trữ`parent[v]`, nó sẽ ghi nhớ lỗ sâu đục trước đó trên con đường ngắn nhất đã chọn tới`v`. Đặt đỉnh 1 vào deque. 
3. Lấy một đỉnh`v`từ phía trước của deque. Đối với mọi chuyển đổi đi`v -> to`với chi phí`w`, tính toán`new_dist = dist[v] + w`. 
4. Nếu`new_dist`nhỏ hơn`dist[to]`, cập nhật`dist[to]`và thiết lập`parent[to] = v`. Nếu như`w`bằng không, đặt`to`ở phía trước của deque. Nếu như`w`là một, đặt`to`ở phía sau. 

Một cạnh có chi phí bằng 0 không làm tăng khoảng cách hiện tại, do đó điểm cuối của nó phải được xử lý trước các đỉnh đã ở xa hơn một hoặc nhiều ant-giờ. Cạnh một chi phí tăng khoảng cách thêm chính xác một, do đó điểm cuối của nó nằm sau các đỉnh hiện có với cùng khoảng cách. 
5. Tiếp tục cho đến khi hết deque. Tại thời điểm này`dist[v]`là chi phí tối thiểu có thể có từ lỗ sâu đục 1 đến mọi lỗ sâu đục có thể tiếp cận được, vì vậy cụ thể`dist[n]`là thời gian di chuyển tối thiểu tới đích. 
6. Nếu`dist[n]`vẫn là vô cùng, in`IMPOSSIBLE`. Nếu không, hãy bắt đầu tại`n`và nhiều lần theo dõi`parent[v]`cho đến khi đạt đến đỉnh 1. Điều này tạo ra lộ trình ngược, vì vậy hãy đảo ngược nó trước khi in. 
7. Tuyến đường được xây dựng lại được đảm bảo không lặp lại hố sâu. Nếu nó chứa một đỉnh lặp lại thì phần giữa hai lần xuất hiện sẽ là một chu trình. Loại bỏ chu trình đó sẽ làm cho tuyến đường không đắt hơn tuyến đường được biểu thị bằng khoảng cách ngắn nhất. Vì chuỗi tiền thân được hình thành từ các khoảng giãn cách ngắn nhất có giá trị nghiêm ngặt nên đường dẫn kết quả có thể được chọn là đường đi ngắn nhất đơn giản. 

### Tại sao nó hoạt động 

Bất biến trung tâm là bất cứ khi nào một đỉnh được xử lý từ deque, deque sẽ duy trì các đỉnh theo thứ tự không giảm của khoảng cách hiện tại của chúng. Việc nới lỏng một chi phí sẽ giữ nguyên khoảng cách và được di chuyển về phía trước, trong khi việc nới lỏng một chi phí sẽ tạo ra một khoảng cách lớn hơn chính xác một lần và được di chuyển về phía sau. Do đó, hành vi xử lý khoảng cách ngắn nhất đầu tiên của thuật toán Dijkstra được giữ nguyên mà không cần hàng đợi ưu tiên. 

Mọi sự thư giãn chỉ thay thế một khoảng cách bằng một giá trị nhỏ hơn rất nhiều, và`parent[to]`ghi lại một cạnh nhận ra khoảng cách mới. Khi thuật toán kết thúc, khoảng cách đích là chi phí tối thiểu của bất kỳ tuyến đường nào từ 1 đến n. Vì tất cả các chi phí cạnh đều không âm nên mỗi bước chứa một chu trình có thể loại bỏ chu trình đó mà không làm tăng chi phí của nó. Do đó tồn tại một tuyến đường đơn giản tối ưu và chuỗi tiền thân đưa ra một tuyến đường tối ưu như vậy. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())

    graph = [[] for _ in range(n + 1)]

    for _ in range(m):
        a, b, t = map(int, input().split())
        graph[a].append((b, t))

    INF = 10**18
    dist = [INF] * (n + 1)
    parent = [-1] * (n + 1)

    dist[1] = 0
    dq = deque([1])

    while dq:
        v = dq.popleft()

        for to, w in graph[v]:
            nd = dist[v] + w

            if nd < dist[to]:
                dist[to] = nd
                parent[to] = v

                if w == 0:
                    dq.appendleft(to)
                else:
                    dq.append(to)

    if dist[n] == INF:
        print("IMPOSSIBLE")
        return

    path = []
    cur = n

    while cur != -1:
        path.append(cur)
        if cur == 1:
            break
        cur = parent[cur]

    path.reverse()

    print(dist[n], len(path))
    print(*path)

if __name__ == "__main__":
    solve()
```Danh sách kề sử dụng các chỉ số từ 1 đến`n`, khớp với cách đánh số trong đầu vào. Phần tử bổ sung ở chỉ số 0 tránh việc trừ liên tục một số từ các số đỉnh.`dist`lưu trữ số lượng kiến ​​tối thiểu hiện được biết đến.`parent`lưu trữ tiền thân cần thiết để xây dựng lại. Một đỉnh liền trước chỉ được thay đổi khi tìm thấy một khoảng cách nhỏ hơn rất nhiều, do đó nó luôn tương ứng với khoảng cách hiện được gán cho đỉnh đó. 

Deque là chi tiết triển khai chính.`appendleft`xử lý lợi thế chi phí bằng 0, trong khi`append`xử lý cạnh một chi phí. Việc sử dụng đầu sai cho một trong hai loại cạnh sẽ phá hủy thứ tự khoảng cách mà BFS 0-1 dựa vào. 

Đích được phép là đỉnh 1. Trong trường hợp đó`dist[1]`đã bằng 0 và vòng lặp xây dựng lại ngay lập tức tạo ra tuyến đường`[1]`. 

Số nguyên Python không bị tràn, mặc dù giá trị vô cùng là`10**18`dù sao cũng là quá đủ. Chi phí đường đi ngắn nhất hữu ích tối đa có thể tối đa là số cạnh trong một đường dẫn đơn giản, do đó nó nằm dưới (10^5). 

Việc tái thiết theo cha mẹ từ`n`quay lại`1`, sau đó đảo ngược danh sách kết quả. Sự rõ ràng`cur == 1`kiểm tra ngăn chặn việc vô tình tiếp tục đi qua`parent[1]`, đó là`-1`. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Những thay đổi khoảng cách quan trọng được hiển thị dưới đây. Deque chính xác có thể chứa các đỉnh bổ sung giữa các trạng thái được hiển thị, nhưng bảng ghi lại các khoảng giãn xác định tuyến đường cuối cùng. 

| Đỉnh được xử lý | Thư giãn | Khoảng cách mới | Phụ huynh | Hiệu ứng Deque | 
| --- | --- | --- | --- | --- | 
| 1 |`1 -> 2`, giá 1 |`dist[2] = 1`| 1 | 2 quay trở lại | 
| 1 |`1 -> 3`, giá 1 |`dist[3] = 1`| 1 | 3 quay trở lại | 
| 2 |`2 -> 4`, giá 1 |`dist[4] = 2`| 2 | 4 quay trở lại | 
| 2 |`2 -> 5`, giá 1 |`dist[5] = 2`| 2 | 5 quay trở lại | 
| 3 |`3 -> 6`, giá 1 |`dist[6] = 2`| 3 | 6 quay trở lại | 
| 5 |`5 -> 7`, giá 0 |`dist[7] = 2`| 5 | 7 đi trước | 
| 7 |`7 -> 8`, giá 0 |`dist[8] = 2`| 7 | 8 đi trước | 
| 8 |`8 -> 9`, giá 0 |`dist[9] = 2`| 8 | 9 đi trước | 

Một kết quả hợp lệ được tạo ra bởi việc triển khai này là:```
2 6
1 2 5 7 8 9
```Mẫu của câu lệnh sử dụng một tuyến đường ngắn nhất khác,`1 3 5 7 8 9`, nhưng cả hai tuyến đều tốn hai kiến ​​giờ và không chứa lỗ sâu đục lặp lại. Điều này chứng tỏ tại sao người kiểm tra phải chấp nhận bất kỳ tuyến đường tối ưu nào thay vì so sánh đường dẫn được in với một đường dẫn mẫu cụ thể. 

### Mẫu 2 

Biểu đồ bao gồm hai vùng bị ngắt kết nối. Cái đầu tiên chứa các lỗ sâu có thể truy cập từ 1, trong khi cái thứ hai chứa lỗ sâu 15. 

| Vùng được xử lý | Chuyển đổi quan trọng | Kết quả | 
| --- | --- | --- | 
| 1 |`1 -> 2`, giá 0 |`dist[2] = 0`| 
| 1 |`1 -> 3`, giá 0 |`dist[3] = 0`| 
| 2 | Các cạnh gửi đi bằng 0/một chi phí | các đỉnh 4, 5, 8 có thể truy cập được | 
| 3 | Các cạnh gửi đi bằng 0/một chi phí | không có cạnh nào đạt 9 | 
| 4 | cạnh không tốn chi phí | các đỉnh 6, 7, 8 có thể truy cập được | 
| 8 | không chuyển sang 9 | thành phần thứ hai vẫn không thể truy cập được | 
| 9 | chưa bao giờ đạt tới |`dist[9] = infinity`| 
| 15 | chưa bao giờ đạt tới |`dist[15] = infinity`| 

Đích đến là wormhole 15 nên thuật toán kết thúc với`dist[15] = infinity`và in:```
IMPOSSIBLE
```Ví dụ này chứng minh rằng khả năng tiếp cận không phụ thuộc vào sự tồn tại của nhiều chuyển đổi không tốn phí. Một thành phần lớn được kết nối với chi phí bằng 0 vẫn không thể đến đích trong một thành phần bị ngắt kết nối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n+m)) | Mỗi cạnh được kiểm tra trong quá trình thư giãn và các phép toán deque là thời gian không đổi. | 
| Không gian | (O(n+m)) | Danh sách kề lưu trữ tất cả các chuyển đổi, trong khi khoảng cách, cha mẹ và deque sử dụng không gian bổ sung (O(n)). | 

Với (n,m \le 10^5), độ phức tạp tuyến tính chính xác là thang đo được yêu cầu bởi các ràng buộc. Thuật toán chỉ lưu trữ một vài mảng có kích thước (n) và danh sách kề có kích thước (m), vừa vặn với giới hạn bộ nhớ. Sự vắng mặt của đống cũng giữ cho các hệ số không đổi đủ nhỏ trong thời gian giới hạn. 

## Trường hợp thử nghiệm 

Bởi vì vấn đề cho phép bất kỳ tuyến đường ngắn nhất nào, nên các thử nghiệm nên xác thực các thuộc tính về chi phí và tuyến đường thay vì giả định một tuyến đường duy nhất. Phần khai thác sau đây nhúng logic giải pháp vào một hàm và kiểm tra kết quả đầu ra theo cấu trúc.```python
import sys
import io
from collections import deque

def solve_data(inp: str) -> str:
    data = list(map(int, inp.split()))
    it = iter(data)

    n = next(it)
    m = next(it)

    graph = [[] for _ in range(n + 1)]

    for _ in range(m):
        a = next(it)
        b = next(it)
        t = next(it)
        graph[a].append((b, t))

    INF = 10**18
    dist = [INF] * (n + 1)
    parent = [-1] * (n + 1)

    dist[1] = 0
    dq = deque([1])

    while dq:
        v = dq.popleft()

        for to, w in graph[v]:
            nd = dist[v] + w

            if nd < dist[to]:
                dist[to] = nd
                parent[to] = v

                if w == 0:
                    dq.appendleft(to)
                else:
                    dq.append(to)

    if dist[n] == INF:
        return "IMPOSSIBLE\n"

    path = []
    cur = n

    while True:
        path.append(cur)
        if cur == 1:
            break
        cur = parent[cur]

    path.reverse()

    return f"{dist[n]} {len(path)}\n" + " ".join(map(str, path)) + "\n"

def check(inp: str, expected_cost=None, expected_path=None):
    out = solve_data(inp).strip()

    if expected_path is not None:
        expected = (
            f"{expected_cost} {len(expected_path)}\n"
            + " ".join(map(str, expected_path))
        )
        assert out == expected
        return

    assert out == "IMPOSSIBLE"

# Provided sample 1.
sample1 = """\
9 11
1 2 1
1 3 1
2 4 1
2 5 1
3 5 1
3 6 1
4 9 1
6 9 1
5 7 0
7 8 0
8 9 0
"""
check(sample1, 2, [1, 2, 5, 7, 8, 9])

# Provided sample 2.
sample2 = """\
15 18
1 2 0
1 3 0
2 3 1
2 5 1
2 4 1
3 4 1
3 8 1
4 5 0
4 6 0
4 7 0
4 8 0
9 13 1
10 13 1
10 11 1
11 14 1
12 14 1
13 14 0
14 15 0
"""
check(sample2)

# Minimum-size graph: start equals destination.
case1 = """\
1 0
"""
check(case1, 0, [1])

# Unreachable destination.
case2 = """\
2 0
"""
check(case2)

# All transitions have zero cost. The route must remain simple.
case3 = """\
5 5
1 2 0
2 3 0
3 2 0
3 4 0
4 5 0
"""
check(case3, 0, [1, 2, 3, 4, 5])

# All useful transitions have cost one.
case4 = """\
4 3
1 2 1
2 3 1
3 4 1
"""
check(case4, 3, [1, 2, 3, 4])

# A zero-cost cycle must not appear in the reconstructed route.
case5 = """\
3 3
1 2 0
2 1 0
2 3 1
"""
check(case5, 1, [1, 2, 3])

# Large boundary case: 100000 vertices connected by 99999 zero-cost edges.
n = 100000
large_edges = "\n".join(
    f"{i} {i + 1} 0" for i in range(1, n)
)
large_input = f"{n} {n - 1}\n{large_edges}\n"
large_output = solve_data(large_input).split()

assert int(large_output[0]) == 0
assert int(large_output[1]) == n
assert int(large_output[2]) == 1
assert int(large_output[-1]) == n
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0`|`0 1 / 1`| Điểm bắt đầu và điểm đến có thể là cùng một đỉnh. | 
|`2 0`|`IMPOSSIBLE`| Xử lý đích không thể truy cập. | 
| Biểu đồ chi phí bằng 0 có chu kỳ |`0 5 / 1 2 3 4 5`| Chu kỳ chi phí bằng 0 không được gây ra các đỉnh lặp lại trong câu trả lời. | 
| Chuỗi bốn đỉnh của các cạnh giá một |`3 4 / 1 2 3 4`| Tích lũy đúng chi phí đơn vị. | 
|`1 -> 2 -> 1 -> 3`|`1 3 / 1 2 3`| Xây dựng lại một tuyến đường đơn giản mặc dù có chu kỳ chi phí bằng 0. | 
| Chuỗi 100.000 đỉnh không tốn chi phí | Trị giá`0`, 100.000 đỉnh | Số đỉnh tối đa và đầu vào tuyến tính lớn. | 

## Vỏ cạnh 

### Bắt đầu bằng đích đến 

cho```
1 0
```deque ban đầu chứa đỉnh 1 và`dist[1]`là số không. Không có quá trình chuyển đổi cần phải được xử lý. Quá trình tái thiết bắt đầu vào lúc`n = 1`, ngay lập tức dừng lại và quay trở lại`[1]`. Đầu ra là`0 1`theo sau là`1`. Đây là trường hợp ranh giới bắt các triển khai giả sử một đường dẫn phải chứa ít nhất một cạnh. 

### Đích đến không thể truy cập 

cho```
2 0
```deque chỉ chứa đỉnh 1. Vì không có chuyển tiếp đi nào nên đỉnh 2 vẫn ở vô cùng. Thuật toán in`IMPOSSIBLE`thay vì cố gắng làm theo`parent[2]`. Đây là lý do tại sao việc kiểm tra khả năng tiếp cận phải được thực hiện trước khi xây dựng lại. 

### Chu kỳ không tốn phí 

Hãy xem xét```
3 3
1 2 0
2 1 0
2 3 1
```Bộ chuyển đổi không chi phí đầu tiên`dist[2] = 0`Và`parent[2] = 1`. Quá trình xử lý 2 có thể kiểm tra cạnh trở về 1, nhưng cạnh đó lại cho khoảng cách bằng 0, điều này không thực sự tốt hơn so với cạnh hiện có`dist[1] = 0`, do đó cha của 1 không bị thay đổi. Cạnh`2 -> 3`cho`dist[3] = 1`Và`parent[3] = 2`. Tái thiết tạo ra`3 -> 2 -> 1`, ngược lại với`1 -> 2 -> 3`. Chu kỳ bị cấm không bao giờ đi vào chuỗi tiền thân. 

### Nhiều tuyến đường tối ưu 

cho```
4 4
1 2 1
1 3 1
2 4 0
3 4 0
```cả hai tuyến đường có thể đều tốn một. Tùy thuộc vào thứ tự kề và xử lý deque, thuật toán có thể chọn một trong hai`1 2 4`hoặc`1 3 4`. Cả hai đều hợp lệ vì khoảng cách là tối ưu và mỗi đỉnh chỉ xuất hiện một lần. Bài toán cho phép một cách rõ ràng bất kỳ giải pháp nào như vậy, do đó việc lựa chọn tất định một con đường ngắn nhất cụ thể là không cần thiết. 

### Kích thước đầu vào tối đa 

Một chuỗi gồm 100.000 đỉnh và 99.999 lần chuyển đổi yêu cầu thuật toán xử lý cơ bản toàn bộ đồ thị. Mỗi mục nhập kề được kiểm tra một lần như một phần của quá trình thư giãn, trong khi mảng khoảng cách và mảng gốc chỉ chứa 100.001 phần tử do lập chỉ mục dựa trên một. Thời gian chạy vẫn tuyến tính và mức tiêu thụ bộ nhớ vẫn tỷ lệ thuận với kích thước đầu vào. 

Bài học trọng tâm là điều kiện có vẻ khó khăn "không truy cập lỗ sâu đục hai lần" không yêu cầu theo dõi các tập đã truy cập ở trạng thái đường đi ngắn nhất. Chi phí cạnh không âm đảm bảo rằng các chu trình luôn có thể được loại bỏ khỏi bước đi tối ưu, do đó, thuật toán đường đi ngắn nhất thông thường là đủ. Sau đó, giới hạn chi phí bằng 0 hoặc một sẽ giảm hàng đợi ưu tiên của Dijkstra thành deque, đưa ra giải pháp (O(n+m)).
