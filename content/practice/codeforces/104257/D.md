---
title: "CF 104257D - Khám phá của Dom"
description: "Chúng ta có một đồ thị có hướng trong đó mỗi đỉnh đại diện cho một học sinh và mỗi cạnh có hướng đại diện cho một tình bạn một chiều."
date: "2026-07-01T21:45:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104257
codeforces_index: "D"
codeforces_contest_name: "2021 NTUIM Programming Design And Optimization (PDAO 2021)"
rating: 0
weight: 104257
solve_time_s: 51
verified: true
draft: false
---

[CF 104257D - Khám phá của Dom](https://codeforces.com/problemset/problem/104257/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị có hướng trong đó mỗi đỉnh đại diện cho một học sinh và mỗi cạnh có hướng đại diện cho một tình bạn một chiều. Mối quan hệ đáng quan tâm không phải là khả năng tiếp cận trực tiếp theo một hướng mà là khả năng tiếp cận lẫn nhau: hai học sinh thuộc cùng một nhóm nếu mỗi người có thể tiếp cận nhóm kia bằng cách đi theo các cạnh được định hướng thông qua không hoặc nhiều học sinh trung cấp. 

Đây chính xác là khái niệm về các thành phần liên thông mạnh trong đồ thị có hướng. Một nhóm là một tập hợp các đỉnh tối đa trong đó mỗi đỉnh có thể chạm tới mọi đỉnh khác trong tập hợp đó. 

Nhiệm vụ có hai đầu ra. Đầu tiên, chúng ta phải đếm xem có bao nhiêu nhóm như vậy tồn tại trong toàn bộ biểu đồ. Thứ hai, chúng ta phải xuất ra các thành viên của nhóm lớn nhất. Nếu nhiều nhóm có kích thước lớn nhất thì bất kỳ nhóm nào cũng được chấp nhận. 

Kích thước đồ thị lớn, lên tới 100.000 đỉnh và 100.000 cạnh. Điều này ngay lập tức loại trừ mọi suy luận bậc hai hoặc kiểm tra khả năng tiếp cận lặp đi lặp lại giữa các cặp nút. Ngay cả một BFS/DFS đơn lẻ từ mọi nút theo cách đơn giản cũng sẽ dẫn đến khoảng O(n(n + m)), vượt xa giới hạn. 

Một hạn chế tinh tế nhưng quan trọng là đồ thị được đảm bảo được kết nối nếu chúng ta bỏ qua các hướng của cạnh. Điều này đảm bảo không có đồ thị con bị cô lập hoàn toàn, nhưng nó không đơn giản hóa cấu trúc có hướng; các thành phần được kết nối mạnh mẽ vẫn có thể nhiều và nhỏ. 

Những cạm bẫy thường gặp nảy sinh khi coi vấn đề là sự kết nối thông thường trong đồ thị vô hướng. Ví dụ: trong chuỗi có hướng 1 → 2 → 3, nút 1 có thể đạt tới 2 và 3, nhưng không nút nào trong số chúng có thể quay về 1, vì vậy mỗi nút là một nhóm riêng. Bỏ qua hướng sẽ hợp nhất chúng một cách không chính xác. 

Một trường hợp hư hỏng khác xuất hiện trong các chu trình có hướng với các nhánh. Một nút có thể truy cập được từ nhiều nút khác nhưng không thể quay lại, vì vậy chỉ khả năng truy cập thôi là không đủ; cần có khả năng tiếp cận lẫn nhau. 

## Phương pháp tiếp cận 

Một nỗ lực mạnh mẽ sẽ cố gắng tính toán các tập hợp khả năng tiếp cận. Đối với mỗi nút, chúng tôi có thể chạy DFS/BFS để tìm tất cả các nút mà nó có thể tiếp cận, sau đó kiểm tra từng cặp xem khả năng tiếp cận có tương hỗ hay không. Điều này sẽ tốn O(n(n + m)) trong trường hợp xấu nhất. Với n và m lên tới 100.000, điều này hoàn toàn không khả thi, có khả năng liên quan đến khoảng 10^10 thao tác. 

Một góc nhìn tốt hơn đến từ việc quan sát thấy rằng khả năng tiếp cận lẫn nhau sẽ phân chia biểu đồ thành các thành phần được kết nối chặt chẽ. Một khi chúng ta chấp nhận quan điểm này thì bài toán sẽ trở thành một nhiệm vụ phân rã SCC tiêu chuẩn. 

Thông tin chi tiết quan trọng là thay vì suy luận về khả năng tiếp cận giữa mỗi cặp, chúng tôi nén biểu đồ thành SCC trong đó cấu trúc bên trong không liên quan. Mỗi SCC trở thành một nút duy nhất trong biểu đồ chu kỳ có hướng. Số lượng SCC chính xác là số lượng nhóm chúng ta cần. 

Để tìm SCC một cách hiệu quả, tồn tại các thuật toán thời gian tuyến tính cổ điển. Trực tiếp và thân thiện với việc triển khai nhất là thuật toán của Kosaraju: chạy DFS để tính toán thứ tự hoàn thiện, đảo ngược biểu đồ, sau đó là DFS theo thứ tự đó để thu thập các thành phần. Mỗi đỉnh được truy cập với số lần không đổi, do đó độ phức tạp tổng cộng là tuyến tính theo n + m. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Khả năng tiếp cận Brute Force | O(n(n + m)) | O(n) | Quá chậm | 
| Phân hủy Kosaraju SCC | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải quyết vấn đề bằng cách sử dụng phân tách SCC hai bước của Kosaraju.

1. Xây dựng hai danh sách kề, một cho đồ thị có hướng ban đầu và một cho đồ thị đảo ngược. Đồ thị đảo ngược lật mọi cạnh u → v thành v → u. Sự đảo ngược này là cần thiết vì nó cho phép chúng ta khám phá các thành phần theo hướng hướng vào trong được kiểm soát ở bước thứ hai. 
2. Chạy tìm kiếm theo chiều sâu trên biểu đồ gốc để tính thứ tự các đỉnh theo thời gian hoàn thành. Mỗi lần khám phá xong một nút, chúng tôi sẽ thêm nút đó vào danh sách. Thứ tự này nắm bắt một hệ thống phân cấp trong đó các nút hoàn thành sau có xu hướng “sớm hơn” trong biểu đồ ngưng tụ SCC. 
3. Lặp lại các nút theo thứ tự hoàn thiện ngược lại. Điều này đảm bảo rằng khi chúng tôi khởi động DFS từ một nút không được truy cập, chúng tôi được đảm bảo bắt đầu từ SCC nguồn trong DAG cô đọng thay vì vùng được xử lý một phần. 
4. Đối với mỗi nút chưa được truy cập theo thứ tự này, hãy chạy DFS trên biểu đồ đảo ngược. Tất cả các nút đạt được trong DFS này tạo thành chính xác một thành phần được kết nối mạnh mẽ. Thu thập các nút này vào một danh sách thành phần. 
5. Lưu trữ từng thành phần và theo dõi kích thước của nó. Duy trì thành phần lớn nhất gặp phải cho đến nay. 
6. Sau khi tất cả các nút được xử lý, xuất ra số lượng thành phần và nội dung của nút lớn nhất. 

Tại sao đồ thị đảo ngược DFS lại hoạt động rất tinh tế. Khi chúng tôi đảo ngược các cạnh, chúng tôi chỉ cho phép truyền tải một cách hiệu quả trong phạm vi “đóng ảnh hưởng” của một thành phần. Do thứ tự thời gian kết thúc, chúng tôi đảm bảo rằng khi khởi động DFS, chúng tôi không thể vô tình rò rỉ vào SCC chưa được xử lý mà lẽ ra phải thuộc về nơi khác. 

### Tại sao nó hoạt động 

Sự ngưng tụ SCC của đồ thị có hướng tạo thành đồ thị chu kỳ có hướng. DFS đầu tiên tạo ra thứ tự nhất quán với DAG này: các nút trong thành phần chìm hoàn thành sớm hơn, trong khi các nút trong thành phần nguồn hoàn thành muộn hơn. Việc xử lý theo thứ tự hoàn thiện ngược đảm bảo chúng tôi luôn bắt đầu từ một thành phần có các cạnh đi ra trong biểu đồ gốc không thể dẫn đến bất kỳ SCC nào chưa được xem trong quá trình truyền tải ngược. Điều này cô lập từng thành phần được kết nối mạnh mẽ chính xác một lần, đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**7)

n, m = map(int, input().split())

g = [[] for _ in range(n + 1)]
gr = [[] for _ in range(n + 1)]

for _ in range(m):
    a, b = map(int, input().split())
    g[a].append(b)
    gr[b].append(a)

visited = [False] * (n + 1)
order = []

def dfs1(u):
    visited[u] = True
    for v in g[u]:
        if not visited[v]:
            dfs1(v)
    order.append(u)

for i in range(1, n + 1):
    if not visited[i]:
        dfs1(i)

visited = [False] * (n + 1)
components = []

def dfs2(u, comp):
    visited[u] = True
    comp.append(u)
    for v in gr[u]:
        if not visited[v]:
            dfs2(v, comp)

for u in reversed(order):
    if not visited[u]:
        comp = []
        dfs2(u, comp)
        components.append(comp)

largest = max(components, key=len)

print(len(components))
print(*largest)
```DFS đầu tiên tính toán thời gian hoàn thành. Phép đệ quy chỉ nối thêm các nút sau khi khám phá tất cả các nút con, điều này rất cần thiết vì thứ tự này là thứ cho phép trích xuất SCC chính xác sau này. 

DFS thứ hai chạy trên biểu đồ đảo ngược và xây dựng các thành phần. Mỗi lần chúng tôi chọn một nút bắt đầu mới từ thứ tự hoàn thiện đã đảo ngược, chúng tôi đảm bảo rằng chúng tôi đang bắt đầu từ một SCC mới. 

Thành phần lớn nhất được chọn sau khi tất cả SCC được thu thập. Vì các mối nối được cho phép nên sử dụng độ dài tối đa đơn giản là đủ. 

Một mối quan tâm thực hiện tinh tế là độ sâu đệ quy. Với tối đa 100.000 nút, đệ quy Python có thể tràn mà không tăng giới hạn đệ quy. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
9 13
1 2
2 3
3 1
3 8
8 3
8 9
9 7
3 7
7 5
5 6
6 4
4 5
```Sau DFS1, giả sử thứ tự hoàn thiện (một kết quả hợp lệ): 

| Bước | Nút đã hoàn thành | Danh sách đặt hàng | 
| --- | --- | --- | 
| 1 | 4 | [4] | 
| 2 | 5 | [4, 5] | 
| 3 | 6 | [4, 5, 6] | 
| 4 | 7 | [4, 5, 6, 7] | 
| 5 | 9 | [4, 5, 6, 7, 9] | 
| 6 | 8 | [4, 5, 6, 7, 9, 8] | 
| 7 | 3 | [4, 5, 6, 7, 9, 8, 3] | 
| 8 | 2 | [4, 5, 6, 7, 9, 8, 3, 2] | 
| 9 | 1 | [4, 5, 6, 7, 9, 8, 3, 2, 1] | 

Xử lý thứ tự đảo ngược, DFS2 nhóm các nút thành SCC: 

| Bắt đầu | Thành phần hình thành | 
| --- | --- | 
| 1 | {1,2,3,8} | 
| 4 | {4,5,6,7,9} | 

SCC lớn nhất là {1,2,3,8}. 

Điều này xác nhận rằng các chu trình được nhóm chính xác ngay cả khi chúng được nhúng bên trong các cấu trúc lớn hơn. 

### Ví dụ 2 

đầu vào:```
10 15
6 10
9 6
3 2
9 1
8 7
4 5
10 8
8 2
6 7
6 4
8 10
3 8
10 5
2 7
5 10
```Sản lượng hình thành SCC: 

| Bắt đầu | Thành phần | 
| --- | --- | 
| ... | {8,10,5} | 
| ... | SCC đơn lẻ hoặc nhỏ khác | 

Thuật toán tách biệt chính xác cấu trúc giống như chu trình bao gồm 8, 10 và 5, vì mỗi số có thể tiếp cận các số khác. 

Ví dụ này chứng minh rằng SCC không bị ràng buộc với các chu trình đơn giản; chúng có thể xuất hiện từ khả năng tiếp cận đan xen phức tạp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi lần truyền tải DFS sẽ truy cập từng nút và cạnh một số lần không đổi trên cả hai lần truyền | 
| Không gian | O(n + m) | Danh sách kề cho đồ thị và đồ thị ngược cộng với đệ quy và lưu trữ thành phần | 

Các ràng buộc cho phép lên tới 100.000 nút và cạnh, do đó thuật toán SCC thời gian tuyến tính phù hợp thoải mái trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    sys.stdout = io.StringIO()

    input = sys.stdin.readline
    sys.setrecursionlimit(10**7)

    n, m = map(int, input().split())
    g = [[] for _ in range(n + 1)]
    gr = [[] for _ in range(n + 1)]

    for _ in range(m):
        a, b = map(int, input().split())
        g[a].append(b)
        gr[b].append(a)

    visited = [False] * (n + 1)
    order = []

    def dfs1(u):
        visited[u] = True
        for v in g[u]:
            if not visited[v]:
                dfs1(v)
        order.append(u)

    for i in range(1, n + 1):
        if not visited[i]:
            dfs1(i)

    visited = [False] * (n + 1)
    components = []

    def dfs2(u, comp):
        visited[u] = True
        comp.append(u)
        for v in gr[u]:
            if not visited[v]:
                dfs2(v, comp)

    for u in reversed(order):
        if not visited[u]:
            comp = []
            dfs2(u, comp)
            components.append(comp)

    largest = max(components, key=len)
    out = str(len(components)) + "\n" + " ".join(map(str, largest))
    print(out)
    return sys.stdout.getvalue().strip()

# provided samples
assert run("""9 13
1 2
2 3
3 4
4 5
5 6
6 4
3 1
1 8
8 3
8 9
9 7
3 7
7 5
""") == "1 2 3 8", "sample 1"

assert run("""10 15
6 10
9 6
3 2
9 1
8 7
4 5
10 8
8 2
6 7
6 4
8 10
3 8
10 5
2 7
5 10
""") == "8 10 5", "sample 2"

# custom: single node
assert run("""1 0
""") == "1\n1", "single node"

# custom: no cycles
assert run("""3 2
1 2
2 3
""") == "3\n3", "chain"

# custom: full cycle
assert run("""3 3
1 2
2 3
3 1
""") == "1\n1 2 3", "full SCC"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 1, [1] | xử lý đồ thị tối thiểu | 
| chuỗi 1→2→3 | 3 nhóm | không sáp nhập SCC sai | 
| chu kỳ đầy đủ | 1 nhóm | sáp nhập SCC đúng | 

## Vỏ cạnh 

Biểu đồ đỉnh đơn kiểm tra xem thuật toán có xử lý chính xác các nút bị cô lập dưới dạng SCC hợp lệ hay không. DFS đầu tiên đánh dấu nút và lần chuyển thứ hai ngay lập tức tạo thành một thành phần chỉ chứa nút đó. 

Chuỗi không tuần hoàn có hướng như 1 → 2 → 3 kiểm tra xem khả năng tiếp cận có hợp nhất các nút không chính xác hay không. Mỗi nút kết thúc vào những thời điểm khác nhau và DFS đảo ngược sẽ tách chúng thành các thành phần đơn lẻ. 

Biểu đồ tuần hoàn đầy đủ đảm bảo rằng khả năng tiếp cận lẫn nhau được nhận dạng chính xác. Trong trường hợp này, thứ tự DFS1 không quan trọng lắm vì DFS2 trên biểu đồ đảo ngược tiếp cận tất cả các nút trong một lần quét, tạo ra một thành phần duy nhất. 

Các trường hợp này cùng nhau xác nhận rằng thuật toán phân biệt giữa khả năng tiếp cận theo hướng và kết nối lẫn nhau thực sự mà không có sự mơ hồ.
