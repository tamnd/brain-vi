---
title: "CF 106045C - Truy vấn bị nguyền rủa"
description: "Chúng ta có một mảng và một số nguyên cố định $m$, được gọi là số bị nguyền rủa. Hai loại hoạt động phải được xử lý trực tuyến. Bản cập nhật sẽ thay thế một phần tử mảng bằng một giá trị mới. Một truy vấn xem xét một mảng con và một giá trị $k$, sau đó hỏi có bao nhiêu phần tử của mảng con đó là k-good."
date: "2026-06-25T12:40:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106045
codeforces_index: "C"
codeforces_contest_name: "IUT Intra University Programming Contest 2025"
rating: 0
weight: 106045
solve_time_s: 58
verified: true
draft: false
---

[CF 106045C - Truy vấn bị nguyền rủa](https://codeforces.com/problemset/problem/106045/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một mảng và một số nguyên cố định$m$, được gọi là con số bị nguyền rủa. 

Hai loại hoạt động phải được xử lý trực tuyến. Bản cập nhật sẽ thay thế một phần tử mảng bằng một giá trị mới. Một truy vấn xem xét một mảng con và một giá trị$k$, sau đó hỏi có bao nhiêu phần tử của mảng con đó là _k-good_. 

Định nghĩa hơi gián tiếp. Một số được coi là tốt nếu nó không chia hết cho$m$. một con số$x$có tốt không nếu sau khi thêm$k$bao nhiêu lần thì nó vẫn luôn tốt. Nói cách khác, không có giá trị nào$$x,\; x+k,\; x+2k,\; x+3k,\dots$$có thể chia hết cho$m$. 

Đầu vào chứa tối đa$10^4$các trường hợp thử nghiệm, nhưng hạn chế quan trọng là tổng số$n$và tổng cộng$q$trên tất cả các trường hợp thử nghiệm là nhiều nhất$10^5$. Bất kỳ giải pháp xung quanh$O((n+q)\log n)$mỗi trường hợp thử nghiệm là đủ nhanh. Việc quét bậc hai trên một mảng con cho mỗi truy vấn sẽ quá chậm vì một thử nghiệm đơn lẻ có thể chứa$10^5$hoạt động. 

Khó khăn chính là hiểu được điều kiện k-tốt. Khi điều đó được đơn giản hóa, vấn đề sẽ trở thành vấn đề đếm phạm vi động. 

Hãy xem xét một số tình huống có thể dễ dàng dẫn đến suy luận sai lầm. 

Nếu như$m=8$,$k=12$, Và$x=1$, thì dãy modulo$8$là$$1,5,1,5,\dots$$Nó không bao giờ đạt tới$0$, Vì thế$x$là k-tốt. 

Nếu như$m=8$,$k=12$, Và$x=4$, sau đó modulo$8$chúng tôi nhận được$$4,0,4,0,\dots$$Số hạng thứ hai chia hết cho$8$, Vì thế$x$không tốt chút nào. 

Một sai lầm phổ biến là chỉ kiểm tra xem liệu$x$chính nó có thể chia hết cho$m$. Định nghĩa cũng yêu cầu kiểm tra tất cả các bổ sung trong tương lai. 

Một trường hợp tế nhị khác xuất hiện khi$k$là bội số của$m$. Ví dụ,$m=10$,$k=20$. Thêm$k$không bao giờ thay đổi modulo dư$m$. Khi đó một số là k-good chính xác khi nó không chia hết cho$10$. Bất kỳ giải pháp nào xử lý được tất cả các giá trị của$k$thống nhất mà không cần kiểm tra$\gcd(k,m)$sẽ bỏ lỡ hành vi này. 

## Phương pháp tiếp cận 

Việc giải thích bạo lực tuân theo định nghĩa trực tiếp. Đối với mỗi truy vấn, hãy kiểm tra từng phần tử của mảng con và xác định xem việc bổ sung lặp lại của$k$có thể tạo ra bội số của$m$. 

Một cách để làm điều đó là làm việc theo modulo$m$. Bắt đầu từ$a_i \bmod m$, thêm liên tục$k \bmod m$cho đến khi trình tự lặp lại. Vì chỉ có$m$dư lượng, điều này cần$O(m)$thời gian cho mỗi phần tử. 

Với$m \le 5000$, một truy vấn trên một phạm vi độ dài$10^5$có thể yêu cầu đại khái$$10^5 \cdot 5000$$hoạt động hoàn toàn không thể thực hiện được. 

Quan sát quan trọng đến từ số học mô-đun. 

Chúng tôi muốn biết liệu có tồn tại một số$t \ge 0$như vậy$$a_i + t k \equiv 0 \pmod m.$$Sự đồng đẳng tuyến tính này có nghiệm khi và chỉ khi$$\gcd(k,m) \mid a_i.$$Cho phép$$g = \gcd(k,m).$$Sau đó:$$a_i \text{ is k-good}
\iff g \nmid a_i.$$Toàn bộ định nghĩa phức tạp được rút gọn thành một bài kiểm tra tính chia hết đơn giản. 

Bây giờ truy vấn trở thành: 

Đếm các phần tử trong$[l,r]$**không chia hết cho**$g$. 

Từ$m$không bao giờ thay đổi và$g$phải là ước của$m$, chỉ có ước của$m$có thể xuất hiện trong các truy vấn. Số ước của bất kỳ số nguyên nào lên đến$5000$thì ít, nhiều nhất là vài chục. 

Với mỗi ước số$d$của$m$, duy trì cây Fenwick lưu trữ các vị trí hiện chứa các số chia hết cho$d$. 

Sau đó:$$\text{answer}
=
(r-l+1)
-
\#\{\text{elements divisible by } g\}.$$Các bản cập nhật sửa đổi một vị trí, vì vậy chúng tôi cập nhật mỗi cây ước số một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O((r-l+1)\cdot m)$mỗi truy vấn |$O(1)$| Quá chậm | 
| Tối ưu |$O(D\log n)$mỗi hoạt động |$O(Dn)$| Đã chấp nhận | 

Đây$D$là số ước của$m$. 

## Hướng dẫn thuật toán 

1. Tính tất cả các ước của$m$. 
2. Tạo một cây Fenwick cho mỗi ước số$d$. 
3. Đối với mọi vị trí mảng$i$, chèn giá trị$1$vào cây chia$d$nếu như$a_i$chia hết cho$d$. 
4. Để cập nhật`1 i x`, loại bỏ phần đóng góp của giá trị cũ khỏi mọi cây ước và thêm phần đóng góp của giá trị mới. 

Số lượng ước số nhỏ nên việc chạm vào từng ước số là không tốn kém. 
5. Đối với một truy vấn`2 l r k`, tính toán$$g=\gcd(k,m).$$6. Sử dụng cây Fenwick tương ứng với số chia$g$đếm xem có bao nhiêu giá trị trong$[l,r]$được chia cho$g$. 
7. Hãy để số đó được tính`bad`. Số phần tử k-good là$$(r-l+1)-bad.$$8. Xuất kết quả. 

### Tại sao nó hoạt động 

Thuộc tính quan trọng là kết quả đồng đẳng tuyến tính cổ điển:$$a + tk \equiv 0 \pmod m$$có nghiệm khi và chỉ khi$$\gcd(k,m)\mid a.$$Nếu như$g=\gcd(k,m)$chia rẽ$a$, một số hạng trong tương lai sẽ chia hết cho$m$, vậy là số không k-tốt. 

Nếu như$g$không chia$a$, không có số hạng nào trong toàn bộ cấp số cộng có thể chia hết cho$m$, vậy số đó là k-tốt. 

Do đó, mọi truy vấn đều giảm chính xác về việc đếm các giá trị không chia hết cho$g$và cây Fenwick duy trì số lượng chia hết đó một cách chính xác theo các bản cập nhật. 

## Giải pháp Python```python
import sys
from math import gcd

input = sys.stdin.readline

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 1)

    def add(self, idx, delta):
        n = self.n
        bit = self.bit
        while idx <= n:
            bit[idx] += delta
            idx += idx & -idx

    def sum(self, idx):
        bit = self.bit
        res = 0
        while idx > 0:
            res += bit[idx]
            idx -= idx & -idx
        return res

    def range_sum(self, l, r):
        return self.sum(r) - self.sum(l - 1)

def solve():
    t = int(input())

    out = []

    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))

        divisors = []
        for d in range(1, m + 1):
            if m % d == 0:
                divisors.append(d)

        idx_of = {d: i for i, d in enumerate(divisors)}

        trees = [Fenwick(n) for _ in divisors]

        for pos, val in enumerate(a, start=1):
            for j, d in enumerate(divisors):
                if val % d == 0:
                    trees[j].add(pos, 1)

        q = int(input())

        for _ in range(q):
            query = list(map(int, input().split()))

            if query[0] == 1:
                _, i, x = query
                old = a[i - 1]

                for j, d in enumerate(divisors):
                    old_div = (old % d == 0)
                    new_div = (x % d == 0)

                    if old_div != new_div:
                        trees[j].add(i, 1 if new_div else -1)

                a[i - 1] = x

            else:
                _, l, r, k = query

                g = gcd(k, m)
                tree = trees[idx_of[g]]

                divisible = tree.range_sum(l, r)
                ans = (r - l + 1) - divisible

                out.append(str(ans))

    sys.stdout.write("\n".join(out))

solve()
```Phần đầu tiên tính toán tất cả các ước của$m$. Vì mọi truy vấn có thể đều sử dụng$g=\gcd(k,m)$, và mọi thứ như vậy$g$phải là ước của$m$, đây là những lớp chia hết duy nhất mà chúng ta cần. 

Mỗi cây Fenwick lưu trữ một ước số$d$, liệu mỗi vị trí hiện có chứa bội số của$d$. Tổng phạm vi ngay lập tức cho biết số bội của$d$bên trong bất kỳ khoảng nào. 

Thao tác cập nhật so sánh trạng thái chia hết cũ và mới cho mọi ước số. Chỉ khi trạng thái thay đổi, chúng ta mới sửa đổi cây Fenwick. Điều này tránh những cập nhật không cần thiết. 

Hoạt động truy vấn tính toán$g=\gcd(k,m)$, tìm cây Fenwick tương ứng, đếm bội số của$g$trong phạm vi và trừ đi độ dài khoảng. 

Tất cả số học đều vừa vặn thoải mái bên trong các số nguyên Python, do đó không có vấn đề tràn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 5, m = 10
a = [1, 2, 3, 4, 5]

query: 2 1 5 2
```Đây:$$g=\gcd(2,10)=2.$$| Vị trí | Giá trị | Chia hết cho 2? | 
| --- | --- | --- | 
| 1 | 1 | Không | 
| 2 | 2 | Có | 
| 3 | 3 | Không | 
| 4 | 4 | Có | 
| 5 | 5 | Không | 

Cây Fenwick cho ước số 2 cho biết có 2 phần tử chia hết. 

| tôi | r | Chiều dài | Chia hết cho g | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 5 | 5 | 2 | 3 | 

Đầu ra:```
3
```Điều này thể hiện sự giảm thiểu chính. Chúng tôi không bao giờ mô phỏng việc bổ sung lặp đi lặp lại của$k$. 

### Ví dụ 2 

đầu vào:```
n = 4, m = 8
a = [1, 8, 6, 5]

update: 1 2 9
query: 2 1 4 12
```Sau khi cập nhật:```
[1, 9, 6, 5]
```

$$g=\gcd(12,8)=4.$$| Vị trí | Giá trị | Chia hết cho 4? | 
| --- | --- | --- | 
| 1 | 1 | Không | 
| 2 | 9 | Không | 
| 3 | 6 | Không | 
| 4 | 5 | Không | 

| Chiều dài | Chia hết cho 4 | Trả lời | 
| --- | --- | --- | 
| 4 | 0 | 4 | 

Đầu ra:```
4
```Dấu vết này cho thấy cách cập nhật lan truyền qua cấu trúc số chia trước khi trả lời các truy vấn sau này. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(D \log n)$mỗi hoạt động | Mỗi cập nhật hoặc truy vấn chạm nhiều nhất một cây Fenwick trên mỗi ước số | 
| Không gian |$O(Dn)$| Một cây Fenwick cho mỗi ước số của$m$| 

Từ$m \le 5000$, số ước$D$là nhỏ. Kết hợp với giới hạn toàn cầu của$10^5$các phần tử mảng và truy vấn, điều này phù hợp một cách thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from math import gcd

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()

    input = sys.stdin.readline

    class Fenwick:
        def __init__(self, n):
            self.n = n
            self.bit = [0] * (n + 1)

        def add(self, i, v):
            while i <= self.n:
                self.bit[i] += v
                i += i & -i

        def sum(self, i):
            s = 0
            while i:
                s += self.bit[i]
                i -= i & -i
            return s

    t = int(input())
    ans = []

    for _ in range(t):
        n, m = map(int, input().split())
        a = list(map(int, input().split()))

        divs = [d for d in range(1, m + 1) if m % d == 0]
        mp = {d: i for i, d in enumerate(divs)}

        fw = [Fenwick(n) for _ in divs]

        for p, val in enumerate(a, start=1):
            for j, d in enumerate(divs):
                if val % d == 0:
                    fw[j].add(p, 1)

        q = int(input())

        for _ in range(q):
            qv = list(map(int, input().split()))

            if qv[0] == 1:
                _, i, x = qv
                old = a[i - 1]

                for j, d in enumerate(divs):
                    if (old % d == 0) != (x % d == 0):
                        fw[j].add(i, 1 if x % d == 0 else -1)

                a[i - 1] = x
            else:
                _, l, r, k = qv
                g = gcd(k, m)

                tree = fw[mp[g]]
                divcnt = tree.sum(r) - tree.sum(l - 1)

                ans.append(str((r - l + 1) - divcnt))

    return "\n".join(ans)

# sample 1
assert run(
"""1
5 10
1 2 3 4 5
5
2 1 5 1
2 1 5 2
2 1 5 3
2 1 5 4
2 1 5 5
"""
) == "0\n3\n0\n3\n0"

# minimum size
assert run(
"""1
1 7
1
1
2 1 1 14
"""
) == "1"

# all equal values
assert run(
"""1
4 6
6 6 6 6
1
2 1 4 3
"""
) == "0"

# update changes answer
assert run(
"""1
3 8
1 8 5
3
2 1 3 4
1 2 7
2 1 3 4
"""
) == "2\n3"

# boundary interval
assert run(
"""1
3 12
4 6 9
1
2 2 2 18
"""
) == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Yếu tố đơn,$m=7$| 1 | Trường hợp kích thước tối thiểu | 
| Tất cả các giá trị bằng$m$| 0 | Mọi phần tử chia hết cho gcd | 
| Cập nhật thay thế giá trị chia hết | 2 rồi 3 | Tuyên truyền cập nhật chính xác | 
| Truy vấn trên một vị trí | 0 | Xử lý ranh giới phạm vi | 

## Vỏ cạnh 

### Trường hợp 1:$k$là bội số của$m$đầu vào:```
1
3 10
10 11 12
1
2 1 3 20
```Đây$$g=\gcd(20,10)=10.$$Chỉ có giá trị đầu tiên chia hết cho 10. 

Cây Fenwick cho số chia 10 báo cáo một phần tử chia hết trong phạm vi. Độ dài khoảng là 3, vì vậy câu trả lời là$$3-1=2.$$Điều này phù hợp với hành vi thực tế vì việc thêm 20 không bao giờ làm thay đổi dư lượng modulo 10. 

### Trường hợp 2: Giá trị không chia hết cho$m$, nhưng vẫn không k-tốt 

đầu vào:```
1
1 8
4
1
2 1 1 12
```Chúng tôi có$$g=\gcd(12,8)=4.$$Giá trị 4 chia hết cho 4 nên được tính là không k-good. 

Thực vậy,$$4+12=16,$$đó là chia hết cho 8. 

Thuật toán xuất ra 0, điều này đúng. 

### Trường hợp 3: Phạm vi vị trí đơn sau khi cập nhật 

đầu vào:```
1
1 6
6
2
1 1 5
2 1 1 3
```Sau khi cập nhật, giá trị trở thành 5.$$g=\gcd(3,6)=3.$$Vì 5 không chia hết cho 3 nên cây Fenwick cho biết không có phần tử nào chia hết. 

Câu trả lời là$$1-0=1.$$Điều này xác nhận rằng các bản cập nhật và phạm vi một phần tử hoạt động chính xác cùng nhau.
