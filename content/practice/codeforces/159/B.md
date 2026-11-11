---
title: "CF 159B - Bà mối"
description: "Chúng tôi có hai bộ sưu tập các đối tượng. Điểm đánh dấu được mô tả bằng (màu sắc, đường kính) và mũ cũng được mô tả bằng (màu sắc, đường kính). Chỉ có thể gắn nắp vào điểm đánh dấu nếu đường kính bằng nhau."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "*special", "greedy", "sortings"]
categories: ["algorithms"]
codeforces_contest: 159
codeforces_index: "B"
codeforces_contest_name: "VK Cup 2012 Qualification Round 2"
rating: 1100
weight: 159
solve_time_s: 121
verified: true
draft: false
---

[CF 159B - Người mai mối](https://codeforces.com/problemset/problem/159/B) 

**Đánh giá:** 1100 
**Tags:** *đặc biệt, tham lam, phân loại 
**Thời gian giải:** 2m 1s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có hai bộ sưu tập các đối tượng. Các dấu hiệu được mô tả bởi`(color, diameter)`và mũ cũng được mô tả bởi`(color, diameter)`. 

Chỉ có thể gắn nắp vào điểm đánh dấu nếu đường kính bằng nhau. Trong số tất cả các cặp hợp lệ, một số được coi là “đẹp” khi màu sắc cũng phù hợp. 

Mục tiêu tối ưu hóa có hai lớp. Đầu tiên, tối đa hóa tổng số cặp điểm đánh dấu phù hợp. Trong số tất cả các cách để đạt được mức tối đa này, hãy tối đa hóa số lượng trận đấu đẹp mắt. 

Mỗi điểm đánh dấu và nắp có thể được sử dụng nhiều nhất một lần. 

Các ràng buộc đủ lớn để chúng ta không thể thử trực tiếp mọi cặp đôi. Cả hai`n`Và`m`có thể đạt được`10^5`, vì vậy một`O(n * m)`giải pháp sẽ thực hiện lên đến`10^10`so sánh, vượt xa thời hạn. Chúng ta cần một cái gì đó gần tuyến tính hoặc`O(n log n)`. 

Quan sát quan trọng nhất là đường kính quyết định hoàn toàn khả năng tương thích. Một chiếc mũ có đường kính`5`không bao giờ có thể tương tác với điểm đánh dấu đường kính`7`. Điều này có nghĩa là mọi nhóm đường kính có thể được xử lý độc lập. 

Có một số trường hợp khó khăn có thể dễ dàng phá vỡ các giải pháp bất cẩn. 

Hãy xem xét đầu vào này:```
2 2
1 5
2 5
3 5
4 5
```Câu trả lời đúng là:```
2 0
```Mọi điểm đánh dấu vẫn có thể được đóng lại vì tất cả các đường kính đều khớp nhau, ngay cả khi không có màu nào khớp. Một chiến lược tham lam sai lầm chỉ tìm kiếm những trận đấu đẹp sẽ quay trở lại`0 0`. 

Bây giờ hãy xem xét:```
2 1
1 3
1 3
1 3
```Câu trả lời đúng là:```
1 1
```Chỉ có một nắp tồn tại nên chỉ có thể đóng một điểm đánh dấu. Việc quên rằng mỗi nắp chỉ sử dụng một lần sẽ tạo ra sai sót`2 2`. 

Một trường hợp tinh vi khác là khi tối đa hóa các trận đấu đẹp quá sớm sẽ làm giảm tổng số trận đấu.```
2 2
1 1
2 1
1 1
3 1
```Câu trả lời đúng là:```
2 1
```Nếu trước tiên chúng ta tham lam tiêu thụ thứ duy nhất`(1,1)`nắp trên điểm đánh dấu đầu tiên, điểm đánh dấu thứ hai vẫn có thể sử dụng`(3,1)`, vì vậy mọi thứ đều hoạt động. Nhưng trong các biến thể phức tạp hơn, việc sử dụng giới hạn mà không tôn trọng mục tiêu chính có thể làm giảm tổng số điểm đánh dấu đã đóng. Vấn đề nói rõ ràng rằng tổng số kết quả phù hợp quan trọng hơn kết quả phù hợp đẹp, vì vậy thuật toán phải duy trì mức độ ưu tiên đó. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Đối với mỗi điểm đánh dấu, hãy quét qua tất cả các nắp và chọn bất kỳ nắp nào chưa sử dụng có cùng đường kính. Nếu có thể, hãy chọn một chiếc mũ có màu sắc phù hợp. 

Cách tiếp cận này đúng về mặt logic vì nó cố gắng xây dựng các cặp hợp lệ một cách trực tiếp. Vấn đề có vẻ nhỏ về mặt khái niệm, nhưng những ràng buộc đã phá hủy giải pháp này. Với`10^5`đánh dấu và`10^5`giới hạn, chúng tôi có thể biểu diễn`10^10`kiểm tra tính tương thích. Ngay cả trong các ngôn ngữ được tối ưu hóa, tốc độ này vẫn quá chậm. 

Quan sát quan trọng là khả năng tương thích chỉ phụ thuộc vào đường kính. Khi chúng ta nhóm mọi thứ theo đường kính, các nhóm khác nhau sẽ trở nên hoàn toàn độc lập. 

Giả sử chúng ta tập trung vào một đường kính cố định`d`. Bên trong nhóm này:```
marker colors: frequencies
cap colors: frequencies
```Số lượng điểm đánh dấu tối đa chúng ta có thể đóng chỉ đơn giản là:```
min(total markers with diameter d,
    total caps with diameter d)
```Không có thông tin màu sắc nào quan trọng đối với mục tiêu đầu tiên này. 

Sau đó, trong số những cặp đôi tối đa có thể có đó, chúng tôi muốn có càng nhiều cặp đôi đẹp nhất có thể. Đối với một màu cụ thể`c`, số cặp đẹp có thể tạo được là:```
min(markers[d][c], caps[d][c])
```bởi vì mỗi trận đấu đẹp mắt sẽ tiêu tốn một điểm đánh dấu và một nắp có màu chính xác đó. 

Điều quan trọng là tối đa hóa các trận đấu đẹp mắt không bao giờ làm giảm tổng số trận đấu. Các cặp đẹp vốn là các cặp hợp lệ và mọi vật phẩm chưa khớp còn lại trong cùng một nhóm đường kính vẫn có thể được ghép nối tùy ý cho đến khi chúng ta đạt được số lượng tối đa có thể. 

Điều này làm giảm vấn đề đếm tần số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm) | O(m) | Quá chậm | 
| Tối ưu | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các điểm đánh dấu và nhóm chúng theo đường kính và màu sắc. 

Đối với mỗi điểm đánh dấu`(x, y)`, tăng:```
markers_by_diameter[y]
markers_by_color[(y, x)]
```Cấu trúc đầu tiên theo dõi số lượng điểm đánh dấu tồn tại cho một đường kính. Bài hát thứ hai chính xác`(diameter, color)`tần số. 
2. Đọc tất cả các chữ hoa và nhóm chúng theo cùng một cách. 

Đối với mỗi nắp`(a, b)`, tăng:```
caps_by_diameter[b]
caps_by_color[(b, a)]
```3. Tính số lượng điểm đánh dấu đóng tối đa. 

Với mỗi đường kính xuất hiện, hãy thêm:```
min(markers_by_diameter[d],
    caps_by_diameter[d])
```Đây là số lượng cặp lớn nhất có thể có đối với đường kính đó vì chỉ những đường kính bằng nhau mới có thể tương tác. 
4. Tính số trận đấu đẹp tối đa. 

Đối với mọi`(diameter, color)`cặp xuất hiện giữa các điểm đánh dấu, thêm:```
min(markers_by_color[(d, c)],
    caps_by_color[(d, c)])
```Mỗi cặp như vậy tương ứng với một cơ hội kết hợp đẹp mắt. 
5. In hai tổng số. 

### Tại sao nó hoạt động 

Thuật toán dựa trên thực tế là các nhóm đường kính là độc lập. Một nắp từ một đường kính không bao giờ có thể giúp ích cho một điểm đánh dấu từ đường kính khác. 

Bên trong một nhóm đường kính cố định, số lượng cặp hợp lệ tối đa hoàn toàn được xác định bởi cạnh nhỏ hơn. Nếu có 8 điểm đánh dấu và 5 nắp đường kính`d`, không có chiến lược nào có thể tạo nhiều hơn 5 cặp và bất kỳ cặp tùy ý nào cũng đạt được 5. 

Những trận đấu đẹp mắt là sự sàng lọc bên trong những cặp đôi đã hợp lệ đó. Với mỗi màu, chúng ta tham lam tạo ra càng nhiều cặp màu bằng nhau càng tốt. Điều này không thể làm giảm tổng số cặp vì dù sao thì mỗi cặp đẹp đều đã có một điểm đánh dấu và một nắp tương thích. Những món còn lại chưa ghép vẫn có thể ghép tùy ý theo đường kính. 

Vì cả hai mục tiêu đều được tối ưu hóa độc lập và nhất quán trong mỗi nhóm đường kính nên câu trả lời cuối cùng là tối ưu toàn cục. 

## Giải pháp Python```python
import sys
from collections import defaultdict

input = sys.stdin.readline

n, m = map(int, input().split())

markers_by_diameter = defaultdict(int)
caps_by_diameter = defaultdict(int)

markers_by_color = defaultdict(int)
caps_by_color = defaultdict(int)

for _ in range(n):
    x, y = map(int, input().split())
    markers_by_diameter[y] += 1
    markers_by_color[(y, x)] += 1

for _ in range(m):
    a, b = map(int, input().split())
    caps_by_diameter[b] += 1
    caps_by_color[(b, a)] += 1

closed = 0
beautiful = 0

all_diameters = set(markers_by_diameter.keys()) | set(caps_by_diameter.keys())

for d in all_diameters:
    closed += min(markers_by_diameter[d], caps_by_diameter[d])

for key, cnt in markers_by_color.items():
    beautiful += min(cnt, caps_by_color[key])

print(closed, beautiful)
```Phần đầu tiên của việc thực hiện xây dựng các bảng tần số. sử dụng`defaultdict(int)`tránh việc kiểm tra sự tồn tại lặp đi lặp lại và cập nhật liên tục. 

Mã tách việc đếm theo đường kính khỏi việc đếm chính xác`(diameter, color)`cặp. Điều này phản ánh hai mục tiêu tối ưu hóa trong vấn đề. Số lượng đường kính xác định số lượng tối đa của tổng số trận đấu, trong khi số lượng cặp chính xác xác định số lượng trận đấu đẹp tối đa. 

Sự kết hợp của các bộ đường kính được sử dụng khi tính tổng các điểm đánh dấu đã đóng. Điều này tránh thiếu đường kính chỉ xuất hiện ở một bên. 

Để có những kết quả phù hợp đẹp mắt, việc lặp qua các phím đánh dấu là đủ. Nếu một`(diameter, color)`cặp không tồn tại giữa các giới hạn, defaultdict sẽ tự động trả về 0. 

Không cần sắp xếp vì bài toán chỉ phụ thuộc vào tần số chứ không phụ thuộc vào thứ tự. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 4
1 2
3 4
2 4
5 4
2 4
1 1
1 2
```### Đếm đường kính 

| Đường kính | Điểm đánh dấu | Mũ | Đóng góp | 
| --- | --- | --- | --- | 
| 2 | 1 | 1 | 1 | 
| 4 | 2 | 2 | 2 | 

Tổng số điểm đóng =`1 + 2 = 3`### Số trận đấu đẹp mắt 

| Đường kính | Màu sắc | Số điểm đánh dấu | Số mũ | Cặp Đẹp | 
| --- | --- | --- | --- | --- | 
| 2 | 1 | 1 | 1 | 1 | 
| 4 | 3 | 1 | 0 | 0 | 
| 4 | 2 | 1 | 1 | 1 | 

Tổng số trận đấu đẹp =`2`Đầu ra:```
3 2
```Dấu vết này cho thấy rằng việc tối đa hóa các trận đấu đẹp được thực hiện độc lập trong mỗi nhóm đường kính. Màu sắc chưa từng có ở đường kính`4`vẫn tham gia trận đấu bình thường. 

### Ví dụ 2 

đầu vào:```
4 3
1 1
2 1
3 2
4 2
2 1
5 2
3 2
```### Đếm đường kính 

| Đường kính | Điểm đánh dấu | Mũ | Đóng góp | 
| --- | --- | --- | --- | 
| 1 | 2 | 1 | 1 | 
| 2 | 2 | 2 | 2 | 

Tổng số điểm đóng =`3`### Số trận đấu đẹp mắt 

| Đường kính | Màu sắc | Số điểm đánh dấu | Số mũ | Cặp Đẹp | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 1 | 0 | 0 | 
| 1 | 2 | 1 | 1 | 1 | 
| 2 | 3 | 1 | 1 | 1 | 
| 2 | 4 | 1 | 0 | 0 | 

Tổng số trận đấu đẹp =`2`Đầu ra:```
3 2
```Ví dụ này chứng tỏ rằng một số điểm đánh dấu chắc chắn không thể so sánh được vì không có đủ nắp có cùng đường kính. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi điểm đánh dấu và nắp được xử lý một lần | 
| Không gian | O(n + m) | Bảng tần số lưu trữ tất cả các nhóm riêng biệt | 

Giải pháp dễ dàng phù hợp trong giới hạn. Với nhiều nhất`2 * 10^5`tổng số đối tượng, xử lý tuyến tính đủ nhanh trong Python và bản đồ băm vẫn thoải mái trong giới hạn bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from collections import defaultdict

def solve():
    input = sys.stdin.readline

    n, m = map(int, input().split())

    markers_by_diameter = defaultdict(int)
    caps_by_diameter = defaultdict(int)

    markers_by_color = defaultdict(int)
    caps_by_color = defaultdict(int)

    for _ in range(n):
        x, y = map(int, input().split())
        markers_by_diameter[y] += 1
        markers_by_color[(y, x)] += 1

    for _ in range(m):
        a, b = map(int, input().split())
        caps_by_diameter[b] += 1
        caps_by_color[(b, a)] += 1

    closed = 0
    beautiful = 0

    all_diameters = set(markers_by_diameter) | set(caps_by_diameter)

    for d in all_diameters:
        closed += min(markers_by_diameter[d], caps_by_diameter[d])

    for key, cnt in markers_by_color.items():
        beautiful += min(cnt, caps_by_color[key])

    print(closed, beautiful)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    return sys.stdout.getvalue().strip()

# provided sample
assert run(
"""3 4
1 2
3 4
2 4
5 4
2 4
1 1
1 2
"""
) == "3 2", "sample 1"

# minimum size
assert run(
"""1 1
1 1
1 1
"""
) == "1 1", "minimum case"

# same diameter, different colors
assert run(
"""2 2
1 5
2 5
3 5
4 5
"""
) == "2 0", "all matches but no beautiful pairs"

# insufficient caps
assert run(
"""3 1
1 2
1 2
1 2
1 2
"""
) == "1 1", "single cap only"

# mixed groups
assert run(
"""4 3
1 1
2 1
3 2
4 2
2 1
5 2
3 2
"""
) == "3 2", "multiple diameter groups"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Điểm đánh dấu và nắp đơn có cùng thuộc tính |`1 1`| Đầu vào hợp lệ tối thiểu | 
| Tất cả các đường kính bằng nhau nhưng màu sắc khác nhau |`2 0`| Trận tổng và trận đẹp là mục tiêu riêng biệt | 
| Nhiều điểm đánh dấu nhưng một nắp |`1 1`| Mũ dùng một lần | 
| Nhiều nhóm đường kính |`3 2`| Xử lý độc lập theo đường kính | 

## Vỏ cạnh 

Hãy xem xét trường hợp màu sắc không bao giờ khớp nhưng đường kính thì có. 

đầu vào:```
2 2
1 5
2 5
3 5
4 5
```Thuật toán tính toán:```
diameter 5:
markers = 2
caps = 2
```Vì vậy, tổng số điểm đóng sẽ trở thành`2`. 

Để có những trận đấu đẹp:```
(5,1): min(1,0) = 0
(5,2): min(1,0) = 0
```Câu trả lời cuối cùng là:```
2 0
```Điều này khẳng định thuật toán không nhầm lẫn yêu cầu sự bình đẳng về màu sắc đối với các kết quả khớp thông thường. 

Bây giờ hãy xem xét các điểm đánh dấu giống hệt nhau lặp đi lặp lại với quá ít chữ hoa. 

đầu vào:```
3 1
1 2
1 2
1 2
1 2
```Thuật toán tính toán:```
diameter 2:
markers = 3
caps = 1
```Vì vậy, chỉ có thể có một trận đấu tổng thể. 

Để có những trận đấu đẹp:```
(2,1): min(3,1) = 1
```Câu trả lời trở thành:```
1 1
```Điều này xác minh rằng mỗi giới hạn được tiêu thụ nhiều nhất một lần. 

Cuối cùng, hãy xem xét các nhóm đường kính bị cô lập. 

đầu vào:```
2 2
1 1
2 2
1 2
2 3
```Đóng góp đường kính:```
diameter 1: min(1,0) = 0
diameter 2: min(1,1) = 1
diameter 3: min(0,1) = 0
```Những trận đấu đẹp mắt:```
(2,2): min(1,0) = 0
```Đầu ra:```
1 0
```Điều này chứng tỏ các nhóm đường kính khác nhau hoàn toàn độc lập và không thể hỗ trợ lẫn nhau.
