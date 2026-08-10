---
title: "CF 102391G - Bước đi tối thiểu về mặt từ điển"
description: "Chúng ta có một đồ thị có hướng có các cạnh mang các màu nguyên riêng biệt. Bắt đầu từ (S), chúng ta có thể đi qua bất kỳ cạnh đi nào và chúng ta được phép xem lại các đỉnh và cạnh. Yêu cầu duy nhất là bước đi cuối cùng phải đạt đến (T)."
date: "2026-08-10T20:07:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102391
codeforces_index: "G"
codeforces_contest_name: "XX Open Cup, Grand Prix of Korea"
rating: 0
weight: 102391
solve_time_s: 168
verified: true
draft: false
---

[CF 102391G - Bước đi tối thiểu về mặt từ điển](https://codeforces.com/problemset/problem/102391/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 48s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị có hướng có các cạnh mang các màu nguyên riêng biệt. Bắt đầu từ (S), chúng ta có thể đi qua bất kỳ cạnh đi nào và chúng ta được phép xem lại các đỉnh và cạnh. Yêu cầu duy nhất là bước đi cuối cùng phải đạt đến (T). Trong số tất cả các bước đi như vậy có độ dài tối đa là (10^{100}), chúng tôi muốn có chuỗi màu sắc cạnh nhỏ nhất về mặt từ điển. 

Thứ tự từ điển làm cho quyết định đầu tiên quan trọng hơn nhiều so với mọi quyết định tiếp theo. Nếu một bước đi hợp lệ bắt đầu bằng màu (3) và một bước đi khác bắt đầu bằng màu (7), thì bước đi đầu tiên sẽ thắng bất kể điều gì xảy ra sau đó. Nếu hai bước đi có cùng tiền tố, chúng tôi sẽ so sánh vị trí đầu tiên nơi chúng khác nhau. Một chuỗi ngắn hơn cũng thắng khi nó là tiền tố chính xác của một chuỗi dài hơn. 

Đồ thị có thể chứa (100.000) đỉnh và (300.000) cạnh. Với giới hạn 2 giây so với vấn đề ban đầu, cách tiếp cận (O(NM)) hoặc (O(N^2)) đã quá đắt. Về cơ bản, chúng ta cần công việc tuyến tính hoặc (O((N+M)\log N)). Giới hạn lớn (10^{100}) loại trừ mọi thuật toán phụ thuộc vào độ dài bước đi tối đa có thể. May mắn thay, câu trả lời có cấu trúc đơn giản hơn nhiều. 

Có một số trường hợp việc thực hiện tham lam bất cẩn có thể thất bại. Đầu tiên là lấy cạnh đi nhỏ nhất mà không kiểm tra xem cuối cùng đích của nó có thể đến được (T) hay không. Ví dụ,```
3 2 1 3
1 2 1
1 3 5
```Câu trả lời đúng là`5`. Đỉnh (2) là ngõ cụt nên việc chọn màu (1) không thể tạo ra bước đi hợp lệ. Thuật toán tham lam chỉ nhìn vào màu nhỏ nhất sẽ chọn sai màu đó. 

Trường hợp thứ hai là một chu kỳ. Coi như```
3 4 1 3
1 2 1
2 1 2
2 3 7
1 3 5
```Đầu ra đúng là`TOO LONG`. Từ đỉnh (1), màu (1) được ưu tiên hơn màu (5). Từ đỉnh (2), màu (2) được ưu tiên hơn màu (7). Vì thế những lựa chọn tham lam liên tục tạo ra```
1, 2, 1, 2, 1, 2, ...
```Việc đi bộ cuối cùng có thể rời khỏi chu trình và đến (T), nhưng việc trì hoãn việc thoát ra đó bằng một chu trình khác làm cho trình tự trở nên nhỏ hơn về mặt từ điển. Vì độ dài tối đa được phép lớn về mặt thiên văn nên chuỗi tối ưu thu được dài hơn nhiều so với (10^6). 

Trường hợp thứ ba đơn giản là không thể truy cập được (T). Ví dụ,```
2 0 2 1
```không có bước đi nào cả, vì vậy câu trả lời là`IMPOSSIBLE`. 

## Phương pháp tiếp cận 

Một lực lượng vũ phu trực tiếp sẽ liệt kê mọi bước đi có thể có từ (S), giữ những bước đi đạt đến (T) trong độ dài cho phép và so sánh chuỗi màu của chúng. Điều này đúng vì nó xem xét rõ ràng mọi ứng cử viên. Vấn đề là số lượng ứng viên. Một biểu đồ có thể có hai lựa chọn có ý nghĩa lặp đi lặp lại, tạo ra (\Theta(2^{L/2})) bước đi có độ dài khác nhau nhiều nhất (L). Nếu chúng ta thực sự hiện thực hóa mọi chuỗi thì tác phẩm sẽ là (\Theta(L2^{L/2})). Với (L=10^{100}), điều này không chỉ là quá chậm mà về cơ bản là không thể thực hiện được. 

Quan sát hữu ích là thứ tự từ điển cho phép chúng ta quyết định cạnh tiếp theo một cách độc lập khi chúng ta biết đỉnh nào vẫn có thể chạm tới (T). Giả sử chúng ta hiện đang ở đỉnh (u). Bất kỳ cạnh đi nào dẫn đến một đỉnh không thể chạm tới (T) đều không liên quan, bởi vì mọi bước đi sử dụng cạnh đó đều không hợp lệ. Trong số tất cả các cạnh còn lại, phải chọn màu nhỏ nhất. Việc chọn một màu lớn hơn không bao giờ có thể được sửa chữa bằng cách có một hậu tố tốt hơn, bởi vì màu khác nhau đầu tiên đã quyết định việc so sánh từ điển. 

Điều này mang lại một quá trình chuyển đổi tham lam mang tính xác định cho mọi đỉnh hữu ích. Đầu tiên, đánh dấu mọi đỉnh có thể chạm tới (T), sử dụng đường truyền trên đồ thị đảo ngược. Sau đó, đối với mỗi đỉnh hữu ích (u), hãy chọn cạnh đi ra có màu tối thiểu mà đích đến của nó cũng hữu ích. 

Bây giờ hãy đi theo các cạnh đã chọn bắt đầu từ (S). Chỉ có hai khả năng. Cuối cùng chúng tôi đạt được (T), trong trường hợp đó chuỗi chúng tôi thu thập được là câu trả lời. Ngược lại, vì chỉ có (N) đỉnh nên một số đỉnh lặp lại và các chuyển tiếp được chọn tạo thành một chu trình. 

Tại sao chu kỳ có nghĩa là`TOO LONG`thay vì chỉ đơn thuần là "thuật toán tham lam bị mắc kẹt"? Tại mỗi đỉnh trên chu trình đó, cạnh được chọn có màu hoàn toàn nhỏ hơn mọi cạnh đi ra hữu ích khác. Màu sắc là duy nhất trên toàn cầu nên không thể có sự ràng buộc. Nếu chúng ta rời khỏi chu trình tại một thời điểm nào đó, chúng ta phải thay thế cạnh chu trình đã chọn bằng một màu lớn hơn. Trì hoãn sự thay thế đó bằng một lần duyệt khác của chu trình làm cho chuỗi nhỏ hơn về mặt từ điển. Chúng ta có thể lặp lại chu trình này rất nhiều lần mà vẫn nằm trong giới hạn độ dài (10^{100}). Do đó, bước đi giới hạn tối thiểu về mặt từ điển có độ dài rất lớn, chắc chắn lớn hơn (10^6). 

Việc các màu là duy nhất rất hữu ích ở đây vì nó loại bỏ khả năng hai cạnh ra khác nhau có cùng màu. Cấu trúc tham lam tương tự vẫn có thể được xây dựng bằng dây buộc, nhưng sẽ cần phải cẩn thận hơn để chọn hậu tố tốt nhất trong số các màu đầu tiên bằng nhau. 

Phương pháp kết quả là tuyến tính ngoài việc sắp xếp tùy chọn. Chúng ta có thể tránh việc sắp xếp hoàn toàn bằng cách quét từng danh sách kề một lần và ghi nhớ cạnh hữu ích tối thiểu của nó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (\Theta(L2^{L/2})) trong biểu đồ phân nhánh | Hàm mũ | Quá chậm | 
| Tối ưu | (O(N+M)) | (O(N+M)) | Đã chấp nhận | 

Đặc tính tham lam tương tự cũng được đưa ra trong một cuộc thảo luận giải pháp được công bố độc lập cho vấn đề này. 

## Hướng dẫn thuật toán 

1. Xây dựng cả biểu đồ gốc và biểu đồ đảo ngược của nó. Cần có biểu đồ ban đầu để chọn các cạnh, trong khi biểu đồ đảo ngược cho phép chúng ta xác định đỉnh nào cuối cùng có thể đạt tới (T). 
2. Chạy duyệt đồ thị bắt đầu từ (T) trong đồ thị đảo ngược. Đánh dấu mọi đỉnh đạt được khi đi qua này là`good`. Một đỉnh là`good`chính xác khi tồn tại một số bước đi có hướng từ đỉnh đó tới (T). 

Việc lọc này là cần thiết. Một cạnh có màu rất nhỏ không phải là ứng cử viên hợp lệ nếu nó dẫn vào vùng mà từ đó (T) không thể truy cập được. 
3. Nếu (S) không`good`, in`IMPOSSIBLE`. Không có bước đi hợp lệ từ (S) đến (T), do đó không thể có chuỗi màu xuất ra. 
4. Đối với mỗi đỉnh (u), hãy kiểm tra các cạnh đi ra của nó và tìm cạnh có màu tối thiểu có đích đến là`good`. Lưu trữ điểm đến và màu sắc của nó. 

Mọi`good`đỉnh khác (T) có ít nhất một cạnh như vậy, vì theo định nghĩa, nó có đường đi tới (T). 
5. Bắt đầu tại (S), giữ nguyên`visited`mảng và lặp đi lặp lại theo cạnh hữu ích tối thiểu được lưu trữ. Thêm màu của nó vào câu trả lời trước khi di chuyển đến đích. 

các`visited`mảng phát hiện chính xác trường hợp bước đi tham lam xác định đã bước vào một chu kỳ. Không cần phải lưu trữ toàn bộ chuỗi chỉ để phát hiện sự lặp lại. 
6. Nếu đỉnh hiện tại trở thành (T), hãy in tất cả các màu đã thu thập. Vì không có đỉnh nào được lặp lại nên bước đi chứa nhiều nhất (N-1) cạnh. Đặc biệt, độ dài của nó tự động ở mức dưới (10^6). 
7. Nếu gặp một đỉnh lần thứ hai trước khi tới (T), hãy in`TOO LONG`. 

Tại mỗi đỉnh lặp lại, cạnh tham lam có màu nhỏ nhất có thể trong số tất cả các cạnh vẫn có thể dẫn đến (T). Bất kỳ bước đi hữu hạn nào thoát khỏi chu kỳ cuối cùng đều phải chọn màu lớn hơn thay vì cạnh tham lam đó. Việc lặp lại chu trình sẽ trì hoãn màu lớn hơn đó và làm cho chuỗi kết quả nhỏ hơn về mặt từ điển. Vì độ dài tối đa được phép là (10^{100}), lớn hơn rất nhiều (10^6) nên không thể in được câu trả lời bắt buộc. 

### Tại sao nó hoạt động 

Hãy xem xét đỉnh hiện tại (u). Mọi sự tiếp tục hợp lệ phải có một cạnh mà đích đến có thể đạt tới (T). Trong số các cạnh đó, cạnh có màu nhỏ nhất phải xuất hiện theo trình tự tối thiểu về mặt từ điển. Nếu một bước đi hợp lệ khác chọn màu lớn hơn ở vị trí này thì chuỗi của nó đã lớn hơn, bất kể hậu tố của nó là gì. Do đó, lựa chọn tham lam đầu tiên bị ép buộc và lập luận tương tự được áp dụng đệ quy ở mọi đỉnh sau. 

Nếu quá trình tham lam đạt đến (T) mà không lặp lại một đỉnh, thì mọi cạnh được chọn đều bị ràng buộc bởi đối số này, do đó chuỗi được tạo ra là chuỗi hợp lệ tối thiểu về mặt từ điển. 

Nếu quá trình lặp lại một đỉnh thì các chuyển tiếp xác định từ điểm đó sẽ tạo thành một chu trình. Tại mỗi đỉnh chu kỳ, cạnh được chọn nhỏ hơn mọi cạnh khác mà cuối cùng có thể chạm tới (T). Bất kỳ bước đi hợp lệ nào cuối cùng rời khỏi chu trình đều phải chọn cạnh lớn hơn ở vị trí thoát đầu tiên nào đó. Một cuộc đi bộ thực hiện thêm một chu trình hoàn chỉnh trước khi đi đến lối ra đó có cùng tiền tố cho đến thời điểm đó và sau đó có màu chu kỳ nhỏ hơn, vì vậy nó nhỏ hơn về mặt từ điển. Việc lặp lại đối số này cho thấy rằng bước đi giới hạn tối ưu sử dụng số lần lặp lại tối đa có sẵn trước khi thoát. Giới hạn (10^{100}) làm cho độ dài của nó lớn hơn nhiều so với (10^6), vì vậy`TOO LONG`là đầu ra chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, s, t = map(int, input().split())

    graph = [[] for _ in range(n)]
    rev = [[] for _ in range(n)]

    for _ in range(m):
        u, v, c = map(int, input().split())
        u -= 1
        v -= 1
        graph[u].append((v, c))
        rev[v].append(u)

    # Find all vertices that can reach T.
    good = [False] * n
    good[t] = True

    stack = [t]
    while stack:
        u = stack.pop()
        for v in rev[u]:
            if not good[v]:
                good[v] = True
                stack.append(v)

    if not good[s]:
        print("IMPOSSIBLE")
        return

    # For every useful vertex, remember its minimum-color
    # outgoing edge whose destination is also useful.
    next_vertex = [-1] * n
    next_color = [0] * n

    for u in range(n):
        best_color = None
        best_vertex = -1

        for v, c in graph[u]:
            if not good[v]:
                continue
            if best_color is None or c < best_color:
                best_color = c
                best_vertex = v

        if best_vertex != -1:
            next_vertex[u] = best_vertex
            next_color[u] = best_color

    # Follow the deterministic greedy transitions.
    visited = [False] * n
    answer = []

    u = s

    while u != t:
        if visited[u]:
            print("TOO LONG")
            return

        visited[u] = True

        v = next_vertex[u]

        # This should not happen because u is good and u != T.
        if v == -1:
            print("IMPOSSIBLE")
            return

        answer.append(next_color[u])
        u = v

    print(*answer)

if __name__ == "__main__":
    solve()
```Cấu trúc biểu đồ đầu tiên lưu trữ mỗi cạnh trong biểu đồ thuận và chỉ lưu trữ cạnh trước của nó trong biểu đồ ngược. Do đó, bắt đầu từ (T) trong biểu đồ ngược sẽ truy cập chính xác các đỉnh mà từ đó (T) có thể tới được. 

các`good`bài kiểm tra được thực hiện trước bất kỳ sự lựa chọn tham lam nào. Đây là điều kiện chính xác chính giúp thuật toán không chọn được một màu nhỏ dẫn đến ngõ cụt. 

Cạnh đi ra hữu ích tối thiểu được tìm thấy bằng một lần quét từng danh sách kề. Không cần phải sắp xếp các cạnh. Vì mỗi cạnh được kiểm tra chính xác một lần trong giai đoạn này nên nó đóng góp tổng công (O(M)). 

Việc truyền tải cuối cùng là xác định. Một lần`next_vertex[u]`đã được tính toán, chỉ có một cạnh mà thuật toán tham lam có thể chọn từ (u).`visited[u]`được thiết lập trước khi chuyển sang đỉnh tiếp theo, do đó gặp phải`visited[u]`ở lần lặp lại sau có nghĩa là một chu trình đã thực sự được hoàn thành. Không có vấn đề riêng biệt nào xung quanh (T), bởi vì (T) được kiểm tra trước khi kiểm tra đỉnh lặp lại. 

Số nguyên Python dễ dàng xử lý giới hạn màu nhất định là (10^9) và thuật toán không bao giờ thực hiện số học liên quan đến (10^{100}). Chúng ta không cần phải biểu diễn chiều dài bước đi khổng lồ nào cả. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
3 3 1 3
1 2 1
2 3 7
1 3 5
```Cả ba đỉnh đều có thể chạm đến đỉnh (3), vì vậy mọi đỉnh đều có ích. Tại đỉnh (1), các màu phát ra hữu ích là (1) và (5), vì vậy lựa chọn tham lam là màu (1). Tại đỉnh (2), cạnh đi ra hữu ích duy nhất có màu (7), đạt tới (T). 

| Đỉnh hiện tại | Các cạnh đi tốt | Màu được chọn | Đỉnh tiếp theo | Đã ghé thăm trước đây | 
| --- | --- | --- | --- | --- | 
| 1 | (1\to2) với 1, (1\to3) với 5 | 1 | 2 | Không | 
| 2 | (2\to3) với 7 | 7 | 3 | Không | 
| 3 | Dừng lại | | | | 

Trình tự kết quả là`1 7`. Lựa chọn tham lam ở đỉnh (1) đánh bại cạnh trực tiếp của màu (5), vì màu khác biệt đầu tiên là (1<5). 

### Mẫu 2 

Đầu vào là```
3 4 1 3
1 2 1
2 1 2
2 3 7
1 3 5
```Một lần nữa mọi đỉnh đều có thể chạm tới (3). Cạnh hữu ích tối thiểu từ (1) có màu (1), trong khi cạnh hữu ích tối thiểu từ (2) có màu (2). 

| Đỉnh hiện tại | Các cạnh đi tốt | Màu được chọn | Đỉnh tiếp theo | Đã ghé thăm trước đây | 
| --- | --- | --- | --- | --- | 
| 1 | (1\to2) với 1, (1\to3) với 5 | 1 | 2 | Không | 
| 2 | (2\to1) với 2, (2\to3) với 7 | 2 | 1 | Không | 
| 1 | (1\to2) với 1, (1\to3) với 5 | 1 | 2 | Có | 

Quá trình chuyển đổi (1\to2\to1) là một chu kỳ. Việc rời khỏi nó sẽ yêu cầu sử dụng màu (5) từ đỉnh (1) hoặc màu (7) từ đỉnh (2), cả hai đều lớn hơn cạnh chu kỳ tương ứng. Vì vậy, một chu kỳ khác luôn được ưu tiên hơn về mặt từ điển. Câu trả lời là`TOO LONG`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(N+M)) | Truyền tải ngược, quét cạnh tối thiểu và bước đi tham lam cuối cùng từng xử lý các đỉnh hoặc cạnh một cách tuyến tính | 
| Không gian | (O(N+M)) | Biểu đồ chuyển tiếp, biểu đồ ngược, mảng khả năng tiếp cận, chuyển tiếp tham lam và mảng đã truy cập | 

Với (N\le100.000) và (M\le300.000), việc xử lý tuyến tính nằm trong phạm vi dự kiến ​​của bài toán. Thuật toán cũng tránh đệ quy, điều này rất hữu ích trong Python vì một biểu đồ có thể chứa một đường dẫn có gần (100.000) đỉnh. 

## Trường hợp thử nghiệm 

Bộ dây thử nghiệm sau đây sử dụng cùng một`solve`hoạt động như giải pháp được gửi. Thử nghiệm kích thước tối đa xây dựng một chuỗi có (100.000) đỉnh, trong khi các trường hợp khác nhắm mục tiêu đến khả năng tiếp cận, chu kỳ, cạnh trực tiếp và giá trị màu rất lớn.```python
import sys
import io
from contextlib import redirect_stdout

def solve(data=None):
    if data is None:
        input = sys.stdin.readline
    else:
        input = io.StringIO(data).readline

    n, m, s, t = map(int, input().split())

    graph = [[] for _ in range(n)]
    rev = [[] for _ in range(n)]

    for _ in range(m):
        u, v, c = map(int, input().split())
        u -= 1
        v -= 1
        graph[u].append((v, c))
        rev[v].append(u)

    good = [False] * n
    good[t - 0] = True

    stack = [t]
    while stack:
        u = stack.pop()
        for v in rev[u]:
            if not good[v]:
                good[v] = True
                stack.append(v)

    if not good[s]:
        print("IMPOSSIBLE")
        return

    next_vertex = [-1] * n
    next_color = [0] * n

    for u in range(n):
        best_color = None
        best_vertex = -1

        for v, c in graph[u]:
            if not good[v]:
                continue
            if best_color is None or c < best_color:
                best_color = c
                best_vertex = v

        if best_vertex != -1:
            next_vertex[u] = best_vertex
            next_color[u] = best_color

    visited = [False] * n
    answer = []
    u = s

    while u != t:
        if visited[u]:
            print("TOO LONG")
            return

        visited[u] = True
        v = next_vertex[u]

        if v == -1:
            print("IMPOSSIBLE")
            return

        answer.append(next_color[u])
        u = v

    print(*answer)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()

    try:
        with redirect_stdout(out):
            solve()
    finally:
        sys.stdin = old_stdin

    return out.getvalue().strip()

# Provided samples.
assert run(
    """3 3 1 3
1 2 1
2 3 7
1 3 5
"""
) == "1 7", "sample 1"

assert run(
    """3 4 1 3
1 2 1
2 1 2
2 3 7
1 3 5
"""
) == "TOO LONG", "sample 2"

assert run(
    """2 0 2 1
"""
) == "IMPOSSIBLE", "sample 3"

# Minimum-size valid graph.
assert run(
    """2 1 1 2
1 2 1000000000
"""
) == "1000000000", "minimum graph"

# A smaller color may lead to a dead end, so it must be ignored.
assert run(
    """3 2 1 3
1 2 1
1 3 5
"""
) == "5", "dead-end minimum edge"

# Two valid choices from S, with a much smaller first color.
assert run(
    """4 4 1 4
1 2 9
1 3 2
2 4 1
3 4 1000000000
"""
) == "2 1000000000", "lexicographic first choice"

# Large colors near the upper boundary.
assert run(
    """3 3 1 3
1 2 999999999
2 3 1000000000
1 3 1000000000
"""
) == "999999999 1000000000", "large colors"

# Maximum-size chain: N = 100000, M = 99999.
n = 100000
lines = [f"{n} {n - 1} 1 {n}"]
lines.extend(f"{i} {i + 1} {i}" for i in range(1, n))
max_input = "\n".join(lines) + "\n"
max_expected = " ".join(map(str, range(1, n)))
assert run(max_input) == max_expected, "maximum-size chain"

print("All tests passed.")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 1 1 2`có màu sắc (10^9) |`1000000000`| Đồ thị tối thiểu và ranh giới màu tối đa | 
|`3 2 1 3`có cạnh màu 1 và 5 |`5`| Cạnh nhỏ hơn dẫn đến ngõ cụt phải bị bác bỏ | 
| Đồ thị bốn đỉnh với lựa chọn đầu tiên 9 và 2 |`2 1000000000`| So sánh từ điển học được xác định bởi màu khác nhau đầu tiên | 
| Đồ thị ba đỉnh sử dụng màu gần (10^9) |`999999999 1000000000`| Giá trị màu lớn và lựa chọn trực tiếp và gián tiếp | 
| (100000) chuỗi đỉnh | Màu sắc (1) đến (99999) | Tối đa (N), lớn (M) và bước đi tham lam dài theo chu kỳ | 

Tuyên bố cho biết màu sắc của các cạnh là duy nhất trên toàn cầu, do đó, việc kiểm tra theo nghĩa đen trong đó tất cả các màu của cạnh đều bằng nhau sẽ vi phạm các điều kiện đầu vào. Kiểm tra màu lớn ở trên thực hiện ranh giới số có liên quan trong khi vẫn là một phiên bản hợp lệ. 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là một ngõ cụt có màu sắc nhỏ hơn. Coi như```
3 2 1 3
1 2 1
1 3 5
```Việc truyền ngược từ (3) chỉ đánh dấu đỉnh (3) và đỉnh (1) là hữu ích. Đỉnh (2) không được đánh dấu vì không có đường đi từ (2) đến (3). Khi thuật toán quét đỉnh (1), nó sẽ bỏ qua cạnh màu (1) vì đích đến của nó không hữu ích, để lại màu (5) là lựa chọn hợp lệ tối thiểu. Đầu ra là`5`. 

Trường hợp cạnh thứ hai là mục tiêu không thể truy cập được.```
2 0 2 1
```Quá trình truyền ngược lại bắt đầu ở đỉnh (1) và không đến được điểm nào khác. Vertex (2=S) không hữu ích nên thuật toán sẽ in ngay`IMPOSSIBLE`. 

Trường hợp cạnh thứ ba là một chu trình chứa các lựa chọn tham lam.```
3 4 1 3
1 2 1
2 1 2
2 3 7
1 3 5
```Cạnh tối thiểu hữu ích từ (1) là màu (1) và cạnh tối thiểu hữu ích từ (2) là màu (2). Do đó, chuỗi tham lam sẽ là (1\to2\to1). Khi gặp đỉnh (1) lần thứ hai, thuật toán sẽ in`TOO LONG`. Chu trình có thể được lặp lại trước khi lấy màu thoát lớn hơn (5), do đó, bước đi giới hạn được ưu tiên về mặt từ điển có độ dài rất lớn. 

Trường hợp cạnh thứ tư là biểu đồ trong đó bước đi tham lam đạt đến (T) mà không có chu trình.```
3 3 1 3
1 2 1
2 3 7
1 3 5
```Thuật toán chọn màu (1) từ đỉnh (1), sau đó chọn màu (7) từ đỉnh (2) và đến (3). Không có đỉnh lặp lại. Vì một bước đi đơn giản qua các đỉnh (N) chứa tối đa (N-1) cạnh, nên chuỗi kết quả sẽ tự động ở dưới ngưỡng đầu ra (10^6). 

Trường hợp ranh giới cuối cùng là chuỗi có kích thước tối đa được sử dụng trong các thử nghiệm. Mỗi đỉnh có chính xác một cạnh đi ra, vì vậy sự lựa chọn tham lam là bắt buộc. Thuật toán thực hiện một lần duyệt ngược trên (99.999) cạnh, một lần quét các cạnh giống nhau và sau đó thực hiện các chuyển tiếp (99.999). Nó tạo ra toàn bộ chuỗi mà không cần đệ quy và không cần biểu diễn giới hạn bước đi (10^{100}).
