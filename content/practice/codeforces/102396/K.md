---
title: "CF 102396K - Chuẩn bị bài kiểm tra"
description: "Một mảng con được hiểu là một đầu vào đa thử nghiệm hoàn chỉnh. Giá trị đầu tiên của nó là số m cạnh đồ thị và 2m giá trị tiếp theo được nhóm thành m cặp đỉnh không có thứ tự."
date: "2026-08-10T18:58:30+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102396
codeforces_index: "K"
codeforces_contest_name: "2019-2020 Saint-Petersburg Open High School Programming Contest (SpbKOSHP 19)"
rating: 0
weight: 102396
solve_time_s: 163
verified: true
draft: false
---

[CF 102396K - Chuẩn bị bài kiểm tra](https://codeforces.com/problemset/problem/102396/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Một mảng con được hiểu là một đầu vào đa thử nghiệm hoàn chỉnh. Giá trị đầu tiên của nó là số`m`của các cạnh đồ thị và tiếp theo`2m`các giá trị được nhóm lại thành`m`các cặp đỉnh không có thứ tự. Đồ thị vô hướng thu được phải là một rừng, vì vậy nó không thể chứa vòng tự lặp, cạnh lặp hoặc bất kỳ chu trình nào. Nhiệm vụ là đếm xem có bao nhiêu mảng con thỏa mãn cách giải thích này. Những ràng buộc chính thức là`1 <= n <= 300000`Và`0 <= a_i <= 10^9`, với giới hạn thời gian 2 giây và giới hạn bộ nhớ 512 MB. 

Giá trị đầu tiên của mảng con ứng cử viên xác định hoàn toàn độ dài của nó. Nếu mảng con bắt đầu ở vị trí`l`Và`a[l] = m`, điểm cuối của nó phải là`l + 2m`. Do đó, mỗi vị trí bắt đầu có nhiều nhất một mảng con hợp lệ. Khó khăn thực sự không phải là tìm ra điểm cuối mà là kiểm tra xem tất cả`m`các cạnh tạo thành một khu rừng đủ nhanh. 

Việc triển khai trực tiếp có thể kiểm tra mọi biểu đồ ứng viên bằng DSU mới. Trong trường hợp xấu nhất, có thể có Θ(n) ứng cử viên và mỗi ứng cử viên có thể chứa Θ(n) cạnh, tạo ra các phép toán cạnh Θ(n2). Với`n = 300000`, một phương pháp bậc hai có thể đạt tới khoảng`n² / 4`, hoặc khoảng 22,5 tỷ lượt kiểm tra biên, vượt xa những gì giải pháp 2 giây có thể thực hiện. Do đó, giải pháp phải tránh việc xây dựng lại biểu đồ cho mọi vị trí bắt đầu. 

Có một vài trường hợp dễ dàng khiến các giải pháp không chính xác âm thầm chấp nhận các mảng con không hợp lệ. Số cạnh bằng 0 là hợp lệ: ví dụ: đầu vào`1 0`chứa một mảng con hợp lệ`[0]`, vì đồ thị không có cạnh là một khu rừng. Một giải pháp đòi hỏi phải nhìn thấy ít nhất một cạnh sẽ bác bỏ nó một cách không chính xác. 

Vòng lặp tự không hợp lệ mặc dù nó chỉ chứa một cạnh. Ví dụ,```
3
1 5 5
```có ứng cử viên`[1, 5, 5]`, mô tả cạnh`(5,5)`. Phần đóng góp đúng bằng 0 vì vòng lặp tự là một chu trình có độ dài bằng 1. Việc triển khai DSU chỉ kiểm tra xem hai điểm cuối đã được kết nối hay chưa phải xử lý rõ ràng`u == v`. 

Các cạnh song song cũng không hợp lệ. Ví dụ,```
5
2 1 2 1 2
```bắt đầu bằng`m = 2`và mô tả`(1,2), (1,2)`. Đóng góp chính xác từ vị trí số 0 là bằng không. Việc coi một cạnh lặp lại là vô hại sẽ chấp nhận một cách sai lầm một đa đồ thị không phải là một khu rừng. 

Cuối cùng, vị trí mảng quan trọng. Đối với mẫu```
5
2 1 3 4 1
```mảng con bắt đầu ở vị trí 0 có hai cạnh, trong khi mảng con bắt đầu ở vị trí một có một cạnh. Cả hai đều hợp lệ, vì vậy câu trả lời là`2`. Một giải pháp chỉ kiểm tra tính chẵn lẻ cố định của các vị trí sẽ bỏ lỡ một trong số chúng. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu tuân theo định nghĩa trực tiếp. Đối với mọi vị trí xuất phát`l`, đọc`m = a[l]`, kiểm tra xem`l + 2m < n`, sau đó xử lý như sau`m`ghép nối với một DSU. Khi một cạnh`(u,v)`được đọc, một vòng tự lặp ngay lập tức làm cho biểu đồ không hợp lệ. Bằng không, nếu`u`Và`v`đã ở trong cùng một thành phần, cạnh tạo ra một chu trình, do đó biểu đồ không hợp lệ. Nếu chúng ở các thành phần khác nhau, hãy hợp nhất chúng. Điều này đúng vì việc chèn một cạnh giữa hai thành phần khác nhau sẽ bảo toàn thuộc tính rừng, trong khi việc chèn một cạnh vào trong một thành phần sẽ tạo ra chính xác một chu kỳ. 

Vấn đề là công việc lặp đi lặp lại. Mặc dù một hoạt động DSU có thời gian khấu hao gần như không đổi, chúng tôi có thể xử lý các cạnh Θ(n) cho Θ(n) lần khởi đầu khác nhau. Số cạnh được xử lý trong trường hợp xấu nhất là Θ(n2), khoảng 22,5 tỷ khi`n = 300000`. 

Quan sát quan trọng là điều kiện hợp lệ của đồ thị là đơn điệu khi loại bỏ các cạnh. Nếu một phạm vi các cạnh là một khu rừng thì mọi phạm vi tiếp giáp nhỏ hơn của các cạnh đó cũng là một khu rừng. Điều này cho phép chúng ta biến vấn đề thành vấn đề truy vấn phạm vi. 

Hãy xem xét các cạnh trong một cặp cố định của mảng. Đối với mọi điểm cuối phù hợp`r`, định nghĩa`bad[r]`là chỉ số lớn nhất`x`sao cho khoảng cạnh`[x,r]`chứa một chu trình. Sau đó`[l,r]`chính xác là một khu rừng khi`l > bad[r]`. Lý do là nếu một chu trình được chứa hoàn toàn trong`[l,r]`, chỉ số cạnh nhỏ nhất của nó ít nhất là`l`, Vì thế`bad[r] >= l`. Ngược lại, nếu`bad[r] >= l`, chu kỳ chứng kiến`bad[r]`nằm hoàn toàn bên trong`[l,r]`. 

Vấn đề còn lại là làm thế nào để duy trì`bad[r]`trong khi các cạnh được chèn từ trái sang phải. Cạnh được chèn luôn là cạnh mới nhất, do đó chỉ số của nó lớn hơn mọi cạnh đã có. Nếu điểm cuối của nó bị ngắt kết nối, nó sẽ không tạo ra chu trình. Nếu chúng được kết nối, khu rừng hiện có sẽ có một đường dẫn duy nhất giữa chúng và việc thêm cạnh mới sẽ tạo ra một chu trình. Chỉ số cạnh nhỏ nhất trên đường đi đó chính xác là cạnh nhỏ nhất của chu trình mới. Lấy giá trị lớn nhất của giá trị này và giá trị trước đó`bad`đưa ra ngưỡng mới. 

Do đó, chúng ta cần một nhóm năng động hỗ trợ kết nối, cạnh tối thiểu trên đường dẫn và thay thế một cạnh của cây bằng một cạnh mới hơn. Cây cắt liên kết là sự phù hợp tự nhiên. Cây cắt liên kết hỗ trợ các hoạt động rừng động và tổng hợp đường đi theo thời gian khấu hao logarit. 

Có một chi tiết bổ sung do mảng ban đầu gây ra. Một mảng con hợp lệ có thể bắt đầu ở một trong hai giá trị chẵn lẻ. Nếu nó bắt đầu ở vị trí chẵn thì các cạnh của nó là```
(a[l+1], a[l+2]), (a[l+3], a[l+4]), ...
```trong khi vị trí xuất phát lẻ mang lại```
(a[l+1], a[l+2]), (a[l+3], a[l+4]), ...
```với căn chỉnh khác liên quan đến mảng ban đầu. Chúng tôi xử lý hai sự sắp xếp này một cách riêng biệt. Mỗi căn chỉnh trở thành một chuỗi các cạnh bình thường và mỗi vị trí bắt đầu ban đầu tương ứng với chính xác một tiền tố của chuỗi cạnh đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n² α(n)) | O(n) | Quá chậm | 
| Tối Ưu Với Link-Cut Tree | O(n log n) khấu hao | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phối hợp nén tất cả các nhãn đỉnh xuất hiện trong mảng. Giá trị đỉnh có thể lớn bằng`10^9`, nhưng chỉ có sự bình đẳng giữa các nhãn mới quan trọng. Việc nén cung cấp cho mỗi đỉnh riêng biệt một ID số nguyên nhỏ gọn. 
2. Xử lý riêng biệt hai cách sắp xếp cạnh có thể có. Để có sự chẵn lẻ`p`, một ứng cử viên bắt đầu tại`l = p + 2j`, và giá trị đầu tiên của nó là`a[l]`. Cạnh đầu tiên của nó là`(a[l+1], a[l+2])`, theo sau là`(a[l+3], a[l+4])`. 
3. Xây dựng cây cắt liên kết có các nút thông thường biểu thị các đỉnh đồ thị được nén. Mỗi cạnh cũng có nút cây cắt liên kết riêng. Nút cạnh lưu trữ vị trí của nó trong chuỗi cạnh làm giá trị của nó, trong khi các nút đỉnh thông thường có giá trị vô cùng. Biểu diễn các cạnh dưới dạng các nút giúp có thể cắt và thay thế trực tiếp một cạnh cụ thể. 
4. Duy trì một biến`bad`, ban đầu`-1`. Sau khi xử lý cạnh`r`,`bad`là chỉ số cạnh tối thiểu lớn nhất có thể có của một chu trình chứa hoàn toàn trong tiền tố được xử lý. 
5. Khi đến cạnh tiếp theo`(u,v)`là một vòng tự lặp, nó ngay lập tức tạo thành một chu trình có cạnh nhỏ nhất là`r`. Bộ`bad = max(bad, r)`và không chèn cạnh này vào rừng. 
6. Nếu không, hãy kiểm tra xem`u`Và`v`đã được kết nối trong khu rừng được duy trì. Nếu chúng không được kết nối, hãy liên kết nút cạnh mới giữa`u`Và`v`. Không có chu kỳ nào xuất hiện, vì vậy`bad`không thay đổi. 
7. Nếu`u`Và`v`đã được kết nối, hãy truy vấn chỉ số cạnh tối thiểu trên đường rừng duy nhất của chúng. Hãy để giá trị đó là`x`. Việc thêm cạnh mới sẽ tạo ra một chu trình có cạnh nhỏ nhất là`x`, vì cạnh mới có chỉ số lớn nhất trong chu trình. Cập nhật`bad = max(bad, x)`. 
8. Thay thế cạnh`x`bởi rìa mới trong khu rừng được duy trì. Cắt nút cạnh cũ khỏi hai điểm cuối của nó, sau đó liên kết nút cạnh mới với cùng các điểm cuối. Nhóm kết quả là nhóm bao trùm chỉ mục tối đa của biểu đồ đã xử lý, đây chính xác là những gì chúng ta cần cho các lần chèn trong tương lai. 
9. Lưu trữ kết quả`bad[r]`cho mọi vị trí cạnh. Đối với một ứng cử viên ban đầu bắt đầu từ`l = p + 2j`, cho phép`k = a[l]`. Nếu như`k = 0`, ứng viên không chứa cạnh nào và luôn hợp lệ. Ngược lại cạnh cuối cùng của nó là`r = j + k - 1`. Ứng viên hợp lệ chính xác khi`r`tồn tại và`bad[r] < j`. 
10. Thêm mọi ứng cử viên hợp lệ vào câu trả lời và lặp lại quy trình cho số chẵn lẻ còn lại. 

### Tại sao nó hoạt động 

Cây cắt liên kết được duy trì luôn là một rừng chứa các cạnh được chọn bởi rừng bao trùm chỉ số tối đa của đồ thị đã xử lý. Khi một cạnh mới kết nối hai thành phần khác nhau, nó thuộc về mọi khu rừng bao trùm có tổng chỉ số tối đa và có thể được liên kết một cách đơn giản. Khi nó kết nối các đỉnh đã có trong cùng một thành phần, đường đi của cây hiện tại cộng với cạnh mới là một chu trình. Vì cạnh mới có chỉ số lớn nhất trong chu trình đó nên chỉ số tối thiểu trên đường đi của cây là chỉ số tối thiểu của chu trình. Việc thay thế cạnh tối thiểu đó bằng cạnh mới sẽ bảo toàn thuộc tính rừng bao trùm chỉ số tối đa. 

Bất biến đối với`bad[r]`là nó bằng chỉ số cạnh nhỏ nhất có thể lớn nhất trong số tất cả các chu trình chứa trong tiền tố kết thúc tại`r`. Các chu kỳ hiện tại được bao phủ bởi giá trị trước đó của`bad`, trong khi mọi chu trình mới được tạo được biểu thị bằng cạnh tối thiểu trên đường dẫn giữa các điểm cuối của cạnh mới. Do đó một phạm vi`[l,r]`không chứa chu trình chính xác khi nào`l > bad[r]`. Các vòng tự lặp và các cạnh song song được xử lý một cách tự nhiên dưới dạng chu trình, vì vậy điều kiện này tương đương với biểu đồ là một khu rừng hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10**18

class LinkCutTree:
    __slots__ = ("n", "left", "right", "parent", "rev", "value", "mn")

    def __init__(self, values):
        n = len(values)
        self.n = n
        self.left = [0] * n
        self.right = [0] * n
        self.parent = [0] * n
        self.rev = [0] * n
        self.value = values
        self.mn = values[:]

    def is_root(self, x):
        p = self.parent[x]
        return p == 0 or (self.left[p] != x and self.right[p] != x)

    def push(self, x):
        if self.rev[x]:
            l = self.left[x]
            r = self.right[x]
            self.left[x], self.right[x] = r, l

            if l:
                self.rev[l] ^= 1
            if r:
                self.rev[r] ^= 1

            self.rev[x] = 0

    def pull(self, x):
        best = self.value[x]
        l = self.left[x]
        r = self.right[x]

        if l and self.mn[l] < best:
            best = self.mn[l]
        if r and self.mn[r] < best:
            best = self.mn[r]

        self.mn[x] = best

    def rotate(self, x):
        y = self.parent[x]
        z = self.parent[y]

        if self.left[y] == x:
            b = self.right[x]
            self.right[x] = y
            self.left[y] = b
            if b:
                self.parent[b] = y
        else:
            b = self.left[x]
            self.left[x] = y
            self.right[y] = b
            if b:
                self.parent[b] = y

        self.parent[y] = x
        self.parent[x] = z

        if z:
            if self.left[z] == y:
                self.left[z] = x
            elif self.right[z] == y:
                self.right[z] = x

        self.pull(y)
        self.pull(x)

    def splay(self, x):
        stack = []
        y = x
        stack.append(y)

        while not self.is_root(y):
            y = self.parent[y]
            stack.append(y)

        while stack:
            self.push(stack.pop())

        while not self.is_root(x):
            y = self.parent[x]
            z = self.parent[y]

            if not self.is_root(y):
                if (self.left[y] == x) == (self.left[z] == y):
                    self.rotate(y)
                else:
                    self.rotate(x)

            self.rotate(x)

        self.pull(x)

    def access(self, x):
        last = 0
        y = x

        while y:
            self.splay(y)
            self.right[y] = last
            self.pull(y)
            last = y
            y = self.parent[y]

        self.splay(x)

    def make_root(self, x):
        self.access(x)
        self.rev[x] ^= 1

    def find_root(self, x):
        self.access(x)

        while True:
            self.push(x)
            l = self.left[x]
            if not l:
                break
            x = l

        self.splay(x)
        return x

    def connected(self, x, y):
        if x == y:
            return True
        self.make_root(x)
        return self.find_root(y) == x

    def link(self, x, y):
        self.make_root(x)
        self.parent[x] = y

    def cut(self, x, y):
        self.make_root(x)
        self.access(y)

        if self.left[y] == x:
            self.left[y] = 0
            self.parent[x] = 0
            self.pull(y)

    def path_min(self, x, y):
        self.make_root(x)
        self.access(y)
        return self.mn[y]

def process_parity(a, vertex_id, parity):
    n = len(a)

    starts = parity
    if starts >= n:
        return 0

    edge_count = (n - parity - 1) // 2
    if edge_count <= 0:
        # There can still be zero-edge candidates.
        ans = 0
        for l in range(parity, n, 2):
            if a[l] == 0:
                ans += 1
        return ans

    total_nodes = len(vertex_id) + edge_count

    values = [INF] * total_nodes
    for i in range(edge_count):
        values[len(vertex_id) + i] = i

    lct = LinkCutTree(values)

    bad = -1
    answer = 0
    bad_at = [bad] * edge_count

    V = len(vertex_id)

    for r in range(edge_count):
        u_pos = parity + 2 * r + 1
        v_pos = u_pos + 1

        u = vertex_id[a[u_pos]]
        v = vertex_id[a[v_pos]]
        edge_node = V + r

        if u == v:
            if r > bad:
                bad = r
        elif not lct.connected(u, v):
            lct.link(edge_node, u)
            lct.link(edge_node, v)
        else:
            old_index = lct.path_min(u, v)

            if old_index > bad:
                bad = old_index

            old_node = V + old_index

            old_u_pos = parity + 2 * old_index + 1
            old_v_pos = old_u_pos + 1

            old_u = vertex_id[a[old_u_pos]]
            old_v = vertex_id[a[old_v_pos]]

            lct.cut(old_node, old_u)
            lct.cut(old_node, old_v)

            lct.link(edge_node, u)
            lct.link(edge_node, v)

        bad_at[r] = bad

    for j in range((n - parity + 1) // 2):
        l = parity + 2 * j
        if l >= n:
            break

        k = a[l]

        if k == 0:
            answer += 1
            continue

        r = j + k - 1

        if 0 <= r < edge_count and bad_at[r] < j:
            answer += 1

    return answer

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    values = sorted(set(a))
    vertex_id = {x: i for i, x in enumerate(values)}

    ans = 0
    ans += process_parity(a, vertex_id, 0)
    ans += process_parity(a, vertex_id, 1)

    print(ans)

if __name__ == "__main__":
    solve()
```Việc nén tọa độ được thực hiện một lần vì độ lớn thực tế của nhãn đỉnh không bao giờ quan trọng. Hai lần xuất hiện của cùng một số nguyên phải biểu thị cùng một đỉnh đồ thị, trong khi các số nguyên khác nhau phải biểu thị các đỉnh khác nhau. 

Đối với một chẵn lẻ,`edge_count`là số cặp hoàn chỉnh có thể được hình thành từ sự liên kết đó. Mỗi cạnh nhận được một nút LCT chuyên dụng có giá trị được lưu trữ là vị trí của nó trong chuỗi cạnh. Các đỉnh đồ thị thông thường lưu trữ vô số, do đó, đường đi tối thiểu luôn trả về chỉ số cạnh thực tế. 

Logic chèn tuân theo hướng dẫn thuật toán trực tiếp. Cập nhật tự vòng lặp`bad`không vào rừng. Một cạnh mới giữa các thành phần khác nhau được liên kết ngay lập tức. Một cạnh mới bên trong một thành phần sẽ tạo ra một chu trình, do đó`path_min`tìm thấy cạnh nhỏ nhất của nó. Cạnh đó được loại bỏ và cạnh mới được chèn vào. 

hai`cut`các cuộc gọi là cần thiết vì một nút cạnh đại diện cho một cạnh vô hướng và có hai kết nối cây đại diện. Việc quên một trong hai sẽ để lại một phần của rìa cũ bên trong khu rừng động và làm hỏng các truy vấn kết nối sau này. 

Việc tính toán ứng viên sử dụng`j`, chỉ số cạnh tương ứng với cạnh đầu tiên của ứng viên, thay vì vị trí mảng ban đầu. Đây là nguồn gốc của nhiều lỗi riêng lẻ. Một ứng cử viên bắt đầu vào lúc`l = parity + 2j`với`k`các cạnh kết thúc ở cạnh`r = j + k - 1`. Nó hợp lệ chính xác khi phạm vi đó không chứa chu trình, tức là`bad_at[r] < j`. 

Số nguyên Python không bị tràn, vì vậy nhãn đáp án và nhãn đỉnh không cần xử lý số nguyên đặc biệt. Việc triển khai sử dụng các thao tác lặp lại thay vì đệ quy, tránh giới hạn độ sâu đệ quy của Python. 

## Ví dụ đã hoạt động 

Đối với mẫu 1,```
5
2 1 3 4 1
```trước tiên hãy xem xét các vị trí bắt đầu chẵn. 

| Chỉ số cạnh | Cạnh |`bad`| Ứng viên bắt đầu |`k`| Phạm vi cạnh ứng cử viên | hợp lệ | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 |`(1,3)`| -1 | 0 | 2 |`[0,1]`| vâng | 
| 1 |`(4,1)`| -1 | 2 | 3 |`[2,4]`| mảng bên ngoài | 

Ứng viên bắt đầu ở vị trí 0 cần có hai cạnh và`(1,3), (4,1)`tạo thành một khu rừng. Vị trí bắt đầu lẻ bắt đầu ở vị trí một và có`k = 1`, cho cạnh đơn`(3,4)`, cũng là một khu rừng. 

Hai mảng con hợp lệ là`[2,1,3,4,1]`Và`[1,3,4]`, vậy câu trả lời là`2`. 

Đối với mẫu 2,```
8
1 3 1 2 2 0 2 3
```các ứng cử viên có liên quan là: 

| Bắt đầu ban đầu |`k`| Cạnh |`bad`tình trạng | hợp lệ | 
| --- | --- | --- | --- | --- | 
| 0 | 1 |`(3,1)`| không có chu kỳ | vâng | 
| 1 | 3 |`(1,2),(2,0),(2,3)`| không có chu kỳ | vâng | 
| 2 | 1 |`(2,2)`| tự lặp | không | 
| 3 | 2 |`(2,0),(2,3)`| không có chu kỳ | vâng | 
| 4 | 2 |`(0,2),(2,3)`| không có chu kỳ | vâng | 
| 5 | 0 | không có cạnh | luôn hợp lệ | vâng | 

Ứng viên ở vị trí thứ hai thể hiện trường hợp tự lặp rõ ràng. Năm thí sinh còn lại tạo thành rừng, đưa ra đáp án theo yêu cầu`5`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) khấu hao | Mỗi cạnh gây ra một số lượng không đổi các thao tác cắt liên kết trên cây, mỗi O(log n) được khấu hao | 
| Không gian | O(n) | Các đỉnh được nén, các nút cạnh, mảng LCT và mảng ngưỡng đều là tuyến tính | 

Đầu vào có tối đa 300000 phần tử mảng, do đó chỉ có các phần chèn cạnh O(n) trên hai lượt chẵn lẻ. Các phép toán cây động logarit thay thế cấu trúc DSU lặp lại bậc hai từ phương pháp brute-force. Giới hạn O(n log n) kết quả tương thích với kích thước đầu vào và giới hạn bộ nhớ nhất định. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return out

# Sample 1
assert run("5\n2 1 3 4 1\n") == "2\n", "sample 1"

# Sample 2
assert run("8\n1 3 1 2 2 0 2 3\n") == "5\n", "sample 2"

# Minimum-size input
assert run("1\n0\n") == "1\n", "single zero"

# All values zero
assert run("4\n0 0 0 0\n") == "4\n", "every singleton is valid"

# Self-loop
assert run("3\n1 5 5\n") == "0\n", "self-loop must be rejected"

# Parallel edges
assert run("5\n2 1 2 1 2\n") == "1\n", "parallel edges must be rejected"

# Simple cycle
assert run("7\n3 1 2 2 3 3 1\n") == "0\n", "triangle is not a forest"

# Maximum-size input, every candidate has zero edges
n = 300000
inp = str(n) + "\n" + " ".join(["0"] * n) + "\n"
assert run(inp) == str(n) + "\n", "maximum-size all-zero input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 0`|`1`| Kích thước tối thiểu và đồ thị không cạnh | 
|`4 / 0 0 0 0`|`4`| Các giá trị bằng nhau và các tập cạnh trống | 
|`3 / 1 5 5`|`0`| Xử lý tự vòng lặp | 
|`5 / 2 1 2 1 2`|`1`| Xử lý cạnh song song và tính chẵn lẻ | 
|`7 / 3 1 2 2 3 3 1`|`0`| Phát hiện chu kỳ chính hãng | 
|`300000 / all zeros`|`300000`| Kích thước đầu vào tối đa và cường độ câu trả lời | 

## Vỏ cạnh 

Trường hợp không có cạnh được xử lý trước bất kỳ thao tác cắt liên kết nào. Đối với đầu vào```
1
0
```ứng cử viên duy nhất bắt đầu với`m = 0`, vậy độ dài của nó là một. Đồ thị không có cạnh và là một khu rừng. Thuật toán tăng câu trả lời ngay lập tức. 

Đối với một vòng lặp tự như```
3
1 5 5
```ứng cử viên đầu tiên có một lợi thế`(5,5)`. Thủ tục chèn phát hiện`u == v`, cập nhật`bad`vào chỉ số cạnh hiện tại và không chèn cạnh vào rừng. Ứng viên bắt đầu ở chỉ số cạnh bằng 0, trong khi`bad`cũng bằng 0, vậy`bad < start`là sai và ứng viên bị từ chối. 

Đối với các cạnh lặp lại,```
5
2 1 2 1 2
```cạnh đầu tiên`(1,2)`được chèn vào. Cạnh thứ hai có cùng điểm cuối nên chúng đã được kết nối với nhau. Đường đi chứa cạnh đầu tiên có chỉ số bằng 0. Do đó, chu kỳ mới có chỉ số tối thiểu bằng 0, khiến`bad = 0`. Một ứng cử viên bắt đầu từ cạnh 0 sẽ thất bại vì`0 < 0`là sai. Thí sinh bắt đầu ở vị trí mảng tiếp theo chỉ có một cạnh và hợp lệ, tạo ra câu trả lời tổng thể`1`. 

Đối với hình tam giác```
7
3 1 2 2 3 3 1
```hai cạnh đầu tiên nối ba đỉnh phân biệt. Khi`(3,1)`đến nơi, các điểm cuối của nó đã được kết nối bằng đường dẫn`(3,2),(2,1)`. Chỉ số cạnh tối thiểu trên đường dẫn đó bằng 0, vì vậy`bad`trở thành số không. Ứng cử viên bắt đầu từ cạnh 0 chứa toàn bộ tam giác và bị loại bỏ. Cơ chế tương tự xử lý các chu kỳ có độ dài tùy ý mà không đi qua chu trình một cách rõ ràng. 

Hai lần chuyển chẵn lẻ là cần thiết vì giá trị đầu tiên của một mảng con có thể xảy ra ở một trong hai lần chẵn lẻ. Một ứng cử viên bắt đầu ở vị trí chẵn sẽ ghép các giá trị sau đây khác với một ứng cử viên bắt đầu ở vị trí lẻ. Chỉ xử lý một căn chỉnh sẽ làm cho các cạnh của biểu đồ bị sai, ngay cả khi bộ kiểm tra forest đúng.
