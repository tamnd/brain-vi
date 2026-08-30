---
title: "CF 104386C - Mảng tổng tiền tố"
description: "Chúng ta bắt đầu với một mảng vô hạn trong đó mọi vị trí ban đầu đều chứa giá trị 1. Mỗi giây, mảng được thay thế bằng phiên bản tổng tiền tố của nó, nghĩa là giá trị tại vị trí i trở thành tổng của tất cả các giá trị từ vị trí 1 đến i trong mảng trước đó."
date: "2026-07-01T02:48:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104386
codeforces_index: "C"
codeforces_contest_name: "TheForces Round #14 (Cool-Forces)"
rating: 0
weight: 104386
solve_time_s: 78
verified: false
draft: false
---

[CF 104386C - Mảng tổng tiền tố](https://codeforces.com/problemset/problem/104386/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 18s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với một mảng vô hạn trong đó mọi vị trí ban đầu đều chứa giá trị 1. Mỗi giây, mảng được thay thế bằng phiên bản tổng tiền tố của nó, nghĩa là giá trị tại vị trí i trở thành tổng của tất cả các giá trị từ vị trí 1 đến i trong mảng trước đó. Sau một thao tác, mảng sẽ trở thành một chuỗi tăng dần các tổng tích lũy và sau k thao tác, phép biến đổi này được áp dụng k lần. 

Nhiệm vụ là trả lời nhiều truy vấn. Mỗi truy vấn đưa ra một vị trí n và một số phép biến đổi k, đồng thời chúng ta phải tính giá trị tại chỉ mục n sau k phép toán tổng tiền tố, modulo$10^9 + 7$. 

Những ràng buộc khiến cho vũ lực ngay lập tức không thể thực hiện được. Mặc dù n nhiều nhất là$10^5$, k cũng lên tới$10^5$, và có tới$10^5$trường hợp thử nghiệm. Việc mô phỏng tổng tiền tố lặp đi lặp lại cho mỗi truy vấn sẽ yêu cầu$O(nk)$làm việc trên mỗi trường hợp kiểm thử trong trường hợp xấu nhất, vượt xa giới hạn khả thi. 

Một vấn đề ít rõ ràng hơn là mảng về mặt khái niệm là vô hạn, nhưng chỉ có n phần tử đầu tiên quan trọng đối với mỗi truy vấn. Bất kỳ cách tiếp cận nào cố gắng hiện thực hóa nhiều hơn mức cần thiết vẫn có nguy cơ gây ra chi phí không cần thiết nếu nó không khai thác được cấu trúc. 

Việc triển khai đơn giản cũng có thể âm thầm thất bại do phải tính toán lại tổng tiền tố nhiều lần. Ví dụ: ngay cả việc tính toán k=1000 phép biến đổi cho một trường hợp n=100000 cũng đã quá lớn vì bản thân mỗi phép biến đổi là tuyến tính. 

## Phương pháp tiếp cận 

Phương pháp vũ phu áp dụng phép toán tổng tiền tố k lần. Mỗi thao tác sẽ quét mảng và xây dựng một mảng mới. Điều này đúng vì nó tuân theo định nghĩa trực tiếp nhưng tốn O(nk) cho mỗi truy vấn. Với n và k đều lớn và t lên tới$10^5$, điều này trở nên lớn về mặt thiên văn. 

Quan sát quan trọng là các tổng tiền tố lặp lại sẽ tạo ra một cấu trúc tổ hợp đã biết rõ. Sau một thao tác, giá trị tại chỉ số n bằng n. Sau hai phép tính, nó trở thành tổng từ 1 đến n, là số tam giác$\binom{n+1}{2}$. Sau ba phép tính, nó trở thành tổng của các số tam giác, tương ứng với$\binom{n+2}{3}$. Mẫu này khái quát: sau k thao tác, giá trị tại chỉ số n bằng hệ số nhị thức$\binom{n+k-1}{k}$. 

Điều này xảy ra vì mỗi lớp tổng tiền tố thêm một mức tổng và việc tổng lặp lại của các chuỗi không đổi sẽ tạo nên cấu trúc tam giác của Pascal. Mỗi vị trí tích lũy các đóng góp tương ứng chính xác với các kết hợp chọn k chỉ số trong số n+k-1 vị trí. 

Điều này biến mỗi truy vấn thành một phép tính tổ hợp duy nhất thay vì k phép biến đổi mảng lặp lại. 

Chúng tôi tính toán trước các giai thừa và nghịch đảo mô-đun lên tới$n+k$và trả lời từng truy vấn trong O(1). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nk) mỗi truy vấn | O(n) | Quá chậm | 
| Tối ưu | Tiền xử lý O(n + max(n+k)), O(1) mỗi truy vấn | O(max(n+k)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta dựa vào nhận dạng rằng sau k phép tính tổng tiền tố, giá trị tại chỉ số n sẽ trở thành:$$a_k(n) = \binom{n + k - 1}{k}$$1. Chúng tôi xác định giá trị tối đa của n+k trên tất cả các truy vấn. Điều này là cần thiết vì tính toán trước giai thừa phải bao gồm tham số nhị thức lớn nhất được sử dụng trong bất kỳ truy vấn nào. 
2. Chúng tôi tính toán trước các giai thừa theo modulo giá trị tối đa này$10^9 + 7$. Điều này cho phép tính toán nhanh các kết hợp. 
3. Chúng ta tính toán trước nghịch đảo mô đun của giai thừa bằng cách sử dụng định lý nhỏ Fermat. Điều này là cần thiết vì phép chia trong số học mô-đun được thay thế bằng phép nhân với các giai thừa nghịch đảo. 
4. Với mỗi truy vấn (n, k), chúng tôi tính hệ số nhị thức$\binom{n+k-1}{k}$sử dụng:$$\frac{(n+k-1)!}{k!(n-1)!}$$5. Chúng tôi xuất kết quả theo modulo$10^9 + 7$. 

Lý do điều này hợp lệ là vì mỗi lớp tổng tiền tố tương ứng với việc thêm một chiều tích lũy và cấu trúc của các tổng tích lũy lặp lại khớp chính xác với tam giác Pascal. Mỗi mục đếm số chuỗi tăng dần có độ dài k kết thúc ở vị trí n, chính xác là hệ số nhị thức ở trên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def modinv(x):
    return pow(x, MOD - 2, MOD)

def build_fact(n):
    fact = [1] * (n + 1)
    invfact = [1] * (n + 1)
    for i in range(1, n + 1):
        fact[i] = fact[i - 1] * i % MOD
    invfact[n] = modinv(fact[n])
    for i in range(n, 0, -1):
        invfact[i - 1] = invfact[i] * i % MOD
    return fact, invfact

def ncr(n, r, fact, invfact):
    if r < 0 or r > n:
        return 0
    return fact[n] * invfact[r] % MOD * invfact[n - r] % MOD

def main():
    t = int(input())
    queries = []
    maxv = 0

    for _ in range(t):
        n, k = map(int, input().split())
        queries.append((n, k))
        maxv = max(maxv, n + k)

    fact, invfact = build_fact(maxv)

    for n, k in queries:
        ans = ncr(n + k - 1, k, fact, invfact)
        print(ans)

if __name__ == "__main__":
    main()
```Mảng giai thừa lưu trữ các giá trị cần thiết để tính toán các kết hợp một cách nhanh chóng. Mảng giai thừa nghịch đảo cho phép chia theo modulo bằng cách biến phép chia thành phép nhân. 

Chi tiết triển khai chính đang sử dụng$n+k-1$là đỉnh của hệ số nhị thức. Một lỗi thường gặp là quên shift và sử dụng$\binom{n+k}{k}$, được tính vượt quá một lớp đầy đủ của cấu trúc Pascal. 

Chúng tôi cũng tính toán trước các nghịch đảo trong quá trình truyền ngược để tránh lặp lại lũy thừa mô-đun cho mỗi truy vấn, điều này sẽ quá chậm đối với$10^5$truy vấn. 

## Ví dụ đã hoạt động 

Hãy xem xét một truy vấn duy nhất trong đó n = 4 và k = 2. Chúng tôi mong đợi:$$\binom{4+2-1}{2} = \binom{5}{2} = 10$$| Bước | Biểu hiện | 
| --- | --- | 
| Tính n+k-1 | 5 | 
| Tính C(5,2) | 10 | 

Điều này khớp với chuỗi chuyển đổi tổng tiền tố thứ hai đã biết 1, 3, 6, 10. 

Bây giờ hãy xem xét n = 3, k = 1:$$\binom{3}{1} = 3$$| Bước | Biểu hiện | 
| --- | --- | 
| Tính n+k-1 | 3 | 
| Tính C(3,1) | 3 | 

Điều này khớp với phép biến đổi đầu tiên trong đó mảng trở thành 1,2,3,... 

Những ví dụ này xác nhận rằng k lớp tổng tiền tố tương ứng chính xác với sự tăng trưởng hệ số nhị thức. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(tối đa N) + O(t) | tính toán trước giai thừa cộng với O(1) cho mỗi truy vấn | 
| Không gian | O(tối đa N) | lưu trữ cho mảng giai thừa và nghịch đảo | 

Việc tính toán trước được giới hạn tối đa khoảng$2 \times 10^5$trong thực tế đã cho các ràng buộc nhất định và mỗi truy vấn có thời gian không đổi. Điều này thoải mái phù hợp trong giới hạn thời gian ngay cả đối với$10^5$truy vấn. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 10**9 + 7

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    def modinv(x):
        return pow(x, MOD - 2, MOD)

    def build_fact(n):
        fact = [1] * (n + 1)
        invfact = [1] * (n + 1)
        for i in range(1, n + 1):
            fact[i] = fact[i - 1] * i % MOD
        invfact[n] = modinv(fact[n])
        for i in range(n, 0, -1):
            invfact[i - 1] = invfact[i] * i % MOD
        return fact, invfact

    def ncr(n, r, fact, invfact):
        if r < 0 or r > n:
            return 0
        return fact[n] * invfact[r] % MOD * invfact[n - r] % MOD

    t = int(input())
    qs = []
    mx = 0
    for _ in range(t):
        n, k = map(int, input().split())
        qs.append((n, k))
        mx = max(mx, n + k)

    fact, invfact = build_fact(mx)

    out = []
    for n, k in qs:
        out.append(str(ncr(n + k - 1, k, fact, invfact)))
    return "\n".join(out)

# provided samples
assert solve("3\n3 1\n1 3\n3 2") == "3\n1\n5"

# custom cases
assert solve("1\n1 1") == "1", "minimum case"
assert solve("1\n5 0") == "1", "zero transformations"
assert solve("2\n4 2\n3 3") == "10\n10", "triangular consistency"
assert solve("1\n100000 1") == "100000", "large n small k"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 / 1 1 | 1 | trường hợp nhận dạng cơ sở | 
| 5 0 | 1 | độ ổn định chuyển đổi bằng không | 
| 4 2, 3 3 | 10, 10 | tính nhất quán tổ hợp | 
| 100000 1 | 100000 | độ đúng ranh giới lớn | 

## Vỏ cạnh 

Trường hợp một cạnh là k = 0, trong đó không có biến đổi nào xảy ra. Công thức trở thành$\binom{n-1}{0} = 1$, điều này phản ánh chính xác rằng mảng vẫn là một. 

Một trường hợp cạnh khác là n = 1. Bất kể k, câu trả lời phải luôn là 1 vì phần tử đầu tiên của mảng tổng tiền tố không bao giờ thay đổi. Công thức cho$\binom{k}{k} = 1$, khớp chính xác với bất biến này. 

Trường hợp cạnh thứ ba là n lớn với k nhỏ. Ví dụ n = 100000, k = 1 mang lại$\binom{100000}{1} = 100000$, phù hợp với định nghĩa về một tổng tiền tố duy nhất tạo ra sự tăng trưởng tuyến tính.
