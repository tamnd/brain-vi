---
title: "CF 102331J - Jiry Matchings"
description: "Chúng ta có một cây có trọng số với (n) đỉnh. Sự so khớp là một tập hợp các cạnh sao cho không có hai cạnh được chọn nào có chung điểm cuối. Với mọi (k=1,2,ldots,n-1), chúng ta cần tổng trọng số cạnh tối đa có thể có trong số tất cả các kết quả khớp có chứa chính xác (k) cạnh."
date: "2026-08-13T03:51:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102331
codeforces_index: "J"
codeforces_contest_name: "2019 Summer Petrozavodsk Camp, Day 2: 300iq Contest 2 (XX Open Cup, Grand Prix of Kazan)"
rating: 0
weight: 102331
solve_time_s: 450
verified: true
draft: false
---

[CF 102331J - Kết hợp Jiry](https://codeforces.com/problemset/problem/102331/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 7 phút 30 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có trọng số với (n) đỉnh. Sự so khớp là một tập hợp các cạnh sao cho không có hai cạnh được chọn nào có chung điểm cuối. Với mọi (k=1,2,\ldots,n-1), chúng ta cần tổng trọng số cạnh tối đa có thể có trong số tất cả các kết quả khớp có chứa chính xác (k) cạnh. Nếu không thể tồn tại sự trùng khớp về kích thước (k), chúng ta sẽ in`?`. 

Đầu vào là một cây nên nó chứa chính xác (n-1) cạnh. Các trọng số có thể âm, có nghĩa là sự phù hợp hợp lệ về kích thước (k) có thể có giá trị tối ưu âm. Chúng ta không được phép thay thế câu trả lời phủ định bằng 0, vì số cạnh được chọn là cố định. 

Với (n\le 200000), một chiếc ba lô hình cây thông thường là quá đắt. Nếu một đỉnh có một cây con có kích thước (các) thì việc lưu trữ câu trả lời cho mọi kích thước phù hợp có thể có sẽ có chi phí (O(s)) và việc kết hợp trực tiếp hai mảng như vậy có chi phí (O(s_1s_2)). Trên một đường dẫn, việc liên tục hợp nhất các mảng đang phát triển sẽ mất (O(n^2)), vượt xa giới hạn sáu giây có thể hỗ trợ. Giải pháp phải khai thác được hình dạng đặc biệt của các mảng DP này. 

Có bốn trường hợp cạnh đặc biệt dễ xử lý sai. 

Đầu tiên, trọng số âm vẫn là câu trả lời hợp lệ. Đối với đầu vào```
2
1 2 -7
```kết quả khớp duy nhất có thể có một cạnh, vì vậy kết quả đầu ra đúng là```
-7
```Việc triển khai khởi tạo các câu trả lời bằng 0 và lấy giá trị tối đa bằng 0 sẽ in không chính xác`0`. 

Thứ hai, việc kết hợp có thể tồn tại đối với một số kích thước và sau đó trở nên không thể thực hiện được. Vì```
3
1 2 -5
2 3 -2
```cách khớp một cạnh tốt nhất sử dụng cạnh (2\text{-}3), mang lại`-2`, trong khi sự so khớp hai cạnh không tồn tại. Câu trả lời là```
-2 ?
```Việc triển khai bất cẩn chỉ kiểm tra một vài mục DP đầu tiên có thể vô tình hiểu trạng thái không thể truy cập được là giá trị thực. 

Thứ ba, kích thước khớp tối đa không phải là (n-1). Trong một đường đi trên sáu đỉnh, có thể chọn tối đa ba cạnh. Vì```
6
1 2 3
2 3 3
3 4 3
4 5 3
5 6 3
```câu trả lời là```
3 6 9 ? ?
```DP phải duy trì số lượng bản số không thể truy cập thay vì cho rằng mọi kích thước lên tới (n-1) đều khả thi. 

Thứ tư, trọng số bằng nhau có thể tạo ra nhiều lựa chọn tối ưu. Vì một ngôi sao```
5
1 2 4
1 3 4
1 4 4
1 5 4
```chỉ có thể chọn một cạnh, vì vậy câu trả lời là```
4 ? ? ?
```Một quy tắc tham lam chỉ sắp xếp các cạnh theo trọng số không thể giải quyết được vấn đề, bởi vì vấn đề quan trọng là sự tương tác giữa các cạnh chia sẻ đỉnh. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp là cây DP. Gốc cây tại đỉnh (1). Đối với mỗi đỉnh (u), giữ hai mảng mô tả các kết quả khớp bên trong cây con của nó. 

Đặt (f_u^0[k]) là trọng số tối đa của một kết quả khớp với (k) cạnh trong cây con của (u), trong đó (u) không khớp với bất kỳ cạnh con nào của nó. Đặt (f_u^1[k]) là giá trị tương ứng khi (u) được khớp với chính xác một con. Đối với một chiếc lá, (f_u^0=[0]) và (f_u^1) trống. 

Giả sử (v) là con của (u), được nối bởi một cạnh có trọng số (w). Nếu chúng ta không sử dụng (uv), phần đóng góp của (v) là (\max(f_v^0,f_v^1)). Nếu chúng ta sử dụng (uv), thì bản thân (v) không thể khớp với một đứa trẻ khác, do đó đóng góp của nó là (f_v^0), được dịch chuyển bởi một cạnh phù hợp và tăng thêm (w). 

Vấn đề còn lại là kết hợp các kích thước lực lượng. Nếu hai mảng DP (a) và (b) mô tả các cây con độc lập thì quá trình chuyển đổi dạng cây thông thường là 

[ 
c[k]=\max_{i+j=k}(a[i]+b[j]). 
] 

Đối với mảng tùy ý, đây là bậc hai. Ở đây các mảng có một tính chất lõm quan trọng. Lợi ích cận biên 

[ 
a[i]-a[i-1] 
] 

đều không tăng. Điều này xuất phát từ cách giải thích luồng chi phí tối thiểu hoặc chi phí tối đa tiêu chuẩn của việc so khớp bị ràng buộc về số lượng. Trong biểu đồ hai bên, việc gửi thêm một đơn vị luồng phù hợp sẽ có chi phí cận biên không tăng khi chúng tôi tối đa hóa trọng số. Cây là cây lưỡng tính, do đó, cùng một thuộc tính áp dụng cho mọi hồ sơ DP của cây con. Quan sát tương tự được sử dụng trong giải pháp tiêu chuẩn của vấn đề này. 

Đối với hai mảng lõm như vậy, tích chập cộng tối đa sẽ trở thành sự hợp nhất của lợi ích cận biên của chúng. Nếu lợi ích cận biên của (a) là 

[ 
a[1]-a[0],a[2]-a[1],\ldots 
] 

và lợi ích cận biên của (b) được xác định tương tự, chúng ta sắp xếp hai chuỗi đã được sắp xếp này lại với nhau. Tổng tiền tố của chuỗi được hợp nhất chính xác là tích chập. Do đó, hai biên dạng có độ dài (A) và (B) có thể được kết hợp thành (O(A+B)) thay vì (O(AB)). Đây là quan điểm Minkowski-sum được sử dụng bởi lời giải chính thức. 

Điều đó giải quyết được vấn đề tích chập tốn kém, nhưng một đường dẫn vẫn gây ra vấn đề. Nếu chúng ta kết hợp từng đỉnh một, các mảng có kích thước (1,2,3,\ldots,n), tạo ra (O(n^2)) hoạt động. Quan sát khắc phục điều này là sự phân hủy ánh sáng nặng. 

Với mỗi đỉnh, chọn cây con lớn nhất của nó làm cây con nặng. Những đứa trẻ khác thì nhẹ nhàng. Trước tiên, chúng tôi kết hợp tất cả các phần tử ánh sáng con của một đỉnh bằng cách sử dụng phép chia và chinh phục, do đó, mọi phần đóng góp ánh sáng con chỉ tham gia vào việc hợp nhất (O(\log n)). Sau đó, chúng tôi xử lý toàn bộ chuỗi nặng cùng một lúc, lại sử dụng phương pháp chia để trị. Mỗi sự phân chia chuỗi được cân bằng tùy theo lượng thông tin cây con không nặng được gắn vào các đỉnh của nó. Điều này đưa ra giới hạn bắt buộc (O(n\log^2 n)). 

Các phương pháp tiếp cận bạo lực và tối ưu hóa có thể được tóm tắt như sau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Ba lô cây trực tiếp | (O(n^2)) | (O(n^2)) trong trường hợp xấu nhất | Quá chậm | 
| Tích chập lõm không có HLD | (O(n^2)) trên một đường dẫn | (O(n)) đến (O(n^2)) tùy thuộc vào bộ nhớ | Vẫn còn quá chậm | 
| HLD + tổng Minkowski chia để trị | (O(n\log^2 n)) | (O(n\log n)) tạm thời, lưu trữ đồ thị (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lấy gốc cây tại đỉnh (1), tính toán mọi kích thước cây con và chọn cây con có kích thước cây con lớn nhất ở mỗi đỉnh là cây con nặng của nó. Lưu trữ trọng lượng của mọi cạnh cha mẹ con với đứa trẻ. 

Cây con nặng giữ cây con lớn nhất còn lại gắn liền với đỉnh hiện tại. Mỗi khi chúng ta di chuyển qua một cạnh ánh sáng, kích thước cây con ít nhất sẽ tăng gấp đôi, do đó một đỉnh chỉ có thể nằm bên dưới (O(\log n)) cạnh ánh sáng. 

1. Với mỗi đỉnh (u), trước tiên hãy giải tất cả các cây con nhẹ. Đối với trẻ nhẹ (v), chúng tôi đã có hai cấu hình (f_v^0) và (f_v^1). 

Nếu (u) không khớp với (uv), phần đóng góp là 

[ 
g_v^0=\max(f_v^0,f_v^1). 
]

Nếu (u) khớp với (v), thì (v) không thể khớp đồng thời với một trong các con của nó. Sự đóng góp là 

[ 
g_v^1[k+1]=f_v^0[k]+w(u,v). 
] 

Đối với tất cả các ánh sáng con, chúng ta cần một hồ sơ cho trường hợp (u) không khớp với các con của nó và một hồ sơ khác cho trường hợp có chính xác một ánh sáng con khớp với (u). 

1. Kết hợp các em nhẹ nhàng bằng cách sử dụng phép chia để trị. Tại một lá của cây phân chia để chinh phục này, cặp hồ sơ chỉ đơn giản là 

[ 
(g_v^0,g_v^1). 
] 

Khi hai nhóm được nối với nhau, cấu hình "số 0" mới là tích chập Minkowski của cấu hình số 0 của chúng. Cấu hình "một" mới là mức tối đa trong hai khả năng trong đó cạnh duy nhất tới đỉnh hiện tại đến từ nhóm bên trái hoặc từ nhóm bên phải. 

Bởi vì tất cả các cấu hình đều lõm nên mỗi tích chập là tuyến tính theo chiều dài của hai mảng. Chia để trị ngăn chặn một chuỗi các vụ sáp nhập ngày càng tốn kém. 

1. Bây giờ hãy xem xét một chuỗi nặng (v_1,v_2,\ldots,v_m), được sắp xếp từ trên xuống dưới. Đối với mỗi đỉnh, chúng ta đã biết hai mặt cắt được tạo ra bởi tất cả các đỉnh con của nó. 

Một đoạn chuỗi cần có bốn cấu hình. Hai trạng thái nhị phân mô tả xem điểm cuối trên và điểm cuối dưới cùng của nó có khớp với nhau trong phân đoạn hay không. Đối với một đỉnh duy nhất, chỉ có thể có trạng thái (00) và trạng thái (11), bởi vì cùng một đỉnh đồng thời là cả hai điểm cuối. 

1. Chia đoạn xích nặng thành hai phần cân đối. Kết hợp bốn trạng thái của các đoạn bên trái và bên phải mà không chọn cạnh kết nối của chúng bằng cách lấy tất cả các phép chập Minkowski tương thích. 

Điểm cuối bên ngoài vẫn là điểm cuối của hai phân đoạn ban đầu. Đây là trường hợp nối thông thường. 

1. Xem xét riêng việc chọn cạnh nối hai nửa. Một cạnh như vậy chỉ hợp pháp khi điểm cuối bên phải của nửa bên trái và điểm cuối bên trái của nửa bên phải hiện không khớp với nhau bên trong các nửa tương ứng của chúng. 

Trong trường hợp đó, hãy thêm trọng số của cạnh và dịch chuyển cấu hình kết quả theo một vị trí vì một cạnh phù hợp bổ sung đã được chọn. Cả hai điểm cuối bên trong đều bị chiếm dụng, do đó trạng thái biên tương ứng thay đổi từ (0) thành (1). 

1. Lặp lại quá trình phân chia và chinh phục chuỗi cho đến khi chuỗi nặng hoàn chỉnh được hợp nhất. Hai cấu hình thu được cho đỉnh trên cùng của nó là các cấu hình bắt buộc cho cây con đó. 
2. Xử lý từng dây chuyền nặng từ dưới lên. Khi một chuỗi kết thúc, chỉ có đỉnh trên cùng của nó cần được giữ lại cho chuỗi cha của nó. Tất cả các cấu hình bên trong có thể được giải phóng, giúp kiểm soát bộ nhớ làm việc. 
3. Sau khi xử lý chuỗi chứa gốc, lấy 

[ 
F[k]=\max(f_1^0[k],f_1^1[k]). 
] 

Với mọi (k=1,\ldots,n-1), in (F[k]) nếu có thể truy cập được và`?`nếu không thì. 

### Tại sao nó hoạt động 

Điều bất biến là mọi cấu hình được lưu trữ cho một đỉnh hoặc đoạn chuỗi thể hiện chính xác tất cả các kết quả khớp bên trong vùng đó, được phân loại theo liệu các đỉnh ranh giới có liên quan đã được khớp trong vùng hay chưa. Quá trình chuyển đổi ánh sáng con xem xét hai khả năng duy nhất cho cạnh kết nối con với cha mẹ của nó, trong khi quá trình chuyển đổi chuỗi xem xét hai khả năng duy nhất cho cạnh giữa hai đoạn chuỗi, được chọn hoặc không được chọn. Do đó, mọi kết hợp pháp lý được đại diện bởi một số trạng thái DP và mọi trạng thái được xây dựng bởi các chuyển đổi đều tương ứng với một kết hợp pháp lý. 

Tối ưu hóa số không làm thay đổi những chuyển đổi này. Nó chỉ tính toán các tích chập tối đa cộng thêm của chúng nhanh hơn. Vì cấu hình số lượng là lõm nên mức tăng cận biên của chúng đã được sắp xếp, do đó, việc hợp nhất mức tăng cận biên sẽ mang lại kết quả tích chập giống hệt như định nghĩa bậc hai. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(1_000_000)

NEG = -10**30

def solve():
    n = int(input())

    graph = [[] for _ in range(n + 1)]
    for _ in range(n - 1):
        u, v, w = map(int, input().split())
        graph[u].append((v, w))
        graph[v].append((u, w))

    parent = [0] * (n + 1)
    value = [0] * (n + 1)
    size = [1] * (n + 1)
    heavy = [0] * (n + 1)

    order = [1]
    parent[1] = -1

    for u in order:
        for v, w in graph[u]:
            if v == parent[u]:
                continue
            parent[v] = u
            value[v] = w
            order.append(v)

    for u in reversed(order):
        size[u] = 1
        best = 0
        best_size = 0
        for v, _ in graph[u]:
            if parent[v] == u:
                size[u] += size[v]
                if size[v] > best_size:
                    best_size = size[v]
                    best = v
        heavy[u] = best

    f0 = [None] * (n + 1)
    f1 = [None] * (n + 1)

    def max_merge(a, b):
        if not a:
            return b[:]
        if not b:
            return a[:]

        m = max(len(a), len(b))
        c = [NEG] * m

        la = len(a)
        lb = len(b)

        for i in range(m):
            x = a[i] if i < la else NEG
            y = b[i] if i < lb else NEG
            c[i] = x if x >= y else y

        return c

    def minkowski(a, b):
        if not a or not b:
            return []

        la = len(a)
        lb = len(b)

        c = [0] * (la + lb - 1)
        c[0] = a[0] + b[0]

        da = [0] * (la - 1)
        db = [0] * (lb - 1)

        for i in range(1, la):
            da[i - 1] = a[i] - a[i - 1]

        for i in range(1, lb):
            db[i - 1] = b[i] - b[i - 1]

        i = 0
        j = 0
        p = 1
        total = len(c)

        while p < total:
            if j == len(db) or (i < len(da) and da[i] > db[j]):
                c[p] = da[i]
                i += 1
            else:
                c[p] = db[j]
                j += 1
            p += 1

        for i in range(1, total):
            c[i] += c[i - 1]

        return c

    def solve_light(ids, l, r):
        if l == r:
            u = ids[l]

            g = f0[u][:]
            if g:
                for i in range(len(g)):
                    g[i] += value[u]
                g.insert(0, NEG)

            return (
                max_merge(f0[u], f1[u]),
                g
            )

        mid = (l + r) >> 1

        left0, left1 = solve_light(ids, l, mid)
        right0, right1 = solve_light(ids, mid + 1, r)

        zero = minkowski(left0, right0)

        one_left = minkowski(left1, right0)
        one_right = minkowski(left0, right1)
        one = max_merge(one_left, one_right)

        return zero, one

    def solve_chain(ids, l, r):
        if l == r:
            u = ids[l]

            return (
                [[f0[u][:], []],
                 [[], f1[u][:]]]
            )

        total_light = 0
        for i in range(l, r + 1):
            u = ids[i]
            total_light += size[u] - size[heavy[u]]

        mid = l
        used = 0

        while mid < r and used < total_light // 2:
            u = ids[mid]
            used += size[u] - size[heavy[u]]
            mid += 1

        left = solve_chain(ids, l, mid - 1)
        right = solve_chain(ids, mid, r)

        res = [
            [[], []],
            [[], []]
        ]

        for a in range(2):
            for b in range(2):
                for c in range(2):
                    for d in range(2):
                        x = minkowski(left[a][b], right[c][d])
                        res[a][d] = max_merge(res[a][d], x)

        for a in range(2):
            for d in range(2):
                x = minkowski(left[a][0], right[0][d])

                if not x:
                    continue

                for i in range(len(x)):
                    x[i] += value[ids[mid]]

                x.insert(0, NEG)

                na = 1 if (l == mid - 1) else a
                nd = 1 if (mid == r) else d

                res[na][nd] = max_merge(res[na][nd], x)

        return res

    def process_chain(top):
        chain = []
        u = top

        while u:
            chain.append(u)
            u = heavy[u]

        for u in chain:
            light = []

            for v, _ in graph[u]:
                if parent[v] == u and v != heavy[u]:
                    process_chain(v)
                    light.append(v)

            if not light:
                f0[u] = [0]
                f1[u] = []
            else:
                a, b = solve_light(light, 0, len(light) - 1)
                f0[u] = a
                f1[u] = b

                for v in light:
                    f0[v] = None
                    f1[v] = None

        res = solve_chain(chain, 0, len(chain) - 1)

        f0[top] = max_merge(res[0][0], res[0][1])
        f1[top] = max_merge(res[1][0], res[1][1])

        for u in chain[1:]:
            f0[u] = None
            f1[u] = None

    process_chain(1)

    answer = []
    root0 = f0[1]
    root1 = f1[1]

    for k in range(1, n):
        best = NEG

        if k < len(root0):
            best = max(best, root0[k])

        if k < len(root1):
            best = max(best, root1[k])

        if best <= NEG // 2:
            answer.append("?")
        else:
            answer.append(str(best))

    print(" ".join(answer))

if __name__ == "__main__":
    solve()
```Quá trình tiền xử lý đầu tiên là lặp lại chứ không phải đệ quy. Điều này tránh làm cho ngăn xếp cuộc gọi Python phụ thuộc vào chiều cao của cây ban đầu, có thể là (200000) trên một đường dẫn. Việc duyệt ngược sau đó sẽ tính toán kích thước cây con và chọn cây con lớn nhất làm cây con nặng. 

các`minkowski`chức năng là tối ưu hóa số trung tâm. Nó tính tích chập cộng cực đại bằng cách hình thành các sai phân liên tiếp và hợp nhất các sai phân đó theo thứ tự giảm dần. Cấu hình đầu vào là lõm nên những khác biệt đó đã được sắp xếp sẵn. Tổng tiền tố sẽ xây dựng lại hồ sơ kết quả. 

Dẫn đầu`NEG`giá trị trong cấu hình đã thay đổi thể hiện kích thước không thể khớp. Nó cố tình nhỏ hơn nhiều so với mọi câu trả lời thực sự. Tổng trọng lượng tuyệt đối lớn nhất có thể là dưới (2\cdot10^{14}), vì vậy`-10**30`để lại một giới hạn an toàn rất lớn và số nguyên Python không có vấn đề tràn.`solve_light`thực hiện hợp nhất phân chia và chinh phục cho tất cả trẻ em ánh sáng. Biên dạng thứ hai của nó tương ứng với việc chọn chính xác một cạnh tới đỉnh hiện tại. Sự dịch chuyển một vị trí chiếm cạnh được chọn đó.`solve_chain`là DP phân chia và chinh phục bốn trạng thái cho một chuỗi nặng. Trạng thái của nó`res[a][b]`ghi lại trạng thái của hai điểm cuối tiếp xúc. Vòng lặp kết hợp đầu tiên xử lý cạnh biên không được chọn. Vòng lặp thứ hai xử lý việc chọn cạnh đó, yêu cầu cả hai trạng thái điểm cuối liền kề bằng 0. 

Sự phân chia được tính trọng số bởi`size[u] - size[heavy[u]]`, lượng thông tin không tiếp tục đi qua phần nặng. Đây là chi tiết mang lại cho đệ quy chuỗi độ phức tạp cân bằng thay vì chỉ phân chia theo số đỉnh. 

Câu trả lời cuối cùng lấy tối đa hai trạng thái gốc vì gốc không có cha, do đó không có hạn chế bên ngoài nào về việc liệu nó có khớp với một trong các con của nó hay không. 

Việc triển khai tuân theo cấu trúc HLD (O(n\log^2 n)) và tích chập lõm được mô tả trong các giải pháp tiêu chuẩn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Cái cây là```
1
|
2
/ \
3  4
|
5
```với trọng số (3,5,4,2). Đường dẫn nặng được chọn theo kích thước cây con là (1\to2\to3\to5), trong khi (4) là con nhẹ của (2). 

Các hồ sơ địa phương có liên quan phát triển như sau. 

| Sân khấu | Đỉnh hoặc đoạn | (f^0) | (f^1) | 
| --- | --- | --- | --- | 
| 1 | Lá 4 |`[0]`|`[]`| 
| 2 | Vertex 2, trước con nặng |`[0]`|`[-inf, 4]`| 
| 3 | Đoạn 3-5 |`[0]`|`[-inf, 2]`| 
| 4 | Cây con của 2 |`[0, 2]`|`[-inf, 5, 6]`| 
| 5 | Toàn cây |`[0, 5, 6]`|`[-inf, 3, 5]`| 

Ở gốc, hồ sơ tốt nhất là`[0,5,6]`. Mục nhập cho một cạnh là (5), lấy từ cạnh (2\text{-}3). Mục nhập cho hai cạnh là (6), thu được từ các cạnh (2\text{-}4) và (3\text{-}5). Số lượng lớn hơn không thể truy cập được, vì vậy đầu ra là`5 6 ? ?`. 

Dấu vết này cũng cho thấy tại sao hai trạng thái này lại cần thiết. Tại đỉnh (2), việc chọn (2\text{-}4) sẽ ngăn việc chọn (2\text{-}3), nhưng không ngăn cản việc chọn (3\text{-}5). 

### Mẫu 2 

Đối với cây mười đỉnh, cấu hình gốc cuối cùng chứa các giá trị có thể truy cập sau. 

| Kích thước phù hợp (k) | Giá trị tốt nhất | 
| --- | --- | 
| 1 | 5 | 
| 2 | 10 | 
| 3 | 15 | 
| 4 | 10 | 
| 5 | không thể truy cập | 
| 6 | không thể truy cập | 
| 7 | không thể truy cập | 
| 8 | không thể truy cập | 
| 9 | không thể truy cập | 

Phần quan trọng của dấu vết là giá trị không phải giảm đơn điệu với (k). Ba kích thước phù hợp đầu tiên có thể sử dụng một số cạnh dương, trong khi buộc cạnh thứ tư có thể yêu cầu thay thế cấu hình tốt hơn nhiều bằng cấu hình có trọng lượng thấp hơn. DP đang tối ưu hóa từng lượng số một cách độc lập, vì vậy`10`for (k=4) hoàn toàn hợp lệ mặc dù nó nhỏ hơn câu trả lời cho (k=3). 

Đầu ra cuối cùng là```
5 10 15 10 ? ? ? ? ?
```## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n\log^2 n)) | Các tích chập lõm có kích thước tuyến tính, trong khi sự hợp nhất chuỗi con nhẹ và chuỗi nặng được cân bằng bằng cách phân chia và chinh phục | 
| Không gian | (O(n\log n)) tạm thời, lưu trữ đồ thị (O(n)) | Các vectơ DP được giải phóng sau khi cây con hoặc chuỗi của chúng được tiêu thụ | 

Giới hạn thời gian (O(n\log^2 n)) là lý do chính khiến phương thức xử lý (n=200000). Một cạnh nhẹ chỉ có thể được duyệt qua (O(\log n)) lần vì kích thước cây con ít nhất tăng gấp đôi sau mỗi lần chuyển đổi như vậy và phép tích phân chia để trị sẽ thêm một hệ số logarit khác. Đây là độ phức tạp tiệm cận dự kiến ​​của giải pháp HLD. 

Trọng số cạnh có thể lớn bằng (10^9) và có thể có (O(n)) cạnh được chọn, do đó, số học 64 bit là bắt buộc trong C++. Số nguyên Python tự nhiên cung cấp phạm vi cần thiết. 

## Trường hợp thử nghiệm 

Khai thác sau đây giả định giải pháp được gửi đã được lưu dưới dạng`solution.py`. Nó sử dụng một quy trình con để kiểm tra giao diện stdin/stdout chính xác của chương trình lập trình cạnh tranh.```python
import subprocess

def run(inp: str) -> str:
    result = subprocess.run(
        ["python3", "solution.py"],
        input=inp,
        text=True,
        capture_output=True,
        check=True,
    )
    return result.stdout.strip()

# Provided sample 1
assert run("""\
5
1 2 3
2 3 5
2 4 4
3 5 2
""") == "5 6 ? ?", "sample 1"

# Provided sample 2
assert run("""\
10
2 8 -5
5 10 5
3 4 -5
1 6 5
3 9 5
1 7 -3
4 8 -5
10 8 -5
1 8 -3
""") == "5 10 15 10 ? ? ? ? ?", "sample 2"

# Provided sample 3
assert run("""\
2
1 2 35
""") == "35", "sample 3"

# Minimum size with a negative edge
assert run("""\
2
1 2 -7
""") == "-7", "negative answer must remain negative"

# Three-vertex path, one matching is possible but two are not
assert run("""\
3
1 2 -5
2 3 -2
""") == "-2 ?", "unreachable cardinality"

# Star, all edges equal
assert run("""\
5
1 2 4
1 3 4
1 4 4
1 5 4
""") == "4 ? ? ?", "star matching size"

# Path with equal positive weights
assert run("""\
6
1 2 3
2 3 3
3 4 3
4 5 3
5 6 3
""") == "3 6 9 ? ?", "maximum matching size"

# Maximum-size stress shape.
# A star on 200000 vertices has maximum matching size 1.
n = 200000
parts = [str(n)]
parts.extend(f"1 {v} 1" for v in range(2, n + 1))
maximum_input = "\n".join(parts) + "\n"
maximum_expected = "1 " + " ".join("?" for _ in range(n - 2))
assert run(maximum_input) == maximum_expected, "maximum-size star"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 / 1 2 -7`|`-7`| Giá trị tối ưu âm | 
|`3 / 1 2 -5 / 2 3 -2`|`-2 ?`| Kích thước phù hợp lớn hơn không thể | 
| Bốn cạnh bằng nhau tập trung vào một tâm |`4 ? ? ?`| Kết hợp các xung đột ở đỉnh bậc cao | 
| Đường dẫn sáu đỉnh với tất cả các trọng số`3`|`3 6 9 ? ?`| Kích thước khớp tối đa chính xác và lợi nhuận biên lặp lại | 
| Ngôi sao hai trăm nghìn đỉnh |`1 ? ? ... ?`| Kích thước đầu vào tối đa và mức độ cực cao | 

## Vỏ cạnh 

Một cạnh âm duy nhất được xử lý vì cấu hình cơ sở chỉ chứa số 0 để chọn không có cạnh nào, trong khi câu trả lời được yêu cầu bắt đầu tại (k=1). Vì```
2
1 2 -7
```cấu hình đã thay đổi cho cạnh duy nhất là`[-inf, -7]`. Do đó, gốc tối đa cuối cùng cho`-7`, không phải bằng không. 

Đường đi gồm ba đỉnh thể hiện trạng thái không thể truy cập được:```
3
1 2 -5
2 3 -2
```Hai cạnh có chung đỉnh (2), do đó DP có thể tạo trạng thái kích thước một hợp lệ với giá trị`-2`, nhưng không có trạng thái cỡ hai hợp lệ. Vị trí mảng tương ứng vẫn giữ nguyên`NEG`và logic đầu ra chuyển đổi nó thành`?`. 

Một ngôi sao chứng minh tại sao việc chọn cạnh nặng nhất cục bộ là không đủ:```
5
1 2 4
1 3 4
1 4 4
1 5 4
```Tất cả bốn cạnh đều có cùng trọng số nhưng chúng đều xung đột ở đỉnh (1). Trạng thái mà tâm đã được khớp sẽ ngăn không cho mọi cạnh sự cố khác được chọn. Cấu hình kết quả chỉ chứa các kích thước bằng 0 và một. 

Đường sáu đỉnh```
6
1 2 3
2 3 3
3 4 3
4 5 3
5 6 3
```có kích thước phù hợp tối đa là ba. Ba cạnh (1\text{-}2), (3\text{-}4) và (5\text{-}6) tạo ra trọng số (9). Số lượng bản số tiếp theo là không thể, vì vậy câu trả lời kết thúc sau giá trị thứ ba. Điều này kiểm tra ranh giới giữa các mục DP có thể truy cập và không thể truy cập. 

Cuối cùng, ngôi sao có kích thước tối đa có (200000) đỉnh và (199999) cạnh. Mọi cạnh đều có chung tâm nên chỉ một cạnh có thể thuộc về một cạnh phù hợp. Thuật toán vẫn phải xử lý tất cả các đỉnh, nhưng cấu trúc nặng-nhẹ chọn một lá là con nặng và coi các lá còn lại là con nhẹ. Câu trả lời kết quả là`1`theo sau là`199998`dấu hỏi. Trường hợp này thực hiện cả việc xử lý đỉnh ở mức độ cao và yêu cầu xử lý kích thước đầu vào đầy đủ.
