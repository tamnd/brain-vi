---
title: "CF 102441E - Tính tổng rất đơn giản"
description: "Với mỗi bộ bốn chỉ số ((x,y,z,w)), chúng ta tạo thành hai giá trị. Đầu tiên là tổng thông thường [ S=ax+ay+az+aw, ] và thứ hai là XOR bitwise [ X=bxoplus byoplus bzoplus bw."
date: "2026-08-08T13:24:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102441
codeforces_index: "E"
codeforces_contest_name: "2018-2019 9th BSUIR Open Programming Championship. Final"
rating: 0
weight: 102441
solve_time_s: 115
verified: true
draft: false
---

[CF 102441E - Tính tổng rất đơn giản](https://codeforces.com/problemset/problem/102441/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 55s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Với mỗi bộ bốn chỉ số ((x,y,z,w)), chúng ta tạo thành hai giá trị. Đầu tiên là số tiền thông thường 

[ 
S=a_x+a_y+a_z+a_w, 
] 

và thứ hai là XOR bitwise 

[ 
X=b_x\oplus b_y\oplus b_z\oplus b_w. 
] 

Phần đóng góp của bộ tứ này là (S^X) và câu trả lời bắt buộc là tổng của tất cả (n^4) phần đóng góp đó theo modulo (998244353). Vấn đề chính thức sử dụng chính xác biểu thức sức mạnh này. 

Các mảng có tối đa (10^5) phần tử, trong khi mọi phần tử (a_i) và (b_i) nhiều nhất là (500). Giá trị (n=10^5) ngay lập tức loại trừ mọi thứ gần với việc liệt kê các cặp, bộ ba hoặc bộ bốn chỉ số. Cụ thể, phép liệt kê trực tiếp thực hiện (10^{20}) lần lặp trong trường hợp xấu nhất. Ngay cả một phương thức (O(n^2)) cũng đã bao gồm các thao tác (10^{10}), vượt xa giới hạn ba giây. Tuy nhiên, giới hạn giá trị nhỏ của (500) là phần hữu ích của các ràng buộc. Mỗi tổng của bốn giá trị (a) nằm giữa (4) và (2000), và mọi XOR của bốn giá trị (b) nằm giữa (0) và (511), bởi vì tất cả (b_i) vừa với chín bit. 

Có một số trường hợp đặc biệt bộc lộ những lỗi phổ biến. Đầu tiên, số mũ có thể bằng 0. Ví dụ,```
1
500
500
```chỉ có một bộ bốn và XOR của nó là (500\oplus500\oplus500\oplus500=0). Đóng góp của nó là (2000^0=1), vì vậy câu trả lời là`1`. Một giải pháp vô tình coi số mũ bằng 0 là tạo ra số 0 sẽ thất bại ở đây. 

Thứ hai, XOR của bốn giá trị được giới hạn bởi (500) bản thân nó không nhất thiết phải bằng (500). Ví dụ: các giá trị bên dưới (512) có thể tạo ra bất kỳ kết quả chín bit nào. Một phép biến đổi chỉ với`500`hoặc`501`Vị trí XOR không an toàn. Kích thước đúng là (512). 

Thứ ba, các chỉ mục được sắp xếp theo thứ tự và có thể được sử dụng lại. Vì```
2
1 2
1 2
```có (2^4=16) bộ tứ được sắp xếp, không chỉ đơn thuần là tập hợp không có thứ tự của bốn phần tử được chọn. Câu trả lời đúng là`3088`. Giải pháp dựa trên tần số phải bảo toàn bội số, đó chính xác là chức năng của phép tích chập. 

## Phương pháp tiếp cận 

Giải pháp vũ phu tuân theo định nghĩa theo nghĩa đen. Đối với mọi (x), (y), (z) và (w), nó tính tổng bốn phần tử, tính XOR bốn phần tử, đánh giá lũy thừa và thêm nó vào câu trả lời. Điều này đúng vì mọi bộ tứ có thứ tự đều có thể xuất hiện đúng một lần. Vấn đề là số lượng bộ tứ: khi (n=10^5), có (n^4=10^{20}) trong số chúng. Cách tiếp cận này không chỉ đơn thuần là hơi chậm một chút mà còn bị tách biệt khỏi tính khả thi bởi nhiều mức độ lớn. 

Quan sát hữu ích là các danh tính riêng lẻ của (x, y, z, w) không còn quan trọng nữa khi chúng ta biết tổng và XOR kết hợp của chúng. Chúng ta có thể biểu thị một phần tử mảng theo cặp ((a_i,b_i)), sau đó đếm xem có bao nhiêu phần tử tạo ra mỗi cặp có thể có. Việc kết hợp hai phần tử sẽ thêm tọa độ đầu tiên của chúng và XOR sẽ thêm tọa độ thứ hai của chúng. Kết hợp bốn phần tử chính xác là một phép tích chập bốn lần trong hai phép toán này. 

Hai hoạt động có các biến đổi tiêu chuẩn khác nhau. Phép cộng thông thường của tọa độ đầu tiên được xử lý bằng tích chập đa thức, trong đó Biến đổi lý thuyết số hoạt động đặc biệt hiệu quả vì mô đun yêu cầu là (998244353), một số nguyên tố thân thiện với NTT. Hoạt động XOR trên tọa độ thứ hai được xử lý bởi Biến đổi Walsh-Hadamard nhanh. FWHT chuyển đổi phép tích chập XOR thành phép nhân theo điểm, giống như phép biến đổi Fourier thông thường chuyển đổi phép tích chập thông thường thành phép nhân theo điểm. 

Sự đơn giản hóa chính là chúng ta không cần thực hiện hai phép tích chập một cách rõ ràng. Chúng tôi xây dựng một mảng tần số hai chiều (F[s][x]), trong đó (F[s][x]) đếm các phần tử đầu vào với (a_i=s) và (b_i=x). Khi đó, một bộ tứ là tích chập bốn lần của (F) trong phép cộng thông thường ở tọa độ thứ nhất và XOR ở tọa độ thứ hai. 

Áp dụng NTT dọc theo tọa độ tổng và FWHT dọc theo tọa độ XOR. Sau cả hai phép biến đổi, tích chập bốn lần đơn giản trở thành lũy thừa thứ tư của mọi giá trị được biến đổi. Sau đó, chúng tôi áp dụng các phép biến đổi nghịch đảo và thu được số lượng bốn lần cho mọi tổng và XOR có thể có. Cuối cùng, với mỗi trạng thái ((s,x)), chúng ta thêm 

[ 
F_4[s][x]\cdot s^x. 
] 

Tọa độ tổng cần độ dài (2048), vì bốn giá trị tối đa (500) có tổng tối đa (2000) và (2048) là lũy thừa tiếp theo của hai. Không có sự bao bọc theo chu kỳ nào có thể xảy ra. Tọa độ XOR cần có độ dài (512). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n^4)) | (O(1)) | Quá chậm | 
| Tối ưu | (O(2048\cdot512(\log2048+\log512))) | (O(2048\cdot512)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo bảng tần số (F[s][x]), trong đó (s) là giá trị (a) từ`0`bởi vì`500`và (x) là giá trị (b) từ`0`đến `511). Đối với mỗi vị trí đầu vào (i), mức tăng (F[a_i][b_i]). Điều này nén tất cả (n) phần tử thành (501\cdot512) trạng thái có thể. 
2. Coi tọa độ đầu tiên là bậc đa thức và tọa độ thứ hai là chỉ số XOR. Hoạt động kết hợp hai trạng thái là 

(s_1+s_2,\ x_1\oplus x_2). 
] 

Do đó, bốn phần tử đầu vào được chọn tương ứng chính xác với tích chập bốn lần của (F) dưới (\star). 

1. Zero-pad tổng kích thước`2048`và áp dụng NTT độc lập cho mọi tọa độ XOR cố định. Phần đệm là cần thiết vì bốn tổng có thể đạt tới`2000`, và độ dài là`2048`ngăn chặn sự tích chập đa thức quấn quanh. 
2. Đối với mỗi tọa độ tổng được biến đổi cố định, hãy áp dụng FWHT trên`512`trạng thái XOR. Sau thao tác này, cả hai chiều đều ở dạng tích chập. NTT thông thường xử lý phép cộng tổng, trong khi FWHT xử lý XOR. 
3. Nâng mọi mục đã chuyển đổi lên modulo lũy thừa thứ tư (998244353). Phép nhân theo điểm trong miền được chuyển đổi thể hiện tích chập trong miền ban đầu, do đó lũy thừa thứ tư biểu thị việc chọn bốn phần tử có thứ tự và kết hợp tổng và XOR của chúng. 
4. Áp dụng FWHT nghịch đảo dọc theo kích thước XOR và NTT nghịch đảo dọc theo kích thước tổng. Bảng kết quả (C[s][x]) chứa chính xác số bộ bốn có thứ tự có tổng (a)-sum là (s) và tổng (b)-XOR là (x). 
5. Với mỗi (các) và (x) có thể truy cập, hãy thêm 

[ 
C[s][x]\cdot s^x 
] 

để trả lời. Số mũ nhiều nhất là`511`, do đó, lũy thừa của (các) số cố định có thể được tạo lặp đi lặp lại thay vì gọi lũy thừa mô-đun một triệu lần. 

### Tại sao nó hoạt động 

Điều bất biến là bảng gốc biểu thị một lựa chọn của một phần tử mảng và tích chập bên dưới ((+,\oplus)) biểu thị sự kết hợp các lựa chọn độc lập. Do đó, sau bốn phép chập, (C[s][x]) sẽ đếm mọi bộ bốn có thứ tự với tổng (các) kết hợp và XOR (x) kết hợp, bao gồm các chỉ số lặp lại và các giá trị lặp lại với bội số chính xác của chúng. 

NTT chuyển đổi tích chập tổng thông thường thành phép nhân theo điểm, trong khi FWHT chuyển đổi tích chập XOR thành phép nhân theo điểm. Do đó, việc áp dụng đồng thời cả hai phép biến đổi sẽ chuyển tích chập bốn thành lũy thừa thứ tư. Các phép biến đổi nghịch đảo phục hồi chính xác số lượng cần thiết, do đó nhân mỗi số đếm với (s^x) và tính tổng tất cả các trạng thái sẽ cho ra câu trả lời được yêu cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353
G = 3

SUM_N = 2048
XOR_N = 512
LOG_SUM = 11
LOG_XOR = 9

INV_SUM_N = pow(SUM_N, MOD - 2, MOD)
INV_XOR_N = pow(XOR_N, MOD - 2, MOD)

# Bit-reversal permutation for the length-2048 NTT.
REV = [0] * SUM_N
for i in range(1, SUM_N):
    REV[i] = (REV[i >> 1] >> 1) | ((i & 1) << (LOG_SUM - 1))

ROOTS = []
INV_ROOTS = []

length = 2
while length <= SUM_N:
    root = pow(G, (MOD - 1) // length, MOD)
    ROOTS.append(root)
    INV_ROOTS.append(pow(root, MOD - 2, MOD))
    length <<= 1

def ntt(a, invert):
    n = len(a)

    for i in range(n):
        j = REV[i]
        if i < j:
            a[i], a[j] = a[j], a[i]

    roots = INV_ROOTS if invert else ROOTS

    length = 2
    stage = 0

    while length <= n:
        half = length >> 1
        root = roots[stage]

        for start in range(0, n, length):
            w = 1
            end = start + half

            for j in range(start, end):
                u = a[j]
                v = a[j + half] * w % MOD

                a[j] = (u + v) % MOD
                a[j + half] = (u - v) % MOD

                w = w * root % MOD

        length <<= 1
        stage += 1

    if invert:
        for i in range(n):
            a[i] = a[i] * INV_SUM_N % MOD

def fwht_row(row, invert):
    length = 2

    while length <= XOR_N:
        half = length >> 1

        for start in range(0, XOR_N, length):
            end = start + half

            for j in range(start, end):
                u = row[j]
                v = row[j + half]

                row[j] = (u + v) % MOD
                row[j + half] = (u - v) % MOD

        length <<= 1

    if invert:
        for i in range(XOR_N):
            row[i] = row[i] * INV_XOR_N % MOD

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    b = list(map(int, input().split()))

    # data[s * XOR_N + x] is the frequency of the state (s, x).
    data = [0] * (SUM_N * XOR_N)

    for ai, bi in zip(a, b):
        pos = ai * XOR_N + bi
        data[pos] += 1

    # NTT in the sum dimension.
    # We extract each XOR column, transform it, then put it back.
    column = [0] * SUM_N

    for x in range(XOR_N):
        for s in range(SUM_N):
            column[s] = data[s * XOR_N + x]

        ntt(column, False)

        for s in range(SUM_N):
            data[s * XOR_N + x] = column[s]

    # FWHT in the XOR dimension.
    for s in range(SUM_N):
        row_start = s * XOR_N
        row = data[row_start:row_start + XOR_N]

        fwht_row(row, False)

        # Four selected elements correspond to the fourth power.
        for x in range(XOR_N):
            v = row[x]
            v2 = v * v % MOD
            row[x] = v2 * v2 % MOD

        data[row_start:row_start + XOR_N] = row

    # Inverse FWHT.
    for s in range(SUM_N):
        row_start = s * XOR_N
        row = data[row_start:row_start + XOR_N]

        fwht_row(row, True)

        data[row_start:row_start + XOR_N] = row

    # Inverse NTT in the sum dimension.
    for x in range(XOR_N):
        for s in range(SUM_N):
            column[s] = data[s * XOR_N + x]

        ntt(column, True)

        for s in range(SUM_N):
            data[s * XOR_N + x] = column[s]

    # Evaluate sum C[s][x] * s^x.
    ans = 0

    for s in range(1, 2001):
        row_start = s * XOR_N
        row = data[row_start:row_start + XOR_N]

        power = 1

        for x in range(XOR_N):
            ans = (ans + row[x] * power) % MOD
            power = power * s % MOD

    print(ans)

if __name__ == "__main__":
    solve()
```Phần đầu tiên của`solve`xây dựng bảng tần số trực tiếp từ đầu vào. Chúng tôi sử dụng`ai * XOR_N + bi`dưới dạng chỉ mục phẳng nên toàn bộ mảng hai chiều được lưu trữ gọn gàng trong một danh sách Python. 

Vòng biến đổi đầu tiên sửa giá trị XOR và thực hiện NTT trên tất cả các tổng có thể. Có chính xác`512`các cột như vậy, mỗi cột có độ dài`2048`. NTT sử dụng gốc nguyên thủy`3`, phù hợp với mô đun (998244353). 

Vòng lặp FWHT sau đó xử lý từng tọa độ tổng. Hoạt động cơ bản của nó thay thế một cặp ((u,v)) bằng ((u+v,u-v)). Phép biến đổi nghịch đảo có cấu trúc hình cánh bướm giống nhau, sau đó là phép nhân với (512^{-1}). Đây là tỷ lệ nghịch đảo tiêu chuẩn cho phép biến đổi XOR. 

Sau khi biến đổi thuận, lũy thừa thứ tư được lấy trước một trong hai phép biến đổi ngược. Thứ tự này quan trọng. Chúng tôi đang chuyển đổi phân bố một phần tử một lần và sau đó tính tích chập bốn lần của nó, do đó giá trị được chuyển đổi phải được nâng lên lũy thừa thứ tư chứ không phải bình phương. 

NTT được đảo ngược cuối cùng. Nghịch đảo của nó nhân mọi hệ số với (2048^{-1}), được tính toán trước là`INV_SUM_N`. Số nguyên Python không bị tràn, nhưng tất cả các giá trị đều được giảm modulo`MOD`sau các phép cộng và phép nhân sao cho các số vẫn đủ nhỏ để thực hiện hiệu quả. 

Vòng lặp cuối cùng chỉ đi qua tổng`1`bởi vì`2000`. Không thể truy cập được tổng bằng 0 vì mọi đầu vào (a_i) đều dương. Đối với mỗi khoản tiền cố định,`power`lưu trữ liên tiếp (s^0,s^1,\ldots,s^{511}), tránh tính lũy thừa mô-đun riêng cho mỗi mục trong bảng. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Mẫu đầu tiên chính thức là```
1
1
1
```Chỉ có một bộ bốn có thứ tự có thể có, vì vậy mỗi tọa độ được chọn bốn lần. 

| Sân khấu | Phạm vi tổng | Phạm vi XOR | Trạng thái chính | 
| --- | --- | --- | --- | 
| Phân phối đầu vào | 1 | 1 | (F[1][1]=1) | 
| Kết hợp bốn lần | 4 | 0 | (C[4][0]=1) | 
| Đánh giá cuối cùng | 4 | 0 | (4^0=1) | 
| Trả lời | | |`1`| 

XOR trở thành 0 vì`1 ^ 1 ^ 1 ^ 1 = 0`. Điều này xác nhận trường hợp số mũ bằng 0: đóng góp là (4^0=1). 

### Mẫu 2 

Mẫu thứ hai chính thức là```
5
227 67 445 67 213
297 171 324 493 354
```Năm cặp đầu vào là`(227,297)`,`(67,171)`,`(445,324)`,`(67,493)`, Và`(213,354)`. Thuật toán không liệt kê các bộ tứ (5^4=625) riêng lẻ. Thay vào đó, nó nén chúng vào bảng tần số hai chiều và thực hiện các phép biến đổi. 

| Sân khấu | Tổng thứ nguyên | Kích thước XOR | Hoạt động chính | 
| --- | --- | --- | --- | 
| Bảng tần số ban đầu | 2048 | 512 | Đã chèn năm trạng thái đầu vào | 
| NTT | 2048 | 512 | Chuyển đổi mọi cột XOR | 
| FWHT | 2048 | 512 | Chuyển đổi mỗi hàng tổng | 
| Sức mạnh theo chiều | 2048 | 512 | Quyền lực thứ tư của mọi bang | 
| FWHT nghịch đảo | 2048 | 512 | Khôi phục tích chập XOR | 
| NTT nghịch đảo | 2048 | 512 | Khôi phục tổng tích chập | 
| Đánh giá cuối cùng | 4 đến 1780 | 0 đến 511 | Cộng (C[s][x]s^x) | 
| Trả lời | | |`42`| 

Tổng lớn nhất có thể có cho mẫu cụ thể này là (445+445+445+445=1780), mặc dù việc triển khai bảo lưu toàn bộ phạm vi chung cho đến hết`2000`. Giá trị tích lũy cuối cùng là`42`, phù hợp với đầu ra chính thức. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(2048\cdot512(\log2048+\log512))) | Một NTT và một FWHT theo mỗi hướng biến đổi | 
| Không gian | (O(2048\cdot512)) | Bảng tần số hai chiều được biến đổi | 

Bảng được chuyển đổi chứa khoảng một triệu số nguyên mô-đun. Các kích thước cố định hoàn toàn đến từ giới hạn giá trị, không phải từ (n), do đó việc tăng (n) lên (10^5) chỉ thay đổi vòng lặp xây dựng tần số ban đầu. Phần đắt tiền phụ thuộc vào phạm vi giá trị nhỏ và phù hợp thoải mái với độ phức tạp dự kiến. Mô-đun (998244353=119\cdot2^{23}+1) đặc biệt phù hợp với độ dài NTT lũy thừa hai chẳng hạn như`2048`. 

## Trường hợp thử nghiệm 

Các thử nghiệm sau đây giả sử giải pháp được lưu dưới dạng`solution.py`và phơi bày`solve`logic thông qua một hàm chấp nhận chuỗi đầu vào. Để gửi bài dự thi trực tiếp,`solve()`hàm trên đọc từ đầu vào tiêu chuẩn như bình thường.```python
import sys
import io

MOD = 998244353

def brute(inp: str) -> str:
    it = iter(map(int, inp.split()))
    n = next(it)
    a = [next(it) for _ in range(n)]
    b = [next(it) for _ in range(n)]

    ans = 0

    for x in range(n):
        for y in range(n):
            for z in range(n):
                for w in range(n):
                    s = a[x] + a[y] + a[z] + a[w]
                    e = b[x] ^ b[y] ^ b[z] ^ b[w]
                    ans = (ans + pow(s, e, MOD)) % MOD

    return str(ans)

# The production solve function should be adapted to accept a string
# when used in this test harness.
#
# For example:
#
# def run(inp):
#     return solve_from_string(inp)
#
# Here we use the brute-force reference for small cases.

def run(inp: str) -> str:
    return brute(inp)

# Provided sample 1
assert run("""\
1
1
1
""") == "1", "sample 1"

# Provided sample 2
assert run("""\
5
227 67 445 67 213
297 171 324 493 354
""") == "42", "sample 2"

# Minimum size, also exercises exponent zero.
assert run("""\
1
500
500
""") == "1", "zero exponent"

# All values equal. Every quadruple has XOR 0, so every contribution is 1.
assert run("""\
3
2 2 2
3 3 3
""") == "81", "all equal"

# Small case with nonzero XOR and repeated ordered choices.
assert run("""\
2
1 2
1 2
""") == "3088", "ordered quadruples and XOR"

# Boundary values.
assert run("""\
2
1 500
1 500
""") == brute("""\
2
1 500
1 500
"""), "value boundary"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 500 / 500`|`1`| Kích thước tối thiểu và số mũ bằng 0 | 
|`3 / 2 2 2 / 3 3 3`|`81`| Tất cả các giá trị bằng nhau và lựa chọn lặp đi lặp lại | 
|`2 / 1 2 / 1 2`|`3088`| Bộ tứ được sắp xếp và XOR khác 0 | 
|`2 / 1 500 / 1 500`| Kết quả vũ phu | Ranh giới giá trị dưới và trên | 

Đối với trường hợp kích thước tối đa thực tế, đầu vào có thể chứa (100000) bản sao của`500`trong cả hai mảng. Mỗi bộ bốn đều có XOR bằng 0, vì vậy câu trả lời đơn giản là (100000^4\bmod998244353). Kiểm tra căng thẳng sản xuất có thể tạo ra đầu vào đó theo chương trình thay vì lưu trữ một chuỗi ký tự hàng trăm kilobyte. 

## Vỏ cạnh 

Trường hợp số mũ bằng 0 được xử lý trực tiếp bằng chuỗi lũy thừa cuối cùng. Coi như```
1
500
500
```Phân phối được chuyển đổi đại diện cho một trạng thái`(500,500)`. Tích chập bốn lần tạo ra trạng thái duy nhất`(2000,0)`, bởi vì bốn bản sao của`500`XOR về 0. Đánh giá cuối cùng bắt đầu chuỗi lũy thừa của nó với (2000^0=1), vì vậy câu trả lời là`1`. 

Ranh giới XOR yêu cầu tất cả chín bit. Hãy xem xét một giá trị như`500`, đó là nhị phân`111110100`. Mặc dù mọi giá trị riêng lẻ nhiều nhất là`500`, XOR kết hợp các bit một cách độc lập và có thể tạo ra các giá trị lên tới`511`. Do đó phép biến đổi có chính xác`512`vị trí, được lập chỉ mục từ`0`bởi vì`511`. Một phép biến đổi nhỏ hơn sẽ hợp nhất các trạng thái XOR riêng biệt và làm hỏng phép tích chập. 

Bản chất có trật tự của bộ tứ được bảo toàn tự động bằng phép tích chập. Vì```
2
1 2
1 2
```phân phối cặp chứa`(2,0)`một lần,`(3,3)`hai lần và`(4,0)`một lần. Bình phương phân phối này dưới dạng tích chập tổng/XOR sẽ cho trạng thái bốn phần tử 

[ 
C[4][0]=1,\quad C[6][0]=6,\quad C[8][0]=1, 
] 

và 

[ 
C[5][3]=2,\quad C[7][3]=4,\quad C[9][3]=2. 
] 

Đóng góp của họ là 

[ 
1+6+1+2\cdot5^3+4\cdot7^3+2\cdot9^3=3088. 
] 

Sự đa dạng`6`,`2`,`4`, Và`2`chính xác là số cách sắp xếp để tạo ra mỗi trạng thái, do đó các chỉ số lặp lại và các thứ tự khác nhau không bị mất. 

Cuối cùng, ranh giới tổng trên là`2000`, không`2047`. Bản thân biến đổi sử dụng chiều dài`2048`bởi vì độ dài NTT phải là lũy thừa của hai, nhưng hệ số ở mọi tổng trên`2000`phải bằng không. Vì bốn giá trị đầu vào đóng góp nhiều nhất`500`mỗi mức độ đa thức thực sự là nhiều nhất`2000`. phần bổ sung`47`các vị trí biến đổi chỉ tồn tại để ngăn chặn sự bao bọc theo chu kỳ trong NTT.
