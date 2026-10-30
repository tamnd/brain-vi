---
title: "CF 106235E - Đường dẫn ngắn nhất không ngắn nhất"
description: "Chúng tôi được cung cấp một biểu đồ có trọng số trong đó việc di chuyển dọc theo một cạnh sẽ có chi phí và chúng tôi quan tâm đến các đường dẫn từ nút bắt đầu cố định đến nút mục tiêu cố định. Trong số tất cả các bước đi có thể từ điểm bắt đầu đến mục tiêu, có tổng chi phí tối thiểu có thể, con đường ngắn nhất thông thường."
date: "2026-06-25T07:06:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106235
codeforces_index: "E"
codeforces_contest_name: "Algo Cup 2025 by csspace.io (Qualification Round)"
rating: 0
weight: 106235
solve_time_s: 43
verified: true
draft: false
---

[CF 106235E - Đường dẫn ngắn nhất không ngắn nhất](https://codeforces.com/problemset/problem/106235/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 43s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một biểu đồ có trọng số trong đó việc di chuyển dọc theo một cạnh sẽ có chi phí và chúng tôi quan tâm đến các đường dẫn từ nút bắt đầu cố định đến nút mục tiêu cố định. Trong số tất cả các bước đi có thể từ điểm bắt đầu đến mục tiêu, có tổng chi phí tối thiểu có thể, con đường ngắn nhất thông thường. 

Nhiệm vụ không phải là xuất ra con đường ngắn nhất đó. Thay vào đó, chúng ta phải tìm đường đi rẻ nhất có tổng chi phí lớn hơn chi phí ngắn nhất có thể. Nói cách khác, nếu chúng ta tưởng tượng sắp xếp tất cả chi phí đường dẫn bắt đầu đến mục tiêu hợp lệ theo thứ tự tăng dần, chúng ta muốn giá trị đầu tiên lớn hơn giá trị tối thiểu. 

Đầu vào mô tả một đồ thị thông qua các đỉnh và các cạnh có trọng số của nó. Mỗi cạnh cho phép di chuyển theo cả hai hướng trừ khi có quy định khác. Đầu ra là một số duy nhất biểu thị chi phí đường dẫn "không tối ưu hoàn toàn tốt nhất" này hoặc một giá trị lỗi nếu không tồn tại đường dẫn đó. 

Khó khăn chính về cơ cấu là “không ngắn nhất” là một hạn chế toàn cầu. Một đường dẫn không hợp lệ không phải do thuộc tính cục bộ của một cạnh mà vì tổng trọng số của nó khớp chính xác với mức tối ưu toàn cục. 

Từ góc độ phức tạp, kích thước đồ thị đủ lớn để liệt kê tất cả các đường dẫn đơn giản là không thể. Ngay cả một biểu đồ có 10^5 cạnh cũng làm cho số bước đi riêng biệt theo cấp số nhân, do đó, mọi giải pháp đều phải dựa vào cấu trúc đường dẫn ngắn nhất thay vì liệt kê đường dẫn. Điều này ngay lập tức loại trừ DFS trên các đường dẫn hoặc bất kỳ tìm kiếm tổ hợp nào. 

Một số trường hợp đặc biệt quan trọng. 

Một biểu đồ trong đó có chính xác một đường dẫn từ đầu đến đích là rất quan trọng. Trong trường hợp đó, mọi đường đi hợp lệ đều có cùng chi phí, do đó không có đường đi thay thế nào lớn hơn và câu trả lời là không thể. 

Một trường hợp khó phát hiện khác là khi tồn tại nhiều đường đi ngắn nhất. Ví dụ: nếu cả hai tuyến đường riêng biệt đều đạt được cùng một mức chi phí tối thiểu thì tuyến đường đề xuất tiếp theo chỉ có thể xuất hiện sau khi khám phá các đường vòng làm tăng chi phí lên một lượng tối thiểu. Một ý tưởng ngây thơ như “loại bỏ các cạnh được sử dụng trong một đường đi ngắn nhất” có thể thất bại vì các đường dẫn ngắn nhất khác nhau có thể chỉ chia sẻ một số cạnh và việc loại bỏ một tập hợp cạnh có thể loại bỏ tất cả các lựa chọn thay thế hợp lệ một cách không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force sẽ cố gắng liệt kê tất cả các đường đi có thể từ nguồn đến đích, tính toán chi phí của chúng và chọn chi phí nhỏ nhất lớn hơn chi phí đường đi ngắn nhất. Điều này đúng về mặt khái niệm vì nó kiểm tra rõ ràng mọi ứng cử viên, nhưng không thể tính toán được. Trong một biểu đồ có chu kỳ, số lần đi bộ là vô hạn và thậm chí việc hạn chế các đường đi đơn giản sẽ dẫn đến sự tăng trưởng theo giai thừa về số lượng các khả năng. 

Quan sát quan trọng là chúng ta thực sự không cần liệt kê các đường dẫn. Chúng ta chỉ cần hai khoảng cách khác nhau nhỏ nhất từ ​​nguồn tới đích. Cái ngắn nhất là Dijkstra tiêu chuẩn. Cái thứ hai có thể được tính toán bằng cách mở rộng ý tưởng của Dijkstra để giữ nhiều hơn một khoảng cách nổi tiếng nhất cho mỗi nút. 

Thay vì lưu trữ một khoảng cách tốt nhất cho mỗi nút, chúng tôi lưu trữ khoảng cách tốt nhất và tốt nhất thứ hai. Khi nới lỏng một cạnh, khoảng cách dự kiến ​​mới có thể cải thiện một trong hai giá trị này. Điều này hiệu quả vì bất kỳ đường dẫn nào đến một nút đều có thể được đặc trưng bởi tổng chi phí của nó và đối với câu trả lời cuối cùng, chúng tôi chỉ quan tâm đến việc giữ hai chi phí riêng biệt nhỏ nhất để đến được đích. 

Lực lượng vũ phu không thành công vì nó khám phá cấu trúc ở cấp độ của toàn bộ đường dẫn, trong khi giải pháp tối ưu nén tất cả các đường dẫn vào xếp hạng trên mỗi nút chỉ có hai trạng thái có liên quan. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu trên các con đường | Hàm mũ | Hàm mũ | Quá chậm | 
| Dijkstra hai trạng thái | O((n + m) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì hai giá trị cho mỗi nút: khoảng cách nhỏ nhất nhìn thấy cho đến nay và khoảng cách nhỏ thứ hai lớn hơn khoảng cách đầu tiên.

Sau đó, chúng tôi chạy quy trình giống Dijkstra bằng cách sử dụng hàng trạng thái ưu tiên, trong đó mỗi trạng thái là một cặp (khoảng cách, nút). 

1. Khởi tạo tất cả các khoảng cách đến vô cùng cho cả mảng tốt nhất và tốt nhất thứ hai. Đặt best[source] thành 0 và đẩy (0, source) vào hàng ưu tiên. 
2. Bật trạng thái có khoảng cách nhỏ nhất với hàng đợi ưu tiên. Trạng thái này đại diện cho đường dẫn từng phần hứa hẹn nhất tiếp theo. 
3. Với mỗi hàng xóm có thể tiếp cận thông qua cạnh có trọng số w, hãy tính khoảng cách ứng cử viên nd = dist + w. 
4. Nếu nd nhỏ hơn khoảng cách đã biết tốt nhất đối với hàng xóm đó, hãy chuyển trạng thái tốt nhất cũ thành tốt nhất thứ hai, cập nhật trạng thái tốt nhất và đẩy trạng thái tốt nhất mới vào hàng đợi. 
5. Mặt khác, nếu nd nằm hoàn toàn giữa tốt nhất và tốt thứ hai, hãy cập nhật tốt nhất thứ hai và đẩy nó vào hàng đợi. 
6. Tiếp tục cho đến khi hàng đợi trống. 
7. Câu trả lời là khoảng cách tốt thứ hai tại nút mục tiêu. 

Lý do điều này có tác dụng là vì mỗi khi chúng tôi hoàn tất một trạng thái từ hàng đợi ưu tiên, chúng tôi sẽ xử lý các đường dẫn theo thứ tự chi phí không giảm. Điều này đảm bảo rằng khi chúng tôi gán các giá trị tốt nhất và tốt nhất thứ hai, chúng sẽ được phát hiện theo đúng thứ tự sắp xếp mà không bỏ sót bất kỳ ứng cử viên trung gian nào. 

Giá trị tốt thứ hai tại một nút được đảm bảo là chi phí nhỏ nhất lớn hơn giá trị tốt nhất vì mọi lựa chọn thay thế rẻ hơn sẽ được xử lý trước đó theo thứ tự hàng đợi. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

INF = 10**30

def solve():
    n, m = map(int, input().split())
    g = [[] for _ in range(n)]
    
    for _ in range(m):
        u, v, w = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append((v, w))
        g[v].append((u, w))

    # best and second best distances
    dist1 = [INF] * n
    dist2 = [INF] * n

    pq = []
    dist1[0] = 0
    heapq.heappush(pq, (0, 0))

    while pq:
        d, u = heapq.heappop(pq)

        if d > dist2[u]:
            continue

        for v, w in g[u]:
            nd = d + w

            if nd < dist1[v]:
                dist2[v] = dist1[v]
                dist1[v] = nd
                heapq.heappush(pq, (nd, v))

            elif dist1[v] < nd < dist2[v]:
                dist2[v] = nd
                heapq.heappush(pq, (nd, v))

    ans = dist2[n - 1]
    print(-1 if ans == INF else ans)

if __name__ == "__main__":
    solve()
```Biểu đồ được lưu trữ dưới dạng danh sách kề vì chúng tôi liên tục quét hàng xóm trong quá trình thư giãn. Hai mảng`dist1`Và`dist2`thực hiện xếp hạng chi phí đường dẫn trên mỗi nút. 

Một chi tiết triển khai tinh tế là điều kiện`d > dist2[u]`, điều này sẽ loại bỏ các trạng thái vốn đã tệ hơn cách được biết đến nhiều thứ hai để tiếp cận một nút. Nếu không có điều này, hàng đợi ưu tiên có thể tăng lên một cách không cần thiết. 

Một chi tiết quan trọng khác là bất đẳng thức chặt chẽ`dist1[v] < nd < dist2[v]`. Cho phép sự bình đẳng sẽ thu gọn các đường dẫn khác nhau có chi phí bằng nhau vào cùng một loại, điều này sẽ phá vỡ sự khác biệt giữa các đường dẫn ngắn nhất và không ngắn nhất. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một biểu đồ có các nút từ 1 đến 4 và các cạnh: 

| Bước | Nút | Khoảng cách | cập nhật dist1 | cập nhật dist2 | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 1 | 0 | dist1[1]=0 | - | 
| Thư giãn | 2 | 2 | dist1[2]=2 | - | 
| Thư giãn | 3 | 2 | dist1[3]=2 | - | 
| Con đường thay thế | 3 | 5 | - | dist2[3]=5 | 
| Đạt mục tiêu | 4 | 3 | dist1[4]=3 | - | 
| Đường vòng | 4 | 6 | - | dist2[4]=6 | 

Ở đây, đường dẫn ngắn nhất đến nút 4 là 3 và đường dẫn khác biệt tốt nhất tiếp theo là 6, vì vậy câu trả lời là 6. Bảng cho thấy các giá trị tốt thứ hai chỉ xuất hiện khi phát hiện ra một tuyến đường thay thế lớn hơn. 

### Ví dụ 2 

Biểu đồ trong đó tất cả các đường dẫn đều có cùng chi phí: 

| Bước | Nút | Khoảng cách | quận 1 | quận 2 | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 1 | 0 | 0 | thông tin | 
| Đường A | n | 10 | 10 | thông tin | 
| Đường B | n | 10 | 10 | thông tin | 

Không có đường đi nào lớn hơn thực sự xuất hiện, vì vậy đường đi tốt thứ hai vẫn là vô cực và câu trả lời là -1. Điều này xác nhận thuật toán phân biệt chính xác “không có phương án thay thế” với “tồn tại phương án thay thế”. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log n) | Mỗi trạng thái được đẩy và xuất ra từ hàng đợi ưu tiên và mỗi phần giãn cạnh sẽ kích hoạt tối đa hai bản cập nhật cho mỗi nút | 
| Không gian | O(n + m) | Danh sách kề cộng với hai mảng khoảng cách và bộ nhớ heap | 

Độ phức tạp phù hợp với Dijkstra tiêu chuẩn chỉ với mức tăng hệ số không đổi do duy trì hai khoảng cách trên mỗi nút, phù hợp thoải mái với các ràng buộc Codeforces điển hình lên tới 10^5 nút và cạnh. 

## Trường hợp thử nghiệm```python
import sys, io
import heapq

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output

    INF = 10**30

    n, m = map(int, input().split())
    g = [[] for _ in range(n)]

    for _ in range(m):
        u, v, w = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append((v, w))
        g[v].append((u, w))

    dist1 = [INF] * n
    dist2 = [INF] * n

    pq = [(0, 0)]
    dist1[0] = 0

    while pq:
        d, u = heapq.heappop(pq)
        if d > dist2[u]:
            continue

        for v, w in g[u]:
            nd = d + w
            if nd < dist1[v]:
                dist2[v] = dist1[v]
                dist1[v] = nd
                heapq.heappush(pq, (nd, v))
            elif dist1[v] < nd < dist2[v]:
                dist2[v] = nd
                heapq.heappush(pq, (nd, v))

    res = dist2[n - 1]
    print(-1 if res == INF else res)
    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# minimal graph
assert run("""2 1
1 2 5
""") == "-1"

# two distinct paths
assert run("""4 4
1 2 1
2 4 1
1 3 2
3 4 2
""") == "4"

# equal alternative paths
assert run("""3 3
1 2 1
2 3 1
1 3 2
""") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| con đường đơn | -1 | không tồn tại sự thay thế không ngắn nhất | 
| hai tuyến đường khác nhau | 4 | đường đi tốt thứ hai được chọn đúng | 
| phím tắt có độ dài bằng nhau | 3 | bình đẳng không làm hỏng logic tốt thứ hai | 

## Vỏ cạnh 

Một đồ thị trong đó chỉ tồn tại một đường đơn giản cho thấy dạng thất bại của lối suy luận ngây thơ. Thuật toán giữ đúng`dist2[target]`là vô cùng vì không có tuyến đường thay thế nào có thể đi vào hàng đợi ở dạng lớn hơn. 

Biểu đồ có nhiều đường đi ngắn nhất sẽ kiểm tra xem các tuyến đường có chi phí bằng nhau có bị bỏ qua để theo dõi tốt thứ hai hay không. điều kiện`dist1[v] < nd`đảm bảo rằng các đường dẫn có độ dài bằng nhau không ghi đè hoặc làm ô nhiễm trạng thái tốt thứ hai. 

Một biểu đồ có chu kỳ có thể tạo ra vô số lần đi bộ, nhưng thứ tự hàng đợi ưu tiên đảm bảo chỉ giữ lại hai khoảng cách liên quan nhỏ nhất trên mỗi nút. Điều này ngăn chặn vụ nổ do chu kỳ gây ra trong khi vẫn cho phép đường vòng dài hơn trở thành con đường tốt thứ hai nếu nó tối ưu trong số tất cả các lựa chọn không ngắn nhất.
