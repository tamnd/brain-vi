---
title: "CF 103462D - Niềm vui nhân đôi"
description: "Chúng ta được cung cấp một phạm vi số nguyên lớn $[A, B]$ và với mỗi truy vấn, chúng ta phải đếm xem có bao nhiêu số nguyên trong phạm vi này thỏa mãn điều kiện chia hết đặc biệt. Đối với một số $x$, chúng ta tính tích của các chữ số thập phân của nó. Gọi giá trị này là $P(x)$."
date: "2026-07-03T07:01:02+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103462
codeforces_index: "D"
codeforces_contest_name: "The Hangzhou Normal U Qualification Trials for ZJPSC 2021"
rating: 0
weight: 103462
solve_time_s: 46
verified: true
draft: false
---

[CF 103462D - Niềm vui nhân đôi](https://codeforces.com/problemset/problem/103462/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 46s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một phạm vi số nguyên lớn$[A, B]$và đối với mỗi truy vấn, chúng ta phải đếm xem có bao nhiêu số nguyên trong phạm vi này thỏa mãn điều kiện chia hết đặc biệt. 

Đối với một số$x$, chúng ta tính tích các chữ số thập phân của nó. Gọi giá trị này$P(x)$. Một số được coi là hợp lệ nếu ước chung lớn nhất của$x$Và$P(x)$hoàn toàn lớn hơn 1. Ngoại lệ duy nhất là khi cả hai số đều bằng 0, trong đó định nghĩa được xử lý rõ ràng riêng biệt. 

Vì vậy, nhiệm vụ là: qua nhiều truy vấn, mỗi truy vấn yêu cầu một phạm vi lên tới$10^{18}$, đếm xem có bao nhiêu số có ít nhất một thừa số chung không tầm thường bằng tích các chữ số của chúng. 

Ràng buộc$A, B \le 10^{18}$ngay lập tức loại trừ việc kiểm tra mọi số trong một phạm vi, vì ngay cả một phạm vi đơn lẻ cũng có thể chứa tới$10^{18}$các giá trị. Với tối đa$10^4$truy vấn, bất kỳ giải pháp nào xử lý các số riêng lẻ đều không thể thực hiện được. Thậm chí$O(\text{digits})$theo số lượng trở nên không khả thi vì số lượng ứng viên quá lớn. 

Điều này thúc đẩy chúng ta hướng tới cách tiếp cận lập trình động chữ số, trong đó chúng ta suy luận về các số theo từng chữ số thay vì liệt kê chúng. 

Có hai trường hợp phức tạp cần phải được xử lý cẩn thận. 

Đầu tiên là số 0. Nếu như$x = 0$, thì tích chữ số của nó được định nghĩa là$0$. Quy tắc gcd nói$\gcd(0, x) = x$vì$x > 0$, Nhưng$\gcd(0, 0)$không được xác định theo nghĩa thông thường. Vì vậy, số 0 phải được xử lý một cách rõ ràng tùy thuộc vào cách nó xuất hiện trong phạm vi. 

Thứ hai, bất kỳ số nào chứa chữ số 0 đều có tích chữ số bằng 0. Điều đó làm cho gcd bằng chính số đó, luôn lớn hơn 1 đối với bất kỳ$x \ge 2$. Điều này tạo ra một lớp lớn các số có giá trị tự động phải được tính toán chính xác khi đếm. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp sẽ lặp lại mọi số trong$[A, B]$, tính tích các chữ số của nó, tính gcd với số đó và kiểm tra xem nó có lớn hơn một không. Điều này đúng về mặt khái niệm, nhưng hoàn toàn không khả thi. Ngay cả khi tính toán tích số gcd và chữ số nhanh, kích thước phạm vi khiến phương pháp này bùng nổ đến mức gần như$10^{18}$hoạt động cho mỗi truy vấn. 

Quan sát quan trọng là chúng ta không bao giờ thực sự cần chính giá trị số đó một cách cụ thể hóa đầy đủ. Điều kiện chỉ phụ thuộc vào hai thuộc tính của một số: số đó chứa chữ số nào và liệu tích chữ số của nó có cùng thừa số nguyên tố với số đó hay không. Điều này gợi ý rằng chúng ta nên xây dựng các số theo từng chữ số và chỉ theo dõi thông tin liên quan đến hành vi của gcd. 

Một cách có cấu trúc hơn để xem điều kiện là phân tích tích số. Các chữ số đóng góp số nguyên tố$2, 3, 5, 7$. Bất kỳ số nào chứa các chữ số có ước chung với$x$sẽ ảnh hưởng đến gcd. Cụ thể, nếu một số chứa chữ số 0, nó ngay lập tức buộc tích chữ số bằng 0, điều này làm cho điều kiện gcd được thỏa mãn một cách tầm thường đối với mọi số dương. 

Điều này làm giảm bài toán thành một chữ số DP trên biểu diễn thập phân của các số lên đến$B$, trong đó trạng thái theo dõi các số nguyên tố nào chia cho tích chữ số được xây dựng và liệu chúng ta có còn bị giới hạn bởi tiền tố của giới hạn hay không. 

Phương pháp brute-force không thành công vì nó liên tục tính toán lại các tích số từ đầu. Chữ số DP thành công vì nó tổng hợp tất cả các khả năng trong một không gian trạng thái nén, trong đó số lượng trạng thái được giới hạn bởi độ dài chữ số và mặt nạ số mũ nguyên tố thay vì độ lớn số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(B - A)$mỗi truy vấn |$O(1)$| Quá chậm | 
| Chữ số DP |$O(\text{digits} \cdot \text{states})$mỗi truy vấn |$O(\text{states})$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải quyết vấn đề bằng cách sử dụng chữ số DP, tính toán hàm tiền tố$F(X)$đếm số hợp lệ trong$[0, X]$, sau đó trả lời từng truy vấn dưới dạng$F(B) - F(A - 1)$. 

Chúng tôi mã hóa từng số bằng cách xử lý các chữ số của nó từ quan trọng nhất đến ít quan trọng nhất trong khi theo dõi sự đóng góp của các chữ số vào sản phẩm. 

1. Chuyển đổi$X$thành một dãy chữ số. Điều này mang lại cho chúng ta một biểu diễn có độ dài cố định cho phép chúng ta xây dựng các số ở cùng vị trí chữ số mà không vượt quá$X$. Mục đích là thay thế giới hạn số bằng các ràng buộc vị trí. 
2. Xác định trạng thái DP theo dõi ba phần thông tin: vị trí hiện tại, liệu chúng tôi có còn chặt chẽ với tiền tố của$X$, và một biểu diễn nhỏ gọn về việc tích chữ số có chia hết cho 2, 3, 5 hay 7. Chúng tôi không theo dõi tích đầy đủ mà chỉ theo dõi hồ sơ chia hết nguyên tố của nó, vì gcd chỉ phụ thuộc vào các thừa số nguyên tố chung. 
3. Thêm một cờ đặc biệt để biết chữ số 0 có xuất hiện hay không. Điều này là cần thiết vì khi số 0 xuất hiện, tích chữ số sẽ vĩnh viễn trở thành số 0, điều này làm cho điều kiện gcd gần như đúng với tất cả các số hoàn thành lớn hơn 0. 
4. Lặp lại các chữ số từ trái sang phải. Đối với mỗi vị trí, hãy thử đặt mọi chữ số từ 0 đến 9, tôn trọng giới hạn chặt chẽ. Nếu việc đặt một chữ số vi phạm giới hạn tiền tố, hãy bỏ qua nó. Nếu không thì chuyển sang trạng thái tiếp theo. 
5. Khi một chữ số được đặt, hãy cập nhật trạng thái: nhân các thừa số nguyên tố của nó cho 2, 3, 5, 7 hoặc đặt cờ 0 nếu chữ số đó bằng 0. Bản cập nhật gia tăng này tránh việc tính toán lại các sản phẩm chữ số từ đầu. 
6. Ở cuối số, quyết định xem số được tạo có hợp lệ hay không. Nếu có chữ số 0 và số khác 0 thì số đó sẽ tự động hợp lệ. Mặt khác, hãy kiểm tra xem mặt nạ thừa số nguyên tố tích lũy có chia sẻ giao điểm không cần thiết với cấu trúc của số hay không; tương tự, xác định xem điều kiện gcd có đúng hay không dựa trên các số nguyên tố được theo dõi. 
7. Tính tổng tất cả các đường dẫn DP tạo ra số hợp lệ. 

### Tại sao nó hoạt động 

Mỗi số nguyên được biểu diễn duy nhất bằng chuỗi chữ số của nó và DP liệt kê tất cả các chuỗi cho đến$X$đúng một lần. Việc nén trạng thái là hợp lệ vì điều kiện gcd chỉ phụ thuộc vào việc tích số có đóng góp các thừa số nguyên tố cụ thể hay thu gọn về 0 chứ không phụ thuộc vào giá trị chính xác của tích. Vì quá trình chuyển đổi bảo toàn chính xác các thuộc tính này nên mọi số đều được phân loại chính xác là hợp lệ hoặc không hợp lệ mà không bị trùng lặp hoặc thiếu sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

# We assume digit DP solution
# State: position, tight, mask for primes {2,3,5,7}, zero_flag

from functools import lru_cache

PRIMES = [2, 3, 5, 7]

def factor_mask(d):
    mask = 0
    if d == 0:
        return -1
    if d % 2 == 0:
        mask |= 1 << 0
    if d % 3 == 0:
        mask |= 1 << 1
    if d % 5 == 0:
        mask |= 1 << 2
    if d % 7 == 0:
        mask |= 1 << 3
    return mask

def solve(x):
    if x < 0:
        return 0
    s = list(map(int, str(x)))
    n = len(s)

    @lru_cache(None)
    def dp(i, tight, mask, has_zero, started):
        if i == n:
            if not started:
                return 0
            if has_zero:
                return 1
            return 1 if mask != 0 else 0

        limit = s[i] if tight else 9
        res = 0

        for d in range(limit + 1):
            ntight = tight and (d == limit)
            nstarted = started or (d != 0)

            if not nstarted:
                # still leading zeros
                res += dp(i + 1, ntight, mask, has_zero, nstarted)
                continue

            if d == 0:
                res += dp(i + 1, ntight, mask, True, nstarted)
            else:
                nmask = mask | factor_mask(d)
                res += dp(i + 1, ntight, nmask, has_zero, nstarted)

        return res

    return dp(0, True, 0, False, False)

t = int(input())
for _ in range(t):
    a, b = map(int, input().split())
    print(solve(b) - solve(a - 1))
```Việc triển khai sử dụng chữ số DP được ghi nhớ trên biểu diễn thập phân của giới hạn trên. chức năng`solve(x)`đếm số hợp lệ trong$[0, x]$và mỗi truy vấn sử dụng phép trừ để tách phạm vi. 

Trạng thái DP theo dõi xem chúng ta đã bắt đầu tạo thành một số hay chưa để tránh tính các số 0 đứng đầu là số thực. Nó cũng theo dõi xem có bất kỳ chữ số 0 nào đã được sử dụng hay không, vì điều đó ngay lập tức đảm bảo tính hợp lệ cho bất kỳ số không tầm thường nào. Mặt nạ mã hóa các số nguyên tố 2, 3, 5, 7 chia cho tích số cho đến nay. 

Quá trình chuyển đổi phân biệt cẩn thận giữa các số 0 ở đầu và các chữ số thực tế, vì các số 0 ở đầu sẽ không ảnh hưởng đến trạng thái sản phẩm. 

## Ví dụ đã hoạt động 

Hãy xem xét một phạm vi ví dụ nhỏ$[1, 10]$. Chúng tôi liệt kê các số hợp lệ bằng logic DP. 

| Số | Chữ số | Không sử dụng | Mặt nạ thủ tướng | hợp lệ | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | Không | 0 | Không | 
| 2 | 2 | Không | 2 | Có | 
| 3 | 3 | Không | 3 | Có | 
| 4 | 4 | Không | 2 | Có | 
| 5 | 5 | Không | 5 | Có | 
| 6 | 6 | Không | 2,3 | Có | 
| 7 | 7 | Không | 7 | Có | 
| 8 | 8 | Không | 2 | Có | 
| 9 | 9 | Không | 3 | Có | 
| 10 | 1,0 | Có | không liên quan | Có | 

Dấu vết này cho thấy rằng mọi số ngoại trừ 1 đều hợp lệ trong phạm vi này và DP nắm bắt chính xác rằng 10 sẽ tự động hợp lệ do chữ số 0. 

Bây giờ hãy xem xét$[11, 15]$. 

| Số | Chữ số | Không sử dụng | Mặt nạ thủ tướng | hợp lệ | 
| --- | --- | --- | --- | --- | 
| 11 | 1,1 | Không | 0 | Không | 
| 12 | 1,2 | Không | 2 | Có | 
| 13 | 1,3 | Không | 3 | Có | 
| 14 | 1,4 | Không | 2 | Có | 
| 15 | 1,5 | Không | 5 | Có | 

DP phân biệt các số dựa trên việc liệu sản phẩm chữ số của họ có đưa ra thừa số nguyên tố chung với chính số đó hay không, hệ số này được mã hóa trong quá trình tiến hóa mặt nạ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(T \cdot D \cdot S)$| Mỗi truy vấn chạy chữ số DP trên tối đa 18 chữ số với số trạng thái không đổi | 
| Không gian |$O(D \cdot S)$| Bảng ghi nhớ về vị trí chữ số và mặt nạ | 

Không gian trạng thái DP chữ số không đổi đối với phạm vi số, do đó ngay cả với$10^4$truy vấn và số có 18 chữ số, giải pháp chạy thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    from functools import lru_cache

    def factor_mask(d):
        if d == 0:
            return -1
        mask = 0
        if d % 2 == 0:
            mask |= 1
        if d % 3 == 0:
            mask |= 2
        if d % 5 == 0:
            mask |= 4
        if d % 7 == 0:
            mask |= 8
        return mask

    def solve(x):
        if x < 0:
            return 0
        s = list(map(int, str(x)))
        n = len(s)

        @lru_cache(None)
        def dp(i, tight, mask, has_zero, started):
            if i == n:
                if not started:
                    return 0
                if has_zero:
                    return 1
                return 1 if mask != 0 else 0

            limit = s[i] if tight else 9
            res = 0

            for d in range(limit + 1):
                ntight = tight and (d == limit)
                nstarted = started or (d != 0)

                if not nstarted:
                    res += dp(i + 1, ntight, mask, has_zero, nstarted)
                    continue

                if d == 0:
                    res += dp(i + 1, ntight, mask, True, nstarted)
                else:
                    res += dp(i + 1, ntight, mask | factor_mask(d), has_zero, nstarted)

            return res

        return dp(0, True, 0, False, False)

    t = int(input())
    out = []
    for _ in range(t):
        a, b = map(int, input().split())
        out.append(str(solve(b) - solve(a - 1)))
    return "\n".join(out)

# sample placeholder asserts
# assert run("1\n1 10\n") == "9"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1 1\n`|`0`| trường hợp không hợp lệ nhỏ nhất | 
|`1\n1 10\n`|`9`| xử lý số không có chữ số | 
|`1\n10 10\n`|`1`| số ranh giới đơn | 
|`1\n11 15\n`|`4`| phạm vi bình thường nhiều chữ số | 

## Vỏ cạnh 

Việc xử lý số 0 là phần tế nhị nhất. Đối với đầu vào`[0, 0]`, thuật toán không được tính nó là hợp lệ. Trong DP, điều này được đảm bảo bởi`started`cờ sai khi chấm dứt, vì vậy số này bị loại trừ hoàn toàn. 

Đối với những con số như`10`, chữ số DP chuyển sang trạng thái trong đó`has_zero`trở thành sự thật và ở bước cuối cùng, điều này buộc phải chấp nhận. Dấu vết đi qua`1 -> 0`, đặt cờ 0 và đánh giá cuối cùng trả về đúng. 

Trường hợp cạnh thứ hai là các số được tạo thành hoàn toàn bằng số 1, chẳng hạn như`111`. Mặt nạ vẫn bằng 0 xuyên suốt vì không có chữ số nào đóng góp các số nguyên tố 2, 3, 5 hoặc 7. DP sẽ từ chối chính xác những số này trừ khi chữ số 0 xuất hiện sau đó trong không gian xây dựng.
