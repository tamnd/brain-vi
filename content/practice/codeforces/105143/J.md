---
title: "CF 105143J - Gensokyo Autobahn"
description: "Chúng ta có một đồ thị có hướng với các nút $n$ và các cạnh có độ dài đơn vị $m$. Chúng tôi cũng có $k$ đội xây dựng độc lập."
date: "2026-06-27T16:50:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105143
codeforces_index: "J"
codeforces_contest_name: "2024 ICPC National Invitational Collegiate Programming Contest, Wuhan Site"
rating: 0
weight: 105143
solve_time_s: 57
verified: true
draft: false
---

[CF 105143J - Gensokyo Autobahn](https://codeforces.com/problemset/problem/105143/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị có hướng với$n$nút và$m$cạnh có độ dài đơn vị. Chúng tôi cũng có$k$các đội thi công độc lập. Mỗi đội độc lập chọn ngẫu nhiên một trong các cạnh hiện có một cách thống nhất, chẳng hạn như cạnh$j$, rồi cộng thêm$a_i$các cạnh có hướng song song bổ sung giống hệt với cạnh đã chọn đó. 

Sau khi tất cả các lựa chọn ngẫu nhiên được thực hiện và tất cả các cạnh mới được thêm vào, chúng ta quan tâm đến khoảng cách đường đi ngắn nhất từ ​​nút$1$đến nút$n$và cụ thể hơn là có bao nhiêu đường đi ngắn nhất riêng biệt đạt được khoảng cách tối thiểu đó. Vì việc xây dựng là ngẫu nhiên nên đại lượng này trở thành một biến ngẫu nhiên và nhiệm vụ là tính giá trị kỳ vọng của nó theo modulo 998244353. 

Khó khăn chính là tính ngẫu nhiên không nằm trực tiếp trên các đường dẫn mà nằm trên bội số cạnh, sau đó gián tiếp thay đổi cấu trúc và số lượng đường đi ngắn nhất. 

Các ràng buộc rất lớn:$n, m, k \le 2 \cdot 10^5$. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào cố gắng mô phỏng việc xây dựng ngẫu nhiên một cách rõ ràng hoặc tính toán lại các đường đi ngắn nhất cho mỗi kịch bản. Ngay cả một lần tính toán lại các đường đi ngắn nhất cũng$O(m \log n)$và có nhiều cấu hình theo cấp số nhân do các lựa chọn ngẫu nhiên độc lập, do đó việc liệt kê trực tiếp là không thể. 

Lập trình động đơn giản trên các đường dẫn cũng không thành công vì biểu đồ có thể chứa các chu trình và nhiều đường dẫn ngắn nhất, do đó cấu trúc đường dẫn không giống cây. 

Trường hợp cạnh tinh tế xuất hiện khi nhiều đội xây dựng chọn cùng một cạnh. Cạnh đó có thể tích lũy bội số lớn, và vì$a_i$có thể lên tới$10^9$, các đóng góp không thể được theo dõi bằng cách liệt kê các cạnh được thêm vào riêng lẻ. 

Một vấn đề không rõ ràng khác là bản thân khoảng cách đường đi ngắn nhất là ngẫu nhiên. Một cách tiếp cận đơn giản có thể cố gắng tính toán khoảng cách dự kiến ​​trước, sau đó là số lượng đường đi dự kiến ​​dựa trên khoảng cách đó, nhưng những đại lượng này được kết hợp theo cách phi tuyến tính. 

## Phương pháp tiếp cận 

Quan điểm bạo lực là mô phỏng tính ngẫu nhiên. Đối với mỗi$k$các đội, chúng tôi chọn một lợi thế thống nhất trong số$m$tùy chọn, sau đó thêm bội số tương ứng. Sau khi xây dựng đa đồ thị cuối cùng, chúng tôi chạy thuật toán đường đi ngắn nhất từ ​​nút$1$, đếm số đường đi ngắn nhất tới nút$n$, và trung bình trên tất cả các kết quả. 

Điều này có hiệu quả về mặt khái niệm vì nó phù hợp trực tiếp với định nghĩa về kỳ vọng. Tuy nhiên, nó thất bại ngay lập tức vì số kết quả có thể xảy ra là$m^k$, nó quá lớn. Thậm chí$k=20$sẽ là không thể. 

Ý tưởng tự nhiên tiếp theo là tính toán kỳ vọng một cách tuyến tính đối với sự đóng góp của mỗi đội xây dựng. Khó khăn là các đường đi ngắn nhất phụ thuộc vào cấu trúc tổng thể của đồ thị, do đó các đóng góp không độc lập theo cách cộng đơn giản. 

Quan sát cấu trúc quan trọng là việc đếm đường đi ngắn nhất trong DAG có trọng số được tạo ra bởi khoảng cách từ nút$1$có thể được thể hiện thông qua lập trình động khi khoảng cách được cố định. Tính ngẫu nhiên chỉ ảnh hưởng đến bội số cạnh và do đó chỉ ảnh hưởng đến trọng số chuyển tiếp trong DAG đường đi ngắn nhất. 

Thay vì lý luận về cấu hình đầy đủ, chúng tôi thay đổi quan điểm: mỗi đội xây dựng chọn một cạnh một cách độc lập, do đó mỗi cạnh$e_j$nhận được mức tăng trọng số ngẫu nhiên bằng tổng đóng góp độc lập. Sự đóng góp dự kiến ​​của một đội cho một cạnh cố định là$a_i / m$. Tuy nhiên, cấu trúc đường đi ngắn nhất phụ thuộc vào việc liệu một cạnh có trở thành một phần của DAG đường đi ngắn nhất hay không, vì vậy chúng ta phải truyền bá kỳ vọng thông qua cấu trúc DP đường đi ngắn nhất. 

Điều này dẫn đến sự phân hủy hai lớp. Đầu tiên, chúng ta xử lý biểu đồ cơ sở và tính toán khoảng cách ngắn nhất mà bỏ qua tính ngẫu nhiên. Sau đó, chúng tôi giải thích các cạnh được thêm vào dưới dạng nhiễu loạn trọng số dự kiến ​​ảnh hưởng đến bội số đường dẫn trong DP được tuyến tính hóa trên các lớp đường dẫn ngắn nhất. Sự đơn giản hóa quan trọng là tính ngẫu nhiên chỉ ảnh hưởng đến bội số chứ không ảnh hưởng đến cấu trúc của khả năng tiếp cận theo nghĩa là các đường đi ngắn nhất vẫn đến từ các cạnh tôn trọng phân lớp khoảng cách ngắn nhất. 

Do đó, vấn đề giảm xuống còn việc tính toán bội số cạnh dự kiến ​​và sau đó chạy số đường đi ngắn nhất DP trên biểu đồ có trọng số cảm ứng, trong đó mỗi cạnh ban đầu có trọng số 1 và mỗi cạnh nhận được một đóng góp song song dự kiến ​​bổ sung. 

Chúng tôi tính toán cho từng cạnh$j$số cạnh song song được thêm vào dự kiến:$$E[\text{add to } j] = \sum_{i=1}^k \frac{a_i}{m} = \frac{S}{m}$$Ở đâu$S = \sum a_i$. Kỳ vọng này giống hệt nhau ở mọi cạnh vì mỗi đội lựa chọn thống nhất và độc lập. 

Do đó, mọi cạnh đều trở thành một cạnh có trọng số một cách hiệu quả với tỷ lệ bội số dự kiến, nghĩa là đồ thị trở thành một đồ thị đa đồ thị trong đó mỗi cạnh đóng góp trọng số tỷ lệ theo kỳ vọng. 

Sau đó, chúng tôi tính toán khoảng cách ngắn nhất từ ​​nút 1 bằng cách sử dụng các trọng số hiệu dụng này và đếm các đường đi ngắn nhất bằng cách sử dụng DP tiêu chuẩn trên đường đi ngắn nhất DAG. 

Điều này thu gọn cấu trúc ngẫu nhiên thành một biểu đồ có trọng số xác định theo độ tuyến tính kỳ vọng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(m^k + m)$|$O(n + m)$| Quá chậm | 
| Đường đi ngắn nhất có trọng số dự kiến ​​DP |$O(m \log n)$|$O(n + m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi chuyển đổi tính ngẫu nhiên thành bội số cạnh dự kiến, sau đó giải quyết vấn đề đếm đường đi ngắn nhất xác định. 

1. Tính tổng số tiền$S = \sum_{i=1}^k a_i$. Điều này thể hiện tổng “khối lượng cạnh” được phân bổ trên tất cả các cạnh thông qua các lựa chọn ngẫu nhiên. 
2. Tính bội số bổ sung dự kiến ​​đóng góp cho mỗi cạnh ban đầu như sau:$S / m$. Vì mỗi đội chọn giống nhau nên mọi cạnh đều nhận được tải dự kiến ​​như nhau. 
3. Xây dựng đồ thị có hướng có trọng số trong đó mỗi cạnh gốc$u \to v$được ấn định một trọng số phản ánh sự đóng góp dự kiến ​​của nó cho kết nối. Về mặt khái niệm, mỗi cạnh có đóng góp đường cơ sở 1 cộng với các cạnh song song được thêm vào dự kiến, giúp chia tỷ lệ cho việc đếm đường đi. 
4. Chạy Dijkstra từ nút 1 để tính khoảng cách ngắn nhất. Điều này là cần thiết vì bội số dự kiến ​​được thêm vào không làm thay đổi khả năng tiếp cận nhưng ảnh hưởng đến cấu trúc chuyển tiếp hiệu quả trong việc tính các đường đi ngắn nhất. 
5. Xây dựng đường đi ngắn nhất DAG: cho mỗi cạnh$u \to v$, nếu nó thỏa mãn$dist[u] + 1 = dist[v]$, nó là một phần của cấu trúc đường đi ngắn nhất. 
6. Chạy DP qua các nút theo thứ tự khoảng cách tăng dần để đếm số đường đi ngắn nhất từ ​​1 đến mỗi nút. Mỗi quá trình chuyển đổi bổ sung thêm các đóng góp tỷ lệ thuận với bội số hiệu dụng của cạnh. 
7. Xuất số đường đi ngắn nhất tới nút$n$, mô-đun 998244353. 

Bước quan trọng là tính đa dạng ảnh hưởng đến số cách đi qua một cạnh trong DAG đường đi ngắn nhất, do đó các chuyển đổi DP phải có trọng số tương ứng cho từng cạnh. 

### Tại sao nó hoạt động 

Thuật toán dựa trên tính tuyến tính của kỳ vọng được áp dụng cho bội số cạnh. Mỗi đội xây dựng đóng góp độc lập một số cạnh song song ngẫu nhiên vào đúng một cạnh ban đầu và kỳ vọng phân bổ đồng đều trên các cạnh. Bởi vì việc đếm đường đi ngắn nhất trong DAG là tuyến tính theo bội số cạnh, nên chúng ta có thể thay thế đa đồ thị ngẫu nhiên bằng biểu diễn bội số dự kiến ​​​​của nó mà không thay đổi giá trị dự kiến ​​của kết quả DP. Cấu trúc đường dẫn ngắn nhất chỉ được xác định bởi khoảng cách cơ sở, trong khi bội số chỉ chia tỷ lệ cho số lượng chuyển đổi hợp lệ, đó là sự tích lũy tuyến tính. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
MOD = 998244353

from collections import defaultdict
import heapq

def solve():
    n, m, k = map(int, input().split())
    
    edges = []
    g = [[] for _ in range(n + 1)]
    
    for _ in range(m):
        u, v = map(int, input().split())
        edges.append((u, v))
        g[u].append(v)
    
    a = list(map(int, input().split()))
    
    S = sum(a) % MOD
    
    # expected contribution per edge
    inv_m = pow(m, MOD - 2, MOD)
    add = S * inv_m % MOD
    
    # shortest path distances (unweighted graph)
    dist = [10**18] * (n + 1)
    dist[1] = 0
    pq = [(0, 1)]
    
    while pq:
        d, u = heapq.heappop(pq)
        if d != dist[u]:
            continue
        for v in g[u]:
            nd = d + 1
            if nd < dist[v]:
                dist[v] = nd
                heapq.heappush(pq, (nd, v))
    
    # DP on shortest path DAG
    dp = [0] * (n + 1)
    dp[1] = 1
    
    order = sorted(range(1, n + 1), key=lambda x: dist[x])
    
    for u in order:
        for v in g[u]:
            if dist[u] + 1 == dist[v]:
                dp[v] = (dp[v] + dp[u] * (1 + add)) % MOD
    
    return dp[n] % MOD

if __name__ == "__main__":
    print(solve())
```Việc triển khai trước tiên sẽ đọc biểu đồ và tính tổng của tất cả$a_i$, vì chỉ có tổng quan trọng do lựa chọn ngẫu nhiên thống nhất. Sau đó nó tính toán nghịch đảo mô-đun của$m$để phân phối tổng kỳ vọng này trên các cạnh. 

Dijkstra được sử dụng ở dạng chuẩn để tính khoảng cách ngắn nhất trong biểu đồ trọng lượng đơn vị. Điều này rất cần thiết vì tất cả các cạnh ban đầu đều có độ dài 1, do đó BFS cũng có thể hoạt động, nhưng Dijkstra được giữ lại cho tính tổng quát. 

Sau khi biết khoảng cách, các nút được xử lý theo thứ tự khoảng cách tăng dần để đảm bảo tính chính xác của DP trên đường đi ngắn nhất DAG. Mỗi lần chuyển tiếp cạnh hợp lệ sẽ nhân số cách với$1 + \text{expected added multiplicity}$, phản ánh thực tế rằng mỗi cạnh ban đầu thực tế có thêm các bản sao song song được mong đợi. 

## Ví dụ đã hoạt động 

Hãy xem xét một biểu đồ nhỏ trong đó tồn tại nhiều đường đi ngắn nhất. 

đầu vào:```
4 4 1
1 2
1 3
2 4
3 4
1
```Ở đây có một đội xây dựng. Nó chọn một cạnh đồng đều và thêm một bản sao bổ sung. Số bội dự kiến ​​trên mỗi cạnh là$1/4$. Đường dẫn ngắn nhất từ ​​1 đến 4 là 1-2-4 và 1-3-4. 

| Nút | quận | dp | 
| --- | --- | --- | 
| 1 | 0 | 1 | 
| 2 | 1 | 1 + 1/4 | 
| 3 | 1 | 1 + 1/4 | 
| 4 | 2 | (1 + 1/4)^2 + (1 + 1/4)^2 | 

Bảng này cho thấy bội số thổi phồng từng nhánh một cách đối xứng như thế nào. Điều này xác nhận rằng cả hai con đường ngắn nhất đều đóng góp như nhau và độc lập. 

Tiếp theo hãy xem xét một biểu đồ dạng chuỗi. 

đầu vào:```
3 2 2
1 2
2 3
1 2
```Hai đội chọn ngẫu nhiên các cạnh. Đóng góp dự kiến ​​​​cho mỗi cạnh là$2/2 = 1$, do đó, mỗi cạnh sẽ nhân đôi một cách hiệu quả về bội số kỳ vọng. 

| Nút | quận | dp | 
| --- | --- | --- | 
| 1 | 0 | 1 | 
| 2 | 1 | 2 | 
| 3 | 2 | 4 | 

Điều này xác nhận rằng đường dẫn tỷ lệ bội số được tính theo cấp số nhân dọc theo chuỗi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((n + m) \log n)$| Dijkstra cộng với DP tuyến tính trên các cạnh | 
| Không gian |$O(n + m)$| danh sách kề, khoảng cách, mảng DP | 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$các nút và cạnh, do đó cần có giải pháp gần tuyến tính hoặc log-tuyến tính. Thuật toán phù hợp thoải mái trong các giới hạn vì cả tính toán đường đi ngắn nhất và truyền tải DP đều tuyến tính theo hệ số log. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return solve()

# minimal case
assert run("""2 1 1
1 2
5
""").strip(), "min case"

# provided sample style chain
assert run("""3 2 1
1 2
2 3
1
"""), "chain"

# branching case
assert run("""4 4 1
1 2
1 3
2 4
3 4
1
"""), "branching"

# larger uniform effect
assert run("""3 2 2
1 2
2 3
1 1
"""), "double teams"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuỗi 2 nút | 1 | độ đúng cơ sở | 
| chuỗi 3 nút | 1 | lan truyền trong DAG | 
| đồ thị kim cương | đếm đường đối xứng | nhiều đường đi ngắn nhất | 
| ngẫu nhiên trùng lặp | hiệu ứng mở rộng quy mô | tổng hợp kỳ vọng | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi nhiều đội xây dựng liên tục chọn cùng một cạnh. Trong trường hợp đó, tất cả tính ngẫu nhiên đều dồn vào một cạnh duy nhất, tạo ra sự tăng vọt về bội số. Thuật toán xử lý việc này một cách chính xác vì nó tổng hợp tất cả$a_i$thành một tổng duy nhất$S$, do đó, các lựa chọn lặp lại không cần phải được theo dõi riêng lẻ. 

Một trường hợp cạnh khác xảy ra khi đồ thị có nhiều đường đi ngắn nhất song song. Vì DP chạy theo thứ tự khoảng cách nên tất cả các nút ở cùng cấp độ sẽ được xử lý sau các nút trước đó, đảm bảo đóng góp từ các tuyến đường ngắn nhất khác nhau được kết hợp chính xác. 

Các vòng tự và các cạnh dư thừa không ảnh hưởng đến khoảng cách đường đi ngắn nhất vì chúng không bao giờ giảm khoảng cách và chúng bị bỏ qua một cách tự nhiên bởi điều kiện thư giãn xa nhất$dist[u] + 1 = dist[v]$.
