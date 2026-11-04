---
title: "CF 1120C - Nén chuỗi"
description: "Chúng tôi được cung cấp một chuỗi và hai chi phí. Thao tác đầu tiên cho phép chúng ta mã hóa chính xác một ký tự với chi phí là a."
date: "2026-06-12T04:24:56+07:00"
tags: ["codeforces", "competitive-programming", "dp", "strings"]
categories: ["algorithms"]
codeforces_contest: 1120
codeforces_index: "C"
codeforces_contest_name: "Codeforces Round 543 (Div. 1, based on Technocup 2019 Final Round)"
rating: 2100
weight: 1120
solve_time_s: 102
verified: true
draft: false
---

[CF 1120C - Nén chuỗi](https://codeforces.com/problemset/problem/1120/C) 

**Xếp hạng:** 2100 
**Thẻ:** dp, chuỗi 
**Thời gian giải:** 1 phút 42s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một chuỗi và hai chi phí. 

Thao tác đầu tiên cho phép chúng ta mã hóa chính xác một ký tự với chi phí là`a`. 

Hoạt động thứ hai cho phép chúng ta mã hóa bất kỳ chuỗi con không trống nào với giá cố định là`b`, nhưng chỉ khi chuỗi con đó đã xuất hiện ở đâu đó trong phần chuỗi đã được mã hóa. 

Chuỗi phải được xử lý từ trái sang phải. Tại mọi vị trí, chúng tôi quyết định có viết trực tiếp ký tự tiếp theo hay không hoặc liệu chúng tôi có thể sao chép chuỗi con dài hơn đã tồn tại trong tiền tố được xử lý trước đó hay không. 

Mục tiêu là giảm thiểu tổng chi phí cần thiết để mã hóa toàn bộ chuỗi. 

Độ dài của chuỗi tối đa là 5000. Điều này ngay lập tức loại trừ bất kỳ thứ gì có hàm mũ hoặc thậm chí bậc ba có hằng số lớn. Một giải pháp lập trình động với khoảng$O(n^2)$công việc thường được chấp nhận$n=5000$, trong khi$O(n^3)$sẽ quá chậm vì nó đòi hỏi khoảng$1.25 \times 10^{11}$hoạt động trong trường hợp xấu nhất. 

Phần khó khăn là xác định, đối với mọi vị trí, một chuỗi con có thể được sao chép từ tiền tố đã được xử lý trong bao lâu. Việc triển khai ngây thơ liên tục so sánh các chuỗi con sẽ dễ dàng trở thành khối. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Coi như:```
3 3 1
aba
```cuối cùng`'a'`có thể bị sao chép vì nó đã xuất hiện trước đó. Câu trả lời tối ưu là`7`, không`9`. 

Coi như:```
4 1 100
aaaa
```Việc sao chép cực kỳ tốn kém. Mặc dù các chuỗi con lặp lại tồn tại ở mọi nơi nhưng câu trả lời tối ưu là`4`, thu được bằng cách viết từng ký tự riêng lẻ. 

Coi như:```
4 10 1
aaaa
```Bây giờ sao chép là rẻ. Một cách xây dựng tối ưu là:```
"a"  -> 10
"a"  -> 1
"aa" -> 1
```Tổng chi phí`12`. 

Một lỗi phổ biến là cho rằng chuỗi con được sao chép phải xuất hiện dưới dạng khối đã chọn trước đó. Vấn đề chỉ yêu cầu nó phải là chuỗi con của tiền tố đã được mã hóa. Bất kỳ sự xuất hiện nào bên trong tiền tố là đủ. 

Một lỗi phổ biến khác là cho phép các bản sao chồng lên các ký tự trong tương lai. Khi mã hóa vị trí`i`, sự xuất hiện nguồn phải nằm hoàn toàn bên trong tiền tố đã được xử lý`s[0..i-1]`. 

## Phương pháp tiếp cận 

Ý tưởng lập trình động trực tiếp nhất là cho phép`dp[i]`biểu thị chi phí tối thiểu để mã hóa lần đầu tiên`i`nhân vật. 

Từ vị trí`i`, chúng ta luôn có thể mã hóa một ký tự và chuyển sang`i+1`với chi phí`a`. 

Chúng ta cũng có thể thử mọi chuỗi con có thể bắt đầu từ`i`, kiểm tra xem nó có xuất hiện trước đó không và nếu có, hãy chuyển trực tiếp đến cuối chuỗi con đó với cost`b`. 

DP này đúng vì mọi mã hóa hợp lệ đều tương ứng với một chuỗi chuyển tiếp giữa các độ dài tiền tố. 

Vấn đề là kiểm tra xem chuỗi con đã xuất hiện chưa. Nếu chúng ta thử mọi vị trí bắt đầu, mọi độ dài và so sánh trực tiếp các chuỗi, chúng ta thu được khoảng$O(n^3)$công việc. 

Quan sát quan trọng là đối với mọi vị trí`i`, chúng ta chỉ cần một số:`mx[i] = maximum length of a substring starting at i that already occurs somewhere before i`. 

Khi giá trị này được biết đến, DP sẽ trở nên đơn giản. Nếu như`mx[i] = L`, thì mọi độ dài từ`1`ĐẾN`L`có thể được sao chép với chi phí`b`. Vì chi phí không phụ thuộc vào độ dài nên lựa chọn tốt nhất luôn là sao chép chuỗi con dài nhất hiện có. 

Thử thách còn lại là tính toán tất cả`mx[i]`. 

Đối với mỗi cặp vị trí`(i, j)`với`j < i`, chúng tôi tính toán tiền tố chung dài nhất của các hậu tố bắt đầu từ`i`Và`j`. 

Cho phép:```
lcp[i][j] = longest common prefix length of s[i:] and s[j:]
```Sau đó, bất kỳ sự xuất hiện nào bắt đầu từ`j`cung cấp một chuỗi con có độ dài có thể sao chép:```
min(lcp[i][j], i - j)
```Thuật ngữ thứ hai là rất quan trọng. Sự xuất hiện nguồn phải nằm hoàn toàn bên trong tiền tố đã được xử lý. Nếu trận đấu kéo dài quá vị trí`i-1`, chúng ta không thể sử dụng phần đó. 

Tận dụng tối đa tất cả các vị trí trước đó`j`cho`mx[i]`. 

Bảng LCP có thể được tính toán trong$O(n^2)$sử dụng DP ngược cổ điển:```
if s[i] == s[j]:
    lcp[i][j] = lcp[i+1][j+1] + 1
else:
    lcp[i][j] = 0
```Một lần`mx`đã biết, DP trở thành$O(n^2)$, dẫn đến tổng thể$O(n^2)$giải pháp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n³) | O(n²) | Quá chậm | 
| Tối ưu | O(n²) | O(n²) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc chuỗi và tạo một`n × n`bảng LCP. 
2. Điền vào bảng từ dưới cùng bên phải đến trên cùng bên trái. 

Nếu như`s[i] == s[j]`, thì độ dài tiền tố chung bằng`1 + lcp[i+1][j+1]`. Nếu không thì nó bằng không. 
3. Đối với mọi vị trí`i`, tính độ dài có thể sao chép dài nhất`mx[i]`. 

Kiểm tra mọi vị trí trước đó`j < i`. 

Độ dài phù hợp với hậu tố là`lcp[i][j]`, nhưng chỉ là lần đầu tiên`i-j`các ký tự hoàn toàn thuộc về tiền tố đã được mã hóa. Do đó chiều dài có thể sử dụng là:```
min(lcp[i][j], i-j)
```Tận dụng tối đa tất cả`j`. 
4. Tạo mảng DP`dp`, Ở đâu`dp[i]`là chi phí tối thiểu để mã hóa lần đầu tiên`i`nhân vật. 
5. Khởi tạo:```
dp[0] = 0
```6. Đối với mọi vị trí`i`: 

Mã hóa một ký tự:```
dp[i+1] = min(dp[i+1], dp[i] + a)
```7. Nếu`mx[i] > 0`, sao chép chuỗi con dài nhất hiện có:```
dp[i + mx[i]] = min(dp[i + mx[i]], dp[i] + b)
```Vì tất cả các độ dài bản sao đều có giá như nhau nên việc sử dụng độ dài tối đa sẽ chiếm ưu thế ở tất cả các độ dài ngắn hơn. 
8. Đầu ra`dp[n]`. 

### Tại sao nó hoạt động 

Bảng LCP đo lường chính xác số lượng ký tự liên tiếp khớp giữa hai hậu tố. 

Đối với một vị trí`i`, mọi khối được sao chép hợp lệ phải đến từ một số lần xuất hiện trước đó bắt đầu từ`j < i`. Khối dài nhất như vậy có chiều dài`min(lcp[i][j], i-j)`, vì chỉ các ký tự hoàn toàn bên trong tiền tố đã được mã hóa mới có thể đóng vai trò là nguồn. Tận dụng tối đa tất cả các vị trí trước đó mang lại chính xác thao tác sao chép dài nhất hiện có tại`i`. 

DP xem xét mọi cách có thể để mở rộng mã hóa tiền tố vốn đã tối ưu. Một chuyển đổi tương ứng với việc viết một ký tự đơn và chuyển đổi còn lại tương ứng với việc sử dụng thao tác sao chép tốt nhất hiện có. Vì chi phí sao chép không phụ thuộc vào độ dài được sao chép nên bản sao ngắn hơn không bao giờ có thể cải thiện được so với bản sao dài hơn bắt đầu ở cùng một vị trí. Mọi mã hóa hợp lệ được biểu thị bằng một chuỗi chuyển đổi DP và mỗi chuyển đổi DP thể hiện một hành động mã hóa hợp lệ. Như vậy`dp[n]`bằng tổng chi phí tối thiểu có thể. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, a, b = map(int, input().split())
    s = input().strip()

    lcp = [[0] * (n + 1) for _ in range(n + 1)]

    for i in range(n - 1, -1, -1):
        si = s[i]
        row = lcp[i]
        next_row = lcp[i + 1]

        for j in range(n - 1, -1, -1):
            if si == s[j]:
                row[j] = next_row[j + 1] + 1

    mx = [0] * n

    for i in range(n):
        best = 0
        for j in range(i):
            best = max(best, min(lcp[i][j], i - j))
        mx[i] = best

    INF = 10 ** 18
    dp = [INF] * (n + 1)
    dp[0] = 0

    for i in range(n):
        dp[i + 1] = min(dp[i + 1], dp[i] + a)

        if mx[i] > 0:
            dp[i + mx[i]] = min(dp[i + mx[i]], dp[i] + b)

    print(dp[n])

solve()
```Phần đầu tiên xây dựng bảng LCP. Tính toán ngược đảm bảo rằng khi chúng ta cần`lcp[i+1][j+1]`, nó đã được tính toán rồi. 

các`mx`mảng lưu trữ độ dài bản sao tối đa có sẵn tại mỗi vị trí. biểu thức`min(lcp[i][j], i-j)`là phần tinh tế. Nếu không có`i-j`hạn chế, chúng tôi sẽ cho phép sự xuất hiện nguồn mở rộng thành các ký tự chưa được mã hóa một cách không chính xác. 

DP lưu trữ chi phí tối thiểu cho tiền tố. Từ vị trí`i`, việc viết một ký tự luôn luôn có thể thực hiện được. Việc sao chép chỉ có thể thực hiện được khi`mx[i] > 0`. 

Một quan sát tinh tế là chúng tôi chỉ chuyển đổi bằng cách sử dụng`mx[i]`, không phải mọi chiều dài từ`1`ĐẾN`mx[i]`. Vì mỗi bản sao đều có giá chính xác`b`, bản sao dài hơn luôn lấn át bản sao ngắn hơn bắt đầu ở cùng một vị trí. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3 1
aba
```Độ dài bản sao được tính toán là: 

| Vị trí tôi | Hậu tố | mx[i] | 
| --- | --- | --- | 
| 0 | aba | 0 | 
| 1 | ba | 0 | 
| 2 | một | 1 | 

Sự phát triển của DP: 

| tôi | dp[i] | Hành động | Trạng thái cập nhật | 
| --- | --- | --- | --- | 
| 0 | 0 | viết 'a' | dp[1] = 3 | 
| 1 | 3 | viết 'b' | dp[2] = 6 | 
| 2 | 6 | viết 'a' | dp[3] = 9 | 
| 2 | 6 | bản sao dài 1 | dp[3] = 7 | 

Câu trả lời cuối cùng:```
7
```Ví dụ này cho thấy tại sao chuỗi con được sao chép có thể là một ký tự đơn. cuối cùng`'a'`đã xuất hiện ở tiền tố`"ab"`. 

### Ví dụ 2 

đầu vào:```
4 10 1
aaaa
```Sao chép độ dài: 

| Vị trí tôi | Hậu tố | mx[i] | 
| --- | --- | --- | 
| 0 | aaa | 0 | 
| 1 | aaa | 1 | 
| 2 | aa | 2 | 
| 3 | một | 1 | 

Sự phát triển của DP: 

| tôi | dp[i] | Hành động | Kết quả | 
| --- | --- | --- | --- | 
| 0 | 0 | viết | dp[1]=10 | 
| 1 | 10 | sao chép 1 | dp[2]=11 | 
| 2 | 11 | sao chép 2 | dp[4]=12 | 

Câu trả lời cuối cùng:```
12
```Dấu vết này chứng tỏ lợi ích của việc lấy bản sao dài nhất hiện có. Ở vị trí 2, sao chép`"aa"`chi phí chỉ bằng việc sao chép`"a"`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | tính toán LCP,`mx`tính toán và DP | 
| Không gian | O(n²) | Bảng LCP chiếm ưu thế trong việc sử dụng bộ nhớ | 

Với`n ≤ 5000`, thuật toán thực hiện khoảng 25 triệu thao tác trên bảng, phù hợp thoải mái trong giới hạn thời gian trong Python được tối ưu hóa. Việc sử dụng bộ nhớ cũng được chấp nhận đối với giới hạn của cuộc thi. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n, a, b = map(int, input().split())
    s = input().strip()

    lcp = [[0] * (n + 1) for _ in range(n + 1)]

    for i in range(n - 1, -1, -1):
        for j in range(n - 1, -1, -1):
            if s[i] == s[j]:
                lcp[i][j] = lcp[i + 1][j + 1] + 1

    mx = [0] * n
    for i in range(n):
        for j in range(i):
            mx[i] = max(mx[i], min(lcp[i][j], i - j))

    INF = 10 ** 18
    dp = [INF] * (n + 1)
    dp[0] = 0

    for i in range(n):
        dp[i + 1] = min(dp[i + 1], dp[i] + a)
        if mx[i]:
            dp[i + mx[i]] = min(dp[i + mx[i]], dp[i] + b)

    return str(dp[n]) + "\n"

# provided sample
assert run("3 3 1\naba\n") == "7\n", "sample 1"

# minimum size
assert run("1 5 2\na\n") == "5\n", "single character"

# copying never worth it
assert run("4 1 100\naaaa\n") == "4\n", "expensive copy"

# repeated string
assert run("4 10 1\naaaa\n") == "12\n", "cheap copy"

# no repeated substrings
assert run("5 3 1\nabcde\n") == "15\n", "all unique"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 5 2 / a`|`5`| Kích thước tối thiểu | 
|`4 1 100 / aaaa`|`4`| Bản sao tồn tại nhưng không được sử dụng | 
|`4 10 1 / aaaa`|`12`| Xử lý chuỗi con lặp đi lặp lại dài | 
|`5 3 1 / abcde`|`15`| Không có thao tác sao chép nào | 

## Vỏ cạnh 

Hãy xem xét:```
4 10 1
aaaa
```Tại vị trí`2`, các hậu tố bắt đầu tại`0`Và`2`phù hợp với chiều dài`2`. Thuật toán tính toán:```
min(lcp[2][0], 2) = min(2, 2) = 2
```Vì thế`mx[2] = 2`. DP nhảy trực tiếp từ độ dài tiền tố`2`đến độ dài tiền tố`4`về chi phí`1`, đưa ra câu trả lời đúng`12`. 

Coi như:```
4 1 100
aaaa
```Các cơ hội sao chép giống nhau đều tồn tại, nhưng DP so sánh chúng với việc viết các ký tự riêng lẻ. Vì mỗi bản sao đều có giá`100`, tất cả các chuyển đổi sao chép đều bị chi phối bởi các chuyển đổi ký tự. Câu trả lời trở thành`4`. 

Coi như:```
5 3 1
abcde
```Mỗi cặp hậu tố đều có LCP`0`, vậy là tất cả`mx[i]`giá trị vẫn còn`0`. DP chỉ sử dụng chuyển đổi ký tự và trả về`15`. 

Coi như:```
6 5 1
ababab
```Tại vị trí`2`, chuỗi con`"ab"`đã xuất hiện ở tiền tố. Bảng LCP tìm thấy sự trùng khớp về độ dài`4`, nhưng chỉ là lần đầu tiên`2`các ký tự có giá trị như một nguồn vì`i-j=2`. Thuật toán sử dụng:```
min(4, 2) = 2
```ngăn chặn sự chồng chéo bất hợp pháp với các ký tự trong tương lai. Hạn chế này chính xác là điều giữ cho giải pháp luôn chính xác.
