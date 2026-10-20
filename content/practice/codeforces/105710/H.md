---
title: "CF 105710H - Phẫu thuật thùy não"
description: "Chúng ta có một mạng lưới các nơ-ron, trong đó mỗi nơ-ron là một nút và mỗi khớp thần kinh là một kết nối vô hướng giữa hai nơ-ron. Không giống như một biểu đồ đơn giản tiêu chuẩn, nhiều khớp thần kinh có thể kết nối cùng một cặp nơ-ron và một khớp thần kinh thậm chí có thể kết nối một nơ-ron với chính nó."
date: "2026-06-26T08:01:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105710
codeforces_index: "H"
codeforces_contest_name: "UTPC Contest 2-12-25 Div. 1 (Advanced)"
rating: 0
weight: 105710
solve_time_s: 70
verified: true
draft: false
---

[CF 105710H - Phẫu thuật thùy não](https://codeforces.com/problemset/problem/105710/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 10s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một mạng lưới các nơ-ron, trong đó mỗi nơ-ron là một nút và mỗi khớp thần kinh là một kết nối vô hướng giữa hai nơ-ron. Không giống như một biểu đồ đơn giản tiêu chuẩn, nhiều khớp thần kinh có thể kết nối cùng một cặp nơ-ron và một khớp thần kinh thậm chí có thể kết nối một nơ-ron với chính nó. 

Bác sĩ muốn thực hiện chính xác một lần cắt, nghĩa là chúng tôi loại bỏ một khớp thần kinh. Một khớp thần kinh được coi là nguy hiểm nếu việc loại bỏ nó sẽ khiến não thành ít nhất hai thành phần bị ngắt kết nối, do đó một số tế bào thần kinh không thể tiếp cận được các tế bào thần kinh khác nữa. Nhiệm vụ là xác định tất cả các khớp thần kinh mà việc loại bỏ sẽ ngắt kết nối biểu đồ và xuất mã định danh của chúng theo thứ tự được sắp xếp. Nếu không có khớp thần kinh như vậy tồn tại, chúng ta sẽ xuất -1. 

Mỗi khớp thần kinh có một ID số nguyên duy nhất và chúng ta phải suy luận theo các ID này, không chỉ các điểm cuối. 

Sự hạn chế về số lượng tế bào thần kinh và khớp thần kinh lên tới vài trăm nghìn. Điều này ngay lập tức loại trừ mọi phương pháp mô phỏng lại bậc hai hoặc theo từng cạnh. Bất kỳ giải pháp nào cố gắng loại bỏ từng cạnh và chạy BFS hoặc DFS sẽ có giá O(m(n + m)), con số này quá lớn. 

Điểm tinh tế quan trọng là biểu đồ không hề đơn giản. Các cạnh song song thay đổi hoàn toàn định nghĩa về kết nối “quan trọng”. Ví dụ: nếu hai khớp thần kinh kết nối cùng một cặp nơ-ron, việc loại bỏ một trong số chúng sẽ không bao giờ ngắt kết nối cặp đó, ngay cả khi cấu trúc bên dưới rất dễ vỡ. 

Một cách tiếp cận ngây thơ mà bỏ qua điều này sẽ thất bại trong các trường hợp như: 

đầu vào:```
3 2
1 0 1
2 0 1
```Ở đây có hai khớp thần kinh song song giữa cùng một cặp nút. Mặc dù các điểm cuối trông giống như một cây cầu trong một biểu đồ đơn giản, nhưng không nên loại bỏ cạnh nào vì cạnh kia vẫn duy trì kết nối. Một thuật toán tìm cầu đơn giản sẽ gắn nhãn sai cho chúng là cầu nối. 

Một chế độ lỗi khác xảy ra với các vòng tự lặp. Một khớp thần kinh từ một nơ-ron đến chính nó không bao giờ đóng góp vào sự kết nối giữa các thành phần khác nhau, vì vậy việc loại bỏ nó không bao giờ có thể ngắt kết nối biểu đồ. Đối xử với nó như một cạnh bình thường sẽ dẫn đến việc đưa vào không chính xác. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ lặp lại trên mọi khớp thần kinh, loại bỏ nó và chạy DFS để kiểm tra xem biểu đồ có còn được kết nối hay không. Mỗi lần kiểm tra kết nối tốn O(n + m), do đó tổng độ phức tạp trở thành O(m(n + m)). Với tối đa 200.000 cạnh, điều này là không thể thực hiện được. 

Cấu trúc của bài toán cho thấy chúng ta thực sự đang được yêu cầu tìm những cây cầu trong đồ thị vô hướng. Cầu là một cạnh mà khi bị loại bỏ sẽ làm tăng số lượng các thành phần được kết nối. Giải pháp cổ điển là thuật toán của Tarjan sử dụng dấu thời gian DFS và các giá trị liên kết thấp, tính toán tất cả các cầu nối theo thời gian tuyến tính O(n + m). 

Tuy nhiên, sự hiện diện của nhiều cạnh sẽ gây ra sự phức tạp. Thuật toán của Tarjan giả định một biểu đồ đơn giản. Nếu hai nút được kết nối bởi nhiều hơn một cạnh thì không có cạnh nào trong số đó được coi là cầu nối vì luôn có ít nhất một tuyến đường trực tiếp thay thế. 

Điều này dẫn đến việc sàng lọc khóa: trước tiên chúng tôi nén cấu trúc đa đồ thị bằng cách theo dõi số lượng khớp thần kinh tồn tại giữa mỗi cặp nút không có thứ tự. Sau đó, chúng tôi chạy DFS tìm cầu tiêu chuẩn, nhưng chúng tôi chỉ chấp nhận một cạnh là cầu nối hợp lệ nếu đó là cầu nối theo nghĩa DFS và đó là khớp thần kinh duy nhất giữa các điểm cuối của nó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Xóa từng cạnh + DFS | O(m(n + m)) | O(n + m) | Quá chậm | 
| Cầu Tarjan với tính năng lọc đa bội | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi coi mỗi khớp thần kinh là một cạnh riêng biệt có ID nhưng chúng tôi cũng nhóm các cạnh theo cặp điểm cuối của chúng. 

1. Chúng tôi xây dựng một danh sách lân cận lưu trữ, đối với mỗi nơ-ron, tất cả các khớp thần kinh sự cố cùng với đích đến và ID của chúng. Đồng thời, chúng tôi đếm xem có bao nhiêu khớp thần kinh kết nối mỗi cặp nơ-ron không có thứ tự. Điều này sau này cho phép chúng ta phân biệt các cạnh duy nhất với các cạnh trùng lặp. 
2. Chúng tôi chạy DFS trên biểu đồ để tính toán thời gian khám phá và giá trị liên kết thấp. Các giá trị này nắm bắt tổ tiên có thể tiếp cận sớm nhất từ ​​mỗi cây con, đây là công cụ tiêu chuẩn để phát hiện các cầu nối. 
3. Trong DFS, khi chúng ta duyệt một cạnh từ u đến v, chúng ta bỏ qua cạnh cây ngược ngay lập tức nhưng mặt khác áp dụng logic Tarjan tiêu chuẩn: sau khi truy cập v, chúng ta kiểm tra xem low[v] > disc[u] hay không. Nếu bất đẳng thức này đúng thì cạnh (u, v) là ứng cử viên cho cầu nối. 
4. Trước khi hoàn thiện cạnh đó làm cầu nối, chúng tôi kiểm tra xem cặp (u, v) có nhiều hơn một khớp thần kinh hay không. Nếu đúng như vậy, chúng tôi sẽ loại bỏ tất cả các cạnh giữa u và v khỏi vai trò là cầu nối, vì tính dư thừa đảm bảo khả năng kết nối ngay cả sau khi loại bỏ. 
5. Chúng tôi thu thập tất cả ID khớp thần kinh thỏa mãn cả hai điều kiện: chúng là cầu nối DFS và thuộc về một cặp cạnh duy nhất. 
6. Cuối cùng, chúng tôi sắp xếp ID kết quả và xuất chúng. Nếu không tồn tại, chúng tôi xuất -1. 

Tại sao nó hoạt động được gắn với hai bất biến. Đầu tiên, các giá trị liên kết thấp DFS mô tả chính xác liệu một cây con có cạnh sau thay thế cho cây tổ tiên hay không, đây chính xác là điều kiện để một cạnh không cần thiết trong kết nối. Thứ hai, tính đa cạnh đóng vai trò như một sự đảm bảo về cấu trúc độc lập: nếu hai đỉnh chia sẻ ít nhất hai cạnh, việc loại bỏ một cạnh không thể ngắt kết nối chúng, bất kể cấu trúc DFS. Việc kết hợp hai yếu tố này đảm bảo rằng mọi khớp thần kinh được báo cáo thực sự là kết nối quan trọng duy nhất giữa hai phần của biểu đồ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**7)

n, m = map(int, input().split())

edges = []
adj = [[] for _ in range(n)]
pair_count = {}

for i in range(m):
    eid, u, v = map(int, input().split())
    edges.append((u, v, eid))
    adj[u].append((v, i))
    adj[v].append((u, i))

    a, b = (u, v) if u <= v else (v, u)
    pair_count[(a, b)] = pair_count.get((a, b), 0) + 1

disc = [-1] * n
low = [0] * n
timer = 0
is_bridge = [False] * m

def dfs(u, pe):
    global timer
    disc[u] = low[u] = timer
    timer += 1

    for v, eid in adj[u]:
        if eid == pe:
            continue

        if disc[v] == -1:
            dfs(v, eid)
            low[u] = min(low[u], low[v])

            if low[v] > disc[u]:
                is_bridge[eid] = True
        else:
            low[u] = min(low[u], disc[v])

for i in range(n):
    if disc[i] == -1:
        dfs(i, -1)

res = []

for i, (u, v, eid) in enumerate(edges):
    a, b = (u, v) if u <= v else (v, u)
    if is_bridge[i] and pair_count[(a, b)] == 1:
        res.append(eid)

if not res:
    print(-1)
else:
    res.sort()
    print(*res)
```DFS duy trì các mảng khám phá và liên kết thấp chính xác như trong thuật toán cầu tiêu chuẩn. Điểm khác biệt duy nhất là chúng tôi theo dõi rõ ràng các danh tính cạnh, vì nhiều khớp thần kinh có thể tồn tại giữa cùng một điểm cuối. các`pe`tham số ngăn cản việc truy cập lại ngay lập tức cạnh mà chúng ta đã xuất phát, điều này cần thiết trong DFS vô hướng để tránh các chu kỳ sai. 

các`pair_count`bản đồ thực thi hiệu chỉnh đa đồ thị. Ngay cả khi Tarjan đánh dấu một cạnh là một cây cầu về mặt cấu trúc, chúng tôi chỉ chấp nhận nó khi đó là kết nối duy nhất giữa các điểm cuối của nó. 

Một chi tiết triển khai tinh tế là chúng tôi lưu trữ thông tin cầu nối trên mỗi chỉ mục cạnh chứ không phải trên mỗi cặp nút vì nhiều khớp thần kinh riêng biệt có thể kết nối cùng một nút. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 4
212 3 0
238 1 2
394 2 0
281 0 1
```| Bước | Nút hiện tại | Hành động DFS | cập nhật thấp | những cây cầu được tìm thấy | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | bắt đầu DFS | thấp[0]=0 | không | 
| 2 | 1 | tham quan từ 0 | thấp[1]=1 | không | 
| 3 | 2 | thăm từ 1 | thấp[2]=2 | ứng cử viên cạnh 238 | 
| 4 | 0 | khám phá cạnh tiếp theo | cập nhật thấp qua các cạnh sau | không | 
| 5 | kết thúc | kiểm tra các cạnh | xác thực tính duy nhất | 212 giữ | 

Cạnh duy nhất trở nên quan trọng là khớp thần kinh 212, vì việc loại bỏ nó sẽ tách một cây con không thể kết nối lại thông qua bất kỳ kết nối song song hoặc cạnh sau thay thế nào. 

Dấu vết này cho thấy một cạnh của cây DFS đơn lẻ chỉ trở thành cầu nối như thế nào khi không có cạnh sau nào tồn tại đối với tổ tiên cao hơn. 

### Ví dụ 2 

đầu vào:```
3 3
101 0 1
102 0 2
103 2 1
```| Bước | Cây DFS | giá trị thấp | tình trạng cầu | 
| --- | --- | --- | --- | 
| 0 | Chu kỳ hình thành cấu trúc 0-1-2 | tất cả các nút có thể truy cập | không low[v] > disc[u] | 
| 1 | phát hiện chu kỳ | liên kết thấp sụp đổ | không có cầu | 

Ở đây mọi cạnh đều tham gia vào một chu trình, vì vậy mỗi nút có một tuyến đường thay thế. Các giá trị liên kết thấp luôn truyền trở lại thư mục gốc, ngăn chặn bất kỳ tình trạng cầu nào được kích hoạt. 

Đầu ra cuối cùng là -1, xác nhận rằng toàn bộ biểu đồ vẫn được kết nối dưới bất kỳ lần cắt nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | DFS truy cập từng nút và cạnh một lần, với việc kiểm tra cầu liên tục | 
| Không gian | O(n + m) | danh sách kề, ngăn xếp đệ quy và mảng phụ trợ | 

Độ phức tạp tuyến tính là cần thiết cho tới 200.000 khớp thần kinh. Bất kỳ giải pháp nào dựa vào việc truyền tải đồ thị lặp đi lặp lại sẽ vượt quá giới hạn thời gian, trong khi phương pháp này xử lý cấu trúc trong một lần duy nhất. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    input = _sys.stdin.readline

    n, m = map(int, input().split())
    edges = []
    adj = [[] for _ in range(n)]
    pair_count = {}

    for i in range(m):
        eid, u, v = map(int, input().split())
        edges.append((u, v, eid))
        adj[u].append((v, i))
        adj[v].append((u, i))
        a, b = (u, v) if u <= v else (v, u)
        pair_count[(a, b)] = pair_count.get((a, b), 0) + 1

    sys.setrecursionlimit(10**7)
    disc = [-1]*n
    low = [0]*n
    timer = 0
    is_bridge = [False]*m

    def dfs(u, pe):
        nonlocal timer
        disc[u] = low[u] = timer
        timer += 1
        for v, eid in adj[u]:
            if eid == pe:
                continue
            if disc[v] == -1:
                dfs(v, eid)
                low[u] = min(low[u], low[v])
                if low[v] > disc[u]:
                    is_bridge[eid] = True
            else:
                low[u] = min(low[u], disc[v])

    for i in range(n):
        if disc[i] == -1:
            dfs(i, -1)

    res = []
    for i, (u, v, eid) in enumerate(edges):
        a, b = (u, v) if u <= v else (v, u)
        if is_bridge[i] and pair_count[(a, b)] == 1:
            res.append(eid)

    return " ".join(map(str, sorted(res))) if res else "-1"

# custom cases

# single edge is bridge
assert run("2 1\n5 0 1\n") == "5", "single edge"

# cycle has no bridges
assert run("3 3\n1 0 1\n2 1 2\n3 2 0\n") == "-1", "cycle"

# parallel edges block bridges
assert run("2 2\n10 0 1\n11 0 1\n") == "-1", "parallel edges"

# chain
assert run("4 3\n1 0 1\n2 1 2\n3 2 3\n") == "1 2 3", "chain"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cạnh đơn | 5 | phát hiện cầu cơ bản | 
| chu kỳ | -1 | không có cầu trong chu kỳ | 
| các cạnh song song | -1 | xử lý đa dạng | 
| chuỗi | 1 2 3 | tất cả các cạnh đều là cầu | 

## Vỏ cạnh 

Tự lặp là trường hợp không rõ ràng đơn giản nhất. Đối với đầu vào như:```
2 1
100 0 0
```DFS nhìn thấy một cạnh từ một nút đến chính nó. Nó không bao giờ thỏa mãn điều kiện cầu vì nó không kết nối các thành phần khác nhau ngay từ đầu. Thuật toán bỏ qua nó một cách chính xác vì nó không xuất hiện dưới dạng cạnh cây dẫn đến nút mới cũng như không thỏa mãn low[v] > disc[u]. 

Các cạnh song song giữa các nút giống nhau chứng minh tại sao chỉ cấu trúc DFS là không đủ:```
2 2
1 0 1
2 0 1
```Mặc dù mỗi cạnh riêng lẻ trông giống như một cây cầu ứng cử viên trong một lần truyền tải đơn giản, nhưng việc kiểm tra pair_count sẽ lọc chúng ra. Cả hai điểm cuối vẫn được kết nối sau khi loại bỏ một trong hai khớp thần kinh, do đó cả hai điểm cuối đều không được kết nối. 

Một chuỗi các nút tuyến tính hiển thị trường hợp tích cực dự định:```
4 3
1 0 1
2 1 2
3 2 3
```Mỗi cạnh là kết nối duy nhất giữa các điểm cuối của nó và không có cạnh sau nào tồn tại, vì vậy mỗi cạnh đều thỏa mãn điều kiện cầu và được báo cáo chính xác.
