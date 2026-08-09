---
title: "CF 102448J - Chuông leng keng"
description: "Chúng ta có một cây có (N) đỉnh và (N-1) cạnh. Mỗi cạnh đều đã có một trong năm màu, được đánh số từ (1) đến (5) hoặc vẫn chưa được tô màu và được đánh dấu bằng (0). Bức tranh cuối cùng sẽ gán một màu cho mọi cạnh không được tô màu trong khi vẫn giữ nguyên tất cả các màu hiện có."
date: "2026-08-08T12:28:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102448
codeforces_index: "J"
codeforces_contest_name: "UFPE Starters Final Try-Outs 2020"
rating: 0
weight: 102448
solve_time_s: 504
verified: true
draft: false
---

[CF 102448J - Chuông leng keng](https://codeforces.com/problemset/problem/102448/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 8m 24s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có (N) đỉnh và (N-1) cạnh. Mỗi cạnh đều đã có một trong năm màu, được đánh số từ (1) đến (5) hoặc vẫn chưa được tô màu và được đánh dấu bằng (0). Bức tranh cuối cùng sẽ gán một màu cho mọi cạnh không được tô màu trong khi vẫn giữ nguyên tất cả các màu hiện có. 

Việc tô màu cuối cùng có hiệu lực khi tại mỗi đỉnh, tất cả các cạnh liên quan đều có màu khác nhau. Một màu giống nhau có thể xuất hiện nhiều lần trên cây, nhưng hai cạnh có chung một đỉnh có thể không bao giờ có cùng màu. Câu trả lời là số lần hoàn thành hợp lệ của cây được tô màu một phần, lấy modulo (10^9+7). Các ràng buộc ban đầu cho phép (N) tối đa (10^5), với các mô tả cạnh chính xác (N-1). 

Kích thước của cây loại trừ bất kỳ số mũ nào trong (N). Có thể có (5^{N-1}) phép gán cho các cạnh không được tô màu, vì vậy việc liệt kê chúng là điều không thể đối với một cây chỉ có vài chục cạnh. Ngay cả thuật toán (O(N^2)) cũng quá lớn đối với (10^5) đỉnh dưới giới hạn 2 giây. Về cơ bản, chúng ta cần thời gian tuyến tính, chỉ với một hệ số không đổi nhỏ tùy thuộc vào năm màu có sẵn. 

Thực tế về cấu trúc quan trọng là một đỉnh hợp lệ có thể có tối đa năm cạnh liên quan. Khi chúng ta biết màu của cạnh nối một đỉnh với đỉnh cha của nó, màu của tất cả các cạnh con chỉ cần phân biệt với nhau và với màu cha mẹ đó. Vì chỉ có năm màu nên tập hợp màu hoàn chỉnh được sử dụng bởi các cạnh con sẽ phù hợp với mặt nạ 5 bit. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Hãy xem xét một đỉnh duy nhất:```
1
```Không có cạnh để tô màu nên chỉ có một bức tranh hợp lệ. Việc triển khai DP giả sử mọi đỉnh đều có cạnh cha có thể vô tình trả về 0 ở đây. Đầu ra đúng là`1`. 

Các cạnh liền kề có cùng màu được gán trước sẽ khiến câu trả lời ngay lập tức là 0:```
3
1 2 1
2 3 1
```Tại đỉnh (2), cả hai cạnh tới đều đã có màu (1) nên không thể hoàn thành. Đầu ra đúng là`0`. Việc triển khai chỉ kiểm tra xung đột trong khi chọn màu cho các cạnh không được tô màu sẽ âm thầm bỏ sót mâu thuẫn này. 

Một đỉnh có nhiều hơn năm cạnh liên tiếp cũng không thể xảy ra:```
7
1 2 0
1 3 0
1 4 0
1 5 0
1 6 0
1 7 0
```Tất cả sáu cạnh sẽ phải nhận được các màu riêng biệt theo cặp ở đỉnh (1), nhưng chỉ tồn tại năm màu. Đầu ra đúng là`0`. Việc triển khai mặt nạ không được vô tình chỉ xử lý năm phần tử con đầu tiên và bỏ qua phần tử thứ sáu. 

Cuối cùng, màu tương tự được cho phép trên các cạnh không gặp nhau ở một đỉnh:```
4
1 2 1
2 3 2
3 4 1
```Mỗi đỉnh vẫn nhìn thấy các màu tới riêng biệt, vì vậy đây là cách tô màu hợp lệ và câu trả lời là`1`. Việc triển khai bất cẩn coi mỗi màu chỉ có thể sử dụng được trên toàn cầu một lần sẽ từ chối màu đó một cách không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là gán cho mỗi cạnh không được tô màu một trong năm màu, sau đó kiểm tra xem màu kết quả có hợp lệ hay không. Nếu có (K) cạnh không được tô màu, điều này tạo ra (5^K) ứng cử viên. Trong trường hợp xấu nhất (K=N-1), do đó có (5^{N-1}) màu hoàn chỉnh. Việc kiểm tra một màu mất (\Theta(N)) thời gian vì mọi cạnh hoặc đỉnh đều có thể cần được kiểm tra. Do đó, tổng công việc là (\Theta(N5^{N-1})), gần như ngay lập tức trở nên vô vọng. 

Lực lượng vũ phu hoạt động vì mọi nhiệm vụ hoàn chỉnh đều có thể được kiểm tra một cách độc lập. Nó thất bại vì nó liên tục giải quyết cùng một cây con. Ví dụ: khi màu của cạnh đi vào cây con được cố định, mọi thứ bên dưới cạnh đó có thể được tính độc lập với phần còn lại của cây. Đó chính xác là loại cây cấu trúc lặp lại mà DP có thể khai thác. 

Gốc cây ở một đỉnh tùy ý. Đối với mỗi đỉnh (v), giả sử cạnh từ cha của nó có màu (p). Thông tin duy nhất từ ​​phần còn lại của cây quan trọng đối với cây con của (v) là giá trị (p). Xác định (dp[v][p]) là số cách tô màu hợp lệ của cây con của (v), giả sử cạnh cha có màu (p). 

Các con của (v) không thể có chung một màu và không màu nào của chúng có thể bằng (p). Đối với một cạnh không được tô màu từ (v) đến cây con (u), việc chọn màu (c) sẽ đóng góp (dp[u][c]) cách từ cây con của cây con. Đối với một cạnh đã được tô màu có màu (c), chỉ có một lựa chọn khả thi, đóng góp (dp[u][c]), miễn là (c) chưa được sử dụng tại (v). 

Thử thách còn lại là kết hợp các em mà không thử từng (5^5) bài tập một cách rõ ràng. Vì chỉ có năm màu nên hãy thể hiện các màu đã được sử dụng trong số các cạnh con được xử lý bằng mặt nạ 5 bit. Chỉ có (2^5=32) mặt nạ. Chúng ta có thể chạy một tập con DP nhỏ trên các phần tử con. 

Có một cách tối ưu hóa hữu ích giúp DP sạch hơn. Bản thân việc gán cạnh con không phụ thuộc vào màu gốc (p). Trước tiên, chúng ta có thể đếm tất cả các phép gán riêng biệt của các màu cạnh con, lưu trữ mặt nạ màu đã sử dụng của chúng. Sau đó, (dp[v][p]) chỉ đơn giản là tổng các mặt nạ không chứa màu (p). Đối với gốc không có cạnh cha, được biểu thị bằng (p=0), vì vậy mọi mặt nạ đều được phép. 

Độ phức tạp thu được là tuyến tính theo số đỉnh cho đến hệ số không đổi từ năm màu và 32 mặt nạ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (\Theta(N5^{N-1})) | (O(N)) | Quá chậm | 
| Tối ưu | (O(N\cdot 5\cdot 32)) | (O(N\cdot 5)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Gốc cây tại đỉnh (1). Đối với mỗi đỉnh, hãy nhớ đỉnh cha của nó sao cho cạnh hướng về phía cha mẹ bị loại trừ khi xử lý các con của nó. Việc truyền tải lặp lại được ưu tiên hơn trong Python vì cây có thể là một đường dẫn có độ dài (10^5), vượt quá giới hạn ngăn xếp cuộc gọi đệ quy thông thường. 
2. Xác định (dp[v][p]) cho (p\in{0,1,2,3,4,5}). Đối với (p=1,\ldots,5), nó đếm các màu hợp lệ bên trong cây con của (v) khi cạnh cha có màu (p). Trạng thái (p=0) chỉ được sử dụng cho gốc, khi không có cạnh cha. 
3. Xử lý các đỉnh theo thứ tự duyệt ngược để giá trị DP của mọi nút con đều được biết trước khi nút cha của nó được xử lý. Đối với đỉnh (v), bắt đầu tập con DP với`ways[0] = 1`. Mặt nạ ghi lại chính xác màu nào đã được gán cho các cạnh con được xử lý. 
4. Đối với cạnh con đã được tô màu bằng màu (c), chỉ có thể sử dụng màu đó. Nếu mặt nạ hiện tại đã chứa (c), quá trình chuyển đổi bị cấm vì hai cạnh liên quan tại (v) sẽ có chung một màu. Nếu không, hãy thêm các cách (dp[u][c]) của phần tử con vào mặt nạ chứa (c). 
5. Đối với cạnh con không được tô màu, hãy thử mọi màu (c) có bit chưa có trong mặt nạ. Việc chọn (c) đóng góp (dp[u][c]) cách, bởi vì sau khi sửa màu cạnh, toàn bộ cây con con có chính xác số lần hoàn thành hợp lệ đó. 
6. Sau khi tất cả trẻ em đã được xử lý,`ways[mask]`biểu thị số lượng phép gán hợp lệ cho tất cả các cạnh con sử dụng chính xác các màu trong`mask`. Đối với phần gốc, hãy tính tổng mọi mặt nạ vì không có màu gốc. Đối với đỉnh không phải gốc và màu gốc (p), chỉ tính tổng các mặt nạ không chứa (p). Đây là điều kiện cục bộ mà cạnh cha phải có màu khác với mọi cạnh con. 
7. Lưu trữ sáu giá trị này dưới dạng`dp[v]`và tiếp tục đi lên. Khi đỉnh (1) được xử lý, câu trả lời bắt buộc là`dp[1][0]`, vì gốc không có cạnh cha. 

Bất biến là sau khi xử lý một đỉnh (v),`dp[v][p]`đếm mọi màu hợp lệ của toàn bộ cây con chính xác một lần, với giả định duy nhất là cạnh cha có màu (p). Tập hợp con DP thực thi các màu riêng biệt theo cặp giữa tất cả các cạnh con, trong khi việc lọc mặt nạ cuối cùng thực thi sự bất bình đẳng với cạnh cha. Vì các cây con khác nhau chỉ chia sẻ đỉnh (v), nên khi màu của các cạnh tới của chúng được cố định, các lựa chọn bên trong của chúng là độc lập. Do đó, mọi màu tổng thể hợp lệ sẽ tương ứng với chính xác một đường dẫn DP và mỗi đường dẫn DP biểu thị một màu hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 1_000_000_007
FULL = 31

def solve():
    n = int(input())

    graph = [[] for _ in range(n)]

    for _ in range(n - 1):
        u, v, c = map(int, input().split())
        u -= 1
        v -= 1
        graph[u].append((v, c))
        graph[v].append((u, c))

    parent = [-1] * n
    parent[0] = -2
    order = [0]

    for v in order:
        for u, c in graph[v]:
            if u == parent[v]:
                continue
            parent[u] = v
            order.append(u)

    dp = [[0] * 6 for _ in range(n)]

    for v in reversed(order):
        ways = [0] * 32
        ways[0] = 1

        for u, edge_color in graph[v]:
            if u == parent[v]:
                continue

            new_ways = [0] * 32

            if edge_color != 0:
                bit = 1 << (edge_color - 1)
                child_ways = dp[u][edge_color]

                if child_ways:
                    for mask in range(32):
                        value = ways[mask]
                        if value and not (mask & bit):
                            new_ways[mask | bit] += value * child_ways

            else:
                child_dp = dp[u]

                for mask in range(32):
                    value = ways[mask]
                    if not value:
                        continue

                    for color in range(1, 6):
                        bit = 1 << (color - 1)
                        if not (mask & bit):
                            new_ways[mask | bit] += value * child_dp[color]

            for mask in range(32):
                new_ways[mask] %= MOD

            ways = new_ways

        total = sum(ways) % MOD
        dp[v][0] = total

        for color in range(1, 6):
            bit = 1 << (color - 1)
            count = 0

            for mask in range(32):
                if not (mask & bit):
                    count += ways[mask]

            dp[v][color] = count % MOD

    print(dp[0][0])

if __name__ == "__main__":
    solve()
```Danh sách kề lưu trữ cả hai điểm cuối của mỗi cạnh cùng với màu cố định của nó. Màu bằng 0 có nghĩa là quá trình chuyển đổi có thể chọn bất kỳ màu nào trong năm màu. 

các`parent`Và`order`mảng biến cây vô hướng thành cây có gốc mà không cần đệ quy. Bởi vì`order`được xây dựng từ gốc ra ngoài, đảo ngược lại đảm bảo con cái được xử lý trước cha mẹ. 

Phần trung tâm của việc thực hiện là`ways`. Chỉ mục của nó là mặt nạ năm bit, trong đó bit (c-1) có nghĩa là màu (c) đã được sử dụng bởi một trong các cạnh con được xử lý. Một cạnh cố định có chính xác một màu có thể, trong khi một cạnh không có màu sẽ thử tất cả năm màu chưa được thể hiện trong mặt nạ. 

Giá trị DP con được nhân lên trong mỗi lần chuyển đổi. Đây là quy tắc tích cho các cây con độc lập: sau khi quyết định rằng cạnh ((v,u)) có màu (c), sẽ có`dp[u][c]`những cách có thể để hoàn thành mọi thứ bên dưới (u). 

Mã trì hoãn hoạt động modulo cho đến khi mỗi phần tử con được kết hợp. Mọi giá trị DP tham gia quá trình chuyển đổi đều ở mức dưới (10^9+7) và mỗi giá trị trung gian`new_ways`giá trị vẫn đủ nhỏ cho số học số nguyên của Python. Việc giảm sau mỗi phần tử con giữ cho các giá trị bị giới hạn mà không phải trả tiền cho thao tác modulo trên mỗi lần chuyển đổi riêng lẻ. 

Vòng lặp cuối cùng tính toán`dp[v][color]`bằng cách loại trừ các mặt nạ có chứa màu đó. Điều này tương đương với việc thực thi hạn chế cạnh cha sau khi tất cả các cạnh con đã được chỉ định. Đối với phần gốc,`dp[0][0]`tính tổng mọi mặt nạ vì màu 0 không tương ứng với màu cạnh thực tế. 

Không có vấn đề tràn số nguyên trong Python, nhưng thao tác modulo vẫn cần thiết vì câu trả lời được yêu cầu là modulo (10^9+7). Quan trọng hơn, việc giảm giá trị DP sẽ ngăn các số nguyên trung gian tăng lên một cách không cần thiết. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Cây có gốc ở đỉnh (1). Cạnh (1-3) được cố định theo màu (1), trong khi các cạnh (1-2) và (3-4) không được tô màu. 

| Đỉnh | Cạnh con | Giá trị DP con | khác không`ways`mặt nạ | Kết quả DP | 
| --- | --- | --- | --- | --- | 
| 4 | không | tất cả các tiểu bang (=1) | mặt nạ (0:1) | (dp[4][p]=1) | 
| 3 | (3-4) không màu | tất cả các tiểu bang (=1) | năm mặt nạ một bit, mỗi mặt nạ (1) | (dp[3][0]=5,\ dp[3][1..5]=4) | 
| 2 | không | tất cả các tiểu bang (=1) | mặt nạ (0:1) | (dp[2][p]=1) | 
| 1 | (1-2) không màu, (1-3) cố định (1) | (dp[2][c]=1,\ dp[3][1]=4) | mặt nạ chứa màu (1) và một màu khác, mỗi màu (4) | (dp[1][0]=16) | 

Tại đỉnh (3), từ cạnh đến đỉnh (4) có thể sử dụng bất kỳ màu nào trong năm màu. Nếu cạnh cha có màu (1), thì chỉ còn lại bốn lựa chọn trong số đó, cho ra (dp[3][1]=4). 

Tại đỉnh (1), cạnh tới đỉnh (3) phải sử dụng màu (1). Khi đó, cạnh tới đỉnh (2) có thể sử dụng bất kỳ màu nào trong bốn màu còn lại. Với mỗi lựa chọn như vậy, cây con của đỉnh (3) có bốn phần hoàn thành. Vì vậy câu trả lời là (4\cdot4=16). 

### Mẫu 2 

Đây là một con đường có ba cạnh không được tô màu. 

| Đỉnh | Cạnh con | Giá trị DP con |`ways`sau con | Kết quả DP | 
| --- | --- | --- | --- | --- | 
| 4 | không | tất cả các tiểu bang (=1) | mặt nạ (0:1) | tất cả các tiểu bang (1) | 
| 3 | (3-4) không màu | tất cả (1) | năm mặt nạ một bit, mỗi mặt nạ (1) | (dp[3][0]=5,\ dp[3][1..5]=4) | 
| 2 | (2-3) không màu | (dp[3][c]=4) | năm mặt nạ một bit, mỗi mặt nạ (4) | (dp[2][0]=20,\ dp[2][1..5]=16) | 
| 1 | (1-2) không màu | (dp[2][c]=16) | năm mặt nạ một bit, mỗi mặt nạ (16) | (dp[1][0]=80) | 

Cạnh đầu tiên có năm lựa chọn. Khi màu của nó được cố định, cạnh tiếp theo có bốn lựa chọn và cạnh cuối cùng cũng có bốn lựa chọn. DP tính toán chính xác (5\cdot4\cdot4=80), khớp với đầu ra mẫu. 

Ví dụ này cũng giải thích tại sao màu gốc phải là một phần của trạng thái DP. Tại đỉnh (3), có năm khả năng khi không có màu gốc nào được áp dụng, nhưng chỉ có bốn khả năng sau khi một trong năm màu đã được cạnh gốc sử dụng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(N\cdot5\cdot32)) | Mỗi đứa trẻ được xử lý tối đa 32 mặt nạ và 5 màu có thể | 
| Không gian | (O(N\cdot5+N)) | DP lưu trữ sáu trạng thái trên mỗi đỉnh, cùng với cây và mảng truyền tải | 

Số lượng màu được cố định ở mức 5, vì vậy các hệ số (5) và (32=2^5) là hằng số. Do đó, thuật toán tuyến tính về số đỉnh cho mục đích tiệm cận. Với (N\le10^5), số cạnh của cây cũng là (O(10^5)), do đó, giải pháp vừa vặn thoải mái trong giới hạn bộ nhớ dự định và tránh tìm kiếm theo cấp số nhân (5^{N-1}). 

## Trường hợp thử nghiệm 

Dây thử nghiệm bên dưới sử dụng tương tự`solve()`hoạt động như giải pháp được gửi. Trường hợp kích thước tối đa được tạo dưới dạng một ngôi sao có (100000) đỉnh. Tâm của nó có (99999) cạnh tới, vì vậy câu trả lời ngay lập tức là 0.```python
import sys
import io

MOD = 1_000_000_007

def solve():
    input = sys.stdin.readline

    n = int(input())
    graph = [[] for _ in range(n)]

    for _ in range(n - 1):
        u, v, c = map(int, input().split())
        u -= 1
        v -= 1
        graph[u].append((v, c))
        graph[v].append((u, c))

    parent = [-1] * n
    parent[0] = -2
    order = [0]

    for v in order:
        for u, c in graph[v]:
            if u == parent[v]:
                continue
            parent[u] = v
            order.append(u)

    dp = [[0] * 6 for _ in range(n)]

    for v in reversed(order):
        ways = [0] * 32
        ways[0] = 1

        for u, edge_color in graph[v]:
            if u == parent[v]:
                continue

            new_ways = [0] * 32

            if edge_color != 0:
                bit = 1 << (edge_color - 1)
                child_ways = dp[u][edge_color]

                if child_ways:
                    for mask in range(32):
                        value = ways[mask]
                        if value and not (mask & bit):
                            new_ways[mask | bit] += value * child_ways
            else:
                child_dp = dp[u]

                for mask in range(32):
                    value = ways[mask]
                    if not value:
                        continue

                    for color in range(1, 6):
                        bit = 1 << (color - 1)
                        if not (mask & bit):
                            new_ways[mask | bit] += value * child_dp[color]

            for mask in range(32):
                new_ways[mask] %= MOD

            ways = new_ways

        dp[v][0] = sum(ways) % MOD

        for color in range(1, 6):
            bit = 1 << (color - 1)
            total = 0
            for mask in range(32):
                if not (mask & bit):
                    total += ways[mask]
            dp[v][color] = total % MOD

    print(dp[0][0])

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample 1
assert run(
    """4
1 2 0
1 3 1
3 4 0
"""
) == "16", "sample 1"

# Provided sample 2
assert run(
    """4
1 2 0
2 3 0
3 4 0
"""
) == "80", "sample 2"

# Minimum-size tree: no edges means exactly one empty coloring.
assert run(
    """1
"""
) == "1", "minimum-size tree"

# Two vertices and one uncolored edge: any of the five colors works.
assert run(
    """2
1 2 0
"""
) == "5", "single uncolored edge"

# Adjacent fixed edges with the same color are impossible.
assert run(
    """3
1 2 1
2 3 1
"""
) == "0", "adjacent equal fixed colors"

# A vertex with exactly five incident edges can use every color once.
assert run(
    """6
1 2 0
1 3 0
1 4 0
1 5 0
1 6 0
"""
) == "120", "degree five"

# Maximum-size input. The center has 99999 incident edges,
# so no proper edge coloring with five colors exists.
max_n = 100000
max_input = str(max_n) + "\n" + "".join(
    f"1 {v} 0\n" for v in range(2, max_n + 1)
)
assert run(max_input) == "0", "maximum-size impossible star"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`1`| Cây rỗng và trạng thái gốc | 
| Hai đỉnh, một cạnh không màu |`5`| Lựa chọn năm màu cơ bản và lập chỉ mục ranh giới | 
| Đường dẫn ba đỉnh có màu cả hai cạnh cố định`1`|`0`| Phát hiện xung đột cục bộ hiện có | 
| Ngôi sao không màu năm cạnh |`120`| Ranh giới chính xác cấp năm, cho (5!) | 
| (100000)-đỉnh sao không màu |`0`| Kích thước và mức độ đầu vào tối đa lớn hơn năm | 

## Vỏ cạnh 

Đối với trường hợp một đỉnh, đầu vào chỉ đơn giản là:```
1
```Đường ngang chỉ chứa đỉnh (1). Tập con DP của nó bắt đầu bằng`ways[0] = 1`và xử lý không có cạnh, vì vậy mặt nạ duy nhất vẫn hợp lệ. Root sử dụng trạng thái`p=0`, tính tổng tất cả các mặt nạ và cho`1`. Không có trường hợp đặc biệt được yêu cầu trong việc thực hiện. 

Đối với các cạnh được tô màu trước liền kề có cùng màu, hãy xem xét:```
3
1 2 1
2 3 1
```Khi đỉnh (2) được xử lý, cả thông tin cạnh con và cạnh cha đều yêu cầu màu (1) ở cùng một đỉnh. Trong tập hợp con DP, màu cố định đầu tiên chiếm bit (0), trong khi màu cố định thứ hai cố gắng chiếm cùng bit đó. Vì bit đã có sẵn nên quá trình chuyển đổi không tạo ra trạng thái nào. Giá trị DP trở thành 0 và câu trả lời cuối cùng là`0`. 

Đối với một đỉnh có nhiều hơn năm cạnh liên tiếp, hãy xem xét ngôi sao sáu cạnh:```
7
1 2 0
1 3 0
1 4 0
1 5 0
1 6 0
1 7 0
```Sau khi năm phần tử con đã được xử lý, mọi mặt nạ có thể có tối đa là năm bit được đặt. Đứa trẻ thứ sáu không thể chọn bất kỳ màu nào bị thiếu bit, vì vậy mọi chuyển đổi đều tạo ra số 0. trận chung kết`ways`mảng hoàn toàn bằng 0, đưa ra câu trả lời`0`. Việc triển khai không cần kiểm tra mức độ riêng biệt vì không gian trạng thái 5 bit tự nhiên phát hiện ra điều không thể thực hiện được. 

Đối với trường hợp màu lặp lại ở các phần khác nhau của cây, hãy xem xét:```
4
1 2 1
2 3 2
3 4 1
```Hai cạnh màu-1 không liền nhau nên không có xung đột. Mọi cạnh đều đã được cố định và mọi đỉnh đều nhìn thấy các màu tới riêng biệt. DP tuân theo chính xác một chuyển đổi ở mỗi đỉnh và trả về`1`. Điều này xác nhận rằng mặt nạ chỉ theo dõi các màu giữa các cạnh liên quan đến đỉnh hiện tại, thay vì xử lý sai các màu là duy nhất trên toàn cầu.
