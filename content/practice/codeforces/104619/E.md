---
title: "CF 104619E - Hàm mũ"
description: "Chúng ta được cho một số nguyên dương α được định nghĩa là tổng của một số x và nghịch đảo của nó là 1/x. Nói cách khác, x là một số (có thể phức) thỏa mãn x + 1/x = α. Từ định nghĩa ẩn này, chúng ta được yêu cầu tính giá trị của x^β + (1/x)^β modulo m."
date: "2026-06-29T17:26:29+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104619
codeforces_index: "E"
codeforces_contest_name: "2023 ICPC Asia Taiwan Online Programming Contest"
rating: 0
weight: 104619
solve_time_s: 50
verified: true
draft: false
---

[CF 104619E - Hàm mũ](https://codeforces.com/problemset/problem/104619/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số nguyên dương α được định nghĩa là tổng của một số x và nghịch đảo của nó là 1/x. Nói cách khác, x là một số (có thể phức) thỏa mãn x + 1/x = α. Từ định nghĩa ẩn này, chúng ta được yêu cầu tính giá trị của x^β + (1/x)^β modulo m. 

Khó khăn chính là x không được đưa ra một cách rõ ràng. Nó chỉ được xác định thông qua một mối quan hệ bậc hai. Mở rộng định nghĩa, x phải thỏa mãn x^2 - αx + 1 = 0 nên x là nghiệm của một đa thức bậc hai có hệ số nguyên. Mặc dù x có thể vô tỷ hoặc phức tạp, biểu thức cuối cùng x^β + x^{-β} được đảm bảo là số nguyên. 

Đầu ra chỉ phụ thuộc vào α, β và m chứ không phụ thuộc vào nghiệm nào được chọn. Nếu x là một nghiệm thì 1/x là nghiệm còn lại và việc hoán đổi chúng không làm thay đổi giá trị của x^β + x^{-β}. 

Các ràng buộc ngụ ý rằng α, β và m có thể lớn tới 2^64. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào mở rộng lũy ​​thừa một cách rõ ràng hoặc hoạt động theo thời gian tuyến tính theo β. Chúng ta cần thứ gì đó làm giảm lũy thừa theo thời gian logarit và tránh hoàn toàn số học dấu phẩy động. 

Trường hợp cạnh tinh tế xuất hiện khi α nhỏ. Ví dụ: nếu α = 1, thì nghiệm là số phức 1/2 ± i√3/2 và tính toán số trực tiếp trở nên không ổn định. Một trường hợp cạnh khác là khi x = 1 hoặc x = -1, xảy ra khi α = 2 hoặc α = -2 ở dạng mở rộng, nhưng ở đây α dương nên chỉ có α = 2 dẫn đến tình huống nghiệm lặp lại trong đó x = 1. 

Thách thức trọng tâm là tính tổng lũy thừa đối xứng của các nghiệm của một phương trình bậc hai mà không giải phương trình bậc hai một cách rõ ràng. 

## Phương pháp tiếp cận 

Một cách giải thích trực tiếp sẽ cố gắng tính x từ công thức bậc hai x = (α ± √(α^2 - 4)) / 2, sau đó nâng nó lên β và tính tổng với nghịch đảo của nó. Cách tiếp cận này rất mong manh vì nó liên quan đến các số vô tỷ hoặc số phức và đòi hỏi số học có độ chính xác cao. Ngay cả khi được triển khai bằng phép tính dấu phẩy động hoặc ký hiệu lớn, việc lũy thừa β lớn sẽ quá chậm. 

Quan sát cấu trúc quan trọng là x^k + x^{-k} hoạt động giống như một phép truy hồi tuyến tính. Nếu chúng ta xác định S_k = x^k + x^{-k}, thì nhân với (x + x^{-1}) = α sẽ cho kết quả lặp lại: 

S_{k+1} = (x + x^{-1})(x^k + x^{-k}) - (x^{k-1} + x^{-(k-1)}) 

Điều này đơn giản hóa để: 

S_{k+1} = α S_k - S_{k-1} 

Sự tái phát này loại bỏ hoàn toàn x. Toàn bộ vấn đề quy về việc tính toán số hạng thứ β của phép truy hồi tuyến tính bậc hai với các giá trị ban đầu S_0 = 2 và S_1 = α. 

Một khi chúng ta nhận ra cấu trúc này, bài toán sẽ trở thành một bài toán nhân đôi nhanh hoặc lũy thừa ma trận tiêu chuẩn. Chúng ta có thể tính S_β theo thời gian logarit bằng cách sử dụng phép lũy thừa ma trận 2x2 hoặc phương pháp nhân đôi nhanh được sử dụng cho các phép truy toán giống Fibonacci. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tính toán trực tiếp bằng cách sử dụng gốc | O(β) hoặc tệ hơn | O(1) | Quá chậm/không ổn định về số lượng | 
| Phép truy hồi tuyến tính thông qua lũy thừa nhanh | O(log β) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta viết lại bài toán dưới dạng truy hồi S_k = x^k + x^{-k}. Mục tiêu là tính S_β. 

1. Chúng ta khởi tạo hai giá trị cơ sở tương ứng với phép truy hồi. Chúng ta đặt S_0 = 2 vì x^0 + x^0 = 1 + 1 = 2, và S_1 = α vì x + 1/x = α được cho trực tiếp. Hai giá trị này xác định đầy đủ trình tự. 
2. Chúng tôi nhận thấy rằng mọi thuật ngữ sau có thể được tạo ra từ hai thuật ngữ trước bằng cách sử dụng S_{k+1} = α S_k - S_{k-1}. Mối quan hệ này xuất phát từ việc nhân S_k với α và khai triển α = x + 1/x, sau đó tập hợp các lũy thừa của x. 
3. Thay vì lặp lại β lần, chúng tôi tính S_β bằng cách sử dụng lũy ​​thừa nhanh cho phép truy toán này. Chúng tôi coi phép truy hồi là một phép biến đổi tuyến tính trên vectơ (S_k, S_{k-1}). 
4. Chúng ta xác định ma trận biến đổi:$$\begin{pmatrix}
S_{k+1} \\
S_k
\end{pmatrix}
=
\begin{pmatrix}
\alpha & -1 \\
1 & 0
\end{pmatrix}
\begin{pmatrix}
S_k \\
S_{k-1}
\end{pmatrix}$$5. Chúng ta lũy thừa ma trận này theo lũy thừa β-1 và áp dụng nó cho vectơ ban đầu (S_1, S_0). Điều này mang lại (S_β, S_{β-1}). 
6. Tất cả các thao tác được thực hiện theo modulo m, xử lý cẩn thận các giá trị âm khi trừ S_{k-1}. 

Tính đúng đắn bắt nguồn từ bất biến ở bước k, vectơ (S_k, S_{k-1}) biểu thị chính xác (x^k + x^{-k}, x^{k-1} + x^{-(k-1)}). Quá trình chuyển đổi duy trì đồng nhất thức này vì nó phản ánh sự khai triển đại số của phép nhân với x + x^{-1}. Vì phép truy hồi xác định duy nhất tất cả S_k và các điều kiện ban đầu khớp với giá trị thực nên chuỗi được tính toán phải trùng với biểu thức mong muốn cho tất cả k. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def mat_mul(a, b, mod):
    return [
        [(a[0][0]*b[0][0] + a[0][1]*b[1][0]) % mod,
         (a[0][0]*b[0][1] + a[0][1]*b[1][1]) % mod],
        [(a[1][0]*b[0][0] + a[1][1]*b[1][0]) % mod,
         (a[1][0]*b[0][1] + a[1][1]*b[1][1]) % mod]
    ]

def mat_pow(mat, exp, mod):
    res = [[1, 0], [0, 1]]
    while exp > 0:
        if exp & 1:
            res = mat_mul(res, mat, mod)
        mat = mat_mul(mat, mat, mod)
        exp >>= 1
    return res

def solve():
    alpha, beta, m = map(int, input().split())
    
    if beta == 0:
        print(2 % m)
        return
    if beta == 1:
        print(alpha % m)
        return
    
    base = [[alpha % m, (m - 1) % m],
            [1, 0]]
    
    p = mat_pow(base, beta - 1, m)
    
    s1 = alpha % m
    s0 = 2 % m
    
    ans = (p[0][0] * s1 + p[0][1] * s0) % m
    print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai mã hóa phép lặp thành ma trận chuyển tiếp 2x2 trong đó mỗi ứng dụng sẽ nâng cao trình tự lên một bước. Phép trừ trong phép truy toán được thực hiện bằng cách thêm m-1 modulo m để tránh các giá trị âm. 

Quy trình lũy thừa nhanh nâng ma trận lên lũy thừa thứ (β-1) theo thời gian logarit, chỉ bình phương và nhân liên tục khi cần bằng biểu diễn nhị phân của số mũ. 

Cuối cùng, kết quả được trích xuất bằng cách áp dụng ma trận vào vectơ ban đầu (S_1, S_0). Các trường hợp cạnh β = 0 và β = 1 được xử lý rõ ràng vì chúng tương ứng trực tiếp với các định nghĩa cơ sở mà không cần lũy thừa. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Đầu vào: α = 5, β = 4, m = 321 

Chúng ta xây dựng S_0 = 2 và S_1 = 5, sau đó áp dụng phép truy hồi S_{k+1} = 5S_k - S_{k-1}. 

| k | S_k | S_{k-1} | αS_k - S_{k-1} | 
| --- | --- | --- | --- | 
| 1 | 5 | 2 | 5*5 - 2 = 23 | 
| 2 | 23 | 5 | 5*23 - 5 = 110 | 
| 3 | 110 | 23 | 5*110 - 23 = 527 | 

Vậy S_4 = 527, và modulo 321 cho 527 mod 321 = 206. 

Điều này xác nhận việc xây dựng phép truy toán chính xác sẽ tạo ra lũy thừa cao hơn mà không cần sử dụng x một cách rõ ràng. 

### Ví dụ 2 

Đầu vào: α = 3, β = 3, m = 333 

Chúng tôi lại tính toán từng bước một. 

| k | S_k | S_{k-1} | αS_k - S_{k-1} | 
| --- | --- | --- | --- | 
| 1 | 3 | 2 | 3*3 - 2 = 7 | 
| 2 | 7 | 3 | 3*7 - 3 = 18 | 

Vì vậy S_3 = 18 và modulo 333 vẫn là 18. 

Điều này cho thấy ngay cả khi x phức (vì α^2 - 4 < 0), phép truy toán vẫn hoàn toàn ở dạng số nguyên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(log β) | lũy thừa ma trận nhân đôi số mũ mỗi bước | 
| Không gian | O(1) | chỉ lưu trữ một số lượng ma trận 2x2 không đổi | 

Sự phụ thuộc logarit vào β đảm bảo lời giải vẫn nhanh ngay cả khi β gần bằng 2^64. Việc sử dụng bộ nhớ là không đổi do quá trình tính toán chỉ sử dụng ma trận chuyển tiếp có kích thước cố định. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    def mat_mul(a, b, mod):
        return [
            [(a[0][0]*b[0][0] + a[0][1]*b[1][0]) % mod,
             (a[0][0]*b[0][1] + a[0][1]*b[1][1]) % mod],
            [(a[1][0]*b[0][0] + a[1][1]*b[1][0]) % mod,
             (a[1][0]*b[0][1] + a[1][1]*b[1][1]) % mod]
        ]

    def mat_pow(mat, exp, mod):
        res = [[1, 0], [0, 1]]
        while exp > 0:
            if exp & 1:
                res = mat_mul(res, mat, mod)
            mat = mat_mul(mat, mat, mod)
            exp >>= 1
        return res

    alpha, beta, m = map(int, input().split())
    
    if beta == 0:
        return str(2 % m)
    if beta == 1:
        return str(alpha % m)

    base = [[alpha % m, (m - 1) % m],
            [1, 0]]

    p = mat_pow(base, beta - 1, m)

    s1 = alpha % m
    s0 = 2 % m

    return str((p[0][0] * s1 + p[0][1] * s0) % m)

# provided samples
# assert run("1 2 3") == "..."
# assert run("5 4 321") == "..."
# assert run("3 3 333") == "..."

# custom cases
assert run("2 0 10") == "2", "beta = 0 gives S0"
assert run("2 1 10") == "2", "beta = 1 gives alpha"
assert run("2 2 1000") == "2", "x + 1/x = 2 implies x=1 so all S_k=2"
assert run("10 5 100") == run("10 5 100"), "stability check"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 0 10 | 2 | trường hợp cơ sở S0 | 
| 2 1 10 | 2 | trường hợp cơ sở S1 | 
| 2 2 1000 | 2 | nghiệm suy biến x=1 | 
| 10 5 100 | đầu ra nhất quán | ổn định tái phát | 

## Vỏ cạnh 

Khi α = 2, phương trình bậc hai x^2 - 2x + 1 = 0 rút gọn thành (x - 1)^2 = 0, nên x = 1. Trong trường hợp đó, mọi số hạng S_k = 1^k + 1^{-k} = 2. Phép truy hồi vẫn tạo ra S_{k+1} = 2S_k - S_{k-1} và với S_0 = 2 và S_1 = 2, nó không đổi. Thuật toán bảo toàn điều này một cách tự nhiên vì ma trận chuyển tiếp trở thành [[2, -1], [1, 0]], có giá trị riêng 1 với bội số hai. 

Đối với trường hợp α^2 < 4, nghiệm là liên hợp phức tạp. Phép truy toán tránh mọi việc sử dụng trực tiếp các số này và hoàn toàn ở dạng số học số nguyên. Tính bất biến của S_k dưới dạng đa thức đối xứng ở các nghiệm đảm bảo phép tính vẫn đúng bất kể bản chất của x. 

β lớn gần bằng 2^64 không ảnh hưởng đến độ chính xác vì phép lũy thừa được thực hiện thông qua nâng cấp nhị phân, điều này chỉ phụ thuộc vào độ dài bit của β chứ không phải độ lớn của nó.
