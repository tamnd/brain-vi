---
title: "CF 102309G - Trò chơi của gấu trúc Orz"
description: "Chúng tôi có n phòng được sắp xếp từ trái sang phải. Mỗi phòng chứa các đống đá và một bản ghi đầu vào được nhóm (p, q, c) có nghĩa là phòng q chứa c các cọc riêng biệt có kích thước là p. Một truy vấn đưa ra một khoảng [l, r]."
date: "2026-08-13T06:59:59+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102309
codeforces_index: "G"
codeforces_contest_name: "The 2019 \u201cOrz Panda\u201d Cup Programming Contest"
rating: 0
weight: 102309
solve_time_s: 789
verified: true
draft: false
---

[CF 102309G - Trò chơi của gấu trúc Orz](https://codeforces.com/problemset/problem/102309/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 13m 9s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

chúng tôi có`n`phòng được sắp xếp từ trái qua phải. Mỗi phòng chứa các đống đá và một bản ghi đầu vào được nhóm lại`(p, q, c)`nghĩa là căn phòng đó`q`chứa`c`cọc riêng biệt có kích thước là`p`. Một truy vấn đưa ra một khoảng`[l, r]`. Từ mỗi phòng trong khoảng này, qkoqhh có thể không chọn cọc hoặc chọn chính xác một trong các cọc trong phòng đó. 

Ngoài ra còn có thêm một đống kích thước`x`, được tạo trước khi trò chơi bắt đầu. Sau khi thực hiện tất cả các lựa chọn, các cọc được chọn cùng với cọc bổ sung này tạo thành vị trí Nim. Người chơi đầu tiên thắng chính xác khi xor của tất cả các kích thước cọc khác 0, do đó qkoqhh thắng chính xác khi xor của cọc phòng đã chọn là`x`. Câu trả lời cho một truy vấn là số cách khác nhau để thực hiện những lựa chọn này, được tính theo modulo`10007`. 

Sự đa dạng rất quan trọng. Nếu một phòng có ba đống kích thước khác nhau`5`, chọn một đống có kích thước`5`đưa ra ba lựa chọn khác nhau. Một căn phòng chứa`c`đống có kích thước bằng 0 đóng góp`c`các cách khác nhau để giữ cho xor không thay đổi, ngoài việc lựa chọn không chọn gì. 

Các ràng buộc làm cho cấu trúc khá cụ thể. chỉ có`n <= 500`phòng, và mọi kích thước cọc và`x`nhiều nhất là`500`, vì vậy mọi kích thước cọc đều phù hợp với trạng thái xor 9 bit. Như vậy chỉ có`2^9 = 512`giá trị xor có thể. Mặt khác,`m`có thể`10000`và một bản ghi có thể mô tả`10000`cọc vật lý, do đó việc mở rộng rõ ràng tất cả các cọc có thể tạo ra tới`10^8`đồ vật. Thuật toán phải tổng hợp các kích thước cọc bằng nhau thay vì lặp lại trên các cọc riêng lẻ. 

Có thể có nhiều như`n(n+1)/2 = 125250`truy vấn. Một phương pháp chi tiêu`O(n)`hoặc nhiều công việc hơn cho mỗi truy vấn đã có khoảng hàng chục triệu thao tác, do đó kích thước xor cố định nhỏ của`512`phải được khai thác. Mục tiêu hữu ích là khoảng`O((n+Q) * 512)`sau khi tiền xử lý. 

Một sức mạnh vũ phu trực tiếp đối với các lựa chọn là hoàn toàn vô vọng. Ngay cả khi chỉ có một đống khác 0 trong mỗi`500`phòng, có`2^500`các lựa chọn có thể, khoảng`3.27 * 10^150`. Nếu mọi lựa chọn quét tất cả các phòng, thì đó là khoảng`500 * 2^500`quyết định về phòng. 

Một số trường hợp phức tạp có thể âm thầm phá vỡ quá trình triển khai đơn giản hơn. Đầu tiên, một cọc có kích thước bằng 0 không thay đổi giá trị xor, nhưng việc chọn các cọc có kích thước bằng 0 khác nhau vẫn cho ra những lựa chọn khác nhau. Ví dụ, với`n=1`,`x=0`, và một đống có kích thước bằng 0 trong phòng`1`, truy vấn`[1,1]`có câu trả lời`2`: không chọn gì hoặc chọn cọc số 0. Việc triển khai bỏ qua các cọc có kích thước bằng 0 sẽ trả về`1`. 

Thứ hai, kích thước trùng lặp phải duy trì tính đa dạng của chúng. Vì`n=1`,`x=1`, và hai đống có kích thước khác nhau`1`trong phòng`1`, truy vấn`[1,1]`có câu trả lời`2`, bởi vì cọc vật lý tạo ra xor`1`. Coi hai cọc là một giá trị sẽ trả về`1`. 

Thứ ba, việc phân chia theo mô-đun không thể áp dụng một cách mù quáng cho các sản phẩm tiền tố. Giá trị phòng được chuyển đổi có thể là`0`modulo`10007`. Nếu một phòng như vậy nằm trong khoảng truy vấn thì tích khoảng được chuyển đổi tương ứng bằng 0. Việc chia hai tích có tiền tố 0 sẽ làm mất thông tin này và có thể tạo ra giá trị khác 0 không chính xác. Giải pháp tối ưu ghi lại rõ ràng hệ số 0 cuối cùng cho mỗi tọa độ được chuyển đổi. 

## Phương pháp tiếp cận 

Giải pháp brute-force sẽ liệt kê mọi lựa chọn cọc hợp lệ, tính toán xor của các cọc được chọn, xor nó với`x`và đếm các lựa chọn có xor cuối cùng bằng 0. Điều này đúng vì Nim đang thua chính xác khi cọc xor của nó bằng 0. Vấn đề là số lượng lựa chọn. Ngay cả trường hợp nhỏ hơn nhiều là mỗi phòng một đống cũng đã tạo ra`2^500`các lựa chọn và đầu vào thực tế có thể chứa tối đa`10^8`cọc vật chất thông qua bội số. 

Một cải tiến lập trình động tự nhiên là giữ`dp[s]`, số cách để có được xor`s`sau khi xử lý một số phòng. Đối với căn phòng có cọc có kích thước`p`với bội số`c[p]`, sự chuyển tiếp là`new[s] = dp[s] + sum(c[p] * dp[s xor p])`. 

Điều này đúng và đã loại bỏ sự phụ thuộc vào số lượng cọc vật lý. Tuy nhiên, việc thực hiện quá trình chuyển đổi này trực tiếp tốn kém`O(512 * number_of_distinct_sizes)`mỗi phòng. Quan trọng hơn, việc trả lời từng khoảng một cách độc lập vẫn yêu cầu xử lý tất cả các phòng trong khoảng đó. 

Quan sát quan trọng là quá trình chuyển đổi là tích chập XOR. Xác định vectơ phòng`f`bằng cách thiết lập`f[0]`ĐẾN`1`để không chọn gì và sau đó cộng bội số của mọi kích thước cọc vào vị trí tương ứng của nó. Khoảng DP chính xác là tích chập XOR của tất cả các vectơ phòng trong khoảng. 

Phép biến đổi Walsh-Hadamard chéo hóa tích chập XOR. Sau khi biến đổi một vectơ, tích chập XOR trở thành phép nhân theo điểm thông thường. Đây cũng chính là lý do Biến đổi Walsh-Hadamard nhanh thường được sử dụng cho tích chập XOR. 

Đối với tọa độ được chuyển đổi`k`, cho phép`a_q[k]`là giá trị biến đổi của phòng`q`. Giá trị được chuyển đổi cho một khoảng`[l,r]`đơn giản là`a_l[k] * a_{l+1}[k] * ... * a_r[k]`. 

Bây giờ bài toán khoảng đã trở thành bài toán tích phạm vi cho`512`chuỗi vô hướng độc lập. 

Một sản phẩm tiền tố thường cho phép chúng tôi có được một sản phẩm phạm vi bằng cách chia. Có một điều phức tạp: một hệ số có thể bằng 0 modulo`10007`. Chúng tôi giải quyết nó bằng cách duy trì hai phần thông tin cho mỗi tọa độ được chuyển đổi. Đầu tiên là tích của tất cả các thừa số khác 0 được thấy cho đến nay. Thứ hai là vị trí của hệ số 0 mới nhất. Nếu số 0 mới nhất nằm bên trong`[l,r]`, tích khoảng bằng không. Mặt khác, tích khoảng là thương của hai tích có tiền tố khác 0. 

Cuối cùng, phép biến đổi Walsh-Hadamard nghịch đảo cho biết số lượng lựa chọn cho mỗi giá trị xor. Ta chỉ cần hệ số tương ứng với`x`, vì vậy chúng ta có thể tính trực tiếp hệ số đó bằng cách sử dụng công thức biến đổi nghịch đảo thay vì chuyển đổi ngược lại toàn bộ khoảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(n * 2^n)`thậm chí với một đống mỗi phòng |`O(n)`| Quá chậm | 
| Xor DP trực tiếp trên mỗi khoảng thời gian |`O(Q * n * 512)`|`O(512)`| Quá chậm | 
| Sản phẩm tối ưu, WHT + |`O(n * 512 * log 512 + (n + Q) * 512)`|`O(n * 512)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Cho mỗi phòng`q`, xây dựng một mảng`f_q`chiều dài`512`. Chức vụ`0`bắt đầu bằng giá trị`1`, đại diện cho sự lựa chọn không chọn gì cả. Đối với mỗi bản ghi đầu vào`(p,q,c)`, thêm vào`c`ĐẾN`f_q[p]`. Nếu như`p=0`, điều này có nghĩa`f_q[0]`trở thành`1+c`, bởi vì việc chọn bất kỳ một trong các cọc số 0 sẽ làm cho xor không thay đổi nhưng đưa ra một lựa chọn khác biệt. 
2. Áp dụng phép biến đổi Walsh-Hadamard cho mọi vectơ phòng. Vectơ biến đổi`A_q`có`512`tọa độ. Tại tọa độ`k`nó đại diện`A_q[k] = sum_v f_q[v] * (-1)^{popcount(k & v)}`. 

Lý do chọn phép biến đổi này là tích chập XOR giữa các phân bố lựa chọn phòng trở thành tọa độ nhân thông thường với tọa độ. 
3. Với mọi tọa độ được chuyển đổi`k`, quét các phòng từ trái sang phải và xây dựng một sản phẩm tiền tố chỉ chứa các thừa số khác 0. Cho phép`pref[k][i]`là sản phẩm của`A_1[k] ... A_i[k]`sau khi bỏ qua mọi yếu tố bằng 0. Cũng lưu trữ`last_zero[k][i]`, chỉ số phòng lớn nhất nhiều nhất`i`Ở đâu`A_q[k]`là số không. 
4. Đối với một truy vấn`[l,r]`và tọa độ cố định`k`, kiểm tra đầu tiên`last_zero[k][r]`. Nếu ít nhất là`l`, thì một thừa số bên trong khoảng bằng 0, do đó tích của khoảng được biến đổi bằng 0. Ngược lại mọi thừa số trong khoảng đều khác 0 và tích của nó là`pref[k][r] / pref[k][l-1]`. 

Vì mô đun`10007`là số nguyên tố, mọi thặng dư khác 0 đều có nghịch đảo mô đun. Việc triển khai sẽ tính toán trước nghịch đảo của mọi phần dư khác 0 một lần, do đó, mỗi tích trong phạm vi sẽ thu được trong thời gian không đổi. 
5. Phép biến đổi Walsh-Hadamard nghịch đảo có công thức đặc biệt thuận tiện. Nếu như`P[k]`là tích khoảng được chuyển đổi, khi đó số lượng lựa chọn có xor là`x`là`answer = inv(512) * sum_k P[k] * (-1)^{popcount(k & x)}`. 

Tính toán trước dấu này cho mọi`k`, sau đó đánh giá công thức cho mọi truy vấn. 
6. Xử lý tất cả các truy vấn bằng cách sử dụng cùng một dữ liệu tiền tố được chuyển đổi. Không cần phải xây dựng lại xor DP cho mỗi khoảng thời gian. Mỗi truy vấn thực hiện`512`tra cứu phạm vi sản phẩm vô hướng độc lập và một lần giảm mô-đun cuối cùng. 

Tại sao nó hoạt động 

Đối với mỗi phòng,`f_q[v]`chính xác là số cách để khiến phòng đóng góp xor`v`. Kết hợp hai phòng có nghĩa là chọn một đóng góp từ mỗi phòng, do đó phân phối kết quả của chúng là tích chập XOR của các vectơ của chúng. Phép biến đổi Walsh-Hadamard chuyển đổi tích chập này thành phép nhân theo tọa độ, do đó vectơ biến đổi của một khoảng chính xác là tích của các phép biến đổi phòng của nó. 

Cấu trúc tiền tố trả về kết quả đó một cách chính xác cho mọi tọa độ. Nếu hệ số 0 xuất hiện bên trong khoảng, vị trí số 0 cuối cùng được lưu trữ sẽ phát hiện nó và trả về 0. Nếu không có số 0 xảy ra thì cả hai tích tiền tố đều khác 0, do đó thương của chúng chính xác là modulo tích khoảng mong muốn.`10007`. 

Công thức Walsh-Hadamard nghịch đảo sau đó rút ra hệ số cho xor`x`. Hệ số đó tính chính xác các lựa chọn có cọc được chọn xor tới`x`, có nghĩa là xor của chúng cùng với đống ban đầu`x`là số không. Vị trí Nim như vậy đang thua đối với người chơi đầu tiên, vì vậy mọi lựa chọn được tính chính xác là lựa chọn chiến thắng cho qkoqhh. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10007
S = 512
INV_S = pow(S, MOD - 2, MOD)

# Modular inverses of every nonzero residue modulo MOD.
INV = [0] * MOD
INV[1] = 1
for i in range(2, MOD):
    INV[i] = MOD - (MOD // i) * INV[MOD % i] % MOD

def fwht(a):
    """Walsh-Hadamard transform for XOR convolution."""
    h = 1
    while h < S:
        step = h << 1
        for base in range(0, S, step):
            end = base + h
            j = base
            while j < end:
                u = a[j]
                v = a[j + h]

                x = u + v
                if x >= MOD:
                    x -= MOD

                y = u - v
                if y < 0:
                    y += MOD

                a[j] = x
                a[j + h] = y
                j += 1
        h <<= 1

def solve():
    out = []

    while True:
        line = input()
        if not line:
            break
        if not line.strip():
            continue

        n, x = map(int, line.split())

        m = int(input())

        rooms = [[0] * S for _ in range(n)]

        for _ in range(m):
            p, q, c = map(int, input().split())
            rooms[q - 1][p] = (rooms[q - 1][p] + c) % MOD

        # Choosing nothing is always one possibility.
        for q in range(n):
            rooms[q][0] = (rooms[q][0] + 1) % MOD

        # Transform every room.
        for q in range(n):
            fwht(rooms[q])

        # pref[k][i] = product of all nonzero transformed factors
        # among rooms 1..i at coordinate k.
        pref = [[1] * (n + 1) for _ in range(S)]

        # last_zero[k][i] = latest room <= i whose transformed
        # value at coordinate k is zero.
        last_zero = [[0] * (n + 1) for _ in range(S)]

        for k in range(S):
            p = 1
            z = 0

            prow = pref[k]
            zrow = last_zero[k]

            for i in range(1, n + 1):
                value = rooms[i - 1][k]

                if value == 0:
                    z = i
                else:
                    p = (p * value) % MOD

                prow[i] = p
                zrow[i] = z

        qn = int(input())
        queries = []

        for _ in range(qn):
            l, r = map(int, input().split())
            queries.append((l, r))

        # The inverse transform coefficient for xor x uses
        # the character (-1)^(popcount(k & x)).
        signs = [1] * S
        for k in range(S):
            if (k & x).bit_count() & 1:
                signs[k] = -1

        answers = [0] * qn

        # Process one transformed coordinate at a time.
        # This keeps the prefix rows local and avoids repeated
        # two-dimensional indexing in the hottest loop.
        for k in range(S):
            prow = pref[k]
            zrow = last_zero[k]
            sign = signs[k]

            for qi, (l, r) in enumerate(queries):
                if zrow[r] >= l:
                    continue

                # Both prefix values are nonzero, so division is valid.
                value = prow[r] * INV[prow[l - 1]]

                if sign == 1:
                    answers[qi] += value
                else:
                    answers[qi] -= value

        for value in answers:
            value %= MOD
            value = value * INV_S % MOD
            out.append(str(value))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Đầu vào đầu tiên được tổng hợp trực tiếp vào`rooms[q][p]`, do đó việc triển khai không bao giờ mở rộng được số lượng cọc vật lý khổng lồ tiềm tàng. phần bổ sung`1`được thêm vào ở vị trí 0 thể hiện việc không chọn gì từ căn phòng đó. 

Phép biến đổi Walsh-Hadamard sử dụng phép toán bướm tiêu chuẩn`u+v`Và`u-v`. Giá trị được giảm modulo`10007`sau mỗi con bướm, điều này giữ cho mọi hệ số được lưu trữ ở mức nhỏ. Độ dài biến đổi chính xác là`512`bởi vì tất cả các kích thước cọc đều ở bên dưới`512`. 

Việc xây dựng tiền tố cố tình bỏ qua các hệ số biến đổi bằng 0 thay vì nhân chúng thành tiền tố. Đây chính là điều khiến cho sự phân chia có thể xảy ra sau này.`last_zero`mang thông tin còn thiếu cần thiết để phân biệt khoảng chứa thừa số 0 với khoảng có tích khác 0 có cùng giá trị tiền tố. 

Bảng nghịch đảo`INV`tránh gọi lũy thừa mô-đun cho mọi truy vấn và tần số. Vì các tích tiền tố không bao giờ bằng 0,`INV[prow[l - 1]]`luôn luôn hợp lệ. biểu hiện`prow[r] * INV[prow[l - 1]]`được để lại dưới dạng số nguyên Python thông thường cho đến khi giảm truy vấn cuối cùng. Điều này loại bỏ thao tác modulo tốn kém khỏi vòng lặp nóng nhất. 

Vòng lặp truy vấn chạy trên các tọa độ đã được chuyển đổi thay vì xây dựng lại một mảng cho mỗi truy vấn. Dấu được xác định bởi tính chẵn lẻ của các bit được chia sẻ bởi`k`Và`x`, khớp chính xác với ký tự Walsh-Hadamard nghịch đảo cho giá trị xor`x`. 

Không có vấn đề tràn số nguyên trong Python. Trong ngôn ngữ có chiều rộng cố định, tích trung gian ở đây cũng nhỏ vì cả hai toán hạng đều nằm dưới`10007`, nhưng Python tự nhiên xử lý số tiền tạm thời lớn hơn được tích lũy qua`512`tọa độ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào mô tả ba phòng với`x=1`. Phòng 1 có hai đống kích thước khác nhau`1`. Phòng 2 và 3 mỗi phòng chứa một đống kích thước`2`. 

Đối với phòng 1, phân bố lựa chọn là`f=[1,2]`trên các giá trị xor`0`Và`1`. Đối với phòng 2 thì là`f=[1,0,1]`và phòng 3 có cùng phân bố. 

Bảng sau đây hiển thị trạng thái xor DP tương đương cho các giá trị xor thấp có liên quan. Đây là trạng thái tương tự được biểu thị bằng phép biến đổi Walsh-Hadamard nghịch đảo, nhưng dễ kiểm tra trực tiếp hơn. 

| Phòng đã xử lý |`dp[0]`|`dp[1]`|`dp[2]`|`dp[3]`| 
| --- | --- | --- | --- | --- | 
| Không có | 1 | 0 | 0 | 0 | 
| Phòng 1 | 1 | 2 | 0 | 0 | 
| Phòng 1..2 | 1 | 2 | 1 | 2 | 
| Phòng 1..3 | 2 | 4 | 2 | 4 | 

Đối với truy vấn`[1,1]`, xor mong muốn là`x=1`, vậy câu trả lời là`dp[1]=2`. Vì`[1,2]`, nó vẫn còn`2`. Vì`[1,3]`, nó trở thành`4`. Do đó đầu ra là`2, 2, 4`. 

Truy vấn thứ ba giải thích lý do tại sao các lựa chọn khác nhau trong các phòng khác nhau lại kết hợp thông qua xor. Để có được xor`1`, qkoqhh chọn một trong hai cọc cỡ 1 ở phòng 1, còn phòng 2 và 3 đều phải được bỏ qua hoặc cả hai đều đóng góp cọc cỡ 2 của mình. Điều đó mang lại`2 * 2 = 4`sự lựa chọn. 

### Mẫu 2 

đây`x=0`. Phòng 1 có hai đống kích thước khác nhau`1`, trong khi phòng 2 chứa hai đống có kích thước khác nhau`2`. Truy vấn bao gồm cả hai phòng. 

| Phòng đã xử lý |`dp[0]`|`dp[1]`|`dp[2]`|`dp[3]`| 
| --- | --- | --- | --- | --- | 
| Không có | 1 | 0 | 0 | 0 | 
| Phòng 1 | 1 | 2 | 0 | 0 | 
| Phòng 1..2 | 1 | 2 | 2 | 4 | 

Câu trả lời là hệ số cho xor`x=0`, cụ thể là`dp[0]=1`. Sự lựa chọn chiến thắng duy nhất là không chọn gì cả. Chọn một cọc cỡ 1 sẽ cho xor`1`, chọn một cọc cỡ 2 sẽ cho xor`2`và chọn một từ cả hai phòng sẽ cho xor`3`. 

Ví dụ này cũng xác nhận rằng bội số được tính chính xác. Các giá trị`2`TRONG`dp[1]`Và`dp[2]`đến từ hai đống vật lý riêng biệt trong các phòng tương ứng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n * 512 * log 512 + (n + Q) * 512 + m)`| Mỗi phòng được chuyển đổi một lần, tiền tố được xây dựng cho tất cả 512 tọa độ và mỗi truy vấn sẽ kiểm tra 512 tọa độ | 
| Không gian |`O(n * 512 + Q)`| Biến đổi phòng, sản phẩm tiền tố, vị trí 0 và truy vấn được lưu trữ | 

Với`n <= 500`Và`Q <= 125250`, giai đoạn truy vấn thực hiện tối đa khoảng`64`triệu phép tính tọa độ đơn giản. Kích thước xor được cố định tại`512`, và việc biến đổi phòng chỉ yêu cầu`9`lớp bướm. Thuật toán cũng tránh việc lưu trữ các chồng vật lý, điều này cần thiết vì bội số có thể biểu diễn nhiều đối tượng hơn nhiều so với`m`gợi ý. 

Trang vấn đề chính thức cũng đưa ra điều tương tự`n <= 500`,`m <= 10000`, Và`Q <= n(n+1)/2`giới hạn. 

## Trường hợp thử nghiệm 

Dây nịt sau đây giả định`solve()`chức năng từ giải pháp trên có sẵn từ một tệp có tên`solution.py`.```python
import sys
import io

from solution import solve

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided samples
sample = """\
3 1
3
1 1 2
2 2 1
2 3 1
3
1 1
1 2
1 3
3 0
3
1 1 1
1 1 1
2 2 2
1
1 3
"""

assert run(sample) == "2\n2\n4\n1", "provided samples"

# Minimum-size input, nonzero pile.
assert run("""\
1 1
1
1 1 1
1
1 1
""") == "1", "minimum-size nonzero case"

# Zero-sized pile must count as a separate choice.
assert run("""\
1 0
1
0 1 1
1
1 1
""") == "2", "zero pile multiplicity"

# Duplicate piles in one room are distinct choices.
assert run("""\
1 1
1
1 1 2
1
1 1
""") == "2", "duplicate physical piles"

# Boundary intervals and xor values.
assert run("""\
3 3
3
1 1 1
2 2 1
4 3 1
3
1 2
2 3
1 3
""") == "1\n0\n1", "interval boundaries"

# Maximum n, with a large multiplicity.
assert run("""\
500 0
1
0 500 10000
1
1 500
""") == "10001", "maximum room count"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`, một cọc cỡ 1 |`1`| Đầu vào tối thiểu và điều kiện Nim xor cơ bản | 
|`1 0`, một cọc cỡ 0 |`2`| Chọn cọc số 0 khác với chọn không chọn gì | 
|`1 1`, hai cọc cỡ 1 |`2`| Kích thước cọc bằng nhau vẫn là sự lựa chọn khác biệt | 
| Ba phòng có kích thước`1,2,4`|`1, 0, 1`| Ranh giới khoảng trái và phải | 
|`n=500`, mười nghìn không cọc |`10001`| Lớn`n`, bội số lớn và tập hợp an toàn theo modulo | 

## Vỏ cạnh 

Một phòng chứa các cọc có kích thước bằng 0 sẽ được xử lý khi vectơ của nó được khởi tạo. Giả sử đầu vào là```
1 0
1
0 1 1
1
1 1
```Phòng 1 ban đầu có lựa chọn trống, góp phần một chiều vào xor 0. Đống có kích thước bằng 0 duy nhất thêm một cách khác vào xor 0, vì vậy`f[0]=2`. Do đó, mọi tọa độ được biến đổi sẽ được nhân với`2`. Hệ số xor-zero cuối cùng là`2`, đó chính xác là số lượng các lựa chọn. Việc thực hiện bất cẩn bắt đầu bằng`f[0]=1`nhưng loại bỏ hồ sơ với`p=0`sẽ sản xuất không chính xác`1`. 

Các cọc trùng lặp được xử lý thông qua việc cộng vào cùng một`f[p]`lối vào. Vì```
1 1
1
1 1 2
1
1 1
```vectơ phòng có`f[0]=1`Và`f[1]=2`. Có ba lựa chọn phòng hợp pháp: không chọn gì, chọn cọc cỡ 1 đầu tiên hoặc chọn cọc cỡ 1 thứ hai. Chỉ có hai cái cuối cùng cho xor`1`, vậy câu trả lời là`2`. Thuật toán không bao giờ cần phân biệt hai cọc bên trong vectơ biến đổi vì bội số của chúng đã được biểu thị bằng hệ số`2`. 

Trường hợp ranh giới khoảng là```
3 3
3
1 1 1
2 2 1
4 3 1
3
1 2
2 3
1 3
```Vì`[1,2]`, các giá trị khác 0 có sẵn là`1`Và`2`và các xor có thể có của chúng là`0,1,2,3`, vậy xin chào`3`xảy ra một lần bằng cách chọn cả hai cọc. Câu trả lời là`1`. Vì`[2,3]`, các xor có thể là`0,2,4,6`, vậy xin chào`3`không bao giờ xảy ra và câu trả lời là`0`. Vì`[1,3]`, việc chọn cọc cỡ 1 và cỡ 2 sẽ cho xor`3`, trong khi cọc cỡ 4 không thể là một phần của tổ hợp khác tạo ra`3`, vậy câu trả lời lại là`1`. Việc lập chỉ mục tiền tố sử dụng`pref[l-1]`, đó chính xác là điều ngăn cản điểm cuối bên trái vô tình bị loại trừ hoặc bao gồm hai lần. 

Trường hợp hệ số biến đổi bằng 0 được xử lý độc lập với kích thước cọc. Đối với một số tần số`k`, giả sử các giá trị phòng được chuyển đổi trong một khoảng thời gian là`5,0,7`. Sản phẩm biến đổi đúng là bằng không. Cấu trúc tiền tố lưu trữ tích của các thừa số khác 0 dưới dạng`35`và ghi lại vị trí của số 0. Một truy vấn bao gồm số 0 nhìn thấy`last_zero[k][r] >= l`và ngay lập tức đóng góp bằng không. Một truy vấn bắt đầu sau số 0 đó sẽ không thấy số 0 nào trong khoảng của nó và phân chia các sản phẩm có tiền tố khác 0 một cách an toàn. Đây là lý do việc triển khai không chỉ đơn giản sử dụng phép chia tiền tố thông thường. 

Cuối cùng, đống ban đầu bổ sung không được coi là một căn phòng khác. Vai trò duy nhất của nó là thay đổi xor đích từ`0`ĐẾN`x`. Một tập hợp cọc phòng được chọn sẽ giành được qkoqhh chính xác khi xor của nó bằng`x`, vì khi đó tổng xor bao gồm cả cọc ban đầu là`x xor x = 0`. Đây là lý do tại sao phép trích biến đổi ngược cuối cùng sử dụng tọa độ`x`thay vì phối hợp`0`.
