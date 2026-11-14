---
title: "CF 1766E - Phân hủy"
description: "Đối với bất kỳ phân đoạn mảng nào, chúng tôi xử lý các phần tử của nó từ trái sang phải và duy trì danh sách các chuỗi con. Khi một giá trị mới xuất hiện, chúng tôi tìm kiếm chuỗi con đầu tiên có phần tử cuối cùng hiện tại có bit dương AND với giá trị mới."
date: "2026-06-09T13:00:26+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "brute-force", "data-structures", "divide-and-conquer", "dp", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1766
codeforces_index: "E"
codeforces_contest_name: "Educational Codeforces Round 139 (Rated for Div. 2)"
rating: 2300
weight: 1766
solve_time_s: 163
verified: false
draft: false
---

[CF 1766E - Phân hủy](https://codeforces.com/problemset/problem/1766/E) 

**Đánh giá:** 2300 
**Tags:** tìm kiếm nhị phân, lực lượng vũ phu, cấu trúc dữ liệu, phân chia và chinh phục, dp, hai con trỏ 
**Thời gian giải:** 2m 43s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Đối với bất kỳ phân đoạn mảng nào, chúng tôi xử lý các phần tử của nó từ trái sang phải và duy trì danh sách các chuỗi con. 

Khi một giá trị mới xuất hiện, chúng tôi tìm kiếm chuỗi con đầu tiên có phần tử cuối cùng hiện tại có bit dương AND với giá trị mới. Nếu một chuỗi con như vậy tồn tại, chúng tôi sẽ thêm giá trị vào đó và cập nhật phần tử cuối cùng của chuỗi con đó. Ngược lại, chúng ta tạo ra một dãy con hoàn toàn mới. 

Cho phép$f(segment)$là số dãy con cuối cùng được tạo bởi thủ tục này. 

Nhiệm vụ là tính tổng$f$trên mỗi mảng con của mảng đã cho. 

Chiều dài mảng đạt$3 \cdot 10^5$, vậy có khoảng$4.5 \cdot 10^{10}$mảng con. Bất kỳ thuật toán nào đánh giá từng mảng con riêng lẻ đều vô vọng. Thậm chí một$O(n^2)$giải pháp hoàn toàn nằm ngoài phạm vi. Chúng ta cần một cái gì đó gần với thời gian tuyến tính. 

Ràng buộc bất thường là mọi giá trị mảng đều thuộc về$\{0,1,2,3\}$. Toàn bộ giải pháp xuất phát từ việc khai thác mức độ nhỏ của tập giá trị này. 

Trường hợp tinh tế đầu tiên là giá trị$0$. Số 0 không bao giờ có thể được thêm vào một dãy con hiện có bởi vì$x \& 0 = 0$cho mọi$x$. Mọi số 0 luôn tạo ra một dãy con mới. 

Ví dụ:```
[0, 0]
```Sự phân rã tạo ra hai chuỗi con, vì vậy$f=2$. 

Một trường hợp tinh vi khác là thuật toán luôn chọn dãy con tương thích đầu tiên chứ không phải dãy con tùy ý. 

Ví dụ:```
[1, 2, 3]
```Sau khi xử lý$1,2$, các dãy con tích cực là:```
[1]
[2]
```Khi$3$đến, nó được thêm vào dãy con đầu tiên vì$1 \& 3 > 0$. Chọn câu thứ hai sẽ tạo ra một trạng thái khác và một câu trả lời sai. 

Bẫy thứ ba là chỉ có phần tử cuối cùng hiện tại của mỗi chuỗi con mới quan trọng. Các yếu tố trước đó không bao giờ ảnh hưởng đến các quyết định trong tương lai. 

Ví dụ:```
[1,3,2]
```Dãy con đầu tiên trở thành$[1,3,2]$. Các quyết định trong tương lai chỉ phụ thuộc vào giá trị cuối cùng$2$, không phải trước đó$1,3$. 

## Phương pháp tiếp cận 

Ý tưởng về vũ lực rất đơn giản. 

Chọn vị trí bắt đầu$l$. Mở rộng điểm cuối bên phải$r$từng bước một. Duy trì sự phân hủy của$a[l..r]$và theo dõi có bao nhiêu chuỗi con hiện đang tồn tại. 

Điều này đúng vì nó mô phỏng trực tiếp định nghĩa. 

Vấn đề là sự phức tạp. có$O(n^2)$mảng con. Ngay cả khi mỗi phần mở rộng được$O(1)$, tổng công vẫn sẽ là phương trình bậc hai, quá lớn đối với$n=3\cdot10^5$. 

Quan sát quan trọng là các giá trị được giới hạn ở$\{0,1,2,3\}$. 

Hãy xem xét trạng thái phân hủy tại một thời điểm nào đó. Các quyết định trong tương lai chỉ phụ thuộc vào yếu tố cuối cùng của mỗi chuỗi con hiện tại "có thể sử dụng được". 

Các phần tiếp theo kết thúc bằng$0$là đặc biệt. Họ không bao giờ có thể chấp nhận bất kỳ giá trị tương lai nào, bởi vì$0 \& x = 0$cho tất cả$x$. Khi một chuỗi con kết thúc bằng$0$, nó trở nên không liên quan đến những chuyển đổi trong tương lai. 

Vì vậy chúng ta chỉ cần nhớ các giá trị cuối cùng của các dãy con khác 0. 

Những giá trị đó thuộc về$\{1,2,3\}$. Nếu chúng ta áp dụng nhiều lần quy tắc dãy tương thích đầu tiên thì chỉ một số lượng nhỏ các trạng thái có thể xuất hiện. Trên thực tế, chỉ có 16 trạng thái có thể truy cập được. Đây là sự nén quan trọng. Quá trình ban đầu có vẻ phức tạp nhưng toàn bộ hành vi trong tương lai của nó được xác định bởi một trong 16 trạng thái có thể có. Ý tưởng này chính xác là cơ sở của giải pháp chính thức. 

Khi không gian trạng thái không đổi, chúng ta có thể thực hiện lập trình động trên các vị trí và trạng thái. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$hoặc tệ hơn |$O(n)$| Quá chậm | 
| Tối ưu |$O(n \cdot S)$,$S=16$|$O(S)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Giả sử một trạng thái là danh sách có thứ tự các giá trị cuối cùng của tất cả các dãy con khác 0. 

Ví dụ:```
()
(1)
(2,1)
(3,2,1)
```Chỉ có 16 tiểu bang có thể truy cập được. 

Xác định hàm chuyển tiếp:```
go(state, x)
```xử lý một giá trị mới. 

Nếu như$x=0$, một dãy con mới luôn được tạo ra, nhưng trạng thái không thay đổi vì các dãy con có kết thúc bằng 0 không bao giờ có ích sau này. 

Nếu như$x>0$, quét trạng thái từ trái sang phải. 

Nếu giá trị đầu tiên có AND dương với$x$được tìm thấy, thay thế giá trị đó bằng$x$. 

Nếu không thì nối thêm$x$đến cuối trạng thái. 

Quá trình chuyển đổi cũng trả về:```
add = 1
```nếu một chuỗi con mới được tạo ra, nếu không thì```
add = 0
```Bây giờ xác định:```
dp[i][state]
```như tổng số tiền đóng góp được tạo ra từ các vị trí$i,i+1,\dots,n-1$, giả sử trạng thái phân rã hiện tại bằng`state`. 

Giả sử vị trí$i$tạo ra`add`các dãy tiếp theo mới. 

Mỗi dãy con mới tạo đều tăng$f$bằng 1 cho mỗi mảng con kết thúc tại$i,i+1,\dots,n-1$. 

Có chính xác:```
n - i
```những kết thúc như vậy. 

Vì vậy sự đóng góp của vị trí$i$bằng:```
add * (n - i)
```Sau khi xử lý$a_i$, chúng ta chuyển sang trạng thái tiếp theo. 

Điều này mang lại sự tái phát:```
dp[i][state]
=
add * (n - i)
+
dp[i+1][next_state]
```Câu trả lời là:```
sum(dp[i][empty_state])
for all starting positions i
```Đây là sự lặp lại tương tự được sử dụng trong giải pháp chính thức. 

### Tại sao nó hoạt động 

Đối với vị trí bắt đầu cố định$l$, mỗi lần phân tách tạo ra một dãy con mới tại vị trí$r$, giá trị$f(a[l..t])$tăng thêm 1 cho mỗi$t \ge r$. 

Có chính xác$n-r$những kết thúc như vậy. 

Sự lặp lại tính mức tăng này tại thời điểm nó được tạo và tính phí ngay lập tức cho tất cả các kết thúc trong tương lai. Vì mỗi lần tăng$f$được tính chính xác một lần và mọi đóng góp vẫn tồn tại với số lượng kết thúc chính xác, tổng kết quả bằng tổng trên tất cả các mảng con. 

Trạng thái chứa chính xác thông tin cần thiết cho các chuyển đổi trong tương lai: giá trị cuối cùng hiện tại của tất cả các chuỗi con khác 0. Không có thông tin trước đó có thể ảnh hưởng đến sự lựa chọn trong tương lai. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n = int(input())
a = list(map(int, input().split()))

states = [()]
state_id = {(): 0}

def transition(state, x):
    arr = list(state)

    if x == 0:
        return 1, state

    for i, v in enumerate(arr):
        if v & x:
            arr[i] = x
            return 0, tuple(arr)

    arr.append(x)
    return 1, tuple(arr)

# Enumerate all reachable states.
ptr = 0
while ptr < len(states):
    s = states[ptr]
    ptr += 1

    for x in range(4):
        _, ns = transition(s, x)
        if ns not in state_id:
            state_id[ns] = len(states)
            states.append(ns)

m = len(states)

nxt_state = [[0] * 4 for _ in range(m)]
add_cost = [[0] * 4 for _ in range(m)]

for sid, s in enumerate(states):
    for x in range(4):
        add, ns = transition(s, x)
        nxt_state[sid][x] = state_id[ns]
        add_cost[sid][x] = add

dp_next = [0] * m
answer = 0
empty_id = state_id[()]

for pos in range(n - 1, -1, -1):
    dp_cur = [0] * m
    x = a[pos]

    for sid in range(m):
        ns = nxt_state[sid][x]
        add = add_cost[sid][x]

        dp_cur[sid] = add * (n - pos) + dp_next[ns]

    answer += dp_cur[empty_id]
    dp_next = dp_cur

print(answer)
```Phần đầu tiên xây dựng biểu đồ trạng thái có thể truy cập hoàn chỉnh. Bắt đầu từ trạng thái trống, chúng tôi liên tục áp dụng tất cả bốn giá trị có thể. Chỉ có 16 tiểu bang xuất hiện. 

Bảng chuyển tiếp lưu trữ hai phần thông tin. Đầu tiên là trạng thái tiếp theo. Thứ hai là liệu một chuỗi con mới có được tạo ra hay không. 

Lập trình động chạy từ phải sang trái.`dp_next`đại diện cho vị trí`i+1`, Và`dp_cur`đại diện cho vị trí`i`. 

biểu hiện```
add * (n - pos)
```là bước đếm trung tâm. Một dãy con mới được tạo ra tại`pos`góp phần vào mọi mảng con kết thúc tại hoặc sau`pos`. 

Tất cả số học vừa vặn thoải mái bên trong số nguyên 64 bit. Trong Python, dù sao thì số nguyên cũng không bị giới hạn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 2 3
```Các trạng thái được viết dưới dạng danh sách các đuôi có dãy con khác 0. 

| Vị trí | Giá trị | Trạng thái trước | Chuỗi mới? | Trạng thái sau | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | () | Có | (1) | 
| 1 | 2 | (1) | Có | (1,2) | 
| 2 | 3 | (1,2) | Không | (3,2) | 

Mảng con: 

| mảng con | f | 
| --- | --- | 
| [1] | 1 | 
| [1,2] | 2 | 
| [1,2,3] | 2 | 
| [2] | 1 | 
| [2,3] | 1 | 
| [3] | 1 | 

Tổng cộng:```
1 + 2 + 2 + 1 + 1 + 1 = 8
```Ví dụ này cho thấy một giá trị có thể sử dụng lại dãy con hiện có ngay cả khi có nhiều dãy con. Cái tương thích đầu tiên phải được chọn. 

### Ví dụ 2 

đầu vào:```
3
0 0 0
```| Vị trí | Giá trị | Trạng thái trước | Chuỗi mới? | Trạng thái sau | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | () | Có | () | 
| 1 | 0 | () | Có | () | 
| 2 | 0 | () | Có | () | 

Mảng con: 

| mảng con | f | 
| --- | --- | 
| [0] | 1 | 
| [0,0] | 2 | 
| [0,0,0] | 3 | 
| [0] | 1 | 
| [0,0] | 2 | 
| [0] | 1 | 

Tổng cộng:```
10
```Điều này chứng tỏ tại sao các chuỗi con có kết thúc bằng 0 không cần phải được lưu trữ trong trạng thái. Chúng không bao giờ ảnh hưởng đến quá trình chuyển đổi trong tương lai. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \cdot S)$|$S=16$trạng thái có thể truy cập | 
| Không gian |$O(S)$| Hai lớp DP cộng với các bảng chuyển tiếp | 

Từ$S$là một hằng số rất nhỏ, thời gian chạy thực tế là tuyến tính theo$n$. Vì$n=3 \cdot 10^5$, giải pháp dễ dàng phù hợp với giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n = int(input())
    a = list(map(int, input().split()))

    states = [()]
    state_id = {(): 0}

    def transition(state, x):
        arr = list(state)

        if x == 0:
            return 1, state

        for i, v in enumerate(arr):
            if v & x:
                arr[i] = x
                return 0, tuple(arr)

        arr.append(x)
        return 1, tuple(arr)

    ptr = 0
    while ptr < len(states):
        s = states[ptr]
        ptr += 1

        for x in range(4):
            _, ns = transition(s, x)
            if ns not in state_id:
                state_id[ns] = len(states)
                states.append(ns)

    m = len(states)

    nxt = [[0] * 4 for _ in range(m)]
    add = [[0] * 4 for _ in range(m)]

    for sid, s in enumerate(states):
        for x in range(4):
            c, ns = transition(s, x)
            nxt[sid][x] = state_id[ns]
            add[sid][x] = c

    dp_next = [0] * m
    ans = 0
    empty = state_id[()]

    for pos in range(n - 1, -1, -1):
        dp_cur = [0] * m

        for sid in range(m):
            ns = nxt[sid][a[pos]]
            dp_cur[sid] = add[sid][a[pos]] * (n - pos) + dp_next[ns]

        ans += dp_cur[empty]
        dp_next = dp_cur

    return str(ans)

# provided sample
assert run("8\n1 3 2 0 1 3 2 1\n") == "71"

# minimum size
assert run("1\n0\n") == "1"

# single nonzero
assert run("1\n3\n") == "1"

# all zeros
assert run("3\n0 0 0\n") == "10"

# first-fit behavior
assert run("3\n1 2 3\n") == "8"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 0`|`1`| Kích thước tối thiểu | 
|`1 / 3`|`1`| Giá trị đơn khác không | 
|`3 / 0 0 0`|`10`| Mọi số 0 đều tạo ra một dãy con mới | 
|`3 / 1 2 3`|`8`| Sửa hành vi tương thích đầu tiên | 

## Vỏ cạnh 

Hãy xem xét:```
1
0
```Sự phân rã ngay lập tức tạo ra một chuỗi con. Trạng thái vẫn trống vì các chuỗi con có kết thúc bằng 0 không được lưu trữ. DP cho biết thêm$1 \cdot 1$, tạo ra câu trả lời đúng 1. 

Hãy xem xét:```
2
0 0
```Số 0 đầu tiên tạo ra một dãy con. Số 0 thứ hai tạo ra một số khác. Các mảng con có các giá trị:```
[0] -> 1
[0,0] -> 2
[0] -> 1
```Tổng số là 4. Thuật toán xử lý việc này vì mỗi lần chuyển đổi bằng 0 đều đóng góp`add = 1`, trong khi trạng thái không bao giờ thay đổi. 

Coi như:```
3
1 2 3
```Việc thực hiện bất cẩn có thể xảy ra`3`sang dãy con thứ hai thay vì dãy con tương thích đầu tiên. Quy tắc thực tế chọn chuỗi con tương thích đầu tiên, tạo trạng thái`(3,2)`còn hơn là`(1,3)`. Hàm chuyển đổi quét rõ ràng từ trái sang phải và dừng ở lần khớp đầu tiên, bảo toàn chính xác định nghĩa. 

Coi như:```
4
1 1 1 1
```Chỉ phần tử đầu tiên tạo ra một chuỗi con mới. Mọi phần tử sau này sẽ sử dụng lại cùng một chuỗi con. DP ghi lại chính xác một sự kiện tạo và truyền bá sự đóng góp của nó cho tất cả các phần cuối dài hơn. Điều này xác minh rằng việc tái sử dụng nhiều lần được xử lý mà không tạo thêm trạng thái.
