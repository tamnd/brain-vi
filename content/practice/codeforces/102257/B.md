---
title: "CF 102257B - Cầu"
description: "Chúng ta có một đồ thị vô hướng có các đỉnh là các hòn đảo và các cạnh là các cây cầu. Mỗi cây cầu đều có một giới hạn về trọng lượng, do đó một chiếc ô tô có trọng lượng w có thể sử dụng chính xác những cây cầu có giới hạn hiện tại ít nhất là w."
date: "2026-08-17T20:57:51+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102257
codeforces_index: "B"
codeforces_contest_name: "2019 Asia-Pacific Informatics Olympiad (APIO 19)"
rating: 0
weight: 102257
solve_time_s: 256
verified: true
draft: false
---

[CF 102257B - Cầu](https://codeforces.com/problemset/problem/102257/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4 phút 16s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị vô hướng có các đỉnh là các hòn đảo và các cạnh là các cây cầu. Mỗi cây cầu đều có giới hạn về trọng lượng nên ô tô có trọng lượng`w`có thể sử dụng chính xác những cây cầu có giới hạn hiện tại ít nhất là`w`. Một bản cập nhật thay đổi giới hạn của một cây cầu hiện có, trong khi một truy vấn yêu cầu số lượng đảo trong thành phần được kết nối chứa một hòn đảo được chỉ định khi chỉ có những cây cầu có thể chở chiếc xe nhất định. Giới hạn biểu đồ và truy vấn đủ lớn để chúng ta cần khai thác thực tế là toàn bộ chuỗi truy vấn đã được biết trước. Những ràng buộc chính thức là`n <= 50,000`,`m <= 100,000`, Và`q <= 100,000`, với giới hạn thời gian 2 giây và giới hạn bộ nhớ 512 MB. 

Khó khăn chính là hai chiều thay đổi cùng một lúc. Biểu đồ thay đổi theo thời gian vì giới hạn cầu nối được cập nhật, trong khi mỗi truy vấn cũng chọn ngưỡng trọng số khác nhau. Việc truyền tải trực tiếp cho mọi truy vấn về cơ bản sẽ kiểm tra toàn bộ biểu đồ mỗi lần. Với tối đa`100,000`truy vấn và`100,000`các cạnh, thậm chí một`O(n + m)`truyền tải trên mỗi truy vấn có thể đạt tới khoảng`1.5 * 10^10`thăm đỉnh và cạnh trong trường hợp xấu nhất, vượt xa giới hạn 2 giây cho phép. 

Có một số trường hợp nhỏ rất dễ xử lý sai. Đầu tiên, một biểu đồ trống vẫn phải tính chính hòn đảo bắt đầu. Ví dụ,```
1 0
1
2 1 1
```có đầu ra```
1
```bởi vì một hòn đảo có thể đến được từ chính nó mà không cần sử dụng bất kỳ cây cầu nào. Việc triển khai truyền tải khởi tạo câu trả lời của nó từ số lượng hàng xóm được truy cập có thể vô tình trả về 0. 

Việc so sánh với giới hạn cầu là bao gồm. Ví dụ,```
2 1
1 2 5
1
2 1 5
```có đầu ra```
2
```bởi vì chính xác là một chiếc xe có trọng lượng`5`được phép đi lên một cây cầu có giới hạn là`5`. sử dụng`>`thay vì`>=`âm thầm mất kết nối này. 

Một bản cập nhật có thể làm cho cây cầu yếu hơn cũng như mạnh hơn. Ví dụ,```
2 1
1 2 5
3
2 1 5
1 1 1
2 1 2
```sản xuất```
2
1
```bởi vì sau khi cập nhật cây cầu duy nhất có giới hạn`1`, vậy một chiếc xe có trọng lượng`2`không thể vượt qua nó. Việc triển khai chỉ lưu trữ các trọng số ban đầu sẽ tiếp tục quay trở lại một cách không chính xác`2`. 

Những cây cầu song song cũng phải tách biệt. Ví dụ,```
2 2
1 2 3
1 2 5
3
2 1 4
1 2 2
2 1 4
```sản xuất```
2
1
```bởi vì trước khi cập nhật cầu giới hạn`5`nối các hòn đảo, trong khi sau khi đổi cây cầu đó thành`2`, cây cầu còn lại chỉ có giới hạn`3`. Chỉ xử lý các cầu bằng cặp điểm cuối của chúng sẽ làm mất đi sự khác biệt này. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản. Đối với mọi truy vấn loại 2, hãy quét tất cả các cầu nối, chỉ giữ lại những cầu nối có giới hạn hiện tại ít nhất bằng trọng số được truy vấn và chạy BFS hoặc DFS từ đảo được yêu cầu. Số lượt truy cập thu được chính xác là câu trả lời vì quá trình truyền tải sử dụng chính xác các cầu có sẵn cho chiếc xe đó. Phương pháp này đúng, nhưng chi phí trong trường hợp xấu nhất của nó là`O(q(n + m))`. Với`n = 50,000`,`m = 100,000`, Và`q = 100,000`, đại khái là vậy`1.5 * 10^10`các thao tác đỉnh và cạnh cơ bản, trước khi tính đến chi phí Python. 

Quan sát hữu ích là các bản cập nhật thưa thớt bên trong một khối truy vấn ngắn liên tiếp. Chia chuỗi truy vấn thành các khối khoảng`B = sqrt(q)`hoạt động. Bên trong một khối, chỉ có thể cập nhật một số lượng nhỏ các cây cầu riêng biệt. Gọi những cây cầu này là đặc biệt. Mọi cây cầu khác đều giữ nguyên trọng lượng như nhau trong toàn bộ khu nhà. 

Đối với một khối, tạm thời loại bỏ tất cả các cây cầu đặc biệt. Đồ thị còn lại là tĩnh. Chúng tôi có thể sắp xếp các cạnh của nó theo trọng số và xử lý tất cả các truy vấn loại 2 theo thứ tự giảm dần về trọng số được yêu cầu của chúng. Sau đó, DSU biểu thị các thành phần được kết nối được hình thành bởi tất cả các cây cầu cố định có trọng số đủ lớn cho truy vấn hiện tại. 

Chỉ còn thiếu những cây cầu đặc biệt. Có nhiều nhất`B`trong số chúng, vì vậy đối với một truy vấn, chúng ta có thể lấy các thành phần DSU của điểm cuối của chúng và xây dựng một DSU phụ trợ nhỏ. Mỗi đỉnh phụ đại diện cho toàn bộ thành phần của đồ thị cố định, với kích thước ban đầu của nó bằng với kích thước được DSU chính lưu trữ. Việc thêm các cây cầu đặc biệt vào biểu đồ nhỏ này sẽ cung cấp chính xác thành phần chứa hòn đảo được truy vấn. 

Phương pháp brute-force hoạt động vì nó xây dựng biểu đồ ngưỡng chính xác cho mọi truy vấn, nhưng nó xây dựng lại gần như cùng một thông tin nhiều lần. Việc quan sát khối cho phép chúng tôi xây dựng phần tĩnh đắt tiền một lần trên mỗi khối và tách biệt tất cả các thay đổi thành nhiều nhất`B`các cạnh cho mỗi truy vấn. 

Việc triển khai bên dưới cũng thể hiện trạng thái cạnh đặc biệt hiện tại của mọi truy vấn dưới dạng mặt nạ bit. Trong quá trình di chuyển theo trình tự thời gian qua một khối, chúng tôi biết trọng số hiện tại của từng cây cầu đặc biệt, vì vậy chúng tôi có thể ghi lại những cây cầu đặc biệt nào đáp ứng ngưỡng của từng truy vấn. Sau đó, khi các truy vấn được sắp xếp lại theo trọng số, trạng thái thời gian ban đầu của chúng vẫn được giữ nguyên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(q(n + m))`|`O(n + m)`| Quá chậm | 
| Phân hủy khối |`O((q/B)m log m + qB)`|`O(n + m + qB)`ở dạng đơn giản | Đã chấp nhận | 

Với`B`xung quanh`sqrt(q)`, công việc tốn kém được trải rộng trên các khối trong khi mỗi truy vấn riêng lẻ chỉ cần kiểm tra một tập hợp nhỏ các cây cầu đặc biệt. 

## Hướng dẫn thuật toán 

1. Đọc biểu đồ hoàn chỉnh và chuỗi truy vấn hoàn chỉnh trước khi trả lời bất kỳ điều gì. Đây là điều khiến cho việc phân tách khối trở nên khả thi, bởi vì chúng ta có thể biết trước những cầu nối nào sẽ được cập nhật bên trong mỗi khối. 
2. Chia các truy vấn thành các khối có kích thước liên tiếp khoảng`sqrt(q)`. Đối với khối hiện tại, hãy thu thập mọi cây cầu được cập nhật ít nhất một lần. Đây là những cây cầu đặc biệt. Có thể có nhiều nhất`B`những cây cầu đặc biệt riêng biệt vì khối chỉ chứa`B`hoạt động. 
3. Mô phỏng khối theo thứ tự thời gian ban đầu. Duy trì trọng lượng hiện tại của mỗi cây cầu. Bất cứ khi nào truy vấn loại 1 thay đổi một cây cầu đặc biệt, hãy cập nhật trọng số của nó. Đối với mỗi truy vấn loại 2, hãy quét các cầu nối đặc biệt và tạo một mặt nạ bit chứa chính xác các cầu nối đặc biệt có trọng số hiện tại ít nhất bằng trọng số truy vấn. Điều này ghi lại phần đặc biệt của biểu đồ tại thời điểm chính xác của truy vấn. 
4. Loại trừ tất cả các cầu đặc biệt và sắp xếp các cầu cố định còn lại theo trọng lượng giảm dần. Trọng số của chúng không bao giờ thay đổi trong suốt khối, vì vậy thứ tự sắp xếp này hợp lệ cho mọi truy vấn trong khối. 
5. Sắp xếp các truy vấn loại 2 của khối bằng cách giảm trọng lượng ô tô yêu cầu. Bắt đầu với một DSU trống. Khi ngưỡng truy vấn giảm, hãy thêm mọi cây cầu cố định có trọng số hiện đã đủ lớn. Do đó, DSU đại diện cho tất cả các cầu nối cố định có thể sử dụng được bởi truy vấn đó. 
6. Đối với mỗi truy vấn theo thứ tự được sắp xếp này, hãy tìm thành phần đồ thị cố định chứa hòn đảo bắt đầu của nó. Sau đó tạo một DSU tạm thời chỉ chứa các thành phần cơ bản được tiếp xúc bởi các cầu đặc biệt đang hoạt động. Cung cấp cho mỗi nút tạm thời kích thước của thành phần cơ sở tương ứng. 
7. Đối với mỗi cây cầu đặc biệt có bit xuất hiện trong mặt nạ truy vấn, hãy tìm các thành phần cơ sở chứa điểm cuối của nó và hợp nhất các nút tạm thời đó. Nếu cả hai điểm cuối đều đã có trong cùng một thành phần cơ sở thì cây cầu đặc biệt sẽ không thay đổi gì. 
8. Kích thước của thành phần tạm thời chứa hòn đảo khởi đầu chính là câu trả lời. Lưu trữ nó ở vị trí ban đầu của truy vấn vì các truy vấn được xử lý theo thứ tự trọng lượng thay vì thứ tự đầu vào. 
9. Chuyển sang khối tiếp theo. Trọng số cầu bây giờ chứa chính xác trạng thái sau tất cả các cập nhật trong khối trước đó, do đó quá trình tương tự có thể được lặp lại. 

### Tại sao nó hoạt động 

Đối với một khối cố định, mỗi cây cầu không đặc biệt đều có trọng lượng không đổi trên toàn bộ khối. Khi các truy vấn được xử lý theo thứ tự ngưỡng giảm dần, DSU chính chứa chính xác những cầu nối cố định có giới hạn ít nhất là ngưỡng hiện tại. Do đó, các thành phần của nó chính xác là các thành phần được kết nối có thể đạt được mà không cần cầu nối đặc biệt. 

Mỗi cây cầu còn sử dụng được đều đặc biệt và chỉ có`B`của họ. Việc thu gọn mọi thành phần DSU chính vào một đỉnh tạm thời sẽ duy trì tất cả kết nối thông qua các cầu nối cố định. Việc thêm chính xác các cầu nối đặc biệt có giới hạn hiện tại đáp ứng truy vấn sẽ tạo ra biểu đồ ngưỡng hoàn chỉnh tại thời điểm truy vấn đó. Do đó, thành phần tạm thời chứa đảo bắt đầu chứa chính xác tất cả các đảo có thể tiếp cận được, vì vậy kích thước được lưu trữ của nó là câu trả lời bắt buộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class DSU:
    __slots__ = ("parent", "size")

    def __init__(self, n):
        self.parent = list(range(n))
        self.size = [1] * n

    def find(self, x):
        parent = self.parent
        while parent[x] != x:
            parent[x] = parent[parent[x]]
            x = parent[x]
        return x

    def union(self, a, b):
        parent = self.parent
        size = self.size

        a = self.find(a)
        b = self.find(b)

        if a == b:
            return False

        if size[a] < size[b]:
            a, b = b, a

        parent[b] = a
        size[a] += size[b]
        return True

def solve():
    n, m = map(int, input().split())

    eu = [0] * m
    ev = [0] * m
    weight = [0] * m

    for i in range(m):
        u, v, w = map(int, input().split())
        eu[i] = u - 1
        ev[i] = v - 1
        weight[i] = w

    q = int(input())
    queries = [None] * q

    for i in range(q):
        t, a, b = map(int, input().split())
        queries[i] = (t, a - 1, b)

    answers = [None] * q
    mask_at = [0] * q

    block_size = max(1, int(q ** 0.5) + 1)

    for left in range(0, q, block_size):
        right = min(q, left + block_size)

        special_set = set()

        for i in range(left, right):
            t, a, b = queries[i]
            if t == 1:
                special_set.add(a)

        special = list(special_set)
        k = len(special)

        special_pos = {e: j for j, e in enumerate(special)}

        # Record the exact state of special edges for every query.
        for i in range(left, right):
            t, a, b = queries[i]

            if t == 1:
                weight[a] = b
            else:
                mask = 0
                for j, e in enumerate(special):
                    if weight[e] >= b:
                        mask |= 1 << j
                mask_at[i] = mask

        # The non-special edges are static throughout this block.
        fixed = [e for e in range(m) if e not in special_set]
        fixed.sort(key=weight.__getitem__, reverse=True)

        # Queries are processed by threshold, not by their original order.
        ordered_queries = [
            i for i in range(left, right)
            if queries[i][0] == 2
        ]
        ordered_queries.sort(key=lambda i: queries[i][2], reverse=True)

        base = DSU(n)
        edge_ptr = 0
        fixed_count = len(fixed)

        for qi in ordered_queries:
            _, s, required = queries[qi]

            while edge_ptr < fixed_count:
                e = fixed[edge_ptr]
                if weight[e] < required:
                    break

                base.union(eu[e], ev[e])
                edge_ptr += 1

            # Build the small DSU induced by the special edges.
            local_parent = []
            local_size = []
            local_index = {}

            def get_local_node(root):
                node = local_index.get(root)
                if node is None:
                    node = len(local_parent)
                    local_index[root] = node
                    local_parent.append(node)
                    local_size.append(base.size[root])
                return node

            def local_find(x):
                while local_parent[x] != x:
                    local_parent[x] = local_parent[local_parent[x]]
                    x = local_parent[x]
                return x

            root_s = base.find(s)
            local_s = get_local_node(root_s)

            mask = mask_at[qi]

            while mask:
                bit = mask & -mask
                j = bit.bit_length() - 1
                mask -= bit

                e = special[j]

                ru = base.find(eu[e])
                rv = base.find(ev[e])

                a = get_local_node(ru)
                b = get_local_node(rv)

                a = local_find(a)
                b = local_find(b)

                if a != b:
                    if local_size[a] < local_size[b]:
                        a, b = b, a

                    local_parent[b] = a
                    local_size[a] += local_size[b]

            local_root = local_find(local_s)
            answers[qi] = local_size[local_root]

    sys.stdout.write(
        "\n".join(str(answers[i]) for i in range(q) if answers[i] is not None)
    )

if __name__ == "__main__":
    solve()
```các`DSU`lưu trữ cả cha mẹ của mọi thành phần và kích thước của nó. Nén đường dẫn được sử dụng vì các DSU này không bao giờ được khôi phục. các`union`hoạt động gắn thành phần nhỏ hơn vào thành phần lớn hơn, giữ cho cây ở trạng thái nông. 

Đối với mỗi khối,`special_set`xác định chính xác những cây cầu có giá trị có thể thay đổi. Quá trình vượt qua theo trình tự thời gian phải xảy ra trước quá trình vượt qua được sắp xếp theo ngưỡng. Nếu không, một truy vấn có thể vô tình sử dụng trọng số cuối cùng của cây cầu thay vì trọng số tồn tại khi truy vấn được đưa ra. 

Các cạnh cố định được sắp xếp theo thứ tự giảm dần. Con trỏ`edge_ptr`chỉ tiến về phía trước vì ngưỡng truy vấn cũng giảm. Điều kiện là`weight[e] < required`, không`<=`, vì cây cầu có giới hạn bằng trọng lượng ô tô là có thể sử dụng được. 

DSU tạm thời sử dụng các gốc thành phần cơ sở làm đỉnh của nó. Kích thước thành phần của nó bắt đầu bằng kích thước của các thành phần cơ sở đó, vì vậy khi hai nút tạm thời được nối với nhau, kích thước của chúng có thể được thêm vào một cách đơn giản. Đây là phần biến phép tính kết nối thành phép tính kích thước thành phần được yêu cầu. 

Mặt nạ bit được lập chỉ mục theo vị trí của cây cầu bên trong`special`. sử dụng`mask & -mask`trích xuất một cây cầu đặc biệt đang hoạt động tại một thời điểm mà không cần quét các vị trí không hoạt động trong giai đoạn DSU tạm thời đắt tiền. 

Tất cả các trọng số đều vừa vặn thoải mái bên trong số nguyên Python. Trong quá trình triển khai C++, câu trả lời nhiều nhất là`n`, mặc dù giới hạn cầu yêu cầu số nguyên 32 bit. 

## Ví dụ đã hoạt động 

Mẫu đầu tiên từ tuyên bố là:```
3 4
1 2 5
2 3 2
3 1 4
2 3 8
5
2 1 5
1 4 1
2 2 5
1 1 1
2 3 2
```Đầu ra chính thức của nó là`3`,`2`,`3`. 

Đối với một dấu vết nhỏ, các truy vấn có thể được xem như sau. 

| Truy vấn | Các cạnh liên quan hiện tại | Ngưỡng | Thành phần cơ bản của start | Các cạnh đặc biệt được sử dụng | Trả lời | 
| --- | --- | --- | --- | --- | --- | 
|`2 1 5`|`1-2:5`,`2-3:8`| 5 |`{1,2,3}`| không | 3 | 
|`1 4 1`| cạnh 4 thay đổi từ 8 thành 1 | | | | | 
|`2 2 5`|`1-2:5`| 5 |`{1,2}`| cạnh 4 không có sẵn | 2 | 
|`1 1 1`| cạnh 1 thay đổi từ 5 thành 1 | | | | | 
|`2 3 2`|`2-3:2`,`3-1:4`| 2 |`{1,2,3}`| cạnh 1 không có sẵn | 3 | 

Truy vấn đầu tiên thể hiện điều kiện ngưỡng bao gồm. Truy vấn loại 2 thứ hai giải thích tại sao giá trị hiện tại của cầu cập nhật phải được sử dụng thay vì giá trị ban đầu của nó. 

Mẫu thứ hai là:```
7 8
1 2 5
1 6 5
2 3 5
2 7 5
3 4 5
4 5 5
5 6 5
6 7 5
12
2 1 6
1 1 1
2 1 2
1 2 3
2 2 2
1 5 2
1 3 1
2 2 4
2 4 2
1 8 1
2 1 1
2 1 3
```Đầu ra chính thức là`7`,`7`,`5`,`7`,`7`,`4`,`7`. 

Một dấu vết hữu ích tập trung vào số lượng cầu hiện có thể sử dụng được. 

| Truy vấn | Ngưỡng | Trạng thái cầu được cập nhật | Thành phần có thể truy cập từ`s`| Trả lời | 
| --- | --- | --- | --- | --- | 
|`2 1 6`| 6 | mọi giới hạn 5 | đảo duy nhất 1 | 1 | 
|`2 1 2`| 2 | cầu 1 có giới hạn 1 | tất cả các đảo ngoại trừ phía biệt lập đi qua cầu 1 | 7 | 
|`2 2 2`| 2 | cầu 1 = 1, cầu 2 = 3 | thành phần năm đảo | 5 | 
|`2 2 4`| 4 | một số giới hạn cập nhật dưới 4 | phần giới hạn cao | 7 | 
|`2 4 2`| 2 | cập nhật giới hạn thấp hiện có thể sử dụng được | cả bảy hòn đảo | 7 | 
|`2 1 1`| 1 | tất cả các giới hạn hiện tại ít nhất là 1 | cả bảy hòn đảo | 7 | 
|`2 1 3`| 3 | chỉ còn lại những cây cầu có giới hạn ít nhất 3 | thành phần bốn đảo | 4 | 

Tên thành phần trung gian chính xác phụ thuộc vào giới hạn cầu hiện tại, nhưng tính bất biến giống nhau ở mọi hàng: DSU chính chứa tất cả các cầu cố định đáp ứng ngưỡng hiện tại, trong khi DSU tạm thời bổ sung chính xác các cầu đặc biệt đang hoạt động. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O((q/B)m log m + qB)`| Mỗi khối sắp xếp các cạnh cố định và mỗi truy vấn xử lý tối đa`B`những cây cầu đặc biệt | 
| Không gian |`O(n + m + q)`| Biểu đồ, truy vấn, trọng số, câu trả lời và trạng thái DSU tạm thời | 

Lựa chọn`B`xung quanh`sqrt(q)`cân bằng công việc được thực hiện một lần trên mỗi khối với công việc được thực hiện cho mỗi truy vấn. Với`q <= 100,000`, một khối chỉ chứa vài trăm cạnh, do đó phần động của mọi truy vấn vẫn nhỏ. Việc triển khai cũng tránh việc xây dựng lại biểu đồ hoặc chạy BFS đầy đủ cho mỗi truy vấn, đây là thao tác khiến giải pháp vũ phu không thể thực hiện được trong các giới hạn nhất định. 

## Trường hợp thử nghiệm```python
# The solve() function from the solution above is assumed to be in the same file.

import sys
import io
from contextlib import redirect_stdout

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    output = io.StringIO()

    try:
        with redirect_stdout(output):
            solve()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

    return output.getvalue().strip()

# Provided sample 1
assert run(
    """\
3 4
1 2 5
2 3 2
3 1 4
2 3 8
5
2 1 5
1 4 1
2 2 5
1 1 1
2 3 2
"""
) == "3\n2\n3", "sample 1"

# Provided sample 2
assert run(
    """\
7 8
1 2 5
1 6 5
2 3 5
2 7 5
3 4 5
4 5 5
5 6 5
6 7 5
12
2 1 6
1 1 1
2 1 2
1 2 3
2 2 2
1 5 2
1 3 1
2 2 4
2 4 2
1 8 1
2 1 1
2 1 3
"""
) == "1\n7\n5\n7\n7\n4\n7", "sample 2"

# Minimum-size graph, no bridges.
assert run(
    """\
1 0
1
2 1 1
"""
) == "1", "single island"

# All bridge limits equal, followed by updates in both directions.
assert run(
    """\
4 3
1 2 5
2 3 5
3 4 5
5
2 1 5
1 2 7
2 1 5
1 1 4
2 1 5
"""
) == "4\n4\n1", "equal weights and updates"

# Parallel bridges and exact threshold boundary.
assert run(
    """\
2 2
1 2 3
1 2 5
4
2 1 5
1 2 2
2 1 3
2 1 4
"""
) == "2\n2\n1", "parallel edges and boundary"

# Maximum number of bridges with a single query.
# Every bridge connects the same two islands, so the answer is always 2.
max_case = (
    "50000 99999\n"
    + "1 2 1\n" * 99999
    + "1\n"
    + "2 1 1\n"
)

assert run(max_case) == "2", "maximum m"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0`, một truy vấn |`1`| Biểu đồ kích thước tối thiểu và hòn đảo bắt đầu | 
| Chuỗi bốn đảo có giới hạn bằng nhau |`4`,`4`,`1`| Giá trị hoàn toàn bằng nhau và cập nhật suy yếu | 
| Hai cây cầu song song |`2`,`2`,`1`| Các cạnh song song và xử lý ngưỡng chính xác | 
|`n=50000`,`m=99999`, một truy vấn |`2`| Số cạnh tối đa và xử lý đầu vào lớn | 

## Vỏ cạnh 

Đối với trường hợp đồ thị trống,```
1 0
1
2 1 1
```khối không có cạnh đặc biệt và không có cạnh cố định. DSU cơ sở ban đầu có một thành phần có kích thước một. Đảo được truy vấn được ánh xạ tới thành phần đó, do đó DSU tạm thời cũng chứa một đỉnh có kích thước bằng một. Đầu ra là`1`. 

Đối với ranh giới bình đẳng,```
2 1
1 2 5
1
2 1 5
```cạnh cố định có trọng số chính xác bằng ngưỡng truy vấn. Con trỏ cạnh cố định sử dụng điều kiện`weight[e] >= required`, do đó cạnh được chèn vào DSU cơ sở. Do đó, cả hai hòn đảo đều thuộc về cùng một thành phần và đầu ra là`2`. 

Đối với một bản cập nhật giảm dần,```
2 1
1 2 5
3
2 1 5
1 1 1
2 1 2
```Cây cầu đặc biệt vì nó được cập nhật bên trong khối. Trước khi cập nhật, truy vấn đầu tiên ghi lại mặt nạ cạnh đặc biệt của nó ở trạng thái hoạt động. Bản cập nhật thay đổi trọng lượng của nó thành`1`. Truy vấn cuối cùng có ngưỡng`2`, vì vậy mặt nạ của nó không chứa các cạnh đặc biệt. Biểu đồ cơ sở trống và DSU tạm thời chỉ chứa hòn đảo bắt đầu. Các câu trả lời là`2`Và`1`. 

Đối với các cạnh song song,```
2 2
1 2 3
1 2 5
3
2 1 4
1 2 2
2 1 4
```cả hai cây cầu đều được lưu trữ với các chỉ số cạnh riêng biệt. Ban đầu chỉ có cầu cân`5`thỏa mãn ngưỡng`4`, vậy câu trả lời là`2`. Sau khi cây cầu đó được đổi thành`2`, không cây cầu nào có thể chịu được trọng lượng`4`, để yên hòn đảo xuất phát và sản xuất`1`. Đây là lý do tại sao việc triển khai lập chỉ mục các cầu nối đặc biệt theo ID cầu thay vì theo cặp điểm cuối. 

Đối với một khối chứa các cập nhật lặp lại cho cùng một cây cầu, tập đặc biệt chỉ chứa cây cầu đó một lần. Trong suốt thời gian trôi qua của nó`weight`giá trị được thay đổi mỗi khi có bản cập nhật xuất hiện và mỗi truy vấn loại 2 ghi lại giá trị hiện tại tại vị trí chính xác đó. Do đó, cùng một cây cầu có thể có các bit hoạt động khác nhau cho các truy vấn khác nhau trong một khối, trong khi DSU phụ vẫn chứa tối đa một nút cho cây cầu đó. 

Bất biến trung tâm tồn tại trong tất cả các trường hợp này: tại mỗi truy vấn được xử lý, DSU chính chứa chính xác các cầu nối cố định mà xe được truy vấn có thể sử dụng được và DSU tạm thời bổ sung chính xác các cầu nối đặc biệt có thể sử dụng được tại thời điểm đó. Do đó, kích thước thành phần thu được chính là số hòn đảo mà chiếc ô tô đó có thể tiếp cận được.
