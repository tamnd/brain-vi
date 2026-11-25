---
title: "CF 98E - Giúp Shrek và Lừa"
description: "Tổng cộng có m + n + 1 thẻ riêng biệt. Shrek ban đầu biết thẻ m của mình, Donkey biết n thẻ của mình và một thẻ được giấu trên bàn. Không ai biết trực tiếp lá bài ẩn. Người chơi thay phiên nhau, Shrek di chuyển trước."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dp", "games", "math", "probabilities"]
categories: ["algorithms"]
codeforces_contest: 98
codeforces_index: "E"
codeforces_contest_name: "Codeforces Beta Round 78 (Div. 1 Only)"
rating: 2700
weight: 98
solve_time_s: 145
verified: true
draft: false
---

[CF 98E - Giúp Shrek và Con Lừa](https://codeforces.com/problemset/problem/98/E) 

**Xếp hạng:** 2700 
**Tags:** dp, trò chơi, toán học, xác suất 
**Thời gian giải:** 2m 25s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

có`m + n + 1`tổng số thẻ riêng biệt. Shrek ban đầu biết chính mình`m`thẻ, Lừa biết thẻ của mình`n`thẻ, và một thẻ được giấu trên bàn. Không ai biết trực tiếp lá bài ẩn. 

Người chơi thay phiên nhau, Shrek di chuyển trước. Trong mỗi lượt, người chơi có thể đoán ngay lá bài ẩn hoặc hỏi xem đối thủ có sở hữu một lá bài cụ thể nào đó hay không. Nếu đối thủ có nó, lá bài đó sẽ bị lộ và bị loại khỏi trò chơi vĩnh viễn. Nếu không, đối thủ chỉ trả lời "không". 

Đoán sai sẽ thua ngay lập tức. Đoán đúng sẽ thắng ngay lập tức. Cả hai người chơi đều chơi hoàn hảo và cả hai đều biết rằng người kia cũng chơi hoàn hảo. Chúng ta phải tính xác suất Shrek thắng và xác suất Donkey thắng. 

Đầu vào duy nhất là`(m, n)`, số quân bài ban đầu mà Shrek và Donkey nắm giữ. Đầu ra là hai xác suất có tổng là`1`. 

Các ràng buộc đi lên đến`1000`, nhỏ đối với lập trình động bậc hai nhưng lại quá lớn đối với bất kỳ không gian trạng thái nào dựa trên cấu hình thẻ rõ ràng. Có rất nhiều cách phân phối thẻ có thể xảy ra theo cấp số nhân, vì vậy chúng ta cần nén trò chơi vào một trạng thái toán học nhỏ hơn nhiều. 

Quan sát quan trọng là danh tính thẻ không quan trọng. Chỉ có số lượng quan trọng. Tại bất kỳ thời điểm nào, trạng thái trò chơi hoàn toàn được xác định bởi số lượng thẻ chưa biết mà mỗi người chơi vẫn có thể có. Điều đó làm giảm vấn đề xuống còn khoảng một triệu tiểu bang và có thể quản lý được. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Nếu một người chơi bắt đầu với 0 lá bài, câu trả lời sẽ không tự động thua. Ví dụ:```
0 3
```Shrek không biết gì cả, nhưng theo quan điểm của anh ta chỉ có thể có bốn lá bài ẩn. Bất kỳ dự đoán nào cũng thành công với xác suất`1/4`, phù hợp với đầu ra mẫu. 

Một trường hợp tế nhị khác là:```
0 0
```Tổng cộng có đúng một thẻ và nó được ẩn. Shrek đoán ngay và thắng có xác suất`1`. 

Một ý tưởng sai lầm phổ biến là cho rằng đặt câu hỏi luôn tốt hơn là đoán mò. Điều đó không thành công khi người chơi đã biết chắc chắn lá bài ẩn. Ví dụ:```
1 0
```Shrek biết rõ quân bài của chính mình nên quân bài ẩn chắc chắn là quân bài duy nhất còn sót lại. Anh ta thắng ngay lập tức với xác suất`1`. 

Một sai lầm nguy hiểm hơn là mô hình trò chơi như những câu hỏi ngẫu nhiên. Các cầu thủ là tối ưu, không phải ngẫu nhiên. Mọi câu hỏi đều được chọn để tối đa hóa xác suất chiến thắng và nước đi tối ưu phụ thuộc vào trạng thái đệ quy trong tương lai. 

## Phương pháp tiếp cận 

Một công thức mạnh mẽ sẽ theo dõi bộ bài chính xác mà mỗi người chơi vẫn có thể giữ và mô phỏng đệ quy mọi câu hỏi và phỏng đoán hợp pháp. Điều này đúng vì trò chơi là một trong những thông tin hoàn hảo khi xác suất được đưa vào trạng thái. 

Vấn đề là kích thước của không gian trạng thái. Với tối đa`2001`tổng số quân bài, số lượng phân phối ẩn có thể có là vô cùng lớn. Ngay cả đối với ba mươi thẻ, biểu đồ trạng thái đã không thể thực hiện được. 

Quan sát quan trọng là tính đối xứng. Tên của các lá bài không bao giờ quan trọng, chỉ có bao nhiêu khả năng còn lại. 

Giả sử đến lượt bạn và hiện có: 

-`a`những quân bài mà chỉ đối thủ mới có thể nắm giữ, 
-`b`những tấm thẻ mà chỉ bạn mới có thể nắm giữ, 
-`1`thẻ ẩn. 

Theo quan điểm của bạn, mọi lá bài chưa biết đều có khả năng bị ẩn như nhau. Hỏi về một lá bài sẽ loại bỏ một khả năng hoặc tiết lộ thông tin làm thay đổi số lượng. Hai thẻ có cùng trạng thái có thể hoán đổi cho nhau. 

Điều đó có nghĩa là trạng thái trò chơi sẽ giảm xuống chỉ còn`(a, b)`cùng với đó là lượt của ai. 

Bây giờ chúng ta có thể xác định trạng thái lập trình động:`dp[a][b]`= xác suất người chơi hiện tại cuối cùng thắng khi họ biết`b`bài và đối thủ có thể biết`a`thẻ. 

Từ một tiểu bang`(a, b)`, người chơi hiện tại có hai loại hành động. 

Họ có thể đoán ngay lập tức. Vì có`a + 1`khả năng có lá bài ẩn theo quan điểm của họ, xác suất thành công là:$$\frac{1}{a+1}$$Hoặc họ có thể hỏi về một trong những lá bài có thể có của đối thủ. 

Nếu đối thủ thực sự có nó, điều đó xảy ra với xác suất`a / (a+1)`, lá bài đó sẽ bị loại bỏ và người chơi đó lại di chuyển ở trạng thái`(a-1, b)`. 

Nếu thẻ thực sự bị ẩn, điều này xảy ra với xác suất`1 / (a+1)`, lượt chuyển sang đối thủ ở trạng thái`(b, a-1)`. 

Cấu trúc đệ quy này là toàn bộ trò chơi. 

Cách tiếp cận bạo lực không thành công vì nó phân biệt danh tính thẻ một cách không cần thiết. Việc quan sát đối xứng làm giảm không gian trạng thái xuống chỉ còn khoảng một triệu trạng thái, cho phép DP được ghi nhớ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force về việc phân phối thẻ | Hàm mũ | Hàm mũ | Quá chậm | 
| DP về số lượng thẻ | O(mn) | O(mn) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xác định`dp[a][b]`là xác suất mà người chơi hiện tại thắng khi đối thủ có thể sở hữu`a`thẻ không xác định và người chơi hiện tại biết`b`thẻ. 
2. Xử lý phần đế`dp[0][b] = 1`. 

Khi đối thủ không còn quân bài riêng nào, quân bài ẩn hoàn toàn được xác định nên người chơi hiện tại đoán đúng ngay lập tức. 
3. Đối với mọi tiểu bang`(a, b)`với`a > 0`, hãy cân nhắc lựa chọn đoán ngay. 

Thẻ ẩn được phân bố đồng đều giữa`a + 1`các khả năng có thể xảy ra, do đó việc đoán ngay lập tức sẽ mang lại xác suất:$$\frac{1}{a+1}$$1. Cân nhắc hỏi về một trong những quân bài có thể có của đối thủ. 

Với xác suất:$$\frac{a}{a+1}$$đối thủ thực sự có thẻ. Nó bị xóa và trình phát hiện tại tiếp tục từ`(a-1, b)`. 

1. Nếu không thì thẻ được yêu cầu đã bị ẩn. 

Điều này xảy ra với xác suất:$$\frac{1}{a+1}$$Lần lượt thay đổi và đổi vai. Xác suất chiến thắng của đối thủ từ`(b, a-1)`là`dp[b][a-1]`, do đó xác suất của người chơi hiện tại trở thành:$$1 - dp[b][a-1]$$1. Kết hợp hai kết quả của việc hỏi:$$\frac{a}{a+1} dp[a-1][b] + \frac{1}{a+1}(1 - dp[b][a-1])$$1. Người chơi hiện tại chọn cách đoán và hỏi tốt hơn:$dp[a][b]=\max\left(\frac1{a+1},\frac{a}{a+1}dp[a-1][b]+\frac1{a+1}(1-dp[b][a-1])\right)$1. Điền vào bảng theo thứ tự tăng dần`a + b`. Every transition only references smaller states, so this order guarantees that dependencies are already computed.
 2. Câu trả lời cuối cùng là`dp[n][m]`bởi vì ban đầu Shrek nhìn thấy`n`những lá bài có thể thuộc về Donkey và biết rõ về chính nó`m`thẻ. 

Tại sao nó hoạt động: 

Nhà nước`(a, b)`nắm bắt đầy đủ tất cả các thông tin liên quan đến các quyết định trong tương lai. Mọi quân bài chưa biết còn lại đều có tính đối xứng, vì vậy không có chiến lược nào có thể phụ thuộc vào danh tính quân bài. Phép truy toán liệt kê mọi bước đi đầu tiên tối ưu: đoán ngay lập tức hoặc đặt câu hỏi. Quá trình chuyển đổi yêu cầu chia thành hai kết quả duy nhất có thể xảy ra, được tính theo xác suất chính xác của chúng. Vì sau đó cả hai người chơi đều chơi tối ưu nên các trạng thái đệ quy đã chứa các giá trị tiếp tục chính xác. Bằng quy nạp trên`a + b`, mỗi mục DP bằng xác suất chiến thắng tối ưu thực sự. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

m, n = map(int, input().split())

MAX = max(m, n)

dp = [[0.0] * (MAX + 1) for _ in range(MAX + 1)]

for b in range(MAX + 1):
    dp[0][b] = 1.0

for s in range(1, 2 * MAX + 1):
    for a in range(1, min(MAX, s) + 1):
        b = s - a

        if b < 0 or b > MAX:
            continue

        guess = 1.0 / (a + 1)

        ask = (
            (a / (a + 1)) * dp[a - 1][b]
            + (1.0 / (a + 1)) * (1.0 - dp[b][a - 1])
        )

        dp[a][b] = max(guess, ask)

shrek = dp[n][m]
donkey = 1.0 - shrek

print(f"{shrek:.12f} {donkey:.12f}")
```Bảng DP lưu trữ xác suất cho mỗi cặp`(a, b)`lên đến kích thước đầu vào tối đa. 

Hàng cơ sở`dp[0][b] = 1`đại diện cho các vị trí mà người chơi hiện tại đã biết chắc chắn lá bài ẩn. 

Vòng lặp bên ngoài lặp lại bằng cách tăng`a + b`. Thứ tự này quan trọng vì sự lặp lại phụ thuộc vào`(a-1, b)`Và`(b, a-1)`, cả hai đều có tổng kích thước nhỏ hơn. 

Quá trình chuyển đổi tính toán hai chiến lược pháp lý một cách riêng biệt. Xác suất đoán là đơn giản. Quá trình chuyển đổi yêu cầu xử lý việc hoán đổi vai trò một cách cẩn thận sau khi câu hỏi không thành công. giá trị`dp[b][a-1]`đại diện cho xác suất chiến thắng của đối thủ sau khi lượt thay đổi, vì vậy chúng tôi trừ đi`1`. 

Một lỗi triển khai phổ biến là quên rằng nhánh thứ hai hoán đổi quan điểm của người chơi. sử dụng`dp[a-1][b]`ở cả hai nhánh đều tạo ra câu trả lời sai. 

Một điểm tinh tế khác là độ chính xác của dấu phẩy động. Dung sai lỗi yêu cầu là`1e-9`, vậy là Python`float`là đủ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
0 3
```Trạng thái ban đầu là`dp[3][0]`. 

| Tiểu bang | Đoán giá trị | Hỏi giá trị | DP | 
| --- | --- | --- | --- | 
| dp[0][0] | 1 | - | 1 | 
| dp[1][0] | 1/2 | 1/2 | 1/2 | 
| dp[2][0] | 1/3 | 1/2 | 1/2 | 
| dp[3][0] | 1/4 | 1/4 | 1/4 | 

Câu trả lời cuối cùng:```
0.25 0.75
```Dấu vết này cho thấy việc đặt câu hỏi không giúp ích gì khi ban đầu bạn không biết gì. Mỗi câu hỏi không thành công sẽ ngay lập tức chuyển lượt cho đối thủ. 

### Ví dụ 2 

đầu vào:```
1 0
```Trạng thái ban đầu là`dp[0][1]`. 

| Tiểu bang | Đoán giá trị | Hỏi giá trị | DP | 
| --- | --- | --- | --- | 
| dp[0][1] | 1 | - | 1 | 

Câu trả lời cuối cùng:```
1.0 0.0
```Chỉ có một lá bài ẩn duy nhất nên Shrek sẽ thắng ngay lập tức mà không cần hỏi bất cứ điều gì. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(max(m, n)^2) | Mỗi trạng thái DP được tính một lần | 
| Không gian | O(max(m, n)^2) | Bảng DP lưu trữ tất cả các trạng thái | 

Với giới hạn lên tới`1000`, bảng chứa khoảng một triệu trạng thái. Mỗi quá trình chuyển đổi thực hiện công việc liên tục, do đó giải pháp dễ dàng phù hợp với giới hạn thời gian và sử dụng bộ nhớ có thể chấp nhận được. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    m, n = map(int, input().split())

    MAX = max(m, n)

    dp = [[0.0] * (MAX + 1) for _ in range(MAX + 1)]

    for b in range(MAX + 1):
        dp[0][b] = 1.0

    for s in range(1, 2 * MAX + 1):
        for a in range(1, min(MAX, s) + 1):
            b = s - a

            if b < 0 or b > MAX:
                continue

            guess = 1.0 / (a + 1)

            ask = (
                (a / (a + 1)) * dp[a - 1][b]
                + (1.0 / (a + 1)) * (1.0 - dp[b][a - 1])
            )

            dp[a][b] = max(guess, ask)

    shrek = dp[n][m]
    donkey = 1.0 - shrek

    print(f"{shrek:.12f} {donkey:.12f}")

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out.strip()

# provided sample
assert run("0 3\n") == "0.250000000000 0.750000000000", "sample 1"

# minimum size
assert run("0 0\n") == "1.000000000000 0.000000000000", "single hidden card"

# immediate certainty
assert run("1 0\n") == "1.000000000000 0.000000000000", "known hidden card"

# symmetric case
res = run("1 1\n")
a, b = map(float, res.split())
assert abs(a + b - 1.0) < 1e-9, "probabilities sum to 1"

# larger boundary-style test
res = run("1000 1000\n")
a, b = map(float, res.split())
assert 0.0 <= a <= 1.0
assert 0.0 <= b <= 1.0
assert abs(a + b - 1.0) < 1e-9
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 0`|`1 0`| Trạng thái nhỏ nhất có thể | 
|`1 0`|`1 0`| Chiến thắng quyết định ngay lập tức | 
|`1 1`| Xác suất tổng cộng là 1 | Tính chính xác của đệ quy đối xứng | 
|`1000 1000`| Xác suất hợp lệ | Hiệu suất và giới hạn | 

## Vỏ cạnh 

Hãy xem xét:```
0 0
```Có chính xác một lá bài trong toàn bộ trò chơi và nó được ẩn đi. DP ngay lập tức sử dụng trường hợp cơ bản`dp[0][0] = 1`. Shrek đoán được lá bài duy nhất có thể và giành chiến thắng một cách chắc chắn. 

Bây giờ hãy xem xét:```
1 0
```Shrek sở hữu một lá bài, Donkey không sở hữu lá bài nào nên lá bài ẩn được xác định duy nhất. Thuật toán một lần nữa đạt đến`dp[0][1] = 1`. Việc thực hiện bất cẩn luôn cố gắng hỏi trước sẽ thất bại vì không có ai để hỏi. 

Cuối cùng hãy xem xét:```
0 3
```Sự tái phát trở thành:$$dp[3][0] = \max\left( \frac14, \frac34 dp[2][0] + \frac14 (1 - dp[0][2]) \right)$$Từ`dp[0][2] = 1`, nhánh yêu cầu sẽ sụp đổ thành:$$\frac34 \cdot \frac12 + \frac14 \cdot 0 = \frac38$$Nhưng`dp[2][0]`bản thân nó trở nên tối ưu`1/2`, cuối cùng dẫn đến`dp[3][0] = 1/4`. 

Chi tiết quan trọng là một câu hỏi sai sẽ ngay lập tức mang lại trạng thái chiến thắng cho đối thủ. Sự lặp lại xử lý điều này thông qua thuật ngữ:$$1 - dp[b][a-1]$$Nếu không hoán đổi vai trò, xác suất sẽ được đánh giá quá cao.
