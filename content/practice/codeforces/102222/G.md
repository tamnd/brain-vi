---
title: "CF 102222G - Nhà máy"
description: "Chúng ta có một cây thành phố có trọng số. Một nhà máy chỉ có thể được đặt trên một chiếc lá ban đầu, nghĩa là một thành phố có bậc trong cây vô hướng chính xác là một."
date: "2026-08-17T22:10:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102222
codeforces_index: "G"
codeforces_contest_name: "2018-2019 ACM-ICPC, China Multi-Provincial Collegiate Programming Contest"
rating: 0
weight: 102222
solve_time_s: 188
verified: true
draft: false
---

[CF 102222G - Nhà máy](https://codeforces.com/problemset/problem/102222/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 8 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây thành phố có trọng số. Một nhà máy chỉ có thể được đặt trên một chiếc lá ban đầu, nghĩa là một thành phố có bậc trong cây vô hướng chính xác là một. Chúng ta phải lựa chọn chính xác`k`các lá riêng biệt và giảm thiểu tổng khoảng cách đường đi ngắn nhất trên tất cả các cặp lá được chọn không có thứ tự. 

Đầu vào chứa tối đa`10^3`trường hợp thử nghiệm, với`n`lên đến`10^5`cho một trường hợp thử nghiệm và tổng của tất cả`n`giới hạn bởi`10^6`. Số lượng nhà máy`k`nhiều nhất là`100`, đây là tham số tạo nên giải pháp quy hoạch động. Giới hạn chính thức là 10 giây và 256 MB. 

Giá trị lớn của`n`loại trừ bất cứ điều gì bậc hai về số lượng thành phố. Thậm chí`O(nk^2)`đạt khoảng`10^9`chuyển tiếp cơ bản khi cả hai`n=10^5`Và`k=100`, vì vậy việc triển khai cần phải giữ chặt chẽ phạm vi trạng thái thực tế thay vì lặp lại một cách mù quáng trên tất cả`k`trạng thái tại mỗi nút. Việc tìm kiếm theo cấp số nhân trên các tập nhà máy có thể là hoàn toàn không thể vì số lượng lá có thể gần bằng`10^5`. 

Trường hợp cạnh đầu tiên là`k=1`. Không có cặp nhà máy nào nên câu trả lời là 0 bất kể cây nào.```
1
2 1
1 2 7
```Đầu ra đúng là`Case #1: 0`. Một giải pháp vô tình cộng thêm khoảng cách từ một nhà máy đến chính nó sẽ tạo ra một giá trị khác 0. 

Trường hợp cạnh thứ hai là cây nhỏ nhất có thể có hai thành phố. Cả hai thành phố đều là những chiếc lá, mặc dù một trong số chúng có thể được chọn làm gốc cho việc triển khai của chúng tôi.```
1
2 2
1 2 7
```Đầu ra đúng là`Case #1: 7`. Việc triển khai cây gốc bất cẩn có thể chỉ phân loại nút con là lá có thể chọn và làm mất một vị trí nhà máy hợp lệ. 

Trường hợp cạnh thứ ba là chỉ có lá gốc mới đủ điều kiện. Hãy xem xét một con đường.```
1
4 2
1 2 1
2 3 1
3 4 1
```Các nhà máy duy nhất có thể là các thành phố`1`Và`4`, vậy câu trả lời là`3`. Một giải pháp coi mọi đỉnh là một nhà máy khả thi có thể chọn sai các thành phố lân cận và thu được`1`. 

Trường hợp cạnh thứ tư xảy ra khi tất cả các lá phải được chọn.```
1
4 3
1 2 2
1 3 3
1 4 4
```Ba lá bị ép buộc. Khoảng cách theo cặp của chúng là`5`,`6`, Và`7`, cho`18`. Một DP tính toán phần đóng góp của một cạnh chỉ sử dụng số lượng lá được chọn bên dưới nó mà quên số lượng lá được chọn bên ngoài nó, sẽ mắc sai lầm trong trường hợp này. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là liệt kê mọi tập hợp`k`lá, tính tổng khoảng cách bên trong tập hợp đó và giữ mức tối thiểu. Nếu có`L`lá, điều này kiểm tra`C(L,k)`bộ nhà máy có thể. Ngay cả khi tất cả các khoảng cách lá theo cặp đều có sẵn trước, mỗi tập ứng cử viên vẫn yêu cầu`C(k,2)`đánh giá cặp đôi. Trong hình dạng xấu nhất cho phép, một ngôi sao có`L=99999`lá, vì vậy đối với`k=100`riêng số lượng đánh giá cặp là`C(99999,100) * C(100,2)`, 

vượt xa mọi tính toán thực tế. Việc tính toán trước mọi khoảng cách từ lá này sang lá khác cũng sẽ yêu cầu bộ nhớ bậc hai. 

Quan sát hữu ích là cây cho phép chúng ta đếm sự đóng góp của mọi cạnh một cách độc lập. Sửa một bộ`k`rời khỏi nhà máy và loại bỏ một phần trọng lượng`w`. Giả sử chính xác`x`những chiếc lá đã chọn nằm ở một bên mép. Thế thì chính xác`k-x`những chiếc lá đã chọn nằm ở phía bên kia. Mỗi cặp bao gồm một lá từ mỗi phía đều có một đường đi chứa cạnh này, do đó cạnh đó được sử dụng chính xác`x(k-x)`cặp nhà máy. Do đó, đóng góp của nó cho câu trả lời tổng thể là`w * x * (k-x)`. 

Điều này chuyển đổi tổng trên các cặp lá thành tổng trên các cạnh. Công thức đóng góp cạnh tương tự là quan sát trung tâm được sử dụng bởi giải pháp tiêu chuẩn cho vấn đề này. 

Bây giờ hãy nhổ cây. Đối với mỗi nút`u`, chỉ xem xét các lá trong cây con có gốc của nó. Định nghĩa`dp[u][j]`là sự đóng góp tối thiểu của tất cả các cạnh hoàn toàn bên trong cây con đó khi chính xác`j`các nhà máy được lựa chọn ở đó. Cây cha không cần biết lá nào đã được chọn, chỉ cần biết bao nhiêu lá, vì cạnh duy nhất nối cây con với phần còn lại của cây là cạnh cha. 

Giả định`v`là con của`u`, và cạnh`u-v`có trọng lượng`w`. Nếu chúng ta lấy`x`nhà máy từ`v`cây con của, cạnh đó đóng góp`w*x*(k-x)`. Nếu phần đã được xử lý của`u`chứa`j-x`các nhà máy, quá trình chuyển đổi trở thành`dp[u][j] = min(dp[u][j], dp[u][j-x] + dp[v][x] + w*x*(k-x))`. 

Đây là một chiếc ba lô hình cây. Quá trình chuyển đổi chính xác là sự hợp nhất cây con tiêu chuẩn được mô tả trong các giải pháp hiện có cho vấn đề này. 

Phương pháp brute-force hoạt động vì mọi tập hợp xuất xưởng có thể đều được xem xét rõ ràng, nhưng nó thất bại vì có nhiều tập hợp như vậy theo cấp số nhân. Quan sát đóng góp cạnh cho phép chúng ta quên danh tính của các lá được chọn trong khi xử lý cây con và chỉ giữ lại số lượng của chúng. Từ`k`nhiều nhất là`100`, điều này biến vấn đề thành một chương trình động bị chặn. 

Trường hợp xấu nhất tiêu chuẩn bị ràng buộc của phép tái diễn tree-knapsack là`O(nk^2)`. Việc triển khai bên dưới cải thiện đáng kể hành vi thực tế bằng cách chỉ lưu trữ các trạng thái có thể truy cập và bằng cách hạn chế mỗi lần chuyển đổi sang phạm vi hợp lệ. Đặc biệt, một chuỗi dài phía trên cây con phân nhánh không quét liên tục các trạng thái không thể thực hiện được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(C(L,k) * k^2)`sau khi xử lý trước khoảng cách |`O(L^2)`nếu tất cả khoảng cách lá được lưu trữ | Quá chậm | 
| DP tối ưu |`O(nk^2)`trường hợp xấu nhất, với việc cắt tỉa trạng thái có thể tiếp cận chặt chẽ |`O(nk)`trường hợp xấu nhất | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Rễ cây tại thành phố`1`và xây dựng thứ tự duyệt qua cha mẹ trước con cái. Phép duyệt lặp được sử dụng vì cây có thể là một đường đi có độ dài`10^5`, điều này sẽ làm cho Python DFS đệ quy dễ bị giới hạn độ sâu đệ quy. 
2. Ghi lại mức độ ban đầu của mỗi thành phố. Một thành phố đóng góp một trạng thái nhà máy có thể lựa chọn chính xác khi mức độ ban đầu của nó được`1`. Gốc được xử lý theo quy tắc tương tự, quy tắc này cần thiết cho cây hai thành phố trong đó cả hai đỉnh đều là lá. 
3. Xử lý các đỉnh theo thứ tự truyền tải ngược, do đó mọi nút con đã tính toán DP của nó trước khi nút cha của nó được xử lý. Đối với nút không có lá, khởi tạo DP của nó là`[0]`, thể hiện sự lựa chọn chọn lá 0 từ cây con. Đối với một chiếc lá, khởi tạo`[0, 0]`, trạng thái ở đâu`1`có nghĩa là chọn lá đó và có chi phí bằng 0 bên trong cây con. 
4. Dành cho mọi trẻ em`v`của`u`, hợp nhất`dp[v]`vào trong`dp[u]`. Giả sử hiện tại`u`tiểu bang hỗ trợ nhiều nhất`a`lá được chọn và đứa trẻ hỗ trợ nhiều nhất`b`. Để có được trạng thái chứa`j`nhà máy, hãy thử mọi số hợp lệ`x`lấy từ đứa trẻ. Ứng viên là`dp[u][j-x] + dp[v][x] + w*x*(k-x)`. 
5. Thực hiện`j`vòng lặp ngược lại. Mảng DP hiện tại được cập nhật tại chỗ, do đó giảm dần`j`đảm bảo rằng`dp[u][j-x]`vẫn đề cập đến các trạng thái từ trước khi đứa trẻ này được sáp nhập. Đây cũng là lý do tại sao ba lô 0/1 một chiều xử lý ngược dung lượng. 
6. Không lặp lại các giá trị không thể. Nếu phần hiện tại chứa nhiều nhất`a`lá có thể lựa chọn, sau đó là một trạng thái`j-x`lớn hơn`a`không thể tồn tại. Tương tự như vậy,`x`không thể vượt quá số lượng trạng thái được đại diện bởi đứa trẻ. Việc cắt tỉa này đặc biệt hữu ích đối với các chuỗi dài, nơi việc triển khai đơn giản có thể dành phần lớn thời gian để thử nghiệm các trạng thái có giá trị vô cực. 
7. Sau khi tất cả các phần tử con của thư mục gốc đã được hợp nhất,`dp[root][k]`là mức đóng góp tối thiểu có thể có của mỗi cạnh cho chính xác`k`lá đã chọn. In giá trị đó làm câu trả lời cho trường hợp thử nghiệm. 

### Tại sao nó hoạt động 

Đối với bất kỳ lựa chọn nhà máy cố định nào, mỗi cạnh đóng góp độc lập tùy theo số lượng lá được chọn nằm ở hai bên của nó. Trạng thái DP ghi lại chính xác thông tin mà nút cha cần, cụ thể là số lá được chọn trong cây con và mức đóng góp tối thiểu của tất cả các cạnh bên dưới cây con đó. Khi một đứa trẻ được hợp nhất, mọi số có thể`x`của các lá được chọn từ con đó được xem xét và cạnh kết nối nhận được chính xác sự đóng góp`w*x*(k-x)`bị ép buộc bởi sự lựa chọn đó. Do đó, mọi tập hợp xuất xưởng hợp lệ tương ứng với một chuỗi các chuyển đổi DP và mỗi chuyển đổi DP thể hiện sự kết hợp hợp lệ của các lựa chọn xuất xưởng từ các cây con rời rạc. Lấy mức tối thiểu trong tất cả các chuyển đổi sẽ mang lại mức tối ưu. 

## Giải pháp Python```python
import sys
from array import array

input = sys.stdin.readline

INF = 10**18

def solve():
    input = sys.stdin.readline
    t = int(input())
    out = []

    for case_id in range(1, t + 1):
        n, k = map(int, input().split())

        # Compact forward-star adjacency representation.
        head = array('i', [-1]) * n
        to = array('i')
        weight = array('i')
        nxt = array('i')

        degree = array('i', [0]) * n

        for _ in range(n - 1):
            u, v, w = map(int, input().split())
            u -= 1
            v -= 1

            idx = len(to)
            to.append(v)
            weight.append(w)
            nxt.append(head[u])
            head[u] = idx

            idx = len(to)
            to.append(u)
            weight.append(w)
            nxt.append(head[v])
            head[v] = idx

            degree[u] += 1
            degree[v] += 1

        # Root the tree at 0 and construct a preorder.
        parent = array('i', [-2]) * n
        parent[0] = -1
        parent_weight = array('i', [0]) * n
        order = [0]

        for u in order:
            e = head[u]
            while e != -1:
                v = to[e]
                if v != parent[u] and parent[v] == -2:
                    parent[v] = u
                    parent_weight[v] = weight[e]
                    order.append(v)
                e = nxt[e]

        # dp[u] is an array indexed by the number of selected leaves.
        # Only reachable states are stored.
        dp = [None] * n

        for u in reversed(order):
            if degree[u] == 1:
                cur = 1
                du = array('q', [0, 0])
            else:
                cur = 0
                du = array('q', [0])

            e = head[u]

            while e != -1:
                v = to[e]

                if parent[v] == u:
                    dv = dp[v]
                    child_lim = len(dv) - 1

                    new_lim = cur + child_lim
                    if new_lim > k:
                        new_lim = k

                    if new_lim > cur:
                        du.extend([INF] * (new_lim - cur))

                    w = parent_weight[v]

                    # Descending j keeps du[j-x] unchanged during
                    # this child merge.
                    for j in range(new_lim, 0, -1):
                        if j <= cur:
                            best = du[j]
                        else:
                            best = INF

                        lo = j - cur
                        if lo < 1:
                            lo = 1

                        hi = child_lim
                        if hi > j:
                            hi = j

                        for x in range(lo, hi + 1):
                            cand = du[j - x] + dv[x] + w * x * (k - x)
                            if cand < best:
                                best = cand

                        du[j] = best

                    cur = new_lim

                    # The child DP is no longer needed after this merge.
                    dp[v] = None

                e = nxt[e]

            dp[u] = du

        out.append(f"Case #{case_id}: {dp[0][k]}")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Cấu trúc kề sử dụng bốn mảng số nguyên nhỏ gọn thay vì các bộ dữ liệu Python và các danh sách lồng nhau. Với tối đa`10^5`thành phố cho mỗi trường hợp thử nghiệm và giới hạn bộ nhớ 256 MB, điều này quan trọng vì chi phí hoạt động của đối tượng Python có thể trở nên đáng kể. 

Việc xây dựng cha mẹ được lặp đi lặp lại.`order`chứa các đỉnh theo thứ tự truyền tải và việc đảo ngược nó sẽ mang lại chính xác thứ tự sau mà DP yêu cầu.`parent_weight[v]`lưu trữ trọng lượng của cạnh kết nối`v`về phía cha mẹ của nó, do đó pha DP không cần tìm kiếm lại trọng số đó. 

Mảng DP cho một nút nội bộ chỉ bắt đầu với trạng thái 0. Một chiếc lá bắt đầu với trạng thái 0 và 1. Sự phân biệt sử dụng mức độ ban đầu chứ không phải số lượng con gốc, bởi vì tính đủ điều kiện của nhà máy được xác định bởi cây vô hướng ban đầu. 

Phần tinh tế nhất là hợp nhất tại chỗ. Giả sử DP cũ chứa các trạng thái thông qua`cur`. Sau khi mở rộng mảng, các trạng thái trên`cur`được khởi tạo đến vô cùng. Đối với trạng thái mục tiêu`j`, giới hạn dưới`j-cur`TRÊN`x`đảm bảo rằng`j-x`là một trạng thái cũ có thể truy cập được. Lặp lại`j`từ lớn đến nhỏ ngăn không cho trạng thái đã cập nhật được sử dụng lại trong cùng một lần hợp nhất con. 

Thuật ngữ`w * x * (k - x)`sử dụng số lượng nhà máy được yêu cầu trên toàn cầu`k`, không phải số hiện được chọn bên trong cây con được xử lý. Sự mất tích`k-x`các nhà máy nhất thiết phải nằm ngoài cây con con, vì vậy chính xác các cặp đó sẽ vượt qua cạnh cha. 

Tất cả số học được thực hiện với bộ lưu trữ 64 bit trong mảng DP. Câu trả lời tối đa có thể thoải mái dưới đây`10^18`, bởi vì có nhiều nhất`C(100,2)`cặp nhà máy và khoảng cách mỗi cây tối đa`(n-1)*10^5`. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Cây là ngôi sao ở trung tâm thành phố`1`, với các cạnh lá có trọng lượng`2`,`3`, Và`4`. Chúng tôi cần hai nhà máy. 

Vì`k=2`, chọn một lá bên dưới một cạnh có trọng lượng`w`đóng góp`w*1*(2-1)=w`. 

| Con đã được xử lý | Trọng lượng cạnh |`dp[0]`|`dp[1]`|`dp[2]`| 
| --- | --- | --- | --- | --- | 
| không | |`0`|`INF`|`INF`| 
| thành phố 2 |`2`|`0`|`2`|`INF`| 
| thành phố 3 |`3`|`0`|`2`|`5`| 
| thành phố 4 |`4`|`0`|`2`|`5`| 

Sau thành phố`2`, chi phí một nhà máy`2`. Sau thành phố`3`, hai nhà máy có thể được đặt tại các thành phố`2`Và`3`, tính chi phí`2+3=5`. Thêm thành phố`4`không thể cải thiện giá trị đó vì hai lá rẻ nhất vẫn là thành phố`2`Và`3`. Câu trả lời là`5`. 

### Mẫu 2 

Ngôi sao tương tự được sử dụng, nhưng bây giờ`k=3`. Mỗi lá được chọn đóng góp cạnh của nó một lần cho mỗi nhà máy trong số hai nhà máy bên ngoài cạnh đó, do đó, một lá trên cạnh có trọng lượng`w`đóng góp`2w`. 

| Con đã được xử lý | Trọng lượng cạnh |`dp[0]`|`dp[1]`|`dp[2]`|`dp[3]`| 
| --- | --- | --- | --- | --- | --- | 
| không | |`0`|`INF`|`INF`|`INF`| 
| thành phố 2 |`2`|`0`|`4`|`INF`|`INF`| 
| thành phố 3 |`3`|`0`|`4`|`10`|`INF`| 
| thành phố 4 |`4`|`0`|`4`|`10`|`18`| 

Trạng thái cuối cùng chọn cả ba lá. Đóng góp cạnh của nó là`4`,`6`, Và`8`, cho`18`. Điều này khẳng định yếu tố`k-x`phải sử dụng tổng số nhà máy được yêu cầu chứ không chỉ số lượng đã có trong phần được xử lý. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(nk^2)`trường hợp xấu nhất | Mỗi lần hợp nhất con là một tích chập ba lô giới hạn nhiều nhất`k`tiểu bang | 
| Không gian |`O(nk)`trường hợp xấu nhất | Trạng thái DP được lưu trữ trên mỗi nút, với giá trị 64 bit nhỏ gọn | 

Những ràng buộc cố tình giữ`k`chỉ ở mức`100`, trong khi tổng số thành phố nhiều nhất là`10^6`. Việc triển khai tiếp tục hạn chế mỗi lần hợp nhất ở các trạng thái thực sự có thể đạt được và loại bỏ DP con ngay sau khi hợp nhất. Nó cũng tránh được DFS đệ quy và sử dụng các mảng nhỏ gọn để lưu trữ đồ thị và DP lớn. Phép truy toán chuẩn và công thức đóng góp cạnh của nó phù hợp với lời giải đã được thiết lập cho bài toán. 

## Trường hợp thử nghiệm 

Khai thác thử nghiệm sau đây giả định giải pháp đã gửi được lưu dưới dạng`solution.py`và phơi bày`solve()`chức năng hiển thị ở trên.```python
# helper: run solution on input string, return output string
import sys
import io
from solution import solve

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided samples
sample = """\
2
4 2
1 2 2
1 3 3
1 4 4
4 3
1 2 2
1 3 3
1 4 4
"""
assert run(sample) == "Case #1: 5\nCase #2: 18", "provided samples"

# Minimum-size tree, k = 1.
assert run("""\
1
2 1
1 2 7
""") == "Case #1: 0", "one factory has no pairwise distance"

# Minimum-size tree, both cities are leaves.
assert run("""\
1
2 2
1 2 7
""") == "Case #1: 7", "both vertices must be recognized as leaves"

# All leaf edges have equal weight.
assert run("""\
1
5 3
1 2 1
1 3 1
1 4 1
1 5 1
""") == "Case #1: 6", "three selected leaves have three pairs of distance 2"

# Only two leaves exist, so both endpoints of the path are forced.
assert run("""\
1
5 2
1 2 1
2 3 2
3 4 3
4 5 4
""") == "Case #1: 10", "internal vertices are not eligible factories"

# Maximum-size star, with k = 100 and all edge weights equal to 1.
# Every pair of selected leaves has distance 2.
n = 100000
edges = "\n".join(f"1 {v} 1" for v in range(2, n + 1))
max_case = f"1\n{n} 100\n{edges}\n"
assert run(max_case) == "Case #1: 9900", "maximum-size stress case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 1`, một cạnh của trọng lượng`7`|`0`| Trường hợp ranh giới`k=1`| 
|`2 2`, một cạnh của trọng lượng`7`|`7`| Cả hai đỉnh đều là lá | 
| Ngôi sao có trọng lượng đơn vị năm nút,`k=3`|`6`| Giá trị bằng nhau và đếm cặp chính xác | 
| Đường dẫn có trọng số năm nút,`k=2`|`10`| Chỉ có thể chọn lá gốc | 
|`100000`-nút có trọng lượng đơn vị sao,`k=100`|`9900`| Tối đa`n`, tối đa`k`và hiệu suất | 

## Vỏ cạnh 

cho`k=1`, hãy xem xét đầu vào```
1
2 1
1 2 7
```Lá DP chứa các trạng thái`0`Và`1`. Quá trình chuyển đổi để chọn một nhà máy thông qua cạnh duy nhất sẽ bổ sung thêm`7*1*(1-1)=0`. Do đó, gốc đạt tới`dp[1]=0`, đó là câu trả lời đúng. 

Đối với trường hợp hai thành phố với`k=2`,```
1
2 2
1 2 7
```cả hai đỉnh đều có bậc một. Root tại thành phố`1`làm nên thành phố`1`một lá gốc và thành phố có thể lựa chọn`2`một lá con có thể lựa chọn. Sau khi sáp nhập con, nhà nước hai nhà máy nhận được`7*1*(2-1)=7`. Kết quả là`7`, do đó biểu diễn gốc không làm mất lá gốc. 

Đối với một con đường,```
1
4 2
1 2 1
2 3 1
3 4 1
```thành phố`1`Và`4`là những chiếc lá duy nhất DP không thể chọn thành phố`2`hoặc`3`bởi vì mức độ ban đầu của họ là hai. Cả hai lá điểm cuối phải được chọn và khoảng cách của chúng là`1+1+1=3`. Những đóng góp cạnh cũng là`1`,`1`, Và`1`, cho tổng số như nhau. 

Đối với trường hợp mỗi lá phải được chọn,```
1
4 3
1 2 2
1 3 3
1 4 4
```gốc là bên trong và mỗi đứa trẻ là một chiếc lá. Với`k=3`, số được chọn bên dưới mỗi cạnh lá là`1`, vậy đóng góp của ba cạnh là`2*1*2=4`,`3*1*2=6`, Và`4*1*2=8`. Tổng của họ là`18`, khớp với khoảng cách theo cặp trực tiếp`5+6+7`. Đây là bất biến chính đằng sau toàn bộ DP: mỗi cặp xuất xưởng được tính phí chính xác một lần trên mỗi cạnh nằm trên đường đi của nó.
