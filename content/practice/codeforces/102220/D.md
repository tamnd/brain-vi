---
title: "CF 102220D - Thạc sĩ cấu trúc dữ liệu"
description: "Chúng ta có một cây có các đỉnh bắt đầu bằng giá trị 0. Mỗi sự kiện sẽ thay đổi các giá trị trên mỗi đỉnh của một đường dẫn đơn giản hoặc yêu cầu tổng hợp trên đường dẫn đó."
date: "2026-08-17T22:32:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102220
codeforces_index: "D"
codeforces_contest_name: "The 13th Chinese Northeast Collegiate Programming Contest"
rating: 0
weight: 102220
solve_time_s: 360
verified: true
draft: false
---

[CF 102220D - Bậc thầy về cấu trúc dữ liệu](https://codeforces.com/problemset/problem/102220/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 6 phút 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có các đỉnh bắt đầu bằng giá trị 0. Mỗi sự kiện sẽ thay đổi các giá trị trên mỗi đỉnh của một đường dẫn đơn giản hoặc yêu cầu tổng hợp trên đường dẫn đó. Các bản cập nhật được trộn lẫn một cách có chủ ý: phép cộng là số học thông thường, XOR là bitwise và phép trừ là có điều kiện vì giá trị nhỏ hơn số lượng phép trừ phải không thay đổi. Các truy vấn yêu cầu tổng đường dẫn, XOR đường dẫn, chênh lệch giữa giá trị tối đa và tối thiểu hoặc giá trị gần nhất trên đường dẫn đến một số nguyên nhất định. 

Đầu vào chứa tối đa năm trường hợp thử nghiệm. Một cây có thể chứa 500.000 đỉnh, trong khi chỉ có 2.000 sự kiện. Sự bất đối xứng đó là hạn chế chính. Cây quá lớn để quét mọi sự kiện. Việc triển khai trực tiếp có thể chạm tới 500.000 đỉnh cho mỗi sự kiện, mang lại khoảng 1.000.000.000 thao tác đỉnh trong trường hợp xấu nhất. 2.000 tuy nhỏ nhưng cũng không đủ nhỏ để bù đắp cho hệ số 500.000. Mặt khác, 2.000 sự kiện đủ nhỏ để một cấu trúc chỉ chứa vài nghìn đỉnh cây được lựa chọn cẩn thận có thể được xử lý gần như trực tiếp. 

Có một số trường hợp ranh giới dễ bỏ sót. Đầu tiên là một phép trừ không làm gì cả. Ví dụ,```
1
1 2
1 1 1 5
3 1 1 7
4 1 1
```Giá trị trở thành 5 sau thao tác đầu tiên và phép trừ cho 7 bị bỏ qua vì 5 nhỏ hơn 7. Kết quả đầu ra đúng là`5`. Việc thực hiện bất cẩn bằng cách sử dụng`max(0, w-k)`sẽ vô tình tạo ra số 0, đây không phải là thao tác được chỉ định. 

Trường hợp thứ hai là XOR trên một số chẵn các giá trị bằng nhau. Xét cây hai đỉnh:```
1
2 2
1 2
1 1 2 5
5 1 2
```Cả hai đỉnh trở thành 5, nhưng`5 XOR 5 = 0`, vì vậy đầu ra đúng là`0`. Khi một cạnh của cây được nén đại diện cho một số đỉnh ban đầu, sự đóng góp của XOR chỉ phụ thuộc vào việc bội số đó là lẻ hay chẵn. 

Trường hợp thứ ba liên quan đến các cạnh bị nén. Trên một chuỗi có năm đỉnh,```
1
5 2
1 2
2 3
3 4
4 5
1 1 5 3
4 1 5
```cả năm đỉnh đều nhận được 3, vì vậy câu trả lời là`15`. Cây ảo chỉ chứa đỉnh 1 và 5 sẽ có một cạnh, nhưng cạnh đó cũng đại diện cho ba đỉnh bên trong. Bỏ qua các đỉnh bị bỏ qua sẽ trả về sai 6 thay vì 15. 

## Phương pháp tiếp cận 

Giải pháp đơn giản lưu trữ giá trị hiện tại của mỗi đỉnh cây. Để cập nhật, hãy tìm đường dẫn giữa hai điểm cuối của nó và truy cập mọi đỉnh trên đường dẫn đó. Đối với một truy vấn, hãy truy cập vào cùng một đường dẫn và tính tổng được yêu cầu. Điều này đúng vì mọi sự kiện đều được xác định trực tiếp theo các đỉnh trên một đường đi đơn giản. 

Vấn đề là trường hợp xấu nhất. Một đường dẫn có thể chứa tất cả 500.000 đỉnh và có thể có 2.000 sự kiện. Do đó, việc triển khai bạo lực có thể thực hiện khoảng 

[ 
500000 \times 2000 = 10^9 
] 

thăm đỉnh. Giá trị lớn của`n`loại trừ điều này. 

Quan sát hữu ích là tập hợp các điểm cuối rất nhỏ. Trong tất cả các sự kiện chỉ có`2m`đỉnh điểm cuối, nhiều nhất là 4.000 khi`m = 2000`. Chúng ta có thể xây dựng một cây ảo chứa mọi điểm cuối và mọi LCA cần thiết để kết nối chúng. Một cây ảo có tối đa khoảng`4m`đỉnh, vậy ở đây nhiều nhất là 8.000. Chiến lược nén tương tự là ý tưởng trung tâm của giải pháp được công bố cho vấn đề này. 

Có một sự tinh tế. Chúng ta không thể đơn giản loại bỏ các đỉnh ban đầu giữa hai đỉnh cây ảo. Giả sử cây ảo chứa một cạnh từ`a`ĐẾN`b`, nhưng đường dẫn cây ban đầu từ`a`ĐẾN`b`có mười đỉnh bên trong. Mọi đường dẫn liên quan đều sử dụng toàn bộ chuỗi đó hoặc hoàn toàn không sử dụng chuỗi đó, vì không có điểm cuối hoặc LCA bắt buộc nào nằm hoàn toàn bên trong chuỗi. Do đó, tất cả các đỉnh bên trong luôn có cùng giá trị. Chúng ta có thể lưu trữ một giá trị cho toàn bộ chuỗi nén cùng với số đỉnh của nó. 

Đỉnh cây ảo lưu trữ giá trị của đỉnh gốc đó. Cạnh cây ảo lưu trữ giá trị được chia sẻ bởi tất cả các đỉnh bên trong bị bỏ qua trên chuỗi cây gốc tương ứng. Đường dẫn giữa hai điểm cuối sau đó có thể được xử lý bằng cách leo lên cây ảo gốc về phía LCA của chúng. Mỗi cạnh ảo được truy cập một lần, thay vì một lần cho mỗi đỉnh ban đầu mà nó đại diện. 

Hai cách tiếp cận có thể được tóm tắt như sau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm) | O(n) | Quá chậm | 
| Cây ảo | O(n log n + m2) | O(n log n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lấy gốc cây gốc ở đỉnh 1 và tính`parent`,`depth`, thứ tự DFS và kích thước cây con. Thứ tự DFS cùng với kích thước cây con cho phép chúng tôi kiểm tra xem một đỉnh có phải là tổ tiên của đỉnh khác hay không, điều này cần thiết khi xây dựng cây ảo. 
2. Làm bàn nâng nhị phân cho tổ tiên. Điều này mang lại`LCA(u, v)`trong O(log n), cần thiết cả khi tạo cây ảo và khi xử lý các sự kiện riêng lẻ. 
3. Đọc tất cả các sự kiện trước khi thực hiện bất kỳ sự kiện nào. Thu thập cả hai điểm cuối của mọi sự kiện. Có nhiều nhất`2m`các đỉnh như vậy, do đó toàn bộ phần liên quan của cây vẫn nhỏ ngay cả khi cây ban đầu có 500.000 đỉnh. 
4. Sắp xếp các điểm cuối riêng biệt theo thứ tự DFS. Đối với mỗi hai điểm cuối liên tiếp theo thứ tự đó, hãy tính LCA của chúng và thêm nó vào tập đỉnh có liên quan. Ngoài ra còn thêm root. Sau khi sắp xếp và loại bỏ các đỉnh trùng lặp một lần nữa, các đỉnh này tạo thành chính xác các đỉnh quan trọng cần thiết cho cây ảo. 
5. Xây dựng cây ảo bằng ngăn xếp. Xử lý các đỉnh quan trọng theo thứ tự DFS. Ngăn xếp đại diện cho chuỗi tổ tiên ảo hiện tại. Đối với mỗi đỉnh mới, hãy bật các đỉnh cho đến khi đỉnh ngăn xếp là tổ tiên của đỉnh mới, sau đó biến đỉnh đó thành đỉnh ảo. 
6. Đối với mọi cạnh của cây ảo từ một đứa trẻ`x`tới cha mẹ ảo của nó`p`, lưu trữ hai mẩu thông tin. Đỉnh con`x`có giá trị riêng của nó, trong khi cạnh lưu trữ giá trị của các đỉnh ban đầu đúng giữa`p`Và`x`. Số của họ là 

[ 
độ sâu [x] -độ sâu [p] -1. 
] 

Số đếm có thể bằng 0, trong trường hợp đó cạnh không có đỉnh bị bỏ sót. 

1. Xử lý bản cập nhật bằng cách tìm LCA của các điểm cuối của nó. Leo từ mỗi điểm cuối tới LCA thông qua cha mẹ ảo. Mọi đỉnh ảo được truy cập đều nhận được bản cập nhật và mọi cạnh nén tương ứng cũng nhận được bản cập nhật. Cuối cùng, áp dụng bản cập nhật cho chính LCA. 
2. Đối với một truy vấn, hãy thực hiện hai lần leo giống nhau. Đối với mỗi đỉnh ảo gặp phải, hãy bao gồm một bản sao giá trị của nó. Đối với mỗi cạnh được nén, hãy bao gồm giá trị được lưu trữ của nó với bội số`depth[x] - depth[parent[x]] - 1`. Tổng được nhân với bội số đó, XOR chỉ được bao gồm khi bội số là số lẻ và giá trị tối thiểu, tối đa và khoảng cách đến-`k`các phép tính sử dụng giá trị được lưu trữ một lần vì mọi đỉnh được biểu diễn đều có cùng giá trị đó. 
3. Đối với truy vấn loại 4, trả về tổng tích lũy. Đối với loại 5 trả về XOR tích lũy. Đối với trả về loại 6`maximum - minimum`. Đối với loại 7, trả về chênh lệch tuyệt đối nhỏ nhất từ ​​​​`k`. 

**Tại sao nó hoạt động.** Xem xét mọi cạnh cây ảo được nén từ`p`ĐẾN`x`. Bằng cách xây dựng, không có điểm cuối hoặc LCA bắt buộc nào nằm hoàn toàn bên trong đường dẫn ban đầu từ`p`ĐẾN`x`. Mọi đường dẫn sự kiện có điểm cuối đến từ tập hợp điểm cuối được thu thập phải chứa toàn bộ chuỗi nội bộ hoặc không chứa chuỗi đó. Do đó, tất cả các đỉnh bên trong trên chuỗi đó luôn trải qua cùng một chuỗi cập nhật, vì vậy chúng luôn có một giá trị chung. Các giá trị đỉnh ảo biểu thị riêng từng đỉnh quan trọng ban đầu, trong khi mỗi cạnh được nén biểu thị tất cả các đỉnh bên trong bị bỏ qua với bội số chính xác của chúng. Do đó, mọi thao tác đường dẫn đều được áp dụng cho chính xác các đỉnh ban đầu giống như trong cây không nén và mọi truy vấn sẽ tổng hợp chính xác các đỉnh giống nhau đó. 

## Giải pháp Python```python
import sys
from array import array

input = sys.stdin.readline

def solve():
    t = int(input())
    output = []

    for _ in range(t):
        n, m = map(int, input().split())

        head = array('i', [-1]) * (n + 1)
        to = array('i', [0]) * (2 * (n - 1))
        nxt = array('i', [0]) * (2 * (n - 1))
        edge_count = 0

        for _ in range(n - 1):
            u, v = map(int, input().split())

            to[edge_count] = v
            nxt[edge_count] = head[u]
            head[u] = edge_count
            edge_count += 1

            to[edge_count] = u
            nxt[edge_count] = head[v]
            head[v] = edge_count
            edge_count += 1

        parent = array('i', [0]) * (n + 1)
        depth = array('i', [0]) * (n + 1)
        tin = array('i', [0]) * (n + 1)
        order = array('i')

        stack = [1]
        timer = 0

        while stack:
            x = stack.pop()
            timer += 1
            tin[x] = timer
            order.append(x)

            e = head[x]
            while e != -1:
                y = to[e]
                if y != parent[x]:
                    parent[y] = x
                    depth[y] = depth[x] + 1
                    stack.append(y)
                e = nxt[e]

        size = array('i', [1]) * (n + 1)
        for x in reversed(order):
            p = parent[x]
            if p:
                size[p] += size[x]

        log = n.bit_length()
        up = [parent]

        for _ in range(1, log):
            prev = up[-1]
            cur = array('i', [0]) * (n + 1)
            for i in range(1, n + 1):
                cur[i] = prev[prev[i]]
            up.append(cur)

        def lca(a, b):
            if depth[a] < depth[b]:
                a, b = b, a

            diff = depth[a] - depth[b]
            bit = 0
            while diff:
                if diff & 1:
                    a = up[bit][a]
                diff >>= 1
                bit += 1

            if a == b:
                return a

            for j in range(log - 1, -1, -1):
                ua = up[j][a]
                ub = up[j][b]
                if ua != ub:
                    a = ua
                    b = ub

            return parent[a]

        operations = []
        endpoints = []

        for _ in range(m):
            parts = list(map(int, input().split()))
            typ, u, v = parts[0], parts[1], parts[2]
            k = parts[3] if len(parts) == 4 else 0

            operations.append((typ, u, v, k))
            endpoints.append(u)
            endpoints.append(v)

        critical = sorted(set(endpoints), key=tin.__getitem__)

        extra = []
        for i in range(1, len(critical)):
            extra.append(lca(critical[i - 1], critical[i]))

        virtual_nodes = sorted(
            set(critical + extra + [1]),
            key=tin.__getitem__
        )

        def is_ancestor(a, b):
            return tin[a] <= tin[b] < tin[a] + size[a]

        vparent = [0] * (n + 1)
        edge_id = [0] * (n + 1)

        edge_value = [0]
        vstack = []

        for x in virtual_nodes:
            if not vstack:
                vstack.append(x)
                continue

            while not is_ancestor(vstack[-1], x):
                vstack.pop()

            p = vstack[-1]
            vparent[x] = p
            edge_id[x] = len(edge_value)
            edge_value.append(0)

            vstack.append(x)

        value = [0] * (n + 1)

        def change(x, k, typ):
            if typ == 1:
                return x + k
            if typ == 2:
                return x ^ k
            if x >= k:
                return x - k
            return x

        for typ, u, v, k in operations:
            if typ <= 3:
                a = lca(u, v)

                x = u
                while x != a:
                    value[x] = change(value[x], k, typ)
                    e = edge_id[x]
                    edge_value[e] = change(edge_value[e], k, typ)
                    x = vparent[x]

                x = v
                while x != a:
                    value[x] = change(value[x], k, typ)
                    e = edge_id[x]
                    edge_value[e] = change(edge_value[e], k, typ)
                    x = vparent[x]

                value[a] = change(value[a], k, typ)
                continue

            a = lca(u, v)

            total_sum = 0
            total_xor = 0
            maximum = -1
            minimum = 10**30
            closest = 10**30

            x = u
            while x != a:
                cur = value[x]

                total_sum += cur
                total_xor ^= cur
                if cur > maximum:
                    maximum = cur
                if cur < minimum:
                    minimum = cur
                d = abs(cur - k)
                if d < closest:
                    closest = d

                p = vparent[x]
                cnt = depth[x] - depth[p] - 1

                if cnt:
                    cur = edge_value[edge_id[x]]
                    total_sum += cnt * cur

                    if cnt & 1:
                        total_xor ^= cur

                    if cur > maximum:
                        maximum = cur
                    if cur < minimum:
                        minimum = cur

                    d = abs(cur - k)
                    if d < closest:
                        closest = d

                x = p

            x = v
            while x != a:
                cur = value[x]

                total_sum += cur
                total_xor ^= cur
                if cur > maximum:
                    maximum = cur
                if cur < minimum:
                    minimum = cur
                d = abs(cur - k)
                if d < closest:
                    closest = d

                p = vparent[x]
                cnt = depth[x] - depth[p] - 1

                if cnt:
                    cur = edge_value[edge_id[x]]
                    total_sum += cnt * cur

                    if cnt & 1:
                        total_xor ^= cur

                    if cur > maximum:
                        maximum = cur
                    if cur < minimum:
                        minimum = cur

                    d = abs(cur - k)
                    if d < closest:
                        closest = d

                x = p

            cur = value[a]
            total_sum += cur
            total_xor ^= cur

            if cur > maximum:
                maximum = cur
            if cur < minimum:
                minimum = cur

            d = abs(cur - k)
            if d < closest:
                closest = d

            if typ == 4:
                output.append(str(total_sum))
            elif typ == 5:
                output.append(str(total_xor))
            elif typ == 6:
                output.append(str(maximum - minimum))
            else:
                output.append(str(closest))

    sys.stdout.write("\n".join(output))

if __name__ == "__main__":
    solve()
```Cấu trúc kề sử dụng ba mảng số nguyên nhỏ gọn thay vì danh sách danh sách Python. Điều này quan trọng vì cây ban đầu có thể chứa 500.000 đỉnh và gần một triệu mục kề cận có hướng. Các mảng giữ cho dung lượng bộ nhớ nhỏ hơn đáng kể. 

DFS ban đầu là lặp lại chứ không phải đệ quy. Một cây có thể là một chuỗi có độ dài 500.000, vượt quá giới hạn đệ quy thông thường của Python và cũng sẽ khiến các lệnh gọi Python đệ quy trở nên tốn kém một cách không cần thiết. 

các`parent`,`depth`,`tin`, Và`size`mảng là đủ để xác định tổ tiên. Đối với các đỉnh`a`Và`b`,`a`là tổ tiên của`b`chính xác khi nào`tin[a] <= tin[b] < tin[a] + size[a]`. Cây con chiếm một khoảng liền kề của thứ tự DFS. 

Nâng nhị phân được lưu trữ trong`array('i')`đối tượng vì 500.000 đỉnh nhân với khoảng 19 cấp độ là khoảng mười triệu mục nhập tổ tiên. Một ma trận số nguyên Python bình thường sẽ tiêu tốn nhiều bộ nhớ hơn. 

Cây ảo chỉ được xây dựng một lần vì mọi sự kiện đều được biết trước khi thực hiện. Đây là một lợi thế lớn của giá trị nhỏ của`m`. các`vparent`mảng trỏ từ mọi đỉnh ảo tới đỉnh ảo của nó, trong khi`edge_id[x]`xác định cạnh nén ngay phía trên`x`. 

Mảng giá trị được lập chỉ mục theo số đỉnh ban đầu, nhưng chỉ các đỉnh ảo mới nhận được giá trị khác 0. Các giá trị cạnh nén được lưu trữ riêng biệt vì một cạnh có thể biểu thị nhiều đỉnh ban đầu và các đỉnh đó không được nhầm lẫn với điểm cuối được biểu thị bởi đỉnh ảo con. 

Để trừ, so sánh là`x >= k`, không`x > k`. Khi`x == k`, giá trị kết quả bằng không. Khi`x < k`, giá trị không đổi. 

Đối với các truy vấn XOR, một cạnh được nén chứa`cnt`các giá trị bằng nhau đóng góp giá trị đó khi`cnt`là số lẻ và đóng góp bằng 0 khi`cnt`là chẵn. Đây chính xác là thuộc tính chẵn lẻ của XOR lặp đi lặp lại. 

Các số nguyên Python có độ chính xác tùy ý, do đó, tổng đường dẫn có thể lớn không yêu cầu xử lý 64 bit rõ ràng. Việc triển khai C++ sẽ cần loại số nguyên 64 bit, nhưng biểu diễn số nguyên của Python đã xử lý phạm vi được yêu cầu. 

## Ví dụ đã hoạt động 

Đối với mẫu được cung cấp, đầu vào hiển thị trong câu lệnh đại diện cho một trường hợp thử nghiệm. Với số lượng trường hợp thử nghiệm được bao gồm, đó là:```
1
5 8
5 2
5 1
2 4
2 3
1 4 4 5
3 4 4 1
2 3 1 4
6 3 5
4 2 5
5 1 3
6 5 4
7 1 4 2
```Cây có cấu trúc dạng dây chuyền với đỉnh 2 được kết nối với 3 và 4, và đỉnh 5 được kết nối với 2 và 1. 

| Sự kiện | Đường dẫn | Giá trị sau sự kiện | Trả lời | 
| --- | --- | --- | --- | 
|`1 4 4 5`|`{4}`|`w4 = 5`| | 
|`3 4 4 1`|`{4}`|`w4 = 4`| | 
|`2 3 1 4`|`{3,2,5,1}`|`w1=w2=w3=w4=w5=4`| | 
|`6 3 5`|`{3,2,5}`| cả ba giá trị đều là 4 |`0`| 
|`4 2 5`|`{2,5}`| cả hai giá trị đều là 4 |`8`| 
|`5 1 3`|`{1,5,2,3}`| bốn giá trị là 4 |`0`| 
|`6 5 4`|`{5,2,4}`| cả ba giá trị đều là 4 |`0`| 
|`7 1 4 2`|`{1,5,2,4}`| cả bốn giá trị đều là 4 |`2`| 

Do đó, đầu ra là`0`,`8`,`0`,`0`, Và`2`. Dấu vết chứng minh rằng các bản cập nhật XOR có thể xóa hoàn toàn những khác biệt được tạo bởi các bản cập nhật số học trước đó, do đó cấu trúc dữ liệu phải lưu trữ giá trị hiện tại thực tế thay vì cố gắng tóm tắt lịch sử hoạt động. 

Đối với ví dụ thứ hai, hãy xem xét:```
1
3 6
1 2
2 3
1 1 3 5
2 2 3 3
3 1 2 6
4 1 3
5 1 3
7 1 3 4
```Nhà nước phát triển như sau. 

| Sự kiện | Đường dẫn | Giá trị sau sự kiện | Trả lời | 
| --- | --- | --- | --- | 
|`1 1 3 5`|`{1,2,3}`|`(5,5,5)`| | 
|`2 2 3 3`|`{2,3}`|`(5,6,6)`| | 
|`3 1 2 6`|`{1,2}`|`(5,0,6)`| | 
|`4 1 3`|`{1,2,3}`|`(5,0,6)`|`11`| 
|`5 1 3`|`{1,2,3}`|`(5,0,6)`|`3`| 
|`7 1 3 4`|`{1,2,3}`|`(5,0,6)`|`1`| 

Sự kiện thứ ba thực hiện phép trừ có điều kiện. Đỉnh 1 có giá trị 5, nhỏ hơn 6 nên giữ nguyên là 5, còn đỉnh 2 bằng 0. Truy vấn cuối cùng yêu cầu giá trị gần nhất với 4 và 5 ở khoảng cách 1, vì vậy câu trả lời là 1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n + m2) | Nâng nhị phân mất O(n log n), trong khi tối đa O(m) đỉnh ảo được đi qua bởi mỗi sự kiện O(m) | 
| Không gian | O(n log n) | Bàn nâng nhị phân chiếm ưu thế trong mảng cây nhỏ gọn | 

Có nhiều nhất`2m`điểm cuối ban đầu và nhiều nhất là một LCA cho mỗi cặp liên tiếp sau khi sắp xếp, do đó cây ảo chứa tối đa khoảng`4m + 1`đỉnh. Với`m <= 2000`, nhiều nhất là khoảng 8.000 đỉnh ảo. Do đó, việc xử lý tất cả các sự kiện yêu cầu khoảng 16 triệu lượt truy cập vào cây ảo trong trường hợp xấu nhất, thay vì một tỷ lượt truy cập vào đỉnh cây ban đầu. Cái lớn`n`được xử lý một lần trong quá trình tiền xử lý, trong khi phần nhỏ`m`kiểm soát phần năng động đắt tiền. 

## Trường hợp thử nghiệm 

Khai thác thử nghiệm sau đây giả định giải pháp đã gửi được lưu dưới dạng`solution.py`. Trường hợp kích thước tối đa được tạo ra thay vì được viết ra một cách rõ ràng, do đó nó vẫn thể hiện ràng buộc đầy đủ.```python
import io
import subprocess
import sys

def run(inp: str) -> str:
    result = subprocess.run(
        [sys.executable, "solution.py"],
        input=inp,
        text=True,
        capture_output=True,
        check=True,
    )
    return result.stdout.strip()

sample1 = """\
1
5 8
5 2
5 1
2 4
2 3
1 4 4 5
3 4 4 1
2 3 1 4
6 3 5
4 2 5
5 1 3
6 5 4
7 1 4 2
"""

assert run(sample1) == "0\n8\n0\n0\n2", "sample 1"

sample2 = """\
1
3 6
1 2
2 3
1 1 3 5
2 2 3 3
3 1 2 6
4 1 3
5 1 3
7 1 3 4
"""

assert run(sample2) == "11\n3\n1", "sample 2"

minimum_case = """\
1
1 5
1 1 1 5
3 1 1 7
4 1 1
2 1 1 3
5 1 1
"""

assert run(minimum_case) == "5\n6", "minimum-size and ignored subtraction"

equal_case = """\
1
4 6
1 2
2 3
3 4
1 1 4 7
4 1 4
5 1 4
6 1 4
7 1 4 5
3 1 4 7
"""

assert run(equal_case) == "28\n0\n0\n2", "all-equal values"

compressed_edge_case = """\
1
5 5
1 2
2 3
3 4
4 5
1 1 5 3
4 1 5
3 2 4 2
4 1 5
7 1 5 2
"""

assert run(compressed_edge_case) == "15\n9\n1", "compressed edge multiplicity"

n = 500000
edges = "\n".join(f"{i} {i + 1}" for i in range(1, n))
maximum_case = (
    f"1\n{n} 2\n"
    + edges
    + f"\n1 1 {n} 1\n4 1 {n}\n"
)

assert run(maximum_case) == str(n), "maximum-size chain"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cung cấp mẫu 1 |`0 8 0 0 2`| Các truy vấn số học, XOR, phạm vi, tổng và giá trị gần nhất | 
| Mẫu 2 |`11 3 1`| Phép trừ có điều kiện và các loại truy vấn hỗn hợp | 
| Vỏ kích thước tối thiểu |`5 6`| Một đỉnh duy nhất và phép trừ khi`w < k`| 
| Trường hợp hoàn toàn bằng nhau |`28 0 0 2`| Tính chẵn lẻ XOR, phạm vi 0 và tính toán giá trị gần nhất | 
| Vỏ nén cạnh |`15 9 1`| Chỉnh sửa bội số của các đỉnh cây ảo bị bỏ qua | 
| Chuỗi kích thước tối đa |`500000`| Lớn`n`với đường dẫn chứa toàn bộ cây | 

## Vỏ cạnh 

Trường hợp trừ có điều kiện được xử lý trực tiếp trong`change`. Vì```
1
1 2
1 1 1 5
3 1 1 7
4 1 1
```đỉnh duy nhất có giá trị 5 sau sự kiện đầu tiên. Từ`5 < 7`, sự kiện thứ hai trả về giá trị không thay đổi và kết quả truy vấn`5`. Việc triển khai không bao giờ thay thế giá trị bằng 0 trừ khi số tiền trừ chính xác bằng giá trị. 

Trường hợp bội số XOR được xử lý khi truy vấn một cạnh nén. Giả sử một cạnh đại diện cho bốn đỉnh ban đầu, tất cả đều có giá trị 5. Đoạn mã thêm`5`chỉ vào bộ tích lũy XOR khi`cnt & 1`là đúng. Vì bốn là số chẵn nên phần đóng góp bằng 0, phù hợp`5 XOR 5 XOR 5 XOR 5 = 0`. Điều này tránh việc mở rộng cạnh bị nén trở lại các đỉnh ban đầu của nó. 

Trường hợp ranh giới cạnh nén được xử lý bằng cách tách từng đỉnh ảo khỏi các đỉnh bên trong ngay bên dưới nó. TRÊN```
1
5 2
1 2
2 3
3 4
4 5
1 1 5 3
4 1 5
```cây ảo về cơ bản có thể bao gồm các đỉnh 1 và 5. Đỉnh 1 lưu trữ 3, đỉnh 5 lưu trữ 3 và cạnh ảo lưu trữ 3 với bội số`5 - 1 - 1 = 3`. Truy vấn tính toán`3 + 3 * 3 + 3 = 15`, khớp chính xác với tất cả năm đỉnh ban đầu. 

Vụ án`u = v`cũng cần phải cẩn thận vì đường đi chỉ chứa đúng một đỉnh. Các vòng cập nhật từ`u`về phía LCA, nhưng khi điểm cuối giống hệt nhau,`u == lca(u, v)`và cả hai vòng leo đều thực hiện không lần nào. Bản thân LCA sau đó được cập nhật chính xác một lần. Lý do tương tự làm cho mọi truy vấn trên một đỉnh đều trả về giá trị hiện tại của đỉnh đó. 

Root có thể vắng mặt ở mọi điểm cuối sự kiện. Việc xây dựng chèn đỉnh 1 vào cây ảo một cách rõ ràng để mỗi đỉnh ảo có một chuỗi tổ tiên được xác định rõ ràng. Điều này không làm thay đổi bất kỳ đường đi sự kiện nào, bởi vì gốc chỉ là một đỉnh cấu trúc trừ khi có một đường đi thực sự đi qua nó. 

Cuối cùng, một cạnh bị nén có thể có các đỉnh bên trong bằng 0. Khi hai đỉnh ảo kề nhau trên cây ban đầu,`depth[x] - depth[parent[x]] - 1`bằng không. Mã vẫn gán một giá trị cạnh cho tính đồng nhất, nhưng logic truy vấn và cập nhật sẽ bỏ qua nó bất cứ khi nào bội số của nó bằng 0. Điều này ngăn chặn một đỉnh nhân tạo bổ sung nhập vào bất kỳ câu trả lời nào.
