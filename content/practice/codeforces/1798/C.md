---
title: "CF 1798C - Cửa Hàng Kẹo"
description: "Mỗi loại kẹo có hai con số gắn liền với nó. Có những chiếc kẹo $ai$ thuộc loại đó trong kho và mỗi chiếc kẹo có giá $2$ xu. Đối với mỗi loại, chúng ta phải chọn kích thước gói $di$."
date: "2026-06-09T09:53:34+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1798
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 860 (Div. 2)"
rating: 1700
weight: 1798
solve_time_s: 272
verified: true
draft: false
---

[CF 1798C - Cửa hàng kẹo](https://codeforces.com/problemset/problem/1798/C) 

**Đánh giá:** 1700 
**Tags:** tham lam, toán học, lý thuyết số 
**Thời gian giải:** 4 phút 32 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi loại kẹo có hai con số gắn liền với nó. có$a_i$loại kẹo đó còn trong kho và giá mỗi viên kẹo$b_i$tiền xu. 

Đối với mỗi loại, chúng ta phải chọn kích thước gói$d_i$. Hạn chế duy nhất là toàn bộ hàng tồn kho phải được chia thành các gói bằng nhau, do đó$d_i$phải chia$a_i$. 

Giá của một gói trở thành$$c_i = b_i \cdot d_i.$$Sau khi chọn tất cả các kích cỡ gói, chúng tôi nhận được một chuỗi giá gói$$c_1,c_2,\dots,c_n.$$Một thẻ giá duy nhất có thể bao gồm một khối liền kề nếu mọi gói trong khối đó có cùng mức giá. Mục tiêu là chọn kích thước gói sao cho số lượng khối liền kề có giá bằng nhau được yêu cầu càng nhỏ càng tốt. 

Độ dài của tất cả các trường hợp thử nghiệm kết hợp tối đa là$2 \cdot 10^5$. Điều đó ngay lập tức loại trừ bất kỳ phương pháp nào cố gắng tìm kiếm các ước số hoặc thực hiện các kiểm tra theo cặp tốn kém trong mỗi phân đoạn. Chúng ta cần một cái gì đó gần tuyến tính hoặc$O(n \log C)$, Ở đâu$C$là kích thước của các con số liên quan. 

Phần tinh tế là chúng tôi không trực tiếp chọn giá gói cuối cùng. Mỗi giá phải có hình thức$b_i \cdot d_i$, Và$d_i$phải chia$a_i$. 

Một sai lầm phổ biến là chỉ nghĩ đến khả năng tương thích theo cặp. Ví dụ:```
(a,b):
(6,2)
(10,3)
(15,5)
```Hai loại đầu tiên có thể có chung một mức giá gói hợp lệ và hai loại cuối cùng có thể có chung một mức giá khác, nhưng có thể không có một mức giá nào phù hợp cho cả ba loại cùng một lúc. Điều kiện phải được kiểm tra cho toàn bộ phân đoạn cùng một lúc. 

Một sai lầm dễ mắc phải khác là làm việc với các ước số$d_i$trực tiếp. Ràng buộc thực tế là về giá gói chung$X$, không phải về việc tạo ra$d_i$bình đẳng. 

Ví dụ:```
a=6, b=2
a=10, b=3
```Giá chung$X=6$hoạt động vì$$d_1 = 6/2 = 3,\qquad d_2 = 6/3 = 2.$$Kích thước gói khác nhau, nhưng giá gói phù hợp. 

## Phương pháp tiếp cận 

Một quan điểm mạnh mẽ là xem xét mọi phân khúc có thể có và hỏi xem liệu tất cả các loại kẹo trong phân khúc đó có thể chia sẻ một mức giá gói chung hay không. Nếu một phân khúc hợp lệ thì nó có thể được biểu thị bằng một thẻ giá. 

Vấn đề là ngay cả việc kiểm tra một phân đoạn một cách ngây thơ cũng tốn kém. Mỗi loại kẹo đều có những ràng buộc về tính chia hết và có$O(n^2)$phân đoạn. Với$n$lên tới$2 \cdot 10^5$, điều này hoàn toàn không thể thực hiện được. 

Nhận xét quan trọng đến từ việc viết lại điều kiện cho một mức giá chung của gói. 

Giả sử một phân khúc sử dụng một mức giá chung$X$. 

Từ$$X = b_i \cdot d_i,$$chúng tôi biết$X$phải chia hết cho$b_i$. 

Cũng,$$d_i = \frac{X}{b_i}$$phải chia$a_i$. Nhân cả hai vế với$b_i$,$$X \mid (a_i b_i).$$Vì vậy, mỗi loại kẹo đều áp đặt hai điều kiện:$$b_i \mid X,$$Và$$X \mid a_i b_i.$$Đối với toàn bộ phân khúc,$X$phải là: 

1. bội số của mọi$b_i$. 
2. Ước của mọi$a_i b_i$. 

Số nhỏ nhất thỏa mãn điều kiện đầu tiên là$$L = \operatorname{lcm}(b_i).$$Số lớn nhất thỏa mãn điều kiện thứ hai là$$G = \gcd(a_i b_i).$$hợp lệ$X$tồn tại chính xác khi một số bội số của$L$chia rẽ$G$. Điều đó xảy ra chính xác khi$$L \mid G.$$Điều này biến vấn đề thành việc duy trì, cho một phân khúc đang phát triển:$$L = \operatorname{lcm}(b_i),$$Và$$G = \gcd(a_i b_i).$$Một phân đoạn hợp lệ khi và chỉ khi$$G \bmod L = 0.$$Bây giờ giải pháp trở nên tham lam. Mở rộng phân đoạn hiện tại miễn là điều kiện vẫn đúng. Thời điểm nó trở thành sai, không có phân đoạn nào lớn hơn bắt đầu ở cùng một vị trí có thể trở lại hợp lệ, bởi vì việc thêm nhiều phần tử chỉ có thể làm giảm gcd và tăng lcm. Chúng ta phải bắt đầu một phân khúc mới. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$hoặc tệ hơn |$O(1)$| Quá chậm | 
| Tham lam tối ưu + GCD/LCM |$O(n \log C)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu một phân đoạn mới với loại kẹo đầu tiên. 
2. Duy trì hai giá trị cho phân đoạn hiện tại:$$G = \gcd(a_i b_i)$$trên phân khúc và$$L = \operatorname{lcm}(b_i)$$trên phân khúc. 
3. Khi có loại kẹo mới, hãy tính:$$G'=\gcd(G, a_i b_i),$$

$$L'=\operatorname{lcm}(L,b_i).$$4. Kiểm tra xem$$G' \bmod L' = 0.$$Nếu điều này đúng thì phân đoạn mở rộng vẫn khả thi, vì vậy hãy thay thế$G,L$qua$G',L'$. 
5. Nếu điều kiện không thành công thì không có giá gói chung cho phân khúc mở rộng. Kết thúc phân đoạn hiện tại, tăng câu trả lời và bắt đầu phân đoạn mới chỉ chứa loại kẹo hiện tại. 
6. Tiếp tục cho đến khi tất cả các loại kẹo được xử lý. 
7. Số lượng phân khúc được hình thành bởi quá trình tham lam này là số lượng thẻ giá tối thiểu được yêu cầu. 

### Tại sao nó hoạt động 

Đối với một phân khúc, giá gói chung tồn tại khi và chỉ khi lcm của phân khúc đó$b_i$chia gcd của phân khúc$a_i b_i$. 

Khi chúng ta mở rộng một đoạn, gcd chỉ có thể giữ nguyên hoặc giảm đi, trong khi lcm chỉ có thể giữ nguyên hoặc tăng lên. Một khi điều kiện chia hết không thành công thì việc thêm nhiều phần tử hơn sẽ không thể khắc phục được. Phía bên trái chỉ có thể trở nên lớn hơn và phía bên phải chỉ có thể trở nên nhỏ hơn. 

Sự đơn điệu đó làm cho chiến lược tham lam trở nên tối ưu. Mỗi phân khúc được mở rộng càng xa càng tốt. Bất kỳ giải pháp nào cũng phải bắt đầu một thẻ giá mới ở vị trí đầu tiên mà điều kiện trở nên không thể thực hiện được, do đó, phân vùng tham lam sử dụng số lượng phân đoạn tối thiểu có thể. 

## Giải pháp Python```python
import sys
from math import gcd

input = sys.stdin.readline

def lcm(a, b):
    return a // gcd(a, b) * b

t = int(input())

for _ in range(t):
    n = int(input())

    ans = 1
    g = 0
    l = 1

    for _ in range(n):
        a, b = map(int, input().split())

        ng = gcd(g, a * b) if g else a * b
        nl = lcm(l, b)

        if ng % nl == 0:
            g = ng
            l = nl
        else:
            ans += 1
            g = a * b
            l = b

    print(ans)
```Việc thực hiện phản ánh trực tiếp điều kiện toán học. 

Biến`g`lưu trữ gc của tất cả các giá trị$a_i b_i$trong phân khúc hiện tại. Biến`l`lưu trữ lcm của tất cả$b_i$các giá trị trong phân đoạn hiện tại. 

Đối với mỗi loại kẹo mới, chúng tôi dự kiến ​​mở rộng phân khúc. Nếu điều kiện chia hết vẫn giữ nguyên thì ta giữ phần mở rộng. Nếu không, chúng tôi sẽ bắt đầu một phân đoạn mới bắt đầu với loại kẹo hiện tại. 

Một chi tiết triển khai là`g`được khởi tạo bằng 0. Từ$$\gcd(0,x)=x,$$điều này cho phép phần tử đầu tiên khởi tạo phân đoạn một cách tự nhiên. 

Một chi tiết khác là tính toán lcm:```
a // gcd(a, b) * b
```Phép chia được thực hiện trước khi nhân để tránh sự tăng trưởng không cần thiết của các giá trị trung gian. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Phân đoạn đầu vào:```
(20,3)
(6,2)
(14,5)
(20,7)
```| Bước | a·b | G hiện tại | L hiện tại | Có hiệu lực? | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 60 | 60 | 3 | Có | 
| Cộng (6,2) | 12 | gcd(60,12)=12 | lcm(3,2)=6 | 12 % 6 = 0 | 
| Cộng (14,5) | 70 | gcd(12,70)=2 | lcm(6,5)=30 | 2 % 30 ≠ 0 | 
| Phân khúc mới | 70 | 70 | 5 | Có | 
| Cộng (20,7) | 140 | gcd(70,140)=70 | lcm(5,7)=35 | 70 % 35 = 0 | 

Mảng chia thành hai đoạn nên câu trả lời là 2. 

Ví dụ này cho thấy lý do tại sao thuật toán cắt chính xác khi điều kiện chia hết bị phá vỡ. 

### Ví dụ 2 

Phân đoạn đầu vào:```
(444,5)
(2002,10)
(2020,2)
```| Bước | a·b | G hiện tại | L hiện tại | Có hiệu lực? | 
| --- | --- | --- | --- | --- | 
| Bắt đầu | 2220 | 2220 | 5 | Có | 
| Thêm | 20020 | 20 | 10 | 20% 10 = 0 | 
| Thêm | 4040 | 20 | 10 | 20% 10 = 0 | 

Toàn bộ mảng vẫn là một phân đoạn hợp lệ, do đó chỉ cần một thẻ giá. 

Ví dụ này chứng tỏ rằng giá chung không cần phải được xây dựng một cách rõ ràng. Chỉ cần điều kiện gcd/lcm là đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log C)$| Mỗi bước thực hiện một số thao tác gcd không đổi | 
| Không gian |$O(1)$| Chỉ có một số số nguyên được duy trì | 

Đây$C$là độ lớn của các số được lưu trữ. Vì tổng số loại kẹo trong tất cả các trường hợp thử nghiệm nhiều nhất là$2 \cdot 10^5$, sự phức tạp này dễ dàng phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io
from math import gcd

def solve():
    input = sys.stdin.readline

    def lcm(a, b):
        return a // gcd(a, b) * b

    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())

        ans = 1
        g = 0
        l = 1

        for _ in range(n):
            a, b = map(int, input().split())

            ng = gcd(g, a * b) if g else a * b
            nl = lcm(l, b)

            if ng % nl == 0:
                g = ng
                l = nl
            else:
                ans += 1
                g = a * b
                l = b

        out.append(str(ans))

    sys.stdout.write("\n".join(out))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return out.strip()

# provided sample
assert run(
"""5
4
20 3
6 2
14 5
20 7
3
444 5
2002 10
2020 2
5
7 7
6 5
15 2
10 3
7 7
5
10 1
11 5
5 1
2 2
8 2
6
7 12
12 3
5 3
9 12
9 3
1000000000 10000
"""
) == """2
1
3
2
5"""

# minimum size
assert run(
"""1
2
1 1
1 1
"""
) == "1"

# every item forced into its own segment
assert run(
"""1
3
2 2
3 3
5 5
"""
) == "3"

# all can stay together
assert run(
"""1
3
10 2
15 2
20 2
"""
) == "1"

# boundary style case with large values
assert run(
"""1
2
1000000000 10000
1000000000 10000
"""
) == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hai loại giống hệt nhau | 1 | Phân đoạn hợp lệ có kích thước tối thiểu | 
|`(2,2),(3,3),(5,5)`| 3 | Mọi tiện ích mở rộng đều thất bại ngay lập tức | 
| Ba loại giống nhau`b`| 1 | Toàn bộ mảng tạo thành một đoạn | 
| Trường hợp kiểu giá trị tối đa | 1 | Xử lý gcd/lcm số lượng lớn | 

## Vỏ cạnh 

Hãy xem xét một phân đoạn bao gồm toàn bộ một loại kẹo:```
1
2
10 7
20 3
```Loại kẹo đầu tiên luôn tự tạo thành một phân đoạn hợp lệ vì việc chọn$d_1=1$cung cấp một giá gói hợp lệ. Thuật toán bắt đầu với`G=a_1b_1`Và`L=b_1`, và rõ ràng`G % L = 0`. 

Bây giờ hãy xem xét trường hợp lcm phát triển quá nhanh:```
1
3
2 2
3 3
5 5
```Sau hai loại đầu tiên,$$G=\gcd(4,9)=1,$$

$$L=\operatorname{lcm}(2,3)=6.$$Từ$1 \not\equiv 0 \pmod 6$, phân đoạn phải được phân chia. Bất kỳ phân đoạn nào lớn hơn sẽ chỉ làm giảm gcd hơn nữa và tăng lcm hơn nữa, do đó lỗi sẽ xảy ra vĩnh viễn. 

Cuối cùng, hãy xem xét trường hợp tồn tại giá chung mặc dù kích thước gói hàng khác nhau:```
1
2
6 2
10 3
```chúng tôi có$$G=\gcd(12,30)=6,$$

$$L=\operatorname{lcm}(2,3)=6.$$Bởi vì$6 \bmod 6 = 0$, phân đoạn là hợp lệ. Giá trọn gói thông thường là$X=6$, đưa ra kích thước gói$3$Và$2$. Điều này xác nhận rằng sự bình đẳng về kích thước gói là không phù hợp; chỉ có sự bình đẳng về giá gói mới quan trọng.
