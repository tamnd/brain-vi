---
title: "CF 75C - GCD sửa đổi"
description: "Chúng ta được cung cấp hai số nguyên dương và sau đó là nhiều truy vấn phạm vi. Đối với mỗi truy vấn [thấp, cao], chúng ta phải tìm số nguyên lớn nhất bên trong khoảng chia cả hai số. Quan sát đầu tiên là chúng ta không thực sự làm việc với a và b một cách độc lập."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 75
codeforces_index: "C"
codeforces_contest_name: "Codeforces Beta Round 67 (Div. 2)"
rating: 1600
weight: 75
solve_time_s: 100
verified: true
draft: false
---

[CF 75C - GCD đã sửa đổi](https://codeforces.com/problemset/problem/75/C) 

**Đánh giá:** 1600 
**Tas:** tìm kiếm nhị phân, lý thuyết số 
**Thời gian giải:** 1 phút 40s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp hai số nguyên dương và sau đó là nhiều truy vấn phạm vi. Đối với mỗi truy vấn`[low, high]`, chúng ta phải tìm số nguyên lớn nhất bên trong khoảng chia cả hai số. 

Quan sát đầu tiên là chúng tôi không thực sự làm việc với`a`Và`b`một cách độc lập. Số nào chia cả hai thì cũng chia hết`gcd(a, b)`. Khi chúng ta tính gcd của hai số, mọi câu trả lời hợp lệ sẽ trở thành một trong các ước số của nó. 

Kích thước đầu vào gợi ý rõ ràng rằng các truy vấn phải được trả lời nhanh chóng. Bản thân những con số có thể đạt tới`10^9`, điều này làm cho việc lặp qua mọi số nguyên trong phạm vi truy vấn là không thể. Một khoảng truy vấn có thể chứa một tỷ số. Với tối đa`10^4`các truy vấn, thậm chí là một`O(high - low)`cách tiếp cận cho mỗi truy vấn sẽ là thảm họa. 

Đồng thời,`10^9`đủ nhỏ để có thể liệt kê các ước của một số. Một con số lên tới`10^9`có nhiều nhất khoảng vài nghìn ước số. Tìm tất cả các ước số bằng cách kiểm tra các số nguyên lên đến`sqrt(g)`đủ nhanh vì`sqrt(10^9)`đại khái là`31623`. 

Phần khó khăn là xử lý chính xác điều kiện phạm vi. Chúng tôi không muốn bất kỳ ước số nào của gcd. Chúng tôi muốn ước số lớn nhất nằm bên trong`[low, high]`. 

Có một số trường hợp đặc biệt có thể lặng lẽ phá vỡ việc triển khai không chính xác. 

Giả định:```
a = 12
b = 18
query = [7, 11]
```gcd là`6`, ước số của nó là`{1, 2, 3, 6}`. Không có gì nằm trong khoảng nên đáp án là`-1`. Việc thực hiện bất cẩn chỉ tìm kiếm từ dưới lên`high`không kiểm tra tính chia hết một cách cẩn thận có thể vô tình trả về`6`, mặc dù nó nằm ngoài phạm vi. 

Một trường hợp tinh tế khác xuất hiện khi câu trả lời bằng giới hạn dưới:```
a = 100
b = 50
query = [25, 30]
```Các ước số chung là`{1, 2, 5, 10, 25, 50}`. Câu trả lời đúng là`25`. Nếu ranh giới tìm kiếm nhị phân được xử lý không chính xác, rất dễ bỏ qua các kết quả khớp chính xác và trả về`-1`. 

Hình vuông hoàn hảo cũng cần được chú ý khi tạo ước số.```
gcd = 36
```Trong khi lặp với`i = 6`, cả hai`i`Và`gcd // i`là cùng một ước số. Thêm cả hai bản sao một cách mù quáng`6`. Thuật toán vẫn hoạt động nếu các bản sao được sắp xếp sau đó, nhưng sự trùng lặp không cần thiết sẽ làm phức tạp việc suy luận và lãng phí thời gian. 

Cuối cùng, hãy xem xét khi nào bản thân gcd nằm trong phạm vi:```
a = 9
b = 27
query = [9, 11]
```Câu trả lời là ngay lập tức`9`. Một số lời giải sai tìm kiếm các ước số nhỏ hơn rất nhiều so với`high`, bỏ lỡ trường hợp ranh giới chính xác này. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp nhất là xử lý từng truy vấn một cách độc lập. Đối với một truy vấn`[low, high]`, chúng ta có thể lặp đi xuống từ`high`ĐẾN`low`và kiểm tra xem mỗi số có chia hết cho cả hai không`a`Và`b`. 

Điều đó có hiệu quả vì số hợp lệ đầu tiên gặp phải tự động là ước số hợp lệ lớn nhất trong khoảng. 

Vấn đề là thời gian chạy. Trong trường hợp xấu nhất, phạm vi truy vấn có thể chứa gần như`10^9`những con số. Với`10^4`truy vấn, điều này trở nên hoàn toàn không thể. 

Nhận xét quan trọng là mọi ước chung của`a`Và`b`phải chia`g = gcd(a, b)`. 

Thay vì tìm kiếm trong chính khoảng đó, chúng ta có thể tính toán trước tất cả các ước của`g`. Điều này thay đổi vấn đề từ: 

"Tìm một số nguyên trong khoảng chia cả hai số" 

đến: 

“Tìm ước số lớn nhất của`g`trong phạm vi." 

Đó là nhỏ hơn nhiều. Một con số lên tới`10^9`có rất ít ước số so với kích thước của nó. Chúng ta có thể tạo ra tất cả các ước số trong`O(sqrt(g))`, sắp xếp chúng một lần rồi trả lời từng truy vấn bằng tìm kiếm nhị phân. 

Đối với một truy vấn`[low, high]`, chúng ta tìm kiếm nhị phân cho ước số lớn nhất`<= high`. Nếu số chia đó ít nhất vẫn bằng`low`, đó là câu trả lời. Nếu không thì không tồn tại ước số hợp lệ. 

Brute-force hoạt động vì việc kiểm tra khả năng chia hết rất đơn giản và chính xác, nhưng nó thất bại vì nó tìm kiếm một phạm vi số rất lớn. Giải pháp dựa trên số chia thành công vì tập hợp các câu trả lời có thể có rất nhỏ và cố định trên tất cả các truy vấn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n × kích thước phạm vi) | O(1) | Quá chậm | 
| Tối ưu | O(sqrt(g) + n log d) | O(d) | Đã chấp nhận | 

Đây,`g = gcd(a, b)`Và`d`là số ước của`g`. 

## Hướng dẫn thuật toán 

1. Đọc số nguyên`a`Và`b`. 
2. Tính toán`g = gcd(a, b)`. 

Mọi ước chung của`a`Và`b`phải chia`g`, vì vậy tất cả các câu trả lời hợp lệ đều đến từ các ước của`g`. 
3. Tạo tất cả các ước của`g`. 

Lặp lại từ`1`ĐẾN`sqrt(g)`. Bất cứ khi nào`i`chia rẽ`g`, cả hai`i`Và`g // i`là các ước số. 
4. Tránh chèn trùng lặp khi`i * i == g`. 

Perfect squares produce the same divisor twice, so we only insert it once.
 5. Sắp xếp danh sách ước. 

Tìm kiếm nhị phân yêu cầu thứ tự sắp xếp. 
6. Đối với mỗi truy vấn`[low, high]`, tìm kiếm nhị phân cho ước số lớn nhất`<= high`. 

Trong Python,`bisect_right(divs, high) - 1`đưa ra chỉ số của ước số lớn nhất không vượt quá`high`. 
7. Kiểm tra xem số chia này có còn ít nhất không`low`. 

Nếu có, hãy in nó. Nếu không thì in`-1`. 

### Tại sao nó hoạt động 

Thuật toán dựa trên một thuộc tính: 

Một số chia hết cả hai`a`Và`b`nếu và chỉ khi nó chia`gcd(a, b)`. 

Điều đó có nghĩa là tập hợp đầy đủ các câu trả lời có thể chính xác là tập hợp ước số của`g`. Bằng cách sắp xếp các ước số đó, tìm kiếm nhị phân luôn có thể xác định được ước số lớn nhất không vượt quá`high`. Nếu giá trị đó ít nhất cũng`low`, nó nằm trong khoảng yêu cầu và là câu trả lời hợp lệ lớn nhất có thể. Nếu nó nhỏ hơn`low`, thì mọi ước số trước đó thậm chí còn nhỏ hơn, do đó không có ước số hợp lệ nào tồn tại trong khoảng. 

## Giải pháp Python```python
import sys
from math import gcd, isqrt
from bisect import bisect_right

input = sys.stdin.readline

def solve():
    a, b = map(int, input().split())

    g = gcd(a, b)

    divisors = []

    for i in range(1, isqrt(g) + 1):
        if g % i == 0:
            divisors.append(i)

            other = g // i
            if other != i:
                divisors.append(other)

    divisors.sort()

    q = int(input())

    for _ in range(q):
        low, high = map(int, input().split())

        idx = bisect_right(divisors, high) - 1

        if idx >= 0 and divisors[idx] >= low:
            print(divisors[idx])
        else:
            print(-1)

solve()
```Giải pháp bắt đầu bằng cách thu gọn bài toán ban đầu thành bài toán gcd. Một lần`g = gcd(a, b)`được tính toán, mọi câu trả lời hợp lệ phải đến từ các ước của nó. 

Vòng lặp tạo số chia chỉ chạy tối đa`sqrt(g)`. Bất cứ khi nào`i`chia rẽ`g`, chúng ta biết ngay rằng`g // i`cũng là số chia. Thủ thuật tạo cặp này làm giảm độ phức tạp từ thời gian tuyến tính sang căn bậc hai. 

Điều kiện:```
if other != i:
```xử lý các ô vuông hoàn hảo một cách chính xác. Không có nó, các ước số như`6`TRONG`36`sẽ được chèn hai lần. 

Sau khi sắp xếp danh sách ước số, mỗi truy vấn sẽ trở thành một bài toán tìm kiếm nhị phân.`bisect_right(divisors, high)`trả về vị trí chèn sau tất cả các giá trị`<= high`. Trừ đi một sẽ cho chỉ số của ứng cử viên hợp lệ lớn nhất. 

Kiểm tra ranh giới:```
divisors[idx] >= low
```là điều cần thiết. Tìm kiếm nhị phân chỉ đảm bảo tối đa số chia là`high`. Chúng ta vẫn phải xác nhận nó nằm trong khoảng. 

Việc thực hiện sử dụng`isqrt`thay vì căn bậc hai dấu phẩy động. Căn bậc hai số nguyên tránh được các vấn đề về độ chính xác và sạch hơn cho phép liệt kê số chia. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
9 27
3
1 5
10 11
9 11
```gcd là:```
gcd(9, 27) = 9
```Ước của`9`:```
[1, 3, 9]
```| Truy vấn | Ước số lớn nhất ≤ cao | Ứng viên | Có hiệu lực? | Trả lời | 
| --- | --- | --- | --- | --- | 
| [1, 5] | 3 | 3 | 3 ≥ 1 | 3 | 
| [10, 11] | 9 | 9 | 9 < 10 | -1 | 
| [9, 11] | 9 | 9 | 9 ≥ 9 | 9 | 

Ví dụ này thể hiện logic tìm kiếm nhị phân cốt lõi. Truy vấn thứ hai cho thấy tại sao chúng ta vẫn phải so sánh với`low`sau khi tìm ước số lớn nhất không vượt quá`high`. 

### Ví dụ 2 

đầu vào:```
100 80
4
1 2
5 15
16 25
41 50
```gcd là:```
gcd(100, 80) = 20
```Ước của`20`:```
[1, 2, 4, 5, 10, 20]
```| Truy vấn | Ước số lớn nhất ≤ cao | Ứng viên | Có hiệu lực? | Trả lời | 
| --- | --- | --- | --- | --- | 
| [1, 2] | 2 | 2 | 2 ≥ 1 | 2 | 
| [5, 15] | 10 | 10 | 10 ≥ 5 | 10 | 
| [16, 25] | 20 | 20 | 20 ≥ 16 | 20 | 
| [41, 50] | 20 | 20 | 20 < 41 | -1 | 

Dấu vết này cho thấy thuật toán không bao giờ tự quét khoảng đó. Ngay cả đối với phạm vi lớn, nó chỉ tìm kiếm trong số các ước số của gcd. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(sqrt(g) + n log d) |`sqrt(g)`để tạo ước số, tìm kiếm nhị phân cho mỗi truy vấn | 
| Không gian | O(d) | lưu trữ tất cả các ước số của gcd | 

Đây`g = gcd(a, b)`Và`d`là số ước của`g`. 

Căn bậc hai tối đa là khoảng`31623`, rất nhỏ trong giới hạn 2 giây. Số lượng ước số cũng nhỏ nên mỗi truy vấn kết thúc theo thời gian logarit trên một mảng rất ngắn. Giải pháp dễ dàng phù hợp với cả hạn chế về thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from math import gcd, isqrt
from bisect import bisect_right

def solve():
    input = sys.stdin.readline

    a, b = map(int, input().split())

    g = gcd(a, b)

    divisors = []

    for i in range(1, isqrt(g) + 1):
        if g % i == 0:
            divisors.append(i)

            other = g // i
            if other != i:
                divisors.append(other)

    divisors.sort()

    q = int(input())

    out = []

    for _ in range(q):
        low, high = map(int, input().split())

        idx = bisect_right(divisors, high) - 1

        if idx >= 0 and divisors[idx] >= low:
            out.append(str(divisors[idx]))
        else:
            out.append("-1")

    print("\n".join(out))

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    return sys.stdout.getvalue()

# provided sample
assert run(
"""9 27
3
1 5
10 11
9 11
"""
) == "3\n-1\n9\n", "sample 1"

# minimum-size input
assert run(
"""1 1
1
1 1
"""
) == "1\n", "minimum case"

# no divisor in range
assert run(
"""12 18
1
7 11
"""
) == "-1\n", "no valid divisor"

# exact lower-bound answer
assert run(
"""100 50
1
25 30
"""
) == "25\n", "boundary inclusion"

# perfect square gcd
assert run(
"""36 72
2
6 6
7 8
"""
) == "6\n-1\n", "perfect square divisor handling"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`với truy vấn`[1,1]`|`1`| Hạn chế tối thiểu | 
|`12 18`với truy vấn`[7,11]`|`-1`| Không có ước số nào tồn tại trong phạm vi | 
|`100 50`với truy vấn`[25,30]`|`25`| Bao gồm ranh giới dưới | 
|`36 72`với các truy vấn`[6,6]`Và`[7,8]`|`6`,`-1`| Xử lý hình vuông hoàn hảo và các khoảng bị thiếu | 

## Vỏ cạnh 

Xét trường hợp không có ước số nào nằm trong khoảng:```
12 18
1
7 11
```gcd là`6`, ước số của nó là`[1, 2, 3, 6]`. 

Tìm kiếm nhị phân cho ước số lớn nhất`<= 11`trả lại`6`. Thuật toán sau đó sẽ kiểm tra xem`6 >= 7`. Điều đó không thành công, vì vậy đầu ra trở thành`-1`. 

Kiểm tra ranh giới bổ sung này ngăn cản việc trả về các ước số ngoài khoảng được yêu cầu. 

Bây giờ hãy xem xét một câu trả lời có ranh giới chính xác:```
100 50
1
25 30
```gcd là`50`, ước số của nó là`[1, 2, 5, 10, 25, 50]`. 

Tìm kiếm nhị phân cho ước số lớn nhất`<= 30`trả lại`25`. Từ`25 >= 25`, đầu ra của thuật toán`25`. 

Điều này xác nhận rằng các khoảng được bao gồm ở cả hai đầu. 

Cuối cùng, hãy xem xét một gcd vuông hoàn hảo:```
36 72
1
6 6
```gcd là`36`. 

Trong quá trình tạo số chia, khi`i = 6`, cả hai`i`Và`36 // 6`bình đẳng`6`. Việc triển khai chỉ chèn nó một lần vì:```
if other != i:
```Danh sách số chia vẫn đúng:```
[1, 2, 3, 4, 6, 9, 12, 18, 36]
```Tìm kiếm nhị phân sau đó tìm thấy chính xác`6`cho khoảng thời gian`[6, 6]`.
