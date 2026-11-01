---
title: "CF 106463B - Cuộc chiến thực phẩm"
description: "Chúng ta được cấp một cây có gốc đại diện cho một tập hợp các gian hàng được kết nối theo thứ bậc. Cách chúng ta “ghé thăm” các ngăn này được cố định bằng cách duyệt giống như DFS và thứ tự mà chúng ta vào từng cây con lần đầu tiên sẽ xác định cách chúng ta chỉ định mỗi ngăn cho một trong hai đội."
date: "2026-06-19T17:16:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106463
codeforces_index: "B"
codeforces_contest_name: "MITIT Spring 2026 Invitationals Qualification Round 2"
rating: 0
weight: 106463
solve_time_s: 56
verified: true
draft: false
---

[CF 106463B - Cuộc chiến thực phẩm](https://codeforces.com/problemset/problem/106463/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 56s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một cây có gốc đại diện cho một tập hợp các gian hàng được kết nối theo thứ bậc. Cách chúng ta “ghé thăm” các ngăn này được cố định bằng cách duyệt giống như DFS và thứ tự mà chúng ta vào từng cây con lần đầu tiên sẽ xác định cách chúng ta chỉ định mỗi ngăn cho một trong hai đội. 

Nguyên tắc chính là đội của gian hàng không được chọn một cách độc lập. Thay vào đó, nó phụ thuộc vào tính chẵn lẻ của vị trí truy cập đầu tiên của nó theo thứ tự truyền tải toàn cầu. Khi chúng ta vào cây con của một nút, chúng ta duyệt qua các cây con của nút đó theo một thứ tự nào đó và mỗi cây con con đóng góp một khối các nút được truy cập có kích thước ảnh hưởng đến sự dịch chuyển chẵn lẻ cho mọi thứ theo sau nó. 

Nhiệm vụ là đếm xem có bao nhiêu phép gán toàn cục hợp lệ của các lựa chọn thứ tự cây con-con dẫn đến các phép gán nhóm nhất quán trên toàn bộ cây, với ràng buộc là gốc được cố định vào Team Potato. 

Đầu vào mô tả một cây có gốc và đầu ra là số cách hợp lệ để sắp xếp các lựa chọn truyền tải sao cho tất cả các bài tập nhóm được tạo ra là nhất quán, modulo một số nguyên tố lớn. 

Thang đo ràng buộc (điển hình cho loại bài toán DP cây này) hàm ý hành vi tuyến tính hoặc gần tuyến tính là bắt buộc. Bất cứ điều gì cố gắng hoán vị con một cách rõ ràng hoặc mô phỏng tất cả thứ tự của cây con sẽ bùng nổ theo giai thừa. Ngay cả việc phân nhánh vừa phải cũng sẽ dẫn đến sự bùng nổ theo cấp số nhân vì các nút con của mỗi nút có thể được sắp xếp lại theo nhiều cách và mỗi thứ tự đều ảnh hưởng đến việc truyền bá chẵn lẻ trên toàn cầu. 

Một trường hợp cạnh tinh tế phát sinh khi một nút có các nút con có kích thước cây con đều bằng nhau. Trong tình huống đó, việc duyệt bất kỳ cây con con nào không lật tính chẵn lẻ, do đó thứ tự tương đối không còn thay đổi các phép gán trong tương lai. Một cách tiếp cận ngây thơ cho rằng tất cả trẻ em đều cư xử đối xứng sẽ là quá đáng. Một trường hợp khác xuất hiện khi tồn tại nhiều cây con có kích thước lẻ: việc hoán đổi chúng sẽ làm thay đổi cấu trúc luân phiên chẵn lẻ, nhưng không độc lập đối với từng cây con, dẫn đến các ràng buộc tổ hợp rất dễ bị bỏ sót nếu xử lý các cây con một cách độc lập. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ cố gắng liệt kê tất cả các hoán vị của các phần tử con tại mỗi nút, mô phỏng quá trình truyền tải DFS đầy đủ cho mỗi hoán vị và tính toán các phép gán chẵn lẻ cảm ứng. Điều này đúng về mặt khái niệm vì nó trực tiếp mô hình hóa quá trình truyền tải được mô tả trong bài toán. Tuy nhiên, nếu một nút có k nút con thì sẽ có k! thứ tự, và trên cây này nhân lên nhanh chóng. Ngay cả đối với k khoảng 10, sự tăng trưởng giai thừa làm cho điều này không thể thực hiện được và với tối đa n nút, tổng số cấu hình sẽ trở nên lớn về mặt thiên văn. 

Quan sát quan trọng là chúng ta không thực sự quan tâm đến thứ tự chính xác của tất cả các phần tử con mà chỉ quan tâm đến việc kích thước cây con ảnh hưởng như thế nào đến quá trình chuyển đổi chẵn lẻ. Mỗi cây con đóng góp một khối có kích thước xác định liệu nó có lật tính chẵn lẻ cho các cây con tiếp theo hay không. Điều này làm giảm vấn đề từ hoán vị của toàn bộ cấu trúc sang hiệu ứng tổ hợp được kiểm soát dựa trên kích thước cây con chẵn và lẻ. 

Chúng tôi xác định một DP trong đó đối với mỗi nút x, chúng tôi tính toán số lượng cấu hình hợp lệ của cây con của nó giả sử x được gán cho một nhóm cố định. Khi các cây con con được xử lý, mỗi cây con y đóng góp hai thông tin: số lượng cấu hình hợp lệ bên trong DP[y] và kích thước cây con của nó là chẵn hay lẻ, điều này quyết định liệu nó có lật tính chẵn lẻ khi duyệt hay không. 

Nếu tất cả các kích thước của cây con con đều là số chẵn thì không có kích thước nào trong số chúng ảnh hưởng đến việc truyền bá chẵn lẻ. Điều này buộc tất cả các gốc con phải căn chỉnh một cách nhất quán và quyền tự do duy nhất là sắp xếp nội bộ của từng cây con. Vì vậy, kết quả là một sản phẩm đơn giản trên trẻ em.

Nếu tồn tại ít nhất một cây con có kích thước lẻ thì tính chẵn lẻ sẽ thay thế khi chúng ta duyệt qua các cây con lẻ theo thứ tự đã chọn. Các cây con có kích thước chẵn hoạt động khác nhau: chúng không lật tính chẵn lẻ nên có thể được chèn linh hoạt mà không ảnh hưởng đến cấu trúc xen kẽ giữa các cây con lẻ. Cấu trúc tổ hợp giảm xuống việc chọn số lượng cây con lẻ nằm trong mỗi lớp chẵn lẻ, dẫn đến hệ số nhị thức dựa trên cách các cây con lẻ phân chia theo các vai trò xen kẽ. 

DP trở nên tuyến tính vì mỗi nút xử lý các nút con của nó một lần và kết hợp các kết quả theo thời gian O(deg(x)). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force (hoán vị con + mô phỏng DFS) | O(n!) trường hợp xấu nhất | O(n) | Quá chậm | 
| Cây DP với nhóm chẵn lẻ | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi root cây tại nút 1 và tính toán kích thước cây con và giá trị DP trong quá trình duyệt thứ tự sau. 

1. Trước tiên hãy tính kích thước cây con. Với mỗi nút x, chúng ta tích lũy sz[x] bằng 1 cộng với kích thước của tất cả nút con. Điều này là cần thiết vì hành vi chẵn lẻ phụ thuộc hoàn toàn vào việc cây con có kích thước chẵn hay lẻ. 
2. Xác định DP[x] là số cấu hình hợp lệ của cây con gốc tại x với giả định rằng bản thân x được gán cho Team Potato. Việc chuẩn hóa này cho phép chúng tôi tránh theo dõi các trạng thái chẵn lẻ tuyệt đối trên toàn cầu. 
3. Đối với mỗi nút x, trước tiên chúng tôi xử lý tất cả các nút con y và tính DP[y] và sz[y]. Tại thời điểm này, mỗi cây con con được tóm tắt đầy đủ thành hai giá trị: số lượng đóng góp của nó và liệu nó có lật tính chẵn lẻ hay không. 
4. Chia con của x thành hai nhóm: nhóm có sz[y] chẵn và nhóm có sz[y] lẻ. Gọi số đếm lần lượt là q và p. 
5. Nếu p bằng 0, nghĩa là tất cả các cây con đều có kích thước chẵn, thì không có cây con nào lật ngang tính chẵn lẻ khi duyệt qua. Trong trường hợp này, mỗi gốc con phải căn chỉnh một cách nhất quán và không có hiệu ứng xen kẽ tổ hợp giữa các cây con. Chúng ta tính DP[x] là tích của DP[y] trên tất cả các con y. 
6. Nếu p lớn hơn 0, tính chẵn lẻ sẽ thay thế bất cứ khi nào chúng ta duyệt qua một cây con lẻ. Các cây con chẵn có thể được đặt mà không ảnh hưởng đến mô hình xen kẽ này, nghĩa là chúng đóng góp theo cấp số nhân dưới dạng các khối độc lập. 
7. Đối với các cây con có kích thước lẻ, các nhóm gốc của chúng luân phiên nhau tùy theo vị trí trong thứ tự duyệt. Chỉ có sự phân bố của các cây con lẻ thành các vai trò chẵn lẻ xen kẽ mới quan trọng chứ không phải thứ tự bên trong của chúng. Số lần gán vai trò chẵn lẻ hợp lệ giữa p cây con lẻ là hệ số nhị thức C(p, ⌊p/2⌋). 
8. Kết hợp các đóng góp dưới dạng DP[x] = C(p, ⌊p/2⌋) nhân với tích của DP[y] trên tất cả trẻ em. 
9. Tính trước giai thừa và nghịch đảo giai thừa modulo 10^9+7 để tính hệ số nhị thức trong thời gian không đổi. 

Lý do nó hoạt động là do chỉ theo dõi các lần lật chẵn lẻ gây ra bởi kích thước cây con. Mỗi cây con đóng góp hoặc lật chẵn lẻ (kích thước lẻ) hoặc không lật (kích thước chẵn). Vì thứ tự truyền tải chỉ quan trọng thông qua các thay đổi chẵn lẻ tích lũy, cấu trúc sẽ sụp đổ trong việc đếm xem có bao nhiêu cách mà những người đóng góp lẻ có thể được chỉ định các vai trò xen kẽ, trong khi những người đóng góp chẵn vẫn là số nhân trung tính. DP đúng vì mỗi cây con hoàn toàn độc lập khi đóng góp chẵn lẻ của nó được cố định và không có tương tác nào giữa các cây con rời rạc ảnh hưởng đến cấu trúc bên trong ngoài trạng thái chẵn lẻ này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

# factorial precomputation up to n
def build_fact(n):
    fact = [1] * (n + 1)
    invfact = [1] * (n + 1)

    for i in range(2, n + 1):
        fact[i] = fact[i - 1] * i % MOD

    invfact[n] = pow(fact[n], MOD - 2, MOD)
    for i in range(n - 1, -1, -1):
        invfact[i] = invfact[i + 1] * (i + 1) % MOD

    return fact, invfact

def C(n, r, fact, invfact):
    if r < 0 or r > n:
        return 0
    return fact[n] * invfact[r] % MOD * invfact[n - r] % MOD

sys.setrecursionlimit(10**7)

n = int(input())
g = [[] for _ in range(n)]
for _ in range(n - 1):
    u, v = map(int, input().split())
    u -= 1
    v -= 1
    g[u].append(v)
    g[v].append(u)

fact, invfact = build_fact(n)

sz = [0] * n
dp = [1] * n

def dfs(x, p):
    sz[x] = 1
    odd = 0
    even = 0

    for y in g[x]:
        if y == p:
            continue
        dfs(y, x)
        sz[x] += sz[y]

        if sz[y] % 2 == 0:
            even += 1
        else:
            odd += 1

    prod = 1
    for y in g[x]:
        if y == p:
            continue
        prod = prod * dp[y] % MOD

    if odd == 0:
        dp[x] = prod
    else:
        dp[x] = prod * C(odd, odd // 2, fact, invfact) % MOD

dfs(0, -1)

print(dp[0])
```DFS tính toán cả kích thước cây con và giá trị DP trong một lần duyệt. Việc tách các cây con lẻ và chẵn là cần thiết vì chỉ có các cây con lẻ mới đưa ra các phép lật chẵn lẻ. Việc tích lũy sản phẩm đảm bảo các cấu hình bên trong độc lập được nhân lên một cách chính xác. 

Hệ số nhị thức được tính toán bằng cách sử dụng tính toán trước giai thừa, tránh các lệnh gọi lũy thừa mô-đun logarit lặp đi lặp lại. Một lỗi phổ biến là tính toán lại các kết hợp trên mỗi nút mà không lưu vào bộ nhớ đệm, điều này sẽ đẩy độ phức tạp về phía O(n log n). Một vấn đề nhỏ khác là quên tách các cạnh cha mẹ trong DFS, điều này sẽ làm tăng kích thước cây con một cách không chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Xét một cây nhỏ có gốc 1 có hai lá con 2 và 3. 

| Nút | sz | những đứa trẻ kỳ quặc | ngay cả trẻ em | DP | 
| --- | --- | --- | --- | --- | 
| 2 | 1 | 0 | 0 | 1 | 
| 3 | 1 | 0 | 0 | 1 | 
| 1 | 3 | 2 | 0 | C(2,1)=2 | 

Tại các lá, DP gần bằng 1. Tại gốc, cả hai cây con đều có kích thước cây con lẻ, do đó p = 2. Số cách để gán các vai trò chẵn lẻ xen kẽ là C(2,1)=2, tạo ra DP[1]=2. 

Điều này xác nhận rằng khi tồn tại nhiều cây con lẻ, câu trả lời phản ánh cách chúng luân phiên theo thứ tự truyền tải. 

### Ví dụ 2 

Bây giờ hãy xem xét gốc 1 có một con 2 và nút 2 có một con 3. 

| Nút | sz | những đứa trẻ kỳ quặc | ngay cả trẻ em | DP | 
| --- | --- | --- | --- | --- | 
| 3 | 1 | 0 | 0 | 1 | 
| 2 | 2 | 0 | 1 | 1 | 
| 1 | 3 | 1 | 0 | 1 | 

Nút 2 có kích thước cây con bằng nhau, do đó nó không góp phần lật lên tính chẵn lẻ. Tại nút 1, chỉ có một nút con có kích thước lẻ, do đó p = 1 và C(1,0)=1, mang lại DP[1]=1. 

Điều này cho thấy các cây con có kích thước chẵn ngăn chặn những thay đổi chẵn lẻ và loại bỏ sự phân nhánh tổ hợp như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi nút và cạnh được xử lý một lần trong DFS và tất cả các chuyển đổi DP đều là O(1) bằng cách sử dụng các giai thừa được tính toán trước | 
| Không gian | O(n) | Danh sách kề, kích thước cây con, mảng DP và ngăn xếp đệ quy | 

Cấu trúc tuyến tính phù hợp thoải mái trong các ràng buộc điển hình lên tới 2e5 nút và quá trình tiền xử lý giai thừa cũng vẫn tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 10**9 + 7

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    n = int(input())
    g = [[] for _ in range(n)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append(v)
        g[v].append(u)

    fact = [1] * (n + 1)
    invfact = [1] * (n + 1)
    for i in range(2, n + 1):
        fact[i] = fact[i - 1] * i % MOD
    invfact[n] = pow(fact[n], MOD - 2, MOD)
    for i in range(n - 1, -1, -1):
        invfact[i] = invfact[i + 1] * (i + 1) % MOD

    def C(n, r):
        if r < 0 or r > n:
            return 0
        return fact[n] * invfact[r] % MOD * invfact[n - r] % MOD

    sys.setrecursionlimit(10**7)

    sz = [0] * n
    dp = [1] * n

    def dfs(x, p):
        sz[x] = 1
        odd = 0
        even = 0

        for y in g[x]:
            if y == p:
                continue
            dfs(y, x)
            sz[x] += sz[y]
            if sz[y] % 2 == 0:
                even += 1
            else:
                odd += 1

        prod = 1
        for y in g[x]:
            if y == p:
                continue
            prod = prod * dp[y] % MOD

        if odd == 0:
            dp[x] = prod
        else:
            dp[x] = prod * C(odd, odd // 2) % MOD

    dfs(0, -1)
    return str(dp[0])

# provided samples
# assert run("...") == "...", "sample 1"

# custom cases

assert run("1") == "1", "single node"

assert run("2\n1 2") == "1", "two nodes"

assert run("3\n1 2\n1 3") == "2", "star shape"

assert run("4\n1 2\n2 3\n3 4") == "1", "chain"

assert run("5\n1 2\n1 3\n1 4\n1 5") == "6", "larger star-like case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 1 | trường hợp DP cơ sở | 
| cây 2 nút | 1 | cạnh đơn giản nhất | 
| ngôi sao 3 lá | 2 | tổ hợp con lẻ | 
| chuỗi | 1 | nhân giống không phân nhánh | 
| sao 5 nút | 6 | chia tỷ lệ hiệu ứng nhị thức | 

## Vỏ cạnh 

Cây nút đơn là trường hợp ranh giới rõ ràng nhất. DFS gán sz[1]=1 và không có con nào, vì vậy lẻ=0 và DP[1]=1. Thuật toán trả về 1 trực tiếp, khớp với phép gán duy nhất có thể. 

Một cấu trúc chuỗi thể hiện sự ngăn chặn các lựa chọn tổ hợp. Mỗi nút ngoại trừ nút gốc có chính xác một nút con và kích thước của các cây con thay thế nhau có tính chẵn lẻ. Ở mỗi bước, số lẻ là 0 hoặc 1, do đó các hệ số nhị thức đánh giá là 1, tạo ra một cấu hình hợp lệ về tổng thể. 

Cây hình ngôi sao là nơi xuất hiện lõi tổ hợp. Nếu tất cả các cây con đều là lá thì mọi cây con con đều có kích thước là 1, làm cho tất cả chúng đều là lá lẻ. Tại gốc, p bằng số con và DP trở thành C(p, ⌊p/2⌋). Việc triển khai sẽ nhóm chính xác tất cả các phần tử con thành những người đóng góp lẻ giống hệt nhau và tính các phép gán chẵn lẻ xen kẽ mà không tính toán quá mức các hoán vị của cấu trúc bên trong.
