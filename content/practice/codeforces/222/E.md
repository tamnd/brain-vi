---
title: "CF 222E - Giải mã bộ gen"
description: "Chúng ta có một bảng chữ cái có kích thước m, bao gồm m ký hiệu đầu tiên từ dãy a..zA..Z. Một số cặp ký hiệu có thứ tự bị cấm. Nếu cặp (x, y) bị cấm thì ký hiệu y không thể xuất hiện ngay sau ký hiệu x trong chuỗi DNA."
date: "2026-06-04T02:06:00+07:00"
tags: ["codeforces", "competitive-programming", "dp", "matrices"]
categories: ["algorithms"]
codeforces_contest: 222
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 137 (Div. 2)"
rating: 1900
weight: 222
solve_time_s: 115
verified: true
draft: false
---

[CF 222E - Giải mã bộ gen](https://codeforces.com/problemset/problem/222/E) 

**Xếp hạng:** 1900 
**Thẻ:** dp, ma trận 
**Thời gian giải:** 1 phút 55s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một bảng chữ cái có kích thước`m`, bao gồm phần đầu tiên`m`ký hiệu từ chuỗi`a..zA..Z`. Một số cặp ký hiệu có thứ tự bị cấm. Nếu cặp`(x, y)`bị cấm thì biểu tượng`y`không thể xuất hiện ngay sau biểu tượng`x`trong chuỗi DNA. 

Nhiệm vụ là đếm xem chuỗi DNA có độ dài bao nhiêu`n`thỏa mãn mọi ràng buộc kề cận. Câu trả lời phải được báo cáo modulo`10^9 + 7`. 

Một cách hữu ích để xem vấn đề là dưới dạng đồ thị có hướng. Mỗi nucleotide là một đỉnh. Có một cạnh định hướng từ đỉnh`u`đến đỉnh`v`nếu cặp`(u, v)`được cho phép. Trình tự DNA hợp lệ tương ứng với bước đi trong biểu đồ này. Mỗi cặp ký tự liền kề trong chuỗi phải tuân theo một cạnh cho phép. 

Các ràng buộc hoàn toàn xác định giải pháp dự định. Kích thước bảng chữ cái nhiều nhất là`52`, nó rất nhỏ. Mặt khác,`n`có thể lớn như`10^15`, loại trừ bất kỳ chương trình động nào lặp lại từng vị trí một. Thậm chí một`O(n)`thuật toán là không thể. Bất cứ khi nào một bài toán kết hợp một không gian trạng thái rất nhỏ với độ dài khổng lồ thì phép lũy thừa ma trận thường là hướng đúng. 

Có một số trường hợp khó xử lý. 

Hãy xem xét một DNA có độ dài một:```
1 3 2
ab
ba
```Câu trả lời là`3`, không bị ảnh hưởng bởi bất kỳ cặp cấm nào. Một chuỗi có độ dài bằng 1 không chứa cặp liền kề nào cả. 

Một trường hợp quan trọng khác là khi mọi chuyển đổi đều bị cấm:```
2 1 1
aa
```Câu trả lời là`0`. Có một ký hiệu, nhưng nó không thể tự theo sau, vì vậy không tồn tại chuỗi dài hai. 

Trường hợp thứ ba là khi`n`cực kỳ lớn nhưng`m = 1`:```
1000000000000000 1 0
```Có chính xác một chuỗi hợp lệ, đó là`"aaaa..."`. Bất kỳ giải pháp nào lặp đi lặp lại các vị trí sẽ không bao giờ kết thúc. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là lập trình động trên các vị trí. 

Cho phép`dp[i][j]`là số chuỗi có độ dài hợp lệ`i`kết thúc bằng ký hiệu`j`. Để tính toán lớp tiếp theo, chúng tôi thử mọi chuyển đổi được phép:$$dp[i+1][v] += dp[i][u]$$Điều này đúng vì mọi chuỗi hợp lệ đều kết thúc tại`u`có thể được mở rộng để`v`bất cứ khi nào`(u,v)`được cho phép. 

Vấn đề là giá trị của`n`. Ngay cả khi chúng tôi tối ưu hóa quá trình chuyển đổi bằng cách sử dụng kích thước bảng chữ cái nhỏ, thời gian chạy vẫn tỷ lệ thuận với độ dài của DNA. Với`n`lên đến`10^{15}`, điều này thật vô vọng. 

Quan sát quan trọng là quá trình chuyển đổi DP là tuyến tính. Nếu chúng ta lưu trữ số đếm cho tất cả các ký hiệu kết thúc trong một vectơ thì một bước DP chỉ đơn giản là phép nhân ma trận. 

Cho phép`A`là`m × m`ma trận chuyển tiếp:$$A[u][v] = \begin{cases} 1 & \text{if transition } u \to v \text{ is allowed}\\ 0 & \text{otherwise} \end{cases}$$Nếu như`f_i`là vectơ hàng đếm theo chiều dài`i`, sau đó$$f_{i+1} = f_i A$$Áp dụng quá trình chuyển đổi nhiều lần sẽ mang lại$$f_n = f_1 A^{n-1}$$Hiện nay giá trị khổng lồ của`n`trở nên có thể quản lý được. Từ`m ≤ 52`, chúng ta có thể lũy thừa ma trận bằng cách sử dụng lũy ​​thừa nhị phân trong`O(m^3 log n)`thời gian. 

Vectơ ban đầu cho độ dài một chứa tất cả những cái một, bởi vì bất kỳ ký hiệu nào cũng có thể được chọn làm ký tự đầu tiên. 

Sau khi tính toán`A^(n-1)`, chúng ta nhân nó với vectơ ban đầu và tính tổng tất cả các số đếm cuối cùng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force DP trên các vị trí | O(nm²) | O(m) | Quá chậm | 
| lũy thừa ma trận | O(m³ log n) | O(m2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo ánh xạ chỉ mục cho các ký tự. Biểu tượng`'a'..'z'`bản đồ tới`0..25`, Và`'A'..'Z'`bản đồ tới`26..51`. 
2. Xây dựng một`m × m`ma trận chuyển tiếp`A`. 

Ban đầu mỗi mục là`1`, nghĩa là mọi chuyển đổi đều được phép. 
3. Đọc từng cặp cấm`(x, y)`và đặt mục nhập ma trận tương ứng thành`0`. 

Ma trận bây giờ thể hiện chính xác biểu đồ của các chuyển đổi được phép. 
4. Xử lý trường hợp đặc biệt`n = 1`. 

Mỗi ký hiệu riêng lẻ tạo thành một chuỗi DNA hợp lệ, vì vậy câu trả lời đơn giản là`m`. 
5. Tính toán`A^(n-1)`sử dụng lũy ​​thừa nhị phân. 

Mỗi phép nhân ma trận được thực hiện theo modulo`10^9 + 7`. 
6. Tạo vector hàng đầu tiên$$f_1 = [1,1,\ldots,1]$$bởi vì bất kỳ ký hiệu nào cũng có thể được chọn làm ký tự đầu tiên. 
7. Nhân lên`f_1`qua`A^(n-1)`. 

Vectơ kết quả chứa số độ dài hợp lệ-`n`chuỗi kết thúc bằng mỗi ký hiệu. 
8. Tính tổng tất cả các phần tử của vectơ modulo kết quả`10^9 + 7`. 

Điều này cho tổng số chuỗi DNA hợp lệ. 

### Tại sao nó hoạt động 

Ma trận chuyển tiếp mã hóa chính xác sự lặp lại giống như chương trình động thông thường. Lối vào`(u,v)`bằng một chính xác khi một chuỗi kết thúc tại`u`có thể được mở rộng bằng ký hiệu`v`. 

Nhân một vectơ đếm với`A`thực hiện một chuyển đổi DP. nhân với`A²`thực hiện hai lần chuyển tiếp. Bằng quy nạp, nhân với`A^k`thực hiện`k`chuyển tiếp DP liên tiếp. Từ một chiều dài-`n`chuỗi bao gồm một ký hiệu ban đầu theo sau là`n-1`chuyển tiếp, vectơ đếm chiều dài`n`chính xác là$$f_1 A^{n-1}.$$Tính tổng vectơ cuối cùng sẽ đếm tất cả các chuỗi hợp lệ bất kể ký hiệu kết thúc của chúng là gì. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 1000000007

def char_id(c):
    if 'a' <= c <= 'z':
        return ord(c) - ord('a')
    return ord(c) - ord('A') + 26

def mat_mul(a, b):
    n = len(a)
    res = [[0] * n for _ in range(n)]

    for i in range(n):
        ai = a[i]
        ri = res[i]

        for k in range(n):
            if ai[k] == 0:
                continue

            val = ai[k]
            bk = b[k]

            for j in range(n):
                ri[j] = (ri[j] + val * bk[j]) % MOD

    return res

def mat_pow(mat, exp):
    n = len(mat)

    res = [[0] * n for _ in range(n)]
    for i in range(n):
        res[i][i] = 1

    while exp:
        if exp & 1:
            res = mat_mul(res, mat)

        mat = mat_mul(mat, mat)
        exp >>= 1

    return res

def solve():
    n, m, k = map(int, input().split())

    trans = [[1] * m for _ in range(m)]

    for _ in range(k):
        s = input().strip()
        u = char_id(s[0])
        v = char_id(s[1])
        trans[u][v] = 0

    if n == 1:
        print(m)
        return

    power = mat_pow(trans, n - 1)

    ans = 0

    for col in range(m):
        total = 0
        for row in range(m):
            total += power[row][col]
        ans = (ans + total) % MOD

    print(ans)

solve()
```Ma trận chuyển tiếp được xây dựng trực tiếp từ thông tin cặp cấm. Mỗi vùng lân cận được phép đóng góp một`1`, mọi vùng lân cận bị cấm đều đóng góp một`0`. 

Thủ tục lũy thừa sử dụng mẫu lũy thừa nhị phân tiêu chuẩn. Từ`n`có thể lớn như`10^15`, chỉ cần khoảng năm mươi ma trận bình phương. 

Tính toán cuối cùng đáng được chú ý. Vectơ ban đầu bao gồm toàn bộ những đơn vị. Nhân một vectơ hàng của tất cả những số 1 với ma trận lũy thừa tương đương với việc tính tổng từng cột của ma trận đó. Việc triển khai khai thác thực tế này và tránh phép nhân ma trận vectơ bổ sung. 

Tất cả số học được thực hiện modulo`10^9 + 7`. Số nguyên Python không bị tràn, nhưng việc giảm modulo ở mỗi lần tích lũy sẽ giúp tính toán hiệu quả. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3 2
ab
ba
```Ma trận chuyển tiếp được phép: 

| Từ \ Đến | một | b | c | 
| --- | --- | --- | --- | 
| một | 1 | 0 | 1 | 
| b | 0 | 1 | 1 | 
| c | 1 | 1 | 1 | 

Sau khi lũy thừa chúng ta cần`A²`. 

| Bước | Ma trận | 
| --- | --- | 
| A | chuyển tiếp ở trên | 
| A² | số lần đi bộ dài 2 | 

Tính tổng cột của`A²`mang lại: 

| Cột | Tổng hợp | 
| --- | --- | 
| một | 5 | 
| b | 5 | 
| c | 7 | 

Tổng số: 

| Giá trị | 
| --- | 
| 5 + 5 + 7 = 17 | 

Đầu ra:```
17
```Dấu vết này cho thấy rằng lũy ​​thừa ma trận đếm số bước đi của nhiều bước. Câu trả lời cuối cùng là tổng số chuỗi có độ dài ba. 

### Ví dụ 2 

đầu vào:```
2 3 0
```Mọi chuyển đổi đều được cho phép. 

Ma trận chuyển tiếp: 

| Từ \ Đến | một | b | c | 
| --- | --- | --- | --- | 
| một | 1 | 1 | 1 | 
| b | 1 | 1 | 1 | 
| c | 1 | 1 | 1 | 

Từ`n = 2`, chúng tôi cần`A¹`. 

| Cột | Tổng hợp | 
| --- | --- | 
| một | 3 | 
| b | 3 | 
| c | 3 | 

Tổng số: 

| Giá trị | 
| --- | 
| 9 | 

Đầu ra:```
9
```Điều này phù hợp với số lượng rõ ràng`3²`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m³ log n) | lũy thừa ma trận với phép nhân ma trận bậc ba | 
| Không gian | O(m2) | Lưu trữ một số`m × m`ma trận | 

Từ`m ≤ 52`, phép nhân ma trận tốn khoảng`52³ ≈ 140,000`các phép toán số học. Phép lũy thừa nhị phân thực hiện khoảng`log₂(10¹⁵) ≈ 50`lần lặp lại. Điều này thoải mái phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

MOD = 1000000007

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def char_id(c):
        if 'a' <= c <= 'z':
            return ord(c) - ord('a')
        return ord(c) - ord('A') + 26

    def mat_mul(a, b):
        n = len(a)
        res = [[0] * n for _ in range(n)]

        for i in range(n):
            for k in range(n):
                if a[i][k] == 0:
                    continue
                for j in range(n):
                    res[i][j] = (res[i][j] + a[i][k] * b[k][j]) % MOD

        return res

    def mat_pow(mat, exp):
        n = len(mat)
        res = [[0] * n for _ in range(n)]

        for i in range(n):
            res[i][i] = 1

        while exp:
            if exp & 1:
                res = mat_mul(res, mat)
            mat = mat_mul(mat, mat)
            exp >>= 1

        return res

    n, m, k = map(int, input().split())

    trans = [[1] * m for _ in range(m)]

    for _ in range(k):
        s = input().strip()
        trans[char_id(s[0])][char_id(s[1])] = 0

    if n == 1:
        return str(m)

    power = mat_pow(trans, n - 1)

    ans = 0
    for col in range(m):
        for row in range(m):
            ans = (ans + power[row][col]) % MOD

    return str(ans)

# sample
assert run("3 3 2\nab\nba\n") == "17"

# minimum size
assert run("1 1 0\n") == "1"

# single symbol, self-transition forbidden
assert run("2 1 1\naa\n") == "0"

# all transitions allowed
assert run("2 3 0\n") == "9"

# n=1 ignores restrictions
assert run("1 2 4\naa\nab\nba\nbb\n") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 0`|`1`| Ví dụ nhỏ nhất có thể | 
|`2 1 1 / aa`|`0`| Vòng tự cấm | 
|`2 3 0`|`9`| Đồ thị được kết nối đầy đủ | 
|`1 2 4 ...`|`2`| Độ dài một bỏ qua các hạn chế kề cận | 

## Vỏ cạnh 

Hãy xem xét:```
1 2 4
aa
ab
ba
bb
```Mọi chuyển đổi đều bị cấm, tuy nhiên câu trả lời là`2`. Thuật toán trả về ngay lập tức khi`n = 1`, vì không có phần kề nào tồn tại trong chuỗi một ký tự. Một giải pháp lũy thừa hoặc áp dụng các phép chuyển đổi một cách mù quáng có thể tạo ra số không không chính xác. 

Coi như:```
2 1 1
aa
```Ma trận chuyển tiếp là:$$[0]$$Từ`n-1 = 1`, ma trận cấp nguồn vẫn`[0]`. Tổng cột bằng 0 nên câu trả lời là 0. Điều này nắm bắt chính xác thực tế là cặp liền kề duy nhất có thể bị cấm. 

Coi như:```
1000000000000000 1 0
```Ma trận là:$$[1]$$Mọi lũy thừa của ma trận này vẫn còn`[1]`. Phép lũy thừa nhị phân xử lý số mũ lớn theo thời gian logarit, đưa ra câu trả lời`1`. Đây chính xác là kịch bản loại bỏ mọi phương pháp lập trình động theo từng vị trí.
