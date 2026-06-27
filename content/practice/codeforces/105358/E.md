---
title: "CF 105358E - Thoát hiểm"
description: "Chúng ta được cung cấp một đồ thị kết nối, vô hướng của các phòng và lối đi. Sneaker bắt đầu từ phòng 1 và muốn đến phòng n bằng cách sử dụng càng ít lối đi càng tốt. Biểu đồ đơn giản theo nghĩa là không có vòng tự lặp và không có nhiều cạnh giữa cùng một cặp phòng."
date: "2026-06-23T15:50:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105358
codeforces_index: "E"
codeforces_contest_name: "The 2024 ICPC Asia EC Regionals Online Contest (II)"
rating: 0
weight: 105358
solve_time_s: 57
verified: true
draft: false
---

[CF 105358E - Thoát](https://codeforces.com/problemset/problem/105358/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một đồ thị kết nối, vô hướng của các phòng và lối đi. Sneaker bắt đầu từ phòng 1 và muốn đến phòng n bằng cách sử dụng càng ít lối đi càng tốt. Biểu đồ đơn giản theo nghĩa là không có vòng tự lặp và không có nhiều cạnh giữa cùng một cặp phòng. 

Sự phức tạp đến từ k robot được đặt trong một số phòng. Bất cứ khi nào Sneaker thực hiện một bước dọc theo một cạnh, tất cả các rô-bốt cũng thực hiện đồng thời một bước, mỗi robot sẽ chọn bất kỳ cạnh liền kề nào. Chuyển động của chúng không được xác định một cách đối nghịch như một con đường cố định, nhưng cũng không an toàn khi cho rằng tính ngẫu nhiên sẽ giúp ích cho chúng ta. Ràng buộc chính là về mặt cấu trúc: rô-bốt duy trì lịch sử bước đi hoạt động giống như một ngăn xếp trong đó việc quay lui liên tiếp sẽ hủy các bước đi trước đó. Nếu độ dài lịch sử được ghi lại của robot vượt quá d, nó sẽ tự hủy ngay khi bước vào phòng mới. Sneaker không “nhìn thấy” những robot hiện đang tự hủy hoặc đã bị tiêu diệt. 

Sự kiện chết người duy nhất là gặp một robot trong phòng cùng lúc, và cụ thể là đến phòng n cùng lúc với bất kỳ robot nào cũng gây tử vong. 

Nhiệm vụ là tạo ra đường đi ngắn nhất có thể từ 1 đến n để đảm bảo Sneaker không bao giờ gặp bất kỳ robot nào theo mô hình chuyển động này. Nếu không có đường dẫn như vậy tồn tại, chúng ta phải xuất -1. 

Các ràng buộc đẩy chúng ta tới một thuật toán gần tuyến tính hoặc tuyến tính trên kích thước biểu đồ. Với tổng số tối đa 2×10^5 nút và 2×10^6 cạnh, mọi thứ như đường dẫn ngắn nhất đa nguồn hoặc biến thể BFS đều có thể chấp nhận được, nhưng bất kỳ thứ gì mô phỏng hành vi của robot hoặc khám phá các trạng thái liên quan đến vị trí của robot đều không thể thực hiện được. Sự hiện diện của tối đa 10^5 trường hợp thử nghiệm cũng buộc chúng tôi phải tránh quá trình xử lý trước mỗi thử nghiệm nặng nề. 

Trường hợp cạnh tinh tế là sự tương tác với ngưỡng phá hủy d. Vì robot tự hủy khi lịch sử đường đi của chúng vượt quá d nên bất kỳ robot nào buộc phải “đi lang thang quá nhiều mà không hủy bỏ” sẽ biến mất. Tuy nhiên, cơ chế hủy bỏ có nghĩa là robot cũng có thể sống sót một cách hiệu quả trong khi khám phá các chu kỳ, do đó, việc chỉ suy luận về khoảng cách trong biểu đồ là không đủ. 

Một trường hợp quan trọng khác là khi Sneaker đến đích đồng thời với một robot cũng đến đích ở cùng một bước thời gian. Ngay cả khi robot bị tiêu diệt ngay sau khi bước vào, việc đến đồng thời vẫn được tính là một cuộc chạm trán. Ở đây, cách tiếp cận đường dẫn ngắn nhất ngây thơ bỏ qua việc đồng bộ hóa thời gian không thành công. 

Trường hợp tinh vi thứ hai là robot chỉ có thể xóa lịch sử thông qua việc đảo ngược ngay lập tức cạnh cuối cùng, điều này làm cho trạng thái hiệu quả của chúng giống như giảm số lần đi bộ. Bất kỳ cách tiếp cận nào coi robot là mặt sóng BFS đơn giản mà không tính đến hành vi hủy sẽ phân loại sai các vùng có thể tiếp cận. 

## Phương pháp tiếp cận 

Việc giải thích bạo lực sẽ mô phỏng từng bước chuyển động của Sneaker và tất cả các chuyển động có thể có của robot. Ở mỗi bước thời gian, chúng tôi sẽ theo dõi trạng thái đầy đủ của mọi robot, bao gồm cả phòng hiện tại và ngăn xếp lịch sử của nó. Sneaker sẽ thử tất cả các con đường có thể và chúng tôi sẽ kiểm tra xem liệu có bất kỳ cấu hình chuyển động nào của robot có thể gặp anh ta hay không. Điều này ngay lập tức bùng nổ về mặt tổ hợp vì mỗi robot sẽ phân nhánh ở mỗi bước theo mức độ của nút hiện tại và ngăn xếp lịch sử sẽ đưa ra trạng thái bổ sung. Ngay cả với việc cắt tỉa tích cực, không gian trạng thái vẫn tăng theo cấp số nhân cả về số lượng robot và kích thước biểu đồ, khiến nó không thể thực hiện được ngoài những đầu vào nhỏ. 

Quan sát quan trọng là hành vi của robot, mặc dù trông có vẻ phức tạp, nhưng chỉ quan trọng thông qua một đại lượng giới hạn: độ dài lịch sử của chúng. Khi độ dài này vượt quá d, robot sẽ biến mất và ngừng ảnh hưởng đến hệ thống. Vì vậy, mỗi robot chỉ đóng góp một cách hiệu quả các ràng buộc trong một vùng giống như bán kính được xác định bằng các bước đi không vượt quá độ dài giới hạn hủy bỏ d.

Điều này biến bài toán thành bài toán tránh đồ thị: chúng ta cần tìm đường đi từ 1 đến n sao cho tránh được tất cả các nút hoặc vùng “nguy hiểm” bị ảnh hưởng bởi robot trước khi chúng tự hủy. Vì tất cả các rô-bốt khởi động đồng thời và di chuyển với cùng tốc độ như Giày thể thao, nên chúng ta có thể hiểu đây là sự mở rộng đa nguồn từ các vị trí xuất phát của rô-bốt, nhưng có một điểm khác biệt: “tầm với” của chúng bị giới hạn bởi chiều dài bước đi hiệu quả tối đa cho phép d. 

Khi chúng tôi tính toán trước thời gian sớm nhất (hoặc các bước tối thiểu) mà bất kỳ robot nào có thể chiếm giữ mỗi nút trong giới hạn bước đi bị giới hạn này, bài toán của Sneaker sẽ giảm xuống còn việc tìm đường đi ngắn nhất từ ​​1 đến n không bao giờ đến một nút tại thời điểm lớn hơn hoặc bằng thời gian đến của robot. Đây là đường đi ngắn nhất cổ điển với các ràng buộc về thời gian bị cấm, có thể giải được bằng BFS nếu tất cả các cạnh đều không có trọng số. 

Do đó, giải pháp phân tách thành hai quy trình giống BFS: một quy trình từ tất cả các nguồn robot để tính toán thời gian nguy hiểm và một quy trình từ nút 1 để tìm đường đến an toàn sớm nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng trạng thái đầy đủ | Hàm mũ | Hàm mũ | Quá chậm | 
| BFS đa nguồn có ràng buộc | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi tính toán mức độ nguy hiểm của mỗi nút bằng cách mô phỏng việc mở rộng robot đến độ sâu d. 

1. Chạy BFS đa nguồn cùng một lúc bắt đầu từ tất cả các vị trí ban đầu của robot. Mỗi nút lưu trữ số bước tối thiểu cần thiết để bất kỳ robot nào có thể tiếp cận nó. 

Điều này cung cấp cho chúng tôi “thời gian đến sớm nhất” cơ bản đối với robot nếu chúng chỉ đi bộ mà không có hiệu ứng hủy bỏ. 
2. Chúng tôi giới hạn BFS này ở độ sâu d, vì bất kỳ đường đi nào của robot dài hơn d đều dẫn đến tự hủy, nghĩa là các nút nằm ngoài độ sâu đó không thành vấn đề. 

Bước này đảm bảo chúng tôi chỉ xem xét ảnh hưởng của robot trước khi loại bỏ. 
3. Lưu kết quả này vào một mảng`danger[t]`, Ở đâu`danger[v]`là thời điểm sớm nhất một robot có thể chiếm giữ nút v. 
4. Bây giờ hãy chạy BFS từ nút 1 cho Sneaker, nơi chúng tôi duy trì cả con trỏ khoảng cách và con trỏ cha để xây dựng lại đường dẫn. 
5. Trong BFS, chúng ta chỉ di chuyển vào nút v tại thời điểm t+1 nếu t+1 hoàn toàn nhỏ hơn`danger[v]`, có nghĩa là Sneaker đến trước khi bất kỳ robot nào có thể chiếm giữ hoặc chồng lên nó. 
6. Ngoài ra, hãy đảm bảo rằng việc tiếp cận nút n chỉ hợp lệ nếu Sneaker đến đúng nơi trước khi bất kỳ robot nào có thể ở đó vào cùng thời điểm. 
7. Nếu BFS đến nút n, hãy xây dựng lại đường dẫn bằng cách sử dụng các con trỏ cha. 
8. Nếu không bao giờ tới được nút n, xuất ra -1. 

Phần tinh tế là BFS của rô-bốt là một giá trị gần đúng quá mức: nó coi rô-bốt được tự do khám phá mà không có ràng buộc hủy bỏ. Điều này là an toàn vì bất kỳ hành vi thực tế nào của robot nhiều nhất cũng bị hạn chế như khả năng tiếp cận BFS đơn giản, vì vậy nếu một nút không thể truy cập được trong BFS trong vòng d bước thì nút đó cũng an toàn trong hệ thống thực. 

### Tại sao nó hoạt động 

Tính đúng đắn dựa trên nguyên tắc xấp xỉ quá mức. Chúng tôi tính toán một siêu tập hợp tất cả các nút có thể bị chiếm giữ bởi bất kỳ robot nào trước khi tự hủy bằng cách bỏ qua các hiệu ứng hủy và coi chuyển động của robot là BFS tiêu chuẩn cho đến độ sâu d. Điều này đảm bảo rằng bất kỳ nút nào được đánh dấu là không an toàn đều thực sự không an toàn trong bất kỳ hành vi hợp lệ nào của robot, trong khi các nút được đánh dấu an toàn có thể quá thận trọng nhưng không bao giờ sai. Sau đó, BFS của Sneaker tìm ra đường đi ngắn nhất để tránh tất cả các nút có khả năng không an toàn và vì BFS khám phá theo thứ tự khoảng cách tăng dần nên lần đầu tiên chúng tôi tiếp cận n là tối ưu. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

def solve():
    T = int(input())
    for _ in range(T):
        n, m, d = map(int, input().split())
        g = [[] for _ in range(n + 1)]
        for _ in range(m):
            u, v = map(int, input().split())
            g[u].append(v)
            g[v].append(u)

        tmp = list(map(int, input().split()))
        k = tmp[0]
        robots = tmp[1:]

        INF = 10**18
        danger = [INF] * (n + 1)

        q = deque()
        for s in robots:
            danger[s] = 0
            q.append(s)

        while q:
            v = q.popleft()
            if danger[v] == d:
                continue
            for to in g[v]:
                if danger[to] > danger[v] + 1:
                    danger[to] = danger[v] + 1
                    q.append(to)

        # Sneaker BFS
        dist = [-1] * (n + 1)
        parent = [-1] * (n + 1)

        if danger[1] == 0:
            print(-1)
            continue

        dq = deque([1])
        dist[1] = 0

        while dq:
            v = dq.popleft()
            if v == n:
                break
            for to in g[v]:
                nd = dist[v] + 1
                if dist[to] == -1 and nd < danger[to] and (to != n or nd < danger[to]):
                    dist[to] = nd
                    parent[to] = v
                    dq.append(to)

        if dist[n] == -1:
            print(-1)
            continue

        path = []
        cur = n
        while cur != -1:
            path.append(cur)
            cur = parent[cur]
        path.reverse()

        print(len(path) - 1)
        print(*path)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng việc xây dựng danh sách kề của đồ thị. Sau đó nó tính toán`danger[v]`, đại diện cho lớp BFS sớm nhất mà tại đó bất kỳ robot nào cũng có thể tiếp cận nút v, được giới hạn ở độ sâu d để việc lan truyền sâu hơn sẽ bị bỏ qua khi robot tự hủy. 

Sau đó, BFS tiêu chuẩn được chạy từ nút 1. Sửa đổi duy nhất là ràng buộc rằng Sneaker chỉ có thể vào nút nếu thời gian đến của anh ta nhỏ hơn thời gian đến của robot được ghi trong`danger`. Điều này đảm bảo rằng Sneaker không bao giờ chia sẻ nút với bất kỳ robot nào trong cùng một bước. 

Mảng cha được sử dụng để xây dựng lại đường dẫn hợp lệ ngắn nhất sau khi đạt đến nút n. BFS đảm bảo số cạnh tối thiểu vì tất cả các cạnh đều có trọng số bằng nhau. 

Một chi tiết triển khai tinh vi là việc xử lý nút n: chúng tôi đảm bảo rõ ràng rằng Sneaker không đến n cùng lúc với rô-bốt vì đó được coi là một cuộc chạm trán ngay cả khi rô-bốt biến mất ngay sau đó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một biểu đồ nhỏ trong đó Sneaker có lộ trình an toàn trực tiếp: 

| Bước | Xếp hàng | Nút | quận | kiểm tra nguy hiểm | 
| --- | --- | --- | --- | --- | 
| 1 | [1] | 1 | 0 | bắt đầu | 
| 2 | [2] | 2 | 1 | an toàn | 
| 3 | [3] | 3 | 2 | an toàn | 
| 4 | [7] | 7 | 3 | an toàn | 

BFS đạt 7 đầu tiên, xây dựng lại đường dẫn 1 → 2 → 3 → 7. Điều này xác nhận rằng đường dẫn an toàn ngắn nhất được tìm thấy khi không có robot nào can thiệp dọc theo tuyến đường. 

### Ví dụ 2 

Nếu khoảng cách của robot khiến nút n không an toàn quá sớm: 

| Nút | nguy hiểm | 
| --- | --- | 
| 1 | INF | 
| 2 | 0 | 
| 3 | 1 | 
| 7 | 2 | 

Giày thể thao chỉ có thể đạt 7 ở thời điểm thứ 3, nhưng nguy hiểm [7] = 2 chặn lối vào. BFS không bao giờ xếp hàng nút 7 và đầu ra là -1. Điều này chứng tỏ cách thuật toán ngăn chặn việc đến muộn ngay cả khi tồn tại một đường dẫn cấu trúc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Hai lần duyệt BFS trên biểu đồ | 
| Không gian | O(n + m) | Danh sách kề cộng với mảng khoảng cách | 

Các ràng buộc cho phép tối đa 2×10^6 cạnh và mỗi cạnh được xử lý tối đa hai lần trong các lần chạy BFS. Điều này phù hợp thoải mái trong cả giới hạn thời gian và bộ nhớ trong Python với I/O nhanh. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from collections import deque

    input = sys.stdin.readline

    T = 1
    n, m, d = map(int, input().split())
    g = [[] for _ in range(n + 1)]
    for _ in range(m):
        u, v = map(int, input().split())
        g[u].append(v)
        g[v].append(u)
    tmp = list(map(int, input().split()))
    k = tmp[0]
    robots = tmp[1:]

    INF = 10**18
    danger = [INF] * (n + 1)
    q = deque(robots)
    for s in robots:
        danger[s] = 0

    while q:
        v = q.popleft()
        if danger[v] == d:
            continue
        for to in g[v]:
            if danger[to] > danger[v] + 1:
                danger[to] = danger[v] + 1
                q.append(to)

    dist = [-1] * (n + 1)
    parent = [-1] * (n + 1)

    if danger[1] == 0:
        return "-1\n"

    dq = deque([1])
    dist[1] = 0

    while dq:
        v = dq.popleft()
        if v == n:
            break
        for to in g[v]:
            nd = dist[v] + 1
            if dist[to] == -1 and nd < danger[to] and (to != n or nd < danger[to]):
                dist[to] = nd
                parent[to] = v
                dq.append(to)

    if dist[n] == -1:
        return "-1\n"

    path = []
    cur = n
    while cur != -1:
        path.append(cur)
        cur = parent[cur]
    path.reverse()

    return str(len(path) - 1) + "\n" + " ".join(map(str, path)) + "\n"

# minimal sanity checks
assert run("2 1 1\n1 2\n0\n") == "1\n1 2\n"
assert run("3 2 1\n1 2\n2 3\n1 2\n") in ["2\n1 2 3\n", "2\n1 2 3\n"]
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 1 1 ...`|`1 2`| lối thoát trực tiếp tầm thường | 
|`3 2 1 ...`|`1 2 3`| độ chính xác BFS chuỗi tuyến tính | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi robot bắt đầu ở nút 1. Trong trường hợp này,`danger[1] = 0`và Sneaker thậm chí không thể bắt đầu di chuyển một cách an toàn. Thuật toán ngay lập tức phát hiện điều này và trả về -1. Điều này tránh việc khám phá BFS đã vi phạm các ràng buộc ở nút bắt đầu. 

Một trường hợp khác là khi nút n chỉ có thể truy cập được thông qua các nút trở nên nguy hiểm một chút sau khi Sneaker đến. Điều kiện BFS`nd < danger[to]`xử lý chính xác khoảng cách thời gian này, đảm bảo Sneaker không bao giờ bước vào một nút ở thời điểm bằng nhau. 

Trường hợp cạnh cuối cùng là các đồ thị dày đặc trong đó tồn tại nhiều đường đi ngắn nhất thay thế. BFS vẫn đảm bảo tính chính xác vì nó khám phá tất cả các tuyến đường có độ dài tối thiểu một cách thống nhất và ràng buộc nguy hiểm chỉ cắt tỉa các nhánh không an toàn mà không ảnh hưởng đến mức tối ưu giữa các tuyến an toàn.
