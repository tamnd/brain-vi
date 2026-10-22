---
title: "CF 105900A - Leo núi"
description: "Chúng ta được cấp một tập hợp các ngọn núi được dán nhãn từ 1 đến N, trong đó mỗi nhãn cũng thể hiện mức độ khó của nó. Có M quy tắc ưu tiên dạng A trước B, nghĩa là Isa không thể leo lên núi B trừ khi cô ấy đã leo lên núi A."
date: "2026-06-21T18:12:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105900
codeforces_index: "A"
codeforces_contest_name: "VI UnBalloon Contest Mirror"
rating: 0
weight: 105900
solve_time_s: 49
verified: true
draft: false
---

[CF 105900A - Leo núi](https://codeforces.com/problemset/problem/105900/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một tập hợp các ngọn núi được dán nhãn từ 1 đến N, trong đó mỗi nhãn cũng thể hiện mức độ khó của nó. Có M quy tắc ưu tiên dạng A trước B, nghĩa là Isa không thể leo lên núi B trừ khi cô ấy đã leo lên núi A. Vì A < B luôn được đảm bảo nên mọi phụ thuộc chỉ trỏ từ ngọn núi được đánh số thấp hơn đến ngọn núi được đánh số cao hơn. 

Nhiệm vụ là xây dựng một thứ tự trong đó Isa leo tất cả các ngọn núi đúng một lần đồng thời tôn trọng tất cả các phụ thuộc. Trong số tất cả các lệnh hợp lệ, chúng ta phải xuất ra lệnh nhỏ nhất về mặt từ điển, nghĩa là chúng ta luôn ưu tiên đặt các dãy núi có số nhỏ hơn càng sớm càng tốt, miễn là chúng ta không vi phạm bất kỳ ràng buộc tiên quyết nào. Nếu không có đơn đặt hàng hợp lệ nào tồn tại, chúng tôi phải báo cáo là không thể thực hiện được. 

Cấu trúc là một đồ thị có hướng trên các đỉnh từ 1 đến N và mỗi ràng buộc là một cạnh có hướng A → B. Mục tiêu là một thứ tự tôpô, nhưng không chỉ là bất kỳ thứ tự tôpô nào, nhỏ nhất theo thứ tự từ điển. 

Các ràng buộc lên tới 2 · 10^5 đỉnh và cạnh. Điều này ngay lập tức loại trừ mọi chiến lược mô phỏng O(NM) hoặc O(N^2). Ngay cả việc truyền tải đồ thị O(NM) cũng quá chậm. Các giải pháp duy nhất được chấp nhận là gần với O((N + M) log N) hoặc O(N + M). 

Một hạn chế về cấu trúc quan trọng là các cạnh chỉ đi từ nhãn nhỏ hơn đến nhãn lớn hơn. Điều này đảm bảo biểu đồ không có tính chu kỳ khi xây dựng, vì bạn không bao giờ có thể quay lại chỉ mục nhỏ hơn, do đó các chu kỳ không thể tồn tại. Tuy nhiên, chúng ta vẫn cần tôn trọng các ràng buộc về thứ tự và tối ưu hóa đầu ra từ điển. 

Một cạm bẫy ngây thơ nhưng quan trọng là nghĩ rằng chỉ cần sắp xếp danh sách lân cận hoặc tham lam chọn nút nhỏ nhất có sẵn mà không theo dõi sự phụ thuộc cẩn thận là đủ. Ví dụ: nếu chúng tôi luôn chọn nút chưa được truy cập nhỏ nhất trên toàn cầu, chúng tôi có thể chọn một nút có điều kiện tiên quyết chưa được đáp ứng. 

Một trường hợp phức tạp khác là khi có nhiều nút có sẵn tại các thời điểm khác nhau. Nếu chúng tôi không duy trì cấu trúc toàn cầu của các nút hiện hợp lệ, chúng tôi có thể bỏ lỡ phần tiếp theo nhỏ nhất về mặt từ điển. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là quét liên tục tất cả các nút và chọn nút nhỏ nhất chưa được sử dụng và các điều kiện tiên quyết của nó đã được đáp ứng. Sau khi chọn nó, chúng tôi đánh dấu nó là đã sử dụng và lặp lại. Để kiểm tra tính hợp lệ mỗi lần, chúng tôi tính toán lại theo độ hoặc quét tất cả các cạnh ảnh hưởng đến nó. 

Điều này hoạt động hợp lý vì ở mỗi bước chúng tôi đều đảm bảo tuân thủ các ràng buộc nhưng chi phí rất cao. Đối với mỗi bước trong số N bước, chi phí quét tất cả các nút là O(N) và việc xác minh các điều kiện tiên quyết có thể cộng tới O(M). Điều này dẫn đến hành vi O(N^2 + M), vượt xa giới hạn khi N là 2 · 10^5. 

Quan sát quan trọng là đây là một bài toán sắp xếp tôpô cổ điển, và điểm khác biệt duy nhất là chúng ta phải luôn chọn nút khả dụng nhỏ nhất trong số những nút có bậc bằng 0. Điều đó cho thấy việc duy trì một tập hợp năng động các ứng cử viên hiện có hiệu lực. 

Khi chúng tôi duy trì tất cả các nút không có cạnh đầu vào trong một cấu trúc luôn cung cấp cho chúng tôi phần tử nhỏ nhất, quá trình này sẽ trở nên đơn giản. Chúng tôi liên tục trích xuất nút nhỏ nhất có sẵn, nối nó vào câu trả lời và loại bỏ các cạnh đi ra của nó, có thể kích hoạt các nút mới. Hàng đợi heap tối thiểu hoặc hàng đợi ưu tiên là công cụ tự nhiên cho việc này. 

Điều kiện A < B đảm bảo biểu đồ đã có tính chu kỳ, vì vậy chúng tôi không cần logic phát hiện chu kỳ ngoài việc xác minh xem liệu chúng tôi có xử lý tất cả các nút ở cuối hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Quét Brute Force từng bước | O(N^2 + M) | O(N + M) | Quá chậm | 
| Sắp xếp cấu trúc liên kết từ điển tối thiểu | O((N + M) log N) | O(N + M) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi mô hình hóa vấn đề dưới dạng đồ thị có hướng và tính toán độ cho mỗi nút. Các nút có mức độ bằng 0 là những nút có điều kiện tiên quyết đã được thỏa mãn. 

1. Xây dựng danh sách kề cho tất cả các cạnh A → B và tính bậc [B] cho mỗi cạnh. Điều này mã hóa chính xác số lượng điều kiện tiên quyết mà mỗi ngọn núi vẫn cần. 
2. Khởi tạo một min-heap (hàng đợi ưu tiên) và chèn mọi nút i sao cho indegree[i] = 0. Đây là những ngọn núi có thể leo lên ngay lập tức mà không vi phạm các ràng buộc. 
3. Liên tục trích xuất phần tử nhỏ nhất từ ​​heap. Điều này đảm bảo rằng trong số tất cả các ngọn núi hiện có, chúng tôi luôn chọn tùy chọn nhỏ nhất về mặt từ điển. 
4. Thêm nút đã chọn này vào chuỗi câu trả lời. 
5. Đối với mỗi lân cận v của nút u đã chọn, giảm bậc [v] đi 1. Nếu bậc [v] bằng 0, đẩy v vào heap. Điều này thể hiện rằng tất cả các điều kiện tiên quyết của v hiện đã được thỏa mãn, vì vậy nó sẽ có sẵn để lựa chọn. 
6. Tiếp tục cho đến khi heap trống. 
7. Nếu chuỗi kết quả có độ dài N, hãy xuất nó ra. Nếu không thì xuất -1. 

Tính chính xác của việc luôn đẩy các nút mới được mở khóa ngay lập tức vào vùng heap đảm bảo rằng chúng tôi không bao giờ trì hoãn một ứng cử viên hợp lệ có thể được chọn trước đó theo từ điển. 

### Tại sao nó hoạt động 

Ở mỗi bước, heap chứa chính xác tập hợp các nút mà tất cả các điều kiện tiên quyết đều được các nút đã chọn trước đó đáp ứng. Bất kỳ nút nào không có trong heap đều có phần phụ thuộc còn lại, vì vậy việc chọn nút đó là không hợp lệ. Trong số các ứng cử viên hợp lệ, việc chọn nhãn nhỏ nhất là tối ưu vì bất kỳ lựa chọn nào lớn hơn sẽ làm xấu đi trình tự về mặt từ điển mà không tạo ra bất kỳ lợi thế nào sau này. Cấu trúc phụ thuộc đảm bảo rằng một khi một nút có sẵn, việc trì hoãn nó chỉ có thể đẩy nó đi xa hơn theo đúng thứ tự, không bao giờ cải thiện tính khả thi hoặc thứ tự từ điển. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    adj = [[] for _ in range(n + 1)]
    indeg = [0] * (n + 1)

    for _ in range(m):
        a, b = map(int, input().split())
        adj[a].append(b)
        indeg[b] += 1

    heap = []
    for i in range(1, n + 1):
        if indeg[i] == 0:
            heapq.heappush(heap, i)

    res = []

    while heap:
        u = heapq.heappop(heap)
        res.append(u)

        for v in adj[u]:
            indeg[v] -= 1
            if indeg[v] == 0:
                heapq.heappush(heap, v)

    if len(res) != n:
        print(-1)
    else:
        print(*res)

if __name__ == "__main__":
    solve()
```Danh sách kề lưu trữ tất cả các cạnh phụ thuộc, trong khi mảng mức độ theo dõi xem mỗi nút vẫn chưa đáp ứng bao nhiêu điều kiện tiên quyết. Heap là cấu trúc chính thực thi tính tối ưu từ điển bằng cách luôn chọn nút có sẵn nhỏ nhất. 

Một chi tiết triển khai tinh tế là các nút chỉ được đẩy vào heap khi mức độ của chúng chính xác bằng 0. Việc đẩy sớm hơn sẽ vi phạm tính chính xác vì nút đó chưa hợp lệ để sử dụng. 

Kiểm tra cuối cùng đảm bảo rằng tất cả các nút đã được xử lý. Mặc dù đầu vào đảm bảo A < B, việc kiểm tra này vẫn là điều kiện an toàn tiêu chuẩn cho các bài toán sắp xếp tôpô. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2
1 2
2 3
```| Bước | Đống | Được chọn | Kết quả | Bằng cấp | 
| --- | --- | --- | --- | --- | 
| 1 | [1] | 1 | [1] | (2:1, 3:0) | 
| 2 | [2] | 2 | [1,2] | (3:0) | 
| 3 | [3] | 3 | [1,2,3] | tất cả đều bằng không | 

Đống luôn chứa chính xác một lựa chọn hợp lệ, do đó việc sắp xếp thứ tự là bắt buộc. Điều này xác nhận rằng thuật toán suy biến chính xác thành một quá trình duyệt chuỗi đơn giản. 

### Ví dụ 2 

đầu vào:```
5 2
1 5
2 5
```| Bước | Đống | Được chọn | Kết quả | Bằng cấp | 
| --- | --- | --- | --- | --- | 
| 1 | [1,2,3,4] | 1 | [1] | 5:1 | 
| 2 | [2,3,4] | 2 | [1,2] | 5:0 | 
| 3 | [3,4,5] | 3 | [1,2,3] | 5:0 | 
| 4 | [4,5] | 4 | [1,2,3,4] | 5:0 | 
| 5 | [5] | 5 | [1,2,3,4,5] | xong | 

Điều này cho thấy nút 5 chỉ khả dụng như thế nào sau khi cả hai điều kiện tiên quyết được xử lý và cách vùng heap thực thi thứ tự từ điển giữa các nút độc lập một cách tự nhiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((N + M) log N) | Mỗi nút được đẩy và bật nhiều nhất một lần, mỗi cạnh được xử lý một lần, chi phí hoạt động heap log N | 
| Không gian | O(N + M) | danh sách kề cộng với mảng không độ và lưu trữ đống | 

Các ràng buộc cho phép tối đa 2 · 10^5 nút và cạnh, do đó chi phí logarit có thể chấp nhận được. Giải pháp thoải mái phù hợp trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io
import heapq

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    input = _sys.stdin.readline

    n, m = map(int, input().split())
    adj = [[] for _ in range(n + 1)]
    indeg = [0] * (n + 1)

    for _ in range(m):
        a, b = map(int, input().split())
        adj[a].append(b)
        indeg[b] += 1

    heap = []
    for i in range(1, n + 1):
        if indeg[i] == 0:
            heapq.heappush(heap, i)

    res = []
    while heap:
        u = heapq.heappop(heap)
        res.append(u)
        for v in adj[u]:
            indeg[v] -= 1
            if indeg[v] == 0:
                heapq.heappush(heap, v)

    if len(res) != n:
        return "-1"
    return " ".join(map(str, res))

# provided samples
assert run("3 2\n1 2\n2 3\n") == "1 2 3"
assert run("5 1\n1 5\n") == "1 2 3 4 5"

# custom cases
assert run("1 0\n") == "1"
assert run("3 0\n") == "1 2 3"
assert run("3 3\n1 2\n2 3\n1 3\n") == "1 2 3"
assert run("4 3\n1 2\n1 3\n1 4\n") == "1 2 3 4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 nút, không có cạnh | 1 | đồ thị tối thiểu | 
| không có ràng buộc | sắp xếp thứ tự | tất cả các nút ban đầu có sẵn | 
| xích + cạnh phụ | 1 2 3 | xử lý phụ thuộc dư thừa | 
| sao từ 1 | 1 2 3 4 | ưu tiên từ điển giữa các nút độc lập | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi không tồn tại sự phụ thuộc. Trong tình huống đó, ban đầu mọi nút đều có bậc bằng 0, do đó vùng chứa tất cả các nút. Sau đó, thuật toán chỉ xuất ra từ 1 đến N theo thứ tự vì vùng heap thực thi việc lựa chọn từ điển ở mỗi bước. 

Một trường hợp khác là khi các phần phụ thuộc tạo thành một chuỗi chặt chẽ. Ví dụ: 1 → 2 → 3 → 4. Đống luôn chứa chính xác một phần tử, do đó thuật toán hoạt động giống như một phép duyệt đơn giản. Không có sự mơ hồ trong thứ tự và chi phí heap không làm thay đổi tính chính xác. 

Một trường hợp tinh tế hơn là khi một nút khả dụng muộn nhưng lại nhỏ hơn các nút đã có sẵn. Ví dụ: nếu nút 3 khả dụng sau nút 5, vùng heap đảm bảo nút 3 được chọn ngay khi chèn, duy trì tính tối thiểu từ điển. Đây chính xác là tình huống mà cách tiếp cận ngây thơ “quét tất cả các ứng viên mỗi lần” sẽ xử lý sai hoặc triển khai không hiệu quả.
