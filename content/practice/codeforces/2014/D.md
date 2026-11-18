---
title: "CF 2014D - Robert Hood và Bà Hood"
description: "Mỗi công việc chiếm khoảng thời gian là ngày [l, r]. Một khách truy cập ở lại chính xác d ngày liên tiếp, vì vậy việc chọn ngày bắt đầu s có nghĩa là lượt truy cập bao gồm khoảng [s, s + d - 1]. Một công việc được tính nếu nó trùng lặp với lượt truy cập ít nhất một lần. Số lượng chồng chéo không thành vấn đề."
date: "2026-06-08T13:01:17+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 2014
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 974 (Div. 3)"
rating: 1400
weight: 2014
solve_time_s: 121
verified: true
draft: false
---

[CF 2014D - Robert Hood và Bà Hood](https://codeforces.com/problemset/problem/2014/D) 

**Đánh giá:** 1400 
**Tags:** bạo lực, cấu trúc dữ liệu, tham lam, sắp xếp 
**Thời gian giải:** 2m 1s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi công việc chiếm một khoảng thời gian trong ngày`[l, r]`. Một du khách ở lại chính xác`d`ngày liên tiếp nên việc chọn ngày bắt đầu`s`có nghĩa là chuyến thăm bao gồm khoảng thời gian`[s, s + d - 1]`. 

Một công việc được tính nếu nó trùng lặp với lượt truy cập ít nhất một lần. Số lượng chồng chéo không thành vấn đề. Một ngày chia sẻ là đủ. 

Đối với mỗi ngày bắt đầu hợp lệ có thể có, chúng ta cần biết có bao nhiêu công việc giao nhau trong khoảng thời gian đó-`d`cửa sổ. Trong số tất cả các lần bắt đầu hợp lệ, chúng tôi muốn: 

- Ngày bắt đầu sớm nhất có cửa sổ giao nhau với số lượng công việc tối đa. 
- Ngày bắt đầu sớm nhất có cửa sổ giao với số lượng công việc tối thiểu. 

Chuyến thăm phải nằm trong lịch, vì vậy ngày bắt đầu hợp lệ là từ`1`bởi vì`n - d + 1`. 

Những hạn chế là những gì thúc đẩy giải pháp. Một bài kiểm tra có thể có tới`10^5`ngày và`10^5`công việc và tổng cộng tất cả`n`giá trị trên toàn bộ đầu vào nhiều nhất là`2 · 10^5`. Điều này có nghĩa là một`O(n)`hoặc`O(n log n)`giải pháp cho mỗi trường hợp thử nghiệm là thoải mái. MỘT`O(nk)`giải pháp là không thể bởi vì nó có thể yêu cầu khoảng`10^10`hoạt động. 

Phần khó khăn là công việc là khoảng thời gian và lượt truy cập là khoảng thời gian. Việc kiểm tra chồng chéo đơn giản cho từng cặp khoảng thời gian là quá tốn kém. 

Một số trường hợp đáng chú ý. 

Coi như:```
n = 5
d = 2
job = [2,3]
```Chuyến thăm`[1,2]`chồng chéo công việc vì họ chia sẻ ngày`2`. Chuyến thăm`[3,4]`cũng trùng nhau vì họ chia sẻ ngày`3`. Việc triển khai bất cẩn chỉ kiểm tra xem liệu điểm bắt đầu truy cập có nằm trong công việc có bỏ sót một trong những phần trùng lặp này hay không. 

Hãy xem xét các mối quan hệ:```
n = 4
d = 1
jobs:
[1,1]
[4,4]
```Số lượng là: 

| Bắt đầu | Công việc chồng chéo | 
| --- | --- | 
| 1 | 1 | 
| 2 | 0 | 
| 3 | 0 | 
| 4 | 1 | 

Số lượng tối đa xảy ra vào ngày`1`Và`4`. Câu trả lời đúng là ngày`1`, sớm nhất. Quy tắc tương tự áp dụng cho mức tối thiểu. 

Một trường hợp tinh vi khác xuất hiện khi một công việc dài hơn thời gian truy cập:```
n = 10
d = 2
job = [2,8]
```Mỗi chuyến thăm bắt đầu từ ngày`1`suốt ngày`8`chồng chéo công việc này. Chúng ta cần một cách để đánh dấu tất cả các lần khởi động như vậy một cách hiệu quả thay vì kiểm tra từng cửa sổ riêng lẻ. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là kiểm tra mỗi ngày bắt đầu hợp lệ và đếm xem có bao nhiêu công việc giao nhau trong khung thời gian đó. 

Cho một ngày bắt đầu`s`, khoảng thời gian truy cập là`[s, s+d-1]`. Một công việc`[l,r]`trùng lặp chuyến thăm này nếu:```
l ≤ s+d-1
and
r ≥ s
```Kiểm tra điều kiện này với tất cả`k`công việc cho mỗi lần bắt đầu hợp lệ mang lại sự phức tạp về:```
O((n-d+1) · k)
```Trong trường hợp xấu nhất cả hai giá trị đều ở xung quanh`10^5`, sản xuất khoảng`10^10`các thử nghiệm chồng chéo. Điều đó vượt xa giới hạn. 

Quan sát quan trọng là đảo ngược quan điểm. 

Thay vì hỏi: 

> Trong ngày bắt đầu, công việc nào chồng chéo lên nhau? 

hỏi: 

> Đối với một công việc, ngày bắt đầu có trùng lặp không? 

Hãy để chuyến thăm bắt đầu lúc`s`. 

Điều kiện trùng lặp là:```
[s, s+d-1] intersects [l,r]
```tương đương với:```
s ≤ r
s+d-1 ≥ l
```Sắp xếp lại:```
s ≤ r
s ≥ l-d+1
```Vì vậy mỗi ngày bắt đầu trong phạm vi```
[l-d+1, r]
```tạo ra một lượt truy cập chồng chéo. 

Điều này cực kỳ hữu ích vì một công việc góp phần`+1`cho toàn bộ phạm vi liền kề của ngày bắt đầu. 

Bất cứ khi nào phải áp dụng nhiều bản cập nhật theo khoảng thời gian cho một mảng thì mảng khác biệt là công cụ tự nhiên. 

Đối với mỗi công việc:```
add 1 to all starts in [l-d+1, r]
```Sau khi giới hạn phạm vi đó thành ngày bắt đầu hợp lệ`[1, n-d+1]`, chúng tôi thực hiện cập nhật phạm vi trong`O(1)`sử dụng một mảng khác biệt. 

Tổng tiền tố cuối cùng sẽ tái tạo lại số lượng công việc chồng chéo chính xác cho mỗi ngày bắt đầu. 

Khi số lượng đó đã được biết, việc tìm mức tối đa sớm nhất và mức tối thiểu sớm nhất là quét tuyến tính đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nk) | O(1) | Quá chậm | 
| Tối ưu | O(n + k) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Hãy để`m = n - d + 1`, số ngày bắt đầu hợp lệ. 
2. Tạo một mảng khác biệt đủ lớn để hỗ trợ cập nhật phạm vi trên các chỉ mục`1...m`. 
3. Đối với từng công việc`[l, r]`, xác định ngày bắt đầu trùng với ngày đó. 

Từ điều kiện chồng chéo:```
l-d+1 ≤ s ≤ r
```4. Kẹp phạm vi này vào những ngày bắt đầu hợp lệ:```
L = max(1, l-d+1)
R = min(m, r)
```Bất kỳ sự khởi đầu nào bên ngoài`1...m`không thể đại diện cho một chuyến thăm hợp lệ. 
5. Nếu`L ≤ R`, thêm một vào tất cả bắt đầu trong`[L, R]`sử dụng mảng khác biệt:```
diff[L] += 1
diff[R+1] -= 1
```6. Tính tổng tiền tố của mảng sai phân. 

Giá trị kết quả tại vị trí`s`bằng số lượng công việc chồng chéo chuyến thăm bắt đầu vào ngày`s`. 
7. Quét tất cả các ngày bắt đầu từ trái sang phải. 

Theo dõi số lượng lớn nhất được thấy cho đến nay và ngày đầu tiên đạt được số lượng đó. 
8. Trong cùng một lần quét, hãy theo dõi số lượng nhỏ nhất được thấy cho đến nay và ngày sớm nhất đạt được số lượng đó. 
9. Xuất ra hai ngày bắt đầu được ghi lại. 

### Tại sao nó hoạt động 

Công việc trùng lặp một lượt truy cập chính xác khi lượt truy cập bắt đầu đáp ứng:```
l-d+1 ≤ s ≤ r
```Mỗi sự khởi đầu như vậy sẽ nhận được một đóng góp từ công việc đó và không có sự khởi đầu nào khác. 

Mảng khác biệt thêm một mảng vào chính xác phạm vi bắt đầu đó. Vì sự đóng góp từ các công việc khác nhau là độc lập nên tổng tiền tố cuối cùng tại mỗi ngày bắt đầu bằng tổng số công việc chồng chéo. 

Việc quét tìm cực đại và cực tiểu là chính xác vì nó xử lý các ngày bắt đầu theo thứ tự tăng dần. Khi số lượng bằng nhau xuất hiện muộn hơn, chúng sẽ bị bỏ qua, giữ nguyên lần xuất hiện sớm nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    ans = []

    for _ in range(t):
        n, d, k = map(int, input().split())

        m = n - d + 1
        diff = [0] * (m + 3)

        for _ in range(k):
            l, r = map(int, input().split())

            L = max(1, l - d + 1)
            R = min(m, r)

            if L <= R:
                diff[L] += 1
                diff[R + 1] -= 1

        cur = 0

        max_cnt = -1
        max_day = 1

        min_cnt = 10**18
        min_day = 1

        for day in range(1, m + 1):
            cur += diff[day]

            if cur > max_cnt:
                max_cnt = cur
                max_day = day

            if cur < min_cnt:
                min_cnt = cur
                min_day = day

        ans.append(f"{max_day} {min_day}")

    sys.stdout.write("\n".join(ans))

solve()
```Biến`m = n - d + 1`đại diện cho tất cả các vị trí bắt đầu hợp pháp. Mọi tính toán được thực hiện trên miền nén này thay vì trên lịch gốc. 

Đối với mỗi công việc, đạo hàm quan trọng là:```
s+d-1 ≥ l
⇒ s ≥ l-d+1
```Cùng với`s ≤ r`, điều này đưa ra khoảng thời gian chính xác của những lần bắt đầu trùng lặp công việc. 

Bước kẹp là cần thiết. Về mặt lý thuyết, một công việc có thể ảnh hưởng đến việc bắt đầu ít hơn`1`hoặc lớn hơn`m`, nhưng những lần bắt đầu đó không tương ứng với lượt truy cập hợp lệ. 

Quá trình quét sử dụng so sánh nghiêm ngặt:```
if cur > max_cnt:
```Và```
if cur < min_cnt:
```còn hơn là`>=`hoặc`<=`. 

Điều này duy trì vị trí sớm nhất bất cứ khi nào nhiều ngày có cùng số lượng. 

Mảng khác biệt được phân bổ thêm một vài ô vì các bản cập nhật có thể chạm vào`R + 1`. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 7
d = 2
jobs:
[1,2]
[1,3]
[6,7]
```Bắt đầu hợp lệ là`1..6`. 

Đối với mỗi công việc: 

| Việc làm | Phạm vi thô | Phạm vi kẹp | 
| --- | --- | --- | 
| [1,2] | [0,2] | [1,2] | 
| [1,3] | [0,3] | [1,3] | 
| [6,7] | [5,7] | [5,6] | 

Sau khi áp dụng các bản cập nhật và lấy tổng tiền tố: 

| Ngày bắt đầu | Công việc chồng chéo | 
| --- | --- | 
| 1 | 2 | 
| 2 | 2 | 
| 3 | 1 | 
| 4 | 0 | 
| 5 | 1 | 
| 6 | 1 | 

Số lượng tối đa là`2`, đạt được lần đầu tiên vào ngày`1`. 

Số lượng tối thiểu là`0`, đạt được lần đầu tiên vào ngày`4`. 

Đầu ra:```
1 4
```Ví dụ này cho thấy một công việc đơn lẻ đóng góp như thế nào vào toàn bộ khoảng thời gian bắt đầu thay vì một vị trí. 

### Ví dụ 2 

đầu vào:```
n = 9
d = 2
jobs:
[7,9]
[4,8]
[1,3]
[2,3]
```Bắt đầu hợp lệ là`1..8`. 

Phạm vi đóng góp: 

| Việc làm | Phạm vi bắt đầu | 
| --- | --- | 
| [7,9] | [6,8] | 
| [4,8] | [3,8] | 
| [1,3] | [1,3] | 
| [2,3] | [1,3] | 

Kết quả tổng tiền tố: 

| Ngày bắt đầu | Đếm | 
| --- | --- | 
| 1 | 2 | 
| 2 | 2 | 
| 3 | 3 | 
| 4 | 1 | 
| 5 | 1 | 
| 6 | 2 | 
| 7 | 2 | 
| 8 | 2 | 

Giá trị lớn nhất là`3`vào ngày`3`. 

Giá trị nhỏ nhất là`1`vào ngày`4`. 

Đầu ra:```
3 4
```Dấu vết này chứng tỏ rằng các công việc có độ dài rất khác nhau được xử lý thống nhất bằng các cập nhật theo khoảng thời gian. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + k) | Một lần cập nhật phạm vi cho mỗi công việc, một lần quét trên tất cả các ngày bắt đầu hợp lệ | 
| Không gian | O(n) | Mảng chênh lệch kích thước`n - d + 1`| 

Vì tổng của tất cả`n`giá trị trên đầu vào nhiều nhất là`2 · 10^5`, tổng công việc trên tất cả các trường hợp thử nghiệm là tuyến tính theo kích thước đầu vào. Điều này dễ dàng phù hợp với giới hạn 2 giây và mức sử dụng bộ nhớ vẫn ở mức thấp. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    input = sys.stdin.readline

    t = int(input())
    ans = []

    for _ in range(t):
        n, d, k = map(int, input().split())

        m = n - d + 1
        diff = [0] * (m + 3)

        for _ in range(k):
            l, r = map(int, input().split())

            L = max(1, l - d + 1)
            R = min(m, r)

            if L <= R:
                diff[L] += 1
                diff[R + 1] -= 1

        cur = 0

        mx = -1
        mx_day = 1

        mn = 10**18
        mn_day = 1

        for day in range(1, m + 1):
            cur += diff[day]

            if cur > mx:
                mx = cur
                mx_day = day

            if cur < mn:
                mn = cur
                mn_day = day

        ans.append(f"{mx_day} {mn_day}")

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return "\n".join(ans)

# provided sample
assert run(
"""6
2 1 1
1 2
4 1 2
1 2
2 4
7 2 3
1 2
1 3
6 7
5 1 2
1 2
3 5
9 2 1
2 8
9 2 4
7 9
4 8
1 3
2 3
"""
) == """1 1
2 1
1 4
1 1
1 1
3 4"""

# minimum size
assert run(
"""1
1 1 1
1 1
"""
) == "1 1"

# tie for maximum, earliest should win
assert run(
"""1
4 1 2
1 1
4 4
"""
) == "1 2"

# d = n, only one possible start
assert run(
"""1
5 5 2
1 1
5 5
"""
) == "1 1"

# long interval covering many starts
assert run(
"""1
10 2 1
2 8
"""
) == "1 9"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n=1,d=1,k=1`|`1 1`| Hạn chế tối thiểu | 
| Hai công việc vào ngày 1 và 4 |`1 2`| Xử lý ràng buộc tối đa sớm nhất | 
|`d=n`|`1 1`| Ngày bắt đầu hợp lệ duy nhất | 
| Một khoảng thời gian dài`[2,8]`|`1 9`| Chuyển đổi chính xác từ khoảng thời gian làm việc sang khoảng thời gian bắt đầu | 

## Vỏ cạnh 

Hãy xem xét:```
1
5 2 1
2 3
```Sự chồng chéo công việc bắt đầu:```
[2-2+1, 3] = [1,3]
```Số lượng trở thành: 

| Bắt đầu | Đếm | 
| --- | --- | 
| 1 | 1 | 
| 2 | 1 | 
| 3 | 1 | 
| 4 | 0 | 

Thuật toán đánh dấu chính xác khoảng thời gian`[1,3]`trong mảng khác biệt và kết quả đầu ra chính xác:```
1 4
```Điều này xác minh rằng việc chỉ chia sẻ một ngày điểm cuối vẫn được tính là chồng chéo. 

Bây giờ hãy xem xét một trường hợp cà vạt:```
1
4 1 2
1 1
4 4
```Số lượng là: 

| Bắt đầu | Đếm | 
| --- | --- | 
| 1 | 1 | 
| 2 | 0 | 
| 3 | 0 | 
| 4 | 1 | 

Trong quá trình quét, ngày`1`trở thành mức tối đa được ghi lại. Khi ngày`4`đạt đến cùng một giá trị, việc so sánh chặt chẽ sẽ ngăn cản việc thay thế. Câu trả lời vẫn là ngày tối đa sớm nhất. 

Cuối cùng, hãy xem xét:```
1
10 2 1
2 8
```Phạm vi chồng chéo là:```
[1,8]
```sau khi kẹp. 

Mảng khác biệt thêm một vào mỗi lần bắt đầu từ`1`bởi vì`8`, rời khỏi điểm bắt đầu`9`với số lượng`0`. Các đầu ra thuật toán:```
1 9
```xác nhận rằng các công việc dài ảnh hưởng đến nhiều cửa sổ được xử lý trong thời gian cập nhật liên tục thay vì kiểm tra chồng chéo lặp đi lặp lại.
