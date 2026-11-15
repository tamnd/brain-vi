---
title: "CF 1866G - Xe ngựa theo nhóm"
description: "Mỗi toa ban đầu chứa một số lượng hành khách. Hành khách bắt đầu toa xe i có thể di chuyển sang trái hoặc sang phải nhưng không được vượt quá cửa Di."
date: "2026-06-08T23:48:57+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "dp", "flows", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1866
codeforces_index: "G"
codeforces_contest_name: "COMPFEST 15 - Preliminary Online Mirror (Unrated, ICPC Rules, Teams Preferred)"
rating: 2100
weight: 1866
solve_time_s: 190
verified: true
draft: false
---

[CF 1866G - Xe ngựa được nhóm](https://codeforces.com/problemset/problem/1866/G) 

**Xếp hạng:** 2100 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, dp, luồng, tham lam 
**Thời gian giải:** 3 phút 10s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi toa ban đầu chứa một số lượng hành khách. Một hành khách bắt đầu lên xe`i`có thể di chuyển sang trái hoặc phải, nhưng không thể vượt quá`D_i`cửa. Điều đó có nghĩa là mọi hành khách từ xe ngựa`i`phải kết thúc ở đâu đó bên trong khoảng thời gian$$[i-D_i,\; i+D_i]$$sau khi cắt các điểm cuối thành phạm vi hợp lệ`1..N`. 

Chúng tôi có thể tự do phân phối lại hành khách miễn là mọi hành khách ở trong khoảng thời gian cho phép khi chuyến xe bắt đầu của họ. 

Mục tiêu không phải là giảm thiểu tổng số chuyển động. Thay vào đó, chúng tôi muốn đoàn tàu được cân bằng nhất có thể. Sau khi thực hiện xong tất cả các bước di chuyển, hãy`Z`là số lượng hành khách lớn nhất trên bất kỳ toa xe nào. Chúng ta phải tìm giá trị nhỏ nhất có thể đạt được của`Z`. 

Những ràng buộc là điều làm cho vấn đề trở nên thú vị. Có tới`2 · 10^5`toa xe và số lượng hành khách có thể lớn bằng`10^9`. Bất kỳ thuật toán nào di chuyển hành khách một cách rõ ràng, xây dựng biểu đồ luồng với một nút cho mỗi hành khách hoặc thử tất cả các phân phối cuối cùng có thể đều ngay lập tức không thể thực hiện được. Thậm chí một`O(N^2)`thuật toán sẽ yêu cầu khoảng`4 · 10^10`hoạt động trong trường hợp xấu nhất. 

Cấu trúc của hạn chế di chuyển là quan sát chính. Mỗi nhóm hành khách từ toa xe`i`chỉ có thể được chỉ định trong một khoảng liền kề. Cấu trúc khoảng đó đơn giản hơn nhiều so với một biểu đồ tùy ý. 

Một vài trường hợp cạnh rất dễ bị bỏ sót. 

Coi như:```
1
10
0
```Chỉ có một chiếc xe ngựa và không ai có thể di chuyển được. Câu trả lời là`10`. Bất kỳ giải pháp nào giả định việc phân phối lại luôn có thể thực hiện được sẽ thất bại ở đây. 

Coi như:```
2
5 0
0 0
```Không ai có thể rời khỏi cỗ xe xuất phát của mình. Câu trả lời là`5`, không`3`. Tổng giá trị trung bình không thành vấn đề khi những hạn chế về chuyển động cản trở sự cân bằng. 

Coi như:```
3
3 3 3
2 2 2
```Mọi hành khách đều có thể đến được mọi toa xe. có`9`hành khách và`3`xe ngựa, vậy câu trả lời là`3`. Một giải pháp chỉ xem xét các ràng buộc cục bộ có thể trả về sai thứ gì đó lớn hơn. 

Khó khăn chính là xác định liệu giá trị đề xuất của`Z`là khả thi. 

## Phương pháp tiếp cận 

Một quan điểm mạnh mẽ là đoán số lượng hành khách cuối cùng trên mỗi toa xe và kiểm tra xem liệu tất cả hành khách có thể được chuyển đến đó hay không. Ngay cả khi chúng tôi bằng cách nào đó đã hạn chế tối đa mỗi toa xe`Z`hành khách, có rất nhiều cách phân phối có thể xảy ra theo cấp số nhân. Điều này thật vô vọng. 

Một ý tưởng phức tạp hơn là mô hình hóa vấn đề như một mạng lưới luồng. Mỗi cỗ xe xuất phát góp phần`A_i`đơn vị dòng chảy Mỗi toa xe đích có sức chứa`Z`. Chúng tôi kết nối khoảng nguồn`i`tới mọi toa xe đích đến có thể tiếp cận được. 

Điều này đúng, nhưng biểu đồ chứa tới$$\sum (2D_i+1)$$các cạnh, có thể`Θ(N^2)`trong trường hợp xấu nhất. Với`N = 2 · 10^5`, nó quá lớn. 

Bước ngoặt đến từ việc nhận thấy rằng mỗi nhóm hành khách tương ứng với một quãng đường duy nhất trên một tuyến. Các bài toán gán khoảng thời gian thường thừa nhận các giải pháp đường quét tham lam. 

Giả sử chúng ta sửa một số giá trị ứng cử viên`Z`. Mỗi toa bây giờ có sức chứa`Z`, nghĩa là nó có thể chấp nhận nhiều nhất`Z`hành khách trong sự sắp xếp cuối cùng. 

Để vận chuyển`i`, chúng tôi có nhu cầu`A_i`hành khách phải được đặt ở đâu đó trong khoảng thời gian`[L_i, R_i]`. 

Điều này trở thành: 

"Liệu tất cả các nhu cầu theo khoảng thời gian có thể được phân công vào các vị trí`1..N`, nơi mọi vị trí đều có năng lực`Z`?" 

Câu hỏi khả thi là đơn điệu. Nếu năng lực`Z`hoạt động thì bất kỳ công suất lớn hơn nào cũng hoạt động. Điều đó ngay lập tức gợi ý tìm kiếm nhị phân cho câu trả lời. 

Nhiệm vụ còn lại là kiểm tra tính khả thi hiệu quả. 

Quét từ trái sang phải qua các toa xe đích. Khi chúng tôi đạt đến vị trí`x`, mọi khoảng có điểm cuối bên trái là`x`trở nên hoạt động. Năng lực của vị trí này là`Z`. 

Để sử dụng dung lượng đó một cách tối ưu, trước tiên chúng ta phải luôn phân phát khoảng thời gian hoạt động với điểm cuối bên phải nhỏ nhất. Khoảng thời gian hết hạn sớm hơn thì cấp bách hơn. Đây là đối số trao đổi tương tự xuất hiện trong lịch trình sớm nhất-thời hạn-đầu tiên. 

Một đống tối thiểu được sắp xếp theo đúng điểm cuối sẽ cung cấp chính xác những gì chúng ta cần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Luồng/phân công rõ ràng | Ít nhất O(N²) | O(N2) | Quá chậm | 
| Tìm kiếm nhị phân + quét khoảng thời gian tham lam | O(N log N log S) | O(N) | Đã chấp nhận | 

Đây`S = sum(A_i)`. 

## Hướng dẫn thuật toán 

### Kiểm tra tính khả thi của bản sửa lỗi`Z`Đối với mỗi toa xe`i`, định nghĩa:$$L_i = \max(1, i-D_i)$$

$$R_i = \min(N, i+D_i)$$Xe`i`đóng góp một nhu cầu`A_i`hành khách phải được chỉ định ở đâu đó bên trong`[L_i, R_i]`. 

Chúng tôi quét các vị trí đích từ trái sang phải. 

1. Tạo danh sách các khoảng thời gian bắt đầu từ mỗi vị trí. 
2. Duy trì một lượng tối thiểu các khoảng thời gian hoạt động được sắp xếp theo điểm cuối bên phải của chúng. 
3. Khi đến vị trí`x`, chèn tất cả các khoảng với`L_i = x`vào đống. 
4. Trước khi sử dụng dung lượng, hãy kiểm tra xem một số khoảng thời gian hoạt động đã có chưa`R_i < x`. Nếu khoảng thời gian đó vẫn tồn tại thì thời hạn của nó đã trôi qua và nhiệm vụ là không thể thực hiện được. 
5. Vị trí`x`có năng lực sẵn có`Z`. 
6. Lặp lại khoảng thời gian hoạt động với điểm cuối bên phải nhỏ nhất. 
7. Chỉ định càng nhiều dung lượng càng tốt cho khoảng thời gian đó. 
8. Nếu khoảng đó hoàn toàn được thỏa mãn, hãy xóa nó vĩnh viễn. 
9. Nếu chặng vẫn cần hành khách sau khi đã sử dụng hết công suất còn lại của vị trí`x`, đặt nó trở lại heap và tiếp tục. 
10. Sau vị trí`x`được xử lý, nếu có bất kỳ khoảng thời gian hoạt động nào`R_i = x`, nó phải được thỏa mãn hoàn toàn rồi. Nếu không tính khả thi sẽ thất bại. 
11. Sau khi xử lý tất cả các vị trí, tính khả thi chỉ thành công nếu không còn khoảng thời gian chưa hoàn thành. 

### Tìm kiếm nhị phân 

1. Đặt`low = 0`. 
2. Đặt`high = sum(A)`. 
3. Tìm kiếm nhị phân nhỏ nhất`Z`mà thử nghiệm tính khả thi thành công. 
4. Xuất giá trị đó. 

### Tại sao nó hoạt động 

Tại bất kỳ vị trí đích nào, giả sử chúng ta phân bổ dung lượng cho một khoảng có điểm cuối bên phải muộn hơn trong khi một khoảng hoạt động khác hết hạn sớm hơn. 

Nếu khoảng thời gian trước đó không thể đáp ứng được thì chúng ta có thể hoán đổi một phần phân bổ và thay vào đó chuyển dung lượng đó cho khoảng thời gian trước đó. Khoảng thời gian muộn hơn vẫn còn ít nhất khoảng trống cho các vị thế trong tương lai vì thời hạn của nó không sớm hơn. 

Việc áp dụng nhiều lần đối số trao đổi này cho thấy rằng việc cung cấp các khoảng thời gian hoạt động theo thứ tự tăng dần của điểm cuối bên phải ít nhất luôn tốt bằng bất kỳ lựa chọn nào khác. 

Việc quét đảm bảo rằng mọi khoảng thời gian chỉ nhận được dung lượng trong phạm vi cho phép của nó. Bất cứ khi nào một khoảng đạt đến điểm cuối bên phải của nó, thuật toán sẽ kiểm tra xem toàn bộ nhu cầu của nó đã được ấn định chưa. Do đó, mọi phép gán được chấp nhận đều thỏa mãn mọi ràng buộc. 

Bởi vì tính khả thi là đơn điệu trong`Z`, tìm kiếm nhị phân tìm thấy giá trị khả thi tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from heapq import heappush, heappop

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    d = list(map(int, input().split()))

    starts = [[] for _ in range(n + 2)]

    for i in range(n):
        l = max(1, i + 1 - d[i])
        r = min(n, i + 1 + d[i])
        starts[l].append((r, a[i]))

    total = sum(a)

    def feasible(z):
        heap = []

        for pos in range(1, n + 1):
            for r, amt in starts[pos]:
                heappush(heap, (r, amt))

            while heap and heap[0][0] < pos:
                return False

            cap = z

            while cap > 0 and heap:
                r, need = heappop(heap)

                take = min(cap, need)
                cap -= take
                need -= take

                if need:
                    heappush(heap, (r, need))
                    break

            while heap and heap[0][0] == pos:
                return False

        return not heap

    lo, hi = 0, total

    while lo < hi:
        mid = (lo + hi) // 2
        if feasible(mid):
            hi = mid
        else:
            lo = mid + 1

    print(lo)

solve()
```Quá trình xử lý trước chuyển đổi mỗi đầu dòng bắt đầu thành một khoảng`[L_i, R_i]`cùng với một yêu cầu`A_i`. 

các`starts`array cho phép chúng ta chèn các khoảng vào heap chính xác khi chúng hoạt động. Điều này tránh việc quét liên tục tất cả các khoảng thời gian trong quá trình quét. 

Heap lưu trữ các cặp`(right_endpoint, remaining_demand)`. Điểm cuối bên phải nhỏ nhất luôn được xử lý đầu tiên. 

Một điểm tinh tế là`break`sau khi lắp lại một khoảng thời gian được thỏa mãn một phần. Khi vị trí hiện tại hết dung lượng, không còn khoảng thời gian nào nữa có thể nhận được bất kỳ thứ gì từ vị trí này. Lắp lại và tiếp tục sẽ chỉ lãng phí thời gian. 

Một điểm tinh tế khác là kiểm tra thời hạn. Trước khi sử dụng vị trí`pos`, bất kỳ khoảng nào với`r < pos`đã hết hạn và phải gây ra lỗi. Sau khi sử dụng vị trí`pos`, mọi khoảng còn lại với`r == pos`cũng gây ra thất bại vì đây là cơ hội cuối cùng để nhận được năng lực. 

Số lượng hành khách có thể đạt tới`10^9`, vì vậy mọi phép tính phải được thực hiện với các số nguyên có khả năng chứa các giá trị lớn. Python xử lý việc này một cách tự nhiên. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
7
7 4 2 0 5 8 3
4 0 0 1 3 1 3
```Các khoảng là: 

| Vận chuyển | A | Khoảng thời gian | 
| --- | --- | --- | 
| 1 | 7 | [1,5] | 
| 2 | 4 | [2,2] | 
| 3 | 2 | [3,3] | 
| 4 | 0 | [3,5] | 
| 5 | 5 | [2,7] | 
| 6 | 8 | [5,7] | 
| 7 | 3 | [4,7] | 

Thử`Z = 5`. 

| Vị trí | Khoảng thời gian mới | Đống trước khi phục vụ | Công suất sử dụng | 
| --- | --- | --- | --- | 
| 1 | [1,5] | (5,7) | 5 được giao | 
| 2 | [2,2], [2,7] | thời hạn sớm nhất là 2 | thỏa mãn vận chuyển 2 | 
| 3 | [3,3], [3,5] | thời hạn sớm nhất là 3 | thỏa mãn vận chuyển 3 | 
| 4 | [4,7] | khoảng thời gian hoạt động vẫn còn | công suất sử dụng | 
| 5 | [5,7] | khoảng thời gian hoạt động vẫn còn | công suất sử dụng | 
| 6 | không | khoảng thời gian hoạt động vẫn còn | công suất sử dụng | 
| 7 | không | khoảng thời gian hoạt động vẫn còn | kết thúc | 

Mọi nhu cầu đều có thể được đáp ứng, vì vậy`Z = 5`là khả thi. 

Đang cố gắng`Z = 4`thất bại vì các khoảng kết thúc ở vị trí`2`Và`3`tiêu thụ quá nhiều dung lượng ban đầu, để lại không đủ chỗ sau này. 

Do đó câu trả lời là`5`. 

Dấu vết này thể hiện quy tắc thời hạn đầu tiên. Các khoảng có điểm cuối bên phải nhỏ được bảo vệ trước các khoảng linh hoạt hơn. 

### Ví dụ được xây dựng 

đầu vào:```
3
3 3 3
2 2 2
```Mọi hành khách đều có thể đến được mọi toa xe. 

Thử`Z = 2`. 

| Vị trí | Nhu cầu hoạt động trước khi phục vụ | Công suất | 
| --- | --- | --- | 
| 1 | 9 | 2 | 
| 2 | 7 | 2 | 
| 3 | 5 | 2 | 

Chỉ một`6`tổng công suất tồn tại, nhưng`9`hành khách phải được đặt. Việc kiểm tra không thành công. 

Thử`Z = 3`. 

| Vị trí | Nhu cầu hoạt động trước khi phục vụ | Công suất | 
| --- | --- | --- | 
| 1 | 9 | 3 | 
| 2 | 6 | 3 | 
| 3 | 3 | 3 | 

Mọi nhu cầu đều được đáp ứng chính xác. 

Câu trả lời là`3`. 

Ví dụ này cho thấy rằng khi mỗi khoảng thời gian trải dài trên toàn bộ dây chuyền, vấn đề sẽ giảm xuống còn việc cân bằng tổng công suất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log N log S) | Tìm kiếm nhị phân thực hiện kiểm tra O(log S), mỗi lần quét là O(N log N) | 
| Không gian | O(N) | Danh sách bắt đầu và lưu trữ heap khoảng O(N) | 

Đây$$S = \sum A_i$$Và`log S`nhiều nhất là khoảng 48 vì`S ≤ 2 · 10^14`. 

Với`N = 2 · 10^5`, độ phức tạp dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io
from heapq import heappush, heappop

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))
    d = list(map(int, input().split()))

    starts = [[] for _ in range(n + 2)]

    for i in range(n):
        l = max(1, i + 1 - d[i])
        r = min(n, i + 1 + d[i])
        starts[l].append((r, a[i]))

    total = sum(a)

    def feasible(z):
        heap = []

        for pos in range(1, n + 1):
            for r, amt in starts[pos]:
                heappush(heap, (r, amt))

            while heap and heap[0][0] < pos:
                return False

            cap = z

            while cap > 0 and heap:
                r, need = heappop(heap)

                take = min(cap, need)
                cap -= take
                need -= take

                if need:
                    heappush(heap, (r, need))
                    break

            while heap and heap[0][0] == pos:
                return False

        return not heap

    lo, hi = 0, total

    while lo < hi:
        mid = (lo + hi) // 2
        if feasible(mid):
            hi = mid
        else:
            lo = mid + 1

    return str(lo) + "\n"

# provided sample
assert run(
"""7
7 4 2 0 5 8 3
4 0 0 1 3 1 3
"""
) == "5\n"

# minimum size
assert run(
"""1
10
0
"""
) == "10\n"

# nobody can move
assert run(
"""2
5 0
0 0
"""
) == "5\n"

# complete freedom
assert run(
"""3
3 3 3
2 2 2
"""
) == "3\n"

# off-by-one interval boundaries
assert run(
"""3
4 0 0
1 0 1
"""
) == "2\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`N=1`,`A=[10]`|`10`| Ví dụ kích thước tối thiểu | 
|`A=[5,0]`,`D=[0,0]`|`5`| Không được phép di chuyển | 
|`A=[3,3,3]`,`D=[2,2,2]`|`3`| Phân phối lại hoàn toàn linh hoạt | 
|`A=[4,0,0]`,`D=[1,0,1]`|`2`| Xử lý ranh giới và điểm cuối khoảng cách | 

## Vỏ cạnh 

Hãy xem xét:```
1
10
0
```Khoảng thời gian là`[1,1]`. Trong quá trình kiểm tra tính khả thi, tất cả mười hành khách phải ở trên toa duy nhất. Bất kì`Z < 10`để lại nhu cầu không được đáp ứng sau khi có được vị trí`1`, do đó việc kiểm tra không thành công.`Z = 10`thành công. 

Coi như:```
2
5 0
0 0
```Các khoảng là`[1,1]`Và`[2,2]`. Khi kiểm tra`Z = 4`, chức vụ`1`có năng lực`4`nhưng nhu cầu`5`đó hết hạn ngay lập tức. Sau khi xử lý vị trí`1`, một khoảng chưa hoàn thành với điểm cuối bên phải`1`vẫn còn, do đó thuật toán sẽ loại bỏ nó một cách chính xác.`Z = 5`là giá trị khả thi đầu tiên. 

Coi như:```
3
3 3 3
2 2 2
```Tất cả các khoảng đều`[1,3]`. Vùng heap luôn chứa các khoảng thời gian tồn tại lâu dài, do đó không xảy ra lỗi thời hạn. Tính khả thi chỉ phụ thuộc vào tổng công suất. Việc quét tiêu thụ chính xác`3Z`năng lực, ít nhất phải có`9`. Giá trị khả thi nhỏ nhất là`3`. 

Những trường hợp này minh họa hai thái cực của vấn đề. Một số trường hợp bị chi phối hoàn toàn bởi thời hạn cục bộ, trong khi những trường hợp khác hoạt động giống như cân bằng tải thuần túy. Thử nghiệm tính khả thi của đường quét xử lý cả hai với cùng một bất biến: mỗi khoảng phải nhận được tất cả nhu cầu của nó trước khi điểm cuối bên phải của nó được vượt qua.
