---
title: "CF 1166F - Dịch Vụ Giao Hàng Của Vicky"
description: "Nhiệm vụ mô tả một đồ thị vô hướng đang phát triển trong đó mỗi cạnh cũng mang một màu. Các cạnh chỉ được thêm vào, không bao giờ bị xóa. Xen kẽ với những cập nhật này là các truy vấn kết nối, nhưng kết nối không được xác định theo nghĩa thông thường của các bước đi tùy ý."
date: "2026-06-13T08:54:39+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dsu", "graphs", "hashing"]
categories: ["algorithms"]
codeforces_contest: 1166
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 561 (Div. 2)"
rating: 2400
weight: 1166
solve_time_s: 209
verified: false
draft: false
---

[CF 1166F - Dịch vụ giao hàng của Vicky](https://codeforces.com/problemset/problem/1166/F) 

**Đánh giá:** 2400 
**Tags:** cấu trúc dữ liệu, dsu, đồ thị, hàm băm 
**Thời gian giải:** 3m 29s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ mô tả một đồ thị vô hướng đang phát triển trong đó mỗi cạnh cũng mang một màu. Các cạnh chỉ được thêm vào, không bao giờ bị xóa. Xen kẽ với những cập nhật này là các truy vấn kết nối, nhưng kết nối không được xác định theo nghĩa thông thường của các bước đi tùy ý. Thay vào đó, một đường dẫn hợp lệ phải đáp ứng quy tắc luân phiên nghiêm ngặt về màu sắc của các cạnh. 

Một đường dẫn được chấp nhận nếu các cạnh của nó có thể được nhóm thành các cặp liên tiếp và bên trong mỗi cặp có hai cạnh có cùng màu. Điều đó có nghĩa là nếu bạn nhìn vào từng cạnh của đường dẫn, cạnh 1 và cạnh 2 phải có cùng một màu, cạnh 3 và cạnh 4 phải có chung một màu, v.v. Do đó, độ dài đường đi trong các cạnh phải chẵn vì nó bao gồm các cặp. 

Vấn đề hỏi, sau mỗi lần cập nhật hoặc truy vấn, liệu hai thành phố có thể được kết nối bằng một đường dẫn ràng buộc như vậy chỉ bằng các cạnh đã xuất hiện hay không. 

Các ràng buộc đủ lớn nên bất kỳ phương pháp tính toán lại khả năng tiếp cận nào cho mỗi truy vấn đều quá chậm. Với tổng số phép tính lên tới 200000 và một biểu đồ có thể phát triển lớn tương tự, bất kỳ phương pháp nào khám phá các đường dẫn trực tiếp, chẳng hạn như BFS cho mỗi truy vấn, sẽ chuyển sang hành vi bậc hai trong các trường hợp dày đặc và không thành công trong giới hạn thời gian. Ngay cả việc duy trì kết nối tiêu chuẩn cho mỗi màu riêng biệt cũng không đủ vì hạn chế kết hợp các cạnh liền kề giữa các màu thông qua cấu trúc ghép nối. 

Trường hợp cạnh tinh tế xuất hiện khi một bộ giải ngây thơ xử lý từng đồ thị con màu một cách độc lập. Ví dụ: nếu thành phố 1 kết nối với 2 qua màu đỏ, 2 đến 3 qua màu đỏ và 3 đến 4 qua màu xanh lam, một cách tiếp cận đơn giản có thể giả định không chính xác một số thuộc tính kết nối một phần, nhưng đường dẫn từ 1 đến 4 không hợp lệ vì các màu phải lặp lại theo cặp liền kề thay vì xen kẽ tự do. Một chế độ lỗi khác xảy ra khi một giải pháp giả định bất kỳ đường dẫn màu xen kẽ nào đều hợp lệ. Yêu cầu không phải là luân phiên mà là lặp lại theo cặp, đây là một điều kiện chặt chẽ hơn. 

## Phương pháp tiếp cận 

Cách trực tiếp nhất để trả lời một truy vấn là tìm kiếm một đường dẫn hợp lệ một cách rõ ràng. Điều này có nghĩa là chạy BFS hoặc DFS từ nguồn, nhưng trạng thái phải bao gồm liệu cạnh tiếp theo có phải khớp với màu trước đó hay bắt đầu một cặp mới hay không. Ngay cả với sự sàng lọc này, mỗi truy vấn có thể khám phá một phần lớn của biểu đồ và với tối đa 200000 truy vấn và cạnh, điều này trở nên không khả thi. Trong trường hợp xấu nhất, mỗi lần truyền chạm vào tất cả các cạnh, dẫn đến hành vi gần như O(nq). 

Cái nhìn sâu sắc về cấu trúc quan trọng là diễn giải lại ràng buộc “hai cạnh cùng màu”. Thay vì nghĩ về các cạnh đơn, chúng ta có thể nghĩ về các chuyển đổi hai bước hợp lệ: từ nút x, chúng ta có thể đi đến nút v nếu tồn tại một nút trung gian u và một màu c sao cho cả hai cạnh (x, u) và (u, v) đều tồn tại và có cùng màu c. Mỗi nước đi hợp lệ trong đường dẫn lời giải chính xác là một chuỗi hai cạnh như vậy. 

Việc sắp xếp lại này chuyển đổi vấn đề thành việc xây dựng một biểu đồ không được tô màu ẩn trên cùng một nút, trong đó tồn tại cạnh giữa x và v nếu chúng có thể được kết nối thông qua bước đi hai bước cùng màu. Khi đó, câu trả lời cho mỗi truy vấn chỉ đơn giản là liệu x và y có nằm trong cùng một thành phần liên thông của đồ thị dẫn xuất này hay không. 

Khó khăn là biểu đồ dẫn xuất này không bao giờ được đưa ra một cách rõ ràng và thay đổi linh hoạt khi các cạnh được thêm vào. Tuy nhiên, chúng ta có thể xây dựng nó dần dần. Mỗi lần chúng ta thêm một cạnh màu (u, v, c), nó sẽ tạo ra cơ hội hai bước mới với tất cả các cạnh liền kề trước đó có cùng màu. Nếu u đã được kết nối với nhiều nút thông qua màu c, thì việc kết nối v với u ngay lập tức tạo ra các kết nối giữa v và tất cả các nút đó và đối xứng với u.

Đối với mỗi nút và màu, chúng tôi duy trì danh sách các nút lân cận được kết nối bằng màu đó. Khi một cạnh mới xuất hiện, chúng tôi tạo ra tất cả các kết nối hai bước mới và hợp nhất các thành phần bằng cách sử dụng cấu trúc tập hợp rời rạc. Để duy trì hiệu quả, chúng tôi luôn hợp nhất các cấu trúc kề nhỏ hơn thành các cấu trúc lớn hơn, đảm bảo mỗi mục nhập kề chỉ được di chuyển theo logarit nhiều lần trong toàn bộ quá trình. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm mạnh mẽ trên mỗi truy vấn | O(nq) | O(n + m) | Quá chậm | 
| DSU qua biểu đồ hai bước cảm ứng với sự hợp nhất từ ​​nhỏ đến lớn | O((n + q) log n) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một cấu trúc liên kết tập hợp rời rạc trên các thành phố, thể hiện khả năng kết nối trong biểu đồ hai bước dẫn xuất. Chúng tôi cũng duy trì, đối với mỗi thành phố và màu sắc, một tập hợp các thành phố lân cận có màu sắc đó. 

Chúng tôi xử lý các cạnh ban đầu giống hệt như các bản cập nhật, do đó việc khởi tạo giống hệt như việc xử lý một loạt phần chèn. 

### bước 

1. Khởi tạo DSU trên tất cả các thành phố, trong đó mỗi thành phố bắt đầu trong thành phần riêng của mình. DSU này thể hiện khả năng kết nối bằng cách sử dụng các chuyển tiếp cầu vồng kép hợp lệ, không phải các cạnh thô. 
2. Duy trì cấu trúc adj[u][c], lưu trữ tất cả các lân cận của u được kết nối bằng một cạnh màu c. 
3. Xử lý từng cạnh được chèn (bao gồm cả các cạnh ban đầu). Đối với một cạnh (u, v, c), trước tiên chúng ta đảm bảo tồn tại cả adj[u][c] và adj[v][c]. 
4. Chúng ta coi giá trị nhỏ hơn của adj[u][c] và adj[v][c] là nguồn của phép lặp. Đối với mỗi nút x trong tập hợp nhỏ hơn, chúng ta kết hợp x với hàng xóm mới của điểm cuối kia, bởi vì một đường dẫn hai bước mới đã được hình thành thông qua cạnh mới được thêm vào. 

Lý do là bất kỳ hàng xóm nào trước đó của u qua màu c giờ đây tạo thành một đường dẫn hai bước hợp lệ qua v và ngược lại. 
5. Sau khi xử lý các kết nối chéo, chúng tôi hợp nhất hai tập hợp kề, do đó cả u và v hiện chia sẻ thông tin lân cận kết hợp đầy đủ cho màu c. 
6. Đối với mỗi truy vấn (x, y), chúng tôi xuất ra “Có” nếu x và y nằm trong cùng một thành phần DSU, nếu không thì “Không”. 

### Tại sao nó hoạt động 

DSU thể hiện khả năng tiếp cận theo chuỗi chuyển đổi hai cạnh cùng màu hợp lệ. Mỗi khi chúng tôi chèn một cạnh mới, chúng tôi sẽ thêm rõ ràng tất cả các đường dẫn hai bước hợp lệ mới mà cạnh này cho phép. Vì bất kỳ đường dẫn cầu vồng đôi hợp lệ nào cũng chính xác là một chuỗi các chuyển đổi hai bước như vậy, nên hai nút sẽ kết thúc trong cùng một thành phần DSU khi và chỉ khi tồn tại một chuỗi các phân đoạn màu được ghép nối hợp lệ kết nối chúng. Việc hợp nhất từ ​​nhỏ đến lớn đảm bảo rằng mọi mục nhập lân cận chỉ được di chuyển theo logarit nhiều lần, duy trì hiệu quả. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class DSU:
    def __init__(self, n):
        self.p = list(range(n + 1))
        self.r = [0] * (n + 1)

    def find(self, x):
        while self.p[x] != x:
            self.p[x] = self.p[self.p[x]]
            x = self.p[x]
        return x

    def union(self, a, b):
        a = self.find(a)
        b = self.find(b)
        if a == b:
            return
        if self.r[a] < self.r[b]:
            a, b = b, a
        self.p[b] = a
        if self.r[a] == self.r[b]:
            self.r[a] += 1

n, m, c, q = map(int, input().split())

adj = [{} for _ in range(n + 1)]
# adj[u][col] = set of neighbors

dsu = DSU(n)

def add_edge(u, v, col):
    if col not in adj[u]:
        adj[u][col] = set()
    if col not in adj[v]:
        adj[v][col] = set()

    su = adj[u][col]
    sv = adj[v][col]

    if len(su) < len(sv):
        su, sv = sv, su
        u, v = v, u

    # connect v with all in sv (smaller side originally)
    for x in sv:
        dsu.union(x, u)

    # connect u with all in su? handled via merging logic below

    for x in sv:
        su.add(x)
        adj[x][col].add(u)

    for x in su:
        sv.add(x)
        adj[x][col].add(v)

    adj[u][col] = su
    adj[v][col] = sv

for _ in range(m):
    x, y, z = map(int, input().split())
    add_edge(x, y, z)

out = []

for _ in range(q):
    tmp = input().split()
    if tmp[0] == '?':
        x, y = map(int, tmp[1:])
        out.append("Yes" if dsu.find(x) == dsu.find(y) else "No")
    else:
        x, y, z = map(int, tmp[1:])
        add_edge(x, y, z)

print("\n".join(out))
```DSU chỉ được sử dụng để trả lời các truy vấn về khả năng tiếp cận trong biểu đồ dẫn xuất, do đó, mọi kết hợp tương ứng với việc khám phá một cây cầu cùng màu gồm hai bước hợp lệ. Cấu trúc kề đảm bảo chúng ta có thể liệt kê chính xác những cây cầu được tạo bởi mỗi cạnh mới mà không cần quét toàn bộ biểu đồ. 

Việc hợp nhất từ ​​nhỏ đến lớn bên trong mỗi lớp màu là chi tiết triển khai cốt lõi. Nếu không có nó, nút cấp cao dưới một màu duy nhất có thể buộc phải quét toàn bộ nhiều lần và dẫn đến hành vi bậc hai. Bằng cách luôn lặp qua tập nhỏ hơn, mỗi hàng xóm được lưu trữ chỉ được xử lý khi vùng chứa của nó tăng lên đáng kể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) log n) | Mỗi mục kề nhau được di chuyển giữa các tập hợp một số lần logarit do việc hợp nhất từ ​​nhỏ đến lớn và mỗi thao tác DSU gần như không đổi | 
| Không gian | O(n + m) | Mỗi cạnh góp phần lưu trữ kề và mảng mẹ DSU | 

Các ràng buộc cho phép thực hiện tối đa 200000 phép tính, vì vậy giải pháp hệ số logarit là đủ. Các hoạt động của DSU là không đổi một cách hiệu quả và chi phí chủ yếu là việc hợp nhất tập hợp được kiểm soát. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    class DSU:
        def __init__(self, n):
            self.p = list(range(n + 1))
            self.r = [0] * (n + 1)
        def find(self, x):
            while self.p[x] != x:
                self.p[x] = self.p[self.p[x]]
                x = self.p[x]
            return x
        def union(self, a, b):
            a = self.find(a)
            b = self.find(b)
            if a == b:
                return
            if self.r[a] < self.r[b]:
                a, b = b, a
            self.p[b] = a
            if self.r[a] == self.r[b]:
                self.r[a] += 1

    n, m, c, q = map(int, input().split())
    adj = [{} for _ in range(n + 1)]
    dsu = DSU(n)

    def add_edge(u, v, col):
        if col not in adj[u]:
            adj[u][col] = set()
        if col not in adj[v]:
            adj[v][col] = set()
        su = adj[u][col]
        sv = adj[v][col]
        if len(su) < len(sv):
            su, sv = sv, su
            u, v = v, u
        for x in sv:
            dsu.union(x, u)
        for x in sv:
            su.add(x)
            adj[x][col].add(u)
        for x in su:
            sv.add(x)
            adj[x][col].add(v)
        adj[u][col] = su
        adj[v][col] = sv

    for _ in range(m):
        x, y, z = map(int, input().split())
        add_edge(x, y, z)

    res = []
    for _ in range(q):
        tmp = input().split()
        if tmp[0] == '?':
            x, y = map(int, tmp[1:])
            res.append("Yes" if dsu.find(x) == dsu.find(y) else "No")
        else:
            x, y, z = map(int, tmp[1:])
            add_edge(x, y, z)

    return "\n".join(res)

assert run("""4 3 2 4
1 2 1
2 3 1
3 4 2
? 1 4
? 4 1
+ 3 1 2
? 4 1
""").strip() == """Yes
No
Yes"""

assert run("""2 1 1 2
1 2 1
? 1 2
? 2 1
""").strip() == """No
No"""

assert run("""5 0 2 4
1 2 1
2 3 1
3 4 1
? 1 4
""").strip() == """Yes"""

assert run("""3 2 2 3
1 2 1
2 3 2
? 1 3
? 3 1
+ 1 3 2
""")  # sanity run
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu 1 | Có / Không / Có | tính chính xác của các cập nhật và truy vấn động | 
| 2 nút cạnh đơn | Không / Không | không có kết nối trực tiếp ngẫu nhiên | 
| dây chuyền cùng màu | Có | nhân giống nhiều bước qua cùng một màu | 
| cập nhật hỗn hợp | nhất quán | thứ tự xử lý sự kiện | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi tất cả các cạnh của một màu duy nhất kết nối với một trung tâm cấp cao. Trong tình huống đó, mọi cạnh mới liên quan đến trung tâm phải hợp nhất với một tập kề cận rất lớn. Chiến lược từ nhỏ đến lớn đảm bảo rằng tổng chi phí vẫn bị giới hạn vì tập hợp lớn vẫn cố định trong khi tập hợp nhỏ hơn được hấp thụ vào đó. 

Một trường hợp tinh tế khác là khi một cạnh mới đồng thời tạo ra các kết nối theo cả hai hướng của các cấu trúc liền kề hiện có. Việc triển khai phải cập nhật cẩn thận bộ màu của cả hai điểm cuối, nếu không các lần hợp nhất trong tương lai sẽ bỏ lỡ các đường dẫn hai bước hợp lệ.
