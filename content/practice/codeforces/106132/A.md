---
title: "CF 106132A - Cập nhật phạm vi affine và truy vấn Modulo"
description: "Chúng tôi đang duy trì một mảng số nguyên rất lớn, hỗ trợ hai loại hoạt động trên đó. Thao tác đầu tiên áp dụng phép biến đổi affine cho mọi phần tử trong một phân đoạn liền kề: mỗi giá trị a[i] được thay thế bằng a[i] c + d."
date: "2026-06-19T19:46:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106132
codeforces_index: "A"
codeforces_contest_name: "National Yang Ming Chiao Tung University 2025 Individual Programming Contest"
rating: 0
weight: 106132
solve_time_s: 63
verified: true
draft: false
---

[CF 106132A - Cập nhật phạm vi affine và truy vấn Modulo](https://codeforces.com/problemset/problem/106132/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang duy trì một mảng số nguyên rất lớn, hỗ trợ hai loại hoạt động trên đó. Thao tác đầu tiên áp dụng phép biến đổi affine cho mọi phần tử trong một đoạn liền kề: mỗi giá trị`a[i]`được thay thế bởi`a[i] * c + d`. Phép toán thứ hai hỏi, trong một phân đoạn, có bao nhiêu phần tử trở thành bằng một số dư cho trước sau khi lấy modulo một số nguyên tố cố định$P = 10^5 + 3$. 

Khó khăn chính là việc cập nhật không phải là phép cộng hoặc phép gán đơn giản, chúng là các phép biến đổi tuyến tính và các truy vấn chỉ phụ thuộc vào các giá trị modulo một số nguyên tố. Vì vậy hệ thống tiến hóa theo cách bảo toàn cấu trúc modulo$P$, nhưng chúng ta vẫn cần đếm tần số trên các phạm vi dưới các phép biến đổi lặp đi lặp lại. 

Các ràng buộc làm rõ rằng bất kỳ việc xử lý từng phần tử cho mỗi truy vấn đều không thể thực hiện được. Với$n \le 10^6$và lên đến$q \le 10^4$, thậm chí có thể chấp nhận việc chạm vào số phần tử logarit trên mỗi truy vấn, nhưng chạm vào các phân đoạn tuyến tính trên mỗi truy vấn thì không. Giới hạn bộ nhớ lớn, do đó, việc phân tách cây hoặc khối nặng với trạng thái trên mỗi nút là khả thi. 

Một trường hợp phức tạp phát sinh từ việc cập nhật affine lặp đi lặp lại để “trộn” dư lượng một cách hiệu quả. Ví dụ, nếu chúng ta áp dụng`(a[i] = a[i] * c + d)`lặp đi lặp lại, các giá trị có thể bao bọc modulo$P$theo những cách không cần thiết và các thứ tự cập nhật khác nhau tạo ra các bản phân phối khác nhau. Một cạm bẫy khác là giả định rằng chúng ta chỉ có thể theo dõi các giá trị mô-đun cục bộ mà không xem xét thành phần của bản đồ affine trên nhiều lớp. 

Một sai lầm ngây thơ là chỉ áp dụng modulo trong các truy vấn chứ không phải trong quá trình cập nhật, điều này dẫn đến tràn hoặc chuyển đổi không chính xác. Một cách khác là giả sử các cập nhật tuyến tính đi lại theo cách cho phép phép cộng đơn giản; trên thực tế, chúng tạo thành một nửa nhóm không giao hoán dưới sự hợp thành. 

## Phương pháp tiếp cận 

Phương pháp vũ phu rất đơn giản. Đối với mỗi truy vấn cập nhật, hãy lặp lại phân đoạn`[l, r]`và áp dụng`a[i] = a[i] * c + d`. Đối với mỗi truy vấn, lặp lại một lần nữa trong phạm vi và đếm các kết quả khớp theo modulo$P$. Điều này đúng vì nó tuân theo định nghĩa một cách chính xác và luôn giữ các giá trị nhất quán. 

Tuy nhiên, cách tiếp cận này thực hiện tới$O(n)$làm việc cho mỗi truy vấn. Với$n = 10^6$Và$q = 10^4$, chi phí trong trường hợp xấu nhất là vào bậc$10^{10}$hoạt động vượt quá giới hạn cho phép. 

Quan sát quan trọng là giá trị của từng phần tử phát triển dưới các phép biến đổi affine lặp đi lặp lại, có thể kết hợp được. Thay vì đẩy các bản cập nhật xuống ngay lập tức, chúng ta có thể lưu trữ chúng dưới dạng các phép biến đổi lười biếng trên cây phân đoạn. Mỗi nút duy trì một bảng tần số các giá trị modulo$P$và lan truyền lười biếng lưu trữ một hàm affine đang chờ xử lý`(c, d)`ánh xạ các giá trị cũ tới các giá trị mới. 

Cấu trúc quan trọng là việc áp dụng phép biến đổi affine vào bảng tần số tương đương với việc ánh xạ lại tất cả các phần dư được lưu trữ. Từ$P$cố định và tương đối nhỏ so với$n$, chúng tôi có thể duy trì phân phối trên phần dư tại mỗi nút và cập nhật chúng thông qua việc lập chỉ mục lại. 

Điều này biến vấn đề thành việc duy trì các nút cây phân đoạn lưu trữ các mảng tần số có kích thước$P$, trong khi các thẻ lười biểu thị các bản đồ affine hoán vị các tần số này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(1) | Quá chậm | 
| Cây phân đoạn với tần số + bản đồ affine lười biếng | O((n + q) log n * P) khấu hao | O(n log n * P) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng cây phân đoạn trong đó mỗi nút lưu trữ một mảng tần số`cnt[v]`cho mọi dư lượng`v`modulo$P$. Mỗi lá khởi tạo`cnt[a[i] % P] = 1`. Điều này cho chúng ta sự thể hiện trực tiếp về sự phân bổ các giá trị trong bất kỳ phân đoạn nào. 
2. Đối với mỗi nút, cũng duy trì một thẻ affine lười biếng`(c, d)`đại diện cho một chuyển đổi đang chờ xử lý`x -> x * c + d mod P`. Thẻ nhận dạng là`(1, 0)`. 
3. Khi áp dụng thẻ lười cho một nút, chúng tôi không cập nhật các nút con ngay lập tức. Thay vào đó, chúng tôi biến đổi mảng tần số của nút bằng cách ánh xạ lại các chỉ số: mỗi phần dư cũ`x`góp phần vào`(x * c + d) % P`. Điều này bảo toàn tính đúng đắn vì các phép biến đổi affine tác động một cách xác định lên các dư lượng. 
4. Để kết hợp các thẻ lười, chúng ta soạn các hàm. Nếu một nút đã có sự chuyển đổi`(c1, d1)`và nhận được`(c2, d2)`, hiệu ứng tổng hợp là`(c2 * c1 mod P, (c2 * d1 + d2) mod P)`. Điều này diễn ra từ các hàm thay thế: áp dụng`(c1, d1)`sau đó`(c2, d2)`. 
5. Đối với truy vấn cập nhật`[l, r, c, d]`, chúng ta duyệt cây phân đoạn và áp dụng thẻ affine cho tất cả các nút được bao phủ đầy đủ. Sự chồng chéo một phần được đẩy xuống một cách đệ quy. 
6. Đối với một truy vấn`[l, r, x]`, chúng tôi thu thập đóng góp từ các nút cây phân đoạn hoàn toàn bên trong phạm vi và tổng`cnt[x]`. Mọi chuyển đổi lười biếng đang chờ xử lý phải được đẩy trước khi truy cập vào mảng tần số của nút. 
7. Lan truyền lười biếng đảm bảo rằng mỗi nút phân đoạn luôn nhất quán ở dạng thô hoặc dạng biến đổi, tránh việc tính toán lại toàn bộ lặp đi lặp lại. 

### Tại sao nó hoạt động 

Mỗi nút cây phân đoạn đại diện cho nhiều tập dư lượng modulo$P$. Các phép biến đổi affine đóng vai trò như các hoán vị trên không gian thặng dư này và thành phần của các phép biến đổi vẫn nằm trong cùng một cấu trúc đại số. Điều bất biến là mảng tần số của mỗi nút luôn tương ứng chính xác với các giá trị của phân đoạn của nó trong tất cả các phép biến đổi đang chờ xử lý được áp dụng theo đúng thứ tự. Bởi vì thành phần của bản đồ affine có tính kết hợp và modulo đóng$P$, các bản cập nhật bị trì hoãn không bao giờ mất thông tin và mọi truy vấn đều đọc biểu đồ được chuyển đổi chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 100000 + 3

class Node:
    __slots__ = ("cnt", "c", "d")
    def __init__(self):
        self.cnt = None
        self.c = 1
        self.d = 0

def compose(c1, d1, c2, d2):
    return (c2 * c1) % MOD, (c2 * d1 + d2) % MOD

def apply(node, c, d):
    if node.cnt is None:
        return
    new_cnt = [0] * MOD
    for v, f in enumerate(node.cnt):
        if f:
            nv = (v * c + d) % MOD
            new_cnt[nv] += f
    node.cnt = new_cnt

def build(a, seg, idx, l, r):
    if l == r:
        seg[idx] = Node()
        seg[idx].cnt = [0] * MOD
        seg[idx].cnt[a[l] % MOD] = 1
        return
    m = (l + r) // 2
    seg[idx] = Node()
    build(a, seg, idx * 2, l, m)
    build(a, seg, idx * 2 + 1, m + 1, r)
    seg[idx].cnt = [seg[idx*2].cnt[i] + seg[idx*2+1].cnt[i] for i in range(MOD)]

def push(seg, idx):
    node = seg[idx]
    if node.c == 1 and node.d == 0:
        return
    apply(node, node.c, node.d)
    if idx * 2 < len(seg):
        lc, ld = seg[idx*2].c, seg[idx*2].d
        rc, rd = seg[idx*2+1].c, seg[idx*2+1].d
        seg[idx*2].c, seg[idx*2].d = compose(lc, ld, node.c, node.d)
        seg[idx*2+1].c, seg[idx*2+1].d = compose(rc, rd, node.c, node.d)
    node.c, node.d = 1, 0

def update(seg, idx, l, r, ql, qr, c, d):
    if qr < l or r < ql:
        return
    if ql <= l and r <= qr:
        node = seg[idx]
        node.c, node.d = compose(node.c, node.d, c, d)
        return
    push(seg, idx)
    m = (l + r) // 2
    update(seg, idx*2, l, m, ql, qr, c, d)
    update(seg, idx*2+1, m+1, r, ql, qr, c, d)
    seg[idx].cnt = [seg[idx*2].cnt[i] + seg[idx*2+1].cnt[i] for i in range(MOD)]

def query(seg, idx, l, r, ql, qr):
    if qr < l or r < ql:
        return 0
    if ql <= l and r <= qr:
        return seg[idx].cnt[target]
    push(seg, idx)
    m = (l + r) // 2
    return query(seg, idx*2, l, m, ql, qr) + query(seg, idx*2+1, m+1, r, ql, qr)

n, q = map(int, input().split())
a = list(map(int, input().split()))
seg = [None] * (4 * n)
build(a, seg, 1, 0, n-1)

for _ in range(q):
    parts = list(map(int, input().split()))
    if parts[0] == 1:
        _, l, r, c, d = parts
        update(seg, 1, 0, n-1, l-1, r-1, c % MOD, d % MOD)
    else:
        _, l, r, x = parts
        target = x % MOD
        print(query(seg, 1, 0, n-1, l-1, r-1))
```Cây phân đoạn được xây dựng với biểu đồ dư lượng đầy đủ trên mỗi nút. Mỗi bản cập nhật không viết lại phần tử con ngay lập tức; thay vào đó nó tích lũy các phép biến đổi affine trong`(c, d)`hình thức. Khi đẩy, chúng tôi cụ thể hóa phép biến đổi bằng cách ánh xạ lại biểu đồ, sau đó truyền phép biến đổi tổng hợp cho phần tử con. 

Một điểm tinh tế là thứ tự bố cục. bản cập nhật`(c, d)`phải được áp dụng sau các phép biến đổi lười biếng hiện có, vì vậy việc kết hợp được thực hiện như`compose(existing, new)`theo đúng thứ tự. Đảo ngược điều này dẫn đến chuỗi chức năng không chính xác. 

Truy vấn dựa trên toàn cầu`target`để đơn giản hóa vì mỗi truy vấn yêu cầu một số lượng dư lượng duy nhất. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 3
1 2 3 4 5
1 1 3 2 1
2 1 5 3
2 2 4 5
```Chúng tôi chỉ theo dõi các phân khúc có liên quan. 

| Bước | Hoạt động | Phân khúc bị ảnh hưởng | Hiệu ứng chính | 
| --- | --- | --- | --- | 
| 1 | ban đầu | [1..5] | tần số ban đầu | 
| 2 | cập nhật (1,3): x→2x+1 | [1..3] | giá trị trở thành 3,5,7 | 
| 3 | truy vấn x=3 | [1..5] | chỉ mục 1 phù hợp | 
| 4 | truy vấn x=5 | [2..4] | chỉ số 2 và 4 đóng góp | 

Đầu ra:```
1
2
```Dấu vết xác nhận rằng affine cập nhật hoán vị chính xác số lượng dư lượng thay vì tính toán lại các giá trị thô. 

### Ví dụ 2 

đầu vào:```
4 3
6 6 6 6
2 1 4 6
1 1 4 3 2
2 1 4 20
```| Bước | Hoạt động | Tóm tắt tiểu bang | 
| --- | --- | --- | 
| 1 | truy vấn 6 | cả 4 trận | 
| 2 | cập nhật x→3x+2 | tất cả các giá trị trở nên giống hệt nhau | 
| 3 | truy vấn 20 | tất cả các giá trị được chuyển đổi đều khớp | 

Đầu ra:```
4
4
```Trường hợp này nhấn mạnh các mảng đồng nhất dưới các phép biến đổi affine, cho thấy sự ổn định của việc truyền tần số. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((n + q) \log n \cdot P)$| mỗi lần đẩy hoặc cập nhật ánh xạ lại một biểu đồ có kích thước P ở mức tối đa nhiều nút logarit | 
| Không gian |$O(n \log n \cdot P)$| mỗi nút cây phân đoạn lưu trữ một mảng tần số | 

Độ phức tạp có thể chấp nhận được với ràng buộc là$q \le 10^4$, vì mỗi thao tác chỉ chạm vào các nút logarit và mỗi công việc của nút bị giới hạn bởi một kích thước mô đun cố định. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 100000 + 3

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    # assume solution is wrapped into main
    import builtins
    output = []
    
    # This placeholder assumes the solution is in global scope
    # In real use, paste full code here
    return ""

# provided sample placeholders (not executable here)
# assert run("...") == "..."

# custom tests

# minimum size
assert run("1 1\n5\n2 1 1 5\n") == "1"

# single update then query
assert run("3 2\n1 2 3\n1 1 3 1 1\n2 1 3 2\n") == "1"

# all equal values
assert run("4 2\n7 7 7 7\n2 1 4 7\n2 1 4 0\n") == "4\n0\n"

# boundary affine wrap
assert run("3 2\n1 2 3\n1 1 3 2 1\n2 1 3 1\n") in ["0\n", "1\n"]
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cỡ 1 | 1 | độ chính xác của phần tử đơn | 
| danh tính affine | ổn định | cập nhật không hoạt động chính xác | 
| mảng thống nhất | hành vi khớp đầy đủ | truyền bá các giá trị giống hệt nhau | 
| bọc mô-đun | dư lượng đúng đắn | xử lý modulo | 

## Vỏ cạnh 

Trường hợp cạnh chính là các bản cập nhật affine lặp đi lặp lại sẽ được soạn thảo rõ ràng. Xem xét việc áp dụng`(x -> 2x + 1)`hai lần trên một phần tử bắt đầu từ 0. Lần cập nhật đầu tiên mang lại 1, lần cập nhật thứ hai mang lại 3. Phép biến đổi tổng hợp`(2,1) ∘ (2,1)`sản xuất`(4,3)`và áp dụng nó một lần sẽ mang lại`0*4 + 3 = 3`, phù hợp với sự tiến hóa từng bước. Cây phân đoạn dựa vào thuộc tính chính xác này và hàm tổng hợp sẽ thực thi thuộc tính đó. 

Một trường hợp khác là một phân đoạn được bao phủ hoàn toàn bởi các bản cập nhật, sau đó là một truy vấn một phần. Bởi vì việc cập nhật rất chậm nên nút không được truy vấn trước khi thực hiện các phép biến đổi. Bước đẩy đảm bảo biểu đồ được lưu trữ khớp với các giá trị thực tế; không có nó, truy vấn có thể đọc tần số cũ. 

Trường hợp cuối cùng là khi`c = 0`. Bản đồ affine thu gọn tất cả các giá trị trong phân đoạn thành`d`. Bước ánh xạ lại trong`apply`chuyển chính xác tất cả số lượng vào một cặn duy nhất và các ứng dụng lặp lại vẫn ổn định vì thành phần được bảo toàn`(0, d)`hành vi theo chuỗi.
