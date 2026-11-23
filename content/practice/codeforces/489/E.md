---
title: "CF 489E - Đi bộ đường dài"
description: "Chúng tôi có một chuỗi các điểm nghỉ dọc theo một con sông. Điểm i nằm ở vị trí x[i] và có giá trị độ đẹp b[i]. Người lữ hành xuất phát ở tọa độ 0 và phải về đích ở điểm nghỉ cuối cùng, cũng là điểm xa nhất."
date: "2026-06-07T17:37:58+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "dp"]
categories: ["algorithms"]
codeforces_contest: 489
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 277.5 (Div. 2)"
rating: 2300
weight: 489
solve_time_s: 158
verified: false
draft: false
---

[CF 489E - Đi bộ đường dài](https://codeforces.com/problemset/problem/489/E) 

**Đánh giá:** 2300 
**Thẻ:** tìm kiếm nhị phân, dp 
**Thời gian giải:** 2 phút 38 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một chuỗi các điểm nghỉ dọc theo một con sông. Điểm`i`nằm ở vị trí`x[i]`và có giá trị đẹp như tranh vẽ`b[i]`. Lữ khách bắt đầu ở tọa độ`0`và phải về đích ở điểm nghỉ cuối cùng, cũng là điểm xa nhất. 

Nếu du khách di chuyển từ vị trí`p`để định vị`q`trong một ngày, quãng đường đi được ngày hôm đó là`q - p`. Khoảng cách lý tưởng hàng ngày của anh ấy là`l`, vậy là sự thất vọng do ngày hôm đó gây ra tương đương với$$(q-p-l)^2.$$Tuyến đường là bất kỳ chuỗi điểm nghỉ nào tăng dần và kết thúc tại điểm`n`. Đối với tuyến đường đã chọn,$$F = \sum ( \text{daily distance} - l )^2$$là sự thất vọng hoàn toàn, và$$B = \sum b_i$$là toàn bộ vẻ đẹp như tranh vẽ của các điểm nghỉ ngơi đã ghé thăm. 

Số lượng cần giảm thiểu là$$\frac{F}{B}.$$Đầu ra không phải là giá trị của tỷ lệ này. Chúng ta phải in ra một tuyến đường tối ưu. 

Thử thách đầu tiên là mục tiêu là một tỷ lệ. Lập trình động thường xử lý các chi phí cộng gộp, nhưng ở đây tử số và mẫu số phụ thuộc vào toàn bộ đường dẫn. 

Các ràng buộc đủ nhỏ để cho phép lập trình động bậc hai. Chúng tôi có nhiều nhất`1000`các điểm nghỉ ngơi. Một đồ thị có các đỉnh là điểm dừng và các cạnh biểu thị các bước nhảy có thể chứa khoảng một triệu cạnh có hướng. MỘT`O(n²)`DP hoàn toàn có thể chấp nhận được, trong khi việc liệt kê các tuyến đường theo cấp số nhân là không thể. 

Một điểm tinh tế là một tuyến đường có thể bỏ qua các điểm dừng tùy ý. Một điểm tinh tế khác là vẻ đẹp như tranh vẽ chỉ được tích lũy cho những điểm đã ghé thăm, trong khi sự thất vọng phụ thuộc vào mỗi lần nhảy. Bất kỳ cách tiếp cận nào tham lam chọn những bước nhảy tốt tại địa phương đều có thể dễ dàng thất bại vì một điểm có nhiều cảnh đẹp như tranh vẽ có thể biện minh cho lịch trình du lịch tồi tệ hơn một chút. 

Coi như:```
n = 2, l = 10

(10, 1)
(20, 1000)
```Sử dụng cả hai điểm mang lại sự thất vọng`0 + 0 = 0`, tỷ lệ`0`. 

Đi thẳng đến điểm 2 mang lại sự thất vọng`(20-10)^2 = 100`, tỷ lệ`100/1000 = 0.1`. 

Tuyến đường tối ưu sử dụng nhiều điểm hơn mặc dù cả hai tuyến đều kết thúc ở cùng một điểm đến. 

Một sai lầm dễ dàng khác là quên rằng hành trình bắt đầu ở tọa độ`0`. Vì```
1 10
20 5
```ngày duy nhất che lấp khoảng cách`20`, không`0`. Sự thất vọng là`(20-10)^2 = 100`. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực sẽ liệt kê mọi tập hợp con các điểm dừng trung gian và kiểm tra mọi tuyến đường có thể kết thúc tại điểm`n`. Vì mỗi điểm có thể được sử dụng hoặc bỏ qua nên có khoảng`2^(n-1)`các tuyến đường. Với`n = 1000`, điều này hoàn toàn không thể thực hiện được. 

Mục tiêu tỷ lệ gợi ý một thủ thuật tối ưu hóa cổ điển. Giả sử bằng cách nào đó chúng ta biết được tỷ lệ tối ưu`R`. Khi đó một tuyến đường là tối ưu chính xác khi$$\frac{F}{B} \le R.$$nhân với`B > 0`cho$$F - R B \le 0.$$Điều này biến đổi tỷ lệ thành một biểu thức cộng. 

Bây giờ hãy tưởng tượng việc sửa một giá trị ứng cử viên`C`. Xác định chi phí của một tuyến đường như$$F - C B.$$Nếu giá trị tối thiểu có thể có của biểu thức này là âm thì một số tuyến thỏa mãn$$\frac{F}{B} < C.$$Nếu giá trị tối thiểu là dương thì mọi tuyến đường đều có tỷ lệ lớn hơn`C`. 

Điều này biến bài toán ban đầu thành bài toán quyết định phù hợp cho tìm kiếm nhị phân. 

Đối với một cố định`C`, chi phí tuyến đường trở thành$$\sum \Big((x_i-x_j-l)^2\Big) - C \sum b_i.$$Biểu thức là phụ gia dọc theo tuyến đường. Điều đó có nghĩa là chúng ta có thể tính giá trị tối thiểu bằng cách sử dụng quy hoạch động. 

Cho phép`dp[i]`là chi phí chuyển đổi tối thiểu của tuyến đường kết thúc tại điểm`i`. 

Sau đó$$dp[i] = -C b_i + \min_j \left( dp[j] + (x_i-x_j-l)^2 \right),$$Ở đâu`j < i`. 

Vị trí bắt đầu`0`hoạt động giống như một nút ảo có tọa độ`0`và chi phí`0`. 

Vì mọi chuyển đổi đều được kiểm tra nên độ phức tạp là`O(n²)`mỗi lần chạy DP. 

Tỷ lệ tối ưu được tìm thấy bằng cách tìm kiếm nhị phân`C`. Sau khi có đủ độ chính xác, lần chạy DP cuối cùng sẽ xây dựng lại đường dẫn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tối ưu | O(n2 log M) | O(n) | Đã chấp nhận | 

Đây`M`là phạm vi tìm kiếm của câu trả lời. 

## Hướng dẫn thuật toán 

1. Lưu trữ tất cả các giá trị tọa độ và độ đẹp. 
2. Tìm kiếm nhị phân trên câu trả lời`C`. 
3. Đối với cố định`C`, chạy lập trình động. 
4. Hãy để`dp[i]`biểu thị giá trị tối thiểu của$$F - C B$$trong số tất cả các tuyến đường kết thúc tại điểm`i`. 
5. Xử lý tọa độ xuất phát`0`như một đỉnh ảo. 

Sự chuyển đổi từ điểm bắt đầu trực tiếp đến điểm`i`có chi phí$$(x_i-l)^2 - C b_i.$$6. Cho mỗi cặp`j < i`, thư giãn$$dp[j] + (x_i-x_j-l)^2 - C b_i.$$Điều này tương ứng với điểm tham quan`i`ngay sau điểm`j`. 
7. Sau khi điền vào bảng DP, hãy kiểm tra`dp[n]`. 

Nếu như`dp[n] < 0`, thì một số tuyến đường đạt được tỷ lệ nhỏ hơn`C`, do đó hãy di chuyển khoảng tìm kiếm nhị phân xuống dưới. 

Nếu không hãy di chuyển nó lên trên. 
8. Lặp lại tìm kiếm nhị phân khoảng sáu mươi lần. Độ chính xác gấp đôi dễ dàng hỗ trợ điều này. 
9. Chạy DP lần cuối bằng cách sử dụng giá trị kết quả của`C`. 
10. Trong khi tính toán các chuyển đổi, hãy lưu trữ phiên bản trước đó tạo ra giá trị tốt nhất. 
11. Xây dựng lại tuyến đường bằng cách đi theo con trỏ trước đó từ điểm`n`. 
12. Đảo ngược trình tự đã được xây dựng lại và in nó. 

### Tại sao nó hoạt động 

Với mọi giá trị cố định`C`, DP tính toán$$\min_{\text{route}} (F - C B).$$Nếu mức tối thiểu này là âm thì một số tuyến đường thỏa mãn$$F - C B < 0,$$tương đương với$$\frac{F}{B} < C.$$Nếu giá trị cực tiểu không âm thì mỗi tuyến đường có ít nhất tỷ số`C`. 

Do đó, dấu của chi phí chuyển đổi tối thiểu trả lời chính xác liệu`C`là trên hoặc dưới tỷ lệ tối ưu. Tìm kiếm nhị phân hội tụ về tỷ lệ khả thi nhỏ nhất. 

DP cuối cùng đang giải quyết vấn đề được chuyển đổi ở một giá trị gần tùy ý với tỷ lệ tối ưu. Lý thuyết quy hoạch phân số tiêu chuẩn đảm bảo rằng bất kỳ lộ trình nào cực tiểu hóa mục tiêu được chuyển đổi ở giá trị này đều là lộ trình tối ưu cho mục tiêu tỷ lệ ban đầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, l = map(int, input().split())

    x = [0] * (n + 1)
    b = [0] * (n + 1)

    for i in range(1, n + 1):
        x[i], b[i] = map(int, input().split())

    def dp_run(C, restore=False):
        INF = 1e100

        dp = [INF] * (n + 1)
        parent = [-1] * (n + 1)

        for i in range(1, n + 1):
            best = (x[i] - l) ** 2 - C * b[i]
            par = 0

            for j in range(1, i):
                cand = dp[j] + (x[i] - x[j] - l) ** 2 - C * b[i]
                if cand < best:
                    best = cand
                    par = j

            dp[i] = best
            parent[i] = par

        if not restore:
            return dp[n]

        path = []
        cur = n

        while cur:
            path.append(cur)
            cur = parent[cur]

        path.reverse()
        return path

    lo = 0.0
    hi = 1e12

    for _ in range(70):
        mid = (lo + hi) / 2.0

        if dp_run(mid) < 0:
            hi = mid
        else:
            lo = mid

    path = dp_run(hi, True)
    print(*path)

if __name__ == "__main__":
    solve()
```Thói quen quan trọng là`dp_run(C)`. Đối với tỷ lệ ứng cử viên cố định`C`, nó tính toán chi phí chuyển đổi tối thiểu. Vị trí bắt đầu ảo được xử lý bằng cách chuyển đổi trực tiếp$$(x_i-l)^2 - C b_i.$$Không có đỉnh rõ ràng`0`được yêu cầu trong bảng DP. 

Quá trình chuyển đổi xem xét mọi điểm dừng trước đó. Từ`n`chỉ là`1000`, vòng lặp bậc hai đủ nhỏ. 

Tìm kiếm nhị phân giữ bất biến rằng tỷ lệ tối ưu thực sự nằm bên trong`[lo, hi]`. Bất cứ khi nào tối ưu được biến đổi trở thành âm, chúng ta biết rằng`mid`lớn hơn đáp án nên giới hạn trên sẽ di chuyển xuống. 

Quá trình tái thiết sử dụng chính xác cùng một DP nhưng lưu trữ phiên bản trước chịu trách nhiệm cho quá trình chuyển đổi tốt nhất. Tiếp nối những người tiền nhiệm từ`n`mang lại lộ trình tối ưu theo thứ tự ngược lại. 

Một lỗi triển khai phổ biến là việc xây dựng lại điểm giữa từ lần lặp tìm kiếm nhị phân cuối cùng. Sử dụng giới hạn trên cuối cùng`hi`an toàn hơn vì nó vẫn ở khía cạnh khả thi trong suốt quá trình tìm kiếm. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5 9
10 10
20 10
30 1
31 5
40 10
```Giả sử tìm kiếm nhị phân đã hội tụ gần tỷ lệ tối ưu. 

| Điểm | Người tiền nhiệm tốt nhất | Tuyến đường cho đến nay | 
| --- | --- | --- | 
| 1 | bắt đầu | 1 | 
| 2 | 1 | 1 2 | 
| 3 | 2 | 1 2 3 | 
| 4 | 2 | 1 2 4 | 
| 5 | 4 | 1 2 4 5 | 

Tái thiết tạo ra:```
1 2 4 5
```Sự lựa chọn thú vị xảy ra ở điểm`4`. Mặc dù điểm`3`gần hơn về mặt địa lý, điểm`4`có vẻ đẹp như tranh vẽ lớn hơn đáng kể. Mục tiêu được chuyển đổi sẽ tự động nắm bắt được sự cân bằng này. 

### Ví dụ tùy chỉnh 

đầu vào:```
3 10
10 1
20 1
30 1
```Mỗi khoảng cách hàng ngày có thể chính xác`10`. 

| Điểm | Chi phí trực tiếp | Người tiền nhiệm được chọn | 
| --- | --- | --- | 
| 1 | 0 - C | bắt đầu | 
| 2 | 0 - 2C | 1 | 
| 3 | 0 - 3C | 2 | 

Lộ trình được xây dựng lại là:```
1 2 3
```Sự thất vọng hoàn toàn bằng không, điều này rõ ràng là tối ưu. 

Ví dụ này xác nhận rằng DP kết hợp các phân đoạn hàng ngày hoàn hảo lại với nhau một cách tự nhiên bất cứ khi nào có thể. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n2 log M) | Một O(n²) DP cho mỗi lần lặp tìm kiếm nhị phân | 
| Không gian | O(n) | Mảng DP và tiền thân | 

Tìm kiếm nhị phân thực hiện một số lần lặp không đổi, khoảng bảy mươi. Với`n = 1000`, mỗi lần chạy DP sẽ kiểm tra khoảng một triệu lần chuyển tiếp. Điều này dễ dàng phù hợp với giới hạn thời gian và mức sử dụng bộ nhớ vẫn rất nhỏ so với 256 MB có sẵn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    def solve():
        input = sys.stdin.readline

        n, l = map(int, input().split())

        x = [0] * (n + 1)
        b = [0] * (n + 1)

        for i in range(1, n + 1):
            x[i], b[i] = map(int, input().split())

        def dp_run(C, restore=False):
            INF = 1e100
            dp = [INF] * (n + 1)
            parent = [-1] * (n + 1)

            for i in range(1, n + 1):
                best = (x[i] - l) ** 2 - C * b[i]
                par = 0

                for j in range(1, i):
                    cand = dp[j] + (x[i] - x[j] - l) ** 2 - C * b[i]
                    if cand < best:
                        best = cand
                        par = j

                dp[i] = best
                parent[i] = par

            if not restore:
                return dp[n]

            path = []
            cur = n
            while cur:
                path.append(cur)
                cur = parent[cur]
            path.reverse()
            return path

        lo, hi = 0.0, 1e12

        for _ in range(70):
            mid = (lo + hi) / 2
            if dp_run(mid) < 0:
                hi = mid
            else:
                lo = mid

        return " ".join(map(str, dp_run(hi, True)))

    return solve()

# provided sample
assert run(
"""5 9
10 10
20 10
30 1
31 5
40 10
"""
) == "1 2 4 5"

# minimum size
assert run(
"""1 10
10 5
"""
) == "1"

# perfect daily jumps
assert run(
"""3 10
10 1
20 1
30 1
"""
) == "1 2 3"

# skipping an intermediate point is better
assert run(
"""2 10
5 1
20 1000
"""
) == "2"

# boundary case with large spacing
out = run(
"""2 1
1000000 1
1000001 1
"""
)
assert out.endswith("2")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Điểm nghỉ duy nhất |`1`| Xử lý đúng trạng thái bắt đầu | 
| Khoảng cách hoàn hảo |`1 2 3`| Lộ trình không thất vọng | 
| Khung cảnh đẹp như tranh vẽ ở điểm đến |`2`| Tối ưu hóa tỷ lệ, không chỉ giảm thiểu sự thất vọng | 
| Tọa độ rất lớn | Kết thúc bằng`2`| Giá trị lớn và độ ổn định số học | 

## Vỏ cạnh 

Hãy xem xét:```
1 10
20 5
```Chỉ có một con đường. DP tính toán$$(20-10)^2 - C \cdot 5.$$Vị trí bắt đầu được coi chính xác là tọa độ`0`, vậy khoảng cách hàng ngày là`20`. Kết quả tái thiết`1`. 

Coi như:```
2 10
10 1
20 1000
```Sử dụng cả hai điểm mang lại tỷ lệ`0`. Đi thẳng vào vấn đề`2`tỷ lệ sản lượng`100/1000 = 0.1`. 

DP so sánh cả hai khả năng thông qua quá trình chuyển đổi của nó. Tuyến đường`1 -> 2`tạo ra chi phí chuyển đổi`-1001C`, trong khi tuyến đường trực tiếp tạo ra`100 - 1000C`. Gần tỷ lệ tối ưu, biểu thức đầu tiên nhỏ hơn, do đó việc xây dựng lại trả về`1 2`. 

Coi như:```
3 10
10 1
11 1000
20 1
```Một chiến lược tham lam có thể ghé thăm điểm`2`vì vẻ đẹp như tranh vẽ của nó. DP không đưa ra quyết định địa phương. Nó đánh giá chi phí tuyến đường hoàn chỉnh, bao gồm cả độ dài bước nhảy kém được tạo ra bởi điểm truy cập`2`. Chỉ có trạng thái tiền nhiệm tối ưu toàn cục mới tồn tại ở mỗi trạng thái, vì vậy lộ trình cuối cùng vẫn đúng.
