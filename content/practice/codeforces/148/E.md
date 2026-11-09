---
title: "CF 148E - Sứ"
description: "Chúng tôi có một số kệ đựng đồ sứ. Bên trong một giá, các mục tạo thành một dòng và tại bất kỳ thời điểm nào, chúng tôi chỉ có thể xóa mục ngoài cùng bên trái hoặc mục ngoài cùng bên phải hiện tại. Sau khi xóa một mục, mục tiếp theo ở bên đó sẽ có thể truy cập được."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dp"]
categories: ["algorithms"]
codeforces_contest: 148
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 105 (Div. 2)"
rating: 1900
weight: 148
solve_time_s: 146
verified: true
draft: false
---

[CF 148E - Sứ](https://codeforces.com/problemset/problem/148/E) 

**Xếp hạng:** 1900 
**Thẻ:** dp 
**Thời gian giải:** 2 phút 26s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một số kệ đựng đồ sứ. Bên trong một giá, các mục tạo thành một dòng và tại bất kỳ thời điểm nào, chúng tôi chỉ có thể xóa mục ngoài cùng bên trái hoặc mục ngoài cùng bên phải hiện tại. Sau khi xóa một mục, mục tiếp theo ở bên đó sẽ có thể truy cập được. 

Công chúa hét lên chính xác`m`nhiều lần, chính xác như vậy`m`tổng số mặt hàng phải được loại bỏ trên tất cả các kệ. Mỗi mục đều có một giá trị và chúng tôi muốn tổng giá trị tối đa có thể có của các mục bị xóa. 

Chi tiết quan trọng là các kệ độc lập. Việc xóa các mục khỏi một giá không bao giờ làm thay đổi các mục có sẵn trên một giá khác. Sự tương tác duy nhất xuất phát từ yêu cầu chung là tổng số mặt hàng bị loại bỏ trên tất cả các kệ bằng nhau`m`. 

Số lượng kệ nhiều nhất là`100`, và mỗi ngăn chứa nhiều nhất`100`mặt hàng. Tổng số mặt hàng có thể đạt`10000`, khớp với giới hạn trên của`m`. 

Một lực lượng vũ phu đối với tất cả các trình tự loại bỏ có thể xảy ra là vô vọng. Ngay cả một kệ duy nhất có chiều dài`100`đại khái đã có rồi`2^100`lựa chọn trái/phải. Chúng ta cần khai thác cấu trúc. 

Kích thước kệ nhỏ là quan sát quan trọng. Mỗi kệ riêng lẻ có tối đa`100`các mục, vì vậy chúng tôi có thể xử lý trước mọi số lượng mục có thể được lấy từ giá đó. Sau đó, vấn đề trở thành vấn đề lập trình động kiểu ba lô trên kệ. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Giả sử một kệ là:```
1 100 1
```và chúng tôi muốn lấy hai món đồ. Sự lựa chọn tốt nhất là`100 + 1 = 101`, thu được bằng cách lấy vật phẩm bên trái hai lần. Việc triển khai bất cẩn giả định "lấy một số từ bên trái và một số từ bên phải một cách độc lập" mà không xem xét sự chồng chéo có thể vô tình tính các trạng thái không thể thực hiện được. 

Một trường hợp khó khăn khác là khi tất cả các vật phẩm hữu ích đều tập trung ở một bên. 

đầu vào:```
1 3
5 100 1 1 1 1
```Câu trả lời đúng là`102`, không`3`. Trình tự tối ưu là tiếp tục lấy từ bên trái. Bất kỳ cách tiếp cận nào tham lam thay đổi các bên đều thất bại nặng nề. 

Một tình huống tế nhị hơn xuất hiện khi kết hợp các kệ. 

đầu vào:```
2 2
2 100 1
2 50 50
```Câu trả lời đúng là`150`, bằng cách lấy cả hai vật phẩm từ kệ thứ hai. Một chiến lược tham lam luôn chiếm mục lớn nhất hiện có trên toàn cầu sẽ chọn`100`đầu tiên, sau đó chỉ`50`, kết thúc bằng`150`ở đây một cách tình cờ. Sửa đổi nhỏ sẽ phá vỡ nó ngay lập tức. Vấn đề đòi hỏi phải xem xét tất cả sự phân bố của các lựa chọn giữa các kệ. 

## Phương pháp tiếp cận 

Ý tưởng về vũ lực rất đơn giản. Với mỗi tiếng hét, hãy chọn một cái kệ, sau đó chọn trái hoặc phải từ kệ đó. Điều này khám phá mọi trình tự loại bỏ hợp lệ. Nó đúng vì mọi chuỗi hành động có thể đều được kiểm tra. 

Vấn đề là yếu tố phân nhánh. Ở mỗi bước, chúng ta có thể có sẵn nhiều kệ và mỗi kệ có hai lựa chọn. Ngay cả khi chúng ta đơn giản hóa và chỉ nghĩ đến các quyết định trái/phải bên trong một kệ duy nhất, một kệ có kích thước`100`đã có rồi`2^100`khả năng. Với tối đa`10000`việc loại bỏ hoàn toàn, việc tìm kiếm toàn diện là hoàn toàn không thể. 

Quan sát quan trọng là thứ tự lấy ra bên trong một kệ không thành vấn đề một khi chúng ta biết có bao nhiêu món đồ được lấy từ bên trái và bao nhiêu món từ bên phải. 

Giả sử một kệ có mảng:```
a0 a1 a2 ... ak
```Nếu cuối cùng chúng ta loại bỏ`x`các mục từ bên trái và`y`các mục từ bên phải thì giá trị thu thập được cố định:```
prefix[x] + suffix[y]
```với sự hạn chế`x + y = t`, Ở đâu`t`là tổng số mục đã bị xóa khỏi giá này. 

Điều đó có nghĩa là mọi kệ và mọi thứ có thể`t`, chúng ta có thể tính toán trước:```
best[t] = maximum value obtainable by removing exactly t items
```Vì kích thước kệ tối đa là`100`, chúng ta có thể đơn giản thử mọi phép chia giữa trái và phải. 

Sau khi xử lý trước tất cả các giá một cách độc lập, vấn đề ban đầu trở thành:```
Choose how many items to take from each shelf so that the total is m and the gained value is maximized.
```Đây chính xác là một vấn đề lập trình động ba lô được nhóm. 

Chúng tôi xử lý từng kệ một. Cho phép:```
dp[j]
```là giá trị lớn nhất có thể đạt được sau khi xử lý một số giá và lấy chính xác`j`tổng số mặt hàng. 

Đối với mỗi giá, chúng tôi thử tất cả số lượng có thể được lấy từ giá đó và cập nhật DP. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n × m × s) | O(m) | Đã chấp nhận | 

Đây`s`tối đa là kích thước kệ tối đa`100`. 

## Hướng dẫn thuật toán 

1. Đọc tất cả các kệ. 

Mỗi kệ được lưu trữ dưới dạng một mảng các giá trị. 
2. Đối với một giá, hãy tính tổng tiền tố và tổng hậu tố.`prefix[i]`là tổng của số đầu tiên`i`mặt hàng.`suffix[i]`là tổng của số cuối cùng`i`mặt hàng. 

Điều này cho phép chúng ta đánh giá bất kỳ sự phân chia trái/phải nào trong thời gian không đổi. 
3. Với mọi số có thể`t`trong số các mặt hàng được lấy từ kệ này, hãy tính giá trị tốt nhất có thể đạt được. 

Chúng tôi thử mọi cách chia:```
left = x
right = t - x
```và tối đa hóa:```
prefix[x] + suffix[right]
```Điều này có tác dụng vì bất kỳ trình tự hợp lệ nào loại bỏ`x`các mục từ bên trái và`right`các mục từ bên phải luôn loại bỏ chính xác các phần tử ranh giới đó. 
4. Duy trì mảng DP toàn cầu.`dp[j]`có nghĩa là tổng giá trị tốt nhất có thể đạt được bằng cách sử dụng các kệ đã được xử lý và lấy chính xác`j`các mặt hàng tổng thể. 
5. Xử lý từng kệ một. 

Đối với giá hiện tại, hãy tạo một mảng DP mới. Đối với mỗi tổng số đã đạt được`j`, thử lấy`t`các mặt hàng từ kệ hiện tại. 

Chuyển tiếp:```
ndp[j + t] = max(ndp[j + t], dp[j] + best[t])
```6. Sau khi xử lý xong tất cả các giá, xuất ra`dp[m]`. 

### Tại sao nó hoạt động 

Đối với giá cố định, mọi trạng thái cuối cùng hợp lệ đều được xác định hoàn toàn bằng số lượng mục được xóa ở bên trái và bao nhiêu mục ở bên phải. Thứ tự nội bộ của những lần xóa đó không ảnh hưởng đến mục nào được thu thập. 

Vì vậy, bước tiền xử lý sẽ tính toán chính xác giá trị tối ưu cho mọi số lượng mục có thể được lấy từ giá đó. 

DP toàn cầu sau đó sẽ xem xét mọi phân phối có thể có của`m`loại bỏ trên các kệ. Vì mỗi kệ đóng góp độc lập sau khi số lượng lấy của nó được cố định, nên việc kết hợp chúng với DP ba lô sẽ khám phá tất cả các giải pháp toàn cầu hợp lệ chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10**18

n, m = map(int, input().split())

dp = [-INF] * (m + 1)
dp[0] = 0

for _ in range(n):
    data = list(map(int, input().split()))
    k = data[0]
    a = data[1:]

    prefix = [0] * (k + 1)
    suffix = [0] * (k + 1)

    for i in range(k):
        prefix[i + 1] = prefix[i] + a[i]

    for i in range(k):
        suffix[i + 1] = suffix[i] + a[k - 1 - i]

    best = [0] * (k + 1)

    for take in range(k + 1):
        cur = 0
        for left in range(take + 1):
            right = take - left
            cur = max(cur, prefix[left] + suffix[right])
        best[take] = cur

    ndp = dp[:]

    for used in range(m + 1):
        if dp[used] < 0:
            continue

        limit = min(k, m - used)

        for take in range(1, limit + 1):
            ndp[used + take] = max(
                ndp[used + take],
                dp[used] + best[take]
            )

    dp = ndp

print(dp[m])
```Quá trình xử lý trước cho một kệ là trọng tâm của giải pháp.`prefix[i]`lưu trữ tổng của số đầu tiên`i`các mặt hàng, trong khi`suffix[i]`lưu trữ tổng của cuối cùng`i`mặt hàng. Sau đó, mọi cách khả thi để thực hiện chính xác`take`các mục được thể hiện bằng cách chọn số lượng đến từ bên trái. 

Dòng:```
prefix[left] + suffix[right]
```là an toàn bởi vì`left + right = take`, do đó các phân đoạn đã chọn không bao giờ trùng lặp không chính xác. 

DP toàn cầu sử dụng mẫu ba lô được nhóm tiêu chuẩn. Chúng tôi sao chép DP trước đó vào`ndp`trước khi xử lý quá trình chuyển đổi cho giá hiện tại. Điều này ngăn việc vô tình sử dụng cùng một giá nhiều lần trong một lần lặp. 

Điều kiện:```
if dp[used] < 0:
    continue
```bỏ qua các trạng thái không thể truy cập được. 

Một chi tiết tinh tế khác là:```
limit = min(k, m - used)
```Nếu không có giới hạn này, chúng tôi có thể thử chuyển đổi vượt quá tổng số mục được yêu cầu. 

Tất cả các giá trị đều dương, do đó, việc khởi tạo các trạng thái không thể truy cập bằng giá trị âm lớn là đủ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 3
3 3 7 2
3 4 1 5
```Đối với kệ đầu tiên:```
[3, 7, 2]
```| lấy | giá trị tốt nhất | giải thích | 
| --- | --- | --- | 
| 0 | 0 | không lấy gì | 
| 1 | 3 | tối đa(3, 2) | 
| 2 | 10 | mất 3 và 7 | 
| 3 | 12 | lấy tất cả | 

Đối với kệ thứ hai:```
[4, 1, 5]
```| lấy | giá trị tốt nhất | giải thích | 
| --- | --- | --- | 
| 0 | 0 | không lấy gì | 
| 1 | 5 | lấy từ bên phải | 
| 2 | 9 | lấy 4 và 5 | 
| 3 | 10 | lấy tất cả | 

Bây giờ xử lý DP toàn cầu. 

Trạng thái ban đầu: 

| lấy | dp | 
| --- | --- | 
| 0 | 0 | 

Sau kệ đầu tiên: 

| lấy | dp | 
| --- | --- | 
| 0 | 0 | 
| 1 | 3 | 
| 2 | 10 | 
| 3 | 12 | 

Sau kệ thứ hai: 

| lấy | tổng số tốt nhất | 
| --- | --- | 
| 0 | 0 | 
| 1 | 5 | 
| 2 | 10 | 
| 3 | 15 | 

Câu trả lời cuối cùng là`15`. 

Dấu vết này cho thấy cách phân chia vấn đề một cách tự nhiên thành tối ưu hóa từng kệ độc lập cộng với DP phân bổ toàn cầu. 

### Ví dụ 2 

đầu vào:```
1 3
5 100 1 1 1 1
```Kệ tiền xử lý: 

| lấy | giá trị tốt nhất | 
| --- | --- | 
| 0 | 0 | 
| 1 | 100 | 
| 2 | 101 | 
| 3 | 102 | 
| 4 | 103 | 
| 5 | 104 | 

Tiến trình DP: 

| lấy | dp | 
| --- | --- | 
| 0 | 0 | 
| 1 | 100 | 
| 2 | 101 | 
| 3 | 102 | 

Trả lời:```
102
```Ví dụ này chứng minh tại sao các quyết định địa phương tham lam lại nguy hiểm. Chiến lược tối ưu liên tục loại bỏ từ cùng một phía. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n × m × s) | Đối với mỗi kệ, chúng tôi thử tất cả tổng số DP và tất cả số lượng có thể lấy được | 
| Không gian | O(m) | Chỉ có một mảng DP được lưu trữ | 

Từ`n ≤ 100`,`m ≤ 10000`và kích thước mỗi kệ`s ≤ 100`, giải pháp vừa vặn trong giới hạn. Số lần chuyển đổi thực tế có thể quản lý được bằng Python. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline
    INF = 10**18

    n, m = map(int, input().split())

    dp = [-INF] * (m + 1)
    dp[0] = 0

    for _ in range(n):
        data = list(map(int, input().split()))
        k = data[0]
        a = data[1:]

        prefix = [0] * (k + 1)
        suffix = [0] * (k + 1)

        for i in range(k):
            prefix[i + 1] = prefix[i] + a[i]

        for i in range(k):
            suffix[i + 1] = suffix[i] + a[k - 1 - i]

        best = [0] * (k + 1)

        for take in range(k + 1):
            for left in range(take + 1):
                right = take - left
                best[take] = max(
                    best[take],
                    prefix[left] + suffix[right]
                )

        ndp = dp[:]

        for used in range(m + 1):
            if dp[used] < 0:
                continue

            limit = min(k, m - used)

            for take in range(1, limit + 1):
                ndp[used + take] = max(
                    ndp[used + take],
                    dp[used] + best[take]
                )

        dp = ndp

    return str(dp[m]) + "\n"

# provided sample
assert run(
"""2 3
3 3 7 2
3 4 1 5
"""
) == "15\n", "sample 1"

# minimum size
assert run(
"""1 1
1 42
"""
) == "42\n", "minimum case"

# all equal values
assert run(
"""2 4
3 5 5 5
3 5 5 5
"""
) == "20\n", "all equal"

# strong one-sided optimal choice
assert run(
"""1 3
5 100 1 1 1 1
"""
) == "102\n", "keep taking from left"

# split across shelves
assert run(
"""2 2
2 100 1
2 50 50
"""
) == "150\n", "best distribution across shelves"

# overlap boundary check
assert run(
"""1 2
3 1 100 1
"""
) == "101\n", "cannot take middle alone"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 1 42`|`42`| Hạn chế tối thiểu | 
| Hai kệ có tất cả 5s |`20`| Giá trị thống nhất | 
|`100 1 1 1 1`kệ |`102`| Liên tục lấy từ cùng một phía | 
| Kệ hỗn hợp với`100`vs`50 50`|`150`| Phân bổ toàn cầu đúng | 
|`1 100 1`với hai lựa chọn |`101`| Xử lý đúng chồng chéo trái/phải | 

## Vỏ cạnh 

Hãy xem xét đầu vào này:```
1 2
3 1 100 1
```Vật phẩm ở giữa không thể lấy trước. Thuật toán xử lý việc này bằng cách chỉ xem xét việc loại bỏ ranh giới hợp lệ. 

Kệ tiền xử lý tính toán: 

| trái | đúng | giá trị | 
| --- | --- | --- | 
| 0 | 2 | 101 | 
| 1 | 1 | 2 | 
| 2 | 0 | 101 | 

Vì vậy, giá trị tốt nhất để lấy hai mặt hàng là`101`, không`200`. Thuật toán không bao giờ tạo ra các trạng thái không thể xảy ra khi mục trung tâm bị xóa mà không để lộ nó trước. 

Bây giờ hãy xem xét:```
1 3
5 100 1 1 1 1
```Tổng tiền tố là:```
[0, 100, 101, 102, 103, 104]
```Tổng hậu tố là:```
[0, 1, 2, 3, 4, 104]
```Để lấy ba mục, thuật toán sẽ kiểm tra từng phần chia: 

| trái | đúng | giá trị | 
| --- | --- | --- | 
| 0 | 3 | 3 | 
| 1 | 2 | 102 | 
| 2 | 1 | 102 | 
| 3 | 0 | 102 | 

Câu trả lời đúng`102`được tìm thấy. 

Cuối cùng, hãy xem xét việc kết hợp các kệ:```
2 2
2 100 1
2 50 50
```Mảng tốt nhất trên mỗi kệ trở thành: 

Kệ đầu tiên:```
[0, 100, 101]
```Kệ thứ hai:```
[0, 50, 100]
```Trong DP toàn cầu, thuật toán so sánh: 

| lấy từ kệ 1 | lấy từ kệ 2 | tổng cộng | 
| --- | --- | --- | 
| 2 | 0 | 101 | 
| 1 | 1 | 150 | 
| 0 | 2 | 100 | 

Tối đa là`150`, thu được bằng cách chia các lựa chọn trên các kệ. Điều này xác nhận DP khám phá chính xác tất cả các bản phân phối thay vì cam kết một cách tham lam.
