---
title: "CF 18D - Người bán Bob"
description: "Bob trải qua một sự kiện mỗi ngày. Một ngày có thể là ngày thưởng hoặc ngày của khách hàng."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dp", "greedy"]
categories: ["algorithms"]
codeforces_contest: 18
codeforces_index: "D"
codeforces_contest_name: "Codeforces Beta Round 18 (Div. 2 Only)"
rating: 2000
weight: 18
solve_time_s: 114
verified: true
draft: false
---
[CF 18D - Người bán Bob](https://codeforces.com/problemset/problem/18/D) 

**Xếp hạng:** 2000 
**Tags:** bạo lực, dp, tham lam 
**Thời gian giải:** 1 phút 54s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Bob trải qua một sự kiện mỗi ngày. Một ngày có thể là ngày thưởng hoặc ngày của khách hàng. 

Vào ngày trao giải, Bob nhận được một thẻ nhớ cỡ lớn$2^x$. Anh ta có thể giữ nó hoặc ngay lập tức cho đi. Anh ta không bao giờ được phép cất nhiều hơn một cây gậy cùng một lúc. 

Vào một ngày của khách hàng, có người muốn mua một cây gậy cỡ lớn$2^x$. Nếu Bob hiện đang sở hữu chính xác cây gậy đó, anh ấy sẽ bán nó và kiếm được$2^x$bellars. Nếu không khách hàng sẽ bỏ đi và cơ hội sẽ biến mất mãi mãi. 

Đầu vào chỉ đơn giản là chuỗi các sự kiện này. Đối với mỗi sự kiện trao giải, chúng ta phải quyết định xem Bob giữ cây gậy hay vứt bỏ nó để tổng số tiền kiếm được là tối đa. 

Ràng buộc$n \le 5000$đủ nhỏ để lập trình động bậc hai, nhưng quá lớn để tìm kiếm theo cấp số nhân. Một thế lực vũ phu thử mọi lựa chọn sẽ cần phải cân nhắc tới$2^{5000}$trạng thái, điều đó là không thể. Giải pháp khối có nhiều rủi ro vì$5000^3 = 1.25 \times 10^{11}$hoạt động vượt xa giới hạn. Giải pháp bậc hai là thoải mái, xung quanh$2.5 \times 10^7$hoạt động trong trường hợp xấu nhất. 

Điều khó khăn là Bob chỉ có thể cầm được một cây gậy. Một quyết định hấp dẫn ở địa phương có thể phá hủy một cuộc mua bán có lợi hơn trong tương lai. 

Hãy xem xét ví dụ này:```
4
win 1
win 10
sell 1
sell 10
```Một chiến lược tham lam luôn giữ cây gậy mới nhất sẽ kết thúc bằng lợi nhuận$2^{10} = 1024$, vì thanh cỡ 1 sẽ bị vứt đi trước khi khách hàng đến. 

Cách chơi tối ưu là khác nhau: 

giữ size 1, bán đi, sau đó giữ size 10 và bán tổng cộng$2 + 1024 = 1026$. 

Một trường hợp tinh tế khác là khi giải thưởng đến sau khi khách hàng phù hợp đã vượt qua.```
3
sell 5
win 5
sell 5
```Chỉ có khách hàng thứ hai mới có thể hài lòng. Một bước tiền xử lý bất cẩn phù hợp với mọi chiến thắng với mỗi lần bán có cùng quy mô sẽ bị tính quá mức. 

Ngoài ra còn có sự tương tác quan trọng giữa các khoảng chồng chéo.```
6
win 1
win 2
sell 1
sell 2
win 1
sell 1
```Không thể giữ đồng thời thanh kích thước 1 đầu tiên và thanh kích thước 2 vì khoảng thời gian hoạt động của chúng trùng nhau. Thuật toán phải mô hình hóa rõ ràng xung đột này. 

## Phương pháp tiếp cận 

Sức mạnh tàn bạo trực tiếp nhất là xử lý từng ngày một và thử mọi quyết định có thể cho mọi vấn đề.`win`sự kiện. Bất cứ khi nào Bob nhận được một cây gậy, chúng tôi chia thành hai khả năng: giữ nó hoặc vứt bỏ nó. Nếu anh ta đã sở hữu một cây gậy khác, chúng ta cũng có thể quyết định thay thế cây gậy hiện tại. 

Tìm kiếm này là chính xác vì nó khám phá mọi chuỗi hành động pháp lý. Vấn đề là số lượng trạng thái. Trong trường hợp xấu nhất có 5000 sự kiện giải thưởng, tạo ra khoảng$2^{5000}$khả năng. Ngay cả việc cắt tỉa tích cực cũng không thể giải cứu được phương pháp này. 

Quan sát quan trọng là việc bán hàng có lợi nhuận hoạt động giống như một khoảng thời gian. 

Giả sử một cây gậy có kích thước$x$được chiến thắng vào ngày$l$, và khách hàng tương ứng xuất hiện vào ngày$r$. Nếu Bob muốn kiếm tiền từ cây gậy này thì từ ngày$l$cho đến ngày$r$anh ta phải dành chỗ lưu trữ duy nhất của mình cho cây gậy này. Trong suốt khoảng thời gian đó anh ta không thể giữ bất kỳ cây gậy nào khác. 

Điều đó biến vấn đề thành việc lập kế hoạch theo khoảng thời gian. 

Mỗi cơ hội sinh lợi sẽ trở thành: 

tạm nghỉ$[l, r]$có giá trị$2^x$. 

Hai khoảng tương thích nếu chúng không trùng nhau. Mục tiêu là chọn một tập hợp giá trị tối đa gồm các khoảng không chồng chéo. 

Bây giờ cấu trúc trở nên đơn giản hơn nhiều. Đối với mọi`sell x`, có nhiều nhất một khách hàng. Chúng tôi có thể quét ngược và ghép nối khách hàng này với mọi khách hàng trước đó`win x`. Mỗi cặp như vậy tạo thành một ứng cử viên khoảng. 

Khi chúng ta có tất cả các khoảng thời gian, bài toán sẽ trở thành lập kế hoạch khoảng thời gian có trọng số, một bài toán quy hoạch động cổ điển. 

Chúng tôi sắp xếp các khoảng thời gian theo ngày kết thúc. Đối với mỗi khoảng thời gian, chúng tôi tìm thấy khoảng thời gian cuối cùng kết thúc trước khi khoảng thời gian này bắt đầu. Sau đó:$$dp[i] = \max(dp[i-1], dp[p(i)] + value_i)$$Điều này làm giảm vấn đề từ tìm kiếm theo cấp số nhân đến tiền xử lý bậc hai cộng với DP tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(2^n)$|$O(n)$| Quá chậm | 
| Tối ưu |$O(n^2)$|$O(n^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các sự kiện thành mảng. 

Đối với mỗi ngày, hãy lưu trữ xem đó có phải là`win`hoặc`sell`, và số mũ liên quan$x$. 
2. Đối với mọi`sell x`ngày, quét ngược và tìm tất cả sớm hơn`win x`ngày. 

Mỗi cặp như vậy tạo thành một cơ hội hợp lệ: 

nếu Bob giữ cây gậy từ ngày thắng cuộc cho đến ngày bán, anh ta sẽ kiếm được$2^x$. 

Khoảng thời gian là:$$[win\_day, sell\_day]$$với lợi nhuận:$$2^x$$3. Lưu trữ tất cả các khoảng trong danh sách. 

Mỗi khoảng chứa: 

ngày bắt đầu, 

ngày kết thúc, 

lợi nhuận. 
4. Sắp xếp các khoảng thời gian theo ngày kết thúc. 

Lập kế hoạch khoảng thời gian có trọng số phụ thuộc vào khoảng thời gian xử lý theo thứ tự hoàn thiện tăng dần. 
5. Đối với mỗi khoảng thời gian$i$, tính toán$p(i)$.$p(i)$là chỉ số lớn nhất của một khoảng có ngày kết thúc hoàn toàn trước khoảng$i$ngày bắt đầu. 

Đây chính xác là những khoảng có thể cùng tồn tại với khoảng$i$. 
6. Chạy lập trình động. 

Cho phép`dp[i]`là lợi nhuận tối đa khi sử dụng lần đầu tiên`i`các khoảng theo thứ tự sắp xếp. 

Chuyển tiếp: 

hoặc bỏ qua khoảng thời gian$i$, 

hoặc mang nó cùng với giải pháp tương thích tốt nhất. 

Chính thức:$$dp[i] = \max(dp[i-1], dp[p(i)] + value_i)$$7. Xuất giá trị DP cuối cùng. 

Đây là lợi nhuận tối đa có thể đạt được. 

### Tại sao nó hoạt động 

Mỗi hành động sinh lợi tương ứng với việc dành một ô lưu trữ duy nhất của Bob trong một khoảng thời gian liên tục. Hai hành động có lợi nhuận có thể thực hiện đồng thời chính xác khi khoảng thời gian của chúng không trùng nhau. 

Sự chuyển đổi là chính xác: 

mọi chiến lược hợp lệ đều tạo ra một tập hợp các khoảng không chồng chéo, 

và mọi tập hợp khoảng không chồng chéo đều mô tả một chiến lược hợp lệ. 

Lập trình động là cấu trúc con tối ưu tiêu chuẩn để lập kế hoạch khoảng thời gian có trọng số. Khi xem xét khoảng$i$, mọi giải pháp tối ưu đều loại trừ hoặc bao gồm nó. Nếu nó bao gồm khoảng$i$, tất cả các khoảng được chọn còn lại phải đến từ tiền tố tương thích kết thúc tại$p(i)$. Vì đây là hai khả năng duy nhất nên phép truy toán luôn tính toán mức tối ưu thực sự. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())

    typ = []
    val = []

    for _ in range(n):
        t, x = input().split()
        typ.append(t)
        val.append(int(x))

    intervals = []

    for r in range(n):
        if typ[r] == "sell":
            x = val[r]

            for l in range(r):
                if typ[l] == "win" and val[l] == x:
                    intervals.append((l, r, 1 << x))

    intervals.sort(key=lambda item: item[1])

    m = len(intervals)

    ends = [0] * m
    starts = [0] * m
    profits = [0] * m

    for i in range(m):
        starts[i] = intervals[i][0]
        ends[i] = intervals[i][1]
        profits[i] = intervals[i][2]

    p = [-1] * m

    for i in range(m):
        best = -1

        for j in range(i - 1, -1, -1):
            if ends[j] < starts[i]:
                best = j
                break

        p[i] = best

    dp = [0] * (m + 1)

    for i in range(1, m + 1):
        skip = dp[i - 1]

        take = profits[i - 1]
        if p[i - 1] != -1:
            take += dp[p[i - 1] + 1]

        dp[i] = max(skip, take)

    print(dp[m])

solve()
```Giai đoạn đầu tiên xây dựng tất cả các khoảng thời gian có lợi nhuận có thể có. Nếu một`win x`xảy ra trước một`sell x`, Bob có thể giữ cây gậy đó trong suốt khoảng thời gian đó và hoàn tất việc mua bán. 

Danh sách khoảng thời gian là bước lập mô hình cốt lõi. Sau khi quá trình chuyển đổi này hoàn tất, câu chuyện ban đầu về thẻ nhớ sẽ biến mất và chỉ còn vấn đề tương thích về khoảng thời gian. 

Thứ tự sắp xếp là quan trọng. DP giả định rằng tất cả các khoảng thời gian tương thích trước đó đều xuất hiện trước khoảng thời gian hiện tại. Sắp xếp theo thời gian kết thúc đảm bảo tính chất này. 

Việc tính toán của`p[i]`tìm kiếm ngược khoảng thời gian tương thích mới nhất. Vì số khoảng là nhiều nhất$O(n^2)$, quá trình tiền xử lý bậc hai là đủ nhanh. 

Mảng DP sử dụng chỉ mục dựa trên 1 để thuận tiện. Khoảng thời gian`i - 1`tương ứng với trạng thái DP`i`. Điều này tránh các chỉ số tiêu cực khi truy cập tiền tố trống. 

So sánh chặt chẽ:```
ends[j] < starts[i]
```là điều cần thiết. Nếu một khoảng thời gian kết thúc vào cùng một ngày một khoảng thời gian khác bắt đầu, chúng vẫn trùng nhau vì Bob không thể vừa bán một cây gậy vừa sở hữu một cây gậy khác trong chuỗi ngày đó. 

Lợi nhuận sử dụng:```
1 << x
```bởi vì giá trị của một cây gậy được bán là$2^x$. 

Số nguyên Python xử lý an toàn các giá trị lớn nhất kể từ$2^{2000}$dễ dàng phù hợp với các số nguyên có độ chính xác tùy ý. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
7
win 10
win 5
win 3
sell 5
sell 3
win 10
sell 10
```Các khoảng được xây dựng: 

| Khoảng thời gian | Bắt đầu | Kết thúc | Lợi nhuận | 
| --- | --- | --- | --- | 
| cỡ 5 | 2 | 4 | 32 | 
| cỡ 3 | 3 | 5 | 8 | 
| cỡ 10 | 1 | 7 | 1024 | 
| cỡ 10 | 6 | 7 | 1024 | 

Sau khi sắp xếp theo ngày kết thúc: 

| Chỉ mục | Khoảng thời gian | Lợi nhuận | p(i) | 
| --- | --- | --- | --- | 
| 0 | [2,4] | 32 | -1 | 
| 1 | [3,5] | 8 | -1 | 
| 2 | [1,7] | 1024 | -1 | 
| 3 | [6,7] | 1024 | 1 | 

Dấu vết DP: 

| tôi | Khoảng thời gian hiện tại | Bỏ qua | Đi | dp[i] | 
| --- | --- | --- | --- | --- | 
| 1 | [2,4] | 0 | 32 | 32 | 
| 2 | [3,5] | 32 | 8 | 32 | 
| 3 | [1,7] | 32 | 1024 | 1024 | 
| 4 | [6,7] | 1024 | 1056 | 1056 | 

Câu trả lời cuối cùng:```
1056
```Ví dụ này cho thấy tại sao khoảng kích thước 10 thứ hai lại có giá trị. Trước tiên, Bob có thể hoàn tất việc bán hàng cỡ 5, sau đó nhận được một chiếc gậy cỡ 10 khác và cũng bán nó. 

### Ví dụ 2 

đầu vào:```
6
win 1
win 2
sell 1
sell 2
win 1
sell 1
```Các khoảng được xây dựng: 

| Khoảng thời gian | Bắt đầu | Kết thúc | Lợi nhuận | 
| --- | --- | --- | --- | 
| cỡ 1 | 1 | 3 | 2 | 
| cỡ 2 | 2 | 4 | 4 | 
| cỡ 1 | 1 | 6 | 2 | 
| cỡ 1 | 5 | 6 | 2 | 

Các khoảng được sắp xếp: 

| Chỉ mục | Khoảng thời gian | Lợi nhuận | p(i) | 
| --- | --- | --- | --- | 
| 0 | [1,3] | 2 | -1 | 
| 1 | [2,4] | 4 | -1 | 
| 2 | [1,6] | 2 | -1 | 
| 3 | [5,6] | 2 | 1 | 

Dấu vết DP: 

| tôi | Khoảng thời gian hiện tại | Bỏ qua | Đi | dp[i] | 
| --- | --- | --- | --- | --- | 
| 1 | [1,3] | 0 | 2 | 2 | 
| 2 | [2,4] | 2 | 4 | 4 | 
| 3 | [1,6] | 4 | 2 | 4 | 
| 4 | [5,6] | 4 | 6 | 6 | 

Câu trả lời cuối cùng:```
6
```Dấu vết chứng minh rằng các khoảng chồng chéo không thể được chọn cả hai. Chiến lược tối ưu là bán size 2 trước, sau đó là size 1 sau. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^2)$| Xây dựng các khoảng thời gian và khả năng tương thích tính toán đều yêu cầu quét bậc hai | 
| Không gian |$O(n^2)$| Số khoảng có thể đạt kích thước bậc hai | 

Với$n \le 5000$, độ phức tạp bậc hai hoàn toàn có thể chấp nhận được. Khoảng 25 triệu thao tác đơn giản phù hợp thoải mái trong giới hạn thời gian trong Python và khoảng thời gian lưu trữ cũng vẫn nằm trong giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def solve():
        n = int(input())

        typ = []
        val = []

        for _ in range(n):
            t, x = input().split()
            typ.append(t)
            val.append(int(x))

        intervals = []

        for r in range(n):
            if typ[r] == "sell":
                x = val[r]

                for l in range(r):
                    if typ[l] == "win" and val[l] == x:
                        intervals.append((l, r, 1 << x))

        intervals.sort(key=lambda item: item[1])

        m = len(intervals)

        starts = [0] * m
        ends = [0] * m
        profits = [0] * m

        for i in range(m):
            starts[i], ends[i], profits[i] = intervals[i]

        p = [-1] * m

        for i in range(m):
            for j in range(i - 1, -1, -1):
                if ends[j] < starts[i]:
                    p[i] = j
                    break

        dp = [0] * (m + 1)

        for i in range(1, m + 1):
            dp[i] = dp[i - 1]

            take = profits[i - 1]
            if p[i - 1] != -1:
                take += dp[p[i - 1] + 1]

            dp[i] = max(dp[i], take)

        return str(dp[m])

    return solve()

# provided sample
assert run(
"""7
win 10
win 5
win 3
sell 5
sell 3
win 10
sell 10
"""
) == "1056", "sample 1"

# minimum size
assert run(
"""1
win 5
"""
) == "0", "single event"

# simple successful sale
assert run(
"""2
win 3
sell 3
"""
) == "8", "basic match"

# overlapping intervals
assert run(
"""4
win 1
win 2
sell 1
sell 2
"""
) == "4", "must choose larger overlapping interval"

# disjoint intervals
assert run(
"""4
win 1
sell 1
win 2
sell 2
"""
) == "6", "both intervals can be taken"

# sell before win
assert run(
"""3
sell 5
win 5
sell 5
"""
) == "32", "only later customer can be satisfied"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đơn`win`sự kiện | 0 | Không có khách hàng nghĩa là không có lợi nhuận | 
| Một cặp phù hợp | 8 | Tạo khoảng cơ bản | 
| Khoảng chồng chéo | 4 | DP chọn khoảng xung đột tốt nhất | 
| Khoảng rời rạc | 6 | Khoảng tương thích kết hợp chính xác | 
| Khách hàng trước giải thưởng | 32 | Kết hợp ngược không hợp lệ bị từ chối | 

## Vỏ cạnh 

Hãy xem xét trường hợp khách hàng xuất hiện trước khi giải thưởng tồn tại.```
3
sell 5
win 5
sell 5
```Thuật toán chỉ tạo các khoảng thời gian từ trước đó`win`vài ngày sau`sell`ngày. Khách hàng đầu tiên không tạo ra khoảng thời gian vì không có khách hàng nào trước đó`win 5`. 

Khoảng thời gian hợp lệ duy nhất là: 

| Bắt đầu | Kết thúc | Lợi nhuận | 
| --- | --- | --- | 
| 2 | 3 | 32 | 

DP trả về 32, điều này đúng. 

Bây giờ hãy xem xét các cơ hội sinh lợi chồng chéo.```
4
win 1
win 10
sell 1
sell 10
```Khoảng thời gian được tạo: 

| Bắt đầu | Kết thúc | Lợi nhuận | 
| --- | --- | --- | 
| 1 | 3 | 2 | 
| 2 | 4 | 1024 | 

Các khoảng này chồng lên nhau nên DP phải chọn một khoảng. Nó chọn khoảng thứ hai và trả về 1024. 

Một chiến lược tham lam ngây thơ luôn giữ cây gậy đầu tiên có sẵn sẽ trả về sai 2. 

Cuối cùng, hãy xem xét các khoảng chạm vào ranh giới.```
4
win 1
sell 1
win 2
sell 2
```Khoảng thời gian: 

| Bắt đầu | Kết thúc | Lợi nhuận | 
| --- | --- | --- | 
| 1 | 2 | 2 | 
| 3 | 4 | 4 | 

Vì khoảng thời gian đầu tiên kết thúc trước khoảng thời gian thứ hai bắt đầu nên chúng tương thích và cả hai đều được chọn để có tổng lợi nhuận 6. 

Điều kiện nghiêm ngặt:```
ends[j] < starts[i]
```xử lý việc này một cách chính xác.
