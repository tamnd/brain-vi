---
title: "CF 1155F - Độc quyền phân phối"
description: "Chúng ta được cho một đồ thị vô hướng đơn giản đã được kết nối 2 cạnh. Theo định lý Menger, điều này có nghĩa là mỗi cặp đỉnh đều có hai đường đi khác nhau giữa chúng, đây chính xác là điều kiện mà hai công ty chuyển phát yêu cầu."
date: "2026-06-12T02:45:59+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dp", "graphs"]
categories: ["algorithms"]
codeforces_contest: 1155
codeforces_index: "F"
codeforces_contest_name: "Educational Codeforces Round 63 (Rated for Div. 2)"
rating: 2800
weight: 1155
solve_time_s: 128
verified: false
draft: false
---

[CF 1155F - Độc quyền phân phối](https://codeforces.com/problemset/problem/1155/F) 

**Đánh giá:** 2800 
**Tags:** vũ lực, dp, đồ thị 
**Thời gian giải:** 2 phút 8 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị vô hướng đơn giản đã được kết nối 2 cạnh. Theo định lý Menger, điều này có nghĩa là mỗi cặp đỉnh đều có hai đường đi khác nhau giữa chúng, đây chính xác là điều kiện mà hai công ty chuyển phát yêu cầu. 

Chính phủ muốn xóa càng nhiều con đường càng tốt trong khi vẫn giữ nguyên tính chất đó. Chúng ta phải xuất ra một tập con nhỏ nhất có thể có của các cạnh ban đầu mà đồ thị cảm ứng của nó vẫn liên thông 2 cạnh. 

Biểu đồ rất nhỏ. Có nhiều nhất 14 đỉnh, mặc dù số cạnh có thể lớn bằng một đồ thị hoàn chỉnh. Một đồ thị có nhiều nhất 14 đỉnh$2^{14}=16384$tập hợp con đỉnh, gợi ý mạnh mẽ về giải pháp lập trình động bitmask. Mặt khác, số tập con cạnh có thể lớn bằng$2^{91}$, vì vậy bất kỳ cách tiếp cận nào tìm kiếm trực tiếp trên các cạnh đều vô vọng. 

Khó khăn chính là kết nối 2 cạnh là đặc tính toàn cầu. Cây bao trùm được kết nối, nhưng mỗi cạnh của cây là một cây cầu, do đó, nó càng xa kết nối 2 cạnh càng tốt. Một DP ngây thơ chỉ theo dõi kết nối sẽ mất quá nhiều thông tin. 

Một sai lầm phổ biến là nghĩ rằng mọi đỉnh mới được thêm vào ngay lập tức phải có ít nhất hai bậc trong nghiệm hiện tại. Coi như:```
1 -- 2 -- 3 -- 4
 \___________/
```Đồ thị cuối cùng là một chu trình và được kết nối 2 cạnh, nhưng trong khi xây dựng nó, chúng ta có thể tạm thời đưa vào các đỉnh chưa được gắn hai lần. 

Một cạm bẫy khác là giả định rằng câu trả lời tối ưu luôn là một chu trình đơn đi qua tất cả các đỉnh. Nếu đồ thị ban đầu không chứa chu trình Hamilton thì đồ thị con bao trùm 2 cạnh liên thông tối thiểu có thể cần nhiều hơn$n$các cạnh. 

Ví dụ:```
1-2-3
|   |
4---5
```với các cạnh bổ sung phù hợp đảm bảo khả năng kết nối 2 cạnh trong biểu đồ gốc. Đồ thị con được bảo toàn tối ưu không nhất thiết phải là một chu trình đơn giản. 

Giá trị nhỏ của$n$là manh mối thực sự. Chúng ta cần một đặc tính cấu trúc để chuyển đổi kết nối 2 cạnh thành tập hợp con DP. 

## Phương pháp tiếp cận 

Ý tưởng về vũ lực rất đơn giản. Liệt kê mọi tập hợp con của các cạnh, kiểm tra xem nó có kéo dài tất cả các đỉnh và duy trì kết nối 2 cạnh hay không và giữ giá trị nhỏ nhất hợp lệ. 

Việc kiểm tra đồ thị ứng viên thật dễ dàng với việc tìm cầu, nhưng không gian tìm kiếm thì không. Ngay cả đối với$m=40$, có$2^{40}$tập hợp con cạnh. Trường hợp xấu nhất ở đây có$m=91$, điều đó hoàn toàn nằm ngoài tầm với. 

Bước đột phá đến từ một định lý cổ điển: mọi đồ thị 2 cạnh đều thừa nhận một sự phân rã tai. Bắt đầu từ một đỉnh duy nhất, chúng tôi liên tục thêm một "tai". Một tai là một đỉnh mới duy nhất được gắn vào biểu đồ hiện có bằng hai cạnh hoặc một đường dẫn có các đỉnh bên trong là mới và có hai điểm cuối kết nối trở lại phần đã được xây dựng. 

Mô tả đó phù hợp gần như hoàn hảo với giải pháp được chấp nhận. 

Giả sử chúng ta đã xây dựng một số tập đỉnh$S$. Một tai có thể thêm: 

1. Một đỉnh$u\notin S$có ít nhất hai người hàng xóm trong$S$. 
2. Đường đi có các đỉnh trong đều nằm ngoài$S$, trong khi cả hai điểm cuối đều có hàng xóm trong$S$. 

Mỗi tai đóng góp một số cạnh đã biết. Từ$n\le14$, chúng ta có thể sử dụng DP trên các tập hợp con đỉnh và xây dựng lại phân tách tai tối ưu. 

Nhiệm vụ còn lại là liệt kê số tai ứng viên một cách hiệu quả. Bởi vì$n$rất nhỏ, chúng ta tính toán trước mọi đường đi đơn giản bằng tập đỉnh của nó. Điều này cho phép DP đính kèm toàn bộ đường dẫn trong một lần chuyển đổi. 

Độ phức tạp thu được là khoảng$O(3^n)$, điều này dễ dàng thực hiện được đối với$n=14$. Giải pháp được sử dụng trong các bài gửi được chấp nhận tuân theo chính xác chiến lược này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên các tập hợp con cạnh |$O(2^m)$|$O(m)$| Quá chậm | 
| Tai phân hủy DP |$O(3^n)$|$O(2^n)$cộng với lưu trữ đường dẫn | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Tính toán trước 

Với mọi đỉnh$i$và mọi mặt nạ tập hợp con, hãy tính:$$\text{vtomask}[i][mask]$$trong đó đếm có bao nhiêu hàng xóm của$i$thuộc về`mask`. 

Điều này cho phép chúng tôi kiểm tra ngay lập tức xem một đỉnh có thể gắn vào phần được xây dựng hiện tại hay không. 

Chúng tôi cũng tính toán trước các đường dẫn đơn giản. 

Cho phép`road[mask][u][v]`lưu trữ một đường dẫn đơn giản từ$u$ĐẾN$v$tập đỉnh của nó chính xác là`mask`. 

Bắt đầu từ các đỉnh đơn lẻ, chúng ta mở rộng đường đi từng đỉnh một. 

### Bang DP 

Hãy:$$dp[mask]$$là số cạnh tối thiểu cần thiết để xây dựng một phân tách tai từng phần hợp lệ có tập đỉnh chính xác`mask`. 

Chúng tôi sửa đỉnh 0 làm đỉnh bắt đầu. 

Ban đầu:$$dp[1<<0]=0.$$### Chuyển tiếp 

1. Thêm một đỉnh mới$u$. 

Nếu như$u\notin mask$Và$u$có ít nhất hai người hàng xóm ở bên trong`mask`, khi đó chúng ta có thể thêm$u$như một tai một đỉnh. 

Tăng chi phí: 2. 
2. Thêm một đường tai. 

Chọn hai đỉnh$u,v\notin mask$. 

Tìm đường đi đơn có các đỉnh trong nằm ngoài`mask`. 

Đặt đỉnh của nó là`tmask`. 

Nếu như$u$có một người hàng xóm ở`mask`Và$v$có một người hàng xóm ở`mask`, chúng ta có thể kết nối đường dẫn trở lại biểu đồ hiện có. 

Một đường dẫn chứa$L$đỉnh đóng góp$L-1$các cạnh bên trong, cộng với hai cạnh đính kèm. 

Tổng mức tăng:$$(L-1)+2=L+1.$$Đây chính xác là quá trình chuyển đổi được sử dụng bởi giải pháp được chấp nhận. 
3. Thư giãn trạng thái đích:$$dp[mask\cup tmask].$$###Tái thiết 

Đối với mỗi bản cập nhật DP, chúng tôi lưu trữ: 

- mặt nạ trước đó, 
- điểm cuối được chọn$u$, 
- điểm cuối được chọn$v$(hoặc$-1$đối với tai một đỉnh). 

Sau khi đạt đến mặt nạ đầy đủ, chúng ta đi lùi qua các chuyển tiếp đã lưu và xuất ra tất cả các cạnh được sử dụng bởi tai đã chọn. 

### Tại sao nó hoạt động 

Sự phân rã tai mô tả chính xác lớp đồ thị 2 cạnh kết nối. Mọi nghiệm hợp lệ đều có thể được viết dưới dạng một dãy tai bắt đầu từ một đỉnh. Ngược lại, mọi đồ thị do DP tạo ra đều thu được bằng cách gắn nhiều lần các tai hợp lệ và do đó được kết nối 2 cạnh. 

DP xem xét mọi tai có thể có các đỉnh bên trong là mới. Vì mọi phân tách tai có thể được sắp xếp sao cho các đỉnh mới được đưa vào chỉ xuất hiện một lần, nên mọi giải pháp tối ưu đều tương ứng với một số chuỗi chuyển tiếp DP. 

DP giảm thiểu tổng số cạnh trong số tất cả các chuỗi như vậy, do đó trạng thái cuối cùng đưa ra sơ đồ con bao trùm 2 cạnh được kết nối có kích thước tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10 ** 9

def solve():
    n, m = map(int, input().split())

    g = [[0] * n for _ in range(n)]
    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        g[u][v] = g[v][u] = 1

    N = 1 << n

    vtomask = [[0] * N for _ in range(n)]
    for i in range(n):
        for mask in range(N):
            c = 0
            mm = mask
            while mm:
                b = mm & -mm
                j = b.bit_length() - 1
                if g[i][j]:
                    c += 1
                mm ^= b
            vtomask[i][mask] = c

    road = [[[None for _ in range(n)] for _ in range(n)] for _ in range(N)]

    for i in range(n):
        road[1 << i][i][i] = [i]

    for mask in range(N):
        for u in range(n):
            for v in range(n):
                p = road[mask][u][v]
                if p is None:
                    continue

                for z in range(n):
                    if (mask >> z) & 1:
                        continue
                    if not g[v][z]:
                        continue

                    nmask = mask | (1 << z)
                    if road[nmask][u][z] is not None:
                        continue

                    road[nmask][u][z] = p + [z]

    dp = [INF] * N
    pre_mask = [-1] * N
    pre_u = [-1] * N
    pre_v = [-2] * N

    dp[1] = 0

    for mask in range(1, N):
        if dp[mask] >= INF:
            continue

        for u in range(n):
            if (mask >> u) & 1:
                continue

            if vtomask[u][mask] >= 2:
                nmask = mask | (1 << u)
                nd = dp[mask] + 2
                if nd < dp[nmask]:
                    dp[nmask] = nd
                    pre_mask[nmask] = mask
                    pre_u[nmask] = u
                    pre_v[nmask] = -1

            if vtomask[u][mask] == 0:
                continue

            for v in range(u + 1, n):
                if (mask >> v) & 1:
                    continue
                if vtomask[v][mask] == 0:
                    continue

                amask = (N - 1) ^ mask ^ (1 << u) ^ (1 << v)

                smask = amask
                while True:
                    tmask = smask | (1 << u) | (1 << v)

                    path = road[tmask][u][v]
                    if path is not None:
                        nmask = mask | tmask
                        nd = dp[mask] + len(path) + 1

                        if nd < dp[nmask]:
                            dp[nmask] = nd
                            pre_mask[nmask] = mask
                            pre_u[nmask] = u
                            pre_v[nmask] = v

                    if smask == 0:
                        break
                    smask = (smask - 1) & amask

    full = N - 1
    print(dp[full])

    ans = []

    mask = full
    while mask != 1:
        pmask = pre_mask[mask]
        u = pre_u[mask]
        v = pre_v[mask]

        if v == -1:
            need = 2
            for x in range(n):
                if need and ((pmask >> x) & 1) and g[u][x]:
                    ans.append((u + 1, x + 1))
                    need -= 1
        else:
            tmask = mask ^ pmask
            path = road[tmask][u][v]

            for i in range(1, len(path)):
                ans.append((path[i - 1] + 1, path[i] + 1))

            for x in range(n):
                if ((pmask >> x) & 1) and g[u][x]:
                    ans.append((x + 1, u + 1))
                    break

            for x in range(n):
                if ((pmask >> x) & 1) and g[v][x]:
                    ans.append((x + 1, v + 1))
                    break

        mask = pmask

    for u, v in ans:
        print(u, v)

solve()
```Việc tính toán trước đường dẫn là phần bất thường nhất trong quá trình triển khai.`road[mask][u][v]`lưu trữ một đường đi đơn giản có các đỉnh chính xác`mask`. Từ$n$chỉ là 14, việc lưu trữ đường dẫn theo tập hợp con là khả thi. 

DP không bao giờ đưa ra lý do trực tiếp về những cây cầu. Thay vào đó nó hoạt động hoàn toàn thông qua quá trình chuyển đổi phân hủy tai. Đó là hiểu biết sâu sắc về cấu trúc giúp giảm thuộc tính đồ thị khó thành tập hợp con DP. 

Trong quá trình tái thiết, mã sẽ tạo lại các cạnh chính xác do mỗi tai đóng góp. Tai đường dẫn đóng góp các cạnh đường dẫn bên trong của nó cộng với một cạnh đính kèm tại mỗi điểm cuối. Tai một đỉnh đóng góp hai cạnh gắn vào. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 3
1 2
2 3
3 1
```Sự phát triển của trạng thái: 

| Mặt nạ | Đỉnh | dp | 
| --- | --- | --- | 
| 001 | {1} | 0 | 
| 111 | {1,2,3} | 3 | 

DP chọn tai đường dẫn$2-3$, sau đó gắn cả hai điểm cuối trở lại đỉnh 1. Đồ thị thu được chính xác là hình tam giác. 

Ví dụ này cho thấy đồ thị 2 cạnh kết nối nhỏ nhất có thể. Mọi cạnh đều được yêu cầu. 

### Ví dụ 2 

đầu vào:```
4 4
1 2
2 3
3 4
4 1
```Sự phát triển của trạng thái: 

| Mặt nạ | Đã thêm tai | Chi phí | 
| --- | --- | --- | 
| 0001 | bắt đầu | 0 | 
| 1111 | đường 2-3-4 | 4 | 

Tai được chọn là con đường$2\to3\to4$. Đóng góp bên trong của nó là ba đỉnh đường dẫn, mang lại chi phí$3+1=4$. 

Đồ thị cuối cùng là chu trình ban đầu. 

Dấu vết này minh họa rằng một tai đường đi có thể tạo ra nhiều đỉnh mới cùng một lúc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(3^n)$| DP trên các tập hợp con và liệt kê tập hợp con bên trong các quá trình chuyển đổi | 
| Không gian |$O(2^n \cdot n^2)$| Bảng DP và lưu trữ đường dẫn | 

Vì$n=14$,$3^{14}\approx4.8$triệu. Điều đó nằm trong giới hạn, đó là lý do tại sao giải pháp dựa vào DP tập hợp đỉnh thay vì tìm kiếm tập hợp con cạnh. 

##
