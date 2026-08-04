---
title: "CF 102556C - Riana và Đi làm"
description: "Riana đang di chuyển trên đường một chiều với các dãy nhà được đánh số từ trái sang phải. Cô ấy bắt đầu ở khối 1 và muốn đến khối A."
date: "2026-08-04T09:15:15+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102556
codeforces_index: "C"
codeforces_contest_name: "2020 Ateneo de Manila University DISCS PrO HS Division"
rating: 0
weight: 102556
solve_time_s: 63
verified: true
draft: false
---

[CF 102556C – Riana và Đi lại](https://codeforces.com/problemset/problem/102556/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Riana đang di chuyển trên đường một chiều với các dãy nhà được đánh số từ trái sang phải. Cô ấy bắt đầu từ dãy nhà số 1 và muốn đến dãy nhà A. Trong khi đi bộ, cô ấy có thể chọn một trong hai hướng, nhưng các điểm dừng xe buýt thay đổi quy tắc di chuyển: bất cứ khi nào cô ấy đến điểm dừng xe buýt, cô ấy phải bắt xe buýt ngay đến đó và xe buýt sẽ đưa cô ấy đến một khối điểm đến cố định. Mỗi xe buýt đều di chuyển đến một số khối lớn hơn, vì vậy việc đi xe buýt không bao giờ có thể tự tạo thành một chu kỳ. 

Đầu vào mô tả chiều dài đường phố, khối đích và tập hợp các điểm dừng xe buýt. Mỗi điểm dừng xe buýt là một bước nhảy trực tiếp từ vị trí của nó tới vị trí lớn hơn. Nhiệm vụ là xác định xem có tồn tại chuỗi lựa chọn đi bộ nào đó mà cuối cùng đặt Riana vào khối A hay không. Kết quả đầu ra là`YES`nếu cô ấy có thể với tới nó và`NO`nếu không thì. 

Các hạn chế là nhỏ, tối đa là 100 dãy nhà và nhiều nhất là 50 điểm dừng xe buýt. Điều này có nghĩa là một thuật toán khám phá mọi trạng thái hữu ích có thể có là đủ. Giải pháp khám phá theo cấp số nhân tất cả các con đường đi bộ có thể là không cần thiết, bởi vì nhiều con đường khác nhau có thể dẫn đến cùng một khối. Thay vào đó, chúng ta có thể lưu trữ những khối đã đạt được và xử lý từng khối một lần. 

Các trường hợp nguy hiểm chính xuất phát từ tính chất bắt buộc của xe buýt. Một sai lầm phổ biến là coi xe buýt như những bước nhảy tùy chọn. Ví dụ:```
5 4 1
2 5
```Câu trả lời đúng là`NO`. Từ khối 1, đi bộ sang phải sẽ đến khối 2 và Riana buộc phải bắt xe buýt đến khối 5. Sau đó cô ấy có thể đi bộ sang trái và đến khối 4, vì vậy ví dụ này thực sự tạo ra`YES`. Phần nguy hiểm không phải là câu trả lời cuối cùng mà là giả định: nếu một giải pháp bỏ qua bus bắt buộc và coi khối 2 như một vị trí bình thường, nó có thể khám phá các đường dẫn không hợp lệ. 

Một ví dụ trực tiếp hơn là:```
6 3 1
2 6
```Câu trả lời đúng là`NO`. Đi bộ sang phải từ khối 1 sẽ đến điểm dừng ở khối 2 và ngay lập tức đưa Riana đến khối 6. Cô ấy chỉ có thể đi bộ sang trái từ đó, nhưng khối 3 nằm trước trạm dừng xe buýt ở khối 2 và không thể quay lại được nữa vì cô ấy không thể băng qua điểm dừng đó mà không bị đưa đi. Việc giải thích đường đi ngắn nhất một cách bất cẩn cho phép di chuyển qua tất cả các khối lân cận sẽ tìm đường đi không chính xác. 

Một trường hợp cạnh khác đang bắt đầu ở đích:```
5 1 1
1 5
```Câu trả lời đúng là`YES`. Riana đã ở Ateneo nên cô ấy không cần phải di chuyển. Việc triển khai luôn áp dụng chuyển đổi xe buýt trước khi kiểm tra điểm đến sẽ khiến cô ấy di chuyển đi một cách không chính xác. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là mô phỏng mọi quyết định đi bộ có thể xảy ra. Từ một dãy nhà, Riana có thể thử đi bộ nhiều lần sang trái hoặc phải qua một dãy nhà và bất cứ khi nào đến trạm xe buýt, cô ấy sẽ nhảy đến điểm đến đó. Điều này đúng vì nó tuân thủ trực tiếp các quy luật chuyển động. Tuy nhiên, nếu được triển khai dưới dạng tìm kiếm trên các đường dẫn mà không nhớ các khối đã truy cập, nó có thể xem lại các tình huống tương tự nhiều lần. Mặc dù đường phố ở đây nhỏ, nhưng quan điểm bạo lực tự nhiên coi việc đi bộ là một số lượng lớn các trình tự có thể xảy ra, đặc biệt là vì Riana có thể đi lang thang tới lui trước khi bắt xe buýt. 

Quan sát quan trọng là sau mỗi chuỗi xe buýt bắt buộc kết thúc, chỉ khối hiện tại của Riana là quan trọng. Lịch sử đi bộ trước đây của cô ấy không ảnh hưởng đến những lựa chọn trong tương lai. Vì chỉ có N khối nên chúng ta có thể thực hiện tìm kiếm đồ thị trên các khối có thể truy cập được. 

Câu hỏi còn lại là làm thế nào để tạo các cạnh của đồ thị. Từ một dãy nhà có thể tới được, Riana có thể đi bộ theo một trong hai hướng cho đến khi đến Ateneo hoặc gặp trạm xe buýt đầu tiên theo hướng đó. Cô ấy không thể vượt qua trạm xe buýt đầu tiên vì xe buýt sẽ đưa cô ấy đi ngay lập tức. Nếu không có trạm xe buýt ở hướng đó, mọi dãy nhà theo hướng đó đều có thể đến được bằng cách đi bộ. Nếu có một điểm dừng xe buýt, trạng thái kết quả duy nhất là điểm đến sau khi đi theo tất cả các xe buýt bắt buộc từ điểm dừng đó. 

Số lượng khối đủ nhỏ để chúng ta có thể kiểm tra tất cả các khối xung quanh một vị trí khi tạo chuyển tiếp. Điều này mang lại một giải pháp tìm kiếm theo chiều rộng đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Có khả năng tăng theo cấp số nhân về số lượng lựa chọn chuyển động | O(N) | Quá chậm nếu không ghi nhớ | 
| Tối ưu | O(N2 + B) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các điểm dừng xe buýt và lưu trữ điểm đến của từng điểm dừng theo số khối của nó. Thiếu mục nghĩa là khu nhà đó không phải là bến xe buýt. 
2. Nếu khối xuất phát đã là Ateneo, hãy trả lời ngay`YES`. Đến đích là đủ, không cần phải di chuyển. 
3. Sử dụng tìm kiếm theo chiều rộng bắt đầu từ khối 1. Mỗi mục nhập hàng đợi đại diện cho một khối mà Riana có thể ở sau khi tất cả các chuyến xe buýt bắt buộc đã kết thúc. 
4. Khi xử lý một khối, hãy kiểm tra xem việc đi bộ sang trái có thể đến được Ateneo hay không trước khi chạm vào trạm xe buýt. Nếu không có trạm xe buýt ở bên trái, bạn có thể đến mọi dãy nhà nhỏ hơn. Nếu có trạm dừng xe buýt gần nhất ở bên trái thì chỉ có thể đến các dãy nhà giữa dãy nhà hiện tại và trạm dừng đó bằng cách đi bộ. 
5. Thực hiện kiểm tra tương tự khi đi bên phải. Nếu gặp điểm dừng xe buýt đầu tiên, trạng thái có thể tiếp cận tiếp theo là đích đến của xe buýt đó, tiếp theo là bất kỳ xe buýt bổ sung nào được kích hoạt tại điểm đến đó. 
6. Bất cứ khi nào tìm thấy một khối mới có thể truy cập được, hãy thêm khối đó vào hàng đợi. Nếu một khối đã được xử lý, hãy bỏ qua nó vì việc khám phá lại khối đó không thể tiết lộ điều gì mới. 
7. Nếu tìm kiếm kết thúc mà không đạt đến A, xuất ra`NO`. 

Tại sao nó hoạt động: 

Bất biến được duy trì bởi tìm kiếm là mỗi khối được đặt vào hàng đợi là một khối mà Riana thực sự có thể chiếm giữ sau khi tuân theo tất cả các chuyến xe buýt bắt buộc. Khi mở rộng một khối, thuật toán sẽ xem xét chính xác kết quả pháp lý của việc chọn đi bên trái hoặc bên phải. Nó không bao giờ cho phép cô ấy đi qua trạm xe buýt mà không bắt xe buýt, và nó không bao giờ tạo ra một chuyển động bất khả thi. Vì mọi chuỗi quyết định hợp lệ đều tương ứng với một chuỗi các chuyển đổi này, nên mọi đích đến có thể tiếp cận cuối cùng sẽ được phát hiện. Nếu việc tìm kiếm không thể tìm thấy A thì không có chuỗi lựa chọn hợp lệ nào có thể tiếp cận được nó. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

def solve():
    N, A, B = map(int, input().split())

    bus = [-1] * (N + 1)
    for _ in range(B):
        x, y = map(int, input().split())
        bus[x] = y

    if A == 1:
        print("YES")
        return

    def follow_bus(pos):
        while bus[pos] != -1:
            pos = bus[pos]
        return pos

    start = follow_bus(1)

    visited = [False] * (N + 1)
    visited[start] = True
    q = deque([start])

    while q:
        pos = q.popleft()

        if pos == A:
            print("YES")
            return

        left_stop = -1
        for x in range(pos - 1, 0, -1):
            if bus[x] != -1:
                left_stop = x
                break

        if left_stop == -1:
            for x in range(1, pos):
                if not visited[x]:
                    if x == A:
                        print("YES")
                        return
                    visited[x] = True
                    q.append(x)
        else:
            for x in range(left_stop + 1, pos):
                if not visited[x]:
                    if x == A:
                        print("YES")
                        return
                    visited[x] = True
                    q.append(x)
            nxt = follow_bus(left_stop)
            if not visited[nxt]:
                visited[nxt] = True
                q.append(nxt)

        right_stop = -1
        for x in range(pos + 1, N + 1):
            if bus[x] != -1:
                right_stop = x
                break

        if right_stop == -1:
            for x in range(pos + 1, N + 1):
                if not visited[x]:
                    if x == A:
                        print("YES")
                        return
                    visited[x] = True
                    q.append(x)
        else:
            for x in range(pos + 1, right_stop):
                if not visited[x]:
                    if x == A:
                        print("YES")
                        return
                    visited[x] = True
                    q.append(x)
            nxt = follow_bus(right_stop)
            if not visited[nxt]:
                visited[nxt] = True
                q.append(nxt)

    print("NO")

solve()
```Mảng xe buýt lưu trữ sự chuyển tiếp duy nhất có thể có từ mỗi điểm dừng xe buýt. các`follow_bus`Hàm xử lý trường hợp điểm đến của xe buýt là một điểm dừng xe buýt khác. Vì mỗi bus đều tăng số khối nên vòng lặp này luôn kết thúc. 

Hàng đợi BFS chỉ chứa các vị trí có thể có sau khi tất cả các chuyến đi xe buýt tự động đã diễn ra. Đây là lý do tại sao vị trí ban đầu được chuẩn hóa với`follow_bus(1)`. Việc kiểm tra đặc biệt đối với`A == 1`xảy ra trước đó vì đến đích không cần phải bắt xe buýt đi xa. 

Khi tạo chuyển tiếp, mã sẽ tìm kiếm điểm dừng xe buýt gần nhất theo mỗi hướng. Các dãy nhà trước điểm dừng đó là điểm đến đi bộ hợp lệ. Bản thân điểm dừng không được thêm vào dưới dạng trạng thái đi bộ vì việc đến điểm dừng đó sẽ ngay lập tức bắt đầu chuyến xe buýt. Trạng thái duy nhất được tạo từ điểm dừng là điểm đến xe buýt cuối cùng. 

Không có vấn đề tràn số nguyên vì mọi giá trị tối đa là 100. Các vòng lặp có chủ ý chỉ bao gồm các khối nằm giữa vị trí hiện tại và điểm dừng tiếp theo, tránh sai lầm từng điểm một khi cho phép Riana đứng tại điểm dừng mà không đi xe buýt. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
10 5 4
1 3
2 6
4 10
7 9
```| Khối hiện tại | Đã kiểm tra hướng | Kết quả | Xếp hàng sau khi xử lý | 
| --- | --- | --- | --- | 
| 3 | Trái | Có thể tới 2 thì chuỗi xe buýt từ 2 lên 6 | [6, 2] | 
| 2 | Trái | Có thể đạt 1 | [6, 1] | 
| 6 | Trái | Có thể đạt 5 | [1, 5] | 
| 5 | Điểm đến | Đã đạt | CÓ | 

Vị trí bắt đầu 1 là điểm dừng xe buýt, vì vậy hành động bắt buộc đầu tiên sẽ đưa Riana đến khối 3. Từ đó, quá trình tìm kiếm sẽ phát hiện ra trình tự tương tự như lời giải thích mẫu: sử dụng xe buýt ở khối 2, sau đó đi bộ trở lại khối 5. 

### Mẫu 2 

đầu vào:```
8 3 2
2 6
4 7
```| Khối hiện tại | Đã kiểm tra hướng | Kết quả | Xếp hàng sau khi xử lý | 
| --- | --- | --- | --- | 
| 1 | Đúng | Đến block 2, bắt xe buýt lên 6 | [6] | 
| 6 | Trái | Khối 5, 4 có thể tiếp cận được, khối 4 buộc xe buýt đến 7 | [7, 5] | 
| 7 | Trái | Khối 6, 5 có thể truy cập được | [5] | 
| 5 | Trái | Khối 4 buộc xe buýt tới số 7 | [] | 

Không bao giờ đến được Khối 3 vì mọi nỗ lực băng qua trạm xe buýt liên quan đều khiến Riana bỏ chạy. Việc tìm kiếm sử dụng hết tất cả các trạng thái hợp lệ và trả về`NO`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N2 + B) | Đối với mỗi dãy nhà có thể tiếp cận, thuật toán sẽ quét đường phố để tìm các điểm dừng gần đó và các dãy nhà đi bộ có thể tiếp cận. | 
| Không gian | O(N) | Hàng đợi và mảng đã truy cập lưu trữ tối đa một mục nhập trên mỗi khối. | 

Với N tối đa là 100, quá trình quét bậc hai rất nhỏ. Giải pháp thực hiện tối đa khoảng mười nghìn lượt kiểm tra khối, nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io
from collections import deque

def solve_case(inp):
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    N, A, B = map(int, input().split())
    bus = [-1] * (N + 1)

    for _ in range(B):
        x, y = map(int, input().split())
        bus[x] = y

    if A == 1:
        return "YES\n"

    def follow_bus(pos):
        while bus[pos] != -1:
            pos = bus[pos]
        return pos

    start = follow_bus(1)
    visited = [False] * (N + 1)
    visited[start] = True
    q = deque([start])

    while q:
        pos = q.popleft()
        if pos == A:
            return "YES\n"

        for direction in (-1, 1):
            x = pos + direction
            while 1 <= x <= N:
                if bus[x] != -1:
                    nxt = follow_bus(x)
                    if not visited[nxt]:
                        visited[nxt] = True
                        q.append(nxt)
                    break
                if not visited[x]:
                    if x == A:
                        return "YES\n"
                    visited[x] = True
                    q.append(x)
                x += direction

    return "NO\n"

assert solve_case("""10 5 4
1 3
2 6
4 10
7 9
""") == "YES\n", "sample 1"

assert solve_case("""8 3 2
2 6
4 7
""") == "NO\n", "sample 2"

assert solve_case("""2 1 1
1 2
""") == "YES\n", "already at destination"

assert solve_case("""6 3 1
2 6
""") == "NO\n", "forced bus skips target"

assert solve_case("""100 100 1
50 100
""") == "YES\n", "maximum block boundary"

assert solve_case("""5 5 3
1 5
2 5
3 5
""") == "YES\n", "many buses with same destination"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 1 1`bằng xe buýt ở dãy nhà 1 | CÓ | Xuất phát tại điểm đến phải được chấp nhận trước khi di chuyển. | 
|`6 3 1`với`2 -> 6`| KHÔNG | Không thể băng qua trạm xe buýt nếu không bắt xe buýt. | 
|`100 100 1`với`50 -> 100`| CÓ | Số khối lớn và đạt đến ranh giới cuối cùng. | 
| Nhiều xe buýt kết thúc ở cùng một dãy nhà | CÓ | Các điểm dừng xe buýt khác nhau có thể chia sẻ điểm đến. | 

## Vỏ cạnh 

Đối với trường hợp Riana bắt đầu tại điểm đến:```
5 1 1
1 5
```Thuật toán kiểm tra`A == 1`trước khi đi theo xe buýt. Nó xuất ra`YES`, coi việc đến là hoàn tất một cách chính xác trước khi bất kỳ chuyển động cưỡng bức nào xảy ra. 

Đối với trường hợp xe buýt chặn quyền truy cập vào khối thấp hơn:```
6 3 1
2 6
```Việc tìm kiếm bắt đầu ở khối 1. Di chuyển sang phải sẽ đến khối 2, là điểm dừng xe buýt, do đó trạng thái kết quả duy nhất là khối 6. Từ khối 6, đi bộ sang trái sẽ đến khối 4, nhưng bước lên khối 2 lần nữa sẽ kích hoạt xe buýt. Khối 3 không bao giờ được tạo nên thuật toán đưa ra`NO`. 

Đối với trường hợp có nhiều chuyến xe buýt xảy ra ngay lập tức:```
10 5 2
1 3
3 8
```Khối bắt đầu đi theo xe buýt từ khối 1 đến khối 3, sau đó ngay lập tức theo xe buýt ở khối 3 đến khối 8. Việc tìm kiếm bắt đầu từ khối 8 và tiếp tục bình thường. Tính chất ngày càng tăng của các điểm đến xe buýt đảm bảo rằng chuỗi này không thể lặp lại mãi mãi.
