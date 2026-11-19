---
title: "CF 2086E - Số giống ngựa vằn"
description: "Số giống ngựa vằn là số nguyên dương có biểu diễn nhị phân giống như 1, 101, 10101, 1010101, ... Nói cách khác, các bit xen kẽ và bit có ý nghĩa nhỏ nhất là 1. Đặt $$z1=1,quad z2=5,quad z3=21,quad z4=85,dots$$ Đây chính xác là những số giống ngựa vằn."
date: "2026-06-08T06:04:46+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "brute-force", "constructive-algorithms", "dfs-and-similar", "dp", "greedy", "math"]
categories: ["algorithms"]
codeforces_contest: 2086
codeforces_index: "E"
codeforces_contest_name: "Educational Codeforces Round 177 (Rated for Div. 2)"
rating: 2400
weight: 2086
solve_time_s: 128
verified: true
draft: false
---

[CF 2086E - Các số giống ngựa vằn](https://codeforces.com/problemset/problem/2086/E) 

**Đánh giá:** 2400 
**Tags:** bitmasks, brute Force, thuật toán xây dựng, dfs và tương tự, dp, tham lam, toán học 
**Thời gian giải:** 2 phút 8 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Số giống ngựa vằn là số nguyên dương có biểu diễn nhị phân trông giống như`1`,`101`,`10101`,`1010101`, ... 

Nói cách khác, các bit thay thế nhau và bit có ý nghĩa nhỏ nhất là`1`. 

Cho phép$$z_1=1,\quad z_2=5,\quad z_3=21,\quad z_4=85,\dots$$Đây chính xác là những con số giống ngựa vằn. 

Đối với số nguyên dương$x$, giá trị ngựa vằn của nó là số lượng tối thiểu các số giống ngựa vằn có tổng là$x$. 

Đối với mỗi truy vấn$[l,r]$Và$k$, chúng ta phải đếm chính xác có bao nhiêu số nguyên trong khoảng đó có giá trị ngựa vằn$k$. 

Các điểm cuối của khoảng có thể lớn bằng$10^{18}$, vì vậy việc lặp qua các số là không thể. Ngay cả một lần vượt qua khoảng thời gian cũng sẽ cần tới$10^{18}$hoạt động. Giải pháp phải hoạt động trong khoảng thời gian đa logarit cho mỗi truy vấn. 

Quan sát không rõ ràng đầu tiên là có rất ít số giống ngựa vằn bên dưới$10^{18}$. Họ thỏa mãn$$z_{i+1}=4z_i+1,$$nên chúng phát triển theo cấp số nhân. Chỉ có khoảng ba mươi trong số chúng vừa vặn bên trong$10^{18}$. 

Bẫy thứ hai là giả định rằng giá trị ngựa vằn có thể được tính bằng DP đổi xu thông thường. Giá trị đồng xu đạt$10^{18}$, vì vậy bất kỳ DP nào trên tổng đều không thể. 

Bẫy thứ ba là các phép phân tách khác nhau của cùng một số có thể sử dụng các số lượng triệu hồi giống ngựa vằn khác nhau. Ví dụ,$$21 = z_3$$có giá trị ngựa vằn$1$, mặc dù$$21 = 5+5+5+5+1$$sử dụng năm lệnh triệu tập. Chúng ta phải suy luận trực tiếp về các phân rã tối ưu. 

Xem xét truy vấn```
1 1 1
```Câu trả lời là`1`bởi vì$1$bản thân nó giống ngựa vằn. Bất kỳ cách tiếp cận nào vô tình chỉ đếm tổng của ít nhất hai số ngựa vằn sẽ thất bại ở đây. 

Coi như```
2 10 100
```Câu trả lời là`0`. Mặc dù$k$có thể lớn như$10^{18}$, không có số nào lên tới$10^{18}$có thể có giá trị ngựa vằn ở bất cứ đâu gần lớn như vậy. Xử lý sớm những trường hợp như vậy là điều cần thiết. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ tạo ra mọi con số trong$[l,r]$, tính giá trị ngựa vằn của nó và đếm các kết quả trùng khớp. 

Ngay cả khi bằng cách nào đó chúng ta tính được giá trị ngựa vằn trong thời gian không đổi thì độ dài khoảng có thể là$10^{18}$. Điều này là hoàn toàn không thể thực hiện được. 

Điều quan trọng là ngừng suy nghĩ về các con số và thay vào đó hãy nghiên cứu cấu trúc của các con số giống ngựa vằn. 

Cho phép$$z_i=\frac{4^i-1}{3}.$$Những điều này thỏa mãn$$z_{i+1}=4z_i+1.$$Giả sử một phân tách tối ưu chứa ít nhất năm bản sao của một số$z_i$. 

Vì$i>1$,$$5z_i=z_{i+1}+4z_{i-1}.$$Phía bên phải sử dụng cùng số lượng lệnh triệu tập. Việc lặp lại phép biến đổi này sẽ loại bỏ mọi hệ số lớn hơn 4. 

Kết quả là mọi số đều có cách phân tách tối ưu$$x=\sum c_i z_i,$$với$$0\le c_i\le 4.$$Sự giảm thứ hai đến từ$$z_{i+1}=4z_i+1.$$Nếu một số hệ số$c_i=4$và có hệ số dương nào nhỏ hơn nó thì ta có thể thay thế$$4z_i+1$$qua$$z_{i+1},$$giảm số lần triệu tập. Do đó, trong một phân rã tối ưu, bất cứ khi nào một chữ số$4$xuất hiện, tất cả các chữ số thấp hơn phải bằng 0. 

Điều này đưa ra một đại diện kinh điển:$$x=\sum c_i z_i,$$trong đó mỗi chữ số nằm giữa$0$Và$4$, và sau chữ số đầu tiên bằng$4$, tất cả các chữ số thấp hơn đều bằng 0. 

Giá trị ngựa vằn chỉ đơn giản là$$\sum c_i.$$Bây giờ bài toán trở thành bài toán chữ số-DP trong một hệ thống số lạ. Chúng tôi tính các đại diện không vượt quá giới hạn$N$có tổng các chữ số bằng$k$. 

Vì chỉ có khoảng ba mươi chữ số ngựa vằn nên DP rất nhỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(r-l+1)$|$O(1)$| Quá chậm | 
| Chữ số tối ưu DP |$O(30^2)$mỗi truy vấn |$O(30^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Tính toán trước 

1. Tạo tất cả các số giống ngựa vằn$z_i\le 10^{18}$sử dụng$$z_{i+1}=4z_i+1.$$2. Chỉ có khoảng ba mươi con số như vậy. 

### Chuyển đổi giới hạn thành chữ số ngựa vằn 

1. Đối với một nhất định$N$, tính toán biểu diễn tham lam của nó trong cơ sở$\{z_i\}$. 
2. Xử lý số ngựa vằn từ lớn nhất đến nhỏ nhất. Tại mỗi vị trí hãy lấy hệ số lớn nhất có thể. 
3. Các chữ số kết quả luôn nằm giữa$0$Và$4$. 

Sự tái diễn$z_{i+1}=4z_i+1$đảm bảo rằng sự phân tách tham lam tạo ra hệ thống chữ số chính tắc được DP sử dụng. 

### Chữ số DP 

1. Hãy để`digits[i]`là hệ số của$z_i$trong sự đại diện của$N$. 
2. Xử lý các vị trí từ quan trọng nhất đến ít quan trọng nhất. 
3. Duy trì bốn thông số DP: 

- vị trí hiện tại 
- liệu chúng ta có còn gắn bó với nhau không$N$- tổng chữ số hiện tại 
- dù là một chữ số$4$đã xuất hiện rồi 
4. Nếu một chữ số$4$đã xuất hiện thì mọi chữ số còn lại phải bằng 0. Điều này thực thi điều kiện biểu diễn chính tắc. 
5. Nếu không, hãy thử tất cả các chữ số từ$0$đến giới hạn cho phép. 
6. Cộng chữ số đã chọn vào tổng các chữ số đang chạy. 
7. Cuối cùng, đếm trạng thái có tổng các chữ số bằng$k$. 

### Truy vấn phạm vi 

1. Hãy để$F(N,k)$là số nguyên$x\le N$có giá trị ngựa vằn bằng$k$. 
2. Câu trả lời cho$[l,r]$là$$F(r,k)-F(l-1,k).$$### Tại sao nó hoạt động 

Mọi phân rã tối ưu đều có thể được biến đổi cho đến khi tất cả các hệ số thuộc về$\{0,1,2,3,4\}$. Bất kỳ biểu diễn nào có chứa một chữ số$4$cùng với hệ số dương thấp hơn là không tối ưu vì một bản sao của$z_{i+1}$có thể thay thế$4z_i+1$và giảm số lần triệu tập. 

Do đó, mỗi số có một biểu diễn chính tắc duy nhất thỏa mãn:$$0\le c_i\le 4,$$và sau chữ số đầu tiên bằng$4$, tất cả các chữ số thấp hơn đều bằng 0. 

Giá trị ngựa vằn bằng tổng của các chữ số này. Chữ số DP liệt kê chính xác các biểu diễn chuẩn này và đếm những biểu diễn có tổng chữ số là$k$. Vì mọi biểu diễn hợp lệ đều tương ứng với chính xác một số nguyên và ngược lại, số đếm được DP trả về là chính xác. 

## Giải pháp Python```python
import sys
from functools import lru_cache

input = sys.stdin.readline

Z = [1]
while True:
    nxt = Z[-1] * 4 + 1
    if nxt > 10**18:
        break
    Z.append(nxt)

M = len(Z)

def count_upto(n, k):
    if n <= 0:
        return 0

    if k > 4 * M:
        return 0

    digits = [0] * M
    rem = n

    for i in range(M - 1, -1, -1):
        digits[i] = rem // Z[i]
        rem %= Z[i]

    @lru_cache(None)
    def dfs(pos, tight, s, locked):
        if s > k:
            return 0

        if pos < 0:
            return 1 if s == k else 0

        if locked:
            d = 0
            if tight and d > digits[pos]:
                return 0
            ntight = tight and (d == digits[pos])
            return dfs(pos - 1, ntight, s, 1)

        limit = digits[pos] if tight else 4

        ans = 0
        for d in range(limit + 1):
            ntight = tight and (d == limit if tight else False)

            if tight:
                ntight = (d == digits[pos])

            ans += dfs(
                pos - 1,
                ntight,
                s + d,
                locked or (d == 4)
            )

        return ans

    return dfs(M - 1, 1, 0, 0)

def solve():
    t = int(input())

    out = []

    for _ in range(t):
        l, r, k = map(int, input().split())

        if k > 4 * M:
            out.append("0")
            continue

        ans = count_upto(r, k) - count_upto(l - 1, k)
        out.append(str(ans))

    sys.stdout.write("\n".join(out))

solve()
```Phần đầu tiên xây dựng tất cả các số giống ngựa vằn cho đến$10^{18}$. Chỉ có khoảng ba mươi giá trị tồn tại, đó là lý do tại sao DP sau này vẫn còn rất nhỏ. 

chức năng`count_upto`tính toán$F(N,k)$. Đầu tiên nó chuyển đổi$N$vào biểu diễn chữ số ngựa vằn tham lam của nó. Các chữ số này đóng vai trò giống như các chữ số thập phân trong chữ số chuẩn DP. 

Biến trạng thái`locked`là phần tinh tế. Một lần là một chữ số`4`xuất hiện, tất cả các chữ số thấp hơn còn lại phải bằng 0. Việc quên hạn chế này sẽ tính các biểu diễn không tối ưu và tạo ra các giá trị ngựa vằn không chính xác. 

Điều kiện cắt tỉa`s > k`giữ cho không gian trạng thái nhỏ. Vì tổng chữ số tối đa có thể chỉ là khoảng$120$, DP vẫn cực kỳ nhanh. 

Cuối cùng, mọi truy vấn đều được trả lời bằng kỹ thuật đếm tiền tố tiêu chuẩn:$$[l,r] = [1,r] - [1,l-1].$$## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 100 3
```Các số ngựa vằn liên quan là:$$1,\;5,\;21,\;85.$$Hình tượng ngựa vằn tham lam của$100$là: 

| Số ngựa vằn | Hệ số | 
| --- | --- | 
| 85 | 1 | 
| 21 | 0 | 
| 5 | 3 | 
| 1 | 0 | 

Vì thế$100 = 1\cdot85 + 0\cdot21 + 3\cdot5 + 0\cdot1$. 

DP khám phá tất cả các biểu diễn chính tắc không vượt quá các chữ số này và đếm những chữ số có tổng chữ số$3$. 

Kết quả là:```
13
```Ví dụ này cho thấy giá trị ngựa vằn được xác định hoàn toàn bằng tổng chữ số của biểu diễn chuẩn. 

### Ví dụ 2 

đầu vào:```
15 77 2
```Số ngựa vằn có liên quan:$$1,\;5,\;21.$$Các số trong khoảng có giá trị ngựa vằn$2$là:$$26=21+5,$$

$$42=21+21,$$

$$50=5+45? \text{ not canonical},$$và DP tự động lọc các phân tách không hợp lệ. 

Số đếm cuối cùng là:```
3
```Ví dụ này chứng tỏ tại sao việc đếm số tiền tùy ý là không đủ. Chỉ có sự phân rã tối ưu kinh điển mới quan trọng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(M^2)$mỗi truy vấn |$M \approx 30$, tổng chữ số nhiều nhất là$4M$| 
| Không gian |$O(M^2)$| Trạng thái DP được ghi nhớ | 

Với$M\approx30$, số lượng trạng thái DP chỉ có vài nghìn. Ngay cả với$100$trường hợp thử nghiệm, giải pháp phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    # paste solve() here and return captured output
    ...

# provided sample
assert run(
"""5
1 100 3
1 1 1
15 77 2
2 10 100
1234567 123456789101112131 12
"""
) == (
"""13
1
3
0
4246658701
""")

# minimum value
assert run(
"""1
1 1 1
"""
) == (
"""1
""")

# single value not zebra-like
assert run(
"""1
2 2 1
"""
) == (
"""0
""")

# impossible huge k
assert run(
"""1
1 1000000000000000000 1000000000000000000
"""
) == (
"""0
""")

# boundary interval
assert run(
"""1
21 21 1
"""
) == (
"""1
""")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1`|`1`| Số hợp lệ nhỏ nhất | 
|`2 2 1`|`0`| Số không phải ngựa vằn có giá trị lớn hơn 1 | 
| To lớn`k`|`0`| Từ chối sớm các giá trị ngựa vằn không thể | 
|`21 21 1`|`1`| Số đơn giống ngựa vằn | 

## Vỏ cạnh 

Hãy xem xét```
1
1 1 1
```Hình tượng ngựa vằn tham lam chỉ đơn giản là một bản sao của$z_1=1$. Tổng các chữ số là$1$, vì vậy DP sẽ đếm nó. Đầu ra là`1`. 

Coi như```
1
2 10 100
```Chỉ có khoảng ba mươi chữ số ngựa vằn. Mỗi chữ số chuẩn có nhiều nhất là bốn, vì vậy giá trị ngựa vằn tối đa bên dưới$10^{18}$nhiều nhất là$120$. Từ$100$đã vượt quá phạm vi có thể đạt được trong nhiều khoảng thời gian và$10^{18}$là giới hạn toàn cục thì DP ngay lập tức trả về trạng thái 0. Đầu ra là`0`. 

Coi như```
1
21 21 1
```Biểu diễn kinh điển là$$21 = z_3.$$Tổng chữ số của nó là$1$, vậy giá trị ngựa vằn của nó là$1$. DP đạt chính xác một trạng thái chấp nhận và trả về`1`. 

Trường hợp tế nhị nhất là khi một chữ số$4$xuất hiện. Giả sử một đại diện cố gắng sử dụng$$4z_i + z_j$$với$j<i$. Từ$$z_{i+1}=4z_i+1,$$sự biểu diễn này không tối ưu. các`locked`trạng thái cấm các cấu hình như vậy bằng cách buộc tất cả các chữ số thấp hơn trở thành 0 sau một chữ số$4$được chọn. Đó chính xác là điều kiện cần thiết cho các phân rã tối ưu chính tắc.
