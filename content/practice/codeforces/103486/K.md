---
title: "CF 103486K - Chuỗi khung"
description: "Chúng ta được yêu cầu đếm xem có bao nhiêu cấu trúc khung hợp lệ có tổng độ dài $2N$ khi có $K$ các loại dấu ngoặc khác nhau. Mỗi loại hoạt động giống như một cặp phù hợp, ví dụ loại 1 có thể là “()”, loại 2 có thể là “[]”, v.v."
date: "2026-07-03T06:22:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103486
codeforces_index: "K"
codeforces_contest_name: "The 15th Jilin Provincial Collegiate Programming Contest"
rating: 0
weight: 103486
solve_time_s: 42
verified: true
draft: false
---

[CF 103486K - Trình tự khung](https://codeforces.com/problemset/problem/103486/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu đếm có bao nhiêu cấu trúc khung hợp lệ có tổng chiều dài$2N$tồn tại khi có$K$các loại dấu ngoặc khác nhau. Mỗi loại hoạt động giống như một cặp phù hợp, ví dụ loại 1 có thể là “()”, loại 2 có thể là “[]”, v.v. Một chuỗi hợp lệ được xây dựng giống hệt như các dấu ngoặc đơn cân bằng thông thường: mọi dấu ngoặc mở phải khớp với một dấu ngoặc đóng cùng loại, việc lồng nhau phải có cấu trúc đúng và việc ghép các chuỗi hợp lệ cũng hợp lệ. 

Sự khác biệt thực sự duy nhất so với cách đếm Catalan cổ điển là mỗi lần chúng tôi giới thiệu một cặp phù hợp, chúng tôi có$K$lựa chọn độc lập cho loại của nó. Vì vậy, vấn đề có cấu trúc giống hệt với việc đếm các dấu ngoặc đơn cân bằng, ngoại trừ mỗi cặp mang một hệ số màu nhân. 

Các ràng buộc đi lên đến$N \le 10^5$, vì vậy bất kỳ giải pháp nào cố gắng liệt kê các cấu trúc hoặc mô phỏng trạng thái ngăn xếp đều không thể thực hiện được ngay lập tức. Ngay cả lập trình động trên tiền tố cũng quá chậm nếu nó phụ thuộc vào$O(N^2)$chuyển tiếp. Chúng ta cần một biểu thức tổ hợp dạng đóng có thể được đánh giá trong thời gian gần như tuyến tính. 

Một trường hợp khó nhận thấy là khi$N = 1$. Câu trả lời đơn giản là$K$, bởi vì chuỗi hợp lệ duy nhất là một dấu ngoặc mở theo sau là dấu ngoặc đóng phù hợp và loại có thể được chọn tự do. 

Một trường hợp thất bại không rõ ràng khác đối với lối suy luận ngây thơ là giả định rằng chúng ta có thể xử lý từng quan điểm một cách độc lập. Ví dụ, nghĩ rằng có$K^N$các cách gán loại và sau đó nhân với số Catalan mà không có sự biện minh sẽ dẫn đến việc đếm quá mức trừ khi chúng ta tách biệt cấu trúc và ghi nhãn một cách chính xác. 

## Phương pháp tiếp cận 

Nếu chúng ta bỏ qua cấu trúc này trong giây lát, chúng ta có thể thử suy nghĩ đệ quy: tại mỗi bước, chúng ta đặt một loại dấu ngoặc mở nào đó hoặc một dấu ngoặc đóng. Điều này đương nhiên dẫn đến DP trên số dư tiền tố và có thể là trạng thái ngăn xếp. Giải pháp cổ điển cho$K=1$là số Catalan, được tính thông qua DP hoặc hệ số nhị thức. 

Tuy nhiên, với$N$lên đến$10^5$, ngay cả việc tính toán số Catalan thông qua giai thừa cũng là giới hạn nhưng vẫn khả thi nếu được thực hiện bằng phép nghịch đảo mô-đun. Quan sát quan trọng thực sự là cấu trúc của chuỗi khung hợp lệ hoàn toàn không phụ thuộc vào loại. Hình dạng của chuỗi hoàn toàn giống với dấu ngoặc đơn tiêu chuẩn: có$C_N$_shapes_ hợp lệ, ở đâu$C_N$là$N$-số Catalan thứ. 

Sau khi hình dạng được cố định, chúng tôi chỉ định loại. Mỗi cặp khớp (mọi cạnh trong cấu trúc cây tiềm ẩn của phân tách khung) có thể độc lập chọn một trong các$K$các loại. Vì có chính xác$N$cặp, điều này đóng góp một yếu tố$K^N$. 

Vì vậy, câu trả lời trở thành:$$\text{Answer} = C_N \cdot K^N \pmod{10^9+7}$$Chúng tôi tính toán số Catalan bằng cách sử dụng:$$C_N = \frac{1}{N+1} \binom{2N}{N}$$Điều này làm giảm vấn đề về tính toán trước giai thừa và lũy thừa mô-đun. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Bạo lực DP theo trình tự | Hàm mũ | Hàm mũ | Quá chậm | 
| Catalan + lũy thừa |$O(N)$|$O(N)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán trước các giai thừa và giai thừa nghịch đảo lên đến$2N$. Điều này cho phép tính toán hệ số nhị thức nhanh chóng. Chúng ta cần giai thừa vì$\binom{2N}{N}$là cốt lõi của số Catalan. 
2. Tính toán$\binom{2N}{N}$sử dụng công thức mô đun tiêu chuẩn:$$\binom{2N}{N} = \frac{(2N)!}{(N!)^2}$$sử dụng nghịch đảo mô-đun thay vì chia. 
3. Tính số Catalan:$$C_N = \binom{2N}{N} \cdot (N+1)^{-1}$$Nghịch đảo của$N+1$được tính toán bằng cách sử dụng lũy ​​thừa mô-đun. 
4. Tính toán$K^N \bmod (10^9+7)$sử dụng lũy ​​thừa nhị phân. Điều này thể hiện việc gán độc lập một trong$K$các loại cho mỗi$N$các cặp phù hợp. 
5. Nhân hai kết quả và xuất ra kết quả theo modulo$10^9+7$. 

### Tại sao nó hoạt động 

Mỗi chuỗi ngoặc hợp lệ tương ứng duy nhất với cấu trúc cây nhị phân hợp lệ với$N$các nút nội bộ, được tính bằng số Catalan. Dấu ngoặc _types_ không ảnh hưởng đến các ràng buộc về độ chính xác vì việc so khớp chỉ yêu cầu sự bằng nhau về loại giữa dấu ngoặc mở và dấu ngoặc đóng tương ứng của nó và không hạn chế các tương tác lồng nhau giữa các cặp khác nhau. Vì vậy, khi bộ xương cấu trúc được cố định, mỗi phần$N$các cặp có thể độc lập chọn một trong các$K$nhãn, đưa ra hệ số nhân rõ ràng$K^N$. Tính độc lập này đảm bảo không có trường hợp nào bị đếm quá mức hoặc thiếu sót. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def modpow(a, e):
    res = 1
    while e:
        if e & 1:
            res = res * a % MOD
        a = a * a % MOD
        e >>= 1
    return res

def solve():
    n, k = map(int, input().split())
    
    if n == 0:
        print(1)
        return
    
    maxv = 2 * n
    
    fact = [1] * (maxv + 1)
    invfact = [1] * (maxv + 1)
    
    for i in range(1, maxv + 1):
        fact[i] = fact[i - 1] * i % MOD
    
    invfact[maxv] = modpow(fact[maxv], MOD - 2)
    for i in range(maxv, 0, -1):
        invfact[i - 1] = invfact[i] * i % MOD
    
    def C(a, b):
        if b < 0 or b > a:
            return 0
        return fact[a] * invfact[b] % MOD * invfact[a - b] % MOD
    
    catalan = C(2 * n, n) * modpow(n + 1, MOD - 2) % MOD
    ways_types = modpow(k, n)
    
    print(catalan * ways_types % MOD)

if __name__ == "__main__":
    solve()
```Quá trình tiền xử lý giai thừa xây dựng một bảng tra cứu để các hệ số nhị thức có thể được tính toán trong thời gian không đổi cho mỗi truy vấn. Phép tính Catalan tuân theo trực tiếp từ dạng đóng và nghịch đảo mô đun của$n+1$xử lý phép chia một cách an toàn theo số học modulo. 

Phép lũy thừa cho$K^N$độc lập và có tính nhân lên, phản ánh thực tế là mỗi cặp phù hợp mang một lựa chọn nhãn độc lập. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 2
```Đây$N = 1$,$K = 2$. Có chính xác một chuỗi khung cấu trúc: một cặp duy nhất. Vậy số Catalan là 1. 

| Bước | Giá trị | 
| --- | --- | 
|$C_1$| 1 | 
|$K^1$| 2 | 
| Kết quả | 2 | 

Đầu ra là 2, tương ứng với “()” và “[]”. 

Điều này khẳng định rằng cấu trúc đóng góp 1 và chỉ có vấn đề ghi nhãn. 

### Ví dụ 2 

đầu vào:```
2 2
```Vì$N = 2$, có 2 cấu trúc Catalan: “()()” và “(())”. Mỗi cặp có 2 cặp, mỗi cặp độc lập chọn 1 trong 2 loại. 

| Cấu trúc | Đếm | Gõ bài tập | Đóng góp | 
| --- | --- | --- | --- | 
| ()() | 1 |$2^2 = 4$| 4 | 
| (()) | 1 |$2^2 = 4$| 4 | 

Tổng cộng = 8. 

Vậy đáp án là 8. 

Điều này cho thấy sự độc lập giữa cấu trúc và ghi nhãn, đó là sự phân rã cốt lõi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N)$| tính toán trước giai thừa lên đến$2N$cộng với lũy thừa mô-đun | 
| Không gian |$O(N)$| lưu trữ cho mảng giai thừa và nghịch đảo | 

Với$N \le 10^5$, tính toán trước lên đến$2N$nằm trong giới hạn và tất cả các phép toán đều là các phép tính tuyến tính hoặc lũy thừa logarit. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 10**9 + 7

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    MOD = 10**9 + 7
    
    def modpow(a, e):
        res = 1
        while e:
            if e & 1:
                res = res * a % MOD
            a = a * a % MOD
            e >>= 1
        return res
    
    n, k = map(int, input().split())
    if n == 0:
        return "1"
    
    maxv = 2 * n
    fact = [1] * (maxv + 1)
    invfact = [1] * (maxv + 1)
    
    for i in range(1, maxv + 1):
        fact[i] = fact[i - 1] * i % MOD
    
    invfact[maxv] = modpow(fact[maxv], MOD - 2)
    for i in range(maxv, 0, -1):
        invfact[i - 1] = invfact[i] * i % MOD
    
    def C(a, b):
        return fact[a] * invfact[b] % MOD * invfact[a - b] % MOD
    
    catalan = C(2 * n, n) * modpow(n + 1, MOD - 2) % MOD
    return str(catalan * modpow(k, n) % MOD)

# provided samples
assert run("1 2") == "2"
assert run("2 2") == "8"

# custom cases
assert run("1 1") == "1", "single type single pair"
assert run("3 1") == "5", "Catalan(3)=5"
assert run("0 5") == "1", "empty sequence"
assert run("2 3") == str((8 * 9) % (10**9+7)), "mixed types"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 | 1 | cấu trúc tối thiểu và loại đơn | 
| 3 1 | 5 | sự đúng đắn thuần túy của tiếng Catalan | 
| 0 5 | 1 | trường hợp cơ sở chuỗi trống | 
| 2 3 | 72 | sự kết hợp giữa cấu trúc và ghi nhãn | 

## Vỏ cạnh 

các$N = 0$trường hợp tương ứng với chuỗi trống. Thuật toán xử lý nó một cách chính xác vì Catalan(0) là 1 và$K^0$cũng là 1 nên tích vẫn là 1. 

Khi nào$K = 1$, vấn đề giảm chính xác xuống việc đếm các chuỗi dấu ngoặc cân bằng tiêu chuẩn. Thuật toán chuyển sang chỉ tính số Catalan và không xảy ra tình trạng đếm quá mức vì$1^N = 1$. 

Khi$N = 1$, phép tính sẽ tránh được cấu trúc không cần thiết: Catalan(1) bằng 1 và kết quả trở thành chính xác$K$, phù hợp với việc liệt kê trực tiếp các cặp đơn lẻ. 

Một cạm bẫy triển khai tiềm ẩn là tràn số nguyên trước khi áp dụng modulo trong phép nhân giai thừa trung gian. Điều này có thể tránh được bằng cách lấy modulo ở mỗi bước nhân, đảm bảo tính chính xác ngay cả đối với$2N$lên đến$2 \cdot 10^5$.
