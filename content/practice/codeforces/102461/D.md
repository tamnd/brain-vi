---
title: "CF 102461D - Bao thanh toán RSA"
description: "Số (n) được cố tình tạo ra từ các thừa số nguyên tố gần nhau một cách bất thường. Đối với (k=2), có hai số nguyên tố (b)-bit riêng biệt (p1,p2), trong đó (p2) là số nguyên tố ngay sau (p1) và (n=p1p2). Với (k=4), có hai cặp độc lập như vậy, vì vậy [ n=p1p2q1q2."
date: "2026-08-08T09:55:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102461
codeforces_index: "D"
codeforces_contest_name: "Innopolis Open 2019-2020, qualification, contest 2"
rating: 0
weight: 102461
solve_time_s: 125
verified: true
draft: false
---

[CF 102461D - Phân tích RSA](https://codeforces.com/problemset/problem/102461/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 5s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Số (n) được cố tình tạo ra từ các thừa số nguyên tố gần nhau một cách bất thường. Đối với (k=2), có hai số nguyên tố (b)-bit riêng biệt (p_1,p_2), trong đó (p_2) là số nguyên tố ngay sau (p_1) và (n=p_1p_2). Với (k=4), có hai cặp độc lập như vậy, nên 

[ 
n=p_1p_2q_1q_2. 
] 

Mọi số nguyên tố đều có chính xác (b) bit và tất cả các thừa số nguyên tố đều khác nhau. Đầu vào cung cấp (b), cho dù có hai hoặc bốn yếu tố và (n) ở dạng thập lục phân. Chúng ta phải in các thừa số nguyên tố, cũng ở dạng thập lục phân. Những đảm bảo này là một phần của cấu trúc toán học mà chúng tôi khai thác chứ không chỉ đơn thuần là các điều kiện xác nhận. 

(n) lớn nhất có thể có (4b\le240) bit. Đó là con số quá lớn đối với việc phân chia xét xử thông thường. Mặc dù bản thân tất cả các thừa số chỉ có 60 bit, nhưng việc thử mọi ước số có thể có cho đến thừa số đầu tiên có thể yêu cầu khoảng (2^{60}) phép chia trong trường hợp xấu nhất. Đối với (k=4), giới hạn chung thông thường của việc tìm kiếm tất cả các ước số lên đến (\sqrt n) thậm chí còn tệ hơn, xung quanh (2^{120}) ứng viên. Giới hạn một giây loại trừ cả hai cách tiếp cận. 

Phần hữu ích của việc xây dựng không phải là các số nguyên tố chỉ là số 60-bit. Các số nguyên tố liên tiếp gần nhau nên tích của chúng thừa nhận các cặp nhân tử cực kỳ gần nhau. Các cặp thừa số gần chính xác là những gì mà phương pháp phân tích nhân tử của Fermat được thiết kế để tìm. 

Có một số trường hợp nghiêm trọng mà việc triển khai bất cẩn có thể dẫn đến xử lý sai. Đầu tiên là bắt đầu tìm kiếm Fermat từ sàn chứ không phải từ trần của (\sqrt n). Ví dụ,```
4 2
dd
```đại diện cho (221=13\cdot17). Vì (\sqrt{221}) nằm trong khoảng từ 14 đến 15 nên giá trị bắt đầu đúng là (a=15). Khi đó (15^2-221=4=2^2), cho (13) và (17). Bắt đầu từ 14 tạo ra sự khác biệt âm, do đó việc triển khai gọi ngay căn bậc hai số nguyên trên đó sẽ không thành công. 

Trường hợp cạnh thứ hai giả sử rằng (a) chỉ có thể được nâng cao thông qua các giá trị lẻ. Đối với mẫu```
4 2
8f
```chúng ta có (143=11\cdot13) và giá trị thành công là (a=12), giá trị này là số chẵn. Việc hạn chế tìm kiếm ở số lẻ (a) sẽ hoàn toàn bỏ qua câu trả lời. 

Trường hợp cạnh thứ ba xuất hiện khi (k=4). Vì```
6 4
534ee3
```các thừa số là (37,41,59,61). Một biểu diễn Fermat cho (2257=37\cdot61) và (2419=41\cdot59), nhưng đây không phải là thừa số nguyên tố. Một giải pháp dừng lại sau khi tìm được một biểu diễn chỉ tìm thấy hai ước số tổng hợp. Chúng ta cần một cách biểu diễn khác và sau đó là gcds giữa các ước số thu được. 

Cuối cùng, một đầu vào có tất cả các thừa số nguyên tố bằng nhau không phải là một ca kiểm thử hợp lệ. Tuyên bố đảm bảo rõ ràng rằng tất cả các yếu tố đều khác biệt, do đó, một phép kiểm tra như (11\cdot11) sẽ vi phạm hợp đồng đầu vào. Đặc biệt, bộ thử nghiệm chính xác không nên mong đợi giải pháp được gửi sẽ xử lý được tình huống không hợp lệ đó. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản nhất là chia thử. Bắt đầu từ 2, kiểm tra xem mỗi số nguyên có chia hết cho (n) hay không và dừng lại sau khi tìm đủ thừa số. Điều này đúng vì mọi số nguyên tổng hợp đều có ước số không lớn hơn căn bậc hai của nó và ở đây thừa số nguyên tố đầu tiên chính nó xấp xỉ (2^{b-1}) hoặc lớn hơn. Với (b=60), điều đó có nghĩa là có khả năng theo thứ tự (2^{60}) phép thử chia hết. Ngay cả khi mỗi bài kiểm tra cực kỳ rẻ, điều này vẫn vượt xa thời hạn. 

Cấu trúc của các số nguyên tố được tạo ra cho chúng ta một điểm khởi đầu tốt hơn nhiều. Đầu tiên giả sử rằng (k=2), do đó (n=pq) và (p,q) gần nhau. Viết 

[ 
a=\frac{p+q}{2},\qquad c=\frac{q-p}{2}. 
] 

Vì (p) và (q) là số lẻ nên cả hai biểu thức đều là số nguyên. Sau đó 

[ 
n=pq=(a-c)(a+c)=a^2-c^2. 
] 

Do đó, 

[ 
a^2-n=c^2. 
] 

Thay vì tìm kiếm ước số, chúng ta tìm kiếm (a\ge\sqrt n) nhỏ nhất mà (a^2-n) là một hình vuông hoàn hảo. Khi tìm thấy (a) như vậy, hai thừa số chỉ đơn giản là (a-c) và (a+c). 

Đây là phép nhân tử Fermat. Lý do nó nhanh ở đây cũng chính là lý do khiến quy trình tạo yếu: hai thừa số nguyên tố gần nhau. Nếu hiệu của chúng là (g=q-p), thì giá trị thành công của (a) chỉ vào khoảng 

[ 
\frac{g^2}{8\sqrt n} 
] 

các vị trí cách xa (\sqrt n). Đối với các số nguyên tố liên tiếp xung quanh (2^b), khoảng cách điển hình nằm trên thang logarit của kích thước nguyên tố, vì vậy tìm kiếm này rất nhỏ đối với (b\le60). Khoảng cách trung bình giữa các số nguyên tố gần (x) là khoảng (\ln x). 

Trường hợp bốn thừa số ban đầu có vẻ khó hơn vì (n) không nhất thiết phải là tích của hai số gần nhau. Điều quan trọng là ghép bốn số nguyên tố khác nhau. Hãy xem xét 

[ 
n=(p_1q_1)(p_2q_2). 
] 

Vì (p_1,p_2) gần nhau và (q_1,q_2) gần nhau nên hai tích này cũng gần nhau. Chúng ta có thể sử dụng tương tự 

[ 
n=(p_1q_2)(p_2q_1). 
] 

Cả hai đều là phân rã Fermat hợp lệ. Bốn thừa số của chúng là hợp số, nhưng chúng trùng nhau ở đúng một số nguyên tố tại một thời điểm. Ví dụ, 

[ 
\gcd(p_1q_1,p_1q_2)=p_1. 
] 

Do đó, khi chúng ta có một số cặp thừa số gần nhau, các phép toán gcd thông thường sẽ khôi phục các số nguyên tố riêng lẻ. 

Lời giải chính thức tuân theo cùng một ý tưởng sai phân bình phương, thu thập một số ước số được tạo ra bởi biểu diễn Fermat và sau đó lấy gcds giữa chúng. 

Phương pháp brute-force hoạt động vì khả năng chia hết cuối cùng sẽ làm lộ ra thừa số nguyên tố, nhưng không thành công vì bản thân hệ số đó có thể là 60 bit. Việc quan sát rằng các thừa số được tạo ra là các số nguyên tố liên tiếp cho phép chúng ta tìm kiếm hiệu bình phương thay vào đó, giảm công việc từ hàm mũ trong (b) sang tìm kiếm bị chi phối bởi các khoảng trống nguyên tố nhỏ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(2^b)) phân chia trong trường hợp xấu nhất | (O(1)) | Quá chậm | 
| Tối ưu | (O(T\cdot M(b))), với (T) bị chi phối bởi các khoảng trống nguyên tố nhỏ | (O(k)) | Đã chấp nhận | 

Ở đây (M(b)) biểu thị chi phí số học trên các số nguyên bit (O(b)) và (T) là số ứng cử viên Fermat được thử nghiệm. Đối với các số nguyên tố ngẫu nhiên được tạo ra, (T) rất nhỏ. Đối với cấu trúc bốn thừa số, nếu hai khoảng trống nguyên tố liên tiếp là (g) và (h), thì tích cặp chéo gần nhau sẽ dẫn đến các phép lặp Fermat khoảng (O((g+h)^2)), rất nhỏ cho (b\le60). 

## Hướng dẫn thuật toán

1. Phân tích (n) từ hệ thập lục phân sang kiểu số nguyên có độ chính xác tùy ý của Python. Số nguyên Python có thể biểu thị trực tiếp toàn bộ giá trị 240 bit, do đó không có vấn đề tràn. 
2. Tính toán (a=\lceil\sqrt n\rceil). Nhu cầu nhận dạng của Fermat (a^2-n\ge0), vì vậy hãy bắt đầu từ mức trần để tránh các giá trị âm. 
3. Tính toán nhiều lần 

[ 
d=a^2-n 
] 

và kiểm tra xem (d) có phải là hình vuông hoàn hảo hay không. Chúng tôi tính toán (s=\lfloor\sqrt d\rfloor) và kiểm tra xem (s^2=d) có hay không. Nếu vậy thì 

[ 
n=a^2-s^2=(a-s)(a+s), 
] 

nên cả (a-s) và (a+s) đều là ước của (n). Chúng ta chèn chúng vào một tập hợp vì cùng một ước số có thể phát sinh từ các cách biểu diễn khác nhau. 

1. Tiếp tục cho đến khi tìm được ít nhất (k) ước số phân biệt. Đối với (k=2), biểu diễn thành công đầu tiên phải là chính hai thừa số nguyên tố, vì (n) có chính xác hai thừa số nguyên tố. 
2. Với (k=4), lấy gcd của mỗi cặp ước số phân biệt thu được ở bước trước. Mọi ước số thu thập được đều là tích của hai trong số bốn số nguyên tố ban đầu. Hai sản phẩm khác nhau như vậy hoặc không có chung một số nguyên tố hoặc có chung một số nguyên tố, do đó gcd của chúng là 1 hoặc một trong các số nguyên tố mong muốn. 
3. Giữ tất cả các gcd lớn hơn 1, sắp xếp các thừa số nguyên tố thu được và in chúng ở dạng thập lục phân. Thứ tự đầu ra không có ý nghĩa về mặt toán học, nhưng việc sắp xếp làm cho kết quả mang tính quyết định. 

### Tại sao nó hoạt động 

Điều bất biến là mọi số được chèn vào tập ước số đều là ước số thực sự của (n), vì nó xuất phát từ một danh tính (n=(a-s)(a+s)). Trong trường hợp hai thừa số, bất kỳ cặp thừa số không tầm thường nào cũng bao gồm chính xác hai thừa số nguyên tố. Trong trường hợp bốn yếu tố, hai phân tách cặp chéo tạo ra bốn sản phẩm riêng biệt như (p_1q_1,p_2q_2,p_1q_2,p_2q_1). Bởi vì bốn số nguyên tố là khác nhau nên việc lấy gcd giữa các tích này sẽ tách biệt từng số nguyên tố. Do đó, mọi giá trị cuối cùng được in ra đều là số nguyên tố và chia hết (n), trong khi số lượng các thừa số nguyên tố riêng biệt cần thiết sẽ được phục hồi. 

## Giải pháp Python```python
import sys
from math import isqrt, gcd

input = sys.stdin.readline

def factor_case(b, k, n):
    a = isqrt(n)
    if a * a < n:
        a += 1

    divs = set()

    while len(divs) < k:
        d = a * a - n
        s = isqrt(d)

        if s * s == d:
            divs.add(a - s)
            divs.add(a + s)

        a += 1

    if k == 2:
        ans = sorted(divs)
    else:
        values = list(divs)
        ans = set()

        for i in range(len(values)):
            for j in range(i + 1, len(values)):
                g = gcd(values[i], values[j])
                if g > 1:
                    ans.add(g)

        ans = sorted(ans)

    return ans[:k]

def solve():
    b, k = map(int, input().split())
    n = int(input().strip(), 16)

    ans = factor_case(b, k, n)

    for x in ans:
        print(format(x, "x"))

if __name__ == "__main__":
    solve()
```Phần đầu tiên của`factor_case`thiết lập điểm tìm kiếm Fermat.`isqrt(n)`trả về giá trị sàn, vì vậy cần phải kiểm tra phép nhân rõ ràng để di chuyển lên trần khi (n) không phải là số chính phương. 

Vòng lặp chính phản ánh trực tiếp danh tính sai phân bình phương. Bởi vì (a\ge\lceil\sqrt n\rceil),`d`không bao giờ là tiêu cực.`isqrt(d)`đưa ra số nguyên lớn nhất có bình phương lớn nhất`d`, và so sánh`s * s`với`d`là một phép thử bình phương hoàn hảo chính xác. 

bộ`divs`được cố tình sử dụng thay vì một danh sách. Biểu diễn hình vuông cho hai ước số và một số biểu diễn có thể chứa cùng một ước số. Điều kiện dừng yêu cầu các ước số riêng biệt, phù hợp với logic cần thiết cho trường hợp bốn yếu tố. 

Với (k=2), các ước số thu được từ cách biểu diễn thành công đầu tiên đã là hai số nguyên tố. Đối với (k=4), mã không thực hiện kiểm tra tính nguyên tố. Điều đó là an toàn vì mọi ước số chúng ta thu thập được đều là tích của đúng hai trong bốn số nguyên tố phân biệt. Hai tích riêng biệt như vậy không thể có tổng gcd: nếu chúng có chung cả hai số nguyên tố thì chúng sẽ là cùng một tích. Do đó, gcd của chúng là 1 hoặc một số nguyên tố gốc. 

Các số nguyên có độ chính xác tùy ý của Python đặc biệt thuận tiện ở đây. Giá trị lớn nhất của (n) chỉ có 240 bit, do đó phép nhân, phép trừ, căn bậc hai và gcd đều có tính thực tế. 

Việc chuyển đổi thập lục phân sử dụng`format(x, "x")`. Nó tạo ra hệ thập lục phân chữ thường mà không có số 0 đứng đầu, khớp chính xác với biểu diễn được yêu cầu. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
4 2
8f
```Giá trị thập lục phân`8f`là (143), và các thừa số cần tìm là (11) và (13). 

| Bước | (a) | (a^2-n) | (s=\lfloor\sqrt{d}\rfloor) | Quảng trường? | Ước số | 
| --- | --- | --- | --- | --- | --- | 
| Bắt đầu | 12 | 1 | 1 | Có | 11, 13 | 

Trần của (\sqrt{143}) là 12. Ngay lập tức, 

[ 
12^2-143=1^2, 
] 

vậy 

[ 
143=(12-1)(12+1)=11\cdot13. 
] 

Do đó, đầu ra thập lục phân là`b`Và`d`. Ví dụ này cũng giải thích tại sao việc tìm kiếm không được bỏ qua các giá trị chẵn của (a). 

### Mẫu 2 

Đầu vào là```
6 4
534ee3
```Giá trị thập phân là (5,459,683) và các thừa số nguyên tố của nó là (37,41,59,61). Việc ghép chéo hữu ích đầu tiên là 

[ 
37\cdot61=2257,\qquad41\cdot59=2419. 
] 

Trung điểm của chúng là 2338. 

| Bước | (a) | (a^2-n) | (các) | Quảng trường? | Ước số mới | 
| --- | --- | --- | --- | --- | --- | 
| Bắt đầu | 2338 | 6561 | 81 | Có | 2257, 2419 | 
| Tiếp theo | 2339 | 11240 | 106 | Không | không | 
| Tiếp theo | 2340 | 15917 | 126 | Không | không | 
| Tiếp theo | 2341 | 20596 | 143 | Không | không | 
| Tiếp theo | 2342 | 25281 | 159 | Có | 2183, 2501 | 

Biểu diễn thứ hai tương ứng với 

[ 
37\cdot59=2183,\qquad41\cdot61=2501. 
] 

Bây giờ gcds tiết lộ các số nguyên tố: 

[ 
\gcd(2257,2183)=37, 
] 

[ 
\gcd(2257,2501)=61, 
] 

[ 
\gcd(2419,2183)=59, 
] 

[ 
\gcd(2419,2501)=41. 
] 

Các dạng thập lục phân là`25`,`29`,`3b`, Và`3d`, phù hợp với mẫu chính thức. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(T\cdot M(b)+k^2\log n)) | (T) Các ứng cử viên Fermat được kiểm tra, theo sau là tối đa 6 gcds cho (k=4) | 
| Không gian | (O(k)) | Tối đa bốn ước số riêng biệt và bốn số nguyên tố phục hồi được lưu trữ | 

Đại lượng quan trọng là (T), chứ không phải độ lớn của chính (n). Đối với hai thừa số có khoảng cách (g), Fermat đạt được câu trả lời sau khoảng tăng (g^2/(8\sqrt n)). Đối với bốn thừa số, nếu hai khoảng trống nguyên tố liên tiếp là (g) và (h), thì một cặp chéo sẽ khác nhau khoảng (2^b(g+h)), trong khi căn bậc hai của nó là khoảng (2^{2b}). Số lần lặp kết quả là khoảng ((g+h)^2/8). Các khoảng trống nguyên tố liên tiếp xung quanh (2^b) là nhỏ trên thang đo phù hợp ở đây, do đó việc tìm kiếm (b\le60) dễ dàng thực tế. Giới hạn một giây của bài toán được thiết kế xung quanh lối tắt cấu trúc này. 

## Trường hợp thử nghiệm 

Trình trợ giúp kiểm tra bên dưới gọi quy trình tính toán tương tự được sử dụng bởi giải pháp đã gửi. Trường hợp kích thước tối đa sử dụng hai số nguyên tố liên tiếp ngay phía trên (2^{60}) và xây dựng đầu vào thập lục phân bên trong thử nghiệm nên không cần phải chuyển đổi sản phẩm 120 bit theo cách thủ công.```python
import sys
import io
from math import isqrt, gcd

input = sys.stdin.readline

def factor_case(b, k, n):
    a = isqrt(n)
    if a * a < n:
        a += 1

    divs = set()

    while len(divs) < k:
        d = a * a - n
        s = isqrt(d)

        if s * s == d:
            divs.add(a - s)
            divs.add(a + s)

        a += 1

    if k == 2:
        ans = sorted(divs)
    else:
        values = list(divs)
        ans = set()

        for i in range(len(values)):
            for j in range(i + 1, len(values)):
                g = gcd(values[i], values[j])
                if g > 1:
                    ans.add(g)

        ans = sorted(ans)

    return ans[:k]

def run(inp: str) -> str:
    global input
    old_stdin = sys.stdin
    old_input = input

    try:
        sys.stdin = io.StringIO(inp)
        input = sys.stdin.readline

        b, k = map(int, input().split())
        n = int(input().strip(), 16)
        ans = factor_case(b, k, n)

        return "".join(format(x, "x") + "\n" for x in ans)
    finally:
        sys.stdin = old_stdin
        input = old_input

# Provided sample 1.
assert run("""4 2
8f
""") == """b
d
""", "sample 1"

# Provided sample 2.
assert run("""6 4
534ee3
""") == """25
29
3b
3d
""", "sample 2"

# Minimum b, four factors: 5, 7 and 11, 13.
assert run("""4 4
138d
""") == """5
7
b
d
""", "minimum-size four-factor case"

# Boundary case where ceil(sqrt(n)) is exactly the successful Fermat value.
# 221 = 13 * 17, and ceil(sqrt(221)) = 15.
assert run("""4 2
dd
""") == """d
11
""", "ceil-sqrt boundary"

# Maximum b, two consecutive 60-bit primes.
p = 1152921504606847009
q = 1152921504606847067
max_n = p * q

max_input = f"60 2\n{max_n:x}\n"
max_output = f"{p:x}\n{q:x}\n"

assert run(max_input) == max_output, "maximum-size case"

# Four factors with different consecutive-prime gaps.
# 37, 41 and 59, 61.
assert run("""6 4
534ee3
""") == """25
29
3b
3d
""", "two close prime pairs"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`4 4 / 138d`|`5, 7, b, d`| Xây dựng tối thiểu (b), bốn yếu tố | 
|`4 2 / dd`|`d, 11`| Trần đúng căn bậc hai | 
|`60 2 / generated from p*q`|`p, q`| Kích thước bit tối đa và số học có độ chính xác tùy ý | 
|`6 4 / 534ee3`|`25, 29, 3b, 3d`| Nhiều biểu diễn Fermat và phục hồi gcd | 

Kiểm tra tất cả các yếu tố bằng nhau được yêu cầu không thể là đầu vào hợp lệ vì vấn đề đảm bảo các thừa số nguyên tố riêng biệt. Một đầu vào chẳng hạn như (121=11\cdot11) sẽ vi phạm sự đảm bảo đó và cũng sẽ thay đổi hành vi của tìm kiếm Fermat, do đó, nó chỉ nên được kiểm tra riêng nếu một đầu vào đang kiểm tra việc xử lý đầu vào không hợp lệ, điều mà vấn đề này không yêu cầu. 

## Vỏ cạnh 

Ranh giới sàn-trần được xử lý bằng cách kiểm tra rõ ràng`a * a < n`. Vì```
4 2
dd
```chúng ta có (n=221),`isqrt(221)`cho kết quả là 14 và mã tăng lên 15. Sau đó, nó thu được (15^2-221=4), do đó các ước số là (15-2=13) và (15+2=17), được in dưới dạng`d`Và`11`. 

Vấn đề chẵn lẻ được xử lý bằng cách tăng (a) lên đúng một. Vì```
4 2
8f
```giá trị thành công là (a=12). Một tìm kiếm chỉ kiểm tra các giá trị lẻ sẽ kiểm tra 11, 13, 15, v.v. và không bao giờ gặp phải biểu diễn được yêu cầu ở 12. Việc triển khai không đưa ra bất kỳ giả định chẵn lẻ nào về (a). 

Trường hợp bốn yếu tố yêu cầu nhiều hơn một đồng nhất thức nhân tố hóa. Vì```
6 4
534ee3
```biểu diễn hình vuông đầu tiên tạo ra (2257) và (2419), tương ứng với (37\cdot61) và (41\cdot59). Thứ hai tạo ra (2183) và (2501), tương ứng với (37\cdot59) và (41\cdot61). Các gcd theo cặp của chúng cô lập tất cả bốn số nguyên tố. Điều kiện dừng dựa trên tập hợp đảm bảo rằng thuật toán không dừng sau khi chỉ khám phá một cặp. 

Phân tích thập lục phân được xử lý bởi`int(..., 16)`, do đó cả chữ số đầu vào viết hoa và viết thường đều được chấp nhận mặc dù các ví dụ sử dụng chữ thường. Đầu ra được tạo bằng hệ thập lục phân chữ thường và không có số 0 đứng đầu, do đó các giá trị như số thập phân 11 và 13 trở nên chính xác`b`Và`d`. 

Các giá trị lớn nhất không yêu cầu xử lý tràn đặc biệt. Số nguyên 240 bit được hỗ trợ thoải mái bởi các số nguyên có độ chính xác tùy ý của Python và tất cả các giá trị trung gian như (a^2) vẫn có kích thước có thể quản lý được. Việc triển khai không bao giờ chuyển đổi (n) thành dấu phẩy động, điều này tránh mất độ chính xác khi (n) gần với (2^{240}). 

Trường hợp hệ số hoàn toàn bằng nhau bị cố tình vắng mặt trong các thử nghiệm hợp lệ. Ví dụ,`4 2`với (n=121) sẽ đại diện cho (11\cdot11), nhưng các yếu tố lặp lại bị cấm bởi đảm bảo đầu vào. Lời giải được phép dựa vào tính khác biệt và việc chứng minh bước gcd cũng dựa vào đó.
