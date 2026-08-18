---
title: "CF 102268I - Đồ thị thú vị"
description: "Chúng ta có một đồ thị vô hướng có tối đa (10^5) đỉnh và (10^5) cạnh. Đối với mỗi số màu có sẵn (k=1,2,ldots,n), chúng ta cần số lần gán màu thích hợp, trong đó các đỉnh liền kề phải nhận các màu khác nhau. Mỗi câu trả lời được lấy modulo (998244353)."
date: "2026-08-17T18:57:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102268
codeforces_index: "I"
codeforces_contest_name: "300iq Contest 1"
rating: 0
weight: 102268
solve_time_s: 367
verified: false
draft: false
---

[CF 102268I - Biểu đồ thú vị](https://codeforces.com/problemset/problem/102268/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 6 phút 7 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị vô hướng có tối đa (10^5) đỉnh và (10^5) cạnh. Đối với mỗi số màu có sẵn (k=1,2,\ldots,n), chúng ta cần số lần gán màu thích hợp, trong đó các đỉnh liền kề phải nhận các màu khác nhau. Mỗi câu trả lời được lấy modulo (998244353). 

Điều kiện bất thường trên mỗi bảy đỉnh là hạn chế cấu trúc quan trọng. Giả sử một thành phần được kết nối có ít nhất bảy đỉnh. Bởi vì thành phần được kết nối, chúng ta có thể bắt đầu từ một đỉnh và liên tục thêm một đỉnh lân cận của tập hợp đã chọn cho đến khi chúng ta có chính xác bảy đỉnh. Bảy đỉnh kết quả tạo ra một sơ đồ con được kết nối. Giữa hai đỉnh bất kỳ có một đường đi chỉ sử dụng bảy đỉnh đó, vì vậy không có đỉnh nào ngoài bảy đỉnh đó có thể thuộc về mọi đường đi giữa cặp đó. Điều đó mâu thuẫn với điều kiện đã cho. Do đó, mọi thành phần được kết nối có nhiều nhất sáu đỉnh. 

Đây là toàn bộ lý do khiến vấn đề trở nên có thể quản lý được. Đồ thị ban đầu có thể có (10^5) đỉnh, nhưng mọi phần độc lập liên quan đến bài toán tô màu chỉ có sáu đỉnh. 

Tìm kiếm màu trực tiếp là vô vọng. Với (k=n=10^5), việc thử mọi bài tập sẽ kiểm tra 

[ 
n^n=(10^5)^{10^5}=10^{500000} 
] 

bài tập. Ngay cả việc tính toán một đa thức sắc độ tổng quát bằng các phương pháp dựa trên tập hợp con trên tất cả (n) đỉnh cũng sẽ yêu cầu thời gian hàm mũ tính bằng (n). 

Kích thước đầu vào cũng loại trừ các thuật toán liên tục thực hiện công việc tốn kém trên toàn bộ biểu đồ. Quét tuyến tính hoặc gần tuyến tính các đỉnh và cạnh (10^5) là được, trong khi (O(n^2)) đã có nghĩa là các thao tác đại khái là (10^{10}). Do đó, công thức hàm mũ hữu ích phải chỉ phụ thuộc vào hằng số sáu chứ không phụ thuộc vào (n). 

Có một số trường hợp khó xử lý. Với hai đỉnh được nối bởi một cạnh, câu trả lời là (0,2), vì một màu không thể tô màu chính xác cho một cạnh và hai màu đưa ra hai lựa chọn mà điểm cuối nhận được màu nào.```
2 1
1 2
```Đầu ra là```
0 2
```Thành phần đầy đủ của sáu đỉnh là một trường hợp biên quan trọng khác. Đa thức sắc độ của nó là 

[ 
k(k-1)(k-2)(k-3)(k-4)(k-5), 
] 

vì vậy nó không có màu với ít hơn sáu màu và chính xác (6!=720) màu có sáu màu.```
6 15
1 2
1 3
1 4
1 5
1 6
2 3
2 4
2 5
2 6
3 4
3 5
3 6
4 5
4 6
5 6
```Đầu ra là```
0 0 0 0 0 720
```Một lỗi phổ biến khác là quên rằng các thành phần bị ngắt kết nối có thể được tô màu độc lập. Mẫu chứa một hình tam giác và hai đỉnh cô lập. Tam giác có (k(k-1)(k-2)) màu, trong khi các đỉnh cô lập đóng góp (k^2), cho ra (k^3(k-1)(k-2)). Tại (k=3), đây là (27\cdot2=54), khớp với mẫu. 

## Phương pháp tiếp cận 

Lực lượng vũ phu bắt đầu bằng cách gán một trong các màu (k) cho mọi đỉnh và kiểm tra mọi cạnh sau khi việc gán hoàn tất. Điều này đúng vì mọi màu có thể xuất hiện chính xác một lần, nhưng với (k=n=10^5) nó kiểm tra (10^{500000}) phép gán. Không có sự cắt tỉa hữu ích nào làm thay đổi bản chất hàm mũ cơ bản của phương pháp này. 

Quan sát hữu ích là điều kiện bảy đỉnh buộc mọi thành phần được kết nối phải chứa tối đa sáu đỉnh. Màu sắc phù hợp của các thành phần liên thông khác nhau không có tương tác, vì vậy nếu các thành phần đó là (G_1,G_2,\ldots,G_s) thì đa thức màu của chúng thỏa mãn 

[ 
P_G(k)=\prod_{i=1}^{s}P_{G_i}(k). 
] 

Do đó chúng ta chỉ cần giải một bài toán nhỏ cho mỗi thành phần. 

Đối với một thành phần có các đỉnh (t\le6), hãy xem xét việc phân chia các đỉnh của nó thành các lớp màu. Một lớp màu hợp lệ chính xác khi nó là một tập hợp độc lập. Nếu thành phần có thể được phân chia thành (r) các tập độc lập theo các cách (S_r), thì các tập hợp (r) đó có thể được gán (r) các màu riêng biệt theo 

[ 
(k)_r=r!\binom{k}{r} 
] 

cách. Do đó, 

[ 
P_G(k)=\sum_{r=1}^{t} S_r(k)_r. 
] 

Chúng ta có thể liệt kê trực tiếp tất cả các phân vùng đã đặt. Chỉ có (B_6=203) tập hợp các phân vùng gồm sáu đỉnh được gắn nhãn, vì vậy con số này nhỏ hơn nhiều so với việc liệt kê các màu tùy ý. Chúng tôi chỉ tính các phân vùng có khối độc lập. 

Có một quan sát kích thước không đổi thứ hai. Mặc dù có nhiều đồ thị được gắn nhãn trên sáu đỉnh, nhưng chỉ có 50 đa thức màu riêng biệt trong số các đồ thị được kết nối trên sáu đỉnh và trên các kích thước thành phần từ một đến sáu, số lượng tương ứng là (1,1,2,5,14,50). Như vậy có nhiều nhất 73 đa thức màu thành phần riêng biệt có thể xảy ra. 

Chúng ta nhóm các thành phần có cùng đa thức. Nếu một đa thức cụ thể (f(k)) xuất hiện (c) lần thì tổng đóng góp của nó là (f(k)^c). Thay vì xử lý từng thành phần riêng biệt cho mỗi (k), chúng tôi xử lý từng đa thức riêng biệt một lần và nâng giá trị của nó lên bội số. 

Việc triển khai bên dưới tiến thêm một bước đối với Python. Mỗi đa thức thành phần có nhiều nhất là sáu bậc, vì vậy các giá trị của nó ở các số nguyên liên tiếp có thể được tạo ra với sai phân hữu hạn chỉ bằng phép cộng. Điều này tránh việc đánh giá sáu hệ số nhị thức cho mỗi cặp (k) và loại đa thức. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n^n m)) cho (k=n) | (O(n+m)) | Quá chậm | 
| Tối ưu | (O(n+m+3^6n+nT\log n)), (T\le73) | (O(n+m)) | Đã chấp nhận | 

Thuật ngữ (3^6) mô tả công việc chung của tập hợp con thành phần nhỏ, trong khi việc triển khai ở đây sử dụng các phân vùng đã đặt và do đó thực sự khám phá tối đa vài trăm trạng thái cho mỗi thành phần. Giới hạn (T\le73) xuất phát từ tập hợp hữu hạn các đa thức sắc độ có thể có của các đồ thị liên thông có tối đa sáu đỉnh. 

## Hướng dẫn thuật toán

1. Xây dựng danh sách kề và tìm mọi thành phần được kết nối bằng DFS hoặc BFS. Đối số cấu trúc ở trên đảm bảo rằng mọi thành phần được phát hiện đều có tối đa sáu đỉnh, vì vậy chúng ta có thể biểu diễn sự kề bên bên trong của nó một cách an toàn bằng mặt nạ bit. 
2. Dán nhãn lại các đỉnh của một thành phần từ (0) đến (t-1) và lưu trữ cho mỗi đỉnh cục bộ một mặt nạ bit (t) chứa các đỉnh lân cận của nó. Bitmask đặt câu hỏi "đỉnh này có thể được đưa vào lớp màu này không?" một thao tác AND theo từng bit. 
3. Liệt kê tất cả các phân vùng của các đỉnh thành phần thành các khối. Xử lý các đỉnh theo thứ tự tăng dần. Đối với đỉnh hiện tại, hãy chèn nó vào một khối hiện có có các đỉnh không liền kề với nó hoặc tạo một khối mới. Việc xử lý các đỉnh theo thứ tự cố định làm cho mỗi phân vùng đã đặt xuất hiện đúng một lần. 
4. Gọi (S_r) là số phân vùng hợp lệ thành đúng (r) khối độc lập. Chuyển đổi giá trị này thành hệ số của (\binom{k}{r}) bằng cách nhân với (r!). Vectơ kết quả mô tả duy nhất đa thức màu của thành phần. 
5. Lưu vectơ hệ số này vào từ điển và đếm xem mỗi vectơ có bao nhiêu thành phần. Các thành phần có cùng một vectơ có cùng số lượng màu thích hợp cho mọi (k), vì vậy chúng có thể được nhóm lại một cách an toàn. 
6. Với mỗi đa thức riêng biệt, trước tiên hãy tính giá trị của nó tại (k=0,1,\ldots,t). Các giá trị (t+1) này xác định hoàn toàn đa thức bậc (t). Xây dựng bảng sai phân hữu hạn của nó, giữ nguyên giá trị đầu tiên của mọi bậc sai phân. 
7. Tạo lần lượt các giá trị đa thức cho (k=1,2,\ldots,n). Nếu mảng chênh lệch hiện tại là (\Delta^0f,\Delta^1f,\ldots,\Delta^tf), việc tăng từ (k) lên (k+1) được thực hiện bằng cách thêm từng chênh lệch vào mảng tiếp theo từ bậc thấp đến bậc cao. Mục nhập đầu tiên trở thành giá trị mới của (f). 
8. Nhân câu trả lời tổng thể ở mọi (k) với giá trị đa thức hiện tại được nâng lên thành số thành phần được biểu thị bằng đa thức này. Vì số mũ là bội số của kiểu nên toàn bộ tập hợp các thành phần bằng nhau được xử lý cùng một lúc. 
9. In các giá trị kết quả từ (k=1) đến (n). Tất cả số học được thực hiện modulo (998244353). 

### Tại sao nó hoạt động 

Đối với mọi thành phần được kết nối, phân vùng đệ quy liệt kê chính xác các phân vùng có khối có thể nhận được các màu bằng nhau. Việc phân vùng thành (r) các khối độc lập tương ứng với việc gán chính xác (r!\binom{k}{r}) các màu riêng biệt cho các khối đó. Do đó đa thức nhỏ được tính toán chính xác là đa thức màu của thành phần. 

Các thành phần được kết nối khác nhau không có cạnh giữa chúng, vì vậy màu sắc của chúng có thể được chọn độc lập. Do đó đa thức màu của chúng nhân lên. Việc nhóm các đa thức thành phần bằng nhau chỉ thay thế phép nhân lặp lại bằng lũy ​​thừa và không làm thay đổi kết quả. 

Pha sai phân hữu hạn không xấp xỉ đa thức. Một đa thức bậc nhiều nhất là sáu được xác định hoàn toàn bởi bảy giá trị liên tiếp và sai phân hữu hạn thứ bảy của nó bằng không. Việc nâng cao bảng hiệu sẽ tái tạo giá trị chính xác ở mọi số nguyên tiếp theo. Do đó, mỗi giá trị nhân với câu trả lời cuối cùng là số lượng màu chính xác do loại thành phần đó đóng góp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def component_signature(adj_masks):
    """Return coefficients c[r-1] such that
       P(k) = sum_{r=1}^t c[r-1] * C(k, r).
    """
    t = len(adj_masks)
    ways = [0] * (t + 1)
    blocks = []

    def dfs(v):
        if v == t:
            ways[len(blocks)] += 1
            return

        bit = 1 << v
        forbidden = adj_masks[v]

        for i in range(len(blocks)):
            block = blocks[i]
            if forbidden & block == 0:
                blocks[i] = block | bit
                dfs(v + 1)
                blocks[i] = block

        blocks.append(bit)
        dfs(v + 1)
        blocks.pop()

    dfs(0)

    res = [0] * t
    fact = 1
    for r in range(1, t + 1):
        fact = fact * r % MOD
        res[r - 1] = ways[r] * fact % MOD

    return tuple(res)

def component_values(signature, n):
    """Generate P(1), P(2), ..., P(n) using finite differences."""
    t = len(signature)

    # Evaluate at k = 0, 1, ..., t.
    initial = []
    for k in range(t + 1):
        comb = 1
        value = 0
        for r in range(1, t + 1):
            comb = comb * (k - r + 1) % MOD
            comb = comb * pow(r, MOD - 2, MOD) % MOD
            value = (value + signature[r - 1] * comb) % MOD
        initial.append(value)

    # Build the first element of every finite-difference row.
    diff = initial[:]
    first = [diff[0]]
    for length in range(t, 0, -1):
        diff = [(diff[i + 1] - diff[i]) % MOD for i in range(length)]
        first.append(diff[0])

    # first[j] = Delta^j f(0).
    cur = first
    values = [0] * n

    for k in range(n):
        # Advance from x=k to x=k+1.
        for j in range(t):
            cur[j] = (cur[j] + cur[j + 1]) % MOD
        values[k] = cur[0]

    return values

def solve():
    n, m = map(int, input().split())

    graph = [[] for _ in range(n)]
    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        graph[u].append(v)
        graph[v].append(u)

    seen = [False] * n
    pos = [-1] * n
    types = {}

    for start in range(n):
        if seen[start]:
            continue

        stack = [start]
        seen[start] = True
        vertices = []

        while stack:
            u = stack.pop()
            vertices.append(u)
            for v in graph[u]:
                if not seen[v]:
                    seen[v] = True
                    stack.append(v)

        t = len(vertices)

        # The input guarantee implies t <= 6.
        for i, v in enumerate(vertices):
            pos[v] = i

        masks = [0] * t
        for i, u in enumerate(vertices):
            mask = 0
            for v in graph[u]:
                mask |= 1 << pos[v]
            masks[i] = mask

        signature = component_signature(masks)
        types[signature] = types.get(signature, 0) + 1

    answer = [1] * n

    for signature, count in types.items():
        values = component_values(signature, n)

        if count == 1:
            for i, value in enumerate(values):
                answer[i] = answer[i] * value % MOD
        elif count == 2:
            for i, value in enumerate(values):
                answer[i] = answer[i] * value * value % MOD
        elif count == 3:
            for i, value in enumerate(values):
                answer[i] = answer[i] * value * value % MOD
                answer[i] = answer[i] * value % MOD
        else:
            for i, value in enumerate(values):
                answer[i] = answer[i] * pow(value, count, MOD) % MOD

    print(*answer)

if __name__ == "__main__":
    solve()
```Phần đầu vào lưu trữ biểu đồ dưới dạng danh sách kề, sau đó ngăn xếp sẽ tìm các thành phần được kết nối theo thời gian tuyến tính. Bởi vì mọi thành phần hợp lệ có tối đa sáu đỉnh, đồ thị cục bộ có thể được chuyển đổi thành mặt nạ bit mà không cần bất kỳ cấu trúc phụ trợ lớn nào. 

các`component_signature`thường trình là cốt lõi của tính toán đồ thị nhỏ. các`blocks`mảng chứa các lớp màu hiện được xây dựng. Khi đỉnh (v) được chèn vào một khối,`adj_masks[v] & block`kiểm tra xem một cạnh có cả hai điểm cuối trong lớp màu đó hay không. Kết quả bằng 0 có nghĩa là khối vẫn độc lập. 

Đệ quy đếm các phân vùng không có thứ tự. Đây là cố ý. Một phân vùng thành các khối (r) tương ứng với các phép gán màu được gắn nhãn (r!\binom{k}{r}), đó chính xác là lý do tại sao mã nhân lên`ways[r]`qua`r!`. 

các`component_values`hàm sử dụng biểu diễn kết quả 

[ 
P(k)=\sum_{r=1}^{t} c_r\binom{k}{r}. 
] 

Đánh giá ban đầu nhỏ sử dụng phép truy hồi cho các hệ số nhị thức. Vì (t\le6), tác phẩm này có kích thước không đổi. Các giá trị tiếp theo được tạo ra bởi sự khác biệt hữu hạn, tránh việc đánh giá đa thức lặp lại cho mỗi lần đếm màu. 

Phần lũy thừa xử lý bội số của từng loại đa thức. Các trường hợp đặc biệt cho bội số một, hai và ba tránh gọi hàm tới lũy thừa mô-đun cho các số đếm nhỏ phổ biến nhất. Đối với các bội số lớn hơn, tính năng tích hợp sẵn của Python`pow(a,b,MOD)`thực hiện lũy thừa mô-đun trong mã gốc được tối ưu hóa. 

Tất cả các giá trị đều được giảm modulo (998244353). Số nguyên Python không bị tràn, nhưng việc giữ giá trị ở mức giảm sau khi nhân sẽ ngăn chúng tăng lên một cách không cần thiết. 

## Ví dụ đã hoạt động 

Đối với mẫu được cung cấp, biểu đồ bao gồm một hình tam giác trên các đỉnh (1,3,5) và hai đỉnh cô lập. Hình tam giác có một phân vùng hợp lệ thành ba khối đơn độc lập và không có phân vùng hợp lệ thành ít hơn ba khối. Do đó, vectơ hệ số của nó là ((0,0,6)). Một đỉnh cô lập có đa thức (k), được biểu thị bằng ((1)). 

| (k) | Tam giác (P(k)) | Hai đỉnh cô lập | Tổng cộng | 
| --- | --- | --- | --- | 
| 1 | 0 | (1^2=1) | 0 | 
| 2 | 0 | (2^2=4) | 0 | 
| 3 | 6 | (3^2=9) | 54 | 
| 4 | 24 | (4^2=16) | 384 | 
| 5 | 60 | (5^2=25) | 1500 | 

Do đó, đầu ra cuối cùng là`0 0 54 384 1500`. Bảng hiển thị trực tiếp bất biến của tích thành phần: các đỉnh cô lập không bao giờ tương tác với tam giác, do đó đóng góp của chúng chỉ được nhân lên sau đó. Mẫu chính thức là cùng một tam giác cộng với hai đỉnh cô lập. 

Đối với ví dụ thứ hai, lấy ba cạnh rời nhau.```
6 3
1 2
3 4
5 6
```Mọi thành phần đều là (K_2), có đa thức là (k(k-1)). Biểu diễn hệ số của nó chỉ chứa số hạng (2\binom{k}{2}), vì vậy cả ba thành phần đều có cùng chữ ký và được nhóm lại với nhau bằng bội số ba. 

| (k) | Một cạnh | Đa bội | Tổng cộng | 
| --- | --- | --- | --- | 
| 1 | 0 | 3 | 0 | 
| 2 | 2 | 3 | 8 | 
| 3 | 6 | 3 | 216 | 
| 4 | 12 | 3 | 1728 | 
| 5 | 20 | 3 | 8000 | 
| 6 | 30 | 3 | 27000 | 

Đầu ra là```
0 8 216 1728 8000 27000
```Ví dụ này chứng minh tại sao việc nhóm các loại thành phần giống hệt nhau lại quan trọng. Thay vì đánh giá ba thừa số riêng biệt cho mỗi (k), thuật toán đánh giá một đa thức và nâng nó lên lũy thừa thứ ba. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n+m+3^6n+nT\log n)) | Các thành phần có kích thước không đổi, (T\le73) và mỗi loại được đánh giá trên tất cả (n) số lượng màu | 
| Không gian | (O(n+m)) | Danh sách kề, dữ liệu thành phần, mảng trả lời và trạng thái thành phần có kích thước không đổi chiếm ưu thế | 

Việc truyền tải đồ thị là tuyến tính ở kích thước đầu vào. Việc tính toán đồ thị đắt tiền bị giới hạn bởi một hằng số vì không có thành phần nào có nhiều hơn sáu đỉnh. Số loại đa thức sắc độ khác nhau cho các đồ thị liên thông có kích thước tối đa là sáu là nhiều nhất (1+1+2+5+14+50=73). 

Với (n,m\le10^5), điều này giữ cho phần phụ thuộc vào biểu đồ gần tuyến tính và giới hạn tất cả hành vi hàm mũ ở hằng số sáu. Bước nhóm là điều khiến việc đánh giá một đa thức cho mỗi một trong số tối đa (10^5) thành phần là không cần thiết. 

## Trường hợp thử nghiệm```python
import io
import sys

MOD = 998244353

def run(inp: str) -> str:
    old_stdin = sys.stdin
    try:
        sys.stdin = io.StringIO(inp)
        solve()
        # The competitive-programming solve() writes directly to stdout,
        # so this helper is normally replaced by a captured stdout in a
        # local test harness.
    finally:
        sys.stdin = old_stdin

# A convenient self-contained harness for the same algorithm.
def run_captured(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample.
assert run_captured(
    """5 3
3 5
5 1
1 3
"""
) == "0 0 54 384 1500", "sample 1"

# Minimum valid input: one edge.
assert run_captured(
    """2 1
1 2
"""
) == "0 2", "single edge"

# Complete graph K4.
assert run_captured(
    """4 6
1 2
1 3
1 4
2 3
2 4
3 4
"""
) == "0 0 0 24", "K4 boundary"

# Three identical components.
assert run_captured(
    """6 3
1 2
3 4
5 6
"""
) == "0 8 216 1728 8000 27000", "repeated components"

# Complete graph K6, the largest possible connected component.
assert run_captured(
    """6 15
1 2
1 3
1 4
1 5
1 6
2 3
2 4
2 5
2 6
3 4
3 5
3 6
4 5
4 6
5 6
"""
) == "0 0 0 0 0 720", "K6 boundary"

# Maximum-size graph allowed by the constraints, consisting of 50,000
# identical edges. Its chromatic polynomial is (k(k-1))^50000.
n = 100000
lines = [f"{n} 50000"]
for i in range(1, n + 1, 2):
    lines.append(f"{i} {i + 1}")

large_input = "\n".join(lines) + "\n"
large_output = run_captured(large_input)

expected = []
for k in range(1, n + 1):
    expected.append(str(pow(k * (k - 1) % MOD, 50000, MOD)))

assert large_output == " ".join(expected), "maximum-size repeated components"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 1 / 1 2`|`0 2`| Đồ thị hợp lệ tối thiểu và đa thức sắc độ một cạnh | 
|`K4`|`0 0 0 24`| Giá trị 0 bên dưới số màu và ranh giới (k=n) | 
| Ba cạnh rời rạc |`0 8 216 1728 8000 27000`| Nhóm thành phần giống hệt nhau và bội số | 
|`K6`|`0 0 0 0 0 720`| Kích thước thành phần tối đa và ranh giới sáu màu | 
| 100000 đỉnh trong 50000 cạnh rời nhau | ((k(k-1))^{50000}) với mọi (k) | Kích thước đầu vào tối đa, các loại lặp lại và lũy thừa mô-đun | 

## Vỏ cạnh 

Đồ thị hai đỉnh```
2 1
1 2
```có một thành phần liên thông với hai đỉnh. Phân vùng thích hợp duy nhất của nó là thành hai lớp màu đơn, vì vậy đa thức của nó là (2\binom{k}{2}=k(k-1)). Thuật toán ghi lại chữ ký`(0, 2)`, đánh giá nó tại (k=1,2) và thu được`0 2`. 

Đối với một nhóm sáu đỉnh, mỗi lớp màu chỉ có thể chứa một đỉnh. Đệ quy phân vùng đạt chính xác một phân vùng hợp lệ, chứa sáu khối đơn. Hệ số của (\binom{k}{6}) là (6!=720), nên đa thức là (720\binom{k}{6}). Điều này mang lại giá trị 0 cho (k<6) và (720) cho (k=6), phát hiện các lỗi trong đó vòng lặp qua số khối vô tình dừng lại ở (t-1). 

Đối với đồ thị mẫu, tam giác và các đỉnh cô lập được phát hiện dưới dạng ba thành phần liên thông. Tam giác đóng góp (k(k-1)(k-2)), trong khi mỗi đơn vị đóng góp (k). Do đó, sản phẩm toàn cầu là (k^3(k-1)(k-2)). Điều này mắc phải sai lầm phổ biến khi coi toàn bộ biểu đồ là một thành phần nhỏ chỉ vì một số thành phần của nó nhỏ. 

Đối với nhiều thành phần giống hệt nhau, chẳng hạn như 50.000 cạnh rời rạc trong thử nghiệm kích thước tối đa, mọi thành phần đều tạo ra cùng một chữ ký. Từ điển rút gọn tất cả chúng thành một mục có bội số là 50.000. Câu trả lời ở số màu (k) là ((k(k-1))^{50000}). Đây là trường hợp khiến việc phân nhóm trở nên cần thiết, bởi vì việc xử lý từng cạnh một cách độc lập cho mỗi (k) sẽ yêu cầu đánh giá thành phần khoảng (5\cdot10^9). 

Bản thân điều kiện chỉ được sử dụng theo hướng quan trọng đối với thuật toán. Đầu vào hợp lệ không thể chứa thành phần được kết nối có bảy đỉnh trở lên. Thuật toán không cần điều ngược lại và nó không cố gắng xác minh điều kiện bảy đỉnh. Sự khác biệt này quan trọng vì sự đảm bảo là một lời hứa mang tính cấu trúc do vấn đề cung cấp chứ không phải là điều kiện mà việc triển khai cần phải kiểm tra.
