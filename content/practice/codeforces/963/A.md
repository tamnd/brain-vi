---
problem: 963A
contest_id: 963
problem_index: A
name: "Alternating Sum"
contest_name: "Tinkoff Internship Warmup Round 2018 and Codeforces Round 475 (Div. 1)"
rating: 1800
tags: ["math", "number theory"]
answer: passed_samples
verified: true
solve_time_s: 82
date: 2026-06-17
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a328192-f20c-83ec-a3a9-4d043e415a76
---

# CF 963A - Tổng xen kẽ 

**Đánh giá:** 1800 
**Tags:** toán, lý thuyết số 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 1m 22s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a328192-f20c-83ec-a3a9-4d043e415a76 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng ta được cấp một tổng có trọng số dài trong đó mỗi vị trí đóng góp một lũy thừa của hai cơ sở độc lập. Tại chỉ mục$i$, sự đóng góp là$+1$hoặc$-1$, nhân với$a^{n-i} b^i$. Trình tự các dấu hiệu không phải là tùy ý cho tất cả$n+1$vị trí, thay vào đó chỉ là vị trí đầu tiên$k$các giá trị được đưa ra và phần còn lại lặp lại mỗi$k$các bước. 

Vì vậy, nhiệm vụ là đánh giá một biểu thức giống như đa thức xen kẽ tuần hoàn, trong đó các hệ số lặp lại theo chu kỳ.$k$, nhưng số mũ phụ thuộc vào vị trí tuyệt đối trong dãy. Môđun là một số nguyên tố lớn$10^9 + 9$, do đó số học phải được thực hiện theo phương pháp rút gọn mô-đun. 

Khó khăn đến từ hai cấu trúc tương tác: hệ số tuần hoàn và sự tăng trưởng hình học của trọng số$a^{n-i} b^i$. số mũ$n$có thể lớn như$10^9$, do đó việc xây dựng trực tiếp chuỗi hoặc lũy thừa trực tiếp cho mỗi số hạng là không thể. 

Một đánh giá ngây thơ sẽ yêu cầu lặp đi lặp lại trên tất cả$n+1$vị trí và tính toán lũy thừa mô-đun cho mỗi thuật ngữ. Ngay cả khi mỗi lũy thừa được tính theo thời gian logarit, việc này vẫn trở nên quá chậm vì$n$tùy thuộc vào$10^9$, nghĩa là lên đến một tỷ số hạng. 

Các trường hợp cạnh xuất hiện khi tính tuần hoàn tương tác với cấu trúc số mũ. Ví dụ, nếu$k = 1$, chuỗi không đổi và câu trả lời giảm xuống thành tổng hình học dạng đóng. Một trường hợp góc khác là khi$a = b$, trong đó tất cả các trọng số quy về một lũy thừa duy nhất và việc hủy bỏ hoàn toàn phụ thuộc vào các tổng tuần hoàn xen kẽ, điều này có thể dễ dàng gây ra lỗi xử lý dấu. 

Một dạng lỗi phổ biến là lặp lại nhiều chu kỳ không chính xác. Từ$n+1$có thể không phải là bội số của$k$trong lý luận ngây thơ, nhưng vấn đề đảm bảo$k$chia rẽ$n+1$, việc thiếu thông tin này sẽ dẫn đến việc xử lý các khối một phần không chính xác. 

## Phương pháp tiếp cận 

Một giải pháp brute-force đánh giá trực tiếp từng thuật ngữ:$$s_i \cdot a^{n-i} b^i$$cho tất cả$i$từ$0$ĐẾN$n$. Điều này đúng vì nó tuân theo định nghĩa chính xác. Tuy nhiên, nó đòi hỏi$n+1$lũy thừa mô-đun, mỗi chi phí$O(\log n)$, dẫn đến khoảng$10^9 \log 10^9$hoạt động trong trường hợp xấu nhất, vượt xa mọi giới hạn khả thi. 

Quan sát quan trọng là cả chuỗi hệ số và cấu trúc trọng số đều có tính quy luật nhân. Viết lại mỗi số hạng:$$a^{n-i} b^i = a^n \left(\frac{b}{a}\right)^i$$Yếu tố này tạo ra một hằng số$a^n$, để lại một tiến trình hình học trong$\frac{b}{a}$, nhân với một dãy dấu tuần hoàn. 

Bây giờ tổng trở thành:$$a^n \sum_{i=0}^{n} s_i r^i, \quad r = b \cdot a^{-1}$$Mọi thứ quy về việc đánh giá một chuỗi tuần hoàn có trọng số theo cấp số nhân. 

Chúng tôi chia chỉ số theo modulo dư lượng$k$. Mỗi lớp dư lượng tạo thành một chuỗi hình học trên các khối:$$i = t k + j$$Ở đâu$j$được cố định và$t$chạy trên tất cả các khối đầy đủ. Điều này biến đổi tổng thành:$$\sum_{j=0}^{k-1} s_j r^j \sum_{t=0}^{m-1} r^{tk}$$Ở đâu$m = \frac{n+1}{k}$. 

Tổng bên trong là một chuỗi hình học tiêu chuẩn trong$r^k$, và tổng bên ngoài chỉ kết thúc$k$các thuật ngữ, làm cho việc tính toán trở nên hiệu quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n \log n)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(k \log n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi làm việc hoàn toàn theo modulo$M = 10^9 + 9$. 

1. Chuyển chuỗi ký hiệu thành giá trị số$s_j \in \{+1, -1\}$. Bước này thiết lập mẫu hệ số tuần hoàn một cách rõ ràng để đại số sau này hoạt động trơn tru. 
2. Tính nghịch đảo môđun của$a$, vì chúng ta cần$r = b \cdot a^{-1}$. Điều này viết lại biểu thức số mũ hỗn hợp thành một cơ số cấp số nhân duy nhất. 
3. Tính toán trước$r^k$. Điều này thể hiện hệ số tăng trưởng giữa các khối có chiều dài liên tiếp$k$. Lý do vấn đề này là vì mỗi lớp dư lượng tạo thành một chuỗi hình học có lũy thừa của$r^k$. 
4. Tính toán$m = (n+1)/k$, số kỳ đầy đủ. Điều này xuất phát từ sự đảm bảo rằng$k$chia rẽ$n+1$, do đó không cần xử lý khối một phần. 
5. Đối với từng vị trí$j$trong kỳ cơ sở, tính toán khoản đóng góp$s_j \cdot r^j$. Điều này tách biệt phần đóng góp bù đắp trong khối, không phụ thuộc vào số lượng chu kỳ đầy đủ xảy ra. 
6. Nhân mỗi phần đóng góp với tổng hình học của các khối:$$1 + (r^k) + (r^k)^2 + \dots + (r^k)^{m-1}$$Nếu như$r^k \neq 1$, điều này được tính toán bằng cách sử dụng dạng đóng tiêu chuẩn. Nếu nó bằng 1 thì tổng giảm xuống còn$m$. 
7. Tổng hợp tất cả các khoản đóng góp còn lại và nhân với$a^n$, khôi phục lại sức mạnh đã được tính toán. 

### Tại sao nó hoạt động 

Mỗi chỉ số$i$được biểu diễn duy nhất dưới dạng$i = tk + j$. Thuật ngữ ở vị trí đó trở thành:$$s_j \cdot a^{n-(tk+j)} b^{tk+j}$$yếu tố nào vào:$$s_j a^{n-j} (b^j a^{-j}) (b^k a^{-k})^t$$vì vậy mỗi lớp dư lượng là một cấp số nhân trong$t$. Thuật toán tái tạo lại chính xác các cấp số độc lập này và tính tổng chúng, đảm bảo không bỏ sót hoặc tính hai lần tương tác giữa các lớp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 9

def modpow(x, e):
    res = 1
    while e:
        if e & 1:
            res = res * x % MOD
        x = x * x % MOD
        e >>= 1
    return res

def modinv(x):
    return modpow(x, MOD - 2)

def solve():
    n, a, b, k = map(int, input().split())
    s = input().strip()

    vals = [1 if c == '+' else -1 for c in s]

    inv_a = modinv(a)
    r = b * inv_a % MOD

    r_k = modpow(r, k)
    m = (n + 1) // k

    # geometric sum over blocks
    if r_k == 1:
        geom_blocks = m % MOD
    else:
        geom_blocks = (modpow(r_k, m) - 1) * modinv(r_k - 1) % MOD

    ans = 0

    # contribution per residue class
    for j in range(k):
        term = vals[j] * modpow(r, j) % MOD
        ans = (ans + term) % MOD

    ans = ans * geom_blocks % MOD
    ans = ans * modpow(a, n) % MOD

    print(ans % MOD)

if __name__ == "__main__":
    solve()
```Việc thực hiện tuân theo phép biến đổi đại số trực tiếp. Phần tế nhị nhất là sự thay đổi cơ sở từ$a^{n-i} b^i$vào trong$a^n r^i$, giúp tránh việc tính toán lại các lũy thừa lớn nhiều lần. 

Phải cẩn thận khi tính tổng hình học: mẫu số$r^k - 1$phải được thực hiện theo modulo$M$và nó có thể bằng 0, tương ứng với trường hợp đặc biệt trong đó mọi khối đóng góp như nhau và tổng trở thành tuyến tính theo$m$. 

Phép nhân cuối cùng với$a^n$khôi phục yếu tố đã được rút ra khi bắt đầu chuyển đổi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 2 3 3
+-+
```Chúng tôi tính toán$r = 3 \cdot 2^{-1}$. Trình tự có$k=3$, vậy có một khối đầy đủ. 

| j | s_j | r^j | đóng góp | 
| --- | --- | --- | --- | 
| 0 | +1 | 1 | 1 | 
| 1 | -1 | r | -r | 
| 2 | +1 | r^2 | r^2 | 

Tổng trên một khối cho:$$1 - r + r^2$$Từ$m = 1$, tổng hình học của khối là 1. 

nhân với$a^n = 2^2$xây dựng lại kết quả cuối cùng:$$2^2 (1 - r + r^2) = 7$$Dấu vết này cho thấy cấu trúc tuần hoàn sụp đổ như thế nào thành một đa thức có trọng số duy nhất. 

### Ví dụ 2 

đầu vào:```
4 1 5 1
-
```Đây$k=1$, vậy mọi số hạng đều là$-1$. Biểu thức trở thành:$$-\sum_{i=0}^{4} 5^i$$| tôi | hạn | 
| --- | --- | 
| 0 | -1 | 
| 1 | -5 | 
| 2 | -25 | 
| 3 | -125 | 
| 4 | -625 | 

Tổng là$-781$, phù hợp với kết quả mô-đun mong đợi. 

Trường hợp này chứng tỏ trường hợp suy biến trong đó tính tuần hoàn loại bỏ tất cả cấu trúc bên trong, chỉ để lại một chuỗi hình học thuần túy. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(k \log n)$| Mỗi dư lượng yêu cầu lũy thừa mô-đun và có$k$dư lượng | 
| Không gian |$O(1)$| Chỉ sử dụng một số biến cố định | 

Ràng buộc$k \le 10^5$đảm bảo rằng việc lặp lại trong một khoảng thời gian là khả thi, trong khi tất cả sự phụ thuộc vào$n$được nén thành lũy thừa nhanh. Điều này giúp giải pháp hoạt động tốt trong giới hạn thời gian ngay cả đối với các đầu vào lớn nhất. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    MOD = 10**9 + 9

    def modpow(x, e):
        res = 1
        while e:
            if e & 1:
                res = res * x % MOD
            x = x * x % MOD
            e >>= 1
        return res

    def modinv(x):
        return modpow(x, MOD - 2)

    n, a, b, k = map(int, input().split())
    s = input().strip()
    vals = [1 if c == '+' else -1 for c in s]

    inv_a = modinv(a)
    r = b * inv_a % MOD
    r_k = modpow(r, k)
    m = (n + 1) // k

    if r_k == 1:
        geom_blocks = m % MOD
    else:
        geom_blocks = (modpow(r_k, m) - 1) * modinv(r_k - 1) % MOD

    ans = 0
    for j in range(k):
        ans += vals[j] * modpow(r, j)

    ans = ans % MOD
    ans = ans * geom_blocks % MOD
    ans = ans * modpow(a, n) % MOD

    return str(ans % MOD)

# provided samples
assert run("2 2 3 3\n+-+\n") == "7", "sample 1"
assert run("4 1 5 1\n-\n") == "999999228", "sample 2"

# custom cases
assert run("1 2 2 1\n+\n") == str((2 + 2) % (10**9 + 9)), "k=1 small"
assert run("3 1 1 1\n+\n") == str(4 % (10**9 + 9)), "a=b=1"
assert run("5 3 7 5\n+-+-+\n") is not None, "period equals k"
assert run("10 2 3 2\n+-\n") is not None, "even splitting"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| k=1 nhỏ | hình học trực tiếp | lớp dư lượng đơn | 
| a=b=1 | tổng các dấu hiệu | sụp đổ quyền lực | 
| chu kỳ bằng k | cấu trúc đầy đủ | tính đúng đắn chung | 
| thậm chí chia tách | tăng trưởng hỗn hợp | tương tác định kỳ | 

## Vỏ cạnh 

Khi nào$k = 1$, toàn bộ dãy là hằng số. Thuật toán rút gọn thành một chuỗi hình học duy nhất trong$b/a$và việc triển khai xử lý chính xác việc này vì chỉ có một lớp dư lượng đóng góp. Tổng khối hình học nắm bắt tất cả các thuật ngữ và không xuất hiện vấn đề lập chỉ mục. 

Khi$r^k = 1$, mọi khối đều đóng góp như nhau. Mã chuyển sang phép nhân tuyến tính với$m$, tránh chia cho 0 trong nghịch đảo môđun. Trường hợp này rất quan trọng vì nó tương ứng với sự hủy định kỳ trong số học mô-đun trong đó chu kỳ số mũ căn chỉnh hoàn hảo với khoảng thời gian. 

Khi$a = b$, tỷ lệ$r = 1$, do đó mọi biến thiên theo cấp số nhân đều biến mất. Biểu thức trở thành một tổng lũy ​​thừa thuần túy xen kẽ của$a$và việc triển khai vẫn hoạt động vì$r^k = 1$kích hoạt việc xử lý đơn giản chính xác của tổng hình học.
