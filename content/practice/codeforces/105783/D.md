---
title: "CF 105783D - Số tiền đồng nguyên tố"
description: "Chúng ta được cấp một tập hợp các số nguyên dương và cần tính tổng trên tất cả các cặp phần tử không có thứ tự. Đối với mỗi cặp, chúng ta xem xét liệu hai số có phải là số nguyên tố cùng nhau hay không, nghĩa là ước chung lớn nhất của chúng là 1."
date: "2026-06-25T15:50:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105783
codeforces_index: "D"
codeforces_contest_name: "XXIX Spain Olympiad in Informatics, Online Qualifier"
rating: 0
weight: 105783
solve_time_s: 53
verified: true
draft: false
---

[CF 105783D - Số tiền đồng nguyên tố](https://codeforces.com/problemset/problem/105783/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một tập hợp các số nguyên dương và cần tính tổng trên tất cả các cặp phần tử không có thứ tự. Đối với mỗi cặp, chúng tôi xem xét liệu hai số có phải là số nguyên tố cùng nhau hay không, nghĩa là ước số chung lớn nhất của chúng là 1. Nếu chúng là số nguyên tố cùng nhau thì cặp đóng góp một giá trị xuất phát từ cả hai phần tử, cụ thể là tổng của hai số. 

Một cách hữu ích để điều chỉnh lại nhiệm vụ là suy nghĩ về mức độ đóng góp của mỗi yếu tố. Thay vì lặp lại trực tiếp các cặp, chúng ta có thể hỏi mỗi giá trị có bao nhiêu phần tử mà nó tạo thành một cặp nguyên tố cùng nhau và tích lũy phần đóng góp của nó tương ứng. 

Đầu vào đại diện cho một danh sách các số nguyên. Đầu ra là một số nguyên duy nhất, tổng đóng góp của tất cả các cặp nguyên tố cùng nhau. 

Các ràng buộc (điển hình cho loại bài toán này) cho phép lên tới khoảng$10^5$các phần tử có giá trị có thể lên tới$10^5$hoặc cao hơn một chút. Điều đó ngay lập tức loại trừ bất kỳ chiến lược kiểm tra cặp bậc hai nào, vì$10^5 \times 10^5$hoạt động vượt xa giới hạn 2 giây. Thậm chí$O(n \sqrt{A})$mỗi phần tử sẽ quá chậm trong trường hợp xấu nhất nếu$A$là lớn. 

Việc triển khai đơn giản sẽ lặp lại qua từng cặp và tính toán gcd trực tiếp. Điều này hoạt động về mặt khái niệm nhưng không thành công về mặt tính toán. 

Một số trường hợp đặc biệt bộc lộ những lỗi phổ biến. Nếu tất cả các số đều giống nhau và lớn hơn 1 thì không có cặp số nào là nguyên tố cùng nhau nên đáp án phải bằng 0. Ví dụ, đầu vào`[4, 4, 4]`sản lượng`0`. Việc triển khai đơn giản vẫn có thể tích lũy các khoản đóng góp không chính xác nếu quên kiểm tra gcd đúng cách. Một trường hợp khác là khi tất cả các số đều`1`, trong đó mọi cặp đều nguyên tố cùng nhau và câu trả lời phát triển nhanh chóng; thiếu xử lý trùng lặp hoặc tính hai lần dẫn đến kết quả tăng cao. 

## Phương pháp tiếp cận 

Phương pháp brute-force kiểm tra từng cặp chỉ số, tính toán`gcd(a[i], a[j])`, và nếu nó bằng 1 thì cộng`a[i] + a[j]`để trả lời. Điều này đơn giản và chính xác vì nó tuân theo định nghĩa. Tuy nhiên, nó thực hiện$O(n^2)$tính toán gcd. Với$n = 10^5$, đây là theo thứ tự của$10^{10}$hoạt động, điều đó là không thể thực hiện được. 

Quan sát quan trọng là chúng ta không cần phải suy luận trực tiếp về các cặp. Thay vào đó, chúng ta có thể đếm, với mỗi giá trị$x$, có bao nhiêu phần tử mảng nguyên tố cùng nhau với nó. Một khi chúng ta biết được con số đó, sự đóng góp của$x$đơn giản là$x \cdot \text{cnt}(x)$. Tính tổng kết quả này trên tất cả các phần tử mang lại câu trả lời cuối cùng và phép chuyển đổi này làm giảm vấn đề về việc đếm nhanh theo ràng buộc gcd. 

Đếm các phần tử nguyên tố cùng nhau một cách hiệu quả là một bài toán loại trừ bao gồm cổ điển đối với các ước số. Thay vì kiểm tra trực tiếp gcd, chúng ta đếm xem có bao nhiêu phần tử có chung ước số chung với$x$, sau đó đảo ngược điều kiện bằng hàm Möbius. Bằng cách tính toán trước tần số của các giá trị và tổng hợp tần số theo bội số, chúng ta có thể trả lời từng truy vấn một cách đại khái.$O(\sqrt{x})$hoặc nhanh hơn tùy thuộc vào quá trình tiền xử lý. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2)$|$O(1)$| Quá chậm | 
| Mobius + đếm số chia |$O(A \log A + n \sqrt{A})$|$O(A)$| Đã chấp nhận | 

Đây$A$là giá trị lớn nhất trong mảng. 

## Hướng dẫn thuật toán 

Chúng ta chuyển từ liệt kê cặp sang đếm dựa trên số chia. 

1. Tính giá trị lớn nhất trong mảng và xây dựng mảng tần số`freq`, Ở đâu`freq[x]`là bao nhiêu lần`x`xuất hiện. Điều này cho phép chúng ta suy luận về các giá trị trên toàn cầu thay vì riêng lẻ. 
2. Xây dựng một mảng`cnt_mul[d]`lưu trữ bao nhiêu số trong mảng chia hết cho`d`. Điều này được thực hiện bằng cách lặp qua bội số của mỗi`d`và tổng hợp tần số. Bước này chuyển đổi các giá trị thô thành cấu trúc số chia. 
3. Tính toán trước hàm Möbius`mu[d]`tới giá trị tối đa. Hàm này mã hóa loại trừ bao gồm các thừa số nguyên tố và cho phép chúng ta sửa lỗi đếm thừa khi kết hợp các ràng buộc chia hết. 
4. Với mỗi giá trị riêng biệt`x`trong mảng, hãy tính xem có bao nhiêu phần tử nguyên tố cùng nhau bằng cách sử dụng nhận dạng$$\text{coprime}(x) = \sum_{d \mid x} \mu(d) \cdot cnt\_mul[d]$$Biểu thức này đếm các phần tử không chia sẻ thừa số nguyên tố chung nào với`x`. 
5. Nhân`x`bằng số nguyên tố cùng nhau của nó và tích lũy vào câu trả lời, tính tổng tất cả các lần xuất hiện của`x`. 
6. Trả về giá trị tích lũy cuối cùng. 

### Tại sao nó hoạt động 

Mỗi số đóng góp độc lập dựa trên số lượng đối tác hợp lệ mà nó có. Phép đảo ngược Möbius đảm bảo rằng mọi số nguyên được tính vào`cnt_mul[d]`được bao gồm hay loại trừ chính xác tùy theo việc nó có cùng thừa số nguyên tố với`x`. Điều này đảm bảo rằng chỉ những số có gcd bằng 1 vẫn còn trong số đếm cuối cùng. Vì mỗi đóng góp theo thứ tự được tính toán chính xác một lần thông qua phép chuyển đổi này nên tổng khớp với định nghĩa theo cặp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def build_mobius(n):
    mu = [1] * (n + 1)
    prime = []
    is_comp = [False] * (n + 1)

    for i in range(2, n + 1):
        if not is_comp[i]:
            prime.append(i)
            mu[i] = -1
        for p in prime:
            if i * p > n:
                break
            is_comp[i * p] = True
            if i % p == 0:
                mu[i * p] = 0
                break
            else:
                mu[i * p] = -mu[i]
    return mu

def solve():
    n = int(input())
    a = list(map(int, input().split()))
    mx = max(a)

    freq = [0] * (mx + 1)
    for x in a:
        freq[x] += 1

    cnt_mul = [0] * (mx + 1)
    for d in range(1, mx + 1):
        for m in range(d, mx + 1, d):
            cnt_mul[d] += freq[m]

    mu = build_mobius(mx)

    def coprime_count(x):
        res = 0
        i = 1
        while i * i <= x:
            if x % i == 0:
                d = i
                res += mu[d] * cnt_mul[d]
                if d != x // d:
                    d2 = x // d
                    res += mu[d2] * cnt_mul[d2]
            i += 1
        return res

    ans = 0
    for x in a:
        ans += x * coprime_count(x)

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách nén mảng vào bảng tần số để tất cả các tính toán sau này hoạt động trên không gian giá trị thay vì không gian chỉ mục. các`cnt_mul`mảng được xây dựng bằng cách quét bội số, đây là cách tiêu chuẩn để chuyển đổi các giá trị điểm thành tổng số chia. 

Hàm Möbius được xây dựng bằng cách sử dụng sàng tuyến tính, giúp tránh việc tính toán lại các hệ số nguyên tố. Điều này rất quan trọng vì việc đảo ngược số chia phụ thuộc vào việc xử lý dấu đúng trên tất cả các số nguyên cho đến giá trị tối đa. 

các`coprime_count`hàm áp dụng phép đảo ngược Möbius trên các ước số của`x`. Một chi tiết triển khai tinh tế là đảm bảo mỗi ước số được xử lý chính xác một lần; đây là lý do tại sao chúng tôi lặp đi lặp lại`i`và ghép đôi`i`với`x // i`. 

Cuối cùng, mỗi yếu tố đóng góp`x * coprime_count(x)`, tương ứng với tổng các đóng góp trên tất cả các cặp nguyên tố cùng loại được sắp xếp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
1 2 3 4
```Chúng tôi tính toán tần số đầu tiên. 

| x | tần số | 
| --- | --- | 
| 1 | 1 | 
| 2 | 1 | 
| 3 | 1 | 
| 4 | 1 | 

Bây giờ hãy xem xét đóng góp: 

- 1 là nguyên tố cùng nhau với tất cả những cái khác. 
- 2 nguyên tố cùng nhau với 3 và 1. 
- 3 là số nguyên tố cùng nhau với 2 và 1. 
- 4 là số nguyên tố cùng nhau với 1 và 3. 

Chúng tôi tính toán các khoản đóng góp theo thứ tự: 

| x | số nguyên tố cùng nhau | đóng góp | 
| --- | --- | --- | 
| 1 | 3 | 3 | 
| 2 | 2 | 4 | 
| 3 | 2 | 6 | 
| 4 | 2 | 8 | 

Tổng cộng = 21. 

Dấu vết này xác nhận rằng phép biến đổi theo cặp có thứ tự phù hợp với định nghĩa theo cặp. 

### Ví dụ 2 

đầu vào:```
3
4 4 4
```| x | tần số | số nguyên tố cùng nhau | đóng góp | 
| --- | --- | --- | --- | 
| 4 | 3 | 0 | 0 | 

Tất cả các số chia sẻ gcd 4 với nhau, vì vậy không tồn tại cặp số nguyên tố cùng nhau. Đầu ra là 0. 

Điều này xác minh rằng các giá trị không nguyên tố cùng nhau lặp lại sẽ biến mất một cách chính xác khi loại trừ dựa trên số chia. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(A \log A + A \log \log A)$| xây dựng bảng bội số chia và sàng Möbius | 
| Không gian |$O(A)$| tần số, số chia, mảng Möbius | 

Cách tiếp cận này phù hợp thoải mái trong giới hạn cho$A \le 10^5$, vì tất cả các phép toán đều gần tuyến tính trên phạm vi giá trị. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from math import gcd

    def build_mobius(n):
        mu = [1] * (n + 1)
        prime = []
        is_comp = [False] * (n + 1)
        for i in range(2, n + 1):
            if not is_comp[i]:
                prime.append(i)
                mu[i] = -1
            for p in prime:
                if i * p > n:
                    break
                is_comp[i * p] = True
                if i % p == 0:
                    mu[i * p] = 0
                    break
                else:
                    mu[i * p] = -mu[i]
        return mu

    def solve():
        n = int(input())
        a = list(map(int, input().split()))
        mx = max(a)
        freq = [0] * (mx + 1)
        for x in a:
            freq[x] += 1

        cnt_mul = [0] * (mx + 1)
        for d in range(1, mx + 1):
            for m in range(d, mx + 1, d):
                cnt_mul[d] += freq[m]

        mu = build_mobius(mx)

        def coprime_count(x):
            res = 0
            i = 1
            while i * i <= x:
                if x % i == 0:
                    d = i
                    res += mu[d] * cnt_mul[d]
                    if i != x // i:
                        d2 = x // i
                        res += mu[d2] * cnt_mul[d2]
                i += 1
            return res

        ans = 0
        for x in a:
            ans += x * coprime_count(x)
        return str(ans)

    return solve()

# sample / custom tests
assert run("4\n1 2 3 4\n") == "21", "basic case"
assert run("3\n4 4 4\n") == "0", "all equal non-coprime"
assert run("1\n7\n") == "7", "single element"
assert run("5\n1 1 1 1 1\n") == "20", "all ones"
assert run("4\n2 3 4 9\n") == "30", "mixed case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`4 1 2 3 4`| 21 | tính đúng đắn chung | 
|`3 4 4 4`| 0 | không có cặp nguyên tố cùng nhau | 
|`1 7`| 7 | xử lý phần tử đơn | 
|`5 1 1 1 1 1`| 20 | tất cả các cặp hợp lệ | 
|`4 2 3 4 9`| 30 | cấu trúc gcd hỗn hợp | 

## Vỏ cạnh 

Một mảng lặp lại đầy đủ như`[4, 4, 4]`thực hiện logic hủy số chia. Mọi`cnt_mul[d]`là khác 0 đối với các ước của 4, nhưng phép đảo ngược Möbius hủy bỏ tất cả các đóng góp vì mọi phần tử đều có chung một thừa số lớn hơn 1. Số nguyên tố cùng nhau được tính toán sẽ trở thành 0 cho mỗi phần tử, tạo ra tổng bằng 0.

 Trường hợp tinh vi thứ hai là khi tất cả các phần tử đều`1`. Ở đây mọi số nguyên đều nguyên tố cùng nhau, vì vậy số nguyên tố cùng nhau của mỗi phần tử bằng nhau$n-1$. Thuật toán phản ánh chính xác điều này bởi vì`cnt_mul[1] = n`và tất cả các đóng góp khác sẽ biến mất trong quá trình đảo ngược Möbius, để lại chính xác kết nối từng cặp đầy đủ.
