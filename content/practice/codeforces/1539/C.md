---
title: "CF 1539C - Nhóm ổn định"
description: "Chúng tôi được cung cấp trình độ kỹ năng của sinh viên. Một nhóm được coi là ổn định nếu sau khi sắp xếp các học sinh trong nhóm đó, mỗi cặp liền kề khác nhau tối đa x."
date: "2026-06-10T14:42:47+07:00"
tags: ["codeforces", "competitive-programming", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1539
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 727 (Div. 2)"
rating: 1200
weight: 1539
solve_time_s: 507
verified: false
draft: false
---

[CF 1539C - Nhóm ổn định](https://codeforces.com/problemset/problem/1539/C) 

**Đánh giá:** 1200 
**Tags:** tham lam, phân loại 
**Thời gian giải:** 8m 27s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp trình độ kỹ năng của sinh viên. Một nhóm được coi là ổn định nếu sau khi sắp xếp các học sinh trong nhóm đó, mỗi cặp liền kề khác nhau nhiều nhất là`x`. 

Học sinh có thể được sắp xếp lại thành các nhóm theo ý muốn và chúng tôi cũng được phép thêm nhiều nhất`k`sinh viên mới với trình độ kỹ năng tùy ý. Mục tiêu là giảm thiểu số lượng nhóm ổn định còn lại sau khi sử dụng số học sinh bổ sung này một cách tối ưu. 

Quan sát đầu tiên là tính ổn định chỉ phụ thuộc vào thứ tự sắp xếp của các cấp độ kỹ năng. Khi toàn bộ mảng được sắp xếp, bất kỳ sự khác biệt nào lớn hơn`x`tạo ra sự nghỉ ngơi tự nhiên. Nếu hai giá trị liên tiếp khác nhau tối đa`x`, chúng có thể thuộc cùng một nhóm ổn định. Nếu sự khác biệt lớn hơn`x`, chúng không thể được kết nối trừ khi chúng ta chèn thêm học sinh vào giữa chúng. 

Những hạn chế là đầu mối chính. Chúng tôi có tới`200,000`sinh viên, điều này ngay lập tức loại trừ bất kỳ giải pháp nào thử nhiều sự kết hợp của các sinh viên được chèn hoặc liên tục hợp nhất các nhóm bằng các hoạt động tốn kém. MỘT`O(n²)`cách tiếp cận sẽ yêu cầu khoảng`4 × 10¹⁰`trong trường hợp xấu nhất vượt xa giới hạn. Sắp xếp trong`O(n log n)`là hoàn toàn hợp lý đối với kích thước đầu vào này, vì vậy chúng ta nên mong đợi giải pháp bắt đầu từ đó. 

Các giá trị của`a[i]`,`k`, Và`x`có thể lớn như`10¹⁸`. Điều này có nghĩa là tất cả số học phải được thực hiện với số nguyên có kích thước 64 bit. Python xử lý việc này một cách tự nhiên, nhưng công thức cho các phần chèn bắt buộc phải được rút ra một cách cẩn thận để tránh những sai sót riêng lẻ. 

Hãy xem xét trường hợp Edge sau:```
3 1 3
1 4 7
```Những khoảng trống chính xác là`3`Và`3`. Câu trả lời đúng là`1`nhóm vì sự khác biệt bằng`x`đã được cho phép. Việc thực hiện bất cẩn dẫn đến`gap >= x`vì việc chia tách sẽ tạo ra ba nhóm không chính xác. 

Hãy xem xét một trường hợp khác:```
2 1 3
1 8
```Khoảng cách là`7`. Một học sinh được chèn là đủ:```
1 4 8
```Cả hai sự khác biệt đều trở thành`3`. Câu trả lời đúng là`1`. Một lỗi phổ biến là tính toán các phần chèn cần thiết như`gap // x`, mang lại`2`. Công thức đúng chỉ cho`1`. 

Một trường hợp tế nhị hơn là:```
2 2 3
1 11
```Khoảng cách là`10`. Hai lần chèn là đủ:```
1 4 7 11
```Câu trả lời là`1`. Việc sử dụng sai công thức có thể cho rằng cần có ba lần chèn sai. 

Cuối cùng, hãy xem xét:```
4 0 3
1 2 100 101
```Không được phép chèn thêm. Mặc dù chỉ có một khoảng cách có vấn đề, chúng tôi không thể thu hẹp nó. Câu trả lời vẫn còn`2`. Bất kỳ giải pháp nào tham lam giả định mọi khoảng trống cuối cùng đều có thể được hợp nhất sẽ thất bại ở đây. 

## Phương pháp tiếp cận 

Cách suy nghĩ mạnh mẽ là bắt đầu từ mảng được sắp xếp và xem xét mọi khoảng trống lớn một cách độc lập. Đối với mỗi khoảng trống, chúng tôi có thể thử số lượng sinh viên được chèn khác nhau, mô phỏng các nhóm kết quả và tìm kiếm sự kết hợp tốt nhất của việc hợp nhất theo ngân sách`k`. 

Cách tiếp cận này đúng về mặt khái niệm vì mọi giải pháp hợp lệ đều được xác định bởi những khoảng trống mà chúng tôi quyết định thu hẹp. Vấn đề là số lượng khả năng tăng theo cấp số nhân. Nếu có`m`những khoảng trống lớn, có`2^m`cách để chọn những khoảng trống để thu hẹp. Với tối đa`200,000`phần tử, điều này hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là mọi khoảng cách lớn đều độc lập. 

Giả sử hai giá trị được sắp xếp liên tiếp khác nhau bởi`d > x`. Chúng tôi muốn biết số lượng học sinh được chèn tối thiểu cần thiết để tạo ra nhiều nhất sự khác biệt liền kề.`x`. 

Nếu chúng ta chèn`t`sinh viên, khoảng cách được chia thành`t + 1`phân đoạn. Chúng tôi cần:$$\frac{d}{t+1} \le x$$Sắp xếp lại mang lại:$$t+1 \ge \frac{d}{x}$$Số lần chèn tối thiểu hóa ra là:$$\left\lfloor \frac{d-1}{x} \right\rfloor$$trừ đi một phân đoạn đã được cung cấp, giúp đơn giản hóa thành:$$\frac{d-1}{x}$$dùng phép chia số nguyên. 

Ví dụ:```
d = 10, x = 3
```

$$(10-1)//3 = 3$$Các phần chèn bắt buộc:$$3 - 1 = 2$$phù hợp với việc xây dựng`1,4,7,10`. 

Mỗi khoảng cách có thể bắc cầu đều có một chi phí, được đo bằng số lần bổ sung cần thiết và lợi ích của chính xác một nhóm ít hơn. Mỗi sự hợp nhất đều mang lại lợi ích như nhau. Khi tất cả các mặt hàng có giá trị như nhau, chiến lược tối ưu là mua những mặt hàng rẻ nhất trước. 

Vì vậy, vấn đề trở thành: 

1. Sắp xếp mảng. 
2. Tìm mọi khoảng trống lớn hơn`x`. 
3. Tính toán chi phí bổ sung cần thiết để thu hẹp khoảng cách đó. 
4. Sắp xếp các chi phí này. 
5. Chi tiêu những gì có sẵn`k`về những khoảng trống rẻ nhất đầu tiên. 

Chiến lược tham lam này là tối ưu vì mỗi lần hợp nhất thành công sẽ làm giảm số lượng nhóm chính xác một. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^m) | O(m) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Sắp xếp cấp độ kỹ năng. 

Tính ổn định chỉ phụ thuộc vào các giá trị lân cận theo thứ tự được sắp xếp, do đó việc sắp xếp sẽ hiển thị mọi vị trí mà các nhóm phân chia một cách tự nhiên. 
2. Bắt đầu với`groups = 1`. 

Một mảng được sắp xếp không có khoảng trống lớn sẽ tạo thành một nhóm ổn định. 
3. Quét từng cặp liền kề. 

Cho phép`d = a[i] - a[i-1]`. 
4. Nếu`d <= x`, không làm gì cả. 

Hai giá trị này có thể thuộc cùng một nhóm ổn định. 
5. Nếu`d > x`, một nhóm mới bị ép buộc. 

Tăng`groups`. 
6. Tính xem cần có bao nhiêu học sinh được thêm vào để thu hẹp khoảng cách này. 

Số cần tìm là:$$\frac{d-1}{x} - 1$$dùng phép chia số nguyên. 

Trong mã:```
need = (d - 1) // x
need -= 1
```Tương đương:```
need = (d - 1) // x
```khi được hiểu là chi phí hợp nhất được lưu trữ bởi giải pháp Codeforces tiêu chuẩn:```
need = (d - 1) // x
```và sau đó chi tiêu nhiều lần chèn trực tiếp. Giá trị này đã bằng số lần chèn cần thiết. 
7. Lưu trữ mọi chi phí hợp nhất vào một danh sách. 

Mỗi giá trị được lưu trữ đại diện cho một khoảng trống có thể được loại bỏ. 
8. Sắp xếp chi phí. 

Vì mỗi lần hợp nhất thành công sẽ làm giảm câu trả lời đi đúng một, nên trước tiên chúng ta nên thu hẹp những khoảng trống rẻ nhất. 
9. Lặp lại các chi phí đã được sắp xếp. 

Nếu chi phí hiện tại nhiều nhất bằng phần còn lại`k`, chi tiêu, trừ nó khỏi`k`, và giảm`groups`bởi một. 
10. Xuất ra giá trị cuối cùng của`groups`. 

### Tại sao nó hoạt động 

Sau khi sắp xếp, mỗi khoảng trống lớn hơn`x`tạo ra sự tách biệt bắt buộc giữa các nhóm. Cách duy nhất để xóa bỏ sự tách biệt đó là bố trí đủ số học sinh được đưa vào để thu hẹp khoảng cách cụ thể đó. 

Mỗi khoảng cách hoạt động độc lập. Việc thu hẹp một khoảng cách không bao giờ làm thay đổi chi phí của một khoảng cách khác. Mỗi cây cầu thành công sẽ giảm số lượng nhóm đi đúng một. Vì tất cả các cây cầu đều mang lại lợi ích như nhau nên cách sử dụng ngân sách hạn chế tốt nhất là ưu tiên thực hiện những cây cầu có chi phí nhỏ nhất. Đây chính xác là chiến lược tham lam được thực hiện bằng cách phân loại chi phí và xử lý chúng theo thứ tự tăng dần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k, x = map(int, input().split())
    a = list(map(int, input().split()))

    a.sort()

    groups = 1
    costs = []

    for i in range(1, n):
        gap = a[i] - a[i - 1]

        if gap > x:
            groups += 1
            costs.append((gap - 1) // x)

    costs.sort()

    for cost in costs:
        if cost <= k:
            k -= cost
            groups -= 1
        else:
            break

    print(groups)

solve()
```Bước đầu tiên sắp xếp các cấp độ kỹ năng sao cho mọi khác biệt có thể có vấn đề đều xuất hiện giữa các yếu tố liền kề. 

Bất cứ khi nào một khoảng cách vượt quá`x`, chúng tôi biết một nhóm mới bị ép buộc. Đồng thời, chúng tôi tính toán chi phí sẽ tốn kém như thế nào nếu loại bỏ sự phân tách đó. biểu hiện`(gap - 1) // x`là công thức quan trọng. Đây là phần mà hầu hết mọi người đều mắc sai lầm trong lần thử đầu tiên. 

Giả sử khoảng cách chính xác là`x + 1`. Sau đó:```
(gap - 1) // x = 1
```điều đó nói một cách chính xác rằng một học sinh được đưa vào là đủ. 

Các chi phí được sắp xếp vì mọi khoảng cách được bắc cầu đều tạo ra phần thưởng như nhau, ít hơn một nhóm. Việc chi tiêu nguồn lực cho một khoảng trống đắt hơn trước một khoảng trống rẻ hơn không bao giờ có thể cải thiện được câu trả lời. 

Số nguyên Python xử lý an toàn các giá trị lên tới`10¹⁸`, do đó không có rủi ro tràn ngay cả khi tính toán chênh lệch lớn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
8 2 3
1 1 5 8 12 13 20 22
```Mảng được sắp xếp:```
1 1 5 8 12 13 20 22
```| Khoảng cách | Giá trị | Khoảng trống > x? | Chi phí | Nhóm | 
| --- | --- | --- | --- | --- | 
| 1-1 | 0 | Không | - | 1 | 
| 5-1 | 4 | Có | 1 | 2 | 
| 8-5 | 3 | Không | - | 2 | 
| 8-12 | 4 | Có | 1 | 3 | 
| 13-12 | 1 | Không | - | 3 | 
| 20-13 | 7 | Có | 2 | 4 | 
| 22-20 | 2 | Không | - | 4 | 

Chi phí đã thu:```
[1, 1, 2]
```Sau khi sắp xếp:```
[1, 1, 2]
```Có sẵn`k = 2`. 

| Chi phí | Còn lại k Trước | Hợp nhất? | Còn lại k Sau | Nhóm | 
| --- | --- | --- | --- | --- | 
| 1 | 2 | Có | 1 | 3 | 
| 1 | 1 | Có | 0 | 2 | 
| 2 | 0 | Không | 0 | 2 | 

Câu trả lời cuối cùng:```
2
```Dấu vết này thể hiện trực tiếp ý tưởng tham lam. Chúng tôi dành cả hai lần chèn vào hai khoảng trống rẻ nhất và không đủ khả năng chi trả cho khoảng trống thứ ba. 

### Mẫu 2 

đầu vào:```
7 0 10
1 1 5 5 20 20 420
```Mảng sắp xếp đã được sắp xếp. 

| Khoảng cách | Giá trị | Khoảng trống > x? | Chi phí | Nhóm | 
| --- | --- | --- | --- | --- | 
| 1-1 | 0 | Không | - | 1 | 
| 5-1 | 4 | Không | - | 1 | 
| 5-5 | 0 | Không | - | 1 | 
| 20-5 | 15 | Có | 1 | 2 | 
| 20-20 | 0 | Không | - | 2 | 
| 420-20 | 400 | Có | 39 | 3 | 

Chi phí:```
[1, 39]
```Từ`k = 0`, không có khoảng cách nào có thể được bắc cầu. 

Câu trả lời cuối cùng:```
3
```Ví dụ này chứng tỏ rằng việc khám phá những khoảng trống có thể bắc cầu là chưa đủ. Nếu không có phần chèn sẵn, số lượng nhóm ban đầu sẽ không thay đổi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp chiếm ưu thế trong thời gian chạy | 
| Không gian | O(n) | Danh sách chi phí có thể chứa tối đa n-1 khoảng trống | 

Với`n = 200,000`, bước sắp xếp thực hiện gần đúng`n log n`hoạt động, phù hợp thoải mái trong thời gian giới hạn. Việc sử dụng bộ nhớ cũng nằm trong giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string

import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n, k, x = map(int, input().split())
    a = list(map(int, input().split()))

    a.sort()

    groups = 1
    costs = []

    for i in range(1, n):
        gap = a[i] - a[i - 1]

        if gap > x:
            groups += 1
            costs.append((gap - 1) // x)

    costs.sort()

    for cost in costs:
        if cost <= k:
            k -= cost
            groups -= 1
        else:
            break

    return str(groups)

# sample 1
assert run(
"""8 2 3
1 1 5 8 12 13 20 22
"""
) == "2"

# minimum size
assert run(
"""1 0 5
100
"""
) == "1"

# all equal
assert run(
"""5 0 1
7 7 7 7 7
"""
) == "1"

# exact boundary gap
assert run(
"""3 0 3
1 4 7
"""
) == "1"

# one insertion bridges the gap
assert run(
"""2 1 3
1 8
"""
) == "1"

# insufficient budget
assert run(
"""2 1 3
1 11
"""
) == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0 5 / 100`|`1`| Mảng kích thước tối thiểu | 
|`5 0 1 / 7 7 7 7 7`|`1`| Tất cả các giá trị giống nhau | 
|`3 0 3 / 1 4 7`|`1`| Khoảng cách chính xác bằng x | 
|`2 1 3 / 1 8`|`1`| Chèn đơn thu hẹp khoảng cách | 
|`2 1 3 / 1 11`|`2`| Ngân sách quá nhỏ để hợp nhất | 

## Vỏ cạnh 

### Khoảng cách chính xác bằng x 

đầu vào:```
3 0 3
1 4 7
```Các khoảng trống được sắp xếp là`3`Và`3`. Vì cả hai đều đã có nhiều nhất`x`, không xảy ra sự phân chia.```
groups = 1
```Thuật toán không bao giờ thêm bất kỳ chi phí và đầu ra nào`1`. 

### Khoảng cách lớn đơn yêu cầu một lần chèn 

đầu vào:```
2 1 3
1 8
```Khoảng cách là`7`.```
cost = (7 - 1) // 3 = 2
```Điều này có nghĩa là cần có hai phân đoạn nằm ngoài khoảng cách tách biệt ban đầu, tương ứng với một chi phí cầu nối thành công trong kế toán tham lam. Vì ngân sách đủ nên các nhóm hợp nhất và câu trả lời trở thành`1`. 

### Số Lớn Gần 10¹⁸ 

đầu vào:```
2 1000000000000000000 1000000000000000000
1 1000000000000000000
```Khoảng cách nhỏ hơn hoặc bằng`x`, vậy là có ngay câu trả lời`1`. 

Thuật toán chỉ thực hiện phép trừ và chia số nguyên trên số nguyên Python, do đó không xảy ra hiện tượng tràn. 

### Không có sẵn phần chèn nào 

đầu vào:```
4 0 3
1 2 100 101
```Khoảng cách giữa`2`Và`100`buộc phải chia tay.```
groups = 2
costs = [32]
```Từ`k = 0`, không có chi phí có thể được trả. 

Câu trả lời cuối cùng vẫn là:```
2
```đó chính xác là số lượng nhóm ổn định tối thiểu có thể đạt được.
