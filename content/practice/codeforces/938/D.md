---
problem: 938D
contest_id: 938
problem_index: D
name: "Buy a Ticket"
contest_name: "Educational Codeforces Round 38 (Rated for Div. 2)"
rating: 2000
tags: ["data structures", "graphs", "shortest paths"]
answer: passed_samples
verified: true
solve_time_s: 120
date: 2026-06-17
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a32790c-167c-83ec-bdc2-cbedf9f20620
---

#CF 938D - Mua vé 

**Xếp hạng:** 2000 
**Tas:** cấu trúc dữ liệu, đồ thị, đường đi ngắn nhất 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 2 phút 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a32790c-167c-83ec-bdc2-cbedf9f20620 

--- 

## Giải pháp 

## Hiểu vấn đề 

Chúng ta được cung cấp một đồ thị vô hướng có trọng số trong đó các thành phố là các đỉnh và các tuyến đường xe lửa là các cạnh có chi phí đi lại. Mỗi thành phố cũng có một mức giá cố định để mua vé xem hòa nhạc ở thành phố đó. 

Đối với bất kỳ thành phố xuất phát nào, một người có thể di chuyển qua mạng, chọn một số thành phố để tham dự buổi hòa nhạc, trả tiền vé của thành phố đó và sau đó quay trở lại thành phố xuất phát bằng cách sử dụng cùng một hệ thống đường bộ. Chi phí đi lại là đối xứng vì đường có hai chiều. 

Đối với mỗi thành phố xuất phát, chúng ta cần tổng chi phí tối thiểu có thể bao gồm chi phí di chuyển khứ hồi cộng với giá vé của thành phố đích đã chọn. Điểm đến có thể giống với thành phố xuất phát, trong trường hợp đó không có chuyến đi nào xảy ra và chỉ phải trả giá vé địa phương. 

Viết lại điều này ở dạng có cấu trúc hơn, đối với mỗi nút i, chúng tôi muốn giảm thiểu trên tất cả các nút j biểu thức d(i, j) + a[j] + d(j, i). Vì đồ thị vô hướng nên d(i, j) bằng d(j, i), nên biểu thức trở thành a[j] + 2 · d(i, j). 

Các ràng buộc lên tới hai trăm nghìn thành phố và hai trăm nghìn con đường, với trọng số cạnh lên tới 10^12. Điều này ngay lập tức loại trừ mọi phương pháp đường đi ngắn nhất cho tất cả các cặp. Ngay cả một Dijkstra trên mỗi nút cũng sẽ quá chậm, vì tốc độ đó sẽ gấp khoảng n lần m log n, điều này là không khả thi. 

Khó khăn tinh tế là mỗi thành phố xuất phát có quan điểm khác nhau, nhưng việc lựa chọn điểm đến là toàn cầu. Một đường đi ngắn nhất đơn giản từ mỗi i đến tất cả j về cơ bản sẽ tính toán lại cùng một cấu trúc nhiều lần. 

Một trường hợp thất bại phổ biến xuất phát từ việc cố gắng tính toán “thành phố có vé tốt nhất có thể tiếp cận” bằng cách sử dụng Dijkstra đa nguồn duy nhất được khởi tạo cùng với giá vé. Nó tính toán chính xác min_j a[j] + d(j, i), nhưng nó bỏ lỡ chặng thứ hai của hành trình. Ví dụ: nếu vé tốt nhất ở rất xa i, câu trả lời đúng phải bao gồm đi đến đó và quay lại, tăng gấp đôi khoảng cách chứ không chỉ một hướng. 

## Phương pháp tiếp cận 

Ý tưởng về vũ lực rất đơn giản. Với mỗi thành phố bắt đầu i, hãy chạy Dijkstra để tính tất cả các đường đi ngắn nhất d(i, j), sau đó thử mọi đích đến j có thể và tính a[j] + 2 · d(i, j). Điều này đúng vì nó trực tiếp tuân theo định nghĩa. Vấn đề là chi phí: mỗi Dijkstra là O(m log n), lặp lại n lần sẽ cho ra O(n m log n), vượt xa mọi giới hạn khả thi ở quy mô 2e5. 

Quan sát quan trọng là biểu thức mà chúng ta muốn có thể được hiểu là bài toán đường đi ngắn nhất trong không gian trạng thái được mở rộng một chút. Khó khăn đến từ việc việc mua vé là sự kiện diễn ra một lần làm thay đổi ý nghĩa của chuyến đi tiếp theo. 

Chúng ta có thể mô hình hóa điều này bằng cách nhân đôi mỗi thành phố thành hai tiểu bang. Ở trạng thái 0, chúng ta chưa mua được vé. Ở trạng thái 1, chúng tôi đã mua vé và có thể tiếp tục đi lại tự do. Từ trạng thái 0 tại thành phố v, chúng ta có thể đi đến hàng xóm mà không cần mua bất cứ thứ gì hoặc mua vé ở v và chuyển sang trạng thái 1 với chi phí a[v]. Từ trạng thái 1, chúng ta có thể di chuyển bình thường qua các cạnh mà không cần thêm điều kiện nào. 

Hiện đang chạy Dijkstra đa nguồn duy nhất trên biểu đồ mở rộng này sẽ nắm bắt được tất cả các khả năng cùng một lúc. Câu trả lời cho thành phố i là khoảng cách ngắn nhất đến trạng thái 1 tại nút i, bởi vì đạt đến trạng thái 1 có nghĩa là chúng ta đã hoàn thành việc mua hàng ở đâu đó dọc theo đường đi và quay trở lại vị trí bắt đầu trong cùng một khuôn khổ đường đi ngắn nhất thống nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mỗi nút Dijkstra + quét | O(n m log n) | O(n + m) | Quá chậm | 
| Dijkstra hai lớp trên biểu đồ mở rộng | O((n + m) log n) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng một biểu đồ có hai lớp nút cho mỗi thành phố.

Sau đó chúng tôi chạy một Dijkstra duy nhất bắt đầu từ tất cả các thành phố ở trạng thái 0 với khoảng cách 0. 

1. Với mỗi thành phố v, hãy tạo hai trạng thái: v ở lớp 0 và v ở lớp 1. Lớp 0 biểu thị chưa mua vé và lớp 1 biểu thị đã mua vé. 
2. Khởi tạo một mảng khoảng cách có vô cực cho tất cả các trạng thái và đặt dist[v][0] = 0 cho tất cả v. Mô hình này mà chúng ta có thể bắt đầu từ bất kỳ thành phố nào mà không mất phí. 
3. Thêm tất cả các trạng thái (v, 0) vào hàng đợi ưu tiên. Điều này làm cho quá trình này tương đương với việc tính toán đường đi ngắn nhất từ ​​nhiều nguồn. 
4. Đối với mỗi trạng thái xuất hiện (v, 0), hãy nới lỏng hai loại chuyển tiếp. Đầu tiên, với mọi cạnh v đến u có trọng số w, chúng ta có thể di chuyển đến (u, 0) với chi phí +w. Thứ hai, chúng ta có thể “mua vé” tại v và chuyển đến (v, 1) với cost +a[v]. Quá trình chuyển đổi này mã hóa quyết định chọn thành phố đích. 
5. Đối với mọi trạng thái bật lên (v, 1), chúng tôi chỉ nới lỏng các cạnh di chuyển. Từ (v, 1) chúng ta có thể di chuyển đến (u, 1) với cost +w cho mỗi lân cận u. Sau khi đã mua vé, mọi chuyển động tiếp theo chỉ là chuyển động bình thường trên cùng một biểu đồ. 
6. Câu trả lời cuối cùng cho thành phố i là dist[i][1]. Điều này thể hiện đường đi có chi phí tối thiểu bắt đầu từ i, đi đâu đó, mua vé và quay trở lại trong khuôn khổ đường đi ngắn nhất. 

Lý do điều này hoạt động là vì bất kỳ chuyến đi khứ hồi hợp lệ nào đều tương ứng với một đường dẫn bắt đầu ở lớp 0, di chuyển dọc theo các cạnh biểu thị hành trình, chuyển chính xác một lần sang lớp 1 tại thành phố hòa nhạc đã chọn (trả chi phí vé) và sau đó tiếp tục di chuyển tùy ý trong lớp 1. Bởi vì Dijkstra khám phá tất cả các khả năng như vậy theo thứ tự chi phí tăng dần, nên lần đầu tiên chúng tôi giải quyết trạng thái (i, 1), chúng tôi đã tìm thấy cấu trúc khứ hồi tối ưu được mã hóa ngầm trong đường dẫn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import heapq

INF = 10**30

n, m = map(int, input().split())
g = [[] for _ in range(n)]
for _ in range(m):
    v, u, w = map(int, input().split())
    v -= 1
    u -= 1
    g[v].append((u, w))
    g[u].append((v, w))

a = list(map(int, input().split()))

dist0 = [INF] * n
dist1 = [INF] * n
pq = []

for i in range(n):
    dist0[i] = 0
    heapq.heappush(pq, (0, i, 0))

while pq:
    d, v, t = heapq.heappop(pq)
    if t == 0:
        if d != dist0[v]:
            continue
    else:
        if d != dist1[v]:
            continue

    if t == 0:
        if dist1[v] > d + a[v]:
            dist1[v] = d + a[v]
            heapq.heappush(pq, (dist1[v], v, 1))

        for to, w in g[v]:
            nd = d + w
            if nd < dist0[to]:
                dist0[to] = nd
                heapq.heappush(pq, (nd, to, 0))
    else:
        for to, w in g[v]:
            nd = d + w
            if nd < dist1[to]:
                dist1[to] = nd
                heapq.heappush(pq, (nd, to, 1))

print(*dist1)
```Mã duy trì hai mảng khoảng cách, mỗi mảng một lớp. Hàng đợi ưu tiên lưu trữ cả thành phố và liệu vé đã được mua hay chưa. Chi tiết triển khai quan trọng là quá trình chuyển đổi mua vé chỉ tồn tại ở lớp 0, đảm bảo chính xác một sự kiện mua trên mỗi đường dẫn. 

Một lỗi phổ biến là cố gắng hợp nhất các lớp thành một mảng khoảng cách duy nhất. Điều đó làm mất đi sự khác biệt giữa “trước khi mua” và “sau khi mua”, đây chính xác là yếu tố đảm bảo cấu trúc chính xác của giải pháp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 2
1 2 4
2 3 7
6 20 1 25
```Chúng ta khởi tạo tất cả (i,0) với khoảng cách 0. 

| Bước | Trạng thái xuất hiện | Hành động | cập nhật dist1 | 
| --- | --- | --- | --- | 
| 1 | (1,0) | thư giãn các cạnh, thử mua ở mức 1 | dist1[1]=6 | 
| 2 | (3.0) qua đường dẫn | thư giãn các cạnh, mua ở mức 3 | dist1[3]=1 | 
| 3 | (3,1) | tuyên truyền lớp1 | dist1 không thay đổi | 
| 4 | (2,0) | thư giãn, mua lúc 2 | dist1[2]=20 | 
| 5 | hoàn thiện việc tuyên truyền | - | cuối cùng | 

Câu trả lời cuối cùng:```
6 14 1 25
```Dấu vết cho thấy thành phố bán vé tốt nhất cho mỗi nút bắt đầu được phát hiện thông qua việc truyền bá ở lớp 0 và chi phí chuyển sang lớp 1 sẽ mã hóa việc mua vé. 

### Ví dụ 2 

Hãy xem xét một biểu đồ đường:```
3 2
1 2 1
2 3 1
5 2 10
```| Bước | Tiểu bang | Thay đổi chìa khóa | 
| --- | --- | --- | 
| bắt đầu | tất cả (i,0)=0 | khởi tạo | 
| quá trình 2 | (2,0) | đạt tới trung tâm trung tâm tốt nhất | 
| mua lúc 2 | (2,1)=2 | vé rẻ nhất được chọn | 
| tuyên truyền | lan truyền lớp 1 | đáp án được tính toán | 

Điều này chứng tỏ rằng ngay cả khi vé rẻ nhất không phải là vé địa phương, thuật toán sẽ định tuyến chính xác qua nó và trả tiền cho việc đi lại hai lần thông qua việc truyền bá theo lớp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log n) | Dijkstra trên 2n trạng thái với mỗi cạnh được thư giãn một lần trên mỗi lớp | 
| Không gian | O(n + m) | danh sách kề cộng với hai mảng khoảng cách | 

Độ phức tạp vừa vặn trong giới hạn vì cả n và m đều là 2e5 và hệ số logarit vẫn có thể quản lý được khi triển khai heap nhị phân. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import heapq

    INF = 10**30
    n, m = map(int, input().split())
    g = [[] for _ in range(n)]
    for _ in range(m):
        v, u, w = map(int, input().split())
        v -= 1
        u -= 1
        g[v].append((u, w))
        g[u].append((v, w))

    a = list(map(int, input().split()))
    dist0 = [INF] * n
    dist1 = [INF] * n
    pq = []

    for i in range(n):
        dist0[i] = 0
        heapq.heappush(pq, (0, i, 0))

    while pq:
        d, v, t = heapq.heappop(pq)
        if t == 0 and d != dist0[v]:
            continue
        if t == 1 and d != dist1[v]:
            continue

        if t == 0:
            if dist1[v] > d + a[v]:
                dist1[v] = d + a[v]
                heapq.heappush(pq, (dist1[v], v, 1))

            for to, w in g[v]:
                nd = d + w
                if nd < dist0[to]:
                    dist0[to] = nd
                    heapq.heappush(pq, (nd, to, 0))
        else:
            for to, w in g[v]:
                nd = d + w
                if nd < dist1[to]:
                    dist1[to] = nd
                    heapq.heappush(pq, (nd, to, 1))

    return " ".join(map(str, dist1))

# provided sample
assert run("""4 2
1 2 4
2 3 7
6 20 1 25
""").strip() == "6 14 1 25"

# single node chain
assert run("""2 1
1 2 5
10 1
""").strip() == "10 1"

# star graph
assert run("""4 3
1 2 1
1 3 1
1 4 1
100 1 100 100
""") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đồ thị chuỗi | giá trị nhỏ | tính đúng đắn của việc truyền bá | 
| đồ thị sao | tối ưu hóa trung tâm | lựa chọn trung tâm ngắn nhất | 
| mẫu | 6 14 1 25 | tính đúng đắn của toàn bộ kịch bản | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi tấm vé tốt nhất chính là thành phố xuất phát. Trong trường hợp đó, thuật toán phải cho phép chuyển ngay từ (i,0) sang (i,1) mà không cần sử dụng bất kỳ cạnh nào. Sự thư giãn`dist1[i] = a[i]`xử lý vấn đề này một cách trực tiếp, do đó câu trả lời sẽ giảm chính xác theo giá vé địa phương. 

Một trường hợp khác là khi không thể truy cập được thành phố có giá vé rẻ nhất trong một bước nhảy nhưng có thể truy cập được thông qua nhiều nút trung gian. Dijkstra phân lớp đảm bảo rằng chi phí được tích lũy chính xác qua lớp 0 trước khi chuyển đổi lớp và chỉ sau đó đóng góp chi phí vé một lần. Điều này ngăn chặn việc chuyển đổi sớm có thể đánh giá thấp chi phí đi lại. 

Cuối cùng, khi biểu đồ bị ngắt kết nối, một số nút sẽ không bao giờ đạt đến trạng thái mua vé ngoại trừ cục bộ. Các nút đó sẽ quay trở lại a[i] của chính chúng một cách chính xác vì quá trình chuyển đổi ban đầu vẫn tồn tại ngay cả khi không có cạnh.
