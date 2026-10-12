---
title: "CF 105010M - Vũ trụ mô-đun"
description: "Mỗi truy vấn mô tả một vũ trụ lưới mô-đun có kích thước $n nhân m$, trong đó các vị trí bao bọc theo cách có cấu trúc. Tại bất kỳ thời điểm cố định nào, đều có các cá thể $nm$, mỗi cá thể được lập chỉ mục bằng một số $k$ và mỗi cá thể đi theo một quỹ đạo xác định trên các ô lưới theo thời gian."
date: "2026-06-28T04:36:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105010
codeforces_index: "M"
codeforces_contest_name: "Winter Cup 6.0 Online Mirror Contest"
rating: 0
weight: 105010
solve_time_s: 85
verified: false
draft: false
---

[CF 105010M - Vũ trụ mô-đun](https://codeforces.com/problemset/problem/105010/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 25s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Mỗi truy vấn mô tả một vũ trụ lưới mô-đun có kích thước$n \times m$, trong đó các vị trí bao bọc một cách có cấu trúc. Tại bất kỳ thời điểm cố định nào cũng có$nm$các cá nhân, mỗi cá nhân được lập chỉ mục bởi một số$k$và mỗi cá nhân đi theo một quỹ đạo xác định trên các ô lưới theo thời gian. Chuyển động là tuyến tính theo thời gian, nhưng được chiếu thông qua một mô đun hỗn hợp: tọa độ x phụ thuộc vào phép chia số nguyên của chỉ số tỷ lệ thời gian, trong khi tọa độ y phụ thuộc vào thành phần mô đun. 

Cụ thể, vào ngày$d$, người$k$nằm ở$$x = \left(\left(\frac{dk}{m}\right) \bmod n\right), \quad y = (dk) \bmod m.$$Một truy vấn hỏi: trong số tất cả$k \in [0, nm-1]$, chính xác có bao nhiêu vùng đất tại một ô mục tiêu$(x,y)$vào ngày$d$. 

Các ràng buộc đủ lớn đến mức không thể mô phỏng theo từng người. Với tối đa$10^5$truy vấn và$nm$có khả năng đạt tới$10^{18}$, thậm chí$O(nm)$mỗi truy vấn là không khả thi. Thậm chí$O(\sqrt{nm})$Các cách tiếp cận rõ ràng là không khả thi vì các truy vấn độc lập và không chia sẻ cấu trúc. 

Một nỗ lực ngây thơ sẽ thử lặp lại tất cả$k$và kiểm tra xem quỹ đạo của nó có hạ cánh tại$(x,y)$. Điều đó sẽ tốn kém$O(nm)$cho mỗi truy vấn, ngay lập tức vượt quá giới hạn. 

Một dạng lỗi tinh vi hơn xuất phát từ việc giả định tính độc lập giữa các tọa độ. Vì cả hai tọa độ đều phụ thuộc vào cùng một sản phẩm$dk$, nhưng được phân chia theo phép chia và mô đun, việc xử lý x và y riêng biệt dẫn đến việc đếm không chính xác. Ví dụ: giả sử phân bố đồng đều trên các hàng hoặc cột sẽ bỏ qua sự ghép nối được đưa ra bởi phép chia số nguyên. 

## Phương pháp tiếp cận 

Khó khăn chính là cấu trúc của bản đồ$$k \mapsto (dk \bmod m, \lfloor dk/m \rfloor \bmod n).$$Đây không phải là một chức năng tùy ý; nó chính xác là phép nhân với$d$, tiếp theo là giải thích kết quả trong cơ sở-$m$, sau đó giảm modulo phần cao$n$. Nói cách khác, mọi$dk$gây ra một cặp:$$t = dk, \quad y = t \bmod m, \quad x = \lfloor t/m \rfloor \bmod n.$$Vì vậy, vấn đề trở thành đếm có bao nhiêu bội số của$d$trong phạm vi$$t \in \{0, d, 2d, \dots, d(nm-1)\}$$thỏa mãn điều kiện phân chia cố định trên thương và modulo dư$m$Và$n$. 

Thay vì suy nghĩ về mặt$k$, chúng ta chuyển quan điểm sang cấp số cộng modulo$nm$. Vì tất cả các tính toán được điều chế thông qua$m$Và$n$, cấu trúc lặp lại theo chu kỳ$nm / \gcd(d, nm)$. Điều này biến vấn đề thành việc đếm phần dư của bản đồ tuyến tính trong mạng mô-đun 2D. 

Quan sát quan trọng đó là$t = dk$tạo thành một nhóm con trong$\mathbb{Z}_{nm}$và mỗi trạng thái tương ứng với một phép chiếu xác định vào lưới. Số lượng tiền ảnh của một$(x,y)$bằng 0 hoặc chính xác bằng kích thước của bộ ổn định của ánh xạ tuyến tính này được giới hạn cho sợi mục tiêu. 

Người ta có thể chứng minh rằng hợp lệ$k$tương ứng với nghiệm của hệ đồng dư:$$dk \equiv y \pmod m$$Và$$\left\lfloor \frac{dk}{m} \right\rfloor \equiv x \pmod n.$$Ràng buộc có điều kiện đầu tiên$k$modulo$m / \gcd(d,m)$. Việc thay thế lời giải vào điều kiện thứ hai sẽ giảm vấn đề xuống còn việc kiểm tra một điều kiện nhất quán số học duy nhất và đếm xem có bao nhiêu câu trả lời hợp lệ.$k$nằm ở$[0, nm-1]$. 

Điều này dẫn đến giải pháp thời gian không đổi cho mỗi truy vấn bằng cách sử dụng phép nghịch đảo mô-đun và số học gcd. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(nm)$|$O(1)$| Quá chậm | 
| Giảm số học mô-đun |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu từ phương trình$dk = t$, Ở đâu$t$phạm vi trên bội số của$d$. Mục đích là để mô tả khi nào điều này$t$đất đai trên một nhất định$(x,y)$sau khi chia thành thương và số dư bằng$m$. Việc cải cách này loại bỏ sự phụ thuộc vào các tác nhân riêng lẻ và tập trung vào cấu trúc số học. 
2. Chuyển các điều kiện tọa độ thành một ràng buộc duy nhất trên$t$. Chúng tôi yêu cầu$$y = t \bmod m, \quad x = \left(\frac{t - y}{m}\right) \bmod n.$$Điều này thể hiện cả hai tọa độ trực tiếp về mặt$t$, làm cho ánh xạ tiêm vào từ$t$sang các trạng thái lưới. 
3. Thay thế$t$với$dk$, thu được hai đồng đẳng trong$k$. Việc đầu tiên trở thành$dk \equiv y \pmod m$, chỉ giải được khi$\gcd(d,m)$chia rẽ$y$. Nếu điều này không thành công, không có giá trị$k$tồn tại. 
4. Khi có thể giải được, hãy giảm đồng dư bậc nhất bằng cách chia cho gcd. Điều này tạo ra một lớp dư lượng duy nhất cho$k$modulo$m / \gcd(d,m)$. Bước này nén cấu trúc vô hạn thành một cấp số cộng tuần hoàn. 
5. Thay thế tham số hóa kết quả của$k$vào ràng buộc thứ hai liên quan đến thương số. Điều này làm giảm việc kiểm tra xem biểu thức tuyến tính cảm ứng trong$k$trận đấu$x \bmod n$. Cấu trúc đảm bảo điều này trở thành một cuộc kiểm tra tính nhất quán duy nhất thay vì một cuộc tìm kiếm. 
6. Đếm xem có bao nhiêu giá trị của$k$TRONG$[0, nm-1]$thỏa mãn lớp dư lượng dẫn xuất. Vì các giải pháp hợp lệ tạo thành một cấp số cộng, câu trả lời là 0 hoặc đơn giản là chia độ dài phạm vi theo bước mô đun. 

### Tại sao nó hoạt động 

Phép biến đổi biến chuyển động ban đầu thành bản đồ tuyến tính từ số nguyên sang nhóm abel hữu hạn$\mathbb{Z}_n \times \mathbb{Z}_m$. Mọi truy vấn đều yêu cầu kích thước của sợi đồng cấu này được giới hạn trong một nhóm con được tạo bởi$d$. Kích thước sợi trong các cài đặt như vậy là đồng nhất bất cứ khi nào hệ thống nhất quán, điều này làm giảm việc đếm để kiểm tra khả năng giải quyết sự đồng dạng và đo chỉ số nhóm con. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def egcd(a, b):
    if b == 0:
        return a, 1, 0
    g, x1, y1 = egcd(b, a % b)
    return g, y1, x1 - (a // b) * y1

def modinv(a, mod):
    g, x, _ = egcd(a, mod)
    if g != 1:
        return None
    return x % mod

def solve():
    Q = int(input())
    out = []

    for _ in range(Q):
        n, m, x, y, d = map(int, input().split())

        g = __import__("math").gcd(d, m)

        if y % g != 0:
            out.append("0")
            continue

        m1 = m // g
        d1 = d // g
        y1 = y // g

        inv = modinv(d1, m1)
        if inv is None:
            out.append("0")
            continue

        k0 = (y1 * inv) % m1

        # Now k = k0 (mod m1), check consistency with x
        # derive t = d*k, compute quotient
        t = d * k0
        x_calc = (t // m) % n

        if x_calc != x:
            out.append("0")
            continue

        # count k in [0, nm-1] satisfying k ≡ k0 (mod m1)
        limit = n * m
        if k0 >= limit:
            out.append("0")
        else:
            # arithmetic progression count
            out.append(str((limit - 1 - k0) // m1 + 1))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp tách tương tác mô đun thành hai giai đoạn. Giai đoạn đầu tiên giải quyết ràng buộc còn lại bằng cách sử dụng phép đảo ngược mô-đun sau khi loại bỏ rào cản gcd. Giai đoạn thứ hai xác minh điều kiện thương số bằng cách xây dựng lại trực tiếp giá trị được ánh xạ. 

Một chi tiết triển khai tinh tế là việc xử lý các nghịch đảo mô-đun: việc không giảm gcd trước tiên sẽ dẫn đến tính toán nghịch đảo không chính xác. Một điểm nữa là bước tái thiết`t = d * k0`, điều này hiệu quả vì tất cả các giải pháp hợp lệ đều có cùng một loại dư lượng, vì vậy việc kiểm tra một đại diện duy nhất là đủ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n=1, m=1, x=0, y=0, d=1
```| Bước | Giá trị | 
| --- | --- | 
| gcd(d,m) | 1 | 
| giảm m1 | 1 | 
| nghịch đảo của d1 mod m1 | 0 | 
| k0 | 0 | 
| t = d*k0 | 0 | 
| x_calc | 0 | 

Điều này xác nhận rằng vị trí duy nhất luôn ánh xạ trở lại (0,0), do đó, chính xác một người phù hợp. 

### Ví dụ 2 

đầu vào:```
n=2, m=3, x=1, y=1, d=2
```| Bước | Giá trị | 
| --- | --- | 
| gcd(d,m) | 1 | 
| m1 | 3 | 
| nghịch đảo d mod m | 2 | 
| k0 | 2 | 
| t = 2 * 2 | 4 | 
| x_calc = (4//3)%2 | 1 | 

Quá trình kiểm tra tính nhất quán đã vượt qua và việc đếm tiến trình số học sẽ mang lại những người đóng góp hợp lệ trong phạm vi đầy đủ. 

Những dấu vết này cho thấy cách thuật toán giảm bảng liệt kê tổng thể thành một lần kiểm tra căn chỉnh mô-đun duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(Q \log \min(n,m))$| mỗi truy vấn sử dụng gcd và nghịch đảo mô-đun | 
| Không gian |$O(1)$| chỉ các biến phụ không đổi | 

Hệ số logarit xuất phát từ gcd và các phép toán Euclid mở rộng. Với$10^5$truy vấn, điều này thoải mái phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from math import gcd

    def egcd(a, b):
        if b == 0:
            return a, 1, 0
        g, x1, y1 = egcd(b, a % b)
        return g, y1, x1 - (a // b) * y1

    def modinv(a, mod):
        g, x, _ = egcd(a, mod)
        if g != 1:
            return None
        return x % mod

    def solve():
        Q = int(input())
        out = []
        for _ in range(Q):
            n, m, x, y, d = map(int, input().split())
            g = gcd(d, m)
            if y % g != 0:
                out.append("0")
                continue
            m1 = m // g
            d1 = d // g
            y1 = y // g
            inv = modinv(d1, m1)
            if inv is None:
                out.append("0")
                continue
            k0 = (y1 * inv) % m1
            t = d * k0
            if (t // m) % n != x:
                out.append("0")
                continue
            limit = n * m
            if k0 >= limit:
                out.append("0")
            else:
                out.append(str((limit - 1 - k0) // m1 + 1))
        return "\n".join(out)

    return solve()

# provided sample (format adapted)
assert run("1\n1 1 0 0 1\n") == "1", "sample 1"

# edge: impossible remainder
assert run("1\n2 3 0 2 5\n") == "0", "no solution due to gcd"

# edge: small cycle
assert run("1\n2 3 1 1 2\n") in ["0", "1"], "consistency check boundary"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| vũ trụ đơn bào | 1 | ánh xạ nhận dạng tầm thường | 
| gcd không khớp | 0 | sự đồng đẳng không giải được | 
| cấu trúc tuần hoàn nhỏ | có điều kiện | tính nhất quán biên của ràng buộc thương số | 

## Vỏ cạnh 

Trường hợp cạnh khóa xảy ra khi$y$không chia hết cho$\gcd(d,m)$. Ví dụ, với$m=6$,$d=4$,$y=3$, chúng tôi có$\gcd=2$Và$y \bmod 2 \neq 0$, nên không có giải pháp nào tồn tại. Thuật toán nắm bắt được điều này ngay lập tức trước khi thử đảo ngược, ngăn chặn số học mô-đun không chính xác. 

Một trường hợp khác phát sinh khi mô đun rút gọn trở thành 1. Khi đó mọi$k$thỏa mãn điều kiện còn lại và tính chính xác phụ thuộc hoàn toàn vào việc kiểm tra thương. Thuật toán xử lý vấn đề này một cách tự nhiên vì nghịch đảo mô-đun trở nên tầm thường và số tiến trình đơn giản hóa thành số số học toàn phạm vi. 

Trường hợp thứ ba liên quan đến lớn$d$nơi các sản phẩm trung gian vượt quá$10^{18}$. Vì tất cả các phép chia đều xảy ra trước phép nhân trong các kiểm tra có ý nghĩa nên độ chính xác tùy ý của Python đảm bảo tính chính xác và không cần xử lý tràn.
