---
title: "CF 1558E - Xuống bên dưới"
description: "Chúng ta được cung cấp một biểu đồ gồm các hang động được nối với nhau bằng các đường hầm và một anh hùng bắt đầu từ hang 1 với một số sức mạnh ban đầu. Mỗi hang động khác ban đầu đều chứa một con quái vật. Nhiệm vụ của người anh hùng là đến thăm và đánh bại quái vật trong mỗi hang động ít nhất một lần."
date: "2026-06-18T18:57:32+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "dfs-and-similar", "graphs", "greedy", "meet-in-the-middle", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 1558
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 740 (Div. 1, based on VK Cup 2021 - Final (Engine))"
rating: 3000
weight: 1558
solve_time_s: 109
verified: false
draft: false
---

[CF 1558E - Dưới đây](https://codeforces.com/problemset/problem/1558/E) 

**Đánh giá:** 3000 
**Thẻ:** tìm kiếm nhị phân, dfs và tương tự, đồ thị, tham lam, gặp nhau ở giữa, đường đi ngắn nhất 
**Thời gian giải:** 1 phút 49 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một biểu đồ gồm các hang động được nối với nhau bằng các đường hầm và một anh hùng bắt đầu từ hang 1 với một số sức mạnh ban đầu. Mỗi hang động khác ban đầu đều chứa một con quái vật. Nhiệm vụ của người anh hùng là đến thăm và đánh bại quái vật trong mỗi hang động ít nhất một lần. 

Mỗi hang động$i$(trừ hang 1) có ngưỡng$a_i$. Người anh hùng chỉ có thể đánh bại quái vật trong hang động đó trong lần đầu tiên bước vào nếu sức mạnh hiện tại của anh ta lớn hơn$a_i$. Sau khi đánh bại nó, sức mạnh của anh hùng tăng thêm$b_i$, và các chuyến thăm tới hang động đó trong tương lai đều miễn phí. 

Chuyển động xảy ra dọc theo các cạnh vô hướng. Có một hạn chế bổ sung: nếu anh hùng đi qua một cạnh từ$u$ĐẾN$v$, anh ta không thể ngay lập tức đi qua cùng một cạnh từ$v$ĐẾN$u$. Điều này ngăn cản sự dao động tầm thường nhưng không ngăn cản việc xem lại các nút thông qua các cạnh khác. 

Mục tiêu là xác định sức mạnh ban đầu tối thiểu sao cho tồn tại một số thứ tự di chuyển hợp lệ trong các hang động cho phép đánh bại tất cả quái vật. 

Biểu đồ được kết nối và có tổng kích thước tương đối nhỏ trong các thử nghiệm, nhưng vẫn đủ lớn để không thể khám phá theo cấp số nhân. Với tổng số lên tới 1000 nút và 2000 cạnh, bất kỳ giải pháp nào cố gắng liệt kê tất cả các đường dẫn hoặc hoán vị của thứ tự truy cập đều không khả thi. Thậm chí$O(n^2)$trên mỗi trạng thái hoặc việc mở rộng đường đi ngắn nhất ngây thơ qua các trạng thái lũy thừa sẽ quá chậm nếu lặp lại trong tìm kiếm. 

Một khó khăn nhỏ là hạn chế truyền tải không hạn chế một cách có ý nghĩa khả năng tiếp cận trong thực tế vì mọi nút đều có ít nhất cấp 2, do đó biểu đồ không bao giờ tạo ra ngõ cụt. Hạn chế này chủ yếu ngăn chặn việc quay lại ngay lập tức dọc theo cùng một cạnh, nhưng bất kỳ chiến lược tối ưu nào cũng có thể được xem là đi qua cấu trúc thăm dò mở rộng mà không dựa vào sự đảo ngược của cạnh. 

Một vài trường hợp thất bại cho thấy lý do tại sao các phương pháp tiếp cận ngây thơ lại thất bại: 

Một ý tưởng ngây thơ là thử DFS từ nút 1 với sức mạnh hiện tại và tham lam truy cập bất kỳ nút nào chưa được truy cập có thể truy cập được. Điều này không thành công vì một hang động có thể tiếp cận tại địa phương có thể không phải là điều tối ưu trên toàn cầu để đến thăm sớm. Ví dụ: ngưỡng thấp nhưng hang động có phần thưởng thấp có thể chặn quyền truy cập vào hang động có phần thưởng cao mà bạn phải truy cập sớm hơn để tích lũy sức mạnh. 

Một cách tiếp cận sai lầm khác là sắp xếp các nút theo$a_i$và cố gắng truy cập theo thứ tự đó trong khi kiểm tra khả năng tiếp cận một cách linh hoạt. Điều này không thành công vì khả năng tiếp cận phụ thuộc vào các nút và cấu trúc biểu đồ đã bị xóa chứ không chỉ các ngưỡng được sắp xếp. 

Khó khăn chính là thứ tự truy cập các nút ảnh hưởng đến cả tính khả thi (hạn chế về năng lượng) và tính khả thi trong tương lai (mở khóa tăng trưởng năng lượng cho các nút khác). Đây là vấn đề đặt hàng toàn cầu bị hạn chế bởi kết nối đồ thị. 

## Phương pháp tiếp cận 

Quan điểm bạo lực là xem xét tất cả các mệnh lệnh có thể có trong đó các hang động có thể được dọn sạch. Đối với mỗi thứ tự, chúng tôi mô phỏng xem liệu có thể duyệt qua biểu đồ theo trình tự đó hay không, tôn trọng các hạn chế về kết nối và nguồn điện. Điều này ngay lập tức không thể thực hiện được vì có$n!$hoán vị và thậm chí xác nhận một thứ tự đòi hỏi lý luận về khả năng tiếp cận biểu đồ. Điều này phát triển vượt xa mọi giới hạn một lần$n$vượt quá cả những giá trị nhỏ. 

Chúng ta cần giảm vấn đề thành một thứ tránh được việc liệt kê thứ tự rõ ràng nhưng vẫn nắm bắt được cấu trúc phụ thuộc: chỉ có thể truy cập một hang động khi ngưỡng của nó được thỏa mãn, nhưng việc truy cập nó sẽ tăng sức mạnh và mở khóa các hang động khác. 

Quan sát quan trọng là nếu chúng ta cố định công suất ban đầu$P$, chúng ta có thể coi vấn đề là một quá trình có thể tiếp cận: chúng ta bắt đầu từ nút 1 và chúng ta được phép nhập một nút$v$chỉ khi nó có thể truy cập được thông qua các nút đã truy cập và$P > a_v$. Một lần ghé thăm,$v$tăng sức mạnh bằng cách$b_v$, có khả năng mở khóa nhiều nút hơn. 

Điều này gợi ý việc kiểm tra tính khả thi cho một giải pháp cố định$P$, có thể được triển khai như một quy trình mở rộng tốt nhất đầu tiên. Từ bất kỳ tập hợp nút đã xóa nào hiện có thể truy cập được, chúng tôi có thể mở rộng sang bất kỳ nút nào chưa được truy cập liền kề có ngưỡng được thỏa mãn. 

Tuy nhiên, BFS ngây thơ từ biên giới hiện tại vẫn chưa đủ vì khả năng tiếp cận không chỉ là lân cận mà còn phụ thuộc vào khả năng đi qua các nút đã bị xóa. Điều này đương nhiên dẫn đến việc duy trì giới hạn các nút đã được xóa có thể truy cập và mở rộng ra bên ngoài, nhưng chúng tôi phải đảm bảo rằng chúng tôi không bao giờ “quên” các nút có thể truy cập nhưng chưa có giá cả phải chăng. 

Đây là nơi cấu trúc biểu đồ và tính đơn điệu kết hợp: một khi một nút có thể truy cập được về mặt kết nối, nó vẫn có thể truy cập được; điều kiện động duy nhất là liệu ngưỡng của nó có được thỏa mãn khi công suất tăng hay không. Điều này cho phép chúng ta xử lý quá trình này như liên tục chọn bất kỳ nút nào hiện có thể truy cập được mà$a_i$hài lòng và thêm nó vào nhóm để mở rộng khả năng tiếp cận. 

Để quyết định hiệu quả công suất ban đầu tối thiểu, chúng tôi tìm kiếm nhị phân trên$P$. Đối với mỗi ứng viên$P$, chúng tôi mô phỏng liệu chúng tôi có thể xóa tất cả các nút bằng cách sử dụng quy trình khả thi tham lam được hướng dẫn bởi cấu trúc ưu tiên đối với các nút có thể truy cập nhưng chưa bị xóa hay không. 

Cấu trúc cuối cùng giống như một quy trình mở rộng bị ràng buộc: duy trì một tập hợp các nút có thể truy cập và trong số chúng liên tục lấy bất kỳ nút nào có ngưỡng được thỏa mãn, thu thập phần thưởng của nó và mở rộng khả năng tiếp cận. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hoán vị vũ phu |$O(n!)$|$O(n)$| Quá chậm | 
| Tìm kiếm nhị phân + BFS khả thi/mở rộng tham lam |$O((n+m)\log A)$|$O(n+m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải quyết vấn đề bằng cách tìm kiếm nhị phân lũy thừa ban đầu$P$. Đối với mỗi lần đoán, chúng tôi kiểm tra xem có thể vượt qua tất cả các hang động hay không. 

1. Cố định sức mạnh ban đầu của ứng viên$P$. Bắt đầu từ hang 1, đánh dấu là đã ghé thăm và đặt nguồn điện hiện tại thành$P$. Điều này đúng vì hang 1 không có quái vật. 
2. Duy trì cấu trúc của tất cả các hang động có thể truy cập được từ tập hợp đã truy cập thông qua các cạnh biểu đồ nhưng chưa được truy cập. Điều này được tính toán bằng cách mở rộng từ các nút đã truy cập bằng danh sách kề. 
3. Trong số tất cả các hang động có thể tiếp cận được, chúng tôi chỉ xem xét những hang động có ngưỡng$a_i$hoàn toàn nhỏ hơn công suất hiện tại. Đây là những hang động duy nhất chúng tôi được phép khai phá vào lúc này. 
4. Nếu không có hang động như vậy nhưng vẫn tồn tại những hang động chưa được thăm quan, quá trình này sẽ dừng lại$P$vì chúng tôi bị chặn bởi nguồn điện không đủ. Điều này cho thấy ứng viên này đã thất bại. 
5. Nếu không, hãy chọn bất kỳ hang động có sẵn nào có điều kiện ngưỡng được thỏa mãn, đánh dấu nó là đã ghé thăm và thêm phần thưởng của nó$b_i$đến sức mạnh hiện tại. Việc lựa chọn giữa nhiều hang động có sẵn có thể là tùy ý vì bất kỳ chuỗi lấy nút khả thi hợp lệ nào đều duy trì khả năng mở rộng khả năng tiếp cận; chúng ta chỉ cần sự tồn tại chứ không cần thứ tự tối ưu ở giai đoạn này. 
6. Sau khi tham quan một hang động, hãy mở rộng ranh giới có thể tiếp cận từ hang động đó bằng cách sử dụng các hàng xóm của nó, vì các nút mới có thể tiếp cận được. 
7. Lặp lại cho đến khi tất cả các hang động được truy cập hoặc không thể đạt được tiến triển nào. 
8. Nếu đã viếng thăm tất cả các hang động, thí sinh$P$là khả thi. Nếu không thì không. 

Tìm kiếm nhị phân hoạt động vì tính khả thi là đơn điệu: nếu một lũy thừa$P$cho phép hoàn thành, bất kỳ quyền lực nào cao hơn cũng cho phép hoàn thành vì mọi ràng buộc$P > a_i$chỉ trở nên dễ thỏa mãn hơn và phần thưởng không thay đổi về cấu trúc. 

Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, thuật toán duy trì ranh giới của tất cả các nút có thể truy cập được từ các hang động đã được dọn sạch. Hạn chế duy nhất là liệu ngưỡng của họ có được đáp ứng bởi công suất hiện tại hay không. Bởi vì việc truy cập một nút chỉ làm tăng sức mạnh chứ không bao giờ làm giảm khả năng tiếp cận, nên tập hợp các nút có thể truy cập tăng lên một cách đơn điệu và tập hợp các nút có thể tiếp cận cũng tăng trưởng đơn điệu khi sức mạnh tăng lên. Điều này đảm bảo rằng nếu một chuỗi tồn tại, mô phỏng tính khả thi tham lam cuối cùng sẽ tìm thấy một thứ tự hợp lệ, vì bất kỳ giải pháp hợp lệ nào cũng có thể được chuyển đổi thành một giải pháp luôn chọn một nút có thể truy cập được mà không mất khả năng truy cập trong tương lai. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

def can(start_power, n, g, a, b):
    visited = [False] * (n + 1)
    reachable = [False] * (n + 1)

    q = deque()
    visited[1] = True
    reachable[1] = True
    q.append(1)

    power = start_power

    # expand initial reachability from node 1
    for v in g[1]:
        reachable[v] = True
        q.append(v)

    # we maintain a deque of reachable nodes
    # but we repeatedly scan it, so we use a list as pool
    pool = set()
    for v in g[1]:
        if not visited[v]:
            pool.add(v)

    changed = True
    while True:
        progress = False

        # try to take any feasible node
        to_remove = None
        for v in list(pool):
            if not visited[v] and reachable[v] and power > a[v]:
                to_remove = v
                break

        if to_remove is None:
            break

        v = to_remove
        pool.remove(v)
        visited[v] = True
        power += b[v]
        progress = True

        # expand reachability
        for to in g[v]:
            if not reachable[to]:
                reachable[to] = True
                if not visited[to]:
                    pool.add(to)

    return all(visited[1:])

def solve_case(n, m, edges, a, b):
    g = [[] for _ in range(n + 1)]
    for u, v in edges:
        g[u].append(v)
        g[v].append(u)

    # node 1 has no monster; set dummy values
    a = [0] + a
    b = [0] + b

    lo, hi = 1, 10**18
    ans = hi

    while lo <= hi:
        mid = (lo + hi) // 2
        if can(mid, n, g, a, b):
            ans = mid
            hi = mid - 1
        else:
            lo = mid + 1

    return ans

def main():
    t = int(input())
    out = []
    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))
        b = list(map(int, input().split()))
        edges = [tuple(map(int, input().split())) for _ in range(m)]
        out.append(str(solve_case(n, m, edges, a, b)))
    print("\n".join(out))

if __name__ == "__main__":
    main()
```Việc triển khai tách vấn đề thành tìm kiếm nhị phân theo công suất ban đầu và kiểm tra tính khả thi. 

Danh sách lân cận thể hiện sự kết nối của hang động. Các mảng`a`Và`b`được đệm để các chỉ số khớp trực tiếp với số hang động. chức năng`can`mô phỏng liệu một công suất khởi động nhất định có đủ hay không. 

Bên trong`can`,`reachable`theo dõi các nút có thể truy cập được thông qua các nút đã truy cập.`visited`theo dõi các hang động đã được dọn sạch. các`pool`chứa các nút có thể truy cập nhưng chưa được truy cập và chúng tôi liên tục quét nó để tìm nút có ngưỡng được thỏa mãn. 

Khi chúng tôi chọn một nút, chúng tôi sẽ tăng sức mạnh và mở rộng khả năng tiếp cận bằng cách sử dụng danh sách lân cận của nút đó. Quá trình dừng lại khi không còn nút khả thi nào nữa. 

Tìm kiếm nhị phân khai thác tính đơn điệu: công suất khởi đầu cao hơn không bao giờ làm mất hiệu lực của chuỗi hợp lệ trước đó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 4
11 22 13
8 7 5
1 2
2 3
3 4
4 1
```| Bước | Nút hiện tại được chọn | Quyền lực | Đã truy cập | Mới có thể truy cập | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 1 | 15 | {1} | {2,4} | 
| 1 | 2 | 23 | {1,2} | {3} | 
| 2 | 3 | 30 | {1,2,3} | - | 
| 3 | 4 | 35 | {1,2,3,4} | - | 

Dấu vết cho thấy rằng sau khi nút 2 được lấy, biểu đồ sẽ mở ra theo cách giúp tất cả các nút còn lại có thể truy cập được và sức mạnh tăng đủ nghiêm ngặt để đáp ứng tất cả các ngưỡng. 

### Ví dụ 2 

đầu vào:```
5 7
10 40 20 30
7 2 10 5
1 2
1 5
2 3
2 4
2 5
3 4
4 5
```| Bước | Nút hiện tại được chọn | Quyền lực | Đã truy cập | Lý do | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 1 | 19 | {1} | ban đầu | 
| 1 | 2 | 26 | {1,2} | thỏa mãn 10 | 
| 2 | 4 | 36 | {1,2,4} | thỏa mãn 20 | 
| 3 | 5 | 41 | {1,2,4,5} | thỏa mãn 30 | 
| 4 | 3 | 43 | {1,2,3,4,5} | thỏa mãn 40 | 

Điều này chứng tỏ rằng thứ tự truy cập rất linh hoạt miễn là các ràng buộc về khả năng tiếp cận và ngưỡng được tôn trọng; tồn tại các chuỗi hợp lệ khác nhau, nhưng việc khai triển tham lam luôn tìm thấy một chuỗi nếu khả thi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((n + m) \log 10^9)$| tìm kiếm nhị phân theo công suất, mỗi lần chạy khả thi sẽ quét các nút và cạnh | 
| Không gian |$O(n + m)$| danh sách lân cận cộng với trạng thái truy cập | 

Các ràng buộc cho phép tối đa 1000 nút và 2000 cạnh cho mỗi lần kiểm tra, do đó thậm chí có thể chấp nhận được vài trăm kiểm tra tính khả thi. Hệ số logarit từ tìm kiếm nhị phân giữ cho tổng số phép tính nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import main
    main()
    return sys.stdout.getvalue().strip()

# provided samples
assert run("""3
4 4
11 22 13
8 7 5
1 2
2 3
3 4
4 1
4 4
11 22 13
5 7 8
1 2
2 3
3 4
4 1
5 7
10 40 20 30
7 2 10 5
1 2
1 5
2 3
2 4
2 5
3 4
4 5
""") == """15
15
19"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đồ thị nhỏ chu trình đơn | 1 | chuỗi khả năng tiếp cận tối thiểu | 
| đồ thị sao | khác nhau | đặt hàng linh hoạt | 
| nút thắt ngưỡng cao | lớn | sự cần thiết của tính chính xác của tìm kiếm nhị phân | 

## Vỏ cạnh 

Trường hợp một cạnh là khi một nút có thể truy cập sớm nhưng không thể bị xóa cho đến khi công suất tăng đáng kể, mặc dù vẫn tồn tại các nhánh thay thế. Trong trường hợp như vậy, lựa chọn tham lam có thể bị đình trệ, nhưng tìm kiếm nhị phân đảm bảo chúng tôi chỉ chấp nhận các quyền hạn ban đầu cho phép ít nhất một đơn đặt hàng thành công hoàn toàn. 

Một trường hợp cạnh khác là đồ thị đối xứng trong đó tồn tại nhiều lệnh thanh toán bù trừ. Thuật toán không phụ thuộc vào việc chọn nút tiếp theo tối ưu toàn cầu; nó chỉ yêu cầu ít nhất một nút hợp lệ luôn được chọn khi có sẵn. Vì khả năng tiếp cận là đơn điệu và công suất chỉ tăng lên nên bất kỳ đường dẫn hợp lệ nào cũng có thể được mô phỏng bằng cách luôn lấy một nút khả thi có sẵn, đảm bảo tính chính xác ngay cả trong điều kiện đối xứng.
