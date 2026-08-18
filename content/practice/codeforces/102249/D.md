---
title: "CF 102249D - Cây xanh như một dịch vụ"
description: "Chúng ta cần xây dựng một cây có gốc trên các đỉnh 1…N. Mọi yêu cầu đều có dạng (x, y, z), nghĩa là khi chúng ta đi lên từ x và y thì đỉnh chung đầu tiên của chúng phải chính xác là z."
date: "2026-08-17T21:56:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102249
codeforces_index: "D"
codeforces_contest_name: "2019 Facebook Hacker Cup, Qualification Round"
rating: 0
weight: 102249
solve_time_s: 264
verified: true
draft: false
---

[CF 102249D - Cây xanh như một dịch vụ](https://codeforces.com/problemset/problem/102249/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4 phút 24s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta cần xây dựng một cây có gốc trên các đỉnh`1 ... N`. Mọi yêu cầu đều có dạng`(x, y, z)`, nghĩa là khi chúng ta đi lên từ`x`Và`y`, đỉnh chung đầu tiên của chúng phải chính xác`z`. Chúng ta có thể chọn bất kỳ cây cha nào cho mỗi đỉnh, miễn là con trỏ cha kết quả mô tả một cây có gốc và mọi yêu cầu đều được thỏa mãn. 

Đầu ra cung cấp cho cha mẹ của mỗi đỉnh. Chính xác một đỉnh có cha mẹ`0`, đó là gốc. Vì bài toán chấp nhận bất kỳ cây hợp lệ nào nên hai mảng cha khác nhau đều có thể đúng. 

Các hạn chế là nhỏ có chủ ý. Có nhiều nhất 60 đỉnh và 120 yêu cầu, do đó, một thuật toán bao quanh`O(NM + N^2)`dễ dàng đủ nhanh. nhỏ`N`cũng cho phép chúng ta sử dụng lặp đi lặp lại cấu trúc tập hợp rời rạc đơn giản trong khi phân tách đệ quy tập hợp đỉnh. Điều bị loại trừ là việc liệt kê đầy đủ các cây có nhãn có gốc, số lượng của chúng là`N^(N-1)`. Tại`N = 60`, đây là đại khái`10^105`, vì vậy ngay cả việc kiểm tra nhanh một ứng viên cũng sẽ vô ích. 

Có một số trường hợp khó xử lý. 

Đầu tiên, LCA có thể là một trong hai đỉnh được truy vấn. Ví dụ,```
2 1
1 2 1
```là hợp lệ. đỉnh`1`là tổ tiên của đỉnh`2`, vậy LCA là`1`và mảng cha có thể là`0 1`. Một công trình luôn giả định`z`phải khác với`x`Và`y`sẽ từ chối trường hợp này một cách không chính xác. 

Thứ hai, việc chọn root tùy ý là không an toàn. Vì```
3 1
1 2 3
```gốc duy nhất có thể là`3`, bởi vì`3`phải là tổ tiên của cả hai`1`Và`2`. Lựa chọn`1`vì root ngay lập tức làm cho yêu cầu không thể thực hiện được. Việc xây dựng phải xác định một đỉnh không bị buộc phải có tổ tiên bên trong tập hợp hiện tại. 

Thứ ba, một số yêu cầu có thể tạo ra một chu kỳ trong mối quan hệ tổ tiên. Coi như```
3 3
1 2 2
2 3 3
3 1 1
```Lực lượng yêu cầu đầu tiên`2`bên trên`1`, lực thứ hai`3`bên trên`2`, và các lực thứ ba`1`bên trên`3`. Không có cây có gốc nào có thể chứa cả ba quan hệ, vì vậy câu trả lời là`Impossible`. 

Cuối cùng, yêu cầu có LCA bằng gốc hiện tại là ràng buộc phân tách chứ không phải ràng buộc nhóm. Ví dụ,```
3 2
1 2 3
1 3 1
```không thể được xử lý bằng cách đơn giản nhóm từng bộ ba lại với nhau. Yêu cầu đầu tiên muốn`1`Và`2`trong các cây con khác nhau của`3`, trong khi người thứ hai nói rằng`1`là tổ tiên của`3`. Những yêu cầu này xung đột. Việc coi mọi yêu cầu như một hoạt động kết hợp thông thường sẽ làm mất đi sự khác biệt giữa "phải ở bên nhau" và "phải tách biệt". 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản về mặt khái niệm. Liệt kê từng cây có gốc trên`N`các đỉnh được gắn nhãn, tính LCA của mỗi cặp được truy vấn và giữ cho cây đầu tiên đáp ứng mọi yêu cầu. có`N^(N-1)`cây được dán nhãn có rễ. Ngay cả trước khi tính đến việc xác minh, trường hợp xấu nhất là`N = 60`là`60^59`, khoảng`10^105`ứng viên. Kiểm tra`M`yêu cầu với một`O(N)`Tính toán LCA sẽ làm cho công việc trở nên gần như`O(N^(N-1)MN)`, điều đó hoàn toàn không thể thực hiện được. 

Quan sát hữu ích là một yêu cầu LCA chứa hai loại thông tin khác nhau. 

Nếu như`LCA(x, y) = z`, sau đó`z`phải là tổ tiên của cả hai`x`Và`y`. Nếu như`z`không phải là gốc của cây con hiện tại thì`x`,`y`, Và`z`tất cả phải nằm trong cùng một cây con của gốc đó. Chúng không bao giờ có thể được chia thành các cây con khác nhau. 

Mặt khác, nếu`z`là gốc của cây con hiện tại thì`x`Và`y`phải thuộc về các cây con khác nhau, trừ khi một trong số chúng là gốc. Nếu không thì LCA của họ sẽ ở dưới đây`z`. 

Điều này cho chúng ta một thủ tục phân vùng đệ quy. Chọn gốc phù hợp`r`cho tập đỉnh hiện tại. Sử dụng cấu trúc tập hợp rời rạc để hợp nhất các đỉnh buộc phải ở trong cùng một cây con của`r`. Sau đó, mọi thành phần kết quả sẽ trở thành một cây con con của`r`. Các yêu cầu có LCA`r`được kiểm tra bằng cách yêu cầu hai điểm cuối không phải gốc của chúng hạ cánh ở các thành phần khác nhau. Quá trình tương tự sau đó được áp dụng độc lập cho mọi thành phần. 

Bản thân gốc không thể tùy tiện. Đối với mọi yêu cầu`LCA(x, y) = z`, nếu như`x != z`, sau đó`z`lực lượng`x`ở dưới`z`; tương tự, nếu`y != z`, sau đó`z`lực lượng`y`ở dưới`z`. Do đó, một nghiệm hợp lệ của tập hợp hiện tại phải không có cạnh tổ tiên bắt buộc đến từ một đỉnh khác trong tập hợp đó. Đỉnh như vậy là phần tử tối thiểu của quan hệ tổ tiên bắt buộc. 

Lý do chúng ta không cần thử mọi đỉnh tối thiểu là một đặc tính hữu ích của những ràng buộc này. Khi một tập hợp được chia thành các thành phần con, các ràng buộc hoàn toàn có trong một thành phần sẽ độc lập với các lựa chọn được thực hiện trong các thành phần khác. Nếu một số thành phần không thể được xây dựng, việc thay đổi gốc được chọn ở trên không thể làm cho những ràng buộc tương tự đó đồng thời biến mất. Cuộc thảo luận trong cuộc thi đưa ra cách giải thích tương tự về việc lấy cây con nhỏ nhất chứa tập con các đỉnh bị lỗi. 

Phương pháp brute-force hoạt động vì nó thử rõ ràng mọi thứ bậc có thể. Nó thất bại vì số lượng hệ thống phân cấp là rất lớn. Quan sát cho thấy mọi yêu cầu của LCA đều buộc các đỉnh vào cùng một cây con con hoặc buộc chúng vào các cây con khác nhau sẽ làm giảm vấn đề phân vùng lặp lại, đó là đa thức. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(N^(N-1) M N)`|`O(N)`| Quá chậm | 
| Phân vùng DSU đệ quy |`O(NM + N^2 α(N))`|`O(N^2)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ mọi yêu cầu`(x, y, z)`và giải thích nó như là hai mối quan hệ tổ tiên có thể có,`z -> x`khi`x != z`Và`z -> y`khi`y != z`. Những mối quan hệ này cho chúng ta biết đỉnh nào không thể được chọn làm gốc của cùng một cây con hiện tại. 
2. Bắt đầu với bộ đỉnh hoàn chỉnh`{1, ..., N}`và xây dựng đệ quy một cây cho nó. Một cuộc gọi đệ quy nhận được một tập hợp`S`các đỉnh của nó được cho là tạo thành một cây con có gốc được kết nối. 
3. Trước khi chọn một gốc, hãy kiểm tra mọi yêu cầu có`z`thuộc về`S`. Nếu như`z`ở bên trong`S`, cả hai`x`Và`y`chắc cũng ở trong`S`, bởi vì`z`là tổ tiên của họ. Nếu một điểm cuối đã được đặt bên ngoài`S`, việc xây dựng là không thể. 
4. Tìm một đỉnh`r`TRONG`S`không có cạnh đến bắt buộc từ một đỉnh khác của`S`. Nói cách khác, không được có yêu cầu nào về`z != r`Và`x = r`, và không có yêu cầu với`z != r`Và`y = r`. Một đỉnh như vậy có thể đóng vai trò là gốc của cây con này. Nếu không có đỉnh như vậy tồn tại thì quan hệ tổ tiên bắt buộc chứa một chu trình nên câu trả lời là không thể. 
5. Tạo một DSU chứa tất cả các đỉnh trong`S`. Đối với mọi yêu cầu`(x, y, z)`hoàn toàn chứa đựng trong`S`với`z != r`, hợp nhất`x`,`y`, Và`z`. Từ`r`ở trên`z`, cả ba đỉnh phải nằm dưới cùng một con của`r`. Hợp nhất cả ba bản ghi chính xác yêu cầu đó. 
6. Bây giờ hãy kiểm tra mọi yêu cầu`(x, y, r)`. Nếu không có điểm cuối nào là`r`, các thành phần DSU của chúng phải khác nhau. Nếu chúng nằm trong cùng một thành phần thì cả hai sẽ nằm trong cùng một cây con của`r`, thực hiện LCA của họ ở mức dưới đây`r`. Nếu một điểm cuối là`r`, yêu cầu được tự động thỏa mãn vì LCA của gốc và bất kỳ hậu duệ nào đều là gốc. 
7. Các thành phần DSU trong số`S - {r}`bây giờ là cây con của`r`. Đối với mọi thành phần, hãy xây dựng cây của nó một cách đệ quy. Nếu cấu trúc đệ quy trả về root`v`, bộ`parent[v] = r`. 
8. Nếu lệnh gọi đệ quy chỉ chứa một đỉnh thì đỉnh đó là gốc của cây con đó và không cần thực hiện thêm thao tác nào. Khi mọi thành phần đã được xử lý, hãy quay lại`r`tới người gọi. 
9. Sau khi xây dựng cây hoàn chỉnh, mảng cha chứa đúng một`0`, gốc và mọi đỉnh khác đều có một đỉnh cha. Việc xây dựng có thể được xác minh tùy ý bằng cách tính toán lại LCA của mọi yêu cầu. Chi phí này chỉ`O(MN)`và rất hữu ích khi kiểm tra việc thực hiện phòng thủ. 

### Tại sao nó hoạt động 

Bất biến quan trọng là mọi tập đệ quy`S`đại diện cho một cây con của cây cuối cùng và mọi yêu cầu có LCA nằm trong`S`có tất cả ba đỉnh của nó bên trong`S`. 

Giả sử gốc hiện tại là`r`. Đối với một yêu cầu`(x, y, z)`với`z != r`, đỉnh`z`ở dưới đúng một con của`r`. Từ`z`phải là tổ tiên của cả hai`x`Và`y`, cả ba đỉnh phải ở dưới cùng một đứa trẻ đó. DSU hợp nhất chúng, do đó việc xây dựng không bao giờ tách rời các đỉnh phải ở cùng nhau. 

Đối với một yêu cầu`(x, y, r)`, LCA phải chính xác là gốc hiện tại. Kể từ đây`x`Và`y`phải chiếm các thành phần con khác nhau và việc kiểm tra DSU sẽ từ chối chính xác trường hợp chúng ở cùng nhau. 

Sau những lần kiểm tra này, mọi thành phần DSU có thể trở thành cây con con của`r`. Các yêu cầu thuộc về các thành phần khác nhau không thể có LCA hoàn toàn nằm trong một trong các thành phần đó, bởi vì yêu cầu như vậy sẽ buộc cả ba đỉnh của nó vào cùng một thành phần. Như vậy các bài toán đệ quy là độc lập. 

Quy tắc chọn gốc ngăn không cho một đỉnh được đặt phía trên một đỉnh đã được yêu cầu là đỉnh tổ tiên của nó. Nếu đồ thị tổ tiên bắt buộc có một chu trình thì không tồn tại nghiệm hợp lệ. Nếu trường hợp khả thi, việc liên tục lấy một đỉnh tối thiểu và phân vùng theo các ràng buộc sẽ duy trì tính khả thi, do đó không cần phải quay lại các lựa chọn gốc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class DSU:
    def __init__(self, nodes):
        self.parent = {v: v for v in nodes}
        self.size = {v: 1 for v in nodes}

    def find(self, x):
        p = self.parent[x]
        if p != x:
            self.parent[x] = self.find(p)
        return self.parent[x]

    def union(self, a, b):
        a = self.find(a)
        b = self.find(b)
        if a == b:
            return

        if self.size[a] < self.size[b]:
            a, b = b, a

        self.parent[b] = a
        self.size[a] += self.size[b]

def construct_tree(n, constraints):
    parent = [-1] * (n + 1)

    def build(nodes):
        if len(nodes) == 1:
            return nodes[0]

        inside = set(nodes)

        # Every constraint whose LCA is inside this subtree
        # must have all of its vertices inside it.
        for x, y, z in constraints:
            if z in inside and (x not in inside or y not in inside):
                return -1

        # Find a minimal vertex in the forced ancestor relation.
        incoming = {v: False for v in nodes}

        for x, y, z in constraints:
            if z not in inside:
                continue

            if x in inside and x != z:
                incoming[x] = True
            if y in inside and y != z:
                incoming[y] = True

        root = -1
        for v in nodes:
            if not incoming[v]:
                root = v
                break

        if root == -1:
            return -1

        dsu = DSU(nodes)

        # If the current root is r and z != r, then x, y, z
        # must all lie in the same child subtree of r.
        for x, y, z in constraints:
            if z not in inside or z == root:
                continue

            dsu.union(x, y)
            dsu.union(x, z)

        # If z is the current root, x and y must be in
        # different child subtrees unless one of them is root.
        for x, y, z in constraints:
            if z != root:
                continue

            if x == root or y == root:
                continue

            if dsu.find(x) == dsu.find(y):
                return -1

        # Build the DSU components excluding the root.
        groups = {}

        for v in nodes:
            if v == root:
                continue

            r = dsu.find(v)
            groups.setdefault(r, []).append(v)

        # Every component becomes one child subtree of root.
        for component in groups.values():
            child_root = build(component)
            if child_root == -1:
                return -1
            parent[child_root] = root

        return root

    root = build(list(range(1, n + 1)))

    if root == -1:
        return None

    parent[root] = 0
    return parent[1:]

def lca(parent, a, b):
    ancestors = set()

    while a != 0:
        ancestors.add(a)
        a = parent[a]

    while b != 0:
        if b in ancestors:
            return b
        b = parent[b]

    return 0

def valid_tree(parent, constraints):
    n = len(parent)
    if parent.count(0) != 1:
        return False

    # Check that every parent pointer stays inside the vertex range.
    for p in parent:
        if p < 0 or p > n:
            return False

    # Check that the parent pointers contain no cycle.
    for v in range(1, n + 1):
        seen = set()
        u = v

        while u != 0:
            if u in seen:
                return False
            seen.add(u)
            u = parent[u]

    for x, y, z in constraints:
        if lca(parent, x, y) != z:
            return False

    return True

def solve_case(n, constraints):
    answer = construct_tree(n, constraints)

    if answer is None:
        return None

    # Defensive verification. The construction itself is sufficient,
    # but this catches implementation mistakes without changing
    # the asymptotic complexity.
    if not valid_tree(answer, constraints):
        return None

    return answer

def main():
    t = int(input())
    out = []

    for case_id in range(1, t + 1):
        n, m = map(int, input().split())
        constraints = [
            tuple(map(int, input().split()))
            for _ in range(m)
        ]

        answer = solve_case(n, constraints)

        if answer is None:
            out.append(f"Case #{case_id}: Impossible")
        else:
            out.append(
                f"Case #{case_id}: " +
                " ".join(map(str, answer))
            )

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    main()
```các`DSU`lớp là cục bộ đối với mỗi lệnh gọi đệ quy vì các thành phần chỉ có ý nghĩa liên quan đến cây con hiện tại. Nén đường dẫn tiếp tục lặp đi lặp lại`find`các hoạt động có hiệu quả không đổi đối với những ràng buộc này. 

các`build`trước tiên, hàm này sẽ kiểm tra xem yêu cầu có LCA thuộc bộ hiện tại có nằm ngoài bộ đó không. Tình trạng này xuất phát trực tiếp từ tổ tiên: nếu`z`nằm trong cây con, cả hai đỉnh được truy vấn phải là con của`z`, vì vậy chúng cũng phải ở bên trong cây con. 

các`incoming`mảng ghi lại mối quan hệ tổ tiên bắt buộc. Khi`z != x`, yêu cầu nói rằng`z`phải ở trên`x`, Vì thế`x`không thể là root hiện tại. Lý do tương tự áp dụng cho`y`. Mối quan hệ tự thân như`LCA(x, y) = x`không đánh dấu`x`như có một cạnh tới, bởi vì`x`được phép là tổ tiên của`y`. 

Thẻ DSU đầu tiên chỉ xử lý các yêu cầu có LCA không phải là gốc hiện tại. Sự khác biệt giữa`z != root`Và`z == root`là điều cần thiết. Cái trước có nghĩa là ba đỉnh phải ở cùng nhau bên dưới một con của`root`; cái sau có nghĩa là hai điểm cuối được truy vấn phải được phân tách tại`root`. 

Các nhóm chỉ được xây dựng sau khi tất cả các ràng buộc liên quan đến gốc hiện tại đã được kiểm tra. Mỗi nhóm được đảm bảo được kết nối nội bộ bằng quan hệ "phải ở cùng nhau" và việc đặt gốc được xây dựng đệ quy của nó ngay bên dưới gốc hiện tại sẽ tạo ra chính xác một cây con con cho nhóm đó. 

Việc xác minh cuối cùng sử dụng tập tổ tiên để tính toán từng LCA trong`O(N)`. Không có vấn đề tràn số nguyên trong Python và độ sâu đệ quy cao nhất là`N`, chỉ bằng 60. Mã này sử dụng cách đánh số đỉnh dựa trên 1 xuyên suốt và chuyển đổi mảng cha cuối cùng thành danh sách Python dựa trên 0 chỉ ở ranh giới đầu ra. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
3 1
1 2 3
```Có một yêu cầu,`LCA(1, 2) = 3`. 

| Bộ hiện tại | Buộc các cạnh đến | Chọn gốc | Các thành phần DSU sau khi nhóm | Kiểm tra tách | 
| --- | --- | --- | --- | --- | 
|`{1,2,3}`|`3 -> 1`,`3 -> 2`|`3`|`{1}`,`{2}`|`1`Và`2`khác nhau | 

Đỉnh duy nhất không có cạnh đến bắt buộc là`3`, vì vậy nó trở thành gốc. Vì yêu cầu có`z = 3`, điểm cuối`1`Và`2`phải ở các thành phần con khác nhau. Họ đã ly thân rồi nên cả hai đều trở thành con của`3`. 

Mảng cha kết quả là`3 3 0`, phù hợp với đầu ra mẫu. 

### Mẫu 2 

Đầu vào là```
3 3
1 2 2
2 3 3
3 1 1
```Các mối quan hệ tổ tiên bắt buộc là: 

| Yêu cầu | Quan hệ cưỡng ép | 
| --- | --- | 
|`LCA(1,2)=2`|`2 -> 1`| 
|`LCA(2,3)=3`|`3 -> 2`| 
|`LCA(3,1)=1`|`1 -> 3`| 

Tìm kiếm gốc thấy rằng mọi đỉnh đều có một mối quan hệ bắt buộc sắp tới. 

| Đỉnh | Tổ tiên cưỡng bức đến | 
| --- | --- | 
|`1`|`2`,`1`| 
|`2`|`3`| 
|`3`|`1`| 

Không có root nên việc xây dựng ngay lập tức trở lại`Impossible`. 

Dấu vết này chứng minh tại sao chỉ kiểm tra các phương trình LCA cục bộ là không đủ. Các phương trình này áp đặt chung một mối quan hệ tổ tiên theo chu kỳ, mà không cây có gốc nào có thể biểu diễn được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(NM + N^2 α(N))`| Nhiều nhất`N`cuộc gọi đệ quy quét`M`các ràng buộc và DSU hoạt động trên tất cả các cấp độ đệ quy được giới hạn bởi`O(N^2 α(N))`. Việc xác minh cuối cùng bổ sung`O(MN)`. | 
| Không gian |`O(N^2)`| Các lệnh gọi đệ quy và các tập đỉnh tạm thời có thể chiếm`O(N^2)`khoảng trống trong trường hợp xấu nhất, trong khi danh sách ràng buộc và mảng cha sử dụng`O(M + N)`. | 

Với`N <= 60`Và`M <= 120`, ngay cả các thừa số bậc hai đơn giản cũng rất nhỏ. Thuật toán chỉ thực hiện vài trăm nghìn thao tác nguyên thủy cho mỗi trường hợp thử nghiệm trong các trường hợp cấu trúc tồi tệ nhất, thấp hơn nhiều so với mức cần thiết để liệt kê theo cấp số nhân của các cây có gốc. 

## Trường hợp thử nghiệm 

Mẫu có nhiều kết quả đầu ra hợp lệ trong một số trường hợp, do đó, việc xác nhận so sánh chuỗi đầu ra hoàn chỉnh sẽ nghiêm ngặt một cách không cần thiết. Thay vào đó, khai thác thử nghiệm sau đây khẳng định rằng mọi cây được báo cáo đều hợp lệ và hai trường hợp mẫu không thể thực sự bị từ chối.```python
import sys
import io

class DSU:
    def __init__(self, nodes):
        self.parent = {v: v for v in nodes}
        self.size = {v: 1 for v in nodes}

    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]

    def union(self, a, b):
        a = self.find(a)
        b = self.find(b)
        if a == b:
            return
        if self.size[a] < self.size[b]:
            a, b = b, a
        self.parent[b] = a
        self.size[a] += self.size[b]

def solve_case(n, constraints):
    parent = [-1] * (n + 1)

    def build(nodes):
        if len(nodes) == 1:
            return nodes[0]

        inside = set(nodes)

        for x, y, z in constraints:
            if z in inside and (x not in inside or y not in inside):
                return -1

        incoming = {v: False for v in nodes}

        for x, y, z in constraints:
            if z not in inside:
                continue
            if x in inside and x != z:
                incoming[x] = True
            if y in inside and y != z:
                incoming[y] = True

        root = -1
        for v in nodes:
            if not incoming[v]:
                root = v
                break

        if root == -1:
            return -1

        dsu = DSU(nodes)

        for x, y, z in constraints:
            if z in inside and z != root:
                dsu.union(x, y)
                dsu.union(x, z)

        for x, y, z in constraints:
            if z == root and x != root and y != root:
                if dsu.find(x) == dsu.find(y):
                    return -1

        groups = {}
        for v in nodes:
            if v == root:
                continue
            r = dsu.find(v)
            groups.setdefault(r, []).append(v)

        for component in groups.values():
            child_root = build(component)
            if child_root == -1:
                return -1
            parent[child_root] = root

        return root

    root = build(list(range(1, n + 1)))
    if root == -1:
        return None

    parent[root] = 0
    return parent[1:]

def lca(parent, a, b):
    seen = set()

    while a != 0:
        seen.add(a)
        a = parent[a]

    while b != 0:
        if b in seen:
            return b
        b = parent[b]

    return 0

def valid_answer(n, constraints, answer):
    if answer is None:
        return False

    if len(answer) != n:
        return False

    if answer.count(0) != 1:
        return False

    for i, p in enumerate(answer, 1):
        if p < 0 or p > n or p == i:
            return False

    for v in range(1, n + 1):
        seen = set()
        u = v
        while u != 0:
            if u in seen:
                return False
            seen.add(u)
            u = answer[u - 1]

    for x, y, z in constraints:
        if lca(answer, x, y) != z:
            return False

    return True

def run(inp: str) -> str:
    data = io.StringIO(inp)
    t = int(data.readline())
    outputs = []

    for case_id in range(1, t + 1):
        n, m = map(int, data.readline().split())
        constraints = [
            tuple(map(int, data.readline().split()))
            for _ in range(m)
        ]

        answer = solve_case(n, constraints)

        if answer is None:
            outputs.append(f"Case #{case_id}: Impossible")
        else:
            outputs.append(
                f"Case #{case_id}: " +
                " ".join(map(str, answer))
            )

    return "\n".join(outputs)

def parse_outputs(out):
    return out.strip().splitlines()

def check_case(line, case_id, n, constraints, must_be_impossible=False):
    prefix = f"Case #{case_id}: "
    assert line.startswith(prefix), line

    body = line[len(prefix):]

    if must_be_impossible:
        assert body == "Impossible", line
        return

    assert body != "Impossible", line
    answer = list(map(int, body.split()))
    assert valid_answer(n, constraints, answer), line

# Provided samples
sample = """6
3 1
1 2 3
3 3
1 2 2
2 3 3
3 1 1
4 2
2 1 2
1 4 3
6 3
2 4 3
6 5 4
1 2 6
7 4
7 3 5
4 1 2
6 3 6
6 4 5
12 9
1 5 7
11 2 6
9 4 12
8 12 6
10 1 7
4 3 12
3 10 6
8 11 8
2 5 10
"""

out = parse_outputs(run(sample))
assert len(out) == 6

check_case(out[0], 1, 3, [(1, 2, 3)])
check_case(
    out[1], 2, 3,
    [(1, 2, 2), (2, 3, 3), (3, 1, 1)],
    must_be_impossible=True
)
check_case(
    out[2], 3, 4,
    [(2, 1, 2), (1, 4, 3)]
)
check_case(
    out[3], 4, 6,
    [(2, 4, 3), (6, 5, 4), (1, 2, 6)],
    must_be_impossible=True
)
check_case(
    out[4], 5, 7,
    [(7, 3, 5), (4, 1, 2), (6, 3, 6), (6, 4, 5)]
)
check_case(
    out[5], 6, 12,
    [
        (1, 5, 7), (11, 2, 6), (9, 4, 12),
        (8, 12, 6), (10, 1, 7), (4, 3, 12),
        (3, 10, 6), (8, 11, 8), (2, 5, 10)
    ]
)

# Minimum-size input.
minimum = """1
2 1
1 2 1
"""
out = parse_outputs(run(minimum))
check_case(out[0], 1, 2, [(1, 2, 1)])

# All requirements use the same LCA.
all_equal = """4
5 4
1 2 5
1 3 5
1 4 5
2 3 5
"""
out = parse_outputs(run(all_equal))
check_case(
    out[0], 1, 5,
    [(1, 2, 5), (1, 3, 5), (1, 4, 5), (2, 3, 5)]
)

# Maximum-size instance, with 60 vertices and 120 consistent constraints.
# Vertex 60 is the root and every other vertex can be its direct child.
constraints = []
for i in range(120):
    x = 1 + (i % 59)
    y = 1 + ((i + 1) % 59)
    if x == y:
        y = 59
    constraints.append((x, y, 60))

maximum = "1\n60 120\n"
maximum += "\n".join(f"{x} {y} {z}" for x, y, z in constraints)
maximum += "\n"

out = parse_outputs(run(maximum))
check_case(out[0], 1, 60, constraints)

# Contradictory ancestor cycle.
cycle = """1
3 3
1 2 2
2 3 3
3 1 1
"""
out = parse_outputs(run(cycle))
check_case(
    out[0], 1, 3,
    [(1, 2, 2), (2, 3, 3), (3, 1, 1)],
    must_be_impossible=True
)

print("All tests passed.")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 1 / 1 2 1`| Cây nào hợp lệ | tối thiểu`N`và trường hợp LCA bằng một đỉnh được truy vấn | 
|`5 4 / ... 5`| Cây nào hợp lệ | Nhiều yêu cầu chia sẻ cùng một LCA và tách nhiều cặp | 
|`60 120 / ... 60`| Cây nào hợp lệ | Tối đa`N`, tối đa`M`và lặp đi lặp lại các hoạt động DSU ở quy mô ranh giới | 
|`3 3 / 1 2 2 / 2 3 3 / 3 1 1`|`Impossible`| Quan hệ tổ tiên cưỡng bức theo chu kỳ | 

## Vỏ cạnh 

Đối với trường hợp điểm cuối-LCA```
2 1
1 2 1
```mối quan hệ bắt buộc là`1 -> 2`, trong khi đỉnh`1`bản thân nó không có cạnh cưỡng bức đi vào. Thuật toán chọn`1`như là gốc. Vì yêu cầu có`z = root`, nó không yêu cầu`1`Và`2`phải chia ly. Thành phần đơn`{2}`trở thành con của`1`, đưa ra mảng cha mẹ`0 1`. LCA của`1`Và`2`chính xác là`1`. 

Đối với trường hợp chọn gốc```
3 1
1 2 3
```những mối quan hệ bắt buộc là`3 -> 1`Và`3 -> 2`. Đỉnh`1`Và`2`cả hai đều có quan hệ sắp tới, trong khi`3`không, vì vậy thuật toán chọn`3`. Bởi vì`z`bằng với gốc,`1`Và`2`phải chiếm các thành phần khác nhau. Họ làm, sản xuất cây`3 3 0`. 

Đối với trường hợp tuần hoàn```
3 3
1 2 2
2 3 3
3 1 1
```những mối quan hệ bắt buộc là`2 -> 1`,`3 -> 2`, Và`1 -> 3`. Mỗi đỉnh đều có một mối quan hệ đến, do đó việc tìm kiếm gốc không thành công trước khi thử phân vùng DSU. Trở về`Impossible`đúng vì mỗi cây có gốc đều có ít nhất một đỉnh không có tổ tiên bên trong toàn bộ tập đỉnh. 

Một trường hợp tách biệt tinh tế là```
3 2
1 2 3
1 3 1
```Lực lượng yêu cầu đầu tiên`3`bên trên`1`Và`2`. Lực lượng thứ hai`1`bên trên`3`. Như vậy mối quan hệ tổ tiên đã chứa đựng`1 -> 3 -> 1`. Trong quá trình lựa chọn gốc, không`1`cũng không`3`có thể được chọn làm gốc tối thiểu hợp lệ và cấu trúc sẽ từ chối thể hiện đó. 

Một trường hợp hữu ích khác là tập hợp các yêu cầu độc lập:```
5 1
1 2 3
```Đỉnh`4`Và`5`không bao giờ xảy ra trong bất kỳ yêu cầu nào. Thuật toán vẫn đặt chúng ở đâu đó trong cây vì mọi thành phần DSU đều được chuyển đổi đệ quy thành cây con. Đơn giản là chúng có thể trở thành những nhánh phụ. Vị trí của chúng không ảnh hưởng đến LCA cần thiết, đó là lý do tại sao các đỉnh không bị ràng buộc không bao giờ cần xử lý đặc biệt.
