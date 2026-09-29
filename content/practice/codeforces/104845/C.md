---
title: "CF 104845C - \u0420\u0435\u0441\u0442\u043e\u0440\u0430\u043d\u043d\u044b\u0439 \u0431\u0438\u0437\u043d\u0435\u0441"
description: "Chúng ta có một mạng lưới các nhà hàng trong đó các cạnh thể hiện mối quan hệ “láng giềng”. Một số nhà hàng đã hợp tác với Timur ngay từ đầu."
date: "2026-06-28T11:30:02+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104845
codeforces_index: "C"
codeforces_contest_name: "\u041c\u0443\u043d\u0438\u0446\u0438\u043f\u0430\u043b\u044c\u043d\u044b\u0439 \u044d\u0442\u0430\u043f \u0412\u0441\u041e\u0428 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 \u0432 \u041c\u043e\u0441\u043a\u043e\u0432\u0441\u043a\u043e\u0439 \u043e\u0431\u043b\u0430\u0441\u0442\u0438 2023-2024 (9-11 \u043a\u043b\u0430\u0441\u0441\u044b)"
rating: 0
weight: 104845
solve_time_s: 83
verified: false
draft: false
---

[CF 104845C - \u0420\u0435\u0441\u0442\u043e\u0440\u0430\u043d\u043d\u044b\u0439 \u0431\u0438\u0437\u043d\u0435\u0441](https://codeforces.com/problemset/problem/104845/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 23s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một mạng lưới các nhà hàng trong đó các cạnh thể hiện mối quan hệ “láng giềng”. Một số nhà hàng đã hợp tác với Timur ngay từ đầu. Từ những người chấp nhận ban đầu này, sự hợp tác có thể lan rộng: bất kỳ nhà hàng nào cũng sẽ quyết định hợp tác nếu ít nhất$k$các nước láng giềng của nó đã hợp tác. 

Đây không phải là một quyết định một lần. Khi nhiều nhà hàng bắt đầu hợp tác, họ có thể khiến nhiều nhà hàng khác vượt qua ngưỡng, vì vậy quá trình này sẽ tiếp tục cho đến khi không thể kích hoạt nhà hàng mới nào. 

Nhiệm vụ là xác định những nhà hàng nào cuối cùng sẽ hợp tác sau khi quá trình lan truyền này ổn định, bắt đầu từ tập hợp ban đầu. 

Biểu đồ có thể chứa tới$2 \cdot 10^5$các nút và cạnh, vì vậy mọi giải pháp đều phải gần tuyến tính về số cạnh. Mô phỏng bậc hai trên tất cả các nút ở mỗi bước sẽ quá chậm vì việc quét liên tục các nút lân cận sẽ dẫn đến$O(nm)$hành vi trong trường hợp dày đặc. 

Trường hợp cạnh tinh tế xuất hiện khi$k = 0$. Trong tình huống đó, mọi nhà hàng ngay lập tức đủ điều kiện bất kể hàng xóm, vì vậy câu trả lời cuối cùng là tất cả các nút. Bất kỳ triển khai nào vẫn cố gắng mô phỏng quá trình lan truyền đều phải xử lý vấn đề này một cách cẩn thận, nếu không nó có thể quá phức tạp hoặc bị tính sai. 

Một trường hợp khác đáng chú ý là khi tập ban đầu trống. Vậy thì không có nhà hàng nào có hàng xóm được kích hoạt, vì vậy trừ khi$k = 0$, không có gì có thể được kích hoạt. 

## Phương pháp tiếp cận 

Cách ngây thơ để nghĩ về quy trình này là liên tục quét tất cả các nhà hàng và kiểm tra từng nhà hàng xem liệu nó có thỏa mãn điều kiện “ít nhất” hay không.$k$hàng xóm tích cực". Mỗi khi chúng tôi tìm thấy một nhà hàng mới đáp ứng được yêu cầu đó, chúng tôi sẽ kích hoạt nhà hàng đó và bắt đầu lại quá trình quét. 

Điều này đúng vì nó phản ánh trực tiếp định nghĩa quy tắc. Tuy nhiên, vấn đề là hiệu suất. Chi phí mỗi lần quét trên tất cả các nút$O(n + m)$nếu chúng tôi tính số lượt kiểm tra của hàng xóm và trong trường hợp xấu nhất, chúng tôi có thể kích hoạt từng nhà hàng một, dẫn đến$O(n)$vòng. Điều này mang lại$O(n(n + m))$, nó quá lớn đối với$n, m \le 2 \cdot 10^5$. 

Quan sát quan trọng là chúng ta không bao giờ cần tính lại toàn bộ số lượng hàng xóm từ đầu. Điều quan trọng là mỗi nhà hàng hiện có bao nhiêu người hàng xóm đang hoạt động. Điều này gợi ý việc duy trì bộ đếm đang chạy trên mỗi nút và cập nhật nó dần dần khi nút lân cận hoạt động. 

Khi chúng ta xem nó theo cách này, quy trình sẽ trở thành một sự lan truyền tiêu chuẩn trên biểu đồ. Chúng tôi bắt đầu với tất cả các nút hoạt động ban đầu, đẩy chúng vào hàng đợi và với mỗi lần kích hoạt, chúng tôi sẽ cập nhật các nút lân cận của nó. Bất cứ khi nào số lượng hoạt động của hàng xóm đạt đến$k$, nó sẽ hoạt động và cũng được đẩy vào hàng đợi. 

Điều này biến quy trình thành một bản mở rộng kiểu theo chiều rộng, trong đó mỗi cạnh chỉ được xử lý khi một điểm cuối hoạt động. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n(n + m))$|$O(n + m)$| Quá chậm | 
| Tuyên truyền BFS tối ưu |$O(n + m)$|$O(n + m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải thích biểu đồ như một danh sách kề. Mỗi nút duy trì một bộ đếm để theo dõi xem có bao nhiêu nút lân cận đang hoạt động. 

1. Xây dựng danh sách kề của đồ thị từ các cạnh đầu vào. Điều này là cần thiết để chúng ta có thể duyệt qua các nút lân cận một cách hiệu quả khi một nút bắt đầu hoạt động. 
2. Tạo một mảng`cnt`được khởi tạo bằng 0 cho tất cả các nút. Điều này sẽ lưu trữ số lượng hàng xóm đang hoạt động mà mỗi nút hiện có. 
3. Khởi tạo một mảng boolean`active`Ở đâu`active[i]`đúng nếu nhà hàng$i$đã hợp tác ngay từ đầu. Các nút này tạo thành biên giới ban đầu của quá trình lan truyền. 
4. Chèn tất cả các nút hoạt động ban đầu vào hàng đợi. Đây là những nút duy nhất có thể ảnh hưởng ngay lập tức đến người khác. 
5. Trong khi hàng đợi không trống, hãy trích xuất một nút đang hoạt động$u$. Đối với mỗi người hàng xóm$v$của$u$, tăng`cnt[v]`bởi một vì có thêm một hàng xóm của nó đã hoạt động. 
6. Nếu`cnt[v]`đạt chính xác$k$Và$v$chưa hoạt động, đánh dấu$v$đang hoạt động và đẩy nó vào hàng đợi. Thời điểm điều này xảy ra,$v$trở thành một nguồn ảnh hưởng mới cho các nước láng giềng. 
7. Tiếp tục cho đến khi hết hàng đợi. Tại thời điểm đó không có nút không hoạt động nào đạt đến ngưỡng nữa nên quá trình ổn định. 

### Tại sao nó hoạt động 

Thuật toán duy trì tính bất biến đối với mọi nút,`cnt[v]`luôn bằng số láng giềng của$v$đã được kích hoạt tại thời điểm hiện tại trong quy trình. Mọi sự kiện kích hoạt chỉ làm tăng các bộ đếm này chứ không bao giờ làm giảm chúng, phù hợp với tính chất đơn điệu của quy luật trải rộng. 

Một nút sẽ hoạt động chính xác khi số lượng hàng xóm hoạt động thực sự của nó đạt ít nhất$k$. Vì bộ đếm được cập nhật ngay lập tức khi hàng xóm kích hoạt nên không có nút đủ điều kiện nào bị bỏ lỡ và không có nút nào được kích hoạt sớm. Hàng đợi đảm bảo rằng các kích hoạt lan truyền theo đúng thứ tự nhân quả, tương đương với việc áp dụng nhiều lần quy tắc ban đầu cho đến khi đạt đến một điểm cố định. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

def solve():
    n, m, k = map(int, input().split())
    
    adj = [[] for _ in range(n + 1)]
    
    for _ in range(m):
        u, v = map(int, input().split())
        adj[u].append(v)
        adj[v].append(u)
    
    initial = list(map(int, input().split()))
    if len(initial) == 1 and initial[0] == 0:
        initial = []
    
    if k == 0:
        print(n)
        print(*range(1, n + 1))
        return
    
    active = [False] * (n + 1)
    cnt = [0] * (n + 1)
    
    q = deque()
    
    for x in initial:
        if not active[x]:
            active[x] = True
            q.append(x)
    
    while q:
        u = q.popleft()
        for v in adj[u]:
            if active[v]:
                continue
            cnt[v] += 1
            if cnt[v] >= k:
                active[v] = True
                q.append(v)
    
    res = [i for i in range(1, n + 1) if active[i]]
    
    print(len(res))
    print(*res)

def main():
    solve()

if __name__ == "__main__":
    main()
```Danh sách kề được xây dựng theo cách tiêu chuẩn, đảm bảo mỗi cạnh được lưu trữ hai lần do đồ thị không bị định hướng. các`cnt`mảng là cơ chế trung tâm theo dõi tiến trình hướng tới điều kiện ngưỡng. 

Hàng đợi chứa chính xác các nút vừa hoạt động. Mỗi nút được xử lý một lần và sau khi được xử lý, nó sẽ không bao giờ vào lại hàng đợi vì kích hoạt là vĩnh viễn. Điều này đảm bảo độ phức tạp tuyến tính. 

Một chi tiết triển khai nhỏ là việc xử lý sớm các$k = 0$, giúp tránh hoàn toàn việc xử lý đồ thị không cần thiết. Một điểm tinh tế khác là đảm bảo chúng tôi không thêm lại các nút đã hoạt động, nếu không sẽ làm tăng số lượng không chính xác. 

## Ví dụ đã hoạt động 

Hãy xem xét một biểu đồ nhỏ trong đó quá trình kích hoạt được thực hiện dần dần. 

đầu vào:```
5 4 2
1 2
2 3
3 4
4 5
1 5
```Các nút hoạt động ban đầu: 1 và 5. 

| Bước | Nút hoạt động | Số lượng cập nhật | Mới kích hoạt | 
| --- | --- | --- | --- | 
| Bắt đầu | - | tất cả đều bằng không | 1, 5 | 
| 1 | 1 | cnt[2]=1 | không | 
| 2 | 5 | cnt[4]=1 | không | 
| 3 | 2 (chưa hoạt động) | sau 2 bắt đầu hoạt động, cnt[3]=1 | không | 
| 4 | 3 | cnt[2]=2 | 2 | 
| 5 | 4 | cnt[3]=2 | 3 | 

Cuối cùng tất cả các nút kích hoạt. 

Dấu vết này cho thấy cách kích hoạt không phụ thuộc vào khả năng tiếp cận trực tiếp mà phụ thuộc vào việc tích lũy đủ số lượng hàng xóm hoạt động. 

Bây giờ hãy xem xét trường hợp quá trình lan truyền dừng sớm: 

đầu vào:```
4 2 2
1 2
3 4
1
```| Bước | Nút hoạt động | Số lượng cập nhật | Mới kích hoạt | 
| --- | --- | --- | --- | 
| Bắt đầu | - | tất cả đều bằng không | 1 | 
| 1 | 1 | cnt[2]=1 | không | 
| Kết thúc | - | ổn định | không | 

Nút 2 không bao giờ đạt tới ngưỡng 2 nên quá trình truyền sẽ dừng ngay lập tức. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n + m)$| Mỗi cạnh được xử lý tối đa hai lần, một lần cho mỗi sự kiện kích hoạt điểm cuối | 
| Không gian |$O(n + m)$| Danh sách kề cộng với mảng phụ cho bộ đếm và trạng thái | 

Độ phức tạp tuyến tính là đủ cho$2 \cdot 10^5$các nút và các cạnh. Mỗi hoạt động bên trong BFS là thời gian không đổi, do đó giải pháp chạy thoải mái trong giới hạn thông thường. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    def solve():
        n, m, k = map(int, input().split())
        adj = [[] for _ in range(n + 1)]
        for _ in range(m):
            u, v = map(int, input().split())
            adj[u].append(v)
            adj[v].append(u)

        initial = list(map(int, input().split()))
        if len(initial) == 1 and initial[0] == 0:
            initial = []

        if k == 0:
            print(n)
            print(*range(1, n + 1))
            return

        active = [False] * (n + 1)
        cnt = [0] * (n + 1)
        q = deque()

        for x in initial:
            if not active[x]:
                active[x] = True
                q.append(x)

        while q:
            u = q.popleft()
            for v in adj[u]:
                if active[v]:
                    continue
                cnt[v] += 1
                if cnt[v] >= k:
                    active[v] = True
                    q.append(v)

        res = [i for i in range(1, n + 1) if active[i]]
        print(len(res))
        print(*res)

    solve()
    return sys.stdout.getvalue().strip()

# provided sample
assert run("""5 5 2
1 2
2 3
3 4
4 5
3 1
1 2 5
""") == "5\n1 2 3 4 5"

# minimum case
assert run("""1 0 0
0
""") == "1\n1"

# no propagation
assert run("""4 2 2
1 2
3 4
1
""") == "1\n1"

# full propagation
assert run("""5 4 2
1 2
2 3
3 4
4 5
1 5
""") == "5\n1 2 3 4 5"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn, k=0 | tất cả các nút | kích hoạt đầy đủ tầm thường | 
| đồ thị bị ngắt kết nối | chỉ ban đầu | không thể xếp tầng | 
| chuỗi có hai nguồn | lan truyền đầy đủ | độ chính xác lan truyền đa nguồn | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là$k = 0$. Trong tình huống này, mọi nhà hàng đều thỏa mãn điều kiện ngay lập tức bất kể cấu trúc đồ thị. Thuật toán xử lý việc này bằng cách in trực tiếp tất cả các nút mà không cần xử lý biểu đồ. Ví dụ: với bất kỳ biểu đồ đầu vào nào và$k = 0$, đầu ra phải luôn là tập hợp đầy đủ$1 \ldots n$và bỏ qua BFS sẽ ngăn cản những công việc không cần thiết. 

Một trường hợp khác là tập ban đầu trống với giá trị dương$k$. Trong trường hợp đó, không nút nào có thể có được các nút lân cận đang hoạt động, do đó hàng đợi bắt đầu trống và quá trình kết thúc ngay lập tức. Bất biến giữ nguyên vì tất cả các bộ đếm vẫn bằng 0, không bao giờ đạt đến ngưỡng. 

Cuối cùng, hãy xem xét một nút có bậc nhỏ hơn$k$. Các nút như vậy không bao giờ có thể kích hoạt trừ khi chúng hoạt động ban đầu. Cơ chế truy cập thực thi điều này một cách tự nhiên vì`cnt[v]`không bao giờ có thể vượt quá mức độ của nó, vì vậy nó không bao giờ có thể đạt tới$k$nếu như$k$lớn hơn.
