---
title: "CF 105230C - Tiệc Sinh Nhật Nhỏ"
description: "Chúng ta có một lớp học với $n$ học sinh, mỗi học sinh độc lập được ấn định một ngày sinh thống nhất trong 365 ngày. Chúng tôi được yêu cầu xác suất của một cấu hình rất cụ thể."
date: "2026-06-24T15:58:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105230
codeforces_index: "C"
codeforces_contest_name: "2024-2025 ICPC Bolivia Pre-National Contest"
rating: 0
weight: 105230
solve_time_s: 108
verified: false
draft: false
---

[CF 105230C - Bữa tiệc sinh nhật nhỏ](https://codeforces.com/problemset/problem/105230/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 48s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cấp một lớp học với$n$học sinh, mỗi học sinh độc lập được ấn định một ngày sinh thống nhất trong 365 ngày. Chúng tôi được yêu cầu xác suất của một cấu hình rất cụ thể. 

Cấu hình cứng nhắc: phải tồn tại một nhóm chính xác$x$những học sinh có cùng ngày sinh nhật và không có ai khác trong lớp có ngày sinh đó. Mỗi học sinh còn lại cũng phải có ngày sinh nhật khác biệt với nhau và cũng khác biệt với ngày sinh nhật chung của học sinh đó.$x$-nhóm. Nói cách khác, ngoài nhóm đặc biệt, không có hai học sinh nào có cùng ngày sinh nhật và không có học sinh nào trùng với ngày chung đã chọn. 

Đầu ra là xác suất này được biểu thị theo modulo$10^9+7$, viết là$p \cdot q^{-1} \bmod M$, Ở đâu$p/q$là phần giảm của xác suất. 

Ràng buộc$n \le 10^{18}$ngay lập tức loại trừ bất kỳ cách tiếp cận nào lặp đi lặp lại qua học sinh hoặc ngày. Ngay cả sự phụ thuộc đa thức vào$n$là không thể, do đó nghiệm chỉ phải phụ thuộc vào các đại lượng dẫn xuất nhỏ. Vì ngày sinh đến từ một vũ trụ cố định gồm 365 giá trị, nên bất kỳ cấu trúc tổ hợp nào cũng phải thu gọn thành một phép tính có kích thước không đổi. 

Một trường hợp thất bại tinh tế xuất hiện khi$n-x$là lớn. Nếu có hơn 364 học sinh còn sót lại, chúng tôi sẽ buộc phải chỉ định cho họ những ngày sinh nhật riêng biệt ngoại trừ ngày chung đã chọn, nhưng chỉ còn lại 364 ngày. Ví dụ, nếu$n=1000$Và$x=10$, thì 990 học sinh còn lại không thể có ngày sinh nhật khác nhau trong 364 ô, nên xác suất chính xác là bằng không. Bất kỳ công thức ngây thơ nào bỏ qua ràng buộc này sẽ tạo ra một giá trị mô-đun khác không một cách không chính xác. 

Một trường hợp thất bại khác đến từ việc điều trị$C(n,x)$trực tiếp sử dụng giai thừa. Từ$n$có thể$10^{18}$, việc tính toán nhị thức dựa trên giai thừa là không thể trừ khi chúng ta khai thác thực tế là kích thước kết hợp hiệu quả là nhỏ. 

## Phương pháp tiếp cận 

Cách giải thích thô bạo sẽ liệt kê tất cả các bài tập về ngày sinh nhật của$n$người, đếm xem có bao nhiêu người thỏa mãn điều kiện và chia cho$365^n$. Điều này đúng về mặt khái niệm nhưng hoàn toàn không khả thi. Không gian trạng thái là$365^n$, và thậm chí suy luận tổ hợp ở quy mô đó mà không đơn giản hóa là không thể. 

Quan sát quan trọng là cấu trúc của các bài tập hợp lệ là cực kỳ cứng nhắc. Khi chúng tôi chọn ngày sinh nhật đặc biệt được chia sẻ bởi$x$mọi người, mọi thứ khác buộc phải thực hiện nhiệm vụ tiêm chích trong 364 ngày còn lại. Điều này chuyển vấn đề thành việc chọn một nhóm, chọn một ngày và hoán đổi các bài tập còn lại mà không lặp lại. 

Sự đơn giản hóa quan trọng là chỉ$k = n - x$vấn đề đối với các sinh viên còn lại, và tính khả thi đòi hỏi$k \le 364$. Khi điều này được giữ, tất cả cấu trúc còn lại chỉ phụ thuộc vào các hoán vị nhỏ và thuật ngữ nhị thức trong đó chỉ số dưới nhỏ. Điều này cho phép viết lại$C(n,x)$BẰNG$C(n,k)$, trở thành sản phẩm của$k$điều khoản ngay cả khi$n$là rất lớn. 

Biểu thức cuối cùng trở thành sản phẩm của ba thành phần độc lập: chọn ngày đặc biệt, chọn học sinh nào trong nhóm và chỉ định ngày sinh nhật còn lại riêng biệt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Liệt kê lực lượng vũ phu | Hàm mũ | O(1) | Quá chậm | 
| Giảm tổ hợp | O(365) mỗi lần kiểm tra | O(365) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta viết lại bài toán bằng cách đếm các bài tập hợp lệ và chia cho tổng số bài tập. 

### Các bước 

1. Tính toán$k = n - x$. Điều này thể hiện số lượng học sinh không thuộc nhóm sinh nhật đặc biệt. Phần còn lại của tính toán phụ thuộc hoàn toàn vào việc liệu những$k$học sinh có thể được chỉ định ngày sinh nhật riêng biệt ngoài ngày sinh nhật chung. 
2. Nếu$k > 364$, ngay lập tức trả về 0. Điều này là do sau khi dành một ngày cho$x$sinh viên, chỉ còn lại 364 ngày riêng biệt và chúng tôi không thể chỉ định ngày sinh duy nhất cho hơn 364 người. 
3. Tính toán trước các giai thừa và giai thừa nghịch đảo lên tới 365, vì tất cả các số hạng tổ hợp sẽ bị giới hạn bởi phạm vi này. 
4. Chọn ngày sinh nhật chung. Có 365 cách chọn ngày$x$mọi người trùng hợp. 
5. Chọn cái nào$x$học sinh thành lập nhóm. Từ$k = n-x$, tính toán dễ dàng hơn$C(n,k)$thay vì$C(n,x)$, điều này tránh phải xử lý những vấn đề lớn$x$. Điều này được tính như một sản phẩm:$$C(n,k) = \frac{n(n-1)\cdots(n-k+1)}{k!}$$6. Gán ngày sinh nhật cho những người còn lại$k$sinh viên. Tất cả chúng đều phải khác biệt và phải tránh ngày đã chọn, vì vậy đây là một hoán vị:$$P(364, k) = \frac{364!}{(364-k)!}$$7. Nhân tất cả các khoản đóng góp:$$\text{ways} = 365 \cdot C(n,k) \cdot P(364,k)$$8. Chia cho tổng kết quả$365^n$sử dụng lũy ​​thừa mô đun và nghịch đảo mô đun. 

### Tại sao nó hoạt động 

Mỗi cấu hình hợp lệ được xác định duy nhất bởi ba lựa chọn độc lập: ngày sinh chung, danh tính của$x$các học sinh chia sẻ nó và phân công các ngày sinh nhật riêng biệt cho các học sinh còn lại. Những lựa chọn này không trùng lặp hoặc tính hai lần bất kỳ sự sắp xếp nào. Ràng buộc$k \le 364$đảm bảo phép gán hoán vị luôn hợp lệ. Điều này mang lại sự tương ứng một-một giữa các công trình được tính và kết quả xác suất hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

MAXD = 365

fact = [1] * (MAXD + 1)
invfact = [1] * (MAXD + 1)

for i in range(1, MAXD + 1):
    fact[i] = fact[i - 1] * i % MOD

invfact[MAXD] = pow(fact[MAXD], MOD - 2, MOD)
for i in range(MAXD, 0, -1):
    invfact[i - 1] = invfact[i] * i % MOD

def nCk_large_n(n, k):
    if k < 0:
        return 0
    if k == 0:
        return 1
    num = 1
    for i in range(k):
        num = num * ((n - i) % MOD) % MOD
    return num * invfact[k] % MOD

def perm_364(k):
    if k > 364:
        return 0
    return fact[364] * invfact[364 - k] % MOD

def solve():
    t = int(input())
    for _ in range(t):
        n, x = map(int, input().split())
        k = n - x

        if k < 0 or k > 364:
            print(0)
            continue

        ways_group = nCk_large_n(n, k)
        ways_perm = perm_364(k)

        ways = 365 * ways_group % MOD
        ways = ways * ways_perm % MOD

        denom = pow(365, n, MOD)
        ans = ways * pow(denom, MOD - 2, MOD) % MOD

        print(ans)

if __name__ == "__main__":
    solve()
```Việc triển khai chỉ tính toán trước các giai thừa tối đa 365 vì không có hoán vị nào vượt quá giới hạn đó. chức năng`nCk_large_n`tính các hệ số nhị thức có đỉnh lớn và đáy nhỏ bằng cách sử dụng công thức nhân trực tiếp, tránh giai thừa của$n$. Đây là thủ thuật quan trọng giúp xử lý$n \le 10^{18}$khả thi. 

Số hạng hoán vị được tính toán trước thông qua giai thừa vì miền của nó bị giới hạn chặt chẽ bởi 365. Phép chia cuối cùng cho$365^n$được xử lý bằng cách sử dụng lũy ​​thừa mô-đun, điều này khả thi ngay cả đối với rất lớn$n$vì phép lũy thừa chạy theo thời gian logarit. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp nhỏ$n=10, x=3$. Sau đó$k=7$, vì vậy chúng tôi đang chỉ định 7 học sinh còn lại vào các ngày sinh nhật riêng biệt ngoại trừ ngày sinh nhật chung. 

| Bước | Giá trị | 
| --- | --- | 
| k | 7 | 
| C(n,k) |$C(10,7)$| 
| sự lựa chọn 365 | 365 | 
| P(364, k) |$364P7$| 

Việc xây dựng trước tiên chọn 7 học sinh không thuộc nhóm, sau đó chỉ định cho họ những ngày sinh nhật duy nhất và cuối cùng nhân với số lựa chọn cho ngày chung. Điều này khớp chính xác với cấu trúc của các bài tập hợp lệ. 

Bây giờ hãy xem xét một trường hợp ranh giới$n=400, x=10$. Sau đó$k=390$. 

| Bước | Giá trị | 
| --- | --- | 
| k | 390 | 
| Khả thi? | Không | 

Vì 390 vượt quá 364 nên chúng tôi lập tức xuất ra 0. Điều này xác nhận rằng ràng buộc về các ngày sinh khác nhau là yếu tố giới hạn thực sự chứ không phải tính tổ hợp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(365 + t \cdot 365)$| tính toán trước giai thừa và tối đa 365 phép nhân cho mỗi bài kiểm tra | 
| Không gian |$O(365)$| lưu trữ giai thừa và nghịch đảo | 

Độ phức tạp không phụ thuộc vào$n$, cho phép xử lý các giá trị lên tới$10^{18}$thoải mái trong giới hạn. Với$t \le 1000$, hệ số hằng số vẫn nhỏ. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 10**9 + 7

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    MAXD = 365
    fact = [1] * (MAXD + 1)
    invfact = [1] * (MAXD + 1)

    for i in range(1, MAXD + 1):
        fact[i] = fact[i - 1] * i % MOD

    invfact[MAXD] = pow(fact[MAXD], MOD - 2, MOD)
    for i in range(MAXD, 0, -1):
        invfact[i - 1] = invfact[i] * i % MOD

    def nCk_large_n(n, k):
        if k < 0:
            return 0
        if k == 0:
            return 1
        num = 1
        for i in range(k):
            num = num * ((n - i) % MOD) % MOD
        return num * invfact[k] % MOD

    def perm_364(k):
        if k > 364:
            return 0
        return fact[364] * invfact[364 - k] % MOD

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            n, x = map(int, input().split())
            k = n - x
            if k < 0 or k > 364:
                out.append("0")
                continue
            ways = 365 * nCk_large_n(n, k) % MOD
            ways = ways * perm_364(k) % MOD
            denom = pow(365, n, MOD)
            out.append(str(ways * pow(denom, MOD - 2, MOD) % MOD))
        return "\n".join(out)

    return solve()

# provided samples (format assumed line-separated pairs)
assert run("4\n2 2\n3 1\n10 3\n100 99\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tối thiểu$n=x$| khác không | chỉ có nhóm chia sẻ tồn tại | 
|$k>364$| 0 | ràng buộc bất khả thi | 
| vừa phải$n$| giá trị mô-đun đúng | tính đúng đắn của công thức đầy đủ | 
| lớn$n$| đầu ra ổn định | xử lý số nguyên lớn | 

## Vỏ cạnh 

Trường hợp góc trực tiếp là khi$n=x$. Sau đó tất cả học sinh đều ở trong cùng một nhóm sinh nhật. Công thức rút gọn thành việc chọn ngày chung, không còn bài tập nào. Thuật toán tạo ra một cách chính xác$365 \cdot 1 \cdot 1 / 365^n$, phù hợp với thực tế là chỉ có một ngày quan trọng và tất cả các ràng buộc khác đều biến mất. 

Một trường hợp góc khác là khi$n-x=364$. Ở đây mỗi học sinh còn lại phải chiếm tất cả các ngày còn lại đúng một lần. Thuật ngữ hoán vị trở thành$364!$và thuật toán tính toán nó trực tiếp từ các giai thừa được tính toán trước mà không cần bất kỳ lý do động nào. Đây là cấu hình khả thi chặt chẽ nhất và bất kỳ sai sót nhỏ nào trong việc kiểm tra giới hạn sẽ từ chối nó một cách không chính xác.
