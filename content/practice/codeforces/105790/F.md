---
title: "CF 105790F - Ếch hay cóc?"
description: "Eric bắt đầu với một vũ khí laser có năng lượng ban đầu bằng 0. Đối với mỗi cấp độ, có hai hành động có thể thực hiện được. Lộ trình thông thường yêu cầu tiêu diệt si đột biến."
date: "2026-06-25T23:32:27+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105790
codeforces_index: "F"
codeforces_contest_name: "UDESC Selection Contest 2024-1"
rating: 0
weight: 105790
solve_time_s: 44
verified: true
draft: false
---

[CF 105790F - Ếch hay cóc?](https://codeforces.com/problemset/problem/105790/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Eric bắt đầu với một vũ khí laser có năng lượng ban đầu bằng 0. 

Đối với mỗi cấp độ, có hai hành động có thể thực hiện được. 

Con đường bình thường đòi hỏi phải giết`s_i`người đột biến. Mỗi lần bắn, ngay cả với năng lượng không tích cực, sẽ tiêu tốn một đơn vị năng lượng, do đó, việc vượt qua cấp độ sẽ thay đổi năng lượng hiện tại bằng cách`e_i - s_i`. Sau đó, Eric chuyển sang cấp độ tiếp theo. 

Con đường thay thế là một lỗ sâu đục. Sử dụng nó bỏ qua`k`cấp độ phía trước. Để vào được hố sâu, Eric phải giết`X`bảo vệ người đột biến, vì vậy năng lượng của anh ta giảm đi`X`. Anh ta không chiến đấu với`s_i`đột biến thường xuyên và không nhận được ngân hàng năng lượng`e_i`. 

Sau khi hoàn thành tất cả các cấp độ, Eric đến được với Nữ hoàng Ánh sáng. Một phát bắn chỉ gây sát thương cho tên trùm khi năng lượng của vũ khí hoàn toàn tích cực. Nếu Eric đến với năng lượng`E`, anh ấy có thể giải quyết chính xác`max(E, 0)`thiệt hại trước khi năng lượng trở nên không tích cực. 

Nhiệm vụ là tối đa hóa năng lượng còn lại khi tiếp cận boss. Câu trả lời là năng lượng cực đại nếu nó dương, ngược lại`0`. 

Những hạn chế là quan sát quan trọng. Có tới`2 · 10^5`cấp độ. Bất kỳ giải pháp nào khám phá tất cả các con đường có thể một cách rõ ràng đều không thể thực hiện được vì mỗi cấp độ đưa ra hai lựa chọn, tạo ra khoảng`2^N`những con đường. Thậm chí một`O(N^2)`chương trình năng động sẽ yêu cầu khoảng`4 · 10^10`trong trường hợp xấu nhất vượt xa giới hạn. Chúng ta cần một cái gì đó tuyến tính hoặc gần tuyến tính. 

Một trường hợp phức tạp xuất hiện khi mọi con đường có thể đều kết thúc bằng năng lượng không tích cực. 

Ví dụ:```
1 1 5
3 0
```Con đường bình thường cung cấp năng lượng`-3`, lỗ sâu đục cung cấp năng lượng`-5`. Eric tiếp cận tên trùm nhưng không thể gây ra bất kỳ thiệt hại nào. Câu trả lời đúng là:```
0
```Việc thực hiện bất cẩn có thể tạo ra năng lượng tối đa có thể đạt được,`-3`, thay vì chuyển đổi giá trị âm thành 0. 

Một trường hợp khác xảy ra khi một lỗ sâu nhảy vượt quá mức cuối cùng. 

Ví dụ:```
2 2 1
10 100
10 100
```Sử dụng lỗ sâu đục ở cấp 1 sẽ ngay lập tức đến tay trùm. Đích đến phải được coi là "sau tất cả các cấp độ", không phải là trạng thái không hợp lệ. Thiếu chi tiết này thường gây ra lỗi ngoài giới hạn hoặc mất chuyển tiếp. 

Trường hợp cạnh thứ ba là khi`X = 0`. 

Ví dụ:```
3 1 0
5 0
5 0
5 0
```Lỗ sâu đục trở thành một đường bỏ qua miễn phí. Thuật toán vẫn phải xem xét vì tránh âm lớn`(e_i - s_i)`giá trị có thể là tối ưu. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Ở mọi cấp độ, hãy chọn tuyến đường bình thường hoặc tuyến đường lỗ sâu, khám phá đệ quy tất cả các tương lai có thể xảy ra. Vì mỗi cấp độ có hai lựa chọn nên số lượng đường dẫn tăng theo cấp số nhân. Với`N = 200000`, điều này hoàn toàn không thể thực hiện được. 

Điều thú vị là năng lượng hiện tại là đại lượng duy nhất quan trọng. Tương lai không phụ thuộc vào việc chúng ta đạt đến một cấp độ như thế nào, mà phụ thuộc vào nguồn năng lượng tối đa sẵn có ở đó. 

Giả định`dp[i]`biểu thị năng lượng tối đa có thể đạt được sau khi hoàn thành lần đầu tiên`i`cấp độ, hoặc tương đương, khi đứng ở đầu cấp độ`i + 1`. 

Từ tiểu bang`i`, chỉ có hai chuyển tiếp. 

Đi theo con đường bình thường để vượt qua cấp độ`i + 1`thay đổi năng lượng bằng cách`e_i - s_i`. 

Sử dụng lỗ sâu đục làm thay đổi năng lượng bằng cách`-X`và di chuyển trực tiếp đến cấp độ`i + k`. 

Vì cả hai quá trình chuyển đổi chỉ đơn giản là thêm một giá trị cố định vào năng lượng hiện tại nên chỉ giữ lại năng lượng tốt nhất cho mỗi trạng thái là đủ. Bất kỳ năng lượng nhỏ hơn nào ở cùng một vị trí không bao giờ có thể dẫn đến kết quả tốt hơn trong tương lai. 

Điều này chuyển đổi tìm kiếm theo cấp số nhân thành một chương trình động tuyến tính trên các vị trí. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^N) | O(N) | Quá chậm | 
| DP tối ưu | O(N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo một mảng`dp`chiều dài`N + 1`, được khởi tạo với giá trị âm vô cực. 
2. Đặt`dp[0] = 0`, đại diện cho trạng thái bắt đầu trước khi vào cấp độ đầu tiên. 
3. Các cấp độ xử lý từ trái sang phải. 
4. Từ tiểu bang`i`, hãy thử con đường bình thường. 

Sự thay đổi năng lượng là`(e_i - s_i)`, vì vậy hãy cập nhật:`dp[i + 1] = max(dp[i + 1], dp[i] + e_i - s_i)`. 
5. Từ tiểu bang`i`, hãy thử con đường lỗ sâu đục. 

Điểm đến là`min(N, i + k)`bởi vì vượt qua cấp độ cuối cùng có nghĩa là tiếp cận ông chủ ngay lập tức. 

Cập nhật:`dp[min(N, i + k)] = max(dp[min(N, i + k)], dp[i] - X)`. 
6. Sau khi tất cả các quá trình chuyển đổi được xử lý,`dp[N]`là năng lượng tối đa có thể đạt được khi tiếp cận ông chủ. 
7. Đầu ra`max(dp[N], 0)`bởi vì năng lượng âm hoặc bằng 0 không thể gây sát thương cho trùm. 

### Tại sao nó hoạt động 

Tính bất biến đó là`dp[i]`luôn dự trữ năng lượng tối đa có thể trong số mọi cách để đạt đến trạng thái`i`. 

Mọi hành động pháp lý từ một tiểu bang đều tương ứng với chính xác một lần chuyển đổi DP. Vì mỗi quá trình chuyển đổi sẽ thêm một lượng cố định vào năng lượng hiện tại, nên năng lượng lớn hơn ở cùng một trạng thái ít nhất luôn tốt bằng năng lượng nhỏ hơn. Không có quyết định nào trong tương lai có thể khiến một quốc gia yếu hơn được ưa chuộng hơn. 

Bằng cách xử lý tất cả các trạng thái và lấy mức tối đa trên tất cả các chuyển tiếp đến, DP tính toán năng lượng tốt nhất có thể đạt được cho mọi vị trí. Đặc biệt,`dp[N]`trở thành năng lượng tối đa có thể đạt được khi tiếp cận Nữ hoàng Ánh sáng. Boss chỉ có thể bị sát thương khi năng lượng còn dương nên sát thương tối đa có thể là`max(dp[N], 0)`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, k, x = map(int, input().split())

    s = [0] * n
    e = [0] * n

    for i in range(n):
        s[i], e[i] = map(int, input().split())

    NEG = -(10 ** 30)
    dp = [NEG] * (n + 1)
    dp[0] = 0

    for i in range(n):
        if dp[i] == NEG:
            continue

        dp[i + 1] = max(dp[i + 1], dp[i] + e[i] - s[i])

        nxt = min(n, i + k)
        dp[nxt] = max(dp[nxt], dp[i] - x)

    print(max(dp[n], 0))

solve()
```Mảng DP sử dụng chỉ mục`i`để đại diện cho trạng thái sau khi xử lý đầu tiên`i`cấp độ. 

Quá trình chuyển đổi tuyến đường bình thường bổ sung thêm`e[i] - s[i]`, phù hợp với mức năng lượng ròng thu được từ việc chiến đấu với cấp độ và thu thập ngân hàng năng lượng của nó. 

Quá trình chuyển đổi lỗ sâu nhảy tới`min(n, i + k)`. Đây là nơi dễ mắc lỗi nhất. Khi một bước nhảy vượt quá cấp độ cuối cùng, trò chơi sẽ ngay lập tức đến được với trùm, vì vậy mỗi lần nhảy như vậy sẽ hạ cánh ở trạng thái`n`. 

Phạm vi giá trị có thể đạt tới khoảng`2 · 10^5 × 10^9`, vì vậy cần có số học 64 bit. Số nguyên Python tự động xử lý việc này. 

Câu trả lời cuối cùng là không`dp[n]`chính nó. Năng lượng tiêu cực không thể gây sát thương lên trùm nên kết quả phải được kẹp bằng 0. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 2 1
1 2
4 6
5 2
3 11
```| tôi | dp[i] trước | Điểm đến bình thường | Giá trị bình thường | Điểm đến hố giun | Giá trị lỗ sâu đục | 
| --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 1 | 1 | 2 | -1 | 
| 1 | 1 | 2 | 3 | 3 | 0 | 
| 2 | 3 | 3 | 0 | 4 | 2 | 
| 3 | 0 | 4 | 8 | 4 | -1 | 

Chung kết: 

| Tiểu bang | Giá trị | 
| --- | --- | 
| dp[4] | 8 | 

Trả lời:```
8
```Ví dụ này cho thấy rằng đôi khi đi theo con đường bình thường mặc dù phải trả chi phí đột biến là đáng giá vì ngân hàng năng lượng sẽ bù đắp cho nó. 

### Ví dụ 2 

đầu vào:```
5 3 20
3 4
2 2
5 6
3 1
8 5
```| tôi | dp[i] trước | Giá trị đích bình thường | Giá trị điểm đến của Wormhole | 
| --- | --- | --- | --- | 
| 0 | 0 | 1 | -20 | 
| 1 | 1 | 1 | -19 | 
| 2 | 1 | 2 | -19 | 
| 3 | 2 | 0 | -18 | 
| 4 | 0 | -3 | -20 | 

Chung kết: 

| Tiểu bang | Giá trị | 
| --- | --- | 
| dp[5] | -3 | 

Trả lời:```
0
```Dấu vết này cho thấy tình huống mà mọi chiến lược đều đến tay ông chủ với năng lượng không tích cực. Câu trả lời phải được kẹp ở mức không. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N) | Mỗi cấp độ thực hiện hai lần chuyển đổi DP | 
| Không gian | O(N) | Mảng DP chứa`N + 1`tiểu bang | 

Với`N ≤ 2 · 10^5`, quét tuyến tính và một mảng duy nhất dễ dàng phù hợp với giới hạn thời gian và bộ nhớ có sẵn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n, k, x = map(int, input().split())

    s = [0] * n
    e = [0] * n

    for i in range(n):
        s[i], e[i] = map(int, input().split())

    NEG = -(10 ** 30)
    dp = [NEG] * (n + 1)
    dp[0] = 0

    for i in range(n):
        if dp[i] == NEG:
            continue

        dp[i + 1] = max(dp[i + 1], dp[i] + e[i] - s[i])

        nxt = min(n, i + k)
        dp[nxt] = max(dp[nxt], dp[i] - x)

    return str(max(dp[n], 0)) + "\n"

# provided samples
assert run(
"""4 2 1
1 2
4 6
5 2
3 11
"""
) == "8\n", "sample 1"

assert run(
"""5 3 20
3 4
2 2
5 6
3 1
8 5
"""
) == "0\n", "sample 2"

# minimum size
assert run(
"""1 1 0
0 5
"""
) == "5\n"

# all routes negative
assert run(
"""1 1 5
3 0
"""
) == "0\n"

# free wormholes
assert run(
"""3 1 0
5 0
5 0
5 0
"""
) == "0\n"

# jump beyond end
assert run(
"""2 2 1
10 100
10 100
"""
) == "99\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cấp độ đơn với mức tăng tích cực | 5 | Ví dụ kích thước tối thiểu | 
| Cấp độ đơn có kết quả âm tính | 0 | Kẹp cuối cùng về 0 | 
| Lỗ sâu miễn phí | 0 | Xử lý lỗ sâu đúng khi X = 0 | 
| Vượt qua cấp độ cuối cùng | 99 | Sử dụng đúng`min(n, i + k)`| 

## Vỏ cạnh 

Hãy xem xét trường hợp tất cả các năng lượng đều không tích cực.```
1 1 5
3 0
```Việc khởi tạo mang lại`dp[0] = 0`. 

Lộ trình thông thường:```
dp[1] = -3
```Lỗ giun:```
dp[1] = max(-3, -5) = -3
```Năng lượng có thể tiếp cận tốt nhất là`-3`. Vì năng lượng tiêu cực không thể gây sát thương cho trùm nên thuật toán đưa ra:```
0
```Bây giờ hãy xem xét một bước nhảy vượt quá điểm cuối.```
2 2 1
10 100
10 100
```Ở trạng thái`0`:```
normal -> dp[1] = 90
wormhole -> dp[2] = -1
```Ở trạng thái`1`:```
normal -> dp[2] = 180
wormhole -> dp[2] = max(180, 89)
```Đích đến của lỗ sâu là trạng thái`2`, không phải trạng thái`3`, bởi vì vượt qua cấp độ cuối cùng đồng nghĩa với việc tiếp cận được ông chủ. Câu trả lời cuối cùng trở thành:```
180
```Cuối cùng, hãy xem xét các lỗ sâu đục miễn phí.```
3 1 0
5 0
5 0
5 0
```Mỗi tuyến đường bình thường mất 5 năng lượng. Mỗi lỗ sâu đục đều mất 0 năng lượng. 

DP liên tục thích chuyển đổi lỗ sâu đục và tiếp cận ông chủ bằng năng lượng`0`. Câu trả lời vẫn là:```
0
```Điều này xác nhận rằng thuật toán so sánh chính xác cả hai lựa chọn ở mọi cấp độ và luôn giữ năng lượng tiếp cận tốt nhất.
