---
title: "CF 105383K - Kế hoạch phát triển của Vương quốc"
description: "Chúng ta được cung cấp một tập hợp các dự án, mỗi dự án được đánh nhãn từ 1 đến n, cùng với một danh sách các quy tắc phụ thuộc có dạng “dự án a phải được hoàn thành trước khi dự án b có thể bắt đầu."
date: "2026-06-23T16:12:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105383
codeforces_index: "K"
codeforces_contest_name: "2024 ICPC Asia Taiwan Online Programming Contest"
rating: 0
weight: 105383
solve_time_s: 54
verified: true
draft: false
---

[CF 105383K - Kế hoạch phát triển của Vương quốc](https://codeforces.com/problemset/problem/105383/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các dự án, mỗi dự án được gắn nhãn từ 1 đến n, cùng với một danh sách các quy tắc phụ thuộc có dạng “dự án a phải hoàn thành trước khi dự án b có thể bắt đầu”. Các quy tắc này xác định mối quan hệ trực tiếp giữa các dự án và nhiệm vụ là quyết định xem liệu tất cả các dự án có thể được hoàn thành theo thứ tự tôn trọng mọi sự phụ thuộc hay không. Nếu thứ tự như vậy tồn tại, chúng ta phải xuất ra một thứ tự hợp lệ cho tất cả các dự án. Nếu tồn tại nhiều đơn hàng hợp lệ, chúng ta phải chọn đơn hàng nhỏ nhất về mặt từ điển. 

Từ góc độ đồ thị, mỗi dự án là một nút và mỗi phụ thuộc là một cạnh có hướng từ a đến b. Do đó, đầu ra là thứ tự tôpô của đồ thị có hướng, với ràng buộc bổ sung là trong số tất cả các thứ tự tôpô hợp lệ, chúng ta phải chọn thứ tự nhỏ nhất theo thứ tự từ điển. 

Các ràng buộc lên tới n = 100000 và m = 200000. Bất kỳ giải pháp bậc hai nào trong n, chẳng hạn như quét liên tục tất cả các nút để tìm dự án hợp lệ tiếp theo, sẽ không hoạt động. Về cơ bản, chúng tôi bị giới hạn ở thời gian tuyến tính hoặc gần tuyến tính, nghĩa là O(n log n) hoặc O(n + m) là phạm vi mục tiêu. Điều này đã gợi ý rõ ràng về một phương pháp truyền tải đồ thị hơn là bất kỳ tìm kiếm tổ hợp nào. 

Một số trường hợp đặc biệt quan trọng ở đây. Đầu tiên, nếu có một chu trình, ví dụ 1 phụ thuộc vào 2, 2 phụ thuộc vào 3 và 3 phụ thuộc vào 1, thì không thể sắp xếp thứ tự và chúng ta phải xuất ra KHÔNG THỂ. Thứ hai, khi tồn tại nhiều dự án hợp lệ tiếp theo, yêu cầu nhỏ nhất về mặt từ điển buộc chúng ta phải luôn ưu tiên dự án được lập chỉ mục nhỏ nhất có sẵn ở mỗi bước, nếu không, việc sắp xếp cấu trúc liên kết tham lam nhưng không có thứ tự sẽ tạo ra một chuỗi hợp lệ nhưng không tối thiểu. Thứ ba, khi không có sự phụ thuộc nào cả, câu trả lời phải đơn giản là 1 2 3 ... n. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là quét liên tục tất cả các dự án và chọn bất kỳ dự án nào có điều kiện tiên quyết được đáp ứng. Ở mỗi bước, chúng tôi sẽ kiểm tra tất cả các cạnh hoặc tính toán lại mức độ từ đầu để xác định nút nào hiện có sẵn. Điều này hoạt động về mặt khái niệm vì chúng tôi luôn tôn trọng sự phụ thuộc, nhưng mỗi bước lựa chọn có thể tốn O(n + m) khi triển khai đơn giản. Vì chúng ta thực hiện việc này n lần nên trường hợp xấu nhất sẽ trở thành O(n(n + m)), quá lớn so với giới hạn đầu vào. 

Quan sát quan trọng là chúng ta không bao giờ cần tính toán lại mức độ hài lòng của người phụ thuộc từ đầu. Những gì chúng ta thực sự cần là một tập hợp năng động của tất cả các dự án hiện có, nghĩa là những dự án có mức độ bằng 0. Khi một dự án được thực hiện, chỉ những người hàng xóm bên ngoài của nó bị ảnh hưởng. Điều này tự nhiên gợi ý việc duy trì mức độ và cập nhật chúng dần dần. 

Để thực thi thứ tự nhỏ nhất về mặt từ điển, chúng ta phải luôn chọn dự án nhỏ nhất có sẵn ở mỗi bước. Điều này biến vấn đề thành một nhiệm vụ sắp xếp tôpô tiêu chuẩn với cơ chế ưu tiên. Thay vì xếp hàng tùy ý, chúng tôi sử dụng vùng nhớ tối thiểu để luôn trích xuất nút được lập chỉ mục nhỏ nhất có bậc bằng 0. 

Mỗi lần chúng tôi loại bỏ một nút khỏi vùng nhớ heap, chúng tôi sẽ mô phỏng việc hoàn thành dự án đó và giảm mức độ của các nút lân cận của nó. Bất kỳ hàng xóm nào có mức độ bằng 0 sẽ được thêm vào heap. Điều này đảm bảo chúng tôi luôn duy trì ranh giới chính xác của các nút có sẵn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n(n + m)) | O(n + m) | Quá chậm | 
| Tối ưu (sắp xếp cấu trúc liên kết tối thiểu) | O((n + m) log n) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta chuyển bài toán thành đồ thị có hướng và theo dõi mức độ.

1. Xây dựng danh sách kề cho đồ thị và tính bậc cho mỗi nút. Điều này cho chúng ta biết mỗi dự án vẫn còn bao nhiêu điều kiện tiên quyết. 
2. Khởi tạo một vùng tối thiểu và đẩy tất cả các nút có mức độ bằng 0. Đây là những dự án có thể được bắt đầu ngay lập tức vì chúng không phụ thuộc vào điều gì. 
3. Liên tục trích xuất nút nhỏ nhất từ ​​​​đống. Sự lựa chọn này đảm bảo tính tối thiểu về mặt từ điển ở mỗi bước trong số tất cả các ứng cử viên hợp lệ hiện tại. 
4. Nối nút được trích xuất vào chuỗi câu trả lời vì chúng tôi hiện đang cam kết hoàn thành dự án đó ở vị trí này. 
5. Đối với mọi hàng xóm của nút này, hãy giảm mức độ của nó. Điều này thể hiện việc đáp ứng một điều kiện tiên quyết cho những dự án phụ thuộc đó. 
6. Nếu indegree của bất kỳ người hàng xóm nào trở thành 0, hãy đẩy nó vào đống, vì nó đã đủ điều kiện để thực thi. 
7. Tiếp tục cho đến khi heap trống. Tại thời điểm đó, tất cả các nút đã được xử lý hoặc tồn tại một chu trình. 
8. Cuối cùng, kiểm tra xem thứ tự kết quả có chứa tất cả n nút hay không. Nếu không, một số nút không bao giờ được giải phóng khỏi sự phụ thuộc, nghĩa là tồn tại một chu trình, do đó xuất ra KHÔNG THỂ. 

Chi tiết quan trọng là vùng heap luôn phản ánh chính xác tập hợp các nút hiện có giá trị để lấy và chúng tôi luôn chọn nút nhỏ nhất trong số đó. 

### Tại sao nó hoạt động 

Ở mỗi bước, thuật toán duy trì tính bất biến rằng tất cả các nút đã xuất ra đều có mức độ bằng 0 đối với biểu đồ còn lại và tất cả các nút hiện trong vùng heap chính xác là những nút có tất cả các điều kiện tiên quyết đều được thỏa mãn. Bởi vì các cạnh chỉ bị loại bỏ hoàn toàn thông qua các cập nhật mức độ, nên không có nút nào được chèn vào thứ tự trước khi tất cả các phụ thuộc của nó xuất hiện trước đó trong chuỗi. Thứ tự đống đảm bảo rằng trong số tất cả các lựa chọn hợp lệ tiếp theo, chỉ mục nhỏ nhất được chọn, do đó không thể tồn tại chuỗi hợp lệ nhỏ hơn về mặt từ điển mà phân kỳ sớm hơn so với chuỗi được xây dựng. 

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

    order = []

    while heap:
        u = heapq.heappop(heap)
        order.append(u)

        for v in adj[u]:
            indeg[v] -= 1
            if indeg[v] == 0:
                heapq.heappush(heap, v)

    if len(order) != n:
        print("IMPOSSIBLE")
    else:
        print(*order)

if __name__ == "__main__":
    solve()
```Việc triển khai bắt đầu bằng cách xây dựng danh sách kề và tính toán mức độ, đây là biểu diễn cấu trúc của tất cả các ràng buộc tiên quyết. Sau đó, vùng heap được gieo hạt với tất cả các nút có độ bằng 0, tạo thành tập hợp hợp lệ ban đầu của các dự án khởi đầu. 

Vòng lặp chính liên tục bật nút nhỏ nhất có sẵn. Sử dụng một đống thay vì hàng đợi là điều thực thi tính tối thiểu từ điển; thay thế nó bằng một hàng đợi thông thường vẫn sẽ tạo ra một thứ tự tôpô hợp lệ nhưng không nhất thiết phải là thứ tự nhỏ nhất. 

Mỗi lần chúng ta loại bỏ một nút, chúng ta sẽ truyền tác dụng của nó qua các cạnh đi ra bằng cách giảm dần độ. Một người hàng xóm chỉ được đẩy khi mức độ của nó chính xác bằng 0, đảm bảo nó trở nên đủ điều kiện chính xác khi tất cả các điều kiện tiên quyết được đáp ứng. 

Kiểm tra cuối cùng so sánh kích thước đơn đặt hàng được xây dựng với n. Nếu chúng khác nhau, thì ít nhất một nút không bao giờ có thể truy cập được thông qua việc giảm mức độ, điều này chỉ có thể xảy ra khi có một chu trình. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 5
1 2
2 3
2 4
2 5
3 4
```Bằng cấp ban đầu: 

| Bước | Đống | Được chọn | Đặt hàng | Thay đổi mức độ | 
| --- | --- | --- | --- | --- | 
| 1 | [1] | 1 | [1] | 2 trở thành 0 | 
| 2 | [2] | 2 | [1,2] | 3,4,5 cập nhật | 
| 3 | [3,4,5] | 3 | [1,2,3] | 4 cập nhật | 
| 4 | [4,5] | 4 | [1,2,3,4] | không | 
| 5 | [5] | 5 | [1,2,3,4,5] | xong | 

Heap đảm bảo rằng khi có nhiều nút, chỉ mục nhỏ nhất luôn được chọn trước tiên, tạo ra thứ tự hợp lệ nhỏ nhất về mặt từ điển. 

### Ví dụ 2 

đầu vào:```
5 4
1 2
2 3
3 1
5 4
```| Bước | Đống | Được chọn | Đặt hàng | 
| --- | --- | --- | --- | 
| bắt đầu | [4,5] | - | [] | 
| 1 | [4,5] | 4 | [4] | 
| 2 | [5] | 5 | [4,5] | 
| dừng lại | [] | - | [4,5] | 

Các nút 1, 2, 3 không bao giờ vào heap vì chu trình của chúng giữ mức độ của chúng luôn khác 0. Chỉ các nút trong thành phần không tuần hoàn mới được xử lý và độ dài cuối cùng nhỏ hơn n, kích hoạt KHÔNG THỂ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log n) | Mỗi nút vào và ra khỏi heap nhiều nhất một lần và mỗi cạnh gây ra một lần cập nhật mức độ | 
| Không gian | O(n + m) | danh sách kề cộng với mảng đẳng cấp cộng với đống | 

Độ phức tạp phù hợp thoải mái trong các giới hạn vì m lên tới 200000 và mỗi thao tác heap là logarit tính bằng n, làm cho tổng số thao tác có thể quản lý được trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io
import heapq

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
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

        order = []
        while heap:
            u = heapq.heappop(heap)
            order.append(u)
            for v in adj[u]:
                indeg[v] -= 1
                if indeg[v] == 0:
                    heapq.heappush(heap, v)

        if len(order) != n:
            return "IMPOSSIBLE"
        return " ".join(map(str, order))

    return solve()

# provided samples
assert run("5 5\n1 2\n2 3\n2 4\n2 5\n3 4\n") == "1 2 3 4 5"
assert run("5 4\n1 2\n2 3\n3 1\n5 4\n") == "IMPOSSIBLE"

# custom cases
assert run("1 0\n") == "1", "single node"
assert run("3 0\n") == "1 2 3", "no edges lexicographically smallest"
assert run("3 3\n1 2\n2 3\n1 3\n") == "1 2 3", "chain with extra edge"
assert run("4 2\n2 1\n3 1\n") == "2 3 1 4", "multiple sources ordering"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 1 | trường hợp cơ sở tối thiểu | 
| không có cạnh | 1 2 3 | DAG tầm thường nhỏ nhất về mặt từ điển | 
| cạnh thêm | 1 2 3 | xử lý ràng buộc dư thừa | 
| nhiều nguồn | 2 3 1 4 | ưu tiên đống chính xác | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi nhiều nút có bậc bằng 0 khi bắt đầu. Ví dụ: với các cạnh 2 → 3 và 4 → 5 trong biểu đồ 5 nút, ban đầu cả 1, 2 và 4 đều có sẵn. Thuật toán đặt tất cả chúng vào heap và luôn chọn cái nhỏ nhất, đảm bảo hành vi từ điển xác định. 

Một trường hợp cạnh khác là đồ thị tuần hoàn đầy đủ, chẳng hạn như 1 → 2 → 3 → 1. Ở đây không có nút nào đạt đến mức 0, do đó vùng heap bắt đầu trống hoặc trở nên trống ngay sau khi khởi tạo. Danh sách đầu ra vẫn chưa đầy đủ và thuật toán đưa ra chính xác KHÔNG THỂ. 

Trường hợp cạnh cuối cùng là một biểu đồ có các thành phần bị ngắt kết nối. Mỗi thành phần được xử lý độc lập thông qua sự lan truyền không đồng đều và vùng heap hợp nhất chúng một cách tự nhiên thành một thứ tự chung duy nhất bằng cách luôn chọn nút có sẵn nhỏ nhất trên các thành phần.
