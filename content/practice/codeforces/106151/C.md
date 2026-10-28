---
title: "CF 106151C - biệt thự"
description: "Chúng ta được cho một đồ thị có hướng trong đó mỗi đỉnh đại diện cho một căn phòng trong một biệt thự và mỗi cạnh có hướng đại diện cho một cánh cửa một chiều giữa các phòng. Mỗi cánh cửa có giá 0 hoặc 1 tùy thuộc vào việc nó đã được mở khóa hay yêu cầu sử dụng huy hiệu để đi qua."
date: "2026-06-19T19:23:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106151
codeforces_index: "C"
codeforces_contest_name: "2025 ICPC Greek Collegiate Programming Contest (GRCPC 2025)"
rating: 0
weight: 106151
solve_time_s: 58
verified: true
draft: false
---

[CF 106151C - biệt thự](https://codeforces.com/problemset/problem/106151/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị có hướng trong đó mỗi đỉnh đại diện cho một căn phòng trong một biệt thự và mỗi cạnh có hướng đại diện cho một cánh cửa một chiều giữa các phòng. Mỗi cánh cửa có giá 0 hoặc 1 tùy thuộc vào việc nó đã được mở khóa hay yêu cầu sử dụng huy hiệu để đi qua. Sử dụng huy hiệu một cách hiệu quả có nghĩa là chúng ta có thể đi qua một cánh cửa bị khóa, nhưng nó luôn cộng thêm chi phí bằng 1 vào tổng thời gian di chuyển. 

Nhiệm vụ là tìm tổng chi phí tối thiểu để đi từ phòng 1 đến phòng N hoặc xác định rằng điều đó là không thể nếu không có đường đi có hướng. 

Các ràng buộc cho phép lên tới 100.000 phòng và 100.000 cửa, điều này ngay lập tức loại trừ mọi giải pháp cố gắng tính toán lại các đường đi ngắn nhất từ ​​đầu cho mỗi cạnh hoặc sử dụng phương pháp thư giãn O(NM) ngây thơ. Chúng ta cần một cái gì đó gần với thời gian tuyến tính hoặc tuyến tính. 

Một trường hợp lỗi phổ biến xuất hiện khi coi biểu đồ là không có trọng số. Ví dụ: nếu chúng tôi chạy BFS tiêu chuẩn bỏ qua các trọng số, chúng tôi giả định không chính xác rằng mọi cạnh đều có chi phí bằng nhau. 

Xét một trường hợp nhỏ: 

đầu vào: 

1 2 1 

1 3 0 

3 2 0 

Đường dẫn tối ưu từ 1 đến 2 là 1 → 3 → 2 với chi phí 0, trong khi cạnh trực tiếp 1 → 2 có chi phí 1. Một BFS đơn giản có thể ưu tiên không chính xác cạnh trực tiếp tùy thuộc vào thứ tự kề và bỏ lỡ đường dẫn rẻ hơn. 

Một vấn đề tế nhị khác đến từ việc sử dụng Dijkstra với vùng nhị phân nhưng coi trọng số là số nguyên tổng quát. Mặc dù đúng nhưng nó hơi quá mức cần thiết vì trọng số chỉ là 0 hoặc 1 và chúng tôi có thể làm tốt hơn. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là sử dụng thuật toán đường đi ngắn nhất tiêu chuẩn như Dijkstra. Mỗi khi chúng tôi nới lỏng một cạnh, chúng tôi sẽ đẩy khoảng cách đã cập nhật vào hàng đợi ưu tiên. Điều này đúng vì tất cả các trọng số của cạnh đều không âm, do đó Dijkstra đảm bảo tính tối ưu. 

Tuy nhiên, độ phức tạp trở thành O(M log N), có thể chấp nhận được ở đây, nhưng nó bỏ qua cấu trúc của bài toán: các trọng số chỉ là 0 hoặc 1. Cấu trúc đó cho phép chúng ta tránh hoàn toàn hệ số logarit. 

Quan sát quan trọng là mỗi lần nới lỏng chỉ tăng khoảng cách tối đa là 1. Điều này có nghĩa là chúng ta không cần thứ tự hàng đợi ưu tiên đầy đủ; chúng ta chỉ cần phân biệt giữa “cùng chi phí” và “chi phí + 1”. Điều đó đương nhiên dẫn đến một biến thể BFS dựa trên deque trong đó các cạnh của chi phí 0 đi về phía trước và các cạnh của chi phí 1 đi về phía sau. 

Điều này biến vấn đề thành đường đi ngắn nhất trong biểu đồ có trọng số 0-1, có thể giải được trong O(N + M) bằng cách sử dụng BFS 0-1. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Dijkstra với đống | O(M log N) | O(N + M) | Đã chấp nhận | 
| 0-1 BFS | O(N + M) | O(N + M) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý biểu đồ bằng cách sử dụng hàng đợi hai đầu, duy trì tính bất biến rằng các nút ở gần phía trước hơn luôn có khoảng cách hiện tại nhỏ hơn. 

1. Xây dựng danh sách kề trong đó mỗi cạnh lưu trữ đích và trọng số của nó. 
2. Khởi tạo một mảng khoảng cách có vô cực cho tất cả các nút ngoại trừ nút 1, được đặt thành 0. Điều này thể hiện rằng chúng ta bắt đầu ở phòng 1 mà không phát sinh chi phí. 
3. Đẩy nút 1 vào deque. Deque sẽ hoạt động giống như một cấu trúc ưu tiên trong đó các trạng thái rẻ hơn được mở rộng trước mà không cần sử dụng heap. 
4. Trong khi deque không trống, hãy bật một nút từ phía trước. Điều này đảm bảo chúng tôi luôn mở rộng trạng thái rẻ nhất hiện được biết đến. 
5. Đối với mỗi cạnh đi từ nút hiện tại, hãy tính khoảng cách dự kiến ​​là khoảng cách hiện tại cộng với trọng số của cạnh. Nếu giá trị này nhỏ hơn khoảng cách được lưu trữ cho hàng xóm, hãy cập nhật nó. 
6. Nếu trọng số của cạnh là 0, hãy đẩy hàng xóm lên phía trước deque. Nếu trọng lượng cạnh là 1, hãy đẩy nó về phía sau. Thứ tự này đảm bảo rằng tất cả các chuyển đổi chi phí bằng 0 đều được xử lý trước bất kỳ chuyển đổi chi phí 1 nào ở cùng mức độ khám phá. 
7. Tiếp tục cho đến khi hết deque. 
8. Câu trả lời là khoảng cách đến nút N hoặc -1 nếu nó vẫn là vô hạn. 

### Tại sao nó hoạt động

Thuật toán duy trì thuộc tính là các nút được xử lý theo thứ tự khoảng cách không giảm. Bất cứ khi nào chúng tôi nới lỏng một cạnh có trọng số 0, chúng tôi sẽ không tăng chi phí, do đó trạng thái kết quả phải được xử lý càng sớm càng tốt, do đó nó được đẩy lên phía trước. Bất kỳ cạnh nào của trọng số 1 đều làm tăng chi phí, vì vậy nó phải chờ sau tất cả các mở rộng chi phí bằng 0 hiện tại. 

Bởi vì tất cả các trọng số của cạnh chỉ là 0 hoặc 1, bất kỳ đường đi ngắn nhất nào cũng có thể được phân tách thành một chuỗi trong đó các chuyển đổi được sắp xếp chính xác theo kỷ luật deque này. Điều này đảm bảo rằng khi một nút được hoàn thiện lần đầu tiên (bật lên), khoảng cách của nó đã ở mức tối thiểu và sẽ không bao giờ được cải thiện sau này. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

n, m = map(int, input().split())
g = [[] for _ in range(n + 1)]

for _ in range(m):
    u, v, t = map(int, input().split())
    g[u].append((v, t))

INF = 10**18
dist = [INF] * (n + 1)
dist[1] = 0

dq = deque([1])

while dq:
    u = dq.popleft()
    for v, w in g[u]:
        nd = dist[u] + w
        if nd < dist[v]:
            dist[v] = nd
            if w == 0:
                dq.appendleft(v)
            else:
                dq.append(v)

print(-1 if dist[n] == INF else dist[n])
```Danh sách kề lưu trữ các cạnh có hướng chính xác như đã cho, duy trì tính định hướng. Mảng khoảng cách được khởi tạo đủ lớn để tránh sự cố tràn khi thêm 0 hoặc 1 liên tục. 

Các thao tác deque là chi tiết triển khai quan trọng: phần bổ sung chỉ được sử dụng cho các cạnh có trọng số bằng 0 để đảm bảo lan truyền ngay lập tức, trong khi phần bổ sung được sử dụng cho các cạnh có trọng lượng-1 để trì hoãn quá trình xử lý thích hợp. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5 6
1 2 0
2 3 1
3 5 0
1 4 1
4 5 1
2 5 1
```Chúng tôi theo dõi khoảng cách và trạng thái deque: 

| Bước | Nút xuất hiện | Khoảng cách | Cập nhật | 
| --- | --- | --- | --- | 
| 1 | 1 | 0 | dist[2]=0, dist[4]=1 | 
| 2 | 2 | 0 | quận[3]=1, quận[5]=1 | 
| 3 | 4 | 1 | không cải thiện | 
| 4 | 3 | 1 | dist[5]=1 (đã bằng) | 
| 5 | 5 | 1 | kết thúc | 

Đường đi tốt nhất là 1 → 2 → 3 → 5 với chi phí là 1. 

Dấu vết này cho thấy các cạnh có chi phí 0 được ưu tiên như thế nào, cho phép xử lý nút 2 trước nút 4 mặc dù cả hai đều được phát hiện sớm. 

### Mẫu 2 

đầu vào:```
3 2
1 2 1
2 1 1
```| Bước | Nút xuất hiện | Khoảng cách | Cập nhật | 
| --- | --- | --- | --- | 
| 1 | 1 | 0 | dist[2]=1 | 
| 2 | 2 | 1 | không cập nhật lên 3 | 

Nút 3 vẫn không thể truy cập được nên đầu ra là -1. 

Điều này thể hiện việc xử lý chính xác các thành phần bị ngắt kết nối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N + M) | Mỗi nút được đẩy vào deque nhiều nhất một lần cho mỗi lần thư giãn thành công và mỗi cạnh được xử lý một lần | 
| Không gian | O(N + M) | Danh sách kề cộng với mảng khoảng cách và deque | 

Độ phức tạp phù hợp thoải mái trong giới hạn cho N, M lên tới 100.000. Hành vi tuyến tính tránh mọi chi phí logarit vẫn có thể chấp nhận được nhưng không cần thiết. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from collections import deque

    n, m = map(int, sys.stdin.readline().split())
    g = [[] for _ in range(n + 1)]
    for _ in range(m):
        u, v, t = map(int, sys.stdin.readline().split())
        g[u].append((v, t))

    INF = 10**18
    dist = [INF] * (n + 1)
    dist[1] = 0

    dq = deque([1])
    while dq:
        u = dq.popleft()
        for v, w in g[u]:
            nd = dist[u] + w
            if nd < dist[v]:
                dist[v] = nd
                if w == 0:
                    dq.appendleft(v)
                else:
                    dq.append(v)

    return str(-1 if dist[n] == INF else dist[n])

# provided samples
assert run("""5 6
1 2 0
2 3 1
3 5 0
1 4 1
4 5 1
2 5 1
""") == "1"

assert run("""3 2
1 2 1
2 1 1
""") == "-1"

# custom cases
assert run("""2 1
1 2 0
""") == "0", "minimum case"

assert run("""2 1
1 2 1
""") == "1", "single edge cost 1"

assert run("""4 3
1 2 1
2 3 1
3 4 1
""") == "3", "all ones chain"

assert run("""4 4
1 2 0
2 3 0
3 4 0
1 4 1
""") == "0", "zero path dominates direct 1-edge"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1→2 đơn 0 | 0 | cạnh cơ bản 0 chi phí | 
| 1→2 đơn 1 | 1 | cạnh khóa đơn | 
| chuỗi những cái | 3 | tích lũy chi phí | 
| đường dẫn hoàn toàn bằng 0 so với đường dẫn trực tiếp | 0 | ưu tiên đường đi ngắn nhất | 

## Vỏ cạnh 

Trường hợp một cạnh là khi tồn tại nhiều đường dẫn với chi phí hỗn hợp và BFS ngây thơ sẽ khóa không chính xác vào một tuyến đường ban đầu tệ hơn. Ví dụ:```
4 4
1 2 1
2 4 1
1 3 0
3 4 0
```Đường dẫn đúng là 1 → 3 → 4 với chi phí 0. Đầu tiên, thuật toán đẩy nút 3 lên phía trước do trọng số cạnh 0, do đó, nó lan truyền hoàn toàn qua chuỗi chi phí bằng 0 trước khi mở rộng các đường dẫn có chi phí cao hơn. Nút 4 đạt được với khoảng cách 0 trước khi tuyến đường 1 chi phí từ nút 2 được hoàn thành. 

Một trường hợp khác là ngắt kết nối hoàn toàn. Nếu nút N không bao giờ đạt tới, khoảng cách của nó vẫn là vô cùng và thuật toán xuất ra chính xác -1 mà không yêu cầu kiểm tra khả năng tiếp cận rõ ràng ngoài so sánh cuối cùng.
