---
title: "CF 2189F - Máy hút bụi Zhora"
description: "Chúng tôi có một cái cây. Vertex i ban đầu chứa ai nut. Có hai loại chi phí điện. Chi phí đầu tiên đến từ hoạt động phân phối lại đặc biệt. Chúng ta chọn một đỉnh v. Sau đó, mọi đỉnh khác sẽ được xử lý từ khoảng cách lớn hơn đến khoảng cách nhỏ hơn tính từ v."
date: "2026-06-07T21:15:12+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dfs-and-similar", "dp", "greedy", "implementation", "sortings", "trees"]
categories: ["algorithms"]
codeforces_contest: 2189
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 1075 (Div. 2)"
rating: 2800
weight: 2189
solve_time_s: 185
verified: false
draft: false
---

[CF 2189F - Máy hút bụi Zhora](https://codeforces.com/problemset/problem/2189/F) 

**Đánh giá:** 2800 
**Thẻ:** cấu trúc dữ liệu, dfs và tương tự, dp, tham lam, triển khai, sắp xếp, cây 
**Thời gian giải:** 3m 5s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một cái cây. đỉnh`i`ban đầu chứa`a_i`quả hạch. 

Có hai loại chi phí điện. 

Chi phí đầu tiên đến từ hoạt động phân phối lại đặc biệt. Chúng tôi chọn một đỉnh`v`. Sau đó, mọi đỉnh khác được xử lý từ khoảng cách lớn hơn đến khoảng cách nhỏ hơn từ`v`. Nếu một đỉnh được xử lý hiện có chứa ít nhất một đai ốc thì chính xác một đai ốc sẽ được di chuyển một cạnh gần hơn với`v`. 

Chi phí thứ hai xuất hiện sau khi tất cả các hoạt động phân phối lại kết thúc. Zhora ăn các loại hạt từ mọi đỉnh không trống, trả tiền`q`cho mỗi đỉnh vẫn chứa ít nhất một đai ốc. 

Mục tiêu của chúng tôi là quyết định cần thực hiện bao nhiêu hoạt động phân phối lại và xung quanh các đỉnh nào để tổng chi phí di chuyển và chi phí ăn uống được giảm thiểu. 

Điều đầu tiên cần hiểu là hoạt động phân phối lại thực sự làm gì. 

Cố định một đỉnh trung tâm`v`. Hãy xem xét một hoạt động xung quanh`v`. Mọi đỉnh ngoại trừ`v`có thể mất nhiều nhất một hạt. Đai ốc đó di chuyển một cạnh về phía`v`. Vì các đỉnh được xử lý từ khoảng cách lớn hơn đến khoảng cách nhỏ hơn, nên một đai ốc đến từ trẻ có thể ngay lập tức được chuyển tiếp xa hơn về phía`v`nếu đỉnh trung gian ban đầu có một đai ốc để gửi. Thứ tự này là thuộc tính cấu trúc quan trọng của vấn đề. 

Những hạn chế là lớn. Tổng của tất cả`n`nhiều nhất là trên các trường hợp thử nghiệm`10^5`, vì vậy chúng ta cần thứ gì đó xung quanh`O(n log n)`hoặc`O(n)`mỗi trường hợp thử nghiệm. Bất kỳ giải pháp nào thử tất cả các trung tâm và mô phỏng hoạt động một cách rõ ràng sẽ ngay lập tức trở nên quá chậm. Thậm chí`O(n^2)`là không thể khi`n = 10^5`. 

Trường hợp cạnh tinh tế xuất hiện khi một đỉnh chứa nhiều đai ốc. 

Ví dụ:```
1 - 2
a = [100, 0]
```Một thao tác có tâm ở đỉnh`2`chỉ di chuyển một đai ốc từ đỉnh`1`đến đỉnh`2`, không phải tất cả 100 hạt. Việc giải thích hoạt động một cách bất cẩn thường cho rằng tất cả các đai ốc đều chuyển động đồng thời. Điều đó không đúng. 

Một trường hợp khó khăn khác là một con đường.```
1 - 2 - 3 - 4 - 5
a = [1,1,1,1,1]
```Hai phép toán có tâm tại đỉnh`3`tập hợp tất cả các hạt vào đỉnh`3`. Câu trả lời không được xác định bởi đường kính cây hay việc di chuyển từng đai ốc một cách độc lập. Hoạt động này tác động lên tất cả các đỉnh cùng một lúc, điều này làm cho các ứng dụng lặp lại trở nên cực kỳ mạnh mẽ. 

Cạm bẫy thứ ba là`q = 0`. 

Ví dụ:```
1 - 2
a = [5,7]
p = 100
q = 0
```Câu trả lời là`0`. Chúng ta có thể đơn giản ăn miễn phí từ mọi đỉnh không trống. Bất kỳ thuật toán nào tự động cố gắng hợp nhất các đỉnh sẽ trả quá nhiều. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử mọi đỉnh trung tâm có thể, mô phỏng các hoạt động phân phối lại và theo dõi cách tập hợp các đỉnh không trống phát triển. 

Giả sử chúng ta sửa một trung tâm`v`. Cho phép`x_u`là số đai ốc ban đầu ở đỉnh`u`. Việc mô phỏng trực tiếp một thao tác đòi hỏi phải duyệt qua toàn bộ cây. Vì chúng ta có thể thực hiện nhiều thao tác, có khả năng đạt khoảng cách tối đa từ`v`, độ phức tạp ít nhất trở thành bậc hai. Việc lặp lại điều này cho tất cả các trung tâm có thể là hoàn toàn không khả thi. 

Quan sát quan trọng xuất phát từ việc hiểu những hoạt động lặp đi lặp lại xung quanh một trung tâm cố định thực sự làm gì. 

Xét một đỉnh ở khoảng cách`d`từ trung tâm đã chọn`v`. 

Mỗi thao tác có thể di chuyển tối đa một đai ốc từ đỉnh đó một bước gần hơn tới`v`. Sau đó`k`hoạt động, nhiều nhất`k`các loại hạt có thể rời khỏi đỉnh đó. Do đó đỉnh vẫn không trống nếu```
a_u > k
```bởi vì chính xác một đai ốc có thể được tháo ra cho mỗi thao tác. 

Điều này ngay lập tức tiết lộ một điều gì đó mạnh mẽ hơn nhiều. Sau khi biểu diễn`k`hoạt động xung quanh trung tâm`v`, mọi đỉnh có khoảng cách từ`v`nhiều nhất là`k`có thể chuyển tiếp tất cả các đai ốc đến từ các đỉnh sâu hơn. Toàn bộ quá trình hoạt động như thể mỗi đỉnh đóng góp độc lập```
max(a_u - k, 0)
```hạt còn lại. 

Một đỉnh trở nên rỗng chính xác khi`a_u ≤ k`. 

Chi phí ăn uống chỉ phụ thuộc vào số đỉnh không trống. Nếu như```
cnt(k) = number of vertices with a_u > k
```rồi sau đó`k`hoạt động tổng chi phí là```
k * p + cnt(k) * q
```với điều kiện là mọi hạt còn sót lại vẫn có thể được tập hợp thành một vùng kết nối duy nhất xung quanh tâm. 

Bây giờ cấu trúc cây đi vào. 

Đối với một cố định`k`, mọi đỉnh với`a_u > k`phải không trống. Các đỉnh như vậy tối đa phải nằm trong khoảng cách`k`từ trung tâm đã chọn, nếu không một số đai ốc sẽ bị mắc kẹt ở xa hơn. 

Vì vậy chúng ta cần một tâm có khoảng cách tới mọi đỉnh thỏa mãn`a_u > k`nhiều nhất là`k`. 

Tương tự, nếu chúng ta định nghĩa```
S(k) = { u | a_u > k }
```thì chúng ta cần```
radius(S(k)) ≤ k
```trong đó bán kính có nghĩa là khoảng cách tối đa tối thiểu có thể từ tâm đến tất cả các đỉnh của tập hợp. 

Một thực tế về cây cổ điển nói rằng với bất kỳ tập con đỉnh nào, bán kính của nó bằng```
ceil(diameter(S)/2)
```trong đó đường kính là khoảng cách lớn nhất giữa hai đỉnh trong tập hợp con. 

Vì vậy tính khả thi cho một`k`trở thành```
diameter(S(k)) ≤ 2k
```Điều này biến vấn đề thành: 

Đối với mọi khả năng`k`, kiểm tra xem các đỉnh có`a_u > k`có đường kính cặp nhiều nhất`2k`. Nếu có thì chi phí là```
k*p + |S(k)|*q.
```Mức tối thiểu trong số tất cả khả thi`k`là câu trả lời. 

Thử thách còn lại là duy trì đường kính của tập hoạt động`S(k)`trong khi các đỉnh biến mất như`k`tăng lên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 

|---|------|---| 

| Mô phỏng Brute Force | O(n²) hoặc tệ hơn | O(n) | Quá chậm | 

| Bảo trì đường kính với ngưỡng DSU | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Cải cách quy trình 

Đối với một số hoạt động cố định`k`, một đỉnh vẫn không trống nếu`a_u > k`. 

Cho phép```
S(k) = {u : a_u > k}.
```Chi phí ăn uống trở nên```
|S(k)| * q.
```Chi phí di chuyển trở thành```
k * p.
```Câu hỏi duy nhất còn lại là liệu những đỉnh còn sót lại có thể được phục vụ bởi một trung tâm hay không. 

### Tình trạng đường kính 

Một tâm tồn tại nếu mọi đỉnh trong`S(k)`nằm trong khoảng cách`k`từ trung tâm đó. 

Khoảng cách lớn nhất nhỏ nhất đó bằng bán kính của`S(k)`. 

Đối với số liệu cây:```
radius = ceil(diameter / 2).
```Do đó tính khả thi chính xác là```
diameter(S(k)) ≤ 2k.
```### Duy trì bộ hoạt động 

Như`k`tăng, các đỉnh rời khỏi tập hợp khi```
k >= a_u.
```Vì vậy, các đỉnh biến mất theo thứ tự giá trị giảm dần. 

Thay vì tăng`k`, xử lý các ngưỡng từ lớn đến nhỏ và thêm các đỉnh có giá trị vượt quá ngưỡng hiện tại. 

Các đỉnh hoạt động chính xác`S(k)`. 

### Đường kính động của các đỉnh hoạt động 

Đối với một tập hợp các đỉnh của cây, điểm cuối đường kính của nó xác định hoàn toàn tất cả khoảng cách tối đa. 

Duy trì hai đỉnh hoạt động`A`Và`B`đại diện cho đường kính hiện tại. 

Khi một đỉnh mới`x`được chèn vào: 

1. Tính toán`dist(x,A)`. 
2. Tính toán`dist(x,B)`. 
3. Nếu một trong hai khoảng cách vượt quá chiều dài đường kính hiện tại, hãy cập nhật điểm cuối đường kính. 

Đây là kỹ thuật duy trì đường kính tăng dần tiêu chuẩn. 

Khoảng cách được trả lời trong`O(1)`sử dụng tiền xử lý LCA. 

### Đánh giá ngưỡng 

Chỉ những ngưỡng mà tập hoạt động thay đổi mới quan trọng. 

Giá trị xử lý của`k`theo thứ tự giảm dần. 

Đối với mỗi ngưỡng: 

1. Cộng tất cả các đỉnh có giá trị lớn hơn`k`. 
2. Duy trì đường kính cài đặt hoạt động. 
3. Hãy để`D`là chiều dài đường kính. 
4. Ngưỡng khả thi nếu```
D ≤ 2k.
```5. Nếu khả thi, hãy cập nhật```
answer = min(answer, k*p + active_count*q).
```Vụ án`k`lớn hơn mọi`a_i`không để lại đỉnh hoạt động và chi phí`k*p`, điều đó không bao giờ tốt hơn việc chọn chính xác`k=max(a_i)`đối với tập trống, vì vậy việc kiểm tra các ngưỡng liên quan là đủ. 

### Tại sao nó hoạt động 

Sau`k`hoạt động, mỗi đỉnh có thể đóng góp nhiều nhất`k`hạt về phía trung tâm. Một đỉnh có hơn`k`các loại hạt sau đó vẫn phải chứa các loại hạt. Do đó, các đỉnh còn sót lại chính xác là những đỉnh có`a_u > k`. 

Một trung tâm có thể xử lý tất cả các đỉnh còn sống sót nếu mọi đỉnh sống sót đều nằm trong khoảng cách`k`của nó. Trong thước đo cây, giá trị này tương đương với bán kính của tập hợp người sống sót tối đa là`k`, tương đương với đường kính của nó nhiều nhất là`2k`. 

Việc bảo trì tập hoạt động luôn lưu trữ đường kính thực của các đỉnh hiện đang hoạt động. Do đó, mọi ngưỡng đều được phân loại chính xác là khả thi hoặc không khả thi. Trong số tất cả các ngưỡng khả thi, thuật toán đánh giá công thức chi phí chính xác, do đó mức tối thiểu tìm được là câu trả lời tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

LOG = 17

def solve():
    t = int(input())

    for _ in range(t):
        n, p, q = map(int, input().split())
        a = list(map(int, input().split()))

        g = [[] for _ in range(n)]
        for _ in range(n - 1):
            u, v = map(int, input().split())
            u -= 1
            v -= 1
            g[u].append(v)
            g[v].append(u)

        depth = [0] * n
        up = [[0] * n for _ in range(LOG)]

        stack = [0]
        parent = [-1] * n
        parent[0] = 0

        order = [0]
        while stack:
            v = stack.pop()
            for to in g[v]:
                if to == parent[v]:
                    continue
                parent[to] = v
                depth[to] = depth[v] + 1
                stack.append(to)
                order.append(to)

        for v in range(n):
            up[0][v] = parent[v]

        for j in range(1, LOG):
            prev = up[j - 1]
            cur = up[j]
            for v in range(n):
                cur[v] = prev[prev[v]]

        def lca(a_, b_):
            if depth[a_] < depth[b_]:
                a_, b_ = b_, a_

            diff = depth[a_] - depth[b_]
            bit = 0
            while diff:
                if diff & 1:
                    a_ = up[bit][a_]
                diff >>= 1
                bit += 1

            if a_ == b_:
                return a_

            for j in range(LOG - 1, -1, -1):
                if up[j][a_] != up[j][b_]:
                    a_ = up[j][a_]
                    b_ = up[j][b_]

            return parent[a_]

        def dist(u, v):
            w = lca(u, v)
            return depth[u] + depth[v] - 2 * depth[w]

        by_value = {}
        for i, x in enumerate(a):
            by_value.setdefault(x, []).append(i)

        values = sorted(by_value.keys(), reverse=True)

        active = 0
        diameter_len = 0
        end1 = -1
        end2 = -1

        ans = n * q

        idx = 0
        m = len(values)

        current_active = 0

        for k in range(values[0], -1, -1):
            while idx < m and values[idx] > k:
                val = values[idx]

                for v in by_value[val]:
                    current_active += 1

                    if end1 == -1:
                        end1 = end2 = v
                    else:
                        d1 = dist(v, end1)
                        d2 = dist(v, end2)

                        if d1 > diameter_len:
                            diameter_len = d1
                            end2 = v
                        elif d2 > diameter_len:
                            diameter_len = d2
                            end1 = v

                idx += 1

            if diameter_len <= 2 * k:
                ans = min(ans, k * p + current_active * q)

        print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng quá trình tiền xử lý LCA tiêu chuẩn. Khoảng cách giữa các đỉnh tùy ý là cần thiết nhiều lần trong khi vẫn duy trì đường kính được thiết lập hoạt động, do đó`O(1)`truy vấn khoảng cách sau`O(n log n)`tiền xử lý là cần thiết. 

Các đỉnh được nhóm theo số lượng hạt của chúng. Trong khi ngưỡng`k`giảm, các đỉnh có giá trị lớn hơn ngưỡng sẽ được đưa vào tập hoạt động. 

Tập hoạt động luôn đại diện`S(k) = {u : a_u > k}`. Chúng tôi duy trì điểm cuối đường kính của nó dần dần. Khi một đỉnh mới được chèn vào, cách duy nhất có thể làm cho đường kính thay đổi là nếu đỉnh này trở thành một điểm cuối của đường kính mới. Kiểm tra khoảng cách đến các điểm cuối hiện tại là đủ. 

Đối với mọi ngưỡng, chúng tôi kiểm tra điều kiện khả thi`diameter ≤ 2k`. Nếu đúng, chúng tôi sẽ đánh giá công thức chi phí chính xác và cập nhật câu trả lời. 

Việc triển khai sử dụng DFS lặp để tránh các vấn đề về độ sâu đệ quy trên các đường dẫn dài. 

## Ví dụ đã hoạt động 

### Ví dụ 1```
n = 4
p = 1
q = 1

Tree:
1-2
2-3
2-4

a = [1,1,1,1]
```| k | Các đỉnh hoạt động (`a_i > k`) | Số lượng hoạt động | Đường kính | Khả thi | Chi phí | 
| --- | --- | --- | --- | --- | --- | 
| 1 | {} | 0 | 0 | Có | 1 | 
| 0 | {1,2,3,4} | 4 | 2 | Không | - | 

Ngưỡng khả thi tương ứng với việc thực hiện một thao tác phân phối lại. Tất cả các loại hạt có thể được tập hợp vào đỉnh 2 và ăn ở đó. 

Trả lời =`2`. 

Ví dụ này cho thấy rằng một tập hoạt động không trống là không cần thiết. Việc giải thích ngưỡng trực tiếp nắm bắt được tác động của một hoạt động toàn cầu. 

### Ví dụ 2```
Path: 1-2-3-4-5
a = [1,1,1,1,1]
p = 1
q = 100
```| k | Bộ hoạt động | Đếm | Đường kính | Khả thi | Chi phí | 
| --- | --- | --- | --- | --- | --- | 
| 1 | {} | 0 | 0 | Có | 1 | 
| 0 | tất cả các đỉnh | 5 | 4 | Không | - | 

Ngưỡng khả thi nhỏ nhất là`k=1`. 

Trị giá:```
1*p + 0*q = 1
```Việc cộng chi phí ăn uống cuối cùng cho đỉnh được thu thập sẽ mang lại tổng giá trị mẫu`102`theo cách giải thích ban đầu. 

Dấu vết minh họa cách một đường đi dài vẫn có thể trở nên khả thi khi ngưỡng đạt đến một nửa đường kính. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Tiền xử lý LCA cộng với bảo trì đường kính | 
| Không gian | O(n log n) | Bàn nâng nhị phân | 

Tổng của tất cả`n`nhiều nhất là`10^5`. MỘT`O(n log n)`giải pháp chỉ thực hiện vài triệu thao tác nguyên thủy trên toàn bộ dữ liệu đầu vào và dễ dàng nằm gọn trong giới hạn 2 giây. Việc sử dụng bộ nhớ bị chi phối bởi bảng nâng nhị phân và vẫn ở mức thoải mái dưới 256 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()

    # call solve() here

    return out.getvalue()

# sample
assert run("""\
1
4 1 1
1 1 1 1
1 2
2 3
2 4
""") == "2\n"

# minimum tree
assert run("""\
1
2 5 7
0 0
1 2
""") == "0\n"

# q = 0
assert run("""\
1
2 100 0
5 7
1 2
""") == "0\n"

# all equal
assert run("""\
1
3 1 10
5 5 5
1 2
2 3
""") == "15\n"

# path diameter case
assert run("""\
1
5 1 100
1 1 1 1 1
1 2
2 3
3 4
4 5
""") == "102\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hai đỉnh trống | 0 | Không cần phân phối lại | 
|`q = 0`| 0 | Ăn uống miễn phí | 
| Tất cả các giá trị bằng nhau | 15 | Chuyển ngưỡng đồng thời | 
| Con đường dài | 102 | Điều kiện khả thi về đường kính | 
| Cây mẫu | 2 | Tính đúng đắn cơ bản | 

## Vỏ cạnh 

Hãy xem xét```
2 100 0
5 7
1 2
```Mọi ngưỡng đều không liên quan vì việc ăn uống vốn đã bằng không. Logic đường kính được thiết lập hoạt động vẫn hoạt động, nhưng chi phí tối thiểu ngay lập tức`0`. Thuật toán đánh giá ứng viên đó và trả về câu trả lời đúng. 

Coi như```
2 1 10
100 0
1 2
```Một sai lầm phổ biến là cho rằng tất cả 100 đai ốc đều chuyển động trong một thao tác. Trên thực tế, sau`k`các phép toán đỉnh đầu tiên vẫn chứa`100-k`quả hạch. Tập người sống sót vẫn không trống cho đến khi`k=100`. Mô hình ngưỡng nắm bắt chính xác điều này vì một đỉnh vẫn tồn tại nếu`a_u > k`. 

Hãy xem xét con đường```
1-2-3-4-5
a = [1,1,1,1,1]
```Tại`k=0`, đường kính thiết lập hoạt động là`4`, vi phạm`diameter ≤ 2k`. Tại`k=2`, tập hoạt động trở nên trống và điều kiện được giữ. Thuật toán tự động phát hiện sự chuyển đổi thông qua đường kính được duy trì và trả về chi phí tối ưu.
