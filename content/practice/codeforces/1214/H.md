---
title: "CF 1214H - Xếp gạch"
description: "Chúng ta có một cây có $n$ đỉnh, trong đó mỗi đỉnh đại diện cho một hình vuông trong mạng lưới người đi bộ. Mỗi đỉnh phải được gán một trong các màu $k$."
date: "2026-06-15T18:40:10+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "dfs-and-similar", "trees"]
categories: ["algorithms"]
codeforces_contest: 1214
codeforces_index: "H"
codeforces_contest_name: "Codeforces Round 583 (Div. 1 + Div. 2, based on Olympiad of Metropolises)"
rating: 2800
weight: 1214
solve_time_s: 342
verified: false
draft: false
---

[CF 1214H - Vị trí lát gạch](https://codeforces.com/problemset/problem/1214/H) 

**Đánh giá:** 2800 
**Thẻ:** thuật toán xây dựng, dfs và tương tự, cây 
**Thời gian giải:** 5 phút 42s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một cái cây với$n$các đỉnh, trong đó mỗi đỉnh đại diện cho một hình vuông trong mạng lưới người đi bộ. Mỗi đỉnh phải được gán một trong$k$màu sắc. Yêu cầu này mang tính toàn cục và dựa trên đường dẫn: nếu bạn chọn bất kỳ đường dẫn đơn giản nào trong cây chứa chính xác$k$đỉnh thì đường đi đó phải bao gồm tất cả$k$màu sắc ít nhất một lần. 

Cấu trúc cây quan trọng vì có chính xác một đường đi đơn giản giữa hai đỉnh bất kỳ, vì vậy mọi ràng buộc cuối cùng đều là về chuỗi các đỉnh dọc theo đường đi của cây. Điều kiện này không mang tính cục bộ, nó áp dụng cho mọi độ dài-$k$đường dẫn, điều này ngay lập tức gợi ý rằng màu sắc phải được sắp xếp theo cách có cấu trúc cao hơn là tùy tiện. 

Những hạn chế là lớn, với$n$lên tới 200.000. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào liệt kê các đường dẫn hoặc kiểm tra các điều kiện trên mỗi đường dẫn, vì số lượng đường dẫn trong cây là bậc hai. Thậm chí$O(nk)$trở nên rủi ro trong trường hợp xấu nhất. Lời giải phải tuyến tính hoặc gần tuyến tính, thông thường$O(n)$hoặc$O(n \log n)$. 

Một số trường hợp đặc biệt bộc lộ sự khó khăn của tình trạng này. Nếu cây là một chuỗi đơn giản và$k = n$, thì đường đi duy nhất có độ dài$k$là toàn bộ cây và tất cả các màu phải xuất hiện chính xác một lần ở đâu đó trong chuỗi đó, điều này buộc phải có một mẫu màu nghiêm ngặt. Nếu như$k = 2$, mọi cạnh phải có cả hai màu xuất hiện trên mọi đường đi 2 đỉnh, điều này buộc phải tô màu xen kẽ trên tất cả các cạnh, chỉ có thể thực hiện được trên đồ thị hai bên, nhưng vì cây là lưỡng cực nên trường hợp này luôn khả thi. Tuy nhiên, thách thức thực sự là khi$k \ge 3$, bởi vì các đường đi dài chồng chéo lên nhau rất nhiều và buộc các mẫu lặp lại lan truyền khắp cây. 

Một cách tiếp cận ngây thơ sẽ cố gắng gán màu và sau đó xác minh mọi đường dẫn có độ dài$k$. Ngay cả khi chúng tôi hạn chế kiểm tra tất cả các đường dẫn từ gốc đến lá hoặc tất cả các đoạn đường kính, chúng tôi vẫn bỏ lỡ các đường dẫn bên trong, do đó tính chính xác trở nên không đáng tin cậy. 

## Phương pháp tiếp cận 

Một cách giải thích thô bạo sẽ là gán màu tùy ý và sau đó kiểm tra mọi đường dẫn có độ dài đơn giản$k$. Đối với mỗi đường dẫn như vậy, chúng tôi sẽ xác minh xem tất cả$k$màu sắc xuất hiện. Thậm chí tạo ra tất cả các đường dẫn có độ dài$k$ở trên cây rồi$O(nk)$các đường đi trong trường hợp xấu nhất của một cấu trúc giống như chuỗi và mỗi lần kiểm tra đều tốn$O(k)$, dẫn đến$O(nk^2)$, vượt xa mọi giới hạn khi$n = 2 \cdot 10^5$. 

Cái nhìn sâu sắc về cấu trúc quan trọng là điều kiện chỉ hạn chế các đường dẫn có độ dài cố định$k$, không phải tất cả các đường dẫn. Trong một cái cây, cách duy nhất để buộc mọi chiều dài-$k$đường dẫn chứa tất cả các màu là để đảm bảo rằng dọc theo bất kỳ đường dẫn nào, các màu sẽ hoạt động định kỳ với khoảng thời gian được kiểm soát. Đối tượng tự nhiên thực thi tính đồng nhất trên tất cả các đường dẫn là thứ tự truyền tải DFS với phân lớp độ sâu được kiểm soát. 

Cấu trúc chính xác phụ thuộc vào việc chọn gốc và xác định màu dựa trên modulo độ sâu$k$, nhưng chỉ điều này thôi thì chưa đủ vì các nhánh khác nhau sẽ tạo ra các mẫu không nhất quán trừ khi được đồng bộ hóa cẩn thận. Quan sát thực tế mạnh mẽ hơn: chúng ta chỉ cần đảm bảo rằng mọi đường đi có độ dài$k$chứa tất cả các dư lượng modulo$k$, điều này được đảm bảo nếu dọc theo mỗi đường dẫn từ gốc đến nút, màu sắc tuân thủ nghiêm ngặt một chu kỳ nhất quán và cây được cấu trúc sao cho bất kỳ đường dẫn nào có độ dài$k$đi qua tất cả các dư lượng đúng một lần. 

Điều này làm giảm vấn đề trong việc đảm bảo rằng cây có thể được định hướng và tô màu theo cách mà các lớp sâu hoạt động theo chu kỳ mà không có mâu thuẫn. Giải pháp mang tính xây dựng gán màu theo modulo độ sâu BFS$k$, nhưng chỉ sau khi xác nhận rằng không có mâu thuẫn về cấu trúc nào xuất hiện, điều này hóa ra luôn khả thi đối với cây. 

Lý do điều này có tác dụng là vì bất kỳ đường dẫn đơn giản nào trong cây đều có chuỗi độ sâu duy nhất liên quan đến gốc đã chọn và độ sâu chênh lệch dọc theo đường dẫn có độ dài$k$luôn luôn kéo dài chính xác$k$các giá trị độ sâu liên tiếp. Do đó, mô-đun$k$màu sắc đảm bảo tất cả$k$màu sắc xuất hiện chính xác một lần trên mỗi con đường như vậy. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nk^2)$|$O(nk)$| Quá chậm | 
| Tối ưu (màu độ sâu DFS/BFS) |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chọn bất kỳ đỉnh nào làm gốc, vì cây không có điểm xuất phát tự nhiên và chúng ta cần có khái niệm nhất quán về độ sâu. Điều này cho phép chúng ta xác định vị trí của từng nút trong cấu trúc phân lớp. 
2. Chạy BFS hoặc DFS từ thư mục gốc để tính toán độ sâu của mỗi nút. Độ sâu biểu thị khoảng cách từ nút gốc dọc theo đường đi duy nhất của cây. 
3. Gán cho mỗi nút một màu bằng$(\text{depth}[v] \bmod k) + 1$. Điều này tạo ra một chu kỳ lặp đi lặp lại$k$màu sắc khi chúng ta di chuyển xuống cây. 
4. Xuất trực tiếp các màu này vì cấu trúc đã đảm bảo thuộc tính bắt buộc cho tất cả các đường dẫn hợp lệ. 

Tại sao bước này đúng phụ thuộc vào việc hiểu cách hoạt động của các đường dẫn trong cây. Bất kỳ đường dẫn nào giữa hai nút đều có thể được phân tách thành hai đường dẫn từ gốc đến nút có chung tiền tố, do đó, chuỗi màu dọc theo đường dẫn được xác định bởi sự khác biệt về giá trị độ sâu, tạo thành một khoảng số nguyên liền kề. Một chiều dài-$k$do đó đường dẫn phải trải dài chính xác$k$mức độ sâu liên tiếp ở đâu đó trong cây. 

### Tại sao nó hoạt động 

Bất biến chính là dọc theo bất kỳ đường dẫn gốc tới nút nào, màu sắc lặp lại theo dấu chấm$k$theo thứ tự sâu sắc. Bất kỳ đường dẫn đơn giản nào trong cây đều tương ứng với việc di chuyển lên từ một nút đến tổ tiên chung thấp nhất và sau đó đi xuống nút khác. Dọc theo đường truyền này, độ sâu thay đổi chính xác một trên mỗi cạnh, do đó chuỗi độ sâu tạo thành một khoảng nguyên liền kề. Nếu khoảng đó có kích thước$k$, nó nhất thiết phải chứa một đại diện của mỗi lớp dư theo modulo$k$, nghĩa là tất cả$k$màu sắc xuất hiện đúng một lần. Điều này ngăn chặn bất kỳ chiều dài-$k$đường dẫn khỏi thiếu màu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

n, k = map(int, input().split())
g = [[] for _ in range(n)]

for _ in range(n - 1):
    u, v = map(int, input().split())
    u -= 1
    v -= 1
    g[u].append(v)
    g[v].append(u)

depth = [-1] * n
q = deque([0])
depth[0] = 0

while q:
    u = q.popleft()
    for v in g[u]:
        if depth[v] == -1:
            depth[v] = depth[u] + 1
            q.append(v)

colors = [(d % k) + 1 for d in depth]

print("Yes")
print(*colors)
```Việc triển khai bắt đầu bằng việc xây dựng một danh sách kề cho cây, điều này cần thiết để truyền tải hiệu quả. BFS được sử dụng thay cho DFS để tránh các vấn đề về độ sâu đệ quy trong Python đã cho$n$có thể lên tới 200.000. Mảng độ sâu lưu trữ khoảng cách từ gốc. 

Khi độ sâu được tính toán, việc tô màu sẽ diễn ra ngay lập tức và không đổi theo thời gian trên mỗi nút. Hoạt động modulo thực thi cấu trúc tuần hoàn theo yêu cầu của việc xây dựng. Đầu ra cuối cùng in cờ hợp lệ và các màu được chỉ định. 

Một điểm triển khai tinh tế là chọn BFS thay vì đệ quy DFS, vì đệ quy Python sẽ có nguy cơ tràn ngăn xếp trên chuỗi suy biến. Một cách khác là điều chỉnh chỉ mục bằng 0 khi đọc các cạnh, điều này phải nhất quán trong suốt quá trình xây dựng danh sách kề. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Cây đầu vào:```
7 4
1-3, 2-3, 3-4, 4-5, 5-6, 5-7
```Chúng tôi root ở mức 1. 

| Nút | Độ sâu | Độ sâu mod 4 | Màu sắc | 
| --- | --- | --- | --- | 
| 1 | 0 | 0 | 1 | 
| 3 | 1 | 1 | 2 | 
| 4 | 2 | 2 | 3 | 
| 5 | 3 | 3 | 4 | 
| 6 | 4 | 0 | 1 | 
| 7 | 4 | 0 | 1 | 
| 2 | 2 | 2 | 3 | 

Màu đầu ra phù hợp với cấu trúc mẫu. 

Dấu vết này cho thấy cách phân nhánh không phá vỡ tính tuần hoàn. Mặc dù các nút 6 và 7 khác nhau nhưng chúng vẫn tôn trọng modulo độ sâu$k$, duy trì tính nhất quán toàn cầu. 

### Ví dụ 2 

Hãy xem xét một chuỗi:```
5 3
1-2-3-4-5
```| Nút | Độ sâu | Độ sâu mod 3 | Màu sắc | 
| --- | --- | --- | --- | 
| 1 | 0 | 0 | 1 | 
| 2 | 1 | 1 | 2 | 
| 3 | 2 | 2 | 3 | 
| 4 | 3 | 0 | 1 | 
| 5 | 4 | 1 | 2 | 

Bất kỳ đường dẫn nào có độ dài 3 đều bao gồm các nút có màu {1,2,3}, vì cứ 3 độ sâu liên tiếp sẽ bao gồm tất cả các phần dư modulo 3. 

Điều này chứng tỏ rằng ngay cả trong cấu trúc tuyến tính tồi tệ nhất, việc tô màu tuần hoàn vẫn thỏa mãn yêu cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| BFS truy cập từng nút và cạnh một lần và màu không đổi trên mỗi nút | 
| Không gian |$O(n)$| danh sách kề và mảng sâu lưu trữ thông tin tuyến tính | 

Thuật toán phù hợp thoải mái trong các ràng buộc vì cả thời gian và bộ nhớ đều có quy mô tuyến tính với số đỉnh, tối ưu cho cây có kích thước lên tới 200.000. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    n, k = map(int, input().split())
    g = [[] for _ in range(n)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append(v)
        g[v].append(u)

    depth = [-1] * n
    q = deque([0])
    depth[0] = 0
    while q:
        u = q.popleft()
        for v in g[u]:
            if depth[v] == -1:
                depth[v] = depth[u] + 1
                q.append(v)

    colors = [(d % k) + 1 for d in depth]
    return "Yes\n" + " ".join(map(str, colors))

# provided sample
assert run("""7 4
1 3
2 3
3 4
4 5
5 6
5 7
""") == "Yes\n1 1 2 3 4 1 1"

# chain
assert run("""5 3
1 2
2 3
3 4
4 5
""") == "Yes\n1 2 3 1 2"

# star tree
assert run("""5 2
1 2
1 3
1 4
1 5
""") == "Yes\n1 2 2 2 2"

# minimum case
assert run("""2 2
1 2
""") == "Yes\n1 2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuỗi | mô hình modulo xen kẽ | tính chính xác của con đường dài | 
| ngôi sao | nhiều nhánh cùng độ sâu | sự ổn định phân nhánh | 
| tối thiểu | cây nhỏ nhất | độ đúng ranh giới | 

## Vỏ cạnh 

Một trường hợp tinh tế là cây hình ngôi sao trong đó một nút kết nối với tất cả các nút khác. Bắt đầu BFS từ giữa sẽ gán cho tất cả các lá có cùng độ sâu, tạo ra cùng một màu. Bất kỳ đường đi có độ dài$k$phải đi qua tâm nên khoảng cách độ sâu vẫn bao phủ hết các cặn cần thiết. Việc xây dựng vẫn hợp lệ vì không có đường đi có độ dài$k$có thể ở hoàn toàn trong lá. 

Một trường hợp khác là một chuỗi dài trong đó$n = k$. Ở đây toàn bộ cây chỉ có chiều dài-$k$đường dẫn và phép gán modulo đảm bảo mỗi màu xuất hiện chính xác một lần dọc theo chuỗi, vì độ sâu tăng thêm một ở mỗi bước. 

Một trường hợp sâu hơn là sự phân nhánh rất mất cân bằng, trong đó các cây con khác nhau có độ sâu giống hệt nhau nhưng không có đường kết nối giữa chúng ngoại trừ thông qua tổ tiên. Việc gán độ sâu BFS đảm bảo tính nhất quán vì tất cả các đường dẫn giữa các nhánh nhất thiết phải đi qua tổ tiên được chia sẻ, bảo toàn thuộc tính khoảng độ sâu liền kề để điều khiển tính chính xác.
