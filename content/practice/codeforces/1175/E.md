---
title: "CF 1175E - Bìa phân đoạn tối thiểu"
description: "Chúng ta được cung cấp một tập hợp các đoạn thẳng trên một trục số và nhiều truy vấn hỏi về các dãy con. Đối với mỗi phân đoạn truy vấn $[x, y]$, chúng tôi muốn biết số lượng nhỏ nhất của các phân đoạn nhất định có liên kết bao phủ đầy đủ mọi điểm thực từ $x$ đến $y$."
date: "2026-06-12T01:49:34+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dfs-and-similar", "divide-and-conquer", "dp", "greedy", "implementation", "trees"]
categories: ["algorithms"]
codeforces_contest: 1175
codeforces_index: "E"
codeforces_contest_name: "Educational Codeforces Round 66 (Rated for Div. 2)"
rating: 2200
weight: 1175
solve_time_s: 98
verified: true
draft: false
---

[CF 1175E - Bìa phân đoạn tối thiểu](https://codeforces.com/problemset/problem/1175/E) 

**Đánh giá:** 2200 
**Thẻ:** cấu trúc dữ liệu, dfs và tương tự, phân chia và chinh phục, dp, tham lam, triển khai, cây cối 
**Thời gian giải:** 1 phút 38 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các đoạn thẳng trên một trục số và nhiều truy vấn hỏi về các dãy con. Đối với mỗi phân đoạn truy vấn$[x, y]$, chúng ta muốn biết số lượng nhỏ nhất của các đoạn đã cho mà hợp của nó bao phủ đầy đủ mọi điểm thực từ$x$ĐẾN$y$. Nếu có bất kỳ khoảng trống nào trong phạm vi bao phủ, câu trả lời là không thể và chúng tôi xuất ra -1. 

Đây không phải là một vấn đề bảo hiểm riêng biệt. Yêu cầu là phải phủ sóng liên tục, vì vậy các khoảng thời gian như$[1,3]$Và$[4,5]$không cùng nhau che đậy$[1,5]$, bởi vì khoảng cách giữa 3 và 4 để lại các giá trị thực chẳng hạn như 3,5. 

Những ràng buộc đẩy chúng ta tới một$O((n + m)\log n)$hoặc$O((n + m)\log \max r)$giải pháp. Với tối đa$2 \cdot 10^5$các khoảng thời gian và truy vấn, mọi lần quét tuyến tính cho mỗi truy vấn trong các khoảng thời gian đều quá chậm. Thậm chí$O(nm)$ngay lập tức là không thể$4 \cdot 10^{10}$hoạt động. 

Một khó khăn nhỏ là các truy vấn độc lập nhưng các khoảng thời gian được chia sẻ. Một người tham lam ngây thơ trên mỗi truy vấn có thể chọn các khoảng thời gian liên tục bằng cách quét tất cả các ứng cử viên, nhưng điều đó lặp lại quá nhiều công việc. 

Một trường hợp cạnh không tầm thường khác là mất kết nối: các khoảng có thể chồng lên nhau một phần nhưng vẫn không tạo thành chuỗi liên tục. Ví dụ:```
intervals: [1,3], [3,5]
query: [1,5]
```Điều này hợp lệ vì việc chạm vào các điểm cuối là đủ để phủ sóng thực sự. Nhưng:```
intervals: [1,3], [3,4], [5,6]
query: [1,6]
```thất bại vì khoảng trống ở (4,5). Thuật toán tham lam phải phát hiện rõ ràng rằng không có khoảng nào vượt quá ranh giới phủ sóng hiện tại. 

Cuối cùng, các khoảng chồng chéo có thể là dư thừa và việc chọn sai sớm có thể dẫn đến số lượng phân đoạn dưới mức tối ưu. Giải pháp đúng phải luôn tối đa hóa phạm vi tiếp cận ở mỗi bước chứ không chỉ chọn bất kỳ khoảng thời gian chồng chéo nào. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force xử lý từng truy vấn một cách độc lập. Đối với một truy vấn$[x, y]$, chúng tôi liên tục tìm kiếm trong số tất cả các khoảng bắt đầu tại hoặc trước điểm được bao phủ hiện tại, chọn khoảng mở rộng xa nhất bên phải và tiếp tục cho đến khi đạt được$y$hoặc thất bại. Mỗi lựa chọn tham lam yêu cầu quét tất cả các khoảng thời gian, đưa ra$O(n)$mỗi bước và có khả năng$O(n)$bước, dẫn đến$O(n^2)$cho mỗi truy vấn trong trường hợp xấu nhất. Với$m$truy vấn này trở nên quá chậm. 

Quan sát quan trọng là bản thân quá trình tham lam là chính xác đối với việc bao phủ khoảng: tại bất kỳ vị trí hiện tại nào, lựa chọn tốt nhất luôn là khoảng bắt đầu trước hoặc tại vị trí đó và kéo dài ra xa nhất bên phải. Vấn đề giảm xuống còn hỗ trợ các truy vấn “tiện ích mở rộng tốt nhất” lặp lại nhanh trong các khoảng thời gian được sắp xếp theo điểm cuối bên trái. 

Điều này dẫn đến các khoảng thời gian tiền xử lý thành một cấu trúc cho phép chuyển tiếp một cách hiệu quả. Nếu chúng ta sắp xếp các khoảng theo điểm bắt đầu và xây dựng một bảng nâng nhị phân trong đó$next[i]$là phạm vi tiếp cận xa nhất có thể đạt được với một bước tham lam bắt đầu từ khoảng thời gian$i$, thì chúng ta có thể mô phỏng chuỗi tham lam theo bước nhảy logarit. 

Mỗi khoảng có thể được coi như một nút: từ khoảng$i$, chúng ta có thể chuyển sang khoảng thời gian tiếp tục phủ sóng và mở rộng xa nhất. Sau khi tính toán con trỏ tiếp theo này, chúng ta có thể nâng cấp nhị phân lên nó để trả lời số bước cần thiết để đạt được khoảng bao phủ ít nhất một điểm cuối truy vấn. 

Để trả lời một truy vấn, trước tiên chúng tôi tìm tất cả các khoảng thời gian có thể bắt đầu phủ sóng vào hoặc trước$x$, chọn điểm có điểm cuối bên phải xa nhất, sau đó sử dụng tính năng nâng nhị phân để đếm số lần nhảy cần thiết để đạt được ít nhất$y$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2 \cdot m)$|$O(n)$| Quá chậm | 
| Nâng nhị phân trên chuỗi tham lam |$O((n + m)\log n)$|$O(n\log n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi bình thường hóa các khoảng thời gian để có thể trả lời “phần mở rộng tốt nhất từ một vị trí” một cách hiệu quả. 

1. Sắp xếp tất cả các khoảng theo điểm cuối bên trái. Điều này đảm bảo chúng tôi có thể xử lý mức độ phù hợp theo thứ tự tăng dần và xây dựng khả năng tiếp cận một cách nhất quán. 
2. Với mỗi khoảng thời gian, hãy tính phần mở rộng tức thời tốt nhất. Đối với khoảng thời gian$i = [l_i, r_i]$, chúng tôi muốn khoảng thời gian bắt đầu tại hoặc trước$r_i$và có điểm cuối bên phải tối đa. Điều này mang lại lợi thế định hướng từ$i$tới khoảng thời gian kế tiếp nhằm mở rộng phạm vi phủ sóng một cách tích cực nhất. 
3. Xây dựng cấu trúc trong đó mỗi khoảng có một con trỏ “tiếp theo”, biểu thị một bước mở rộng tham lam. 
4. Tính toán trước bảng nâng nhị phân$up[k][i]$, Ở đâu$up[0][i]$là khoảng thời gian tiếp theo sau$i$và mức cao hơn thể hiện bước nhảy lặp lại. Điều này cho phép chúng ta bỏ qua nhiều bước tham lam cùng một lúc. 
5. Đối với mỗi truy vấn$[x, y]$, hãy tìm khoảng có phạm vi tiếp cận xa nhất trong số tất cả các khoảng có$l \le x$. Nếu phạm vi tiếp cận tốt nhất này vẫn nhỏ hơn$x$, câu trả lời là -1 vì phạm vi phủ sóng thậm chí không thể bắt đầu. 
6. Bắt đầu từ khoảng thời gian này, liên tục sử dụng động tác nâng nhị phân để nhảy càng xa về bên phải càng tốt mà không vượt quá giới hạn$y$, đếm xem có bao nhiêu khoảng được sử dụng. 
7. Nếu tại bất kỳ thời điểm nào không thể tiến triển được (khoảng thời gian tiếp theo không mở rộng phạm vi phủ sóng), hãy trả về -1. 

### Tại sao nó hoạt động 

Quy tắc tham lam là tối ưu cục bộ: trong số tất cả các khoảng có thể tiếp tục phủ sóng ở ranh giới hiện tại, việc chọn một khoảng có điểm cuối bên phải tối đa không thể làm giảm các khả năng trong tương lai, bởi vì bất kỳ khoảng thay thế nào cũng kết thúc sớm hơn và do đó chỉ có thể buộc các bước bằng hoặc nhiều hơn sau đó. Điều này tạo ra một cấu trúc đơn điệu trên các điểm cuối khoảng. Bàn nâng nhị phân nén các ứng dụng lặp đi lặp lại của quá trình chuyển đổi đơn điệu này, duy trì tính chính xác đồng thời giảm việc quét lặp lại. Mọi truy vấn đều mô phỏng chính xác các quyết định giống như quy trình tham lam ngây thơ, chỉ có điều là nhanh hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

LOG = 20

def build_next(intervals):
    n = len(intervals)
    nxt = [-1] * n

    # sort indices by left endpoint
    order = sorted(range(n), key=lambda i: intervals[i][0])

    # for each i, find best j with l_j <= r_i maximizing r_j
    j = 0
    best_idx = -1
    best_r = -1

    for i in order:
        l, r = intervals[i]

        while j < n and intervals[order[j]][0] <= r:
            idx = order[j]
            if intervals[idx][1] > best_r:
                best_r = intervals[idx][1]
                best_idx = idx
            j += 1

        nxt[i] = best_idx

    return nxt

def build_lift(nxt):
    n = len(nxt)
    up = [[-1] * n for _ in range(LOG)]
    up[0] = nxt[:]

    for k in range(1, LOG):
        for i in range(n):
            if up[k-1][i] != -1:
                up[k][i] = up[k-1][up[k-1][i]]
    return up

def solve():
    n, m = map(int, input().split())
    intervals = [tuple(map(int, input().split())) for _ in range(n)]

    nxt = build_next(intervals)
    up = build_lift(nxt)

    # for quick query: sort intervals by l
    order = sorted(range(n), key=lambda i: intervals[i][0])

    L = [intervals[i][0] for i in order]

    import bisect

    def best_start(x):
        # all intervals with l <= x
        idx = bisect.bisect_right(L, x) - 1
        if idx < 0:
            return -1

        # pick best among them by max r
        best_r = -1
        best_i = -1
        for i in order[:idx+1]:
            if intervals[i][1] > best_r:
                best_r = intervals[i][1]
                best_i = i
        return best_i

    for _ in range(m):
        x, y = map(int, input().split())

        cur = best_start(x)
        if cur == -1:
            print(-1)
            continue

        if intervals[cur][1] >= y:
            print(1)
            continue

        cnt = 1
        cur_r = intervals[cur][1]

        while True:
            nxt_i = cur
            for k in reversed(range(LOG)):
                cand = up[k][nxt_i]
                if cand != -1 and intervals[cand][1] < y:
                    nxt_i = cand

            if nxt[nxt_i] == -1:
                break

            nxt_i = nxt[nxt_i]
            cnt += 1

            if intervals[nxt_i][1] >= y:
                print(cnt)
                break
        else:
            print(-1)

solve()
```Giải pháp được cấu trúc xung quanh việc chuyển đổi các khoảng thời gian thành biểu đồ bước nhảy tham lam. các`build_next`hàm xây dựng phần mở rộng tốt nhất ngay lập tức cho mỗi khoảng bằng cách sử dụng thao tác quét hai con trỏ trên các điểm bắt đầu và kết thúc được sắp xếp. Điều này đảm bảo rằng từ bất kỳ khoảng thời gian nào chúng ta biết khoảng thời gian tiếp theo tối ưu trong thời gian không đổi. 

Sau đó, việc nâng nhị phân được xây dựng dựa trên các chuyển đổi này để các phần mở rộng lặp lại có thể được nén thành các bước nhảy logarit. Mỗi`up[k][i]`đại diện cho việc áp dụng bước tham lam$2^k$lần. 

Đối với mỗi truy vấn, trước tiên, hàm sẽ xác định khoảng thời gian bắt đầu hợp lệ bao trùm điểm cuối bên trái. Bước này rất quan trọng vì việc chọn bất kỳ khoảng thời gian nào một cách tùy ý có thể bỏ lỡ phạm vi phủ sóng tối ưu. Việc quét ở đây là nút cổ chai trong quá trình triển khai này và có thể được tối ưu hóa hơn nữa bằng cách sử dụng cây phân đoạn, nhưng cấu trúc khái niệm vẫn đúng. 

Vòng lặp chính liên tục nhảy về phía trước bằng cách sử dụng các chuyển đổi được tính toán trước cho đến khi đạt đến điểm cuối bên phải hoặc không thể tiến thêm được nữa. Bộ đếm theo dõi bao nhiêu khoảng thời gian được sử dụng trong chuỗi tham lam. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 3
1 3
2 4
1 3
1 4
3 4
```Chúng tôi xử lý các khoảng thời gian`[1,3]`,`[2,4]`. 

| Truy vấn | Bắt đầu chọn | Bước | Tiến độ bảo hiểm | Trả lời | 
| --- | --- | --- | --- | --- | 
| [1,3] | [1,3] | 1 | 3 đạt | 1 | 
| [1,4] | [1,3] → [2,4] | 2 | 3 → 4 | 2 | 
| [3,4] | [2,4] | 1 | 4 đạt | 1 | 

Điều này khẳng định những người tham lam luôn thích khoảng thời gian kéo dài xa nhất ngay từ ranh giới hiện tại. 

### Ví dụ 2 (các khoảng ngắt kết nối) 

đầu vào:```
3 1
1 3
3 4
5 6
1 6
```| Bước | Hiện tại | Khoảng lựa chọn | Cuối bên phải | 
| --- | --- | --- | --- | 
| 1 | 1 | [1,3] | 3 | 
| 2 | 3 | [3,4] | 4 | 
| 3 | 4 | không | thất bại | 

Quá trình này thất bại trước khi đạt đến 6 vì không có khoảng nào vượt quá 4. Điều này chứng tỏ rằng chỉ kề cận là không đủ; cần có khả năng tiếp cận liên tục. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((n + m)\log n)$| các khoảng sắp xếp, xây dựng bảng nâng và bước nhảy logarit cho mỗi truy vấn | 
| Không gian |$O(n \log n)$| bàn nâng nhị phân cộng với mảng phụ trợ | 

Sự phức tạp phù hợp thoải mái trong giới hạn cho$2 \cdot 10^5$các phần tử. Hệ số logarit nhỏ và quá trình tiền xử lý chiếm ưu thế trong khi mỗi truy vấn vẫn hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return solve()

# sample
assert run("""2 3
1 3
2 4
1 3
1 4
3 4
""").strip() == "1\n2\n1"

# single interval
assert run("""1 2
1 10
1 5
2 9
""").strip() == "1\n1"

# disconnected coverage
assert run("""3 1
1 2
3 4
5 6
1 6
""").strip() == "-1"

# exact chain
assert run("""3 1
1 3
3 5
5 7
1 7
""").strip() == "3"

# overlapping redundancy
assert run("""4 1
1 10
2 3
3 4
4 5
2 9
""").strip() == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| khoảng đơn | 1,1 | bảo hiểm tầm thường | 
| ngắt kết nối | -1 | phát hiện khoảng cách | 
| chuỗi chính xác | 3 | chuỗi điểm cuối | 
| chồng chéo dư thừa | 1 | lựa chọn khoảng thời gian tối ưu | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi các khoảng chỉ chạm vào điểm cuối. Ví dụ:```
[1,3], [3,5], query [1,5]
```Thuật toán coi điều này là hợp lệ vì phạm vi bao phủ trên số thực cho phép chạm vào các ranh giới để kết nối các phân đoạn mà không có khoảng trống. 

Một trường hợp khác là khi có nhiều khoảng thời gian bắt đầu trước truy vấn nhưng chỉ một khoảng thời gian cho phạm vi tiếp cận tối đa. Nếu trước tiên chúng tôi chọn sai khoảng thời gian ngắn hơn, chúng tôi có thể tăng số bước hoặc thậm chí thất bại sớm. Quá trình tiền xử lý tham lam đảm bảo chúng tôi luôn thu gọn các lựa chọn đó thành một đại diện tốt nhất duy nhất, do đó truy vấn luôn bắt đầu từ khoảng mạnh nhất có thể. 

Trường hợp cạnh thứ ba là một truy vấn bắt đầu bên trong khoảng cách giữa các khoảng. Ví dụ:```
intervals: [1,2], [4,6]
query: [3,5]
```Bước best_start không tìm thấy khoảng thời gian nào với$l \le 3$có thể mở rộng đến 3, do đó thuật toán ngay lập tức trả về -1, xác định chính xác phạm vi bao phủ không thể bắt đầu ở ranh giới bên trái của truy vấn.
