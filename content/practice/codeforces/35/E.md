---
title: "CF 35E - Diễu hành"
description: "Mỗi tòa nhà chọc trời là một hình chữ nhật thẳng hàng nằm trên mặt đất. Một tòa nhà có các thông số (h, l, r) chiếm mọi điểm có l ≤ x ≤ r và 0 ≤ y ≤ h."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "sortings"]
categories: ["algorithms"]
codeforces_contest: 35
codeforces_index: "E"
codeforces_contest_name: "Codeforces Beta Round 35 (Div. 2)"
rating: 2100
weight: 35
solve_time_s: 101
verified: true
draft: false
---
[CF 35E - Diễu hành](https://codeforces.com/problemset/problem/35/E) 

**Xếp hạng:** 2100 
**Thẻ:** cấu trúc dữ liệu, sắp xếp 
**Thời gian giải:** 1 phút 41 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mỗi tòa nhà chọc trời là một hình chữ nhật thẳng hàng nằm trên mặt đất. Một tòa nhà với các thông số`(h, l, r)`chiếm mọi điểm với`l ≤ x ≤ r`Và`0 ≤ y ≤ h`. 

Chúng ta phải xây dựng một đường đa tuyến bắt đầu trên mặt đất, chỉ di chuyển theo chiều ngang hoặc chiều dọc và bao quanh tất cả các tòa nhà cùng với mặt đất. Trong số tất cả các đường polyline như vậy, trước tiên chúng tôi giảm thiểu diện tích kèm theo. Nếu nhiều đường polyline có cùng diện tích tối thiểu, chúng ta sẽ chọn đường polyline có tổng chiều dài nhỏ nhất. 

Về mặt hình học, đây chính xác là đường chân trời phía trên của sự kết hợp các hình chữ nhật. Đường polyline leo lên bất cứ khi nào một tòa nhà cao hơn bắt đầu, giữ nguyên trong khi chiều cao nhìn thấy được không đổi và hạ xuống khi tòa nhà cao nhất hiện tại biến mất. 

Các ràng buộc ngay lập tức loại trừ bất kỳ cách tiếp cận bậc hai nào. Với`n ≤ 100000`, thậm chí`O(n^2)`với những thao tác đơn giản trở nên xung quanh`10^10`làm việc trong trường hợp xấu nhất, vượt xa thời hạn. Chúng ta cần một cái gì đó gần gũi`O(n log n)`. 

Các tòa nhà có thể chồng lên nhau theo những cách tùy ý. Một số trường hợp khó phát hiện nếu chúng ta cố gắng xây dựng đường chân trời một cách bất cẩn. 

Một trường hợp nguy hiểm là khi có nhiều sự kiện xảy ra ở cùng một tọa độ x. 

Ví dụ:```
3
5 0 2
3 2 4
7 2 5
```Tại`x = 2`, một tòa nhà kết thúc và hai tòa nhà khác bắt đầu. Đường chân trời phải nhảy trực tiếp từ độ cao`5`chiều cao`7`. Một quá trình quét đơn giản xử lý việc loại bỏ trước khi chèn vào có thể tạo ra một mức giảm trung gian không chính xác thành`0`hoặc`3`. 

Một trường hợp phức tạp khác là các tòa nhà lồng nhau. 

Ví dụ:```
3
10 0 10
3 2 4
5 6 8
```Các tòa nhà nhỏ hơn không bao giờ ảnh hưởng đến đường nét vì chúng hoàn toàn ẩn dưới tòa nhà cao hơn. Việc triển khai ngây thơ phát ra các phân đoạn một cách mù quáng cho mọi tòa nhà sẽ tạo ra các đỉnh dư thừa và không đáp ứng được yêu cầu về độ dài tối thiểu. 

Các tòa nhà rời rạc cũng rất quan trọng. 

Ví dụ:```
2
4 0 2
3 5 7
```Đường chân trời chính xác phải quay trở lại mặt đất giữa các tòa nhà:```
0 0
0 4
2 4
2 0
5 0
5 3
7 3
7 0
```Nếu chúng tôi vô tình hợp nhất các thành phần riêng biệt, chúng tôi sẽ bao gồm những vùng không cần thiết và vi phạm tiêu chí tối ưu hóa chính. 

Cuối cùng, chiều cao bằng nhau cần được chăm sóc. 

Ví dụ:```
2
5 0 2
5 2 4
```Câu trả lời đúng là một đoạn nằm ngang liên tục ở độ cao`5`, không phải hai cao nguyên riêng biệt có cạnh thẳng đứng không cần thiết tại`x = 2`. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là rời rạc hóa trục x. Chúng ta có thể thu thập tất cả các tọa độ riêng biệt, chia mặt phẳng thành các khoảng cơ bản và tính chiều cao tòa nhà tối đa trong mỗi khoảng. Khi đã biết cấu hình chiều cao, việc tạo đường đa tuyến thật dễ dàng. 

Điều này hoạt động về mặt khái niệm vì đa tuyến tối ưu được xác định duy nhất bởi chiều cao nhìn thấy tối đa ở mọi tọa độ x. Bất kỳ đường cong nào thấp hơn sẽ giao nhau với một tòa nhà và bất kỳ đường cong nào cao hơn sẽ làm tăng diện tích bao quanh. 

Vấn đề với vũ lực là kiểm tra phạm vi bảo hiểm. Giả sử chúng ta nén tọa độ thành`m`khoảng thời gian. Đối với mỗi khoảng thời gian, chúng tôi kiểm tra tất cả các tòa nhà để tìm chiều cao tối đa. Trong trường hợp xấu nhất,`m`là`2n`, vì vậy điều này trở thành`O(nm) = O(n^2)`. 

Với`100000`các tòa nhà, điều đó có nghĩa là đại khái`10^10`so sánh. 

Quan sát quan trọng là đường chân trời chỉ thay đổi ở ranh giới tòa nhà. Giữa hai tọa độ sự kiện liên tiếp, tập hợp các tòa nhà đang hoạt động không bao giờ thay đổi, do đó chiều cao nhìn thấy được ở đó không đổi. 

Điều này tự nhiên gợi ý một đường quét. Chúng tôi xử lý tất cả các sự kiện bắt đầu và kết thúc tòa nhà từ trái sang phải trong khi vẫn duy trì tòa nhà đang hoạt động cao nhất trong cấu trúc dữ liệu. 

Khi một tòa nhà bắt đầu xây dựng, chúng tôi sẽ chèn chiều cao của nó. 

Khi một tòa nhà kết thúc, chúng tôi loại bỏ chiều cao của nó. 

Sau khi xử lý sự kiện tại vị trí`x`, độ cao hoạt động tối đa sẽ xác định đường chân trời ngay bên phải của`x`. 

Vùng heap tối đa cho phép truy cập nhanh vào tòa nhà cao nhất hiện tại. Vì vùng heap của Python là vùng heap tối thiểu nên chúng tôi lưu trữ độ cao âm. Việc loại bỏ các độ cao tùy ý khỏi một đống là bất tiện, vì vậy chúng tôi sử dụng tính năng xóa lười bằng bản đồ bộ đếm. 

Các quá trình quét`2n`các sự kiện và chi phí cho mỗi lần chèn hoặc xóa`O(log n)`, cho độ phức tạp tổng thể`O(n log n)`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển mỗi tòa nhà thành hai sự kiện. 

Một tòa nhà`(h, l, r)`trở thành: 

- bắt đầu sự kiện`(l, type=0, h)`- kết thúc sự kiện`(r, type=1, h)`Chúng tôi xử lý các sự kiện bắt đầu trước các sự kiện kết thúc ở cùng tọa độ. Điều này tránh việc giảm chiều cao tạm thời khi một tòa nhà kết thúc chính xác ở nơi tòa nhà khác bắt đầu. 
2. Sắp xếp tất cả các sự kiện theo tọa độ x và loại sự kiện. 

Sau khi sắp xếp, quét từ trái sang phải đảm bảo chúng ta luôn biết tòa nhà nào hiện đang hoạt động. 
3. Duy trì mức độ cao hoạt động tối đa. 

Vì Python chỉ cung cấp vùng heap tối thiểu nên hãy lưu trữ độ cao âm. 

Ban đầu heap chứa`0`, đại diện cho mặt đất. 
4. Duy trì bản đồ tần số để xóa lười. 

Khi một tòa nhà kết thúc, chúng tôi giảm tần số của nó thay vì loại bỏ nó trực tiếp khỏi vùng nhớ heap. 

Trước khi đọc mức tối đa hiện tại, hãy bật liên tục các phần tử heap có tần số bằng 0. 
5. Xử lý tất cả các sự kiện có cùng tọa độ x với nhau. 

Điều này rất quan trọng. Đường chân trời không thể thay đổi nhiều lần ở cùng một tọa độ x. 

Đối với mọi sự kiện tại vị trí`x`: 

- thêm độ cao cho các sự kiện bắt đầu 
- loại bỏ độ cao đánh dấu cho các sự kiện kết thúc 
6. Sau khi xử lý tất cả các sự kiện tại vị trí`x`, làm sạch đống. 

Xóa các mục nhập cũ cho đến khi đỉnh tương ứng với chiều cao hiện hoạt. 
7. So sánh chiều cao tối đa mới với chiều cao đường chân trời trước đó. 

Nếu chiều cao thay đổi từ`prev_h`ĐẾN`cur_h`, đường chân trời phải rẽ vào`x`. 

Hai đỉnh được thêm vào: 

-`(x, prev_h)`-`(x, cur_h)`Điều này tạo ra phân khúc dọc cần thiết. 
8. Loại bỏ các bản sao liên tiếp dư thừa. 

Một số chuyển tiếp có thể tạo ra các điểm lặp lại hoặc các đỉnh thẳng hàng không cần thiết. Việc nén chúng đảm bảo polyline có độ dài tối thiểu. 
9. Tự động thêm điểm tiếp đất ban đầu và cuối cùng thông qua quá trình chuyển đổi quét. 

### Tại sao nó hoạt động 

Tại mỗi tọa độ x, heap chứa chính xác chiều cao của các tòa nhà bao phủ các vị trí ngay bên phải x. Giá trị tối đa trong số đó là chiều cao hợp lệ tối thiểu có thể có của đường bao ở đó, bởi vì bất kỳ giá trị nhỏ hơn nào cũng sẽ giao nhau với một tòa nhà. 

Đường chân trời chỉ thay đổi khi độ cao hoạt động tối đa thay đổi. Việc phát ra các đỉnh chính xác tại các điểm chuyển tiếp đó sẽ tạo ra đường bao có diện tích tối thiểu duy nhất. Bất kỳ đoạn dọc hoặc ngang bổ sung nào cũng sẽ tăng tổng chiều dài mà không thay đổi diện tích, do đó, đường đa giác được làm sạch cũng có độ dài tối thiểu trong số tất cả các giải pháp có diện tích tối thiểu. 

## Giải pháp Python```python
import sys
import heapq
from collections import defaultdict

input = sys.stdin.readline

def solve():
    n = int(input())

    events = []

    for _ in range(n):
        h, l, r = map(int, input().split())

        # start before end at same x
        events.append((l, 0, h))
        events.append((r, 1, h))

    events.sort()

    heap = [0]
    cnt = defaultdict(int)
    cnt[0] = 1

    ans = []

    prev_h = 0

    i = 0
    m = len(events)

    while i < m:
        x = events[i][0]

        while i < m and events[i][0] == x:
            _, typ, h = events[i]

            if typ == 0:
                cnt[h] += 1
                heapq.heappush(heap, -h)
            else:
                cnt[h] -= 1

            i += 1

        while heap and cnt[-heap[0]] == 0:
            heapq.heappop(heap)

        cur_h = -heap[0]

        if cur_h != prev_h:
            ans.append((x, prev_h))
            ans.append((x, cur_h))
            prev_h = cur_h

    # remove redundant consecutive duplicates / collinear points
    res = []

    for p in ans:
        if res and res[-1] == p:
            continue

        while len(res) >= 2:
            x1, y1 = res[-2]
            x2, y2 = res[-1]
            x3, y3 = p

            if (x1 == x2 == x3) or (y1 == y2 == y3):
                res.pop()
            else:
                break

        res.append(p)

    print(len(res))
    for x, y in res:
        print(x, y)

solve()
```Đường quét là cốt lõi của việc thực hiện. Mỗi tòa nhà đóng góp một sự kiện chèn và một sự kiện xóa. Sự kiện sắp xếp sẽ đưa ra thứ tự duyệt từ trái sang phải. 

Heap lưu trữ tất cả các độ cao dự kiến ​​hiện có thể xác định đường chân trời. Vì việc loại bỏ các phần tử tùy ý khỏi heap rất tốn kém nên mã sử dụng tính năng xóa lười. Độ cao vẫn còn trong đống cho đến khi chúng đạt đến đỉnh. Tại thời điểm đó, chúng tôi kiểm tra xem bản đồ tần số có cho biết chúng vẫn hoạt động hay không. 

Việc xử lý tất cả các sự kiện ở cùng tọa độ x sẽ tránh được các chuyển đổi trung gian không chính xác. Giả sử một chiều cao biến mất và một chiều cao khác cao hơn xuất hiện ở cùng tọa độ. Nếu chúng ta đưa ra các thay đổi tại một thời điểm, chúng ta có thể vô tình tạo ra các đường ngoằn ngoèo có độ rộng bằng 0. 

Vòng lặp dọn dẹp ở cuối sẽ loại bỏ các đỉnh thẳng hàng dư thừa. Không có nó, các cao nguyên có chiều cao bằng nhau hoặc các đoạn rẽ thẳng đứng lặp đi lặp lại có thể tạo ra các đường polyline không tối thiểu. 

Tất cả tọa độ và độ cao đều khớp an toàn với số nguyên Python, do đó không xuất hiện vấn đề tràn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
2
3 0 2
4 1 3
```Sự kiện được sắp xếp: 

| x | gõ | h | 
| --- | --- | --- | 
| 0 | bắt đầu | 3 | 
| 1 | bắt đầu | 4 | 
| 2 | kết thúc | 3 | 
| 3 | kết thúc | 4 | 

Quét dấu vết: 

| x | Chiều cao hoạt động | Tối đa hiện tại | Trước Max | Đã thêm đỉnh | 
| --- | --- | --- | --- | --- | 
| 0 | {3} | 3 | 0 | (0,0), (0,3) | 
| 1 | {3,4} | 4 | 3 | (1,3), (1,4) | 
| 2 | {4} | 4 | 4 | không | 
| 3 | {} | 0 | 4 | (3,4), (3,0) | 

Đầu ra cuối cùng:```
0 0
0 3
1 3
1 4
3 4
3 0
```Dấu vết này cho thấy các tòa nhà chồng lên nhau chỉ ảnh hưởng đến đường chân trời khi một tòa nhà cao hơn xuất hiện. 

### Ví dụ 2 

đầu vào:```
3
5 0 10
3 2 4
7 6 8
```Sự kiện được sắp xếp: 

| x | gõ | h | 
| --- | --- | --- | 
| 0 | bắt đầu | 5 | 
| 2 | bắt đầu | 3 | 
| 4 | kết thúc | 3 | 
| 6 | bắt đầu | 7 | 
| 8 | kết thúc | 7 | 
| 10 | kết thúc | 5 | 

Quét dấu vết: 

| x | Chiều cao hoạt động | Tối đa hiện tại | Trước Max | Đã thêm đỉnh | 
| --- | --- | --- | --- | --- | 
| 0 | {5} | 5 | 0 | (0,0), (0,5) | 
| 2 | {5,3} | 5 | 5 | không | 
| 4 | {5} | 5 | 5 | không | 
| 6 | {5,7} | 7 | 5 | (6,5), (6,7) | 
| 8 | {5} | 5 | 7 | (8,7), (8,5) | 
| 10 | {} | 0 | 5 | (10,5), (10,0) | 

Tòa nhà nhỏ hơn từ`2`ĐẾN`4`không bao giờ thay đổi đường chân trời vì nó luôn ẩn dưới độ cao`5`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Sắp xếp các sự kiện và hoạt động heap | 
| Không gian | O(n) | Sự kiện, vùng heap và bản đồ tần suất | 

Thuật toán thoải mái phù hợp với các giới hạn. Với`2n`các sự kiện và phép toán đống logarit, ngay cả trường hợp xấu nhất với`100000`tòa nhà hoạt động hiệu quả trong vòng 2 giây. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from collections import defaultdict
import heapq

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    out = io.StringIO()
    sys.stdout = out

    n = int(input())

    events = []

    for _ in range(n):
        h, l, r = map(int, input().split())
        events.append((l, 0, h))
        events.append((r, 1, h))

    events.sort()

    heap = [0]
    cnt = defaultdict(int)
    cnt[0] = 1

    ans = []

    prev_h = 0

    i = 0

    while i < len(events):
        x = events[i][0]

        while i < len(events) and events[i][0] == x:
            _, typ, h = events[i]

            if typ == 0:
                cnt[h] += 1
                heapq.heappush(heap, -h)
            else:
                cnt[h] -= 1

            i += 1

        while cnt[-heap[0]] == 0:
            heapq.heappop(heap)

        cur_h = -heap[0]

        if cur_h != prev_h:
            ans.append((x, prev_h))
            ans.append((x, cur_h))
            prev_h = cur_h

    res = []

    for p in ans:
        if res and res[-1] == p:
            continue

        while len(res) >= 2:
            x1, y1 = res[-2]
            x2, y2 = res[-1]
            x3, y3 = p

            if (x1 == x2 == x3) or (y1 == y2 == y3):
                res.pop()
            else:
                break

        res.append(p)

    print(len(res))
    for x, y in res:
        print(x, y)

    sys.stdout = sys.__stdout__

    return out.getvalue()

# provided sample
assert run(
"""2
3 0 2
4 1 3
"""
) == (
"""6
0 0
0 3
1 3
1 4
3 4
3 0
"""
), "sample 1"

# single building
assert run(
"""1
5 2 7
"""
) == (
"""4
2 0
2 5
7 5
7 0
"""
), "single rectangle"

# equal heights touching
assert run(
"""2
5 0 2
5 2 4
"""
) == (
"""4
0 0
0 5
4 5
4 0
"""
), "merge equal plateaus"

# nested buildings
assert run(
"""3
10 0 10
3 2 4
5 6 8
"""
) == (
"""4
0 0
0 10
10 10
10 0
"""
), "hidden buildings"

# disjoint buildings
assert run(
"""2
4 0 2
3 5 7
"""
) == (
"""8
0 0
0 4
2 4
2 0
5 0
5 3
7 3
7 0
"""
), "ground gap"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hình chữ nhật đơn | Một cao nguyên | Đầu vào kích thước tối thiểu | 
| Chạm vào độ cao bằng nhau | Phân khúc đã hợp nhất | Loại bỏ các cạnh dọc không cần thiết | 
| Tòa nhà lồng nhau | Chỉ có thể nhìn thấy cao nhất | Hình chữ nhật ẩn bị bỏ qua | 
| Tòa nhà rời rạc | Trở lại mặt đất | Xử lý đúng các khoảng trống | 

## Vỏ cạnh 

Hãy xem xét các tòa nhà có chung ranh giới:```
3
5 0 2
3 2 4
7 2 5
```Tại`x = 2`, chiều cao`5`biến mất khi đang ở độ cao`3`Và`7`xuất hiện. Thuật toán xử lý tất cả các sự kiện tại`x = 2`cùng nhau trước khi đọc mức tối đa mới. Heap chuyển tiếp trực tiếp từ`5`ĐẾN`7`, sản xuất:```
0 0
0 5
2 5
2 7
5 7
5 0
```Không xảy ra hiện tượng sụt giảm tạm thời. 

Bây giờ hãy xem xét các tòa nhà lồng nhau:```
3
10 0 10
3 2 4
5 6 8
```Số heap tối đa vẫn còn`10`trong suốt quá trình quét. Các tòa nhà nhỏ hơn vào và ra khỏi đống nhưng không bao giờ trở thành tối đa, vì vậy đường chân trời không bao giờ thay đổi bên trong`[0,10]`. 

Đối với các tòa nhà liền kề bằng nhau:```
2
5 0 2
5 2 4
```Khi tòa nhà đầu tiên kết thúc và tòa nhà thứ hai bắt đầu lúc`x = 2`, chiều cao tối đa trước và sau khi xử lý vẫn giữ nguyên`5`. Từ`cur_h == prev_h`, không có đỉnh mới nào được phát ra. Kết quả trở thành một đoạn ngang liên tục. 

Cuối cùng, các tòa nhà rời rạc:```
2
4 0 2
3 5 7
```Sau khi xử lý`x = 2`, mức tối đa của đống giảm xuống`0`, do đó đường chân trời trở lại mặt đất. Tại`x = 5`, nó lại tăng lên`3`. Điều này bảo toàn chính xác không gian trống giữa các tòa nhà thay vì bao bọc những khu vực không cần thiết.
