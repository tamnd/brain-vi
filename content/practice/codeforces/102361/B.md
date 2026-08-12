---
title: "CF 102361B - Cây của Haruhi Suzumiya"
description: "Chúng ta có một cây có gốc có gốc là đỉnh 1. Mỗi đỉnh có trọng số nguyên và độ sâu của nó là khoảng cách từ gốc."
date: "2026-08-13T00:06:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102361
codeforces_index: "B"
codeforces_contest_name: "2019 China Collegiate Programming Contest Qinhuangdao Onsite"
rating: 0
weight: 102361
solve_time_s: 246
verified: true
draft: false
---

[CF 102361B - Cây của Haruhi Suzumiya](https://codeforces.com/problemset/problem/102361/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4 phút 6s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có gốc có gốc là đỉnh 1. Mỗi đỉnh có trọng số nguyên và độ sâu của nó là khoảng cách từ gốc. Chúng tôi chia tất cả các đỉnh thành nhóm A và nhóm B, và với mọi kích thước có thể có của B, từ 0 đến n, chúng tôi cần giá trị tối thiểu có thể có của tổng số lượt không thích của hai nhóm. 

Nhóm A trả tiền cho ba loại việc. Tổ tiên có trọng số lớn hơn con cháu của nó sẽ tạo ra một hình phạt, hai đỉnh không thể so sánh được tạo ra một hình phạt và mọi đỉnh trong A đều đóng góp chiều sâu của nó. Nhóm B chỉ có một loại hình phạt cặp: tổ tiên có trọng lượng nhỏ hơn con cháu của nó. 

Phần tinh tế là một cặp có thể bị phạt ở A, bị phạt ở B hoặc không bị phạt ở nhóm nào. Khi hai đỉnh có trọng số bằng nhau và một đỉnh là tổ tiên của đỉnh kia thì không nhóm nào ghét cặp đó. Trường hợp ngoại lệ đó là lý do tại sao một đối số đơn giản "gán cho mỗi đỉnh một điểm và sắp xếp" cần thêm một quan sát cấu trúc. 

Các ràng buộc chính thức cho phép n lên tới 500.000, với giới hạn thời gian 2 giây và giới hạn bộ nhớ 1024 MB. Một thuật toán bậc hai sẽ yêu cầu khoảng 250 tỷ cặp phép toán cho một lần truyền tải đầy đủ các đỉnh, do đó, ngay cả O(n²) cũng vượt xa phạm vi dự định. Giải pháp cần nằm ở khoảng O(n log n), với bộ nhớ tuyến tính. 

Trường hợp cạnh đầu tiên là một đỉnh duy nhất.```
1
7
```Không có cặp nào và đỉnh duy nhất có độ sâu bằng 0, vì vậy câu trả lời là```
0
0
```Giải pháp khởi tạo độ sâu gốc thành 1 sẽ báo cáo sai chi phí dương. 

Trường hợp cạnh thứ hai có trọng số bằng nhau dọc theo chuỗi tổ tiên.```
3
1 1 1
1 2
2 3
```Đầu ra đúng là```
3
1
0
0
```Với cả ba đỉnh trong A, đóng góp độ sâu là 0 + 1 + 2 = 3. Nếu chỉ có đỉnh sâu nhất di chuyển đến B, A chứa hai đỉnh đầu tiên và có giá 1. Nếu hai đỉnh sâu nhất di chuyển đến B, A chỉ chứa gốc và chi phí bằng 0. Các cặp tổ tiên có trọng lượng bằng nhau không bao giờ đóng góp vào một trong hai nhóm. Một giải pháp coi mọi cặp đều thuộc về bên A hoặc bên B sẽ sai trong trường hợp này. 

Trường hợp cạnh thứ ba là cây phân nhánh có trọng số bằng nhau.```
4
1 1 1 1
1 2
1 3
1 4
```Đầu ra đúng là```
6
3
1
0
0
```Khi một lá được chuyển đến B, ba đỉnh còn lại vẫn ở A. Ba lá có quan hệ cặp đôi không thể so sánh được nên hai lá còn lại đóng góp một cặp phạt, trong khi độ sâu của chúng đóng góp hai, cho 3. Khi hai lá được chuyển đến B, A chứa gốc và một lá, có thể so sánh được nên chi phí chỉ bằng 1. Trường hợp này bắt các triển khai xử lý các chuỗi tổ tiên có trọng số bằng nhau nhưng quên rằng các đỉnh có trọng số bằng nhau có thể phân nhánh. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là liệt kê tập hợp được B chọn. Đối với mỗi tập hợp con, kích thước của nó cho chúng ta biết nó thuộc về vị trí đầu ra nào và chúng ta có thể đánh giá từng cặp đỉnh và mọi đóng góp theo độ sâu để có được chi phí chính xác. Điều này đúng vì nó xem xét mọi phân vùng có thể. 

Vấn đề là số lượng tập hợp con. Có 2^n lựa chọn có thể có của B và việc đánh giá một lựa chọn một cách đơn giản sẽ thực hiện kiểm tra cặp Θ(n2). Trong trường hợp xấu nhất, đây là Θ(n2^n), vô vọng với n = 500.000. Ngay cả việc tính toán một câu trả lời bằng cách liệt kê tất cả các tập hợp con có kích thước cụ thể cũng đòi hỏi nhiều lựa chọn nhị thức. 

Quan sát hữu ích đến từ việc xem xét từng cặp một. Đặt x_i bằng 1 khi đỉnh i thuộc B và 0 khi nó thuộc A. Đối với một cặp không thể so sánh được, cặp này có giá bằng 1 khi cả hai điểm cuối đều thuộc A, do đó đóng góp của nó là 

[ 
(1-x_i)(1-x_j)=1-x_i-x_j+x_ix_j. 
] 

Đối với cặp tổ tiên có trọng số lớn hơn, biểu thức tương tự xuất hiện vì hình phạt thuộc về A. Đối với cặp tổ tiên có trọng số nhỏ hơn, hình phạt thuộc về B, do đó đóng góp chỉ đơn giản là 

[ 
x_ix_j. 
] 

Cặp duy nhất không có sự đóng góp là cặp tổ tiên có trọng số bằng nhau. 

Điều này cho một dạng bậc hai chung. Sau khi sửa |B| = k, tổng của x_i x_j trên tất cả các cặp thông thường đóng góp một giá trị cố định (\binom{k}{2}), ngoại trừ các cặp tổ tiên có trọng số bằng nhau đã bị bỏ qua. Phần phụ thuộc vào đỉnh còn lại có thể được biểu thị bằng điểm cho mỗi đỉnh. 

Gọi b_i là độ sâu của i cộng với số hình phạt cặp loại A liên quan đến i. Nếu chúng ta bỏ qua các cặp tổ tiên có trọng số bằng nhau trong giây lát, việc di chuyển i đến B sẽ cải thiện mục tiêu thêm b_i, trong khi việc chọn hai đỉnh B sẽ đưa ra số hạng cố định (\binom{k}{2}). 

Các cặp tổ tiên có trọng lượng bằng nhau cần được đối xử đặc biệt. Giả sử đỉnh u là tổ tiên có trọng số bằng nhau của v. Nếu u được chọn cho B nhưng v thì không, chúng ta có thể thay thế u bằng v mà không làm cho lời giải tệ hơn. Dọc theo cạnh cha-con có trọng số bằng nhau, điểm b tăng đủ để bù đắp cho mọi hậu duệ có trọng lượng bằng nhau có thể bị mất khỏi phần thưởng cặp. Việc lặp lại phép biến đổi này mang lại một giải pháp tối ưu trong đó, đối với mọi quan hệ tổ tiên có trọng số bằng nhau, việc chọn tổ tiên ngụ ý chọn tất cả các con cháu có trọng số bằng nhau. 

Đối với một tập đóng hướng xuống như vậy, số cặp tổ tiên có trọng số bằng nhau bên trong B chỉ đơn giản là tổng số các con cháu có trọng số bằng nhau trên các đỉnh được chọn của chúng. Chúng ta có thể hấp thụ điều này vào điểm số đỉnh. 

Sau khi đại số được đơn giản hóa, điểm cuối cùng có dạng đặc biệt nhỏ gọn: 

[ 
g_i = 
n-\tên toán tử{cây conSize__i 
+#{\text{tổ tiên của }i\text{ có trọng số}>w_i} 
+#{\text{hậu duệ nghiêm ngặt của }i\text{ có trọng số}\le w_i}. 
] 

Khi đã biết các điểm này, giải pháp tốt nhất có kích thước k sẽ đạt được bằng cách lấy k điểm lớn nhất. Điểm bằng nhau phải được sắp xếp theo độ sâu giảm dần, điều này bảo toàn thuộc tính đóng xuống cần thiết cho các quan hệ tổ tiên có trọng số bằng nhau. 

Chi phí ban đầu, khi mọi đỉnh đều thuộc A, cũng được đơn giản hóa. Số cặp không so sánh được là 

[ 
\binom n2-\sum_i d_i, 
] 

bởi vì đỉnh tôi có chính xác d_i tổ tiên nghiêm ngặt. Việc thêm (\sum_i d_i) của Mikuru sẽ hủy bỏ thuật ngữ đó. Do đó chi phí ban đầu chỉ đơn giản là 

[ 
C=\binom n2+ 
#{(u,v)\text{ là tổ tiên của }v,\ w_u>w_v}. 
] 

Số đếm sau chính xác là tổng số lượng tổ tiên lớn hơn được sử dụng trong g_i.

Nhiệm vụ còn lại là tính toán kích thước cây con, số lượng tổ tiên lớn hơn và số lượng con cháu ít hơn hoặc bằng một cách hiệu quả. Một chuyến tham quan Euler làm cho mỗi cây con trở thành một khoảng liền kề. Sau đó, cây Fenwick có thể trả lời các truy vấn đếm ngoại tuyến được yêu cầu trong O(log n). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n22^n) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Root cây ở đỉnh 1 bằng DFS lặp. Lưu trữ cha mẹ, độ sâu, vị trí đặt hàng trước và danh sách đặt hàng trước. Xử lý các đỉnh theo thứ tự ngược lại sẽ cho ra kích thước của mỗi cây con. Cây con của đỉnh i chiếm khoảng Euler ([tin_i,tout_i]). 
2. Sắp xếp tất cả các đỉnh theo trọng số. Chúng tôi sẽ sử dụng thứ tự này cho hai lần quét Fenwick ngoại tuyến. Sắp xếp một lần là đủ vì hai số đếm bắt buộc sử dụng các hướng ngược nhau của cùng một thứ tự trọng số. 
3. Tính (a_i), số tổ tiên của i có trọng lượng lớn hơn (w_i). Xử lý các đỉnh trong nhóm trọng số giảm dần. Trước khi chèn một nhóm, hãy truy vấn mọi đỉnh trong nhóm đó. Cây Fenwick lưu trữ các phép cộng phạm vi trên các cây con, do đó, một đỉnh đã được chèn sẽ đóng góp 1 cho mỗi đỉnh trong cây con của nó. Vì chỉ có trọng số lớn hơn mới được chèn vào nên truy vấn tại i sẽ đếm chính xác tổ tiên có trọng số lớn hơn của nó. Các đỉnh có cùng trọng số được truy vấn trước khi chèn, điều này thực thi chính xác bất đẳng thức nghiêm ngặt. 
4. Tính (c_i), số con cháu nghiêm ngặt của i có trọng số lớn nhất (w_i). Đặt lại cây Fenwick và xử lý các nhóm trọng số theo thứ tự tăng dần. Chèn mọi đỉnh của nhóm hiện tại vào vị trí Euler của nó, sau đó truy vấn toàn bộ khoảng cây con. Kết quả đếm các đỉnh trong cây con có trọng số tối đa (w_i), bao gồm cả chính i, vì vậy hãy trừ đi một. 
5. Tính toán 

[ 
g_i=n-\tên toán tử{subtreeSize__i+a_i+c_i. 
] 

Số hạng đầu tiên biểu thị các đỉnh nằm ngoài cây con của i. Số hạng thứ hai dành cho tổ tiên có trọng lượng lớn hơn. Số hạng thứ ba dành cho những con cháu có cân nặng đủ nhỏ để tham gia tính điểm bên A, bao gồm cả những con cháu có cân nặng bằng nhau. 

1. Tính đường cơ sở toàn A 

[ 
C=\binom n2+\sum_i a_i. 
] 

Sự hủy bỏ giữa các hình phạt cặp không thể so sánh được và các hình phạt chiều sâu là điều khiến cách diễn đạt này trở nên ngắn gọn. 

1. Sắp xếp các đỉnh theo thứ tự giảm dần (g_i), phá vỡ các mối liên kết bằng cách giảm độ sâu. Đối với các đỉnh tổ tiên và con cháu có trọng số bằng nhau, con cháu có số điểm ít nhất bằng tổ tiên. Khi điểm số bằng nhau, sâu hơn trước tiên sẽ làm cho mọi tiền tố được đóng xuống dưới đối với tổ tiên có trọng số bằng nhau. 
2. Gọi (P_k) là tổng của k điểm đầu tiên. Câu trả lời cho (|B|=k) là 

[ 
\boxed{C+\binom{k}{2}-P_k}. 
] 

Với k = 0, tổng tiền tố bằng 0 và công thức tính chi phí toàn A. Với k = n, nó cho biết chi phí để đưa mọi đỉnh vào B. 

Tại sao nó hoạt động: Đối với bất kỳ B cố định nào, mọi cặp tổ tiên không bằng nhau hoặc cặp không thể so sánh được đều đóng góp một thuật ngữ bậc hai (x_ix_j), trong khi mọi cặp bên A cũng đóng góp một thuật ngữ tuyến tính (-x_i) cho mỗi điểm cuối. Vì có chính xác k đỉnh có x_i = 1 nên các số hạng bậc hai thông thường đóng góp (\binom{k}{2}). Các cặp tổ tiên có trọng số bằng nhau là số hạng bậc hai duy nhất còn thiếu và đóng góp của chúng chính xác là số lượng các cặp được chọn như vậy. Điểm (g_i) kết hợp cả sự cải thiện tuyến tính và phần thưởng con cháu có trọng số bằng nhau. Một tập hợp tối ưu có thể được chuyển đổi thành một tập hợp đóng xuống giữa các con cháu có trọng số bằng nhau mà không làm giảm điểm của nó. Đối với mỗi tập hợp như vậy, phần thưởng cặp có trọng số bằng nhau chính xác là tổng số lượng con cháu tương ứng, do đó việc tối đa hóa mục tiêu chính là tối đa hóa chính xác tổng g_i. Lấy g_i lớn nhất k sẽ thực hiện điều đó và sự ràng buộc sâu làm cho tiền tố đã chọn bị đóng xuống. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    w = list(map(int, input().split()))

    graph = [[] for _ in range(n)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        graph[u].append(v)
        graph[v].append(u)

    parent = [-2] * n
    depth = [0] * n
    tin = [0] * n
    order = []

    parent[0] = -1
    stack = [0]

    while stack:
        u = stack.pop()
        tin[u] = len(order) + 1
        order.append(u)

        for v in graph[u]:
            if v == parent[u]:
                continue
            parent[v] = u
            depth[v] = depth[u] + 1
            stack.append(v)

    size = [1] * n
    for u in reversed(order):
        p = parent[u]
        if p >= 0:
            size[p] += size[u]

    tout = [0] * n
    for u in range(n):
        tout[u] = tin[u] + size[u] - 1

    by_weight = list(range(n))
    by_weight.sort(key=w.__getitem__)

    bit = [0] * (n + 2)

    def add(pos, delta):
        while pos <= n:
            bit[pos] += delta
            pos += pos & -pos

    def prefix(pos):
        res = 0
        while pos:
            res += bit[pos]
            pos -= pos & -pos
        return res

    # Number of strictly larger-weight ancestors.
    anc_greater = [0] * n

    i = n - 1
    while i >= 0:
        j = i
        value = w[by_weight[i]]
        while j >= 0 and w[by_weight[j]] == value:
            j -= 1

        for p in range(j + 1, i + 1):
            u = by_weight[p]
            anc_greater[u] = prefix(tin[u])

        for p in range(j + 1, i + 1):
            u = by_weight[p]
            add(tin[u], 1)
            add(tout[u] + 1, -1)

        i = j

    # Reuse the Fenwick tree.
    bit = [0] * (n + 2)

    # Number of strict descendants with weight <= w[i].
    desc_le = [0] * n

    i = 0
    while i < n:
        j = i
        value = w[by_weight[i]]
        while j < n and w[by_weight[j]] == value:
            j += 1

        for p in range(i, j):
            u = by_weight[p]
            add(tin[u], 1)

        for p in range(i, j):
            u = by_weight[p]
            desc_le[u] = prefix(tout[u]) - prefix(tin[u] - 1) - 1

        i = j

    score = [0] * n
    base = n * (n - 1) // 2

    for u in range(n):
        score[u] = n - size[u] + anc_greater[u] + desc_le[u]
        base += anc_greater[u]

    # Equal-score equal-weight ancestors must come after descendants.
    vertices = list(range(n))
    scale = n + 1
    vertices.sort(
        key=lambda u: -(score[u] * scale + depth[u])
    )

    ans = [0] * (n + 1)
    prefix_score = 0

    for k, u in enumerate(vertices, 1):
        prefix_score += score[u]
        ans[k] = base + k * (k - 1) // 2 - prefix_score

    sys.stdout.write("\n".join(map(str, ans)))

if __name__ == "__main__":
    solve()
```DFS đầu tiên là lặp lại chứ không phải đệ quy vì một cây có thể là một chuỗi gồm 500.000 đỉnh, điều này sẽ vượt quá giới hạn đệ quy của Python và cũng khiến việc triển khai đệ quy trở nên dễ dàng. Vị trí đặt hàng trước là dựa trên một vì cây Fenwick sử dụng các chỉ số dựa trên một cách tự nhiên. 

Việc duyệt ngược sẽ tính toán kích thước cây con mà không cần duyệt theo thứ tự sau riêng biệt. Khi đã biết kích thước,`tout[u] = tin[u] + size[u] - 1`tuân theo trực tiếp từ thứ tự Euler. 

Lần quét Fenwick đầu tiên sử dụng phép cộng phạm vi và truy vấn điểm. Việc thêm một điểm vào toàn bộ cây con của một đỉnh có nghĩa là truy vấn điểm tại i sẽ tính chính xác các tổ tiên đang hoạt động của i. Việc xử lý nghiêm ngặt các trọng số lớn hơn trước nhóm trọng số hiện tại sẽ làm cho các trọng số bằng nhau không hiển thị đối với truy vấn. 

Lần quét thứ hai sử dụng phép cộng điểm và truy vấn phạm vi. Ở cuối nhóm trọng số w, cây Fenwick chứa mọi đỉnh có trọng số lớn nhất là w. Truy vấn phạm vi cây con đếm các đỉnh như vậy bên trong cây con. Bản thân đỉnh đã được bao gồm, vì vậy phép trừ cuối cùng của một là cần thiết. 

Công thức cho`score`đã bao gồm các con cháu có trọng lượng bằng nhau thông qua`desc_le`. Không có thuật ngữ có trọng số bằng nhau riêng biệt trong quá trình triển khai vì 

#(\text{con cháu có trọng lượng}\le w_i). 
] 

Số nguyên Python có độ chính xác tùy ý, điều này rất hữu ích ở đây vì số lượng cặp trung gian là Θ(n²), khoảng 1,25 × 10¹¹ ở kích thước đầu vào tối đa. 

Việc sắp xếp cuối cùng sử dụng`score * (n + 1) + depth`dưới dạng một khóa số nguyên duy nhất. Điều này tránh việc phân bổ một bộ hai phần tử cho mỗi đỉnh. Điểm số là khóa chính, trong khi độ sâu chỉ giải quyết được điểm bằng nhau. 

## Ví dụ đã hoạt động 

Mẫu chính thức là```
4
4 1 2 3
1 2
2 3
2 4
```DFS cung cấp độ sâu 0, 1, 2, 2 và kích thước cây con 4, 3, 1, 1. Số lượng tổ tiên lớn hơn và số lượng con cháu ít hơn hoặc bằng nhau là: 

| Đỉnh | Độ sâu | Kích thước cây con | Tổ tiên lớn hơn | Hậu duệ ≤ Cân nặng | Điểm | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 4 | 0 | 3 | 3 | 
| 2 | 1 | 3 | 1 | 0 | 2 | 
| 3 | 2 | 1 | 1 | 0 | 4 | 
| 4 | 2 | 1 | 1 | 0 | 4 | 

Đường cơ sở là 

[ 
\binom42 + (0+1+1+1)=6+3=9. 
] 

Điểm được sắp xếp là 4, 4, 3, 2. 

| k | Điểm chọn | Điểm tiền tố | (\binom{k}{2}) | Trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | không | 0 | 0 | 9 | 
| 1 | 4 | 4 | 0 | 5 | 
| 2 | 4, 4 | 8 | 1 | 2 | 
| 3 | 4, 4, 3 | 11 | 3 | 1 | 
| 4 | 4, 4, 3, 2 | 13 | 6 | 2 | 

Điều này tái tạo đầu ra chính thức`9 5 2 1 2`. Các đỉnh được chọn cho k = 1, 2, 3 có thể là các đỉnh 3, 3 và 4, rồi 1, 3 và 4, khớp với sơ đồ được mô tả trong câu lệnh. 

Đối với ví dụ thứ hai, hãy xem xét một chuỗi có trọng lượng bằng nhau.```
3
1 1 1
1 2
2 3
```Mỗi cặp tổ tiên đều có trọng số như nhau nên cả Haruhi và Kyon đều không thích cặp tổ tiên nào. Chi phí duy nhất khi mọi thứ đều ở A là tổng độ sâu. 

| Đỉnh | Độ sâu | Kích thước cây con | Tổ tiên lớn hơn | Hậu duệ ≤ Cân nặng | Điểm | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 3 | 0 | 2 | 2 | 
| 2 | 1 | 2 | 0 | 1 | 2 | 
| 3 | 2 | 1 | 0 | 0 | 2 | 

Tất cả các điểm đều bằng nhau, do đó độ sâu giảm dần sẽ chọn đỉnh 3 trước, sau đó là đỉnh 2, rồi đến đỉnh 1. 

| k | Các đỉnh được chọn | Điểm tiền tố | (\binom{k}{2}) | Trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | không | 0 | 0 | 3 | 
| 1 | 3 | 2 | 0 | 1 | 
| 2 | 3, 2 | 4 | 1 | 0 | 
| 3 | 3, 2, 1 | 6 | 3 | 0 | 

Sự ràng buộc là điều cần thiết ở đây. Nếu đỉnh 1 được chọn trước đỉnh 3, tập hợp đã chọn sẽ không bị đóng xuống giữa các trọng số bằng nhau và công thức tính điểm đơn giản hóa sẽ không còn thể hiện chính xác phần thưởng cặp có trọng số bằng nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Một lần duyệt cây, một lần sắp xếp theo trọng lượng, hai lần quét Fenwick và một lần sắp xếp cuối cùng | 
| Không gian | O(n) | Lưu trữ cây, dữ liệu Euler, điểm số, mảng sắp xếp và cây Fenwick | 

Các hoạt động chủ yếu là hai đợt quét Fenwick và các hoạt động phân loại. Với n = 500.000, O(n log n) là thang đo dự kiến ​​cho giới hạn C++ 2 giây, trong khi dung lượng bộ nhớ lớn 1024 MB dành cho các mảng phụ tuyến tính được sử dụng khi triển khai Python. 

## Trường hợp thử nghiệm```python
# The solution code above should be placed in the same file before these tests.
# The test harness calls solve() with redirected stdin/stdout.

import sys
import io
from contextlib import redirect_stdout

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()

    try:
        with redirect_stdout(out):
            solve()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

    return out.getvalue() + ("\n" if out.getvalue() and not out.getvalue().endswith("\n") else "")

# Official sample
sample = """\
4
4 1 2 3
1 2
2 3
2 4
"""
assert run(sample) == "9\n5\n2\n1\n2\n", "official sample"

# Minimum-size input
minimum = """\
1
7
"""
assert run(minimum) == "0\n0\n", "single vertex"

# Equal weights on a chain
equal_chain = """\
3
1 1 1
1 2
2 3
"""
assert run(equal_chain) == "3\n1\n0\n0\n", "equal-weight chain"

# Equal weights on a branching tree
equal_star = """\
4
1 1 1 1
1 2
1 3
1 4
"""
assert run(equal_star) == "6\n3\n1\n0\n0\n", "equal-weight branching tree"

# Boundary weights and strict inequalities
boundary = """\
2
500000 1
1 2
"""
assert run(boundary) == "2\n0\n0\n", "maximum and minimum weights"

# Maximum-size test, all weights equal, chain.
# For an equal-weight chain, the answer for k B-vertices is
# C(n-k, 2), so the expected output can be generated directly.
n = 500000
weights = " ".join(["1"] * n)
edges = "\n".join(f"{i} {i + 1}" for i in range(1, n))
maximum_input = f"{n}\n{weights}\n{edges}\n"

expected = "\n".join(
    str((n - k) * (n - k - 1) // 2)
    for k in range(n + 1)
) + "\n"

assert run(maximum_input) == expected, "maximum-size all-equal chain"
```Các bài kiểm tra tùy chỉnh thực hiện các phần khác nhau của đạo hàm. Trường hợp đỉnh đơn kiểm tra quy ước độ sâu gốc và tất cả các ranh giới cặp trống. Chuỗi có trọng lượng bằng nhau kiểm tra loại cặp đặc biệt và khả năng bẻ gãy theo độ sâu. Ngôi sao có trọng lượng bằng nhau kiểm tra sự phân nhánh giữa các con cháu có trọng lượng bằng nhau. Trường hợp trọng số biên kiểm tra các so sánh nghiêm ngặt và không nghiêm ngặt. Chuỗi kích thước tối đa kiểm tra cả khả năng mở rộng và công thức trên toàn bộ phạm vi k. 

| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 7`|`0 0`| N tối thiểu và độ sâu gốc | 
| Chuỗi 3 đỉnh có trọng lượng bằng nhau |`3 1 0 0`| Trọng lượng tổ tiên bằng nhau và ràng buộc | 
| Ngôi sao có trọng lượng bằng nhau gồm 4 đỉnh |`6 3 1 0 0`| Phân nhánh con cháu có trọng lượng bằng nhau | 
| Hai đỉnh có trọng số`500000 1`|`2 0 0`| So sánh trọng lượng nghiêm ngặt và giá trị biên | 
| Xích có trọng lượng bằng nhau có n = 500000 | (\binom{500000-k}{2}) | Triển khai bộ nhớ tuyến tính và n tối đa | 

## Vỏ cạnh 

Đối với đầu vào một đỉnh```
1
7
```DFS chỉ định độ sâu bằng 0 và kích thước cây con là một. Không có tổ tiên và không có con cháu, nên cả hai đều`anc_greater`Và`desc_le`là số không. Điểm số là (1-1=0) và đường cơ sở là (\binom12=0). Cả k = 0 và k = 1 đều tạo ra số 0. 

Đối với chuỗi có trọng lượng bằng nhau```
3
1 1 1
1 2
2 3
```mọi so sánh tổ tiên đều có trọng số bằng nhau, do đó số lượng tổ tiên lớn hơn đều bằng 0. Điểm số đều là 2 vì đỉnh đầu tiên có hai đỉnh đủ tiêu chuẩn, đỉnh thứ hai có một con cháu cộng với một đỉnh nằm ngoài cây con của nó và đỉnh thứ ba có hai đỉnh nằm ngoài cây con của nó. Cả ba tỷ số đều hòa. Sắp xếp theo độ sâu giảm dần sẽ cho thứ tự 3, 2, 1, do đó mọi tiền tố đều được đóng xuống. Kết quả trả lời là 3, 1, 0, 0. 

Đối với ngôi sao có trọng lượng bằng nhau```
4
1 1 1 1
1 2
1 3
1 4
```gốc có điểm 3 vì nó có ba con cháu có trọng số bằng nhau và mỗi lá cũng có điểm 3 vì ba đỉnh nằm bên ngoài cây con của nó. Dây buộc sâu đặt tất cả các lá trước gốc. Với hai đỉnh trong B, hai lá được chọn, để lại gốc và một lá ở A, do đó chi phí chính xác bằng độ sâu của lá còn lại, 1. Với ba đỉnh trong B, tất cả các lá đều được chọn và A chỉ chứa gốc, cho kết quả bằng 0. 

Đối với trường hợp ranh giới nghiêm ngặt```
2
500000 1
1 2
```gốc có trọng số lớn hơn đỉnh con của nó, vì vậy cặp này là một hình phạt bên A khi cả hai đỉnh đều nằm trong A. Đường cơ sở là (\binom22+1=2). Trẻ có điểm 2 nên việc chọn B sẽ giảm chi phí xuống 0. Việc chọn cả hai đỉnh cho B cũng tốn 0 vì Kyon chỉ không thích tổ tiên có trọng số nhỏ hơn con cháu của nó, điều này sai ở đây. 

Đối với chuỗi có trọng lượng bằng nhau có kích thước tối đa, mỗi cặp đỉnh là cặp tổ tiên và do đó không bị phạt cặp. Chi phí duy nhất là tổng độ sâu của các đỉnh còn lại trong A. Tập B tối ưu bao gồm k đỉnh sâu nhất, để lại n-k đỉnh đầu tiên trong A. Độ sâu của chúng có tổng bằng 

\frac{(n-k)(n-k-1)}2. 
] 

Công thức triển khai tạo ra biểu thức giống hệt nhau, xác nhận rằng hiệu chỉnh bậc hai và liên kết trọng số bằng nhau hoạt động chính xác ngay cả ở kích thước đầu vào tối đa.
