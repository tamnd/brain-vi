---
title: "CF 102361F - Chương trình Rừng"
description: "Chúng ta có một đồ thị vô hướng có các thành phần liên thông đều là xương rồng. Chúng ta có thể chọn bất kỳ tập hợp cạnh nào để loại bỏ. Sau khi loại bỏ, mọi thành phần được kết nối phải là một cây, tương đương với việc nói rằng biểu đồ còn lại không được chứa chu trình."
date: "2026-08-13T00:12:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102361
codeforces_index: "F"
codeforces_contest_name: "2019 China Collegiate Programming Contest Qinhuangdao Onsite"
rating: 0
weight: 102361
solve_time_s: 84
verified: true
draft: false
---

[CF 102361F - Chương trình Rừng](https://codeforces.com/problemset/problem/102361/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 24s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị vô hướng có các thành phần liên thông đều là xương rồng. Chúng ta có thể chọn bất kỳ tập hợp cạnh nào để loại bỏ. Sau khi loại bỏ, mọi thành phần được kết nối phải là một cây, tương đương với việc nói rằng biểu đồ còn lại không được chứa chu trình. 

Cách hữu ích để quan sát một cây xương rồng là chia các cạnh của nó thành hai loại. Một số cạnh thuộc về một chu trình và mỗi cạnh như vậy thuộc về đúng một chu trình. Các cạnh còn lại không phải là một phần của bất kỳ chu trình nào. Vì các chu kỳ ở cây xương rồng không có chung cạnh nên các quyết định đưa ra cho các chu kỳ khác nhau là độc lập. 

Giả sử một chu trình chứa (k) cạnh. Giữ tất cả các cạnh (k) sẽ giữ nguyên chu trình đó, do đó lựa chọn duy nhất đó bị cấm. Mọi tập con khác của các cạnh của nó đều hợp lệ, cho 

[ 
2^k-1 
] 

sự lựa chọn. Một cạnh bên ngoài mỗi chu kỳ có thể bị loại bỏ hoặc giữ lại, đưa ra hai lựa chọn. 

Nếu độ dài chu trình là (c_1,c_2,\ldots,c_t) và tổng số cạnh của chúng là 

[ 
C=c_1+c_2+\cdots+c_t, 
] 

thì câu trả lời là 

[ 
2^{m-C}\prod_{i=1}^{t}(2^{c_i}-1). 
] 

Nhiệm vụ còn lại là tìm mọi chu kỳ và độ dài của nó một cách hiệu quả. 

Sự cố chính thức có (n\le300000), (m\le500000), với giới hạn thời gian 1 giây và giới hạn bộ nhớ 1024 MB. Một thuật toán quét liên tục toàn bộ đồ thị để tìm các tập hợp con cạnh theo cấp số nhân là hoàn toàn không khả thi. Về cơ bản, chúng ta cần công việc tuyến tính trong kích thước biểu đồ, có lẽ với các yếu tố logarit để lũy thừa mô-đun. 

Có một số trường hợp khó khăn dễ gây ra giải pháp sai. 

Đối với đồ thị vô cạnh,```
1 0
```câu trả lời là`1`. Có chính xác một tập hợp có thể loại bỏ được, tập hợp trống. Một giải pháp giả định mọi thành phần đều chứa một chu trình có thể vô tình trả về 0. 

Đối với cây có một cạnh,```
2 1
1 2
```câu trả lời là`2`. Chúng tôi có thể giữ lại cạnh hoặc loại bỏ nó. Cả hai đồ thị kết quả đều là rừng. Một giải pháp chỉ tính các cách phá vỡ chu kỳ có thể quay trở lại`1`, quên rằng các cạnh không có chu trình cũng có thể tháo rời độc lập. 

Các thành phần bị ngắt kết nối phải được xử lý riêng. Vì```
6 6
1 2
2 3
3 1
4 5
5 6
6 4
```có hai tam giác độc lập nên đáp án là 

[ 
(2^3-1)^2=7^2=49. 
] 

Một DFS chỉ bắt đầu từ đỉnh 1 sẽ hoàn toàn bỏ lỡ thành phần thứ hai và trả về`7`. 

Cuối cùng, hãy xem xét một chu trình có gắn một cây cầu:```
4 4
1 2
2 3
3 1
3 4
```Tam giác góp phần`7`sự lựa chọn và cây cầu góp phần`2`, vậy câu trả lời là`14`. Xử lý mọi cạnh như thể nó thuộc về chu trình hoặc bỏ qua hoàn toàn các cạnh không thuộc chu trình sẽ cho kết quả sai. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là liệt kê mọi tập hợp con của các cạnh (m). Đối với mỗi tập hợp con, giữ nguyên các cạnh đã chọn và kiểm tra xem đồ thị thu được có phải là không theo chu kỳ hay không. Kiểm tra dựa trên DFS hoặc DSU tiêu chuẩn có thể xác định liệu một chu trình có tồn tại trong thời gian (O(n+m)) hay không. Vì có (2^m) tập hợp con nên việc này cần 

[ 
O(2^m(n+m)) 
] 

thời gian. Ở mức tối đa (m=500000), điều này có nghĩa là kiểm tra khoảng (2^{500000}), khoảng (10^{150515}), các tập hợp con khác nhau. Phương pháp này đúng vì nó kiểm tra rõ ràng mọi sơ đồ loại bỏ có thể, nhưng không gian tìm kiếm vượt xa mọi giới hạn thực tế. 

Cấu trúc xương rồng cho chúng ta khả năng quan sát mạnh mẽ hơn rất nhiều. Đồ thị còn lại không thể trở thành một khu rừng chính xác khi một số chu kỳ ban đầu tồn tại hoàn toàn. Do đó, mọi chu trình ban đầu chỉ áp đặt một điều kiện: ít nhất một trong các cạnh của nó phải bị loại bỏ. Đối với một chu trình có độ dài (k), tất cả (2^k) các tập con của các cạnh của nó đều có thể ngoại trừ một tập con không loại bỏ gì, cho ra (2^k-1). 

Các cạnh bên ngoài chu kỳ không áp đặt hạn chế nào cả. Việc loại bỏ một cạnh như vậy không thể tạo ra một chu trình và việc giữ nó không thể tạo ra một chu trình vì cạnh đó không phải là một phần của một chu trình trong biểu đồ ban đầu. Do đó, mỗi cạnh như vậy đều đóng góp một yếu tố`2`. 

Vấn đề đồ thị duy nhất còn lại là xác định tất cả độ dài chu kỳ. DFS cung cấp chính xác những gì chúng ta cần. Trong cây DFS vô hướng, mọi cạnh không phải là cây đều kết nối một đỉnh với một trong các đỉnh tổ tiên của nó. Nếu đỉnh hiện tại có độ sâu (d_u) và đỉnh tổ tiên có độ sâu (d_v), thì đường đi giữa chúng chứa các cạnh (d_u-d_v) và cạnh không phải cây sẽ đóng một chu kỳ có độ dài 

[ 
d_u-d_v+1. 
] 

Tình trạng xương rồng khiến nơi này trở nên đặc biệt sạch sẽ. Mỗi cạnh không phải cây tương ứng với một chu trình đơn giản riêng biệt, do đó việc đếm các cạnh sau này sẽ cho mỗi chu trình chính xác một lần. 

Phương pháp brute-force hoạt động vì nó kiểm tra rõ ràng mọi tập hợp con có thể, nhưng không thành công vì số lượng tập hợp con là theo cấp số nhân. Việc quan sát cây xương rồng làm giảm toàn bộ vấn đề trong việc phát hiện độ dài chu kỳ với một DFS và nhân các đóng góp độc lập. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(2^m(n+m))) | (O(n+m)) | Quá chậm | 
| DFS trên cây xương rồng | (O(n+m)) | (O(n+m)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng đồ thị vô hướng trong khi gán ID cạnh cho mọi cạnh đầu vào. Việc triển khai lưu trữ biểu đồ với các mảng kề để có thể phân biệt được cạnh vô hướng với cạnh ngược của nó ngay cả khi cả hai điểm cuối đã được truy cập. 
2. Bắt đầu DFS từ mọi đỉnh có độ sâu chưa được chỉ định. Đồ thị không nhất thiết phải được kết nối, vì vậy một DFS từ đỉnh`1`là không đủ. 
3. Gán độ sâu đỉnh bắt đầu`0`. Bất cứ khi nào DFS phát hiện ra một đỉnh chưa được thăm`v`từ`u`, giao phó`depth[v] = depth[u] + 1`và nhớ cạnh được sử dụng để đạt được`v`là cạnh cha của nó. 
4. Khi kiểm tra một người hàng xóm đã đến thăm`v`của`u`, bỏ qua đảo ngược của cạnh cha. Nếu như`depth[v] < depth[u]`, sau đó`v`là tổ tiên của`u`, do đó cạnh này là cạnh không phải cây đóng một chu trình. 
5. Tính độ dài chu kỳ là`depth[u] - depth[v] + 1`. Con đường cây từ`v`ĐẾN`u`có`depth[u] - depth[v]`các cạnh và cạnh hiện tại sẽ thêm một cạnh nữa. 
6. Nhân câu trả lời với (2^{k}-1), trong đó (k) là độ dài chu trình được phát hiện. Đồng thời thêm (k) vào`cycle_edges`, số cạnh thuộc về chu trình. 
7. Sau khi DFS kết thúc, chính xác`m - cycle_edges`các cạnh nằm ngoài mọi chu trình. Mỗi trong số này có thể được giữ hoặc loại bỏ một cách độc lập, vì vậy hãy nhân câu trả lời với 

[ 
2^{m-\text{cycle_edges}}. 
] 

1. Tính tất cả các lũy thừa của hai đến`n`modulo`998244353`trước DFS. Một chu trình không thể chứa nhiều hơn`n`các cạnh, do đó mọi thừa số (2^k) có thể thu được trong thời gian không đổi. 

### Tại sao nó hoạt động 

Hãy xem xét bất kỳ sơ đồ loại bỏ nào do thuật toán tạo ra. Đối với mỗi chu kỳ ban đầu, hệ số (2^k-1) chọn một tập hợp con các cạnh của nó chứ không phải tập hợp con trống đã bị loại bỏ, do đó ít nhất một cạnh của chu trình đó bị loại bỏ. Vì mọi chu trình ban đầu đều bị hỏng nên không có chu trình ban đầu nào còn nguyên vẹn. 

Ngược lại, giả sử một sơ đồ loại bỏ để lại một chu trình. Mọi cạnh còn lại đều có trong đồ thị ban đầu nên chu trình đó cũng là chu trình của cây xương rồng ban đầu. Khi đó, sơ đồ sẽ không loại bỏ cạnh nào khỏi chu trình đó, đây chính xác là một lựa chọn bị cấm bị loại trừ bởi hệ số (2^k-1) của nó. Do đó mọi sơ đồ được tính theo công thức đều để lại một khu rừng. 

DFS đếm mỗi chu kỳ xương rồng một lần. Mỗi cạnh không phải là cây trong DFS vô hướng kết nối một đỉnh với một đỉnh tổ tiên và đường đi tương ứng của cây cộng với cạnh đó tạo thành một chu trình. Bởi vì mỗi cạnh của một cây xương rồng thuộc về nhiều nhất một chu trình đơn giản, nên hai chu trình được phát hiện như vậy không thể biểu diễn cùng một chu trình cạnh. Do đó, độ dài chu trình được thu thập chiếm chính xác tất cả các cạnh của chu trình và mọi cạnh còn lại thực sự nằm ngoài mọi chu trình. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def solve():
    n, m = map(int, input().split())

    head = [-1] * n
    to = [0] * (2 * m)
    nxt = [0] * (2 * m)

    edge_ptr = 0

    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1

        to[edge_ptr] = v
        nxt[edge_ptr] = head[u]
        head[u] = edge_ptr
        edge_ptr += 1

        to[edge_ptr] = u
        nxt[edge_ptr] = head[v]
        head[v] = edge_ptr
        edge_ptr += 1

    pow2 = [1] * (n + 1)
    for i in range(1, n + 1):
        pow2[i] = (pow2[i - 1] * 2) % MOD

    depth = [-1] * n
    parent_edge = [-1] * n
    current_edge = [-1] * n

    answer = 1
    cycle_edges = 0

    for root in range(n):
        if depth[root] != -1:
            continue

        depth[root] = 0
        parent_edge[root] = -1
        current_edge[root] = head[root]

        stack = [root]

        while stack:
            u = stack[-1]
            e = current_edge[u]

            if e == -1:
                stack.pop()
                continue

            current_edge[u] = nxt[e]

            if parent_edge[u] != -1 and e == (parent_edge[u] ^ 1):
                continue

            v = to[e]

            if depth[v] == -1:
                depth[v] = depth[u] + 1
                parent_edge[v] = e
                current_edge[v] = head[v]
                stack.append(v)
            elif depth[v] < depth[u]:
                length = depth[u] - depth[v] + 1
                cycle_edges += length
                answer = answer * (pow2[length] - 1) % MOD

    answer = answer * pow2[m - cycle_edges] % MOD
    print(answer)

if __name__ == "__main__":
    solve()
```Cấu trúc liền kề sử dụng`head`,`to`, Và`nxt`thay vì danh sách các bộ dữ liệu Python cho mọi cạnh. Mỗi cạnh đầu vào tạo ra hai mục kề nhau có hướng và hai mục có chỉ số liên tiếp. Đó là lý do tại sao có được sự đảo ngược của cạnh cha bằng`parent_edge[u] ^ 1`. 

DFS có tính lặp lại chứ không phải đệ quy. Với tối đa`300000`các đỉnh, độ sâu đệ quy mặc định của Python không đủ cho một cây xương rồng có hình đường dẫn và việc tăng giới hạn đệ quy vẫn khiến đệ quy phải trả phí. Sự rõ ràng`stack`tránh được cả hai vấn đề.`depth[v] == -1`là tình trạng chưa được thăm khám. Khi một đỉnh có chiều sâu, cạnh dẫn đến đỉnh nông hơn sẽ được coi là cạnh sau. Các cạnh dẫn đến các đỉnh sâu hơn sẽ bị bỏ qua vì chúng là hướng ngược lại của cạnh cây DFS hoặc cạnh không phải cây đã được xử lý. 

biểu hiện`depth[u] - depth[v] + 1`là độ dài chu kỳ chính xác. các`+1`rất dễ bị bỏ sót vì chênh lệch độ sâu chỉ tính các cạnh của cây, trong khi cạnh không phải là cây hiện tại là cạnh cuối cùng kết thúc chu trình.`cycle_edges`lưu trữ tổng độ dài chu kỳ, không phải số lượng chu kỳ. Vì các chu trình xương rồng có các cạnh rời rạc nên đây chính xác là số cạnh bị cấm coi là các cạnh giống như cây cầu độc lập. 

Số học mô-đun an toàn trong Python vì số nguyên có độ chính xác tùy ý. Mô đun rõ ràng sau mỗi phép nhân giữ giá trị trung gian nhỏ và tuân theo mô đun yêu cầu của`998244353`. 

Việc triển khai cũng bắt đầu DFS từ mọi đỉnh chưa được thăm dò. Điều này xử lý các đỉnh bị cô lập và các thành phần xương rồng bị ngắt kết nối mà không có trường hợp đặc biệt nào. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đồ thị là một hình tam giác duy nhất.```
3 3
1 2
2 3
3 1
```DFS có thể chọn các cạnh của cây`1-2`Và`2-3`. Khi nó chạm tới mép`3-1`, đỉnh`1`là tổ tiên của đỉnh`3`, thế là tìm được một chu trình. 

| Đỉnh hiện tại | Hàng xóm | Độ sâu hiện tại | Độ sâu của hàng xóm | Hành động | Độ dài chu kỳ | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 0 | -1 | Khám phá 2 | | 
| 2 | 3 | 1 | -1 | Khám phá 3 | | 
| 3 | 1 | 2 | 0 | Cạnh sau | 3 | 
| 3 | 2 | 2 | 1 | Cạnh cha mẹ | | 
| 2 | 1 | 1 | 0 | Cạnh cha mẹ | | 

Sự đóng góp của chu kỳ là 

[ 
2^3-1=7. 
] 

Cả ba cạnh đều thuộc chu trình này, vì vậy`cycle_edges = 3`và không có cạnh không chu trình độc lập. Câu trả lời cuối cùng là`7`. 

Điều này thể hiện quy tắc tính trung tâm: mọi tập con của các cạnh của tam giác đều hợp lệ ngoại trừ tập con không loại bỏ gì cả. 

### Mẫu 2 

Đồ thị chứa hai hình tam giác có chung đỉnh`2`:```
6 6
1 2
2 3
3 1
2 4
4 5
5 2
```Cây DFS có thể chứa các cạnh`1-2`,`2-3`,`2-4`, Và`4-5`. Hai cạnh còn lại đóng hai chu kỳ riêng biệt. 

| Đỉnh hiện tại | Hàng xóm | Độ sâu hiện tại | Độ sâu của hàng xóm | Hành động | Độ dài chu kỳ | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 1 | 0 | Cạnh cây | | 
| 2 | 3 | 0 | -1 | Khám phá 3 | | 
| 3 | 1 | 1 | 1 | Trở lại tổ tiên | 3 | 
| 2 | 4 | 0 | -1 | Khám phá 4 | | 
| 4 | 5 | 1 | -1 | Khám phá 5 | | 
| 5 | 2 | 2 | 0 | Trở lại tổ tiên | 3 | 

Có hai chu kỳ dài`3`, vậy hệ số chu kỳ là 

[ 
(2^3-1)(2^3-1)=7\cdot7=49. 
] 

Tất cả sáu cạnh đều thuộc một trong các chu trình này nên không có cạnh độc lập nào. Câu trả lời là`49`. 

Hai tam giác có chung một đỉnh không gây trở ngại cho sự lựa chọn của nhau vì chúng không có cạnh nào. Đây chính xác là sự độc lập được cung cấp bởi tài sản xương rồng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n+m)) | Mỗi mục nhập đỉnh và kề được xử lý với số lần không đổi và lũy thừa của hai được tính toán trước trong (O(n)). | 
| Không gian | (O(n+m)) | Các mảng kề, trạng thái DFS, bảng lũy ​​thừa và ngăn xếp rõ ràng đều sử dụng không gian tuyến tính. | 

Giới hạn cho phép lên tới`300000`đỉnh và`500000`các cạnh. Thuật toán xử lý đồ thị một cách tuyến tính, do đó nó tránh được sự phụ thuộc hàm mũ vào số lượng các sơ đồ loại bỏ cạnh có thể có. DFS lặp cũng tránh được các lỗi sâu về đệ quy Python trên các thành phần dạng cây dài. 

## Trường hợp thử nghiệm```python
import sys
import io

MOD = 998244353

def solve_data(inp: str) -> str:
    data = inp.split()
    it = iter(data)

    n = int(next(it))
    m = int(next(it))

    head = [-1] * n
    to = [0] * (2 * m)
    nxt = [0] * (2 * m)

    ptr = 0

    for _ in range(m):
        u = int(next(it)) - 1
        v = int(next(it)) - 1

        to[ptr] = v
        nxt[ptr] = head[u]
        head[u] = ptr
        ptr += 1

        to[ptr] = u
        nxt[ptr] = head[v]
        head[v] = ptr
        ptr += 1

    pow2 = [1] * (n + 1)
    for i in range(1, n + 1):
        pow2[i] = pow2[i - 1] * 2 % MOD

    depth = [-1] * n
    parent_edge = [-1] * n
    current_edge = [-1] * n

    answer = 1
    cycle_edges = 0

    for root in range(n):
        if depth[root] != -1:
            continue

        depth[root] = 0
        current_edge[root] = head[root]
        stack = [root]

        while stack:
            u = stack[-1]
            e = current_edge[u]

            if e == -1:
                stack.pop()
                continue

            current_edge[u] = nxt[e]

            if parent_edge[u] != -1 and e == (parent_edge[u] ^ 1):
                continue

            v = to[e]

            if depth[v] == -1:
                depth[v] = depth[u] + 1
                parent_edge[v] = e
                current_edge[v] = head[v]
                stack.append(v)
            elif depth[v] < depth[u]:
                length = depth[u] - depth[v] + 1
                cycle_edges += length
                answer = answer * (pow2[length] - 1) % MOD

    answer = answer * pow2[m - cycle_edges] % MOD
    return str(answer)

# Provided samples
assert solve_data(
    """3 3
1 2
2 3
3 1
"""
) == "7", "sample 1"

assert solve_data(
    """6 6
1 2
2 3
3 1
2 4
4 5
5 2
"""
) == "49", "sample 2"

# Minimum-size graph: one isolated vertex.
assert solve_data(
    """1 0
"""
) == "1", "minimum graph"

# A tree with four edges. Every edge is independent, so there are 2^4 schemes.
assert solve_data(
    """5 4
1 2
1 3
1 4
1 5
"""
) == "16", "tree with independent edges"

# One triangle plus one bridge.
assert solve_data(
    """4 4
1 2
2 3
3 1
3 4
"""
) == "14", "cycle plus bridge"

# Two disconnected triangles.
assert solve_data(
    """6 6
1 2
2 3
3 1
4 5
5 6
6 4
"""
) == "49", "disconnected cycles"

# Large boundary case: 300000 vertices, 149999 triangles sharing vertex 1.
# This gives 300000 vertices and 449997 edges.
n = 300000
lines = [f"{n} 449997"]
for i in range(149999):
    a = 2 + 2 * i
    b = a + 1
    lines.append(f"1 {a}")
    lines.append(f"{a} {b}")
    lines.append(f"{b} 1")

large_input = "\n".join(lines) + "\n"
expected = str(pow(7, 149999, MOD))

assert solve_data(large_input) == expected, "large cactus boundary case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0`|`1`| Đỉnh cô lập và cạnh bằng 0 | 
| Sao năm đỉnh |`16`| Mọi cạnh đều nằm ngoài một chu trình và có thể được loại bỏ độc lập | 
| Tam giác cộng một cầu |`14`| Phân tách chính xác các cạnh chu kỳ và các cạnh không chu trình | 
| Hai hình tam giác bị ngắt kết nối |`49`| Nhiều thành phần được kết nối và các yếu tố chu trình độc lập | 
| Cây xương rồng 300000 đỉnh có 149999 hình tam giác | (7^{149999}\bmod998244353) | Số đỉnh lớn, nhiều chu kỳ, DFS lặp và hiệu suất | 

## Vỏ cạnh 

Đối với một đỉnh bị cô lập, đầu vào là```
1 0
```DFS bắt đầu ở đỉnh`1`, gán cho nó độ sâu`0`và kết thúc ngay lập tức vì danh sách kề của nó trống. Không tìm thấy chu kỳ, vì vậy`cycle_edges = 0`. Hệ số cuối cùng là (2^0=1), cho kết quả đầu ra`1`. Điều này đếm chính xác bộ loại bỏ trống. 

Đối với một cái cây như```
5 4
1 2
1 3
1 4
1 5
```mọi cạnh DFS đều là cạnh cây và không có cạnh nào kết nối một đỉnh với tổ tiên. Do đó không có hệ số chu kỳ nào được nhân vào câu trả lời. Tất cả bốn cạnh đều là chu trình ngoài nên thừa số cuối cùng là (2^4=16). Thuật toán cho phép chính xác mọi sự kết hợp loại bỏ hoặc giữ các cạnh đó. 

Đối với một hình tam giác có một cây cầu,```
4 4
1 2
2 3
3 1
3 4
```DFS phát hiện cạnh sau đóng tam giác và thu được độ dài chu kỳ`3`. Nó thêm`3`ĐẾN`cycle_edges`, rời đi`4-3=1`cạnh độc lập. Câu trả lời trở thành 

[ 
(2^3-1)2=7\cdot2=14. 
] 

Cây cầu có thể được loại bỏ mặc dù không cần thiết để phá vỡ chu trình, đó là lý do tại sao hệ số của nó phải được giữ nguyên trong công thức. 

Đối với các hình tam giác bị ngắt kết nối,```
6 6
1 2
2 3
3 1
4 5
5 6
6 4
```DFS đầu tiên tìm thấy chu trình có độ dài ba, sau đó vòng lặp bên ngoài đạt đến đỉnh`4`và bắt đầu một DFS khác vì nó vẫn chưa được truy cập. DFS thứ hai tìm thấy chu kỳ dài ba thứ hai. Kết quả thu được là (7\cdot7=49). Điều này chứng tỏ tại sao vòng lặp bên ngoài trên tất cả các đỉnh là cần thiết. 

Đối với một cây xương rồng lớn chứa nhiều chu kỳ ngắn, ngăn xếp rõ ràng sẽ ngăn ngừa các vấn đề về độ sâu đệ quy. Trong phép thử với 149999 hình tam giác, mỗi chu trình đều đóng góp hệ số`7`, vậy đáp án là (7^{149999}) modulo`998244353`. DFS vẫn chỉ xử lý từng cạnh trong số khoảng 450000 cạnh với số lần không đổi, duy trì độ phức tạp tuyến tính.
