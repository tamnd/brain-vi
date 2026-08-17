---
title: "CF 102277G - Cơn sốt World Cup"
description: "Bài toán đặt hai đội bóng đá vào mặt phẳng Descartes. Mỗi đội có (n) người chơi nên có tổng cộng (2n) điểm khác nhau. Vị trí của người chơi không bao giờ thay đổi."
date: "2026-08-16T19:37:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102277
codeforces_index: "G"
codeforces_contest_name: "UCF Locals 2018"
rating: 0
weight: 102277
solve_time_s: 98
verified: true
draft: false
---

[CF 102277G - Cơn sốt World Cup](https://codeforces.com/problemset/problem/102277/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 38 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Bài toán đặt hai đội bóng đá vào mặt phẳng Descartes. Mỗi đội có (n) người chơi nên có tổng cộng (2n) điểm khác nhau. Vị trí của người chơi không bao giờ thay đổi. 

Một cầu thủ chỉ có thể chuyền bóng trực tiếp cho một cầu thủ khác khi cả hai cầu thủ đều thuộc cùng một đội và không có cầu thủ nào khác của cả hai đội nằm hoàn toàn giữa họ trên đoạn thẳng nối hai cầu thủ. Khoảng cách vượt qua không thành vấn đề. 

Bóng bắt đầu từ Cầu thủ 1 của Đội 1 và mục tiêu là đến được Cầu thủ (n) của Đội 1 bằng cách sử dụng càng ít đường chuyền càng tốt. Nếu không có chuỗi đường chuyền hợp lệ nào đến được mục tiêu thì câu trả lời là (-1). 

Tuyên bố cuộc thi ban đầu đưa ra (2 \le n \le 11), trong khi mọi tọa độ đều là số nguyên từ 1 đến 999 và tất cả (2n) vị trí của người chơi đều khác biệt. Giới hạn thời gian là 1 giây và giới hạn bộ nhớ là 256 MB. Vì có tối đa 22 đỉnh nên ngay cả thuật toán (O(n^3)) cũng chỉ thực hiện được vài nghìn phép kiểm tra hình học. Vấn đề thực sự không phải là kích thước của biểu đồ mà là nhận ra rằng các quy tắc chuyển tạo thành bài toán đường đi ngắn nhất thay vì yêu cầu tìm kiếm hình học. 

Có một số trường hợp nghiêm trọng mà việc triển khai bất cẩn có thể thất bại. Đầu tiên, một đối thủ ở giữa hai đồng đội sẽ chặn đường chuyền. Ví dụ,```
2
1 1 3 1
2 1 2 2
```có Đội 1 ở ((1,1)) và ((3,1)), với đối thủ ở ((2,1)). Đầu ra đúng là```
-1
```bởi vì đường chuyền duy nhất có thể có của Đội 1 đã bị chặn. Một thử nghiệm chỉ kiểm tra xem ba điểm có thẳng hàng hay không mà không kiểm tra xem điểm thứ ba có nằm giữa các điểm cuối hay không, có thể từ chối hoặc chấp nhận phân đoạn sai tùy thuộc vào việc triển khai nó. 

Thứ hai, một đồng đội giữa hai cầu thủ cũng chặn đường chuyền trực tiếp, nhưng đồng đội đó có thể trở thành đỉnh trung gian. Ví dụ,```
3
1 1 3 1 5 1
1 3 3 3 5 3
```có người chơi Đội 1 tại ((1,1),(3,1),(5,1)). Người chơi 1 không thể chuyền trực tiếp cho Người chơi 3 vì Người chơi 2 ở giữa họ, nhưng Người chơi 1 có thể chuyền cho Người chơi 2 và sau đó Người chơi 2 có thể chuyền cho Người chơi 3. Kết quả đúng là```
2
```Một giải pháp bất cẩn coi "có một đồng đội giữa họ" có nghĩa là mục tiêu không thể tiếp cận được sẽ bỏ lỡ con đường này. 

Thứ ba, chỉ cộng tác thôi là chưa đủ. Một cầu thủ nằm trên cùng một đường vô hạn nhưng nằm ngoài đoạn đường chuyền không chặn được đường chuyền. Ví dụ,```
2
1 1 3 1
5 1 5 2
```không có trình phát nào ở giữa ((1,1)) và ((3,1)), vì vậy đầu ra đúng là```
1
```Giải pháp chỉ kiểm tra tích chéo và quên điều kiện "giữa" sẽ khai báo sai đường chuyền này bị chặn. 

## Phương pháp tiếp cận 

Ý tưởng bạo lực trực tiếp nhất là liệt kê mọi chuỗi đường chuyền có thể có từ Người chơi 1 cho đến khi đạt được Người chơi (n). Điều này đúng vì mọi nghiệm pháp lý đều chính xác là một dãy các cạnh đồ thị như vậy. Vì việc lặp lại một đỉnh không bao giờ có thể cải thiện được đường đi ngắn nhất nên chúng ta có thể hạn chế sự chú ý vào những đường đi đơn giản. Với tối đa 22 người chơi, có thể có tới 

[ 
\sum_{k=0}^{20} P(20,k) 
] 

các đường đi đơn giản khác nhau từ điểm bắt đầu đến đích, trong đó (P(20,k)=20!/(20-k)!). Số lượng này vào cỡ (20!), xấp xỉ (6,6\cdot10^{18}), vì vậy việc liệt kê các đường dẫn là hoàn toàn không thực tế. 

Brute-force hoạt động vì nó khám phá chính xác các chuỗi có thể đi qua, nhưng không thành công vì số lượng chuỗi tăng theo giai thừa. Quan sát quan trọng là phần hình học của bài toán chỉ cần thiết để xác định xem có tồn tại đường truyền trực tiếp hay không. Một khi những đường đi hợp pháp đó được biết đến, vấn đề sẽ trở thành một vấn đề về đường đi ngắn nhất không có trọng số thông thường. 

Chúng ta có thể tạo một đỉnh đồ thị cho mọi người chơi. Nối hai đỉnh khi những người chơi tương ứng là đồng đội và không có người chơi nào nằm hoàn toàn giữa chúng. Mỗi đường chuyền hợp pháp có giá chính xác là một, vì vậy số đường chuyền tối thiểu chính xác là khoảng cách đường đi ngắn nhất giữa Người chơi 1 của Đội 1 và Người chơi của Đội 1 (n). 

Vì có nhiều nhất 22 đỉnh nên chúng ta có thể chỉ cần kiểm tra từng cặp người chơi và quét từng người chơi thứ ba để quyết định xem cặp đó có được kết nối hay không. Việc này mất (O(n^3)) thời gian. Sau khi đồ thị được xây dựng, BFS tìm đường đi ngắn nhất trong (O(n^2)), không đáng kể so với tiền xử lý hình học. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(20!)) Khám phá đường dẫn trong trường hợp xấu nhất | (O(n)) độ sâu đệ quy | Quá chậm | 
| Tối ưu | (O(n^3)) | (O(n^2)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ tất cả (2n) người chơi trong một mảng. (n) người chơi đầu tiên thuộc về Đội 1 và (n) người chơi còn lại thuộc về Đội 2. Điều này cho phép mọi kiểm tra hình học hoạt động với một sơ đồ lập chỉ mục chung. 
2. Đối với mỗi cặp người chơi riêng biệt (u,v), trước tiên hãy kiểm tra xem họ có thuộc cùng một đội hay không. Nếu họ ở các đội khác nhau, không bao giờ có thể có lợi thế giữa họ vì đường chuyền chỉ được phép giữa các đồng đội. 
3. Đối với một cặp cùng đội, hãy kiểm tra mọi người chơi khác (w). Tính tích chéo 

[ 
(v-u)\times(w-u). 
] 

Nếu nó khác 0 thì (w) không nằm trên đường đi qua (u) và (v) nên không thể chặn đường chuyền. 

1. Khi tích chéo bằng 0, kiểm tra xem (w) có nằm trong đoạn từ (u) đến (v) hay không. Với các điểm khác nhau, việc kiểm tra xem tọa độ của nó có nằm trong phạm vi tọa độ của các điểm cuối hay không là đủ sau khi đã thiết lập được sự cộng tuyến. Nếu một cầu thủ như vậy tồn tại, đường chuyền trực tiếp sẽ bị chặn. 
2. Nếu không có người chơi nào chặn đoạn đó, hãy thêm một cạnh đồ thị vô hướng giữa (u) và (v). Việc truyền hoạt động theo một trong hai hướng, do đó đồ thị là vô hướng. 
3. Chạy BFS từ đỉnh 0, đại diện cho Người chơi 1 của Đội 1. Khởi tạo khoảng cách của nó về 0 và mọi khoảng cách khác thành (-1). Khi BFS lần đầu tiên đến một đỉnh, khoảng cách của nó là số lần vượt qua tối thiểu cần thiết để đến đỉnh đó vì mọi cạnh của đồ thị đều có cùng chi phí. 
4. Trả về khoảng cách của đỉnh (n-1), đại diện cho Người chơi Đội 1 (n). Nếu BFS không bao giờ đạt tới giá trị đó thì giá trị được lưu trữ vẫn là (-1), đây chính xác là kết quả được yêu cầu. 

Tại sao nó hoạt động: biểu đồ chứa một cạnh chính xác khi đường chuyền tương ứng là hợp pháp. Do đó, mỗi chuỗi các bước đi hợp lệ tương ứng với một đường dẫn đồ thị và mọi đường dẫn đồ thị tương ứng với một chuỗi các bước đi hợp lệ. Vì mỗi đường chuyền có giá bằng một, nên việc giảm thiểu số đường chuyền giống hệt với việc tìm đường đi đồ thị không có trọng số ngắn nhất. BFS trả về khoảng cách ngắn nhất đó, do đó thuật toán không thể tạo ra câu trả lời hợp lệ nhỏ hơn hoặc lớn hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve_case(n, team1, team2):
    points = team1 + team2
    total = 2 * n

    graph = [[] for _ in range(total)]

    def blocked(a, b):
        ax, ay = points[a]
        bx, by = points[b]

        dx = bx - ax
        dy = by - ay

        for c in range(total):
            if c == a or c == b:
                continue

            cx, cy = points[c]

            cross = dx * (cy - ay) - dy * (cx - ax)
            if cross != 0:
                continue

            if min(ax, bx) <= cx <= max(ax, bx) and \
               min(ay, by) <= cy <= max(ay, by):
                return True

        return False

    for i in range(total):
        for j in range(i + 1, total):
            same_team = (i < n) == (j < n)
            if not same_team:
                continue

            if not blocked(i, j):
                graph[i].append(j)
                graph[j].append(i)

    dist = [-1] * total
    dist[0] = 0
    queue = [0]
    head = 0

    while head < len(queue):
        u = queue[head]
        head += 1

        if u == n - 1:
            return dist[u]

        for v in graph[u]:
            if dist[v] == -1:
                dist[v] = dist[u] + 1
                queue.append(v)

    return -1

def solve():
    data = list(map(int, sys.stdin.buffer.read().split()))
    pos = 0

    n = data[pos]
    pos += 1

    team1 = []
    for _ in range(n):
        x = data[pos]
        y = data[pos + 1]
        pos += 2
        team1.append((x, y))

    team2 = []
    for _ in range(n):
        x = data[pos]
        y = data[pos + 1]
        pos += 2
        team2.append((x, y))

    print(solve_case(n, team1, team2))

if __name__ == "__main__":
    solve()
```Việc triển khai đầu tiên kết hợp cả hai nhóm vào`points`. Chỉ mục`0`bởi vì`n - 1`là Đội 1, trong khi chỉ số`n`bởi vì`2 * n - 1`là Đội 2. Điều này làm cho việc nhận dạng đội của người chơi trở thành một sự so sánh đơn giản với`n`. 

các`blocked`hàm thực hiện phần hình học của thuật toán. Tích chéo chỉ sử dụng số học số nguyên, do đó không có vấn đề về độ chính xác của dấu phẩy động. Với tọa độ được giới hạn bởi 999, các giá trị trung gian rất nhỏ trong Python và cũng sẽ vừa khít với các số nguyên 64 bit tiêu chuẩn. 

Việc kiểm tra phạm vi chỉ diễn ra sau`cross == 0`. Thứ tự này quan trọng vì bản thân việc kiểm tra phạm vi tọa độ không xác định được rằng điểm nằm trên đoạn đó. Cộng tuyến cộng với việc nằm trong cả hai phạm vi tọa độ mang lại chính xác điều kiện "nghiêm ngặt giữa" cần thiết, vì bản thân các điểm cuối bị bỏ qua một cách rõ ràng. 

Việc xây dựng đồ thị kiểm tra từng cặp không có thứ tự một lần bằng cách sử dụng`j`từ`i + 1`trở đi. Bất cứ khi nào đường chuyền hợp pháp, cả hai hướng đều được chèn vào vì đường chuyền giữa hai đồng đội có thể đảo ngược được. 

BFS sử dụng danh sách cộng với số nguyên`head`thay vì liên tục loại bỏ phần tử đầu tiên. Việc xóa khỏi đầu danh sách Python có giá (O(n)), trong khi tiến lên`head`giữ cho mỗi hoạt động hàng đợi có thời gian không đổi. Việc trả về sớm khi mục tiêu được loại bỏ là hợp lệ vì BFS xử lý các đỉnh theo thứ tự khoảng cách không giảm. 

Câu lệnh đầu vào chứa một trường hợp kiểm thử chứ không phải số lượng trường hợp kiểm thử, do đó giải pháp đọc chính xác một giá trị của (n) và hai danh sách tọa độ. Việc sử dụng`sys.stdin.buffer.read()`ở đây an toàn và tiếp tục phân tích cú pháp nhanh chóng. 

## Ví dụ đã hoạt động 

Tuyên bố ban đầu của cuộc thi năm 2018 không cung cấp các cặp đầu vào/đầu ra mẫu, vì vậy các dấu vết sau đây sử dụng các ví dụ được xây dựng để thực hiện việc xây dựng biểu đồ và BFS. 

### Ví dụ 1```
3
1 1 3 1 5 1
1 3 3 3 5 3
```Đội 1 gồm 3 người chơi trên hàng ngang (y=1). Người chơi ở giữa chặn đường chuyền trực tiếp từ Người chơi 1 sang Người chơi 3. 

| Người chơi đã xử lý | Lợi thế pháp lý mới | Khoảng cách | 
| --- | --- | --- | 
| 1 | 1 -> 2 | 0 | 
| 2 | 2 -> 3 | 1 | 
| 3 | đạt mục tiêu | 2 | 

Không có cạnh trực tiếp từ Người chơi 1 đến Người chơi 3 vì Người chơi 2 nằm chặt chẽ giữa họ. BFS thay vào đó làm theo`1 -> 2 -> 3`, tạo ra tối thiểu hai lượt. Các cầu thủ của đội thứ hai đứng trên một đường thẳng song song nên không chặn bất kỳ đoạn nào của Đội 1 nằm ngang này. 

### Ví dụ 2```
2
1 1 3 1
2 1 2 2
```Hai người chơi duy nhất của Đội 1 là ((1,1)) và ((3,1)). Người chơi của Đội 2 tại ((2,1)) nằm ngay giữa họ. 

| Cặp cầu thủ | Cùng một đội | Bị chặn | Cạnh | 
| --- | --- | --- | --- | 
| 1, 2 | Có | Có, đối thủ ở (2,1) | Không | 

BFS bắt đầu ở Người chơi 1, không tìm thấy cạnh nào đi ra và mục tiêu vẫn ở khoảng cách (-1). Điều này chứng tỏ rằng một cầu thủ của đội kia chặn đường chuyền hiệu quả như đồng đội. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n^3)) | Có (O(n^2)) cặp người chơi và mỗi cặp sẽ kiểm tra tối đa (2n) trình chặn có thể. | 
| Không gian | (O(n^2)) | Biểu đồ đi qua chứa tối đa (O(n^2)) cạnh và mảng BFS là tuyến tính. | 

Với (n \le 11), có nhiều nhất 22 người chơi, do đó việc xây dựng hình học thực hiện tối đa vài nghìn lượt kiểm tra chặn. Giới hạn (O(n^3)) dễ dàng nằm trong giới hạn thời gian 1 giây và biểu đồ (O(n^2)) chỉ sử dụng một phần rất nhỏ trong giới hạn bộ nhớ 256 MB. 

## Trường hợp thử nghiệm```python
# helper: run the solution logic on an input string
import sys
import io

def solve_case(n, team1, team2):
    points = team1 + team2
    total = 2 * n

    graph = [[] for _ in range(total)]

    def blocked(a, b):
        ax, ay = points[a]
        bx, by = points[b]

        dx = bx - ax
        dy = by - ay

        for c in range(total):
            if c == a or c == b:
                continue

            cx, cy = points[c]
            cross = dx * (cy - ay) - dy * (cx - ax)

            if cross == 0 and \
               min(ax, bx) <= cx <= max(ax, bx) and \
               min(ay, by) <= cy <= max(ay, by):
                return True

        return False

    for i in range(total):
        for j in range(i + 1, total):
            if (i < n) != (j < n):
                continue

            if not blocked(i, j):
                graph[i].append(j)
                graph[j].append(i)

    dist = [-1] * total
    dist[0] = 0
    queue = [0]
    head = 0

    while head < len(queue):
        u = queue[head]
        head += 1

        if u == n - 1:
            return dist[u]

        for v in graph[u]:
            if dist[v] == -1:
                dist[v] = dist[u] + 1
                queue.append(v)

    return -1

def run(inp: str) -> str:
    data = list(map(int, inp.split()))
    pos = 0

    n = data[pos]
    pos += 1

    team1 = []
    for _ in range(n):
        team1.append((data[pos], data[pos + 1]))
        pos += 2

    team2 = []
    for _ in range(n):
        team2.append((data[pos], data[pos + 1]))
        pos += 2

    return str(solve_case(n, team1, team2))

# The original 2018 statement has no sample input/output pairs.

# Minimum-size input, direct pass.
assert run(
    """2
1 1 3 1
1 3 3 3
"""
) == "1", "minimum-size direct pass"

# Blocked direct pass, and no alternate teammate exists.
assert run(
    """2
1 1 3 1
2 1 2 2
"""
) == "-1", "opponent blocks the only pass"

# Teammate blocks the direct pass but provides an intermediate route.
assert run(
    """3
1 1 3 1 5 1
1 3 3 3 5 3
"""
) == "2", "intermediate teammate"

# Player on the same infinite line but outside the segment does not block.
assert run(
    """2
1 1 3 1
5 1 5 2
"""
) == "1", "collinear point outside segment"

# Maximum-size case: 22 players, Team 1 lies on y=1 and Team 2 on y=2.
# Adjacent Team 1 players can pass, so reaching Player 11 needs 10 passes.
team1 = " ".join(f"{x} 1" for x in range(1, 12))
team2 = " ".join(f"{x} 2" for x in range(1, 12))
assert run(f"11\n{team1}\n{team2}\n") == "10", "maximum-size case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n=2`, bốn người chơi riêng biệt không có người chặn |`1`| Đồ thị kích thước tối thiểu và cạnh trực tiếp | 
|`n=2`, đối thủ chính xác giữa hai đồng đội |`-1`| Bị đội khác chặn | 
|`n=3`, đồng đội giữa nguồn và đích |`2`| Đỉnh trung gian và đường đi ngắn nhất | 
|`n=2`, người chơi cộng tác ngoài phân khúc |`1`| Xử lý ranh giới phân đoạn đúng | 
|`n=11`, 22 người chơi trên hai đường thẳng song song |`10`| Số lượng người chơi tối đa và chuyển tiếp BFS lặp đi lặp lại | 

## Vỏ cạnh 

Đối với trường hợp kích thước tối thiểu```
2
1 1 3 1
1 3 3 3
```chỉ có bốn đỉnh. Hai người chơi của Đội 1 không có người chơi nào ở giữa họ, do đó biểu đồ chứa cạnh từ đỉnh 0 đến đỉnh 1. BFS khởi tạo nguồn ở khoảng cách 0, phát hiện mục tiêu ở khoảng cách một và trả về`1`. Không có giả định nào trong quá trình triển khai rằng phải tồn tại một đồng đội trung gian. 

Đối với trường hợp đoạn bị chặn```
2
1 1 3 1
2 1 2 2
```cặp cầu thủ của Đội 1 có vectơ chỉ phương ((2,0)). Đối thủ tại ((2,1)) cho tích chéo bằng 0 và nằm trong cả hai phạm vi tọa độ điểm cuối, do đó`blocked`trả về đúng. Không có cạnh đồ thị nào được thêm vào. Do đó BFS để lại mục tiêu ở`-1`, đưa ra câu trả lời đúng. 

Đối với trường hợp người chơi trung gian```
3
1 1 3 1 5 1
1 3 3 3 5 3
```cặp ((1,1),(5,1)) bị chặn bởi ((3,1)), do đó không có cạnh trực tiếp giữa Người chơi 1 và 3. Các cặp liền kề ((1,1),(3,1)) và ((3,1),(5,1)) không có người chơi nào ở giữa điểm cuối của chúng, vì vậy cả hai cạnh đều xuất hiện. BFS tiếp cận Người chơi 2 ở khoảng cách một và Người chơi 3 ở khoảng cách hai. 

Đối với trường hợp thẳng hàng-ngoài đoạn```
2
1 1 3 1
5 1 5 2
```điểm ((5,1)) thẳng hàng với cặp Đội 1 nhưng nằm ngoài đoạn từ ((1,1)) đến ((3,1)). Kiểm tra phạm vi từ chối nó như một trình chặn. Cạnh của Đội 1 vẫn còn trong biểu đồ và BFS trả về`1`. 

Đối với trường hợp kích thước tối đa, có 22 người chơi, đây là biểu đồ lớn nhất được câu lệnh cho phép:```
11
1 1 2 1 3 1 4 1 5 1 6 1 7 1 8 1 9 1 10 1 11 1
1 2 2 2 3 2 4 2 5 2 6 2 7 2 8 2 9 2 10 2 11 2
```Mỗi cặp của Đội 1 liền kề đều có một phân đoạn rõ ràng, trong khi một cặp không liền kề bị một trong những người chơi của Đội 1 chặn giữa các điểm cuối của nó. Do đó, BFS sẽ duyệt qua tất cả 11 cầu thủ của Đội 1 theo thứ tự, đến Cầu thủ 11 sau 10 đường chuyền. Thử nghiệm cũng xác nhận rằng việc triển khai vẫn đơn giản ở kích thước đầu vào lớn nhất có thể.
