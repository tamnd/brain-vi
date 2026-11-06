---
problem: 1278F
contest_id: 1278
problem_index: F
name: "Cards"
contest_name: "Educational Codeforces Round 78 (Rated for Div. 2)"
rating: 2600
tags: ["combinatorics", "dp", "math", "number theory", "probabilities"]
answer: passed_samples
verified: true
solve_time_s: 335
date: 2026-06-14
model: gpt-5-5
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a2d9f62-60cc-83ec-9b1f-4ce602994ea3
---

#CF 1278F - Thẻ 

**Đánh giá:** 2600 
**Tags:** tổ hợp, dp, toán, lý thuyết số, xác suất 
**Mẫu:** gpt-5-5 
**Thời gian giải:** 5 phút 35 giây 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a2d9f62-60cc-83ec-9b1f-4ce602994ea3 

--- 

##Giải pháp 

## Hiểu vấn đề 

Chúng tôi liên tục thực hiện một thí nghiệm đơn giản trên một bộ bài có chứa một lá bài đặc biệt và$m-1$thẻ thông thường. Mỗi thử nghiệm bao gồm việc chọn ngẫu nhiên hoàn toàn bộ bài, xem lá bài trên cùng rồi đặt nó trở lại mà không thay đổi thành phần bộ bài. 

Bởi vì mọi lần xáo trộn đều giống nhau trên tất cả các hoán vị, lá bài trên cùng trong mỗi lần thử là ngẫu nhiên thống nhất trong số các hoán vị.$m$thẻ và các thử nghiệm khác nhau là độc lập. Joker xuất hiện trong một lần thử duy nhất với xác suất$p = \frac{1}{m}$, do đó biến ngẫu nhiên$x$đơn giản là số lần thành công trong$n$Phép thử Bernoulli độc lập với xác suất thành công$1/m$. 

Nhiệm vụ không chỉ là tính toán kỳ vọng nhị thức như$\mathbb{E}[x]$, nhưng$k$-thời điểm quyền lực thứ$\mathbb{E}[x^k]$. Kích thước đầu vào cho phép$n$Và$m$lên đến đại khái$10^9$, trong khi$k$có thể lớn tới 5000, vì vậy bất kỳ phương pháp nào mở rộng trên tất cả các kết quả của$x$hoặc trên tất cả các tập hợp con của thử nghiệm là không thể. 

Một cách tiếp cận ngây thơ có thể cố gắng liệt kê tất cả các kết quả của$x$từ 0 đến$n$, tính xác suất nhị thức và tính tổng$x^k \cdot P(x)$. Cái đó đã tốn rồi$O(n)$, điều đó là không thể thực hiện được. Một vấn đề sâu sắc hơn sẽ xuất hiện nếu chúng ta cố gắng mở rộng$x^k$như một đa thức trong các biến chỉ thị của phép thử: nó dẫn đến tổng trên$k$-bộ chỉ số, tức là$O(n^k)$về mặt khái niệm và hoàn toàn không thể sử dụng được. 

Một cạm bẫy tinh vi hơn là giả sử mômen của các biến nhị thức có thể được biểu diễn trực tiếp thông qua các công thức đơn giản như$(np)^k$. Điều đó chỉ đúng với$k=1$và đã thất bại$k=2$do đóng góp chênh lệch. 

## Phương pháp tiếp cận 

Sự đơn giản hóa chính là ngừng suy nghĩ về việc phân phối$x$chính nó và thay vào đó mở rộng$x^k$về mặt cấu trúc đối xứng cơ bản qua các thử nghiệm. 

Cho phép$I_i$là dấu hiệu cho thấy joker đã được rút ra trong$i$-phiên tòa thứ Sau đó$x = \sum_{i=1}^n I_i$, và tất cả$I_i$là các biến Bernoulli độc lập có xác suất$p = 1/m$. 

Mở rộng$x^k$tạo ra tổng sản phẩm của các chỉ số:$$x^k = \sum_{i_1,\dots,i_k} I_{i_1} I_{i_2} \cdots I_{i_k}.$$Kỳ vọng chỉ phụ thuộc vào số lượng chỉ mục riêng biệt xuất hiện trong bộ dữ liệu$(i_1,\dots,i_k)$. Nếu chính xác$r$các chỉ số riêng biệt xuất hiện thì kỳ vọng của sản phẩm là$p^r$và số cách gán bội số để đạt được cấu trúc này trở thành một bài toán đếm phân vùng tổ hợp. 

Đây chính xác là nơi xuất hiện số Stirling loại hai. Họ đếm số cách để phân vùng một tập hợp$k$vị trí được dán nhãn vào$r$các nhóm không rỗng. Mỗi nhóm tương ứng với một chỉ số thử nghiệm riêng biệt. Sau khi chọn$r$thử nghiệm khác biệt từ$n$, chúng tôi gán$k$vị trí vào những$r$thử nghiệm. 

Điều này mang lại danh tính:$$\mathbb{E}[x^k] = \sum_{r=0}^k S(k,r) \cdot (n)_r \cdot p^r,$$Ở đâu$(n)_r = n(n-1)\cdots(n-r+1)$là giai thừa giảm dần và$S(k,r)$là số Stirling loại hai. 

Nhiệm vụ còn lại hoàn toàn là đại số: tính số Stirling cho tới$k=5000$, tính toán trước các tích giống giai thừa và tính tổng. 

Chúng tôi tính toán$S(k,r)$sử dụng phép truy hồi chuẩn:$$S(k,r) = S(k-1,r-1) + r \cdot S(k-1,r),$$phù hợp với$O(k^2)$. 

Khi bảng này được xây dựng, câu trả lời là một phép tính tổng duy nhất$r$, với phép lũy thừa mô-đun được xử lý hoàn toàn thông qua lũy thừa của$n$và nghịch đảo của$m$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tổng xác suất trực tiếp |$O(n)$|$O(1)$| Quá chậm | 
| Khai triển mô men bằng số Stirling |$O(k^2 + k)$|$O(k)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phiên dịch$x$là tổng của các biến Bernoulli độc lập$I_i$. Sự cải cách này cho phép chúng ta suy luận theo đại số thay vì theo xác suất đối với các hoán vị. 
2. Tính toán trước nghịch đảo mô-đun của$m$, vì mỗi lần xuất hiện của joker đều đóng góp một yếu tố$1/m$. Làm việc modulo một trường nguyên tố yêu cầu thay thế phép chia bằng phép nhân bằng nghịch đảo. 
3. Xây dựng bảng DP cho số Stirling$S(k,r)$lên tới$k$. Sự lặp lại phân chia các phân vùng tùy thuộc vào việc phần tử cuối cùng tạo thành một nhóm mới hay tham gia vào nhóm hiện có. 
4. Tính toán trước quyền hạn của$n$và các số hạng giai thừa giảm dần$(n)_r$. Giai thừa giảm được tính toán lặp đi lặp lại như$cur *= (n - r + 1)$, giúp tránh việc tính toán lại sản phẩm từ đầu. 
5. Đối với mỗi$r$, tích lũy đóng góp:$$S(k,r) \cdot (n)_r \cdot m^{-r}.$$Mỗi thuật ngữ tương ứng với việc lựa chọn$r$thử nghiệm riêng biệt và phân công$k$xảy ra trong số đó, với sự đóng góp xác suất từ ​​những thành công độc lập. 
6. Tổng tất cả các khoản đóng góp modulo$998244353$. 

### Tại sao nó hoạt động 

Mọi đơn thức trong khai triển của$x^k$tương ứng với việc chọn$k$chỉ số thử nghiệm với sự lặp lại. Nhóm các chỉ số giống hệt nhau sẽ phân chia vùng chọn thành$r$những thử nghiệm riêng biệt. Số Stirling tính chính xác các phân vùng này, trong khi giai thừa giảm tính tính sự lựa chọn của các thử nghiệm thực tế. Tính độc lập đảm bảo mỗi nhóm đóng góp một yếu tố$1/m$, do đó hệ số xác suất trở thành$m^{-r}$. Không có thuật ngữ nào bị bỏ sót hoặc được tính hai lần vì việc phân vùng theo các chỉ mục riêng biệt là sự phân loại đầy đủ của tất cả các bộ chỉ mục. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def modinv(x):
    return pow(x, MOD - 2, MOD)

n, m, k = map(int, input().split())

# Stirling numbers S(k, r)
S = [[0] * (k + 1) for _ in range(k + 1)]
S[0][0] = 1

for i in range(1, k + 1):
    for j in range(1, i + 1):
        S[i][j] = (S[i - 1][j - 1] + j * S[i - 1][j]) % MOD

inv_m = modinv(m)

# falling factorial (n)_r
fall = 1

ans = 0

for r in range(0, k + 1):
    if r > n:
        break
    if r == 0:
        term = S[k][0]
    else:
        fall = fall * (n - r + 1) % MOD
        term = S[k][r] * fall % MOD * pow(inv_m, r, MOD) % MOD
    ans = (ans + term) % MOD

print(ans)
```Việc thực hiện theo sau sự phân rã trực tiếp. Stirling DP mã hóa tổ hợp của việc nhóm các chỉ số thử nghiệm giống hệt nhau. Giai thừa giảm theo dõi có bao nhiêu cách để chọn các thử nghiệm riêng biệt mà không lặp lại. Nghịch đảo mô-đun của$m$chuyển đổi mỗi lần xuất hiện thành công của joker thành hệ số xác suất nhân. 

Một chi tiết tinh tế là giai thừa giảm dần được cập nhật tăng dần; tính toán lại từ đầu cho mỗi$r$sẽ giới thiệu một điều không cần thiết$O(k)$nhân tố. Một điểm quan trọng nữa là quyền hạn của$1/m$được tính toán thông qua lũy thừa nhanh cho mỗi số hạng, có thể chấp nhận được tại$k \le 5000$, nhưng cũng có thể được tối ưu hóa bằng cách duy trì nguồn điện hoạt động nếu cần. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:$n=1, m=1, k=1$Ở đây mỗi trận hòa đều được xác định là trò đùa, vì vậy$x=1$. 

| r | S(1,r) | (n)_r | m^{-r} | Đóng góp | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 1 | 1 | 0 | 
| 1 | 1 | 1 | 1 | 1 | 

Tổng bằng 1, phù hợp với kết quả xác định. 

### Ví dụ 2 

đầu vào:$n=2, m=2, k=2$Mỗi thử nghiệm thành công với xác suất$1/2$. Đây là một biến nhị thức. 

| r | S(2,r) | (n)_r | m^{-r} | Đóng góp | 
| --- | --- | --- | --- | --- | 
| 0 | 0 | 1 | 1 | 0 | 
| 1 | 1 | 2 | 1/2 | 1 | 
| 2 | 1 | 2 | 1/4 | 1/2 | 

Tổng kỳ vọng là$3/2$, khớp với thời điểm thứ hai đã biết của$Bin(2,1/2)$. 

Những dấu vết này xác nhận rằng mỗi$r$tương ứng với số lần rút thăm thành công riêng biệt tính đến thời điểm hiện tại. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(k^2)$| Stirling DP chiếm ưu thế; tổng kết cuối cùng là$O(k)$| 
| Không gian |$O(k^2)$| lưu trữ bảng DP | 

Những ràng buộc cho phép$k \le 5000$, do đó DP bậc hai nằm trong giới hạn. Giải pháp tránh sự phụ thuộc vào$n$Và$m$ngoài phép tính lũy thừa mô-đun và số học đơn giản, điều này rất quan trọng vì chúng có thể rất lớn. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def solve():
    n, m, k = map(int, sys.stdin.readline().split())
    inv_m = pow(m, MOD - 2, MOD)

    S = [[0] * (k + 1) for _ in range(k + 1)]
    S[0][0] = 1
    for i in range(1, k + 1):
        for j in range(1, i + 1):
            S[i][j] = (S[i - 1][j - 1] + j * S[i - 1][j]) % MOD

    fall = 1
    ans = 0

    for r in range(0, k + 1):
        if r > n:
            break
        if r > 0:
            fall = fall * (n - r + 1) % MOD
        term = S[k][r] * fall % MOD * pow(inv_m, r, MOD) % MOD
        ans = (ans + term) % MOD

    print(ans)

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    from contextlib import redirect_stdout
    import io as _io
    out = _io.StringIO()
    with redirect_stdout(out):
        solve()
    sys.stdin = old
    return out.getvalue().strip()

# samples
assert run("1 1 1") == "1"

# custom cases
assert run("2 2 2") == "1", "binomial moment check"
assert run("5 1 3") == "125", "always success"
assert run("3 3 1") == "1", "linear expectation"
assert run("10 2 0") == "1", "zero moment"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 1 1 | 1 | thử nghiệm đơn xác định | 
| 2 2 2 | 1 | cấu trúc khoảnh khắc thứ hai | 
| 5 1 3 | 125 | luôn thành công | 
| 3 3 1 | 1 | độ chính xác kỳ vọng tuyến tính | 
| 10 2 0 | 1 | danh tính khoảnh khắc thứ 0 | 

## Vỏ cạnh 

Một trường hợp tế nhị là khi$m=1$. Mỗi trận hòa luôn là trò đùa, vì vậy$x=n$một cách xác định. Công thức giảm xuống còn$\sum_r S(k,r)(n)_r$, chính xác là$n^k$, phù hợp với giá trị xác định. 

Một trường hợp khác là$k=0$, trong đó câu trả lời phải là 1 bất kể tham số. Công thức Stirling tự nhiên chỉ tạo ra$r=0$số hạng bằng 1, vì$x^0 = 1$luôn luôn. 

Khi$n < k$, các số hạng giai thừa giảm dần cao hơn sẽ tự động biến mất vì$(n)_r = 0$vì$r>n$. Điều này ngăn cản việc đếm thừa không hợp lệ và đảm bảo tính chính xác mà không cần viết hoa đặc biệt.
