---
title: "CF 1155C - Đồng hồ báo thức mọi nơi"
description: "Chúng tôi được cung cấp một số lần sự kiện, đã được sắp xếp theo thứ tự tăng dần. Ivan muốn định cấu hình đồng hồ báo thức đổ chuông định kỳ. Khi tiếng chuông đầu tiên vang lên ở phút y, đồng hồ tiếp tục đổ chuông lần lượt: y, y + p, y + 2p, y + 3p, ..."
date: "2026-06-12T02:42:10+07:00"
tags: ["codeforces", "competitive-programming", "math", "number-theory"]
categories: ["algorithms"]
codeforces_contest: 1155
codeforces_index: "C"
codeforces_contest_name: "Educational Codeforces Round 63 (Rated for Div. 2)"
rating: 1300
weight: 1155
solve_time_s: 91
verified: true
draft: false
---

[CF 1155C - Đồng hồ báo thức ở mọi nơi](https://codeforces.com/problemset/problem/1155/C) 

**Đánh giá:** 1300 
**Tags:** toán, lý thuyết số 
**Thời gian giải:** 1 phút 31s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một số lần sự kiện, đã được sắp xếp theo thứ tự tăng dần. Ivan muốn định cấu hình đồng hồ báo thức đổ chuông định kỳ. Khi tiếng chuông đầu tiên vang lên vào phút`y`, đồng hồ tiếp tục đổ chuông vào những thời điểm:`y, y + p, y + 2p, y + 3p, ...`Thời gian bắt đầu`y`có thể là bất kỳ số nguyên nào, nhưng dấu chấm`p`phải được chọn từ danh sách thời gian cho phép nhất định. 

Nhiệm vụ của chúng ta là xác định liệu có tồn tại một khoảng thời gian cho phép nào đó sao cho mỗi thời gian sự kiện đều thuộc về chuỗi chuông hay không. Nếu khoảng thời gian đó tồn tại, chúng tôi phải xuất ra phút bắt đầu hợp lệ và chỉ mục của khoảng thời gian đã chọn. 

Các ràng buộc ngay lập tức gợi ý rằng lời giải phải gần với tuyến tính. Cả hai`n`Và`m`có thể đạt được`3·10^5`, do đó, bất kỳ thuật toán nào kiểm tra mọi sự kiện theo từng thời kỳ sẽ yêu cầu khoảng`9·10^10`trong trường hợp xấu nhất là hoàn toàn không thể thực hiện được. 

Thời gian và khoảng thời gian của sự kiện có thể lớn như`10^18`. Điều này loại trừ mọi cách tiếp cận dựa trên các mảng được lập chỉ mục theo giá trị hoặc bất kỳ mô phỏng nào của quá trình đổ chuông. Chúng ta phải làm việc hoàn toàn với các thuộc tính số học. 

Một điểm tinh tế là báo thức có thể đổ chuông vào những thời điểm bổ sung không phải là thời gian diễn ra sự kiện. Chúng tôi chỉ yêu cầu thời gian diễn ra sự kiện phải nằm trong số những khoảnh khắc vang lên. Sự khác biệt này là rất quan trọng. 

Coi như:```
x = [3, 12, 18]
p = 3
```Chuỗi đổ chuông bắt đầu từ`3`là:```
3, 6, 9, 12, 15, 18, ...
```Các vòng bổ sung tại`6`,`9`, Và`15`đều hoàn toàn có thể chấp nhận được. 

Một sai lầm dễ mắc phải khác là chỉ tập trung vào những khác biệt liên tiếp. 

Ví dụ:```
x = [1, 7, 13]
```Sự khác biệt là:```
6, 6
```Một khoảng thời gian`6`hoạt động. 

Nếu không có:```
x = [1, 7, 19]
```Sự khác biệt là:```
6, 12
```Một khoảng thời gian`6`vẫn hoạt động vì tất cả thời gian sự kiện đều đồng dạng theo modulo`6`. 

Đối tượng chính không phải là sự khác biệt riêng lẻ mà là ước số chung lớn nhất của chúng. 

Trường hợp cạnh cuối cùng xảy ra khi không có khoảng thời gian cho phép nào phân chia cấu trúc được yêu cầu. 

Ví dụ:```
x = [1, 7, 13]
allowed periods = [4, 5]
```Tất cả thời gian sự kiện đều đồng dạng modulo`6`, nhưng cũng không`4`cũng không`5`có thể tạo ra tất cả, vì vậy câu trả lời là`NO`. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là thử mọi khoảng thời gian được phép. Trong khoảng thời gian đã chọn`p`, chúng ta có thể kiểm tra xem tất cả thời gian sự kiện có thuộc cùng một lớp dư lượng theo modulo hay không`p`. 

Nếu mọi biến cố đều thỏa mãn:```
xi ≡ x1 (mod p)
```sau đó chọn`y = x1`hoạt động. 

Thử nghiệm này đúng vì mỗi thời gian đổ chuông được tạo ra từ`x1`với thời gian`p`có cùng số dư theo modulo`p`. 

Vấn đề là chi phí. Có tới`3·10^5`thời gian ứng cử viên và lên đến`3·10^5`lần sự kiện. Kiểm tra từng cặp yêu cầu:```
O(nm)
```hoạt động quá lớn. 

Nhận xét quan trọng đến từ việc viết lại điều kiện đồng dạng. 

Nếu tất cả thời gian sự kiện phải thỏa mãn:```
xi ≡ x1 (mod p)
```sau đó:```
p | (xi - x1)
```cho mọi`i`. 

Một số chia hết mọi giá trị trong một tập hợp phải chia ước chung lớn nhất của chúng. Cho phép```
g = gcd(x2 - x1, x3 - x1, ..., xn - x1)
```Khi đó khoảng thời gian hợp lệ nào đó phải chia`g`. 

Điều này thay đổi hoàn toàn vấn đề. 

Thay vì kiểm tra mọi sự kiện theo từng thời kỳ, trước tiên chúng tôi tính toán một số`g`. Sau đó chúng ta chỉ cần kiểm tra xem một số khoảng thời gian được phép có chia`g`. 

Nếu trong khoảng thời gian cho phép`pj`thỏa mãn:```
g % pj == 0
```thì mọi sự khác biệt`(xi - x1)`chia hết cho`pj`, nghĩa là tất cả thời gian sự kiện đều đồng dạng modulo`pj`. Lựa chọn:```
y = x1
```hoạt động ngay lập tức. 

Điều này làm giảm vấn đề xuống còn quét tuyến tính sau một lần tính toán gcd. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm) | O(1) | Quá chậm | 
| Tối ưu | O(n + m) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc thời gian diễn ra sự kiện và thời gian được phép. 
2. Tính toán:```
g = gcd(x2 - x1, x3 - x1, ..., xn - x1)
```Điều này nắm bắt mọi ràng buộc về khả năng phân chia do thời gian diễn ra sự kiện áp đặt. 
3. Lặp lại các khoảng thời gian được phép theo thứ tự đầu vào của chúng. 
4. Đối với từng thời kỳ`pj`, kiểm tra xem:```
g % pj == 0
```Nếu điều này giữ vững thì`pj`chia rẽ mọi sự khác biệt`(xi - x1)`. 
5. Ngay khi tìm thấy khoảng thời gian như vậy, hãy ghi:```
YES
x1 j
```Ở đâu`j`là chỉ số dựa trên 1 của thời kỳ. 
6. Nếu không được phép chia thời gian`g`, đầu ra:```
NO
```### Tại sao nó hoạt động 

hãy để```
g = gcd(x2 - x1, ..., xn - x1).
```Nếu một khoảng thời gian`p`là hợp lệ thì mọi thời gian sự kiện phải thỏa mãn:```
xi ≡ x1 (mod p).
```Tương đương,```
p | (xi - x1)
```cho mọi`i`. 

Từ`p`chia mọi khác biệt thì phải chia gcd của họ`g`. 

Ngược lại, nếu trong một khoảng thời gian cho phép`p`chia rẽ`g`, sau đó nó chia mọi khác biệt`(xi - x1)`. Kể từ đây:```
xi ≡ x1 (mod p)
```cho tất cả`i`. 

Lựa chọn`y = x1`tạo ra một chuỗi chuông chứa mọi thời gian sự kiện. Như vậy điều kiện`p | g`là vừa cần vừa đủ. 

## Giải pháp Python```python
import sys
from math import gcd

input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    x = list(map(int, input().split()))
    p = list(map(int, input().split()))

    g = 0
    for i in range(1, n):
        g = gcd(g, x[i] - x[0])

    for idx, period in enumerate(p, start=1):
        if g % period == 0:
            print("YES")
            print(x[0], idx)
            return

    print("NO")

solve()
```Phần đầu tiên tính toán gcd của tất cả sự khác biệt liên quan đến sự kiện đầu tiên. Đang khởi tạo`g`bằng 0 thì thuận tiện vì:```
gcd(0, a) = a
```vì vậy sự khác biệt đầu tiên sẽ tự động trở thành giá trị bắt đầu. 

Vòng lặp thứ hai quét các khoảng thời gian được phép. Thời điểm một ước của`g`được tìm thấy, câu trả lời đã được biết và chúng ta có thể chấm dứt ngay lập tức. 

sử dụng`x[0]`vì thời gian bắt đầu là đủ vì mỗi thời gian sự kiện đều có cùng phần dư theo modulo của khoảng thời gian đã chọn. Không tìm kiếm`y`là cần thiết. 

Số nguyên Python xử lý các giá trị lên tới`10^18`mà không có mối quan tâm tràn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 5
3 12 18
2 6 5 3 3
```Sự khác biệt liên quan đến`3`:```
9, 15
```| Bước | Giá trị | 
| --- | --- | 
| g ban đầu | 0 | 
| gcd(0, 9) | 9 | 
| gcd(9, 15) | 3 | 

Bây giờ hãy quét các khoảng thời gian: 

| Chỉ mục | Thời kỳ | g % Thời gian | 
| --- | --- | --- | 
| 1 | 2 | 1 | 
| 2 | 6 | 3 | 
| 3 | 5 | 3 | 
| 4 | 3 | 0 | 

Khoảng thời gian hợp lệ đầu tiên xuất hiện ở chỉ mục 4. 

Đầu ra:```
YES
3 4
```Dấu vết này thể hiện định lý trung tâm. Một lần`g = 3`, bất kỳ khoảng thời gian hợp lệ phải chia`3`. 

### Ví dụ 2 

đầu vào:```
3 2
1 7 13
4 5
```Sự khác biệt liên quan đến`1`:```
6, 12
```| Bước | Giá trị | 
| --- | --- | 
| g ban đầu | 0 | 
| gcd(0, 6) | 6 | 
| gcd(6, 12) | 6 | 

Thời gian quét: 

| Chỉ mục | Thời kỳ | g % Thời gian | 
| --- | --- | --- | 
| 1 | 4 | 2 | 
| 2 | 5 | 1 | 

Không phân chia thời kỳ`6`. 

Đầu ra:```
NO
```Ví dụ này cho thấy rằng việc có một cấu trúc lũy tiến số học nhất quán là chưa đủ. Một trong những khoảng thời gian được phép thực sự phải chia gcd. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Một lượt tính toán gcd và một lượt chuyển qua các giai đoạn | 
| Không gian | O(1) | Chỉ có một vài biến ngoài mảng đầu vào | 

Thuật toán thực hiện một lượng công việc tuyến tính và sử dụng bộ nhớ bổ sung không đổi. Với`n, m ≤ 3·10^5`, điều này dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from math import gcd

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n, m = map(int, input().split())
    x = list(map(int, input().split()))
    p = list(map(int, input().split()))

    g = 0
    for i in range(1, n):
        g = gcd(g, x[i] - x[0])

    for idx, period in enumerate(p, start=1):
        if g % period == 0:
            return f"YES\n{x[0]} {idx}\n"

    return "NO\n"

# provided sample
assert run(
    "3 5\n"
    "3 12 18\n"
    "2 6 5 3 3\n"
) == "YES\n3 4\n"

# minimum size
assert run(
    "2 1\n"
    "1 2\n"
    "1\n"
) == "YES\n1 1\n"

# no valid period
assert run(
    "3 2\n"
    "1 7 13\n"
    "4 5\n"
) == "NO\n"

# gcd has several divisors, first valid index should be returned
assert run(
    "3 3\n"
    "5 17 29\n"
    "4 6 12\n"
) == "YES\n5 1\n"

# large values
assert run(
    "2 1\n"
    "1 1000000000000000000\n"
    "1\n"
) == "YES\n1 1\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 1 / 1 2 / 1`| CÓ | Phiên bản hợp lệ tối thiểu | 
|`1 7 13`với thời gian`4 5`| KHÔNG | Không có ước số nào của gcd tồn tại | 
|`5 17 29`với thời gian`4 6 12`| CÓ, chỉ số 1 | Nhiều ước số hợp lệ, tìm thấy ước số đầu tiên | 
| Lớn`10^18`giá trị | CÓ | Xử lý đúng số nguyên có kích thước 64 bit | 

## Vỏ cạnh 

Hãy xem xét:```
3 2
1 7 19
6 12
```Sự khác biệt là`6`Và`18`, cho:```
g = 6
```Giai đoạn kiểm tra thuật toán`6`và chấp nhận nó. Một sai lầm phổ biến là yêu cầu tất cả các sai phân liên tiếp phải bằng khoảng thời gian đã chọn. Ở đây sự khác biệt không bằng nhau, nhưng một khoảng thời gian`6`là hoàn toàn hợp lệ vì tất cả thời gian sự kiện đều có cùng một modulo dư`6`. 

Coi như:```
3 2
1 7 13
4 5
```Chúng tôi có được:```
g = 6
```Không`4`cũng không`5`chia rẽ`6`, do đó kết quả đầu ra của thuật toán`NO`. Điều này xử lý tình huống trong đó cấu trúc sự kiện thừa nhận một khoảng thời gian hợp lệ nhưng không có tùy chọn nào được phép cung cấp điều đó. 

Coi như:```
2 3
10 25
3 5 15
```gcd là:```
g = 15
```Thuật toán tìm thấy rằng`3`chia rẽ`15`và kết quả đầu ra:```
YES
10 1
```Một số giải pháp tìm kiếm nhầm một kết quả khác biệt chính xác và sẽ chỉ chấp nhận`15`. Bất kỳ ước số nào của gcd đều hợp lệ, bởi vì tất cả các thời gian sự kiện vẫn bằng modulo của ước số đó.
