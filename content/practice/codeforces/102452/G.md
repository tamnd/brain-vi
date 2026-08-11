---
title: "CF 102452G - Thiết kế trò chơi"
description: "Chúng ta phải xây dựng một cây có gốc mà gốc đại diện cho cơ sở. Mỗi chiếc lá đều chứa đựng một con quái vật và con quái vật đó sẽ đi về phía gốc rễ."
date: "2026-08-10T06:16:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102452
codeforces_index: "G"
codeforces_contest_name: "2019-2020 ICPC Asia Hong Kong Regional Contest"
rating: 0
weight: 102452
solve_time_s: 117
verified: true
draft: false
---

[CF 102452G - Thiết kế trò chơi](https://codeforces.com/problemset/problem/102452/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 57 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta phải xây dựng một cây có gốc mà gốc đại diện cho cơ sở. Mỗi chiếc lá đều chứa đựng một con quái vật và con quái vật đó sẽ đi về phía gốc rễ. Một tòa tháp được đặt ở một đỉnh sẽ tiêu diệt mọi quái vật đạt đến đỉnh đó, do đó, một tập hợp các tòa tháp chỉ hợp lệ khi mọi đường đi từ gốc đến lá đều chứa ít nhất một tòa tháp. 

Mỗi đỉnh có chi phí xây dựng dương. Trong số tất cả các bộ tháp hợp lệ, chúng ta muốn chính xác (K) các bộ khác nhau có tổng chi phí tối thiểu. Đầu vào chỉ chứa (K), với (1\le K\le 10^9). Chúng ta có thể tự do chọn cả cây và giá mỗi đỉnh. 

Đầu ra mô tả cây bằng cách đưa ra đỉnh gốc của mọi đỉnh ngoại trừ gốc, theo sau là tất cả chi phí của đỉnh. Cây phải chứa ít nhất hai và nhiều nhất (10^5) đỉnh, trong khi mọi giá phải nằm trong khoảng từ (1) đến (10^9). 

Giới hạn trên lớn của (K) là manh mối chính. Chúng ta không thể tạo một đỉnh cho mọi giải pháp tối ưu, vì (K) có thể là một tỷ. Do đó, việc xây dựng cần làm cho số lượng lời giải tối ưu tăng nhanh hơn nhiều so với số đỉnh. Giới hạn cuộc thi 0,5 giây khiến việc xây dựng (O(K)) đặc biệt không phù hợp. 

Có một số trường hợp ranh giới rất dễ bị xử lý sai. Đối với (K=1), không được phép có một lá vì cây đầu ra phải có ít nhất hai đỉnh. Một cách xây dựng nhỏ nhất đúng là```
1
```về mặt khái niệm được biểu thị bằng một gốc có một lá, với chi phí (2,1). Lá rẻ hơn nên chỉ có một giải pháp tối ưu. 

Với (K=2), cách xây dựng nhỏ nhất là```
2
1
1 1
```Có một gốc và một lá. Xây dựng phần gốc có chi phí (1), trong khi xây dựng phần lá cũng có chi phí (1), như vậy có đúng hai phương án tối ưu. Việc triển khai bất cẩn luôn làm cho root rẻ hơn hoặc đắt hơn sẽ vô tình chỉ tạo ra một giải pháp tối ưu. 

Với (K=3), chúng ta cần ba giải pháp tối ưu mà không cần sử dụng ba lựa chọn độc lập. Một cách xây dựng hợp lệ là```
5
1 2 1 4
2 2 1 1 1
```Gốc có hai cây con. Cây con bên trái có hai giải pháp tối ưu và cây con bên phải có hai giải pháp tối ưu. Nếu gốc không được sử dụng, các lựa chọn sẽ nhân lên bốn khả năng. Bản thân gốc có cùng chi phí với bốn lựa chọn đó, tạo ra một lựa chọn tối ưu bổ sung, tổng cộng là năm chứ không phải ba. Điều này minh họa tại sao chi phí và cấu trúc đệ quy phải được thiết kế cùng nhau. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ bắt đầu với một số cây ứng cử viên và liệt kê mọi tập hợp con các đỉnh như một tập hợp các vị trí tháp có thể có. Đối với mỗi tập hợp con, chúng ta có thể kiểm tra mọi đường dẫn từ lá tới gốc, xác định xem tập hợp con đó có chặn mọi đường dẫn hay không, tính toán chi phí của nó và giữ lại chi phí tối thiểu cũng như bội số của nó. Có (2^N) tập hợp con và việc kiểm tra một tập hợp con có thể yêu cầu công việc (O(N)), do đó, công việc trong trường hợp xấu nhất là tối đa (N2^N) lượt truy cập đỉnh. Với (N=10^5), điều này hoàn toàn không thể thực hiện được. 

Cách hữu ích hơn để xem xét vấn đề là hỏi điều gì xảy ra bên trong một cây con. Gọi (dp(v)) là chi phí tối thiểu cần thiết để bảo vệ tất cả các lá trong cây con của (v) và gọi (cách(v)) là số cách để đạt được mức tối thiểu đó. 

Giả sử (v) là một đỉnh trong có các đỉnh con (u_1,u_2,\ldots,u_t). Có chính xác hai khả năng có ý nghĩa cho một giải pháp tối ưu bên trong cây con này. Chúng ta có thể xây một tòa tháp ở (v), chi phí (c_v). Vì tất cả các chi phí đều dương nên một giải pháp tối ưu đã sử dụng (v) không có lý do gì để đặt thêm các tháp bên dưới nó. Điều này đưa ra chính xác một giải pháp về chi phí (c_v). 

Khả năng khác là không đặt tháp ở (v). Khi đó mỗi cây con con phải được bảo vệ độc lập. Chi phí là 

[ 
S=\sum_i dp(u_i), 
] 

và số cách là 

[ 
P=\prod_i cách(u_i), 
] 

bởi vì chúng ta có thể độc lập lựa chọn giải pháp tối ưu ở mỗi đứa trẻ. 

Do đó, hành vi cục bộ hoàn toàn được xác định bằng cách so sánh (c_v) với (S). Nếu (c_v<S), có một giải pháp tối ưu. Nếu (c_v>S) thì có (P) nghiệm tối ưu. Nếu (c_v=S), cả hai khả năng đều tối ưu, cho ra nghiệm (P+1). 

Sự tái diễn này là chìa khóa cho việc xây dựng. Chúng ta không cần tìm cây có nghiệm (K). Chúng ta có thể cố ý xây dựng một đỉnh có số lượng lời giải tối ưu là tích của số con hoặc nhiều hơn tích đó một. 

Danh tính đặc biệt hữu ích là 

[ 
K=2\left\lfloor\frac K2\right\rfloor 
] 

khi (K) chẵn và 

[ 
K=2\left\lfloor\frac K2\right\rfloor+1 
] 

khi (K) lẻ. 

Vì vậy, với mỗi (K>2), chúng ta tạo ra hai cây con con. Giải pháp đầu tiên có chính xác (\lfloor K/2\rfloor) giải pháp tối ưu và giải pháp thứ hai có chính xác (2). Tích của họ là (K) cho số chẵn (K), trong khi đó là (K-1) cho số lẻ (K). Sau đó chúng tôi chọn chi phí gốc một cách thích hợp. Đối với chẵn (K), làm cho gốc đắt hơn hẳn so với việc bảo vệ cả hai con, do đó sản phẩm vẫn còn (K). Đối với số lẻ (K), hãy làm cho gốc chính xác bằng việc bảo vệ cả hai phần tử con, thêm một giải pháp trong đó gốc tự lấy được tháp. 

Phần quan trọng là (\lfloor K/2\rfloor) được sử dụng đệ quy. Mỗi cấp độ gần như giảm một nửa (K), do đó số cấp độ đệ quy chỉ là (O(\log K)). Con thứ hai luôn là cấu trúc cố định cực nhỏ cho hai phương án tối ưu. 

Đây chính xác là cấu trúc được sử dụng bởi các giải pháp của cuộc thi được chấp nhận cho vấn đề này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(N2^N)) | (O(N)) | Quá chậm | 
| Tối ưu | (O(\log K)) | (O(\log K)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định cấu trúc đệ quy`solve(k, parent)`tạo ra gốc của cây con có chính xác (k) nghiệm tối ưu. Nó trả về chi phí tối thiểu của cây con đó. 
2. Với (k=1) hoặc (k=2), tạo một gốc và một lá. Cho biết chi phí lá (1). Với (k=1), hãy cho biết chi phí gốc (2), vậy chỉ có lá là tối ưu. Với (k=2), hãy cho giá trị gốc (1), do đó việc chọn gốc và chọn lá là bằng nhau. 
3. Đối với (k>2), tạo một gốc mới và xây dựng đệ quy một cây con con với nghiệm tối ưu (\lfloor k/2\rfloor). 
4. Tạo con thứ hai bằng cách sử dụng cấu trúc cố định (k=2). Chi phí tối thiểu của nó là (1) và nó có đúng hai nghiệm tối ưu. 
5. Gọi (S) là tổng chi phí tối thiểu mà hai đứa trẻ trả lại. Nếu (k) chẵn, hãy ấn định chi phí gốc mới (S+1). Gốc khi đó quá đắt nên mỗi giải pháp tối ưu phải độc lập lựa chọn một giải pháp tối ưu từ cả hai giải pháp con. Số cách giải quyết là 

[ 
\frac{k}{2}\cdot2=k. 
] 

1. Nếu (k) là số lẻ, hãy gán chính xác chi phí gốc mới (S). Hiện nay có hai loại giải pháp tối ưu. Một người sử dụng chính gốc, trong khi người kia không sử dụng nó và độc lập chọn giải pháp tối ưu cho cả hai đứa trẻ. Như vậy số đó là 

[ 
\frac{k-1}{2}\cdot2+1=k. 
] 

1. Đính kèm mọi cây con mới được tạo bên dưới cây gốc được cung cấp cho lệnh gọi đệ quy. Vì các đỉnh được tạo trước các đỉnh con của chúng nên mọi chỉ mục cha đều nhỏ hơn các chỉ mục con của nó, điều này làm cho định dạng mảng cha được yêu cầu trở nên ngay lập tức. 

Tại sao nó hoạt động: bất biến của`solve(k, parent)`là cây con được xây dựng có chính xác (k) bộ tháp hợp lệ có chi phí tối thiểu và giá trị trả về là chi phí tối thiểu của cây con đó. Các trường hợp cơ sở thỏa mãn trực tiếp bất biến này. Đối với (k>2), con thứ nhất có (\lfloor k/2\rfloor) giải pháp tối ưu và con thứ hai cố định có hai giải pháp tối ưu. Nếu (k) chẵn thì nghiệm mới có chủ ý là quá đắt một đơn vị, do đó tất cả các giải pháp tối ưu đều đến từ các lựa chọn con độc lập và số lượng của chúng nhân lên (k). Nếu (k) là số lẻ thì nghiệm được gắn với nghiệm chỉ con, thêm chính xác một khả năng vào (k-1). Do đó, mọi lệnh gọi đệ quy đều giữ nguyên bất biến cho đến khi đạt được (K) ban đầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def construct(k):
    parent = [0]
    cost = [0]

    def add_node(p):
        parent.append(p)
        cost.append(0)
        return len(parent) - 1

    def solve(k, p):
        v = add_node(p)

        if k <= 2:
            leaf = add_node(v)
            cost[leaf] = 1

            # k = 1: root costs 2, so only the leaf is optimal.
            # k = 2: root costs 1, tying the leaf.
            cost[v] = 3 - k
            return 1

        left_cost = solve(k // 2, v)
        right_cost = solve(2, v)

        best_without_root = left_cost + right_cost

        # Odd k: tie, giving one extra solution.
        # Even k: root is more expensive, so only child choices remain.
        cost[v] = best_without_root + (k % 2 == 0)

        return best_without_root

    solve(k, 0)

    n = len(parent) - 1

    out = [str(n)]
    out.append(" ".join(map(str, parent[2:])))
    out.append(" ".join(map(str, cost[1:])))

    return "\n".join(out)

def main():
    k = int(input())
    sys.stdout.write(construct(k) + "\n")

if __name__ == "__main__":
    main()
```Hai mảng`parent`Và`cost`lưu trữ chính xác thông tin được yêu cầu bởi đầu ra. Chỉ số 0 là một mục nhập giả, vì vậy đỉnh (1) được lưu trữ tại chỉ mục (1), khớp với cách đánh số dựa trên một của câu lệnh. 

Trường hợp cơ sở cố tình tạo ra hai đỉnh thay vì cho phép cây một đỉnh. Với (k=1), chi phí là (2,1), do đó giải pháp tối ưu duy nhất là chiếc lá. Với (k=2), chi phí là (1,1), tạo ra hai lựa chọn ràng buộc. 

Đối với cuộc gọi đệ quy với (k>2),`left_cost + right_cost`là chi phí tối thiểu khi gốc mới không có tháp. biểu thức`(k % 2 == 0)`là một boolean Python, đánh giá là`1`với số chẵn (k) và`0`cho số lẻ (k). Do đó, gốc sẽ đắt hơn một đơn vị trong trường hợp chẵn và được gắn chính xác trong trường hợp lẻ. 

Độ sâu đệ quy chỉ (O(\log K)), nhiều nhất là khoảng 30 cho (K\le10^9), vì vậy giới hạn đệ quy của Python không phải là vấn đề đáng lo ngại. Tổng số đỉnh cũng rất nhỏ. Trong thực tế, nếu (T(k)) là số đỉnh thì với (k>2), 

[ 
T(k)=1+T(\lfloor k/2\rfloor)+T(2) 
] 

và (T(1)=T(2)=2). Đối với (K=10^9), điều này cho ít hơn 100 đỉnh. 

Tất cả các chi phí vẫn còn rất nhỏ. Chi phí tối thiểu được trả về chỉ tăng một ở mỗi cấp độ đệ quy, do đó chi phí lớn nhất là (O(\log K)), thấp hơn nhiều so với mức cho phép (10^9). 

## Ví dụ đã hoạt động 

Mẫu được cung cấp có (K=2). Cuộc gọi đệ quy ngay lập tức đến trường hợp cơ sở. 

| Bước | (k) | Giá gốc mới | Chi phí trẻ em | Những cách tối ưu | 
| --- | --- | --- | --- | --- | 
| Xây dựng cơ sở | 2 | 1 | 1 | 2 | 

Cây kết quả là```
2
1
1 1
```Có hai giải pháp tối ưu. Một người đặt tháp ở gốc, còn người kia đặt nó ở lá. Cả hai đều có giá (1). 

Đối với (K=3), gốc xây dựng đệ quy một cây con (k=1) và một cây con (k=2). 

| Bước | (k) | Số lượng trẻ em | Chi phí trẻ em | Giá gốc | Kết quả đếm | 
| --- | --- | --- | --- | --- | --- | 
| Con trái | 1 | 1 | 1 | 2 | 1 | 
| Đúng con | 2 | 1 | 1 | 1 | 2 | 
| Gốc cuối cùng | 3 | (1,2) | (1,1) | 2 | (1+1\cdot2=3) | 

Kết quả đầu ra là```
5
1 2 1 4
2 2 1 1 1
```Cây con bên trái có một giải pháp tối ưu và cây con bên phải có hai giải pháp tối ưu. Nếu gốc không được chọn, sẽ có (1\cdot2=2) kết hợp tối ưu. Vì bản thân gốc có tổng chi phí bằng nhau nên chỉ chọn gốc sẽ mang lại thêm một giải pháp tối ưu. Tổng số là (3). 

Đối với một giá trị chẵn chẳng hạn như (K=4), cả hai đệ quy con đều có hai nghiệm tối ưu. 

| Bước | (k) | Số lượng trẻ em | Chi phí trẻ em | Giá gốc | Kết quả đếm | 
| --- | --- | --- | --- | --- | --- | 
| Con trái | 2 | 2 | 1 | 1 | 2 | 
| Đúng con | 2 | 2 | 1 | 1 | 2 | 
| Gốc cuối cùng | 4 | (2,2) | (1,1) | 3 | (2\cdot2=4) | 

Ở đây giá trị gốc là (3), trong khi hai cây con con cùng nhau chỉ có giá (2). Vì vậy, một giải pháp tối ưu không thể sử dụng gốc. Nó phải chọn một cách độc lập một trong hai giải pháp tối ưu ở mỗi đứa trẻ, tạo ra các khả năng (2\cdot2=4). 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(\log K)) | Mỗi cấp độ đệ quy thay thế (k) bằng (\lfloor k/2\rfloor) và thêm một cây con có kích thước không đổi (k=2). | 
| Không gian | (O(\log K)) | Cây được xây dựng có các đỉnh (O(\log K)) và độ sâu đệ quy là (O(\log K)). | 

Vì (K\le10^9), có ít hơn 30 lần chia đôi trước khi đạt đến trường hợp cơ sở. Do đó, việc xây dựng chỉ tạo ra vài chục đỉnh, nằm dưới giới hạn (10^5). Chi phí cũng chỉ là (O(\log K)), vì vậy chúng thấp hơn nhiều (10^9). Điều này dễ dàng phù hợp với giới hạn 0,5 giây và 512 MB của cuộc thi. 

## Trường hợp thử nghiệm 

Bởi vì đây là một bài toán mang tính xây dựng nên kết quả đầu ra không phải là duy nhất. Một xác nhận so sánh chuỗi đầu ra hoàn chỉnh với một câu trả lời cố định sẽ từ chối các cấu trúc hoàn toàn hợp lệ. Thay vào đó, trình trợ giúp kiểm tra bên dưới sẽ phân tích cây được tạo ra và tính toán lại chi phí tối thiểu cũng như số lượng giải pháp tối ưu một cách độc lập.```python
import sys
import io

def construct(k):
    parent = [0]
    cost = [0]

    def add_node(p):
        parent.append(p)
        cost.append(0)
        return len(parent) - 1

    def solve(k, p):
        v = add_node(p)

        if k <= 2:
            leaf = add_node(v)
            cost[leaf] = 1
            cost[v] = 3 - k
            return 1

        a = solve(k // 2, v)
        b = solve(2, v)
        s = a + b
        cost[v] = s + (k % 2 == 0)
        return s

    solve(k, 0)

    n = len(parent) - 1
    return (
        str(n) + "\n" +
        " ".join(map(str, parent[2:])) + "\n" +
        " ".join(map(str, cost[1:])) + "\n"
    )

def run(inp: str) -> str:
    k = int(inp.strip())
    return construct(k)

def validate(output: str, wanted_k: int) -> bool:
    data = output.split()
    pos = 0

    n = int(data[pos])
    pos += 1

    if not (2 <= n <= 100000):
        return False

    parents = [0, 0]
    for v in range(2, n + 1):
        p = int(data[pos])
        pos += 1
        if not (1 <= p < v):
            return False
        parents.append(p)

    costs = [0]
    for _ in range(n):
        c = int(data[pos])
        pos += 1
        if not (1 <= c <= 10**9):
            return False
        costs.append(c)

    if pos != len(data):
        return False

    children = [[] for _ in range(n + 1)]
    for v in range(2, n + 1):
        children[parents[v]].append(v)

    dp = [0] * (n + 1)
    ways = [0] * (n + 1)

    for v in range(n, 0, -1):
        if not children[v]:
            dp[v] = costs[v]
            ways[v] = 1
            continue

        child_cost = 0
        child_ways = 1

        for u in children[v]:
            child_cost += dp[u]
            child_ways *= ways[u]

        if costs[v] < child_cost:
            dp[v] = costs[v]
            ways[v] = 1
        elif costs[v] > child_cost:
            dp[v] = child_cost
            ways[v] = child_ways
        else:
            dp[v] = costs[v]
            ways[v] = child_ways + 1

    return ways[1] == wanted_k

# Provided sample.
assert validate(run("2\n"), 2), "sample 1"

# Minimum K.
assert validate(run("1\n"), 1), "K = 1"

# Small odd value, exercises the tie case.
assert validate(run("3\n"), 3), "K = 3"

# Small even value, exercises the product case.
assert validate(run("4\n"), 4), "K = 4"

# Maximum allowed K.
assert validate(run("1000000000\n"), 1000000000), "K = 10^9"

print("all tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2`| Bất kỳ cây hợp lệ nào có đúng 2 nghiệm tối ưu, bao gồm`2 / 1 / 1 1`| Cung cấp mẫu và trường hợp đẳng thức | 
|`1`| Bất kỳ cây hợp lệ nào có đúng 1 nghiệm tối ưu | Tối thiểu (K) và bắt buộc (N\ge2) | 
|`3`| Bất kỳ cây hợp lệ nào có đúng 3 nghiệm tối ưu | Lẻ (K), trong đó gốc thêm một giải pháp gắn | 
|`4`| Bất kỳ cây hợp lệ nào có đúng 4 nghiệm tối ưu | Chẵn (K), trong đó số nghiệm con nhân lên | 
|`1000000000`| Bất kỳ cây hợp lệ nào có chính xác (10^9) lời giải tối ưu | Tối đa (K), độ sâu và giới hạn đệ quy | 

## Vỏ cạnh 

Với (K=1), thuật toán đi vào trường hợp cơ sở và tạo ra hai đỉnh. Lá có giá (1), còn rễ có giá (2). Giải pháp hợp lệ với chi phí tối thiểu duy nhất là đặt tháp ở phần lá. Đầu ra chính xác là```
2
1
2 1
```Cấu trúc một đỉnh cũng sẽ có một giải pháp tối ưu rõ ràng, nhưng nó vi phạm yêu cầu cây phải có ít nhất hai đỉnh. Trường hợp cơ sở rõ ràng sẽ tránh được sai lầm đó. 

Với (K=2), trường hợp cơ sở tạo ra hai đỉnh có chi phí (1,1). Tháp ở gốc sẽ giết chết quái vật của chiếc lá duy nhất, trong khi tháp ở lá cũng giết chết quái vật đó. Cả hai đều có chi phí tối thiểu (1), vì vậy số lượng chính xác là hai. Đầu ra là```
2
1
1 1
```Đối với số lẻ (K>2), gốc phải gắn với chi phí bảo vệ con của nó. Hãy xem xét (K=5). Con đệ quy có (2) nghiệm và con thứ hai cố định cũng có (2), đưa ra bốn kết hợp chỉ dành cho con. Việc đặt chi phí gốc bằng với chi phí kết hợp của chúng sẽ thêm giải pháp chỉ có gốc, tạo ra (4+1=5). Nếu gốc được làm rẻ hơn dù chỉ một đơn vị, sẽ chỉ có một giải pháp tối ưu, trong khi làm cho nó đắt hơn sẽ chỉ còn lại bốn. 

Đối với số chẵn (K>2), gốc phải đắt hơn hai cây con cộng lại. Với (K=6), đứa thứ nhất có ba đáp án và đứa thứ hai có hai. Sự lựa chọn độc lập của họ cho (3\cdot2=6). Gốc được chỉ định nhiều hơn chi phí con kết hợp, ngăn không cho nó trở thành một mức tối ưu khác. Sử dụng đẳng thức ở đây sẽ tạo ra bảy nghiệm không chính xác. 

Đối với (K=10^9), đối số đệ quy vẫn còn ngắn. Tham số đệ quy đầu tiên theo sau 

[ 
10^9,\ 5\cdot10^8,\ 2.5\cdot10^8,\ldots 
] 

cho đến khi đạt (1) hoặc (2), sau khoảng 30 cấp độ. Mỗi cấp độ chỉ thêm một gốc và một cây con hai đỉnh cố định, do đó cây cuối cùng chứa ít hơn 100 đỉnh. Điều này xử lý đầu vào lớn nhất mà không đạt đến giới hạn đỉnh (10^5). 

Cuối cùng, chi phí dương là điều cần thiết cho lập luận tính toán. Khi một tháp được đặt ở một đỉnh, việc thêm một tháp con không cần thiết không bao giờ có thể bảo toàn được chi phí tối thiểu vì mỗi tháp bổ sung có giá ít nhất là (1). Do đó, phương án thay thế "tháp ở đỉnh này" đóng góp chính xác một giải pháp tối ưu chứ không phải là toàn bộ tập hợp các giải pháp bên dưới đỉnh đó. Đây là điều làm cho việc lặp lại (1+\prod way) trở nên chính xác.
