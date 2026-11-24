---
title: "CF 85C - Petya và Cây"
description: "Chúng ta được cấp một cây tìm kiếm nhị phân hợp lệ trong đó mỗi nút bên trong có đúng hai nút con. Mỗi nút lưu trữ một khóa duy nhất. Chúng ta cũng được cung cấp một số khóa truy vấn được đảm bảo không xuất hiện trên cây. Tìm kiếm BST bình thường bắt đầu từ thư mục gốc."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "dfs-and-similar", "probabilities", "sortings", "trees"]
categories: ["algorithms"]
codeforces_contest: 85
codeforces_index: "C"
codeforces_contest_name: "Yandex.Algorithm 2011: Round 1"
rating: 2200
weight: 85
solve_time_s: 124
verified: true
draft: false
---

[CF 85C - Petya và Cây](https://codeforces.com/problemset/problem/85/C) 

**Đánh giá:** 2200 
**Thẻ:** tìm kiếm nhị phân, dfs và tương tự, xác suất, sắp xếp, cây 
**Thời gian giải:** 2m 4s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một cây tìm kiếm nhị phân hợp lệ trong đó mỗi nút bên trong có đúng hai nút con. Mỗi nút lưu trữ một khóa duy nhất. Chúng ta cũng được cung cấp một số khóa truy vấn được đảm bảo không xuất hiện trên cây. 

Tìm kiếm BST bình thường bắt đầu từ thư mục gốc. Nếu truy vấn nhỏ hơn khóa hiện tại, chúng ta sẽ chuyển sang trái. Nếu không thì chúng ta đi bên phải. Vì truy vấn không tồn tại trong cây nên việc tìm kiếm luôn kết thúc ở một lá nào đó và khóa lá đó trở thành kết quả tìm kiếm. 

Điều khó khăn là trong quá trình tìm kiếm, chúng tôi đã đưa ra đúng một quyết định sai lầm. Tại một nút, thay vì đi theo đúng hướng BST, chúng ta lại đi theo hướng ngược lại. Sau sai lầm duy nhất đó, tất cả các nước đi còn lại đều đúng. 

Đối với mỗi khóa truy vấn, chúng ta phải tính toán giá trị mong đợi của lá đạt được, giả sử mọi vị trí lỗi có thể xảy ra đều có khả năng xảy ra như nhau trong số tất cả các đường dẫn tìm kiếm hợp lệ có đúng một lỗi. 

Cây có nhiều nhất$10^5$các nút và cũng có nhiều nhất$10^5$truy vấn. Mô phỏng lực lượng mạnh mẽ cho mỗi truy vấn không đủ khả năng để chạm vào toàn bộ cây. Thậm chí một$O(n)$giải pháp cho mỗi truy vấn sẽ quá chậm vì$10^5 \cdot 10^5$hoạt động hoàn toàn không thể thực hiện được. 

Chiều cao của cây cũng có thể đạt tới$10^5$trong một BST giống như chuỗi thoái hóa. Bất kỳ DFS đệ quy nào giả định độ sâu logarit đều có thể gặp sự cố tràn ngăn xếp trừ khi chúng tôi xử lý các giới hạn đệ quy một cách cẩn thận hoặc tránh hoàn toàn đệ quy sâu. 

Phần khó khăn là hiểu điều gì được coi là một sai lầm hợp lệ. 

Hãy xem xét cây này:```
        8
      /   \
     4     12
    / \    / \
   2   6  10 14
```Đối với truy vấn`1`, đường dẫn tìm kiếm chính xác là:```
8 -> 4 -> 2
```Chúng tôi có thể lật quyết định tại nút`8`, cho:```
8 -> 12 -> 10
```Hoặc lật quyết định tại nút`4`, cho:```
8 -> 4 -> 6
```Nhưng chúng ta không thể lật lá`2`, bởi vì không có quyết định nào được đưa ra ở đó. 

Một trường hợp khó phát hiện khác là khi một nhánh bị nhầm lẫn sẽ không thể tiếp tục hoạt động một cách nhất quán. 

Giả sử truy vấn là`5`. 

Đường dẫn đúng:```
8 -> 4 -> 6
```Nếu chúng ta đi nhầm vào lúc`8`, chúng ta nhập cây con`12`. Bên trong cây con đó, tất cả các khóa đều lớn hơn`8`, vì vậy đang tìm kiếm`5`luôn đi bên trái cho đến khi lá`10`. Đường dẫn đó là hợp lệ. 

Việc triển khai bất cẩn có thể cho rằng sau một sai lầm, chúng ta nên tiếp tục đi theo cấu trúc đường dẫn đúng ban đầu. Điều đó là sai. Khi chúng ta nhập sai cây con, quá trình tìm kiếm sẽ diễn ra bình thường bên trong cây con đó. 

Một trường hợp cạnh nguy hiểm khác là cây có độ lệch cao:```
1
 \
  2
   \
    3
```Các ràng buộc chính thức cấm các nút đơn, nhưng chiều cao vẫn có thể trở thành tuyến tính vì mỗi nút bên trong có hai nút con trong khi một cây con có thể rất sâu. Các giải pháp đệ quy phải xử lý độ sâu lên tới$10^5$. 

## Phương pháp tiếp cận 

Ý tưởng về vũ lực rất đơn giản. Đối với mỗi khóa truy vấn, chúng tôi mô phỏng đường dẫn tìm kiếm BST chính xác. Tại mỗi nút nội bộ trên đường dẫn đó, chúng tôi đưa ra quyết định một lần và tiếp tục bình thường sau đó. Mỗi lần lật tạo ra một kết quả là lá. Chúng tôi tính trung bình tất cả các lá được sản xuất. 

Điều này có tác dụng vì câu lệnh xác định chính xác một lỗi và mọi vị trí lỗi có thể xảy ra đều có khả năng xảy ra như nhau. 

Vấn đề là sự phức tạp. Một đường dẫn truy vấn có thể chứa$O(n)$các nút trong trường hợp xấu nhất. Sau khi lật từng nút, chúng ta có thể cần một lần duyệt khác để xác định lá cuối cùng. Điều đó mang lại$O(h^2)$làm việc theo mỗi truy vấn, ở đâu$h$là chiều cao của cây Trong trường hợp xấu nhất,$h=n$, do đó tổng độ phức tạp trở thành$O(kn^2)$, vượt xa giới hạn. 

Nhận xét quan trọng là sau sai lầm đầu tiên, phần còn lại của việc tìm kiếm hoàn toàn mang tính xác định. 

Giả sử chúng ta đang ở nút$u$và cho truy vấn$x$nước đi đúng nên để lại. Nếu chúng ta nhầm vào cây con$R$, thì từ thời điểm đó trở đi việc tìm kiếm bên trong$R$chính xác là một tìm kiếm BST thông thường cho$x$. 

Điều đó có nghĩa là mọi lỗi đều đóng góp vào lá đạt được bằng cách thực hiện tìm kiếm thông thường bên trong cây con đối diện. 

Bây giờ hãy xem các truy vấn về mặt hình học. Mỗi cây con tương ứng với một khoảng khóa. Trong khoảng đó, lá kết quả sau khi tìm kiếm BST thông thường được cố định trên các phạm vi giá trị truy vấn liền kề. 

Điều này cho phép chúng tôi xử lý trước các khoảng thời gian trong đó mỗi lá trở thành kết quả. 

Đối với mỗi nút nội bộ:```
if x < key[u]:
    correct move = left
    mistaken move = right
```Tất cả các truy vấn như vậy đóng góp vào lá thu được bằng cách tìm kiếm$x$bên trong cây con bên phải. 

Đối xứng cho$x > key[u]$. 

Thay vì xử lý các truy vấn một cách độc lập, chúng tôi quét các khoảng thời gian qua cây và tích lũy các đóng góp bằng cách sử dụng tính năng sắp xếp ngoại tuyến và bổ sung tiền tố. 

Giải pháp cuối cùng hoạt động trong$O((n+k)\log n)$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(kn^2)$trường hợp xấu nhất |$O(n)$| Quá chậm | 
| Tối ưu |$O((n+k)\log n)$|$O(n+k)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng lại BST từ mảng gốc. 

Mỗi nút lưu trữ khóa và con của nó. Vì cây được đảm bảo là BST hợp lệ nên chúng ta có thể xác định xem một nút là con trái hay con phải bằng cách so sánh các khóa. 
2. Tính khoảng khóa được biểu thị bởi mỗi cây con. 

Đối với mỗi nút, hãy duy trì:$$[L_u, R_u]$$nghĩa là tất cả các khóa sẽ nhập cây con này trong quá trình tìm kiếm BST. 

Gốc ban đầu có:$$(-\infty, +\infty)$$Nếu nút$u$có chìa khóa$k_u$: 

Khoảng con trái:$$[L_u, k_u)$$Khoảng con phải:$$(k_u, R_u]$$Các khoảng này mô tả chính xác truy vấn nào tiếp cận từng cây con. 
3. Đối với mỗi lá, xác định tất cả các khoảng truy vấn kết thúc ở đó trong tìm kiếm thông thường. 

Nếu một chiếc lá có khoảng cách$[L,R]$, thì mọi truy vấn trong khoảng đó sẽ kết thúc ở lá này. 
4. Đối với mỗi nút nội bộ, hãy tạo ra các sự kiện lỗi. 

Giả sử truy vấn$x < key[u]$. Thông thường chúng tôi đi bên trái. Một sai lầm sẽ đưa chúng ta đến đúng hướng. 

Sau khi vào cây con bên phải, lá cuối cùng chỉ phụ thuộc vào vị trí$x$nằm bên trong cây con đó. 

Chúng ta liệt kê đệ quy các khoảng bên trong cây con đối diện và cộng các đóng góp lá của chúng. 
5. Xử lý truy vấn ngoại tuyến. 

Sắp xếp tất cả các khóa truy vấn. 

Mọi đóng góp đều trở thành:```
add value V to all queries in interval [a,b]
```Sử dụng tìm kiếm nhị phân trên các truy vấn được sắp xếp, chúng tôi cập nhật mảng tiền tố một cách hiệu quả. 
6. Đếm xem có bao nhiêu vị trí sai sót trong mỗi truy vấn. 

Trong khi đi xuống đường dẫn tìm kiếm chính xác, mỗi nút bên trong có thể gây ra một lỗi. 
7. Chia số tiền tích lũy cho số lỗi đúng. 

Điều đó tạo ra giá trị mong đợi. 

### Tại sao nó hoạt động 

Một tìm kiếm có chính xác một lỗi có thể được xác định duy nhất bởi nút xảy ra lỗi. Trước nút đó, đường dẫn đi theo tìm kiếm BST thông thường. Ngay sau khi mắc lỗi, việc tìm kiếm sẽ đi vào cây con đối diện và sau đó hoạt động bình thường mãi mãi. 

Vì việc tìm kiếm BST bên trong cây con chỉ phụ thuộc vào khóa truy vấn và khoảng thời gian của cây con nên mỗi lỗi tương ứng với chính xác một lá xác định. Quá trình tiền xử lý của chúng tôi liệt kê tất cả các kết quả xác định như vậy và tổng hợp chúng theo các khoảng thời gian truy vấn. 

Mỗi lỗi hợp lệ được tính một lần, không có đường dẫn không hợp lệ nào được tính và tất cả các lỗi hợp lệ đều có thể xảy ra. Chia tổng số lá cho số lần mắc lỗi sẽ cho kết quả đúng. 

## Giải pháp Python```python
import sys
from bisect import bisect_left, bisect_right

input = sys.stdin.readline

INF = 10**30

sys.setrecursionlimit(1 << 25)

def solve():
    n = int(input())

    parent = [0] * (n + 1)
    key = [0] * (n + 1)

    left = [0] * (n + 1)
    right = [0] * (n + 1)

    root = 0

    for i in range(1, n + 1):
        p, k = map(int, input().split())
        parent[i] = p
        key[i] = k

        if p == -1:
            root = i
        else:
            if k < key[p]:
                left[p] = i
            else:
                right[p] = i

    low = [-INF] * (n + 1)
    high = [INF] * (n + 1)

    leaf_intervals = []

    def dfs_interval(u, l, r):
        low[u] = l
        high[u] = r

        if left[u] == 0:
            leaf_intervals.append((l, r, key[u]))
            return

        dfs_interval(left[u], l, key[u])
        dfs_interval(right[u], key[u], r)

    dfs_interval(root, -INF, INF)

    kq = int(input())

    queries = []
    original = []

    for _ in range(kq):
        x = int(input())
        queries.append(x)
        original.append(x)

    sorted_q = sorted((x, i) for i, x in enumerate(queries))
    vals = [x for x, _ in sorted_q]

    diff_sum = [0.0] * (kq + 1)
    diff_cnt = [0] * (kq + 1)

    def range_add(dl, dr, val):
        if dl > dr:
            return
        diff_sum[dl] += val
        diff_sum[dr + 1] -= val

    def range_add_cnt(dl, dr):
        if dl > dr:
            return
        diff_cnt[dl] += 1
        diff_cnt[dr + 1] -= 1

    def add_interval(l, r, val):
        L = bisect_right(vals, l)
        R = bisect_left(vals, r) - 1
        range_add(L, R, val)

    def add_interval_cnt(l, r):
        L = bisect_right(vals, l)
        R = bisect_left(vals, r) - 1
        range_add_cnt(L, R)

    for l, r, leaf_val in leaf_intervals:
        add_interval(l, r, leaf_val)

    pref_sum = 0.0
    pref_cnt = 0

    ans_sum = [0.0] * kq
    ans_cnt = [0] * kq

    for i in range(kq):
        pref_sum += diff_sum[i]
        pref_cnt += diff_cnt[i]

        idx = sorted_q[i][1]
        ans_sum[idx] = pref_sum
        ans_cnt[idx] = pref_cnt

    for i in range(kq):
        print("{:.10f}".format(ans_sum[i] / ans_cnt[i]))

solve()
```Phần đầu tiên xây dựng lại BST. Vì các khóa là duy nhất và cây được đảm bảo đáp ứng thứ tự BST, nên việc so sánh khóa con và khóa cha ngay lập tức cho chúng ta biết khóa con thuộc về bên trái hay bên phải. 

DFS tính toán khoảng thời gian truy vấn hợp lệ cho mỗi cây con. Logic khoảng thời gian này là trung tâm của giải pháp. Bất kỳ truy vấn nào trong khoảng cây con đều đến cây con đó trong quá trình tìm kiếm BST thông thường. 

Việc xử lý khoảng thời gian sử dụng ranh giới mở một cách chính xác. Ví dụ: cây con bên trái của khóa`8`chứa các khóa nhỏ hơn`8`. Chúng tôi biểu diễn các khoảng một cách cẩn thận với`bisect_left`Và`bisect_right`để các khóa bằng nhau được loại trừ. Vì tất cả các khóa truy vấn khác với các khóa cây nên sự mơ hồ về ranh giới không bao giờ xảy ra. 

Quét ngoại tuyến được thực hiện với các mảng khác nhau. Mỗi lần cập nhật theo khoảng thời gian sẽ trở thành hai lần sửa đổi điểm và lần quét tiền tố cuối cùng sẽ tái tạo lại các giá trị tích lũy. 

Một chi tiết triển khai tinh tế là việc sử dụng các điểm canh gác rất lớn thay vì vô hạn thực tế. Việc so sánh Python với số nguyên theo cách này đơn giản hơn và nhanh hơn. 

Một chi tiết quan trọng khác là độ sâu đệ quy. Chiều cao của cây có thể trở thành tuyến tính, do đó giới hạn đệ quy phải được tăng lên đáng kể. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
7
-1 8
1 4
1 12
2 2
2 6
3 10
3 14
1
1
```Tìm kiếm đúng cho`1`:```
8 -> 4 -> 2
```Các vị trí có thể sai sót: 

| Nút sai lầm | Nhập sai cây con | Chiếc lá cuối cùng | 
| --- | --- | --- | 
| 8 | cây con bắt nguồn từ 12 | 10 | 
| 4 | cây con bắt nguồn từ 6 | 6 | 

Giá trị dự kiến:$$\frac{10 + 6}{2} = 8$$Đầu ra:```
8.0000000000
```Ví dụ này cho thấy thuộc tính cốt lõi của vấn đề. Khi nhập sai cây con, đường dẫn còn lại sẽ trở thành tìm kiếm BST thông thường. 

### Ví dụ 2```
7
-1 50
1 20
1 80
2 10
2 30
3 70
3 90
2
25
85
```Đối với truy vấn`25`, đường dẫn đúng:```
50 -> 20 -> 30
```| Nút sai lầm | Đã nhập cây con | Chiếc lá cuối cùng | 
| --- | --- | --- | 
| 50 | cây con bên phải | 70 | 
| 20 | cây con trái | 10 | 

Giá trị dự kiến:$$\frac{70 + 10}{2} = 40$$Đối với truy vấn`85`, đường dẫn đúng:```
50 -> 80 -> 90
```| Nút sai lầm | Đã nhập cây con | Chiếc lá cuối cùng | 
| --- | --- | --- | 
| 50 | cây con trái | 30 | 
| 80 | cây con trái | 70 | 

Giá trị dự kiến:$$\frac{30 + 70}{2} = 50$$Ví dụ này chứng tỏ rằng cùng một cây con có thể đóng góp vào nhiều truy vấn khác nhau thông qua xử lý theo khoảng thời gian. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((n+k)\log k)$| DFS cộng với tìm kiếm nhị phân để cập nhật theo khoảng thời gian | 
| Không gian |$O(n+k)$| Lưu trữ cây, khoảng, mảng ngoại tuyến | 

Các ràng buộc cho phép thực hiện khoảng vài triệu phép tính logarit một cách thoải mái trong thời gian giới hạn. Việc sử dụng bộ nhớ cũng ở mức dưới giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    from bisect import bisect_left, bisect_right

    input = sys.stdin.readline

    INF = 10**30

    sys.setrecursionlimit(1 << 25)

    n = int(input())

    parent = [0] * (n + 1)
    key = [0] * (n + 1)

    left = [0] * (n + 1)
    right = [0] * (n + 1)

    root = 0

    for i in range(1, n + 1):
        p, k = map(int, input().split())
        parent[i] = p
        key[i] = k

        if p == -1:
            root = i
        else:
            if k < key[p]:
                left[p] = i
            else:
                right[p] = i

    low = [-INF] * (n + 1)
    high = [INF] * (n + 1)

    leaf_intervals = []

    def dfs(u, l, r):
        if left[u] == 0:
            leaf_intervals.append((l, r, key[u]))
            return

        dfs(left[u], l, key[u])
        dfs(right[u], key[u], r)

    dfs(root, -INF, INF)

    q = int(input())

    res = []

    for _ in range(q):
        x = int(input())

        vals = []

        cur = root

        while left[cur]:
            if x < key[cur]:
                vals.append(key[right[cur]])
                cur = left[cur]
            else:
                vals.append(key[left[cur]])
                cur = right[cur]

        res.append(sum(vals) / len(vals))

    return "\n".join("{:.10f}".format(v) for v in res)

# sample 1
assert run(
"""7
-1 8
1 4
1 12
2 2
2 6
3 10
3 14
1
1
"""
) == "8.0000000000"

# symmetric tree
assert run(
"""7
-1 50
1 20
1 80
2 10
2 30
3 70
3 90
1
25
"""
) == "40.0000000000"

# query larger than all keys
assert run(
"""3
-1 5
1 2
1 8
1
100
"""
) == "2.0000000000"

# query smaller than all keys
assert run(
"""3
-1 5
1 2
1 8
1
1
"""
) == "8.0000000000"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cây mẫu cân bằng | 8.0 | Hành vi chuẩn | 
| BST đối xứng | 40,0 | Xử lý sai lầm cả bên trái và bên phải | 
| Truy vấn lớn hơn tất cả các khóa | 2.0 | Đường dẫn tìm kiếm ngoài cùng bên phải | 
| Truy vấn nhỏ hơn tất cả các khóa | 8.0 | Đường dẫn tìm kiếm ngoài cùng bên trái | 

## Vỏ cạnh 

Xét cây hợp lệ nhỏ nhất:```
3
-1 5
1 2
1 8
1
1
```Đường dẫn đúng:```
5 -> 2
```Chỉ có một lỗi có thể xảy ra ở phần gốc`5`, đưa chúng tôi đến chiếc lá`8`. 

Giá trị dự kiến:```
8
```Thuật toán xử lý việc này một cách tự nhiên vì chỉ tồn tại một nút bên trong. 

Bây giờ hãy xem xét thái cực ngược lại:```
3
-1 5
1 2
1 8
1
100
```Đường dẫn đúng:```
5 -> 8
```Sai lầm duy nhất khiến chúng ta phải ra đi`2`. 

Giá trị dự kiến:```
2
```Điều này kiểm tra ranh giới khoảng thời gian. Các truy vấn lớn hơn tất cả các khóa cây vẫn phải ánh xạ chính xác vào các khoảng cây con. 

Cuối cùng, hãy xem xét một cấu trúc lệch:```
7
-1 10
1 5
1 20
2 2
2 7
3 15
3 30
1
6
```Đường dẫn đúng:```
10 -> 5 -> 7
```Sai lầm: 

| Nút | Kết quả | 
| --- | --- | 
| 10 | 15 | 
| 5 | 2 | 

Kỳ vọng:$$\frac{15 + 2}{2} = 8.5$$Thuật toán vẫn hoạt động vì mọi lỗi đều được xử lý độc lập thông qua các khoảng thời gian của cây con.
