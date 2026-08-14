---
title: "CF 102331I - Đỉnh tương tác"
description: "Chúng ta được cho một cây có tới (200.000) đỉnh. Ở đâu đó trên cây này có một đỉnh đặc biệt ẩn (u). Chúng ta biết toàn bộ cây nhưng không biết (u) và chúng ta phải khám phá nó thông qua các truy vấn tương tác. Một truy vấn chọn một đỉnh (x) và một tập hợp các đỉnh (V)."
date: "2026-08-13T03:43:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102331
codeforces_index: "I"
codeforces_contest_name: "2019 Summer Petrozavodsk Camp, Day 2: 300iq Contest 2 (XX Open Cup, Grand Prix of Kazan)"
rating: 0
weight: 102331
solve_time_s: 180
verified: true
draft: false
---

[CF 102331I - Đỉnh tương tác](https://codeforces.com/problemset/problem/102331/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một cây có tới (200.000) đỉnh. Ở đâu đó trên cây này có một đỉnh đặc biệt ẩn (u). Chúng ta biết toàn bộ cây nhưng không biết (u) và chúng ta phải khám phá nó thông qua các truy vấn tương tác. 

Một truy vấn chọn một đỉnh (x) và một tập hợp các đỉnh (V). Người tương tác trả lời liệu 

[ 
\operatorname{dist}(u,x)\leq \operatorname{dist}(u,v) 
] 

với mọi (v\in V). Nói cách khác, câu trả lời là 1 chính xác khi (x) không cách xa đỉnh ẩn hơn mọi đỉnh trong tập truy vấn. Cây và đỉnh ẩn vẫn cố định trong toàn bộ quá trình tương tác. Giao thức chính thức cho phép tối đa (4\lceil\log_2 n\rceil) truy vấn. 

Đầu vào bao gồm (n), theo sau là các cạnh cây (n-1). Không có đầu ra ngoại tuyến thông thường vì câu trả lời cho mỗi truy vấn đều đến từ trình tương tác. Khi đỉnh ẩn được xác định, chúng tôi in`! u`. 

Giới hạn kích thước của (200.000) loại trừ mọi thứ bậc hai trong kích thước cây. Thậm chí (O(n^2)) sẽ có nghĩa là khoảng (4\cdot10^{10}) hoạt động trong trường hợp xấu nhất, vượt xa giới hạn hai giây. Tinh tế hơn, thuật toán tiền xử lý (O(n\log n)) là hoàn toàn hợp lý, trong khi số lượng truy vấn tương tác có giới hạn logarit chặt chẽ hơn nhiều. Do đó, giải pháp phải kết hợp việc xử lý cây tuyến tính hoặc gần tuyến tính với một chuỗi truy vấn được cân bằng cẩn thận. 

Có một số trường hợp ranh giới dễ dàng có thể phá vỡ việc triển khai bất cẩn. Hãy xem xét cây nhỏ nhất có thể,```
2
1 2
```có đỉnh ẩn (u=2). Truy vấn có tâm ở đỉnh 1 với đỉnh 2 trong tập truy vấn phải trả lời 0, bởi vì (\operatorname{dist}(2,2)=0<1=\operatorname{dist}(2,1)). Nếu việc triển khai giả định rằng một centroid luôn có ít nhất hai thành phần lân cận thì nó có thể thất bại ở đây. 

Ngôi sao là một trường hợp hữu ích khác. Vì```
5
1 2
1 3
1 4
1 5
```nếu đỉnh ẩn là 1, truy vấn (x=1) đối với cả bốn đỉnh lân cận sẽ trả về 1. Mỗi đỉnh lân cận cách đỉnh ẩn 1, trong khi bản thân (x) ở khoảng cách 0. Một giải pháp chỉ diễn giải truy vấn là chọn một trong các thành phần con sẽ bỏ lỡ khả năng chính trọng tâm là câu trả lời. 

Tình huống ngược lại cũng có vấn đề. Trong cùng một ngôi sao, nếu (u=2), truy vấn bốn đỉnh lân cận của đỉnh 1 trả về 0, vì đỉnh 2 nằm trong số các đỉnh được truy vấn và có khoảng cách 0 đến (u), trong khi đỉnh 1 có khoảng cách 1. Một giải pháp bất cẩn có thể đảo ngược ý nghĩa của câu trả lời và tiếp tục đi vào các thành phần sai. 

Cuối cùng, một đường đi có thể có trọng tâm mà hai cạnh có cấu trúc rất khác nhau. Ví dụ,```
7
1 2
2 3
3 4
4 5
5 6
6 7
```có một vùng trung tâm cân bằng, nhưng sau khi loại bỏ một trọng tâm, hai thành phần ứng cử viên chính là đường dẫn. Thuật toán phải tính toán lại trọng tâm bên trong thành phần còn sót lại thay vì tiếp tục sử dụng root ban đầu. 

## Phương pháp tiếp cận 

Chiến lược đơn giản nhất có thể là kiểm tra từng đỉnh một. Đối với một ứng cử viên (x), truy vấn (x) đối với mọi đỉnh khác. Câu trả lời chính xác là 1 khi (x=u), bởi vì nếu (x=u), khoảng cách của nó bằng 0 và mọi đỉnh khác đều ở khoảng cách không âm, trong khi nếu (x\neq u), bao gồm (u) trong tập truy vấn sẽ tạo ra câu trả lời 0. Cách tiếp cận này đúng, nhưng nó có thể yêu cầu (n-1) truy vấn. Ở mức tối đa (n=200.000), tức là (199.999) truy vấn, trong khi giới hạn cho phép chỉ là (4\lceil\log_2 200000\rceil=72). Do đó, vấn đề không phải là công việc tính toán bên trong một truy vấn mà là lượng thông tin chúng tôi trích xuất từ ​​mỗi tương tác. 

Quan sát quan trọng là điều gì xảy ra khi (x) liền kề với mọi đỉnh trong tập truy vấn. Giả sử (c) là một đỉnh nào đó và (v) là một trong những đỉnh lân cận của nó. Loại bỏ (c) chia cây thành các thành phần, mỗi thành phần chứa mỗi cây lân cận. Nếu đỉnh ẩn (u) nằm trong thành phần chứa (v), đường đi từ (u) đến (c) bắt đầu bằng cạnh (vc). Do đó, 

[ 
\operatorname{dist}(u,v)=\operatorname{dist}(u,c)-1. 
] 

Vì vậy, một truy vấn với (x=c) và tập hợp các đỉnh lân cận (v_1,\ldots,v_k) trả về 0 chính xác khi đỉnh ẩn nằm ở một trong các thành phần tương ứng. Nếu nó nằm bên ngoài tất cả các thành phần đó, thì mỗi hàng xóm được truy vấn sẽ cách xa hơn một cạnh thông qua (c), vì vậy câu trả lời là 1. Điều này chuyển phép so sánh khoảng cách bất thường thành một bài kiểm tra thành viên trực tiếp cho một tập hợp các thành phần cây. Quan sát cốt lõi tương tự được sử dụng bởi giải pháp tiêu chuẩn. 

Câu hỏi tiếp theo là làm thế nào để chọn (c). Chúng tôi sử dụng trọng tâm của thành phần ứng cử viên hiện tại. Việc loại bỏ tâm sẽ để lại các thành phần chứa tối đa một nửa số đỉnh hiện tại. Điều này mang lại cho chúng ta một không gian tìm kiếm được thu hẹp một cách tự nhiên. 

Nếu chúng ta chỉ đơn giản tìm kiếm nhị phân các thành phần lân cận theo số lượng của chúng, thì chúng ta vẫn có thể làm không tốt khi một thành phần chứa nhiều đỉnh hơn thành phần khác. Thay vào đó, chúng tôi thực hiện tìm kiếm nhị phân có trọng số trong đó mọi thành phần đều được tính trọng số theo số đỉnh của nó. Ở mỗi lần phân chia, chúng tôi chọn tiền tố có tổng trọng lượng mang lại cho hai bên cân bằng nhất. Bộ tương tác sau đó sẽ cho chúng ta biết bên nào chứa (u). Đây là tìm kiếm có trọng số được mô tả trong hướng dẫn cuộc thi. 

Bản thân truy vấn trung tâm tốn một lần tương tác. Nếu nó trả về 1 thì centroid là câu trả lời. Ngược lại, đỉnh ẩn nằm ở một trong các thành phần. Tìm kiếm nhị phân có trọng số xác định thành phần đó. Chúng tôi đánh dấu trọng tâm là đã bị loại bỏ và lặp lại quy trình tương tự bên trong thành phần còn sót lại. Bởi vì mỗi giai đoạn đều giảm đáng kể số đỉnh có thể có nên tổng số truy vấn vẫn ở dạng logarit, trong giới hạn bắt buộc (4\lceil\log_2 n\rceil). 

Phương pháp brute-force dành các truy vấn trên từng đỉnh riêng lẻ. Thay vào đó, phương pháp tối ưu sử dụng dấu phân cách của cây để biến một truy vấn thành câu lệnh về toàn bộ thành phần được kết nối. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n)) truy vấn, (O(n^2)) nếu khoảng cách được tính toán lại một cách ngây thơ | (O(n)) | Quá nhiều truy vấn | 
| Tối ưu | (O(n\log n)) xử lý cây, truy vấn (O(\log n)) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu với toàn bộ cây làm thành phần ứng cử viên. Duy trì một`blocked`mảng. Một đỉnh bị chặn đã được sử dụng làm trọng tâm và sẽ không bao giờ thuộc về thành phần ứng cử viên trong tương lai. 
2. Tìm trọng tâm (c) của thành phần hiện tại đã được bỏ chặn. Chúng tôi thực hiện điều này bằng cách duyệt cây lặp, vì một đường dẫn có (200.000) đỉnh sẽ tràn ngăn xếp đệ quy của Python nếu sử dụng DFS đệ quy. 
3. Xét mọi hàng xóm không bị chặn (v) của (c). Việc loại bỏ (c) sẽ tạo ra một thành phần ứng viên cho mỗi thành phần lân cận như vậy. Lưu trữ kích thước của từng thành phần cùng với hàng xóm của nó. Vì (c) là trọng tâm nên mọi thành phần như vậy chứa nhiều nhất một nửa số đỉnh ứng viên hiện tại. 
4. Truy vấn (c) đối với tất cả các đỉnh lân cận này. Nếu câu trả lời là 1, (u=c). Thật vậy, nếu (u=c), thì (\operatorname{dist}(u,c)=0), vậy điều kiện là đúng. Nếu (u) nằm trong bất kỳ thành phần nào của lân cận (v), thì (v) là một cạnh gần (u) hơn (c), làm cho điều kiện sai. Truy vấn duy nhất này phát hiện chính xác trọng tâm. 
5. Nếu câu trả lời là 0, hãy sắp xếp các thành phần ứng viên theo kích cỡ của chúng. Duy trì một khoảng thời gian của các chỉ số thành phần. Chọn tiền tố giảm thiểu kích thước tổng lớn hơn và kích thước hậu tố còn lại. Truy vấn trọng tâm dựa vào các đỉnh lân cận thuộc tiền tố đó. 
6. Nếu câu trả lời là 0, đỉnh ẩn thuộc về tiền tố đã chọn, vì vậy hãy loại bỏ hậu tố đó. Nếu câu trả lời là 1 thì đỉnh ẩn thuộc về hậu tố, vì vậy hãy loại bỏ tiền tố đó. Lặp lại cho đến khi chỉ còn lại một thành phần. 
7. Đánh dấu trọng tâm là bị chặn và biến thành phần duy nhất còn sót lại thành thành phần ứng cử viên mới. Vì cây được kết nối nên thành phần này có thể được biểu diễn đơn giản bằng hàng xóm còn sống sót và kích thước đã biết của nó. 
8. Khi thành phần ứng viên có một đỉnh thì đỉnh đó nhất thiết phải là đỉnh ẩn. In nó và xả nước. 

Bất biến trung tâm là sau mỗi truy vấn, đỉnh ẩn vẫn nằm trong thành phần ứng cử viên hiện tại. Truy vấn centroid bảo toàn bất biến này bằng cách kết thúc tại centroid hoặc chứng minh rằng đỉnh ẩn nằm ở một trong các thành phần. Sau đó, mọi truy vấn nhị phân có trọng số sẽ chọn chính xác một cạnh phải chứa đỉnh ẩn. Khi trọng tâm bị loại bỏ, phía còn lại là thành phần được kết nối thực sự của cây ứng cử viên ban đầu, do đó, lý do tương tự được áp dụng đệ quy. Việc phân chia có trọng số tiếp tục thu hẹp tập ứng cử viên đủ nhanh cho giới hạn truy vấn logarit. Giải pháp chính thức tuân theo cấu trúc tìm kiếm nhị phân trọng tâm cộng với trọng số này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())

    g = [[] for _ in range(n + 1)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        g[u].append(v)
        g[v].append(u)

    blocked = [False] * (n + 1)
    parent = [0] * (n + 1)
    size = [0] * (n + 1)

    def ask(x, vertices):
        print("?", len(vertices), x, *vertices, flush=True)
        return int(input())

    def find_centroid(start):
        order = [start]
        parent[start] = 0

        for v in order:
            pv = parent[v]
            for to in g[v]:
                if blocked[to] or to == pv:
                    continue
                parent[to] = v
                order.append(to)

        total = len(order)

        for v in order:
            size[v] = 1

        for v in reversed(order):
            p = parent[v]
            if p:
                size[p] += size[v]

        centroid = start
        for v in order:
            largest = total - size[v]

            for to in g[v]:
                if blocked[to]:
                    continue
                if parent[to] == v:
                    if size[to] > largest:
                        largest = size[to]

            if largest * 2 <= total:
                centroid = v
                break

        return centroid, total

    current = 1
    current_size = n

    while current_size > 1:
        centroid, total = find_centroid(current)

        parts = []

        for to in g[centroid]:
            if blocked[to]:
                continue

            if parent[to] == centroid:
                part_size = size[to]
            else:
                part_size = total - size[centroid]

            parts.append((part_size, to))

        parts.sort()

        # If the centroid itself is the hidden vertex, the answer is 1.
        all_neighbors = [v for _, v in parts]
        if ask(centroid, all_neighbors):
            print("!", centroid, flush=True)
            return

        left = 0
        right = len(parts) - 1

        while left < right:
            total_weight = 0
            for i in range(left, right + 1):
                total_weight += parts[i][0]

            best_mid = left
            best_balance = 10**30
            prefix = 0

            for i in range(left, right + 1):
                prefix += parts[i][0]
                balance = max(prefix, total_weight - prefix)

                if balance < best_balance:
                    best_balance = balance
                    best_mid = i

            chosen = [parts[i][1] for i in range(left, best_mid + 1)]

            if ask(centroid, chosen):
                left = best_mid + 1
            else:
                right = best_mid

        next_vertex = parts[left][1]
        next_size = parts[left][0]

        blocked[centroid] = True
        current = next_vertex
        current_size = next_size

    print("!", current, flush=True)

if __name__ == "__main__":
    solve()
```Giai đoạn đầu vào xây dựng cây vô hướng chính xác như bình thường. Không có nhiều trường hợp thử nghiệm vì vấn đề tương tác này mô tả một cây cho mỗi lệnh gọi. Giao thức chính thức cũng bắt đầu bằng một cạnh (n) và (n-1) của nó. 

các`ask`chức năng là cố ý nhỏ. Nó in chính xác một dòng truy vấn, xóa một lần và đọc ngay câu trả lời của người tương tác. Câu lệnh yêu cầu xóa rõ ràng sau mỗi truy vấn trong Python.`find_centroid`sử dụng một quá trình duyệt lặp. Lần đầu tiên thu thập tất cả các đỉnh của thành phần hiện tại được bỏ chặn và ghi lại cha mẹ của chúng. Đường chuyền ngược tính toán kích thước cây con. Đối với một đỉnh (v), thành phần lớn nhất được tạo bằng cách loại bỏ nó hoặc là phần ở trên (v), với kích thước`total - size[v]`, hoặc một trong các cây con của nó. Đỉnh đầu tiên mà mọi thành phần như vậy có kích thước tối đa bằng một nửa là tâm. 

Khi trọng tâm không phải là gốc của quá trình truyền tải tạm thời, một trong những hàng xóm của nó là cha mẹ của nó. Đối với hàng xóm đó, thành phần của nó có kích thước`total - size[centroid]`. Đối với mỗi hàng xóm con, kích thước thành phần chỉ đơn giản là`size[to]`. Đây là lý do tại sao mã cần thông tin gốc từ tìm kiếm centroid. 

các`parts`danh sách cửa hàng`(component_size, neighbor)`ghép nối và sắp xếp chúng theo kích thước thành phần. Việc sắp xếp là không cần thiết để đảm bảo tính chính xác của một truy vấn riêng lẻ, nhưng nó là một phần của chiến lược tìm kiếm nhị phân có trọng số giúp đưa ra giới hạn truy vấn được yêu cầu. Các đỉnh truy vấn là các đỉnh lân cận chứ không phải các đỉnh tùy ý nằm sâu bên trong các thành phần của chúng. Đó là tài sản khoảng cách quan trọng. 

Vòng lặp`while left < right`là tìm kiếm nhị phân có trọng số. Ranh giới của nó là các chỉ số thành`parts`, không phải số đỉnh. Sau mỗi câu trả lời, chính xác một bên của khoảng thời gian sẽ bị loại bỏ. Hàng xóm cuối cùng còn lại xác định thành phần duy nhất có thể chứa (u). 

Số nguyên Python có độ chính xác tùy ý, do đó không có vấn đề tràn số nguyên trong số học kích thước thành phần. Việc triển khai cũng tránh được DFS đệ quy, đây là một yêu cầu thực tế đối với một cây có hình dạng như một chuỗi có (200.000) đỉnh. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Cây là một ngôi sao có tâm ở đỉnh 1 và người tương tác trả lời 1 cho truy vấn đầu tiên. 

| Bước | Quy mô ứng viên | Trung tâm | Truy vấn hàng xóm | Trả lời | Tiểu bang mới | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 5 | 1 | 2, 3, 4, 5 | 1 | câu trả lời = 1 | 

Câu trả lời ngay lập tức là đỉnh 1. Điều này chứng tỏ tại sao bản thân trọng tâm phải được kiểm tra trước khi cố gắng chọn một trong các thành phần của nó. Mẫu chính thức sử dụng chính xác tương tác một truy vấn này. 

### Mẫu 2 

Cây giống nhau nhưng đỉnh ẩn là 2. Mẫu chính thức cung cấp bốn câu trả lời bằng 0, mặc dù các giải pháp tương tác được phép hỏi các truy vấn hợp lệ khác nhau. Sự phân chia có trọng số của chúng tôi có thể sử dụng một chuỗi khác trong khi vẫn đạt đến đỉnh 2. 

| Bước | Quy mô ứng viên | Trung tâm | Các đỉnh được truy vấn | Trả lời | Tiểu bang mới | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 5 | 1 | 2, 3, 4, 5 | 0 | (u) nằm trong thành phần một lá | 
| 2 | 4 | 1 | 2, 3 | 0 | (u) nằm ở thành phần 2 | 
| 3 | 1 | 1 | 2 | 0 | thành phần 2 sống sót | 

Thành phần cuối cùng còn sót lại là singleton chứa đỉnh 2, do đó chương trình sẽ in`! 2`. Bản ghi của mẫu chứa một chuỗi các phân vùng hợp lệ khác nhau, điều này là bình thường đối với một vấn đề tương tác vì thứ tự truy vấn chính xác không được quy định duy nhất. Mẫu chính thức xác nhận câu trả lời cuối cùng là 2. 

Bất biến quan trọng trong cả hai ví dụ là câu trả lời bằng 0 cho truy vấn đối với các lân cận của tâm có nghĩa là đỉnh ẩn nằm ở một trong các thành phần của lân cận đó. Thuật toán không bao giờ phải tính khoảng cách đến đỉnh ẩn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n\log n)) | Mỗi giai đoạn trung tâm quét thành phần hiện tại của nó và mỗi đỉnh chỉ thuộc về cấp độ trung tâm (O(\log n)) | 
| Không gian | (O(n)) | Mỗi danh sách kề và mảng phụ đều sử dụng không gian tuyến tính | 
| Truy vấn | (O(\log n)) | Kiểm tra Centroid và truy vấn tìm kiếm nhị phân có trọng số thu nhỏ không gian ứng viên về mặt hình học | 

Kích thước cây tối đa là (200.000), do đó, mảng có kích thước tuyến tính và quá trình xử lý trước (O(n\log n)) vừa vặn thoải mái trong giới hạn bộ nhớ 512 MiB. Phần tương tác là yêu cầu khắt khe hơn. Sự cố cho phép truy vấn (4\lceil\log_2 n\rceil), tối đa chỉ có 72 truy vấn (n). Cấu trúc tìm kiếm trọng tâm và tìm kiếm có trọng số được thiết kế đặc biệt để nằm trong giới hạn logarit đó. 

## Trường hợp thử nghiệm 

Một người bình thường`run(input) == output`dây nịt không thể kiểm tra vấn đề này một cách trung thực vì đầu ra phụ thuộc vào phản hồi do bộ tương tác tạo ra. Thay vào đó, kiểm tra cục bộ hữu ích là một trình mô phỏng đánh giá xác định: nó chọn một đỉnh ẩn, tính toán câu trả lời của mỗi truy vấn từ khoảng cách cây thực tế và kiểm tra xem thuật toán cuối cùng có xác định được đỉnh đó hay không. 

Khai thác sau đây phản ánh thuật toán ngoại tuyến. Nó cũng ghi lại số lượng truy vấn, do đó giới hạn tương tác logarit có thể được kiểm tra trực tiếp.```python
# Offline simulator for the interactive algorithm.
# It does not replace the interactive submission above.

def simulate(n, edges, hidden):
    g = [[] for _ in range(n + 1)]
    for u, v in edges:
        g[u].append(v)
        g[v].append(u)

    blocked = [False] * (n + 1)
    parent = [0] * (n + 1)
    size = [0] * (n + 1)
    queries = 0

    def distances_from_hidden():
        dist = [-1] * (n + 1)
        dist[hidden] = 0
        q = [hidden]

        for v in q:
            for to in g[v]:
                if dist[to] == -1:
                    dist[to] = dist[v] + 1
                    q.append(to)

        return dist

    dist = distances_from_hidden()

    def ask(x, vertices):
        nonlocal queries
        queries += 1
        return all(dist[v] >= dist[x] for v in vertices)

    def find_centroid(start):
        order = [start]
        parent[start] = 0

        for v in order:
            pv = parent[v]
            for to in g[v]:
                if blocked[to] or to == pv:
                    continue
                parent[to] = v
                order.append(to)

        total = len(order)

        for v in order:
            size[v] = 1

        for v in reversed(order):
            p = parent[v]
            if p:
                size[p] += size[v]

        for v in order:
            largest = total - size[v]

            for to in g[v]:
                if blocked[to]:
                    continue
                if parent[to] == v:
                    largest = max(largest, size[to])

            if largest * 2 <= total:
                return v, total

        raise AssertionError("centroid not found")

    current = 1
    current_size = n

    while current_size > 1:
        centroid, total = find_centroid(current)

        parts = []

        for to in g[centroid]:
            if blocked[to]:
                continue

            if parent[to] == centroid:
                part_size = size[to]
            else:
                part_size = total - size[centroid]

            parts.append((part_size, to))

        parts.sort()

        if ask(centroid, [v for _, v in parts]):
            answer = centroid
            assert answer == hidden
            assert queries <= 4 * ((n - 1).bit_length())
            return answer, queries

        left = 0
        right = len(parts) - 1

        while left < right:
            total_weight = sum(parts[i][0] for i in range(left, right + 1))

            best_mid = left
            best_balance = 10**30
            prefix = 0

            for i in range(left, right + 1):
                prefix += parts[i][0]
                balance = max(prefix, total_weight - prefix)

                if balance < best_balance:
                    best_balance = balance
                    best_mid = i

            chosen = [parts[i][1] for i in range(left, best_mid + 1)]

            if ask(centroid, chosen):
                left = best_mid + 1
            else:
                right = best_mid

        current = parts[left][1]
        current_size = parts[left][0]
        blocked[centroid] = True

    assert current == hidden
    assert queries <= 4 * ((n - 1).bit_length())
    return current, queries

# Sample 1
edges = [(1, 2), (1, 3), (1, 4), (1, 5)]
assert simulate(5, edges, 1)[0] == 1

# Sample 2
assert simulate(5, edges, 2)[0] == 2

# Minimum-size tree, hidden at the endpoint.
edges = [(1, 2)]
assert simulate(2, edges, 2)[0] == 2

# Equal-size components around a centroid.
edges = [
    (1, 2), (1, 3), (1, 4),
    (1, 5), (1, 6), (1, 7)
]
assert simulate(7, edges, 6)[0] == 6

# Long path, hidden at the boundary.
edges = [(i, i + 1) for i in range(1, 15)]
assert simulate(15, edges, 15)[0] == 15

# Maximum-size star, all components initially have equal size.
n = 200000
edges = [(1, i) for i in range(2, n + 1)]
answer, queries = simulate(n, edges, n)
assert answer == n
assert queries <= 4 * ((n - 1).bit_length())
```Kiểm tra kích thước tối đa có chủ ý là một ngôi sao vì nó nhấn mạnh số lượng lớn các thành phần có kích thước bằng nhau và kiểm tra xem tìm kiếm nhị phân có trọng số không vô tình trở thành tuyến tính ở mức độ nào đó. Trình mô phỏng cũng kiểm tra ngân sách truy vấn chính thức, điều này hữu ích hơn cho vấn đề tương tác này so với việc so sánh một chuỗi đầu ra cố định. 

| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 sao ẩn đỉnh 1 | 1 | Bản thân Centroid chính là câu trả lời | 
| Mẫu 2 sao ẩn đỉnh 2 | 2 | Lựa chọn thành phần sau truy vấn trọng tâm bằng 0 | 
| (2)-cây đỉnh, đỉnh ẩn 2 | 2 | Trường hợp ranh giới kích thước tối thiểu | 
| Sao bảy đỉnh, đỉnh ẩn 6 | 6 | Kích thước thành phần bằng nhau và phân chia có trọng số | 
| Đường đi mười lăm đỉnh, đỉnh ẩn 15 | 15 | Mục tiêu ranh giới và root gốc không cân bằng sâu sắc | 
| (200.000)-đỉnh sao, đỉnh ẩn (200.000) | (200.000) | Kích thước tối đa và giới hạn số lượng truy vấn | 

## Vỏ cạnh 

Đối với cây hai đỉnh```
2
1 2
```trọng tâm duy nhất có thể là đỉnh. Giả sử thuật toán bắt đầu ở đỉnh 1. Hàng xóm duy nhất được bỏ chặn của nó là 2, do đó truy vấn đầu tiên tương đương với việc hỏi liệu đỉnh 2 ít nhất có gần đỉnh ẩn như đỉnh 1 hay không. Nếu (u=2), câu trả lời là 0 và thành phần còn sót lại là singleton`{2}`. Lần lặp tiếp theo bị bỏ qua vì kích thước của nó đã là một và chương trình sẽ in`! 2`. Không có giả định nào về việc có nhiều con. 

Đối với ngôi sao```
5
1 2
1 3
1 4
1 5
```với (u=1), đỉnh 1 là tâm và truy vấn sử dụng các lân cận 2, 3, 4 và 5. Khoảng cách từ (u) đến (u) đều bằng 1, trong khi khoảng cách từ (u) đến tâm là 0 nên đáp án là 1. Thuật toán in ngay`! 1`. Đây chính xác là tình huống được thể hiện bằng mẫu chính thức đầu tiên. 

Đối với cùng một ngôi sao với (u=2), truy vấn centroid trả về 0 vì đỉnh được truy vấn 2 có khoảng cách 0 từ đỉnh ẩn trong khi centroid có khoảng cách 1. Sau đó, thuật toán xử lý bốn thành phần lá như bốn ứng cử viên có trọng số, mỗi ứng cử viên có kích thước 1. Phép chia cân bằng có thể hỏi về hai lá, nhận được 0 vì lá 2 nằm trong số đó và sau đó giảm khoảng cách cho các ứng cử viên liên quan còn lại. Cuối cùng chỉ còn lại thành phần 2 nên câu trả lời là`! 2`. Mẫu chính thức sử dụng bốn truy vấn để có được cùng một câu trả lời. 

Đối với một con đường dài như```
7
1 2
2 3
3 4
4 5
5 6
6 7
```tâm của toàn cây là đỉnh 4. Nếu đỉnh ẩn là 7, thì truy vấn centroid đầu tiên trả về 0. Chỉ thành phần chứa 5 mới có thể chứa mục tiêu và kích thước của nó là 3. Khi đó, đỉnh 4 bị chặn và thuật toán tìm ra tâm của đường còn lại 5-6-7, tức là 6. So sánh khoảng cách tương tự sẽ xác định thành phần chứa 7. Việc tìm kiếm tiếp tục cho đến khi 7 bị cô lập. Điểm thiết yếu là trọng tâm được tính toán lại sau mỗi lần giảm, thay vì coi gốc của cây ban đầu là vĩnh viễn. 

Trường hợp mức độ tối đa là một ngôi sao lớn. Mọi thành phần sau khi loại bỏ phần giữa đều có kích thước là 1, do đó việc sắp xếp sẽ giữ nguyên tất cả các trọng số bằng nhau. Việc tìm kiếm có trọng số sau đó hoạt động giống như tìm kiếm nhị phân thông thường trên các lá, chỉ yêu cầu nhiều tương tác logarit. Đây chính xác là loại cây hiển thị một triển khai vô tình quét từng lá một. 

Bẫy triển khai cuối cùng là DFS đệ quy. Một đường dẫn có (200.000) đỉnh có thể tạo độ sâu đệ quy gần (200.000), điều này không an toàn trong Python ngay cả khi bản thân thuật toán là chính xác. Việc triển khai đã gửi sử dụng các danh sách rõ ràng làm ngăn xếp truyền tải, do đó độ sâu của cây đầu vào không ảnh hưởng đến ngăn xếp lệnh gọi của Python.
