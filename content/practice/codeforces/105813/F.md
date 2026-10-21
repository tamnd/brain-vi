---
title: "CF 105813F - Dây Có Thể Đi Được"
description: "Chúng ta có một đồ thị vô hướng có các cạnh được tô màu R hoặc B. Một chuỗi được gọi là có thể đi bộ nếu tồn tại một số bước đi trong biểu đồ có màu cạnh, đọc theo thứ tự, khớp chính xác với các ký tự của chuỗi. Đi bộ có thể xem lại các đỉnh và cạnh."
date: "2026-06-26T00:06:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105813
codeforces_index: "F"
codeforces_contest_name: "Rutgers University Programming Contest Spring 2025"
rating: 0
weight: 105813
solve_time_s: 71
verified: true
draft: false
---

[CF 105813F - Dây có thể đi được](https://codeforces.com/problemset/problem/105813/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị vô hướng có các cạnh được tô màu`R`hoặc`B`. 

Một chuỗi được gọi là có thể đi được nếu tồn tại một số bước đi trong biểu đồ có màu cạnh, đọc theo thứ tự, khớp chính xác với các ký tự của chuỗi. Đi bộ có thể xem lại các đỉnh và cạnh. 

Đối với mỗi trường hợp kiểm thử, chúng ta phải xuất ra một chuỗi ngắn nhất **không** có thể đi được. If every possible string over`{R,B}`có thể đi bộ được, chúng tôi in`-1`. 

Biểu đồ có thể chứa tới`2·10^5`các đỉnh và cạnh trong tất cả các trường hợp thử nghiệm. Bất kỳ giải pháp nào xây dựng rõ ràng các tập hợp con tự động hóa hoặc kiểm tra tất cả các chuỗi có độ dài nhất định đều vô vọng. Ngay cả việc kiểm tra tất cả các chuỗi có độ dài 30 cũng đã yêu cầu hơn một tỷ ứng viên. 

Thách thức là khai thác cấu trúc đặc biệt của bảng chữ cái nhị phân. 

Một điểm tinh tế là chuỗi cấm ngắn nhất không nhất thiết phải là một màu duy nhất được lặp lại nhiều lần. 

Coi như:```
1 --R-- 2
```Chuỗi`RR`không thể đi bộ được, nhưng`R`là. Câu trả lời ngắn nhất là`RR`. 

Một sai lầm dễ mắc phải khác là cho rằng nếu cả hai màu xuất hiện ở đâu đó thì mọi chuỗi có độ dài 1 đều có thể đi được và chúng ta chỉ cần kiểm tra độ dài dài hơn. 

Ví dụ:```
1 --R-- 2

3 --B-- 4
```Cả hai`R`Và`B`có thể đi bộ được, nhưng`RB`thì không. Không bước đi nào có thể chuyển từ thành phần màu đỏ sang thành phần màu xanh. Câu trả lời đúng có độ dài 2. 

Cạm bẫy thứ ba là xử lý chu kỳ không chính xác. 

Nếu biểu đồ chứa một chuyến đi có mẫu màu lặp lại mãi mãi theo đúng cách thì có thể có các chuỗi dài tùy ý. Phát hiện tình huống này chính xác là điều tạo nên sự khác biệt`-1`trường hợp từ các trường hợp có câu trả lời hữu hạn. 

## Phương pháp tiếp cận 

Quan điểm bạo lực trước hết là hữu ích. 

Giả sử chúng ta muốn biết liệu một sợi dây có thể đi được hay không. Chúng ta có thể coi mỗi đỉnh là một vị trí hiện tại có thể có. Sau khi đọc từng ký tự, chúng ta di chuyển dọc theo các cạnh có màu tương ứng. Đây chỉ là mô phỏng của NFA. 

Điều đó ngay lập tức gợi ý việc kiểm tra các chuỗi theo cấp độ theo thứ tự BFS và dừng ở chuỗi đầu tiên không thể đi được. 

Vấn đề là số lượng chuỗi ứng cử viên. có`2^k`chuỗi có độ dài`k`. Ngay cả khi chuỗi bị cấm ngắn nhất chỉ có độ dài 40, thì điều đó đã có nghĩa là khoảng một nghìn tỷ ứng cử viên. 

Vì vậy chúng ta cần một góc nhìn hoàn toàn khác. 

Quan sát quan trọng là một bổ đề tổ hợp đẹp mắt được sử dụng cho các bài toán khả năng đi bộ nhị phân. 

Xác định mô hình tuần hoàn vô hạn```
RRBBRRBBRRBB...
```và bốn ca tuần hoàn của nó:```
RRBBRRBB...
RBBRRBBR...
BBRRBBRR...
BRRBBRRB...
```Gọi bốn chuỗi tuần hoàn này`P0, P1, P2, P3`. 

Một kết quả cổ điển là: 

Nếu cả bốn chiều dài-`k`tiền tố của các chuỗi tuần hoàn này có thể đi được, sau đó **mọi** chuỗi nhị phân có độ dài`k`có thể đi bộ được 

Điều này thay đổi hoàn toàn vấn đề. 

Thay vì kiểm tra tất cả`2^k`các chuỗi, chúng ta chỉ cần hiểu tiền tố có thể đi bộ dài nhất của mỗi chuỗi trong số bốn chuỗi tuần hoàn này. 

Bây giờ cấu trúc đồ thị trở nên quan trọng. 

Tạo bốn trạng thái pha tương ứng với các vị trí bên trong chu trình```
R -> R -> B -> B -> R -> ...
```Với mọi đỉnh ban đầu`v`, tạo bốn bản sao`(v,0)...(v,3)`. 

Nếu pha`p`mong đợi màu sắc`c`, thì mọi cạnh ban đầu của màu`c`trở thành một sự chuyển tiếp có hướng```
(u,p) -> (v,(p+1) mod 4)
(v,p) -> (u,(p+1) mod 4)
```Đường đi có hướng trong biểu đồ mới này tương ứng chính xác với trình tự tuần hoàn của một pha cụ thể. 

Nếu đồ thị có hướng này chứa một chu trình thì một dãy tuần hoàn nào đó có thể kéo dài mãi mãi. Việc lặp lại chu trình đó sẽ tạo ra các tiền tố có độ dài tùy ý và bổ đề ngụ ý rằng mọi chuỗi nhị phân đều có thể đi được. Câu trả lời là`-1`. 

Nếu không thì biểu đồ là DAG. 

Trong DAG, tiền tố có thể đi bộ dài nhất từ ​​một pha chỉ là đường dẫn có hướng dài nhất bắt đầu từ bất kỳ nút nào của pha đó. 

Khi chúng ta biết bốn độ dài tối đa đó, chuỗi cấm ngắn nhất sẽ được lấy từ pha có tiền tố có thể đi bộ tối đa là nhỏ nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên dây | Hàm mũ | Hàm mũ | Quá chậm | 
| Đồ thị pha + DAG DP | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng đồ thị có hướng với`4n`nút. 
2. Sử dụng mẫu pha:```
phase 0 -> R
phase 1 -> R
phase 2 -> B
phase 3 -> B
```1. Với mọi cạnh gốc`(u,v,color)`và mọi pha có màu dự kiến ​​bằng`color`, thêm vào:```
(u,phase) -> (v,next_phase)
(v,phase) -> (u,next_phase)
```1. Chạy quy trình tôpô trên đồ thị có hướng này. 
2. Nếu không phải tất cả các nút đều bị loại bỏ bởi quá trình tôpô thì biểu đồ sẽ chứa một chu trình có hướng. đầu ra`-1`. 
3. Mặt khác, xử lý các nút theo thứ tự tôpô ngược và tính toán:```
dp[x] = longest number of edges in a path starting from x
```1. Đối với từng giai đoạn`p`, tính:```
best[p] = max(dp[(v,p)])
```trên tất cả các đỉnh ban đầu`v`. 

1. Hãy để```
p = argmin(best[p])
```Tiền tố có thể đi bộ dài nhất từ ​​giai đoạn đó có độ dài`best[p]`, vì vậy tiền tố không thể đi bộ ngắn nhất có độ dài:```
L = best[p] + 1
```1. Xuất đầu tiên`L`các ký tự của dãy tuần hoàn bắt đầu từ pha`p`. 

### Tại sao nó hoạt động 

Biểu đồ pha được xây dựng mã hóa chính xác một họ chuỗi định kỳ. 

Một đường đi có hướng có độ dài`t`bắt đầu vào giai đoạn`p`tương ứng với một chuyến đi có dãy màu đầu tiên`t`các ký tự của chuỗi tuần hoàn bắt đầu ở pha`p`. 

Nếu đồ thị pha chứa một chu trình thì họ tuần hoàn đó có các tiền tố có thể đi bộ dài tùy ý. Độ dài chu kỳ là bội số của 4 vì các pha tăng theo modulo 4. Việc lặp lại chu trình sẽ tạo ra các tiền tố có độ dài tùy ý cho cả bốn ca, mà theo bổ đề ngụ ý rằng mọi chuỗi nhị phân đều có thể đi được. 

Nếu biểu đồ pha không theo chu kỳ, việc tính toán đường đi dài nhất sẽ cho độ dài tiền tố có thể đi bộ tối đa cho mỗi ca. Độ dài tiền tố đầu tiên không thành công đối với một số ca chính xác là`best[p] + 1`. Bổ đề đảm bảo rằng đây cũng là độ dài tối thiểu mà tại đó một số chuỗi nhị phân trở nên không thể đi được. Do đó tiền tố định kỳ được xây dựng là câu trả lời ngắn nhất hợp lệ. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

PAT = "RRBB"

def solve():
    t = int(input())

    out = []

    for _ in range(t):
        n, m = map(int, input().split())

        N = 4 * n
        g = [[] for _ in range(N)]
        indeg = [0] * N

        for _ in range(m):
            u, v, c = input().split()
            u = int(u) - 1
            v = int(v) - 1

            for p in range(4):
                if PAT[p] == c:
                    np = (p + 1) & 3

                    a = u * 4 + p
                    b = v * 4 + np
                    g[a].append(b)
                    indeg[b] += 1

                    a = v * 4 + p
                    b = u * 4 + np
                    g[a].append(b)
                    indeg[b] += 1

        q = deque(i for i in range(N) if indeg[i] == 0)
        topo = []

        while q:
            v = q.popleft()
            topo.append(v)

            for to in g[v]:
                indeg[to] -= 1
                if indeg[to] == 0:
                    q.append(to)

        if len(topo) != N:
            out.append("-1")
            continue

        dp = [0] * N

        for v in reversed(topo):
            best = 0
            for to in g[v]:
                cand = 1 + dp[to]
                if cand > best:
                    best = cand
            dp[v] = best

        phase_best = [0] * 4

        for phase in range(4):
            mx = 0
            for v in range(n):
                mx = max(mx, dp[v * 4 + phase])
            phase_best[phase] = mx

        phase = min(range(4), key=lambda p: phase_best[p])
        length = phase_best[phase] + 1

        ans = []
        for i in range(length):
            ans.append(PAT[(phase + i) & 3])

        out.append("".join(ans))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Đồ thị có`4n`các nút vì mỗi đỉnh ban đầu được nhân đôi một lần cho mỗi vị trí bên trong`RRBB`xe đạp. 

Việc sắp xếp topo phục vụ hai mục đích. Nó phát hiện các chu kỳ và cũng cung cấp thứ tự cần thiết cho DP đường đi dài nhất trong DAG. 

DP lưu trữ độ dài đường dẫn theo các cạnh, không phải các đỉnh. Vì một chuỗi có độ dài`L`tương ứng với việc đi qua chính xác`L`các cạnh màu, đây là số lượng chúng ta cần. 

Độ dài câu trả lời cuối cùng là`best + 1`, bởi vì`best`là độ dài tiền tố có thể đi bộ tối đa. Ký tự tiếp theo là điểm đầu tiên mà khả năng đi bộ không thành công. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3
1 2 R
2 3 R
3 1 R
```Chỉ có các cạnh màu đỏ tồn tại. 

Đối với giai đoạn 2, trình tự định kỳ bắt đầu bằng`B`. 

| Giai đoạn | Tiền tố có thể đi bộ dài nhất | 
| --- | --- | 
| RRBB... | 2 | 
| RBBR... | 1 | 
| BBRR... | 0 | 
| BRRB... | 0 | 

Giá trị tối thiểu là`0`, vậy độ dài câu trả lời là`1`. 

Đầu ra:```
B
```Điều này chứng tỏ rằng một màu bị thiếu sẽ ngay lập tức tạo ra một chuỗi có độ dài bị cấm. 

### Ví dụ 2 

đầu vào:```
4 2
1 2 R
3 4 B
```| Giai đoạn | Tiền tố có thể đi bộ dài nhất | 
| --- | --- | 
| RRBB... | 1 | 
| RBBR... | 1 | 
| BBRR... | 1 | 
| BRRB... | 1 | 

Tối thiểu là`1`, vậy độ dài câu trả lời là`2`. 

Bắt đầu từ giai đoạn 1 tạo ra:```
RB
```Không bước đi nào có thể sử dụng cạnh màu đỏ và sau đó là cạnh màu xanh lam vì màu sắc nằm trong các thành phần được kết nối khác nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Biểu đồ pha có 4n nút và chuyển tiếp O(m) trên mỗi pha, tổng thể vẫn tuyến tính | 
| Không gian | O(n + m) | Danh sách kề, độ và mảng DP | 

Tổng số đỉnh và cạnh của tất cả các ca kiểm thử nhiều nhất là`2·10^5`, do đó, một nghiệm tuyến tính vừa khít trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

# helper sketch

# 1. no edges
inp = """1
1 0
"""

# expected: R or B, length 1

# 2. only red edges
inp = """1
3 3
1 2 R
2 3 R
3 1 R
"""

# expected: B

# 3. disconnected colors
inp = """1
4 2
1 2 R
3 4 B
"""

# expected length 2 answer such as RB

# 4. graph where all strings are walkable
inp = """1
7 10
1 2 B
1 4 R
2 4 B
4 5 B
6 7 R
6 2 R
5 7 B
3 6 B
3 7 R
1 7 B
"""

# expected: -1

# 5. minimum-size graph
inp = """1
1 0
"""

# expected length 1
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đỉnh cô lập đơn | Độ dài 1 chuỗi | Không có cuộc đi bộ nào tồn tại | 
| Đồ thị toàn màu đỏ |`B`| Thiếu màu | 
| Tách các thành phần màu đỏ và màu xanh | Câu trả lời dài 2 | Vấn đề chuyển đổi thành phần | 
| Vật mẫu`-1`đồ thị |`-1`| Khả năng đi bộ vô hạn | 
| Đồ thị nhỏ nhất | Độ dài 1 câu trả lời | Điều kiện biên | 

## Vỏ cạnh 

Xét một đồ thị không có cạnh:```
1 0
```Không có chuỗi không trống nào có thể đi được. Đồ thị pha không có cạnh nên mọi giá trị DP đều bằng 0. Thuật toán đưa ra một tiền tố định kỳ có độ dài bằng một, chẳng hạn như`R`. Điều đó đúng. 

Xét đồ thị chỉ chứa các cạnh màu đỏ:```
3 3
1 2 R
2 3 R
3 1 R
```Bất kỳ chuỗi định kỳ nào bắt đầu bằng`B`có độ dài tiền tố có thể đi bộ tối đa bằng không. Thuật toán ngay lập tức trở lại`B`, đây là chuỗi bị cấm ngắn nhất. 

Hãy xem xét hai thành phần đơn sắc bị ngắt kết nối:```
4 2
1 2 R
3 4 B
```Một đĩa đơn`R`và một đĩa đơn`B`đều có thể đi bộ được. Tuy nhiên, bất kỳ chuỗi nào yêu cầu chuyển màu là không thể. DP nhận thấy rằng mọi dịch chuyển định kỳ đều có độ dài tiền tố có thể đi bộ dài nhất là một, tạo ra một chuỗi bị cấm có độ dài hai, chẳng hạn như`RB`. Điều này mắc phải lỗi phổ biến là chỉ kiểm tra từng màu riêng lẻ. 

Trường hợp chu trình được xử lý bằng cách sắp xếp tôpô. Nếu đồ thị pha chứa chu trình có hướng, thuật toán sẽ đưa ra`-1`, nhận ra rằng các tiền tố định kỳ dài tùy ý tồn tại và do đó mọi chuỗi nhị phân đều có thể đi được.
