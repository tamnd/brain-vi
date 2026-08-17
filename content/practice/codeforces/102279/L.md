---
title: "CF 102279L - Trái hay phải? Còn cả hai thì sao?"
description: "Chúng ta có một mảng một chiều gồm N vị trí. B21 xuất phát ở vị trí u và muốn đến vị trí v. Di chuyển từ vị trí i đến i + 1 tốn R, còn di chuyển từ i sang i - 1 tốn L. Những bước di chuyển này chỉ thực hiện được khi vị trí đích nằm trong mảng."
date: "2026-08-16T19:23:59+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102279
codeforces_index: "L"
codeforces_contest_name: "HCW 19 Team Round (ICPC format)"
rating: 0
weight: 102279
solve_time_s: 138
verified: true
draft: false
---

[CF 102279L - Trái hay Phải? Còn không thì sao?](https://codeforces.com/problemset/problem/102279/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 18s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Ta có mảng một chiều`N`các vị trí. B21 xuất phát tại vị trí`u`và muốn đạt được vị trí`v`. Di chuyển từ vị trí`i`ĐẾN`i + 1`chi phí`R`, khi di chuyển từ`i`ĐẾN`i - 1`chi phí`L`. Những di chuyển này chỉ có thể thực hiện được khi vị trí đích nằm trong mảng. 

Có một hoạt động bổ sung. Nếu hai vị trí chứa cùng một giá trị mảng, B21 có thể dịch chuyển trực tiếp giữa chúng để lấy phí`C`, bất kể khoảng cách của họ. Nhiệm vụ là tìm ra năng lượng tối thiểu cần thiết để có được từ`u`ĐẾN`v`. 

Đầu vào chứa`N`, chi phí di chuyển hai hướng`L`Và`R`, và chi phí dịch chuyển`C`. Dòng tiếp theo cung cấp vị trí bắt đầu và đích, dòng cuối cùng chứa các giá trị mảng. Đầu ra cần thiết là năng lượng tối thiểu của bất kỳ chuỗi di chuyển và dịch chuyển thông thường hợp lệ nào. 

Ràng buộc`N <= 10^5`loại trừ các thuật toán kiểm tra từng cặp vị trí. Đặc biệt, nếu nhiều vị trí có cùng giá trị, việc xem xét rõ ràng mọi khả năng dịch chuyển có thể tạo ra khoảng`N^2`kết nối. Với tối đa`10^5`vị trí, chúng ta cần một giải pháp có công việc gần như tuyến tính hoặc`N log N`. Chi phí có thể lớn như`10^9`và một đường dẫn có thể chứa nhiều thao tác, vì vậy số nguyên 32 bit là không đủ trong các ngôn ngữ có loại số nguyên có chiều rộng cố định. Số nguyên Python tự động xử lý việc này. 

Một số trường hợp rất dễ xử lý sai. Đầu tiên, vị trí xuất phát và đích có thể bằng nhau. Ví dụ,```
2 5 7 3
2 2
1 2
```yêu cầu`0`, vì B21 đã ở đích rồi. Việc triển khai luôn xem xét ít nhất một chuyển động có thể trả về chi phí dương một cách không chính xác. 

Tuyến đường rẻ nhất có thể sử dụng dịch chuyển tức thời ngay cả khi vị trí phù hợp không liền kề nhau. Ví dụ,```
3 10 10 2
1 3
5 1 5
```có câu trả lời`2`: vị trí`1`Và`3`chứa cùng một giá trị nên B21 có thể dịch chuyển trực tiếp. Việc triển khai chỉ xem xét các vị trí lân cận sẽ trả về`20`. 

Hướng đi rẻ hơn cũng có thể phụ thuộc vào hướng mà tuyến đường đi. Ví dụ,```
2 7 2 100
2 1
1 2
```có câu trả lời`7`, bởi vì chuyển động hữu ích duy nhất là lùi lại một bước. Việc coi chuyển động có một chi phí đối xứng sẽ là sai lầm. 

Cuối cùng, dịch chuyển tức thời có thể kết nối các vị trí cách xa nhau và một số nhóm dịch chuyển khác nhau có thể tham gia vào một tuyến đường tối ưu. Sẽ không đủ nếu chỉ xem xét một lần dịch chuyển tức thời từ giá trị bắt đầu đến giá trị đích. Công thức biểu đồ bên dưới tự động xử lý các chuỗi tùy ý. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là coi mọi vị trí mảng là một đỉnh của đồ thị. Các vị trí liên tiếp có các cạnh có hướng: từ`i`ĐẾN`i + 1`với chi phí`R`, và từ`i`ĐẾN`i - 1`với chi phí`L`. Đối với mỗi cặp vị trí có cùng giá trị, chúng ta cũng có thể thêm chi phí dịch chuyển`C`. 

Biểu đồ này thể hiện chính xác bài toán ban đầu, vì vậy việc chạy thuật toán Dijkstra sẽ cho câu trả lời đúng. Vấn đề là số lượng cạnh dịch chuyển. Nếu tất cả`N`các vị trí chứa cùng một giá trị, có`N(N - 1)`các cạnh dịch chuyển có hướng. Vì`N = 100000`, đó là về`10^10`các cạnh, vượt xa những gì có thể được xây dựng hoặc xử lý trong thời gian giới hạn. 

Quan sát quan trọng là tất cả các cạnh dịch chuyển được liên kết với một giá trị đều hoạt động giống hệt nhau. Giả sử giá trị`x`xảy ra ở các vị trí`p1, p2, ..., pk`. Từ bất kỳ vị trí nào trong số này, B21 có thể chi tiêu chính xác`C`để vào mạng dịch chuyển để lấy giá trị`x`và từ mạng đó anh ta có thể đạt được bất kỳ sự xuất hiện nào của`x`mà không phải trả thêm bất cứ điều gì. 

Chúng ta có thể biểu diễn toàn bộ bộ sưu tập các cạnh dịch chuyển theo cặp bằng một đỉnh biểu đồ bổ sung được gọi là trung tâm giá trị. Đối với mỗi lần xuất hiện`pi`của`x`, thêm một cạnh`pi -> hub_x`với chi phí`C`, và một cạnh`hub_x -> pi`với chi phí`0`. Đi từ`pi`ĐẾN`pj`thông qua trung tâm chi phí chính xác`C + 0 = C`, đó chính xác là chi phí của dịch chuyển tức thời ban đầu. 

Bây giờ mỗi vị trí mảng chỉ đóng góp một số cạnh không đổi. Có nhiều nhất`N`các giá trị khác nhau, do đó có nhiều nhất`2N`đỉnh đồ thị và`O(N)`các cạnh. Thuật toán Dijkstra sau đó chạy vào`O(N log N)`thời gian. 

Cấu trúc brute-force hoạt động hiệu quả vì mọi dịch chuyển tức thời có thể được thể hiện rõ ràng nhưng không thành công khi một giá trị xảy ra nhiều lần. Quan sát cho thấy tất cả các dịch chuyển tức thời chia sẻ một giá trị có thể được nén vào một trung tâm sẽ làm giảm biểu đồ dịch chuyển tức thời bậc hai thành biểu đồ thưa thớt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(N^2 log N)`sau khi xây dựng tất cả các cạnh dịch chuyển |`O(N^2)`| Quá chậm | 
| Tối ưu |`O(N log N)`|`O(N)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng và tạo một trung tâm ảo cho mọi giá trị riêng biệt. Hub thể hiện khả năng dịch chuyển tức thời giữa tất cả các vị trí chứa giá trị đó. 
2. Tạo các cạnh chuyển động thông thường giữa các vị trí liền kề. Đối với mọi`i < N`, thêm vào`i -> i + 1`với chi phí`R`, và thêm`i + 1 -> i`với chi phí`L`. Đây là các cạnh có hướng vì việc di chuyển tiến và lùi có thể có chi phí khác nhau. 
3. Đối với mọi vị trí`i`, kết nối nó với trung tâm thuộc về`A[i]`với chi phí`C`và kết nối trung tâm đó trở lại vị trí`i`với chi phí`0`. Đường dẫn hai cạnh biểu thị một lần dịch chuyển từ`i`đến bất kỳ lần xuất hiện nào khác có cùng giá trị. 
4. Chạy thuật toán Dijkstra từ vị trí`u`. Mỗi cạnh của đồ thị đều có chi phí không âm, do đó, lần đầu tiên một đỉnh bị xóa khỏi hàng ưu tiên với khoảng cách ngắn nhất hiện tại, khoảng cách đó là cuối cùng. 
5. Trả về khoảng cách vị trí`v`. Vì mọi chuyển động dịch chuyển hợp lệ hoặc dịch chuyển tức thời trong bài toán ban đầu đều có một đường dẫn tương đương trong biểu đồ nén và mọi đường dịch chuyển tức thời được nén đều tương ứng với một dịch chuyển tức thời hợp pháp, nên đường đi ngắn nhất của đồ thị chính xác là năng lượng tối thiểu cần thiết. 

### Tại sao nó hoạt động 

Hãy xem xét bất kỳ con đường pháp lý nào trong vấn đề ban đầu. Mỗi nước đi thông thường đều tương ứng trực tiếp với một cạnh vị trí liền kề. Mỗi lần dịch chuyển từ vị trí`i`để định vị`j`với`A[i] = A[j]`tương ứng với`i -> hub[A[i]] -> j`, tổng chi phí của nó là`C`. Do đó, mọi tuyến đường ban đầu đều có một tuyến đồ thị có giá trị như nhau. 

Ngược lại, mọi tuyến đồ thị bao gồm các cạnh chuyển động thông thường hoặc một chuỗi từ vị trí đến trung tâm đến vị trí. Cái sau là dịch chuyển hợp lệ vì cả hai vị trí đều thuộc cùng một nhóm giá trị và chi phí chính xác`C`. Do đó, mọi tuyến đồ thị biểu thị một tuyến ban đầu hợp lệ với cùng chi phí. 

Do đó, đường đi ngắn nhất trong đồ thị nén là năng lượng hợp lệ tối thiểu. Dijkstra tìm thấy đường đi ngắn nhất vì tất cả trọng số của các cạnh đều không âm. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    n, L, R, C = map(int, input().split())
    u, v = map(int, input().split())
    a = list(map(int, input().split()))

    # Compress each distinct array value into one virtual hub.
    value_id = {}
    groups = []

    for i, x in enumerate(a):
        if x not in value_id:
            value_id[x] = n + len(groups)
            groups.append([])
        groups[value_id[x] - n].append(i)

    m = n + len(groups)
    graph = [[] for _ in range(m)]

    # Ordinary moves along the array.
    for i in range(n - 1):
        graph[i].append((i + 1, R))
        graph[i + 1].append((i, L))

    # Teleport hubs.
    for hub_index, positions in enumerate(groups):
        hub = n + hub_index
        for pos in positions:
            graph[pos].append((hub, C))
            graph[hub].append((pos, 0))

    start = u - 1
    target = v - 1

    INF = 10**30
    dist = [INF] * m
    dist[start] = 0

    pq = [(0, start)]

    while pq:
        d, node = heapq.heappop(pq)

        if d != dist[node]:
            continue

        if node == target:
            print(d)
            return

        for nxt, cost in graph[node]:
            nd = d + cost
            if nd < dist[nxt]:
                dist[nxt] = nd
                heapq.heappush(pq, (nd, nxt))

if __name__ == "__main__":
    solve()
```Phần đầu tiên gán cho mỗi giá trị mảng riêng biệt một đỉnh ảo duy nhất. Giá trị số thực tế có thể lớn bằng`10^9`, vì vậy việc sử dụng chính giá trị đó làm chỉ mục mảng sẽ lãng phí. Từ điển ánh xạ từng giá trị vào một chỉ mục trung tâm nhỏ gọn. 

Các cạnh vị trí liền kề mã hóa hai hướng chuyển động riêng biệt. Cạnh từ`i`ĐẾN`i + 1`chi phí`R`, trong khi cạnh ngược lại có giá`L`. Các vị trí được lưu trữ nội bộ bằng cách sử dụng chỉ mục dựa trên số 0, vì vậy vị trí đầu vào`u`trở thành`u - 1`. 

Đối với mỗi lần xuất hiện của một giá trị, mã sẽ thêm chi phí`C`cạnh từ vị trí đến trung tâm của nó và cạnh có chi phí bằng 0 từ trung tâm trở lại vị trí. Do đó, dịch chuyển tức thời từ lần xuất hiện này sang lần xuất hiện khác được biểu thị bằng chính xác hai cạnh. 

Hàng đợi ưu tiên chứa`(distance, vertex)`cặp. Séc`if d != dist[node]`loại bỏ các mục cũ vì Python`heapq`không hỗ trợ giảm khóa hiện có. Khi tìm thấy khoảng cách tốt hơn, một cặp mới chỉ cần được chèn vào. 

Việc quay trở lại sớm khi mục tiêu được bắn ra là hợp lệ theo thuật toán của Dijkstra. Tại thời điểm đó, mục tiêu có khoảng cách dự kiến ​​nhỏ nhất trong số tất cả các đỉnh còn lại, do đó, việc thư giãn sau này không thể tạo ra một đường đi nhỏ hơn. 

Không có vấn đề tràn số nguyên trong Python. các`10**30`value chỉ đơn thuần là một giá trị vô cùng tiện lợi lớn hơn nhiều so với bất kỳ câu trả lời khả thi nào. 

## Ví dụ đã hoạt động 

Đối với mẫu 1,```
5 1 2 3
1 5
1 2 1 1 2
```Bắt đầu là vị trí`1`, và đích đến là vị trí`5`. giá trị`1`xảy ra ở các vị trí`1`,`3`, Và`4`, trong khi giá trị`2`xảy ra ở các vị trí`2`Và`5`. 

Con đường hữu ích là di chuyển từ vị trí`1`để định vị`3`, chi phí nào`2R = 4`, hoặc di chuyển đến vị trí`2`và sau đó dịch chuyển đến vị trí`5`, chi phí nào`R + C = 2 + 3 = 5`. Tuyến đường thứ hai tốt hơn là tiếp tục dọc theo mảng. 

Dấu vết Dijkstra đại diện là: 

| Nút xuất hiện | Khoảng cách | Thư giãn có liên quan | Khoảng cách mới | 
| --- | --- | --- | --- | 
| Vị trí 1 | 0 | Vị trí 2 | 2 | 
| Vị trí 1 | 0 | Hub cho giá trị 1 | 3 | 
| Vị trí 2 | 2 | Vị trí 3 | 4 | 
| Vị trí 2 | 2 | Hub cho giá trị 2 | 5 | 
| Hub cho giá trị 1 | 3 | Vị trí 3 | 3 | 
| Hub cho giá trị 1 | 3 | Vị trí 4 | 3 | 
| Vị trí 3 | 3 | Vị trí 4 | 3 | 
| Vị trí 4 | 3 | Vị trí 5 | 5 | 
| Hub cho giá trị 2 | 5 | Vị trí 5 | 5 | 

Câu trả lời là`5`. Dấu vết chứng minh rằng một trung tâm giá trị có thể giúp có thể tiếp cận được một vị trí ở xa có giá trị bằng nhau mà không cần lưu trữ rõ ràng tất cả các cạnh dịch chuyển theo cặp. 

Đối với mẫu 2,```
5 1 4 3
3 5
1 2 1 1 2
```Bắt đầu là vị trí`3`và đích đến là vị trí`5`. Giá trị tại vị trí`3`là`1`, và lần xuất hiện gần nhất của`2`là vị trí`2`, có thể dịch chuyển đến vị trí`5`. 

Di chuyển lùi lại từ vị trí`3`để định vị`2`chi phí`L = 1`. Bước vào trung tâm để tìm giá trị`2`chi phí`C = 3`, theo sau là quá trình chuyển đổi không tốn chi phí từ trung tâm đó sang vị trí`5`. 

| Nút xuất hiện | Khoảng cách | Thư giãn có liên quan | Khoảng cách mới | 
| --- | --- | --- | --- | 
| Vị trí 3 | 0 | Vị trí 2 | 1 | 
| Vị trí 3 | 0 | Vị trí 4 | 4 | 
| Hub cho giá trị 1 | 3 | Vị trí 1 | 3 | 
| Vị trí 2 | 1 | Vị trí 1 | 2 | 
| Vị trí 2 | 1 | Hub cho giá trị 2 | 4 | 
| Vị trí 1 | 2 | Hub cho giá trị 1 | 3 | 
| Hub cho giá trị 2 | 4 | Vị trí 5 | 4 | 
| Vị trí 5 | 4 | Đã đạt mục tiêu | 4 | 

Câu trả lời là`4`. Dấu vết này thể hiện chi phí di chuyển không đối xứng và cho thấy tại sao đường đi tối ưu có thể di chuyển ngược lại với hướng từ điểm xuất phát đến đích. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(N log N)`| Đồ thị nén có`O(N)`các đỉnh và các cạnh và Dijkstra xử lý chúng bằng một đống nhị phân. | 
| Không gian |`O(N)`| Các vị trí, trung tâm giá trị, danh sách lân cận, khoảng cách và hàng đợi ưu tiên đều chứa`O(N)`dữ liệu. | 

Với`N <= 10^5`, đồ thị nén có ít hơn`2N`đỉnh và nhỏ hơn`4N`các mục kề cận có hướng đến các thừa số không đổi. Điều này đủ nhỏ cho giới hạn bộ nhớ 256 MB, trong khi`O(N log N)`tránh hành vi bậc hai gây ra bằng cách kết nối rõ ràng các vị trí có giá trị bằng nhau. 

## Trường hợp thử nghiệm```python
import sys
import io
import heapq

def solve():
    input = sys.stdin.readline

    n, L, R, C = map(int, input().split())
    u, v = map(int, input().split())
    a = list(map(int, input().split()))

    value_id = {}
    groups = []

    for i, x in enumerate(a):
        if x not in value_id:
            value_id[x] = n + len(groups)
            groups.append([])
        groups[value_id[x] - n].append(i)

    m = n + len(groups)
    graph = [[] for _ in range(m)]

    for i in range(n - 1):
        graph[i].append((i + 1, R))
        graph[i + 1].append((i, L))

    for hub_index, positions in enumerate(groups):
        hub = n + hub_index
        for pos in positions:
            graph[pos].append((hub, C))
            graph[hub].append((pos, 0))

    start = u - 1
    target = v - 1

    INF = 10**30
    dist = [INF] * m
    dist[start] = 0

    pq = [(0, start)]

    while pq:
        d, node = heapq.heappop(pq)

        if d != dist[node]:
            continue

        if node == target:
            print(d)
            return

        for nxt, cost in graph[node]:
            nd = d + cost
            if nd < dist[nxt]:
                dist[nxt] = nd
                heapq.heappush(pq, (nd, nxt))

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

# Provided sample 1.
assert run("""\
5 1 2 3
1 5
1 2 1 1 2
""") == "5", "sample 1"

# Provided sample 2.
assert run("""\
5 1 4 3
3 5
1 2 1 1 2
""") == "4", "sample 2"

# Minimum-size input, start equals destination.
assert run("""\
2 5 7 3
2 2
1 2
""") == "0", "start already at destination"

# Maximum-size input and all values equal.
n = 100000
maximum_case = (
    f"{n} 1 1 1\n"
    f"1 {n}\n"
    + " ".join(["42"] * n)
    + "\n"
)
assert run(maximum_case) == "1", "maximum-size all-equal case"

# Boundary case, destination is to the left and backward movement is cheaper.
assert run("""\
2 7 2 100
2 1
1 2
""") == "7", "reverse direction"

# Matching values at the two boundaries, catching indexing and teleport errors.
assert run("""\
3 10 10 2
1 3
5 1 5
""") == "2", "boundary teleport"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 5 7 3 / 2 2 / 1 2`|`0`| Kích thước tối thiểu và`u == v`| 
|`100000 1 1 1 / 1 100000 / all values 42`|`1`| Kích thước tối đa, tất cả các giá trị bằng nhau và trung tâm dịch chuyển được nén | 
|`2 7 2 100 / 2 1 / 1 2`|`7`| Điểm đến bên trái và chi phí di chuyển không đối xứng | 
|`3 10 10 2 / 1 3 / 5 1 5`|`2`| Dịch chuyển tức thời giữa vị trí đầu tiên và cuối cùng, bao gồm cả việc lập chỉ mục ranh giới | 

## Vỏ cạnh 

Khi nào`u == v`, đường đi ngắn nhất có chi phí bằng 0 bất kể giá trị mảng hay chi phí di chuyển. Ví dụ,```
2 5 7 3
2 2
1 2
```cả điểm bắt đầu và mục tiêu đều đề cập đến vị trí`2`. Dijkstra bắt đầu bằng`dist[2] = 0`, ngay lập tức bật mục tiêu đó và in`0`. Không cần dịch chuyển hoặc di chuyển. 

Khi tuyến đường hữu ích duy nhất là dịch chuyển tức thời qua mảng, trung tâm giá trị sẽ ngăn việc triển khai yêu cầu ranh giới rõ ràng giữa mọi cặp vị trí bằng nhau. Vì```
3 10 10 2
1 3
5 1 5
```chức vụ`1`đi vào trung tâm để lấy giá trị`5`với chi phí`2`, và hub đạt đến vị trí`3`về chi phí`0`. Câu trả lời là`2`, mặc dù tuyến đường thông thường sẽ tốn kém`20`. 

Khi mục tiêu nằm ở bên trái, thuật toán phải sử dụng chi phí lùi thay vì chi phí chuyển tiếp. TRONG```
2 7 2 100
2 1
1 2
```Dijkstra xuất phát ở vị trí`2`và thư giãn tư thế`1`với chi phí`L = 7`. Vì dịch chuyển tức thời đắt hơn nhiều và không có giá trị phù hợp nào hữu ích nên câu trả lời cuối cùng là`7`. 

Khi mọi vị trí đều có cùng giá trị, việc xây dựng dịch chuyển tức thời rõ ràng sẽ yêu cầu không gian bậc hai. Đối với trường hợp kích thước tối đa với`100000`giá trị bằng nhau và`C = 1`, biểu đồ nén chỉ chứa một trung tâm giá trị. Mọi vị trí đều kết nối với trung tâm duy nhất đó, vì vậy vị trí`1`đạt đến vị trí`100000`với chi phí`1`. Câu trả lời là`1`và đồ thị vẫn có kích thước tuyến tính. 

Vị trí đầu tiên và cuối cùng cũng là các đỉnh đồ thị thông thường nên không có cạnh chuyển động đặc biệt nào được tạo ra bên ngoài mảng. Việc xây dựng thêm một cạnh phía trước chỉ dành cho`i < N`và một cạnh lùi chỉ dành cho cùng một cặp liền kề. Điều này tránh được cả các chuyển đổi ngoài giới hạn và lỗi thường gặp là nhầm lẫn giữa các vị trí đầu vào dựa trên một với các chỉ số Python dựa trên 0.
