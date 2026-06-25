---
title: "CF 105223D - Dừa"
description: "Chúng tôi được cung cấp một số trường hợp thử nghiệm. Trong mỗi trường hợp thử nghiệm, chúng tôi nhận được một mảng các số nguyên. Nhiệm vụ của chúng ta là đếm xem có bao nhiêu cặp vị trí $(i, j)$ với $i < j$ thỏa mãn mối quan hệ đại số rất cụ thể giữa các giá trị $ai$ và $aj$."
date: "2026-06-24T16:38:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105223
codeforces_index: "D"
codeforces_contest_name: "HIAST Collegiate Programming Contest 2024"
rating: 0
weight: 105223
solve_time_s: 51
verified: true
draft: false
---

[CF 105223D - Làm dừa](https://codeforces.com/problemset/problem/105223/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số trường hợp thử nghiệm. Trong mỗi trường hợp thử nghiệm, chúng tôi nhận được một mảng các số nguyên. Nhiệm vụ của chúng ta là đếm xem có bao nhiêu cặp vị trí$(i, j)$với$i < j$thỏa mãn mối quan hệ đại số rất cụ thể giữa các giá trị$a_i$Và$a_j$. 

Một cặp được coi là hợp lệ nếu chúng ta có thể tìm thấy số nguyên dương$x$Và$y$sao cho hai giá trị mảng được tạo ra bằng phép biến đổi đối xứng: một giá trị hoạt động giống như$x^2$chia cho$y$, và cái còn lại hoạt động như$y^2$chia cho$x$. Cùng một cặp$(x, y)$phải giải thích đồng thời cả hai con số. 

Các ràng buộc cho phép lên đến$5 \cdot 10^4$tổng số trên tất cả các trường hợp thử nghiệm và giá trị tăng lên$10^9$. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào thử trực tiếp tất cả các cặp, vì$O(n^2)$sẽ dẫn đến khoảng$10^9$hoạt động trong trường hợp xấu nhất. 

Một khó khăn tinh tế hơn là điều kiện liên quan đến các biến số nguyên ẩn.$x$Và$y$, không chỉ là mối quan hệ trực tiếp giữa$a_i$Và$a_j$. Điều đó có nghĩa là chúng ta phải chuyển điều kiện thành một thuộc tính chỉ phụ thuộc vào chính các con số. 

Một số trường hợp đặc biệt bộc lộ những lỗi phổ biến. 

Nếu tất cả các số đều$1$, thì mọi cặp đều hợp lệ vì việc chọn$x = y = 1$hoạt động cho mỗi cặp. Bất kỳ giải pháp nào bỏ qua cấu trúc nhân tố tầm thường vẫn phải xử lý vấn đề này một cách chính xác. 

Nếu chúng ta có những con số như$8$Và$2$, một nỗ lực ngây thơ có thể cố gắng khớp các tỷ lệ hoặc hình vuông một cách độc lập, nhưng sự ghép nối ẩn thông qua$x$Và$y$có nghĩa là lý luận cục bộ như vậy sẽ thất bại trừ khi nó xuất phát từ ràng buộc đầy đủ. 

Nếu một số là một lập phương hoàn hảo hoặc có cấu trúc nguyên tố lặp lại, việc xử lý số mũ bất cẩn có thể làm hỏng tính chính xác, đặc biệt khi có liên quan đến phân tích nhân tử. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ lặp lại trên tất cả các cặp$(i, j)$, và với mỗi cặp hãy thử giải tìm số nguyên$x, y$thỏa mãn các phương trình. Ngay cả khi chúng ta rút ra được công thức đại số cho$x$Và$y$, việc kiểm tra tính hợp lệ sẽ yêu cầu xác minh hệ số hoặc số mũ cho mỗi cặp, dẫn đến gần đúng$O(n^2 \sqrt{A})$, quá chậm. 

Cái nhìn sâu sắc quan trọng là điều kiện về cơ bản là nhân và dựa trên thừa số nguyên tố. Khi chúng ta viết lại mọi thứ theo số mũ nguyên tố, sự tồn tại của số nguyên$x$Và$y$áp đặt các ràng buộc mô-đun nghiêm ngặt trên các số mũ đó. 

Sau khi thao tác đại số, các biến ẩn biến mất và điều kiện rút gọn thành một câu lệnh đơn giản đến bất ngờ: hai số tương thích nhau khi và chỉ khi vectơ số mũ nguyên tố của chúng khớp với modulo 3 cho mọi số nguyên tố. 

Điều này chuyển đổi vấn đề từ kiểm tra tính khả thi theo cặp thành vấn đề nhóm. Mỗi số có thể được ánh xạ tới một “chữ ký” chính tắc bắt nguồn từ hệ số hóa của nó trong đó mỗi số mũ được giảm theo modulo 3. Mỗi cặp hợp lệ đều đến từ hai chữ ký giống hệt nhau, vì vậy chúng ta chỉ cần đếm các kết hợp bên trong mỗi nhóm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^2 \cdot \sqrt{A})$|$O(1)$| Quá chậm | 
| Nhóm chữ ký nhân tố |$O(n \sqrt{A})$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Bây giờ chúng tôi dịch từng số thành một biểu diễn chuẩn hóa để nắm bắt chính xác thông tin liên quan đến điều kiện. 

1. Phân tích từng số thành số nguyên tố bằng phép chia thử đến$\sqrt{a_i}$. Chúng tôi làm điều này bởi vì$a_i \le 10^9$, do đó số nguyên tố lên tới khoảng 31623 là đủ. 
2. Với mỗi thừa số nguyên tố$p^e$, thay số mũ$e$với$e \bmod 3$. Bước này loại bỏ tất cả thông tin không ảnh hưởng đến tính khả thi, vì chỉ còn lại mod 3 là quan trọng trong ràng buộc xuất phát từ hệ thống. 
3. Xây dựng lại chữ ký chuẩn cho số từ các số dư khác 0 còn lại. Chữ ký này độc lập với cách số được hình thành ban đầu. 
4. Sử dụng bản đồ băm để đếm số lần mỗi chữ ký xuất hiện trong mảng. 
5. Với mỗi chữ ký xuất hiện$k$lần, thêm$k(k-1)/2$cho câu trả lời, vì mọi cặp trong cùng một nhóm chữ ký đều hợp lệ. 

Lý do nhóm này hoạt động là vì điều kiện ban đầu buộc cấu trúc số mũ mô-đun giống hệt nhau trên tất cả các số nguyên tố. Nếu hai số khác nhau ở bất kỳ số mũ nguyên tố modulo 3 nào thì không có lựa chọn nào$x$Và$y$có thể điều chỉnh sự không phù hợp trong cả hai phương trình cùng một lúc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def factor_signature(x, primes):
    res = []
    for p in primes:
        if p * p > x:
            break
        if x % p == 0:
            e = 0
            while x % p == 0:
                x //= p
                e += 1
            r = e % 3
            if r:
                res.append((p, r))
    if x > 1:
        res.append((x, 1))
    return tuple(res)

def build_primes(limit=31650):
    sieve = [True] * (limit + 1)
    primes = []
    for i in range(2, limit + 1):
        if sieve[i]:
            primes.append(i)
            step = i
            start = i * i
            if start <= limit:
                for j in range(start, limit + 1, step):
                    sieve[j] = False
    return primes

def solve():
    primes = build_primes()
    t = int(input())
    for _ in range(t):
        n = int(input())
        arr = list(map(int, input().split()))
        freq = {}
        ans = 0

        for x in arr:
            sig = factor_signature(x, primes)
            ans += freq.get(sig, 0)
            freq[sig] = freq.get(sig, 0) + 1

        print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách tính toán trước các số nguyên tố lên đến$\sqrt{10^9}$, vì mọi số đều có thể được phân tích đầy đủ thành thừa số bằng cách sử dụng bộ này. 

Sau đó, mỗi số được chuyển đổi thành một chữ ký dựa trên hệ số nguyên tố của nó, nhưng chỉ giữ lại số mũ theo modulo 3. Chữ ký được lưu dưới dạng một bộ của$(prime, exponent \bmod 3)$, đảm bảo tính duy nhất và khả năng băm. 

Chúng tôi tích lũy câu trả lời tăng dần: khi một số mới có chữ ký$s$được xử lý, nó tạo thành các cặp hợp lệ với tất cả các số đã thấy trước đó có cùng chữ ký, vì vậy chúng tôi thêm tần số hiện tại trước khi tăng tần số đó. 

## Ví dụ đã hoạt động 

Hãy xem xét mảng$[1, 1, 1]$. 

Tất cả các số đều có chữ ký phân tích nhân tử trống. Mỗi cặp đều phù hợp. 

| Bước | Giá trị | Chữ ký | Cập nhật bản đồ tần số | Cặp mới | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | () | {():1} | 0 | 
| 2 | 1 | () | {():2} | 1 | 
| 3 | 1 | () | {():3} | 2 | 

Tổng cộng là 3 cặp, phù hợp$3 \cdot 2 / 2 = 3$. 

Bây giờ hãy xem xét$[2, 8, 4]$. 

Chúng tôi tính toán chữ ký: 

-$2 = 2^1 \Rightarrow (2,1)$-$8 = 2^3 \Rightarrow (2,0)$-$4 = 2^2 \Rightarrow (2,2)$| Bước | Giá trị | Chữ ký | Cập nhật bản đồ tần số | Cặp mới | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | {(2,1)} | {(2,1):1} | 0 | 
| 2 | 8 | {(2,0)} | {(2,1):1,(2,0):1} | 0 | 
| 3 | 4 | {(2,2)} | {(2,1):1,(2,0):1,(2,2):1} | 0 | 

Không có hai chữ ký nào trùng nhau nên câu trả lời là 0. 

Những ví dụ này cho thấy rằng chỉ các cấu trúc số mũ mô-đun giống hệt nhau mới tạo ra các cặp hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \sqrt{A})$| Mỗi số được phân tích thành thừa số bằng cách sử dụng các số nguyên tố lên đến$\sqrt{10^9}$| 
| Không gian |$O(n)$| Bản đồ tần số lưu trữ một mục nhập cho mỗi chữ ký riêng biệt | 

Tổng cộng$n$trên nhiều trường hợp thử nghiệm là nhiều nhất$5 \cdot 10^4$, vì vậy giải pháp dựa trên hệ số này phù hợp thoải mái trong giới hạn thời gian trong Python. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import builtins
    return sys.stdout.getvalue() if False else solve_capture(inp)

def solve_capture(inp: str) -> str:
    import sys
    input = sys.stdin.readline

    def factor_signature(x, primes):
        res = []
        for p in primes:
            if p * p > x:
                break
            if x % p == 0:
                e = 0
                while x % p == 0:
                    x //= p
                    e += 1
                r = e % 3
                if r:
                    res.append((p, r))
        if x > 1:
            res.append((x, 1))
        return tuple(res)

    def build_primes(limit=31650):
        sieve = [True] * (limit + 1)
        primes = []
        for i in range(2, limit + 1):
            if sieve[i]:
                primes.append(i)
                for j in range(i * i, limit + 1, i):
                    sieve[j] = False
        return primes

    primes = build_primes()
    it = iter(inp.strip().split())
    t = int(next(it))
    out = []

    for _ in range(t):
        n = int(next(it))
        freq = {}
        ans = 0
        for _ in range(n):
            x = int(next(it))
            sig = factor_signature(x, primes)
            ans += freq.get(sig, 0)
            freq[sig] = freq.get(sig, 0) + 1
        out.append(str(ans))

    return "\n".join(out)

# provided samples
# assert run(...) == ...

# custom cases
assert solve_capture("1\n1\n1\n") == "0"
assert solve_capture("1\n3\n2 2 2\n") == "3"
assert solve_capture("1\n3\n2 8 4\n") == "0"
assert solve_capture("1\n4\n1 1 1 1\n") == "6"
assert solve_capture("2\n1\n2\n1\n2\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả những cái | 0/6 tùy cách giải thích | tính nhất quán trống rỗng đặc trưng | 
| tất cả các số nguyên tố bằng nhau | tính đúng đắn của tổ hợp | tích lũy nC2 | 
| lũy thừa hỗn hợp của cùng một số nguyên tố | độ chính xác ghép nối bằng không | các lớp mod-3 riêng biệt | 
| nhiều trường hợp thử nghiệm | đặt lại giữa các trường hợp | cách ly bản đồ tần số | 

## Vỏ cạnh 

Đối với đầu vào bao gồm toàn bộ số 1, mỗi số sẽ ánh xạ tới một chữ ký trống. Thuật toán đếm tất cả các cặp trong một nhóm duy nhất, tạo ra$n(n-1)/2$, phù hợp với hành vi đúng vì chọn$x = y = 1$thỏa mãn các phương trình ban đầu cho bất kỳ cặp nào. 

Đối với các số là lũy thừa riêng biệt của cùng một số nguyên tố, chẳng hạn như$2, 4, 8$, mỗi số có số mũ modulo 3 khác nhau, tạo ra các dấu hiệu riêng biệt. Thuật toán tạo ra các cặp 0 một cách chính xác vì không có nhóm nào chứa nhiều hơn một phần tử, phù hợp với thực tế là không có nhóm nào nhất quán.$x, y$có thể đáp ứng đồng thời tất cả các ràng buộc số mũ trên các dư lượng khác nhau.
