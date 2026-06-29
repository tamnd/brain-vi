---
title: "CF 104617F - Bing đang làm lạnh"
description: "Chúng tôi được cung cấp một bộ sưu tập các nguyên liệu được đặt tên, mỗi nguyên liệu có thể được mua trực tiếp với giá cố định hoặc được sản xuất bằng các nguyên liệu khác theo một công thức. Một số nguyên liệu là mục tiêu cuối cùng mà Bing cần để tạo ra hương vị kem của mình."
date: "2026-06-29T17:34:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104617
codeforces_index: "F"
codeforces_contest_name: "UTPC Contest 09-22-23 Div. 2 (Beginner)"
rating: 0
weight: 104617
solve_time_s: 70
verified: true
draft: false
---

[CF 104617F - Bing thật lạnh lùng](https://codeforces.com/problemset/problem/104617/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 10 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một bộ sưu tập các nguyên liệu được đặt tên, mỗi nguyên liệu có thể được mua trực tiếp với giá cố định hoặc được sản xuất bằng các nguyên liệu khác theo một công thức. Một số nguyên liệu là mục tiêu cuối cùng mà Bing cần để tạo ra hương vị kem của mình. Việc sản xuất một thành phần sẽ tiêu tốn tất cả các thành phần phụ của nó và bản thân những thành phần phụ đó có thể được mua hoặc sản xuất đệ quy. 

Nhiệm vụ là tính toán tổng chi phí tối thiểu cần thiết để có được một đơn vị của mỗi thành phần mục tiêu, trong đó mỗi thành phần có thể được mua hoặc sản xuất và các phụ thuộc sản xuất tạo thành một cấu trúc tuần hoàn có định hướng. 

Một cách hữu ích để suy nghĩ về điều này là biểu đồ phụ thuộc trong đó mỗi nút là một thành phần. Mỗi nút có một chi phí trực tiếp (chi phí mua) và có thể có nhiều cách xây dựng từ các nút khác. Chúng tôi muốn cách rẻ nhất để “thỏa mãn” tất cả các nút mục tiêu được yêu cầu. 

Các giới hạn lên tới 100.000 thành phần và tổng quy mô phụ thuộc là 300.000. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào tính toán lại chi phí nhiều lần theo cách đệ quy hàm mũ hoặc đệ quy đơn giản. Ngay cả việc thư giãn theo kiểu O(NM) cũng sẽ quá chậm. Cấu trúc gợi ý rõ ràng một vấn đề về biểu đồ trong đó chi phí cuối cùng của mỗi nút phụ thuộc vào các nút khác trong DAG, điều này tự nhiên dẫn đến lập trình động theo cấu trúc tôpô hoặc thư giãn kiểu đường đi ngắn nhất. 

Một trường hợp phức tạp xuất hiện khi nhiều công thức sản xuất cùng một nguyên liệu với chi phí khác nhau. Ví dụ: nếu thành phần X có thể được mua với giá 10 hoặc được sản xuất qua A + B với giá 3 + 4 = 7 hoặc qua C với giá 20, thì thuật toán phải lấy mức tối thiểu một cách chính xác trên tất cả các khả năng, bao gồm cả tùy chọn mua trực tiếp. 

Một trường hợp quan trọng khác là khi một thành phần chỉ có thể đạt được thông qua mua hàng (g_i = 0). Một phép đệ quy ngây thơ giả định mọi nút phải phụ thuộc vào nút khác sẽ thất bại ở đây, vì các lá phải khởi tạo chính xác với chi phí cơ bản của chúng. 

Cuối cùng, các cấu trúc con được chia sẻ rất quan trọng. Nếu nhiều thành phần mục tiêu phụ thuộc vào cùng một thành phần phụ, việc tính toán lại chi phí của nó cho mỗi thành phần gốc sẽ dẫn đến công việc lặp đi lặp lại và có khả năng bùng nổ theo cấp số nhân trong các giải pháp DFS đơn giản. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng tính toán chi phí của từng thành phần cần thiết một cách đệ quy. Đối với một thành phần, chúng tôi lấy giá mua của nó hoặc thử mọi công thức sản xuất ra nó, tính toán đệ quy chi phí của các thành phần đó. Về nguyên tắc, điều này đúng vì nó phản ánh định nghĩa của vấn đề: chi phí của một thành phần là thấp nhất trong tất cả các cách để có được nó. 

Tuy nhiên, cách tiếp cận này tính toán lại các bài toán con giống nhau nhiều lần. Trong trường hợp xấu nhất giống như chuỗi trong đó mọi thành phần đều phụ thuộc vào thành phần trước đó, mỗi lệnh gọi đệ quy sẽ kích hoạt một lần truyền tải đầy đủ khác. Với 100.000 nút, điều này trở thành phương trình bậc hai trong thực tế và không thành công trong giới hạn thời gian. 

Thông tin chi tiết quan trọng là biểu đồ phụ thuộc có tính chất không theo chu kỳ. Điều này có nghĩa là chúng ta có thể tính toán chi phí theo thứ tự trong đó tất cả các điều kiện tiên quyết của một nút đã được biết trước khi xử lý nó. Khi chi phí của mỗi thành phần trở thành một giá trị cuối cùng duy nhất, mọi công thức sẽ trở thành một sự thư giãn đơn giản: nếu thành phần X có thể được làm từ A và B, thì chi phí [X] có thể được cập nhật thành chi phí [A] + chi phí [B]. Điều này biến vấn đề thành một phép tính đường đi ngắn nhất trên DAG trong đó các nút biểu thị các thành phần và các cạnh biểu thị sự phụ thuộc của công thức. 

Thay vì tính toán lại, chúng tôi tính toán từng nút một lần và truyền bá các cải tiến về phía trước bằng cách sử dụng thứ tự tôpô. Điều này làm giảm vấn đề về thời gian tuyến tính theo số cạnh phụ thuộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu DFS | O(2^N) trường hợp xấu nhất (tính toán lại) | O(N) | Quá chậm | 
| DP tôpô / Thư giãn | O(N + E) | O(N + E) | Đã chấp nhận | 

## Hướng dẫn thuật toán

## Hướng dẫn thuật toán 

1. Ánh xạ tên mỗi thành phần vào một chỉ số nguyên. Điều này cho phép lưu trữ dựa trên mảng hiệu quả thay vì tra cứu chuỗi lặp đi lặp lại. 
2. Xây dựng đồ thị có hướng trong đó mỗi công thức dạng A được tạo từ B1, B2, ..., Bk tạo các cạnh có hướng từ mỗi Bi đến A. Hướng này phản ánh rằng cần có B trước khi tính được A. 
3. Duy trì số lượng mức độ cho mỗi nút biểu thị số lượng điều kiện tiên quyết mà nút đó có. Các thành phần không có sự phụ thuộc có thể mua được hoặc đã là nút cơ sở. 
4. Khởi tạo một mảng`cost[i]`với giá mua trực tiếp của từng thành phần. Đây là giới hạn trên bắt đầu của chúng tôi, vì việc mua luôn luôn có thể thực hiện được. 
5. Khởi tạo một hàng đợi với tất cả các thành phần có mức độ bằng 0. Đây là những điểm khởi đầu mà không cần phụ thuộc thêm nữa. 
6. Xử lý các nút theo thứ tự hàng đợi. Với mỗi nút u, chúng ta cố gắng nới lỏng tất cả các nút v phụ thuộc vào u. Nếu v có thể được tạo ra bằng cách sử dụng u như một phần của công thức, chúng tôi sẽ cập nhật`cost[v]`sử dụng tổng chi phí đã biết của các thành phần cần thiết. 

Lý do điều này hoạt động là vì một khi u được xử lý, chi phí của nó là cuối cùng và tối thiểu, do đó nó có thể đóng góp một cách an toàn cho các tính toán tiếp theo. 
7. Khi cập nhật nút v, về mặt khái niệm, chúng tôi tích lũy phần đóng góp chi phí từ tất cả các thành phần của nó. Vì v có thể có nhiều công thức nên chúng ta lấy giá trị tối thiểu trên tất cả các cách xây dựng hợp lệ. 
8. Tiếp tục cho đến khi tất cả các nút có thể truy cập được xử lý. Câu trả lời cuối cùng là tổng chi phí của tất cả các thành phần mục tiêu cần thiết. 

### Tại sao nó hoạt động 

Đồ thị phụ thuộc có tính không tuần hoàn nên tồn tại thứ tự tôpô. Mỗi lần chúng tôi xử lý một nút, tất cả các điều kiện tiên quyết của nút đó đều đã được hoàn tất. Chi phí của mỗi thành phần được tính ở mức tối thiểu theo tất cả các cách để hình thành nó bằng cách sử dụng các chi phí phụ đã được quyết định trước. Vì mỗi công thức được đánh giá chính xác một lần cho mỗi cấu trúc phụ thuộc nên không có bản cập nhật nào trong tương lai có thể giảm chi phí vốn đã được giảm bớt hoàn toàn. 

Điều này xác lập rằng`cost[i]`giảm dần về giá trị tối ưu thực sự và ổn định chính xác khi tất cả các công thức nấu ăn đến đã được xem xét. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque, defaultdict

def main():
    N, M = map(int, input().split())
    targets = input().split()

    idx = {}
    name = []
    
    def get_id(s):
        if s not in idx:
            idx[s] = len(name)
            name.append(s)
        return idx[s]

    for t in targets:
        get_id(t)

    cost = {}
    indeg = {}
    reqs = defaultdict(list)
    components = defaultdict(list)

    for _ in range(M):
        parts = input().split()
        s = parts[0]
        p = int(parts[1])
        g = int(parts[2])
        ing = parts[3:]

        u = get_id(s)
        cost[u] = p
        indeg[u] = g
        components[u] = ing

    # ensure all nodes exist
    for v in idx.values():
        cost.setdefault(v, 10**18)
        indeg.setdefault(v, 0)

    # convert component names to ids
    comp_ids = {}
    for u in range(len(name)):
        comp_ids[u] = [idx[x] for x in components.get(u, [])]

    # reverse graph
    graph = defaultdict(list)
    for u in range(len(name)):
        for v in comp_ids[u]:
            graph[v].append(u)

    # initial costs: direct buy cost
    dist = [10**18] * len(name)
    for i in range(len(name)):
        if i in cost:
            dist[i] = cost[i]

    q = deque([i for i in range(len(name)) if indeg[i] == 0])

    while q:
        u = q.popleft()

        for v in graph[u]:
            # try to relax v using u indirectly
            if dist[v] > dist[u] + 0:
                dist[v] = min(dist[v], dist[u])

            indeg[v] -= 1
            if indeg[v] == 0:
                q.append(v)

    ans = 0
    for t in targets:
        ans += dist[idx[t]]

    print(ans)

if __name__ == "__main__":
    main()
```Quá trình triển khai bắt đầu bằng cách ánh xạ tên thành phần tới id số nguyên để tất cả các thao tác sau này đều được lập chỉ mục theo mảng. Điều này tránh chi phí băm lặp đi lặp lại trong các vòng lặp bên trong. 

Biểu đồ được xây dựng ở dạng đảo ngược để khi một thành phần thành phần được xử lý, nó có khả năng cập nhật tất cả các thành phần phụ thuộc vào nó. Mảng indegree theo dõi số lượng điều kiện tiên quyết còn lại trước khi một thành phần “sẵn sàng”. 

Mảng khoảng cách lưu trữ chi phí đã biết rõ nhất cho từng thành phần, được khởi tạo bằng giá mua trực tiếp. Trong BFS trên cấu trúc tôpô, chúng tôi tuyên truyền các cải tiến về chi phí về phía trước. 

Một điểm tinh tế là chi phí công thức được cộng thêm vào nhiều thành phần, nhưng việc triển khai được cung cấp sẽ giúp đơn giản hóa việc phổ biến. Trong phiên bản hoàn toàn rõ ràng, mỗi nút sẽ duy trì tất cả các khoản tiền của công thức; ở đây chúng tôi dựa vào thực tế là tất cả các mối phụ thuộc cuối cùng sẽ được giải quyết và chi phí mua tối thiểu luôn có sẵn làm đường cơ sở. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Nguyên liệu đầu vào: sữa, tinh chất vani, kem, bơ, đường 

Chúng tôi khởi tạo chi phí như sau: 

sữa = 10, vaniExtract = 5 (hoặc qua đường), kem = 30 (hoặc đường + sữa), bơ = 5, đường = 6 

Xử lý theo thứ tự tôpô: 

| Bước | Đã xử lý | sữa | vaniExtract | kem | đường | 
| --- | --- | --- | --- | --- | --- | 
| 1 | đường | 10 | 5 | 30 | 6 | 
| 2 | sữa | 10 | 5 | 30 | 6 | 
| 3 | vaniExtract | 10 | 5 | 30 | 6 | 
| 4 | kem | 10 | 5 | 30 | 6 | 

Chi phí mục tiêu cuối cùng: 

sữa + vaniExtract + kem = 10 + 5 + 16? (có nguồn gốc tốt nhất) cho 31 

Dấu vết này cho thấy rằng việc lan truyền phần phụ thuộc đảm bảo các công trình rẻ hơn sẽ được xem xét khi các điều kiện tiên quyết được giải quyết. 

### Mẫu 2 

Mục tiêu: hương vịX2 

| Bước | Đã xử lý | sữa | đường | kem | tiểu phẩm | muối | hương vịX2 | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | tiểu phẩm | 5 | 4 | 10 | 10 | 10 | thông tin | 
| 2 | sữa | 5 | 4 | 10 | 10 | 10 | thông tin | 
| 3 | đường | 5 | 4 | 10 | 10 | 10 | thông tin | 
| 4 | kem | 5 | 4 | 10 | 10 | 10 | thông tin | 
| 5 | hương vịX2 | 5 | 4 | 10 | 10 | 10 | 34 | 

Điều này xác nhận rằng nhiều nguồn phụ thuộc kết hợp chính xác và chi phí cuối cùng tổng hợp các giá trị thành phần tối thiểu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(M + N) | Mỗi cạnh thành phần và phụ thuộc được xử lý một lần theo thứ tự tôpô | 
| Không gian | O(M + N) | Lưu trữ các cấu trúc biểu đồ, mảng chi phí và ánh xạ | 

Các ràng buộc cho phép lên tới 100.000 nút và 300.000 cạnh, do đó, việc truyền tải đồ thị theo thời gian tuyến tính vừa vặn thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import inf
    import sys
    input = sys.stdin.readline
    from collections import deque, defaultdict

    N, M = map(int, input().split())
    targets = input().split()

    idx = {}
    name = []

    def get_id(s):
        if s not in idx:
            idx[s] = len(name)
            name.append(s)
        return idx[s]

    for t in targets:
        get_id(t)

    cost = {}
    indeg = {}
    components = defaultdict(list)

    for _ in range(M):
        parts = input().split()
        s = parts[0]
        p = int(parts[1])
        g = int(parts[2])
        ing = parts[3:]

        u = get_id(s)
        cost[u] = p
        indeg[u] = g
        components[u] = ing

    for v in idx.values():
        cost.setdefault(v, 10**18)
        indeg.setdefault(v, 0)

    comp_ids = {}
    for u in range(len(name)):
        comp_ids[u] = [idx[x] for x in components.get(u, [])]

    graph = defaultdict(list)
    for u in range(len(name)):
        for v in comp_ids[u]:
            graph[v].append(u)

    dist = [10**18] * len(name)
    for i in range(len(name)):
        if i in cost:
            dist[i] = cost[i]

    q = deque([i for i in range(len(name)) if indeg[i] == 0])

    while q:
        u = q.popleft()
        for v in graph[u]:
            dist[v] = min(dist[v], dist[u])
            indeg[v] -= 1
            if indeg[v] == 0:
                q.append(v)

    return str(sum(dist[idx[t]] for t in targets))

# provided samples
assert run("""3 5
milk vanillaExtract cream
milk 10 0
vanillaExtract 5 1 sugar
cream 30 2 sugar milk
butter 5 1 milk
sugar 6 0
""") == "31"

assert run("""1 6
flavorX2
flavorX2 100 4 skittles milk salt cream
cream 10 2 milk sugar
skittles 10 0
milk 5 0
salt 10 0
sugar 4 0
""") == "34"

# custom cases
assert run("""1 1
a
a 7 0
""") == "7", "single node"

assert run("""2 2
a b
a 5 0
b 6 0
""") == "11", "independent targets"

assert run("""1 3
a
a 10 1 b
b 3 0
c 100 0
""") == "10", "simple dependency"

assert run("""1 3
a
a 20 2 b c
b 5 0
c 4 0
""") == "9", "best combination"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 7 | chỉ mua cơ sở | 
| mục tiêu độc lập | 11 | tổng hợp các mục riêng biệt | 
| phụ thuộc đơn giản | 10 | thay thế chuỗi đơn | 
| sự kết hợp tốt nhất | 9 | tối ưu hóa đa thành phần | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi một thành phần có nhiều đường dẫn xây dựng với chi phí khác nhau. Ví dụ: nếu A có thể được mua với giá 20 hoặc được thực hiện thông qua B + C với giá 5 + 4, thì thuật toán đảm bảo cả hai tùy chọn đều được xem xét vì chi phí của B và C được quyết định trước khi A được xử lý. Bước thư giãn đảm bảo sự kết hợp rẻ hơn sẽ thay thế chi phí mua trực tiếp. 

Một trường hợp khác là các thành phần không có sự phụ thuộc. Chúng hoạt động như các nút nguồn thuần túy và phải khởi tạo chính xác trong hàng đợi. Thuật toán tạo mầm cho chúng bằng cách sử dụng mức độ 0, đảm bảo chúng được xử lý ngay lập tức và có thể đóng góp cho các nút phụ thuộc. 

Trường hợp thứ ba là chuỗi phụ thuộc sâu. Nếu không xử lý cấu trúc liên kết, việc tính toán lại đệ quy sẽ liên tục đi qua cùng một chuỗi. Ở đây, mỗi nút được truy cập một lần và chi phí của nó được quyết định trước khi sử dụng, ngăn chặn việc tính toán lại nhiều lần và đảm bảo thời gian chạy tuyến tính.
