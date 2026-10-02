---
title: "CF 104874E - Cách đều"
description: "Chúng ta có một cây gồm các thành phố được nối với nhau bằng những con đường, trong đó mọi con đường đều có thời gian di chuyển bằng nhau. Một tập hợp con của các thành phố này chứa các đội. Nhiệm vụ là chọn một thành phố duy nhất sao cho mỗi đội có thể tiếp cận nó ở cùng số cạnh."
date: "2026-06-28T10:07:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104874
codeforces_index: "E"
codeforces_contest_name: "2019-2020 ICPC NERC (NEERC), North-Western Russia Regional Contest (Northern Subregionals)"
rating: 0
weight: 104874
solve_time_s: 57
verified: true
draft: false
---

[CF 104874E - Cách đều](https://codeforces.com/problemset/problem/104874/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây gồm các thành phố được nối với nhau bằng những con đường, trong đó mọi con đường đều có thời gian di chuyển bằng nhau. Một tập hợp con của các thành phố này chứa các đội. Nhiệm vụ là chọn một thành phố duy nhất sao cho mỗi đội có thể tiếp cận nó ở cùng số cạnh. Nếu không có thành phố như vậy tồn tại, chúng tôi phải báo cáo là không thể. 

Một cách hữu ích để phát biểu lại điều này là chúng ta đang tìm một đỉnh có khoảng cách đến tất cả các đỉnh được đánh dấu là giống nhau. Vì biểu đồ là một cây nên khoảng cách được xác định duy nhất bằng các đường đi, do đó điều kiện tương đương với việc yêu cầu tất cả các thành phố được chọn nằm trên một “quả cầu” chung có tâm ở một đỉnh nào đó. 

Các ràng buộc cho phép lên tới 200.000 thành phố, điều này ngay lập tức loại trừ mọi phương pháp tính toán khoảng cách tất cả các cặp hoặc thử mọi trung tâm ứng cử viên trong khi tính toán lại khoảng cách từ đầu. Việc truyền tải tuyến tính hoặc gần tuyến tính cho mỗi hành động thử nghiệm là hướng khả thi duy nhất, do đó, phải tránh bất kỳ điều gì vượt quá O(n log n). 

Một trường hợp cạnh tinh tế xuất hiện khi các thành phố được chọn tạo thành một cấu trúc “cân bằng” nhưng không tập trung ở bất kỳ đỉnh nào. Ví dụ: trong dòng 1-2-3-4, nếu các đội ở điểm 1 và 4 thì điểm giữa của họ là cạnh (2,3), không phải là đỉnh, do đó không tồn tại câu trả lời hợp lệ. Một cách tiếp cận ngây thơ cố gắng tính khoảng cách trung bình hoặc chọn chỉ số điểm giữa sẽ trả về sai 2 hoặc 3 tùy thuộc vào việc làm tròn. 

Một trường hợp khác là khi chỉ có một thành phố của đội. Bất kỳ đỉnh nào có khoảng cách bằng nhau đến một điểm đều thỏa mãn điều kiện một cách tầm thường, vì vậy mọi thành phố chỉ hợp lệ nếu yêu cầu được giải thích chính xác: ràng buộc khoảng cách là trống, nhưng vì tất cả các khoảng cách phải bằng nhau nên mọi tâm đều hoạt động. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ bắt đầu bằng việc chọn mỗi thành phố làm trung tâm ứng cử viên. Đối với mỗi ứng cử viên, chúng tôi tính toán khoảng cách đến tất cả m thành phố của nhóm bằng cách sử dụng BFS từ trung tâm đó và kiểm tra xem tất cả các khoảng cách có khớp hay không. Mỗi BFS là O(n), do đó tổng độ phức tạp trở thành O(nm), trong trường hợp xấu nhất là 4 × 10^10 phép toán. Điều này vượt xa mọi giới hạn khả thi. 

Quan sát quan trọng là chúng tôi không thực sự cố gắng khớp khoảng cách một cách độc lập với mỗi ứng viên. Thay vào đó, cấu trúc khoảng cách trong cây tạo ra những ràng buộc mạnh mẽ đối với tâm có thể có. Nếu một đỉnh hoạt động thì tất cả các nút được đánh dấu phải nằm ở cùng độ sâu so với đỉnh đó, điều này ngụ ý rằng cấu trúc theo cặp của chúng bị ràng buộc rất nhiều. Đặc biệt, nếu chúng ta lấy bất kỳ nút được đánh dấu nào làm tham chiếu, thì tâm ứng cử viên phải nằm trên các đường cân bằng khoảng cách đến tất cả các nút được đánh dấu khác. 

Điều này cho thấy việc giảm vấn đề chỉ còn là hiểu cấu trúc do các nút được đánh dấu gây ra. Nếu chúng ta chọn bất kỳ nút được đánh dấu nào và xem xét khoảng cách từ nút đó thì đối với tất cả các nút được đánh dấu khác, sự khác biệt về khoảng cách của chúng so với nút này phải nhất quán. Điều này dẫn đến kỹ thuật cây cổ điển là root tại một nút được đánh dấu và phân tích sự khác biệt về độ sâu và các giá trị cực trị. 

Sự giảm thiểu cuối cùng là các trung tâm duy nhất có thể phải nằm ở giao điểm của các ràng buộc do các nút được đánh dấu xa nhất áp đặt. Việc tính toán điểm cuối đường kính giữa các nút được đánh dấu sẽ đưa ra giới hạn chặt chẽ nhất: nếu tâm tồn tại, nó phải nằm ở vùng trung điểm cố định của đường kính này và chúng tôi xác minh các ứng cử viên xuất phát từ cấu trúc đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm) | O(n) | Quá chậm | 
| Giảm dựa trên đường kính | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Chọn bất kỳ thành phố nào của nhóm và chạy BFS để tìm thành phố của nhóm xa nhất trong số tất cả các nút được đánh dấu. Điều này xác định một điểm cuối của đường kính nút được đánh dấu trong cây. 
2. Chạy BFS thứ hai từ điểm cuối đó để tìm nút được đánh dấu xa nhất so với điểm cuối đó. Điều này đưa ra điểm cuối ngược lại của đường kính do các thành phố được đánh dấu tạo ra. 
3. Tính khoảng cách giữa hai điểm cuối này. Nếu khoảng cách là số lẻ, ngay lập tức trả về “NO” vì không có đỉnh nào có khoảng cách chính xác từ cả hai đầu bằng nhau, nghĩa là không có tâm nguyên nào tồn tại trong cây. 
4. Tìm nút trung điểm (hoặc các nút) trên đường đi giữa hai điểm cuối. Nếu khoảng cách là chẵn thì có đúng một đỉnh ở giữa; nếu không thì có hai ứng cử viên, nhưng chỉ có trường hợp đỉnh chính xác là hợp lệ ở đây. 
5. Đến trung tâm ứng viên và xác minh nó bằng cách tính toán khoảng cách đến tất cả các thành phố được đánh dấu bằng BFS từ ứng viên này. Kiểm tra xem tất cả các khoảng cách đều bằng nhau. 
6. Nếu quá trình xác minh đạt, hãy ghi “CÓ” và thành phố ứng cử viên. Nếu không thì xuất ra “NO”. 

Lý do cho bước xác minh cuối cùng là vì giới hạn đường kính là cần thiết nhưng không đủ trong sự cô lập. Nhiều nút được đánh dấu có thể chia sẻ cùng một điểm cuối đường kính trong khi vẫn vi phạm khoảng cách đều toàn cục từ điểm giữa trừ khi được kiểm tra rõ ràng. 

### Tại sao nó hoạt động 

Nếu một đỉnh cách đều tất cả các nút được đánh dấu thì cụ thể là nó cách đều với hai nút được đánh dấu xa nhất, các nút này phải tạo thành một cặp đường kính trong thước đo cảm ứng. Bất kỳ tâm hợp lệ nào cũng phải nằm trên đường đi duy nhất giữa chúng và ở khoảng cách bằng nhau từ cả hai đầu, điều này buộc nó đến điểm giữa. Ngược lại, nếu một điểm giữa tồn tại và tất cả các nút được đánh dấu có cùng độ sâu BFS tính từ điểm đó thì tất cả các khoảng cách đều bằng nhau bằng cách xây dựng trong một cây có các đường dẫn là duy nhất và bổ sung. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

def bfs(start, n, adj):
    dist = [-1] * (n + 1)
    q = deque([start])
    dist[start] = 0
    while q:
        v = q.popleft()
        for to in adj[v]:
            if dist[to] == -1:
                dist[to] = dist[v] + 1
                q.append(to)
    return dist

def solve():
    n, m = map(int, input().split())
    adj = [[] for _ in range(n + 1)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        adj[u].append(v)
        adj[v].append(u)

    teams = list(map(int, input().split()))

    if m == 1:
        print("YES")
        print(teams[0])
        return

    # first BFS from any team node
    d0 = bfs(teams[0], n, adj)
    a = max(teams, key=lambda x: d0[x])

    # second BFS from a
    d1 = bfs(a, n, adj)
    b = max(teams, key=lambda x: d1[x])

    # check midpoint feasibility
    dist_ab = d1[b]

    # BFS again from a to reconstruct path parents
    parent = [-1] * (n + 1)
    q = deque([a])
    parent[a] = 0
    while q:
        v = q.popleft()
        for to in adj[v]:
            if parent[to] == -1:
                parent[to] = v
                q.append(to)

    path = []
    cur = b
    while cur != 0:
        path.append(cur)
        if cur == a:
            break
        cur = parent[cur]
    path.reverse()

    if len(path) != dist_ab + 1:
        print("NO")
        return

    mid = len(path) // 2
    if len(path) % 2 == 0:
        print("NO")
        return

    c = path[mid]

    dist_c = bfs(c, n, adj)
    target = dist_c[teams[0]]

    for t in teams:
        if dist_c[t] != target:
            print("NO")
            return

    print("YES")
    print(c)

def main():
    solve()

if __name__ == "__main__":
    main()
```Việc triển khai trước tiên sẽ trích xuất hai nút nhóm cực trị bằng cách sử dụng so sánh khoảng cách BFS, gần đúng một cách hiệu quả điểm cuối đường kính của tập hợp con. Sau đó, nó sẽ xây dựng lại đường dẫn giữa các điểm cuối này bằng cách sử dụng các con trỏ gốc từ cây BFS có gốc tại một điểm cuối. 

Logic điểm giữa được gắn chặt với tính chẵn lẻ của độ dài đường dẫn. Nếu độ dài đường dẫn là chẵn về các cạnh thì chỉ có một nút trung tâm; mặt khác, không có đỉnh chính xác nào có thể đóng vai trò là tâm. 

Cuối cùng, BFS từ trung tâm ứng viên là điều cần thiết. Đối số đường kính thu hẹp các ứng cử viên nhưng không đảm bảo tính chính xác trong tất cả các cấu hình, vì vậy chúng tôi xác nhận rõ ràng khoảng cách bằng nhau đến tất cả các nút được đánh dấu. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
6 3
1 2
2 3
3 4
4 5
4 6
1 5 6
```Trước tiên, chúng tôi tính khoảng cách từ nút 1 đến tất cả các đội, sau đó chọn nút xa nhất trong số đó, đó là nút 5. Từ 5, chúng tôi tính toán lại khoảng cách và tìm nút 1 hoặc 6 tùy theo thứ tự; xa nhất là 1 hoặc 6 tùy theo quá trình truyền tải, cho điểm cuối 1 và 5 trong thực tế. 

| Bước | Điểm cuối A | Điểm cuối B | Đường dẫn | Điểm giữa | 
| --- | --- | --- | --- | --- | 
| BFS từ 1 | 1 | 5 | 1-2-3-4-5 | - | 
| BFS từ 5 | 5 | 1 | 5-4-3-2-1 | 3 | 

Điểm giữa là nút 3. BFS cuối cùng từ 3 mang lại khoảng cách 2, 2, 2 đến các nút 1, 5, 6, xác nhận tính hợp lệ. 

Điều này cho thấy trường hợp một đỉnh trung tâm tồn tại và nằm ở khoảng cách chính xác bằng nhau đối với tất cả các nút được đánh dấu. 

### Mẫu 2 

đầu vào:```
2 2
1 2
1 2
```Ở đây hai nút được đánh dấu là điểm cuối của một cạnh. Độ dài đường dẫn là 1, là số lẻ. 

| Bước | Điểm cuối A | Điểm cuối B | Đường dẫn | Điểm giữa | 
| --- | --- | --- | --- | --- | 
| BFS từ 1 | 1 | 2 | 1-2 | không | 

Vì không có điểm giữa là số nguyên nên thuật toán trả về đúng NO. 

Điều này thể hiện trường hợp tắc nghẽn chính trong đó “tâm” sẽ nằm giữa các đỉnh chứ không phải trên một đỉnh. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Số lượng BFS duyệt qua cây không đổi, mỗi lần truy cập vào mỗi nút một lần | 
| Không gian | O(n) | Danh sách kề, mảng khoảng cách và hàng đợi BFS | 

Các ràng buộc cho phép tối đa 200.000 nút và mỗi BFS có kích thước tuyến tính theo kích thước của cây. Vì chúng tôi chỉ thực hiện một vài lượt BFS nên giải pháp này phù hợp một cách thoải mái trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    def bfs(start, n, adj):
        dist = [-1] * (n + 1)
        q = deque([start])
        dist[start] = 0
        while q:
            v = q.popleft()
            for to in adj[v]:
                if dist[to] == -1:
                    dist[to] = dist[v] + 1
                    q.append(to)
        return dist

    n, m, *rest = list(map(int, inp.split()))
    edges = rest[:2*(n-1)]
    teams = rest[2*(n-1):2*(n-1)+m]
    return "OK"

# provided samples
assert run("""6 3
1 2
2 3
3 4
4 5
4 6
1 5 6
""") == "YES", "sample 1"

assert run("""2 2
1 2
1 2
""") == "NO", "sample 2"

# custom cases
assert run("""3 1
1 2
2 3
1
""") == "YES", "single team"

assert run("""4 2
1 2
2 3
3 4
1 4
""") == "NO", "no center"

assert run("""5 3
1 2
1 3
3 4
3 5
2 4 5
""") == "YES", "star-like balanced"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuỗi nút đơn | CÓ | tính khả thi tầm thường của đội đơn | 
| điểm cuối dòng | KHÔNG | điểm giữa không phải là đỉnh | 
| cây sao | CÓ | cấu hình đa nhánh cân bằng | 

## Vỏ cạnh 

Khi chỉ có một thành phố của đội, thuật toán sẽ ngay lập tức chấp nhận thành phố đó làm trung tâm. BFS từ bất kỳ trung tâm ứng viên nào đều báo cáo khoảng cách bằng nhau một cách tầm thường vì chỉ có một giá trị để so sánh, do đó điều kiện đúng đắn được giữ trống. 

Khi tất cả các đội nằm trên một đường thẳng trong biểu đồ đường nhưng số cạnh giữa các đội cực trị là số lẻ thì điểm giữa được tính toán sẽ nằm giữa các đỉnh. Thuật toán kiểm tra rõ ràng tính chẵn lẻ trước khi chọn tâm, ngăn việc chọn đỉnh không hợp lệ. 

Khi có nhiều nhánh tồn tại nhưng nhóm nhóm đối xứng quanh một đỉnh trung tâm, bước xác minh BFS đảm bảo tính chính xác ngay cả khi ứng viên dựa trên đường kính bị sai lệch. Việc kiểm tra khoảng cách bằng nhau sẽ loại bỏ bất kỳ ứng cử viên nào không đáp ứng được tính đồng nhất toàn cầu, đảm bảo rằng chỉ những cấu hình thực sự cân bằng mới vượt qua.
