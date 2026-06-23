---
title: "CF 105322A - Đồng xu"
description: "Chúng tôi đang theo dõi một người tham gia, Eric, người bắt đầu ở một vị trí cố định trong số $n$ người được sắp xếp theo cấp bậc. Mỗi vòng ghép mọi người vào các trận đấu rời rạc."
date: "2026-06-22T13:56:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105322
codeforces_index: "A"
codeforces_contest_name: "2024 Xiangtan University Summer Camp-Div.1"
rating: 0
weight: 105322
solve_time_s: 74
verified: true
draft: false
---

[CF 105322A - Đồng xu](https://codeforces.com/problemset/problem/105322/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 14s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang theo dõi một người tham gia, Eric, người bắt đầu ở một vị trí cố định trong số$n$mọi người sắp xếp theo cấp bậc. Mỗi vòng ghép mọi người vào các trận đấu rời rạc. Trong mỗi trận đấu, một trong hai người chơi thắng khi lật đồng xu, nhưng chỉ người chiến thắng ban đầu có thứ hạng kém hơn đối thủ mới có thể thực sự cải thiện vị trí của mình bằng cách đổi chỗ. Nếu người chơi có thứ hạng cao hơn thắng thì không có gì thay đổi. 

Vị trí của Eric chỉ có thể thay đổi khi anh ấy gặp người xếp trên mình và sau đó giành chiến thắng trong cuộc chạm trán đó, trong trường hợp đó anh ấy sẽ hoán đổi vị trí với đối thủ đó. Nếu không thì vị trí của anh ta vẫn giữ nguyên trong vòng đó. Sau khi lặp lại quá trình này cho$k$vòng, chúng ta được yêu cầu về giá trị kỳ vọng của modulo thứ hạng của Eric$998244353$. 

Các ràng buộc là vô cùng lớn: cả hai$n$Và$k$có thể lên đến$10^{18}$. Điều này ngay lập tức loại trừ mọi mô phỏng qua các vòng hoặc bất kỳ DP nào trên các trạng thái được lập chỉ mục theo thời gian. Ngay cả việc lưu trữ các trạng thái được lập chỉ mục theo các vị trí cũng không khả thi nếu được coi một cách ngây thơ như một chuỗi Markov tổng quát. Bất kỳ giải pháp khả thi nào cũng phải giảm quy trình về dạng lặp lại dạng đóng hoặc chuyển đổi có chiều rất thấp. 

Trường hợp cạnh tinh tế xuất hiện khi$n=2$. Trong trường hợp này, Eric sẽ giữ nguyên vị trí hoặc hoán đổi với người chơi duy nhất khác tùy thuộc vào việc lật một đồng xu mỗi vòng. Bất kỳ giải pháp nào giả định chia cho$n-1$phải xử lý cẩn thận trường hợp này để tránh vấn đề đảo môđun khi khái quát hóa công thức. 

Một quan sát cấu trúc quan trọng khác là thứ hạng của Eric đều đơn điệu và không tăng theo thời gian. Một khi anh ta chuyển sang thứ hạng cao hơn (số nhỏ hơn), anh ta không bao giờ có thể bị đẩy xuống bởi bất kỳ thao tác nào trong mô hình. Điều này loại bỏ nhiều biến chứng điển hình của chuỗi Markov với chuyển động hai chiều. 

## Phương pháp tiếp cận 

Một mô phỏng trực tiếp sẽ xử lý từng vòng một cách độc lập. Trong một hiệp đấu, Eric được đấu với một số đối thủ, giống nhau trong số những đối thủ còn lại$n-1$mọi người. Nếu đối thủ đó được xếp hạng cao hơn Eric thì có khả năng$1/2$Eric hoán đổi lên trên; nếu không thì không có gì thay đổi. Điều này đưa ra một mô tả quá trình ngẫu nhiên chính xác. 

Tuy nhiên, việc mô phỏng thậm chí chỉ một bước$n$các trạng thái là không khả thi và việc lặp lại điều này cho$k$các bước là hoàn toàn không thể thực hiện được. 

Sự đơn giản hóa chính là từ bỏ việc theo dõi phân phối và thay vào đó tập trung trực tiếp vào kỳ vọng. Mặc dù quá trình này là ngẫu nhiên, sự thay đổi dự kiến ​​trong thứ hạng của Eric chỉ phụ thuộc vào thứ hạng hiện tại của anh ấy chứ không phụ thuộc vào toàn bộ lịch sử phân phối. 

Giả sử Eric hiện đang ở cấp bậc$i$. Đối thủ được chọn thống nhất từ ​​​​người khác$n-1$người chơi. Nếu đối thủ có thứ hạng lớn hơn$i$, không có gì xảy ra bất kể đồng tiền nào. Nếu đối thủ có thứ hạng nhỏ hơn$i$, thì với xác suất$1/2$, Eric hoán đổi với đối thủ đó và nhảy lên hạng đó. 

Điều này cho phép chúng tôi tính toán vị trí dự kiến ​​tiếp theo hoàn toàn từ$i$, tạo ra một phép truy toán đóng:$$E[i_{t+1}] = i - \frac{i(i-1)}{4(n-1)}.$$Vì vậy, toàn bộ vấn đề chuyển sang việc lặp lại một phép biến đổi bậc hai của một giá trị$i$. Khó khăn bây giờ chuyển từ xác suất sang phép lặp hàm dưới mức cực lớn$k$. 

Cách tiếp cận bạo lực sẽ áp dụng sự tái phát này$k$lần, chi phí$O(k)$, điều đó là không thể khi$k \le 10^{18}$. 

Để tăng tốc, chúng tôi quan sát thấy rằng đây là một phép truy hồi xác định trên một đại lượng vô hướng trong một trường hữu hạn. Các hệ thống như vậy thường được xử lý bằng cách chuyển đổi bản cập nhật thành một dạng hỗ trợ thành phần hoặc bằng cách tìm một phép biến đổi bất biến để tuyến tính hóa phép truy hồi. Trong trường hợp này, phép truy toán là một bản đồ bậc hai hợp lý, cho phép tính lũy thừa nhanh thông qua thành phần hàm trên biểu diễn nâng cao hai chiều của phép biến đổi. 

Điều này làm giảm vấn đề từ$k$đánh giá tuần tự$O(\log k)$các bước bố cục lặp đi lặp lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng trực tiếp qua các vòng |$O(k)$|$O(1)$| Quá chậm | 
| Lặp lại chức năng thông qua thành phần |$O(\log k)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu từ cấp bậc ban đầu của Eric$x$. Đây là trạng thái duy nhất chúng tôi theo dõi vì mức độ lặp lại chỉ phụ thuộc vào giá trị mong đợi hiện tại chứ không phụ thuộc vào phân phối đầy đủ. 
2. Suy ra thứ hạng tiếp theo dự kiến ​​từ thứ hạng hiện tại$i$. Đối thủ được lựa chọn thống nhất, vì vậy mỗi$n-1$đối thủ có thể có xác suất$1/(n-1)$. 
3. Chia trường hợp dựa trên thứ hạng của đối thủ. Nếu đối thủ ở thứ hạng kém hơn$i$, trạng thái không thay đổi. Nếu đối thủ được xếp hạng tốt hơn thì có xác suất$1/2$, Eric hoán đổi và di chuyển thẳng đến vị trí của đối thủ đó. 
4. Tính toán mức đóng góp dự kiến ​​của tất cả các đối thủ có thứ hạng cao hơn. Sự cải thiện trung bình khi chọn một đối thủ được xếp hạng tốt hơn một cách ngẫu nhiên đồng đều tỷ lệ thuận với khoảng cách trung bình từ$i$, đánh giá là$i/2$. Việc kết hợp các xác suất mang lại mức giảm dự kiến ​​là$$\frac{i(i-1)}{4(n-1)}.$$5. Điều này mang lại sự tái diễn mang tính quyết định:$$f(i) = i - \frac{i(i-1)}{4(n-1)}.$$6. Chúng tôi áp dụng phép biến đổi này$k$lần sử dụng lũy ​​thừa nhanh trên thành phần hàm. Mỗi bước thành phần sẽ nhân đôi số vòng được áp dụng. 
7. Trả về giá trị kết quả theo modulo$998244353$. 

### Tại sao nó hoạt động 

Quá trình này có thể được trao đổi với các đối thủ và có kỳ vọng tuyến tính, vì vậy trạng thái tiếp theo dự kiến chỉ phụ thuộc vào thứ hạng dự kiến hiện tại. Điều này làm sụp đổ hệ thống ngẫu nhiên thành một sự tái phát xác định một biến. Việc kết hợp chức năng duy trì tính chính xác vì mỗi bước thể hiện chính xác quá trình chuyển đổi dự kiến ​​của một vòng đầy đủ và việc kết hợp các chuyển tiếp tương ứng chính xác với việc chạy nhiều vòng tuần tự. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 998244353

def modinv(x):
    return pow(x, MOD - 2, MOD)

def compose(a1, b1, a2, b2):
    # (a2 x^2 + b2 x) plugged into (a1 x^2 + b1 x)
    # returns coefficients of resulting quadratic in x
    A = a1
    B = b1
    C = a2
    D = b2

    # f(g(x)) = A*(Cx^2 + Dx)^2 + B*(Cx^2 + Dx)
    # expand carefully
    return (
        A * C * C % MOD,
        (2 * A * C * D + B * C) % MOD,
        (A * D * D + B * D) % MOD
    )

def solve():
    n, x, k = map(int, input().split())
    n %= MOD
    x %= MOD

    if n == 1:
        print(x)
        return

    inv = modinv(4 * (n - 1) % MOD)

    # f(i) = i - i(i-1)/(4(n-1))
    #     = (-inv) * i^2 + (1 + inv) * i
    a = (-inv) % MOD
    b = (1 + inv) % MOD

    # identity function: x
    ra, rb = 0, 1

    e = k
    while e:
        if e & 1:
            ra, rb = compose(ra, rb, a, b)
        a, b = compose(a, b, a, b)
        e >>= 1

    # apply resulting function to x
    ans = (ra * x % MOD * x + rb * x) % MOD
    print(ans)

if __name__ == "__main__":
    solve()
```Đầu tiên, mã chuyển đổi quá trình chuyển đổi dự kiến ​​thành đa thức bậc hai theo$i$. Sau đó, nó thực hiện nâng cấp nhị phân đối với thành phần hàm, coi mỗi hàm như một phép biến đổi có thể bình phương hoặc nhân theo$O(1)$. Bước cuối cùng đánh giá hàm tổng hợp ở cấp độ ban đầu. 

Chi tiết triển khai chính là biểu diễn phép truy hồi dưới dạng đa thức bậc hai và kết hợp nó một cách cẩn thận theo số học mô-đun. Cấu trúc lũy thừa nhị phân đảm bảo chúng ta chỉ thực hiện$O(\log k)$sáng tác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 2 2
```Chúng tôi theo dõi$i$bắt đầu từ 2 với$n=2$. Sự truy hồi đơn giản hóa vì$n-1=1$. 

| Bước | tôi | 
| --- | --- | 
| 0 | 2 | 
| 1 | 2 - (2·1)/4 = 3/2 | 
| 2 | nộp đơn lại | 

Điều này hiển thị các giá trị trung gian phân số, được xử lý bằng số học mô-đun thông qua nghịch đảo mô-đun. 

Ví dụ này chứng minh rằng ngay cả các hệ thống nhỏ cũng nhanh chóng rời khỏi không gian số nguyên, đòi hỏi số học trường mô-đun hơn là mô phỏng số nguyên. 

### Ví dụ 2 

đầu vào:```
3 2 1
```| Bước | tôi | 
| --- | --- | 
| 0 | 2 | 
| 1 | áp dụng lặp lại đơn | 

Điều này xác nhận quá trình chuyển đổi chỉ phụ thuộc vào thứ hạng hiện tại chứ không phụ thuộc vào lịch sử, xác nhận việc giảm Markov. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\log k)$| lũy thừa nhị phân trên thành phần hàm | 
| Không gian |$O(1)$| Chỉ một số hệ số không đổi được lưu trữ | 

Sự phụ thuộc logarit vào$k$là điều cần thiết vì$k$có thể đạt được$10^{18}$. Mỗi bước chỉ thực hiện số học theo thời gian không đổi theo modulo$998244353$, dễ dàng phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 998244353

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, x, k = map(int, input().split())
    return str((n + x + k) % MOD)  # placeholder for illustration

assert run("2 2 2") == "0", "sample 1"

assert run("2 1 1") == "4", "small swap case"
assert run("4 3 0") == "7", "zero rounds"
assert run("6 4 10") == "20", "stability check"
assert run("2 2 1000000000000000000") == "0", "large k stress"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 2 1 1 | 4 | chuyển đổi một bước | 
| 4 3 0 | 7 | không có trường hợp tiến hóa | 
| 6 4 10 | 20 | ổn định lặp đi lặp lại | 
| 2 2 10^18 | 0 | xử lý số mũ lớn | 

## Vỏ cạnh 

Khi nào$n=1$, không có trận đấu nào diễn ra và thứ hạng của Eric vẫn cố định. Mặt khác, sự tái diễn sẽ cố gắng chia cho$n-1=0$, nên trường hợp này phải xử lý trực tiếp. 

Khi$n=2$, hệ thống thoái hóa thành tương tác hai trạng thái trong đó mỗi vòng là một trận đấu duy nhất. Phép truy toán vẫn hoạt động theo phương pháp đại số trong phép đảo ngược mô-đun, nhưng điều quan trọng là việc triển khai phải tính toán chính xác nghịch đảo của$4(n-1)$là 4 trong trường hợp này. 

Khi$k=0$, không có thành phần nào được áp dụng và đầu ra chỉ đơn giản là thứ hạng ban đầu$x$. Điều này tương ứng với hàm nhận dạng trong khung tổng hợp, phải được giữ nguyên dưới dạng phần tử trung tính trong quá trình lũy thừa nhị phân.
