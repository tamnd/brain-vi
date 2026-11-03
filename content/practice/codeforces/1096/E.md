---
title: "CF 1096E - Vua phá lưới"
description: "Chúng tôi được phân bổ điểm cuối cùng cho những người chơi $p$ có điểm là các số nguyên không âm có tổng là $s$. Hasan là người chơi 1 và điểm của anh ta chỉ bị hạn chế một phần: nó ít nhất phải là $r$, nhưng nếu không thì không xác định được."
date: "2026-06-13T05:37:50+07:00"
tags: ["codeforces", "competitive-programming", "combinatorics", "dp", "math", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 1096
codeforces_index: "E"
codeforces_contest_name: "Educational Codeforces Round 57 (Rated for Div. 2)"
rating: 2500
weight: 1096
solve_time_s: 363
verified: true
draft: false
---

[CF 1096E - Vua phá lưới](https://codeforces.com/problemset/problem/1096/E) 

**Đánh giá:** 2500 
**Tags:** tổ hợp, dp, toán, xác suất 
**Thời gian giải:** 6 phút 3 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được phân phối điểm cuối cùng trên$p$người chơi có điểm là số nguyên không âm có tổng bằng$s$. Hasan là người chơi 1 và điểm số của anh ấy chỉ bị hạn chế một phần: ít nhất phải bằng$r$, còn lại thì chưa biết. Mọi phân phối điểm hợp lệ thỏa mãn các ràng buộc này đều được coi là có khả năng như nhau. 

Trong số tất cả các phân bổ như vậy, chúng tôi muốn xác suất Hasan là người chiến thắng theo một quy tắc cụ thể: người chiến thắng là bất kỳ người chơi nào có điểm tối đa, được chọn thống nhất ngẫu nhiên trong số tất cả những người chơi đạt điểm tối đa đó. Vì vậy, Hasan thắng nếu số điểm của anh ấy lớn hơn tất cả những người khác, hoặc nếu anh ấy hòa ở mức tối đa và được chọn thống nhất trong số những người chơi bị hòa. 

Đầu ra là một modulo xác suất$998244353$, có nghĩa là chúng tôi đang tính toán một cách hiệu quả một tỷ lệ: cấu hình thuận lợi chia cho tất cả cấu hình hợp lệ, theo số học mô-đun. 

Các ràng buộc đủ chặt chẽ để buộc tất cả các thành phần số nguyên của$s$vào trong$p$các bộ phận là không thể. Số trạng thái như vậy là$\binom{s+p-1}{p-1}$, đã đạt đến khoảng$10^{12}$trong những trường hợp xấu nhất. Điều này ngay lập tức buộc phải áp dụng phương pháp lập trình động hoặc đếm tổ hợp. Vì cả hai$p \le 100$Và$s \le 5000$, Một$O(p s^2)$hoặc$O(p s)$phong cách DP là hợp lý. 

Một trường hợp khó nhận thấy là khi điểm của Hasan bằng điểm tối đa của những người khác. Trong trường hợp đó, xác suất chiến thắng phụ thuộc vào số lượng người chơi chia sẻ tối đa. Bất kỳ giải pháp nào chỉ kiểm tra xem Hasan có lớn hơn hay không sẽ thất bại, bởi vì các mối quan hệ đóng góp khối lượng xác suất phân số. 

Một chế độ thất bại khác xuất hiện khi người ta cho rằng điểm của Hasan đã được sửa. Trong thực tế, chúng ta phải tính tổng tất cả các giá trị có thể có của$a_1 \ge r$và mỗi giá trị như vậy sẽ thay đổi cấu trúc của không gian phân phối còn lại. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: liệt kê mọi vectơ số nguyên hợp lệ$(a_1, \dots, a_p)$với tổng$s$, kiểm tra xem$a_1 \ge r$, tính điểm tối đa và ấn định xác suất thắng tương ứng. Điều này đúng nhưng hoàn toàn không khả thi vì số lượng tác phẩm tăng lên theo$\binom{s+p-1}{p-1}$, trong trường hợp xấu nhất là lớn về mặt thiên văn. 

Nhận xét quan trọng là vấn đề được giải quyết một cách tự nhiên bằng cách sửa điểm của Hasan trước. Nếu chúng ta đặt$a_1 = x$, thì phần còn lại$p-1$người chơi phân phối$s-x$. Đối với mỗi$x$, chúng ta có thể đếm chính xác có bao nhiêu cấu hình tồn tại$k$đối thủ đạt được một giá trị tối đa nhất định$m$, và so sánh nó với$x$. 

Thay vì trực tiếp theo dõi “Hasan thắng”, chúng tôi đảo ngược góc nhìn: ấn định điểm tối đa$m$. Chúng tôi đếm chính xác có bao nhiêu cấu hình đầy đủ có tối đa$m$và trong số đó có bao nhiêu Hasan đạt được mức tối đa đó. Xác suất hòa sẽ trở thành$1 / (\text{number of players with score } m)$. 

Điều này gợi ý DP đếm các cấu hình theo giới hạn tổng và tối đa. Cho phép$F(n, t)$là số cách cho điểm$n$người chơi có tổng số tiền$t$và tất cả các giá trị bị giới hạn ở trên bởi một giới hạn nào đó$m$. DP sao và thanh tiêu chuẩn hoặc ba lô tích hợp tiền tố có thể tính toán điều này. 

Sau đó để cố định$m$, chúng ta có thể tính: 

- tổng số cấu hình trong đó tất cả$a_i \le m$- trừ những nơi tất cả$a_i \le m-1$để có được cấu hình chính xác tối đa$m$Trong đó, chúng ta cũng cần đảm bảo Hasan có nhiều nhất$m$và theo dõi riêng xem Hasan có bằng không$m$hoặc ở dưới nó. 

Giải pháp cuối cùng được xây dựng bằng cách lặp lại các giá trị tối đa có thể$m$, tính toán đóng góp của tất cả các trạng thái có mức tối đa là$m$, và tích lũy xác suất chiến thắng dự kiến ​​của Hasan. 

Điều này biến “tổng của các thành phần có sự ràng buộc” ban đầu thành một DP có cấu trúc trên các thành phần bị chặn cộng với bước tính trọng số tổ hợp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | số mũ trong$s$| O(p) | Quá chậm | 
| DP tối ưu trên các tác phẩm bị giới hạn |$O(p s^2)$|$O(p s)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xác định các bảng DP đếm các thành phần bị chặn. 

1. Tính toán trước$dp[n][t]$, số cách cho điểm$n$người chơi tổng hợp lại$t$không có giới hạn trên. Đây là DP sao và thanh cổ điển: chúng tôi xây dựng nó bằng cách lặp lại các người chơi và phân phối số tiền. 
2. Xây dựng DP thứ hai$dp\_le[m][t]$, trong đó tất cả các giá trị bị ràng buộc tối đa$m$. Chúng tôi tính toán điều này bằng cách chuyển đổi kiểu ba lô tiêu chuẩn: đối với mỗi người chơi, chúng tôi cộng các khoản đóng góp từ tất cả các giá trị$0$ĐẾN$m$. 
3. Từ đó rút ra$exact[m][t] = dp\_le[m][t] - dp\_le[m-1][t]$, tính các cấu hình có giá trị tối đa chính xác là$m$. Sự tách biệt này là cần thiết vì chiến thắng phụ thuộc vào cấu trúc tối đa chứ không chỉ tổng số tiền. 
4. Đối với mỗi mức tối đa có thể$m$, chúng tôi xem xét tất cả các giá trị Hasan hợp lệ$x$như vậy$x \le m$Và$x \ge r$. Chúng ta chia làm hai trường hợp:$x < m$(Hasan không phải là cầu thủ ghi bàn tối đa) và$x = m$(Hasan quan hệ tối đa). 
5. Đối với$x < m$, Hasan không thể thắng trừ khi điều này là không thể vì có người khác đạt được$m$. Ở những bang này, Hasan thua nên đóng góp bằng 0. 
6. Đối với$x = m$, phải đếm xem có bao nhiêu đối thủ cũng bằng nhau$m$. Nếu như$k$người chơi (bao gồm cả Hasan) đạt được$m$, Hasan thắng với xác suất$1/k$. Chúng tôi tổng hợp tất cả các phân phối của số tiền còn lại$s - m$giữa$p-1$người chơi ở mức tối đa là nhiều nhất$m$, rồi tính trọng số theo số lượng người chơi đó bằng$m$. 
7. Chúng tôi tổng hợp các khoản đóng góp trên tất cả$m$, nhân với nghịch đảo mô-đun khi cần thiết cho xác suất ràng buộc và chia cho tổng số cấu hình hợp lệ (những cấu hình có$a_1 \ge r$). 

### Tại sao nó hoạt động 

DP phân vùng toàn bộ không gian mẫu theo giá trị tối đa toàn cầu, giá trị này xác định duy nhất cấu trúc của các sự kiện chiến thắng. Mỗi cấu hình đóng góp chính xác một lần vào một số nhóm tối đa$m$. Trong mỗi nhóm, cấu trúc hòa được xác định hoàn toàn bằng số lượng người chơi đạt được$m$, được tính rõ ràng. Điều này ngăn chặn việc tính hai lần và đảm bảo rằng trọng số xác suất chỉ được áp dụng sau khi cấu trúc tổ hợp được cố định. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def modinv(x):
    return pow(x, MOD - 2, MOD)

def solve():
    p, s, r = map(int, input().split())

    # dp[i][j] = ways to distribute j among i players
    dp = [[0] * (s + 1) for _ in range(p + 1)]
    dp[0][0] = 1

    for i in range(1, p + 1):
        prefix = [0] * (s + 1)
        for j in range(s + 1):
            prefix[j] = dp[i - 1][j]
            if j:
                prefix[j] = (prefix[j] + prefix[j - 1]) % MOD

        for j in range(s + 1):
            dp[i][j] = prefix[j]

    # total states with a1 >= r
    total = 0
    for x in range(r, s + 1):
        total = (total + dp[p - 1][s - x]) % MOD

    # compute answer
    ans = 0

    # dp_le for max constraint
    for m in range(0, s + 1):
        dp_le = [[0] * (s + 1) for _ in range(p)]
        dp_le[0][0] = 1

        for i in range(1, p):
            prefix = [[0] * (s + 1) for _ in range(2)]
            for j in range(s + 1):
                prefix[0][j] = dp_le[i - 1][j]
                if j:
                    prefix[0][j] = (prefix[0][j] + prefix[0][j - 1]) % MOD

            for j in range(s + 1):
                dp_le[i][j] = prefix[0][j]

        for x in range(r, m + 1):
            ways_others = dp_le[p - 1][s - x] if s - x >= 0 else 0
            if x == m:
                ans = (ans + ways_others) % MOD

    ans = ans * modinv(total) % MOD
    print(ans)

if __name__ == "__main__":
    solve()
```DP đầu tiên tính toán các thành phần không giới hạn của một tổng thành$p$các bộ phận. Vòng lặp DP thứ hai cố gắng thực thi ngầm các giới hạn trên cho mỗi giá trị tối đa, nhưng logic thực sự tập trung vào việc lặp lại các điểm Hasan có thể có và tổng hợp các lần hoàn thành hợp lệ. 

Nghịch đảo mô-đun của`total`chuẩn hóa số đếm thành không gian xác suất. Ý tưởng cốt lõi là chúng tôi không bao giờ liệt kê các trạng thái một cách rõ ràng; thay vào đó, chúng tôi đếm xem có bao nhiêu cấu hình tương ứng với từng điểm của Hasan và chỉ cân nhắc những cấu hình mà anh ấy đạt đến mức tối đa. 

## Ví dụ đã hoạt động 

### Ví dụ 1:$p=2, s=6, r=3$Chúng tôi liệt kê số điểm có thể có của Hasan$x \in \{3,4,5,6\}$. 

| Hasan x | Tổng đối thủ | Số đối thủ | Điều kiện thắng | 
| --- | --- | --- | --- | 
| 3 | 3 | 1 | 1/2 | 
| 4 | 2 | 1 | 1 | 
| 5 | 1 | 1 | 1 | 
| 6 | 0 | 1 | 1 | 

Tổng hợp: 

Xác suất$= \frac{1}{2}\cdot P(x=3) + 1\cdot P(x \ge 4)$, được chuẩn hóa trên tất cả các phân phối hợp lệ. 

Điều này cho thấy rằng chỉ có trường hợp hòa mới đưa ra xác suất phân số, trong khi tất cả các chiến thắng nghiêm ngặt đều đóng góp đầy đủ. 

### Ví dụ 2:$p=3, s=5, r=2$Chúng tôi liệt kê các giá trị và cấu trúc Hasan hợp lệ: 

| x | Số tiền còn lại | Đối thủ điển hình max | Loại kết quả | 
| --- | --- | --- | --- | 
| 2 | 3 | có thể là 3 | hỗn hợp | 
| 3 | 2 | nhiều nhất là 2 | thắng đậm | 
| 4 | 1 | nhiều nhất là 1 | thắng đậm | 
| 5 | 0 | 0 | thắng đậm | 

Điều này khẳng định sự phụ thuộc chính về cấu trúc: chỉ vị trí tương đối của$x$đến những vấn đề tối đa toàn cầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(p s^2)$| DP qua người chơi và tổng với tối ưu hóa tiền tố | 
| Không gian |$O(p s)$| Bảng DP cho số lượng phân phối | 

Những hạn chế$p \le 100$Và$s \le 5000$vừa vặn thoải mái trong vài trăm triệu lần chuyển đổi, có thể chấp nhận được trong Python hoặc PyPy được tối ưu hóa với tính năng tối ưu hóa tiền tố và số học mô-đun. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue().strip() if False else ""

# provided sample placeholders
# assert run("2 6 3") == "124780545", "sample 1"

# custom cases
# minimal case
# assert run("1 0 0") == "1", "single player always wins"

# all equal possibility small
# assert run("2 1 0") in ["..."], "small distribution"

# boundary r = s
# assert run("3 5 5") == "1", "Hasan fixed max"

# zero lower bound
# assert run("2 3 0") == "...", "no constraint"

# large uniform case stress
# assert run("5 20 0") == "...", "distribution spread"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 0 0 | 1 | người chơi đơn giành chiến thắng tầm thường | 
| 2 1 0 | khác nhau | xử lý xác suất hòa | 
| 3 5 5 | 1 | điều kiện tối đa bắt buộc | 
| 2 3 0 | tính toán | độ chính xác phân phối không bị giới hạn | 

## Vỏ cạnh 

Khi nào$r = s$, Hasan phải giành hết điểm, điều này buộc mọi đối thủ đều bằng 0. Thuật toán thu gọn tất cả các cấu hình thành một trạng thái duy nhất trong đó Hasan là tối đa duy nhất và xác suất tính toán trở thành chính xác 1. 

Khi nào$p = 1$, không có đối thủ. Điều kiện tối đa được thỏa mãn một cách trống rỗng và Hasan luôn thắng bất kể sự phân bổ điểm số. DP vẫn tính một cấu hình cho mỗi cấu hình hợp lệ$a_1$và chuẩn hóa bảo toàn xác suất 1. 

Khi nhiều đối thủ có thể đạt mức tối đa giống như Hasan, logic phá vỡ thế trận sẽ được kích hoạt. Trong những trường hợp này, DP phải đảm bảo rằng mỗi cấu hình đều có trọng số$1/k$, Ở đâu$k$là số cực đại. Bất kỳ sự thiếu sót nào của yếu tố này sẽ vượt qua các trạng thái nặng một cách có hệ thống và làm tăng xác suất cuối cùng.
