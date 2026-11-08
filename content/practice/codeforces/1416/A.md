---
title: "CF 1416A - Những con số kỳ diệu"
description: "Chúng ta được cho một mảng có giá trị từ 1 đến n. Với mỗi độ dài k, chúng ta xem xét tất cả các mảng con có độ dài k. Giá trị x được coi là hợp lệ nếu mỗi mảng con như vậy chứa ít nhất một lần xuất hiện của x. Trong số tất cả các giá trị hợp lệ, chúng ta phải xuất ra giá trị nhỏ nhất."
date: "2026-06-11T07:02:37+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "implementation", "two-pointers"]
categories: ["algorithms"]
codeforces_contest: 1416
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 673 (Div. 1)"
rating: 1500
weight: 1416
solve_time_s: 148
verified: true
draft: false
---

[CF 1416A - k-Những con số đáng kinh ngạc](https://codeforces.com/problemset/problem/1416/A) 

**Đánh giá:** 1500 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, triển khai, hai con trỏ 
**Thời gian giải:** 2m 28s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một mảng có giá trị nằm trong khoảng`1`Và`n`. 

Đối với mỗi chiều dài`k`, chúng ta xem xét **tất cả** mảng con có độ dài`k`. Một giá trị`x`được coi là hợp lệ nếu mỗi mảng con như vậy chứa ít nhất một lần xuất hiện của`x`. Trong số tất cả các giá trị hợp lệ, chúng ta phải xuất ra giá trị nhỏ nhất. Nếu không có giá trị nào xuất hiện ở mọi độ dài-`k`mảng con, câu trả lời là`-1`. 

Một cách khác để suy nghĩ về điều kiện là: nếu một giá trị`x`bị thiếu trong một thời gian dài-`k`mảng con thì`x`không phải là một ứng cử viên Chúng ta cần những giá trị không thể tránh khỏi khi lấy bất kỳ đoạn có độ dài nào`k`. 

Tổng kích thước của tất cả các trường hợp thử nghiệm nhiều nhất là`3 · 10^5`. Bất kỳ giải pháp nào xử lý mọi`k`riêng biệt hoặc quét tất cả các mảng con cho từng giá trị sẽ quá chậm. Với`n = 3 · 10^5`, chúng ta thường cần một cái gì đó gần với`O(n log n)`hoặc`O(n)`mỗi trường hợp thử nghiệm. 

Phần khó khăn là câu trả lời phải được đưa ra cho mọi`k`từ`1`ĐẾN`n`, vì vậy chúng ta cần một cách để mô tả thời điểm một giá trị trở nên hợp lệ đối với độ dài phân đoạn nhất định. 

Hãy xem xét mảng:```
1 2 1
```Vì`k = 2`, các phân đoạn là`[1,2]`Và`[2,1]`. Giá trị`1`xuất hiện ở cả hai, vì vậy câu trả lời là`1`. 

Một cách tiếp cận bất cẩn có thể chỉ tính tổng số lần xuất hiện. Từ`2`xuất hiện một lần và`1`xuất hiện hai lần, có vẻ như tần số mới là điều quan trọng. Không phải vậy. Các vị trí quan trọng hơn nhiều so với số lượng. 

Một trường hợp quan trọng khác là:```
1 2 3 4 5
```Mỗi giá trị xuất hiện chính xác một lần. 

Vì`k = 3`, các phân đoạn là:```
[1,2,3]
[2,3,4]
[3,4,5]
```Giá trị duy nhất`3`xuất hiện trong tất cả chúng, vì vậy câu trả lời là`3`. 

Cách tiếp cận dựa trên tần số hoàn toàn bỏ qua hành vi này. 

Thêm một trường hợp Edge là một giá trị xuất hiện nhiều lần nhưng có khoảng cách lớn:```
1 1 1 2 1
```Giá trị`1`xuất hiện bốn lần, nhưng nếu có một đoạn dài không có nó thì một đoạn nào đó có thể tránh được. Điều quan trọng là khoảng cách lớn nhất giữa các lần xuất hiện liên tiếp chứ không phải tổng số lần xuất hiện. 

## Phương pháp tiếp cận 

Một giải pháp brute-force sẽ xử lý mọi`k`một cách độc lập. Đối với mỗi giá trị, chúng tôi có thể kiểm tra xem mọi độ dài-`k`mảng con chứa giá trị đó. 

Một cách là tạo ra tất cả`n-k+1`mảng con và theo dõi giá trị nào xuất hiện trong mỗi mảng con đó. Ngay cả khi tối ưu hóa, điều này ít nhất cũng nhanh chóng trở thành`O(n²)`và thường xuyên`O(n³)`. Với`n = 3 · 10^5`, những cách tiếp cận như vậy là vô vọng. 

Quan sát chính xuất phát từ việc xem xét một giá trị duy nhất`x`. 

Giả sử sự xuất hiện của`x`đang ở các vị trí:```
p1, p2, ..., pm
```Thêm hai vị trí ảo:```
0 and n+1
```Bây giờ hãy xem xét các khoảng trống:```
p1 - 0
p2 - p1
...
pm - p(m-1)
(n+1) - pm
```Hãy để khoảng cách lớn nhất là`g`. 

Điều này có nghĩa là gì? 

Nếu có khoảng cách về chiều dài`g`, thì giữa hai lần xuất hiện liên tiếp đó có`g-1`vị trí không chứa`x`. 

Một đoạn có độ dài`g-1`có thể nằm gọn hoàn toàn bên trong khoảng trống đó, tránh`x`hoàn toàn. 

Vì thế`x`không thể có mặt ở mọi đoạn dài`g-1`. 

Mặt khác, bất kỳ đoạn nào có độ dài`g`phải vượt qua ít nhất một lần xuất hiện của`x`, vì không có khoảng trống đủ lớn để giữ`g`các vị trí liên tiếp mà không có`x`. 

Điều này mang lại một đặc tính rất hữu ích: 

Một giá trị`x`xuất hiện trong mọi mảng con có độ dài`k`nếu và chỉ nếu```
k ≥ largest_gap(x)
```Do đó mọi giá trị đều có một ngưỡng:```
need[x] = largest_gap(x)
```Cho tất cả`k ≥ need[x]`, giá trị`x`là hợp lệ. 

Bây giờ vấn đề trở thành: 

Đối với mỗi`k`, tìm giá trị nhỏ nhất có ngưỡng lớn nhất`k`. 

Điều này dễ dàng hơn nhiều. 

Chúng tôi tính toán`need[x]`cho mọi giá trị. Sau đó với mỗi giá trị`x`, nó trở thành một ứng cử viên bắt đầu từ vị trí`need[x]`. Chúng tôi lưu trữ:```
ans[need[x]] = min(ans[need[x]], x)
```Sau đó, quá trình quét tiền tố tối thiểu sẽ truyền các ứng cử viên tới các ứng cử viên lớn hơn.`k`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n2) đến O(n³) | O(n²) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo danh sách vị trí xuất hiện cho mọi giá trị từ`1`ĐẾN`n`. 
2. Đối với từng vị trí mảng`i`, nối thêm`i`vào danh sách xuất hiện của`a[i]`. 
3. Với mọi giá trị`x`, tính khoảng cách lớn nhất của nó. 

Bắt đầu với vị trí trước đó`0`. Đối với mỗi vị trí xuất hiện`p`, cập nhật:```
gap = p - previous
```Giữ khoảng cách lớn nhất như vậy. 

Sau khi xử lý tất cả các lần xuất hiện, hãy xem xét:```
(n + 1) - last_occurrence
```Điều này xử lý hậu tố sau lần xuất hiện cuối cùng. 
4. Hãy để khoảng cách lớn nhất này`need[x]`. 

Giá trị`x`có giá trị cho mọi`k ≥ need[x]`. 
5. Tạo một mảng`ans`được khởi tạo với vô cùng. 
6. Với mọi giá trị`x`xuất hiện ít nhất một lần, hãy cập nhật:```
ans[need[x]] = min(ans[need[x]], x)
```Chúng tôi ghi lại giá trị nhỏ nhất có ngưỡng chính xác`need[x]`. 
7. Thực hiện quét tiền tố tối thiểu:```
ans[k] = min(ans[k], ans[k-1])
```cho tất cả`k`từ`2`ĐẾN`n`. 

Khi một giá trị trở nên hợp lệ, nó vẫn hợp lệ cho tất cả các độ dài phân đoạn lớn hơn. 
8. Thay thế mọi mục nhập vô cùng còn lại bằng`-1`. 
9. Đầu ra`ans[1] ... ans[n]`. 

### Tại sao nó hoạt động 

Đối với một giá trị`x`, khoảng cách lớn nhất giữa các lần xuất hiện liên tiếp (bao gồm cả ranh giới mảng) xác định vùng dài nhất không có`x`. 

Một mảng con có thể tránh được`x`chính xác khi nó nằm hoàn toàn bên trong một vùng như vậy. Độ dài lớn nhất có thể của việc tránh mảng con`x`là`largest_gap(x) - 1`. 

Do đó, mọi mảng con có độ dài`k`chứa`x`chính xác khi nào`k ≥ largest_gap(x)`. 

Thuật toán tính ngưỡng này cho mọi giá trị. Một giá trị trở thành ứng cử viên hợp lệ bắt đầu từ ngưỡng của nó và vẫn hợp lệ sau đó. Lấy giá trị tối thiểu trong số tất cả các ứng cử viên cho mỗi`k`tạo ra chính xác số k-tuyệt vời cần thiết. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    out = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        pos = [[] for _ in range(n + 1)]

        for i, x in enumerate(a, start=1):
            pos[x].append(i)

        INF = n + 1
        ans = [INF] * (n + 1)

        for value in range(1, n + 1):
            if not pos[value]:
                continue

            mx_gap = 0
            prev = 0

            for p in pos[value]:
                mx_gap = max(mx_gap, p - prev)
                prev = p

            mx_gap = max(mx_gap, n + 1 - prev)

            ans[mx_gap] = min(ans[mx_gap], value)

        for k in range(2, n + 1):
            ans[k] = min(ans[k], ans[k - 1])

        result = []

        for k in range(1, n + 1):
            if ans[k] == INF:
                result.append("-1")
            else:
                result.append(str(ans[k]))

        out.append(" ".join(result))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Danh sách xảy ra là nền tảng của giải pháp. Đối với mỗi giá trị, chúng ta chỉ cần các vị trí mà nó xuất hiện, vì khoảng cách lớn nhất giữa các lần xuất hiện hoàn toàn xác định thời điểm giá trị đó trở nên hợp lệ. 

Các vị trí ranh giới`0`Và`n+1`được xử lý ngầm thông qua`prev = 0`và kiểm tra cuối cùng`n + 1 - prev`. Quên một trong hai ranh giới là lỗi phổ biến nhất trong vấn đề này. Một giá trị xuất hiện ở vị trí đầu tiên vẫn để lại khoảng cách kích thước tiền tố`1`và một giá trị xuất hiện ở vị trí cuối cùng vẫn để lại một khoảng cách về kích thước hậu tố`1`. 

Mảng`ans`lưu trữ thông tin theo ngưỡng. Nếu như`need[x] = 5`, thì giá trị`x`đầu tiên có hiệu lực tại`k = 5`. Thẻ tối thiểu tiền tố chuyển đổi các mục nhập ngưỡng này thành câu trả lời cho tất cả các độ dài lớn hơn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5
1 2 3 4 5
```Khoảng trống xuất hiện: 

| Giá trị | Vị trí | Khoảng cách lớn nhất | 
| --- | --- | --- | 
| 1 | [1] | 5 | 
| 2 | [2] | 4 | 
| 3 | [3] | 3 | 
| 4 | [4] | 4 | 
| 5 | [5] | 5 | 

Sau khi chèn giá trị theo ngưỡng: 

| k | Được lưu trữ tối thiểu | 
| --- | --- | 
| 1 | INF | 
| 2 | INF | 
| 3 | 3 | 
| 4 | 2 | 
| 5 | 1 | 

Sau tiền tố cực tiểu: 

| k | Trả lời | 
| --- | --- | 
| 1 | -1 | 
| 2 | -1 | 
| 3 | 3 | 
| 4 | 2 | 
| 5 | 1 | 

Đầu ra:```
-1 -1 3 2 1
```Ví dụ này cho thấy rằng một giá trị chỉ xuất hiện một lần vẫn có thể trở thành câu trả lời cho độ dài đoạn đủ lớn. 

### Ví dụ 2 

đầu vào:```
6
1 3 1 5 3 1
```Phân tích sự cố: 

| Giá trị | Vị trí | Khoảng cách lớn nhất | 
| --- | --- | --- | 
| 1 | [1,3,6] | 3 | 
| 2 | [] | - | 
| 3 | [2,5] | 3 | 
| 4 | [] | - | 
| 5 | [4] | 4 | 

Chèn ngưỡng: 

| k | Được lưu trữ tối thiểu | 
| --- | --- | 
| 3 | 1 | 
| 4 | 5 | 

Tiền tố tối thiểu: 

| k | Trả lời | 
| --- | --- | 
| 1 | -1 | 
| 2 | -1 | 
| 3 | 1 | 
| 4 | 1 | 
| 5 | 1 | 
| 6 | 1 | 

Đầu ra:```
-1 -1 1 1 1 1
```Dấu vết này thể hiện tính chất đơn điệu. Một lần giá trị`1`trở nên hợp lệ tại`k = 3`, nó vẫn hợp lệ cho mọi lớn hơn`k`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi lần xuất hiện được xử lý một số lần không đổi | 
| Không gian | O(n) | Danh sách lần xuất hiện và mảng câu trả lời lưu trữ dữ liệu O(n) | 

Trên tất cả các trường hợp thử nghiệm, tổng số`n`nhiều nhất là`3 · 10^5`, do đó độ phức tạp tổng thể vẫn còn`O(3 · 10^5)`. Điều này thoải mái phù hợp trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    def input():
        return sys.stdin.readline()

    t = int(input())
    out = []

    for _ in range(t):
        n = int(input())
        a = list(map(int, input().split()))

        pos = [[] for _ in range(n + 1)]

        for i, x in enumerate(a, start=1):
            pos[x].append(i)

        INF = n + 1
        ans = [INF] * (n + 1)

        for value in range(1, n + 1):
            if not pos[value]:
                continue

            prev = 0
            mx_gap = 0

            for p in pos[value]:
                mx_gap = max(mx_gap, p - prev)
                prev = p

            mx_gap = max(mx_gap, n + 1 - prev)
            ans[mx_gap] = min(ans[mx_gap], value)

        for k in range(2, n + 1):
            ans[k] = min(ans[k], ans[k - 1])

        out.append(
            " ".join(
                "-1" if ans[k] == INF else str(ans[k])
                for k in range(1, n + 1)
            )
        )

    return "\n".join(out)

# provided samples
assert run(
"""3
5
1 2 3 4 5
5
4 4 4 4 2
6
1 3 1 5 3 1
"""
) == (
"""-1 -1 3 2 1
-1 4 4 4 2
-1 -1 1 1 1 1"""
), "sample"

# minimum size
assert run(
"""1
1
1
"""
) == "1", "single element"

# all equal
assert run(
"""1
4
2 2 2 2
"""
) == "2 2 2 2", "all equal values"

# alternating pattern
assert run(
"""1
5
1 2 1 2 1
"""
) == "-1 1 1 1 1", "alternating occurrences"

# boundary gap check
assert run(
"""1
3
1 2 3
"""
) == "-1 2 1", "prefix and suffix gaps"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`[1]`|`1`| Ví dụ nhỏ nhất có thể | 
|`[2,2,2,2]`|`2 2 2 2`| Giá trị lặp lại với kích thước khoảng cách 1 | 
|`[1,2,1,2,1]`|`-1 1 1 1 1`| Nhiều lần xuất hiện và lan truyền ngưỡng | 
|`[1,2,3]`|`-1 2 1`| Xử lý đúng khoảng trống ranh giới | 

## Vỏ cạnh 

Hãy xem xét:```
1
3
1 2 3
```Giá trị`2`xuất hiện ở giữa. Khoảng trống của nó là:```
2 - 0 = 2
4 - 2 = 2
```Khoảng cách lớn nhất là`2`, do đó nó trở nên hợp lệ cho`k = 2`. 

Giá trị`1`có khoảng cách lớn nhất`3`, vì vậy nó chỉ có hiệu lực đối với`k = 3`. 

Đầu ra là:```
-1 2 1
```Không tính đến các vị trí ảo`0`Và`n+1`, câu trả lời này sẽ sai. 

Bây giờ hãy xem xét:```
1
4
2 2 2 2
```Những khoảng trống đều là`1`, Vì thế`need[2] = 1`. 

Thuật toán đặt giá trị`2`Tại`ans[1]`và quá trình quét tối thiểu tiền tố sẽ truyền nó tới mọi giá trị lớn hơn`k`. 

Đầu ra:```
2 2 2 2
```Điều này xác minh rằng một giá trị xuất hiện ở mọi nơi là hợp lệ ngay lập tức đối với tất cả độ dài phân đoạn. 

Cuối cùng hãy xem xét:```
1
5
1 2 1 2 1
```Đối với giá trị`1`những khoảng trống là:```
1, 2, 2, 2
```Khoảng cách lớn nhất là`2`, vậy mỗi đoạn có độ dài ít nhất`2`phải chứa một`1`. 

Thuật toán ghi lại:```
need[1] = 2
```và sản xuất:```
-1 1 1 1 1
```Ví dụ này cho thấy tại sao khoảng cách lớn nhất, chứ không phải tần suất, lại là đại lượng quyết định tính hợp lệ.
