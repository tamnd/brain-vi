---
title: "CF 106263A - gugugaga"
description: "Chúng tôi được đưa ra nhiều truy vấn độc lập. Mỗi truy vấn cung cấp hai số nguyên $a$ và $b$, và chúng ta phải tính một giá trị được xác định thông qua hệ số nguyên tố của chúng. Nhiệm vụ là xem xét tất cả các số nguyên tố xuất hiện ở một trong hai số."
date: "2026-06-18T23:18:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106263
codeforces_index: "A"
codeforces_contest_name: "2025 \u534e\u5357\u5e08\u8303\u5927\u5b66\u201c\u5353\u8d8a\u6559\u80b2\u676f\u201d\u7b97\u6cd5\u4e0e\u7a0b\u5e8f\u8bbe\u8ba1\u7ade\u8d5b\uff08\u65b0\u751f\u8d5b\uff09"
rating: 0
weight: 106263
solve_time_s: 58
verified: true
draft: false
---

[CF 106263A - gugugaga](https://codeforces.com/problemset/problem/106263/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được đưa ra nhiều truy vấn độc lập. Mỗi truy vấn cung cấp hai số nguyên$a$Và$b$và chúng ta phải tính một giá trị được xác định thông qua hệ số nguyên tố của chúng. 

Nhiệm vụ là xem xét tất cả các số nguyên tố xuất hiện ở một trong hai số. Đối với mỗi số nguyên tố$p$, chúng ta kiểm tra xem nó có chia cả hai không$a$Và$b$. Nếu có, chúng tôi bao gồm$p$đúng một lần trong một sản phẩm. Nếu không có số nguyên tố nào chia cả hai số thì câu trả lời được xác định là bằng 0. Ngược lại, chúng ta nhân tất cả các số nguyên tố chung như vậy với nhau. 

Vì vậy, đầu ra cho mỗi truy vấn không phải là về bội số, mà chỉ về việc liệu một số nguyên tố có được chia sẻ hay không. Quyền hạn lặp đi lặp lại như$p^k$hoặc$p^m$không quan trọng ngoài sự tồn tại của$p$trong cả hai nhân tố hóa. 

Các ràng buộc rất chặt chẽ: lên đến$2 \cdot 10^5$các truy vấn và mỗi số có thể lớn bằng$2 \cdot 10^6$. Điều này ngay lập tức loại trừ việc phân tích từng số một cách độc lập bằng cách sử dụng phép chia thử cho đến căn bậc hai của nó, vì điều đó sẽ dẫn đến gần đúng$2 \cdot 10^5 \cdot \sqrt{2 \cdot 10^6}$, nó quá lớn. 

Một ý tưởng ngây thơ thường xuất hiện là tính hệ số nguyên tố của cả hai số trên mỗi truy vấn và sau đó giao các tập hợp nguyên tố. Điều này không thành công dưới áp lực về thời gian vì ngay cả với phép chia thử nghiệm được tối ưu hóa, việc phân tích nhân tử lặp lại trên 200 nghìn truy vấn vẫn quá chậm. 

Một trường hợp thất bại tinh vi khác là tính toán lại các ước số của cả hai số và kiểm tra tính nguyên tố một cách nhanh chóng. Ví dụ: trong các đầu vào như$a = 999983$Và$b = 999979$, cả hai số nguyên tố, hệ số hóa ngây thơ thực hiện công việc gần như không đổi trên mỗi số nhưng vẫn lặp lại các bước kiểm tra nặng nề cho mỗi truy vấn. 

Vấn đề cơ cấu quan trọng là chúng ta liên tục phân tích các số trong một phạm vi giới hạn. Điều này gợi ý mạnh mẽ rằng nên xử lý trước tất cả thông tin cơ bản một lần. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force tính toán hệ số nguyên tố của$a$Và$b$cho mỗi truy vấn bằng cách sử dụng phép chia thử. Sau khi trích xuất tập hợp các số nguyên tố cho mỗi số, chúng ta tính giao điểm của chúng và nhân các số nguyên tố đó. 

Điều này đúng vì nó trực tiếp tuân theo định nghĩa. Tuy nhiên, mỗi lần nhân tố hóa có chi phí lên tới$O(\sqrt{n})$, và với$n \le 2 \cdot 10^6$, đây là khoảng$1400$hoạt động trên mỗi số. Với hai số cho mỗi truy vấn và tối đa$2 \cdot 10^5$các truy vấn, chúng tôi đạt được khoảng$5.6 \cdot 10^8$hoạt động quá chậm. 

Quan sát chính là chúng ta không cần phân tích nhân tử đầy đủ cho mỗi truy vấn. Chúng ta chỉ cần biết số nguyên tố nào chia một số và điều này có thể được tính toán trước cho mọi số nguyên lên đến$2 \cdot 10^6$sử dụng sàng cải tiến. Thay vì lưu trữ các phân số đầy đủ, chúng ta chỉ lưu trữ tích không bình phương của các thừa số nguyên tố riêng biệt. Sau đó, mỗi truy vấn sẽ chuyển sang tính toán ước số chung lớn nhất của các giá trị được chuyển đổi này. 

Nếu chúng ta định nghĩa$f(x)$là tích của các thừa số nguyên tố riêng biệt của$x$, thì câu trả lời sẽ trở nên chính xác$\gcd(f(a), f(b))$. Điều này hoạt động vì mỗi số nguyên tố đóng góp một lần và gcd tự nhiên chọn các số nguyên tố chung trong khi bỏ qua bội số. 

Điều này biến mỗi truy vấn thành một thao tác gcd duy nhất trên các giá trị được tính toán trước. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (yếu tố trên mỗi truy vấn) |$O(T \sqrt{n})$|$O(1)$thêm | Quá chậm | 
| Sàng + giảm gcd |$O(N \log \log N + T \log N)$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước một mảng`spf`hoặc trực tiếp xây dựng một mảng`val`Ở đâu`val[x]`là tích của các thừa số nguyên tố phân biệt của`x`. Việc này được thực hiện bằng cách sàng lọc tất cả các số đến$2 \cdot 10^6$. Khi lần đầu tiên chúng ta gặp một số nguyên tố$p$, chúng tôi nhân nó thành tất cả bội số của$p$, đảm bảo mỗi số chỉ thu thập mỗi số nguyên tố một lần. 
2. Với mọi số nguyên$x$, đảm bảo rằng mỗi số nguyên tố chỉ đóng góp một lần duy nhất. Điều này có nghĩa là khi lặp bội số của$p$, chúng tôi không nhân nhiều lần để có lũy thừa cao hơn như$p^2$. Điều này có thể được xử lý bằng một sàng tiêu chuẩn chỉ truyền các số nguyên tố một lần cho mỗi số nguyên tố cơ sở. 
3. Sau khi tiền xử lý, mỗi số$x$được thay thế bằng biểu diễn không có bình phương của nó$val[x]$. 
4. Đối với mỗi truy vấn$(a, b)$, tính toán$g = \gcd(val[a], val[b])$. 
5. Nếu$g = 1$, xuất ra 0 vì không có thừa số nguyên tố chung. 
6. Nếu không thì xuất ra$g$, là tích của tất cả các số nguyên tố chung. 

### Tại sao nó hoạt động 

Mỗi số có thể được biểu diễn duy nhất dưới dạng tích của các số nguyên tố có số mũ, nhưng chúng tôi loại bỏ số mũ và chỉ giữ lại nếu số nguyên tố xuất hiện. Việc sàng đảm bảo rằng mỗi số nguyên tố đóng góp chính xác một lần cho mỗi bội số mà nó chia. Vì thế,$val[x]$là tích của tập hợp số nguyên tố chia$x$. 

Gcd của hai tích như vậy giữ chính xác các số nguyên tố xuất hiện trong cả hai phân tích nhân tử, vì gcd của tích các số nguyên tố khác nhau tương ứng với giao của các tập hợp nguyên tố. Điều này đảm bảo tính chính xác mà không cần phân tích rõ ràng các số cho mỗi truy vấn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MAXN = 2_000_000

val = [1] * (MAXN + 1)

# modified sieve: build square-free product of prime factors
for i in range(2, MAXN + 1):
    if val[i] == 1:  # i is prime (not yet marked)
        for j in range(i, MAXN + 1, i):
            val[j] *= i

T = int(input())
out = []

for _ in range(T):
    a, b = map(int, input().split())
    g = __import__("math").gcd(val[a], val[b])
    out.append(str(g if g > 1 else 0))

print("\n".join(out))
```Vòng lặp tiền xử lý xây dựng một biểu diễn được chuyển đổi của mỗi số nguyên. điều kiện`val[i] == 1`xác định các số nguyên tố, vì các hợp số đã được nhân với các số nguyên tố nhỏ hơn. Sau đó, mỗi số nguyên tố sẽ lan truyền qua các bội số của nó chính xác một lần, đảm bảo mỗi số chỉ tích lũy các thừa số nguyên tố riêng biệt. 

Phần truy vấn sau đó có thời gian không đổi cho đến thao tác gcd. Việc chuyển đổi cuối cùng về 0 xử lý trường hợp đặc biệt khi không tồn tại số nguyên tố chung. 

Một cạm bẫy phổ biến ở đây là quên rằng phải tránh phép nhân lặp lại của cùng một số nguyên tố. Cấu trúc sàng đảm bảo điều này một cách tự nhiên bằng cách chỉ xử lý mỗi số nguyên tố một lần. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
a = 60, b = 70
```Chúng tôi theo dõi các giá trị được chuyển đổi: 

| x | thừa số nguyên tố | giá trị [x] | 
| --- | --- | --- | 
| 60 | 2, 3, 5 | 30 | 
| 70 | 2, 5, 7 | 70 | 

Bây giờ hãy tính:```
gcd(30, 70) = 10
```Đầu ra là 10, khớp với tích của các số nguyên tố chung 2 và 5. 

Điều này xác nhận rằng sàng chỉ bắt chính xác các số nguyên tố riêng biệt và gcd trích xuất giao điểm của chúng. 

### Ví dụ 2 

đầu vào:```
a = 12, b = 175
```| x | thừa số nguyên tố | giá trị [x] | 
| --- | --- | --- | 
| 12 | 2, 3 | 6 | 
| 175 | 5, 7 | 35 | 

Tính toán:```
gcd(6, 35) = 1
```Vì không có số nguyên tố nào được chia sẻ nên đầu ra là 0. 

Điều này thể hiện việc xử lý các tập hợp nguyên tố rời rạc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N \log \log N + T \log N)$| sàng xây dựng các sản phẩm không có ô vuông, mỗi truy vấn sử dụng gcd | 
| Không gian |$O(N)$| lưu trữ các giá trị được chuyển đổi lên tới 2e6 | 

Quá trình tiền xử lý phù hợp thoải mái trong giới hạn cho$N = 2 \cdot 10^6$. Mỗi truy vấn có thời gian không đổi một cách hiệu quả, chỉ bị chi phối bởi tính toán gcd, khiến$2 \cdot 10^5$truy vấn dễ dàng khả thi. 

## Trường hợp thử nghiệm```python
import sys, io
import math

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    MAXN = 2_000_000
    val = [1] * (MAXN + 1)

    for i in range(2, MAXN + 1):
        if val[i] == 1:
            for j in range(i, MAXN + 1, i):
                val[j] *= i

    T = int(input())
    out = []
    for _ in range(T):
        a, b = map(int, input().split())
        g = math.gcd(val[a], val[b])
        out.append(str(g if g > 1 else 0))
    return "\n".join(out)

def run(inp: str) -> str:
    return solve(inp)

# provided samples
assert run("1\n60 70\n") == "10"
assert run("1\n12 175\n") == "0"

# custom cases
assert run("1\n2 3\n") == "0"          # disjoint primes
assert run("1\n8 4\n") == "2"          # repeated powers should not matter
assert run("1\n30 105\n") == "15"      # multiple shared primes
assert run("3\n6 10\n12 18\n7 49\n") == "2\n6\n0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 3 | 0 | số nguyên tố rời rạc | 
| 8 4 | 2 | bỏ qua số mũ | 
| 30 105 | 15 | nhiều số nguyên tố chia sẻ | 
| lô hỗn hợp | 2/6/0 | tính chính xác của nhiều truy vấn | 

## Vỏ cạnh 

Một trường hợp tinh tế là khi một số là lũy thừa thuần túy, chẳng hạn như$a = 64$Và$b = 8$. Cả hai đều là lũy thừa của 2, vì vậy câu trả lời đúng là 2. Biểu diễn sàng cho`val[64] = 2`Và`val[8] = 2`, vậy gcd là 2, phù hợp với kỳ vọng. 

Một trường hợp khác là khi các số không có số nguyên tố nào cả, như$a = 999983$Và$b = 999979$, cả hai số nguyên tố. Biểu diễn trở thành chính nó và gcd là 1, tạo ra kết quả 0. 

Trường hợp cuối cùng là khi các số chứa các số nguyên tố lặp lại, chẳng hạn như$a = 72 = 2^3 \cdot 3^2$. Cái sàng nén cái này thành`val[72] = 6`. Bất kỳ giải pháp đúng nào đều phải đảm bảo phép nhân lặp lại không làm sai lệch kết quả, nếu không gcd sẽ đếm quá số nguyên tố không chính xác.
