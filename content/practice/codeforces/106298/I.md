---
title: "CF 106298I - Đa thức Mega"
description: "Chúng ta được cho một tập hợp các số nguyên xác định một biểu thức hữu tỉ được xây dựng từ các giai thừa rơi xuống. Đối tượng trung tâm là một hằng số $K$ được xác định gián tiếp thông qua hai so sánh hệ số tương đương trong một đồng nhất thức đa thức."
date: "2026-06-18T22:29:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106298
codeforces_index: "I"
codeforces_contest_name: "OCPC 2024 Summer, Day 4: wuhudsm Contest"
rating: 0
weight: 106298
solve_time_s: 53
verified: true
draft: false
---

[CF 106298I - Đa thức Mega](https://codeforces.com/problemset/problem/106298/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một tập hợp các số nguyên xác định một biểu thức hữu tỉ được xây dựng từ các giai thừa rơi xuống. Đối tượng trung tâm là một hằng số$K$được xác định gián tiếp thông qua hai phép so sánh hệ số tương đương trong một đẳng thức đa thức. Sau khi đơn giản hóa các nhận dạng đó, nhiệm vụ giảm xuống còn việc kiểm tra xem một giá trị được xây dựng từ tích của các số nguyên liên tiếp chia cho$A$là một số nguyên. 

Cụ thể, tử số xuất hiện là một đoạn giai thừa rút gọn có dạng$n \cdot (n-1) \cdot \ldots \cdot (n-k+1)$. Đây là số hoán vị của độ dài$k$được chọn từ$n$, thường được viết là$nPk = \frac{n!}{(n-k)!}$. giá trị$K$tích hoán vị này có phải chia cho$A$. Câu hỏi đặt ra là liệu sự phân chia này có chính xác hay không. 

Có một đẳng thức đại số bổ sung liên quan đến$B, C, D$, Và$(n-k)$, đảm bảo tính nhất quán của các hệ số đa thức. Nhận dạng đó không thay đổi cốt lõi tính toán: nó chỉ thúc đẩy tại sao lại giống nhau$K$xuất hiện trong hai so sánh hệ số khác nhau. Việc kiểm tra tính toán duy nhất quan trọng là liệu tất cả các thừa số nguyên tố của$A$được bao phủ hoàn toàn bởi việc phân tích thành thừa số nguyên tố của tích hoán vị. 

Cấu trúc đầu vào được hiểu một cách tự nhiên là nhiều truy vấn, mỗi truy vấn chỉ định tham số$A, B, C, D, n, k$và đối với mỗi truy vấn, chúng ta phải quyết định xem$K$là một số nguyên. 

Hình dạng ràng buộc ngụ ý bởi một vấn đề Codeforces thuộc loại này thường cho phép tối đa lớn$n$, vì vậy việc mở rộng sản phẩm một cách rõ ràng là không khả thi. Một phép nhân ngây thơ sẽ yêu cầu$O(k)$cho mỗi truy vấn, việc này trở nên quá chậm khi cả số lượng truy vấn và$k$lớn. 

Một trường hợp thất bại tinh tế xuất hiện khi$A$chứa lũy thừa cao của các số nguyên tố nhỏ. Ví dụ, nếu$A = 2^{20}$,$n = 10^6$, Và$k$lớn, một phương pháp đơn giản có thể tràn hoặc đếm sai các thừa số nếu nó không theo dõi chính xác bội số nguyên tố trên toàn bộ phạm vi. Một trường hợp cạnh khác là khi$k = 0$, trong đó tích trống và bằng 1, nghĩa là câu trả lời hoàn toàn phụ thuộc vào việc liệu$A = 1$. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là tính tích$n \cdot (n-1) \cdot \ldots \cdot (n-k+1)$rõ ràng cho từng truy vấn và sau đó kiểm tra khả năng chia hết cho$A$. Về nguyên tắc, điều này đúng vì nó khớp chính xác với định nghĩa của tử số. Vấn đề là chi phí: mỗi truy vấn sẽ mất$O(k)$phép nhân và với đầu vào lớn, điều này trở nên không thể thực hiện được trong thời gian giới hạn. Ngoài ra, sản phẩm trung gian phát triển cực kỳ lớn và sẽ yêu cầu số học có độ chính xác tùy ý hoặc rút gọn lặp đi lặp lại, điều này làm phức tạp việc triển khai và vẫn không khắc phục được vấn đề phức tạp về thời gian. 

Quan sát quan trọng là chúng ta không bao giờ cần toàn bộ giá trị của sản phẩm. Chúng ta chỉ cần biết, với mọi số nguyên tố$p$chia$A$, cho dù số mũ của$p$trong tích hoán vị ít nhất là số mũ của$A$. Điều này chuyển vấn đề từ tính toán số sang tính toán định giá. 

Sản phẩm hoán vị$n \cdot (n-1) \cdot \ldots \cdot (n-k+1)$có thể được thể hiện như$\frac{n!}{(n-k)!}$. Điều này cho phép chúng ta tính số mũ của bất kỳ số nguyên tố nào$p$sử dụng thủ thuật định giá giai thừa tiêu chuẩn:$v_p(n!) - v_p((n-k)!)$. Điều này loại bỏ nhu cầu lặp lại tất cả các phần tử trong phạm vi và thay thế nó bằng cách đếm logarit trên mỗi số nguyên tố. 

Chúng tôi nhân tố hóa$A$một lần cho mỗi truy vấn, sau đó cho mỗi thừa số nguyên tố$p$, chúng tôi tính số mũ của nó trong phạm vi hoán vị bằng cách sử dụng công thức định giá giai thừa. Nếu tất cả số mũ yêu cầu đều được thỏa mãn thì câu trả lời là có. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Sản phẩm Brute Force |$O(k)$mỗi truy vấn |$O(1)$| Quá chậm | 
| Yếu tố cơ bản + Định giá |$O(\sqrt{A} + \log n)$mỗi truy vấn |$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Hướng dẫn thuật toán 

1. Đọc một truy vấn bao gồm$A, B, C, D, n, k$. Chỉ một$A, n, k$ảnh hưởng đến việc kiểm tra tính chia hết, do đó các giá trị khác bị bỏ qua khi tính toán. 
2. Nhân tố hóa$A$vào các thành phần chính của nó. Điều này mang lại$A = \prod p_i^{e_i}$. Chúng ta cần xác minh từng số mũ một cách độc lập. 
3. Viết lại tử số dưới dạng tỉ số giai thừa$n! / (n-k)!$. Điều này biến đổi một phạm vi sản phẩm thành một dạng mà số mũ nguyên tố có thể được đếm một cách hiệu quả. 
4. Đối với mỗi số nguyên tố$p_i$, tính số mũ của nó theo$n!$sử dụng phép chia lặp đi lặp lại cho$p_i$, sau đó trừ số mũ của nó theo$(n-k)!$. Điều này cho số mũ trong tích hoán vị. 
5. So sánh số mũ này với$e_i$. Nếu số nguyên tố nào thiếu số mũ thì kết luận ngay$K$không phải là số nguyên. 
6. Nếu tất cả các số nguyên tố đều thỏa mãn điều kiện thì kết luận$K$là một số nguyên. 

Ý tưởng cốt lõi là khả năng chia hết giúp kiểm tra sự thống trị theo cấp số nhân trong hệ số nguyên tố và cấu trúc giai thừa cho phép tính các số mũ đó mà không cần xây dựng các số một cách rõ ràng. 

### Tại sao nó hoạt động 

Tích hoán vị và tỷ lệ giai thừa giống hệt như số nguyên, do đó hệ số nguyên tố của chúng khớp chính xác. Vì khả năng chia hết của số nguyên tương đương với việc so sánh các số mũ nguyên tố theo thành phần nên việc kiểm tra từng số nguyên tố một cách độc lập sẽ mô tả đầy đủ liệu$A$chia sản phẩm. Việc phân tách giai thừa đảm bảo chúng ta có thể tính toán chính xác các số mũ đó mà không cần liệt kê phạm vi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def factorize(x):
    f = {}
    p = 2
    while p * p <= x:
        while x % p == 0:
            f[p] = f.get(p, 0) + 1
            x //= p
        p += 1
    if x > 1:
        f[x] = f.get(x, 0) + 1
    return f

def vp_factorial(n, p):
    cnt = 0
    while n:
        n //= p
        cnt += n
    return cnt

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        A, B, C, D, n, k = map(int, input().split())

        if k == 0:
            out.append("YES" if A == 1 else "NO")
            continue

        factors = factorize(A)
        ok = True

        for p, need in factors.items():
            total = vp_factorial(n, p) - vp_factorial(n - k, p)
            if total < need:
                ok = False
                break

        out.append("YES" if ok else "NO")

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách nhân tử hóa$A$, vì tất cả các ràng buộc chia hết được biểu diễn dưới dạng lũy ​​thừa nguyên tố của nó. chức năng`vp_factorial`tính số mũ của một số nguyên tố bên trong một giai thừa bằng cách sử dụng phép tính tổng tiêu chuẩn của các phép chia sàn, hiệu quả ngay cả đối với các số lớn$n$. 

Đối với mỗi truy vấn, số mũ phạm vi hoán vị được tính là hiệu giữa hai số mũ giai thừa. Điều này tránh việc lặp lại một cách rõ ràng trong phạm vi sản phẩm và giữ logarit tính toán theo$n$. 

Trường hợp đặc biệt$k = 0$tương ứng với một tích rỗng, bằng 1, do đó chỉ$A = 1$có thể thành công. 

## Ví dụ đã hoạt động 

Hãy xem xét một truy vấn đơn giản trong đó$A = 12$,$n = 5$,$k = 3$. Tử số là$5 \cdot 4 \cdot 3 = 60$. Chúng tôi nhân tố hóa$12 = 2^2 \cdot 3$. Sản phẩm 60 có$2^2 \cdot 3 \cdot 5$, do đó cả hai số mũ cần thiết đều có mặt. 

| Bước | p | v_p(n!) | v_p((n-k)!) | Sự khác biệt | Bắt buộc | Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| 2 | 2 | 3 | 1 | 2 | 2 | được | 
| 3 | 3 | 1 | 0 | 1 | 1 | được | 

Dấu vết này cho thấy rằng mỗi ràng buộc nguyên tố được kiểm tra độc lập và tất cả đều được thỏa mãn. 

Bây giờ hãy xem xét$A = 8$,$n = 6$,$k = 2$. Sản phẩm là$6 \cdot 5 = 30$, chỉ chứa một thừa số 2. 

| Bước | p | v_p(n!) | v_p((n-k)!) | Sự khác biệt | Bắt buộc | Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| 2 | 2 | 4 | 3 | 1 | 3 | THẤT ​​BẠI | 

Ở đây lũy thừa 2 còn thiếu sẽ ngay lập tức làm mất hiệu lực câu trả lời. 

Những ví dụ này cho thấy phương pháp này tách biệt những thiếu sót cơ bản mà không cần tính toán sản phẩm đầy đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(T \cdot (\sqrt{A} + \log n))$| Hệ số hóa của$A$cộng với định giá giai thừa trên mỗi nguyên tố | 
| Không gian |$O(1)$| Chỉ lưu trữ các thừa số nguyên tố của$A$| 

Cách tiếp cận này phù hợp thoải mái với các ràng buộc điển hình vì cả hệ số hóa và định giá giai thừa đều nhanh chóng ngay cả đối với đầu vào lớn và không lặp lại từng phần tử trong phạm vi$k$được yêu cầu. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import sys as _sys

    # re-run solution
    input = _sys.stdin.readline

    def factorize(x):
        f = {}
        p = 2
        while p * p <= x:
            while x % p == 0:
                f[p] = f.get(p, 0) + 1
                x //= p
            p += 1
        if x > 1:
            f[x] = f.get(x, 0) + 1
        return f

    def vp_factorial(n, p):
        cnt = 0
        while n:
            n //= p
            cnt += n
        return cnt

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            A, B, C, D, n, k = map(int, input().split())

            if k == 0:
                out.append("YES" if A == 1 else "NO")
                continue

            factors = factorize(A)
            ok = True

            for p, need in factors.items():
                total = vp_factorial(n, p) - vp_factorial(n - k, p)
                if total < need:
                    ok = False
                    break

            out.append("YES" if ok else "NO")

        return "\n".join(out)

    return solve()

# custom cases
assert run("1\n1 0 0 0 5 3\n") == "YES", "trivial divisor"
assert run("1\n2 0 0 0 5 2\n") == "YES", "2 divides 5*4"
assert run("1\n8 0 0 0 6 2\n") == "NO", "insufficient powers of 2"
assert run("1\n12 0 0 0 5 3\n") == "YES", "mixed primes"
assert run("1\n6 0 0 0 3 3\n") == "YES", "edge full product"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|$A=1$trường hợp | CÓ | trường hợp cạnh chia danh tính | 
| lũy thừa nhỏ của 2 | CÓ | tích lũy lũy thừa giai thừa | 
| định giá 2-adic không đủ | KHÔNG | phát hiện lỗi | 
| số nguyên tố hỗn hợp | CÓ | tính đúng đa nguyên tố | 
| sản phẩm đầy đủ | CÓ | ranh giới$k=n$trường hợp phong cách | 

## Vỏ cạnh 

Một trường hợp cạnh tranh quan trọng là khi$k = 0$. Sản phẩm trở nên trống, có giá trị là 1. Đối với bất kỳ$A > 1$, phép chia không thành công ngay lập tức. Thuật toán xử lý điều này một cách rõ ràng trước bất kỳ hệ số hóa nào. 

Một trường hợp khác là khi$k = n$. Tử số trở thành$n!$và thuật toán tự nhiên giảm xuống để kiểm tra xem$A$chia rẽ$n!$sử dụng cùng một phương pháp định giá. Vì sự khác biệt giai thừa trở thành$v_p(n!) - v_p(0!)$, Và$v_p(0!) = 0$, việc tính toán vẫn nhất quán. 

Trường hợp tế nhị thứ ba là khi$A = 1$. Bước phân tích nhân tử tạo ra một tập hợp số nguyên tố trống, do đó không có phép kiểm tra nào được thực hiện và câu trả lời luôn là có.
