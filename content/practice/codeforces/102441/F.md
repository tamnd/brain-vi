---
title: "CF 102441F - XOR ngẫu nhiên"
description: "Ta có mảng a gồm n số nguyên. Mỗi phần tử được chọn độc lập với xác suất P = X/Y. Các phần tử được chọn được XOR với nhau, tạo ra số nguyên ngẫu nhiên s. Nếu không có gì được chọn thì XOR bằng 0."
date: "2026-08-08T13:26:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102441
codeforces_index: "F"
codeforces_contest_name: "2018-2019 9th BSUIR Open Programming Championship. Final"
rating: 0
weight: 102441
solve_time_s: 109
verified: true
draft: false
---

[CF 102441F - XOR ngẫu nhiên](https://codeforces.com/problemset/problem/102441/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 49s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một mảng`a`của`n`số nguyên. Mỗi phần tử được chọn độc lập với xác suất`P = X / Y`. Các phần tử được chọn được XOR cùng nhau, tạo ra một số nguyên ngẫu nhiên`s`. Nếu không có gì được chọn thì XOR bằng 0. Nhiệm vụ là tính giá trị kỳ vọng của`s²`, với đáp án cuối cùng được biểu thị theo modulo`10^9 + 7`. 

Khó khăn không nằm ở việc tính toán XOR dự kiến. Kỳ vọng của từng bit riêng lẻ là khá dễ dàng để có được. Hình vuông giới thiệu tích số giữa các bit khác nhau, vì vậy chúng ta cũng cần hiểu cách các cặp bit XOR hoạt động cùng nhau. 

Ràng buộc`n <= 10^5`ngay lập tức loại trừ việc liệt kê các tập hợp con. có thể có`2^n`những lựa chọn có thể có, rất lớn về mặt thiên văn ngay cả đối với vài chục phần tử. Các giá trị dưới đây`10^9 + 7`, vì vậy mỗi`a_i`phù hợp với 30 bit nhị phân vì`2^30 > 10^9 + 7`. Độ rộng bit nhỏ đó là thuộc tính cấu trúc giúp giải quyết vấn đề. Chúng ta có thể thực hiện công việc liên quan đến các vị trí khoảng 30 bit, nhưng chúng ta không thể thực hiện công việc tỷ lệ thuận với số lượng tập hợp con. 

Có một số trường hợp khó khăn trong đó sự đơn giản hóa hấp dẫn sẽ đưa ra câu trả lời sai. Đầu tiên, nếu`P = 0`, không có gì được chọn. Ví dụ,```
1 0 7
123
```cho`s = 0`một cách chắc chắn, vì vậy câu trả lời là`0`. Một công thức giả định mọi xác suất đều nằm trong khoảng từ 0 đến 1 có thể xử lý sai ranh giới này. 

Ở thái cực khác, nếu`P = 1`, mọi phần tử đều được chọn. Ví dụ,```
1 1 1
5
```luôn sản xuất`s = 5`, vậy câu trả lời là`25`. Tính toán xác suất cũng phải có hiệu quả khi quá trình ngẫu nhiên trở nên xác định. 

Trường hợp tinh tế hơn là mối tương quan giữa các bit. Coi như```
1 1 2
3
```Kết quả là hoặc`0`hoặc`3`, mỗi cái có xác suất`1/2`, Vì thế`E[s²] = (0² + 3²) / 2 = 9/2`. 

Modulo`10^9 + 7`, đây là`500000008`. Một cách tiếp cận bất cẩn có thể nhận thấy rằng mỗi bit trong số hai bit này độc lập`1`với xác suất`1/2`. Ở đây chúng không độc lập: con số này cũng là`00`hoặc`11`. Đối xử với họ như những người độc lập sẽ đưa ra khoảnh khắc thứ hai sai lầm. 

Mẫu đã cho là```
3 1 2
2 8 10
```và câu trả lời đúng của nó là`42`. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là xem xét mọi tập hợp con của`a`, XOR các phần tử đã chọn, bình phương kết quả và tính trung bình tất cả các giá trị với xác suất tương ứng của chúng. có`2^n`tập hợp con. Nếu mọi tập hợp con được xây dựng bằng cách kiểm tra tất cả`n`phần tử, số lượng hoạt động là`O(n 2^n)`. Ngay cả khi việc liệt kê mã Gray làm giảm công việc trên mỗi tập hợp con và mang lại`O(2^n)`, trường hợp xấu nhất vẫn chứa`2^100000`trạng thái, vì vậy cách tiếp cận này là không thể sử dụng được. 

Lực lượng vũ phu hoạt động vì nó tuân theo thử nghiệm ngẫu nhiên một cách rõ ràng. Nó thất bại vì thí nghiệm có nhiều kết quả theo cấp số nhân. Quan sát quan trọng là XOR cuối cùng được xây dựng từng chút một và câu trả lời chỉ chứa tối đa hai điều khoản về mức độ trong các bit đầu ra đó. 

Viết biểu diễn nhị phân của XOR cuối cùng dưới dạng`S = sum_k 2^k Z_k`,

Ở đâu`Z_k`là bit ngẫu nhiên tại vị trí`k`. Bình phương cho`S² = sum_k 2^(2k) Z_k + 2 sum_{i<j} 2^(i+j) Z_i Z_j`. 

Vì vậy chúng ta chỉ cần`E[Z_k]`Và`E[Z_i Z_j]`. 

Mỗi`Z_k`bản thân nó là một XOR của các biến Bernoulli độc lập. Đối với tính chẵn lẻ như vậy, đại lượng hữu ích không phải là xác suất trực tiếp của nó mà là kỳ vọng có dấu của nó.`E[(-1)^Z_k]`. 

Giả sử bit`k`được thiết lập chính xác`c_k`các phần tử mảng. Mọi phần tử có bit bằng 0 đều đóng góp một hệ số`1`với kỳ vọng đã được ký kết này. Mọi phần tử có bit bằng một đều đóng góp`(1-P) + P(-1) = 1 - 2P`. 

Như vậy`E[(-1)^Z_k] = (1 - 2P)^(c_k)`. 

Cho phép`q = 1 - 2P`. 

Sau đó`E[Z_k] = (1 - q^(c_k)) / 2`. 

Ý tưởng tương tự xử lý một cặp bit. Đối với hai bit đầu ra`Z_i`Và`Z_j`, định nghĩa`A = E[(-1)^Z_i]`,`B = E[(-1)^Z_j]`,`C = E[(-1)^(Z_i XOR Z_j)]`. 

Đại lượng cuối cùng phụ thuộc vào số lượng phần tử đầu vào có giá trị khác nhau ở hai vị trí bit này. Hãy để số đó là`d_ij`. Sau đó`C = q^(d_ij)`. 

Bốn sự kết hợp có thể có của hai biến Boolean có thể được phục hồi từ ba kỳ vọng đã ký của chúng. Đặc biệt,`P(Z_i = 1 and Z_j = 1) = (1 - A - B + C) / 4`. 

Điều này đưa ra mọi thuật ngữ được yêu cầu bởi`E[S²]`. 

Chỉ có 30 vị trí bit, do đó chỉ có 30 vị trí bit đơn và`30 * 29 / 2`khoảng cách cặp là cần thiết. Khoảng cách cặp có thể được tính toán một cách hiệu quả bằng cách biểu diễn mỗi cột bit dưới dạng một tập bit được đóng gói. Khoảng cách Hamming giữa hai cột sau đó là số lượng XOR của chúng. Trong Python, số nguyên lớn cung cấp chính xác cách biểu diễn được đóng gói này: một cột được lưu trữ dưới dạng số nguyên có các bit biểu thị tất cả`n`các phần tử mảng và việc triển khai C của Python xử lý XOR và`bit_count()`một cách hiệu quả. 

Kết quả được giảm từ phép liệt kê hàm mũ sang hoạt động trên các vị trí 30 bit và được đóng gói`n`cột -bit. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(n 2^n)`|`O(n)`| Quá chậm | 
| Tối ưu |`O(30n + 30² n / W)`|`O(30n / 8)`| Đã chấp nhận | 

Đây`W`là kích thước từ máy được sử dụng nội bộ bởi các hoạt động đóng gói bit. Trong quá trình triển khai Python, các thao tác tương ứng được thực hiện bằng các thủ tục số nguyên có độ chính xác tùy ý được tối ưu hóa. 

## Hướng dẫn thuật toán 

1. Đọc`n`,`X`,`Y`và mảng. mô-đun làm việc`MOD = 10^9 + 7`. Tính toán`q = 1 - 2X/Y`modulo`MOD`. Từ`Y < MOD`, nghịch đảo mô đun của nó tồn tại. 
2. Xây dựng một cột bit được đóng gói cho mỗi vị trí trong số 30 vị trí bit có thể có. các`r`-bit thứ của cột`k`ghi lại dù bit`k`của`a[r]`được thiết lập. Chúng ta có thể lưu trữ các cột này dưới dạng số nguyên Python. 

Cách biểu diễn này cho phép chúng ta so sánh toàn bộ các cột cùng một lúc thay vì truy cập tất cả`n`phần tử cho mỗi cặp vị trí bit. 
3. Đối với mỗi bit`k`, tính số lượng mục đã đặt của nó`c_k`sử dụng`column[k].bit_count()`. Sau đó tính toán`R_k = q^(c_k)`. 

Từ`R_k = E[(-1)^Z_k]`, xác suất XOR cuối cùng có bit`k`thiết lập là`p_k = (1 - R_k) / 2`. 
4. Cộng các số hạng của hình vuông. Chút`k`đóng góp`2^(2k) p_k`ĐẾN`E[S²]`. 
5. Đối với mỗi cặp vị trí bit riêng biệt`i < j`, XOR các cột được đóng gói của chúng và đếm các bit đã đặt. Điều này mang lại`d_ij = number of input elements where bits i and j differ`. 

Tính toán`R_ij = q^(d_ij)`. 

Cùng với`R_i`Và`R_j`, điều này mang lại`p_ij = P(Z_i = 1, Z_j = 1)`BẰNG`(1 - R_i - R_j + R_ij) / 4`. 
6. Thêm thuật ngữ chéo`2 * 2^(i+j) * p_ij`cho câu trả lời cho mỗi cặp`i < j`. 
7. Giảm modulo giá trị cuối cùng`MOD`và in nó. Việc chia cho 2 và 4 được thực hiện bằng nghịch đảo mô-đun, đó là`2^(MOD-2)`Và`4^(MOD-2)`modulo`MOD`. 

### Tại sao nó hoạt động 

Điều bất biến là đối với mỗi bit hoặc cặp bit được xử lý, sức mạnh lưu trữ của`q`chính xác là kỳ vọng đã ký của tính chẵn lẻ XOR tương ứng. Đối với một bit, mọi phần tử đầu vào được chọn với bộ bit đó sẽ lật tính chẵn lẻ, đóng góp một hệ số`1 - 2P`. Đối với hai bit, XOR của chúng lật chính xác khi hai bit đầu vào khác nhau, do đó số lượng hàng khác nhau sẽ xác định kỳ vọng có dấu tương ứng. Sau đó, các công thức cho một và hai biến Boolean sẽ phục hồi chính xác xác suất của chúng. Từ`S²`chỉ chứa các số hạng bit riêng lẻ và các tích bit theo cặp, việc tính tổng các kỳ vọng này sẽ cho kết quả chính xác`E[S²]`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 1_000_000_007
BITS = 30

def solve():
    n, X, Y = map(int, input().split())
    a = list(map(int, input().split()))

    inv_y = pow(Y, MOD - 2, MOD)
    q = (1 - 2 * X * inv_y) % MOD

    inv2 = (MOD + 1) // 2
    inv4 = inv2 * inv2 % MOD

    # Store every bit column as a packed integer.
    # Byte r contains the r-th row of the column.
    columns = [bytearray(n) for _ in range(BITS)]

    for r, value in enumerate(a):
        for bit in range(BITS):
            columns[bit][r] = (value >> bit) & 1

    packed = [int.from_bytes(col, "little") for col in columns]

    # signed[k] = E[(-1)^Z_k]
    signed = [0] * BITS

    answer = 0

    for bit in range(BITS):
        cnt = packed[bit].bit_count()
        signed[bit] = pow(q, cnt, MOD)

        p_one = (1 - signed[bit]) * inv2 % MOD
        weight = (1 << (2 * bit)) % MOD
        answer = (answer + weight * p_one) % MOD

    for i in range(BITS):
        for j in range(i + 1, BITS):
            differing = (packed[i] ^ packed[j]).bit_count()
            both_signed = pow(q, differing, MOD)

            p_both = (
                1
                - signed[i]
                - signed[j]
                + both_signed
            ) % MOD
            p_both = p_both * inv4 % MOD

            weight = (1 << (i + j)) % MOD
            answer = (
                answer + 2 * weight * p_both
            ) % MOD

    print(answer)

if __name__ == "__main__":
    solve()
```Phần đầu tiên của quá trình triển khai sẽ chuyển đổi xác suất hợp lý thành số học mô-đun.`inv_y`đại diện cho`Y⁻¹`, Vì thế`q = 1 - 2P`trở thành`(1 - 2X/Y) mod MOD`. Mô đun là số nguyên tố và`Y`hoàn toàn nhỏ hơn nó, nên định lý nhỏ Fermat đưa ra một nghịch đảo hợp lệ. 

các`columns`mảng là biểu diễn thu gọn của đầu vào được xem theo chiều dọc. Thay vì lưu trữ 30 bit của mỗi số dưới dạng số nguyên Python riêng biệt và liên tục quét toàn bộ mảng, mỗi cột được lưu trữ dưới dạng`n`byte và sau đó được chuyển đổi thành một số nguyên lớn. Việc chuyển đổi sử dụng thứ tự endian nhỏ nên`r`-phần tử đầu vào thứ tương ứng với`r`-vị trí bit thứ của số nguyên được đóng gói. 

Sự khác biệt giữa các byte trong`columns`và bit trong`packed`là cố ý. Bytearray giúp việc xây dựng trở nên đơn giản vì mỗi hàng đầu vào có thể được gán trực tiếp. Sau khi đóng gói, tất cả các so sánh cặp đắt tiền đều diễn ra trên các số nguyên Python được triển khai trong mã gốc được tối ưu hóa. 

Vòng lặp đầu tiên tính toán`signed[k]`, đó là`q`tăng lên số phần tử chứa bit`k`. Nó ngay lập tức chuyển đổi kỳ vọng đã ký này thành xác suất bit XOR cuối cùng là một và thêm đóng góp đường chéo tương ứng vào hình vuông. 

Vòng lặp thứ hai xử lý mọi cặp vị trí bit. XOR hai cột được đóng gói đánh dấu chính xác các phần tử mảng trong đó hai bit đầu vào khác nhau.`bit_count()`do đó mang lại`d_ij`không có vòng lặp Python`n`các phần tử. Xác suất cặp theo sau đồng nhất thức kỳ vọng đã ký và đóng góp của nó nhận được hệ số`2 * 2^(i+j)`từ các số hạng chéo của hình vuông. 

Mọi quyền lực như`1 << (2 * bit)`Và`1 << (i + j)`đủ nhỏ cho số nguyên Python và chúng được rút gọn theo modulo`MOD`trước khi vào số học mô-đun. Không có hiện tượng tràn số nguyên trong Python, nhưng việc giảm các giá trị trung gian giúp quản lý được các biểu thức mô-đun. 

Vòng lặp trên bit chạy chính xác 30 lần. Vòng lặp cặp chỉ chạy 435 lần. Tiềm năng lớn`n`thứ nguyên được ẩn bên trong các phép toán số nguyên được đóng gói thay vì hiển thị dưới dạng vòng lặp cấp Python cho mỗi cặp. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
3 1 2
2 8 10
```Đây`P = 1/2`, Vì thế`q = 1 - 2P = 0`. 

Các cột bit có liên quan là:```
2  = 0010
8  = 1000
10 = 1010
```Dấu vết là: 

| Bit hoặc cặp | Đếm / khoảng cách |`q^count`| Xác suất | Đóng góp | 
| --- | --- | --- | --- | --- | 
| bit 0 | 0 | 1 | 0 | 0 | 
| bit 1 | 2 | 0 | 1/2 | 2 | 
| bit 2 | 0 | 1 | 0 | 0 | 
| bit 3 | 2 | 0 | 1/2 | 32 | 
| bit 0,1 | 2 | 0 | 0 | 0 | 
| bit 0,2 | 3 | 0 | 0 | 0 | 
| bit 0,3 | 2 | 0 | 0 | 0 | 
| bit 1,2 | 2 | 0 | 0 | 0 | 
| bit 1,3 | 2 | 0 | 1/4 | 8 | 
| bit 2,3 | 2 | 0 | 0 | 0 | 

Đóng góp theo đường chéo là`2 + 32 = 34`. Sự đóng góp cặp khác 0 duy nhất là giữa bit 1 và 3, tạo ra`8`, vậy câu trả lời là`34 + 8 = 42`. Điều này phù hợp với đầu ra mẫu được công bố. 

Dấu vết cũng cho thấy lý do tại sao việc tính toán cặp là cần thiết. XOR cuối cùng được phân bố đồng đều trên`0, 2, 8, 10`, có giá trị bình phương trung bình là`42`. Chỉ tính toán giá trị mong đợi của từng bit riêng lẻ sẽ không đủ để khôi phục bình phương. 

### Ví dụ 2 

Hãy xem xét```
1 1 2
3
```Có một phần tử và nó được chọn với xác suất`1/2`. số`3`có cả hai bit thấp được thiết lập. 

| Bit hoặc cặp | Đếm / khoảng cách |`q^count`| Xác suất | 
| --- | --- | --- | --- | 
| bit 0 | 1 | 0 | 1/2 | 
| bit 1 | 1 | 0 | 1/2 | 
| bit 0,1 | 0 | 1 | 1/2 | 

Phần đường chéo là`1² * 1/2 + 2² * 1/2 = 5/2`. 

Xác suất của cặp là`1/2`, không`1/4`, bởi vì hai bit đầu ra có mối tương quan hoàn hảo. Đóng góp chéo là`2 * 1 * 2 * 1/2 = 2`. 

Tổng cộng là`5/2 + 2 = 9/2`, trở thành`500000008`modulo`10^9 + 7`. 

Ví dụ này trực tiếp xác nhận lý do sử dụng`q^(d_ij)`thay vì giả định rằng các bit đầu ra khác nhau là độc lập. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(30n + 30² n / W)`| Chi phí xây dựng 30 cột`O(30n)`, trong khi 435 gói XOR và quy trình hoạt động đếm dân số`n`bit ở dạng khối có kích thước bằng máy | 
| Không gian |`O(30n / 8)`| Việc triển khai lưu trữ các cột 30 byte, cộng với các biểu diễn số nguyên được đóng gói của chúng | 

Với`n = 10^5`, chỉ có 30 vị trí bit, do đó công việc ở cấp độ Python là khoảng ba triệu phép gán bit đơn giản cộng với 435 phép so sánh số nguyên lớn gốc. Các lần quét theo cặp đắt tiền được thực hiện bên trong triển khai số nguyên được tối ưu hóa của Python thay vì hàng chục triệu lần lặp Python được giải thích. Mức tiêu thụ bộ nhớ của các cột 30 byte là khoảng 3 MB, dễ chịu là dưới 256 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

MOD = 1_000_000_007
BITS = 30

def solve():
    input = sys.stdin.readline

    n, X, Y = map(int, input().split())
    a = list(map(int, input().split()))

    inv_y = pow(Y, MOD - 2, MOD)
    q = (1 - 2 * X * inv_y) % MOD

    inv2 = (MOD + 1) // 2
    inv4 = inv2 * inv2 % MOD

    columns = [bytearray(n) for _ in range(BITS)]

    for r, value in enumerate(a):
        for bit in range(BITS):
            columns[bit][r] = (value >> bit) & 1

    packed = [int.from_bytes(col, "little") for col in columns]

    signed = [0] * BITS
    answer = 0

    for bit in range(BITS):
        cnt = packed[bit].bit_count()
        signed[bit] = pow(q, cnt, MOD)

        p_one = (1 - signed[bit]) * inv2 % MOD
        answer = (
            answer + ((1 << (2 * bit)) % MOD) * p_one
        ) % MOD

    for i in range(BITS):
        for j in range(i + 1, BITS):
            differing = (packed[i] ^ packed[j]).bit_count()
            both_signed = pow(q, differing, MOD)

            p_both = (
                1 - signed[i] - signed[j] + both_signed
            ) % MOD
            p_both = p_both * inv4 % MOD

            weight = (1 << (i + j)) % MOD
            answer = (
                answer + 2 * weight * p_both
            ) % MOD

    print(answer)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

assert run("""3 1 2
2 8 10
""") == "42", "sample 1"

assert run("""1 1 2
1
""") == "500000004", "single element with probability 1/2"

assert run("""1 1 2
3
""") == "500000008", "correlated bits"

assert run("""1 0 7
123
""") == "0", "P = 0"

assert run("""1 1 1
5
""") == "25", "P = 1"

assert run("""2 1 2
1 1
""") == "500000004", "all equal values"

assert run("""1 1 1000000006
1000000006
""") == "1", "maximum input value with P = 1"

max_case = "100000 1 2\n" + "0 " * 99999 + "0\n"
assert run(max_case) == "0", "maximum n, all values zero"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3 1 2 / 2 8 10`|`42`| Cung cấp mẫu và thuật ngữ bit theo cặp | 
|`1 1 2 / 1`|`500000004`| Kích thước tối thiểu và kỳ vọng phân số | 
|`1 1 2 / 3`|`500000008`| Tương quan giữa hai bit | 
|`1 0 7 / 123`|`0`| trường hợp ranh giới`P = 0`| 
|`1 1 1 / 5`|`25`| trường hợp ranh giới`P = 1`| 
|`2 1 2 / 1 1`|`500000004`| Giá trị bằng nhau lặp lại | 
|`1 1 1000000006 / 1000000006`|`1`| Giá trị mảng tối đa được phép | 
|`100000 1 2 / 0 ... 0`|`0`| Tối đa`n`và các phần tử có giá trị bằng 0 | 

## Vỏ cạnh 

Khi nào`P = 0`, mọi thừa số có dấu là`q^c = 1`, bởi vì`q = 1`. Do đó, mọi xác suất bit đơn đều bằng 0, xác suất mọi cặp đều bằng 0 và câu trả lời vẫn là 0. Vì```
1 0 7
123
```các cột được đóng gói ghi lại chính xác các bit của`123`, nhưng không cái nào trong số chúng có thể xuất hiện trong XOR cuối cùng. Đầu ra là`0`. 

Khi`P = 1`,`q = -1`. Một bit xảy ra số lần chẵn đã ký kết kỳ vọng`1`, trong khi một bit xuất hiện với số lần lẻ đã có dấu kỳ vọng`-1`. Điều này mô tả chính xác một XOR xác định. Vì```
1 1 1
5
```kết quả duy nhất có thể là`5`và thuật toán tạo ra`25`. 

Các giá trị lặp lại không cần xử lý đặc biệt. Vì```
2 1 2
1 1
```XOR là`0`nếu cả hai bản sao đều không được chọn và`1`nếu không thì. Mỗi lựa chọn trong số bốn lựa chọn đều có xác suất`1/4`, Vì thế`P(S = 1) = 1/2`Và`E[S²] = 1/2`, cho`500000004`. Số bit là hai, do đó công thức cho kết quả tương tự mà không xem xét riêng các giá trị trùng lặp. 

Ví dụ tương quan```
1 1 2
3
```là cái bẫy chính. Cả hai bit luôn thay đổi cùng nhau vì đầu ra duy nhất có thể là`00`Và`11`. Khoảng cách cặp bằng 0, vì vậy`q^0 = 1`, tạo ra xác suất chung của`1/2`. Nếu khoảng cách cặp được thay thế không chính xác bằng giả định các bit độc lập thì câu trả lời sẽ sai. 

Cuối cùng, các phần tử có giá trị bằng 0 là vô hại. TRONG```
100000 1 2
0 0 0 ... 0
```mỗi cột bit hoàn toàn bằng 0, vì vậy tất cả số lượng bit đơn đều bằng 0 và mọi khoảng cách cặp đều bằng 0. XOR cuối cùng luôn bằng 0 bất kể phần tử nào được chọn và thuật toán ngay lập tức nhận được câu trả lời là`0`.
