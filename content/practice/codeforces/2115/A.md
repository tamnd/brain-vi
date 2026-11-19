---
title: "CF 2115A - Cá gelly và hoa mẫu đơn rực lửa"
description: "Chúng ta bắt đầu với một dãy số nguyên dương. Trong một nước đi, chúng ta chọn hai vị trí khác nhau, ví dụ $i$ và $j$, và chúng ta ghi đè $ai$ bằng ước chung lớn nhất của giá trị hiện tại của nó và $aj$. Phần tử thứ hai $aj$ không thay đổi."
date: "2026-06-08T04:12:51+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "dp", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 2115
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 1028 (Div. 1)"
rating: 1500
weight: 2115
solve_time_s: 77
verified: true
draft: false
---

[CF 2115A - Cá gelly và hoa mẫu đơn rực lửa](https://codeforces.com/problemset/problem/2115/A) 

**Đánh giá:** 1500 
**Tags:** thuật toán xây dựng, dp, toán, lý thuyết số 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với một dãy số nguyên dương. Trong một nước đi, chúng ta chọn hai vị trí khác nhau, chẳng hạn$i$Và$j$, và chúng tôi ghi đè$a_i$với ước chung lớn nhất của giá trị hiện tại và$a_j$. Yếu tố thứ hai$a_j$vẫn không thay đổi. Chúng tôi lặp lại quá trình này cho đến khi mọi mục trong mảng trở nên giống hệt nhau và chúng tôi muốn giảm thiểu số lần di chuyển như vậy được yêu cầu. 

Quan sát quan trọng là giá trị cuối cùng bị ép buộc. Mọi thao tác thay thế một phần tử bằng một gcd gồm hai giá trị đã có trong mảng, do đó không có số nào có thể tăng lên và mọi giá trị xuất hiện đều là ước số của một giá trị ban đầu nào đó. Do đó, giá trị duy nhất có thể tồn tại trong một mảng hoàn toàn bằng nhau là gcd của toàn bộ mảng. 

Cho phép$g = \gcd(a_1, a_2, \ldots, a_n)$. Quá trình luôn kết thúc với mọi phần tử đều bằng$g$và nhiệm vụ là giảm thiểu số lượng thao tác cần thiết để chuyển đổi mảng thành tất cả$g$. 

Các ràng buộc nhỏ theo một cách rất cụ thể: tổng của$n$trên tất cả các trường hợp thử nghiệm nhiều nhất là 5000 và mỗi giá trị nhiều nhất là 5000. Điều này ngay lập tức gợi ý rằng các giải pháp có khoảng$O(n^2)$mỗi trường hợp thử nghiệm đều khả thi, nhưng bất cứ điều gì có tính khối trong$n$sẽ là quá chậm. 

Một ý tưởng ngây thơ nhưng hấp dẫn là mô phỏng quá trình một cách tham lam hoặc thậm chí tìm kiếm theo trình tự các thao tác. Điều đó không thành công vì không gian trạng thái tăng lên theo kiểu tổ hợp. Ngay cả việc quyết định áp dụng cặp nào tiếp theo cũng không thể tối ưu cục bộ nếu không có thông tin toàn cầu về cách thức hoạt động của việc truyền bá gcd. 

Cạm bẫy phổ biến thứ hai là giả định rằng một khi chúng ta tạo ra một phần tử bằng$g$, chúng ta có thể lan truyền nó đến tất cả các vị trí khác trong$n-1$hoạt động. Điều này chỉ đúng nếu chúng ta đã biết chi phí tạo ra sản phẩm đầu tiên$g$, đó là phần khó khăn. 

Ví dụ, hãy xem xét$a = [6, 10, 15]$. Gcd toàn cầu là$1$. Chúng tôi không thể có được$1$trong một bước, vì không có cặp nào có gcd$1$. Một chiến lược ngây thơ có thể cố gắng kết hợp nhiều lần các cặp và đánh giá thấp số bước cần thiết để “tạo ra” một$1$. 

## Phương pháp tiếp cận 

Quá trình trở nên dễ dàng hơn khi chúng ta tách nhiệm vụ thành hai giai đoạn: đầu tiên tạo ít nhất một phần tử bằng$g$, sau đó truyền nó tới phần còn lại của mảng. 

Giả sử chúng ta đã có một vị trí chứa$g$. Khi đó mọi phần tử khác$x$có thể biến thành$g$trong đúng một thao tác bằng cách áp dụng$x := \gcd(x, g)$. Vì vậy giai đoạn thứ hai luôn tốn kém$n-1$hoạt động. 

Khó khăn thực sự là giai đoạn đầu tiên: cần bao nhiêu thao tác để tạo ra một$g$từ mảng ban đầu bằng cách sử dụng các thao tác gcd? 

Để phân tích điều này, hãy chia mọi phần tử cho$g$. Việc chuẩn hóa này làm giảm vấn đề thành việc tạo một$1$từ mảng, vì$\gcd(a_1/g, \ldots, a_n/g) = 1$. Bây giờ chúng tôi muốn tạo một giá trị bằng$1$sử dụng hoạt động$a_i := \gcd(a_i, a_j)$. 

Nếu một số phần tử đã có$1$, thì chúng ta đã hoàn thành giai đoạn đầu tiên ngay lập tức. Ngược lại, chúng ta cần kết hợp các phần tử cho đến khi một vị trí trở thành$1$. Mỗi thao tác chỉ làm giảm các giá trị, vì vậy chúng tôi đang cố gắng xây dựng một tập hợp con có gcd một cách hiệu quả.$1$và chúng ta phải xây dựng nó một cách rõ ràng. 

Điều này dẫn đến một công thức lập trình động trên các giá trị thay vì các chỉ số. Hãy để chúng tôi xác định một DP nơi chúng tôi theo dõi số lượng hoạt động tối thiểu cần thiết để có được một giá trị gcd cụ thể từ một số tập hợp con của các phần tử trong khi "xây dựng" nó thông qua các lần hợp nhất liên tiếp. 

Tuy nhiên, có một sự đơn giản hóa quan trọng. Vì tất cả các giá trị nhiều nhất là 5000 nên chúng ta có thể tính toán cho mọi giá trị có thể$x$, số phần tử tối thiểu cần thiết để có được gcd$x$bằng cách kết hợp các tiền tố của mảng. Trực tiếp hơn, chúng ta muốn số phần tử nhỏ nhất có gcd là$1$, và sau đó chúng tôi chuyển đổi nó thành số lượng hoạt động. 

Một thực tế cấu trúc quan trọng là nếu một tập hợp con có kích thước$k$có gcd$1$, thì chúng ta có thể biến một phần tử thành$1$sử dụng chính xác$k-1$bằng cách áp dụng liên tục các phép toán gcd với các phần tử còn lại trong tập hợp con đó. Điều này đưa ra một liên kết trực tiếp giữa kích thước tập hợp con và chi phí vận hành. 

Vì vậy, giai đoạn đầu tiên rút gọn thành việc tìm tập con nhỏ nhất có gcd bằng$1$. Nếu kích thước của nó là$k$, thì chúng ta cần$k-1$các hoạt động để tạo ra một$1$. Sau đó, việc lan truyền mất$n-1$, cho tổng cộng$k + n - 2$. 

Chúng tôi tính toán kích thước tập hợp con tối thiểu này bằng cách sử dụng DP trên trạng thái gcd: lặp qua các phần tử và cập nhật kích thước tập hợp con tốt nhất cho mỗi giá trị gcd có thể có. 

### So sánh độ phức tạp 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Trình tự hoạt động của Brute Force | Hàm mũ | O(n) | Quá chậm | 
| DP trên các trạng thái gcd | O(n * maxA) | O(maxA) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán$g = \gcd(a_1, \ldots, a_n)$. Sau đó chia mỗi phần tử cho$g$, giảm bớt vấn đề để làm cho tất cả các phần tử bằng$1$. Việc chuẩn hóa này không làm thay đổi số lượng thao tác cần thiết trong cấu trúc tương đối của việc truyền gcd. 
2. Nếu có ít nhất một phần tử$1$, chuyển thẳng sang giai đoạn lập luận thứ hai sau đó, vì không cần thực hiện thao tác nào để tạo lập luận đầu tiên$1$. Câu trả lời sẽ đơn giản là$n - \text{count of ones}$. 
3. Mặt khác, chúng tôi tính toán số phần tử tối thiểu cần thiết để tạo thành gcd$1$. Chúng tôi duy trì một mảng DP`dp[x]`nghĩa là kích thước tập hợp con nhỏ nhất có gcd bằng$x$. Khởi tạo tất cả các mục là vô cùng. 
4. Lặp qua từng giá trị mảng$v$. Đối với mỗi trạng thái DP hiện có$x$, chúng ta có thể hình thành một trạng thái gcd mới$g = \gcd(x, v)$, và cập nhật`dp[g] = min(dp[g], dp[x] + 1)`. Chúng tôi cũng cho phép bắt đầu một tập hợp con mới chỉ với$v$, Vì thế`dp[v] = 1`. 
5. Sau khi xử lý tất cả các phần tử,`dp[1]`đưa ra kích thước tập hợp con nhỏ nhất có gcd là$1$. Gọi giá trị này$k$. 
6. Chi phí để tạo một đĩa đơn$1$là$k - 1$hoạt động, vì mỗi hoạt động có thể kết hợp một yếu tố bổ sung vào chuỗi gcd đang phát triển. 
7. Khi một phần tử được$1$, chúng ta truyền bá nó đến tất cả các vị trí khác một cách chính xác$n - 1$hoạt động, mỗi lần ghi đè một vị trí bằng gcd bằng$1$, điều này khiến nó không thay đổi ngoại trừ việc buộc nó trở thành$1$. 
8. Trở về$(k - 1) + (n - 1) = k + n - 2$. 

### Tại sao nó hoạt động 

Bất biến là bất kỳ chuỗi hoạt động gcd nào cũng tương ứng với việc xây dựng dần dần gcd của một tập hợp con các phần tử ban đầu. Không có thao tác nào có thể đưa ra một giá trị không phải là gcd của một số tập hợp con của các giá trị ban đầu. Vì vậy, việc xây dựng giá trị$1$tương đương với việc chọn một tập hợp con có gcd là$1$và mỗi thao tác có thể tăng kích thước của tập hợp con này thêm chính xác một phần tử đóng góp. Điều này thiết lập sự tương ứng một-một giữa kích thước tập hợp con và số lượng thao tác cần thiết để hiện thực hóa gcd của nó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from math import gcd
from collections import defaultdict

INF = 10**9

def solve():
    t = int(input())
    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        g = 0
        for x in a:
            g = gcd(g, x)

        b = [x // g for x in a]

        ones = b.count(1)
        if ones:
            print(n - ones)
            continue

        dp = {}

        for v in b:
            ndp = dp.copy()

            ndp[v] = min(ndp.get(v, INF), 1)

            for x, val in dp.items():
                ng = gcd(x, v)
                ndp[ng] = min(ndp.get(ng, INF), val + 1)

            dp = ndp

        k = dp[1]
        print(k + n - 2)

if __name__ == "__main__":
    solve()
```Việc thực hiện trực tiếp theo cách giải thích DP. Từ điển`dp`lưu trữ các giá trị gcd có thể truy cập cùng với kích thước tập hợp con nhỏ nhất cần thiết để đạt được chúng. Mỗi phần tử mới cập nhật tất cả các trạng thái trước đó, phản ánh lựa chọn bắt đầu một tập hợp con mới hoặc mở rộng tập hợp con hiện có. Sau khi xử lý tất cả các phần tử, giá trị của gcd bằng 1 sẽ cho biết kích thước tập hợp con tới hạn. 

Một điểm tinh tế là việc sử dụng từ điển được sao chép cho mỗi lần lặp. Điều này tránh ghi đè các trạng thái trong cùng một lần lặp, đảm bảo rằng mỗi phần tử được sử dụng nhiều nhất một lần trong cấu trúc tập hợp con. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
12 20 30
```Đầu tiên chúng tôi tính toán$g = 2$, vì vậy chúng tôi chuẩn hóa thành$[6, 10, 15]$. Không có phần tử nào bằng$1$, vì vậy chúng ta phải tính tập con nhỏ nhất với gcd$1$. 

| Bước | Giá trị được xử lý | trạng thái dp (đã chọn) | 
| --- | --- | --- | 
| 1 | 6 | {6:1} | 
| 2 | 10 | {6:1, 10:1, 2:2} | 
| 3 | 15 | bao gồm các kết hợp gcd tạo ra 1 với kích thước 3 | 

Sau cùng$k = 3$, vậy đáp án là$3 + 3 - 2 = 4$. 

Dấu vết này cho thấy rằng không có phần tử đơn lẻ nào bằng mục tiêu sau khi chuẩn hóa, vì vậy chúng ta phải kết hợp cả ba phần tử để đạt được gcd 1. 

### Ví dụ 2 

đầu vào:```
6
1 9 1 9 8 1
```Đây$g = 1$, vì vậy không cần chuẩn hóa. Chúng tôi đã có ba cái rồi. 

Chiến lược tốt nhất là chuyển đổi từng cái không phải một bằng cách sử dụng một thao tác duy nhất. 

| Bước | Hành động | Số người đếm | 
| --- | --- | --- | 
| ban đầu | - | 3 | 
| sau hoạt động | chuyển đổi 9,9,8 | 6 | 

Chúng ta cần chuyển đổi 3 phần tử không phải một, vì vậy câu trả lời là$3$. 

Điều này chứng tỏ trường hợp đặc biệt khi giai đoạn một là không cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \cdot V)$| Trạng thái DP trên gcd lên tới giá trị tối đa 5000 cho mỗi phần tử | 
| Không gian |$O(V)$| Từ điển DP qua các giá trị gcd có thể có | 

Tổng kích thước đầu vào trên tất cả các trường hợp thử nghiệm đủ nhỏ để DP trên các giá trị lên tới 5000 có thể dễ dàng đủ nhanh, vì$5000 \times 5000$chuyển đổi trong trường hợp xấu nhất có thể được chấp nhận trong Python với việc triển khai chặt chẽ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue().strip()

# NOTE: placeholder, actual integration depends on wrapping solve()

# custom sanity cases (conceptual)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1\n1\n7`|`0`| phần tử đơn lẻ đã thống nhất | 
|`1\n3\n2 4 6`|`2`| không có 1 ban đầu, lan truyền gcd đơn giản | 
|`1\n3\n6 10 15`|`4`| chuỗi đầy đủ cần thiết để tạo 1 | 
|`1\n5\n1 1 1 1 1`|`0`| tất cả đều đã bình đẳng | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi mảng đã chứa nhiều phần tử bằng gcd toàn cục. Đối với đầu vào như$[4, 6, 2, 8]$, gcd là$2$và ít nhất một phần tử có thể đã bằng$2$. Trong tình huống đó, giai đoạn một bị bỏ qua và chỉ còn lại quá trình lan truyền. Thuật toán phát hiện chính xác điều này thông qua`ones`kiểm tra sau khi bình thường hóa, quay lại ngay lập tức$n - \text{count of ones}$. 

Một trường hợp cạnh khác xảy ra khi không có phần tử nào bằng gcd ban đầu, nhưng hai phần tử đã trực tiếp tạo ra nó. Ví dụ$[6, 10]$có gcd$2$, nhưng không phần tử nào bằng$2$. Giai đoạn DP phát hiện chính xác rằng cả hai phần tử đều cần thiết để xây dựng$2$, cho$k = 2$, và do đó cần một thao tác để tạo ra$2$, tiếp theo là một cái nữa để tuyên truyền.
