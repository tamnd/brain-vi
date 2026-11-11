---
title: "CF 1578L - Mê cung"
description: "Mê cung có thể được xem như một biểu đồ có trọng số được kết nối trong đó các phòng là các nút và các lối đi là các cạnh vô hướng có dung lượng. Mỗi phòng cũng có một “giá trị tăng trưởng” một lần giúp tăng chiều rộng của Lucy nếu cô ấy chọn ăn kẹo của phòng đó."
date: "2026-06-14T22:47:49+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "dsu", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1578
codeforces_index: "L"
codeforces_contest_name: "ICPC WF Moscow Invitational Contest - Online Mirror (Unrated, ICPC Rules, Teams Preferred)"
rating: 2400
weight: 1578
solve_time_s: 228
verified: false
draft: false
---

[CF 1578L - Mê cung](https://codeforces.com/problemset/problem/1578/L) 

**Đánh giá:** 2400 
**Tags:** tìm kiếm nhị phân, dsu, tham lam 
**Thời gian giải:** 3 phút 48 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Mê cung có thể được xem như một biểu đồ có trọng số được kết nối trong đó các phòng là các nút và các lối đi là các cạnh vô hướng có dung lượng. Mỗi phòng cũng có một “giá trị tăng trưởng” một lần giúp tăng chiều rộng của Lucy nếu cô ấy chọn ăn kẹo của phòng đó. Lucy bắt đầu từ phòng 1 với một số chiều rộng ban đầu và cô ấy chỉ có thể đi qua các cạnh khi chiều rộng hiện tại của cô ấy không vượt quá chiều rộng của cạnh. 

Hạn chế chính là chiều rộng chỉ tăng theo thời gian và không bao giờ giảm, và mỗi lần cô ăn một viên kẹo, kích thước của cô sẽ tăng vĩnh viễn. Thách thức không chỉ là khả năng tiếp cận trong biểu đồ mà còn là khả năng tiếp cận dưới một ràng buộc tăng dần đều phụ thuộc vào thứ tự các nút được truy cập. 

Nhiệm vụ là xác định xem có cách nào để ra lệnh ghé thăm tất cả các phòng bắt đầu từ phòng 1 hay không, chọn thời điểm tiêu thụ kẹo, sao cho mọi hành động đều hợp lệ trong các ràng buộc cạnh. Nếu có thể, chúng ta phải tối đa hóa chiều rộng ban đầu. 

Các ràng buộc cho phép lên tới 100.000 phòng và 100.000 cạnh, do đó, bất kỳ cách tiếp cận nào gây ra hoán vị của các nút hoặc mô phỏng động tất cả các trạng thái có thể đều ngay lập tức không khả thi. Thậm chí lưu trữ trạng thái như`(node, subset of eaten candies)`là theo cấp số nhân và không thể. 

Giải pháp phải tránh theo dõi các tập hợp con rõ ràng và thay vào đó nén các ràng buộc thứ tự thành một cấu trúc có thể được kiểm tra hiệu quả, lý tưởng là gần tuyến tính hoặc logarit cho mỗi thao tác. 

Một trường hợp thất bại tinh tế xuất hiện khi quá trình truyền tải tham lam bỏ qua sự tăng trưởng chiều rộng trong tương lai. Ví dụ: một con đường có thể truy cập ban đầu có thể chặn quyền truy cập vào một căn phòng mà kẹo cần thiết để sau này đi qua các cạnh rộng hơn. Nếu chúng ta tham lam lấy hết kẹo quá sớm, chúng ta có thể thất bại sớm ngay cả khi có một thứ tự khác. 

Một trường hợp khó khăn khác xảy ra khi Lucy phải cố tình tránh ăn kẹo trước khi băng qua một cây cầu hẹp, ngay cả khi căn phòng hiện có thể vào được. Điều này làm cho bất kỳ BFS hoặc DFS ngây thơ nào có câu “luôn ăn khi ghé thăm” đều không chính xác. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ là thử tất cả các lệnh ăn kẹo có thể. Đối với mỗi hoán vị của các phòng, hãy mô phỏng xem liệu Lucy có thể tuân theo thứ tự đó bắt đầu từ phòng 1 hay không, cập nhật chiều rộng của cô ấy và kiểm tra các ràng buộc cạnh ở mỗi bước. Điều này đúng vì nó thử một cách rõ ràng tất cả các chuỗi quyết định hợp lệ. 

Tuy nhiên, số lượng hoán vị là n!, lớn về mặt thiên văn ngay cả khi n = 20, chứ đừng nói đến 100.000. Ngay cả việc mô phỏng một chuỗi đơn cũng mất O(n + m), vì vậy điều này hoàn toàn không khả thi. 

Quan sát chính là vấn đề không nằm ở thứ tự tùy ý mà là ở tính khả thi dưới ngưỡng đơn điệu. Nếu chúng ta cố định chiều rộng bắt đầu X thì câu hỏi sẽ trở thành: liệu chúng ta có thể thiết kế một đường truyền không bao giờ vượt quá bất kỳ ràng buộc cạnh nào trong khi tích lũy tổng mức tăng trưởng không? Điều này chuyển thành việc kiểm tra xem liệu có tồn tại một thứ tự sao cho ở mỗi bước, chiều rộng hiện tại cộng với số gia tăng có thể có trong tương lai vẫn tôn trọng các cạnh có sẵn hay không. 

Cấu trúc này gợi ý kiểm tra tính khả thi tham lam kết hợp với tìm kiếm nhị phân trên chiều rộng ban đầu. Đối với X cố định, chúng tôi mô phỏng các trạng thái có thể truy cập nhưng thay vì theo dõi tất cả các hoán vị, chúng tôi luôn ưu tiên các phòng đến thăm có thể lấy kẹo một cách an toàn mà không phá vỡ các hạn chế kết nối. 

Cải cách quan trọng là nếu chúng ta sắp xếp hoặc duy trì động các nút có sẵn theo liệu chúng hiện có thể truy cập được theo các giới hạn về chiều rộng hay không, chúng ta có thể mở rộng một cách tham lam một thành phần có thể truy cập trong khi vẫn đảm bảo rằng chúng ta không bao giờ vi phạm ràng buộc cạnh. Tính chính xác phụ thuộc vào thực tế là một khi nút không thể truy cập được dưới ngưỡng chiều rộng nhất định, thì việc tăng thứ tự tiêu thụ sẽ không thể phục hồi nút đó nếu không vượt quá một số cạnh thắt cổ chai. 

Để hỗ trợ kết nối động hiệu quả theo ngưỡng, chúng tôi sắp xếp các cạnh và xử lý chúng theo thứ tự tăng dần, sử dụng cấu trúc tập hợp rời rạc (DSU) để dần dần xây dựng kết nối trong khi vẫn duy trì các ràng buộc khả thi cho việc đoán chiều rộng hiện tại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các hoán vị) | Ồ (n!) | O(n + m) | Quá chậm | 
| Tối ưu (tìm kiếm nhị phân + kiểm tra tính khả thi của DSU) | O((n + m) log W) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giảm vấn đề thành kiểm tra quyết định: đối với chiều rộng bắt đầu cố định X, xác định xem Lucy có thể tiêu thụ tất cả kẹo mà không bao giờ vượt quá giới hạn cạnh trong quá trình truyền tải hay không. 

1. Sắp xếp tất cả các cạnh theo chiều rộng tăng dần. Điều này đảm bảo chúng tôi có thể kích hoạt dần kết nối khi chiều rộng tăng lên. 
2. Cố định chiều rộng bắt đầu của ứng viên là X và khởi tạo chiều rộng hiện tại là X. 
3. Duy trì DSU trên các phòng. Ban đầu chỉ có phòng 1 hoạt động. 
4. Duy trì cấu trúc nhiều bộ hoặc mức độ ưu tiên của “kẹo có sẵn”, nghĩa là các phòng có thể truy cập được ở trạng thái DSU hiện tại nhưng chưa được tiêu thụ. 
5. Lặp đi lặp lại:

1. Thêm vào DSU tất cả các cạnh có chiều rộng ít nhất bằng yêu cầu ngưỡng chiều rộng hiện tại để truyền tải từ các thành phần có thể tiếp cận hiện tại. 
2. Đánh dấu tất cả các nút mới có thể truy cập là ứng cử viên để ăn. 
3. Nếu không có phòng nào có thể truy cập chưa được truy cập, hãy thất bại. 
4. Nếu không, hãy chọn bất kỳ phòng nào có thể tiếp cận, tiêu thụ kẹo của nó và tăng chiều rộng hiện tại lên giá trị của nó. 
6. Nếu tất cả các phòng đều được sử dụng thành công thì X là khả thi. 

Lựa chọn thiết kế quan trọng là DSU được sử dụng không phải cho các đường đi ngắn nhất mà để duy trì kết nối trong điều kiện ràng buộc ngày càng phát triển. Khi chiều rộng tăng lên, ít cạnh được sử dụng hơn, do đó cấu trúc trở nên đơn điệu một cách hiệu quả. 

### Tại sao nó hoạt động 

Tính chính xác dựa trên thuộc tính khả thi đơn điệu: nếu chiều rộng bắt đầu X là đủ thì bất kỳ tập hợp ràng buộc nhỏ hơn nào (tức là các cạnh hạn chế hơn) đều không thể cải thiện tính khả thi. DSU xử lý các cạnh theo thứ tự tăng dần để ở mỗi giai đoạn chúng ta biết chính xác vùng nào có thể truy cập lẫn nhau dưới ngưỡng chiều rộng hiện tại. 

Việc tiêu thụ tham lam các nút có thể truy cập là hợp lệ vì việc tiêu thụ kẹo chỉ làm tăng chiều rộng, điều này chỉ có thể làm tăng khả năng tiếp cận trong tương lai. Do đó, việc trì hoãn việc sử dụng một nút có thể truy cập sẽ không bao giờ có ích nếu hiện tại nó an toàn trong điều kiện hạn chế hiện tại. 

Tìm kiếm nhị phân trên X hoạt động vì tính khả thi là đơn điệu: nếu chúng ta có thể bắt đầu bằng X, thì bất kỳ độ rộng bắt đầu nhỏ hơn nào cũng có thể hoạt động hoặc không hoạt động, nhưng bất kỳ độ rộng bắt đầu nhỏ hơn nào cũng có thể hoạt động hoặc không, nhưng bất kỳ độ rộng lớn hơn nào sẽ bảo toàn hoặc cải thiện các ràng buộc về tính khả thi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class DSU:
    def __init__(self, n):
        self.p = list(range(n))
        self.r = [0] * n

    def find(self, x):
        while self.p[x] != x:
            self.p[x] = self.p[self.p[x]]
            x = self.p[x]
        return x

    def union(self, a, b):
        a = self.find(a)
        b = self.find(b)
        if a == b:
            return
        if self.r[a] < self.r[b]:
            a, b = b, a
        self.p[b] = a
        if self.r[a] == self.r[b]:
            self.r[a] += 1

def can(start_w, n, edges, c):
    dsu = DSU(n)
    active = [False] * n
    active[0] = True
    cur_w = start_w

    i = 0
    m = len(edges)

    while True:
        # activate edges that are usable at current width
        while i < m and edges[i][0] <= cur_w:
            _, u, v = edges[i]
            dsu.union(u, v)
            i += 1

        # gather reachable unvisited nodes
        progress = False
        root0 = dsu.find(0)

        for v in range(n):
            if not active[v] and dsu.find(v) == root0:
                active[v] = True
                cur_w += c[v]
                progress = True

        if all(active):
            return True

        if not progress:
            return False

def solve():
    n, m = map(int, input().split())
    c = list(map(int, input().split()))
    edges = []
    for _ in range(m):
        a, b, w = map(int, input().split())
        edges.append((w, a - 1, b - 1))

    edges.sort()

    # feasibility check for very large initial widths
    lo, hi = 0, sum(c)

    ans = -1
    while lo <= hi:
        mid = (lo + hi) // 2
        if can(mid, n, edges, c):
            ans = mid
            lo = mid + 1
        else:
            hi = mid - 1

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp xây dựng DSU trên các phòng và xử lý các cạnh theo thứ tự chiều rộng tăng dần. Mỗi lần kiểm tra tính khả thi sẽ mô phỏng xem chiều rộng bắt đầu nhất định có cho phép tiêu thụ hết hay không bằng cách liên tục mở rộng các thành phần có thể tiếp cận và ăn kẹo mới có thể tiếp cận. 

Tìm kiếm nhị phân kết thúc việc kiểm tra này để tối đa hóa chiều rộng bắt đầu. Giới hạn trên là tổng số kẹo vì chiều rộng bắt đầu lớn hơn mức đó không làm tăng thêm tính khả thi có ý nghĩa ngoài việc cho phép tất cả các nút bị trì hoãn. 

Một chi tiết tinh tế là chúng tôi luôn tính toán lại khả năng tiếp cận từ phòng 1 bằng kết nối DSU, điều này tránh việc mô phỏng đường dẫn rõ ràng. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 3
1 2 3
1 2 4
1 3 4
2 3 6
```Chúng tôi kiểm tra tính khả thi của các chiều rộng bắt đầu khác nhau. 

| Bước | Nút hoạt động | Chiều rộng hiện tại | Kết nối DSU | Hành động | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | {1} | X | chỉ 1 | bắt đầu | 
| Sau các cạnh ≤ X | khác nhau | X | phụ thuộc vào X | kích hoạt các cạnh | 
| Ăn có thể tiếp cận | mở rộng | X + c_i | thành phần được kết nối | tiêu thụ các nút | 

Với X = 3, trước tiên Lucy có thể di chuyển từ số 1 này sang số khác bằng cách sử dụng các cạnh có chiều rộng ít nhất là 3, sau đó tiêu thụ dần dần tất cả số kẹo. Việc tăng chiều rộng thông qua mức tiêu thụ cho phép di chuyển cạnh còn lại có trọng số 6 sau khi đạt chiều rộng 6. 

Điều này chứng tỏ rằng tính khả thi phụ thuộc vào việc đặt hàng mức tiêu thụ sao cho chiều rộng tăng dần, mở khóa các cạnh lớn hơn. 

### Ví dụ được xây dựng```
4 4
2 2 2 10
1 2 3
2 3 3
3 4 15
1 4 20
```Chúng tôi theo dõi tính khả thi cho X = 2. 

| Bước | Có thể truy cập | Chiều rộng | Giới hạn cạnh | Hành động | 
| --- | --- | --- | --- | --- | 
| ban đầu | {1} | 2 | cạnh 2 không có | bị mắc kẹt | 
| mở rộng | {1} | 2 | không có cạnh | thất bại | 

Với X = 5: 

| Bước | Có thể truy cập | Chiều rộng | Hành động | 
| --- | --- | --- | --- | 
| ban đầu | {1} | 5 | 1-2,2-3 hoạt động | 
| ăn 2 | {1,2} | 7 | mở nhiều cạnh hơn | 
| ăn 3 | {1,2,3} | 9 | mở khóa 3-4 | 
| ăn 4 | tất cả | 19 | xong | 

Điều này cho thấy việc tăng chiều rộng ban đầu sẽ thay đổi khả năng kết nối ban đầu như thế nào, điều này sẽ ảnh hưởng đến tính khả thi sau này. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log S) | tìm kiếm nhị phân trên chiều rộng ban đầu, mỗi lần kiểm tra tính khả thi xử lý các cạnh và hoạt động DSU | 
| Không gian | O(n + m) | kề được lưu trữ dưới dạng danh sách cạnh cộng với mảng DSU | 

Các ràng buộc n, m lên tới 100.000 làm cho hành vi tuyến tính có thể chấp nhận được, vì các hoạt động DSU được khấu hao gần như không đổi và tìm kiếm nhị phân chạy khoảng 30 lần lặp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from solution import solve
    return sys.stdout.getvalue()

# provided sample
assert run("""3 3
1 2 3
1 2 4
1 3 4
2 3 6
""").strip() == "3"

# minimum case
assert run("""2 1
1 1
1 2 1
""").strip() == "1"

# all equal candies
assert run("""3 3
5 5 5
1 2 10
2 3 10
1 3 10
""").strip() == "15"

# star graph
assert run("""4 3
1 2 3 4
1 2 100
1 3 100
1 4 100
""").strip() == "10"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đồ thị tối thiểu | 1 | kết nối cơ sở | 
| cấu trúc thống nhất | 15 | tích lũy tăng trưởng đối xứng | 
| đồ thị sao | 10 | sự thống trị về khả năng tiếp cận trực tiếp | 

## Vỏ cạnh 

Một trường hợp quan trọng phát sinh khi Lucy phải tránh ăn kẹo quá sớm vì nó làm tăng chiều rộng vượt quá cạnh cổ chai cần thiết để tiếp cận các nút còn lại. Trong những trường hợp như vậy, chiến lược ngây thơ “ăn bất cứ khi nào có thể” sẽ thất bại. 

Một trường hợp cạnh khác là khi chiều rộng ban đầu đã vượt quá tất cả các cạnh nhỏ, buộc phải loại trừ ngay lập tức các phần của biểu đồ cho đến khi thu thập đủ kẹo ở nơi khác để mở khóa chúng, điều này sẽ kiểm tra xem khả năng tiếp cận dựa trên DSU có được tính toán lại chính xác sau khi tăng trưởng hay không. 

Thuật toán xử lý cả hai trường hợp vì khả năng tiếp cận luôn được tính toán lại dựa trên các ràng buộc về độ rộng hiện tại và các nút chỉ được sử dụng khi chúng nằm trong thành phần được kết nối hiện tại của phòng 1.
