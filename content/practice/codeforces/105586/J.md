---
title: "CF 105586J - \u597d\u5f97\u4e0d\u80fd\u518d\u597d\u4e86\uff01\u6cf0\u62c9\u6295\u8d44\u5927\u5e08\u8bfe"
description: "Chúng ta được giao một thử nghiệm lặp lại trong đó mỗi lần thử độc lập đều dẫn đến thắng với xác suất $frac{p}{q}$ và thua với xác suất $1 - frac{p}{q}$. Chúng tôi bắt đầu từ 0 thắng và 0 thua, và chúng tôi tiếp tục chơi cho đến khi đạt được một trong hai điều kiện dừng."
date: "2026-06-22T06:01:04+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105586
codeforces_index: "J"
codeforces_contest_name: "\u201c\u534e\u4e3a\u676f\u201d 2024 \u5e74\u5e7f\u4e1c\u5de5\u4e1a\u5927\u5b66 ACM \u65b0\u751f\u7a0b\u5e8f\u8bbe\u8ba1\u7ade\u8d5b\uff08\u51b3\u8d5b\uff09"
rating: 0
weight: 105586
solve_time_s: 55
verified: true
draft: false
---

[CF 105586J - \u597d\u5f97\u4e0d\u80fd\u518d\u597d\u4e86\uff01\u6cf0\u62c9\u6295\u8d44\u5927\u5e08\u8bfe](https://codeforces.com/problemset/problem/105586/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 55s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được đưa ra một thử nghiệm lặp đi lặp lại trong đó mỗi thử nghiệm độc lập đều mang lại kết quả thắng lợi với xác suất$\frac{p}{q}$và thua lỗ có xác suất$1 - \frac{p}{q}$. Chúng tôi bắt đầu từ 0 thắng và 0 thua, và chúng tôi tiếp tục chơi cho đến khi đạt được một trong hai điều kiện dừng. Quá trình dừng thành công nếu chúng tôi nhận được$n$thắng trước khi chúng ta tích lũy$n$tổn thất. Nó thất bại nếu tổn thất đạt đến$n$Đầu tiên. 

Nhiệm vụ là tính xác suất thành công theo quy tắc dừng này và xuất nó theo modulo$10^9 + 7$. 

Một cách hữu ích để diễn giải lại quá trình này là thực hiện một bước đi một chiều. Mỗi trận thắng giúp chúng tôi +1, mỗi trận thua khiến chúng tôi -1. Chúng ta bắt đầu ở vị trí 0. Quá trình kết thúc khi chúng ta nhấn một trong hai$+n$(thành công) hoặc$-n$(sự thất bại). Câu hỏi trở thành: xác suất để một bước đi ngẫu nhiên có sai lệch xảy ra là bao nhiêu?$+n$trước$-n$? 

Các ràng buộc cho phép lên đến$10^4$trường hợp thử nghiệm và tổng số$5 \cdot 10^6$trên tất cả$n$. Điều này loại trừ mọi mô phỏng hoặc DP trên tất cả các trạng thái cho mỗi trường hợp thử nghiệm. Bất kỳ giải pháp nào cũng phải giảm từng trường hợp thử nghiệm thành lũy thừa mô-đun logarit hoặc theo thời gian không đổi. 

Một cách tiếp cận đơn giản mô phỏng việc đi bộ sẽ đòi hỏi thời gian kỳ vọng theo cấp số nhân trước khi hấp thụ và ngay lập tức là không thể thực hiện được. Ngay cả một giải pháp lập trình động trên các trạng thái từ$-n$ĐẾN$n$mỗi trường hợp thử nghiệm sẽ là$O(n^2)$hoặc tốt nhất$O(n)$, quá chậm đối với các ràng buộc toàn cầu. 

Trường hợp cạnh tinh tế xuất hiện khi$\frac{p}{q} = \frac{1}{2}$. Trong trường hợp đối xứng này, bước đi ngẫu nhiên không có độ lệch và tính đối xứng ngụ ý xác suất đạt được$+n$trước$-n$chính xác là$\frac{1}{2}$, độc lập với$n$. Việc triển khai dựa trên công thức phân chia một cách mù quáng cho$1 - r^{2n}$vẫn hoạt động nhưng có thể bị mất ổn định mô-đun nếu không được xử lý cẩn thận. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là mô hình hóa quy trình như một chuỗi Markov trên các trạng thái biểu thị điểm ròng hiện tại và tính toán xác suất hấp thụ bằng cách sử dụng lập trình động. Cho phép$dp[i]$là xác suất cuối cùng đạt được$+n$bắt đầu từ trạng thái$i$. Chúng ta sẽ đặt điều kiện biên$dp[n] = 1$Và$dp[-n] = 0$, rồi giải phương trình truy hồi tuyến tính. Tuy nhiên, mỗi trạng thái phụ thuộc vào hai trạng thái lân cận và việc giải hệ thống này cho tất cả các trạng thái đòi hỏi phải thực hiện lặp đi lặp lại hoặc loại bỏ Gaussian trên hệ thống ba đường chéo. Ngay cả khi tối ưu hóa, việc thực hiện việc này một cách độc lập cho từng trường hợp thử nghiệm là quá chậm so với tổng thể.$n$lên đến$5 \cdot 10^6$. 

Nhận xét quan trọng là đây là bài toán phá hoại của một tay cờ bạc cổ điển với các ranh giới hấp thụ đối xứng. Các quy trình như vậy thừa nhận một giải pháp dạng đóng chỉ phụ thuộc vào tỷ lệ giữa các xác suất của bước chứ không phụ thuộc vào cấu trúc tuyệt đối của biểu đồ DP. Khi chúng ta biểu diễn hệ thống theo tỷ lệ$r = \frac{q-p}{p}$, xác suất sẽ chuyển thành một biểu thức hình học đơn giản rút ra từ việc giải hệ thức truy hồi. 

Điều này làm giảm mỗi trường hợp thử nghiệm thành việc tính toán một số lũy thừa mô-đun và nghịch đảo mô-đun. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| DP qua các tiểu bang |$O(n)$mỗi bài kiểm tra |$O(n)$| Quá chậm | 
| Hình thức đóng tối ưu |$O(\log n)$mỗi bài kiểm tra |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi mô hình xác suất thành một bước đi ngẫu nhiên có độ lệch với các ranh giới hấp thụ và sau đó đánh giá dạng đóng đã biết. 

1. Chuyển xác suất thành công sang dạng số học môđun. Xác suất thắng trong một bước là$\frac{p}{q}$, vì vậy chúng tôi làm việc với các phép nghịch đảo mô-đun để biểu diễn các phân số dưới$10^9 + 7$. 
2. Xác định xác suất thất bại là$\frac{q-p}{q}$. Điều này cho phép chúng tôi thể hiện sự thiên vị của bước đi bằng cách sử dụng tỷ lệ thất bại và thành công. 
3. Xây dựng hệ số trôi$r = \frac{q-p}{p}$. Tỷ lệ này cho thấy khả năng chúng ta di chuyển xuống dưới nhiều hơn là đi lên trong một bước, không phụ thuộc vào mẫu số chung$q$. 
4. Tính toán$r^n$Và$r^{2n}$sử dụng lũy ​​thừa nhanh theo mô đun. Các giá trị này tương ứng với sự đóng góp theo trọng số xác suất của các đường dẫn đạt đến ranh giới hấp thụ. 
5. Nếu$r = 1$, xảy ra khi$q = 2p$, xuất trực tiếp$\frac{1}{2}$modulo$10^9 + 7$, vì bước đi là đối xứng. 
6. Mặt khác, tính xác suất cuối cùng bằng dạng đóng:$$\frac{1 - r^n}{1 - r^{2n}}$$theo số học mô-đun, thay thế phép chia bằng phép nhân bằng nghịch đảo mô-đun. 

Tính đúng đắn đến từ việc giải quyết phép truy toán xác suất hấp thụ trong bước đi ngẫu nhiên có giới hạn. Hàm xác suất thỏa mãn phép truy hồi tuyến tính bậc hai với hệ số không đổi. Lời giải tổng quát của nó là sự kết hợp tuyến tính của các lũy thừa của$r$và các điều kiện biên tại$-n$Và$+n$xác định duy nhất các hệ số, thu được chính xác công thức đã nêu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def mod_pow(a, e):
    res = 1
    while e:
        if e & 1:
            res = res * a % MOD
        a = a * a % MOD
        e >>= 1
    return res

def solve():
    T = int(input())
    inv2 = (MOD + 1) // 2

    for _ in range(T):
        n, p, q = map(int, input().split())

        if 2 * p == q:
            print(inv2)
            continue

        # r = (q-p)/p
        r = (q - p) * pow(p, MOD - 2, MOD) % MOD

        rn = mod_pow(r, n)
        r2n = rn * rn % MOD

        numerator = (1 - rn) % MOD
        denominator = (1 - r2n) % MOD

        ans = numerator * pow(denominator, MOD - 2, MOD) % MOD
        print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ chuyển đổi xác suất thành tỷ lệ loại bỏ mẫu số chung$q$. Điều này quan trọng vì việc giữ$q$bên trong phép lũy thừa sẽ không cần thiết và chậm hơn. 

Phép lũy thừa nhanh được sử dụng để$r^n$bởi vì$n$có thể lớn như$10^5$, và có nhiều trường hợp thử nghiệm. Nghịch đảo mô-đun chỉ được áp dụng ở bước chia cuối cùng, tránh việc đảo ngược lặp lại. 

Trường hợp đối xứng được xử lý riêng để tránh chia cho 0 trong biểu thức$1 - r^{2n}$, nếu không thì sẽ trở thành 0 khi$r = 1$. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp đơn giản$n = 1, p = 1, q = 2$. Đây$r = 1$, nên ta lập tức rơi vào nhánh đối xứng. Câu trả lời là$\frac{1}{2}$. 

| Bước | r | r^n | r^{2n} | Kết quả | 
| --- | --- | --- | --- | --- | 
| ban đầu | 1 | 1 | 1 | trường hợp đối xứng | 

Điều này khẳng định rằng khi khả năng thành công và thất bại ngang nhau thì bước đi đầu tiên sẽ quyết định tất cả. 

Bây giờ hãy xem xét$n = 2, p = 1, q = 3$. Khi đó xác suất thành công là$1/3$, thất bại là$2/3$, Vì thế$r = 2$. 

| Bước | r | r^2 | r^4 | tử số | mẫu số | 
| --- | --- | --- | --- | --- | --- | 
| tính toán | 2 | 4 | 16 |$1-4$|$1-16$| 

Xác suất cuối cùng trở thành:$$\frac{-3}{-15} = \frac{1}{5}$$Điều này phù hợp với trực giác rằng quá trình này thiên về thất bại, do đó việc đạt được hai thành công trước hai lần thất bại là tương đối khó xảy ra. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(T \log n)$| mỗi trường hợp thử nghiệm yêu cầu lũy thừa mô-đun cho lũy thừa của$n$| 
| Không gian |$O(1)$| chỉ duy trì một số lượng biến không đổi | 

Các ràng buộc cho phép lên đến$5 \cdot 10^6$tổng cộng$n$và phép lũy thừa logarit cho mỗi trường hợp thử nghiệm đủ nhanh trong Python khi được triển khai lặp đi lặp lại. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 10**9 + 7
inv2 = (MOD + 1) // 2

def solve():
    input = sys.stdin.readline
    T = int(input())
    for _ in range(T):
        n, p, q = map(int, input().split())
        if 2 * p == q:
            print(inv2)
            continue
        r = (q - p) * pow(p, MOD - 2, MOD) % MOD
        rn = pow(r, n, MOD)
        r2n = rn * rn % MOD
        ans = (1 - rn) * pow((1 - r2n) % MOD, MOD - 2, MOD) % MOD
        print(ans)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as sio
    out = sio.StringIO()
    with redirect_stdout(out):
        solve()
    return out.getvalue().strip()

# sample-like tests
assert run("1\n1 1 2\n") == str(inv2)

# symmetric case
assert run("1\n5 1 2\n") == str(inv2)

# small biased case
assert run("1\n2 1 3\n") == str(pow(5, MOD-2, MOD))

# multiple tests
assert run("2\n1 1 2\n2 1 3\n") == str(inv2) + "\n" + str(pow(5, MOD-2, MOD))
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đối xứng$p/q=1/2$|$1/2$| nhánh xác suất bằng nhau | 
| trường hợp sai lệch nhỏ | phân số được tính toán | tính đúng đắn của công thức | 
| nhiều trường hợp thử nghiệm | độc lập theo từng trường hợp | xử lý T | 

## Vỏ cạnh 

Tình huống tế nhị nhất là khi$q = 2p$. Trong trường hợp này tỷ lệ$r = \frac{q-p}{p} = 1$, làm cho mẫu số$1 - r^{2n}$trở thành số không. Việc triển khai ngây thơ sẽ cố gắng đảo ngược mô-đun về số 0 và gây ra sự cố hoặc tạo ra rác. Việc xử lý đối xứng rõ ràng sẽ tránh được điều này hoàn toàn bằng cách đoản mạch để$\frac{1}{2}$. 

Một trường hợp tế nhị khác là khi$n = 1$. Quá trình kết thúc sau một bước duy nhất nên đáp án phải chính xác là xác suất thắng trong lần thử đầu tiên, tức là$\frac{p}{q}$. Cắm vào công thức:$$\frac{1 - r}{1 - r^2} = \frac{1}{1 + r} = \frac{p}{q}$$khẳng định tính nhất quán. 

Cuối cùng, khi xác suất lớn và nghịch đảo mô-đun được tính toán lặp đi lặp lại, việc quên giảm các giá trị trung gian có thể dẫn đến tràn hoặc số học mô-đun không chính xác. Giữ tất cả các giá trị trung gian modulo$10^9 + 7$đảm bảo sự ổn định trong suốt quá trình tính toán.
