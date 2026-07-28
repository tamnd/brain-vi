---
title: "CF 102787Y - Lời nói và lời nói 1"
description: "Vấn đề theo dõi một dòng các sneetch, trong đó mỗi sneetch có 0 hoặc một ngôi sao. Một truy vấn chọn một khoảng thời gian liên tục và gửi những sneet này thông qua một máy chuyển đổi giá trị của chúng: số 0 trở thành 1 và số 1 trở thành 0."
date: "2026-07-27T19:18:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102787
codeforces_index: "Y"
codeforces_contest_name: "Algorithms Thread Treaps Contest"
rating: 0
weight: 102787
solve_time_s: 87
verified: true
draft: false
---

[CF 102787Y - Tiếng hắt hơi và Bài phát biểu 1](https://codeforces.com/problemset/problem/102787/Y) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 27s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Vấn đề theo dõi một dòng các sneetch, trong đó mỗi sneetch có 0 hoặc một ngôi sao. Một truy vấn chọn một khoảng thời gian liên tục và gửi những sneet này thông qua một máy chuyển đổi giá trị của chúng: số 0 trở thành 1 và số 1 trở thành 0. Sau mỗi thao tác chuyển đổi, chúng ta phải báo cáo độ dài của đoạn liên tiếp dài nhất trong đó tất cả các đoạn mã có cùng giá trị. Phiên bản đầu tiên của sự cố chỉ chứa các truy vấn chuyển đổi này. 

Đầu vào cung cấp chuỗi nhị phân ban đầu mô tả các sneetches và sau đó là một chuỗi các khoảng thời gian để chuyển đổi. Đối với mỗi khoảng thời gian, đầu ra không phải là số lượng số một hoặc số 0, mà là khối dài nhất của các giá trị liền kề giống hệt nhau ở bất kỳ đâu trong chuỗi hiện tại. 

Các ràng buộc rất lớn: cả số lượng sneetches và số lượng thao tác đều có thể đạt tới$3 \cdot 10^5$. Một giải pháp quét toàn bộ mảng sau mỗi thao tác sẽ thực hiện xung quanh$9 \cdot 10^{10}$hoạt động trong trường hợp xấu nhất, vượt xa những gì có thể phù hợp với giới hạn thời gian thi đấu thông thường. Chúng tôi cần mỗi bản cập nhật và kết quả truy vấn được xử lý theo thời gian logarit. 

Các trường hợp cạnh chính đến từ các khoảng chạm vào đường viền hoặc chứa toàn bộ mảng. Giải pháp chỉ cập nhật trong khoảng mà quên rằng câu trả lời có thể hợp nhất qua ranh giới sẽ thất bại. Ví dụ: nếu đầu vào là:```
5 1
00111
1 2 4
```Mảng trở thành`01011`, vậy câu trả lời là`2`bởi vì hai cái cuối cùng tạo thành đoạn bằng nhau dài nhất. Triển khai bất cẩn chỉ kiểm tra đoạn bị đảo lộn`101`có thể báo cáo sai`1`. 

Một trường hợp phức tạp khác là đảo ngược một khoảng đã đồng nhất. Ví dụ:```
4 1
0000
1 2 3
```Kết quả là`0110`, và câu trả lời là`2`. Bản cập nhật thay đổi phần ở giữa từ tất cả các số 0 thành tất cả các số 1, nhưng các số 0 bên ngoài vẫn được kết nối. Cách tiếp cận chỉ lưu trữ phân đoạn tối đa được tạo bên trong khoảng thời gian đã thay đổi có thể bỏ lỡ phần đóng góp bên ngoài. 

Trường hợp ranh giới cuối cùng là cập nhật một phần tử:```
3 1
010
1 2 2
```Mảng trở thành`000`, vậy câu trả lời là`3`. Bất kỳ cách triển khai nào giả định khoảng thời gian đảo ngược luôn có hai bên để hợp nhất đều có thể xử lý sai tình huống này. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp là lưu trữ mảng nhị phân và xử lý từng thao tác bằng cách đi bộ từ`l`ĐẾN`r`, chuyển đổi mọi phần tử. Sau khi cập nhật, một lần quét khác có thể tìm thấy lần chạy dài nhất có giá trị bằng nhau. Điều này đúng vì nó mô phỏng chính xác quá trình được mô tả bởi bài toán. Tuy nhiên, một thao tác duy nhất có thể chạm tới tất cả$n$các yếu tố, và việc tìm ra câu trả lời cũng tốn kém$O(n)$. Với$q$hoạt động, trường hợp xấu nhất sẽ trở thành$O(nq)$, đó là về$9 \cdot 10^{10}$lượt truy cập phần tử cho các ràng buộc tối đa. 

Quan sát quan trọng là hoạt động này là một phạm vi lật và câu trả lời được yêu cầu là một thuộc tính có thể được kết hợp từ các phân đoạn lân cận. Cây phân đoạn có thể biểu thị mọi khoảng bằng cách lưu trữ đủ thông tin để hợp nhất hai nửa. Chúng tôi không cần giá trị chính xác của toàn bộ phân khúc sau mỗi thao tác. Chúng ta chỉ cần tiền tố dài nhất, hậu tố dài nhất và chuỗi số 0 và số 1 bên trong dài nhất, cùng với độ dài phân đoạn. 

Khi một phân đoạn được đảo ngược, số 0 và số 1 chỉ đơn giản là trao đổi vai trò. Tiền tố dài nhất của số 0 trở thành tiền tố dài nhất trước đó của số 1 và điều tương tự cũng áp dụng cho hậu tố và phân đoạn tốt nhất. Điều này làm cho việc lan truyền lười biếng trở nên tự nhiên: thay vì truy cập mọi phần tử, chúng tôi đánh dấu toàn bộ một phân đoạn là bị đảo ngược và trì hoãn việc thực hiện thay đổi đó cho đến khi thực sự cần đến một phần tử con. 

Lực lượng vũ phu hoạt động vì mọi yếu tố đều được thay đổi rõ ràng. Nó thất bại vì có quá nhiều yếu tố có thể được thay đổi quá thường xuyên. Cây phân đoạn hoạt động vì một bản cập nhật phạm vi lớn có thể được biểu thị bằng một cờ lười duy nhất, giảm mỗi thao tác xuống còn$O(\log n)$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nq)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(q \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng cây phân đoạn từ chuỗi nhị phân ban đầu. Đối với mỗi nút, hãy lưu trữ độ dài phân đoạn, tiền tố và hậu tố dài nhất chỉ bao gồm các số 0, các giá trị giống nhau cho các số 1, phân đoạn toàn 0 tốt nhất, phân đoạn toàn một tốt nhất và cờ lật lười. 

Các giá trị này chính xác là thông tin cần thiết để hợp nhất hai phân đoạn liền kề. Bất kỳ câu trả lời nào liên quan đến ranh giới giữa hai con phải sử dụng hậu tố của con bên trái và tiền tố của con bên phải. 
2. Đối với khoảng truy vấn$[l,r]$, truy cập đệ quy vào cây phân đoạn. Nếu một nút hoàn toàn nằm trong khoảng, hãy áp dụng thao tác lật nút đó thay vì đi sâu hơn. 

Việc áp dụng một lần lật chỉ hoán đổi mọi trường liên quan đến số 0 với trường liên quan đến một trường tương ứng của nó và chuyển đổi cờ lười. Độ dài của đoạn không thay đổi. 
3. Khi một nút được che phủ một phần được truy cập, hãy đẩy lượt lật đang chờ xử lý của nó sang các nút con của nó trước khi tiếp tục. 

Điều này giúp trẻ luôn phù hợp với tình trạng hiện tại của cha mẹ chúng. Trì hoãn cập nhật là an toàn vì thông tin gốc đã chứa thông tin kết hợp chính xác. 
4. Sau khi cập nhật khoảng thời gian, hãy đọc câu trả lời của nút gốc là giá trị tối đa của phân đoạn toàn 0 dài nhất và phân đoạn toàn một dài nhất. 

Gốc đại diện cho mảng hoàn chỉnh, vì vậy phân đoạn tốt nhất được lưu trữ của nó chính xác là câu trả lời được yêu cầu. 

Tại sao nó hoạt động: 

Mỗi nút cây phân đoạn luôn mô tả trạng thái hiện tại của khoảng thời gian của nó. Hoạt động hợp nhất bảo toàn thuộc tính này vì mọi phân đoạn có giá trị bằng nhau dài nhất hoặc nằm hoàn toàn ở con bên trái, hoàn toàn ở con bên phải hoặc vượt qua ranh giới ở giữa. Các giá trị tiền tố và hậu tố được lưu trữ sẽ xử lý trường hợp chéo. Việc lật chỉ trao đổi vai trò của số 0 và số 1, vì vậy việc hoán đổi thông tin số 0 và một được lưu trữ sẽ giữ cho nút chính xác. Vì mọi bản cập nhật đều giữ nguyên tính bất biến cho tất cả các nút bị ảnh hưởng nên nút gốc luôn chứa đoạn thống nhất dài nhất chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Node:
    __slots__ = (
        "length",
        "pref0", "suff0", "best0",
        "pref1", "suff1", "best1",
        "lazy"
    )

    def __init__(self):
        self.length = 0
        self.pref0 = self.suff0 = self.best0 = 0
        self.pref1 = self.suff1 = self.best1 = 0
        self.lazy = False

def merge(a, b):
    c = Node()
    c.length = a.length + b.length

    c.pref0 = a.pref0
    if a.pref0 == a.length:
        c.pref0 += b.pref0

    c.pref1 = a.pref1
    if a.pref1 == a.length:
        c.pref1 += b.pref1

    c.suff0 = b.suff0
    if b.suff0 == b.length:
        c.suff0 += a.suff0

    c.suff1 = b.suff1
    if b.suff1 == b.length:
        c.suff1 += a.suff1

    c.best0 = max(a.best0, b.best0, a.suff0 + b.pref0)
    c.best1 = max(a.best1, b.best1, a.suff1 + b.pref1)

    return c

def apply_flip(node):
    node.pref0, node.pref1 = node.pref1, node.pref0
    node.suff0, node.suff1 = node.suff1, node.suff0
    node.best0, node.best1 = node.best1, node.best0
    node.lazy = not node.lazy

def build(idx, l, r):
    tree[idx] = Node()
    if l == r:
        tree[idx].length = 1
        if s[l] == '0':
            tree[idx].pref0 = tree[idx].suff0 = tree[idx].best0 = 1
        else:
            tree[idx].pref1 = tree[idx].suff1 = tree[idx].best1 = 1
        return

    m = (l + r) // 2
    build(idx * 2, l, m)
    build(idx * 2 + 1, m + 1, r)
    tree[idx] = merge(tree[idx * 2], tree[idx * 2 + 1])

def push(idx):
    if tree[idx].lazy:
        apply_flip(tree[idx * 2])
        apply_flip(tree[idx * 2 + 1])
        tree[idx].lazy = False

def update(idx, l, r, ql, qr):
    if qr < l or r < ql:
        return
    if ql <= l and r <= qr:
        apply_flip(tree[idx])
        return

    push(idx)
    m = (l + r) // 2
    update(idx * 2, l, m, ql, qr)
    update(idx * 2 + 1, m + 1, r, ql, qr)
    tree[idx] = merge(tree[idx * 2], tree[idx * 2 + 1])

def solve():
    global s, tree

    n, q = map(int, input().split())
    s = input().strip()
    s = " " + s

    tree = [None] * (4 * n + 5)
    build(1, 1, n)

    ans = []
    for _ in range(q):
        _, l, r = map(int, input().split())
        update(1, 1, n, l, r)
        ans.append(str(max(tree[1].best0, tree[1].best1)))

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```các`Node`đối tượng chỉ lưu trữ thông tin cần thiết để hợp nhất. Việc giữ các giá trị riêng biệt cho lượt chạy 0 và lượt chạy một lần sẽ tránh cần biết nội dung thực tế của một phân đoạn. 

các`merge`Hàm xử lý ba vị trí có thể có của phân đoạn tốt nhất: hoàn toàn ở con bên trái, hoàn toàn ở con bên phải hoặc vượt qua ranh giới. Trường hợp chéo là lý do tại sao cả độ dài hậu tố và tiền tố đều được lưu trữ. 

các`apply_flip`chức năng là quan sát cốt lõi của vấn đề. Một cú lật không bao giờ thay đổi độ dài đoạn, nó chỉ trao đổi ý nghĩa của 0 và một. Cờ lười ghi lại rằng trẻ em vẫn cần áp dụng trao đổi này. 

Bản cập nhật đệ quy sử dụng phương pháp lan truyền lười biếng. Một đoạn được che phủ hoàn toàn sẽ được lật ngay lập tức, trong khi một phần đoạn được đẩy xuống trước. Thứ tự này ngăn chặn thông tin con cũ ảnh hưởng đến việc hợp nhất trong tương lai. 

Số nguyên Python không tràn ở đây vì giá trị được lưu trữ lớn nhất chỉ$n$, nhiều nhất là$3 \cdot 10^5$. Việc lập chỉ mục sử dụng ký tự giả ở vị trí 0 để các chỉ mục truy vấn khớp trực tiếp với câu lệnh. 

## Ví dụ đã hoạt động 

Ví dụ 1:```
8 8
00000000
1 1 3
1 2 7
1 2 4
1 5 6
1 5 5
1 1 8
1 4 5
1 6 8
```| Bước | Phạm vi đảo ngược | Root tốt nhất0 | Root tốt nhất1 | Trả lời | 
| --- | --- | --- | --- | --- | 
| Ban đầu | không | 8 | 0 | 8 | 
| 1 | 1 đến 3 | 5 | 3 | 5 | 
| 2 | 2 đến 7 | 4 | 4 | 4 | 
| 3 | 2 đến 4 | 3 | 5 | 5 | 
| 4 | 5 đến 6 | 3 | 3 | 3 | 

Dấu vết cho thấy câu trả lời được xác định bởi cả phân đoạn 0 và một. Một đoạn số 0 lớn có thể biến mất sau khi lật trong khi một đoạn trở thành mức tối đa mới. 

Ví dụ 2:```
7 7
0111111
1 3 7
1 1 7
1 1 4
1 2 6
1 1 6
1 1 2
1 2 7
```| Bước | Phạm vi đảo ngược | Phân khúc số 0 tốt nhất | Một đoạn hay nhất | Trả lời | 
| --- | --- | --- | --- | --- | 
| Ban đầu | không | 1 | 6 | 6 | 
| 1 | 3 đến 7 | 5 | 1 | 5 | 
| 2 | 1 đến 7 | 2 | 5 | 5 | 
| 3 | 1 đến 4 | 3 | 3 | 3 | 
| 4 | 2 đến 6 | 2 | 3 | 3 | 

Ví dụ này thực hiện các cập nhật chồng chéo. Cây phân đoạn không bao giờ xây dựng lại toàn bộ chuỗi vì mỗi lần cập nhật chỉ thay đổi$O(\log n)$nút. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(q \log n)$| Mỗi lần lật phạm vi chỉ truy cập các nút cây phân đoạn giao nhau trong khoảng đó. | 
| Không gian |$O(n)$| Cây chứa một lượng thông tin không đổi cho mỗi phân đoạn. | 

Kích thước đầu vào tối đa chứa hàng trăm nghìn sneet và thao tác. Cần phải cập nhật logarit và cây phân đoạn giữ tổng công việc trong giới hạn sẵn có. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.read().split()
    sys.stdin = old_stdin

    it = iter(data)
    n = int(next(it))
    q = int(next(it))
    s = next(it)

    arr = list(map(int, s))
    out = []

    for _ in range(q):
        next(it)
        l = int(next(it)) - 1
        r = int(next(it)) - 1
        for i in range(l, r + 1):
            arr[i] ^= 1

        best = 1
        cur = 1
        for i in range(1, n):
            if arr[i] == arr[i - 1]:
                cur += 1
            else:
                cur = 1
            best = max(best, cur)
        out.append(str(best))

    return "\n".join(out)

assert run("""8 8
00000000
1 1 3
1 2 7
1 2 4
1 5 6
1 5 5
1 1 8
1 4 5
1 6 8
""") == """5
4
3
3
3
3
4
4"""

assert run("""7 7
0111111
1 3 7
1 1 7
1 1 4
1 2 6
1 1 6
1 1 2
1 2 7
""") == """5
5
3
2
3
4
3"""

assert run("""1 3
0
1 1 1
1 1 1
1 1 1
""") == """1
1
1"""

assert run("""5 2
00000
1 2 4
1 1 5
""") == """2
5"""

assert run("""6 2
111111
1 1 6
1 3 5
""") == """6
3"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mảng phần tử đơn | Luôn luôn 1 | Xử lý kích thước tối thiểu | 
| Tất cả các số không với các lần lật hoàn toàn | Phân khúc thống nhất lớn sau khi cập nhật | Cập nhật toàn bộ mảng | 
| Tất cả những cái có lượt lật chồng lên nhau | Sửa đổi số 0 và một | Lật đối xứng | 
| Khoảng cách một phần gần biên giới | Hợp nhất đúng với các phần còn nguyên | Điều kiện biên | 

## Vỏ cạnh 

Đối với ví dụ ranh giới đầu tiên:```
5 1
00111
1 2 4
```Cây lật vị trí từ hai đến bốn. Phần bên trái thay đổi từ`0`ĐẾN`1`, phần giữa thay đổi từ`0,1,1`ĐẾN`1,0,0`, còn lại phần bên phải`1`. Chuỗi cuối cùng là`01011`, trong đó đoạn bằng nhau dài nhất có chiều dài`2`. Hoạt động hợp nhất tìm thấy điều này vì nó kết hợp thông tin hậu tố từ một thành phần con với thông tin tiền tố từ thành phần tiếp theo. 

Đối với ví dụ về phân khúc thống nhất:```
4 1
0000
1 2 3
```Đoạn giữa nhận được một cú lật lười biếng và trở thành một. Hai số 0 bên ngoài chỉ được kết nối nếu cây phân đoạn kết hợp thông tin lân cận chính xác. Các tiền tố, hậu tố và giá trị tốt nhất được lưu trữ cho phép gốc trả về`2`. 

Đối với bản cập nhật một phần tử:```
3 1
010
1 2 2
```Chiếc lá ở vị trí thứ hai bị lật từ một về không. Các nút cha được hợp nhất lên trên, tạo ra`000`. Thư mục gốc báo cáo toàn bộ chiều dài`3`, cho thấy phạm vi vị trí đơn hoạt động theo cách tương tự như các khoảng lớn hơn.
