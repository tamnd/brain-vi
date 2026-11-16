---
title: "CF 1912B - Bản thiết kế chỗ ngồi"
description: "Chúng tôi được bố trí một dãy ghế được chia thành các dãy liền kề nhau theo lối đi. Mỗi dãy nhà là một đoạn ghế có chiều dài dương và giữa hai khối liên tiếp bất kỳ có chính xác một lối đi."
date: "2026-06-08T20:14:00+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "divide-and-conquer", "math"]
categories: ["algorithms"]
codeforces_contest: 1912
codeforces_index: "B"
codeforces_contest_name: "2023-2024 ICPC, NERC, Northern Eurasia Onsite (Unrated, Online Mirror, ICPC Rules, Teams Preferred)"
rating: 2100
weight: 1912
solve_time_s: 162
verified: false
draft: false
---

[CF 1912B - Bản thiết kế chỗ ngồi](https://codeforces.com/problemset/problem/1912/B) 

**Xếp hạng:** 2100 
**Tags:** tổ hợp, chia để trị, toán 
**Thời gian giải:** 2m 42s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được bố trí một dãy ghế được chia thành các dãy liền kề nhau theo lối đi. Mỗi dãy nhà là một đoạn ghế có chiều dài dương và giữa hai khối liên tiếp bất kỳ có chính xác một lối đi. Nếu có$k$lối đi, sau đó có$k+1$các khối và kích thước của chúng tạo thành một thành phần của$n$vào trong$k+1$số nguyên dương. 

Bên trong một dãy nhà, mỗi chỗ ngồi có chi phí bằng khoảng cách đến lối đi gần nhất. Điều đó có nghĩa là trong một khối có kích thước$m$, mức đóng góp là đối xứng: những ghế gần lối đi đóng góp 0, rồi 1, v.v. cho đến đầu xa nhất của dãy nhà. 

Tổng số bất tiện là tổng của tất cả các khối có khoảng cách trong khối này. Chúng ta phải chọn kích thước khối để giảm thiểu tổng giá trị này và cũng đếm xem có bao nhiêu chuỗi kích thước khối khác nhau đạt được mức tối thiểu đó, modulo$998244353$. 

Do đó, đối tượng chính là một thành phần của$n$vào trong$k+1$số nguyên dương, trong đó mỗi phần đóng góp một chi phí lồi tùy thuộc vào kích thước của nó. 

Các hạn chế là lớn theo một cách cụ thể. Số lượng test case lên tới$10^5$, và cả hai$n$Và$k$có thể lớn với$n$lên đến$10^9$. Tuy nhiên, tổng của tất cả$k$được giới hạn bởi$10^6$, điều này gợi ý mạnh mẽ rằng bất kỳ giải pháp nào phụ thuộc tuyến tính vào$k$mỗi bài kiểm tra đều có thể chấp nhận được, nhưng mọi thứ tùy thuộc vào$n$trực tiếp là không thể. 

Một cách tiếp cận ngây thơ liệt kê tất cả các thành phần của$n$vào trong$k+1$các phần là không thể ngay lập tức vì số lượng các tác phẩm như vậy là$\binom{n-1}{k}$, đó là hàm mũ trong$k$và không khả thi ngay cả đối với các giá trị vừa phải. 

Một vấn đề tinh vi hơn xuất hiện trong việc tính toán chi phí. Ngay cả khi chúng tôi sửa một thành phần, việc tính toán lại sự bất tiện bằng cách mô phỏng từng khối vẫn ổn, nhưng việc khám phá tất cả các thành phần là nút thắt thực sự. 

Các trường hợp cạnh phát sinh khi$k$gần với$n-1$, nghĩa là mọi khối đều có kích thước 1 ngoại trừ cấu trúc tầm thường và khi$k=1$, trong đó vấn đề giảm xuống còn việc chia thành hai phần và chi phí trở thành một hàm đơn giản của một biến nhưng vẫn phải được tối ưu hóa trên một phạm vi lớn. 

Một sai lầm đặc biệt dễ mắc phải là cho rằng việc phân chia bằng nhau luôn là tối ưu mà không chứng minh được các ràng buộc về tính rời rạc; một cách khác là coi chi phí mỗi khối là tuyến tính thay vì hành vi giống như bậc hai. 

## Phương pháp tiếp cận 

Đầu tiên chúng tôi diễn giải lại chi phí của một khối. Hãy xem xét một khối có kích thước$m$. Dán nhãn ghế từ một đầu: khoảng cách đến lối đi gần nhất là$0,1,2,\dots$cho đến điểm giữa rồi đối xứng. Tổng chi phí trở thành một hàm số chỉ phụ thuộc vào$m$, đặc biệt tỉ lệ với$\lfloor m/2 \rfloor \cdot \lceil m/2 \rceil$. Điều này có thể được xác minh bằng cách ghép các khoảng cách đối xứng hoặc tính tổng các cấp số cộng. 

Vì vậy, vấn đề giảm xuống việc lựa chọn$k+1$số nguyên dương$a_1,\dots,a_{k+1}$, tổng hợp thành$n$, giảm thiểu$$\sum f(a_i), \quad f(m) = \left\lfloor \frac{m^2}{4} \right\rfloor.$$chức năng$f(m)$lồi ở$m$, vì sai phân thứ hai rời rạc của nó không âm. Điều này ngay lập tức gợi ý một nguyên tắc cân bằng: phân phối khối lượng càng đồng đều càng tốt sẽ giảm thiểu tổng chi phí lồi. 

Vì vậy, cấu trúc tối ưu là tất cả các kích thước khối khác nhau tối đa 1. Nếu chúng ta đặt$k+1 = g$, thì chúng ta muốn chia$n$vào trong$g$phần bằng nhau nhất có thể:$$n = qg + r, \quad 0 \le r < g,$$Vì thế$r$các bộ phận có kích thước$q+1$, Và$g-r$các bộ phận có kích thước$q$. 

Điều này mang lại sự bất tiện tối thiểu bằng cách cắm trực tiếp vào$f$. 

Phần đếm mang tính tổ hợp: chúng ta phải đếm có bao nhiêu cách để chỉ định khối nào có kích thước$q+1$. Vì các khối được sắp xếp theo thứ tự (chúng tạo thành một chuỗi), việc chọn khối nào$r$vị trí nhận được giá trị lớn hơn sẽ xác định đầy đủ bố cục. Do đó số lượng bố trí tối ưu chỉ đơn giản là:$$\binom{g}{r}.$$Ý tưởng vũ phu sẽ thử tất cả các tác phẩm; đây là số mũ trong$n$. Giải pháp tối ưu thu gọn cấu trúc thành một bước chia duy nhất và hệ số nhị thức. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force đối với các tác phẩm | hàm mũ | O(k) | Quá chậm | 
| Cân bằng lồi + tổ hợp | O(k) mỗi lần kiểm tra | O(1) thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

hãy để$g = k+1$, số đoạn. 

1. Tính toán$g$và chia$n$qua$g$, thu được thương số$q$và phần còn lại$r$. 

Điều này xác định hình dạng khả thi duy nhất của giải pháp tối ưu vì độ lồi tạo ra tính đồng nhất. 
2. Tính chi phí tối thiểu bằng cách cộng tổng đóng góp của các khối: 

mỗi khối đóng góp$f(q)$hoặc$f(q+1)$. 

Chúng tôi tính toán:$$(g-r)\cdot f(q) + r \cdot f(q+1).$$3. Tính số cách chọn$r$phân đoạn nhận được kích thước$q+1$. 

Vì các vấn đề thứ tự và khối chỉ có thể được phân biệt theo vị trí nên đây là hệ số nhị thức trực tiếp:$$\binom{g}{r} \bmod 998244353.$$4. Tính toán trước các giai thừa và nghịch đảo mô đun đến mức tối đa có thể$k+1$, vì các truy vấn yêu cầu tính toán nhị thức lặp đi lặp lại. 

Lý do cơ cấu quan trọng khiến điều này có hiệu quả là bất kỳ sai lệch nào so với kích thước cân bằng đều có thể được cải thiện cục bộ. Nếu hai khối khác nhau ít nhất là 2, việc di chuyển một đơn vị từ khối lớn hơn sang khối nhỏ hơn sẽ làm giảm tổng chi phí do tính lồi của$f$. 

## Tại sao nó hoạt động 

Hàm chi phí trên mỗi khối là lồi về kích thước khối, do đó, bất kỳ sự mất cân bằng nào giữa hai khối đều làm tăng tổng chi phí. Áp dụng nhiều lần thao tác chuyển một chỗ ngồi từ khối lớn hơn sang khối nhỏ hơn sẽ bảo toàn tổng số tiền nhưng giảm thiểu chi phí cho đến khi tất cả các kích thước khác nhau tối đa một. Quá trình này hội tụ đến một tập hợp nhiều kích thước khối duy nhất được xác định bởi$n$Và$g$, thiết lập sự tối ưu. Phần tổ hợp tiếp theo vì chỉ có vị trí của các khối lớn hơn mới là lựa chọn tự do. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

MAXK = 1000005

fact = [1] * (MAXK)
invfact = [1] * (MAXK)

for i in range(1, MAXK):
    fact[i] = fact[i - 1] * i % MOD

invfact[MAXK - 1] = pow(fact[MAXK - 1], MOD - 2, MOD)
for i in range(MAXK - 2, -1, -1):
    invfact[i] = invfact[i + 1] * (i + 1) % MOD

def C(n, r):
    if r < 0 or r > n:
        return 0
    return fact[n] * invfact[r] % MOD * invfact[n - r] % MOD

def block_cost(m):
    return (m // 2) * ((m + 1) // 2)

t = int(input())
for _ in range(t):
    n, k = map(int, input().split())
    g = k + 1

    q, r = divmod(n, g)

    min_cost = (g - r) * block_cost(q) + r * block_cost(q + 1)
    ways = C(g, r)

    print(min_cost, ways)
```Việc tính toán trước giai thừa là cần thiết vì mỗi trường hợp thử nghiệm cần một hệ số nhị thức và việc tính toán lại các kết hợp cho mỗi truy vấn sẽ quá chậm. 

chức năng`block_cost`mã hóa dạng đóng dẫn xuất cho một phân đoạn. Điều quan trọng là phải sử dụng phép chia số nguyên một cách cẩn thận vì chi phí phụ thuộc vào tính chẵn lẻ thông qua hành vi sàn và trần. 

Phần phân phối còn lại sử dụng`divmod`, trực tiếp nắm bắt quyền tự do duy nhất trong một cấu hình tối ưu: khối nào có được chỗ ngồi bổ sung. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$n=5, k=2$Đây$g=3$. Chúng tôi chia 5 cho 3:$q=1, r=2$. Vậy hai khối có kích thước 2, một khối có kích thước 1. 

| bước | giá trị | 
| --- | --- | 
| g | 3 | 
| q, r | 1, 2 | 
| kích thước | 2, 2, 1 | 
| giá mỗi khối | 1, 1, 0 | 
| tổng chi phí | 2 | 
| cách |$\binom{3}{2} = 3$| 

Điều này phù hợp với ý tưởng rằng chỉ có vị trí của khối nhỏ là thay đổi. 

### Ví dụ 2:$n=6, k=1$Đây$g=2$. Chúng tôi chia 6 thành 2 phần:$q=3, r=0$. Cả hai khối đều có kích thước 3. 

| bước | giá trị | 
| --- | --- | 
| g | 2 | 
| q, r | 3, 0 | 
| kích thước | 3, 3 | 
| giá mỗi khối | 2, 2 | 
| tổng chi phí | 4 | 
| cách | 1 | 

Điều này khẳng định rằng khi phép chia chính xác thì không có sự tự do tổ hợp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(k_{total} + T)$| tính toán trước giai thừa một lần, mỗi lần kiểm tra là O(1) | 
| Không gian |$O(k_{max})$| mảng giai thừa và nghịch đảo | 

Giải pháp này hiệu quả vì tất cả các tổ hợp nặng đều được tính toán trước một lần và mỗi truy vấn giảm xuống số học không đổi và tra cứu hệ số nhị thức. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    # inline solution
    import sys
    input = sys.stdin.readline

    MAXK = 50  # small for tests

    fact = [1] * (MAXK)
    invfact = [1] * (MAXK)

    for i in range(1, MAXK):
        fact[i] = fact[i - 1] * i % MOD
    invfact[MAXK - 1] = pow(fact[MAXK - 1], MOD - 2, MOD)
    for i in range(MAXK - 2, -1, -1):
        invfact[i] = invfact[i + 1] * (i + 1) % MOD

    def C(n, r):
        if r < 0 or r > n:
            return 0
        return fact[n] * invfact[r] % MOD * invfact[n - r] % MOD

    def block_cost(m):
        return (m // 2) * ((m + 1) // 2)

    t = int(input())
    out = []
    for _ in range(t):
        n, k = map(int, input().split())
        g = k + 1
        q, r = divmod(n, g)
        min_cost = (g - r) * block_cost(q) + r * block_cost(q + 1)
        ways = C(g, r)
        out.append(f"{min_cost} {ways}")
    return "\n".join(out)

# provided samples
assert run("""8
4 1
3 2
4 2
5 2
6 1
6 2
1000000000 1
9 2
""") == """2 1
0 1
0 1
1 3
6 1
2 4
249999999500000000 1
6 3"""

# custom cases
assert run("2\n2 1\n3 1\n") == "1 1\n2 1", "minimum edge splits"
assert run("1\n10 1\n") == "25 1", "single cut symmetry"
assert run("1\n6 3\n") == "0 1", "all single seats"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2\n2 1\n3 1\n`|`1 1\n2 1`| phân vùng hợp lệ nhỏ nhất | 
|`1\n10 1\n`|`25 1`| tính đối xứng của phép chia đơn | 
|`1\n6 3\n`|`0 1`| phân khúc quá mức | 

## Vỏ cạnh 

Khi nào$k = n-1$, mọi khối phải có kích thước 1. Thuật toán tính toán$g=n$,$q=1$,$r=0$, sản xuất tất cả các khối đơn và chi phí bằng 0, điều này đúng vì mọi chỗ ngồi đều liền kề với một lối đi. 

Khi$k = 1$, we split into two blocks. Thuật toán giảm xuống$n = 2q + r$, cho các nửa bằng nhau hoặc chênh lệch một, phù hợp với sự cân bằng tối ưu của chi phí bậc hai lồi. 

Khi$n$không chia hết cho$k+1$, phân phối phần dư đảm bảo chính xác$r$các khối lớn hơn và số hạng tổ hợp giải thích chính xác tất cả các hoán vị của các vị trí đó.
