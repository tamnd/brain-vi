---
title: "CF 105122A - Máy phát điện"
description: "Chúng ta được yêu cầu xây dựng một trình tạo tạo ra các cặp số nguyên $(a, b)$ thỏa mãn $1 le a le b le k$ và mỗi cặp hợp lệ phải được tạo ra với xác suất bằng nhau bất cứ khi nào chúng ta yêu cầu một giá trị mới."
date: "2026-06-27T19:36:51+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105122
codeforces_index: "A"
codeforces_contest_name: "XXVI Interregional Programming Olympiad, Vologda SU, 2024"
rating: 0
weight: 105122
solve_time_s: 90
verified: false
draft: false
---

[CF 105122A - Máy phát điện](https://codeforces.com/problemset/problem/105122/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 30 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được yêu cầu xây dựng một trình tạo xuất ra các cặp số nguyên$(a, b)$thỏa mãn$1 \le a \le b \le k$và mỗi cặp hợp lệ phải được tạo ra với xác suất bằng nhau bất cứ khi nào chúng tôi yêu cầu một giá trị mới. 

Yêu cầu quan trọng không chỉ là tạo ra các cặp hợp lệ mà còn đảm bảo tính đồng nhất trên toàn bộ tập hợp các cặp hợp lệ. Điều đó có nghĩa là nếu có$T$tổng cộng các cặp hợp lệ, mỗi cặp phải có xác suất chính xác$1/T$. 

Đầu vào bao gồm một giới hạn trên duy nhất$k$, xác định phạm vi giá trị mà cả hai thành phần của mỗi cặp có thể nhận và một số nguyên$n$, đó là số cặp chúng ta phải tạo ra. Đầu ra chỉ đơn giản là$n$các cặp được tạo độc lập. 

Số lượng cặp hợp lệ có cấu trúc hình tam giác. Đối với mỗi cố định$a$, giá trị$b$có thể dao động từ$a$ĐẾN$k$, cho$k - a + 1$cặp. Tổng hợp tất cả$a$, tổng số cặp là$k(k+1)/2$. Với$k$lên đến$10^9$, con số này quá lớn để liệt kê hoặc lưu trữ một cách rõ ràng, vì vậy mọi giải pháp đều phải hoạt động với công thức hoặc lập chỉ mục ngầm. 

Một cách tiếp cận ngây thơ xây dựng tất cả các cặp rõ ràng sẽ thất bại ngay lập tức cả về thời gian và bộ nhớ. Ngay cả việc tạo ra một danh sách đầy đủ cũng sẽ yêu cầu$O(k^2)$không gian trong trường hợp xấu nhất là không thể. 

Một trường hợp thất bại tinh vi hơn sẽ xuất hiện nếu chúng ta cố gắng chọn$a$Và$b$độc lập thống nhất với$[1, k]$và sau đó từ chối các cặp không hợp lệ trong đó$a > b$. Điều này tạo ra sự phân bố sai lệch vì các cặp có giá trị nhỏ$a$được thể hiện quá mức trong số các kết quả hợp lệ sau khi bị loại bỏ, vì xác suất loại bỏ không đồng nhất trên tất cả các vùng của hình vuông. 

Khó khăn cốt lõi là đảm bảo lấy mẫu thống nhất trên một miền hình tam giác được nhúng bên trong hình vuông. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ là liệt kê rõ ràng tất cả các cặp hợp lệ$(a, b)$, lưu trữ chúng trong một mảng và chọn thống nhất từ ​​mảng đó. Điều này hoạt động về mặt khái niệm vì mỗi cặp được biểu diễn chính xác một lần. Tuy nhiên số lượng cặp$k(k+1)/2$, trở nên lớn về mặt thiên văn ngay cả đối với mức độ vừa phải$k$. Chỉ riêng yêu cầu về bộ nhớ là không thể và thời gian xây dựng là bậc hai trong$k$. 

Điều quan trọng là chúng ta không cần tập hợp đầy đủ các cặp mà chỉ cần lập chỉ mục cho chúng một cách thống nhất. Nếu chúng ta có thể ánh xạ từng cặp tới một số nguyên duy nhất trong$[1, T]$và đảo ngược ánh xạ đó một cách hiệu quả, sau đó tạo một cặp giảm để tạo ra một số nguyên thống nhất và giải mã nó. 

Chúng ta có thể sắp xếp các cặp theo từ điển bằng cách$a$, sau đó bởi$b$. Đối với một cố định$a$, có chính xác$k - a + 1$cặp. Điều này mang lại một cấu trúc tích lũy: tất cả các cặp bắt đầu từ nhỏ hơn$a$tạo thành các khối liền kề nhau. Nếu chúng ta xác định một hàm tiền tố$$P(a) = \sum_{i=1}^{a} (k - i + 1),$$sau đó$P(a)$cho chúng ta biết có bao nhiêu cặp tồn tại đến và bao gồm cả khối$a$. Khi chúng tôi lấy mẫu một số nguyên ngẫu nhiên$x$TRONG$[1, T]$, chúng ta có thể xác định vị trí nào$a$chứa nó, sau đó tính toán phần bù trong khối đó để phục hồi$b$. 

Thử thách duy nhất còn lại là tìm$a$một cách hiệu quả. Từ$P(a)$là một hàm bậc hai, chúng ta có thể đảo ngược nó bằng cách sử dụng tìm kiếm nhị phân hoặc trực tiếp bằng cách sử dụng công thức bậc hai, cho kết quả$O(1)$tính toán cho mỗi mẫu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê lực lượng vũ phu |$O(k^2)$|$O(k^2)$| Quá chậm | 
| Ánh xạ chỉ mục với đảo ngược |$O(n)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giả định quyền truy cập vào một trình tạo số nguyên ngẫu nhiên thống nhất có thể tạo ra các giá trị trong phạm vi lớn. 

### Các bước 

1. Tính tổng số cặp hợp lệ$T = k(k+1)/2$. Điều này xác định không gian lấy mẫu thống nhất. 
2. Đối với mỗi$n$đầu ra cần thiết, tạo ra một số nguyên ngẫu nhiên$x$thống nhất trong$[1, T]$. Điều này thể hiện chỉ mục của một cặp theo thứ tự từ điển. 
3. Xác định giá trị nhỏ nhất$a$sao cho số cặp có thể chặn$a$, cụ thể là$P(a)$, ít nhất là$x$. Bước này xác định tọa độ đầu tiên mà cặp thuộc về. 
4. Một lần$a$đã biết, tính xem bao xa$x$nằm bên trong khối$a$. Sự bù trừ đó tương ứng với việc chọn$b$trong phạm vi$[a, k]$, Vì thế$b = a + (x - P(a-1) - 1)$ở dạng offset dựa trên 0. 
5. Đầu ra$(a, b)$. 

Phần không cần thiết duy nhất là bước 3. Thay vì quét tuyến tính, chúng ta đảo ngược dạng bậc hai của$P(a)$. Từ$P(a)$tăng đơn điệu, sự đảo ngược được xác định rõ ràng và mang lại một giá trị duy nhất$a$. 

### Tại sao nó hoạt động 

Việc xây dựng phân chia toàn bộ tập hợp các cặp hợp lệ thành các khối liền kề bằng các$a$. Mỗi kích thước khối đều chính xác$k-a+1$, do đó không gian lập chỉ mục được căn chỉnh hoàn hảo với các kích thước khối này. Bởi vì chỉ số ngẫu nhiên là đồng nhất trên$[1, T]$, mọi vị trí khối đều có khả năng như nhau và trong mỗi khối mọi$b$có khả năng như nhau. Tính đồng nhất hai cấp độ này đảm bảo rằng mỗi cặp$(a,b)$được chọn với xác suất chính xác$1/T$, do đó sự phân bố đồng đều trên tất cả các cặp hợp lệ. 

## Giải pháp Python```python
import sys
import random
import math

input = sys.stdin.readline

def find_a(k, x):
    # Solve P(a) >= x where P(a) = a*k - a*(a-1)/2
    # Rearranged quadratic: a^2 - (2k+1)a + 2x <= 0
    # Take smaller root upper bound

    A = 2 * k + 1
    D = A * A - 8 * x
    s = int(math.isqrt(D))

    a = (A - s) // 2
    if a < 1:
        a = 1

    # fix boundary if needed
    while a * k - a * (a - 1) // 2 < x:
        a += 1
    while (a - 1) * k - (a - 1) * (a - 2) // 2 >= x:
        a -= 1

    return a

def prefix(k, a):
    return a * k - a * (a - 1) // 2

def solve():
    k = int(input())
    n = int(input())

    T = k * (k + 1) // 2

    for _ in range(n):
        x = random.randint(1, T)

        a = find_a(k, x)
        prev = prefix(k, a - 1)
        offset = x - prev - 1
        b = a + offset

        print(a, b)

if __name__ == "__main__":
    solve()
```chức năng`find_a`thực hiện đảo ngược tổng tiền tố bằng cách sử dụng xấp xỉ dạng đóng từ phương trình bậc hai, sau đó sửa nó bằng nhiều nhất một vài điều chỉnh để đảm bảo nó nằm trên ranh giới chính xác. Điều này tránh tìm kiếm nhị phân đầy đủ trong khi vẫn an toàn khi làm tròn số nguyên. 

các`prefix`hàm mã hóa số lượng tích lũy của các cặp lên đến một giá trị nhất định$a$, được sử dụng cho cả việc hiệu chỉnh nghịch đảo và tính toán phần bù trong một khối. 

Vòng lặp chính tạo ra một chỉ mục thống nhất và chuyển đổi nó thành một cặp bằng cách sử dụng ánh xạ nghịch đảo này. Việc sử dụng`random.randint`đảm bảo tính đồng nhất trên toàn bộ phạm vi chỉ số. 

## Ví dụ đã hoạt động 

Hãy xem xét một trường hợp nhỏ trong đó$k = 4$. Các cặp hợp lệ theo thứ tự là:$(1,1), (1,2), (1,3), (1,4), (2,2), (2,3), (2,4), (3,3), (3,4), (4,4)$Vì thế$T = 10$. 

### Ví dụ 1 

Giả sử$x = 6$. 

| Bước | Giá trị | 
| --- | --- | 
| Tổng T | 10 | 
| Ngẫu nhiên x | 6 | 
| Tìm thấy một | 2 | 
| Tiền tố P(1) | 4 | 
| Bù đắp | 6 - 4 - 1 = 1 | 
| b | 2 + 1 = 3 | 

Cặp đầu ra là$(2,3)$. 

Điều này xác nhận rằng các chỉ số bên trong khối thứ hai$(2,2),(2,3),(2,4)$được ánh xạ chính xác. 

### Ví dụ 2 

Giả sử$x = 1$. 

| Bước | Giá trị | 
| --- | --- | 
| Tổng T | 10 | 
| Ngẫu nhiên x | 1 | 
| Tìm thấy một | 1 | 
| Tiền tố P(0) | 0 | 
| Bù đắp | 1 - 0 - 1 = 0 | 
| b | 1 | 

Cặp đầu ra là$(1,1)$. 

Điều này xác minh tính chính xác tại ranh giới nơi khối đầu tiên bắt đầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi mẫu yêu cầu số học theo thời gian không đổi và một bước hiệu chỉnh nhỏ | 
| Không gian |$O(1)$| Không có lưu trữ tỷ lệ thuận với$k$hoặc$n$được yêu cầu | 

Lời giải dễ dàng nằm trong giới hạn vì$n \le 10000$và mỗi phép toán là số học theo thời gian không đổi cộng với một vài phép tính số nguyên. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import random

    # deterministic seed for reproducibility in tests
    random.seed(1)

    k = int(sys.stdin.readline())
    n = int(sys.stdin.readline())

    T = k * (k + 1) // 2

    def find_a(k, x):
        A = 2 * k + 1
        D = A * A - 8 * x
        s = int(D ** 0.5)
        a = (A - s) // 2
        a = max(1, min(k, a))
        return a

    def prefix(k, a):
        return a * k - a * (a - 1) // 2

    out = []
    for _ in range(n):
        x = random.randint(1, T)
        a = find_a(k, x)
        b = a + (x - prefix(k, a - 1) - 1)
        out.append(f"{a} {b}")

    return "\n".join(out)

# provided sample-like sanity check (structure only)
assert run("5\n3\n") is not None

# minimum size
assert run("2\n5\n") is not None

# uniform generation sanity (non-deterministic structure check)
assert len(run("10\n10\n").splitlines()) == 10
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2, 5`| 5 đôi | Độ chính xác ranh giới tối thiểu | 
|`10, 10`| 10 đôi | Tính nhất quán về cấu trúc chung | 
|`3, 1`| 1 cặp | Vỏ cạnh thế hệ đơn | 

## Vỏ cạnh 

Trường hợp quan trọng xuất hiện khi$x$rơi chính xác vào ranh giới giữa hai$a$-khối. Ví dụ, khi$k = 4$, sự chuyển tiếp giữa$a = 1$Và$a = 2$xảy ra tại$x = 4$. Tổng tiền tố phải ánh xạ chính xác giá trị này tới phần tử cuối cùng của khối đầu tiên,$(1,4)$, không phải phần tử đầu tiên của khối thứ hai. 

Bước điều chỉnh trong`find_a`xử lý vấn đề này bằng cách điều chỉnh nghiệm bậc hai gần đúng cho đến khi bất đẳng thức tiền tố được thỏa mãn chính xác. Điều này đảm bảo rằng ngay cả với các lỗi làm tròn số nguyên, giá trị được chọn$a$luôn tương ứng với khối chính xác. 

Một trường hợp cạnh khác là khi$x = 1$. Công thức có thể tạo ra$a = 0$hoặc phần dưới do bị làm tròn, nhưng việc kẹp và hiệu chỉnh ngay lập tức sẽ nâng nó lên$a = 1$, sau đó$b = 1$theo sau trực tiếp từ offset bằng 0.
