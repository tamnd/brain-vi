---
title: "CF 102220I - Khảo sát nhiệt độ"
description: "Chúng ta được cho dãy nhiệt độ đã được sắp xếp của thành phố A, a1, a2, ..., an. Chúng ta cần đếm xem có bao nhiêu dãy khác nhau b1, b2, ..., bn có thể tạo ra các quan sát, trong đó nhiệt độ của B cũng không giảm và bi <= ai cho mỗi ngày."
date: "2026-08-17T22:39:32+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102220
codeforces_index: "I"
codeforces_contest_name: "The 13th Chinese Northeast Collegiate Programming Contest"
rating: 0
weight: 102220
solve_time_s: 189
verified: true
draft: false
---

[CF 102220I - Khảo sát nhiệt độ](https://codeforces.com/problemset/problem/102220/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 9 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp trình tự nhiệt độ đã được sắp xếp của thành phố A,`a1, a2, ..., an`. Chúng ta cần đếm xem có bao nhiêu trình tự khác nhau`b1, b2, ..., bn`có thể đã tạo ra các quan sát, trong đó nhiệt độ của B cũng không giảm và`bi <= ai`cho mỗi ngày. Mỗi nhiệt độ là một số nguyên từ`1`ĐẾN`n`, và câu trả lời là bắt buộc theo modulo`998244353`. Các giới hạn chính thức là`n <= 2 * 10^5`, với tổng số`n`trên tất cả các trường hợp thử nghiệm nhiều nhất`5 * 10^5`, giới hạn thời gian 8 giây và bộ nhớ 512 MB. 

Tính đơn điệu là đặc tính cấu trúc quan trọng. hợp lệ`b`không phải là sự lựa chọn tùy ý một giá trị cho mỗi vị trí. Một lần`b_i`được chọn thì tất cả các giá trị sau đó ít nhất phải bằng giá trị đó, trong khi mỗi vị trí cũng có giới hạn trên khác nhau. Việc liệt kê trực tiếp đã là vô vọng: ngay cả khi không có giới hạn trên, số lượng chuỗi có độ dài không giảm`n`qua`n`giá trị là`C(2n-1,n)`, vì vậy việc kiểm tra mọi ứng viên sẽ yêu cầu khoảng`n * C(2n-1,n)`hoạt động trong trường hợp xấu nhất. Một chương trình năng động đơn giản với`dp[i][j]`sẽ giảm điều này xuống`O(n^2)`, Nhưng`n = 2 * 10^5`có nghĩa là về`4 * 10^10`vượt xa giới hạn thời gian cho phép. Tổng kích thước đầu vào của`5 * 10^5`cũng loại trừ các thuật toán có chi phí bậc hai chỉ trải đều trên các trường hợp thử nghiệm. 

Có một số trường hợp ranh giới bộc lộ những cách giải thích không chính xác phổ biến. Vì`n = 1`Và`a = [1]`, trình tự duy nhất có thể là`b = [1]`, vậy câu trả lời là`1`. Một phương pháp vô tình cho phép số 0 hoặc xử lý vị trí đầu tiên theo cách khác có thể tạo ra số đếm không chính xác. 

Vì`a = [1, 2, 3, 4]`, câu trả lời là`14`. Một cách giải thích lưới hấp dẫn là chỉ đếm các đường dẫn đến`(n, a_n)`, mang lại`5`, nhưng điều đó làm mất đi quyền tự do lựa chọn`b_n`. Cấu trúc đúng sẽ thêm một hàng và cột bổ sung sao cho bước di chuyển xuống cuối cùng thể hiện`b_n`; số kết quả là số Catalan`C_4 = 14`. 

Vì`a = [4, 4, 4, 4]`, mọi đều hợp lệ`b`chỉ đơn giản là một chuỗi không giảm gồm bốn giá trị từ`{1,2,3,4}`. Số của nó là`C(7,4) = 35`. Coi bốn quan điểm này là độc lập sẽ cho`4^4 = 256`, trong khi nhân các lựa chọn địa phương như`4 * 3 * 2 * 1`cũng đưa ra câu trả lời sai vì sự lựa chọn tại một vị trí sẽ thay đổi giới hạn dưới ở mọi vị trí sau đó. 

Các giá trị lặp lại cũng quan trọng. Vì`a = [2,2,3]`, các cặp có thể`(b1,b2)`là`(1,1)`,`(1,2)`, Và`(2,2)`. Họ cho phép tương ứng`3`,`2`, Và`2`sự lựa chọn cho`b3`, cho`7`. Việc triển khai phân chia và chinh phục nhằm phân chia các giá trị bằng nhau mà không xử lý toàn bộ vùng cao nguyên cùng nhau có thể đếm hai lần hoặc bỏ lỡ các đường dẫn dọc theo ranh giới phẳng đó. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất liệt kê mọi chuỗi không giảm`b`, kiểm tra`bi <= ai`, và đếm những cái hợp lệ. Điều này đúng vì nó thực sự kiểm tra toàn bộ tập hợp các ứng cử viên, nhưng ngay cả trước khi kiểm tra các giới hạn trên vẫn có`C(2n-1,n)`dãy không giảm. Vì`n = 2 * 10^5`, con số đó là số mũ trong`n`, vì vậy phương pháp này không thể sử dụng được. 

Sự cải tiến tự nhiên là lập trình động. Cho phép`dp[i][j]`là số tiền tố hợp lệ kết thúc bằng`b_i = j`. Sau đó`dp[i][j] = dp[i-1][1] + dp[i-1][2] + ... + dp[i-1][j]`bất cứ khi nào`j <= ai`. Tổng tiền tố làm cho mỗi lần chuyển đổi có thời gian không đổi, nhưng vẫn có`O(n^2)`tiểu bang. Lực lượng vũ phu hoạt động vì các ràng buộc mang tính cục bộ, nhưng không thành công khi số lượng giá trị nhiệt độ có thể trở nên lớn. 

Quan sát mở ra giải pháp nhanh hơn là DP này chính xác là một vấn đề về đường mạng. Vẽ hàng`i`với`ai`các ô, căn trái. Một con đường chỉ di chuyển sang phải hoặc đi xuống. Bất cứ khi nào đường dẫn di chuyển từ hàng`i`chèo thuyền`i+1`, ghi lại cột nơi diễn ra sự di chuyển đi xuống đó. Các cột được ghi lại đó tạo thành một chuỗi không giảm và thực tế là hàng đó`i`chỉ chứa`ai`tế bào cho`bi <= ai`. 

Để đại diện cho tất cả`n`giá trị của`b`, nối thêm một giá trị`a_{n+1}=n+1`. Việc di chuyển xuống thêm sau đó thể hiện giá trị cuối cùng ban đầu`b_n`, và đường dẫn kết thúc ở góc dưới bên phải. Chúng ta có thể đảo ngược các hàng, biến ranh giới không giảm thành cầu thang không tăng. Vấn đề nảy sinh là đếm các đường đi đơn điệu bên trong vùng hình Ferrers. 

Đối với phần hình chữ nhật của vùng này, DP thông thường có phép truy hồi`F(i,j) = F(i-1,j) + F(i,j-1)`. 

Nếu chúng ta biết các giá trị ở ranh giới trên và trái của hình chữ nhật thì mọi giá trị ở ranh giới dưới và phải của nó có thể được biểu diễn dưới dạng tích chập nhị thức. Đối với hình chữ nhật có chiều cao`h`và chiều rộng`w`, phần đóng góp từ giá trị biên trên cùng`x_j`đến vị trí dưới cùng`i`là`x_j * C(i-j+h-1, h-1)`, 

trong khi phần đóng góp từ giá trị biên trái`y_j`đến vị trí dưới cùng`i`là`y_j * C(i+h-1-j, i)`. 

Các công thức tương ứng cho ranh giới bên phải là đối xứng. 

Các hệ số nhị thức chứa các giai thừa, do đó mỗi tích chập có thể được chuyển đổi thành một phép nhân đa thức thông thường. Ví dụ,`C(i-j+h-1, h-1) = (i-j+h-1)! / ((h-1)! (i-j)!)`. 

Sau khi đảo ngược một thừa số và nhân với chuỗi giai thừa hoặc giai thừa nghịch đảo, toàn bộ quá trình chuyển đổi trở thành một tích chập đa thức. Vì mô đun`998244353`hỗ trợ NTT, mỗi chi phí tích chập`O(k log k)`thay vì thời gian bậc hai. 

Bản thân ranh giới được xử lý bằng cách chia để trị. Ở mỗi cấp độ đệ quy, chúng tôi chọn độ cao giữa của ranh giới đơn điệu, trích xuất hình chữ nhật tối đa có chiều cao đó, giải quyết các chuyển đổi ranh giới của nó bằng NTT và lặp lại trên hai phần còn lại. Các giá trị liền kề bằng nhau được giữ trong cùng một hình chữ nhật, đây là yếu tố xử lý các cao nguyên một cách chính xác. 

Tại bất kỳ độ sâu đệ quy cố định nào, các hình chữ nhật đều rời nhau nên tổng chiều rộng và chiều cao của chúng là`O(n)`. có`O(log n)`cấp độ và mỗi cấp độ thực hiện`O(n log n)`tổng cộng làm việc. Độ phức tạp thu được là`O(n log^2 n)`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê tất cả`b`|`O(n C(2n-1,n))`|`O(n)`| Quá chậm | 
| DP đầy đủ |`O(n^2)`|`O(n^2)`hoặc`O(n)`với hàng lăn | Quá chậm | 
| Hình chữ nhật chia để trị + NTT |`O(n log^2 n)`|`O(n log n)`trong biểu diễn ranh giới thưa thớt | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Giải thích mọi trình tự hợp lệ`b`như một đường dẫn mạng. Tạo một lưới có`i`-hàng thứ có`ai`tế bào. Di chuyển xuống từ hàng`i`tại cột`j`hồ sơ`bi = j`. Di chuyển sang phải sẽ làm tăng cột nên giá trị được ghi không giảm. Việc hạn chế chiều rộng hàng mang lại`bi <= ai`. 
2. Thêm một hàng có chiều rộng`n+1`. Hàng cuối cùng này cho phép mã hóa bước đi xuống cuối cùng`b_n`, do đó câu trả lời sẽ trở thành số đường đi từ góc trên bên trái tới góc dưới bên phải mới. 
3. Đảo ngược trình tự hàng. Bản gốc`a`không giảm nên sau khi đảo chiều nó không tăng. Điều này làm cho ranh giới trở thành một cầu thang có thể được phân tách thành các hình chữ nhật nằm ngang tối đa. 
4. Tính toán trước giai thừa và nghịch đảo giai thừa modulo`998244353`lên đến`2n+2`. Sau đó, mọi hệ số nhị thức được sử dụng bởi quá trình chuyển đổi hình chữ nhật có thể được đánh giá theo thời gian không đổi. 
5. Cho hình chữ nhật có chiều cao`h`và chiều rộng`w`, lưu trữ các giá trị ở ranh giới trên cùng của nó trong một mảng`top`và các giá trị ở ranh giới bên trái của nó trong một mảng`left`. Số lượng đường đi từ mỗi vị trí biên tới đích là một hệ số nhị thức vì một đường đi được xác định bởi vị trí di chuyển sang phải và xuống của nó. 
6. Tính ranh giới dưới từ ranh giới trên bằng tích chập`bottom[i] += sum(top[j] * C(i-j+h-1,h-1))`. 

Ranh giới bên trái cũng góp phần vào phía dưới:`bottom[i] += sum(left[j] * C(i+h-1-j,i))`. 

Tổng đầu tiên là một tích chập thông thường với dãy`C(k+h-1,h-1)`. Số thứ hai trở thành tích chập sau khi nhân`left[j]`qua`1/(h-1-j)!`và nhân hệ số kia với giai thừa. 

1. Tính toán đối xứng ranh giới bên phải. Ranh giới trên cùng đóng góp thông qua`right[i] += sum(top[j] * C(i+w-1-j,i))`, 

và ranh giới bên trái đóng góp thông qua`right[i] += sum(left[j] * C(i-j+w-1,w-1))`. 

Đây là hai mẫu tích chập giống nhau với chiều rộng và chiều cao được hoán đổi. 

1. Sử dụng NTT cho mỗi tích chập đủ lớn. Đối với các mảng ngắn, phép nhân trực tiếp trong Python nhanh hơn, do đó việc triển khai sẽ chuyển sang phương pháp bậc hai dưới ngưỡng nhỏ. 
2. Chia cầu thang bằng hàng giữa. Nếu một số hàng liên tiếp có cùng chiều cao biên thì coi chúng là một điểm bằng phẳng tối đa. Giải đệ quy phần phía trên cao nguyên, giải hình chữ nhật bị chiếm bởi cao nguyên, sau đó giải đệ quy phần bên dưới nó. Việc giữ nguyên cao nguyên sẽ ngăn việc xử lý cùng một đường ranh giới hai lần. 
3. Trạng thái ban đầu là góc trên bên trái. Sau khi toàn bộ cầu thang đã được xử lý, câu trả lời bắt buộc là giá trị DP ở góc dưới bên phải cuối cùng. 

### Tại sao nó hoạt động 

Việc xây dựng lưới điện mang lại sự song hành giữa pháp lý`b`trình tự và đường đi đơn điệu. Bên trong mỗi hình chữ nhật, phép lặp đường dẫn mạng tiêu chuẩn là chính xác, do đó các công thức nhị thức tính toán chính xác các giá trị DP giống như một phép tính đầy đủ.`O(n^2)`bảng sẽ chứa. Việc chia để trị chỉ thay đổi thứ tự tính toán các giá trị biên. Tính bất biến của nó là mọi giá trị mà hình chữ nhật con cần đều đã có sẵn ở ranh giới trên cùng hoặc bên trái của nó. Vì cầu thang được chia thành các hình chữ nhật rời rạc và quá trình chuyển đổi hình chữ nhật tính mọi lối đi đi qua một trong hai ranh giới chính xác một lần, nên mọi lối đi hợp pháp đều đến góc cuối cùng một lần và không có lối đi không hợp lệ nào được tính. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353
G = 3
NAIVE_LIMIT = 4096

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

        half = length >> 1
        for start in range(0, n, length):
            w = 1
            end = start + half
            for i in range(start, end):
                u = a[i]
                v = a[i + half] * w % MOD

                x = u + v
                if x >= MOD:
                    x -= MOD
                y = u - v
                if y < 0:
                    y += MOD

                a[i] = x
                a[i + half] = y
                w = w * wlen % MOD

        length <<= 1

    if invert:
        inv_n = pow(n, MOD - 2, MOD)
        for i in range(n):
            a[i] = a[i] * inv_n % MOD

def convolution(a, b):
    if not a or not b:
        return []

    need = len(a) + len(b) - 1

    if len(a) * len(b) <= NAIVE_LIMIT:
        c = [0] * need
        for i, x in enumerate(a):
            if x:
                for j, y in enumerate(b):
                    c[i + j] = (c[i + j] + x * y) % MOD
        return c

    size = 1
    while size < need:
        size <<= 1

    fa = a[:] + [0] * (size - len(a))
    fb = b[:] + [0] * (size - len(b))

    ntt(fa, False)
    ntt(fb, False)

    for i in range(size):
        fa[i] = fa[i] * fb[i] % MOD

    ntt(fa, True)
    return fa[:need]

def count_arrays(original):
    n = len(original)

    # Add the extra row, then reverse the staircase.
    a = [0] + original + [n + 1]
    a[1:] = reversed(a[1:])

    max_fact = 2 * n + 2
    fac = [1] * (max_fact + 1)
    invfac = [1] * (max_fact + 1)

    for i in range(1, max_fact + 1):
        fac[i] = fac[i - 1] * i % MOD

    invfac[max_fact] = pow(fac[max_fact], MOD - 2, MOD)
    for i in range(max_fact, 0, -1):
        invfac[i - 1] = invfac[i] * i % MOD

    def comb(x, y):
        if y < 0 or y > x:
            return 0
        return fac[x] * invfac[y] % MOD * invfac[x - y] % MOD

    # dp[row] stores only boundary values that are needed later.
    dp = [dict() for _ in range(n + 2)]

    def add_dp(row, col, value):
        d = dp[row]
        d[col] = (d.get(col, 0) + value) % MOD

    def rect(l, r, bot, top):
        if l == 1 and r == 1 and top == n + 1:
            for col in range(bot, top + 1):
                dp[1][col] = 1
            return

        width = r - l + 1
        height = top - bot + 1

        # Top boundary.
        upper = [
            dp[l + i].get(top + 1, 0)
            for i in range(width)
        ]

        # Left boundary.
        left = [
            dp[l - 1].get(top - i, 0)
            for i in range(height)
        ]

        bottom = [0] * width
        right = [0] * height

        # Left -> bottom.
        x = [
            left[i] * invfac[height - 1 - i] % MOD
            for i in range(height)
        ]
        y = fac[:width + height - 1]
        z = convolution(x, y)

        for i in range(width):
            bottom[i] = (
                bottom[i]
                + z[height - 1 + i] * invfac[i]
            ) % MOD

        # Top -> bottom.
        kernel = [
            comb(i + height - 1, height - 1)
            for i in range(width)
        ]
        z = convolution(upper, kernel)

        for i in range(width):
            bottom[i] = (bottom[i] + z[i]) % MOD

        # Top -> right.
        x = [
            upper[i] * invfac[width - 1 - i] % MOD
            for i in range(width)
        ]
        y = fac[:width + height - 1]
        z = convolution(x, y)

        for i in range(height):
            right[i] = (
                right[i]
                + z[width - 1 + i] * invfac[i]
            ) % MOD

        # Left -> right.
        kernel = [
            comb(i + width - 1, width - 1)
            for i in range(height)
        ]
        z = convolution(left, kernel)

        for i in range(height):
            right[i] = (right[i] + z[i]) % MOD

        for i in range(width):
            add_dp(l + i, bot, bottom[i])

        # The lower-right corner belongs to both boundaries.
        for i in range(top, bot, -1):
            add_dp(r, i, right[top - i])

    def solve_staircase(l, r, bot):
        if l > r:
            return

        mid = (l + r) >> 1

        x = mid
        while x - 1 >= l and a[x - 1] == a[mid]:
            x -= 1

        y = mid
        while y + 1 <= r and a[y + 1] == a[mid]:
            y += 1

        solve_staircase(l, x - 1, a[mid] + 1)
        rect(l, y, bot, a[mid])
        solve_staircase(y + 1, r, bot)

    solve_staircase(1, n + 1, 1)
    return dp[n + 1].get(1, 0)

def solve_data(data):
    it = iter(map(int, data.split()))
    t = next(it)
    out = []

    for _ in range(t):
        n = next(it)
        a = [next(it) for _ in range(n)]
        out.append(str(count_arrays(a)))

    return "\n".join(out)

def solve():
    data = sys.stdin.buffer.read()
    sys.stdout.write(solve_data(data))

if __name__ == "__main__":
    solve()
```Việc khởi tạo giai thừa được thực hiện trước tiên vì mọi chuyển đổi hình chữ nhật đều sử dụng hệ số nhị thức. Chỉ số giai thừa lớn nhất là nhiều nhất`2n+2`, vì một hình chữ nhật có nhiều nhất`n+1`hàng và`n+1`cột. 

các`convolution`thường lệ có chủ ý có một nhánh nhân trực tiếp nhỏ. NTT có chi phí cố định từ việc đảo ngược bit và một số lần chuyển đổi, do đó việc nhân trực tiếp hai mảng nhỏ sẽ nhanh hơn. Các sản phẩm lớn sử dụng gốc nguyên thủy dành riêng cho mô đun`3`, điều này hợp lệ vì`998244353 = 119 * 2^23 + 1`. 

Quy trình hình chữ nhật tuân theo chính xác bốn lần chuyển ranh giới. các`height - 1 + i`Và`width - 1 + i`chỉ số là những phần tinh tế nhất. Chúng xuất phát từ việc dịch chuyển tích chập để các giai thừa biến hệ số nhị thức thành tích của hai chuỗi một chiều. 

các`dp`cấu trúc thưa thớt vì lưới đầy đủ có kích thước bậc hai. Chỉ các giá trị nằm trên ranh giới của hình chữ nhật mới được lưu trữ. Khi một giá trị mới thuộc về một ranh giới hiện có,`add_dp`thêm vào nó thay vì ghi đè lên nó. Góc dưới bên phải được tạo ra bởi cả phép tính dưới cùng và bên phải, do đó việc triển khai cố tình bỏ qua nó trong lần chèn thứ hai. 

Số nguyên trong Python không bị tràn nhưng mỗi phép nhân vẫn bị giảm modulo`998244353`ngay lập tức. Độ sâu đệ quy là logarit vì mỗi vùng cầu thang được chia xung quanh phần giữa của nó, do đó giới hạn đệ quy Python thông thường là đủ cho việc phân chia và chinh phục chính nó. 

Việc triển khai tuân theo chiến lược hình chữ nhật và NTT giống như cách tiếp cận cuộc thi được chấp nhận. Vấn đề ban đầu và các giới hạn chính thức của nó có sẵn trên Codeforces. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Mẫu đầu tiên có`n = 4`Và`a = [1,1,1,1]`. Sau khi thêm giá trị biên bổ sung và đảo ngược, độ rộng của hàng là`[5,1,1,1,1]`. Đường dẫn không có lựa chọn về cột của nó trong khi nó đi qua bốn hàng có chiều rộng một, do đó chỉ có một đường dẫn tồn tại. 

| Sân khấu | Ranh giới hàng đảo ngược | Chiều cao hình chữ nhật | Chiều rộng hình chữ nhật | Kết quả | 
| --- | --- | --- | --- | --- | 
| Trạng thái ban đầu |`5,1,1,1,1`| 1 | 5 | Năm trạng thái biên ban đầu có giá trị`1`| 
| Cao nguyên |`1,1,1,1`| 4 | 1 | Chỉ cột`1`vẫn có thể truy cập được | 
| Góc cuối cùng |`(5,1)`| | |`dp[5][1] = 1`| 

Dấu vết cho thấy tại sao các giá trị biên bằng nhau phải được xử lý như một cao nguyên. Không có sự phân nhánh trong chuỗi gốc nên thuật toán phải bảo toàn chính xác một đường đi. 

### Mẫu 2 

cho`a = [1,2,3,4]`, dãy mở rộng là`[1,2,3,4,5]`. Lưới kết quả là một cầu thang và các đường dẫn hợp lệ chính xác là các đường dẫn từ góc trên bên trái đến góc dưới bên phải không bao giờ rời khỏi cầu thang đó. có`14`những con đường như vậy. 

| Sân khấu | Ranh giới lưới | Số lượng đường dẫn | 
| --- | --- | --- | 
| Bắt đầu |`(1,1)`|`1`| 
| Sau bậc thang đầu tiên | chiều rộng`1`|`1`| 
| Sau cấp hai | chiều rộng`2`|`2`| 
| Sau cấp ba | chiều rộng`3`|`5`| 
| Sau cấp bốn | chiều rộng`4`|`14`| 

Trình tự`1, 2, 5, 14`là sự khởi đầu của dãy Catalan. Ví dụ này cũng chứng minh tại sao hàng bổ sung lại cần thiết. Việc đếm các đường đi chỉ qua hàng thứ tư ban đầu sẽ dừng lại trước khi mã hóa lựa chọn tự do cuối cùng được biểu thị bằng`b4`. 

Mẫu chính thức thứ ba,`a = [4,4,4,4]`, có câu trả lời`35`, khớp với số lượng sao và vạch trực tiếp của bốn giá trị không giảm được chọn từ bốn nhiệt độ có thể. Các kết quả đầu ra mẫu chính thức là`1`,`14`, Và`35`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n log^2 n)`| Mỗi cấp độ phân chia và chinh phục có tổng kích thước ranh giới hình chữ nhật`O(n)`và mỗi lần chuyển đổi ranh giới sử dụng tích chập NTT | 
| Không gian |`O(n log n)`| Sử dụng giai thừa`O(n)`, trong khi các ranh giới hình chữ nhật thưa thớt yêu cầu tối đa nhiều lớp được lưu trữ theo logarit cho mỗi vị trí | 

Tổng cộng`n`trên tất cả các trường hợp thử nghiệm là nhiều nhất`5 * 10^5`, do đó, các hệ số logarit được chia sẻ trên tổng kích thước đầu vào bị chặn. Việc triển khai C++ dự định nằm trong giới hạn chính thức 8 giây và 512 MB. 

## Trường hợp thử nghiệm```
# The solution is assumed to be saved as solution.py.
# It exposes solve_data(data), which returns the complete output string.

from solution import solve_data

def run(inp: str) -> str:
    return solve_data(inp).strip()

# Official samples.
assert run(
    """3
4
1 1 1 1
4
1 2 3 4
4
4 4 4 4
"""
) == "1\n14\n35", "official samples"

# Minimum size.
assert run(
    """1
1
1
"""
) == "1", "minimum-size case"

# Repeated values with a non-trivial transition.
# Valid pairs (b1,b2) are (1,1), (1,2), (2,2),
# giving 3, 2, 2 choices for b3 respectively.
assert run(
    """1
3
2 2 3
"""
) == "7", "repeated boundary values"

# Boundary case where every A value is maximal.
assert run(
    """1
4
4 4 4 4
"""
) == "35", "all values equal to n"

# Large input, all values equal to 1.
# There is exactly one possible B array.
n = 200000
large = "1\n{}\n{}\n".format(n, " ".join(["1"] * n))
assert run(large) == "1", "maximum-size all-one case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 / 1`|`1`| Kích thước tối thiểu và ranh giới dưới | 
|`3 / 2 2 3`|`7`| Giá trị lặp lại và xử lý cao nguyên | 
|`4 / 4 4 4 4`|`35`| Tất cả các giá trị bằng mức tối đa | 
|`200000 / 1 1 ... 1`|`1`| Tối đa`n`, xử lý bộ nhớ và ranh giới cực phẳng | 

## Vỏ cạnh 

cho`n = 1`Và`a = [1]`, dãy mở rộng là`[1,2]`. Chỉ có một giá trị có thể có cho`b1`, cụ thể là`1`. Lưới có đúng một đường dẫn hợp lệ nên thuật toán đạt đến góc cuối cùng có giá trị`1`. 

Vì`a = [1,2,3,4]`, ranh giới mở rộng là`[1,2,3,4,5]`. Số lượng lối đi cầu thang là`14`. Công trình dừng ở hàng ban đầu`n`sẽ chỉ được tính`5`đường dẫn, bởi vì nó chưa đại diện cho sự lựa chọn của`b_n`. Hàng bổ sung sửa lỗi ranh giới này bằng cách chuyển`b_n`bước vào quá trình chuyển đổi đi xuống cuối cùng. 

Vì`a = [4,4,4,4]`, mọi dãy bốn phần tử không giảm trên`{1,2,3,4}`là hợp lệ. Số lượng là`C(4+4-1,4) = C(7,4) = 35`. Trong lưới đảo ngược, ranh giới hầu hết bằng phẳng, do đó logic cao nguyên tối đa xử lý toàn bộ phần lặp lại dưới dạng một hình chữ nhật thay vì coi mỗi hàng bằng nhau là một thay đổi ranh giới độc lập. 

Vì`a = [2,2,3]`, câu trả lời là`7`. Hai vị trí đầu tiên có thể là`(1,1)`,`(1,2)`, hoặc`(2,2)`. Khi những giá trị đó đã được cố định, giá trị cuối cùng sẽ tương ứng`3`,`2`, hoặc`2`những giá trị có thể. Quá trình chuyển đổi hình chữ nhật thêm các họ đường dẫn này mà không hợp nhất các đường dẫn riêng biệt một cách không chính xác, tạo ra`7`. 

Đối với trường hợp kích thước tối đa`n = 200000`với mọi`ai = 1`, mọi`bi`cũng phải bằng`1`, vậy đáp án vẫn là`1`. Thuật toán không bao giờ xây dựng đầy đủ`200000 × 200000`bảng DP. Nó chỉ lưu trữ các giá trị ranh giới thưa thớt và sử dụng cầu thang phẳng để giữ cho sự phân chia và chinh phục nông cạn.
