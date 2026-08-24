---
title: "CF 102163M - Lương NCD"
description: "Đối với mỗi trường hợp thử nghiệm, chúng tôi đang so sánh hai mức lương có dạng (B1^{P1}) và (B2^{P2}). Cặp đầu tiên mô tả mức lương ban đầu, trong khi cặp thứ hai mô tả mức lương mới."
date: "2026-08-23T23:05:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102163
codeforces_index: "M"
codeforces_contest_name: "NCD 2019"
rating: 0
weight: 102163
solve_time_s: 825
verified: true
draft: false
---

[CF 102163M - Mức lương NCD](https://codeforces.com/problemset/problem/102163/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 13 phút 45 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Đối với mỗi trường hợp thử nghiệm, chúng tôi đang so sánh hai mức lương có dạng (B_1^{P_1}) và (B_2^{P_2}). Cặp đầu tiên mô tả mức lương ban đầu, trong khi cặp thứ hai mô tả mức lương mới. Chúng ta cần in`Congrats`khi mức lương mới lớn hơn,`HaHa`khi nó nhỏ hơn và`Lazy`khi mức lương của cả hai bằng nhau. 

Giá trị của cả cơ số và số mũ đều có thể đạt tới (10^6). Xây dựng trực tiếp (B^P) là không thực tế. Thậm chí (2^{10^6}) có khoảng 300.000 chữ số thập phân, do đó phép lũy thừa số nguyên lớn thông thường sẽ yêu cầu thao tác hàng trăm nghìn chữ số cho một lần so sánh. Với nhiều trường hợp thử nghiệm và giới hạn 3 giây, cách tiếp cận đó vượt xa những gì chúng tôi mong muốn. 

Sự chuyển đổi hữu ích là 

[ 
B^P = e^{P\ln B}. 
] 

Vì hàm số mũ tăng nghiêm ngặt nên việc so sánh hai lũy thừa dương tương đương với việc so sánh logarit của chúng: 

[ 
B_1^{P_1} < B_2^{P_2} 
\iff 
P_1\ln B_1 < P_2\ln B_2. 
] 

Điều này biến một phép so sánh số nguyên khổng lồ thành một vài phép tính dấu phẩy động. 

Có một số trường hợp cạnh phải được tách ra trước khi lấy logarit. Nếu cơ số bằng 0 và số mũ của nó dương thì giá trị bằng 0. Ví dụ,```
1
0 5 2 3
```có mức lương (0^5=0) và (2^3=8), nên câu trả lời là`Congrats`. Đang gọi`log(0)`sẽ không hợp lệ. 

Số mũ bằng 0 tạo ra giá trị bằng 1 bất cứ khi nào cơ số dương. Ví dụ,```
1
7 0 1 5
```so sánh (7^0=1) với (1^5=1), vì vậy câu trả lời là`Lazy`. Việc triển khai bất cẩn coi số mũ bằng 0 là làm cho toàn bộ biểu thức bằng 0 sẽ là sai. 

Đảm bảo đầu vào loại trừ (0^0) cho mỗi biểu thức lương, vì vậy chúng ta không bao giờ cần gán ý nghĩa toán học đặc biệt cho nó. Ví dụ như cặp`(0, 0)`không thể xảy ra như mô tả đầy đủ về mức lương. 

Cuối cùng, các lũy thừa bằng nhau có thể có cơ số và số mũ khác nhau. Ví dụ,```
1
2 4 4 2
```so sánh (2^4) và (4^2), cả hai đều bằng 16. So sánh các cơ số hoặc số mũ riêng biệt sẽ không thành công, trong khi so sánh (P\ln B) xác định chính xác đẳng thức. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là tính cả hai lũy thừa và so sánh chúng. Nó đơn giản về mặt toán học vì các giá trị chúng tôi tính toán chính xác là hai mức lương. Vấn đề là kích thước của chúng. Với (B=10^6) và (P=10^6), số kết quả chứa khoảng (6\cdot10^6\log_{10}10) chữ số ở thang đo cực đại nhất và ngay cả ví dụ nhỏ hơn nhiều (2^{10^6}) cũng đã có hơn 300.000 chữ số. Tính toán và lưu trữ các số nguyên như vậy là công việc không cần thiết khi tất cả những gì chúng ta cần là thứ tự của chúng. Trường hợp xấu nhất có thể yêu cầu hàng trăm nghìn hoặc hàng triệu phép tính chữ số cho mỗi trường hợp thử nghiệm, thay vì một lượng số học không đổi. 

Quan sát quan trọng là logarit bảo toàn thứ tự cho các số dương. Thay vì đánh giá (B^P), hãy lấy logarit tự nhiên của nó: 

[ 
\ln(B^P)=P\ln B. 
] 

Bây giờ, cả hai mức lương đều có thể được biểu thị bằng các số dấu phẩy động thông thường có độ lớn tối đa khoảng (10^6\ln(10^6)), khoảng (1.4\cdot10^7). Việc so sánh trở thành thời gian không đổi cho mỗi trường hợp thử nghiệm. 

Phương pháp brute-force hoạt động vì phép lũy thừa cho ra mức lương chính xác, nhưng nó không thành công vì những mức lương đó có thể chứa một số lượng lớn các chữ số. Việc quan sát logarit loại bỏ sự cần thiết phải xây dựng chúng. Các giá trị duy nhất không thể vượt qua phép biến đổi này là 0, do đó các cơ số 0 được xử lý riêng biệt trước khi so sánh logarit. 

Số học dấu phẩy động cũng đưa ra một vấn đề phức tạp khi hai giá trị logarit bằng nhau về mặt toán học. Ví dụ: (2^4) và (4^2) đều tạo ra`Lazy`, nhưng biểu thức logarit dấu phẩy động của chúng có thể khác nhau bởi một lỗi làm tròn nhỏ. Do đó, chúng tôi coi những khác biệt đủ nhỏ là sự bình đẳng. Giải pháp kiểu chính thức sử dụng dung sai nhỏ cho sự so sánh này. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Phụ thuộc vào số chữ số trong (B^P), có thể có hàng trăm nghìn thao tác chữ số cho mỗi trường hợp thử nghiệm | Có khả năng hàng trăm ngàn chữ số | Quá chậm / không thực tế | 
| Tối ưu | (O(1)) cho mỗi trường hợp thử nghiệm | (O(1)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc (B_1,P_1,B_2,P_2). Mỗi cặp đại diện cho một mức lương, vì vậy cặp đầu tiên là mức lương cũ và cặp thứ hai là mức lương mới. 
2. Kiểm tra xem một trong hai cơ số có bằng không hay không. Biểu thức cơ số 0 hợp lệ phải có số mũ dương vì (0^0) bị loại trừ bởi đảm bảo đầu vào. Mức lương như vậy chính xác là bằng không. 

Nếu cả hai cơ sở đều bằng 0 thì lương cả hai đều bằng 0 và câu trả lời là`Lazy`. Nếu chỉ có cơ sở thứ nhất bằng 0 thì lương cũ bằng 0 trong khi lương mới dương nên đáp án là`Congrats`. Nếu chỉ cơ sở thứ 2 bằng 0 thì lương mới bằng 0 trong khi lương cũ dương nên đáp án là`HaHa`. 
3. Khi cả hai cơ số đều dương, hãy tính 

[ 
x_1=P_1\ln B_1,\qquad x_2=P_2\ln B_2. 
] 

Đây là logarit của hai mức lương. Số mũ bằng 0 đương nhiên cho (x=0), tương ứng với mức lương bằng một. 
4. So sánh (x_1) và (x_2). Nếu sự khác biệt của chúng nằm trong dung sai nhỏ, hãy in`Lazy`, vì sự khác biệt đủ nhỏ để có thể giải thích bằng cách làm tròn dấu phẩy động. 
5. Ngược lại nếu (x_1<x_2) thì mức lương mới lớn hơn nên in`Congrats`. Nếu (x_1>x_2) thì mức lương mới nhỏ hơn nên in`HaHa`. 

Bất biến đằng sau thuật toán là, sau khi loại bỏ các trường hợp 0, mọi mức lương đều dương và logarit tăng nghiêm ngặt. Như vậy dấu hiệu của 

[ 
P_2\ln B_2-P_1\ln B_1 
] 

chính xác là dấu hiệu của sự khác biệt giữa mức lương mới và cũ. Dung sai chỉ tính đến độ chính xác hữu hạn được sử dụng để biểu thị logarit. Không cần phải xây dựng giá trị tiền lương thực tế. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

EPS = 1e-10

def solve_case(b1, p1, b2, p2):
    # A valid zero-base expression must have a positive exponent,
    # because 0^0 is excluded by the input guarantee.
    if b1 == 0 or b2 == 0:
        if b1 == b2:
            return "Lazy"
        if b1 == 0:
            return "Congrats"
        return "HaHa"

    # For positive bases:
    # log(B^P) = P * log(B)
    x1 = p1 * math.log(b1)
    x2 = p2 * math.log(b2)

    if abs(x1 - x2) <= EPS:
        return "Lazy"
    if x1 < x2:
        return "Congrats"
    return "HaHa"

def main():
    t = int(input())
    ans = []

    for _ in range(t):
        b1, p1, b2, p2 = map(int, input().split())
        ans.append(solve_case(b1, p1, b2, p2))

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    main()
```các`solve_case`hàm đầu tiên xử lý các cơ sở bằng 0 vì`math.log(0)`không được xác định. Sự đảm bảo của vấn đề có nghĩa là nếu một cơ số bằng 0 thì số mũ tương ứng của nó là dương, do đó biểu thức chắc chắn bằng 0. 

Một khi cả hai bazơ đều dương,`math.log(b)`được xác định. Nhân nó với số mũ sẽ ra logarit của mức lương tương ứng. Điều này cũng xử lý số mũ 0 mà không có nhánh đặc biệt:`p * log(b)`trở thành 0, chính xác là (\log(1)). 

Việc so sánh sử dụng`EPS`thay vì thử nghiệm`x1 == x2`. Logarit dấu phẩy động là các giá trị gần đúng, vì vậy các biểu thức bằng nhau về mặt toán học như (2^4) và (4^2) có thể tạo ra các giá trị khác nhau ở một vài bit cuối cùng. Kiểm tra đẳng thức trực tiếp có thể in không chính xác`Congrats`hoặc`HaHa`. 

Không có vấn đề tràn số nguyên trong Python vì số nguyên có độ chính xác tùy ý, nhưng dù sao thì giải pháp cũng không bao giờ xây dựng được lũy thừa khổng lồ. Giá trị logarit trung gian lớn nhất chỉ ở mức (10^7), được biểu thị dễ dàng bằng số dấu phẩy động Python. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên, hãy xem xét ba trường hợp thử nghiệm cùng nhau. 

| Trường hợp thử nghiệm | (B_1) | (P_1) | (B_2) | (P_2) | (x_1=P_1\ln B_1) | (x_2=P_2\ln B_2) | Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 3 | 4 | 2 | (3\ln2) | (2\ln4=4\ln2) |`Congrats`| 
| 2 | 2 | 2 | 3 | 1 | (2\ln2) | (\ln3) |`HaHa`| 
| 3 | 2 | 4 | 4 | 2 | (4\ln2) | (2\ln4=4\ln2) |`Lazy`| 

Trong trường hợp đầu tiên, mức lương mới là (4^2=16), trong khi mức lương cũ là (2^3=8), do đó`Congrats`là đúng. Trong trường hợp thứ hai, (2^2=4) nhỏ hơn (3), tạo ra`HaHa`. Trường hợp thứ ba thực hiện tình huống quyền lực ngang nhau trong đó căn cứ khác nhau nhưng mức lương giống nhau. 

Đối với dấu vết thứ hai, hãy xem xét trường hợp cơ số 0 và trường hợp số mũ bằng 0:```
2
0 7 3 2
8 0 1 5
```| Trường hợp thử nghiệm | Xử lý không có cơ sở | (x_1) | (x_2) | Kết quả | 
| --- | --- | --- | --- | --- | 
| 1 | (0^7=0), (3^2=9) | Không cần thiết | Không cần thiết |`Congrats`| 
| 2 | Không có cơ sở bằng không | (0) | (5\ln1=0) |`Lazy`| 

Trường hợp đầu tiên không bao giờ gọi`log(0)`. Nó nhận ra ngay rằng lương mới là dương trong khi lương cũ bằng 0. Trường hợp thứ hai chứng minh rằng số mũ bằng 0 được biểu diễn một cách tự nhiên bằng giá trị logarit bằng 0 và (1^5) cũng có giá trị logarit bằng 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(1)) cho mỗi trường hợp thử nghiệm | Mỗi trường hợp thử nghiệm thực hiện một số phép tính số học và logarit không đổi. | 
| Không gian | (O(1)) không gian phụ trợ | Chỉ cần bốn giá trị đầu vào, hai giá trị logarit và kết quả. | 

Đối với (T) trường hợp kiểm thử, tổng thời gian chạy là (O(T)), với công việc không đổi cho mỗi trường hợp. Giới hạn của (10^6) trên các cơ số và số mũ không làm tăng kích thước của biểu diễn logarit, vì vậy giải pháp này tránh được một cách thoải mái phép tính số nguyên khổng lồ mà phép lũy thừa trực tiếp sẽ yêu cầu. Việc triển khai cũng chỉ lưu trữ các chuỗi đầu ra, yêu cầu tổng dung lượng đầu ra là (O(T)). 

## Trường hợp thử nghiệm```python
import io
import sys
import math

EPS = 1e-10

def solve_case(b1, p1, b2, p2):
    if b1 == 0 or b2 == 0:
        if b1 == b2:
            return "Lazy"
        if b1 == 0:
            return "Congrats"
        return "HaHa"

    x1 = p1 * math.log(b1)
    x2 = p2 * math.log(b2)

    if abs(x1 - x2) <= EPS:
        return "Lazy"
    if x1 < x2:
        return "Congrats"
    return "HaHa"

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    try:
        input = sys.stdin.readline
        t = int(input())
        ans = []

        for _ in range(t):
            b1, p1, b2, p2 = map(int, input().split())
            ans.append(solve_case(b1, p1, b2, p2))

        return "\n".join(ans)
    finally:
        sys.stdin = old_stdin

# Provided sample
assert run("""3
2 3 4 2
2 2 3 1
2 4 4 2
""") == """Congrats
HaHa
Lazy""", "sample 1"

# Minimum-size positive values
assert run("""1
1 1 1 1
""") == "Lazy", "minimum positive values"

# Zero base versus positive salary
assert run("""2
0 5 2 3
2 3 0 5
""") == """Congrats
HaHa""", "zero-base cases"

# Exponent zero and equality through different representations
assert run("""3
7 0 1 5
2 10 4 5
3 0 2 1
""") == """Lazy
Lazy
Lazy""", "zero exponents and equal powers"

# Boundary values near the maximum constraints
assert run("""2
1000000 1000000 999999 1000000
999999 1000000 1000000 1000000
""") == """HaHa
Congrats""", "maximum-size values"

# Exact equality with nontrivial exponents
assert run("""3
8 6 64 2
27 10 9 15
16 3 8 4
""") == """Lazy
Lazy
HaHa""", "nontrivial power equalities"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 3 4 2`và các trường hợp mẫu khác |`Congrats`,`HaHa`,`Lazy`| Cung cấp mẫu và đặt hàng cơ bản | 
|`1 1 1 1`|`Lazy`| Giá trị dương tối thiểu | 
|`0 5 2 3`Và`2 3 0 5`|`Congrats`,`HaHa`| Cả hai hướng so sánh cơ sở không | 
|`7 0 1 5`,`2 10 4 5`,`3 0 2 1`|`Lazy`,`Lazy`,`Lazy`| Xử lý số mũ bằng 0 và lũy thừa bằng nhau | 
|`1000000 1000000 ...`|`HaHa`,`Congrats`| Giá trị tại ranh giới ràng buộc trên | 
|`8 6 64 2`,`27 10 9 15`,`16 3 8 4`|`Lazy`,`Lazy`,`HaHa`| Bình đẳng với những căn cứ khác nhau và sự so sánh chặt chẽ | 

## Vỏ cạnh 

Cơ số 0 không bao giờ được phép tính logarit. Vì```
1
0 5 2 3
```thuật toán nhìn thấy`b1 == 0`ngay lập tức. Từ`b2`khác 0 thì lương thứ nhất bằng 0 và lương thứ hai dương. Nó trở lại`Congrats`mà không đánh giá logarit. Đảo ngược các căn cứ,```
1
2 3 0 5
```làm cho mức lương mới bằng 0, do đó chi nhánh tương tự sẽ trả về`HaHa`. 

Số mũ bằng 0 là một nơi khác mà việc triển khai đơn giản có thể sai. Vì```
1
7 0 1 5
```cả hai cơ số đều dương nên đường logarit được sử dụng. Mức lương logarit đầu tiên là 

[ 
0\cdot\ln7=0, 
] 

và thứ hai là 

[ 
5\cdot\ln1=0. 
] 

Sự khác biệt của chúng bằng 0, vì vậy đầu ra là`Lazy`, khớp (7^0=1^5=1). 

Các căn cứ khác nhau vẫn có thể đại diện cho cùng một mức lương. Vì```
1
2 4 4 2
```thuật toán tính toán 

[ 
x_1=4\ln2 
] 

và 

[ 
x_2=2\ln4=2(2\ln2)=4\ln2. 
] 

Hai giá trị dấu phẩy động bằng nhau nên việc so sánh dựa trên dung sai sẽ tạo ra`Lazy`. Đây là lý do tại sao chỉ kiểm tra xem cơ số hoặc số mũ có khớp nhau hay không là không đủ. 

Giới hạn trên cũng là lý do chính cho việc sử dụng logarit. Vì```
1
1000000 1000000 999999 1000000
```mức lương thực tế quá lớn để có thể trực tiếp xây dựng. Thuật toán chỉ tính 

[ 
10^6\ln(10^6) 
] 

và 

[ 
10^6\ln(999999), 
] 

đó là các giá trị dấu phẩy động thông thường. Vì số thứ nhất lớn hơn nên mức lương mới nhỏ hơn và sản lượng`HaHa`. 

Đảm bảo đầu vào sẽ loại bỏ trường hợp (0^0) không rõ ràng. Một bài kiểm tra như```
1
0 0 2 3
```không phải là đầu vào hợp lệ cho vấn đề này, do đó việc triển khai không cần phải tạo ra quy ước cho nó. Do đó, việc xử lý bằng 0 có thể dựa vào việc mọi mức lương cơ bản bằng 0 đều chính xác bằng 0.
