---
title: "CF 102407K - Sự sắp xếp điên rồ"
description: "Bản thân cái cây không phải là khó khăn thực sự. Mỗi cạnh cây nhận được một bit và mọi đường dẫn được yêu cầu sẽ tạo ra XOR của các bit trên đường dẫn đó. Chúng ta cần chuỗi kết quả của các XOR đường dẫn không giảm."
date: "2026-08-11T16:27:43+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102407
codeforces_index: "K"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2019-2020, \u0412\u0442\u043e\u0440\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430, \u0443\u0441\u043b\u043e\u0436\u043d\u0435\u043d\u043d\u0430\u044f \u043d\u043e\u043c\u0438\u043d\u0430\u0446\u0438\u044f"
rating: 0
weight: 102407
solve_time_s: 483
verified: false
draft: false
---

[CF 102407K - Những sự sắp xếp điên rồ](https://codeforces.com/problemset/problem/102407/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 8 phút 3 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Bản thân cái cây không phải là khó khăn thực sự. Mỗi cạnh cây nhận được một bit và mọi đường dẫn được yêu cầu sẽ tạo ra XOR của các bit trên đường dẫn đó. Chúng ta cần chuỗi kết quả của các XOR đường dẫn không giảm. 

Vì mọi đường dẫn XOR đều là 0 hoặc 1 nên chuỗi không giảm có dạng rất cụ thể. Đối với một số ranh giới (k), giá trị đường dẫn (k) đầu tiên là 0 và tất cả các giá trị còn lại là 1. Do đó, thay vì xem xét các chuỗi nhị phân tùy ý, chúng ta chỉ cần xem xét các chuỗi có thể có (m+1) 

[ 
0^k1^{m-k}, \qquad 0\le k\le m. 
] 

Cây đầu vào có (n) đỉnh và (n-1) cạnh, theo sau là (m) cặp thứ tự mô tả các đường dẫn được yêu cầu. Đầu ra được yêu cầu là số lượng phép gán bit cạnh tạo ra một trong các chuỗi XOR đường dẫn không giảm, modulo (998244353). Giới hạn chính thức là (n,m\le250000), với giới hạn thời gian 2 giây và giới hạn bộ nhớ 512 MB. 

Kích thước của (m) ngay lập tức loại trừ việc kiểm tra mọi ranh giới có thể có bằng cách duyệt đồ thị mới. Làm như vậy sẽ tốn phí (O(m(n+m))). Việc liệt kê các phép gán (2^{n-1}) của các cạnh cây ban đầu thậm chí còn tệ hơn. Ngay cả sau khi thực hiện việc giảm thiểu hữu ích nhất và kiểm tra phân công ứng viên trong (O(m)), lực lượng vũ phu vẫn cần khoảng 

[ 
250000\cdot 2^{249999} 
] 

hoạt động trong trường hợp xấu nhất. 

Có một số trường hợp ranh giới rất dễ bị xử lý sai. Coi như```
2 2
1
1 2
1 2
```Cả hai đường dẫn được yêu cầu đều giống nhau nên giá trị XOR của chúng luôn bằng nhau. Cả hai phép gán cạnh cây có thể có đều hợp lệ, cho đầu ra 2. Một giải pháp giả sử mọi ranh giới (k) đưa ra một chuỗi khả thi khác nhau sẽ đếm không chính xác mẫu ở giữa (01). 

Điểm tinh tế thứ hai là sự mất kết nối trong biểu đồ được hình thành bởi các cặp được yêu cầu. Vì```
4 2
1 2 3
1 2
3 4
```hai cặp được yêu cầu thuộc về các thành phần khác nhau. Mỗi một trong ba mẫu (00,01,11) đều khả thi và mỗi mẫu khả thi có thể có hai nhãn đỉnh vì hai thành phần có thể được lật độc lập. Do đó, câu trả lời là 6 chứ không phải chỉ 3. 

Chu kỳ tạo ra vấn đề ngược lại. Vì```
3 3
1 2
1 2
2 3
1 3
```các cặp được yêu cầu tạo thành một hình tam giác. Các mẫu (000) và (011) là khả thi, trong khi (001) và (111) thì không. Câu trả lời là 2. Chỉ kiểm tra xem mọi ràng buộc riêng lẻ có thể được thỏa mãn có bỏ lỡ điều kiện chẵn lẻ chu trình hay không. 

Cuối cùng, cả hai ranh giới cực đoan đều quan trọng. (k=0) nghĩa là mọi đường dẫn được yêu cầu đều có XOR 1, trong khi (k=m) nghĩa là mọi đường dẫn được yêu cầu đều có XOR 0. Phương án sau luôn khả thi, do đó việc triển khai chỉ kiểm tra ranh giới giữa hai đường dẫn liên tiếp sẽ mất ít nhất một trường hợp hợp lệ. 

## Phương pháp tiếp cận 

Một lực lượng vũ phu trực tiếp sẽ gán 0 hoặc 1 cho mọi cạnh của cây, tính toán tất cả (m) đường dẫn XOR và kiểm tra xem chúng có giảm không. Cây có (n-1) bit cạnh độc lập, do đó có (2^{n-1}) phép gán. Ngay cả khi mọi đường dẫn XOR đều có sẵn trong (O(1)), việc liệt kê sẽ có giá (O(m2^{n-1})). Với (n=250000), điều đó hoàn toàn không khả thi. 

Quan sát hữu ích là cây cho phép chúng ta thay thế các biến cạnh bằng các biến đỉnh. Chọn một gốc tùy ý và đặt (h_v) là XOR của tất cả các trọng số cạnh trên đường đi từ gốc đến (v). Đối với đường dẫn được yêu cầu giữa (u) và (v), mọi cạnh được chia sẻ bởi hai đường dẫn gốc đều bị hủy, do đó XOR của nó chỉ đơn giản là 

[ 
s_i=h_{u_i}\oplus h_{v_i}. 
] 

Ngược lại, nếu (h_{\text{root}}=0), mọi trọng số cạnh cây được phục hồi duy nhất từ hai nhãn điểm cuối. Vì vậy, cây có thể bị loại bỏ sau quá trình chuyển đổi này. Đây chính xác là mức giảm được sử dụng trong phân tích chính thức. 

Bây giờ tạo một đồ thị (G) khác trên cùng (n) đỉnh. Yêu cầu (i) trở thành một cạnh ((u_i,v_i)). Khi chúng tôi sửa một ranh giới (k), yêu cầu (i) phải đáp ứng 

[ 
h_{u_i}\oplus h_{v_i}= 
\bắt đầu{trường hợp} 
0,&i\le k,\ 
1,&i>k. 
\end{trường hợp} 
] 

Đây là một hệ thống các ràng buộc XOR trên biểu đồ. 

Đối với một ranh giới cố định, hệ thống như vậy nhất quán một cách chính xác khi mỗi chu kỳ có tổng XOR chẵn. DSU chẵn lẻ phát hiện tình trạng này trong khi vẫn duy trì các thành phần được kết nối. Nếu hệ thống nhất quán và (G) có (c) các thành phần được kết nối, thì có (2^c) cách để chọn một bit bắt đầu cho mỗi thành phần. Gốc ban đầu phải được cố định bằng 0, loại bỏ một bit trống, do đó mọi ranh giới khả thi đều góp phần 

[ 
2^{c-1}. 
] 

Vấn đề còn lại là tìm xem có bao nhiêu ranh giới (m+1) khả thi mà không cần xây dựng lại DSU (m+1) lần. 

Cấu trúc chính là ràng buộc cho cạnh (i) chỉ thay đổi một lần khi đường biên di chuyển. Nó là 1 trên các ranh giới (k<i) và 0 trên các ranh giới (k\ge i). Chúng ta có thể xử lý đồng thời tất cả các ranh giới bằng cách phân chia và chinh phục. Tại một nút biểu thị các ranh giới ([l,r]), các cạnh có chỉ số tối đa (l) đã được cố định bằng 0 và các cạnh có chỉ số lớn hơn (r) đã được cố định bằng 1. Khi tách tại (giữa), nửa bên trái có tất cả các cạnh (mid+1,\ldots,r) được cố định bằng 1, trong khi nửa bên phải có tất cả các cạnh (l+1,\ldots,mid) được cố định bằng 0. Các cạnh còn lại được trì hoãn ở các mức sâu hơn. 

DSU chẵn lẻ khôi phục cho phép chúng ta thêm các ràng buộc cố định đó, giải quyết đệ quy một nửa và khôi phục trạng thái trước đó sau đó. Mỗi cạnh chỉ được giới thiệu tại một nút cho mỗi cấp độ chia và chinh phục, do đó có các phần chèn ràng buộc (O(m\log m)). Phân tích chính thức mô tả ý tưởng phân chia và chinh phục tương tự, với DSU khôi phục đưa ra phiên bản (O(m\log^2 m)) và triển khai nén rõ ràng có khả năng (O(m\log m)). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(m2^{n-1})) | (O(n+m)) | Quá chậm | 
| Tối ưu | (O(m\log m\log n)) | (O(n+m\log n)) lịch sử trường hợp xấu nhất | Đã chấp nhận | 

Việc triển khai Python bên dưới sử dụng kết hợp theo kích thước và khôi phục mà không cần nén đường dẫn. Hệ số logarit trong`find`xuất phát từ giới hạn độ sâu liên kết theo kích thước. 

## Hướng dẫn thuật toán 

1. Thay thế mọi phép gán cạnh cây bằng các giá trị XOR đỉnh (h_v). Việc root cây ban đầu là không cần thiết vì việc ánh xạ giữa các phép gán cạnh và phép gán (h) với một giá trị gốc cố định là phỏng đoán. Do đó, chỉ có (m) cặp được yêu cầu mới quan trọng. 
2. Coi mọi cặp được yêu cầu ((u_i,v_i)) là một cạnh của biểu đồ mới. Đối với ranh giới (k), gắn giá trị chẵn lẻ 0 vào các cạnh (1,\ldots,k) và giá trị chẵn lẻ 1 vào các cạnh (k+1,\ldots,m). Một ranh giới khả thi chính xác là một ranh giới mà các ràng buộc chẵn lẻ này thừa nhận các nhãn đỉnh. 
3. Sử dụng DSU chẵn lẻ. Đối với mỗi đỉnh, DSU lưu trữ cha mẹ của nó và XOR giữa đỉnh đó và cha mẹ của nó. Một ràng buộc (h_u\oplus h_v=w) được thêm vào bằng cách tìm các nghiệm của (u) và (v) và XOR của chúng đối với các nghiệm đó. Nếu các gốc khác nhau, hãy hợp nhất chúng và lưu trữ giá trị chẵn lẻ cần thiết. Nếu các nghiệm đã bằng nhau thì ràng buộc sẽ mâu thuẫn chính xác khi XOR hiện có khác với (w). 
4. Chia phạm vi ranh giới ([0,m]) theo cách đệ quy. Tại một nút ([l,r]), tất cả các ràng buộc nằm ngoài phạm vi biến đã được sửa. Đặt (mid=\lfloor(l+r)/2\rfloor). Đối với con bên trái, mọi cạnh có chỉ số lớn hơn (giữa) chắc chắn nằm ở bên 1, vì vậy hãy thêm các cạnh (mid+1,\ldots,r) có chẵn lẻ 1. Đối với con bên phải, mọi cạnh có chỉ số lớn nhất (giữa) chắc chắn nằm ở bên 0, vì vậy hãy thêm các cạnh (l+1,\ldots,mid) có chẵn lẻ 0. 
5. Tại một lá (k), mọi cạnh ngoại trừ cạnh (k) đã được chèn với tính chẵn lẻ chính xác của nó. Nếu (1\le k\le m), chèn cạnh (k) có tính chẵn lẻ bằng 0. Với (k=0), mọi cạnh đều đã là 1 và đối với (k=m), mọi cạnh đều đã là 0. 
6. Nếu DSU chẵn lẻ không có mâu thuẫn ở một lá, hãy đếm ranh giới đó. Tại thời điểm đó, mọi cạnh được yêu cầu đều xuất hiện, do đó số lượng thành phần hiện tại của DSU chính xác là số lượng thành phần (c) của biểu đồ yêu cầu đầy đủ. Số phép gán cây ban đầu được biểu thị bằng ranh giới này là (2^{c-1}). 
7. Đưa DSU trở lại ảnh chụp nhanh được chụp trước khi xử lý nhánh hiện tại. Điều này khôi phục chính xác trạng thái mà nhánh anh em cần, do đó, mọi ranh giới đều được đánh giá chính xác bằng tập hợp các ràng buộc cố định của riêng nó. 

### Tại sao nó hoạt động 

Đối với mọi ranh giới (k), chuỗi nhị phân không giảm được xác định duy nhất là (0^k1^{m-k}). Việc truyền tải phân chia và chinh phục cuối cùng đạt đến chính xác một lá cho (k). Dọc theo đường dẫn từ gốc tới lá, một cạnh có chỉ số nhỏ hơn (k) được chèn với chẵn lẻ 0, một cạnh có chỉ số lớn hơn (k) được chèn với chẵn lẻ 1, và cạnh (k) được chèn với chẵn lẻ 0. Do đó, DSU tại lá biểu thị chính xác hệ thống XOR cho ranh giới (k). 

DSU chẵn lẻ báo cáo sự mâu thuẫn một cách chính xác khi các ràng buộc XOR chứa một chu trình không nhất quán. Nếu không có mâu thuẫn, việc gán một bit tùy ý cho mỗi thành phần được kết nối sẽ xác định tất cả các nhãn đỉnh khác và việc sửa gốc ban đầu bằng 0 sẽ loại bỏ chính xác một bit trống. Do đó, mọi ranh giới khả thi đều đóng góp chính xác (2^{c-1}) phép gán cạnh. Vì các ranh giới khác nhau tương ứng với các chuỗi đường dẫn-XOR khác nhau, nên tập hợp nhiệm vụ của chúng không liên kết với nhau, do đó việc tính tổng các đóng góp của chúng sẽ đưa ra câu trả lời cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def solve_case(n, m, edges):
    parent = list(range(n))
    size = [1] * n
    xr = [0] * n

    components = n
    bad = 0

    # History entries:
    # (child, root, old_size_of_root, bad_delta)
    # child == -1 means no union happened.
    history = []

    def find(x):
        parity = 0
        while parent[x] != x:
            parity ^= xr[x]
            x = parent[x]
        return x, parity

    def add_constraint(u, v, w):
        nonlocal components, bad

        ru, xu = find(u)
        rv, xv = find(v)

        if ru == rv:
            delta = 1 if (xu ^ xv) != w else 0
            if delta:
                bad += 1
            history.append((-1, -1, 0, delta))
            return

        # Union by size keeps the tree height logarithmic.
        if size[ru] > size[rv]:
            ru, rv = rv, ru

        # h[ru] xor h[rv] must equal xu xor xv xor w.
        link_xor = xu ^ xv ^ w

        history.append((ru, rv, size[rv], 0))

        parent[ru] = rv
        xr[ru] = link_xor
        size[rv] += size[ru]
        components -= 1

    def rollback(snapshot):
        nonlocal components, bad

        while len(history) > snapshot:
            child, root, old_size, delta = history.pop()

            if child == -1:
                bad -= delta
            else:
                parent[child] = child
                size[root] = old_size
                components += 1

    valid = 0
    factor = None

    # Boundary k means:
    # edges i <= k have parity 0
    # edges i > k have parity 1
    #
    # At node [l, r], edges <= l are already fixed to 0,
    # and edges > r are already fixed to 1.
    def divide(l, r):
        nonlocal valid, factor

        if bad:
            return

        if l == r:
            snapshot = len(history)

            # Edge l is the only still-unfixed edge.
            # For boundary k=l, it belongs to the zero prefix.
            if 1 <= l <= m:
                u, v = edges[l - 1]
                add_constraint(u, v, 0)

            if bad == 0:
                valid += 1
                if factor is None:
                    factor = pow(2, components - 1, MOD)

            rollback(snapshot)
            return

        mid = (l + r) // 2

        # Left half: k in [l, mid].
        # Edges mid+1 ... r are always on the one side.
        snapshot = len(history)
        for i in range(mid + 1, r + 1):
            u, v = edges[i - 1]
            add_constraint(u, v, 1)

        divide(l, mid)
        rollback(snapshot)

        # Right half: k in [mid+1, r].
        # Edges l+1 ... mid are always on the zero side.
        snapshot = len(history)
        for i in range(l + 1, mid + 1):
            u, v = edges[i - 1]
            add_constraint(u, v, 0)

        divide(mid + 1, r)
        rollback(snapshot)

    divide(0, m)

    return valid * factor % MOD

def main():
    n, m = map(int, input().split())

    # The original tree is irrelevant after the h_v transformation.
    # We only need to consume its parent list.
    input()

    edges = [None] * m
    for i in range(m):
        u, v = map(int, input().split())
        edges[i] = (u - 1, v - 1)

    print(solve_case(n, m, edges))

if __name__ == "__main__":
    main()
```Danh sách cha của cây ban đầu được đọc và loại bỏ. Đây là hành động có chủ ý chứ không phải là sự tối ưu hóa xảy ra trên các mẫu. Khi (h_v) được xác định là XOR gốc-(v), cây ban đầu chỉ cung cấp phép đối chiếu giữa các phép gán cạnh và nhãn đỉnh với một giá trị gốc cố định. Giải pháp chính thức đưa ra nhận xét tương tự rằng câu trả lời không phụ thuộc vào cây cụ thể.`find`trả về cả đại diện và XOR từ đỉnh được truy vấn cho đại diện đó. Mối quan hệ chẵn lẻ được lưu trữ trong`xr[x]`luôn là XOR giữa`x`Và`parent[x]`. Khi hai thành phần khác nhau được hợp nhất, tính chẵn lẻ yêu cầu giữa các gốc của chúng là`xu ^ xv ^ w`. Thứ tự của phép kết không làm thay đổi XOR này, đó là lý do tại sao việc hoán đổi gốc lấy phép kết theo kích thước không yêu cầu thay đổi công thức. 

Mọi sửa đổi đều được ghi lại trong`history`. Một liên minh thành công sẽ lưu trữ gốc con và kích thước cũ của cha mẹ mới. Ràng buộc dư thừa lưu trữ một điểm đánh dấu đặc biệt và ghi lại xem liệu nó có đưa ra mâu thuẫn hay không. Điều này là cần thiết vì ngay cả mâu thuẫn cũng phải được giải quyết khi đệ quy quay trở lại. 

các`components`biến bắt đầu tại (n) và chỉ giảm khi hai thành phần riêng biệt trước đó được nối với nhau. Tại mỗi lá, tất cả (m) cạnh yêu cầu đã được chèn vào, vì vậy giá trị của nó là số thành phần của biểu đồ yêu cầu hoàn chỉnh. Vì biểu đồ đó giống nhau đối với mọi ranh giới nên hệ số (2^{c-1}) giống nhau đối với mọi lá hợp lệ. Mã chỉ tính toán nó một lần. 

Việc xử lý ranh giới là nơi dễ dàng nhất để đưa ra lỗi từng cái một. Một ranh giới (k) có nghĩa chính xác là (k) số 0 theo sau là (m-k) số 1. Do đó, bản thân cạnh (k) có tính chẵn lẻ là 0, trong khi cạnh (k+1) có tính chẵn lẻ là 1. Đây là lý do tại sao một chiếc lá lại thêm cạnh`l - 1`với chẵn lẻ 0 khi`l > 0`. 

Độ sâu đệ quy chỉ là (O(\log m)), do đó giới hạn đệ quy của Python không cần điều chỉnh. DSU cố tình không sử dụng tính năng nén đường dẫn thông thường, bởi vì những thay đổi gốc có tính hủy hoại sẽ phải được ghi lại để khôi phục. Liên kết theo kích thước giữ cho mọi logarit của chuỗi gốc. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Biểu đồ yêu cầu là một hình tam giác có các cạnh 

[ 
(1,2),\quad(2,3),\quad(1,3). 
] 

Nó được kết nối, do đó một hệ thống nhất quán có (2^{1-1}=1) phép gán cây tương ứng. 

| Ranh giới (k) | XOR đường dẫn bắt buộc | Kết quả DSU | Linh kiện | Đóng góp | 
| --- | --- | --- | --- | --- | 
| 0 | 111 | Mâu thuẫn | 1 | 0 | 
| 1 | 011 | Nhất quán | 1 | 1 | 
| 2 | 001 | Mâu thuẫn | 1 | 0 | 
| 3 | 000 | Nhất quán | 1 | 1 | 

Đối với (k=1), tam giác nhận được các số chẵn lẻ (0,1,1), có XOR xung quanh chu kỳ bằng 0. Đối với (k=2), chỉ cạnh cuối cùng có chẵn lẻ 1, do đó chu trình XOR là một và các ràng buộc là không thể. Hai ranh giới khả thi cho đầu ra 2. 

### Mẫu 2 

Biểu đồ yêu cầu có bốn chu kỳ: 

[ 
1-2-3-4-1. 
] 

Một lần nữa, nó được kết nối, do đó mỗi ranh giới nhất quán đóng góp chính xác một phép gán. 

| Ranh giới (k) | XOR đường dẫn bắt buộc | Chu kỳ chẵn lẻ | Kết quả DSU | Đóng góp | 
| --- | --- | --- | --- | --- | 
| 0 | 1111 | 0 | Nhất quán | 1 | 
| 1 | 0111 | 1 | Mâu thuẫn | 0 | 
| 2 | 0011 | 0 | Nhất quán | 1 | 
| 3 | 0001 | 1 | Mâu thuẫn | 0 | 
| 4 | 0000 | 0 | Nhất quán | 1 | 

Các ranh giới khả thi là (0,2,4), đưa ra câu trả lời bắt buộc là 3. Ví dụ này cũng thực hiện cả các ranh giới cực trị và cho thấy tại sao các ranh giới khả thi không nhất thiết phải tạo thành một khoảng liên tục. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(m\log m\log n)) | Mỗi yêu cầu được chèn tối đa một lần cho mỗi cấp độ phân chia và chinh phục và DSU khôi phục`find`lấy (O(\log n)) với liên kết theo kích thước | 
| Không gian | (O(n+m\log m)) trường hợp xấu nhất | Mảng DSU sử dụng (O(n)), đệ quy là (O(\log m)) và lịch sử khôi phục lưu trữ các phần bổ sung đang hoạt động | 

Với (m\le250000), đệ quy có ít hơn 20 cấp độ. Sự khác biệt quan trọng so với việc xây dựng lại hệ thống XOR một cách độc lập cho mọi ranh giới là các ràng buộc được chia sẻ bởi toàn bộ một nửa phạm vi ranh giới chỉ được chèn một lần và được sử dụng lại bởi mọi lá bên dưới nửa đó. Giải pháp chính thức mô tả quan điểm kết nối động chia để trị này. 

## Trường hợp thử nghiệm 

Dây nịt sau đây giả định`solve_case`chức năng từ giải pháp trên có sẵn. Các thử nghiệm bao gồm ba mẫu chính thức, phiên bản hữu ích nhỏ nhất, biểu đồ yêu cầu bị ngắt kết nối, chuỗi hoàn toàn bằng nhau và phiên bản được tạo có kích thước tối đa.```python
import io
import sys

def run(inp: str) -> str:
    data = list(map(int, inp.split()))
    it = iter(data)

    n = next(it)
    m = next(it)

    # Consume the original tree.
    for _ in range(n - 1):
        next(it)

    edges = []
    for _ in range(m):
        u = next(it) - 1
        v = next(it) - 1
        edges.append((u, v))

    return str(solve_case(n, m, edges))

# Provided sample 1
assert run("""\
3 3
1 2
1 2
2 3
1 3
""") == "2", "sample 1"

# Provided sample 2
assert run("""\
4 4
1 1 1
1 2
2 3
3 4
1 4
""") == "3", "sample 2"

# Provided sample 3
assert run("""\
4 2
1 2 3
1 2
3 4
""") == "6", "sample 3"

# Minimum-size tree, repeated identical request.
# The two path XORs are always equal, so only 00 and 11 work.
assert run("""\
2 2
1
1 2
1 2
""") == "2", "minimum size and repeated path"

# Three requests forming a forest.
# Every one of 00, 01, 011? For m=3 the possible patterns are
# 000, 001, 011, 111, and all are feasible because there is no cycle.
# The request graph has two components, so each contributes a factor 2.
assert run("""\
5 3
1 2 3 4
1 2
2 3
4 5
""") == "8", "forest and disconnected components"

# Maximum-size stress case.
# All 250000 requests are the same edge, so all path XORs are equal.
# Only the all-zero and all-one sequences are possible.
n = 250000
m = 250000
parents = " ".join(["1"] * (n - 1))
queries = "\n".join(["1 2"] * m)

max_input = f"{n} {m}\n{parents}\n{queries}\n"
assert run(max_input) == "2", "maximum-size repeated-edge case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 2`, cả hai yêu cầu`(1,2)`| 2 | Kích thước tối thiểu, hạn chế trùng lặp, ranh giới cực đoan | 
|`5 3`, các cạnh yêu cầu`(1,2),(2,3),(4,5)`| 8 | Cấu trúc rừng và các thành phần rời rạc | 
|`250000 250000`, mọi yêu cầu`(1,2)`| 2 | Tối đa (n,m), mức sử dụng bộ nhớ, các ràng buộc lặp lại | 
| Mẫu 1 hình tam giác | 2 | Tính nhất quán của chu kỳ lẻ | 
| Mẫu 2 bốn chu kỳ | 3 | Luân phiên ranh giới khả thi và không khả thi | 
| Mẫu 3 hai cạnh rời nhau | 6 | Đa thành phần | 

## Vỏ cạnh 

Đối với trường hợp kích thước tối thiểu```
2 2
1
1 2
1 2
```đồ thị yêu cầu có hai cạnh song song. Một ranh giới (k=0) gán chẵn lẻ 1 cho cả hai, do đó các ràng buộc là nhất quán. Ranh giới (k=1) gán các giá trị chẵn lẻ 0 và 1 cho hai bản sao của cùng một cạnh, điều này buộc cùng một cặp đỉnh phải có cả nhãn bằng nhau và nhãn khác nhau, do đó nó không nhất quán. Ranh giới (k=2) gán tính chẵn lẻ 0 cho cả hai và nhất quán. Biểu đồ có một thành phần, vì vậy mỗi ranh giới hợp lệ đóng góp (2^{1-1}=1), cho ra 2. 

Đối với trường hợp ngắt kết nối```
4 2
1 2 3
1 2
3 4
```không có chu kỳ, do đó mọi phép gán chẵn lẻ trên hai cạnh yêu cầu đều nhất quán. Ba mẫu đơn điệu (00,01,11) đều khả thi. Biểu đồ yêu cầu có hai thành phần và việc sửa gốc ban đầu sẽ loại bỏ một trong hai lần lật thành phần, để lại (2^{2-1}=2) phép gán đỉnh cho mỗi ranh giới. Tổng số là (3\cdot2=6). 

Đối với hình tam giác```
3 3
1 2
1 2
2 3
1 3
```ranh giới (k=1) cho các cạnh chẵn lẻ (0,1,1). XOR của chúng xung quanh tam giác bằng 0, do đó DSU vẫn nhất quán. Ranh giới (k=2) cho (0,0,1), có chu kỳ XOR bằng 1, do đó ràng buộc cuối cùng tạo ra mâu thuẫn. DSU khôi phục phát hiện điều này ngay lập tức và khôi phục trạng thái trước khi khám phá ranh giới tiếp theo. 

Đối với trường hợp tất cả đều có giá trị bằng nhau với các yêu cầu lặp lại, hệ thống có thể chứa nhiều cạnh song song. Các cạnh song song không tự động vô hại: nếu hai bản sao nhận được các số chẵn lẻ được yêu cầu khác nhau, chúng sẽ tạo thành một chu kỳ có độ dài hai và mâu thuẫn nhau. DSU chẵn lẻ xử lý việc này vì cả hai bản sao đều kết nối cùng một cặp gốc và bản sao thứ hai kiểm tra xem liệu tính chẵn lẻ được yêu cầu của nó có phù hợp với tính chẵn lẻ đã được ngụ ý hay không. 

Ranh giới (k=0) yêu cầu mọi cạnh yêu cầu phải có tính chẵn lẻ là 1. Ranh giới (k=m) yêu cầu mọi cạnh yêu cầu phải có tính chẵn lẻ là 0. Công thức chia để trị bao gồm cả hai vì phạm vi tìm kiếm là ([0,m]), không phải ([1,m-1]). Tại (k=m), lá không cần chèn cạnh đặc biệt vì mọi yêu cầu đã được cố định ở mức chẵn lẻ 0 bởi đường đi qua nửa bên phải; tại lá trong (1\le k<m), cạnh biên (k) được chèn rõ ràng với chẵn lẻ 0. 

Trường hợp kích thước tối đa có thể chứa hàng trăm nghìn cạnh yêu cầu trùng lặp. Thuật toán không bao giờ xây dựng danh sách kề của cây ban đầu và không bao giờ mở rộng đường đi của cây riêng lẻ. Nó chỉ lưu trữ các điểm cuối yêu cầu, do đó việc sử dụng bộ nhớ bị chi phối bởi số đỉnh và thao tác khôi phục thay vì tổng độ dài của tất cả các đường dẫn cây được yêu cầu.
