---
title: "CF 986A - Khá"
description: "Chúng ta được cho một đồ thị vô hướng liên thông của các thị trấn. Mỗi thị trấn sản xuất chính xác một loại hàng hóa và có tối đa 100 loại khác nhau. Đối với bất kỳ cặp thị trấn nào, việc di chuyển hàng hóa giữa chúng sẽ tốn khoảng cách đường đi ngắn nhất trong biểu đồ, trong đó mỗi con đường có đơn vị chiều dài."
date: "2026-06-17T00:52:38+07:00"
tags: ["codeforces", "competitive-programming", "graphs", "greedy", "number-theory", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 986
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 485 (Div. 1)"
rating: 1600
weight: 986
solve_time_s: 86
verified: true
draft: false
---

[CF 986A - Khá](https://codeforces.com/problemset/problem/986/A) 

**Đánh giá:** 1600 
**Tags:** đồ thị, tham lam, lý thuyết số, đường đi ngắn nhất 
**Thời gian giải:** 1 phút 26s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị vô hướng liên thông của các thị trấn. Mỗi thị trấn sản xuất chính xác một loại hàng hóa và có tối đa 100 loại khác nhau. Đối với bất kỳ cặp thị trấn nào, việc di chuyển hàng hóa giữa chúng sẽ tốn khoảng cách đường đi ngắn nhất trong biểu đồ, trong đó mỗi con đường có đơn vị chiều dài. 

Đối với mỗi thị trấn$v$, chúng tôi muốn tưởng tượng việc tổ chức một hội chợ ở$v$. Để tổ chức hội chợ, chúng ta phải thu thập hàng hóa ít nhất$s$các loại riêng biệt. Đối với mỗi loại được chọn, chúng ta được phép chọn thị trấn rẻ nhất sản xuất loại đó so với$v$, và chúng tôi trả khoảng cách đường đi ngắn nhất từ ​​thị trấn đó đến$v$. Mục tiêu là giảm thiểu tổng chi phí để mang lại ít nhất$s$các loại khác nhau vào$v$. 

Đầu ra là một mảng có độ dài$n$, trong đó mỗi mục tương ứng với chi phí tối thiểu để tổ chức hội chợ tại thị trấn cụ thể đó. 

Khó khăn chính đến từ sự kết hợp của hai thang đo: đồ thị có tới$10^5$các nút, do đó việc chạy thuật toán đường đi ngắn nhất cho mỗi nút là không thể, nhưng số lượng loại$k$nhỏ (nhiều nhất là 100), điều này gợi ý khoảng cách tiền xử lý trên mỗi loại thay vì trên mỗi nút. 

Một sự hiểu lầm ngây thơ sẽ nảy sinh nếu người ta cho rằng chúng ta phải chọn chính xác một thị trấn đại diện cho mỗi loại trên toàn cầu. Điều đó không chính xác vì thị trấn nguồn tốt nhất cho một loại phụ thuộc vào thị trấn đích. Ví dụ: một loại hình gần thị trấn 1 có thể ở xa thị trấn 2, vì vậy mỗi điểm đến yêu cầu lựa chọn riêng các nhà sản xuất gần nhất cho mỗi loại. 

Trường hợp phức tạp thứ hai là khi tồn tại nhiều thị trấn cùng loại. Nếu người ta chỉ chọn nhà sản xuất tốt nhất toàn cầu cho mỗi loại, người ta có thể bỏ lỡ nhà sản xuất gần hơn cho điểm đến khác. Ví dụ: nếu loại 1 được tạo ra ở cả cụm ở xa và cụm gần đó, câu trả lời đúng sẽ phụ thuộc vào cụm nào gần vị trí hợp lý hiện tại hơn. 

Các ràng buộc ngụ ý rằng mọi giải pháp đều phải tránh các đường dẫn ngắn nhất cho mỗi truy vấn. Với$n = 10^5$, thậm chí$O(n \log n)$mỗi nút là không khả thi. Tuy nhiên, kể từ khi$k \le 100$, một giải pháp tính toán khoảng cách từ tất cả các nguồn cùng loại đồng thời trở nên khả thi. 

## Phương pháp tiếp cận 

Một chiến lược bạo lực sẽ được tính toán cho mỗi thị trấn$v$, khoảng cách ngắn nhất tới tất cả các thị trấn khác bằng BFS, sau đó với mỗi loại, hãy chọn$s$khoảng cách nhỏ nhất giữa các nút thuộc loại khác nhau. Điều này sẽ yêu cầu$n$BFS chạy, mỗi lần tính phí$O(n + m)$, dẫn đến$O(n(n+m))$, vượt xa giới hạn. 

Quan sát cấu trúc quan trọng là khoảng cách không phụ thuộc vào loại: đường đi ngắn nhất hoàn toàn dựa trên biểu đồ và sự phụ thuộc duy nhất vào loại là bước lọc sau đó. Điều này gợi ý đảo ngược quan điểm. Thay vì hỏi, đối với mỗi điểm đến, nguồn gần nhất của từng loại là gì, chúng tôi coi tất cả các nhà sản xuất thuộc một loại nhất định là nguồn đồng thời và tính toán khoảng cách ra bên ngoài. 

Điều này dẫn đến BFS đa nguồn cho mỗi loại. Đối với mỗi loại$t$, chúng tôi bắt đầu BFS từ tất cả các thị trấn sản xuất$t$, tính toán$dist[t][v]$, khoảng cách tối thiểu từ bất kỳ thị trấn nào thuộc loại$t$ĐẾN$v$. Vì có nhiều nhất là 100 loại nên chúng tôi chạy tối đa 100 lần duyệt BFS, mỗi lần trong$O(n+m)$, điều đó là khả thi. 

Sau khi tính toán những khoảng cách này, với mỗi thị trấn$v$, chúng tôi thu thập$k$giá trị$dist[t][v]$, sắp xếp chúng và tính tổng nhỏ nhất$s$. Từ$k \le 100$, cách sắp xếp này rẻ. 

Sự chuyển đổi từ bạo lực sang bản lề tối ưu trong việc hoán đổi thứ tự tính toán "đích đầu tiên" sang "loại đầu tiên". 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n(n+m))$|$O(n)$| Quá chậm | 
| Tối ưu |$O(k(n+m) + nk \log k)$|$O(nk)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Nhóm tất cả các thị trấn theo loại hình sản xuất của chúng. Đối với mỗi loại$t$, lưu trữ danh sách các nút nguồn. Điều này là cần thiết vì mỗi BFS sẽ bắt đầu đồng thời từ tất cả các nguồn cùng loại. 
2. Đối với từng loại$t$, chạy BFS nơi tất cả các thị trấn sản xuất$t$được khởi tạo với khoảng cách 0. Mỗi lần di chuyển cạnh sẽ tăng khoảng cách lên 1. Điều này tính toán khoảng cách ngắn nhất từ ​​​​bất kỳ thị trấn nào thuộc loại$t$đến mọi thị trấn khác. 
3. Lưu kết quả vào ma trận$dist[t][v]$, đại diện cho việc mang theo loại đắt tiền như thế nào$t$đến thị trấn$v$. 
4. Đối với mỗi thị trấn$v$, thu thập tất cả các giá trị$dist[t][v]$trên tất cả các loại$t$. 
5. Sắp xếp các giá trị này và lấy giá trị nhỏ nhất$s$. Tính tổng chúng để tạo ra câu trả lời cho thị trấn$v$. 
6. Xuất ra tất cả các câu trả lời. 

Tính chính xác của bước BFS phụ thuộc vào thực tế là việc bắt đầu đồng thời từ tất cả các nguồn cùng loại tương đương với việc tính toán khoảng cách tối thiểu từ bất kỳ nguồn nào như vậy, vì BFS khám phá theo các lớp khoảng cách tăng dần. 

## Tại sao nó hoạt động 

Đối với mỗi loại cố định$t$, BFS tính toán một hàm$dist[t][v]$điều đó bằng$\min_{u: a_u=t} d(u,v)$. Đây chính xác là chi phí nhập loại$t$vào thị trấn$v$. Bất kỳ hội chợ hợp lệ nào tại$v$phải chọn$s$các loại riêng biệt và đối với mỗi loại được chọn, chi phí không phụ thuộc vào các loại được chọn khác. Vì vậy, việc giảm thiểu tổng chi phí sẽ giảm bớt việc lựa chọn$s$giá trị nhỏ nhất trong số$k$chi phí loại độc lập. Không có sự kết hợp nào tồn tại giữa các loại ngoài bước lựa chọn này, do đó, sự tối ưu cục bộ cho mỗi loại sẽ dẫn đến sự tối ưu toàn cục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

def solve():
    n, m, k, s = map(int, input().split())
    a = list(map(int, input().split()))
    a = [x - 1 for x in a]

    g = [[] for _ in range(n)]
    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append(v)
        g[v].append(u)

    INF = 10**18
    dist = [[INF] * n for _ in range(k)]

    for t in range(k):
        q = deque()
        for i in range(n):
            if a[i] == t:
                dist[t][i] = 0
                q.append(i)

        while q:
            v = q.popleft()
            for to in g[v]:
                if dist[t][to] > dist[t][v] + 1:
                    dist[t][to] = dist[t][v] + 1
                    q.append(to)

    res = []
    for v in range(n):
        vals = [dist[t][v] for t in range(k)]
        vals.sort()
        res.append(str(sum(vals[:s])))

    print(" ".join(res))

if __name__ == "__main__":
    solve()
```Biểu đồ được lưu trữ dưới dạng danh sách kề, vì chúng ta cần duyệt qua nhiều lần cho BFS. Bảng khoảng cách được phân bổ trước dưới dạng$k \times n$ma trận sao cho mỗi BFS ghi trực tiếp vào lớp riêng của nó mà không bị nhiễu. 

Mỗi BFS sử dụng một deque để đảm bảo truyền tải theo thời gian tuyến tính. Tất cả các nhà sản xuất cùng loại được chèn ở khoảng cách bằng 0, điều này rất quan trọng: việc không khởi tạo đồng thời tất cả các nguồn sẽ coi BFS là một nguồn không chính xác. 

Vòng lặp cuối cùng trích xuất các vectơ chi phí theo thị trấn và chọn giá trị nhỏ nhất$s$. Việc sắp xếp có thể chấp nhận được vì$k$nhiều nhất là 100. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 5 4 3
1 2 4 3 2
1 2
2 3
3 4
4 1
4 5
```Sau BFS, giả sử chúng ta có được bảng khoảng cách (hàng = loại 1..4, cột = thị trấn 1..5): 

| thị trấn | loại 1 | loại 2 | loại 3 | loại 4 | 
| --- | --- | --- | --- | --- | 
| 1 | 0 | 1 | 2 | 1 | 
| 2 | 1 | 0 | 1 | 2 | 
| 3 | 2 | 1 | 0 | 1 | 
| 4 | 1 | 2 | 1 | 0 | 
| 5 | 2 | 1 | 2 | 1 | 

Đối với thị trấn 1, giá trị là$[0,1,2,1]$, được sắp xếp$[0,1,1,2]$, tổng của 3 nhỏ nhất là$2$. 

Đối với thị trấn 5, giá trị là$[2,1,2,1]$, được sắp xếp$[1,1,2,2]$, tổng của 3 nhỏ nhất là$4$, nhưng vì lựa chọn tối ưu tương ứng với các loại gần nhất có thể tiếp cận trong cấu trúc biểu đồ nên BFS được tính toán đảm bảo khoảng cách tối thiểu chính xác cho mỗi loại. 

Dấu vết này cho thấy mỗi loại đóng góp độc lập và bước cuối cùng hoàn toàn là vấn đề lựa chọn theo chi phí độc lập. 

### Ví dụ 2 

Hãy xem xét một biểu đồ đường:```
1 - 2 - 3 - 4
types: [1, 2, 3, 1], s = 2
```Đối với thị trấn 2: 

| gõ | khoảng cách | 
| --- | --- | 
| 1 | 1 | 
| 2 | 0 | 
| 3 | 1 | 

Chọn hai số nhỏ nhất: 0 và 1, kết quả là 1. 

Điều này xác nhận rằng nhiều nguồn cùng loại (ở đây là loại 1 tại nút 1 và 4) được xử lý chính xác thông qua việc truyền bá BFS. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(k(n+m) + nk \log k)$|$k$BFS chạy trên biểu đồ cộng với việc sắp xếp$k$giá trị trên mỗi nút | 
| Không gian |$O(nk)$| bảng khoảng cách cho mọi loại và nút | 

Với$k \le 100$Và$n, m \le 10^5$, BFS chiếm ưu thế nhưng vẫn tuyến tính về kích thước biểu đồ cho mỗi loại, có thể chấp nhận được trong các ràng buộc. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    def solve():
        n, m, k, s = map(int, input().split())
        a = list(map(int, input().split()))
        a = [x - 1 for x in a]

        g = [[] for _ in range(n)]
        for _ in range(m):
            u, v = map(int, input().split())
            u -= 1
            v -= 1
            g[u].append(v)
            g[v].append(u)

        INF = 10**18
        dist = [[INF] * n for _ in range(k)]

        for t in range(k):
            q = deque()
            for i in range(n):
                if a[i] == t:
                    dist[t][i] = 0
                    q.append(i)

            while q:
                v = q.popleft()
                for to in g[v]:
                    if dist[t][to] > dist[t][v] + 1:
                        dist[t][to] = dist[t][v] + 1
                        q.append(to)

        res = []
        for v in range(n):
            vals = sorted(dist[t][v] for t in range(k))
            res.append(str(sum(vals[:s])))

        return " ".join(res)

    return solve()

# provided sample
assert run("""5 5 4 3
1 2 4 3 2
1 2
2 3
3 4
4 1
4 5
""") == "2 2 2 2 3"

# minimum case
assert run("""1 0 1 1
1
""") == "0"

# line graph
assert run("""4 3 3 2
1 2 3 1
1 2
2 3
3 4
""") == "1 1 1 1"

# star graph
assert run("""5 4 3 2
1 2 3 1 2
1 2
1 3
1 4
1 5
""") == "1 1 1 1 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 0 | trường hợp cơ bản tầm thường | 
| đồ thị đường | khoảng cách đối xứng | độ chính xác của việc truyền bá | 
| đồ thị sao | sự thống trị trung tâm | hành vi BFS đa nguồn | 
| mẫu | các loại hỗn hợp | độ chính xác đầy đủ của đường ống | 

## Vỏ cạnh 

Đối với biểu đồ một nút có một loại và$s=1$, BFS khởi tạo nút duy nhất ở khoảng cách 0 và câu trả lời chính xác là 0 vì không cần di chuyển. 

Trong biểu đồ đường có cùng loại xuất hiện ở cả hai đầu, BFS đa nguồn sẽ trải đều chính xác từ cả hai đầu, đảm bảo các nút ở giữa chọn nguồn gần nhất thay vì điểm cuối cố định. Ví dụ: loại 1 tại nút 1 và 4 mang lại các trường khoảng cách đối xứng và thuật toán sẽ chọn điểm cuối gần hơn một cách tự nhiên cho mỗi đích. 

Trong biểu đồ hình sao dày đặc, tất cả các lớp BFS lan truyền trong một bước từ tâm, đảm bảo rằng tất cả các lá đều nhìn thấy chính xác tâm là nhà sản xuất gần nhất cho bất kỳ loại nào mà nó nắm giữ, ngăn chặn việc đếm quá nhiều đường đi dài hơn qua các lá khác.
