---
title: "CF 2193E - Truy vấn sản phẩm"
description: "Chúng ta được cho một mảng có giá trị nằm trong khoảng từ 1 đến n. Một phần tử có thể được sử dụng lại nhiều lần, vì vậy điều quan trọng duy nhất là giá trị nào có trong mảng chứ không phải chúng xuất hiện bao nhiêu lần."
date: "2026-06-07T20:51:59+07:00"
tags: ["codeforces", "competitive-programming", "dp", "math", "number-theory", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 2193
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 1076 (Div. 3)"
rating: 1300
weight: 2193
solve_time_s: 155
verified: true
draft: false
---

[CF 2193E - Truy vấn sản phẩm](https://codeforces.com/problemset/problem/2193/E) 

**Đánh giá:** 1300 
**Tags:** dp, toán, lý thuyết số, đường đi ngắn nhất 
**Thời gian giải:** 2m 35s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng có giá trị nằm giữa`1`Và`n`. Một phần tử có thể được sử dụng lại nhiều lần, vì vậy điều quan trọng duy nhất là giá trị nào có trong mảng chứ không phải chúng xuất hiện bao nhiêu lần. 

Với mọi số nguyên`i`từ`1`ĐẾN`n`, chúng ta phải tìm số lượng tối thiểu các giá trị mảng đã chọn có tích chính xác`i`. Nếu không thể tạo ra sản phẩm như vậy, chúng ta xuất ra`-1`. 

Một cách hữu ích để xem vấn đề là dùng đồ thị nhân. Nếu một giá trị`k`xuất hiện trong mảng, sau đó từ một sản phẩm`x`chúng ta có thể chuyển đến`x * k`, với điều kiện là kết quả không vượt quá`n`. Mỗi phép nhân sử dụng thêm một phần tử được chọn. 

Tổng số tiền của`n`trên tất cả các trường hợp thử nghiệm là nhiều nhất`3 · 10^5`. Bất kỳ giải pháp nào gần`O(n^2)`ngay lập tức bị loại trừ. Chúng ta cần thứ gì đó xung quanh`O(n log n)`qua một trường hợp thử nghiệm. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Coi như:```
n = 3
a = [1, 1, 1]
```Sản phẩm`1`có thể đạt được với một phần tử đã chọn, vì vậy câu trả lời cho`1`là`1`, không`0`. Câu lệnh yêu cầu chọn ít nhất một phần tử. 

Coi như:```
n = 4
a = [2]
```Sản phẩm`4`có thể đạt được như`2 · 2`, mặc dù mảng chỉ chứa một lần xuất hiện của`2`. Tái sử dụng được cho phép. 

Coi như:```
n = 5
a = [2, 4]
```Sản phẩm`3`là không thể. Một DP bất cẩn cho rằng mọi số đều có thể được tạo từ các số nhỏ hơn sẽ đánh dấu sai số đó là có thể truy cập được. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là coi mọi sản phẩm là một trạng thái và thực hiện tìm kiếm đường đi ngắn nhất. Từ một sản phẩm`x`, hãy thử nhân với mọi giá trị mảng có sẵn. Điều này đúng vì mỗi phép nhân tương ứng với việc chọn thêm một phần tử. 

Vấn đề là số lần chuyển đổi. Nếu có`Θ(n)`các giá trị sẵn có riêng biệt và chúng tôi kiểm tra tất cả chúng từ mọi trạng thái, độ phức tạp sẽ trở thành`Θ(n^2)`, nó quá lớn đối với`n = 3 · 10^5`. 

Quan sát quan trọng nhất là tất cả các sản phẩm đều có nhiều nhất`n`và mọi chuyển đổi đều có dạng```
x -> x * k
```Ở đâu`k`là một giá trị có trong mảng. 

Từ`k ≥ 2`đối với mỗi phép nhân hữu ích, đích sẽ lớn hơn nguồn. Biểu đồ là một DAG có thứ tự tôpô chỉ đơn giản là thứ tự số tăng dần. 

Cho phép`dp[x]`là số lượng tối thiểu các phần tử được chọn cần thiết để có được sản phẩm`x`, giả sử chúng ta bắt đầu từ tích trung tính`1`với chi phí`0`. 

Khi chúng tôi xử lý`x`, mọi số nhân có sẵn`k`tạo ra sự thư giãn:```
dp[x * k] = min(dp[x * k], dp[x] + 1)
```Thay vì lặp lại tất cả các giá trị có sẵn cho mọi trạng thái, chúng tôi lặp lại tất cả các số nhân có thể có`k`lên tới`n / x`và chỉ cần kiểm tra xem`k`có mặt trong mảng. 

Tổng số cặp được kiểm tra là```
Σ floor(n / x)
```đó là`O(n log n)`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng mảng boolean`present`, Ở đâu`present[v]`là đúng nếu giá trị`v`xuất hiện trong mảng. 
2. Tạo một mảng DP được khởi tạo với giá trị vô cùng. 
3. Đặt`dp[1] = 0`. Điều này thể hiện sản phẩm trống trước khi chọn bất kỳ phần tử nào. 
4. Số quy trình`x`từ`1`ĐẾN`n`theo thứ tự tăng dần. 
5. Nếu`dp[x]`vẫn là vô cùng, bỏ qua vì sản phẩm`x`không thể hình thành được. 
6. Với mọi số nhân`k`từ`2`ĐẾN`⌊n / x⌋`:

Nếu như`present[k]`là sự thật, thư giãn đi```
dp[x * k] = min(dp[x * k], dp[x] + 1)
```Phép nhân với`k`tương ứng với việc chọn thêm một phần tử mảng. 
7. Xây dựng câu trả lời. 

Vì`i > 1`, câu trả lời là`dp[i]`nếu nó là hữu hạn, ngược lại`-1`. 

Vì`i = 1`, câu trả lời là:```
1  if present[1]
-1 otherwise
```Chúng tôi không thể xuất`0`vì ít nhất một phần tử phải được chọn. 

### Tại sao nó hoạt động 

Đồ thị chứa một cạnh từ`x`ĐẾN`x * k`bất cứ khi nào giá trị`k`tồn tại trong mảng. Mỗi cạnh đều có chi phí`1`và mọi cạnh đều đi đến một nút lớn hơn. Do đó, thứ tự số tăng dần là một thứ tự tôpô hợp lệ. 

Khi xử lý`x`,`dp[x]`đã bằng độ dài đường đi ngắn nhất từ ​​sản phẩm`1`ĐẾN`x`. Việc thư giãn tất cả các cạnh hướng ra sẽ truyền tải giá trị tối ưu cho các sản phẩm lớn hơn. Vì mọi đường dẫn đều tương ứng chính xác với một chuỗi các phần tử mảng đã chọn,`dp[i]`trở thành số lượng phần tử được chọn tối thiểu có sản phẩm là`i`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        present = [False] * (n + 1)
        for x in a:
            present[x] = True

        INF = 10 ** 9
        dp = [INF] * (n + 1)
        dp[1] = 0

        for x in range(1, n + 1):
            if dp[x] == INF:
                continue

            limit = n // x
            for k in range(2, limit + 1):
                if present[k]:
                    nx = x * k
                    if dp[nx] > dp[x] + 1:
                        dp[nx] = dp[x] + 1

        ans = []

        if present[1]:
            ans.append("1")
        else:
            ans.append("-1")

        for i in range(2, n + 1):
            if dp[i] == INF:
                ans.append("-1")
            else:
                ans.append(str(dp[i]))

        print(" ".join(ans))

solve()
```các`present`mảng loại bỏ tất cả các giá trị trùng lặp. Vì các phần tử có thể được tái sử dụng vô số lần nên chỉ sự tồn tại mới là quan trọng. 

DP bắt đầu từ sản phẩm`1`với chi phí`0`. Đây là trạng thái bắt đầu ảo và bản thân nó không phải là một câu trả lời hợp lệ. Chính vì vậy câu trả lời cho sản phẩm`1`được xử lý riêng biệt. 

Thứ tự vòng lặp là rất quan trọng. Xử lý`x`theo thứ tự tăng dần có tác dụng vì mọi chuyển đổi đều chuyển sang số lượng lớn hơn. Không cần xếp hàng ưu tiên hoặc thư giãn lặp đi lặp lại. 

Một điểm tinh tế khác là số nhân bắt đầu từ`2`. nhân với`1`không bao giờ thay đổi sản phẩm và sẽ tạo ra vòng lặp tự thân. Những chuyển đổi như vậy là vô ích đối với các đường đi ngắn nhất và có thể bị bỏ qua. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
n = 8
a = [3, 2, 2, 3, 7, 3, 6, 7]
```Các giá trị hiện tại là`{2, 3, 6, 7}`. 

| x | dp[x] trước | Thư giãn | 
| --- | --- | --- | 
| 1 | 0 | dp[2]=1, dp[3]=1, dp[6]=1, dp[7]=1 | 
| 2 | 1 | dp[4]=2 | 
| 3 | 1 | dp[6] ở lại 1 | 
| 4 | 2 | dp[8]=3 | 
| 6 | 1 | không có cập nhật hữu ích | 
| 7 | 1 | không có cập nhật hữu ích | 

Câu trả lời cuối cùng:```
-1 1 1 2 -1 1 1 3
```Ví dụ này cho thấy việc sử dụng lặp đi lặp lại cùng một giá trị sẽ tạo ra các sản phẩm như`8 = 2·2·2`. 

### Ví dụ 2 

đầu vào:```
n = 5
a = [1, 2, 3, 4, 5]
```| x | dp[x] trước | Thư giãn | 
| --- | --- | --- | 
| 1 | 0 | dp[2]=1, dp[3]=1, dp[4]=1, dp[5]=1 | 
| 2 | 1 | không cải thiện | 
| 3 | 1 | không cải thiện | 
| 4 | 1 | không cải thiện | 
| 5 | 1 | không cải thiện | 

Vì mọi mục tiêu đều đã xuất hiện trong mảng nên mọi câu trả lời đều`1`.```
1 1 1 1 1
```## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Vòng lặp bên trong thực hiện`Σ floor(n / x)`lặp đi lặp lại | 
| Không gian | O(n) |`present`Và`dp`mảng | 

Giới hạn chuỗi hài hòa cho khoảng`n log n`hoạt động cho mỗi trường hợp thử nghiệm. Với tổng số tiền là`n`giới hạn bởi`3 · 10^5`, điều này dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline
    out = []

    t = int(input())

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        present = [False] * (n + 1)
        for x in a:
            present[x] = True

        INF = 10 ** 9
        dp = [INF] * (n + 1)
        dp[1] = 0

        for x in range(1, n + 1):
            if dp[x] == INF:
                continue

            for k in range(2, n // x + 1):
                if present[k]:
                    dp[x * k] = min(dp[x * k], dp[x] + 1)

        ans = []
        ans.append("1" if present[1] else "-1")

        for i in range(2, n + 1):
            ans.append(str(dp[i]) if dp[i] < INF else "-1")

        out.append(" ".join(ans))

    return "\n".join(out)

# provided sample
assert run(
"""6
8
3 2 2 3 7 3 6 7
5
1 2 3 4 5
3
1 1 1
10
2 1 2 1 3 5 5 7 7 7
4
1 1 2 2
1
1
"""
) == (
"""-1 1 1 2 -1 1 1 3
1 1 1 1 1
1 -1 -1
1 1 1 2 1 2 1 3 2 2
1 1 -1 2
1"""
)

# minimum size
assert run(
"""1
1
1
"""
) == "1"

# only value 2
assert run(
"""1
4
2 2 2 2
"""
) == "-1 1 -1 2"

# only ones
assert run(
"""1
5
1 1 1 1 1
"""
) == "1 -1 -1 -1 -1"

# composite reachable through reuse
assert run(
"""1
8
2 3 2 3 2 3 2 3
"""
) == "-1 1 1 2 -1 2 -1 3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n=1, [1]`|`1`| Trường hợp hợp lệ nhỏ nhất | 
|`n=4, [2,2,2,2]`|`-1 1 -1 2`| Tái sử dụng cùng một giá trị nhiều lần | 
|`n=5, [1,1,1,1,1]`|`1 -1 -1 -1 -1`| Sản phẩm`1`xử lý | 
|`n=8, only 2 and 3 present`|`-1 1 1 2 -1 2 -1 3`| Đường dẫn nhân nhiều bước | 

## Vỏ cạnh 

Hãy xem xét:```
1
3
1 1 1
```DP giữ`dp[1] = 0`, nhưng câu trả lời cuối cùng cho sản phẩm`1`không được lấy từ DP. Thuật toán kiểm tra`present[1]`và đầu ra`1`, thỏa mãn yêu cầu phải chọn ít nhất một phần tử. 

Coi như:```
1
4
2 2 2 2
```Xử lý`x = 1`tạo ra`dp[2] = 1`. Sau này xử lý`x = 2`tạo ra`dp[4] = 2`. Thuật toán cho phép sử dụng chính xác cùng một giá trị mặc dù chỉ có một lần xuất hiện trong mảng. 

Coi như:```
1
5
2 4 4 2 2
```Không có sự chuyển đổi nào có thể đạt tới`3`hoặc`5`. Giá trị DP của chúng vẫn là vô cùng, do đó thuật toán đưa ra`-1`cho cả hai mục tiêu. Điều này ngăn cản việc đánh dấu các sản phẩm không thể truy cập là hợp lệ.
