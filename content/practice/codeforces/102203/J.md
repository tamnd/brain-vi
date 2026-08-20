---
title: "CF 102203J - \u041d\u043e\u0447\u043d\u043e\u0439 \u043f\u0430\u0442\u0440\u0443\u043b\u044c"
description: "Chúng tôi có một biểu đồ có trọng số có hướng với tối đa 300 giao điểm. Mỗi đường có hướng đều có thời gian đi qua dương. Hai nhân viên tuần tra xuất phát tại giao lộ s1 và s2. Họ phải kiểm tra dãy p1, p2, ..., pk theo đúng thứ tự này."
date: "2026-08-18T00:52:01+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102203
codeforces_index: "J"
codeforces_contest_name: "\u0427\u0435\u0442\u0432\u0435\u0440\u0442\u0430\u044f \u041b\u0438\u043f\u0435\u0446\u043a\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e (8-11 \u043a\u043b\u0430\u0441\u0441\u044b)"
rating: 0
weight: 102203
solve_time_s: 131
verified: true
draft: false
---

[CF 102203J - \u041d\u043e\u0447\u043d\u043e\u0439 \u043f\u0430\u0442\u0440\u0443\u043b\u044c](https://codeforces.com/problemset/problem/102203/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 11s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một biểu đồ có trọng số có hướng với tối đa 300 giao điểm. Mỗi đường có hướng đều có thời gian đi qua dương. Hai sĩ quan tuần tra xuất phát ở ngã tư`s1`Và`s2`. 

Họ phải kiểm tra trình tự`p1, p2, ..., pk`theo đúng thứ tự này. Đối với mỗi cuộc kiểm tra bắt buộc, một trong hai viên chức có thể là người thực hiện việc đó. Sau khi cảnh sát đến giao lộ cần thiết, cuộc kiểm tra tiếp theo có thể bắt đầu. Cảnh sát được phép đứng ở cùng một ngã tư và nếu một cảnh sát đã có mặt ở ngã tư cần kiểm tra thì việc kiểm tra đó không mất thời gian di chuyển. 

Nhiệm vụ là tìm tổng thời gian tối thiểu có thể cho đến khi`pk`đã được kiểm tra. Nếu một số chuyển đổi bắt buộc là không thể thực hiện được thì câu trả lời là`-1`. 

Vấn đề đồ thị đầu tiên ẩn bên trong câu lệnh là đường đi ngắn nhất. Bất cứ khi nào cảnh sát phải di chuyển khỏi ngã tư`u`đến giao lộ`v`, chỉ có thời gian di chuyển ngắn nhất từ`u`ĐẾN`v`vấn đề. Vì đồ thị có hướng nên khoảng cách từ`u`ĐẾN`v`không nhất thiết là khoảng cách từ`v`ĐẾN`u`. 

Giới hạn đủ nhỏ trong`n`để cho phép tính toán đường đi ngắn nhất cho tất cả các cặp trong`O(n^3)`. Với`n <= 300`, tức là nhiều nhất là 27 triệu hoạt động thư giãn. Mặt khác,`k`có thể đạt tới 1000, do đó, một DP có chuyển đổi bậc hai cho mỗi giao lộ bắt buộc sẽ đạt khoảng 90 triệu chuyển đổi trạng thái và việc phân công mạnh mẽ mọi điểm kiểm tra cho một trong hai sĩ quan sẽ yêu cầu`2^1000`khả năng, điều đó hoàn toàn không thể thực hiện được. 

Có một số trường hợp nghiêm trọng mà giải pháp bất cẩn có thể xử lý sai. Đầu tiên là khi không có đường nào cả nhưng các nút giao thông cần thiết đã bị chiếm dụng. Ví dụ:```
2 0 31 1 21 2
```Sĩ quan thứ nhất bắt đầu lúc 1 và người thứ hai lúc 2. Trình tự bắt buộc là`1, 1, 2`, vì vậy mọi cuộc kiểm tra đều có thể được thực hiện bởi một sĩ quan đã đứng ở ngã tư bên phải. Câu trả lời là`0`. Một giải pháp giả định rằng mỗi cặp trạm kiểm soát liên tiếp đều cần một con đường thực tế sẽ quay trở lại không chính xác`-1`. 

Một trường hợp tinh tế khác là một điểm kiểm tra lặp đi lặp lại. Vì```
1 0 41 1 1 11 1
```câu trả lời là`0`. Việc kiểm tra lặp đi lặp lại không yêu cầu phải di chuyển khi đã có nhân viên ở đó. Việc coi các đỉnh liên tiếp bằng nhau là một quá trình chuyển đổi không thể thực hiện được sẽ là sai lầm. 

Hai sĩ quan cũng có thể gặp nhau. Ví dụ:```
2 1 21 2 51 21 1
```Lần kiểm tra đầu tiên đã được đáp ứng ở đỉnh 1, sau đó một sĩ quan đi đến đỉnh 2 vào thời điểm 5, vì vậy câu trả lời là`5`. Một DP khẳng định hai sĩ quan phải chiếm giữ các đỉnh khác nhau sẽ từ chối trạng thái hoàn toàn hợp lệ. 

Cuối cùng, sự chỉ đạo là quan trọng. Coi như:```
2 1 21 2 72 11 1
```Lần kiểm tra đầu tiên ở mức 2 tốn 7, nhưng việc quay lại từ 2 về 1 là không thể. Câu trả lời là`-1`. Việc thay đồ thị có hướng bằng đồ thị vô hướng sẽ tạo ra câu trả lời hữu hạn không chính xác. 

## Phương pháp tiếp cận 

Đối với mỗi trạm kiểm soát được yêu cầu, giải pháp vũ lực trực tiếp có thể quyết định xem ai trong hai sĩ quan thực hiện nó. có`2^k`những nhiệm vụ như vậy. Sau khi ấn định nhiệm vụ, lộ trình của mỗi sĩ quan hoàn toàn được xác định bởi các trạm kiểm soát được chỉ định cho sĩ quan đó và mọi chuyển động có thể được thay thế bằng khoảng cách đường đi ngắn nhất. Như vậy phép liệt kê là đúng, nhưng trong trường hợp xấu nhất nó sẽ kiểm tra`2^1000`nhiệm vụ, mỗi nhiệm vụ yêu cầu tối đa`O(k)`công việc. Đây là đại khái`O(k * 2^k)`, vượt xa những gì có thể. 

Một cách tiếp cận hứa hẹn hơn là lập trình động. Sau trạm kiểm soát`pi`đã được kiểm tra, chắc chắn có một sĩ quan đang đứng ở`pi`, cụ thể là viên chức vừa thực hiện cuộc kiểm tra đó. Cần có thông tin gì về viên chức khác? Chỉ giao lộ hiện tại của nó. Mọi thứ trước đây`pi`đã đóng góp chi phí của nó vào giá trị DP. 

Điều này đưa ra một trạng thái bao gồm chỉ mục`i`và đỉnh của sĩ quan kia`x`. Lúc đầu điều này dường như tạo ra`O(k n)`trạng thái, nhưng một quá trình chuyển đổi bất cẩn có thể so sánh mọi trạng thái cũ có thể`x`với mọi trạng thái mới có thể, tạo ra`O(k n^2)`công việc. Quan sát quan trọng là từ một trạng thái`(i, x)`chỉ có hai lựa chọn có ý nghĩa cho lần kiểm tra tiếp theo. 

Viên chức hiện đang ở`pi`có thể kiểm tra`p(i+1)`. Trong trường hợp đó viên chức còn lại vẫn ở`x`. 

Ngoài ra, viên chức khác có thể kiểm tra`p(i+1)`. Trong trường hợp đó viên chức hiện đang ở`pi`trở thành sĩ quan không tại ngũ nên chức vụ mới kia chính xác là`pi`. 

Quá trình chuyển đổi thứ hai đặc biệt hữu ích vì trạng thái đích của nó luôn giống nhau đối với mọi chuyển đổi cũ.`x`. Chúng ta không bao giờ cần phải xem xét các cặp vị trí cũ và mới tùy ý. 

Do đó, sau khi tính toán đường đi ngắn nhất của tất cả các cặp, DP chỉ mất`O(k n)`thời gian. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(k * 2^k)`sau những con đường ngắn nhất |`O(n^2)`| Quá chậm | 
| DP + Floyd-Warshall |`O(n^3 + k n)`|`O(n^2)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính khoảng cách đường đi ngắn nhất`dist[u][v]`giữa mỗi cặp giao lộ sử dụng Floyd-Warshall. Ban đầu,`dist[u][u] = 0`, mỗi con đường có hướng đều góp phần vào thời gian di chuyển của nó và những cặp đường không thể tiếp cận có khoảng cách vô tận. Bởi vì mọi con đường đều có trọng số dương, nên các đường đi ngắn nhất có thể được xử lý trực tiếp bằng cách nới lỏng tiêu chuẩn. 
2. Xác định`dp[x]`sau khi xử lý điểm kiểm tra`pi`là thời gian trôi qua tối thiểu khi một sĩ quan ở`pi`và viên sĩ quan kia đang ở ngã tư`x`. Chúng ta không cần phải nhớ sĩ quan nào là sĩ quan nào. Viên chức ở`pi`được gọi đơn giản là sĩ quan tích cực. 
3. Khởi tạo DP cho`p1`. Hoặc viên chức bắt đầu từ`s1`đạt tới`p1`, để viên sĩ quan kia ở lại`s2`, hoặc viên chức bắt đầu từ`s2`đạt tới`p1`, để viên sĩ quan kia ở lại`s1`. Do đó, hai trạng thái có thể là:`dp[s2] = dist[s1][p1]`Và`dp[s1] = dist[s2][p1]`. 

Nếu hai khả năng dẫn đến cùng một trạng thái, chúng ta giữ chi phí nhỏ hơn. 
4. Với mỗi cặp liên tiếp`pi`,`p(i+1)`, cho phép`next = p(i+1)`. Từ một tiểu bang`dp[x]`, trước tiên hãy cân nhắc việc để sĩ quan tại ngũ chuyển từ`pi`ĐẾN`next`. Trạng thái mới vẫn`x`, với chi phí bổ sung`dist[pi][next]`. 
5. Cũng nên cân nhắc việc để sĩ quan ngừng hoạt động chuyển từ`x`ĐẾN`next`. Sĩ quan tại ngũ mới bây giờ là người trước đây không hoạt động, trong khi sĩ quan tại ngũ cũ vẫn giữ nguyên`pi`. Do đó trạng thái mới là`pi`, với chi phí bổ sung`dist[x][next]`. 
6. Thay thế mảng DP hiện tại bằng mảng DP mới được tính toán. Bất kỳ quá trình chuyển đổi nào liên quan đến khoảng cách đường đi ngắn nhất vô hạn đều bị bỏ qua vì nhân viên tương ứng không thể thực hiện việc kiểm tra đó. 
7. Sau khi xử lý`pk`, mọi hữu hạn`dp[x]`đại diện cho một cách hợp lệ để hoàn thành toàn bộ trình tự kiểm tra. Lấy mức tối thiểu trên tất cả`x`. Nếu mọi trạng thái đều là vô hạn thì xuất ra`-1`. 

### Tại sao nó hoạt động 

Điều bất biến là sau khi xử lý`pi`,`dp[x]`lưu trữ thời gian tối thiểu có thể có trong số tất cả các lịch trình hợp lệ có tiền tố được kiểm tra kết thúc bằng một sĩ quan tại`pi`và cái khác tại`x`. 

Đến điểm kiểm tra tiếp theo, đúng một trong hai nhân viên thực hiện việc kiểm tra. Nếu viên chức ở`pi`thực hiện nó, quá trình chuyển đổi đầu tiên sẽ xem xét lịch trình đó và rời khỏi`x`không thay đổi. Nếu viên chức ở`x`thực hiện nó, quá trình chuyển đổi thứ hai sẽ xem xét lịch trình đó và để viên chức kia ở lại`pi`. Đây là hai lựa chọn duy nhất có thể thực hiện được, vì vậy mọi lịch trình hợp lệ đều có một quá trình chuyển đổi DP tương ứng. 

Ngược lại, mọi chuyển đổi DP đều đi theo một đường dẫn thực sự ngắn nhất trong biểu đồ và tạo ra cấu hình hợp lệ sau lần kiểm tra tiếp theo. Vì DP giữ chi phí rẻ nhất cho mọi cấu hình có thể, nên việc quy nạp theo trình tự điểm kiểm tra chứng tỏ rằng mức tối thiểu cuối cùng chính xác là khoảng thời gian tuần tra tối ưu. 

## Giải pháp Python```python
Pythonimport sysinput = sys.stdin.readline

def solve():    n, m, k = map(int, input().split())
    INF = 10**30    dist = [[INF] * n for _ in range(n)]
    for i in range(n):        dist[i][i] = 0
    for _ in range(m):        v, u, t = map(int, input().split())        v -= 1        u -= 1        if t < dist[v][u]:            dist[v][u] = t
    p = [x - 1 for x in map(int, input().split())]    s1, s2 = map(lambda x: x - 1, map(int, input().split()))
    # Floyd-Warshall.    for mid in range(n):        dmid = dist[mid]        for u in range(n):            du = dist[u]            if du[mid] == INF:                continue            base = du[mid]            for v in range(n):                nd = base + dmid[v]                if nd < du[v]:                    du[v] = nd
    # dp[x]:    # one patrol is at p[i], the other is at x.    dp = [INF] * n
    first = p[0]
    if dist[s1][first] < INF:        dp[s2] = min(dp[s2], dist[s1][first])
    if dist[s2][first] < INF:        dp[s1] = min(dp[s1], dist[s2][first])
    for i in range(k - 1):        cur = p[i]        nxt = p[i + 1]
        move_active = dist[cur][nxt]        ndp = [INF] * n
        for other in range(n):            cur_cost = dp[other]            if cur_cost == INF:                continue
            # The patrol currently at cur handles nxt.            if move_active < INF:                value = cur_cost + move_active                if value < ndp[other]:                    ndp[other] = value
            # The other patrol handles nxt.            move_other = dist[other][nxt]            if move_other < INF:                value = cur_cost + move_other                if value < ndp[cur]:                    ndp[cur] = value
        dp = ndp
    answer = min(dp)    print(-1 if answer == INF else answer)

if __name__ == "__main__":    solve()
```Ma trận khoảng cách được khởi tạo bằng 0 trên đường chéo vì sĩ quan có thể kiểm tra trạm kiểm soát mà không cần di chuyển khi đã ở đó. Nhiều đường có hướng giữa cùng một cặp được xử lý bằng cách chỉ giữ lại trọng số cạnh nhỏ nhất, mặc dù câu lệnh không yêu cầu phải vắng mặt các cạnh trùng lặp. 

Vòng lặp Floyd-Warshall sử dụng`mid`làm đỉnh trung gian. Séc`du[mid] == INF`tránh những công việc không cần thiết khi`u`không thể với tới`mid`. giá trị`10**30`lớn hơn bất kỳ câu trả lời hữu hạn nào có thể có, vì đường đi ngắn nhất đơn giản sử dụng nhiều nhất`n - 1`các cạnh và chi phí mỗi cạnh nhiều nhất`10^6`. 

DP chứa một trạng thái cho mọi vị trí có thể có của sĩ quan hiện không đứng ở trạm kiểm soát mới nhất. Khi sĩ quan đang hoạt động di chuyển, vị trí không hoạt động vẫn không thay đổi. Khi sĩ quan không hoạt động di chuyển, địa điểm hoạt động cũ sẽ trở thành địa điểm không hoạt động mới, điều này giải thích tại sao chỉ số đích là`cur`còn hơn là`nxt`. 

Việc khởi tạo phải xét đến cả sĩ quan xuất phát. Chỉ lựa chọn`s1`sẽ bỏ lỡ lịch trình nơi sĩ quan thứ hai đến`p1`Đầu tiên. Lý do tương tự cũng áp dụng cho mọi trạm kiểm soát sau này, nơi cả hai sĩ quan phải được coi là những người có thể thực hiện. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là:```
5 0 55 5 4 4 55 4
```Không có đường nên khoảng cách hữu hạn duy nhất bằng 0 từ một đỉnh đến chính nó. Các sĩ quan bắt đầu ở đỉnh 5 và 4, khớp chính xác với các đỉnh cần thiết theo trình tự. 

Sau trạm kiểm soát đầu tiên, sĩ quan thứ nhất có thể kiểm tra đỉnh 5 ngay lập tức, để lại đỉnh 4. 

| Điểm kiểm tra | Vị trí hoạt động | Vị trí khác | Chi phí DP | 
| --- | --- | --- | --- | 
|`p1 = 5`| 5 | 4 | 0 | 
|`p2 = 5`| 5 | 4 | 0 | 
|`p3 = 4`| 4 | 5 | 0 | 
|`p4 = 4`| 4 | 5 | 0 | 
|`p5 = 5`| 5 | 4 | 0 | 

Mỗi lần chuyển đổi đều không tốn chi phí vì nhân viên cần thiết cho lần kiểm tra tiếp theo đã có mặt ở đó. Câu trả lời cuối cùng là`0`. 

Điều này chứng tỏ tại sao các trạm kiểm soát liên tiếp bằng nhau và việc không có đường không tự động bị lỗi. 

### Mẫu 2 

Đầu vào là:```
5 4 41 5 35 1 101 2 12 3 25 1 2 31 2
```Khoảng cách ngắn nhất hữu ích là:```
dist[1][5] = 3dist[5][1] = 10dist[2][3] = 2
```Điểm kiểm tra đầu tiên là đỉnh 5. Sĩ quan ở đỉnh 1 đến đó trong 3 đơn vị, trong khi sĩ quan còn lại vẫn ở đỉnh 2. 

| Điểm kiểm tra | Vị trí hoạt động | Vị trí khác | Chi phí tối thiểu | 
| --- | --- | --- | --- | 
|`p1 = 5`| 5 | 2 | 3 | 
|`p2 = 1`| 1 | 5 | 13 | 
|`p3 = 2`| 2 | 1 | 14 | 
|`p4 = 3`| 3 | 1 | 16 | 

Vì`p2`, viên chức ở số 5 phải quay về 1, giá 10. Điều này tạo ra chi phí`13`. Sau đó, sĩ quan khác, đã ở tuổi 2, xử lý`p3`để không có chuyển động bổ sung. Cuối cùng, viên sĩ quan đó đi từ 2 đến 3 trong 2 đơn vị. 

Tổng cộng là`3 + 10 + 2 = 15`, không phải 16 trong bảng trên nếu chúng ta theo dõi trạng thái tối ưu một cách chính xác. DP có liên quan sau`p2`thực chất là bang có sĩ quan tại ngũ lúc 1 và người kia là 2, giá là 13, vì sau khi đạt 1 thì sĩ quan bắt đầu lúc 2 vẫn ở mức 2. Khi đó`p3 = 2`được nhân viên kia xử lý với chi phí bằng 0, tạo ra vị trí hoạt động 2, vị trí khác 1, với chi phí 13. Cuối cùng, việc di chuyển từ 2 lên 3 tốn 2, cho 15. 

Dấu vết đã sửa là: 

| Điểm kiểm tra | Vị trí hoạt động | Vị trí khác | Chi phí tối thiểu | 
| --- | --- | --- | --- | 
|`p1 = 5`| 5 | 2 | 3 | 
|`p2 = 1`| 1 | 2 | 13 | 
|`p3 = 2`| 2 | 1 | 13 | 
|`p4 = 3`| 3 | 1 | 15 | 

Dấu vết này minh họa ý tưởng DP trung tâm. Vị trí của sĩ quan thứ hai vẫn tồn tại qua nhiều cuộc kiểm tra mà không thay đổi, cho phép nó tiếp quản sau này. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n^3 + k n)`| Floyd-Warshall đảm nhận`O(n^3)`và mọi quá trình chuyển đổi điểm kiểm tra sẽ quét tất cả`n`Trạng thái DP | 
| Không gian |`O(n^2)`| Ma trận đường đi ngắn nhất chiếm ưu thế trong cả hai`O(n)`mảng DP | 

Với`n <= 300`, giai đoạn đường đi ngắn nhất tất cả các cặp có tối đa 27 triệu lần thư giãn cơ bản. DP có nhiều nhất`1000 * 300 = 300,000`các chuyển đổi trạng thái. Việc sử dụng bộ nhớ bị chi phối bởi`300 x 300`ma trận khoảng cách, dễ dàng trong phạm vi 256 MB. 

## Trường hợp thử nghiệm```python
Pythonimport sysimport io

def solve():    input = sys.stdin.readline
    n, m, k = map(int, input().split())
    INF = 10**30    dist = [[INF] * n for _ in range(n)]
    for i in range(n):        dist[i][i] = 0
    for _ in range(m):        v, u, t = map(int, input().split())        v -= 1        u -= 1        if t < dist[v][u]:            dist[v][u] = t
    p = [x - 1 for x in map(int, input().split())]    s1, s2 = map(int, input().split())    s1 -= 1    s2 -= 1
    for mid in range(n):        dmid = dist[mid]        for u in range(n):            du = dist[u]            if du[mid] == INF:                continue            base = du[mid]            for v in range(n):                nd = base + dmid[v]                if nd < du[v]:                    du[v] = nd
    dp = [INF] * n    first = p[0]
    if dist[s1][first] < INF:        dp[s2] = min(dp[s2], dist[s1][first])
    if dist[s2][first] < INF:        dp[s1] = min(dp[s1], dist[s2][first])
    for i in range(k - 1):        cur = p[i]        nxt = p[i + 1]        move_active = dist[cur][nxt]        ndp = [INF] * n
        for other in range(n):            cost = dp[other]            if cost == INF:                continue
            if move_active < INF:                value = cost + move_active                if value < ndp[other]:                    ndp[other] = value
            move_other = dist[other][nxt]            if move_other < INF:                value = cost + move_other                if value < ndp[cur]:                    ndp[cur] = value
        dp = ndp
    answer = min(dp)    print(-1 if answer == INF else answer)

def run(inp: str) -> str:    old_stdin = sys.stdin    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)    sys.stdout = io.StringIO()
    try:        solve()        return sys.stdout.getvalue().strip()    finally:        sys.stdin = old_stdin        sys.stdout = old_stdout

# Provided sample 1assert run("""5 0 55 5 4 4 55 4""") == "0", "sample 1"
# Provided sample 2assert run("""5 4 41 5 35 1 101 2 12 3 25 1 2 31 2""") == "15", "sample 2"
# Minimum-size graph, repeated checkpoint, both officers already there.assert run("""1 0 51 1 1 1 11 1""") == "0", "minimum-size repeated vertex"
# Directed reachability: the required second movement is impossible.assert run("""2 1 21 2 72 11 1""") == "-1", "directed unreachable transition"
# Both officers may use the same vertex, and the best strategy changes which# officer is active.assert run("""3 3 41 2 52 3 23 1 11 2 3 11 1""") == "8", "officers can meet and swap roles"
# Multiple edges between the same vertices, the smaller one must be used.assert run("""3 4 31 2 1001 2 42 3 61 3 201 2 31 1""") == "10", "parallel edges and shortest path"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0 5`, tất cả các điểm kiểm tra đều bằng 1 |`0`| Kích thước biểu đồ tối thiểu và kiểm tra lặp lại không tốn chi phí | 
|`2 1 2`, chỉ một`1 -> 2`tồn tại |`-1`| Khả năng tiếp cận được định hướng và chuyển đổi không thể tiếp cận | 
|`3 3 4`, đồ thị tuần hoàn |`8`| Cán bộ có thể gặp và thay đổi cán bộ thực hiện đợt kiểm tra tiếp theo | 
|`3 4 3`, song song`1 -> 2`cạnh |`10`| Chọn cạnh song song nhỏ nhất và sử dụng đường đi ngắn nhất trung gian | 

## Vỏ cạnh 

Khi mọi trạm kiểm soát bắt buộc đều đã kín người thì không cần đường nữa. Trong ví dụ kích thước tối thiểu```
1 0 51 1 1 1 11 1
```việc khởi tạo tạo ra trạng thái chi phí bằng 0 vì`dist[1][1] = 0`. Mỗi lần chuyển đổi sau này cũng không có chi phí. Câu trả lời là`0`, xuất phát trực tiếp từ thực tế là việc kiểm tra không yêu cầu di chuyển nếu cảnh sát đã có mặt ở ngã tư đó. 

Các điểm kiểm tra lặp lại được xử lý theo cùng một đường chéo 0 trong ma trận khoảng cách. Nếu sĩ quan tại ngũ đã có mặt tại`pi`Và`p(i+1) = pi`, sau đó`dist[pi][p(i+1)] = 0`. Việc chuyển đổi sĩ quan tại ngũ giữ nguyên vị trí của sĩ quan kia và không tăng thêm chi phí. 

Đối với các chuyển đổi không thể truy cập được, vô cực sẽ ngăn lịch trình không hợp lệ vào DP. TRONG```
2 1 21 2 72 11 1
```Việc kiểm tra đầu tiên có thể được thực hiện bởi một trong hai nhân viên sau khi đạt đến đỉnh 2, nhưng khi vị trí hoạt động là 2 thì không có đường quay lại 1. Nếu nhân viên kia đã ở đỉnh 1, thì DP có thể để nhân viên đó thực hiện lần kiểm tra thứ hai, việc này phải được xem xét. Nếu không có cấu hình nào cho phép thứ tự yêu cầu thì tất cả các trạng thái cuối cùng vẫn là vô hạn và câu trả lời là`-1`. 

Các sĩ quan được phép chiếm giữ cùng một ngã tư đương nhiên được đại diện bởi vì`dp[x]`không có hạn chế về`x`bằng với vị trí hoạt động. The state describes locations, not distinct vertices. This also handles situations where one officer catches up with the other and later takes responsibility for an inspection.

 Parallel directed edges require taking the minimum edge weight. Ví dụ, nếu cả hai`1 -> 2`với chi phí 100 và 4 tồn tại, việc sử dụng 100 làm mục nhập ma trận sẽ khiến mọi đường đi ngắn nhất tiếp theo trở nên đắt đỏ một cách không cần thiết. Việc khởi tạo sử dụng`min(dist[v][u], t)`, do đó Floyd-Warshall bắt đầu từ biểu đồ đúng. 

Cuối cùng, chi phí đường đi lớn đòi hỏi một số nguyên đủ lớn. Số nguyên Python không bị tràn mà sử dụng một trọng điểm như`10**30`vẫn làm cho việc kiểm tra trạng thái không thể truy cập trở nên rõ ràng và giữ cho các phần bổ sung được tách biệt một cách an toàn khỏi các câu trả lời hữu hạn.
