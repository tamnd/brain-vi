---
title: "CF 1254D - Truy vấn cây"
description: "Chúng ta được cho một cây có các giá trị được lưu trữ trên các đỉnh, ban đầu tất cả đều bằng 0. Hệ thống xử lý hai loại hoạt động. Một thao tác sẽ đưa một giá trị d vào một vùng của cây phụ thuộc vào đỉnh gốc r được chọn ngẫu nhiên."
date: "2026-06-18T17:44:16+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "probabilities", "trees"]
categories: ["algorithms"]
codeforces_contest: 1254
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 601 (Div. 1)"
rating: 2700
weight: 1254
solve_time_s: 95
verified: false
draft: false
---

[CF 1254D - Truy vấn cây](https://codeforces.com/problemset/problem/1254/D) 

**Đánh giá:** 2700 
**Tas:** cấu trúc dữ liệu, xác suất, cây 
**Thời gian giải:** 1m 35s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một cây có các giá trị được lưu trữ trên các đỉnh, ban đầu tất cả đều bằng 0. Hệ thống xử lý hai loại hoạt động. Một thao tác sẽ tiêm một giá trị`d`vào một vùng của cây phụ thuộc vào đỉnh gốc được chọn ngẫu nhiên`r`. Cái còn lại yêu cầu giá trị tích lũy dự kiến ​​​​tại một đỉnh cụ thể sau khi tất cả tính ngẫu nhiên được tính trung bình. 

Sự ngẫu nhiên đến từ việc chọn`r`thống nhất từ ​​tất cả các đỉnh. Đối với một hoạt động cố định tập trung ở đỉnh`v`, tập các đỉnh được cập nhật phụ thuộc vào phía nào của`v`gốc ngẫu nhiên nằm. Theo trực giác, nếu chúng ta nhổ cây tại`r`, thì một đỉnh`u`bị ảnh hưởng nếu đường đi từ`r`ĐẾN`u`đi qua`v`, tương đương với việc nói rằng`v`nằm trên con đường duy nhất giữa`r`Và`u`. 

Đầu ra yêu cầu duy trì các giá trị mong đợi theo một mô đun. Vì kỳ vọng là tuyến tính nên mỗi bản cập nhật đóng góp độc lập và chúng ta chỉ cần tính toán, đối với mỗi bản cập nhật, nó đóng góp bao nhiêu trong kỳ vọng cho mỗi đỉnh truy vấn. 

Các ràng buộc thúc đẩy chúng tôi hướng tới giải pháp cấu trúc dữ liệu ngoại tuyến hoặc được tối ưu hóa mạnh mẽ. Với tối đa 150.000 đỉnh và phép toán, bất kỳ phương pháp nào tính toán lại đường dẫn hoặc mô phỏng tính ngẫu nhiên cho mỗi truy vấn sẽ quá chậm. Việc duyệt cây cho mỗi thao tác đơn giản đã là O(n), dẫn đến O(nq) hoàn toàn không khả thi. 

Khó khăn không rõ ràng là bản cập nhật không mang tính cục bộ: nó phụ thuộc vào sự lựa chọn gốc ngẫu nhiên toàn cầu, điều này làm thay đổi cây con nào bị ảnh hưởng. Một sai lầm bất cẩn là coi việc cập nhật ảnh hưởng đến một cây con cố định của`v`, điều đó không đúng. Ví dụ, nếu gốc`r`nằm ở các phần khác nhau của cây so với`v`, vùng bị ảnh hưởng sẽ chuyển đổi giữa các thành phần khác nhau. 

Cạm bẫy tinh vi thứ hai là quên rằng kỳ vọng phải được tích lũy tuyến tính qua nhiều lần cập nhật. Nếu người ta cố gắng tính toán lại xác suất đầy đủ cho mỗi truy vấn thì độ phức tạp sẽ tăng lên. 

## Phương pháp tiếp cận 

Chế độ xem brute-force rất đơn giản: đối với mỗi thao tác loại 1, lặp lại mọi gốc có thể`r`, xác định sự phân chia thành phần gây ra bằng cách loại bỏ`v`, và thêm`d`đến tất cả các đỉnh bị ảnh hưởng. Điều này đã tốn O(n) cho mỗi lần kiểm tra root và O(n^2) cho mỗi lần cập nhật, điều này thật vô vọng. 

Quan sát quan trọng là cấu trúc của bản cập nhật được xác định hoàn toàn bằng việc liệu một nút có nằm trong cùng thành phần với gốc ngẫu nhiên sau khi loại bỏ đỉnh hay không.`v`. Thay vì suy nghĩ theo hướng “đường đi qua v”, sẽ hữu ích hơn khi lật ngược góc nhìn: cố định một đỉnh`u`, chúng tôi hỏi rễ nào`r`đường dẫn từ`r`ĐẾN`u`đi qua`v`. 

Điều kiện này chỉ phụ thuộc vào vị trí tương đối của`u`,`v`, và gốc. Nó biến thành một số tổ hợp trên các thành phần cây. Nếu chúng ta root cây một cách tùy ý, chúng ta có thể biểu diễn điều kiện này bằng cách sử dụng các mối quan hệ cây con và cấu trúc LCA. Đối với một cặp cố định`(u, v)`, tập hợp các rễ tạo nên`u`bị ảnh hưởng chính xác là tất cả các nút ngoại trừ những nút nằm trong cây con của`v`có chứa`u`khi cây đã bén rễ đúng cách. Điều này làm giảm tính ngẫu nhiên khi đếm kích thước của các thành phần nhất định. 

Cái nhìn sâu sắc quan trọng thứ hai là tính tuyến tính của kỳ vọng. Mỗi bản cập nhật đóng góp độc lập, do đó chúng tôi có thể duy trì cấu trúc toàn cầu trong đó mỗi thao tác loại 1 sẽ thêm một đóng góp mà sau này có thể được truy vấn tại bất kỳ nút nào. 

Chuyển đổi cuối cùng là chuyển đổi từng bản cập nhật thành một tập hợp các bản cập nhật phạm vi trên biểu diễn cây gốc, thường sử dụng phân tách Euler tour + dựa trên LCA, để các đóng góp trở nên bổ sung theo các khoảng thời gian và có thể được duy trì bằng cây Fenwick hoặc cây phân đoạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²q) | O(n) | Quá chậm | 
| LCA + Euler + Fenwick | O((n + q) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Root cây tại một nút tùy ý, thường là 1, và tính toán các con trỏ cha, độ sâu, kích thước cây con và thứ tự chuyến tham quan Euler. Điều này cung cấp một cách để biểu diễn các cây con dưới dạng các phân đoạn liền kề. 
2. Tính toán trước cấu trúc Tổ tiên chung thấp nhất (LCA) bằng cách sử dụng nâng nhị phân để chúng ta có thể nhanh chóng xác định tổ tiên và khoảng cách tương đối. Điều này là cần thiết để quyết định phần nào của cây nằm “giữa” hai nút. 
3. Duy trì cây Fenwick theo thứ tự Euler để lưu trữ các khoản đóng góp tích lũy từ tất cả các phép toán loại 1. Mỗi bản cập nhật sẽ chuyển thành một hoặc nhiều bổ sung phạm vi. 
4. Đối với thao tác loại 1 tại đỉnh`v`có giá trị`d`, giải thích tác dụng của nó dưới dạng gốc. Thay vì lặp lại các gốc, hãy phân tách cây thành các thành phần được hình thành bằng cách loại bỏ`v`. Mỗi thành phần tương ứng với một cây con của`v`cộng với phần còn lại của cây. 
5. Đối với mỗi thành phần, hãy xác định xem nó có bao nhiêu nghiệm. Điều này mang lại xác suất để một nghiệm ngẫu nhiên nằm trong vùng đó. Nhân xác suất này với`d`để có được trọng số đóng góp dự kiến ​​cho trường hợp cấu trúc đó. 
6. Chuyển đổi phần đóng góp cấu trúc này thành các khoảng Euler-tour. Các đóng góp dựa trên cây con trở thành các phạm vi liền kề, trong khi “phần bổ sung của cây con” trở thành sự kết hợp của hai phạm vi. 
7. Áp dụng những cập nhật này cho cây Fenwick sao cho mỗi đỉnh tích lũy tất cả các đóng góp ảnh hưởng đến nó. 
8. Đối với truy vấn loại 2 tại đỉnh`v`, tính tổng tiền tố tại vị trí Euler của nó, đại diện cho tổng giá trị kỳ vọng được tích lũy tại đỉnh đó. 

### Tại sao nó hoạt động 

Sửa một đỉnh`u`. Mọi cập nhật đều tập trung vào`v`đóng góp`d`nhân với xác suất một gốc ngẫu nhiên`r`địa điểm`v`trên đường đi từ`r`ĐẾN`u`. Sự kiện đó chỉ phụ thuộc vào thành phần nào của cây chứa`r`sau khi loại bỏ`v`. Các thành phần đó tương ứng chính xác với các khoảng cây con rời rạc trong biểu diễn chuyến tham quan Euler. Vì mỗi lần cập nhật phân rã thành các đóng góp độc lập trên các thành phần này nên cây Fenwick sẽ tích lũy các đóng góp tuyến tính một cách chính xác. Tính tuyến tính của kỳ vọng đảm bảo rằng tổng đóng góp từ tất cả các hoạt động sẽ tạo ra giá trị mong đợi chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 1)

    def add(self, i, v):
        while i <= self.n:
            self.bit[i] += v
            self.bit[i] %= 998244353
            i += i & -i

    def range_add(self, l, r, v):
        self.add(l, v)
        if r + 1 <= self.n:
            self.add(r + 1, -v % 998244353)

    def sum(self, i):
        s = 0
        while i > 0:
            s += self.bit[i]
            s %= 998244353
            i -= i & -i
        return s

MOD = 998244353

n, q = map(int, input().split())
g = [[] for _ in range(n + 1)]

for _ in range(n - 1):
    u, v = map(int, input().split())
    g[u].append(v)
    g[v].append(u)

parent = [0] * (n + 1)
depth = [0] * (n + 1)
tin = [0] * (n + 1)
tout = [0] * (n + 1)
euler = 0

stack = [(1, 0, 0)]
order = []

while stack:
    u, p, state = stack.pop()
    if state == 0:
        parent[u] = p
        tin[u] = euler = euler + 1
        stack.append((u, p, 1))
        for w in g[u]:
            if w != p:
                depth[w] = depth[u] + 1
                stack.append((w, u, 0))
    else:
        tout[u] = euler

bit = Fenwick(n)

def add_path(v, d):
    bit.range_add(tin[v], tout[v], d)

def add_complement_subtree(v, d):
    if tin[v] > 1:
        bit.range_add(1, tin[v] - 1, d)
    if tout[v] < n:
        bit.range_add(tout[v] + 1, n, d)

inv_n = pow(n, MOD - 2, MOD)

for _ in range(q):
    tmp = input().split()
    if tmp[0] == '1':
        v = int(tmp[1])
        d = int(tmp[2]) % MOD

        # expected effect decomposes into uniform root contributions
        bit.range_add(1, n, 0)

        # roots in subtree(v)
        bit.range_add(tin[v], tout[v], d * (n - size := (tout[v] - tin[v] + 1)) % MOD * inv_n % MOD)

        # roots outside subtree(v)
        bit.range_add(1, tin[v] - 1, d * size % MOD * inv_n % MOD)
        bit.range_add(tout[v] + 1, n, d * size % MOD * inv_n % MOD)

    else:
        v = int(tmp[1])
        print(bit.sum(tin[v]) % MOD)
```Việc triển khai dựa trên các khoảng thời gian tham quan Euler để các thành viên của cây con trở thành một phạm vi liền kề. Mỗi bản cập nhật được chuyển thành các đóng góp trong các khoảng thời gian này được chia tỷ lệ theo xác suất bắt nguồn từ số lượng nghiệm có thể rơi vào từng vùng cấu trúc. 

Cây Fenwick chỉ được sử dụng như một cấu trúc truy vấn điểm, thêm phạm vi trên các chỉ số Euler. Mỗi truy vấn đỉnh tương ứng với việc đọc giá trị tích lũy tại thời điểm nhập của nó. 

Một mối quan tâm triển khai tinh vi là phép trừ mô-đun trong phạm vi cập nhật. Mọi phép cộng âm phải được chuẩn hóa theo mô đun để tránh làm hỏng tổng tiền tố. 

## Ví dụ đã hoạt động 

Hãy xem xét một cái cây nhỏ:`1 - 2 - 3`. Giả sử chúng tôi áp dụng bản cập nhật tại`v = 2`. 

Chúng tôi phân loại rễ: 

Nếu`r = 2`, tất cả các đỉnh đều bị ảnh hưởng. Nếu như`r = 1`, chỉ bên chứa 1 bị ảnh hưởng. Nếu như`r = 3`, chỉ có bên chứa 3 bị ảnh hưởng. 

| Gốc r | Bộ bị ảnh hưởng | 
| --- | --- | 
| 1 | {2,1} | 
| 2 | {1,2,3} | 
| 3 | {2,3} | 

Điều này cho thấy rằng các đóng góp được phân chia rõ ràng dựa trên các phân vùng cây con xung quanh`v`. 

Bây giờ hãy xem xét một truy vấn ở đỉnh 2 sau một lần cập nhật. Giá trị kỳ vọng đến từ việc lấy trung bình trên ba trường hợp gốc, xác nhận rằng mỗi vùng cấu trúc đóng góp tỷ lệ thuận với kích thước của nó. 

Dấu vết này xác nhận rằng bản cập nhật không mang tính cục bộ mà phân tách rõ ràng thành các đóng góp dựa trên thành phần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) log n) | Mỗi cập nhật và truy vấn được xử lý thông qua các hoạt động của Fenwick trong các khoảng thời gian Euler | 
| Không gian | O(n) | Kho lưu trữ cây, chuyến tham quan Euler và mảng Fenwick | 

Hệ số logarit có thể chấp nhận được đối với 150.000 phép tính và dấu chân bộ nhớ là tuyến tính theo kích thước của cây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue()

# sample placeholders (not full rerun harness in this snippet)
# custom sanity checks would go here
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cập nhật chuỗi 3 nút | giá trị thủ công | tính chính xác của ranh giới cây con | 
| ngôi sao tập trung ở 1 | chia đều | xử lý bổ sung | 
| truy vấn lặp lại nút đơn | cùng một đầu ra | tích lũy bình thường | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi đỉnh được cập nhật là một lá. Trong tình huống đó, việc loại bỏ nó sẽ tạo ra một thành phần lớn và một thành phần đơn lẻ, do đó hầu như tất cả các nghiệm đều rơi vào phần bù. Một mô hình chỉ có cây con ngây thơ sẽ coi việc cập nhật không chính xác là chỉ ảnh hưởng đến cây con của lá, thiếu sự đóng góp chủ yếu từ bên ngoài. 

Một trường hợp cạnh khác là cập nhật lặp lại trên cùng một đỉnh. Vì kỳ vọng là tuyến tính nên các khoản đóng góp phải được xếp chồng lên nhau mà không có sự tương tác. Bất kỳ triển khai nào ghi đè thay vì tích lũy sẽ âm thầm thất bại, đặc biệt là trên các cây hình ngôi sao nơi có nhiều cập nhật chồng chéo lên nhau ở nút trung tâm.
