---
title: "CF 2124H - Chuỗi tốt dài nhất"
description: "Chúng ta được cho một mảng a. Chúng ta muốn chọn một dãy con và diễn giải lại nó thành một mảng mới b. Mục tiêu là tối đa hóa độ dài của b, với điều kiện b là một mảng tốt. Định nghĩa về lòng tốt được viết dưới dạng hoán vị và phạm vi tối thiểu."
date: "2026-06-08T03:34:55+07:00"
tags: ["codeforces", "competitive-programming", "dp", "math", "trees"]
categories: ["algorithms"]
codeforces_contest: 2124
codeforces_index: "H"
codeforces_contest_name: "EPIC Institute of Technology Round Summer 2025 (Codeforces Round 1036, Div. 1 + Div. 2)"
rating: 3400
weight: 2124
solve_time_s: 111
verified: false
draft: false
---

[CF 2124H - Chuỗi tốt dài nhất](https://codeforces.com/problemset/problem/2124/H) 

**Đánh giá:** 3400 
**Tags:** dp, toán, cây 
**Thời gian giải:** 1 phút 51 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một mảng`a`. Chúng tôi muốn chọn một chuỗi con và diễn giải lại nó dưới dạng một mảng mới`b`. Mục đích là tối đa hóa chiều dài của`b`, tùy thuộc vào`b`là một mảng _good_. 

Định nghĩa về lòng tốt được viết dưới dạng hoán vị và phạm vi tối thiểu. Thoạt nhìn nó có vẻ không liên quan đến các dãy con, nhưng nhiệm vụ thực sự là phải hiểu mảng nào`b`thực sự có thể xuất hiện từ công trình đó. 

giá trị`b_i`đại diện cho vị trí ngoài cùng bên trái của một đoạn kết thúc tại`i`cực tiểu của nó xảy ra chính xác tại vị trí`i`. Vì hoán vị chứa các giá trị riêng biệt nên mọi so sánh đều nghiêm ngặt. Cấu trúc ẩn trong định nghĩa hóa ra lại quan trọng hơn nhiều so với bản thân hoán vị. 

Độ dài của mảng ban đầu nhiều nhất là`15000`, và tổng của`n²`trên tất cả các trường hợp thử nghiệm là nhiều nhất`15000²`. Đây là một gợi ý rất mạnh mẽ. Một giải pháp có độ phức tạp xung quanh`O(n²)`là dự định. Một thuật toán khối sẽ yêu cầu khoảng`3·10¹²`hoạt động trong trường hợp xấu nhất và hoàn toàn không khả thi. Một giải pháp quy hoạch động bậc hai, về`2.25·10⁸`các thao tác đơn giản trong tất cả các thử nghiệm, chính xác là phạm vi mà các ràng buộc này được thiết kế cho. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Coi như:```
a = [2,2,2,2]
```Một ý tưởng hấp dẫn là các giá trị bằng nhau luôn có thể được xâu chuỗi lại với nhau. Họ không thể. Bất kỳ mảng tốt nào cũng phải chứa một vị trí có giá trị bằng chỉ số của chính nó. Vì mọi giá trị đều`2`, không tồn tại dãy con tốt khác rỗng. Câu trả lời đúng là`0`. 

Coi như:```
a = [1]
```Phần tử đơn đã đáp ứng được cấu trúc được yêu cầu. Câu trả lời là`1`. 

Coi như:```
a = [1,1,2]
```Toàn bộ trình tự không tốt. Một sự kiểm tra ngây thơ chỉ dựa trên sự bất bình đẳng cục bộ sẽ chấp nhận nó một cách sai lầm. Dãy con tốt dài nhất là`[1,2]`, có độ dài là`2`. 

Những ví dụ này cho thấy khó khăn then chốt không phải là lựa chọn vị trí mà là xác định đặc điểm của trình tự nào thực sự tốt. 

## Phương pháp tiếp cận 

Ý tưởng về vũ lực rất đơn giản. Liệt kê mọi dãy con, kiểm tra xem nó có tốt không và giữ độ dài tối đa. 

Đối với một mảng có độ dài`n`, có`2^n`các chuỗi tiếp theo. Ngay cả đối với`n = 60`điều này đã vô vọng rồi, và đây`n`có thể đạt được`15000`. Cách tiếp cận bạo lực chỉ đóng vai trò là điểm khởi đầu mang tính khái niệm. 

Bước đột phá thực sự đến từ việc hiểu cấu trúc của các mảng tốt. 

Cho phép`b_i = x`. Nhìn vào cách giải thích phạm vi tối thiểu, vị trí`x`bản thân nó phải là khởi đầu của một khối hợp lệ. Sau khi giải mã định nghĩa, người ta thu được đặc tính sau: 

Một trình tự`b`là tốt khi và chỉ nếu với mọi vị trí`i`với`b_i = x`, một trong các khẳng định sau: 

1.`x = i`. 
2.`b_x = x`và mọi giá trị trên đoạn`[x, i]`ít nhất là`x`. 

Điều này loại bỏ hoàn toàn hoán vị khỏi vấn đề. Chúng ta chỉ cần lý luận về các chỉ số và giá trị. 

Bây giờ hãy nghĩ đến mọi vị trí`x`thỏa mãn`b_x = x`như là gốc của một khối. Bất kỳ sự xuất hiện sau này của giá trị`x`phải thuộc về khối đó và tất cả các giá trị bên trong khối ít nhất phải bằng`x`. 

Điều này tự nhiên dẫn đến lập trình động theo khoảng. Chúng tôi xử lý các gốc khối có thể có và mở rộng chúng sang bên phải. Bất cứ khi nào chúng ta gặp một giá trị lớn hơn, chúng ta có thể bắt đầu một khối lồng nhau. Quan sát quan trọng là vị trí bắt đầu của khối lồng nhau như vậy nên được chọn càng sớm càng tốt. Điều đó cho phép tất cả các chuyển đổi được thực hiện theo thời gian bậc hai. 

DP thu được hoàn toàn phù hợp với`Σ n² ≤ 15000²`hạn chế. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(2^n · n)`|`O(n)`| Quá chậm | 
| DP tối ưu |`O(n²)`|`O(n²)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### Định nghĩa trạng thái 

hãy để`dp[i][j]`biểu thị độ dài tối đa của một dãy con tốt có thể được hình thành trong khoảng`[i, j]`theo các điều kiện: 

- vị trí`i`được chọn, 
- giá trị của nó hoạt động như một khối gốc, 
- vị trí`j`là lần xuất hiện cuối cùng của giá trị gốc đó bên trong dãy con được xây dựng. 

Việc thực hiện biên tập lưu trữ chính xác số lượng này. 

### Thi công chuyển tiếp 

1. Vị trí bắt đầu xử lý`i`từ phải sang trái. 
2. Khởi tạo khối có gốc tại`i`. 

Dãy con hợp lệ nhỏ nhất chỉ bao gồm các lần xuất hiện của`a[i]`. Ban đầu:```
dp[i][i] = a[i]
```Biến`curr`lưu trữ giá trị tốt nhất thu được cho đến nay. 
3. Mở rộng điểm cuối phù hợp`j`. 
4. Nếu`a[j] < a[i]`, phần tử không thể thuộc về khối hiện tại. 

Việc mô tả đặc tính yêu cầu mọi giá trị bên trong khối ít nhất phải là giá trị gốc. 
5. Nếu`a[j] = a[i]`, chúng tôi có thể thêm sự kiện này miễn phí. 

Tăng`curr`bằng một và đặt:```
dp[i][j] = curr
```6. Nếu`a[j] > a[i]`, phần tử có thể trở thành gốc của khối lồng nhau. 

Duy trì một mảng`good[v]`.`good[v]`lưu trữ vị trí sớm nhất có giá trị là`v`và giá trị DP của nó đã đủ lớn để hỗ trợ một khối loại`v`. 
7. Khi một khối lồng nhau như vậy tồn tại, hãy hợp nhất nó vào cấu trúc hiện tại:```
curr = max(curr, dp[idx][j])
```Ở đâu`idx = good[a[j]]`. 
8. Tiếp tục cho đến khi tất cả các điểm cuối đã được xử lý. 

### Trích xuất câu trả lời 

Sau khi tất cả các trạng thái khoảng được tính toán, hãy chạy logic chuyển đổi tương tự một lần nữa trên toàn bộ mảng. 

Bất cứ khi nào có một root hợp lệ, hãy sử dụng nó`dp`value để cập nhật câu trả lời toàn cầu. 

Giá trị cuối cùng của`curr`là độ dài của dãy con tốt dài nhất. 

### Tại sao nó hoạt động 

Đặc tính của các mảng tốt nói lên rằng mỗi lần xuất hiện của giá trị`x`phải thuộc về một khối bắt nguồn từ vị trí có giá trị cũng bằng`x`. 

Bên trong khối như vậy, tất cả các giá trị ít nhất phải bằng`x`. Điều này có nghĩa là các giá trị nhỏ hơn không bao giờ có thể được thêm vào, các giá trị bằng nhau luôn mở rộng khối và các giá trị lớn hơn chỉ có thể xuất hiện thông qua các khối lồng nhau có gốc đã thỏa mãn cùng một thuộc tính theo cách đệ quy. 

Trạng thái DP lưu trữ chính xác độ dài tốt nhất có thể đạt được cho gốc khối và lần xuất hiện cuối cùng cố định. Mọi chuỗi con hợp lệ đều có thể được phân tách thành các khối lồng nhau này và mọi chuyển đổi được thực hiện bởi DP đều tương ứng với một phân tách hợp pháp. Vì nghiệm khả thi sớm nhất luôn là tối ưu cho các phần mở rộng trong tương lai nên lựa chọn tham lam được sử dụng bởi`good[]`không bao giờ mất một giải pháp tốt hơn. 

Do đó, mọi chuỗi con tốt hợp lệ đều được xem xét và mọi chuỗi con do DP tạo ra đều thỏa mãn đặc tính. Thuật toán là chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = [0] + list(map(int, input().split()))

    dp = [[0] * (n + 1) for _ in range(n + 1)]

    for i in range(n, 0, -1):
        good = [-1] * (n + 1)

        curr = a[i]
        dp[i][i] = curr

        for j in range(i + 1, n + 1):
            if a[j] < a[i]:
                continue

            if a[j] == a[i]:
                curr += 1
                dp[i][j] = curr
                continue

            if good[a[j]] == -1 and a[j] <= curr + 1:
                good[a[j]] = j

            if good[a[j]] != -1:
                idx = good[a[j]]
                curr = max(curr, dp[idx][j])

    curr = 0
    good = [-1] * (n + 1)

    for j in range(1, n + 1):
        if good[a[j]] == -1 and a[j] <= curr + 1:
            good[a[j]] = j

        if good[a[j]] != -1:
            idx = good[a[j]]
            curr = max(curr, dp[idx][j])

    print(curr)

t = int(input())
for _ in range(t):
    solve()
```Vòng lặp bên ngoài tính toán mọi trạng thái khoảng thời gian`dp[i][j]`. Xử lý`i`từ phải sang trái đảm bảo rằng tất cả các khối lồng nhau cần thiết cho quá trình chuyển đổi trong tương lai đã được tính toán. 

Mảng`good`được xây dựng lại cho mọi vị trí bắt đầu. Nó ghi lại gốc có thể sử dụng sớm nhất cho mỗi giá trị. Chỉ lưu trữ vị trí đầu tiên như vậy là đủ vì các gốc trước chiếm ưu thế hơn các gốc sau. 

Một điểm tinh tế là điều kiện:```
a[j] <= curr + 1
```Một khối bắt nguồn từ giá trị`v`yêu cầu ít nhất`v - 1`các vị trí đã được xây dựng trước khi nó có thể bắt đầu. Điều kiện này chính xác là kiểm tra tính khả thi. 

Một chi tiết tinh tế khác là các mục có`a[j] < a[i]`được bỏ qua hoàn toàn. Cho phép chúng sẽ vi phạm định lý mô tả đặc tính và tạo ra các khối không hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
[1,1,3,3,5]
```Sự phát triển DP chính: 

| tôi | j | một[j] | hiện tại | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 
| 1 | 2 | 1 | 2 | 
| 1 | 3 | 3 | 2 | 
| 1 | 4 | 3 | 4 | 
| 1 | 5 | 5 | 5 | 

Câu trả lời cuối cùng:```
5
```Toàn bộ chuỗi đã đáp ứng được cấu trúc khối. Mỗi giá trị lớn hơn sẽ bắt đầu một khối lồng nhau và tất cả các điều kiện vẫn hợp lệ. 

### Ví dụ 2 

đầu vào:```
[1,1,2]
```Sự phát triển chính của giai đoạn toàn cầu: 

| j | một[j] | hiện tại trước | hiện sau | 
| --- | --- | --- | --- | 
| 1 | 1 | 0 | 1 | 
| 2 | 1 | 1 | 2 | 
| 3 | 2 | 2 | 2 | 

Câu trả lời cuối cùng:```
2
```Trình tự`[1,1,2]`không tốt, nhưng`[1,2]`là. DP chính xác từ chối xây dựng cấu trúc không hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n²)`| Mỗi cặp`(i, j)`được xử lý một lần | 
| Không gian |`O(n²)`| Bảng kích thước DP`(n+1)²`| 

Vấn đề đảm bảo một cách rõ ràng rằng tổng của`n²`trên tất cả các trường hợp thử nghiệm không vượt quá`15000²`. MỘT`O(n²)`giải pháp lập trình động chính xác là những gì những ràng buộc này được thiết kế để hỗ trợ và việc sử dụng bộ nhớ cũng vừa vặn thoải mái trong giới hạn 1024 MB. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    from io import StringIO

    data = StringIO(inp)
    out = StringIO()

    input = data.readline

    t = int(input())

    for _ in range(t):
        n = int(input())
        a = [0] + list(map(int, input().split()))

        dp = [[0] * (n + 1) for _ in range(n + 1)]

        for i in range(n, 0, -1):
            good = [-1] * (n + 1)
            curr = a[i]
            dp[i][i] = curr

            for j in range(i + 1, n + 1):
                if a[j] < a[i]:
                    continue

                if a[j] == a[i]:
                    curr += 1
                    dp[i][j] = curr
                    continue

                if good[a[j]] == -1 and a[j] <= curr + 1:
                    good[a[j]] = j

                if good[a[j]] != -1:
                    curr = max(curr, dp[good[a[j]]][j])

        curr = 0
        good = [-1] * (n + 1)

        for j in range(1, n + 1):
            if good[a[j]] == -1 and a[j] <= curr + 1:
                good[a[j]] = j

            if good[a[j]] != -1:
                curr = max(curr, dp[good[a[j]]][j])

        out.write(str(curr) + "\n")

    return out.getvalue()

# provided sample
assert run(
"""5
5
1 1 3 3 5
3
1 1 2
4
2 2 2 2
7
1 2 4 2 4 6 2
1
1
"""
) == """5
2
0
5
1
"""

# minimum size
assert run(
"""1
1
1
"""
) == """1
"""

# all equal but impossible
assert run(
"""1
4
2 2 2 2
"""
) == """0
"""

# single valid chain
assert run(
"""1
4
1 2 3 4
"""
) == """4
"""

# off-by-one style case
assert run(
"""1
3
1 1 2
"""
) == """2
"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`[1]`|`1`| Ví dụ không trống nhỏ nhất | 
|`[2,2,2,2]`|`0`| Không tồn tại vị trí gốc hợp lệ | 
|`[1,2,3,4]`|`4`| Toàn bộ mảng tạo thành một cấu trúc hợp lệ | 
|`[1,1,2]`|`2`| Từ chối một chuỗi đầy đủ hấp dẫn nhưng không hợp lệ | 

## Vỏ cạnh 

Hãy xem xét:```
1
4
2 2 2 2
```Trong giai đoạn toàn cầu, giá trị`2`không bao giờ có thể trở thành gốc hợp lệ vì độ dài có thể đạt được hiện tại không bao giờ đạt tới`1`trước khi phải tạo root. Không có trạng thái DP nào có thể sử dụng được và câu trả lời vẫn là`0`. Điều này phù hợp với đặc tính, vì một chuỗi tốt khác trống phải chứa một số vị trí`i`có giá trị`i`. 

Coi như:```
1
1
1
```Vị trí duy nhất đồng thời là gốc và cuối khối của nó. DP khởi tạo`dp[1][1] = 1`, và pha toàn cục ngay lập tức khôi phục câu trả lời`1`. 

Coi như:```
1
3
1 1 2
```Hai phần tử đầu tiên tạo một khối bắt nguồn từ giá trị`1`. Khi giá trị cuối cùng`2`được kiểm tra, DP phát hiện ra rằng cấu trúc lồng nhau được yêu cầu không tồn tại cho toàn bộ chuỗi. Dãy con hợp lệ tốt nhất vẫn có độ dài`2`, tương ứng với`[1,2]`. 

Những ví dụ này minh họa hai cạm bẫy phổ biến nhất: giả sử các giá trị bằng nhau luôn có thể được xâu chuỗi và bỏ qua yêu cầu rằng mọi khối phải bắt nguồn từ một gốc hợp lệ.
