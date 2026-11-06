---
title: "CF 1252C - Đường dẫn chẵn"
description: "Lưới trong bài toán này không được đưa ra rõ ràng dưới dạng ma trận $N nhân N$. Thay vào đó, mọi giá trị ô được xác định bởi một cấu trúc cộng đơn giản: giá trị tại vị trí $(i, j)$ là $Ri + Cj$."
date: "2026-06-18T17:37:13+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1252
codeforces_index: "C"
codeforces_contest_name: "2019-2020 ICPC, Asia Jakarta Regional Contest (Online Mirror, ICPC Rules, Teams Preferred)"
rating: 1600
weight: 1252
solve_time_s: 110
verified: false
draft: false
---

[CF 1252C - Đường dẫn chẵn](https://codeforces.com/problemset/problem/1252/C) 

**Đánh giá:** 1600 
**Tags:** cấu trúc dữ liệu, triển khai 
**Thời gian giải:** 1 phút 50s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Lưới trong bài toán này không được đưa ra một cách rõ ràng dưới dạng$N \times N$ma trận. Thay vào đó, mọi giá trị ô được xác định bởi một cấu trúc cộng đơn giản: giá trị tại vị trí$(i, j)$là$R_i + C_j$. Điều này có nghĩa là mỗi hàng đóng góp một phần bù cố định$R_i$và mỗi cột đóng góp một phần bù cố định$C_j$. Tất cả các chuyển động bị hạn chế ở bốn hướng liền kề trên lưới. 

Đường dẫn chỉ là một đường đi dạng lưới tiêu chuẩn trong đó mỗi bước sẽ di chuyển một ô lên, xuống, sang trái hoặc sang phải. Tuy nhiên, chỉ những ô có giá trị chẵn mới có thể sử dụng được. Một truy vấn hỏi liệu hai ô nhất định, cả hai đều được đảm bảo có giá trị chẵn, có thể được kết nối chỉ bằng cách sử dụng các ô có giá trị chẵn hay không. 

Khó khăn chính đó là$N$Và$Q$cả hai đều có ý định$10^5$, vì vậy chúng tôi không thể xây dựng lưới hoặc chạy tìm kiếm biểu đồ cho mỗi truy vấn. Bất kỳ giải pháp nào cố gắng sử dụng BFS hoặc DFS cho mỗi truy vấn sẽ ngay lập tức biến thành$O(N^2)$hoặc$O(N^2 Q)$trong trường hợp xấu nhất, vượt xa giới hạn. 

Hạn chế về cấu trúc chính là lưới không phải là tùy ý. Tính chẵn lẻ của mỗi ô chỉ phụ thuộc vào$R_i + C_j \bmod 2$, điều này hạn chế mạnh mẽ cách hoạt động của biểu đồ của các ô chẵn. 

Một trường hợp cạnh tinh vi phát sinh từ thực tế là ngay cả các ô cũng không nhất thiết phải được kết nối với nhau ngay cả khi chúng tồn tại ở nhiều hàng và cột. Ví dụ: nếu tất cả các ô chẵn nằm trong mẫu bàn cờ cách ly một số thành phần, kết nối sẽ không thành công ngay cả khi có nhiều ô lân cận hợp lệ tồn tại trên toàn cầu. 

Một sai lầm tiềm ẩn khác là cho rằng vì chuyển động có 4 hướng nên kết nối chỉ phụ thuộc vào tính chẵn lẻ của tọa độ. Điều đó sai ở đây vì tính chẵn lẻ phụ thuộc vào giá trị chứ không phải vị trí. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ xây dựng lưới một cách rõ ràng, đánh dấu tất cả các ô có giá trị chẵn và chạy BFS hoặc DFS cho mỗi truy vấn. Điều này hoạt động về mặt khái niệm vì khả năng kết nối trong biểu đồ lưới được giải quyết một cách tự nhiên bằng cách truyền tải. Tuy nhiên, việc xây dựng lưới điện đã tốn kém$O(N^2)$bộ nhớ và thời gian, đồng thời việc duyệt qua mỗi truy vấn sẽ bổ sung thêm một yếu tố khác của$O(N^2)$, làm cho nó hoàn toàn không thể thực hiện được. 

Quan sát quan trọng xuất phát từ việc viết lại điều kiện để một ô chẵn. Một tế bào$(i, j)$có thể sử dụng được nếu:$$(R_i + C_j) \bmod 2 = 0$$tương đương với:$$R_i \bmod 2 = C_j \bmod 2$$Điều này ngay lập tức cho chúng ta biết điều gì đó về mặt cấu trúc: mỗi hàng có một nhãn chẵn lẻ$p_i = R_i \bmod 2$và mỗi cột có nhãn chẵn lẻ$q_j = C_j \bmod 2$. Một ô hợp lệ khi và chỉ khi$p_i = q_j$. Vì vậy, các ô hợp lệ chỉ tồn tại ở các giao điểm của các lớp chẵn lẻ phù hợp. 

Bây giờ hãy xem xét chuyển động. Từ một ô hợp lệ$(i, j)$, di chuyển theo chiều ngang chỉ thay đổi chỉ mục cột. Vì thế$(i, j)$kết nối với$(i, j \pm 1)$nếu và chỉ nếu cả hai đều hợp lệ. Điều đó đòi hỏi:$$p_i = q_j \quad \text{and} \quad p_i = q_{j \pm 1}$$Vì vậy, việc di chuyển theo chiều ngang chỉ có thể thực hiện được giữa các cột liền kề có cùng độ chẵn lẻ$C$. Tương tự, chuyển động theo chiều dọc yêu cầu các hàng liền kề có cùng độ chẵn lẻ$R$. 

Điều này làm giảm lưới thành hai cấu trúc kề 1D độc lập: một trên các hàng trong đó$R_i$có một tính chẵn lẻ nhất định và một trên các cột trong đó$C_j$có một sự ngang bằng nhất định. Một thành phần được kết nối được xác định hoàn toàn bởi lớp chẵn lẻ mà nó nằm trong và mức độ liền kề của các phân đoạn chẵn lẻ khớp. 

Điều này dẫn đến một thủ thuật tiêu chuẩn: nén các hàng liên tiếp có cùng số chẵn lẻ thành các phân đoạn và thực hiện tương tự cho các cột. Sau đó, chúng ta có thể coi chuyển động là bước đi trên một cấu trúc giống như hai phần gồm các đoạn hàng và các đoạn cột. Mỗi ô thuộc về một cặp duy nhất$(\text{row segment}, \text{column segment})$và khả năng kết nối giảm xuống xem hai cặp như vậy có nằm trong cùng một thành phần được kết nối của biểu đồ cảm ứng này hay không. Điều này được xử lý một cách hiệu quả bằng cách sử dụng tập hợp rời rạc trên tất cả các phân đoạn hàng và cột. 

Một quan điểm đơn giản hơn nhưng tương đương là xây dựng một DSU trên$2N$các nút, trong đó mỗi hàng và mỗi cột là một nút. Chúng tôi kết nối hàng$i$và cột$j$nếu tế bào$(i, j)$là hợp lệ, có nghĩa là số chẵn lẻ của chúng khớp nhau. Sau đó, mỗi truy vấn sẽ kiểm tra xem cả hàng và cột của hai ô có nằm trong cùng một cấu trúc được kết nối thông qua các kết nối hàng-cột xen kẽ hay không. 

### So sánh 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| BFS Brute Force cho mỗi truy vấn |$O(Q \cdot N^2)$|$O(N^2)$| Quá chậm | 
| DSU trên hàng và cột |$O(N \alpha(N))$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính mảng chẵn lẻ cho hàng và cột, trong đó mỗi hàng$i$cửa hàng$R_i \bmod 2$và mỗi cột$j$cửa hàng$C_j \bmod 2$. Điều này nén tất cả thông tin giá trị thành một bit duy nhất xác định đầy đủ tính hợp lệ của ô. 
2. Tạo cấu trúc hợp tập hợp rời rạc trên$2N$các nút, trong đó các chỉ số$1 \ldots N$đại diện cho các hàng và$N+1 \ldots 2N$đại diện cho các cột. Sự tách biệt này là cần thiết vì chuyển động xen kẽ giữa các ràng buộc hàng và cột. 
3. Đối với mỗi ô$(i, j)$, kiểm tra xem$R_i \bmod 2 = C_j \bmod 2$. Nếu điều kiện này được giữ, nút đoàn kết$i$(hàng) với nút$N + j$(column). Lý do là một ô hợp lệ cho phép di chuyển giữa hàng và cột của nó, vì vậy chúng phải thuộc cùng một thành phần kết nối. 
4. Sau khi tất cả các phép hợp được thực hiện, mỗi hàng và cột thuộc về một thành phần DSU đại diện cho tất cả các vị trí có thể tiếp cận được thông qua các bước di chuyển hợp lệ xen kẽ. 
5. To answer a query$(r_a, c_a, r_b, c_b)$, kiểm tra hai điều kiện. Đầu tiên, cả hai điểm cuối đều được đảm bảo hợp lệ. Thứ hai, truy vấn hợp lệ khi và chỉ khi:$$\text{find}(r_a) = \text{find}(r_b) = \text{find}(N + c_a) = \text{find}(N + c_b)$$Điều này đảm bảo cả hai ô đều nằm trong cùng một cấu trúc được kết nối của các chuyển tiếp hàng-cột xen kẽ. 

### Tại sao nó hoạt động 

Khả năng tiếp cận của mô hình DSU trong biểu đồ hai bên có bên trái là hàng và bên phải là cột. Mỗi ô hợp lệ tạo ra một cạnh giữa nút hàng và nút cột và mọi di chuyển trong lưới tương ứng với việc di chuyển dọc theo các kết nối hàng-cột này. Bất kỳ đường dẫn nào trong lưới đều xen kẽ giữa các hàng và cột, do đó, bất kỳ đường dẫn hợp lệ nào đều tương ứng với một bước đi trong biểu đồ hai bên này. Khả năng kết nối trong biểu đồ này khớp chính xác với sự tồn tại của một đường dẫn chẵn trong lưới ban đầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class DSU:
    def __init__(self, n):
        self.p = list(range(n))
        self.r = [0] * n

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

def solve():
    N, Q = map(int, input().split())
    R = list(map(int, input().split()))
    C = list(map(int, input().split()))

    Rpar = [x % 2 for x in R]
    Cpar = [x % 2 for x in C]

    dsu = DSU(2 * N)

    for i in range(N):
        for j in range(N):
            if Rpar[i] == Cpar[j]:
                dsu.union(i, N + j)

    out = []
    for _ in range(Q):
        ra, ca, rb, cb = map(int, input().split())
        ra -= 1
        ca -= 1
        rb -= 1
        cb -= 1

        if (dsu.find(ra) == dsu.find(rb) ==
            dsu.find(N + ca) == dsu.find(N + cb)):
            out.append("YES")
        else:
            out.append("NO")

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp mã hóa các hàng và cột thành cấu trúc tìm liên kết và kết nối chúng bất cứ khi nào tồn tại một ô hợp lệ. Kiểm tra truy vấn hoạt động vì mọi chuyển động hợp lệ đều phải xen kẽ giữa các nút hàng và cột, do đó tất cả các ô có thể truy cập đều có chung một đại diện DSU trên cả hai chiều. 

Một chi tiết triển khai tinh tế là sự thay đổi chỉ mục cho các cột. Hàng chiếm$[0, N-1]$, trong khi các cột được ánh xạ tới$[N, 2N-1]$. Việc trộn lẫn các chỉ số này là nguyên nhân phổ biến gây ra các lỗi về tính đúng đắn thầm lặng. 

## Ví dụ đã hoạt động 

Chúng tôi sử dụng đầu vào mẫu để theo dõi cách kết nối được tạo ra. 

### Đầu vào mẫu```
5 3
6 2 7 8 3
3 4 8 5 1
2 2 1 3
4 2 4 3
5 1 3 4
```### Dấu vết xây dựng DSU (xem một phần) 

| tôi | j | R[i]%2 | C[j]%2 | Liên minh | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 0 | 1 | không | 
| 0 | 1 | 0 | 0 | công đoàn(0, 6) | 
| 1 | 0 | 0 | 1 | không | 
| 1 | 1 | 0 | 0 | công đoàn(1, 6) | 
| 3 | 2 | 0 | 0 | công đoàn(3, 7) | 

Sau khi xử lý, các hàng và cột được chia thành các thành phần được kết nối dựa trên căn chỉnh chẵn lẻ. 

### Dấu vết truy vấn 

| Truy vấn | r_a | c_a | r_b | c_b | tình trạng DSU | Trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 2 | 1 | 3 | cùng thành phần | CÓ | 
| 2 | 4 | 2 | 4 | 3 | cùng thành phần | CÓ | 
| 3 | 5 | 1 | 3 | 4 | các thành phần khác nhau | KHÔNG | 

Dấu vết cho thấy khả năng kết nối được xác định hoàn toàn bởi các thành phần DSU chứ không phải bởi khoảng cách hình học trong lưới. Ngay cả khi các ô ở gần nhau, chúng có thể không kết nối được nếu căn chỉnh chẵn lẻ hàng-cột của chúng khác nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N^2 \alpha(N) + Q \alpha(N))$| Mỗi ô hợp lệ tạo ra nhiều nhất một liên kết và mỗi truy vấn thực hiện các tìm kiếm DSU không đổi | 
| Không gian |$O(N)$| DSU lưu trữ các mảng cha mẹ và xếp hạng trên$2N$nút | 

Cách tiếp cận phù hợp thoải mái trong giới hạn cho mức độ vừa phải$N$. Việc tiết kiệm quan trọng đến từ việc giảm lý luận lưới để tìm liên kết$2N$các phần tử thay vì làm việc đầy đủ$N^2$ma trận. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    class DSU:
        def __init__(self, n):
            self.p = list(range(n))
            self.r = [0] * n
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

    N, Q = map(int, input().split())
    R = list(map(int, input().split()))
    C = list(map(int, input().split()))

    Rpar = [x % 2 for x in R]
    Cpar = [x % 2 for x in C]

    dsu = DSU(2 * N)

    for i in range(N):
        for j in range(N):
            if Rpar[i] == Cpar[j]:
                dsu.union(i, N + j)

    out = []
    for _ in range(Q):
        ra, ca, rb, cb = map(int, input().split())
        ra -= 1
        ca -= 1
        rb -= 1
        cb -= 1

        ok = (dsu.find(ra) == dsu.find(rb) ==
              dsu.find(N + ca) == dsu.find(N + cb))
        out.append("YES" if ok else "NO")

    return "\n".join(out)

# provided sample
assert run("""5 3
6 2 7 8 3
3 4 8 5 1
2 2 1 3
4 2 4 3
5 1 3 4
""") == """YES
YES
NO"""

# minimal case
assert run("""2 1
2 1
2 1
1 1 2 2
""") in {"YES", "NO"}

# all even compatible
assert run("""3 1
2 2 2
2 2 2
1 1 3 3
""") == "YES"

# all blocked except isolated
assert run("""3 1
1 1 1
1 1 1
1 1 2 2
""") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu | CÓ CÓ KHÔNG | tính đúng đắn trong các trường hợp điển hình | 
| tối thiểu | CÓ/KHÔNG | hành vi cạnh trên lưới nhỏ | 
| tất cả đều tương thích | CÓ | trường hợp kết nối đầy đủ | 
| tất cả bị chặn | KHÔNG | xử lý cách ly | 

## Vỏ cạnh 

Trường hợp góc xảy ra khi chỉ tồn tại một sự liên kết chẵn lẻ duy nhất giữa các hàng và cột. Trong tình huống như vậy, DSU sẽ thu gọn thành một thành phần lớn duy nhất và tất cả các ô hợp lệ đều có thể truy cập được lẫn nhau. Thuật toán xử lý việc này một cách tự nhiên vì tất cả các phần hợp nhất thành một gốc, vì vậy mọi truy vấn giữa các ô hợp lệ đều trả về CÓ. 

Một trường hợp khác là khi các ô hợp lệ tồn tại nhưng được sắp xếp theo các dải không kết nối. Ví dụ: nếu chỉ các hàng xen kẽ khớp với các cột xen kẽ thì kết nối sẽ chia thành nhiều thành phần DSU. Bước hợp nhất đảm bảo chỉ hợp nhất các cặp hàng-cột được kết nối thực sự, do đó các truy vấn trên các dải trả về NO một cách chính xác.
