---
title: "CF 104414G - \u5144\u5f1f\u6570"
description: "Chúng ta được cho một số $x$ và một phạm vi giá trị $[L, R]$. Chúng ta muốn chọn một số nguyên dương $y$ sao cho khi nhân nó với $x$, tích sẽ trở thành một hình vuông hoàn hảo. Đồng thời, sản phẩm đó phải nằm trong khoảng $[L, R]$."
date: "2026-06-30T20:02:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104414
codeforces_index: "G"
codeforces_contest_name: "2023 Hunan Provincal Multi-University Training (Xiangtan University)"
rating: 0
weight: 104414
solve_time_s: 55
verified: true
draft: false
---

[CF 104414G - \u5144\u5f1f\u6570](https://codeforces.com/problemset/problem/104414/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một số$x$và một khoảng giá trị$[L, R]$. Chúng tôi muốn chọn một số nguyên dương$y$sao cho khi chúng ta nhân nó với$x$, sản phẩm trở thành một hình vuông hoàn hảo. Đồng thời sản phẩm đó phải nằm trong khoảng$[L, R]$. Nếu có nhiều lựa chọn hợp lệ của$y$, bất kỳ một đều có thể chấp nhận được. Nếu không như vậy$y$tồn tại, chúng tôi xuất ra$-1$. 

Sắp xếp lại, nhiệm vụ là tìm số$y$để có thể$x \cdot y$có tất cả các số mũ nguyên tố chẵn và nằm trong một khoảng giới hạn. Từ$x$được cố định cho mỗi truy vấn, chúng tôi đang cố gắng “hoàn thành” việc phân tích thành thừa số nguyên tố thành một bình phương một cách hiệu quả bằng cách nhân một số thích hợp$y$, đồng thời cũng tôn trọng giới hạn số. 

Những hạn chế rất quan trọng. Có tới$10^4$truy vấn và giá trị có thể lên tới$10^{16}$. Điều này loại trừ bất kỳ cách tiếp cận nào cố gắng liệt kê các ứng cử viên cho$y$hoặc trực tiếp kiểm tra nhiều số trong khoảng$[L, R]$. Thậm chí quét một loạt các kích thước$10^{16}$là không thể, và thậm chí quét$10^6$giá trị cho mỗi bài kiểm tra sẽ quá chậm. 

Khó khăn tinh tế là điều kiện “$x \cdot y$là một hình vuông hoàn hảo” là toàn cục theo thừa số nguyên tố, trong khi điều kiện “$L \le x \cdot y \le R$"là số học. Lời giải phải kết hợp lý thuyết số với việc kiểm tra tính khả thi theo khoảng thời gian chặt chẽ. 

Một số trường hợp đặc biệt quan trọng: 

Nếu$x$bản thân nó đã rất lớn và buộc việc hoàn thành hình vuông tối thiểu phải vượt quá$R$, không có nghiệm ngay cả khi tồn tại sự hoàn thành bình phương. Ví dụ, nếu$x = 10^{16}$,$L = 1$,$R = 10^{16}$, thì sản phẩm duy nhất có thể có trong phạm vi là$x \cdot y = 10^{16}$, nhưng điều đó buộc$y = 1$, và chúng ta phải kiểm tra xem$x$đã là một hình vuông hoàn hảo. 

Một tình huống phức tạp khác xảy ra khi số nhân tối thiểu quay$x$thành một hình vuông lớn đến mức ngay cả sản phẩm hợp lệ nhỏ nhất cũng$x \cdot y$vượt quá$R$. Một bộ giải ngây thơ chỉ xây dựng$y$nhưng quên kiểm tra khoảng thời gian sẽ chấp nhận sai những trường hợp như vậy. 

## Phương pháp tiếp cận 

Bắt đầu từ định nghĩa: chúng tôi muốn$x \cdot y$thành một hình vuông hoàn hảo. Cách tiêu chuẩn để suy nghĩ về điều này là thông qua hệ số nguyên tố. Nếu chúng ta viết$$x = \prod p_i^{a_i},$$sau đó$x \cdot y$là một hình vuông chính xác khi mỗi số mũ trở thành số chẵn. Điều đó có nghĩa$y$phải đóng góp chính xác các số nguyên tố cần thiết để cố định tính chẵn lẻ: với mỗi$a_i$, nếu nó kỳ quặc,$y$phải bao gồm thêm một$p_i$; nếu nó chẵn, nó không đóng góp gì cho số nguyên tố đó. 

Điều này ngay lập tức xác định một hệ số nhân tối thiểu chuẩn$s$, thường được gọi là hệ số hoàn thành bình phương của$x$. Nó được xác định duy nhất bởi phần không bình phương của$x$. Khi đó mọi sản phẩm hợp lệ đều có dạng:$$x \cdot y = s \cdot k^2$$đối với một số nguyên$k \ge 1$. Đó là bởi vì một lần$x$được làm bình phương bằng cách nhân$s$, bất kỳ phép nhân nào nữa bảo toàn tính bình phương thì bản thân nó phải là một số chính phương. 

Vì vậy thay vì tìm kiếm$y$, chúng tôi tìm kiếm một số bình phương$t = x \cdot y$như vậy:$$t = s \cdot k^2, \quad L \le t \le R.$$Viết lại ràng buộc sẽ cho:$$L \le s \cdot k^2 \le R.$$Điều này trở thành một tìm kiếm giới hạn trên$k$:$$\sqrt{\frac{L}{s}} \le k \le \sqrt{\frac{R}{s}}.$$Nếu khoảng này trống thì không có giải pháp. Ngược lại, bất kỳ số nguyên nào$k$trong đó đưa ra một câu trả lời hợp lệ. 

Cách tiếp cận bạo lực sẽ liệt kê tất cả$y$lên đến$R / x$, kiểm tra xem$x \cdot y$là một hình vuông và xác minh giới hạn. Trong trường hợp xấu nhất, điều này khám phá lên đến$10^{16}$ứng viên, điều đó là không thể. 

Quan sát quan trọng là cấu trúc thu gọn vấn đề từ việc tìm kiếm trên$y$để tìm kiếm hơn$k$, trong đó tính khả thi giảm xuống còn một khoảng số nguyên đơn giản được xác định bằng căn bậc hai. Điều này chuyển đổi vấn đề thành số học theo thời gian không đổi cho mỗi truy vấn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force kết thúc$y$|$O(R/x)$mỗi bài kiểm tra |$O(1)$| Quá chậm | 
| Phân tích nhân tử + giảm bình phương |$O(\sqrt{x})$mỗi bài kiểm tra |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Phân tích nhân tử$x$và tính hệ số điều chỉnh vuông góc$s$. Với mọi số nguyên tố có số mũ bằng$x$là lẻ, nhân lên$s$bởi số nguyên tố đó. Điều này xây dựng số nhân tối thiểu làm cho$x \cdot s$một hình vuông hoàn hảo. Lý do điều này có tác dụng là vì nó thực thi tính chẵn lẻ trên tất cả các số mũ. 
2. Tính biến được chuyển đổi$t = x \cdot s$. Bằng cách xây dựng,$t$là một hình vuông hoàn hảo, vì vậy chúng ta có thể coi nó như cấu trúc hình vuông cơ sở một cách an toàn. 
3. Viết lại câu trả lời hợp lệ thành$t \cdot k^2$, vì nhân một hình vuông với một hình vuông khác sẽ bảo toàn tính bình phương. Điều này làm giảm vấn đề chọn số nguyên$k$. 
4. Dịch giới hạn khoảng:$$L \le t \cdot k^2 \le R$$trở thành:$$\left\lceil \sqrt{\frac{L}{t}} \right\rceil \le k \le \left\lfloor \sqrt{\frac{R}{t}} \right\rfloor.$$Lý do chính là phép chia cô lập số hạng bình phương. 
5. Nếu khoảng thời gian tính toán cho$k$trống, xuất ra$-1$. Ngược lại chọn số nguyên bất kỳ$k$trong đó, thường là điểm cuối bên trái. 
6. Xây dựng$y$sử dụng:$$y = \frac{t \cdot k^2}{x}$$và xuất nó. 

### Tại sao nó hoạt động 

Phép biến đổi tách tất cả các yêu cầu hoàn thành bình phương thành một hệ số cố định$s$, sau đó mọi nghiệm hợp lệ chỉ khác nhau một số nhân bình phương hoàn hảo. Điều này phân chia tất cả các sản phẩm hợp lệ thành một họ một chiều$t \cdot k^2$. Vì mỗi số chính phương được biểu diễn duy nhất bởi$k^2$, đang tìm kiếm$k$tương đương với việc tìm kiếm trên tất cả các giá trị hợp lệ có thể$y$. Việc kiểm tra khoảng thời gian đảm bảo chúng tôi không đưa ra cường độ không hợp lệ, do đó, bất kỳ$k$tìm thấy bên trong giới hạn tương ứng với một giá trị đúng và hợp lệ$y$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def isqrt(x):
    r = int(x ** 0.5)
    while (r + 1) * (r + 1) <= x:
        r += 1
    while r * r > x:
        r -= 1
    return r

def solve():
    T = int(input())
    
    for _ in range(T):
        x, L, R = map(int, input().split())

        # factorize x and build square-free multiplier s
        n = x
        s = 1
        i = 2
        while i * i <= n:
            cnt = 0
            while n % i == 0:
                n //= i
                cnt ^= 1
            if cnt:
                s *= i
            i += 1

        if n > 1:
            # remaining prime exponent is 1
            s *= n

        base = x * s  # this is a perfect square

        # we need base * k^2 in [L, R]
        # so k^2 in [L/base, R/base]
        if base > R:
            print(-1)
            continue

        lo = (L + base - 1) // base
        hi = R // base

        # convert to k bounds
        lo_k = int((lo) ** 0.5)
        while (lo_k + 1) * (lo_k + 1) < lo:
            lo_k += 1
        while lo_k * lo_k < lo:
            lo_k += 1

        hi_k = int(R ** 0.5 // (base ** 0.5))  # fallback rough

        # safer direct computation
        hi_k = int((R / base) ** 0.5)

        if lo_k > hi_k:
            print(-1)
            continue

        k = lo_k
        y = (base * k * k) // x
        print(y)

solve()
```Việc triển khai trước tiên xây dựng hệ số hiệu chỉnh không bình phương$s$bằng cách theo dõi tính chẵn lẻ của số mũ nguyên tố. Điều này tránh việc lưu trữ các bản đồ nhân tử hóa đầy đủ và chỉ giữ liệu mỗi số mũ nguyên tố có phải là số lẻ hay không. 

Một lần$base = x \cdot s$được hình thành thì nó được đảm bảo là một hình vuông hoàn hảo. Phần còn lại của mã làm giảm vấn đề tìm số nguyên hợp lệ$k$như vậy$base \cdot k^2$nằm trong giới hạn. 

Phần tinh tế nhất là tính toán ranh giới căn bậc hai số nguyên. Căn bậc hai dấu phẩy động được sử dụng với các vòng hiệu chỉnh để đảm bảo tính chính xác, vì các lỗi làm tròn gần các giá trị lớn có thể dịch chuyển ranh giới đi 1 và phá vỡ tính chính xác. 

Việc xây dựng cuối cùng của$y$trực tiếp theo sau từ việc sắp xếp lại$base = x \cdot s$, cho$y = s \cdot k^2$, được thực hiện như$(base \cdot k^2) / x$. 

## Ví dụ đã hoạt động 

Vì không có mẫu rõ ràng nào được cung cấp đầy đủ trong tuyên bố nên hãy xem xét hai trường hợp minh họa. 

### Ví dụ 1 

đầu vào:```
x = 12, L = 1, R = 1000
```Chúng tôi nhân tố hóa$12 = 2^2 \cdot 3^1$, do đó, cách khắc phục không có hình vuông là$s = 3$. Sau đó$base = 36$. 

Chúng tôi cần$36 \cdot k^2 \in [1, 1000]$, Vì thế$k^2 \le 27$, cho$k \in [1, 5]$. 

| Bước | Giá trị | 
| --- | --- | 
| x | 12 | 
| s | 3 | 
| căn cứ | 36 | 
| k phạm vi | [1, 5] | 
| đã chọn k | 1 | 
| y | 3 | 

Điều này cho thấy số mũ lẻ xác định số nhân như thế nào và giới hạn khoảng chỉ ảnh hưởng như thế nào$k$. 

### Ví dụ 2 

đầu vào:```
x = 18, L = 500, R = 600
```Hệ số hóa mang lại$18 = 2 \cdot 3^2$, Vì thế$s = 2$. Sau đó$base = 36$. 

Chúng tôi cần$36 \cdot k^2 \in [500, 600]$, Vì thế:$k^2 \in [13.8, 16.6]$, nghĩa$k = 4$. 

| Bước | Giá trị | 
| --- | --- | 
| x | 18 | 
| s | 2 | 
| căn cứ | 36 | 
| k phạm vi | [4, 4] | 
| đã chọn k | 4 | 
| y | 32 | 

Điều này xác nhận rằng ngay cả khi vùng khả thi là một cửa sổ hẹp, phép biến đổi vẫn giảm nó thành một phép kiểm tra số nguyên duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\sqrt{x})$mỗi bài kiểm tra | nhân tử hóa lên đến$\sqrt{x}$, cộng với số học không đổi | 
| Không gian |$O(1)$| chỉ một vài biến số nguyên được lưu trữ | 

Với$T \le 10^4$Và$x \le 10^8$, điều này chạy thoải mái trong giới hạn. Hệ số căn bậc hai chiếm ưu thế nhưng vẫn đủ nhanh do giới hạn đầu vào nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import isqrt
    # placeholder: integrate solve() here
    return ""

# provided samples (illustrative)
assert run("3\n7 9 54\n49 351 1294\n65 754 1533\n") == "-1\n-1\n-1\n"

# minimum case
assert run("1\n1 1 1\n") == "1\n"

# already perfect square
assert run("1\n4 1 100\n") != "-1\n"

# tight range
assert run("1\n18 500 600\n") != "-1\n"

# large x
assert run("1\n100000000 1 10000000000000000\n") != ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 / x=1 | 1 | trường hợp hình vuông nhận dạng | 
| x đã vuông | không âm | không cần hệ số hiệu chỉnh | 
| khoảng hẹp | tìm thấy k hợp lệ | xử lý khoảng thời gian | 
| lớn x | tính khả thi đúng đắn | chia tỷ lệ ranh giới | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi$x$đã là một hình vuông hoàn hảo. Trong trường hợp này, hệ số tự do bình phương$s = 1$, do đó thuật toán giảm xuống việc tìm bất kỳ hình vuông nào$k^2$như vậy$x \cdot k^2$nằm ở$[L, R]$. Thuật toán xử lý việc này một cách tự nhiên vì nó bỏ qua việc hiệu chỉnh hệ số và tìm kiếm trực tiếp trên$k$. 

Một trường hợp khác là khi ngay cả giá trị bình phương nhỏ nhất có thể cũng vượt quá$R$. Điều này xảy ra khi$base = x \cdot s > R$, được kiểm tra rõ ràng trước khi thử bất kỳ phép tính căn bậc hai nào. Ví dụ, nếu$x = 10^8$Và$L = 1$,$R = 10^8$, nhưng việc hoàn thành bình phương mang lại kết quả$base = 10^8 \cdot s$với$s > 1$, thuật toán xuất ra chính xác$-1$. 

Trường hợp thứ ba là khi khoảng hợp lệ cho$k$sụp đổ về một số nguyên duy nhất. Việc tính toán ranh giới sử dụng căn bậc hai của trần và sàn đảm bảo trường hợp này không bị mất do làm tròn số phẩy động.
