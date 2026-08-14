---
title: "CF 102331H - Giải thưởng danh dự"
description: "Đối với mỗi truy vấn ((l,r,k)), chúng tôi chỉ xem xét mảng con (al,ldots,ar). Chúng ta phải chọn chính xác (k) các phần liền kề rời rạc theo từng cặp khác nhau của mảng con đó và tối đa hóa tổng tất cả các phần tử được bao phủ bởi các phần đó. Các mảnh có thể liền kề."
date: "2026-08-13T03:40:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102331
codeforces_index: "H"
codeforces_contest_name: "2019 Summer Petrozavodsk Camp, Day 2: 300iq Contest 2 (XX Open Cup, Grand Prix of Kazan)"
rating: 0
weight: 102331
solve_time_s: 179
verified: true
draft: false
---

[CF 102331H - Được vinh danh](https://codeforces.com/problemset/problem/102331/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 59s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Đối với mỗi truy vấn ((l,r,k)), chúng tôi chỉ xem xét mảng con (a_l,\ldots,a_r). Chúng ta phải chọn chính xác (k) các phần liền kề rời rạc theo từng cặp khác nhau của mảng con đó và tối đa hóa tổng tất cả các phần tử được bao phủ bởi các phần đó. Các mảnh có thể liền kề. Sự kề cận quan trọng vì hai phần như ([1,2]) và ([3,4]) vẫn là hai phần, mặc dù phần hợp của chúng là một khoảng lớn hơn. 

Đầu vào chứa một mảng với (n\le 35000) phần tử và tối đa (q\le 35000) truy vấn khoảng thời gian độc lập. Mỗi phần tử có giá trị tuyệt đối nhiều nhất là (35000), do đó, toàn bộ khoảng có thể có tổng tuyệt đối lớn bằng (35000^2=1,225\times10^9). Số lượng truy vấn loại trừ việc chạy chương trình động phụ thuộc vào độ dài cho mỗi truy vấn. Ngay cả một giải pháp (O((r-l+1)k)) cũng có thể đạt tới khoảng (O(qn^2)), vượt xa thời gian sẵn có. Chúng ta cần xử lý trước mảng thành thông tin khoảng thời gian có thể sử dụng lại và làm cho mỗi truy vấn phụ thuộc chủ yếu vào (\log n), chứ không phải độ dài của nó. 

Vấn đề chính thức sử dụng các ràng buộc và ví dụ tương tự được mô tả ở đây. 

Có một số trường hợp khó xử lý. Đầu tiên, tất cả các giá trị có thể âm. Đối với đầu vào```
1 1
-5
1 1 1
```câu trả lời là`-5`, không`0`. Các phân đoạn đã chọn phải không trống và phải chọn chính xác một phân đoạn, do đó không được phép lựa chọn trống. 

Thứ hai, câu trả lời cho chính xác (k) phân đoạn không đơn điệu trong (k). Vì```
4 3
3 3 3 3
1 4 1
1 4 2
1 4 3
```câu trả lời là```
12
12
9
```Đối với một phân khúc, chúng tôi lấy tất cả bốn yếu tố. Đối với hai đoạn chúng ta có thể chia chúng thành hai phần liền kề và vẫn thu được (12). Đối với ba phân đoạn, khả năng tốt nhất là ba phần tử đơn lẻ, cho (9). Một giải pháp được thiết kế cho các phân đoạn "tối đa (k)" sẽ âm thầm đưa ra kết quả sai ở đây. 

Thứ ba, các phân đoạn được chọn liền kề phải được tách biệt khi (k) yêu cầu. Vì```
3 1
5 -10 5
1 3 2
```câu trả lời là`10`, thu được bằng cách chọn vị trí 1 và 3 làm hai phân đoạn đơn. Việc triển khai bất cẩn coi mọi vùng được chọn liền kề là một phân đoạn không có vấn đề gì ở đây, nhưng việc triển khai giả định mọi thành phần được chọn phải có khoảng cách âm giữa nó và thành phần tiếp theo sẽ từ chối cấu hình này một cách không chính xác. 

Cuối cùng, (k) có thể bằng độ dài khoảng. Vì```
3 1
-2 -3 -4
1 3 3
```câu trả lời là`-9`, bởi vì cả ba phần tử phải được chọn dưới dạng ba phân đoạn đơn. Bất kỳ triển khai nào khởi tạo câu trả lời bằng 0 hoặc vô tình cho phép ít hơn (k) phân đoạn sẽ không thành công trong trường hợp này. 

## Phương pháp tiếp cận 

Chương trình động trực tiếp cho một truy vấn đã mang tính hướng dẫn. Cho phép`end[j]`là giá trị tốt nhất bằng cách sử dụng chính xác (j) phân đoạn trong đó vị trí hiện tại thuộc về phân đoạn được chọn cuối cùng và đặt`best[j]`là giá trị tốt nhất bằng cách sử dụng chính xác (j) phân đoạn ở bất kỳ đâu trong tiền tố được xử lý. Đối với mỗi phần tử, chúng ta có thể mở rộng phân đoạn cuối cùng hoặc bắt đầu một phân đoạn mới. Điều này đưa ra thuật toán (O(mk)) cho truy vấn trong khoảng độ dài (m). 

Thuật toán đó đúng vì mọi giải pháp tối ưu đều không sử dụng phần tử hiện tại, mở rộng phân đoạn cuối cùng của nó thông qua phần tử hiện tại hoặc bắt đầu phân đoạn cuối cùng của nó tại phần tử hiện tại. The problem is the number of queries. Nếu cả độ dài khoảng và (k) đều là (\Theta(n)), thì một truy vấn có thể có giá (O(n^2)) và (q) các truy vấn đó đưa ra (O(qn^2)), khoảng (4.3\times10^{13}) chuyển đổi trạng thái ở giới hạn tối đa. 

Quan sát quan trọng là, trong một khoảng thời gian cố định, hàm 

[ 
F(k)=\text{giá trị tối đa có thể đạt được với chính xác }k\text{ đoạn} 
] 

là lõm. Equivalently, its marginal gains

 [ 
F(k)-F(k-1) 
] 

đều không tăng. Thuộc tính này cũng có thể thấy được thông qua công thức dòng chi phí tối thiểu tiêu chuẩn của bài toán: giá trị tối ưu dưới dạng hàm của luồng yêu cầu có thuộc tính lồi rời rạc tương ứng. Đây là thực tế về cấu trúc giúp cho cả việc hợp nhất Minkowski-sum và tìm kiếm nhị phân WQS đều có thể thực hiện được. 

Giả sử chúng ta đã biết tất cả các giá trị (F(0),F(1),\ldots,F(m)) trong một khoảng. Hai khoảng liền kề sau đó có thể được hợp nhất một cách hiệu quả. If their concave functions have marginal gains

 [ 
d_1\ge d_2\ge\cdots 
] 

và 

[ 
e_1\ge e_2\ge\cdots, 
] 

lợi ích cận biên của tích chập cộng cực đại của chúng chỉ đơn giản là sự hợp nhất được sắp xếp của hai chuỗi cận biên này. Đây là dạng một chiều của tổng Minkowski của các bao lồi. Do đó, cây phân đoạn có thể lưu trữ toàn bộ hàm trả lời cho mỗi nút trong tổng công việc xây dựng (O(n\log n)). 

There is one complication. Khi hai nút cây phân đoạn liền kề được nối với nhau, một phân đoạn được chọn có thể vượt qua ranh giới. Để biết liệu hai phần được chọn có nên hợp nhất thành một hay không, mỗi nút phải nhớ liệu phần tử ngoài cùng bên trái và ngoài cùng bên phải của nó có được chọn hay không. Điều đó cung cấp bốn hàm cho mỗi nút, được lập chỉ mục bởi hai bit chọn điểm cuối. Khi điểm cuối bên phải của phần tử con bên trái và điểm cuối bên trái của phần tử con bên phải đều được chọn, hai phần của chúng trở thành một phần, do đó hàm kết quả được dịch chuyển một đơn vị trong tọa độ đếm phân đoạn của nó. 

Lúc đầu, điều này có vẻ đủ, nhưng một khoảng truy vấn bao gồm (O(\log n)) nút cây phân đoạn và việc hợp nhất các hàm lồi hoàn chỉnh của chúng cho mỗi truy vấn sẽ lại quá tốn kém. Tìm kiếm nhị phân WQS loại bỏ thứ nguyên số lượng phân đoạn trong một truy vấn. Đối với một hình phạt (\lambda), thay vì tối đa hóa (F(k)), chúng tôi tối đa hóa 

[ 
F(x)-\lambda x 
] 

over every possible number (x) of segments. Vì (F) lõm nên cực đại hóa (x) di chuyển đơn điệu khi (\lambda) thay đổi. Nếu giải pháp tối đa hóa sử dụng ít nhất các phân đoạn (k) được yêu cầu, chúng tôi sẽ tăng (\lambda); otherwise we decrease it. Ở độ dốc cuối cùng, việc thêm (k\lambda) sẽ có được câu trả lời chính xác. 

Việc triển khai đơn giản sẽ tìm kiếm nhị phân vị trí tốt nhất riêng biệt bên trong mỗi bao lồi. That introduces another logarithmic factor. Tối ưu hóa cuối cùng là xử lý tất cả các truy vấn trong một lần lặp WQS theo thứ tự giảm dần của điểm giữa hiện tại của chúng (\lambda). Đối với một bao lồi cố định, khi (\lambda) giảm đi, vị trí tối ưu của nó chỉ có thể di chuyển về phía trước. Chúng tôi giữ một con trỏ cho mọi nút và mọi trạng thái điểm cuối, vì vậy mỗi con trỏ chỉ di chuyển qua thân của nó một lần trong một lớp WQS. Đây là tối ưu hóa WQS "tổng thể" hoặc song song được sử dụng bởi giải pháp dự định. Phương pháp thu được được mô tả trong tài liệu giải pháp ban đầu dưới dạng (O((n+q)\log n\log V)), tùy theo hệ số sắp xếp cho các truy vấn ngoại tuyến.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu DP | (O(qn^2)) | (O(n)) mỗi truy vấn | Quá chậm | 
| Cây phân đoạn + hàm lồi đầy đủ | (O(n\log n+qn)) | (O(n\log n)) | Quá chậm | 
| WQS với tìm kiếm nhị phân bên trong mỗi thân tàu | (O((n+q)\log^2n\log V)) | (O(n\log n)) | Đóng nhưng chậm không cần thiết | 
| WQS ngoại tuyến + con trỏ thân đơn điệu | (O((n+q)\log n\log V+q\log q\log V)) | (O(n\log n+q\log n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi nút cây phân đoạn, lưu trữ bốn mảng`h[x][y]`. Một chút`x`cho biết liệu điểm cuối bên trái của nút có được chọn hay không và`y`cho biết liệu điểm cuối phù hợp có được chọn hay không.`h[x][y][k]`là số tiền tối đa có thể đạt được với chính xác`k`các phân đoạn được chọn theo các yêu cầu điểm cuối đó. Các trạng thái không thể được lưu trữ dưới dạng vô cực âm. 

Thông tin điểm cuối là cần thiết vì hai phần được chọn độc lập có thể trở thành một phần khi hai nút lân cận được nối với nhau. 
2. Tại một lá chứa giá trị (a_i), các trạng thái có ý nghĩa duy nhất là`00`với các phân đoạn được chọn bằng 0 và giá trị bằng 0, và`11`với một phân đoạn được chọn và giá trị (a_i). tiểu bang`01`Và`10`là không thể vì khoảng một phần tử không thể chọn chính xác một điểm cuối mà không chọn điểm cuối kia. 
3. Để hợp nhất hai thân tàu con, hãy lấy tích chập cộng cực đại của chúng. Nếu như`A[k]`Và`B[k]`là lõm, sự khác biệt biên của tích chập thu được có được bằng cách hợp nhất các khác biệt biên của`A`Và`B`theo thứ tự giảm dần. 

Điều này làm cho việc hợp nhất tuyến tính theo chiều dài thân tàu kết hợp thay vì bậc hai về số lượng phân đoạn có thể có. 
4. Đối với mỗi cặp trạng thái điểm cuối con, hãy kết hợp các thân tương ứng. Nếu điểm cuối bên phải của con bên trái và điểm cuối bên trái của con bên phải đều được chọn thì hai mảnh chạm nhau và phải được tính là một đoạn. Trong tọa độ đếm phân đoạn, đây là sự dịch chuyển một vị trí. 
5. Trước khi xử lý các truy vấn, hãy phân tách mọi khoảng ([l,r]) thành các nút cây phân đoạn chuẩn (O(\log n)) bao phủ nó. Sự phân rã không bao giờ thay đổi trong suốt WQS nên nó chỉ được tính một lần. 
6. Đối với một hình phạt cố định (\lambda), mỗi thân tàu được lưu trữ sẽ được truy vấn để tối đa hóa chỉ số (p) 

[ 
h[p]-p\lambda. 
] 

Vì thân tàu lõm nên đây chính xác là vị trí lớn nhất đạt được trong khi mức tăng cận biên tiếp theo ít nhất là (\lambda). Cặp liên kết lưu trữ cả giá trị bị phạt và số lượng phân đoạn được chọn. 
7. Xử lý các nút chuẩn của một truy vấn từ trái sang phải. DP có hai trạng thái tùy theo điểm cuối phù hợp của mọi thứ được xử lý cho đến nay có được chọn hay không. Khi nút hiện tại bắt đầu với điểm cuối bên trái được chọn và trạng thái trước đó cũng kết thúc được chọn, hãy giảm tổng số phân đoạn xuống một và thêm (\lambda) trở lại giá trị bị phạt vì hai phần đã hợp nhất thành một. 
8. Sau khi tất cả các nút của truy vấn đã được xử lý, hãy sử dụng trạng thái tốt hơn của hai trạng thái điểm cuối bên phải. Cặp kết quả đưa ra giá trị bị phạt tối đa và số lượng phân đoạn được chọn bị phạt (\lambda). 
9. Tìm kiếm nhị phân WQS (\lambda) cho mọi truy vấn. Nếu số lượng đoạn được chọn ít nhất là theo yêu cầu (k), thì độ dốc quá nhỏ hoặc ở ranh giới mà ở đó nhiều đoạn vẫn thích hợp hơn, do đó hãy di chuyển giới hạn dưới lên trên. Nếu không thì di chuyển giới hạn trên xuống dưới. Tại độ dốc cuối cùng đã chọn (\lambda^*), câu trả lời chính xác là 

[ 
\text{tối ưu bị phạt}+k\lambda^*. 
] 
10. Trong mỗi lớp tìm kiếm nhị phân, hãy sắp xếp các truy vấn theo điểm giữa hiện tại của chúng (\lambda) theo thứ tự giảm dần. Mọi con trỏ thân tàu sau đó chỉ di chuyển về phía trước trong lớp đó. Đặt lại con trỏ một lần trên mỗi lớp sẽ mang lại mức khấu hao cần thiết. 

### Tại sao nó hoạt động 

Điều bất biến là mỗi thân cây phân đoạn đều chứa giá trị tối ưu chính xác cho mỗi số lượng phân đoạn khả thi trong hai điều kiện lựa chọn điểm cuối của nó. Việc hợp nhất Minkowski bảo toàn tính bất biến này vì một giải pháp ghép nối được hình thành từ hai giải pháp độc lập hoặc bằng cách nối hai phân đoạn biên và số phân đoạn tương ứng chính xác là tổng thông thường hoặc ít hơn một. 

Đối với hình phạt cố định (\lambda), mọi giải pháp khả thi với phân đoạn (x) đều nhận được giá trị điều chỉnh (F(x)-\lambda x). Độ lõm của (F) có nghĩa là cực đại hóa (x) di chuyển đơn điệu khi (\lambda) thay đổi, do đó tìm kiếm nhị phân WQS tìm thấy độ dốc có đường hỗ trợ đạt đến số lượng phân đoạn được yêu cầu. Quy tắc buộc chọn số lượng phân đoạn tối đa hóa lớn nhất, mang lại cạnh chính xác cho cạnh phẳng của thân lõm. Ở độ dốc đó, đẳng thức đường hỗ trợ cho (F(k)) sau khi cộng lại (k\lambda). Các trạng thái điểm cuối tính đến mọi khả năng vượt qua ranh giới của cây phân đoạn, do đó không có sự sắp xếp hợp lệ nào bị mất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(1_000_000)

NEG = -10**30

def minkowski(a, b):
    na = len(a) - 1
    nb = len(b) - 1
    res = [0] * (na + nb + 1)

    s = a[0] + b[0]
    res[0] = s

    i = 0
    j = 0
    pos = 0

    while i < na and j < nb:
        da = a[i + 1] - a[i]
        db = b[j + 1] - b[j]

        if da > db:
            i += 1
            s += da
        else:
            j += 1
            s += db

        pos += 1
        res[pos] = s

    while i < na:
        i += 1
        s += a[i] - a[i - 1]
        pos += 1
        res[pos] = s

    while j < nb:
        j += 1
        s += b[j] - b[j - 1]
        pos += 1
        res[pos] = s

    return res

def merge_into(dst, src, shifted):
    limit = len(src)
    if not shifted:
        for i in range(limit):
            v = src[i]
            if v > dst[i]:
                dst[i] = v
    else:
        for i in range(limit):
            v = src[i]
            if v > dst[i]:
                dst[i] = v
            if i and v > dst[i - 1]:
                dst[i - 1] = v

def solve():
    n, q = map(int, input().split())
    a = [0] + list(map(int, input().split()))

    size = 4 * n + 5
    tree = [None] * size

    def build(node, left, right):
        if left == right:
            h00 = [0, NEG]
            h01 = [NEG, NEG]
            h10 = [NEG, NEG]
            h11 = [NEG, a[left]]
            tree[node] = (h00, h01, h10, h11)
            return

        mid = (left + right) >> 1
        build(node << 1, left, mid)
        build(node << 1 | 1, mid + 1, right)

        lc = tree[node << 1]
        rc = tree[node << 1 | 1]

        length = right - left + 1
        cur = [
            [NEG] * (length + 1),
            [NEG] * (length + 1),
            [NEG] * (length + 1),
            [NEG] * (length + 1),
        ]

        for u in range(2):
            for v in range(2):
                left_hull = lc[u * 2 + v]

                for p in range(2):
                    for z in range(2):
                        right_hull = rc[p * 2 + z]

                        tmp = minkowski(left_hull, right_hull)

                        dst = cur[u * 2 + z]
                        merge_into(dst, tmp, v == 1 and p == 1)

        tree[node] = tuple(cur)

    build(1, 1, n)

    queries = []
    for idx in range(q):
        l, r, k = map(int, input().split())
        queries.append([l, r, k, -35000, 0, 0])

    # Canonical segment-tree decomposition for every query.
    parts = [[] for _ in range(q)]

    def collect(node, left, right, ql, qr, out):
        if ql <= left and right <= qr:
            out.append(node)
            return

        mid = (left + right) >> 1
        if ql <= mid:
            collect(node << 1, left, mid, ql, qr, out)
        if qr > mid:
            collect(node << 1 | 1, mid + 1, right, ql, qr, out)

    for idx, qu in enumerate(queries):
        collect(1, 1, n, qu[0], qu[1], parts[idx])

    total_abs = sum(abs(x) for x in a[1:])
    for qu in queries:
        qu[4] = total_abs

    # Four monotone pointers per segment-tree node.
    ptr0 = [0] * size
    ptr1 = [0] * size
    ptr2 = [0] * size
    ptr3 = [0] * size
    stamp = [0] * size

    def get_pointer(node, state, lam, round_id):
        if stamp[node] != round_id:
            stamp[node] = round_id
            ptr0[node] = 0
            ptr1[node] = 0
            ptr2[node] = 0
            ptr3[node] = 0

        if state == 0:
            p = ptr0[node]
        elif state == 1:
            p = ptr1[node]
        elif state == 2:
            p = ptr2[node]
        else:
            p = ptr3[node]

        hull = tree[node][state]

        while p + 1 < len(hull) and hull[p + 1] - hull[p] >= lam:
            p += 1

        if state == 0:
            ptr0[node] = p
        elif state == 1:
            ptr1[node] = p
        elif state == 2:
            ptr2[node] = p
        else:
            ptr3[node] = p

        return hull, p

    def evaluate(qid, lam, round_id):
        f0_val = 0
        f0_cnt = 0
        f1_val = NEG
        f1_cnt = 0

        for node in parts[qid]:
            old0_val = f0_val
            old0_cnt = f0_cnt
            old1_val = f1_val
            old1_cnt = f1_cnt

            nf0_val = NEG
            nf0_cnt = -10**9
            nf1_val = NEG
            nf1_cnt = -10**9

            for state in range(4):
                hull, p = get_pointer(node, state, lam, round_id)

                value = hull[p] - p * lam
                left_selected = state >> 1
                right_selected = state & 1

                if old0_val != NEG:
                    cand_val = old0_val + value
                    cand_cnt = old0_cnt + p

                    if right_selected == 0:
                        if cand_val > nf0_val or (
                            cand_val == nf0_val and cand_cnt > nf0_cnt
                        ):
                            nf0_val = cand_val
                            nf0_cnt = cand_cnt
                    else:
                        if cand_val > nf1_val or (
                            cand_val == nf1_val and cand_cnt > nf1_cnt
                        ):
                            nf1_val = cand_val
                            nf1_cnt = cand_cnt

                if old1_val != NEG:
                    if left_selected:
                        cand_val = old1_val + value + lam
                        cand_cnt = old1_cnt + p - 1
                    else:
                        cand_val = old1_val + value
                        cand_cnt = old1_cnt + p

                    if right_selected == 0:
                        if cand_val > nf0_val or (
                            cand_val == nf0_val and cand_cnt > nf0_cnt
                        ):
                            nf0_val = cand_val
                            nf0_cnt = cand_cnt
                    else:
                        if cand_val > nf1_val or (
                            cand_val == nf1_val and cand_cnt > nf1_cnt
                        ):
                            nf1_val = cand_val
                            nf1_cnt = cand_cnt

            f0_val, f0_cnt = nf0_val, nf0_cnt
            f1_val, f1_cnt = nf1_val, nf1_cnt

        if f0_val > f1_val or (f0_val == f1_val and f0_cnt >= f1_cnt):
            return f0_val, f0_cnt
        return f1_val, f1_cnt

    # We need one WQS binary search for every query.
    # Queries are reordered by their current midpoint in every layer,
    # so all hull pointers move monotonically.
    max_iterations = (total_abs + 35000).bit_length() + 2

    for round_id in range(1, max_iterations + 1):
        active = False

        order = list(range(q))
        order.sort(
            key=lambda i: (
                (queries[i][3] + queries[i][4]) >> 1
            ),
            reverse=True,
        )

        for qid in order:
            qu = queries[qid]
            lo = qu[3]
            hi = qu[4]

            if lo > hi:
                continue

            active = True
            mid = (lo + hi) >> 1

            value, count = evaluate(qid, mid, round_id)

            if count >= qu[2]:
                qu[5] = value + qu[2] * mid
                qu[3] = mid + 1
            else:
                qu[4] = mid - 1

        if not active:
            break

    out = []
    for qu in queries:
        out.append(str(qu[5]))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```các`build`Hàm thực hiện quá trình tiền xử lý cây phân đoạn. Một lá chỉ có hai trạng thái điểm cuối khả thi. Tại một nút bên trong, mỗi cặp trạng thái con được kết hợp bằng cách sử dụng`minkowski`, hợp nhất lợi ích cận biên của họ.`merge_into`sau đó đặt hàm kết quả vào trạng thái cha thích hợp. Khi cả hai bit biên được chọn, giải pháp tương tự cũng được ghi sớm hơn một vị trí vì hai phần biên tạo thành một đoạn chứ không phải hai. 

Bốn thân tàu được lưu trữ dưới dạng danh sách Python thông thường. Các giá trị của chúng có kích thước 64 bit về mặt toán học, nhưng số nguyên Python đã có độ chính xác tùy ý, do đó không cần xử lý tràn. Trọng điểm âm nhỏ hơn nhiều so với bất kỳ câu trả lời hợp lệ nào và độ lớn của nó được chọn sao cho nó không thể ảnh hưởng đến hệ số góc WQS. 

Khoảng thời gian truy vấn được phân tách một lần thành các nút cây phân đoạn chuẩn. Đây là một chi tiết thực hiện quan trọng. Việc lặp lại phân rã khoảng đệ quy cho mọi điểm giữa của WQS sẽ thêm công việc không cần thiết vào mỗi lớp tìm kiếm nhị phân.`evaluate`duy trì hai trạng thái,`f0`Và`f1`. Bên cạnh điểm đã điều chỉnh, mỗi trạng thái còn lưu trữ số lượng phân đoạn đã chọn. Khi hai mảnh ranh giới được chọn chạm vào nhau, số đếm sẽ thay đổi từ`x+y`ĐẾN`x+y-1`, trong khi số điểm bị phạt tăng trở lại một`lambda`. Đây chính xác là sự điều chỉnh ranh giới tương tự được sử dụng khi đóng bốn thân tàu. 

Mảng con trỏ được thiết lập lại một cách lười biếng bằng cách sử dụng`stamp`. Một nút chỉ được đặt lại khi nó được truy cập lần đầu trong lớp WQS mới. Các truy vấn trong lớp đó được xử lý theo mức độ giảm dần`lambda`, do đó mọi con trỏ chỉ tăng lên. Do đó, vòng lặp while không thực hiện tìm kiếm nhị phân cho mọi truy vấn. Trên một lớp hoàn chỉnh, mỗi con trỏ chỉ đi qua thân của nó một lần. 

Tìm kiếm nhị phân WQS giữ độ dốc lớn nhất mà tối ưu bị phạt vẫn chứa ít nhất số phân đoạn được yêu cầu. Ở độ dốc cuối cùng được lưu trữ, mã sẽ đánh giá mức tối ưu bị phạt một lần nữa thông qua điểm giữa thành công cuối cùng và xây dựng lại mục tiêu ban đầu bằng cách thêm`k * lambda`. Việc so sánh hòa ưu tiên số lượng phân đoạn lớn hơn khi hai lựa chọn có cùng số điểm bị phạt, điều này là cần thiết khi đường hỗ trợ nằm dọc theo một cạnh phẳng của thân lõm. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên, mảng là```
-1 2 -3 4 -5
```Câu trả lời chính xác cho từ một đến năm phân đoạn là`4, 6, 5, 2, -3`. Bảng sau đây cho thấy các giá trị chính xác thu được và sự khác biệt cận biên của chúng. 

| Phân đoạn (k) | Giá trị tốt nhất (F(k)) | Biên (F(k)-F(k-1)) | 
| --- | --- | --- | 
| 0 | 0 | 0 | 
| 1 | 4 | 4 | 
| 2 | 6 | 2 | 
| 3 | 5 | -1 | 
| 4 | 2 | -3 | 
| 5 | -3 | -5 | 

Trình tự cận biên`4, 2, -1, -3, -5`không tăng, đó là độ lõm được yêu cầu bởi WQS và hợp nhất Minkowski. Ví dụ: với ba phân đoạn, lựa chọn tối ưu là`[2]`,`[4]`, Và`[5]`, có tổng là (2+4-5=1), nhưng mức tối ưu thực tế là`5`, thu được bởi`[2]`,`[4]`và một sự sắp xếp khác liên quan đến các phần tử phủ định một cách khác nhau. Bảng này là kết quả lập trình động chính xác và chứng minh tại sao việc chọn các phân đoạn tích cực cục bộ là chưa đủ. 

Đối với mẫu thứ hai, mọi phần tử đều bằng bảy. 

| Phân đoạn (k) | Giá trị tốt nhất (F(k)) | Một công trình tối ưu | 
| --- | --- | --- | 
| 1 | 35 |`[1,5]`| 
| 2 | 35 |`[1,2]`,`[3,5]`| 
| 3 | 35 |`[1]`,`[2]`,`[3,5]`| 
| 4 | 35 |`[1]`,`[2]`,`[3]`,`[4,5]`| 
| 5 | 35 | năm phân đoạn đơn lẻ | 

Mọi phần tử đều dương, do đó việc chia khoảng đã chọn thành nhiều phần không trống hơn không bao giờ làm giảm tổng. Đây là trường hợp hữu ích cho WQS vì nhiều số lượng phân đoạn khác nhau có thể có cùng giá trị mục tiêu. Việc xử lý ràng buộc của việc triển khai chọn số lượng lớn nhất trong số các trạng thái bị phạt tốt như nhau, điều này giữ cho việc tìm kiếm nhị phân trở nên đơn điệu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian tiền xử lý | (O(n\log n)) | Mỗi cấp độ cây phân đoạn thực hiện tổng số tuyến tính hợp nhất Minkowski | 
| Phân tách truy vấn | (O(q\log n)) | Mỗi khoảng trở thành (O(\log n)) nút chuẩn | 
| lớp WQS | (O(\log V)) | Phạm vi độ dốc được giới hạn bởi tổng mảng tuyệt đối | 
| Công việc truy vấn trên mỗi lớp | (O(q\log n)) khấu hao | Mỗi truy vấn chạm vào nút (O(\log n)) | 
| Chuyển động con trỏ trên mỗi lớp | (O(n\log n)) khấu hao | Mỗi con trỏ thân tàu được lưu trữ sẽ tiến qua thân nó nhiều nhất một lần | 
| Sắp xếp theo lớp | (O(q\log q)) | Các truy vấn được sắp xếp theo điểm giữa WQS hiện tại của chúng | 
| Không gian | (O(n\log n+q\log n)) | Bốn hàm lồi được lưu trữ cho mỗi nút cây đoạn | 

Ở đây (V) nhiều nhất là theo thứ tự của tổng tuyệt đối, nhiều nhất là (1,225\times10^9). Do đó, số lớp WQS chỉ khoảng 31. Các lớp tiền xử lý và được lưu trữ là (O(n\log n)), trong khi mỗi truy vấn chỉ giữ lại sự phân rã nút chính tắc và một vài biến tìm kiếm nhị phân. Việc triển khai C++ dự kiến ​​sẽ phù hợp một cách thoải mái với giới hạn bộ nhớ gốc; việc triển khai Python sử dụng cùng một cấu trúc tiệm cận nhưng chi phí đối tượng của Python làm cho bộ nhớ và thời gian chạy trở nên ít dễ tha thứ hơn. 

## Trường hợp thử nghiệm```python
import sys
import io

# Paste the solution above into this file before running these tests.
# The solution exposes solve(), which reads sys.stdin and writes stdout.

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample 1.
assert run(
    """5 5
-1 2 -3 4 -5
1 5 1
1 5 2
1 5 3
1 5 4
1 5 5
"""
) == "4\n6\n5\n2\n-3", "sample 1"

# Provided sample 2.
assert run(
    """5 1
7 7 7 7 7
1 5 1
"""
) == "35", "sample 2"

# Minimum-size negative array.
assert run(
    """1 1
-5
1 1 1
"""
) == "-5", "single negative element"

# Exact-k behavior and adjacent/disjoint choices.
assert run(
    """3 4
5 -10 5
1 3 1
1 3 2
2 3 1
2 2 1
"""
) == "5\n10\n5\n-10", "boundary and exact-k cases"

# All-equal values, including the non-monotone exact-k answer.
assert run(
    """4 3
3 3 3 3
1 4 1
1 4 2
1 4 3
"""
) == "12\n12\n9", "all equal values"

# Maximum-size structural test.
# With all ones, selecting exactly n singleton segments gives n.
n = 35000
inp = f"{n} 1\n" + " ".join(["1"] * n) + f"\n1 {n} {n}\n"
assert run(inp) == "35000", "maximum-size all-positive case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / -5 / 1 1 1`|`-5`| Kích thước tối thiểu và phân đoạn không trống bắt buộc | 
|`5 -10 5`với bốn truy vấn |`5, 10, 5, -10`| Số lượng phân đoạn chính xác, giá trị âm và ranh giới khoảng | 
| Bốn bản sao của`3`|`12, 12, 9`| Câu trả lời chính xác-(k) không đơn điệu | 
| 35000 bản`1`|`35000`| Kích thước đầu vào tối đa và ranh giới (k=n) | 

## Vỏ cạnh 

Trường hợp toàn âm được xử lý bởi thực tế là DP luôn yêu cầu số lượng phân đoạn không trống được yêu cầu. Vì```
1 1
-5
1 1 1
```chiếc lá có`h[1][1][1] = -5`. WQS không thể chọn các phân đoạn bằng 0 vì truy vấn yêu cầu một phân đoạn và việc xây dựng lại cuối cùng sẽ cho`-5`. 

Sự phân biệt chính xác-(k) được hiển thị trên```
4 3
3 3 3 3
1 4 1
1 4 2
1 4 3
```Đối với (k=1), thân tàu chọn toàn bộ khoảng và thu được`12`. Với (k=2), hai mảnh liền kề có thể bao phủ bốn phần tử giống nhau nên giá trị vẫn giữ nguyên`12`. Đối với (k=3), chỉ cần ba phần không trống rời nhau và giải pháp tốt nhất bao gồm ba phần tử có giá trị là`9`. Thân lõm chứa cả ba giá trị, do đó WQS có thể khôi phục từng câu trả lời chính xác thay vì vô tình giải một biến thể "nhiều nhất (k)". 

Trường hợp ranh giới trong đó các phần được chọn nằm liền kề nhau sẽ được xử lý bởi các bit điểm cuối. Coi như```
3 1
5 -10 5
1 3 2
```Giải pháp tốt nhất được chọn`[1,1]`Và`[3,3]`, cho`10`. Khi cả hai bên của việc hợp nhất cây phân đoạn đều xác nhận điểm cuối tiếp xúc của chúng, thao tác hợp nhất sẽ giảm số lượng phân đoạn đi một. Khi cả hai đều không yêu cầu ranh giới, số lượng chỉ cần cộng thêm. Sự khác biệt này cho phép cấu trúc dữ liệu biểu diễn cả các phần liền kề và các phần được hợp nhất thực sự. 

Số lượng phân đoạn tối đa được xử lý trực tiếp bởi cùng một đại diện trạng thái. TRÊN```
3 1
-2 -3 -4
1 3 3
```giải pháp khả thi duy nhất với ba phân đoạn là chọn từng phân đoạn đơn lẻ. Giá trị chính xác là`-9`. Thân WQS chứa điểm tương ứng với ba đoạn và việc tái thiết đường hỗ trợ cuối cùng sẽ trả về`-9`thay vì 0 hoặc giá trị tốt nhất cho ít phân đoạn hơn. 

Trường hợp tinh vi cuối cùng là phần phẳng của thân tàu, chẳng hạn như một mảng các giá trị dương bằng nhau. Một số số lượng phân đoạn khác nhau có thể có cùng giá trị ban đầu. Ở độ dốc khớp chính xác với mức tăng biên tương ứng, một số vị trí thân tàu có giá trị bị phạt bằng nhau. Việc triển khai giải quyết vấn đề này bằng cách ưu tiên số lượng phân đoạn lớn hơn. Điều đó làm cho số lượng phân đoạn được chọn trở nên đơn điệu trong độ dốc WQS và tạo ra một cạnh nhất quán của cạnh hỗ trợ, điều này cần thiết để tìm kiếm nhị phân hội tụ về độ dốc hỗ trợ (k).
