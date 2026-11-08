---
title: "CF 1415C - Bóng nảy"
description: "Chúng ta có một chuỗi nhị phân biểu thị các ô ở một cấp độ. Số 1 có nghĩa là nền tảng đã tồn tại và số 0 có nghĩa là ô trống. Quả bóng phải bắt đầu nảy từ vị trí p, sau đó đến các vị trí p + k, p + 2k, v.v. Mỗi một vị trí đó phải có một nền tảng."
date: "2026-06-11T07:11:34+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "dp", "implementation"]
categories: ["algorithms"]
codeforces_contest: 1415
codeforces_index: "C"
codeforces_contest_name: "Technocup 2021 - Elimination Round 2"
rating: 1400
weight: 1415
solve_time_s: 105
verified: true
draft: false
---

[CF 1415C - Bóng nảy](https://codeforces.com/problemset/problem/1415/C) 

**Đánh giá:** 1400 
**Tags:** vũ lực, dp, thực hiện 
**Thời gian giải:** 1 phút 45s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một chuỗi nhị phân biểu thị các ô ở một cấp độ. MỘT`1`có nghĩa là một nền tảng đã tồn tại và`0`có nghĩa là ô trống. 

Bóng phải bắt đầu nảy từ vị trí`p`, sau đó thăm các vị trí`p + k`,`p + 2k`, vân vân. Mỗi một vị trí đó phải có một nền tảng. Chúng tôi được phép thêm nền tảng vào các ô trống, trả tiền`x`mỗi phép cộng. 

Có thao tác thứ hai: loại bỏ ô đầu tiên của cấp độ. Sau khi loại bỏ ô đầu tiên, mỗi ô còn lại dịch chuyển sang trái một vị trí. Chi phí này`y`mỗi lần loại bỏ. Chúng ta có thể lặp lại thao tác này, nhưng chúng ta không bao giờ được rút ngắn mức độ quá nhiều đến mức vị trí đó`p`biến mất. 

Mục tiêu là tìm ra tổng chi phí tối thiểu cần thiết để sau một số lần xóa khỏi mặt trước và một số bổ sung nền tảng, trình tự thoát trở nên hợp lệ. 

Giá trị lớn nhất của`n`là`10^5`, và tổng của tất cả`n`trên các trường hợp thử nghiệm cũng nhiều nhất là`10^5`. Điều này ngay lập tức loại trừ bất kỳ thuật toán nào kiểm tra mọi vị trí bắt đầu có thể và sau đó quét lại toàn bộ hậu tố mỗi lần. MỘT`O(n²)`giải pháp sẽ thực hiện xung quanh`10^10`hoạt động trong trường hợp xấu nhất. Chúng tôi cần thứ gì đó gần với thời gian tuyến tính cho mỗi trường hợp thử nghiệm. 

Một điểm tinh tế là việc xóa các ô sẽ thay đổi vị trí`p`kết thúc. Giả định:```
n = 5
p = 3
k = 2
s = 00100
```Nếu chúng ta xóa một ô ở phía trước, vị trí mới`p`tương ứng với chỉ số ban đầu`4`, không phải chỉ mục gốc`3`. Bất kỳ giải pháp nào xử lý việc xóa và bổ sung một cách độc lập sẽ tính ra câu trả lời sai. 

Một sai lầm dễ mắc phải khác là quên rằng chỉ các vị trí cách nhau bởi`k`vấn đề. Ví dụ:```
n = 6
p = 2
k = 3
s = 000100
```Các ô bắt buộc là vị trí`2`Và`5`. Chức vụ`3`và vị trí`4`không liên quan. Việc triển khai ngây thơ cố gắng làm cho toàn bộ hậu tố trở nên tốt đẹp sẽ phải trả giá cho những bổ sung không cần thiết. 

Trường hợp cạnh thứ ba xảy ra ở gần cuối chuỗi:```
n = 5
p = 5
k = 2
s = 00000
```Vị trí duy nhất`5`được yêu cầu. Câu trả lời chỉ đơn giản là một sự bổ sung. Mã bất cẩn thường truy cập vượt quá phần cuối của mảng khi tính toán các chuyển đổi liên quan đến`i + k`. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là thử mọi số lần xóa trước có thể. 

Nếu chúng ta xóa`d`các ô, sau đó định vị`p`trong chuỗi mới tương ứng với vị trí`p + d`trong chuỗi gốc. Bắt đầu từ vị trí đó, chúng ta có thể duyệt qua các chỉ số```
p + d,
p + d + k,
p + d + 2k,
...
```và đếm xem có bao nhiêu trong số chúng hiện chứa`0`. Mỗi ô như vậy yêu cầu bổ sung một nền tảng. Tổng chi phí trở thành:```
d * y + additions * x
```Đang thử tất cả các giá trị hợp lệ của`d`thật dễ dàng. Vấn đề là bước đếm. Nếu chúng tôi quét lại các vị trí thoát cho mỗi lần xóa, chúng tôi có thể chi tiêu`O(n)`làm việc cho mỗi ứng viên. Vì có`O(n)`các ứng cử viên, tổng số sẽ trở thành`O(n²)`. 

Quan sát quan trọng là mọi vị trí bắt đầu của ứng viên đều hỏi cùng một câu hỏi: 

> Có bao nhiêu số 0 xuất hiện trong cấp số cộng bắt đầu từ chỉ số này và tăng dần`k`? 

Những câu trả lời này chồng chéo lên nhau rất nhiều. Nếu chúng ta đã biết câu trả lời cho vị trí`i + k`, sau đó định vị`i`chỉ khác nhau ở chỗ ô có`i`bản thân nó bằng không. 

Định nghĩa:```
dp[i] = number of additions needed
        if bouncing starts at i
```Sử dụng lập chỉ mục dựa trên 0,```
dp[i] = (s[i] == '0') + dp[i + k]
```khi`i + k`tồn tại. Nếu không thì,```
dp[i] = (s[i] == '0')
```Sự lặp lại này tính toán chính xác số lượng nền tảng bị thiếu dọc theo đường dẫn thoát được yêu cầu bắt đầu từ`i`. 

Một lần tất cả`dp[i]`các giá trị đã biết, mọi số lần xóa có thể được đánh giá trong thời gian không đổi:```
cost = deletions * y + dp[start] * x
```Ở đâu`start`là vị trí ban đầu trở thành vị trí`p`sau khi xóa. 

Điều này biến vấn đề thành quét tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi`p`lập chỉ mục dựa trên 0. 
2. Tạo một mảng`dp`chiều dài`n`. 
3. Xử lý các vị trí từ phải sang trái. 
4. Đối với từng vị trí`i`, tính:```
need = 1 if s[i] == '0' else 0
```5. Nếu`i + k < n`, thêm vào`dp[i + k]`:```
dp[i] = need + dp[i + k]
```Nếu không thì:```
dp[i] = need
```Điều này cung cấp số lượng nền tảng phải được thêm vào nếu quá trình thoát bắt đầu tại`i`. 
6. Xem xét mọi số lần xóa có thể. 

Nếu chúng ta xóa`d`các ô, vị trí bắt đầu được yêu cầu trong chuỗi gốc sẽ trở thành:```
start = p + d
```7. Đối với mỗi hợp lệ`start`, tính:```
cost = (start - p) * y + dp[start] * x
```Thuật ngữ đầu tiên là chi phí xóa và thuật ngữ thứ hai là chi phí thêm tất cả các nền tảng còn thiếu trên đường thoát. 
8. Lấy mức tối thiểu đối với tất cả các ứng cử viên. 

### Tại sao nó hoạt động 

Đối với bất kỳ chỉ mục nào`i`, đường dẫn nảy bao gồm chính xác:```
i, i + k, i + 2k, ...
```Sự tái diễn```
dp[i] = missing_at_i + dp[i + k]
```đếm số lượng số không trên đường dẫn đó. Mỗi số 0 yêu cầu chính xác một bổ sung nền tảng và mỗi số 0 đều đã đáp ứng yêu cầu. 

Sau khi xóa`d`tế bào, vị trí`p`ở cấp độ mới tương ứng với vị trí`p + d`ở mức ban đầu. Những bổ sung cần thiết được chính xác`dp[p + d]`. Vì việc xóa và thêm đóng góp độc lập vào tổng chi phí nên việc đánh giá```
d * y + dp[p + d] * x
```cho mọi hợp lệ`d`kiểm tra mọi cấu hình cuối cùng khả thi. Giá trị tối thiểu của các giá trị này là câu trả lời tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n, p, k = map(int, input().split())
        s = input().strip()
        x, y = map(int, input().split())

        p -= 1

        dp = [0] * n

        for i in range(n - 1, -1, -1):
            dp[i] = (s[i] == '0')
            if i + k < n:
                dp[i] += dp[i + k]

        best = 10**18

        for start in range(p, n):
            deletions = start - p
            cost = deletions * y + dp[start] * x
            best = min(best, cost)

        ans.append(str(best))

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```Mảng lập trình động được điền từ phải sang trái vì mọi trạng thái đều phụ thuộc vào`i + k`, nằm về bên phải. 

biểu hiện```
(s[i] == '0')
```đánh giá để`True`hoặc`False`, mà Python tự động coi là`1`hoặc`0`trong các biểu thức số học. Điều này cho biết số lượng bổ sung cần thiết tại ô hiện tại. 

Vòng lặp kết thúc`start`đại diện cho tất cả số lần xóa có thể. Nếu như`start = p`, không có ô nào bị loại bỏ. Nếu như`start = p + 1`, một ô sẽ bị xóa, v.v. 

Lỗi triển khai phổ biến nhất là trộn lẫn chỉ mục dựa trên 1 và dựa trên 0. Vị trí đầu vào`p`dựa trên 1, nhưng tất cả các truy cập mảng đều dựa trên 0, vì vậy chúng tôi thực hiện ngay:```
p -= 1
```Một lỗi phổ biến khác là quên rằng chỉ những vị trí mới có thể truy cập được bằng cách tăng kích thước lặp đi lặp lại.`k`vấn đề. Sự truy hồi tự nhiên nắm bắt chính xác những vị trí đó. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
n = 10
p = 3
k = 2
s = 0101010101
x = 2
y = 2
```Sử dụng lập chỉ mục dựa trên 0,`p = 2`. 

| bắt đầu | xóa | dp[bắt đầu] | chi phí | 
| --- | --- | --- | --- | 
| 2 | 0 | 1 | 2 | 
| 3 | 1 | 0 | 2 | 
| 4 | 2 | 1 | 6 | 
| 5 | 3 | 0 | 6 | 
| 6 | 4 | 1 | 10 | 
| 7 | 5 | 0 | 10 | 
| 8 | 6 | 1 | 14 | 
| 9 | 7 | 0 | 14 | 

Chi phí tối thiểu là`2`. 

Dấu vết này cho thấy lý do tại sao việc xóa một ô cũng có thể hiệu quả như việc thêm một nền tảng. Thuật toán đánh giá cả hai khả năng và tự động chọn kết quả rẻ hơn. 

### Mẫu 2 

đầu vào:```
n = 5
p = 4
k = 1
s = 00000
x = 2
y = 10
```Sử dụng lập chỉ mục dựa trên 0,`p = 3`. 

| bắt đầu | xóa | dp[bắt đầu] | chi phí | 
| --- | --- | --- | --- | 
| 3 | 0 | 2 | 4 | 
| 4 | 1 | 1 | 12 | 

Chi phí tối thiểu là`4`. 

Việc xóa ở đây rất tốn kém, vì vậy giải pháp tối ưu giữ nguyên chuỗi và thêm nền tảng khi cần thiết. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Một lần vượt qua DP và một lần vượt qua đánh giá | 
| Không gian | O(n) | Mảng DP có chiều dài`n`| 
| Tổng số trong tất cả các bài kiểm tra | O(tổng n) | Tổng của tất cả`n`nhiều nhất là`10^5`| 

Tổng kích thước đầu vào được giới hạn bởi`10^5`, do đó, một giải pháp tuyến tính chỉ thực hiện vài trăm nghìn phép tính. Điều này thoải mái phù hợp trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline
    t = int(input())
    out = []

    for _ in range(t):
        n, p, k = map(int, input().split())
        s = input().strip()
        x, y = map(int, input().split())

        p -= 1

        dp = [0] * n
        for i in range(n - 1, -1, -1):
            dp[i] = (s[i] == '0')
            if i + k < n:
                dp[i] += dp[i + k]

        best = 10**18
        for start in range(p, n):
            best = min(
                best,
                (start - p) * y + dp[start] * x
            )

        out.append(str(best))

    return "\n".join(out)

# provided samples
assert run(
"""3
10 3 2
0101010101
2 2
5 4 1
00000
2 10
11 2 3
10110011000
4 3
"""
) == "2\n4\n10"

# minimum size
assert run(
"""1
1 1 1
0
5 7
"""
) == "5"

# already valid
assert run(
"""1
5 1 2
11111
3 4
"""
) == "0"

# deletion better than additions
assert run(
"""1
5 1 1
00001
10 1
"""
) == "4"

# off-by-one near end
assert run(
"""1
5 5 2
00000
3 1
"""
) == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n=1, s=0`|`5`| Trường hợp hợp lệ nhỏ nhất | 
| Tất cả những cái |`0`| Không cần thao tác | 
| Bổ sung đắt tiền, xóa bỏ giá rẻ |`4`| Câu trả lời tối ưu có thể liên quan đến việc xóa nhiều | 
|`p=n`|`3`| Xử lý đúng ở vị trí cuối cùng | 

## Vỏ cạnh 

Hãy xem xét:```
1
5 3 2
00100
1 1
```Không xóa, đường thoát bắt đầu ở vị trí`3`và thăm vị trí`5`. Cả hai đều`1`Và`0`, vì vậy cần phải có một phép cộng. 

Nếu chúng ta xóa một ô, hãy định vị`p`bây giờ tương ứng với vị trí ban đầu`4`. Con đường thay đổi hoàn toàn. Thuật toán xử lý việc này vì nó đánh giá`dp[2]`,`dp[3]`, Và`dp[4]`riêng. Nó không bao giờ cho rằng đường dẫn nảy là cố định. 

Bây giờ hãy xem xét:```
1
6 2 3
000100
5 1
```Các vị trí liên quan chỉ`2`Và`5`. Tế bào`3`,`4`, Và`6`không quan trọng. Sự tái diễn sau những bước nhảy vọt về quy mô`k`, vì vậy những ô không liên quan đó không bao giờ ảnh hưởng đến chi phí được tính toán. 

Cuối cùng, hãy xem xét:```
1
5 5 2
00000
3 1
```Đường dẫn nảy chỉ chứa ô cuối cùng. Trong quá trình xây dựng DP,`i + k`nằm ngoài mảng, do đó quá trình lặp lại dừng lại một cách chính xác:```
dp[4] = 1
```Câu trả lời trở thành chính xác một phép cộng, tính giá`3`. Không có truy cập ngoài giới hạn nào xảy ra và không có ô bổ sung nào được tính.
