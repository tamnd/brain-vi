---
title: "CF 102192F - Boolean 3-Array"
description: "Mảng 3 Boolean là một hộp (mtimes ntimes p) có các ô chứa 0 hoặc 1. Chúng ta có thể hoán vị các lát cắt một cách độc lập dọc theo từng chiều trong số ba chiều và chúng ta có thể chuyển đổi bất kỳ lát cắt hoàn chỉnh nào dọc theo bất kỳ chiều nào."
date: "2026-08-18T02:04:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102192
codeforces_index: "F"
codeforces_contest_name: "2018 Chinese Multi-University Training, Nanjing U Contest"
rating: 0
weight: 102192
solve_time_s: 273
verified: true
draft: false
---

[CF 102192F - Boolean 3-Array](https://codeforces.com/problemset/problem/102192/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4m 33s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mảng Boolean 3-là một hộp (m\times n\times p) có các ô chứa 0 hoặc một. Chúng ta có thể hoán vị các lát cắt một cách độc lập dọc theo từng chiều trong số ba chiều và chúng ta có thể chuyển đổi bất kỳ lát cắt hoàn chỉnh nào dọc theo bất kỳ chiều nào. Hai mảng thuộc cùng một lớp nếu một mảng có thể được thay đổi thành mảng khác bằng các thao tác này. Nhiệm vụ là đếm xem có bao nhiêu lớp như vậy tồn tại, vì từ mỗi lớp chúng ta có thể chọn nhiều nhất một đại diện. 

Đầu vào chính thức có tới 300 trường hợp thử nghiệm và mọi chiều tối đa là 13. Giới hạn chiều nhỏ là đầu mối trung tâm. Việc liệt kê trực tiếp các mảng (2^{mnp}) đã không thể thực hiện được tại (13\times13\times13), trong đó có chính xác (2^{2197}) mảng. Các nhóm hoán vị cũng quá lớn để liệt kê riêng lẻ. Chúng ta cần đếm quỹ đạo mà không cần xây dựng chúng. 

Có một số trường hợp dễ dàng bộc lộ sai sót khi triển khai một cách ngây thơ. Vì`1 1 1`, ô đơn có thể được chuyển đổi, do đó hai mảng có thể tương đương nhau và câu trả lời là`1`. Vì`1 1 2`, mỗi lớp trong số hai lớp có thể được chuyển đổi độc lập, do đó cả bốn mảng đều tương đương nhau và câu trả lời lại là`1`. Một chương trình chỉ xem xét các hoán vị sẽ trả về không chính xác hai lớp trong trường hợp này. 

Một trường hợp biên ít tầm thường hơn một chút là`1 2 2`. Sau khi chiều thứ nhất biến mất, cấu trúc là ma trận nhị phân (2\times2). Việc chuyển đổi hàng và cột thay đổi hai ô cùng một lúc, do đó tính chẵn lẻ của bốn ô là bất biến. Cả hai chẵn lẻ xảy ra, đưa ra câu trả lời`2`. Một lập luận bất cẩn cho rằng việc lật lát tùy ý luôn có thể xóa toàn bộ mảng đã bỏ lỡ bất biến này. 

Tuyên bố chính thức xác nhận giới hạn (m,n,p\le13), giới hạn 2 giây và đầu ra mẫu`1`,`9`, Và`723`vì`1 1 1`,`2 2 2`, Và`2 3 4`. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ liệt kê từng mảng trong số (2^{mnp}) và gán từng mảng cho một lớp tương đương bằng cách thử các thao tác được phép. Điều này đúng vì các phép toán tạo ra chính xác quan hệ tương đương từ câu lệnh. Ở kích thước lớn nhất có (2^{2197}) mảng, khoảng (10^{661}), do đó, thậm chí chỉ đọc tất cả các mảng có thể là không thể. So sánh theo cặp sẽ yêu cầu so sánh chính xác (\binom{2^{2197}}2) trong trường hợp xấu nhất. 

Quan sát hữu ích là các phép toán tạo thành một tác động nhóm hữu hạn trên tập hợp các mảng Boolean. Do đó, số lượng các lớp tương đương được đưa ra bởi bổ đề Burnside: trung bình, trên tất cả các phần tử nhóm, số mảng cố định bởi phần tử đó. 

Một phần tử nhóm bao gồm một hoán vị của chiều thứ nhất, một hoán vị của chiều thứ hai, một hoán vị của chiều thứ ba và một tập hợp các chuyển đổi lát cắt. Thay vì xử lý các hoán vị riêng lẻ, chúng tôi nhóm các hoán vị theo loại chu trình. Một hoán vị có kích thước tối đa là 13 có rất ít loại chu trình có thể có, chỉ có 101 ở kích thước 13. 

Sửa ba hoán vị. Xét một chu kỳ có độ dài (a), một chu kỳ có độ dài (b) và một chu kỳ có độ dài (c). Hành động sản phẩm của họ trên khối ô tương ứng có 

[ 
\frac{abc}{\operatorname{lcm}(a,b,c)} 
] 

quỹ đạo tế bào. Do đó, nếu ba hoán vị có danh sách chu trình (A,B,C), tổng số quỹ đạo của ô là 

\sum_{a\in A}\sum_{b\in B}\sum_{c\in C} 
\frac{abc}{\operatorname{lcm}(a,b,c)}. 
] 

Phần khó hơn là đếm những lựa chọn chuyển đổi lát cắt nào cho phép tồn tại một mảng cố định. Điều này trở thành một hệ thống tuyến tính trên (\mathbb F_2). Đối với mỗi chu kỳ hoán vị, chỉ có tính chẵn lẻ của các chuyển đổi trên chu kỳ đó mới quan trọng. Số lượng nhiệm vụ chuyển đổi thực tế tương ứng với tính chẵn lẻ của chu kỳ đã chọn đã được đưa vào tổng kích thước của hệ thống này. 

Đối với quỹ đạo ô đến từ độ dài chu kỳ (a,b,c), hãy để (L=\operatorname{lcm}(a,b,c)). Phương trình nhất quán tương ứng chứa tính chẵn lẻ của chu kỳ (a) chính xác (L/a). Modulo hai, hệ số này chính xác bằng 1 khi (L/a) là số lẻ. Điều đó xảy ra chính xác khi chu trình (a) có lũy thừa lớn nhất bằng 2 chia độ dài của nó cho (a,b,c). 

Điều này có nghĩa là đại số tuyến tính không phụ thuộc vào độ dài chu trình đầy đủ. Nó chỉ phụ thuộc vào việc định giá 2 adic của họ. Ở mức định giá cố định (v), gọi (x,y,z) là số chu kỳ định giá chính xác (v) trong ba hoán vị. Các ràng buộc ở mức định giá này là độc lập với tất cả các mức định giá khác. Nếu cả ba chiều đều có các chu kỳ có thể sử dụng cho đến mức định giá (v), thì thứ hạng đóng góp ở cấp độ này là (x+y+z-2) khi cả ba chiều đều có chu kỳ định giá (v), (x+y-1) khi có chính xác hai chiều và số chu kỳ khi chỉ có một chiều. 

Hai mảnh bây giờ kết hợp sạch sẽ. Đối với bộ ba loại chu trình hoán vị cố định, tổng các mảng cố định trên tất cả các lựa chọn chuyển đổi lát cắt là 

[ 
2^{m+n+p-r}\cdot 2^{Q}, 
] 

trong đó (r) là thứ hạng của các ràng buộc chẵn lẻ và (Q) là số lượng quỹ đạo ô. 

Cách tiếp cận bạo lực có hiệu quả vì nó thể hiện rõ ràng mọi quỹ đạo. Nó thất bại vì có rất nhiều mảng về mặt thiên văn. Burnside cho phép chúng tôi đếm tất cả các quỹ đạo đó cùng một lúc, trong khi các loại chu trình nén không gian hoán vị từ nhiều hoán vị giai thừa xuống tối đa 101 phân vùng trên mỗi chiều. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(2^{mnp})) hoặc tệ hơn | (O(2^{mnp})) | Quá chậm | 
| Burnside với các loại chu kỳ | (O(P(m)P(n)P(p)\cdot 13)) trên mỗi bộ ba chiều riêng biệt | (O(P(13)^2\cdot13)) phụ trợ | Đã chấp nhận | 

Ở đây (P(k)) biểu thị số phân vùng nguyên của (k), với (P(13)=101). Vì câu trả lời là đối xứng theo ba chiều nên việc triển khai sẽ sắp xếp mọi chiều theo bộ ba trước khi lưu vào bộ nhớ đệm. Chỉ có thể có (\binom{15}{3}=455) bộ ba được sắp xếp. 

## Hướng dẫn thuật toán

1. Tạo mọi phân vùng số nguyên của mọi số từ 1 đến 13. Đối với mỗi phân vùng, hãy lưu trữ độ dài chu kỳ của nó, số hoán vị có loại chu kỳ đó và số lượng chu kỳ có mỗi giá trị 2-adic. 

Một phân vùng như (1+1+2) biểu thị các hoán vị chứa hai điểm cố định và một 2 chu trình. Tính đa dạng của nó là 

[ 
\frac{3!}{1^2,2^1,2!,1!}=3. 
] 
2. Đối với mỗi cặp loại chu trình cần thiết, hãy tính toán trước 

\sum_{a,b} 
\text{cnt__A[a]\text{cnt__B[b] 
\frac{abc}{\operatorname{lcm}(a,b,c)} 
] 

cho mọi độ dài chu kỳ thứ ba có thể có (c). 

Khi đã biết vectơ này, tổng số quỹ đạo tế bào cho loại chu kỳ thứ ba chỉ là 

[ 
Q=\sum_c \text{cnt__C[c]S[c]. 
] 

Điều này tránh việc đánh giá nhiều lần các hoạt động gcd và lcm bên trong vòng lặp trong cùng. 
3. Đối với bộ ba loại chu kỳ, hãy tính thứ hạng của các phương trình nhất quán chuyển đổi một cách độc lập để định giá (v=0,1,2,3). Không có độ dài chu kỳ nào nhiều nhất là 13 có giá trị 2-adic lớn hơn 3. 

Khi định giá (v), trước tiên hãy kiểm tra xem mỗi chiều có chứa tối đa một chu kỳ định giá hay không (v). Nếu không thì không có quỹ đạo ô nào có thể có (v) là giá trị tối đa của nó. Nếu cấp độ đang hoạt động, hãy đếm xem có bao nhiêu thứ nguyên có ít nhất một chu kỳ định giá chính xác (v) và thêm đóng góp xếp hạng tương ứng. 
4. Đối với mỗi bộ ba loại chu trình hoán vị, nhân ba bội số hoán vị của nó với 

[ 
2^{m+n+p-r+Q}. 
] 

Số mũ (m+n+p-r) tính tất cả các phép gán chuyển đổi lát cắt thỏa mãn các ràng buộc chẵn lẻ, trong khi (Q) tính các lựa chọn nhị phân cho các ô trong quỹ đạo ô kết quả. 
5. Tính tổng các khoản đóng góp này cho tất cả các bộ ba thuộc loại chu kỳ. 
6. Chia tổng Burnside cho 

[ 
m!,n!,p!,2^{m+n+p}. 
] 

Có một sự tinh tế nhỏ ở đây. Chuyển đổi lát cắt có hạt nhân hai chiều: việc chuyển đổi từng lát cắt theo hai chiều không tạo ra thay đổi nào. Do đó, nhóm chuyển đổi thực tế có kích thước (2^{m+n+p-2}). Tính toán của chúng tôi tính tổng một cách có chủ ý trên tất cả (2^{m+n+p}) mô tả chuyển đổi, do đó mọi phần tử nhóm thực tế sẽ xuất hiện bốn lần. Hệ số 4 tương tự xuất hiện ở tử số, đó là lý do tại sao việc chia cho (2^{m+n+p}) là hoàn toàn chính xác. 
7. Sắp xếp ba chiều trước khi đánh giá câu trả lời và lưu trữ kết quả. Các phép toán xử lý ba trục một cách đối xứng, do đó, điều này không làm thay đổi câu trả lời và tránh lặp lại công việc trên các hoán vị có cùng chiều. 

### Tại sao nó hoạt động 

Đối với mỗi phần tử nhóm, Burnside yêu cầu số lượng mảng cố định chính xác. Phần hoán vị chia các ô thành các quỹ đạo độc lập. Một mảng được cố định bởi phần tử phải không đổi theo mức chẵn lẻ chuyển đổi được quy định dọc theo mỗi quỹ đạo như vậy, đưa ra các lựa chọn (2^Q) khi các mức tương đương chuyển đổi nhất quán. 

Các điều kiện nhất quán chỉ phụ thuộc vào một chu trình thông qua tính chẵn lẻ của các chuyển đổi lát cắt của nó. Hệ số của tính chẵn lẻ đó chính xác là số lẻ đối với các chu kỳ có giá trị 2-adic đạt giá trị tối đa trong bộ ba tương ứng. Do đó, tất cả các ràng buộc có cùng mức định giá tối đa tạo thành một hệ thống tuyến tính riêng biệt và hạng của nó là hạng chẵn lẻ hai bên hoặc ba bên hoàn chỉnh được mô tả ở trên. 

Do đó, mỗi bộ ba hoán vị đóng góp chính xác giá trị được thuật toán sử dụng. Tổng tất cả các hoán vị thông qua bội số kiểu chu kỳ của chúng sẽ cho ra tử số Burnside hoàn chỉnh và phép chia cuối cùng cho chính xác số lớp tương đương. 

## Giải pháp Python```python
import sys
from math import gcd
from functools import lru_cache

input = sys.stdin.readline

MOD = 998244353
MAXN = 13
MAXCELLS = MAXN * MAXN * MAXN

fact = [1] * (MAXN + 1)
for i in range(1, MAXN + 1):
    fact[i] = fact[i - 1] * i % MOD

invfact = [1] * (MAXN + 1)
invfact[MAXN] = pow(fact[MAXN], MOD - 2, MOD)
for i in range(MAXN, 0, -1):
    invfact[i - 1] = invfact[i] * i % MOD

pow2 = [1] * (MAXCELLS + 1)
for i in range(1, MAXCELLS + 1):
    pow2[i] = pow2[i - 1] * 2 % MOD

invpow2 = [1] * (MAXCELLS + 1)
inv2 = (MOD + 1) // 2
for i in range(1, MAXCELLS + 1):
    invpow2[i] = invpow2[i - 1] * inv2 % MOD

def v2(x):
    return (x & -x).bit_length() - 1

# orbit3[a][b][c] is the number of cell orbits produced
# by one a-cycle, one b-cycle and one c-cycle.
orbit3 = [[[0] * (MAXN + 1) for _ in range(MAXN + 1)]
          for _ in range(MAXN + 1)]

for a in range(1, MAXN + 1):
    for b in range(1, MAXN + 1):
        ab = a * b // gcd(a, b)
        for c in range(1, MAXN + 1):
            l = ab * c // gcd(ab, c)
            orbit3[a][b][c] = a * b * c // l

class CycleType:
    __slots__ = ("n", "parts", "cnt", "vcnt", "weight", "cum")

    def __init__(self, n, parts, cnt):
        self.n = n
        self.parts = parts
        self.cnt = cnt

        vcnt = [0] * 4
        for length, number in cnt.items():
            vcnt[v2(length)] += number
        self.vcnt = tuple(vcnt)

        denom = 1
        for length, number in cnt.items():
            for _ in range(number):
                denom *= length
            denom *= fact[number]
        self.weight = fact[n] * pow(denom, MOD - 2, MOD) % MOD

        s = 0
        cum = []
        for x in vcnt:
            s += x
            cum.append(s)
        self.cum = tuple(cum)

def make_cycle_types(n):
    result = []

    def dfs(rem, last, parts):
        if rem == 0:
            cnt = {}
            for x in parts:
                cnt[x] = cnt.get(x, 0) + 1
            result.append(CycleType(n, tuple(parts), cnt))
            return

        for x in range(last, rem + 1):
            parts.append(x)
            dfs(rem - x, x, parts)
            parts.pop()

    dfs(n, 1, [])
    return result

types = [None] * (MAXN + 1)
for n in range(1, MAXN + 1):
    types[n] = make_cycle_types(n)

pair_orbit_cache = {}

def get_pair_orbits(A, B):
    key = (id(A), id(B))
    if key in pair_orbit_cache:
        return pair_orbit_cache[key]

    s = [0] * (MAXN + 1)

    for a in A.parts:
        ca = A.cnt[a]
        for b in B.parts:
            cb = B.cnt[b]
            mul = ca * cb
            row = orbit3[a][b]
            for c in range(1, MAXN + 1):
                s[c] += mul * row[c]

    pair_orbit_cache[key] = tuple(s)
    return pair_orbit_cache[key]

def rank_of(A, B, C):
    rank = 0

    for v in range(4):
        av = A.vcnt[v]
        bv = B.vcnt[v]
        cv = C.vcnt[v]

        if av == 0 and bv == 0 and cv == 0:
            continue

        # Every dimension must have some cycle of valuation <= v,
        # otherwise v cannot be the maximum valuation of a cell orbit.
        if A.cum[v] == 0 or B.cum[v] == 0 or C.cum[v] == 0:
            continue

        active_dimensions = (av > 0) + (bv > 0) + (cv > 0)
        total = av + bv + cv

        rank += total - active_dimensions + 1

    return rank

@lru_cache(maxsize=None)
def solve_dimension(n, m, p):
    n, m, p = sorted((n, m, p))

    total = 0

    for A in types[n]:
        wa = A.weight

        for B in types[m]:
            wb = B.weight
            s = get_pair_orbits(A, B)
            wab = wa * wb % MOD

            for C in types[p]:
                q = 0
                for c in C.parts:
                    q += C.cnt[c] * s[c]

                r = rank_of(A, B, C)

                contribution = pow2[n + m + p - r + q]
                contribution = contribution * wab % MOD
                contribution = contribution * C.weight % MOD

                total += contribution

    total %= MOD

    denominator_inverse = (
        invfact[n]
        * invfact[m]
        % MOD
        * invfact[p]
        % MOD
        * invpow2[n + m + p]
        % MOD
    )

    return total * denominator_inverse % MOD

def main():
    T = int(input())
    out = []

    for _ in range(T):
        n, m, p = map(int, input().split())
        out.append(str(solve_dimension(n, m, p)))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    main()
```Mảng giai thừa xử lý tính đa dạng của các loại chu trình hoán vị. Đối với loại chu trình có (c_l) chu kỳ có độ dài (l), bội số của nó là 

[ 
\frac{n!}{\prod_l l^{c_l}c_l!}. 
] 

các`CycleType`đối tượng lưu trữ chính xác thông tin mà Burnside cần: độ dài chu kỳ, bội số của chúng, trọng số hoán vị của nó và số chu kỳ ở mỗi lần định giá 2 adic. 

các`orbit3`table loại bỏ công việc gcd và lcm khỏi bảng liệt kê trong cùng. Giá trị của nó là số lượng quỹ đạo bên trong tích của ba chu kỳ hoán vị riêng lẻ, là tích của kích thước của chúng chia cho lcm của các kích thước đó.`get_pair_orbits`sau đó kết hợp hai loại chu trình thành một vectơ nhỏ gọn. Khi vectơ này đã được tính toán, việc chọn loại chu kỳ thứ ba chỉ yêu cầu tổng có trọng số ngắn.`rank_of`cố tình làm việc với số lượng định giá tích lũy. Một mức định giá chỉ có thể đóng góp các ràng buộc khi mỗi chiều có ít nhất một chu kỳ có mức định giá không lớn hơn mức đó. biểu hiện`total - active_dimensions + 1`đưa ra thứ hạng của một hệ thống chẵn lẻ một, hai hoặc ba bên hoàn chỉnh. 

Số nguyên Python không bị tràn, nhưng mọi giá trị tổ hợp đều được giảm theo modulo`998244353`sau khi nhân. Số mũ của`pow2`nhiều nhất là (mnp+m+n+p), nằm trong phạm vi được tính toán trước. 

Hệ số nghịch đảo cuối cùng chứa`invpow2[n+m+p]`, không`invpow2[n+m+p-2]`. Mã này liệt kê tất cả các mô tả chuyển đổi lát cắt, bao gồm bốn mô tả đại diện cho danh tính trong nhóm con chuyển đổi lát cắt. Sự dư thừa gấp bốn lần đó là có chủ ý và triệt tiêu hệ số tương ứng trong tử số Burnside. 

## Ví dụ đã hoạt động 

### Mẫu 1:`1 1 1`Chỉ có một loại chu trình ở mọi chiều, cụ thể là một chu trình có độ dài 1. 

| Kích thước | Loại chu kỳ | Trọng lượng hoán vị | số lượng 2 adic | 
| --- | --- | --- | --- | 
| 1 |`[1]`| 1 | (v_2=0:1) | 
| 1 |`[1]`| 1 | (v_2=0:1) | 
| 1 |`[1]`| 1 | (v_2=0:1) | 

Ô đơn là một quỹ đạo ô, vì vậy (Q=1). Ở mức định giá bằng 0, cả ba chiều đều tham gia, xếp hạng (1+1+1-2=1). 

| Số lượng | Giá trị | 
| --- | --- | 
| (m+n+p) | 3 | 
| (Q) | 1 | 
| xếp hạng | 1 | 
| tổng mảng cố định | (2^{3-1+1}=8) | 
| mẫu số | (2^3=8) | 
| trả lời | 1 | 

Kết quả là`1`, bởi vì hai mảng ô đơn có thể chỉ khác nhau bằng cách chuyển đổi. 

### Mẫu 2:`2 2 2`Đối với kích thước 2, hai loại chu trình hoán vị là`[1,1]`Và`[2]`, mỗi số có bội số là 1. 

| Chiều thứ nhất | Chiều thứ hai | Chiều thứ ba | Tác dụng chính | 
| --- | --- | --- | --- | 
|`[1,1]`|`[1,1]`|`[1,1]`| Tất cả các chu kỳ đều có giá trị 0 | 
|`[1,1]`|`[1,1]`|`[2]`| Chiều thứ ba có giá trị lớn hơn | 
|`[1,1]`|`[2]`|`[1,1]`| Chiều thứ hai có giá trị lớn hơn | 
|`[1,1]`|`[2]`|`[2]`| Hai chu kỳ 2 chia sẻ mức định giá lớn nhất | 
|`[2]`|`[1,1]`|`[1,1]`| Đối xứng với hàng thứ hai | 
|`[2]`|`[1,1]`|`[2]`| Đối xứng với hàng thứ tư | 
|`[2]`|`[2]`|`[1,1]`| Đối xứng với hàng thứ tư | 
|`[2]`|`[2]`|`[2]`| Cả ba chiều đều có giá trị 1 | 

Thuật toán đánh giá tám bộ ba loại chu kỳ này, nhân mỗi số lượng mảng cố định với bội số hoán vị của nó và chia tổng của chúng cho 

[ 
2^{6}(2!)^3. 
] 

Số quỹ đạo thu được là`9`, phù hợp với mẫu chính thức. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(P(n)P(m)P(p)\cdot13)) trên mỗi bộ ba thứ nguyên không được lưu vào bộ đệm | Mỗi bộ ba loại chu kỳ cần tổng quỹ đạo ngắn và bốn mức định giá | 
| Không gian | (O(P(13)^2\cdot13)) | Các vectơ quỹ đạo cặp được lưu trong bộ nhớ cache cộng với tất cả dữ liệu phân vùng | 

Vì (P(13)=101), ngay cả trường hợp riêng lẻ lớn nhất cũng có nhiều nhất (101^3) bộ ba loại chu kỳ. Việc sắp xếp các thứ nguyên làm cho câu trả lời trở nên đối xứng và bộ nhớ đệm ngăn chặn công việc lặp lại đối với các bộ ba thứ nguyên giống hệt nhau. Các ràng buộc này đủ nhỏ để các phân vùng số nguyên thay thế việc liệt kê các hoán vị và mảng không thể thực hiện được. 

## Trường hợp thử nghiệm```python
# This test harness uses the solve_dimension function from the solution above.

def run(inp: str) -> str:
    data = list(map(int, inp.split()))
    t = data[0]
    pos = 1
    ans = []

    for _ in range(t):
        n, m, p = data[pos], data[pos + 1], data[pos + 2]
        pos += 3
        ans.append(str(solve_dimension(n, m, p)))

    return "\n".join(ans)

# Official samples
assert run("""1
1 1 1
""") == "1", "sample 1"

assert run("""1
2 2 2
""") == "9", "sample 2"

assert run("""1
2 3 4
""") == "723", "sample 3"

# Minimum size
assert run("""1
1 1 1
""") == "1", "minimum dimensions"

# Maximum allowed dimension, with the other two dimensions minimal.
# Every layer can be toggled independently, so all arrays are equivalent.
assert run("""1
1 1 13
""") == "1", "maximum single dimension"

# Boundary case where the answer is not 1.
# This exposes the invariant given by the parity of all four cells.
assert run("""1
1 2 2
""") == "2", "2x2 matrix parity"

# Another one-dimensional boundary case.
assert run("""1
1 1 2
""") == "1", "independent layer toggles"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1`|`1`| Mảng kích thước tối thiểu và chuyển đổi tương đương hoàn toàn | 
|`1 1 13`|`1`| Chuyển đổi lớp độc lập và ràng buộc kích thước tối đa | 
|`1 2 2`|`2`| Bất biến chẵn lẻ không tầm thường khi một chiều là 1 | 
|`1 1 2`|`1`| Chuyển đổi độc lập các lớp riêng lẻ | 
|`2 2 2`|`9`| Tương tác giữa các hoán vị, chuyển đổi và xếp hạng 2 adic | 

## Vỏ cạnh 

cho`1 1 1`, thuật toán có một loại chu trình ở mọi chiều. Số quỹ đạo ô của nó là (1), thứ hạng chẵn lẻ của nó là (1) và tử số Burnside là (2^{3-1+1}=8). Mẫu số cũng là 8 nên kết quả chính xác`1`. 

Vì`1 1 2`, hai chiều đầu tiên chứa một chu trình 1 và chiều thứ ba có hai chu trình 1 hoặc một chu trình 2. Các thao tác chuyển đổi trên hai lớp riêng lẻ đã kết nối tất cả bốn mảng nhị phân. Phép tính Burnside cho một quỹ đạo, vì vậy câu trả lời là`1`. Trường hợp này nắm bắt các triển khai coi hoán vị lớp là thao tác duy nhất. 

Vì`1 2 2`, hai chiều không tầm thường tạo thành ma trận nhị phân (2\times2). Việc chuyển đổi một hàng hoặc một cột sẽ thay đổi chính xác hai ô, do đó tính chẵn lẻ của tổng số ô không thể thay đổi. Việc hoán đổi hàng hoặc cột cũng duy trì tính chẵn lẻ này. Cả hai giá trị chẵn lẻ đều xảy ra và mọi ma trận có chẵn lẻ cố định đều tương đương với các phép toán được phép, đưa ra chính xác hai lớp. Việc tính toán kiểu chu trình tạo ra`2`. 

Đối với kích thước đơn tối đa được phép,`1 1 13`, mỗi lớp trong số 13 lớp có thể được chuyển đổi độc lập. Bắt đầu từ bất kỳ vectơ nhị phân nào, chuyển đổi chính xác các lớp chứa một, thu được mảng hoàn toàn bằng 0. Do đó mọi mảng đều tương đương với mọi mảng khác và câu trả lời vẫn là`1`. Thuật toán xử lý việc này mà không cần có trường hợp đặc biệt vì thứ hạng Burnside tự động tính đến tất cả các chuyển đổi lát cắt độc lập. 

Vụ án`2 2 2`là việc kiểm tra độ tỉnh táo hữu ích cho toàn bộ máy móc. Nó chứa cả chu kỳ có độ dài lẻ và chu kỳ có độ dài chẵn, do đó, sự khác biệt giữa việc định giá 2 adic rất quan trọng. Chỉ sử dụng số chu kỳ mà không theo dõi xem độ dài của chúng là số lẻ hay số chẵn sẽ đưa ra số lượng mảng cố định sai. Việc tính toán xếp hạng dựa trên định giá chính xác là yếu tố phân biệt các trường hợp này và đưa ra câu trả lời đúng`9`.
