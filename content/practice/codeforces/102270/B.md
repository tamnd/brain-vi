---
title: "CF 102270B - Nghiền nát"
description: "Chúng tôi được tặng một cây máy ảnh. Mỗi camera có một nhãn nhị phân. Nhãn 1 có nghĩa là việc truy cập vào camera đó yêu cầu phải phá mật khẩu, việc này tốn một đơn vị năng lượng, trong khi nhãn 0 không tốn gì cả."
date: "2026-08-17T18:31:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102270
codeforces_index: "B"
codeforces_contest_name: "HCW 19 Individual Day 2"
rating: 0
weight: 102270
solve_time_s: 434
verified: false
draft: false
---

[CF 102270B - Nghiền nát](https://codeforces.com/problemset/problem/102270/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 7 phút 14s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một cây máy ảnh. Mỗi camera có một nhãn nhị phân. Một nhãn của`1`có nghĩa là việc truy cập vào camera đó yêu cầu phải phá mật khẩu, việc này tiêu tốn một đơn vị năng lượng, trong khi nhãn của`0`không tốn kém gì. Chúng ta cần đếm các bộ camera được kết nối mà tổng số camera được chọn được gắn nhãn`1`chính xác là`K`. 

Từ "kết nối" là điều kiện cấu trúc quan trọng. Vì biểu đồ là một cây nên tập hợp đã chọn được kết nối chính xác khi mỗi cặp camera được chọn được nối với nhau bằng một đường dẫn chỉ chứa các camera được chọn. Câu trả lời tính các tập đỉnh khác nhau, không phải các thứ tự khác nhau mà các camera có thể được truy cập. Đây là cách giải thích bài toán đếm cây con được kết nối thông thường. 

Cây có nhiều nhất`50,000`đỉnh, trong khi`K`nhiều nhất là`100`. Việc liệt kê các tập hợp con theo cấp số nhân là không thể ngay lập tức vì một cây có thể có nhiều tập hợp con được kết nối theo cấp số nhân. Một giải pháp lập trình động đơn giản với không giới hạn`K × K`tích chập ở mỗi đỉnh cũng sẽ quá đắt, có khả năng đạt tới hàng trăm triệu lần chuyển đổi. Giới hạn nhỏ trên`K`cho chúng ta biết rằng tiểu bang chỉ nên theo dõi số lượng camera mật khẩu, trong khi nên sử dụng cấu trúc cây để tránh xem xét các tập hợp con tùy ý. 

Có một số trường hợp nghiêm trọng mà việc triển khai bất cẩn có thể dẫn đến xử lý sai. Nếu như`K = 0`, một bộ kết nối hợp lệ chỉ có thể chứa các camera được gắn nhãn`0`. Ví dụ,```
3 0
1 1 0
1 2
2 3
```có câu trả lời`1`, bởi vì bộ kết nối không trống duy nhất có camera không mật khẩu là`{3}`. DP không cần tập hợp trống vì mọi trạng thái được đếm đều chứa đỉnh gốc được chỉ định của nó. 

Trường hợp thứ hai là camera mật khẩu ở gốc của cây con DP. Ví dụ,```
1 1
1
```có câu trả lời`1`, bởi vì người độc thân`{1}`sử dụng đúng một đơn vị năng lượng. Một DP khởi tạo mọi đỉnh có trạng thái`dp[0] = 1`sẽ cho phép một bộ đã chọn có chứa camera mật khẩu không có chi phí. 

Trường hợp cạnh thứ ba xảy ra khi cây con không chứa camera mật khẩu. Ví dụ,```
2 0
0 0
1 2
```có câu trả lời`3`, tương ứng với`{1}`,`{2}`, Và`{1,2}`. Một đứa trẻ như vậy vẫn còn quan trọng vì nó có thể được đưa vào một tập hợp liên thông hợp lệ mà không làm tăng năng lượng. Một DP chỉ lưu trữ số lượng mật khẩu dương sẽ âm thầm mất đi các giải pháp này. 

Cuối cùng, khi`K`lớn hơn số lượng camera mật khẩu trong cây con, trạng thái đó là không thể và không bao giờ được biểu diễn ngoài phạm vi hữu ích. Cắt bớt mọi mảng DP tại`K`vừa đúng vừa cần thiết cho thời gian chạy. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu rất đơn giản về mặt khái niệm. Liệt kê mọi tập hợp con của camera, kiểm tra xem các camera đã chọn có tạo ra một sơ đồ con được kết nối hay không, đếm các camera mật khẩu của nó và thêm một camera khi số lượng đó là`K`. có`2^N`các tập hợp con và thậm chí việc kiểm tra khả năng kết nối cho mỗi tập hợp con sẽ tốn ít nhất thời gian tuyến tính cho mỗi tập hợp con, đưa ra khoảng`O(N 2^N)`hoạt động trong trường hợp xấu nhất. Với`N = 50,000`, thậm chí`2^50`đã vượt xa mọi thứ có thể được xử lý, vì vậy phương pháp này chỉ hữu ích như một cách để hiểu những gì đang được tính. 

Quan sát hữu ích đầu tiên là mọi tập liên thông khác rỗng trong cây có gốc đều có một đỉnh cao nhất duy nhất. Nếu cây có gốc ở đỉnh`1`, lấy đỉnh đã chọn gần gốc nhất và gọi nó`u`. Mọi đỉnh được chọn khác phải nằm trong cây con của`u`. Hơn nữa, đối với mỗi đứa trẻ`v`của`u`, chúng ta có đúng hai khả năng: không lấy gì từ cây con của`v`hoặc lấy một bộ kết nối có chứa`v`. Lựa chọn thứ hai chính xác là vấn đề tương tự trên`v`. 

Điều này mang lại cho một cây DP. Định nghĩa`dp[u][k]`là số tập hợp kết nối có trong cây con gốc của`u`, chứa`u`, và chứa chính xác`k`camera mật khẩu. Trạng thái ban đầu chỉ chứa`u`. Nếu như`u`có mật khẩu, nó đóng góp một vào số đếm; nếu không thì nó đóng góp bằng không. 

Khi xử lý một đứa trẻ`v`, giả sử giải pháp từng phần hiện tại sử dụng`i`camera mật khẩu và bộ phận được kết nối được chọn từ`v`công dụng`j`. Kết hợp chúng tạo ra một giải pháp với`i + j`camera mật khẩu, góp phần`current[i] * dp[v][j]`cách. Chúng ta cũng có quyền lựa chọn không lấy gì từ`v`, vậy là cái cũ`current`mảng phải được bảo tồn. 

Lực lượng vũ phu hoạt động vì mọi tập hợp được kết nối đều có một đỉnh cao nhất duy nhất, nhưng nó không thành công vì nó xem xét rõ ràng nhiều tập hợp theo cấp số nhân. DP chỉ giữ thông tin liên quan đến tương lai, cụ thể là có bao nhiêu camera mật khẩu đã được sử dụng. 

Việc triển khai đơn giản có thể thực hiện đầy đủ`K × K`tích chập cho mọi con của mỗi đỉnh, cho`O(NK^2)`. Với`N = 50,000`Và`K = 100`, giới hạn trên đó là xung quanh`500 million`các kết hợp trạng thái cơ bản, quá lớn cho giới hạn một giây. 

Sự cải thiện cần thiết đến từ việc chỉ giữ mỗi mảng DP miễn là số lượng camera mật khẩu thực sự có thể xuất hiện trong cây con đó, giới hạn ở mức`K`. Một đỉnh có nhãn`0`bắt đầu chỉ với một số có thể, trong khi một đỉnh được gắn nhãn`1`bắt đầu bằng hai. Tổng quát hơn, một cây con chứa`s`máy ảnh mật khẩu cần nhiều nhất`min(s, K) + 1`tiểu bang. Tổng công của việc hợp nhất cây-knapsack này là`O(NK)`được khấu hao. Cách tính toán theo cây ba lô tương tự là điều làm cho việc hợp nhất các cây con trạng thái giới hạn rẻ hơn đáng kể so với việc nhân hai cây đầy đủ.`K`-mảng có độ dài ở mọi đỉnh. 

Có một vấn đề thực tế khác trong Python. Lưu trữ một`K + 1`mảng DP có kích thước cho tất cả`50,000`các đỉnh sẽ tạo ra hàng triệu đối tượng hoặc tài liệu tham khảo Python. Thay vào đó, việc triển khai bên dưới sẽ xử lý các DP con ngay lập tức và giải phóng chúng sau khi hợp nhất vào DP gốc. Nút cha giữ DP hiện tại của nó, trong khi nút sẽ sẵn sàng ngay sau khi tất cả các nút con của nó đã hoàn thành. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(N 2^N)`|`O(N)`| Quá chậm | 
| Đầy`K × K`cây DP |`O(NK^2)`|`O(NK)`| Quá chậm so với giới hạn | 
| Cây giới hạn DP |`O(NK)`khấu hao |`O(NK)`trường hợp xấu nhất, nhỏ hơn nhiều với các trạng thái con được giải phóng | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Root cây vào camera`1`và xác định cha của mỗi đỉnh. Chúng ta cũng đếm xem có bao nhiêu đỉnh con vẫn cần phải hoàn thành. Cấu trúc lặp sẽ tránh được các vấn đề về độ sâu đệ quy của Python trên một đường dẫn có độ dài`50,000`. 
2. Khởi tạo DP của mỗi đỉnh với tập hợp kết nối chỉ chứa đỉnh đó. Nếu đỉnh có nhãn`0`, mảng ban đầu của nó là`[1]`, bởi vì nó đóng góp camera không có mật khẩu. Nếu nhãn của nó là`1`, mảng ban đầu của nó là`[0, 1]`, bởi vì tập hợp chứa đỉnh này nhất thiết phải sử dụng một đơn vị năng lượng. 
3. Đặt từng lá vào hàng đợi xử lý. Một lá không còn thông tin con nào để nhận, do đó DP của nó đã hoàn chỉnh và có thể được gửi trực tiếp đến lá mẹ của nó. 
4. Khi một đứa trẻ hoàn thiện`v`đến được cha mẹ của nó`u`, hợp nhất hai mảng DP. Nếu trạng thái gốc hiện tại sử dụng`i`camera mật khẩu và việc sử dụng trạng thái con`j`, thêm vào`current[i] * dp[v][j]`tới nhà nước cho`i + j`. Mảng bị cắt ngắn sau chỉ mục`K`, bởi vì các giá trị lớn hơn không bao giờ có thể đóng góp vào câu trả lời được yêu cầu. 
5. Giữ nguyên DP gốc cũ trong khi thực hiện hợp nhất. Điều này thể hiện tùy chọn không lấy camera từ cây con con. Phép tích chập biểu thị tất cả các lựa chọn có một tập hợp liên thông chứa phần tử con. 
6. Giảm số lượng trẻ chưa hoàn thành`u`. Khi nó đạt đến 0, mọi đứa trẻ đều đã được hợp nhất, vì vậy`dp[u]`đã hoàn tất. Hệ số của nó tại chỉ số`K`đếm mọi tập hợp kết nối hợp lệ có đỉnh được chọn cao nhất là`u`. Thêm hệ số đó vào câu trả lời chung và vượt qua`dp[u]`tới cha mẹ của nó. 
7. Khi gốc đã hoàn thành, tất cả các tập hợp kết nối khác rỗng đều được tính chính xác một lần. In câu trả lời tích lũy. 

### Tại sao nó hoạt động 

Tính bất biến đó là`dp[u][k]`chứa chính xác các tập đỉnh được kết nối bên trong cây con của`u`chứa`u`và có chính xác`k`camera mật khẩu. Ban đầu điều này đúng vì bộ duy nhất có sẵn là`{u}`. Trong quá trình hợp nhất con, mọi tập hợp lệ chứa`u`hoặc không chứa đỉnh nào từ cây con đó hoặc chứa tập hợp liên thông chứa cây con đó. Hai trường hợp này rời rạc và đầy đủ vì việc loại bỏ ranh giới giữa`u`và trẻ tách cái cây thành hai phần. Tích chập đếm mọi sự kết hợp của hai lựa chọn độc lập chính xác một lần. Cuối cùng, mọi tập liên thông khác rỗng đều có một đỉnh cao nhất duy nhất, do đó tính tổng`dp[u][K]`trên các đỉnh đã hoàn thành không thể đếm gấp đôi một tập hợp. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

def solve(data: str) -> str:
    it = iter(data.split())
    n = int(next(it))
    k = int(next(it))

    color = [0] * n
    for i in range(n):
        color[i] = int(next(it))

    graph = [[] for _ in range(n)]
    for _ in range(n - 1):
        u = int(next(it)) - 1
        v = int(next(it)) - 1
        graph[u].append(v)
        graph[v].append(u)

    # Root the tree at 0.
    parent = [-1] * n
    parent[0] = -2
    children_left = [0] * n

    order = [0]
    for u in order:
        for v in graph[u]:
            if v == parent[u]:
                continue
            parent[v] = u
            order.append(v)

    for u in range(n):
        cnt = 0
        for v in graph[u]:
            if parent[v] == u:
                cnt += 1
        children_left[u] = cnt

    # dp[u] is the current connected-subtree DP containing u.
    # Only states 0..K are stored.
    dp = []
    for c in color:
        if c == 0:
            dp.append([1])
        else:
            if k == 0:
                dp.append([0])
            else:
                dp.append([0, 1])

    q = deque()
    for u in range(n):
        if children_left[u] == 0:
            q.append(u)

    answer = 0

    while q:
        u = q.popleft()

        if k < len(dp[u]):
            answer += dp[u][k]

        p = parent[u]
        if p < 0:
            continue

        child = dp[u]
        current = dp[p]

        a = len(current)
        b = len(child)

        new_len = min(k + 1, a + b - 1)
        new_dp = current[:new_len]

        # Take a nonempty connected part containing u from the child.
        # The copy above represents taking nothing from the child.
        for i in range(a):
            x = current[i]
            if x == 0:
                continue

            max_j = min(b - 1, k - i)
            for j in range(max_j + 1):
                y = child[j]
                if y:
                    new_dp[i + j] += x * y

        dp[p] = new_dp
        dp[u] = None

        children_left[p] -= 1
        if children_left[p] == 0:
            q.append(p)

    return str(answer)

def main():
    data = sys.stdin.buffer.read().decode()
    sys.stdout.write(solve(data))

if __name__ == "__main__":
    main()
```Phần đầu tiên của quá trình triển khai xây dựng cây vô hướng và sau đó lấy gốc ở đỉnh`0`. các`parent`mảng đưa ra hướng duy nhất từ ​​nút con tới nút gốc, cho phép chúng tôi xác định khi nào một nút đã nhận được tất cả các DP con của nó. 

DP ban đầu khác biệt có chủ ý đối với các nhãn`0`Và`1`. Đối với máy ảnh không được bảo vệ,`{u}`có chi phí bằng 0, vì vậy`dp[u][0] = 1`. Đối với một máy ảnh được bảo vệ,`{u}`có giá một, vì vậy`dp[u][1] = 1`. Khi`K = 0`, trạng thái sau nằm ngoài phạm vi bắt buộc, do đó mảng ban đầu chỉ có thể chứa số 0. 

Việc hợp nhất bắt đầu với`current[:]`. Bản sao này không chỉ đơn thuần là một chi tiết tối ưu hóa. Nó đại diện cho sự lựa chọn không lấy gì từ đứa trẻ. Sau đó, các vòng lặp lồng nhau sẽ thêm mọi khả năng trong đó tập hợp kết nối chứa phần tử con được chọn. Giới hạn trên`k - i`ngăn chặn việc viết vượt quá ngân sách năng lượng mục tiêu. 

dòng`dp[u] = None`giải phóng DP của đứa trẻ sau khi đóng góp của nó đã được tích hợp vào cha mẹ. Điều này quan trọng đối với việc sử dụng bộ nhớ trên cây lớn. Một đường dẫn có thể có nhiều đỉnh, nhưng chỉ trạng thái gốc hiện đang hoạt động mới cần duy trì độ lớn ở mỗi giai đoạn. 

Thuật toán lặp lại chứ không phải đệ quy. Một DFS đệ quy trên đường dẫn chứa`50,000`các đỉnh sẽ yêu cầu tăng giới hạn đệ quy của Python và vẫn có thể gây áp lực không cần thiết lên ngăn xếp trình thông dịch. Hàng đợi xử lý cây từ lá về gốc mà không gặp rủi ro đó. 

Số nguyên Python không bị tràn, điều này rất hữu ích ở đây vì bài toán yêu cầu số đếm chính xác và không chỉ định mô đun. Số lượng tập hợp con được kết nối có thể lớn theo cấp số nhân, vì vậy việc sử dụng các số nguyên có độ chính xác tùy ý của Python là cần thiết. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
5 2
0 1 0 1 1
1 2
1 3
1 4
2 5
```Root tại`1`mang lại cho trẻ em`2`,`3`, Và`4`, với`5`dưới`2`. Các trạng thái ban đầu và các sự hợp nhất đã hoàn thành quan trọng là: 

| Đỉnh | Nhãn | DP ban đầu | DP đã hoàn thành | 
| --- | --- | --- | --- | 
|`5`|`1`|`[0, 1]`|`[0, 1]`| 
|`2`|`1`|`[0, 1]`|`[0, 1, 1]`| 
|`3`|`0`|`[1]`|`[1]`| 
|`4`|`1`|`[0, 1]`|`[0, 1]`| 
|`1`|`0`|`[1]`|`[1, 3, 5, ...]`| 

Tại đỉnh`2`, người độc thân`{2}`đóng góp một camera mật khẩu, trong khi`{2,5}`đóng góp hai. Như vậy`dp[2][2] = 1`. 

Tại đỉnh`1`, việc kết hợp ba lựa chọn con sẽ cho ra năm tập hợp kết nối chứa`1`với chính xác hai camera mật khẩu. Giải pháp bổ sung`{2,5}`được tính khi đỉnh`2`chính nó đã hoàn thành, vì đỉnh được chọn cao nhất của nó là`2`. Câu trả lời cuối cùng là`5`. 

Dấu vết này chứng tỏ tại sao câu trả lời phải được tích lũy từ mọi đỉnh chứ không chỉ từ gốc. Một bộ kết nối như`{2,5}`không chứa gốc của toàn bộ cây nhưng vẫn có một đỉnh cao nhất duy nhất, đó là`2`. 

### Mẫu 2 

Đầu vào là```
3 1
1 0 1
1 2
1 3
```đỉnh`1`có hai con. Cả hai`1`Và`3`yêu cầu mật khẩu, trong khi`2`không. 

| Đỉnh | Nhãn | DP ban đầu | DP đã hoàn thành | Đóng góp giải đáp | 
| --- | --- | --- | --- | --- | 
|`2`|`0`|`[1]`|`[1]`|`0`| 
|`3`|`1`|`[0, 1]`|`[0, 1]`|`1`| 
|`1`|`1`|`[0, 1]`|`[0, 2]`|`2`| 

Người độc thân`{3}`đóng góp một giải pháp. Tại đỉnh`1`, hai lựa chọn hợp lệ với chính xác một camera mật khẩu là`{1}`Và`{1,2}`, đưa ra thêm hai giải pháp. Câu trả lời là`3`. 

Dấu vết cũng cho thấy tại sao không thể bỏ qua một đứa trẻ không được bảo vệ. Máy ảnh`2`không thay đổi số lượng năng lượng, nhưng việc chọn nó sẽ tạo ra tập hợp kết nối bổ sung`{1,2}`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(NK)`khấu hao | Mảng DP được cắt ngắn ở`K`và việc hợp nhất cây-knapsack bị chặn có sự phụ thuộc tuyến tính vào số đỉnh và giới hạn trạng thái | 
| Không gian |`O(NK)`trường hợp xấu nhất | Mảng DP gốc và mảng DP con đang chờ xử lý sử dụng nhiều nhất`K + 1`tiểu bang, với các mảng con đã hoàn thành sẽ được phát hành ngay lập tức | 

Kích thước trạng thái tối đa chỉ là`101`, từ`K <= 100`. Với`N <= 50,000`, giới hạn trạng thái dự kiến ​​​​là đủ nhỏ cho cách tiếp cận ba lô cây. Việc triển khai cũng tránh việc giữ đồng thời mọi DP đã hoàn thành, điều này làm giảm đáng kể mức tiêu thụ bộ nhớ thực tế so với quy trình thông thường.`dp[N][K+1]`cách trình bày. Bản thân cái cây chỉ cần`O(N)`ký ức. 

## Trường hợp thử nghiệm```python
import io
import sys
from collections import deque

def solve(data: str) -> str:
    it = iter(data.split())
    n = int(next(it))
    k = int(next(it))

    color = [int(next(it)) for _ in range(n)]

    graph = [[] for _ in range(n)]
    for _ in range(n - 1):
        u = int(next(it)) - 1
        v = int(next(it)) - 1
        graph[u].append(v)
        graph[v].append(u)

    parent = [-1] * n
    parent[0] = -2
    order = [0]

    for u in order:
        for v in graph[u]:
            if v == parent[u]:
                continue
            parent[v] = u
            order.append(v)

    children_left = [0] * n
    for v in range(1, n):
        children_left[parent[v]] += 1

    dp = []
    for c in color:
        if c == 0:
            dp.append([1])
        elif k == 0:
            dp.append([0])
        else:
            dp.append([0, 1])

    q = deque(u for u in range(n) if children_left[u] == 0)
    answer = 0

    while q:
        u = q.popleft()

        if k < len(dp[u]):
            answer += dp[u][k]

        p = parent[u]
        if p < 0:
            continue

        child = dp[u]
        current = dp[p]

        a = len(current)
        b = len(child)
        new_len = min(k + 1, a + b - 1)
        new_dp = current[:new_len]

        for i in range(a):
            if i > k:
                break
            x = current[i]
            if x == 0:
                continue

            limit = min(b - 1, k - i)
            for j in range(limit + 1):
                y = child[j]
                if y:
                    new_dp[i + j] += x * y

        dp[p] = new_dp
        dp[u] = None

        children_left[p] -= 1
        if children_left[p] == 0:
            q.append(p)

    return str(answer)

def run(inp: str) -> str:
    return solve(inp).strip()

# Provided samples
assert run("""5 2
0 1 0 1 1
1 2
1 3
1 4
2 5
""") == "5", "sample 1"

assert run("""3 1
1 0 1
1 2
1 3
""") == "3", "sample 2"

assert run("""3 0
1 1 0
1 2
2 3
""") == "1", "sample 3"

# Minimum-size tree, one unprotected camera, zero energy.
assert run("""1 0
0
""") == "1", "minimum-size zero-cost set"

# Minimum-size tree, one protected camera, exactly one energy.
assert run("""1 1
1
""") == "1", "minimum-size password camera"

# Two protected cameras connected by one edge.
# Exactly one password means either singleton.
assert run("""2 1
1 1
1 2
""") == "2", "two singletons"

# Four protected cameras in a path.
# Exactly two protected cameras means every connected set of size two.
assert run("""4 2
1 1 1 1
1 2
2 3
3 4
""") == "3", "path of four protected cameras"

# All cameras unprotected, K = 0.
# Every nonempty connected set of a 5-vertex path is an interval:
# 5 + 4 + 3 + 2 + 1 = 15.
assert run("""5 0
0 0 0 0 0
1 2
2 3
3 4
4 5
""") == "15", "all-zero path"

# Maximum-size case.
# Every vertex is protected and K = 1, so only the N singleton sets work.
n = 50000
colors = " ".join(["1"] * n)
edges = "\n".join(f"1 {v}" for v in range(2, n + 1))
maximum_case = f"{n} 1\n{colors}\n{edges}\n"
assert run(maximum_case) == str(n), "maximum-size star"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0 / 0`|`1`| Cây có kích thước tối thiểu và`K = 0`| 
|`1 1 / 1`|`1`| Một camera mật khẩu đóng góp chính xác một năng lượng | 
|`2 1 / 1 1`|`2`| Chính xác một trạng thái đỉnh và trạng thái đơn được đánh dấu | 
| Bốn đỉnh được bảo vệ trên một đường đi,`K = 2`|`3`| Các tập hợp kết nối của kích thước mục tiêu và việc cắt bớt ranh giới | 
| Năm đỉnh không được bảo vệ trên một đường đi,`K = 0`|`15`| Các trạng thái con không tốn phí và tất cả các khoảng thời gian được kết nối | 
|`50,000`các đỉnh được bảo vệ trong một ngôi sao,`K = 1`|`50,000`| Tối đa`N`và xử lý trạng thái ở quy mô tuyến tính | 

## Vỏ cạnh 

cho`K = 0`, coi như```
3 0
1 1 0
1 2
2 3
```Gốc không bao giờ có thể là một phần của tập hợp kết nối không có năng lượng vì nó được bảo vệ. đỉnh`2`có cùng tài sản. đỉnh`3`bắt đầu bằng`dp[3] = [1]`, đại diện`{3}`với camera không mật khẩu. Nó góp phần`1`khi được xử lý, vì vậy câu trả lời là`1`. Thuật toán không cần trường hợp đặc biệt cho tập hợp trống vì mọi trạng thái DP biểu thị một tập hợp liên thông chứa đỉnh của chính nó. 

Đối với một camera mật khẩu không có con, hãy xem xét```
1 1
1
```Trạng thái ban đầu là`[0,1]`. Hệ số tại chỉ số`1`là một, tương ứng với tập đơn`{1}`. Thuật toán thêm hệ số đó vào câu trả lời và in ra`1`. Nếu việc khởi tạo thay vào đó được sử dụng`[1]`, nó sẽ tính nhầm máy ảnh là không tiêu tốn năng lượng. 

Đối với một đứa trẻ không mất phí, hãy cân nhắc```
2 0
0 0
1 2
```đỉnh`2`bắt đầu bằng`[1]`và đóng góp singleton`{2}`. Khi nó được sáp nhập vào đỉnh`1`, cha mẹ đã có rồi`[1]`và tích chập tạo ra một trạng thái năng lượng bằng 0 bổ sung đại diện cho`{1,2}`. đỉnh`1`bản thân nó đại diện`{1}`. Câu trả lời cuối cùng là`3`, chính xác là ba tập con liên thông khác rỗng. 

Đối với các tiểu bang ngoài`K`, hãy xem xét một đường đi gồm bốn camera được bảo vệ với`K = 2`:```
4 2
1 1 1 1
1 2
2 3
3 4
```Có thể thực hiện một bộ kết nối chứa hai camera theo ba cách:`{1,2}`,`{2,3}`, Và`{3,4}`. Các tập hợp kết nối lớn hơn không thích hợp vì chúng sử dụng nhiều hơn hai đơn vị năng lượng. Mảng DP được cắt ngắn ở chỉ mục`2`, do đó việc hợp nhất không bao giờ xây dựng trạng thái cho ba hoặc bốn camera mật khẩu. Câu trả lời là`3`. 

Ngôi sao có kích thước tối đa```
50000 1
1 1 1 ... 1
1 2
1 3
...
1 50000
```chỉ chứa các camera được bảo vệ. Với`K = 1`, các tập hợp kết nối hợp lệ duy nhất là`50,000`máy ảnh đơn. Mọi sự hợp nhất nội bộ đều rất nhỏ vì mỗi lá chỉ có các trạng thái`[0,1]`và khi gốc đạt đến giới hạn đích, không có trạng thái nào ở trên`1`được giữ lại. Do đó, thuật toán xử lý giá trị lớn nhất được phép`N`mà không liệt kê các tập hợp con được kết nối.
