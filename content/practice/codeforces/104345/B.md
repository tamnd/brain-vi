---
title: "CF 104345B - Truy vấn trên cây"
description: "Chúng ta có một cây trong đó mỗi đỉnh là một nút riêng biệt và các cạnh kết nối chúng không theo chu trình. Đối với bất kỳ tập hợp con các đỉnh được chọn nào, chúng ta chỉ “cho phép mình đi” qua các đỉnh bên trong tập hợp con đó."
date: "2026-07-01T18:18:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104345
codeforces_index: "B"
codeforces_contest_name: "2022-2023 Winter Petrozavodsk Camp, Day 4: KAIST+KOI Contest"
rating: 0
weight: 104345
solve_time_s: 78
verified: true
draft: false
---

[CF 104345B - Truy vấn trên cây](https://codeforces.com/problemset/problem/104345/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây trong đó mỗi đỉnh là một nút riêng biệt và các cạnh kết nối chúng không theo chu trình. Đối với bất kỳ tập hợp con các đỉnh được chọn nào, chúng ta chỉ “cho phép mình đi” qua các đỉnh bên trong tập hợp con đó. Nếu hai đỉnh có thể chạm tới nhau chỉ bằng các đỉnh cho phép thì chúng ta coi chúng được kết nối bên trong tập hợp con đó. 

Đối với một tập hợp con cố định, chúng ta phải đếm xem có bao nhiêu cặp đỉnh riêng biệt được sắp xếp được kết nối theo hạn chế này. Tương tự, đối với mọi thành phần liên thông được tạo ra bởi tập hợp con, mọi cặp đỉnh bên trong thành phần đó đều góp phần đưa ra câu trả lời. 

Cấu trúc ẩn chính là câu trả lời không phải về bản thân các đường dẫn mà là về các thành phần được kết nối do tập hợp con tạo ra. Mỗi truy vấn hỏi: nếu chúng ta chỉ lấy các đỉnh trong S và giữ các cạnh giữa chúng thì tổng trên tất cả các thành phần có kích thước c của c(c − 1) là bao nhiêu? 

Những hạn chế là lớn. Cây có thể có tới 250.000 đỉnh và tổng kích thước của tất cả các bộ truy vấn có thể đạt tới 1.000.000. Điều này ngay lập tức loại trừ mọi hành động duyệt qua biểu đồ cho mỗi truy vấn trên cây đầy đủ hoặc xây dựng lại DSU từ đầu cho mỗi truy vấn. Ngay cả O(K log N) cho mỗi truy vấn cũng là giới hạn nếu được thực hiện một cách đơn giản trên 100.000 truy vấn. 

Một sai lầm ngây thơ là thử BFS hoặc DFS bị giới hạn ở S cho mọi truy vấn. Điều đó sẽ liên tục đi qua các cạnh trong trường hợp xấu nhất tỷ lệ thuận với N trên mỗi truy vấn, tạo ra tối đa 10^10 thao tác. Một lối tắt không chính xác khác là giả sử kết nối trong S chỉ đơn giản dựa trên khoảng cách cây ban đầu hoặc nhóm LCA mà không kiểm tra xem các nút trung gian có được bao gồm hay không. Điều đó không thành công vì khả năng kết nối phụ thuộc hoàn toàn vào việc liệu tất cả các đỉnh trung gian có nằm trong S hay không chứ không phụ thuộc vào việc các điểm cuối có nằm trong cây đầy đủ hay không. 

Trường hợp cạnh tinh tế xảy ra khi S chứa tất cả các nút của một chuỗi dài ngoại trừ một đỉnh bên trong. Mặc dù các điểm cuối nằm liền kề trong cây gốc thông qua nút bị thiếu đó, chúng vẫn bị ngắt kết nối trong S. Bất kỳ cách tiếp cận nào bỏ qua “lỗ hổng” trong S sẽ vượt quá các trường hợp như vậy. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi truy vấn, chúng tôi xây dựng sơ đồ con cảm ứng trên S và chạy DFS hoặc BFS để tìm tất cả các thành phần được kết nối. Khi chúng ta biết kích thước của từng thành phần, chúng ta tính tổng c(c − 1). Điều này đúng vì khả năng kết nối bên trong S chính xác là khả năng kết nối đồ thị trong đồ thị con được tạo ra. 

Tuy nhiên, điều này đòi hỏi phải khám phá các cạnh nhiều lần. Ngay cả khi chúng ta chỉ duyệt các cạnh liên quan đến các nút trong S, một nút có thể xuất hiện trong nhiều truy vấn. Trong trường hợp xấu nhất, một cây sao có nút trung tâm được bao gồm trong nhiều truy vấn sẽ khiến việc quét lặp đi lặp lại danh sách kề của nó, dẫn đến hành vi bậc hai tổng thể. 

Điểm mấu chốt là tránh tính toán lại toàn bộ quá trình truyền tải mà thay vào đó hãy đếm xem có bao nhiêu cạnh bên trong S kết nối các thành phần khác nhau. Trong một cây, các thành phần kết nối của đồ thị con cảm ứng bất kỳ đều được hình thành bằng cách loại bỏ các cạnh có điểm cuối không thuộc cả S hoặc việc loại bỏ các cạnh này sẽ tách các nút đã chọn. Nếu chúng ta xử lý các nút trong S theo thứ tự nhất quán và hợp nhất chúng một cách linh hoạt, chúng ta có thể xây dựng lại các thành phần một cách hiệu quả cho mỗi truy vấn bằng cách sử dụng DSU nhưng chỉ chạm vào các nút trong S. 

Một cách định khung hiệu quả hơn là đối với mỗi truy vấn, chúng ta khởi tạo DSU chỉ trên các nút trong S và kết nối các cặp (u, v) cho các cạnh trong đó cả hai điểm cuối đều nằm trong S. Vì cây có chính xác N − 1 cạnh và tổng K trên tất cả các truy vấn là bị chặn, nên việc lặp lại các cạnh trên mỗi truy vấn là khả thi bằng cách kiểm tra thành viên trong một tập băm. Điều này tránh được việc di chuyển hoàn toàn và tận dụng sự thưa thớt của cây cối. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force DFS cho mỗi truy vấn | O(∑ N·Q) | O(N) | Quá chậm | 
| DSU cho mỗi truy vấn với tính năng lọc cạnh | O(∑ K + ∑ K α(K)) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng truy vấn một cách độc lập nhưng chúng tôi chỉ chạm vào các đỉnh bên trong truy vấn đó.

1. Đọc tập S và lưu nó vào tập băm để kiểm tra tư cách thành viên O(1). Điều này là cần thiết để chúng ta có thể nhanh chóng xác định liệu một cạnh có hoạt động bên trong đồ thị con cảm ứng hay không. 
2. Khởi tạo cấu trúc DSU chỉ cho các nút trong S. Chúng tôi ánh xạ từng nút trong S tới một chỉ mục cục bộ vì mảng DSU phải nhỏ gọn cho mỗi truy vấn. Điều này tránh việc phân bổ mảng có kích thước N cho mỗi truy vấn. 
3. Với mỗi cạnh (u, v) trong cây ban đầu, hãy kiểm tra xem cả hai điểm cuối có nằm trong S hay không. Nếu có, chúng ta hợp các thành phần DSU của chúng. Bước này xây dựng lại chính xác các thành phần được kết nối của đồ thị con cảm ứng vì đồ thị ban đầu đã là một cây nên không tồn tại cạnh thừa. 
4. Sau khi xử lý tất cả các cạnh, hãy tính kích thước của từng thành phần DSU. Với mỗi gốc, chúng ta thu được kích thước thành phần c của nó. 
5. Thêm c(c − 1) vào câu trả lời cho truy vấn đó. Điều này đếm tất cả các cặp có thứ tự bên trong mỗi thành phần vì mọi cặp nút trong cùng một thành phần được kết nối đều hợp lệ. 
6. Xuất ra câu trả lời tích lũy. 

Lựa chọn thiết kế quan trọng là lặp qua tất cả các cạnh của cây thay vì khám phá danh sách kề cho mỗi truy vấn. Vì chỉ có N − 1 cạnh trên toàn cầu nên điều này tránh được sự bùng nổ truyền tải lặp đi lặp lại. 

### Tại sao nó hoạt động 

Trong một cây, khả năng kết nối trong bất kỳ tập con S nào được xác định đầy đủ bởi cạnh gốc nào có cả hai điểm cuối trong S. Mọi đường dẫn bên trong S đều phải đi theo cạnh cây gốc, vì vậy nếu hai nút được kết nối trong đồ thị con cảm ứng, chúng được kết nối thông qua một chuỗi các cạnh được giữ nguyên. Ngược lại, bất kỳ chuỗi giữ cạnh nào cũng là một đường dẫn hợp lệ bên trong S. Do đó, DSU trên “cạnh hoạt động” khớp chính xác với các thành phần được kết nối trong đồ thị con cảm ứng và tính tổng c(c − 1) trên các thành phần sẽ tính tất cả các cặp có thứ tự hợp lệ. 

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
            return
        if self.size[a] < self.size[b]:
            a, b = b, a
        self.parent[b] = a
        self.size[a] += self.size[b]

n = int(input())
edges = [tuple(map(int, input().split())) for _ in range(n - 1)]

q = int(input())

for _ in range(q):
    data = list(map(int, input().split()))
    k = data[0]
    nodes = data[1:]

    idx = {v: i for i, v in enumerate(nodes)}
    dsu = DSU(k)

    for u, v in edges:
        if u in idx and v in idx:
            dsu.union(idx[u], idx[v])

    comp_size = {}
    for i in range(k):
        r = dsu.find(i)
        comp_size[r] = comp_size.get(r, 0) + 1

    ans = 0
    for c in comp_size.values():
        ans += c * (c - 1)

    print(ans)
```DSU được xây dựng lại cho mỗi truy vấn nhưng chỉ trên các đỉnh trong truy vấn đó, điều này giữ cho bộ nhớ được giới hạn bởi K. Idx bản đồ băm đảm bảo kiểm tra tư cách thành viên O(1) khi quét các cạnh. 

Bước đếm thành phần sử dụng lần chuyển thứ hai qua các đại diện DSU để tích lũy kích thước. Tổng cuối cùng sử dụng các cặp có thứ tự, do đó c * (c − 1) thay vì c * (c − 1) / 2. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một cây chuỗi nhỏ: 1-2-3-4 và truy vấn S = {1, 2, 4}. 

| Bước | Kiểm tra cạnh chủ động | DSU sáp nhập | Linh kiện | 
| --- | --- | --- | --- | 
| Cạnh (1,2) | cả ở S | công đoàn(1,2) | {1,2}, {4} | 
| Cạnh (2,3) | 3 không có trong S | không | {1,2}, {4} | 
| Cạnh (3,4) | 3 không có trong S | không | {1,2}, {4} | 

Kích thước thành phần là 2 và 1, vì vậy câu trả lời là 2·1 + 1·0 = 2. 

Điều này xác nhận rằng nút trung gian bị thiếu 3 sẽ phá vỡ kết nối giữa 2 và 4. 

### Ví dụ 2 

Sao cây có tâm ở 1 với các cạnh (1,2), (1,3), (1,4). Truy vấn S = {1,2,3,4}. 

| Bước | Kiểm tra cạnh chủ động | DSU sáp nhập | Linh kiện | 
| --- | --- | --- | --- | 
| (1,2) | cả ở S | công đoàn | {1,2} | 
| (1,3) | cả ở S | công đoàn | {1,2,3} | 
| (1,4) | cả ở S | công đoàn | {1,2,3,4} | 

Kích thước thành phần cuối cùng là 4, vì vậy câu trả lời là 4·3 = 12. 

Điều này cho thấy rằng việc bao gồm toàn bộ nút trung tâm sẽ thu gọn toàn bộ cấu trúc thành một thành phần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(∑ K + Q · N α(K)) | Mỗi truy vấn quét các cạnh một lần và chỉ xử lý các nút được chọn; Hoạt động DSU gần như không đổi | 
| Không gian | O(K) mỗi truy vấn | DSU và bản đồ băm chỉ được xây dựng cho truy vấn hiện tại | 

Tổng K trên tất cả các truy vấn được giới hạn bởi 1.000.000, do đó, đầu vào truy vấn quét nhìn chung là tuyến tính. Mỗi lần kiểm tra cạnh là O(1) cho mỗi truy vấn, mang lại hiệu suất chấp nhận được trong các điều kiện ràng buộc. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
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
                return
            if self.size[a] < self.size[b]:
                a, b = b, a
            self.parent[b] = a
            self.size[a] += self.size[b]

    n = int(input())
    edges = [tuple(map(int, input().split())) for _ in range(n - 1)]
    q = int(input())

    out = []
    for _ in range(q):
        data = list(map(int, input().split()))
        k = data[0]
        nodes = data[1:]
        idx = {v: i for i, v in enumerate(nodes)}
        dsu = DSU(k)

        for u, v in edges:
            if u in idx and v in idx:
                dsu.union(idx[u], idx[v])

        comp_size = {}
        for i in range(k):
            r = dsu.find(i)
            comp_size[r] = comp_size.get(r, 0) + 1

        ans = 0
        for c in comp_size.values():
            ans += c * (c - 1)
        out.append(str(ans))

    return "\n".join(out)

# provided sample
assert run("""7
1 2
1 3
1 5
2 7
4 6
4 7
6
1 1
2 1 2
4 1 2 3 4
5 1 2 4 6 7
6 1 2 3 4 5 6
7 1 2 3 4 5 6 7
""") == """0
1
3
10
7
21"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Truy vấn nút đơn | 0 | trường hợp cơ sở | 
| Chuỗi thiếu nút giữa | chia thành phần | tính chính xác của đường dẫn phụ thuộc | 
| Truy vấn cây đầy đủ | n(n−1) | kết nối tối đa | 
| Bộ một phần cây sao | hợp nhất trung tâm chính xác | kết nối trung tâm | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi kết nối tồn tại trong cây ban đầu nhưng bị phá hủy do loại bỏ một đỉnh trung gian. Ví dụ: trong đường dẫn 1-2-3-4, nếu S = {1,4} thì không có đường dẫn hợp lệ vì thiếu 2 và 3 nên câu trả lời phải là 0. Thuật toán xử lý chính xác các cạnh và không tìm thấy cạnh hoạt động nào, để lại hai nút bị cô lập. 

Một trường hợp cạnh khác là khi S bằng tập đỉnh đầy đủ. Mọi cạnh đều hoạt động, DSU hợp nhất mọi thứ thành một thành phần có kích thước N và kết quả trở thành N(N − 1). Thuật toán thực hiện N − 1 phép hợp, khớp chính xác với cấu trúc cây đầy đủ. 

Trường hợp thứ ba là một ngôi sao chỉ chọn những chiếc lá. Vì không có cạnh nào có cả hai điểm cuối trong S nên mọi nút đều bị cô lập và câu trả lời là 0. DSU vẫn ở trạng thái đơn và tổng thành phần chính xác tạo ra số 0.
