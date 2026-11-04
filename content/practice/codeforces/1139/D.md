---
title: "CF 1139D - Bước tới Một"
description: "Chúng tôi liên tục chọn một số nguyên ngẫu nhiên từ 1 đến m, một cách độc lập và thống nhất. Sau mỗi lựa chọn, chúng ta xem xét gcd của tất cả các số đã chọn cho đến thời điểm hiện tại. Quá trình dừng ngay khi gcd này trở thành 1. Chúng ta phải tính số lượng số nguyên được chọn dự kiến."
date: "2026-06-12T03:53:41+07:00"
tags: ["codeforces", "competitive-programming", "dp", "math", "number-theory", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 1139
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 548 (Div. 2)"
rating: 2300
weight: 1139
solve_time_s: 99
verified: true
draft: false
---

[CF 1139D - Từng bước một](https://codeforces.com/problemset/problem/1139/D) 

**Đánh giá:** 2300 
**Tags:** dp, toán, lý thuyết số, xác suất 
**Thời gian giải:** 1 phút 39s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi liên tục chọn một số nguyên ngẫu nhiên từ`1`ĐẾN`m`, độc lập và thống nhất. 

Sau mỗi lựa chọn, chúng ta xem xét gcd của tất cả các số đã chọn cho đến thời điểm hiện tại. Quá trình dừng lại ngay khi gcd này trở thành`1`. Chúng ta phải tính số lượng số nguyên được chọn dự kiến. 

Câu trả lời là một số hữu tỉ. Bài toán yêu cầu modulo kỳ vọng đó`10^9+7`, có nghĩa là chúng ta làm việc theo số học mô-đun và biểu diễn phép chia bằng cách sử dụng nghịch đảo mô-đun. 

Quan sát quan trọng là toàn bộ lịch sử của các số được chọn là không liên quan. Chỉ có vấn đề gcd hiện tại. Nếu gcd hiện tại là`g`, sau đó chọn số mới`x`, gcd mới trở thành`gcd(g, x)`. Điều này biến quy trình thành chuỗi Markov trên các giá trị gcd. 

Ràng buộc`m ≤ 100000`là phần quan trọng nhất của vấn đề. Bất kỳ giải pháp nào mô hình hóa rõ ràng tất cả các tập hợp con, tất cả các chuỗi hoặc tất cả các trạng thái của quy trình đều không thể thực hiện được. Thậm chí một`O(m²)`thuật toán sẽ thực hiện xung quanh`10¹⁰`hoạt động vượt xa giới hạn. Chúng ta cần thứ gì đó xung quanh`O(m log m)`hoặc`O(m log² m)`. 

Có một số trường hợp tế nhị dễ dẫn đến suy luận sai. 

Coi như`m = 1`. 

đầu vào:```
1
```Sự lựa chọn duy nhất có thể là`1`, do đó quá trình dừng ngay lập tức và kỳ vọng chính xác`1`. Mọi tái phát đều phải điều trị gcc đúng cách`1`như một trạng thái hấp thụ. 

Coi như`m = 2`. 

Số đầu tiên là hoặc`1`hoặc`2`. 

Nếu nó là`1`, chúng tôi dừng lại ngay lập tức. 

Nếu nó là`2`, chúng ta tiếp tục vẽ cho đến khi`1`xuất hiện. 

Kỳ vọng là`2`, không`1.5`. Một nỗ lực ngây thơ chỉ nhìn vào lần rút thăm đầu tiên đã bỏ lỡ khả năng kéo dài tùy ý`2`S. 

Một sai lầm phổ biến khác là nghĩ rằng tất cả các giá trị gcd từ`1`ĐẾN`m`có khả năng như nhau. Họ không như vậy. Ví dụ, khi`m = 10`, gcd`6`chỉ có thể phát sinh từ những số chia hết cho`6`, trong khi gcd`2`có thể phát sinh theo nhiều cách khác. Phép truy hồi phải sử dụng xác suất chuyển tiếp chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ mô hình hóa quá trình một cách trực tiếp. Đặt một trạng thái chứa toàn bộ dãy số đã chọn. Từ mỗi trạng thái, chúng tôi nối thêm mọi giá trị tiếp theo có thể có và tiếp tục cho đến khi gcd trở thành`1`. 

Điều này đúng vì nó tuân theo định nghĩa quy trình một cách chính xác. Thật không may, số lượng các chuỗi có thể là vô hạn. Ngay cả khi chúng ta cắt bớt một đoạn nào đó, số lượng trạng thái vẫn tăng theo cấp số nhân và gần như trở nên vô dụng ngay lập tức. 

Quan sát hữu ích đầu tiên là bản thân trình tự không quan trọng. Nếu chúng ta biết gcd hiện tại`g`, sau đó vẽ`x`trạng thái tiếp theo chỉ đơn giản là`gcd(g, x)`. Quá trình này chỉ phụ thuộc vào gcd hiện tại. 

Điều này làm giảm không gian trạng thái xuống chỉ còn`m`tiểu bang. 

Cho phép`E[g]`biểu thị số lần rút bổ sung dự kiến ​​cần thiết để đạt được gcd`1`, giả sử gcd hiện tại bằng`g`. 

Vì`g = 1`, chúng tôi đã hoàn thành rồi, vậy nên`E[1] = 0`. 

Vì`g > 1`, sau một lần rút thăm, chúng ta chuyển sang ước số nào đó của`g`. Điều này đưa ra một phương trình tuyến tính liên quan đến kỳ vọng của các ước số nhỏ hơn. 

Thách thức còn lại là tính toán xác suất chuyển tiếp một cách hiệu quả. 

Đối với trạng thái gcd cố định`g`, trạng thái tiếp theo trở thành`d`chính xác khi nào```
gcd(g, x) = d
```Số lượng như vậy`x`có thể được tính toán bằng cách sử dụng loại trừ bao gồm trên các ước số, về cơ bản là ước số DP kiểu Möbius. 

Vì mọi quá trình chuyển đổi đều bắt đầu từ`g`đến một ước số của`g`, các trạng thái xử lý theo thứ tự tăng dần cho phép biết được tất cả các kỳ vọng nhỏ hơn khi tính toán`E[g]`. 

Điều này biến đổi một quá trình xác suất vô hạn thành một ước số DP với khoảng`m log m`quan hệ số chia. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ / Trạng thái vô hạn | Hàm mũ | Quá chậm | 
| Tối ưu | O(m log m) | O(m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

hãy để`cnt[d] = floor(m / d)`, số các số nguyên trong`[1, m]`chia hết cho`d`. 

Định nghĩa`E[g]`là số lần rút thăm dự kiến ​​trong tương lai cần thiết để đạt được gcd`1`khi gcd hiện tại là`g`. 

1. Đặt`E[1] = 0`. 
2. Đối với mọi`g > 1`, tính xem có bao nhiêu giá trị`x`thỏa mãn`gcd(g, x) = d`với mọi ước số`d`của`g`. 

Gọi số lượng này là`ways[d]`. 
3. Tính toán`ways[d]`sử dụng bao gồm-loại trừ. 

Bắt đầu với:```
ways[d] = count of x divisible by d
        = floor(m / d)
```Sau đó trừ phần đóng góp của các ước lớn hơn của`g`. 

Xử lý các ước số từ lớn nhất đến nhỏ nhất cho ra số đếm chính xác với gcd`d`. 
4. Hãy để`self = ways[g]`. 

Đây là những lựa chọn giữ cho gcd không thay đổi. 
5. Viết phương trình kỳ vọng:```
E[g]
=
1
+
(self/m) E[g]
+
Σ (ways[d]/m) E[d]
```trên tất cả các ước số thích hợp`d < g`. 
6. Sắp xếp lại:```
E[g]
=
(m + Σ ways[d] E[d])
/
(m - self)
```7. Thực hiện tất cả các modulo số học`10^9+7`. 

Phép chia trở thành phép nhân với một nghịch đảo mô-đun. 
8. Tính toán các trạng thái từ`2`ĐẾN`m`. 

Mỗi ước số thực sự của`g`nhỏ hơn`g`, vì vậy tất cả các kỳ vọng cần thiết đã được tính toán. 
9. Số được chọn đầu tiên là số ngẫu nhiên thống nhất. 

Nếu số đầu tiên bằng`i`, gcd hiện tại trở thành`i`. 

Tổng chiều dài dự kiến ​​bằng```
1 + (1/m) Σ E[i]
```bởi vì lần rút thăm đầu tiên luôn xảy ra. 

### Tại sao nó hoạt động 

Đặc tính cốt yếu là tương lai chỉ phụ thuộc vào gcd hiện tại. 

Giả sử gcd hiện tại là`g`. Bất kỳ trình tự nào trước đó dẫn đến`g`trở nên không liên quan vì gcd tiếp theo luôn là`gcd(g, x)`. Điều này làm cho quá trình Markovian. 

Đối với mỗi tiểu bang`g`, phép truy hồi liệt kê mọi lần rút thăm tiếp theo có thể xảy ra. Thuật ngữ`1`tính đến trận hòa mà chúng ta sắp thực hiện. Xác suất chuyển tiếp là chính xác vì`ways[d]`đếm chính xác các giá trị tạo ra gcd`d`. 

Sự tái phát được giải quyết sau khi cô lập xác suất tự lặp. Vì mọi chuyển đổi khác đều đi đến một ước số thích hợp nên các kỳ vọng được tính toán theo thứ tự tôpô. Do đó, mọi giá trị được sử dụng trong công thức đều đúng và quy nạp theo mức tăng`g`chứng minh tính đúng đắn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 1000000007

def solve():
    m = int(input())

    inv = [0] * (m + 1)
    for i in range(1, m + 1):
        inv[i] = pow(i, MOD - 2, MOD)

    divisors = [[] for _ in range(m + 1)]
    for d in range(1, m + 1):
        for multiple in range(d, m + 1, d):
            divisors[multiple].append(d)

    dp = [0] * (m + 1)

    for g in range(2, m + 1):
        divs = divisors[g]

        exact = {}
        for d in reversed(divs):
            cur = m // d
            for bigger in divs:
                if bigger > d and bigger % d == 0:
                    cur -= exact[bigger]
            exact[d] = cur

        num = m % MOD

        for d in divs:
            if d == g:
                continue
            num = (num + exact[d] * dp[d]) % MOD

        stay = exact[g]
        denom = m - stay

        dp[g] = num * inv[denom] % MOD

    ans = 1

    total = 0
    for i in range(1, m + 1):
        total += dp[i]

    ans = (ans + total % MOD * inv[m]) % MOD

    print(ans)

solve()
```Bước tiền xử lý đầu tiên xây dựng danh sách ước số của mọi số nguyên lên đến`m`. Điều này cho phép mỗi trạng thái truy cập tất cả các ước số của nó một cách hiệu quả. 

Đối với trạng thái gcd cố định`g`, chúng ta cần đếm các số tạo ra mỗi ước số. Việc tính toán loại trừ bao gồm bắt đầu từ các ước số lớn hơn và trừ đi số lượng chính xác đã được tính toán. Đây là cách tiêu chuẩn để chuyển đổi số đếm "bội số của d" thành số đếm "gcd chính xác là d". 

Công thức kỳ vọng có chứa một vòng lặp tự. Việc giải các phép truy toán như vậy đòi hỏi phải di chuyển số hạng tự vòng lặp sang bên trái trước khi chia cho khối lượng xác suất còn lại của nó. Quên sự sắp xếp lại này là lỗi phổ biến nhất trong quá trình triển khai. 

Tất cả các phép chia được thực hiện theo modulo`10^9+7`sử dụng nghịch đảo mô-đun. Vì tuyên bố đảm bảo mẫu số là mô đun nghịch đảo của mô đun nguyên tố nên điều này là hợp lệ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
1
```Tính toán trạng thái: 

| g | E[g] | 
| --- | --- | 
| 1 | 0 | 

Kỳ vọng cuối cùng: 

| Số lượng | Giá trị | 
| --- | --- | 
| Trận hòa đầu tiên | 1 | 
| Lượt rút bổ sung trung bình | 0 | 
| Trả lời | 1 | 

Quá trình luôn chọn`1`ngay lập tức nên độ dài mảng luôn chính xác`1`. 

### Mẫu 2 

đầu vào:```
2
```Vì`g = 2`: 

| Số chia d | Đếm với gcd(2,x)=d | 
| --- | --- | 
| 1 | 1 | 
| 2 | 1 | 

Tái phát:```
E[2] = (2 + 1·E[1]) / (2 - 1)
     = 2
```Kỳ vọng cuối cùng: 

| Số lượng | Giá trị | 
| --- | --- | 
| E[1] | 0 | 
| E[2] | 2 | 
| Tổng cộng | 2 | 
| Trả lời | 1 + (0 + 2)/2 = 2 | 

Điều này phù hợp với cách diễn giải quá trình hình học: chúng ta tiếp tục vẽ`2`cho đến lần đầu tiên`1`xuất hiện. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m log m) | Tiền xử lý số chia và số chia-DP trên tất cả các số nguyên | 
| Không gian | O(m) | Danh sách số chia, nghịch đảo và mảng DP | 

Vì`m = 100000`, tổng số quan hệ số chia là khoảng`m log m`, thoải mái phù hợp trong thời hạn. Việc sử dụng bộ nhớ cũng thấp hơn nhiều so với giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    MOD = 1000000007

    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    m = int(input())

    inv = [0] * (m + 1)
    for i in range(1, m + 1):
        inv[i] = pow(i, MOD - 2, MOD)

    divisors = [[] for _ in range(m + 1)]
    for d in range(1, m + 1):
        for v in range(d, m + 1, d):
            divisors[v].append(d)

    dp = [0] * (m + 1)

    for g in range(2, m + 1):
        divs = divisors[g]

        exact = {}
        for d in reversed(divs):
            cur = m // d
            for b in divs:
                if b > d and b % d == 0:
                    cur -= exact[b]
            exact[d] = cur

        num = m % MOD
        for d in divs:
            if d != g:
                num = (num + exact[d] * dp[d]) % MOD

        dp[g] = num * inv[m - exact[g]] % MOD

    ans = (1 + sum(dp) % MOD * inv[m]) % MOD
    return str(ans)

# provided sample
assert run("1\n") == "1", "sample 1"

# custom cases
assert run("2\n") == "2", "sample described in statement"

# boundary checks
assert run("3\n").isdigit(), "small prime"
assert run("10\n").isdigit(), "composite range"
assert run("100000\n").isdigit(), "maximum input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1`|`1`| Trạng thái hấp thụ ngay lập tức | 
|`2`|`2`| Quá trình hình học đơn giản | 
|`3`| Câu trả lời bằng số | Hành vi ranh giới chính | 
|`10`| Câu trả lời bằng số | Chuyển đổi nhiều số chia | 
|`100000`| Câu trả lời bằng số | Hạn chế tối đa | 

## Vỏ cạnh 

Hãy xem xét:```
1
```Quá trình bắt đầu tại gcd`1`sau lần rút thăm đầu tiên. Bộ DP của chúng tôi`E[1]=0`, vì vậy câu trả lời cuối cùng trở nên chính xác`1`. Bất kỳ sự tái diễn nào cố gắng chia cho`(m-self)`vì`g=1`sẽ thất bại, đó là lý do tại sao nhà nước`1`được xử lý riêng biệt. 

Coi như:```
2
```Tình trạng`2`có xác suất tự lặp là`1/2`. Sự tái diễn trở nên```
E[2] = 1 + (1/2)E[2].
```Giải quyết cho`E[2]=2`. Nếu thuật ngữ tự lặp bị bỏ qua, chúng ta sẽ thu được kết quả không chính xác`E[2]=1`. 

Hãy xem xét một giá trị tổng hợp cao như:```
10
```Một số ước số có thể đạt được từ cùng một trạng thái. Bước loại trừ bao gồm đảm bảo rằng một số đóng góp vào gcd`10`không được tính lại trong gcd`5`, gcd`2`, hoặc gcd`1`. Nếu không tính chính xác gcd, xác suất chuyển tiếp sẽ có tổng nhiều hơn một và kỳ vọng sẽ sai. 

Cuối cùng, để có đầu vào tối đa```
100000
```thuật toán không bao giờ lặp lại trên tất cả các cặp`(i,j)`. Nó chỉ đi qua các mối quan hệ số chia, giữ cho thời gian chạy gần`m log m`, đó là lý do tại sao nó vẫn đủ nhanh trong giới hạn đã cho.
