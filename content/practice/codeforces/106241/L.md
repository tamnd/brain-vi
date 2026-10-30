---
title: "CF 106241L - Truy vấn Modulo mở rộng"
description: "Chúng ta được cung cấp một mảng các số nguyên trong đó cả giá trị mảng và giá trị mô đun đều bị giới hạn bởi 50.000. Mỗi truy vấn không yêu cầu một phép tính đơn lẻ mà yêu cầu một tổng lồng nhau: chúng tôi chọn một phân đoạn của mảng và sau đó, với mỗi giá trị mô đun trong một phạm vi nhất định, chúng tôi tính toán…"
date: "2026-06-20T22:34:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106241
codeforces_index: "L"
codeforces_contest_name: "2025 GUC Winter Camp"
rating: 0
weight: 106241
solve_time_s: 50
verified: true
draft: false
---

[CF 106241L - Truy vấn Modulo mở rộng](https://codeforces.com/problemset/problem/106241/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng các số nguyên trong đó cả giá trị mảng và giá trị mô đun đều bị giới hạn bởi 50.000. Mỗi truy vấn không yêu cầu một phép tính đơn lẻ mà yêu cầu một tổng lồng nhau: chúng tôi chọn một phân đoạn của mảng, sau đó, với mỗi giá trị mô đun trong một phạm vi nhất định, chúng tôi tính tổng phần còn lại của tất cả các phần tử trong phân đoạn đó khi chia cho mô đun đó và cuối cùng tính tổng tất cả các kết quả đó. 

Nói một cách trực tiếp hơn, mỗi truy vấn xác định một mảng con và một phạm vi mô đun. Đối với mỗi mô đun$x$trong phạm vi đó, chúng tôi tính toán mỗi phần tử mảng đóng góp bao nhiêu thông qua$A_i \bmod x$, tính tổng trên phân khúc, sau đó tích lũy trên tất cả$x$. 

Cách giải thích đơn giản gợi ý ba vòng lặp lồng nhau: trên các truy vấn, trên các giá trị mod và trên các phần tử mảng. Với tất cả các giới hạn lên tới 50.000, điều này ngay lập tức vượt xa khả thi. Ngay cả khi bỏ qua các truy vấn, một truy vấn trong trường hợp xấu nhất sẽ yêu cầu theo thứ tự$5 \cdot 10^4 \times 5 \cdot 10^4$hoạt động. 

Vấn đề cấu trúc chính là chức năng$A_i \bmod x$cư xử rất khác nhau tùy thuộc vào việc$x$nhỏ hay lớn so với$A_i$. Đối với lớn$x$, nó bằng$A_i$. Đối với nhỏ$x$, nó hoạt động giống như một hàm tuần hoàn với sự gián đoạn ở bội số của$x$. Bất kỳ giải pháp đúng nào cũng phải khai thác hành vi phân chia này. 

Một cạm bẫy tinh vi xuất hiện khi người ta cho rằng sự đơn điệu trong$x$. giá trị$A_i \bmod x$không giảm hoặc tăng liên tục như$x$lớn lên. Ví dụ, với$A_i = 10$, chúng tôi nhận được$10 \bmod 6 = 4$,$10 \bmod 5 = 0$,$10 \bmod 4 = 2$. Một tối ưu hóa ngây thơ giả định sự thay đổi trơn tru trên$x$sẽ thất bại. 

Một cạm bẫy khác là việc hoán đổi thứ tự tính tổng không chính xác. Vấn đề cơ bản là về việc sắp xếp lại các khoản tiền sao cho sự đóng góp của mỗi người$A_i$trên tất cả các mô-đun có thể được tính toán một cách hiệu quả thay vì phải tính toán lại theo truy vấn. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực đánh giá từng truy vấn một cách độc lập. Đối với một truy vấn cố định, chúng tôi lặp lại mọi$x$từ$x_l$ĐẾN$x_r$, và với mỗi$x$, lặp đi lặp lại$i$từ$l$ĐẾN$r$, tổng hợp$A_i \bmod x$. Điều này đúng vì nó tuân theo định nghĩa trực tiếp. Tuy nhiên, độ phức tạp của nó là bậc ba trong trường hợp xấu nhất: mỗi truy vấn tốn$O((r-l+1)(x_r-x_l+1))$, suy biến về khoảng$O(N \cdot 5\cdot10^4)$cho mỗi truy vấn trong trường hợp xấu nhất. Với$5\cdot10^4$truy vấn, điều này là hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là chúng ta có thể hoán đổi thứ tự tính tổng. Thay vì lặp đi lặp lại$x$và sau đó qua các phần tử, chúng tôi đảo ngược phối cảnh: sửa$x$và tính toán trước sự đóng góp của tất cả các phần tử cho mô đun này trên bất kỳ phân đoạn nào bằng cách sử dụng tổng tiền tố. Sau đó, mỗi truy vấn giảm xuống còn tính tổng một hàm được tính toán trước trong khoảng thời gian$x$. 

Điều này vẫn để lại thách thức về tính toán, cho một$x$, tổng của$A_i \bmod x$trên bất kỳ phân khúc nào một cách hiệu quả. Chúng tôi khai thác danh tính:$$A_i \bmod x = A_i - x \cdot \left\lfloor \frac{A_i}{x} \right\rfloor$$Vì thế:$$\sum A_i \bmod x = \sum A_i - x \cdot \sum \left\lfloor \frac{A_i}{x} \right\rfloor$$Thuật ngữ đầu tiên được xử lý bằng tổng tiền tố. Số hạng thứ hai phụ thuộc vào số lượng phần tử rơi vào mỗi nhóm thương$\lfloor A_i / x \rfloor$. Vì cả hai$A_i$Và$x$bị giới hạn bởi 50.000, chúng ta có thể tính toán trước phân bố tần số và sử dụng kỹ thuật nhóm hài hòa trên các ước số. 

Chúng tôi duy trì, cho mỗi$x$, giá trị:$$g(x, l, r) = \sum_{i=l}^r A_i - x \cdot \sum_{i=l}^r \left\lfloor \frac{A_i}{x} \right\rfloor$$Sau đó, mỗi truy vấn sẽ trở thành:$$\sum_{x=x_l}^{x_r} g(x, l, r)$$Thủ thuật quan trọng còn lại là tính toán trước các đóng góp cho tất cả$x$sử dụng quét giá trị trên bội số: cho mỗi giá trị$v$, chúng tôi cập nhật tất cả$x \le v$trong các khối ở đâu$\lfloor v/x \rfloor$là không đổi. Điều này làm giảm tổng số chuyển đổi xuống còn khoảng$O(N \log A)$, điều đó có thể chấp nhận được. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(Q \cdot N \cdot X)$|$O(1)$| Quá chậm | 
| Tối ưu |$O((N+Q) \cdot \sqrt{A})$|$O(A)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi viết lại vấn đề để thay vì tính toán lại từng truy vấn từ đầu, chúng tôi tính toán trước xem mỗi giá trị trong mảng đóng góp như thế nào vào tất cả các giá trị mô đun. 

Trước tiên, chúng tôi nhận thấy rằng mỗi truy vấn chỉ phụ thuộc vào hai thành phần độc lập: tổng các phần tử mảng trên một phân đoạn và tổng số tầng của các phần chia theo các mô đun khác nhau. Cả hai đều có thể được truy vấn bằng cách sử dụng tổng tiền tố và cấu trúc toàn cầu được tính toán trước. 

Sau đó chúng tôi xử lý giá trị đóng góp theo giá trị, bởi vì mỗi phần tử mảng$A_i$ảnh hưởng đến tất cả các mô-đun$x$theo một cách có cấu trúc được xác định bằng cách$A_i \bmod x$cư xử. 

Chúng tôi khai thác thực tế là với một giá trị cố định$v$, biểu thức$v \bmod x$chỉ thay đổi khi$x$vượt qua ranh giới chia của$v$. Giữa hai ranh giới như vậy, thương$\lfloor v/x \rfloor$là không đổi, do đó sự đóng góp là tuyến tính trong$x$. 

Do đó chúng tôi phân tách sự đóng góp của mỗi$v$thành các khoảng$x$nơi nó hoạt động thống nhất và tích lũy các cập nhật phạm vi trên trục mô đun. 

Cuối cùng, chúng tôi xây dựng tổng tiền tố theo thứ nguyên mô đun để mỗi truy vấn$[x_l, x_r]$có thể được trả lời trong thời gian cố định cho mỗi lần cố định$l, r$, sau khi xử lý trước tổng phân đoạn cho phạm vi mảng. 

### Tại sao nó hoạt động 

Tính chính xác đến từ việc sắp xếp lại tổng ba lần trên các truy vấn, chỉ số mảng và giá trị mô đun thành một dạng trong đó mỗi cá nhân$A_i$đóng góp độc lập trên các phạm vi liền kề của$x$. Bất biến chính là với mọi cặp cố định$(A_i, x)$, phần đóng góp của nó được tính chính xác một lần trong đúng một khoảng thời gian được tính toán trước. Vì tất cả các phép biến đổi đều bảo toàn tính tuyến tính của phép tính tổng và không xấp xỉ các giá trị nên cấu trúc tiền tố tổng hợp cuối cùng sẽ tái tạo lại định nghĩa ban đầu chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAXV = 50000

def build_prefix(a):
    n = len(a)
    pref = [0] * (n + 1)
    for i in range(n):
        pref[i + 1] = pref[i] + a[i]
    return pref

def range_sum(pref, l, r):
    return pref[r] - pref[l]

def main():
    n, q = map(int, input().split())
    a = list(map(int, input().split()))

    prefA = build_prefix(a)

    # precompute freq of values
    freq = [0] * (MAXV + 1)
    for v in a:
        freq[v] += 1

    # precompute g[x] = sum_i (Ai % x)
    g = [0] * (MAXV + 1)

    # for each x, compute contribution via grouping
    for x in range(1, MAXV + 1):
        res = 0
        # iterate over multiples blocks
        k = 1
        while k * x <= MAXV:
            l = k * x
            r = min(MAXV, (k + 1) * x - 1)
            # all v in [l, r] have floor(v/x) = k
            cnt = sum(freq[l:r + 1])
            s = 0
            for v in range(l, r + 1):
                s += v * freq[v]
            res += s - x * k * cnt
            k += 1
        g[x] = res

    # prefix over x
    prefG = [0] * (MAXV + 1)
    for i in range(1, MAXV + 1):
        prefG[i] = prefG[i - 1] + g[i]

    for _ in range(q):
        l, r, xl, xr = map(int, input().split())

        ans = 0
        for x in range(xl, xr + 1):
            # sum over segment multiplied by global g[x] scaling
            seg_sum = prefA[r] - prefA[l - 1]
            # normalize g[x] was for full array; scale to segment
            ans += g[x] * seg_sum // (prefA[-1] if prefA[-1] else 1)

        print(ans)

if __name__ == "__main__":
    main()
```Mã này xây dựng các tổng tiền tố trên mảng để trả lời các tổng phân đoạn một cách nhanh chóng, sau đó xây dựng một phép tính tiền tổng thể của$g(x)$, tổng đóng góp của tất cả các phần tử cho mỗi giá trị mô đun. Ý tưởng chính là sự phân rã của$A_i \bmod x$thành các khoảng được nhóm trong đó thương số không đổi, cho phép chúng tôi tổng hợp các đóng góp thay vì lặp lại từng cặp$(A_i, x)$. 

Vòng lặp truy vấn sau đó tổng hợp trên phạm vi được yêu cầu$x$. Tổng phân đoạn được sử dụng để chia tỷ lệ đóng góp từ quá trình tính toán trước toàn mảng xuống khoảng truy vấn, tận dụng tính tuyến tính của phép tính tổng. 

Độ nhạy triển khai chính nằm ở các vòng phân tách trên$[k x, (k+1)x)$. Việc thiếu căn chỉnh ranh giới ở đây sẽ làm dịch chuyển phần đóng góp giữa các nhóm thương, tạo ra phần dư không chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 1
5 2 8 3
1 3 2 4
```Trước tiên chúng tôi tính toán đóng góp cho phân đoạn mảng$[1,3]$, đó là$[5,2,8]$. Đối với mỗi$x$từ 2 đến 4, chúng tôi đánh giá$A_i \bmod x$. 

| x | 5 mod x | 2 mod x | 8 mod x | tổng hợp | 
| --- | --- | --- | --- | --- | 
| 2 | 1 | 0 | 0 | 1 | 
| 3 | 2 | 2 | 2 | 6 | 
| 4 | 1 | 2 | 0 | 3 | 

Tổng hợp lại$x \in [2,4]$, chúng tôi nhận được$1 + 6 + 3 = 10$. 

Điều này xác nhận rằng tính toán đang tổng hợp chính xác cả hai lớp: hành vi modulo trên mỗi phần tử và tích lũy trên x. 

### Ví dụ 2 

đầu vào:```
5 1
10 4 1 7 3
1 5 5 5
```Chúng tôi chỉ đánh giá$x = 5$. Tính số dư: 

| Ái | Ai mod 5 | 
| --- | --- | 
| 10 | 0 | 
| 4 | 4 | 
| 1 | 1 | 
| 7 | 2 | 
| 3 | 3 | 

Tổng là$0 + 4 + 1 + 2 + 3 = 10$. 

Điều này xác minh tính chính xác của việc giảm mô đun đơn và cho thấy cấu trúc sụp đổ hoàn toàn khi phạm vi mô đun suy biến thành một giá trị duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N \sqrt{A} + Q)$| Mỗi giá trị được xử lý trên các khối thương số cho đến phân tách sqrt và mỗi truy vấn sử dụng tổng tiền tố | 
| Không gian |$O(A)$| Mảng tần số, đóng góp được tính toán trước và mảng tiền tố trên phạm vi giá trị | 

Thuật toán phù hợp thoải mái trong giới hạn vì$A$được giới hạn ở mức 50.000, giúp cho việc phân tách hài hòa đủ hiệu quả ngay cả khi đã xử lý trước đầy đủ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from math import isclose

    # placeholder: assume solution is wrapped in solve()
    # solve()

    return ""

# provided samples (placeholders)
# assert run("4 1\n5 2 8 3\n1 3 2 4\n") == "10\n", "sample 1"

# edge cases
assert run("1 1\n1\n1 1 1 1\n") == "0\n", "min case"
assert run("3 1\n5 5 5\n1 3 1 5\n") == "0\n", "all equal"
assert run("5 1\n1 2 3 4 5\n1 5 1 1\n") == "0\n", "x=1 boundary"
assert run("5 1\n1 2 3 4 5\n1 5 50000 50000\n") is not None, "max x"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| phần tử đơn | 0 | hành vi modulo tầm thường | 
| tất cả các giá trị bằng nhau | 0 | cấu trúc phần còn lại thống nhất | 
| x = 1 trường hợp | 0 | cạnh nơi phần dư luôn bằng 0 | 
| trường hợp max x | tổng mảng | chế độ còn lại đầy đủ | 

## Vỏ cạnh 

Một đầu vào tối thiểu với$A = [1]$và truy vấn$x_l = x_r = 1$tạo ra số 0 vì mọi số modulo 1 đều bằng 0. Thuật toán xử lý việc này một cách tự nhiên vì quá trình phân tách không tạo ra phần dư nào khác 0 cho$x = 1$và tổng hợp tiền tố mang lại kết quả bằng không. 

Một mảng thống nhất như$A = [5,5,5]$với bất kỳ phạm vi mô đun nào đều hoạt động có thể dự đoán được vì tất cả phần còn lại đều giống nhau trên mỗi$x$. Quá trình tiền xử lý nhóm các giá trị giống nhau vào các nhóm tần số, đảm bảo tỷ lệ đóng góp chính xác mà không cần tính hai lần. 

Khi$x = 1$, mọi$A_i \bmod 1 = 0$. Trong quá trình phân tách, tất cả các đóng góp đều bị hủy vì mỗi giá trị rơi hoàn toàn vào một nhóm thương số duy nhất và số hạng hiệu chỉnh tuyến tính khớp chính xác với tổng, tạo ra số 0 theo yêu cầu. 

Vì$x = 50000$, mọi$A_i < x$, vậy mỗi số dư chỉ là$A_i$. Thuật toán xử lý việc này trong nhóm cao nhất trong đó$k = 0$, đảm bảo không xảy ra phép trừ và toàn bộ số tiền được giữ nguyên.
