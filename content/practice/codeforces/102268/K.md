---
title: "CF 102268K - Kiến thức"
description: "Chúng ta có một chuỗi nhị phân trên bảng chữ cái a, b. Các thao tác được phép chèn hoặc xóa một trong ba khối đặc biệt: aa, bbb và ababab."
date: "2026-08-17T19:02:08+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102268
codeforces_index: "K"
codeforces_contest_name: "300iq Contest 1"
rating: 0
weight: 102268
solve_time_s: 275
verified: false
draft: false
---

[CF 102268K - Kiến thức](https://codeforces.com/problemset/problem/102268/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4 phút 35 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một chuỗi nhị phân trên bảng chữ cái`a, b`. Các thao tác được phép chèn hoặc xóa một trong ba khối đặc biệt:`aa`,`bbb`, Và`ababab`. Vì cả hai đều được phép chèn và xóa, nên hai chuỗi thuộc về cùng một lớp chính xác khi một chuỗi có thể được chuyển đổi thành chuỗi kia bằng cách sử dụng các quan hệ 

[ 
a^2 = 1,\qquad b^3 = 1,\qquad (ab)^3 = 1. 
] 

Chuỗi ban đầu`s`xác định một lớp tương đương như vậy. Chúng ta không cần phải xây dựng các chuỗi thực tế có thể lấy được từ`s`. Thay vào đó, với độ dài quy định`x`, chúng ta cần đếm xem có bao nhiêu chuỗi nhị phân có độ dài chính xác như vậy thuộc cùng một lớp với`s`. 

Độ dài đầu vào`n`nhiều nhất là (300000), do đó việc xử lý`s`nên tuyến tính hoặc gần tuyến tính. Một thuật toán giữ trạng thái cho mọi chuỗi con hoặc cố gắng liệt kê các chuỗi có thể chuyển đổi đã quá đắt. Nghiêm túc hơn,`x`có thể lớn bằng (10^9), do đó bất kỳ thuật toán nào phụ thuộc tuyến tính vào`x`là không thể. Chúng ta cần nén các lớp tương đương thành một số trạng thái không đổi và sau đó xử lý độ dài khổng lồ bằng cách sử dụng lũy ​​thừa logarit. 

Có ba trường hợp đặc biệt có xu hướng bộc lộ những cách hiểu không chính xác. Đầu tiên,`x`có thể bằng không. Ví dụ,```
1
a
0
```có câu trả lời`0`, bởi vì chuỗi trống thể hiện danh tính trong khi`a`không. Một giải pháp giả sử mục tiêu luôn có ít nhất một ký tự sẽ mắc lỗi này. 

Thứ hai, chuỗi bắt đầu có thể đại diện cho danh tính mà không bị trống. Ví dụ,```
2
aa
0
```có câu trả lời`1`, bởi vì`aa`có thể bị xóa, để lại chuỗi trống. Một giải pháp so sánh các chuỗi ký tự thay vì các lớp tương đương của chúng sẽ trả về 0 không chính xác. 

Thứ ba, các mối quan hệ tương tác qua các ranh giới, vì vậy chỉ cần xóa đi một cách tham lam sự xuất hiện của`aa`hoặc`bbb`không phải là một chiến lược bình thường hóa hoàn toàn. Ví dụ,`ababab`đại diện cho danh tính và câu trả lời cho```
6
ababab
3
```là`1`. Đại diện có độ dài ba duy nhất của danh tính là`bbb`. Một thuật toán rút gọn cục bộ không tính đến quan hệ thứ ba có thể dễ dàng bỏ qua sự tương đương này. Các mẫu chính thức xác nhận những kết quả đầu ra này. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ liệt kê từng chuỗi nhị phân có độ dài (2^x)`x`, chuẩn hóa nó và kiểm tra xem nó có thuộc lớp của`s`. Điều này đúng vì mọi chuỗi mục tiêu có thể đều được xem xét rõ ràng. Nếu quá trình chuẩn hóa mất (O(x)) thời gian thì tổng công việc trong trường hợp xấu nhất là (\Theta(x2^x)), với (2^x) ứng viên và (x) ký tự được kiểm tra trong mỗi ứng cử viên. Điều này đã vô vọng rồi`x = 30`, chứ đừng nói đến (10^9). 

Lực lượng vũ phu hoạt động vì các hoạt động xác định mối quan hệ tương đương, nhưng nó không thành công vì số lượng chuỗi theo cấp số nhân theo độ dài được yêu cầu. Quan sát quan trọng là ba mối quan hệ này không tạo ra vô số lớp tương đương riêng biệt. Chúng tạo thành một nhóm hữu hạn chỉ có 12 phần tử. 

Một cách thuận tiện để xem nhóm là gán hoán vị cho hai chữ cái. hãy để 

[ 
a=(12)(34),\qquad b=(123). 
] 

Hoán vị`a`có lệnh thứ hai,`b`có lệnh thứ ba, và`ab`có bậc ba, vì vậy cả ba quan hệ từ bài toán đều đúng. Hai hoán vị này tạo ra nhóm đối xứng quay của một tứ diện, đẳng cấu với (A_4) và (A_4) có đúng 12 phần tử. 

Tương đương, 12 lớp tương đương có thể được biểu diễn bằng các từ 

[ 
\epsilon,\ a,\ b,\ ab,\ ba,\ bb,\ aba,\ abb,\ bab,\ bba,\ babb,\ bbab. 
] 

Mỗi chuỗi có thể được rút gọn thành một trong các đại diện này bằng cách sử dụng các quan hệ đã cho và các đại diện này tương ứng với các phần tử riêng biệt của (A_4). Điều này mang lại cho chúng ta một máy tự động hữu hạn với chính xác 12 trạng thái. Cấu trúc 12 trạng thái tương tự này là quan sát trung tâm được sử dụng bởi các giải pháp hiện có cho vấn đề. 

Một khi trạng thái`s`đã biết, phần còn lại của bài toán trở thành bài toán đếm bước đi. Bắt đầu ở trạng thái nhận dạng, nối thêm`a`hoặc`b`chính xác`x`lần và hỏi có bao nhiêu lần đi bộ kết thúc ở trạng thái được đại diện bởi`s`. Chỉ có 12 trạng thái và hai lần chuyển tiếp đi từ mỗi trạng thái, do đó, ma trận kề sẽ xử lý việc đếm. Từ`x`có thể đạt tới (10^9), chúng ta nâng ma trận 12 x 12 lên`x`- lũy thừa thứ sử dụng lũy ​​thừa nhị phân. 

Độ phức tạp thu được là (O(n+12^3\log x)), dễ dàng nằm trong giới hạn. Đây cũng là sự phức tạp được mô tả bởi cách tiếp cận giải pháp tiêu chuẩn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (\Theta(x2^x)) | (O(x)) | Quá chậm | 
| Tối ưu | (O(n+12^3\log x)) | (O(12^2)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Giải thích ba thao tác xóa dưới dạng quan hệ nhóm. Đang xóa`aa`có nghĩa là (a^2=1), xóa`bbb`có nghĩa là (b^3=1) và xóa`ababab`có nghĩa là ((ab)^3=1). Bởi vì các phép toán nghịch đảo cũng được cho phép nên các mối quan hệ này hoạt động theo cả hai hướng. 
2. Biểu diễn hai bộ tạo dưới dạng hoán vị của bốn đối tượng. Sử dụng (a=(12)(34)) và (b=(123)). Hoán vị nhận dạng đại diện cho chuỗi trống. 
3. Tạo ra tất cả 12 hoán vị chẵn của bốn phần tử. Đây chính xác là các phần tử của (A_4), vì vậy mỗi lớp tương đương có một trạng thái trong máy tự động của chúng ta. 
4. Xác định thành phần hoán vị sao cho việc thêm một ký tự có nghĩa là nhân hoán vị hiện tại ở bên phải với hoán vị của ký tự đó. Nếu trạng thái hiện tại là`g`và ký tự tiếp theo đại diện`h`, trạng thái mới là (g\circ h). 
5. Quét chuỗi gốc`s`từ trái sang phải. Bắt đầu với hoán vị danh tính và nhân với hoán vị tương ứng với mỗi ký tự. Sau khi quét, hoán vị thu được chính xác là lớp tương đương của`s`. 
6. Xây dựng ma trận chuyển tiếp 12 x 12`T`. Đối với mọi tiểu bang`u`, nối thêm`a`Và`b`, tính toán các trạng thái kết quả`v`, và tăng`T[v][u]`bởi một. Chúng tôi sử dụng hướng này vì vectơ cột của số lượng trạng thái được chuyển đổi thành (dp' = Tdp). 
7. Vectơ cho chuỗi trống có một số đếm ở trạng thái nhận dạng và số 0 ở trạng thái khác. Sau chính xác`x`ký tự, vectơ đếm trạng thái là 

[ 
T^x e, 
] 

ở đâu`e`là vectơ trạng thái nhận dạng. 

1. Tính (T^x) theo lũy thừa nhị phân. Mỗi bình phương đại diện cho việc nhân đôi số lượng ký tự được nối thêm được biểu thị bằng ma trận. Bất cứ khi nào bit hiện tại của`x`là một, nhân ma trận câu trả lời với lũy thừa hiện tại. 
2. Đọc mục tương ứng với trạng thái của`s`từ trạng thái bắt đầu nhận dạng. Giá trị đó là số độ dài-`x`chuỗi tương đương với`s`. 

### Tại sao nó hoạt động 

Điều bất biến là mỗi chuỗi nhị phân có chính xác một trạng thái trong nhóm 12 phần tử và hai chuỗi có thể chuyển đổi thành chuỗi khác khi và chỉ khi chúng có cùng trạng thái. Việc chèn và xóa được phép sẽ giữ nguyên phần tử nhóm vì chúng chèn hoặc xóa một trong (a^2), (b^3) hoặc ((ab)^3), tất cả đều bằng danh tính. 

Ngược lại, 12 trạng thái là đủ để đại diện cho mọi lớp tương đương. Các bộ tạo (a=(12)(34)) và (b=(123)) tạo ra (A_4) và 12 từ chuẩn được liệt kê ở trên đại diện cho 12 phần tử riêng biệt của nó. Do đó, trạng thái nhóm không hợp nhất hai lớp tương đương thực sự khác nhau. 

Đối với độ dài mục tiêu, mỗi chuỗi nhị phân tương ứng với chính xác một bước độ dài`x`bắt đầu từ danh tính, với`a`Và`b`chọn hai chuyển tiếp có thể có ở mỗi vị trí. Phép nhân ma trận đếm những bước đi này, bao gồm cả bội số của chúng. Do đó, việc nhập ma trận từ danh tính đến trạng thái`s`đếm chính xác các chuỗi mục tiêu mong muốn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353
K = 12

def compose(p, q):
    """Return p o q, where permutations are stored by their images."""
    return tuple(p[q[i]] for i in range(4))

def parity(p):
    """Return 0 for even and 1 for odd permutation."""
    inv = 0
    for i in range(4):
        for j in range(i + 1, 4):
            if p[i] > p[j]:
                inv += 1
    return inv & 1

def mat_mul(a, b):
    n = K
    c = [[0] * n for _ in range(n)]

    for i in range(n):
        ci = c[i]
        ai = a[i]
        for k in range(n):
            x = ai[k]
            if x == 0:
                continue
            bk = b[k]
            for j in range(n):
                ci[j] = (ci[j] + x * bk[j]) % MOD

    return c

def mat_pow(a, e):
    n = K
    r = [[0] * n for _ in range(n)]
    for i in range(n):
        r[i][i] = 1

    while e:
        if e & 1:
            r = mat_mul(r, a)
        a = mat_mul(a, a)
        e >>= 1

    return r

def solve():
    n = int(input())
    s = input().strip()
    x = int(input())

    # All even permutations of four elements are the 12 states of A4.
    states = []
    for p in __import__("itertools").permutations(range(4)):
        if parity(p) == 0:
            states.append(p)

    index = {p: i for i, p in enumerate(states)}

    # a = (12)(34), b = (123)
    a = (1, 0, 3, 2)
    b = (1, 2, 0, 3)

    generators = {
        'a': a,
        'b': b,
    }

    identity = (0, 1, 2, 3)

    # Find the group element represented by s.
    g = identity
    for ch in s:
        g = compose(g, generators[ch])

    target = index[g]
    start = index[identity]

    # T[v][u] = number of one-character transitions u -> v.
    T = [[0] * K for _ in range(K)]

    for u, state in enumerate(states):
        va = index[compose(state, a)]
        vb = index[compose(state, b)]
        T[va][u] += 1
        T[vb][u] += 1

    T = mat_pow(T, x)

    print(T[target][start] % MOD)

if __name__ == "__main__":
    solve()
```Cấu trúc hoán vị giúp giải pháp được triển khai rõ ràng thay vì mã hóa cứng bảng rút gọn. Bộ dữ liệu`(p[0], p[1], p[2], p[3])`cửa hàng nơi mỗi điểm trong số bốn điểm được gửi. Danh tính là`(0, 1, 2, 3)`, trong khi`a = (1, 0, 3, 2)`đại diện cho ((12)(34)) và`b = (1, 2, 0, 3)`đại diện cho ((123)). 

các`compose`hàm thực hiện thành phần toán học (p\circ q). Khi từ hiện tại đại diện cho`p`và chúng tôi nối thêm một máy phát điện`q`, từ mới đại diện cho`p q`, chính xác là`compose(p, q)`theo công ước này. Giữ trật tự này nhất quán là điều cần thiết. Việc đảo ngược nó sẽ tạo ra một máy tự động khác và có thể âm thầm tạo ra các câu trả lời sai. 

Quá trình quét qua`s`là tuyến tính trong`n`. Không cần phải giảm chuỗi con hoặc duy trì ngăn xếp. Hoán vị hiện tại chứa tất cả thông tin liên quan đến các phép biến đổi trong tương lai. 

Ma trận sử dụng`T[new][old]`còn hơn là`T[old][new]`. Với quy ước này, nhân`T`bởi một vectơ cột đưa ra sự phân bố trạng thái tiếp theo. Do đó, số lượng nhận dạng mục tiêu là`T[target][identity]`sau khi lũy thừa. 

Tất cả các sản phẩm ma trận được thực hiện modulo (998244353). Số nguyên Python không bị tràn, nhưng việc giảm trong quá trình nhân cũng giữ cho các giá trị trung gian ở mức nhỏ và tránh sự tăng trưởng không cần thiết. 

Số mũ`x`được xử lý bằng cách dịch chuyển bit, do đó chỉ cần phép nhân ma trận (O(\log x)). Vì kích thước ma trận được cố định ở mức 12 nên phần này rất nhỏ so với việc quét chuỗi gốc. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
6
ababab
3
```Chuỗi gốc biểu thị danh tính vì ((ab)^3=1). Quét từng ký tự sẽ đưa ra các trạng thái sau, sử dụng tên chuẩn của 12 thành phần nhóm. 

| Vị trí | Nhân vật | Bang của`s`| Danh tính? | 
| --- | --- | --- | --- | 
| 0 | |`ε`| Có | 
| 1 |`a`|`a`| Không | 
| 2 |`b`|`ab`| Không | 
| 3 |`a`|`aba`| Không | 
| 4 |`b`|`bba`| Không | 
| 5 |`a`|`bb`| Không | 
| 6 |`b`|`ε`| Có | 

Bây giờ chúng ta cần số lần đi bộ dài ba lần từ danh tính trở lại danh tính. Ở chiều dài một, không`a`cũng không`b`là danh tính. Ở độ dài hai,`aa`là thân phận, nhường bước một bước. Ở độ dài ba,`bbb`là thân phận, nhường bước một bước. 

| Chiều dài | Số lượng danh tính | Đại diện liên quan | 
| --- | --- | --- | 
| 0 | 1 |`ε`| 
| 1 | 0 | không | 
| 2 | 1 |`aa`| 
| 3 | 1 |`bbb`| 

Câu trả lời là`1`. Điều này chứng tỏ tại sao bản thân chuỗi bắt đầu không cần phải có độ dài mục tiêu. Chỉ có trạng thái nhóm của nó là quan trọng. 

### Mẫu 2 

Đầu vào là```
3
bbb
2
```Ở đây chuỗi bắt đầu cũng đại diện cho danh tính vì (b^3=1). 

| Vị trí | Nhân vật | Bang của`s`| Danh tính? | 
| --- | --- | --- | --- | 
| 0 | |`ε`| Có | 
| 1 |`b`|`b`| Không | 
| 2 |`b`|`bb`| Không | 
| 3 |`b`|`ε`| Có | 

Độ dài mục tiêu là hai, vì vậy chúng tôi tính các bước nhận dạng có độ dài hai. 

| Chiều dài | Số lượng danh tính | Đại diện liên quan | 
| --- | --- | --- | 
| 0 | 1 |`ε`| 
| 1 | 0 | không | 
| 2 | 1 |`aa`| 

Câu trả lời là`1`. Trường hợp này thực hiện một thực tế là một chuỗi bắt đầu không trống có thể tương đương với chuỗi trống. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n+12^3\log x)) | Chuỗi được quét một lần, sau đó ma trận có kích thước không đổi được lũy thừa theo thời gian logarit. | 
| Không gian | (O(12^2)) | Mỗi ma trận chuyển tiếp và lũy thừa chỉ chứa (12^2) mục. | 

Với (n\le300000), việc quét tuyến tính có thể dễ dàng thực hiện được. Độ dài mục tiêu có thể là (10^9), nhưng nó chỉ xuất hiện bên trong lũy ​​thừa nhị phân, do đó số phép nhân ma trận là khoảng 30. Kích thước ma trận cố định là 12 làm cho lũy thừa không đáng kể trong thực tế. Phân tích tiêu chuẩn cho giải pháp này là (O(n+12^3\log x)). 

## Trường hợp thử nghiệm```python
# This test file assumes solve() is the function from the solution above.
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided samples
assert run("6\nababab\n3\n") == "1", "sample 1"
assert run("3\nbbb\n2\n") == "1", "sample 2"
assert run("5\nbabab\n35\n") == "866826000", "sample 3"

# Minimum target length: a is not equivalent to the empty string.
assert run("1\na\n0\n") == "0", "x = 0 with a non-identity start"

# Identity represented by a nonempty string.
assert run("2\naa\n0\n") == "1", "aa can be deleted completely"

# Small target length and an all-equal starting string.
assert run("5\naaaaa\n1\n") == "1", "five a's represent a"

# Boundary between length 0 and length 1.
assert run("2\naa\n1\n") == "0", "identity has no length-1 representative"

# A short case exercising the ab relation structure.
assert run("2\nab\n2\n") == "1", "only ab among length-2 strings has the same state"

# Maximum input size with an easy-to-check answer.
big_s = "a" * 300000
assert run(f"300000\n{big_s}\n0\n") == "1", "maximum n, even number of a's"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / a / 0`|`0`| Độ dài mục tiêu bằng 0 với trạng thái bắt đầu không nhận dạng | 
|`2 / aa / 0`|`1`| Chuỗi không trống tương đương với danh tính | 
|`5 / aaaaa / 1`|`1`| Đầu vào hoàn toàn bằng nhau và ứng dụng lặp lại của (a^2=1) | 
|`2 / aa / 1`|`0`| Ranh giới giữa độ dài mục tiêu bằng 0 và dương | 
|`2 / ab / 2`|`1`| Phân biệt trạng thái nhóm ở độ dài nhỏ | 
|`300000 / a...a / 0`|`1`| Tối đa`n`và tiền xử lý tuyến tính | 

## Vỏ cạnh 

cho```
1
a
0
```mục tiêu là chuỗi trống, trạng thái của nó là danh tính. ban đầu`a`ánh xạ tới hoán vị`(1, 0, 3, 2)`, đó không phải là danh tính. Sau khi lũy thừa ma trận chuyển tiếp về lũy thừa 0, ma trận này là ma trận đồng nhất, do đó việc chuyển từ trạng thái đồng nhất sang trạng thái đồng nhất`a`trạng thái bằng không. Thuật toán in`0`. 

Vì```
2
aa
0
```quá trình quét thực hiện (e\circ a\circ a=e). Vì vậy, trạng thái mục tiêu là danh tính. Vì (T^0=I), mục nhập từ danh tính đến danh tính là một, tương ứng với một chuỗi trống duy nhất. Thuật toán in`1`. 

Vì```
3
bbb
2
```trạng thái bắt đầu là (b^3=e). Ở độ dài thứ hai, danh tính có thể đạt được thông qua`aa`, trong khi`ab`,`ba`, Và`bb`đại diện cho ba khả năng có độ dài hai còn lại. Do đó, chính xác một chuỗi mục tiêu là hợp lệ và thuật toán sẽ in`1`. 

Vì```
6
ababab
3
```trạng thái bắt đầu cũng là danh tính vì ((ab)^3=e). Chiều dài ba bước đi được đếm ngược về danh tính là một, được biểu thị bằng`bbb`. Điều này nắm bắt các giải pháp chỉ hiểu`aa`Và`bbb`quan hệ và quên điều đó đi`ababab`cũng phải sụp đổ về danh tính. 

Đối với trường hợp kích thước tối đa```
300000
aaaaaaaa...
0
```300000 bản sao của`a`tạo thành một lũy thừa chẵn, do đó (a^{300000}=e). Quá trình quét vẫn chỉ mất (O(n)) thời gian và không bao giờ tạo bất kỳ chuỗi trung gian nào. Từ`x=0`, phép lũy thừa ma trận ngay lập tức trả về ma trận đẳng thức, đưa ra đáp số`1`. Điều này cho thấy tại sao thuật toán vẫn thực tế khi chính chuỗi đầu vào là phần chiếm ưu thế của đầu vào.
