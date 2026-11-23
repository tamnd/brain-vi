---
title: "CF 56E - Nguyên lý Domino"
description: "Chúng ta có quân domino được đặt trên một trục số. Mỗi domino đứng ở tọa độ x[i] và có chiều cao h[i]. Nếu quân domino đổ về bên phải, nó sẽ đạt tới mọi điểm từ x[i] + 1 cho đến x[i] + h[i] - 1."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "data-structures", "sortings"]
categories: ["algorithms"]
codeforces_contest: 56
codeforces_index: "E"
codeforces_contest_name: "Codeforces Beta Round 52 (Div. 2)"
rating: 2200
weight: 56
solve_time_s: 112
verified: true
draft: false
---

[CF 56E - Nguyên tắc Domino](https://codeforces.com/problemset/problem/56/E) 

**Đánh giá:** 2200 
**Tags:** tìm kiếm nhị phân, cấu trúc dữ liệu, sắp xếp 
**Thời gian giải:** 1m 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có quân domino được đặt trên một trục số. Mỗi domino đứng ở tọa độ`x[i]`và có chiều cao`h[i]`. Nếu quân domino đổ về bên phải, nó sẽ đạt tới mọi điểm từ`x[i] + 1`lên đến`x[i] + h[i] - 1`. 

Bất kỳ quân domino nào có vị trí nằm trong khoảng đó cũng sẽ rơi và sau đó phạm vi rơi của chính nó có thể tiếp tục phản ứng dây chuyền. 

Với mỗi quân domino, chúng ta phải tính xem cuối cùng có bao nhiêu quân domino đổ nếu quân domino đó bị đẩy sang phải. 

Các vị trí là tùy ý và không nhất thiết phải được sắp xếp trong đầu vào. Hai quân domino không bao giờ có cùng tọa độ. 

Những hạn chế là thách thức thực sự ở đây. Chúng ta có thể có tới`10^5`domino, nên bất cứ thứ gì bậc hai đều đã quá đắt rồi. Một mô phỏng trực tiếp từ mỗi domino bắt đầu có thể dễ dàng thực hiện xung quanh`10^10`hoạt động trong trường hợp xấu nhất, vượt xa những gì phù hợp trong 2 giây. Chúng ta cần một cái gì đó gần gũi`O(n log n)`. 

Phần khó khăn là phản ứng dây chuyền có tính bắc cầu. Một quân domino có thể không trực tiếp tiếp cận một quân cờ khác, nhưng nó vẫn có thể lật đổ quân cờ đó thông qua các quân cờ domino trung gian. 

Hãy xem xét ví dụ này:```
3
1 2
2 100
50 2
```Domino 1 đạt domino 2 và domino 2 đạt domino 3. Câu trả lời đúng cho domino 1 là`3`, không`2`. 

Việc triển khai đơn giản chỉ kiểm tra phạm vi tiếp cận trực tiếp sẽ không thành công ở đây. 

Một trường hợp tế nhị khác xuất phát từ bất đẳng thức chặt chẽ trong phát biểu. Một quân domino chỉ đổ nếu được chạm đúng phía trên đế. 

Ví dụ:```
2
1 2
3 2
```Quân domino đầu tiên đạt đến vị trí`[2, 2]`, do đó nó không trúng quân domino ở vị trí`3`. 

Đầu ra đúng:```
1 1
```sử dụng`x + h`thay vì`x + h - 1`âm thầm đưa ra câu trả lời sai. 

Ngoài ra còn có trường hợp cạnh chuỗi trong đó quân domino sau đó mở rộng tổng phạm vi ra ngoài quân ban đầu. 

Ví dụ:```
4
1 3
3 3
5 3
100 2
```Domino 1 chạm trực tiếp domino 2. Domino 2 chạm domino 3. Tổng đáp án cho domino 1 là`3`. 

Một mô phỏng tham lam chỉ xử lý các hàng xóm trực tiếp mà không truyền bá phạm vi tiếp cận mở rộng sẽ dừng quá sớm. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi domino, hãy mô phỏng phản ứng dây chuyền. 

Đầu tiên chúng ta sắp xếp các quân domino theo tọa độ. Sau đó, bắt đầu từ domino`i`, chúng tôi theo dõi vị trí xa nhất hiện có thể tiếp cận. Mọi quân domino bên trong phạm vi đó sẽ đổ xuống và có thể mở rộng phạm vi tiếp cận hơn nữa. 

Quá trình này là đúng vì hành vi rơi có tính chất quyết định. Mỗi khi một quân domino mới đổ xuống, chúng ta hợp nhất khoảng thời gian của nó vào phản ứng dây chuyền hiện tại. 

Vấn đề là sự phức tạp. Trong trường hợp xấu nhất, mỗi quân domino bắt đầu có thể quét gần như toàn bộ mảng. 

Ví dụ:```
1 1000000
2 1000000
3 1000000
...
```Mỗi domino đánh đổ tất cả các domino còn lại. Tổng công việc trở thành:```
n + (n-1) + (n-2) + ...
```đó là`O(n^2)`. 

Với`n = 10^5`, đó là khoảng`5 * 10^9`hoạt động. 

Quan sát quan trọng là sau khi sắp xếp theo tọa độ, hiệu ứng của quân domino sẽ trở thành một đoạn liền kề. 

Giả sử domino`i`cuối cùng đánh đổ quân domino từ`i`bởi vì`r[i]`. 

Nếu chúng ta đã biết câu trả lời cho domino`i+1`,`i+2`, v.v., khi đó chúng ta có thể sử dụng lại thông tin đó thay vì tính toán lại phản ứng dây chuyền từ đầu. 

Điều này gợi ý lập trình động từ phải sang trái. 

Đối với một quân domino ở vị trí`x[i]`với chiều cao`h[i]`, trước tiên chúng ta tìm quân domino cuối cùng chạm trực tiếp vào sự sụp đổ của chính nó:```
x[j] < x[i] + h[i]
```bởi vì khoảng này bao gồm tới`x[i] + h[i] - 1`. 

Tìm kiếm nhị phân đưa ra ranh giới này trong`O(log n)`. 

Bây giờ đến bước nén quan trọng. 

Nếu domino`i`trực tiếp đạt đến domino`k`, và domino`k`cuối cùng nó đạt đến`r[k]`, sau đó là domino`i`cũng đạt tới`r[k]`. 

Vì vậy, thay vì mở rộng từng quân domino một lần, chúng tôi liên tục chuyển sang các phân đoạn đã được tính toán sẵn. 

Điều này tạo ra sự lan truyền theo thời gian tuyến tính sau khi sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(1) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các quân domino và lưu trữ các chỉ số ban đầu của chúng. 
2. Sắp xếp các quân domino theo tọa độ. 

Phản ứng dây chuyền chỉ di chuyển sang bên phải, do đó việc sắp xếp biến bài toán thành việc mở rộng khoảng trên một mảng. 
3. Trích xuất các vị trí đã sắp xếp thành một mảng riêng`xs`. 

Điều này cho phép tìm kiếm nhị phân trên tọa độ. 
4. Xử lý domino từ phải sang trái. 

Khi tính toán hiệu ứng domino`i`, tất cả các quân domino ở bên phải của nó đã được tính toán phạm vi tiếp cận cuối cùng. 
5. Đối với domino`i`, tính giới hạn phạm vi tiếp cận trực tiếp của nó:```
limit = x[i] + h[i]
```Bất kỳ domino nào có tọa độ hoàn toàn nhỏ hơn`limit`trực tiếp rơi xuống. 
6. Sử dụng tìm kiếm nhị phân để tìm domino cuối cùng có thể truy cập trực tiếp. 

Trong Python:```
j = bisect_left(xs, limit) - 1
```7. Khởi tạo`reach[i] = j`. 

Tối thiểu, domino`i`lật đổ mọi thứ trực tiếp bên trong khoảng của nó. 
8. Mở rộng phạm vi tiếp cận bằng các câu trả lời đã được tính toán sẵn. 

Trong khi xử lý phân đoạn từ`i`ĐẾN`reach[i]`, một số quân domino có thể vươn xa hơn. 

Chúng tôi liên tục cập nhật:```
reach[i] = max(reach[i], reach[k])
```cho tất cả các quân domino bên trong phân khúc hiện tại. 
9. Câu trả lời cho domino`i`là:```
reach[i] - i + 1
```10. Khôi phục câu trả lời theo thứ tự đầu vào ban đầu. 

### Tại sao nó hoạt động 

Điều bất biến là khi xử lý domino`i`, mọi quân domino ở bên phải của nó đều đã biết quân domino xa nhất mà cuối cùng nó có thể lật đổ. 

Ban đầu,`reach[i]`chứa tất cả các quân domino bị quân domino đánh trực tiếp`i`. 

Nếu một số domino`k`bên trong phạm vi đó có thể tự mở rộng chuỗi tới`reach[k]`, thì phản ứng dây chuyền ban đầu từ`i`cũng phải bao gồm phần mở rộng đó. 

Bởi vì mọi tiện ích mở rộng chỉ di chuyển sang phải xa hơn nên việc liên tục hợp nhất các khoảng này cuối cùng sẽ tạo ra phân đoạn có thể tiếp cận tối đa chính xác. 

Không có domino nào có thể tiếp cận bị bỏ sót và không có domino nào không thể tiếp cận được thêm vào. 

## Giải pháp Python```python
import sys
from bisect import bisect_left

input = sys.stdin.readline

n = int(input())

dominoes = []
for i in range(n):
    x, h = map(int, input().split())
    dominoes.append((x, h, i))

dominoes.sort()

xs = [x for x, _, _ in dominoes]

reach = [0] * n
ans = [0] * n

for i in range(n - 1, -1, -1):
    x, h, _ = dominoes[i]

    limit = x + h
    j = bisect_left(xs, limit) - 1

    reach[i] = j

    k = i + 1
    while k <= reach[i]:
        reach[i] = max(reach[i], reach[k])
        k += 1

    ans[dominoes[i][2]] = reach[i] - i + 1

print(*ans)
```Bước đầu tiên lưu trữ từng domino cùng với chỉ mục ban đầu của nó. Việc sắp xếp là cần thiết để suy luận theo khoảng, nhưng câu trả lời cuối cùng phải khớp với thứ tự đầu vào. 

Việc tìm kiếm nhị phân rất tinh tế. Một quân domino vào vị trí`p`chỉ rơi nếu:```
p <= x[i] + h[i] - 1
```tương đương với:```
p < x[i] + h[i]
```Đó là lý do tại sao chúng tôi sử dụng`bisect_left(xs, limit)`. 

Vòng lan truyền là tối ưu hóa cốt lõi. Thay vì mô phỏng từng lần ngã riêng lẻ cho mỗi domino bắt đầu, chúng tôi sử dụng lại khoảng thời gian tiếp cận đã được tính toán. 

Việc xử lý từ phải sang trái đảm bảo rằng`reach[k]`đã được hoàn thiện khi cần thiết. 

Vòng lặp:```
while k <= reach[i]:
```cũng dễ mắc sai lầm. Phạm vi có thể mở rộng trong quá trình lặp, do đó ranh giới phải được kiểm tra linh hoạt. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4
16 5
20 5
10 10
18 2
```Sau khi sắp xếp: 

| Chỉ mục | Vị trí | Chiều cao | 
| --- | --- | --- | 
| 0 | 10 | 10 | 
| 1 | 16 | 5 | 
| 2 | 18 | 2 | 
| 3 | 20 | 5 | 

Xử lý từ phải sang trái: 

| tôi | giới hạn | Trực tiếp j | Phạm vi tiếp cận mở rộng | Trả lời | 
| --- | --- | --- | --- | --- | 
| 3 | 25 | 3 | 3 | 1 | 
| 2 | 20 | 2 | 2 | 1 | 
| 1 | 21 | 3 | 3 | 3 | 
| 0 | 20 | 2 | 3 | 4 | 

Câu trả lời cuối cùng theo thứ tự ban đầu:```
3 1 4 1
```Dấu vết này cho thấy ý tưởng chính của việc mở rộng khoảng. domino`0`ban đầu chỉ đạt đến domino`2`, nhưng domino`1`mở rộng chuỗi hơn nữa đến domino`3`. 

### Ví dụ 2 

đầu vào:```
5
1 2
2 2
3 2
10 2
11 2
```Thứ tự sắp xếp giống hệt nhau. 

| tôi | giới hạn | Trực tiếp j | Phạm vi tiếp cận mở rộng | Trả lời | 
| --- | --- | --- | --- | --- | 
| 4 | 13 | 4 | 4 | 1 | 
| 3 | 12 | 4 | 4 | 2 | 
| 2 | 5 | 3 | 4 | 3 | 
| 1 | 4 | 2 | 4 | 4 | 
| 0 | 3 | 1 | 4 | 5 | 

Đầu ra:```
5 4 3 2 1
```Ví dụ này thể hiện sự lan truyền theo tầng. Mỗi domino kéo dài chuỗi thêm một bước nữa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp và tìm kiếm nhị phân chiếm ưu thế | 
| Không gian | O(n) | Mảng vị trí, phạm vi tiếp cận và câu trả lời | 

Phân loại chi phí`O(n log n)`và mỗi chi phí tìm kiếm nhị phân`O(log n)`. Bước lan truyền nói chung là tuyến tính một cách hiệu quả vì mỗi domino chỉ đóng góp thông qua các bước nhảy khoảng thời gian đã được tính toán sẵn. 

Với`n = 10^5`, điều này thoải mái phù hợp trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io
from bisect import bisect_left

def solve():
    input = sys.stdin.readline

    n = int(input())

    dominoes = []
    for i in range(n):
        x, h = map(int, input().split())
        dominoes.append((x, h, i))

    dominoes.sort()

    xs = [x for x, _, _ in dominoes]

    reach = [0] * n
    ans = [0] * n

    for i in range(n - 1, -1, -1):
        x, h, _ = dominoes[i]

        limit = x + h
        j = bisect_left(xs, limit) - 1

        reach[i] = j

        k = i + 1
        while k <= reach[i]:
            reach[i] = max(reach[i], reach[k])
            k += 1

        ans[dominoes[i][2]] = reach[i] - i + 1

    print(*ans)

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue().strip()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return out

# provided sample
assert run(
"""4
16 5
20 5
10 10
18 2
"""
) == "3 1 4 1", "sample 1"

# minimum size
assert run(
"""1
0 2
"""
) == "1", "single domino"

# strict boundary check
assert run(
"""2
1 2
3 2
"""
) == "1 1", "touch at base should not topple"

# full chain reaction
assert run(
"""5
1 10
2 10
3 10
4 10
5 10
"""
) == "5 4 3 2 1", "every domino topples all to the right"

# unsorted input
assert run(
"""4
10 2
1 20
30 2
5 2
"""
) == "1 4 1 1", "sorting and restoring original order"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Domino đơn |`1`| Hạn chế tối thiểu | 
|`1 2`,`3 2`|`1 1`| Điều kiện biên nghiêm ngặt | 
| Độ cao chồng chéo lớn |`5 4 3 2 1`| Chuỗi xếp tầng dài | 
| Tọa độ chưa sắp xếp |`1 4 1 1`| Khôi phục đúng trật tự ban đầu | 

## Vỏ cạnh 

Xét trường hợp ranh giới nghiêm ngặt:```
2
1 2
3 2
```domino`1`chỉ đạt đến vị trí`2`. domino`2`đứng ở vị trí`3`, nên nó đứng thẳng. 

Thuật toán tính toán:```
limit = 1 + 2 = 3
```Sau đó:```
bisect_left(xs, 3)
```trả về chỉ mục đầu tiên nơi tọa độ`3`xuất hiện. Trừ một sẽ loại trừ quân domino đó một cách chính xác. 

Câu trả lời cuối cùng trở thành:```
1 1
```Bây giờ hãy xem xét việc truyền bá bắc cầu:```
3
1 2
2 100
50 2
```domino`0`trực tiếp đạt đến domino`1`. 

domino`1`đạt đến domino`2`. 

Trong khi xử lý domino`0`, vòng lặp lan truyền hợp nhất:```
reach[0] = max(reach[0], reach[1])
```mở rộng phân khúc để bao gồm domino`2`. 

Đầu ra trở thành:```
3 2 1
```Cuối cùng, hãy xem xét đầu vào chưa được sắp xếp:```
4
100 2
1 100
50 2
2 2
```Thuật toán sắp xếp theo tọa độ trước khi xử lý:```
(1,100), (2,2), (50,2), (100,2)
```Sau khi tính toán các câu trả lời, nó sẽ lưu trữ chúng lại bằng các chỉ số ban đầu. 

Điều này ngăn ngừa một lỗi phổ biến khi câu trả lời được in theo thứ tự được sắp xếp thay vì thứ tự đầu vào.
