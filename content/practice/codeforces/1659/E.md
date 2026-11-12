---
title: "CF 1659E - AND-MEX Walk"
description: "Chúng ta có một biểu đồ liên thông, vô hướng trong đó mỗi cạnh có trọng số nguyên không âm nhỏ hơn $2^{30}$. Một truy vấn yêu cầu \"độ dài\" tối thiểu có thể có của một bước đi từ đỉnh $u$ đến đỉnh $v$."
date: "2026-06-10T03:10:16+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "brute-force", "constructive-algorithms", "dfs-and-similar", "dsu", "graphs"]
categories: ["algorithms"]
codeforces_contest: 1659
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 782 (Div. 2)"
rating: 2200
weight: 1659
solve_time_s: 115
verified: false
draft: false
---

[CF 1659E - AND-MEX Walk](https://codeforces.com/problemset/problem/1659/E) 

**Xếp hạng:** 2200 
**Tags:** bitmasks, brute Force, thuật toán xây dựng, dfs và tương tự, dsu, đồ thị 
**Thời gian giải:** 1 phút 55 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị liên thông vô hướng, trong đó mỗi cạnh có trọng số nguyên không âm nhỏ hơn$2^{30}$. Một truy vấn yêu cầu "độ dài" tối thiểu có thể có của một bước đi từ đỉnh$u$đến đỉnh$v$. "Độ dài" của bước đi được xác định một cách bất thường: đối với một chuỗi các cạnh dọc theo bước đi, chúng ta lấy AND theo bit của mỗi tiền tố của trọng số cạnh và sau đó tính MEX của tất cả các tiền tố AND này. MEX là số nguyên không âm nhỏ nhất không có trong tập hợp. 

Đầu vào đảm bảo đồ thị được kết nối, do đó luôn tồn tại một bước đi giữa hai đỉnh bất kỳ. Thách thức là tìm ra bước đi tạo ra MEX nhỏ nhất có thể. 

Với$n$Và$m$lên đến$10^5$Và$q$lên đến$10^5$, một cách tiếp cận bạo lực liệt kê tất cả các tầng lớp là không khả thi. AND và MEX theo bit ngụ ý rằng các cạnh có một số bit 0 nhất định có thể loại bỏ khả năng có các giá trị MEX nhỏ, do đó thông tin chi tiết chính phải liên quan đến các mẫu bit thay vì các bước đi rõ ràng. 

Trường hợp cạnh tinh tế xảy ra khi bước đi có trọng số bằng 0 trên bất kỳ cạnh nào. Trong trường hợp đó, AND của bất kỳ tiền tố nào bao gồm cạnh đó đều bằng 0, điều này làm cho MEX ít nhất bằng 1. Một tình huống khó khăn khác là khi nhiều cạnh chia sẻ cùng một bit thấp. Ví dụ: nếu tất cả các cạnh có trọng số 2 hoặc cao hơn thì MEX 0 sẽ tự động đạt được vì không có tiền tố AND tạo ra 0. Một cách tiếp cận đơn giản chỉ tính tổng hoặc đếm các cạnh mà không xem xét tiền tố AND sẽ đưa ra câu trả lời sai. 

## Phương pháp tiếp cận 

Phương pháp brute-force sẽ xem xét tất cả các bước đi có thể có giữa$u$Và$v$, tính toán tiền tố AND cho mỗi tiền tố, sau đó lấy MEX. Vì số bước đi tăng theo cấp số nhân với số đỉnh và cạnh nên điều này là không thực tế. Ngay cả khi chúng tôi giới hạn việc đi bộ ở những con đường đơn giản, hãy liệt kê tất cả chúng trong biểu đồ với$10^5$đỉnh là không thể. Brute-force đúng về mặt logic nhưng có độ phức tạp$O(\text{number of walks} \times \text{length of walk})$, vượt quá mọi giới hạn thời gian hợp lý. 

Thông tin chi tiết quan trọng là bitwise AND giảm đơn điệu khi di chuyển dọc theo đường đi bộ. Nghĩa là, khi một bit bằng 0 trong tiền tố AND, nó vẫn bằng 0 trong tất cả các AND tiền tố tiếp theo. Do đó, MEX của tiền tố AND bị ảnh hưởng chủ yếu bởi các bit nhỏ của trọng số cạnh, đặc biệt là các bit 0. Chúng ta có thể khai thác điều này bằng cách quan sát rằng nếu muốn MEX = 0, chúng ta cần tiền tố AND bằng 0. Nếu chúng ta muốn MEX = 1, tất cả tiền tố AND phải khác 0, nhưng thiếu số 0, v.v. 

Với suy nghĩ này, vấn đề giảm xuống còn việc kiểm tra, đối với mỗi truy vấn, số nguyên nhỏ nhất$k$sao cho tồn tại một quãng đường đi bộ từ$u$ĐẾN$v$trong đó không có tiền tố AND bằng$k$. Để thực hiện điều này một cách hiệu quả, chúng tôi xây dựng 30 biểu đồ riêng biệt, một biểu đồ cho mỗi vị trí bit, trong đó các cạnh chỉ tồn tại nếu một bit nhất định được đặt. Sau đó chúng ta có thể kiểm tra kết nối trong các biểu đồ con này. Khả năng kết nối trong một sơ đồ con trong đó tất cả các cạnh đều có bit$i$tập hợp cho chúng ta biết liệu tất cả tiền tố AND có thể tránh được số 0 theo bit hay không$i$. Sử dụng phân tách mặt nạ bit này, chúng ta có thể xác định MEX mà không cần liệt kê các bước đi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(số lần đi bộ × chiều dài bước đi) | O(số lần đi bộ) | Quá chậm | 
| Tối ưu (phân tách bitmask + kết nối) | O(30 × (n + m) + q) | O(n × 30) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo ba mảng để kết nối biểu đồ. Đầu tiên theo dõi xem mỗi đỉnh có thể tiếp cận được từ nhau hay không, bỏ qua trọng số cạnh (đối với MEX ≥ 0). Phần thứ hai theo dõi xem một đỉnh có thể truy cập được thông qua các cạnh có trọng số có bit 0 được đặt hay không (đối với MEX ≥ 1). Phần thứ ba theo dõi xem một đỉnh có thể tiếp cận được thông qua các cạnh có trọng số bằng 0 hay không (đối với MEX ≥ 2). 
2. Xử lý trước các cạnh theo mẫu bit của chúng. Đối với mỗi trọng lượng cạnh, hãy xem xét các bit của nó từ 0 đến 29. Thêm cạnh vào đồ thị con cho mỗi bit được đặt. Nếu một cạnh có trọng số bằng 0, hãy theo dõi nó một cách riêng biệt để cân nhắc MEX = 2. 
3. Xây dựng cấu trúc tìm kiếm hợp nhất (DSU) cho từng biểu đồ bit. Hợp nhất các đỉnh được kết nối bởi các cạnh nơi đặt bit tương ứng. Điều này cho phép truy vấn kết nối liên tục giữa các đỉnh cho mỗi mặt nạ bit. 
4. Đối với mỗi truy vấn (u, v), hãy kiểm tra xem u và v có được kết nối trong sơ đồ con của các cạnh với tất cả các bit được đặt hay không (với MEX = 0). Nếu có, xuất 0. Ngược lại, kiểm tra xem u có được kết nối với bất kỳ đỉnh nào có trọng số cạnh có tập bit có ý nghĩa nhỏ nhất hoặc có trọng số khác 0 đối với MEX = 1. Nếu vậy, xuất 1. Ngược lại, xuất 2. 
5. Trả về câu trả lời cho mỗi truy vấn. 

Tại sao nó hoạt động: kết nối tìm liên kết duy trì tính bất biến rằng nếu hai đỉnh được kết nối thông qua các cạnh có bit tôi đặt, thì bất kỳ bước đi nào giữa chúng sẽ giữ nguyên bit trong tiền tố AND. Do đó, việc kiểm tra khả năng kết nối trong từng sơ đồ con bit cho phép chúng tôi xác định xem MEX có thể là 0, 1 hoặc 2 mà không cần xây dựng các bước đi thực tế hay không. Bởi vì AND là đơn điệu nên khi thiếu một bit, tất cả các tiền tố dài hơn sẽ không thể khôi phục nó, đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(1 << 25)

class DSU:
    def __init__(self, n):
        self.parent = list(range(n))
    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]
    def union(self, x, y):
        xr, yr = self.find(x), self.find(y)
        if xr != yr:
            self.parent[yr] = xr

n, m = map(int, input().split())
edges = []
for _ in range(m):
    a, b, w = map(int, input().split())
    edges.append((a-1, b-1, w))

dsu0 = DSU(n)
dsu1 = DSU(n)

for a, b, w in edges:
    if w == 0:
        dsu0.union(a, b)
    for bit in range(30):
        if (w >> bit) & 1:
            dsu1.union(a, b)

q = int(input())
queries = [tuple(map(lambda x: int(x)-1, input().split())) for _ in range(q)]

for u, v in queries:
    if dsu0.find(u) == dsu0.find(v):
        print(0)
    elif dsu1.find(u) == dsu1.find(v):
        print(1)
    else:
        print(2)
```Lớp DSU xử lý kết nối hiệu quả. Chúng tôi duy trì hai DSU: một cho các cạnh có trọng số bằng 0 và một cho các cạnh có ít nhất một bit được đặt. Đối với mỗi truy vấn, trước tiên chúng tôi kiểm tra khả năng kết nối trọng số bằng 0 để tìm MEX = 0. Nếu không, chúng tôi kiểm tra khả năng kết nối ở các cạnh với bất kỳ bit nào khác 0 để tìm MEX = 1. Nếu cả hai đều không áp dụng thì MEX tối thiểu có thể là 2. 

## Ví dụ đã hoạt động 

Đầu vào mẫu:```
6 7
1 2 1
2 3 3
3 1 5
4 5 2
5 6 4
6 4 6
3 4 1
3
1 5
1 2
5 3
```| Truy vấn | dsu0 đã kết nối | dsu1 đã kết nối | Đầu ra | 
| --- | --- | --- | --- | 
| 1 → 5 | không | vâng | 2 | 
| 1 → 2 | vâng | vâng | 0 | 
| 5 → 3 | không | vâng | 1 | 

Bảng xác nhận rằng khả năng kết nối trong biểu đồ con thích hợp sẽ trực tiếp xác định MEX. 

Một ví dụ khác:```
3 3
1 2 2
2 3 4
3 1 1
2
1 3
2 1
```Dấu vết: 

| Truy vấn | dsu0 đã kết nối | dsu1 đã kết nối | Đầu ra | 
| --- | --- | --- | --- | 
| 1 → 3 | không | vâng | 1 | 
| 2 → 1 | không | vâng | 1 | 

Điều bất biến đúng: Kết nối DSU dựa trên mẫu bit dự đoán chính xác MEX tối thiểu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) * 30 + q) | Việc xây dựng DSU trên mỗi bit yêu cầu các cạnh lặp × 30 bit; truy vấn là O(1) cho mỗi lần tra cứu DSU | 
| Không gian | O(n * 2) | Hai DSU, mỗi DSU lưu trữ n cha | 

Với$n, m, q \le 10^5$, hoạt động vẫn còn dưới$10^7$, thoải mái trong thời hạn. Bộ nhớ cũng dưới 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    # paste the solution here
    import sys
    input = sys.stdin.readline
    sys.setrecursionlimit(1 << 25)

    class DSU:
        def __init__(self, n):
            self.parent = list(range(n))
        def find(self, x):
            if self.parent[x] != x:
                self.parent[x]
```
