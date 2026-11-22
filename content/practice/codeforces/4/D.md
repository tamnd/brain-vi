---
title: "CF 4D - Món Quà Bí Ẩn"
description: "Chúng ta được phát một bộ phong bì, mỗi phong bì có chiều rộng và chiều cao. Bưu thiếp đã có kích thước cố định và chúng tôi muốn xây dựng chuỗi phong bì lồng nhau dài nhất có thể sao cho:"
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dp", "sortings"]
categories: ["algorithms"]
codeforces_contest: 4
codeforces_index: "D"
codeforces_contest_name: "Codeforces Beta Round 4 (Div. 2 Only)"
rating: 1700
weight: 4
solve_time_s: 77
verified: true
draft: false
---
[CF 4D - Món quà bí ẩn](https://codeforces.com/problemset/problem/4/D) 

**Đánh giá:** 1700 
**Thẻ:** dp, sắp xếp 
**Thời gian giải:** 1 phút 17s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được phát một bộ phong bì, mỗi phong bì có chiều rộng và chiều cao. Bưu thiếp đã có kích thước cố định và chúng tôi muốn xây dựng chuỗi phong bì lồng nhau dài nhất có thể sao cho: 

- tấm bưu thiếp vừa với phong bì đầu tiên, 
- mỗi phong bì tiếp theo đều lớn hơn cả về chiều rộng và chiều cao so với phong bì trước. 

Xoay bị cấm, vì vậy chiều rộng phải so sánh trực tiếp với chiều rộng và chiều cao với chiều cao. 

Đầu ra không chỉ là độ dài chuỗi tối đa mà còn là một chuỗi hợp lệ của các chỉ số đường bao theo thứ tự lồng tăng dần. 

Hạn chế quan trọng nhất là`n ≤ 5000`. Việc tìm kiếm mạnh mẽ trên tất cả các tập hợp con hoặc tất cả các hoán vị là không thể bởi vì ngay cả`2^5000`hoặc`5000!`có kích thước lớn về mặt thiên văn. Mặt khác, quy hoạch động bậc hai với khoảng`5000^2 = 25,000,000`so sánh là hoàn toàn thực tế trong C++ và vẫn được chấp nhận trong Python nếu triển khai cẩn thận. 

Vấn đề về cơ bản là yêu cầu chuỗi dài nhất trong đó cả hai chiều đều tăng nghiêm ngặt. Điều đó ngay lập tức gợi ý một giải pháp lập trình động kiểu dãy con tăng dài nhất sau khi sắp xếp. 

Một số trường hợp đặc biệt có thể âm thầm phá vỡ việc triển khai không chính xác. 

Giả sử nhiều phong bì có cùng kích thước. 

đầu vào:```
3 1 1
2 2
2 2
3 3
```Đầu ra đúng:```
2
1 3
```hai`(2,2)`phong bì không thể cùng xuất hiện trong chuỗi vì mức tăng phải nghiêm ngặt ở cả hai chiều. Triển khai LIS bất cẩn bằng cách sử dụng`>=`thay vì`>`sẽ cho phép họ một cách không chính xác. 

Một trường hợp phức tạp khác xảy ra khi một phong bì chỉ vừa với một chiều. 

đầu vào:```
3 2 2
3 2
2 3
4 4
```Đầu ra đúng:```
1
3
```Không`(3,2)`cũng không`(2,3)`có thể giữ bưu thiếp vì cả hai kích thước đều phải lớn hơn. Chỉ một`(4,4)`hoạt động. Ở đây chỉ lọc theo khu vực hoặc theo một thứ nguyên sẽ không thành công. 

Trường hợp cạnh thứ ba xuất hiện khi không có phong bì nào có thể chứa được bưu thiếp. 

đầu vào:```
2 5 5
5 6
6 5
```Đầu ra đúng:```
0
```Vấn đề bất bình đẳng nghiêm ngặt một lần nữa. Chiều rộng bằng nhau hoặc chiều cao bằng nhau là không đủ. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Chúng ta có thể thử từng tập hợp con của phong bì, sau đó kiểm tra xem liệu các phong bì trong tập hợp con đó có thể được sắp xếp thành một chuỗi hợp lệ hay không. Điều này hiệu quả vì định nghĩa của chuỗi hợp lệ rất dễ xác minh: mọi cặp liên tiếp phải tăng nghiêm ngặt ở cả hai chiều. 

Vấn đề là số lượng tập hợp con. Với`n = 5000`, thậm chí việc lặp qua tất cả các tập hợp con là không thể. Tìm kiếm đệ quy thử mọi cách tiếp tục có thể cũng bùng nổ theo cấp số nhân vì mỗi đường bao có thể phân nhánh thành nhiều đường bao lớn hơn. 

Cấu trúc của vấn đề đưa ra một con đường rõ ràng hơn nhiều. Nếu phong bì`A`có thể đi trước phong bì`B`, sau đó`B`phải lớn hơn ở cả hai chiều. Sau khi sắp xếp các đường bao, chúng ta có thể coi đây là một biểu đồ tuần hoàn có hướng trong đó các cạnh luôn di chuyển về phía các đường bao lớn hơn. Khi biểu đồ trở nên không theo chu kỳ, việc lập trình động đường đi dài nhất sẽ trở nên tự nhiên. 

Sắp xếp là quan sát quan trọng. Sau khi sắp xếp theo chiều rộng và chiều cao, bất kỳ chuỗi hợp lệ nào cũng phải di chuyển về phía trước theo thứ tự đã sắp xếp. Khi đó ta chỉ cần tính:`dp[i] = longest valid chain ending at envelope i`Đối với mọi phong bì trước đó`j`, nếu cả hai chiều của`j`nhỏ hơn so với`i`, sau đó`i`có thể kéo dài chuỗi kết thúc tại`j`. 

Điều này biến vấn đề thành một vấn đề kinh điển`O(n^2)`giải pháp lập trình động. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| DP tối ưu với sắp xếp | O(n^2) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các phong bì và giữ lại chỉ mục gốc của chúng. 

Chúng ta phải in các chỉ mục từ thứ tự đầu vào ban đầu, vì vậy mỗi phong bì sẽ lưu trữ`(width, height, original_index)`. 
2. Loại bỏ những phong bì không thể chứa bưu thiếp. 

Một phong bì chỉ có thể sử dụng được nếu:```
width > postcard_width
height > postcard_height
```Bất kỳ phong bì nào không đạt được điều kiện này sẽ không bao giờ xuất hiện trong một chuỗi hợp lệ. 
3. Sắp xếp các phong bì còn lại theo chiều rộng, sau đó theo chiều cao. 

Điều này đảm bảo rằng bất cứ khi nào chúng ta di chuyển từ trái sang phải, chiều rộng không bao giờ giảm. Nó làm giảm không gian tìm kiếm vì các chuyển đổi hợp lệ chỉ cần xem xét các đường bao trước đó. 
4. Khởi tạo mảng lập trình động. 

Cho phép:```
dp[i] = length of longest chain ending at i
parent[i] = previous envelope index in that chain
```Ban đầu mỗi phong bì tạo thành một chuỗi dài`1`. 
5. Đối với mỗi phong bì`i`, kiểm tra tất cả các phong bì trước đó`j`. 

Nếu như:```
width[j] < width[i]
height[j] < height[i]
```sau đó phong bì`i`có thể kéo dài chuỗi kết thúc tại`j`. 

Cập nhật:```
dp[i] = dp[j] + 1
parent[i] = j
```bất cứ khi nào điều này mang lại một chuỗi dài hơn. 
6. Tìm phong bì có số lớn nhất`dp[i]`. 

Đây là sự kết thúc của chuỗi tối ưu. 
7. Xây dựng lại chuỗi bằng cách sử dụng`parent`mảng. 

Bắt đầu từ điểm cuối tốt nhất và liên tục theo dõi cha mẹ ngược lại cho đến khi`-1`. 
8. Đảo ngược trình tự đã được xây dựng lại và in nó. 

Quá trình tái thiết diễn ra ngược lại, từ đường bao lớn nhất đến đường bao nhỏ nhất. 

### Tại sao nó hoạt động 

Sau khi sắp xếp, mọi chuỗi hợp lệ sẽ xuất hiện theo thứ tự chỉ mục tăng dần vì chiều rộng và chiều cao đều phải tăng nghiêm ngặt. Quá trình chuyển đổi DP xem xét mọi phong bì trước đó có thể có trước phong bì hiện tại về mặt pháp lý. Từ`dp[j]`đã lưu trữ chuỗi tốt nhất kết thúc tại`j`, mở rộng nó với`i`mang lại kết thúc chuỗi tốt nhất tại`i`. 

Phép truy toán khám phá mọi tiền thân hợp lệ chính xác một lần, do đó không có chuỗi nào có thể bị bỏ sót. Tối đa trên tất cả`dp[i]`value là chuỗi lồng nhau hợp lệ dài nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, w, h = map(int, input().split())

    envelopes = []

    for i in range(1, n + 1):
        x, y = map(int, input().split())

        if x > w and y > h:
            envelopes.append((x, y, i))

    if not envelopes:
        print(0)
        return

    envelopes.sort()

    m = len(envelopes)

    dp = [1] * m
    parent = [-1] * m

    best_len = 1
    best_idx = 0

    for i in range(m):
        wi, hi, _ = envelopes[i]

        for j in range(i):
            wj, hj, _ = envelopes[j]

            if wj < wi and hj < hi:
                if dp[j] + 1 > dp[i]:
                    dp[i] = dp[j] + 1
                    parent[i] = j

        if dp[i] > best_len:
            best_len = dp[i]
            best_idx = i

    path = []

    cur = best_idx

    while cur != -1:
        path.append(envelopes[cur][2])
        cur = parent[cur]

    path.reverse()

    print(best_len)
    print(*path)

solve()
```Phần đầu tiên lọc ngay những phong bì không sử dụng được. Điều này giúp đơn giản hóa DP vì mọi phong bì còn lại đảm bảo vừa với bưu thiếp. 

Việc sắp xếp sử dụng thứ tự bộ dữ liệu mặc định của Python, vì vậy các phong bì được sắp xếp theo chiều rộng trước, sau đó là chiều cao. Vì quá trình chuyển đổi vẫn yêu cầu so sánh chặt chẽ trên cả hai chiều nên chiều rộng bằng nhau hoặc chiều cao bằng nhau không bao giờ tạo ra chuỗi không hợp lệ. 

Vòng lặp DP kiểm tra mọi phong bì trước đó như một phong bì tiền thân có thể có. Với`5000`phong bì, số lượng so sánh trong trường hợp xấu nhất là khoảng 25 triệu, có thể chấp nhận được trong Python khi được triển khai lặp đi lặp lại. 

các`parent`mảng rất quan trọng cho việc tái thiết. Không có nó, chúng ta có thể tính độ dài chuỗi tối đa nhưng sẽ không biết đường bao nào đã hình thành chuỗi đó. 

Một chi tiết tinh tế là sử dụng các bất đẳng thức nghiêm ngặt:```
wj < wi and hj < hi
```Thay thế chúng bằng`<=`sẽ cho phép các kích thước bằng nhau vào chuỗi một cách không chính xác. 

Một điểm tinh tế khác là lọc trước khi sắp xếp. Nếu vẫn còn những phong bì không sử dụng được, DP có thể vô tình tạo ra chuỗi bắt đầu từ những phong bì không thể chứa bưu thiếp. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 1 1
2 2
2 2
```Sau khi lọc và sắp xếp: 

| tôi | Phong bì | Chỉ mục gốc | 
| --- | --- | --- | 
| 0 | (2,2) | 1 | 
| 1 | (2,2) | 2 | 

Tiến trình DP: 

| tôi | Phong bì | Hợp lệ Trước đó | dp[i] | cha mẹ[i] | 
| --- | --- | --- | --- | --- | 
| 0 | (2,2) | không | 1 | -1 | 
| 1 | (2,2) | không | 1 | -1 | 

Chiều dài chuỗi tốt nhất là`1`. 

Một câu trả lời hợp lệ:```
1
1
```Ví dụ này chứng minh tại sao sự bất bình đẳng nghiêm ngặt lại quan trọng. Mặc dù các phong bì giống hệt nhau nhưng không cái nào có thể chứa cái kia. 

### Ví dụ 2 

đầu vào:```
5 1 1
2 3
3 4
4 5
3 3
5 6
```Phong bì được sắp xếp: 

| tôi | Phong bì | Chỉ mục gốc | 
| --- | --- | --- | 
| 0 | (2,3) | 1 | 
| 1 | (3,3) | 4 | 
| 2 | (3,4) | 2 | 
| 3 | (4,5) | 3 | 
| 4 | (5,6) | 5 | 

Tiến trình DP: 

| tôi | Phong bì | Tốt nhất trước đó | dp[i] | cha mẹ[i] | 
| --- | --- | --- | --- | --- | 
| 0 | (2,3) | không | 1 | -1 | 
| 1 | (3,3) | không | 1 | -1 | 
| 2 | (3,4) | (2,3) | 2 | 0 | 
| 3 | (4,5) | (3,4) | 3 | 2 | 
| 4 | (5,6) | (4,5) | 4 | 3 | 

Tái thiết mang lại:```
1 -> 2 -> 3 -> 5
```Dấu vết này cho thấy độ rộng bằng nhau ngăn cản quá trình chuyển đổi như thế nào. Phong bì`(3,3)`không thể đi trước`(3,4)`vì chiều rộng phải tăng nghiêm ngặt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Mọi phong bì đều kiểm tra tất cả các phong bì trước đó | 
| Không gian | O(n) | Mảng DP và mảng gốc lưu trữ một giá trị trên mỗi phong bì | 

Với`n ≤ 5000`, DP bậc hai thực hiện khoảng 25 triệu phép so sánh trong trường hợp xấu nhất. Điều đó phù hợp thoải mái trong giới hạn thời gian trong Python khi được triển khai lặp đi lặp lại với đầu vào nhanh. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def solve():
        n, w, h = map(int, input().split())

        envelopes = []

        for i in range(1, n + 1):
            x, y = map(int, input().split())

            if x > w and y > h:
                envelopes.append((x, y, i))

        if not envelopes:
            return "0"

        envelopes.sort()

        m = len(envelopes)

        dp = [1] * m
        parent = [-1] * m

        best_len = 1
        best_idx = 0

        for i in range(m):
            wi, hi, _ = envelopes[i]

            for j in range(i):
                wj, hj, _ = envelopes[j]

                if wj < wi and hj < hi:
                    if dp[j] + 1 > dp[i]:
                        dp[i] = dp[j] + 1
                        parent[i] = j

            if dp[i] > best_len:
                best_len = dp[i]
                best_idx = i

        path = []

        cur = best_idx

        while cur != -1:
            path.append(envelopes[cur][2])
            cur = parent[cur]

        path.reverse()

        return str(best_len) + "\n" + " ".join(map(str, path))

    return solve().strip()

# provided sample
assert run(
"""2 1 1
2 2
2 2
"""
) in ["1\n1", "1\n2"], "sample 1"

# no envelope fits
assert run(
"""2 5 5
5 6
6 5
"""
) == "0", "strict inequality"

# simple increasing chain
assert run(
"""3 1 1
2 2
3 3
4 4
"""
) == "3\n1 2 3", "basic chain"

# equal widths block transitions
assert run(
"""4 1 1
2 2
2 3
2 4
3 5
"""
) in ["2\n1 4", "2\n2 4", "2\n3 4"], "equal widths"

# single envelope
assert run(
"""1 1 1
2 2
"""
) == "1\n1", "minimum valid case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Không có phong bì phù hợp |`0`| Bất bình đẳng nghiêm ngặt về kích thước bưu thiếp | 
| Tăng phong bì nghiêm ngặt | Chiều dài chuỗi đầy đủ | Độ chính xác DP cơ bản | 
| Chiều rộng bằng nhau | Chuỗi không thể sử dụng chiều rộng bằng nhau | Điều kiện chuyển tiếp đúng | 
| Phong bì hợp lệ duy nhất | Chiều dài`1`| Trường hợp không trống tối thiểu | 

## Vỏ cạnh 

Hãy xem xét trường hợp các phong bì có chung kích thước. 

đầu vào:```
3 1 1
2 2
2 2
3 3
```Sau khi sắp xếp:```
(2,2), (2,2), (3,3)
```Khi xử lý thứ hai`(2,2)`, điều kiện:```
2 < 2
```không thành công nên không có quá trình chuyển đổi nào xảy ra. Cả hai bản sao vẫn là chuỗi dài`1`. Sau đó`(3,3)`có thể mở rộng một trong hai, tạo ra chuỗi dài cuối cùng`2`. 

Bây giờ hãy xem xét những phong bì chỉ vượt quá một phần kích thước của bưu thiếp. 

đầu vào:```
3 2 2
3 2
2 3
4 4
```Lọc loại bỏ`(3,2)`bởi vì chiều cao không hoàn toàn lớn hơn`2`, và loại bỏ`(2,3)`bởi vì chiều rộng không hoàn toàn lớn hơn`2`. 

Chỉ một`(4,4)`vẫn còn, vì vậy câu trả lời là:```
1
3
```Cuối cùng, hãy xem xét trường hợp hoàn toàn không thể xảy ra. 

đầu vào:```
2 5 5
5 6
6 5
```Phong bì đầu tiên không thành công vì chiều rộng bằng chiều rộng bưu thiếp. Lần thứ hai không thành công vì chiều cao bằng chiều cao của bưu thiếp. Danh sách đã lọc sẽ trống ngay lập tức và thuật toán sẽ in:```
0
```Quá trình lọc sớm này ngăn chặn các chuỗi không hợp lệ bước vào giai đoạn DP.
