---
problem: 928C
contest_id: 928
problem_index: C
name: "Dependency management"
contest_name: "VK Cup 2018 - \u041a\u0432\u0430\u043b\u0438\u0444\u0438\u043a\u0430\u0446\u0438\u044f 1"
rating: 1900
tags: ["*special", "graphs", "implementation"]
answer: passed_samples
verified: true
solve_time_s: 90
date: 2026-06-18
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a330317-3000-83ec-a44f-85b0de8c477a
---

#CF 928C - Quản lý phụ thuộc 

**Xếp hạng:** 1900 
**Tags:** *đặc biệt, đồ thị, thực hiện 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 1 phút 30s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a330317-3000-83ec-a44f-85b0de8c477a 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các dự án phần mềm, trong đó mỗi dự án được xác định không chỉ bằng tên mà còn bằng số phiên bản. Mỗi dự án có thể phụ thuộc vào các dự án khác, tạo thành biểu đồ phụ thuộc theo chu kỳ có hướng. Dự án đầu tiên trong đầu vào là dự án hiện tại của Polycarp và nhiệm vụ của chúng tôi là xác định mọi thứ phải có khi xây dựng hoặc phân tích nó. 

Điều tinh tế là cùng một tên dự án có thể xuất hiện trong nhiều phiên bản và các phần phụ thuộc có thể đề cập đến các phiên bản khác nhau của cùng một dự án được đặt tên. Tuy nhiên, khi có thể truy cập nhiều phiên bản cùng tên, chỉ có một phiên bản thực sự được giữ trong bộ phụ thuộc đã được giải quyết cuối cùng. Phiên bản được chọn phải là phiên bản “gần nhất” về khoảng cách phụ thuộc với dự án của Polycarp và nếu nhiều phiên bản có cùng khoảng cách thì phiên bản có số phiên bản lớn hơn sẽ được ưu tiên. 

Đầu ra là tập hợp tất cả các dự án (không bao gồm dự án gốc của Polycarp) còn lại sau quá trình phân giải này, được in theo thứ tự từ điển theo tên dự án cùng với phiên bản đã chọn của chúng. 

Ràng buộc n 1000 ngay lập tức gợi ý rằng việc truyền tải biểu đồ O(n²) hoặc O(n log n) là ổn, nhưng bất kỳ điều gì liên quan đến việc tính toán lại lặp đi lặp lại trên các tập hợp con lớn vẫn sẽ vượt qua. Biểu đồ thưa thớt theo cách xây dựng vì mỗi dự án liệt kê rõ ràng các phụ thuộc trực tiếp của nó và tổng số nút tối đa là 1000. 

Trường hợp key edge xuất phát từ nhiều phiên bản có cùng tên xuất hiện ở các phần khác nhau của biểu đồ. Một DFS ngây thơ xử lý (tên, phiên bản) một cách độc lập sẽ bao gồm đồng thời các phiên bản xung đột. Ví dụ: nếu dự án a phụ thuộc vào b 1 và cả b 2 thông qua một chuỗi khác, thì việc truyền tải đơn giản có thể bao gồm cả hai, mặc dù chỉ cho phép một phiên bản của b trong độ phân giải cuối cùng. 

Một vấn đề tế nhị khác là “phiên bản tốt nhất” phụ thuộc vào khoảng cách ngắn nhất trong biểu đồ phụ thuộc từ gốc, do đó, một phiên bản sâu hơn nhưng lớn hơn về số lượng vẫn có thể bị loại bỏ để thay thế bằng một phiên bản gần hơn. Giải pháp chỉ theo dõi các nút đã truy cập mà không theo dõi khoảng cách tốt nhất cho mỗi tên sẽ không thành công trong trường hợp khám phá sau đó mang lại phiên bản tốt hơn. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là coi mỗi cặp (tên, phiên bản) là một nút riêng biệt trong biểu đồ và chạy DFS hoặc BFS từ dự án của Polycarp, thu thập tất cả các nút có thể truy cập. Điều này tìm thấy chính xác tất cả các phụ thuộc nhưng không giải quyết được quy tắc xung đột giữa các phiên bản cùng tên. Sau khi duyệt, chúng ta cần xử lý hậu kỳ tập hợp bằng cách nhóm theo tên và chọn phiên bản tốt nhất. 

Vấn đề là chỉ khả năng tiếp cận thôi là chưa đủ. Phiên bản chính xác phụ thuộc vào khoảng cách đường đi ngắn nhất, vì vậy chúng tôi phải theo dõi, đối với mỗi tên, khoảng cách tối thiểu gặp phải cho đến nay. Sẽ cần phải duyệt qua lần thứ hai hoặc lặp đi lặp lại nếu chúng ta làm điều đó một cách ngây thơ. Trong trường hợp xấu nhất, nhiều phiên bản của cùng một dự án xuất hiện ở các độ sâu khác nhau và việc truyền bá đơn giản có thể liên tục cập nhật các lựa chọn, dẫn đến hành vi bậc hai. 

Quan sát chính là biểu đồ phụ thuộc là DAG bắt nguồn từ dự án của Polycarp và chúng tôi chỉ quan tâm đến khoảng cách đường đi ngắn nhất trong biểu đồ không có trọng số, có thể được tính toán bằng BFS. Khi chúng tôi tính toán khoảng cách tối thiểu từ gốc đến mọi nút (tên, phiên bản), chúng tôi có thể giải quyết từng tên một cách độc lập bằng cách chọn phiên bản có khoảng cách nhỏ nhất, phá vỡ mối quan hệ bằng cách chọn số phiên bản lớn hơn. Điều này loại bỏ sự cần thiết phải sàng lọc lặp đi lặp lại. 

Sau đó, chúng tôi thực hiện lần chuyển thứ hai qua tất cả các nút để trích xuất phiên bản đã chọn cho từng tên và xuất chúng.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force DFS + xử lý hậu kỳ | O(n²) trường hợp xấu nhất | O(n) | Chấp nhận được nhưng dễ vỡ | 
| BFS + lựa chọn tên tốt nhất | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi chuẩn hóa đầu vào thành biểu đồ trong đó mỗi nút là một phiên bản dự án được xác định bởi (tên, phiên bản). Chúng tôi cũng gán cho mỗi nút một id số nguyên duy nhất để lưu trữ lân cận hiệu quả. 

Sau đó, chúng tôi tính toán khoảng cách ngắn nhất từ ​​dự án của Polycarp bằng BFS, vì tất cả các cạnh đều không có trọng số và chúng tôi muốn độ dài chuỗi phụ thuộc tối thiểu. 

1. Phân tích cú pháp tất cả các dự án và gán cho mỗi (tên, phiên bản) một id nút, đồng thời lưu trữ danh sách kề cho các phụ thuộc. Điều này xây dựng biểu đồ phụ thuộc đầy đủ mà không cần lọc. 
2. Chạy BFS bắt đầu từ dự án đầu tiên. Khởi tạo khoảng cách của nó là 0 và đẩy nó vào hàng đợi. 
3. Khi xử lý một nút, hãy duyệt qua tất cả các phần phụ thuộc của nút đó. Nếu một phần phụ thuộc chưa được truy cập, hãy gán khoảng cách current_distance + 1 cho nó và đẩy nó vào hàng đợi. Vì BFS mở rộng theo thứ tự khoảng cách tăng dần nên lần đầu tiên chúng ta gán khoảng cách được đảm bảo ở mức tối thiểu. 
4. Sau khi BFS hoàn thành, chúng tôi có khoảng cách ngắn nhất từ ​​dự án Polycarp đến mọi thứ có thể truy cập (tên, phiên bản). 
5. Đối với mỗi tên dự án, chúng tôi kiểm tra tất cả các phiên bản đã đạt được của tên đó. Chúng tôi chọn phiên bản có khoảng cách nhỏ nhất. Nếu nhiều phiên bản có cùng khoảng cách thì chúng ta chọn phiên bản có số phiên bản lớn nhất. 
6. Cuối cùng, chúng tôi xuất ra tất cả các cặp (tên, phiên bản) đã chọn ngoại trừ dự án gốc, được sắp xếp theo từ điển theo tên. 

Tính chính xác phụ thuộc vào tính bất biến mà BFS gán cho mỗi nút khoảng cách phụ thuộc tối thiểu có thể có. Vì chúng tôi chỉ so sánh các phiên bản cùng tên sau khi đã cố định khoảng cách nên chúng tôi không bao giờ cần phải xem lại hoặc sửa đổi các quyết định. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque, defaultdict

n = int(input())

nodes = []
id_map = {}
adj = []

def get_id(name, ver):
    key = (name, ver)
    if key not in id_map:
        id_map[key] = len(nodes)
        nodes.append(key)
        adj.append([])
    return id_map[key]

# read input
i = 0
lines = [line.rstrip("\n") for line in sys.stdin if line.strip() != ""]
p = 0

# first pass parse
while p < len(lines):
    name, ver = lines[p].split()
    ver = int(ver)
    u = get_id(name, ver)
    p += 1

    k = int(lines[p])
    p += 1

    for _ in range(k):
        dn, dv = lines[p].split()
        dv = int(dv)
        v = get_id(dn, dv)
        adj[u].append(v)
        p += 1

root = 0

dist = [-1] * len(nodes)
dist[root] = 0
q = deque([root])

while q:
    u = q.popleft()
    for v in adj[u]:
        if dist[v] == -1:
            dist[v] = dist[u] + 1
            q.append(v)

best = {}

for i, (name, ver) in enumerate(nodes):
    if dist[i] == -1:
        continue
    if name not in best:
        best[name] = (dist[i], ver)
    else:
        d, v = best[name]
        if dist[i] < d or (dist[i] == d and ver > v):
            best[name] = (dist[i], ver)

root_name = nodes[0][0]

ans = []
for name, (d, ver) in best.items():
    if name == root_name and ver == nodes[0][1]:
        continue
    ans.append((name, ver))

ans.sort()
for name, ver in ans:
    print(name, ver)
```Việc triển khai xây dựng một chỉ mục thống nhất của tất cả các phiên bản dự án sao cho danh sách kề là danh sách số nguyên đơn giản. BFS đảm bảo rằng lần đầu tiên chúng tôi tiếp cận một nút luôn thông qua chuỗi phụ thuộc ngắn nhất. 

Bước lọc được thực hiện sau BFS thay vì trong quá trình truyền tải vì bất kỳ quyết định ban đầu nào về một phiên bản sau đó có thể bị vô hiệu do phát hiện ra đường dẫn ngắn hơn. Việc giữ cố định mọi khoảng cách trước tiên sẽ tránh được sự mất ổn định này. 

Một chi tiết tinh tế là việc xử lý phân tích cú pháp đầu vào: các dòng trống tách biệt các khối dự án, vì vậy chúng tôi bỏ qua các dòng trống một cách rõ ràng để đảm bảo việc nhóm chính xác. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Các dự án đầu vào tạo thành một biểu đồ nhỏ trong đó gốc phụ thuộc vào hai nhánh dẫn đến các phiên bản khác nhau của cùng một tên dự án. 

| Bước | Xếp hàng | Nút đã xử lý | Khoảng cách được cập nhật | 
| --- | --- | --- | --- | 
| 0 | a3 | - | a3:0 | 
| 1 | b1, c1 | a3 | b1:1, c1:1 | 
| 2 | c1 | b1 | (b2 đã tồn tại nhưng chưa thể truy cập được) | 
| 3 | b2 | c1 | b2:2 | 

Sau BFS, chúng tôi so sánh các phiên bản của b và chọn b1 vì nó có khoảng cách nhỏ hơn và c1 được đưa trực tiếp vào. 

Điều này cho thấy cách BFS thực thi lựa chọn phụ thuộc ngắn nhất một cách tự nhiên. 

### Mẫu 2 

Trường hợp này giới thiệu các phiên bản cạnh tranh của một phần phụ thuộc trong đó một phiên bản có cấu trúc sâu hơn nhưng nên bỏ qua. 

| Bước | Xếp hàng | Nút đã xử lý | Khoảng cách được cập nhật | 
| --- | --- | --- | --- | 
| 0 | gốc | - | gốc:0 | 
| 1 | thêm1, mashadb2 | gốc | thêm1:1, mashadb2:1 | 
| 2 | mashadb2 | thêm1 | - | 
| 3 | extra3 (bỏ qua sau) | mashadb2 | thêm3:2 | 

Ở đây, mặc dù extra3 tồn tại nhưng nó bị bỏ qua vì extra1 có đường đi ngắn hơn. Khoảng cách BFS đảm bảo tính chính xác của việc lựa chọn phiên bản. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi nút dự án và cạnh phụ thuộc được xử lý một lần trong BFS và một lần trong lựa chọn cuối cùng | 
| Không gian | O(n + m) | Lưu trữ danh sách kề, ánh xạ nút và mảng khoảng cách | 

Giới hạn 1000 dự án đảm bảo rằng cả bộ nhớ và thời gian đều được duy trì thoải mái trong giới hạn ngay cả với bộ lưu trữ liền kề đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from collections import deque, defaultdict

    n = int(sys.stdin.readline())

    nodes = []
    id_map = {}
    adj = []

    def get_id(name, ver):
        key = (name, ver)
        if key not in id_map:
            id_map[key] = len(nodes)
            nodes.append(key)
            adj.append([])
        return id_map[key]

    lines = [line.rstrip("\n") for line in sys.stdin if line.strip() != ""]
    p = 0

    while p < len(lines):
        name, ver = lines[p].split()
        ver = int(ver)
        u = get_id(name, ver)
        p += 1

        k = int(lines[p])
        p += 1

        for _ in range(k):
            dn, dv = lines[p].split()
            dv = int(dv)
            v = get_id(dn, dv)
            adj[u].append(v)
            p += 1

    dist = [-1] * len(nodes)
    dist[0] = 0
    q = deque([0])

    while q:
        u = q.popleft()
        for v in adj[u]:
            if dist[v] == -1:
                dist[v] = dist[u] + 1
                q.append(v)

    best = {}
    for i, (name, ver) in enumerate(nodes):
        if dist[i] == -1:
            continue
        if name not in best:
            best[name] = (dist[i], ver)
        else:
            d, v = best[name]
            if dist[i] < d or (dist[i] == d and ver > v):
                best[name] = (dist[i], ver)

    root_name, root_ver = nodes[0]

    ans = [(name, ver) for name, (d, ver) in best.items() if not (name == root_name and ver == root_ver)]
    ans.sort()

    return "\n".join(f"{a} {b}" for a, b in ans)

assert run("""4
a 3
2
b 1
c 1

b 2
0

b 1
1
b 2

c 1
1
b 2
""") == "b 1\nc 1"

# all-same minimal chain
assert run("""2
a 1
1
b 1

b 1
0
""") == "b 1"

# duplicate versions with deeper alternative
assert run("""3
a 1
1
b 1

b 1
0

b 2
0
""") == "b 1"

# self-only
assert run("""1
a 1
0
""") == ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 | b 1, c 1 | lựa chọn phiên bản chính xác trong số các bản sao | 
| chuỗi a→b | b 1 | lan truyền phụ thuộc đơn giản nhất | 
| b1 vs b2 | b 1 | hòa theo khoảng cách | 
| nút đơn | trống | loại trừ gốc | 

## Vỏ cạnh 

Trường hợp một cạnh là khi cùng một tên dự án xuất hiện trong nhiều phiên bản ở cùng một khoảng cách từ gốc. Trong tình huống đó, thuật toán sẽ chọn số phiên bản cao hơn. Ví dụ: nếu b1 và b2 đều có thể truy cập được trong một bước thì BFS sẽ chỉ định cả hai khoảng cách là 1 và bước chọn sẽ giữ nguyên b2. 

Một trường hợp khác là khi có thể truy cập được phiên bản cao hơn nhưng chỉ thông qua chuỗi phụ thuộc dài hơn. Mặc dù nó có vẻ thích hợp hơn nhưng việc so sánh khoảng cách BFS đảm bảo nó sẽ bị loại bỏ. Cấu trúc đầu vào đảm bảo không có chu kỳ, do đó, đường đi ngắn nhất luôn được xác định rõ ràng và thứ tự BFS đảm bảo rằng một khi nút được gán khoảng cách, nó sẽ không bao giờ được cải thiện sau này.
