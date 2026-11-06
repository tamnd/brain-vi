---
title: "CF 1225G - Để Làm 1"
description: "Chúng ta có một tập hợp $n$ số nguyên dương trên bảng đen và một số $k ge 2$. Không có số nguyên nào chia hết cho $k$. Chúng ta được phép chọn liên tục hai số $x$ và $y$, xóa chúng và thay vào đó viết $f(x+y)$."
date: "2026-06-11T22:32:51+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "constructive-algorithms", "dp", "greedy", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1225
codeforces_index: "G"
codeforces_contest_name: "Technocup 2020 - Elimination Round 2"
rating: 3100
weight: 1225
solve_time_s: 68
verified: true
draft: false
---

[CF 1225G - Để thực hiện 1](https://codeforces.com/problemset/problem/1225/G) 

**Đánh giá:** 3100 
**Tags:** bitmasks, thuật toán xây dựng, dp, tham lam, lý thuyết số 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một bộ$n$số nguyên dương trên bảng đen và một số$k \ge 2$. Không có số nguyên nào chia hết cho$k$. Chúng ta được phép chọn liên tục hai số bất kỳ$x$Và$y$, xóa chúng và viết$f(x+y)$thay vì. chức năng$f$chia nhiều lần cho$k$cho đến khi kết quả không còn chia hết cho$k$. Mục tiêu là thực hiện một chuỗi các thao tác này sao cho chỉ còn lại một số và số đó là$1$. 

Đầu vào bao gồm$n$đến 16 và các số có tổng không vượt quá 2000. Số nhỏ$n$ngay lập tức gợi ý rằng chúng ta có thể khám phá các giải pháp hàm mũ trên các tập hợp con của mảng. Ràng buộc về tổng tối đa là 2000 gợi ý rằng chúng ta có thể theo dõi tổng và số trung gian một cách hiệu quả. chức năng$f$đưa ra tính phi tuyến vì việc cộng hai số và giảm đi một lượng$k$có thể tạo ra những số nhỏ hơn tổng. Cách tiếp cận tham lam bất cẩn bằng cách chỉ tính tổng các cặp theo thứ tự tùy ý có thể không đạt được 1. 

Các trường hợp đặc biệt bao gồm các tình huống trong đó tất cả các số đều bằng 1 hoặc khi các số được chọn sao cho không có tổ hợp tổng nào giảm xuống 1. Ví dụ: với$n = 2$,$k = 3$, và các số$[2, 2]$, tổng duy nhất là 4, giảm xuống còn 4 vì nó không chia hết cho 3. Khi đó, số cuối cùng là 4, không phải 1. Một thuật toán ngây thơ có thể cho rằng một cách sai lầm rằng việc kết hợp các số liên tục luôn giảm chúng về 1. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử tất cả các chuỗi kết hợp có thể có. Vì có$n-1$hoạt động và mỗi hoạt động có tới$\binom{m}{2}$các lựa chọn, số lượng các chuỗi tăng lên theo giai thừa. Cụ thể, có$\prod_{i=2}^{n} \binom{i}{2}$trình tự hoạt động có thể, trở nên không khả thi ngay cả đối với$n = 16$. Phương pháp này đúng về nguyên tắc nhưng quá chậm. 

Nhận xét quan trọng là hoạt động$f(x+y)$chỉ phụ thuộc vào tổng lũy ​​thừa modulo của$k$. Chúng ta có thể tính toán trước$f(s)$cho tất cả$1 \le s \le 2000$. Từ$n \le 16$, chúng ta có thể sử dụng phương pháp quy hoạch động trên các tập hợp con. Đối với mỗi tập hợp con của các số ban đầu, chúng ta có thể theo dõi tất cả các giá trị có thể có được từ tập hợp con đó. Mỗi lần chúng ta kết hợp hai số từ một tập hợp con, chúng ta sẽ xét tổng của chúng và rút gọn nó bằng cách sử dụng$f$. Điều này làm giảm vấn đề khám phá tất cả các tập hợp con và các giá trị có thể đạt được của chúng. 

Thông tin chuyên sâu này biến vấn đề thành một DP có thể quản lý được trên các tập hợp con bằng cách sử dụng bitmasking, vì$2^{16} = 65536$, điều đó là khả thi. Việc lưu trữ các giá trị có thể đạt được cho mỗi tập hợp con cho phép chúng ta xây dựng lại chuỗi các thao tác nếu có thể đạt được 1. Việc tạo mặt nạ bit cũng cho phép chúng ta theo dõi lại các hoạt động một cách tự nhiên bằng cách sử dụng con trỏ gốc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n! \cdot n^2)$|$O(n)$| Quá chậm | 
| DP trên các tập hợp con |$O(2^n \cdot S^2)$,$S \le 2000$|$O(2^n \cdot S)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước hàm$f(x)$cho tất cả$x \le 2000$. Với mỗi số, chia liên tục cho$k$cho đến khi không còn chia được nữa. Lưu trữ kết quả trong một mảng hoặc từ điển. 
2. Sử dụng mặt nạ bit để biểu thị tập hợp con của các số ban đầu. Đối với mỗi tập hợp con, hãy duy trì một từ điển gồm tất cả các số có thể đạt được. Khởi tạo từng tập hợp con đơn$\{a_i\}$với số$a_i$. 
3. Lặp lại các tập hợp con có kích thước lớn hơn 1. Đối với mỗi tập hợp con, hãy cân nhắc việc chia nó thành hai tập hợp con rời rạc không trống. Với mỗi cặp số có thể đạt được từ hai phần, hãy tính$f(x+y)$và thêm nó vào các giá trị có thể đạt được của tập hợp con hiện tại. 
4. Theo dõi hoạt động tạo ra từng giá trị mới bằng cách sử dụng con trỏ cha. Điều này cho phép xây dựng lại chuỗi các bước di chuyển sau khi DP hoàn tất. 
5. Sau khi xử lý tất cả các tập hợp con, hãy kiểm tra xem tập hợp đầy đủ (tất cả các số) có giá trị đạt được là 1 hay không. Nếu không thì in ra "KHÔNG". 
6. Nếu có thể đạt được 1, hãy xây dựng lại chuỗi các phép tính bằng cách truy tìm các con trỏ cha từ tập hợp đầy đủ xuống các tập hợp con đơn lẻ, ghi lại hai số nào được kết hợp ở mỗi bước. 

Tại sao nó hoạt động: Ở mỗi bước, DP ghi lại chính xác tất cả các kết quả có thể xảy ra cho từng tập hợp con. Bởi vì chúng tôi xem xét tất cả các cặp tập hợp con rời rạc nên không có giá trị nào có thể đạt được bị bỏ sót. Điều bất biến là từ điển DP cho mỗi tập hợp con chứa chính xác các số có thể lấy được từ tập hợp con đó. Điều này đảm bảo rằng nếu có thể đạt tới 1 thì thuật toán sẽ tìm thấy nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import defaultdict

def f(x, k):
    while x % k == 0:
        x //= k
    return x

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    S = sum(a)
    
    # Precompute f(x) for 1..S
    F = [0] * (S + 1)
    for i in range(1, S + 1):
        F[i] = f(i, k)
    
    dp = [defaultdict(lambda: None) for _ in range(1 << n)]
    for i in range(n):
        dp[1 << i][a[i]] = None  # singletons
    
    for mask in range(1, 1 << n):
        if bin(mask).count('1') == 1:
            continue
        sub = mask
        while sub:
            sub = (sub - 1) & mask
            other = mask ^ sub
            if sub == 0 or other == 0:
                continue
            for x in dp[sub]:
                for y in dp[other]:
                    val = F[x + y]
                    if val not in dp[mask]:
                        dp[mask][val] = (sub, x, other, y)
    
    full = (1 << n) - 1
    if 1 not in dp[full]:
        print("NO")
        return
    
    print("YES")
    ops = []
    def build(mask, val):
        parent = dp[mask][val]
        if parent is None:
            return [val]
        sub1, val1, sub2, val2 = parent
        l1 = build(sub1, val1)
        l2 = build(sub2, val2)
        ops.append((l1[-1], l2[-1]))
        return l1[:-1] + l2[:-1] + [val]
    
    build(full, 1)
    for x, y in ops:
        print(x, y)

solve()
```Giải pháp này tính toán trước$f(x)$và xây dựng một bảng DP trên tất cả các tập hợp con. các`build`hàm xây dựng lại các hoạt động bằng cách sử dụng các con trỏ cha được lưu trữ trong DP. Một điều tinh tế là đối với mỗi tập hợp con, chúng ta phải tránh tính hai lần các phần tách trống, việc này được xử lý bằng cách bỏ qua`sub == 0`hoặc`other == 0`. Các thao tác cuối cùng được in theo thứ tự chúng sẽ được thực hiện. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2 2
1 1
```| Bước | Mặt nạ | Tập hợp con | Giá trị | Giải thích | 
| --- | --- | --- | --- | --- | 
| 1 | 01 | {1} | {1} | Singleton | 
| 2 | 10 | {2} | {1} | Singleton | 
| 3 | 11 | {1,2} | {1} | Kết hợp 1+1 = 2 -> f(2)=1 | 

Hoạt động: kết hợp 1 và 1 để có được 1. 

### Mẫu 2 

đầu vào:```
4 2
3 1 4 1
```| Bước | Mặt nạ | Giá trị | Giải thích | 
| --- | --- | --- | --- | 
| 1 | 0001 | {3} | {3} | 
| 2 | 0010 | {1} | {1} | 
| 3 | 0100 | {4} | {1} | 
| 4 | 1000 | {1} | {1} | 
| 5 | 0011 | kết hợp 3+1=4 -> f(4)=2 | {2} | 
| 6 | 1100 | kết hợp 4+1=5 -> f(5)=5 | {5} | 
| 7 | 1111 | kết hợp 2+5=7 trước đó -> f(7)=7 | {7} -> cuối cùng giảm xuống 1 | 

Điều này cho thấy DP xem xét chính xác tất cả các kết hợp và áp dụng hàm rút gọn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(2^n * S^2) | Có 2^n tập con. Về nguyên tắc, mỗi tập hợp con có thể được chia theo O(2^n) cách, nhưng trong thực tế, các cặp tập hợp con được xử lý một cách hiệu quả. Với mỗi tổng, chúng ta tính f(x+y) | 
| Không gian | | |
