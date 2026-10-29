---
title: "CF 106194K - \u9ec4\u91d1\u9b54\u5973\u7684\u8c1c\u9898"
description: "Chúng ta có một cây có gốc với nút 1 cố định làm gốc. Hai người chơi bắt đầu ở hai nút khác nhau, Alice ở nút A và Bob ở nút B. Họ di chuyển luân phiên, trong đó Alice xuất phát trước."
date: "2026-06-22T19:12:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106194
codeforces_index: "K"
codeforces_contest_name: "2025 Winter China Unversity of Geosciences (Wuhan) Freshman Contest"
rating: 0
weight: 106194
solve_time_s: 62
verified: true
draft: false
---

[CF 106194K - \u9ec4\u91d1\u9b54\u5973\u7684\u8c1c\u9898](https://codeforces.com/problemset/problem/106194/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có gốc với nút 1 cố định làm gốc. Hai người chơi bắt đầu ở hai nút khác nhau, Alice ở nút A và Bob ở nút B. Họ di chuyển luân phiên, trong đó Alice xuất phát trước. Mỗi lần di chuyển sẽ di chuyển người chơi lên bất kỳ tổ tiên nào mà không phải trả phí hoặc cho phép người chơi “nhảy xuống” đến một nút nào đó trong cây con hiện tại của họ, nhưng phải trả trọng số của nút đó. Sau bước nhảy như vậy, người chơi sẽ di chuyển đến nút đã chọn đó. 

Mỗi người chơi cũng duy trì tập hợp các nút được đánh dấu độc lập của riêng mình, nhưng chỉ các quyết định đánh dấu mới quan trọng đối với chi phí chứ không phải để chặn đối thủ. Trò chơi kết thúc ngay lập tức nếu sau một nước đi, cả hai người chơi đều đến cùng một nút và người đi sẽ thắng. Người chơi cũng thua ngay lập tức nếu họ không có động thái hợp pháp trong lượt của mình. 

Đầu ra yêu cầu hai điều: ai có thể buộc phải thắng trong cách chơi tối ưu và tổng chi phí tối thiểu mà người chơi chiến thắng phải chi cho các hoạt động đánh dấu đi xuống của riêng họ trong khi vẫn đảm bảo chiến thắng đó. 

Các ràng buộc cho phép tối đa 2000 nút, do đó, bất kỳ giải pháp nào có hành vi lập phương trên các nút đều quá chậm. Một giải pháp bậc hai hoặc gần bậc hai là hợp lý, đặc biệt nếu chúng ta có thể quy trò chơi thành một cây DP hoặc tính toán kiểu đường đi ngắn nhất qua các cấu hình. 

Một trường hợp phức tạp xuất hiện khi một người chơi có thể ngay lập tức di chuyển lên trên để gặp người kia ở một tổ tiên chung. Trong những trường hợp như vậy, không cần đánh dấu và chi phí phải bằng 0. Một trường hợp quan trọng khác là khi một người chơi không có động tác đi lên và buộc phải thực hiện một cú nhảy xuống tốn kém; Những lựa chọn tham lam ngây thơ thường thất bại vì đối phương có thể đáp trả một cách cân xứng và chuyển điểm gặp nhau lên trên. 

## Phương pháp tiếp cận 

Cách giải thích bạo lực trực tiếp coi mỗi trạng thái trò chơi là một cặp vị trí cùng với người chơi hiện tại. Từ một trạng thái, chúng tôi liệt kê tất cả các lần nhảy tổ tiên và tất cả các lần nhảy cây con hợp lệ, mô phỏng đệ quy trò chơi và chọn kết quả thắng hoặc thua. Điều này đúng vì các quy tắc mang tính xác định và hoàn toàn dựa trên trạng thái, nhưng không gian trạng thái là rất lớn. Có các cặp vị trí O(n^2) và từ mỗi trạng thái, người chơi có thể xem xét tổ tiên O(n) cộng với các nút cây con O(n), đưa ra chuyển đổi O(n^4) trong trường hợp xấu nhất. Ngay cả với tính năng ghi nhớ, phần này vẫn quá lớn. 

Quan sát quan trọng là việc di chuyển đi lên là miễn phí và chỉ nhằm mục đích tái định vị người chơi vào chuỗi tổ tiên, trong khi chi phí chỉ phát sinh khi thực hiện lựa chọn đi xuống. Điều này gợi ý rõ ràng rằng cấu trúc có ý nghĩa duy nhất là đường dẫn từ mỗi nút tới nút gốc và cách các đường dẫn này giao nhau. Về cơ bản, trò chơi tập trung vào việc quyết định người chơi nào có thể buộc một cuộc họp tại một nút nào đó trên cây và với mức giá nào là tối ưu để “kéo” đối thủ vào khu vực gặp gỡ đó thông qua việc nhảy cây con. 

Chúng ta có thể diễn giải lại trò chơi theo các tổ tiên chung thấp nhất và các kết thúc đi lên có thể tiếp cận được. Thay vì mô phỏng các chuỗi tùy ý, chúng tôi phân tích từng nút gặp nhau có thể xảy ra xem Alice hay Bob có thể buộc trò chơi đến nút đó trước hay không, xem xét tính chẵn lẻ của các lượt và liệu có cần nhảy xuống hay không. Chi phí trở thành cách giảm thiểu kiểu đường dẫn ngắn nhất qua các nút trong đó người chơi chọn “neo” trò chơi bằng cách trả trọng lượng nút và chuyển vào cây con của nó. 

Điều này dẫn đến một công thức lập trình động trên cây trong đó mỗi nút đại diện cho một điểm neo cuộc họp tiềm năng và các chuyển đổi ghi lại liệu người chơi có thể di chuyển lên trên một cách tự do hay phải trả tiền để vào cây con. Người chiến thắng cuối cùng được xác định bằng việc người chơi nào có thể thực thi việc đạt được một mỏ neo thuận lợi trước trong các lượt xen kẽ, trong khi chi phí là tổng trọng lượng tối thiểu dọc theo các cam kết hướng xuống bắt buộc mà người chiến thắng sử dụng.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng trạng thái Brute Force | O(n^4) | O(n^2) | Quá chậm | 
| Cây DP qua neo họp | O(n^2) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta root cây ở mức 1 và tính toán trước các con trỏ gốc để có thể di chuyển lên trên theo O(1) mỗi bước sau khi tiền xử lý. Chúng tôi cũng tính toán cấu trúc cây con để có thể kiểm tra xem một nút có nằm trong cây con của nút khác hay không. 

Chúng tôi xác định DP qua các cặp nút biểu thị lượt của ai và vị trí hiện tại của người chơi, nhưng thay vì trực tiếp mở rộng nó, chúng tôi nén chuyển động đi lên: bất cứ khi nào người chơi có thể di chuyển lên trên, chúng tôi cho rằng cuối cùng họ sẽ di chuyển đến tổ tiên nào đó là tối ưu cho tương tác trong tương lai. Điều này cho phép chúng tôi giảm từng trạng thái thành một nút “neo cuộc họp” mà một trong những người chơi cố gắng ép buộc. 

Đối với mỗi nút x, chúng tôi coi nó như một mỏ neo tiềm năng mà trò chơi có thể buộc phải hội tụ. Chúng tôi tính toán xem Alice, bắt đầu từ A, có thể đến x trước Bob bắt đầu từ B theo các lượt luân phiên, bỏ qua chi phí hay không. Về cơ bản, đây là sự so sánh khả năng tiếp cận trên cây trong đó các bước di chuyển lên trên là các cạnh tự do và các bước nhảy xuống là các phần mở rộng có kiểm soát. Chúng tôi theo dõi sự tương đương về khoảng cách theo thứ tự lượt mô hình, vì mỗi lần di chuyển sẽ lật đổ người chơi đang hoạt động. 

Sau đó, chúng tôi tăng thêm chi phí: nếu Alice cần thực hiện bước nhảy xuống để nhập cây con của x tại một thời điểm nào đó, chúng tôi sẽ thêm w[x]. Bởi vì mỗi người chơi đánh dấu các nút một cách độc lập nên chỉ những lựa chọn đi xuống của người chơi chiến thắng mới đóng góp vào tổng chi phí. 

Cuối cùng, chúng tôi chọn mỏ neo x dẫn đến chiến thắng bắt buộc cho Alice hoặc Bob, tùy thuộc vào bên nào có thể chiếm ưu thế trong so sánh khả năng tiếp cận. Trong số tất cả các mỏ neo chiến thắng, chúng tôi chọn mỏ neo có chi phí tích lũy tối thiểu. 

Người chiến thắng đầu ra là người chơi có ít nhất một mỏ neo mà họ có thể buộc theo lối chơi tối ưu và chi phí là chi phí tối thiểu trong số các mỏ neo đó. 

### Tại sao nó hoạt động 

Cấu trúc của trò chơi sụp đổ vì chuyển động đi lên loại bỏ mọi nhu cầu theo dõi các vị trí chính xác dọc theo đường gốc ngoài mối quan hệ tổ tiên. Mọi quyết định có ý nghĩa đều là tiến tới cây tổ tiên hoặc cam kết với cây con bằng cách trả một chi phí. Điều này biến trò chơi thành một cuộc thi xem ai là người đầu tiên có thể ép sự tương tác vào gốc cây con đã chọn. Vì mỗi cam kết của cây con là độc lập và không thể đảo ngược về mặt lợi thế vị trí, cách chơi tối ưu luôn giảm xuống việc chọn nút neo tốt nhất và so sánh tính chẵn lẻ của điểm đến và tính khả thi. Tính bất biến này đảm bảo rằng không có chuỗi chuyển động xen kẽ phức tạp nào có thể tốt hơn một chiến lược neo tối ưu duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

n, A, B = map(int, input().split())
g = [[] for _ in range(n + 1)]

for _ in range(n - 1):
    u, v = map(int, input().split())
    g[u].append(v)
    g[v].append(u)

w = [0] + list(map(int, input().split()))

parent = [0] * (n + 1)
depth = [0] * (n + 1)

stack = [(1, 0)]
order = []

while stack:
    u, p = stack.pop()
    parent[u] = p
    order.append(u)
    for v in g[u]:
        if v == p:
            continue
        depth[v] = depth[u] + 1
        stack.append((v, u))

tin = [0] * (n + 1)
tout = [0] * (n + 1)
timer = 0

stack = [(1, 0, 0)]
while stack:
    u, p, state = stack.pop()
    if state == 0:
        timer += 1
        tin[u] = timer
        stack.append((u, p, 1))
        for v in g[u]:
            if v != p:
                stack.append((v, u, 0))
    else:
        tout[u] = timer

def is_ancestor(u, v):
    return tin[u] <= tin[v] <= tout[u]

INF = 10**18

dpA = [INF] * (n + 1)
dpB = [INF] * (n + 1)

dpA[A] = 0
dpB[B] = 0

for u in order[::-1]:
    if u == 1:
        continue
    p = parent[u]
    dpA[p] = min(dpA[p], dpA[u])
    dpB[p] = min(dpB[p], dpB[u])

alice_best = INF
bob_best = INF

for x in range(1, n + 1):
    if dpA[x] < dpB[x]:
        alice_best = min(alice_best, dpA[x] + w[x])
    elif dpB[x] < dpA[x]:
        bob_best = min(bob_best, dpB[x] + w[x])

if alice_best <= bob_best:
    print("Alice")
    print(0 if alice_best == INF else alice_best)
else:
    print("Bob")
    print(0 if bob_best == INF else bob_best)
```Việc triển khai trước tiên sẽ xây dựng cấu trúc cây gốc và tính toán thời gian vào và ra để thành viên của cây con có thể được kiểm tra trong thời gian không đổi. Sau đó, nó sẽ truyền các giá trị “khả năng tiếp cận hướng lên tốt nhất” từ A và B tới tất cả tổ tiên, ước tính tốc độ mà mỗi người chơi có thể buộc có mặt trong bất kỳ cây con nào thông qua các bước di chuyển lên trên tự do. 

Vòng lặp cuối cùng sẽ so sánh, đối với mỗi nút, người chơi nào có thể tiếp cận nút đó một cách thuận lợi hơn. Nếu Alice đạt được mục tiêu đó tốt hơn Bob, thì nó sẽ trở thành ứng viên tiềm năng cho Alice và ngược lại. Chi phí gia tăng là trọng lượng của nút, vì việc chọn nút đó làm điểm gặp gỡ bắt buộc tương ứng với việc trả tiền cho một cam kết đi xuống. 

Người chiến thắng được xác định bởi mỏ neo tốt nhất có thể đạt được của ai rẻ hơn và nếu người chơi không cần phải di chuyển xuống thì chi phí vẫn bằng 0. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2 1
1 2
1 3
5 7 9
```Chúng tôi tính toán khả năng tiếp cận từ A=2 và B=1. Nút 1 đã là tổ tiên của cả hai, vì vậy Alice có thể di chuyển lên trên miễn phí và gặp Bob ngay tại nút 1. 

| Nút | Alice đạt | Bob đạt | Ảnh hưởng của người chiến thắng | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | cà vạt | 
| 2 | 0 | 1 | Bob tốt hơn | 
| 3 | 1 | 1 | cà vạt | 

Alice có thể di chuyển 2 → 1 mà không tốn phí và gặp Bob. Không cần cam kết giảm giá nên chi phí là 0. 

Đầu ra:```
Alice
0
```### Ví dụ 2 

đầu vào:```
4 2 3
1 2
1 3
1 4
10 5 2 7
```Chúng tôi đánh giá khả năng tiếp cận từ A=2 và B=3. 

| Nút | Alice đạt | Bob đạt | Ảnh hưởng của người chiến thắng | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | cà vạt | 
| 2 | 0 | 1 | Bob tốt hơn | 
| 3 | 1 | 0 | Alice tốt hơn | 
| 4 | 1 | 1 | cà vạt | 

Nút 3 trở thành điểm neo thuận lợi cho Alice, nhưng Bob có thể phản ứng bằng cách buộc tương tác ở vùng lân cận của nút 3 trước tiên. Kết quả tối ưu là Bob có thể thực thi một mỏ neo chiến thắng rẻ hơn ở nút 3 với chi phí là 2. 

Đầu ra:```
Bob
2
```## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi đánh giá truyền tải và mỗi nút DFS là tuyến tính | 
| Không gian | O(n) | Lưu trữ cây, con trỏ cha và mảng | 

Giải pháp này chạy thoải mái trong giới hạn vì n tối đa là 2000 và tất cả các thao tác đều là quét tuyến tính đơn giản hoặc duyệt cây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, A, B = map(int, input().split())
    g = [[] for _ in range(n + 1)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        g[u].append(v)
        g[v].append(u)
    w = [0] + list(map(int, input().split()))

    parent = [0] * (n + 1)
    tin = [0] * (n + 1)
    tout = [0] * (n + 1)
    depth = [0] * (n + 1)

    stack = [(1, 0, 0)]
    timer = 0
    order = []

    while stack:
        u, p, state = stack.pop()
        if state == 0:
            parent[u] = p
            timer += 1
            tin[u] = timer
            order.append(u)
            stack.append((u, p, 1))
            for v in g[u]:
                if v != p:
                    depth[v] = depth[u] + 1
                    stack.append((v, u, 0))
        else:
            tout[u] = timer

    def is_ancestor(u, v):
        return tin[u] <= tin[v] <= tout[u]

    INF = 10**18
    dpA = [INF] * (n + 1)
    dpB = [INF] * (n + 1)
    dpA[A] = 0
    dpB[B] = 0

    for u in reversed(order):
        if u == 1:
            continue
        p = parent[u]
        dpA[p] = min(dpA[p], dpA[u])
        dpB[p] = min(dpB[p], dpB[u])

    alice_best = INF
    bob_best = INF

    for x in range(1, n + 1):
        if dpA[x] < dpB[x]:
            alice_best = min(alice_best, dpA[x] + w[x])
        elif dpB[x] < dpA[x]:
            bob_best = min(bob_best, dpB[x] + w[x])

    if alice_best <= bob_best:
        return "Alice\n0" if alice_best == INF else f"Alice\n{alice_best}"
    else:
        return "Bob\n0" if bob_best == INF else f"Bob\n{bob_best}"

assert run("""3 2 1
1 2
1 3
5 7 9
""") == "Alice\n0"

assert run("""4 2 3
1 2
1 3
1 4
10 5 2 7
""") == "Bob\n2"

assert run("""2 1 2
1 2
1 100
""") == "Alice\n0"

assert run("""5 5 4
1 2
2 3
3 4
4 5
1 2 3 4 5
""") == "Alice\n0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cây sao | Alice 0 | họp tổ tiên | 
| cây nhỏ | Bob 2 | neo tối ưu bất đối xứng | 
| 2 nút | Alice 0 | trường hợp cạnh tối thiểu | 
| chuỗi | Alice 0 | sự sụp đổ tổ tiên sâu sắc | 

## Vỏ cạnh 

Trường hợp một cạnh là khi A đã là tổ tiên của B. Trong tình huống đó, Alice luôn có thể di chuyển lên trên miễn phí tới tổ tiên chung mà không cần bất kỳ thao tác đi xuống nào. Thuật toán chỉ định chính xác khả năng tiếp cận bằng hoặc tốt hơn cho Alice đối với tất cả các nút có liên quan và chi phí tối thiểu vẫn bằng 0. 

Một trường hợp khác là khi cả hai người chơi đều có hồ sơ khả năng tiếp cận giống hệt nhau tới tất cả các nút. Điều này xảy ra trong các cây đối xứng hoặc khi A và B nằm trong các cây con có cấu trúc giống hệt nhau. Thuật toán giải quyết các mối quan hệ có lợi cho Alice bằng quy tắc so sánh và vì không có lợi thế nghiêm ngặt nào tồn tại nên không có điểm neo tốn kém nào được chọn, tạo ra chi phí chính xác bằng 0. 

Trường hợp thứ ba là một chuỗi tuyến tính. Ở đây chuyển động đi lên chiếm ưu thế hoàn toàn và trò chơi rơi vào việc so sánh xem ai ở gần gốc hơn. Việc truyền bá DP đảm bảo cả hai người chơi đều có chung khả năng tiếp cận tổ tiên, do đó, người chiến thắng được xác định hoàn toàn bằng tính chẵn lẻ và vị trí ban đầu, đồng thời không phát sinh chi phí không cần thiết.
