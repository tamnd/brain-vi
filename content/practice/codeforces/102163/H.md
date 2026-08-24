---
title: "CF 102163H - Ông Hamra và các hạt lượng tử của ông"
description: "Hãy coi các hạt là các đỉnh của đồ thị vô hướng. Mọi quan hệ vướng víu đã biết đều cho một cạnh giữa hai đỉnh."
date: "2026-08-23T22:21:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102163
codeforces_index: "H"
codeforces_contest_name: "NCD 2019"
rating: 0
weight: 102163
solve_time_s: 832
verified: true
draft: false
---

[CF 102163H - Ông Hamra và các hạt lượng tử của ông](https://codeforces.com/problemset/problem/102163/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 13m 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Hãy coi các hạt là các đỉnh của đồ thị vô hướng. Mọi quan hệ vướng víu đã biết đều cho một cạnh giữa hai đỉnh. Tính chất bắc cầu trong phát biểu có nghĩa là nếu có một đường đi từ hạt (x) đến hạt (y), thì hai hạt được coi là vướng víu, ngay cả khi không có mối quan hệ trực tiếp nào được đưa ra giữa chúng. 

Do đó, đối với mọi truy vấn ((x,y)), chúng ta cần xác định xem (x) và (y) có thuộc cùng một thành phần liên thông của đồ thị hay không. Đầu ra là một chuỗi nhị phân theo thứ tự truy vấn. Ký tự thứ (i) là`1`khi hai hạt được truy vấn được kết nối thông qua các mối quan hệ đã cho và`0`nếu không thì. 

Các giá trị của (N), (M) và (Q) đều có thể đạt tới (10^5). Với tỷ lệ đó và giới hạn 2 giây, một thuật toán thực hiện duyệt đồ thị cho mọi truy vấn là quá đắt. Một lần truyền tải đơn lẻ có thể kiểm tra (O(N+M)) đỉnh và cạnh, do đó, việc lặp lại (10^5) lần có thể đạt được khoảng (10^{10}) phép tính đồ thị. Công việc bậc hai như kiểm tra từng cặp hạt thậm chí còn ít khả thi hơn. Chúng ta cần xử lý biểu đồ một lần và làm cho mỗi truy vấn gần với thời gian không đổi. 

Có một số trường hợp thực hiện không cẩn thận có thể dẫn đến kết quả sai. Mối quan hệ trùng lặp không được thay đổi kết nối. Ví dụ,```
1
3 2 3
1 2
1 2
1 2
2 3
3 1
```chỉ có một kết nối thực tế giữa hạt 1 và hạt 2, nhưng hạt 3 bị cô lập. Đầu ra đúng là`110`vì 1 và 2 được kết nối, 1 và 2 được kết nối lại, còn 1 và 3 thì không. Việc triển khai xử lý số cạnh đầu vào như thể mọi cạnh được kết nối với một thành phần mới có thể đưa ra các giả định không chính xác về cấu trúc biểu đồ. 

Một truy vấn cũng có thể chứa cùng một phần tử hai lần. Ví dụ,```
1
1 1 2
1 1
1 1
1 1
```có đầu ra`11`. Một hạt luôn được kết nối với chính nó vì một đỉnh thuộc cùng một thành phần được kết nối với chính nó. Việc triển khai chỉ kiểm tra xem có cạnh rõ ràng giữa hai đỉnh được truy vấn hay không sẽ không thành công đối với truy vấn như vậy. 

Cuối cùng, kết nối có thể là gián tiếp hơn là trực tiếp. Coi như```
1
3 2 2
1 2
2 3
1 3
1 1
```Đầu ra là`11`. Không có cạnh trực tiếp nào giữa 1 và 3, nhưng đường dẫn (1 \rightarrow 2 \rightarrow 3) khiến chúng trở thành một phần của cùng một thành phần liên thông. Chỉ tìm kiếm các cặp được liệt kê rõ ràng sẽ trả lời sai`0`cho truy vấn đầu tiên. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là xây dựng danh sách kề và chạy BFS hoặc DFS cho mọi truy vấn. Bắt đầu từ (x), chúng ta khám phá tất cả các hạt có thể tiếp cận được từ (x), sau đó kiểm tra xem liệu (y) có đạt được hay không. Điều này đúng vì khả năng tiếp cận của biểu đồ chính xác là định nghĩa của mối quan hệ vướng víu bắc cầu. 

Vấn đề là công việc lặp đi lặp lại. Trong trường hợp xấu nhất, một BFS có thể truy cập tất cả các đỉnh (N) và kiểm tra tất cả các cạnh (M), đưa ra thời gian (O(N+M)) cho một truy vấn. Với truy vấn (Q), tổng số sẽ trở thành (O(Q(N+M))). Ở giá trị tối đa, đây là khoảng (10^5(10^5+10^5)=2\cdot10^{10}) thao tác, vượt xa giới hạn thời gian. 

Quan sát hữu ích là bản thân biểu đồ không thay đổi giữa các truy vấn. Mọi truy vấn đều hỏi cùng một câu hỏi về việc phân chia cố định các đỉnh thành các thành phần liên thông. Chúng ta không cần phải khám phá lại các thành phần đó một cách riêng biệt cho từng cặp. 

Cấu trúc Disjoint Set Union, còn được gọi là Union-Find, thể hiện chính xác phân vùng này. Trong khi đọc mọi cạnh ((u,v)), chúng ta hợp nhất các thành phần chứa (u) và (v). Sau khi tất cả các cạnh đã được xử lý, hai hạt được kết nối chính xác khi đại diện DSU của chúng bằng nhau. Mỗi truy vấn sau đó có thể được trả lời bằng hai`find`hoạt động. 

Giải pháp brute-force hoạt động vì truyền tải đồ thị phát hiện chính xác khả năng tiếp cận, nhưng không thành công vì liên tục phát hiện ra các thành phần giống nhau. Quan sát cho thấy tất cả các truy vấn đều tham chiếu đến một biểu đồ cố định cho phép chúng tôi tính toán các thành phần được kết nối của nó một lần và giảm từng truy vấn thành một so sánh thành phần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(Q(N+M))) | (O(N+M)) | Quá chậm | 
| Tối ưu | (O((N+M+Q)\alpha(N))) | (O(N)) | Đã chấp nhận | 

Ở đây (\alpha(N)) là hàm Ackermann nghịch đảo, hàm này tăng chậm đến mức nó thực sự không đổi đối với tất cả các kích thước đầu vào thực tế. 

## Hướng dẫn thuật toán 

1. Tạo cấu trúc DSU với một bộ cho mỗi hạt. Ban đầu, mỗi hạt là thành phần riêng của nó vì chưa có mối quan hệ nào được xử lý. 
2. Với mỗi (M) quan hệ vướng víu đã biết ((u,v)), hãy gọi`union(u, v)`. Nếu hai hạt đã thuộc cùng một thành phần thì không cần thay đổi gì. Mặt khác, các thành phần của chúng được hợp nhất vì một cạnh giữa chúng làm cho mọi hạt có thể tiếp cận được từ bên này và có thể tiếp cận được từ bên kia. 
3. Sau khi tất cả các quan hệ đã được xử lý, đọc từng truy vấn ((x,y)) và tính toán`find(x)`Và`find(y)`. Nếu các đại diện bằng nhau, hãy nối thêm`1`để trả lời. Nếu không thì nối thêm`0`. 
4. In các ký tự đã thu thập dưới dạng một chuỗi. Việc tạo câu trả lời trong danh sách sẽ tránh việc liên tục tạo một chuỗi dài mới trong khi xử lý các truy vấn. 

### Tại sao nó hoạt động 

Bất biến DSU là hai hạt có cùng một đại diện chính xác khi tất cả các cạnh được xử lý kết nối chúng thông qua một thành phần được kết nối. Ban đầu điều này đúng vì mỗi hạt đều đơn độc. Khi một cạnh nối hai thành phần khác nhau,`union`hợp nhất chính xác hai thành phần đó, bảo toàn tính bất biến. Khi hai điểm cuối đã ở trong cùng một thành phần, cạnh đó không thêm kết nối mới và việc giữ nguyên DSU là đúng. 

Sau khi mỗi cạnh (M) được xử lý, các thành phần DSU chính xác là các thành phần được kết nối của toàn bộ đồ thị. Một truy vấn hỏi xem (x) và (y) có được kết nối hay không, tương đương với việc hỏi liệu chúng có cùng một đại diện hay không. Vì vậy mọi ký tự đầu ra đều chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    output = []

    for _ in range(t):
        n, m, q = map(int, input().split())

        parent = list(range(n + 1))
        size = [1] * (n + 1)

        def find(x):
            while parent[x] != x:
                parent[x] = parent[parent[x]]
                x = parent[x]
            return x

        def union(a, b):
            ra = find(a)
            rb = find(b)

            if ra == rb:
                return

            if size[ra] < size[rb]:
                ra, rb = rb, ra

            parent[rb] = ra
            size[ra] += size[rb]

        for _ in range(m):
            u, v = map(int, input().split())
            union(u, v)

        ans = []

        for _ in range(q):
            x, y = map(int, input().split())
            ans.append('1' if find(x) == find(y) else '0')

        output.append(''.join(ans))

    sys.stdout.write('\n'.join(output))

if __name__ == "__main__":
    solve()
```các`parent`mảng lưu trữ phần tử cha của mỗi nút DSU. Một gốc được xác định bởi`parent[x] == x`, vì vậy đang khởi tạo`parent`với`range(n + 1)`tạo ra (N) thành phần riêng biệt. Vị trí bổ sung ở chỉ số 0 cho phép chúng ta sử dụng trực tiếp số hạt vì các đỉnh đầu vào được đánh số từ 1 đến (N). 

các`size`mảng hỗ trợ liên kết theo kích thước. Khi hai rễ khác nhau được hợp nhất, cây nhỏ hơn sẽ được gắn bên dưới cây lớn hơn. Điều này giữ cho cây DSU cạn. các`find`Hàm cũng thực hiện nén đường dẫn bằng cách thay đổi từng nút được truy cập thành điểm gần nút gốc hơn. Sự kết hợp của hai cách tối ưu hóa này mang lại độ phức tạp khấu hao gần như không đổi được biểu thị bằng (\alpha(N)). 

Các cạnh trùng lặp được xử lý tự nhiên bởi`union`. Nếu cả hai điểm cuối đều có cùng một đại diện thì hàm sẽ trả về ngay lập tức. Các cạnh tự hoạt động theo cách tương tự và chúng không cần bất kỳ trường hợp đặc biệt nào. 

Mã xử lý tất cả các cạnh trước khi trả lời các truy vấn, điều này phù hợp với thực tế là toàn bộ biểu đồ được cố định trước khi đặt ra bất kỳ câu hỏi nào. Không cần danh sách kề, do đó việc triển khai chỉ sử dụng bộ nhớ phụ (O(N)). 

Không có vấn đề tràn số nguyên trong Python. Việc thực hiện sử dụng lặp đi lặp lại`find`, điều này cũng tránh được các vấn đề về độ sâu đệ quy có thể phát sinh từ việc triển khai đệ quy trong Python. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, đồ thị chứa các hạt 1 và 2 được nối với nhau bằng một cạnh. Lần xuất hiện thứ hai của`1 2`ở đầu vào là quan hệ trùng lặp nên không làm thay đổi cấu trúc thành phần. 

| Hoạt động | Cặp | Nước đại diện | Kết quả truy vấn | 
| --- | --- | --- | --- | 
| Khởi tạo | không |`{1}`,`{2}`,`{3}`| | 
| Liên minh |`(1, 2)`|`{1,2}`,`{3}`| | 
| Truy vấn |`(1,2)`|`find(1) = 1`,`find(2) = 1`|`1`| 
| Truy vấn |`(2,3)`|`find(2) = 1`,`find(3) = 3`|`0`| 
| Truy vấn |`(3,1)`|`find(3) = 3`,`find(1) = 1`|`0`| 
| Truy vấn |`(2,2)`|`find(2) = 1`,`find(2) = 1`|`1`| 

Chuỗi kết quả là`1001`. Dấu vết này thể hiện cả việc xây dựng thành phần không trùng lặp và thực tế là một hạt luôn được kết nối với chính nó. 

Đối với ví dụ thứ hai, hãy xem xét đầu vào sau:```
1
5 3 4
1 2
2 3
4 5
1 3
1 5
4 5
2 2
```Ba cạnh đầu tiên tạo ra hai thành phần không cần thiết là ({1,2,3}) và ({4,5}), trong khi hạt 5 không được kết nối với thành phần đầu tiên. 

| Hoạt động | Cặp | Thành Phần/Người Đại Diện | Kết quả truy vấn | 
| --- | --- | --- | --- | 
| Khởi tạo | không |`{1}`,`{2}`,`{3}`,`{4}`,`{5}`| | 
| Liên minh |`(1,2)`|`{1,2}`,`{3}`,`{4}`,`{5}`| | 
| Liên minh |`(2,3)`|`{1,2,3}`,`{4}`,`{5}`| | 
| Liên minh |`(4,5)`|`{1,2,3}`,`{4,5}`| | 
| Truy vấn |`(1,3)`| cùng đại diện |`1`| 
| Truy vấn |`(1,5)`| đại diện khác nhau |`0`| 
| Truy vấn |`(4,5)`| cùng đại diện |`1`| 
| Truy vấn |`(2,2)`| cùng đại diện |`1`| 

Đầu ra là`1011`. Truy vấn đầu tiên xác nhận rằng DSU nắm bắt được kết nối gián tiếp, trong khi truy vấn thứ hai xác nhận rằng các thành phần riêng biệt vẫn tách biệt. Truy vấn cuối cùng thực hiện thuộc tính tự kết nối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O((N+M+Q)\alpha(N))) | Mỗi cạnh thực hiện tối đa hai thao tác DSU được khấu hao gần như không đổi và mỗi truy vấn thực hiện hai`find`hoạt động | 
| Không gian | (O(N)) | các`parent`Và`size`mỗi mảng chứa (N+1) mục | 

Với (N,M,Q\le10^5), thuật toán chỉ thực hiện một số tuyến tính các hoạt động DSU cho mỗi trường hợp thử nghiệm, mỗi trường hợp có chi phí khấu hao thực tế không đổi. Việc sử dụng bộ nhớ cũng tuyến tính theo (N), thoải mái trong khoảng 256 MB. Cách tiếp cận này tránh việc lưu trữ hoàn toàn biểu đồ, điều này giúp duy trì cả việc triển khai và mức sử dụng bộ nhớ ở mức nhỏ. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_data(inp: str) -> str:
    data = inp.split()
    it = iter(data)

    t = int(next(it))
    output = []

    for _ in range(t):
        n = int(next(it))
        m = int(next(it))
        q = int(next(it))

        parent = list(range(n + 1))
        size = [1] * (n + 1)

        def find(x):
            while parent[x] != x:
                parent[x] = parent[parent[x]]
                x = parent[x]
            return x

        def union(a, b):
            ra = find(a)
            rb = find(b)

            if ra == rb:
                return

            if size[ra] < size[rb]:
                ra, rb = rb, ra

            parent[rb] = ra
            size[ra] += size[rb]

        for _ in range(m):
            union(int(next(it)), int(next(it)))

        ans = []

        for _ in range(q):
            x = int(next(it))
            y = int(next(it))
            ans.append('1' if find(x) == find(y) else '0')

        output.append(''.join(ans))

    return '\n'.join(output)

# Provided sample
assert solve_data(
    """1
3 1 4
1 2
1 2
2 3
3 1
2 2
"""
) == "1001", "sample 1"

# Minimum-size graph, self-query and duplicate self-edge
assert solve_data(
    """1
1 1 3
1 1
1 1
1 1
1 1
"""
) == "111", "minimum-size self queries"

# Indirect connectivity plus a disconnected vertex
assert solve_data(
    """1
4 2 4
1 2
2 3
1 3
1 4
2 3
4 4
"""
) == "1011", "indirect connectivity"

# Duplicate edges and a component containing the largest-numbered vertex
assert solve_data(
    """1
5 4 5
1 2
1 2
2 3
4 5
1 2
2 3
1 5
4 5
5 5
"""
) == "11011", "duplicate edges and boundary vertex"

# Multiple test cases
assert solve_data(
    """2
3 1 3
1 2
1 2
2 3
3 3
4 3 4
1 2
2 3
3 4
1 4
2 4
1 1
3 4
"""
) == "101\n1111", "multiple test cases"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 1 3 / 1 1 / ...`|`111`| Tối thiểu (N), tự truy vấn và tự biên | 
|`4 2 4 / 1 2 / 2 3 / ...`|`1011`| Kết nối gián tiếp, các đỉnh bị ngắt kết nối và tự truy vấn | 
|`5 4 5 / 1 2 / 1 2 / ...`|`11011`| Các cạnh trùng lặp và số đỉnh hợp lệ lớn nhất | 
| Hai trường hợp thử nghiệm |`101`Và`1111`| Thiết lập lại chính xác trạng thái DSU giữa các trường hợp thử nghiệm | 

## Vỏ cạnh 

Một mối quan hệ trùng lặp được xử lý bởi cùng một`union`hoạt động như mọi cạnh khác. Đối với đầu vào```
1
3 2 3
1 2
1 2
1 2
2 3
3 1
```cái đầu tiên`1 2`tạo thành phần ({1,2}), trong khi thành phần thứ hai bị bỏ qua vì cả hai điểm cuối đều có cùng một đại diện. Các truy vấn tạo ra`110`, như mong đợi. DSU không bao giờ giả định rằng mọi cạnh đầu vào phải hợp nhất hai thành phần khác nhau. 

Để tự truy vấn, hãy xem xét```
1
1 1 1
1 1
1 1
```Hạt 1 bắt đầu như thành phần của chính nó. Cạnh`1 1`giữ nguyên thành phần đó và truy vấn so sánh`find(1)`với chính nó. Cả hai cuộc gọi đều trả về cùng một gốc, vì vậy đầu ra là`1`. Không cần nhánh đặc biệt cho (x=y) vì định nghĩa DSU đã xử lý nó một cách chính xác. 

Kết nối gián tiếp được bao phủ bởi```
1
3 2 1
1 2
2 3
1 3
```Sau khi xử lý cạnh đầu tiên, hạt 1 và hạt 2 có chung một đại diện. Sau khi xử lý hạt thứ hai, hạt 3 được hợp nhất thành thành phần tương tự. Sau đó, truy vấn sẽ tìm cùng một đại diện cho 1 và 3 và đưa ra kết quả`1`, mặc dù cặp`1 3`không bao giờ xuất hiện như một cạnh đầu vào. 

Truy vấn bị ngắt kết nối hoạt động ngược lại. Với```
1
4 2 1
1 2
2 3
1 4
```các thành phần là ({1,2,3}) và ({4}). Truy vấn so sánh đại diện của 1 với đại diện của 4, khác nhau nên câu trả lời là`0`. Đây chính xác là sự khác biệt giữa khả năng tiếp cận trực tiếp bên trong một thành phần và các phần tử không liên quan. 

Cuối cùng, nhiều trường hợp thử nghiệm yêu cầu DSU mới cho mỗi biểu đồ. các`parent`Và`size`các mảng được phân bổ bên trong vòng lặp ca kiểm thử, do đó không có thông tin thành phần nào có thể bị rò rỉ từ trường hợp kiểm thử này sang trường hợp kiểm thử tiếp theo.
