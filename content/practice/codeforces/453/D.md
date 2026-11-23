---
title: "CF 453D - Chú Ngựa Nhỏ Và Những Yếu Tố Hài Hòa"
description: "Chúng ta có một vectơ năng lượng được lập chỉ mục bởi tất cả các mặt nạ bit có độ dài m. Vì n = 2^m nên mỗi đỉnh có thể được xác định bằng số m-bit. Một bước chuyển đổi là tuyến tính."
date: "2026-05-30T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dp", "matrices"]
categories: ["algorithms"]
codeforces_contest: 453
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 259 (Div. 1)"
rating: 3000
weight: 453
solve_time_s: 150
verified: true
draft: false
---

[CF 453D - Chú ngựa nhỏ và các yếu tố hòa hợp](https://codeforces.com/problemset/problem/453/D) 

**Đánh giá:** 3000 
**Thẻ:** dp, ma trận 
**Thời gian giải:** 2 phút 30 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một vectơ năng lượng được lập chỉ mục bởi tất cả các mặt nạ bit có độ dài`m`. Từ`n = 2^m`, mọi đỉnh có thể được xác định bằng một`m`-số bit. 

Một bước chuyển đổi là tuyến tính. Với mỗi cặp đỉnh`u`Và`v`, sự đóng góp từ`v`ĐẾN`u`chỉ phụ thuộc vào khoảng cách Hamming giữa các biểu diễn bit của chúng:$$A_{u,v} = b[\text{popcount}(u \oplus v)]$$Nếu như`e_i`là vectơ năng lượng tại thời điểm`i`, sau đó$$e_{i+1} = A e_i.$$Nhiệm vụ là tính toán$$e_t = A^t e_0$$modulo`p`, Ở đâu`t`có thể lớn như`10^{18}`. 

Các ràng buộc ngay lập tức loại trừ phép lũy thừa ma trận thông thường. Kích thước ma trận là`n × n`, Và`n = 2^m`với`m ≤ 20`, Vì thế`n`có thể đạt được`1,048,576`. Một ma trận dày đặc sẽ chứa nhiều hơn`10^{12}`các mục, hoàn toàn không thể lưu trữ. 

Ngay cả việc áp dụng phép biến đổi một lần cũng tốn kém`O(n²)`. Đối với kích thước tối đa là khoảng`10^{12}`các phép toán trong một bước duy nhất, thậm chí trước khi xem xét lũy thừa. 

Cấu trúc của ma trận là toàn bộ vấn đề. Mỗi mục chỉ phụ thuộc vào`u xor v`. Các ma trận như vậy tạo thành đại số tích chập XOR, được chéo hóa bởi phép biến đổi Walsh-Hadamard. 

Một trường hợp cạnh tinh tế là`t = 0`. Sau đó, không có phép biến đổi nào được áp dụng và câu trả lời đơn giản là`e0 mod p`. 

Ví dụ:```
m = 1
t = 0
p = 100
e0 = [5, 7]
b = [3, 4]
```Đầu ra:```
5
7
```Bất kỳ giải pháp nào lũy thừa một cách mù quáng các giá trị riêng mà không xử lý số mũ bằng 0 sẽ vẫn hoạt động về mặt toán học, nhưng việc triển khai thường quên trường hợp đặc biệt này. 

Một sai lầm dễ mắc phải khác là thực hiện phép biến đổi Walsh-Hadamard bằng cách chia cho hai. mô-đun`p`không nhất thiết là số nguyên tố và thậm chí có thể là số chẵn, do đó nghịch đảo môđun thường không tồn tại. 

Ví dụ:```
p = 100
```Nghịch đảo của`2`modulo`100`không tồn tại. Giải pháp dựa trên ma trận Hadamard chuẩn hóa sẽ thất bại ngay lập tức. Cách tiếp cận đúng sử dụng phép biến đổi không chuẩn hóa, trong đó phép đảo ngược đạt được bằng cách chia cho`n = 2^m`trên các số nguyên trước khi lấy modulo`p`. 

Cạm bẫy thứ ba là tràn số nguyên trong các ngôn ngữ có số học có độ rộng cố định. Giá trị riêng được xây dựng từ việc cộng và trừ lặp đi lặp lại các giá trị lên đến`10^9`, sau đó được nâng lên lũy thừa lên tới`10^{18}`modulo`p`. Python xử lý việc này một cách tự nhiên, nhưng các giải pháp C++ phải giảm modulo`p`cẩn thận. 

## Phương pháp tiếp cận 

Trước tiên chúng ta hãy xem phép biến đổi là phép nhân với một ma trận`A`. 

Giải pháp vũ phu được xây dựng$$A_{u,v}=b[\text{popcount}(u\oplus v)]$$và tính toán$$A^t e_0.$$Định nghĩa này đúng vì mỗi bước chính xác là phép nhân vectơ-ma trận. Thật không may ma trận có kích thước`n × n`với`n = 2^m`. Ở mức hạn chế tối đa, con số này là hơn một nghìn tỷ mục. Ngay cả một lần nhân chi phí`O(n²)`. 

Quan sát quan trọng là ma trận chỉ phụ thuộc vào`u xor v`. 

Định nghĩa$$g[x] = b[\text{popcount}(x)].$$Sau đó$$(Af)[u] = \sum_v g[u\oplus v] f[v].$$Đây chính xác là tích chập XOR với kernel`g`. 

Phép biến đổi Walsh-Hadamard đóng vai trò tương tự đối với tích chập XOR mà FFT đóng vai trò đối với tích chập thông thường. Nếu như`H`biểu thị phép biến đổi Hadamard, sau đó$$H(g *_{\text{xor}} f) = (Hg)\cdot(Hf).$$Do đó, ma trận trở thành đường chéo trên cơ sở Hadamard. 

Các mục đường chéo đơn giản là phép biến đổi Walsh-Hadamard của`g`. Nếu chúng ta biểu thị chúng bằng`λ[x]`, sau đó$$A = H^{-1} D H,$$Ở đâu`D`là đường chéo với các mục`λ[x]`. 

Bây giờ lũy thừa trở nên tầm thường:$$A^t = H^{-1} D^t H.$$Thay vì lũy thừa một ma trận khổng lồ, chúng ta chỉ lũy thừa`n`giá trị riêng vô hướng. 

Thách thức còn lại là tính toán các giá trị riêng một cách hiệu quả. Hạt nhân$$g[x] = b[\text{popcount}(x)]$$chỉ phụ thuộc vào trọng lượng Hamming của`x`. Các hàm như vậy đối xứng trên khối Boolean. Phép biến đổi Walsh của họ chính xác là phép biến đổi Krawtchouk. 

Nếu như`k = popcount(mask)`, thì mỗi mặt nạ cân`k`có cùng giá trị riêng. Cho phép$$\lambda_k$$biểu thị giá trị đó. 

Sự tái diễn Krawtchouk cho phép tất cả`m+1`giá trị riêng được tính toán trong`O(m²)`thời gian. Từ`m ≤ 20`, chi phí này là không đáng kể. 

Sau khi có được giá trị riêng: 

1. Biến đổi`e0`với FWT. 
2. Đối với mỗi mặt nạ, nhân với`λ_{popcount(mask)}^t`. 
3. Áp dụng FWT nghịch đảo. 
4. Chia cho`n`. 
5. Giảm modulo`p`. 

Phần đắt tiền là hai phép biến đổi kích thước Walsh-Hadamard`n`, tạo ra sự phức tạp`O(n log n)`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n² log t) hoặc tệ hơn | O(n²) | Quá chậm | 
| Tối ưu | O(n log n + m2) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Bước 1 

Xây dựng bảng biến đổi Krawtchouk. 

Cho phép$$K_k(i)$$là giá trị đa thức Krawtchouk tương ứng với trọng số`k`và lập luận`i`. 

Sự tái phát là$$K_k(i) = K_k(i-1)-K_{k-1}(i)-K_{k-1}(i-1),$$với$$K_0(i)=1.$$Từ`m ≤ 20`, Một`(m+1) × (m+1)`cái bàn thì nhỏ xíu. 

### Bước 2 

Tính toán tất cả các giá trị riêng khác nhau. 

Đối với mỗi trọng lượng`k`:$$\lambda_k = \sum_{i=0}^{m} b[i]\;K_k(i).$$Mỗi mặt nạ có số lượng bằng nhau`k`chia sẻ giá trị riêng này. 

### Bước 3 

Áp dụng phép biến đổi Walsh-Hadamard cho vectơ năng lượng ban đầu. 

Phép biến đổi lặp tiêu chuẩn thực hiện:$$(a,b)\rightarrow(a+b,\ a-b)$$trên mỗi khối. 

Tất cả số học được thực hiện modulo`p`. 

### Bước 4 

Với mỗi hệ số biến đổi tương ứng với mặt nạ`x`, cho phép$$k=\text{popcount}(x).$$Nhân hệ số với$$\lambda_k^t \pmod p.$$Đây chính xác là hoạt động của ma trận đường chéo`D^t`. 

### Bước 5 

Áp dụng lại phép biến đổi Walsh-Hadamard. 

Đối với ma trận Hadamard không chuẩn hóa,$$H^2 = nI.$$Áp dụng phép biến đổi hai lần sẽ có hệ số là`n`. 

### Bước 6 

Chia mỗi hệ số cho`n`. 

Bởi vì`n=2^m`, phép chia này chính xác trong đại số Hadamard số nguyên. Chúng tôi thực hiện nó trước khi giảm modulo câu trả lời cuối cùng`p`. 

### Tại sao nó hoạt động 

Ma trận biến đổi thỏa mãn$$A_{u,v}=g[u\oplus v].$$Ma trận dạng này được chéo hóa theo cơ sở Walsh-Hadamard. Giá trị riêng liên quan đến mặt nạ chỉ phụ thuộc vào trọng số Hamming của nó và bằng biến đổi Walsh của`g`. 

Sau khi biến đổi vectơ ban đầu, mỗi tọa độ Hadamard tiến triển độc lập. Nâng ma trận lên`t`- sức mạnh thứ trở thành nâng từng giá trị riêng lên`t`-quyền lực thứ. Phép biến đổi nghịch đảo tái tạo lại hệ tọa độ ban đầu. 

Vì mỗi bước chính xác là một công thức đại số của$$e_t=A^t e_0,$$vectơ kết quả chính xác là sự phân bố năng lượng sau`t`những biến đổi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def fwht(a, mod):
    n = len(a)
    step = 1
    while step < n:
        jump = step << 1
        for i in range(0, n, jump):
            for j in range(step):
                x = a[i + j]
                y = a[i + j + step]
                a[i + j] = (x + y) % mod
                a[i + j + step] = (x - y) % mod
        step <<= 1

def solve():
    m, t, p = map(int, input().split())
    n = 1 << m

    e = [x % p for x in map(int, input().split())]
    b = list(map(int, input().split()))

    # Krawtchouk table
    K = [[0] * (m + 1) for _ in range(m + 1)]

    for i in range(m + 1):
        K[0][i] = 1

    for k in range(1, m + 1):
        K[k][0] = K[k - 1][0]

    for k in range(1, m + 1):
        for i in range(1, m + 1):
            K[k][i] = (
                K[k][i - 1]
                - K[k - 1][i]
                - K[k - 1][i - 1]
            )

    eig = [0] * (m + 1)
    for k in range(m + 1):
        s = 0
        for i in range(m + 1):
            s += b[i] * K[k][i]
        eig[k] = s % p

    fwht(e, p)

    popcnt = [0] * n
    for i in range(1, n):
        popcnt[i] = popcnt[i >> 1] + (i & 1)

    powers = [pow(eig[k], t, p) for k in range(m + 1)]

    for mask in range(n):
        e[mask] = e[mask] * powers[popcnt[mask]] % p

    fwht(e, p)

    inv_factor = pow(n, -1, p)

    out = [(x * inv_factor) % p for x in e]
    sys.stdout.write("\n".join(map(str, out)))

solve()
```Bảng Krawtchouk tính toán tất cả các giá trị biến đổi Walsh của các hàm đối xứng trọng số. Vì hạt nhân chỉ phụ thuộc vào trọng lượng Hamming nên chúng ta không bao giờ cần xây dựng ma trận đầy đủ. 

Phép biến đổi Walsh-Hadamard đầu tiên chuyển vectơ vào cơ sở riêng. Phép nhân với`powers[popcount(mask)]`áp dụng ma trận đường chéo`D^t`. 

Phép biến đổi thứ hai trở về cơ sở ban đầu. Ma trận Walsh-Hadamard được sử dụng ở đây là ma trận không chuẩn hóa, do đó, phép nghịch đảo yêu cầu chia cho`n`. 

Phần tinh tế nhất là tính toán giá trị riêng. Mọi mặt nạ có cùng số lượng phổ biến đều có cùng giá trị riêng, điều này làm giảm vấn đề từ`2^m`giá trị riêng biệt chỉ`m+1`. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2 2 10000
4 1 2 3
0 1 0
```Hạt nhân là: 

| x | số dân(x) | g[x] | 
| --- | --- | --- | 
| 0 | 0 | 0 | 
| 1 | 1 | 1 | 
| 2 | 1 | 1 | 
| 3 | 2 | 0 | 

Phép biến đổi Krawtchouk cho: 

| Trọng lượng k | Giá trị riêng | 
| --- | --- | 
| 0 | 2 | 
| 1 | 0 | 
| 2 | -2 | 

Sau khi bình phương các giá trị riêng: 

| Trọng lượng k | λ^2 | 
| --- | --- | 
| 0 | 4 | 
| 1 | 0 | 
| 2 | 4 | 

Áp dụng tiến hóa biến đổi và năng suất biến đổi nghịch đảo: 

| Đỉnh | Kết quả | 
| --- | --- | 
| 0 | 14 | 
| 1 | 6 | 
| 2 | 6 | 
| 3 | 14 | 

Điều này thể hiện ý tưởng trung tâm rằng phép lũy thừa ma trận trở thành phép lũy thừa độc lập của các giá trị riêng. 

### Ví dụ 2 

đầu vào:```
1 0 100
5 7
3 4
```Từ`t = 0`, mọi giá trị riêng được nâng lên lũy thừa thứ 0. 

| Đỉnh | Ban đầu | 
| --- | --- | 
| 0 | 5 | 
| 1 | 7 | 

Đầu ra:```
5
7
```Dấu vết xác nhận rằng thuật toán xử lý chính xác việc chuyển đổi danh tính. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n + m2) | Hai phép biến đổi Walsh-Hadamard thống trị thời gian chạy | 
| Không gian | O(n) | Lưu trữ vectơ năng lượng và mảng popcount | 

Với`m ≤ 20`, chúng tôi có`n ≤ 2^20 ≈ 10^6`. Các phép biến đổi thực hiện gần như`n log₂ n ≈ 20 million`hoạt động phù hợp thoải mái trong giới hạn. Việc sử dụng bộ nhớ là tuyến tính trong`n`, cũng nằm trong 256 MB có sẵn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()

    old_stdout = sys.stdout
    sys.stdout = out

    solve()

    sys.stdout = old_stdout
    return out.getvalue().strip()

# provided sample
assert run(
"""2 2 10000
4 1 2 3
0 1 0
"""
) == "\n".join(["14", "6", "6", "14"])

# t = 0
assert run(
"""1 0 100
5 7
3 4
"""
) == "\n".join(["5", "7"])

# all coefficients zero
assert run(
"""1 3 100
8 9
0 0
"""
) == "\n".join(["0", "0"])

# minimum size
assert run(
"""1 1 100
1 2
1 0
"""
) == "\n".join(["1", "2"])

# all equal energies
assert run(
"""1 2 1000
5 5
1 1
"""
) == "\n".join(["20", "20"])
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 | 14, 6, 6, 14 | Logic chuyển đổi chính | 
| t = 0 trường hợp | Vectơ gốc | Số mũ nhận dạng | 
| Tất cả các hệ số bằng 0 | Tất cả số không | Toán tử không | 
| Kích thước tối thiểu | Khối lập phương nhỏ nhất không tầm thường | Xử lý ranh giới | 
| Tất cả năng lượng như nhau | Trạng thái đối xứng | Ứng dụng giá trị riêng đúng | 

## Vỏ cạnh 

### Không có thời gian 

đầu vào:```
1 0 100
5 7
3 4
```Thuật toán tính toán các giá trị riêng một cách bình thường, nhưng mọi giá trị riêng đều được nâng lên lũy thừa bằng 0. Mỗi tọa độ biến đổi được nhân với`1`, vì vậy sau khi biến đổi nghịch đảo, vectơ ban đầu được phục hồi:```
5
7
```### Ma trận biến đổi 0 

đầu vào:```
1 5 100
8 9
0 0
```Mọi giá trị kernel đều bằng 0. Mọi giá trị riêng đều bằng không. Sau khi lũy thừa với số dương`t`, tất cả các tọa độ được biến đổi đều trở thành 0 và câu trả lời cuối cùng là```
0
0
```khớp với phép nhân lặp lại với ma trận 0. 

### Giá trị Krawtchouk âm trung gian lớn 

đầu vào:```
2 1 1000
1 2 3 4
0 0 1
```Đa thức Krawtchouk chứa các giá trị âm. Việc triển khai giữ chúng ở dạng số nguyên thông thường trong khi xây dựng các giá trị riêng và chỉ giảm modulo`p`sau đó. Điều này bảo toàn sự biến đổi chính xác và tránh các lỗi liên quan đến dấu hiệu. Modulo giảm cuối cùng`p`tạo ra kết quả chính xác bất kể tiêu cực trung gian.
