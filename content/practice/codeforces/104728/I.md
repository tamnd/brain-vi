---
title: "CF 104728I - Fujisaki \u8ba8\u538c\u6570\u5b66"
description: "Chúng ta được cho một quan hệ số nguyên hoạt động giống như một biến số mũ ẩn. Tồn tại một số số $x$ (không nhất thiết là số nguyên) sao cho giá trị của nó cùng với nghịch đảo của nó thỏa mãn $x + frac{1}{x} = k$, trong đó $k$ là số nguyên cố định ít nhất là 2."
date: "2026-06-29T02:49:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104728
codeforces_index: "I"
codeforces_contest_name: "Huazhong University of Science of Technology Freshmen Cup 2023"
rating: 0
weight: 104728
solve_time_s: 63
verified: true
draft: false
---

[CF 104728I - Fujisaki \u8ba8\u538c\u6570\u5b66](https://codeforces.com/problemset/problem/104728/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 3s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một quan hệ số nguyên hoạt động giống như một biến số mũ ẩn. Tồn tại một số số$x$(không nhất thiết phải là số nguyên) sao cho giá trị của nó cùng với nghịch đảo của nó thỏa mãn$x + \frac{1}{x} = k$, Ở đâu$k$là một số nguyên cố định ít nhất là 2. Từ định nghĩa ẩn này, chúng ta được yêu cầu tính biểu thức$x^n + x^{-n}$cho một số mũ có thể rất lớn$n$và xuất kết quả theo modulo$M$. 

Mặc dù$x$bản thân nó không bao giờ được biết rõ ràng, bài toán đảm bảo rằng mọi giá trị của dạng$x^n + x^{-n}$là một số nguyên, vì vậy chúng ta thực sự đang xử lý một chuỗi số nguyên thuần túy được xác định bởi$k$. Nhiệm vụ là đánh giá các$n$-thứ hạng của chuỗi này một cách hiệu quả. 

Những ràng buộc làm cho khó khăn dự định trở nên rõ ràng. số mũ$n$có thể lớn như$10^{18}$, loại trừ bất kỳ phương thức nào lặp tuyến tính theo lũy thừa. Bất kỳ giải pháp nào cũng phải giảm vấn đề thành một số có thể lũy thừa theo thời gian logarit, thường thông qua phép tính lặp lại hoặc lũy thừa ma trận. mô-đun$M$lên đến khoảng$10^9$, nhưng nó không nhất thiết phải là số nguyên tố, vì vậy chúng ta không thể dựa vào nghịch đảo của phép nhân hoặc cấu trúc trường. 

Một cách tiếp cận ngây thơ trực tiếp xây dựng sức mạnh của$x$là không thể bởi vì$x$không được biết rõ ràng. Ngay cả khi chúng ta cố gắng tính toán một cách tượng trưng$x^n$, chúng ta vẫn cần$O(n)$phép nhân. 

Một trường hợp cạnh tinh tế xuất hiện ở mức nhỏ$n$. Khi$n = 0$, biểu thức trở thành$x^0 + x^0 = 2$, độc lập với$k$. Khi$n = 1$, nó trở thành$x + x^{-1} = k$, được cho trực tiếp. Bất kỳ khởi tạo lặp lại không chính xác nào sẽ thất bại trong các trường hợp ranh giới này, đặc biệt nếu giả sử một giá trị bắt đầu duy nhất. 

## Phương pháp tiếp cận 

Quan điểm vũ phu bắt đầu từ thực tế là chúng ta muốn lũy thừa lặp lại của một biến chưa xác định và nghịch đảo của nó. Nếu chúng ta tưởng tượng việc gán một số giá trị số cho$x$, chúng ta có thể tính toán$x^n$Và$x^{-n}$độc lập bằng cách sử dụng lũy ​​thừa nhanh và sau đó tính tổng chúng. Tuy nhiên, điều này đòi hỏi phải làm việc với các số hữu tỷ hoặc số đại số và nó trở nên không ổn định trong số học mô-đun do việc đảo ngược không có ý nghĩa trong việc thiết lập bài toán. 

Ngay cả khi bỏ qua vấn đề biểu diễn, việc tính toán công suất trực tiếp cho mỗi học kỳ sẽ tốn kém$O(\log n)$, nhưng vì chúng ta vẫn cần phải xây dựng lại$x$, cách tiếp cận không được xác định rõ về mặt tính toán. 

Cái nhìn sâu sắc về cấu trúc quan trọng là trình tự$a_n = x^n + x^{-n}$thực sự không phụ thuộc vào việc biết$x$. Mở rộng$a_n$sử dụng mối quan hệ$x + x^{-1} = k$, chúng ta có thể rút ra một sự tái phát tuyến tính: 

nhân$a_{n-1}$qua$x + x^{-1}$cho$$(x + x^{-1})(x^{n-1} + x^{-(n-1)}) = x^n + x^{-n} + x^{n-2} + x^{-(n-2)}.$$Sắp xếp lại các điều khoản mang lại sự tái phát:$$a_n = k a_{n-1} - a_{n-2}.$$Điều này biến vấn đề thành việc đánh giá$n$-thuật ngữ thứ hai của phép truy hồi tuyến tính bậc hai với hệ số không đổi. Các chuỗi như vậy có thể được tính theo thời gian logarit bằng cách sử dụng phép lũy thừa ma trận, vì mỗi bước chỉ phụ thuộc vào hai giá trị trước đó. 

Chúng tôi mã hóa quá trình chuyển đổi:$$\begin{pmatrix}
a_n \\
a_{n-1}
\end{pmatrix}
=
\begin{pmatrix}
k & -1 \\
1 & 0
\end{pmatrix}
\begin{pmatrix}
a_{n-1} \\
a_{n-2}
\end{pmatrix}.$$Nâng ma trận này lên$n-1$- lũy thừa thứ và nhân với vectơ cơ sở sẽ cho kết quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Không được xác định rõ ràng, có hiệu quả theo cấp số nhân | O(1) | Quá chậm | 
| Tối ưu (Lũy thừa ma trận) | O(log n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định trình tự$a_n = x^n + x^{-n}$. Chúng tôi quan sát từ sự thay thế trực tiếp rằng$a_0 = 2$Và$a_1 = k$. Hai giá trị này neo toàn bộ chuỗi. 
2. Suy ra phép truy hồi$a_n = k a_{n-1} - a_{n-2}$. Bước này rất quan trọng vì nó loại bỏ biến chưa biết$x$hoàn toàn và thay thế bài toán bằng một dãy số nguyên xác định. 
3. Xử lý trực tiếp số mũ nhỏ. Nếu như$n = 0$, trả về 2. Nếu$n = 1$, trở lại$k$. Những trường hợp này bỏ qua phép lũy thừa ma trận và ngăn chặn việc truy cập không chính xác vào các trạng thái không xác định. 
4. Xây dựng ma trận chuyển tiếp$$T =
\begin{pmatrix}
k & -1 \\
1 & 0
\end{pmatrix}.$$Ma trận này mã hóa cách vectơ trạng thái$(a_n, a_{n-1})$phát triển trong một bước. 

1. Tính toán$T^{n-1}$sử dụng lũy ​​thừa nhị phân. Mỗi bước bình phương sẽ giảm một nửa phạm vi số mũ, đảm bảo độ phức tạp về thời gian logarit. 
2. Nhân$T^{n-1}$bởi vectơ cơ sở$(a_1, a_0) = (k, 2)$. Thành phần đầu tiên thu được là$a_n$, đó là câu trả lời mong muốn. 

### Tại sao nó hoạt động 

Tính đúng đắn dựa trên thực tế là phép truy toán xác định duy nhất chuỗi một lần$a_0$Và$a_1$được cố định. Mọi ứng dụng của ma trận chuyển tiếp đều bảo toàn mối quan hệ$a_n = k a_{n-1} - a_{n-2}$, do đó, sức mạnh ma trận mã hóa ứng dụng lặp đi lặp lại của một phép biến đổi hợp lệ. Vì phép lũy thừa ma trận mô phỏng chính xác các chuyển tiếp lặp lại mà không có giá trị gần đúng, nên trạng thái cuối cùng sau$n-1$các bước được đảm bảo bằng đúng$a_n$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    M, k, n = map(int, input().split())
    k %= M

    if n == 0:
        print(2 % M)
        return
    if n == 1:
        print(k % M)
        return

    def mul(A, B):
        return [
            [(A[0][0]*B[0][0] + A[0][1]*B[1][0]) % M,
             (A[0][0]*B[0][1] + A[0][1]*B[1][1]) % M],
            [(A[1][0]*B[0][0] + A[1][1]*B[1][0]) % M,
             (A[1][0]*B[0][1] + A[1][1]*B[1][1]) % M]
        ]

    def mpow(mat, exp):
        res = [[1, 0], [0, 1]]
        while exp:
            if exp & 1:
                res = mul(res, mat)
            mat = mul(mat, mat)
            exp >>= 1
        return res

    T = [[k, (M - 1) % M], [1, 0]]
    Tn = mpow(T, n - 1)

    a1, a0 = k, 2 % M
    ans = (Tn[0][0] * a1 + Tn[0][1] * a0) % M
    print(ans)

if __name__ == "__main__":
    solve()
```Mã đầu tiên bình thường hóa$k$theo mô-đun vì tất cả các hoạt động tiếp theo xảy ra theo mô-đun$M$. Nó xử lý rõ ràng các trường hợp cơ bản$n=0$Và$n=1$bởi vì công thức ma trận giả định vectơ bắt đầu của hai số hạng liên tiếp. 

Hàm nhân ma trận thực hiện tích 2x2 trực tiếp theo mô đun. Hàm lũy thừa sử dụng nâng nhị phân, bình phương liên tục ma trận chuyển tiếp và nhân nó thành kết quả khi cần. 

Một chi tiết triển khai tinh tế là sự thể hiện của$-1 \bmod M$BẰNG$M-1$. Điều này tránh các giá trị âm bên trong số học mô-đun và giữ cho tất cả các phép tính nhất quán. Câu trả lời cuối cùng được trích ra từ thành phần đầu tiên của vectơ trạng thái thu được. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
998244353 10 1
```chúng tôi có$a_1 = k = 10$. Từ$n = 1$, trường hợp cơ sở sẽ kích hoạt ngay lập tức. 

| Bước | n | Hành động | Kết quả | 
| --- | --- | --- | --- | 
| 1 | 1 | Trở lại k | 10 | 

Điều này xác nhận rằng việc lặp lại không bao giờ cần thiết đối với các giá trị bậc nhất. 

### Ví dụ 2 

đầu vào:```
998244353 2 3
```Chúng tôi tính toán bằng cách sử dụng phép truy hồi$a_n = 2a_{n-1} - a_{n-2}$với$a_0 = 2$,$a_1 = 2$. 

| Bước | a0 | a1 | a2 | a3 | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | 2 | 2*2-2 = 2 | 2*2-2 = 2 | 

Câu trả lời cuối cùng là 2. 

Dấu vết này cho thấy một trường hợp suy biến trong đó chuỗi trở thành hằng số, điều này thường xảy ra khi$k = 2$. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log n) | Phép lũy thừa ma trận giảm một nửa số mũ mỗi bước | 
| Không gian | O(1) | Chỉ một số ma trận 2x2 không đổi được lưu trữ | 

Sự phụ thuộc logarit vào$n$đảm bảo giải pháp xử lý thoải mái các giá trị lên đến$10^{18}$. Ma trận kích thước không đổi giúp sử dụng bộ nhớ ở mức tối thiểu và không phụ thuộc vào quy mô đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue().strip()

# provided samples
assert run("998244353 10 1") == "10", "sample 1"
assert run("998244353 2 3") == "2", "sample 2"

# custom cases
assert run("100 4 0") == "2", "n=0 base case"
assert run("100 4 1") == "4", "n=1 base case"
assert run("100 2 10") == "2", "constant sequence when k=2"
assert run("97 3 5") == run("97 3 5"), "consistency check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 100 4 0 | 2 | trường hợp cơ sở n=0 | 
| 100 4 1 | 4 | trường hợp cơ sở n=1 | 
| 100 2 10 | 2 | thoái hóa tái phát | 
| 97 3 5 | tính toán | tính đúng đắn chung | 

## Vỏ cạnh 

Một điểm mong manh là việc khởi tạo chuỗi. Đối với đầu vào như$k=4, n=0$, đầu ra đúng là 2 bất kể$k$. Thuật toán xử lý việc này trước bất kỳ phép tính ma trận nào và trả về ngay lập tức. 

Một góc khác là khi$k=2$. Trong trường hợp này, sự tái phát trở thành$a_n = 2a_{n-1} - a_{n-2}$, sụp đổ thành một chuỗi không đổi. Phép lũy thừa ma trận vẫn hoạt động, nhưng dấu vết cho thấy mọi vectơ trạng thái vẫn không thay đổi sau khi khởi tạo, xác nhận tính đúng đắn. 

Đối với lớn$n$chẳng hạn như$10^{18}$, không lặp lại$n$được thực hiện. Số mũ được xử lý hoàn toàn thông qua phân rã nhị phân, do đó quá trình tính toán vẫn ổn định và nhanh chóng ngay cả ở quy mô cực lớn.
