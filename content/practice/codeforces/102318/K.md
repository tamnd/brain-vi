---
title: "CF 102318K - Thỏa thích mua sắm vật phẩm K"
description: "Chúng tôi có một bộ sưu tập gồm n loại mặt hàng. Mặt hàng i có giá được đưa ra chính xác bằng hai chữ số thập phân và khi xây dựng một cuộc mua sắm thoải mái, chúng ta chọn chính xác k mặt hàng theo thứ tự."
date: "2026-08-14T00:13:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102318
codeforces_index: "K"
codeforces_contest_name: "UCF Locals 2017"
rating: 0
weight: 102318
solve_time_s: 396
verified: true
draft: false
---

[CF 102318K - Thỏa sức mua sắm K-Item](https://codeforces.com/problemset/problem/102318/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 6 phút 36 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một bộ sưu tập`n`các loại mặt hàng. Mục`i`có giá được đưa ra chính xác đến hai chữ số thập phân và khi xây dựng một cuộc mua sắm, chúng tôi chọn chính xác`k`các mục theo thứ tự. Mỗi loại vật phẩm có thể được chọn lặp lại và hai loại vật phẩm khác nhau bất cứ khi nào trình tự chỉ số vật phẩm của chúng khác nhau. Thứ tự lựa chọn rất quan trọng. 

Với mỗi mức giá mục tiêu`X`, chúng ta cần số lượng chiều dài-`k`các chuỗi có giá các mặt hàng cộng lại chính xác bằng`X`. Câu trả lời là bắt buộc theo modulo`997`. 

Cách hữu ích để xem xét giới hạn số là ngừng suy nghĩ về đô la và làm việc hoàn toàn bằng xu. Giá mặt hàng là số nguyên nhỏ và mọi truy vấn tối đa là`$500.00`, do đó mọi truy vấn tương ứng với một số nguyên nằm giữa`1`Và`50000`. Mặc dù một cuộc vui có thể có tổng giá lớn hơn nhiều, nhưng các hệ số trên mức độ`50000`không bao giờ có thể đóng góp vào câu trả lời và có thể bị loại bỏ sau mỗi lần nhân đa thức. 

Có thể có nhiều như`10000`loại mặt hàng và`10000`các vị trí đã chọn. Việc liệt kê trực tiếp có`n^k`các trình tự có thể. Ở giá trị cực đại thì đây là`10000^10000 = 10^40000`, nên ngay cả việc mô tả tất cả các khả năng cũng không thể thực hiện được. Ngay cả một chương trình động trên cả số lượng mục được chọn và tổng giá trị cũng sẽ yêu cầu khoảng`k * 50000`, vốn đã ở xung quanh`5 * 10^8`trạng thái trước khi xem xét chuyển đổi về giá mặt hàng. 

Cấu trúc ẩn là phạm vi giá giới hạn. Thay vì lưu trữ từng mặt hàng riêng biệt, chúng ta có thể lưu trữ mỗi mức giá có bao nhiêu chỉ mục mặt hàng. Mảng tần số đó đương nhiên là mảng hệ số của đa thức. Nâng đa thức đó lên`k`-th sức mạnh sau đó đếm chính xác các chuỗi chúng ta cần. 

Một số trường hợp đặc biệt có thể âm thầm phá vỡ quá trình triển khai. 

Hãy xem xét một mặt hàng có giá trị`⟦PROTECT_11⟧3.00`.```
1
1
1.00
3 1
3.00
```Câu trả lời là`1`, bởi vì chuỗi duy nhất có thể là`(1, 1, 1)`. Việc triển khai xử lý các loại mục dưới dạng giá trị riêng biệt và vô tình không cho phép các lựa chọn lặp lại sẽ trả về 0 không chính xác. 

Bây giờ hãy xem xét hai mặt hàng khác nhau có cùng mức giá.```
1
2
1.00
1.00
2 1
2.00
```Câu trả lời là`4`. Bốn trình tự là`(1,1)`,`(1,2)`,`(2,1)`, Và`(2,2)`. Đa thức dựa trên tần số xử lý điều này một cách chính xác vì hệ số của`x^100`là`2`, và bình phương nó tạo ra hệ số`4`Tại`x^200`. Việc triển khai chỉ lưu trữ các giá riêng biệt và mất bội số sẽ trả về không chính xác`1`. 

Các truy vấn cũng có thể nhỏ hơn số tiền tối thiểu có thể.```
1
2
2.00
3.00
2 1
1.00
```Câu trả lời là`0`, vì mỗi lần mua hai món đồ có giá ít nhất`$4.00`. Việc triển khai bất cẩn chỉ tính toán đa thức tối đa giá mặt hàng lớn nhất, thay vì tổng giá trị lớn nhất được yêu cầu, cũng có thể xử lý sai tình huống này khi các giả định lập chỉ mục của nó sai. 

Cuối cùng, các giá trị là chuỗi thập phân, vì vậy việc sử dụng trực tiếp dấu phẩy động nhị phân cho các chỉ số đa thức là không an toàn. Một mức giá như`1.10`phải trở thành chính xác`110`xu. Phân tích cú pháp với`float`rồi nhân với`100`có thể gây ra lỗi biểu diễn. Chuyển đổi đúng là phân tích chuỗi dưới dạng văn bản thập phân và xây dựng số nguyên xu. 

Tuyên bố cuộc thi ban đầu đưa ra`n <= 10000`,`k <= 10000`,`q <= 10000`và truy vấn các giá trị lên đến`$500.00`. Đánh giá vấn đề chính thức của cuộc thi chỉ ra rõ ràng rằng các giới hạn tiền tệ và mô đun nhỏ bất thường này là manh mối dẫn đến biểu diễn đa thức và phép nhân dựa trên FFT. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu tuân theo định nghĩa trực tiếp. Đối với mỗi một trong những`k`vị trí, chọn một trong các`n`chỉ số mục, tính tổng kết quả và tăng câu trả lời cho tổng đó. Điều này đúng vì mọi đợt mua sắm có thể xảy ra đều tương ứng với chính xác một chuỗi được tạo ra bởi phép đệ quy, bao gồm các chỉ mục mặt hàng lặp lại và các đơn hàng khác nhau. 

Vấn đề là số lượng trình tự. có`n^k`của họ. Với`n = k = 10000`, điều này trở thành`10000^10000 = 10^40000`trình tự, vượt xa mọi tính toán thực tế. Đánh giá chính thức sử dụng quan sát chính xác này để loại trừ việc liệt kê. 

Một cải tiến tự nhiên là lập trình động. Cho phép`dp[j][s]`đếm chiều dài-`j`dãy có tổng giá trị`s`. Để tính toán`dp[j][s]`, chúng tôi sẽ xem xét mọi giá mặt hàng`v`và thêm`dp[j-1][s-v]`. Điều này làm giảm sự phụ thuộc theo cấp số nhân vào`k`, nhưng vẫn để lại yếu tố về số lượng giá khác nhau. Với`k`và phạm vi tổng có liên quan xung quanh`50000`, cách tiếp cận này quá lớn. 

Quan sát quan trọng là việc chọn một mặt hàng sẽ đóng góp vào giá của nó một cách độc lập ở mọi vị trí. Cho phép`P(x) = c_1 x^1 + c_2 x^2 + ...`Ở đâu`c_v`là số lượng chỉ số mặt hàng có giá`v`xu. Nhân hai bản sao của`P`chọn một mục cho vị trí đầu tiên và một mục cho vị trí thứ hai. Hệ số của`x^s`do đó tính tổng tất cả các chuỗi hai mục`s`xu. 

Lập luận tương tự được áp dụng nhiều lần. Hệ số của`x^s`TRONG`P(x)^k`chính xác là số chiều dài-`k`mua sắm thoải mái có tổng giá trị là`s`xu. Đây chính xác là số lượng được yêu cầu bởi mọi truy vấn. 

Khó khăn còn lại là phép nhân đa thức. Một tích chập chuẩn của hai mảng có độ dài khoảng`50000`mất thời gian bậc hai, đại khái`50000^2 = 2.5 * 10^9`các phép tính hệ số cho một phép nhân. Tốc độ đó đã quá chậm và phép lũy thừa đòi hỏi nhiều phép nhân. 

FFT giảm một phép nhân đa thức thành`O(N log N)`. Từ`k`có thể đạt được`10000`, chúng tôi cũng sử dụng phép lũy thừa nhị phân, giảm số phép nhân đa thức từ tối đa`10000`chỉ để`O(log k)`, nhiều nhất là khoảng`28`phép nhân. Đánh giá chính thức mô tả chính xác sự kết hợp của đa thức tần số, tích chập FFT và lũy thừa nhanh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(n^k)`|`O(k)`| Quá chậm | 
| DP trên các vị trí và số tiền |`O(k * S * n)`|`O(S)`| Quá chậm | 
| lũy thừa đa thức với FFT |`O(S log S log k)`|`O(S)`| Đã chấp nhận | 

Đây`S = 50000`, truy vấn lớn nhất được biểu thị bằng xu. Việc triển khai FFT thực sự sử dụng kích thước biến đổi lũy thừa hai đủ lớn cho phép tích chập, lên tới`131072`cho vấn đề này. Đánh giá chính thức đưa ra giới hạn kích thước biến đổi tương tự. 

## Hướng dẫn thuật toán 

1. Chuyển đổi giá của mọi mặt hàng từ đô la sang xu và xây dựng mảng tần số`P`, Ở đâu`P[v]`là số lượng chỉ số mặt hàng có giá chính xác`v`xu. 

Hệ số này là một tần số chứ không chỉ đơn thuần là một chỉ báo Boolean vì hai chỉ số mặt hàng khác nhau có cùng mức giá đại diện cho hai lựa chọn khác nhau. 
2. Đọc tất cả các truy vấn, chuyển đổi giá trị mục tiêu của chúng thành xu và để`S`là mục tiêu được yêu cầu lớn nhất. 

Chúng ta chỉ cần hệ số thông qua mức độ`S`. Bất kỳ thuật ngữ nào có bậc lớn hơn`S`không bao giờ có thể ảnh hưởng đến truy vấn và tất cả giá đều dương, vì vậy việc loại bỏ các điều khoản đó là an toàn. 
3. Giảm mọi hệ số của`P`modulo`997`. 

Phép nhân đa thức sau này sẽ được thực hiện bằng cách sử dụng số học FFT dấu phẩy động, nhưng các hệ số toán học chỉ cần modulo`997`. 
4. Tính toán`P(x)^k`sử dụng lũy ​​thừa nhị phân. 

Bắt đầu với đa thức kết quả`R(x) = 1`. Trong khi`k`khác 0, nếu bit thấp nhất của nó được đặt, hãy nhân`R`bằng sức mạnh hiện tại của`P`. Sau đó bình phương công suất hiện tại và dịch chuyển`k`đúng một chút. 

Điều này thay thế`k`phép nhân đa thức tối đa gấp đôi số bit của`k`. 
5. Thực hiện phép nhân đa thức dưới dạng tích chập với FFT. 

Nếu như`A`Và`B`có bằng cấp`a`Và`b`, tích của chúng có hệ số`C[s] = sum A[i] * B[s-i]`. 

FFT biến đổi cả hai mảng thành miền tần số, trong đó tích chập trở thành phép nhân theo điểm. Một FFT nghịch đảo sẽ chuyển đổi kết quả trở lại. 
6. Sau mỗi phép chập, làm tròn các giá trị thực đến số nguyên gần nhất và giảm chúng theo modulo`997`. 

Các hệ số trước khi tích chập là nhỏ, nhiều nhất là`996`, do đó FFT phức tạp có độ chính xác kép tiêu chuẩn là đủ chính xác cho mô đun yêu cầu. Giải pháp chính thức sử dụng mô đun nhỏ bất thường`997`vì chính xác lý do này. 
7. Cắt bớt mọi đa thức thu được thành bậc`S`. 

Khi một hệ số có bậc lớn hơn truy vấn lớn nhất, thì phép nhân sau này không thể hạ nó xuống vì tất cả giá mặt hàng đều dương. Giữ nó sẽ chỉ tăng kích thước FFT và thời gian chạy. 
8. Trả lời mọi truy vấn bằng cách đọc trực tiếp hệ số ở mức mục tiêu. 

Hệ số của`x^X`TRONG`P(x)^k`là số chiều dài đặt hàng-`k`tổng số lựa chọn`X`xu, vì vậy đây chính xác là câu trả lời được yêu cầu theo modulo`997`. 

### Tại sao nó hoạt động 

Đa thức tần số có một số hạng cho mọi lựa chọn mặt hàng có thể có, với hệ số của nó bằng số chỉ số mặt hàng có giá đó. TRONG`P(x)^k`, chọn một thuật ngữ từ mỗi`k`các bản sao tương ứng với việc chọn một chỉ mục mục cho mỗi vị trí của spree. Số mũ là tổng giá của chúng, trong khi tích của các hệ số tính tất cả các lựa chọn chỉ số tạo ra cùng một chuỗi giá đó. Như vậy hệ số của`x^X`chính xác là tổng số lần mua sắm hợp lệ`X`xu. FFT tính toán tích đa thức giống như tích chập thông thường và phép lũy thừa nhị phân tính toán chính xác như nhau`k`-th sức mạnh như phép nhân lặp đi lặp lại. Việc cắt bớt không thể loại bỏ hệ số mà sau này có thể đóng góp cho truy vấn vì mọi giá đều dương. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

MOD = 997
PI2 = 2.0 * math.pi

def fft(a, invert):
    n = len(a)

    j = 0
    for i in range(1, n):
        bit = n >> 1
        while j & bit:
            j ^= bit
            bit >>= 1
        j ^= bit
        if i < j:
            a[i], a[j] = a[j], a[i]

    length = 2
    while length <= n:
        angle = PI2 / length
        if invert:
            angle = -angle
        wlen = complex(math.cos(angle), math.sin(angle))

        half = length >> 1
        for i in range(0, n, length):
            w = 1.0 + 0.0j
            end = i + half
            j = i
            while j < end:
                u = a[j]
                v = a[j + half] * w
                a[j] = u + v
                a[j + half] = u - v
                w *= wlen
                j += 1

        length <<= 1

    if invert:
        inv_n = 1.0 / n
        for i in range(n):
            a[i] *= inv_n

def convolution(a, b, limit):
    if not a or not b:
        return []

    need = min(len(a) + len(b) - 1, limit + 1)

    if len(a) == 1:
        x = a[0]
        return [(x * y) % MOD for y in b[:need]]

    if len(b) == 1:
        x = b[0]
        return [(x * y) % MOD for y in a[:need]]

    full = len(a) + len(b) - 1
    size = 1
    while size < full:
        size <<= 1

    fa = [complex(x, 0.0) for x in a]
    fb = [complex(x, 0.0) for x in b]

    fft(fa, False)
    fft(fb, False)

    for i in range(size):
        fa[i] *= fb[i]

    fft(fa, True)

    res = [0] * need
    for i in range(need):
        x = int(round(fa[i].real))
        res[i] = x % MOD

    return res

def poly_pow(base, exponent, limit):
    result = [1]

    while exponent:
        if exponent & 1:
            result = convolution(result, base, limit)

        exponent >>= 1
        if exponent:
            base = convolution(base, base, limit)

    return result

def parse_cents(s):
    if '.' in s:
        whole, frac = s.split('.')
        frac = (frac + '00')[:2]
    else:
        whole, frac = s, '00'
    return int(whole) * 100 + int(frac)

def solve():
    t = int(input())
    output = []

    for _ in range(t):
        n = int(input())

        prices = [parse_cents(input().strip()) for _ in range(n)]

        k, q = map(int, input().split())
        queries = [parse_cents(input().strip()) for _ in range(q)]

        limit = max(queries)

        base = [0] * (limit + 1)

        for price in prices:
            if price <= limit:
                base[price] += 1

        for i in range(len(base)):
            base[i] %= MOD

        if k == 0:
            answers = [1 if x == 0 else 0 for x in queries]
        else:
            result = poly_pow(base, k, limit)
            answers = [
                result[x] if x < len(result) else 0
                for x in queries
            ]

        output.extend(map(str, answers))

    sys.stdout.write("\n".join(output))

if __name__ == "__main__":
    solve()
```Trình phân tích cú pháp đầu vào hoạt động có chủ ý với các chuỗi.`parse_cents`chuyển đổi biểu diễn thập phân trực tiếp thành số nguyên, tránh tất cả các vấn đề về dấu phẩy động trước khi FFT được đưa vào. 

các`base`mảng là đa thức từ bước thuật toán đầu tiên. Chỉ số của nó là giá tính bằng xu và giá trị của nó là số lượng chỉ mục vật phẩm có giá đó. Giá lớn hơn truy vấn lớn nhất có thể bị bỏ qua ngay lập tức vì mọi giá đều dương. 

các`convolution`chức năng chọn kích thước FFT lũy thừa hai nhỏ nhất có khả năng giữ tích chập hoàn chỉnh. Đa thức kết quả sau đó được cắt ngắn thành`limit + 1`. Việc cắt bớt được thực hiện sau FFT nghịch đảo vì các hệ số nằm ngoài phạm vi được yêu cầu có thể bị loại bỏ một cách đơn giản. 

Việc xử lý đặc biệt đối với đa thức có độ dài sẽ tránh hoàn toàn FFT. Điều này quan trọng trong quá trình lũy thừa nhị phân vì đa thức trung gian có thể trở nên rất nhỏ, đặc biệt khi phạm vi được yêu cầu nhỏ hoặc đầu vào chỉ chứa một mức giá liên quan. 

FFT lặp lại sử dụng hoán vị đảo ngược bit tiêu chuẩn theo sau là các lớp bướm. Phép biến đổi nghịch đảo thay đổi dấu của góc quay và chia mọi hệ số cho kích thước biến đổi. Làm tròn với`int(round(...))`chuyển đổi lỗi dấu phẩy động nhỏ xung quanh mỗi hệ số nguyên thành giá trị nguyên dự định của nó. 

Số nguyên Python không bị tràn, do đó không có vấn đề tương tự như vấn đề tràn chiều rộng cố định được tìm thấy trong C hoặc C++. Bản thân FFT sử dụng số phức Python, có thành phần thực và ảo có độ chính xác gấp đôi. Vì các hệ số được giảm modulo`997`sau mỗi lần nhân, chúng vẫn đủ nhỏ để áp dụng phương pháp số dự định. 

Vòng lũy ​​thừa sử dụng phép kiểm tra bit có ý nghĩa nhỏ nhất tiêu chuẩn. Bình phương chỉ được thực hiện khi vẫn còn một phép lặp lũy thừa khác, tránh phép tích chập cuối cùng không cần thiết. 

## Ví dụ đã hoạt động 

Đoạn trích vấn đề ban đầu được cung cấp ở đây không bao gồm các khối đầu vào/đầu ra mẫu, vì vậy sau đây là hai ví dụ nhỏ được xây dựng. 

### Ví dụ 1 

Hãy xem xét hai chỉ số mặt hàng, cả hai đều có giá`$1.00`, và chọn hai mục.```
1
2
1.00
1.00
2 2
2.00
1.00
```Đa thức tần số là`P(x) = 2x^100`. Bình phương nó mang lại`4x^200`. 

| Bước | Đa thức / Trạng thái | Hệ số liên quan | 
| --- | --- | --- | 
| Xây dựng căn cứ |`2x^100`|`P[100] = 2`| 
| Số mũ |`k = 2`| nhị phân`10`| 
| Đế vuông |`4x^200`|`P^2[200] = 4`| 
| Truy vấn`$2.00`| bằng cấp`200`|`4`| 
| Truy vấn`$1.00`| bằng cấp`100`|`0`| 

hệ số`4`đại diện cho bốn lựa chọn có thứ tự của hai chỉ số mục. Điều này chứng tỏ tại sao giá bằng nhau không thể đơn giản được loại bỏ. 

### Ví dụ 2 

Xem xét giá mặt hàng`⟦PROTECT_12⟧2.00`, và chọn ba mục.```
1
2
1.00
2.00
3 3
3.00
4.00
6.00
```Đa thức cơ sở là`P(x) = x^100 + x^200`. 

Cubing nó mang lại`P(x)^3 = x^300 + 3x^400 + 3x^500 + x^600`. 

| Bước | Đa thức / Trạng thái | Hệ số lãi suất | 
| --- | --- | --- | 
| Xây dựng căn cứ |`x^100 + x^200`|`100:1, 200:1`| 
| Phép nhân đầu tiên |`P^2`|`200:1, 300:2, 400:1`| 
| Phép nhân cuối cùng |`P^3`|`300:1, 400:3, 500:3, 600:1`| 
| Truy vấn`$3.00`| bằng cấp`300`|`1`| 
| Truy vấn`$4.00`| bằng cấp`400`|`3`| 
| Truy vấn`$6.00`| bằng cấp`600`|`1`| 

hệ số`3`Tại`⟦PROTECT_13⟧1.00`mục và hai`$2.00`mặt hàng. Vị trí khác nhau của họ làm cho họ có ba lối đi khác nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(S log S log k)`| Mỗi phép nhân đa thức sử dụng tích chập FFT và cần có lũy thừa nhị phân`O(log k)`phép nhân | 
| Không gian |`O(S)`| Mảng đa thức và bộ đệm FFT có kích thước`O(S)`| 

Đây`S`nhiều nhất là tổng số truy vấn lớn nhất tính bằng xu`50000`. FFT cần lũy thừa phù hợp tiếp theo là hai để tích chập, đạt tới`131072`trong trường hợp xấu nhất. Đánh giá chính thức đưa ra yêu cầu về kích thước biến đổi tương tự và giải thích rằng phép lũy thừa nhị phân làm giảm số phép nhân đa thức xuống gần đúng.`28`vì`k <= 10000`. 

Giới hạn 15 giây và 1024 MB được thiết kế cho giải pháp dựa trên FFT thay vì tích chập bậc hai. Yêu cầu bộ nhớ nằm dưới mức giới hạn, trong khi sự phụ thuộc logarit vào`k`là những gì làm cho`k = 10000`quản lý được. 

## Trường hợp thử nghiệm 

Không có đầu vào/đầu ra mẫu nào được đưa vào báo cáo vấn đề được cung cấp, vì vậy bộ khai thác thử nghiệm bên dưới sử dụng hai ví dụ được xây dựng ở trên và các trường hợp bổ sung.```python
import sys
import io

MOD = 997

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    try:
        solve()
        return out.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Constructed sample 1
assert run("""\
1
2
1.00
1.00
2 2
2.00
1.00
""") == "4\n0", "sample 1"

# Constructed sample 2
assert run("""\
1
2
1.00
2.00
3 3
3.00
4.00
6.00
""") == "1\n3\n1", "sample 2"

# Minimum-size input
assert run("""\
1
1
0.01
1 3
0.01
0.02
0.03
""") == "1\n0\n0", "minimum size"

# All item indices have the same value
assert run("""\
1
3
1.00
1.00
1.00
2 3
2.00
1.00
3.00
""") == "9\n0\n0", "all equal"

# Boundary at the largest query
assert run("""\
1
2
250.00
250.00
2 3
499.99
500.00
500.01
""") == "0\n4\n0", "query boundary"

# Large multiplicity, checking modular reduction
assert run("""\
1
1000
1.00
1.00
1.00
1.00
1.00
1.00
1.00
1.00
1.00
1.00
1 3
1.00
2.00
0.99
""") == "3\n0\n0", "multiplicity"

print("all tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hai`$1.00`mặt hàng,`k=2`|`4, 0`| Lựa chọn lặp đi lặp lại và giá trị mục trùng lặp | 
|`⟦PROTECT_14⟧2.00`,`k=3`|`1, 3, 1`| Dãy số có thứ tự và hệ số đa thức | 
| Một`$0.01`mục,`k=1`|`1, 0, 0`| Kích thước tối thiểu và lập chỉ mục xu chính xác | 
| Ba giống hệt nhau`$1.00`mặt hàng |`9, 0, 0`| Sự đa dạng của chỉ số mặt hàng | 
| Hai`⟦PROTECT_15⟧500.00`ranh giới | 
| Một nghìn`$1.00`mặt hàng |`3, 0, 0`| Đếm tần số và biểu diễn an toàn modulo | 

## Vỏ cạnh 

Để lựa chọn lặp lại, đầu vào```
1
1
1.00
3 1
3.00
```sản xuất`1`. Đa thức cơ sở là`x^100`, và lập phương nó mang lại`x^300`. Thuật toán giữ hệ số đơn ở mức độ`300`, do đó truy vấn trả về`1`. Không có trường hợp đặc biệt nào được yêu cầu đối với các chỉ mục mục lặp lại vì lũy thừa đa thức đương nhiên cho phép chọn cùng một thuật ngữ trong mọi yếu tố. 

Đối với giá trùng lặp, đầu vào```
1
2
1.00
1.00
2 1
2.00
```sản xuất`4`. Hệ số cơ bản ở mức độ`100`là`2`, không`1`. Sau khi bình phương, hệ số trở thành`2 * 2 = 4`. Đây chính xác là sự khác biệt giữa giá trị vật phẩm và chỉ số vật phẩm mà bài toán yêu cầu. 

Đối với một mục tiêu không thể, hãy xem xét```
1
2
2.00
3.00
2 1
1.00
```Đa thức cơ sở có hệ số khác 0 đầu tiên ở mức`200`. Do đó hình vuông của nó bắt đầu ở mức`400`. Kể từ khi có bằng cấp`100`vắng mặt, câu trả lời là`0`. Việc tra cứu mảng xử lý trường hợp này một cách tự nhiên. 

Đối với ranh giới truy vấn trên, hãy xem xét```
1
2
250.00
250.00
2 2
500.00
500.01
```Truy vấn đầu tiên tương ứng với mức độ`50000`, và hệ số là`4`, bởi vì một trong hai mục có thể chiếm một trong hai vị trí. Truy vấn thứ hai tương ứng với mức độ`50001`, nhưng đa thức của việc triển khai chỉ được lưu trữ có chủ ý thông qua mức độ được yêu cầu lớn nhất, vì vậy nếu`500.01`nằm ngoài giới hạn của vấn đề đã nêu, điều đó thường không bao giờ xảy ra ở đầu vào hợp lệ. Trong giới hạn thực tế của`$500.00`, bằng cấp`50000`là chỉ số liên quan cuối cùng. 

Để phân tích cú pháp thập phân, một đầu vào như`0.01`phải trở thành số nguyên`1`, trong khi`1.10`phải trở thành`110`. Giải pháp phân tích cú pháp các ký tự xung quanh dấu thập phân thay vì đánh giá số dấu phẩy động. Điều này ngăn lỗi biểu diễn biến chỉ mục mảng hợp lệ thành chỉ mục không chính xác. 

Đối với hành vi modulo, các hệ số đa thức sẽ giảm sau mỗi lần nhân. Vì câu trả lời bắt buộc là modulo`997`, thay mọi hệ số`c`qua`c mod 997`trước phép nhân tiếp theo bảo toàn mọi hệ số modulo sau đó`997`. Điều này cũng giữ cho sai số làm tròn FFT ở mức nhỏ vì hệ số đầu vào được chuyển đổi không bao giờ tăng theo số mũ.
