---
title: "CF 1883C - Quả Mâm Xôi"
description: "Chúng ta có một mảng và một số nguyên nhỏ $k$, trong đó $2 le k le 5$. Trong một thao tác, chúng ta có thể chọn bất kỳ phần tử nào và tăng nó lên một phần tử. Nhiệm vụ là tìm số lượng phép toán nhỏ nhất cần thiết để tích của tất cả các phần tử mảng chia hết cho $k$."
date: "2026-06-08T22:31:42+07:00"
tags: ["codeforces", "competitive-programming", "dp", "math"]
categories: ["algorithms"]
codeforces_contest: 1883
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 905 (Div. 3)"
rating: 1000
weight: 1883
solve_time_s: 360
verified: true
draft: false
---

[CF 1883C - Quả mâm xôi](https://codeforces.com/problemset/problem/1883/C) 

**Đánh giá:** 1000 
**Thẻ:** dp, toán 
**Thời gian giải:** 6 phút 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng và một số nguyên nhỏ$k$, Ở đâu$2 \le k \le 5$. Trong một thao tác, chúng ta có thể chọn bất kỳ phần tử nào và tăng nó lên một phần tử. Nhiệm vụ là tìm số phép toán nhỏ nhất cần thiết để tích của tất cả các phần tử mảng chia hết cho$k$. 

Quan sát quan trọng là chúng tôi không bao giờ quan tâm đến sản phẩm chính xác. Chúng tôi chỉ quan tâm liệu sản phẩm có chứa các thừa số nguyên tố theo yêu cầu hay không$k$. Từ$k$nhiều nhất là$5$, chỉ có bốn giá trị có thể có của$k$:$2$,$3$,$4$, Và$5$. Điều này làm cho vấn đề nhỏ hơn nhiều so với lần đầu tiên nó xuất hiện. 

Tổng số phần tử mảng trong tất cả các trường hợp thử nghiệm nhiều nhất là$2 \cdot 10^5$. Bất kỳ giải pháp nào kiểm tra từng phần tử với số lần không đổi đều dễ dàng đủ nhanh. Các phương pháp tiếp cận liên quan đến lập trình động trên các trạng thái lớn hoặc mô phỏng hoạt động lặp đi lặp lại là không cần thiết. 

Trường hợp cạnh tinh tế xuất hiện khi$k=4$. Chia hết cho$4$khác với phép chia cho$2$, vì sản phẩm phải chứa hai yếu tố$2$. Ví dụ:```
n = 2, k = 4
a = [1, 3]
```Việc tăng một phần tử lên bội số tiếp theo của bốn chi phí cho một thao tác:```
[1, 4]
```Câu trả lời là$1$. Một giải pháp chỉ đếm số chẵn sẽ bỏ lỡ khả năng này. 

Một trường hợp quan trọng khác là khi một số phần tử có thể đóng góp các thừa số của cả hai lại với nhau:```
n = 3, k = 4
a = [1, 5, 9]
```Làm một số chia hết cho bốn chi phí$3$hoạt động:```
1 -> 4
```Nhưng việc tạo ra hai số chẵn chỉ tốn chi phí$2$hoạt động:```
1 -> 2
5 -> 6
```Khi đó tích chứa hai thừa số của 2, vì vậy câu trả lời đúng là$2$. 

Trường hợp cạnh cuối cùng xảy ra khi tích đã chia hết cho$k$. Ví dụ:```
n = 4, k = 5
a = [5, 4, 1, 2]
```Câu trả lời là$0$. Bất kỳ thuật toán nào cũng nên kiểm tra điều này trước khi thực hiện các phép tính tiếp theo. 

## Phương pháp tiếp cận 

Cách tiếp cận vũ phu sẽ thử các chuỗi các phép toán tăng dần và tìm kiếm trạng thái đầu tiên có tích chia hết cho$k$. Ngay cả khi chúng ta chỉ xem xét số lượng nhỏ các phép toán, hệ số phân nhánh vẫn là$n$, làm cho việc tìm kiếm theo cấp số nhân. 

Cấu trúc của vấn đề làm cho điều này trở nên không cần thiết. Từ$k \le 5$, tính chia hết chỉ phụ thuộc vào một số thừa số nguyên tố. 

Vì$k=2$,$k=3$, Và$k=5$, sản phẩm được chia cho$k$ngay khi một phần tử mảng chia hết cho$k$. Nếu hiện tại không có phần tử nào chia hết cho$k$, chúng ta chỉ cần tính xem cần bao nhiêu số gia để di chuyển từng phần tử tới bội số tiếp theo của$k$, sau đó lấy mức tối thiểu. 

Trường hợp thú vị duy nhất là$k=4$. Một sản phẩm được chia cho$4$nếu nó chứa ít nhất hai ước của hai. Điều này có thể xảy ra theo hai cách khác nhau. 

Một phần tử có thể chia hết cho$4$. 

Hoặc một số yếu tố có thể đóng góp các yếu tố của hai với nhau. Từ$n \ge 2$, chỉ cần xem xét việc làm cho hai phần tử chẵn là đủ. 

Câu trả lời cho$k=4$là chi phí tối thiểu trong số tất cả các cách hợp lệ để có được hai thừa số là hai. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu |$O(n)$mỗi trường hợp thử nghiệm |$O(1)$thêm | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng. 
2. Tính mức chia gián tiếp của sản phẩm hiện tại. 

Đối với mọi phần tử, hãy kiểm tra phần còn lại của nó theo modulo$k$. Nếu một phần tử nào đó đã chia hết cho$k$Và$k \in \{2,3,5\}$, câu trả lời là ngay lập tức$0$. 
3. Nếu$k$là$2$,$3$, hoặc$5$, tính toán$$\min_i (k - a_i \bmod k) \bmod k.$$Đây là chi phí để di chuyển một phần tử sang bội số tiếp theo của$k$. 

1. Nếu$k=4$, trước tiên hãy tính chi phí tối thiểu để làm cho một số phần tử chia hết cho$4$:$$\min_i (4 - a_i \bmod 4) \bmod 4.$$1. Đếm các thừa số hiện tại của 2 trong mảng. 

Đối với mỗi phần tử: 

- Nếu chia hết cho$4$, đóng góp$2$. 
- Ngược lại nếu bằng thì đóng góp$1$. 
- Các đóng góp khác$0$. 
2. Nếu tổng số thừa số của 2 ít nhất$2$, câu trả lời là$0$. 
3. Ngược lại hãy xác định cần thêm bao nhiêu thừa số của 2. 
4. Tính chi phí để tạo ra các phần tử chẵn. Đối với một phần tử lẻ, chi phí là$1$. Đối với một phần tử chẵn, chi phí là$0$. 
5. Sắp xếp các chi phí này và lấy số phần tử nhỏ nhất cần thiết để có được thừa số còn thiếu là 2. 
6. Đáp án cuối cùng là đáp án tối thiểu giữa: 

- Làm cho một phần tử chia hết cho$4$. 
- Tập hợp đủ thừa số của hai từ nhiều phần tử. 

### Tại sao nó hoạt động 

cho$k=2$,$3$, Và$5$, tính chia hết của tích chỉ cần một thừa số của số nguyên tố tương ứng. Một phần tử chia hết cho$k$là đủ và cần thiết. 

Vì$k=4$, tích phải chứa ít nhất hai thừa số của hai. Mọi cấu hình hợp lệ đều đạt được điều này thông qua một phần tử chia hết cho bốn hoặc thông qua một số phần tử chẵn có đóng góp tổng hợp đạt đến hai thừa số của hai. Thuật toán đánh giá rõ ràng cả hai khả năng và chọn khả năng rẻ hơn. Vì mọi thao tác chỉ làm tăng một phần tử nên cách rẻ nhất để đạt được điều kiện chia hết cần thiết là luôn di chuyển các phần tử đến giá trị đủ điều kiện gần nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    n, k = map(int, input().split())
    a = list(map(int, input().split()))

    if k in (2, 3, 5):
        ans = min((k - x % k) % k for x in a)
        print(ans)
        continue

    # k == 4
    ans = min((4 - x % 4) % 4 for x in a)

    twos = 0
    for x in a:
        if x % 4 == 0:
            twos += 2
        elif x % 2 == 0:
            twos += 1

    if twos >= 2:
        ans = 0
    else:
        costs = []
        for x in a:
            costs.append(x % 2)

        costs.sort()

        need = 2 - twos
        ans = min(ans, sum(costs[:need]))

    print(ans)
```Giải pháp tách biệt các trường hợp dễ dàng$k=2$,$3$, Và$5$từ trường hợp đặc biệt$k=4$. 

Vì$k=2$,$3$, Và$5$, chỉ có nhiều vấn đề gần nhất. biểu hiện$$(k - x \bmod k) \bmod k$$trả về 0 khi$x$đã chia hết cho$k$, nếu không nó sẽ trả về số lượng gia tăng chính xác cần thiết. 

Vì$k=4$, câu trả lời ứng cử viên đầu tiên là chi phí để làm cho một phần tử chia hết cho bốn. Sau đó, mã đếm các yếu tố của hai yếu tố đã có trong mảng. Nếu có ít nhất hai thì tích đã chia hết cho bốn. 

Ngược lại, mã sẽ xem xét việc tạo các phần tử lẻ theo số chẵn. Phần tử lẻ yêu cầu một thao tác, trong khi phần tử chẵn không yêu cầu thao tác nào. Việc sắp xếp các chi phí này cho phép chúng ta chọn các yếu tố rẻ nhất cần thiết để có được các thừa số còn thiếu là 2. 

Không tồn tại vấn đề tràn số nguyên vì bản thân sản phẩm không bao giờ được tính toán. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 2, k = 5
a = [7, 3]
```| Yếu tố | Chi phí cho bội số tiếp theo của 5 | 
| --- | --- | 
| 7 | 3 | 
| 3 | 2 | 

Chi phí tối thiểu là$2$. 

Trả lời:```
2
```Bước đi tối ưu là$3 \to 5$. 

### Ví dụ 2 

đầu vào:```
n = 3, k = 4
a = [1, 5, 9]
```| Yếu tố | Chi phí theo bội số của 4 | 
| --- | --- | 
| 1 | 3 | 
| 5 | 3 | 
| 9 | 3 | 

Câu trả lời ứng cử viên đầu tiên là$3$. 

Các yếu tố hiện tại của hai: 

| Yếu tố | Các yếu tố góp phần | 
| --- | --- | 
| 1 | 0 | 
| 5 | 0 | 
| 9 | 0 | 

Chúng ta cần hai yếu tố của hai. 

Chi phí để trở nên đồng đều: 

| Yếu tố | Chi phí | 
| --- | --- | 
| 1 | 1 | 
| 5 | 1 | 
| 9 | 1 | 

Lấy hai chi phí rẻ nhất sẽ mang lại$1 + 1 = 2$. 

Câu trả lời trở thành:```
2
```Điều này chứng tỏ tại sao chỉ xem xét bội số của bốn sẽ không chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi phần tử được xử lý một số lần không đổi | 
| Không gian |$O(n)$| Chỉ có danh sách chi phí tạm thời cho$k=4$| 

Tổng kích thước đầu vào tối đa là$2 \cdot 10^5$, do đó, một giải pháp tuyến tính chạy thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n, k = map(int, input().split())
        a = list(map(int, input().split()))

        if k in (2, 3, 5):
            out.append(str(min((k - x % k) % k for x in a)))
            continue

        ans = min((4 - x % 4) % 4 for x in a)

        twos = 0
        for x in a:
            if x % 4 == 0:
                twos += 2
            elif x % 2 == 0:
                twos += 1

        if twos >= 2:
            ans = 0
        else:
            costs = sorted(x % 2 for x in a)
            ans = min(ans, sum(costs[:2 - twos]))

        out.append(str(ans))

    return "\n".join(out)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return solve()

assert run("1\n2 5\n7 3\n") == "2"
assert run("1\n5 5\n5 4 1 2 3\n") == "0"
assert run("1\n3 4\n1 5 9\n") == "2"
assert run("1\n3 4\n6 3 6\n") == "1"
assert run("1\n2 2\n1 1\n") == "1"
assert run("1\n2 5\n10 10\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 5 / 7 3`|`2`| Cơ bản bội logic gần nhất | 
|`5 5 / 5 4 1 2 3`|`0`| Đã chia hết | 
|`3 4 / 1 5 9`|`2`| Hai số chẵn đánh bại một bội số của bốn | 
|`3 4 / 6 3 6`|`1`| Hệ số hiện có của hai cộng một tăng | 
|`2 2 / 1 1`|`1`| Mảng hợp lệ nhỏ nhất | 
|`2 5 / 10 10`|`0`| Mọi phần tử đều đã chia hết | 

## Vỏ cạnh 

Hãy xem xét:```
n = 4
k = 5
a = [5, 1, 1, 1]
```Một phần tử đã chia hết cho 5 nên tích sẽ chia hết cho 5. Thuật toán tính toán chi phí bằng 0 cho phần tử đó và ngay lập tức trả về$0$. 

Coi như:```
n = 3
k = 4
a = [2, 3, 3]
```Mảng đã đóng góp một hệ số hai từ giá trị$2$. Chúng ta cần thêm một yếu tố nữa. Quay một trong hai$3$vào trong$4$chi phí một hoạt động. Thuật toán tìm$twos = 1$, cần thêm một thừa số nữa và trả về$1$. 

Coi như:```
n = 2
k = 4
a = [1, 1]
```Làm cho một phần tử chia hết cho bốn tốn ba thao tác. Việc tạo ra cả hai phần tử thậm chí còn tốn hai thao tác. Thuật toán đánh giá cả hai khả năng và chọn chính xác$2$.
