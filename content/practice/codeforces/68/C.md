---
title: "CF 68C - Synchrophasotron"
description: "Chúng ta có đồ thị chu kỳ có hướng trên các đỉnh 1...n. Với mỗi cặp i < j, có đúng một cạnh có hướng từ i đến j. Mỗi ống có ba thông số. Nó phải chứa từ l đến h đơn vị dòng chảy, ngay cả khi chúng ta không muốn sử dụng đường ống đó."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force"]
categories: ["algorithms"]
codeforces_contest: 68
codeforces_index: "C"
codeforces_contest_name: "Codeforces Beta Round 62"
rating: 2200
weight: 68
solve_time_s: 423
verified: true
draft: false
---

[CF 68C - Synchrophasotron](https://codeforces.com/problemset/problem/68/C) 

**Đánh giá:** 2200 
**Tags:** vũ lực 
**Thời gian giải:** 7 phút 3 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị chu kỳ có hướng trên các đỉnh`1...n`. Đối với mỗi cặp`i < j`, có đúng một cạnh có hướng từ`i`ĐẾN`j`. 

Mỗi ống có ba thông số. Nó phải mang giữa`l`Và`h`đơn vị bao gồm dòng chảy, ngay cả khi chúng ta không muốn sử dụng đường ống đó. Nếu lưu lượng thực tế qua đường ống là dương thì chúng ta phải trả chi phí kích hoạt`a`cộng với bình phương của dòng chảy. Nếu dòng tiền bằng 0, chúng tôi không phải trả gì cả. 

Nhiên liệu đi vào nút`1`và rời khỏi nút`n`. Mỗi nút trung gian phải bảo toàn luồng, luồng vào bằng luồng ra. Tất cả các luồng đều là số nguyên. 

Trong số tất cả các phép gán luồng khả thi, trước tiên chúng tôi muốn giảm thiểu tổng số lượng nút tiếp cận`n`. Trong số tất cả các giải pháp có tổng lưu lượng tối thiểu đó, chúng tôi muốn tổng chi phí tối đa có thể. 

Biểu đồ rất nhỏ.`n ≤ 6`, vậy số cạnh nhiều nhất là`15`. Mọi công suất cạnh tối đa là`5`. Điều này làm thay đổi hoàn toàn bản chất của vấn đề. Chúng tôi không tìm kiếm một thuật toán tối ưu hóa đa thức phức tạp. Không gian tìm kiếm đủ nhỏ để tác động mạnh mẽ lên các luồng biên trở nên thực tế. 

Tuy nhiên, lực lượng vũ phu ngây thơ đối với tất cả các nhiệm vụ là quá lớn. Mọi cạnh đều có thể nhận giá trị`0...5`, vậy trong trường hợp xấu nhất có`6^15 ≈ 4.7e11`bài tập. Ngay cả với việc cắt tỉa tích cực, điều đó vẫn vô vọng. 

Năng lực nhỏ mới là chìa khóa thực sự. Tổng lượng luồng di chuyển qua mạng không bao giờ có thể trở nên lớn. Vì chỉ có các cạnh rời khỏi nút`1`tạo ra dòng chảy và mỗi cạnh như vậy có công suất tối đa`5`, tổng luồng vào toàn bộ đồ thị nhiều nhất là`5 * (n-1) ≤ 25`. 

Điều đó có nghĩa là chúng ta có thể suy nghĩ về mặt cân bằng nút thay vì lựa chọn cạnh độc lập. 

Có một số trường hợp cạnh tinh tế. 

Đầu tiên là giới hạn dưới áp dụng ngay cả với các cạnh không hoạt động. Coi như:```
3
1 2 0 2 0
2 3 3 5 0
1 3 0 5 0
```Bờ rìa`2 -> 3`ít nhất phải mang theo`3`, vậy nút`2`phải nhận được ít nhất`3`. Nhưng cạnh`1 -> 2`có thể mang nhiều nhất`2`. Câu trả lời đúng là:```
-1 -1
```Việc triển khai bất cẩn chỉ kiểm tra dung lượng trên các biên đã sử dụng sẽ chấp nhận mạng một cách không chính xác. 

Một cái bẫy khác là giải pháp không dòng chảy. Ví dụ:```
2
1 2 0 5 3
```Chúng tôi có thể gửi 0 đơn vị qua cạnh duy nhất. Vì cạnh không hoạt động nên chi phí cũng bằng 0. Đầu ra đúng là:```
0 0
```Nếu chúng ta luôn thêm chi phí kích hoạt bất cứ khi nào có một cạnh tồn tại thì chúng ta sẽ nhận được câu trả lời sai. 

Hàm chi phí cũng hoạt động theo cách không trực quan. Vì chúng tôi tối đa hóa tổng bình phương nên việc chia luồng qua nhiều cạnh có thể làm tăng tổng số vì mỗi cạnh được kích hoạt đóng góp phần thưởng kích hoạt riêng. Ví dụ:```
4
1 2 0 2 5
1 3 0 2 5
1 4 0 2 0
2 3 0 2 5
2 4 0 2 5
3 4 0 2 5
```Giả sử tổng lưu lượng khả thi tối thiểu là`2`. Việc gửi cả hai đơn vị dọc theo một đường dẫn sẽ tạo ra ít cạnh được kích hoạt hơn so với việc chia các đơn vị trên các đường dẫn khác nhau. Câu trả lời tối ưu thích kích hoạt nhiều cạnh hơn khi có thể. 

Điều tinh tế cuối cùng là giới hạn dưới có thể buộc hành vi giống như lưu thông bên trong DAG. Vì đồ thị chỉ đi từ các đỉnh nhỏ hơn đến các đỉnh lớn hơn nên các chu trình là không thể, nhưng các đỉnh trung gian vẫn có thể bị buộc phải chuyển tiếp luồng bắt buộc do các giới hạn dưới đi ra. 

## Phương pháp tiếp cận 

Lực lượng vũ phu trực tiếp nhất là gán một giá trị cho mọi cạnh một cách độc lập. Đối với mỗi cạnh, chúng tôi thử mọi luồng số nguyên giữa`0`Và`h`, từ chối các phép gán vi phạm giới hạn dưới hoặc bảo toàn luồng, sau đó tính tổng luồng và chi phí. 

Điều này đúng vì mọi giải pháp khả thi đều tương ứng với chính xác một phép gán giá trị cạnh. 

Vấn đề là kích thước của không gian tìm kiếm. Với`15`các cạnh và`6`các lựa chọn trên mỗi cạnh, chúng tôi nhận được khoảng`4.7e11`tiểu bang. Ngay cả khi mỗi trạng thái nhận một lệnh CPU, điều đó vẫn không thể thực hiện được. 

Cấu trúc đồ thị cho chúng ta cách suy nghĩ vấn đề tốt hơn. 

Với mọi đỉnh ngoại trừ`1`Và`n`, việc bảo toàn luồng sẽ xác định tổng luồng đi khi chúng ta biết luồng vào. Vì dung lượng rất nhỏ nên tổng số lượng vào bất kỳ nút nào cũng rất nhỏ. 

Điều này gợi ý lập trình động trên các đỉnh và điểm cân bằng. 

Xử lý các đỉnh từ trái sang phải. Khi chúng ta đạt đến đỉnh`v`, tất cả các cạnh đến vào`v`đã được cố định vì chúng đến từ các đỉnh nhỏ hơn. Tổng của chúng xác định chính xác lượng luồng đi phải để lại`v`. 

Bây giờ chúng ta chỉ cần phân phối số lượng cần thiết đó giữa các cạnh`v -> u`Ở đâu`u > v`, tôn trọng giới hạn dưới và trên. 

Số lượng phân phối có thể là nhỏ. Một đỉnh có nhiều nhất`5`các cạnh đi ra và mỗi cạnh mang nhiều nhất`5`. Tổng lưu lượng đi ra nhiều nhất là`25`. Việc liệt kê tất cả các phân phối trở nên khả thi. 

Nhận xét quan trọng là một khi chúng ta xử lý các đỉnh theo thứ tự thì không có quyết định nào trong tương lai có thể thay đổi sự cân bằng của một đỉnh đã được xử lý. Điều đó biến vấn đề khả thi toàn cầu thành vấn đề chuyển đổi cục bộ. 

Chúng tôi giữ các trạng thái DP mô tả lượng luồng đến mà mỗi đỉnh trong tương lai đã tích lũy cho đến nay. Khi xử lý đỉnh`v`, chúng tôi biết số tiền hiện tại đến từ tiểu bang. Chúng tôi liệt kê tất cả các nhiệm vụ gửi đi hợp lệ và cập nhật số dư trong tương lai. 

Tổng không gian trạng thái vẫn có thể quản lý được vì chỉ có`5`các đỉnh tương lai và mỗi số dư nhiều nhất là`25`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên tất cả các bài tập biên |$O(6^{15})$|$O(15)$| Quá chậm | 
| DP trên số dư đỉnh | đại khái$O(\text{states} \times \text{transitions})$với ít hơn vài triệu hoạt động |$O(\text{states})$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các cạnh và lưu trữ giới hạn dưới của chúng`l`, giới hạn trên`h`và chi phí kích hoạt`a`. 
2. Xác định trạng thái DP sau khi xử lý các đỉnh`1...v-1`. 

Các cửa hàng trạng thái, cho mọi đỉnh trong tương lai`u ≥ v`, bao nhiêu luồng đến đã được chỉ định cho`u`. 

Vì các cạnh chỉ tiến về phía trước nên các quyết định trong tương lai không thể sửa đổi số dư của các đỉnh trước đó. 
3. Bắt đầu với tất cả số dư bằng 0 trước khi xử lý bất kỳ đỉnh nào. 
4. Khi xử lý đỉnh`v`, xác định lượng dòng chảy phải rời khỏi nó. 

Vì`v = 1`, số tiền này không bị hạn chế vì nút`1`là nguồn. Tổng số đi của nó sẽ trở thành bất cứ thứ gì chúng ta chỉ định. 

Đối với các đỉnh trung gian, luồng đi ra phải bằng luồng đi vào đã được tích lũy cho đỉnh đó trong trạng thái. 
5. Liệt kê mọi phép gán có thể có cho các cạnh đi`v -> u`. 

Mỗi luồng cạnh phải nằm trong`[l, h]`. 

Đối với các đỉnh trung gian, tổng số luồng đi được chỉ định phải bằng số lượng đi được yêu cầu. 

Đối với đỉnh`1`, mọi tổng số đều được phép vì đó là nguồn. 
6. Với mỗi bài tập, hãy tính: 

- chi phí tăng thêm, 
- luồng đến bổ sung góp phần tạo nên các đỉnh trong tương lai, 
- tổng lưu lượng cuối cùng đến nút`n`. 
7. Cập nhật trạng thái DP cho đỉnh tiếp theo. 

Nếu hai cách đạt đến cùng một trạng thái với tổng lưu lượng cuối cùng như nhau, hãy giữ chi phí lớn hơn. 
8. Sau khi xử lý tất cả các đỉnh, kiểm tra trạng thái nơi nút`n`đã tích lũy một số dòng chảy. 

Số lượng nút vào`n`chính xác là tổng số được chuyển đến synchrophasotron. 
9. Chọn tổng lưu lượng khả thi nhỏ nhất. Trong đó hãy chọn mức chi phí lớn nhất. 

### Tại sao nó hoạt động 

Bất biến là sau khi xử lý các đỉnh`1...v-1`, trạng thái DP ghi lại chính xác tất cả luồng đã bị ép vào các đỉnh`v...n`. 

Vì đồ thị không có chu kỳ và các cạnh luôn hướng về phía trước nên các quyết định trong tương lai không thể làm thay đổi sự cân bằng của các đỉnh được xử lý. Mọi luồng toàn cầu khả thi đều tương ứng với một chuỗi nhiệm vụ đi ra cục bộ trong DP và mọi chuỗi do DP tạo ra đều đáp ứng tất cả các ràng buộc về bảo tồn và năng lực. 

DP khám phá tất cả các phân phối khả thi của luồng đi ở mọi đỉnh. Vì quá trình chuyển đổi duy trì sự cân bằng và năng lực chính xác nên không có giải pháp không hợp lệ nào còn tồn tại. Vì mọi giải pháp hợp lệ đều có thể được xây dựng lại theo từng đỉnh nên không có giải pháp khả thi nào bị bỏ sót. 

## Giải pháp Python```python
import sys
from collections import defaultdict
from itertools import product

input = sys.stdin.readline

def solve():
    n = int(input())

    l = [[0] * n for _ in range(n)]
    h = [[0] * n for _ in range(n)]
    a = [[0] * n for _ in range(n)]

    for _ in range(n * (n - 1) // 2):
        s, f, lo, hi, cost = map(int, input().split())
        s -= 1
        f -= 1

        l[s][f] = lo
        h[s][f] = hi
        a[s][f] = cost

    # state:
    # tuple of accumulated incoming flow for every vertex
    # dp[state] = maximum cost

    start = tuple([0] * n)
    dp = {start: 0}

    for v in range(n - 1):
        ndp = dict()

        targets = list(range(v + 1, n))

        ranges = [range(l[v][u], h[v][u] + 1) for u in targets]

        for state, cur_cost in dp.items():

            required = state[v]

            for flows in product(*ranges):

                total_out = sum(flows)

                # conservation for intermediate vertices
                if v != 0 and total_out != required:
                    continue

                new_state = list(state)

                add_cost = 0

                for idx, u in enumerate(targets):
                    f = flows[idx]
                    new_state[u] += f

                    if f > 0:
                        add_cost += a[v][u] + f * f

                new_state = tuple(new_state)

                value = cur_cost + add_cost

                if new_state not in ndp or ndp[new_state] < value:
                    ndp[new_state] = value

        dp = ndp

    best_flow = None
    best_cost = -1

    for state, cost in dp.items():
        flow = state[n - 1]

        if best_flow is None or flow < best_flow:
            best_flow = flow
            best_cost = cost
        elif flow == best_flow:
            best_cost = max(best_cost, cost)

    if best_flow is None:
        print("-1 -1")
    else:
        print(best_flow, best_cost)

solve()
```Ý tưởng cốt lõi trong việc thực hiện là`state[i]`lưu trữ bao nhiêu đỉnh luồng đến`i`đã được tích lũy từ các đỉnh được xử lý. 

Khi xử lý đỉnh`v`, tất cả các cạnh đến vào`v`đã được cố định vì mọi cạnh như vậy đều bắt nguồn từ một đỉnh nhỏ hơn. Điều đó có nghĩa`state[v]`là số tiền chính xác phải để lại`v`nếu như`v`là đỉnh trung gian 

Quá trình chuyển đổi liệt kê tất cả các nhiệm vụ gửi đi có thể bằng cách sử dụng`itertools.product`. Điều này hoạt động vì mỗi công suất cạnh tối đa là`5`và có nhiều nhất`5`các cạnh đi ra. 

điều kiện```
if v != 0 and total_out != required:
```thực hiện bảo toàn dòng chảy. đỉnh`1`bị loại trừ vì đây là nguồn và có thể chèn luồng tùy ý. 

Một điểm tinh tế khác là chi phí kích hoạt:```
if f > 0:
    add_cost += a[v][u] + f * f
```Chúng tôi chỉ trả chi phí kích hoạt cố định nếu cạnh thực sự mang dòng chảy dương. 

DP chỉ lưu trữ chi phí tối đa có thể đạt được cho mỗi trạng thái. Nếu hai công trình xây dựng khác nhau tạo ra số dư tích lũy giống nhau thì khả năng xảy ra trong tương lai sẽ giống hệt nhau, do đó việc giữ chi phí lớn hơn sẽ lấn át khả năng chi phí nhỏ hơn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2
1 2 1 2 3
```Đỉnh xử lý`1`: 

| Trạng thái trước | Luồng đã chọn | Tiểu bang mới | Chi phí bổ sung | 
| --- | --- | --- | --- | 
| (0,0) | 1 | (0,1) | 4 | 
| (0,0) | 2 | (0,2) | 7 | 

Các trạng thái cuối cùng: 

| Luồng cuối cùng tới nút 2 | Chi phí | 
| --- | --- | 
| 1 | 4 | 
| 2 | 7 | 

Luồng phân phối khả thi tối thiểu là`1`, và trong số các trạng thái đó, chi phí tối đa là`4`. 

Đầu ra:```
1 4
```Dấu vết này cho thấy đỉnh nguồn được xử lý khác nhau như thế nào. Nó được phép tạo ra luồng đi tùy ý trong phạm vi dung lượng biên. 

### Ví dụ 2 

đầu vào:```
3
1 2 0 2 0
1 3 0 5 0
2 3 3 5 0
```Đỉnh xử lý`1`: 

| Luồng 1->2 | Luồng 1->3 | Trạng thái sau | 
| --- | --- | --- | 
| 0 | 0 | (0,0,0) | 
| 1 | 0 | (0,1,0) | 
| 2 | 0 | (0,2,0) | 
| ... | ... | ... | 

Bây giờ xử lý đỉnh`2`. 

Nếu trạng thái là`(0,0,0)`, luồng đi yêu cầu là`0`, nhưng cạnh`2->3`ít nhất phải mang theo`3`. 

| Bắt buộc gửi đi | Dòng chảy cạnh được phép | Có hiệu lực? | 
| --- | --- | --- | 
| 0 | 3..5 | Không | 
| 1 | 3..5 | Không | 
| 2 | 3..5 | Không | 

Không có nhà nước nào tồn tại được. 

Đầu ra:```
-1 -1
```Điều này chứng tỏ các giới hạn dưới có thể buộc phải không khả thi như thế nào mặc dù mọi cạnh riêng lẻ đều có vẻ hợp lệ cục bộ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | đại khái$O(S \cdot T)$|`S`là số trạng thái cân bằng có thể đạt được,`T`là số lượng tổ hợp luồng đi | 
| Không gian |$O(S)$| DP lưu trữ một giá trị cho mỗi trạng thái có thể truy cập | 

Trong thực tế, các giới hạn là rất nhỏ. Có nhiều nhất`15`các cạnh và dung lượng của mọi cạnh là nhiều nhất`5`. Số lượng trạng thái có thể tiếp cận vẫn nhỏ ở mức vừa phải và giải pháp dễ dàng phù hợp với`3s`giới hạn trong Python. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from collections import defaultdict
from itertools import product

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n = int(input())

    l = [[0] * n for _ in range(n)]
    h = [[0] * n for _ in range(n)]
    a = [[0] * n for _ in range(n)]

    for _ in range(n * (n - 1) // 2):
        s, f, lo, hi, cost = map(int, input().split())
        s -= 1
        f -= 1

        l[s][f] = lo
        h[s][f] = hi
        a[s][f] = cost

    start = tuple([0] * n)
    dp = {start: 0}

    for v in range(n - 1):
        ndp = dict()

        targets = list(range(v + 1, n))
        ranges = [range(l[v][u], h[v][u] + 1) for u in targets]

        for state, cur_cost in dp.items():

            required = state[v]

            for flows in product(*ranges):

                total_out = sum(flows)

                if v != 0 and total_out != required:
                    continue

                new_state = list(state)

                add_cost = 0

                for idx, u in enumerate(targets):
                    f = flows[idx]
                    new_state[u] += f

                    if f > 0:
                        add_cost += a[v][u] + f * f

                new_state = tuple(new_state)

                value = cur_cost + add_cost

                if new_state not in ndp or ndp[new_state] < value:
                    ndp[new_state] = value

        dp = ndp

    best_flow = None
    best_cost = -1

    for state, cost in dp.items():
        flow = state[n - 1]

        if best_flow is None or flow < best_flow:
            best_flow = flow
            best_cost = cost
        elif flow == best_flow:
            best_cost = max(best_cost, cost)

    if best_flow is None:
        return "-1 -1"

    return f"{best_flow} {best_cost}"

# provided sample
assert run(
"""2
1 2 1 2 3
"""
) == "1 4", "sample 1"

# zero-flow solution
assert run(
"""2
1 2 0 5 3
"""
) == "0 0", "zero flow"

# impossible because of lower bounds
assert run(
"""3
1 2 0 2 0
1 3 0 5 0
2 3 3 5 0
"""
) == "-1 -1", "infeasible network"

# forcing relay through intermediate node
assert run(
"""3
1 2 2 2 1
1 3 0 0 0
2 3 2 2 1
"""
) == "2 10", "exact forced flow"

# activation-cost maximization
assert run(
"""3
1 2 0 1 5
1 3 0 2 0
2 3 0 1 5
"""
) == "0 0", "minimum flow dominates cost"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cạnh đơn có giới hạn dưới 1 |`1 4`| Tính đúng đắn cơ bản | 
| Cạnh có giới hạn dưới 0 |`0 0`| Xử lý đúng các cạnh không hoạt động | 
| Hạn chế chuyển tiếp không thể |`-1 -1`| Giới hạn dưới gây bất khả thi | 
| Dòng chảy cưỡng bức chính xác qua đường dẫn |`2 10`| Tính chính xác bảo toàn dòng chảy | 
| Cạnh tranh các tuyến đường chi phí cao |`0 0`| Tối ưu hóa từ điển, giảm thiểu luồng trước | 

## Vỏ cạnh 

Hãy xem xét lại trường hợp chuyển tiếp không khả thi:```
3
1 2 0 2 0
1 3 0 5 0
2 3 3 5 0
```Sau khi xử lý nút`1`, nút luồng đến tối đa`2`có thể tích lũy là`2`. 

Khi xử lý nút`2`, sự bảo toàn đòi hỏi luồng đi bằng luồng vào. Nhưng cạnh`2 -> 3`có giới hạn dưới`3`, vì vậy mỗi lần chuyển đổi yêu cầu ít nhất`3`các đơn vị xuất cảnh. Không có nhà nước nào tồn tại được. DP tự nhiên loại bỏ tất cả các cấu hình không thể thực hiện được và trả về`-1 -1`. 

Bây giờ hãy xem xét trường hợp luồng không:```
2
1 2 0 5 3
```DP liệt kê các luồng`0...5`trên cạnh duy nhất. Chảy`0`hợp lệ vì nó thỏa mãn giới hạn dưới. Vì cạnh không hoạt động nên không có chi phí kích hoạt nào được thêm vào. Câu trả lời cuối cùng trở thành`0 0`. 

Một lỗi phổ biến là thêm chi phí kích hoạt bất cứ khi nào một cạnh tồn tại thay vì chỉ khi luồng của nó dương. 

Cuối cùng, hãy xem xét trường hợp tối đa hóa chi phí sẽ ưu tiên kích hoạt nhiều cạnh hơn:```
4
1 2 0 1 5
1 3 0 1 5
1 4 0 2 0
2 4 0 1 5
3 4 0 1 5
2 3 0 0 0
```Giả sử chúng ta muốn tổng lưu lượng được phân phối`2`. 

Một khả năng là gửi trực tiếp cả hai đơn vị`1 -> 4`, trị giá`4`. 

Một khả năng khác là chia tách:`1 -> 2 = 1`,`2 -> 4 = 1`,`1 -> 3 = 1`,`3 -> 4 = 1`. 

Điều này kích hoạt bốn cạnh đắt tiền, đưa ra chi phí:```
(5+1) + (5+1) + (5+1) + (5+1) = 24
```DP giữ chi phí lớn hơn giữa các trạng thái có số dư bằng nhau, do đó, nó ưu tiên cấu hình phân chia một cách chính xác.
