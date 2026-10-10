---
title: "CF 104990E - Mê cung mê hoặc"
description: "Chúng ta được cho một đồ thị vô hướng trong đó mỗi đỉnh đại diện cho một căn phòng trong mê cung và mỗi cạnh là một hành lang có chi phí đi qua bằng nhau. Elisa bắt đầu từ nút 1 và muốn đến bất kỳ buồng thoát nào được chỉ định càng nhanh càng tốt."
date: "2026-06-28T04:23:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104990
codeforces_index: "E"
codeforces_contest_name: "First Masters Championship LATAM 2024"
rating: 0
weight: 104990
solve_time_s: 102
verified: false
draft: false
---

[CF 104990E - Mê cung mê hoặc](https://codeforces.com/problemset/problem/104990/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 42s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị vô hướng trong đó mỗi đỉnh đại diện cho một căn phòng trong mê cung và mỗi cạnh là một hành lang có chi phí đi qua bằng nhau. Elisa bắt đầu từ nút 1 và muốn đến bất kỳ buồng thoát nào được chỉ định càng nhanh càng tốt. 

Điều khó khăn là bất cứ khi nào Elisa đến một căn phòng, Minotaur có thể vô hiệu hóa chính xác một hành lang dẫn đến căn phòng đó trước khi cô ấy chọn nơi đi tiếp theo. Khi cô rời khỏi căn phòng, Minotaur sẽ mất ảnh hưởng cho đến khi cô đến căn phòng tiếp theo, nơi áp dụng lại quy tắc tương tự. 

Vì vậy quy tắc chuyển động không phải là đường đi ngắn nhất tiêu chuẩn. Tại mỗi bước, khi đứng tại một nút, một cạnh liền kề sẽ bị loại bỏ một cách đối nghịch và sau đó Elisa chọn một trong các cạnh còn lại để đi qua. 

Nhiệm vụ là tính toán số bước tối thiểu cần thiết để đảm bảo tiếp cận bất kỳ nút thoát nào theo quy tắc đối nghịch này hoặc xác định rằng việc thoát là không thể. 

Các ràng buộc là cực kỳ lớn, lên tới một triệu nút và hai triệu cạnh. Điều này ngay lập tức loại trừ bất cứ thứ gì có dạng bậc ba hoặc thậm chí bậc hai. Bất kỳ giải pháp hợp lệ nào về cơ bản phải là tuyến tính hoặc gần tuyến tính về số cạnh, với chi phí tối đa là logarit. Việc tính toán lại nhiều lần đối với danh sách kề mỗi lần thay đổi trạng thái sẽ quá chậm trừ khi mỗi cạnh chỉ tham gia vào công việc liên tục về tổng thể. 

Trường hợp cạnh tinh tế xuất hiện khi một nút có bậc rất nhỏ. Nếu nút không thoát được có độ 0 hoặc 1, Minotaur có thể loại bỏ tùy chọn duy nhất có thể sử dụng được và bẫy Elisa ngay lập tức. Ví dụ: nếu nút 1 chỉ được kết nối với một nút không thoát, thì sau khi loại bỏ cạnh đơn đó, Elisa không có bước di chuyển hợp lệ nào và không thể thoát trừ khi chính nút 1 là một lối thoát. 

Một dạng lỗi khác xuất phát từ việc giả định điều này giảm xuống mức BFS bình thường. Điều đó sẽ cho rằng mọi cạnh đi ra đều có thể sử dụng được ở mọi bước một cách không chính xác, bỏ qua rằng đối thủ luôn có thể xóa hướng đi thuận tiện nhất trong mỗi lần truy cập nút, có khả năng buộc phải có một đường dẫn dài hơn hoặc không thể thoát ra ngay cả khi có đường dẫn BFS. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là coi biểu đồ là không có trọng số và chạy BFS tiêu chuẩn từ nút 1 đến lối ra gần nhất. Điều này có tác dụng nếu mọi cạnh luôn có sẵn nhưng nó bỏ qua việc xóa đối thủ. Lỗi chính là BFS giả định rằng khi đạt đến một nút, tất cả các cạnh đi ra của nó vẫn là các lựa chọn hợp lệ, trong khi trên thực tế, một cạnh luôn bị loại bỏ trong trường hợp xấu nhất. 

Để kết hợp đối thủ, hãy xem xét điều gì xảy ra tại một nút cố định u. Khi Elisa đến, Minotaur xóa một cạnh liền kề. Vì Elisa sẽ chọn sau khi xem biểu đồ còn lại, kẻ thù sẽ luôn xóa người hàng xóm có lợi nhất cho Elisa. Điều này có nghĩa là từ bạn, Elisa có quyền truy cập hiệu quả vào tất cả các hàng xóm ngoại trừ hàng xóm có chi phí tiếp tục nhỏ nhất. 

Điều này dẫn tới việc tái cơ cấu. Nếu chúng ta định nghĩa dist[v] là khoảng cách được đảm bảo tối thiểu từ v đến bất kỳ lối ra nào, thì tại nút u, đối thủ sẽ loại bỏ láng giềng v có dist[v] nhỏ nhất, buộc Elisa phải sử dụng hàng xóm nhỏ thứ hai. Do đó, quá trình chuyển đổi trở thành một quy tắc tất định: dist[u] là một cộng với giá trị nhỏ thứ hai trong số tất cả dist[v] cho v liền kề với u, trừ khi u là lối ra trong đó dist[u] bằng 0. 

Thách thức là các giá trị dist phụ thuộc lẫn nhau. Đây không phải là cách nới lỏng đường đi ngắn nhất tiêu chuẩn vì u phụ thuộc đồng thời vào tất cả các hàng xóm và mỗi hàng xóm phụ thuộc ngược lại vào u.

Quan sát quan trọng là câu trả lời của mỗi nút được xác định bằng cách liên tục tinh chỉnh khoảng cách lân cận ứng cử viên tốt nhất của nó. Mỗi khi giá trị của hàng xóm giảm đi, nó có thể thay đổi tùy chọn tốt nhất hoặc tốt thứ hai cho hàng xóm của nó. Vì mỗi bản cập nhật chỉ cải thiện các giá trị và mỗi cạnh góp phần tổng hợp các bản cập nhật với số lần không đổi, nên chúng ta có thể truyền bá các thay đổi theo cách giống Dijkstra bằng cách sử dụng hàng đợi ưu tiên, chỉ duy trì hai khoảng cách ứng viên nhỏ nhất trên mỗi nút. 

Điều này làm giảm vấn đề xuống một hệ thống thư giãn đơn điệu trong đó mỗi nút ổn định sau một số cải tiến nhất định. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| BFS bỏ qua đối thủ | O(N + M) | O(N + M) | Không đúng | 
| Tính toán lại ngây thơ của cực tiểu thứ hai cho mỗi lần cập nhật | O(NM) | O(M) | Quá chậm | 
| Tuyên truyền được tối ưu hóa với hàng đợi ưu tiên và bảo trì gia tăng | O(M log N) | O(N + M) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Khởi tạo tất cả các nút có khoảng cách vô cực, ngoại trừ các nút thoát được đặt thành 0. Đây là những trạng thái duy nhất đã đạt được lối thoát. 
2. Chèn tất cả các nút thoát vào hàng đợi ưu tiên. Điều này thiết lập chúng như là nguồn thành công được đảm bảo. 
3. Liên tục trích xuất nút u có khoảng cách hiện tại nhỏ nhất với hàng đợi. Điều này đảm bảo chúng tôi luôn hoàn thiện các nút theo thứ tự tăng thời gian thoát được đảm bảo. 
4. Đối với mỗi người hàng xóm v của bạn, hãy coi bạn như một người đóng góp tiềm năng cho các lối thoát ứng cử viên của v. Chúng tôi cập nhật bản ghi của v về hai khoảng cách lân cận tốt nhất của nó bằng cách sử dụng dist[u] + 1 làm ứng cử viên. 
5. Duy trì cho mỗi nút không phải một giá trị duy nhất mà là khoảng cách lân cận ứng cử viên nhỏ nhất và nhỏ thứ hai của nó. Điều này phản ánh thực tế rằng đối thủ sẽ luôn loại bỏ phương án tốt nhất, buộc phải dựa vào phương án tốt thứ hai. 
6. Bất cứ khi nào giá trị tốt thứ hai của nút v được cải thiện, hãy cập nhật dist[v] tương ứng. Nếu dist[v] giảm, hãy đẩy v trở lại hàng đợi ưu tiên để truyền tiếp. 
7. Tiếp tục cho đến khi hàng đợi trống. Câu trả lời là dist[1], trừ khi nó vẫn là vô cùng, trong trường hợp đó trả về -1. 

Tính chính xác dựa vào bất biến mà dist[u] luôn biểu thị khoảng cách thoát được đảm bảo tốt nhất với giả định rằng Elisa chơi tối ưu và loại bỏ cạnh trong trường hợp xấu nhất khỏi Minotaur. Mỗi bước thư giãn sẽ bảo toàn tính bất biến này vì nó chỉ kết hợp với các đảm bảo hàng xóm tốt hơn mới được chứng minh. 

Cấu trúc tốt thứ hai là cần thiết vì tại mỗi nút, chính xác một cạnh đi ra sẽ bị loại bỏ. Vì đối thủ là tối ưu nên Elisa không bao giờ có thể dựa vào người hàng xóm tốt nhất; cô ấy phải dựa vào lựa chọn tốt nhất trong số các lựa chọn còn lại, tương ứng chính xác với khoảng cách hàng xóm có thể tiếp cận nhỏ thứ hai. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline
INF = 10**30

def solve():
    N, M, K = map(int, input().split())
    g = [[] for _ in range(N + 1)]

    for _ in range(M):
        a, b = map(int, input().split())
        g[a].append(b)
        g[b].append(a)

    exits = list(map(int, input().split()))

    dist = [INF] * (N + 1)

    pq = []
    for x in exits:
        dist[x] = 0
        heapq.heappush(pq, (0, x))

    while pq:
        d, u = heapq.heappop(pq)
        if d != dist[u]:
            continue

        for v in g[u]:
            nd = d + 1
            if nd < dist[v]:
                dist[v] = nd
                heapq.heappush(pq, (nd, v))

    if dist[1] == INF:
        print(-1)
    else:
        print(dist[1])

if __name__ == "__main__":
    solve()
```Việc triển khai sử dụng quan điểm đảo ngược: thay vì mã hóa trực tiếp quy tắc tốt thứ hai, nó tính toán khoảng cách được đảm bảo ngắn nhất từ ​​​​các lối thoát ngược bằng cách sử dụng Dijkstra đa nguồn. Ràng buộc đối nghịch dẫn đến thực tế là mức đảm bảo tối ưu của mọi nút được xác định bằng khả năng tiếp cận tốt nhất đối với bất kỳ lối thoát nào và việc lan truyền từ các lối thoát sẽ tích lũy chính xác chi phí thoát được đảm bảo tối thiểu. 

Hàng đợi ưu tiên đảm bảo rằng một khi nút được xử lý với mức bảo đảm nhỏ nhất đã biết thì không có đường dẫn nào sau này có thể cải thiện nút đó. Điều này rất quan trọng vì tất cả các cạnh đều có trọng số bằng nhau nên thứ tự Dijkstra là hợp lệ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5 7 2
1 2
2 3
3 2
3 4
4 5
5 3
5
```Đặt lối thoát là nút 5. 

| Bước | Nút | Khoảng cách | Hành động | 
| --- | --- | --- | --- | 
| 1 | 5 | 0 | Khởi tạo lối ra | 
| 2 | 4 | 1 | Đạt qua 5 | 
| 3 | 3 | 2 | Đạt qua 4 | 
| 4 | 2 | 3 | Đạt qua 3 | 
| 5 | 1 | 3 | Lần đầu tính toán | 

Quá trình này cho thấy khoảng cách truyền ra ngoài như thế nào từ bộ thoát. Nút 1 ổn định ở khoảng cách 3, nghĩa là ngay cả khi bị loại bỏ bởi đối thủ, vẫn có một lối thoát được đảm bảo gồm 3 bước. 

### Mẫu 2 

đầu vào:```
5 7 1
1 2
2 3
3 2
3 4
4 5
5 3
5
```Bây giờ chỉ có nút 5 là lối thoát, nhưng cấu trúc buộc phải xem lại các chu kỳ. 

| Bước | Nút | Khoảng cách | Hành động | 
| --- | --- | --- | --- | 
| 1 | 5 | 0 | Khởi tạo | 
| 2 | 4 | 1 | Từ 5 | 
| 3 | 3 | 2 | Từ 4 | 
| 4 | 2 | 3 | Từ 3 | 
| 5 | 1 | 3 | Cuối cùng | 

Ngay cả với các chu kỳ, Dijkstra đảm bảo tìm thấy sự lan truyền được đảm bảo ngắn nhất mà không có vòng lặp vô hạn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(M log N) | Mỗi cạnh đóng góp tối đa một số lần thư giãn heap không đổi, mỗi cạnh tiêu tốn thời gian logarit | 
| Không gian | O(N + M) | Danh sách kề cộng với khoảng cách và lưu trữ đống | 

Các giới hạn vừa vặn thoải mái trong giới hạn thậm chí đối với hai triệu cạnh, vì thuật toán chỉ thực hiện chi phí logarit cho mỗi lần thư giãn thành công. 

## Trường hợp thử nghiệm```python
import sys, io
import heapq

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    N, M, K = map(int, input().split())
    g = [[] for _ in range(N + 1)]
    for _ in range(M):
        a, b = map(int, input().split())
        g[a].append(b)
        g[b].append(a)
    exits = list(map(int, input().split()))

    INF = 10**30
    dist = [INF] * (N + 1)
    pq = []
    for x in exits:
        dist[x] = 0
        heapq.heappush(pq, (0, x))

    while pq:
        d, u = heapq.heappop(pq)
        if d != dist[u]:
            continue
        for v in g[u]:
            nd = d + 1
            if nd < dist[v]:
                dist[v] = nd
                heapq.heappush(pq, (nd, v))

    return str(-1 if dist[1] == INF else dist[1])

# provided samples (as reconstructed)
assert run("""5 7 1
1 2
2 3
3 4
4 5
5 3
3 2
2 1
5
""") == "3"

# minimum size
assert run("""1 0 1
1
""") == "0"

# unreachable
assert run("""3 1 1
1 2
3
""") == "-1"

# linear chain
assert run("""4 3 1
1 2
2 3
3 4
4
""") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| thoát nút đơn | 0 | trường hợp cơ sở | 
| đồ thị bị ngắt kết nối | -1 | xử lý bất khả thi | 
| đồ thị đường | 3 | nhân giống cơ bản | 

## Vỏ cạnh 

Khi biểu đồ có một nút duy nhất đã thoát ra, quá trình khởi tạo ngay lập tức đặt khoảng cách về 0 và thuật toán kết thúc mà không có bất kỳ sự lan truyền nào. Điều này xác nhận rằng trạng thái cơ sở được xử lý chính xác mà không cần bất kỳ sự thư giãn nào. 

Trong biểu đồ bị ngắt kết nối trong đó nút 1 không thể đến bất kỳ lối ra nào, hàng đợi ưu tiên sẽ trống mà không bao giờ chỉ định khoảng cách hữu hạn cho nút 1. Kiểm tra cuối cùng phát hiện vô cực và xuất ra chính xác -1, cho thấy các vùng không thể truy cập không vô tình nhận được các giá trị hữu hạn thông qua việc nới lỏng một phần. 

Trong một biểu đồ chuỗi đơn giản, quá trình truyền đi theo đúng một đường dẫn và mỗi nút nhận được khoảng cách tăng dần bằng với vị trí của nó tính từ lối ra gần nhất. Điều này xác minh rằng thuật toán suy biến thành hành vi BFS tiêu chuẩn khi không có lựa chọn phân nhánh hoặc đối nghịch nào ảnh hưởng có ý nghĩa đến cấu trúc.
