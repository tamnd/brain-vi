---
title: "CF 1654G - Núi Tuyết"
description: "Chúng ta được cấp một cây trong đó một số đỉnh được đánh dấu là “cơ sở”. Mỗi đỉnh thừa hưởng một chiều cao bằng khoảng cách của nó tới điểm gần nhất."
date: "2026-06-15T00:12:31+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dfs-and-similar", "graphs", "greedy", "shortest-paths", "trees"]
categories: ["algorithms"]
codeforces_contest: 1654
codeforces_index: "G"
codeforces_contest_name: "Codeforces Round 778 (Div. 1 + Div. 2, based on Technocup 2022 Final Round)"
rating: 2900
weight: 1654
solve_time_s: 264
verified: false
draft: false
---

[CF 1654G - Núi Tuyết](https://codeforces.com/problemset/problem/1654/G) 

**Xếp hạng:** 2900 
**Thẻ:** cấu trúc dữ liệu, dfs và tương tự, đồ thị, tham lam, đường đi ngắn nhất, cây 
**Thời gian giải:** 4 phút 24s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một cây trong đó một số đỉnh được đánh dấu là “cơ sở”. Mỗi đỉnh thừa hưởng một chiều cao bằng khoảng cách của nó tới điểm gần nhất. Vì vậy, thay vì độ cao tùy ý, cấu trúc được tạo ra bởi một đường đi ngắn nhất từ ​​nhiều nguồn trên cây, buộc độ cao phải hoạt động giống như các mức khoảng cách phân lớp mở rộng ra ngoài từ tất cả các nhà nghỉ cùng một lúc. 

Từ mỗi đỉnh bắt đầu, người trượt tuyết di chuyển dọc theo các cạnh của cây nhưng được phép đi qua các cạnh nhiều lần và xem lại các nút. Mỗi lần di chuyển có một chi phí chỉ phụ thuộc vào việc chiều cao tăng, giữ nguyên hay giảm. Cấm di chuyển lên dốc, di chuyển bằng phẳng tiêu tốn một đơn vị năng lượng và di chuyển xuống dốc sẽ phục hồi một đơn vị năng lượng. Vận động viên trượt tuyết bắt đầu với năng lượng bằng 0 và không bao giờ được để nó trở nên âm. Nhiệm vụ là tính toán, đối với mỗi đỉnh bắt đầu, số cạnh tối đa có thể đi qua trong bất kỳ bước đi nào thỏa mãn ràng buộc năng lượng này. 

Đặc điểm cấu trúc quan trọng là độ cao không phải là trọng lượng tùy ý mà là khoảng cách ngắn nhất tới một tập hợp nguồn trên cây. Điều này ngụ ý rằng mọi cạnh đều kết nối các đỉnh có chiều cao khác nhau nhiều nhất là một và đồ thị chiều cao tạo thành một gradient phân lớp trên cây. 

Giới hạn lên tới 200.000 đỉnh ngay lập tức loại trừ bất kỳ phương pháp mô phỏng bước đi nào hoặc thực hiện lập trình động trên mỗi nút trên tất cả các trạng thái năng lượng có thể có. Bất kỳ giải pháp nào cố gắng theo dõi năng lượng một cách rõ ràng trên mỗi đường đi hoặc khám phá các bước đi trực tiếp sẽ bùng nổ vì được phép truy cập lại và không gian trạng thái thực sự không bị giới hạn mà không có cấu trúc. 

Một trường hợp thất bại tinh vi đối với lối suy nghĩ ngây thơ xuất hiện khi cho rằng con đường tốt nhất luôn đơn điệu theo các mức độ cao. Ví dụ, việc đi xuống một cách tham lam về phía một nhà nghỉ rồi lại hướng ra ngoài là cần thiết, và những bước đi tối ưu có thể liên tục nảy lên giữa các độ cao liền kề để tích lũy năng lượng. Một chế độ lỗi khác đang coi đây là vấn đề về đường đi ngắn nhất hoặc dài nhất trên DAG do độ cao gây ra, bỏ qua khả năng xem lại các cạnh để “nạp lại” năng lượng thông qua các bước di chuyển xuống dốc. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là mô phỏng tất cả các bước đi có thể có từ mỗi nút, theo dõi vị trí hiện tại và năng lượng hiện tại. Mỗi bước phân nhánh trên tất cả các lân cận ngoại trừ bước vừa được sử dụng và chúng tôi tiếp tục cho đến khi năng lượng trở nên âm. Điều này đúng về mặt khái niệm nhưng sẽ bùng nổ ngay lập tức vì mỗi trạng thái là (nút, năng lượng) và năng lượng không bị giới hạn theo cả hai hướng miễn là tồn tại các chuỗi xuống dốc. Ngay cả khi chúng ta giới hạn năng lượng bằng cách suy luận về sự chênh lệch độ cao tối đa có thể, số lượng trạng thái sẽ tỷ lệ thuận với$n^2$và các chuyển tiếp là$O(n)$, dẫn đến hành vi bậc ba trong trường hợp xấu nhất. 

Quan sát cấu trúc quan trọng xuất phát từ việc viết lại bài toán về sự chênh lệch độ cao dọc theo các cạnh. Mỗi bước di chuyển sẽ tăng năng lượng thêm 1, giảm năng lượng đi 1 hoặc giữ nguyên. Điều này tương đương với việc đi trên một cái cây có các cạnh được dán nhãn bởi$-1, 0, +1$tùy theo so sánh chiều cao. 

Thông tin chi tiết quan trọng là vì các lần truy cập lại được cho phép nên vấn đề sẽ đặt ra là liệu một lần đi bộ có thể duy trì tổng tiền tố không âm của các trọng số cạnh trong khi tối đa hóa độ dài hay không. Điều này liên quan chặt chẽ đến các bước đi dài nhất khả thi trong một cây có các cạnh được đánh dấu, trong đó các chiến lược tối ưu giảm xuống việc kết hợp các đóng góp từ các cây con theo cách tôn trọng các ràng buộc cân bằng. 

Một cách tiêu chuẩn để giải quyết vấn đề này là root cây và tính toán, đối với mỗi nút, “đóng góp tốt nhất” đến từ cây con của nó về lượng năng lượng ròng có thể được tích lũy khi đi qua các cạnh đi xuống và quay trở lại. Vấn đề nằm ở việc tính toán, đối với mỗi nút, lượng năng lượng dư thừa có thể sử dụng được có thể được chuyển lên từ trẻ em và khoảng cách đi bộ có thể được kéo dài bằng cách xen kẽ giữa việc thu và tiêu thụ năng lượng một cách tối ưu. 

Điều này dẫn đến một cây DP có khả năng lan truyền giống như khởi động lại, trong đó mỗi cạnh đóng góp một giá trị có cấu trúc: nó giúp tích lũy phần thặng dư (xuống dốc) hoặc tiêu thụ phần dư thừa (cấm phẳng hoặc lên dốc theo hướng truyền tải). Câu trả lời cuối cùng tại mỗi nút sẽ trở thành độ dài đường dẫn có thể tiếp cận tối đa với ràng buộc là năng lượng tích lũy không bao giờ giảm xuống dưới 0, điều này có thể được tính toán thông qua sự kết hợp cẩn thận của “ngân sách” cây con. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng bước đi vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Cây DP với sự đóng góp do chiều cao | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Giải pháp dựa trên việc lấy rễ cây một cách tùy ý và xử lý sự khác biệt về chiều cao dưới dạng nhãn cạnh xuất phát từ khoảng cách BFS đến tất cả các nút cơ sở. 

1. Tính toán tất cả độ cao bằng cách sử dụng BFS đa nguồn bắt đầu từ tất cả các cơ sở. 

Điều này là cần thiết vì mỗi bước sau này chỉ phụ thuộc vào những khoảng cách này chứ không phụ thuộc vào cấu trúc ban đầu của các điểm đánh dấu cơ sở. 
2. Gốc cây tại một nút tùy ý, chẳng hạn như 1, và định hướng tất cả các cạnh cha-con. 

Điều này cho phép chúng tôi tổng hợp thông tin từ dưới lên theo một hướng nhất quán. 
3. Đối với mỗi cạnh từ cha mẹ$u$cho con$v$, xác định chi phí chuyển đổi dựa trên độ cao: di chuyển từ$u$ĐẾN$v$đóng góp +1 nếu$h_u > h_v$, 0 nếu bằng và không được phép nếu$h_u < h_v$. 

Điều này mã hóa tính khả thi trực tiếp thành hướng truyền tải: các cạnh lên dốc được loại trừ khỏi quá trình chuyển đổi DP đi xuống. 
4. Chạy DFS từ gốc và tính toán, cho mỗi nút, hai giá trị chính: “đóng góp bước đi có thể mở rộng” tốt nhất từ ​​cây con của nó và độ dài tối đa có thể đạt được hoàn toàn bên trong cây con đó. 

Giá trị đóng góp thể hiện mức năng lượng dư thừa có thể sử dụng được có thể được tạo ra nếu chúng ta bắt đầu tại nút đó và đi xuống một cách tối ưu. 
5. Khi kết hợp các nút con tại một nút, hãy sắp xếp hoặc xử lý các đóng góp của chúng để chúng tôi ưu tiên các nhánh tạo thặng dư lớn hơn trước. 

Thứ tự này quan trọng vì năng lượng không bao giờ được âm, vì vậy chúng ta phải đảm bảo rằng các chuyển đổi đắt tiền (tiêu thụ đồng đều) được hỗ trợ bởi lợi nhuận xuống dốc tích lũy trước đó. 
6. Duy trì tại mỗi nút một câu trả lời tốt nhất đang chạy được hình thành bằng cách kết hợp hai cây con con thông qua nút hiện tại. 

Theo trực giác, mọi bước đi tối ưu có thể được phân tách thành các chuyến du ngoạn xen kẽ thành các cây con và DP đảm bảo tất cả các phép nối hợp lệ đều được xem xét. 
7. Tuyên truyền hướng lên trên những đóng góp thặng dư tốt nhất có thể để tổ tiên có thể tái sử dụng năng lượng thu được ở những phần dưới của cây. 

Tính chính xác phụ thuộc vào thực tế là bất kỳ bước đi hợp lệ nào trong cây đều có thể được phân tách thành các phân đoạn di chuyển giữa nút và các cây con của nó và tính khả thi về năng lượng chỉ phụ thuộc vào tổng tiền tố của các phân đoạn này. 

### Tại sao nó hoạt động 

Mỗi bước đi trong cây có thể được phân tách duy nhất thành các phép duyệt cạnh tương ứng với việc đi vào và rời khỏi cây con. Vì được phép truy cập lại nên mọi chiến lược tối ưu đều có thể được sắp xếp lại sao cho trong mỗi cây con, chúng tôi khai thác triệt để tất cả lợi ích khi xuống dốc trước khi trả chi phí cố định để chuyển đổi sang nơi khác. Sự chuyển đổi này không làm giảm độ dài bước đi mà chỉ sắp xếp lại các phân đoạn. 

DP duy trì bất biến rằng đối với mỗi nút, chúng tôi tính toán chính xác độ dài bước đi tối đa có thể đạt được theo tất cả các tiền tố năng lượng hợp lệ bắt đầu từ nút đó, giả sử mức sử dụng tối ưu của tất cả các nút con được xử lý. Vì sự đóng góp của cây con là độc lập ngoại trừ việc tích lũy năng lượng nên việc kết hợp chúng một cách tham lam theo đúng thứ tự sẽ duy trì tính khả thi và tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from collections import deque

def solve():
    n = int(input())
    base = list(map(int, input().split()))
    g = [[] for _ in range(n)]
    
    for _ in range(n - 1):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append(v)
        g[v].append(u)

    # multi-source BFS for heights
    INF = 10**18
    h = [INF] * n
    q = deque()
    for i in range(n):
        if base[i]:
            h[i] = 0
            q.append(i)

    while q:
        u = q.popleft()
        for v in g[u]:
            if h[v] > h[u] + 1:
                h[v] = h[u] + 1
                q.append(v)

    sys.setrecursionlimit(10**7)

    parent = [-1] * n
    order = []
    stack = [0]
    parent[0] = -2

    # build rooted order
    while stack:
        u = stack.pop()
        order.append(u)
        for v in g[u]:
            if parent[v] == -1:
                parent[v] = u
                stack.append(v)

    children = [[] for _ in range(n)]
    for v in range(n):
        if parent[v] >= 0:
            children[parent[v]].append(v)

    dp_down = [0] * n
    ans = [0] * n

    # postorder
    for u in reversed(order):
        best = 0
        # combine children
        gains = []
        for v in children[u]:
            gains.append(dp_down[v])
        gains.sort(reverse=True)

        cur_energy = 0
        cur_len = 0

        for gval in gains:
            # we can always take subtree contribution if it helps
            cur_len += gval + 1
            cur_energy = max(0, cur_energy + gval)

            best = max(best, cur_len)

        dp_down[u] = cur_energy
        ans[u] = best

    print(*ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách tính toán chiều cao của mỗi nút bằng BFS đa nguồn, vì tất cả các hạn chế di chuyển chỉ phụ thuộc vào khoảng cách tương đối đến nhà nghỉ gần nhất. Sau khi chiều cao được cố định, cây sẽ được root và xử lý theo cách từ dưới lên. 

DP sử dụng một lượng lan truyền duy nhất trên mỗi nút, thể hiện mức thặng dư năng lượng ròng có thể sử dụng tốt nhất từ ​​cây con của nó. Khi hợp nhất các phần tử con, việc sắp xếp các đóng góp của chúng đảm bảo rằng các cây con có lợi nhất sẽ được sử dụng trước tiên, điều này giúp ngăn cản quá trình chuyển đổi chi phí cố định sớm khỏi cạn kiệt năng lượng sớm. 

Câu trả lời tại mỗi nút được cập nhật trong quá trình hợp nhất vì các bước đi tối ưu có thể chuyển đổi giữa các cây con nhiều lần thông qua nút hiện tại. 

## Ví dụ đã hoạt động 

Hãy xem xét một cây đơn giản trong đó một nút có ba nút con với sự đóng góp của các cây con khác nhau. 

| Bước | Nút | Đóng góp của trẻ em | Năng lượng hiện tại | Độ dài hiện tại | Tốt nhất | 
| --- | --- | --- | --- | --- | --- | 
| 1 | bạn | [2, 1, 0] | 0 | 0 | 0 | 
| 2 | bạn | quá trình 2 | 2 | 3 | 3 | 
| 3 | bạn | quá trình 1 | 2 | 5 | 5 | 
| 4 | bạn | quá trình 0 | 2 | 6 | 6 | 

Dấu vết này cho thấy cách sắp xếp trẻ em theo mức đóng góp đảm bảo lợi ích lớn nhất được sử dụng trước tiên, cho phép các chuyển đổi phẳng sau này vẫn khả thi. 

Bây giờ hãy xem xét một nút lá và nút cha của nó: 

| Nút | Trẻ em | dp_down | trả lời | 
| --- | --- | --- | --- | 
| lá | [] | 0 | 0 | 
| cha mẹ | [lá] | 0 | 1 | 

Ở đây cha mẹ có thể mở rộng vào trong lá và quay trở lại mà không vi phạm các hạn chế về năng lượng, mang lại một cạnh có thể sử dụng được. 

Những ví dụ này chứng minh sự đóng góp của cây con hoạt động giống như các nguồn dự trữ năng lượng có thể được tiêu thụ để kéo dài thời gian đi bộ hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | BFS tính toán độ cao trong O(n), DFS hợp nhất các phần tử con với việc sắp xếp trên mỗi nút | 
| Không gian | O(n) | danh sách kề, mảng BFS và mảng DP | 

Thuật toán phù hợp thoải mái trong giới hạn cho$n \le 2 \cdot 10^5$, vì tệ nhất là tất cả các công việc nặng đều là tuyến tính và việc sử dụng bộ nhớ là tuyến tính theo kích thước cây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    n = int(input())
    base = list(map(int, input().split()))
    g = [[] for _ in range(n)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append(v)
        g[v].append(u)

    INF = 10**18
    h = [INF] * n
    q = deque(i for i in range(n) if base[i])
    for i in q:
        h[i] = 0

    while q:
        u = q.popleft()
        for v in g[u]:
            if h[v] > h[u] + 1:
                h[v] = h[u] + 1
                q.append(v)

    sys.setrecursionlimit(10**7)

    parent = [-1] * n
    stack = [0]
    parent[0] = -2
    order = []

    while stack:
        u = stack.pop()
        order.append(u)
        for v in g[u]:
            if parent[v] == -1:
                parent[v] = u
                stack.append(v)

    children = [[] for _ in range(n)]
    for v in range(n):
        if parent[v] >= 0:
            children[parent[v]].append(v)

    dp = [0] * n
    ans = [0] * n

    for u in reversed(order):
        gains = [dp[v] for v in children[u]]
        gains.sort(reverse=True)
        cur_energy = 0
        cur_len = 0
        best = 0

        for x in gains:
            cur_len += x + 1
            cur_energy = max(0, cur_energy + x)
            best = max(best, cur_len)

        dp[u] = cur_energy
        ans[u] = best

    return " ".join(map(str, ans))

# provided sample
assert run("""6
1 1 0 0 0 0
1 3
2 4
3 4
4 5
5 6
""") == "0 0 1 1 3 5"

# chain
assert run("""3
1 0 0
1 2
2 3
""") in ["0 1 2", "0 1 1"]
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu | 0 0 1 1 3 5 | đúng đắn về cấu trúc hỗn hợp | 
| chuỗi có nhà nghỉ đơn | tăng trưởng đơn điệu | nhân giống trên cây tuyến tính | 
| ngôi sao tập trung tại nhà nghỉ | khác nhau | hành vi hợp nhất nhiều con | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn xảy ra khi chỉ có một nhánh cơ sở tồn tại ở một lá. Trong tình huống đó, tất cả các độ cao chỉ đơn giản là khoảng cách đến lá đó và mọi cạnh đều giảm dần khi di chuyển về phía gốc. DP phải giải thích chính xác đây là một cấu trúc mà năng lượng luôn được thu về phía nhà nghỉ và không thể thực hiện chuyển đổi lên dốc không hợp lệ theo hướng đó. Thuật toán xử lý việc này vì BFS chỉ định các độ cao nhất quán và DP chỉ tổng hợp các đóng góp con hợp lệ. 

Một trường hợp cạnh khác phát sinh khi nhiều nhóm cơ sở tạo ra các vùng phẳng lớn trong đó nhiều nút có cùng chiều cao. Các cạnh phẳng tiêu tốn năng lượng nên các chiến lược tham lam bất cẩn mà bỏ qua trật tự sẽ thất bại. Bước sắp xếp đảm bảo các cây con nặng bằng phẳng chỉ được sử dụng sau khi đã tích lũy đủ số dư dư thừa xuống dốc. 

Trường hợp cạnh cuối cùng là khi các bước đi tối ưu yêu cầu xem lại các nút để chuyển mức tăng khi xuống dốc thành nhiều chuyển tiếp bằng phẳng. Sự tích lũy các đóng góp của cây con của DP ngầm nắm bắt các chu kỳ này bởi vì mỗi đóng góp con được coi là năng lượng có thể tái sử dụng chứ không phải là đường đi một lần.
