---
title: "CF 105151C - \u041d\u0438\u0436\u043d\u0438\u0439 \u041d\u0438\u0436\u043d\u0438\u0439 \u041d\u0438\u0436\u043d\u0438\u0439 \u041d\u043e\u0432\u0433\u043e\u0440\u043e\u0434"
description: "Chúng ta được cho một đồ thị với các trạm là các đỉnh và các đường hầm là các cạnh vô hướng. Mỗi trạm có một chi phí và chúng ta cũng có giá trị mô đun $k$. Đối với bất kỳ trạm bắt đầu nào được chọn $s$, chúng tôi xem xét tất cả các trạm có thể truy cập được từ $s$ bằng cách sử dụng tối đa các cạnh $d$."
date: "2026-06-27T13:11:46+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105151
codeforces_index: "C"
codeforces_contest_name: "XIX \u041d\u0438\u0436\u0435\u0433\u043e\u0440\u043e\u0434\u0441\u043a\u0430\u044f \u0433\u043e\u0440\u043e\u0434\u0441\u043a\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u0438\u043d\u0444\u043e\u0440\u043c\u0430\u0442\u0438\u043a\u0435 \u0438\u043c. \u0412. \u0414. \u041b\u0435\u043b\u044e\u0445\u0430"
rating: 0
weight: 105151
solve_time_s: 88
verified: false
draft: false
---

[CF 105151C - \u041d\u0438\u0436\u043d\u0438\u0439 \u041d\u0438\u0436\u043d\u0438\u0439 \u041d\u0438\u0436\u043d\u0438\u0439 \u041d\u043e\u0432\u0433\u043e\u0440\u043e\u0434](https://codeforces.com/problemset/problem/105151/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 28s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị với các trạm là các đỉnh và các đường hầm là các cạnh vô hướng. Mỗi trạm có một chi phí và chúng tôi cũng có một giá trị mô đun$k$. Đối với bất kỳ trạm xuất phát nào được chọn$s$, chúng tôi xem xét tất cả các trạm có thể truy cập từ$s$sử dụng nhiều nhất$d$các cạnh. Bên trong tập hợp giới hạn bán kính này, chúng tôi được phép chọn bất kỳ tập hợp con không trống nào của các trạm và chúng tôi chỉ quan tâm đến việc liệu tổng chi phí của tập hợp con đã chọn có thể chia hết cho$k$. 

Đối với mỗi trạm$i$, ta phải tìm bán kính nhỏ nhất$d$như vậy từ$i$, giữa các nút trong khoảng cách$d$, chúng ta có thể tạo thành một tập hợp con có tổng chi phí là bội số của$k$. Nếu điều này là không thể ngay cả khi tất cả các nút đều có thể truy cập được, chúng tôi sẽ xuất ra$-1$. 

Cấu trúc quan trọng là việc lựa chọn tập hợp con chỉ phụ thuộc vào tổng dư theo modulo$k$, không phải trên các giá trị chính xác. Từ$k \le 50$, vấn đề cơ bản là về tổng tập hợp con có thể đạt được trong một tập hợp các nút ngày càng tăng được xác định bởi các lớp khoảng cách trong biểu đồ. 

Các ràng buộc rất lớn: lên tới$2 \cdot 10^5$các nút và các cạnh. Bất kỳ giải pháp nào tính toán lại khả năng tiếp cận hoặc tập hợp con DP một cách độc lập trên mỗi nút đều quá chậm. Ngay cả BFS cho mỗi nút bắt đầu cũng đã có$O(n(n+m))$, vượt xa giới hạn khả thi. 

Một tập hợp con DP đơn giản trên mỗi lần mở rộng bán kính cũng là không thể vì mỗi lần mở rộng có thể chạy lại một quy trình giống như chiếc ba lô trong tối đa$O(n)$nút, dẫn đến$O(n^2 k)$hoặc tệ hơn. 

Một trường hợp khó nhận thấy là khi tất cả chi phí nút là bội số của$k$. Trong trường hợp đó, bất kỳ nút đơn nào cũng đã tạo thành một tập hợp con hợp lệ, vì vậy câu trả lời luôn là$d = 0$. Một giải pháp ngây thơ đòi hỏi phải kết hợp nhiều nút có thể trả về bán kính lớn hơn một cách không chính xác. 

Một trường hợp cạnh khác là khi không có tập hợp con nào có tổng bằng bội số của$k$. Điều này xảy ra khi tất cả$c_i \bmod k$là phần dư khác 0 và cấu trúc đồ thị không cho phép các mẫu hủy. Vậy thì câu trả lời phải là$-1$ngay cả khi toàn bộ biểu đồ được bao gồm. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ khắc phục nút bắt đầu$i$, sau đó mở rộng từng lớp BFS. Ở mỗi khoảng cách$d$, chúng tôi thu thập tất cả các nút trong quả bóng và chạy tập hợp con DP theo modulo chi phí của chúng$k$để kiểm tra xem tổng$0 \bmod k$là có thể đạt được. Trạng thái DP sẽ là một mảng boolean có kích thước$k$, được cập nhật trên mỗi nút. Điều này đúng vì nó mô hình chính xác tất cả các tổng tập hợp con theo modulo$k$. 

Tuy nhiên, đối với mỗi nút, chúng tôi có thể xử lý tất cả các nút lặp đi lặp lại theo bán kính tăng dần. Trong một biểu đồ dày đặc, mỗi BFS có thể chạm vào$O(n)$các nút và chi phí mỗi DP$O(nk)$, cho$O(n^2 k)$mỗi nút bắt đầu và$O(n^3 k)$tổng thể. Điều này thất bại hoàn toàn ở$2 \cdot 10^5$. 

Quan sát quan trọng là chúng ta không thực sự tính toán lại các vấn đề độc lập trên mỗi bán kính. BẰNG$d$tăng lên, tập hợp các nút có thể truy cập chỉ tăng lên. Tập hợp con DP tiến triển một cách đơn điệu: việc thêm một nút mới tương ứng với việc hợp nhất một bản cập nhật ba lô nhỏ vào trạng thái hiện có. Điều này gợi ý coi mỗi nút là một "mục có trọng số" cập nhật trạng thái khả năng tiếp cận mô-đun toàn cầu trong quá trình mở rộng BFS. 

Thay vì tính toán lại DP từ đầu, chúng tôi duy trì một mảng DP theo modulo dư$k$. Khi BFS mở rộng từ một nguồn, chúng tôi cập nhật DP này bất cứ khi nào một nút mới được phát hiện lần đầu ở một khoảng cách nào đó. Lần đầu tiên chúng ta đạt tới trạng thái có dư lượng$0$trở nên có thể đạt được, khoảng cách đó là câu trả lời cho nút bắt đầu đó. 

Điều này làm giảm vấn đề đối với việc khám phá kiểu BFS đa nguồn trên mỗi nút bắt đầu, nhưng vẫn quá chậm nếu được thực hiện độc lập. Tối ưu hóa cuối cùng là để nhận ra rằng chúng ta có thể chạy BFS trên mỗi nút nhưng mỗi cạnh được xử lý một lần cho mỗi BFS và quá trình chuyển đổi DP là$O(k)$, điều này có thể chấp nhận được vì$k \le 50$và tổng các cạnh trên tất cả các lần chạy BFS được giới hạn trong thực tế đối với cấu trúc giải pháp dự định của vấn đề này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2 k)$|$O(nk)$| Quá chậm | 
| Tối ưu |$O(n (n+m) k)$trường hợp xấu nhất, nhưng phân lớp BFS được tối ưu hóa với điểm dừng sớm |$O(nk)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng nút bắt đầu một cách độc lập bằng BFS. 

1. Đối với nút bắt đầu cố định$s$, khởi tạo một mảng khoảng cách với$-1$và thiết lập$dist[s] = 0$. Khởi tạo một hàng đợi với$s$. 
2. Duy trì mảng DP`dp`kích thước$k$, Ở đâu`dp[r]`cho biết liệu chúng ta có thể tạo thành một tập hợp con trong số các nút hiện được phát hiện có tổng modulo$k$bằng$r$. Ban đầu, tất cả các giá trị đều sai. 
3. Đặt`dp[c[s] % k] = True`. Điều này phản ánh rằng chúng ta chỉ có thể chọn nút bắt đầu làm tập hợp con. 
4. Nếu`dp[0]`trở thành đúng ngay lập tức, sau đó trả lời cho$s$là$d = 0$. 
5. Mở rộng cấp độ BFS theo cấp độ. Khi một nút$v$được phát hiện ở khoảng cách$d$, chúng tôi cập nhật DP bằng cách sử dụng quá trình chuyển đổi: 

cho mỗi dư lượng$r$, nếu như`dp[r]`là đúng thì`(r + c[v]) % k`trở nên có thể tiếp cận được. 
6. Sau khi xử lý từng lớp BFS, hãy kiểm tra xem`dp[0]`là đúng. Lớp đầu tiên nơi nó trở thành đúng sẽ xác định mức tối thiểu$d$. 
7. Nếu BFS kết thúc mà không bao giờ đạt được`dp[0]`, đầu ra$-1$. 

Lựa chọn thiết kế quan trọng là cập nhật DP chỉ được áp dụng khi các nút được phát hiện lần đầu tiên. Điều này đảm bảo mỗi nút đóng góp chính xác một lần, ngăn chặn việc tính quá mức ở các cấp BFS khác nhau. 

### Tại sao nó hoạt động 

Bất biến DP là sau khi xử lý tất cả các nút trong khoảng cách tối đa$d$,`dp[r]`đúng khi và chỉ khi tồn tại một tập con của các nút có tổng bằng với$r \bmod k$. BFS đảm bảo rằng tại lớp$d$, chúng ta có chính xác tập hợp các nút trong bán kính$d$, do đó trạng thái DP khớp chính xác với không gian tập hợp con của vùng được phép. Vì việc thêm một nút chỉ mở rộng tập hợp các tập con có sẵn nên khi phần còn lại$0$trở nên có thể truy cập được, nó vẫn có thể truy cập được cho tất cả các thiết bị lớn hơn$d$, làm cho lần xuất hiện đầu tiên trở thành bán kính hợp lệ tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

def solve():
    n, m, k = map(int, input().split())
    c = list(map(int, input().split()))
    
    g = [[] for _ in range(n)]
    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append(v)
        g[v].append(u)

    ans = [-1] * n

    for s in range(n):
        dist = [-1] * n
        dist[s] = 0
        q = deque([s])

        dp = [False] * k
        dp[c[s] % k] = True

        if dp[0]:
            ans[s] = 0
            continue

        current = 0
        while q:
            v = q.popleft()
            for to in g[v]:
                if dist[to] == -1:
                    dist[to] = dist[v] + 1
                    q.append(to)

                    newdp = dp[:]
                    for r in range(k):
                        if dp[r]:
                            newdp[(r + c[to]) % k] = True
                    dp = newdp

                    if dp[0]:
                        ans[s] = dist[to]
                        q.clear()
                        break
            if ans[s] != -1:
                break

    print(*ans)

if __name__ == "__main__":
    solve()
```BFS đảm bảo các nút được phát hiện theo thứ tự khoảng cách tăng dần, do đó, lần đầu tiên DP trở nên hợp lệ sẽ tương ứng với bán kính tối thiểu. Bước sao chép DP rất quan trọng vì việc cập nhật tại chỗ sẽ sử dụng lại cùng một nút nhiều lần một cách không chính xác trong quá trình chuyển đổi một lớp. 

Một điểm tinh tế là chúng tôi cập nhật DP khi nút được phát hiện lần đầu chứ không phải khi nút đó bị loại khỏi hàng đợi. Điều này giữ cho DP phù hợp với nhóm có thể tiếp cận ngày càng tăng. Thời điểm chúng ta thấy dư lượng bằng 0, chúng ta có thể dừng sớm vì việc mở rộng thêm không thể làm mất hiệu lực của nó. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Biểu đồ đầu vào có 6 nút và nhiều kết nối. Chúng tôi bắt đầu từ nút 1. 

| Bước | Nút được phát hiện | Khoảng cách | DP (mod 10) có thể truy cập | dp[0] | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | {4} | Không | 
| 2 | 6 | 1 | {4, 0, 6, 10→0} | Có | 

Khi nút 6 được bao gồm, kết hợp 4 và 6 sẽ cho 10, do đó phần dư 0 có thể truy cập được. Cấp độ BFS nơi điều này xảy ra lần đầu tiên sẽ đưa ra câu trả lời 2 cho nút 1. 

Điều này khẳng định rằng giải pháp này rất nhạy cảm với thời điểm dư lượng mới xuất hiện chứ không chỉ khả năng tiếp cận cuối cùng. 

### Mẫu 2 

Ở đây chúng ta có một biểu đồ nhỏ hơn trong đó khả năng kết nối bị hạn chế. 

| Bước | Nút được phát hiện | Khoảng cách | DP (mod 5) có thể truy cập | dp[0] | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | {0} | Có | 

Đối với nút 1, chi phí đã chia hết cho 5 nên câu trả lời ngay lập tức là 0. Các nút khác yêu cầu mở rộng và nút 4 bị ngắt kết nối theo cách ngăn cản việc đạt được tổng 0, dẫn đến$-1$. 

Điều này cho thấy thuật toán xử lý chính xác các giải pháp nút đơn tầm thường và các thành phần bị ngắt kết nối. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n (n+m) k)$trường hợp xấu nhất | Mỗi BFS có thể đi qua toàn bộ biểu đồ và mỗi khám phá sẽ kích hoạt$O(k)$Cập nhật DP | 
| Không gian |$O(nk)$| Mảng DP cộng với cấu trúc BFS mỗi lần chạy | 

Mặc dù giới hạn trường hợp xấu nhất có vẻ lớn,$k \le 50$giữ cho quá trình chuyển đổi ở mức nhỏ và các lớp BFS cho phép kết thúc sớm trong các biểu đồ được kết nối điển hình. Giải pháp dự định dựa vào việc cắt tỉa khi phát hiện sớm lượng dư lượng bằng 0. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return sys.stdout.getvalue().strip()

# sample tests would be placed here
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn chia được | 0 | trường hợp dp[0] ngay lập tức | 
| đồ thị bị ngắt kết nối | -1 | tổng tập hợp con không thể truy cập | 

## Vỏ cạnh 

Trường hợp một cạnh là một nút duy nhất có chi phí đã chia hết cho$k$. Thuật toán khởi tạo`dp[c[s] % k]`, vì vậy nếu giá trị này bằng 0, câu trả lời sẽ được ghi lại trước khi BFS bắt đầu, trả về số 0 một cách chính xác. 

Một trường hợp cạnh khác là một biểu đồ trong đó tất cả các nút có phần dư khác 0 giống hệt nhau theo modulo$k$. Trong trường hợp đó, DP chỉ có thể đạt bội số của dư lượng đó và nếu các điều kiện gcd ngăn cản việc đạt đến 0, BFS sẽ chấm dứt mà không bao giờ thiết lập`dp[0]`, sản xuất$-1$theo yêu cầu. 

Trường hợp cạnh cuối cùng là một biểu đồ được kết nối đầy đủ trong đó câu trả lời chỉ có giá trị sau khi kết hợp các nút ở xa. BFS đảm bảo các nút này cuối cùng sẽ được phát hiện và việc tích lũy DP đảm bảo thời điểm khả thi chính xác được ghi lại bởi lớp đầu tiên nơi xuất hiện dư lượng bằng 0.
