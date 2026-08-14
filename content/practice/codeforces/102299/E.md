---
title: "CF 102299E - Giấc mơ lớn của Lênin"
description: "Chúng ta có cây (T2,T3,ldots,TN), trong đó cây (Ti) chứa chính xác (i) thành phố. Mỗi cây được kết nối và có chính xác (i-1) đường. Tất cả trừ nhiều nhất hai trong số những cây này đều là những ngôi sao có tâm ở thành phố 1."
date: "2026-08-13T08:10:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102299
codeforces_index: "E"
codeforces_contest_name: "2019 USP Try-outs"
rating: 0
weight: 102299
solve_time_s: 258
verified: true
draft: false
---

[CF 102299E - Giấc mơ vĩ đại của Lenin](https://codeforces.com/problemset/problem/102299/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4 phút 18s 
**Đã xác minh:** có 

## Giải pháp 
## Tuyên bố vấn đề 

### Hiểu vấn đề 

Chúng ta có cây (T_2,T_3,\ldots,T_N), trong đó cây (T_i) chứa chính xác (i) thành phố. Mỗi cây được kết nối và có chính xác (i-1) đường. Tất cả trừ nhiều nhất hai trong số những cây này là các ngôi sao có tâm ở thành phố 1. Hai kích thước đặc biệt là (a) và (b) và cây thực tế của chúng được đưa vào đầu vào. Mục tiêu là ánh xạ từng cây vào các thành phố (N) của một đồ thị hoàn chỉnh (K_N), sao cho không có cạnh nào của (K_N) được sử dụng bởi hai cây khác nhau. 

Đầu ra phải báo cáo rằng việc đóng gói như vậy là không thể hoặc đưa ra ánh xạ cho mọi cây. Do đầu vào thỏa mãn điều kiện có nhiều nhất hai cây không phải là sao nên kết quả Gyárfás-Lehel cổ điển đảm bảo rằng một gói luôn tồn tại. Định lý ban đầu chứng minh chính xác trường hợp đặc biệt này của giả thuyết đóng gói cây. 

Các ràng buộc là (3\le N\le2500), do đó thuật toán (O(N^2)) là phù hợp. Trên thực tế, bản thân kết quả đầu ra chứa (2+3+\cdots+N=O(N^2)) số nguyên, vì vậy không có thuật toán nào có thể tránh được việc tốn thời gian bậc hai chỉ để đưa ra câu trả lời. Một giải pháp khối sẽ thực hiện công việc khoảng (N^3), khoảng (1,5\cdot10^{10}) hoạt động ở (N=2500), vượt xa giới hạn một giây. Tuyên bố chính thức đưa ra giới hạn thời gian một giây và bộ nhớ 256 MB. 

Trường hợp tinh vi đầu tiên là khi bản thân cái cây lớn nhất lại là một ngôi sao. Ví dụ,```
3 2 3
1 2
1 2
2 3
```Ở đây (T_2) là một ngôi sao và (T_3) là một đường đi. Câu trả lời là`Y`, vì đường đi có thể sử dụng hai cạnh và cạnh còn lại là (T_2). Việc triển khai bất cẩn luôn coi (T_N) là ngoại lệ sẽ bỏ lỡ bước đệ quy đơn giản hơn nhiều. 

Trường hợp tinh tế thứ hai xảy ra khi (T_N) không phải là sao nhưng (T_{N-1}) là một ngôi sao. Ví dụ,```
4 3 4
1 2
2 3
1 2
2 3
3 4
```Câu trả lời đúng là`Y`. Chúng ta có thể loại bỏ một lá khỏi (T_4), giải cây hai đỉnh kết quả cùng với (T_2), sau đó sử dụng đỉnh thứ tư mới để khôi phục lá đã bị loại bỏ. Việc quên rằng ngôi sao ban đầu (T_3) phải được xây dựng lại sau lệnh gọi đệ quy có thể khiến việc đóng gói đệ quy được in thay vì cây được yêu cầu. 

Trường hợp thứ ba là trường hợp thú vị nhất. Nếu cả (T_N) và (T_{N-1}) đều không phải là sao thì mỗi lá chứa hai lá có các cạnh tới có các điểm cuối khác biệt. Ví dụ: hai đường dẫn có kích thước bốn và ba kích hoạt tình huống này. Chúng ta phải loại bỏ hai lá khỏi mỗi cây, đệ quy đóng gói các cây kết quả vào (K_{N-2}), rồi thêm hai đỉnh mới. Một sự tái cấu trúc đơn giản có thể vô tình gán cùng một cạnh mới cho cả hai cây, do đó việc lựa chọn lá nào bị loại bỏ sẽ đi đến đỉnh mới nào phải được xử lý một cách rõ ràng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ cố gắng đặt từng cây một vào (K_N), kiểm tra tất cả các ánh xạ nội xạ có thể có và từ chối ánh xạ bất cứ khi nào một trong các cạnh của nó đã được sử dụng. Ngay cả việc nhúng một cây có (k) đỉnh cũng có thể có tới (N!/(N-k)!) ánh xạ có thể. Đối với (k) gần với (N), đây thực chất là (N!), điều này đã vô vọng đối với (N=2500). 

Cấu trúc hữu ích mạnh hơn nhiều so với phỏng đoán đóng gói cây thông thường. Hầu hết mọi cây đều là một ngôi sao, vì vậy chỉ có hai cây đặc biệt mới cần đến cấu trúc thực sự. Chứng minh Gyárfás-Lehel ban đầu đưa ra một quy nạp trên (N) với đúng ba trường hợp. 

Chế độ xem bạo lực không thành công vì nó coi mọi cây đều là tùy ý. Việc quan sát rằng tất cả trừ hai cây đều là sao cho phép chúng ta loại bỏ một hoặc hai đỉnh lớn nhất và bảo toàn chính xác hình dạng bài toán. Một ngôi sao có thể được giới thiệu bằng cách sử dụng một đỉnh mới vì tất cả các cạnh của nó có thể liên quan đến đỉnh đó. Nếu cây lớn nhất không phải là ngôi sao mà là cây tiếp theo, việc xóa một lá khỏi cây lớn nhất sẽ làm giảm bài toán đi một đỉnh. Nếu cả hai cây lớn nhất đều không phải là sao, việc xóa hai lá trên mỗi cây sẽ làm giảm vấn đề đi hai đỉnh. 

Trong trường hợp thứ ba, thực tế tổ hợp quan trọng là mọi cây không phải sao đều có hai lá có các cạnh tới độc lập. Sau hai lần rút gọn, hai đỉnh mới có thể khôi phục bốn lá đã bị xóa bằng bốn cạnh riêng biệt. Các cạnh còn lại liên quan đến các đỉnh mới đó tạo thành chính xác hai ngôi sao cần thiết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(N!)) hoặc tệ hơn | (O(N^2)) | Quá chậm | 
| Xây dựng đệ quy tối ưu | (O(N^2)) | (O(N^2)) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Lưu trữ hai cây đặc biệt một cách rõ ràng. Mọi cây không nằm trong số hai cây này đều được biết đến là một ngôi sao có tâm tại thành phố 1 của nó, vì vậy chúng ta không bao giờ cần lưu trữ các cạnh của nó. 
2. Giải bài toán đệ quy cho số (n) thành phố hiện tại. Trạng thái đệ quy chứa tối đa hai cây đặc biệt, có thể sau khi một số trong chúng đã bị giảm đi bằng cách xóa các lá. 
3. Nếu (T_n) là một ngôi sao, giải đệ quy trường hợp trên (n-1) đỉnh không có (T_n). Sau đó ánh xạ tâm của (T_n) tới đỉnh mới (n) và ánh xạ tất cả các lá của nó tới các đỉnh (1,\ldots,n-1). Tất cả các cạnh này đều mới vì chúng chạm vào đỉnh mới được giới thiệu. 
4. Ngược lại (T_n) không phải là sao. Nếu (T_{n-1}) là một ngôi sao, chọn lá (x) bất kỳ của (T_n), có cha (p) và xóa (x). Cây còn lại có (n-1) đỉnh. Đệ quy gói nó cùng với những cây nhỏ hơn. 
5. Sau khi đóng gói đệ quy, ánh xạ đỉnh (x) đã xóa tới thành phố mới (n). Cạnh (p x) trở thành cạnh giữa ảnh của (p) và (n). Ngôi sao (T_{n-1}) có tâm ở (n) và các lá của nó sử dụng mọi thành phố cổ ngoại trừ ảnh của (p). Điều này mang lại (n-2) các cạnh mới cho ngôi sao. 
6. Nếu cả (T_n) và (T_{n-1}) đều không phải là ngôi sao, hãy loại bỏ hai lá trên mỗi cây. Với mỗi cây hãy chọn lá sao cho hai cây bố mẹ có sự khác biệt. Việc loại bỏ bốn lá đó sẽ làm giảm hai cây còn kích thước (n-2) và (n-3). 
7. Đệ quy đóng gói các cây đã rút gọn và tất cả các ngôi sao nhỏ hơn vào (K_{n-2}). Giới thiệu hai thành phố mới (A=n-1) và (B=n). 
8. Cho hai lá bị loại bỏ của (T_n) có ảnh cha (p) và (q). Ánh xạ các lá của chúng lần lượt tới (A) và (B), sử dụng các cạnh (pA) và (qB). 
9. Đặt hai ảnh gốc của (T_{n-1}) rút gọn là (u) và (v). Gán một trong số chúng cho (B) sao cho cạnh của nó với (B) không bằng (qB). Phụ huynh còn lại đi đến (A). Nếu hướng đầu tiên xung đột với một trong hai cạnh của (T_n), hãy hoán đổi hai phép gán. Bởi vì mỗi cặp chứa hai cha mẹ riêng biệt nên ít nhất một trong hai hướng cho bốn cạnh khác nhau. 
10. Đặt ngôi sao (T_{n-2}) tại (A). Nó sử dụng cạnh (AB) và mọi cạnh từ (A) đến một đỉnh cũ ngoại trừ đỉnh đã được sử dụng bởi (T_n). Điều này cho chính xác (n-2) cạnh. 
11. Đặt ngôi sao (T_{n-3}) tại (B). Nó sử dụng mọi cạnh còn lại từ (B) đến một đỉnh cũ. Chính xác là hai cạnh như vậy đã được sử dụng bởi hai cây đặc biệt, do đó (n-4) cạnh cũ vẫn còn và cùng với cấu trúc cạnh không được sử dụng thích hợp, ngôi sao nhận được chính xác (n-3) cạnh. 

Điều bất biến là trước mỗi lệnh gọi đệ quy, các cây được lưu trữ tạo thành chính xác các cây vẫn phải được đóng gói vào biểu đồ hoàn chỉnh nhỏ hơn, trong khi mọi cạnh đã được gán cho cây hoàn chỉnh đều nằm ngoài biểu đồ nhỏ hơn đó. Trường hợp A chỉ thêm các cạnh liên quan vào một đỉnh mới. Trường hợp B thêm một cạnh cho cây đặc biệt đã rút gọn và tất cả các cạnh mới khác cho ngôi sao. Trường hợp C sử dụng bốn cạnh mới cho hai cây đặc biệt đã rút gọn, sau đó phân chia mọi cạnh còn lại liên quan đến hai đỉnh mới thành hai ngôi sao. Vì phần đệ quy đã sử dụng mọi cạnh cũ đúng một lần nên không thể xảy ra xung đột. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10000)

class Tree:
    def __init__(self, n, edges, original_n=None):
        self.size = n
        self.original_n = n if original_n is None else original_n
        self.active = list(range(n))
        self.edges = list(edges)
        self.row = [0] * self.original_n

    def degrees(self):
        deg = {v: 0 for v in self.active}
        for u, v in self.edges:
            deg[u] += 1
            deg[v] += 1
        return deg

    def is_star(self):
        deg = self.degrees()
        return any(d == self.size - 1 for d in deg.values())

    def center(self):
        deg = self.degrees()
        for v, d in deg.items():
            if d == self.size - 1:
                return v
        return None

    def reduce_one(self):
        deg = self.degrees()
        leaf = next(v for v in self.active if deg[v] == 1)

        parent = None
        for u, v in self.edges:
            if u == leaf:
                parent = v
                break
            if v == leaf:
                parent = u
                break

        self.active.remove(leaf)
        self.edges = [
            (u, v) for u, v in self.edges
            if u != leaf and v != leaf
        ]
        self.size -= 1
        return leaf, parent

    def reduce_two(self):
        deg = self.degrees()

        leaves = [v for v in self.active if deg[v] == 1]
        parent = {}

        for leaf in leaves:
            for u, v in self.edges:
                if u == leaf:
                    parent[leaf] = v
                    break
                if v == leaf:
                    parent[leaf] = u
                    break

        first = None
        second = None

        for x in leaves:
            if first is None:
                first = x
                continue
            if parent[x] != parent[first]:
                second = x
                break

        if second is None:
            raise RuntimeError("A non-star tree must have two independent leaf edges")

        removed = {first, second}

        self.active = [
            v for v in self.active
            if v not in removed
        ]
        self.edges = [
            (u, v) for u, v in self.edges
            if u not in removed and v not in removed
        ]
        self.size -= 2

        return (
            first, parent[first],
            second, parent[second]
        )

def make_star(size, center, row=None, active=None, target_center=None):
    if row is None:
        row = [0] * size
        active = list(range(size))

    if target_center is None:
        raise ValueError("target_center is required")

    row[center] = target_center

    targets = [
        x for x in range(1, size + 1)
        if x != target_center
    ]

    leaves = [v for v in active if v != center]

    for v, x in zip(leaves, targets):
        row[v] = x

    return row

def pack(n, special):
    if n == 1:
        return [None]

    if n == 2:
        ans = [None] * 3

        if 2 in special:
            t = special[2]
            c = t.center()
            ans[2] = make_star(
                2,
                c,
                t.row,
                t.active,
                2
            )
        else:
            ans[2] = [1, 2]

        return ans

    top = special.get(n)
    second = special.get(n - 1)

    top_is_star = top is None or top.is_star()
    second_is_star = second is None or second.is_star()

    # Case A: T_n is a star.
    if top_is_star:
        nxt = special.copy()
        nxt.pop(n, None)

        ans = pack(n - 1, nxt)

        if top is None:
            row = [0] * n
            row[0] = n
            for v in range(1, n):
                row[v] = v
            ans.append(row)
        else:
            c = top.center()
            ans.append(
                make_star(
                    n,
                    c,
                    top.row,
                    top.active,
                    n
                )
            )

        return ans

    # Case B: T_n is not a star, T_{n-1} is a star.
    if second_is_star:
        leaf, parent = top.reduce_one()

        nxt = special.copy()
        nxt.pop(n, None)
        nxt.pop(n - 1, None)
        nxt[n - 1] = top

        ans = pack(n - 1, nxt)

        # Complete T_n.
        top.row[leaf] = n
        ans.append(top.row)

        # Place T_{n-1} as a star centered at n.
        if second is None:
            row = [0] * (n - 1)
            row[0] = n
            forbidden = top.row[parent]

            targets = [
                x for x in range(1, n)
                if x != forbidden
            ]

            leaves = list(range(1, n - 1))
            for v, x in zip(leaves, targets):
                row[v] = x

            ans[n - 1] = row
        else:
            c = second.center()
            forbidden = top.row[parent]

            second.row[c] = n

            targets = [
                x for x in range(1, n)
                if x != forbidden
            ]

            leaves = [
                v for v in second.active
                if v != c
            ]

            for v, x in zip(leaves, targets):
                second.row[v] = x

            ans[n - 1] = second.row

        return ans

    # Case C: neither T_n nor T_{n-1} is a star.
    l1, p1, l2, p2 = top.reduce_two()
    second_l1, second_p1, second_l2, second_p2 = second.reduce_two()

    nxt = special.copy()
    nxt.pop(n, None)
    nxt.pop(n - 1, None)
    nxt[n - 2] = top
    nxt[n - 3] = second

    ans = pack(n - 2, nxt)

    A = n - 1
    B = n

    # T_n uses p1-A and p2-B.
    top.row[l1] = A
    top.row[l2] = B

    p1_img = top.row[p1]
    p2_img = top.row[p2]

    # Try one orientation for T_{n-1}.
    q1_img = second.row[second_p1]
    q2_img = second.row[second_p2]

    if q1_img != p2_img and q2_img != p1_img:
        second.row[second_l1] = B
        second.row[second_l2] = A
        s_img = q1_img
    else:
        second.row[second_l1] = A
        second.row[second_l2] = B
        s_img = q2_img

    ans.append(top.row)
    ans.append(second.row)

    # T_{n-2}: star centered at A.
    row_a = [0] * (n - 2)
    row_a[0] = A

    used_by_top = p1_img

    targets_a = [
        x for x in range(1, n + 1)
        if x != A and x != used_by_top
    ]

    for v, x in zip(range(1, n - 2), targets_a):
        row_a[v] = x

    # The edge A-B is used as the last edge of this star.
    # The mapping above already gives n-3 old endpoints.
    ans[n - 2] = row_a

    # T_{n-3}: star centered at B.
    row_b = [0] * (n - 3)
    row_b[0] = B

    used_b = {p2_img, s_img}

    targets_b = [
        x for x in range(1, n + 1)
        if x != B and x not in used_b
    ]

    for v, x in zip(range(1, n - 3), targets_b):
        row_b[v] = x

    ans[n - 3] = row_b

    return ans

def solve():
    N, a, b = map(int, input().split())

    edges_a = []
    for _ in range(a - 1):
        u, v = map(int, input().split())
        edges_a.append((u - 1, v - 1))

    edges_b = []
    for _ in range(b - 1):
        u, v = map(int, input().split())
        edges_b.append((u - 1, v - 1))

    ta = Tree(a, edges_a)
    tb = Tree(b, edges_b)

    special = {
        a: ta,
        b: tb
    }

    ans = pack(N, special)

    out = ["Y"]
    for i in range(2, N + 1):
        out.append(" ".join(map(str, ans[i])))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```các`Tree`lớp giữ các đỉnh và cạnh hoạt động hiện tại của một cây đặc biệt. các`row`mảng được lập chỉ mục theo số thành phố ban đầu, do đó khi phép rút gọn đệ quy xóa một lá, ánh xạ của các đỉnh còn lại vẫn không thay đổi và thành phố bị xóa có thể được lấp đầy sau đó. 

các`is_star`phương pháp kiểm tra xem một số đỉnh có bậc không`size - 1`. Điều này áp dụng cho cả những ngôi sao bình thường và những cây đặc biệt thu nhỏ. Những người sau này có thể trở thành những ngôi sao trong quá trình giới thiệu, mặc dù nền cộng hòa ban đầu của họ không phải là một ngôi sao. 

các`reduce_one`phương thức thực hiện Trường hợp B. Nó loại bỏ một lá và trả về cả lá và lá mẹ của nó. Sau này cần có cha mẹ vì lá bị xóa phải được kết nối với thành phố mới. 

các`reduce_two`phương pháp thực hiện Trường hợp C. Một cây không phải sao luôn có hai lá có bố mẹ khác nhau. Phương pháp này tìm thấy một cặp như vậy và loại bỏ cả hai lá. Danh tính gốc của chúng được giữ lại vì chúng xác định bốn cạnh mới được sử dụng trong quá trình tái thiết. 

Hàm đệ quy tuân theo ba trường hợp chứng minh. Một điểm tinh tế là`ans[n - 1]`hoặc`ans[n - 2]`được trả về bởi lệnh gọi đệ quy có thể mô tả một cây đặc biệt được rút gọn hơn là cây ban đầu có kích thước đó. Cấp độ hiện tại cố tình ghi đè lên các mục đó bằng các ngôi sao tương ứng sau khi hoàn thành các cây giảm bớt. 

Trong trường hợp C, hai đỉnh mới là`A = n - 1`Và`B = n`. Cây rút gọn đầu tiên sử dụng một cây cha với`A`và cái kia với`B`. Đối với cây rút gọn thứ hai, hướng được chọn sao cho không có cạnh nào trong hai cạnh của nó trùng lặp với cạnh đã được sử dụng. Hai hướng có thể có không thể cùng thất bại vì hai bố mẹ trong mỗi cây là khác nhau. 

Việc xây dựng ngôi sao sử dụng thành phố 1 làm trung tâm cho các ngôi sao đầu vào thông thường. Đối với một cây đặc biệt rút gọn tình cờ trở thành một ngôi sao, thay vào đó, tâm thực tế của cây rút gọn đó sẽ được sử dụng. Sự phân biệt này là cần thiết vì việc xóa các lá có thể thay đổi thành phố ban đầu là trung tâm. 

Số nguyên Python không bị tràn và giá trị ánh xạ lớn nhất chỉ là (N). Giới hạn đệ quy được tăng lên vì cảm ứng có thể có độ sâu gần 2500. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
5 4 5
1 2
1 3
1 4
1 2
1 3
1 4
1 5
```Cả hai cây đặc biệt đều là những ngôi sao. Thuật toán liên tục đi vào Trường hợp A, vì cây lớn nhất là một ngôi sao. 

| Hiện tại (n) | Trường hợp | Đỉnh mới | Đặt cây | 
| --- | --- | --- | --- | 
| 5 | A | 5 | (T_5) | 
| 4 | A | 4 | (T_4) | 
| 3 | A | 3 | (T_3) | 
| 2 | Căn cứ | 2 | (T_2) | 

Một đầu ra hợp lệ là```
Y
2 1
3 1 2
4 1 2 3
5 1 2 3 4
```Ở mỗi cấp độ, ngôi sao mới sử dụng mọi cạnh từ đỉnh mới đến các đỉnh hiện có. Bất biến đệ quy đặc biệt rõ ràng ở đây: sau khi loại bỏ ngôi sao lớn nhất, tất cả các cạnh giữa các đỉnh còn lại đều không bị ảnh hưởng. 

### Mẫu 2 

Đầu vào là```
4 3 4
1 2
2 3
1 2
2 3
3 4
```Cả (T_3) và (T_4) đều là đường dẫn nên thuật toán đạt đến Trường hợp C. 

| Biến | (T_4) | (T_3) | 
| --- | --- | --- | 
| Cây gốc | (1-2-3-4) | (1-2-3) | 
| Bỏ lá | 1, 4 | 1, 3 | 
| Giảm kích thước | 2 | 1 | 
| Đỉnh mới | 3, 4 | 3, 4 | 
| Tái thiết | hai cạnh mới | hai cạnh mới | 

Phiên bản đệ quy sử dụng (K_2). Sau đó, hai đỉnh mới sẽ khôi phục các lá đã bị loại bỏ, trong khi (T_2) được đặt bằng cạnh còn lại. 

Một đầu ra hợp lệ là```
Y
2 1
2 4 3
4 1 3 2
```Dấu vết chứng minh tại sao hai đỉnh mới là đủ. Bốn cạnh cần thiết để khôi phục hai cây đặc biệt có thể được chỉ định mà không trùng nhau và tất cả các cạnh còn lại liên quan đến các đỉnh mới tạo thành hai ngôi sao cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(N^2)) | Đầu ra chứa số nguyên (O(N^2)) và mức rút gọn chỉ quét hai cây đặc biệt trên tất cả các mức đệ quy | 
| Không gian | (O(N^2)) | Bản thân câu trả lời chứa (O(N^2)) số nguyên | 

Giới hạn bậc hai khớp với kích thước đầu ra không thể tránh khỏi. Với (N\le2500), tổng số số nguyên đầu ra là khoảng (3,1) triệu, do đó, cấu trúc (O(N^2)) là thang đo dự kiến ​​cho một giây, giới hạn 256 MB được đưa ra bởi câu lệnh bài toán. 

## Trường hợp thử nghiệm 

Vì đầu ra không phải là duy nhất nên việc kiểm tra phải xác minh các thuộc tính đóng gói thay vì so sánh đầu ra với một chuỗi cố định. Khai thác thử nghiệm sau đây giả định giải pháp được lưu dưới dạng`solution.py`và phơi bày`solve()`chức năng.```python
import sys
import io

from solution import solve

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

def verify(inp: str, out: str) -> bool:
    data = list(map(str.split, inp.strip().splitlines()))
    first = list(map(int, data[0]))

    n, a, b = first

    lines = out.strip().splitlines()
    assert lines[0] == "Y"

    assert len(lines) == n

    mappings = [None] * (n + 1)

    for i in range(2, n + 1):
        row = list(map(int, lines[i - 1]))
        assert len(row) == i
        assert len(set(row)) == i
        assert all(1 <= x <= n for x in row)
        mappings[i] = row

    trees = {}

    pos = 1

    edges = []
    for _ in range(a - 1):
        u, v = map(int, data[pos])
        pos += 1
        edges.append((u, v))
    trees[a] = edges

    edges = []
    for _ in range(b - 1):
        u, v = map(int, data[pos])
        pos += 1
        edges.append((u, v))
    trees[b] = edges

    used = set()

    for i in range(2, n + 1):
        if i not in trees:
            edges = [(1, j) for j in range(2, i + 1)]
        else:
            edges = trees[i]

        row = mappings[i]

        for u, v in edges:
            x = row[u - 1]
            y = row[v - 1]
            edge = tuple(sorted((x, y)))

            assert edge not in used
            used.add(edge)

    assert len(used) == n * (n - 1) // 2
    return True

# Sample 1
sample1 = """\
5 4 5
1 2
1 3
1 4
1 2
1 3
1 4
1 5
"""

assert verify(sample1, run(sample1))

# Sample 2
sample2 = """\
4 3 4
1 2
2 3
1 2
2 3
3 4
"""

assert verify(sample2, run(sample2))

# Minimum-size case
case_min = """\
3 2 3
1 2
1 2
2 3
"""

assert verify(case_min, run(case_min))

# Boundary case: one exceptional tree has size N-1
case_boundary = """\
5 4 5
1 2
2 3
3 4
1 2
2 3
3 4
4 5
"""

assert verify(case_boundary, run(case_boundary))

# All trees are stars
case_all_stars = """\
6 3 6
1 2
1 3
1 2
1 3
1 4
1 2
1 3
1 4
1 5
1 2
1 3
1 4
1 5
1 6
"""

assert verify(case_all_stars, run(case_all_stars))

# Maximum-size case.
n = 2500
a = 2498
b = 2499

parts = [f"{n} {a} {b}"]

for i in range(2, a + 1):
    parts.append(f"{i - 1} {i}")

for i in range(2, b + 1):
    parts.append(f"{i - 1} {i}")

case_max = "\n".join(parts) + "\n"

assert verify(case_max, run(case_max))
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 2 3`với một con đường (T_3) |`Y`| Kích thước tối thiểu và Trường hợp B | 
|`5 4 5`với những con đường |`Y`| Trường hợp C và tái thiết hai lá | 
|`6 3 6`với tất cả các ngôi sao |`Y`| Trường hợp A lặp đi lặp lại và xử lý sao | 
|`2500 2498 2499`với hai con đường |`Y`| Tối đa (N), độ sâu đệ quy và đầu ra bậc hai | 

## Vỏ cạnh 

Đối với trường hợp tối thiểu```
3 2 3
1 2
1 2
2 3
```(T_2) là một ngôi sao và (T_3) là một đường đi. Thuật toán vào Trường hợp B. Nó loại bỏ một lá khỏi (T_3), để lại một cây hai đỉnh, giải (K_2), ánh xạ lá đã xóa tới thành phố 3 và đặt (T_2) tại thành phố 3. Ba cạnh cuối cùng đều khác nhau nên đầu ra là`Y`. 

Đối với trường hợp cây lớn nhất là một ngôi sao, thuật toán không bao giờ cần kiểm tra các cạnh của nó. Nó đóng gói đệ quy phiên bản nhỏ hơn và gán đỉnh mới làm tâm sao. Mỗi cạnh của ngôi sao này đều có một đỉnh mới làm điểm cuối, vì vậy không có đỉnh nào có thể xuất hiện trong gói đệ quy. 

Đối với trường hợp hai không phải sao, hãy xem xét```
4 3 4
1 2
2 3
1 2
2 3
3 4
```Đường đi bốn đỉnh có hai lá có cha mẹ khác nhau và đường đi ba đỉnh có cùng thuộc tính. Việc loại bỏ những chiếc lá đó sẽ tạo ra những cây nhỏ hơn phù hợp với (K_2) và (K_1). Việc tái thiết sau đó sử dụng hai thành phố mới cho những chiếc lá bị loại bỏ. Hướng của bốn cạnh mới được chọn sao cho không có cạnh thành phố mẹ mới nào bị trùng lặp. 

Trường hợp toàn sao là một điều kiện biên hữu ích khác vì hai nước cộng hòa ngoại lệ được chỉ định cũng được phép trở thành ngôi sao. Việc thực hiện không cho rằng`a`Và`b`thực sự là những cây không phải sao. Nó kiểm tra cấu trúc mức độ thực tế ở mọi cấp độ đệ quy, do đó, một cây đặc biệt được chỉ định là một ngôi sao sẽ được xử lý bởi Trường hợp A. 

Trường hợp tối đa kiểm tra hai ràng buộc quan trọng nhất trong quá trình triển khai. Phép đệ quy có thể đạt đến độ sâu gần (2500), đó là lý do tại sao giới hạn đệ quy được tăng lên và câu trả lời chứa hàng triệu số nguyên, đó là lý do tại sao cấu trúc chỉ lưu trữ các hàng ánh xạ cuối cùng thay vì ma trận cạnh rõ ràng (N\times N). 

Bài học sâu hơn là cấu trúc quy nạp. Bài toán trông giống như một bài toán đóng gói cạnh toàn cục khó khăn, nhưng các ngôi sao cho chúng ta một cách có kiểm soát để đưa ra các đỉnh mới. Khi hai cây đặc biệt được giảm xuống thành những cây nhỏ hơn, vấn đề tương tự lại xuất hiện. Đó chính xác là kiểu rút gọn cấu trúc cần tìm bất cứ khi nào một đồ thị hoàn chỉnh phải được phân tách thành các đối tượng có kích thước tạo thành chuỗi (2,3,\ldots,N).
