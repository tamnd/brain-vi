---
title: "CF 102365D - Hướng dẫn thiên văn"
description: "Chúng ta có các hành tinh được đánh số từ 1 đến (N) và lễ hội diễn ra đúng một trong số đó. Astrodavid bắt đầu ở hành tinh 1. Bước nhảy theo (d0) hành tinh sẽ có chi phí (fd), trong khi bước nhảy theo (d<0) hành tinh sẽ có chi phí (fd), trong đó đầu vào cung cấp các chi phí này cho mỗi lần dịch chuyển lên tới (J)."
date: "2026-08-12T23:51:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102365
codeforces_index: "D"
codeforces_contest_name: "UBC Programming Contest 2019 (UBCPC 2019)"
rating: 0
weight: 102365
solve_time_s: 359
verified: true
draft: false
---

[CF 102365D - Hướng dẫn thiên văn](https://codeforces.com/problemset/problem/102365/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 5 phút 59 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có các hành tinh được đánh số từ 1 đến (N) và lễ hội diễn ra đúng một trong số đó. Astrodavid bắt đầu ở hành tinh 1. Một bước nhảy qua (d>0) hành tinh sẽ có chi phí (f_d), trong khi một bước nhảy qua (d<0) hành tinh sẽ có chi phí (f_d), trong đó đầu vào cung cấp các chi phí này cho mỗi lần dịch chuyển lên tới (J). 

Sau mỗi lần hạ cánh, Astrodavid có thể hỏi đường. Câu trả lời cho anh ta biết liệu hành tinh lễ hội ẩn nhỏ hơn hành tinh hiện tại của anh ta, bằng nó hay lớn hơn nó. Mục tiêu là chọn các bước nhảy một cách thích ứng để cuối cùng có thể đến được mọi địa điểm lễ hội có thể, đồng thời giảm thiểu tổng lượng nhiên liệu tiêu thụ tối đa trên tất cả các địa điểm có thể. 

Cách hữu ích để suy nghĩ về thông tin là một khoảng thời gian. Khi chúng tôi đã truy vấn một số hành tinh (p), một phản hồi cho biết lễ hội cao hơn sẽ giới hạn các hành tinh có thể có ở một hậu tố, trong khi phản hồi thấp hơn sẽ giới hạn chúng ở một tiền tố. Do đó, chiến lược này là một cây quyết định có các nút là các hành tinh được truy vấn, với sự phức tạp bổ sung là việc di chuyển giữa hai truy vấn có chi phí phụ thuộc vào hướng. 

Các ràng buộc chính thức có (2\le N\le4000) và (1\le J\le N/2). Một chương trình động khối sẽ yêu cầu khoảng (4000^3=64) tỷ chuyển đổi cơ bản, vượt xa giới hạn một giây. Giải pháp dự định phải nằm trong khoảng (O(N^2)). Chi phí nhảy tối đa là (10^4), do đó, tổng câu trả lời vừa vặn thoải mái với số nguyên 64 bit, mặc dù số nguyên Python loại bỏ mọi lo ngại về tràn. 

Có hai sai lầm ranh giới dễ dàng. Đầu tiên, hành tinh 1 đã được truy vấn miễn phí. Ví dụ, với```
2 1
7
3
```câu trả lời là 7, không phải 0, vì lễ hội có thể diễn ra ở hành tinh 2 và khả năng nhảy duy nhất là (+1). Thứ hai, khoảng hữu ích đầu tiên chứa các hành tinh từ 2 đến (N), không phải các hành tinh từ 1 đến (N), vì hành tinh 1 đã được kiểm tra. 

Một vấn đề tinh tế hơn là chiến lược tối ưu không nhất thiết phải hướng tới lễ hội một cách đơn điệu. Ví dụ,```
4 2
100 0
0 0
```cho phép trình tự (1\to3\to2\to4), không sử dụng nhiên liệu. Bước nhảy từ 3 lên 2 di chuyển ra khỏi lễ hội được biết là cao hơn 3, nhưng nó đặt con tàu vào một vị trí mà từ đó lần nhảy cuối cùng lên 4 là miễn phí. Bất kỳ giải pháp nào cho rằng mỗi lần nhảy phải di chuyển về phía mục tiêu đều không chính xác. 

## Phương pháp tiếp cận 

Chương trình động ngắt quãng trực tiếp là điểm khởi đầu tự nhiên. Giả sử các hành tinh hiện có có thể tạo thành một khoảng và con tàu ở ngay bên ngoài một đầu của khoảng đó. Nếu chúng ta chọn hành tinh thứ (k)-của khoảng làm truy vấn tiếp theo, một câu trả lời sẽ loại bỏ (k-1) ứng viên và câu trả lời còn lại sẽ loại bỏ các ứng viên còn lại. Nhiên liệu cần thiết sau khi nhảy là mức tối đa trong yêu cầu của 2 nhánh đó. 

Điều này mang lại kết quả lặp lại minimax chính xác nếu biết chi phí chuyển sang truy vấn tiếp theo. Khó khăn là Astrodavid có thể thực hiện một số bước nhảy trong khi đã biết hướng, sử dụng những bước nhảy đó hoàn toàn để định vị lại con tàu. Ví dụ từ phần trước chứng minh tại sao việc bỏ qua việc tái định vị như vậy có thể tạo ra một câu trả lời sai. 

Quan sát quan trọng là một chuỗi các bước nhảy được thực hiện trước lần so sánh hữu ích tiếp theo có thể được nén thành chi phí nhiên liệu tối thiểu có thể. Chi phí chỉ phụ thuộc vào độ dịch chuyển, do đó, bài toán tái định vị này bản thân nó là bài toán đường đi ngắn nhất trên đồ thị bất biến tịnh tiến một chiều. Khi biết được các chi phí di chuyển hiệu quả này, phần tìm kiếm sẽ trở thành DP tối đa khoảng thời gian. 

Đối với độ dịch chuyển ròng (d), hãy đặt (g_d) là nhiên liệu tối thiểu cần thiết để di chuyển qua chính xác (d) hành tinh, cho phép các bước nhảy trung gian tùy ý. Chỉ cần những chuyển vị có liên quan đến một truy vấn hữu ích duy nhất. Chúng tôi tính toán các chi phí hiệu quả này bằng phép tính đường đi ngắn nhất theo các chuyển vị từ (-J) đến (J). Quá trình chuyển đổi từ dịch chuyển (x) sang (x+i) có chi phí (f_i). 

Chi phí hiệu quả thu được cho phép khoảng DP coi toàn bộ chuỗi tái định vị là một chuyển động. Việc lặp lại tìm kiếm sau đó chỉ cần xem xét vị trí thực hiện so sánh tiếp theo. 

Khoảng thời gian brute-force DP sẽ kiểm tra mọi phân chia có thể có cho mỗi khoảng thời gian, cho thời gian (O(N^3)). Công thức được tối ưu hóa có (O(NJ)) chuyển đổi tìm kiếm sau khi chi phí hiệu quả được tính toán và (J\le N/2), vì vậy đây là (O(N^2)). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Khoảng thời gian Brute Force DP | (O(N^3)) | (O(N^2)) | Quá chậm | 
| Chi phí di chuyển hiệu quả + khoảng DP | (O(NJ+J^2)) | (O(N+J)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng đồ thị chuyển động có hướng trên các trạng thái dịch chuyển (-J,-J+1,\ldots,J). Từ trạng thái (x), bước nhảy của (i) chuyển sang (x+i), miễn là chuyển vị thu được vẫn nằm trong phạm vi được xem xét và chi phí (f_i). Chạy tính toán đường đi ngắn nhất từ ​​độ dịch chuyển 0. Khoảng cách thu được đến độ dịch chuyển (d) là cách rẻ nhất để nhận ra độ dịch chuyển ròng (d). 
2. Trích xuất chi phí di chuyển lên và xuống hiệu quả từ những khoảng cách đường đi ngắn nhất đó. Một truy vấn hữu ích đã tiếp cận (d) hành tinh với chi phí bên phải (g_d), trong khi truy vấn (d) các hành tinh tới bên trái có chi phí (g_{-d}). 
3. Xác định (L[m]) là lượng nhiên liệu tối thiểu trong trường hợp xấu nhất cần thiết để xác định một lễ hội giữa (m) hành tinh ứng cử viên liên tiếp khi con tàu ở ngay bên trái của chúng. Xác định (R[m]) đối xứng khi tàu ở ngay bên phải họ. Khoảng trống có giá trị bằng 0. 
4. Để tính toán (L[m]), hãy chọn hành tinh được truy vấn tiếp theo làm ứng cử viên thứ (k) từ bên trái. Chi phí để đạt được nó (g_k). Nếu lễ hội ở bên dưới hành tinh đó, sẽ có (k-1) ứng cử viên còn lại và con tàu ở ngay bên phải họ, đưa ra chi phí (R[k-1]). Nếu lễ hội ở phía trên nó, có (m-k) ứng cử viên và con tàu ở ngay bên trái của họ, đưa ra chi phí (L[m-k]). Nhánh tệ nhất có giá trị tối đa trong hai giá trị này. 
5. Lấy mức tối thiểu trên mọi khả thi (k). Như vậy 

\min_{1\le k\le\min(J,m)} 
\left( 
g_k+\max(R[k-1],L[m-k]) 
\đúng). 
]

1. Tính (R[m]) đối xứng. Nếu hành tinh thứ (k) được tính từ bên phải, thì bước nhảy có giá (g_{-k}). Lễ hội thấp hơn sẽ để lại (m-k) ứng viên với con tàu ở bên phải của họ, trong khi lễ hội cao hơn sẽ để lại các ứng viên (k-1) với con tàu ở bên trái của họ. Do đó 

\min_{1\le k\le\min(J,m)} 
\left( 
g_{-k}+\max(R[m-k],L[k-1]) 
\đúng). 
] 

1. Hành tinh 1 được biết là đã được kiểm tra trước khi sử dụng bất kỳ nhiên liệu nào. Nếu có lễ hội thì chúng ta coi như xong. Ngược lại, các ứng cử viên còn lại là hành tinh từ 2 đến (N), và con tàu ở ngay bên trái của họ. Do đó, nhiên liệu ban đầu cần thiết là (L[N-1]). 

### Tại sao nó hoạt động 

Đối với mỗi khoảng thời gian, DP xem xét mọi hành tinh có thể có để thực hiện so sánh thông tin tiếp theo. Phản ứng định hướng chia các ứng cử viên còn lại thành chính xác hai khoảng nhỏ hơn và đối thủ có thể buộc nhánh nào cần nhiều nhiên liệu hơn, điều này giải thích mức tối đa trong mỗi lần chuyển đổi. Việc tái định vị giữa các so sánh thông tin được nén lại thành chi phí di chuyển ngắn nhất có thể, do đó, không có chiến lược nào bị mất khi thay thế chuỗi đó bằng chi phí dịch chuyển hiệu quả của nó. Vì mọi so sánh tiếp theo có thể xảy ra và mọi nhánh kết quả đều được biểu thị bằng phép truy hồi, mức tối thiểu trên tất cả các lựa chọn chính xác là yêu cầu nhiên liệu tối thiểu trong trường hợp xấu nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10**30

def shortest_displacement_cost(up, down, J):
    size = 2 * J + 1
    offset = J

    dist = [INF] * size
    used = [False] * size
    dist[offset] = 0

    for _ in range(size):
        u = -1
        best = INF

        for i in range(size):
            if not used[i] and dist[i] < best:
                best = dist[i]
                u = i

        if u == -1:
            break

        used[u] = True
        pos = u - offset

        lo = max(-J, pos - J)
        hi = min(J, pos + J)

        for nxt in range(lo, hi + 1):
            if nxt == pos:
                continue

            d = nxt - pos
            if d > 0:
                w = up[d - 1]
            else:
                w = down[-d - 1]

            v = nxt + offset
            nd = best + w
            if nd < dist[v]:
                dist[v] = nd

    return dist

def solve():
    N, J = map(int, input().split())
    up = list(map(int, input().split()))
    down = list(map(int, input().split()))

    dist = shortest_displacement_cost(up, down, J)
    offset = J

    effective_up = [INF] * (J + 1)
    effective_down = [INF] * (J + 1)

    for d in range(1, J + 1):
        effective_up[d] = dist[offset + d]
        effective_down[d] = dist[offset - d]

    left = [0] * N
    right = [0] * N

    for m in range(1, N):
        best_left = INF
        limit = min(J, m)

        for k in range(1, limit + 1):
            cost = effective_up[k]
            branch = max(right[k - 1], left[m - k])
            value = cost + branch
            if value < best_left:
                best_left = value

        left[m] = best_left

        best_right = INF

        for k in range(1, limit + 1):
            cost = effective_down[k]
            branch = max(right[m - k], left[k - 1])
            value = cost + branch
            if value < best_right:
                best_right = value

        right[m] = best_right

    print(left[N - 1])

if __name__ == "__main__":
    solve()
```Phần đầu tiên của mã xây dựng biểu đồ dịch chuyển. Trạng thái dịch chuyển thể hiện khoảng cách giữa con tàu và vị trí mà chuỗi so sánh hữu ích bắt đầu. Mỗi bước nhảy hợp pháp sẽ thay đổi lượng dịch chuyển đó tối đa (J), với chi phí nhiên liệu đầu vào chính xác tương ứng. 

Việc triển khai Dijkstra dày đặc là phù hợp vì chỉ có trạng thái dịch chuyển (2J+1). Với (J\le2000), có tối đa 4001 trạng thái và việc nới lỏng tất cả các độ dài bước nhảy có thể sẽ mang lại công việc (O(J^2)). 

Hai mảng chi phí hiệu quả chứa chi phí rẻ nhất cho mỗi lần dịch chuyển ròng. Chúng là những gì cho phép khoảng DP bỏ qua các chi tiết bên trong của trình tự tái định vị. 

Các mảng`left`Và`right`lưu trữ hai hướng khoảng. Các mục không có mục nào của họ đại diện cho một lễ hội đã được xác định, vì vậy một nhánh không còn ứng cử viên nào sẽ đóng góp nhiên liệu bằng không. 

Vòng lặp kết thúc`m`đang tăng lên vì mọi chuyển đổi chỉ đề cập đến các khoảng thời gian có ít hơn`m`ứng viên. Vòng lặp kết thúc`k`dừng lại ở`J`, vì không thể đạt được truy vấn hữu ích với một bước nhảy lớn hơn sức mạnh bước nhảy. 

Câu trả lời cuối cùng là`left[N - 1]`, còn hơn là`left[N]`, bởi vì hành tinh 1 được truy vấn miễn phí trước khi tiêu hết nhiên liệu. 

Số nguyên Python có độ chính xác tùy ý, do đó giá trị nhiên liệu tích lũy và trọng điểm lớn không thể tràn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

cho```
16 8
5 5 5 5 5 5 5 5
5 5 5 5 5 5 5 5
```mỗi lần nhảy tốn 5 đô la, vì vậy việc định vị lại không bao giờ cải thiện được điều gì. Chi phí hiệu quả của mỗi lần dịch chuyển từ 1 đến 8 vẫn là 5. 

Khoảng thời gian DP liên tục chọn một hành tinh gần trung tâm. Các trạng thái quan trọng ở gần cuối trông như thế này. 

| Ứng viên | Chiến lược trái tốt nhất | Chiến lược đúng đắn nhất | 
| --- | --- | --- | 
| 0 | 0 | 0 | 
| 1 | 5 | 5 | 
| 2 | 10 | 10 | 
| 3 | 10 | 10 | 
| 4 | 15 | 15 | 
| 8 | 20 | 20 | 
| 15 | 20 | 20 | 

Sau khi hành tinh 1 được kiểm tra, có 15 hành tinh có thể tổ chức lễ hội. Bốn lần nhảy là đủ ở nhánh tệ nhất, mang lại```
20
```Dấu vết thể hiện bản chất minimax của DP. Mỗi truy vấn được chọn đều có hai câu trả lời hướng có thể có và chỉ nhánh đắt hơn mới xác định được nhiên liệu cần thiết. 

### Mẫu 2 

cho```
16 2
2 0
33 33
```một bước nhảy lên trên hai hành tinh không tốn kém gì, trong khi một bước nhảy lên trên một hành tinh có giá 2. Những bước nhảy xuống rất tốn kém. 

Do đó, DP thích dịch chuyển 2 bất cứ khi nào có thể. Các trạng thái không đối xứng vì`right[m]`đắt hơn nhiều so với`left[m]`. 

| Ứng viên | Yêu cầu còn lại | Đúng yêu cầu | 
| --- | --- | --- | 
| 0 | 0 | 0 | 
| 1 | 2 | 33 | 
| 2 | 4 | 35 | 
| 3 | 6 | 37 | 
| 4 | 8 | 39 | 
| ... | ... | ... | 
| 15 | 30 | 63 | 

Câu trả lời là```
30
```Ví dụ này minh họa tại sao hai hướng phải được lưu trữ riêng biệt. Việc coi chuyển động lên và xuống là đối xứng sẽ làm mất đi đặc điểm chính của bài kiểm tra này. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(J^2+NJ)) | Các đường dẫn ngắn nhất dày đặc sử dụng (O(J^2)) và khoảng DP kiểm tra tối đa các phần tách (J) cho từng kích thước khoảng (N). | 
| Không gian | (O(N+J)) | Chỉ có hai mảng DP khoảng và mảng đường đi ngắn nhất dịch chuyển được lưu trữ. | 

Vì (J\le N/2) nên tổng thời gian là (O(N^2)). Với (N\le4000), giá trị này nằm trong phạm vi bậc hai dự định và mức sử dụng bộ nhớ là tuyến tính theo (N). 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_data(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    try:
        solve()
        return out.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided samples
assert solve_data(
    """16 8
5 5 5 5 5 5 5 5
5 5 5 5 5 5 5 5
"""
) == "20", "sample 1"

assert solve_data(
    """16 2
2 0
33 33
"""
) == "30", "sample 2"

assert solve_data(
    """10 5
50 60 70 80 90
5 4 3 2 1
"""
) == "185", "sample 3"

# Minimum-size input
assert solve_data(
    """2 1
7
3
"""
) == "7", "minimum size"

# All jump costs equal
assert solve_data(
    """5 2
5 5
5 5
"""
) == "15", "all equal costs"

# Zero-cost jumps
assert solve_data(
    """4 2
0 0
0 0
"""
) == "0", "zero-cost movement"

# Maximum-size instance
assert solve_data(
    "4000 2000\n"
    + " ".join(["0"] * 2000)
    + "\n"
    + " ".join(["0"] * 2000)
    + "\n"
) == "0", "maximum size with zero costs"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 1 / 7 / 3`|`7`| Tối thiểu (N) và hành tinh ban đầu đã được truy vấn miễn phí. | 
|`5 2 / 5 5 / 5 5`|`15`| Chuyển động đối xứng, chi phí bằng nhau và phân chia khoảng thời gian. | 
|`4 2 / 0 0 / 0 0`|`0`| Bước nhảy không tốn chi phí và trạng thái DP có giá trị bằng 0. | 
|`N=4000, J=2000`, tất cả chi phí bằng không |`0`| Kích thước đầu vào tối đa và xử lý trạng thái bậc hai. | 

## Vỏ cạnh 

Trường hợp kích thước tối thiểu chỉ có hai hành tinh. Với```
2 1
7
3
```hành tinh 1 được kiểm tra ngay lập tức. Nếu lễ hội không có ở đó thì nó phải ở hành tinh 2 và khả năng nhảy duy nhất có thể là (+1), tốn 7. Thuật toán bắt đầu bằng`left[0] = right[0] = 0`, tính toán`left[1] = 7`, và bản in`left[1]`. 

Chi phí bằng nhau tạo ra một vấn đề đối xứng. Với```
5 2
5 5
5 5
```chiến lược tốt nhất có thể liên tục chia nhỏ khoảng thời gian còn lại trong khi trả 5 cho mỗi lần nhảy hữu ích. Câu trả lời được tính toán là 15. Vì cả hai hướng di chuyển đều có chi phí giống nhau nên mảng DP bên trái và bên phải phát triển giống hệt nhau. 

Chuyển động không tốn phí là một điều kiện biên hữu ích khác. Với```
4 2
0 0
0 0
```mọi bước nhảy hợp pháp đều miễn phí. DP truyền 0 qua mọi kích thước khoảng, vì vậy`left[3]`bằng 0 và câu trả lời cuối cùng là 0. 

Trường hợp bất đối xứng là lý do việc triển khai tiếp tục`left`Và`right`riêng. Một bước nhảy xuống có thể rẻ hơn nhiều so với một bước nhảy lên, vì vậy sau một truy vấn, hai nhánh hướng có thể có chi phí rất khác nhau. Phép truy hồi luôn chọn yêu cầu tối đa trong hai nhánh vì lễ hội ẩn có thể nằm ở một trong hai nhánh. 

Cuối cùng, giới hạn nhảy phải được thực thi độc lập với kích thước khoảng. Khi một khoảng chứa ít hơn (J) ứng viên, chỉ những ứng viên đó mới có thể được truy cập dưới dạng truy vấn thông tin tiếp theo. Khi nó chứa nhiều hơn (J) ứng cử viên, tối đa (J) vị trí có thể được xem xét từ ranh giới hiện tại trong một lần nhảy. các`min(J, m)`limit xử lý cả hai trường hợp mà không gặp lỗi nào.
