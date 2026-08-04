---
title: "CF 102558D - \u041f\u0435\u0440\u0435\u043c\u0435\u0449\u0435\u043d\u0438\u0435 \u0447\u0430\u043d\u043a\u043e\u0432"
description: "Chúng tôi có một mảng gồm n khối. Giá trị tại vị trí i cho biết máy chủ nào hiện đang lưu trữ chunk i. Một truy vấn yêu cầu di chuyển từng đoạn trong khoảng [l, r] từ máy chủ a đến máy chủ b. Việc di chuyển chỉ được phép nếu mọi giá trị trong khoảng đó chính xác là a trước khi thực hiện thao tác."
date: "2026-08-03T19:16:01+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102558
codeforces_index: "D"
codeforces_contest_name: "Contest for Yandex interns 2019"
rating: 0
weight: 102558
solve_time_s: 655
verified: true
draft: false
---

[CF 102558D - \u041f\u0435\u0440\u0435\u043c\u0435\u0449\u0435\u043d\u0438\u0435 \u0447\u0430\u043d\u043a\u043e\u0432](https://codeforces.com/problemset/problem/102558/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 10 phút 55s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một loạt`n`khối. Giá trị tại vị trí`i`cho biết máy chủ nào hiện đang lưu trữ chunk`i`. Một truy vấn yêu cầu di chuyển từng đoạn trong một khoảng thời gian`[l, r]`từ máy chủ`a`đến máy chủ`b`. Việc di chuyển chỉ được phép nếu mọi giá trị trong khoảng đó chính xác`a`trước khi phẫu thuật. Nếu thậm chí một đoạn nằm trên một máy chủ khác thì toàn bộ yêu cầu sẽ bị bỏ qua. Đối với mọi yêu cầu chúng ta phải xuất ra`1`nếu việc di chuyển xảy ra và`0`nếu không thì. 

Đầu vào chứa sự phân công máy chủ ban đầu của tất cả các khối và sau đó là trình tự thời gian của các yêu cầu di chuyển phạm vi. Đầu ra mô tả những yêu cầu nào tồn tại trong quá trình kiểm tra tính nhất quán. 

Các ràng buộc đủ lớn nên việc kiểm tra từng đoạn trong mỗi khoảng thời gian là không thể. Với`n`Và`q`cả hai đều đạt`100000`, một giải pháp thực hiện`O(n)`làm việc cho mọi truy vấn có thể thực hiện`10^10`hoạt động trong trường hợp xấu nhất. Thuật toán phải xử lý từng yêu cầu theo thời gian gần như logarit. Vì thao tác thay đổi toàn bộ khoảng thời gian cùng một lúc, nên chúng ta cần một cấu trúc dữ liệu vừa có thể tóm tắt các khoảng thời gian vừa cập nhật chúng một cách lười biếng. 

Một số trường hợp đặc biệt có thể phá vỡ việc triển khai đơn giản. Một khoảng thời gian duy nhất vẫn phải được kiểm tra và cập nhật chính xác. Ví dụ:```
Input:
1 2 1
1
1 2 1 1
```Đầu ra là:```
1
```Khoảng thời gian chứa một đoạn và máy chủ của nó khớp với máy chủ nguồn được yêu cầu. 

Một lỗi phổ biến khác là chỉ kiểm tra phần tử đầu tiên hoặc phần tử cuối cùng của khoảng. Coi như:```
Input:
3 3 1
1 1 2
1 3 1 1 1
```Đầu ra là:```
0
```Hai phần đầu tiên nằm trên máy chủ`1`, nhưng cái thứ ba ở trên máy chủ`2`. Việc kiểm tra chỉ có ranh giới sẽ chấp nhận việc di chuyển một cách không chính xác. 

Trường hợp cạnh thứ ba xuất hiện sau một số nước đi thành công. Trạng thái hiện tại, không phải trạng thái ban đầu, quyết định liệu truy vấn có thành công hay không. Ví dụ:```
Input:
2 3 2
1 2
1 3 1 1
3 2 1 2
```Đầu ra là:```
1
0
```Sau truy vấn đầu tiên, mảng trở thành`[3, 2]`, vì vậy truy vấn thứ hai yêu cầu di chuyển cả hai khối từ máy chủ`3`thất bại. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là lưu trữ máy chủ hiện tại của từng đoạn trong một mảng. Đối với mỗi truy vấn, hãy quét khoảng thời gian`[l, r]`và xác minh rằng mọi phần tử đều bằng`a`. Nếu kiểm tra thành công, hãy quét lại khoảng thời gian tương tự và thay thế mọi giá trị bằng`b`. 

Cách tiếp cận này đúng vì nó mô phỏng trực tiếp định nghĩa về một nước đi hợp lệ. Tuy nhiên, trong trường hợp xấu nhất một truy vấn có thể bao gồm tất cả`100000`khối, và có thể có`100000`những truy vấn như vậy. Tổng công việc trở nên xung quanh`10^10`các chuyến thăm phần tử, vượt xa những gì có thể. 

Quan sát quan trọng là chúng ta không bao giờ cần giá trị chính xác của mọi phần tử bên trong một phân đoạn trong khi xử lý truy vấn. Chúng tôi chỉ cần biết liệu toàn bộ phân khúc có một giá trị máy chủ hay không. Đồng thời, các truy vấn thành công sẽ thay thế toàn bộ khoảng bằng một giá trị mới. Sự kết hợp này chính xác là điều mà cây phân đoạn lười biếng xử lý tốt. 

Mỗi nút cây phân đoạn lưu trữ giá trị máy chủ nếu toàn bộ khoảng được biểu thị là đồng nhất. Nếu một khoảng chứa nhiều giá trị máy chủ, nút sẽ lưu một điểm đánh dấu đặc biệt có nghĩa là "hỗn hợp". Một truy vấn hỏi cây về trạng thái của`[l, r]`. Nếu giá trị trả về là`a`, việc di chuyển có thể xảy ra và khoảng thời gian được gán cho`b`. Nếu giá trị trả về bị trộn lẫn hoặc số máy chủ khác, yêu cầu sẽ bị từ chối. 

Nhân giống lười biếng là cần thiết vì việc ấn định một khoảng thời gian lớn không cần phải truy cập từng lá. Khi một nút nhận được một nhiệm vụ toàn diện, chúng tôi lưu trữ giá trị mới trong nút đó và trì hoãn việc cập nhật các nút con của nó cho đến khi cần đến chúng sau này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(n) | Quá chậm | 
| Tối ưu | O(q log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng cây phân đoạn lười biếng từ việc gán máy chủ ban đầu. Mỗi nút lưu trữ một số máy chủ nếu toàn bộ khoảng thời gian của nó nằm trên máy chủ đó hoặc`-1`nếu các máy chủ khác xuất hiện bên trong nó. 
2. Đối với mọi yêu cầu di chuyển`(a, b, l, r)`, truy vấn cây phân đoạn để biết trạng thái của khoảng`[l, r]`. Kết quả thể hiện tất cả thông tin cần thiết cho việc kiểm tra tính nhất quán. 
3. Nếu giá trị trả về chính xác`a`, đánh dấu truy vấn là thành công và gán toàn bộ khoảng thời gian`[l, r]`đến máy chủ`b`. 
4. Nếu giá trị trả về là giá trị khác, xuất ra`0`và không sửa đổi cây. 

Bất biến quan trọng là mọi nút cây phân đoạn luôn mô tả chính xác trạng thái hiện tại trong khoảng của nó. Một nút có thể là số máy chủ chính xác vì tất cả các khối bên trong nó đều ở đó hoặc được trộn lẫn vì có ít nhất hai máy chủ khác nhau xuất hiện. Tuyên truyền lười biếng duy trì sự bất biến này bằng cách trì hoãn các cập nhật con trong khi vẫn giữ cho bản tóm tắt gốc chính xác. 

Tại sao nó hoạt động: 

Trước mỗi truy vấn, cây phân đoạn trả về trạng thái thống nhất chính xác của khoảng thời gian được yêu cầu. Nếu trạng thái đó là`a`, mọi đoạn trong khoảng được lưu trữ trên máy chủ nguồn được yêu cầu, do đó việc di chuyển là hợp lệ và thay thế toàn bộ khoảng bằng`b`phù hợp với hoạt động thực tế. Nếu trạng thái được trộn lẫn hoặc máy chủ khác thì ít nhất một đoạn vi phạm yêu cầu, do đó việc từ chối truy vấn là đúng. Vì mọi bản cập nhật được chấp nhận đều thay đổi biểu diễn cây để khớp với trạng thái mảng mới nên tất cả các lần kiểm tra trong tương lai đều sử dụng thông tin chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, q = map(int, input().split())
    arr = list(map(int, input().split()))

    size = 1
    while size < n:
        size *= 2

    tree = [-1] * (2 * size)
    lazy = [-1] * (2 * size)

    for i, x in enumerate(arr):
        tree[size + i] = x

    for i in range(size - 1, 0, -1):
        if tree[2 * i] == tree[2 * i + 1]:
            tree[i] = tree[2 * i]
        else:
            tree[i] = -1

    def apply(node, value):
        tree[node] = value
        lazy[node] = value

    def push(node):
        if lazy[node] != -1:
            apply(node * 2, lazy[node])
            apply(node * 2 + 1, lazy[node])
            lazy[node] = -1

    def query(node, left, right, ql, qr):
        if qr < left or right < ql:
            return -2

        if ql <= left and right <= qr:
            return tree[node]

        push(node)
        mid = (left + right) // 2
        a = query(node * 2, left, mid, ql, qr)
        b = query(node * 2 + 1, mid + 1, right, ql, qr)

        if a == -2:
            return b
        if b == -2:
            return a
        if a == b:
            return a
        return -1

    def update(node, left, right, ql, qr, value):
        if qr < left or right < ql:
            return

        if ql <= left and right <= qr:
            apply(node, value)
            return

        push(node)
        mid = (left + right) // 2
        update(node * 2, left, mid, ql, qr, value)
        update(node * 2 + 1, mid + 1, right, ql, qr, value)

        if tree[node * 2] == tree[node * 2 + 1]:
            tree[node] = tree[node * 2]
        else:
            tree[node] = -1

    ans = []
    for _ in range(q):
        a, b, l, r = map(int, input().split())
        l -= 1
        r -= 1

        if query(1, 0, size - 1, l, r) == a:
            ans.append("1")
            update(1, 0, size - 1, l, r, b)
        else:
            ans.append("0")

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Cây được xây dựng theo lũy thừa hai kích thước sao cho mỗi nút có hai nút con, bao gồm cả các nút chưa sử dụng. Những lá thừa đó được khởi tạo bằng`-1`, nhưng chúng không bao giờ được đưa vào các truy vấn thực tế vì tất cả các yêu cầu đều nằm trong`[0, n-1]`. 

các`tree`mảng lưu trữ bản tóm tắt hiện tại của mọi phân đoạn. các`lazy`mảng lưu trữ các bài tập đang chờ xử lý. Giá trị đang chờ xử lý có nghĩa là toàn bộ phân khúc đã có số máy chủ đó nhưng các phần tử con của nó vẫn có thể chứa thông tin cũ. Trước khi trở thành trẻ em,`push`áp dụng nhiệm vụ bị trì hoãn này. 

Hàm truy vấn trả về`-2`đối với các phân đoạn hoàn toàn không liên quan để có thể dễ dàng hợp nhất các phần chồng chéo một phần. Hai số máy chủ được trả về hợp lệ chỉ được kết hợp thành một giá trị nếu chúng bằng nhau. Ngược lại kết quả sẽ trở thành`-1`, nghĩa là khoảng không đều. 

Hàm cập nhật chỉ định toàn bộ khoảng thời gian trong một thao tác khi nút hiện tại được bao phủ hoàn toàn. Sau đó nó sẽ xây dựng lại tổ tiên sau khi cập nhật đệ quy. Số nguyên Python không bị tràn nên không cần xử lý đặc biệt đối với các giá trị được lưu trữ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
1 2 1
1
1 2 1 1
```| Truy vấn | Khoảng thời gian yêu cầu | Kết quả cây | Hành động | Trạng thái mảng | 
| --- | --- | --- | --- | --- | 
| 1 |`[1,1]`, nguồn`1`|`1`| Gán cho máy chủ`2`|`[2]`| 

Đoạn duy nhất đã có trên máy chủ nguồn được yêu cầu nên thao tác được chấp nhận. Điều này kiểm tra trường hợp khoảng phần tử đơn. 

### Mẫu 2 

đầu vào:```
1 2 1
1
2 1 1 1
```| Truy vấn | Khoảng thời gian yêu cầu | Kết quả cây | Hành động | Trạng thái mảng | 
| --- | --- | --- | --- | --- | 
| 1 |`[1,1]`, nguồn`2`|`1`| Từ chối |`[1]`| 

Phân đoạn này đồng nhất nhưng giá trị của nó không phải là máy chủ nguồn được yêu cầu. Việc di chuyển phải thất bại vì việc kiểm tra tính nhất quán được so sánh với`a`, không chỉ liệu khoảng có một giá trị hay không. 

### Mẫu 3 

đầu vào:```
5 5 6
1 2 3 4 5
1 2 1 1
2 3 1 3
4 2 4 4
2 5 1 4
3 2 2 3
3 2 3 3
```| Truy vấn | Kết quả khoảng thời gian trước khi hoạt động | Quyết định | Mảng sau thao tác | 
| --- | --- | --- | --- | 
| 1 |`[1]`có máy chủ`1`| Chấp nhận |`[2,2,3,4,5]`| 
| 2 |`[1,3]`được trộn lẫn | Từ chối |`[2,2,3,4,5]`| 
| 3 |`[4]`có máy chủ`4`| Chấp nhận |`[2,2,3,2,5]`| 
| 4 |`[1,4]`được trộn lẫn | Từ chối |`[2,2,3,2,5]`| 
| 5 |`[2,3]`được trộn lẫn | Từ chối |`[2,2,3,2,5]`| 
| 6 |`[3]`có máy chủ`3`| Chấp nhận |`[2,2,2,2,5]`| 

Dấu vết cho thấy lý do tại sao cấu trúc phải được cập nhật sau mỗi yêu cầu thành công. Các yêu cầu không thành công sẽ không thay đổi trạng thái nên các truy vấn sau này vẫn thấy cấu hình trước đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(q log n) | Mỗi truy vấn thực hiện một truy vấn cây phân đoạn và chỉ khi các yêu cầu thành công mới thực hiện một phép gán phạm vi. | 
| Không gian | O(n) | Cây phân đoạn và mảng lười chứa số lượng mục không đổi trên mỗi nút cây. | 

Với`100000`khối và`100000`yêu cầu, xử lý logarit giữ tổng số nút được truy cập vào khoảng vài triệu, phù hợp với giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    data = inp.strip().split()
    it = iter(data)

    n = int(next(it))
    m = int(next(it))
    q = int(next(it))
    arr = [int(next(it)) for _ in range(n)]

    size = 1
    while size < n:
        size *= 2

    tree = [-1] * (2 * size)
    lazy = [-1] * (2 * size)

    for i, x in enumerate(arr):
        tree[size + i] = x

    for i in range(size - 1, 0, -1):
        tree[i] = tree[2 * i] if tree[2 * i] == tree[2 * i + 1] else -1

    def apply(node, value):
        tree[node] = value
        lazy[node] = value

    def push(node):
        if lazy[node] != -1:
            apply(node * 2, lazy[node])
            apply(node * 2 + 1, lazy[node])
            lazy[node] = -1

    def query(node, l, r, ql, qr):
        if qr < l or r < ql:
            return -2
        if ql <= l and r <= qr:
            return tree[node]
        push(node)
        mid = (l + r) // 2
        x = query(node * 2, l, mid, ql, qr)
        y = query(node * 2 + 1, mid + 1, r, ql, qr)
        if x == -2:
            return y
        if y == -2:
            return x
        return x if x == y else -1

    def update(node, l, r, ql, qr, value):
        if qr < l or r < ql:
            return
        if ql <= l and r <= qr:
            apply(node, value)
            return
        push(node)
        mid = (l + r) // 2
        update(node * 2, l, mid, ql, qr, value)
        update(node * 2 + 1, mid + 1, r, ql, qr, value)
        tree[node] = tree[node * 2] if tree[node * 2] == tree[node * 2 + 1] else -1

    out = []
    for _ in range(q):
        a = int(next(it))
        b = int(next(it))
        l = int(next(it)) - 1
        r = int(next(it)) - 1
        if query(1, 0, size - 1, l, r) == a:
            out.append("1")
            update(1, 0, size - 1, l, r, b)
        else:
            out.append("0")
    return "\n".join(out)

assert run("""1 2 1
1
1 2 1 1
""") == "1"

assert run("""1 2 1
1
2 1 1 1
""") == "0"

assert run("""5 5 6
1 2 3 4 5
1 2 1 1
2 3 1 3
4 2 4 4
2 5 1 4
3 2 2 3
3 2 3 3
""") == "1\n0\n1\n0\n0\n1"

assert run("""3 5 3
2 2 2
2 3 1 3
3 4 1 2
3 5 2 3
""") == "1\n1\n0"

assert run("""4 4 2
1 1 1 1
1 2 2 3
2 3 1 4
""") == "1\n1"

assert run("""2 3 3
1 2
1 3 1 1
3 2 1 1
2 1 2 2
""") == "1\n1\n1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Di chuyển một đoạn |`1`| Kích thước tối thiểu và cập nhật lá | 
| Mẫu 2 |`0`| Khoảng thời gian thống nhất với máy chủ nguồn sai | 
| Tất cả các khối ban đầu bằng nhau |`1,1`| Nhiệm vụ phạm vi thành công lớn | 
| Khoảng thời gian hỗn hợp sau khi cập nhật |`1,1,1`| Thay đổi trạng thái giữa các truy vấn | 

## Vỏ cạnh 

Khoảng một phần tử được xử lý bằng logic cây phân đoạn giống như các phạm vi lớn hơn. Trong đầu vào:```
1 2 1
1
1 2 1 1
```truy vấn đến một lá, nhận giá trị`1`và cập nhật lá đó thành`2`. Kết quả là`1`. 

Một khoảng có vẻ đúng ở ranh giới của nó nhưng bị trộn lẫn bên trong sẽ được xử lý vì các nút bên trong duy trì trạng thái hỗn hợp. Vì:```
3 3 1
1 1 2
1 3 1 1
```phân đoạn gốc bao gồm phạm vi truy vấn kết hợp các phần tử con với các giá trị khác nhau và trả về`-1`. Từ`-1`không phải là máy chủ nguồn được yêu cầu, câu trả lời là`0`. 

Các thao tác sau lần di chuyển trước sử dụng trạng thái cây được cập nhật. Vì:```
2 3 2
1 2
1 3 1 1
3 2 1 2
```truy vấn đầu tiên thay đổi đoạn đầu tiên thành máy chủ`3`. Truy vấn thứ hai nhìn thấy khoảng thời gian`[3,2]`, được trộn lẫn và từ chối yêu cầu. Đầu ra là:```
1
0
```Cơ chế gán lười biếng xử lý các khoảng thời gian lớn mà không cần mở rộng chúng ngay lập tức. Nếu một truy vấn thay đổi tất cả các phần trong một phân đoạn lớn, nút sẽ lưu trữ trực tiếp giá trị máy chủ mới và các hoạt động trong tương lai chỉ đẩy thông tin đó khi chúng cần kiểm tra các phần nhỏ hơn.
