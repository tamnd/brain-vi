---
title: "CF 172D - Cải Cách Lịch"
description: "Mỗi năm ở Berland có một số ngày nhất định. Năm đầu tiên có một ngày, năm tiếp theo có +1, v.v. trong n năm liên tiếp. Đối với một năm có x ngày, chính phủ chọn độ dài một tháng thỏa mãn hai điều kiện. Đầu tiên, độ dài tháng phải là một hình vuông hoàn hảo."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 172
codeforces_index: "D"
codeforces_contest_name: "Croc Champ 2012 - Qualification Round"
rating: 1500
weight: 172
solve_time_s: 89
verified: true
draft: false
---

[CF 172D - Cải cách lịch](https://codeforces.com/problemset/problem/172/D) 

**Đánh giá:** 1500 
**Tags:** *đặc biệt, lý thuyết số 
**Thời gian giải:** 1 phút 29s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi năm ở Berland có một số ngày nhất định. Năm đầu tiên có`a`ngày tiếp theo có`a + 1`, v.v. cho`n`năm liên tiếp. 

Trong một năm với`x`ngày, chính phủ chọn độ dài tháng thỏa mãn hai điều kiện. Đầu tiên, độ dài tháng phải là một hình vuông hoàn hảo. Thứ hai, phải phân chia`x`chính xác thì số tháng là số nguyên. Trong số tất cả các lựa chọn như vậy, họ chọn độ dài tháng lớn nhất có thể. 

Nếu độ dài tháng được chọn là`d²`, thì số tháng trong năm đó là`x / d²`. Vì mỗi tháng cần một tờ giấy nên đóng góp của năm đó cho câu trả lời chính xác là:$$\frac{x}{\text{largest square divisor of }x}$$Chúng ta phải tính tổng số tờ giấy trong tất cả các năm từ`a`ĐẾN`a + n - 1`. 

Những hạn chế là lớn. Cả hai`a`Và`n`có thể đạt được`10^7`và độ dài khoảng cũng có thể gần bằng`10^7`. Một giải pháp phân tích mọi số một cách độc lập bằng phép chia thử sẽ thực hiện gần đúng:$$10^7 \cdot \sqrt{10^7}$$trong trường hợp xấu nhất vượt xa thời hạn. 

Thách thức chính là chúng ta cần thông tin về ước số bình phương cho một dãy số nguyên liên tiếp rất lớn. Điều này gợi ý rõ ràng rằng nên xử lý trước bằng phương pháp giống như sàng thay vì xử lý từng số một. 

Có một số sai lầm dễ dàng xảy ra ở đây. 

Một ý tưởng sai lầm phổ biến là sử dụng số bình phương hoàn hảo lớn nhất nhỏ hơn hoặc bằng độ dài năm. Như vậy vẫn chưa đủ vì bình phương phải chia năm cho chính xác. 

Ví dụ: 

đầu vào:```
10 1
```Một năm có 10 ngày. Bình phương lớn nhất không vượt quá 10 là 9 nhưng 9 không chia hết cho 10. Ước số bình phương đúng là 1 nên đáp án là:```
10
```Một trường hợp tế nhị khác là khi bản thân năm đó là một hình vuông hoàn hảo. 

đầu vào:```
36 1
```Ước số bình phương lớn nhất chính là 36, nên năm chỉ có một tháng và đóng góp:```
1
```Việc triển khai bất cẩn chỉ tìm kiếm các ước số thích hợp sẽ trả về sai 4 hoặc 9. 

Số nguyên tố là một trường hợp cạnh quan trọng khác. 

đầu vào:```
13 1
```Ước số bình phương duy nhất là 1, nên đóng góp là:```
13
```Nếu việc triển khai giả định mọi số đều có ước số bình phương lớn hơn thì nó sẽ thất bại ở đây. 

## Phương pháp tiếp cận 

Giải pháp bạo lực trực tiếp rất đơn giản. Đối với độ dài mỗi năm`x`, liệt kê tất cả các số nguyên`d`như vậy`d² ≤ x`. Bất cứ khi nào`d²`chia rẽ`x`, cập nhật ước số bình phương tốt nhất. Cuối cùng, thêm`x / best`để trả lời. 

Điều này hoạt động vì định nghĩa là rõ ràng. Chúng tôi chỉ cần kiểm tra mọi ước số bình phương có thể có và giữ giá trị lớn nhất hợp lệ. 

Vấn đề là thời gian chạy. Một số duy nhất lên đến`10^7`yêu cầu kiểm tra đại khái`√10^7 ≈ 3162`ứng viên. Làm điều này trong tối đa`10^7`năm dẫn đến hàng chục tỷ hoạt động. 

Chúng ta cần tránh việc tìm kiếm số chia lặp đi lặp lại. 

Quan sát quan trọng là số lượng chúng ta thực sự cần là:$$\frac{x}{\text{largest square divisor of }x}$$Giá trị này có cách giải thích nổi tiếng trong lý thuyết số. Nếu chúng ta phân tích thành nhân tử:$$x = p_1^{e_1} p_2^{e_2} \cdots p_k^{e_k}$$thì ước số bình phương lớn nhất sẽ loại bỏ mọi phần chẵn của số mũ. Giá trị còn lại là:$$p_1^{e_1 \bmod 2} p_2^{e_2 \bmod 2} \cdots p_k^{e_k \bmod 2}$$Đây được gọi là hạt nhân không vuông góc của`x`. 

Ví dụ:$$108 = 2^2 \cdot 3^3$$Ước số bình phương lớn nhất là:$$2^2 \cdot 3^2 = 36$$và hệ số còn lại là:$$3$$Như vậy mỗi năm đóng góp 3 tờ. 

Bây giờ vấn đề trở nên dễ dàng hơn nhiều. Chúng tôi chỉ cần hạt nhân không có hình vuông cho mọi số trong phạm vi lên đến`10^7`. 

Điều này có thể được tính toán một cách hiệu quả bằng một cái sàng. 

Bắt đầu với`f[i] = i`. Đối với mỗi số nguyên tố`p`, chia`f[j]`qua`p²`lặp đi lặp lại cho tất cả các bội số`j`của`p²`. Sau khi xử lý tất cả các số nguyên tố,`f[x]`chính xác trở thành hạt nhân không có hình vuông của`x`. 

Độ phức tạp tổng thể gần bằng độ phức tạp của sàng hài hòa và dễ dàng nằm trong giới hạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n\sqrt{A})$|$O(1)$| Quá chậm | 
| Tối ưu |$O(M \log \log M)$|$O(M)$| Đã chấp nhận | 

Đây,`M = a + n - 1`. 

## Hướng dẫn thuật toán 

1. Đọc`a`Và`n`, và xác định`m = a + n - 1`, độ dài năm lớn nhất mà chúng tôi cần xử lý. 
2. Tạo một mảng`f`ban đầu ở đâu`f[i] = i`cho mọi`1 ≤ i ≤ m`. 

Khi bắt đầu, mỗi số được coi là hạt nhân không có hình vuông của chính nó. 
3. Xây sàng để xác định các số nguyên tố đến`√m`. 

Chúng tôi chỉ quan tâm đến các số nguyên tố có bình phương nằm trong phạm vi. 
4. Với mọi số nguyên tố`p`, tính toán`sq = p * p`. 
5. Lặp lại tất cả các bội số của`sq`. 

Mỗi bội số chứa ít nhất một ước số của`p²`, do đó hạt nhân không có hình vuông của nó không được chứa hệ số bình phương đó. 
6. Trong khi giá trị hiện tại chia hết cho`sq`, chia nó cho`sq`. 

Phép chia lặp đi lặp lại rất quan trọng vì một số có thể chứa lũy thừa cao hơn như`p⁴`hoặc`p⁶`. 
7. Sau khi xử lý tất cả các số nguyên tố,`f[x]`bằng hạt nhân không có bình phương của`x`. 
8. Tổng hợp`f[x]`trong suốt nhiều năm`x`từ`a`ĐẾN`m`. 
9. In tổng số. 

### Tại sao nó hoạt động 

Mọi số nguyên đều có thể phân tích thành thừa số duy nhất thành số nguyên tố. Giả định:$$x = \prod p_i^{e_i}$$Ước số bình phương lớn nhất sẽ loại bỏ tất cả các phần đóng góp chẵn từ số mũ:$$\prod p_i^{2\lfloor e_i/2 \rfloor}$$Chia`x`theo số lượng này lá:$$\prod p_i^{e_i \bmod 2}$$đó chính xác là hạt nhân không có hình vuông. 

Sàng xử lý mọi ô vuông nguyên tố`p²`và loại bỏ nó nhiều lần khỏi tất cả các số bị ảnh hưởng. Sau tất cả những lần loại bỏ như vậy, mọi số mũ nguyên tố còn lại là 0 hoặc 1, nghĩa là kết quả không có chính phương. Vì tất cả các thừa số bình phương đã bị loại bỏ và chỉ có các thừa số bình phương bị loại bỏ nên giá trị cuối cùng chính xác là khoản đóng góp bắt buộc cho năm đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    a, n = map(int, input().split())

    m = a + n - 1

    # f[x] will become the square-free kernel of x
    f = list(range(m + 1))

    # sieve for primes
    is_prime = [True] * (m + 1)
    is_prime[0] = is_prime[1] = False

    p = 2
    while p * p <= m:
        if is_prime[p]:
            # remove p^2 factors from all multiples
            sq = p * p

            for x in range(sq, m + 1, sq):
                while f[x] % sq == 0:
                    f[x] //= sq

            # mark composite numbers
            for multiple in range(p * p, m + 1, p):
                is_prime[multiple] = False

        p += 1

    ans = 0
    for x in range(a, m + 1):
        ans += f[x]

    print(ans)

solve()
```Mảng`f`bắt đầu như ánh xạ nhận dạng, vì vậy`f[x] = x`. Khi sàng chạy, các phần tử hình vuông sẽ bị loại bỏ khỏi mỗi mục cho đến khi chỉ còn lại phần không có hình vuông. 

Vòng lặp phân chia lặp đi lặp lại là điều cần thiết. Giả định`x = 144 = 2^4 * 3^2`. Đang xóa`2²`chỉ một lần sẽ để lại một thừa số bình phương khác:$$144 \to 36$$nhưng 36 vẫn chứa`2²`. Vòng lặp tiếp tục cho đến khi yếu tố đó biến mất hoàn toàn. 

Sàng sơ cấp và quá trình loại bỏ hình vuông được kết hợp trong một lần duyệt. Việc thực hiện đánh dấu vật liệu tổng hợp chính xác như sàng Eratosthenes tiêu chuẩn. 

Câu trả lời có thể trở nên lớn, có khả năng xoay quanh:$$10^7 \cdot 10^7$$vì vậy số nguyên Python là cần thiết. Python tự động xử lý việc này. 

Việc sử dụng bộ nhớ có thể chấp nhận được vì các mảng chứa khoảng`10^7`từng phần tử, phù hợp với giới hạn cuộc thi trong PyPy. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
25 3
```Các năm đó là 25, 26 và 27. 

| Năm | Thừa số nguyên tố | Ước số bình phương lớn nhất | Đóng góp | 
| --- | --- | --- | --- | 
| 25 |$5^2$| 25 | 1 | 
| 26 |$2 \cdot 13$| 1 | 26 | 
| 27 |$3^3$| 9 | 3 | 

Tổng cộng:$$1 + 26 + 3 = 30$$Đầu ra:```
30
```Ví dụ này thể hiện tất cả các trường hợp chính cùng một lúc: số chính phương, số không có chính phương và số có ước số bình phương không tầm thường. 

### Ví dụ 2 

đầu vào:```
10 5
```Các năm là từ 10 đến 14. 

| Năm | Ước số bình phương lớn nhất | Đóng góp | 
| --- | --- | --- | 
| 10 | 1 | 10 | 
| 11 | 1 | 11 | 
| 12 | 4 | 3 | 
| 13 | 1 | 13 | 
| 14 | 1 | 14 | 

Tổng cộng:$$10 + 11 + 3 + 13 + 14 = 51$$Đầu ra:```
51
```Dấu vết này cho thấy các số tổng hợp có thừa số bình phương co lại đáng kể như thế nào trong khi các số nguyên tố vẫn không thay đổi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(M \log \log M)$| Xử lý kiểu sàng trên tất cả các ô vuông nguyên tố | 
| Không gian |$O(M)$| Mảng lưu trữ trạng thái sàng và hạt không vuông | 

Đây`M = a + n - 1`. 

Giới hạn trên là`10^7`, lớn nhưng vẫn có thể quản lý được đối với sàng có kích thước tuyến tính trong Python hoặc PyPy được tối ưu hóa. Thuật toán tránh các hệ số tốn kém cho mỗi số và thay vào đó xử lý các hệ số bình phương trên toàn cầu. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    a, n = map(int, input().split())

    m = a + n - 1

    f = list(range(m + 1))

    is_prime = [True] * (m + 1)
    is_prime[0] = is_prime[1] = False

    p = 2
    while p * p <= m:
        if is_prime[p]:
            sq = p * p

            for x in range(sq, m + 1, sq):
                while f[x] % sq == 0:
                    f[x] //= sq

            for multiple in range(p * p, m + 1, p):
                is_prime[multiple] = False

        p += 1

    ans = sum(f[a:m + 1])
    print(ans)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out.strip()

# provided sample
assert run("25 3\n") == "30", "sample 1"

# minimum input
assert run("1 1\n") == "1", "minimum case"

# perfect square
assert run("36 1\n") == "1", "perfect square year"

# prime number
assert run("13 1\n") == "13", "prime year"

# repeated square factors
assert run("144 1\n") == "1", "multiple square removals"

# small interval
assert run("10 5\n") == "51", "mixed interval"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`|`1`| Giới hạn tối thiểu | 
|`36 1`|`1`| Toàn bộ số là một hình vuông | 
|`13 1`|`13`| Số nguyên tố không thay đổi | 
|`144 1`|`1`| Loại bỏ hệ số bình phương lặp đi lặp lại | 
|`10 5`|`51`| Hành vi khoảng hỗn hợp | 

## Vỏ cạnh 

Hãy xem xét đầu vào:```
13 1
```Số 13 là số nguyên tố. Sàng không bao giờ loại bỏ bất kỳ hệ số bình phương nào vì không tồn tại. Giá trị được lưu trữ vẫn là:$$f[13] = 13$$Thuật toán cho kết quả là 13, điều này đúng vì ước số bình phương duy nhất là 1. 

Bây giờ hãy xem xét:```
36 1
```Ban đầu:$$f[36] = 36$$Khi xử lý`2² = 4`, thuật toán chia liên tục:$$36 \to 9$$Khi xử lý`3² = 9`, nó lại chia:$$9 \to 1$$Khoản đóng góp cuối cùng là 1, nghĩa là một tháng có 36 ngày. 

Một ví dụ tinh tế hơn là:```
72 1
```Nhân tố hóa:$$72 = 2^3 \cdot 3^2$$Ước số bình phương lớn nhất là:$$2^2 \cdot 3^2 = 36$$vì vậy câu trả lời nên là:$$72 / 36 = 2$$Cái sàng loại bỏ`2²`một lần:$$72 \to 18$$sau đó loại bỏ`3²`một lần:$$18 \to 2$$Giá trị còn lại chính xác là kernel không có ô vuông. 

Cuối cùng, hãy xem xét một số có lũy thừa bình phương cao hơn:```
144 1
```Nhân tố hóa:$$144 = 2^4 \cdot 3^2$$Vòng lặp chia lặp lại sẽ loại bỏ`2²`hai lần:$$144 \to 36 \to 9$$sau đó loại bỏ`3²`:$$9 \to 1$$Nếu không lặp lại phép chia, thuật toán sẽ dừng sai ở số 9 thay vì 1.
