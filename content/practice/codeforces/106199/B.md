---
title: "CF 106199B - \u0412\u044b\u0431\u043e\u0440 \u0432\u0435\u0440\u0441\u0438\u0439 \u043a\u043e\u043c\u043f\u043e\u043d\u0435\u043d\u0442\u043e\u0432"
description: "Chúng ta có một hệ thống gồm các thành phần $n$, mỗi thành phần phải được gán một giá trị nguyên không âm $di$. Những giá trị này không độc lập. Có những ràng buộc trực tiếp giữa các cặp thành phần."
date: "2026-06-19T18:34:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106199
codeforces_index: "B"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2024-2025, \u0412\u0442\u043e\u0440\u0430\u044f \u043b\u0438\u0447\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 106199
solve_time_s: 68
verified: true
draft: false
---

[CF 106199B - \u0412\u044b\u0431\u043e\u0440 \u0432\u0435\u0440\u0441\u0438\u0439 \u043a\u043e\u043c\u043f\u043e\u043d\u0435\u043d\u0442\u043e\u0432](https://codeforces.com/problemset/problem/106199/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một hệ thống$n$thành phần, mỗi thành phần phải được gán một giá trị nguyên không âm$d_i$. Những giá trị này không độc lập. Có những ràng buộc trực tiếp giữa các cặp thành phần. Mỗi ràng buộc có dạng$u \to v$với các thông số$a, b$, và nó thực thi một giới hạn dưới trên$d_u$về mặt$d_v$: giá trị tại$u$ít nhất phải có$a \cdot d_v + b$. 

Ngoài các quy tắc phụ thuộc này, còn có ngân sách toàn cầu: tổng của tất cả các giá trị được chọn$d_i$không được vượt quá$X$. Trong số tất cả các phép gán thỏa mãn cả ràng buộc phụ thuộc và ràng buộc tổng, chúng ta muốn tối đa hóa giá trị tối thiểu trong số tất cả các phép gán$d_i$. Nếu không có nhiệm vụ hợp lệ nào tồn tại, chúng tôi phải báo cáo là không thể thực hiện được. 

Vì vậy, cấu trúc là một hệ thống các bất đẳng thức tuyến tính giới hạn dưới với giới hạn trên toàn cục của tổng. Mục tiêu không phải là tối đa hóa tổng hoặc các giá trị riêng lẻ mà là đẩy biến nhỏ nhất lên cao nhất có thể trong khi vẫn khả thi. 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$các biến và phụ thuộc cho mỗi thử nghiệm và tối đa$10^4$các bài kiểm tra. Điều này ngay lập tức loại trừ mọi thứ bậc hai hoặc thậm chí bậc ba cho mỗi bài kiểm tra. Bất kỳ giải pháp nào về cơ bản phải tuyến tính hoặc gần tuyến tính trong tổng kích thước đầu vào, có thể$O((n + m) \log X)$hoặc tốt hơn. 

Một cách giải thích ngây thơ sẽ cố gắng gán các giá trị một cách tham lam hoặc truyền bá các ràng buộc một lần. Điều đó không thành công vì các ràng buộc có thể hình thành các chuỗi và chu trình khuếch đại các giá trị theo cấp số nhân thông qua các hệ số$a_i$. Ngay cả những chu kỳ nhỏ cũng có thể tạo ra hành vi điểm cố định lớn. 

Một vài tình huống thất bại minh họa cho sự tinh tế này: 

Hãy xem xét hai biến có ràng buộc lẫn nhau:$1 \to 2: d_1 \ge d_2$,$2 \to 1: d_2 \ge d_1 + 1$. Điều này là không thể bởi vì nó ngụ ý$d_1 \ge d_1 + 1$. 

Một trường hợp tế nhị hơn:$1 \to 2: d_1 \ge 2 d_2$,$2 \to 1: d_2 \ge 2 d_1$. Điều này buộc tăng trưởng theo cấp số nhân theo cả hai hướng và là không thể trừ khi tất cả các giá trị bằng 0, nhưng 0 có thể vi phạm các ràng buộc tùy thuộc vào hằng số. 

Một trường hợp khác là khi các ràng buộc tạo thành một chuỗi và các mức tăng nhỏ sẽ lan truyền ngược lại, khiến một nút thống trị toàn bộ hệ thống. Phép gán tham lam cục bộ không thành công vì việc thay đổi một nút sẽ làm mất hiệu lực các giả định trước đó. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng đoán giá trị mục tiêu$T$, thi hành$d_i \ge T$cho tất cả các nút và truyền các ràng buộc lặp đi lặp lại cho đến khi đạt đến một điểm cố định, sau đó kiểm tra xem liệu ràng buộc tổng có được thỏa mãn hay không. Điều này vốn đã tốn kém vì mỗi lần truyền có thể đi qua tất cả các cạnh nhiều lần và chúng ta sẽ cần thử nhiều giá trị của$T$. Trong trường hợp xấu nhất, điều này trở thành$O(nm)$mỗi lần kiểm tra và nhân với tìm kiếm nhị phân trên các giá trị lớn trở nên không khả thi. 

Quan sát quan trọng là vấn đề đơn điệu ở giá trị mục tiêu tối thiểu. Nếu chúng ta chọn một ứng cử viên$T$, chúng ta có thể kiểm tra tính khả thi: thực thi$d_i \ge T$, sau đó liên tục áp dụng các ràng buộc$d_u = \max(d_u, a d_v + b)$cho đến khi ổn định. Nếu quá trình này vượt quá ngân sách$X$, hoặc tạo ra mâu thuẫn, ứng cử viên không hợp lệ. 

Điều này chuyển vấn đề thành kiểm tra tính khả thi của hệ bất đẳng thức tuyến tính cực đại. Mỗi ràng buộc truyền bá các giới hạn dưới về phía trước và hệ thống hội tụ vì mọi cập nhật chỉ làm tăng giá trị và các giá trị bị giới hạn ở trên bởi tính khả thi hoặc bằng cách phát hiện tràn. 

Sau đó chúng tôi tìm kiếm nhị phân giá trị tối thiểu khả thi tối đa$T$. Đối với mỗi lần kiểm tra, chúng tôi tính toán nhiệm vụ khả thi tối thiểu phù hợp với$d_i \ge T$và các ràng buộc phụ thuộc, sau đó xác minh tổng. 

Điều này có hiệu quả vì ngày càng tăng$T$chỉ tăng tất cả$d_i$trong việc đóng kết quả, không bao giờ giảm chúng, vì vậy tính khả thi là đơn điệu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Tuyên truyền Brute Force mỗi lần đoán |$O(nm \log X)$|$O(n + m)$| Quá chậm | 
| Tìm kiếm nhị phân + lan truyền ràng buộc |$O((n + m) \log X \log C)$|$O(n + m)$| Đã chấp nhận | 

Đây$C$đại diện cho độ lớn của các giá trị trong quá trình truyền bá, được giới hạn bởi$10^{18}$tỉ lệ. 

## Hướng dẫn thuật toán 

Chúng tôi coi vấn đề này như việc giải quyết lặp đi lặp lại một “bài tập khả thi tối thiểu” dưới một ràng buộc giới hạn dưới cố định$T$. 

1. Cố định giá trị ứng viên$T$. Khởi tạo mọi$d_i = T$. Điều này thực thi yêu cầu rằng mức tối thiểu ít nhất là$T$. 
2. Đặt tất cả các nút vào hàng đợi. Chúng tôi sẽ tuyên truyền nới lỏng ràng buộc bằng cách sử dụng quy trình giống như BFS. 
3. Trong khi hàng đợi không trống, hãy lấy một nút$v$và cố gắng nới lỏng mọi ràng buộc đang đến có ảnh hưởng đến nó. Đối với mỗi cạnh$u \to v$với các thông số$a, b$, chúng tôi kiểm tra xem$d_u$phải tăng để thỏa mãn$d_u \ge a \cdot d_v + b$. Nếu vậy, hãy cập nhật$d_u$và đẩy$u$vào hàng đợi. 

Lý do chúng tôi xử lý cập nhật theo cách này là vì giá trị của mỗi nút là giá trị tối đa trong số tất cả các ràng buộc buộc nó đi lên, do đó việc giãn lặp đi lặp lại sẽ hội tụ về điểm cố định tối thiểu. 

1. Trong quá trình truyền bá, nếu bất kỳ giá trị nào vượt quá giới hạn an toàn (ví dụ:$X$), chúng ta có thể dừng lại sớm và tuyên bố điều này$T$không thể thực hiện được. Bất kỳ sự gia tăng nào nữa chỉ làm xấu đi tính khả thi. 
2. Sau khi ổn định, tính tổng của tất cả$d_i$. Nếu nó vượt quá$X$, cái này$T$là không thể thực hiện được. Nếu không thì nó là khả thi. 
3. Tìm kiếm nhị phân trên$T$từ$0$đến giới hạn trên đủ lớn (thường$10^{18}$). Theo dõi nhiệm vụ khả thi nhất. 
4. Chạy lại quá trình truyền để có kết quả tối ưu cuối cùng$T$để xây dựng lại thực tế$d_i$các giá trị. 

Tại sao nó hoạt động: 

Bất biến chính là sau mỗi bước nới lỏng, mọi ràng buộc của biểu mẫu$d_u \ge a d_v + b$được thỏa mãn cho tất cả các cạnh được xử lý liên quan đến các nút được cập nhật và giá trị chỉ tăng khi cần thiết. Điều này đảm bảo rằng một khi hàng đợi trống, không có ràng buộc nào có thể tăng thêm bất kỳ biến nào mà không vi phạm mức tối thiểu của các giá trị trước đó. Vectơ kết quả là điểm cố định tối thiểu ở trên$T$. Vì cả những ràng buộc và mục tiêu đều đơn điệu trong$T$, tìm kiếm nhị phân sẽ tìm chính xác mức tối thiểu khả thi tối đa. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

def check(n, edges, X, T):
    d = [T] * n
    q = deque(range(n))
    inq = [True] * n

    while q:
        v = q.popleft()
        inq[v] = False
        dv = d[v]

        for u, a, b in edges[v]:
            # constraint: d_u >= a * d_v + b
            nd = a * dv + b
            if nd > d[u]:
                d[u] = nd
                if d[u] > X:
                    return None
                if not inq[u]:
                    q.append(u)
                    inq[u] = True

    total = sum(d)
    if total > X:
        return None
    return d

def solve():
    t = int(input())
    for _ in range(t):
        n, m, X = map(int, input().split())
        edges = [[] for _ in range(n)]

        for _ in range(m):
            u, v, a, b = map(int, input().split())
            u -= 1
            v -= 1
            edges[v].append((u, a, b))

        lo, hi = 0, 10**18
        best = None

        while lo <= hi:
            mid = (lo + hi) // 2
            res = check(n, edges, X, mid)
            if res is not None:
                best = res
                lo = mid + 1
            else:
                hi = mid - 1

        if best is None:
            print(-1)
        else:
            print(*best)

if __name__ == "__main__":
    solve()
```Việc triển khai mã hóa các ràng buộc ở dạng kề cận ngược để mỗi nút chỉ cần kiểm tra các ảnh hưởng đến khi giá trị của nó thay đổi. Hàng đợi BFS đảm bảo rằng mọi sự gia tăng sẽ lan truyền cho đến khi ổn định. 

chức năng`check`tính toán vectơ khả thi tối thiểu cho một ngưỡng tối thiểu cố định. Điều kiện dừng sớm khi giá trị vượt quá$X$ngăn chặn sự lan truyền không cần thiết. Tìm kiếm nhị phân sau đó đảm bảo chúng tôi tìm thấy mức tối thiểu lớn nhất có thể. 

Một điểm tinh tế là chúng tôi luôn chỉ truyền từ các nút được cập nhật. Nếu không có`inq`Guard, hàng đợi có thể tăng quá mức và xử lý lại các nút không thay đổi nhiều lần, điều này sẽ làm giảm hiệu suất đáng kể. 

## Ví dụ đã hoạt động 

Hãy xem xét một hệ thống nhỏ có ba nút và các phụ thuộc tuyến tính đơn giản. 

đầu vào:```
3 2 100
1 2 1 0
2 3 1 0
```Chúng tôi giải thích các ràng buộc như sau:$d_1 \ge d_2$,$d_2 \ge d_3$, với tổng số tiền giới hạn. 

Chúng tôi tìm kiếm nhị phân$T$. 

Vì$T = 10$: 

| Bước | Xếp hàng | Nút cập nhật | Giá trị (d1,d2,d3) | 
| --- | --- | --- | --- | 
| ban đầu | 0,1,2 | không | (10,10,10) | 
| thư giãn 2→3 | 2 | d2=10 ≥10 | (10,10,10) | 
| thư giãn 1→2 | 1 | d1=10 ≥10 | (10,10,10) | 

Tổng là 30, khả thi. 

Vì$T = 40$: 

| Bước | Xếp hàng | Nút cập nhật | Giá trị (d1,d2,d3) | 
| --- | --- | --- | --- | 
| ban đầu | 0,1,2 | không | (40,40,40) | 

Tổng là 120 > 100, không thể thực hiện được. 

Điều này cho thấy tìm kiếm nhị phân phân biệt chính xác tính khả thi dựa trên tổng toàn cục. 

Dấu vết xác nhận rằng khi một ngưỡng được cố định, việc truyền bá không thay đổi cấu trúc tương đối mà chỉ thay đổi tỷ lệ tuyệt đối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((n + m) \log 10^{18})$| mỗi kiểm tra tính khả thi đều tuyến tính, tìm kiếm nhị phân vượt ngưỡng | 
| Không gian |$O(n + m)$| danh sách kề cộng với mảng làm việc | 

Tổng kích thước đầu vào qua các thử nghiệm là$2 \cdot 10^5$, do đó, sự lan truyền tuyến tính trên mỗi lần kiểm tra vẫn nằm trong giới hạn khi kết hợp với độ sâu tìm kiếm logarit. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    def check(n, edges, X, T):
        d = [T] * n
        q = deque(range(n))
        inq = [True] * n

        while q:
            v = q.popleft()
            inq[v] = False
            dv = d[v]
            for u, a, b in edges[v]:
                nd = a * dv + b
                if nd > d[u]:
                    d[u] = nd
                    if d[u] > X:
                        return None
                    if not inq[u]:
                        q.append(u)
                        inq[u] = True

        if sum(d) > X:
            return None
        return d

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            n, m, X = map(int, input().split())
            edges = [[] for _ in range(n)]
            for _ in range(m):
                u, v, a, b = map(int, input().split())
                u -= 1; v -= 1
                edges[v].append((u, a, b))

            lo, hi = 0, 10**6
            best = None
            while lo <= hi:
                mid = (lo + hi) // 2
                res = check(n, edges, X, mid)
                if res is not None:
                    best = res
                    lo = mid + 1
                else:
                    hi = mid - 1

            if best is None:
                out.append("-1")
            else:
                out.append(" ".join(map(str, best)))

        return "\n".join(out)

    return solve()

# provided sample placeholders would go here
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Nút đơn, không có cạnh | giá trị tầm thường | tính khả thi cơ bản | 
| Phụ thuộc chuỗi | truyền đơn điệu | xử lý ràng buộc bắc cầu | 
| Ràng buộc theo chu kỳ | -1 hoặc điểm cố định giới hạn | phát hiện chu kỳ thông qua tăng trưởng | 
| Tổng chặt chẽ X | bão hòa ranh giới | thực thi ràng buộc toàn cầu | 

## Vỏ cạnh 

Một chu kỳ thuần túy với những ràng buộc tăng trưởng trái ngược nhau là trường hợp tế nhị nhất. Hãy xem xét hai nút:```
2 2 100
1 2 2 0
2 1 2 0
```Bắt đầu từ bất kỳ$T$, sự lan truyền làm cho cả hai giá trị tăng theo cấp số nhân mà không bị ràng buộc trừ khi bị giới hạn bởi$X$. Thuật toán phát hiện điều này thông qua các bản cập nhật lặp đi lặp lại mà cuối cùng vượt quá$X$, trả về tính không khả thi đối với trường hợp đủ lớn$T$. Tìm kiếm nhị phân sau đó giảm chính xác xuống dưới chế độ không ổn định. 

Trường hợp cạnh thứ hai là một chuỗi dài có hệ số lớn. Ngay cả khi các giá trị vẫn hữu hạn, chúng có thể tăng nhanh đến gần$10^{18}$, nên cắt sớm khi vượt quá$X$là điều cần thiết để tránh tràn và lan truyền không cần thiết.
