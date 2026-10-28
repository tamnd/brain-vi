---
title: "CF 106160E - Thang máy khắc nghiệt"
description: "Mỗi đỉnh đồ thị có một mã định danh duy nhất. Một trong các đỉnh có mã định danh là 1. Chúng ta được phép xóa các cạnh nhưng không được phép thêm các cạnh. Sau khi xóa một số cạnh, chúng ta xem xét thành phần liên thông chứa đỉnh có mã định danh là 1."
date: "2026-06-25T11:12:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106160
codeforces_index: "E"
codeforces_contest_name: "2025 Benelux Algorithm Programming Contest (BAPC 25)"
rating: 0
weight: 106160
solve_time_s: 56
verified: true
draft: false
---

[CF 106160E - Thang máy khắc nghiệt](https://codeforces.com/problemset/problem/106160/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi đỉnh đồ thị có một mã định danh duy nhất. Một trong các đỉnh có mã định danh`1`. 

Chúng ta được phép xóa các cạnh nhưng không được phép thêm các cạnh. Sau khi xóa một số cạnh, chúng ta xem xét thành phần liên thông chứa đỉnh có mã định danh là`1`. 

Hướng dẫn sử dụng thang máy yêu cầu các mã định danh xuất hiện trong thành phần đó phải chính xác`1, 2, 3, ..., k`đối với một số người`k`. 

Nhiệm vụ là tối đa hóa điều này`k`. 

Một cách hữu ích để phát biểu lại vấn đề là: với mọi giá trị ứng viên`k`, thành phần phải chứa đỉnh duy nhất có mã định danh`1`, đỉnh duy nhất có mã định danh`2`, v.v. cho đến`k`và nó không được chứa định danh lớn hơn`k`. 

Đồ thị có tới`2 · 10^5`đỉnh và`4 · 10^5`các cạnh. Bất kỳ giải pháp nào chạy lặp lại việc truyền tải đồ thị cho nhiều giá trị khác nhau của`k`sẽ là quá đắt. Ví dụ: kiểm tra kết nối từ đầu cho mọi tiền tố sẽ tốn khoảng`O(n(n+m))`, vượt xa giới hạn. 

Quan sát tinh tế đầu tiên là các mã định danh không được đảm bảo là liên tiếp. Giả sử các định danh là:```
1 2 4
```Không có cách nào để có được hoán vị kích thước`3`, bởi vì định danh`3`không tồn tại ở bất cứ đâu trong biểu đồ. 

Một lỗi dễ mắc phải khác là kiểm tra kết nối trong biểu đồ gốc thay vì bên trong bộ định danh được phép. 

Coi như:```
ids: 1 2 3
edges:
1-3
3-2
```Nếu định danh`3`lớn hơn ứng viên`k = 2`, thì các đỉnh`1`Và`2`không được kết nối chỉ bằng cách sử dụng số nhận dạng`≤ 2`. Đường dẫn thông qua định danh`3`không thể được sử dụng vì đỉnh đó cũng thuộc về thành phần. 

Trường hợp cạnh thứ hai xuất hiện khi số nhận dạng được yêu cầu tồn tại nhưng được chia thành nhiều thành phần.```
ids: 1 2 3
edges:
(1,2)
```Số nhận dạng`1..3`tồn tại nhưng đồ thị cảm ứng trên các đỉnh đó không liên thông nên`k = 3`là không thể. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi giá trị khả thi của`k`. 

Đối với một cố định`k`, chúng tôi lấy tất cả các đỉnh có định danh tối đa`k`, xây dựng đồ thị cảm ứng và kiểm tra xem các đỉnh đó có tạo thành thành phần liên thông hay không. Nếu đúng như vậy thì bằng cách xóa mọi cạnh đi từ tập hợp này ra thế giới bên ngoài, thành phần chứa mã định danh`1`trở thành chính xác các định danh`1..k`. 

Điều này đúng, nhưng quá chậm. Có thể có tới`2 · 10^5`giá trị ứng viên của`k`và mỗi lần kiểm tra kết nối có thể quét toàn bộ biểu đồ. 

Quan sát quan trọng là các tập hợp chúng tôi kiểm tra được lồng nhau. 

Cho phép`S_k`là các đỉnh có định danh thuộc về`1..k`. 

Khi chúng tôi di chuyển từ`k`ĐẾN`k+1`, chúng ta thêm chính xác một đỉnh mới, đỉnh có mã định danh là`k+1`. Chúng tôi không bao giờ loại bỏ các đỉnh. 

Điều đó làm cho vấn đề này trở thành một vấn đề kết nối động trong đó các đỉnh được kích hoạt theo thứ tự nhận dạng tăng dần. Cấu trúc Disjoint Set Union là lý tưởng cho tình huống này. 

Khi một đỉnh mới hoạt động, chúng ta chỉ cần kết nối nó với các đỉnh lân cận đã hoạt động. Nếu chúng ta duy trì số lượng thành phần được kết nối giữa các đỉnh hoạt động thì:```
S_k is connected
⇔
the number of active components equals 1
```Chi tiết duy nhất còn lại là số nhận dạng có thể có khoảng trống. Nếu định danh`k+1`không tồn tại thì không thể hoán vị lớn hơn vì tập hợp`1..k+1`không bao giờ có thể hình thành được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n(n+m)) | O(n+m) | Quá chậm | 
| DSU tối ưu | O((n+m) α(n)) | O(n+m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các mã định danh và xác định đỉnh tương ứng với mỗi mã định danh. 
2. Tính toán tiền tố liên tiếp lớn nhất hiện có trên toàn cầu của các mã định danh. Bắt đầu từ`1`, tiếp tục tăng trong khi mã định danh tiếp theo tồn tại. Hãy để giới hạn này là`L`. 
3. Xây dựng danh sách kề của đồ thị. 
4. Kích hoạt các đỉnh theo thứ tự định danh`1, 2, ..., L`. 
5. Khi kích hoạt đỉnh bằng mã định danh`k`, tạo thành phần DSU mới cho nó và tăng số lượng thành phần hoạt động lên một. 
6. Đối với mỗi lân cận của đỉnh đó, hãy kiểm tra xem lân cận đó đã hoạt động chưa. Điều này xảy ra chính xác khi số nhận dạng của nó nhiều nhất`k`. 
7. Liên kết hai đỉnh bất cứ khi nào chúng thuộc các bộ DSU khác nhau. Mỗi liên kết thành công sẽ giảm số lượng thành phần hoạt động đi một. 
8. Sau khi xử lý tất cả các kết hợp để định danh`k`, kiểm tra xem số lượng thành phần hoạt động có bằng không`1`. 
9. Nếu đúng như vậy thì tất cả các đỉnh có mã định danh`1..k`tạo thành một đồ thị cảm ứng liên thông, do đó`k`là có thể đạt được. Cập nhật câu trả lời. 
10. Tiếp tục cho đến khi nhận dạng được`L`được xử lý. 

### Tại sao nó hoạt động 

Đối với một cố định`k`, các đỉnh hoạt động chính xác là những đỉnh có định danh thuộc về`1..k`. 

DSU chứa chính xác các cạnh có điểm cuối đều hoạt động, vì việc kết hợp được thực hiện khi điểm cuối sau hoạt động. 

Kết quả là DSU biểu diễn các thành phần liên thông của đồ thị cảm ứng trên các mã định danh`1..k`. 

Nếu số lượng thành phần hoạt động là`1`, đồ thị cảm ứng đó được kết nối. Sau đó, chúng ta có thể xóa mọi cạnh khỏi tập hợp thành các đỉnh có số nhận dạng lớn hơn, để lại một thành phần có số nhận dạng chính xác`1..k`. 

Nếu số lượng thành phần hoạt động lớn hơn`1`, không có thao tác xóa cạnh nào có thể kết nối các phần đó lại với nhau, vì việc xóa không bao giờ tạo ra đường dẫn mới. 

Như vậy một giá trị`k`khả thi chính xác khi số lượng thành phần hoạt động bằng`1`, và lấy giá trị lớn nhất như vậy`k`đưa ra câu trả lời tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class DSU:
    def __init__(self, n):
        self.parent = list(range(n))
        self.size = [1] * n

    def find(self, x):
        while self.parent[x] != x:
            self.parent[x] = self.parent[self.parent[x]]
            x = self.parent[x]
        return x

    def union(self, a, b):
        a = self.find(a)
        b = self.find(b)

        if a == b:
            return False

        if self.size[a] < self.size[b]:
            a, b = b, a

        self.parent[b] = a
        self.size[a] += self.size[b]
        return True

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))

    pos = {}
    for i, x in enumerate(a):
        pos[x] = i

    g = [[] for _ in range(n)]

    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append(v)
        g[v].append(u)

    L = 1
    while L + 1 in pos:
        L += 1

    dsu = DSU(n)
    active = [False] * n
    components = 0
    answer = 0

    for ident in range(1, L + 1):
        v = pos[ident]

        active[v] = True
        components += 1

        for to in g[v]:
            if active[to]:
                if dsu.union(v, to):
                    components -= 1

        if components == 1:
            answer = ident

    print(answer)

solve()
```Phần đầu tiên ánh xạ mọi định danh tới đỉnh của nó. Điều này cho phép chúng tôi kích hoạt các đỉnh trực tiếp theo thứ tự định danh. 

Biến`L`là tiền tố liên tiếp lớn nhất của số nhận dạng thực sự tồn tại trong đầu vào. Khi thiếu một số, mọi hoán vị lớn hơn sẽ không thể thực hiện được, vì vậy không có lý do gì để xử lý thêm các số nhận dạng. 

DSU chỉ duy trì kết nối giữa các đỉnh hoạt động. Một đỉnh sẽ hoạt động chính xác khi mã định danh của nó nhập vào tiền tố hiện tại. 

Bộ đếm thành phần là chi tiết triển khai quan trọng. Kích hoạt một đỉnh sẽ tạo ra một thành phần mới. Mỗi liên minh thành công sẽ hợp nhất hai thành phần và giảm bộ đếm đi một. Sau khi xử lý định danh`k`, bộ đếm bằng số lượng thành phần được kết nối trong biểu đồ cảm ứng trên mã định danh`1..k`. 

Không cần xử lý đặc biệt đối với các cạnh dẫn đến các đỉnh không hoạt động. Các đỉnh đó tương ứng với các mã định danh lớn hơn giá trị hiện tại`k`, nên họ cố tình bỏ qua. 

## Ví dụ đã hoạt động 

### Ví dụ 1```
n = 5
ids = [4, 3, 10, 2, 1]

edges:
1-2
4-5
2-3
2-4
1-3
2-5
```Tiền tố định danh liên tiếp là`1,2,3,4`, Vì thế`L = 4`. 

| k | Định danh được kích hoạt | Linh kiện sau đoàn | Đã kết nối? | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | Có | 1 | 
| 2 | 2 | 1 | Có | 2 | 
| 3 | 3 | 1 | Có | 3 | 
| 4 | 4 | 1 | Có | 4 | 

Đồ thị cảm ứng trên định danh`1..4`được kết nối, vì vậy câu trả lời trở thành`4`. 

Ví dụ này cho thấy rằng các mã định danh lớn hơn giá trị đích có thể bị ngắt kết nối đơn giản bằng cách xóa các cạnh. 

### Ví dụ 2```
ids = [1, 2, 3]
edges:
1-2
```| k | Định danh được kích hoạt | Linh kiện sau đoàn | Đã kết nối? | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | Có | 1 | 
| 2 | 2 | 1 | Có | 2 | 
| 3 | 3 | 2 | Không | 2 | 

Đỉnh thứ ba không có kết nối với hai đỉnh đầu tiên, vì vậy đồ thị cảm ứng trên các mã định danh`1..3`bị ngắt kết nối. 

Câu trả lời vẫn còn`2`. 

Điều này chứng tỏ rằng chỉ có tất cả các định danh bắt buộc là chưa đủ, chúng còn phải tạo thành một biểu đồ cảm ứng được kết nối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) α(n)) | Mỗi cạnh được xem xét nhiều nhất hai lần, các phép toán DSU gần như không đổi | 
| Không gian | O(n + m) | Danh sách kề, DSU và mảng phụ trợ | 

Với`n ≤ 2 · 10^5`Và`m ≤ 4 · 10^5`, điều này dễ dàng nằm trong giới hạn. DSU chỉ đóng góp chi phí nghịch đảo của Ackermann, vốn không đổi trong thực tế. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    class DSU:
        def __init__(self, n):
            self.parent = list(range(n))
            self.size = [1] * n

        def find(self, x):
            while self.parent[x] != x:
                self.parent[x] = self.parent[self.parent[x]]
                x = self.parent[x]
            return x

        def union(self, a, b):
            a = self.find(a)
            b = self.find(b)

            if a == b:
                return False

            if self.size[a] < self.size[b]:
                a, b = b, a

            self.parent[b] = a
            self.size[a] += self.size[b]
            return True

    n, m = map(int, input().split())
    a = list(map(int, input().split()))

    pos = {}
    for i, x in enumerate(a):
        pos[x] = i

    g = [[] for _ in range(n)]

    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append(v)
        g[v].append(u)

    L = 1
    while L + 1 in pos:
        L += 1

    dsu = DSU(n)
    active = [False] * n
    comp = 0
    ans = 0

    for ident in range(1, L + 1):
        v = pos[ident]
        active[v] = True
        comp += 1

        for to in g[v]:
            if active[to]:
                if dsu.union(v, to):
                    comp -= 1

        if comp == 1:
            ans = ident

    return str(ans) + "\n"

# sample
assert run(
"""5 6
4 3 10 2 1
1 2
4 5
2 3
2 4
1 3
2 5
"""
) == "4\n"

# single vertex
assert run(
"""1 0
1
"""
) == "1\n"

# missing identifier 2
assert run(
"""3 2
1 3 4
1 2
2 3
"""
) == "1\n"

# all identifiers exist but graph disconnected
assert run(
"""3 1
1 2 3
1 2
"""
) == "2\n"

# chain becomes connected only at the end
assert run(
"""4 3
1 2 3 4
1 2
2 3
3 4
"""
) == "4\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đỉnh đơn | 1 | Kích thước đầu vào tối thiểu | 
| Thiếu định danh 2 | 1 | Giới hạn tiền tố liên tiếp | 
| Đỉnh thứ ba bị ngắt kết nối | 2 | Yêu cầu kết nối | 
| Dây chuyền đơn giản | 4 | Tiền tố đầy đủ được kết nối | 
| Trường hợp mẫu | 4 | Hành vi chính thức | 

## Vỏ cạnh 

Hãy xem xét các định danh:```
3 1 4
```Mã định danh`2`không tồn tại. 

Thuật toán tính toán`L = 1`, vì tiền tố liên tiếp dừng ngay sau`1`. Chỉ định danh`1`được xử lý và câu trả lời là`1`. 

Không thể có câu trả lời lớn hơn vì hoán vị kích thước`2`yêu cầu mã định danh`2`. 

Bây giờ hãy xem xét:```
3 1
1 2 3
1 2
```Khi nhận dạng`1`được kích hoạt, có một thành phần. 

Khi nhận dạng`2`được kích hoạt, cạnh`(1,2)`hợp nhất hai đỉnh thành một thành phần. 

Khi nhận dạng`3`được kích hoạt, nó tạo thành thành phần thứ hai vì không có cạnh nào kết nối nó với đồ thị hoạt động. 

Số lượng thành phần trở thành`2`, Vì thế`k = 3`bị từ chối và câu trả lời vẫn giữ nguyên`2`. 

Cuối cùng, hãy xem xét:```
3 2
1 2 100
1 3
2 3
```Số nhận dạng`1`Và`2`không được kết nối trực tiếp nhưng cả hai đều kết nối thông qua mã định danh`100`. 

Vì`k = 2`, đỉnh`100`không hoạt động và không thể tham gia vào biểu đồ cảm ứng. Biểu đồ hoạt động không chứa cạnh giữa các mã định danh`1`Và`2`, thế là nó bị ngắt kết nối. 

Thuật toán bác bỏ chính xác`k = 2`, phản ánh thực tế là thành phần cuối cùng có thể không chứa các mã định danh lớn hơn`k`.
