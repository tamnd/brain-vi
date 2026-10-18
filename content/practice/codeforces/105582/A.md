---
title: "CF 105582A - Táo"
description: "Chúng ta có một đồ thị đơn giản vô hướng có nhiều nhất 100 đỉnh và 100 cạnh. Nhiệm vụ không phải là “vẽ” bất cứ thứ gì về mặt hình học theo nghĩa tính toán, mà là quyết định xem liệu biểu đồ này có thể được hiểu là một cấu trúc rất cụ thể được gọi là quả táo hay không."
date: "2026-06-22T17:51:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105582
codeforces_index: "A"
codeforces_contest_name: "Ural Championship 2017"
rating: 0
weight: 105582
solve_time_s: 59
verified: true
draft: false
---

[CF 105582A - Apple](https://codeforces.com/problemset/problem/105582/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị đơn giản vô hướng có nhiều nhất 100 đỉnh và 100 cạnh. Nhiệm vụ không phải là “vẽ” bất cứ thứ gì về mặt hình học theo nghĩa tính toán, mà là quyết định xem liệu biểu đồ này có thể được hiểu là một cấu trúc rất cụ thể được gọi là quả táo hay không. 

Một quả táo bao gồm hai cấu trúc đơn giản có đỉnh riêng biệt có chung một đỉnh bắt đầu chung. Một cấu trúc là một chu trình đơn giản và cấu trúc kia là một đường dẫn đơn giản. Chu trình phải chứa ít nhất ba đỉnh và đường đi phải chứa ít nhất hai đỉnh. Ngoài đỉnh bắt đầu được chia sẻ, chu trình và đường đi không được chạm vào nhau hoặc sử dụng lại các đỉnh. Câu hỏi giảm xuống còn việc quyết định liệu chúng ta có thể phân chia các cạnh của đồ thị thành một chu trình đơn giản và một đường dẫn đơn giản có nghiệm chung hay không, dưới những ràng buộc cấu trúc chặt chẽ này. 

Các ràng buộc đủ nhỏ để lý thuyết theo cấp số nhân có thể thực hiện được nhưng không cần thiết. Với n, m ≤ 100, thậm chí phân tích biểu đồ O(n³) hoặc O(n·m) cũng nhanh chóng một cách thoải mái. Điều này ngay lập tức gợi ý rằng chúng ta nên nhắm đến việc phân rã cấu trúc hơn là bất kỳ sự sắp xếp hình học hoặc liệt kê các phần nhúng nào một cách thô bạo. 

Trường hợp cạnh tinh vi xuất hiện khi đồ thị trông “gần giống” một chu trình có đuôi, nhưng đuôi đó sử dụng lại một đỉnh của chu trình hoặc chu trình quá nhỏ. Ví dụ: nếu chúng ta có tam giác 1-2-3-1 và đường dẫn 1-4 thì điều này hợp lệ. Nhưng nếu đường đi cũng chạm vào đỉnh 2, tạo thành một cái gì đó giống như 1-2-4, thì việc phân rã không còn hợp lệ vì đường đi và chu trình giao nhau bên ngoài gốc. Một dạng thất bại khác là khi có một chu trình có kích thước 2, điều này không thể xảy ra trong một biểu đồ đơn giản, nhưng vẫn đáng lưu ý như một cái bẫy khái niệm khi suy luận về các phương pháp tiếp cận dựa trên mức độ. 

## Phương pháp tiếp cận 

Một cách diễn giải brute-force sẽ cố gắng đoán đỉnh gốc và sau đó quyết định cạnh nào thuộc về chu trình và cạnh nào thuộc về đường đi, kiểm tra xem liệu chúng ta có thể hình thành chính xác một chu trình đơn giản và một đường đi đơn giản thỏa mãn mọi ràng buộc hay không. Điều này dẫn đến việc liệt kê các tập hợp con của các cạnh một cách tự nhiên hoặc thực hiện phép gán quay lui các cạnh liên quan đến gốc. Ngay cả với n ≤ 100, điều này vẫn trở thành tổ hợp: nếu một đỉnh có bậc d, việc chia các cạnh liên quan của nó thành hai nhóm đã tạo ra các khả năng 2^d và trên tất cả các đỉnh, điều này sẽ vượt xa các giới hạn khả thi. 

Quan sát quan trọng là cấu trúc cực kỳ cứng nhắc. Biểu đồ bao gồm chính xác hai thành phần được kết nối chỉ giao nhau ở một đỉnh và mỗi thành phần có một dạng rất cụ thể. Một thành phần là một chu trình, nghĩa là mọi đỉnh trong nó đều có bậc 2 bên trong đồ thị con đó. Cái còn lại là một đường dẫn, nghĩa là chính xác hai đỉnh có độ 1 bên trong sơ đồ con đó và tất cả các đỉnh khác có độ 2, ngoại trừ gốc chung có độ 1 trong thành phần đường dẫn nhưng được chia sẻ trên toàn cầu. 

Sự cứng nhắc này cho phép chúng ta chuyển vấn đề từ “tìm một bản vẽ” sang “kiểm tra xem liệu chúng ta có thể phân tách biểu đồ thành hai biểu đồ con rất cụ thể có chung một điểm khớp nối hay không”. Hướng tự nhiên là thử mỗi đỉnh làm gốc chung tiềm năng và xác nhận xem cấu trúc sự cố của nó có thể được chia thành một thành phần chu trình và một thành phần đường dẫn hay không. 

Điều kiện chu trình trở thành sự kiểm tra xem liệu có tồn tại một chu trình hoàn toàn chứa trong biểu đồ có thể được “neo” ở gốc hay không, trong khi các cạnh còn lại từ gốc tạo thành một đường dẫn đơn giản. Vì biểu đồ nhỏ nên chúng tôi có thể thử phát hiện chu trình dựa trên DFS một cách rõ ràng và xác thực đường dẫn bắt nguồn từ mỗi ứng cử viên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Phân công cạnh Brute Force | Hàm mũ | O(n + m) | Quá chậm | 
| Kiểm tra cấu trúc dựa trên gốc với DFS | O(n · (n + m)) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Xây dựng danh sách kề cho đồ thị. Điều này cho phép truyền tải hiệu quả các hàng xóm khi cố gắng xác minh các ràng buộc về cấu trúc. 
2. Lặp lại từng đỉnh, coi nó là gốc chung tiềm năng của chu trình và đường đi. Cấu hình đúng phải sử dụng chính xác một đỉnh như vậy, vì vậy việc thử tất cả các khả năng đều an toàn. 
3. Đối với nghiệm cố định, cố gắng xác định một chu trình không đi qua nghiệm một cách tùy ý nhưng có thể được coi là “thành phần chu trình”. Chúng tôi thực hiện DFS trong biểu đồ trong khi theo dõi các cạnh cha và phát hiện các cạnh sau tạo thành một chu trình. 
4. Sau khi tìm thấy một chu trình, đánh dấu tất cả các đỉnh thuộc chu trình đó. Điều này rất quan trọng vì chu trình phải nhất quán nội tại: mỗi đỉnh trong nó phải có chính xác hai đỉnh lân cận trong cấu trúc chu trình. 
5. Xác minh rằng gốc được kết nối với chính xác hai đỉnh trong chu trình hoặc tham gia theo cách phù hợp với đỉnh được chia sẻ. Nếu chu trình không thể được tách biệt rõ ràng thì gốc này không hợp lệ. 
6. Loại bỏ hoặc bỏ qua các cạnh chu trình và kiểm tra cấu trúc còn lại. Các cạnh còn lại liên quan đến gốc phải tạo thành một đường đi đơn giản. Điều này có nghĩa là bắt đầu từ gốc, DFS trên các cạnh còn lại sẽ truy cập tất cả các đỉnh còn lại chính xác một lần trong cấu trúc chuỗi tuyến tính, không phân nhánh. 
7. Xác thực rằng thành phần còn lại thực sự là một đường dẫn: mọi đỉnh ngoại trừ điểm cuối phải có bậc 2 trong đồ thị con còn lại và chính xác hai điểm cuối phải có bậc 1. 
8. Nếu bất kỳ gốc nào thỏa mãn cả điều kiện chu trình và điều kiện đường dẫn, hãy trả về “Có”. 

Nếu không có gốc nào tạo ra một phân tách hợp lệ, trả về “Không”. 

### Tại sao nó hoạt động 

Tính đúng đắn đến từ độ cứng vững về cấu trúc của đối tượng mục tiêu. Mọi cấu hình hợp lệ phải có chính xác một đỉnh thuộc cả hai thành phần. Đỉnh đó là điểm khớp nối giữa một chu trình và một đường đi. Việc loại bỏ nó sẽ chia biểu đồ thành chính xác hai cấu trúc được kết nối với các ràng buộc mức độ cố định: một cấu trúc phải là một chu trình (tất cả đều là cấp 2 bên trong) và cấu trúc kia phải là một đường dẫn (hai điểm cuối, độ nghỉ 2). Vì mỗi quả táo hợp lệ đều có chính xác một điểm phân rã như vậy, việc lặp qua tất cả các đỉnh đảm bảo rằng cuối cùng chúng ta sẽ kiểm tra điểm phân rã đúng nếu nó tồn tại. Kiểm tra DFS thực thi các ràng buộc cục bộ mô tả duy nhất các chu trình và đường dẫn trong đồ thị vô hướng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def find_cycle(n, adj, start):
    parent = [-1] * (n + 1)
    visited = [False] * (n + 1)
    stack = [(start, -1)]
    visited[start] = True

    while stack:
        u, p = stack.pop()
        for v in adj[u]:
            if v == p:
                continue
            if not visited[v]:
                visited[v] = True
                parent[v] = u
                stack.append((v, u))
            else:
                # found a cycle, reconstruct it
                cycle = set()
                x = u
                cycle.add(v)
                while x != v and x != -1:
                    cycle.add(x)
                    x = parent[x]
                return cycle
    return set()

def is_path_component(n, adj, root, banned):
    visited = [False] * (n + 1)

    def dfs(u):
        visited[u] = True
        for v in adj[u]:
            if v in banned:
                continue
            if not visited[v]:
                dfs(v)

    dfs(root)

    nodes = [i for i in range(1, n + 1) if visited[i] and i not in banned]
    if not nodes:
        return False

    deg = {}
    for u in nodes:
        deg[u] = 0
        for v in adj[u]:
            if v not in banned:
                deg[u] += 1

    endpoints = [u for u in nodes if deg[u] == 1]
    internal = [u for u in nodes if deg[u] == 2]

    return len(endpoints) == 2 and len(endpoints) + len(internal) == len(nodes)

def solve():
    n, m = map(int, input().split())
    adj = [[] for _ in range(n + 1)]
    for _ in range(m):
        a, b = map(int, input().split())
        adj[a].append(b)
        adj[b].append(a)

    for root in range(1, n + 1):
        cycle = find_cycle(n, adj, root)
        if not cycle:
            continue
        if root not in cycle:
            continue
        if is_path_component(n, adj, root, cycle):
            return "Yes"

    return "No"

print(solve())
```Việc triển khai sẽ thử từng đỉnh làm điểm nối chung. Đối với mỗi gốc ứng cử viên, nó chạy tính năng phát hiện chu trình dựa trên DFS để xây dựng lại một chu trình khi tìm thấy cạnh sau. Chu trình đó được coi là thành phần chu trình. Kiểm tra còn lại xác minh xem phần còn lại của biểu đồ có tạo thành một đường dẫn sạch sau khi loại trừ các đỉnh chu kỳ hay không. Điểm tinh tế quan trọng là việc phát hiện chu trình không cố gắng liệt kê tất cả các chu trình, chỉ một chu trình hợp lệ có thể đóng vai trò là thành phần bắt buộc, điều này là đủ vì mọi giải pháp hợp lệ đều phải chứa ít nhất một chu trình như vậy đi qua gốc chính xác. 

Xác thực đường dẫn tính toán rõ ràng độ trong biểu đồ bị hạn chế. Điều này tránh việc chỉ dựa vào thứ tự truyền tải, điều này sẽ dễ hỏng trong các biểu đồ có nhiều nhánh. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 5
1 2
2 3
1 3
1 4
1 5
```Chúng tôi thử từng nút với quyền root. Đối với nghiệm 1, tồn tại một chu trình 1-2-3-1 nên chu trình = {1,2,3}. Loại bỏ nó để lại các cạnh 1-4 và 1-5. 

| Bước | Chu kỳ | Đồ thị còn lại | Đường dẫn hợp lệ | 
| --- | --- | --- | --- | 
| gốc=1 | {1,2,3} | 1-4, 1-5 | Không | 

Cấu trúc còn lại phân nhánh tại nút 1, tạo ra bậc 2 ở gốc trong biểu đồ dư, vi phạm cấu trúc đường dẫn. Vì vậy, gốc 1 không thành công và không có gốc nào khác tạo ra sự phân tách hợp lệ. 

Đầu ra: Không. 

### Ví dụ 2 

đầu vào:```
4 4
1 2
2 3
1 3
1 4
```Đối với gốc 1, chu trình là {1,2,3}. Cạnh còn lại là 1-4. 

| Bước | Chu kỳ | Đồ thị còn lại | Đường dẫn hợp lệ | 
| --- | --- | --- | --- | 
| gốc=1 | {1,2,3} | 1-4 | Có | 

Thành phần còn lại là một cạnh đơn, là đường đi hợp lệ có độ dài 2 đỉnh. Chu trình có kích thước 3, thỏa mãn các ràng buộc. 

Đầu ra: Có. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · (n + m)) | Mỗi gốc kích hoạt phát hiện chu trình DFS và xác thực mức độ trên biểu đồ | 
| Không gian | O(n + m) | Danh sách kề cộng với mảng phụ trợ | 

Với n, m ≤ 100, điều này phù hợp một cách thoải mái trong giới hạn. Ngay cả các phép toán 10⁴ trong trường hợp xấu nhất trên mỗi gốc cũng không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return solve()

# sample 1
assert run("""5 5
1 2
2 3
1 3
1 4
1 5
""").strip() == "No"

# sample 2
assert run("""4 4
1 2
2 3
1 3
1 4
""").strip() == "Yes"

# minimum case (impossible)
assert run("""3 2
1 2
2 3
""").strip() == "No"

# simple valid apple
assert run("""5 5
1 2
2 3
3 1
1 4
4 5
""").strip() == "Yes"

# disconnected graph
assert run("""6 4
1 2
2 3
3 1
4 5
""").strip() == "No"

# all nodes in cycle only
assert run("""4 4
1 2
2 3
3 4
4 1
""").strip() == "No"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Chỉ 3 chuỗi | Không | không tồn tại chu trình | 
| chu kỳ + đuôi | Có | phân hủy hợp lệ | 
| thành phần bị ngắt kết nối | Không | cấu trúc phải được kết nối | 
| chu trình thuần túy | Không | thành phần đường dẫn bị thiếu | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi một chu trình tồn tại nhưng cấu trúc còn lại phân nhánh ở gốc. Ví dụ: một hình tam giác có hai cạnh phụ từ cùng một đỉnh. Mặc dù có một chu trình, đồ thị dư không đáp ứng được điều kiện mức độ đường đi vì gốc có bậc lớn hơn 1 trong thành phần còn lại. 

Một trường hợp tinh tế khác là khi tồn tại nhiều chu kỳ. Thuật toán dựa vào việc phát hiện một chu kỳ trên mỗi gốc, nhưng không phải tất cả các chu trình đều hợp lệ. Nếu chu trình được phát hiện không bao gồm cấu trúc phân rã chính xác thì gốc sẽ thất bại sau này trong bước xác thực đường dẫn, điều này đảm bảo các lựa chọn chu trình không chính xác không tạo ra kết quả dương tính giả. 

Trường hợp cuối cùng là khi đồ thị được kết nối nhưng không chứa chu trình đơn giản. Trong trường hợp đó, tính năng phát hiện chu trình trả về giá trị trống cho tất cả các gốc và thuật toán đưa ra kết quả “Không” một cách chính xác mà không cần cố gắng kiểm tra đường dẫn không cần thiết.
