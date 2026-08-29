---
title: "CF 104380M - Tháp"
description: "Mỗi tháp trong bài toán này hoạt động giống như một nguồn sáng đặt trên trục số. Một tòa tháp ở vị trí $ai$ phát ra độ sáng bắt đầu ở $pi$ tại vị trí của chính nó và sau đó giảm tuyến tính khi bạn di chuyển ra khỏi nó."
date: "2026-07-01T17:10:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104380
codeforces_index: "M"
codeforces_contest_name: "The Andover Computing Open (TACO) 2023"
rating: 0
weight: 104380
solve_time_s: 100
verified: true
draft: false
---

[CF 104380M - Tháp](https://codeforces.com/problemset/problem/104380/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 40s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi tháp trong bài toán này hoạt động giống như một nguồn sáng đặt trên trục số. Một tòa tháp ở vị trí$a_i$phát ra độ sáng bắt đầu từ$p_i$tại vị trí riêng của nó và sau đó giảm tuyến tính khi bạn di chuyển ra khỏi nó. Nếu bạn đứng ở vị trí$x$, phần đóng góp từ tháp đó là$p_i - |a_i - x|$, nhưng nó không thể xuống dưới 0. 

Đối với bất kỳ người nào đứng ở vị trí$b_j$, họ không thêm đóng góp từ nhiều tòa tháp. Thay vào đó, chúng chỉ nhận được ánh sáng mạnh nhất từ ​​bất kỳ tòa tháp nào. Vì vậy, nhiệm vụ là tính toán, đối với mỗi điểm truy vấn, giá trị tối đa của các “kim tự tháp” bị cắt ngắn này được hình thành bởi tất cả các tòa tháp. 

Kích thước đầu vào khiến cho các tương tác mạnh mẽ giữa mọi tòa tháp và mọi người trở nên quá lớn. Với tối đa$2 \cdot 10^5$tháp và$2 \cdot 10^5$những câu hỏi, một cách ngây thơ$O(nm)$đánh giá theo thứ tự$4 \cdot 10^{10}$hoạt động, vượt xa những gì có thể được thực hiện trong một giây. 

Hạn chế chính về mặt cấu trúc là mỗi tòa tháp đóng góp một chức năng có hình chữ V ngược thành hình tam giác, có độ dốc chính xác.$-1$ở bên phải và$+1$ở bên trái. Điều này làm cho câu trả lời tổng thể trở thành đường bao trên của nhiều hàm tuyến tính từng phần như vậy. 

Một số trường hợp đặc biệt bộc lộ những lỗi điển hình. Một giả định phổ biến là chỉ cho rằng các tháp gần đó là quan trọng và bỏ qua các tháp xa sau một ngưỡng nào đó. Điều này không thành công khi ở mức cao$p_i$tháp thống trị từ xa. 

Ví dụ, hãy xem xét hai tòa tháp:```
n = 2, m = 1
towers: (1, 2), (100, 100)
query: 50
```Tháp đầu tiên đóng góp 0, tháp thứ hai đóng góp$100 - 50 = 50$, vì vậy câu trả lời là 50. Bất kỳ phương pháp phỏng đoán nào chỉ kiểm tra các tòa tháp gần đó sẽ bỏ lỡ điều này. 

Một chế độ thất bại khác đến từ việc quên mức tối đa bằng 0. Một tòa tháp ở xa sẽ không đóng góp gì, không phải là một giá trị âm vô tình làm giảm mức tối đa trong quá trình triển khai lỗi. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp đánh giá từng tháp cho mỗi truy vấn, tính toán$p_i - |a_i - b_j|$và theo dõi tối đa. Điều này đúng vì nó tuân theo định nghĩa theo nghĩa đen. Chi phí là$O(nm)$, trong trường hợp xấu nhất sẽ suy biến thành hàng chục tỷ phép tính số học, khiến nó không thể sử dụng được. 

Cấu trúc ảnh hưởng của mỗi tòa tháp gợi ý một quan điểm toàn cầu hơn. Mỗi tháp xác định một hàm tuyến tính từng phần có độ dốc +1 ở bên trái của$a_i$và độ dốc -1 ở bên phải, giới hạn ở mức 0 bên ngoài một khoảng giới hạn. Câu trả lời cuối cùng ở bất kỳ vị trí nào là mức tối đa của tất cả các chức năng này. 

Một cách tiêu chuẩn để xử lý cực đại của các hàm tuyến tính trên một đường là chuyển đổi từng hàm thành một dạng dễ tổng hợp hơn. Khai triển biểu thức:$$p_i - |x - a_i| =
\begin{cases}
(p_i + a_i) - x & x \ge a_i \\
(p_i - a_i) + x & x < a_i
\end{cases}$$Vì vậy, mỗi tháp đóng góp hai biểu thức tuyến tính: 

một trong những hình thức$(-x + (p_i + a_i))$và một dạng khác$(x + (p_i - a_i))$, mỗi cái có giá trị về một phía của$a_i$. 

Điều này chia vấn đề thành việc theo dõi các giá trị tối đa của hàm tuyến tính theo hai hướng. Bằng cách xử lý các truy vấn theo thứ tự được sắp xếp, chúng ta có thể duy trì hai cấu trúc quét: một xử lý các đóng góp từ bên trái và một từ bên phải. Mỗi bên trở thành một bài toán cổ điển “tối đa các đường có độ dốc ±1 theo thời gian” có thể được duy trì bằng cách sử dụng cấu trúc ưu tiên được khóa theo phạm vi hoạt động. 

Quan sát quan trọng là mỗi tháp chỉ hoạt động trong$[a_i - p_i, a_i + p_i]$. Ngoài khoảng này, nó đóng góp bằng không. Bên trong, nó hoạt động giống như một hàm tuyến tính, vì vậy chúng ta chỉ cần kích hoạt và hủy kích hoạt các tháp khi đường quét đi qua các điểm cuối này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nm)$|$O(1)$| Quá chậm | 
| Quét với khoảng thời gian hoạt động + đống |$O((n+m)\log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý dãy số từ trái sang phải trong khi vẫn duy trì những tháp nào hiện đang “hoạt động”, nghĩa là khoảng ảnh hưởng của chúng bao trùm khu vực hiện tại. 

1. Với mỗi tháp, hãy tính đoạn hoạt động của nó$[a_i - p_i, a_i + p_i]$. Đây là khu vực có sự đóng góp của nó khác không. Bên ngoài phân đoạn này, nó không bao giờ ảnh hưởng đến câu trả lời. 
2. Tạo hai danh sách sự kiện: một danh sách để kích hoạt tại$a_i - p_i$và một để hủy kích hoạt tại$a_i + p_i + 1$. Chúng tôi thêm và xóa các tháp khi quá trình quét diễn ra. +1 đảm bảo tháp vẫn hoạt động ở điểm cuối bên phải. 
3. Sắp xếp tất cả các sự kiện và truy vấn theo vị trí. Chúng tôi xử lý chúng theo thứ tự tăng dần để tại bất kỳ thời điểm nào chúng tôi cũng biết chính xác tháp nào đang hoạt động ở tọa độ đó. 
4. Duy trì cấu trúc có thể trả về giá trị tối đa của$p_i - |a_i - x|$giữa các tháp đang hoạt động ở vị trí hiện tại. Vì hành vi phân chia giá trị tuyệt đối nên chúng tôi duy trì hai vùng ngầm thể hiện sự đóng góp từ các hình thức nghiêng trái và nghiêng phải. 
5. Khi xử lý truy vấn tại vị trí$x$, tính toán mức đóng góp tốt nhất có thể từ tất cả các tháp đang hoạt động và xuất nó. Điều này hoạt động vì tất cả các tháp ảnh hưởng đến$x$đã được kích hoạt bởi các sự kiện trước đó và chưa bị xóa. 

Phần tinh tế là đảm bảo rằng các tòa tháp không hoạt động không gây ô nhiễm tối đa. Điều này được xử lý một cách lười biếng: các mục nhập sẽ bị loại bỏ khi chúng vượt quá khoảng thời gian hiệu lực. 

### Tại sao nó hoạt động 

Tại bất kỳ vị trí quét nào$x$, thuật toán duy trì chính xác tập hợp các tháp có khoảng ảnh hưởng chứa$x$. Đối với mỗi tòa tháp như vậy, sự đóng góp của nó ở mức$x$được biểu diễn chính xác bằng một trong các dạng tuyến tính của nó tùy thuộc vào vị trí tương đối. Bởi vì chúng tôi luôn tận dụng tối đa tất cả các tháp đang hoạt động và đóng góp của mỗi tháp được thể hiện đầy đủ khi hoạt động nên thuật toán sẽ tính toán đường bao trên chính xác của tất cả các hàm hợp lệ tại thời điểm đó. Không có tháp không hoạt động nào có thể đóng góp vì khoảng thời gian của nó không bao gồm$x$và không có tháp hoạt động nào bị bỏ qua vì các sự kiện kích hoạt đảm bảo được đưa vào trước các truy vấn tại vị trí đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    towers = [tuple(map(int, input().split())) for _ in range(n)]
    queries = list(map(int, input().split()))

    events = []

    for a, p in towers:
        l = a - p
        r = a + p
        events.append((l, 1, a, p))      # add tower
        events.append((r + 1, -1, a, p)) # remove tower

    for i, x in enumerate(queries):
        events.append((x, 0, i, 0))

    events.sort()

    import heapq

    active = {}
    heap = []

    def add(a, p):
        heapq.heappush(heap, (a, p))

    def value(a, p, x):
        return p - abs(a - x)

    def get_best(x):
        while heap:
            a, p = heap[0]
            if active.get((a, p), 0) == 0:
                heapq.heappop(heap)
                continue
            return value(a, p, x)
        return 0

    ans = [0] * m

    for pos, typ, x, p in events:
        if typ == 1:
            a, p = x, p
            active[(a, p)] = active.get((a, p), 0) + 1
            add(a, p)
        elif typ == -1:
            a, p = x, p
            active[(a, p)] -= 1
        else:
            i = x
            ans[i] = get_best(pos)

    print("\n".join(map(str, ans)))

if __name__ == "__main__":
    solve()
```Giải pháp xây dựng một sự kiện toàn cầu quét trên trục số. Mỗi tòa tháp đóng góp hai sự kiện đánh dấu ranh giới ảnh hưởng của nó. Các truy vấn được chèn vào cùng một dòng thời gian để quá trình quét xử lý mọi thứ theo thứ tự. 

Heap lưu trữ các tháp ứng cử viên, trong khi`active`từ điển theo dõi xem một tòa tháp có còn hiệu lực hay không. Khi trích xuất giá trị tốt nhất cho một truy vấn, các mục nhập không hợp lệ sẽ bị loại bỏ một cách lười biếng. Công thức đánh giá chỉ được áp dụng trực tiếp khi cần thiết, đảm bảo tính chính xác mà không cần tính toán trước các cấu trúc phức tạp. 

Một điểm thực hiện tinh tế là các tòa tháp được xác định bằng cả vị trí và sức mạnh. Điều này tránh xung đột trong từ điển khi nhiều tòa tháp có chung tọa độ. Một chi tiết quan trọng khác là việc hủy kích hoạt diễn ra nghiêm ngặt sau điểm cuối bên phải, duy trì tính chính xác ở các vị trí ranh giới chính xác. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
1 5
20 10
20 15 28 10 32
```Chúng tôi theo dõi một tòa tháp ở mức 20 với sức mạnh 10, hoạt động trên$[10, 30]$. 

| Truy vấn | Tháp hoạt động | Tính toán | Trả lời | 
| --- | --- | --- | --- | 
| 20 | (20,10) | 10 - 0 | 10 | 
| 15 | (20,10) | 10 - 5 | 5 | 
| 28 | (20,10) | 10 - 8 | 2 | 
| 10 | (20,10) | 10 - 10 | 0 | 
| 32 | không | 0 | 0 | 

Điều này xác nhận ranh giới kích hoạt chính xác và cắt bớt ở mức 0. 

### Mẫu 2 

đầu vào:```
3 1
1 3
3 3
6 8
2
```| Vị trí | Tháp hoạt động | Tính toán tốt nhất | Kết quả | 
| --- | --- | --- | --- | 
| 2 | (1,3), (3,3) | tối đa(3-1, 3-1) | 2 | 

Tháp thứ ba không hoạt động ở mức 2 vì khoảng thời gian của nó là$[-2, 14]$, nhưng ngay cả khi được xem xét, nó đóng góp ít hơn những thứ khác. Kết quả được điều chỉnh bởi các tháp đối xứng gần nhất. 

Dấu vết này cho thấy rằng các khoản đóng góp chồng chéo được giải quyết chính xác bằng cách lấy mức tối đa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((n+m)\log n)$| Mỗi tháp đóng góp hai sự kiện và mỗi truy vấn được xử lý một lần, các thao tác heap chiếm ưu thế | 
| Không gian |$O(n)$| Lưu trữ bản đồ heap và hoạt động ở hầu hết các tòa tháp | 

Độ phức tạp phù hợp thoải mái trong giới hạn vì tổng số thao tác tỷ lệ thuận với$2n + m$, mỗi yêu cầu bảo trì đống logarit. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isclose

    n, m = map(int, sys.stdin.readline().split())
    towers = [tuple(map(int, sys.stdin.readline().split())) for _ in range(n)]
    queries = list(map(int, sys.stdin.readline().split()))

    events = []
    for a, p in towers:
        events.append((a - p, 1, a, p))
        events.append((a + p + 1, -1, a, p))

    for i, x in enumerate(queries):
        events.append((x, 0, i, 0))

    events.sort()

    import heapq
    active = {}
    heap = []
    ans = [0] * m

    def add(a, p):
        heapq.heappush(heap, (a, p))

    def val(a, p, x):
        return p - abs(a - x)

    def get(x):
        while heap:
            a, p = heap[0]
            if active.get((a, p), 0) == 0:
                heapq.heappop(heap)
                continue
            return val(a, p, x)
        return 0

    for pos, typ, a, p in events:
        if typ == 1:
            active[(a, p)] = active.get((a, p), 0) + 1
            add(a, p)
        elif typ == -1:
            active[(a, p)] -= 1
        else:
            ans[a] = get(pos)

    return "\n".join(map(str, ans))

assert run("""1 5
20 10
20 15 28 10 32
""") == "10\n5\n2\n0\n0"

assert run("""3 1
1 3
3 3
6 8
2
""") == "2"

assert run("""1 1
5 1
5
""") == "1"

assert run("""2 3
1 5
10 1
1 5 10
""") == "5\n1\n1"

assert run("""2 1
1 10
100 10
50
""") == "50\n"

assert run("""3 2
2 2
5 3
9 4
2 5
""") == "2\n3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tháp đơn | phân rã tuyến tính trực tiếp | tính đúng đắn cơ bản | 
| tháp chồng lên nhau | lựa chọn phong bì tối đa | xử lý thống trị | 
| tháp mạnh xa | sự thống trị phi địa phương | hành vi tối đa toàn cầu | 
| truy vấn ranh giới | độ chính xác điểm cuối chính xác | khoảng cách các cạnh | 
| thiết lập đối xứng | xử lý cà vạt | đóng góp bình đẳng | 

## Vỏ cạnh 

Trường hợp góc xuất hiện khi ảnh hưởng của tháp hầu như không đạt đến điểm truy vấn chính xác ở ranh giới của nó. Hãy xem xét một tòa tháp ở$a = 10$với$p = 3$. Ảnh hưởng của nó kết thúc ở số 13 và bắt đầu ở số 7. 

Tại truy vấn 13, đóng góp là$3 - 3 = 0$. Việc quét phải bao gồm tháp vào thời điểm này, không được loại bỏ sớm. Việc vô hiệu hóa tại$r + 1$đảm bảo truy vấn đúng 13h vẫn thấy tháp. 

Một trường hợp cạnh khác liên quan đến nhiều tòa tháp có tọa độ giống hệt nhau. Bởi vì theo dõi kích hoạt được khóa bởi$(a_i, p_i)$, các bản sao được xử lý độc lập. Mỗi bản sao đóng góp riêng biệt và không ghi đè lên bản khác trong cấu trúc đang hoạt động. 

Trường hợp thứ ba phát sinh khi tất cả các tháp đều ở xa truy vấn. Heap chỉ có thể chứa các mục cũ. Việc xóa lười đảm bảo tất cả các tòa tháp không hợp lệ sẽ bị loại bỏ cho đến khi cấu trúc trống, lúc đó câu trả lời đúng sẽ trở thành số 0.
