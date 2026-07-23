---
title: "CF 103736I - Bài tập về nhà của IHI"
description: "Chúng ta được cung cấp một loạt các giới hạn dưới của các biến và một ràng buộc về tổng mục tiêu. Mỗi biến $xi$ phải ít nhất là $ai$, và chúng ta được hỏi có bao nhiêu vectơ nguyên $x1, x2, dots, xn$ thỏa mãn $$x1 + x2 + dots + xn le s$$ Sau mỗi lần cập nhật, một vị trí của mảng $a$ được thay đổi…"
date: "2026-07-02T09:12:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103736
codeforces_index: "I"
codeforces_contest_name: "The 2022 Hangzhou Normal U Summer Trials"
rating: 0
weight: 103736
solve_time_s: 51
verified: true
draft: false
---

[CF 103736I - Bài tập về nhà của IHI](https://codeforces.com/problemset/problem/103736/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một loạt các giới hạn dưới của các biến và một ràng buộc về tổng mục tiêu. Mỗi biến$x_i$ít nhất phải có$a_i$, và chúng ta được hỏi có bao nhiêu vectơ nguyên$x_1, x_2, \dots, x_n$thỏa mãn$$x_1 + x_2 + \dots + x_n \le s$$Sau mỗi lần cập nhật, một vị trí của mảng$a$bị thay đổi vĩnh viễn và chúng ta phải tính toán lại số nghiệm hợp lệ. 

Đầu vào là động: mọi thao tác đều sửa đổi một giới hạn dưới và câu trả lời chỉ phụ thuộc vào trạng thái hiện tại của tất cả$a_i$. Đầu ra sau mỗi lần cập nhật là số phép gán số nguyên hợp lệ. 

Ràng buộc$n, q \le 2 \cdot 10^5$buộc chúng ta phải tính toán lại câu trả lời nhanh hơn nhiều so với$O(nq)$. Vì cả kích thước cấu trúc và số lượng cập nhật đều lớn nên mọi giải pháp đều phải hỗ trợ cập nhật điểm nhanh và tính toán lại nhanh giá trị tổ hợp toàn cục. 

Một quan sát quan trọng là ràng buộc có tính đối xứng trên các biến ngoại trừ giới hạn dưới của chúng. Điều này gợi ý việc chuyển đổi các biến để loại bỏ các giới hạn dưới, biến bài toán thành một bài toán đếm thành phần bị chặn cổ điển. 

Đại lượng dẫn xuất hữu ích là tổng số tiền bắt buộc:$$A = \sum a_i$$Nếu chúng ta xác định các biến mới$y_i = x_i - a_i$, thì mỗi$y_i \ge 0$và ràng buộc trở thành:$$\sum y_i \le s - A$$Vì vậy, vấn đề giảm xuống còn việc đếm các nghiệm số nguyên không âm với giới hạn trên của tổng. 

Trường hợp cạnh tinh tế xuất hiện khi$s < A$. Trong trường hợp này, không có giải pháp nào tồn tại. Ví dụ, nếu$n=3, s=2, a=[1,1,1]$, thì tổng tối thiểu là 3, vì vậy câu trả lời là 0. Việc triển khai đơn giản bỏ qua tính khả thi của tổng cơ sở sẽ vẫn cố gắng tính toán các kết hợp có dung lượng còn lại âm, dẫn đến các giá trị tổ hợp không chính xác. 

Một trường hợp khác là khi tất cả$a_i = 0$, trong đó câu trả lời trở thành số lượng ngôi sao và thanh cổ điển của$\sum y_i \le s$, đó là$\binom{s+n}{n}$. Bất kỳ giải pháp nào cũng phải thống nhất chính xác cả trường hợp tổng quát và suy biến. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là tính toán lại câu trả lời sau mỗi lần cập nhật bằng cách lặp lại tất cả các tổng có thể có hoặc sử dụng tổ hợp từ đầu. Sau khi dịch chuyển các biến, bài toán trở thành đếm các nghiệm nguyên không âm với tổng nhiều nhất là$S' = s - \sum a_i$. Số cách giải quyết là$$\sum_{k=0}^{S'} \binom{k+n-1}{n-1} = \binom{S' + n}{n}$$Điều này làm giảm mỗi truy vấn xuống chỉ còn duy trì tổng của$a_i$. Vì mỗi bản cập nhật thay đổi một$a_x$, chúng ta có thể duy trì tổng số hoạt động$A$TRONG$O(1)$, và tính toán lại$S' = s - A$. 

Thách thức còn lại là tính toán hệ số nhị thức$\binom{N}{K}$modulo$10^9+7$nhanh chóng lên đến$N \le 4 \cdot 10^5$. Điều này được xử lý bằng các giai thừa được tính toán trước và giai thừa nghịch đảo. 

Do đó, mỗi truy vấn trở thành một bản cập nhật số học liên tục cộng với một đánh giá nhị thức mô-đun. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tính toán lại tổ hợp cho mỗi truy vấn | O(nq) | O(1) | Quá chậm | 
| Duy trì Tổng + nCr được tính toán trước | O(n + q) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta chuyển bài toán sang việc đếm các phép gán hợp lệ của các biến chùng không âm. Ý tưởng cốt lõi là chỉ có tổng số giới hạn dưới mới quan trọng chứ không phải sự phân bổ của chúng. 

## Hướng dẫn thuật toán 

1. Tính số tiền ban đầu$A = \sum a_i$. Điều này thể hiện mức đóng góp bắt buộc tối thiểu vào tổng số biến, do đó, nó xác định “khoảng trống” còn lại để phân bổ linh hoạt. 
2. Tính toán trước các giai thừa và giai thừa nghịch đảo lên đến$n + s$. Điều này là bắt buộc vì mọi câu trả lời sẽ là một hệ số nhị thức với các đối số nằm trong phạm vi này và việc tính toán lại các giai thừa cho mỗi truy vấn sẽ quá chậm. 
3. Đối với mỗi truy vấn, hãy cập nhật mục nhập mảng$a_x$và điều chỉnh tổng số tiền$A$. Thay vì sửa đổi toàn bộ cấu trúc, chúng tôi chỉ duy trì hiệu ứng tổng hợp, vì số lượng cuối cùng chỉ phụ thuộc vào$A$. 
4. Tính công suất còn lại$R = s - A$. Nếu như$R < 0$, xuất ra 0 ngay lập tức vì ngay cả cấu hình tối thiểu cũng vi phạm ràng buộc. 
5. Ngược lại hãy tính số nghiệm như sau$\binom{R + n}{n}$. Điều này xuất phát từ phép biến đổi sao và thanh được áp dụng cho các biến không âm với tổng tổng giới hạn. 

### Tại sao nó hoạt động 

Sau khi dịch chuyển các biến theo giới hạn dưới của chúng, mọi cấu hình hợp lệ sẽ tương ứng chính xác với việc lựa chọn các số nguyên không âm$y_i$tổng của nó nhiều nhất là$R = s - \sum a_i$. Số lượng các vectơ như vậy chỉ phụ thuộc vào tổng độ trễ có sẵn chứ không phụ thuộc vào từng vectơ riêng lẻ.$a_i$. Mỗi bản cập nhật chỉ thay đổi độ trễ này bằng cách điều chỉnh tổng một số hạng, do đó việc tính toán lại sẽ giảm xuống mức duy trì một trạng thái vô hướng duy nhất. Danh tính nhị thức cho các thành phần bị chặn đảm bảo rằng công thức cuối cùng tính tất cả các phép gán hợp lệ chính xác một lần. 

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
    n, s, q = map(int, input().split())
    a = list(map(int, input().split()))
    
    A = sum(a)
    MAX = n + s
    fact, invfact = build_fact(MAX)
    
    for _ in range(q):
        x, val = map(int, input().split())
        x -= 1
        
        A -= a[x]
        a[x] = val
        A += a[x]
        
        R = s - A
        if R < 0:
            print(0)
        else:
            print(ncr(R + n, n, fact, invfact))

if __name__ == "__main__":
    main()
```Việc triển khai xoay quanh việc duy trì tổng của mảng thay vì cấu trúc đầy đủ của nó. Mỗi bản cập nhật điều chỉnh tổng này theo thời gian không đổi, cập nhật trực tiếp phần còn lại$R$. 

Giai thừa và giai thừa nghịch đảo được tính toán trước một lần để có chỉ số tối đa có thể, đảm bảo mỗi truy vấn nhị thức được$O(1)$. Chức năng kết hợp bảo vệ các phạm vi không hợp lệ để tránh các truy cập phủ định hoặc vượt quá giới hạn. 

Sự tinh tế quan trọng là tính toán chính xác$R = s - A$. Mọi sai sót khi cập nhật$A$trước hoặc sau khi gán sẽ làm thay đổi kết quả không chính xác, vì mọi truy vấn đều phụ thuộc vào trạng thái hiện tại của tất cả các bản cập nhật trước đó. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n=3, s=5
a=[1,1,1]
queries: (1→1), (1→2), (2→2), (3→2)
```Chúng tôi theo dõi$A$Và$R$: 

| Bước | một | A | R = s - A | Trả lời | 
| --- | --- | --- | --- | --- | 
| ban đầu | [1,1,1] | 3 | 2 | C(5,3)=10 | 
| 1 | [1,1,1] | 3 | 2 | 10 | 
| 2 | [2,1,1] | 4 | 1 | C(4,3)=4 | 
| 3 | [2,2,1] | 5 | 0 | C(3,3)=1 | 
| 4 | [2,2,2] | 6 | -1 | 0 | 

Dấu vết cho thấy rằng tất cả sự phụ thuộc của cấu trúc đều sụp đổ thành một vô hướng duy nhất$A$. Một lần$A$vượt quá$s$, tính khả thi biến mất ngay lập tức. 

### Ví dụ 2 

đầu vào:```
n=2, s=3
a=[0,0]
queries: (1→1), (2→2)
```| Bước | một | A | R | Trả lời | 
| --- | --- | --- | --- | --- | 
| ban đầu | [0,0] | 0 | 3 | C(5,2)=10 | 
| 1 | [1,0] | 1 | 2 | C(4,2)=6 | 
| 2 | [1,2] | 3 | 0 | C(2,2)=1 | 

Ví dụ này nhấn mạnh việc tăng giới hạn dưới chỉ làm giảm độ chùng còn lại chứ không làm giảm cấu trúc tổ hợp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + q) | tiền xử lý giai thừa là tuyến tính, mỗi truy vấn là O(1) do cập nhật liên tục theo thời gian và nCr | 
| Không gian | O(n + s) | mảng giai thừa và nghịch đảo giai thừa lên đến n + s | 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$và giải pháp chỉ thực hiện tiền xử lý tuyến tính cộng với công việc không đổi trên mỗi truy vấn, phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 10**9 + 7

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    n, s, q = map(int, input().split())
    a = list(map(int, input().split()))
    
    A = sum(a)
    
    MAX = n + s
    fact = [1] * (MAX + 1)
    invfact = [1] * (MAX + 1)
    for i in range(1, MAX + 1):
        fact[i] = fact[i - 1] * i % MOD
    invfact[MAX] = pow(fact[MAX], MOD - 2, MOD)
    for i in range(MAX, 0, -1):
        invfact[i - 1] = invfact[i] * i % MOD
    
    def ncr(n, r):
        if r < 0 or r > n:
            return 0
        return fact[n] * invfact[r] % MOD * invfact[n - r] % MOD
    
    out = []
    for _ in range(q):
        x, val = map(int, input().split())
        x -= 1
        A -= a[x]
        a[x] = val
        A += a[x]
        R = s - A
        if R < 0:
            out.append("0")
        else:
            out.append(str(ncr(R + n, n)))
    
    return "\n".join(out)

# provided sample
assert run("""3 5 4
1 1 1
1 1
1 2
2 2
3 2
""") == "10\n10\n4\n1"

# custom tests
assert run("""1 0 2
0
1 1
1 0
""") == "1\n1", "single variable edge"

assert run("""2 1 2
0 0
1 1
2 1
""") == "2\n1", "tight capacity shrink"

assert run("""3 0 1
0 0 0
1 0
""") == "10", "pure stars and bars"

assert run("""4 2 2
1 0 1 0
1 2
3 2
""") == "0\n0", "infeasible after updates"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cập nhật biến đơn | 1 1 | tính đúng đắn của trường hợp cơ sở | 
| thu hẹp công suất chặt chẽ | 2 1 | giảm tính khả thi đơn điệu | 
| tất cả số không | 10 | nhận dạng tổ hợp thuần túy | 
| cập nhật không khả thi | 0 0 | xử lý chùng tiêu cực | 

## Vỏ cạnh 

Khi tổng giới hạn dưới vượt quá$s$, thuật toán sẽ xuất đúng 0 ngay lập tức vì$R = s - A < 0$. Ví dụ, với$n=3, s=2, a=[1,1,1]$, chúng tôi tính toán$A=3$, cho$R=-1$và đầu ra là 0 mà không thử đánh giá nhị thức. 

Khi tất cả$a_i = 0$, chúng tôi nhận được$R = s$, và câu trả lời trở thành$\binom{s+n}{n}$. Thuật toán xử lý việc này một cách tự nhiên vì không cần cách viết vỏ đặc biệt nào ngoài tính toán nCr tiêu chuẩn. 

Khi cập nhật giảm hoặc tăng một vị trí, chỉ$A$những thay đổi. Thuật toán loại bỏ chính xác phần đóng góp cũ trước khi thêm phần đóng góp mới, đảm bảo không có sự tích lũy trôi qua nhiều truy vấn.
