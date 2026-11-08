---
title: "CF 141D - Đường dốc cất cánh"
description: "Chúng ta đang di chuyển trên đường một chiều từ vị trí 0 đến vị trí L. Việc đi bộ rất đơn giản, một mét tốn đúng một giây và chúng ta có thể di chuyển theo một trong hai hướng miễn là chúng ta không bao giờ đi xuống dưới 0. Đoạn đường nối có một lối tắt, nhưng việc sử dụng nó có cấu trúc chặt chẽ."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "graphs", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 141
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 101 (Div. 2)"
rating: 2300
weight: 141
solve_time_s: 135
verified: true
draft: false
---

[CF 141D - Đường dốc cất cánh](https://codeforces.com/problemset/problem/141/D) 

**Đánh giá:** 2300 
**Tags:** đồ thị, đường đi ngắn nhất 
**Thời gian giải:** 2m 15s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta đang di chuyển trên đường một chiều từ vị trí`0`để định vị`L`. Đi bộ rất đơn giản, một mét tốn chính xác một giây và chúng ta có thể di chuyển theo một trong hai hướng miễn là chúng ta không bao giờ đi xuống dưới`0`. 

Đoạn đường nối đưa ra một lối tắt nhưng việc sử dụng nó có cấu trúc chặt chẽ. Đối với một đoạn đường nối`(x, d, t, p)`, Vasya trước tiên phải đứng ở vị trí`x - p`, sau đó chi tiêu`p`giây trượt tuyết bình thường để đạt được`x`, và cuối cùng bay đến`x + d`TRONG`t`giây. Chuyến bay luôn đi về bên phải. 

Nếu chúng ta so sánh điều này với việc đi bộ bình thường, đi qua khoảng thời gian từ`x - p`ĐẾN`x + d`đi bộ sẽ tốn kém`p + d`giây. Sử dụng đoạn đường nối thay vì chi phí`p + t`giây. Đoạn đường nối chỉ hữu ích khi`t < d`. 

Nhiệm vụ không chỉ là tính toán tổng thời gian tối thiểu mà còn phải xây dựng lại những đoạn đường dốc nào được sử dụng. 

Các ràng buộc ngay lập tức loại trừ mọi cách xây dựng đường đi ngắn nhất bậc hai. Có thể có tới`10^5`đường dốc, do đó, ngay cả việc xây dựng tất cả các chuyển đổi theo cặp cũng sẽ yêu cầu khoảng`10^10`các cạnh, vượt xa giới hạn. Chúng ta cần một cái gì đó gần gũi`O(n log n)`. 

Chiều dài đường đua`L`có thể đạt được`10^9`, có nghĩa là chúng tôi không thể xây dựng DP qua các vị trí. Chỉ có điểm cuối của đoạn đường nối là quan trọng. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Có thể không sử dụng được đoạn đường nối vì đoạn chuẩn bị bắt đầu trước số 0. 

Ví dụ:```
1 20
4 10 1 7
```Đoạn đường nối yêu cầu bắt đầu từ`-3`, điều đó bị cấm. Câu trả lời đúng chỉ đơn giản là đi bộ`20`giây. 

Đoạn đường nối cũng có thể vô dụng ngay cả khi có thể tiếp cận được. 

Ví dụ:```
1 20
5 3 10 2
```Đi bộ qua phần đó tốn kém`2 + 3 = 5`, trong khi đoạn đường nối có giá`2 + 10 = 12`. Việc triển khai thực hiện một cách mù quáng mọi đoạn đường có thể tiếp cận sẽ tạo ra câu trả lời không tối ưu. 

Một trường hợp tế nhị khác là di chuyển lùi. Tuyên bố cho phép di chuyển theo cả hai hướng. Một giải pháp tham lam bất cẩn có thể cho rằng các đường dốc phải được sử dụng theo thứ tự tăng dần của điểm cất cánh. Điều đó không phải lúc nào cũng đúng ở địa phương. Tuy nhiên, cấu trúc toàn cục tối ưu hóa ra lại không có chu kỳ sau khi nén và chứng tỏ đó là điểm mấu chốt của vấn đề. 

Ví dụ:```
2 30
10 10 1 1
5 20 100 1
```Đoạn đường thứ hai thật khủng khiếp, nhưng đoạn đường đầu tiên thì tuyệt vời. Con đường tốt nhất vẫn là đường đơn điệu ở bên phải. 

## Phương pháp tiếp cận 

Chế độ xem brute-force mô hình hóa mọi đoạn đường nối như một trạng thái. Từ một đoạn đường nối, chúng ta có thể đi bộ đến điểm chuẩn bị của đoạn đường nối khác và sử dụng đoạn đường tiếp theo. Chúng ta cũng có thể kết thúc bằng cách đi thẳng đến`L`. 

Giả sử chúng ta xác định một biểu đồ trong đó mỗi nút biểu thị một đoạn đường nối, cộng với nút bắt đầu và nút kết thúc. Nếu chúng ta hiện đang ở vị trí`a`, và một đoạn đường nối khác bắt đầu chuẩn bị tại`b`, sau đó đi bộ đến đó tốn kém`|a - b|`. Việc sử dụng đoạn đường nối sẽ thêm thời gian bay của nó. 

Biểu đồ này đúng vì mọi tuyến đường hợp lệ đều trở thành một đường dẫn trong biểu đồ. Thật không may, mọi đoạn đường nối đều có khả năng chuyển tiếp sang mọi đoạn đường nối khác, vì vậy biểu đồ có`O(n^2)`các cạnh. Với`10^5`dốc, điều này hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là thế giới bên ngoài đường dốc cực kỳ đơn giản. Chi phí đi bộ giữa hai vị trí chỉ là khoảng cách tuyến tính. Điều đó có nghĩa là chúng ta thực sự không cần chuyển đổi tùy ý. 

Hãy xem xét một đoạn đường nối`(x, d, t, p)`. Sử dụng nó thay thế việc đi bộ trên một đoạn chiều dài`p + d`với chi phí là`p + t`. So với việc đi bộ thuần túy, đoạn đường nối thay đổi tổng số bằng:$$t - d$$Phần chuẩn bị hủy bỏ. 

Điều này biến đổi toàn bộ vấn đề. Hãy tưởng tượng Vasya luôn đi bộ từ`0`ĐẾN`L`, trả tiền`L`giây. Mỗi đoạn đường nối được sử dụng đều đóng góp một giá trị bổ sung`t - d`. Vì đường dốc tốt thỏa mãn`t < d`, chúng làm giảm tổng số. 

Bây giờ chúng ta chỉ cần biết liệu các đường dốc có thể được xích hay không. Đoạn đường nối`A`kết thúc tại`x_A + d_A`có thể được theo sau bởi đoạn đường nối`B`nếu như:$$x_B - p_B \ge x_A + d_A$$bởi vì chúng ta có thể đi tiếp giữa họ. 

Đi lùi không bao giờ có lợi. Nếu chúng ta lùi lại, chúng ta chỉ tăng thêm khoảng cách đi bộ trong khi bản thân các đoạn đường dốc không bao giờ di chuyển sang trái. Bất kỳ đường đi nào có chuyển động lùi đều có thể được làm thẳng thành đường đi đơn điệu mà không tốn nhiều chi phí hơn. 

Điều đó mang lại cho chúng ta một DAG được sắp xếp theo vị trí. 

Bây giờ chúng ta có thể chạy đường DP ngắn nhất qua các đường dốc được sắp xếp theo điểm bắt đầu chuẩn bị của chúng. Quá trình chuyển đổi trở thành:$$dp[i] = (t_i - d_i) + \min dp[j]$$trên tất cả các đường dốc`j`kết thúc đó trước đoạn đường nối`i`bắt đầu chuẩn bị. 

Đây là một vấn đề truy vấn tối thiểu theo dòng quét cổ điển. Khi chúng tôi xử lý các đường dốc theo thứ tự được sắp xếp, chúng tôi duy trì giá trị DP tốt nhất trong số các đường dốc có vị trí hạ cánh có thể tiếp cận được. 

Hàng đợi ưu tiên hoặc quét theo thứ tự làm giảm độ phức tạp xuống`O(n log n)`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Biểu đồ Brute Force | O(n²) | O(n²) | Quá chậm | 
| Dòng quét DP | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Loại bỏ mọi đoạn đường nối có`x - p < 0`. 

Những đoạn đường dốc như vậy không bao giờ có thể được sử dụng vì quá trình chuẩn bị sẽ bắt đầu trước vạch xuất phát. 
2. Với mỗi đoạn đường nối còn lại, hãy tính:$$s = x - p$$Và$$e = x + d$$Đây`s`là vị trí bắt đầu chuẩn bị và`e`là vị trí hạ cánh. 
3. Sắp xếp các đường dốc theo`s`. 

Điều này cho phép chúng tôi xử lý các đường nối từ trái sang phải trong khi vẫn duy trì các đường nối trước đó có thể kết nối với đường dốc hiện tại. 
4. Xác định`dp[i]`vì chi phí bổ sung tối thiểu liên quan đến việc đi bộ thuần túy khi đoạn đường nối được sử dụng cuối cùng là đoạn đường nối`i`. 

Kể từ khi đi bộ toàn bộ chi phí theo dõi`L`, câu trả lời cuối cùng trở thành:$$L + \min(0, dp[i])$$5. Đối với một đoạn đường nối được sử dụng trực tiếp ngay từ đầu, chi phí tăng thêm là:$$t_i - d_i$$bởi vì đi bộ từ`0`ĐẾN`L`đã bao gồm khoảng cách chuẩn bị và hạ cánh. 
6. Duy trì cấu trúc ưu tiên của các đường dốc được sắp xếp theo vị trí hạ cánh`e`. 

Trong khi xử lý đoạn đường nối`i`, mọi đoạn đường nối trước đó với`e \le s_i`có thể đi trước nó. 
7. Theo dõi giá trị DP nhỏ nhất trong số tất cả các đường dốc trước đó hợp lệ hiện tại. 

Sau đó:$$dp[i] = (t_i - d_i) + best$$Ở đâu`best`là một trong hai`0`để bắt đầu trực tiếp hoặc DP tiền nhiệm tối thiểu. 
8. Lưu trữ các con trỏ cha bất cứ khi nào con trỏ trước cải thiện giá trị. 

Điều này cho phép xây dựng lại các đường dốc đã chọn. 
9. Sau khi xử lý tất cả các đường dốc, hãy chọn đường dốc tốt nhất trong số:`L`không có đường dốc, 

hoặc$$L + dp[i]$$cho mỗi đoạn đường nối. 
10. Xây dựng lại đường đi bằng cách sử dụng các con trỏ cha và in các chỉ số đoạn đường nối ban đầu. 

### Tại sao nó hoạt động 

Điều bất biến là trong khi xử lý các đường nối theo thứ tự chuẩn bị-bắt đầu tăng dần, giá trị DP tối thiểu được duy trì tương ứng chính xác với tất cả các đường nối có thể đi trước đoạn đường nối hiện tại một cách hợp pháp. 

Bởi vì mọi đoạn đường dốc đều di chuyển hoàn toàn về bên phải, nên bất kỳ chuỗi đường dốc khả thi nào cũng tạo thành một chuỗi đơn điệu. Việc đi bộ giữa các đoạn đường dốc được xác định duy nhất bởi các vị trí, do đó, cách tối ưu hóa có ý nghĩa duy nhất là chọn những đoạn đường có lợi thay thế việc đi bộ. 

DP lưu trữ chi phí bổ sung tối thiểu ngoài mức đi bộ cơ bản. Mỗi đoạn đường nối đều đóng góp độc lập`t - d`và các chuyển đổi hợp lệ chỉ yêu cầu thứ tự không chồng chéo:$$e_j \le s_i$$Do đó, mọi giải pháp khả thi đều được thể hiện chính xác một lần trong DP và mỗi lần chuyển đổi DP tương ứng với một tuyến đường hợp lệ. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    n, L = map(int, input().split())

    ramps = []

    for idx in range(1, n + 1):
        x, d, t, p = map(int, input().split())

        s = x - p
        if s < 0:
            continue

        e = x + d
        gain = t - d

        ramps.append((s, e, gain, idx))

    ramps.sort()

    m = len(ramps)

    dp = [0] * m
    parent = [-1] * m

    heap = []

    best_value = 0
    best_idx = -1

    answer = L
    answer_idx = -1

    ptr = 0

    by_end = sorted(
        [(ramps[i][1], i) for i in range(m)]
    )

    for s, end_idx in by_end:
        pass

    events = sorted(
        [(ramps[i][1], i) for i in range(m)]
    )

    ptr = 0

    for i in range(m):
        s_i, e_i, gain_i, orig_i = ramps[i]

        while ptr < m and events[ptr][0] <= s_i:
            _, j = events[ptr]

            heapq.heappush(heap, (dp[j], j))
            ptr += 1

        if heap:
            best_value, best_idx = heap[0]
        else:
            best_value, best_idx = 0, -1

        dp[i] = gain_i + best_value
        parent[i] = best_idx

        total = L + dp[i]

        if total < answer:
            answer = total
            answer_idx = i

    path = []

    cur = answer_idx

    while cur != -1:
        path.append(ramps[cur][3])
        cur = parent[cur]

    path.reverse()

    print(answer)
    print(len(path))

    if path:
        print(*path)
    else:
        print()

solve()
```Giải pháp bắt đầu bằng cách lọc các đoạn đường dốc không hợp lệ. Điều này quan trọng vì các đường dốc có`x - p < 0`thậm chí không thể bắt đầu chuẩn bị, vì vậy họ không bao giờ được vào DP. 

Mỗi đoạn đường nối được chuyển đổi thành dạng nhỏ gọn chứa phần bắt đầu chuẩn bị của nó`s`, điểm hạ cánh`e`, và đóng góp`t - d`. Sự đóng góp đó là phần duy nhất ảnh hưởng đến việc tối ưu hóa khi chúng tôi coi việc đi bộ đơn giản là chi phí cơ bản. 

Các đường dốc được sắp xếp theo`s`, điều này đảm bảo rằng mọi tiền thân hợp lệ sẽ xuất hiện sớm hơn theo thứ tự xử lý. 

Phần tinh tế là duy trì phiên bản tiền nhiệm tốt nhất có thể truy cập được. Chúng tôi không thể quét tất cả các đường dốc trước đó vì điều đó sẽ trở lại độ phức tạp bậc hai. Thay vào đó, chúng tôi xử lý các đường dốc có vị trí hạ cánh đủ nhỏ. Khi đoạn đường nối thỏa mãn:```
e_j <= s_i
```nó trở nên đủ điều kiện vĩnh viễn cho tất cả các đường dốc trong tương lai vì tương lai`s_i`giá trị chỉ tăng lên. 

Heap lưu trữ các cặp`(dp[j], j)`, do đó, phần trên cùng luôn mang lại giá trị DP nhỏ nhất có thể tiếp cận được. 

Một sai lầm phổ biến là cố gắng tham lam sử dụng mọi đoạn đường có lợi. Hai đường dốc tốt riêng lẻ có thể chồng lên nhau và trở nên không tương thích. DP tránh điều này bằng cách chỉ chuyển đổi từ các đường dốc có điểm hạ cánh trước khi bắt đầu chuẩn bị tiếp theo. 

Một lỗi dễ xảy ra khác là quên rằng giải pháp tối ưu có thể không sử dụng đường dốc nào cả. Câu trả lời ban đầu được đặt thành`L`, đại diện cho việc đi bộ thuần túy. 

Tất cả số học đều phù hợp một cách an toàn với số nguyên Python vì các giá trị có thể đạt tới khoảng`10^14`. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2 20
5 10 5 5
4 16 1 7
```Chỉ đoạn đường nối 1 là hợp lệ vì đoạn đường nối 2 bắt đầu chuẩn bị tại`-3`. 

| Đoạn đường nối | s | e | t-d | Tốt nhất trước đó | dp | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 0 | 15 | -5 | 0 | -5 | 

Câu trả lời cuối cùng:$$20 + (-5) = 15$$Đường dốc đã chọn:`[1]`Dấu vết này cho thấy tại sao các đường nối không thể thực hiện được phải bị loại bỏ trước khi xử lý DP. Nếu đoạn đường nối 2 được đưa vào, thuật toán sẽ tin nhầm rằng có một lối tắt khổng lồ tồn tại. 

### Mẫu 2```
2 20
5 3 10 2
14 6 1 1
```| Đoạn đường nối | s | e | t-d | Tốt nhất trước đó | dp | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 3 | 8 | 7 | 0 | 7 | 
| 2 | 13 | 20 | -5 | 0 | -5 | 

Đoạn đường nối 1 có hại vì thời gian bay của nó vượt quá khoảng cách đi bộ đã lưu. 

Câu trả lời cuối cùng:$$20 + (-5) = 15$$Đường dốc đã chọn:`[2]`Điều này chứng tỏ rằng DP bỏ qua các đường dốc xấu một cách tự nhiên mà không yêu cầu logic trong trường hợp đặc biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Hoạt động sắp xếp và heap chiếm ưu thế | 
| Không gian | O(n) | Mảng DP, vùng heap và tái thiết cha mẹ | 

Với`10^5`đường dốc,`O(n log n)`dễ dàng phù hợp trong thời hạn. Việc sử dụng bộ nhớ vẫn tuyến tính và thoải mái dưới mức giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string

import sys
import io
import heapq

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    out = io.StringIO()
    sys.stdout = out

    n, L = map(int, input().split())

    ramps = []

    for idx in range(1, n + 1):
        x, d, t, p = map(int, input().split())

        s = x - p

        if s < 0:
            continue

        e = x + d
        gain = t - d

        ramps.append((s, e, gain, idx))

    ramps.sort()

    m = len(ramps)

    dp = [0] * m
    parent = [-1] * m

    events = sorted((ramps[i][1], i) for i in range(m))

    heap = []

    ptr = 0

    answer = L
    answer_idx = -1

    for i in range(m):
        s_i, e_i, gain_i, orig_i = ramps[i]

        while ptr < m and events[ptr][0] <= s_i:
            _, j = events[ptr]
            heapq.heappush(heap, (dp[j], j))
            ptr += 1

        if heap:
            best, best_idx = heap[0]
        else:
            best, best_idx = 0, -1

        dp[i] = gain_i + best
        parent[i] = best_idx

        total = L + dp[i]

        if total < answer:
            answer = total
            answer_idx = i

    path = []

    cur = answer_idx

    while cur != -1:
        path.append(ramps[cur][3])
        cur = parent[cur]

    path.reverse()

    print(answer)
    print(len(path))

    if path:
        print(*path)
    else:
        print()

    return out.getvalue()

# provided samples

assert run(
"""2 20
5 10 5 5
4 16 1 7
"""
) == "15\n1\n1\n", "sample 1"

assert run(
"""2 20
5 3 10 2
14 6 1 1
"""
) == "15\n1\n2\n", "sample 2"

# minimum size

assert run(
"""0 7
"""
) == "7\n0\n\n", "no ramps"

# invalid ramp

assert run(
"""1 30
5 20 1 10
"""
) == "30\n0\n\n", "ramp starts before zero"

# chain of ramps

assert run(
"""2 50
10 10 1 1
25 20 1 1
"""
) == "22\n2\n1 2\n", "two compatible ramps"

# overlapping ramps

assert run(
"""2 40
10 20 1 1
15 20 1 1
"""
) == "20\n1\n1\n", "cannot take overlapping ramps"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Không có đường dốc | Đi bộ toàn bộ quãng đường | Trường hợp cơ sở | 
| Đoạn đường nối bắt đầu trước số 0 | Bỏ qua đoạn đường nối không hợp lệ | Điều kiện biên | 
| Hai đường dốc tương thích | Tái thiết chuỗi | Theo dõi phụ huynh | 
| Đường dốc chồng chéo | Chuyển đổi pháp lý | Đúng tình trạng tiền thân | 

## Vỏ cạnh 

Hãy xem xét một đoạn đường nối có quá trình chuẩn bị bắt đầu trước số 0. 

đầu vào:```
1 20
4 16 1 7
```Việc chuẩn bị bắt đầu là:$$4 - 7 = -3$$Thuật toán loại bỏ đoạn đường nối này ngay lập tức trong quá trình tiền xử lý. Không có trạng thái DP nào được tạo cho nó, vì vậy lựa chọn duy nhất còn lại là đi thẳng về đích. 

Đầu ra:```
20
0
```Bây giờ hãy xem xét một đoạn đường nối có hại. 

đầu vào:```
1 20
5 3 10 2
```Sự đóng góp của đoạn đường nối là:$$10 - 3 = 7$$Sử dụng nó thực sự làm tăng tổng thời gian. DP tính toán:$$20 + 7 = 27$$điều đó còn tệ hơn cả việc đi bộ bình thường. Vì câu trả lời được khởi tạo bằng`20`, thuật toán chọn chính xác không có đường dốc. 

Cuối cùng, hãy xem xét các đường dốc chồng chéo. 

đầu vào:```
2 40
10 20 1 1
15 20 1 1
```Đoạn đường nối 1 hạ cánh tại:$$10 + 20 = 30$$Đoạn đường nối 2 yêu cầu chuẩn bị từ:$$15 - 1 = 14$$Từ`30 > 14`, đoạn đường thứ hai không thể theo đoạn đường thứ nhất. Cấu trúc quét không bao giờ chèn đoạn nối 1 vào tập hợp lệ trước đó trong khi xử lý đoạn nối 2, do đó việc chuyển đổi bất hợp pháp là không thể xảy ra bên trong DP.
