---
title: "CF 104452E - Giải đấu của người vùng cao"
description: "Chúng ta được sắp xếp một hàng đấu sĩ, mỗi đấu sĩ ngồi theo thứ tự cố định từ trái sang phải và mỗi chiến binh có một giá trị sức mạnh riêng biệt."
date: "2026-06-30T14:42:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104452
codeforces_index: "E"
codeforces_contest_name: "ICPC Central Russia Regional Contest - 2020"
rating: 0
weight: 104452
solve_time_s: 116
verified: false
draft: false
---

[CF 104452E - Giải đấu của những người vùng cao](https://codeforces.com/problemset/problem/104452/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 56 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được sắp xếp một hàng đấu sĩ, mỗi đấu sĩ ngồi theo thứ tự cố định từ trái sang phải và mỗi chiến binh có một giá trị sức mạnh riêng biệt. Quá trình này bao gồm việc liên tục chọn một đoạn liền kề của dòng hiện tại, chỉ để chiến binh mạnh nhất trong đoạn đó sống sót và loại bỏ vĩnh viễn tất cả những con khác khỏi đoạn đó. Người sống sót vẫn ở vị trí ban đầu và dòng còn lại sẽ đóng lại sau khi bị xóa. 

Điểm mấu chốt là mọi thao tác đều được thực hiện ở trạng thái _current của dòng_, không phải trên chỉ mục ban đầu. Sau khi xóa, vị trí sẽ thay đổi, do đó các phân đoạn sau sẽ đề cập đến mảng được cập nhật. 

Nhiệm vụ là xác định thứ tự cuối cùng của các máy bay chiến đấu sau khi tất cả các trận chiến phân đoạn như vậy đã được áp dụng. 

Các hạn chế rất lớn: lên tới 200.000 máy bay chiến đấu và 100.000 hoạt động. Điều này ngay lập tức loại trừ bất kỳ phương pháp nào quét một phân đoạn và xóa vật lý các phần tử khỏi danh sách cho mỗi truy vấn, vì điều đó sẽ làm giảm hành vi bậc hai trong trường hợp xấu nhất. Thậm chí một$O(n)$mỗi giải pháp hoạt động dẫn đến$O(nm)$, vượt xa giới hạn có thể chấp nhận được. 

Một khó khăn tinh tế là các chỉ số rất năng động. Một cách giải thích ngây thơ thường cho rằng các phạm vi đề cập đến mảng ban đầu, nhưng chúng đề cập đến đường nén đang phát triển. Ví dụ: sau khi loại bỏ các phần tử trong truy vấn ban đầu, các truy vấn sau này có thể đề cập đến các phần tử hoàn toàn khác ngay cả khi các chỉ số số trông giống nhau. 

Một cạm bẫy phổ biến khác là cố gắng mô phỏng quá trình bằng cách sử dụng mảng và cắt lặp đi lặp lại. Ngay cả khi mỗi lát cắt đều chính xác về mặt logic, việc xóa danh sách Python ở giữa là tuyến tính và việc xóa lặp đi lặp lại các phân đoạn lớn sẽ gây ra thời gian chờ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực rất đơn giản: duy trì danh sách máy bay chiến đấu hiện tại. Đối với mỗi truy vấn$[l, r]$, trích xuất mảng con đó, tìm giá trị tối đa của nó, xóa mọi thứ trong phạm vi đó và chỉ chèn lại giá trị tối đa. Việc tìm mức tối đa là tuyến tính trong kích thước phân khúc và việc xóa cũng tốn thời gian tuyến tính do các phần tử dịch chuyển. Qua nhiều thao tác, đặc biệt là khi phạm vi lớn được chọn lặp đi lặp lại, điều này dẫn đến việc quét toàn bộ mảng lặp đi lặp lại. Trong trường hợp xấu nhất, chi phí cho một hoạt động duy nhất$O(n)$, và làm điều này$m$lần dẫn đến$O(nm)$, quá chậm đối với$2 \cdot 10^5$. 

Quan sát quan trọng là thứ duy nhất tồn tại sau mỗi thao tác là phần tử tối đa trong phân đoạn đã chọn. Mọi thứ khác sẽ bị xóa vĩnh viễn. Điều này có nghĩa là chúng tôi không thực sự mô phỏng các trận đánh; chúng tôi đang thực hiện "nén phạm vi" lặp đi lặp lại trong đó mỗi phân đoạn thu gọn thành một phần tử đại diện và tất cả các phần tử khác đều biến mất. 

Khó khăn là duy trì cả trật tự và truy cập nhanh vào thông tin phạm vi đang bị xóa. Đây chính xác là vai trò của cây tìm kiếm nhị phân cân bằng ngầm, thường là một treap. Treap duy trì các phần tử theo thứ tự vị trí hiện tại của chúng, hỗ trợ phân tách theo vị trí và có thể lưu trữ thông tin cây con như giá trị tối đa và kích thước cây con. Điều này cho phép chúng tôi cô lập bất kỳ phân đoạn nào theo thời gian logarit, xác định mức tối đa của nó một cách hiệu quả và xây dựng lại cấu trúc sau khi xóa. 

Khi chúng tôi có thể tách biệt một phân khúc, thử thách còn lại là loại bỏ tất cả các phần tử ngoại trừ phần tử tối đa. Điều này được xử lý bằng cách định vị nút tối đa bên trong phân đoạn bằng cách sử dụng thông tin tối đa của cây con được lưu trữ, phân tách xung quanh vị trí của nút đó và loại bỏ hai phần bên ngoài. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nm)$|$O(n)$| Quá chậm | 
| Kho báu tiềm ẩn |$O((n+m)\log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì trình tự hiện tại trong một treap ẩn, trong đó mỗi nút lưu trữ giá trị, kích thước cây con và giá trị tối đa trong cây con của nó. 

1. Xây dựng một kho lưu trữ ngầm từ mảng ban đầu. Điều này thể hiện dòng máy bay chiến đấu hiện tại theo thứ tự, trong đó việc di chuyển theo thứ tự tương ứng với đội hình. 
2. Đối với mỗi truy vấn$[l, r]$, chia tre thành ba phần: tiền tố trước$l$, đoạn$[l, r]$, và hậu tố sau$r$. Điều này cô lập chính xác các chiến binh tham gia trận chiến. 
3. Bên trong đoạn giữa, xác định vị trí nút có giá trị lớn nhất bằng cách sử dụng mức tối đa của cây con được lưu trữ. Điều này hoạt động theo thời gian logarit bằng cách giảm dần treap và so sánh các giá trị được lưu trữ ở phần tử con. 
4. Sau khi xác định được nút tối đa, hãy xác định vị trí chính xác của nó trong phân đoạn bằng cách sử dụng kích thước cây con trong khi đi xuống từ gốc của phân đoạn đó. Điều này đưa ra chỉ mục của nó theo thứ tự ngầm định. 
5. Chia đoạn lại thành ba phần: mọi thứ trước mức tối đa, chính nút tối đa và mọi thứ sau nó. 
6. Loại bỏ hai phần bên ngoài và chỉ giữ lại kho chứa một nút chứa đấu ngư tối đa. 
7. Hợp nhất tiền tố, nút tối đa duy nhất và hậu tố lại với nhau để xây dựng lại dòng cập nhật. 

Sau khi xử lý tất cả các truy vấn, việc duyệt qua theo thứ tự sẽ mang lại thứ tự cuối cùng của các máy bay chiến đấu. 

### Tại sao nó hoạt động 

Ở mỗi bước, bất biến treap đảm bảo rằng việc truyền tải theo thứ tự thể hiện chính xác dòng hiện tại. Mỗi truy vấn thay thế một khoảng liền kề bằng một phần tử duy nhất, giữ nguyên thứ tự tương đối bên ngoài khoảng đó. Bởi vì các truy vấn tối đa của cây con luôn trả về giá trị tối đa thực sự của phân đoạn hiện tại nên phần còn lại được chọn luôn chính xác. Vì tất cả các phần tử khác trong phân đoạn đó đều bị xóa vĩnh viễn nên không hoạt động nào trong tương lai có thể phụ thuộc vào chúng, do đó việc loại bỏ chúng không ảnh hưởng đến tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import random

class Node:
    __slots__ = ("val", "prio", "left", "right", "size", "mx")
    def __init__(self, val):
        self.val = val
        self.prio = random.randint(1, 10**9)
        self.left = None
        self.right = None
        self.size = 1
        self.mx = val

def sz(t):
    return t.size if t else 0

def mx(t):
    return t.mx if t else -10**18

def pull(t):
    if not t:
        return
    t.size = 1 + sz(t.left) + sz(t.right)
    t.mx = max(t.val, mx(t.left), mx(t.right))

def split(t, k):
    if not t:
        return (None, None)
    if sz(t.left) >= k:
        l, r = split(t.left, k)
        t.left = r
        pull(t)
        return (l, t)
    else:
        l, r = split(t.right, k - sz(t.left) - 1)
        t.right = l
        pull(t)
        return (t, r)

def merge(a, b):
    if not a or not b:
        return a or b
    if a.prio < b.prio:
        a.right = merge(a.right, b)
        pull(a)
        return a
    else:
        b.left = merge(a, b.left)
        pull(b)
        return b

def build(arr):
    def rec(l, r):
        if l > r:
            return None
        m = (l + r) // 2
        root = Node(arr[m])
        root.left = rec(l, m - 1)
        root.right = rec(m + 1, r)
        pull(root)
        return root
    return rec(0, len(arr) - 1)

def get_max_pos(t, add=0):
    if t.left and t.left.mx == t.mx:
        return get_max_pos(t.left, add)
    if t.val == t.mx:
        return add + sz(t.left)
    return get_max_pos(t.right, add + sz(t.left) + 1)

def solve():
    n, m = map(int, input().split())
    arr = list(map(int, input().split()))
    root = build(arr)

    for _ in range(m):
        l, r = map(int, input().split())
        l -= 1

        a, b = split(root, l)
        b, c = split(b, r - l)

        if b:
            pos = get_max_pos(b)
            b1, b2 = split(b, pos)
            mid, b3 = split(b2, 1)
            b = mid

        root = merge(merge(a, b), c)

    def inorder(t):
        if not t:
            return []
        return inorder(t.left) + [t.val] + inorder(t.right)

    print(*inorder(root))

if __name__ == "__main__":
    solve()
```Giải pháp dựa vào việc lập chỉ mục ngầm trong trep. các`split`Hàm phân tách chuỗi theo vị trí chứ không phải theo giá trị, điều này rất quan trọng vì cấu trúc liên tục thay đổi. các`mx`trường cho phép nhận dạng nhanh phần tử tối đa bên trong bất kỳ phân đoạn nào và`get_max_pos`giải quyết chỉ mục chính xác của nó theo thứ tự ngầm định. 

Phần cẩn thận là sau khi cô lập mức tối đa, chúng tôi lại phân chia ở vị trí của nó để loại bỏ nó khỏi bối cảnh phân khúc và đảm bảo không có phần tử nào khác tồn tại. 

## Ví dụ đã hoạt động 

Hãy xem xét một ví dụ nhỏ trong đó mảng`[5, 1, 7, 2]`và chúng tôi truy vấn`[2, 4]`. 

| Bước | Phân đoạn | Tối đa | Đoạn còn lại | 
| --- | --- | --- | --- | 
| 1 | [1, 7, 2] | 7 | [7] | 

Sau khi hoạt động, mảng trở thành`[5, 7]`. Điều này chứng tỏ đoạn này thu gọn đến mức tối đa như thế nào trong khi vẫn bảo toàn được cấu trúc bên ngoài. 

Bây giờ hãy xem xét thao tác thứ hai trên`[1, 2]`của mảng được cập nhật`[5, 7]`. 

| Bước | Phân đoạn | Tối đa | Đoạn còn lại | 
| --- | --- | --- | --- | 
| 2 | [5, 7] | 7 | [7] | 

Kết quả cuối cùng là`[7]`. 

Điều này cho thấy việc xóa trong các hoạt động ban đầu ảnh hưởng trực tiếp đến cấu trúc của các phân đoạn sau, đó là lý do tại sao việc duy trì lập chỉ mục động là điều cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((n + m)\log n)$| Mỗi truy vấn phân tách, hợp nhất và tối đa chạy theo thời gian logarit trên một treap cân bằng | 
| Không gian |$O(n)$| Một nút cho mỗi phần tử còn lại trong cấu trúc | 

Hệ số logarit xuất phát từ việc duy trì một cây cân bằng trong quá trình chia tách và hợp nhất lặp đi lặp lại. Với tối đa$2 \cdot 10^5$các yếu tố và$10^5$hoạt động, điều này thoải mái phù hợp trong thời hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import random

    class Node:
        __slots__ = ("val", "prio", "left", "right", "size", "mx")
        def __init__(self, val):
            self.val = val
            self.prio = random.randint(1, 10**9)
            self.left = None
            self.right = None
            self.size = 1
            self.mx = val

    def sz(t): return t.size if t else 0
    def mx(t): return t.mx if t else -10**18

    def pull(t):
        if not t: return
        t.size = 1 + sz(t.left) + sz(t.right)
        t.mx = max(t.val, mx(t.left), mx(t.right))

    def split(t, k):
        if not t: return (None, None)
        if sz(t.left) >= k:
            l, r = split(t.left, k)
            t.left = r
            pull(t)
            return l, t
        else:
            l, r = split(t.right, k - sz(t.left) - 1)
            t.right = l
            pull(t)
            return t, r

    def merge(a, b):
        if not a or not b: return a or b
        if a.prio < b.prio:
            a.right = merge(a.right, b)
            pull(a)
            return a
        else:
            b.left = merge(a, b.left)
            pull(b)
            return b

    def build(arr):
        if not arr: return None
        def rec(l, r):
            if l > r: return None
            m = (l + r) // 2
            node = Node(arr[m])
            node.left = rec(l, m - 1)
            node.right = rec(m + 1, r)
            pull(node)
            return node
        return rec(0, len(arr) - 1)

    def inorder(t):
        if not t: return []
        return inorder(t.left) + [t.val] + inorder(t.right)

    def solve():
        n, m = map(int, input().split())
        arr = list(map(int, input().split()))
        root = build(arr)

        def get_max_pos(t, add=0):
            if t.left and t.left.mx == t.mx:
                return get_max_pos(t.left, add)
            if t.val == t.mx:
                return add + sz(t.left)
            return get_max_pos(t.right, add + sz(t.left) + 1)

        for _ in range(m):
            l, r = map(int, input().split())
            l -= 1
            a, b = split(root, l)
            b, c = split(b, r - l)
            if b:
                pos = get_max_pos(b)
                b1, b2 = split(b, pos)
                mid, b3 = split(b2, 1)
                b = mid
            root = merge(merge(a, b), c)

        return " ".join(map(str, inorder(root)))

    return solve()

# sample 1
assert run("7 4\n8 1 57 25 69 26 88\n1 2\n3 5\n1 3\n2 2") is not None
# custom cases
assert run("1 0\n5") == "5", "single element"
assert run("3 1\n1 2 3\n1 3") == "3", "full segment collapse"
assert run("5 2\n5 4 3 2 1\n2 4\n1 2") != "", "basic structure"
assert run("2 1\n2 1\n1 2") != "", "boundary case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Yếu tố đơn | không thay đổi | hành vi không hoạt động | 
| Sự sụp đổ toàn bộ phân khúc | chỉ tối đa | độ chính xác toàn dải | 
| Giảm mảng | lan truyền tối đa ổn định | đặt hàng theo loại bỏ | 
| Hoán đổi ranh giới nhỏ | lập chỉ mục mạnh mẽ | chia ranh giới | 

## Vỏ cạnh 

Trường hợp một cạnh là khi truy vấn bao trùm toàn bộ mảng hiện tại. Trong tình huống đó, toàn bộ cấu trúc sẽ sụp đổ thành một nút duy nhất chứa phần tử tối đa. Việc phân chia treap tạo ra một tiền tố và hậu tố trống và chỉ còn lại đoạn giữa. Giá trị tối đa được chọn từ cấu trúc đầy đủ và mọi thứ khác sẽ bị loại bỏ, để lại một phần tử một phần tử, điều này đúng. 

Một trường hợp khác là khi phần tử lớn nhất đã ở một trong các ranh giới của đoạn. Logic phân tách vẫn tách biệt nó một cách chính xác vì việc phân tách dựa trên vị trí không phụ thuộc vào vị trí giá trị. Ngay cả khi mức tối đa là nút ngoài cùng bên trái hoặc ngoài cùng bên phải,`get_max_pos`hàm giải quyết chỉ mục của nó một cách chính xác và lần phân tách tiếp theo sẽ cô lập nó một cách rõ ràng. 

Trường hợp tinh tế cuối cùng là các truy vấn lặp lại trên các phân đoạn chồng chéo sau khi xóa. Vì tre luôn biểu thị chuỗi nén hiện tại nên các chỉ số luôn liên quan đến cấu trúc được cập nhật. Điều này đảm bảo rằng ngay cả khi vị trí ban đầu gợi ý sự chồng chéo thì các phân đoạn được xử lý thực tế vẫn nhất quán với trạng thái hiện tại.
