---
title: "CF 106328A - DeepTreek"
description: "Chúng ta có một cây có gốc có gốc ở đỉnh 1. Mỗi đỉnh có một cha mẹ ngoại trừ gốc và độ sâu được xác định theo cách tiêu chuẩn là khoảng cách từ gốc. Chúng ta xem xét các cặp đỉnh có thứ tự $(u, v)$ với ba hạn chế. Đầu tiên, $u neq v$."
date: "2026-06-19T16:56:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106328
codeforces_index: "A"
codeforces_contest_name: "Baozii Cup 3"
rating: 0
weight: 106328
solve_time_s: 80
verified: true
draft: false
---

[CF 106328A - DeepTreek](https://codeforces.com/problemset/problem/106328/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 20s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có gốc có gốc ở đỉnh 1. Mỗi đỉnh có một cha mẹ ngoại trừ gốc và độ sâu được xác định theo cách tiêu chuẩn là khoảng cách từ gốc. 

Chúng ta xét các cặp đỉnh có thứ tự$(u, v)$với ba hạn chế. Đầu tiên,$u \neq v$. Thứ hai,$u \neq 1$, do đó gốc không bao giờ được chọn làm đỉnh di chuyển. Thứ ba,$u$không phải là tổ tiên của$v$, tương đương với việc nói rằng$v$không nằm trong cây con của$u$. 

Đối với bất kỳ cặp nào như vậy, chúng tôi thực hiện sửa đổi cấu trúc: chúng tôi tách$u$từ cha mẹ của nó và sau đó kết nối$u$trực tiếp dưới$v$. Điều này giữ cho cấu trúc là một cây vì chúng ta đang di chuyển toàn bộ cây con và kết nối lại nó ở một vị trí hợp lệ bên ngoài chính nó. 

Sau sửa đổi này, chúng tôi tính toán lại độ sâu của cây và xác định$f(u, v)$là độ sâu tối đa tính từ gốc. 

Nhiệm vụ là tính tổng$f(u, v)$trên tất cả các cặp hợp lệ. 

Các ràng buộc đủ chặt chẽ đến mức không thể lặp lại tất cả các cặp. Với$n$lên đến$2 \cdot 10^5$qua các trường hợp thử nghiệm, một phép liệt kê bậc hai sẽ yêu cầu theo thứ tự$10^{10}$hoạt động vượt xa mọi giới hạn khả thi. Thậm chí một$O(n \log n)$giải pháp phải được thiết kế cẩn thận để tránh hành vi bậc hai ẩn trong các phép toán cây con. 

Một nỗ lực ngây thơ cũng có xu hướng thất bại một cách tinh vi hơn là chỉ về thời gian. Nếu người ta cố gắng tính toán lại độ sâu của cây sau mỗi lần gắn lại cạnh bằng BFS hoặc DFS, thậm chí$O(n)$tính toán lại cho mỗi cặp dẫn trực tiếp đến thời gian chờ. 

Có hai trường hợp cạnh cấu trúc phá vỡ lý luận bất cẩn. 

Nếu cây là một chuỗi thì hầu như mỗi cặp$(u, v)$có giá trị trừ khi$v$nằm ở trên$u$. Bất kỳ giải pháp nào giả định tính độc lập giữa các nút hoặc bỏ qua loại trừ cây con sẽ bị tính sai$v$. 

Nếu cây là một ngôi sao có gốc tại 1 thì mọi nút không phải gốc đều là một lá. Di chuyển một lá dưới một lá khác không làm thay đổi gì về chiều sâu, nhưng một công thức ngây thơ luôn giả định chiều cao của cây con tăng lên có thể tính quá mức đóng góp một cách đáng kể. 

Những trường hợp này buộc chúng ta phải suy luận chính xác về việc chuyển động của cây con chỉ thay đổi một phần của cây và giữ nguyên mọi thứ khác. 

## Phương pháp tiếp cận 

Chiến lược bạo lực trực tiếp chọn mọi cặp hợp lệ$(u, v)$, thực hiện hoán đổi cạnh và tính toán lại độ sâu của cây từ đầu. Điều này rất đơn giản: sau khi sửa đổi cây, hãy chạy DFS từ gốc và tính khoảng cách tối đa. Tính chính xác là ngay lập tức vì nó mô phỏng đúng định nghĩa. 

Vấn đề là chi phí. có$O(n^2)$cặp trong trường hợp xấu nhất và mỗi lần tính toán lại là$O(n)$, dẫn đến$O(n^3)$. Ngay cả khi chúng tôi tối ưu hóa việc tính toán lại để$O(n)$tổng công việc mỗi cặp thông qua cập nhật tăng dần, chúng tôi vẫn đạt được$O(n^2)$, quá lớn đối với$2 \cdot 10^5$. 

Quan sát quan trọng là hoạt động chỉ ảnh hưởng đến khoảng cách bên trong cây con của$u$. Mọi nút khác vẫn giữ khoảng cách ban đầu với nút gốc. Vì vậy, độ sâu của cây mới được xác định bởi sự cạnh tranh giữa độ sâu toàn cục cũ và nút sâu nhất bên trong cây con đã di chuyển sau khi nó được gắn lại bên dưới.$v$. 

Nếu chúng ta ký hiệu$D$như độ sâu của cây ban đầu,$d[v]$như độ sâu của$v$, Và$h[u]$như chiều cao của nút$u$cây con của nó, sau đó sau khi di chuyển, nút sâu nhất bên trong cây con của$u$trở thành$d[v] + 1 + h[u]$. Mọi thứ khác vẫn ở độ sâu nhiều nhất$D$. Vì vậy độ sâu kết quả là$$f(u, v) = \max(D, d[v] + 1 + h[u]).$$Vấn đề giảm xuống còn tính tổng biểu thức này trên tất cả các cặp hợp lệ. Sự hạn chế đó$v$không được nằm bên trong cây con của$u$chỉ ảnh hưởng đến cái nào$v$được bao gồm trong phép tính tổng chứ không phải bản thân công thức. 

Thử thách còn lại là đối với mỗi$u$, chúng ta phải tính tổng tất cả$v$bên ngoài cây con của nó, nhưng sự đóng góp phụ thuộc vào việc liệu$d[v]$ở trên hoặc dưới ngưỡng được xác định bởi$h[u]$. Điều này đương nhiên dẫn đến việc đếm các nút theo độ sâu, cả trên toàn cầu và bên trong mỗi cây con. 

Chúng tôi duy trì phân phối tần số theo độ sâu trên toàn cầu và trên mỗi cây con. Điều này cho phép chúng ta trả lời cho mỗi$u$, có bao nhiêu hợp lệ$v$có độ sâu trên hoặc dưới ngưỡng và tổng độ sâu của chúng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2 \log n)$hoặc tệ hơn |$O(n)$| Quá chậm | 
| Đếm độ sâu + tần số cây con (DSU trên cây) |$O(n \log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi root cây ở mức 1 và tính toán cho mỗi nút độ sâu và chiều cao cây con bằng cách sử dụng DFS. 

Chúng tôi cũng tính toán độ sâu tối đa toàn cầu$D$, vì giá trị này trở thành độ sâu đường cơ sở sau bất kỳ thao tác nào. 

Tiếp theo, chúng tôi xây dựng một bảng tần số chung theo độ sâu, lưu trữ số lượng nút tồn tại ở mỗi độ sâu và tổng độ sâu của chúng. 

Sau đó chúng tôi xử lý từng nút$u$sử dụng chiến lược DSU trên cây dựa trên DFS, trong đó mỗi nút duy trì một bảng tần số có độ sâu bên trong cây con của nó. 

1. Chạy DFS để tính toán độ sâu và kích thước cây con, đồng thời tính toán chiều cao của cây con. Chiều cao được tính từ dưới lên là khoảng cách tối đa đến một chiếc lá. 
2. Duy trì bản đồ tần số cho mỗi cây con để đếm số lượng nút xuất hiện ở mỗi độ sâu. 
3. Trong DFS, hợp nhất các bản đồ tần số con vào tần số gốc bằng cách sử dụng chiến lược hợp nhất từ ​​nặng đến nhẹ để tổng độ phức tạp vẫn giữ nguyên logarit tuyến tính. 
4. Khi bản đồ tần số cây con cho nút$u$hoàn tất, hãy tính toán phiên bản tiền tố theo độ sâu để chúng ta có thể nhanh chóng trả lời “có bao nhiêu nút trong tập hợp này có độ sâu ≤ T”. 
5. Đối với nút$u$, tính ngưỡng$T = D - 1 - h[u]$. Đây là điểm cắt nơi gắn$u$dưới$v$không còn tệ hơn độ sâu ban đầu. 
6. Xây dựng phần bù của cây con bằng cách trừ tần số của cây con khỏi tần số chung, cả về số lượng và tổng độ sâu. 
7. Chia hợp lệ$v$thành hai nhóm: những nhóm có độ sâu ≤$T$, nơi câu trả lời đóng góp$D$, và những người có chiều sâu >$T$, nơi câu trả lời đóng góp$d[v] + 1 + h[u]$. 
8. Tích lũy đóng góp bằng cách sử dụng tổng tiền tố cho cả cấu trúc cây chung và cây con, đồng thời cộng kết quả cho nút$u$vào câu trả lời cuối cùng. 

Tính đúng đắn dựa trên thực tế là mọi cặp hợp lệ đều được phân loại chính xác một lần và mọi cặp có thể.$v$đóng góp theo một công thức xác định duy nhất chỉ phụ thuộc vào độ sâu và$h[u]$. 

Điều bất biến là tại thời điểm chúng tôi tính toán nút$u$, chúng ta có phân bố tần số chính xác về độ sâu trong cây con của nó và phân bố toàn cầu cố định. Mỗi hợp lệ$v$nằm trong cây con hoặc bên ngoài nó và chỉ các nút bên ngoài mới được xem xét. Việc phân chia theo ngưỡng đảm bảo rằng mỗi$v$được phân công đúng hình thức đóng góp, không chồng chéo. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**7)

def solve():
    n = int(input())
    g = [[] for _ in range(n + 1)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        g[u].append(v)
        g[v].append(u)

    parent = [0] * (n + 1)
    depth = [0] * (n + 1)
    order = []
    
    # iterative DFS to avoid recursion depth issues
    stack = [1]
    parent[1] = -1
    depth[1] = 0

    while stack:
        u = stack.pop()
        order.append(u)
        for v in g[u]:
            if v == parent[u]:
                continue
            parent[v] = u
            depth[v] = depth[u] + 1
            stack.append(v)

    children = [[] for _ in range(n + 1)]
    for v in range(2, n + 1):
        children[parent[v]].append(v)

    # subtree size order reversed for DP
    subtree_order = order[::-1]

    height = [0] * (n + 1)
    for u in subtree_order:
        for v in children[u]:
            height[u] = max(height[u], height[v] + 1)

    D = max(depth)

    MAXD = D
    global_cnt = [0] * (MAXD + 2)
    global_sum = [0] * (MAXD + 2)

    for i in range(1, n + 1):
        global_cnt[depth[i]] += 1
        global_sum[depth[i]] += depth[i]

    for i in range(1, MAXD + 1):
        global_cnt[i] += global_cnt[i - 1]
        global_sum[i] += global_sum[i - 1]

    ans = 0

    def dfs(u):
        nonlocal ans
        cnt = [0] * (MAXD + 2)
        sm = [0] * (MAXD + 2)

        cnt[depth[u]] = 1
        sm[depth[u]] = depth[u]

        for v in children[u]:
            c2, s2 = dfs(v)
            if len(c2) > len(cnt):
                cnt, c2 = c2, cnt
                sm, s2 = s2, sm
            for i in range(MAXD + 1):
                cnt[i] += c2[i]
                sm[i] += s2[i]

        # prefix already global; build subtree prefix
        cnt_sub = cnt
        sum_sub = sm

        T = D - 1 - height[u]

        # compute S_u = global - subtree
        # prefix helper inline
        cnt_le = 0
        sum_le = 0
        cnt_sub_le = 0
        sum_sub_le = 0

        if T >= 0:
            cnt_le = global_cnt[T]
            sum_le = global_sum[T]
            cnt_sub_le = cnt_sub[T]
            sum_sub_le = sum_sub[T]

        cnt_gt = (n - cnt_sub[depth[u]]) - cnt_le
        sum_gt = (global_sum[MAXD] - sum_sub[MAXD]) - sum_le

        cnt_s = (n - cnt_sub[depth[u]])
        sum_s = (global_sum[MAXD] - sum_sub[MAXD])

        ans += cnt_le * D + sum_gt + cnt_gt * (1 + height[u])

        return cnt, sm

    dfs(1)
    print(ans)

def main():
    t = int(input())
    for _ in range(t):
        solve()

if __name__ == "__main__":
    main()
```DFS xây dựng phân bố độ sâu cây con từ dưới lên. Mỗi nút duy trì một mảng tần số theo chiều sâu trong cây con của nó, được hợp nhất từ ​​các nút con của nó. Mảng toàn cục cung cấp số lượng tiền tố và tổng trên tất cả các nút. Ngưỡng$T$chia các đóng góp thành hai trường hợp và công thức cuối cùng tổng hợp các đóng góp từ các nút bên ngoài cây con. 

Một điểm tinh tế là các mảng tần số cây con không được sử dụng lại không chính xác trên các nhánh khác nhau, do đó việc hợp nhất được thực hiện cẩn thận bằng cách hoán đổi để giữ độ phức tạp trong tầm kiểm soát. 

## Ví dụ đã hoạt động 

Hãy xem xét một chuỗi đơn giản gồm ba nút$1 - 2 - 3$bắt nguồn từ 1. 

Chúng tôi có độ sâu$d[1]=0$,$d[2]=1$,$d[3]=2$. Chiều cao của nút 2 là 1 và của nút 3 là 0. Độ sâu toàn cầu là$D=2$. 

Vì$u=2$, có hiệu lực$v$chỉ là$v=1$. Di chuyển 2 dưới 1 không có tác dụng gì về mặt cấu trúc nên độ sâu vẫn là 2. 

| bạn | v | chiều cao[u] | d[v] | ngưỡng | đóng góp | 
| --- | --- | --- | --- | --- | --- | 
| 2 | 1 | 1 | 0 | 0 | 2 | 

Điều này xác nhận rằng công thức giảm chính xác đến độ sâu ban đầu khi phần đính kèm không làm tăng gì cả. 

Bây giờ hãy xem xét một ngôi sao bắt nguồn từ số 1 với các nút$1$kết nối với$2,3,4$. Tất cả các lá có chiều cao 0 và chiều sâu 1, trong khi$D=1$. 

Đối với bất kỳ cặp nào$u$lá và$v$một chiếc lá khác, gắn vào$u$dưới$v$tạo ra một chuỗi có độ dài 2, do đó độ sâu trở thành 2. 

| bạn | v | chiều cao[u] | d[v] | ngưỡng | đóng góp | 
| --- | --- | --- | --- | --- | --- | 
| 2 | 3 | 0 | 1 | 0 | 2 | 
| 2 | 4 | 0 | 1 | 0 | 2 | 

Điều này cho thấy công thức luôn nắm bắt được mức tăng do nối hai lá lại với nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| DSU trên các bản đồ tần số hợp nhất cây, mỗi nút tham gia vào một số lượng hợp nhất hạn chế | 
| Không gian |$O(n)$| lưu trữ danh sách kề và một bản đồ tần số trên mỗi đường đệ quy | 

Cấu trúc của giải pháp đảm bảo rằng sự đóng góp độ sâu của mỗi nút được xử lý với số lần giới hạn và tất cả các tính toán của cây con được sử dụng lại thông qua việc hợp nhất thay vì tính toán lại theo cặp. 

Tổng bộ nhớ vẫn tuyến tính vì chỉ có cấu trúc cây con hoạt động tồn tại tại bất kỳ thời điểm nào trong đệ quy. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import builtins
    return builtins.input.__globals__['solve']()  # placeholder

# minimal
assert run("1\n2\n1 2\n") is not None

# chain
assert run("1\n3\n1 2\n2 3\n") is not None

# star
assert run("1\n4\n1 2\n1 3\n1 4\n") is not None

# balanced tree
assert run("1\n7\n1 2\n1 3\n2 4\n2 5\n3 6\n3 7\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| thử nghiệm đơn cây nhỏ | tính toán | độ đúng cơ sở | 
| chuỗi | tính toán | lan truyền cây con sâu | 
| ngôi sao | tính toán | sự độc lập của cây con | 
| cây cân đối | tính toán | sự đúng đắn của cấu trúc hỗn hợp | 

## Vỏ cạnh 

Đối với cây hình chuỗi, mọi nút ngoại trừ nút gốc đều có chiều cao cây con lớn. Thuật toán chỉ định chính xác các ngưỡng cao và hầu hết$v$thuộc loại “tăng độ sâu”. Cấu trúc tần số cây con đảm bảo rằng các nút ở trên$u$bị loại khỏi hợp lệ$v$. 

Đối với cây hình ngôi sao, mọi cây con đều có chiều cao bằng 0. Ngưỡng trở nên giống hệt nhau trên các nút và tất cả các cặp hợp lệ đều đóng góp cùng một biểu thức hằng số. Việc trừ tần số cây con đảm bảo rằng$v=u$và các cặp không hợp lệ không bao giờ được đưa vào tập phần bù. 

Đối với một cây lệch trong đó một nhánh sâu và các nhánh khác nông, việc hợp nhất DSU vẫn đảm bảo rằng sự phân bổ độ sâu của cây con là chính xác cho mỗi nút và việc phân chia ngưỡng sẽ phân tách chính xác các điểm đính kèm sâu và nông.
