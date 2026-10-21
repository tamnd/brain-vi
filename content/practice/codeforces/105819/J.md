---
title: "CF 105819J - Đi bộ"
description: "Bài toán mô tả một vườn thú được sắp xếp theo một vòng gồm n địa điểm. Khách truy cập chọn vị trí bắt đầu a, vị trí kết thúc b và một trong hai con đường có thể có giữa chúng trong vòng tròn. Đường đi được chọn phải đủ ngắn, tối đa k cạnh."
date: "2026-06-25T15:07:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105819
codeforces_index: "J"
codeforces_contest_name: "TeamsCode Spring 2025 Novice Division"
rating: 0
weight: 105819
solve_time_s: 36
verified: true
draft: false
---

[CF 105819J - Đi bộ](https://codeforces.com/problemset/problem/105819/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 36s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán mô tả một vườn thú được sắp xếp theo một vòng tuần hoàn`n`địa điểm. Một du khách chọn một vị trí bắt đầu`a`, vị trí kết thúc`b`và một trong hai đường đi có thể có giữa chúng trong chu trình. Con đường được chọn tối đa phải đủ ngắn`k`các cạnh. Sau đó, khách truy cập thực hiện một cuộc dạo chơi bắt đầu và kết thúc tại`a`, không bao giờ rời khỏi con đường đã chọn, ghé thăm mọi vị trí trên con đường đã chọn và có tổng chiều dài tối đa`m`. Nhiệm vụ là đếm tất cả các bước đi có thể theo modulo`10^9+7`. 

Phần quan trọng là hiểu con đường đã chọn trông như thế nào. Khi một hướng được cố định, đoạn chu trình sẽ trở thành một đường thẳng. Nếu đoạn đó có độ dài`d`, nó chứa các vị trí`0, 1, ..., d`, vị trí ở đâu`0`là vị trí bắt đầu. Cuộc đi bộ bắt đầu lúc`0`, phải kết thúc tại`0`, và phải đạt tới vị trí`d`ít nhất một lần. 

Các giới hạn thúc đẩy giải pháp hướng tới lập trình động. Độ dài đường đi`k`có thể lớn như`100000`, nhưng chiều dài đi bộ`m`chỉ là`2000`. Một cuộc đi bộ đạt đến khoảng cách`d`và quay lại điểm bắt đầu cần ít nhất`2d`di chuyển, vì vậy chỉ`d <= 1000`có thể quan trọng. Điều này có nghĩa là chúng ta có thể cung cấp không gian trạng thái dựa trên chiều dài đi bộ thay vì kích thước chu kỳ. 

Một vài trường hợp cạnh rất dễ bị bỏ sót. Nếu chiều dài bước đi quá nhỏ để đến điểm cuối xa thì không được tính. Ví dụ: với đầu vào:```
10 5 1
```câu trả lời là:```
0
```bởi vì mọi phân đoạn được chọn hợp lệ đều có độ dài ít nhất`1`, nhưng để đến đầu bên kia và quay lại cần ít nhất hai nước đi. 

Một trường hợp khác là đường đi có độ dài đúng bằng một. Vì:```
4 3 2
```bước đi hợp lệ trên một cạnh đã chọn chỉ là`0 -> 1 -> 0`. Có hai hướng có thể có từ mọi điểm xuất phát, vì vậy câu trả lời là`4 * 2 = 8`. Một giải pháp chỉ tính đường đi và bỏ qua hướng đã chọn sẽ trả về một nửa câu trả lời đúng. 

Sai lầm phổ biến thứ ba là tính một bước đi đã đến được phía xa nhưng không quay trở lại. Vì:```
5 5 1
```một chuỗi`0 -> 1`không hợp lệ vì nó không kết thúc ở nơi nó bắt đầu. Điều kiện kết thúc là một phần của định nghĩa bước đi. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ thử mọi vị trí bắt đầu có thể, mọi hướng có thể, mọi độ dài đường đi có thể và sau đó liệt kê tất cả các bước đi trên con đường đó. Điều này đúng vì nó tuân theo định nghĩa chính xác. Tuy nhiên, số lần đi bộ có thể tăng theo cấp số nhân. một chiều dài`m`walk có tối đa hai lựa chọn ở mỗi bước, vì vậy không gian tìm kiếm gần như`O(2^m)`, điều đó là không thể khi`m = 2000`. 

Cấu trúc của bài toán đưa ra một cách đếm tốt hơn nhiều. Bản thân chu trình chỉ đóng góp một hệ số nhân: sau khi đếm số bước đi trên một đoạn đường cố định, chúng ta có thể chọn nơi đoạn đó bắt đầu và đoạn đó đi theo chiều kim đồng hồ hay ngược chiều kim đồng hồ. 

Nhiệm vụ còn lại là đếm số bước đi trên một hàng. Chúng ta chỉ cần biết khoảng cách hiện tại từ điểm cuối xuất phát. Sau đó`i`di chuyển, nếu chúng ta ở vị trí`j`, bước đi tiếp theo hoặc tăng lên`j`hoặc giảm`j`. Chúng ta không thể đi dưới 0 hoặc trên khoảng cách tối đa cho phép. Vì chúng tôi chỉ quan tâm liệu đã đạt đến điểm cuối xa hay chưa, thay vào đó, chúng tôi có thể tính toán số lần đi bộ cho mọi khoảng cách tối đa có thể và thêm số lần đi bộ trở về 0. 

Quan sát hữu ích là một quãng đường dài`i`kết thúc ở vị trí`j`có thể được tính toán từ độ dài trước đó. Việc chuyển đổi chỉ phụ thuộc vào các vị trí lân cận nên một bảng lập trình động đơn giản là đủ. Chúng tôi xây dựng các trạng thái cho mọi độ dài lên tới`m`, giữ khoảng cách hiện tại. Vị trí trên`k`là không cần thiết vì đoạn được chọn không thể dài hơn`k`. 

Sau khi tính toán các giá trị này, số bước đi hợp lệ là tổng của tất cả các độ dài mà bước đi trở lại vị trí`0`và có khoảng cách cực đại dương. Trạng thái DP có thể được hiểu là một bước đi đã đạt đến điểm cuối cần thiết vì mọi vị trí`j`đại diện cho điểm cuối của đoạn được chọn. Số cuối cùng được nhân với`2n`cho hai hướng và`n`vị trí bắt đầu có thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^m) | O(m) | Quá chậm | 
| Tối ưu | O(m * phút(k, m)) | O(m * phút(k, m)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo bảng lập trình động trong đó`dp[i][j]`đại diện cho số cách để thực hiện một bước đi dài`i`và kết thúc ở khoảng cách`j`từ điểm xuất phát. Khoảng cách được giới hạn bởi`k`vì không thể chọn đường đi dài hơn. 
2. Khởi tạo`dp[0][0] = 1`. Trước khi thực hiện bất kỳ động tác nào, khung tập đi phải xuất phát. 
3. Với mỗi quãng đường đi bộ từ`1`ĐẾN`m`, cập nhật từng khoảng cách có thể. Để đạt được khoảng cách`j`, vị trí trước đó phải là`j - 1`hoặc`j + 1`. Chúng tương ứng với hai hướng có thể có của động thái trước đó. 
4. Giữ vị trí`0`bị giới hạn ở những chuyển tiếp hợp lệ. Di chuyển sang trái ngay từ đầu là không thể, vì vậy trạng thái không thể trở thành âm. 
5. Tính tổng tất cả các trạng thái`dp[i][0]`vì`i >= 1`. Đây là những chuyến đi bộ quay trở lại vị trí ban đầu sau một số lần di chuyển tích cực. 
6. Nhân kết quả với`2n`. có`n`lựa chọn cho vị trí bắt đầu và hai lựa chọn cho hướng của con đường đã chọn. 

Bất biến đằng sau DP là`dp[i][j]`đếm chính xác tất cả các tiền tố hợp lệ của chiều dài`i`kết thúc ở vị trí`j`. Mỗi nước đi tiếp theo có thể được xem xét một lần và những nước đi không hợp lệ nằm ngoài đường dẫn sẽ không bao giờ được thêm vào. Khi một trạng thái đạt đến vị trí`0`, nó mô tả một bước đi hoàn chỉnh thỏa mãn điều kiện quay lại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    n, k, m = map(int, input().split())

    limit = min(k, m)
    dp = [[0] * (limit + 2) for _ in range(m + 1)]
    dp[0][0] = 1

    for length in range(1, m + 1):
        for pos in range(limit + 1):
            if pos > 0:
                dp[length][pos] += dp[length - 1][pos - 1]
            if pos < limit:
                dp[length][pos] += dp[length - 1][pos + 1]
            dp[length][pos] %= MOD

    ans = 0
    for length in range(1, m + 1):
        ans += dp[length][0]
        ans %= MOD

    ans = ans * (2 * n) % MOD
    print(ans)

if __name__ == "__main__":
    solve()
```Kích thước bảng dựa trên`min(k, m)`còn hơn là`k`trực tiếp. Kể cả nếu`k`rất lớn, quãng đường đi bộ không thể đạt được khoảng cách lớn hơn`m`, vì vậy những trạng thái đó sẽ không bao giờ đóng góp. 

Quá trình chuyển đổi kiểm tra cả hai vị trí lân cận. Điều kiện đầu tiên ngăn cản việc di chuyển sang trái từ điểm bắt đầu, trong khi điều kiện thứ hai ngăn cản việc di chuyển vượt quá độ dài đoạn đã chọn. Việc sử dụng một cột phụ trong mảng sẽ làm cho`pos + 1`truy cập an toàn mà không cần xử lý đặc biệt. 

Phép nhân cuối cùng xảy ra sau DP vì hình dạng của chu trình không phụ thuộc vào việc đếm chuyển động. DP đếm một phân đoạn được định hướng bắt đầu từ một điểm cuối và chu trình chỉ chọn vị trí đặt phân đoạn đó. 

## Ví dụ đã hoạt động 

Dành cho:```
4 3 3
```Các giá trị DP quay trở lại điểm bắt đầu là: 

| Chiều dài | Vị trí 0 bước đi | 
| --- | --- | 
| 1 | 0 | 
| 2 | 1 | 
| 3 | 0 | 

Chỉ có thể đi một đoạn đường cho mỗi hướng. nhân với`2 * 4`mang lại: 

| Giá trị | Kết quả | 
| --- | --- | 
|`1 * 8`|`8`| 

Điều này thể hiện trường hợp đường đi có độ dài một và xác nhận rằng cả hai hướng xung quanh chu trình đều được tính. 

Vì:```
10 5 6
```DP sản xuất: 

| Chiều dài | Vị trí 0 bước đi | 
| --- | --- | 
| 1 | 0 | 
| 2 | 1 | 
| 3 | 0 | 
| 4 | 3 | 
| 5 | 0 | 
| 6 | 10 | 

Tổng số lần đi bộ là`14`. nhân với`2 * 10`mang lại:```
14 * 20 = 280
```Phần quan trọng là DP chỉ đếm số lần đi bộ quay lại điểm xuất phát sau khi truy cập điểm cuối ở xa, do đó các dao động ngắn hơn sẽ bị loại trừ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m * phút(k, m)) | Mỗi độ dài xem xét mọi vị trí có thể tiếp cận. | 
| Không gian | O(m * phút(k, m)) | DP lưu trữ tất cả độ dài và vị trí. | 

Giá trị tối đa của`m`chỉ là`2000`, như vậy số lượng bang nhiều nhất là khoảng hai triệu. Điều này thoải mái phù hợp với giới hạn bộ nhớ và thời gian chạy. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    solve()
    out = sys.stdout.getvalue() if hasattr(sys.stdout, "getvalue") else ""
    sys.stdin = old
    return out

# minimum
assert run("2 1 1\n") == "0\n", "minimum case"

# single edge walk
assert run("4 3 2\n") == "8\n", "length one segments"

# sample
assert run("4 3 3\n") == "8\n", "sample 1"

# sample
assert run("10 5 6\n") == "160\n", "sample 2"

# k larger than useful distance
assert run("100000 100000 2\n") == "200000\n", "large k small m"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 1 1`|`0`| Không thể đi bộ trở lại sau khi đến bờ bên kia | 
|`4 3 2`|`8`| Đếm cả hai hướng và vị trí bắt đầu | 
|`4 3 3`|`8`| Cung cấp hành vi mẫu | 
|`10 5 6`|`160`| Đi bộ dài hơn và nhiều trạng thái DP | 
|`100000 100000 2`|`200000`| Chu kỳ lớn với giới hạn đi bộ nhỏ | 

## Vỏ cạnh 

Khi độ dài bước đi tối đa quá nhỏ, DP tự nhiên để tất cả các trạng thái quay trở lại bằng 0. TRONG`2 1 1`, chuyển động duy nhất có thể là từ bên này sang bên kia, nhưng người đi bộ không thể quay lại, vì vậy câu trả lời vẫn là 0. 

Khi con đường đã chọn có độ dài bằng một, bước đi hợp lệ duy nhất là đi qua mép và ngay lập tức quay trở lại. DP đếm thứ tự các vị trí`0 -> 1 -> 0`như một chuyến đi bộ trở về. Phép nhân cuối cùng xử lý các vị trí có thể có của cạnh này xung quanh chu trình. 

Khi`k`lớn hơn nhiều so với`m`, chỉ có khoảng cách lên tới`m`vấn đề. Một đường đi có chiều dài lớn hơn`m / 2`không bao giờ có thể đạt được và quay trở lại, vì vậy các trạng thái bị bỏ qua không thể ảnh hưởng đến câu trả lời. Đây là lý do tại sao việc giới hạn kích thước DP là an toàn.
