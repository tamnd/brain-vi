---
title: "CF 1808B - Chơi trong sòng bạc"
description: "Mỗi người chơi nhận được một lá bài chứa m số nguyên. Đối với mỗi cặp người chơi, số tiền thắng trong trò chơi của họ là tổng chênh lệch tuyệt đối giữa các vị trí tương ứng trên lá bài của họ. Nếu người chơi i có giá trị quân bài c[i][1...m] và người chơi j có giá trị quân bài c[j][1..."
date: "2026-06-09T08:55:56+07:00"
tags: ["codeforces", "competitive-programming", "math", "sortings"]
categories: ["algorithms"]
codeforces_contest: 1808
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 861 (Div. 2)"
rating: 1200
weight: 1808
solve_time_s: 84
verified: true
draft: false
---

[CF 1808B - Chơi trong sòng bạc](https://codeforces.com/problemset/problem/1808/B) 

**Đánh giá:** 1200 
**Tags:** toán, sắp xếp 
**Thời gian giải:** 1 phút 24s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi người chơi nhận được một lá bài chứa`m`số nguyên. Đối với mỗi cặp người chơi, số tiền thắng trong trò chơi của họ là tổng chênh lệch tuyệt đối giữa các vị trí tương ứng trên lá bài của họ. 

Nếu người chơi`i`có giá trị thẻ`c[i][1...m]`và người chơi`j`có giá trị thẻ`c[j][1...m]`, đóng góp của họ là:$$\sum_{k=1}^{m} |c[i][k] - c[j][k]|$$Chúng tôi cần tổng số tiền đóng góp cho tất cả các cặp người chơi không có thứ tự. 

Một cách giải thích trực tiếp gợi ý so sánh từng cặp thẻ và mọi vị trí bên trong thẻ. Với`n`người chơi và`m`giá trị trên mỗi thẻ, điều đó sẽ yêu cầu kiểm tra tất cả`n(n-1)/2`cặp và tất cả`m`cột cho mỗi cặp. 

Ràng buộc quan trọng là tổng số mục ma trận trên tất cả các trường hợp thử nghiệm:$$\sum (n \cdot m) \le 3 \cdot 10^5$$Điều này đủ nhỏ cho các thuật toán xung quanh`O(nm log n)`, nhưng quá lớn đối với`O(n^2 m)`khi`n`là lớn. Ví dụ, nếu`n = 3 \cdot 10^5`Và`m = 1`, sau đó một`O(n^2)`cách tiếp cận sẽ yêu cầu khoảng$9 \times 10^{10}$hoạt động. 

Cấu trúc ẩn là tổng của sự khác biệt tuyệt đối có thể phân tách bằng cột. Mỗi cột đóng góp độc lập cho câu trả lời cuối cùng. 

Một số trường hợp khó khăn có thể dễ dàng gây ra sai lầm. 

Coi như:```
2 1
5
5
```Chỉ có một cặp và sự đóng góp của nó là`|5-5| = 0`. Bất kỳ công thức nào giả định giá trị tăng dần sau khi sắp xếp sẽ thêm giá trị dương một cách không chính xác. 

Coi như:```
3 1
1
10
100
```Câu trả lời là:```
|1-10| + |1-100| + |10-100|
= 9 + 99 + 90
= 198
```Việc thực hiện bất cẩn mà chỉ nhìn vào sự khác biệt liền kề sau khi sắp xếp sẽ nhận được`9 + 90 = 99`, tức là chỉ bằng một nửa số tiền cần thiết. 

Coi như:```
1 4
7 8 9 10
```Không có cặp người chơi nào cả. Câu trả lời phải là`0`. Bất kỳ giải pháp nào áp dụng một cách mù quáng các công thức cặp mà không xem xét`n = 1`có thể tạo ra các giá trị rác. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi cặp người chơi, hãy so sánh các quân bài của họ theo tọa độ, tính tổng chênh lệch tuyệt đối và thêm nó vào câu trả lời. 

Điều này hiệu quả vì nó khớp trực tiếp với định nghĩa về tiền thắng. Vấn đề là thời gian chạy:$$O(n^2 m)$$Nếu như`n = 3 \cdot 10^5`Và`m = 1`, điều này trở nên hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là sự đóng góp từ mỗi cột là độc lập. 

Giả sử chúng ta tập trung vào một cột duy nhất và thu thập tất cả các giá trị từ cột đó:$$x_1, x_2, \dots, x_n$$Tổng đóng góp của cột này là:$$\sum_{i<j} |x_i - x_j|$$Đây là một cách diễn đạt cổ điển. Sau khi sắp xếp:$$x_1 \le x_2 \le \cdots \le x_n$$giá trị tuyệt đối biến mất:$$|x_j-x_i| = x_j-x_i \quad (j>i)$$Bây giờ hãy xem xét tần suất mỗi giá trị được sắp xếp xuất hiện tích cực và tiêu cực. 

Đối với vị trí`i`:$$x_i$$xuất hiện chính xác dưới dạng phần tử lớn hơn`i`lần và chính xác là phần tử nhỏ hơn`n-1-i`lần (sử dụng lập chỉ mục dựa trên 0). 

Hệ số ròng của nó trở thành:$$i - (n-1-i)$$Một cách khác để tính tổng tương tự là tính tổng tiền tố. Trong khi quét mảng đã sắp xếp, với mỗi giá trị`x[i]`, tất cả các giá trị trước đó đã được biết đến. 

Đóng góp của nó so với các yếu tố trước đó là:$$i \cdot x[i] - \text{prefix}$$Ở đâu`prefix`là tổng của tất cả các giá trị trước đó. 

Điều này cho phép chúng tôi tính toán toàn bộ phần đóng góp của cột theo thời gian tuyến tính sau khi sắp xếp. 

Vì mỗi cột được xử lý độc lập nên độ phức tạp sẽ trở thành:$$O(m \cdot n \log n)$$dễ dàng phù hợp với các ràng buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²m) | O(1) | Quá chậm | 
| Tối ưu | O(m·n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc ma trận đại diện cho tất cả các thẻ. 
2. Với mỗi cột, hãy thu thập`n`các giá trị thành một mảng. 
3. Sắp xếp các giá trị cột. 

Sau khi sắp xếp, mỗi đóng góp theo cặp sẽ trở thành một phép trừ đơn giản thay vì một giá trị tuyệt đối. 
4. Khởi tạo`prefix = 0`. 
5. Quét cột được sắp xếp từ trái sang phải. 

Đối với giá trị hiện tại`x[i]`, tất cả các giá trị trước đó đều nhỏ hơn hoặc bằng nhau. 
6. Thêm$$i \cdot x[i] - prefix$$để trả lời. 

Điều này bằng tổng của:$$x[i]-x[0],\ x[i]-x[1],\ \dots,\ x[i]-x[i-1]$$đó chính xác là sự đóng góp của`x[i]`với tất cả các phần tử trước đó. 
7. Cập nhật$$prefix += x[i]$$để các yếu tố trong tương lai có thể sử dụng nó. 
8. Lặp lại cho mỗi cột. 
9. Xuất ra câu trả lời tích lũy. 

### Tại sao nó hoạt động 

Sau khi sắp xếp một cột, mỗi cặp`(i,j)`với`i < j`đóng góp`x[j] - x[i]`. Khi xử lý`x[j]`, số lượng$$j \cdot x[j] - \text{prefix}$$thêm chính xác$$(x[j]-x[0]) + (x[j]-x[1]) + \cdots + (x[j]-x[j-1])$$đó là tổng đóng góp của tất cả các cặp kết thúc ở vị trí`j`. 

Mỗi cặp không có thứ tự xuất hiện chính xác một lần, cụ thể là khi điểm cuối được sắp xếp lớn hơn của nó được xử lý. Không có cặp nào bị bỏ qua và không có cặp nào được tính hai lần. Tổng giá trị này trên tất cả các vị trí và tất cả các cột sẽ mang lại chính xác tổng số tiền thắng cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

t = int(input())

for _ in range(t):
    n, m = map(int, input().split())

    a = [list(map(int, input().split())) for _ in range(n)]

    ans = 0

    for col in range(m):
        vals = [a[row][col] for row in range(n)]
        vals.sort()

        prefix = 0

        for i, x in enumerate(vals):
            ans += i * x - prefix
            prefix += x

    print(ans)
```Ma trận được đọc đúng một lần. Đối với mỗi cột, chúng tôi trích xuất các giá trị của nó, sắp xếp chúng và tính tổng chênh lệch theo cặp bằng cách sử dụng tổng tiền tố đang chạy. 

Biểu thức:```
ans += i * x - prefix
```là đường tới hạn. Tại chỉ mục`i`, có chính xác`i`các phần tử trước đó. Tổng đóng góp của họ so với`x`là:```
(x - v1) + (x - v2) + ... + (x - vi)
```mà đơn giản hóa thành:```
i * x - (v1 + v2 + ... + vi)
```và số tiền đó được lưu trữ trong`prefix`. 

Số nguyên Python tự động xử lý các giá trị lớn, do đó không có rủi ro tràn. Trong các ngôn ngữ như C++, số nguyên 64 bit sẽ được yêu cầu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 5
1 4 2 8 5
7 9 2 1 4
3 8 5 3 1
```Giá trị cột 1 là`[1, 7, 3]`. 

Sau khi sắp xếp:```
[1, 3, 7]
```| tôi | x | tiền tố trước | Giá trị gia tăng | Chạy câu trả lời | 
| --- | --- | --- | --- | --- | 
| 0 | 1 | 0 | 0 | 0 | 
| 1 | 3 | 1 | 2 | 2 | 
| 2 | 7 | 4 | 10 | 12 | 

Cột 1 đóng góp`12`. 

Giá trị cột 2 là`[4, 9, 8]`. 

Đã sắp xếp:```
[4, 8, 9]
```| tôi | x | tiền tố trước | Giá trị gia tăng | Tổng số cột | 
| --- | --- | --- | --- | --- | 
| 0 | 4 | 0 | 0 | 0 | 
| 1 | 8 | 4 | 4 | 4 | 
| 2 | 9 | 12 | 6 | 10 | 

Cột 2 đóng góp`10`. 

Lặp lại cho cả năm cột mang lại:```
12 + 10 + 6 + 14 + 8 = 50
```phù hợp với đầu ra mẫu. 

Dấu vết này cho thấy cách mỗi cột được xử lý độc lập và cách công thức tổng tiền tố tái tạo tất cả các khác biệt theo cặp. 

### Ví dụ 2 

đầu vào:```
4 3
1 2 3
3 2 1
1 2 1
4 2 7
```Cột đầu tiên:```
[1,3,1,4]
→ [1,1,3,4]
```| tôi | x | tiền tố trước | Đã thêm | 
| --- | --- | --- | --- | 
| 0 | 1 | 0 | 0 | 
| 1 | 1 | 1 | 0 | 
| 2 | 3 | 2 | 4 | 
| 3 | 4 | 5 | 7 | 

Đóng góp =`11`. 

Cột thứ hai:```
[2,2,2,2]
```| tôi | x | tiền tố trước | Đã thêm | 
| --- | --- | --- | --- | 
| 0 | 2 | 0 | 0 | 
| 1 | 2 | 2 | 0 | 
| 2 | 2 | 4 | 0 | 
| 3 | 2 | 6 | 0 | 

Đóng góp =`0`. 

Cột thứ ba:```
[3,1,1,7]
→ [1,1,3,7]
```| tôi | x | tiền tố trước | Đã thêm | 
| --- | --- | --- | --- | 
| 0 | 1 | 0 | 0 | 
| 1 | 1 | 1 | 0 | 
| 2 | 3 | 2 | 4 | 
| 3 | 7 | 5 | 16 | 

Đóng góp =`20`. 

Câu trả lời cuối cùng:```
11 + 0 + 20 = 31
```Ví dụ này nhấn mạnh rằng các cột có tất cả các giá trị bằng nhau đương nhiên đóng góp bằng 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m·n log n) | Mỗi cột có kích thước`n`được sắp xếp một lần | 
| Không gian | O(n) | Mảng tạm thời lưu trữ một cột | 

Tổng số mục ma trận trên tất cả các trường hợp thử nghiệm nhiều nhất là`3·10^5`. Việc sắp xếp từng cột mang lại tổng độ phức tạp thoải mái trong giới hạn và bộ nhớ bổ sung chỉ là một mảng cột duy nhất. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    t = int(input())
    out = []

    for _ in range(t):
        n, m = map(int, input().split())
        a = [list(map(int, input().split())) for _ in range(n)]

        ans = 0

        for col in range(m):
            vals = [a[row][col] for row in range(n)]
            vals.sort()

            pref = 0
            for i, x in enumerate(vals):
                ans += i * x - pref
                pref += x

        out.append(str(ans))

    sys.stdout.write("\n".join(out))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    res = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout

    return res

# provided samples
assert run(
"""3
3 5
1 4 2 8 5
7 9 2 1 4
3 8 5 3 1
1 4
4 15 1 10
4 3
1 2 3
3 2 1
1 2 1
4 2 7
"""
) == "50\n0\n31"

# minimum size
assert run(
"""1
1 1
5
"""
) == "0"

# all equal
assert run(
"""1
3 2
7 7
7 7
7 7
"""
) == "0"

# single column
assert run(
"""1
3 1
1
10
100
"""
) == "198"

# off-by-one style check
assert run(
"""1
2 2
1 2
3 4
"""
) == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1x1`ma trận |`0`| Không có cặp người chơi nào tồn tại | 
| Tất cả các giá trị bằng nhau |`0`| Giá trị bằng nhau không đóng góp gì | 
| Cột đơn`[1,10,100]`|`198`| Sửa lỗi tích lũy chênh lệch theo cặp | 
| Hai hàng`[[1,2],[3,4]]`|`4`| Mỗi cặp được tính đúng một lần | 

## Vỏ cạnh 

### Người chơi đơn 

đầu vào:```
1
1 4
7 8 9 10
```Mỗi cột chỉ chứa một giá trị. 

Với mỗi cột, sau khi sắp xếp chỉ có một phần tử: 

| tôi | x | tiền tố | Đã thêm | 
| --- | --- | --- | --- | 
| 0 | x | 0 | 0 | 

Không có cặp nào tồn tại nên mỗi cột đều đóng góp bằng 0. Câu trả lời cuối cùng là:```
0
```### Tất cả các giá trị bằng nhau 

đầu vào:```
1
3 1
5
5
5
```Cột được sắp xếp:```
[5,5,5]
```| tôi | x | tiền tố trước | Đã thêm | 
| --- | --- | --- | --- | 
| 0 | 5 | 0 | 0 | 
| 1 | 5 | 5 | 0 | 
| 2 | 5 | 10 | 0 | 

Câu trả lời vẫn là 0 vì mỗi cặp khác nhau đều bằng 0. 

### Khoảng cách lớn giữa các giá trị 

đầu vào:```
1
3 1
1
10
100
```Cột được sắp xếp:```
[1,10,100]
```| tôi | x | tiền tố trước | Đã thêm | 
| --- | --- | --- | --- | 
| 0 | 1 | 0 | 0 | 
| 1 | 10 | 1 | 9 | 
| 2 | 100 | 11 | 189 | 

Tổng cộng:```
9 + 189 = 198
```bằng:```
|1-10| + |1-100| + |10-100|
= 9 + 99 + 90
= 198
```Điều này xác nhận rằng công thức tổng tiền tố nắm bắt được mọi khác biệt theo cặp, không chỉ những khác biệt liền kề theo thứ tự được sắp xếp.
