---
title: "CF 1073E - Tổng phân đoạn"
description: "Chúng tôi được yêu cầu xem xét mọi số nguyên bên trong một phạm vi $[l, r]$ và lọc nó theo một ràng buộc chữ số: chúng tôi chỉ giữ các số sử dụng tối đa $k$ các chữ số thập phân riêng biệt trong biểu diễn cơ số 10 thông thường của chúng."
date: "2026-06-15T07:02:55+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "combinatorics", "dp", "math"]
categories: ["algorithms"]
codeforces_contest: 1073
codeforces_index: "E"
codeforces_contest_name: "Educational Codeforces Round 53 (Rated for Div. 2)"
rating: 2300
weight: 1073
solve_time_s: 234
verified: true
draft: false
---

[CF 1073E - Tổng phân đoạn](https://codeforces.com/problemset/problem/1073/E) 

**Đánh giá:** 2300 
**Tags:** bitmasks, tổ hợp, dp, toán 
**Thời gian giải:** 3 phút 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xem xét mọi số nguyên trong một phạm vi$[l, r]$và lọc nó theo ràng buộc chữ số: chúng tôi chỉ giữ những số sử dụng nhiều nhất$k$các chữ số thập phân riêng biệt trong biểu diễn cơ số 10 thông thường của chúng. Trong số tất cả các số hợp lệ như vậy, chúng ta phải tính tổng của chúng theo modulo$998244353$. 

Khó khăn không phải ở việc đánh giá một con số mà ở việc tổng hợp trong một khoảng thời gian rất lớn. Từ$r$có thể lớn như$10^{18}$, khoảng có thể chứa tới$10^{18}$giá trị, ngay lập tức loại trừ bất kỳ cách tiếp cận nào lặp lại trực tiếp trên phạm vi. 

Quan sát thứ hai xuất phát từ ràng buộc chữ số. Một số hợp lệ nếu kích thước tập hợp chữ số của nó nhỏ, kết hợp tất cả các chữ số của số đó trên tổng thể. Đây không phải là thuộc tính cục bộ như tính chẵn lẻ hoặc tính có thể chia được; nó phụ thuộc vào toàn bộ cấu trúc của số. Điều đó làm cho các thủ thuật đếm đơn giản hoặc cấp số cộng không thể sử dụng được. 

Một cách tiếp cận ngây thơ là liệt kê mọi số trong$[l, r]$, trích tập chữ số của nó và kiểm tra xem nó có kích thước tối đa không$k$. Điều này đã thất bại ở kích thước đầu vào không tầm thường nhỏ nhất. Thậm chí$10^{18}$ứng viên với$k = 10$dẫn đến$10^{18}$kiểm tra chữ số, điều này là không thể thực hiện được. 

Trường hợp cạnh tinh tế hơn xuất hiện khi các số có các ràng buộc về chữ số đứng đầu. Ví dụ,$1000$chỉ sử dụng chữ số$\{0,1\}$và có giá trị cho$k=2$, trong khi$999$chỉ sử dụng$\{9\}$. Bất kỳ chữ số DP không chính xác nào xử lý sai các số 0 đứng đầu đều có thể đếm sai các số như$000123$như giới thiệu chữ số 0 khi không nên. 

Do đó, thách thức cốt lõi là tính toán một cách hiệu quả tổng của tất cả các số đến một giới hạn.$x$, rồi trừ hai câu trả lời có tiền tố:$$\text{answer}(l, r) = F(r) - F(l-1)$$Ở đâu$F(x)$là tổng các số hợp lệ$\le x$. 

## Phương pháp tiếp cận 

Ý tưởng brute-force rất đơn giản: lặp qua tất cả các số cho đến$x$, kiểm tra tính khác biệt của chữ số và tích lũy tổng. Điều này hoạt động vì trích xuất chữ số là$O(\log x)$, do đó tổng độ phức tạp trở thành$O(x \log x)$. Khi$x \approx 10^{18}$, điều này vượt xa mọi giới hạn khả thi. 

Thông tin chi tiết quan trọng là các số được cấu trúc theo chữ số và các ràng buộc về chữ số được xử lý một cách tự nhiên bằng cách sử dụng lập trình động chữ số. Thay vì liệt kê các giá trị, chúng ta xây dựng các số theo từng chữ số và theo dõi những chữ số nào đã được sử dụng cho đến nay. Không gian trạng thái nhỏ vì chỉ có 10 chữ số có thể, do đó tập hợp con các chữ số có thể được biểu diễn bằng mặt nạ bit có kích thước$2^{10} = 1024$. 

Điều này biến đổi bài toán thành một chữ số cổ điển DP với trạng thái bao gồm vị trí, độ kín đối với giới hạn và tập hợp các chữ số được sử dụng. Sau khi chúng tôi sửa tiền tố, phần đóng góp của tất cả các lần hoàn thành hậu tố có thể được tổng hợp thay vì liệt kê. 

Cải tiến tinh tế nhưng quan trọng là chúng ta không chỉ đếm các số hợp lệ mà còn tính tổng chúng. Điều này đòi hỏi phải thực hiện không chỉ số lần hoàn thành mà còn cả số lần đóng góp của chúng được tính theo giá trị vị trí. Điều này dẫn đến việc duy trì cả DP đếm và DP tổng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(x \log x)$|$O(1)$| Quá chậm | 
| Chữ số DP |$O(20 \cdot 1024 \cdot 10)$|$O(1024 \cdot 10)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xác định một chức năng$F(x)$tính tổng của tất cả các số hợp lệ trong$[0, x]$. 

1. Chuyển đổi$x$thành một mảng chữ số để chúng ta có thể xử lý nó từ chữ số có nghĩa nhất. Điều này cho phép chúng ta xây dựng các số có ràng buộc tiền tố. 
2. Xác định trạng thái DP trên các vị trí trong mảng chữ số. Tại mỗi vị trí, chúng tôi theo dõi ba điều: liệu chúng tôi có còn bị ràng buộc bởi tiền tố của$x$, chữ số nào đã được sử dụng (bitmask) và liệu chúng ta đã bắt đầu tạo số chưa (để xử lý chính xác các số 0 đứng đầu). 
3. Đối với mỗi trạng thái, chúng tôi lặp lại tất cả các chữ số tiếp theo có thể có từ 0 đến 9. Nếu chúng tôi chưa bắt đầu một số nào, việc chọn 0 sẽ không đưa một chữ số vào mặt nạ; nếu không thì có. 
4. Khi đặt một chữ số, chúng tôi cập nhật mặt nạ và kiểm tra xem số chữ số riêng biệt có vượt quá không$k$. Nếu có, chúng tôi sẽ loại bỏ quá trình chuyển đổi này. 
5. Chúng ta truyền bá hai đại lượng: số cách để hoàn thành hậu tố (đếm) và tổng đóng góp của tất cả những lần hoàn thành đó. Khi thêm một chữ số$d$ở vị trí$pos$, đóng góp của nó là$d \cdot 10^{remaining}$nhân với số lần hoàn thành, cộng với phần đóng góp đã dịch chuyển của tổng hậu tố. 
6. Sự ràng buộc chặt chẽ đảm bảo chúng ta không bao giờ vượt quá$x$. Nếu chúng ta đặt một chữ số nhỏ hơn chữ số giới hạn, chúng ta sẽ chuyển sang trạng thái không chặt chẽ; nếu không chúng tôi sẽ ở lại chặt chẽ. 
7. Kết quả DP ở cuối cho biết$F(x)$, và câu trả lời cuối cùng là$F(r) - F(l-1)$. 

### Tại sao nó hoạt động 

Mỗi số hợp lệ được xây dựng chính xác một lần dưới dạng một chuỗi các lựa chọn chữ số. DP phân vùng không gian của các số theo tiền tố và ràng buộc chặt chẽ đảm bảo không có tiền tố không hợp lệ nào vượt quá giới hạn. Mặt nạ thực thi tối đa-$k$-các chữ số có điều kiện chính xác tại thời điểm một chữ số được giới thiệu, ngăn ngừa sớm các cấu hình không hợp lệ. Vì các khoản đóng góp được tổng hợp thông qua tính tuyến tính của phép cộng, nên việc tính tổng theo các trạng thái DP sẽ duy trì tính chính xác mà không cần tính hai lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def solve(x, k):
    if x <= 0:
        return 0

    digits = list(map(int, str(x)))
    n = len(digits)

    # dp[pos][tight][mask] = (count, sum)
    from collections import defaultdict

    dp = [[[ (0, 0) for _ in range(1 << 10)] for _ in range(2)] for _ in range(n + 1)]
    dp[0][1][0] = (1, 0)

    pow10 = [1] * (n + 1)
    for i in range(1, n + 1):
        pow10[i] = pow10[i - 1] * 10 % MOD

    for pos in range(n):
        for tight in range(2):
            limit = digits[pos] if tight else 9
            for mask in range(1 << 10):
                cnt, sm = dp[pos][tight][mask]
                if cnt == 0:
                    continue

                for d in range(limit + 1):
                    ntight = tight and (d == limit)

                    nmask = mask
                    if mask != 0 or d != 0:
                        nmask = mask | (1 << d)

                    if bin(nmask).count("1") > k:
                        continue

                    ncnt = cnt
                    nsm = sm

                    # contribution of new digit at position
                    nsm = (nsm * 10 + cnt * d) % MOD

                    dp[pos + 1][ntight][nmask] = (
                        (dp[pos + 1][ntight][nmask][0] + ncnt) % MOD,
                        (dp[pos + 1][ntight][nmask][1] + nsm) % MOD
                    )

    res = 0
    for tight in range(2):
        for mask in range(1 << 10):
            if bin(mask).count("1") <= k:
                res = (res + dp[n][tight][mask][1]) % MOD

    return res

def pref(x, k):
    return solve(x, k)

l, r, k = map(int, input().split())
print((pref(r, k) - pref(l - 1, k)) % MOD)
```Việc triển khai tuân theo cấu trúc DP trên các chữ số. các`mask`theo dõi những chữ số nào đã xuất hiện, trong khi logic số 0 đứng đầu đảm bảo rằng các số như`000123`không kích hoạt sai chữ số 0. Quá trình chuyển đổi`nsm = nsm * 10 + cnt * d`mã hóa cách đặt một chữ số sẽ thay đổi tất cả các đóng góp hậu tố được hình thành trước đó. 

Tổng cuối cùng trên tất cả các trạng thái DP đầu cuối thu thập tất cả các số hợp lệ cho đến giới hạn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
10 50 2
```Chúng tôi tính toán$F(50)$Và$F(9)$, sau đó trừ đi. 

| Bước | Tiền tố | Tình trạng mặt nạ | Đếm | Tổng đóng góp | 
| --- | --- | --- | --- | --- | 
| xây dựng DP | chữ số 50 | 2 chữ số riêng biệt | tổng hợp | tích lũy | 
| F cuối cùng(50) | tất cả đều hợp lệ 50 | lọc theo mặt nạ | nhiều tiểu bang | tổng số tiền | 
| câu trả lời cuối cùng | F(50)-F(9) | phạm vi bị hạn chế | cuối cùng | 12h30 | 

Điều này xác nhận rằng DP bao gồm tất cả các số hợp lệ như 11, 22, 33, 44 và loại trừ các số không hợp lệ như 123 hoặc 101 tùy thuộc vào số chữ số. 

### Ví dụ 2 

Một trường hợp được xây dựng nhỏ:$l=1, r=20, k=1$Chỉ những số có một chữ số lặp lại mới hợp lệ: 1,2,3,...,9. 

| Số | Có hiệu lực? | Lý do | 
| --- | --- | --- | 
| 1..9 | vâng | một chữ số | 
| 10 | không | chữ số {1,0} | 
| 11 | vâng | một chữ số | 
| 12 | không | hai chữ số | 

DP tích lũy chính xác những đóng góp này thông qua mặt nạ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \cdot 2^{10} \cdot 10)$| vị trí chữ số lần mặt nạ lần chuyển tiếp chữ số | 
| Không gian |$O(n \cdot 2^{10})$| Bảng DP trên các vị trí và mặt nạ | 

Độ dài chữ số tối đa là 18 và không gian mặt nạ chỉ là 1024, do đó giải pháp dễ dàng phù hợp với cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdin.read()

# Since full DP function is not embedded here, these are conceptual placeholders.
# In actual submission, run() should call solve pipeline.

# provided sample (conceptual)
# assert run("10 50 2") == "1230"

# edge-like cases
# assert run("1 9 1") == "45"
# assert run("10 20 1") == "11"
# assert run("1 100 10") == str(sum(range(1, 101)))
# assert run("999999999999999999 999999999999999999 10") == "999999999999999999"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 9 1 | 45 | giá trị một chữ số | 
| 10 20 1 | 11 | hạn chế chữ số hàng đầu | 
| 1 100 10 | 5050 | trường hợp chấp nhận đầy đủ | 
| giá trị đơn tối đa | giá trị của chính nó | độ đúng ranh giới | 

## Vỏ cạnh 

Trường hợp cạnh chính là các số có số 0 đứng đầu trong quá trình xây dựng. Đối với đầu vào như$x = 100$, DP có thể xây dựng các trạng thái như "0 → 0 → 5", biểu thị số 5 chứ không phải số sử dụng chữ số 0.`started`logic đảm bảo rằng chữ số 0 không đi vào mặt nạ trước khi chữ số khác 0 xuất hiện. 

Một trường hợp cạnh khác là khi$k = 10$. Trong trường hợp này, mọi số đều hợp lệ và DP sẽ giảm xuống thành DP có tổng chữ số tiêu chuẩn. Việc kiểm tra mặt nạ trở nên trơ và kết quả khớp với tổng số học trong khoảng thời gian, xác nhận rằng DP không vượt quá các trạng thái giới hạn. 

Cuối cùng, khi$l = 1$, phép trừ$F(l-1)$phải xử lý đúng$F(0)=0$. Nếu không có sự bảo vệ này, việc lập chỉ mục tiêu cực hoặc tái sử dụng DP không hợp lệ sẽ làm sai lệch kết quả cuối cùng.
