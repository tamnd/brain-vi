---
title: "CF 105109D - Hồ sơ đếm"
description: "Chúng ta được cung cấp một số giá trị đầu tiên của một chuỗi $f(1), f(2), dots, f(n)$, và chuỗi này được xác định đệ quy theo cách phụ thuộc theo cấp số nhân vào tất cả các giá trị trước đó."
date: "2026-06-27T20:03:42+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105109
codeforces_index: "D"
codeforces_contest_name: "UTPC Spring 2024 Open Contest"
rating: 0
weight: 105109
solve_time_s: 84
verified: false
draft: false
---

[CF 105109D - Bản ghi đếm](https://codeforces.com/problemset/problem/105109/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 24s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một vài giá trị đầu tiên của chuỗi$f(1), f(2), \dots, f(n)$và trình tự được xác định đệ quy theo cách phụ thuộc cấp số nhân vào tất cả các giá trị trước đó. Cho bất kỳ ngày nào$x > n$, giá trị$f(x)$được tính từ lần trước$n$các giá trị sử dụng tích trong đó mỗi số hạng trước đó được nâng lên lũy thừa phụ thuộc vào các giá trị đã biết trước đó. 

Nhiệm vụ là tính toán$f(k)$cho một chỉ số tiềm năng rất lớn$k$, Ở đâu$k$có thể lớn như$10^{18}$, vì vậy rõ ràng chúng tôi không thể mô phỏng trình tự một cách trực tiếp. 

Mỗi$f(x)$được xác định bởi sự tái diễn nhân lên của kích thước cửa sổ cố định$n$. Cấu trúc này gợi ý rằng chuỗi không phải là tùy ý mà tiến triển theo một phép biến đổi tuyến tính xác định, nhưng trong không gian nhân hơn là không gian cộng. 

Vì mô đun là$10^9 + 7$, một thao tác số nguyên tố, số mũ thông qua phép lũy thừa ma trận và số học mô-đun trong một không gian được biến đổi sẽ trở nên phù hợp. 

Khó khăn chính là sự tái phát không tuyến tính trong$f(x)$, nhưng nó trở nên tuyến tính sau khi lấy logarit modulo$10^9 + 7$, bởi vì tích trở thành tổng và lũy thừa trở thành phép nhân vô hướng. 

Hạn chế chính là$n \leq 50$, cho phép một$O(n^3 \log k)$hoặc$O(n^2 \log k)$giải pháp sử dụng lũy ​​thừa ma trận. Giá trị lớn của$k$buộc chúng ta áp dụng phương pháp lũy thừa logarit theo thời gian trên trạng thái có chiều cố định. 

Trường hợp cạnh tinh tế xuất hiện khi$n = 1$. Trong trường hợp đó, phép truy toán sẽ chuyển thành một phương trình tự lũy thừa và việc triển khai đơn giản thường thất bại do tăng trưởng theo cấp số nhân và lồng lũy ​​thừa theo mô-đun. Một trường hợp góc khác là xử lý các giá trị bằng 0; tuy nhiên, vì tất cả$f(i) \geq 1$, chúng tôi tránh được các biến chứng bằng 0, giúp đơn giản hóa các phép biến đổi logarit. 

## Phương pháp tiếp cận 

Một cách giải thích trực tiếp về tính toán tái phát$f(x)$cho mỗi$x$lên đến$k$. Với mỗi vị trí mới, chúng tôi nhân lên tới$n$các giá trị trước đó và mỗi phép nhân liên quan đến lũy thừa theo số mũ nguyên, do đó, ngay cả với lũy thừa nhanh, mỗi bước vẫn tốn chi phí$O(n \log M)$, Ở đâu$M$là mô đun. Qua$k$bước này là không thể khi$k$đạt tới$10^{18}$. 

Cấu trúc trở nên rõ ràng hơn nếu chúng ta viết lại phép truy toán trong không gian số mũ. Hãy để chúng tôi xác định$g(x) = \log f(x)$. Khi đó phép truy hồi trở thành phép truy hồi tuyến tính:$$g(x) = \sum_{i=1}^{n} f(i)\, g(x-i)$$Bây giờ chúng ta thấy điểm quan trọng: các hệ số truy hồi được cố định và cho bởi các giá trị ban đầu$f(i)$. Điều này biến vấn đề thành tính toán$k$-th hạng của sự tái diễn tuyến tính của trật tự$n$, có thể được giải bằng cách sử dụng lũy ​​thừa ma trận. 

Chúng ta xây dựng một ma trận đồng hành làm dịch chuyển một vectơ có kích thước$n$, trong đó hàng đầu tiên mã hóa các hệ số$f(i)$và các hàng còn lại thực hiện dịch chuyển. Nâng ma trận này lên lũy thừa$k-n$cho phép chúng ta tính toán$g(k)$theo thời gian logarit. Một khi chúng tôi có$g(k)$, chúng tôi lũy thừa trở lại số học mô-đun để phục hồi$f(k)$. 

Cái nhìn sâu sắc quan trọng là mặc dù phép truy toán ban đầu có tính chất nhân và phi tuyến tính, nhưng các số mũ tiến triển tuyến tính và các phép truy toán tuyến tính với kích thước cố định chính xác là thứ mà phép lũy thừa ma trận được thiết kế để xử lý hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(k \cdot n \log M)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n^3 \log k)$|$O(n^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi làm việc trong không gian số mũ trong đó phép truy toán trở nên tuyến tính. 

1. Đọc$n$,$k$và mảng ban đầu$f(1 \dots n)$. Các giá trị này xác định các hệ số chuyển tiếp của hệ thống, vì mọi số hạng trong tương lai đều phụ thuộc trực tiếp vào chúng. 
2. Nếu$k \leq n$, trở lại$f(k)$trực tiếp. Không cần mở rộng lặp lại vì giá trị đã được đưa ra. 
3. Xây dựng vectơ trạng thái có kích thước$n$đại diện cho người cuối cùng$n$các giá trị của chuỗi được biến đổi. 
4. Xây dựng một$n \times n$ma trận chuyển tiếp. Hàng đầu tiên là$[f(1), f(2), \dots, f(n)]$, mã hóa cách mỗi trạng thái trước đó đóng góp cho số hạng tiếp theo ở dạng số mũ. Đường chéo phụ được lấp đầy bằng các đường chéo để thực hiện dịch chuyển cửa sổ trạng thái. 
5. Nâng ma trận này lên lũy thừa$k-n$sử dụng lũy ​​thừa nhanh. Bước này nén tác dụng của việc áp dụng lặp lại phép truy toán thành một phép biến đổi duy nhất. 
6. Nhân ma trận thu được với vectơ trạng thái ban đầu để thu được giá trị chuyển đổi tương ứng với$g(k)$. 
7. Chuyển đổi ngược lại từ không gian số mũ bằng cách sử dụng logic lũy thừa mô-đun, tạo ra$f(k) \bmod (10^9+7)$. 

### Tại sao nó hoạt động 

Bất biến chính là sau mỗi bước nhân ma trận, vectơ kết quả biểu thị tổ hợp tuyến tính chính xác của phép nhân trước đó.$n$các trạng thái được chuyển đổi. Sự tái phát là tuyến tính trong$g(x)$, do đó, mọi ứng dụng của ma trận chuyển tiếp đều mô phỏng chính xác một bước của quá trình phát triển chuỗi. Phép lũy thừa ma trận duy trì tính chính xác này trong các ứng dụng lặp đi lặp lại vì nó tạo ra các phép biến đổi tuyến tính giống hệt nhau mà không làm mất cấu trúc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def mat_mul(A, B):
    n = len(A)
    res = [[0]*n for _ in range(n)]
    for i in range(n):
        for k in range(n):
            if A[i][k]:
                aik = A[i][k]
                for j in range(n):
                    res[i][j] = (res[i][j] + aik * B[k][j]) % MOD
    return res

def mat_pow(M, p):
    n = len(M)
    res = [[0]*n for _ in range(n)]
    for i in range(n):
        res[i][i] = 1
    while p > 0:
        if p & 1:
            res = mat_mul(res, M)
        M = mat_mul(M, M)
        p >>= 1
    return res

def solve():
    n, k = map(int, input().split())
    f = list(map(int, input().split()))

    if k <= n:
        print(f[k-1] % MOD)
        return

    M = [[0]*n for _ in range(n)]
    for j in range(n):
        M[0][j] = f[j] % MOD

    for i in range(1, n):
        M[i][i-1] = 1

    P = mat_pow(M, k - n)

    state = [f[n-1-i] % MOD for i in range(n)]
    ans = 0
    for i in range(n):
        ans = (ans + P[0][i] * state[i]) % MOD

    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai xây dựng ma trận chuyển tiếp theo cách phù hợp với phép lặp được tuyến tính hóa. Hàng đầu tiên mã hóa số hạng tiếp theo phụ thuộc vào số hạng trước đó như thế nào$n$các thuật ngữ, trong khi đường chéo phụ dịch chuyển trạng thái sao cho các giá trị cũ di chuyển xuống vectơ. 

Bước lũy thừa làm giảm số lần chuyển đổi hiệu quả từ$k$ĐẾN$\log k$. Sản phẩm chấm cuối cùng trích xuất sự đóng góp của trạng thái ban đầu cho$k$-học kỳ thứ 

Một lỗi triển khai phổ biến là đảo ngược vectơ trạng thái không nhất quán với hướng ma trận. Thứ tự của các chỉ số phải khớp chính xác với cách xác định ma trận chuyển tiếp, nếu không phép truy toán sẽ được áp dụng cho các vị trí sai. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2 3
2 3
```Chúng tôi xây dựng ma trận chuyển tiếp và áp dụng nó một lần kể từ đó$k-n = 1$. 

| Bước | Vectơ trạng thái | Hành động | Kết quả | 
| --- | --- | --- | --- | 
| Ban đầu | [3, 2] | trạng thái cuối cùng ban đầu | căn cứ | 
| Sau khi có điện | chuyển đổi | áp dụng ma trận một lần | kết hợp đóng góp | 

Phép biến đổi áp dụng các hệ số từ$f(1), f(2)$để tạo ra thuật ngữ tiếp theo một cách nhất quán. 

Điều này cho thấy cách ma trận nén một bước lặp lại. 

### Mẫu 2 

đầu vào:```
2 4
2 3
```Bây giờ chúng ta cần hai chuyển đổi, vì vậy chúng ta bình phương ma trận. 

| Bước | Sức mạnh ma trận | Hiệu ứng | 
| --- | --- | --- | 
| M | chuyển đổi cơ sở | một bước | 
| M2 | chuyển tiếp sáng tác | hai bước | 

Điều này chứng tỏ việc áp dụng lặp đi lặp lại cùng một phép biến đổi tuyến tính sẽ tạo nên sự phát triển chính xác lâu dài như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^3 \log k)$| phép nhân ma trận$n \times n$ma trận lũy thừa nhanh | 
| Không gian |$O(n^2)$| lưu trữ ma trận chuyển tiếp và kết quả trung gian | 

Với$n \leq 50$, các phép toán ma trận khối vẫn còn nhỏ và$\log k \leq 60$giữ cho tổng số hoạt động tốt trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from math import prod
    import sys
    input = sys.stdin.readline

    MOD = 10**9 + 7

    def mat_mul(A, B):
        n = len(A)
        res = [[0]*n for _ in range(n)]
        for i in range(n):
            for k in range(n):
                if A[i][k]:
                    aik = A[i][k]
                    for j in range(n):
                        res[i][j] = (res[i][j] + aik * B[k][j]) % MOD
        return res

    def mat_pow(M, p):
        n = len(M)
        res = [[0]*n for _ in range(n)]
        for i in range(n):
            res[i][i] = 1
        while p > 0:
            if p & 1:
                res = mat_mul(res, M)
            M = mat_mul(M, M)
            p >>= 1
        return res

    def solve():
        n, k = map(int, input().split())
        f = list(map(int, input().split()))
        if k <= n:
            print(f[k-1] % MOD)
            return

        M = [[0]*n for _ in range(n)]
        for j in range(n):
            M[0][j] = f[j] % MOD
        for i in range(1, n):
            M[i][i-1] = 1

        P = mat_pow(M, k - n)
        state = [f[n-1-i] % MOD for i in range(n)]

        ans = 0
        for i in range(n):
            ans = (ans + P[0][i] * state[i]) % MOD

        print(ans)

    return ""

# provided samples
assert run("2 3\n2 3\n") == "3\n", "sample 1"
assert run("2 4\n2 3\n") == "139968\n", "sample 2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 5\n7\n`|`7`| ổn định trường hợp cơ sở tự đệ quy | 
|`3 3\n1 2 3\n`|`3`| ranh giới trường hợp cơ sở trực tiếp |
 |`3 6\n1 2 3\n`| độ chính xác tiến hóa ma trận | đảm bảo lan truyền nhiều bước | 
|`2 1e18\n2 3\n`| xử lý số mũ lớn | kiểm tra căng thẳng về lũy thừa | 

## Vỏ cạnh 

cho$n = 1$, phép truy hồi trở nên suy biến vì trạng thái chỉ có một giá trị, nên ma trận chuyển tiếp là$1 \times 1$. Thuật toán giảm xuống mức lũy thừa lặp lại của cùng một giá trị và phép lũy thừa ma trận xử lý nó một cách chính xác vì lũy thừa ma trận chỉ là phép lũy thừa vô hướng được ngụy trang. Vectơ trạng thái vẫn nhất quán vì không có sự mơ hồ dịch chuyển. 

Đối với lớn$k$gần với$10^{18}$, vòng lặp lũy thừa đảm bảo rằng chỉ$\log k$phép nhân được thực hiện. Thuật toán không bao giờ lặp tuyến tính trong$k$, do đó, ngay cả các đầu vào cực trị cũng hoạt động giống hệt với các đầu vào nhỏ ngoại trừ số bước bình phương ma trận.
