---
title: "CF 1941E - Cầu Rudolf và k"
description: "Mỗi hàng sông có thể được xử lý độc lập. Dọc theo một hàng đã chọn, chúng ta muốn đặt các điểm hỗ trợ trong một số cột sao cho cột đầu tiên và cột cuối cùng luôn chứa các điểm hỗ trợ và số lượng ô bị bỏ qua giữa hai điểm hỗ trợ lân cận nhiều nhất là d."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "dp", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1941
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 933 (Div. 3)"
rating: 1600
weight: 1941
solve_time_s: 126
verified: true
draft: false
---
[CF 1941E - Cầu Rudolf và k](https://codeforces.com/problemset/problem/1941/E) 

**Đánh giá:** 1600 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, dp, hai con trỏ 
**Thời gian giải:** 2m 6s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi hàng sông có thể được xử lý độc lập. Dọc theo một hàng đã chọn, chúng ta muốn đặt các điểm hỗ trợ trong một số cột sao cho cột đầu tiên và cột cuối cùng luôn chứa các điểm hỗ trợ và số lượng ô bị bỏ qua giữa hai điểm hỗ trợ lân cận là nhiều nhất.`d`. 

Nếu gối đỡ đặt ở cột`j1`Và`j2`, điều kiện khe hở là:$$|j_1 - j_2| - 1 \le d$$tương đương với:$$|j_1 - j_2| \le d + 1$$Lắp đặt giá đỡ tại cột`j`chi phí`a[i][j] + 1`. 

Đối với mỗi hàng, chúng ta cần chi phí cầu tối thiểu có thể. Sau đó, chúng ta phải chọn`k`các hàng liên tiếp có chi phí cầu có tổng số tiền tối thiểu. 

Cấu trúc tự nhiên chia thành hai vấn đề phụ: 

1. Tính chi phí cầu tối ưu cho một hàng. 
2. Tìm tổng nhỏ nhất của tất cả các nhóm liên tiếp của`k`hàng. 

Các ràng buộc định hình mạnh mẽ giải pháp. Tổng số ô trên tất cả các trường hợp thử nghiệm nhiều nhất là`2 * 10^5`, do đó, điều gì đó gần giống tuyến tính trong kích thước đầu vào được mong đợi. Thuật toán bậc hai trên các cột sẽ thất bại vì một hàng có thể chứa`2 * 10^5`cột. Thậm chí`O(m * d)`trở nên nguy hiểm khi`d`là lớn. 

Phần khó khăn nhất là việc xây cầu theo một hàng. Một cách tiếp cận tham lam bất cẩn sẽ thất bại vì việc lựa chọn sự hỗ trợ gần đó rẻ nhất hiện tại có thể buộc phải có nhiều sự hỗ trợ đắt tiền sau này. 

Hãy xem xét hàng này:```
0 1 100 1 0
```với`d = 1`. 

Bạn không thể nhảy trực tiếp từ cột`1`vào cột`5`vì khoảng cách giữa các giá đỡ sẽ bỏ qua 3 ô. Giải pháp tối ưu phải sử dụng các hỗ trợ trung gian. 

Một trường hợp tế nhị khác xuất hiện khi`d`là rất lớn. 

Ví dụ:```
0 5 7 2 0
```với`d = 10`. 

Bây giờ chỉ riêng hỗ trợ đầu tiên và cuối cùng đã thỏa mãn điều kiện khoảng cách, vì vậy câu trả lời đơn giản là:```
1 + 1 = 2
```Việc triển khai có lỗi vẫn có thể buộc phải có những hỗ trợ ở giữa không cần thiết. 

Một cái bẫy dễ dàng hơn nữa xuất phát từ định nghĩa khoảng cách. Vấn đề xác định khoảng cách là các ô bị bỏ qua, không phải là sự khác biệt cột tuyệt đối. 

Nếu như`d = 1`, hỗ trợ có thể khác nhau nhiều nhất`2`trong chỉ mục cột. 

Ví dụ:```
0 4 0
```với`d = 1`. 

Hỗ trợ tại các cột`1`Và`3`hợp lệ vì chúng bỏ qua chính xác một ô. 

Một giải pháp sử dụng`|j1 - j2| <= d`sẽ từ chối điều này một cách không chính xác. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là lập trình động đơn giản. 

Cho phép`dp[j]`là chi phí tối thiểu để đặt một giá đỡ tại cột`j`. Sau đó:$$dp[j] = cost(j) + \min(dp[t])$$Ở đâu`t`phạm vi trên tất cả các vị trí hỗ trợ trước đó có thể kết nối với`j`. 

Vì các hỗ trợ có thể khác nhau nhiều nhất`d + 1`trong chỉ mục cột:$$j - (d+1) \le t < j$$Đối với mỗi cột, chúng tôi quét tất cả các cột hợp lệ trước đó và lấy mức tối thiểu. 

Điều đó mang lại:$$O(m \cdot d)$$thời gian cho một hàng. 

Trong trường hợp xấu nhất,`d`có thể`m`, vì vậy điều này trở thành`O(m^2)`. Với`m = 2 * 10^5`, điều này hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là quá trình chuyển đổi chỉ yêu cầu giá trị tối thiểu bên trong cửa sổ trượt. 

Đối với cột`j`, chúng ta cần:$$\min(dp[j-d-1], \dots, dp[j-1])$$BẰNG`j`tăng lên thì khoảng này cũng trượt sang bên phải. Các vấn đề tối thiểu về cửa sổ trượt chính xác là những gì mà hàng đợi đơn điệu giải quyết một cách hiệu quả. 

Thay vì tính toán lại mức tối thiểu mỗi lần, chúng tôi duy trì một deque chứa các cột ứng cử viên với mức tăng dần`dp`các giá trị. Mặt trước luôn lưu giá trị nhỏ nhất`dp`. 

Điều đó làm giảm độ phức tạp trên mỗi hàng từ bậc hai sang tuyến tính. 

Sau khi tính toán chi phí cầu tối ưu cho mỗi hàng, giai đoạn thứ hai thật dễ dàng. Chúng ta cần số tiền tối thiểu là`k`chi phí hàng liên tiếp, đó là một cửa sổ trượt khác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n · m · d) | O(m) | Quá chậm | 
| Tối ưu | O(n · m) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đối với mỗi hàng, hãy tạo một mảng DP trong đó`dp[j]`biểu thị tổng chi phí tối thiểu nếu hỗ trợ được cài đặt cuối cùng ở cột`j`. 
2. Khởi tạo cột đầu tiên. 

Hỗ trợ đầu tiên là bắt buộc và chi phí của nó là:$$a[i][1] + 1 = 1$$Vì thế:$$dp[0] = 1$$1. Duy trì một deque đơn điệu để lưu trữ các vị trí trước đó của ứng viên. 

Deque giữ các chỉ số có`dp`các giá trị ngày càng tăng. Mặt trước luôn chứa tối thiểu`dp`trong số các cột hiện có thể truy cập được. 
2. Xử lý các cột từ trái qua phải. 

Đối với cột`j`, hãy xóa các chỉ số ở phía trước nếu chúng ở quá xa:$$idx < j - (d + 1)$$bởi vì những hỗ trợ đó không còn có thể kết nối với`j`. 

1. Tính toán quá trình chuyển đổi. 

Sự hỗ trợ tốt nhất trước đó là ở mặt trước deque:$$dp[j] = dp[best] + a[i][j] + 1$$1. Chèn cột hiện tại vào deque. 

Trong khi mặt sau có một`dp`giá trị lớn hơn hoặc bằng`dp[j]`, loại bỏ nó. Những vị thế như vậy không bao giờ hữu ích nữa vì vị thế hiện tại vừa rẻ hơn vừa mới hơn. 
2. Sau khi kết thúc hàng, lưu trữ`dp[m-1]`là chi phí cầu tối thiểu cho hàng đó. 
3. Khi đã biết tất cả chi phí của hàng, hãy tính tổng tối thiểu của`k`hàng liên tiếp bằng cửa sổ trượt. 
4. Xuất ra tổng cửa sổ nhỏ nhất. 

### Tại sao nó hoạt động 

DP đúng vì mọi cây cầu hợp lệ đều kết thúc ở cột`j`phải đến từ một số hỗ trợ trước đó trong khoảng cách`d + 1`. Phép truy toán kiểm tra tất cả các khả năng như vậy và chọn khả năng rẻ nhất. 

Deque luôn chứa chính xác các cột hợp lệ trước đó cho vị trí hiện tại, được sắp xếp sao cho giá trị nhỏ nhất`dp`vẫn ở phía trước. Vì các vị trí không hợp lệ sẽ bị xóa ngay lập tức và các vị trí thống trị sẽ bị loại bỏ từ phía sau, nên mức tối thiểu của deque luôn đúng. 

Vì mỗi cột vào và ra khỏi deque nhiều nhất một lần nên tổng công việc vẫn tuyến tính. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

def solve():
    t = int(input())
    
    for _ in range(t):
        n, m, k, d = map(int, input().split())
        grid = [list(map(int, input().split())) for _ in range(n)]

        row_costs = []

        for row in grid:
            dp = [0] * m
            dp[0] = 1

            dq = deque([0])

            for j in range(1, m):
                while dq and dq[0] < j - (d + 1):
                    dq.popleft()

                best = dq[0]
                dp[j] = dp[best] + row[j] + 1

                while dq and dp[dq[-1]] >= dp[j]:
                    dq.pop()

                dq.append(j)

            row_costs.append(dp[-1])

        current = sum(row_costs[:k])
        answer = current

        for i in range(k, n):
            current += row_costs[i]
            current -= row_costs[i - k]
            answer = min(answer, current)

        print(answer)

solve()
```Giai đoạn đầu tiên tính toán chi phí cầu tối ưu cho mỗi hàng một cách độc lập.`dp[j]`lưu trữ tổng chi phí tối thiểu nếu chúng tôi đặt hỗ trợ tại cột`j`. Vì cột đầu tiên phải luôn chứa phần hỗ trợ,`dp[0] = 1`. 

Deque lưu trữ các cột trước đó ứng cử viên cho quá trình chuyển đổi. Trước khi xử lý cột`j`, chúng tôi loại bỏ các cột quá xa để kết nối hợp pháp. 

Sự chuyển tiếp:```
dp[j] = dp[best] + row[j] + 1
```sử dụng trạng thái tối thiểu có thể truy cập trước đó. 

Tính chất đơn điệu được duy trì với:```
while dq and dp[dq[-1]] >= dp[j]:
    dq.pop()
```Điều này giữ cho deque tăng thêm`dp`giá trị, do đó mặt trước luôn là tối ưu. 

Một chi tiết tinh tế là điều kiện hợp lệ:```
dq[0] < j - (d + 1)
```Cây cầu cho phép bỏ qua nhiều nhất`d`các ô, có nghĩa là các hỗ trợ có thể khác nhau nhiều nhất`d + 1`trong chỉ mục. 

Giai đoạn thứ hai sử dụng cửa sổ trượt tiêu chuẩn theo chi phí hàng. Vì chúng ta chỉ cần các hàng liên tiếp, tổng tiền tố hoặc tổng luân phiên đều hoạt động. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hàng đầu vào:```
0 1 2 3 2 1 2 3 3 2 0
```với`d = 4`. 

| Cột | Chi phí | Phạm vi trước đó hợp lệ | DP trước hay nhất | DP mới | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | bắt đầu | 0 | 1 | 
| 2 | 2 | [1] | 1 | 3 | 
| 3 | 3 | [1,2] | 1 | 4 | 
| 4 | 4 | [1,2,3] | 1 | 5 | 
| 5 | 3 | [1,2,3,4] | 1 | 4 | 
| 6 | 2 | [1,2,3,4,5] | 1 | 3 | 
| 7 | 3 | trượt | 3 | 6 | 
| 8 | 4 | trượt | 3 | 7 | 
| 9 | 4 | trượt | 3 | 7 | 
| 10 | 3 | trượt | 3 | 6 | 
| 11 | 1 | trượt | 3 | 4 | 

Chi phí hàng cuối cùng là`4`. 

Dấu vết này cho thấy tại sao deque lại quan trọng. Mặc dù nhiều cột trước đó hợp lệ nhưng chúng ta chỉ cần DP tối thiểu trong số đó. 

### Ví dụ 2 

đầu vào:```
0 3 3 0
```với`d = 1`. 

| Cột | Có thể truy cập từ | DP | 
| --- | --- | --- | 
| 1 | bắt đầu | 1 | 
| 2 | 1 | 5 | 
| 3 | 1,2 | 5 | 
| 4 | 2,3 | 6 | 

Chi phí cuối cùng là`6`. 

Ví dụ này thực hiện việc giải thích chính xác quy tắc khoảng cách. Các hỗ trợ có thể khác nhau 2 về chỉ mục vì chỉ một ô bị bỏ qua. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n · m) | Mỗi cột vào và rời khỏi deque một lần | 
| Không gian | O(m) | Mảng DP và deque cho một hàng | 

Tổng số ô trên tất cả các trường hợp thử nghiệm nhiều nhất là`2 * 10^5`, do đó việc xử lý tuyến tính vừa vặn thoải mái trong giới hạn thời gian. Việc sử dụng bộ nhớ cũng nhỏ vì chúng tôi chỉ lưu trữ một hàng DP mỗi lần. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from collections import deque

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    out = []

    t = int(input())

    for _ in range(t):
        n, m, k, d = map(int, input().split())
        grid = [list(map(int, input().split())) for _ in range(n)]

        row_costs = []

        for row in grid:
            dp = [0] * m
            dp[0] = 1

            dq = deque([0])

            for j in range(1, m):
                while dq and dq[0] < j - (d + 1):
                    dq.popleft()

                dp[j] = dp[dq[0]] + row[j] + 1

                while dq and dp[dq[-1]] >= dp[j]:
                    dq.pop()

                dq.append(j)

            row_costs.append(dp[-1])

        cur = sum(row_costs[:k])
        ans = cur

        for i in range(k, n):
            cur += row_costs[i]
            cur -= row_costs[i - k]
            ans = min(ans, cur)

        out.append(str(ans))

    return "\n".join(out)

# provided samples
assert run(
"""5
3 11 1 4
0 1 2 3 4 5 4 3 2 1 0
0 1 2 3 2 1 2 3 3 2 0
0 1 2 3 5 5 5 5 5 2 0
4 4 2 1
0 3 3 0
0 2 1 0
0 1 2 0
0 3 3 0
4 5 2 5
0 1 1 1 0
0 2 2 2 0
0 2 1 1 0
0 3 2 1 0
1 8 1 1
0 10 4 8 4 4 2 0
4 5 3 2
0 8 4 4 0
0 3 4 8 0
0 8 1 10 0
0 10 1 5 0
"""
) == """4
8
4
15
14"""

# minimum size
assert run(
"""1
1 3 1 1
0 5 0
"""
) == "2"

# very large d, direct jump possible
assert run(
"""1
1 5 1 10
0 9 9 9 0
"""
) == "2"

# all equal values
assert run(
"""1
3 5 2 1
0 1 1 1 0
0 1 1 1 0
0 1 1 1 0
"""
) == "8"

# off-by-one distance check
assert run(
"""1
1 3 1 1
0 100 0
"""
) == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Lưới tối thiểu 1 × 3 | 2 | Chỉ hỗ trợ ngân hàng bắt buộc | 
| Lớn`d`| 2 | Nhảy trực tiếp từ cột đầu tiên đến cột cuối cùng | 
| Tất cả các giá trị bằng nhau | 8 | Hành vi DP ổn định trên các hàng | 
|`m = 3`,`d = 1`| 2 | Giải thích chính xác định nghĩa khoảng cách | 

## Vỏ cạnh 

Hãy xem xét trường hợp không cần hỗ trợ ở giữa. 

đầu vào:```
1
1 5 1 10
0 9 9 9 0
```Hỗ trợ đầu tiên ở cột 1 và hỗ trợ cuối cùng ở cột 5. Sự khác biệt của chúng là 4, do đó, các ô bị bỏ qua bằng 3, nằm trong`d = 10`. 

Trong DP, cột 5 có thể chuyển trực tiếp từ cột 1 vì:```
5 - 1 <= d + 1
```Thuật toán trả về chính xác:```
1 + 1 = 2
```Bây giờ hãy xem xét cái bẫy từng cái một. 

đầu vào:```
1
1 3 1 1
0 100 0
```Hỗ trợ ở cột 1 và 3 bỏ qua đúng một ô nên hợp lệ. 

Kích thước cửa sổ DP là`d + 1 = 2`, cho phép quá trình chuyển đổi này. Thuật toán tính toán:```
1 + 1 = 2
```Triển khai có lỗi khi sử dụng kích thước cửa sổ`d`sẽ buộc phải hỗ trợ ở giữa đắt tiền một cách không chính xác. 

Cuối cùng, hãy xem xét trường hợp các lựa chọn tham lam thất bại. 

đầu vào:```
1
1 6 1 1
0 1 100 1 100 0
```Chiến lược tham lam cục bộ có thể chọn mức hỗ trợ giá rẻ ở cột 2, sau đó bị ép vào cột 4 và cuối cùng là cột 6. 

Thay vào đó, DP đánh giá mọi hỗ trợ trước đó có thể tiếp cận và tìm ra chuỗi tối ưu toàn cầu. Deque đảm bảo những chuyển đổi đó được xử lý hiệu quả mà không làm mất tính chính xác.
