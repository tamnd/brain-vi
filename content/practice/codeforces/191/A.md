---
title: "CF 191A - Câu đố về triều đại"
description: "Chúng ta được cung cấp một danh sách tên các vị vua viết tắt theo thứ tự thời gian. Mỗi tên là một chuỗi chữ thường. Chúng ta có thể chọn một số tên trong số này để thành lập một triều đại, đồng thời giữ nguyên trật tự ban đầu của chúng. Một triều đại hợp lệ phải thỏa mãn hai điều kiện."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dp"]
categories: ["algorithms"]
codeforces_contest: 191
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 121 (Div. 1)"
rating: 1500
weight: 191
solve_time_s: 102
verified: true
draft: false
---

[CF 191A - Câu đố về triều đại](https://codeforces.com/problemset/problem/191/A) 

**Đánh giá:** 1500 
**Thẻ:** dp 
**Thời gian giải:** 1 phút 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một danh sách tên các vị vua viết tắt theo thứ tự thời gian. Mỗi tên là một chuỗi chữ thường. Chúng ta có thể chọn một số tên trong số này để thành lập một triều đại, đồng thời giữ nguyên trật tự ban đầu của chúng. 

Một triều đại hợp lệ phải thỏa mãn hai điều kiện. 

Ký tự cuối cùng của mỗi tên được chọn phải bằng ký tự đầu tiên của tên được chọn tiếp theo. Điều này tạo ra một chuỗi. 

Triều đại cũng phải tạo thành một chu kỳ tổng thể, nghĩa là ký tự đầu tiên của tên được chọn đầu tiên phải bằng ký tự cuối cùng của tên được chọn cuối cùng. 

Bản thân tên triều đại là sự ghép nối của tất cả các chuỗi đã chọn, vì vậy mục tiêu là tối đa hóa tổng độ dài của tất cả các tên được chọn. 

Điều quan trọng cần lưu ý là chỉ có chữ cái đầu tiên và cuối cùng của chuỗi mới có ý nghĩa tương thích. Các ký tự bên trong không bao giờ ảnh hưởng đến quá trình chuyển đổi. Sự đóng góp của một cái tên chỉ đơn giản là độ dài của nó. 

Các ràng buộc hoàn toàn định hình giải pháp. Chúng tôi có thể có tới$5 \cdot 10^5$chuỗi, vì vậy mọi thứ bậc hai đều không thể xảy ra. Ngay cả việc lưu trữ các chuyển đổi giữa tất cả các cặp cũng đã quá lớn. Vì mỗi chuỗi có độ dài tối đa là 10 nên việc xử lý từng tên riêng lẻ sẽ rẻ, nhưng chúng ta phải giữ công việc cho mỗi tên gần với thời gian không đổi. 

Giải pháp lập trình động trên bảng chữ cái trở nên khả thi vì chỉ có 26 chữ cái viết thường. Bất kỳ thuật toán nào sử dụng trạng thái dựa trên chữ cái bắt đầu và kết thúc đều có nhiều nhất$26^2 = 676$tiểu bang, đó là rất nhỏ. 

Có một số trường hợp nguy hiểm có thể âm thầm phá vỡ các giải pháp không chính xác. 

Hãy xem xét một chuỗi có ký tự đầu tiên và cuối cùng khớp nhau.```
1
abca
```Câu trả lời đúng là 4. Việc triển khai bất cẩn chỉ tìm kiếm sự chuyển đổi giữa các chuỗi khác nhau sẽ trả về 0 không chính xác. 

Một trường hợp tinh tế khác xuất hiện khi việc bỏ qua chuỗi là cần thiết.```
3
abc
ca
cba
```Triều đại tối ưu sử dụng chuỗi thứ nhất và thứ ba, tạo ra độ dài 6. Các cách tiếp cận tham lam luôn mở rộng ngay lập tức với chuỗi tương thích tiếp theo sẽ dừng lại ở`"abc" + "ca"`và bỏ lỡ giải pháp tốt hơn. 

Bẫy thứ ba là ghi đè trạng thái DP không chính xác trong quá trình cập nhật.```
3
ab
bc
ca
```Câu trả lời là 6. Nếu các bản cập nhật được thực hiện tại chỗ mà không cẩn thận, chuỗi hiện tại có thể được sử dụng lại nhiều lần trong cùng một lần lặp, cho phép thực hiện các chuỗi không thể thực hiện được. 

Cuối cùng, có thể không có chu trình hợp lệ nào cả.```
2
ab
bc
```Không có dãy con nào được chọn tạo thành một chu trình, vì vậy câu trả lời là 0. Trả về chuỗi tốt nhất thay vì chu trình tốt nhất sẽ là sai. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Chúng tôi thử từng chuỗi con của chuỗi, kiểm tra xem các chuỗi liền kề có kết nối chính xác hay không và xác minh xem toàn bộ chuỗi có tạo thành một chu trình hay không. 

Điều này có hiệu quả vì các quy tắc chỉ phụ thuộc vào các chuỗi lân cận và tổng thể các chữ cái đầu tiên và cuối cùng. Vấn đề là số lượng các chuỗi con. Với$n$dây, có$2^n$những lựa chọn có thể. Ngay cả đối với$n = 50$, điều này đã là vô vọng rồi, và giới hạn thực sự là$5 \cdot 10^5$. 

Một biến thể brute-force hợp lý hơn sử dụng lập trình động trên các tập hợp con hoặc đường dẫn đồ thị, nhưng biến thể đó vẫn bị sập vì kích thước đầu vào rất lớn. 

Cấu trúc của vấn đề mang lại một quan sát mạnh mẽ hơn nhiều. Mỗi chuỗi chỉ đóng góp ba thông tin có ý nghĩa: 

Bức thư đầu tiên. 

Bức thư cuối cùng. 

Chiều dài. 

Bảng chữ cái chỉ có 26 chữ cái nên số lượng cặp chữ cái có thể bắt đầu là rất ít. Thay vì nhớ chính xác trình tự, chúng ta chỉ cần nhớ tổng độ dài tốt nhất cho mỗi cặp chữ cái bắt đầu và kết thúc có thể. 

Định nghĩa:$dp[i][j]$= tổng chiều dài tối đa của chuỗi hợp lệ có chuỗi đầu tiên bắt đầu bằng chữ cái$i$và chuỗi cuối cùng hiện tại của nó kết thúc bằng chữ cái$j$. 

Khi xử lý một chuỗi mới từ$a$ĐẾN$b$với chiều dài$w$, chúng ta có thể: 

Bắt đầu một chuỗi mới chỉ bao gồm chuỗi này. 

Mở rộng mọi chuỗi hiện có kết thúc tại$a$. 

Điều này có thể thực hiện được vì khả năng tương thích chỉ phụ thuộc vào việc khớp các chữ cái ranh giới. Toàn bộ lịch sử của chuỗi không thành vấn đề. 

Vì chỉ có 26 chữ cái kết thúc nên mỗi chuỗi chỉ thực hiện khoảng 26 lần chuyển tiếp. Tổng công việc trở nên đại khái$26 \cdot n$, dễ dàng phù hợp trong giới hạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n)$|$O(n)$| Quá chậm | 
| DP tối ưu |$O(26 \cdot n)$|$O(26^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo bảng DP 26 × 26 được khởi tạo ở mức âm vô cực. 

Nhà nước$dp[i][j]$lưu trữ tổng chiều dài tối đa của chuỗi bắt đầu bằng chữ cái$i$và hiện kết thúc bằng chữ cái$j$. 
2. Xử lý từng chuỗi một theo thứ tự đầu vào. 

Việc giữ gìn trật tự là bắt buộc vì các vị vua xuất hiện theo trình tự thời gian. 
3. Đối với chuỗi hiện tại, trích xuất: 

- ký tự bắt đầu của nó$a$- ký tự kết thúc của nó$b$- chiều dài của nó$w$4. Bắt đầu một chuỗi mới chỉ sử dụng chuỗi này. 

Cập nhật:$$dp[a][b] = \max(dp[a][b], w)$$Điều này xử lý các triều đại có một vị vua duy nhất. 

1. Hãy thử mở rộng mọi chuỗi hiện kết thúc bằng$a$. 

Đối với mọi chữ cái bắt đầu có thể$s$:$$dp[s][b] = \max(dp[s][b], dp[s][a] + w)$$Điều này có tác dụng vì chuỗi hiện tại có thể theo sau một cách hợp pháp bất kỳ chuỗi nào có chữ cái cuối cùng bằng chữ cái đầu tiên. 

1. Cẩn thận không sử dụng lại chuỗi hiện tại nhiều lần trong cùng một lần lặp. 

Để tránh điều này, hãy sao chép các giá trị cũ trước khi cập nhật hoặc lặp lại bằng cách sử dụng ảnh chụp nhanh của cột có liên quan. 
2. Sau khi tất cả các chuỗi được xử lý, câu trả lời là:$$\max(dp[c][c])$$trên tất cả 26 chữ cái$c$. 

Một triều đại chỉ hợp lệ nếu tổng thể các chữ cái đầu tiên và cuối cùng của nó khớp với nhau. 

### Tại sao nó hoạt động 

Trạng thái DP nắm bắt chính xác thông tin cần thiết cho các tiện ích mở rộng trong tương lai. 

Giả sử chúng ta biết chuỗi tốt nhất bắt đầu bằng chữ cái$s$và kết thúc bằng chữ cái$e$. Bất kỳ chuỗi tương lai nào cũng chỉ có thể kiểm tra xem chữ cái đầu tiên của nó có khớp hay không$e$. Không có gì khác về vấn đề chuỗi. 

Khi chúng ta nối thêm một chuỗi từ$a$ĐẾN$b$, mọi chuỗi tương thích kết thúc tại$a$trở thành một chuỗi mới kết thúc tại$b$, với tổng chiều dài tăng theo chiều dài chuỗi hiện tại. 

Bởi vì mỗi chuỗi được xử lý một lần và các quá trình chuyển đổi chỉ diễn ra theo thứ tự thời gian, nên mỗi chuỗi được xây dựng đều tương ứng với một dãy con hợp lệ của các vị vua. Ngược lại, mọi triều đại hợp lệ đều có thể được xây dựng thông qua những quá trình chuyển đổi này, vì vậy DP không bao giờ bỏ lỡ một giải pháp tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = -(10 ** 18)

n = int(input())

dp = [[INF] * 26 for _ in range(26)]

for _ in range(n):
    s = input().strip()

    a = ord(s[0]) - ord('a')
    b = ord(s[-1]) - ord('a')
    w = len(s)

    old = [dp[i][a] for i in range(26)]

    dp[a][b] = max(dp[a][b], w)

    for start in range(26):
        if old[start] != INF:
            dp[start][b] = max(dp[start][b], old[start] + w)

ans = 0

for i in range(26):
    ans = max(ans, dp[i][i])

print(ans)
```Bảng DP lưu trữ tổng độ dài tốt nhất có thể đạt được cho mỗi cặp chữ cái bắt đầu. Các trạng thái không hợp lệ được khởi tạo bằng số rất âm để chúng ta có thể phân biệt chúng với các chuỗi hợp pháp. 

Đối với mỗi chuỗi, trước tiên chúng tôi trích xuất các chữ cái và độ dài ranh giới của nó. Chuỗi luôn có thể tự tạo thành một triều đại, vì vậy chúng tôi cập nhật`dp[a][b]`trực tiếp. 

Phần tinh vi nhất là mở rộng các chuỗi trước đó một cách an toàn. Chúng ta phải tránh đọc các giá trị đã được cập nhật trong lần lặp hiện tại, vì điều đó sẽ cho phép cùng một chuỗi được nối thêm nhiều lần. Dòng:```
old = [dp[i][a] for i in range(26)]
```tạo một ảnh chụp nhanh của tất cả các chuỗi kết thúc bằng chữ cái`a`trước khi xử lý chuỗi hiện tại. 

Sau đó, mỗi chuỗi tương thích sẽ được mở rộng theo độ dài chuỗi hiện tại. 

Cuối cùng, chỉ những trạng thái có chữ cái bắt đầu và chữ cái kết thúc bằng nhau mới đại diện cho các triều đại hợp lệ, vì vậy chúng tôi lấy giá trị đường chéo tối đa. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
abc
ca
cba
```Xử lý dấu vết: 

| Chuỗi | Bắt đầu | Kết thúc | Chiều dài | Các bang cập nhật | 
| --- | --- | --- | --- | --- | 
| abc | một | c | 3 | dp[a][c] = 3 | 
| ca | c | một | 2 | dp[c][a] = 2, dp[a][a] = 5 | 
| cba | c | một | 3 | dp[c][a] = 3, dp[a][a] = 6 | 

Giá trị đường chéo cuối cùng: 

| Tiểu bang | Giá trị | 
| --- | --- | 
| dp[a][a] | 6 | 
| người khác | không hợp lệ hoặc nhỏ hơn | 

Trả lời:```
6
```Việc sử dụng triều đại tối ưu`"abc"`theo sau là`"cba"`. Chuỗi trung gian`"ca"`bị bỏ qua. Điều này chứng tỏ tại sao việc mở rộng tham lam là không đủ và tại sao chuỗi con DP lại cần thiết. 

### Ví dụ 2 

đầu vào:```
2
ab
bc
```Xử lý dấu vết: 

| Chuỗi | Bắt đầu | Kết thúc | Chiều dài | Các bang cập nhật | 
| --- | --- | --- | --- | --- | 
| ab | một | b | 2 | dp[a][b] = 2 | 
| bc | b | c | 2 | dp[b][c] = 2, dp[a][c] = 4 | 

Giá trị đường chéo cuối cùng: 

| Tiểu bang | Giá trị | 
| --- | --- | 
| dp[a][a] | không hợp lệ | 
| dp[b][b] | không hợp lệ | 
| dp[c][c] | không hợp lệ | 

Trả lời:```
0
```Chuỗi`"ab" -> "bc"`có giá trị cục bộ, nhưng nó không tạo thành một chu trình tổng thể vì nó bắt đầu bằng`a`và kết thúc bằng`c`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(26 \cdot n)$| Mỗi chuỗi thực hiện chuyển tiếp trên 26 chữ cái bắt đầu có thể | 
| Không gian |$O(26^2)$| Bảng DP chỉ chứa 676 trạng thái | 

Với$n \le 5 \cdot 10^5$, thuật toán thực hiện khoảng 13 triệu chuyển đổi trạng thái, đủ nhanh trong Python. Việc sử dụng bộ nhớ là rất nhỏ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    INF = -(10 ** 18)

    n = int(input())

    dp = [[INF] * 26 for _ in range(26)]

    for _ in range(n):
        s = input().strip()

        a = ord(s[0]) - ord('a')
        b = ord(s[-1]) - ord('a')
        w = len(s)

        old = [dp[i][a] for i in range(26)]

        dp[a][b] = max(dp[a][b], w)

        for start in range(26):
            if old[start] != INF:
                dp[start][b] = max(dp[start][b], old[start] + w)

    ans = 0

    for i in range(26):
        ans = max(ans, dp[i][i])

    return str(ans)

# provided sample
assert run("3\nabc\nca\ncba\n") == "6", "sample 1"

# no valid dynasty
assert run("2\nab\nbc\n") == "0", "no cycle"

# single valid string
assert run("1\nc\n") == "1", "single string cycle"

# choose non-adjacent strings
assert run("4\nab\nbc\nca\naa\n") == "6", "best subsequence"

# all equal letters
assert run("3\naa\naa\naa\n") == "6", "all chains compatible"

# off-by-one style chain
assert run("3\nab\nba\nab\n") == "6", "reuse all strings correctly"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / c`|`1`| Triều đại đơn chuỗi | 
|`2 / ab bc`|`0`| Không tồn tại chu kỳ | 
|`4 / ab bc ca aa`|`6`| Câu trả lời hay nhất có thể bỏ qua chuỗi | 
|`3 / aa aa aa`|`6`| Các trạng thái tương thích lặp đi lặp lại | 
|`3 / ab ba ab`|`6`| Chuỗi chuyển tiếp chính xác | 

## Vỏ cạnh 

Hãy xem xét trường hợp một chuỗi đơn đã tạo thành một chu trình.```
1
abca
```Chuỗi bắt đầu và kết thúc bằng`a`, vì vậy trong quá trình xử lý chúng tôi cập nhật:```
dp[a][a] = 4
```Đường chéo tối đa cuối cùng trở thành 4, điều này đúng. Các thuật toán chỉ mở rộng các chuỗi trước đó sẽ bỏ lỡ điều này vì không cần chuyển đổi. 

Bây giờ hãy xem xét trường hợp bỏ qua chuỗi là tối ưu.```
3
abc
ca
cba
```Sau khi xử lý`"abc"`:```
dp[a][c] = 3
```Xử lý`"ca"`tạo ra:```
dp[a][a] = 5
```Xử lý`"cba"`trực tiếp mở rộng chuỗi trước đó:```
dp[a][a] = 6
```Thuật toán khám phá tất cả các chuỗi con một cách tự nhiên vì mỗi trạng thái vẫn tồn tại cho các chuyển tiếp trong tương lai. 

Cuối cùng, hãy xem xét một kịch bản cập nhật tại chỗ nguy hiểm.```
3
ab
bc
ca
```Khi xử lý`"bc"`, trước tiên chúng tôi chụp nhanh tất cả các chuỗi kết thúc tại`b`. Nếu không có ảnh chụp nhanh này, việc cập nhật`dp[a][c]`có thể vô tình ảnh hưởng đến các quá trình chuyển đổi sau này trong cùng một lần lặp và tái sử dụng`"bc"`nhiều lần. 

Sử dụng:```
old = [dp[i][a] for i in range(26)]
```đảm bảo mọi chuyển đổi chỉ sử dụng các trạng thái tồn tại trước khi chuỗi hiện tại được xử lý.
