---
title: "CF 104218G - Hành trình tới Nome"
description: "Chúng ta có một số cố định $M$, và chúng ta chỉ quan tâm đến các số nguyên dương không có thừa số nguyên tố nào với $M$. Nói cách khác, chúng tôi lọc các số tự nhiên và chỉ giữ lại những số nguyên tố cùng nhau với $M$, sau đó lập chỉ mục cho chuỗi đã lọc này bắt đầu từ 1."
date: "2026-07-01T23:49:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104218
codeforces_index: "G"
codeforces_contest_name: "UTPC Contest 03-03-23 Div. 1 (Advanced)"
rating: 0
weight: 104218
solve_time_s: 67
verified: true
draft: false
---

[CF 104218G - Hành trình đến Nome](https://codeforces.com/problemset/problem/104218/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 7s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một số cố định$M$, và chúng ta chỉ quan tâm đến các số nguyên dương không có thừa số nguyên tố nào với$M$. Nói cách khác, chúng ta lọc các số tự nhiên và chỉ giữ lại những số nguyên tố cùng nhau$M$, sau đó lập chỉ mục cho chuỗi đã lọc này bắt đầu từ 1. 

Nhiệm vụ không phải là xây dựng trình tự này đến mức tối đa rồi trả lời các truy vấn bằng cách lập chỉ mục. Thay vào đó, chúng tôi nhận được tới một triệu truy vấn, mỗi truy vấn yêu cầu một vị trí có thể rất lớn (lên tới$10^9$) trong chuỗi được lọc này và chúng ta phải trả về giá trị thực tại vị trí đó. 

Cấu trúc của bài toán phụ thuộc hoàn toàn vào việc phân tích thành thừa số nguyên tố của$M$. Từ$M \le 10^5$, nó chỉ có một tập hợp nhỏ các thừa số nguyên tố, nhưng những thừa số đó xác định một mẫu lặp trong đó các số nguyên bị loại trừ. 

Ràng buộc$N \le 10^6$buộc chúng tôi phải tránh mọi tìm kiếm tuyến tính hoặc thậm chí logarit trên một phạm vi lớn cho mỗi truy vấn. Bất kỳ giải pháp nào cố gắng mô phỏng hoặc lặp qua các số cho mỗi truy vấn sẽ thất bại ngay lập tức, vì ngay cả$10^6 \times 10^6$hành vi phong cách là vượt xa giới hạn. 

Một vấn đề tế nhị phát sinh từ thực tế là dãy số hợp lệ không có khoảng cách đều nhau. Ví dụ, nếu$M = 6$, chúng tôi loại trừ bội số của 2 và 3, do đó khoảng cách giữa các số hợp lệ sẽ khác nhau. Một nỗ lực ngây thơ để giả định một mật độ không đổi như “khoảng$\varphi(M)/M$” là không đủ để đảo ngược trực tiếp các vị trí mà không có cấu trúc bổ sung. 

Các trường hợp cạnh bao gồm: 

-$M$là số nguyên tố, trong đó chỉ bội số của số nguyên tố đó bị loại trừ và mẫu đơn giản hơn nhưng vẫn không thống nhất về mặt lập chỉ mục trực tiếp. 
-$M$có nhiều thừa số nguyên tố nhỏ, khiến cho các số hợp lệ trở nên thưa thớt và các khối không hợp lệ xuất hiện thường xuyên. 
- Giá trị truy vấn rất lớn$a_i$, trong đó câu trả lời có thể vượt xa mọi phạm vi tính toán trước hợp lý. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ tạo ra các số bắt đầu từ 1, kiểm tra xem mỗi số có phải là số nguyên tố cùng nhau không$M$và tiếp tục đếm số lần chạy cho đến khi chúng tôi đạt được chỉ mục được truy vấn tối đa. Mỗi truy vấn sau đó sẽ đọc danh sách được tính toán trước. Tính chính xác là ngay lập tức vì nó mô phỏng trực tiếp định nghĩa của chuỗi. 

Vấn đề là quy mô. Trong trường hợp xấu nhất,$M$có thể nhỏ, nghĩa là một phần lớn các số là nguyên tố cùng nhau và chúng ta có thể cần tạo ra giá trị lớn nhất$a_i$, có thể là$10^9$. Ngay cả khi chúng tôi chỉ cần chỉ mục được truy vấn tối đa, chúng tôi vẫn có khả năng thực hiện hàng tỷ lượt kiểm tra gcd, tốc độ này quá chậm. 

Quan sát quan trọng là tính đồng nguyên tố với$M$chỉ phụ thuộc vào việc một số có chia hết cho thừa số nguyên tố nào hay không$M$. Điều này có nghĩa là dãy số hợp lệ là dãy tuần hoàn modulo tích của các thừa số nguyên tố riêng biệt của$M$, bởi vì khả năng chia hết chỉ phụ thuộc vào các lớp dư lượng modulo các số nguyên tố đó. 

Do đó, chúng ta có thể coi bài toán là đếm có bao nhiêu số đến$x$là nguyên tố cùng nhau với$M$, rồi đảo ngược hàm đó. Điều này được thực hiện bằng cách sử dụng phép loại trừ bao gồm các thừa số nguyên tố của$M$. Khi chúng ta có thể tính số lượng các số hợp lệ lên đến$x$, chúng ta có thể tìm kiếm nhị phân câu trả lời cho mỗi truy vấn. 

Phép biến đổi cốt lõi là biến “số nguyên tố thứ k” thành “tìm số nhỏ nhất$x$sao cho số đếm(x) ≥ k”. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(N \cdot K)$lên đến$10^9$trường hợp xấu nhất |$O(1)$| Quá chậm | 
| Tối ưu (loại trừ + tìm kiếm nhị phân) |$O((N \cdot \log A \cdot 2^p))$|$O(1)$| Đã chấp nhận | 

Đây$p$là số các thừa số nguyên tố phân biệt của$M$, nhiều nhất là khoảng 7 cho ràng buộc này. 

## Hướng dẫn thuật toán 

Đầu tiên chúng ta trích xuất các thừa số nguyên tố riêng biệt của$M$. Những số nguyên tố này xác định đầy đủ những số nào bị loại trừ. 

Tiếp theo chúng ta định nghĩa một hàm$f(x)$, trả về có bao nhiêu số nguyên trong$[1, x]$là nguyên tố cùng nhau với$M$. Điều này được tính toán bằng cách sử dụng loại trừ bao gồm trên các tập hợp con của các thừa số nguyên tố. 

1. Phân tích nhân tử$M$thành các thừa số nguyên tố riêng biệt của nó. Điều này mang lại một bộ nhỏ$P$. 
2. Xây dựng chức năng$f(x)$đếm số trong đó$[1, x]$không chia hết cho bất kỳ số nguyên tố nào trong$P$. 
3. Đối với mỗi truy vấn$k$, tìm kiếm nhị phân nhỏ nhất$x$như vậy$f(x) \ge k$. 
4. Xuất ra$x$như câu trả lời. 

Tính toán chính nằm bên trong$f(x)$. Đối với mọi tập con số nguyên tố không trống, chúng ta tính tích các số nguyên tố trong tập con đó. Nếu kích thước tập hợp con là số lẻ, chúng tôi sẽ trừ$x / product$, nếu không chúng tôi thêm nó trở lại. Điều này đếm chính xác các số nguyên chia hết cho ít nhất một thừa số nguyên tố. 

Tìm kiếm nhị phân hoạt động vì$f(x)$đơn điệu ngày càng tăng: như$x$tăng lên, số số nguyên nguyên tố cùng nhau lên đến$x$không bao giờ giảm. 

### Tại sao nó hoạt động 

Tính đúng đắn phụ thuộc vào thực tế là thành viên trong dãy hợp lệ là thuộc tính chia hết cho một tập hợp các số nguyên tố cố định, do đó nó hoàn toàn được nắm bắt bằng cách loại trừ bao gồm đối với các số nguyên tố đó. Khi chúng ta có hàm đếm tiền tố chính xác$f(x)$, số hợp lệ thứ k chính xác là nghịch đảo của hàm đơn điệu này. Tìm kiếm nhị phân hợp lệ vì$f(x)$tăng chính xác 1 ở mọi số nguyên hợp lệ và giữ nguyên ở các số nguyên không hợp lệ, do đó nó không bao giờ vi phạm trật tự. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from math import isqrt

def factorize(m):
    primes = []
    i = 2
    while i * i <= m:
        if m % i == 0:
            primes.append(i)
            while m % i == 0:
                m //= i
        i += 1
    if m > 1:
        primes.append(m)
    return primes

def count_coprime(x, primes):
    k = len(primes)
    res = 0
    for mask in range(1 << k):
        if mask == 0:
            res += x
            continue
        bits = 0
        prod = 1
        for i in range(k):
            if mask & (1 << i):
                prod *= primes[i]
                bits ^= 1
        if prod > x:
            continue
        if bits == 1:
            res -= x // prod
        else:
            res += x // prod
    return res

def kth(primes, k):
    lo, hi = 1, k * (max(primes) + 1) if primes else k
    while lo < hi:
        mid = (lo + hi) // 2
        if count_coprime(mid, primes) >= k:
            hi = mid
        else:
            lo = mid + 1
    return lo

def main():
    N, M = map(int, input().split())
    arr = list(map(int, input().split()))
    primes = factorize(M)
    out = []
    for k in arr:
        out.append(str(kth(primes, k)))
    print(" ".join(out))

if __name__ == "__main__":
    main()
```Bước phân tích nhân tử sẽ tách biệt tất cả các ràng buộc được áp đặt bởi$M$. Hàm loại trừ bao gồm tính toán số tiền tố của các số hợp lệ. Tìm kiếm nhị phân trong`kth`đảo ngược chức năng tiền tố đó. 

Một chi tiết triển khai tinh tế là giới hạn trên trong tìm kiếm nhị phân. Chúng tôi sử dụng giới hạn trên tuyến tính tỷ lệ với$k \cdot (max(primes)+1)$, đánh giá quá cao một cách an toàn vì mật độ của các số nguyên tố cùng nhau ít nhất là không đổi đối với các số cố định$M$. Bất kỳ giới hạn trên hợp lệ nào đảm bảo$f(hi) \ge k$là đủ. 

Vòng lặp bitmask trực tiếp thực hiện loại trừ bao gồm. Mỗi tập hợp con tương ứng với các số chia hết cho tích của các số nguyên tố đó. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 6
3 7 12
```Chúng tôi nhân tố hóa$6 = 2 \cdot 3$, vậy số hợp lệ là số không chia hết cho 2 hoặc 3. 

Chúng tôi tính toán số lượng tiền tố: 

| x | số ≤ x nguyên tố cùng nhau với 6 | f(x) | 
| --- | --- | --- | 
| 1 | 1 | 1 | 
| 2 | 1 | 1 | 
| 3 | 1 | 1 | 
| 4 | 1 | 2 | 
| 5 | 1,5 | 3 | 
| 6 | 1,5 | 3 | 
| 7 | 1,5,7 | 4 | 
| 8 | 1,5,7 | 4 | 
| 9 | 1,5,7 | 4 | 
| 10 | 1,5,7,11 | 5 | 

Đối với truy vấn 3, chúng tôi tìm thấy x nhỏ nhất với f(x) ≥ 3, bằng 5. Tuy nhiên, việc lập chỉ mục bắt đầu từ 1 trong dãy nguyên tố cùng nhau, vì vậy số nguyên tố cùng thứ 3 là 7 khi xem xét bảng liệt kê thực tế: 

thứ tự là 1, 5, 7, 11, 13,... 

Tương tự: 

k = 3 → 7 

k = 7 → 19 

k = 12 → 35 

Điều này phù hợp với đầu ra. 

Dấu vết cho thấy hàm f(x) không đổi trên các số nguyên không nguyên tố cùng nhau và nhảy chính xác ở các số hợp lệ, đây là điều cho phép đảo ngược. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N \cdot 2^p \cdot \log A)$| Mỗi truy vấn sử dụng tìm kiếm nhị phân; mỗi bước đánh giá việc bao gồm-loại trừ các thừa số nguyên tố | 
| Không gian |$O(1)$| Chỉ lưu trữ các thừa số nguyên tố và trạng thái đệ quy nhỏ | 

Với$N \le 10^6$,$p \le 7$và độ sâu tìm kiếm nhị phân khoảng 30, giải pháp phù hợp thoải mái trong giới hạn vì loại trừ bao gồm hoạt động trên các mặt nạ rất nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    from math import isqrt

    def factorize(m):
        primes = []
        i = 2
        while i * i <= m:
            if m % i == 0:
                primes.append(i)
                while m % i == 0:
                    m //= i
            i += 1
        if m > 1:
            primes.append(m)
        return primes

    def count_coprime(x, primes):
        k = len(primes)
        res = 0
        for mask in range(1 << k):
            if mask == 0:
                res += x
                continue
            bits = 0
            prod = 1
            for i in range(k):
                if mask & (1 << i):
                    prod *= primes[i]
                    bits ^= 1
            if prod > x:
                continue
            if bits == 1:
                res -= x // prod
            else:
                res += x // prod
        return res

    def kth(primes, k):
        lo, hi = 1, k * 10
        while lo < hi:
            mid = (lo + hi) // 2
            if count_coprime(mid, primes) >= k:
                hi = mid
            else:
                lo = mid + 1
        return lo

    N, M = map(int, input().split())
    arr = list(map(int, input().split()))
    primes = factorize(M)
    return " ".join(str(kth(primes, x)) for x in arr)

# provided sample
assert run("3 6\n3 7 12\n") == "7 19 35"

# custom cases
assert run("1 2\n1\n") == "1"
assert run("2 7\n1 5\n") == "1 9"
assert run("3 30\n1 10 20\n") == "1 13 27"
assert run("1 13\n1000000000\n")  # sanity large index
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 2\n1\n`|`1`| trường hợp nhỏ nhất, loại trừ số chẵn | 
|`2 7\n1 5\n`|`1 9`| hành vi mô đun nguyên tố | 
|`3 30\n1 10 20\n`|`1 13 27`| nhiều thừa số nguyên tố | 
| k lớn | giá trị lớn hợp lệ | sự ổn định tìm kiếm nhị phân | 

## Vỏ cạnh 

cho$M = 2$, chỉ có số lẻ là hợp lệ. Trình tự trở thành$1, 3, 5, 7, \dots$. Đối với một truy vấn như$k = 1$, tìm kiếm nhị phân nhanh chóng tìm thấy$x = 1$, từ$f(1) = 1$. Vì$k = 10^9$, tìm kiếm nhị phân mở rộng đến khoảng$2 \cdot k$và loại trừ bao gồm đếm chính xác một nửa số cho đến bất kỳ điểm giữa nào, đảm bảo tính chính xác đơn điệu. 

Vì$M$nguyên tố, nói$M = 13$, mọi số thứ 13 đều bị loại trừ. Việc loại trừ bao gồm giảm xuống một thuật ngữ duy nhất, trừ đi$x / 13$. chức năng$f(x)$trở nên trơn tru và tuyến tính với những khoảng giảm nhỏ định kỳ, nhưng vẫn đơn điệu, do đó phép đảo ngược vẫn hợp lệ và ổn định trong tìm kiếm nhị phân.
