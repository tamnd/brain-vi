---
title: "CF 105940K - Chiếc lồng trong ASZoo"
description: "Cái lồng có thể chứa đúng k con vật. Có n họ động vật, và một họ có thể đóng góp bất kỳ số lượng động vật không âm nào vì mỗi họ có vô số thành viên. Gia đình i đóng góp con vật có trọng lượng a[i]."
date: "2026-06-25T13:56:20+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105940
codeforces_index: "K"
codeforces_contest_name: "ASU Coding Cup 10"
rating: 0
weight: 105940
solve_time_s: 37
verified: true
draft: false
---

[CF 105940K - Chiếc lồng trong ASZoo](https://codeforces.com/problemset/problem/105940/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 37s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Lồng có thể giữ chính xác`k`động vật. có`n`các họ động vật và một họ có thể đóng góp bất kỳ số lượng động vật không âm nào vì mỗi họ có vô số thành viên. Gia đình`i`đóng góp động vật có trọng lượng`a[i]`. Nhiệm vụ là tìm ra mọi trọng lượng tổng cộng có thể được hình thành bằng cách chọn chính xác`k`động vật, cho phép sử dụng nhiều lần cùng một họ. Đầu ra là danh sách tất cả các khoản tiền có thể truy cập theo thứ tự tăng dần. 

Các giới hạn là nhỏ đối với số lượng họ và số lượng động vật, nhưng phạm vi câu trả lời có thể có mới là phần quan trọng. Từ`k`và mọi trọng lượng nhiều nhất là`1000`, số tiền lớn nhất có thể có nhiều nhất là`1,000,000`. Điều này có nghĩa là giải pháp tùy thuộc vào phạm vi trọng lượng tối đa là thực tế. Mặt khác, cách tiếp cận lập trình động trực tiếp trên mỗi gia đình, mỗi số lượng động vật và mỗi tổng sẽ mất khoảng`1000 * 1000 * 1,000,000`hoạt động vượt xa khả năng của chúng tôi. 

Những trường hợp phức tạp xuất phát từ việc các gia đình có thể được sử dụng lại. Một giải pháp coi mỗi gia đình chỉ có thể sử dụng được một lần sẽ thất bại. Ví dụ:```
Input
1 3
5

Output
15
```Sự lựa chọn duy nhất có thể là lấy cùng một gia đình ba lần. Một kiểu tập hợp con DP sẽ nói không chính xác rằng chỉ có trọng số`5`là có thể. 

Một sai lầm phổ biến khác là quên chính xác điều đó`k`động vật được yêu cầu. Ví dụ:```
Input
2 2
1 100

Output
2 101 200
```tổng`1`không có giá trị ngay cả khi chỉ một con vật có trọng lượng`1`tồn tại. Chúng ta phải theo dõi số lượng động vật đã chọn, không chỉ số tiền có thể tiếp cận. 

Trường hợp cạnh cuối cùng xuất hiện khi tất cả các họ có cùng trọng số:```
Input
5 5
7 7 7 7 7

Output
35
```Có nhiều sự lựa chọn khác nhau nhưng tất cả đều tạo ra cùng một số tiền. Câu trả lời chỉ được chứa trọng số duy nhất. 

## Phương pháp tiếp cận 

Ý tưởng đơn giản là xây dựng một bảng lập trình động trong đó`dp[c][s]`cho chúng ta biết liệu chúng ta có thể chọn chính xác`c`động vật có tổng trọng lượng`s`. Đối với mọi trạng thái có thể tiếp cận, chúng tôi coi mọi gia đình là động vật tiếp theo. Điều này đúng vì mọi lựa chọn hợp lệ đều có thể được mô tả bằng cách thêm liên tục một con vật. 

Vấn đề là số lượng chuyển tiếp. Có tới`k`số lượng động vật có thể lên tới`k * max(a[i])`số tiền có thể, và lên đến`n`sự lựa chọn cho con vật tiếp theo. Trong trường hợp xấu nhất thì đây là xung quanh`1000 * 1,000,000 * 1000`, tức là khoảng một triệu triệu phép tính. 

Quan sát quan trọng là quá trình chuyển đổi giống nhau đối với mọi số tiền có thể đạt được. Chúng ta chỉ cần chuyển tất cả số tiền hiện có thể đạt được theo trọng lượng của mỗi gia đình. Bitsets được thiết kế chính xác cho việc này. Một chút ở vị trí`x`có nghĩa là số tiền đó`x`có thể truy cập được. Dịch chuyển bitset sang trái`a[i]`đánh dấu tất cả số tiền có thể được tạo ra bằng cách thêm một con vật từ gia đình`i`. 

Thay vì lưu trữ tất cả số tiền trong một bảng lớn, chúng tôi giữ một bit cho mỗi số lượng động vật có thể. Khi xử lý số lượng động vật mới, chúng tôi cập nhật lớp tiếp theo bằng cách sử dụng dịch chuyển bit. Các số nguyên Python đã hoạt động giống như các tập hợp bit có độ dài tùy ý, do đó, một phép dịch chuyển và OR sẽ cập nhật hàng nghìn trạng thái cùng một lúc. 

Lực lượng vũ phu hoạt động vì nó khám phá mọi lựa chọn cuối cùng có thể. Phương pháp nhanh hơn giữ nguyên định nghĩa trạng thái nhưng thay thế hàng triệu chuyển đổi riêng lẻ bằng các thao tác bit ở cấp độ máy. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(k * k * max(a) * n) | O(k * k * max(a)) | Quá chậm | 
| Tối ưu | O(k * n * (k * max(a)) / word_size) | O(k * k * max(a) / word_size) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo`k + 1`bitset. các`i`-bitset thứ đại diện cho tất cả các trọng số có thể đạt được bằng cách sử dụng chính xác`i`động vật. Ban đầu chỉ có thể có 0 con vật có trọng số bằng 0, vì vậy tập bit đầu tiên chỉ chứa bit`0`. 
2. Với mọi số lượng có thể của động vật đã được chọn từ`0`ĐẾN`k - 1`, hãy thử thêm một con vật nữa. Cân nặng của mỗi gia đình`w`, chuyển bitset hiện tại sang trái`w`và hợp nhất nó vào lớp tiếp theo. Sự thay đổi thể hiện việc thêm một con vật vào gia đình đó. 
3. Sau khi xây dựng tất cả các lớp, hãy kiểm tra chính xác bitset`k`động vật. Mỗi bit được đặt tương ứng với một tổng trọng lượng có thể đạt được, vì vậy hãy thu thập các vị trí đó theo thứ tự tăng dần. 

Thứ tự cập nhật quan trọng. Chúng ta chỉ chuyển từ ít động vật hơn sang nhiều động vật hơn, do đó tổng được tạo ra ở cấp độ`c + 1`không bao giờ được sử dụng lại trong cùng một cấp độ và vô tình bị tính là sử dụng thêm động vật. 

Tại sao nó hoạt động: bất biến là sau khi hoàn thiện lớp`c`, mỗi bit được đặt trong lớp đó biểu thị chính xác các trọng số có thể đạt được với`c`động vật và không có trọng lượng không hợp lệ được lưu trữ. Thêm một con vật nữa từ bất kỳ họ nào cũng tương đương với việc dịch chuyển toàn bộ trọng lượng có thể tiếp cận theo trọng lượng của gia đình đó. Việc kết hợp tất cả các ca như vậy sẽ tạo ra chính xác các trạng thái hợp lệ cho`c + 1`động vật. Bằng quy nạp, lớp cuối cùng chứa chính xác tất cả các trọng lượng lồng có thể có. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    data = list(map(int, sys.stdin.buffer.read().split()))
    if not data:
        return

    n, k = data[0], data[1]
    a = data[2:2 + n]

    dp = [0] * (k + 1)
    dp[0] = 1

    for cnt in range(k):
        cur = dp[cnt]
        if cur == 0:
            continue
        nxt = 0
        for w in a:
            nxt |= cur << w
        dp[cnt + 1] = nxt

    ans = []
    bits = dp[k]
    pos = 0
    while bits:
        low = bits & -bits
        pos = low.bit_length() - 1
        ans.append(str(pos))
        bits ^= low

    print(" ".join(ans))

if __name__ == "__main__":
    solve()
```Mảng`dp`lưu trữ trạng thái bitset cho từng số lượng động vật chính xác. Việc khởi tạo`dp[0] = 1`có nghĩa là chỉ có thể đạt được tổng bằng 0 trước khi chọn bất kỳ con vật nào. 

Đối với mỗi lớp, biến`cur`chứa tất cả các khoản tiền hiện có thể truy cập. Mỗi`cur << w`thêm một con vật có trọng lượng`w`đến mọi khả năng trước đó. Phép toán OR kết hợp tất cả các họ có thể. 

Vòng lặp trích xuất liên tục tìm bit được đặt thấp nhất. Bởi vì các bit số nguyên được sắp xếp từ thấp đến cao nên điều này tự nhiên tạo ra các câu trả lời theo thứ tự tăng dần. Việc loại bỏ bit thấp nhất sẽ tránh việc quét qua mọi giá trị trọng số có thể có. 

Việc triển khai không cần xử lý đặc biệt đối với tình trạng tràn số nguyên vì số nguyên Python tự động tăng lên. Chi tiết ranh giới chính chỉ lặp lại cho đến khi`k - 1`, bởi vì quá trình chuyển đổi cuối cùng tạo ra trạng thái chính xác`k`động vật. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 2
1 2 3
```Dấu vết là: 

| Động vật được chọn | Số tiền có thể tiếp cận hiện tại | Hành động | 
| --- | --- | --- | 
| 0 | {0} | Thêm trọng lượng 1, 2, 3 | 
| 1 | {1, 2, 3} | Thêm trọng lượng 1, 2, 3 | 
| 2 | {2, 3, 4, 5, 6} | Đầu ra | 

Quá trình chuyển đổi đầu tiên tạo ra trọng lượng của từng con vật. Quá trình chuyển đổi thứ hai thêm một con vật khác và tạo ra tất cả các cặp hợp lệ. 

### Mẫu 2 

đầu vào:```
5 5
1 1 1 1 1
```| Động vật được chọn | Số tiền có thể tiếp cận hiện tại | Hành động | 
| --- | --- | --- | 
| 0 | {0} | Thêm trọng lượng 1 | 
| 1 | {1} | Thêm trọng lượng 1 | 
| 2 | {2} | Thêm trọng lượng 1 | 
| 3 | {3} | Thêm trọng lượng 1 | 
| 4 | {4} | Thêm trọng lượng 1 | 
| 5 | {5} | Đầu ra | 

Mặc dù có nhiều cách để chọn năm con vật nhưng tập hợp có thể truy cập chỉ chứa một giá trị. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k * n * S / word_size) | Mỗi lớp dịch chuyển một bit có kích thước tối đa`S`, Ở đâu`S`là số tiền lớn nhất có thể. | 
| Không gian | O(k * S / word_size) | Chúng tôi lưu trữ một bitset cho mỗi số lượng động vật có thể. | 

Đây`S`nhiều nhất là`1,000,000`. Biểu diễn bitset nén nhiều trạng thái boolean vào mỗi số nguyên máy, làm cho cách tiếp cận phù hợp thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    data = list(map(int, sys.stdin.buffer.read().split()))
    if not data:
        return ""
    n, k = data[0], data[1]
    a = data[2:2+n]

    dp = [0] * (k + 1)
    dp[0] = 1
    for c in range(k):
        for w in a:
            dp[c + 1] |= dp[c] << w

    ans = []
    x = dp[k]
    while x:
        b = x & -x
        ans.append(str(b.bit_length() - 1))
        x -= b

    sys.stdin = old
    return " ".join(ans)

assert run("""3 2
1 2 3
""") == "2 3 4 5 6", "sample 1"

assert run("""5 5
1 1 1 1 1
""") == "5", "sample 2"

assert run("""3 3
3 5 11
""") == "9 11 13 15 17 19 21 25 27 33", "sample 3"

assert run("""1 3
5
""") == "15", "repeated family"

assert run("""2 2
1 100
""") == "2 101 200", "exact count"

assert run("""4 1
7 7 7 7
""") == "7", "duplicate weights"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 3 / 5`|`15`| Một gia đình có thể được chọn nhiều lần. | 
|`2 2 / 1 100`|`2 101 200`| Chính xác`k`động vật phải được lựa chọn. | 
|`4 1 / 7 7 7 7`|`7`| Trọng số họ trùng lặp tạo ra số tiền duy nhất. | 
|`5 5 / 1 1 1 1 1`|`5`| Nhiều lựa chọn có thể thu gọn lại thành một câu trả lời. | 

## Vỏ cạnh 

Đối với trường hợp gia đình nhiều người:```
Input
1 3
5
```Thuật toán bắt đầu với số tiền có thể truy cập`{0}`. Sau một lần chuyển đổi nó có`{5}`, sau hai nó có`{10}`, và sau ba nó có`{15}`. Lớp cuối cùng chỉ chứa`15`, cho phép sử dụng chính xác không giới hạn một gia đình. 

Đối với trường hợp đếm chính xác:```
Input
2 2
1 100
```Sau một con vật, trọng lượng có thể đạt được là`{1, 100}`. Quá trình chuyển đổi thứ hai kết hợp lại các trạng thái này và tạo ra`{2, 101, 200}`. giá trị`1`không bao giờ xuất hiện ở lớp cuối cùng vì nó chỉ sử dụng một con vật. 

Đối với trường hợp trọng lượng trùng lặp:```
Input
5 5
7 7 7 7 7
```Mọi chuyển đổi đều dịch chuyển một lượng như nhau. Bộ có thể truy cập tăng lên khi`{0}`,`{7}`,`{14}`,`{21}`,`{28}`,`{35}`. Bitset tự động loại bỏ các khả năng trùng lặp vì một bit chỉ có thể được đặt một lần.
