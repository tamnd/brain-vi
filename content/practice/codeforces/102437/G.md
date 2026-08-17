---
title: "CF 102437G - Đường dẫn ngắn nhất được quy định"
description: "Chúng ta có một đồ thị vô hướng có các đỉnh là thành phố và các cạnh là đường. Sam xuất phát ở thành phố s vào thời điểm 0 và muốn đến thành phố t càng sớm càng tốt. Mỗi con đường đều có lịch trình thời tiết lặp lại riêng. Nếu một con đường có các tham số a, b và d thì chu kỳ của nó là P = a + b."
date: "2026-08-16T09:26:00+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102437
codeforces_index: "G"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2019-2020, \u0427\u0435\u0442\u0432\u0451\u0440\u0442\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430, \u0443\u0441\u043b\u043e\u0436\u043d\u0435\u043d\u043d\u0430\u044f \u043d\u043e\u043c\u0438\u043d\u0430\u0446\u0438\u044f"
rating: 0
weight: 102437
solve_time_s: 180
verified: false
draft: false
---

[CF 102437G - Đường dẫn ngắn nhất được quy định](https://codeforces.com/problemset/problem/102437/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị vô hướng có các đỉnh là thành phố và các cạnh là đường. Sam bắt đầu ở thành phố`s`vào thời điểm đó`0`và muốn đến thành phố`t`càng sớm càng tốt. Mỗi con đường đều có lịch trình thời tiết lặp lại riêng. Nếu đường có thông số`a`,`b`, Và`d`, thì chu kỳ của nó là`P = a + b`. Trong mỗi thời kỳ, đường khô ráo theo thời gian`Pk`bởi vì`Pk + a`, và có mưa từ`Pk + a`bởi vì`P(k + 1)`. Sam có thể tự do chờ đợi ở các thành phố, nhưng khi đi qua một con đường`d`đơn vị thời gian, toàn bộ quá trình truyền tải phải nằm trong một khoảng khô. 

Nhiệm vụ là tính thời gian đến sớm nhất tại`t`, hoặc`-1`nếu không có tuyến đường nào có thể sử dụng được. Các con đường không có hướng dẫn nên lịch trình thời gian giống nhau được áp dụng bất kể hướng Sam sử dụng đường. 

Đồ thị có tới`100000`thành phố và`200000`những con đường. Với kích thước này, các thuật toán kiểm tra tất cả các đường dẫn có thể là vô vọng, bởi vì số lượng các đường dẫn đơn giản có thể là số mũ. Thậm chí một`O(nm)`thuật toán đồ thị sẽ quá đắt ở quy mô này, vì vậy chúng ta cần một thuật toán gần tuyến tính hoặc`O(m log n)`tiếp cận. Các giá trị của`a`,`b`, Và`d`có thể đạt được`10^9`và việc chờ đợi có thể tích lũy trên nhiều con đường, do đó việc triển khai cũng phải xử lý thời gian lớn hơn nhiều so với số nguyên 32 bit. Số nguyên Python tự động phù hợp cho việc này. 

Có một số ranh giới mà việc triển khai bất cẩn có thể thất bại. Thứ nhất, một con đường chỉ có thể sử dụng được nếu`d <= a`. Ví dụ,```
2 1 1 2
1 2 1 1 2
```có một khoảng thời gian khô dài`1`, nhưng con đường mất`2`đơn vị để vượt qua, vì vậy câu trả lời là`-1`. Việc coi đường là có thời gian di chuyển trung bình hoặc chỉ kiểm tra xem đường có khô ráo vào thời điểm khởi hành hay không sẽ cho phép điều đó một cách không chính xác. 

Ranh giới thứ hai là Sam có thể bắt đầu chính xác khi giai đoạn khô ráo bắt đầu và có thể kết thúc chính xác khi giai đoạn đó kết thúc. Ví dụ,```
2 1 1 2
1 2 2 3 2
```có một khoảng thời gian khô`[0, 2]`, cứ thế du hành theo thời gian`0`theo thời gian`2`là hợp lệ và câu trả lời là`2`. Một bất đẳng thức chặt chẽ như`departure + d < a`sẽ từ chối tuyến đường này một cách không chính xác. 

Ranh giới thứ ba xuất hiện khi Sam đến một con đường đúng lúc trời bắt đầu mưa. Coi như```
3 2 1 3
1 2 2 3 2
2 3 2 3 1
```Con đường đầu tiên có thể đi từ`0`ĐẾN`2`. Vào thời điểm`2`, mưa bắt đầu trên con đường thứ hai nên Sam không thể bắt đầu đi được. Khoảng thời gian khô tiếp theo bắt đầu vào thời điểm`5`, cho biết thời gian đến`6`. Đầu ra đúng là`6`. Một công thức chỉ kiểm tra thời gian khởi hành so với khoảng thời gian khô và quên toàn bộ quá trình di chuyển sẽ tạo ra kết quả không chính xác`3`. 

Cuối cùng, nếu`s == t`, Sam đã đến đích rồi nên đáp án là`0`, ngay cả khi không có đường. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực trực tiếp có thể liệt kê mọi tuyến đường có thể từ`s`ĐẾN`t`, mô phỏng lịch trình thời tiết trên từng con đường trên tuyến đường đó và giữ nguyên thời gian đến sớm nhất. Điều này đúng vì mọi hành trình khả thi đều tương ứng với một số đường đi trên đồ thị và việc mô phỏng một đường đi cố định sẽ cho chúng ta biết thời gian đi qua sớm nhất có thể của nó. Vấn đề là số lượng đường dẫn. Ngay cả với biểu đồ thưa thớt, biểu đồ lớp có thể chứa`2^(n/2)`rõ ràng đơn giản`s`ĐẾN`t`những con đường. Nếu mọi con đường đều mất`Theta(n)`làm việc để mô phỏng, trường hợp xấu nhất là`Theta(n * 2^(n/2))`hoạt động, vượt xa những gì có thể được xử lý cho`n = 100000`. 

Quan sát hữu ích là chúng ta không cần phải nhớ toàn bộ lịch sử của Sam khi anh ấy đến một thành phố. Giả sử anh ta đến một thành phố vào lúc`x`. Đối với bất kỳ con đường đi nào, có một thời điểm sớm nhất được xác định duy nhất mà tại đó anh ta có thể bắt đầu đi qua con đường đó. Việc chờ đợi trong thành phố lâu hơn không bao giờ có thể khiến việc đến điểm cuối kia sớm hơn thời điểm khởi hành sớm nhất khả thi này. 

Đối với một con đường, hãy`P = a + b`và để`r = x mod P`. Nếu như`d > a`, con đường không bao giờ có thể đi qua được vì mỗi khoảng thời gian khô ráo chỉ là`a`dài đơn vị. Ngược lại Sam có thể rời đi ngay khi`r + d <= a`. Nếu điều kiện đó không thành công, anh ta phải đợi cho đến khi khoảng thời gian khô tiếp theo bắt đầu, tức là`x - r + P`, rồi chi tiêu`d`các đơn vị trên đường. 

Điều này mang lại sự thư giãn cạnh phụ thuộc vào thời gian và cần có thời gian không đổi. Quan trọng hơn, hàm đến sớm nhất là FIFO: đến một thành phố muộn hơn không bao giờ có thể cho phép chúng ta đến điểm cuối khác sớm hơn bằng cách đi cùng một con đường. Bên trong phần có thể di chuyển tức thời, thời gian đến tăng theo thời gian bắt đầu. Khi đường truyền không còn phù hợp nữa, tất cả thời gian bắt đầu đó sẽ chờ cùng khoảng thời gian khô tiếp theo, tạo ra một mặt cắt phẳng và sau đó chức năng sẽ tăng trở lại. Vì vậy, sự lựa chọn tham lam của Dijkstra bình thường vẫn có hiệu lực. 

Cách tiếp cận brute-force có hiệu quả vì mọi đường dẫn đều có thể được đánh giá chính xác nhưng không thành công vì có quá nhiều đường dẫn. Thuộc tính FIFO cho phép chúng ta thu gọn tất cả các đường dẫn đến cùng một thành phố vào một trạng thái duy nhất, thời gian đến sớm nhất. Sau đó, chúng ta có thể sử dụng thuật toán Dijkstra, thay thế trọng số cạnh cố định bằng hàm thời gian không đổi để tính toán thời điểm đến hợp pháp sớm nhất qua con đường đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`Theta(n * 2^(n/2))`trong biểu đồ lớp thưa thớt |`O(n + m)`cộng với trạng thái đệ quy/đường dẫn | Quá chậm | 
| Tối ưu |`O((n + m) log n)`|`O(n + m)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng danh sách lân cận chứa mọi con đường theo cả hai hướng. Đối với mỗi con đường, lưu trữ các thông số của nó`a`,`b`, Và`d`, vì lịch trình thời tiết ở cả hai điểm cuối đều giống nhau. 
2. Khởi tạo`dist[s] = 0`và mọi khoảng cách khác đến vô cùng. Đặt`(0, s)`thành một đống tối thiểu. Đống dữ liệu luôn cho chúng ta biết thành phố có thời gian đến sớm nhất được biết hiện nay là nhỏ nhất. 
3. Khi thành phố`u`được loại bỏ khỏi heap theo thời gian`cur`, bỏ qua mục nhập nếu`cur`lớn hơn`dist[u]`. Mục nhập như vậy đã cũ vì việc nới lỏng sau này đã tìm ra cách tốt hơn để tiếp cận`u`. 
4. Đối với mọi con đường từ`u`ĐẾN`v`, trước tiên hãy tính chu kỳ của nó`P = a + b`. Nếu như`d > a`, bỏ qua con đường này vì không có khoảng thời gian khô ráo nào đủ dài để chứa người đi qua. 
5. Ngược lại tính toán`r = cur % P`. Nếu như`r + d <= a`, Sam có thể vào đường ngay nên thời gian đến của ứng viên là`cur + d`. 
6. Nếu`r + d > a`, khoảng thời gian khô hiện tại quá ngắn. Sam đợi cho đến khi tiết học tiếp theo bắt đầu vào lúc`cur - r + P`, sau đó băng qua đường, đưa ứng viên đến`cur - r + P + d`. 
7. Nếu ứng viên này nhỏ hơn`dist[v]`, cập nhật`dist[v]`và đẩy`(candidate, v)`vào đống. Việc thư giãn có vai trò giống hệt như trong Dijkstra thông thường, ngoại trừ thời gian di chuyển hiệu quả của cạnh phụ thuộc vào thời gian hiện tại. 
8. Khi vùng heap trống, xuất ra`dist[t]`nếu nó hữu hạn, nếu không thì xuất ra`-1`. Chúng ta cũng có thể dừng lại ngay khi`t`được xuất hiện với khoảng cách hiện tại của nó, bởi vì heap đảm bảo rằng không có trạng thái nào trong tương lai có thể có thời gian đến nhỏ hơn. 

### Tại sao nó hoạt động 

Điều bất biến là bất cứ khi nào một thành phố`u`được trích xuất vĩnh viễn khỏi đống,`dist[u]`là thời gian sớm nhất có thể mà Sam có thể đến được`u`. Đối với mỗi con đường đi, việc thư giãn sẽ tính toán thời điểm đến sớm nhất có thể tại điểm cuối khác của nó với thời gian đến đó tại`u`. Hàm đến của đường là FIFO, do đó việc đạt tới`u`vào thời điểm sau đó không thể tạo ra một điểm đến sớm hơn trên con đường đó. Do đó, lập luận Dijkstra thông thường được áp dụng: sau khi trích xuất thời gian đến dự kiến ​​nhỏ nhất, không có tuyến đường nào chưa được khám phá có thể đến thành phố đó sớm hơn. Việc lặp lại điều này trong tất cả các lần thư giãn sẽ mang lại thời gian đến sớm nhất thực sự cho mọi thành phố có thể tiếp cận, bao gồm cả`t`. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

INF = 10**30

def solve():
    n, m, s, t = map(int, input().split())

    graph = [[] for _ in range(n)]

    for _ in range(m):
        u, v, a, b, d = map(int, input().split())
        u -= 1
        v -= 1
        graph[u].append((v, a, b, d))
        graph[v].append((u, a, b, d))

    dist = [INF] * n
    dist[s - 1] = 0

    pq = [(0, s - 1)]

    while pq:
        cur, u = heapq.heappop(pq)

        if cur != dist[u]:
            continue

        if u == t - 1:
            print(cur)
            return

        for v, a, b, d in graph[u]:
            if d > a:
                continue

            period = a + b
            r = cur % period

            if r + d <= a:
                arrive = cur + d
            else:
                arrive = cur - r + period + d

            if arrive < dist[v]:
                dist[v] = arrive
                heapq.heappush(pq, (arrive, v))

    print(-1)

if __name__ == "__main__":
    solve()
```Danh sách kề lưu trữ mỗi con đường vô hướng hai lần, điều này làm cho mọi phần còn lại giống hệt nhau bất kể hướng đi qua. Mỗi bộ dữ liệu được lưu trữ chứa chính xác các tham số cần thiết để tính toán lần truyền tải hợp lệ tiếp theo. 

Séc`d > a`loại bỏ những con đường không thể ngay lập tức. Khoảng thời gian khô có độ dài chính xác`a`, do đó không có đường truyền dài hơn`a`có thể nằm gọn trong đó. 

biểu thức`cur % period`cho biết vị trí của Sam trong chu kỳ thời tiết hiện tại. Khi`r + d <= a`, toàn bộ quá trình truyền tải phù hợp trước khi mưa bắt đầu. Sự bình đẳng được cho phép vì điểm cuối của khoảng thời gian khô là hợp lệ. 

Khi quá trình truyền tải không phù hợp,`cur - r`là sự khởi đầu của giai đoạn hiện tại, vì vậy`cur - r + period`là thời điểm bắt đầu của giai đoạn tiếp theo. Thêm`d`đến sớm nhất sau khi chờ đợi. Công thức này tránh mọi số học dấu phẩy động và xử lý thời gian có kích thước tùy ý. 

Hàng đợi ưu tiên sử dụng tính năng xóa lười. Một thành phố có thể được chèn nhiều lần sau khi tìm thấy các tuyến đường tốt hơn dần dần, do đó, cặp được trích xuất sẽ bị bỏ qua bất cứ khi nào thời gian của nó khác với thời gian hiện tại.`dist[u]`. 

Câu trả lời tối đa có thể có thể lớn hơn nhiều so với`10^9`, vì mỗi con đường có thể đóng góp một lượng lớn thời gian đi lại và chờ đợi. Các số nguyên có độ chính xác tùy ý của Python tránh tràn mà không cần xử lý đặc biệt. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
3 2 1 3
1 2 3 4 1
2 3 2 3 2
```Đối với con đường đầu tiên,`P = 7`và khoảng thời gian khô là`[0, 3]`. Bắt đầu vào lúc`0`, việc truyền tải một đơn vị kết thúc vào thời điểm`1`. 

Đối với con đường thứ hai,`P = 5`và khoảng thời gian khô là`[0, 2]`. Sam đến thành phố`2`vào thời điểm đó`1`, nhưng việc truyền tải hai đơn vị sẽ kết thúc vào lúc`3`, sau khi mưa đã bắt đầu. Vì thế anh đợi đến lúc`5`và đến thành phố`3`vào thời điểm đó`7`. 

| Thành phố nổi bật | Thời điểm hiện tại | Đường được xem xét |`r`| Đến sớm nhất | 
| --- | --- | --- | --- | --- | 
| 1 | 0 |`1 -> 2`| 0 | 1 | 
| 2 | 1 |`2 -> 1`| 1 | 2 | 
| 2 | 1 |`2 -> 3`| 1 | 7 | 
| 3 | 7 | destination | | 7 | 

Điểm mấu chốt là sự thư giãn thứ hai. Có mặt thực tế tại thành phố`2`vào thời điểm đó`1`không có nghĩa là Sam có thể sử dụng ngay con đường thứ hai. Toàn bộ quá trình truyền tải phải phù hợp với khoảng thời gian khô, vì vậy thuật toán sẽ đợi khoảng thời gian tiếp theo và thu được chính xác`7`. 

### Mẫu 2 

Không có mẫu chính thức thứ hai trong tuyên bố được cung cấp, vì vậy hãy xem xét ví dụ tập trung vào ranh giới sau:```
4 3 1 4
1 2 3 2 2
2 4 2 3 1
1 3 1 1 2
```Đường thứ nhất có đoạn khô`[0, 3]`, để Sam có thể duyệt nó từ`0`ĐẾN`2`. Đường thứ hai có đoạn khô`[0, 2]`, và Sam đến đúng lúc`2`, vì vậy việc bắt đầu từ ranh giới đó là hợp lệ và anh ta đến được thành phố`4`vào thời điểm đó`3`. Con đường thứ ba không sử dụng được vì thời gian di chuyển của nó`2`lớn hơn chiều dài khoảng thời gian khô của nó`1`. 

| Thành phố nổi bật | Thời điểm hiện tại | Đường được xem xét |`r`| Đến sớm nhất | 
| --- | --- | --- | --- | --- | 
| 1 | 0 |`1 -> 2`| 0 | 2 | 
| 1 | 0 |`1 -> 3`| 0 | không sử dụng được | 
| 2 | 2 |`2 -> 4`| 2 | 3 | 
| 4 | 3 | điểm đến | | 3 | 

Dấu vết này thực hiện hai quy tắc biên cùng một lúc. Việc truyền tải kết thúc chính xác ở cuối khoảng khô là hợp pháp và một cạnh với`d > a`phải bị từ chối trước khi cố gắng tính thời gian khởi hành. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O((n + m) log n)`| Mỗi con đường được nới lỏng từ cả hai điểm cuối và chi phí vận hành cao`O(log n)`| 
| Không gian |`O(n + m)`| Danh sách lân cận chứa`2m`các mục được hướng dẫn, trong khi khoảng cách và việc sử dụng vùng heap`O(n + m)`không gian | 

Với`n <= 100000`Và`m <= 200000`, thuật toán thực hiện một lượng thư giãn số học không đổi trên mỗi cạnh và sử dụng đống nhị phân cho hàng đợi ưu tiên. Đây là thang đo tiêu chuẩn`O(m log n)`là thực tế, trong khi việc liệt kê các đường đi hoặc quét liên tục tất cả các cạnh sẽ không thực hiện được. 

## Trường hợp thử nghiệm```python
import sys
import io
import heapq

input = sys.stdin.readline
INF = 10**30

def solve():
    n, m, s, t = map(int, input().split())

    graph = [[] for _ in range(n)]

    for _ in range(m):
        u, v, a, b, d = map(int, input().split())
        u -= 1
        v -= 1
        graph[u].append((v, a, b, d))
        graph[v].append((u, a, b, d))

    dist = [INF] * n
    dist[s - 1] = 0
    pq = [(0, s - 1)]

    while pq:
        cur, u = heapq.heappop(pq)

        if cur != dist[u]:
            continue

        if u == t - 1:
            print(cur)
            return

        for v, a, b, d in graph[u]:
            if d > a:
                continue

            period = a + b
            r = cur % period

            if r + d <= a:
                arrive = cur + d
            else:
                arrive = cur - r + period + d

            if arrive < dist[v]:
                dist[v] = arrive
                heapq.heappush(pq, (arrive, v))

    print(-1)

def run(inp: str) -> str:
    global input
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline
    out = io.StringIO()

    old_stdout = sys.stdout
    sys.stdout = out
    try:
        solve()
    finally:
        sys.stdout = old_stdout

    return out.getvalue().strip()

assert run(
    """3 2 1 3
1 2 3 4 1
2 3 2 3 2
"""
) == "7", "sample 1"

assert run(
    """1 0 1 1
"""
) == "0", "start already equals destination"

assert run(
    """2 1 1 2
1 2 1 1 2
"""
) == "-1", "road traversal is longer than every dry interval"

assert run(
    """4 3 1 4
1 2 3 2 2
2 4 2 3 1
1 3 1 1 2
"""
) == "3", "exact dry-interval boundary and unusable road"

assert run(
    """3 2 1 3
1 2 2 3 2
2 3 2 3 1
"""
) == "6", "arriving exactly when rain starts requires waiting"

assert run(
    """4 3 1 4
1 2 1 1 1
2 3 1 1 1
3 4 1 1 1
"""
) == "3", "all equal values"

n = 100000
lines = [f"{n} {n - 1} 1 {n}"]
for i in range(1, n):
    lines.append(f"{i} {i + 1} 1 1 1")
assert run("\n".join(lines) + "\n") == str(n - 1), "large chain"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0 1 1`|`0`| Đồ thị kích thước tối thiểu và`s == t`| 
|`2 1 1 2`với`a=1, d=2`|`-1`| Đường là không thể khi`d > a`| 
| Trường hợp ranh giới bốn thành phố |`3`| Quá trình truyền tải có thể kết thúc chính xác tại ranh giới khô | 
| Trường hợp chờ đợi ba thành phố |`6`| Đến lúc bắt đầu mưa buộc phải chờ đợi cả thời gian | 
| Chuỗi tham số bằng nhau bốn thành phố |`3`| Lịch trình giống hệt nhau lặp đi lặp lại | 
|`100000`chuỗi -đỉnh |`99999`| Kích thước đầu vào lớn và thời gian di chuyển tích lũy | 

## Vỏ cạnh 

cho`s == t`, hãy xem xét đầu vào chính xác```
1 0 1 1
```Hàng đợi ưu tiên bắt đầu bằng thành phố`1`vào thời điểm đó`0`. Vì đã là đích nên thuật toán in ngay`0`. Không cần xử lý cạnh và việc không có đường là không liên quan. 

Đối với một con đường không thể sử dụng được, hãy xem xét```
2 1 1 2
1 2 1 1 2
```Con đường có chu kỳ`2`, với các khoảng khô có chiều dài`1`. Thuật toán nhìn thấy`d = 2 > a = 1`và bỏ qua đường. Đích vẫn ở vô cực nên đầu ra là`-1`. Điều này tốt hơn là cố gắng chờ đợi một vị trí đặc biệt trong chu kỳ, bởi vì không có khoảng thời gian khô nào có thể chứa đựng toàn bộ quá trình truyền tải. 

Để di chuyển qua ranh giới khô chính xác, hãy sử dụng```
2 1 1 2
1 2 2 3 2
```Vào thời điểm`0`,`r = 0`, Và`r + d = 2 = a`. Đã thỏa mãn điều kiện nên Sam rời đi ngay và đến đúng giờ`2`. Đầu ra là`2`. Sự bình đẳng trong`r + d <= a`là cần thiết. 

Để đến chính xác khi mưa bắt đầu, hãy sử dụng```
3 2 1 3
1 2 2 3 2
2 3 2 3 1
```Con đường đầu tiên đưa Sam từ`0`ĐẾN`2`. Đối với con đường thứ hai, chu kỳ của nó là`5`, vậy có lúc`2`chúng tôi có`r = 2`. Từ`r + d = 3 > a = 2`, khoảng thời gian khô hiện tại không thể chứa đường truyền. Khoảng thời gian khô tiếp theo bắt đầu lúc`5`, đưa đến`5 + 1 = 6`. Đầu ra là`6`. Điều này mắc phải sai lầm phổ biến là chỉ kiểm tra xem đường có khô ráo ngay lúc khởi hành hay không. 

Đối với ranh giới đầu vào lớn, một chuỗi`100000`các thành phố với mọi con đường được đặt thành`a = b = d = 1`có một hành vi đặc biệt đơn giản. Mỗi con đường có thể đi qua từ một ranh giới chu kỳ chính xác, do đó mỗi cạnh đóng góp chính xác một đơn vị và không cần phải chờ đợi. Đích đến là vào lúc`99999`. Việc triển khai Dijkstra xử lý biểu đồ trong`O(n log n)`thời gian cho trường hợp này và sử dụng lưu trữ đồ thị tuyến tính, điều này chứng tỏ tại sao độ phức tạp tiệm cận lại phù hợp với các ràng buộc tối đa.
