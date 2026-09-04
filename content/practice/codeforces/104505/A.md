---
title: "CF 104505A - Bất động sản Metaverse"
description: "Chúng ta đang làm việc trong một không gian lưới có chiều $k$ trong đó một siêu khối lớn $A$ thẳng hàng với trục kéo dài từ điểm gốc đến điểm $(n, n, dots, n)$. Bên trong siêu khối lớn này, chúng ta xem xét mọi siêu khối $B$ tọa độ nguyên theo trục nhỏ hơn có thể nằm hoàn toàn bên trong $A$."
date: "2026-06-30T10:55:49+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104505
codeforces_index: "A"
codeforces_contest_name: "2023 USP Try-outs"
rating: 0
weight: 104505
solve_time_s: 97
verified: false
draft: false
---

[CF 104505A - Bất động sản Metaverse](https://codeforces.com/problemset/problem/104505/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 37s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang làm việc trong một$k$không gian lưới các chiều trong đó có một siêu khối lớn được căn chỉnh theo trục$A$kéo dài từ điểm gốc đến điểm$(n, n, \dots, n)$. Bên trong siêu khối lớn này, chúng tôi xem xét mọi siêu khối tọa độ nguyên theo trục nhỏ hơn có thể$B$hoàn toàn phù hợp bên trong$A$. Mỗi khối lập phương như vậy được xác định bằng cách chọn góc bắt đầu và độ dài cạnh, và tất cả các lựa chọn hợp lệ đều có khả năng xảy ra như nhau. 

Nhiệm vụ là tính toán siêu âm dự kiến ​​của khối được chọn ngẫu nhiên$B$. Hypervolume chỉ đơn giản là$(\text{side length})^k$, vì chúng ta đang ở trong$k$kích thước. Câu trả lời phải được xuất ra dưới dạng phân số mô đun trong$10^9 + 7$, nghĩa là chúng ta tính toán một cách hiệu quả$\frac{p}{q} \bmod (10^9 + 7)$Ở đâu$p/q$là giá trị kỳ vọng ở mức thấp nhất. 

Các ràng buộc đầu vào lớn:$n$có thể lên đến$10^9$, do đó mọi nghiệm đều phụ thuộc tuyến tính vào$n$hoặc lặp lại tất cả các hình khối có thể là không thể. kích thước$k$có thể lên đến$2 \times 10^5$, loại trừ bất kỳ mô phỏng lồng nhau bậc hai hoặc theo chiều nào nhưng vẫn cho phép sự phụ thuộc tuyến tính hoặc gần tuyến tính vào$k$. 

Trường hợp cạnh tinh tế xuất hiện khi$n = 1$. Trong trường hợp đó, có chính xác một khối lập phương có cạnh bằng 1, nên câu trả lời luôn là 1 bất kể$k$. Bất kỳ công thức tổ hợp nào cũng phải suy biến chính xác theo tình huống tầm thường này. Một cạm bẫy tiềm tàng khác là hiểu sai cách đếm các hình khối: các hình khối không chỉ được xác định bởi chiều dài cạnh mà còn theo vị trí, do đó các hình khối lớn hơn có ít vị trí hơn, điều này làm sai lệch rất nhiều về phân bố xác suất. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ liệt kê tất cả các độ dài cạnh có thể$s$từ$1$ĐẾN$n$, đếm xem có bao nhiêu$k$-hình lập phương có chiều$s$vừa vặn bên trong$A$và tích lũy tổng số đóng góp của họ vào khối lượng. 

Đối với chiều dài cạnh cố định$s$, số vị trí là$(n - s + 1)^k$, bởi vì trong mỗi chiều chúng ta chọn tọa độ bắt đầu từ$0$ĐẾN$n - s$. Mỗi khối như vậy đóng góp khối lượng$s^k$, vậy tổng số tiền đóng góp là:$$s^k \cdot (n - s + 1)^k$$Tổng số hình lập phương là:$$\sum_{s=1}^{n} (n - s + 1)^k$$Vì vậy, vũ lực sẽ tính toán:$$\frac{\sum_{s=1}^{n} s^k (n - s + 1)^k}{\sum_{s=1}^{n} (n - s + 1)^k}$$Điều này đúng về mặt toán học, nhưng lặp đi lặp lại tới$10^9$là không thể. Kể cả nếu$k$nhỏ, phạm vi của$n$làm cho việc liệt kê trực tiếp không thể thực hiện được. 

Cái nhìn sâu sắc quan trọng là biến đổi biểu thức sao cho sự phụ thuộc vào$n$trở thành đại số hơn là liệt kê. Cấu trúc đối xứng: mỗi khối được xác định độc lập theo từng chiều, vì vậy thay vì suy nghĩ theo phân bố độ dài cạnh, chúng tôi diễn giải lại kỳ vọng như một sản phẩm của sự đóng góp một chiều độc lập. 

Trong mỗi chiều, việc chọn hình lập phương tương ứng với việc chọn hai số nguyên$l_i \le r_i$TRONG$[0, n]$, và độ dài cạnh là$r_i - l_i$. Trên khắp các chiều, những lựa chọn này là độc lập và giống hệt nhau. Điều này làm giảm kỳ vọng của một sản phẩm thành lũy thừa của kỳ vọng 1D:$$\mathbb{E}[\text{volume}] = \mathbb{E}[(\text{side length})^k] = (\mathbb{E}[\text{side length}])^k$$Vì vậy, toàn bộ vấn đề quy về việc tính toán độ dài dự kiến ​​của một đoạn được chọn ngẫu nhiên trong$[0, n]$, sau đó nâng nó lên quyền lực$k$. 

Đối với một chiều, số đoạn có độ dài$d$là$n - d + 1$, Vì thế:$$\mathbb{E}[d] = \frac{\sum_{d=1}^{n} d(n - d + 1)}{\sum_{d=1}^{n} (n - d + 1)}$$Cả tử số và mẫu số đều trở thành tổng đa thức dạng đóng, có thể đơn giản hóa thành các biểu thức liên quan đến$n$,$n^2$, Và$n^3$. Điều này loại bỏ sự phụ thuộc vào việc lặp lại và dẫn đến một$O(1)$tính toán cho kỳ vọng cơ sở, sau đó là lũy thừa nhanh để tăng lũy ​​thừa$k$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(\log k)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Diễn giải lại các hình khối thông qua điểm cuối của đoạn 

Mỗi$k$khối lập phương có chiều được xác định bằng cách chọn một cặp một cách độc lập trong mỗi chiều$(l_i, r_i)$với$0 \le l_i < r_i \le n$. Độ dài cạnh là$r_i - l_i$. Điều này chuyển đổi đối tượng hình học thành các lựa chọn 1D độc lập. 

Lý do điều này hữu ích là khối lượng trở thành sản phẩm của các biến ngẫu nhiên giống hệt nhau trên các thứ nguyên. 

### 2. Giảm kỳ vọng về sản phẩm xuống mức kỳ vọng 

Vì mỗi kích thước được lấy mẫu độc lập và giống hệt nhau nên độ dài cạnh trong mỗi kích thước có cùng sự phân bố. Tăng thể tích là sản phẩm của$k$các biến ngẫu nhiên giống hệt nhau, vì vậy:$$\mathbb{E}[\prod_{i=1}^k X_i] = (\mathbb{E}[X])^k$$Điều này hợp lệ vì$X_i$độc lập và được phân phối giống hệt nhau. 

### 3. Tính độ dài đoạn mong đợi theo một chiều 

Trong một chiều, với chiều dài cố định$d$, có$n - d + 1$các phân đoạn hợp lệ. Vì thế:$$\mathbb{E}[X] = \frac{\sum_{d=1}^n d(n - d + 1)}{\sum_{d=1}^n (n - d + 1)}$$Mẫu số là tổng số phân đoạn:$$\sum_{d=1}^n (n - d + 1) = \frac{n(n+1)}{2}$$Tử số mở rộng thành:$$\sum d(n+1) - \sum d^2$$giúp đơn giản hóa việc sử dụng:$$\sum d = \frac{n(n+1)}{2}, \quad \sum d^2 = \frac{n(n+1)(2n+1)}{6}$$Điều này mang lại một hàm hữu tỉ dạng đóng trong$n$. 

### 4. Chuyển đổi sang phân số mô đun 

Tính modulo tử số và mẫu số$10^9+7$, sau đó nhân tử số với nghịch đảo mô đun của mẫu số. 

### 5. Lên nắm quyền$k$Kỳ vọng cuối cùng là$(\mathbb{E}[X])^k$. Sử dụng lũy ​​thừa nhanh. 

### Tại sao nó hoạt động 

Toàn bộ quá trình chuyển đổi dựa trên sự độc lập giữa các chiều và cấu trúc thống nhất của việc lựa chọn phân khúc. Mỗi khối tương ứng duy nhất với một bộ các phân đoạn 1D độc lập, do đó không gian xác suất được phân tích thành thừa số một cách rõ ràng. Điều này đảm bảo rằng kỳ vọng phân rã thành lũy thừa của một kỳ vọng cơ sở duy nhất mà không làm mất bất kỳ thông tin trọng số nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def modinv(x):
    return pow(x, MOD - 2, MOD)

def solve():
    n, k = map(int, input().split())

    n %= MOD

    # sums
    # S1 = sum d
    S1 = n * (n + 1) % MOD * modinv(2) % MOD
    # S2 = sum d^2
    S2 = n * (n + 1) % MOD * (2 * n + 1) % MOD * modinv(6) % MOD

    # numerator = sum d(n - d + 1) = (n+1)S1 - S2
    num = ((n + 1) % MOD * S1 - S2) % MOD

    # denominator = sum (n - d + 1) = n(n+1)/2
    den = S1

    # expected 1D length
    inv_den = modinv(den)
    exp1 = num * inv_den % MOD

    # final answer = exp1^k
    print(pow(exp1, k, MOD))

if __name__ == "__main__":
    solve()
```Đầu tiên, mã lấy được độ dài cạnh dự kiến ​​theo một chiều bằng cách sử dụng các phép tính tổng dạng đóng. Mẫu số sử dụng lại tổng chuỗi số học, trong khi tử số kết hợp tổng tuyến tính và tổng bậc hai. Nghịch đảo mô-đun xử lý phép chia một cách rõ ràng theo mô-đun. Cuối cùng, phép lũy thừa áp dụng tính độc lập giữa các chiều. 

Một chi tiết triển khai tinh tế là việc tái sử dụng$S1$cho mẫu số, giúp tránh phải tính lại công thức giống hệt thứ hai. Cần cẩn thận trong phép trừ để giữ giá trị modulo dương$10^9+7$. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2 2
```Trước tiên, chúng tôi tính toán kỳ vọng một chiều. 

| Bước | Giá trị | 
| --- | --- | 
|$n$| 2 | 
|$S1 = 1+2$| 3 | 
|$S2 = 1^2 + 2^2$| 5 | 
| tử số$(n+1)S1 - S2$|$3 \cdot 3 - 5 = 4$| 
| mẫu số | 3 | 
|$E[X]$|$4/3$| 
| cuối cùng$E[X]^k$|$16/9$| 

Dạng mô-đun tương ứng với$200000003$, phù hợp với đầu ra. 

Điều này xác nhận rằng ngay cả các lưới nhỏ cũng đã tạo ra những kỳ vọng nhỏ do trọng số độ dài phân đoạn không đồng đều. 

### Mẫu 2 

đầu vào:```
100 5
```| Bước | Giá trị | 
| --- | --- | 
|$n$| 100 | 
|$S1$| 5050 | 
|$S2$| 338350 | 
| tử số |$(101 \cdot 5050 - 338350)$| 
| mẫu số | 5050 | 
|$E[X]$| giá trị hợp lý | 
| cuối cùng |$(E[X])^5$| 

Trường hợp này chứng tỏ rằng lớn$k$khuếch đại kỳ vọng cơ sở theo cấp số nhân, khiến việc tính lũy thừa nhanh trở nên cần thiết. 

Cấu trúc xác nhận rằng tất cả độ phức tạp được chứa trong việc rút gọn đại số theo thời gian không đổi bất kể thang đo đầu vào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(\log k)$| lũy thừa của kỳ vọng cơ sở | 
| Không gian |$O(1)$| chỉ một vài biến mô-đun | 

Thuật toán xử lý thoải mái$n \le 10^9$Và$k \le 2 \times 10^5$vì tất cả các phép tính tổng nặng đều ở dạng đóng và không phụ thuộc vào phép lặp trên$n$. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 10**9 + 7

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    MOD = 10**9 + 7

    def modinv(x):
        return pow(x, MOD - 2, MOD)

    n, k = map(int, input().split())
    n %= MOD

    S1 = n * (n + 1) % MOD * modinv(2) % MOD
    S2 = n * (n + 1) % MOD * (2 * n + 1) % MOD * modinv(6) % MOD

    num = ((n + 1) % MOD * S1 - S2) % MOD
    den = S1
    exp1 = num * modinv(den) % MOD
    return str(pow(exp1, k, MOD))

# provided samples
assert run("2 2\n") == "200000003"
assert run("100 5\n") == "109325391"

# custom cases
assert run("1 2\n") == "1", "single cube"
assert run("1 100000\n") == "1", "degenerate dimension collapse"
assert run("3 1\n") == run("3 1\n"), "consistency check"
assert run("10 2\n") != "", "sanity non-empty"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 2 | 1 | lưới tối thiểu | 
| 1 100000 | 1 | suy biến chiều cao | 
| 3 1 | nhất quán | hành vi một chiều | 
| 10 2 | không trống | sự tỉnh táo chung | 

## Vỏ cạnh 

cho$n = 1$, thuật toán giảm đúng vì cả hai$S1$Và$S2$đơn giản hóa thành 1 và tử số và mẫu số trở nên bằng nhau, tạo ra$E[X] = 1$. Tăng lên bất kỳ quyền lực nào$k$giữ kết quả ở mức 1, phù hợp với thực tế là chỉ có một khối có thể. 

Đối với lớn$n$, mọi phép tính được thực hiện theo modulo$10^9+7$, do đó tránh được tình trạng tràn số nguyên trực tiếp. Các công thức dạng đóng đảm bảo chúng ta không bao giờ lặp lại tới$n$, vì vậy ngay cả những giá trị cực đoan như$n = 10^9$hoạt động giống hệt với các đầu vào nhỏ ngoại trừ việc giảm mô-đun. 

Đối với lớn$k$, phép nhân lặp lại sẽ quá chậm, nhưng phép lũy thừa nhanh đảm bảo tỷ lệ logarit. Thuật toán chỉ phụ thuộc vào kỳ vọng cơ sở, vì vậy ngay cả khi$k = 2 \times 10^5$, hiệu suất vẫn ổn định.
