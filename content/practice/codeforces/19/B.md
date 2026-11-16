---
title: "CF 19B - Trợ lý thanh toán"
description: "Mỗi mục có hai thuộc tính. Nếu Bob thanh toán cho món hàng đó, nhân viên thu ngân sẽ dành t[i] giây để xử lý món hàng đó và Bob cũng tiêu c[i] tiền. Trong t[i] giây đó, Bob có thể lấy trộm các vật phẩm khác, một vật phẩm mỗi giây."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dp"]
categories: ["algorithms"]
codeforces_contest: 19
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 19"
rating: 1900
weight: 19
solve_time_s: 185
verified: true
draft: false
---
[CF 19B - Trợ lý thanh toán](https://codeforces.com/problemset/problem/19/B) 

**Xếp hạng:** 1900 
**Thẻ:** dp 
**Thời gian giải:** 3m 5s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi mục có hai thuộc tính. Nếu Bob trả tiền cho món hàng đó thì nhân viên thu ngân sẽ chi`t[i]`giây để xử lý nó và Bob cũng dành`c[i]`tiền bạc. Trong những thời gian đó`t[i]`giây, Bob có thể lấy trộm các vật phẩm khác, mỗi giây một vật phẩm. 

Đơn đặt hàng do Bob kiểm soát hoàn toàn, điều đó có nghĩa là điều duy nhất quan trọng là mặt hàng nào được thanh toán. Khi chúng tôi quyết định các mặt hàng phải trả tiền, tổng thời gian xử lý của chúng sẽ xác định số lượng mặt hàng khác có thể bị đánh cắp. 

Giả sử Bob trả tiền cho`k`các mặt hàng có tổng thời gian thu ngân là`T`. Những thứ kia`k`các mặt hàng phải trả tiền đã có được một cách hợp pháp và trong thời gian`T`giây Bob có thể ăn trộm nhiều nhất`T`các mục bổ sung. Vậy tổng số vật phẩm thu được là:```
k + T
```Để lấy tất cả`n`các mặt hàng, chúng tôi cần:```
k + T >= n
```Nhiệm vụ trở thành:```
Choose a subset of items minimizing total cost,
such that sum(t[i]) + number_of_chosen_items >= n.
```Sự cải cách này là toàn bộ vấn đề. 

Những ràng buộc là manh mối tiếp theo. Chúng tôi có tối đa 2000 mặt hàng và mỗi mặt hàng`t[i]`cũng nhiều nhất là 2000. Việc liệt kê tập hợp con mạnh mẽ sẽ yêu cầu kiểm tra`2^n`những khả năng mà không thể có được`n = 2000`. Thậm chí`O(n^3)`bắt đầu trở nên khó chịu khi ở dưới giới hạn 1 giây trong Python. 

Cấu trúc gợi ý mạnh mẽ về lập trình động. Trạng thái phụ thuộc vào số lượng mặt hàng mà chúng tôi có thể đảm bảo có được và giá trị đó không bao giờ vượt quá`n`. MỘT`O(n^2)`DP là hoàn toàn khả thi ở đây. 

Có một số trường hợp khó bỏ sót. 

Hãy xem xét một mục có thời gian xử lý bằng 0:```
1
0 5
```Câu trả lời là`5`. 

Việc trả tiền cho vật phẩm này sẽ không mất thời gian trộm cắp, nhưng bản thân vật phẩm đó vẫn được tính là đã nhận được. Giải thích sai có thể cho rằng các mục không có thời gian là vô dụng. 

Một tình huống khó khăn khác là khi chỉ cần một món đồ đắt tiền là đủ:```
3
10 100
0 1
0 1
```Câu trả lời đúng là`1`. 

Trả tiền cho một trong hai vật phẩm giá rẻ sẽ nhận được một vật phẩm và không mất thời gian trộm cắp, vì vậy chúng ta vẫn cần nhiều vật phẩm trả phí hơn. Nhưng việc trả tiền cho món hàng đầu tiên sẽ mang lại:```
1 paid item + 10 stealable items >= 3
```vì vậy trả tiền`100`thực sự là không cần thiết vì việc trả tiền cho cả hai món đồ rẻ chỉ tốn chi phí`2`. 

Đợi đã, hãy kiểm tra cẩn thận. Thanh toán cho cả hai mặt hàng giá rẻ mang lại:```
2 paid items + 0 stealing time = 2 items
```vẫn chưa đủ. Vì vậy mức tối thiểu thực sự là`100`. 

Ví dụ này cho thấy tại sao chỉ tính tổng thời gian ăn trộm là sai. Bản thân các mặt hàng phải trả tiền cũng phải được tính. 

Một trường hợp nguy hiểm khác là khi kết hợp nhiều lần nhỏ:```
4
1 5
1 5
1 5
1 5
```Câu trả lời là`10`. 

Thanh toán cho hai mặt hàng mang lại:```
2 paid + 2 stealable = 4
```Một chiến lược tham lam như “luôn mua với thời gian lớn nhất” không tồn tại ở đây vì tất cả các mặt hàng đều đối xứng. Việc tối ưu hóa về cơ bản là tổ hợp. 

## Phương pháp tiếp cận 

Giải pháp brute-force rất đơn giản. Đối với mỗi tập hợp con của các mục, hãy tính:```
paid_count
total_time
total_cost
```Nếu như:```
paid_count + total_time >= n
```thì tập hợp con này hợp lệ và chúng tôi giảm thiểu chi phí. 

Lực lượng vũ phu là chính xác bởi vì mọi bộ mặt hàng trả phí có thể được kiểm tra chính xác một lần. Vấn đề là kích thước của không gian tìm kiếm. Với`n = 2000`, có:```
2^2000
```tập hợp con, điều này là không thể về mặt thiên văn. 

Quan sát quan trọng là mọi mặt hàng phải trả phí đều đóng góp chính xác:```
t[i] + 1
```hướng tới yêu cầu cuối cùng. 

các`+1`xuất phát từ bản thân vật phẩm đó được lấy một cách hợp pháp và`t[i]`đến từ số lượng mặt hàng bổ sung có thể lấy được trong quá trình thanh toán. 

Vì vậy, mỗi mục được chọn hoạt động giống như một đối tượng với:```
value = t[i] + 1
cost = c[i]
```Chúng tôi cần tổng giá trị ít nhất`n`đồng thời giảm thiểu chi phí. 

Đó là một DP kiểu ba lô cổ điển. 

Định nghĩa:```
dp[x] = minimum cost to achieve total contribution exactly x
```trong đó đóng góp có nghĩa là:```
sum(t[i] + 1)
```Giá trị lớn hơn`n`là không cần thiết bởi vì một khi chúng ta đạt được`n`, tất cả các mặt hàng có thể được lấy. 

Đối với mỗi mục, chúng tôi hoặc lấy nó hoặc bỏ qua nó. Điều này trở thành quá trình chuyển đổi ba lô 0/1 tiêu chuẩn. 

Lực lượng vũ phu hoạt động vì vấn đề cơ bản là về các tập hợp con. DP hoạt động vì thông tin duy nhất quan trọng về một tập hợp con là tổng đóng góp của nó chứ không phải thứ tự chính xác của các mục. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(1) | Quá chậm | 
| DP tối ưu | O(n^2) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các mục. 
2. Với mỗi mục, hãy tính phần đóng góp của nó:```
gain = t[i] + 1
```Điều này thể hiện tổng số mặt hàng có thể đạt được nếu chúng ta trả tiền cho mặt hàng này. 

1. Tạo một mảng DP có kích thước`n + 1`.```
dp[x] = minimum cost to achieve contribution x
```Khởi tạo tất cả các giá trị thành vô cùng ngoại trừ:```
dp[0] = 0
```bởi vì đạt được sự đóng góp`0`không tốn kém gì. 

1. Xử lý từng mục một. 

Đối với mỗi mục có:```
gain = t[i] + 1
cost = c[i]
```lặp lại các giá trị đóng góp ngược từ`n`ĐẾN`0`. 

Lặp lại ngược ngăn chặn việc sử dụng cùng một mục nhiều lần. 

1. Từ đóng góp trước đó`x`, chuyển sang:```
nx = min(n, x + gain)
```Chúng tôi giới hạn ở`n`vì giá trị lớn hơn là tương đương. Khi mức đóng góp đạt đến`n`, tất cả các mặt hàng đã có thể được lấy. 

1. Cập nhật:```
dp[nx] = min(dp[nx], dp[x] + cost)
```Điều này có nghĩa là chúng tôi giữ nguyên cách tốt nhất trước đó để đạt được`nx`, hoặc chúng tôi sử dụng mục hiện tại. 

1. Sau khi xử lý tất cả các mục, kết quả:```
dp[n]
```bởi vì sự đóng góp`n`có nghĩa:```
paid_items + stealable_items >= n
```đảm bảo tất cả các mặt hàng có được. 

### Tại sao nó hoạt động 

Bất biến DP là:```
After processing the first i items,
dp[x] stores the minimum possible cost
to achieve total contribution x.
```Mỗi mục có chính xác hai lựa chọn, được thực hiện hoặc không được thực hiện và quá trình chuyển đổi sẽ xem xét cả hai. Vì mỗi tập hợp con tương ứng với một chuỗi lựa chọn duy nhất nên DP sẽ ngầm khám phá tất cả các tập hợp con hợp lệ. 

Sự đóng góp của một tập hợp con là:```
sum(t[i] + 1)
```bằng:```
number_of_paid_items + total_stealing_time
```Một tập hợp con có giá trị chính xác khi số lượng này đạt ít nhất`n`. DP giảm thiểu chi phí trong số tất cả các tập hợp con như vậy, vì vậy câu trả lời cuối cùng là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10**18

n = int(input())
items = [tuple(map(int, input().split())) for _ in range(n)]

dp = [INF] * (n + 1)
dp[0] = 0

for t, c in items:
    gain = t + 1

    for x in range(n, -1, -1):
        if dp[x] == INF:
            continue

        nx = min(n, x + gain)
        dp[nx] = min(dp[nx], dp[x] + c)

print(dp[n])
```Mảng DP lưu trữ chi phí tối thiểu cho mỗi mức đóng góp có thể đạt được. Kích thước mảng chỉ`n + 1`vì giá trị lớn hơn`n`là không cần thiết. 

Việc lặp lại là chi tiết triển khai quan trọng nhất. Nếu chúng ta lặp đi lặp lại, một mục có thể cập nhật một trạng thái và sau đó ngay lập tức được sử dụng lại trong cùng một lần lặp, khiến vấn đề trở thành một chiếc ba lô không giới hạn do nhầm lẫn. 

Mũ lưỡi trai:```
nx = min(n, x + gain)
```tránh truy cập ngoài giới hạn và cũng nén tất cả các trạng thái "đủ tốt" thành một trạng thái mục tiêu cuối cùng. 

Giá trị vô cực phải lớn vì chi phí có thể đạt tới:```
2000 * 10^9
```Vì thế`10^18`là sự lựa chọn an toàn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
2 10
0 20
1 5
1 3
```Lợi ích là:```
3, 1, 2, 2
```Chúng tôi theo dõi DP. 

| Mục | Đạt được | Chi phí | Các bang cập nhật | 
| --- | --- | --- | --- | 
| Bắt đầu | - | - | dp[0] = 0 | 
| (2,10) | 3 | 10 | dp[3] = 10 | 
| (0,20) | 1 | 20 | dp[1] = 20, dp[4] = 30 | 
| (1,5) | 2 | 5 | dp[2] = 5, dp[4] = 15 | 
| (1,3) | 2 | 3 | dp[2] = 3, dp[4] = 8 | 

Câu trả lời cuối cùng:```
8
```Sự lựa chọn tối ưu là trả tiền cho hai mục cuối cùng. Tổng đóng góp của họ là:```
2 + 2 = 4
```đủ cho mọi món đồ. 

### Ví dụ 2 

đầu vào:```
3
0 5
0 6
0 7
```Mỗi mục đều có lợi`1`. 

| Mục | Đạt được | Chi phí | Các quốc gia có thể tiếp cận tốt nhất | 
| --- | --- | --- | --- | 
| Bắt đầu | - | - | dp[0] = 0 | 
| Đầu tiên | 1 | 5 | dp[1] = 5 | 
| Thứ hai | 1 | 6 | dp[2] = 11 | 
| Thứ ba | 1 | 7 | dp[3] = 18 | 

Câu trả lời cuối cùng:```
18
```Không thể ăn cắp vì mọi thời gian xử lý đều bằng không. Bob phải trả tiền cho mọi món hàng. 

Ví dụ này xác nhận rằng`+1`sự đóng góp từ chính mặt hàng phải trả tiền là điều cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Đối với mỗi`n`các mục, chúng tôi lặp lại nhiều nhất`n`Trạng thái DP | 
| Không gian | O(n) | Mảng DP có kích thước`n + 1`| 

Với`n <= 2000`, thuật toán thực hiện khoảng bốn triệu lần chuyển đổi trong trường hợp xấu nhất, dễ dàng nằm trong giới hạn trong Python. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    INF = 10**18

    n = int(input())
    items = [tuple(map(int, input().split())) for _ in range(n)]

    dp = [INF] * (n + 1)
    dp[0] = 0

    for t, c in items:
        gain = t + 1

        for x in range(n, -1, -1):
            if dp[x] == INF:
                continue

            nx = min(n, x + gain)
            dp[nx] = min(dp[nx], dp[x] + c)

    print(dp[n])

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()

    backup = sys.stdout
    sys.stdout = out

    solve()

    sys.stdout = backup
    return out.getvalue().strip()

# provided sample
assert run(
"""4
2 10
0 20
1 5
1 3
""") == "8", "sample 1"

# minimum size
assert run(
"""1
0 7
""") == "7", "single item"

# all zero times
assert run(
"""3
0 5
0 6
0 7
""") == "18", "must pay for all"

# one large time dominates
assert run(
"""5
10 4
0 100
0 100
0 100
0 100
""") == "4", "single item covers everything"

# equal values
assert run(
"""4
1 5
1 5
1 5
1 5
""") == "10", "choose any two"

# off-by-one contribution check
assert run(
"""2
0 3
1 10
""") == "10", "gain includes paid item itself"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mục duy nhất không có thời gian | 7 | Bản thân mặt hàng trả phí đóng góp một | 
| Tất cả đều bằng 0 | 18 | Không thể ăn trộm | 
| Một mục thời gian khổng lồ | 4 | Đóng góp lớn có thể bao gồm tất cả các hạng mục | 
| Tất cả các giá trị bằng nhau | 10 | DP kết hợp chính xác các đóng góp | 
| Trường hợp nhỏ từng cái một | 10 | Đóng góp phải được`t+1`, không`t`| 

## Vỏ cạnh 

Hãy xem xét đầu vào nhỏ nhất có thể:```
1
0 5
```Đóng góp của vật phẩm là:```
0 + 1 = 1
```DP chuyển tiếp từ:```
dp[0] = 0
```ĐẾN:```
dp[1] = 5
```Từ`n = 1`, câu trả lời trở thành`5`. 

Trường hợp này chứng tỏ rằng một mặt hàng phải trả phí được tính vào tổng số cuối cùng ngay cả khi không có thời gian trộm cắp. 

Bây giờ hãy xem xét:```
3
0 5
0 6
0 7
```Mỗi mục đóng góp chính xác`1`. Cách duy nhất để đạt được sự đóng góp`3`đang chọn cả ba mục. DP dần dần xây dựng:```
dp[1] = 5
dp[2] = 11
dp[3] = 18
```Không có lối tắt nào tồn tại vì tổng thời gian ăn cắp luôn bằng không. 

Một trường hợp tế nhị khác là:```
5
10 4
0 100
0 100
0 100
0 100
```Mục đầu tiên đóng góp:```
10 + 1 = 11
```đã vượt quá`n = 5`. 

DP ngay lập tức cập nhật:```
dp[5] = 4
```vì cái mũ:```
nx = min(n, x + gain)
```Nếu không có giới hạn này, việc triển khai sẽ lãng phí bộ nhớ hoặc có nguy cơ xảy ra lỗi lập chỉ mục. 

Cuối cùng, hãy xem xét:```
2
0 3
1 10
```Mục đầu tiên góp phần`1`, đóng góp thứ hai`2`. 

Chỉ trả tiền cho món hàng thứ hai đã thỏa mãn:```
2 >= n
```vậy câu trả lời là`10`. 

Triển khai sai khi sử dụng đóng góp`t`thay vì`t+1`sẽ kết luận sai rằng cả hai mục đều cần thiết và trả lại`13`.
