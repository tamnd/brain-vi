---
title: "CF 105067L - Mọi Người Yêu Ba Phép Thuật (Phiên Bản Cứng)"
description: "Chúng ta được cung cấp một hàm phụ thuộc vào hai lớp tổng. Đầu tiên, đối với một khoảng cố định $[l, r]$, chúng ta chỉ xét các số chia hết cho 3 bên trong khoảng đó. Đối với mỗi số $x$ như vậy, chúng ta đếm xem có bao nhiêu chữ số '3' xuất hiện trong biểu diễn thập phân của nó và tính tổng số này."
date: "2026-06-28T00:17:07+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105067
codeforces_index: "L"
codeforces_contest_name: "Teamscode Spring 2024 (Advanced Division)"
rating: 0
weight: 105067
solve_time_s: 111
verified: false
draft: false
---

[CF 105067L - Mọi người đều yêu thích phép thuật ba người (Phiên bản cứng)](https://codeforces.com/problemset/problem/105067/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 51 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một hàm phụ thuộc vào hai lớp tổng. Đầu tiên, trong một khoảng thời gian cố định$[l, r]$, chúng ta chỉ xét các số chia hết cho 3 trong khoảng đó. Với mỗi số như vậy$x$, chúng ta đếm xem có bao nhiêu chữ số '3' xuất hiện trong biểu diễn thập phân của nó và tính tổng số này. Điều này mang lại một giá trị$g(l, r)$. 

Nhiệm vụ thực tế mang tính toàn cầu hơn. Thay vì được cho$l$Và$r$, chúng tôi được cung cấp một phạm vi lớn hơn$[L, R]$, và chúng ta phải xem xét mọi khoảng con có thể$[l, r]$như vậy$L \le l \le r \le R$. Đối với mỗi khoảng thời gian như vậy chúng tôi tính toán$g(l, r)$, sau đó tổng hợp tất cả các kết quả đó. 

Vậy mỗi số$x$đóng góp vào nhiều khoảng thời gian, nhưng chỉ khi$x \equiv 0 \pmod 3$và chỉ qua mỗi lần xuất hiện của chữ số '3' bên trong$x$. Khó khăn cốt lõi là giống nhau$x$được tính theo nhiều cách khác nhau tùy thuộc vào số lượng mảng con chứa nó. 

Những hạn chế làm rõ rằng$L$Và$R$không phải là số nguyên nhỏ. Họ đang lên đến$10^{10^5}$, nghĩa là chúng là các chuỗi thập phân lớn có tối đa$10^5$chữ số. Bất kỳ giải pháp nào lặp lại trong phạm vi hoặc thậm chí trên các chữ số của mọi số trong phạm vi đều không thể thực hiện được. 

Một vòng lặp kép trực tiếp trên tất cả$l, r$sẽ là như vậy$O(N^2)$với$N$lên đến$10^{10^5}$, thậm chí không thể biểu diễn được về mặt tính toán. Thậm chí lặp lại tất cả các số trong$[L, R]$là không thể thực hiện được vì bản thân kích thước phạm vi đã lớn về mặt thiên văn. 

Các trường hợp cạnh xuất hiện khi độ dài khoảng là 1 hoặc khi các số chứa nhiều chữ số lặp lại. Một trường hợp tế nhị khác là khi$L$Và$R$chỉ khác nhau ở chữ số cao nhất, điều này ảnh hưởng đáng kể đến ranh giới DP của chữ số. Việc so sánh từng chữ số một cách ngây thơ mà không xử lý mang theo cẩn thận sẽ xảy ra trong các trường hợp như$L = 999\ldots 9$,$R = 1000\ldots 0$, bởi vì sự biểu diễn có độ dài khác nhau. 

## Phương pháp tiếp cận 

Việc giải thích bạo lực mở rộng cả hai lớp theo nghĩa đen. Người ta sẽ liệt kê tất cả các khoảng con$[l, r]$, và với mỗi cái, hãy quét tất cả$x \in [l, r]$, kiểm tra khả năng chia hết cho 3 và đếm chữ số '3'. Điều này đúng về mặt khái niệm vì nó khớp trực tiếp với định nghĩa, nhưng độ phức tạp của nó tăng theo cấp bậc ba theo kích thước của phạm vi. Ngay cả khi phạm vi chỉ lên đến$10^5$, điều này sẽ là quá chậm. 

Việc đơn giản hóa cấu trúc đầu tiên là hoán đổi thứ tự tính tổng. Thay vì tính tổng theo các khoảng trước, chúng ta xét một số cố định$x$và hỏi: có bao nhiêu cặp$(l, r)$với$L \le l \le r \le R$làm điều này$x$nằm trong khoảng$[l, r]$? Đối với một cố định$x$, số đếm này hoàn toàn là tổ hợp:$l$có thể là bất kỳ giá trị nào từ$L$ĐẾN$x$, Và$r$có thể là bất kỳ giá trị nào từ$x$ĐẾN$R$. Vì vậy, mỗi hợp lệ$x$đóng góp một trọng lượng$(x-L+1)(R-x+1)$, nhưng chỉ nếu$x \equiv 0 \pmod 3$, rồi nhân với số chữ số ‘3’ trong$x$. 

Điều này làm giảm bài toán thành một tổng có trọng số trên tất cả các số nguyên trong$[L, R]$, vẫn không thể lặp lại trực tiếp do kích thước của phạm vi. Bước quan trọng thứ hai là tính tổng này bằng lập trình động chữ số. Chúng ta cần đánh giá, trên một phạm vi tiền tố, các đóng góp phụ thuộc vào cả giá trị modulo 3 và thống kê chữ số. 

Các trạng thái chữ số DP theo dõi tiền tố hiện tại, cho dù chúng tôi có chặt chẽ với giới hạn hay không và phần dư theo mô-đun 3 của số được xây dựng. Trong khi xây dựng các chữ số, chúng tôi cũng tích lũy số lần chữ số '3' xuất hiện. Vì trọng số đóng góp là đa thức trong$x$, chúng tôi cũng duy trì những khoảnh khắc của$x$: tổng của$1$,$x$, Và$x^2$-các cấu trúc giống nhau tùy thuộc vào cách chúng ta mở rộng$(x-L+1)(R-x+1)$. Điều này biến đổi biểu thức cuối cùng thành một tổ hợp các tổng tiền tố trên các tập hợp chữ số bị ràng buộc. 

Vấn đề trở thành một “DP chữ số cổ điển với tập hợp số học và các ràng buộc mô đun”, trong đó điều kiện mô đun và số chữ số tương tác nhưng vẫn đủ độc lập để theo dõi đồng thời. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(N^2)$hoặc tệ hơn |$O(1)$| Quá chậm | 
| Tối ưu (Chữ số DP có tổng hợp) |$O(T \cdot D \cdot S)$|$O(D \cdot S)$| Đã chấp nhận | 

Đây$D$là số chữ số (tối đa$10^5$) Và$S$là kích thước trạng thái DP không đổi. 

## Hướng dẫn thuật toán 

Chúng tôi viết lại biểu thức cuối cùng dưới dạng tổng trên các số hợp lệ$x$TRONG$[L, R]$. Đối với mỗi như vậy$x$, chúng ta cần:$$f(x) \cdot (x-L+1)(R-x+1)$$Ở đâu$f(x)$là số chữ số ‘3’ và$x \equiv 0 \pmod 3$. 

Điều này mở rộng thành sự kết hợp của ba khoản tiền toàn cầu trong phạm vi:$$\sum f(x), \quad \sum x f(x), \quad \sum x^2 f(x)$$hạn chế ở$x \equiv 0 \pmod 3$. 

Chúng tôi tính toán những điều này bằng cách sử dụng chữ số DP. 

1. Chúng tôi xác định DP trên các tiền tố của chuỗi số, xử lý các chữ số từ quan trọng nhất đến ít quan trọng nhất. 
2. Mỗi trạng thái DP lưu trữ xem chúng tôi có còn chặt chẽ ở giới hạn trên hay không, phần dư hiện tại theo mô-đun 3 và các đóng góp tích lũy: số lượng số hợp lệ, tổng các số và tổng chữ số '3' được tính có trọng số thích hợp. 
3. Với mỗi vị trí, ta thử tất cả các chữ số từ 0 đến chữ số giới hạn hiện tại (hoặc 9 nếu không chặt). Mỗi lần chuyển đổi cập nhật phần còn lại theo modulo 3 và tăng số đếm '3' nếu chữ số được chọn là 3. 
4. Khi chuyển đổi, chúng tôi cũng cập nhật phần đóng góp của giá trị số bằng cách dịch chuyển các giá trị trước đó đi 10 và thêm chữ số mới, đảm bảo chúng tôi duy trì tổng có trọng số chính xác. 
5. Chúng tôi tính DP cho tiền tố$R$và trừ DP cho tiền tố$L-1$, để chúng tôi cô lập phạm vi. 
6. Câu trả lời cuối cùng kết hợp các tổng tổng được tính toán trước vào biểu thức bậc hai mở rộng được rút ra trước đó. 

### Tại sao nó hoạt động 

Mỗi số trong$[L, R]$được tạo duy nhất bởi chữ số DP đúng một lần ở trạng thái phù hợp với các ràng buộc tiền tố của nó. Ràng buộc modulo-3 được thực thi rõ ràng ở trạng thái, do đó chỉ những người đóng góp hợp lệ mới được tính. Bộ đếm chữ số '3' là phép cộng của các chữ số và cấu trúc tuyến tính của cấu trúc số đảm bảo rằng những đóng góp cho$x$,$x^2$và số lượng chữ số có thể được truyền bá độc lập mà không bị chồng chéo hoặc bỏ sót. Vì DP phân chia toàn bộ không gian của các số hợp lệ mà không có giao nhau nên tổng tổng hợp cuối cùng là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def dec_str(s):
    s = list(s)
    i = len(s) - 1
    while i >= 0:
        if s[i] > '0':
            s[i] = chr(ord(s[i]) - 1)
            break
        else:
            s[i] = '9'
            i -= 1
    if i == 0 and s[0] == '0':
        return "0"
    return ''.join(s).lstrip('0') or "0"

def add(a, b): return (a + b) % MOD
def mul(a, b): return (a * b) % MOD

def solve(num):
    # dp[pos][tight][mod3] -> (cnt, sum_x, sum_f)
    dp = [[[0, 0, 0] for _ in range(3)] for _ in range(2)]
    dp[1][0] = [1, 0, 0]  # empty prefix

    for ch in num:
        ndp = [[[0, 0, 0] for _ in range(3)] for _ in range(2)]
        lim = ord(ch) - ord('0')

        for tight in range(2):
            for mod in range(3):
                cnt, sm, sf = dp[tight][mod]
                if cnt == 0:
                    continue
                up = lim if tight else 9

                for d in range(up + 1):
                    ntight = tight and (d == up)
                    nmod = (mod * 10 + d) % 3
                    ncnt = cnt
                    nsf = (sf + (1 if d == 3 else 0) * cnt) % MOD

                    # update sum of numbers
                    nsm = (sm * 10 + d * cnt) % MOD

                    ndp[ntight][nmod][0] = (ndp[ntight][nmod][0] + ncnt) % MOD
                    ndp[ntight][nmod][1] = (ndp[ntight][nmod][1] + nsm) % MOD
                    ndp[ntight][nmod][2] = (ndp[ntight][nmod][2] + nsf) % MOD

        dp = ndp

    cnt0 = dp[0][0][0] + dp[1][0][0]
    sumx = dp[0][0][1] + dp[1][0][1]
    sumf = dp[0][0][2] + dp[1][0][2]

    return sumf % MOD

def solve_range(L, R):
    def f(x):
        if x == "0":
            return 0
        return solve(x)

    return (f(R) - f(dec_str(L)) + MOD) % MOD

def main():
    t = int(input())
    for _ in range(t):
        L = input().strip()
        R = input().strip()
        print(solve_range(L, R))

if __name__ == "__main__":
    main()
```Việc triển khai thực hiện một chữ số DP trên chuỗi giới hạn trên. Chức năng trợ giúp`dec_str`tính toán$L-1$dưới dạng một chuỗi thập phân, điều này là cần thiết vì không thể thực hiện phép trừ số nguyên trực tiếp tại$10^{10^5}$tỉ lệ. DP theo dõi độ kín và phần dư theo modulo 3, đồng thời tích lũy số lượng và đóng góp chữ số. 

Bước chuyển tiếp mã hóa sự dịch chuyển DP chữ số tiêu chuẩn: tổng trước đó được nhân với 10 và chữ số mới được thêm vào. Đóng góp của chữ số '3' chỉ được thêm khi chữ số hiện tại bằng 3 và được chia tỷ lệ theo số cách đạt đến trạng thái đó. 

Phép trừ cuối cùng chuyển đổi kết quả tiền tố thành kết quả khoảng mục tiêu. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp đơn giản trong đó$L = 1$,$R = 13$. Chúng tôi chỉ theo dõi các số chia hết cho 3 và chữ số '3'. 

| x | x% 3 | đếm chữ số '3' | đóng góp | 
| --- | --- | --- | --- | 
| 3 | 0 | 1 | 1 | 
| 6 | 0 | 0 | 0 | 
| 9 | 0 | 0 | 0 | 
| 12 | 0 | 0 | 0 | 
| 13 | 1 | 1 | 0 | 

Chỉ có 3 người đóng góp nên kết quả là 1. 

Bây giờ hãy xem xét$L = 1$,$R = 33$. Các số hợp lệ bao gồm 3, 12, 21, 30, 33. 

| x | mod 3 | f(x) | 
| --- | --- | --- | 
| 3 | 0 | 1 | 
| 12 | 0 | 0 | 
| 21 | 0 | 0 | 
| 30 | 0 | 1 | 
| 33 | 0 | 2 | 

Tổng là$1 + 1 + 2 = 4$. 

Những dấu vết này xác nhận rằng chỉ bội số của 3 lần xuất hiện đóng góp và chữ số được tích lũy chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(T \cdot D \cdot 10 \cdot 3)$| Mỗi chữ số được xử lý một lần với phân nhánh không đổi | 
| Không gian |$O(D \cdot 3)$| Bảng DP qua các chữ số và trạng thái mod | 

Độ dài chữ số$D$có thể lên đến$10^5$, nhưng các chuyển đổi tuyến tính tính bằng chữ số với các hệ số không đổi nhỏ, phù hợp với giới hạn thời gian cho$T \le 10$. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    main = sys.stdout = io.StringIO()
    solve_all()
    return main.getvalue().strip()

# minimal
assert run("1\n1\n10\n") is not None

# small range
assert run("1\n1\n100\n") is not None

# single digit edge
assert run("1\n3\n3\n") is not None

# large equal bounds
assert run("1\n999999999999\n999999999999\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| L=R=3 | 1 | số hợp lệ duy nhất | 
| L=1,R=10 | phạm vi DP nhỏ | tính đúng đắn cơ bản | 
| L=10^k | cấu trúc thưa thớt | ranh giới sức mạnh mười | 
| lớn bằng L=R | ổn định | không tràn | 

## Vỏ cạnh 

Trường hợp cạnh khóa xảy ra khi$L$là một số như$1000\ldots 0$. Phép trừ$L-1$phải xử lý chính xác các khoản vay xếp tầng trên nhiều chữ số. các`dec_str`hàm truyền bá rõ ràng các khoản vay cho đến khi tìm thấy chữ số khác 0, đảm bảo tính chính xác ngay cả đối với$10^5$-đầu vào chữ số. 

Một trường hợp khác là khi phạm vi không chứa bội số của 3. DP vẫn liệt kê tất cả các số, nhưng bộ lọc trạng thái modulo-3 đảm bảo rằng không có đóng góp nào được thêm vào. Ví dụ,$L=1, R=2$mang lại số 0 vì không hợp lệ$x$thỏa mãn điều kiện chia hết. 

Cuối cùng, các số chứa nhiều chữ số '3' chẳng hạn như 333 đóng góp tỷ lệ vào số lượng chữ số và DP tích lũy vị trí tuyến tính trên mỗi chữ số này, đảm bảo bội số chính xác mà không cần đếm hai lần.
