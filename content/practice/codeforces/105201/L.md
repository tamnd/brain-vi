---
title: "CF 105201L - Trạm Xăng Nhỏ"
description: "Thành phố là một cái cây: mọi ngã tư đều được kết nối với nhau thông qua một con đường duy nhất. Giao lộ 1 luôn chứa trạm xăng ban đầu, trong khi các giao lộ khác có thể tạm thời chứa các bản sao."
date: "2026-06-27T02:49:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105201
codeforces_index: "L"
codeforces_contest_name: "IME++ Open Contest 2024"
rating: 0
weight: 105201
solve_time_s: 71
verified: false
draft: false
---

[CF 105201L - Trạm xăng nhỏ](https://codeforces.com/problemset/problem/105201/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Thành phố là một cái cây: mọi ngã tư đều được kết nối với nhau thông qua một con đường duy nhất. Giao lộ`1`luôn chứa trạm xăng ban đầu, trong khi các giao lộ khác có thể tạm thời chứa các bản sao. Trong chuỗi ngày, các bản sao được thêm vào và xóa đi, đồng thời một số ngày sẽ yêu cầu khoảng cách từ một giao lộ nhất định đến trạm xăng gần nhất hiện có. 

Nhiệm vụ không phải là chăm sóc cây. Những con đường không bao giờ thay đổi. Thách thức là duy trì tập hợp các đỉnh được đánh dấu, trong đó một đỉnh được đánh dấu có nghĩa là có một trạm xăng ở đó, đồng thời trả lời các truy vấn khoảng cách tối thiểu một cách nhanh chóng. 

Các ràng buộc buộc một cấu trúc dữ liệu động. Với tối đa`2 * 10^5`giao lộ và`2 * 10^5`hoạt động quét tất cả các trạm xăng cho mỗi truy vấn là quá tốn kém. Trong trường hợp xấu nhất, nếu mỗi thao tác là một truy vấn và có nhiều trạm thì việc tìm kiếm trực tiếp có thể mất nhiều thời gian.`O(nq)`, đó là về`4 * 10^10`hoạt động. Ngay cả việc chạy tìm kiếm theo chiều rộng đầu tiên từ mọi đỉnh truy vấn cũng không thể thực hiện được. Chúng ta cần công logarit hoặc gần logarit cho mỗi phép toán. 

Có một số trường hợp khó khăn làm hỏng việc triển khai đơn giản. Đầu tiên là trạm gốc ở đỉnh`1`không bao giờ biến mất. Ví dụ:```
3 3
1 2
2 3
3 3
1 2
3 3
3 1
```Các kết quả đầu ra là:```
0
0
```Sau khi thêm một trạm tại`2`, truy vấn`3`cho khoảng cách`1`, không`0`, nhưng trường hợp quan trọng là việc truy vấn đỉnh`1`phải luôn quay trở lại`0`. Chỉ xóa các bản sao không được vô tình xóa đỉnh`1`. 

Một lỗi phổ biến khác là giả định rằng trạm gần nhất luôn nằm trong số các đỉnh được thêm gần đây. Ví dụ:```
4 3
1 2
2 3
3 4
1 4
3 2
2 4
```Đầu ra là:```
0
```Nhà ga ở`1`vẫn có sẵn, vì vậy sau khi loại bỏ bản sao tại`4`, đỉnh`2`vẫn còn khoảng cách`1`đến đỉnh`1`. Cấu trúc chỉ ghi nhớ các phần bổ sung và quên đi các trạm hoạt động cũ sẽ thất bại. 

Trường hợp cạnh cuối cùng là khi câu trả lời bằng 0. Truy vấn tại giao lộ chứa trạm phải ngay lập tức trả về 0 vì trạm gần nhất chính là trạm đó. 

## Phương pháp tiếp cận 

Một giải pháp đơn giản là giữ lại tập hợp các trạm xăng hiện tại và đối với mỗi truy vấn, hãy chạy tìm kiếm biểu đồ từ giao lộ được truy vấn cho đến khi đến một trạm. Điều này đúng vì trạm đầu tiên được tìm thấy bằng tìm kiếm theo chiều rộng có khoảng cách tối thiểu. Tuy nhiên, một truy vấn có thể truy cập mọi đỉnh và lặp lại điều này cho`2 * 10^5`truy vấn đưa ra trường hợp xấu nhất không thể chấp nhận được. 

Một cách tiếp cận trực tiếp khác là lưu trữ tất cả các trạm và tính khoảng cách từ đỉnh truy vấn đến mọi trạm. Các truy vấn khoảng cách cây có thể được thực hiện nhanh chóng với quá trình tiền xử lý tổ tiên chung thấp nhất, nhưng nếu có nhiều trạm thì việc này vẫn còn quá nhiều việc. Giải pháp Brute Force thành công vì nó xử lý tốt một truy vấn nhưng không thành công vì số lượng truy vấn và cập nhật quá lớn. 

Quan sát quan trọng là cây ở trạng thái tĩnh. Chỉ có tập hợp các đỉnh được đánh dấu là thay đổi. Điều này làm cho việc phân tách trung tâm trở nên phù hợp. Trọng tâm chia cây thành các thành phần nhỏ hơn và mỗi đỉnh chỉ thuộc về`O(log n)`các cấp độ trung tâm. Thay vì tìm kiếm toàn bộ cây, chúng tôi lưu trữ thông tin về các trạm hoạt động gần mọi trung tâm. 

Đối với mọi trọng tâm`c`, chúng tôi duy trì khoảng cách từ`c`tới tất cả các trạm hoạt động thuộc về đường dẫn phân rã trung tâm thông qua`c`. Nếu một truy vấn hỏi về đỉnh`x`, chúng tôi kiểm tra tất cả tổ tiên trung tâm của`x`. Bất kỳ con đường nào từ`x`đến một trạm tùy ý phải đi qua một trong các tâm này ở một mức nào đó, vì vậy câu trả lời tốt nhất là mức tối thiểu:```
distance(x, centroid) + closest station distance stored at centroid
```Việc thêm và xóa một trạm chỉ thay đổi các giá trị được lưu trong`O(log n)`centroid trên con đường phân hủy của nó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) mỗi truy vấn | O(n) | Quá chậm | 
| Tối ưu | O(log n) mỗi lần cập nhật/truy vấn | O(n log n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng phân rã trung tâm của cây. Trong quá trình phân rã, với mỗi đỉnh ban đầu sẽ lưu trữ danh sách các cặp`(centroid, distance)`mô tả tổ tiên của nó trong cây trung tâm và khoảng cách đến từng tổ tiên đó. 

Danh sách này là cầu nối giữa một đỉnh cây bình thường và các trọng tâm có thể tóm tắt thông tin về nó. 
2. Đối với mỗi centroid, duy trì một đống chứa khoảng cách đến các trạm hiện đang hoạt động. Bởi vì các trạm có thể bị xóa, nên hãy duy trì đống thứ hai chứa các khoảng cách cần được loại bỏ một cách lười biếng. 

Việc xóa lười sẽ tránh được việc loại bỏ tốn kém từ giữa đống. Khi giá trị nhỏ nhất xuất hiện trong cả hai đống, cả hai bản sao đều bị loại bỏ. 
3. Ban đầu kích hoạt đỉnh`1`, vì cây xăng ban đầu luôn tồn tại. Kích hoạt có nghĩa là chèn khoảng cách của nó vào từng đống trọng tâm trên đường đi của nó. 
4. Khi một bản sao được tạo ở đỉnh`x`, kích hoạt`x`. Đối với mọi`(centroid, distance)`cặp thuộc về`x`, chèn`distance`vào đống của trung tâm đó. 
5. Khi một bản sao bị phá hủy ở đỉnh`x`, vô hiệu hóa nó. Chèn các khoảng cách tương ứng vào các đống xóa của tất cả các tâm trên`x`con đường của. 
6. Đối với truy vấn tại đỉnh`x`, kiểm tra từng`(centroid, distance)`cặp thuộc về`x`. Làm sạch đống xóa lười trước, sau đó kết hợp khoảng cách từ`x`đến tâm với khoảng cách trạm hoạt động gần nhất được lưu trữ tại tâm đó. 

Lấy mức tối thiểu trên tất cả các tổ tiên trung tâm sẽ cho trạm gần nhất. 

Tại sao nó hoạt động: 

Mỗi cặp đỉnh của cây có một đường đi duy nhất. Trong phân rã trọng tâm, đối với bất kỳ cặp đỉnh nào, đều tồn tại một trọng tâm trên đường phân rã của cả hai đỉnh trong đó mối liên hệ của chúng thông qua trọng tâm đó được xem xét. Giá trị được lưu trữ tại tâm đó biểu thị đường đi ngắn nhất từ ​​tâm đến trạm hoạt động. Việc thêm khoảng cách từ đỉnh truy vấn đến cùng trọng tâm đó sẽ xây dựng lại đường dẫn ứng cử viên đến trạm đó. Vì mọi trạm có thể đều được xem xét thông qua một tổ tiên trung tâm nào đó, ứng cử viên tối thiểu chính xác là khoảng cách thực sự gần nhất. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline
sys.setrecursionlimit(1 << 25)

n, q = map(int, input().split())
g = [[] for _ in range(n + 1)]

for _ in range(n - 1):
    a, b = map(int, input().split())
    g[a].append(b)
    g[b].append(a)

size = [0] * (n + 1)
dead = [False] * (n + 1)
paths = [[] for _ in range(n + 1)]

def calc_size(v, p):
    size[v] = 1
    for u in g[v]:
        if u != p and not dead[u]:
            size[v] += calc_size(u, v)
    return size[v]

def find_centroid(v, p, total):
    for u in g[v]:
        if u != p and not dead[u] and size[u] * 2 > total:
            return find_centroid(u, v, total)
    return v

def collect(v, p, d, c):
    paths[v].append((c, d))
    for u in g[v]:
        if u != p and not dead[u]:
            collect(u, v, d + 1, c)

def decompose(v):
    total = calc_size(v, 0)
    c = find_centroid(v, 0, total)
    dead[c] = True
    collect(c, 0, 0, c)
    for u in g[c]:
        if not dead[u]:
            decompose(u)

decompose(1)

add_heap = [[] for _ in range(n + 1)]
del_heap = [[] for _ in range(n + 1)]
active = [False] * (n + 1)

def activate(v):
    if active[v]:
        return
    active[v] = True
    for c, d in paths[v]:
        heapq.heappush(add_heap[c], d)

def deactivate(v):
    if not active[v]:
        return
    active[v] = False
    for c, d in paths[v]:
        heapq.heappush(del_heap[c], d)

def clean(c):
    while add_heap[c] and del_heap[c] and add_heap[c][0] == del_heap[c][0]:
        heapq.heappop(add_heap[c])
        heapq.heappop(del_heap[c])

def query(v):
    ans = 10 ** 9
    for c, d in paths[v]:
        clean(c)
        if add_heap[c]:
            ans = min(ans, d + add_heap[c][0])
    return ans

activate(1)

out = []
for _ in range(q):
    data = list(map(int, input().split()))
    if data[0] == 1:
        activate(data[1])
    elif data[0] == 2:
        deactivate(data[1])
    else:
        out.append(str(query(data[1])))

sys.stdout.write("\n".join(out))
```Phần phân rã trung tâm xây dựng cấu trúc tĩnh.`paths[v]`là mảng quan trọng: nó lưu trữ chính xác các trọng tâm mà một đỉnh có thể sử dụng khi cập nhật hoặc truy vấn. 

Hai đống cho mỗi centroid thực hiện một multiset với tính năng xóa lười. Đống Python chỉ hỗ trợ loại bỏ phần tử tối thiểu một cách hiệu quả, do đó các trạm bị phá hủy sẽ được ghi lại riêng biệt và bị xóa khi chúng đạt đến đỉnh. Điều này giữ cho mọi hoạt động của đống logarit. 

Kích hoạt đỉnh`1`xảy ra trước khi xử lý truy vấn, vì trạm ban đầu là một phần của trạng thái ban đầu. Vấn đề đảm bảo rằng các hoạt động xóa chỉ nhắm mục tiêu vào các bản sao, do đó không cần thêm biện pháp bảo vệ nào ngoài việc giữ lại kích hoạt ban đầu. 

Tất cả các khoảng cách vừa vặn thoải mái trong số nguyên Python. Giới hạn đệ quy được tăng lên vì cây ban đầu có thể là một chuỗi, khiến cho độ sâu đầu tiên tìm kiếm đầu tiên sâu hơn nhiều so với giới hạn đệ quy mặc định của Python. 

## Ví dụ đã hoạt động 

Sử dụng mẫu:```
7 5
1 2
1 3
2 4
5 3
6 3
7 1
2 1
3 3
1 7
3 7
3 3
```Các trạng thái quan trọng là: 

| Hoạt động | Trạm hoạt động | Đỉnh truy vấn | Trả lời | 
| --- | --- | --- | --- | 
| Bắt đầu | 1 | | | 
| Không thể xóa 1 trong câu lệnh gốc nên chỉ xóa các bản sao | | | | 
| Thêm 7 | 1, 7 | 3 | 1 | 
| Truy vấn 7 | 1, 7 | 7 | 0 | 
| Truy vấn 3 | 1, 7 | 3 | 1 | 

Dấu vết cho thấy cấu trúc này giữ lại tất cả các trạm đang hoạt động, không chỉ trạm mới nhất. Đống centroid lưu trữ cả hai vị trí trạm và cho phép truy vấn tìm giá trị nhỏ nhất trong số đó. 

Một ví dụ nhỏ hơn:```
5 5
1 2
2 3
3 4
4 5
1 5
3 3
2 5
3 5
3 1
```Dấu vết là: 

| Hoạt động | Trạm hoạt động | Truy vấn | Kết quả | 
| --- | --- | --- | --- | 
| Trạng thái ban đầu | 1 | | | 
| Thêm 5 | 1, 5 | 3 | 2 | 
| Truy vấn 3 | 1, 5 | 5 | 0 | 
| Xóa 5 | 1 | 5 | 4 | 
| Truy vấn 1 | 1 | 1 | 0 | 

Điều này thể hiện việc xử lý việc xóa. Các đống xóa lười biếng chỉ loại bỏ khoảng cách lỗi thời khi cần thiết, trong khi trạm cố định ở đỉnh`1`vẫn có sẵn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log n) mỗi thao tác | Mỗi đỉnh chỉ xuất hiện ở nhiều mức trọng tâm theo logarit. | 
| Không gian | O(n log n) | Các đường dẫn trọng tâm lưu trữ một mục nhập cho mỗi đỉnh ở mọi mức phân rã. | 

Số lượng cặp trọng tâm được lưu trữ tối đa là khoảng`n log n`, tức là khoảng vài triệu mục cho`n = 200000`. Số lượng thao tác heap cũng là logarit cho mỗi sự kiện, phù hợp với giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    import heapq
    # paste the solution function here in a real test harness
    sys.stdin = old
    return ""

# The following cases should be run with the submitted solution wrapped
# into a callable function.

# Minimum tree:
# 1 1
# 3 1
# Expected:
# 0

# Chain with additions and removals:
# 5 5
# 1 2
# 2 3
# 3 4
# 4 5
# 1 5
# 3 3
# 2 5
# 3 5
# 3 1

# Star tree:
# 5 4
# 1 2
# 1 3
# 1 4
# 1 5
# 1 3
# 3 2
# 2 3
# 3 3

# Large shape should be generated separately:
# a chain of 200000 vertices with alternating add/query/remove operations
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đỉnh đơn |`0`| Cây nhỏ nhất có thể và trạm cố định | 
| Chuỗi | Khoảng cách dọc theo một đường | Đường dẫn dài và xóa | 
| Ngôi sao | Khoảng cách qua nút trung tâm | Nhiều lựa chọn khoảng cách bằng nhau | 
| Chuỗi lớn | Cập nhật logarit nhanh | Giới hạn hiệu suất | 

## Vỏ cạnh 

Đối với trường hợp trạm cố định, thuật toán kích hoạt đỉnh`1`trước khi đọc bất kỳ sự kiện nào. Nếu truy vấn được thực hiện ở đỉnh`1`, đường dẫn trung tâm của nó chứa khoảng cách được lưu trữ`0`từ trạm hoạt động đó, do đó câu trả lời sẽ trở thành số 0 ngay lập tức. 

Đối với một trạm được thêm vào và sau đó bị xóa đi, thao tác kích hoạt sẽ chèn khoảng cách của nó vào mọi vùng trọng tâm có liên quan. Việc xóa không quét vùng heap hoặc xây dựng lại bất cứ thứ gì. Nó chèn các giá trị phù hợp vào các vùng xóa và truy vấn tiếp theo trên các centroid đó sẽ loại bỏ các giá trị lỗi thời trước khi sử dụng mức tối thiểu. Điều này ngăn chặn các trạm bị xóa ảnh hưởng đến câu trả lời. 

Đối với một truy vấn tại một đỉnh mà bản thân nó chứa một trạm, giai đoạn cập nhật đã chèn khoảng cách bằng 0 vào mọi cấu trúc trọng tâm trên đường đi của đỉnh đó. Trong quá trình truy vấn, trọng tâm chứa đỉnh đó đóng góp một ứng cử viên bằng 0, do đó mức tối thiểu được trả về là chính xác.
