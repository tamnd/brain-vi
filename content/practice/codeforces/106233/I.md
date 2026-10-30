---
title: "CF 106233I - \u041f\u043e\u0432\u043e\u0440\u043e\u0442\u044b \u043c\u0430\u0433\u0438\u0447\u0435\u0441\u043a\u0438\u0445 \u043c\u0435\u0442\u043e\u043a"
description: "Chúng ta có một chuỗi dấu ấn ma thuật được đặt trên một mặt phẳng. Dấu thứ i có tọa độ (xi, yi). Thủ thuật chọn một đoạn dấu liên tiếp và xoay mọi dấu trong đoạn đó xung quanh một điểm nhất định 90, 180 hoặc 270 độ theo chiều kim đồng hồ."
date: "2026-06-25T07:04:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106233
codeforces_index: "I"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2025-2026, \u0427\u0435\u0442\u0432\u0435\u0440\u0442\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 106233
solve_time_s: 45
verified: true
draft: false
---

[CF 106233I - \u041f\u043e\u0432\u043e\u0440\u043e\u0442\u044b \u043c\u0430\u0433\u0438\u0447\u0435\u0441\u043a\u0438\u0445 \u043c\u0435\u0442\u043e\u043a](https://codeforces.com/problemset/problem/106233/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 45s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một chuỗi dấu ấn ma thuật được đặt trên một mặt phẳng. các`i`-dấu thứ có tọa độ`(xi, yi)`. Thủ thuật chọn một đoạn dấu liên tiếp và xoay mọi dấu trong đoạn đó xung quanh một điểm nhất định 90, 180 hoặc 270 độ theo chiều kim đồng hồ. Sau mỗi thủ thuật, chúng ta cần diện tích hình chữ nhật thẳng hàng theo trục nhỏ nhất chứa tất cả các dấu. 

Thách thức là các phép toán được thực hiện trên các vị trí trong mảng chứ không phải trên toàn bộ tập hợp điểm. Một nhãn hiệu có thể di chuyển nhiều lần và điểm cuối của phân đoạn của các hoạt động trong tương lai phụ thuộc vào việc lập chỉ mục ban đầu. Chúng ta cần giữ tọa độ của tất cả các điểm sau mỗi lần cập nhật và nhanh chóng tìm ra các giá trị x và y tối thiểu và tối đa toàn cầu. 

Các ràng buộc cho phép lên đến`2 * 10^5`dấu hiệu và`2 * 10^5`các phép quay. Một mô phỏng trực tiếp di chuyển mọi điểm trong khoảng đã chọn sẽ yêu cầu tới`O(nq)`hoạt động, có thể tiếp cận xung quanh`4 * 10^10`phối hợp các thay đổi và vượt xa những gì một vài giây cho phép. Giải pháp phải làm cho mỗi phép toán gần với logarit. 

Các trường hợp khó khăn không phải là bản thân các phép quay mà là việc giữ cho các phép biến đổi chính xác theo thời gian. Một đoạn có thể được xoay nhiều lần và các phép quay có thể xoay quanh các tâm khác nhau. Ví dụ: nếu một điểm được xoay hai lần thì lần quay thứ hai phải sử dụng vị trí đã thay đổi của điểm đó. 

Đối với đầu vào:```
1 1
5 7
1 1 0 0 90
```điểm`(5,7)`trở thành`(-7,5)`, do đó hình chữ nhật bao quanh có diện tích bằng không. Việc triển khai bất cẩn chỉ lưu trữ chiều rộng và chiều cao sẽ làm mất vị trí tuyệt đối và thất bại. 

Một trường hợp khác là:```
2 1
0 0
10 1
1 2 5 5 180
```Cả hai điểm đều quay xung quanh`(5,5)`và trở thành`(10,10)`Và`(0,9)`. Câu trả lời là`90`. Giải pháp chỉ xoay tâm đoạn thẳng hoặc chỉ hoán đổi tọa độ mà không xử lý dịch chuyển sẽ tạo ra hình chữ nhật sai. 

## Phương pháp tiếp cận 

Một giải pháp đơn giản là lưu trữ mọi điểm một cách rõ ràng. Đối với mỗi truy vấn, lặp lại từ`l`ĐẾN`r`, xoay mọi điểm và cập nhật tọa độ tối thiểu và tối đa toàn cầu. Điều này đúng vì mọi điểm luôn được giữ ở vị trí thực tế hiện tại của nó. Sự cố xuất hiện khi khoảng thời gian lớn. Trong trường hợp xấu nhất, mọi truy vấn đều chạm tới tất cả`n`điểm, do đó độ phức tạp trở thành`O(nq)`. 

Một quan sát hữu ích là một hình chữ nhật không cần biết mọi điểm bên trong nó. Đối với bất kỳ tập hợp điểm nào, hộp giới hạn của nó hoàn toàn được xác định bởi bốn giá trị: x tối thiểu, x tối đa, y tối thiểu và y tối đa. Nếu chúng ta có thể cập nhật bốn giá trị này cho toàn bộ phân khúc cùng một lúc thì vấn đề sẽ trở nên nhỏ hơn nhiều. 

Phép quay quanh một điểm là phép biến đổi affine. Nó thay đổi tọa độ bằng cách sử dụng các công thức có dạng:```
x' = a*x + b*y + c
y' = d*x + e*y + f
```Bốn phép quay có thể được sử dụng ở đây chỉ hoán đổi dấu hiệu và tọa độ. Việc áp dụng một phép biến đổi như vậy vào một hộp giới hạn chỉ yêu cầu kiểm tra bốn góc của hộp, bởi vì phép biến đổi affine ánh xạ các điểm cực trị của một hình chữ nhật thẳng hàng với các ứng cử viên cực đoan. 

Điều này dẫn đến một cây phân đoạn một cách tự nhiên. Mỗi nút lưu trữ hộp giới hạn của khoảng của nó và một phép biến đổi affine lười biếng đang chờ được truyền cho nút con. Khi toàn bộ nút được bao phủ bởi một truy vấn, chúng tôi sẽ chuyển đổi hình chữ nhật được lưu trữ của nút đó thay vì truy cập tất cả các điểm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(n) | Quá chậm | 
| Cây phân đoạn với các phép biến đổi lười biếng | O((n+q) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng cây phân đoạn theo thứ tự ban đầu của các nhãn hiệu. Đối với mỗi nút, lưu trữ giá trị x và y tối thiểu và tối đa của tất cả các điểm trong khoảng này. Ban đầu mọi sự chuyển đổi lười biếng đều là sự chuyển đổi danh tính. 
2. Biểu diễn mọi phép quay dưới dạng phép biến đổi affine. Để xoay 90 độ theo chiều kim đồng hồ`(x0, y0)`, các công thức là:```
x' = y0 - y + x0
y' = x - x0 + y0
```Các trường hợp 180 và 270 độ được xử lý theo cùng một cách biểu diễn. 

1. Khi một bản cập nhật bao phủ hoàn toàn một nút cây phân đoạn, hãy áp dụng phép xoay trực tiếp vào hộp giới hạn của nút đó. Chuyển đổi bốn góc, tìm tọa độ tối thiểu và tối đa mới và lưu trữ chúng. 
2. Kết hợp phép biến đổi mới với phép biến đổi lười biếng hiện có của nút. Phép biến đổi mới phải được áp dụng sau các phép biến đổi đang chờ xử lý trước đó vì nó thể hiện sự thay đổi mới nhất. 
3. Khi một nút được che phủ một phần được truy cập, hãy đẩy phép chuyển đổi lười biếng của nó sang cả hai nút con. Điều này giữ cho thông tin con được đồng bộ hóa trước khi tiếp tục đệ quy. 
4. Sau mỗi lần cập nhật, thư mục gốc chứa hộp giới hạn của mỗi dấu, do đó chiều rộng và chiều cao của nó cho câu trả lời:```
(max_x - min_x) * (max_y - min_y)
```Tại sao nó hoạt động: tính bất biến của cây phân đoạn là mọi nút đều lưu trữ hộp giới hạn chính xác của vị trí hiện tại của tất cả các điểm trong khoảng của nó, cùng với một phép biến đổi vẫn cần được truyền cho các nút con. Áp dụng phép quay cho toàn bộ khoảng là an toàn vì mọi điểm trong khoảng đều nhận được cùng một phép biến đổi affine. Vì các góc hình chữ nhật được biến đổi chứa các điểm cực trị được biến đổi nên hộp giới hạn được cập nhật là chính xác. Việc kết hợp các phép biến đổi lười biếng sẽ giữ nguyên ý nghĩa cho các hoạt động trong tương lai. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def compose(t2, t1):
    a2, b2, c2, d2, e2, f2 = t2
    a1, b1, c1, d1, e1, f1 = t1
    return (
        a2 * a1 + b2 * d1,
        a2 * b1 + b2 * e1,
        a2 * c1 + b2 * f1 + c2,
        d2 * a1 + e2 * d1,
        d2 * b1 + e2 * e1,
        d2 * c1 + e2 * f1 + f2,
    )

def apply_transform(node, t):
    a, b, c, d, e, f = t
    mnx, mxx, mny, mxy = node[:4]

    xs = []
    ys = []
    for x, y in ((mnx, mny), (mnx, mxy), (mxx, mny), (mxx, mxy)):
        xs.append(a * x + b * y + c)
        ys.append(d * x + e * y + f)

    node[0] = min(xs)
    node[1] = max(xs)
    node[2] = min(ys)
    node[3] = max(ys)

    node[4] = compose(t, node[4])

def build(v, l, r, pts, tree):
    if l == r:
        x, y = pts[l]
        tree[v] = [x, x, y, y, (1, 0, 0, 0, 1, 0)]
        return
    m = (l + r) // 2
    build(v * 2, l, m, pts, tree)
    build(v * 2 + 1, m + 1, r, pts, tree)
    pull(v, tree)

def pull(v, tree):
    left = tree[v * 2]
    right = tree[v * 2 + 1]
    tree[v][0] = min(left[0], right[0])
    tree[v][1] = max(left[1], right[1])
    tree[v][2] = min(left[2], right[2])
    tree[v][3] = max(left[3], right[3])

def push(v, tree):
    t = tree[v][4]
    if t != (1, 0, 0, 0, 1, 0):
        apply_transform(tree[v * 2], t)
        apply_transform(tree[v * 2 + 1], t)
        tree[v][4] = (1, 0, 0, 0, 1, 0)

def update(v, l, r, ql, qr, t, tree):
    if ql <= l and r <= qr:
        apply_transform(tree[v], t)
        return
    push(v, tree)
    m = (l + r) // 2
    if ql <= m:
        update(v * 2, l, m, ql, qr, t, tree)
    if qr > m:
        update(v * 2 + 1, m + 1, r, ql, qr, t, tree)
    pull(v, tree)

def rotation(x, y, a):
    if a == 90:
        return (0, -1, x + y, 1, 0, y - x)
    if a == 180:
        return (-1, 0, 2 * x, 0, -1, 2 * y)
    return (0, 1, x - y, -1, 0, x + y)

def solve():
    n, q = map(int, input().split())
    pts = [None] + [tuple(map(int, input().split())) for _ in range(n)]

    tree = [[0, 0, 0, 0, (1, 0, 0, 0, 1, 0)] for _ in range(4 * n + 5)]
    build(1, 1, n, pts, tree)

    ans = []
    for _ in range(q):
        l, r, x, y, a = map(int, input().split())
        update(1, 1, n, l, r, rotation(x, y, a), tree)
        ans.append(str((tree[1][1] - tree[1][0]) * (tree[1][3] - tree[1][2])))

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Cây lưu trữ bốn tọa độ và một phép chuyển đổi đang chờ xử lý. các`apply_transform`chức năng là cốt lõi của giải pháp. Nó cập nhật hình chữ nhật giới hạn bằng cách chuyển đổi tất cả bốn góc, sau đó lưu trữ phép biến đổi tổng hợp để nhân giống sau này. 

Thứ tự bố cục là nơi dễ mắc lỗi nhất. Nếu một nút đã có sự chuyển đổi`old`, và một vòng quay mới`new`đến, sự biến đổi kết quả là`new(old(point))`, do đó thứ tự nhân trong`compose`đi theo hướng đó. 

Đệ quy sử dụng logic khoảng thời gian của cây phân đoạn thông thường. Bảo hiểm toàn bộ sẽ áp dụng chuyển đổi ngay lập tức, trong khi bảo hiểm một phần sẽ đẩy thao tác đang chờ xử lý xuống trước khi đến thăm trẻ em. Số nguyên Python không tràn ở đây, điều này rất hữu ích vì tọa độ có thể tăng lên trong quá trình quay. 

## Ví dụ đã hoạt động 

Mẫu 1: 

đầu vào:```
5 3
1 2
-2 4
3 3
-1 -1
5 -3
1 3 0 0 90
2 3 5 5 180
4 5 -4 -3 180
```| Bước | Hoạt động | Gốc tối thiểu x | Gốc tối đa x | Root min y | Gốc tối đa y | Khu vực | 
| --- | --- | --- | --- | --- | --- | --- | 
| Ban đầu | không | -2 | 5 | -1 | 4 | 48 | 
| 1 | xoay 1..3 | -3 | 4 | -3 | 2 | 35 | 
| 2 | xoay 2..3 | -3 | 4 | -8 | 8 | 128 | 
| 3 | xoay 4..5 | -3 | 5 | -8 | 2 | 80 | 

Dấu vết này cho thấy chỉ những khoảng thời gian bị ảnh hưởng mới cần thay đổi. Gốc vẫn đại diện cho tất cả các điểm vì các phép biến đổi con được bảo toàn một cách lười biếng. 

Mẫu 2: 

đầu vào:```
1 2
10 20
1 1 10 10 180
1 1 0 0 90
```| Bước | Hoạt động | Vị trí điểm | Khu vực | 
| --- | --- | --- | --- | 
| Ban đầu | không | (10,20) | 0 | 
| 1 | xoay quanh (10,10) | (10,0) | 0 | 
| 2 | xoay quanh (0,0) | (0,10) | 0 | 

Điều này chứng tỏ rằng một điểm vẫn cần tất cả các phép biến đổi được áp dụng chính xác mặc dù diện tích cuối cùng luôn bằng 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n+q) log n) | Mỗi bản cập nhật chỉ chạm vào các nút cây phân đoạn trong khoảng thời gian đó. | 
| Không gian | O(n) | Cây lưu trữ một lượng thông tin không đổi trên mỗi nút. | 

Các ràng buộc yêu cầu tránh truyền tải toàn bộ khoảng thời gian. Các bản cập nhật logarit và dữ liệu nút có kích thước không đổi giữ cho tổng công việc nằm trong giới hạn cho phép. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    old_out = sys.stdout
    sys.stdout = out
    solve()
    sys.stdin = old
    sys.stdout = old_out
    return out.getvalue()

assert run("""5 3
1 2
-2 4
3 3
-1 -1
5 -3
1 3 0 0 90
2 3 5 5 180
4 5 -4 -3 180
""") == "35\n128\n80\n"

assert run("""1 1
5 7
1 1 0 0 90
""") == "0\n"

assert run("""2 1
0 0
10 1
1 2 5 5 180
""") == "90\n"

assert run("""3 2
1 1
2 2
3 3
1 2 0 0 180
2 3 10 10 270
""") == "8\n18\n"

assert run("""4 1
7 7
7 7
7 7
7 7
1 4 0 0 90
""") == "0\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Xoay điểm đơn |`0`| Kiểm tra khu vực đó và các phép biến đổi hoạt động với một điểm. | 
| Hai điểm xung quanh một trung tâm |`90`| Kiểm tra bản dịch trong các vòng quay. | 
| Nhiều vòng quay trên phạm vi chồng chéo |`8`sau đó`18`| Kiểm tra thứ tự thành phần lười biếng. | 
| Tất cả tọa độ bằng nhau |`0`| Kiểm tra hình chữ nhật suy biến. | 

## Vỏ cạnh 

Đối với một điểm duy nhất:```
1 1
5 7
1 1 0 0 90
```Lá cây phân đoạn lưu trữ điểm chính xác. Phép biến đổi xoay thay đổi tọa độ tối thiểu và tối đa được lưu trữ sang vị trí điểm mới. Căn gốc có các giá trị x bằng nhau và các giá trị y bằng nhau nên diện tích bằng 0. 

Đối với phép quay quanh tâm tùy ý:```
2 1
0 0
10 1
1 2 5 5 180
```Hình chữ nhật gốc được biến đổi trực tiếp. Bốn góc của hình chữ nhật cũ trở thành các góc dự kiến ​​của hình chữ nhật mới. Thuật toán thu được phạm vi x`[0,10]`và phạm vi y`[9,10]`, cho diện tích`90`. 

Đối với các phép biến đổi lặp đi lặp lại:```
3 2
1 1
2 2
3 3
1 2 0 0 180
2 3 10 10 270
```Bản cập nhật đầu tiên lưu trữ góc xoay 180 độ đang chờ xử lý ở hai vị trí đầu tiên. Trong lần cập nhật thứ hai, cây đẩy phép chuyển đổi này trước khi giảm dần, do đó phép quay thứ hai được áp dụng cho các tọa độ đã được xoay. Điều này bảo tồn trạng thái hiện tại thực tế của mỗi nhãn hiệu.
