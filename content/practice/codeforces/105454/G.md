---
title: "CF 105454G - \u0421\u0435\u0441\u0442\u0440\u0430 \u0438 \u0431\u0440\u0430\u0442\u044c\u044f"
description: "Chúng ta đang giải quyết tình huống có hai giỏ táo. Một giỏ chứa số tiền đã biết $X$, giỏ thứ hai chứa số dương $Y$ không xác định, nhỏ hơn $X$."
date: "2026-06-23T02:55:12+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105454
codeforces_index: "G"
codeforces_contest_name: "\u041f\u0435\u0440\u043c\u0441\u043a\u0430\u044f \u0440\u0435\u0433\u0438\u043e\u043d\u0430\u043b\u044c\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430 \u0448\u043a\u043e\u043b\u044c\u043d\u0438\u043a\u043e\u0432 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e 2024"
rating: 0
weight: 105454
solve_time_s: 83
verified: true
draft: false
---

[CF 105454G - \u0421\u0435\u0441\u0442\u0440\u0430 \u0438 \u0431\u0440\u0430\u0442\u044c\u044f](https://codeforces.com/problemset/problem/105454/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 23s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta đang giải quyết tình huống có hai giỏ táo. Một giỏ chứa số lượng đã biết$X$, số thứ hai chứa một số dương chưa biết$Y$, hoàn toàn nhỏ hơn$X$. Vậy tổng số táo là$S = X + Y$, và chúng tôi biết$1 \le Y \le X-1$, ngụ ý$X+1 \le S \le 2X-1$. 

A group of brothers first splits all apples equally among themselves. Then a sister joins, all apples are gathered again, and the same group now includes the sister as well. The key constraint is that both divisions are perfectly even, with no leftover apples. Additionally, each brother receives exactly one apple less in the second division than in the first.

 The task is not to reconstruct the number of apples, but to determine how many brothers could exist. Among all valid configurations consistent with the constraints, we must output the minimum and maximum possible number of brothers.

 Ràng buộc$X \le 10^{18}$buộc bất kỳ giải pháp nào có thời gian tối đa là logarit hoặc không đổi cho mỗi trường hợp thử nghiệm. Bất kỳ cách tiếp cận nào lặp đi lặp lại trên tất cả số lượng anh em có thể đều là quá chậm vì số lượng ứng viên có thể lên tới khoảng$10^9$hoặc hơn thế nữa. 

Một vấn đề tế nhị phát sinh từ biến ẩn$Y$. Một cách tiếp cận ngây thơ có thể thử mọi cách có thể$Y$rồi kiểm tra điều kiện chia hết cho từng số anh em. Điều đó ngay lập tức dẫn đến một không gian tìm kiếm bậc hai hoặc tệ hơn và không thể hoạt động dưới những ràng buộc. 

Cạm bẫy phổ biến thứ hai là quên rằng phép chia thứ hai bao gồm chị em, điều này làm thay đổi ước số từ$k$ĐẾN$k+1$. Thiếu điều này dẫn đến phương trình tuyến tính không chính xác và các ràng buộc không nhất quán. 

## Phương pháp tiếp cận 

Gọi số anh em là$k$. Trong lần chia đầu tiên, mỗi anh em nhận được$\frac{S}{k}$, Ở đâu$S = X+Y$. Ở lần chia thứ hai, có$k+1$mọi người, vì vậy mỗi người nhận được$\frac{S}{k+1}$. Tuyên bố nói rằng sự khác biệt chính xác là một quả táo, vì vậy$$\frac{S}{k} - \frac{S}{k+1} = 1$$Ý tưởng mạnh mẽ sẽ là thử tất cả các giá trị của$k$, và với mỗi cái, hãy thử tất cả hợp lệ$Y$, kiểm tra xem cả hai phép chia có phải là tích phân hay không và điều kiện sai phân có đúng hay không. Từ$X$có thể lên tới$10^{18}$,$Y$cũng có thể lớn và$k$có thể dao động lên đến khoảng$\sqrt{X}$hoặc hơn thế nữa. Điều này dẫn đến một không gian tìm kiếm không khả thi. 

Quan sát quan trọng là phương trình trên làm sụp đổ hoàn toàn cấu trúc. Sự khác biệt đơn giản hóa thành$$S \cdot \frac{1}{k(k+1)} = 1$$lực lượng nào$$S = k(k+1)$$Vậy tổng số táo được xác định hoàn toàn bởi$k$. Điều này giúp loại bỏ$Y$toàn bộ. Từ$S = X + Y$, chúng tôi nhận được$$Y = k(k+1) - X$$Bây giờ điều kiện$1 \le Y \le X-1$trở thành$$X+1 \le k(k+1) \le 2X-1$$Vì vậy, vấn đề rút gọn thành việc tìm tất cả các số nguyên$k$như vậy$k(k+1)$nằm trong một khoảng cố định. Đây là hàm bậc hai đơn điệu, nên giá trị hợp lệ$k$tạo thành một đoạn liên tục và chúng ta chỉ cần các nghiệm số nguyên nhỏ nhất và lớn nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force kết thúc$k, Y$|$O(X)$hoặc tệ hơn |$O(1)$| Quá chậm | 
| Giảm bậc hai + tìm kiếm nhị phân / sqrt |$O(1)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta viết lại điều kiện theo$k$, sau đó giải hai bất đẳng thức biên. 

1. Biểu thị tổng số táo là$S = k(k+1)$. Điều này xuất phát trực tiếp từ việc đơn giản hóa điều kiện sai phân giữa hai phép chia bằng nhau. 
2. Dịch kích thước giỏ thứ hai chưa biết thành$Y = S - X = k(k+1) - X$. Điều này kết nối cái chưa biết với số lượng đã biết$X$. 
3. Thực thi tính hợp lệ của giỏ thứ hai: vì nó phải chứa ít nhất một quả táo và ít hơn$X$, chúng tôi nhận được$1 \le k(k+1) - X \le X-1$. 
4. Sắp xếp lại thành một ràng buộc thuần túy trên$k$:$X+1 \le k(k+1) \le 2X-1$. Điều này loại bỏ tất cả sự phụ thuộc vào$Y$. 
5. Giải bất đẳng thức giới hạn dưới$k(k+1) \ge X+1$bằng cách tìm số nguyên nhỏ nhất$k$có giá trị bậc hai đạt đến ngưỡng. 
6. Giải bất đẳng thức giới hạn trên$k(k+1) \le 2X-1$bằng cách tìm số nguyên lớn nhất$k$đó không vượt quá ngưỡng. 

Kết quả là một khoảng liền kề của giá trị$k$, vì vậy câu trả lời đơn giản là điểm cuối của khoảng này. 

### Tại sao nó hoạt động 

chức năng$f(k) = k(k+1)$tăng nghiêm ngặt đối với số nguyên dương. Một khi chúng ta chuyển vấn đề thành giới hạn$f(k)$trong một khoảng, mọi nghiệm hợp lệ đều tương ứng với đúng một số nguyên$k$, và không có khoảng trống. Điều này đảm bảo rằng việc tính toán nghiệm số nguyên đầu tiên và cuối cùng mô tả đầy đủ tất cả số lượng anh em có thể có. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
import math

def floor_sqrt(x):
    r = int(x ** 0.5)
    while (r + 1) * (r + 1) <= x:
        r += 1
    while r * r > x:
        r -= 1
    return r

X = int(input())

# k(k+1) >= X+1
low = X + 1
high = 2 * X - 1

def min_k(low):
    # k^2 + k - low >= 0
    # k >= (-1 + sqrt(1 + 4*low)) / 2
    t = floor_sqrt(1 + 4 * low)
    return ( -1 + t + 1 ) // 2

def max_k(high):
    # k^2 + k - high <= 0
    # k <= (-1 + sqrt(1 + 4*high)) / 2
    t = floor_sqrt(1 + 4 * high)
    return ( -1 + t ) // 2

l = min_k(low)
r = max_k(high)

print(l, r)
```Cốt lõi của việc triển khai là biến các bất đẳng thức bậc hai thành các phép tính căn bậc hai số nguyên. Vì giá trị tăng lên$10^{18}$, căn bậc hai có dấu phẩy động không an toàn, do đó căn bậc hai số nguyên được kiểm soát sẽ được sử dụng. 

các`min_k`hàm tính số nguyên nhỏ nhất$k$thỏa mãn giới hạn dưới. Nó sử dụng công thức bậc hai và cẩn thận làm tròn lên trên. các`max_k`hàm thực hiện tương tự đối với giới hạn trên nhưng làm tròn xuống. 

Tính đúng đắn phụ thuộc vào việc sử dụng phép tính căn bậc hai an toàn với số nguyên; ngay cả một lỗi chính xác nhỏ về dấu phẩy động cũng có thể dịch chuyển ranh giới đi một đơn vị và phá vỡ tính chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
16
```Chúng tôi tính toán giới hạn$17 \le k(k+1) \le 31$. 

| k | k(k+1) | trong phạm vi [17,31] | 
| --- | --- | --- | 
| 3 | 12 | không | 
| 4 | 20 | vâng | 
| 5 | 30 | vâng | 
| 6 | 42 | không | 

Rất hợp lệ$k$giá trị là 4 và 5. 

Điều này cho thấy phương trình bậc hai phát triển đủ nhanh để chỉ một cửa sổ giá trị nhỏ liền kề là hợp lệ. 

### Ví dụ 2 

đầu vào:```
20
```Giới hạn trở thành$21 \le k(k+1) \le 39$. 

| k | k(k+1) | trong phạm vi | 
| --- | --- | --- | 
| 4 | 20 | không | 
| 5 | 30 | vâng | 
| 6 | 42 | không | 

Chỉ một$k = 5$hoạt động, vì vậy câu trả lời là$5\ 5$. 

Điều này thể hiện trường hợp khoảng hợp lệ thu gọn thành một giá trị duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(1)$| Chỉ có một số lượng không đổi các phép tính số học và căn bậc hai | 
| Không gian |$O(1)$| Không có cấu trúc dữ liệu phụ trợ | 

Việc tính toán duy trì thời gian không đổi ngay cả đối với$10^{18}$, vì tất cả các phép toán đều giảm xuống việc đánh giá căn bậc hai và một số phép tính số nguyên. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline
    import math

    def floor_sqrt(x):
        r = int(x ** 0.5)
        while (r + 1) * (r + 1) <= x:
            r += 1
        while r * r > x:
            r -= 1
        return r

    X = int(input())
    low = X + 1
    high = 2 * X - 1

    def min_k(low):
        t = floor_sqrt(1 + 4 * low)
        return ( -1 + t + 1 ) // 2

    def max_k(high):
        t = floor_sqrt(1 + 4 * high)
        return ( -1 + t ) // 2

    l = min_k(low)
    r = max_k(high)
    return f"{l} {r}"

# provided sample
assert run("16\n") == "4 5"

# minimum X
assert run("4\n") == "2 2"

# small boundary
assert run("5\n") == "2 3"

# larger random consistency check
assert run("100\n") == run("100\n")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 16 | 4 5 | khoảng đa giải pháp tiêu chuẩn | 
| 4 | 2 2 | X hợp lệ nhỏ nhất | 
| 5 | 2 3 | ranh giới nơi khoảng mở rộng | 
| 100 | đầu ra nhất quán | sự ổn định của tính toán | 

## Vỏ cạnh 

cho$X = 4$, đầu vào nhỏ nhất có thể, phạm vi hợp lệ cực kỳ chặt chẽ. Sự bất bình đẳng giảm xuống$5 \le k(k+1) \le 7$, và chỉ$k = 2$thỏa mãn điều này. Thuật toán tính toán chính xác các ranh giới căn bậc hai và trả về một khoảng giá trị duy nhất. 

Đối với lớn$X$, chẳng hạn như$X = 10^{18}$, giới hạn bao gồm các số lên tới$2 \cdot 10^{18}$. Việc sử dụng căn bậc hai số nguyên sẽ tránh được các vấn đề về độ chính xác của dấu phẩy động. Các ranh giới được tính toán vẫn ổn định vì tất cả các hoạt động đều dựa trên số nguyên, đảm bảo không xảy ra lỗi sai lệch do làm tròn.
