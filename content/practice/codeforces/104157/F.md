---
title: "CF 104157F - Đơn đặt hàng nhà vệ sinh"
description: "Chúng ta được cấp hai số nguyên lớn cho mỗi trường hợp thử nghiệm, biểu thị số lượng có sẵn của hai phần bổ sung. Từ những phép tính này, Thomas tạo ra một số cặp hoàn chỉnh bằng ước số chung lớn nhất của hai giá trị."
date: "2026-07-02T01:15:59+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104157
codeforces_index: "F"
codeforces_contest_name: "UTPC Contest 01-27-23 Div. 2 (Beginner)"
rating: 0
weight: 104157
solve_time_s: 79
verified: false
draft: false
---

[CF 104157F - Đơn đặt hàng nhà vệ sinh](https://codeforces.com/problemset/problem/104157/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 19s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp hai số nguyên lớn cho mỗi trường hợp thử nghiệm, biểu thị số lượng có sẵn của hai phần bổ sung. Từ những phép tính này, Thomas tạo ra một số cặp hoàn chỉnh bằng ước số chung lớn nhất của hai giá trị. Giá trị gcd đó, hãy gọi nó$g$, là số lượng duy nhất chúng ta thực sự quan tâm, bởi vì nó thể hiện số lượng vật phẩm đầy đủ có thể được hình thành một cách nhất quán từ cả hai nguồn cung cấp. 

Đối với mỗi trường hợp thử nghiệm, một lần$g$được xác định, nhiệm vụ không phải là xuất ra$g$chính nó mà biểu diễn nó dưới dạng thừa số nguyên tố. Chúng ta cần tất cả các số nguyên tố phân biệt chia$g$, được sắp xếp tăng dần và với mỗi số nguyên tố như vậy, chúng ta phải xuất ra số lần nó xuất hiện trong phép phân tích nhân tử. 

Các ràng buộc nhỏ về số lượng ca kiểm thử, nhưng bản thân các giá trị lại lớn đến$10^{12}$. Điều đó ngay lập tức loại trừ bất kỳ cách tiếp cận nào liên tục liệt kê các ứng cử viên lên đến$g$hoặc thực hiện phép chia thử tới$g$. Ngay cả một yếu tố ngây thơ lên ​​đến$10^{12}$mỗi trường hợp thử nghiệm sẽ quá chậm nếu lặp lại trường hợp xấu nhất 100 lần. 

Do đó, cấu trúc tính toán chính rất rõ ràng: chúng ta cần tính gcd một cách hiệu quả và sau đó nhân một số lên tới$10^{12}$một cách hiệu quả. 

Một số trường hợp đặc biệt quan trọng. Nếu như$b$Và$l$là nguyên tố cùng nhau thì$g = 1$và chúng ta chỉ phải in dấu kết thúc cho trường hợp thử nghiệm đó không có số nguyên tố trước nó. Nếu như$g$là số nguyên tố, chúng ta phải xuất ra chính xác một dòng chứa số nguyên tố đó có bội số là 1. Một lỗi phổ biến là quên rằng bội số đề cập đến số mũ trong hệ số hóa của$g$, không phải của đầu vào ban đầu. 

## Phương pháp tiếp cận 

Cách tiếp cận ngây thơ tính toán$g = \gcd(b, l)$, sau đó phân tích nó bằng cách kiểm tra tất cả các số nguyên từ 2 đến$\sqrt{g}$và chia nhiều lần. Điều này đúng vì mỗi số tổng hợp có nhiều nhất một thừa số nguyên tố căn bậc hai của nó. Tuy nhiên, trong trường hợp xấu nhất$g \approx 10^{12}$, Vì thế$\sqrt{g} \approx 10^6$. Thực hiện tới một triệu phép chia thử nghiệm cho mỗi trường hợp thử nghiệm và lặp lại điều này cho tối đa 100 trường hợp sẽ mang lại kết quả gần đúng.$10^8$các phép toán và mỗi bước chia không hề đơn giản trong Python. Đây là điểm gần ranh giới và có nguy cơ TLE. 

Cải tiến quan trọng đến từ việc nhận thấy rằng việc phân tích nhân tử chỉ cần các số nguyên tố chứ không phải tất cả các số nguyên. Chúng ta có thể tính toán trước tất cả các số nguyên tố lên đến$10^6$dùng rây rồi tính từng yếu tố$g$bằng cách chỉ chia cho các số nguyên tố này. Vì bất kỳ số tổng hợp nào lên đến$10^{12}$có ít nhất một thừa số nguyên tố ≤$10^6$, thế này là đủ rồi. Sau khi loại bỏ các số nguyên tố nhỏ, nếu giá trị còn lại lớn hơn 1 thì bản thân nó là số nguyên tố. 

Điều này làm giảm mỗi trường hợp thử nghiệm từ$O(\sqrt{g})$kiểm tra đại khái$O(\pi(\sqrt{g}))$, tức là khoảng 78.000 số nguyên tố, nhưng trên thực tế ít hơn nhiều do rút gọn sớm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Phòng xét xử Brute Force |$O(T \sqrt{g})$|$O(1)$| Quá chậm | 
| Sàng + Hệ số nguyên tố |$O(\sqrt{N} + T \cdot \pi(\sqrt{g}))$|$O(\sqrt{N})$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Tính trước các số nguyên tố lên tới$10^6$Chúng tôi xây dựng một sàng Eratosthenes. Điều này cung cấp cho chúng ta tất cả các số nguyên tố cần thiết để phân tích bất kỳ số nào lên đến$10^{12}$, vì mọi hợp số trong phạm vi đó phải có nhiều nhất là thừa số nguyên tố$10^6$. 

### 2. Xử lý từng test case 

Cho mỗi cặp$(b, l)$, tính toán$g = \gcd(b, l)$. Bước này nhanh,$O(\log \min(b,l))$, và không chi phối gì cả. 

### 3. Yếu tố$g$sử dụng số nguyên tố được tính toán trước 

Chúng ta lặp qua các số nguyên tố theo thứ tự tăng dần. Đối với mỗi số nguyên tố$p$, chúng tôi chia$g$miễn là$p \mid g$, đếm xem điều này xảy ra bao nhiêu lần. 

Mỗi bộ phận thành công đều thu nhỏ lại$g$, do đó việc kiểm tra sau này trở nên rẻ hơn. Đây là lý do tại sao chúng tôi dừng lại sớm khi$p^2 > g$. 

### 4. Xử lý giá trị còn sót lại 

Nếu sau khi xử lý tất cả các số nguyên tố lên tới$\sqrt{g}$, phần còn lại$g > 1$, thì nó là số nguyên tố và đóng góp một thừa số cuối cùng với số mũ 1. 

### 5. Định dạng đầu ra 

Chúng ta in từng thừa số nguyên tố và số mũ của nó theo thứ tự tăng dần. Sau khi hoàn thành một test, chúng ta in ra một dòng chứa số 0. 

### Tại sao nó hoạt động 

Tính đúng đắn dựa trên tính duy nhất của việc phân tích thành thừa số nguyên tố và thực tế là mọi hợp số đều có ước số nguyên tố không vượt quá căn bậc hai của nó. Sàng đảm bảo chúng tôi kiểm tra tất cả các số nguyên tố có thể có theo thứ tự, vì vậy mọi thừa số đều được trích xuất hoàn toàn chính xác một lần. Vì mỗi phép chia loại bỏ tất cả sự xuất hiện của một số nguyên tố trước khi tiếp tục, bội số được tính chính xác và thứ tự được giữ nguyên bằng cách lặp lại các số nguyên tố theo thứ tự tăng dần. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

MAX = 10**6

is_prime = [True] * (MAX + 1)
is_prime[0] = is_prime[1] = False
primes = []

for i in range(2, MAX + 1):
    if is_prime[i]:
        primes.append(i)
        for j in range(i * i, MAX + 1, i):
            is_prime[j] = False

def factorize(n):
    res = []
    for p in primes:
        if p * p > n:
            break
        if n % p == 0:
            cnt = 0
            while n % p == 0:
                n //= p
                cnt += 1
            res.append((p, cnt))
    if n > 1:
        res.append((n, 1))
    return res

t = int(input())
out = []

for _ in range(t):
    b, l = map(int, input().split())
    g = math.gcd(b, l)

    if g == 1:
        out.append("0")
        continue

    factors = factorize(g)
    for p, c in factors:
        out.append(f"{p} {c}")
    out.append("0")

sys.stdout.write("\n".join(out))
```Sàng được tạo một lần khi bắt đầu và được sử dụng lại trong tất cả các trường hợp thử nghiệm. Quá trình phân tích nhân tử cẩn thận dừng lại một lần$p^2 > n$, điều này ngăn cản việc lặp lại không cần thiết khi số còn lại đã là số nguyên tố. 

Tính toán gcd đảm bảo chúng tôi chỉ tính một số rút gọn duy nhất cho mỗi trường hợp thử nghiệm chứ không phải hai đầu vào lớn. 

Đầu ra được tích lũy trong một danh sách để tránh lặp lại chi phí I/O bên trong các vòng lặp, điều này rất quan trọng trong Python trong giới hạn thời gian chặt chẽ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
360 240
```Đầu tiên chúng tôi tính toán$g = \gcd(360, 240) = 120$. 

| Bước | Xuất sắc$p$| Hiện hành$g$| Hành động | Đầu ra cho đến nay | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 120 → 15 | chia 2 ba lần | (2, 3) | 
| 2 | 3 | 15 → 5 | chia một lần | (3, 1) | 
| 3 | 5 | 5 → 1 | chia một lần | (5, 1) | 

Đầu ra cuối cùng:```
2 3
3 1
5 1
0
```Điều này khẳng định việc trích xuất chính xác tất cả các bội số nguyên tố theo thứ tự tăng dần. 

### Mẫu 2 

đầu vào:```
83 24
15 25
```Cặp đầu tiên:$\gcd(83, 24) = 1$| Bước | g | Hành động | Đầu ra | 
| --- | --- | --- | --- | 
| 1 | 1 | dừng ngay lập tức | 0 | 

Cặp thứ hai:$\gcd(15, 25) = 5$| Bước | Xuất sắc$p$| Hiện hành$g$| Hành động | Đầu ra | 
| --- | --- | --- | --- | --- | 
| 1 | 5 | 5 → 1 | chia một lần | (5, 1) | 

Đầu ra cuối cùng:```
0
5 1
0
```Trường hợp đầu tiên xác minh việc xử lý đúng$g = 1$, trong khi phần thứ hai xác nhận việc xử lý chính xác gcd nguyên tố. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\sqrt{N} + T \cdot \pi(\sqrt{g}))$| sàng lên đến$10^6$, rồi thử chia cho số nguyên tố | 
| Không gian |$O(\sqrt{N})$| sàng và lưu trữ danh sách nguyên tố | 

Chi phí sàng được thanh toán một lần và được sử dụng lại cho tất cả các trường hợp thử nghiệm. Mỗi hệ số hoạt động trên một số lên đến$10^{12}$, nhưng bị giảm nhanh chóng thông qua phép chia. Với tối đa 100 trường hợp thử nghiệm, điều này hoàn toàn phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io, math

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import math

    MAX = 10**6
    is_prime = [True] * (MAX + 1)
    is_prime[0] = is_prime[1] = False
    primes = []
    for i in range(2, MAX + 1):
        if is_prime[i]:
            primes.append(i)
            for j in range(i * i, MAX + 1, i):
                is_prime[j] = False

    def factorize(n):
        res = []
        for p in primes:
            if p * p > n:
                break
            if n % p == 0:
                cnt = 0
                while n % p == 0:
                    n //= p
                    cnt += 1
                res.append((p, cnt))
        if n > 1:
            res.append((n, 1))
        return res

    t = int(input())
    out = []
    for _ in range(t):
        b, l = map(int, input().split())
        g = math.gcd(b, l)
        if g == 1:
            out.append("0")
        else:
            for p, c in factorize(g):
                out.append(f"{p} {c}")
            out.append("0")
    return "\n".join(out) + "\n"

# provided samples
assert run("1\n360 240\n") == "2 3\n3 1\n5 1\n0\n", "sample 1"
assert run("2\n83 24\n15 25\n") == "0\n5 1\n0\n", "sample 2"

# custom cases
assert run("1\n2 2\n") == "2 1\n0\n", "minimum prime power"
assert run("1\n49 49\n") == "7 2\n0\n", "perfect square gcd"
assert run("1\n1000000000000 999999999999\n")[-2:] == "0\n", "large coprime-ish"
assert run("3\n6 9\n10 25\n17 19\n") == "3 1\n0\n5 2\n0\n0\n", "mixed cases"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 2 | 2 1 0 | gcd nhỏ nhất không tầm thường | 
| 49 49 | 7 2 0 | xử lý số mũ nguyên tố lặp đi lặp lại | 
| cặp lớn | kết thúc bằng 0 | ổn định số lượng lớn | 
| trường hợp hỗn hợp | nhiều mẫu | độ chính xác hàng loạt | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi gcd bằng 1. Ví dụ: đầu vào`83 24`sản xuất$g = 1$. Vòng lặp nhân tố được bỏ qua hoàn toàn và đầu ra được yêu cầu duy nhất là một`0`. Thuật toán xử lý việc này một cách tự nhiên vì hàm phân tích nhân tử trả về một danh sách trống và phương thức gọi trực tiếp nối thêm dấu kết thúc. 

Một trường hợp cạnh khác là khi gcd là lũy thừa nguyên tố, chẳng hạn như`49 49`, Ở đâu$g = 49 = 7^2$. Vòng lặp phát hiện số chia hết cho 7, đếm hai số chia và nối thêm`(7, 2)`. Vì giá trị giảm trở thành 1 nên không có phần dư nào được thêm vào, ngăn chặn đầu ra trùng lặp. 

Trường hợp khó phát hiện cuối cùng là khi bản thân gcd là số nguyên tố lớn lớn hơn$10^6$, Ví dụ`1000000000039 1000000000039`. Trong trường hợp đó không có số nguyên tố sàng nào chia nó, do đó vòng lặp kết thúc và phần còn lại`n > 1`được thêm vào dưới dạng một thừa số nguyên tố duy nhất với số mũ 1. Điều này đảm bảo tính chính xác ngay cả ngoài phạm vi sàng.
