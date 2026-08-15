---
title: "CF 102452C - Xây dựng trang trại"
description: "Mỗi cửa hàng là một đỉnh của cây và cửa hàng (i) bán chính xác một đoạn hàng rào có chiều dài (ai). Chọn hai cửa hàng (x) và (y) có nghĩa là lấy mọi đoạn trên con đường cây duy nhất giữa chúng."
date: "2026-08-12T08:37:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102452
codeforces_index: "C"
codeforces_contest_name: "2019-2020 ICPC Asia Hong Kong Regional Contest"
rating: 0
weight: 102452
solve_time_s: 220
verified: true
draft: false
---

[CF 102452C - Xây dựng trang trại](https://codeforces.com/problemset/problem/102452/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 40s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi cửa hàng là một đỉnh của cây và cửa hàng (i) bán chính xác một đoạn hàng rào có chiều dài (a_i). Chọn hai cửa hàng (x) và (y) có nghĩa là lấy mọi đoạn trên con đường cây duy nhất giữa chúng. Câu hỏi đặt ra là có bao nhiêu cặp (x<y) tạo ra một tập hợp các độ dài đoạn có thể sắp xếp thành một đa giác đơn giản không suy biến. 

Đối với bất kỳ đường dẫn cố định nào, chỉ độ dài các đoạn của nó là quan trọng. Giả sử tổng chiều dài của chúng là (S) và đoạn dài nhất có chiều dài (M). Một tập hợp các độ dài dương có thể tạo thành một đa giác đơn giản chính xác khi 

[ 
S > 2M. 
] 

Lý do là cạnh dài nhất phải ngắn hơn tổng các cạnh còn lại. Nếu sự bất đẳng thức đó đúng thì các đoạn có thể được sắp xếp thành một đa giác; nếu đẳng thức giữ nguyên thì chúng chỉ tạo thành cấu hình thẳng suy biến. 

Vì vậy, vấn đề đồ thị trở thành vấn đề truy vấn đường dẫn. Đối với mỗi cặp đỉnh, chúng ta cần biết tổng các giá trị trên đường đi của chúng và giá trị lớn nhất trên đường đi đó, sau đó kiểm tra xem tổng có lớn hơn hai lần giá trị tối đa hay không. 

Cây chứa tối đa (2\cdot10^5) đỉnh trong một trường hợp thử nghiệm và tổng số đỉnh của tất cả các trường hợp thử nghiệm nhiều nhất là (4\cdot10^5). Thuật toán (O(n^2)) sẽ kiểm tra khoảng (2\cdot10^{10}) cặp tại (n=2\cdot10^5), vượt xa giới hạn 5,5 giây. Chúng ta cần một cái gì đó gần với (O(n\log^2 n)), đó là độ phức tạp của giải pháp dự định. 

Có một số trường hợp ranh giới rất dễ bị xử lý sai. Chỉ với một đỉnh thì không có cặp nào cả. Ví dụ,```
1
1
5
```có câu trả lời`0`. Một đường đi chứa hai đỉnh cũng không thể tạo thành một đa giác, bất kể độ dài như thế nào, bởi vì một đoạn không thể ngắn hơn tổng của đoạn kia. 

Sự bình đẳng cũng phải bị từ chối. Coi như```
1
3
1 1 2
1 2
2 3
```Đường đi duy nhất chứa cả ba đỉnh có tổng chiều dài (4) và chiều dài tối đa (2). Vì (4=2\cdot2) nên nó suy biến nên đáp án là`0`. Một so sánh không nghiêm ngặt như (S\ge2M) sẽ tính sai. 

Đoạn tối đa không nhất thiết phải là trọng tâm khi chúng ta sử dụng phân tách trọng tâm. Ví dụ,```
1
3
2 1 2
1 2
2 3
```đưa ra đường dẫn (1\to2\to3) có độ dài (2,1,2). Tổng của nó là (5), trong khi gấp đôi mức tối đa của nó là (4), vì vậy câu trả lời là`1`. Bất kỳ giải pháp nào chỉ tính các đường dẫn có đoạn lớn nhất nằm ở tâm sẽ không thành công trong trường hợp này. 

Cuối cùng, độ dài bằng nhau mang lại khả năng kiểm tra độ chính xác hữu ích. Vì```
1
5
1 1 1 1 1
1 2
1 3
1 4
1 5
```mọi đường dẫn giữa hai lá chứa ba phân đoạn đơn vị và hợp lệ, trong khi mọi đường dẫn liên quan đến tâm chỉ chứa hai phân đoạn và không hợp lệ. Có (\binom42=6) cặp hợp lệ. 

## Phương pháp tiếp cận 

Giải pháp brute-force được rút ra trực tiếp từ định nghĩa. Đối với mỗi cặp đỉnh (x<y), hãy đi dọc theo đường cây duy nhất của chúng, tích lũy tổng và giá trị lớn nhất của nó. Sau đó kiểm tra xem tổng có lớn hơn gấp đôi mức tối đa hay không. Điều này đúng vì điều kiện của đa giác chỉ phụ thuộc chính xác vào hai đại lượng đó. 

Vấn đề là số lượng đường dẫn. Có các cặp điểm cuối (\binom n2), vốn đã là (O(n^2)) và việc đi dọc theo một con đường có thể tự mất (O(n)). Trên một chuỗi, điều này tạo ra 

[ 
\sum_{k=1}^{n-1} k(n-k)=\Theta(n^3) 
] 

hoạt động. Ngay cả khi chúng tôi xử lý trước thông tin đường dẫn đủ để tạo từng cặp (O(1)), chúng tôi vẫn sẽ có khoảng (2\cdot10^{10}) cặp cho (n=2\cdot10^5). 

Cấu trúc hữu ích là biểu đồ là một cái cây. Phân rã centroid cho phép chúng ta phân chia mọi đường đi theo trọng tâm đầu tiên mà nó đi qua. Tại một tâm (c), mọi đường đi qua (c) có thể được mô tả bằng cách sử dụng thông tin từ hai điểm cuối của nó so với (c). 

Đối với đỉnh (u) trong thành phần hiện tại, hãy xác định (s_u) là tổng các giá trị trên đường đi từ (c) đến (u), bao gồm (a_c) và xác định (m_u) là giá trị lớn nhất trên cùng đường dẫn đó. Nếu (u) và (v) nằm trong các thành phần khác nhau sau khi loại bỏ (c), đường đi đầy đủ của chúng sẽ đi từ (u) đến (c) rồi từ (c) đến (v). Do đó tổng và giá trị lớn nhất của nó là 

[ 
S=s_u+s_v-a_c 
] 

và 

[ 
M=\max(m_u,m_v). 
] 

Điều kiện hợp lệ trở thành 

[ 
s_u+s_v-a_c>2\max(m_u,m_v). 
] 

Đây là mức giảm trung tâm. Cây đã biến mất khỏi sự bất bình đẳng. Tại một trọng tâm cố định, mỗi điểm cuối chỉ được biểu thị bằng hai số (s_u) và (m_u). 

Chúng ta vẫn cần tránh đếm các cặp từ cùng một thành phần con của tâm, vì đường đi thực tế của chúng không đi qua tâm. Một cách thuận tiện để làm điều này là đếm tất cả các cặp trong số các đỉnh được thu thập bằng cách sử dụng công thức trên, sau đó đếm riêng các cặp hoàn toàn bên trong mỗi thành phần con và trừ chúng. Bản thân trọng tâm được giữ thành một nhóm riêng biệt, do đó các cặp liên quan đến trọng tâm vẫn được tính. 

Để đếm các cặp thỏa mãn bất đẳng thức một cách hiệu quả, hãy sắp xếp các đỉnh theo mức giảm dần (m). Khi xử lý (u), mọi đỉnh (v) được xử lý trước đó đều thỏa mãn (m_v\ge m_u), do đó giá trị lớn nhất là (m_v). Bất đẳng thức trở thành 

[ 
s_u+s_v-a_c>2m_v, 
] 

có thể được sắp xếp lại thành 

[ 
2m_v-s_v+a_c<s_u. 
] 

Đối với mỗi đỉnh được xử lý, hãy chèn khóa 

[ 
k_v=2m_v-s_v+a_c 
] 

vào một cây Fenwick. Đối với (u) hiện tại, chúng ta chỉ cần đếm các khóa được chèn nhỏ hơn (s_u). Việc nén tọa độ cho phép cây Fenwick xử lý các khóa số nguyên tùy ý này. 

Quy trình đếm tương tự có thể được áp dụng cho toàn bộ thành phần trung tâm và cho từng thành phần con riêng biệt. Sau đó, phân tách centroid xử lý đệ quy mọi thành phần con, do đó mỗi cặp không có thứ tự được tính chính xác một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n^3)) | (O(n)) | Quá chậm | 
| Brute Force với truy vấn đường dẫn (O(1)) | (O(n^2)) | (O(n^2)) trở lên | Quá chậm | 
| Phân hủy Centroid + Cây Fenwick | (O(n\log^2 n)) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đầu tiên hãy sử dụng bổ đề đa giác. Một đường dẫn hợp lệ chính xác khi tổng độ dài đoạn của nó lớn hơn gấp đôi độ dài đoạn tối đa trên đường dẫn đó. 
2. Xây dựng phân rã trung tâm của cây. Đối với thành phần hiện tại, hãy tìm trọng tâm (c) của nó. Mọi đường dẫn trong thành phần này đều đi qua (c) hoặc được chứa hoàn toàn trong một trong các thành phần thu được sau khi loại bỏ (c). 
3. Với mỗi đỉnh (u) có thể tới được từ (c) mà không vượt qua tâm bị chặn khác, hãy tính hai giá trị. Gọi (s_u) là tổng từ (c) đến (u), bao gồm (a_c) và gọi (m_u) là giá trị lớn nhất trên cùng đường đi đó. Ghi lại thành phần con nào của (c) chứa (u). 
4. Thêm chính tâm đó làm bản ghi đặc biệt với (s_c=a_c) và (m_c=a_c). Điều này cho phép các đường đi từ (c) đến một đỉnh khác được xử lý theo cùng một công thức giống như đường đi giữa hai thành phần con khác nhau. 
5. Đếm từng cặp trong số tất cả các bản ghi được thu thập bằng cách sử dụng điều kiện 

[ 
s_u+s_v-a_c>2\max(m_u,m_v). 
] 

Để thực hiện việc này một cách hiệu quả, hãy sắp xếp các bản ghi theo mức giảm (m). Đối với bản ghi hiện tại (u), tất cả các bản ghi trước đó (v) đều có (m_v\ge m_u). Điều kiện là lúc đó 

[ 
2m_v-s_v+a_c<s_u. 
] 

Lưu trữ (2m_v-s_v+a_c) cho các bản ghi trước đó trong cây Fenwick và truy vấn xem có bao nhiêu bản ghi nhỏ hơn (s_u). 

1. Số đếm ở bước trước cũng bao gồm các cặp có hai đỉnh thuộc cùng thành phần con của (c). Một cặp như vậy không thực sự đi qua (c), vì vậy việc tính toán nó qua (c) là không liên quan. Đối với mọi thành phần con, hãy chạy quy trình đếm cặp giống nhau trên các bản ghi của nó và trừ kết quả đó. 
2. Đánh dấu (c) là đã bị loại bỏ khỏi thành phần hiện tại. Giải quyết đệ quy mọi thành phần con còn lại. Mọi đường dẫn không được tính ở (c) đều được chứa hoàn toàn trong chính xác một trong các thành phần nhỏ hơn này, vì vậy nó sẽ được xử lý ở đó. 
3. Tính tổng phần đóng góp của trọng tâm hiện tại và tất cả các thành phần đệ quy. Số kết quả là câu trả lời cho trường hợp thử nghiệm. 

### Tại sao nó hoạt động 

Tại mỗi centroid (c), thuật toán đếm chính xác các đường dẫn hợp lệ có điểm cuối là chính (c) hoặc nằm trong hai thành phần khác nhau sau khi loại bỏ (c). Đối với một đường dẫn như vậy, (s_u+s_v-a_c) chính xác là tổng chiều dài của nó và (\max(m_u,m_v)) chính xác là đoạn lớn nhất của nó, vì vậy điều kiện Fenwick tương đương với điều kiện đa giác. 

Các cặp nằm bên trong một thành phần con sẽ bị xóa khỏi phần đóng góp hiện tại và được chuyển sang lệnh gọi đệ quy cho thành phần đó. Do đó, mọi đường dẫn đều được xử lý ở trọng tâm cao nhất nơi các điểm cuối của nó trở nên tách biệt hoặc nơi một điểm cuối chính là trọng tâm. Không có đường dẫn nào có thể được tính ở hai mức phân tách khác nhau và không có đường dẫn nào có thể bị bỏ qua. 

Đối số sắp xếp cũng chính xác. Khi các đỉnh được xử lý giảm dần (m), đỉnh trước đó luôn có đường đi lớn nhất hoặc bằng cực đại. Điều này cho phép chúng tôi thay thế giá trị tối đa của hai giá trị bằng (m_v) đã được xử lý, chuyển đổi bất đẳng thức tối đa hai biến thành một so sánh ngưỡng duy nhất phù hợp với cây Fenwick. 

## Giải pháp Python```python
import sys
from bisect import bisect_left

input = sys.stdin.readline

class Fenwick:
    __slots__ = ("n", "bit")

    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 1)

    def add(self, i, value=1):
        n = self.n
        bit = self.bit
        while i <= n:
            bit[i] += value
            i += i & -i

    def sum(self, i):
        bit = self.bit
        res = 0
        while i:
            res += bit[i]
            i -= i & -i
        return res

def count_pairs(items, ac):
    """
    Count unordered pairs (u, v) inside items satisfying

        s_u + s_v - ac > 2 * max(m_u, m_v)

    where each item is (s, m).
    """
    k = len(items)
    if k < 2:
        return 0

    items.sort(key=lambda x: x[1], reverse=True)

    keys = [2 * m - s + ac for s, m in items]
    coords = sorted(set(keys))

    fw = Fenwick(len(coords))
    ans = 0

    for s, m in items:
        # Need key < s, hence bisect_left rather than bisect_right.
        pos = bisect_left(coords, s)
        ans += fw.sum(pos)

        key = 2 * m - s + ac
        idx = bisect_left(coords, key) + 1
        fw.add(idx)

    return ans

def solve(data):
    it = iter(map(int, data.split()))
    t = next(it)
    outputs = []

    for _ in range(t):
        n = next(it)
        a = [next(it) for _ in range(n)]

        graph = [[] for _ in range(n)]
        for _ in range(n - 1):
            u = next(it) - 1
            v = next(it) - 1
            graph[u].append(v)
            graph[v].append(u)

        blocked = [False] * n
        parent = [-1] * n
        size = [0] * n

        def find_centroid(start):
            order = [start]
            parent[start] = -1

            for u in order:
                pu = parent[u]
                for v in graph[u]:
                    if blocked[v] or v == pu:
                        continue
                    parent[v] = u
                    order.append(v)

            for u in reversed(order):
                size[u] = 1
                for v in graph[u]:
                    if blocked[v] or parent[v] != u:
                        continue
                    size[u] += size[v]

            total = len(order)
            centroid = start
            best = total + 1

            for u in order:
                largest = total - size[u]
                for v in graph[u]:
                    if blocked[v] or parent[v] != u:
                        continue
                    if size[v] > largest:
                        largest = size[v]

                if largest < best:
                    best = largest
                    centroid = u

            return centroid

        answer = 0

        def decompose(start):
            nonlocal answer

            c = find_centroid(start)
            ac = a[c]

            all_items = [(ac, ac)]

            branch_items = []

            for first in graph[c]:
                if blocked[first]:
                    continue

                current = []
                stack = [(first, c, ac + a[first], max(ac, a[first]))]

                while stack:
                    u, p, s, m = stack.pop()
                    current.append((s, m))
                    all_items.append((s, m))

                    for v in graph[u]:
                        if blocked[v] or v == p or v == c:
                            continue
                        stack.append(
                            (v, u, s + a[v], max(m, a[v]))
                        )

                branch_items.append(current)

            # Count all pairs whose path is considered through c.
            answer += count_pairs(all_items, ac)

            # Remove pairs whose endpoints are in the same branch.
            for items in branch_items:
                answer -= count_pairs(items, ac)

            blocked[c] = True

            for v in graph[c]:
                if not blocked[v]:
                    decompose(v)

        if n > 0:
            decompose(0)

        outputs.append(str(answer))

    return "\n".join(outputs)

def main():
    data = sys.stdin.buffer.read().decode()
    sys.stdout.write(solve(data))

if __name__ == "__main__":
    main()
```Cây Fenwick là một cấu trúc đếm tiền tố tiêu chuẩn. Sau khi nén tọa độ,`add`chèn một phím và`sum(pos)`đếm tất cả các phím được chèn có vị trí nén tối đa`pos`. 

biểu thức`bisect_left(coords, s)`là cố tình nghiêm ngặt. Bất đẳng thức mong muốn là (2m_v-s_v+a_c<s_u), không nhỏ hơn hoặc bằng (s_u). Nếu hai cạnh bằng nhau thì đa giác bị suy biến và không được tính. 

Bản thân trung tâm được đại diện bởi`(ac, ac)`. Nếu (u) là một đỉnh khác thì cặp`(c,u)`có tổng (a_c+s_u-a_c=s_u) và giá trị tối đa của nó là (m_u), do đó công thức chung xử lý nó mà không cần trường hợp đặc biệt. 

Phép trừ trên mỗi nhánh là điều ngăn cản việc tính các đường dẫn nằm hoàn toàn bên trong một nhánh tại tâm hiện tại. Những đường dẫn đó vẫn còn hiện diện trong phân rã đệ quy, trong đó hình học thực tế của chúng được biểu diễn chính xác. 

Số nguyên Python có độ chính xác tùy ý, do đó tổng có thể đạt tới (2\cdot10^{14}) một cách an toàn mà không bị tràn. Việc triển khai cũng sử dụng các phép duyệt lặp để khám phá thành phần, tránh các vấn đề về độ sâu đệ quy Python trên chuỗi. Sự đệ quy của`decompose`nó có độ sâu logarit vì mỗi thành phần đệ quy có nhiều nhất một nửa số đỉnh của thành phần gốc của nó. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Mẫu đầu tiên là```
3
1 10 100
1 2
3 2
```Cây là một chuỗi. Chỉ có ba cặp điểm cuối có thể có và mỗi đường dẫn chứa tối đa hai đoạn. 

| Cặp | Độ dài đường đi | Tổng hợp | Tối đa | Tình trạng | hợp lệ | 
| --- | --- | --- | --- | --- | --- | 
| (1, 2) | 1, 10 | 11 | 10 | (11>20) | Không | 
| (1, 3) | 1, 10, 100 | 111 | 100 | (111>200) | Không | 
| (2, 3) | 10, 100 | 110 | 100 | (110>200) | Không | 

Câu trả lời là`0`. Ví dụ này cũng giải thích tại sao hai đoạn không bao giờ có thể tạo thành một đa giác hợp lệ và tại sao một đoạn rất lớn ngay lập tức khiến đường dẫn không hợp lệ. 

### Mẫu 2 

Mẫu thứ hai là```
5
1 1 1 1 1
1 2
1 3
1 4
1 5
```Đỉnh 1 là tâm và 4 đỉnh còn lại là các lá. Mỗi đường dẫn từ lá này sang lá khác có ba đoạn đơn vị. 

Tại trọng tâm thứ nhất, (a_c=1). Bản ghi trung tâm là`(1,1)`, và mỗi lá có`(2,1)`bởi vì đường đi của nó từ tâm chứa hai giá trị đơn vị. 

| Loại điểm cuối | (các) | (m) | Số | 
| --- | --- | --- | --- | 
| Trung tâm | 1 | 1 | 1 | 
| Lá | 2 | 1 | 4 | 

Đối với hai lá, 

[ 
s_u+s_v-a_c=2+2-1=3, 
] 

trong khi 

[ 
2\max(m_u,m_v)=2. 
] 

Vì vậy mỗi cặp lá đều có giá trị. 

| Loại cặp | Số cặp | hợp lệ | 
| --- | --- | --- | 
| Trung tâm và lá | 4 | Không | 
| Hai chiếc lá khác nhau | (\binom42=6) | Có | 

Sự đóng góp trung tâm hiện tại là`6`. Mỗi nhánh chỉ chứa một đỉnh nên không có cặp nhánh nào giống nhau để trừ. Tất cả các thành phần đệ quy là các đỉnh đơn và không đóng góp gì. 

Câu trả lời cuối cùng là`6`, phù hợp với mẫu 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n\log^2 n)) | Mỗi cấp độ trung tâm xử lý các đỉnh (O(n)), sắp xếp và chi phí hoạt động Fenwick (O(n\log n)) và có các cấp độ (O(\log n)) | 
| Không gian | (O(n)) | Cây, trạng thái phân rã centroid, bộ đệm truyền tải và mảng đếm tạm thời đều là tuyến tính | 

Sự phân rã trọng tâm làm giảm khoảng một nửa mỗi thành phần, do đó một đỉnh tham gia vào các mức (O(\log n)). Ở mỗi cấp độ, các thao tác sắp xếp và Fenwick yêu cầu tổng công việc (O(n\log n)) ở cấp độ đó. Với tổng số (n\le2\cdot10^5) cho mỗi trường hợp và (4\cdot10^5), điều này phù hợp với giải pháp (O(n\log^2 n)) dự định. Bài xã luận chính thức cũng đưa ra sự phức tạp tương tự. 

## Trường hợp thử nghiệm 

Các thử nghiệm sau đây giả định giải pháp đã gửi được lưu dưới dạng`solution.py`và phơi bày`solve(data)`chức năng hiển thị ở trên.```
from solution import solve

def run(inp: str) -> str:
    return solve(inp).strip()

# Provided sample
sample = """\
2
3
1 10 100
1 2
3 2
5
1 1 1 1 1
1 2
1 3
1 4
1 5
"""
assert run(sample) == "0\n6", "provided samples"

# Minimum-size input
assert run("""\
1
1
7
""") == "0", "one vertex has no pair"

# Two vertices can never form a polygon
assert run("""\
1
2
1 100
1 2
""") == "0", "two segments are impossible"

# Equality case: 1 + 1 = 2, so the polygon is degenerate
assert run("""\
1
3
1 1 2
1 2
2 3
""") == "0", "strict polygon inequality"

# A maximum is away from the centroid.
# Path 1-2-3 has lengths 2, 1, 2 and is valid.
assert run("""\
1
3
2 1 2
1 2
2 3
""") == "1", "maximum need not be the centroid"

# All equal values on a chain.
# Every path with at least 3 vertices is valid.
n = 200000
values = " ".join(["1"] * n)
edges = "\n".join(f"{i} {i + 1}" for i in range(1, n))
expected = (n - 1) * (n - 2) // 2

large_input = f"""\
1
{n}
{values}
{edges}
"""
assert run(large_input) == str(expected), "large all-equal chain"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 / 7`|`0`| Ranh giới kích thước tối thiểu | 
|`1 / 2 / 1 100 / 1 2`|`0`| Hai đoạn không thể tạo thành đa giác | 
|`1 / 3 / 1 1 2 / 1-2-3`|`0`| Ranh giới bất bình đẳng và bình đẳng nghiêm ngặt | 
|`1 / 3 / 2 1 2 / 1-2-3`|`1`| Tối đa có thể cách xa tâm | 
| Chuỗi 200000 đỉnh đơn vị |`19999700001`| Đầu vào có kích thước tối đa, giá trị bằng nhau, hiệu suất, câu trả lời lớn | 

## Vỏ cạnh 

Trường hợp một đỉnh được xử lý trước khi bất kỳ việc đếm cặp có ý nghĩa nào có thể xảy ra. Đối với đầu vào```
1
1
7
```trọng tâm duy nhất là đỉnh 1, và`all_items`chỉ chứa`(7,7)`. Quy trình Fenwick ngay lập tức trả về 0 vì có ít hơn hai bản ghi. Phân tách đệ quy không còn thành phần nào nên đáp án cuối cùng là`0`. 

Đối với hai đỉnh, thuật toán cũng trả về 0. Coi như```
1
2
1 100
1 2
```Sự phân rã trọng tâm có thể chọn một trong hai đỉnh làm trọng tâm. Đường dẫn duy nhất chứa độ dài (1) và (100), tính tổng (101) và tối đa (100). Bất đẳng thức cần tìm là (101>200), sai. Phép so sánh Fenwick cũng bác bỏ nó vì ngưỡng tương ứng không hoàn toàn thấp hơn tổng hiện tại. 

Ranh giới bình đẳng được xử lý bằng cách sử dụng`bisect_left`. Vì```
1
3
1 1 2
1 2
2 3
```toàn bộ đường đi có (s=4) và (m=2), do đó (s=2m). Trong bất đẳng thức được chuyển đổi, khóa liên quan chính xác bằng (các) hiện tại.`bisect_left`đặt truy vấn trước các khóa bằng nhau, do đó cặp đó không được tính. Đây chính xác là sự bất bình đẳng nghiêm ngặt cần thiết. 

Giá trị tối đa bên ngoài trọng tâm sẽ được xử lý vì thuật toán không cho rằng trọng tâm là giá trị lớn nhất. Vì```
1
3
2 1 2
1 2
2 3
```lấy đỉnh 2 làm trọng tâm. Mỗi lá có (s=3) và (m=2), trong khi tâm có (s=m=1). Đối với hai lá, 

[ 
3+3-1=5>2\cdot2=4. 
] 

Cặp này được tính ngay cả khi giá trị của centroid nhỏ hơn cực đại của cả hai điểm cuối. Đây chính xác là lý do tại sao thuật toán lưu trữ cả (s_u) và (m_u), thay vì chỉ lưu trữ tổng từ tâm. 

Đối với chuỗi có kích thước tối đa hoàn toàn bằng nhau, mọi đường dẫn chứa ít nhất ba đỉnh đều hợp lệ. Một đường đi gồm hai đỉnh có tổng (2) và cực đại (1), do đó nó đã hợp lệ theo bất đẳng thức số, nhưng đường đi như vậy vẫn không thể tạo thành một đa giác. Vấn đề rõ ràng này được giải quyết bằng bổ đề đa giác vì đối với hai đoạn, đoạn dài nhất ít nhất luôn bằng đoạn kia, khiến (S>2M) không thể xảy ra. Đối với các giá trị đơn vị, đường dẫn hai đỉnh có (S=2) và (2M=2), do đó nó hoàn toàn không thành công. Mọi đường đi có ba đỉnh trở lên đều thành công. 

Do đó, trên một chuỗi gồm (n=200000) đỉnh đơn vị, các đường đi hợp lệ chính xác là những đường có ít nhất ba đỉnh. có 

[ 
\binom n2-(n-1) 
=\frac{(n-1)(n-2)}2 
=19999700001 
] 

trong số chúng, đó là giá trị được kiểm tra bằng phép thử lớn. 

Lý do tương tự giải thích tại sao mẫu ngôi sao lại có câu trả lời thứ sáu. Đường dẫn từ tâm đến lá chứa hai phân đoạn đơn vị và thất bại ở mức bằng nhau, trong khi mọi đường dẫn từ lá đến lá chứa ba phân đoạn đơn vị và thành công. Phép trừ cấp trung tâm không loại bỏ cặp lá hợp lệ vì mỗi lá nằm trong một nhánh khác nhau.
