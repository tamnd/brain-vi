---
title: "CF 2082D - Cân bằng"
description: "Điều duy nhất quan trọng đối với mảng ban đầu là dấu của mọi phép so sánh liền kề. Với mỗi vị trí $i$, xác định một cạnh giữa $ai$ và $a{i+1}$. Nếu $ai < a{i+1}$, gọi cạnh đó là dương. Nếu $ai a{i+1}$, gọi cạnh đó là âm."
date: "2026-06-09T03:46:26+07:00"
tags: ["codeforces", "competitive-programming", "greedy"]
categories: ["algorithms"]
codeforces_contest: 2082
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 1010 (Div. 2, Unrated)"
rating: 2500
weight: 2082
solve_time_s: 161
verified: true
draft: false
---

[CF 2082D - Cân bằng](https://codeforces.com/problemset/problem/2082/D) 

**Đánh giá:** 2500 
**Thẻ:** tham lam 
**Thời gian giải:** 2m 41s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Điều duy nhất quan trọng đối với mảng ban đầu là dấu của mọi phép so sánh liền kề. 

Đối với mỗi vị trí$i$, xác định một cạnh giữa$a_i$Và$a_{i+1}$. 

Nếu như$a_i < a_{i+1}$, gọi cạnh đó là dương. 

Nếu như$a_i > a_{i+1}$, gọi cạnh đó là âm. 

Mảng đã tăng lên chính xác khi mọi cạnh đều dương. 

Một thao tác chọn một phân đoạn liền kề và viết lại tất cả các giá trị bên trong nó. Hạn chế quan trọng là mọi so sánh hoàn toàn bên trong phân khúc đã chọn phải giữ nguyên dấu hiệu ban đầu. Cạnh dương vẫn là dương, cạnh âm vẫn là âm. 

Kích thước đầu vào lớn. Tổng chiều dài của tất cả các trường hợp thử nghiệm là$2 \cdot 10^5$, do đó mọi thuật toán bậc hai đều bị loại trừ ngay lập tức. Chúng tôi cần một cái gì đó tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. 

Sai lầm dễ nhất là nghĩ về các giá trị thực tế. Hoạt động không quan tâm đến độ lớn, chỉ quan tâm đến dấu hiệu so sánh. 

Coi như:```
3 2 1
```Có hai cạnh tiêu cực. Một thao tác chỉ có thể ảnh hưởng đến các cạnh trên ranh giới của khoảng của nó, do đó không thể loại bỏ cả hai giá trị âm cùng một lúc. Câu trả lời đúng là`2`. 

Một cái bẫy dễ mắc khác là giả định rằng mọi cặp cạnh âm luôn có thể bị loại bỏ cùng nhau.```
3 2 1
```Hai cạnh âm là cạnh đầu tiên và cạnh cuối cùng. Nếu chúng ta cố gắng loại bỏ cả hai bằng một khoảng thì các giá trị nằm ngoài khoảng đó sẽ phải thỏa mãn`3 < 1`, điều đó là không thể. Câu trả lời vẫn còn`2`. 

Một ví dụ tinh tế hơn là:```
-2 -5 5 2
```Các cạnh âm nằm ở vị trí 1 và 3. Ở đây các giá trị bên ngoài là`-2`Và`2`, vì vậy một thao tác là đủ. Câu trả lời đúng là`1`. 

Những ví dụ này gợi ý rằng câu trả lời phụ thuộc vào vị trí của các cạnh âm và vào một so sánh đơn lẻ giữa hai giá trị biên. 

## Phương pháp tiếp cận 

Quan điểm bạo lực là coi mọi thao tác như việc chọn một khoảng và thay đổi các giá trị bên trong nó. Người ta có thể thử mô hình hóa tất cả các khoảng có thể, tất cả các chuỗi hoạt động có thể có và tìm kiếm số lượng tối thiểu. 

Cách tiếp cận đó sụp đổ ngay lập tức. có$O(n^2)$các khoảng thời gian, các thao tác có thể được lặp lại và không gian trạng thái rất lớn. 

Quan sát quan trọng là một thao tác chỉ thay đổi hai cạnh chạm vào ranh giới khoảng. 

Giả sử chúng ta chọn$[l,r]$. 

Mọi cạnh hoàn toàn bên trong khoảng sẽ giữ nguyên dấu ban đầu của nó mãi mãi trong suốt thao tác đó. Chỉ có cạnh$(l-1,l)$và cạnh$(r,r+1)$có thể thay đổi. 

Điều này có nghĩa là một thao tác đơn lẻ có thể loại bỏ tối đa hai cạnh âm. Cho phép$m$là số cạnh âm. Sau đó:$$\text{answer} \ge \left\lceil \frac{m}{2} \right\rceil.$$Toàn bộ vấn đề trở thành: 

Chúng ta có thể luôn đạt được giới hạn dưới này không? 

Nếu như$m$thật kỳ lạ, câu trả lời là có. 

Nếu như$m$là số chẵn, câu trả lời đôi khi là có và đôi khi lớn hơn. 

Nhiệm vụ còn lại là xác định chính xác thời điểm có thể đạt được giới hạn dưới. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Để các cạnh âm ở vị trí$$d_1 < d_2 < \dots < d_m.$$Đây$d_i$có nghĩa$a_{d_i} > a_{d_i+1}$. 

### 1. Đếm các cạnh âm 

Hãy đếm số lượng của họ$m$. 

Nếu như$m=0$, mảng đã tăng lên rất nhiều nên câu trả lời là`0`. 

### 2. Xử lý trường hợp lẻ 

Nếu$m$thật kỳ lạ, câu trả lời là$$\frac{m+1}{2}.$$Giới hạn dưới có thể đạt được bằng cách loại bỏ một cạnh âm trong thao tác đầu tiên và sau đó loại bỏ hai cạnh âm còn lại cùng một lúc. 

### 3. Xử lý trường hợp chẵn 

hãy để$$k = \frac{m}{2}.$$Để đạt được chính xác$k$các phép toán, mỗi phép toán phải loại bỏ chính xác hai cạnh âm. 

Điều đó buộc cạnh âm đầu tiên và cạnh âm cuối cùng vẫn là hai ranh giới bên ngoài của toàn bộ công trình. Các giá trị bên ngoài liền kề của chúng là$$x = a_{d_1}, \qquad y = a_{d_m+1}.$$Hai giá trị này trở thành bất biến của bất kỳ chiến lược nào sử dụng chính xác$k$hoạt động. 

### 4. Kiểm tra tính bất biến 

Nếu$$x < y,$$thì toàn bộ vùng giữa chúng có thể được xây dựng lại thành một chuỗi tăng dần trong khi ghép tất cả các cạnh âm theo từng hai. Giới hạn dưới$k$là có thể đạt được. 

Nếu như$$x \ge y,$$thì một cây cầu tăng nghiêm ngặt như vậy là không thể. Cần thêm một thao tác nữa. 

Vì thế:$$\text{answer} = \begin{cases} k, & x < y,\\ k+1, & x \ge y. \end{cases}$$### Tại sao nó hoạt động 

Một cạnh âm chỉ có thể biến mất nếu nó trở thành cạnh biên của một số thao tác. Vì một phép toán chỉ có hai biên nên nó có thể loại bỏ tối đa hai cạnh âm. Điều này mang lại giới hạn dưới$\lceil m/2 \rceil$. 

Khi$m$thật kỳ lạ, giới hạn dưới đó luôn có thể được khớp. 

Khi$m$là số chẵn, khớp với giới hạn dưới có nghĩa là mọi thao tác phải loại bỏ chính xác hai số âm. Trong trường hợp đó, giá trị bên trái cạnh cạnh âm đầu tiên và giá trị bên phải cạnh cạnh âm cuối cùng không thể thay đổi. Bất kỳ công trình xây dựng tăng nghiêm ngặt cuối cùng nào cũng phải kết nối hai giá trị cố định này. 

Nếu như$a_{d_1} < a_{d_m+1}$, một kết nối như vậy tồn tại và giới hạn dưới có thể đạt được. 

Nếu như$a_{d_1} \ge a_{d_m+1}$, không có dãy tăng dần nào có thể bắt đầu ở giá trị bên trái và kết thúc ở giá trị bên phải, do đó không thể tránh khỏi một thao tác nữa. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        desc = []
        for i in range(n - 1):
            if a[i] > a[i + 1]:
                desc.append(i)

        m = len(desc)

        if m == 0:
            ans.append("0")
            continue

        if m & 1:
            ans.append(str((m + 1) // 2))
        else:
            k = m // 2
            first = desc[0]
            last = desc[-1]

            if a[first] < a[last + 1]:
                ans.append(str(k))
            else:
                ans.append(str(k + 1))

    sys.stdout.write("\n".join(ans))

solve()
```Việc thực hiện chỉ cần vị trí của các cạnh âm.`desc`lưu trữ mọi chỉ mục`i`như vậy`a[i] > a[i+1]`. 

Nếu số đếm là số lẻ thì công thức là ngay lập tức. 

Nếu số đếm là số chẵn, chúng ta xét giá trị ngay bên trái của cạnh âm đầu tiên và giá trị ngay bên phải của cạnh âm cuối cùng. Việc so sánh giữa hai giá trị đó sẽ xác định liệu có thể đạt được giới hạn dưới hay không. 

Chi tiết lập chỉ mục dễ mắc sai lầm nhất là điểm cuối phù hợp. Nếu cạnh âm cuối cùng ở vị trí`i`, giá trị biên bên phải là`a[i + 1]`, trở thành`a[last + 1]`trong lập chỉ mục dựa trên số không. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
3 2 1
```Các cạnh tiêu cực: 

| Vị trí cạnh | So sánh | Tiêu cực? | 
| --- | --- | --- | 
| 1 | 3 > 2 | Có | 
| 2 | 2 > 1 | Có | 

Vì vậy: 

| Biến | Giá trị | 
| --- | --- | 
| m | 2 | 
| k | 1 | 
| đầu tiên | 1 | 
| cuối cùng | 2 | 
| x | 3 | 
| y | 1 | 

Từ`x >= y`, câu trả lời là`k + 1 = 2`. 

Đây chính xác là tình huống mà một thao tác không thể kết nối hai giá trị biên cố định thành một chuỗi tăng nghiêm ngặt. 

### Ví dụ 2 

đầu vào:```
4
-2 -5 5 2
```Các cạnh tiêu cực: 

| Vị trí cạnh | So sánh | Tiêu cực? | 
| --- | --- | --- | 
| 1 | -2 > -5 | Có | 
| 2 | -5 < 5 | Không | 
| 3 | 5 > 2 | Có | 

Bây giờ: 

| Biến | Giá trị | 
| --- | --- | 
| m | 2 | 
| k | 1 | 
| x | -2 | 
| y | 2 | 

Từ`x < y`, giới hạn dưới có thể đạt được và câu trả lời là`1`. 

Điều này chứng tỏ trường hợp chẵn trong đó cả hai cạnh âm có thể bị loại bỏ cùng nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lần quét để tìm các cạnh âm | 
| Không gian | O(n) | Lưu trữ vị trí của họ | 

Tổng của tất cả$n$nhiều nhất là$2 \cdot 10^5$, do đó việc quét tuyến tính cho mỗi trường hợp thử nghiệm dễ dàng đủ nhanh trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline
    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        desc = [i for i in range(n - 1) if a[i] > a[i + 1]]
        m = len(desc)

        if m == 0:
            out.append("0")
        elif m & 1:
            out.append(str((m + 1) // 2))
        else:
            k = m // 2
            out.append(str(k if a[desc[0]] < a[desc[-1] + 1] else k + 1))

    return "\n".join(out)

# provided sample
assert run(
"""4
3
3 2 1
3
3 1 2
4
-2 -5 5 2
7
1 9 1 9 8 1 0
"""
) == """2
1
1
3"""

# already increasing
assert run(
"""1
5
1 2 3 4 5
"""
) == "0"

# minimum size, decreasing
assert run(
"""1
2
2 1
"""
) == "1"

# even number of descents, lower bound achievable
assert run(
"""1
4
-2 -5 5 2
"""
) == "1"

# even number of descents, one extra operation needed
assert run(
"""1
3
3 2 1
"""
) == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 2 3 4 5`|`0`| Đã tăng | 
|`2 1`|`1`| Kích thước tối thiểu | 
|`-2 -5 5 2`|`1`| Trường hợp chẵn, giới hạn dưới có thể đạt được | 
|`3 2 1`|`2`| Trường hợp chẵn, cần thêm thao tác | 

## Vỏ cạnh 

Hãy xem xét:```
3
3 2 1
```Có hai cạnh tiêu cực. Giới hạn dưới là`1`, nhưng các giá trị biên là`3`Và`1`. Từ`3 >= 1`, không có dãy tăng dần nào có thể kết nối chúng. Thuật toán trả về`2`. 

Coi như:```
4
-2 -5 5 2
```Lại có hai cạnh âm, nhưng bây giờ các giá trị biên là`-2`Và`2`. Từ`-2 < 2`, giới hạn dưới có thể đạt được. Thuật toán trả về`1`. 

Coi như:```
5
1 2 3 4 5
```Không có cạnh tiêu cực. Mảng đã tăng lên rất nhiều nên câu trả lời là`0`. 

Coi như:```
3
3 1 2
```Có chính xác một cạnh tiêu cực. Bất kỳ thao tác nào cũng loại bỏ tối đa hai số âm, vì vậy một thao tác là đủ. Công thức đếm lẻ trả về`1`.
