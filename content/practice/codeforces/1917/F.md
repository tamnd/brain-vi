---
title: "CF 1917F - Cây xây dựng"
description: "Chúng ta được cho trước độ dài của tất cả các cạnh phải xuất hiện trong cây. Cây có chính xác $n$ cạnh và $n+1$ đỉnh, vì vậy mỗi chiều dài từ mảng được sử dụng chính xác một lần làm trọng số cạnh."
date: "2026-06-08T19:48:32+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "constructive-algorithms", "dp", "trees"]
categories: ["algorithms"]
codeforces_contest: 1917
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 917 (Div. 2)"
rating: 2500
weight: 1917
solve_time_s: 116
verified: true
draft: false
---

[CF 1917F - Cây xây dựng](https://codeforces.com/problemset/problem/1917/F) 

**Đánh giá:** 2500 
**Tags:** bitmask, thuật toán xây dựng, dp, cây 
**Thời gian giải:** 1 phút 56 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho trước độ dài của tất cả các cạnh phải xuất hiện trong cây. Cây có chính xác$n$các cạnh và$n+1$các đỉnh, do đó mỗi độ dài của mảng được sử dụng chính xác một lần làm trọng số cạnh. 

Câu hỏi đặt ra là liệu chúng ta có thể nối các cạnh đó vào một cây nào đó có đường kính có trọng số chính xác là$d$. 

Một cách hữu ích để suy nghĩ về vấn đề này là mỗi đường kính của cây là một đường đi đơn giản. Nếu cây có đường kính$d$, thì ở đâu đó bên trong cây phải tồn tại một đường đi có tổng chiều dài chính xác là$d$và mọi cạnh khác được gắn vào đường dẫn đó mà không tạo đường dẫn dài hơn. 

Những hạn chế nhỏ đáng ngạc nhiên về mặt trọng lượng. Cả hai$d$và mọi$l_i$nhiều nhất là$2000$, và tổng của tất cả$n$trên đầu vào cũng nhiều nhất$2000$. Điều này ngay lập tức gợi ý một số loại lập trình động tập hợp con hoặc ba lô trên giá trị$d$. MỘT$O(nd)$hoặc$O(nd^2 / 64)$giải pháp là hoàn toàn hợp lý, trong khi việc liệt kê tập hợp con theo cấp số nhân là không thể. 

Có một số tình huống không rõ ràng có thể dễ dàng đánh lừa một cách xây dựng ngây thơ. 

Coi như:```
n = 2
d = 5
lengths = [3, 3]
```Câu trả lời đúng là`No`. 

Bất kỳ cây nào có hai cạnh chỉ là một con đường. Đường kính của nó là$3+3=6$, đã lớn hơn$d$. Một cách tiếp cận đơn giản chỉ kiểm tra xem một số tập hợp con có tính tổng bằng$d$sẽ bỏ lỡ điều này. 

Một ví dụ khác:```
n = 3
d = 10
lengths = [6, 2, 2]
```Câu trả lời đúng là`Yes`. 

Sử dụng cạnh của chiều dài$6$trên đường kính. Hai cạnh còn lại tạo thành một đường đi có độ dài$4$, tạo ra đường kính có chiều dài$10$. Một chiến lược nhất quyết xây dựng đường kính không có cạnh lớn nhất sẽ bác bỏ trường hợp này một cách không chính xác. 

Một trường hợp tế nhị hơn là:```
n = 4
d = 10
lengths = [4, 4, 4, 4]
```Câu trả lời đúng là`No`. 

Mặc dù nhiều tập hợp con có tổng gần bằng$10$, mọi cạnh không được sử dụng đều có độ dài$4$. Bất kỳ điểm đính kèm nào trên đường kính chiều dài$10$ở khoảng cách nhiều nhất$5$từ một điểm cuối, do đó việc gắn một chiều dài-$4$cạnh có thể dễ dàng tạo một đường dẫn dài hơn$10$. Hình dạng của đường kính quan trọng, không chỉ các tổng tập hợp con. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ thử mọi tập hợp con các cạnh làm ứng cử viên cho đường kính. Nếu một tập hợp con có tổng bằng$d$, khi đó chúng ta có thể kiểm tra xem các cạnh còn lại có thể được gắn mà không cần tăng đường kính hay không. 

Điều này đã vô vọng rồi. có$2^n$tập hợp con và$n$có thể đạt được$2000$. 

Quan sát quan trọng là chỉ có cạnh lớn nhất thực sự quan trọng trong số tất cả các cạnh không thuộc đường kính. 

Gọi độ dài cạnh lớn nhất là$M$. 

Giả sử chúng ta đã xây dựng một đường kính có tổng chiều dài$d$. Tất cả các cạnh còn lại có thể được gắn vào một đỉnh duy nhất trên đường dẫn đó. Nếu cạnh lớn nhất chưa được sử dụng có thể được gắn vào mà không tạo ra đường kính dài hơn thì mọi cạnh nhỏ hơn chưa được sử dụng cũng có thể được gắn vào đó. 

Điều này làm giảm vấn đề tìm đường kính và đỉnh đính kèm phù hợp. 

Một quan sát quan trọng khác là nếu hai cạnh có độ dài lớn nhất thỏa mãn$$l_n + l_{n-1} > d,$$thì câu trả lời là ngay lập tức`No`. 

Bất kỳ cây nào cũng chứa một đường đi qua hai cạnh đó và đường đi đó đã vượt quá$d$. 

Sau khi sắp xếp độ dài, hãy$M=l_n$. 

Chúng tôi tách cạnh lớn nhất khỏi các cạnh khác và thực hiện DP. DP phân vùng các cạnh còn lại thành ba nhóm: 

Đầu tiên, các cạnh được đặt ở một phía của đỉnh đính kèm đã chọn. 

Thứ hai, các cạnh được đặt ở phía bên kia. 

Thứ ba, các cạnh không được sử dụng trên đường kính. 

Nếu hai cạnh có độ dài$x$Và$y$, thì chiều dài đường kính thu được là$x+y$. 

Cạnh lớn nhất không được sử dụng có chiều dài$M$. Để gắn nó một cách an toàn, đỉnh đính kèm phải cách nhau ít nhất$M$từ cả hai đầu của đường kính. Điều đó có nghĩa$$x \ge M,\qquad y \ge M.$$Trường hợp đặc biệt duy nhất xảy ra khi cạnh lớn nhất nằm trên đường kính. Sau đó chúng ta chỉ cần tạo thành chiều dài còn lại$d-M$sử dụng các cạnh khác. 

Trạng thái DP đương nhiên trở thành tập hợp con hai chiều. Bởi vì$d \le 2000$, việc triển khai bitset làm cho nó đủ nhanh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n)$|$O(n)$| Quá chậm | 
| Bitset tối ưu DP |$O(nd^2/64)$|$O(d^2/64)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp tất cả các độ dài cạnh. 
2. Hãy để$M$có chiều dài lớn nhất. 
3. Nếu tổng của hai độ dài lớn nhất vượt quá$d$, đầu ra`No`. 

Bất kỳ cây nào nhất thiết phải có đường kính lớn hơn$d$. 
4. Bỏ qua$M$tạm thời và xử lý cái khác$n-1$độ dài bằng DP. 
5. Xác định`dp[a][b]`như liệu chúng ta có thể phân phối các cạnh đã được xử lý sao cho một cạnh của đường kính tương lai có tổng chiều dài hay không$a$và phía bên kia có tổng chiều dài$b$. 

Mỗi cạnh có ba lựa chọn: 

Nó có thể được thêm vào mặt đầu tiên. 

Nó có thể được thêm vào bên thứ hai. 

Nó có thể không được sử dụng. 
6. Lưu trữ chiều thứ hai dưới dạng bitset. 

Nếu một cạnh có độ dài$w$, các chuyển tiếp là: 

Đặt nó ở phía thứ hai:$$dp[a] \gets dp[a] \;|\; (dp[a] \ll w)$$Đặt nó ở phía đầu tiên:$$dp[a] \gets dp[a] \;|\; dp[a-w]$$7. Sau khi xử lý tất cả các cạnh ngoại trừ$M$, kiểm tra hai khả năng. 
8. Khả năng thứ nhất:$M$thuộc đường kính. 

Khi đó các cạnh còn lại phải tạo thành chiều dài$d-M$. 

Kiểm tra xem$$dp[d-M][0]$$có thể truy cập được. 
9. Khả năng thứ hai:$M$không thuộc đường kính. 

Sau đó chúng ta cần một đường kính có chiều dài$d$chia xung quanh một số đỉnh đính kèm. 

Kiểm tra xem có tồn tại không$$x+y=d$$với$$x\ge M,\qquad y\ge M,$$Và`dp[x][y]`có thể truy cập được. 
10. Nếu một trong hai điều kiện thành công, xuất ra`Yes`; nếu không thì xuất ra`No`. 

### Tại sao nó hoạt động 

Mỗi cây hợp lệ có thể được chuyển đổi thành cây trong đó tất cả các cạnh không có đường kính được gắn vào một đỉnh duy nhất trên đường kính. Chỉ cạnh không có đường kính lớn nhất mới có thể tạo ra một đường dẫn mới dài hơn, do đó chỉ cần xác minh rằng cạnh này có thể được gắn một cách an toàn là đủ. 

Nếu cạnh lớn nhất nằm trên đường kính thì ta chỉ cần điền chiều dài đường kính còn lại$d-M$. 

Nếu nó không nằm trên đường kính thì đỉnh đính kèm phải cách nhau ít nhất$M$từ cả hai đầu của đường kính. Viết những khoảng cách đó dưới dạng$x$Và$y$, ta thu được điều kiện$x+y=d$,$x\ge M$, Và$y\ge M$. 

DP liệt kê chính xác tất cả các cặp có thể$(x,y)$có thể đạt được từ các cạnh còn lại. Do đó, thuật toán chấp nhận chính xác khi tồn tại một trong các cấu trúc cần và đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        n, d = map(int, input().split())
        a = list(map(int, input().split()))
        a.sort()

        if a[-1] + a[-2] > d:
            print("No")
            continue

        dp = [0] * (d + 1)
        dp[0] = 1  # bit 0 set

        for w in a[:-1]:
            ndp = dp[:]

            for x in range(d, -1, -1):
                cur = dp[x]
                if cur == 0:
                    continue

                # put w on second side
                ndp[x] |= cur << w

                # put w on first side
                if x + w <= d:
                    ndp[x + w] |= cur

            mask = (1 << (d + 1)) - 1
            for i in range(d + 1):
                ndp[i] &= mask

            dp = ndp

        m = a[-1]

        ok = False

        if d - m >= 0:
            ok |= ((dp[d - m] >> 0) & 1) != 0

        for x in range(m, d - m + 1):
            if ((dp[x] >> (d - x)) & 1):
                ok = True
                break

        print("Yes" if ok else "No")

if __name__ == "__main__":
    solve()
```Mảng được sắp xếp đầu tiên vì cạnh lớn nhất đóng vai trò đặc biệt. 

Sự từ chối sớm`a[-1] + a[-2] > d`loại bỏ ngay những trường hợp không thể thực hiện được. 

DP sử dụng một loạt các bitset. chỉ mục`x`lưu trữ tất cả các giá trị có thể truy cập của`y`. Mỗi cạnh có thể được bỏ qua, thêm vào cạnh thứ nhất hoặc thêm vào cạnh thứ hai. Lựa chọn "bỏ qua" được xử lý tự động vì chúng tôi bắt đầu từ bản sao của DP trước đó. 

Việc dịch chuyển bit thực hiện thêm một cạnh vào cạnh thứ hai. Di chuyển từ`x`ĐẾN`x+w`thực hiện thêm nó vào phía đầu tiên. 

Kiểm tra cuối cùng khớp chính xác với hai trường hợp mang tính xây dựng đã được chứng minh ở trên. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 10
1 2 3 4
```Sau khi sắp xếp:```
[1, 2, 3, 4]
```Đây$M=4$. 

| Cạnh đã được xử lý | Số tiền phụ có thể tiếp cận | 
| --- | --- | 
| bắt đầu | (0,0) | 
| 1 | (0,0), (1,0), (0,1) | 
| 2 | nhiều cặp bao gồm (3,0), (1,2), (0,3) | 
| 3 | nhiều cặp bao gồm (6,0), (3,3), (4,2) | 

Chúng tôi kiểm tra xem có tồn tại$x+y=10$ít nhất là với cả hai bên$4$. 

Cặp đôi$(4,6)$có thể truy cập được, vì vậy câu trả lời là`Yes`. 

Điều này chứng tỏ trường hợp cạnh lớn nhất nằm ngoài đường kính. 

### Ví dụ 2 

đầu vào:```
4 7
1 4 3 4
```Đã sắp xếp:```
[1, 3, 4, 4]
```Hai cạnh lớn nhất thỏa mãn$$4+4=8>7.$$| Lớn nhất | Lớn thứ hai | Kiểm tra | 
| --- | --- | --- | 
| 4 | 4 | 8 > 7 | 

Thuật toán xuất ra ngay lập tức`No`. 

Điều này thể hiện điều kiện cần thiết mạnh nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(nd^2 / 64)$| Chuyển tiếp Bitset DP | 
| Không gian |$O(d^2 / 64)$| Bảng bitset DP | 

Từ$d \le 2000$và tổng số tiền của tất cả$n$nhiều nhất cũng là$2000$, bitset DP dễ dàng phù hợp với giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    data = io.StringIO(inp)
    out = io.StringIO()

    input = data.readline

    t = int(input())

    for _ in range(t):
        n, d = map(int, input().split())
        a = list(map(int, input().split()))
        a.sort()

        if a[-1] + a[-2] > d:
            out.write("No\n")
            continue

        dp = [0] * (d + 1)
        dp[0] = 1

        for w in a[:-1]:
            ndp = dp[:]

            for x in range(d, -1, -1):
                cur = dp[x]
                if cur == 0:
                    continue

                ndp[x] |= cur << w

                if x + w <= d:
                    ndp[x + w] |= cur

            mask = (1 << (d + 1)) - 1
            for i in range(d + 1):
                ndp[i] &= mask

            dp = ndp

        m = a[-1]
        ok = False

        if d - m >= 0:
            ok |= ((dp[d - m] >> 0) & 1) != 0

        for x in range(m, d - m + 1):
            if ((dp[x] >> (d - x)) & 1):
                ok = True
                break

        out.write("Yes\n" if ok else "No\n")

    return out.getvalue()

# provided sample
assert run(
"""3
4 10
1 2 3 4
4 7
1 4 3 4
6 18
2 4 3 7 6 7
"""
) == """Yes
No
Yes
"""

# minimum size
assert run(
"""1
2 5
2 3
"""
) == """Yes
"""

# impossible because two largest exceed d
assert run(
"""1
2 5
3 3
"""
) == """No
"""

# all equal
assert run(
"""1
4 8
2 2 2 2
"""
) == """Yes
"""

# boundary condition
assert run(
"""1
4 10
4 4 4 4
"""
) == """No
"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 5 / 2 3`| Có | Cây hợp lệ không tầm thường nhỏ nhất | 
|`2 5 / 3 3`| Không | Đường kính đã vượt mục tiêu | 
|`2 2 2 2 , d=8`| Có | Tất cả các độ dài bằng nhau | 
|`4 4 4 4 , d=10`| Không | Vấn đề hạn chế về tệp đính kèm | 

## Vỏ cạnh 

Hãy xem xét:```
1
2 5
3 3
```Thuật toán đầu tiên kiểm tra hai độ dài lớn nhất.$$3+3=6>5$$và ngay lập tức quay trở lại`No`. 

Điều này tránh việc chấp nhận sai chỉ dựa trên lý luận tổng tập hợp con. 

Coi như:```
1
3 10
6 2 2
```Sau khi sắp xếp,$M=6$. DP trên các cạnh còn lại tìm thấy một tập hợp con tổng bằng$d-M=4$. Thuật toán chấp nhận thông qua trường hợp "cạnh lớn nhất nằm trên đường kính" và xuất ra`Yes`. 

Coi như:```
1
4 10
4 4 4 4
```DP có thể tạo ra nhiều phân tách đường kính tổng cộng thành$10$, nhưng không thỏa mãn yêu cầu rằng cả hai cạnh xung quanh đỉnh đính kèm ít nhất$4$. Xác nhận cuối cùng thất bại và câu trả lời là`No`. 

Những ví dụ này minh họa tại sao cả điều kiện tổng đường kính và hình dạng đính kèm phải được kiểm tra đồng thời.
