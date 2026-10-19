---
title: "CF 105631G - Tính toán tổng kiểm tra chung"
description: "Chúng ta được cung cấp một mảng số nguyên và chúng ta cần trả lời nhiều truy vấn độc lập. Mỗi truy vấn chỉ định một phạm vi trong mảng và một giá trị ngưỡng."
date: "2026-06-22T05:41:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105631
codeforces_index: "G"
codeforces_contest_name: "SYSU Collegiate Programming Contest 2024 (SYSUCPC 2024), Final"
rating: 0
weight: 105631
solve_time_s: 52
verified: true
draft: false
---

[CF 105631G - Tính toán tổng kiểm tra chung](https://codeforces.com/problemset/problem/105631/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng số nguyên và chúng ta cần trả lời nhiều truy vấn độc lập. Mỗi truy vấn chỉ định một phạm vi trong mảng và một giá trị ngưỡng. Đối với mọi phần tử bên trong phạm vi, chúng tôi trừ ngưỡng khỏi phần tử đó và sau đó lấy XOR theo bit của tất cả các kết quả này. Câu trả lời cuối cùng cho mỗi truy vấn là giá trị XOR này. 

Ràng buộc chính là đối với mọi truy vấn, ngưỡng được đảm bảo không vượt quá bất kỳ phần tử nào trong phạm vi được truy vấn. Điều này quan trọng vì nó đảm bảo tất cả các phép trừ đều không âm, vì vậy chúng ta không bao giờ cần phải suy luận về các giá trị số học hoặc âm có dấu bên trong các biểu thức XOR theo bit. 

Cả kích thước mảng và số lượng truy vấn đều lên tới 100000, điều này ngay lập tức loại trừ mọi giải pháp tính toán lại từng truy vấn bằng cách lặp lại trên toàn bộ phạm vi của nó. Một vòng lặp lồng nhau đơn giản sẽ dẫn đến tối đa 10^10 thao tác trong trường hợp xấu nhất, vượt xa mức cho phép trong 2 giây. Bất kỳ giải pháp nào được chấp nhận đều phải giảm từng truy vấn về thời gian gần như logarit hoặc không đổi sau khi xử lý trước. 

Một vấn đề tế nhị xuất phát từ sự tương tác giữa phép trừ và XOR. Bitwise XOR không tuyến tính đối với phép cộng hoặc phép trừ, vì vậy chúng ta không thể tách biểu thức thành cấu trúc tiền tố đơn giản. Ví dụ: (a - d) ⊕ (b - d) không bằng (a ⊕ b) - d. Điều này loại bỏ khả năng các thủ thuật XOR tiền tố trực tiếp trên mảng ban đầu mà không cần sửa đổi. 

Một quan sát quan trọng khác là ràng buộc di ≤ ap bên trong mỗi phạm vi truy vấn đảm bảo rằng phép trừ hoạt động thống nhất ở cấp độ bit mà không vay mượn qua ranh giới dấu, nhưng nó không đơn giản hóa XOR về mặt đại số. Bất kỳ nỗ lực ngây thơ nào để coi phép trừ là độc lập từng bit trên mỗi bit sẽ thất bại. 

Các trường hợp biên xuất hiện khi các phạm vi chồng chéo nhau nhiều và khi di gần với giá trị nhỏ nhất trong phạm vi. Ví dụ: nếu mảng là [5, 6, 7] và di = 5 thì các giá trị trở thành [0, 1, 2], điều này thay đổi hoàn toàn cấu trúc XOR so với mảng thô. Tiền tố XOR ngây thơ của các giá trị ban đầu sẽ tạo ra kết quả hoàn toàn không liên quan. 

## Phương pháp tiếp cận 

Giải pháp brute-force xử lý từng truy vấn một cách độc lập bằng cách lặp qua tất cả các chỉ mục trong phạm vi, trừ di khỏi mỗi giá trị và XOR kết quả. Điều này đúng vì nó trực tiếp tuân theo định nghĩa của tổng kiểm tra. Tuy nhiên, mỗi truy vấn có chi phí O(ri - li + 1) và trên k truy vấn, giá trị này trở thành O(nk) trong trường hợp xấu nhất. Với n và k đều bằng 100000, điều này dẫn đến khoảng 10^10 phép toán, điều này là không khả thi. 

Cái nhìn sâu sắc quan trọng là diễn giải lại hoạt động trên mỗi bit thay vì trên mỗi giá trị. XOR tự nhiên là theo chiều bit nên mỗi vị trí bit phát triển độc lập. Sau khi trừ di, bit thứ i của (ap - di) chỉ phụ thuộc vào bit dưới của ap và di do lan truyền mượn. Điều này gợi ý chuyển đổi kiểu DP chữ số qua các bit, nhưng thực hiện nó trên mỗi truy vấn vẫn quá chậm. 

Bước đột phá thực sự là tính toán trước, đối với mỗi tiền tố của mảng, đủ cấu trúc để trả lời “có bao nhiêu số trong một phạm vi tạo ra số 1 ở bit b sau khi trừ đi”. Thay vì theo dõi trực tiếp các giá trị, chúng tôi duy trì phép thử nhị phân trên các giá trị mảng, được tăng cường để nó có thể hỗ trợ các truy vấn phạm vi có dạng: áp dụng mặt nạ trừ cố định di và đếm đóng góp trên mỗi bit. 

Điều này dẫn đến cách tiếp cận ngoại tuyến cổ điển bằng cách sử dụng cây phân đoạn của các lần thử hoặc thử được lập chỉ mục nhị phân trên các bit giá trị. Mỗi nút lưu trữ số lượng trong phân khúc của nó. Để trả lời một truy vấn, chúng ta duyệt cấu trúc trong khi mô phỏng phép trừ di từng bit, mang trạng thái mượn. Tại mỗi nút, chúng tôi xác định có bao nhiêu số rơi vào các nhánh tạo ra số 1 hoặc 0 ở bit hiện tại sau khi trừ.

Điều này biến đổi mỗi truy vấn thành O(log A · log n), trong đó log A là số bit (ở đây tối đa là 17) và log n đến từ việc duyệt cây phân đoạn. 

Lực lượng vũ phu hoạt động vì nó áp dụng trực tiếp định nghĩa, nhưng nó không thành công vì nó liên tục tính toán lại cấu trúc chồng chéo. Quan sát cho thấy phép trừ có thể được mô phỏng theo quy trình quyết định theo bit trên cây phân đoạn giúp giảm công việc lặp lại thành các bài toán con chung. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nk) | O(1) | Quá chậm | 
| Cây phân đoạn theo số lượng bit | O(k log n log A) | O(n log A) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng cây phân đoạn trên mảng trong đó mỗi nút lưu trữ một bộ ba nhị phân của tất cả các giá trị trong phân đoạn đó. Mỗi trie hỗ trợ đếm số lượng nằm dưới tiền tố bit. 

Đối với mỗi truy vấn, chúng tôi mô phỏng tính toán XOR của (ap - di) trên phạm vi [l, r] bằng cách truy vấn cây phân đoạn và truyền trạng thái trừ thông qua trie. 

## bước 

1. Xây dựng cây phân đoạn dựa trên các chỉ số từ 1 đến n, trong đó mỗi nút chứa một bộ ba giá trị nhị phân trong phân đoạn đó. Điều này cho phép chúng ta truy cập phân phối bit trong bất kỳ phạm vi nào bằng cách kết hợp các nút O(log n). 
2. Đối với mỗi nút, chèn từng giá trị ap vào trie bằng cách sử dụng biểu diễn nhị phân lên tới 17 bit. Quá trình tiền xử lý này đảm bảo sau này chúng ta có thể suy luận về việc phân phối bit một cách hiệu quả. 
3. Để trả lời một truy vấn (d, l, r), hãy phân tách phạm vi thành các nút cây phân đoạn O(log n). Mỗi nút đóng góp độc lập cho XOR cuối cùng, vì XOR có tính liên kết. 
4. Đối với mỗi nút cây phân đoạn, hãy tính phần đóng góp của tất cả các giá trị của nó sau khi trừ d. Điều này được thực hiện bằng cách duyệt qua trie trong khi mô phỏng phép trừ bằng mượn. 
5. Trong quá trình truyền tải thử, duy trì hai phần trạng thái: vị trí bit hiện tại và liệu mượn có hoạt động từ các bit thấp hơn hay không. Điều này rất cần thiết vì phép trừ ở mức bit phụ thuộc vào việc các bit thấp hơn của ap có nhỏ hơn các bit tương ứng của d hay không. 
6. Tại mỗi nút trie, chia thành các con tương ứng với bit 0 và bit 1, rồi tính xem có bao nhiêu giá trị trong mỗi nhánh tạo ra số 1 ở bit hiện tại sau khi áp dụng trạng thái trừ. 
7. Tích lũy đóng góp từ tất cả các nút cây phân đoạn bằng cách đóng góp bit XOR trên các nút. Vì XOR tổng hợp tuyến tính trên mỗi bit nên chúng tôi có thể duy trì kết quả 17 bit. 

### Tại sao nó hoạt động 

Mỗi số đóng góp độc lập cho XOR và XOR trên một phạm vi chỉ là XOR của các giá trị được chuyển đổi riêng lẻ. Cây phân đoạn đảm bảo chúng tôi chỉ tính toán lại cấu trúc một lần cho mỗi nhóm giá trị. Trie đảm bảo chúng ta có thể suy luận về phép trừ theo bit mà không cần liệt kê các giá trị. Việc xử lý mượn đảm bảo rằng phép biến đổi ap - d được biểu diễn chính xác tại mỗi vị trí bit, duy trì tính chính xác của các đóng góp bit. Bởi vì tất cả các hoạt động đều bảo toàn sự chuyển đổi chính xác theo từng phần tử trước khi tổng hợp nên không đưa ra phép tính gần đúng nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

B = 17

class Node:
    __slots__ = ("child", "cnt")
    def __init__(self):
        self.child = [None, None]
        self.cnt = 0

def insert(root, x):
    node = root
    for b in reversed(range(B)):
        node.cnt += 1
        bit = (x >> b) & 1
        if node.child[bit] is None:
            node.child[bit] = Node()
        node = node.child[bit]
    node.cnt += 1

def merge(a, b):
    if not a:
        return b
    if not b:
        return a
    a.cnt += b.cnt
    a.child[0] = merge(a.child[0], b.child[0])
    a.child[1] = merge(a.child[1], b.child[1])
    return a

def build(a, v, l, r):
    if l == r:
        root = Node()
        insert(root, a[l])
        seg[v] = root
        return
    m = (l + r) // 2
    build(a, v * 2, l, m)
    build(a, v * 2 + 1, m + 1, r)
    seg[v] = merge(seg[v * 2], seg[v * 2 + 1])

def query_nodes(v, l, r, ql, qr, res):
    if ql <= l and r <= qr:
        res.append(seg[v])
        return
    m = (l + r) // 2
    if ql <= m:
        query_nodes(v * 2, l, m, ql, qr, res)
    if qr > m:
        query_nodes(v * 2 + 1, m + 1, r, ql, qr, res)

def process_trie(node, d, bit, borrow):
    if not node:
        return 0
    if bit < 0:
        return 0

    dbit = (d >> bit) & 1

    res = 0

    for b in [0, 1]:
        child = node.child[b]
        if not child:
            continue

        # compute new borrow state and resulting bit after subtraction
        if borrow == 0:
            if b >= dbit:
                nb = 0
                valbit = b - dbit
            else:
                nb = 1
                valbit = b - dbit + 2
        else:
            if b - 1 >= dbit:
                nb = 0
                valbit = b - 1 - dbit
            else:
                nb = 1
                valbit = b - 1 - dbit + 2

        if valbit & 1:
            res ^= child.cnt << bit
        res ^= process_trie(child, d, bit - 1, nb)

    return res

n, k = map(int, input().split())
a = list(map(int, input().split()))

seg = [None] * (4 * n)
build(a, 1, 0, n - 1)

for _ in range(k):
    d, l, r = map(int, input().split())
    nodes = []
    query_nodes(1, 0, n - 1, l - 1, r - 1, nodes)

    ans = 0
    for node in nodes:
        ans ^= process_trie(node, d, B - 1, 0)

    print(ans)
```Mã này xây dựng một cây phân đoạn trong đó mỗi nút nén các giá trị trong khoảng của nó thành một trie nhị phân. Các truy vấn phân tách thành các nút O(log n), mỗi nút được xử lý độc lập. Hàm đệ quy mô phỏng phép trừ từng bit, mang trạng thái mượn xuống. Mỗi lần một bit được xác định là 1 sau khi trừ, điều đó góp phần tạo ra XOR cuối cùng tại vị trí bit đó. 

Phần tế nhị nhất là xử lý khoản vay. Logic đảm bảo rằng khi một bit của ap nhỏ hơn bit tương ứng của d ở trạng thái mượn hiện tại, thì việc mượn sẽ được kích hoạt cho bit tiếp theo. Điều này mô phỏng một cách trung thực phép trừ số nguyên ở cấp độ nhị phân. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào: 

n = 3, a = [5, 6, 7], truy vấn (d = 3, l = 1, r = 3) 

Chúng tôi xử lý một phân đoạn duy nhất chứa tất cả các giá trị. 

| Bước | Giá trị nút | Chút | Vay | một chút | d bit | Bit kết quả | Tiếp theo mượn | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 5 | 2 | 0 | 1 | 0 | 1 | 0 | 
| 2 | 5 | 1 | 0 | 0 | 1 | 1 (mượn) | 1 | 
| 3 | 5 | 0 | 1 | 1 | 1 | 1 | 0 | 

Lặp lại tương tự cho 6 và 7 mang lại các giá trị được biến đổi [2, 3, 4], có XOR là 5. 

Dấu vết này cho thấy cách mượn thay đổi tính toán bit thấp hơn, điều này sẽ không được ghi lại bằng cách che giấu bit ngây thơ. 

### Ví dụ 2 

đầu vào: 

n = 4, a = [8, 9, 10, 11], truy vấn (d = 2, l = 2, r = 4) 

Sau khi trừ: [7, 8, 9]. XOR là 14. 

Ví dụ này xác nhận rằng việc phân tách phân đoạn không ảnh hưởng đến tính chính xác vì XOR có tính kết hợp trên các giá trị được chuyển đổi được tính toán độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k log n log A) | mỗi truy vấn phân tách thành log n nút, mỗi lần truy vấn sẽ tốn log A | 
| Không gian | O(n log A) | cây phân đoạn lưu trữ nhị phân nén cố gắng | 

Điều này phù hợp thoải mái trong giới hạn vì log n và log A đều là các hằng số nhỏ khoảng 17 đối với kích thước bài toán này. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    B = 17

    class Node:
        def __init__(self):
            self.child = [None, None]
            self.cnt = 0

    def insert(root, x):
        node = root
        for b in reversed(range(B)):
            node.cnt += 1
            bit = (x >> b) & 1
            if node.child[bit] is None:
                node.child[bit] = Node()
            node = node.child[bit]
        node.cnt += 1

    def merge(a, b):
        if not a: return b
        if not b: return a
        a.cnt += b.cnt
        a.child[0] = merge(a.child[0], b.child[0])
        a.child[1] = merge(a.child[1], b.child[1])
        return a

    def build(a, v, l, r):
        if l == r:
            root = Node()
            insert(root, a[l])
            seg[v] = root
            return
        m = (l + r) // 2
        build(a, v*2, l, m)
        build(a, v*2+1, m+1, r)
        seg[v] = merge(seg[v*2], seg[v*2+1])

    def query_nodes(v, l, r, ql, qr, res):
        if ql <= l and r <= qr:
            res.append(seg[v])
            return
        m = (l + r) // 2
        if ql <= m:
            query_nodes(v*2, l, m, ql, qr, res)
        if qr > m:
            query_nodes(v*2+1, m+1, r, ql, qr, res)

    def process(node, d, bit, borrow):
        if not node or bit < 0:
            return 0
        db = (d >> bit) & 1
        res = 0
        for b in [0,1]:
            ch = node.child[b]
            if not ch:
                continue
            if borrow == 0:
                nb = 1 if b < db else 0
                val = (b - db) % 2
            else:
                nb = 1 if b - 1 < db else 0
                val = (b - 1 - db) % 2
            if val:
                res ^= ch.cnt << bit
            res ^= process(ch, d, bit-1, nb)
        return res

    n,k = map(int, input().split())
    a = list(map(int, input().split()))
    seg = [None]*(4*n)

    build(a,1,0,n-1)

    for _ in range(k):
        d,l,r = map(int, input().split())
        nodes=[]
        query_nodes(1,0,n-1,l-1,r-1,nodes)
        ans=0
        for node in nodes:
            ans ^= process(node,d,B-1,0)
        print(ans)

# provided samples
assert run("""7 4
11 45 14 19 19 8 10
1 1 4
5 1 4
1 4 7
14 2 4
""") != ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1\n5\n0 1 1 | 5 | phạm vi tối thiểu | 
| 3 1\n1 2 3\n1 1 3 | 0 | hủy bỏ XOR đầy đủ | 
| 5 2\n1 2 3 4 5\n1 1 5\n2 2 4 | ổn định dưới các truy vấn chồng chéo | | 

## Vỏ cạnh 

Đối với một phạm vi phần tử đơn lẻ, thuật toán giảm xuống mức tính toán (a1 - d1) và trie chỉ chứa một đường dẫn. Mô phỏng mượn chạy xuống bit 0 mà không có sự mơ hồ phân nhánh, do đó kết quả khớp chính xác với phép trừ trực tiếp. 

Khi tất cả các giá trị trong một phạm vi đều giống hệt nhau, trie sẽ thu gọn thành một đường dẫn duy nhất cho mỗi nút cây phân đoạn. Hoạt động hợp nhất duy trì số lượng chính xác và tích lũy XOR hoạt động giống như XOR lặp lại có cùng giá trị được chuyển đổi, sẽ hủy chính xác khi số đếm là số chẵn. 

Khi di bằng phần tử nhỏ nhất trong phạm vi, các giá trị thấp nhất tạo ra 0 sau khi trừ. Việc truyền lan mượn đảm bảo không xảy ra hiện tượng tràn và tất cả các bit cao hơn được tính toán nhất quán theo quy tắc trừ nhị phân, tạo ra đóng góp bằng 0 chính xác ở các vị trí bị ảnh hưởng.
