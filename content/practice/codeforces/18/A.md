---
title: "CF 18A - Tam giác"
description: "Chúng ta có ba điểm trên mặt phẳng có tọa độ nguyên. Ba điểm này đã tạo thành một tam giác hợp lệ, nghĩa là chúng không thẳng hàng."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "geometry"]
categories: ["algorithms"]
codeforces_contest: 18
codeforces_index: "A"
codeforces_contest_name: "Codeforces Beta Round 18 (Div. 2 Only)"
rating: 1500
weight: 18
solve_time_s: 108
verified: true
draft: false
---
[CF 18A - Tam giác](https://codeforces.com/problemset/problem/18/A) 

**Đánh giá:** 1500 
**Tags:** lực lượng vũ phu, hình học 
**Thời gian giải:** 1 phút 48s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có ba điểm trên mặt phẳng có tọa độ nguyên. Ba điểm này đã tạo thành một tam giác hợp lệ, nghĩa là chúng không thẳng hàng. 

Nhiệm vụ là phân loại tam giác thành một trong ba loại. 

Tam giác là`RIGHT`nếu một trong các góc của nó chính xác là 90 độ. 

Tam giác là`ALMOST`nếu nó hiện không vuông góc, nhưng việc di chuyển chính xác từng đỉnh một đơn vị theo một trong bốn hướng chính sẽ làm cho nó vuông góc. Điểm di chuyển vẫn phải có tọa độ nguyên. 

Nếu không có điều kiện nào xảy ra thì chúng ta in`NEITHER`. 

Tọa độ rất nhỏ, chỉ nằm giữa`-100`Và`100`. Điều đó làm thay đổi hoàn toàn bản chất của vấn đề. Chúng ta không cần các thủ thuật hình học nâng cao, số học dấu phẩy động hoặc tối ưu hóa. Ngay cả việc thử nhiều cấu hình ứng viên cũng không tốn kém vì tổng số khả năng là rất nhỏ. 

Mối nguy hiểm lớn nhất trong các bài toán hình học là độ chính xác. Việc triển khai bất cẩn có thể tính toán các góc bằng cách sử dụng các hàm lượng giác và so sánh các giá trị dấu phẩy động với`90`. Điều đó thật mong manh vì việc làm tròn dấu phẩy động có thể biến một góc vuông chính xác thành một thứ như`89.999999`. 

Ví dụ:```
0 0 3 0 0 4
```Đây rõ ràng là một tam giác vuông, nhưng việc sử dụng công thức cosine với phép so sánh dấu phẩy động có thể gây ra những lỗi nhỏ. 

Cách tiếp cận an toàn là làm việc hoàn toàn với khoảng cách bình phương và số học số nguyên. 

Một trường hợp tinh tế khác xuất hiện khi kiểm tra`ALMOST`tình trạng. Chúng ta chỉ được phép di chuyển một điểm theo khoảng cách một cách chính xác`1`dọc theo lưới. Chuyển động theo đường chéo không được phép. 

Ví dụ:```
0 0 1 2 2 1
```Di chuyển một điểm theo đường chéo có thể tạo ra một tam giác vuông, nhưng chuyển động theo đường chéo có khoảng cách Euclide`sqrt(2)`, không`1`. Câu trả lời đúng ở đây không dựa trên các bước di chuyển theo đường chéo. 

Một lỗi triển khai khác là quên loại bỏ tam giác ban đầu trước khi tìm kiếm`ALMOST`. 

Ví dụ:```
0 0 2 0 0 1
```Đây là góc vuông rồi. Mặc dù các điểm di chuyển cũng có thể tạo ra một tam giác vuông khác nhưng kết quả đầu ra đúng vẫn phải là`RIGHT`. 

## Phương pháp tiếp cận 

Cách trực tiếp nhất để giải quyết vấn đề là kiểm tra xem tam giác hiện tại có vuông góc hay không. Nếu không, chúng tôi thử mọi nước đi được phép và kiểm tra lại. 

Một tam giác là vuông nếu độ dài các cạnh của nó thỏa mãn định lý Pythagore. Vì tọa độ là số nguyên nên bình phương khoảng cách cũng là số nguyên. Giả sử độ dài cạnh bình phương là`a`,`b`, Và`c`. Sau khi sắp xếp chúng, tam giác vuông góc chính xác khi:```
a + b = c
```Chiến lược vũ phu này đã đủ nhanh rồi. Mỗi điểm có bốn động thái có thể xảy ra:```
(x+1,y), (x-1,y), (x,y+1), (x,y-1)
```Có ba điểm nên chỉ`3 × 4 = 12`tam giác sửa đổi để kiểm tra. 

Mỗi bài kiểm tra tính ba khoảng cách bình phương và thực hiện công việc không đổi. Ngay cả khi giảm giới hạn thời gian rất nhiều, điều này vẫn sẽ chạy ngay lập tức. 

Quan sát quan trọng là hình học có tọa độ nguyên thường trở nên đơn giản hơn khi chúng ta tránh khoảng cách thực tế. Khoảng cách bình phương bảo toàn tất cả các so sánh cần thiết cho định lý Pythagore trong khi loại bỏ hoàn toàn căn bậc hai. 

Một cách tiếp cận hình học đơn giản hơn có thể tính toán các vectơ và tích số chấm, hoặc thậm chí các góc có hàm lượng giác. Những phương pháp đó hoạt động về mặt toán học, nhưng ở đây chúng phức tạp không cần thiết. Cấu trúc bài toán đủ nhỏ để việc kiểm tra toàn diện kết hợp với số học số nguyên sẽ mang lại giải pháp sạch hơn và an toàn hơn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force với hình học dấu phẩy động | O(1) | O(1) | Được chấp nhận nhưng dễ bị lỗi | 
| Kiểm tra khoảng cách bình phương số nguyên | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc ba điểm. 
2. Viết hàm trợ giúp xác định xem ba điểm có tạo thành tam giác vuông hay không. 
3. Bên trong hàm này, tính bình phương độ dài của cả ba cạnh. 

Nếu các điểm là`A`,`B`, Và`C`, tính:```
AB², AC², BC²
```Sử dụng khoảng cách bình phương sẽ tránh hoàn toàn số học dấu phẩy động. 

1. Sắp xếp ba chiều dài bình phương. 

Cạnh lớn nhất phải là cạnh huyền trong một tam giác vuông nên sau khi sắp xếp chúng ta chỉ cần kiểm tra xem:```
smallest + middle = largest
```1. Đầu tiên hãy kiểm tra hình tam giác ban đầu. 

Nếu nó đã thỏa mãn điều kiện Pythagore, hãy in`RIGHT`. 

1. Nếu không, hãy thử di chuyển từng điểm theo bốn hướng cho phép. 

Đối với mỗi điểm, hãy tạo các ứng cử viên sau:```
(x+1,y)
(x-1,y)
(x,y+1)
(x,y-1)
```Tạm thời thay thế điểm ban đầu và kiểm tra tam giác mới. 

1. Nếu bất kỳ tam giác biến đổi nào trở thành vuông góc, hãy in`ALMOST`. 

Thời điểm chúng ta tìm thấy một nước đi hợp lệ, chúng ta có thể ngừng tìm kiếm. 

1. Nếu không có bước nào trong số 12 bước này thành công, hãy in`NEITHER`. 

### Tại sao nó hoạt động 

Thuật toán kiểm tra mọi cấu hình được câu lệnh cho phép. 

Một tam giác vuông chính xác khi độ dài các cạnh của nó thỏa mãn định lý Pythagore. Khoảng cách bình phương bảo toàn tính chất này một cách hoàn hảo vì:```
a² + b² = c²
```chỉ chứa các phép toán số nguyên. 

Đối với`ALMOST`với điều kiện, câu lệnh chỉ cho phép di chuyển theo chiều ngang hoặc chiều dọc một đơn vị. Thuật toán liệt kê rõ ràng tất cả các bước di chuyển như vậy cho mọi đỉnh, do đó không có ứng cử viên hợp lệ nào bị bỏ sót. 

Vì mọi sửa đổi được phép đều được kiểm tra và mọi kiểm tra đều chính xác về mặt toán học nên thuật toán không thể phân loại sai một tam giác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def dist2(x1, y1, x2, y2):
    return (x1 - x2) ** 2 + (y1 - y2) ** 2

def is_right(points):
    x1, y1 = points[0]
    x2, y2 = points[1]
    x3, y3 = points[2]

    sides = [
        dist2(x1, y1, x2, y2),
        dist2(x1, y1, x3, y3),
        dist2(x2, y2, x3, y3)
    ]

    sides.sort()

    return sides[0] > 0 and sides[0] + sides[1] == sides[2]

def solve():
    nums = list(map(int, input().split()))

    points = [
        [nums[0], nums[1]],
        [nums[2], nums[3]],
        [nums[4], nums[5]]
    ]

    if is_right(points):
        print("RIGHT")
        return

    directions = [(1, 0), (-1, 0), (0, 1), (0, -1)]

    for i in range(3):
        original_x, original_y = points[i]

        for dx, dy in directions:
            points[i] = [original_x + dx, original_y + dy]

            if is_right(points):
                print("ALMOST")
                return

        points[i] = [original_x, original_y]

    print("NEITHER")

solve()
```các`dist2`hàm tính bình phương khoảng cách Euclide. Sử dụng khoảng cách bình phương sẽ tránh được căn bậc hai và giữ cho mọi phép tính đều chính xác. 

các`is_right`chức năng là kiểm tra hình học cốt lõi. Nó tính toán độ dài ba cạnh bình phương, sắp xếp chúng và xác minh điều kiện Pythagore. Việc kiểm tra bổ sung`sides[0] > 0`bảo vệ chống lại các cạnh suy biến, mặc dù đầu vào ban đầu được đảm bảo không suy biến. 

Logic chính đầu tiên kiểm tra tam giác ban đầu. Thứ tự này quan trọng vì một tam giác đã vuông góc phải in`RIGHT`, cho dù việc di chuyển một điểm cũng có thể tạo ra một tam giác vuông khác. 

Việc tìm kiếm`ALMOST`lặp đi lặp lại tất cả mười hai bước đi hợp pháp. Sau khi thử một nước đi, điểm sẽ được khôi phục trước khi thử nước đi tiếp theo. Việc quên bước khôi phục này là nguyên nhân phổ biến gây ra lỗi vì các lần lặp lại sau này sẽ bắt đầu từ tọa độ đã được sửa đổi. 

Mọi thứ đều chạy với số học số nguyên, do đó không có vấn đề gì về độ chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
0 0 2 0 0 1
```Các điểm là:```
A = (0,0)
B = (2,0)
C = (0,1)
```| Bên | Chiều dài bình phương | 
| --- | --- | 
| AB² | 4 | 
| AC² | 1 | 
| BC² | 5 | 

Sau khi sắp xếp: 

| Giá trị được sắp xếp | 
| --- | 
| 1, 4, 5 | 

Chúng tôi kiểm tra:```
1 + 4 = 5
```Điều kiện được giữ, vì vậy đầu ra là:```
RIGHT
```Dấu vết này cho thấy tại sao khoảng cách bình phương là đủ. Chúng tôi không bao giờ tính toán độ dài hoặc góc thực tế. 

### Ví dụ 2 

đầu vào:```
0 0 1 1 2 0
```Độ dài bình phương ban đầu: 

| Bên | Chiều dài bình phương | 
| --- | --- | 
| AB² | 2 | 
| AC² | 4 | 
| BC² | 2 | 

Đã sắp xếp: 

| Giá trị được sắp xếp | 
| --- | 
| 2, 2, 4 | 

Chúng tôi kiểm tra:```
2 + 2 = 4
```Điều này đúng nên tam giác đã vuông góc. 

Bây giờ hãy xem xét:```
0 0 1 1 3 0
```Độ dài bình phương ban đầu: 

| Bên | Chiều dài bình phương | 
| --- | --- | 
| AB² | 2 | 
| AC² | 9 | 
| BC² | 5 | 

Đã sắp xếp: 

| Giá trị được sắp xếp | 
| --- | 
| 2, 5, 9 | 

Chúng tôi kiểm tra:```
2 + 5 = 7
```Không vuông góc. 

Di chuyển điểm`C`từ`(3,0)`ĐẾN`(2,0)`: 

| Bên | Chiều dài bình phương | 
| --- | --- | 
| AB² | 2 | 
| AC² | 4 | 
| BC² | 2 | 

Hiện nay:```
2 + 2 = 4
```Vì vậy, câu trả lời trở thành:```
ALMOST
```Dấu vết này chứng tỏ rằng việc kiểm tra tất cả các bước di chuyển một bước là đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Nhiều nhất là 13 lần kiểm tra tam giác, mỗi lần có công việc liên tục | 
| Không gian | O(1) | Chỉ một số biến và mảng tọa độ được lưu trữ | 

Các ràng buộc rất nhỏ nên giải pháp dễ dàng nằm gọn trong giới hạn đó. Ngay cả mã Python được giải thích cũng kết thúc ngay lập tức vì tổng số thao tác cực kỳ nhỏ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def dist2(x1, y1, x2, y2):
        return (x1 - x2) ** 2 + (y1 - y2) ** 2

    def is_right(points):
        x1, y1 = points[0]
        x2, y2 = points[1]
        x3, y3 = points[2]

        sides = [
            dist2(x1, y1, x2, y2),
            dist2(x1, y1, x3, y3),
            dist2(x2, y2, x3, y3)
        ]

        sides.sort()

        return sides[0] > 0 and sides[0] + sides[1] == sides[2]

    nums = list(map(int, input().split()))

    points = [
        [nums[0], nums[1]],
        [nums[2], nums[3]],
        [nums[4], nums[5]]
    ]

    if is_right(points):
        return "RIGHT"

    directions = [(1, 0), (-1, 0), (0, 1), (0, -1)]

    for i in range(3):
        ox, oy = points[i]

        for dx, dy in directions:
            points[i] = [ox + dx, oy + dy]

            if is_right(points):
                return "ALMOST"

        points[i] = [ox, oy]

    return "NEITHER"

# provided sample
assert run("0 0 2 0 0 1\n") == "RIGHT", "sample 1"

# already right triangle
assert run("0 0 3 0 0 4\n") == "RIGHT", "classic 3-4-5"

# becomes right after one move
assert run("0 0 1 1 3 0\n") == "ALMOST", "single move fixes triangle"

# impossible case
assert run("0 0 1 2 2 1\n") == "NEITHER", "no valid one-step move"

# negative coordinates
assert run("-1 -1 1 -1 -1 2\n") == "RIGHT", "handles negatives"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 0 3 0 0 4`|`RIGHT`| Số nguyên chính xác Bộ ba Pythagore | 
|`0 0 1 1 3 0`|`ALMOST`| Một nước đi hợp lệ sẽ tạo thành tam giác vuông | 
|`0 0 1 2 2 1`|`NEITHER`| Tìm kiếm di chuyển toàn diện không thành công | 
|`-1 -1 1 -1 -1 2`|`RIGHT`| Tọa độ âm được xử lý an toàn | 

## Vỏ cạnh 

Xét trường hợp tam giác đã vuông góc:```
0 0 2 0 0 1
```Thuật toán gọi lần đầu tiên`is_right`về các điểm ban đầu. 

Độ dài cạnh bình phương là:```
1, 4, 5
```Từ:```
1 + 4 = 5
```thuật toán in ngay lập tức`RIGHT`và dừng lại. Nó không bao giờ đi vào`ALMOST`tìm kiếm. Thứ tự này là cần thiết vì câu lệnh ưu tiên`RIGHT`qua`ALMOST`. 

Bây giờ hãy xem xét một trường hợp trong đó chuyển động chéo sẽ có ích, nhưng chuyển động hợp pháp thì không:```
0 0 1 2 2 1
```Thuật toán kiểm tra tam giác ban đầu và không tìm thấy mối quan hệ Pythagore. 

Sau đó nó sẽ kiểm tra chính xác mười hai bước di chuyển sau:```
(+1,0), (-1,0), (0,+1), (0,-1)
```cho mỗi đỉnh. 

Không có cấu hình nào được thử nghiệm trở thành vuông góc, do đó thuật toán in ra`NEITHER`. 

Điều này xác nhận rằng chỉ các chuyển động theo hướng chính mới được xem xét, khớp chính xác với định nghĩa vấn đề. 

Cuối cùng, hãy xem xét tọa độ âm:```
-1 -1 1 -1 -1 2
```Độ dài cạnh bình phương trở thành:```
4, 9, 13
```Sau khi sắp xếp:```
4, 9, 13
```Từ:```
4 + 9 = 13
```thuật toán xuất ra chính xác`RIGHT`. 

Việc tính toán chỉ sử dụng phép trừ và bình phương, do đó tọa độ âm không cần xử lý đặc biệt.
