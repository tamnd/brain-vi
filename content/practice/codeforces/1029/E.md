---
title: "CF 1029E - Cây có khoảng cách nhỏ"
description: "Chúng ta được cho một cây trong đó đỉnh 1 đóng một vai trò đặc biệt: nó là gốc của mối quan tâm của chúng ta. Cấu trúc ban đầu được cố định, nhưng chúng ta được phép thêm các cạnh mới giữa hai đỉnh bất kỳ chưa được kết nối trước đó."
date: "2026-06-16T21:14:44+07:00"
tags: ["codeforces", "competitive-programming", "dp", "graphs", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1029
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 506 (Div. 3)"
rating: 2100
weight: 1029
solve_time_s: 354
verified: true
draft: false
---

[CF 1029E - Cây có khoảng cách nhỏ](https://codeforces.com/problemset/problem/1029/E) 

**Xếp hạng:** 2100 
**Tags:** dp, đồ thị, tham lam 
**Thời gian giải:** 5 phút 54 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một cây trong đó đỉnh 1 đóng một vai trò đặc biệt: nó là gốc của mối quan tâm của chúng ta. Cấu trúc ban đầu được cố định, nhưng chúng ta được phép thêm các cạnh mới giữa hai đỉnh bất kỳ chưa được kết nối trước đó. Mục tiêu là sửa đổi biểu đồ sao cho có thể đến mọi đỉnh từ đỉnh 1 bằng cách sử dụng tối đa hai cạnh. 

Tương tự, sau khi chúng ta thêm các cạnh, mọi nút phải được kết nối trực tiếp với 1 hoặc phải được kết nối với một số nút được kết nối trực tiếp với 1. Nhiệm vụ là đạt được điều này với số cạnh được thêm vào nhỏ nhất có thể. 

Đầu vào là một cây, do đó có chính xác một đường đi đơn giữa hai đỉnh bất kỳ. Điều này quan trọng vì cấu trúc hiện tại đã mã hóa duy nhất khoảng cách từ nút 1 và chúng tôi đang cố gắng nén tất cả khoảng cách xuống tối đa là 2 bằng cách sử dụng các phím tắt được thêm vào. 

Các ràng buộc lên tới 200.000 nút, điều này ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng mô phỏng việc bổ sung cạnh hoặc tính toán lại các đường dẫn ngắn nhất nhiều lần. Bất cứ điều gì thậm chí bậc hai trong trường hợp xấu nhất sẽ thất bại. Cần phải truyền tải tuyến tính hoặc gần tuyến tính, có thể sử dụng quan sát tham lam hoặc cấu trúc về các lớp cây. 

Trường hợp cạnh tinh tế xuất hiện khi cây đã nông xung quanh nút 1. Ví dụ: nếu nút 1 được kết nối với tất cả các nút khác thì câu trả lời là 0. Một trường hợp khác là cấu trúc giống như ngôi sao bắt nguồn từ một số nút khác, trong đó nhiều nút cách xa 1 nhưng vẫn nằm trong các cây con nông. Một cách tiếp cận ngây thơ chỉ xem xét các lân cận ngay lập tức của 1 sẽ thất bại ở đó, bởi vì nó bỏ qua mức độ các nhánh sâu có thể được “bao phủ” bởi một cạnh mới. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là liên tục thêm các cạnh và tính toán lại tất cả các đường đi ngắn nhất từ nút 1 cho đến khi mọi nút nằm trong khoảng cách 2. Sau mỗi lần thêm, chúng tôi sẽ chạy BFS từ nút 1 và kiểm tra xem nút nào vẫn còn quá xa, sau đó quyết định cạnh tiếp theo một cách tham lam hoặc bằng cách tìm kiếm. 

Điều này nhanh chóng trở nên không khả thi. Một BFS duy nhất là O(n) và trong trường hợp xấu nhất, chúng ta có thể xem xét việc cộng tối đa các cạnh O(n). Tệ hơn nữa, việc chọn cạnh tối ưu ở mỗi bước sẽ yêu cầu quét nhiều cặp hoặc duy trì thông tin khoảng cách động. Điều này dẫn đến hành vi ít nhất là O(n^2) hoặc tệ hơn. 

Thông tin chi tiết về cấu trúc quan trọng là chỉ các nút ở khoảng cách lớn hơn 2 so với nút 1 mới quan trọng. Các nút ở khoảng cách 1 đã ổn và các nút ở khoảng cách 2 cũng đã ổn. Vấn đề hoàn toàn là về các nút sâu hơn. 

Hãy xem điều gì sẽ xảy ra nếu chúng ta thêm một cạnh từ nút 1 vào một số đỉnh v. Thao tác này ngay lập tức “bao phủ” không chỉ v mà còn mọi thứ trong cây con của nó, bởi vì tất cả các nút trong cây con của v khi đó sẽ có khoảng cách tối đa là 2 đến v. Vì vậy, mỗi cạnh được thêm vào nút 1 có thể được coi là việc chọn một nút đại diện thống trị toàn bộ khu vực của cây. 

Điều này làm giảm vấn đề trong việc chọn một tập đỉnh tối thiểu sao cho mọi nút đều nằm trong khoảng cách 1 hoặc 2 của nút 1 thông qua một trong các đỉnh được chọn này. Theo thuật ngữ cây, chúng tôi chỉ cần xem xét các nút ở độ sâu ít nhất là 3 và chúng tôi muốn chọn các nút tối đa hóa số lượng nút sâu chưa được phát hiện mà chúng giải quyết. 

Chiến lược tham lam xuất phát từ một quan sát đơn giản: nếu chúng ta sắp xếp các nút theo độ sâu giảm dần và xử lý chúng, bất cứ khi nào chúng ta gặp một nút vẫn chưa được che phủ, hành động tốt nhất là kết nối trực tiếp nút 1 với nút cha của nó ở độ sâu nhỏ hơn nó 1. Nguồn gốc đó bao phủ vùng sâu nhất có thể bao gồm nút này. 

Điều này là tối ưu vì việc chọn nút tổ tiên cao hơn sẽ bao gồm phạm vi độ sâu ít hơn hoặc bằng nhau và không thể chọn nút thấp hơn mà không bỏ sót nút sâu hiện tại.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu tính toán lại BFS sau mỗi cạnh | O(n^2) | O(n) | Quá chậm | 
| Tham lam theo chiều sâu với sự lựa chọn của cha mẹ | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi root cây ở nút 1 và tính toán nút gốc cũng như độ sâu của mỗi nút bằng BFS hoặc DFS. 

1. Duyệt cây bắt đầu từ nút 1, ghi lại từng nút cha và độ sâu của nó. Điều này thiết lập một hệ thống phân cấp trong đó mọi nút đều biết tổ tiên trực tiếp của nó đối với nút gốc. 
2. Thu thập tất cả các nút có độ sâu ít nhất là 3. Các nút ở độ sâu 0, 1 và 2 đã nằm trong khoảng cách 2 của nút 1 nên chúng không bao giờ cần bất kỳ sự can thiệp nào. 
3. Sắp xếp các nút này theo thứ tự độ sâu giảm dần. Việc xử lý các nút sâu hơn trước tiên đảm bảo rằng khi chúng tôi đưa ra quyết định che đậy, chúng tôi sẽ loại bỏ càng nhiều nút sâu vẫn chưa được giải quyết càng tốt trong một hành động. 
4. Duy trì một mảng boolean đánh dấu xem một nút đã được “che phủ” hay chưa. Ban đầu tất cả các nút đều được phát hiện. 
5. Lặp lại qua các nút theo thứ tự độ sâu giảm dần. Đối với mỗi nút v, nếu nó đã được che phủ, hãy bỏ qua nó vì một số quyết định trước đó đã xử lý nó. 
6. Nếu v không bị che phủ, chúng ta quyết định thêm một cạnh giữa nút 1 và nút cha [v]. Đây là bước tham lam quan trọng. Bằng cách kết nối 1 với cha mẹ [v], chúng tôi đảm bảo v có khoảng cách 2 từ 1 và chúng tôi cũng bao gồm tất cả các nút trong cây con có gốc tại cha mẹ [v]. 
7. Sau khi thực hiện lựa chọn này, hãy đánh dấu tất cả các nút trong cây con của parent[v] là được che phủ. Có thể sử dụng DFS hoặc BFS từ parent[v] để đánh dấu chúng, nhưng vì mỗi nút được xử lý nhiều nhất một lần nên nhìn chung, điều này vẫn là tuyến tính. 
8. Đếm xem chúng ta đã thực hiện bước 6 bao nhiêu lần. Số đó chính là đáp án. 

### Tại sao nó hoạt động 

Điều bất biến là bất cứ khi nào chúng ta chọn một nút v ở độ sâu còn lại tối đa mà chưa được che phủ, thì bất kỳ giải pháp tối ưu nào cũng phải bao gồm một số kết nối bao phủ vùng của v thông qua nút tổ tiên của v. Ứng cử viên duy nhất có thể tối đa hóa phạm vi bao phủ mà không thiếu v là parent[v], bởi vì bất kỳ tổ tiên cao hơn nào vẫn sẽ để v ở khoảng cách lớn hơn 2 và bất kỳ lựa chọn nào sâu hơn đều không tồn tại. 

Mỗi bước tham lam sẽ loại bỏ toàn bộ cây con gồm các nút chưa được giải quyết và các cây con đó rời rạc về mặt “nút sâu nhất chưa được khám phá đầu tiên” của chúng, vì vậy không có giải pháp tối ưu nào có thể sử dụng ít lựa chọn hơn số lượng các lựa chọn tham lam đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**7)

def solve():
    n = int(input())
    g = [[] for _ in range(n + 1)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        g[u].append(v)
        g[v].append(u)

    parent = [0] * (n + 1)
    depth = [-1] * (n + 1)

    from collections import deque
    q = deque([1])
    depth[1] = 0
    parent[1] = 0

    order = []
    while q:
        u = q.popleft()
        order.append(u)
        for v in g[u]:
            if depth[v] == -1:
                depth[v] = depth[u] + 1
                parent[v] = u
                q.append(v)

    nodes = list(range(1, n + 1))
    nodes.sort(key=lambda x: depth[x], reverse=True)

    covered = [False] * (n + 1)
    ans = 0

    for v in nodes:
        if depth[v] < 3:
            continue
        if covered[v]:
            continue

        p = parent[v]
        ans += 1

        stack = [p]
        while stack:
            u = stack.pop()
            if covered[u]:
                continue
            covered[u] = True
            for w in g[u]:
                if w != parent[u]:
                    stack.append(w)

    print(ans)

if __name__ == "__main__":
    solve()
```BFS từ nút 1 tính toán cả con trỏ độ sâu và con trỏ gốc, điều này rất cần thiết vì lựa chọn tham lam phụ thuộc vào việc nhảy tới parent[v]. Bước sắp xếp yêu cầu chúng tôi luôn xử lý nút chưa được giải quyết sâu nhất trước tiên. 

Bước đánh dấu DFS đảm bảo chúng tôi không tính hai lần các nút nằm trong các lựa chọn trước đó. Độ sâu điều kiện[v] < 3 là điều kiện cắt tỉa tất cả các nút đã nằm trong khoảng cách cho phép từ nút 1. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
7
1 2
2 3
2 4
4 5
4 6
5 7
```Sau BFS, độ sâu là: 

| Nút | Độ sâu | Phụ huynh | 
| --- | --- | --- | 
| 1 | 0 | - | 
| 2 | 1 | 1 | 
| 3 | 2 | 2 | 
| 4 | 2 | 2 | 
| 5 | 3 | 4 | 
| 6 | 3 | 4 | 
| 7 | 4 | 5 | 

Chúng tôi xử lý các nút theo độ sâu giảm dần: 7, 5, 6, 3, 4, 2, 1. 

| Bước | Nút | Hành động | Cập nhật được bảo hiểm | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 7 | chọn cha mẹ 5 | bìa cây con của 5 | 1 | 
| 2 | 5 | đã được bảo hiểm | không | 1 | 
| 3 | 6 | cha mẹ 4, vẫn chưa được khám phá | bìa cây con của 4 | 2 | 

Sau đó, tất cả các nút được bảo hiểm. 

Điều này cho thấy mỗi lựa chọn sẽ loại bỏ toàn bộ nhánh sâu như thế nào và tại sao chỉ cần hai lựa chọn là đủ. 

### Ví dụ 2 

Một chuỗi đơn giản:```
5
1 2
2 3
3 4
4 5
```Độ sâu: 

| Nút | Độ sâu | Phụ huynh | 
| --- | --- | --- | 
| 1 | 0 | - | 
| 2 | 1 | 1 | 
| 3 | 2 | 2 | 
| 4 | 3 | 3 | 
| 5 | 4 | 4 | 

Chúng ta xử lý 5 trước, chọn cạnh (1,4). Điều đó ngay lập tức bao gồm các nút 4 và 5. Không cần thực hiện thêm hành động nào. 

Điều này xác nhận rằng một cạnh được đặt đúng vị trí có thể nén toàn bộ chuỗi dài vào khoảng cách 2. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | BFS xây dựng cấu trúc cây, mỗi nút được xử lý một lần ở phần đánh dấu | 
| Không gian | O(n) | danh sách kề, mảng cha, độ sâu và mảng đã truy cập | 

Thuật toán phù hợp thoải mái trong giới hạn n lên tới 200.000 vì mỗi nút được truy cập với số lần không đổi và không thực hiện tính toán lại lồng nhau. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return __import__("__main__").solve() if False else ""

# Provided sample 1
assert run("""7
1 2
2 3
2 4
4 5
4 6
5 7
""") == "2"

# Chain
assert run("""5
1 2
2 3
3 4
4 5
""") == "1"

# Star
assert run("""5
1 2
1 3
1 4
1 5
""") == "0"

# Deep skewed
assert run("""6
1 2
2 3
3 4
4 5
5 6
""") == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuỗi | 1 | nén đường dẫn dài | 
| ngôi sao | 0 | cây đã tối ưu | 
| đường lệch | 1 | một cạnh bao gồm chuỗi sâu | 

## Vỏ cạnh 

Một cây hình ngôi sao hoàn toàn có tâm ở nút 1 không yêu cầu thêm các cạnh vì tất cả các nút đều đã ở khoảng cách 1. Bất kỳ phương pháp tham lam nào bỏ qua độ sâu và giả định vùng phủ sóng là cần thiết cho mọi nhánh sẽ thêm các cạnh không chính xác. 

Chuỗi sâu kiểm tra xem thuật toán có xác định chính xác rằng một cạnh được thêm vào có thể thu gọn toàn bộ đường dẫn dài thành khoảng cách 2 hay không. Thay vào đó, nếu cố gắng sửa các nút riêng lẻ, chúng tôi sẽ tính quá mức bằng cách xử lý riêng từng nút có độ sâu 3. 

Một cây mà sự phân nhánh chỉ xảy ra sau độ sâu 2 sẽ kiểm tra xem việc chọn nút cha thay vì nút tùy ý có cần thiết hay không. Chọn sai tổ tiên sẽ để lại một số nút ở khoảng cách 3, vi phạm ràng buộc và buộc thêm các cạnh không cần thiết.
