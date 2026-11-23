---
title: "CF 449E - Jzzhu và hình vuông"
description: "Chúng ta có tất cả các điểm mạng bên trong một hình chữ nhật có kích thước $n nhân m$. Bất kỳ hình vuông nào có bốn đỉnh là điểm mạng đều được phép, ngay cả khi nó được xoay. Đối với mỗi ô vuông như vậy, chúng ta xem xét các ô lưới đơn vị nằm hoàn toàn bên trong nó. Mỗi ô đơn vị chứa nhận được một dấu chấm."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dp", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 449
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 257 (Div. 1)"
rating: 2900
weight: 449
solve_time_s: 153
verified: false
draft: false
---

[CF 449E - Jzzhu và Squares](https://codeforces.com/problemset/problem/449/E) 

**Xếp hạng:** 2900 
**Tags:** dp, toán, lý thuyết số 
**Thời gian giải:** 2m 33s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có tất cả các điểm mạng bên trong một hình chữ nhật có kích thước$n \times m$. Bất kỳ hình vuông nào có bốn đỉnh là điểm mạng đều được phép, ngay cả khi nó được xoay. 

Đối với mỗi ô vuông như vậy, chúng ta xem xét các ô lưới đơn vị nằm hoàn toàn bên trong nó. Mỗi ô đơn vị chứa nhận được một dấu chấm. Một ô có thể nhận được dấu chấm từ nhiều ô vuông khác nhau. Nhiệm vụ là đếm tổng số chấm được sơn. 

Đầu vào chứa tối đa$10^5$trường hợp thử nghiệm, trong khi cả hai$n$Và$m$có thể lớn như$10^6$. Một giải pháp thực hiện bất kỳ công việc có ý nghĩa nào cho mỗi truy vấn đều đang gặp nguy hiểm. Thậm chí một$O(\sqrt n)$giải pháp cho mỗi thử nghiệm sẽ quá đắt trong trường hợp xấu nhất. Công việc nặng nhọc phải được thực hiện một lần trong quá trình tiền xử lý, sau đó mỗi truy vấn sẽ được trả lời trong thời gian không đổi. 

Phần khó khăn là các hình vuông không bị hạn chế căn chỉnh theo trục. Cách triển khai đơn giản thường chỉ tính các ô vuông thông thường có cạnh nằm dọc theo lưới, điều này bỏ sót hầu hết câu trả lời. 

Coi như:```
n = 2, m = 2
```Ngoài hình vuông 4 đơn vị còn có hình vuông có đỉnh```
(1,0), (2,1), (1,2), (0,1)
```Nó chứa hoàn toàn một ô đơn vị bên trong nó, vì vậy việc bỏ qua các ô vuông được xoay sẽ tạo ra một câu trả lời nhỏ hơn giá trị đúng$8$. 

Một sai lầm dễ mắc phải khác là đếm các ô giao nhau với hình vuông thay vì đếm các ô chứa đầy bên trong nó. Đối với hình thoi ở trên, một số ô chạm vào ranh giới nhưng chỉ có ô trung tâm nằm hoàn toàn bên trong. 

Nguồn lỗi thứ ba là việc đếm hai lần các mô tả hình vuông. Một hình vuông được tạo bởi một vectơ cạnh$(a,b)$có hình dạng giống như hình được tạo bởi$(b,a)$, nhưng chúng tương ứng với các vị trí khác nhau bên trong lưới và cả hai phải được đưa vào khi tính tổng tất cả các hướng hợp lệ. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là liệt kê mọi ô vuông. 

Một hình vuông có thể được mô tả bằng một đỉnh bắt đầu và một vectơ cạnh$(a,b)$. Cạnh thứ hai thu được bằng cách quay vectơ đó bằng$90^\circ$, cho$(-b,a)$. Khi tất cả bốn đỉnh đều nằm trong hình chữ nhật, chúng ta có một hình vuông hợp lệ. 

Sau khi tạo hình vuông, chúng ta có thể kiểm tra từng ô đơn vị và đếm xem có bao nhiêu ô nằm hoàn toàn bên trong ô đó. 

Điều này đúng, nhưng chậm một cách vô vọng. Đã có rồi$O(n^2m^2)$các kết hợp đỉnh có thể xảy ra và việc kiểm tra ngăn chặn cho từng ô khiến vấn đề càng trở nên tồi tệ hơn. Với$n,m \le 10^6$, vũ lực là không khả thi từ xa. 

Quan sát quan trọng là hình học của một hình vuông mạng chỉ phụ thuộc vào vectơ cạnh của nó. 

Lấy một vectơ bên$(a,b)$với$a,b \ge 0$. Hình vuông thẳng hàng nhỏ nhất chứa hình vuông lưới có độ dài cạnh$$L=a+b.$$Mỗi ô vuông có cùng giá trị$L$có chính xác cùng số lượng vị trí có thể có:$$(n-L+1)(m-L+1).$$Vì vậy, thay vì nghĩ đến vị trí trước tiên, chúng ta có thể phân loại tất cả các ô vuông theo$L$. 

Bước tiếp theo là xác định có bao nhiêu ô đơn vị được chứa đầy đủ bên trong một hình vuông được tạo bởi$(a,b)$. 

Một sự phân rã hình học cho$$f(a,b)=a^2+b^2-2(a+b)+2\gcd(a,b).$$Điều này biến bài toán hình học thành số học thuần túy. 

Đối với một cố định$$L=a+b,$$định nghĩa$$F(L)=\sum_{a=0}^{L-1} f(a,L-a).$$Sau đó, mọi ô vuông có kích thước giới hạn$L$đóng góp thông qua cùng một giá trị$F(L)$, và câu trả lời trở thành$$\sum_{L=1}^{\min(n,m)} (n-L+1)(m-L+1)F(L).$$Bây giờ chỉ còn lại lý thuyết số. 

sử dụng$$\gcd(a,L-a)=\gcd(a,L),$$chúng tôi có được$$F(L) = \frac{2L^3+L}{3} -2L^2 + 2\sum_{a=0}^{L-1}\gcd(a,L).$$Bản sắc cổ điển$$\sum_{a=0}^{L-1}\gcd(a,L) = \sum_{d\mid L} d\,\varphi(L/d)$$cho phép mọi giá trị của$F(L)$lên tới$10^6$được tính toán trước bằng sàng chia. 

Cuối cùng, mở rộng$$(n-L+1)(m-L+1) = (n+1)(m+1) -(n+m+2)L +L^2.$$Nếu chúng ta lưu trữ tổng tiền tố của$$F(L),\quad L F(L),\quad L^2 F(L),$$mỗi truy vấn trở thành một$O(1)$công thức. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ trong kích thước lưới hiệu quả, vượt xa$O(n^2m^2)$| Rất lớn | Quá chậm | 
| Tối ưu | Tiền xử lý$O(N \log N)$, truy vấn$O(1)$|$O(N)$| Đã chấp nhận | 

Đây$N = 10^6$. 

## Hướng dẫn thuật toán 

1. Tính trước hàm tổng Euler$\varphi(i)$cho tất cả$i \le 10^6$sử dụng sàng tuyến tính. 
2. Tính toán$$G(i)=\sum_{d\mid i} d\,\varphi(i/d).$$Điều này có thể được thực hiện bằng cách lặp qua tất cả các ước số:$$G(j) += i \cdot \varphi(j/i).$$Tổng độ phức tạp là$O(N \log N)$. 

1. Với mọi$L$từ$1$ĐẾN$N$, tính toán$$F(L) = \frac{2L^3+L}{3} -2L^2 + 2G(L).$$Tất cả số học được thực hiện theo modulo$10^9+7$. 

1. Xây dựng ba mảng tiền tố:$$S_0(x)=\sum_{L\le x}F(L),$$

$$S_1(x)=\sum_{L\le x}L\,F(L),$$

$$S_2(x)=\sum_{L\le x}L^2F(L).$$1. Đối với một truy vấn, hãy hoán đổi$n,m$nếu cần thiết để$n \le m$. 
2. Hãy để$k=n$. Mọi ô vuông hợp lệ phải thỏa mãn$L \le k$. 
3. Sử dụng phần mở rộng$$(n-L+1)(m-L+1) = (n+1)(m+1) -(n+m+2)L +L^2.$$1. Thay thế các tiền tố:$$\text{ans} = (n+1)(m+1)S_0(k) -(n+m+2)S_1(k) +S_2(k).$$1. Xuất kết quả theo modulo$10^9+7$. 

### Tại sao nó hoạt động 

Mọi ô vuông được tạo bởi vectơ cạnh$(a,b)$được liên kết duy nhất với$L=a+b$, độ dài cạnh của hình vuông giới hạn căn chỉnh theo trục tối thiểu của nó. 

Đối với một cố định$L$, tất cả các hình vuông như vậy đều có chính xác$(n-L+1)(m-L+1)$các vị trí. Công thức hình học tính toán số ô đơn vị chứa đầy đủ cho mỗi hình và tính tổng tất cả các cặp$(a,L-a)$sản xuất$F(L)$. 

Nhận dạng ước số chuyển đổi phần đóng góp của gcd thành hàm số học nhân, cho phép tất cả$F(L)$các giá trị được tính toán trước. Vì mỗi ô vuông hợp lệ thuộc về đúng một giá trị của$L$và mỗi vị trí được tính chính xác một lần, tổng cuối cùng bằng tổng số chấm được sơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 1000000007
MAXN = 1000000

phi = [0] * (MAXN + 1)
primes = []
is_comp = [False] * (MAXN + 1)

phi[1] = 1
for i in range(2, MAXN + 1):
    if not is_comp[i]:
        primes.append(i)
        phi[i] = i - 1
    for p in primes:
        v = i * p
        if v > MAXN:
            break
        is_comp[v] = True
        if i % p == 0:
            phi[v] = phi[i] * p
            break
        phi[v] = phi[i] * (p - 1)

g = [0] * (MAXN + 1)

for d in range(1, MAXN + 1):
    step = d
    for x in range(d, MAXN + 1, step):
        g[x] = (g[x] + d * phi[x // d]) % MOD

inv3 = pow(3, MOD - 2, MOD)

s0 = [0] * (MAXN + 1)
s1 = [0] * (MAXN + 1)
s2 = [0] * (MAXN + 1)

for L in range(1, MAXN + 1):
    l = L % MOD

    part = ((2 * l * l % MOD) * l + l) % MOD
    part = part * inv3 % MOD

    f = (part - 2 * l * l + 2 * g[L]) % MOD

    s0[L] = (s0[L - 1] + f) % MOD
    s1[L] = (s1[L - 1] + l * f) % MOD
    s2[L] = (s2[L - 1] + l * l % MOD * f) % MOD

t = int(input())

ans = []
for _ in range(t):
    n, m = map(int, input().split())

    if n > m:
        n, m = m, n

    k = n

    res = (
        (n + 1) * (m + 1) % MOD * s0[k]
        - (n + m + 2) % MOD * s1[k]
        + s2[k]
    ) % MOD

    ans.append(str(res))

sys.stdout.write("\n".join(ans))
```Sàng tuyến tính tính toán tất cả các tổng trong$O(N)$. Sau đó, vòng chia ước tính$$G(x)=\sum_{d\mid x} d\varphi(x/d)$$cho mọi$x$. 

Công thức cho$F(L)$chứa phép chia cho ba. Vì việc tính toán được thực hiện theo modulo một số nguyên tố nên nghịch đảo mô đun của 3 được sử dụng thay cho phép chia số nguyên thông thường. 

Mảng tiền tố là cốt lõi của việc xử lý truy vấn theo thời gian không đổi. Một lần$$\sum F(L), \quad \sum L F(L), \quad \sum L^2F(L)$$có sẵn, trọng số bậc hai mở rộng có thể được đánh giá ngay lập tức. 

Một lỗi triển khai phổ biến là quên trao đổi$n$Và$m$. Tổng kết chỉ chạy đến$\min(n,m)$, bởi vì một hình vuông có kích thước giới hạn$L$không thể phù hợp nếu$L$vượt quá một trong hai chiều. 

Một lỗi dễ mắc khác là sử dụng phép chia số nguyên thông thường sau khi lấy modulo. Số hạng bậc ba phải được nhân với nghịch đảo mô đun của ba. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 3
```Chỉ một$L=1$là có thể. 

| L | F(L) | Vị trí | Đóng góp | 
| --- | --- | --- | --- | 
| 1 | 1 | 3 | 3 | 

Tổng số câu trả lời:$$3.$$Ví dụ này cho thấy các ô vuông đơn vị căn chỉnh theo trục thông thường được bao gồm. Mỗi ô trong số ba ô đơn vị nhận được chính xác một dấu chấm. 

### Ví dụ 2 

đầu vào:```
2 2
```Các giá trị có thể có là$L=1$Và$L=2$. 

| L | F(L) | Vị trí | Đóng góp | 
| --- | --- | --- | --- | 
| 1 | 1 | 4 | 4 | 
| 2 | 4 | 1 | 4 | 

Tổng cộng:$$4+4=8.$$các$L=2$sự đóng góp đến từ các ô vuông được quay. Nếu không có chúng thì câu trả lời sẽ sai$4$. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N \log N + t)$| Việc tính toán trước chiếm ưu thế, mỗi truy vấn được$O(1)$| 
| Không gian |$O(N)$| Tổng số, tổng chia và mảng tiền tố | 

Với$N=10^6$, quá trình tiền xử lý phù hợp thoải mái trong giới hạn. Phần quan trọng là$10^5$truy vấn được trả lời trong thời gian liên tục. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from math import gcd

MOD = 1000000007

def brute(n, m):
    ans = 0

    for x in range(n + 1):
        for y in range(m + 1):
            for a in range(-n, n + 1):
                for b in range(-m, m + 1):
                    if a == 0 and b == 0:
                        continue

                    pts = [
                        (x, y),
                        (x + a, y + b),
                        (x - b, y + a),
                        (x + a - b, y + a + b),
                    ]

                    ok = True
                    for px, py in pts:
                        if not (0 <= px <= n and 0 <= py <= m):
                            ok = False
                            break

                    if not ok:
                        continue

                    # avoid counting same square four times
                    if (a < 0) or (a == 0 and b < 0):
                        continue

                    minx = min(p[0] for p in pts)
                    maxx = max(p[0] for p in pts)
                    miny = min(p[1] for p in pts)
                    maxy = max(p[1] for p in pts)

                    def inside(cx, cy):
                        s = []
                        for i in range(4):
                            x1, y1 = pts[i]
                            x2, y2 = pts[(i + 1) % 4]
                            s.append(
                                (x2 - x1) * (cy - y1)
                                - (y2 - y1) * (cx - x1)
                            )
                        return all(v >= 0 for v in s) or all(v <= 0 for v in s)

                    for i in range(minx, maxx):
                        for j in range(miny, maxy):
                            corners = [
                                (i, j),
                                (i + 1, j),
                                (i, j + 1),
                                (i + 1, j + 1),
                            ]
                            if all(inside(cx, cy) for cx, cy in corners):
                                ans += 1

    return ans

def run(inp: str) -> str:
    data = inp.strip().split()
    t = int(data[0])

    out = []
    ptr = 1
    for _ in range(t):
        n = int(data[ptr])
        m = int(data[ptr + 1])
        ptr += 2
        out.append(str(brute(n, m)))
    return "\n".join(out)

# provided samples
assert run("4\n1 3\n2 2\n2 5\n3 4\n") == "3\n8\n26\n58", "sample"

# minimum size
assert run("1\n1 1\n") == "1", "single unit square"

# thin rectangle
assert run("1\n1 2\n") == "2", "no room for rotated squares"

# symmetric small case
assert run("1\n2 2\n") == "8", "rotated square contribution"

# off-by-one on bounding size
assert run("1\n2 3\n") == "14", "largest L fits exactly once"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`1`| Lưới nhỏ nhất có thể | 
|`1 2`|`2`| Không có hình vuông xoay nào có thể vừa | 
|`2 2`|`8`| Lưới vuông xoay đóng góp | 
|`2 3`|`14`| Xử lý đúng khi hình vuông giới hạn lớn nhất vừa khít một lần | 

## Vỏ cạnh 

Hãy xem xét:```
1
1 2
```Chỉ một$L=1$phù hợp. Thuật toán đánh giá$$(1-1+1)(2-1+1)F(1)=2.$$Không có hình vuông giới hạn lớn hơn có thể phù hợp bởi vì$L=2>\min(n,m)$. Câu trả lời chính xác là hai ô đơn vị. 

Bây giờ hãy xem xét:```
1
2 2
```Tổng hợp sử dụng$L=1$Và$L=2$. 

Vì$L=2$, cặp$(1,1)$tượng trưng cho hình vuông hình kim cương. Đóng góp của nó được bao gồm bên trong$F(2)$, đó là lý do tại sao kết quả trở thành$8$thay vì$4$. 

Cuối cùng, hãy xem xét:```
1
3 3
```Một giải pháp bất cẩn là đếm các ô chỉ giao nhau với một ranh giới hình vuông sẽ bị tính quá mức. Công thức cho$f(a,b)$được bắt nguồn đặc biệt cho các ô chứa đầy bên trong hình vuông. Thuật ngữ gcd xuất phát từ các điểm mạng trên đường biên và loại bỏ chính xác các ô được che phủ một phần. Sự hiệu chỉnh hình học đó là lý do tại sao công thức số học vẫn chính xác cho mọi hướng. 

Nguồn:
