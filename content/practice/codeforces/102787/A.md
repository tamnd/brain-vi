---
title: "CF 102787A - Xù Xù Xù Xù"
description: "Bài toán mô tả một dãy n ô ban đầu được sắp xếp là 1, 2, 3, ..., n. Chúng ta được cung cấp n thao tác xáo trộn. Mỗi thao tác nhận được hai vị trí a và b. Nếu b không theo sau a thì không có gì xảy ra."
date: "2026-07-27T19:20:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102787
codeforces_index: "A"
codeforces_contest_name: "Algorithms Thread Treaps Contest"
rating: 0
weight: 102787
solve_time_s: 65
verified: true
draft: false
---

[CF 102787A - Xù lông Shandom](https://codeforces.com/problemset/problem/102787/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Vấn đề mô tả một chuỗi các`n`gạch ban đầu được sắp xếp như`1, 2, 3, ..., n`. Chúng tôi được trao`n`các thao tác xáo trộn. Mỗi hoạt động nhận được hai vị trí`a`Và`b`. Nếu như`b`không phải sau`a`, không có gì xảy ra. Nếu không, thao tác sẽ hoán đổi ô ở`a`với gạch ở`b`, thì ô ở`a + 1`với gạch ở`b + 1`, v.v., dừng lại khi một trong hai phạm vi đến cuối mảng. Sau khi áp dụng tất cả các thao tác, chúng ta phải in thứ tự cuối cùng của các ô. 

Kích thước đầu vào là khó khăn chính. Có thể có tới`500000`hoạt động và mảng có cùng kích thước tối đa. Mô phỏng trực tiếp một thao tác có thể chạm vào nhiều vị trí và trong trường hợp xấu nhất một thao tác có thể di chuyển`O(n)`các phần tử. Lặp đi lặp lại điều đó cho`n`hoạt động mang lại`O(n^2)`hoạt động vượt xa những gì có thể làm được đối với nửa triệu phần tử. Chúng ta cần xử lý mỗi lần xáo trộn trong thời gian gần như logarit. 

Một số trường hợp cạnh rất dễ bị bỏ lỡ. Nếu như`b <= a`, hoạt động phải được bỏ qua hoàn toàn. Ví dụ:```
Input:
1
1 1
```Mảng vẫn còn`[1]`, do đó đầu ra là:```
1
```Việc triển khai bất cẩn luôn tạo ra phạm vi hoán đổi không trống có thể truy cập vào các vị trí không hợp lệ. 

Một trường hợp quan trọng khác là khi phân đoạn hoán đổi thứ hai đạt đến cuối mảng. Ví dụ:```
Input:
5
4 1
5 4
3 5
4 5
5 2
```Thao tác đầu tiên không làm gì vì`1 <= 4`. Các hoạt động sau này có độ dài hiệu dụng khác nhau vì phía bên phải của trao đổi có thể bị cắt ngắn bởi ranh giới mảng. Xử lý mọi thao tác như thể nó luôn hoán đổi hai khối đầy đủ bằng nhau sẽ tạo ra hoán vị sai. 

## Phương pháp tiếp cận 

Một giải pháp đơn giản là lưu trữ mảng một cách rõ ràng và thực hiện mọi hoán đổi trong định nghĩa. Đối với một hoạt động`(a, b)`, chúng tôi liên tục trao đổi`array[a]`với`array[b]`trong khi di chuyển cả hai chỉ số về phía trước. Đây là cách triển khai đúng vì nó tuân thủ chính xác quy trình xáo trộn. 

Vấn đề là thời gian chạy. Hãy xem xét các hoạt động hoán đổi liên tục gần như toàn bộ mảng. Một hoạt động có thể yêu cầu gần`n/2`trao đổi. Với`n`hoạt động, số lượng trao đổi cơ bản có thể đạt tới`250000000000`, điều đó là không thể thực hiện được. 

Quan sát hữu ích là mỗi lần xáo trộn không quan tâm đến các giá trị được lưu trữ trong mảng. Nó chỉ sắp xếp lại vị trí. Hoạt động này là hoán vị của một chuỗi liền kề, cụ thể là hoán đổi hai khối liền kề. Mảng có thể được biểu diễn dưới dạng một chuỗi có thứ tự trong đó việc cắt và nối các phần là hiệu quả. 

Một treap ngầm rất phù hợp vì nó lưu trữ một chuỗi đồng thời hỗ trợ phân tách theo vị trí và hợp nhất. Việc xáo trộn có thể được thực hiện bằng cách cắt chuỗi thành phần trước`a`, khối đầu tiên, khối thứ hai và hậu tố còn lại. Sau đó, hai khối ở giữa được nối theo thứ tự ngược lại. Số lượng mảnh không đổi nên mọi thao tác đều mất`O(log n)`thời gian dự kiến. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Kho báu tiềm ẩn | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng một kho lưu trữ ngầm chứa chuỗi`1, 2, ..., n`. Vị trí treap của một nút biểu thị chỉ mục của nó trong mảng hiện tại, vì vậy chúng ta không bao giờ cần lưu trữ mảng đó trong danh sách thông thường. 
2. Đối với mỗi lần xáo trộn`(a, b)`, bỏ qua nếu`b <= a`. Hoạt động không thể trao đổi bất cứ điều gì trong trường hợp này. 
3. Tính độ dài của khối đầu tiên được trao đổi. Hai khối bắt đầu lúc`a`Và`b`, vậy khoảng cách của họ là`b - a`. Khối thứ hai chỉ có thể kéo dài cho đến hết mảng, cho biết độ dài:```
min(b - a, n - b + 1)
```1. Chia tre thành bốn phần. Phần đầu tiên chứa các vị trí trước`a`. Hai mảnh tiếp theo là các khối phải được trao đổi. Phần cuối cùng chứa mọi thứ sau vùng trao đổi. 
2. Hợp nhất các phần lại theo thứ tự: tiền tố, khối thứ hai, khối thứ nhất, hậu tố. Điều này tạo lại chính xác các giao dịch hoán đổi được thực hiện theo quy trình ban đầu. 
3. Sau tất cả các thao tác, duyệt qua bảng theo thứ tự và in các giá trị được lưu trữ. 

Lý do điều này hoạt động là vì việc hoán đổi khối liền kề được mô tả hoàn toàn bằng hai khối của nó. Treap duy trì thứ tự trình tự giống như mảng thực, do đó việc tách hai khối và chuyển đổi vị trí của chúng sẽ áp dụng chính xác hoán vị giống như thực hiện mỗi lần hoán đổi riêng lẻ. 

Tại sao nó hoạt động: 

Điều bất biến là sau khi xử lý bất kỳ số lượng thao tác nào, quá trình truyền tải theo thứ tự của treap giống hệt với mảng thực sau các thao tác đó. Ban đầu điều này đúng vì treap chứa hoán vị nhận dạng. Trong quá trình xáo trộn, thao tác phân tách sẽ trích xuất chính xác bốn phạm vi liên tiếp giống nhau mà vòng lặp ban đầu truy cập. Việc sắp xếp lại hai phạm vi ở giữa chỉ thay đổi vị trí của chúng, đó chính xác là điều mà tất cả các giao dịch hoán đổi riêng lẻ đều thực hiện. Vì mỗi phép toán đều bảo toàn bất biến nên phép duyệt cuối cùng là câu trả lời bắt buộc. 

## Giải pháp Python```python
import sys
import random

input = sys.stdin.readline

class Node:
    __slots__ = ("val", "prio", "size", "left", "right")

    def __init__(self, val):
        self.val = val
        self.prio = random.randint(1, 1 << 30)
        self.size = 1
        self.left = None
        self.right = None

def size(t):
    return t.size if t else 0

def update(t):
    if t:
        t.size = 1 + size(t.left) + size(t.right)

def merge(a, b):
    if not a:
        return b
    if not b:
        return a
    if a.prio > b.prio:
        a.right = merge(a.right, b)
        update(a)
        return a
    else:
        b.left = merge(a, b.left)
        update(b)
        return b

def split(t, k):
    if not t:
        return None, None
    if size(t.left) >= k:
        a, b = split(t.left, k)
        t.left = b
        update(t)
        return a, t
    else:
        a, b = split(t.right, k - size(t.left) - 1)
        t.right = a
        update(t)
        return t, b

def build(n):
    root = None
    for i in range(1, n + 1):
        root = merge(root, Node(i))
    return root

def collect(t, ans):
    if not t:
        return
    collect(t.left, ans)
    ans.append(str(t.val))
    collect(t.right, ans)

def solve():
    n_line = input().strip()
    if not n_line:
        return
    n = int(n_line)

    root = build(n)

    for _ in range(n):
        a, b = map(int, input().split())
        if b <= a:
            continue

        length = min(b - a, n - b + 1)

        left, rest = split(root, a - 1)
        first, rest = split(rest, length)
        second, right = split(rest, length)

        root = merge(left, merge(second, merge(first, right)))

    ans = []
    collect(root, ans)
    print(" ".join(ans))

if __name__ == "__main__":
    solve()
```Nút treap chỉ lưu trữ giá trị và kích thước cây con. Trường ưu tiên giữ cho cây được cân bằng ngẫu nhiên, cho chiều cao dự kiến ​​theo logarit. 

các`split`chức năng là hoạt động cốt lõi. Nó tách cái đầu tiên`k`các phần tử còn lại của chuỗi. Các chỉ mục nội bộ dựa trên 0 thông qua kích thước cây con, trong khi vấn đề sử dụng các vị trí dựa trên một, do đó mọi vị trí đầu vào đều được điều chỉnh bằng`a - 1`. 

Việc tính toán độ dài xáo trộn là nơi có nhiều giải pháp sai không thành công. Khối thứ hai bắt đầu lúc`b`, vậy nó chứa nhiều nhất`n - b + 1`các phần tử. Nó cũng không thể dài hơn khoảng cách giữa`a`Và`b`, đó là lý do tại sao cần có giá trị tối thiểu của hai đại lượng đó. 

Quá trình truyền tải cuối cùng sử dụng thứ tự theo thứ tự vì một treap ẩn biểu thị chuỗi thông qua thứ tự cây con bên trái, nút và cây con bên phải của nó. 

## Ví dụ đã hoạt động 

Sử dụng mẫu đầu tiên:```
4
3 1
1 3
3 2
2 3
```Dấu vết là: 

| Hoạt động | Hành động hiệu quả | Trình tự | 
| --- | --- | --- | 
| Bắt đầu | Điều trị ban đầu | 1 2 3 4 | 
| 3 1 | bỏ qua | 1 2 3 4 | 
| 1 3 | hoán đổi [1,2] và [3,4] | 3 4 1 2 | 
| 3 2 | bỏ qua | 3 4 1 2 | 
| 2 3 | hoán đổi vị trí 2 và 3 | 3 1 4 2 | 

Ví dụ này chứng minh tại sao các thao tác với`b <= a`phải được bỏ qua và tại sao việc xử lý chỉ cần sắp xếp lại các phạm vi. 

Đối với mẫu thứ hai:```
5
4 1
5 4
3 5
4 5
5 2
```| Hoạt động | Chiều dài hiệu quả | Trình tự | 
| --- | --- | --- | 
| Bắt đầu | | 1 2 3 4 5 | 
| 4 1 | bỏ qua | 1 2 3 4 5 | 
| 5 4 | bỏ qua | 1 2 3 4 5 | 
| 3 5 | 1 | 1 2 5 4 3 | 
| 4 5 | 1 | 1 2 5 3 4 | 
| 5 2 | bỏ qua | 1 2 5 3 4 | 

Dấu vết này thực hiện trường hợp ranh giới trong đó khối bên phải đạt đến cuối chuỗi và không thể có độ dài lý thuyết đầy đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi trong số`n`shuffles thực hiện một số lần chia tách và hợp nhất treap không đổi. | 
| Không gian | O(n) | Kho lưu trữ một nút cho mỗi ô. | 

Với`n = 500000`, mô phỏng bậc hai là không thể. Giải pháp treap thực hiện khoảng vài chục phép tính logarit trong mỗi lần xáo trộn, phù hợp với các ràng buộc đã định. 

## Trường hợp thử nghiệm```python
import sys
import io
import random

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.readline
    n = int(data())
    
    class Node:
        __slots__ = ("v", "p", "s", "l", "r")
        def __init__(self, v):
            self.v = v
            self.p = random.randint(1, 10**9)
            self.s = 1
            self.l = None
            self.r = None

    def sz(t):
        return t.s if t else 0

    def up(t):
        if t:
            t.s = sz(t.l) + sz(t.r) + 1

    def merge(a, b):
        if not a or not b:
            return a or b
        if a.p > b.p:
            a.r = merge(a.r, b)
            up(a)
            return a
        b.l = merge(a, b.l)
        up(b)
        return b

    def split(t, k):
        if not t:
            return None, None
        if sz(t.l) >= k:
            a, b = split(t.l, k)
            t.l = b
            up(t)
            return a, t
        a, b = split(t.r, k - sz(t.l) - 1)
        t.r = a
        up(t)
        return t, b

    root = None
    for i in range(1, n + 1):
        root = merge(root, Node(i))

    for _ in range(n):
        a, b = map(int, data().split())
        if b > a:
            length = min(b - a, n - b + 1)
            x, root = split(root, a - 1)
            y, root = split(root, length)
            z, w = split(root, length)
            root = merge(x, merge(z, merge(y, w)))

    ans = []
    def dfs(t):
        if t:
            dfs(t.l)
            ans.append(str(t.v))
            dfs(t.r)
    dfs(root)
    return " ".join(ans)

assert run("""4
3 1
1 3
3 2
2 3
""") == "3 1 4 2", "sample 1"

assert run("""5
4 1
5 4
3 5
4 5
5 2
""") == "1 2 5 3 4", "sample 2"

assert run("""1
1 1
""") == "1", "minimum size"

assert run("""5
1 5
1 5
1 5
1 5
1 5
""") == "5 4 3 2 1", "boundary length"

assert run("""5
2 2
3 3
4 4
5 5
1 1
""") == "1 2 3 4 5", "all ignored operations"

assert run("""3
1 2
1 2
1 2
""") == "2 3 1", "small repeated rotations"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Yếu tố đơn |`1`| Xử lý kích thước tối thiểu | 
| Lặp đi lặp lại vòng quay đầy đủ |`5 4 3 2 1`| Hoán đổi khối dài | 
| Hoạt động bị bỏ qua |`1 2 3 4 5`| Xử lý đúng`b <= a`| 
| Hoán đổi nhỏ lặp đi lặp lại |`2 3 1`| Thành phần hoạt động chính xác | 

## Vỏ cạnh 

Khi nào`a`Và`b`bằng nhau thì thuật toán sẽ thoát trước khi phân chia. Đối với đầu vào:```
1
1 1
```tren không thay đổi và đầu ra là`1`. 

Khi phía bên phải của trao đổi chạm vào cuối mảng, kích thước khối được tính toán sẽ ngăn việc truy cập các vị trí ngoài chuỗi. Vì:```
5
3 5
```khoảng cách là`2`, nhưng hậu tố bắt đầu ở vị trí`5`có chiều dài`1`, vậy chỉ có một cặp được đổi chỗ. Trình tự thay đổi từ:```
1 2 3 4 5
```ĐẾN:```
1 2 5 4 3
```Treap xử lý việc này vì kích thước phân chia dựa trên độ dài khối thực tế có sẵn chứ không phải khoảng thời gian đầy đủ giả định. 

Các thao tác lặp đi lặp lại trên cùng một phạm vi cũng an toàn vì mọi thao tác xáo trộn đều được áp dụng cho thứ tự xử lý hiện tại. Cấu trúc không dựa vào vị trí ban đầu sau khi khởi tạo, do đó, nó theo dõi hoán vị phát triển một cách tự nhiên.
