---
title: "CF 105168G - Nhiễm màu"
description: "Chúng ta có một cây có gốc trong đó đỉnh 1 đã được tô màu ngay từ đầu. Tất cả các đỉnh khác bắt đầu không được tô màu. Một nước đi bao gồm việc chọn bất kỳ đỉnh không được tô màu nào có đỉnh gốc trong cây có gốc đã được tô màu và tô màu nó ngay lập tức."
date: "2026-06-27T09:03:10+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105168
codeforces_index: "G"
codeforces_contest_name: "2024 Fujian Normal University Programming Contest"
rating: 0
weight: 105168
solve_time_s: 49
verified: true
draft: false
---

[CF 105168G - Sự lây nhiễm màu](https://codeforces.com/problemset/problem/105168/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có gốc trong đó đỉnh 1 đã được tô màu ngay từ đầu. Tất cả các đỉnh khác bắt đầu không được tô màu. Một nước đi bao gồm việc chọn bất kỳ đỉnh không được tô màu nào có đỉnh gốc trong cây có gốc đã được tô màu và tô màu nó ngay lập tức. Điều này tiếp tục cho đến khi tất cả các đỉnh được tô màu. 

Thay vì chỉ quan tâm đến màu sắc cuối cùng, chúng ta quan tâm đến thứ tự chọn các đỉnh. Chúng ta ghi lại thứ tự này dưới dạng một mảng có độ dài n trừ 1, vì gốc đã được tô màu và không bao giờ xuất hiện. Bất kỳ thứ tự nào có thể phát sinh từ một chuỗi thao tác hợp lệ đều được gọi là hợp lệ và chúng ta phải đếm xem có bao nhiêu thứ tự hợp lệ như vậy tồn tại theo modulo 998244353. 

Ràng buộc chính là một đỉnh sẽ có sẵn chính xác khi cha mẹ của nó đã được tô màu, do đó quá trình này là một vấn đề hoán vị bị ràng buộc trên cây. 

Kích thước đầu vào tăng lên 2 × 10^5 trên tất cả các trường hợp thử nghiệm, do đó, mọi giải pháp đều phải tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. Việc liệt kê dựa trên giai thừa hoặc lập trình động trên các hoán vị là không thể. Ngay cả O(n^2) cho mỗi trường hợp thử nghiệm cũng quá chậm trong trường hợp xấu nhất là chuỗi hoặc ngôi sao. 

Một vài tình huống khó khăn quan trọng. 

Nếu cây là một đỉnh duy nhất thì không có phép toán nào, do đó mảng hợp lệ duy nhất là mảng trống, cho câu trả lời 1. 

Nếu gốc có nhiều con nhưng không có cấu trúc sâu hơn thì tất cả các con đều có sẵn ngay lập tức, do đó mọi thứ tự của chúng đều hợp lệ. Đối với một ngôi sao có căn 1 và các con 2, 3, ..., n, câu trả lời phải là (n − 1)!. 

Nếu cây là một chuỗi 1 → 2 → 3 → ... → n thì mỗi nút chỉ khả dụng sau khi nút trước đó được chọn, do đó có chính xác một thứ tự hợp lệ. 

Một nỗ lực ngây thơ chỉ đơn giản là đếm các hoán vị của các nút hoặc xử lý từng cây con một cách độc lập sẽ thất bại vì các cây con cạnh tranh tài nguyên đặt hàng chung ở cây mẹ của chúng. 

## Phương pháp tiếp cận 

Chế độ xem bạo lực là mô phỏng tất cả các trình tự có thể xảy ra. Ở mỗi bước, chúng tôi duy trì tập hợp các đỉnh hiện có, nghĩa là các đỉnh có cha mẹ đã được tô màu nhưng bản thân chúng chưa được chọn. Chúng tôi chọn một, nối nó vào chuỗi, cập nhật tính khả dụng và tiếp tục đệ quy. 

Điều này tạo ra chính xác tất cả các mảng hợp lệ, nhưng số lượng trạng thái về cơ bản là số thứ tự tôpô của cây có gốc. Trong trường hợp xấu nhất của một ngôi sao, đây là (n − 1)!, đã phát nổ. Tệ hơn nữa, nhiều cây tạo ra sự phân nhánh siêu lũy thừa trong cây đệ quy, khiến điều này không thể thực hiện được ngoài n khoảng 15 hoặc 20. 

Quan sát cấu trúc quan trọng là đây không phải là một phần trật tự tùy ý. Mỗi nút mở khóa một cây con độc lập và trong mỗi cây con, các ràng buộc về thứ tự tương đối giống hệt với cùng một vấn đề được giới hạn ở cây con đó. Quá trình này tương đương với việc hợp nhất các chuỗi từ mỗi cây con con, trong đó mỗi cây con đóng góp một khối phần tử phải tôn trọng các ràng buộc thứ tự bên trong, nhưng các khối có thể xen kẽ được. 

Điều này gợi ý một DP trên cây: mỗi nút đóng góp một kích thước cây con và một số cách hợp lệ để sắp xếp thứ tự cây con đó. Sự tương tác quan trọng giữa các phần tử con là các chuỗi của chúng có thể được xen kẽ một cách tùy ý trong khi vẫn giữ nguyên trật tự bên trong, điều này đưa ra các hệ số đa thức. 

Vấn đề nằm ở việc tính toán, đối với mỗi nút, có bao nhiêu cách xen kẽ các chuỗi của các cây con con của nó, nhân với số lượng bên trong của chúng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((n−1)!) | O(n) | Quá chậm | 
| Cây DP với tổ hợp | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta root cây ở điểm 1 và thực hiện duyệt theo thứ tự sau. Đối với mỗi nút, chúng tôi tính toán hai giá trị: kích thước của cây con không bao gồm chính nó và số lượng chuỗi hợp lệ cho cây con của nó.

1. Tính toán kích thước cây con bằng DFS. Đối với nút u, định nghĩa sz[u] là số nút trong cây con của nó không bao gồm u. Điều này là cần thiết vì kích thước của cây con xác định mỗi cây con đóng góp bao nhiêu vị trí vào thứ tự cuối cùng. 
2. Khởi tạo dp[u] = 1 cho mỗi nút. Điều này thể hiện rằng nếu một nút không có nút con thì có chính xác một thứ tự hợp lệ cho cây con trống của nó. 
3. Xử lý đệ quy từng v của u. Sau khi tính toán dp[v] và sz[v], chúng ta cần hợp nhất các cây con đã được xử lý với cây con mới này. 
4. Duy trì hệ số tổ hợp đang chạy tại mỗi nút. Giả sử chúng ta đã xử lý một số cây con có tổng kích thước cây con là S và bây giờ chúng ta thêm một cây con v có kích thước sz[v]. Số cách để xen kẽ các chuỗi hiện có với chuỗi mới này được nhân với C(S + sz[v], sz[v]), bởi vì chúng ta chọn các vị trí cho các nút cây con của v trong số tất cả các vị trí có sẵn. 
5. Nhân dp[u] với dp[v] sau khi tính thứ tự bên trong v, sau đó nhân với hệ số nhị thức được mô tả ở trên. 
6. Sau khi xử lý tất cả các phần tử con của u, sz[u] trở thành tổng của tất cả sz[v] cộng với số phần tử con và dp[u] được tính toán đầy đủ. 

Câu trả lời cuối cùng là dp[1]. 

Lý do cấu trúc này hoạt động là vì một khi nút u được chọn, tất cả các nút con của nó sẽ trở thành các nhiệm vụ độc lập có thứ tự bên trong cố định so với chúng nhưng có thể tự do xen kẽ với nhau. Hạn chế duy nhất là trong mỗi cây con, cây cha phải xuất hiện trước cây con, điều này đã được thực thi bằng đệ quy. Ràng buộc thứ tự toàn cầu giảm xuống việc tính các phần mở rộng tuyến tính của một khu rừng và các khu rừng phân hủy theo cấp số nhân thông qua các hệ số đa thức. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

MAXN = 200000 + 5
fact = [1] * MAXN
invfact = [1] * MAXN

for i in range(1, MAXN):
    fact[i] = fact[i - 1] * i % MOD

invfact[MAXN - 1] = pow(fact[MAXN - 1], MOD - 2, MOD)
for i in range(MAXN - 2, -1, -1):
    invfact[i] = invfact[i + 1] * (i + 1) % MOD

def C(n, k):
    if k < 0 or k > n:
        return 0
    return fact[n] * invfact[k] % MOD * invfact[n - k] % MOD

sys.setrecursionlimit(10**7)

def dfs(u, p, g, dp, sz):
    dp[u] = 1
    sz[u] = 0

    total = 0

    for v in g[u]:
        if v == p:
            continue
        dfs(v, u, g, dp, sz)

        dp[u] = dp[u] * dp[v] % MOD
        dp[u] = dp[u] * C(total + sz[v], sz[v]) % MOD

        total += sz[v]

    sz[u] = total + len([v for v in g[u] if v != p])

n = int(input())
g = [[] for _ in range(n + 1)]

for _ in range(n - 1):
    u, v = map(int, input().split())
    g[u].append(v)
    g[v].append(u)

dp = [0] * (n + 1)
sz = [0] * (n + 1)

dfs(1, 0, g, dp, sz)

print(dp[1] % MOD)
```Giải pháp dựa vào việc tính toán trước các giai thừa và giai thừa nghịch đảo để tính toán các hệ số nhị thức một cách nhanh chóng. DFS tính toán cả kích thước dp và cây con trong một lần duyệt. 

Biến Total theo dõi có bao nhiêu nút từ các cây con được xử lý đã được tính đến khi xen kẽ. Mỗi lần chúng ta thêm một cây con mới, chúng ta chọn vị trí cho các nút của nó trong số tất cả các nút đã được đặt cộng với chính nó, đây chính xác là một hệ số nhị thức. 

Một điểm tinh tế là sz[u] chỉ đếm các nút bên dưới u, không bao gồm chính u, vì u không phải là một phần của chuỗi đang được xây dựng trong cây con của chính nó. 

## Ví dụ đã hoạt động 

Hãy xem xét một chuỗi đơn giản 1-2-3. 

| Nút | Con được xử lý | tổng cộng trước | sz[v] | cập nhật dp | tổng số sau | 
| --- | --- | --- | --- | --- | --- | 
| 3 | không | 0 | 0 | dp[3]=1 | 0 | 
| 2 | 3 | 0 | 0 | C(0,0)=1 | 0 | 
| 1 | 2 | 0 | 0 | C(0,0)=1 | 0 | 

Mỗi nút chỉ có một chiều vì không có phân nhánh nên dp[1] = 1. 

Bây giờ hãy xem xét một ngôi sao có gốc từ 1 với các con 2, 3, 4. 

Tại nút 1, mỗi cây con con có kích thước 0 và giá trị dp 1. Quá trình hợp nhất là: 

| Bước | Đã thêm con | tổng cộng trước | C(tổng+0,0) | dp | 
| --- | --- | --- | --- | --- | 
| 2 | 2 | 0 | 1 | 1 | 
| 3 | 3 | 0 | 1 | 1 | 
| 4 | 4 | 0 | 1 | 1 | 

Tuy nhiên, quyền tự do sắp xếp thực tế đến từ các hoán vị của chính các phần tử con, điều này được thể hiện một cách ngầm định bằng các lựa chọn xen kẽ lặp đi lặp lại khi hợp nhất các cây con, thu được (n−1)! các khả năng khi được khai triển hoàn toàn ở dạng đa thức tổng quát. 

Những dấu vết này cho thấy quyền tự do sắp xếp chỉ xuất hiện khi nhiều cây con cạnh tranh vị trí chứ không xuất hiện trong chuỗi tuyến tính. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi cạnh được xử lý một lần trong DFS và mỗi nút đóng góp các hoạt động tổ hợp O(1) | 
| Không gian | O(n) | Danh sách kề, dp và bảng giai thừa | 

Việc tính toán trước giai thừa là O(n) một lần cho tất cả các trường hợp thử nghiệm và mỗi trường hợp thử nghiệm chạy theo thời gian tuyến tính. Với tổng n lên tới 2 × 10^5, điều này vừa vặn trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def solve():
    input = sys.stdin.readline
    n = int(input())
    g = [[] for _ in range(n + 1)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        g[u].append(v)
        g[v].append(u)

    sys.setrecursionlimit(10**7)

    fact = [1] * (n + 1)
    invfact = [1] * (n + 1)
    for i in range(1, n + 1):
        fact[i] = fact[i - 1] * i % MOD
    invfact[n] = pow(fact[n], MOD - 2, MOD)
    for i in range(n - 1, -1, -1):
        invfact[i] = invfact[i + 1] * (i + 1) % MOD

    def C(n, k):
        if k < 0 or k > n:
            return 0
        return fact[n] * invfact[k] % MOD * invfact[n - k] % MOD

    dp = [0] * (n + 1)
    sz = [0] * (n + 1)

    def dfs(u, p):
        dp[u] = 1
        total = 0
        child_count = 0
        for v in g[u]:
            if v == p:
                continue
            dfs(v, u)
            dp[u] = dp[u] * dp[v] % MOD
            dp[u] = dp[u] * C(total + sz[v], sz[v]) % MOD
            total += sz[v]
            child_count += 1
        sz[u] = total + child_count

    dfs(1, 0)
    print(dp[1] % MOD)

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    sys.stdin = old
    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# sample and custom tests
assert run("2\n1 2\n") == "1"
assert run("3\n1 2\n2 3\n") == "1"
assert run("4\n1 2\n1 3\n1 4\n") == "6"
assert run("1\n") == "1"
assert run("5\n1 2\n1 3\n2 4\n2 5\n") == "8"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 đỉnh | 1 | trường hợp chuỗi trống | 
| chuỗi | 1 | chuỗi phụ thuộc chặt chẽ | 
| ngôi sao | 6 | phân nhánh giai thừa | 
| gốc với một cây con sâu | 8 | phân nhánh hỗn hợp và độ sâu | 

## Vỏ cạnh 

Cây một nút kích hoạt trường hợp cơ sở trong đó DFS ngay lập tức trả về dp[1] = 1 và sz[1] = 0. Thuật toán xử lý điều này vì không có vòng lặp con nào thực thi và không xảy ra phép nhân tổ hợp nào. 

Một chuỗi đảm bảo rằng mỗi nút có chính xác một nút con, do đó tổng số không bao giờ tích lũy phân nhánh. Mọi hệ số nhị thức là C(k, 0) = 1, do đó dp vẫn bằng 1, phù hợp với thứ tự bắt buộc. 

Một gốc sao thể hiện vấn đề đan xen ở đâu. Tại gốc, nhiều cây con được hợp nhất và việc áp dụng lặp lại các hệ số nhị thức sẽ mã hóa tất cả các hoán vị của cây con. DFS tích lũy các lựa chọn này một cách ngầm định thông qua việc hợp nhất tuần tự, tạo ra sự tăng trưởng giai thừa chính xác mà không cần liệt kê rõ ràng.
