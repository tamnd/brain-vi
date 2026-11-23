---
title: "CF 59E - Đường đi ngắn nhất"
description: "Chúng tôi được yêu cầu điều hướng biểu đồ các thành phố được kết nối bằng đường hai chiều, với một điểm khác biệt: một số trình tự nhất định của ba thành phố liên tiếp bị cấm do mê tín. Chính thức, có n nút và m cạnh, tất cả đều không có trọng số, đại diện cho các thành phố và đường."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "graphs", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 59
codeforces_index: "E"
codeforces_contest_name: "Codeforces Beta Round 55 (Div. 2)"
rating: 2000
weight: 59
solve_time_s: 78
verified: true
draft: false
---

[CF 59E - Đường đi ngắn nhất](https://codeforces.com/problemset/problem/59/E) 

**Xếp hạng:** 2000 
**Tags:** đồ thị, đường đi ngắn nhất 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu điều hướng biểu đồ các thành phố được kết nối bằng đường hai chiều, với một điểm khác biệt: một số trình tự nhất định của ba thành phố liên tiếp bị cấm do mê tín. Về mặt hình thức, có _n_ nút và _m_ cạnh, tất cả đều không có trọng số, đại diện cho các thành phố và đường. Chúng tôi bắt đầu tại thành phố 1 và muốn đến thành phố _n_ dọc theo một con đường không bao giờ chứa bất kỳ bộ ba bị cấm _k_ nào dưới dạng các nút liên tiếp. Đầu ra phải là số lượng đường tối thiểu dọc theo một đường dẫn như vậy và một đường dẫn hợp lệ. Nếu không có đường dẫn nào tồn tại, chúng ta trả về -1. 

Với các ràng buộc, chúng tôi thấy rằng _n_ có thể lên tới 3000 và _m_ lên tới 20000. Điều này làm cho các thuật toán có đường biên phức tạp O(n^3), trong khi thuật toán O(n^2) có thể chấp nhận được. Số lượng bộ ba bị cấm _k_ có thể đạt tới 10^5, vì vậy việc kiểm tra đơn giản cho từng bộ ba trên mọi đường đi tiềm năng sẽ quá chậm. 

Các trường hợp cạnh không rõ ràng phát sinh khi các bộ ba bị cấm ngăn chặn mọi đường đi trực tiếp đến mục tiêu hoặc khi thành phố xuất phát chỉ có một hàng xóm, là một phần của chuỗi bộ ba bị cấm. Ví dụ: nếu n = 4, m = 3, các đường nối 1-2, 2-3, 3-4 và bộ ba bị cấm là 1-2-3, BFS ngây thơ sẽ xác nhận không chính xác rằng có một đường dẫn tồn tại nếu nó bỏ qua bộ ba. Một tình huống phức tạp khác là khi một đường dẫn tồn tại ở nhiều dạng ngắn và thuật toán phải chọn đường dẫn tối thiểu để tránh tất cả các bộ ba bị cấm. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ thử BFS tiêu chuẩn trên biểu đồ trong khi ghi nhớ toàn bộ đường đi đã đi qua cho đến nay. Mỗi lần chúng tôi mở rộng đường đi đến một thành phố mới, chúng tôi sẽ kiểm tra xem ba thành phố cuối cùng có tạo thành bộ ba bị cấm hay không. Về nguyên tắc, điều này hoạt động, nhưng số lượng đường dẫn có thể tăng theo cấp số nhân với _n_ và _m_, đồng thời việc lưu trữ đường dẫn rõ ràng cực kỳ chậm. Trong trường hợp xấu nhất, cách tiếp cận đơn giản này có độ phức tạp O(m * 2^n), rõ ràng là không khả thi với n = 3000. 

Quan sát quan trọng là các bộ ba bị cấm chỉ hạn chế chuỗi ba nút liên tiếp. Điều này có nghĩa là "ký ức" chúng ta cần chỉ là hai thành phố cuối cùng đã ghé thăm chứ không phải toàn bộ con đường. Nếu chúng tôi xác định lại trạng thái BFS của mình để bao gồm thành phố hiện tại và thành phố trước đó, chúng tôi có thể đảm bảo rằng mọi thành phố mới được truy cập không hoàn thành bộ ba bị cấm. Với thông tin chi tiết này, chúng tôi có thể thực hiện BFS trong không gian trạng thái có kích thước n^2 (thành phố trước × thành phố hiện tại), có thể quản lý được với các ràng buộc. Các bộ ba bị cấm có thể được lưu trữ trong ánh xạ từ điển (trước, hiện tại) → set(các thành phố bị cấm tiếp theo) để cho phép tra cứu O(1) khi mở rộng đường dẫn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(m * 2^n) | O(n * 2^n) | Quá chậm | 
| BFS có trạng thái (trước đây, hiện tại) | O(n * m) | O(n^2 + k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích dữ liệu đầu vào để xây dựng danh sách kề cho đồ thị vô hướng. Đối với mỗi thành phố, chúng tôi lưu trữ tất cả các thành phố lân cận được kết nối trực tiếp. 
2. Phân tích các bộ ba bị cấm thành cặp ánh xạ từ điển`(prev_city, current_city)`đến một bộ`next_city`các giá trị. Điều này cho phép kiểm tra O(1) khi mở rộng đường đi để xem liệu việc thêm thành phố mới có hoàn thành bộ ba bị cấm hay không. 
3. Khởi tạo hàng đợi BFS. Mỗi phần tử của hàng đợi là một bộ`(prev_city, current_city)`. Chúng ta có thể bắt đầu với`(0, 1)`, trong đó 0 đại diện cho một thành phố giả trước khi bắt đầu. Chúng tôi cũng duy trì một`distance`từ điển được khóa bởi`(prev_city, current_city)`cho độ dài đường đi ngắn nhất và`parent`từ điển để xây dựng lại đường dẫn. 
4. Khi hàng đợi không trống, hãy bật trạng thái`(prev_city, current_city)`và lặp qua tất cả hàng xóm`next_city`. Nếu như`next_city`bị cấm sau`(prev_city, current_city)`, bỏ qua nó. Nếu như`(current_city, next_city)`chưa được truy cập, hãy cập nhật`distance`Và`parent`, sau đó xếp hàng`(current_city, next_city)`. 
5. Nếu một trạng thái`(prev_city, n)`đạt được, hãy xây dựng lại đường đi bằng cách quay lại`parent`từ điển, bắt đầu từ`(prev_city, n)`. Thu thập các thành phố theo chiều ngược lại rồi đảo ngược danh sách để tạo ra thứ tự đúng. 
6. Nếu BFS hoàn thành mà không đến được thành phố`n`, đầu ra -1. Nếu không, hãy xuất khoảng cách và đường dẫn được xây dựng lại. 

Lý do điều này có hiệu quả là vì BFS đảm bảo lần đầu tiên chúng ta đạt đến trạng thái`(prev_city, n)`là đi qua con đường ngắn nhất tôn trọng bộ ba bị cấm. Bằng cách chỉ duy trì hai thành phố cuối cùng trong tiểu bang, chúng tôi đảm bảo rằng chúng tôi áp dụng chính xác tất cả các ràng buộc mà không sử dụng bộ nhớ không cần thiết. 

## Giải pháp Python```python
import sys
from collections import deque, defaultdict
input = sys.stdin.readline

n, m, k = map(int, input().split())
graph = [[] for _ in range(n + 1)]
for _ in range(m):
    u, v = map(int, input().split())
    graph[u].append(v)
    graph[v].append(u)

forbidden = defaultdict(set)
for _ in range(k):
    a, b, c = map(int, input().split())
    forbidden[(a, b)].add(c)

queue = deque()
queue.append((0, 1))
distance = dict()
distance[(0, 1)] = 0
parent = dict()

found = False
end_state = None

while queue:
    prev, curr = queue.popleft()
    if curr == n:
        found = True
        end_state = (prev, curr)
        break
    for nxt in graph[curr]:
        if nxt in forbidden.get((prev, curr), set()):
            continue
        if (curr, nxt) not in distance:
            distance[(curr, nxt)] = distance[(prev, curr)] + 1
            parent[(curr, nxt)] = (prev, curr)
            queue.append((curr, nxt))

if not found:
    print(-1)
else:
    path = []
    state = end_state
    while state != (0, 1):
        _, curr = state
        path.append(curr)
        state = parent[state]
    path.append(1)
    path.reverse()
    print(len(path) - 1)
    print(' '.join(map(str, path)))
```Giải pháp này trước tiên xây dựng biểu đồ và bản đồ bị cấm. BFS sau đó khám phá biểu đồ trong khi theo dõi hai thành phố cuối cùng. BFS đảm bảo việc phát hiện đường dẫn ngắn nhất và từ điển gốc cho phép chúng ta xây dựng lại đường dẫn một cách hiệu quả. sử dụng`(0, 1)`vì trạng thái giả ban đầu đảm bảo xử lý thống nhất các bộ ba bị cấm khi bắt đầu. 

## Ví dụ đã hoạt động 

Đầu vào mẫu 1:```
4 4 1
1 2
2 3
3 4
1 3
1 4 3
```| Xếp hàng | Trạng thái hiện tại (trước đây, hiện tại) | Hàng xóm | Các bang mới được xếp hàng | Khoảng cách | 
| --- | --- | --- | --- | --- | 
| [(0,1)] | (0,1) | 2,3 | (1,2), (1,3) | 1 | 
| [(1,2),(1,3)] | (1,2) | 1,3 | (2,3) | 2 | 
| [(1,3),(2,3)] | (1,3) | 1,4 | (3,4) | 2 | 

Chúng ta đến (3,4) là thành phố 4. Quay lui cho đường 1 → 3 → 4 với khoảng cách 2, tránh bộ ba bị cấm (1,4,3). 

Đầu vào tùy chỉnh:```
3 2 1
1 2
2 3
1 2 3
```Lượt truy cập BFS (0,1) → (1,2) → không thể lên 3 do bị cấm gấp ba lần. Không có con đường nào tồn tại. Đầu ra -1. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n*m) | Mỗi trạng thái BFS là (trước, hiện tại) và mỗi cạnh được kiểm tra một lần cho mỗi trạng thái. | 
| Không gian | O(n^2 + k) | O(n^2) cho khoảng cách/gốc, O(k) cho bộ ba bị cấm. | 

Cho n ≤ 3000 và m ≤ 20000, các phép toán n*m ≈ 6×10^7, khả thi trong vòng 3 giây. Bộ nhớ trong vòng 256 MB. 

## Trường hợp thử nghiệm```python
def run(inp: str) -> str:
    import sys, io
    sys.stdin = io.StringIO(inp)
    from collections import deque, defaultdict
    n, m, k = map(int, input().split())
    graph = [[] for _ in range(n + 1)]
    for _ in range(m):
        u, v = map(int, input().split())
        graph[u].append(v)
        graph[v].append(u)
    forbidden = defaultdict(set)
    for _ in range(k):
        a, b, c = map(int, input().split())
        forbidden[(a, b)].add(c)
    queue = deque()
    queue.append((0, 1))
    distance = dict()
    distance[(0, 1)] = 0
    parent = dict()
    found = False
    end_state = None
    while queue:
        prev, curr = queue.popleft()
        if curr == n:
            found = True
            end_state = (prev, curr)
            break
        for nxt in
```
