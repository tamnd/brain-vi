---
title: "CF 2185E - Cuộc đua robot"
description: "Mọi robot đều nhận được trình tự di chuyển giống hệt nhau. Sau lệnh đầu tiên, tất cả các robot đều dịch chuyển với cùng một độ dịch chuyển. Sau lệnh thứ hai, tất cả các robot sẽ dịch chuyển theo một chuyển vị chung khác, v.v."
date: "2026-06-07T21:31:20+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "greedy", "implementation", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 2185
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 1074 (Div. 4)"
rating: 1500
weight: 2185
solve_time_s: 166
verified: true
draft: false
---

[CF 2185E - Cuộc đua robot](https://codeforces.com/problemset/problem/2185/E) 

**Đánh giá:** 1500 
**Tags:** tìm kiếm nhị phân, tham lam, triển khai, hai con trỏ 
**Thời gian giải:** 2m 46s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mọi robot đều nhận được trình tự di chuyển giống hệt nhau. Sau lệnh đầu tiên, tất cả các robot đều dịch chuyển với cùng một độ dịch chuyển. Sau lệnh thứ hai, tất cả các robot sẽ dịch chuyển theo một chuyển vị chung khác, v.v. 

Cho phép$p_i$là độ dịch chuyển tích lũy sau lần đầu tiên$i$hướng dẫn. Một robot xuất phát ở vị trí$a$sẽ ở vị trí$a + p_i$sau bước$i$. 

Robot sẽ chết ngay khi chạm vào một cành cây lần đầu tiên. Chúng ta phải xuất ra, với mỗi tiền tố của lệnh, có bao nhiêu robot vẫn còn sống. 

Quan sát đầu tiên là robot không bao giờ tương tác với nhau. Liệu một robot có sống sót hay không chỉ phụ thuộc vào vị trí xuất phát của chính nó và các vị trí đột biến. Điều này cho phép chúng tôi tính toán thời gian chết độc lập cho mỗi robot. 

Những hạn chế là thách thức thực sự. Trên tất cả các trường hợp thử nghiệm, tổng của$n$,$m$, Và$k$mỗi cái nhiều nhất$2 \cdot 10^5$. Bất kỳ giải pháp nào mô phỏng mọi robot cho mọi hướng dẫn đều yêu cầu$O(nk)$công việc có thể đạt được$4 \cdot 10^{10}$hoạt động và hoàn toàn không khả thi. Chúng ta cần một cái gì đó gần gũi$O((n+m+k)\log k)$. 

Một trường hợp phức tạp xuất hiện khi robot không bao giờ đạt được bất kỳ đột biến nào. Ví dụ:```
1
1 1 3
10
100
LRL
```Robot chỉ di chuyển gần vị trí 10 trong khi mũi nhọn ở mức 100. Các câu trả lời đúng là:```
1 1 1
```Một giải pháp chỉ nhìn vào vị trí cuối cùng sẽ kết luận sai rằng một số va chạm trước đó đã xảy ra. 

Một sai lầm dễ mắc phải khác là quên rằng việc chạm vào một mũi nhọn ở bất kỳ thời điểm trung gian nào sẽ giết chết robot ngay lập tức. Coi như:```
1
1 1 2
1
2
RL
```Robot di chuyển$1 \to 2 \to 1$. Nó chết sau lần di chuyển đầu tiên nên câu trả lời là:```
0 0
```Chỉ kiểm tra vị trí sau tất cả các hướng dẫn sẽ bỏ lỡ sự va chạm này. 

Trường hợp cuối cùng không rõ ràng là robot có thể chết do bị đột biến ở hai bên. Ví dụ:```
robot = 10
spikes = {7, 20}
moves = LLL
```Robot đạt đến 7 ở lần di chuyển thứ ba và chết. Chỉ nhìn vào cành gần nhất ở bên phải sẽ không chính xác. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi robot, hãy mô phỏng trình tự hướng dẫn từng bước, cập nhật vị trí của nó và kiểm tra xem nó có đứng trên đỉnh nhọn hay không. Bước đầu tiên mà điều này xảy ra là thời điểm nó chết. 

Điều này đúng vì nó tuân theo chính xác quy trình được mô tả trong tuyên bố. Thật không may, nó thực hiện$O(nk)$cập nhật. Với cả hai giá trị gần$2 \cdot 10^5$, thời gian chạy trở nên quá lớn. 

Quan sát quan trọng là tất cả các robot đều có chung trình tự dịch chuyển. 

Cho phép$$p_i$$là độ dịch chuyển tích lũy sau$i$hướng dẫn. 

Đối với robot xuất phát ở vị trí$a$, vị trí của nó sau bước$i$là$$a+p_i.$$Robot sẽ chết nếu có một vị trí đột biến nào đó nằm trong số tất cả các vị trí mà nó đã ghé thăm. 

Giả sử chúng ta biết độ dịch chuyển tích lũy tối thiểu và tối đa đạt được trong lần đầu tiên$i$hướng dẫn:$$L_i=\min_{1\le j\le i} p_j, \qquad R_i=\max_{1\le j\le i} p_j.$$Bởi vì mỗi bước di chuyển sẽ thay đổi vị trí chính xác một đơn vị, nên bước đi sẽ ghé thăm mọi điểm nguyên giữa độ dịch chuyển tối thiểu và tối đa của nó. Như vậy, trong thời gian đầu$i$hướng dẫn, một robot bắt đầu từ$a$thăm chính xác khoảng thời gian$$[a+L_i,\; a+R_i].$$Vậy là robot vẫn còn sống sau$i$bước nếu và chỉ khi không có đột biến nào nằm trong khoảng này. Các gai duy nhất quan trọng là gai gần nhất ở bên trái và gai gần nhất ở bên phải vị trí bắt đầu của robot. 

Đối với mỗi robot, chúng ta có thể tìm kiếm nhị phân độ dài tiền tố lớn nhất mà khoảng thời gian đó vẫn tránh được cả các xung lân cận. Điều đó đưa ra bước cuối cùng nơi robot còn sống. Sau khi tính toán giá trị này cho mỗi robot, chúng ta chỉ cần đếm xem có bao nhiêu robot tồn tại ở mỗi độ dài tiền tố. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(nk)$|$O(m)$| Quá chậm | 
| Tối ưu |$O((m+k)\log m + n(\log m+\log k))$|$O(m+k+n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Tính các cực trị chuyển vị tích lũy 

Xử lý chuỗi lệnh một lần. 

Duy trì sự dịch chuyển hiện tại$p$. 

Đối với mỗi tiền tố$i$, cửa hàng:$$L_i=\min(L_{i-1},p),$$Và$$R_i=\max(R_{i-1},p).$$Các giá trị này mô tả độ dịch chuyển ngoài cùng bên trái và ngoài cùng bên phải đạt được trong lần đầu tiên$i$hướng dẫn. 

### 2. Sắp xếp vị trí tăng đột biến 

Sắp xếp tất cả các tọa độ tăng đột biến. 

Đối với mỗi vị trí robot$a$, tìm: 

- mũi nhọn gần nhất ở bên trái, 
- cành gần nhất ở đúng bên phải. 

Điều này được thực hiện bằng tìm kiếm nhị phân trong mảng tăng đột biến đã được sắp xếp. 

### 3. Tìm kiếm nhị phân thời gian sống sót của mỗi robot 

Đối với robot cố định tại vị trí$a$, cho phép:

-$left$là cành trái gần nhất của nó, 
-$right$là cành phải gần nhất của nó. 

Đối với độ dài tiền tố$mid$, robot vẫn còn sống chính xác khi$$left < a+L_{mid}$$Và$$a+R_{mid} < right.$$Bất đẳng thức đầu tiên có nghĩa là khoảng đã khám phá chưa đạt đến đỉnh bên trái. Thứ hai có nghĩa là nó chưa đạt đến mức tăng đột biến phù hợp. 

Những điều kiện này là đơn điệu. Một khi robot chết đi, nó sẽ không bao giờ có thể sống lại được. Do đó chúng ta tìm kiếm nhị phân độ dài tiền tố lớn nhất thỏa mãn cả hai bất đẳng thức. 

Gọi giá trị này$c$. Robot vẫn còn sống sau các bước$1,2,\dots,c$và chết sau đó. 

### 4. Đếm số người sống sót cho mỗi bước 

Lưu trữ tất cả các giá trị$c$. 

Sắp xếp chúng. 

Đối với mỗi số lượng lệnh$i$, chúng ta cần số lượng robot với$$c \ge i.$$Quét hai con trỏ trên mảng đã sắp xếp sẽ đưa ra tất cả các câu trả lời theo thời gian tuyến tính. 

### Tại sao nó hoạt động 

Đối với bất kỳ tiền tố hướng dẫn nào, mọi robot đều tuân theo cùng một bước di chuyển. Tập hợp các chuyển vị đã ghé thăm trong quá trình đi bộ đó chính xác là khoảng giữa chuyển vị tối thiểu và tối đa đạt được cho đến nay. 

Robot xuất phát tại vị trí$a$do đó truy cập chính xác khoảng thời gian$[a+L_i,a+R_i]$. Robot tồn tại khi và chỉ khi không có đột biến nào nằm trong khoảng đó. 

Các gai trái và phải gần nhất xác định hoàn toàn liệu khoảng đó có chứa gai hay không. Nếu khoảng cách nằm ở giữa chúng thì tất cả các đỉnh khác thậm chí còn ở xa hơn và không thể chạm tới được. Nếu một trong hai mũi nhọn lân cận bước vào khoảng thời gian, robot sẽ chết ngay lập tức. 

Điều kiện tồn tại là đơn điệu đối với độ dài tiền tố, vì vậy tìm kiếm nhị phân sẽ tìm chính xác tiền tố cuối cùng còn sót lại. Đếm các robot có thời gian sống sót ít nhất$i$mang lại số còn sống sau$i$hướng dẫn. 

## Giải pháp Python```python
import sys
from bisect import bisect_left, bisect_right

input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        n, m, k = map(int, input().split())
        robots = list(map(int, input().split()))
        spikes = sorted(map(int, input().split()))
        s = input().strip()

        min_disp = [0] * (k + 1)
        max_disp = [0] * (k + 1)

        cur = 0
        for i, ch in enumerate(s, 1):
            if ch == 'L':
                cur -= 1
            else:
                cur += 1

            min_disp[i] = min(min_disp[i - 1], cur)
            max_disp[i] = max(max_disp[i - 1], cur)

        NEG_INF = -10**18
        POS_INF = 10**18

        survive = []

        for a in robots:
            pos = bisect_left(spikes, a)

            left_spike = spikes[pos - 1] if pos > 0 else NEG_INF
            right_spike = spikes[pos] if pos < m else POS_INF

            lo, hi = 1, k
            best = 0

            while lo <= hi:
                mid = (lo + hi) // 2

                alive = (
                    left_spike < a + min_disp[mid]
                    and a + max_disp[mid] < right_spike
                )

                if alive:
                    best = mid
                    lo = mid + 1
                else:
                    hi = mid - 1

            survive.append(best)

        survive.sort()

        ans = []
        ptr = 0

        for step in range(1, k + 1):
            while ptr < n and survive[ptr] < step:
                ptr += 1
            ans.append(str(n - ptr))

        print(*ans)

solve()
```Phần đầu tiên tính toán cực trị dịch chuyển tích lũy.`min_disp[i]`Và`max_disp[i]`mô tả toàn bộ phạm vi được khám phá bởi chuỗi chuyển động cho đến bước`i`. 

Đối với mỗi robot, chúng tôi xác định vị trí các xung lân cận bằng cách sử dụng tìm kiếm nhị phân trên mảng tăng đột biến đã được sắp xếp. Lính gác vô hạn được sử dụng khi không có gai nhọn ở một bên. Điều này tránh logic trường hợp đặc biệt sau này. 

Việc tìm kiếm nhị phân trên các tiền tố lệnh dựa vào tính đơn điệu. Nếu robot tồn tại ở tiền tố dài hơn thì nó cũng tồn tại ở mọi tiền tố ngắn hơn. Ngược lại, khi mức tăng đột biến bước vào khoảng được khám phá, mọi tiền tố lớn hơn vẫn không hợp lệ. 

Giai đoạn đếm cuối cùng rất dễ xảy ra sai sót.`survive[i]`lưu trữ chỉ số bước lớn nhất mà robot vẫn còn sống. Robot góp phần trả lời`step`chính xác khi nào`survive[i] >= step`. Sắp xếp các giá trị này cho phép quét một con trỏ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2 1 3
0 1
2
LRR
```Tiền tố dịch chuyển là: 

| Bước | Di chuyển | Dịch chuyển | Tối thiểu | Tối đa | 
| --- | --- | --- | --- | --- | 
| 1 | L | -1 | -1 | 0 | 
| 2 | R | 0 | -1 | 0 | 
| 3 | R | 1 | -1 | 1 | 

Robot ở mức 0: 

| Bước | Khoảng thời gian đã truy cập | Spike 2 đã đạt tới? | 
| --- | --- | --- | 
| 1 | [-1,0] | Không | 
| 2 | [-1,0] | Không | 
| 3 | [-1,1] | Không | 

Robot lúc 1: 

| Bước | Khoảng thời gian đã truy cập | Spike 2 đã đạt tới? | 
| --- | --- | --- | 
| 1 | [0,1] | Không | 
| 2 | [0,1] | Không | 
| 3 | [0,2] | Có | 

Thời gian sống sót là`[3, 2]`. 

Câu trả lời:```
2 2 1
```Ví dụ này cho thấy lý do tại sao việc giải thích khoảng thời gian lại có tác dụng. Robot thứ hai chết chính xác khi điểm cuối bên phải đạt đến mức tăng đột biến. 

### Mẫu 3 

đầu vào:```
3 2 3
1 3 7
9 6
RRL
```Thống kê tiền tố: 

| Bước | Dịch chuyển | Tối thiểu | Tối đa | 
| --- | --- | --- | --- | 
| 1 | 1 | 0 | 1 | 
| 2 | 2 | 0 | 2 | 
| 3 | 1 | 0 | 2 | 

Robot ở vị trí thứ 7 có đỉnh nhọn ở vị trí thứ 9 ở bên phải. 

| Bước | Khoảng thời gian | 
| --- | --- | 
| 1 | [7,8] | 
| 2 | [7,9] | 
| 3 | [7,9] | 

Nó chết ở bước 2. 

Robot ở mức 1 và robot ở mức 3 không bao giờ đạt được mức tăng đột biến. 

Câu trả lời:```
3 2 2
```Điều này chứng tỏ rằng chỉ có chuyển vị tối đa và tối thiểu mới quan trọng. Con đường chính xác giữa chúng là không liên quan. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((m+k)\log m + n(\log m+\log k))$| sắp xếp đột biến, định vị hàng xóm và thời gian tồn tại của tìm kiếm nhị phân | 
| Không gian |$O(n+m+k)$| mảng tăng đột biến, cực trị tiền tố và độ dài tồn tại | 

Vì tổng số tiền của$n$,$m$, Và$k$trên tất cả các trường hợp thử nghiệm nhiều nhất là$2 \cdot 10^5$, sự phức tạp này dễ dàng phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io
from bisect import bisect_left

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline
    out = []

    t = int(input())

    for _ in range(t):
        n, m, k = map(int, input().split())
        robots = list(map(int, input().split()))
        spikes = sorted(map(int, input().split()))
        s = input().strip()

        mn = [0] * (k + 1)
        mx = [0] * (k + 1)

        cur = 0
        for i, ch in enumerate(s, 1):
            cur += -1 if ch == 'L' else 1
            mn[i] = min(mn[i - 1], cur)
            mx[i] = max(mx[i - 1], cur)

        surv = []

        for a in robots:
            p = bisect_left(spikes, a)

            left = spikes[p - 1] if p else -10**18
            right = spikes[p] if p < m else 10**18

            lo, hi = 1, k
            best = 0

            while lo <= hi:
                mid = (lo + hi) // 2

                if left < a + mn[mid] and a + mx[mid] < right:
                    best = mid
                    lo = mid + 1
                else:
                    hi = mid - 1

            surv.append(best)

        surv.sort()

        ptr = 0
        ans = []
        for step in range(1, k + 1):
            while ptr < n and surv[ptr] < step:
                ptr += 1
            ans.append(str(n - ptr))

        out.append(" ".join(ans))

    return "\n".join(out)

# provided sample
assert run("""3
2 1 3
0 1
2
LRR
2 3 3
2 4
1 3 5
LRL
3 2 3
1 3 7
9 6
RRL
""") == """2 2 1
0 0 0
3 2 2"""

# minimum size
assert run("""1
1 1 1
0
10
L
""") == "1"

# dies immediately
assert run("""1
1 1 2
1
2
RL
""") == "0 0"

# no spike ever reached
assert run("""1
1 1 3
10
100
LRL
""") == "1 1 1"

# left-side collision
assert run("""1
1 1 3
10
7
LLL
""") == "1 1 0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Robot đơn, tăng đột biến |`1`| Kích thước tối thiểu | 
|`1 -> 2`va chạm |`0 0`| Chết ngay lập tức | 
| Spike xa |`1 1 1`| Robot tồn tại mãi mãi | 
| Mũi trái ở khoảng cách 3 |`1 1 0`| Xử lý va chạm bên trái | 

## Vỏ cạnh 

### Robot không bao giờ đạt được mức tăng đột biến nào 

đầu vào:```
1
1 1 3
10
100
LRL
```Phạm vi dịch chuyển sau mỗi tiền tố vẫn ở trong$[-1,0]$. Khoảng thời gian được truy cập là:```
[9,10]
[9,10]
[9,10]
```Mức tăng đột biến ở mức 100 không bao giờ đi vào khoảng thời gian, vì vậy tìm kiếm nhị phân trả về độ dài tồn tại 3. Câu trả lời là:```
1 1 1
```### Va chạm ngay lập tức 

đầu vào:```
1
1 1 2
1
2
RL
```Sau bước đi đầu tiên, robot đứng ở vị trí thứ 2. Khoảng thời gian sau bước 1 là:```
[1,2]
```đã chứa phần tăng đột biến. Tiền tố lớn nhất còn tồn tại là 0, tạo ra:```
0 0
```### Va chạm từ phía bên trái 

đầu vào:```
1
1 1 3
10
7
LLL
```Chuyển vị cực tiểu trở thành:```
-1, -2, -3
```Sau bước 3, khoảng thời gian truy cập là:```
[7,10]
```đạt mức tăng đột biến ở mức 7. Robot sống sót sau đúng hai bước, đưa ra:```
1 1 0
```Trường hợp này xác nhận rằng việc chỉ kiểm tra các gai ở bên phải sẽ không chính xác. Phải xem xét mức tăng đột biến gần nhất ở hai bên.
