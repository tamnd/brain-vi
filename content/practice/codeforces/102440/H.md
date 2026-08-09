---
title: "CF 102440H - Cảnh sát từ Rublevka"
description: "Chúng ta có một mảng số nguyên (a1,dots,an), trong đó mỗi phần tử mô tả độ khó của một tội phạm được giải quyết. Lesha muốn xóa một phần tử hiện có (ap) và chèn hai giá trị số nguyên mới (q) và (r). Mảng kết quả có (n+1) phần tử."
date: "2026-08-09T13:34:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102440
codeforces_index: "H"
codeforces_contest_name: "2018-2019 9th BSUIR Open Programming Championship. Junior"
rating: 0
weight: 102440
solve_time_s: 415
verified: true
draft: false
---

[CF 102440H - Cảnh sát từ Rublevka](https://codeforces.com/problemset/problem/102440/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 6 phút 55 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một mảng số nguyên (a_1,\dots,a_n), trong đó mỗi phần tử mô tả độ khó của một tội phạm được giải quyết. Lesha muốn xóa một phần tử hiện có (a_p) và chèn hai giá trị số nguyên mới (q) và (r). Mảng kết quả có (n+1) phần tử. 

Trình kiểm tra không so sánh trực tiếp các mảng. Nó chỉ so sánh giá trị trung bình số học và phương sai của chúng. Chúng ta cần tìm một chỉ số (p) và hai số nguyên (q,r) sao cho cả hai số liệu thống kê vẫn giống hệt nhau. Nếu không có sửa đổi như vậy tồn tại, chúng tôi in`Impossible`. 

hãy để 

[ 
S=\sum_{i=1}^n a_i 
] 

và 

[ 
Q=\sum_{i=1}^n a_i^2. 
] 

Giá trị trung bình ban đầu là (m=S/n), và phương sai có thể được viết lại thành 

[ 
D=\frac{Q}{n}-m^2. 
] 

Biểu mẫu này hữu ích hơn nhiều so với việc khai triển mọi ((a_i-m)^2), vì nó làm giảm vấn đề bảo toàn hai tổng lũy thừa đầu tiên. 

Mảng chứa tối đa (10^5) phần tử, do đó, thuật toán kiểm tra số cặp bậc hai là không khả thi. Với giới hạn cuộc thi khoảng một giây, giải pháp dự định cần phải tuyến tính hoặc gần tuyến tính. Bản thân các giá trị được giới hạn bởi (10^5), nhưng tổng của tối đa (10^5) các giá trị đó có thể đạt tới (10^{10}) và tổng bình phương có thể đạt tới (10^{15}). Số nguyên Python xử lý các giá trị này một cách an toàn, trong khi các ngôn ngữ có chiều rộng cố định cần số nguyên 64 bit. 

Có một số trường hợp nguy hiểm mà việc triển khai bất cẩn có thể bỏ sót. Nếu (n=1), phần tử duy nhất luôn có thể được thay thế bằng hai bản sao của chính nó. Ví dụ, đối với`1 / 5`, câu trả lời đúng là`Possible`, với việc loại bỏ phần tử đầu tiên và chèn`5 5`. Một phương thức giả sử mảng có ít nhất hai phần tử có thể từ chối nó một cách không chính xác. 

Vấn đề thứ hai là giá trị trung bình không nguyên. Coi như```
2
0 1
```Giá trị trung bình ban đầu là (1/2). Sau phép toán, có ba số nguyên, vì vậy tổng của chúng là số nguyên và giá trị trung bình của chúng không thể là (1/2), vì ba lần (1/2) không phải là số nguyên. Như vậy câu trả lời là`Impossible`. Một giải pháp sử dụng dấu phẩy động và so sánh gần đúng có thể dễ dàng che khuất khả năng chính xác này. 

Vấn đề thứ ba là chỉ bảo toàn giá trị trung bình là không đủ. Vì```
3
0 0 6
```giá trị trung bình là (2), nhưng việc chỉ chọn hai số nguyên có tổng bù cho phần tử bị loại bỏ không đảm bảo phương sai sẽ tồn tại. Khoảnh khắc thứ hai cũng phải được bảo tồn. 

Cuối cùng, ngay cả khi phân biệt đối xử là bình phương hoàn hảo thì tính chẵn lẻ của nó vẫn quan trọng. Nếu (q+r=s) và (q-r=d), thì 

[ 
q=\frac{s+d}{2},\qquad r=\frac{s-d}{2}. 
] 

Cả hai giá trị chỉ là số nguyên khi (s) và (d) có cùng tính chẵn lẻ. Chỉ kiểm tra xem phân biệt đối xử có phải là hình vuông hay không là không đủ. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp có thể thử mọi vị trí (p), xóa (a_p) và sau đó liệt kê các giá trị có thể có cho một trong các phần tử mới. Khi (p) và (q) được cố định, giá trị yêu cầu của (r) được xác định bằng giá trị trung bình, do đó mỗi ứng cử viên có thể được kiểm tra bằng cách tính toán lại hai số liệu thống kê. Ý tưởng này đúng vì mọi sự thay thế có thể cuối cùng đều được xem xét. 

Vấn đề là số lượng ứng viên. Các giá trị ban đầu có độ lớn tối đa là (10^5), trong khi các giá trị thay thế có thể lớn hơn một chút vì mảng mới có thêm một phần tử. Ngay cả việc hạn chế tìm kiếm trong một khoảng an toàn xung quanh (10^5) sẽ để lại thứ tự (10^5) ứng cử viên cho mỗi (10^5) vị trí hoặc khoảng (10^{10}) kiểm tra. Việc tính toán lại số liệu thống kê bên trong vòng lặp đó sẽ khiến nó trở nên tồi tệ hơn. 

Brute-force hoạt động vì số liệu thống kê áp đặt các hạn chế đại số mạnh mẽ, nhưng nó thất bại vì không khai thác chúng đủ sớm. Quan sát quan trọng là đẳng thức của giá trị trung bình ngay lập tức cố định tổng (q+r), trong khi đẳng thức của phương sai, cùng với giá trị trung bình bằng nhau, cố định (q^2+r^2). Khi đã biết cả tổng và tổng bình phương, (q) và (r) là hai nghiệm của phương trình bậc hai. Không cần phải tìm kiếm chúng. 

Giả sử chúng ta loại bỏ (x=a_p). Đặt giá trị trung bình ban đầu là (m) và đặt 

[ 
T=\frac{Q}{n}. 
] 

Bảo toàn giá trị trung bình mang lại 

[ 
\frac{S-x+q+r}{n+1}=m. 
] 

Vì (S=nm), điều này đơn giản hóa thành 

[ 
q+r=x+m. 
] 

Vì (x,q,r) là số nguyên nên điều này cho chúng ta biết rằng (m) phải là số nguyên. Nếu giá trị trung bình ban đầu không phải là số nguyên thì ngay lập tức câu trả lời là không thể. 

Bảo toàn phương sai tương đương với bảo toàn mômen thô thứ hai (Q/n), vì phương tiện đã bằng nhau. Do đó 

[ 
\frac{Q-x^2+q^2+r^2}{n+1}=\frac{Q}{n}, 
] 

mang lại 

[ 
q^2+r^2=x^2+\frac{Q}{n}. 
] 

Vì vậy (Q) cũng phải chia hết cho (n), vì vế trái và (x^2) là số nguyên. 

Bây giờ hãy 

[ 
s=q+r=x+m. 
] 

sử dụng 

[ 
(q-r)^2=2(q^2+r^2)-(q+r)^2, 
] 

chúng tôi có được 

2\left(x^2+\frac Qn\right)-(x+m)^2. 
] 

Đối với mọi phần tử có thể bị loại bỏ (x), giá trị này có thể được tính theo thời gian không đổi. Nếu nó không âm và là số chính phương (d^2) và (d) có cùng tính chẵn lẻ với (x+m), thì 

[ 
q=\frac{x+m+d}{2}, 
\qquad 
r=\frac{x+m-d}{2} 
] 

là số nguyên và tạo thành một sự thay thế hợp lệ. 

Do đó, toàn bộ vấn đề sẽ trở thành một lần quét duy nhất qua mảng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n\cdot V)), khoảng (10^{10}) ứng viên trong trường hợp xấu nhất | (O(1)) | Quá chậm | 
| Tối ưu | (O(n)) | (O(n)) cho mảng đầu vào | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng và tính toán 

[ 
S=\sum a_i,\qquad Q=\sum a_i^2. 
] 

Hai đại lượng này là đủ vì giá trị trung bình và phương sai đều có thể được biểu thị thông qua hai tổng lũy thừa đầu tiên. 

1. Kiểm tra xem (S) có chia hết cho (n) hay không. Nếu không thì in`Impossible`. 

Lý do đặc biệt đơn giản. Các giá trị thay thế là số nguyên, vì vậy (q+r) là số nguyên. Từ việc bảo toàn giá trị trung bình, 

[ 
q+r=x+\frac Sn. 
] 

Vì (x) là số nguyên nên (S/n) cũng phải là số nguyên. 

1. Đặt 

[ 
m=\frac Sn. 
] 

Sau đó kiểm tra xem (Q) có chia hết cho (n) hay không. Nếu không thì in`Impossible`. 

Đối với bất kỳ (x) bị loại bỏ, 

[ 
q^2+r^2=x^2+\frac Qn. 
] 

Vế trái và (x^2) là số nguyên nên (Q/n) phải là số nguyên. 

1. Đặt 

[ 
T=\frac Qn. 
] 

Quét mọi phần tử mảng (x=a_i) để tìm phần tử có thể cần loại bỏ. 

1. Với dòng điện (x), hãy tính 

[ 
s=x+m 
] 

và 

[ 
d^2=2(x^2+T)-s^2. 
] 

(Các) giá trị bị ép buộc bởi giá trị trung bình. Giá trị (d^2) bị ép buộc ở thời điểm thứ hai. 

1. Nếu (d^2<0) thì (x) này không thể loại bỏ được. Nếu không, hãy tính căn bậc hai số nguyên (d=\lfloor\sqrt{d^2}\rfloor) và kiểm tra xem (d^2=d\cdot d). 

Phân biệt đối xử âm có nghĩa là không tồn tại giá trị thực (q,r). Phân biệt đối xử không bình phương có nghĩa là hai nghiệm không cách nhau bởi hiệu số nguyên. 

1. Kiểm tra xem (các) và (d) có cùng tính chẵn lẻ hay không. 

Nếu không, các công thức 

[ 
q=\frac{s+d}{2},\qquad r=\frac{s-d}{2} 
] 

sẽ tạo ra các nửa số nguyên, điều này bị cấm. 

1. Xây dựng 

[ 
q=\frac{s+d}{2},\qquad r=\frac{s-d}{2} 
] 

và xuất ra chỉ số (i+1), theo sau là (q,r). 

Thứ tự của (q) và (r) không quan trọng vì chúng đóng góp một cách đối xứng vào cả hai số liệu thống kê cần thiết. 

1. Nếu quá trình quét kết thúc mà không tìm thấy phần tử hợp lệ, hãy in`Impossible`. 

### Tại sao nó hoạt động 

Đối với mọi ứng cử viên (x), thuật toán lấy tổng duy nhất có thể có (q+r) từ đẳng thức của phương tiện và giá trị duy nhất có thể có của (q^2+r^2) từ đẳng thức của các khoảnh khắc thứ hai. Do đó, bất kỳ cặp hợp lệ nào (q,r) đều phải đáp ứng giá trị tính toán của ((q-r)^2). Kiểm tra bình phương và chẵn lẻ chính xác là những điều kiện cần thiết để hai số nguyên có tổng và hiệu đó tồn tại. Do đó, mọi cặp được thuật toán tạo ra đều bảo toàn cả giá trị trung bình và phương sai. Ngược lại, bất kỳ sự thay thế hợp lệ nào cũng phải vượt qua tất cả các bước kiểm tra này, do đó, việc quét mọi phần tử có thể bị loại bỏ sẽ đảm bảo rằng một giải pháp hợp lệ được tìm thấy bất cứ khi nào có giải pháp đó. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    total = sum(a)
    squares = sum(x * x for x in a)

    # The new mean is the old mean.
    # Since q + r and a[p] are integers, total / n must be integer.
    if total % n != 0:
        print("Impossible")
        return

    mean = total // n

    # q^2 + r^2 = a[p]^2 + squares / n,
    # so squares / n must also be integer.
    if squares % n != 0:
        print("Impossible")
        return

    second_moment = squares // n

    for i, x in enumerate(a):
        s = x + mean

        # d^2 = (q-r)^2
        d2 = 2 * (x * x + second_moment) - s * s

        if d2 < 0:
            continue

        d = math.isqrt(d2)

        if d * d != d2:
            continue

        if (s - d) % 2 != 0:
            continue

        q = (s + d) // 2
        r = (s - d) // 2

        print("Possible")
        print(i + 1, q, r)
        return

    print("Impossible")

if __name__ == "__main__":
    solve()
```Hai tổng đầu tiên được tính một lần. Biến`total`đại diện cho tổng lũy ​​thừa đầu tiên, trong khi`squares`đại diện cho tổng lũy ​​thừa thứ hai. Cả hai đều có thể lớn, do đó việc tính toán được thực hiện bằng số nguyên Python thay vì dấu phẩy động. 

Việc kiểm tra khả năng phân chia có chủ ý xảy ra trước vòng lặp chính. Nếu như`total % n != 0`, không phần tử bị loại bỏ nào có thể hoạt động vì mọi ứng viên đều yêu cầu (q+r=x+m). Nếu như`squares % n != 0`, không ứng viên nào có thể làm việc vì (q^2+r^2=x^2+Q/n). 

Bên trong vòng lặp,`s`là giá trị bắt buộc của (q+r). biểu thức`d2`là giá trị bắt buộc của ((q-r)^2). sử dụng`math.isqrt`tránh các vấn đề về độ chính xác của dấu phẩy động khi kiểm tra xem một số nguyên lớn có phải là một hình vuông hoàn hảo hay không. 

Việc kiểm tra tính chẵn lẻ sử dụng`(s - d) % 2`. Đang kiểm tra`s-d`hoặc`s+d`là đủ vì chúng có cùng tính chẵn lẻ. Sau khi kiểm tra thành công, phép chia số nguyên sẽ tạo ra hai giá trị thay thế bắt buộc. 

Chỉ số đầu ra là`i + 1`, bởi vì mảng Python không được lập chỉ mục trong khi vấn đề đánh số tội phạm bắt đầu từ một. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
11
-5 -4 -3 -2 -1 0 1 2 3 4 5
```Các giá trị toàn cầu quan trọng là 

[ 
S=0,\qquad Q=110, 
] 

vậy 

[ 
m=0,\qquad T=10. 
] 

Quá trình quét hoạt động như sau. 

| Chỉ mục | (x) | (s=x+m) | (d^2) | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | -5 | -5 | 45 | Không phải là hình vuông | 
| 2 | -4 | -4 | 36 | Hợp lệ, (d=6) | 

Với (x=-4), 

[ 
q+r=-4 
] 

và 

[ 
q-r=6. 
] 

Do đó 

[ 
q=1,\qquad r=-5. 
] 

Mảng kết quả thu được bằng cách thay thế (-4) bằng (1,-5). Tổng của nó vẫn là (0) và tổng bình phương của nó trở thành 

[ 
110-16+1+25=120. 
] 

Hiện tại có (12) phần tử, vì vậy khoảnh khắc thứ hai là (120/12=10), chính xác là giá trị ban đầu. 

Do đó, đầu ra là```
Possible
2 1 -5
```Dấu vết này thể hiện tính bất biến trung tâm: sau khi loại bỏ (x), cả tổng mới và tổng bình phương mới đều được xác định hoàn toàn. 

### Xây dựng ví dụ 2 

Hãy xem xét```
1
5
```đây 

[ 
S=5,\qquad Q=25, 
] 

vậy 

[ 
m=5,\qquad T=25. 
] 

Chỉ có một yếu tố có thể loại bỏ. 

| Chỉ mục | (x) | (s=x+m) | (d^2) | (d) | (q,r) | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 5 | 10 | 0 | 0 | 5, 5 | 

Mảng kết quả là`[5, 5]`. Giá trị trung bình của nó vẫn là (5) và phương sai của nó vẫn bằng 0. 

Đầu ra có thể là```
Possible
1 5 5
```Trường hợp này thực hiện giá trị tối thiểu của (n) và xác nhận rằng phân biệt đối xử được phép bằng 0. Hai giá trị được chèn có thể giống hệt nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n)) | Các tổng được tính trong một lần và mỗi phần tử mảng được kiểm tra một lần. | 
| Không gian | (O(n)) | Mảng đầu vào được lưu trữ để có thể kiểm tra giá trị tội phạm đã chọn và chỉ mục của nó. | 

Đối với (n\le 10^5), thuật toán chỉ thực hiện một lượng số học nguyên không đổi cho mỗi phần tử. Không có tìm kiếm lồng nhau trên các giá trị thay thế và không có tính toán dấu phẩy động, do đó nó phù hợp thoải mái với các giới hạn dự định. 

## Trường hợp thử nghiệm 

Đầu ra của một giải pháp hợp lệ không phải là duy nhất, vì vậy các thử nghiệm bên dưới sẽ xác thực các điều kiện ngữ nghĩa thay vì yêu cầu một bộ ba hợp lệ chính xác. Trình trợ giúp sẽ xây dựng lại mảng kết quả và kiểm tra xem giá trị trung bình và phương sai của nó có khớp chính xác với mảng ban đầu hay không.```python
import sys
import io
import math

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    n = int(sys.stdin.readline())
    a = list(map(int, sys.stdin.readline().split()))

    total = sum(a)
    squares = sum(x * x for x in a)

    if total % n != 0 or squares % n != 0:
        ans = "Impossible\n"
        sys.stdin = old_stdin
        return ans

    mean = total // n
    second_moment = squares // n

    for i, x in enumerate(a):
        s = x + mean
        d2 = 2 * (x * x + second_moment) - s * s

        if d2 < 0:
            continue

        d = math.isqrt(d2)

        if d * d != d2:
            continue

        if (s - d) % 2 != 0:
            continue

        q = (s + d) // 2
        r = (s - d) // 2

        ans = f"Possible\n{i + 1} {q} {r}\n"
        sys.stdin = old_stdin
        return ans

    sys.stdin = old_stdin
    return "Impossible\n"

def validate(inp: str, out: str) -> bool:
    lines = out.strip().splitlines()

    n, *rest = inp.strip().splitlines()
    n = int(n)
    a = list(map(int, rest[0].split()))

    if lines[0] == "Impossible":
        # Independently check whether any solution exists by using
        # the same necessary-and-sufficient conditions.
        total = sum(a)
        squares = sum(x * x for x in a)

        if total % n != 0 or squares % n != 0:
            return True

        mean = total // n
        second_moment = squares // n

        for x in a:
            s = x + mean
            d2 = 2 * (x * x + second_moment) - s * s

            if d2 < 0:
                continue

            d = math.isqrt(d2)

            if d * d == d2 and (s - d) % 2 == 0:
                return False

        return True

    if lines[0] != "Possible" or len(lines) != 2:
        return False

    p, q, r = map(int, lines[1].split())

    if not (1 <= p <= n):
        return False

    b = a[:p - 1] + [q, r] + a[p:]

    old_sum = sum(a)
    new_sum = sum(b)

    old_sq = sum(x * x for x in a)
    new_sq = sum(x * x for x in b)

    return (
        new_sum * n == old_sum * (n + 1)
        and new_sq * n == old_sq * (n + 1)
    )

# Provided sample
sample1 = """\
11
-5 -4 -3 -2 -1 0 1 2 3 4 5
"""
out = run(sample1)
assert validate(sample1, out), "sample 1"

# Minimum-size input
case2 = """\
1
5
"""
out = run(case2)
assert validate(case2, out), "minimum-size case"

# All values equal
case3 = """\
5
7 7 7 7 7
"""
out = run(case3)
assert validate(case3, out), "all-equal case"

# Non-integer mean, immediately impossible
case4 = """\
2
0 1
"""
out = run(case4)
assert out.strip() == "Impossible", "non-integer mean"

# Large boundary values
case5 = """\
100000
""" + " ".join(["100000"] * 100000) + "\n"
out = run(case5)
assert validate(case5, out), "maximum-size boundary case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`11`theo sau là`-5 ... 5`|`Possible`với bất kỳ bộ ba hợp lệ nào | Cung cấp mẫu và dẫn xuất phân biệt | 
|`1 / 5`|`Possible`| Tối thiểu (n), không phân biệt đối xử | 
| Năm bản sao của`7`|`Possible`| Giá trị hoàn toàn bằng nhau | 
|`2 / 0 1`|`Impossible`| Giá trị gốc không nguyên | 
| (10^5) bản sao của`100000`|`Possible`| Kích thước đầu vào tối đa, tổng lớn và bình phương | 

Trình xác thực cố tình tránh so sánh bộ ba đầu ra chính xác. Vấn đề này yêu cầu bất kỳ sự thay thế hợp lệ nào, vì vậy cả hai câu trả lời khác nhau đều có thể đúng. 

## Vỏ cạnh 

Đối với đầu vào kích thước tối thiểu```
1
5
```thuật toán tính toán (m=5) và (T=25). Đối với giá trị bị loại bỏ duy nhất có thể (x=5), 

[ 
s=10 
] 

và 

[ 
d^2=2(25+25)-100=0. 
] 

Do đó (d=0) và cả hai giá trị được chèn đều là (5). Đầu ra là`Possible`, theo yêu cầu. 

Đối với một giá trị không nguyên như```
2
0 1
```chúng ta có (S=1) và (n=2), vì vậy (S\bmod n=1). Thuật toán in ngay lập tức`Impossible`. Điều này mạnh hơn phép so sánh dấu phẩy động vì nó chứng minh rằng không có số nguyên (q+r) nào có thể thỏa mãn giá trị trung bình được yêu cầu. 

Đối với một mảng hoàn toàn bằng nhau như```
5
7 7 7 7 7
```chúng ta có (m=7) và (T=49). Loại bỏ bất kỳ (x=7) nào sẽ cho ra (s=14) và (d^2=0). Thuật toán chèn (7,7), bảo toàn phương sai bằng 0. 

Kiểm tra bình phương hoàn hảo là một ranh giới quan trọng khác. Trong mẫu, loại bỏ (-4) sẽ cho (d^2=36), do đó (d=6) và nghiệm là số nguyên. Thay vào đó, nếu một ứng cử viên tạo ra (d^2=35), hai nghiệm sẽ không tách rời ngay cả khi phân biệt đối xử dương. Thuật toán từ chối nó vì`isqrt(35)`là (5) và (5^2\ne35). 

Tính chẵn lẻ là một điều kiện riêng biệt. Giả sử các giá trị được tính là (s=5) và (d=2). Căn bậc hai sẽ là 

[ 
\frac{5+2}{2}=\frac72,\qquad 
\frac{5-2}{2}=\frac32, 
] 

vì vậy không tồn tại sự thay thế số nguyên. Thuật toán loại bỏ ứng viên này vì (s-d=3) là số lẻ. 

Các giá trị lớn nhất cũng vẫn an toàn. Với (10^5) phần tử, mỗi phần tử bằng (10^5), 

[ 
S=10^{10} 
] 

và 

[ 
Q=10^{15}. 
] 

Cả hai đều phù hợp thoải mái với các số nguyên có độ chính xác tùy ý của Python. Quan trọng hơn, thuật toán không bao giờ xây dựng phương sai đầy đủ dưới dạng số dấu phẩy động, do đó không có sự mất mát về độ chính xác khi so sánh hai số liệu thống kê bằng nhau về mặt lý thuyết.
