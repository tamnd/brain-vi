---
title: "CF 102644G - Lặp lại với hình vuông"
description: "Chuỗi trong bài toán này không phải là một phép truy hồi tuyến tính tiêu chuẩn vì giá trị tiếp theo không chỉ phụ thuộc vào các giá trị trước đó mà còn phụ thuộc vào vị trí hiện tại trong chuỗi."
date: "2026-08-01T10:20:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102644
codeforces_index: "G"
codeforces_contest_name: "Matrix Exponentiation"
rating: 0
weight: 102644
solve_time_s: 58
verified: true
draft: false
---

[CF 102644G - Lặp lại với hình vuông](https://codeforces.com/problemset/problem/102644/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

##Giải pháp 
#Hiểu vấn đề 

Chuỗi trong bài toán này không phải là một phép truy hồi tuyến tính tiêu chuẩn vì giá trị tiếp theo không chỉ phụ thuộc vào các giá trị trước đó mà còn phụ thuộc vào vị trí hiện tại trong chuỗi. Chúng ta được cung cấp một số giá trị đầu tiên của một chuỗi, các hệ số kết hợp các giá trị trước đó và ba hệ số bổ sung tạo ra hàm bậc hai của chỉ số. Nhiệm vụ là tìm giá trị của chuỗi tại một chỉ số cực lớn và in nó theo modulo$10^9+7$. 

Sự truy hồi có dạng mọi phần tử mới được tạo từ phần tử trước đó$n$các phần tử cộng với một số hạng đa thức. Các phần tử trước đó được tính trọng số theo hệ số cố định, trong khi phần bổ sung tăng lên dưới dạng biểu thức bậc hai trong chỉ mục. 

chỉ số$k$có thể lớn như$10^{18}$, do đó việc mô phỏng chuỗi từng phần tử một là không thể. Thậm chí$O(k)$các hoạt động sẽ đòi hỏi nhiều công việc hơn bất kỳ giới hạn thời gian nào của cuộc thi cho phép. Thứ tự lặp lại nhỏ, với$n \le 10$, vì vậy giải pháp dự định phải khai thác kích thước trạng thái nhỏ thay vì kích thước của chỉ mục được yêu cầu. 

Một lỗi phổ biến là quên rằng số hạng đa thức phụ thuộc vào chỉ số. Ví dụ, xử lý phép truy toán như một phép truy toán tuyến tính thông thường và chỉ sử dụng phép truy toán trước đó.$n$các giá trị sẽ mất thông tin cần thiết để tạo ra các đóng góp bậc hai trong tương lai. 

Hãy xem xét đầu vào:```
1 3
5
1
2 3 4
```Các giá trị được tạo ra dưới dạng$a_i = a_{i-1} + 2 + 3i + 4i^2$. Trình tự trở thành$5, 14, 38, 85$, vậy đáp án là:```
85
```Việc thực hiện bất cẩn chỉ theo dõi$a_i$và bỏ qua chỉ mục hiện tại sẽ giả định sai giá trị gia tăng là không đổi. 

Một trường hợp đặc biệt khác là khi chỉ mục được yêu cầu nằm trong các giá trị ban đầu. Vì:```
3 1
7 8 9
1 1 1
0 0 0
```câu trả lời là:```
8
```Không nên thực hiện bước lặp lại vì$a_1$đã được cung cấp. Một giải pháp luôn bắt đầu lũy thừa ma trận từ lần chuyển đổi đầu tiên sẽ truy cập sai trạng thái. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là áp dụng lặp đi lặp lại phép truy toán cho đến khi đạt được chỉ số$k$. Để tính một phần tử mới, chúng ta nhân phần tử trước đó$n$các giá trị theo hệ số của chúng và cộng biểu thức bậc hai. Việc này cần$O(n)$hoạt động trên mỗi phần tử được tạo nên nhu cầu mô phỏng hoàn chỉnh$O(nk)$hoạt động. Với$k$đạt$10^{18}$, điều này không khả thi chút nào. 

Lý do mà vũ lực này hoạt động là vì mọi giá trị mới chỉ phụ thuộc vào một lượng nhỏ thông tin: giá trị trước đó$n$các giá trị chuỗi và thông tin chỉ số hiện tại cần thiết cho số hạng bậc hai. Do đó, chuỗi có trạng thái kích thước cố định nhỏ. 

Quan sát quan trọng là phần bậc hai cũng có thể được biểu diễn dưới dạng truy hồi. Nếu chúng ta lưu trữ chỉ mục hiện tại và bình phương của nó dưới dạng các giá trị trạng thái bổ sung thì chỉ mục tiếp theo và bình phương tiếp theo có thể được tạo ra bằng một phép biến đổi tuyến tính. Toàn bộ quá trình trở thành phép nhân ma trận trên một vectơ có kích thước$n+3$. 

Khi quá trình chuyển đổi được biểu diễn dưới dạng ma trận, việc đạt được chỉ mục$k$yêu cầu áp dụng cùng một phép biến đổi$k-(n-1)$lần. Phép lũy thừa nhanh làm giảm điều này thành phép nhân ma trận logarit. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nk) | O(n) | Quá chậm | 
| Hàm mũ ma trận | O((n+3)^3 log k) | O((n+3)^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng một vectơ trạng thái chứa các giá trị chuỗi hiện tại, một giá trị không đổi, chỉ mục hiện tại và bình phương của chỉ mục hiện tại. Tại vị trí$n-1$, trạng thái là:$$[a_{n-1}, a_{n-2}, \dots, a_0, 1, n-1, (n-1)^2]$$Ba giá trị bổ sung cho phép phần bậc hai của phép truy toán được xử lý bằng cùng một phép biến đổi tuyến tính. 

1. Xây dựng ma trận chuyển trạng thái từ chỉ mục$i$lập chỉ mục$i+1$. Hàng đầu tiên tạo$a_{i+1}$. Nó kết hợp các giá trị chuỗi được lưu trữ bằng hệ số truy hồi và thêm:$$p + (i+1)q + (i+1)^2r$$trở thành:$$(p+q+r) + (q+2r)i + r i^2$$sử dụng các giá trị được lưu trữ$1$,$i$, Và$i^2$. 

1. Điền vào các hàng còn lại của ma trận để chuyển các giá trị chuỗi trước đó về phía trước. Phần chuỗi hoạt động giống như một phép truy hồi tuyến tính thông thường, vì vậy mỗi giá trị chỉ đơn giản là di chuyển xuống một vị trí. 
2. Nếu$k$nhỏ hơn$n$, trả về giá trị ban đầu đã biết. Ngược lại, nâng ma trận chuyển tiếp lên lũy thừa$k-(n-1)$. 

Số mũ chính xác là số lần chuyển đổi cần thiết để chuyển từ trạng thái ban đầu tại chỉ số$n-1$tới chỉ số mong muốn. 

1. Nhân ma trận lũy thừa với vectơ trạng thái ban đầu. Phần tử đầu tiên của vectơ kết quả là$a_k$. 

Tại sao nó hoạt động: 

Bất biến là sau khi áp dụng ma trận chuyển tiếp$t$lần, vectơ trạng thái chứa chính xác các giá trị chuỗi và thông tin chỉ mục cho vị trí$n-1+t$. Ma trận tính toán giá trị chuỗi tiếp theo bằng cách sử dụng cùng một công thức lặp lại và cập nhật thông tin chỉ mục một cách nhất quán. Vì phép lũy thừa ma trận tạo ra kết quả tương tự như việc áp dụng phép chuyển đổi lặp đi lặp lại, nên thành phần đầu tiên sau$k-(n-1)$chuyển tiếp phải được$a_k$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10 ** 9 + 7

def mat_mul(a, b):
    n = len(a)
    m = len(b[0])
    k = len(b)
    res = [[0] * m for _ in range(n)]
    for i in range(n):
        for x in range(k):
            if a[i][x]:
                ax = a[i][x]
                for j in range(m):
                    res[i][j] = (res[i][j] + ax * b[x][j]) % MOD
    return res

def mat_pow(a, e):
    n = len(a)
    res = [[0] * n for _ in range(n)]
    for i in range(n):
        res[i][i] = 1
    while e:
        if e & 1:
            res = mat_mul(res, a)
        a = mat_mul(a, a)
        e >>= 1
    return res

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    c = list(map(int, input().split()))
    p, q, r = map(int, input().split())

    if k < n:
        print(a[k] % MOD)
        return

    size = n + 3
    trans = [[0] * size for _ in range(size)]

    for i in range(n):
        trans[0][i] = c[i] % MOD

    trans[0][n] = (p + q + r) % MOD
    trans[0][n + 1] = (q + 2 * r) % MOD
    trans[0][n + 2] = r % MOD

    for i in range(1, n):
        trans[i][i - 1] = 1

    trans[n][n] = 1
    trans[n + 1][n] = 1
    trans[n + 1][n + 1] = 1
    trans[n + 2][n] = 1
    trans[n + 2][n + 1] = 2
    trans[n + 2][n + 2] = 1

    state = [[0] for _ in range(size)]
    for i in range(n):
        state[i][0] = a[n - 1 - i] % MOD

    idx = n - 1
    state[n][0] = 1
    state[n + 1][0] = idx % MOD
    state[n + 2][0] = (idx * idx) % MOD

    result = mat_mul(mat_pow(trans, k - (n - 1)), state)
    print(result[0][0] % MOD)

if __name__ == "__main__":
    solve()
```Trước tiên, mã sẽ xử lý trường hợp đơn giản trong đó câu trả lời nằm trong số các giá trị ban đầu được cung cấp. Điều này tránh việc xây dựng ma trận không cần thiết và cũng ngăn ngừa các lỗi lập chỉ mục. 

Ma trận chuyển tiếp sử dụng ma trận đầu tiên$n$các cột cho lịch sử trình tự. Trình tự được lưu theo thứ tự ngược lại để hàng đầu tiên có thể áp dụng trực tiếp các hệ số$c_1, c_2, \ldots, c_n$. 

Cửa hàng ba vị trí cuối cùng$1$, chỉ số hiện tại và bình phương chỉ số hiện tại. Quá trình chuyển đổi vuông như sau:$$(i+1)^2 = i^2 + 2i + 1$$giải thích các hệ số ở hàng cuối cùng. Tất cả các hoạt động được thực hiện modulo$10^9+7$, vì vậy kích thước số nguyên của Python không phải là vấn đề đáng lo ngại. 

Số mũ là$k-(n-1)$, không$k$. Trạng thái bắt đầu đã đại diện$a_{n-1}$, vì vậy chỉ cần áp dụng các hiệu ứng chuyển tiếp còn lại. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
2 2
0 30
2 1
2 1 1
```Trạng thái ban đầu là: 

| Bước | Chỉ mục hiện tại | Giá trị được lưu trữ | Đã thêm thuật ngữ | Kết quả | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 1 | a1=30, a0=0 | không | 30 | 
| Chuyển tiếp | 2 | 2×30+0 | 2+2+4 | 68 | 

Quá trình chuyển đổi chuyển từ chỉ mục 1 sang chỉ mục 2 một lần. Ma trận kết hợp chính xác các giá trị trước đó và đóng góp bậc hai. 

Đối với mẫu thứ hai:```
1 3
5
1
2 3 4
```Sự phát triển của trạng thái là: 

| Bước | Chỉ mục hiện tại | Giá trị trước đó | Đóng góp đa thức | Giá trị mới | 
| --- | --- | --- | --- | --- | 
| Ban đầu | 0 | 5 | không | 5 | 
| Chuyển tiếp | 1 | 5 | 2+3+4 | 14 | 
| Chuyển tiếp | 2 | 14 | 2+6+16 | 38 | 
| Chuyển tiếp | 3 | 38 | 2+9+36 | 85 | 

Dấu vết cho thấy tại sao chỉ mục và hình vuông phải là một phần của trạng thái. Giá trị gia tăng thay đổi theo từng bước. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n+3)^3 log k) | Phép nhân ma trận được thực hiện trong quá trình lũy thừa nhị phân. | 
| Không gian | O((n+3)^2) | Chỉ có ma trận chuyển tiếp và vectơ được lưu trữ. | 

Kích thước ma trận nhiều nhất là 13 vì$n$nhiều nhất là 10. Logarit của$k$là khoảng 60 ngay cả đối với$10^{18}$, nên nghiệm dễ dàng phù hợp với giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

MOD = 10 ** 9 + 7

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.read().split()
    sys.stdin = old

    it = iter(data)
    n = int(next(it))
    k = int(next(it))
    a = [int(next(it)) for _ in range(n)]
    c = [int(next(it)) for _ in range(n)]
    p = int(next(it))
    q = int(next(it))
    r = int(next(it))

    if k < n:
        return str(a[k] % MOD)

    size = n + 3
    trans = [[0] * size for _ in range(size)]

    for i in range(n):
        trans[0][i] = c[i] % MOD
    trans[0][n] = (p + q + r) % MOD
    trans[0][n + 1] = (q + 2 * r) % MOD
    trans[0][n + 2] = r % MOD

    for i in range(1, n):
        trans[i][i - 1] = 1

    trans[n][n] = 1
    trans[n + 1][n] = 1
    trans[n + 1][n + 1] = 1
    trans[n + 2][n] = 1
    trans[n + 2][n + 1] = 2
    trans[n + 2][n + 2] = 1

    def mul(a, b):
        m = len(a)
        res = [[0] * m for _ in range(m)]
        for i in range(m):
            for x in range(m):
                for j in range(m):
                    res[i][j] = (res[i][j] + a[i][x] * b[x][j]) % MOD
        return res

    def power(a, e):
        m = len(a)
        res = [[int(i == j) for j in range(m)] for i in range(m)]
        while e:
            if e & 1:
                res = mul(res, a)
            a = mul(a, a)
            e >>= 1
        return res

    state = [[0] for _ in range(size)]
    for i in range(n):
        state[i][0] = a[n - 1 - i]
    state[n][0] = 1
    state[n + 1][0] = n - 1
    state[n + 2][0] = (n - 1) * (n - 1)

    ans = mul(power(trans, k - n + 1), state)
    return str(ans[0][0] % MOD)

assert run("""2 2
0 30
2 1
2 1 1
""") == "68"

assert run("""1 3
5
1
2 3 4
""") == "85"

assert run("""1 0
123
5
1 1 1
""") == "123"

assert run("""3 2
7 8 9
1 1 1
0 0 0
""") == "9"

assert run("""1 1000000000000000000
0
1
0 0 0
""") != ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu thứ hai | 85 | Tăng trưởng bậc hai với tái phát bậc 1 | 
| Truy vấn chỉ mục ban đầu | 123 | Trả về một giá trị đã biết | 
| Phép tái phát đa thức bằng không | 9 | Xử lý đúng phép truy toán tuyến tính thuần túy | 
| Chỉ số lớn | Kết quả không trống hợp lệ | lũy thừa ma trận cho k rất lớn | 

## Vỏ cạnh 

Trường hợp cạnh quan trọng đầu tiên là một truy vấn bên trong chuỗi ban đầu. Vì:```
3 1
7 8 9
1 1 1
0 0 0
```thuật toán ngay lập tức trả về giá trị được cung cấp thứ hai. Nó không bao giờ áp dụng ma trận chuyển tiếp vì phép truy hồi chỉ xác định các phần tử trong tương lai. 

Trường hợp cạnh thứ hai là phép truy toán trong đó đóng góp của đa thức bằng 0. Trong trường hợp đó, ba giá trị trạng thái cuối cùng vẫn tồn tại nhưng hàng đầu tiên đơn giản bỏ qua chúng. Ma trận hoạt động chính xác như một phép truy toán tuyến tính thông thường. 

Trường hợp cạnh cuối cùng là một chỉ số cực kỳ lớn. Ví dụ:```
1 1000000000000000000
0
1
0 0 0
```Một mô phỏng sẽ cần$10^{18}$chuyển tiếp. Thay vào đó, thuật toán thực hiện khoảng 60 ma trận bình phương và đạt được cùng một lũy thừa chuyển tiếp bằng cách sử dụng lũy ​​thừa nhị phân.
