---
title: "CF 105575I - \u5fc3\u610f\u65e0\u5411\uff0c\u524d\u7a0b\u6709\u5411"
description: "Chúng ta được cung cấp một cấu trúc đồ thị với các ràng buộc có hướng và các mối quan hệ vô hướng trên cùng một tập đỉnh."
date: "2026-06-22T17:43:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105575
codeforces_index: "I"
codeforces_contest_name: "Southeast University 6th Programming Competition Competition (Winter, Novice Group) \u4e1c\u5357\u5927\u5b66\u7b2c\u516d\u5c4a\u7a0b\u5e8f\u8bbe\u8ba1\u7ade\u8d5b\u51ac\u5b63\u8d5b\uff08\u65b0\u624b\u7ec4\uff09"
rating: 0
weight: 105575
solve_time_s: 57
verified: true
draft: false
---

[CF 105575I - \u5fc3\u610f\u65e0\u5411\uff0c\u524d\u7a0b\u6709\u5411](https://codeforces.com/problemset/problem/105575/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một cấu trúc đồ thị với các ràng buộc có hướng và các mối quan hệ vô hướng trên cùng một tập đỉnh. Các cạnh có hướng thể hiện các quy tắc sắp xếp nghiêm ngặt không thể vi phạm, trong khi các cạnh không có hướng rất linh hoạt và phải được gán một hướng trong kết quả cuối cùng. 

Nhiệm vụ là quyết định xem có thể định hướng tất cả các cạnh vô hướng để toàn bộ đồ thị trở thành không tuần hoàn trong khi vẫn giữ nguyên các cạnh có hướng đã cho hay không. Nếu có thể, chúng ta phải đưa ra hướng nhất quán cho mọi cạnh vô hướng; nếu không chúng ta sẽ xuất ra rằng không có cấu trúc hợp lệ nào tồn tại. 

Khó khăn chính là các cạnh có hướng đã áp đặt các ràng buộc thứ tự một phần. Nếu những ràng buộc đó chứa một chu trình, thì việc gán hướng cho các cạnh vô hướng có thể khắc phục được mâu thuẫn, bởi vì các cạnh có hướng là bất biến. Nếu phần được định hướng là không tuần hoàn thì các cạnh vô hướng luôn có thể được định hướng một cách nhất quán với một trật tự tổng thể bắt nguồn từ cấu trúc không tuần hoàn đó. 

Các ràng buộc ngụ ý một biểu đồ có tối đa 200000 đỉnh và cạnh. Điều này buộc mọi giải pháp phải chạy theo thời gian tuyến tính hoặc gần tuyến tính. Bất kỳ hành vi bậc hai nào trên các cạnh hoặc đỉnh sẽ thất bại ngay lập tức. Điều này loại trừ các phương pháp cố gắng kiểm tra tính nhất quán bằng cách liên tục mô phỏng các hướng của cạnh hoặc thử thực hiện các phép quay lui. 

Một trường hợp thất bại tinh vi xuất hiện khi các cạnh có hướng tạo thành một chu trình mặc dù tồn tại các cạnh vô hướng dường như có khả năng “phá vỡ” nó bằng trực giác. Ví dụ: nếu 1 → 2, 2 → 3, 3 → 1 đều là các cạnh có hướng thì câu trả lời là không thể ngay lập tức, ngay cả khi có nhiều cạnh vô hướng. Một cách tiếp cận ngây thơ cố gắng định hướng các cạnh vô hướng trước tiên có thể tin tưởng một cách không chính xác rằng nó có thể tránh được chu trình, nhưng các chu kỳ có hướng đã gây tai hại. 

Một trường hợp cạnh khác là khi không có cạnh nào được định hướng cả. Trong trường hợp đó, bất kỳ thứ tự nào của các đỉnh đều có tác dụng, vì vậy chúng ta luôn có thể tạo ra hướng hợp lệ cho các cạnh vô hướng. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là coi mọi cạnh vô hướng là một lựa chọn nhị phân và thử tất cả các hướng có thể. Sau khi chỉ định hướng, chúng tôi sẽ kiểm tra xem đồ thị có hướng thu được có phải là không tuần hoàn hay không bằng cách sử dụng sắp xếp tôpô hoặc phát hiện chu trình DFS. Vì có thể có tới O(m2) cạnh vô hướng trong trường hợp xấu nhất, phương pháp này sẽ phát triển thành các cấu hình 2^m và thậm chí một lần kiểm tra tính hợp lệ cũng tốn O(n + m), khiến nó hoàn toàn không khả thi. 

Quan sát quan trọng là các cạnh có hướng xác định một phần thứ tự. Nếu thứ tự một phần này là nhất quán, nghĩa là nó không có chu trình, thì nó có thể được mở rộng thành một thứ tự tuyến tính đầy đủ của tất cả các đỉnh. Khi chúng ta có thứ tự tuyến tính như vậy, mọi cạnh vô hướng có thể được định hướng một cách đơn giản từ đầu đến cuối theo thứ tự đó. Điều này tự động đảm bảo rằng không có chu trình nào được đưa ra vì mọi cạnh đều tôn trọng một thứ tự toàn cầu duy nhất. 

Vì vậy, toàn bộ vấn đề giảm xuống còn việc kiểm tra xem đồ thị con có hướng có phải là DAG hay không và nếu có thì xây dựng một thứ tự tôpô. Một khi thứ tự đó tồn tại, tất cả các cạnh vô hướng sẽ trở nên tầm thường để định hướng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^m · (n + m)) | O(n + m) | Quá chậm | 
| Tối ưu (Sắp xếp tôpô) | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Xây dựng đồ thị có hướng chỉ sử dụng các cạnh có hướng. Tính độ cho tất cả các đỉnh. Điều này tách biệt các ràng buộc bắt buộc và phải được thỏa mãn bất kể các cạnh vô hướng được xử lý như thế nào. 
2. Chạy thuật toán Kahn tiêu chuẩn để sắp xếp cấu trúc liên kết bằng cách sử dụng hàng đợi. Bắt đầu với tất cả các đỉnh có bậc bằng 0. Các đỉnh này không có điều kiện tiên quyết và có thể xuất hiện sớm nhất theo bất kỳ thứ tự hợp lệ nào. 
3. Liên tục loại bỏ một đỉnh khỏi hàng đợi, gán cho nó vị trí tiếp theo trong thứ tự toàn cục và giảm độ của các đỉnh lân cận. Bất cứ khi nào mức độ của hàng xóm trở thành 0, hãy đẩy nó vào hàng đợi. 
4. Nếu ở cuối có ít hơn n đỉnh được xử lý thì sẽ tồn tại một chu trình trong đồ thị có hướng. Trong trường hợp này, không có định hướng hợp lệ nào có thể tồn tại vì các ràng buộc định hướng vốn đã mâu thuẫn. 
5. Nếu thu được thứ tự đầy đủ, hãy sử dụng nó để gán hướng cho tất cả các cạnh vô hướng. Đối với mỗi cạnh vô hướng (u, v), so sánh vị trí của chúng theo thứ tự tôpô và hướng cạnh từ đỉnh trước đến đỉnh sau. 

### Tại sao nó hoạt động 

Thứ tự tôpô xác định một thứ tự tổng thể chặt chẽ phù hợp với tất cả các cạnh có hướng. Mọi cạnh có hướng u → v đảm bảo rằng u xuất hiện trước v theo thứ tự này. Bất kỳ cạnh nào được định hướng theo thứ tự này không bao giờ có thể tạo ra một chu trình có hướng, bởi vì các cạnh sau luôn tăng vị trí theo thứ tự một cách nghiêm ngặt. Vì mọi cạnh vô hướng buộc phải tuân theo quy tắc đơn điệu này nên đồ thị cuối cùng thừa hưởng tính chu trình trực tiếp từ thứ tự tuyến tính. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m1, m2 = map(int, input().split())

    undirected = []
    adj = [[] for _ in range(n + 1)]
    indeg = [0] * (n + 1)

    # undirected edges
    for _ in range(m1):
        u, v = map(int, input().split())
        undirected.append((u, v))

    # directed edges
    for _ in range(m2):
        u, v = map(int, input().split())
        adj[u].append(v)
        indeg[v] += 1

    from collections import deque
    q = deque([i for i in range(1, n + 1) if indeg[i] == 0])

    topo = []
    while q:
        u = q.popleft()
        topo.append(u)
        for v in adj[u]:
            indeg[v] -= 1
            if indeg[v] == 0:
                q.append(v)

    if len(topo) != n:
        print(-1)
        return

    pos = [0] * (n + 1)
    for i, v in enumerate(topo):
        pos[v] = i

    for u, v in undirected:
        if pos[u] < pos[v]:
            print(u, v)
        else:
            print(v, u)

t = 1
for _ in range(t):
    solve()
```Giải pháp tách các cạnh vô hướng và có hướng ngay lập tức, vì chỉ những cạnh có hướng mới ảnh hưởng đến tính khả thi. Danh sách kề và mảng đẳng cấp được sử dụng hoàn toàn cho thuật toán Kahn. Hàng đợi ban đầu chứa tất cả các đỉnh không có cạnh có hướng đi vào, đảm bảo chúng ta luôn mở rộng thứ tự một phần hợp lệ. 

Bước quan trọng là xác minh xem liệu sắp xếp tôpô có bao gồm tất cả các đỉnh hay không. Nếu không, các ràng buộc trực tiếp đã mâu thuẫn với chính chúng, do đó việc xử lý tiếp theo không còn ý nghĩa nữa. 

Sau khi tính toán thứ tự, chúng ta ánh xạ từng đỉnh tới vị trí của nó. Điều này cho phép so sánh theo thời gian không đổi đối với mọi cạnh vô hướng. Mỗi cạnh vô hướng được định hướng chặt chẽ theo trật tự toàn cầu này, đảm bảo tính nhất quán. 

## Ví dụ đã hoạt động 

Xét trường hợp có cấu trúc có hướng nhỏ: 

đầu vào: 

n = 4 

cạnh có hướng: 1 → 2, 2 → 3 

các cạnh vô hướng: (1, 3), (4, 2) 

Quá trình tôpô: 

| Bước | Xếp hàng | Xuất hiện | topo | thay đổi mức độ | 
| --- | --- | --- | --- | --- | 
| 1 | [1,4] | 1 | [1] | 2→0 | 
| 2 | [4,2] | 4 | [1,4] | không | 
| 3 | [2] | 2 | [1,4,2] | 3→0 | 
| 4 | [3] | 3 | [1,4,2,3] | xong | 

Thứ tự cuối cùng là [1,4,2,3]. Bây giờ định hướng các cạnh vô hướng: 

Cạnh (1,3): 1 trước 3, trở thành 1 → 3 

Cạnh (4,2): 4 trước 2, trở thành 4 → 2 

Điều này chứng tỏ rằng một khi trật tự tôpô tồn tại, tất cả các cạnh vô hướng sẽ trở nên nhất quán với nó. 

Bây giờ hãy xem xét một trường hợp thất bại: 

đầu vào: 

n = 3 

các cạnh có hướng: 1 → 2, 2 → 3, 3 → 1 

các cạnh vô hướng: không có 

Hàng đợi trở nên trống ngay lập tức hoặc kết thúc với ít hơn 3 nút được xử lý. Thuật toán phát hiện rằng không phải tất cả các nút đều được bao gồm trong topo, vì vậy đầu ra là -1. Điều này cho thấy các chu kỳ có định hướng không thể sửa chữa được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m1 + m2) | Mỗi đỉnh vào và ra khỏi hàng đợi một lần, mỗi cạnh được xử lý một lần | 
| Không gian | O(n + m1 + m2) | danh sách kề, mảng bậc và lưu trữ thứ tự | 

Các ràng buộc cho phép tối đa 200000 phần tử, do đó độ phức tạp tuyến tính là đủ. Thuật toán xử lý mỗi cạnh một số lần không đổi, vừa vặn trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read()

# Since full IO solver is embedded above, these are conceptual placeholders
# In a real setup, solve() would be imported and called directly

# sample-style cycle case
# assert run("3 0 3\n1 2\n2 3\n3 1\n") == "-1"

# minimal acyclic
# assert run("2 1 0\n1 2\n") in ["1 2\n"]

# mixed case
# assert run("4 2 2\n1 3\n2 4\n1 2\n3 4\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Chu kỳ định hướng 3 nút | -1 | độ chính xác phát hiện chu kỳ | 
| 2 nút cạnh đơn | định hướng hợp lệ | độ đúng cơ sở | 
| đồ thị hỗn hợp | định hướng nhất quán | tích hợp topo + xử lý vô hướng | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi không có cạnh nào được định hướng cả. Trong trường hợp đó, mọi đỉnh ban đầu đều có bậc bằng 0, do đó hàng đợi bắt đầu với tất cả các nút. Mọi thứ tự tôpô đều hợp lệ và thuật toán tạo ra hoán vị đầy đủ ngay lập tức. Tất cả các cạnh vô hướng sau đó được định hướng tùy ý theo hoán vị đó, hoán vị này luôn hợp lệ. 

Một trường hợp cạnh khác là khi các cạnh được định hướng tạo thành một chuỗi cộng với các nút bị cô lập. Thứ tự tôpô sẽ đặt các nút biệt lập ở các vị trí tùy ý so với chuỗi, nhưng điều này không ảnh hưởng đến tính chính xác vì các nút biệt lập không áp đặt ràng buộc nào. 

Trường hợp cạnh lỗi là một chu trình được định hướng ẩn giữa nhiều nút. Ví dụ: nếu chỉ có ba nút tạo thành một chu trình bên trong biểu đồ lớn hơn, thuật toán của Kahn sẽ sớm bị đình trệ. Kiểm tra độ dài topo cuối cùng sẽ phát hiện điều này mà không cần phải tìm kiếm chu kỳ một cách rõ ràng.
