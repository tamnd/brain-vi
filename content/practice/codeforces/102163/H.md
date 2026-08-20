---
title: "CF 102163H - Ông Hamra và các hạt lượng tử của ông"
description: "Hãy coi các hạt là các đỉnh của đồ thị vô hướng. Mọi mối quan hệ vướng víu đã biết giữa hai hạt đều là một cạnh."
date: "2026-08-20T00:02:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102163
codeforces_index: "H"
codeforces_contest_name: "NCD 2019"
rating: 0
weight: 102163
solve_time_s: 599
verified: false
draft: false
---

[CF 102163H - Ông Hamra và các hạt lượng tử của ông](https://codeforces.com/problemset/problem/102163/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 9 phút 59 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Hãy coi các hạt là các đỉnh của đồ thị vô hướng. Mọi mối quan hệ vướng víu đã biết giữa hai hạt đều là một cạnh. Tính chất đặc biệt của sự vướng víu nói rằng nếu hạt (A) được kết nối với (B) và (B) được kết nối với (C), thì (A) cũng phải được kết nối với (C). Áp dụng nhiều lần quy tắc này có nghĩa là mọi cặp hạt thuộc cùng một thành phần liên kết đều được coi là vướng víu. 

Đối với mỗi trường hợp thử nghiệm, chúng tôi nhận được (N) hạt, (M) mối quan hệ đã biết và (Q) truy vấn. Mỗi truy vấn đưa ra hai số hạt (X) và (Y). Chúng ta phải xuất ra một chuỗi nhị phân có độ dài (Q), trong đó ký tự ở vị trí (i) là`1`chính xác khi hai phần tử trong truy vấn thứ (i) thuộc về cùng một thành phần được kết nối. 

Ví dụ, với các cạnh (1-2) và (2-3), các hạt (1) và (3) không liên quan trực tiếp với nhau ở đầu vào mà chúng bị vướng víu thông qua hạt (2). Do đó, một truy vấn cho (1,3) phải tạo ra`1`. 

Các giá trị của (N), (M) và (Q) đều có thể đạt tới (10^5). Một thuật toán kiểm tra toàn bộ biểu đồ một cách riêng biệt cho mỗi truy vấn có thể thực hiện theo thứ tự (10^{10}), vượt xa giới hạn 2 giây có thể chịu đựng được. Ngay cả các thuật toán có tiền xử lý (O(N^2)) cũng không phù hợp ở quy mô này. Chúng ta cần một giải pháp gần tuyến tính về số lượng hạt, mối quan hệ và truy vấn. 

Có một số trường hợp đặc biệt có thể dẫn đến việc triển khai không chính xác. 

Một truy vấn có thể chứa cùng một phần tử hai lần. Ví dụ,```
1
1 1 3
1 1
1 1
1 1
1 1
```có đầu ra`111`. Một hạt luôn ở trong cùng một thành phần được kết nối với chính nó, do đó, việc triển khai bất cẩn chỉ kiểm tra xem một cạnh thực tế có tồn tại hay không có thể trả về sai`0`. 

Các mối quan hệ trùng lặp cũng được cho phép bởi định dạng đầu vào. Ví dụ,```
1
2 3 2
1 2
1 2
2 1
1 2
```có đầu ra`11`. Cạnh lặp lại không tạo thành phần mới hoặc yêu cầu xử lý đặc biệt. Việc triển khai giả định mọi cạnh là duy nhất không cần phải đúng về tính duy nhất vì khả năng kết nối của biểu đồ không thay đổi. 

Kết nối gián tiếp là trường hợp cạnh trung tâm. Coi như```
1
3 2 2
1 2
2 3
1 3
1 1
```Đầu ra là`11`. Các hạt (1) và (3) không có mối quan hệ trực tiếp ở đầu vào, nhưng đường đi (1 \rightarrow 2 \rightarrow 3) khiến chúng bị vướng víu. Chỉ kiểm tra các cạnh ban đầu sẽ tạo ra sai số`01`. 

Cuối cùng, các hạt bị ngắt kết nối phải ở trạng thái tách biệt. Vì```
1
4 1 3
1 2
1 2
3 4
1 3
4 4
```đầu ra là`101`. Các hạt (1) và (2) thuộc về một thành phần, các hạt (3) và (4) thuộc về một thành phần khác và không có mối liên hệ nào giữa các thành phần đó. Giải pháp vô tình hợp nhất các nhóm không liên quan sẽ tạo ra kết quả không chính xác cho truy vấn thứ hai. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là xử lý mọi truy vấn một cách độc lập. Cho (X) và (Y), chạy DFS hoặc BFS bắt đầu từ (X), tuân theo các mối quan hệ đã biết và kiểm tra xem có thể đạt được (Y) hay không. Điều này đúng vì khả năng tiếp cận của biểu đồ chính xác là định nghĩa thuộc về cùng một thành phần được kết nối. 

Vấn đề là công việc lặp đi lặp lại. Trong trường hợp xấu nhất, một lần duyệt đơn sẽ kiểm tra các đỉnh và cạnh (O(N+M)). Làm điều đó cho tất cả (Q) truy vấn có chi phí (O(Q(N+M))). Với (N=M=Q=10^5), điều này có thể đạt được khoảng (10^{10}) phép tính biểu đồ. Thành phần được kết nối tương tự sẽ được khám phá lại từ đầu hàng nghìn lần. 

Chúng ta có thể làm tốt hơn bằng cách xem xét tất cả các truy vấn thực sự đang yêu cầu điều gì. Không ai trong số họ quan tâm đến đường đi giữa hai hạt. Họ chỉ quan tâm đến thành phần được kết nối nào chứa từng hạt. Về mặt khái niệm, toàn bộ biểu đồ có thể được nén thành các nhóm, trong đó mọi hạt trong một nhóm đều có cùng một câu trả lời khi so sánh với một hạt khác trong nhóm đó. 

Đây chính xác là tình huống được xử lý bởi cấu trúc Disjoint Set Union, còn được gọi là Union-Find. Trong khi đọc mọi mối quan hệ đã biết (u,v), chúng ta hợp nhất các tập hợp chứa (u) và (v). Sau khi tất cả các mối quan hệ (M) đã được xử lý, hai hạt bị vướng víu một cách chính xác khi đại diện DSU của chúng bằng nhau. 

Thay đổi quan trọng là chúng tôi tính toán kết nối một lần trong khi đọc các cạnh, thay vì khám phá lại nó cho mọi truy vấn. DSU hỗ trợ hợp nhất hai thành phần và tìm đại diện thành phần của hạt trong thời gian khấu hao gần như không đổi. Với việc nén và kết hợp đường dẫn theo kích thước hoặc thứ hạng, tổng độ phức tạp là (O((N+M+Q)\alpha(N))), trong đó (\alpha) là hàm Ackermann nghịch đảo và thực tế là một hằng số cho tất cả các kích thước đầu vào thực tế. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(Q(N+M))) | (O(N+M)) | Quá chậm | 
| Tối ưu | (O((N+M+Q)\alpha(N))) | (O(N)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo DSU chứa tất cả (N) hạt. Ban đầu, mỗi hạt là thành phần riêng của nó, vì chưa có mối quan hệ nào được xử lý. 
2. Đọc từng mối quan hệ (M) (u,v) và gọi`union(u, v)`. Nếu các hạt đã có trong cùng một thành phần thì không có gì thay đổi. Nếu không thì hai thành phần của chúng sẽ được hợp nhất. 

Điều này trực tiếp mô hình hóa bản chất bắc cầu của sự vướng víu. Nếu (A) được nối với (B) và sau đó (B) được nối với (C), DSU sẽ tự động đặt cả ba vào một bộ. 
3. Sau khi tất cả các mối quan hệ đã được xử lý, đọc từng truy vấn (X,Y) và tính toán`find(X)`Và`find(Y)`. 
4. Nối thêm`1`vào câu trả lời nếu hai đại diện bằng nhau và nối thêm`0`nếu không thì. Các đại diện bằng nhau chính xác khi các hạt thuộc cùng một thành phần được kết nối. 
5. In các ký tự tích lũy dưới dạng một chuỗi cho trường hợp kiểm thử. Việc tạo một chuỗi được ưu tiên hơn là in một ký tự cho mỗi truy vấn vì nó tránh được một số lượng lớn các thao tác đầu ra. 

### Tại sao nó hoạt động 

DSU duy trì tính bất biến rằng hai hạt có cùng một đại diện chính xác khi các mối quan hệ được xử lý cho đến nay kết nối chúng thông qua một đường dẫn nào đó. Ban đầu điều này đúng vì chỉ có một hạt mới có thể tiếp cận được từ hạt đó. Khi một cạnh (u,v) được xử lý, việc hợp nhất hai tập hợp của chúng làm cho mọi hạt có thể tiếp cận từ (u) thuộc về cùng một tập hợp với mọi hạt có thể tiếp cận từ (v), khớp chính xác với các đường dẫn mới được tạo bởi cạnh đó. Không có thành phần không liên quan nào được hợp nhất. Sau khi tất cả các cạnh đã được xử lý, bất biến mô tả các thành phần được kết nối của đồ thị hoàn chỉnh. Do đó, một truy vấn trả về`1`chính xác khi hai hạt của nó được nối với nhau bằng một đường đi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class DSU:
    def __init__(self, n):
        self.parent = list(range(n + 1))
        self.size = [1] * (n + 1)

    def find(self, x):
        while self.parent[x] != x:
            self.parent[x] = self.parent[self.parent[x]]
            x = self.parent[x]
        return x

    def union(self, a, b):
        a = self.find(a)
        b = self.find(b)

        if a == b:
            return

        if self.size[a] < self.size[b]:
            a, b = b, a

        self.parent[b] = a
        self.size[a] += self.size[b]

def solve():
    t = int(input())
    output = []

    for _ in range(t):
        n, m, q = map(int, input().split())

        dsu = DSU(n)

        for _ in range(m):
            u, v = map(int, input().split())
            dsu.union(u, v)

        answer = []

        for _ in range(q):
            x, y = map(int, input().split())
            answer.append('1' if dsu.find(x) == dsu.find(y) else '0')

        output.append(''.join(answer))

    sys.stdout.write('\n'.join(output))

if __name__ == "__main__":
    solve()
```các`DSU`hàm tạo tạo`parent[i] = i`, do đó mọi hạt bắt đầu với tư cách là đại diện cho thành phần của chính nó. các`size`mảng ghi lại số lượng hạt hiện có trong mỗi thành phần. 

các`find`phương thức đi theo con trỏ cha cho đến khi đạt đến gốc. Trong quá trình truyền tải, nó thực hiện giảm một nửa đường dẫn với`self.parent[x] = self.parent[self.parent[x]]`. Điều này rút ngắn các đường dẫn mà các truy vấn trong tương lai gặp phải và mang lại độ phức tạp khấu hao gần như không đổi tương tự như nén đường dẫn tiêu chuẩn. 

các`union`phương pháp đầu tiên tìm thấy các gốc thay vì sửa đổi các nút tùy ý. Nếu cả hai nghiệm đều bằng nhau thì mối quan hệ là dư thừa và không cần thực hiện thao tác nào. Ngược lại, thành phần nhỏ hơn sẽ được gắn bên dưới thành phần lớn hơn. Đây là sự kết hợp theo kích thước và nó ngăn không cho cây DSU trở nên sâu một cách không cần thiết. 

Đầu vào sử dụng số hạt từ (1) đến (N), do đó mảng có độ dài (N+1). Chỉ số 0 không được sử dụng. Điều này tránh việc liên tục trừ đi một số từ mỗi số hạt và loại bỏ nguyên nhân phổ biến gây ra lỗi lập chỉ mục. 

Mã xử lý tất cả các mối quan hệ trước khi trả lời bất kỳ truy vấn nào. Thứ tự đó quan trọng vì truy vấn phải xem kết nối được tạo bởi mọi mối quan hệ trong trường hợp thử nghiệm. 

Số nguyên Python không gặp vấn đề tràn ở đây và các giá trị duy nhất được lưu trữ trong`size`nhiều nhất là (N). Đầu ra được tích lũy cho mỗi trường hợp thử nghiệm và sau đó được ghi một lần, giúp duy trì chi phí I/O ở mức thấp. 

## Ví dụ đã hoạt động 

Tuyên bố cung cấp một mẫu. Vì không có mẫu thứ hai trong văn bản vấn đề được cung cấp nên dấu vết thứ hai bên dưới sử dụng trường hợp thử nghiệm được xây dựng nhỏ nhằm nhấn mạnh đến khả năng kết nối gián tiếp và các thành phần bị ngắt kết nối. 

Đối với mẫu 1,```
1
3 1 4
1 2
1 2
2 3
3 1
2 2
```mối quan hệ ban đầu duy nhất là (1-2). 

| Hoạt động | Đầu vào | Bang gốc | Kết quả truy vấn | 
| --- | --- | --- | --- | 
| Trạng thái ban đầu | không |`[1, 2, 3]`| | 
| Liên minh |`1 2`|`[1, 1, 3]`| | 
| Truy vấn |`1 2`|`[1, 1, 3]`|`1`| 
| Truy vấn |`2 3`|`[1, 1, 3]`|`0`| 
| Truy vấn |`3 1`|`[1, 1, 3]`|`0`| 
| Truy vấn |`2 2`|`[1, 1, 3]`|`1`| 

Chuỗi kết quả là`1001`. Dấu vết chứng tỏ rằng mối quan hệ trực tiếp là đủ cho truy vấn đầu tiên, trong khi phần (3) vẫn bị cô lập. Tự truy vấn cuối cùng cũng xác nhận rằng mọi hạt đều thuộc về thành phần riêng của nó. 

Đối với trường hợp đã xây dựng,```
1
5 3 5
1 2
2 3
4 5
1 3
1 4
2 3
4 5
3 3
```ba hạt đầu tiên tạo thành một thành phần và hai hạt cuối cùng tạo thành một thành phần khác. 

| Hoạt động | Đầu vào | Linh kiện sau khi vận hành | Kết quả truy vấn | 
| --- | --- | --- | --- | 
| Trạng thái ban đầu | không |`{1}`,`{2}`,`{3}`,`{4}`,`{5}`| | 
| Liên minh |`1 2`|`{1,2}`,`{3}`,`{4}`,`{5}`| | 
| Liên minh |`2 3`|`{1,2,3}`,`{4}`,`{5}`| | 
| Liên minh |`4 5`|`{1,2,3}`,`{4,5}`| | 
| Truy vấn |`1 3`|`{1,2,3}`,`{4,5}`|`1`| 
| Truy vấn |`1 4`|`{1,2,3}`,`{4,5}`|`0`| 
| Truy vấn |`2 3`|`{1,2,3}`,`{4,5}`|`1`| 
| Truy vấn |`4 5`|`{1,2,3}`,`{4,5}`|`1`| 
| Truy vấn |`3 3`|`{1,2,3}`,`{4,5}`|`1`| 

Đầu ra là`10111`. Phần quan trọng là truy vấn đầu tiên: các hạt (1) và (3) không có cạnh trực tiếp, nhưng hai phép toán hợp đã đặt chúng vào cùng một thành phần DSU. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O((N+M+Q)\alpha(N))) | Mỗi hoạt động liên kết và tìm kiếm có chi phí khấu hao nghịch đảo với Ackermann | 
| Không gian | (O(N)) | DSU lưu trữ một giá trị gốc và một giá trị kích thước cho mỗi hạt | 

Với (N,M,Q) lên tới (10^5), điều này mang lại số lượng hoạt động DSU gần như tuyến tính cho mỗi trường hợp thử nghiệm. Hệ số Ackermann nghịch đảo rất nhỏ nên lời giải nằm trong độ phức tạp dự định trong giới hạn 2 giây. Việc sử dụng bộ nhớ cũng tuyến tính theo (N), dưới 256 MB. 

## Trường hợp thử nghiệm 

Bộ khai thác thử nghiệm sau đây triển khai thuật toán tương tự như giải pháp đã gửi và kiểm tra mẫu được cung cấp cùng với một số trường hợp được nhắm mục tiêu.```python
import sys
import io

class DSU:
    def __init__(self, n):
        self.parent = list(range(n + 1))
        self.size = [1] * (n + 1)

    def find(self, x):
        while self.parent[x] != x:
            self.parent[x] = self.parent[self.parent[x]]
            x = self.parent[x]
        return x

    def union(self, a, b):
        a = self.find(a)
        b = self.find(b)

        if a == b:
            return

        if self.size[a] < self.size[b]:
            a, b = b, a

        self.parent[b] = a
        self.size[a] += self.size[b]

def solve_io(inp: str) -> str:
    data = io.StringIO(inp)
    t = int(data.readline())
    output = []

    for _ in range(t):
        n, m, q = map(int, data.readline().split())
        dsu = DSU(n)

        for _ in range(m):
            u, v = map(int, data.readline().split())
            dsu.union(u, v)

        answer = []

        for _ in range(q):
            x, y = map(int, data.readline().split())
            answer.append('1' if dsu.find(x) == dsu.find(y) else '0')

        output.append(''.join(answer))

    return '\n'.join(output)

# Provided sample
sample1 = """\
1
3 1 4
1 2
1 2
2 3
3 1
2 2
"""

assert solve_io(sample1) == "1001", "sample 1"

# Minimum-size graph and repeated/self relationships
case2 = """\
1
1 1 4
1 1
1 1
1 1
1 1
"""

assert solve_io(case2) == "1111", "minimum-size self queries"

# Indirect connectivity versus disconnected components
case3 = """\
1
5 3 5
1 2
2 3
4 5
1 3
1 4
2 3
4 5
3 3
"""

assert solve_io(case3) == "10111", "indirect connectivity"

# Duplicate edges and reversed endpoints
case4 = """\
1
4 4 5
1 2
2 1
1 2
3 4
1 2
2 1
3 4
1 3
4 3
"""

assert solve_io(case4) == "11011", "duplicate and reversed edges"

# Boundary particle numbers, including N itself
case5 = """\
1
6 2 5
1 6
5 6
1 5
1 6
5 6
2 6
6 6
"""

assert solve_io(case5) == "11101", "boundary indices"

# Multiple test cases
case6 = """\
2
2 1 2
1 2
1 2
1 1
3 1 3
1 2
1 2
2 3
3 3
"""

assert solve_io(case6) == "111", "multiple test cases"

print("All tests passed.")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 1 4 / 1 1 / ...`|`1111`| Kích thước biểu đồ tối thiểu và tự truy vấn | 
| Chuỗi năm hạt và cặp riêng biệt |`10111`| Kết nối gián tiếp và các thành phần bị ngắt kết nối | 
| Các cạnh trùng lặp và đảo ngược |`11011`| Các mối quan hệ dư thừa không làm thay đổi thành phần | 
| Sáu hạt có cạnh liên quan đến hạt 6 |`11101`| Các chỉ số ranh giới một cơ sở và hạt bị cô lập | 
| Hai trường hợp thử nghiệm trong một đầu vào |`111`| Đặt lại đúng DSU giữa các trường hợp thử nghiệm | 

## Vỏ cạnh 

Trường hợp tự truy vấn có (N=1), với hạt duy nhất được truy vấn đối với chính nó. đầu vào```
1
1 1 3
1 1
1 1
1 1
1 1
```chứa một cạnh tự dư thừa và ba tự truy vấn. Liên minh đầu tiên nhìn thấy cùng một gốc ở cả hai bên, vì vậy nó không thay đổi gì. Mọi truy vấn so sánh nghiệm của hạt (1) với chính nó, tạo ra`111`. Thuật toán không cần trường hợp đặc biệt cho (X=Y`, vì DSU xử lý nó một cách tự nhiên. 

Đối với các mối quan hệ trùng lặp, hãy xem xét```
1
2 3 2
1 2
1 2
2 1
1 2
```Mối quan hệ đầu tiên hợp nhất các hạt (1) và (2). Hai mối quan hệ tiếp theo thấy rằng gốc của chúng đã bằng nhau nên cả hai đều bị bỏ qua. Cả hai truy vấn đều so sánh cùng một đại diện và tạo ra`11`. Đây là lý do tại sao`union`phải xử lý an toàn trường hợp hai đối số của nó đã thuộc về một thành phần. 

Để kết nối gián tiếp, hãy sử dụng```
1
3 2 2
1 2
2 3
1 3
1 1
```Sau đó`union(1, 2)`, các hạt (1) và (2) có chung một đại diện. Sau đó`union(2, 3)`, hạt (3) được hợp nhất thành thành phần đó. Truy vấn`1 3`do đó so sánh các đại diện và lợi nhuận bằng nhau`1`. Truy vấn`1 1`cũng trở lại`1`, cho`11`. Một phương pháp chỉ kiểm tra xem`(1, 3)`xuất hiện dưới dạng cạnh đầu vào sẽ không thành công ở đây. 

Đối với các thành phần bị ngắt kết nối, hãy xem xét```
1
4 1 3
1 2
1 2
3 4
4 4
```Sau khi liên kết đơn, các thành phần được`{1,2}`,`{3}`, Và`{4}`. Truy vấn đầu tiên trả về`1`, trong khi truy vấn thứ hai so sánh các đại diện của (3) và (4), khác nhau nên nó trả về`0`. Tự truy vấn cuối cùng trả về`1`. Kết quả đầu ra là`101`. DSU không bao giờ hợp nhất các phần tử chỉ vì chúng tồn tại trong cùng một trường hợp thử nghiệm, do đó các nhóm không kết nối vẫn được phân biệt. 

Đối với các giá trị được phép lớn nhất, (N=M=Q=10^5), thuật toán thực hiện (10^5) khởi tạo, (10^5) phép toán hợp và (10^5) phép toán truy vấn. Với tính năng nén đường dẫn và kết hợp theo kích thước, điều này vẫn gần như tuyến tính thay vì tiếp cận các phép toán (10^{10}) được yêu cầu bởi việc duyệt đồ thị lặp đi lặp lại. Lý do tương tự áp dụng độc lập cho mọi trường hợp thử nghiệm, vì DSU mới được tạo cho mỗi trường hợp thử nghiệm.
