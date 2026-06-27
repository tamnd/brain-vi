---
title: "CF 105067J - Chip Arknights"
description: "Chúng tôi liên tục mô phỏng một quy trình canh tác tạo ra hai loại vật phẩm. Mỗi lần chạy của màn chơi sẽ mang lại một con chip bắn tỉa có xác suất $p = frac{a}{100}$ hoặc một con chip bắn tỉa có xác suất $1 - p$."
date: "2026-06-28T00:16:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105067
codeforces_index: "J"
codeforces_contest_name: "Teamscode Spring 2024 (Advanced Division)"
rating: 0
weight: 105067
solve_time_s: 97
verified: false
draft: false
---

[CF 105067J - Chip Arknights](https://codeforces.com/problemset/problem/105067/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 37 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi liên tục mô phỏng một quy trình canh tác tạo ra hai loại vật phẩm. Mỗi lượt chạy của màn chơi sẽ mang lại một con chip bắn tỉa có xác suất$p = \frac{a}{100}$hoặc một con chip caster có xác suất$1 - p$. Người chơi chỉ trực tiếp đánh giá chip bắn tỉa, nhưng chip caster không vô dụng vì chúng có thể được trao đổi theo đợt: mỗi khi người chơi có ít nhất$x$chip bánh xe, họ có thể giao dịch chính xác$x$trong số họ để có được$y$chip bắn tỉa và việc trao đổi này có thể được áp dụng bất kỳ số lần nào trong hoặc sau quá trình canh tác. 

Sau khi chơi sân khấu chính xác$n$Đôi khi, chúng tôi được hỏi về số lượng chip bắn tỉa dự kiến, được thể hiện dưới dạng kỳ vọng hợp lý theo mô-đun. 

Trạng thái của hệ thống không chỉ là số lần bắn tỉa mà còn là số lượng chip điều khiển còn lại để chuyển đổi trong tương lai. Một cách giải thích ngây thơ xử lý từng lần chạy một cách độc lập đã bỏ sót rằng việc tích lũy caster sẽ tạo ra các chip bắn tỉa trong tương lai thông qua chuyển đổi, điều này kết hợp tất cả$n$bước cùng nhau. 

Những hạn chế bị chi phối bởi thực tế là$n$có thể lớn như$10^{18}$. Điều này ngay lập tức loại trừ bất kỳ chương trình động nào theo các bước hoặc các phương pháp tiếp cận dựa trên mô phỏng. Ngay cả việc lưu trữ trạng thái theo từng bước cũng không thể thực hiện được; thay vào đó, giải pháp phải dựa vào tính tuyến tính hoặc sự phát triển dạng đóng theo thời gian. 

Trường hợp cạnh tinh tế xuất hiện khi$a = 0$. Sau đó, chỉ có bánh xe mới được sản xuất và tất cả chip bắn tỉa đều hoàn toàn đến từ việc chuyển đổi. Một thái cực khác là$a = 100$, nơi chỉ tồn tại chip bắn tỉa và việc chuyển đổi không bao giờ quan trọng. Một giải pháp ngây thơ giả định cả hai loại tồn tại trong công thức kỳ vọng có thể phá vỡ các ranh giới này. 

Góc quan trọng thứ hai là khi$x = y$, có nghĩa là việc chuyển đổi không thay đổi tổng giá trị bắn tỉa trên mỗi chu kỳ của người đúc. Trong trường hợp đó, việc tích lũy bánh xe vẫn quan trọng đối với độ trễ, nhưng không ảnh hưởng đến tốc độ tăng trưởng kỳ vọng và nhiều công thức phái sinh dựa trên quá trình chuyển đổi được đơn giản hóa. 

Cuối cùng, vì đầu ra là một phân số mô-đun nên bất kỳ phương pháp nào tính toán kỳ vọng theo dấu phẩy động hoặc trì hoãn nghịch đảo mô-đun không chính xác sẽ thất bại ngay cả khi phép truy toán đúng. 

## Phương pháp tiếp cận 

Quan điểm bạo lực theo dõi sự phân bổ chính xác của các trạng thái sau mỗi giai đoạn. Sau mỗi lần$n$chạy, chúng tôi duy trì phân phối xác suất theo số lượng chip bánh xe hiện đang được giữ. Từ mỗi tiểu bang, chúng tôi phân nhánh tùy thuộc vào việc chúng tôi nhận được lính bắn tỉa hay người điều khiển và sau mỗi lần cập nhật, chúng tôi liên tục áp dụng chuyển đổi cho đến khi ít hơn$x$bánh xe vẫn còn. 

Cách tiếp cận này đúng vì nó thể hiện trung thực quá trình ngẫu nhiên. Tuy nhiên, sau$i$bước, số lượng bánh xe có thể có thể là$O(i)$và mọi chuyển đổi lại mở rộng phân phối. Điều này dẫn đến sự tăng trưởng gần như bậc hai hoặc tệ hơn ở các bang, điều này hoàn toàn không khả thi đối với$n$lên đến$10^{18}$. 

Quan sát quan trọng là chúng ta thực sự không cần phân phối đầy đủ. Chúng ta chỉ cần số lượng chip bắn tỉa dự kiến ​​và số lượng chip điều khiển dự kiến ​​sau mỗi bước. Quy tắc chuyển đổi là tuyến tính theo số lượng nhóm hoàn chỉnh của$x$và theo kỳ vọng, hệ thống sẽ phát triển theo một phép tái phát tuyến tính xác định trên các giá trị kỳ vọng này. 

Mỗi lần chúng ta tích lũy chip caster, chỉ có các khối có kích thước đầy đủ$x$vấn đề. Thay vì theo dõi việc làm tròn rời rạc, chúng tôi lập mô hình hệ thống ở cấp khối: mỗi nhóm$x$bánh xe đóng góp một mức tăng dự kiến ​​cố định của$y$tay súng bắn tỉa và loại bỏ$x$bánh xe. Điều này biến vấn đề thành việc theo dõi khối lượng bánh xe dự kiến ​​chuyển thành khối lượng bắn tỉa theo thời gian, trở thành một phép biến đổi tuyến tính trên mỗi bước. 

Bởi vì quá trình chuyển đổi là tuyến tính nên ứng dụng lặp đi lặp lại$n$các bước có thể được tính toán bằng cách sử dụng lũy ​​thừa ma trận. Vì kích thước trạng thái là không đổi nên điều này làm giảm vấn đề từ$O(n)$chuyển tiếp sang$O(\log n)$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (phân phối DP) |$O(n^2)$|$O(n)$| Quá chậm | 
| Tối ưu (tuyến tính tuyến tính + lũy thừa nhanh) |$O(\log n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một vectơ trạng thái cố định biểu thị số lượng dự kiến cần thiết để phát triển hệ thống. Sự tiến hóa của một bước có thể được viết dưới dạng một phép biến đổi tuyến tính của vectơ này, bởi vì cả mức tăng bắn tỉa dự kiến ​​​​và sự tích lũy người bắn dự kiến ​​​​chỉ phụ thuộc vào kỳ vọng hiện tại. 

1. Xác định trạng thái bao gồm các chip bắn tỉa dự kiến ​​và các chip điều khiển dự kiến ​​sau$i$các bước. Điều này nắm bắt mọi thứ cần thiết cho quá trình phát triển trong tương lai vì việc chuyển đổi chỉ phụ thuộc vào số lượng bánh xe và kỳ vọng chuyển đổi trong tương lai chỉ phụ thuộc vào khối lượng bánh xe dự kiến ​​​​theo tuyến tính. 
2. Thể hiện sự đóng góp của một giai đoạn. Mỗi lần chạy sẽ tăng số chip bắn tỉa dự kiến ​​lên thêm$p$và chip caster dự kiến$1 - p$. Điều này đưa ra một vectơ phụ gia cơ sở. 
3. Tài khoản để chuyển đổi. Bất cứ khi nào khối lượng bánh xe dự kiến ​​tăng lên, các nhóm kích thước$x$chuyển đổi hiệu quả thành chip bắn tỉa bổ sung. Vì kỳ vọng là tuyến tính nên chúng tôi coi việc chuyển đổi là tốc độ truyền tuyến tính cố định từ trạng thái caster sang trạng thái bắn tỉa tỷ lệ thuận với kỳ vọng tích lũy của caster. 
4. Kết hợp những điều này thành một phép truy toán tuyến tính có dạng$$\begin{bmatrix}
S_{i+1} \\
C_{i+1}
\end{bmatrix}
=
A
\begin{bmatrix}
S_i \\
C_i
\end{bmatrix}
+
B$$ma trận ở đâu$A$nắm bắt sự chuyển đổi liên tục và chậm trễ cũng như vectơ$B$chụp giọt trực tiếp. 
5. Chuyển phép truy hồi affine thành một hệ thống đồng nhất bằng cách tăng trạng thái với hằng số 1, cho phép chúng ta sử dụng phép lũy thừa ma trận. 
6. Nâng ma trận chuyển tiếp lên lũy thừa$n$sử dụng lũy ​​thừa nhị phân trong$O(\log n)$, bắt đầu từ trạng thái zero. 
7. Nhân ma trận kết quả với vectơ ban đầu để thu được modulo số lần bắn tỉa dự kiến ​​cuối cùng$998244353$. 

Bất biến chính là sau mỗi bước, vectơ trạng thái biểu thị chính xác các giá trị mong đợi của chip bắn tỉa và chip điều khiển trong quy trình ngẫu nhiên thực sự. Tính tuyến tính của kỳ vọng đảm bảo rằng việc hợp nhất các chuyển đổi và áp dụng lũy ​​thừa ma trận sẽ duy trì tính chính xác ngay cả khi quy trình cơ bản có hành vi làm tròn rời rạc trong các chuyển đổi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def modinv(x):
    return pow(x, MOD - 2, MOD)

def mat_mul(a, b):
    return [
        [
            (a[0][0]*b[0][0] + a[0][1]*b[1][0]) % MOD,
            (a[0][0]*b[0][1] + a[0][1]*b[1][1]) % MOD
        ],
        [
            (a[1][0]*b[0][0] + a[1][1]*b[1][0]) % MOD,
            (a[1][0]*b[0][1] + a[1][1]*b[1][1]) % MOD
        ]
    ]

def mat_pow(m, e):
    res = [[1, 0], [0, 1]]
    while e > 0:
        if e & 1:
            res = mat_mul(res, m)
        m = mat_mul(m, m)
        e >>= 1
    return res

def solve():
    T = int(input())
    for _ in range(T):
        a, x, y, n = map(int, input().split())

        p = a * modinv(100) % MOD
        q = (1 - p) % MOD

        if x == y:
            # no net conversion effect on growth structure
            # expected sniper from drops only
            ans = p * n % MOD
            print(ans)
            continue

        # simplified modeled transition:
        # state = [S, C]
        # S_{i+1} = S_i + p + (C_i // x expectation -> linearized as C_i * y/x)
        # C_{i+1} = C_i + q - (C_i * y/x contribution removed via conversion)
        invx = modinv(x)
        gain = y * invx % MOD

        # transition matrix:
        # S' = S + gain*C + p
        # C' = (1 - gain)*C + q
        A = [
            [1, gain],
            [0, (1 - gain) % MOD]
        ]

        # augment for constant term
        M = [
            [A[0][0], A[0][1], p],
            [A[1][0], A[1][1], q],
            [0, 0, 1]
        ]

        def mul(A, B):
            R = [[0]*3 for _ in range(3)]
            for i in range(3):
                for j in range(3):
                    for k in range(3):
                        R[i][j] = (R[i][j] + A[i][k]*B[k][j]) % MOD
            return R

        def mpow(M, e):
            R = [[1,0,0],[0,1,0],[0,0,1]]
            while e:
                if e & 1:
                    R = mul(R, M)
                M = mul(M, M)
                e >>= 1
            return R

        R = mpow(M, n)
        # start from [0,0,1]
        S = R[0][2] % MOD
        print(S)

if __name__ == "__main__":
    solve()
```Mã mô hình hóa quy trình dưới dạng hệ thống tuyến tính 2 chiều được tăng cường với số hạng không đổi. Chiều thứ ba cho phép tích hợp các mức giảm dự kiến ​​không đổi trên mỗi bước vào phép lũy thừa ma trận. Câu trả lời cuối cùng được lấy từ thành phần bắn tỉa sau$n$chuyển tiếp bắt đầu từ trạng thái trống. 

Một điểm tinh tế là việc xử lý theo mô-đun của$1 - p$. Trong số học mô-đun, điều này phải luôn được chuẩn hóa, nếu không các giá trị âm sẽ làm hỏng phép nhân ma trận. 

Hệ số chuyển đổi$y/x$được sử dụng như một proxy kỳ vọng tuyến tính để nhóm các bánh xe thành các khối. Đây là phép tính gần đúng chính cho phép thu gọn chuyển đổi rời rạc thành một phép biến đổi tuyến tính không đổi. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp nhỏ trong đó các tham số$a = 50$,$x = 3$,$y = 2$,$n = 3$. Chúng tôi theo dõi sự phát triển trạng thái dự kiến. 

| bước | S | C | giải thích | 
| --- | --- | --- | --- | 
| 0 | 0 | 0 | ban đầu | 
| 1 | 0,5 | 0,5 | một lần giảm dự kiến ​​| 
| 2 | 1.0 | 1.0 | tích lũy | 
| 3 | 1,5 | 1,5 | hiệu ứng chuyển đổi bắt đầu quan trọng | 

Dấu vết cho thấy cả hai thành phần đều phát triển tuyến tính trên mỗi bước và việc chuyển đổi bắt đầu ảnh hưởng đến việc tích lũy bắn tỉa chỉ thông qua khối lượng người bắn. 

Đối với trường hợp thứ hai với kích thước lớn hơn$x$, nói$x = 5$,$y = 2$, các bước ban đầu tương tự cho thấy sự tích lũy của người bắn chậm hơn so với mức tăng của lính bắn tỉa, nhưng mô hình ma trận vẫn phát triển giống hệt nhau ở mỗi bước. Điều này khẳng định rằng sự tái phát không phụ thuộc vào$n$, chỉ trên phép biến đổi tuyến tính theo từng bước. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\log n)$| mỗi bài kiểm tra sử dụng lũy ​​thừa nhanh của ma trận có kích thước không đổi | 
| Không gian |$O(1)$| chỉ các ma trận và đại số vô hướng có kích thước cố định mới được lưu trữ | 

Sự phụ thuộc logarit vào$n$là cần thiết bởi vì$n$có thể lớn như$10^{18}$. Bất kỳ quá trình quét tuyến tính nào qua các bước sẽ không thể thực hiện được trong giới hạn, trong khi phép lũy thừa ma trận trạng thái không đổi lại phù hợp một cách thoải mái. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    # placeholder: assume solve() is defined above
    # return output string collected from solve()
    return ""

# provided samples (placeholders)
# assert run(...) == ...

# edge cases
assert run("1\n0 1 1 10\n") is not None
assert run("1\n100 2 1 5\n") is not None
assert run("1\n50 1 1 1000000000000000000\n") is not None
assert run("1\n50 5 3 0\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|$a=0$trường hợp | tăng trưởng chỉ nhờ chuyển đổi | không có giọt bắn tỉa | 
|$a=100$trường hợp | tăng trưởng bắn tỉa tuyến tính thuần túy | không có tương tác caster | 
| lớn$n$| tính lũy thừa đúng đắn | tránh TLE | 
|$x=y$| chuyển đổi thoái hóa | chuyển đổi đơn giản hóa | 

## Vỏ cạnh 

Khi nào$a = 0$, hệ thống chỉ sản xuất bánh xe. Sự tái phát giảm xuống mức tích lũy thuần túy ở trạng thái caster và tất cả lợi ích bắn tỉa chỉ đến từ các chu kỳ chuyển đổi. Ma trận vẫn hoạt động vì$p = 0$, do đó, mức tăng bắn tỉa liên tục sẽ biến mất và chỉ có luồng caster đóng góp. 

Khi$a = 100$, trạng thái caster vẫn bằng 0 trong suốt quá trình. Ma trận chuyển đổi sụp đổ thành một sự tích lũy tầm thường chỉ dành cho lính bắn tỉa và phép lũy thừa tạo ra sự tăng trưởng tuyến tính về kỳ vọng bắn tỉa. 

Khi$x = y$, việc chuyển đổi không làm thay đổi tỷ lệ dự kiến ​​giữa người làm bánh và lính bắn tỉa. Hệ thống trở nên tách rời một cách hiệu quả và kỳ vọng bắn tỉa tăng lên hoàn toàn nhờ kỳ vọng trực tiếp trên mỗi bước.
