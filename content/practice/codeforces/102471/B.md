---
title: "CF 102471B - Đen và Trắng"
description: "Chúng ta có một bàn cờ (ntimes m) có ô ((i,j)) có giá trị (+1) khi (i+j) chẵn và (-1) nếu ngược lại. Một đường dẫn hợp lệ bao gồm chính xác (n) bước về phía bắc và (m) bước về phía đông, bắt đầu từ góc dưới bên trái và kết thúc ở góc trên bên phải."
date: "2026-08-12T08:56:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102471
codeforces_index: "B"
codeforces_contest_name: "2019 ICPC Asia-East Continent Final"
rating: 0
weight: 102471
solve_time_s: 731
verified: true
draft: false
---

[CF 102471B - Đen và Trắng](https://codeforces.com/problemset/problem/102471/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 12m 11s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một bàn cờ (n\times m) có ô ((i,j)) có giá trị (+1) khi (i+j) chẵn và (-1) nếu ngược lại. Một đường dẫn hợp lệ bao gồm chính xác (n) bước về phía bắc và (m) bước về phía đông, bắt đầu từ góc dưới bên trái và kết thúc ở góc trên bên phải. 

Các ô ở phía bên trái của đường dẫn được chỉ dẫn đóng góp giá trị của chúng vào điểm số. Chúng ta cần đếm xem có bao nhiêu đường đi có điểm chính xác (k), modulo (998244353). 

Đầu vào chứa tối đa (100) trường hợp thử nghiệm độc lập. Cả hai thứ nguyên đều có thể là (10^5), do đó, giải pháp tùy thuộc vào (nm) đã quá lớn: một trường hợp thử nghiệm có thể chứa (10^{10}) ô. Việc liệt kê tất cả các con đường lại càng vô vọng hơn vì số lượng của chúng là 

[ 
\binom{n+m}{n}, 
] 

đó là rất lớn ngay cả đối với kích thước vừa phải. Thuật toán hữu ích phải xử lý một trường hợp kiểm thử trong thời gian cơ bản không đổi sau khi tính toán trước giai thừa toàn cục. 

Có một số trường hợp ranh giới trong đó việc triển khai dựa trên phạm vi điểm đoán có thể sai. Với (n=m=1), hai đường dẫn có điểm (1) và (0), do đó đầu vào`1 1 0`có câu trả lời (1), trong khi`1 1 -1`có câu trả lời (0). Một công thức giả định điểm số đối xứng quanh 0 sẽ thất bại ở đây. 

Tính chẵn lẻ của (n+m) cũng có vấn đề. Vì`2 3 1`, câu trả lời đúng là (1), trong khi với`2 3 -1`đó là (3). Hai bên không đối xứng khi tổng chiều dài đường đi là số lẻ. Cuối cùng, một số điểm không tưởng như`2 2 2`có câu trả lời (0) và cách triển khai an toàn nhất sẽ tự động lấy được câu trả lời này từ các hệ số nhị thức không hợp lệ thay vì xử lý từng ranh giới điểm một cách riêng biệt. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực trực tiếp có thể liệt kê mọi con đường. Có (\binom{n+m}{n}) đường dẫn như vậy. Ngay cả khi chúng tôi tính điểm của một đường dẫn chỉ trong thời gian (O(n+m)), tổng công việc là 

[ 
O\left((n+m)\binom{n+m}{n}\right). 
] 

Tại (n=m=100000), đây là khoảng (200000\binom{200000}{100000}) hoạt động, vượt xa mọi giới hạn khả thi. Việc tính điểm theo từng ô thậm chí còn tệ hơn nếu thêm một hệ số khác là (nm). 

Một quan sát hữu ích là việc tô màu bàn cờ làm cho điểm số chỉ phụ thuộc vào sự ngang bằng về vị trí của các bậc thang phía bắc trong từ đường dẫn. 

Viết đường dẫn dưới dạng một chuỗi (n+m) ký tự, với`N`cho một bước về phía bắc và`E`cho một bước về phía đông. Đánh số các vị trí này bắt đầu từ (1). Gọi (A) là số bậc thang về phía bắc xuất hiện ở các vị trí chẵn. 

Bản sắc trung tâm là 

[ 
\boxed{\text{score}=A-\left\lfloor\frac n2\right\rfloor}. 
] 

Một khi điều này được biết đến, hình học sẽ biến mất. có 

[ 
L_1=\left\lfloor\frac{n+m}{2}\right\rfloor 
] 

vị trí chẵn và 

[ 
L_2=\left\lceil\frac{n+m}{2}\right\rceil 
] 

vị trí lẻ trong từ đường dẫn. Nếu điểm là (k) thì số bước bắc ở vị trí chẵn phải là 

[ 
A=\left\lfloor\frac n2\right\rfloor+k. 
] 

Phần còn lại 

[ 
n-A=\left\lceil\frac n2\right\rceil-k 
] 

bậc thang phía bắc phải chiếm vị trí lẻ. Các lựa chọn là độc lập, đưa ra 

[ 
\đóng hộp{ 
\binom{\lfloor(n+m)/2\rfloor} 
{\lfloor n/2\rfloor+k} 
\binom{\lceil(n+m)/2\rceil} 
{\lceil n/2\rceil-k} 
}. 
] 

Giai thừa và giai thừa nghịch đảo có thể được tính toán trước một lần lên tới (200000), tạo ra mọi trường hợp thử nghiệm (O(1)). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O((n+m)\binom{n+m}{n})) | (O(n+m)) | Quá chậm | 
| Tối ưu | (O(1)) mỗi lần kiểm tra sau khi tính toán trước | (O(n+m)) tổng cộng | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Biểu thị đường dẫn dưới dạng chữ của (n)`N`bước và (m)`E`các bước. Mỗi đường đơn điệu tương ứng với chính xác một từ như vậy, do đó việc đếm các đường dẫn tương đương với việc đếm các từ này. 
2. Với mỗi hàng (i), gọi (c_i) là cột nơi đường đi đi theo hướng bắc từ hàng (i) đến hàng (i+1). Các ô ở bên trái của bước dọc đó chính xác là các ô có chỉ số cột (0,1,\ldots,c_i-1). 

Đóng góp có chữ ký của họ là 

[ 
\sum_{j=0}^{c_i-1}(-1)^{i+j}. 
] 

Tổng này bằng 0 khi (c_i) là số chẵn. Khi (c_i) lẻ, nó bằng ((-1)^i). 
3. Đánh số các bậc thang phía bắc theo thứ tự xuất hiện của chúng. Đặt bước bắc thứ (q)-th xảy ra ở vị trí (t_q) trong từ đường dẫn đầy đủ. Trước bước đó có (q-1) bước về phía bắc nên cột của nó là 

[ 
c_{q-1}=t_q-q. 
] 

Nếu cột này là số lẻ thì (t_q) và (q) có tính chẵn lẻ ngược nhau. Sự đóng góp của bước phía bắc này là ((-1)^{q-1}). 
4. Gọi (A) là số bậc thang về phía bắc ở vị trí chẵn. Những đóng góp tích cực chính xác là các bước về phía bắc với chỉ số lẻ (q) ở vị trí chẵn. Các đóng góp âm chính xác là các bước hướng bắc với chỉ số chẵn (q) ở vị trí lẻ. 

Trong số các bước phía bắc (\lfloor n/2\rfloor) được lập chỉ mục chẵn, mỗi bước đều ở vị trí chẵn hoặc lẻ. Do đó 

## #(\text{odd }q,\ t_q\text{ chẵn}) 

# #(\text{chẵn }q,\ t_q\text{ lẻ}) 

A-\left\lfloor\frac n2\right\rfloor. 
] 

Danh tính này là bất biến chính của toàn bộ giải pháp. 
5. Với điểm yêu cầu (k), hãy giải đẳng thức cho (A): 

[ 
A=\left\lfloor\frac n2\right\rfloor+k. 
] 

Có (\lfloor(n+m)/2\rfloor) vị trí chẵn, nên có 

[ 
\binom{\lfloor(n+m)/2\rfloor} 
{\lfloor n/2\rfloor+k} 
] 

cách chọn vị trí chẵn của các bậc thang phía bắc. 
6. Có (\lceil(n+m)/2\rceil) vị trí lẻ. Các bậc thang phía bắc (n-A) còn lại phải chiếm các vị trí này. Kể từ khi 

[ 
n-A=\left\lceil\frac n2\right\rceil-k, 
] 

có 

[ 
\binom{\lceil(n+m)/2\rceil} 
{\lceil n/2\rceil-k} 
] 

sự lựa chọn. 
7. Nhân hai hệ số nhị thức theo modulo (998244353). Nếu một trong hai đối số thấp hơn nằm ngoài phạm vi hợp lệ của nó thì hệ số nhị thức tương ứng bằng 0, do đó, các điểm không thể đạt được sẽ không cần có trường hợp đặc biệt. 

### Tại sao nó hoạt động 

Hàng chứa mỗi bước về phía bắc đưa ra mô tả trực tiếp về những ô trong hàng đó nằm ở bên trái. Vì các màu của ô xen kẽ nhau nên tổng có dấu của hàng đó bằng 0 đối với cột chẵn và chính xác là (+1) hoặc (-1) đối với cột lẻ. Biểu thị cột của bước bắc (q)-th dưới dạng (t_q-q) sẽ chuyển đổi điều kiện này thành mối quan hệ chẵn lẻ giữa chỉ số bước bắc và vị trí tuyệt đối của nó trong từ đường dẫn. 

Sau sự chuyển đổi này, mọi đóng góp vào điểm số chỉ được xác định bằng việc bước phía bắc đó chiếm vị trí chẵn hay lẻ. Tổng số điểm chính xác là số (A) của các bước về phía bắc ở vị trí chẵn trừ (\lfloor n/2\rfloor). Khi (A) được cố định, việc lựa chọn vị trí bước bắc ở phần chẵn và lẻ của từ là độc lập. Hai hệ số nhị thức tính chính xác những lựa chọn đó, vì vậy mọi đường dẫn hợp lệ sẽ được tính một lần và không có đường dẫn không hợp lệ nào được tính. 

## Giải pháp Python```python
import sys

input = sys.stdin.readline

MOD = 998244353

def prepare(max_n):
    fact = [1] * (max_n + 1)
    for i in range(1, max_n + 1):
        fact[i] = fact[i - 1] * i % MOD

    invfact = [1] * (max_n + 1)
    invfact[max_n] = pow(fact[max_n], MOD - 2, MOD)
    for i in range(max_n, 0, -1):
        invfact[i - 1] = invfact[i] * i % MOD

    return fact, invfact

def comb(n, r, fact, invfact):
    if r < 0 or r > n:
        return 0
    return fact[n] * invfact[r] % MOD * invfact[n - r] % MOD

def main():
    T = int(input())
    tests = [tuple(map(int, input().split())) for _ in range(T)]

    max_size = 0
    for n, m, _ in tests:
        max_size = max(max_size, n + m)

    fact, invfact = prepare(max_size)

    ans = []

    for n, m, k in tests:
        even_positions = (n + m) // 2
        odd_positions = (n + m + 1) // 2

        north_on_even = n // 2 + k
        north_on_odd = (n + 1) // 2 - k

        left = comb(even_positions, north_on_even, fact, invfact)
        right = comb(odd_positions, north_on_odd, fact, invfact)

        ans.append(str(left * right % MOD))

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    main()
```Dữ liệu đầu vào được đọc hoàn toàn trước khi xử lý trước nên mảng giai thừa chỉ cần được xây dựng lên đến giá trị lớn nhất (n+m) xuất hiện trong tệp thử nghiệm. Vì (n,m\leq100000), giới hạn này tối đa là (200000). 

các`comb`hàm trả về 0 một cách rõ ràng khi đối số dưới của nó âm hoặc lớn hơn đối số trên. Điều này xử lý các điểm nằm ngoài phạm vi có thể mà không cần các nhánh bổ sung trong thuật toán chính. 

Đối với mỗi trường hợp thử nghiệm,`even_positions`Và`odd_positions`là số vị trí (2,4,\ldots) và (1,3,\ldots) trong một từ đường dẫn có độ dài (n+m). Các biến`north_on_even`Và`north_on_odd`đến trực tiếp từ danh tính điểm số. Tổng của chúng chính xác là (n), đây là một phép kiểm tra độ tỉnh táo hữu ích. 

Không có vấn đề tràn số nguyên trong Python. Mọi phép nhân đều được rút gọn modulo (998244353) và nghịch đảo môđun thu được bằng định lý Fermat vì môđun là số nguyên tố. 

## Ví dụ đã hoạt động 

### Mẫu 1:`1 1 0`Đường đi có hai bước nên có một vị trí chẵn và một vị trí lẻ. Chúng ta cần các giá trị sau. 

| Biến | Giá trị | 
| --- | --- | 
| (n) | 1 | 
| (m) | 1 | 
| (k) | 0 | 
| vị trí chẵn | 1 | 
| vị trí lẻ | 1 | 
| bắc ở các vị trí chẵn | (0+0=0) | 
| bắc ở vị trí lẻ | (1-0=1) | 
| trả lời | (\binom10\binom11=1) | 

Con đường duy nhất có điểm 0 là`NE`. Bước bắc của nó ở vị trí (1), do đó không có bước bắc nào ở vị trí chẵn. Nhận dạng điểm cho (0-\lfloor1/2\rfloor=0). 

Dấu vết thể hiện hành vi không đối xứng gây ra bởi tổng số bước lẻ. 

### Mẫu 2:`1 1 -1`Các giá trị cấu trúc không thay đổi nhưng hiện tại điểm được yêu cầu là (-1). 

| Biến | Giá trị | 
| --- | --- | 
| (n) | 1 | 
| (m) | 1 | 
| (k) | -1 | 
| vị trí chẵn | 1 | 
| vị trí lẻ | 1 | 
| bắc ở các vị trí chẵn | (0-1=-1) | 
| bắc ở vị trí lẻ | (1-(-1)=2) | 
| trả lời | (\binom1{-1}\binom12=0) | 

Cả hai hệ số nhị thức bắt buộc đều không hợp lệ. Do đó câu trả lời là số không. Điều này phù hợp với thực tế là bảng (1\times1) không thể có điểm (-1). 

### Mẫu 3:`2 2 1`Có bốn vị trí, mỗi vị trí có hai vị trí chẵn lẻ. 

| Biến | Giá trị | 
| --- | --- | 
| (n) | 2 | 
| (m) | 2 | 
| (k) | 1 | 
| vị trí chẵn | 2 | 
| vị trí lẻ | 2 | 
| bắc ở các vị trí chẵn | (1+1=2) | 
| bắc ở vị trí lẻ | (1-1=0) | 
| trả lời | (\binom22\binom20=1) | 

Con đường duy nhất được tính ở đây là`NENE`. Cả hai bước phía bắc đều chiếm vị trí chẵn, cho (A=2) và điểm (2-1=1). 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(M+T)) | Giai thừa được tính toán trước lên tới (M=\max(n+m)), sau đó mỗi trường hợp kiểm thử sẽ mất thời gian không đổi | 
| Không gian | (O(M)) | Mỗi mảng giai thừa và nghịch đảo giai thừa đều chứa các giá trị (M+1) | 

Giá trị lớn nhất có thể (M) là (200000), do đó quá trình tiền xử lý đủ nhỏ cho giới hạn bộ nhớ. Sau khi tiền xử lý, thậm chí (100) trường hợp thử nghiệm lớn chỉ yêu cầu một lượng công việc không đổi cho mỗi trường hợp. Không có lưới nào được xây dựng và không có đường dẫn riêng lẻ nào được liệt kê. 

## Trường hợp thử nghiệm```python
# Complete assert-based tests for the formula used by the solution.

MOD = 998244353

def build_fact(limit):
    fact = [1] * (limit + 1)
    for i in range(1, limit + 1):
        fact[i] = fact[i - 1] * i % MOD

    invfact = [1] * (limit + 1)
    invfact[limit] = pow(fact[limit], MOD - 2, MOD)
    for i in range(limit, 0, -1):
        invfact[i - 1] = invfact[i] * i % MOD

    return fact, invfact

FACT, INVFACT = build_fact(200000)

def C(n, r):
    if r < 0 or r > n:
        return 0
    return FACT[n] * INVFACT[r] % MOD * INVFACT[n - r] % MOD

def expected(n, m, k):
    l1 = (n + m) // 2
    l2 = (n + m + 1) // 2
    a = n // 2 + k
    b = (n + 1) // 2 - k
    return C(l1, a) * C(l2, b) % MOD

def run(inp: str) -> str:
    data = list(map(int, inp.split()))
    t = data[0]
    pos = 1
    out = []

    for _ in range(t):
        n, m, k = data[pos], data[pos + 1], data[pos + 2]
        pos += 3
        out.append(str(expected(n, m, k)))

    return "\n".join(out)

# Provided samples.
sample = """\
5
1 1 0
1 1 -1
2 2 1
2 2 0
4 4 1
"""
assert run(sample) == """\
1
0
1
4
16
""", "provided samples"

# Minimum-size and asymmetric odd-length cases.
assert run("1\n1 1 1\n") == "1", "1x1 maximum score"
assert run("1\n1 2 0\n") == "2", "1x2 zero score"
assert run("1\n1 2 1\n") == "1", "1x2 positive score"

# Catches the asymmetry between positive and negative scores.
assert run("3\n2 3 1\n2 3 0\n2 3 -1\n") == """\
1
6
3
""", "odd total path length"

# Impossible score.
assert run("1\n2 2 2\n") == "0", "score outside the possible range"

# Maximum-size test case.
max_expected = expected(100000, 100000, 0)
assert run("1\n100000 100000 0\n") == str(max_expected), \
    "maximum n and m"

# A maximum-size dimension with a very small other dimension.
assert run("1\n100000 1 0\n") == "50001", \
    "maximum n with m=1"

print("all tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1`|`1`| Bảng tối thiểu và điểm tối đa có thể đạt được | 
|`1 2 0`|`2`| Tổng chiều dài đường đi lẻ và điểm 0 | 
|`2 3 1`,`2 3 0`,`2 3 -1`|`1`,`6`,`3`| Điểm dương, điểm 0 và điểm âm với phân bố không đối xứng | 
|`2 2 2`|`0`| Điểm không hợp lệ và xử lý ranh giới nhị thức | 
|`100000 100000 0`|`C(100000,50000)^2 mod 998244353`| Cả hai chiều đều ở mức tối đa | 
|`100000 1 0`|`50001`| Kích thước lớn với một tấm ván mỏng | 

## Vỏ cạnh 

cho`1 1 0`, công thức cho (L_1=L_2=1), (A=0) và (B=1). Câu trả lời là (\binom10\binom11=1). con đường`NE`có bước bắc của nó ở vị trí lẻ thứ nhất, nên điểm của nó bằng 0. 

Vì`1 1 -1`, số bước bắc cần thiết ở vị trí chẵn là (-1). Vì hệ số nhị thức có đối số âm thấp hơn bằng 0 nên câu trả lời ngay lập tức là 0. Cơ chế tương tự xử lý mọi điểm không thể. 

Vì`2 3`, tổng chiều dài đường đi
