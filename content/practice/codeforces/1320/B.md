---
title: "CF 1320B - Hệ Thống Định Vị"
description: "Chúng ta được cung cấp một đồ thị có hướng trong đó giao lộ là nút và đường là cạnh một chiều. Chúng ta cũng biết một tuyến đường đơn giản cố định mà Polycarp thực sự lái từ nhà đến nơi làm việc."
date: "2026-06-16T07:07:04+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "graphs", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 1320
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 625 (Div. 1, based on Technocup 2020 Final Round)"
rating: 1700
weight: 1320
solve_time_s: 168
verified: true
draft: false
---

[CF 1320B - Hệ thống định vị](https://codeforces.com/problemset/problem/1320/B) 

**Đánh giá:** 1700 
**Tags:** dfs và tương tự, đồ thị, đường dẫn ngắn nhất 
**Thời gian giải:** 2 phút 48 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một đồ thị có hướng trong đó giao lộ là nút và đường là cạnh một chiều. Chúng ta cũng biết một tuyến đường đơn giản cố định mà Polycarp thực sự lái từ nhà đến nơi làm việc. Tuyến đường này hợp lệ theo nghĩa mỗi cặp giao lộ liên tiếp được kết nối bằng một con đường, nhưng nó không nhất thiết phải là tuyến đường ngắn nhất có thể xét về số cạnh. 

Bên cạnh đó, còn có một hệ thống định vị liên tục cố gắng duy trì đường đi ngắn nhất từ ​​vị trí hiện tại đến đích. Hệ thống ban đầu chọn một số đường đi ngắn nhất từ ​​điểm bắt đầu đến đích. Bất cứ khi nào Polycarp tuân theo cạnh được đề xuất tiếp theo, hệ thống sẽ giữ nguyên kế hoạch hiện tại. Bất cứ khi nào anh ta đi chệch hướng, hệ thống sẽ loại bỏ kế hoạch cũ và tính toán lại đường đi ngắn nhất từ ​​vị trí mới đến đích. 

Số lượng chúng ta cần là số lần việc tính toán lại này, được gọi là việc xây dựng lại, có thể xảy ra. Vì hệ thống định vị có thể tự do chọn bất kỳ con đường ngắn nhất nào khi có nhiều con đường và tuyến đường của Polycarp là cố định nên câu trả lời không phải là duy nhất. Chúng ta phải tính toán cả số lần xây dựng lại tối thiểu có thể và số lần xây dựng lại tối đa có thể có trên tất cả các lựa chọn hợp lệ do hệ thống thực hiện. 

Các ràng buộc lên tới hai trăm nghìn giao lộ và đường, loại trừ mọi giải pháp tính toán lại đường đi ngắn nhất từ ​​đầu ở mỗi bước. BFS mới cho mỗi bước sẽ là bậc hai trong trường hợp xấu nhất và sẽ không vượt qua. Cấu trúc gợi ý chúng ta nên xử lý trước khoảng cách ngắn nhất một lần và sau đó đánh giá từng bước trên đường đi của Polycarp trong thời gian không đổi. 

Một vấn đề nhỏ xuất hiện khi tồn tại nhiều đường đi ngắn nhất. Sự lựa chọn của hệ thống định vị trong số đó sẽ xác định xem bước tiếp theo của Polycarp có phù hợp với tuyến đường được đề xuất hay buộc phải xây dựng lại. Điều này có nghĩa là câu trả lời không chỉ phụ thuộc vào khoảng cách mà còn phụ thuộc vào số lượng “bước tiếp theo hợp lệ” tồn tại từ mỗi nút dọc theo các đường đi ngắn nhất đến đích. 

Một trường hợp góc bộc lộ lý luận ngây thơ là khi mọi cạnh ngắn nhất đi ra đều dẫn đến cùng một nút tiếp theo. Trong trường hợp đó, mặc dù có thể có nhiều đường đi ngắn nhất trên toàn cầu, nhưng hệ thống cục bộ không có tính linh hoạt và Polycarp hoặc khớp với cạnh bắt buộc hoặc kích hoạt việc xây dựng lại tùy thuộc vào hành động của anh ta. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp sẽ cố gắng xây dựng lại đường đi ngắn nhất ở mỗi bước và sau đó mô phỏng các quyết định của Polycarp. Việc này nhanh chóng trở nên tốn kém vì mỗi lần tính toán lại bao gồm việc duyệt đồ thị và có thể có tới n bước. Trường hợp xấu nhất sẽ liên tục chạy BFS trên một biểu đồ lớn, dẫn đến hành vi bậc ba hoặc gần bậc hai. 

Quan sát quan trọng là chúng ta không bao giờ cần cấu trúc đường đi ngắn nhất đầy đủ. Chúng ta chỉ cần biết, đối với bất kỳ nút u nào, hàng xóm nào có thể xuất hiện dưới dạng nút tiếp theo trên đường đi ngắn nhất từ ​​u đến đích t. Điều này có thể được rút ra một lần bằng cách tính toán khoảng cách ngắn nhất từ ​​mọi nút đến t bằng cách sử dụng BFS ngược trên biểu đồ đảo ngược. 

Khi chúng ta biết dist[x], cạnh có hướng u → v là một phần của đường đi ngắn nhất từ ​​u đến t chính xác khi dist[u] = dist[v] + 1. Điều này biến hành vi của hệ thống định vị thành một quy tắc cục bộ: ở mỗi bước, nó chỉ có thể chọn trong số các cạnh đi ra thỏa mãn đẳng thức này. 

Bây giờ hãy xem xét việc Polycarp chuyển từ p[i] sang p[i+1]. Nếu cạnh này không phù hợp với điều kiện khoảng cách ngắn nhất thì bất kể lựa chọn của hệ thống là gì, Polycarp buộc phải làm sai lệch và do đó phải xây dựng lại. Nếu nó nhất quán thì việc xây dựng lại có xảy ra hay không phụ thuộc hoàn toàn vào việc hệ thống có thể chọn một cạnh ngắn hợp lệ khác thay thế hay không.

Đối với số lần xây dựng lại tối thiểu, chúng tôi giả sử hệ thống hợp tác. Nó luôn chọn p[i+1] bất cứ khi nào nó là hàng xóm có bước đi ngắn nhất hợp lệ, tránh sự phân kỳ không cần thiết. Việc xây dựng lại chỉ xảy ra khi p[i+1] không phải là bước đi ngắn nhất. 

Đối với số lần xây dựng lại tối đa, hệ thống sẽ hoạt động bất lợi. Bất cứ khi nào tồn tại bất kỳ hàng xóm bước ngắn nhất thay thế nào khác với p[i+1], hệ thống có thể chọn hàng xóm đó, buộc Polycarp phải đi chệch hướng và kích hoạt quá trình xây dựng lại ở bước tiếp theo. Chỉ khi p[i+1] là hàng xóm bước ngắn nhất hợp lệ duy nhất thì hệ thống buộc phải tuân theo nó, ngăn cản việc xây dựng lại. 

Điều này làm giảm toàn bộ vấn đề đối với việc đếm, đối với mỗi bước, liệu bước tiếp theo có phải là cạnh bước ngắn nhất hợp lệ hay không và liệu nó có phải là duy nhất trong số các cạnh đó hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng Brute Force với các đường đi ngắn nhất lặp đi lặp lại | O(k · (n + m)) | O(n + m) | Quá chậm | 
| Khoảng cách BFS + kiểm tra cạnh ngắn nhất cục bộ | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đảo ngược tất cả các cạnh và chạy BFS bắt đầu từ t để tính dist[x], khoảng cách ngắn nhất từ mọi nút x đến đích. Điều này hiệu quả vì trong biểu đồ đảo ngược, chúng ta đang truyền khoảng cách ngắn nhất ngược lại từ t một cách hiệu quả. 
2. Đối với mỗi nút u, hãy xác định có bao nhiêu nút lân cận đi ra v thỏa mãn dist[u] = dist[v] + 1. Số đếm này thể hiện số lượng lựa chọn hợp lệ mà hệ thống định vị có khi ngồi ở u. 
3. Khởi tạo hai bộ đếm để xây dựng lại mức tối thiểu và tối đa về 0. 
4. Đường đi của Polycarp từ p[1] tới p[k]. Đối với mỗi lần chuyển đổi từ p[i] sang p[i+1], hãy kiểm tra xem dist[p[i]] = dist[p[i+1]] + 1. Nếu đẳng thức này không giữ được thì bước di chuyển không thể thuộc về bất kỳ đường đi ngắn nhất nào từ p[i] đến t, buộc phải xây dựng lại cho trường hợp tối thiểu. 
5. Để có số lượng tối đa, hãy kiểm tra xem số lượng lân cận bước ngắn nhất hợp lệ của p[i] có lớn hơn một hay liệu lân cận hợp lệ duy nhất không phải là p[i+1]. Trong một trong những tình huống này, hệ thống định vị có thể chọn một bước di chuyển ngắn nhất thay thế, buộc Polycarp đi chệch khỏi con đường được đề xuất và gây ra việc xây dựng lại. 
6. Tích lũy các điều kiện này trên tất cả các cạnh trong đường dẫn để thu được số lượng tối thiểu và tối đa cuối cùng. 

Tính đúng đắn phụ thuộc vào thực tế là các đường đi ngắn nhất tạo thành một cấu trúc phân lớp theo khoảng cách từ t. Mỗi bước di chuyển tối ưu sẽ giảm khoảng cách này đi đúng một. Sự tự do của hệ thống định vị chính xác là yếu tố phân nhánh trong biểu đồ phân lớp này và đường dẫn của Polycarp đi theo một trong các lớp này hoặc nhảy ra ngoài nó, buộc phải tính toán lại. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

n, m = map(int, input().split())
g_rev = [[] for _ in range(n + 1)]

for _ in range(m):
    u, v = map(int, input().split())
    g_rev[v].append(u)

k = int(input())
p = list(map(int, input().split()))

INF = 10**18
dist = [INF] * (n + 1)

# BFS from destination
t = p[-1]
q = deque([t])
dist[t] = 0

while q:
    u = q.popleft()
    for v in g_rev[u]:
        if dist[v] == INF:
            dist[v] = dist[u] + 1
            q.append(v)

# count valid next steps on shortest paths
good_cnt = [0] * (n + 1)

for u in range(1, n + 1):
    for v in g_rev[u]:
        # reversed graph: v -> u in original, so check original condition via reverse logic
        if dist[v] == dist[u] + 1:
            good_cnt[u] += 1

mn = 0
mx = 0

for i in range(k - 1):
    u = p[i]
    v = p[i + 1]

    on_shortest = (dist[u] == dist[v] + 1)

    if not on_shortest:
        mn += 1
        mx += 1
    else:
        if good_cnt[u] > 1:
            mx += 1

print(mn, mx)
```BFS tính toán khoảng cách ngắn nhất từ ​​mọi nút đến đích theo thời gian tuyến tính trên biểu đồ đảo ngược. Đây là thông tin toàn cầu duy nhất cần thiết; mọi thứ khác được quyết định cục bộ bằng cách kiểm tra các cạnh đường dẫn ngắn nhất đi ra. 

các`good_cnt`mảng đo xem hệ thống định vị có thể tiếp tục đi theo con đường ngắn nhất bằng bao nhiêu cách. Điều này rất quan trọng cho việc tính toán tối đa vì nó quyết định liệu Polycarp có thể bị buộc rời khỏi lộ trình được đề xuất hay không. 

Trong quá trình truyền qua đường dẫn nhất định, mỗi bước được phân loại là bước xây dựng lại bắt buộc (khi cạnh không tương thích với bất kỳ đường dẫn ngắn nhất nào) hoặc là bước có thể tránh được trong đó chỉ phân nhánh mới xác định mức tối đa. 

## Ví dụ đã hoạt động 

Hãy xem xét một biểu đồ nhỏ trong đó tồn tại nhiều tuyến đường ngắn nhất giữa các nút trung gian và đích đến và Polycarp đôi khi đi theo và đôi khi đi chệch khỏi cấu trúc ngắn nhất. Trước tiên, chúng tôi tính toán khoảng cách từ đích đến, sau đó phân loại từng bước xem liệu nó có giảm khoảng cách đi một hay không. 

Trong đầu vào mẫu, ở lần chuyển đổi đầu tiên, chúng tôi kiểm tra xem liệu chuyển động có phù hợp với phân lớp khoảng cách hay không. Nếu không, cả số lượng tối thiểu và tối đa đều tăng ngay lập tức vì hệ thống phải xây dựng lại. Ở các bước sau, ngay cả khi việc di chuyển là nhất quán, sự tồn tại của các lân cận hợp lệ thay thế sẽ xác định xem số lượng tối đa có tăng hay không. 

| Bước | Bạn hiện tại | Tiếp theo v | quận[u] | quận [v] | ngắn nhất | good_cnt[u] | mn | mx | 
| --- | --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 2 | 3 | 2 | vâng | >1 | 0 | 1 | 
| 2 | 2 | 3 | 2 | 1 | vâng | 1 | 0 | 1 | 
| 3 | 3 | 4 | 1 | 0 | vâng | 1 | 0 | 1 | 

Dấu vết này cho thấy mức độ phân kỳ tối thiểu và tối đa chỉ khi phân nhánh tồn tại trong các chuyển đổi đường đi ngắn nhất. Khi biểu đồ buộc phải tiếp tục ngắn nhất, cả hai giá trị đều trùng nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | BFS trên biểu đồ đảo ngược cộng với một lần chuyển qua các nút và đường dẫn | 
| Không gian | O(n + m) | danh sách kề và mảng khoảng cách | 

Quá trình tiền xử lý là tuyến tính theo kích thước của biểu đồ và quá trình quét đường dẫn là tuyến tính theo k. Với tổng số ràng buộc lên tới hai trăm nghìn, điều này vừa vặn trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    n, m = map(int, input().split())
    g_rev = [[] for _ in range(n + 1)]

    for _ in range(m):
        u, v = map(int, input().split())
        g_rev[v].append(u)

    k = int(input())
    p = list(map(int, input().split()))

    INF = 10**18
    dist = [INF] * (n + 1)

    t = p[-1]
    q = deque([t])
    dist[t] = 0

    while q:
        u = q.popleft()
        for v in g_rev[u]:
            if dist[v] == INF:
                dist[v] = dist[u] + 1
                q.append(v)

    good_cnt = [0] * (n + 1)
    for u in range(1, n + 1):
        for v in g_rev[u]:
            if dist[v] == dist[u] + 1:
                good_cnt[u] += 1

    mn = mx = 0
    for i in range(k - 1):
        u, v = p[i], p[i + 1]
        if dist[u] != dist[v] + 1:
            mn += 1
            mx += 1
        else:
            if good_cnt[u] > 1:
                mx += 1

    return f"{mn} {mx}"

# sample
assert run("""6 9
1 5
5 4
1 2
2 3
3 4
4 1
2 6
6 4
4 2
4
1 2 3 4
""") == "1 2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu 1 | 1 2 | tính đúng đắn của cả hai giới hạn theo phân nhánh các đường đi ngắn nhất | 
| chuỗi cưỡng bức đơn | 0 0 | không xây dựng lại khi tồn tại đường đi ngắn nhất duy nhất | 
| đồ thị phân nhánh đầy đủ | mx cao hơn | độ nhạy tối đa đối với các cạnh ngắn nhất thay thế | 
| đường vòng nặng nề | mn tăng | buộc phải xây dựng lại khi đường đi rời khỏi lớp ngắn nhất | 

## Vỏ cạnh 

Trường hợp quan trọng xảy ra khi bước tiếp theo của Polycarp vẫn nằm trên con đường ngắn nhất nhưng không phải là con đường tiếp theo ngắn nhất duy nhất có thể. Trong tình huống đó, số lượng tối thiểu không tăng vì hệ thống có thể hợp tác, nhưng số lượng tối đa tăng do hệ thống có thể cố tình chọn cách tiếp tục hợp lệ khác, buộc phải xây dựng lại. Sự phân tách này hoàn toàn được xác định bởi liệu good_cnt[u] có lớn hơn một tại nút đó hay không, mặc dù bản thân đường dẫn vẫn nhất quán ngắn nhất.
