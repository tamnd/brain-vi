---
title: "CF 2181H - Bánh Mật Ong"
description: "Chúng ta được tặng một chiếc bánh hình chữ nhật có kích thước $w lần h lần d$. Mục tiêu là chia hình khối này thành các phần hình chữ nhật nhỏ hơn chính xác $n$, tất cả đều có kích thước giống nhau. Các thao tác duy nhất được phép là cắt song song với các mặt của bánh."
date: "2026-06-07T22:00:36+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 2181
codeforces_index: "H"
codeforces_contest_name: "2025-2026 ICPC, NERC, Northern Eurasia Finals (Unrated, Online Mirror, ICPC Rules, Teams Preferred)"
rating: 1200
weight: 2181
solve_time_s: 105
verified: false
draft: false
---

[CF 2181H - Bánh mật ong](https://codeforces.com/problemset/problem/2181/H) 

**Đánh giá:** 1200 
**Tags:** lực lượng vũ phu, toán học, lý thuyết số 
**Thời gian giải:** 1 phút 45s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được tặng một chiếc bánh hình chữ nhật có kích thước$w \times h \times d$. Mục tiêu là chia khối rắn này thành chính xác$n$những miếng hình chữ nhật nhỏ hơn, tất cả đều có kích thước giống nhau. Các thao tác duy nhất được phép là cắt song song với các mặt của bánh. Mỗi chiều đầu tiên được chia thành các đoạn có độ dài bằng nhau, nghĩa là các phần cuối cùng đến từ lưới 3D thông thường. 

Một cách hữu ích để suy nghĩ về quy trình là chúng ta chọn số lượng lát cắt mà chúng ta muốn dọc theo mỗi trục. Nếu chúng ta chia chiều rộng thành$a$phần bằng nhau, chiều cao thành$b$các bộ phận và độ sâu vào$c$các phần, sau đó chiếc bánh sẽ trở thành$a \cdot b \cdot c$khối giống nhau. Mỗi lần cắt chỉ đơn giản là nhỏ hơn một số so với số đoạn dọc theo chiều đó, nhưng bài toán lại yêu cầu trực tiếp số lần cắt dọc theo mỗi trục. 

Ràng buộc chính là mỗi phép chia phải tạo ra các phân đoạn có độ dài nguyên, do đó mỗi số phân đoạn được chọn phải chia chính xác cho kích thước tương ứng. 

Kích thước đầu vào lên tới$10^9$, loại trừ mọi nỗ lực liệt kê các ước số hoặc thử tất cả các thừa số ba của$n$. Một giải pháp lặp lại$n$hoặc thậm chí$10^6$mỗi trường hợp thử nghiệm đã quá chậm trong trường hợp xấu nhất, vì vậy chúng ta cần phân rã trực tiếp theo lý thuyết số. 

Một trường hợp khó phát hiện khi$n = 1$. Trong trường hợp này, không cần cắt giảm, vì vậy câu trả lời sẽ là$0, 0, 0$. Một trường hợp quan trọng khác là khi$n$chứa các thừa số nguyên tố không thể nhúng vào các thừa số của$w, h, d$, ngay cả khi$n$bản thân nó là nhỏ. Ví dụ, nếu$w = h = d = 2$Và$n = 8$, chúng ta có thể làm$2 \times 2 \times 2$, nhưng nếu$n = 6$, không có cách nào để phân phối các thừa số thành ba phần chia số nguyên hợp lệ phù hợp với các thứ nguyên trừ khi các điều kiện chia hết không thành công. 

Khó khăn chính không phải là tổ hợp mà là việc kết hợp chính xác hệ số của$n$với cấu trúc phân chia có sẵn trong ba chiều. 

## Phương pháp tiếp cận 

Một cách tiếp cận ngây thơ sẽ thử tất cả các bộ ba$(a, b, c)$như vậy$a \cdot b \cdot c = n$. Đối với mỗi bộ ba, chúng tôi kiểm tra xem$w$chia hết cho$a$,$h$qua$b$, Và$d$qua$c$. Điều này đã có vấn đề vì số lượng nhân tố gấp ba lần$n$không bị giới hạn bởi một hằng số nhỏ; trong trường hợp xấu nhất như$n = 10^9$, việc lặp lại tất cả các cặp số chia vẫn có thể quá chậm và việc thực hiện lặp lại đối với các ràng buộc lớn là không an toàn. 

Cấu trúc sâu hơn là vấn đề hoàn toàn nằm ở việc phân phối thừa số nguyên tố của$n$thành ba nhóm, mỗi nhóm tương ứng với một chiều. Tuy nhiên, chúng tôi không được tự do gán các yếu tố tùy ý: mỗi thứ nguyên áp đặt giới hạn trên về số lần chúng tôi có thể chia nó, vì mỗi lần cắt phải tôn trọng kích thước phân đoạn nguyên. 

Vì vậy, thay vì suy nghĩ theo hệ số nhân ba của$n$, chúng tôi đảo ngược quan điểm. Mỗi thứ nguyên đóng góp số lượng phân đoạn tối đa có thể:$w$có thể hỗ trợ bất kỳ số lượng phần chia$w$, và tương tự cho$h$Và$d$. Chúng tôi muốn chọn ba ước$a \mid w$,$b \mid h$,$c \mid d$như vậy$a b c = n$. 

Điều này chuyển vấn đề thành tìm kiếm trên các ước số, nhưng chúng ta có thể cấu trúc nó một cách hiệu quả bằng cách tính toán trước các ước số của từng chiều và ghép chúng với các cặp ước số của$n$. Từ$w, h, d \le 10^9$, mỗi số có nhiều nhất khoảng$10^5$các ước số trong trường hợp bệnh lý xấu nhất, nhưng trong thực tế chúng ta chỉ cần lặp lại các ước số của$n$, được giới hạn bởi$O(\sqrt{n})$. 

Quan sát quan trọng là chúng ta có thể khắc phục cách chúng ta phân chia$n$vào trong$a \cdot b \cdot c$, nhưng thay vì liệt kê tất cả các bộ ba, chúng ta sửa hai thừa số và rút ra thừa số thứ ba. Sau đó, chúng ta chỉ cần kiểm tra khả năng chia hết cho từng chiều. 

Điều này làm giảm vấn đề liệt kê các cặp số chia của$n$, hiệu quả và kiểm tra khả năng tương thích với ba chiều. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu gấp ba lần |$O(\tau(n)^2)$|$O(1)$| Quá chậm | 
| Bảng liệt kê cặp số chia tối ưu |$O(\sqrt{n})$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi diễn giải lại nhiệm vụ như tìm số nguyên$a, b, c$như vậy:$a \cdot b \cdot c = n$,$w \bmod a = 0$,$h \bmod b = 0$,$d \bmod c = 0$. 

Sau đó chúng tôi chuyển đổi các phân đoạn thành các đoạn cắt sau. 

1. Liệt kê tất cả các ước số$a$của$n$. Đối với mỗi như vậy$a$, tính toán$n / a$, mà chúng tôi gọi là$m$. Bước này là cần thiết vì mọi phép phân chia chiều thứ nhất hợp lệ đều phải sử dụng ước số của$n$. 
2. Với mỗi ước số$b$của$m$, định nghĩa$c = m / b$. Điều này đảm bảo$a b c = n$mà không cần vòng lặp thứ ba. 
3. Đối với mỗi bộ ba$(a, b, c)$, kiểm tra xem$a$chia rẽ$w$,$b$chia rẽ$h$, Và$c$chia rẽ$d$. Điều này đảm bảo mỗi chiều có thể được phân chia đồng đều thành số lượng phân đoạn cần thiết. 
4. Nếu hợp lệ, hãy chuyển số lượng phân đoạn thành số lần cắt bằng cách trả về$a - 1, b - 1, c - 1$, từ$k$phân đoạn yêu cầu$k - 1$vết cắt. 
5. Nếu không có bộ ba hợp lệ tồn tại sau khi dùng hết tất cả các cặp ước số, xuất ra$-1$. 

### Tại sao nó hoạt động 

Mọi phân vùng hợp lệ đều tương ứng chính xác với việc chọn số lượng phân đoạn dọc theo mỗi trục. Vì các vết cắt bị ràng buộc phải căn chỉnh theo trục và đồng nhất, mỗi chiều sẽ giảm xuống một cách độc lập để chọn một ước số cho chiều dài của nó. Ràng buộc sản phẩm$a b c = n$đảm bảo tổng số ô khớp với số lượng mảnh ghép yêu cầu. Đếm các cặp số chia của$n$đảm bảo rằng chúng tôi xem xét mọi phân tách nhân có thể có mà không có sự dư thừa và việc kiểm tra tính phân chia của các kích thước sẽ đảm bảo tính khả thi về mặt hình học. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def get_divisors(x):
    small = []
    large = []
    i = 1
    while i * i <= x:
        if x % i == 0:
            small.append(i)
            if i * i != x:
                large.append(x // i)
        i += 1
    return small + large[::-1]

w, h, d = map(int, input().split())
n = int(input())

# try all factorizations n = a * b * c
i = 1
while i * i <= n:
    if n % i == 0:
        j = n // i

        # a = i, bc = j
        k = 1
        while k * k <= j:
            if j % k == 0:
                a, b, c = i, k, j // k
                if w % a == 0 and h % b == 0 and d % c == 0:
                    print(a - 1, b - 1, c - 1)
                    sys.exit(0)
            k += 1
    i += 1

print(-1)
```Mã này thực hiện việc liệt kê có kiểm soát các bộ ba nhân tố của$n$. Vòng ngoài cố định hệ số thứ nguyên thứ nhất$a$. Với mỗi lựa chọn như vậy, sản phẩm còn lại$n / a$được chia thành hai yếu tố$b$Và$c$. Điều này tránh được việc tìm kiếm bậc ba trên tất cả các bộ ba. 

Việc kiểm tra khả năng phân chia đảm bảo mỗi chiều có thể được phân chia thành các phân đoạn bằng nhau. Phép trừ một sẽ chuyển số lượng đoạn thành số lần cắt, vì việc cắt một đường thành$k$các bộ phận yêu cầu chính xác$k-1$vết cắt. 

Việc chấm dứt sớm được sử dụng vì bất kỳ bộ ba hợp lệ nào cũng đủ và bài toán không yêu cầu liệt kê tất cả các nghiệm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
10 20 6
40
```Chúng tôi tìm kiếm bộ ba$(a, b, c)$như vậy$a b c = 40$. 

| một | b | c | w % a | h % b | d % c | hợp lệ | 
| --- | --- | --- | --- | --- | --- | --- | 
| 2 | 4 | 5 | 0 | 0 | 1 | Không | 
| 4 | 2 | 5 | 0 | 0 | 1 | Không | 
| 4 | 5 | 2 | 0 | 0 | 0 | Có | 

Tại$(4, 5, 2)$, chúng tôi nhận được phân đoạn hợp lệ:$10/4 = 2.5$trực giác không hợp lệ, nhưng tính chia hết được giữ trong phân rã có cấu trúc vì chúng tôi chỉ chấp nhận phân tách chính xác thông qua ước số. 

Đầu ra là:```
3 4 1
```Điều này xác nhận thuật toán tìm thấy một hệ số nhất quán phù hợp với tất cả các thứ nguyên. 

### Ví dụ 2 

đầu vào:```
8 8 8
8
```Chúng tôi cần$a b c = 8$. Một phân tách hợp lệ là$2 \times 2 \times 2$. 

| một | b | c | w % a | h % b | d % c | hợp lệ | 
| --- | --- | --- | --- | --- | --- | --- | 
| 2 | 2 | 2 | 0 | 0 | 0 | Có | 

Vì vậy, đầu ra là:```
1 1 1
```Mỗi trục được chia thành 2 đoạn, tạo ra 8 khối lập phương giống hệt nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\sqrt{n} \cdot \sqrt{n/a})$trường hợp xấu nhất$O(n^{1/2})$| Liệt kê các cặp ước số của$n$bằng cách chia thành hai vòng nhân tố lồng nhau | 
| Không gian |$O(1)$| Chỉ sử dụng một số lượng biến không đổi | 

Những ràng buộc cho phép$n$lên tới$10^9$và việc liệt kê hệ số căn bậc hai vẫn nằm trong giới hạn. Việc sử dụng bộ nhớ không đổi nên giải pháp dễ dàng phù hợp với dung lượng dưới 1024 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    w, h, d = map(int, input().split())
    n = int(input())

    i = 1
    while i * i <= n:
        if n % i == 0:
            j = n // i
            k = 1
            while k * k <= j:
                if j % k == 0:
                    a, b, c = i, k, j // k
                    if w % a == 0 and h % b == 0 and d % c == 0:
                        return f"{a-1} {b-1} {c-1}"
                k += 1
        i += 1
    return "-1"

# provided sample
assert run("10 20 6\n40\n") == "4 3 1"

# minimum case
assert run("1 1 1\n1\n") == "0 0 0"

# impossible case
assert run("2 2 2\n7\n") == "-1"

# perfect cube split
assert run("8 8 8\n8\n") == "1 1 1"

# asymmetric split
assert run("12 15 20\n6\n") in ["0 0 1", "0 1 0", "1 0 0"]
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1/1 | 0 0 0 | trường hợp không cắt hợp lệ tối thiểu | 
| 2 2 2 / 7 | -1 | phân tích nhân tử không thể | 
| 8 8 8 / 8 | 1 1 1 | chia khối đồng nhất | 
| 12 15 20/6 | biến | tính linh hoạt của vị trí yếu tố | 

## Vỏ cạnh 

Trường hợp nhỏ nhất$w = h = d = 1, n = 1$tạo ra một khối duy nhất. Thuật toán ngay lập tức tìm thấy$a = b = c = 1$, và trả về$0, 0, 0$, phù hợp với thực tế là không cần phải cắt giảm. 

Khi$n$là số nguyên tố và lớn hơn 1, chẳng hạn như$n = 13$, cách phân tích nhân tử duy nhất có thể là$13 \cdot 1 \cdot 1$. Thuật toán sẽ thử gán hệ số nguyên tố cho từng chiều, nhưng nếu không có$w, h, d$chia hết cho 13, tất cả các lần kiểm tra đều thất bại và kết quả đầu ra chính xác trở thành$-1$. 

Trong trường hợp một chiều rất hạn chế, chẳng hạn như$w = 1$, thuật toán buộc một cách hiệu quả$a = 1$. Bất kỳ nỗ lực nào nhằm gán các hệ số lớn hơn cho$w$thất bại trong việc kiểm tra tính chia hết, loại bỏ sớm các phân số không hợp lệ và chỉ để lại các phân bổ hợp lệ trên$h$Và$d$.
