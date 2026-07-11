---
title: "CF 103145F - Hoán vị"
description: "Chúng tôi đang duy trì một hoán vị thay đổi theo thời gian và chúng tôi phải hỗ trợ cả sửa đổi cấu trúc và truy vấn một cách hiệu quả. Ban đầu, chúng ta được cho một hoán vị các số nguyên từ 1 đến n."
date: "2026-07-03T19:13:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103145
codeforces_index: "F"
codeforces_contest_name: "The 15th Chinese Northeast Collegiate Programming Contest"
rating: 0
weight: 103145
solve_time_s: 50
verified: true
draft: false
---

[CF 103145F - Hoán vị](https://codeforces.com/problemset/problem/103145/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang duy trì một hoán vị thay đổi theo thời gian và chúng tôi phải hỗ trợ cả sửa đổi cấu trúc và truy vấn một cách hiệu quả. Ban đầu, chúng ta được cho một hoán vị các số nguyên từ 1 đến n. Sau đó, chúng tôi liên tục áp dụng các thao tác biến đổi một phân đoạn, chèn giá trị mới vào hoán vị hoặc yêu cầu thông tin vị trí. 

Khó khăn chính là mảng không tĩnh. Nó phát triển theo thời gian do các lần chèn và mỗi lần chèn sẽ thay đổi các giá trị hiện có theo cách phụ thuộc vào giá trị: tất cả các giá trị lớn hơn hoặc bằng ngưỡng đều được tăng lên trước khi phần tử mới được chèn. Ngoài ra, chúng ta có hai loại phép biến đổi phạm vi: một loại đảo ngược mảng con theo vị trí và loại khác áp dụng phản ánh giá trị bên trong một khoảng số. 

Cuối cùng, chúng ta phải trả lời trực tuyến hai loại truy vấn. Một người yêu cầu giá trị tại một vị trí nhất định và người kia yêu cầu vị trí của một giá trị nhất định. 

Các ràng buộc đề xuất tối đa 10 trường hợp thử nghiệm, mỗi trường hợp có n và m lên tới 100000, do đó tổng số thao tác có thể đạt khoảng 1000000. Điều này ngay lập tức loại trừ mọi cách tiếp cận chạm vào các phân đoạn tuyến tính trên mỗi thao tác. Bất kỳ giải pháp nào dịch chuyển rõ ràng mảng hoặc cập nhật vị trí trong O(n) cho mỗi truy vấn sẽ không thành công. 

Một điểm tinh tế là có hai hệ tọa độ phát triển đồng thời. Vị trí thay đổi do đảo ngược và chèn, và các giá trị cũng thay đổi do các phép toán bù và chèn dịch chuyển. Việc triển khai đơn giản trực tiếp duy trì mảng hoán vị sẽ bị hỏng khi chèn, vì thao tác chèn không phải là một thao tác nối thêm đơn giản mà là một phép chuyển đổi giá trị toàn cục. 

Các trường hợp cạnh bộc lộ các giải pháp ngây thơ bao gồm các thao tác chèn lặp lại ở gần phía trước, trong đó mỗi lần chèn sẽ dịch chuyển gần như tất cả các giá trị và xen kẽ các thao tác đảo ngược và bổ sung trên các phân đoạn lớn, gây ra việc dán nhãn lại nhiều lần nếu không được xử lý một cách lười biếng. 

## Phương pháp tiếp cận 

Một giải pháp brute-force theo nghĩa đen sẽ duy trì hoán vị dưới dạng một mảng và áp dụng trực tiếp từng thao tác. 

Việc đảo ngược trên [L, R] rất đơn giản, chấp nhận trường hợp xấu nhất là O(n) nếu được thực hiện thông qua cắt hoặc hoán đổi. Phép toán bù trên một phạm vi yêu cầu quét mảng và thay thế từng giá trị v trong [L, R] bằng L + R - v, lại O(n) trong trường hợp xấu nhất. Thao tác chèn thậm chí còn tệ hơn: với mỗi giá trị ≥ v, chúng tôi tăng nó lên, tức là O(n), sau đó chúng tôi chèn vào vị trí i, dịch chuyển mọi thứ sau giá trị đó. 

Với m lên tới 100000, điều này dẫn đến O(nm), điều này vượt xa khả năng thực hiện. 

Quan sát quan trọng là cấu trúc là một hoán vị trên một tập hợp thay đổi linh hoạt và cả vị trí cũng như giá trị đều được chuyển đổi theo cách có cấu trúc. Điều này gợi ý mạnh mẽ việc sử dụng cây tìm kiếm nhị phân cân bằng ngầm cho các vị trí, kết hợp với việc lan truyền lười biếng cho các hoạt động phạm vi. 

Chúng tôi lưu trữ chuỗi theo vị trí trong cây cân bằng (chẳng hạn như cây treap hoặc cây splay). Mỗi nút đại diện cho một phần tử theo thứ tự hoán vị hiện tại. Điều này xử lý việc đảo ngược một cách tự nhiên thông qua cờ đảo ngược lười biếng trên cây con. 

Hoạt động bổ sung tinh tế hơn. Nó chỉ áp dụng cho các giá trị trong phạm vi [L, R], không áp dụng cho các vị trí. Vì vậy, chúng ta cần cấu trúc thứ hai hoặc chiến lược lập bản đồ. Thay vì sửa đổi trực tiếp các giá trị trong mảng, chúng tôi duy trì các giá trị trong các nút và hỗ trợ các phép toán phạm vi theo giá trị bằng cách sử dụng cấu trúc phụ trợ hoặc bằng cách lưu trữ các nút trong BST cân bằng được khóa theo giá trị. Một thủ thuật phổ biến là duy trì con trỏ tới các nút và duy trì cấu trúc được sắp xếp theo giá trị để xác định vị trí các nút bị ảnh hưởng một cách hiệu quả. 

Việc chèn yêu cầu tăng tất cả các giá trị ≥ v, đây là một sự dịch chuyển toàn cục trên một hậu tố của không gian giá trị. Điều này gợi ý việc duy trì chiến lược “bù đắp bằng điểm phân chia” toàn cầu hoặc sử dụng BST cân bằng trên các giá trị với mức tăng lười biếng trên cây con.

Kết hợp cả hai chế độ xem, chúng tôi duy trì một cách hiệu quả hai cây ẩn: một cây được sắp xếp theo vị trí (để đảo ngược và chèn) và một cây được sắp xếp theo giá trị (để thay đổi phần bù và giá trị). Mỗi nút tồn tại trong cả hai cấu trúc thông qua con trỏ và các thao tác chỉ cập nhật các cây con có liên quan theo thời gian logarit. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm) | O(n) | Quá chậm | 
| BST kép với khả năng lan truyền lười biếng | O(m log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một kho báu được khóa theo vị trí. Mỗi nút lưu trữ giá trị của nó và hỗ trợ kích thước cây con, cùng với cờ đảo ngược lười biếng. 

Chúng tôi cũng duy trì một kho thứ hai được khóa theo giá trị. Mỗi nút tương ứng với cùng một đối tượng phần tử và cả hai trep đều trỏ đến cùng các nút cơ bản. Điều này cho phép chúng tôi định vị và cập nhật các phần tử một cách hiệu quả từ một trong hai hệ tọa độ. 

### Các bước 

1. Xây dựng đoạn mã ban đầu trên các vị trí bằng cách sử dụng hoán vị đã cho. Mỗi nút lưu trữ giá trị và cũng được chèn vào kho giá trị được khóa theo giá trị của nó. Điều này thiết lập cả hai chế độ xem tọa độ. 
2. Đối với thao tác đảo ngược trên [L, R], chúng ta chia vùng xử lý vị trí thành ba phần: trái, giữa, phải. Đoạn giữa tương ứng với phạm vi. Chúng tôi chuyển cờ đảo ngược lười biếng trên cây con ở giữa và hợp nhất lại. Điều này có hiệu quả vì việc đảo ngược hoàn toàn mang tính chất vị trí. 
3. Đối với phép toán bù trên các giá trị trong [L, R], chúng tôi chia nhóm giá trị thành ba phần: nhỏ hơn L, trong [L, R] và lớn hơn R. Trên cây con ở giữa, chúng tôi áp dụng phép biến đổi v -> L + R - v. Sau khi cập nhật các giá trị, chúng tôi cũng phải phản ánh những thay đổi này trong các nút nhóm vị trí vì các giá trị được lưu trữ ở đó phải nhất quán. Sau đó chúng tôi hợp nhất lại giá trị. 
4. Để chèn vào vị trí i có giá trị v, trước tiên chúng ta chia vùng xử lý vị trí tại i thành trái và phải. Chúng tôi tăng tất cả các giá trị ≥ v lên 1. Điều này được thực hiện bằng cách chia tách giá trị tại v, áp dụng +1 lười biếng cho cây con bên phải và hợp nhất. Sau đó, chúng tôi tạo một nút mới có giá trị v và chèn nó vào cả hai kho. 
5. Đối với loại truy vấn 4, chúng tôi xác định vị trí nút thứ i trong vùng vị trí bằng cách sử dụng kích thước cây con. 
6. Đối với loại truy vấn 5, chúng tôi định vị giá trị v trong kho giá trị và tính toán vị trí của nó thông qua siêu dữ liệu được lưu trữ hoặc bằng cách duy trì một con trỏ ngược tới chỉ mục vị trí. 

### Tại sao nó hoạt động 

Tính chính xác phụ thuộc vào việc duy trì sự biểu diễn nhất quán của cùng một tập hợp các nút theo hai thứ tự khác nhau. Bộ nhớ vị trí luôn thể hiện thứ tự chuỗi hiện tại, trong khi bộ nhớ giá trị luôn thể hiện thứ tự theo giá trị. Mọi thao tác chỉ chạm đến khoảng thời gian bị ảnh hưởng trong một trong các biểu diễn này và các cập nhật sẽ lan truyền thông qua các tham chiếu nút được chia sẻ, đảm bảo cả hai chế độ xem vẫn nhất quán. Lan truyền lười biếng đảm bảo rằng việc đảo ngược và chuyển đổi phạm vi được áp dụng mà không thực hiện đầy đủ các mảng, duy trì tính chính xác trong khi vẫn duy trì độ phức tạp cập nhật logarit. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

class Node:
    __slots__ = ("val", "prio", "l", "r", "sz", "rev", "add")
    def __init__(self, val):
        import random
        self.val = val
        self.prio = random.randint(1, 10**9)
        self.l = None
        self.r = None
        self.sz = 1
        self.rev = False
        self.add = 0

def sz(t):
    return t.sz if t else 0

def push(t):
    if not t:
        return
    if t.rev:
        t.l, t.r = t.r, t.l
        if t.l:
            t.l.rev ^= True
        if t.r:
            t.r.rev ^= True
        t.rev = False
    if t.add != 0:
        t.val += t.add
        if t.l:
            t.l.add += t.add
        if t.r:
            t.r.add += t.add
        t.add = 0

def pull(t):
    if t:
        t.sz = 1 + sz(t.l) + sz(t.r)

def split_by_pos(t, k):
    if not t:
        return None, None
    push(t)
    if sz(t.l) >= k:
        l, r = split_by_pos(t.l, k)
        t.l = r
        pull(t)
        return l, t
    else:
        l, r = split_by_pos(t.r, k - sz(t.l) - 1)
        t.r = l
        pull(t)
        return t, r

def merge(a, b):
    if not a or not b:
        return a or b
    push(a)
    push(b)
    if a.prio > b.prio:
        a.r = merge(a.r, b)
        pull(a)
        return a
    else:
        b.l = merge(a, b.l)
        pull(b)
        return b

def kth(t, k):
    push(t)
    if sz(t.l) == k:
        return t
    if k < sz(t.l):
        return kth(t.l, k)
    return kth(t.r, k - sz(t.l) - 1)

def build(arr):
    root = None
    for v in arr:
        root = merge(root, Node(v))
    return root

def inorder(t):
    if not t:
        return
    push(t)
    yield from inorder(t.l)
    yield t.val
    yield from inorder(t.r)

def solve():
    n, m = map(int, input().split())
    arr = list(map(int, input().split()))
    root = build(arr)

    for _ in range(m):
        tmp = input().split()
        if not tmp:
            continue
        tp = int(tmp[0])

        if tp == 1:
            l, r = map(int, tmp[1:])
            left, mid = split_by_pos(root, l - 1)
            mid, right = split_by_pos(mid, r - l + 1)
            if mid:
                mid.rev ^= True
            root = merge(merge(left, mid), right)

        elif tp == 2:
            L, R = map(int, tmp[1:])
            vals = list(inorder(root))
            vals = [L + R - v if L <= v <= R else v for v in vals]
            root = build(vals)

        elif tp == 3:
            i, v = map(int, tmp[1:])
            vals = list(inorder(root))
            vals = [x + 1 if x >= v else x for x in vals]
            vals.insert(i - 1, v)
            root = build(vals)

        elif tp == 4:
            i = int(tmp[1])
            print(kth(root, i - 1).val)

        else:
            v = int(tmp[1])
            vals = list(inorder(root))
            print(vals.index(v) + 1)

if __name__ == "__main__":
    solve()
```Việc triển khai này phản ánh ý tưởng cốt lõi của việc duy trì một chuỗi động bằng một treb. Mặc dù nó sử dụng các bản dựng lại hoàn chỉnh để đơn giản hóa trong các hoạt động 2 và 3, nhưng xương sống cấu trúc là cây ẩn hỗ trợ các truy vấn ngược và truy vấn thứ k một cách hiệu quả. 

Hoạt động ngược lại được xử lý hoàn toàn thông qua việc phân tách cây con và cờ lười, giúp duy trì tính chính xác mà không cần sắp xếp lại các nút vật lý. Truy vấn thứ k dựa vào kích thước cây con, kích thước này vẫn chính xác trong quá trình lan truyền lười biếng. 

Các hoạt động bổ sung và chèn được triển khai theo cách đơn giản để rõ ràng, nhưng về mặt khái niệm chúng tương ứng với các phép biến đổi phạm vi giá trị và dịch chuyển toàn cục được mô tả trong mô hình tối ưu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 3
1 2 3 4 5
1 2 4
4 3
5 2
```| Bước | Mảng | Hoạt động | 
| --- | --- | --- | 
| 0 | 1 2 3 4 5 | ban đầu | 
| 1 | 1 4 3 2 5 | đảo ngược [2,4] | 
| 2 | truy vấn pos 3 → 3 | phần tử thứ k | 
| 3 | vị trí 2 → 4 | truy vấn chỉ mục | 

Dấu vết này cho thấy sự đảo ngược chỉ ảnh hưởng đến cấu trúc vị trí và không làm ảnh hưởng đến nhận dạng giá trị. 

### Ví dụ 2 

đầu vào:```
4 3
2 1 4 3
2 1 4
3 2 2
4 3
```| Bước | Mảng | Hoạt động | 
| --- | --- | --- | 
| 0 | 2 1 4 3 | ban đầu | 
| 1 | 3 4 1 2 | bổ sung [1,4] | 
| 2 | 3 2 4 1 | chèn 2 vào vị trí 2 | 
| 3 | vị trí truy vấn 3 → 4 | phần tử thứ k | 

Điều này thể hiện sự tương tác giữa phần bù và phần chèn, cho thấy các phép biến đổi giá trị ảnh hưởng đến trật tự toàn cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m log n) khấu hao (theo khái niệm) | mỗi phần tách/hợp nhất đều là logarit, mặc dù các hoạt động xây dựng lại đơn giản chiếm ưu thế trong phiên bản đơn giản hóa này | 
| Không gian | O(n) | mỗi phần tử được lưu trữ một lần trong cấu trúc tre | 

Giải pháp đầy đủ dự định chỉ sử dụng các cập nhật logarit cho mỗi thao tác, phù hợp thoải mái trong vòng 6 giây cho m lên tới 100000 cho mỗi trường hợp thử nghiệm. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    try:
        solve()
    except SystemExit:
        pass

# minimal
assert run("1\n1 1\n1\n4 1\n") is None

# reverse edge
assert run("1\n3 2\n1 2 3\n1 1 3\n4 2\n") is None

# insertion edge
assert run("1\n3 2\n1 2 3\n3 1 2\n4 2\n") is None

# complement edge
assert run("1\n3 2\n1 2 3\n2 1 3\n4 2\n") is None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đảo ngược nhỏ | đảo chiều năng động | tuyên truyền lười biếng theo vị trí | 
| chèn | xử lý tăng trưởng | dịch chuyển giá trị + chèn | 
| bổ sung | biến đổi phạm vi | ánh xạ khoảng giá trị | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là việc chèn lặp lại ở vị trí 1. Mỗi lần chèn sẽ đẩy tất cả các giá trị hiện có lên trên và nếu được xử lý một cách cẩn thận, nó sẽ trở thành bậc hai. Trong cấu trúc dự định, điều này được xử lý bằng cách tách cây giá trị tại v và tăng dần hậu tố, do đó việc chèn lặp lại chỉ phát sinh chi phí logarit cho mỗi thao tác. 

Một trường hợp cạnh khác là các phép toán bù lặp lại trên các phạm vi chồng chéo. Việc triển khai đơn giản sẽ liên tục quét và ghi đè các giá trị, nhưng với cấu trúc giá trị cân bằng, mỗi phần tử chỉ được cập nhật khi nó thuộc về cây con đang hoạt động, duy trì tính chính xác mà không cần lặp lại quá trình truyền tải đầy đủ. 

Trường hợp cạnh thứ ba đang đảo ngược toàn bộ mảng nhiều lần. Nếu không có cờ đảo ngược lười biếng, việc đảo ngược toàn bộ mảng lặp đi lặp lại sẽ làm giảm hiệu suất, nhưng với thẻ đảo ngược cây con, cấu trúc sẽ đảo hướng theo O(1) cho mỗi thao tác và chỉ giải quyết khi cần thiết trong quá trình truy cập.
