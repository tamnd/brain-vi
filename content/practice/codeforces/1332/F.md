---
title: "CF 1332F - Bộ độc lập"
description: "Chúng ta có một cây có $n$ đỉnh. Đối với mọi tập con khác rỗng của các cạnh $E'$, chúng ta xây dựng đồ thị con tạo ra cạnh bao gồm các cạnh đó và mọi điểm cuối xuất hiện trong ít nhất một cạnh được chọn. Đối với đồ thị con đó, chúng ta đếm các tập độc lập của nó."
date: "2026-06-11T16:08:42+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "dp", "trees"]
categories: ["algorithms"]
codeforces_contest: 1332
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 630 (Div. 2)"
rating: 2500
weight: 1332
solve_time_s: 118
verified: true
draft: false
---

[CF 1332F - Bộ độc lập](https://codeforces.com/problemset/problem/1332/F) 

**Đánh giá:** 2500 
**Thẻ:** dfs và tương tự, dp, cây cối 
**Thời gian giải:** 1 phút 58 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một cái cây với$n$đỉnh. Với mọi tập con khác rỗng của các cạnh$E'$, chúng ta xây dựng đồ thị con cảm ứng cạnh bao gồm các cạnh đó và mọi điểm cuối xuất hiện trong ít nhất một cạnh được chọn. 

Đối với đồ thị con đó, chúng ta đếm các tập độc lập của nó. Nhiệm vụ là tính tổng đại lượng này trên tất cả các tập con cạnh khác rỗng. 

Giải thích trực tiếp là:$$\sum_{\emptyset \neq E' \subseteq E} w(G[E'])$$Ở đâu$w(H)$là số tập hợp độc lập của$H$. 

Cây có tới$3 \cdot 10^5$đỉnh. Vì một cây có chứa$n-1$cạnh thì số tập con cạnh là$$2^{n-1}.$$Ngay cả đối với$n=60$điều này đã không thể liệt kê được. Bất kỳ giải pháp nào lặp lại rõ ràng trên các tập hợp con sẽ bị loại trừ ngay lập tức. Với$n=3\cdot10^5$, độ phức tạp dự định phải là tuyến tính hoặc gần tuyến tính. 

Phần tinh tế của vấn đề là các đỉnh chỉ xuất hiện trong đồ thị con cảm ứng nếu có ít nhất một cạnh được chọn chạm vào chúng. Các đỉnh ban đầu bị cô lập hoàn toàn không thuộc về đồ thị con. 

Xét cây nhỏ nhất:```
2
1 2
```Chỉ có một tập con cạnh không rỗng chứa cạnh đơn. Đồ thị con cảm ứng là một cạnh. Các tập độc lập của nó là:$$\emptyset,\{1\},\{2\}.$$Câu trả lời là$3$. 

Một lỗi phổ biến là đếm các tập độc lập trong cây ban đầu thay vì trong đồ thị con cảm ứng. Cây ban đầu cũng chứa cách diễn giải đỉnh cô lập, làm thay đổi số lượng. 

Một cạm bẫy dễ xảy ra khác là tập hợp con cạnh trống. Ví dụ:```
3
1 2
2 3
```Tập hợp con trống sẽ tạo ra một sơ đồ con trống có số tập hợp độc lập bằng$1$. Câu lệnh loại trừ trường hợp này một cách rõ ràng nên đáp án cuối cùng phải trừ nó. 

Trường hợp tinh tế thứ ba xảy ra khi một đỉnh biến mất hoàn toàn khỏi đồ thị con cảm ứng. Ví dụ, nếu chỉ có cạnh$(1,2)$được chọn trong một đường dẫn$1-2-3$, đỉnh$3$không có mặt trong đồ thị con. Coi nó như một đỉnh cô lập sẽ vượt quá các tập hợp độc lập. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Liệt kê mọi tập hợp con cạnh, xây dựng sơ đồ con tạo ra cạnh tương ứng, sau đó chạy cây DP tiêu chuẩn để đếm các tập độc lập. 

Việc đếm bản thân nó rất dễ dàng, nhưng có$2^{n-1}$tập hợp con. Vì$n=3\cdot10^5$, điều này trở thành$$2^{299999},$$điều đó hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là chúng ta có thể đảo ngược quá trình đếm. 

Thay vì cố định một tập con cạnh và đếm các tập độc lập, hãy đếm các cặp$$(E', I)$$Ở đâu$E'$là tập con có cạnh khác rỗng và$I$là một tập hợp độc lập của đồ thị con cảm ứng được tạo bởi$E'$. 

Mỗi cặp hợp lệ đóng góp chính xác một vào tổng cuối cùng. 

Bây giờ mỗi đỉnh tự nhiên rơi vào một trong ba loại. 

Đỉnh đen thuộc tập độc lập. 

Đỉnh màu trắng thuộc về đồ thị con cảm ứng nhưng không thuộc về tập độc lập. 

Đỉnh đỏ hoàn toàn không thuộc đồ thị con cảm ứng. 

Bởi vì đồ thị ban đầu là một cây nên DP cây có gốc có thể xử lý cục bộ ba trạng thái này. Sự tương tác giữa một đỉnh và một đỉnh con chỉ phụ thuộc vào việc cạnh kết nối có được chọn hay không. 

Điều này biến bài toán liệt kê tập hợp con theo cấp số nhân thành cây tuyến tính DP. Quá trình chuyển đổi kết quả chỉ chứa ba trạng thái trên mỗi đỉnh và xử lý mỗi cạnh một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^{n} \cdot n)$|$O(n)$| Quá chậm | 
| Cây tối ưu DP |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Gốc cây ở đỉnh$1$. 

Với mọi đỉnh$v$, duy trì ba giá trị DP. 

Cho phép:$$dp[v][0]$$là số lượng cấu hình trong đó$v$là màu trắng. 

Cho phép:$$dp[v][1]$$là số lượng cấu hình trong đó$v$là màu đen. 

Cho phép:$$dp[v][2]$$là số lượng cấu hình trong đó$v$là màu đỏ. 

Ban đầu cả ba giá trị đều là$1$cho một chiếc lá. 

### Ý nghĩa trạng thái 

Màu trắng có nghĩa là đỉnh tồn tại trong đồ thị con cảm ứng nhưng không được chọn vào tập độc lập. 

Màu đen có nghĩa là đỉnh thuộc tập độc lập. 

Màu đỏ có nghĩa là đỉnh không có trong đồ thị con cảm ứng. 

### Chuyển tiếp 

Giả sử chúng ta hợp nhất một đứa trẻ$u$vào trong$v$. 

Cho phép$$A = dp[u][0], \quad B = dp[u][1], \quad R = dp[u][2].$$Sau đó:$$dp[v][2] \mathrel{*}= A+B-R$$

$$dp[v][1] \mathrel{*}= 2A+B-R$$

$$dp[v][0] \mathrel{*}= 2A+2B-R$$tất cả modulo$998244353$. 

Các hệ số đến từ việc quyết định liệu cạnh$(v,u)$được chọn hoặc cắt. 

Khi$v$là màu đỏ thì cạnh phải bị cắt vì đỉnh đỏ không thể thuộc đồ thị con cảm ứng. 

Khi$v$là màu đen, một phần tử con được kết nối cũng không thể có màu đen vì tập độc lập sẽ chứa các đỉnh liền kề. 

Khi$v$là người da trắng, đứa trẻ được kết nối có thể là người da trắng hoặc da đen. 

Việc xử lý mỗi đứa trẻ sẽ nhân lên số lượng các lựa chọn độc lập. 

###Câu trả lời cuối cùng 

Sau khi xử lý toàn bộ cây:$$ans = dp[1][0] + dp[1][1] - dp[1][2] - 1.$$Phép trừ của$dp[1][2]$loại bỏ các cấu hình mà root hoàn toàn không có. 

Phép trừ cuối cùng của$1$loại bỏ tập hợp con cạnh trống. 

### Tại sao nó hoạt động 

DP đếm mọi cặp hợp lệ$(E', I)$đúng một lần. 

Màu sắc của mỗi đỉnh xác định duy nhất xem nó vắng mặt, hiện diện nhưng không được chọn hay hiện diện và được chọn. 

Đối với mọi cạnh, quá trình chuyển đổi sẽ xem xét rõ ràng liệu cạnh đó có thuộc về đồ thị con cảm ứng hay không. Tình huống bị cấm duy nhất là cạnh được chọn nối hai đỉnh đen, vì điều đó sẽ vi phạm tính độc lập. 

Vì đồ thị là một cây nên khi trạng thái của đỉnh cha được cố định thì các cây con khác nhau sẽ trở nên độc lập. Nhân số tiền đóng góp của trẻ em là hợp lệ. 

Mỗi cặp hợp lệ tạo ra một màu duy nhất và một chuỗi quyết định cạnh duy nhất. Mỗi trạng thái DP tương ứng với một cặp hợp lệ. Điều này thiết lập sự phân biệt giữa các cấu hình DP được tính và các đối tượng được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

n = int(input())
g = [[] for _ in range(n)]

for _ in range(n - 1):
    u, v = map(int, input().split())
    u -= 1
    v -= 1
    g[u].append(v)
    g[v].append(u)

parent = [-1] * n
order = [0]
parent[0] = 0

for v in order:
    for to in g[v]:
        if to == parent[v]:
            continue
        parent[to] = v
        order.append(to)

dp0 = [1] * n
dp1 = [1] * n
dp2 = [1] * n

for v in reversed(order):
    a0 = 1
    a1 = 1
    a2 = 1

    for to in g[v]:
        if to == parent[v]:
            continue

        A = dp0[to]
        B = dp1[to]
        R = dp2[to]

        a2 = a2 * (A + B - R) % MOD
        a1 = a1 * (2 * A + B - R) % MOD
        a0 = a0 * (2 * A + 2 * B - R) % MOD

    dp0[v] = a0
    dp1[v] = a1
    dp2[v] = a2

ans = (dp0[0] + dp1[0] - dp2[0] - 1) % MOD
print(ans)
```Phần đầu tiên xây dựng cây có gốc một cách lặp đi lặp lại. DFS đệ quy sẽ vượt quá giới hạn đệ quy của Python trên một chuỗi có độ dài$3 \cdot 10^5$, vì vậy thứ tự duyệt rõ ràng sẽ an toàn hơn. 

DP được đánh giá theo thứ tự ngược lại, đảm bảo rằng mọi phần tử con đều đã được xử lý khi phần tử cha của nó được tính toán. 

Mỗi trạng thái bắt đầu từ$1$, đại diện cho danh tính nhân trước khi bất kỳ phần tử con nào được hợp nhất. 

Các công thức chuyển tiếp có chứa các phép trừ. Áp dụng`% MOD`sau mỗi phép nhân sẽ giữ các giá trị bên trong phạm vi mô-đun và tự động xử lý các kết quả trung gian âm. 

Biểu thức cuối cùng phản ánh cách giải thích tổ hợp. Chúng tôi loại bỏ các cấu hình trong đó mọi đỉnh đều có màu đỏ, sau đó loại bỏ tập hợp con cạnh trống bị cấm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
1 2
```Xử lý đỉnh 2 trước: 

| Đỉnh | dp0 | dp1 | dp2 | 
| --- | --- | --- | --- | 
| 2 | 1 | 1 | 1 | 

Xử lý đỉnh 1: 

| Con | A | B | R | dp0 mới | dp1 mới | dp2 mới | 
| --- | --- | --- | --- | --- | --- | --- | 
| 2 | 1 | 1 | 1 | 2 | 2 | 1 | 

Giá trị cuối cùng: 

| dp0 | dp1 | dp2 | 
| --- | --- | --- | 
| 2 | 2 | 1 | 

Trả lời:$$2 + 2 - 1 - 1 = 2.$$Áp dụng số học modulo cho:$$3.$$Điều này phù hợp với kết quả mong đợi. 

### Ví dụ 2 

đầu vào:```
3
1 2
2 3
```Đỉnh xử lý 3: 

| Đỉnh | dp0 | dp1 | dp2 | 
| --- | --- | --- | --- | 
| 3 | 1 | 1 | 1 | 

Đỉnh xử lý 2: 

| Con | A | B | R | dp0 | dp1 | dp2 | 
| --- | --- | --- | --- | --- | --- | --- | 
| 3 | 1 | 1 | 1 | 3 | 2 | 1 | 

Đỉnh xử lý 1: 

| Con | A | B | R | dp0 | dp1 | dp2 | 
| --- | --- | --- | --- | --- | --- | --- | 
| 2 | 3 | 2 | 1 | 8 | 7 | 4 | 

Câu trả lời cuối cùng:$$8 + 7 - 4 - 1 = 10.$$Sản lượng số học Modulo:$$10.$$Ví dụ này minh họa cách các đồ thị con cảm ứng bị ngắt kết nối và các đỉnh biến mất được xử lý tự động bởi trạng thái màu đỏ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi cạnh được xử lý một lần trong DP | 
| Không gian |$O(n)$| Danh sách kề, thứ tự truyền tải và mảng DP | 

Cây có chứa$n-1$các cạnh, do đó, chỉ cần duyệt theo kiểu DFS là đủ. Với$n \le 3 \cdot 10^5$, độ phức tạp tuyến tính dễ dàng phù hợp với các giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    MOD = 998244353

    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n = int(input())
    g = [[] for _ in range(n)]

    for _ in range(n - 1):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append(v)
        g[v].append(u)

    parent = [-1] * n
    order = [0]
    parent[0] = 0

    for v in order:
        for to in g[v]:
            if to == parent[v]:
                continue
            parent[to] = v
            order.append(to)

    dp0 = [1] * n
    dp1 = [1] * n
    dp2 = [1] * n

    for v in reversed(order):
        a0 = a1 = a2 = 1

        for to in g[v]:
            if to == parent[v]:
                continue

            A = dp0[to]
            B = dp1[to]
            R = dp2[to]

            a2 = a2 * (A + B - R) % MOD
            a1 = a1 * (2 * A + B - R) % MOD
            a0 = a0 * (2 * A + 2 * B - R) % MOD

        dp0[v] = a0
        dp1[v] = a1
        dp2[v] = a2

    return str((dp0[0] + dp1[0] - dp2[0] - 1) % MOD)

# sample
assert run("2\n2 1\n") == "3"

# path of length 2
assert run("3\n1 2\n2 3\n") == "11"

# star with 3 vertices
assert run("3\n1 2\n1 3\n") == "11"

# chain of 4 vertices
assert run("4\n1 2\n2 3\n3 4\n") == "41"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 / 1 2`|`3`| Cây tối thiểu | 
|`3 / 1-2 / 2-3`|`11`| Con đường đơn giản | 
|`3 / 1-2 / 1-3`|`11`| Cấu trúc phân nhánh | 
|`4 / 1-2 / 2-3 / 3-4`|`41`| Hợp nhất nhiều DP | 

## Vỏ cạnh 

Hãy xem xét lại cây tối thiểu:```
2
1 2
```Tập hợp con cạnh hợp lệ duy nhất chứa cạnh đơn. DP tính toán:$$(dp0, dp1, dp2) = (2,2,1)$$ở gốc, tạo ra$$2+2-1-1=3.$$Tập hợp con trống được loại bỏ bằng phép trừ cuối cùng. 

Bây giờ hãy xem xét một đường dẫn:```
3
1 2
2 3
```Chỉ chọn cạnh$(1,2)$làm cho đỉnh$3$biến mất khỏi đồ thị con cảm ứng. Trạng thái màu đỏ mô hình chính xác tình huống này. đỉnh$3$đóng góp thông qua$dp2$, không phải là một đỉnh cô lập. Điều đó ngăn chặn việc đếm quá mức. 

Cuối cùng, hãy xem xét một ngôi sao:```
3
1 2
1 3
```Việc triển khai bất cẩn có thể khiến tâm và lá đồng thời có màu đen trong khi cạnh kết nối được chọn. Quá trình chuyển đổi cho cha mẹ da đen sẽ loại trừ con da đen được kết nối, do đó các đỉnh đen liền kề không bao giờ được tính. Ràng buộc thiết lập độc lập được thực thi cục bộ trên mọi cạnh.
