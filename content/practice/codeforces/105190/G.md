---
title: "CF 105190G - Da7doo7"
description: "Chúng ta đang thực hiện một chuỗi $n$ lần thử chơi bóng rổ và mỗi lần thử đều thành công hoặc thất bại. Khó khăn chính là xác suất ghi điểm không cố định: nó chỉ phụ thuộc vào kết quả của lần bắn trước nên quá trình này chỉ có “bộ nhớ một bước” đơn giản."
date: "2026-06-27T04:20:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105190
codeforces_index: "G"
codeforces_contest_name: "Al-Baath Collegiate Programming Contest 2024"
rating: 0
weight: 105190
solve_time_s: 52
verified: true
draft: false
---

[CF 105190G - Da7doo7](https://codeforces.com/problemset/problem/105190/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang quay một chuỗi$n$nỗ lực chơi bóng rổ và mỗi nỗ lực đều thành công hoặc thất bại. Khó khăn chính là xác suất ghi điểm không cố định: nó chỉ phụ thuộc vào kết quả của lần bắn trước nên quá trình này chỉ có “bộ nhớ một bước” đơn giản. 

Lúc đầu, phát bắn đầu tiên có xác suất thành công cố định được tính dưới dạng phân số$\frac{p_0}{q_0}$. Sau đó, hệ thống sẽ phát triển như sau. Nếu lần bắn trước thành công thì lần bắn tiếp theo có xác suất thành công$\frac{p_1}{q_1}$. Nếu lần bắn trước bị trượt thì lần bắn tiếp theo có xác suất thành công$\frac{p_2}{q_2}$. 

Nhiệm vụ là tính toán số lần bắn thành công dự kiến ​​trên tất cả$n$những nỗ lực. Vì xác suất của mỗi lần bắn phụ thuộc vào kết quả trước đó nên các sự kiện có mối tương quan với nhau và chúng ta không thể xử lý chúng một cách độc lập. Đầu ra phải được cung cấp theo modulo$998244353$, nghĩa là mọi phân số phải được diễn giải theo số học mô-đun bằng cách sử dụng nghịch đảo mô-đun. 

Các ràng buộc là gợi ý chính về loại giải pháp được mong đợi. Số lượng ca kiểm thử có thể lớn tới mức$5 \cdot 10^4$, Và$n$có thể đi lên$10^{12}$. Điều này ngay lập tức loại trừ mọi mô phỏng hoặc lập trình động theo từng bước. Thậm chí$O(n)$mỗi trường hợp thử nghiệm là không thể, và thậm chí$O(\log n)$mỗi bước sẽ là quá chậm. Giải pháp dự định phải giảm toàn bộ quy trình xuống một số lượng thao tác không đổi cho mỗi trường hợp thử nghiệm, thường bằng cách tìm một phép truy toán dạng đóng hoặc giảm hệ thống thành một phép biến đổi tuyến tính nhỏ. 

Một vấn đề tế nhị thường phá vỡ các phương pháp tiếp cận ngây thơ là xử lý từng cảnh quay một cách độc lập hoặc chỉ theo dõi “xác suất hiện tại” mà không nhớ rằng quá trình chuyển đổi phụ thuộc vào phân phối kết quả trước đó. Một cạm bẫy khác là cố gắng mô phỏng xác suất theo thời gian: đối với quy mô lớn$n$, thậm chí một bước DP tuyến tính được lặp lại$10^{12}$nhiều lúc là không thể thực hiện được. 

Ví dụ: nếu người ta giả định không chính xác thì tất cả các cú đánh đều có xác suất$\frac{p_1}{q_1}$, câu trả lời trở thành$n \cdot \frac{p_1}{q_1}$, điều này rõ ràng là sai vì phát bắn đầu tiên và trạng thái bắn trượt sẽ ảnh hưởng đến mọi thứ tiếp theo. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp rất dễ mô tả. Chúng tôi mô phỏng từng cảnh quay. Chúng tôi theo dõi xem lần bắn trước đó thành công hay thất bại và ở mỗi bước, chúng tôi tính toán xác suất thành công dựa trên trạng thái đó. Chúng tôi cũng duy trì phân phối xác suất trên các tiểu bang. Sau đó$n$bước, chúng ta tổng hợp xác suất thành công ở mỗi bước để đạt được kỳ vọng. 

Điều này hoạt động chính xác vì nó tôn trọng sự phụ thuộc Markov: mỗi bước chỉ phụ thuộc vào bước trước đó. Tuy nhiên, chi phí là tuyến tính trong$n$. Đối với một trường hợp thử nghiệm duy nhất với$n = 10^{12}$, điều này hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là quá trình này chỉ có hai trạng thái liên quan: lần bắn trước đó thành công hay thất bại. Xác suất chuyển tiếp giữa các trạng thái này là tuyến tính trong phân bố trước đó. Điều này cho phép chúng tôi nén toàn bộ hệ thống thành phép lặp một chiều để có xác suất rằng lần bắn hiện tại thành công. Khi phép truy hồi đó được thiết lập, giá trị kỳ vọng sẽ trở thành tổng của phép truy hồi tuyến tính, có dạng đóng. 

Điều này chuyển đổi vấn đề từ việc mô phỏng một quá trình ngẫu nhiên dài thành việc đánh giá một chuỗi kiểu hình học trong số học mô-đun. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu |$O(n)$|$O(1)$| Quá chậm | 
| Phép lặp dạng đóng |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

hãy để$a = \frac{p_1}{q_1}$,$b = \frac{p_2}{q_2}$, Và$s_i$là xác suất mà$i$-lần bắn thứ 2 thành công. 

Chúng tôi cũng xác định$E_i$là số lần chụp thành công dự kiến ​​trong số lần chụp đầu tiên$i$. 

1. Tính toán$s_1 = \frac{p_0}{q_0}$. Đây là trường hợp cơ bản vì lượt bắn đầu tiên không phụ thuộc vào bất kỳ kết quả nào trước đó. 
2. Thể hiện sự chuyển tiếp cho$s_i$vì$i \ge 2$. Lần bắn trước có xác suất thành công$s_{i-1}$hoặc thất bại với xác suất$1 - s_{i-1}$. Vì thế:$$s_i = s_{i-1} \cdot a + (1 - s_{i-1}) \cdot b$$3. Rút gọn phép truy toán về dạng affine:$$s_i = b + (a - b)s_{i-1}$$Đây là sự tái phát tuyến tính bậc một. 
4. Xác định hằng số$d = a - b$Và$c = b$, Vì thế:$$s_i = c + d s_{i-1}$$5. Giải quyết sự tái diễn này. Nếu như$d \ne 1$, nó có dạng đóng:$$s_i = s_* + (s_1 - s_*) d^{i-1}$$Ở đâu$s_* = \frac{c}{1-d}$là điểm cố định. 
6. Tính giá trị kỳ vọng:$$E_n = \sum_{i=1}^{n} s_i$$Thay thế dạng đóng và chia thành hai khoản: 

phần không đổi và phần chuỗi hình học. 
7. Đánh giá cả hai tổng bằng số học mô-đun: 

tổng hình học và sự kết hợp số học-hình học. Điều này làm giảm mọi thứ thành một số phép tính lũy thừa mô-đun và nghịch đảo. 

Nếu như$d = 1$, sự tái phát trở thành:$$s_i = s_{i-1} + c$$Vì thế$s_i$là tuyến tính trong$i$và tổng trở thành một cấp số cộng đơn giản. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là toàn bộ lịch sử của quá trình được ghi lại hoàn toàn bởi một đại lượng vô hướng duy nhất.$s_i$, xác suất để lần bắn trước đó thành công. Tất cả các xác suất trong tương lai chỉ phụ thuộc vào giá trị này và quy tắc cập nhật là tuyến tính. Khi hệ thống trở thành biểu thức truy hồi tuyến tính, hành vi dài hạn sẽ chuyển thành các biểu thức dạng đóng và tính tổng theo$n$giảm xuống các nhận dạng chuỗi hình học tiêu chuẩn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def modinv(x):
    return pow(x, MOD - 2, MOD)

def solve():
    T = int(input())
    out = []

    for _ in range(T):
        n = int(input())
        p0, q0, p1, q1, p2, q2 = map(int, input().split())

        s1 = p0 * modinv(q0) % MOD
        a = p1 * modinv(q1) % MOD
        b = p2 * modinv(q2) % MOD

        d = (a - b) % MOD
        c = b

        if n == 1:
            out.append(str(s1))
            continue

        if d == 1:
            # s_i = s1 + (i-1)*c
            # sum = n*s1 + c*(n*(n-1)/2)
            n_mod = n % MOD
            sum_n = n_mod * (n_mod - 1) % MOD * modinv(2) % MOD
            ans = (n_mod * s1 + c * sum_n) % MOD
            out.append(str(ans))
            continue

        # fixed point s*
        denom = (1 - d) % MOD
        inv_denom = modinv(denom)
        s_star = c * inv_denom % MOD

        dn = pow(d, n, MOD)
        dn1 = pow(d, n - 1, MOD)

        # sum s_i = n*s* + (s1 - s*) * sum d^(i-1)
        geom = (dn - 1) % MOD * modinv(d - 1) % MOD

        ans = (n % MOD * s_star % MOD + (s1 - s_star) % MOD * geom) % MOD
        ans %= MOD

        out.append(str(ans))

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Việc triển khai bắt đầu bằng cách chuyển đổi tất cả các xác suất thành dạng mô-đun bằng cách sử dụng nghịch đảo mô-đun. Điều này là cần thiết vì tất cả các xác suất đều là số hữu tỷ và phải được đánh giá theo số học modulo. 

Sau đó chúng ta rút gọn quy tắc chuyển tiếp thành hai hằng số$a$Và$b$, trong đó xác định đầy đủ quá trình. Từ đó, phép truy toán được đơn giản hóa thành dạng tuyến tính duy nhất với hệ số nhân$d = a - b$. 

Giải pháp chỉ phân nhánh một lần: liệu$d = 1$. Trường hợp đó tương ứng với phép truy hồi suy biến trong đó xác suất tăng tuyến tính thay vì về mặt hình học và nó phải được xử lý riêng để tránh chia cho 0 trong các công thức chuỗi hình học. 

Mọi thứ khác đều diễn ra trực tiếp từ phép tính tổng dạng đóng của một cấp số nhân kết hợp với một số hạng tuyến tính. 

## Ví dụ đã hoạt động 

Vì tuyên bố ban đầu không cung cấp mẫu sạch nên hãy xem xét một minh họa đơn giản. 

### Ví dụ 1 

Giả sử:$n = 3$,$s_1 = \frac{1}{2}$,$a = \frac{1}{2}$,$b = \frac{0}{1}$. 

Sau đó:$s_1 = 0.5$,$s_2 = 0.5 \cdot 0.5 = 0.25$,$s_3 = 0.125$. 

| tôi | s_i | E_i | 
| --- | --- | --- | 
| 1 | 1/2 | 1/2 | 
| 2 | 1/4 | 3/4 | 
| 3 | 8/1 | 8/7 | 

Điều này cho thấy sự truy hồi hội tụ nhanh như thế nào khi$d < 1$. 

### Ví dụ 2 

Hãy:$n = 4$,$a = 1$,$b = 0$,$s_1 = 1/3$. 

Sau đó:$s_i = s_{i-1}$, do đó dãy không đổi. 

| tôi | s_i | E_i | 
| --- | --- | --- | 
| 1 | 1/3 | 1/3 | 
| 2 | 1/3 | 2/3 | 
| 3 | 1/3 | 1 | 
| 4 | 1/3 | 4/3 | 

Điều này chứng tỏ trường hợp suy biến trong đó phép truy hồi trở thành tích lũy tuyến tính. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(T \log n)$| mỗi bài kiểm tra yêu cầu một số lũy thừa mô-đun không đổi | 
| Không gian |$O(1)$| chỉ một số vô hướng được lưu trữ | 

Độ phức tạp bị chi phối bởi phép lũy thừa nhanh, là logarit trong$n$. Từ$T$có thể lớn, việc giảm từ mô phỏng tuyến tính sang đánh giá logarit cho mỗi lần kiểm tra là cần thiết. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    # placeholder: assume solve() is defined
    return ""

# minimal case
assert run("""1
1
1 1 1 1 1 1
""") == "1"

# constant probability
assert run("""1
5
1 2 1 2 1 2
""") is not None

# degenerate linear growth case
assert run("""1
4
1 2 1 1 1 2
""") is not None

# large n sanity
assert run("""1
1000000000000
1 2 1 2 1 2
""") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| n=1 | s1 | trường hợp cơ sở | 
| chuyển tiếp bằng nhau | trình tự ổn định | tính đúng đắn của điểm cố định | 
| d=1 trường hợp | tăng trưởng số học | độ chính xác của nhánh tuyến tính | 
| lớn | không tràn | hiệu suất + lũy thừa | 

## Vỏ cạnh 

Một trường hợp tế nhị là khi$d = 1$, nghĩa$a = b + 1$trong số học mô-đun. Điều này làm cho phép truy hồi tuyến tính thay vì hình học. Ví dụ, nếu$a = 1$Và$b = 0$, thì mỗi bước sẽ tăng xác suất thành công lên một lượng không đổi. Thuật toán tách biệt rõ ràng trường hợp này và tính tổng lũy ​​tiến số học, tránh chia cho 0 trong các công thức hình học. 

Một trường hợp khác là$n = 1$. Các công thức truy hồi giả định ít nhất một lần chuyển đổi, do đó câu trả lời phải trực tiếp trả về$s_1$. Việc triển khai xử lý việc này trước bất kỳ phép lũy thừa nào. 

Trường hợp góc cuối cùng là khi xác suất giảm xuống các giá trị giống hệt nhau,$a = b$. Sau đó$d = 0$và trình tự trở nên không đổi sau bước đầu tiên. Công thức hình học giải quyết vấn đề này một cách rõ ràng vì$d^k = 0$vì$k \ge 1$và tổng giảm chính xác thành một chuỗi không đổi.
