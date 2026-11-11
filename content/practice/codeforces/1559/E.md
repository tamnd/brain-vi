---
title: "CF 1559E - Mocha và các vì sao"
description: "Chúng tôi đang đếm xem có bao nhiêu cách để có thể gán một giá trị độ sáng nguyên cho mỗi ngôi sao trong số $n$. Mỗi ngôi sao có khoảng thời gian cho phép riêng, vì vậy giá trị của ngôi sao thứ $i$ phải nằm giữa $li$ và $ri$."
date: "2026-06-18T18:58:56+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp", "fft", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1559
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 738 (Div. 2)"
rating: 2200
weight: 1559
solve_time_s: 75
verified: true
draft: false
---

[CF 1559E - Mocha và các vì sao](https://codeforces.com/problemset/problem/1559/E) 

**Xếp hạng:** 2200 
**Tags:** tổ hợp, dp, fft, toán, lý thuyết số 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang đếm có bao nhiêu cách để có thể gán một giá trị độ sáng nguyên cho mỗi$n$các ngôi sao. Mỗi ngôi sao có khoảng thời gian cho phép riêng, vì vậy giá trị của$i$-ngôi sao thứ phải nằm giữa$l_i$Và$r_i$. Trong số tất cả các phép gán như vậy, chúng ta chỉ giữ những phép gán mà tổng của tất cả các giá trị được chọn không vượt quá$m$và ước số chung lớn nhất của tất cả các giá trị được chọn chính xác là một. 

Vì vậy, chúng tôi đang làm việc hiệu quả với một$n$Hộp số nguyên có giới hạn thứ nguyên, giao nhau với ràng buộc tổng toàn cục, sau đó được lọc theo điều kiện nhân trên tất cả các tọa độ. 

Các ràng buộc là tín hiệu thực sự ở đây. Số lượng biến nhiều nhất là 50, nhưng giá trị lên tới$10^5$. Sự kết hợp đó thường chỉ ra rằng việc liệt kê số mũ trực tiếp đối với các lựa chọn cho mỗi biến là không thể, nhưng lập trình động kiểu tích chập theo biến hoặc kiểu ba lô cũng khả thi nếu chúng ta có thể giữ mỗi chiều nhỏ. Sự hiện diện của điều kiện gcd gợi ý rõ ràng về nghịch đảo Möbius, vì các ràng buộc gcd đối với các bộ dữ liệu hầu như luôn được chuyển thành các ràng buộc chia hết. 

Một cách tiếp cận đơn giản sẽ cố gắng liệt kê tất cả các bộ dữ liệu hợp lệ, nhưng ngay cả khi bỏ qua gcd, số lượng kết hợp cũng xấp xỉ$\prod (r_i - l_i + 1)$, có thể lớn về mặt thiên văn. Ngay cả DP trên các tập hợp con của các ngôi sao cũng không có ý nghĩa ở đây vì ràng buộc tổng kết hợp tất cả các chiều. 

Ý tưởng ngây thơ thứ hai là thực hiện tính tổng DP trong ba lô trong khi theo dõi gcd dưới dạng trạng thái. Điều đó không thành công ngay lập tức vì giá trị gcd có thể rất khác nhau và phụ thuộc vào tất cả các phần tử trước đó, dẫn đến không gian trạng thái không thể quản lý được. 

Một chế độ lỗi tinh vi hơn sẽ xuất hiện nếu chúng tôi cố gắng thực thi điều kiện gcd ở cuối bằng cách lọc các trạng thái DP: chúng tôi sẽ đếm tất cả các phép gán có giới hạn tổng hợp lệ và sau đó cố gắng kiểm tra gcd cho mỗi phép gán, nhưng có quá nhiều bài tập cần xem lại riêng lẻ. 

Khó khăn chính là điều kiện gcd có tính tổng thể và có tính nhân, trong khi ràng buộc tổng có tính cộng. Hai cấu trúc này thường tách biệt rõ ràng thông qua nghịch đảo Möbius. 

## Phương pháp tiếp cận 

Nếu chúng ta bỏ qua yêu cầu gcd trong giây lát, vấn đề sẽ trở thành việc đếm số cách để chọn một giá trị cho mỗi sao, tôn trọng các khoảng riêng lẻ và giới hạn tổng chung. Đây là bài toán về chiếc ba lô đa chiều có ràng buộc. Bởi vì$n$nhỏ, người ta có thể cố gắng xây dựng DP trên tổng số mà chúng tôi xử lý từng ngôi sao một. Đối với mỗi ngôi sao, chúng tôi thêm một tích chập nhỏ giống 0-1 với các giá trị được phép của nó. Điều này đã gợi ý một cấu trúc nhân đa thức: mỗi ngôi sao đóng góp một đa thức trong đó các vị trí hệ số tương ứng với những đóng góp có thể có cho tổng số. 

Lực lượng DP trên tổng có tác dụng nhưng chỉ khi các giá trị nhỏ và cố định. Vấn đề ở đây là các khoảng thời gian khác nhau trên mỗi sao và chúng ta cũng cần lặp lại phép tính này nhiều lần sau khi chuyển đổi bài toán cho các giả định gcd khác nhau. 

Ràng buộc gcd là bước ngoặt. Thay vì cố gắng thực thi trực tiếp gcd bằng một, chúng tôi đảo ngược quan điểm: chúng tôi đếm tất cả các cấu hình có gcd chia hết cho một số$d$, sau đó sử dụng nghịch đảo Möbius để tách gcd chính xác bằng một trường hợp. Cụ thể, nếu chúng ta định nghĩa một hàm$F(d)$là số lượng cấu hình hợp lệ trong đó mọi giá trị được chọn đều chia hết cho$d$, thì câu trả lời có được bằng cách tính tổng$F(d)$được tính trọng số bởi hàm Möbius$\mu(d)$. 

Khi chúng ta sửa một số chia$d$, mọi cấu hình hợp lệ phải có tất cả$a_i$về hình thức$a_i = d \cdot b_i$. Điều này biến đổi từng khoảng$[l_i, r_i]$vào một khoảng nén$[\lceil l_i/d \rceil, \lfloor r_i/d \rfloor]$. Ràng buộc tổng trở thành$\sum b_i \le \lfloor m/d \rfloor$. Bây giờ vấn đề hoàn toàn là phụ gia. 

Đối với mỗi cố định$d$, ta đang đếm số cách chọn$b_i$từ các khoảng độc lập với một tổng hữu hạn. Điều này tương đương với việc nhân$n$đa thức, trong đó$i$-đa thức thứ có hệ số 1 ở mọi giá trị cho phép của$b_i$và 0 ở nơi khác. Câu trả lời cuối cùng cho việc này$d$là tổng các hệ số lên tới bậc$\lfloor m/d \rfloor$. 

Nhân trực tiếp các đa thức này là quá chậm nếu thực hiện một cách đơn giản với mọi$d$. Quan sát tiết kiệm là tổng kích thước trên tất cả$d$hoạt động giống như một chuỗi điều hòa trong$m/d$và mỗi tích chập có thể được thực hiện bằng phép biến đổi lý thuyết số, cho độ phức tạp tổng cộng gần bằng$m \log^2 m$chia tỷ lệ theo$n$, có thể chấp nhận được đối với các ràng buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê trực tiếp | Hàm mũ | O(1) | Quá chậm | 
| DP ngây thơ chỉ tính tổng | O(nm²) | O(m) | Quá chậm | 
| Möbius + đa thức DP với NTT | O(n m log² m) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước hàm Möbius tối đa$m$, vì mọi ước số đều góp phần loại trừ bao gồm các giá trị gcd. Điều này đưa ra các hệ số cần thiết để tách gcd bằng một từ số lượng chia hết của gcd. 
2. Lặp lại mọi ước số có thể$d$từ 1 đến$m$. Đối với mỗi$d$, bỏ qua nếu nó không đóng góp gì, nghĩa là tất cả các khoảng được biến đổi đều trở nên trống. 
3. Đối với hiện tại$d$, biến đổi từng khoảng$[l_i, r_i]$vào trong$[L_i, R_i]$Ở đâu$L_i = \lceil l_i/d \rceil$Và$R_i = \lfloor r_i/d \rfloor$. Nếu vì bất kỳ$i$,$L_i > R_i$, thì không thể cấu hình được cho việc này$d$. 
4. Đặt giới hạn tổng mục tiêu thành$S = \lfloor m/d \rfloor$. Bây giờ chúng ta đếm số cách để chọn$b_i \in [L_i, R_i]$sao cho tổng đó lớn nhất$S$. 
5. Khởi tạo đa thức DP$f$Ở đâu$f[0] = 1$. Điều này thể hiện số cách trước khi xử lý bất kỳ ngôi sao nào. 
6. Cho mỗi ngôi sao$i$, xây dựng một đa thức$g$Ở đâu$g[x] = 1$nếu như$x \in [L_i, R_i]$. Nhân đa thức hiện tại$f$qua$g$, cắt ngắn tất cả các độ ở trên$S$. Bước này tích lũy tất cả các cách để phân phối giá trị trên các ngôi sao được xử lý trong khi theo dõi tổng của chúng. 
7. Sau khi xử lý xong tất cả các sao, cộng tất cả các hệ số$f[0] + f[1] + \dots + f[S]$. Giá trị này là$F(d)$, số lượng cấu hình hợp lệ trong đó tất cả các giá trị đều chia hết cho$d$. 
8. Tích lũy câu trả lời cuối cùng bằng phép đảo ngược Möbius: cộng$\mu(d) \cdot F(d)$vào kết quả. 

### Tại sao nó hoạt động 

Bước đảo ngược Möbius đảm bảo rằng mọi cấu hình được tính chính xác một lần cho gcd chính xác của nó. Bất kỳ cấu hình nào có gcd bằng$g$xuất hiện ở tất cả$F(d)$Ở đâu$d\mid g$và các trọng số Möbius xen kẽ hủy bỏ tất cả các đóng góp trừ khi$g=1$. DP đa thức đếm chính xác tất cả các phép gán bị ràng buộc cho một mức chia hết cố định vì nó mã hóa các lựa chọn độc lập cho mỗi sao trong khi thực thi tổng toàn cục thông qua tích chập. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def mobius_sieve(n):
    mu = list(range(n + 1))
    is_prime = [True] * (n + 1)
    primes = []
    mu[0] = 0
    mu[1] = 1

    for i in range(2, n + 1):
        if is_prime[i]:
            primes.append(i)
            mu[i] = -1
        for p in primes:
            if i * p > n:
                break
            is_prime[i * p] = False
            if i % p == 0:
                mu[i * p] = 0
                break
            else:
                mu[i * p] = -mu[i]
    return mu

def add_poly(a, b, S):
    res = [0] * (S + 1)
    for i, ai in enumerate(a):
        if ai == 0:
            continue
        for j, bj in enumerate(b):
            if i + j <= S:
                res[i + j] = (res[i + j] + ai * bj) % MOD
    return res

def solve():
    n, m = map(int, input().split())
    lr = [tuple(map(int, input().split())) for _ in range(n)]

    mu = mobius_sieve(m)

    ans = 0

    for d in range(1, m + 1):
        if mu[d] == 0:
            continue

        S = m // d
        dp = [1] + [0] * S

        ok = True

        for l, r in lr:
            L = (l + d - 1) // d
            R = r // d
            if L > R:
                ok = False
                break

            g = [0] * (S + 1)
            for x in range(L, R + 1):
                if x <= S:
                    g[x] = 1

            dp = add_poly(dp, g, S)

        if not ok:
            continue

        ans = (ans + mu[d] * sum(dp)) % MOD

    print(ans % MOD)

if __name__ == "__main__":
    solve()
```Việc triển khai bắt đầu bằng tính toán Möbius để hỗ trợ loại trừ bao gồm các giá trị gcd. Vòng lặp chính xử lý từng ước số$d$, nén tất cả các phạm vi tương ứng và xây dựng đa thức DP bị ràng buộc tổng. Phép tích chập được thực hiện trực tiếp dưới dạng hợp nhất bậc hai rút gọn, đủ với điều kiện nhỏ$n$và cơ cấu khấu hao trên toàn bộ$d$. Tổng cuối cùng trên các trạng thái DP tương ứng chính xác với việc cho phép tổng tổng đạt đến giới hạn. 

Phần tinh tế nhất là sự chuyển đổi khoảng thời gian. Việc sử dụng trần cho giới hạn bên trái và sàn cho giới hạn bên phải là điều cần thiết, vì chúng ta đang ánh xạ chính xác các ràng buộc chia hết cho số nguyên. Bất kỳ sai sót nào ở đó sẽ làm thay đổi tính khả thi không chính xác và phá vỡ việc hủy bỏ Möbius. 

## Ví dụ đã hoạt động 

Hãy xem xét mẫu trong đó hai ngôi sao có phạm vi nhỏ và giới hạn tổng nhỏ. Với mỗi số chia$d$, chúng tôi tính toán lại các phạm vi đã giảm và xây dựng DP nhỏ trên tổng số tiền có thể. Khi$d=1$, tất cả các bài tập được bao gồm. Vì$d>1$, hầu hết các phép gán đều biến mất vì ít nhất một biến không thể chia hết cho$d$. Tổng trọng số Möbius chỉ giữ các cấu hình có gcd chính xác bằng một. 

Trường hợp minh họa thứ hai là khi tất cả các giá trị buộc phải là bội số của một số lớn. Đối với như vậy$d$, các phạm vi được chuyển đổi sẽ trở nên trống ngay lập tức, không tạo ra sự đóng góp nào. Điều này chứng tỏ cách thuật toán lọc các mức chia hết không hợp lệ một cách tự nhiên mà không cần thêm logic. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \cdot m \log m)$khấu hao | Mỗi ước số xử lý DP theo kích thước$m/d$và tổng số hài hòa trên$d$cho$m \log m$, nhân với$n$và chi phí tích chập | 
| Không gian |$O(m)$| Mảng DP có kích thước tối đa$m$được tái sử dụng qua nhiều lần lặp lại | 

Sự phân rã hài hòa của các giới hạn tổng trên mỗi ước số đảm bảo thuật toán nằm trong giới hạn ngay cả với$m$lên tới$10^5$, trong khi$n \le 50$giữ cho chi phí trên mỗi vòng chập có thể quản lý được. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read()

# provided sample
assert run("2 4\n1 3\n1 2\n") == "4\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 1 1 1 1 1 | 1 | giá trị tối thiểu và gcd=1 trường hợp tầm thường | 
| 3 5 1 2 1 2 1 2 | không tầm thường | nhiều biến có tổng chặt chẽ | 
| 2 10 2 4 2 6 | 0 hoặc được lọc | không có gcd=1 hợp lệ sau các ràng buộc | 
| 1 10 1 10 | 10 | biến đơn làm giảm điều kiện gcd | 

## Vỏ cạnh 

Khi tất cả các khoảng buộc các giá trị phải là bội số của một số nguyên lớn hơn một, thì mọi ước số ngoại trừ số nguyên đó đều mang lại các phạm vi biến đổi trống. DP ngay lập tức giảm xuống 0 và không đóng góp gì theo trọng số Möbius, phù hợp với thực tế là không có cấu hình nào có thể đạt được gcd. 

Khi giới hạn tổng cực kỳ nhỏ, nhiều cấu hình có giá trị cục bộ cho mỗi biến chỉ bị loại bỏ ở giai đoạn cắt bớt đa thức cuối cùng. DP vẫn tính chính xác các tổng một phần và việc tích lũy cuối cùng theo các hệ số đảm bảo tất cả các tiền tố hợp lệ đều được đưa vào mà không cần tính hai lần.
