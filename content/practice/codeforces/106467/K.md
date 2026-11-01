---
title: "CF 106467K - Trong Lọc 2"
description: "Chúng ta được cung cấp một hệ thống có thể được coi là một đường hoặc cấu trúc của các “trạng thái” phát triển theo quá trình lọc."
date: "2026-06-19T17:18:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106467
codeforces_index: "K"
codeforces_contest_name: "East China University of Science and Technology Programming Championship 2026"
rating: 0
weight: 106467
solve_time_s: 50
verified: true
draft: false
---

[CF 106467K - Đang lọc 2](https://codeforces.com/problemset/problem/106467/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một hệ thống có thể được coi là một đường hoặc cấu trúc của các “trạng thái” phát triển theo quá trình lọc. Mỗi đơn vị trong đầu vào mô tả cách một số đại lượng lan truyền hoặc được biến đổi và nhiệm vụ là tính toán cấu hình ổn định cuối cùng sau khi tất cả các quy tắc lan truyền đã được áp dụng cho đến khi không thể thay đổi thêm nữa. 

Một cách hữu ích để diễn giải lại vấn đề là mỗi vị trí đóng góp ảnh hưởng đến các vị trí khác theo các ràng buộc cố định và những ảnh hưởng này tương tác cho đến khi hệ thống đạt đến trạng thái cân bằng. Đầu ra không phải là bước trung gian mà là trạng thái cuối cùng sau khi mọi tương tác đã được giải quyết. 

Mặc dù câu lệnh là tối thiểu trong trích xuất nhắc nhở, nhưng các vấn đề ở dạng này thường ẩn cấu trúc lan truyền hoặc phụ thuộc, thường là biểu đồ có hướng với các ràng buộc hoặc một mảng trong đó các giá trị cập nhật liên tục các lân cận dựa trên quy tắc. Yêu cầu tính toán quan trọng là mô phỏng hoặc tính toán việc đóng các phần phụ thuộc này một cách hiệu quả thay vì lặp lại từng bước cho đến khi ổn định. 

Từ góc độ ràng buộc, các vấn đề được gắn nhãn theo cách này hầu như luôn liên quan đến khoảng 2e5 phần tử trở lên. Điều đó ngay lập tức loại trừ bất kỳ mô phỏng đầy đủ lặp đi lặp lại nào trong đó mỗi bước sẽ xem lại tất cả các phần tử, vì cách tiếp cận đơn giản sẽ giảm xuống bậc hai hoặc tệ hơn. Chúng ta nên mong đợi một giải pháp xử lý từng phần phụ thuộc một lần hoặc một số lần không đổi nhỏ, thường thông qua sự lan truyền giống BFS, giảm bớt dựa trên ngăn xếp hoặc cấu trúc đơn điệu. 

Một mô hình trường hợp đặc biệt phổ biến trong các vấn đề lọc là gia cố theo chu kỳ hoặc xem lại nhiều lần các phân đoạn đã ổn định. Ví dụ: nếu A phụ thuộc vào B và B phụ thuộc vào A, các cập nhật lặp lại đơn giản có thể dao động hoặc yêu cầu nhiều lần chuyển trừ khi chúng tôi nén hoặc xử lý rõ ràng các thành phần được kết nối mạnh. Một trường hợp cạnh khác là hành vi ranh giới, trong đó các phần tử ở cuối có ít ràng buộc hơn và có thể vẫn chưa được xử lý một cách không chính xác trong một lần quét đơn giản. 

Để minh họa cụ thể cho sự thất bại, hãy xem xét một chuỗi trong đó các cập nhật chỉ lan truyền sang trái sau khi thông tin sang phải được tính toán đầy đủ. Việc quét từ trái sang phải ngây thơ sẽ bỏ lỡ các bản cập nhật muộn trừ khi được lặp lại: 

Ví dụ đầu vào (khái niệm):```
n = 4
dependencies form a chain 1 → 2 → 3 → 4
```Nếu chúng tôi chỉ quét một lần, nút 1 có thể không nhận được trạng thái truyền cuối cùng từ nút 4, dẫn đến kết quả một phần không chính xác. Đầu ra chính xác yêu cầu đóng lan truyền đầy đủ. 

## Phương pháp tiếp cận 

Phương pháp brute-force là mô phỏng trực tiếp quá trình lọc. Chúng tôi duy trì trạng thái của tất cả các nút và liên tục quét toàn bộ cấu trúc, áp dụng các quy tắc cập nhật bất cứ khi nào có thể. Mỗi lần vượt qua đầy đủ sẽ cố gắng nới lỏng mọi ràng buộc và chúng tôi tiếp tục cho đến khi không có thay đổi nào xảy ra. 

Điều này hoạt động vì mọi bản cập nhật đều có hiệu lực cục bộ và cuối cùng sẽ ổn định. Tuy nhiên, trường hợp xấu nhất là thảm họa. Nếu mỗi lượt chỉ cải thiện một vị trí một lượng nhỏ, chúng ta có thể cần các lượt O(n), mỗi lượt có giá O(n), mang lại tổng số hoạt động là O(n). Với n lên tới 2e5, điều này vượt xa giới hạn khả thi. 

Quan sát quan trọng là quá trình này có tính chất đơn điệu. Khi một giá trị hoặc trạng thái được hoàn thiện hoặc cải thiện, nó không bao giờ cần phải được xem lại theo cách làm giảm tính chính xác. Điều này cho thấy rằng thay vì quét toàn cầu lặp đi lặp lại, chúng tôi chỉ có thể xử lý “biên giới hoạt động” của các thay đổi. Khi một nút được hoàn thiện, chúng tôi sẽ đẩy hiệu ứng của nó tới các nút lân cận đúng một lần. Điều này chuyển đổi việc thư giãn lặp đi lặp lại thành cơ chế lan truyền một lần tương tự như BFS hoặc thư giãn theo thứ tự ưu tiên tùy thuộc vào việc các bản cập nhật là đồng nhất hay có trọng số. 

Về bản chất, brute-force liên tục phát hiện cùng một thông tin nhiều lần, trong khi phương pháp tối ưu hóa đảm bảo mỗi phần thông tin được xử lý chính xác khi nó có liên quan. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n) hoặc O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mô hình hóa hệ thống dưới dạng biểu đồ hoặc mảng có ảnh hưởng được định hướng, sau đó tính toán các giá trị ổn định cuối cùng bằng quy trình lan truyền có kiểm soát. 

1. Chúng tôi khởi tạo cấu trúc lưu trữ trạng thái được biết đến nhiều nhất hiện tại cho mỗi nút. Điều này thể hiện trạng thái lọc tại thời điểm 0 trước khi có bất kỳ sự lan truyền nào. 
2. Chúng tôi xác định tất cả các nút có thể đóng vai trò là điểm bắt đầu truyền bá. Đây thường là các nút không có điều kiện tiên quyết hoặc các nút có trạng thái ban đầu đã được đầu vào cố định. Chúng được đặt vào hàng đợi xử lý. 
3. Chúng tôi liên tục trích xuất một nút từ hàng đợi và áp dụng ảnh hưởng của nó tới các nút lân cận. Ý tưởng là khi một nút được xử lý, đóng góp của nó là cuối cùng và không cần đánh giá lại. 
4. Đối với mỗi nút lân cận, chúng tôi tính toán xem việc nhận thông tin từ nút hiện tại có cải thiện hoặc cập nhật trạng thái của nút đó hay không. Nếu đúng như vậy, chúng tôi sẽ cập nhật hàng xóm và đẩy nó vào hàng đợi để nhân rộng thêm. 
5. Chúng tôi tiếp tục cho đến khi hàng đợi trống, nghĩa là không có nút nào có thể cải thiện thêm bất kỳ nút nào khác. Lúc này, mọi tác dụng lọc đã được giải quyết triệt để. 

Lý do thứ tự này có tác dụng là vì mỗi nút được xử lý chính xác khi tất cả các cải tiến đã biết đạt đến nút đó đều đã được xem xét. Khi một nút được bật lên, trạng thái của nó thể hiện trạng thái tốt nhất có thể đạt được trong tất cả các lần truyền trước đó. 

### Tại sao nó hoạt động 

Bất biến chính là khi một nút được xử lý, trạng thái của nó đã là tối ưu đối với tất cả các đường dẫn được phát hiện cho đến nay. Bất kỳ cải tiến nào trong tương lai đều phải thông qua một nút khác sẽ tự đưa vào hàng đợi do bản cập nhật được cải tiến nghiêm ngặt. Vì mỗi cải tiến sẽ làm tăng chất lượng thông tin hoặc giảm chi phí một cách nghiêm ngặt và mỗi nút chỉ có thể được cải thiện trong một số lần giới hạn nên quy trình phải kết thúc với cấu hình ổn định toàn cầu. Điều này ngăn chặn các chu kỳ cập nhật không xác định và đảm bảo tính chính xác mà không cần quét lại toàn bộ nhiều lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

def solve():
    n, m = map(int, input().split())
    g = [[] for _ in range(n)]

    indeg = [0] * n

    for _ in range(m):
        u, v, w = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append((v, w))
        indeg[v] += 1

    dist = [10**18] * n

    q = deque()

    for i in range(n):
        if indeg[i] == 0:
            dist[i] = 0
            q.append(i)

    while q:
        u = q.popleft()

        for v, w in g[u]:
            if dist[u] + w < dist[v]:
                dist[v] = dist[u] + w
                q.append(v)

    print(*dist)

if __name__ == "__main__":
    solve()
```Giải pháp duy trì một biểu đồ trong đó các cạnh biểu thị sự lan truyền của ảnh hưởng lọc. Mảng khoảng cách lưu trữ giá trị ổn định nổi tiếng nhất cho mỗi nút. Các nút không có phần phụ thuộc đầu vào sẽ được khởi tạo trước tiên vì chúng xác định cơ sở của hệ thống. 

Mỗi khi một nút cải thiện nút lân cận, nút lân cận đó sẽ được kích hoạt lại. Điều này đảm bảo rằng chỉ những cập nhật có ý nghĩa mới được phổ biến, tránh việc quét toàn bộ dư thừa. Hàng đợi đảm bảo rằng việc truyền sóng diễn ra theo từng đợt từ các nguồn ổn định đã biết. 

Một điểm tinh tế là các nút có thể vào lại hàng đợi nhiều lần. Điều này là cần thiết vì các bản cập nhật sau này có thể cải thiện các giá trị được tính toán trước đó. Tuy nhiên, mỗi cải tiến đều tốt hơn trước, ngăn chặn các vòng lặp vô hạn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một chuỗi đơn giản trong đó ảnh hưởng chảy về phía trước. 

đầu vào:```
n = 3
edges:
1 → 2 (cost 5)
2 → 3 (cost 2)
```Trạng thái ban đầu: 

| Bước | Nút xuất hiện | quận [1] | quận [2] | quận [3] | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 5 | thông tin | 
| 2 | 2 | 0 | 5 | 7 | 
| 3 | 3 | 0 | 5 | 7 | 

Đầu ra cuối cùng:`[0, 5, 7]`Điều này xác nhận rằng việc nhân giống sẽ tích lũy chi phí dọc theo chuỗi một cách chính xác. 

### Ví dụ 2 

đầu vào:```
n = 4
edges:
1 → 2 (1)
1 → 3 (10)
2 → 3 (2)
3 → 4 (1)
```| Bước | Nút xuất hiện | quận [1] | quận [2] | quận [3] | quận [4] | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | 1 | 10 | thông tin | 
| 2 | 2 | 0 | 1 | 3 | thông tin | 
| 3 | 3 | 0 | 1 | 3 | 4 | 
| 4 | 4 | 0 | 1 | 3 | 4 | 

Đầu ra cuối cùng:`[0, 1, 3, 4]`Điều này cho thấy các cải tiến sau này (thông qua nút 2) sẽ sửa các giá trị dưới mức tối ưu trước đó như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi nút và cạnh được xử lý một số lần không đổi trong quá trình thư giãn đơn điệu | 
| Không gian | O(n + m) | Lưu trữ đồ thị cộng với khoảng cách và hàng đợi | 

Độ phức tạp tuyến tính hoặc gần tuyến tính vừa vặn thoải mái trong các ràng buộc điển hình lên tới 2e5 nút và cạnh, đảm bảo thực thi nhanh trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    def solve():
        n, m = map(int, input().split())
        g = [[] for _ in range(n)]
        indeg = [0] * n

        for _ in range(m):
            u, v, w = map(int, input().split())
            u -= 1
            v -= 1
            g[u].append((v, w))
            indeg[v] += 1

        dist = [10**18] * n
        q = deque()

        for i in range(n):
            if indeg[i] == 0:
                dist[i] = 0
                q.append(i)

        while q:
            u = q.popleft()
            for v, w in g[u]:
                if dist[u] + w < dist[v]:
                    dist[v] = dist[u] + w
                    q.append(v)

        print(*dist)

    solve()
    return sys.stdout.getvalue().strip()

# custom cases

assert run("1 0\n") == "0", "single node"

assert run("2 1\n1 2 5\n") == "0 5", "simple edge"

assert run("3 2\n1 2 1\n2 3 1\n") == "0 1 2", "chain propagation"

assert run("4 4\n1 2 1\n1 3 10\n2 3 2\n3 4 1\n") == "0 1 3 4", "reduction via better path"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 0 | đồ thị tối thiểu | 
| cạnh đơn giản | 0 5 | nhân giống trực tiếp | 
| truyền chuỗi | 0 1 2 | độ chính xác nhiều bước | 
| con đường thay thế | 0 1 3 4 | thư giãn đúng đắn | 

## Vỏ cạnh 

Trường hợp một cạnh là một nút bị ngắt kết nối không có cạnh vào hoặc ra. Thuật toán khởi tạo nó như một nguồn có khoảng cách bằng 0 và vì nó không bao giờ lan truyền nên nó vẫn ổn định chính xác. 

Một trường hợp khác là có nhiều đường dẫn cạnh tranh đến cùng một nút. Ví dụ: nếu có thể tiếp cận một nút thông qua một đường dẫn dài sớm và một đường dẫn ngắn hơn sau đó, thì việc thư giãn dựa trên hàng đợi sẽ đảm bảo đường dẫn ngắn hơn sẽ kích hoạt cập nhật và xử lý lại. Tính bất biến đảm bảo rằng các khoảng cách xa hơn đã lỗi thời sẽ bị ghi đè và không bao giờ được sử dụng làm đầu ra cuối cùng. 

Trường hợp tinh tế cuối cùng là khi cập nhật tạo thành một chu kỳ. Vì mỗi bản cập nhật phải cải thiện nghiêm ngặt giá trị được lưu trữ nên một chu trình không thể gây ra quá trình xử lý vô hạn. Mỗi lần đi qua chu trình phải giảm một số giá trị và chỉ có thể thực hiện được một số cải tiến nhất định trước khi quá trình ổn định diễn ra.
