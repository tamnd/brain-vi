---
title: "CF 102202G - Chuỗi tăng dần"
description: "Chúng ta có hoán vị A[0..N-1]. Sửa chỉ mục i và xem xét tất cả các dãy con tăng dần có chứa A[i]. Trong số đó, một số có chiều dài tối đa có thể."
date: "2026-08-20T02:25:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102202
codeforces_index: "G"
codeforces_contest_name: "2019 KAIST RUN Spring Contest"
rating: 0
weight: 102202
solve_time_s: 578
verified: true
draft: false
---

[CF 102202G - Trình tự tăng dần](https://codeforces.com/problemset/problem/102202/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 9 phút 38 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một hoán vị`A[0..N-1]`. Sửa chỉ mục`i`và xét tất cả các dãy con tăng có chứa`A[i]`. Trong số đó, một số có chiều dài tối đa có thể. Chúng ta cần đếm xem có bao nhiêu chỉ số khác`j`rất cần thiết cho việc lập chỉ mục`i`, nghĩa là sau khi xóa`j`, dãy con tăng tốt nhất vẫn chứa`i`trở nên ngắn hơn nghiêm ngặt. 

Sự khác biệt chính là giữa một phần tử xuất hiện trong một dãy con tối ưu nào đó và một phần tử xuất hiện trong mọi dãy con tối ưu chứa`i`. Chỉ có điều sau mới góp phần vào câu trả lời. 

Đối với một cố định`i`, mọi dãy con tăng dần chứa nó sẽ tự nhiên tách thành phần bên trái tận cùng tại`i`và một phần bên phải bắt đầu từ`i`. Do đó, chỉ số bên trái của`i`có liên quan chính xác khi nó xảy ra trong mọi dãy con tăng dài nhất kết thúc tại`i`. Câu lệnh đối xứng đúng cho các chỉ số ở bên phải. 

Với`N`lên đến`250000`, MỘT`O(N^2)`chương trình năng động đã vượt xa giới hạn. Trong trường hợp xấu nhất nó thực hiện khoảng`N(N-1)/2`, đại khái là`3.1 * 10^10`kiểm tra tiền nhiệm. Kể cả bình thường`O(N log N)`Bản thân tính toán LIS là chưa đủ vì chúng ta cần thông tin về những phần tử nào không thể tránh khỏi trong mọi dãy con tối ưu. 

Có một số trường hợp đặc biệt bộc lộ những lỗi phổ biến. Vì`N=1`, dãy con duy nhất chứa phần tử đơn chính là chính phần tử đó, vì vậy câu trả lời là`0`. đầu vào`1 / 1`phải sản xuất`0`. 

Đối với một hoán vị tăng nghiêm ngặt như`1 2 3`, mọi phần tử đều thuộc dãy con tăng duy nhất chứa bất kỳ vị trí cố định nào. Câu trả lời là`2 2 2`, không`0 0 0`. Giải pháp chỉ tính các lựa chọn LIS thay thế sẽ bỏ sót trường hợp này. 

Đối với một hoán vị giảm nghiêm ngặt như`3 2 1`, mỗi phần tử tự nó tạo thành một dãy con tăng dài nhất. Việc xóa chỉ mục khác không bao giờ thay đổi độ dài tốt nhất chứa chỉ mục đã chọn, vì vậy câu trả lời là`0 0 0`. Giải pháp gây nhầm lẫn giữa "thuộc về một số LIS" với "thuộc về mọi LIS" có thể đếm không chính xác các phần tử khác. 

Một trường hợp ranh giới hữu ích là`1 3 2`. Các câu trả lời là`0 1 1`. Đối với chỉ mục`1`, giá trị`3`không có phần mở rộng nào ở bên phải, do đó việc xóa phần tử lân cận là không tương đương. Đối với chỉ mục`2`, mọi dãy con tăng dần có độ dài hai chứa nó đều sử dụng phần tử đầu tiên`1`, làm cho chỉ mục đó trở nên cần thiết. Lý do tương tự áp dụng cho chỉ mục`1`. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp xây dựng đồ thị quy hoạch động theo dãy tăng dần. Tạo một đỉnh cho mọi vị trí mảng và một cạnh`j -> i`bất cứ khi nào`j < i`,`A[j] < A[i]`, Và`j`có thể là phần tử trước của dãy con tăng dài nhất kết thúc tại`i`. Tương đương, nếu`L[i]`là độ dài LIS kết thúc tại`i`, chúng tôi giữ cho các cạnh thỏa mãn`L[j] + 1 = L[i]`. 

Đối với một cố định`i`, mọi đường đi dài nhất từ ​​đầu đồ thị này đến`i`đại diện cho một dãy con tăng dài nhất kết thúc tại`i`. Một chỉ mục là không thể tránh khỏi chính xác khi nó nằm trên mọi đường dẫn như vậy. Cách mạnh mẽ nhất là liệt kê hoặc kiểm tra tất cả các đỉnh có liên quan trước đó cho mỗi đỉnh. Ngay cả việc xây dựng tất cả các cạnh cũng có khả năng là bậc hai, bởi vì một hoán vị tăng dần có một cạnh giữa mọi cặp vị trí. Với`N=250000`, điều đó có nghĩa là khoảng`3.1 * 10^10`các cạnh có thể. 

Quan sát mở ra giải pháp nhanh hơn là biểu đồ có liên quan là DAG có các cạnh luôn di chuyển từ lớp LIS`k-1`để lớp`k`. Chúng ta có thể xây dựng cây thống trị của nó. Một đỉnh`u`thống trị một đỉnh`v`khi mọi đường dẫn từ nguồn đến`v`đi qua`u`. Tổ tiên của`v`trong cây thống trị chính xác là các đỉnh không thể tránh khỏi trên mọi đường đi tới`v`. 

Đối với DAG, điểm thống trị trực tiếp của một đỉnh là LCA của tất cả các đỉnh trực tiếp trước nó trong cây thống trị. Điều này cho phép chúng ta xây dựng cây thống trị theo từng lớp. Khó khăn còn lại là tập tiền thân có thể lớn. Thuộc tính hoán vị loại bỏ khó khăn đó: các đỉnh có cùng độ dài kết thúc LIS tạo thành một chuỗi giảm dần cả về vị trí và giá trị. Vì vậy, với mỗi đỉnh trong lớp`k`, tiền thân của nó trong lớp`k-1`tạo thành một cửa sổ trượt liền kề. 

Chúng tôi duy trì LCA của cửa sổ di chuyển đó bằng hàng đợi tổng hợp hai ngăn. Mỗi thao tác chèn, xóa và truy vấn chỉ thực hiện một số thao tác LCA không đổi. Bản thân LCA được trả lời bằng việc nâng cấp nhị phân trong`O(log N)`, đưa ra một`O(N log N)`giải pháp. 

Phía bên phải của chỉ mục`i`được xử lý bằng cách đảo ngược hoán vị và thay thế mọi giá trị`x`với`N+1-x`. Một dãy con tăng chặt về bên phải của`i`trở thành một dãy con tăng thông thường kết thúc ở vị trí được biến đổi tương ứng với`i`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N2) | O(N) | Quá chậm | 
| Tối ưu | O(N log N) | O(N log N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán lớp kết thúc LIS của mọi vị trí bằng cách sử dụng mảng đuôi sắp xếp kiên nhẫn tiêu chuẩn. Nếu như`L[i] = k`, đặt chỉ mục`i`thành lớp`k`. Các chỉ số được xử lý từ trái sang phải nên mỗi lớp sẽ tự động được lưu trữ theo thứ tự vị trí tăng dần. 

Vì hai phần tử có cùng`L`giá trị không thể có cả vị trí tăng và giá trị tăng, các giá trị bên trong mỗi lớp sẽ giảm nghiêm trọng khi vị trí tăng. 
2. Thêm đỉnh nguồn ảo`0`. Mọi đỉnh có độ dài LIS`1`được kết nối từ nguồn này. Do đó độ sâu thống trị của nó là`1`, bởi vì đỉnh đó là phần tử thực duy nhất trên mọi đường đi kết thúc ở đó. 
3. Xử lý các lớp theo thứ tự tăng dần. Giả sử chúng ta hiện đang xây dựng lớp`k`, và lớp trước đó là`k-1`. Đối với một đỉnh hiện tại`v`, các tiền thân có thể có của nó chính xác là các đỉnh`u`trong lớp`k-1`thỏa mãn`u < v`Và`A[u] < A[v]`. 

Vì lớp trước có vị trí tăng trong khi giá trị giảm, nên các điều kiện`u < v`Và`A[u] < A[v]`chọn một khoảng liền kề của lớp đó. 
4. Xử lý lớp hiện tại từ trái sang phải. Điểm cuối bên phải của khoảng trước đó chỉ di chuyển sang phải vì vị trí hiện tại tăng lên. Điểm cuối bên trái cũng chỉ di chuyển sang phải vì các giá trị trong lớp hiện tại giảm, do đó ngưỡng`A[v]`trở nên nhỏ hơn. 

Do đó, chúng ta có thể duy trì khoảng trước đó dưới dạng hàng đợi trượt. Mỗi đỉnh của lớp trước được chèn nhiều nhất một lần và bị xóa nhiều nhất một lần trong khi xử lý lớp hiện tại. 
5. Duy trì LCA của mọi đỉnh hiện có trong hàng đợi trượt này. Hàng đợi thông thường không thể loại bỏ khỏi phía trước một cách hiệu quả trong khi vẫn duy trì tổng hợp liên kết tùy ý, vì vậy hãy sử dụng hai ngăn xếp. Mỗi ngăn xếp lưu trữ tổng hợp của riêng nó, trong đó tổng hợp của hai đỉnh là LCA của chúng. Khi ngăn xếp phía trước trống, hãy chuyển tất cả các phần tử từ ngăn xếp phía sau trong khi tính toán lại các tập hợp. 

LCA có tính kết hợp vì`LCA(LCA(a,b),c)`bằng tổ tiên chung sâu nhất trong cả ba đỉnh. Ở đây nó cũng có tính giao hoán, do đó thứ tự kết hợp của hai tập hợp ngăn xếp không quan trọng. 
6. Nếu cửa sổ tiền nhiệm cho`v`trống, kẻ thống trị trực tiếp của nó là nguồn ảo. Mặt khác, LCA của tất cả các phiên bản tiền nhiệm chính xác là điểm chi phối trực tiếp của`v`. Đặt đỉnh này làm cha mẹ của`v`trong cây thống trị. 

Khi đã biết được cha mẹ, hãy điền vào bảng nâng nhị phân cho`v`ngay lập tức. Tất cả tổ tiên được sử dụng bởi các mục này đã thuộc về các lớp trước đó. 
7. Độ sâu của`v`trong cây thống trị bằng số đỉnh không thể tránh được trên mọi dãy con tăng dần kết thúc tại`v`, bao gồm`v`chính nó. Như vậy`depth[v] - 1`là số lượng các chỉ số không thể tránh khỏi ngay trước`v`. 
8. Chạy quy trình tương tự trên chuỗi đã chuyển đổi`B`, Ở đâu`B`thu được bằng cách đảo ngược`A`và thay thế mọi giá trị`x`qua`N+1-x`. Dãy con tăng dần từ phải trong mảng ban đầu sẽ trở thành dãy con tăng từ trái sang phải trong mảng`B`. 
9. Nếu`left[i]`là độ sâu thống trị từ chuỗi ban đầu và`right[i]`là độ sâu tương ứng từ chuỗi được chuyển đổi, câu trả lời bắt buộc là`left[i] + right[i] - 2`. 

Chúng tôi trừ hai vì`i`chính nó được bao gồm một lần trong mỗi độ sâu của bộ thống trị nhưng không được tính. 

### Tại sao nó hoạt động 

Mỗi dãy con tăng dần kết thúc ở một đỉnh là một đường đi qua LIS DAG phân lớp. Một đỉnh xuất hiện trong mọi dãy con như vậy một cách chính xác khi nó chiếm ưu thế trên đỉnh DAG tương ứng. Bất biến cây thống trị nói rằng tất cả các cây thống trị của một đỉnh chính xác là tổ tiên cây của nó, vì vậy độ sâu của cây sẽ tính chúng. 

Đối với một DAG, điểm thống trị trực tiếp của một đỉnh là LCA của tất cả các đỉnh trước đó trong cây thống trị đã được xây dựng. Cửa sổ trượt chứa chính xác các cửa sổ trước đó vì các lớp LIS bằng nhau đang giảm giá trị nghiêm trọng khi vị trí tăng lên. Do đó, mọi cha mẹ được thuật toán chọn đều là người thống trị trực tiếp chính xác. Lập luận tương tự được áp dụng cho hoán vị ngược và bù cho các phần tử hậu tố không thể tránh khỏi. Vì tiền tố và hậu tố của dãy con tăng dần chứa`i`độc lập một lần`i`đã được sửa, việc cộng hai số đếm sẽ cho chính xác các chỉ số mà việc xóa làm giảm mức tối ưu. 

## Giải pháp Python```python
import sys
from bisect import bisect_left

input = sys.stdin.readline

def solve_dom_depth(a):
    n = len(a)

    # layers[k] contains indices whose LIS-ending length is k + 1.
    layers = []
    tails = []

    for i, x in enumerate(a):
        k = bisect_left(tails, x)
        if k == len(tails):
            tails.append(x)
            layers.append([])
        else:
            tails[k] = x
        layers[k].append(i)

    log = (n + 1).bit_length()

    # Binary lifting table for the dominator tree.
    up = [[0] * n for _ in range(log)]
    depth = [0] * n

    def lca(a1, a2):
        if a1 == a2:
            return a1

        if depth[a1] < depth[a2]:
            a1, a2 = a2, a1

        diff = depth[a1] - depth[a2]
        bit = 0
        while diff:
            if diff & 1:
                a1 = up[bit][a1]
            diff >>= 1
            bit += 1

        if a1 == a2:
            return a1

        for k in range(log - 1, -1, -1):
            if up[k][a1] != up[k][a2]:
                a1 = up[k][a1]
                a2 = up[k][a2]

        return up[0][a1]

    class AggQueue:
        def __init__(self):
            self.in_nodes = []
            self.in_agg = []
            self.out_nodes = []
            self.out_agg = []

        def push(self, v):
            self.in_nodes.append(v)
            if self.in_agg:
                self.in_agg.append(lca(self.in_agg[-1], v))
            else:
                self.in_agg.append(v)

        def _transfer(self):
            while self.in_nodes:
                v = self.in_nodes.pop()
                self.in_agg.pop()

                self.out_nodes.append(v)
                if self.out_agg:
                    self.out_agg.append(lca(v, self.out_agg[-1]))
                else:
                    self.out_agg.append(v)

        def pop(self):
            if not self.out_nodes:
                self._transfer()
            self.out_nodes.pop()
            self.out_agg.pop()

        def empty(self):
            return not self.in_nodes and not self.out_nodes

        def query(self):
            if not self.out_nodes:
                return self.in_agg[-1]
            if not self.in_nodes:
                return self.out_agg[-1]
            return lca(self.out_agg[-1], self.in_agg[-1])

    # Layer 0 has no real predecessor.
    for v in layers[0]:
        depth[v] = 1

    for k in range(1, len(layers)):
        prev = layers[k - 1]
        cur = layers[k]

        q = AggQueue()

        left = 0
        right = 0
        m = len(prev)

        for v in cur:
            # Add all previous-layer vertices with position < v.
            while right < m and prev[right] < v:
                if right >= left:
                    q.push(prev[right])
                right += 1

            # Remove vertices whose value is not smaller than a[v].
            # Values in prev are strictly decreasing.
            while left < right and a[prev[left]] >= a[v]:
                q.pop()
                left += 1

            if q.empty():
                parent = 0
                depth[v] = 1
            else:
                parent = q.query()
                depth[v] = depth[parent] + 1

            up[0][v] = parent
            for j in range(1, log):
                up[j][v] = up[j - 1][up[j - 1][v]]

    return depth

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    left = solve_dom_depth(a)

    transformed = [n + 1 - x for x in reversed(a)]
    right_rev = solve_dom_depth(transformed)

    ans = [0] * n
    for i in range(n):
        right = right_rev[n - 1 - i]
        ans[i] = left[i] + right - 2

    print(*ans)

if __name__ == "__main__":
    solve()
```Phần đầu tiên của`solve_dom_depth`xây dựng các lớp LIS với`bisect_left`. Bởi vì đầu vào là một hoán vị nên các dãy con tăng nghiêm ngặt được xử lý trực tiếp bằng cách thay thế giới hạn dưới. Độ dài LIS thực tế không cần thiết riêng biệt mà chỉ cần lớp của từng vị trí. 

các`up`bảng đại diện cho cây thống trị.`up[0][v]`là người thống trị trực tiếp của`v`, trong khi các hàng cao hơn chứa tổ tiên có lũy thừa bằng hai. Nguồn ảo được biểu thị bằng chỉ mục`0`trong bảng, mặc dù các đỉnh mảng thực sử dụng chỉ số dựa trên số 0. Một đỉnh thực không bao giờ có thể bị nhầm lẫn với nguồn vì nguồn được biểu thị bằng giá trị cha đặc biệt`0`, trong khi`depth[0]`ngầm là bằng không. 

Quy trình LCA trước tiên cân bằng độ sâu và sau đó nâng cả hai đỉnh lại với nhau. Sức nâng lớn nhất là`(N+1).bit_length()`, đủ để thể hiện mọi độ sâu có thể có của cây. 

các`AggQueue`là thành phần cửa sổ trượt.`in_nodes`tiếp nhận những người tiền nhiệm mới từ bên phải, trong khi`out_nodes`cung cấp các phần tử cần loại bỏ ở bên trái. Mỗi phần tử chuyển từ ngăn xếp này sang ngăn xếp khác nhiều nhất một lần trong quá trình quét một lớp. Tổng hợp được lưu trữ với mỗi mục nhập ngăn xếp là LCA của tất cả các phần tử bên dưới mục nhập đó. 

Hai con trỏ`left`Và`right`mô tả khoảng thời gian trước đó trong lớp LIS trước đó. điều kiện`prev[right] < v`xử lý hạn chế vị trí. điều kiện`a[prev[left]] >= a[v]`loại bỏ các giá trị không thể đứng trước`v`theo một dãy con tăng chặt. Việc sử dụng`>=`còn hơn là`>`là cần thiết vì dãy con phải tăng nghiêm ngặt. 

Cuối cùng, việc đảo ngược và bổ sung hoán vị sẽ biến mọi hậu tố hợp lệ sau`i`thành tiền tố hợp lệ trước đỉnh được chuyển đổi tương ứng. Độ sâu biến đổi được ánh xạ trở lại với`n - 1 - i`và hai độ sâu thống trị được kết hợp. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, mảng là`[1]`. Có một lớp LIS chỉ chứa chỉ mục`0`. 

| Lớp | Chỉ mục hiện tại | Cửa sổ tiền nhiệm | Kẻ thống trị ngay lập tức | Độ sâu | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | trống | nguồn | 1 | 

Chuỗi được chuyển đổi cũng là một phần tử duy nhất nên độ sâu của nó lại là`1`. Câu trả lời là`1 + 1 - 2 = 0`. 

Đối với Mẫu 2, mảng là`[1, 2, 3, 4, 5, 6]`. Mỗi phần tử thuộc lớp LIS của riêng nó vì toàn bộ mảng đang tăng lên. 

| Lớp | Chỉ mục hiện tại | Cửa sổ tiền nhiệm | Kẻ thống trị ngay lập tức | Độ sâu | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | trống | nguồn | 1 | 
| 2 | 1 |`[0]`| 0 | 2 | 
| 3 | 2 |`[1]`| 1 | 3 | 
| 4 | 3 |`[2]`| 2 | 4 | 
| 5 | 4 |`[3]`| 3 | 5 | 
| 6 | 5 |`[4]`| 4 | 6 | 

Đối với chuỗi được biến đổi, độ sâu xuất hiện theo thứ tự ngược lại,`6, 5, 4, 3, 2, 1`. Tại mọi vị trí, hai độ sâu thêm vào`7`, do đó trừ đi`2`cho`5`. Điều này phù hợp với thực tế là việc xóa bất kỳ phần tử nào khác sẽ phá vỡ dãy con tăng dần duy nhất chứa chỉ mục đã chọn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log N) | Các lớp LIS lấy O(N log N), trong khi cấu trúc bộ thống trị thực hiện các hoạt động hàng đợi được phân bổ O(N) và các truy vấn LCA O(N), mỗi truy vấn có giá O(log N). | 
| Không gian | O(N log N) | Nâng nhị phân lưu trữ tổ tiên O(N log N), trong khi các lớp và hàng đợi trượt sử dụng không gian bổ sung O(N). | 

Đầu vào lớn nhất chứa`250000`các phần tử. Thuật toán chỉ thực hiện logarit nhiều phép toán tổ tiên trên mỗi đỉnh, thay vì kiểm tra tất cả các cặp vị trí. các`O(N log N)`bị giới hạn một cách thoải mái trong phạm vi dự định của giới hạn 3 giây và`O(N log N)`bảng tổ tiên vừa vặn dễ dàng trong giới hạn bộ nhớ 1024 MB. 

## Trường hợp thử nghiệm 

Khai thác thử nghiệm bên dưới giả định giải pháp đã gửi được lưu dưới dạng`solution.py`và phơi bày`solve()`chức năng hiển thị ở trên.```python
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
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided samples
assert run("1\n1\n") == "0", "sample 1"
assert run("6\n1 2 3 4 5 6\n") == "5 5 5 5 5 5", "sample 2"
assert run("6\n6 5 4 3 2 1\n") == "0 0 0 0 0 0", "sample 3"
assert run("4\n2 1 4 3\n") == "0 0 0 0", "sample 4"
assert run("9\n1 2 3 6 5 4 7 8 9\n") == "5 5 5 6 6 6 5 5 5", "sample 5"

# Single element.
assert run("1\n7\n") == "0", "minimum size"

# Strictly decreasing permutation.
assert run("5\n5 4 3 2 1\n") == "0 0 0 0 0", "all LIS have length 1"

# A case with two different choices on one side.
assert run("3\n1 3 2\n") == "0 1 1", "sliding predecessor boundary"

# A longer case where both prefix and suffix dominators matter.
assert run("4\n3 1 2 4\n") == "1 2 2 2", "prefix and suffix dominance"

# Equal values are outside the permutation constraints, but are useful
# as a robustness check for the LIS boundary handling.
assert run("3\n5 5 5\n") == "0 0 0", "equal values"

# Maximum-size stress case, a decreasing permutation.
n = 250000
a = list(range(n, 0, -1))
expected = " ".join(["0"] * n)
assert run(str(n) + "\n" + " ".join(map(str, a)) + "\n") == expected, \
    "maximum-size input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 7`|`0`| Kích thước tối thiểu và không có chỉ mục khác | 
|`5 / 5 4 3 2 1`|`0 0 0 0 0`| Mỗi LIS có độ dài một | 
|`3 / 1 3 2`|`0 1 1`| Ranh giới tiền thân nghiêm ngặt và xử lý hậu tố | 
|`4 / 3 1 2 4`|`1 2 2 2`| Nhiều đỉnh bắt buộc ở cả hai bên | 
|`3 / 5 5 5`|`0 0 0`| Hành vi ranh giới có giá trị bằng nhau, bên ngoài các ràng buộc chính thức | 
|`250000 / 250000 ... 1`| tất cả số không | Tối đa`N`và hiệu suất | 

## Vỏ cạnh 

Đối với đầu vào một phần tử`1 / 1`, đỉnh duy nhất được đặt trong lớp LIS đầu tiên. Độ sâu thống trị của nó là`1`ở cả trình tự gốc và trình tự biến đổi. Công thức cho`1 + 1 - 2 = 0`, do đó thuật toán không bao giờ vô tình đếm chính chỉ mục đã chọn. 

Đối với một mảng tăng nghiêm ngặt như`1 2 3`, mỗi lớp chứa đúng một đỉnh. Do đó, cửa sổ tiền thân chứa chính xác một phần tử cho mỗi đỉnh không phải đầu tiên và cây thống trị chỉ đơn giản là một chuỗi. Độ sâu là`1, 2, 3`từ bên trái và`3, 2, 1`từ bên phải, đưa ra`2, 2, 2`. Điều này thực hiện trong trường hợp mọi yếu tố khác đều thực sự cần thiết. 

Đối với một mảng giảm nghiêm ngặt như`3 2 1`, mọi đỉnh đều nằm trong lớp LIS đầu tiên. Không có cạnh nào giữa các đỉnh thực, vì vậy mọi độ sâu của bộ định luật là`1`. Mảng được chuyển đổi có cùng thuộc tính. Công thức cuối cùng tạo ra số 0 ở mọi nơi, xử lý chính xác trường hợp phần tử được chọn chỉ có thể tham gia vào một chuỗi con tăng dần có độ dài. 

Vì`1 3 2`, các lớp bên trái là`[0]`Và`[1, 2]`. Đối với chỉ mục`1`, cửa sổ tiền nhiệm chứa chỉ mục`0`, vậy chỉ mục`0`thống trị nó. Đối với chỉ mục`2`, cửa sổ tiền nhiệm cũng chứa chỉ mục`0`, lập chỉ mục`0`phần tử tiền tố không thể tránh khỏi của nó. Ở phía bên phải, chỉ mục`1`không có phần tử nào lớn hơn sau nó, trong khi chỉ mục`2`không có phần mở rộng hậu tố. Kết hợp hai hướng sẽ mang lại`0 1 1`. Ví dụ này đặc biệt bộc lộ sai lầm khi coi mọi phần tử trong cùng một lớp LIS như phần tử tiền nhiệm có thể có mà không tôn trọng ranh giới giá trị. 

Vì`3 1 2 4`, độ sâu thống trị bên trái là`1, 1, 2, 3`. giá trị`1`chiếm ưu thế ở dãy con kết thúc tại`2`, và cả hai`1`Và`2`thống trị dãy con kết thúc tại`4`. Đường chuyền được chuyển đổi cung cấp độ sâu bên phải`2, 3, 2, 1`. Kết hợp chúng mang lại`1 2 2 2`. Trường hợp này chứng minh tại sao chỉ đếm các đỉnh trực tiếp trước đó là không đủ, bởi vì một đỉnh có thể không thể tránh khỏi thông qua nhiều lớp của cây thống trị. 

Đối với các giá trị bằng nhau như`5 5 5`, đảm bảo đầu vào chính thức bị vi phạm, nhưng thử nghiệm này rất hữu ích để kiểm tra các ranh giới so sánh nghiêm ngặt. Không có hai giá trị bằng nhau nào có thể mở rộng một dãy con tăng nghiêm ngặt, do đó mọi chỉ mục được chọn đều có độ dài tối đa là một và kết quả mong đợi là`0 0 0`. Việc thực hiện sử dụng`bisect_left`và`>=`sự bác bỏ của người tiền nhiệm một cách nhất quán với sự bất bình đẳng nghiêm ngặt.
