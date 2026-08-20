---
title: "CF 102174L - \u65c5\u884c\u7684\u610f\u4e49"
description: "Các thành phố tạo thành một biểu đồ tuần hoàn có hướng và cuộc hành trình bắt đầu từ thành phố 1. Bất cứ khi nào du khách đến một thành phố, họ sẽ dành một ngày để tham quan trước khi đưa ra bất kỳ quyết định nào. Giả sử thành phố hiện tại có (d0) rìa đường sắt đi."
date: "2026-08-19T07:12:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102174
codeforces_index: "L"
codeforces_contest_name: "The 14-th BIT Campus Programming Contest"
rating: 0
weight: 102174
solve_time_s: 111
verified: true
draft: false
---

[CF 102174L - \u65c5\u884c\u7684\u610f\u4e49](https://codeforces.com/problemset/problem/102174/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 51 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Các thành phố tạo thành một biểu đồ tuần hoàn có hướng và cuộc hành trình bắt đầu từ thành phố 1. Bất cứ khi nào du khách đến một thành phố, họ sẽ dành một ngày để tham quan trước khi đưa ra bất kỳ quyết định nào. 

Giả sử thành phố hiện tại có (d>0) rìa đường sắt đi ra. Vào ngày hôm sau, có (d+1) các lựa chọn có khả năng như nhau: bắt bất kỳ một trong (d) chuyến tàu đi hoặc ở lại thêm một ngày tham quan. Nếu ở lại thì không được ở lại nữa nên ngày hôm sau phải chọn một trong (d) chuyến tàu đi thống nhất. Một thành phố không có ranh giới đi ra là một thành phố cuối, nơi họ dành tổng cộng hai ngày và sau đó cuộc hành trình kết thúc. 

Đầu vào chứa (T) trường hợp thử nghiệm độc lập. Mỗi trường hợp thử nghiệm cung cấp (n) thành phố và (m) các cạnh đường sắt được định hướng. Biểu đồ được đảm bảo là DAG, nhưng việc đánh số thành phố không được đảm bảo là thứ tự tôpô. Chúng tôi cần tổng số ngày dự kiến ​​bắt đầu từ thành phố 1, được biểu thị theo modulo (998244353). 

Các giới hạn (n,m\le 10^5) là tín hiệu thuật toán chính. Thuật toán bậc hai có thể thực hiện khoảng (10^{10}) thao tác trên một trường hợp thử nghiệm lớn, vượt xa giới hạn một giây. Ngay cả một thuật toán phụ thuộc vào việc liệt kê tất cả các tuyến đường có thể cũng là vô vọng vì một DAG có thể chứa nhiều đường dẫn riêng biệt theo cấp số nhân. Chúng ta cần một giải pháp mà công của nó về cơ bản tỷ lệ thuận với kích thước đồ thị, cụ thể là (O(n+m)). 

Có một số trường hợp đặc biệt có thể khiến việc triển khai bất cẩn trở nên sai lầm. Đồ thị nhỏ nhất là```
1
1 0
```Không có ranh giới đi ra ngoài, nhưng du khách vẫn dành hai ngày ở thành phố duy nhất. Câu trả lời đúng là`2`. Một triển khai coi mức độ bằng 0 là chi phí trong tương lai bằng 0 và quên mất ngày tham quan thứ hai sẽ xuất ra`1`. 

Một thành phố có một cạnh đi ra cũng cần xử lý đặc biệt trong tính toán xác suất. Vì```
1
2 1
1 2
```thành phố 2 đóng góp hai ngày. Từ thành phố 1, có xác suất (1/2) bắt tàu ngay và xác suất (1/2) ở lại thêm một ngày trước khi lên tàu. Phần đóng góp dự kiến ​​trước khi đến thành phố 2 là (1+3/2=5/2), do đó tổng số là (9/2), có biểu diễn mô đun là`499122181`. Thay thế xác suất ở lại bằng (1/d), thay vì (1/(d+1)), sẽ cho kết quả sai. 

Các số thành phố cũng không thể được giả định để tạo thành một trật tự tôpô. Ví dụ,```
1
3 2
1 3
3 2
```yêu cầu thành phố 2 phải được xử lý trước thành phố 3 và thành phố 3 trước thành phố 1. Một DP chỉ lặp từ (n) xuống (1) sẽ hoạt động đối với một số biểu đồ nhưng không có đảm bảo tính chính xác ở đây. 

Cuối cùng, những thành phố không thể tiếp cận được không ảnh hưởng đến câu trả lời. Vì```
1
3 0
```du khách bắt đầu ở thành phố 1 và kết thúc ở đó sau hai ngày. Thành phố 2 và 3 không liên quan. Việc tính toán giá trị DP cho mọi thành phố vẫn ổn vì câu trả lời chỉ sử dụng giá trị của thành phố 1. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ mô phỏng mọi hành trình có thể xảy ra. Tại một thành phố có (d) rìa đi, chúng tôi sẽ phân nhánh thành các lựa chọn tàu ngay lập tức và khả năng ở lại, và sau khi ở lại, chúng tôi sẽ lại phân nhánh qua các rìa đi. Đối với mỗi lộ trình hoàn chỉnh, chúng ta có thể tính toán thời lượng và xác suất của nó, sau đó tính tổng các khoản đóng góp. 

Điều này đúng vì kỳ vọng là tổng trọng số xác suất trên tất cả các hành trình có thể xảy ra. Vấn đề là số lượng hành trình. Một DAG chứa mọi cạnh (i\to j) cho (i<j) có (2^{n-2}) đường đi riêng biệt từ thành phố 1 đến thành phố (n), bởi vì mọi tập hợp con của các thành phố trung gian có thể xuất hiện theo thứ tự tăng dần. Các quyết định ở lại thêm các nhánh bổ sung vào không gian trạng thái. Với (n=10^5), thậm chí (2^{99998}) vượt xa mọi thứ có thể liệt kê được. 

Brute-force hoạt động vì mỗi hành trình có thể được đánh giá độc lập nhưng không thành công vì nhiều hành trình khác nhau liên tục đi qua cùng một thành phố. Quan sát quan trọng là khi chúng ta đến một thành phố, khoảng thời gian còn lại dự kiến ​​chỉ phụ thuộc vào thành phố đó chứ không phụ thuộc vào lịch sử đã sử dụng để đến đó. Chúng ta có thể lưu trữ giá trị mong đợi đó ở trạng thái DP. 

Có một sự đơn giản hóa hữu ích hơn trong xác suất chuyển đổi. Nếu một thành phố có (d>0) cạnh đi, mỗi lựa chọn tàu ngay lập tức có xác suất (1/(d+1)). Việc giữ nguyên cũng có xác suất (1/(d+1)) và sau khi giữ nguyên, mỗi cạnh đi ra được chọn với xác suất (1/d). Do đó tổng xác suất để cuối cùng đạt được bất kỳ cạnh đi cụ thể nào là 

[ 
\frac{1}{d+1}+\frac{1}{d+1}\cdot\frac{1}{d} 
=\frac{1}{d}. 
] 

Vì vậy, sau khi tính đến số ngày tham quan có thể có thêm, thành phố tiếp theo chỉ đơn giản là được phân bố đồng đều giữa các nước láng giềng sắp rời đi. 

Gọi (f_u) là số ngày dự kiến ​​còn lại khi du khách vừa đến thành phố (u), trước khi tham quan ở đó. Với (d=0), 

[ 
f_u=2. 
] 

Với (d>0), ngày tham quan đầu tiên tốn một ngày. Sau đó, đi tàu tốn một ngày, còn ở lại lần đầu tốn thêm một ngày trước khi đi tàu. Vì việc lưu trú có xác suất (1/(d+1)), chi phí dự kiến từ thời điểm đó cho đến khi đến thành phố tiếp theo là 

[ 
1+\left(1+\frac{1}{d+1}\right) 
=2+\frac{1}{d+1}. 
] 

Thành phố tiếp theo được phân bố đồng đều giữa (d) những người hàng xóm sắp đi, đưa ra 

2+\frac{1}{d+1} 
+ 
\frac{1}{d}\sum_{u\to v}f_v. 
] 

Bởi vì đồ thị có tính chất không tuần hoàn nên mọi (f_u) chỉ phụ thuộc vào các giá trị xa hơn dọc theo DAG. Thứ tự tôpô cho phép chúng ta tính toán tất cả các giá trị theo thứ tự ngược lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(2^n)) trong trường hợp xấu nhất | (O(n)) độ sâu đệ quy | Quá chậm | 
| Tối ưu | (O(n+m)) | (O(n+m)) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Đọc biểu đồ có hướng và ghi lại các vùng lân cận của mỗi thành phố. Đồng thời, tính toán mức độ của mỗi thành phố. Các giá trị độ cho phép chúng ta có được thứ tự tôpô mà không cần dựa vào nhãn thành phố. 
2. Chạy thuật toán sắp xếp tôpô của Kahn. Đặt mọi thành phố có độ bằng 0 vào hàng đợi, liên tục loại bỏ một thành phố và giảm độ bằng của các thành phố lân cận. Vì biểu đồ được đảm bảo là DAG nên cuối cùng mọi thành phố sẽ nhập thứ tự. 
3. Tính toán trước các nghịch đảo mô-đun cho tất cả các số nguyên lên đến (n+1). Đối với một thành phố có cấp độ ngoài (d), phép truy hồi cần cả (1/d) và (1/(d+1)). Vì mọi độ lớn nhất là (10^5), nên tất cả các mẫu số này đều nhỏ hơn mô đun (998244353), do đó tồn tại nghịch đảo của chúng. 
4. Xử lý thứ tự tôpô theo chiều ngược lại. Khi thành phố (u) được xử lý, mọi hàng xóm đi ra (v) đều đã được xử lý, do đó tất cả các giá trị (f_v) đều được biết. 
5. Nếu thành phố (u) không có cạnh đi ra, đặt (f_u=2). Du khách dành một ngày để tham quan và thêm một ngày nữa vì không có tàu để đi. 
6. Ngược lại, coi (d) là bậc lớn hơn của (u). Tính toán 

[ 
f_u= 
2+\frac{1}{d+1} 
+\frac{1}{d}\sum_{u\to v}f_v 
\pmod {998244353}. 
] 

Số hạng (2+1/(d+1)) chứa số ngày ở thành phố hiện tại và thời gian chuyển tiếp sang thành phố tiếp theo. Giá trị trung bình của các giá trị kế tiếp cho biết chuyến tàu đi cuối cùng sẽ được thực hiện. 

1. Đầu ra (f_1), vì thành phố 1 là thành phố xuất phát. 

Điều bất biến là khi một thành phố được xử lý theo thứ tự tôpô ngược, giá trị DP của nó được tính từ các giá trị chính xác cho mọi thành phố có thể tiếp cận được bằng một cạnh đi ra. Bản thân sự tái diễn là quy luật kỳ vọng hoàn toàn vào mọi quyết định có thể xảy ra tại thành phố đó. Vì mọi cạnh đều tiến lên theo thứ tự tôpô nên không có giá trị nào được sử dụng trước khi nó được hoàn thiện. Do đó, mọi (f_u) và đặc biệt là (f_1) đều bằng thời gian di chuyển còn lại dự kiến ​​thực sự. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353
MAXN = 100000 + 2

# inv[i] = i^(-1) mod MOD
inv = [0] * (MAXN + 1)
inv[1] = 1
for i in range(2, MAXN + 1):
    inv[i] = MOD - (MOD // i) * inv[MOD % i] % MOD

def solve():
    T = int(input())
    answers = []

    for _ in range(T):
        n, m = map(int, input().split())

        graph = [[] for _ in range(n)]
        indeg = [0] * n

        for _ in range(m):
            u, v = map(int, input().split())
            u -= 1
            v -= 1
            graph[u].append(v)
            indeg[v] += 1

        # Kahn's algorithm for a topological ordering.
        queue = [u for u in range(n) if indeg[u] == 0]
        head = 0
        topo = []

        while head < len(queue):
            u = queue[head]
            head += 1
            topo.append(u)

            for v in graph[u]:
                indeg[v] -= 1
                if indeg[v] == 0:
                    queue.append(v)

        dp = [0] * n

        # Every successor appears earlier in reverse topological order.
        for u in reversed(topo):
            d = len(graph[u])

            if d == 0:
                dp[u] = 2
                continue

            s = 0
            for v in graph[u]:
                s += dp[v]
            s %= MOD

            dp[u] = (
                2
                + inv[d + 1]
                + s * inv[d]
            ) % MOD

        answers.append(str(dp[0]))

    sys.stdout.write("\n".join(answers))

if __name__ == "__main__":
    solve()
```Danh sách kề lưu trữ chính xác các lựa chọn đường sắt đi theo yêu cầu lặp lại. Nó cũng cho phép cả việc sắp xếp tôpô và DP chỉ đi qua mỗi cạnh một số lần không đổi. 

Sắp xếp cấu trúc liên kết sử dụng một danh sách cùng với một con trỏ đầu thay vì liên tục xóa khỏi phía trước danh sách Python. Việc loại bỏ khỏi phía trước sẽ tốn (O(n)) cho mỗi thao tác, trong khi tiến lên`head`là thời gian không đổi. 

Mảng nghịch đảo được tính một lần cho tất cả các trường hợp thử nghiệm. Sự tái diễn của nghịch đảo mô-đun là 

-\left\lfloor\frac{MOD}{i}\right\rfloor 
\operatorname{inv}(MOD\bmod i) 
\pmod{MOD}. 
] 

Điều này tránh việc thực hiện lũy thừa mô-đun cho mọi thành phố. Đang gọi`pow(x, MOD-2, MOD)`riêng cho tối đa (10^5) độ sẽ thêm hệ số logarit không cần thiết. 

Nhánh không độ phải được xử lý trước khi sử dụng`inv[d]`, vì (1/0) không được xác định. Quan trọng hơn, giá trị mong đợi của nó chính xác là 2 chứ không phải 1. 

Tất cả số học được giảm modulo (998244353). Các số nguyên Python không bị tràn, nhưng việc giảm tổng lân cận và phép truy toán cuối cùng sẽ giữ cho các giá trị trung gian ở mức nhỏ và phù hợp với tính toán mô-đun toán học. 

Đồ thị không được coi là được sắp xếp theo số đỉnh. Sự di chuyển ngược của`topo`là điều đảm bảo rằng mọi`dp[v]`được sử dụng bởi`dp[u]`đã có sẵn. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên, đầu vào là```
1
1 0
```Chỉ có một thành phố và không có đường sắt. 

| Thành phố | Bằng cấp cao hơn | Hàng xóm Sum | DP | 
| --- | --- | --- | --- | 
| 1 | 0 | 0 | 2 | 

Quy tắc bậc 0 ngay lập tức cho kết quả (f_1=2). Vì thế câu trả lời là`2`. 

Đối với mẫu thứ hai, đầu vào là```
1
2 1
1 2
```Thứ tự tôpô là (1,2). Xử lý ngược lại mang lại cho thành phố 2 đầu tiên. 

| Thành phố | Bằng cấp cao hơn | Hàng xóm Sum | DP | 
| --- | --- | --- | --- | 
| 2 | 0 | 0 | 2 | 
| 1 | 1 | 2 | (2+\frac12+2=\frac92) | 

Đối với thành phố 1, (d=1), do đó, thời gian lưu trú thêm sẽ đóng góp (1/(1+1)=1/2). Người kế nhiệm duy nhất có giá trị kỳ vọng là 2. Như vậy 

[ 
f_1=2+\frac12+2=\frac92. 
] 

Modulo (998244353), 

[ 
\frac92=9\cdot 2^{-1} 
=499122181, 
] 

phù hợp với đầu ra mẫu. 

Dấu vết phong phú hơn một chút sẽ hữu ích cho việc xem số hạng trung bình. Coi như```
1
4 4
1 2
1 3
2 4
3 4
```Thành phố 4 là nhà ga, trong khi thành phố 2 và 3 đều dẫn thẳng đến thành phố 4. 

| Thành phố | Bằng cấp cao hơn | Hàng xóm Sum | DP | 
| --- | --- | --- | --- | 
| 4 | 0 | 0 | 2 | 
| 3 | 1 | 2 | (2/9) | 
| 2 | 1 | 2 | (2/9) | 
| 1 | 2 | 9 | (27/4) | 

Tại thành phố 1 có hai cạnh đi ra nên kỳ vọng lưu trú thêm là (1/3) và thành phố tiếp theo được phân bố đồng đều giữa thành phố 2 và 3. Cả hai đều có giá trị (9/2), do đó 

[ 
f_1=2+\frac13+\frac{1}{2}\left(\frac92+\frac92\right) 
=\frac{27}{4}. 
] 

Ví dụ này xác nhận rằng sự truy hồi phụ thuộc vào kỳ vọng trung bình kế tiếp hơn là vào bất kỳ đường dẫn cụ thể nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n+m)) | Phân loại cấu trúc liên kết sẽ truy cập mọi thành phố và rìa một lần và DP sẽ quét mọi cạnh một lần nữa. | 
| Không gian | (O(n+m)) | Danh sách kề chứa (m) cạnh, trong khi biểu đồ và mảng DP chứa (O(n)) giá trị bổ sung. | 

Với (n,m\le 10^5), thuật toán chỉ thực hiện một vài lần tuyến tính trên biểu đồ đầu vào. Qua nhiều trường hợp thử nghiệm, lý do tương tự được áp dụng độc lập cho từng trường hợp. Việc sử dụng bộ nhớ cũng tuyến tính theo kích thước biểu đồ và vừa vặn thoải mái trong giới hạn bộ nhớ đã nêu. 

## Trường hợp thử nghiệm```python
# The production solution above is organized around solve(), which reads
# from sys.stdin. For isolated tests, this helper temporarily replaces stdin.

import sys
import io
from contextlib import redirect_stdout

MOD = 998244353

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    data = sys.stdin.read().split()
    it = iter(data)

    T = int(next(it))
    out = []

    for _ in range(T):
        n = int(next(it))
        m = int(next(it))

        graph = [[] for _ in range(n)]
        indeg = [0] * n

        for _ in range(m):
            u = int(next(it)) - 1
            v = int(next(it)) - 1
            graph[u].append(v)
            indeg[v] += 1

        queue = [i for i in range(n) if indeg[i] == 0]
        head = 0
        topo = []

        while head < len(queue):
            u = queue[head]
            head += 1
            topo.append(u)

            for v in graph[u]:
                indeg[v] -= 1
                if indeg[v] == 0:
                    queue.append(v)

        max_degree = max((len(x) for x in graph), default=0)

        inv = [0] * (max_degree + 2)
        if max_degree + 1 >= 1:
            inv[1] = 1

        for i in range(2, max_degree + 2):
            inv[i] = MOD - (MOD // i) * inv[MOD % i] % MOD

        dp = [0] * n

        for u in reversed(topo):
            d = len(graph[u])

            if d == 0:
                dp[u] = 2
            else:
                s = sum(dp[v] for v in graph[u]) % MOD
                dp[u] = (
                    2 + inv[d + 1] + s * inv[d]
                ) % MOD

        out.append(str(dp[0]))

    sys.stdin = old_stdin
    return "\n".join(out)

# Provided sample 1
assert run("""\
1
1 0
""") == "2", "sample 1"

# Provided sample 2
assert run("""\
1
2 1
1 2
""") == "499122181", "sample 2"

# Minimum-size graph and unreachable cities.
assert run("""\
1
3 0
""") == "2", "only city 1 is reachable"

# Two branches merging into one terminal city.
assert run("""\
1
4 4
1 2
1 3
2 4
3 4
""") == "249561095", "diamond DAG"

# Star graph: f[1] = 2 + 1/4 + 2 = 17/4.
assert run("""\
1
4 3
1 2
1 3
1 4
""") == "748683269", "outdegree three"

# Maximum-size chain: 100000 cities and 99999 edges.
# There are 99999 nonterminal cities contributing 5/2 each,
# followed by one terminal city contributing 2.
n = 100000
edges = "\n".join(f"{i} {i + 1}" for i in range(1, n))
max_case = f"{n} {n - 1}\n{edges}\n"

assert run("1\n" + max_case) == "499372177", "maximum-size chain"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 0`|`2`| Biểu đồ tối thiểu và xử lý thành phố đầu cuối | 
|`3 0`|`2`| Thành phố không thể tiếp cận không được ảnh hưởng đến thành phố 1 | 
| Kim Cương DAG |`249561095`| Phân nhánh, sáp nhập và tính trung bình kế thừa | 
| Sao ba cạnh |`748683269`| Đúng (1/(d+1)) thời hạn lưu trú cho (d=3) | 
| Chuỗi 100000 thành phố |`499372177`| Kích thước đầu vào tối đa và chuyển tiếp DP lặp đi lặp lại | 

## Vỏ cạnh 

Đối với đồ thị chỉ bao gồm thành phố 1,```
1
1 0
```thứ tự tôpô chứa thành phố 1 và DP ngược ngay lập tức nhìn thấy mức độ bằng 0. Nó gán`dp[0] = 2`, vì vậy đầu ra là`2`. Không có sự phân chia theo mức độ bên ngoài vì trường hợp đầu cuối được xử lý riêng. 

Đối với một cạnh đường sắt duy nhất,```
1
2 1
1 2
```thứ tự tôpô ngược xử lý thành phố 2 trước và gán cho nó giá trị 2. Thành phố 1 có bậc 1, do đó sự tái diễn của nó là 

[ 
2+\frac12+\frac{2}{1}=\frac92. 
] 

Kết quả mô-đun là`499122181`. Điều này mắc phải sai lầm phổ biến khi coi xác suất đi tàu là 1 thay vì tính thêm ngày tham quan có thể có. 

Đối với biểu đồ có nhãn không được sắp xếp theo cấu trúc liên kết,```
1
3 2
1 3
3 2
```thứ tự tôpô đúng là (1,3,2). Xử lý ngược mang lại giá trị thành phố 2 là 2, sau đó là giá trị thành phố 3 (9/2), sau đó là giá trị thành phố 1 (27/4). Đầu ra mô-đun là`249561095`. Vòng lặp chỉ dựa trên số thành phố giảm dần hoặc tăng dần sẽ không cung cấp thứ tự phụ thuộc cần thiết nói chung. 

Đối với một thành phố có nhiều rìa hướng ra ngoài,```
1
4 3
1 2
1 3
1 4
```thành phố 2, 3 và 4 đều là cuối và có giá trị 2. Thành phố 1 có (d=3), vì vậy 

# 2+\frac14+\frac{2+2+2}{3} 

\frac{17}{4}. 
] 

Kết quả mô-đun là`748683269`. Điều này xác minh cả xác suất tồn tại (1/(d+1)) và phân bố đồng đều (1/d) trên cạnh đi ra cuối cùng. 

Chuỗi kích thước tối đa chứa 100000 thành phố,```
100000 99999
1 2
2 3
...
99999 100000
```với các cạnh liên tiếp rõ ràng. Mỗi thành phố không có ga cuối đều có cấp 1, vì vậy mỗi thành phố đóng góp (5/2) ngày trước thành phố tiếp theo, trong khi thành phố 100000 đóng góp 2. Kỳ vọng chính xác là 

\frac{499999}{2}, 
] 

đó là`499372177`modulo (998244353). DP tuyến tính xử lý tất cả 100000 trạng thái mà không đệ quy, tránh cả các vấn đề về độ sâu đệ quy và mọi phép liệt kê đường dẫn hàm mũ.
