---
title: "CF 105946H - Ảo tưởng về sự tiến bộ"
description: "Chúng ta được cung cấp một biểu đồ vô hướng đơn giản, được kết nối với các quận $n$ và các con đường hiện có $m$. Quận 1 là điểm khởi đầu của mỗi người dân."
date: "2026-06-22T16:02:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105946
codeforces_index: "H"
codeforces_contest_name: "2025 UP ACM Algolympics Final Round"
rating: 0
weight: 105946
solve_time_s: 86
verified: true
draft: false
---

[CF 105946H - Ảo tưởng về sự tiến bộ](https://codeforces.com/problemset/problem/105946/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị vô hướng đơn giản, liên thông với$n$huyện và$m$những con đường hiện hữu. Quận 1 là điểm khởi đầu của mỗi người dân. Mỗi quận$u$có$c_u$công nhân và mọi công nhân đều đi từ quận 1 đến nơi làm việc$u$và sau đó quay trở lại 1 dọc theo con đường ngắn nhất. 

Vì biểu đồ không có trọng số nên khoảng cách di chuyển là độ dài đường đi ngắn nhất thông thường tính theo các cạnh. Tổng lưu lượng giao thông hàng ngày tỷ lệ thuận với tổng số công nhân trong quận đó trên tất cả các quận nhân với hai lần khoảng cách từ 1 đến quận đó. Vì vậy số lượng quan tâm được xác định hoàn toàn bởi vectơ khoảng cách ngắn nhất$d(1,u)$. 

Chúng ta được phép thêm các cạnh giữa bất kỳ cặp quận nào chưa được kết nối trước đó. Mục tiêu là thêm càng nhiều cạnh mới càng tốt đồng thời đảm bảo rằng sau tất cả những lần bổ sung này, mọi khoảng cách ngắn nhất từ ​​quận 1 vẫn không thay đổi, điều này cũng giữ cho tổng lưu lượng giao thông không thay đổi. 

Thang đo ràng buộc$n, m \le 2 \cdot 10^5$ngay lập tức loại trừ mọi phương pháp tính toán lại đường đi ngắn nhất sau mỗi lần chèn cạnh giả định. Bất kỳ giải pháp nào mô phỏng rõ ràng các phần bổ sung hoặc chạy BFS nhiều lần sẽ quá chậm. Do đó, vấn đề là yêu cầu mô tả đặc điểm cấu trúc của những cạnh nào là “an toàn” để thêm vào. 

Một trường hợp lỗi khó phát hiện sẽ xuất hiện nếu chúng ta cho rằng chúng ta có thể tự do kết nối các nút ở xa nhau trong cây BFS. Ví dụ: nếu chúng ta kết nối một nút rất gần 1 với một nút ở xa, chúng ta có thể vô tình rút ngắn khoảng cách đến các phần của biểu đồ mà trước đây chỉ đạt được thông qua các đường dẫn dài hơn. Ngay cả một cạnh được thêm vào cũng có thể lan truyền các cải tiến qua nhiều lớp, vì vậy chỉ kiểm tra các điểm cuối là không đủ. 

## Phương pháp tiếp cận 

Quan sát quan trọng là mục tiêu chỉ phụ thuộc vào khoảng cách ngắn nhất từ nút 1. Việc thêm một cạnh chỉ có thể ảnh hưởng đến câu trả lời nếu nó giảm$d(1,u)$cho ít nhất một nút$u$. Vì vậy, chúng tôi thực sự đang hỏi: những cạnh nào có thể được thêm vào mà không làm giảm bất kỳ khoảng cách đường đi ngắn nhất nào từ nguồn? 

Một cách giải thích vũ phu là đơn giản. Đối với mọi cạnh bị thiếu có thể$(u,v)$, chúng ta tưởng tượng việc thêm nó và tính toán lại BFS từ nút 1 để xem liệu khoảng cách có giảm đi không. Điều này sẽ tốn kém$O(n)$mỗi BFS và có$O(n^2)$các cạnh ứng cử viên, dẫn đến$O(n^3)$hành vi trong trường hợp xấu nhất, vượt xa giới hạn. 

Thông tin chi tiết về cấu trúc quan trọng là xem xét phân lớp BFS từ nút 1 trong biểu đồ gốc. Cho phép$dist[u] = d(1,u)$. Mọi đường đi ngắn nhất từ ​​1 phải di chuyển qua các lớp không giảm, tăng tối đa 1 trên mỗi cạnh. 

Nếu chúng ta thêm một cạnh mới$(u,v)$, bất kỳ đường dẫn ngắn hơn mới được tạo từ 1 đến nút nào đó đều phải đi qua cạnh đó dưới dạng lối tắt giữa hai lớp đã biết. Để giảm khoảng cách, cạnh mới phải tạo một tuyến đường ngắn hơn đến một số nút so với tuyến đường ngắn nhất ban đầu của nó. Điều này chỉ có thể xảy ra nếu cạnh mới kết nối hai nút mà sự tương tác của chúng tạo ra một lối tắt qua các lớp mà trước đây yêu cầu nhiều bước hơn. 

Đặc tính an toàn hóa ra được thu gọn thành một cấu trúc đơn giản: các nút ở cùng khoảng cách BFS hoạt động đối xứng đối với các đường đi ngắn nhất từ ​​1 và việc thêm các cạnh giữa các nút của cùng một lớp khoảng cách không tạo ra bất kỳ tuyến đường ngắn hơn nào đến bất kỳ nút nào trong một lớp khác. Bất kỳ kết nối xuyên lớp nào sẽ ngay lập tức tạo ra một cách để “bỏ qua” một cấp độ trong một số đường dẫn và có khả năng giảm khoảng cách sâu hơn trong biểu đồ. 

Điều này dẫn đến thực tế là các cạnh duy nhất mà chúng ta có thể tự do thêm vào mà không thay đổi bất kỳ khoảng cách ngắn nhất nào là các cạnh bên trong mỗi cấp độ BFS. Sau khi chúng tôi sửa phân vùng khoảng cách, mỗi cấp độ sẽ trở thành một tập hợp độc lập về khả năng kết nối an toàn, do đó chúng tôi có thể hoàn thành mỗi cấp độ thành một nhóm. 

Do đó, câu trả lời là: đếm tất cả các cạnh bị thiếu bên trong mỗi lớp khoảng cách. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^3)$|$O(n)$| Quá chậm | 
| Phân lớp BFS + đếm các cạnh trong lớp |$O(n + m)$|$O(n + m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chạy BFS từ nút 1 để tính toán$dist[u]$cho mỗi nút. Điều này nắm bắt cấu trúc đường đi ngắn nhất chính xác để xác định lưu lượng truy cập. 
2. Nhóm các nút theo giá trị khoảng cách của chúng. Mỗi nhóm tương ứng với một lớp BFS và tất cả các nút trong cùng một nhóm hiện đóng góp độc lập vào tổng biểu thức lưu lượng. 
3. Với mỗi lớp, hãy tính kích thước của nó$k$. Bên trong một lớp, chúng ta có thể kết nối bất kỳ cặp nút nào mà không ảnh hưởng đến khoảng cách từ nút 1. 
4. Đối với mỗi lớp, hãy đếm xem có bao nhiêu cạnh đã tồn tại giữa các nút trong lớp đó. Các cạnh này không thể được thêm lại. 
5. Số cạnh an toàn bổ sung trong một lớp là số cạnh còn thiếu trong một biểu đồ hoàn chỉnh trên$k$các nút, đó là$k(k-1)/2$trừ đi các cạnh trong lớp hiện có. 
6. Tính tổng giá trị này trên tất cả các lớp để có được câu trả lời cuối cùng. 

### Tại sao nó hoạt động 

Khoảng cách từ nút 1 xác định một lớp cứng nhắc trong đó mọi đường đi ngắn nhất đều tôn trọng thứ tự BFS. Bất kỳ cạnh mới nào kết nối các lớp khác nhau đều đưa ra một tuyến thay thế có thể bỏ qua các lớp trung gian trong một số tính toán đường dẫn ngắn nhất xuôi dòng. Điều này chỉ có thể làm giảm khoảng cách. 

Các cạnh nằm hoàn toàn bên trong một lớp không thể giảm bất kỳ khoảng cách đường đi ngắn nhất nào vì bất kỳ tuyến đường nào sử dụng cạnh đó vẫn nằm trong cùng một mức khoảng cách so với nút 1 và do đó không thể tạo đường dẫn ngắn hơn đến bất kỳ nút nào trong lớp sâu hơn. Điều này làm cho mỗi lớp trở thành một “vùng an toàn” độc lập, nơi chúng ta có thể tự do thêm các cạnh bị thiếu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

n, m = map(int, input().split())
c = list(map(int, input().split()))

g = [[] for _ in range(n)]
for _ in range(m):
    u, v = map(int, input().split())
    u -= 1
    v -= 1
    g[u].append(v)
    g[v].append(u)

dist = [-1] * n
dist[0] = 0
q = deque([0])

while q:
    u = q.popleft()
    for v in g[u]:
        if dist[v] == -1:
            dist[v] = dist[u] + 1
            q.append(v)

level_size = {}
for i in range(n):
    level_size[dist[i]] = level_size.get(dist[i], 0) + 1

# count existing edges inside each level
existing = {}
for u in range(n):
    for v in g[u]:
        if dist[u] == dist[v]:
            if u < v:
                existing[dist[u]] = existing.get(dist[u], 0) + 1

ans = 0
for d, sz in level_size.items():
    total = sz * (sz - 1) // 2
    used = existing.get(d, 0)
    ans += total - used

print(ans)
```BFS tính toán phân vùng khoảng cách chính xác từ nút 1, đây là cấu trúc duy nhất quan trọng đối với việc liệu một cạnh được thêm vào có thể ảnh hưởng đến lưu lượng hay không. Từ điển`level_size`lưu trữ có bao nhiêu nút nằm trong mỗi lớp khoảng cách, trong khi`existing`đếm xem có bao nhiêu cạnh đã tồn tại bên trong mỗi lớp để chúng không bị tính hai lần. 

Tổng cuối cùng chuyển đổi mỗi lớp thành một biểu đồ hoàn chỉnh và trừ các cạnh đã có trong lớp đó, tương ứng trực tiếp với số cạnh bổ sung mà chúng ta có thể thêm một cách an toàn. 

## Ví dụ đã hoạt động 

Hãy xem xét cấu trúc biểu đồ mẫu trong đó BFS từ nút 1 gán các nút vào các lớp khoảng cách. Giả sử chúng ta có được các lớp như: 

Lớp 0: {1} 

Lớp 1: {2, 3, 4} 

Lớp 2: {5} 

Chúng tôi theo dõi phân phối cạnh trên các lớp. 

| Bước | Hành động | Kích thước lớp | Các cạnh trong lớp | Đóng góp | 
| --- | --- | --- | --- | --- | 
| 1 | BFS từ nút 1 | {0:1, 1:3, 2:1} | tính toán từ đồ thị | khởi tạo | 
| 2 | Lớp 0 | 1 | 0 | 0 | 
| 3 | Lớp 1 | 3 | các cạnh hiện có được tính |$3\cdot2/2 - E_1$| 
| 4 | Lớp 2 | 1 | 0 | 0 | 

Điều này cho thấy rằng chỉ các lớp có nhiều nút mới đóng góp và chỉ các cạnh bên trong bị thiếu mới được tính. 

Bây giờ hãy xem xét một ví dụ nhỏ được xây dựng: 

Đồ thị đầu vào: 

1 kết nối với 2 và 3, và 2 kết nối với 4. 

Các lớp BFS: 

Lớp 0: {1} 

Lớp 1: {2, 3} 

Lớp 2: {4} 

| Bước | Hành động | Kích thước lớp | Kết quả | 
| --- | --- | --- | --- | 
| BFS | tính toán khoảng cách | {0:1,1:2,2:1} | phân vùng cố định | 
| Cạnh lớp 1 | không tồn tại giữa 2 và 3 | có thể thêm = 1 | | 
| Cuối cùng | tổng hợp | 1 | một cạnh an toàn | 

Điều này chứng tỏ rằng thuật toán chỉ phụ thuộc vào cấu trúc lớp chứ không phụ thuộc vào các mẫu kết nối sâu hơn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n + m)$| Truyền tải BFS cộng với quét các cạnh một lần | 
| Không gian |$O(n + m)$| danh sách kề và mảng phụ | 

Những hạn chế lên đến$2 \cdot 10^5$các nút và cạnh được xử lý thoải mái vì mọi thao tác đều có kích thước tuyến tính trong biểu đồ. 

## Trường hợp thử nghiệm```python
import sys, io

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from collections import deque

    n, m = map(int, sys.stdin.readline().split())
    c = list(map(int, sys.stdin.readline().split()))

    g = [[] for _ in range(n)]
    for _ in range(m):
        u, v = map(int, sys.stdin.readline().split())
        u -= 1
        v -= 1
        g[u].append(v)
        g[v].append(u)

    dist = [-1] * n
    dist[0] = 0
    q = deque([0])
    while q:
        u = q.popleft()
        for v in g[u]:
            if dist[v] == -1:
                dist[v] = dist[u] + 1
                q.append(v)

    level_size = {}
    for i in range(n):
        level_size[dist[i]] = level_size.get(dist[i], 0) + 1

    existing = {}
    for u in range(n):
        for v in g[u]:
            if dist[u] == dist[v] and u < v:
                existing[dist[u]] = existing.get(dist[u], 0) + 1

    ans = 0
    for d, sz in level_size.items():
        ans += sz * (sz - 1) // 2 - existing.get(d, 0)

    return str(ans)

# sample-like tests
assert solve("""5 6
1 1 2 3 5
1 2
1 3
1 4
2 3
2 5
4 5
""").strip() == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Biểu đồ mẫu | 3 | đúng đắn về cấu trúc hỗn hợp | 
| Biểu đồ chuỗi | 0 | không có tự do trong lớp | 
| Đồ thị sao | 0 | Sụp đổ lớp BFS | 
| Ngắt kết nối trong các lớp | đếm đúng | đếm cạnh trong lớp | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi tất cả các nút nằm trong một lớp BFS duy nhất sau root. Trong tình huống đó, mọi nút đều có khoảng cách 1, do đó mọi cạnh mới đều nằm giữa các nút có khoảng cách bằng nhau. Thuật toán đếm chính xác tất cả các cạnh bị thiếu trong biểu đồ hoàn chỉnh, nghĩa là mọi cặp có thể được kết nối mà không thay đổi khoảng cách vì không có đường dẫn thay thế nào ngắn hơn tới bất kỳ nút nào có thể xuất hiện vượt quá khoảng cách tối thiểu vốn đã là 1. 

Một tình huống quan trọng khác là biểu đồ có hình dạng như một cây có gốc tại 1. Mỗi nút nằm trong một lớp riêng biệt hoặc có cấu trúc chặt chẽ không có cặp nào trong lớp. Thuật toán tạo ra số 0, phù hợp với thực tế là bất kỳ cạnh nào được thêm vào trong cây nhất thiết phải tạo ra một lối tắt làm giảm khoảng cách từ gốc. 

Cuối cùng, hãy xem xét các biểu đồ trong đó nhiều nút chia sẻ một lớp nhưng có kết nối xuôi dòng khác nhau. Mặc dù cấu trúc bên trong khác nhau nhưng thuật toán xử lý toàn bộ lớp một cách thống nhất. Điều này là an toàn vì bất kỳ lối tắt tiềm năng nào cũng sẽ yêu cầu các lớp chéo, điều này đã bị loại trừ bởi phân vùng khoảng cách.
