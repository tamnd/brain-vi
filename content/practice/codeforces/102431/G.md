---
title: "CF 102431G - Trò chơi trên cây"
description: "Chúng ta có một cây có gốc ở đỉnh 1, với mã thông báo ban đầu ở đỉnh 1. Gấu trúc di chuyển trước. Ở mỗi lượt sau lượt đầu tiên, người chơi phải di chuyển mã thông báo xa hơn đối thủ đã di chuyển ở lượt trước. Người chơi không có nước đi hợp pháp sẽ thua."
date: "2026-08-08T23:51:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102431
codeforces_index: "G"
codeforces_contest_name: "2019 China Collegiate Programming Contest Final (CCPC-Final 2019)"
rating: 0
weight: 102431
solve_time_s: 247
verified: true
draft: false
---

[CF 102431G - Trò chơi trên cây](https://codeforces.com/problemset/problem/102431/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4 phút 7 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có gốc ở đỉnh 1, với mã thông báo ban đầu ở đỉnh 1. Gấu trúc di chuyển trước. Ở mỗi lượt sau lượt đầu tiên, người chơi phải di chuyển mã thông báo xa hơn đối thủ đã di chuyển ở lượt trước. Người chơi không có nước đi hợp pháp sẽ thua. 

Cừu được phép xóa các cạnh và đỉnh một cách gián tiếp bằng cách chọn bất kỳ đồ thị con liên thông nào chứa đỉnh 1. Vì đồ thị gốc là một cây nên mỗi đồ thị con như vậy tự nó là một cây chứa gốc. Chúng ta cần đếm xem có bao nhiêu đồ thị con được kết nối có gốc này đang giành được vị trí cho Cừu, modulo (10^9+7). Giải pháp chính thức giảm trò chơi về vị trí tâm đường kính của cây, sau đó đếm các đồ thị con có gốc mà đỉnh 1 là tâm đó. 

Đầu vào chứa tối đa (2\cdot10^5) đỉnh cho mỗi trường hợp thử nghiệm và tối đa mười trường hợp thử nghiệm. Không có sự phụ thuộc hữu ích vào trọng số cạnh hoặc các tham số số nhỏ khác, do đó, thuật toán bậc hai theo (n) đã quá đắt. Cụ thể, một DP quét một mảng độ sâu có độ dài (O(n)) ở mọi đỉnh có thể thực hiện công việc (O(n^2)) trên một cây dài. Chúng ta cần làm cho tổng lượng DP độ sâu hoạt động gần tuyến tính hoặc tệ nhất là (O(n\log n)). 

Có ba trường hợp ranh giới đáng được quan tâm. 

Đối với một đỉnh duy nhất, đồ thị con duy nhất có thể có là đỉnh 1. Đường kính của nó có chiều dài bằng 0 và tâm của nó là đỉnh 1, vì vậy câu trả lời là 1. 

Đối với hai đỉnh, đồ thị con chiến thắng duy nhất là đồ thị đơn ({1}). Đồ thị con chứa cả hai đỉnh có đường kính gồm một cạnh, tâm của nó nằm ở giữa cạnh đó chứ không phải đỉnh 1. Do đó```
1
2
1 2
```sản xuất```
Case #1: 1
```Một đường đi cũng có thể bị hiểu sai nếu chúng ta chỉ nhìn vào độ sâu tối đa của nó từ gốc. Vì```
1
3
1 2
2 3
```ba đồ thị con có gốc có thể có là ({1}), ({1,2}) và ({1,2,3}). Chỉ có đỉnh đơn có đỉnh 1 là tâm đường kính của nó, vì vậy câu trả lời là 1. Việc thực hiện bất cẩn coi gốc là tâm bất cứ khi nào nó là một trong những đỉnh sâu nhất sẽ đếm sai nhiều hơn. 

Một ngôi sao cho ranh giới đối diện. Vì```
1
4
1 2
1 3
1 4
```bất kỳ đồ thị con nào chứa đỉnh 1 và ít nhất hai lá có đường kính 2 và tâm 1. Có bốn đồ thị con không đơn lẻ như vậy, tương ứng với ba cặp lá và tập hợp cả ba lá. Cùng với câu trả lời đơn lẻ, câu trả lời là 5. Trường hợp này cho thấy sự khác biệt giữa "độ sâu tối đa xảy ra" và "độ sâu tối đa xảy ra ở ít nhất hai nhánh gốc khác nhau". 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp có thể liệt kê mọi đồ thị con được kết nối chứa đỉnh 1, xác định đường kính của nó và kiểm tra xem đỉnh 1 có phải là tâm hay không. Có rất nhiều ứng cử viên theo cấp số nhân. Ngay cả khi sử dụng giới hạn trên lỏng lẻo hơn của các tập hợp con cạnh (2^{n-1}), việc kiểm tra từng ứng cử viên trong (O(n)) vẫn cho kết quả (O(n2^{n-1})). Với (n=2\cdot10^5), điều này hoàn toàn không thể xảy ra. 

Quan sát hữu ích đầu tiên là quy tắc di chuyển phức tạp có đặc điểm lý thuyết trò chơi đơn giản đến mức đáng ngạc nhiên. Hãy xem xét bất kỳ cây cố định nào và đường kính của nó. Nếu mã thông báo ban đầu nằm ở tâm đường kính, người chơi thứ hai sẽ có chiến lược phản chiếu. Bất cứ khi nào người chơi thứ nhất di chuyển đến một đỉnh nào đó ở khoảng cách (d) tính từ tâm, người chơi thứ hai có thể di chuyển đến một đỉnh ở cùng khoảng cách ở phía đối diện của tâm. Khoảng cách di chuyển bắt buộc phải khớp và sự bất bình đẳng nghiêm ngặt buộc trò chơi cuối cùng hướng tới điểm cuối đường kính. Phân tích chính thức đưa ra chính xác đặc điểm đường kính-tâm này. 

Nếu đỉnh bắt đầu không phải là tâm đường kính, người chơi đầu tiên có thể di chuyển về phía tâm và giành được lợi thế tương ứng. Do đó, Cừu thắng chính xác khi đỉnh 1 là tâm đường kính của đồ thị con đã chọn. 

Bây giờ trò chơi đã biến mất. Vấn đề hoàn toàn mang tính tổ hợp: đếm các cây con có gốc được kết nối có đường kính tập trung ở đỉnh 1. 

Gốc của cây ban đầu ở đỉnh 1. Xét một cây con liên thông được chọn chứa gốc. Đặt độ sâu tối đa của nó từ đỉnh 1 là (D). Đỉnh 1 là đường kính tâm chính xác khi độ sâu (D) đạt được ở ít nhất hai cây con khác nhau của đỉnh 1. 

Tại sao? Nếu hai nhánh khác nhau đều chứa các đỉnh ở độ sâu (D), thì hai đỉnh đó cách nhau một khoảng (2D) tính từ gốc. Bất kỳ đường đi nào hoàn toàn bên trong một nhánh đều có chiều dài tối đa (2D-2), do đó đường kính đi qua đỉnh 1 và tâm của nó chính xác là đỉnh 1. Ngược lại, nếu chỉ có một nhánh gốc đạt tới độ sâu (D) thì đường kính có chiều dài (2D) không thể hình thành được qua gốc, do đó gốc không phải là tâm. 

Do đó, chúng ta cần một cây DP đếm các cây con được kết nối chứa mỗi đỉnh, được phân loại theo độ sâu tối đa của chúng. 

Xác định (f_u[i]) là số cây con được kết nối bên trong cây con của (u), chứa (u), có độ sâu tối đa được đo từ (u) chính xác là (i). Đối với một chiếc lá, (f_u[0]=1). 

Giả sử chúng ta đã xử lý một số phần tử con của (u), được biểu thị bằng một mảng (A_i) và bây giờ thêm một phần tử con (v), có mảng tương ứng là (B_i). Một cây con mới có độ sâu tối đa là (i) có thể phát sinh theo hai cách. Phần con mới đạt độ sâu (i), trong khi phần trước có độ sâu tối đa (i), hoặc phần trước đạt độ sâu (i), trong khi phần con mới có độ sâu nhỏ hơn (i). Điều này mang lại sự tái diễn theo điểm 

B_i\left(1+\sum_{j=0}^{i}A_j\right) 
+ 
A_i\left(1+\sum_{j=0}^{i-1}B_j\right). 
] 

Sự tái phát rất đơn giản, nhưng việc đánh giá nó một cách độc lập đối với từng đứa trẻ vẫn còn quá chậm. Quan sát cấu trúc quan trọng là chúng ta chỉ cần xử lý các mảng con ngắn hơn một cách rõ ràng. Với mỗi đỉnh, chúng ta chọn một con có chiều cao tối đa là con nặng nhất của nó. Mảng DP được lưu trữ dọc theo chuỗi nặng này và được chia sẻ giữa các đỉnh của nó. Mảng của ánh sáng con sau đó được hợp nhất vào mảng hiện tại theo thời gian tỷ lệ thuận với chiều cao của ánh sáng con. Đây là sự phân rã chuỗi dài được sử dụng bởi các giải pháp được chấp nhận cho vấn đề này.

Hậu tố của mảng DP hiện tại nằm ngoài độ sâu tối đa của ánh sáng con chỉ cần nhân với một vô hướng. Chúng tôi lưu trữ phép nhân này một cách lười biếng. Khi một vị trí mảng cuối cùng được truy cập, phép nhân đang chờ xử lý của nó sẽ được đẩy sang vị trí tiếp theo. Vì vị trí DP được sử dụng từ độ sâu nhỏ đến độ sâu lớn nên mỗi thẻ lười chỉ di chuyển về phía trước dọc theo chuỗi. 

Sau khi tính toán tất cả (f_u), gốc được xử lý riêng. Với mọi con (c) của gốc, xác định 

[ 
a_c(D)=f_c[D-1], 
] 

tính các lựa chọn không trống trong nhánh (c) có độ sâu gốc tối đa chính xác là (D). Đồng thời xác định 

[ 
b_c(D)=1+\sum_{j=0}^{D-2}f_c[j], 
] 

trong đó số 1 bổ sung thể hiện việc không chọn gì từ nhánh đó. Do đó (b_c(D)) tính các lựa chọn nhánh có độ sâu tối đa hoàn toàn nhỏ hơn (D). 

Đối với độ sâu cố định (D), sản phẩm 

[ 
P_D=\prod_c (a_c(D)+b_c(D)) 
] 

đếm tất cả các cây con có gốc có độ sâu tối đa lớn nhất (D). Do đó (P_D-P_{D-1}) đếm những người có độ sâu tối đa chính xác là (D). 

Trong số này, chúng ta phải loại bỏ các cấu hình trong đó chính xác một nhánh gốc đạt đến độ sâu (D). Xác định 

[ 
Q_D=\sum_c a_c(D)\prod_{j\ne c}b_j(D). 
] 

Sau đó 

[ 
P_D-P_{D-1}-Q_D 
] 

chính xác là số lượng cấu hình có độ sâu tối đa (D) đạt được trong ít nhất hai nhánh gốc. 

Chúng ta duy trì (P_D), (\prod b_c(D)) và (Q_D) với một cây phân đoạn trên các nút con của gốc. Mỗi nhánh chỉ thay đổi (a_c,b_c) một lần cho mỗi độ sâu và tổng số thay đổi đó tối đa bằng tổng chiều cao của nhánh, là (O(n)). Cây phân đoạn thực hiện mọi thay đổi (O(\log n)). 

Căn bậc một được xử lý riêng biệt, do đó câu trả lời cuối cùng là một cộng với tổng các cấu hình hợp lệ cho tất cả các độ sâu dương. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n2^{n-1})) | (O(n)) | Quá chậm | 
| Cây thông thường DP | (O(n^2)) | (O(n^2)) trong trường hợp xấu nhất | Quá chậm | 
| Tập hợp gốc DP + chuỗi dài | (O(n\log n)) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Lấy gốc cây ở đỉnh 1 và xây dựng thứ tự duyệt cha-con. Việc xử lý ngược thứ tự này sẽ mang lại DP của mọi đứa trẻ trước khi cha mẹ của nó được xử lý. 
2. Tính chiều cao của mỗi đỉnh và chọn con nặng có chiều cao lớn nhất. Mảng DP của một đỉnh được đặt tại một vị trí trên chuỗi nặng, do đó phần tử con ngay bên dưới nó tương ứng với vị trí tiếp theo trong cùng một mảng. 
3. Phân bổ một mảng DP liền kề cho mỗi chuỗi nặng. Một chuỗi có chiều cao (h) cần các vị trí (h+2), vì cần thêm một vị trí để nhân hậu tố lười biếng. 
4. Xử lý các đỉnh từ lá về gốc. Đặt (f_u[0]=1), biểu thị cây con chỉ bao gồm (u). 
5. Bỏ qua thành phần con nặng trong quá trình hợp nhất vì mảng DP của nó đã được chia sẻ vật lý với (u). Mọi đứa trẻ khác đều là đứa trẻ nhẹ nhàng và được hợp nhất một cách rõ ràng. 
6. Trong khi hợp nhất một light con (v), hãy quét mảng DP của nó từ độ sâu 0 trở lên. Duy trì hai tổng tiền tố. Đầu tiên là (1+\sum_{j\le i}f_u[j]) và thứ hai là (1+\sum_{j<i}f_v[j]). Hai đại lượng này chính xác là các hệ số được yêu cầu bởi phép truy toán đối với độ sâu cực đại mới. 
7. Sau độ sâu cuối cùng được biểu thị bằng (v), mọi vị trí còn lại được nhân với cùng một hệ số, đó là (1+\sum_j f_v[j]). Lưu trữ thao tác đó dưới dạng phép nhân lười biếng thay vì chạm vào phần còn lại của chuỗi. 
8. Sau cây DP hoàn chỉnh, coi mỗi nút con của nút gốc là một nhánh độc lập. Ở độ sâu (D), duy trì (a_c=f_c[D-1]) và (b_c=1+\sum_{j<D-1}f_c[j]). Cặp ((a_c,b_c)) mô tả đầy đủ cách nhánh (c) tham gia vào cây con có độ sâu tối đa là (D). 
9. Sử dụng cây phân đoạn có nút lưu trữ ba giá trị. Cái đầu tiên là tích của (a_c+b_c), cái thứ hai là tích của (b_c) và cái thứ ba tính các cấu hình trong đó có chính xác một nhánh đóng góp (a_c). Nếu hai nút con của nút cây phân đoạn có trạng thái ((P_1,B_1,Q_1)) và ((P_2,B_2,Q_2)), hãy kết hợp chúng thành 
[ 
P=P_1P_2, 
] 
[ 
B=B_1B_2, 
] 
[ 
Q=Q_1B_2+B_1Q_2. 
] 
Công thức cuối cùng cho biết nhánh duy nhất đạt mức tối đa hiện tại nằm ở đoạn bên trái hoặc ở đoạn bên phải. 
10. Với mỗi độ sâu dương (D), giả sử gốc cây phân đoạn cho (P_D) và (Q_D). Nếu (P_{D-1}) là số lựa chọn có độ sâu tối đa bên dưới (D), thì 
[ 
P_D-P_{D-1}-Q_D 
] 
đếm chính xác các cây con có độ sâu tối đa là (D) trong ít nhất hai nhánh gốc. Thêm phần này vào câu trả lời và tiếp tục đến độ sâu tiếp theo. 

### Tại sao nó hoạt động 

Trò chơi sẽ giành chiến thắng cho người chơi thứ hai khi đỉnh bắt đầu chính là tâm đường kính của cây. Một cây con kết nối gốc có đỉnh 1 là tâm đường kính của nó chính xác khi độ sâu gốc lớn nhất của nó xảy ra ở ít nhất hai nhánh gốc khác nhau. DP đếm mọi lựa chọn được kết nối có thể có bên trong mỗi nhánh theo độ sâu tối đa chính xác của nó. Tập hợp gốc sau đó phân tách các cấu hình theo độ sâu tối đa toàn cầu của chúng và loại bỏ chính xác những cấu hình trong đó chỉ có một nhánh đạt đến mức tối đa đó. Do đó, mọi cây con được đếm đều có gốc 1 là tâm đường kính của nó và mọi cây con có gốc 1 là tâm đường kính của nó được tính chính xác một lần. 

Việc biểu diễn chuỗi nặng không thay đổi bất kỳ giá trị DP nào. Nó chỉ thay đổi nơi các giá trị được lưu trữ. Một đỉnh và con nặng của nó sử dụng các vị trí liền kề trong cùng một mảng, trong khi mảng của con nhẹ được hợp nhất vào mảng chia sẻ đó. Hệ số nhân hậu tố lười biếng giống hệt về mặt đại số với việc nhân tất cả các trạng thái có độ sâu lớn hơn không bị ảnh hưởng với cùng một hệ số. Do đó, việc tối ưu hóa sẽ duy trì sự lặp lại DP ban đầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 1_000_000_007

def solve_case(n, adj):
    if n == 1:
        return 1

    # Root the tree at 0.
    parent = [-1] * n
    parent[0] = -2
    order = [0]

    for u in order:
        for v in adj[u]:
            if v == parent[u]:
                continue
            parent[v] = u
            order.append(v)

    # Height and heavy child.
    height = [0] * n
    heavy = [-1] * n

    for u in reversed(order):
        best_h = -1
        best_v = -1
        for v in adj[u]:
            if parent[v] == u:
                hv = height[v]
                if hv > best_h:
                    best_h = hv
                    best_v = v
        if best_v != -1:
            height[u] = best_h + 1
            heavy[u] = best_v

    # Allocate one contiguous array per heavy chain.
    base = [0] * n
    pool_size = 0

    for u in order:
        p = parent[u]
        if p != -2 and heavy[p] == u:
            continue

        x = u
        cur = pool_size
        length = height[u] + 2
        pool_size += length

        while x != -1:
            base[x] = cur
            cur += 1
            x = heavy[x]

    val = [0] * pool_size
    tag = [1] * pool_size

    def modify(pos, mul):
        val[pos] = val[pos] * mul % MOD
        tag[pos] = tag[pos] * mul % MOD

    def pushdown(pos):
        t = tag[pos]
        if t != 1:
            nxt = pos + 1
            val[nxt] = val[nxt] * t % MOD
            tag[nxt] = tag[nxt] * t % MOD
            tag[pos] = 1

    # Tree DP.
    for u in reversed(order):
        bu = base[u]
        val[bu] = 1

        # Only light children are explicitly merged.
        for v in adj[u]:
            if parent[v] != u or v == heavy[u]:
                continue

            bv = base[v]
            length = height[v] + 1

            r = 1
            s = 1

            for i in range(1, length + 1):
                pu = bu + i
                pv = bv + i - 1

                pushdown(pu)
                pushdown(pv)

                a = val[pu]
                b = val[pv]

                # r = 1 + sum of current f-values through depth i.
                r += a
                if r >= MOD:
                    r -= MOD

                # s = 1 + sum of child f-values below depth i.
                val[pu] = (r * b + s * a) % MOD

                s += b
                if s >= MOD:
                    s -= MOD

            # For all larger depths the light child can only be chosen
            # completely or omitted, so the multiplier is the same.
            modify(bu + length + 1, s)

    # Root aggregation.
    root_children = [v for v in adj[0] if parent[v] == 0]
    k = len(root_children)

    if k == 0:
        return 1

    max_depth = max(height[v] + 1 for v in root_children)

    # events[d] contains branches whose height reaches depth d.
    events = [[] for _ in range(max_depth + 1)]

    for idx, v in enumerate(root_children):
        branch_height = height[v] + 1
        for d in range(1, branch_height + 1):
            events[d].append(idx)

    # For each root branch:
    # b[idx] = number of choices with maximum depth < current d
    # a[idx] = number of choices with maximum depth == current d
    b = [1] * k
    a = [1] * k

    size = 1
    while size < k:
        size <<= 1

    seg_p = [1] * (2 * size)
    seg_b = [1] * (2 * size)
    seg_q = [0] * (2 * size)

    # Initially d = 1, so b = 1 and a = f_child[0] = 1.
    for i in range(k):
        pos = size + i
        seg_p[pos] = 2
        seg_b[pos] = 1
        seg_q[pos] = 1

    for pos in range(size - 1, 0, -1):
        left = pos << 1
        right = left | 1

        seg_p[pos] = seg_p[left] * seg_p[right] % MOD
        seg_b[pos] = seg_b[left] * seg_b[right] % MOD
        seg_q[pos] = (
            seg_q[left] * seg_b[right]
            + seg_b[left] * seg_q[right]
        ) % MOD

    def update_branch(idx):
        pos = size + idx
        x = (b[idx] + a[idx]) % MOD

        seg_p[pos] = x
        seg_b[pos] = b[idx]
        seg_q[pos] = a[idx]

        pos >>= 1
        while pos:
            left = pos << 1
            right = left | 1

            seg_p[pos] = seg_p[left] * seg_p[right] % MOD
            seg_b[pos] = seg_b[left] * seg_b[right] % MOD
            seg_q[pos] = (
                seg_q[left] * seg_b[right]
                + seg_b[left] * seg_q[right]
            ) % MOD

            pos >>= 1

    answer = 1
    previous_p = 1

    for d in range(1, max_depth + 1):
        if d >= 2:
            for idx in events[d]:
                v = root_children[idx]

                old_a = a[idx]
                b[idx] += old_a
                if b[idx] >= MOD:
                    b[idx] -= MOD

                pos = base[v] + d - 1
                pushdown(pos)
                a[idx] = val[pos]

                update_branch(idx)

        current_p = seg_p[1]
        exactly_one = seg_q[1]

        good = current_p - previous_p - exactly_one
        good %= MOD

        answer += good
        if answer >= MOD:
            answer -= MOD

        previous_p = current_p

    return answer

def solve():
    t = int(input())
    out = []

    for case_id in range(1, t + 1):
        n = int(input())
        adj = [[] for _ in range(n)]

        for _ in range(n - 1):
            x, y = map(int, input().split())
            x -= 1
            y -= 1
            adj[x].append(y)
            adj[y].append(x)

        ans = solve_case(n, adj)
        out.append(f"Case #{case_id}: {ans}")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc truyền tải đầu tiên thiết lập biểu diễn gốc mà không cần đệ quy. Việc tránh DFS đệ quy là có chủ ý vì đường dẫn của các đỉnh (2\cdot10^5) sẽ vượt quá độ sâu đệ quy của Python. 

Việc tính toán chiều cao được thực hiện theo thứ tự ngược lại. Đứa trẻ có chiều cao lớn nhất sẽ trở thành đứa trẻ nặng cân. Phần con nặng không được hợp nhất một cách rõ ràng vì bộ lưu trữ DP của nó đã nằm cạnh bộ lưu trữ của cha mẹ. Đây là tối ưu hóa bộ nhớ trung tâm. 

các`val`mảng lưu trữ các giá trị DP thực tế, trong khi`tag`cửa hàng đang chờ nhân. Khi một hậu tố bắt đầu ở vị trí (p) được nhân với một giá trị nào đó thì chỉ cần thay đổi vị trí (p) ngay lập tức. Thẻ ghi lại hệ số tương tự cho các vị trí sau. Đang gọi`pushdown(p)`chuyển nó tới (p+1). Vì mỗi lần hợp nhất sẽ đọc độ sâu theo thứ tự tăng dần nên mỗi thẻ đang chờ xử lý sẽ di chuyển về phía trước chính xác khi cần. 

Các biến`r`Và`s`trong việc hợp nhất là số lượng tiền tố. biểu hiện```
val[pu] = (r * b + s * a) % MOD
```là sự lặp lại hai trường hợp cho độ sâu tối đa chính xác mới. Thứ tự cập nhật quan trọng:`r`phải bao gồm hiện tại`a`, trong khi`s`vẫn chỉ thể hiện độ sâu con nhỏ hơn độ sâu hiện tại. 

Việc tổng hợp gốc có chủ ý không sử dụng phép chia mô-đun. Tích của số nhánh có thể bằng 0 modulo (10^9+7), do đó, việc cố gắng loại bỏ một thừa số bằng nghịch đảo mô-đun sẽ yêu cầu xử lý đặc biệt đối với các thừa số bằng 0. Cây phân đoạn tránh hoàn toàn sự phân chia. 

Đối với nút cây phân đoạn,`seg_p`đại diện cho mọi sự lựa chọn,`seg_b`đại diện cho các lựa chọn trong đó không có nhánh nào trong phân khúc đó đạt đến mức tối đa hiện tại và`seg_q`đại diện cho các lựa chọn trong đó chính xác một nhánh thực hiện. Ba giá trị này đủ để kết hợp các nhóm nhánh gốc tùy ý. 

Tất cả số học đều được giảm modulo (10^9+7). Số nguyên trong Python không bị tràn, nhưng việc giảm tích số sẽ giữ cho số nguyên nhỏ và tránh sự tăng trưởng không cần thiết. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Cây đơn giản là (1-2). Chỉ có một nhánh gốc nên không có cây con nào trống có thể có độ sâu tối đa ở hai nhánh khác nhau. 

| Độ sâu (D) | Chi nhánh (a) | Chi nhánh (b) | (P_D) | (Q_Đ) | Cây con hợp lệ mới | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 2 | 1 | (2-1-1=0) | 

Gốc đơn được thêm vào riêng biệt, đưa ra câu trả lời cuối cùng là 1. 

### Mẫu 2 

Cái cây là```
      1
     / \
    2   4
    |  / \
    3 5   6
```Đối với nhánh bắt nguồn từ 2, DP có độ sâu chính xác là 

[ 
f_2=[1,1]. 
] 

Đối với nhánh bắt nguồn từ 4, DP có độ sâu chính xác là 

[ 
f_4=[1,3], 
] 

bởi vì lựa chọn không có độ sâu duy nhất là đỉnh 4, trong khi ở độ sâu 1, chúng ta có thể chọn đỉnh 5, đỉnh 6 hoặc cả hai. 

Ở độ sâu 1, cả hai nhánh có thể đạt mức tối đa: 

| Độ sâu (D) | Nhánh 2 (a,b) | Nhánh 4 (a,b) | (P_D) | (Q_Đ) | Mới hợp lệ | 
| --- | --- | --- | --- | --- | --- | 
| 1 | ((1,1)) | ((1,1)) | 4 | 2 | (4-1-2=1) | 

Ở độ sâu 2, nhánh bắt nguồn ở 2 có (a=1,b=2), trong khi nhánh bắt nguồn ở 4 có (a=3,b=2). 

| Độ sâu (D) | Nhánh 2 (a,b) | Nhánh 4 (a,b) | (P_D) | (Q_Đ) | Mới hợp lệ | 
| --- | --- | --- | --- | --- | --- | 
| 2 | ((1,2)) | ((3,2)) | 15 | 8 | (15-4-8=3) | 

Có một cây đơn, một cây con hợp lệ với độ sâu tối đa 1 và ba cây con hợp lệ với độ sâu tối đa 2. Tổng cộng là 

[ 
1+1+3=5, 
] 

phù hợp với đầu ra mẫu. 

Dấu vết cũng cho thấy tại sao chỉ đếm các cây con có đường kính đi qua gốc là không đủ. Độ sâu tối đa phải đạt được trên ít nhất hai nhánh riêng biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n\log n)) | DP chuỗi dài xử lý từng độ sâu chuỗi nhẹ một cách rõ ràng và các cập nhật nhánh gốc sử dụng cây phân đoạn. | 
| Không gian | (O(n)) | Cấu trúc lưu trữ DP chuỗi nặng, biểu diễn cây và tập hợp gốc đều là tuyến tính. | 

Cây DP nhỏ hơn đáng kể so với phương pháp bậc hai vì đường đi dài và nặng của một đỉnh được lưu trữ một lần và được sử dụng lại bởi tất cả các đỉnh trên đường đi đó. Việc hợp nhất con nhẹ được tính cho các chuỗi ngắn hơn, đưa ra ràng buộc tiêu chuẩn (O(n\log n)) cho việc triển khai này. Tập hợp gốc chỉ thực hiện các cập nhật theo chiều sâu nhánh (O(n)), mỗi lần lấy (O(\log n)). Với (n\le2\cdot10^5), điều này phù hợp với độ phức tạp dự kiến ​​của vấn đề. Tài liệu chính thức của cuộc thi cũng đưa ra các giải pháp cây-DP (O(n)) hoặc (O(n\log n)) và cảnh báo rõ ràng về sự thoái hóa (O(n^2)). 

## Trường hợp thử nghiệm 

Khai thác thử nghiệm sau đây giả định giải pháp được gửi đã được lưu dưới dạng`solution.py`. Nó gọi chương trình thực tế, do đó các xác nhận sẽ kiểm tra hành vi đầu vào/đầu ra hoàn chỉnh thay vì triển khai lại riêng biệt.```python
# helper: run the submitted solution and return its output
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

# Provided sample
sample = """\
2
2
1 2
6
1 2
2 3
1 4
4 5
4 6
"""
assert run(sample) == """\
Case #1: 1
Case #2: 5
""".strip(), "provided samples"

# Minimum-size tree
assert run("""\
1
1
""") == "Case #1: 1", "single vertex"

# Path of length 3
assert run("""\
1
3
1 2
2 3
""") == "Case #1: 1", "path"

# Star with three leaves.
# Valid choices are the singleton plus every choice containing
# at least two leaves: 1 + C(3,2) + C(3,3) = 5.
assert run("""\
1
4
1 2
1 3
1 4
""") == "Case #1: 5", "star"

# Maximum-size path.
# Every connected subgraph containing vertex 1 is a prefix of the path,
# and only the singleton has vertex 1 as its diameter center.
n = 200000
edges = "\n".join(f"{i} {i + 1}" for i in range(1, n))
max_path = f"1\n{n}\n{edges}\n"

assert run(max_path) == "Case #1: 1", "maximum-size path"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1`|`Case #1: 1`| Kích thước tối thiểu và xử lý đơn lẻ | 
|`1-2-3`|`Case #1: 1`| Một đường dẫn trong đó gốc không bao giờ là trung tâm của tiền tố tầm thường | 
| Ngôi sao ba lá |`Case #1: 5`| Nhiều nhánh đạt cùng độ sâu tối đa | 
| Đường đi có 200000 đỉnh |`Case #1: 1`| Kích thước tối đa và tránh DFS đệ quy hoặc DP bậc hai | 
| Mẫu chính thức |`1`,`5`| Tính chính xác hoàn toàn so với các ví dụ đã cho | 

## Vỏ cạnh 

Đối với cây một đỉnh```
1
1
```cây có gốc không có con. DP chỉ chứa (f_1[0]=1) và tập hợp gốc không có độ sâu dương để xử lý. Câu trả lời bắt đầu từ một cho người độc thân và vẫn là một. 

Đối với cây hai đỉnh```
1
2
1 2
```có một nhánh gốc. Ở độ sâu 1, nhánh đó có (a=1) và (b=1), do đó (P_1=2). Chính xác một nhánh đạt đến độ sâu 1 trong một cấu hình, cho ra (Q_1=1). Vì (P_0=1), số có ít nhất hai nhánh ở độ sâu tối đa là (2-1-1=0). Singleton vẫn là lựa chọn chiến thắng duy nhất. 

Đối với con đường```
1
3
1 2
2 3
```gốc vẫn chỉ có một nhánh. Độ sâu tối đa có thể là 1 hoặc 2 nhưng không bao giờ có thể xảy ra ở hai nhánh khác nhau. Cả hai độ sâu đều không đóng góp cấu hình chiến thắng nào, chỉ để lại cấu hình đơn lẻ. 

Đối với ngôi sao```
1
4
1 2
1 3
1 4
```có ba nhánh gốc. Ở độ sâu 1, mỗi nhánh có (a=1) và (b=1). Do đó, tất cả các lựa chọn nhánh (2^3=8) được biểu thị bằng (P_1), trong khi chính xác một nhánh đạt mức tối đa trong cấu hình (Q_1=3). Việc loại bỏ trường hợp đơn lẻ và trường hợp chính xác một nhánh sẽ cho (8-1-3=4) cây con không đơn lẻ hợp lệ. Thêm singleton cho 5. 

Đường dẫn có kích thước tối đa cũng là một trường hợp hữu ích khi triển khai. Độ sâu của nó có thể là (199999), do đó việc triển khai sử dụng DFS đệ quy thường sẽ vượt quá giới hạn đệ quy của Python. Giải pháp sử dụng phương pháp truyền tải lặp đi lặp lại xuyên suốt. Vì một đường dẫn không có ánh sáng con nên vòng lặp hợp nhất đắt tiền gần như hoàn toàn không có, do đó độ sâu lớn không gây ra phép tính bậc hai. 

Các nhánh có chiều cao bằng nhau là một trường hợp tế nhị khác. Trẻ nặng cân được chọn tùy ý trong số những trẻ có chiều cao tối đa. Nếu hai đứa trẻ có cùng chiều cao thì một đứa nặng và đứa kia nhẹ. Nhánh ánh sáng vẫn được hợp nhất hoàn toàn vào mảng DP dùng chung, do đó không có lựa chọn cây con nào bị mất. Việc lựa chọn con có chiều cao bằng nhau chỉ ảnh hưởng đến việc lưu trữ chứ không bao giờ ảnh hưởng đến giá trị DP. 

Cuối cùng, số 0 mô-đun không được coi là số nguyên 0 thông thường khi chia tích. Số lượng nhánh có thể bằng 0 modulo (10^9+7), mặc dù số lượng cấu hình thực tế là dương. Do đó, tập hợp gốc chỉ sử dụng cây phân đoạn và phép nhân, tránh nghịch đảo mô đun và làm cho phép tính hợp lệ ngay cả khi một số tích triệt tiêu theo mô đun.
