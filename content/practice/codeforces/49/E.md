---
title: "CF 49E - Tổ tiên chung"
description: "Mỗi chuỗi DNA tiến hóa bằng cách áp dụng lặp đi lặp lại các quy tắc có dạng a -> bc. Một ký tự được thay thế bằng chính xác hai ký tự và thao tác này có thể được lặp lại bao nhiêu lần cũng được. Bắt đầu từ chuỗi tổ tiên nào đó, quá trình tiến hóa chỉ làm tăng độ dài."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dp"]
categories: ["algorithms"]
codeforces_contest: 49
codeforces_index: "E"
codeforces_contest_name: "Codeforces Beta Round 46 (Div. 2)"
rating: 2300
weight: 49
solve_time_s: 121
verified: true
draft: false
---
[CF 49E - Tổ tiên chung](https://codeforces.com/problemset/problem/49/E) 

**Đánh giá:** 2300 
**Thẻ:** dp 
**Thời gian giải:** 2m 1s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi chuỗi DNA tiến hóa bằng cách liên tục áp dụng các quy tắc có dạng`a -> bc`. Một ký tự được thay thế bằng chính xác hai ký tự và thao tác này có thể được lặp lại bao nhiêu lần cũng được. Bắt đầu từ chuỗi tổ tiên nào đó, quá trình tiến hóa chỉ làm tăng độ dài. 

Chúng ta có hai chuỗi DNA cuối cùng và chúng ta muốn biết liệu cả hai chuỗi này có thể bắt nguồn từ cùng một tổ tiên hay không. Nếu có thể, chúng ta phải tìm ra độ dài tối thiểu có thể có của một tổ tiên như vậy. 

Quan sát quan trọng là sự tiến hóa tác động độc lập lên từng đặc điểm của tổ tiên. Nếu một chuỗi tổ tiên là:```
x1 x2 x3 ...
```sau đó mỗi ký tự mở rộng thành một chuỗi con nào đó và việc ghép tất cả các phần mở rộng sẽ tạo ra DNA cuối cùng. 

Chuỗi đầu vào có độ dài tối đa là 50 và có tối đa 50 quy tắc sản xuất. Nó đủ nhỏ để lập trình động bậc ba hoặc bậc bốn, nhưng quá lớn để tạo ra lực lượng vũ phu cho tất cả các chuỗi có thể truy cập. Ngay cả một ký tự đơn cũng có thể tạo ra nhiều chuỗi khác nhau theo cấp số nhân vì các quy tắc có thể phân nhánh theo cách đệ quy:```
c -> cc
```đã tạo ra vô số chuỗi có thể truy cập được. 

Kích thước bảng chữ cái chỉ có 26, điều này cực kỳ quan trọng. Bất kỳ DP nào được lập chỉ mục theo ký tự đều rẻ. 

Một trường hợp phức tạp xuất hiện khi không có sự thay thế nào tồn tại. 

Ví dụ:```
ab
ab
0
```Câu trả lời là`2`, bởi vì tổ tiên có thể đơn giản là`"ab"`chính nó. Một giải pháp bất cẩn cho rằng mọi ký tự phải được mở rộng ít nhất một lần sẽ từ chối trường hợp này một cách không chính xác. 

Một trường hợp khó khăn khác là sản xuất đệ quy. 

Ví dụ:```
a
aaaa
1
a->aa
```Câu trả lời đúng là`1`. Một ký tự tổ tiên có thể liên tục mở rộng thành toàn bộ chuỗi mục tiêu. Một BFS ngây thơ trên các chuỗi được tạo có thể không bao giờ chấm dứt vì ngữ pháp có tính tuần hoàn. 

Một trường hợp góc quan trọng khác là khi tổ tiên tối ưu dài hơn một ký tự mặc dù cả hai chuỗi đều có thể được dẫn xuất riêng lẻ từ một ký hiệu duy nhất. 

Ví dụ:```
ab
ba
2
x->ab
x->ba
```Câu trả lời là`2`, không`1`. Không có ký hiệu đơn lẻ nào có thể tạo ra cả hai chuỗi cùng một lúc. Tổ tiên chung ngắn nhất là`"ab"`chính nó. 

Sự tinh tế cuối cùng là sự liên kết. Các ký tự tổ tiên khác nhau có thể mở rộng thành các chuỗi con có độ dài khác nhau. 

Ví dụ:```
abba
baab
2
x->ab
x->ba
```Sự phân chia tối ưu không phải là theo từng ký tự. Phương pháp khớp tham lam không thành công vì cùng một phân vùng phải hoạt động đồng thời cho cả hai chuỗi. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mọi chuỗi tổ tiên có thể, hãy tạo tất cả các chuỗi có thể truy cập được từ chuỗi đó và kiểm tra xem cả hai chuỗi mục tiêu có xuất hiện hay không. 

Điều này đúng về mặt lý thuyết vì quá trình tiến hóa hoàn toàn được xác định bởi các quy tắc thay thế. Vấn đề là không gian trạng thái có thể tiếp cận là vô hạn bất cứ khi nào tồn tại các chu kỳ. Ngay cả khi không có chu kỳ, số lượng chuỗi được tạo vẫn tăng theo cấp số nhân với số bước mở rộng. Với độ dài mục tiêu lên tới 50, sức mạnh vũ phu gần như trở nên vô vọng ngay lập tức. 

Một góc nhìn tốt hơn đến từ việc đọc ngược các quy tắc. 

Một quy tắc:```
a -> bc
```có nghĩa là bất cứ khi nào chúng ta thấy các chuỗi con liền kề được tạo ra từ`b`Và`c`, chúng cùng nhau có thể có nguồn gốc từ`a`. 

Đây chính xác là phân tích ngữ pháp không ngữ cảnh. Mỗi ký tự là một ký hiệu không kết thúc và mỗi quy tắc kết hợp hai ký hiệu thành một ký hiệu gốc. 

Giả sử chúng ta xác định:```
can[c][l][r]
```có nghĩa là nhân vật đó`c`có thể tạo chuỗi con`s[l:r]`. 

Sau đó, chúng ta có thể tính toán điều này với khoảng DP, giống với phân tích cú pháp CYK. Một ký tự đơn lẻ tự tạo ra một cách tầm thường và các khoảng lớn hơn được tạo bằng cách chia chúng thành hai khoảng nhỏ hơn và áp dụng các quy tắc ngữ pháp. 

Một khi chúng ta biết, với mọi chuỗi con của`s1`Và`s2`, ký tự nào có thể tạo ra nó, vấn đề ban đầu sẽ trở thành một DP khác. 

Nếu một nhân vật tổ tiên nào đó`x`có thể tạo chuỗi con`s1[l1:r1]`và cả chuỗi con`s2[l2:r2]`, thì một vị trí tổ tiên có thể giải thích đồng thời cả hai phần. 

Bây giờ xác định:```
dp[i][j]
```là độ dài tổ tiên tối thiểu cần thiết để tạo tiền tố:```
s1[:i], s2[:j]
```Chúng tôi thử từng cặp chuỗi con kết thúc và kiểm tra xem có tồn tại ký tự tạo chung hay không. Nếu có, chúng tôi mở rộng phân tách theo một ký tự tổ tiên. 

Điều này biến bài toán từ tìm kiếm vô hạn thành quy hoạch động đa thức. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ / Vô hạn | Hàm mũ | Quá chậm | 
| Tối ưu | O(26 × n³ + | s1 | ² × | 

đây`n`nhiều nhất là 50. 

## Hướng dẫn thuật toán 

1. Phân tích tất cả các quy tắc thay thế. 

Đối với mọi quy tắc`a -> bc`, lưu trữ nó`a`có thể kết hợp các kết quả từ`b`Và`c`. 
2. Xây dựng khoảng DP cho chuỗi đầu tiên. 

Định nghĩa:```
can1[ch][l][r]
```điều đó đúng nếu nhân vật`ch`có thể tạo chuỗi con`s1[l:r]`. 

Về chiều dài`1`, chỉ có thể có ký tự phù hợp. 
3. Mở rộng các khoảng theo thứ tự độ dài tăng dần. 

Đối với mỗi khoảng thời gian`[l, r)`, hãy thử mọi điểm phân chia`m`. 

Nếu một số quy tắc:```
a -> bc
```thỏa mãn:```
can1[b][l][m] and can1[c][m][r]
```sau đó:```
can1[a][l][r] = True
```Điều này có hiệu quả vì mọi cây dẫn xuất cho một chuỗi con không tầm thường phải được chia thành hai dẫn xuất con ở quy tắc gốc. 
4. Lặp lại DP tương tự cho chuỗi thứ hai. 

Bây giờ chúng ta biết mọi ký tự có khả năng tạo ra mọi chuỗi con trong cả hai chuỗi. 
5. Xác định DP chính. 

Cho phép:```
dp[i][j]
```là độ dài tổ tiên tối thiểu cần thiết để tạo:```
s1[:i] and s2[:j]
```Khởi tạo:```
dp[0][0] = 0
```6. Chuyển đổi bằng cách khớp một ký tự tổ tiên. 

Đối với mọi trạng thái trước đó`(i, j)`, hãy thử tất cả các kết thúc chuỗi con:```
s1[i:ni], s2[j:nj]
```Nếu tồn tại một ký tự`c`như vậy:```
can1[c][i][ni] and can2[c][j][nj]
```thì một ký hiệu tổ tiên có thể tạo ra cả hai chuỗi con, vì vậy:```
dp[ni][nj] = min(dp[ni][nj], dp[i][j] + 1)
```7. Câu trả lời là`dp[len(s1)][len(s2)]`. 

Nếu không thể truy cập được, hãy in`-1`. 

### Tại sao nó hoạt động 

Khoảng DP tính toán chính xác tập hợp các chuỗi con có thể rút ra từ mỗi ký tự. Điều này tuân theo quy nạp từ cấu trúc ngữ pháp. Các chuỗi con có độ dài 1 là chính xác theo định nghĩa và mọi dẫn xuất lớn hơn phải áp dụng một sản phẩm ở gốc và chia thành hai dẫn xuất nhỏ hơn. 

DP thứ hai phân chia cả hai chuỗi thành các phần tương ứng. Mỗi cặp mảnh được tạo ra từ cùng một ký tự tổ tiên. Việc ghép các ký tự tổ tiên này tạo thành một chuỗi tổ tiên chung hợp lệ. 

Mỗi tổ tiên hợp lệ tạo ra một phân vùng như vậy và mọi phân vùng do DP xây dựng đều tương ứng với một tổ tiên hợp lệ. Vì quá trình chuyển đổi thêm chính xác một ký tự tổ tiên nên DP giảm thiểu độ dài tổ tiên một cách chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10**9

def build_can(s, rules):
    n = len(s)

    can = [[[-1] * (n + 1) for _ in range(n)] for _ in range(26)]

    for i, ch in enumerate(s):
        can[ord(ch) - ord('a')][i][i + 1] = 1

    for length in range(2, n + 1):
        for l in range(n - length + 1):
            r = l + length

            for m in range(l + 1, r):
                for a, b, c in rules:
                    if can[b][l][m] == 1 and can[c][m][r] == 1:
                        can[a][l][r] = 1

    return can

def solve():
    s1 = input().strip()
    s2 = input().strip()

    n = int(input())

    rules = []

    for _ in range(n):
        line = input().strip()

        a = ord(line[0]) - ord('a')
        b = ord(line[3]) - ord('a')
        c = ord(line[4]) - ord('a')

        rules.append((a, b, c))

    can1 = build_can(s1, rules)
    can2 = build_can(s2, rules)

    n1 = len(s1)
    n2 = len(s2)

    dp = [[INF] * (n2 + 1) for _ in range(n1 + 1)]
    dp[0][0] = 0

    for i in range(n1 + 1):
        for j in range(n2 + 1):
            if dp[i][j] == INF:
                continue

            for ni in range(i + 1, n1 + 1):
                for nj in range(j + 1, n2 + 1):

                    ok = False

                    for ch in range(26):
                        if can1[ch][i][ni] == 1 and can2[ch][j][nj] == 1:
                            ok = True
                            break

                    if ok:
                        dp[ni][nj] = min(dp[ni][nj], dp[i][j] + 1)

    ans = dp[n1][n2]

    print(-1 if ans == INF else ans)

solve()
```các`build_can`hàm thực hiện phân tích ngữ pháp DP. Mỗi trạng thái lưu trữ liệu một ký tự có thể lấy được một chuỗi con hay không. Các khoảng được xử lý theo thứ tự độ dài tăng dần vì các đạo hàm lớn hơn phụ thuộc vào các khoảng nhỏ hơn. 

Sự đại diện:```
can[ch][l][r]
```hơi bất thường vì điểm cuối phù hợp là độc quyền. Điều này tránh được nhiều lỗi lầm xảy ra. Chuỗi con luôn là:```
s[l:r]
```chính xác như việc cắt Python. 

DP chính xử lý các chuỗi như được phân chia thành các khối được đồng bộ hóa. Mỗi lần chuyển đổi sẽ kiểm tra xem một ký tự có thể lấy được cả hai chuỗi con đã chọn hay không. 

Các vòng lặp lồng nhau có thể trông đắt tiền nhưng giới hạn lại rất nhỏ. Nhiều nhất:```
51 × 51 × 51 × 51 × 26
```kiểm tra xảy ra, dễ dàng phù hợp với thời gian. 

Một chi tiết triển khai tinh tế là chúng tôi không bao giờ yêu cầu cây phái sinh phải có hình dạng giống nhau. Chúng tôi chỉ yêu cầu một số ký tự chung có thể tạo cả hai chuỗi con một cách độc lập. Khoảng DP đã gói gọn tất cả các dẫn xuất có thể có. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
ababa
aba
2
c->ba
c->cc
```Việc sản xuất hữu ích duy nhất là:```
c -> ba
```Quy tắc thứ hai cho phép mở rộng lặp đi lặp lại. 

Khoảng thời gian DP phát hiện: 

| Chuỗi | Chuỗi con | Tạo nhân vật | 
| --- | --- | --- | 
| ba ba | ba | c | 
| aba | ba | c | 

Bây giờ DP chính tiếp tục. 

| Tiểu bang | Chuỗi con được chọn | Ký tự chung | Chi phí mới | 
| --- | --- | --- | --- | 
| (0,0) | một/một | một | 1 | 
| (1,1) | ba/ba | c | 2 | 
| (3,3) | ba/trống | không thể | - | 

Sự phân hủy tối ưu là:```
a + c
```chiều dài của nó là`2`. 

Ví dụ này cho thấy thế hệ đệ quy. nhân vật`c`không xuất hiện trực tiếp trong chuỗi cuối cùng, nhưng phân tích cú pháp ngược lặp đi lặp lại vẫn phát hiện ra nó. 

### Ví dụ 2 

đầu vào:```
aaaa
aa
1
a->aa
```Phân tích khoảng thời gian tìm thấy: 

| Chuỗi con | Có thể được tạo bởi | 
| --- | --- | 
| một | một | 
| aa | một | 
| aaa | một | 

Quy tắc đệ quy cho phép một`a`mở rộng tùy ý nhiều lần. 

DP chính trở thành: 

| Tiểu bang | Phần được chọn | Ký tự chung | Chi phí | 
| --- | --- | --- | --- | 
| (0,0) | aaa / aa | một | 1 | 

Vậy câu trả lời là`1`. 

Điều này xác nhận rằng độ sâu dẫn xuất không quan trọng. Một ký tự tổ tiên có thể tạo ra độ dài chuỗi con rất khác nhau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(26 × n³ + n⁴ × 26) | Phân tích khoảng thời gian cộng với chuỗi con theo cặp DP | 
| Không gian | O(26 × n2) | Bảng khả năng tiếp cận cho chuỗi con | 

Với`n ≤ 50`, số thao tác trong trường hợp xấu nhất nằm dưới giới hạn một cách thoải mái. Việc sử dụng bộ nhớ cũng rất nhỏ vì kích thước bảng chữ cái được cố định ở mức 26. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

INF = 10**9

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def build_can(s, rules):
        n = len(s)

        can = [[[-1] * (n + 1) for _ in range(n)] for _ in range(26)]

        for i, ch in enumerate(s):
            can[ord(ch) - ord('a')][i][i + 1] = 1

        for length in range(2, n + 1):
            for l in range(n - length + 1):
                r = l + length

                for m in range(l + 1, r):
                    for a, b, c in rules:
                        if can[b][l][m] == 1 and can[c][m][r] == 1:
                            can[a][l][r] = 1

        return can

    s1 = input().strip()
    s2 = input().strip()

    n = int(input())

    rules = []

    for _ in range(n):
        line = input().strip()

        a = ord(line[0]) - ord('a')
        b = ord(line[3]) - ord('a')
        c = ord(line[4]) - ord('a')

        rules.append((a, b, c))

    can1 = build_can(s1, rules)
    can2 = build_can(s2, rules)

    n1 = len(s1)
    n2 = len(s2)

    dp = [[INF] * (n2 + 1) for _ in range(n1 + 1)]
    dp[0][0] = 0

    for i in range(n1 + 1):
        for j in range(n2 + 1):
            if dp[i][j] == INF:
                continue

            for ni in range(i + 1, n1 + 1):
                for nj in range(j + 1, n2 + 1):

                    ok = False

                    for ch in range(26):
                        if can1[ch][i][ni] == 1 and can2[ch][j][nj] == 1:
                            ok = True
                            break

                    if ok:
                        dp[ni][nj] = min(dp[ni][nj], dp[i][j] + 1)

    ans = dp[n1][n2]

    return str(-1 if ans == INF else ans)

# provided sample
assert run(
"""ababa
aba
2
c->ba
c->cc
"""
) == "2"

# identical strings, no rules
assert run(
"""abc
abc
0
"""
) == "3"

# recursive expansion
assert run(
"""aaaa
aa
1
a->aa
"""
) == "1"

# impossible case
assert run(
"""ab
cd
0
"""
) == "-1"

# single-character ancestor expands differently
assert run(
"""aaaa
aaaaaaaa
1
a->aa
"""
) == "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuỗi giống hệt nhau không có quy tắc | 3 | Trường hợp cơ bản không có sự thay thế | 
| đệ quy`a->aa`| 1 | Chuỗi đạo hàm vô hạn | 
| các chuỗi hoàn toàn khác nhau | -1 | Tái thiết bất khả thi | 
| khai triển đệ quy lớn | 1 | Độ dài đầu ra khác nhau từ cùng một tổ tiên | 

## Vỏ cạnh 

Hãy xem xét trường hợp không có sự thay thế. 

đầu vào:```
ab
ab
0
```Khoảng DP chỉ đánh dấu các chuỗi con có một ký tự. Không có chuỗi con lớn hơn nào có thể được tạo ra bởi một ký hiệu. 

DP chính phân chia cả hai chuỗi thành:```
a + b
```vì vậy câu trả lời trở thành`2`. 

Mô hình này chính xác cho phép các ký tự không thay đổi. Evolution có thể thực hiện các hoạt động bằng không. 

Bây giờ hãy xem xét tăng trưởng đệ quy. 

đầu vào:```
aaaa
aa
1
a->aa
```Khoảng DP đầu tiên đánh dấu tất cả các đơn`a`các chuỗi con. Sau đó, các chuỗi con có độ dài 2 có thể bắt nguồn từ`a`. Việc kết hợp lại những chuỗi đó làm cho chuỗi con có độ dài 4 có thể bắt nguồn từ`a`. 

Thuật toán không bao giờ mô phỏng rõ ràng độ sâu mở rộng. Nó chỉ theo dõi khả năng dẫn xuất của chuỗi con nên các chu trình là vô hại. 

Cuối cùng, hãy xem xét các dẫn xuất không tương thích. 

đầu vào:```
ab
ba
2
x->ab
x->ba
```Khoảng thời gian DP phát hiện:```
x generates ab
x generates ba
```nhưng không có cặp chuỗi con nào bao gồm cả hai chuỗi cùng một lúc với cùng một ký tự. 

Thay vào đó, DP chính sẽ phân hủy thành:```
a + b
```đưa ra câu trả lời`2`. 

Điều này cho thấy tại sao việc phân vùng đồng bộ là cần thiết. Khả năng dẫn xuất phù hợp một cách độc lập cho hai chuỗi là không đủ.
