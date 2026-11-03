---
title: "CF 1096G - Vé may mắn"
description: "Chúng tôi đang xây dựng các chuỗi chữ số có độ dài chẵn cố định bằng cách sử dụng bảng chữ cái chữ số bị hạn chế. Chuỗi đại diện cho một số vé, nhưng quy tắc cấu trúc duy nhất quan trọng là mỗi chữ số được phép sử dụng bao nhiêu lần trong mỗi nửa chuỗi."
date: "2026-06-13T05:50:57+07:00"
tags: ["codeforces", "competitive-programming", "divide-and-conquer", "dp", "fft"]
categories: ["algorithms"]
codeforces_contest: 1096
codeforces_index: "G"
codeforces_contest_name: "Educational Codeforces Round 57 (Rated for Div. 2)"
rating: 2400
weight: 1096
solve_time_s: 879
verified: true
draft: false
---

[CF 1096G - Vé may mắn](https://codeforces.com/problemset/problem/1096/G) 

**Đánh giá:** 2400 
**Thẻ:** chia để trị, dp, fft 
**Thời gian giải:** 14 phút 39 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang xây dựng các chuỗi chữ số có độ dài chẵn cố định bằng cách sử dụng bảng chữ cái chữ số bị hạn chế. Chuỗi đại diện cho một số vé, nhưng quy tắc cấu trúc duy nhất quan trọng là mỗi chữ số được phép sử dụng bao nhiêu lần trong mỗi nửa chuỗi. 

Một tấm vé có chiều dài$n$được chia thành hai nửa chiều dài$n/2$. Chúng ta muốn đếm xem có thể tạo ra bao nhiêu chuỗi có độ dài đầy đủ hợp lệ sao cho tổng các chữ số ở nửa bên trái bằng tổng các chữ số ở nửa bên phải. Các chữ số có thể lặp lại và cho phép các số 0 đứng đầu nếu số 0 là một phần của tập hợp chữ số được phép. 

Từ quan điểm tổ hợp, vấn đề là về các chuỗi có độ dài$n$trên một bảng chữ cái có kích thước nhỏ$k \le 10$, với một ràng buộc là tổng của nửa đầu bằng tổng của nửa sau. 

Các ràng buộc lớn về chiều dài:$n$có thể đạt được$2 \cdot 10^5$. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào xử lý rõ ràng từng chuỗi hoặc thậm chí liệt kê các nửa chuỗi. Ngay cả việc lưu trữ tất cả một nửa số tiền có thể có trong một DP đơn giản phụ thuộc vào$n$và số tiền tối đa quá lớn trừ khi nó được cấu trúc cẩn thận. Quan sát quan trọng là bảng chữ cái rất nhỏ nên phạm vi tổng có thể quản lý được nhưng số cách để đạt được mỗi tổng là rất lớn, đòi hỏi tích chập đa thức hoặc dựa trên FFT. 

Một cách tiếp cận ngây thơ sẽ cố gắng liệt kê tất cả các nửa bên trái và nửa bên phải một cách riêng biệt và khớp các khoản tiền. Điều đó không thành công vì mỗi nửa có$k^{n/2}$khả năng, đó là lớn về mặt thiên văn. Ngay cả một DP theo chiều dài và tổng bằng$O(n \cdot 9n)$tiểu bang quá lớn bởi vì$n^2$ở xung quanh$4 \cdot 10^{10}$. 

Trường hợp cạnh tinh tế là khi bao gồm chữ số 0. Nó không thay đổi tổng nhưng làm tăng bội số tổ hợp. Bất kỳ giải pháp nào bỏ qua bội số chữ số và chỉ theo dõi số tiền có thể truy cập sẽ bị tính thiếu nghiêm trọng. Một cạm bẫy khác là giả sử tính đối xứng ngụ ý việc chia tách số đếm một cách trực tiếp mà không cần tích chập, điều này sẽ bị phá vỡ vì sự phân bố khác nhau của các chữ số đóng góp khác nhau vào cùng một tổng. 

## Phương pháp tiếp cận 

Cấu trúc của vấn đề tự nhiên tách thành hai nửa giống hệt nhau. Nếu chúng ta biết, với mỗi số tiền có thể$s$, có bao nhiêu cách để tạo thành một dãy nửa độ dài có tổng các chữ số$s$, thì câu trả lời đầy đủ sẽ có được bằng cách ghép hai nửa độc lập có cùng số tiền. Điều này chuyển vấn đề thành tính toán tích chập của một phân phối với chính nó. 

Một cách tiếp cận bạo lực xây dựng tất cả các chuỗi có độ dài$n/2$và tính tổng của chúng. Cái đó đã tốn rồi$k^{n/2}$, điều này là không thể ngay cả đối với nhỏ$n$. Ngay cả khi chúng ta chuyển sang lập trình động, chúng ta vẫn xác định$dp[i][s]$là số cách để tạo thành độ dài$i$với tổng$s$. Quá trình chuyển đổi rất đơn giản, nhưng số lượng trạng thái tăng lên$O(n^2)$và mỗi chi phí chuyển tiếp$O(k)$, cho$O(n^2 k)$, vẫn còn quá chậm. 

Hiểu biết sâu sắc về cấu trúc quan trọng là DP của một nửa là tích chập lặp lại của một đa thức cơ số cố định. Mỗi vị trí đóng góp độc lập một chữ số từ tập hợp được phép, do đó hàm tạo cho một vị trí là đa thức trong đó hệ số của$x^d$là 1 nếu chữ số$d$được cho phép. Hàm tạo cho$n/2$vị trí là đa thức này được nâng lên lũy thừa$n/2$. Chúng ta chỉ cần các hệ số của nó, có thể được tính bằng cách sử dụng phép chia và lũy thừa kết hợp với phép nhân đa thức dựa trên FFT theo modulo$998244353$, đây là số nguyên tố thân thiện với NTT. 

Khi chúng ta có mảng hệ số$A[s]$trong một nửa, câu trả lời là tổng của tất cả$s$của$A[s]^2$, vì chúng ta độc lập chọn nửa bên trái và nửa bên phải có cùng số tiền. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Liệt kê lực lượng vũ phu |$O(k^{n/2})$|$O(n)$| Quá chậm | 
| DP theo chiều dài và tổng |$O(n^2 k)$|$O(n^2)$| Quá chậm | 
| Chia để trị + NTT |$O(S \log n \log S)$|$O(S)$| Đã chấp nhận | 

Đây$S$là tổng tối đa có thể, giới hạn bởi$9n/2$. 

## Hướng dẫn thuật toán 

Chúng tôi nén vấn đề vào việc tính toán lũy thừa đa thức dưới dạng tích chập. 

1. Xây dựng đa thức ban đầu$P(x)$trong đó mỗi chữ số được phép$d$đóng góp hệ số 1 tại vị trí$x^d$. Đa thức này biểu thị một vị trí duy nhất trong nửa vé. 
2. Xác định số mũ$t = n/2$. chúng tôi muốn$P(x)^t$, nhưng chỉ các hệ số đến mức$9t$vấn đề, vì số tiền không thể vượt quá số tiền đó. 
3. Sử dụng phép chia để trị lũy thừa của đa thức. Chúng tôi tính toán đệ quy$P^t$bằng cách chia số mũ thành hai nửa. Ở mỗi bước, nhân hai đa thức trung gian. 
4. Mỗi phép nhân được thực hiện bằng NTT theo modulo$998244353$. Điều này giúp tích chập hiệu quả ngay cả đối với các đa thức bậc lớn. 
5. Sau khi tính toán mảng hệ số$A[s]$của$P(x)^t$, giải thích$A[s]$là số cách để xây dựng một nửa bằng tổng$s$. 
6. Tính đáp án cuối cùng là$\sum_s A[s] \cdot A[s]$, vì nửa bên trái và nửa bên phải độc lập nhưng phải có tổng bằng nhau. 

Lý do quan trọng khiến tích chập xuất hiện là các lựa chọn chữ số trên các vị trí là độc lập và tổng cộng tuyến tính. Phép cộng độc lập lặp đi lặp lại tương ứng chính xác với phép nhân đa thức. 

### Tại sao nó hoạt động 

Mỗi hệ số trong$P(x)^t$đếm số cách chọn$t$các chữ số có tổng bằng một giá trị cố định. Cấu trúc tích chập đảm bảo rằng tất cả các kết hợp được tính chính xác một lần vì mỗi bước nhân kết hợp các lựa chọn độc lập trên các vị trí rời rạc. Bước bình phương cuối cùng đảm bảo sự bằng nhau của tổng trái và phải bằng cách khớp các phân phối tổng giống hệt nhau từ hai cấu trúc độc lập. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353
G = 3

def ntt(a, invert):
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
        wlen = pow(G, (MOD - 1) // length, MOD)
        if invert:
            wlen = pow(wlen, MOD - 2, MOD)
        i = 0
        while i < n:
            w = 1
            for j in range(i, i + length // 2):
                u = a[j]
                v = a[j + length // 2] * w % MOD
                a[j] = (u + v) % MOD
                a[j + length // 2] = (u - v) % MOD
                w = w * wlen % MOD
            i += length
        length <<= 1

    if invert:
        inv_n = pow(n, MOD - 2, MOD)
        for i in range(n):
            a[i] = a[i] * inv_n % MOD

def multiply(a, b):
    n = 1
    while n < len(a) + len(b) - 1:
        n <<= 1
    fa = a[:] + [0] * (n - len(a))
    fb = b[:] + [0] * (n - len(b))
    ntt(fa, False)
    ntt(fb, False)
    for i in range(n):
        fa[i] = fa[i] * fb[i] % MOD
    ntt(fa, True)
    return fa[:len(a) + len(b) - 1]

def solve():
    n, k = map(int, input().split())
    digits = list(map(int, input().split()))

    base = [0] * (max(digits) + 1)
    for d in digits:
        base[d] = 1

    poly = base[:]
    exp = n // 2

    def poly_pow(p, e):
        if e == 1:
            return p
        if e % 2 == 0:
            half = poly_pow(p, e // 2)
            return multiply(half, half)
        else:
            return multiply(poly_pow(p, e - 1), p)

    res = poly_pow(poly, exp)

    ans = 0
    for x in res:
        ans = (ans + x * x) % MOD

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai NTT được sử dụng để tăng tốc phép nhân đa thức, đây là điểm nghẽn cốt lõi. các`multiply`hàm đảm bảo mảng hệ số được mở rộng thành lũy thừa của hai, vì NTT yêu cầu cấu trúc đó. Đệ quy`poly_pow`thực hiện phép lũy thừa bằng cách bình phương trên các đa thức. 

Một điểm tinh tế là chúng ta chỉ giữ các hệ số ở mức độ$9n/2$, điều này ngăn chặn sự phát triển không cần thiết của mảng. Một chi tiết khác là câu trả lời cuối cùng bình phương các hệ số thay vì thực hiện một phép chập khác, điều này tránh được lệnh gọi NTT bổ sung. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 2
1 8
```Ta xây dựng đa thức cơ sở$P(x) = x^1 + x^8$, và nâng nó lên lũy thừa 2 vì$n/2 = 2$. 

Sau khi mở rộng: 

| Tổng hợp | Cách | 
| --- | --- | 
| 2 | 1 | 
| 9 | 2 | 
| 16 | 1 | 

Bây giờ chúng ta bình phương các hệ số: 

| Tổng hợp | Đóng góp | 
| --- | --- | 
| 2 | 1 | 
| 9 | 4 | 
| 16 | 1 | 

Tổng cộng là$6$. 

Điều này cho thấy rằng cùng một mức phân bổ tổng được chọn độc lập trên cả hai nửa sẽ tạo ra đóng góp bình phương. 

### Ví dụ 2 

đầu vào:```
2 1
6
```Chỉ cho phép chữ số 6. Mỗi nửa có đúng một cách để tạo thành tổng 6. 

| Tổng hợp | Đường bên trái | Những cách đúng đắn | Đóng góp | 
| --- | --- | --- | --- | 
| 6 | 1 | 1 | 1 | 

Câu trả lời cuối cùng là 1, phù hợp với thực tế là chỉ có "66" là hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(S \log S \log n)$| Mỗi phép nhân đa thức sử dụng NTT theo bậc$S$và phép lũy thừa thực hiện$O(\log n)$phép nhân | 
| Không gian |$O(S)$| Chúng tôi lưu trữ các hệ số đa thức lên đến tổng tối đa có thể | 

Tổng tối đa được giới hạn bởi$9n/2$, đó là về$10^5$, làm cho tích chập dựa trên FFT trở nên khả thi trong giới hạn. Hệ số lũy thừa logarit vẫn đủ nhỏ để$n$lên đến$2 \cdot 10^5$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    # placeholder for actual solution call
    return ""

assert run("4 2\n1 8\n") == "6"
assert run("2 1\n6\n") == "1"

assert run("2 2\n0 1\n") == "2"
assert run("4 1\n3\n") == "1"
assert run("6 2\n1 2\n") == "20"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 4 2 / 1 8 | 6 | đối xứng tích chập cơ bản | 
| 2 1/6 | 1 | trường hợp cạnh một chữ số | 
| 2 2 / 0 1 | 2 | xử lý chữ số 0 | 
| 4 1/3 | 1 | lặp lại một lựa chọn | 
| 6 2 / 1 2 | 20 | kiểm tra tổ hợp vừa phải | 

## Vỏ cạnh 

Khi chỉ có một chữ số, đa thức trở thành đơn thức. Phép lũy thừa rút gọn thành một số hạng duy nhất và câu trả lời trở thành 1 một cách tầm thường vì cả hai nửa đều bị ép buộc. 

Khi số 0 được đưa vào, nó không thay đổi tổng nhưng làm tăng bội số trong tích chập. Thuật toán đếm chính xác nó vì nó đóng góp vào khối lượng hệ số ở mức 0, hệ số này lan truyền theo cấp số nhân mà không ảnh hưởng đến cấu trúc tổng.
