---
title: "CF 102215D - Bộ phận Quốc gia"
description: "Chúng ta có một cây thành phố. Đối với mỗi dự đoán, một số đỉnh có màu đỏ, một số có màu xanh lam và tất cả các đỉnh còn lại là trung tính. Chúng tôi có thể loại bỏ bất kỳ con đường nào chúng tôi thích."
date: "2026-08-18T11:50:13+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102215
codeforces_index: "D"
codeforces_contest_name: "2019, XII Samara Regional Intercollegiate Programming Contest"
rating: 0
weight: 102215
solve_time_s: 328
verified: true
draft: false
---

[CF 102215D - Bộ phận quốc gia](https://codeforces.com/problemset/problem/102215/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 5 phút 28s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây thành phố. Đối với mỗi dự đoán, một số đỉnh có màu đỏ, một số có màu xanh lam và tất cả các đỉnh còn lại là trung tính. Chúng tôi có thể loại bỏ bất kỳ con đường nào chúng tôi thích. Sau khi loại bỏ chúng, mọi thành phố màu đỏ vẫn phải thuộc về một thành phần được kết nối, mọi thành phố màu xanh phải thuộc về một thành phần được kết nối khác và không thành phố màu đỏ nào được kết nối với bất kỳ thành phố màu xanh nào. 

Khó khăn chính là những con đường chúng ta loại bỏ được chia sẻ bởi nhiều con đường khả thi. Việc kết nối hai thành phố màu đỏ có thể buộc chúng ta phải giữ đường đi qua các thành phố trung lập và những thành phố trung lập đó có thể trở thành một phần của vùng màu đỏ. Điều tương tự cũng xảy ra với các thành phố xanh. Câu hỏi thực sự là liệu cây con kết nối tối thiểu chứa tất cả các đỉnh màu đỏ có thể tách rời khỏi cây con kết nối tối thiểu chứa tất cả các đỉnh màu xanh hay không. Đầu vào chính thức có tới (200000) thành phố và tổng số đỉnh màu trên tất cả các dự đoán cũng nhiều nhất là (200000). 

Đối với (n=200000), thuật toán (O(n^2)) vượt xa giới hạn hai giây. Mục tiêu hữu ích là tiền xử lý đại khái (O(n\log n)), theo sau là (O((r+b)\log n)) hoạt động trên mỗi dự đoán, vì tổng (r+b) trên mỗi dự đoán chỉ là (200000). Chúng tôi có đủ khả năng thực hiện công việc logarit cho từng thành phố được tô màu, nhưng chúng tôi không thể đủ khả năng quét tất cả (n) thành phố cho mọi dự đoán. 

Có một số trường hợp đặc biệt không thể giải thích được vấn đề một cách đơn giản hơn. Đầu tiên, cây con Steiner màu đỏ và màu xanh lam có thể gặp nhau tại một thành phố trung tính mặc dù không có thành phố nào có cả hai màu. Ví dụ:```
5
1 2
1 3
2 4
3 5
1
2 2 4 5 3
```Ở đây các thành phố màu đỏ là (2,4) và các thành phố màu xanh là (5,3). Kết nối màu đỏ sử dụng (2-1), trong khi kết nối màu xanh lam sử dụng (3-1), vì vậy cả hai vùng được kết nối đều chứa thành phố (1). Câu trả lời đúng là`NO`. Một giải pháp bất cẩn chỉ kiểm tra xem các đỉnh được tô màu rõ ràng chồng lên nhau có trả về không chính xác hay không`YES`. 

Hiện tượng tương tự xuất hiện trong mẫu đầu tiên. Đối với dự đoán thứ hai, các thành phố màu đỏ là (4,6) và các thành phố màu xanh là (5,7). Các đỉnh được tô màu của chúng hoàn toàn tách biệt, nhưng cả hai kết nối bắt buộc đều đi qua thành phố (1), vì vậy câu trả lời là`NO`. 

Trường hợp cạnh thứ hai xảy ra khi cây con bắt buộc của một màu chứa LCA của màu kia. Hãy xem xét chuỗi```
4
1 2
2 3
3 4
1
2 1 1 3 4
```Các thành phố màu đỏ là (1,3), vì vậy cây con yêu cầu của chúng chứa (1,2,3). Thành phố xanh là (4). Chúng có thể được phân tách bằng cách cắt cạnh (3-4), vì vậy câu trả lời là`YES`. Việc LCA màu đỏ là tổ tiên của đỉnh màu xanh không tự nó làm cho câu trả lời là phủ định. 

Trường hợp ngược lại là```
4
1 2
2 3
3 4
1
2 1 1 4 3
```Các thành phố màu đỏ là (1,4), trong khi màu xanh lam là (3). Đường màu đỏ là toàn bộ chuỗi và nhất thiết phải chứa thành phố màu xanh, vì vậy câu trả lời là`NO`. Một bài kiểm tra chỉ so sánh hai đỉnh LCA sẽ bỏ sót điều này. 

## Phương pháp tiếp cận 

Về mặt khái niệm, một cách tiếp cận bạo lực trực tiếp là có thể. Đối với mỗi con đường, hãy xóa con đường đó và kiểm tra hai thành phần tạo thành. Chúng ta có thể kiểm tra xem tất cả các thành phố màu đỏ có nằm ở một bên và tất cả các thành phố màu xanh lam có nằm ở phía bên kia hay không và liệu các nhóm màu đỏ và xanh lam có được kết nối với nhau hay không. Vì có (n-1) đường ứng cử viên và quá trình xác minh đầy đủ có thể kiểm tra (O(n)) thành phố nên một dự đoán có thể mất (O(n^2)) thời gian. Số lượng dự đoán có thể lớn tới (100000), vì mỗi dự đoán đều chứa ít nhất một thành phố màu đỏ và một thành phố màu xanh trong khi tổng số thành phố có màu được giới hạn bởi (200000). Do đó, một cấu trúc trong trường hợp xấu nhất có thể đạt tới khoảng (10^5\cdot 2\cdot 10^5=2\cdot10^{10}) kiểm tra đỉnh, điều này gần như không khả thi. 

Lực lượng vũ phu hoạt động vì sự phân chia hợp lệ của cây luôn được thể hiện bằng các cạnh cắt giữa các thành phần được kết nối. Vấn đề là tìm ra những thành phần đó mà không xem xét rõ ràng mọi cạnh. 

Quan sát hữu ích là, bên trong một cái cây, chỉ có một con đường duy nhất giữa hai thành phố bất kỳ. Do đó, nếu một số thành phố đỏ phải kết nối với nhau thì mọi con đường giữa chúng đều bị buộc phải thực hiện. Hợp của chúng là một cây con kết nối tối thiểu duy nhất. Điều tương tự cũng đúng với các thành phố xanh. Đây chính xác là phiên bản cây của cây con Steiner, sơ đồ con được kết nối tối thiểu chứa một tập hợp các thiết bị đầu cuối được chỉ định. 

Nếu hai cây con được yêu cầu không có đỉnh riêng biệt, chúng ta có thể giữ mọi cạnh bên trong mỗi cây con và cắt các cạnh ngăn cách chúng với phần còn lại của cây. Các thành phố màu đỏ vẫn được kết nối, các thành phố màu xanh vẫn được kết nối và hai nhóm không thể tiếp cận nhau. Nếu hai cây con giao nhau, không có lựa chọn loại bỏ đường nào có thể hữu ích, bởi vì mọi thành phần được kết nối chứa tất cả các thành phố màu đỏ phải chứa cây con màu đỏ và mọi thành phần được kết nối chứa tất cả các thành phố màu xanh lam phải chứa cây con màu xanh lam. 

Bây giờ hãy nhổ cây ở thành phố (1). Đối với bất kỳ tập đỉnh nào, hãy để LCA của nó là tổ tiên chung thấp nhất của tất cả các đỉnh trong tập đó. Nếu (A) là LCA của tất cả các thành phố màu đỏ thì mọi đỉnh màu đỏ đều nằm trong cây con của (A). Tương tự, nếu (B) là LCA của tất cả các thành phố màu xanh lam thì mọi đỉnh màu xanh lam đều nằm trong cây con của (B). 

Khi đó chỉ có hai khả năng về cấu trúc. Nếu cả (A) và (B) đều không phải là tổ tiên của cây kia thì các cây con gốc của chúng là rời nhau, do đó hai cây con cần thiết là rời nhau và câu trả lời là`YES`. Nếu (A) là tổ tiên của (B), cây con bắt buộc màu xanh nằm bên trong cây con của (B). Cách duy nhất để cây con màu đỏ có thể giao nhau với nó là để chính thành phố màu đỏ nào đó nằm bên trong cây con của (B). Đối số đối xứng được áp dụng khi (B) là tổ tiên của (A). 

Điều này mang lại đặc tính dựa trên LCA tương tự được sử dụng bởi các giải pháp đã biết cho vấn đề này. 

Chúng ta có thể tìm LCA của hai đỉnh trong (O(\log n)) bằng cách nâng nhị phân. Sau đó, LCA của toàn bộ lớp màu được lấy tăng dần: bắt đầu với thành phố màu đỏ đầu tiên và liên tục thay thế LCA hiện tại bằng LCA của nó và thành phố màu đỏ tiếp theo. Quá trình tương tự sẽ tạo ra LCA màu xanh lam. Vì tổng số thành phố được tô màu trong tất cả các dự đoán nhiều nhất là (200000), nên tốc độ này đủ nhanh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(qn^2)) ở dạng trực tiếp | (O(n)) | Quá chậm | 
| Tối ưu | (O(n\log n + S\log n)), trong đó (S\le 200000) là tổng số thành phố có màu | (O(n\log n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lấy gốc cây tại thành phố (1) và tính toán độ sâu và thành phố gốc trực tiếp của mỗi thành phố. Mối quan hệ cha cung cấp cho chúng ta cấu trúc cần thiết cho các truy vấn LCA. 
2. Xây dựng bàn nâng nhị phân.`up[k][v]`lưu trữ tổ tiên của (v) có (2^k) cạnh phía trên nó. Điều này cho phép chúng ta di chuyển lên trên một khoảng cách lớn trong nhiều phép toán logarit. 
3. Đối với mỗi dự đoán, hãy đọc tất cả các thành phố màu đỏ và tính toán dần dần LCA chung của chúng. Bắt đầu với thành phố màu đỏ đầu tiên`red_lca`. Với mỗi thành phố đỏ tiếp theo (x), đặt`red_lca = LCA(red_lca, x)`. Đỉnh kết quả là đỉnh thấp nhất vốn là tổ tiên của mọi thành phố đỏ. 
4. Thực hiện tương tự cho tất cả các thành phố màu xanh lam và nhận được`blue_lca`. 
5. Tính toán`common = LCA(red_lca, blue_lca)`. Nếu như`common`khác với cả hai LCA, thì LCA màu không phải là tổ tiên của LCA kia. Hai vùng bắt buộc nằm trong các cây con khác nhau của`common`, do đó đầu ra`YES`. 
6. Nếu`red_lca == common`, thì LCA màu đỏ là tổ tiên của LCA màu xanh. Cây con bắt buộc màu xanh được chứa trong cây con có gốc tại`blue_lca`. Kiểm tra mọi thành phố màu đỏ (x). Nếu như`LCA(x, blue_lca) == blue_lca`, thì (x) nằm bên trong cây con blue-LCA. Vì cây con đỏ phải kết nối (x) với các thành phố đỏ khác nên nó đạt tới`blue_lca`, thuộc về cây con bắt buộc màu xanh lam. Các vùng giao nhau, do đó đầu ra`NO`. Nếu không có thành phố màu đỏ nào nằm ở đó, xuất ra`YES`. 
7. Trường hợp còn lại là`blue_lca == common`, do đó LCA màu xanh là tổ tiên của LCA màu đỏ. Thực hiện kiểm tra đối xứng trên mỗi thành phố xanh, kiểm tra xem`LCA(x, red_lca) == red_lca`. Một giao lộ cho`NO`; nếu không thì hai vùng bắt buộc sẽ rời nhau và câu trả lời là`YES`. 

Bất biến đằng sau thuật toán là`red_lca`Và`blue_lca`luôn mô tả gốc bắt buộc của các cây con được kết nối theo yêu cầu tương ứng của chúng. Khi hai LCA nằm trong các cây con gốc riêng biệt thì các vùng được yêu cầu không thể đáp ứng được. Khi một LCA chứa LCA kia, giao điểm chỉ có thể xảy ra bên trong cây con của LCA con cháu và giao lộ đó tồn tại chính xác khi màu đối diện có điểm cuối ở đó. Điều này đặc trưng cho mọi sự sắp xếp có thể, vì vậy mọi`YES`tương ứng với một tập hợp các đường cắt có thể thực hiện được và mọi`NO`tương ứng với một giao lộ không thể tránh khỏi. 

## Giải pháp Python```python
import sys
from array import array

input = sys.stdin.readline

def solve(reader=None):
    input = reader if reader is not None else sys.stdin.readline

    n = int(input())

    graph = [[] for _ in range(n + 1)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        graph[u].append(v)
        graph[v].append(u)

    # Root the tree at 1.
    parent = array('i', [0]) * (n + 1)
    depth = array('i', [0]) * (n + 1)

    parent[1] = 1
    stack = [1]

    while stack:
        u = stack.pop()
        pu = parent[u]
        du = depth[u] + 1

        for v in graph[u]:
            if v == pu:
                continue
            parent[v] = u
            depth[v] = du
            stack.append(v)

    # Binary lifting table.
    log = n.bit_length()
    up = [parent]

    for _ in range(1, log):
        prev = up[-1]
        cur = array('i', (prev[prev[v]] for v in range(n + 1)))
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

        for k in range(log - 1, -1, -1):
            ua = up[k][a]
            ub = up[k][b]
            if ua != ub:
                a = ua
                b = ub

        return up[0][a]

    q = int(input())
    answers = []

    for _ in range(q):
        parts = list(map(int, input().split()))
        r, b = parts[0], parts[1]

        red = parts[2:2 + r]
        blue = parts[2 + r:2 + r + b]

        red_lca = red[0]
        for x in red[1:]:
            red_lca = lca(red_lca, x)

        blue_lca = blue[0]
        for x in blue[1:]:
            blue_lca = lca(blue_lca, x)

        common = lca(red_lca, blue_lca)

        if red_lca != common and blue_lca != common:
            answers.append("YES")
            continue

        if red_lca == common:
            possible = True

            for x in red:
                if lca(x, blue_lca) == blue_lca:
                    possible = False
                    break

            answers.append("YES" if possible else "NO")
        else:
            possible = True

            for x in blue:
                if lca(x, red_lca) == red_lca:
                    possible = False
                    break

            answers.append("YES" if possible else "NO")

    return "\n".join(answers)

if __name__ == "__main__":
    sys.stdout.write(solve())
```Danh sách kề lưu trữ cây ban đầu. DFS lặp lại chứ không phải đệ quy vì cây có thể là một chuỗi có độ dài (200000), vượt quá độ sâu đệ quy thông thường của Python. 

các`parent`mảng là một mảng số nguyên nhỏ gọn thay vì danh sách Python. Biểu diễn tương tự được sử dụng cho mọi cấp độ nâng nhị phân. Điều này quan trọng trong giới hạn bộ nhớ (256) MB vì ​​danh sách số nguyên (200000) của Python mang nhiều chi phí hơn đáng kể so với một mảng số nguyên được đóng gói. của Python`array`type lưu trữ các phần tử số có kích thước cố định một cách nhỏ gọn. 

Gốc có chính nó là cha mẹ của nó. Do đó, các lần nhảy lặp lại phía trên gốc vẫn ở đỉnh (1), điều này làm cho việc triển khai LCA trở nên đơn giản và tránh việc xử lý đặc biệt đối với tổ tiên bằng 0. 

Hàm LCA trước tiên cân bằng độ sâu bằng cách sử dụng biểu diễn nhị phân của chênh lệch của chúng. Sau đó, nó xem xét các bước nhảy từ lũy thừa lớn nhất của hai xuống dưới. Nếu hai đỉnh tương ứng khác nhau thì cả hai đỉnh đều có thể nhảy lên trên một cách an toàn vì LCA của chúng hoàn toàn nằm trên các đỉnh đó. Khi không thể thực hiện được bước nhảy lớn hơn, cha mẹ trực tiếp của chúng là LCA. 

Đối với mỗi truy vấn, danh sách màu đỏ và màu xanh được cắt trực tiếp từ dòng đầu vào. Các ràng buộc đảm bảo rằng một truy vấn hoàn chỉnh phù hợp với một dòng đầu vào. Danh sách được giữ lại vì trong trường hợp tổ tiên, chúng ta phải kiểm tra mọi thiết bị đầu cuối có màu đối lập. 

Không có vấn đề tràn số nguyên trong Python. Giá trị lớn nhất được sử dụng làm mã định danh đỉnh, độ sâu và mục nhập bảng nhiều nhất là (200000). 

Việc sử dụng`array('i')`cũng giữ cho bàn nâng nhị phân (O(n\log n)) nhỏ gọn. Bảng có khoảng (200000\cdot18) mục nhập số nguyên và mỗi số nguyên được đóng gói chiếm bốn byte khi triển khai thông thường, do đó, bản thân bảng chỉ có khoảng mười lăm megabyte thay vì hàng trăm megabyte đối tượng số nguyên Python. 

## Ví dụ đã hoạt động 

Dấu vết đầu tiên sử dụng dự đoán đầu tiên của Mẫu 1. 

Cây có gốc tại (1). Các đỉnh màu đỏ là (2,4) nên LCA chung của chúng là (2). Các đỉnh màu xanh là (6,7) nên LCA chung của chúng là (3). 

| Sân khấu | LCA đỏ | LCA xanh | LCA chung | Quyết định | 
| --- | --- | --- | --- | --- | 
| Bắt đầu với màu đỏ (2) | 2 | | | | 
| Thêm màu đỏ (4) | 2 | | | | 
| Bắt đầu với màu xanh (6) | 2 | 6 | | | 
| Thêm màu xanh (7) | 2 | 3 | | | 
| Tính toán`LCA(2,3)`| 2 | 3 | 1 |`YES`| 

Vì (1) khác với cả (2) và (3), hai LCA màu nằm trong các cây con khác nhau của gốc. Cây con bắt buộc màu đỏ là (2-4), trong khi cây con bắt buộc màu xanh lam là (3-6) và (3-7). Chúng rời rạc nên các con đường có thể bị cắt giữa các vùng đó và câu trả lời là`YES`. 

Dấu vết thứ hai sử dụng dự đoán thứ hai của Mẫu 1. Các đỉnh màu đỏ của nó là (4,6), trong khi các đỉnh màu xanh của nó là (5,7). 

| Sân khấu | LCA đỏ | LCA xanh | LCA chung | Quyết định | 
| --- | --- | --- | --- | --- | 
| Bắt đầu với màu đỏ (4) | 4 | | | | 
| Thêm màu đỏ (6) | 1 | | | | 
| Bắt đầu với màu xanh (5) | 1 | 5 | | | 
| Thêm màu xanh (7) | 1 | 1 | | | 
| Tính toán`LCA(1,1)`| 1 | 1 | 1 | Vụ án tổ tiên | 
| Kiểm tra màu đỏ (4) | 1 | 1 |`LCA(4,1)=1`|`NO`| 

Ở đây cả hai LCA màu đều là (1). Cây con bắt buộc màu đỏ chứa đường dẫn giữa (4) và (6), đi qua (1). Cây con bắt buộc màu xanh chứa đường dẫn giữa (5) và (7), cũng đi qua (1). Đỉnh chung là điều khó tránh khỏi nên không một con đường ngăn cách nào có thể chia cắt được hai nhóm. 

Như một ví dụ độc lập thứ hai, hãy xem xét chuỗi này:```
5
1 2
2 3
3 4
4 5
2
2 1 1 3 5
2 1 1 5 3
```Truy vấn đầu tiên có màu đỏ (1,3) và xanh lam (5). LCA màu đỏ là (1), LCA màu xanh lam là (5) và`LCA(1,5)=1`. LCA màu đỏ là trường hợp tổ tiên, nhưng không thành phố màu đỏ nào nằm trong cây con của (5), vì vậy kết quả là`YES`. 

Truy vấn thứ hai có màu đỏ (1,5) và màu xanh lam (3). LCA màu đỏ là (1), LCA màu xanh lam là (3) và một lần nữa LCA màu đỏ là tổ tiên. Lần này thành phố đỏ (5) thỏa mãn`LCA(5,3)=3`, nghĩa là nó nằm bên trong cây con có gốc tại (3). Con đường màu đỏ buộc phải đi qua thành phố (3), nên kết quả là`NO`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n\log n + S\log n)) | Tòa nhà nâng nhị phân mất (O(n\log n)); mỗi thành phố da màu chỉ tham gia vào một số hoạt động LCA không đổi và (S\le200000) | 
| Không gian | (O(n\log n)) | Bảng nâng chứa (O(n\log n)) số nguyên được đóng gói, trong khi cây và bộ lưu trữ truy vấn sử dụng (O(n)) không gian bổ sung | 

Với (n\le200000), quá trình tiền xử lý có khoảng mười tám mức nâng nhị phân. Tổng số thành phố được tô màu cũng nhiều nhất là (200000), do đó công việc truy vấn vẫn bị giới hạn bởi vài triệu phép tính tổ tiên logarit. Quá trình truyền tải lặp lại sẽ tránh được các vấn đề về độ sâu đệ quy và bàn nâng được đóng gói giúp giữ bộ nhớ thoải mái trong giới hạn (256) MB. 

## Trường hợp thử nghiệm 

Khai thác thử nghiệm sau đây giả định giải pháp trên được lưu dưới dạng`solution.py`. Nó kêu giống nhau`solve`hoạt động với một`StringIO`reader, do đó các xác nhận thực hiện việc triển khai thực tế chứ không phải là một thuật toán tham chiếu riêng biệt.```
# solution.py must contain the solve(reader=None) function from above.

import io
from solution import solve

def run(inp: str) -> str:
    return solve(io.StringIO(inp).readline)

# Provided sample
sample1 = """\
7
1 2
1 3
2 4
2 5
3 6
3 7
6
2 2 4 5 6 7
2 2 4 6 5 7
2 1 4 5 2
2 1 4 5 1
1 1 1 2
6 1 1 2 3 4 5 6 7
"""

assert run(sample1) == """\
YES
NO
NO
YES
YES
YES
""".strip(), "sample 1"

# Minimum-size tree. With two cities, one red and one blue,
# cutting the only road always separates them.
assert run("""\
2
1 2
1
1 1 1 2
""") == "YES", "minimum-size tree"

# A chain where the red path contains the blue city.
assert run("""\
5
1 2
2 3
3 4
4 5
1
2 1 1 5 3
""") == "NO", "intersection on a required path"

# A chain where the groups can be separated at one edge.
assert run("""\
5
1 2
2 3
3 4
4 5
1
2 1 1 3 5
""") == "YES", "ancestor case with valid separation"

# Every city is colored. Red leaves must connect through the blue center,
# so the answer is NO.
assert run("""\
5
1 2
1 3
1 4
1 5
1
4 1 2 3 4 5
""") == "NO", "all cities colored"

# Maximum-size test. The tree is a star with 199999 red leaves and
# the center blue. Connecting all red leaves forces the blue center
# into the red component.
n = 200000
edges = "\n".join(f"1 {v}" for v in range(2, n + 1))
red = " ".join(map(str, range(2, n + 1)))

max_case = (
    f"{n}\n"
    f"{edges}\n"
    "1\n"
    f"{n - 1} 1 {red} 1\n"
)

assert run(max_case) == "NO", "maximum-size star"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| (n=2), một màu đỏ và một màu xanh |`YES`| Cây tối thiểu và thực tế là chỉ cần một cạnh ngăn cách | 
| Dây chuyền có màu đỏ (1,5) và xanh lam (3) |`NO`| Một thành phố trung tính hoặc có màu đối lập nằm trên con đường màu đỏ bắt buộc | 
| Dây chuyền có màu đỏ (1,3) và xanh lam (5) |`YES`| Trường hợp tổ tiên trong đó hai cây con bắt buộc vẫn tách biệt | 
| Sao với mọi thành phố được tô màu |`NO`| Tất cả các thành phố được tô màu và một giao lộ bắt buộc ở trung tâm | 
| Ngôi sao với (200000) thành phố |`NO`| Tối đa (n), kích thước truy vấn tối đa và hành vi bộ nhớ/thời gian | 

Câu lệnh yêu cầu tất cả số nhận dạng thành phố bên trong dự đoán phải khác biệt, do đó, phép kiểm tra theo nghĩa đen trong đó tất cả các giá trị thành phố đầu vào bằng nhau sẽ không hợp lệ. Ngôi sao đủ màu là trường hợp ranh giới có liên quan: mỗi thành phố thuộc về một trong hai lớp màu, không có thành phố trung lập nào có thể hấp thụ một giao lộ. 

## Vỏ cạnh 

Đối với một thành phố màu đỏ và một thành phố màu xanh, mỗi cây con bắt buộc chỉ bao gồm đỉnh được tô màu của nó. Vì hai mã định danh thành phố là khác nhau nên hai cây con không thể giao nhau. Thuật toán được`red_lca = red`,`blue_lca = blue`và LCA của họ là một trong số đó, vì vậy nó đạt đến trường hợp tổ tiên. Kiểm tra ngăn chặn không thành công vì thành phố có màu đối lập không thể nằm trong cây con của đỉnh riêng biệt của nó. Câu trả lời là`YES`. 

Đối với giao điểm tại một đỉnh trung hòa, hãy xem xét```
5
1 2
1 3
2 4
3 5
1
2 2 4 5 3
```LCA màu đỏ là (2), vì các thành phố màu đỏ là (2,4). LCA màu xanh lam là (3), vì các thành phố màu xanh lam là (5,3). LCA chung của họ là (1), khác với cả hai. Thoạt nhìn, trường hợp này trông giống như trường hợp riêng biệt, nhưng cây con yêu cầu màu đỏ là (2-4), trong khi cây con yêu cầu màu xanh lam chỉ là (3-5). Chúng thực sự không giao nhau nên câu trả lời đúng là`YES`cho đầu vào cụ thể này. 

Để thực hiện giao lộ trung lập thực sự, hãy sử dụng```
5
1 2
1 3
2 4
3 5
1
2 2 5 4 3
```Các thành phố màu đỏ là (5,4), vì vậy LCA của họ là (1). Các thành phố màu xanh lam là (3,4) sẽ vi phạm các màu riêng biệt, vì vậy thay vào đó, ví dụ rõ ràng là cấu trúc mẫu có màu đỏ (4,6) và xanh lam (5,7):```
7
1 2
1 3
2 4
2 5
3 6
3 7
1
2 2 4 6 5 7
```LCA màu đỏ là (1), LCA màu xanh lam là (1) và thuật toán sẽ chuyển ngay sang trường hợp tổ tiên. Kiểm tra thành phố màu đỏ (4) với LCA màu xanh (1) cho kết quả`LCA(4,1)=1`, do đó giao điểm được phát hiện và đầu ra là`NO`. 

Đối với mối quan hệ tổ tiên vẫn có thể tách rời, hãy xem xét```
5
1 2
2 3
3 4
4 5
1
2 1 1 3 5
```LCA màu đỏ là (1), LCA màu xanh lam là (5) và LCA thông thường là (1). Thuật toán kiểm tra xem thành phố màu đỏ có nằm trong cây con của (5) hay không. Cũng không, vì vậy nó trả về`YES`. Điểm cắt (4-5) tách thành phố xanh trong khi vẫn kết nối các thành phố đỏ. 

Đối với trường hợp ngược lại,```
5
1 2
2 3
3 4
4 5
1
2 1 1 5 3
```LCA màu đỏ là (1), LCA màu xanh lam là (3) và LCA thông thường là (1). Thành phố đỏ (5) nằm trong cây con gốc tại (3), được phát hiện bởi`LCA(5,3) == 3`. Kết nối màu đỏ phải đi qua thành phố xanh (3) nên thuật toán trả về`NO`. 

Cuối cùng, ngôi sao có kích thước tối đa```
200000
1 2
1 3
...
1 200000
1
199999 1 2 3 ... 200000 1
```có mỗi lá màu đỏ và ở giữa màu xanh. LCA của tất cả các thành phố màu đỏ là (1), cũng là LCA màu xanh. Chiếc lá đỏ đầu tiên đã thỏa mãn`LCA(2,1) == 1`, do đó truy vấn kết thúc với`NO`. Điều này chứng tỏ rằng thuật toán có thể từ chối một truy vấn lớn ngay lập tức khi nó tìm thấy giao điểm bắt buộc, trong khi vẫn xử lý toàn bộ đầu vào (200000)-đỉnh trong giới hạn tiệm cận dự kiến.
