---
title: "CF 106045G - GCD so với LCM"
description: "Chúng ta cần xây dựng một mảng gồm n số nguyên dương với một thuộc tính rất cụ thể. Gọi G là gcd của toàn bộ mảng. Với mọi vị trí i, nếu chúng ta loại bỏ phần tử thứ i và tính gcd của n - 1 số còn lại thì gcd đó phải lớn hơn G."
date: "2026-06-25T12:42:25+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106045
codeforces_index: "G"
codeforces_contest_name: "IUT Intra University Programming Contest 2025"
rating: 0
weight: 106045
solve_time_s: 47
verified: true
draft: false
---

[CF 106045G - GCD so với LCM](https://codeforces.com/problemset/problem/106045/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta cần xây dựng một mảng`n`số nguyên dương có tính chất rất cụ thể. 

Cho phép`G`là gcd của toàn bộ mảng. Đối với mọi vị trí`i`, nếu chúng ta loại bỏ`i`-phần tử thứ và tính gcd của phần tử còn lại`n - 1`số, thì gcd đó phải lớn hơn rất nhiều so với`G`. 

Đồng thời, lcm của toàn mảng không được vượt quá giới hạn nhất định`l`. 

Đối với mỗi trường hợp thử nghiệm, chúng tôi xuất ra bất kỳ mảng hợp lệ nào hoặc báo cáo rằng không tồn tại mảng đó. 

Đầu vào chứa tối đa`10^5`trường hợp thử nghiệm và tổng của tất cả`n`giá trị nhiều nhất là`2 · 10^5`. Điều đó ngay lập tức gợi ý rằng chúng ta có đủ khả năng chi trả cho công việc tương ứng với`n`cho mỗi trường hợp thử nghiệm, nhưng bất cứ điều gì bậc hai trong`n`sẽ là quá đắt. 

Sự ràng buộc`l ≤ 10^18`là hạn chế thú vị nhất. Vì câu trả lời chỉ phụ thuộc vào cấu trúc chia hết nên chúng ta mong đợi một cách xây dựng theo lý thuyết số có lcm nằm trong giới hạn này. 

Phần nguy hiểm của vấn đề là điều kiện phải được giữ cho mọi phần tử bị loại bỏ. Một công trình chỉ hoạt động cho một số vị trí là không hợp lệ. 

Coi như`n = 3`và mảng:```
6 10 15
```Gcd của tất cả các số là`1`. 

Đang xóa`6`lá`{10,15}`, gcd của ai là`5`. 

Đang xóa`10`lá`{6,15}`, gcd của ai là`3`. 

Đang xóa`15`lá`{6,10}`, gcd của ai là`2`. 

Tất cả đều lớn hơn`1`, vì vậy mảng này thú vị. 

Bây giờ hãy xem xét:```
2 4 8
```Gcd của tất cả các số là`2`. 

Đang xóa`8`lá`{2,4}`, gcd của ai vẫn là`2`. 

Điều kiện yêu cầu tăng _strict_ nên mảng này không hợp lệ. 

Một sai lầm dễ mắc phải khác là cho rằng việc lặp lại các con số sẽ có ích. Ví dụ:```
6 6 6
```gcd của toàn bộ mảng là`6`và xóa bất kỳ phần tử nào vẫn để lại gcd`6`. Tình trạng thất bại ở khắp mọi nơi. 

Những ví dụ này cho thấy rằng mọi phần tử phải chịu trách nhiệm loại bỏ một số ước chung mà tất cả các phần tử khác chia sẻ. 

## Phương pháp tiếp cận 

Tư duy vũ phu bắt đầu bằng cách cố gắng trực tiếp thỏa mãn định nghĩa. 

Chúng ta có thể tạo các mảng ứng cử viên, tính gcd của toàn bộ mảng, sau đó, với mỗi vị trí, hãy tính gcd của các phần tử còn lại và xác minh điều kiện. Điều này đúng về mặt kiểm tra nhưng hoàn toàn không thực tế khi xét về chiến lược xây dựng. Không gian tìm kiếm rất lớn và thậm chí việc kiểm tra nhiều ứng viên cũng sẽ vô vọng. 

Điều quan trọng là phải hiểu điều kiện ngụ ý gì về mặt cấu trúc. 

Đặt gcd của toàn bộ mảng là`G`. Chia mọi phần tử cho`G`. Sau quá trình chuẩn hóa này, gcd của toàn bộ mảng sẽ trở thành`1`. 

Bây giờ tập trung vào một vị trí cố định`i`. 

Kể từ khi loại bỏ`a[i]`phải tăng gcd thì các số còn lại có ước chung nào đó lớn hơn`1`. Cho phép`p_i`là bất kỳ thừa số nguyên tố nào của gcd đó. 

Sau đó`p_i`chia mọi phần tử ngoại trừ`a[i]`. 

Bởi vì gcd của toàn bộ mảng chuẩn hóa là`1`, cùng số nguyên tố đó không thể chia được`a[i]`. Nếu không nó sẽ chia mọi phần tử. 

Vì vậy mỗi vị trí`i`có số nguyên tố`p_i`phân chia tất cả các phần tử ngoại trừ`i`-thứ 1 

Sau đây là một sự thật thậm chí còn mạnh mẽ hơn. Những số nguyên tố này phải khác biệt. 

Giả định`p_i = p_j = p`. 

Từ`p`chia tất cả các phần tử ngoại trừ`a[i]`, nó chia`a[j]`. 

Từ`p`chia tất cả các phần tử ngoại trừ`a[j]`, nó chia`a[i]`. 

Kể từ đây`p`chia mọi phần tử, mâu thuẫn với thực tế là gcd chuẩn hóa là`1`. 

Vì vậy, bất kỳ mảng độ dài thú vị nào`n`yêu cầu ít nhất`n`số nguyên tố riêng biệt. 

Vì mọi số nguyên tố như vậy đều phải chia lcm nên ta có điều kiện cần:$$\text{LCM} \ge p_1 p_2 \cdots p_n$$Sản phẩm nhỏ nhất có thể có của`n`các số nguyên tố riêng biệt thu được bằng cách lấy số đầu tiên`n`số nguyên tố:$$2 \cdot 3 \cdot 5 \cdot 7 \cdots$$Nếu sản phẩm đó đã vượt quá`l`, không có giải pháp nào có thể tồn tại. 

Quan sát này cũng đưa ra cách xây dựng. 

Cho phép$$P = p_1 p_2 \cdots p_n$$Ở đâu`p_1, ..., p_n`là những người đầu tiên`n`số nguyên tố. 

Định nghĩa$$a_i = \frac{P}{p_i}.$$Sau đó: 

- Mọi số nguyên tố ngoại trừ`p_i`xuất hiện ở`a_i`. 
- Gcd của mọi số là`1`. 
- Loại bỏ`a_i`để lại các số chia hết cho`p_i`, do đó gcd trở thành ít nhất`p_i > 1`. 
- Lcm chính xác đấy`P`. 

Do đó việc xây dựng có giá trị bất cứ khi nào`P ≤ l`. 

Từ`l ≤ 10^18`, tích của các số nguyên tố đầu tiên vượt quá giới hạn rất nhanh. Trong thực tế chỉ có mười lăm số nguyên tố đầu tiên phù hợp:$$2 \cdot 3 \cdot 5 \cdots 47
= 614889782588491410.$$Nhân với số nguyên tố tiếp theo`53`đã vượt quá`10^{18}`. Vì vậy đối với`n > 15`, một giải pháp tự động là không thể. 

Toàn bộ vấn đề quy về việc kiểm tra xem sản phẩm đầu tiên có`n`nhiều nhất là số nguyên tố`l`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tìm kiếm vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Xây dựng sản phẩm Prime | O(n) cho mỗi trường hợp thử nghiệm | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính trước mười lăm số nguyên tố đầu tiên. 
2. Tính toán trước các sản phẩm tiền tố của họ. 

Tích của mười lăm số nguyên tố đầu tiên vẫn nằm trong`10^18`, vì vậy số học 64 bit là đủ. 
3. Đối với test case có giá trị`n`:

Nếu như`n > 15`, đầu ra`-1`. 

Không có mảng thú vị nào có thể tồn tại bởi vì bất kỳ mảng hợp lệ nào cũng yêu cầu ít nhất`n`các số nguyên tố phân biệt trong lcm của nó. 
4. Hãy để`P`là sản phẩm đầu tiên`n`số nguyên tố. 
5. Nếu`P > l`, đầu ra`-1`. 

Ngay cả lcm tối thiểu có thể có của một mảng thú vị cũng sẽ vượt quá giới hạn cho phép. 
6. Xây dựng mảng$$a_i = P / p_i.$$7. Xuất mảng đã xây dựng. 

### Tại sao nó hoạt động 

Sau khi chuẩn hóa, mỗi vị trí phải tương ứng với một số nguyên tố riêng biệt chia hết các phần tử khác và không chia phần tử của vị trí đó. Điều này buộc lcm phải chứa ít nhất`n`số nguyên tố riêng biệt. Lcm nhỏ nhất có thể đạt được bằng cách sử dụng giá trị đầu tiên`n`số nguyên tố. 

Việc xây dựng đạt được chính xác giới hạn dưới đó. Gcd của tất cả các phần tử là`1`bởi vì mọi số nguyên tố đều bị thiếu trong một phần tử. Khi phần tử`i`được loại bỏ, số nguyên tố`p_i`xuất hiện trong mọi phần tử còn lại, do đó gcd của tập hợp còn lại ít nhất là`p_i > 1`. Do đó mỗi lần loại bỏ đều làm tăng gcd. 

Vì lcm của công trình chính xác là`P`, nó thỏa mãn giới hạn một cách chính xác khi`P ≤ l`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

primes = [2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47]

pref = [1]
for p in primes:
    pref.append(pref[-1] * p)

t = int(input())
ans = []

for _ in range(t):
    n, l = map(int, input().split())

    if n > 15:
        ans.append("-1")
        continue

    P = pref[n]

    if P > l:
        ans.append("-1")
        continue

    arr = [str(P // p) for p in primes[:n]]
    ans.append(" ".join(arr))

sys.stdout.write("\n".join(ans))
```Phần đầu tiên tính toán trước 15 số nguyên tố đầu tiên và tích các tiền tố của chúng. Điều này cho phép mọi trường hợp thử nghiệm trả lời câu hỏi về sự tồn tại trong thời gian không đổi. 

Séc`n > 15`là điều cần thiết. Từ`l`không bao giờ vượt quá`10^18`, lcm không thể chứa nhiều hơn 15 số nguyên tố riêng biệt. Bất kỳ mảng thú vị nào cũng cần ít nhất`n`các số nguyên tố khác nhau, làm cho các giá trị lớn hơn không thể được. 

Việc xây dựng chính nó là đơn giản. Nếu như`P`là sản phẩm đầu tiên`n`số nguyên tố, mỗi phần tử có được bằng cách loại bỏ chính xác một thừa số nguyên tố khỏi`P`. 

Không cần tính toán gcd hoặc lcm trong quá trình thực thi. Bằng chứng toán học đảm bảo tính chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1
3 500
```Ba số nguyên tố đầu tiên là`2, 3, 5`.`P = 30`. 

| tôi | Thủ tướng đã xóa | Giá trị | 
| --- | --- | --- | 
| 1 | 2 | 15 | 
| 2 | 3 | 10 | 
| 3 | 5 | 6 | 

Đầu ra:```
15 10 6
```Xác minh: 

| Phần tử đã xóa | Số Còn Lại | GCD | 
| --- | --- | --- | 
| 15 | 10, 6 | 2 | 
| 10 | 15, 6 | 3 | 
| 6 | 15, 10 | 5 | 

Gcd của tất cả các số là`1`và mỗi lần loại bỏ đều làm tăng nó. 

### Ví dụ 2 

đầu vào:```
1
4 200
```Bốn số nguyên tố đầu tiên cho:$$P = 2 \cdot 3 \cdot 5 \cdot 7 = 210.$$Từ`210 > 200`, không có mảng hợp lệ nào có thể tồn tại. 

| n | LCM tối thiểu có thể có | 
| --- | --- | 
| 4 | 210 | 

Đầu ra:```
-1
```Điều này thể hiện đối số giới hạn dưới. Ngay cả công trình tốt nhất có thể cũng vượt quá giới hạn cho phép. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Xuất ra`n`những con số thống trị công việc | 
| Không gian | O(n) | Lưu trữ mảng đã xây dựng | 

Tổng của tất cả`n`giá trị nhiều nhất là`2 · 10^5`, do đó tổng kích thước đầu ra là nút cổ chai thực sự. Thuật toán chỉ thực hiện số học đơn giản và dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    primes = [2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47]

    pref = [1]
    for p in primes:
        pref.append(pref[-1] * p)

    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n, l = map(int, input().split())

        if n > 15:
            out.append("-1")
            continue

        P = pref[n]

        if P > l:
            out.append("-1")
            continue

        out.append(" ".join(str(P // p) for p in primes[:n]))

    return "\n".join(out)

# sample-like cases
assert run("1\n3 500\n") == "15 10 6"

assert run("1\n4 200\n") == "-1"

# minimum n
assert run("1\n2 6\n") == "3 2"

# exact boundary
assert run("1\n4 210\n") == "105 70 42 30"

# impossible because n > 15
assert run("1\n16 1000000000000000000\n") == "-1"

# l too small
assert run("1\n3 29\n") == "-1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 6`|`3 2`| Kích thước hợp lệ nhỏ nhất | 
|`4 210`| Mảng được xây dựng | Ranh giới lcm chính xác | 
|`16 10^18`|`-1`| Cần có hơn mười lăm số nguyên tố riêng biệt | 
|`3 29`|`-1`| Lcm tối thiểu có thể đã vượt quá giới hạn | 

## Vỏ cạnh 

Hãy xem xét:```
1
2 5
```Vì`n = 2`, lcm nhỏ nhất có thể là`2 × 3 = 6`. 

Từ`6 > 5`, thuật toán đầu ra:```
-1
```Điều này đúng vì bất kỳ mảng thú vị nào có độ dài hai đều yêu cầu hai số nguyên tố riêng biệt trong lcm của nó. 

Bây giờ hãy xem xét:```
1
15 1000000000000000000
```Tích của 15 số nguyên tố đầu tiên là$$614889782588491410.$$Điều này nằm trong giới hạn, do đó thuật toán xây dựng một mảng hợp lệ. Mỗi phần tử bỏ qua chính xác một thừa số nguyên tố, giữ nguyên bất biến mà việc loại bỏ phần tử đó sẽ khôi phục số nguyên tố bị thiếu cho gcd. 

Cuối cùng hãy xem xét:```
1
16 1000000000000000000
```Ngay cả trước khi kiểm tra`l`, đầu ra của thuật toán`-1`. 

Bất kỳ mảng thú vị nào cũng cần có mười sáu số nguyên tố riêng biệt trong lcm của nó, nhưng tích của mười sáu số nguyên tố đầu tiên đã vượt quá`10^{18}`. Không có công trình hợp lệ nào có thể tồn tại. Đây chính xác là điều kiện bất khả thi đã được chứng minh trong lời giải.
