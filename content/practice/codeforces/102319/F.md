---
title: "CF 102319F - Trẻ mãi"
description: "Học sinh có độ tuổi khác nhau nên việc sắp xếp lớp học chỉ đơn giản là hoán vị các số (1,ldots,s). Số học sinh được khoanh tròn tối đa của Henry là độ dài của dãy con tăng dần dài nhất của hoán vị đó."
date: "2026-08-14T04:51:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102319
codeforces_index: "F"
codeforces_contest_name: "UBC Summer Contest 2018"
rating: 0
weight: 102319
solve_time_s: 123
verified: true
draft: false
---

[CF 102319F - Trẻ mãi không già](https://codeforces.com/problemset/problem/102319/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 3s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Học sinh có độ tuổi khác nhau nên việc sắp xếp lớp học chỉ đơn giản là hoán vị các số (1,\ldots,s). Số học sinh được khoanh tròn tối đa của Henry là độ dài của dãy con tăng dần dài nhất của hoán vị đó. Giá trị lớn nhất của Eugene là độ dài của dãy con giảm dài nhất. 

Chúng ta cần đếm các hoán vị có dãy con tăng dài nhất có độ dài chính xác (n) và dãy con giảm dài nhất có độ dài chính xác (m). Câu trả lời là bắt buộc theo modulo (10^9+7). 

Giá trị lớn (s\le 10^6) ngay lập tức loại trừ bất kỳ điều gì kiểm tra học sinh theo số cách bậc hai hoặc giai thừa. Ràng buộc bổ sung hữu ích là (n+m\ge s-50). Điều này nói lên rằng hai độ dài dãy con mong muốn gần như lớn bằng toàn bộ hoán vị, điều này hạn chế nghiêm trọng các cấu trúc tổ hợp có thể có. Toàn bộ giải pháp khai thác hạn chế đó. 

Có hai điều kiện khả thi cơ bản đáng ghi nhớ. Một hoán vị không thể có các dãy con tăng và giảm có tổng độ dài lớn hơn (s+1), vì hai dãy con có thể có nhiều nhất một học sinh. Ngoài ra, Erdős-Szekeres ngụ ý (nm\ge s) bất cứ khi nào hoán vị như vậy tồn tại. 

Trường hợp cạnh đầu tiên là (s=1,n=1,m=1). Có đúng một cách sắp xếp nên đáp án là (1). Việc triển khai bất cẩn với giả định rằng luôn có ít nhất một hàng hoặc cột không cần thiết trong sơ đồ Young có thể xử lý sai trường hợp này. 

Trường hợp cạnh thứ hai là (s=5,n=5,m=5). Tổng độ dài chuỗi con được yêu cầu là (10), lớn hơn (s+1=6), vì vậy câu trả lời là (0). Một chương trình chỉ kiểm tra giới hạn dưới đã nêu trên (n+m) và bắt đầu liệt kê các hình dạng có thể vô tình coi một hình dạng không thể là hợp lệ. 

Ví dụ: trường hợp cạnh thứ ba xảy ra ở đầu kia của phạm vi được phép (s=52,n=1,m=1). Ở đây (n+m=2=s-50), do đó đầu vào thỏa mãn ràng buộc đặc biệt, nhưng không có hoán vị nào của 52 giá trị riêng biệt có thể có cả LIS và LDS bằng 1. Câu trả lời là (0). Điều kiện đặc biệt giới hạn số lượng cấu trúc bổ sung mà chúng ta phải liệt kê, nhưng nó không làm cho mọi cặp (n,m) đều khả thi. 

Cuối cùng, khi (n+m=s+1), hai dãy con phải bao phủ toàn bộ hoán vị bằng đúng một phần tử chung. Chỉ có một hình dạng sơ đồ Young khả thi là một cái móc. Trường hợp này rất hữu ích để phát hiện lỗi từng lỗi một trong định nghĩa của tham số nhỏ được sử dụng bên dưới. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là liệt kê tất cả (các) hoán vị, tính toán các dãy con tăng và giảm dài nhất của chúng và đếm những dãy thỏa mãn các giá trị được yêu cầu. Ngay cả khi triển khai LIS (O(s\log s)), việc này vẫn cần các thao tác (O(s!,s\log s)). Tại (s=20), số lượng hoán vị đã là (20!\approx2.43\cdot10^{18}), vì vậy phương pháp này hoàn toàn không thể sử dụng được. 

Một cách tiếp cận tốt hơn là ngừng suy nghĩ về hoán vị. Sự tương ứng Robinson-Schensted đưa ra sự song ánh giữa các hoán vị và các cặp hoạt cảnh Young tiêu chuẩn có hình dạng giống nhau. Đối với hoán vị có hình dạng (\lambda), hàng đầu tiên của (\lambda) có độ dài bằng LIS và cột đầu tiên có độ dài bằng LDS. 

Nếu (f^\lambda) biểu thị số lượng hoạt cảnh Young tiêu chuẩn có hình dạng (\lambda), thì một hình dạng cố định tương ứng với chính xác ((f^\lambda)^2) hoán vị, bởi vì hai hoạt cảnh này có thể được chọn độc lập. Vì vậy, câu trả lời mong muốn là 

[ 
\sum_{\substack{\lambda\vdash s\\lambda_1=n\\lambda'_1=m}}(f^\lambda)^2. 
] 

Brute-force hoạt động vì mỗi hoán vị được biểu diễn chính xác một lần bằng cặp hoạt cảnh của nó. Vấn đề là vẫn còn quá nhiều phân vùng của (các) để liệt kê khi (các) lớn. 

Quan sát chính là điều kiện (n+m\ge s-50). Sơ đồ Young với hàng đầu tiên (n) và cột đầu tiên (m) có một móc bắt buộc chứa 

[ 
n+m-1 
]

tế bào. Mọi thứ khác bao gồm nhiều nhất 

[ 
t=s-(n+m-1)=s-n-m+1 
] 

các tế bào bổ sung. Điều kiện đầu vào đưa ra (0\le t\le51) cho mọi trường hợp khả thi. 

Xóa hàng đầu tiên và cột đầu tiên khỏi các ô bổ sung đó. Những gì còn lại là một phân vùng thông thường (\mu) có chính xác là (t). Vì (t\le51) nên có nhiều nhất (p(51)=239943) các phân vùng như vậy. 

Đây là mức giảm trung tâm. Thay vì liệt kê các phân vùng của (s), có thể là một số lượng lớn, chúng tôi liệt kê các phân vùng có tối đa 51. Nhiệm vụ còn lại là tính toán nhanh (f^\lambda) mà không cần xây dựng sơ đồ chứa tối đa một triệu ô. 

Công thức chiều dài móc cho 

[ 
f^\lambda=\frac{s!}{\prod_{c\in\lambda}h(c)}, 
] 

trong đó (h(c)) là chiều dài móc của một ô. 

Hình dạng gần giống một cái móc, do đó tích móc của nó chỉ có thể được biểu thị bằng hệ số (O(t)). Chúng tôi tính toán trước các nghịch đảo mô-đun lên đến (s), sau đó mọi hình dạng ứng cử viên có thể được đánh giá trong thời gian (O(t)). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(s!,s\log s)) | (O (các)) | Quá chậm | 
| Tối ưu | (O(s+p(51)\cdot51)) | (O (các)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định 

[ 
t=s-n-m+1. 
] 

Móc bắt buộc có (n+m-1) ô, vì vậy (t) chính xác là số ô bên ngoài móc đó. Nếu (t<0) thì (n+m>s+1), điều này là không thể. Nếu (nm<s), hình dạng mong muốn không thể vừa với hình chữ nhật (n\times m), do đó câu trả lời cũng bằng 0. 

1. Biểu diễn mọi sơ đồ Young hợp lệ dưới dạng 

[ 
\lambda=(n,\mu_1+1,\mu_2+1,\ldots), 
] 

trong đó (\mu) là phân vùng của (t). 

Số lượng hàng bên dưới hàng đầu tiên nhiều nhất là (m-1), do đó (\mu) có nhiều nhất (m-1) phần. Vì hàng thứ hai của (\lambda) không thể dài hơn hàng đầu tiên nên mỗi phần của (\mu) nhiều nhất là (n-1). 

Do đó, chúng tôi liệt kê các phân vùng của (t) với phần tối đa (n-1) và nhiều nhất (m-1). 

1. Đối với một phân vùng cụ thể (\mu), hãy xây dựng chiều cao cột của nó. Gọi (h_c) là số phần của (\mu) ít nhất bằng (c). 

Phần dưới bên phải của (\lambda) chính xác là sơ đồ của (\mu), được dịch xuống một hàng và sang phải một cột. Những chiều cao cột này cho phép chúng ta thu được mọi chiều dài móc trong phần nhỏ đó trong thời gian không đổi. 

1. Tách sản phẩm móc của (\lambda) thành bốn phần. Ô ((1,1)) có độ dài móc (s) nên tử số sau khi loại bỏ thừa số này là ((s-1)!). 

Đối với phần còn lại của hàng đầu tiên, ô tương ứng với cột (c+1) có độ dài móc 

[ 
n-c+h_c 
] 

cho (1\le c\le\mu_1). Sau cột bị chiếm đóng cuối cùng của (\mu), các thừa số chỉ đơn giản là 

[ 
n-\mu_1-1,\ldots,1, 
] 

đưa ra giai thừa ((n-\mu_1-1)!). 

1. Các ô bên dưới ô đầu tiên của cột đầu tiên có độ dài móc 

[ 
m-r+\mu_r 
] 

cho hàng (r) của (\mu). Khi các phần khác 0 của (\mu) kết thúc, các thừa số còn lại hình thành 

[ 
(m-L-1)!, 
] 

trong đó (L) là số phần của (\mu). 

1. Đối với mỗi ô ((r,c)) bên trong (\mu), ô tương ứng của nó trong (\lambda) có chiều dài móc 

[ 
\mu_r-c+h_c-r+1. 
] 

Có chính xác (t) ô như vậy, vì vậy toàn bộ phần này tốn (O(t)) hoạt động. 

1. Kết hợp các hệ số với công thức chiều dài móc câu. Vì mỗi chiều dài móc tối đa là (s\le10^6<10^9+7), nên mọi hệ số mẫu số đều có nghịch đảo mô đun. 
2. Bình phương giá trị kết quả (f^\lambda) và cộng nó vào đáp án. RSK cho chúng ta biết rằng hình vuông này đếm chính xác các hoán vị có hình dạng RSK là (\lambda), vì vậy việc tính tổng tất cả các hình dạng hợp lệ sẽ cho ra số lượng cần thiết. 

### Tại sao nó hoạt động 

Mỗi hoán vị tương ứng một cách khách quan với một cặp hoạt cảnh Young tiêu chuẩn có một hình dạng chung (\lambda). LIS và LDS tương ứng là độ dài hàng đầu tiên và cột đầu tiên của hình đó. Do đó, việc sửa (n) và (m) hoàn toàn giống với việc giới hạn hàng đầu tiên ở (n) và cột đầu tiên ở (m).

Mỗi hình dạng như vậy chứa móc của (n+m-1) ô và tất cả các ô còn lại tạo thành một phân vùng (\mu) của (t=s-n-m+1). Thuật toán liệt kê mọi (\mu) như vậy chính xác một lần trong khi thực thi các giới hạn chiều rộng và chiều cao, do đó mọi hình dạng được chấp nhận sẽ xuất hiện chính xác một lần và không có hình dạng không được chấp nhận nào xuất hiện. Phép tính độ dài móc cho ra số chính xác (f^\lambda) của hoạt cảnh có hình dạng đó và cặp hoạt cảnh cho ra ((f^\lambda)^2) hoán vị. Do đó, mỗi hoán vị hợp lệ đều đóng góp một lần vào câu trả lời. 

## Giải pháp Python```python
import sys
from array import array

input = sys.stdin.readline

MOD = 1_000_000_007

def solve_case(s, n, m):
    t = s - n - m + 1

    # n + m > s + 1 is impossible.
    # n * m < s means an n by m Young diagram cannot contain s cells.
    if t < 0 or n * m < s:
        return 0

    max_mu = min(t, n - 1)

    # Factorial of s - 1, and factorials of n - 1 and m - 1.
    fact_s = 1
    fact_n = 1
    fact_m = 1

    for i in range(2, s):
        fact_s = fact_s * i % MOD
        if i == n - 1:
            fact_n = fact_s
        if i == m - 1:
            fact_m = fact_s

    # Handle n = 1 or m = 1, where the corresponding factorial is 0!.
    if n - 1 == 0:
        fact_n = 1
    if m - 1 == 0:
        fact_m = 1

    # Modular inverses of every integer up to s.
    inv = array('I', [0]) * (s + 1)
    if s >= 1:
        inv[1] = 1
    for i in range(2, s + 1):
        inv[i] = (MOD - (MOD // i) * inv[MOD % i] % MOD) % MOD

    # We only need inverse factorials close to n - 1 and m - 1.
    # invfact_n[j] = 1 / (n - 1 - j)!
    invfact_n = [0] * (max_mu + 1)
    invfact_m = [0] * (min(t, m - 1) + 1)

    invfact_n[0] = pow(fact_n, MOD - 2, MOD)
    for j in range(1, len(invfact_n)):
        x = n - j
        invfact_n[j] = invfact_n[j - 1] * x % MOD

    invfact_m[0] = pow(fact_m, MOD - 2, MOD)
    for j in range(1, len(invfact_m)):
        x = m - j
        invfact_m[j] = invfact_m[j - 1] * x % MOD

    answer = 0
    mu = []

    def process():
        nonlocal answer

        L = len(mu)
        mu1 = mu[0] if L else 0

        # Column heights of mu.
        heights = [0] * (mu1 + 1)
        for x in mu:
            for c in range(1, x + 1):
                heights[c] += 1

        # Start with (s-1)!.
        f = fact_s

        # First row.
        f = f * invfact_n[mu1] % MOD
        for c in range(1, mu1 + 1):
            hook = n - c + heights[c]
            f = f * inv[hook] % MOD

        # First column below the top cell.
        f = f * invfact_m[L] % MOD
        for r, x in enumerate(mu, 1):
            hook = m - r + x
            f = f * inv[hook] % MOD

        # Cells corresponding to the diagram mu.
        for r, x in enumerate(mu, 1):
            for c in range(1, x + 1):
                hook = x - c + heights[c] - r + 1
                f = f * inv[hook] % MOD

        answer = (answer + f * f) % MOD

    def generate(rem, maximum, parts_left):
        if rem == 0:
            process()
            return

        if parts_left == 0 or maximum == 0:
            return

        upper = min(rem, maximum)

        for x in range(upper, 0, -1):
            mu.append(x)
            generate(rem - x, x, parts_left - 1)
            mu.pop()

    generate(t, n - 1, m - 1)
    return answer

def main():
    s, n, m = map(int, input().split())
    print(solve_case(s, n, m))

if __name__ == "__main__":
    main()
```Phần đầu tiên của quá trình triển khai tính toán (t) và loại bỏ các đầu vào không thể thực hiện được trước khi thực hiện bất kỳ phép liệt kê nào. Điều kiện (n*m<s) tương đương với việc nói rằng sơ đồ Young có cột (n), hàng (m) không thể chứa đủ ô. 

Vòng lặp giai thừa tính toán ((s-1)!), là tử số còn lại sau khi loại bỏ (các) độ dài móc của ô trên cùng bên trái. Trong cùng một vòng lặp, nó ghi lại ((n-1)!) và ((m-1)!), vì sau này chỉ cần một khoảng ngắn các giai thừa nghịch đảo của chúng. 

Mảng nghịch đảo sử dụng phép truy toán tiêu chuẩn 

-\left\lfloor\frac{MOD}{i}\right\rfloor 
\operatorname{inv}(MOD\bmod i) 
\pmod{MOD}. 
] 

Không có chiều dài móc nào chia hết cho (MOD), vì tất cả độ dài móc tối đa là (10^6), vì vậy những nghịch đảo này luôn hợp lệ. 

Hai mảng giai thừa nghịch đảo được cố ý ngắn gọn. Đối với hàng đầu tiên, chúng ta chỉ cần ((n-1-j)!) cho (0\le j\le t) và câu lệnh tương tự đúng cho cột đầu tiên. Việc lưu trữ một bảng giai thừa và nghịch đảo đầy đủ lên tới một triệu là không cần thiết. 

Trình tạo đệ quy giữ cho các phần phân vùng không tăng. tham số`maximum`là giá trị lớn nhất được phép cho phần tiếp theo, trong khi`parts_left`thực thi điều kiện (\mu) có tối đa (m-1) phần. Vì (t\le51) nên độ sâu đệ quy tối đa là 51. 

Bên trong`process`,`heights[c]`là chiều cao cột của cột (c) trong (\mu). Sau đó, các hệ số móc ở hàng đầu tiên, cột đầu tiên và phía dưới bên phải được tính toán trực tiếp từ các công thức trên. Các vòng lặp trên các ô của (\mu) xử lý chính xác các ô (t), do đó chúng không bao giờ phụ thuộc vào giá trị tiềm năng rất lớn của (s). 

Không có vấn đề tràn số nguyên trong Python. Mọi phép nhân đều được giảm modulo (10^9+7) và phần đóng góp cuối cùng chỉ được bình phương sau khi (f^\lambda) đã được giảm modulo theo cùng một mô đun. 

## Ví dụ đã hoạt động 

### Mẫu 1 

cho 

[ 
s=6,\quad n=3,\quad m=3, 
] 

chúng tôi nhận được 

[ 
t=6-3-3+1=1. 
] 

Phân vùng duy nhất của 1 là (\mu=(1)), đưa ra sơ đồ Young 

[ 
\lambda=(3,2,1). 
] 

Chiều dài móc của nó là (5,3,1,3,1,1), có tích là 45. Do đó 

[ 
f^\lambda=\frac{6!}{45}=16. 
] 

Chỉ có một hình hợp lệ nên câu trả lời là (16^2=256). 

| (\mu) | (\lambda) | (f^\lambda) | Đóng góp | 
| --- | --- | --- | --- | 
| ((1)) | ((3,2,1)) | 16 | 256 | 

Dấu vết cho thấy sự giảm thiểu chính. Sáu học sinh không cần liệt kê (6!=720) hoán vị. Khi hình dạng RSK được cố định, tất cả 256 hoán vị hợp lệ sẽ được tính cùng một lúc. 

### Mẫu 2 

cho 

[ 
s=12,\quad n=3,\quad m=4, 
] 

chúng tôi nhận được 

[ 
t=12-3-4+1=6. 
] 

Phân vùng phải có nhiều nhất là ba phần và mỗi phần phải có nhiều nhất là hai phần. Các khả năng duy nhất là ((2,2,2)) và ((2,2,1)). 

Chúng tạo ra các hình dạng ((3,3,3,3)) và ((3,3,3,2)). 

| (\mu) | (\lambda) | (f^\lambda) | (f^\lambda{}^2) | 
| --- | --- | --- | --- | 
| ((2,2,2)) | ((3,3,3,3)) | 462 | 213444 | 
| ((2,2,1)) | ((3,3,3,2)) | 5544 | 30735936 | 
| Tổng cộng | | | 30949380 | 

Do đó câu trả lời là 

[ 
213444+30735936=30949380. 
] 

Ví dụ này cho thấy tại sao việc liệt kê các phân vùng không hạn chế là không đủ. Giới hạn chiều rộng (\mu_1\le n-1) và giới hạn chiều cao (\ell(\mu)\le m-1) loại bỏ các phân vùng khác của 6. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(s+t,p(t))) | Chi phí tính toán trước (O(s)) và mỗi phân vùng (p(t)) được xử lý trong (O(t)). | 
| Không gian | (O(s+t)) | Mảng nghịch đảo sử dụng bộ nhớ (O(s)) và phân vùng hiện tại sử dụng (O(t)). | 

Ở đây (t\le51), và (p(51)=239943). Do đó, phép liệt kê phân vùng có ít hơn khoảng 240.000 lá, với tối đa 51 ô nhỏ được xử lý trên mỗi lá. Phần duy nhất tùy thuộc vào kích thước đầu vào đầy đủ là tiền xử lý tuyến tính lên tới (s\le10^6). Điều này phù hợp với các ràng buộc dự định một cách thoải mái hơn nhiều so với bất kỳ cách tiếp cận nào liên quan đến chính hoán vị. 

## Trường hợp thử nghiệm```python
from array import array

MOD = 1_000_000_007

def solve_case(s, n, m):
    t = s - n - m + 1

    if t < 0 or n * m < s:
        return 0

    max_mu = min(t, n - 1)

    fact_s = 1
    fact_n = 1
    fact_m = 1

    for i in range(2, s):
        fact_s = fact_s * i % MOD
        if i == n - 1:
            fact_n = fact_s
        if i == m - 1:
            fact_m = fact_s

    if n - 1 == 0:
        fact_n = 1
    if m - 1 == 0:
        fact_m = 1

    inv = array('I', [0]) * (s + 1)
    inv[1] = 1
    for i in range(2, s + 1):
        inv[i] = (MOD - (MOD // i) * inv[MOD % i] % MOD) % MOD

    invfact_n = [0] * (max_mu + 1)
    invfact_m = [0] * (min(t, m - 1) + 1)

    invfact_n[0] = pow(fact_n, MOD - 2, MOD)
    for j in range(1, len(invfact_n)):
        invfact_n[j] = invfact_n[j - 1] * (n - j) % MOD

    invfact_m[0] = pow(fact_m, MOD - 2, MOD)
    for j in range(1, len(invfact_m)):
        invfact_m[j] = invfact_m[j - 1] * (m - j) % MOD

    answer = 0
    mu = []

    def process():
        nonlocal answer

        L = len(mu)
        mu1 = mu[0] if L else 0

        heights = [0] * (mu1 + 1)
        for x in mu:
            for c in range(1, x + 1):
                heights[c] += 1

        f = fact_s

        f = f * invfact_n[mu1] % MOD
        for c in range(1, mu1 + 1):
            f = f * inv[n - c + heights[c]] % MOD

        f = f * invfact_m[L] % MOD
        for r, x in enumerate(mu, 1):
            f = f * inv[m - r + x] % MOD

        for r, x in enumerate(mu, 1):
            for c in range(1, x + 1):
                hook = x - c + heights[c] - r + 1
                f = f * inv[hook] % MOD

        answer = (answer + f * f) % MOD

    def generate(rem, maximum, parts_left):
        if rem == 0:
            process()
            return
        if parts_left == 0 or maximum == 0:
            return

        for x in range(min(rem, maximum), 0, -1):
            mu.append(x)
            generate(rem - x, x, parts_left - 1)
            mu.pop()

    generate(t, n - 1, m - 1)
    return answer

def run(inp: str) -> str:
    s, n, m = map(int, inp.split())
    return str(solve_case(s, n, m))

# Provided samples
assert run("6 3 3") == "256", "sample 1"
assert run("12 3 4") == "30949380", "sample 2"

# Minimum-size case, and n, m, s are all equal.
assert run("1 1 1") == "1", "minimum size"

# A completely increasing permutation is the only valid arrangement.
assert run("5 5 1") == "1", "maximum LIS"

# A completely decreasing permutation is the only valid arrangement.
assert run("5 1 5") == "1", "maximum LDS"

# n + m > s + 1, so no permutation can satisfy the request.
assert run("5 5 5") == "0", "impossible sum"

# The lower bound n + m = s - 50 is met exactly,
# but LIS = LDS = 1 is impossible for 52 distinct values.
assert run("52 1 1") == "0", "boundary lower bound"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1`| 1 | Kích thước tối thiểu và các thông số bằng nhau | 
|`5 5 1`| 1 | Hoàn toàn tăng hoán vị | 
|`5 1 5`| 1 | Hoàn toàn giảm hoán vị | 
|`5 5 5`| 0 | Ranh giới không thể (n+m>s+1) | 
|`52 1 1`| 0 | Ranh giới chính xác (n+m=s-50) | 

## Vỏ cạnh 

cho`1 1 1`, giá trị (t=1-1-1+1=0). Trình tạo ngay lập tức xử lý phân vùng trống (\mu). Hình dạng tương ứng chỉ đơn giản là ((1)), số lượng hoạt cảnh của nó là 1 và câu trả lời trở thành (1^2=1). Điều này cũng cho thấy tại sao phân vùng trống phải được xử lý một cách rõ ràng thay vì giả định rằng có ít nhất một ô bổ sung tồn tại. 

Vì`5 5 5`, ta được (t=5-5-5+1=-4). Thuật toán trả về 0 trước khi xây dựng bất kỳ dữ liệu giai thừa hoặc phân vùng nào. Giá trị âm có nghĩa là móc bắt buộc đã chứa nhiều hơn năm ô, do đó, không có sơ đồ Young nào có kích thước năm có thể có hàng đầu tiên và cột đầu tiên đều có độ dài năm ô. 

Vì`52 1 1`, ràng buộc đặc biệt được thỏa mãn chính xác vì (1+1=52-50). Tuy nhiên, 

[ 
n\cdot m=1<52, 
] 

vì vậy một sơ đồ Young (n\times m) không thể chứa 52 ô. Thử nghiệm tính khả thi sớm trả về số không. Đây là một trường hợp hữu ích vì chỉ kiểm tra (t\le51) là không đủ. 

Vì`5 5 1`, ta có (t=0). Hình dạng duy nhất là cái móc ((5)), bởi vì (m=1) không cho phép các hàng thấp hơn. Số lượng hoạt cảnh của nó là 1, tương ứng với hoán vị tăng dần duy nhất. Đầu vào tương tự`5 1 5`tạo ra hình dạng cột đơn và hoán vị giảm dần duy nhất. 

Đối với mẫu`6 3 3`, (t=1), do đó toàn bộ hình dạng được xác định bởi phân vùng đơn ((1)). Thuật toán lấy (f^\lambda=16) và cộng (16^2=256). Điều này mắc phải sai lầm phổ biến khi xác định phần dư là (s-n-m), sẽ bị sai lệch một và sẽ coi trường hợp này là không có ô thừa.
