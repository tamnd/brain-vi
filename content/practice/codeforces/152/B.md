---
title: "CF 152B - Bậc thang"
description: "Chúng ta có một lưới hình chữ nhật có n hàng và m cột. Vasya bắt đầu ở vị trí (xc, yc). Sau đó anh ta xử lý k vectơ chuyển động từng cái một. Đối với một vectơ (dx, dy), anh ta di chuyển liên tục: Anh ta tiếp tục di chuyển theo hướng đó cho đến khi bước tiếp theo rời khỏi lưới."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "binary-search", "implementation"]
categories: ["algorithms"]
codeforces_contest: 152
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 108 (Div. 2)"
rating: 1300
weight: 152
solve_time_s: 102
verified: true
draft: false
---

[CF 152B - Các bước](https://codeforces.com/problemset/problem/152/B) 

**Đánh giá:** 1300 
**Tags:** tìm kiếm nhị phân, triển khai 
**Thời gian giải:** 1 phút 42s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Ta có một lưới hình chữ nhật có`n`hàng và`m`cột. Vasya xuất phát ở vị trí`(xc, yc)`. Sau đó anh ta xử lý`k`từng vectơ chuyển động. 

Đối với một vectơ`(dx, dy)`, anh ta liên tục di chuyển:```
(x, y) -> (x + dx, y + dy)
```Anh ta tiếp tục di chuyển theo hướng đó cho đến khi bước đi tiếp theo rời khỏi lưới. Sau khi hoàn thành một vectơ, anh ta bắt đầu vectơ tiếp theo từ vị trí hiện tại của mình. 

Nhiệm vụ là tính tổng số bước đi hợp lệ trên tất cả các vectơ. 

Kích thước lưới có thể lớn bằng`10^9`, điều này ngay lập tức loại trừ bất kỳ mô phỏng nào trên các ô của chính lưới. Một vectơ duy nhất cũng có thể di chuyển Vasya lên gần`10^9`nhiều lần nên việc mô phỏng từng bước là không thể. Mặc dù`k`chỉ là`10^4`, tổng số bước di chuyển được thực hiện có thể rất lớn. 

Quan sát cốt lõi là đối với mỗi vectơ, chúng ta thực sự không cần mô phỏng chuyển động. Chúng ta chỉ cần xác định số lần vectơ có thể được áp dụng trước khi một tọa độ rời khỏi phạm vi hợp lệ. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Giả sử chuyển động hoàn toàn theo chiều dọc. 

đầu vào:```
5 5
3 3
1
-2 0
```Câu trả lời đúng là:```
1
```Sau một bước di chuyển, Vasya đạt tới`(1, 3)`. Một động thái khác sẽ đạt được`(-1, 3)`, bên ngoài lưới. Một giải pháp bất cẩn khiến cả hai chia rẽ`dx`Và`dy`không xử lý các số 0 riêng biệt sẽ bị hỏng hoặc tạo ra rác. 

Hướng tiêu cực là một nguồn sai lầm phổ biến khác. 

đầu vào:```
5 5
4 4
1
-1 -2
```Câu trả lời đúng là:```
1
```Sau một bước di chuyển, Vasya đạt tới`(3, 2)`. Một động thái khác sẽ đạt được`(2, 0)`, điều này không hợp lệ vì cột`0`không tồn tại. Nếu các công thức giả định tất cả các hướng đều dương thì số bước được tính sẽ không chính xác. 

Cũng có trường hợp không thể di chuyển được gì cả. 

đầu vào:```
3 3
1 1
1
-1 0
```Câu trả lời đúng là:```
0
```Nước đi đầu tiên đã rời khỏi bàn cờ. Một số triển khai vô tình tạo ra số bước âm và thêm nó vào câu trả lời. 

Cuối cùng, bản thân câu trả lời có thể trở nên rất lớn. Với kích thước lên tới`10^9`và lên đến`10^4`vectơ, tổng số lần di chuyển có thể vượt quá phạm vi số nguyên 32 bit. Python xử lý việc này một cách tự động, nhưng các giải pháp C++ phải sử dụng`long long`. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là mô phỏng mọi động tác theo đúng nghĩa đen. Đối với mỗi vectơ`(dx, dy)`, chúng tôi liên tục kiểm tra xem`(x + dx, y + dy)`vẫn còn bên trong bảng. Nếu đúng như vậy, chúng tôi sẽ di chuyển đến đó và tăng câu trả lời. 

Cách tiếp cận bạo lực này là đúng vì nó tuân thủ chính xác các quy tắc của trò chơi. Vấn đề là thời gian chạy. Một vectơ duy nhất có thể tạo ra gần như`10^9`di chuyển. Với`10^4`vectơ, tổng công việc có thể đạt khoảng`10^13`hoạt động, vượt xa những gì phù hợp trong 2 giây. 

Quan sát quan trọng là chuyển động dọc theo một vectơ là hoàn toàn có thể dự đoán được. Nếu Vasya ở`(x, y)`và sử dụng vectơ`(dx, dy)`, sau đó`t`những bước đi anh ấy sẽ ở:```
(x + t * dx, y + t * dy)
```Chúng ta cần số nguyên không âm lớn nhất`t`sao cho cả hai tọa độ đều nằm trong lưới. 

Điều đó có nghĩa là:```
1 <= x + t * dx <= n
1 <= y + t * dy <= m
```Mỗi tọa độ áp đặt một cách độc lập một giới hạn trên cho`t`. Số bước hợp lệ tối đa chỉ đơn giản là mức tối thiểu của các giới hạn đó. 

Ví dụ, nếu`dx > 0`, sau đó:```
x + t * dx <= n
t <= (n - x) / dx
```Nếu như`dx < 0`, sau đó:```
x + t * dx >= 1
t <= (x - 1) / (-dx)
```Chúng tôi làm tương tự cho`dy`, sau đó lấy giá trị tối thiểu cho phép. 

Điều này làm giảm mỗi vectơ từ hàng tỷ bước di chuyển được mô phỏng thành một lượng số học không đổi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(tổng số bước) | O(1) | Quá chậm | 
| Tối ưu | O(k) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc kích thước lưới`n`Và`m`. 
2. Đọc vị trí bắt đầu`(x, y)`. 
3. Khởi tạo`answer = 0`. 
4. Xử lý từng vectơ`(dx, dy)`một cách độc lập. 
5. Tính toán tọa độ hàng được phép di chuyển bao nhiêu bước. 

Nếu như`dx > 0`, ranh giới giới hạn là cạnh dưới:```
(n - x) // dx
```Nếu như`dx < 0`, ranh giới giới hạn là cạnh trên:```
(x - 1) // (-dx)
```Nếu như`dx == 0`, chuyển động không bao giờ làm thay đổi hàng nên tọa độ hàng không bị hạn chế. 
6. Tính toán tọa độ cột cho phép bao nhiêu bước di chuyển bằng cách sử dụng cùng một logic. 
7. Số bước di chuyển thực tế của vectơ này là hạn chế tối thiểu từ cả hai tọa độ. 

Cả hai tọa độ phải đồng thời có hiệu lực, do đó giới hạn chặt chẽ hơn sẽ kiểm soát chuyển động. 
8. Thêm giá trị này vào`answer`. 
9. Cập nhật vị trí hiện tại:```
x += steps * dx
y += steps * dy
```10. Sau khi tất cả các vectơ được xử lý, hãy in`answer`. 

### Tại sao nó hoạt động 

Đối với một vectơ cố định`(dx, dy)`, mỗi lần di chuyển sẽ thay đổi vị trí một lượng như nhau. Sau đó`t`di chuyển, vị trí chính xác là:```
(x + t * dx, y + t * dy)
```Thuật toán tính giá trị lớn nhất`t`mà cả hai tọa độ đều nằm trong phạm vi hợp lệ. Bất kỳ giá trị nào lớn hơn sẽ vi phạm ít nhất một ranh giới, vì vậy không thể di chuyển thêm nữa. Bất kỳ giá trị nhỏ hơn rõ ràng là hợp lệ. 

Bởi vì mỗi vectơ được xử lý chính xác theo các bước di chuyển hợp lệ tối đa này nên thuật toán tái tạo các quy tắc của trò chơi một cách hoàn hảo. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m = map(int, input().split())
x, y = map(int, input().split())

k = int(input())

answer = 0

INF = 10**30

for _ in range(k):
    dx, dy = map(int, input().split())

    steps_x = INF
    steps_y = INF

    if dx > 0:
        steps_x = (n - x) // dx
    elif dx < 0:
        steps_x = (x - 1) // (-dx)

    if dy > 0:
        steps_y = (m - y) // dy
    elif dy < 0:
        steps_y = (y - 1) // (-dy)

    steps = min(steps_x, steps_y)

    answer += steps

    x += steps * dx
    y += steps * dy

print(answer)
```Việc thực hiện tuân theo đạo hàm toán học trực tiếp. 

Đối với mỗi vectơ, chúng tôi tính toán độc lập chuyển động hợp lệ tối đa dọc theo hàng và cột. giá trị`INF`đại diện cho chuyển động "không giới hạn" đối với tọa độ không bao giờ thay đổi. Ví dụ, nếu`dx == 0`, tọa độ hàng không bao giờ rời khỏi lưới, vì vậy chỉ có ràng buộc cột mới quan trọng. 

Các công thức khác nhau tùy thuộc vào dấu hiệu của hướng. Khi di chuyển xuống dưới với chiều dương`dx`, ranh giới dưới`n`trở thành yếu tố hạn chế. Khi di chuyển lên trên với âm`dx`, ranh giới trên cùng`1`trở thành yếu tố hạn chế. 

Bước cập nhật:```
x += steps * dx
y += steps * dy
```là quan trọng. Vectơ tiếp theo bắt đầu từ vị trí cuối cùng sau khi vectơ hiện tại kết thúc. 

Số nguyên Python tự động xử lý các câu trả lời rất lớn một cách an toàn. Trong các ngôn ngữ có số nguyên có chiều rộng cố định, câu trả lời phải sử dụng bộ nhớ 64 bit. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4 5
1 1
3
1 1
1 1
0 -2
```| Vectơ | Vị trí hiện tại | bước_x | bước_y | Các bước đã chọn | Vị Trí Mới | Tổng số câu trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| (1, 1) | (1, 1) | 3 | 4 | 3 | (4, 4) | 3 | 
| (1, 1) | (4, 4) | 0 | 1 | 0 | (4, 4) | 3 | 
| (0, -2) | (4, 4) | INF | 1 | 1 | (4, 2) | 4 | 

Vectơ đầu tiên di chuyển theo đường chéo cho đến khi ranh giới hàng chặn chuyển động tiếp theo. Vectơ thứ hai hoàn toàn không thể di chuyển vì hàng tiếp theo sẽ là`5`. Vectơ cuối cùng chỉ thay đổi các cột, vì vậy chỉ có giới hạn cột mới quan trọng. 

### Mẫu 2 

đầu vào:```
3 3
1 2
1
-1 0
```| Vectơ | Vị trí hiện tại | bước_x | bước_y | Các bước đã chọn | Vị Trí Mới | Tổng số câu trả lời | 
| --- | --- | --- | --- | --- | --- | --- | 
| (-1, 0) | (1, 2) | 0 | INF | 0 | (1, 2) | 0 | 

Giới hạn hàng ngay lập tức trở thành 0 vì di chuyển lên từ hàng`1`rời khỏi lưới. Thuật toán xử lý chính xác trường hợp không thể di chuyển được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(k) | Mỗi vectơ yêu cầu số học theo thời gian không đổi | 
| Không gian | O(1) | Chỉ có một vài biến được lưu trữ | 

Với nhiều nhất`10^4`vectơ, giải pháp này chỉ thực hiện một vài phép tính số học trên mỗi vectơ. Thời gian chạy dễ dàng phù hợp với giới hạn thời gian và mức sử dụng bộ nhớ là không đáng kể. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    n, m = map(int, input().split())
    x, y = map(int, input().split())

    k = int(input())

    ans = 0
    INF = 10**30

    for _ in range(k):
        dx, dy = map(int, input().split())

        sx = INF
        sy = INF

        if dx > 0:
            sx = (n - x) // dx
        elif dx < 0:
            sx = (x - 1) // (-dx)

        if dy > 0:
            sy = (m - y) // dy
        elif dy < 0:
            sy = (y - 1) // (-dy)

        steps = min(sx, sy)

        ans += steps

        x += steps * dx
        y += steps * dy

    print(ans)

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()

    backup = sys.stdout
    sys.stdout = out

    solve()

    sys.stdout = backup

    return out.getvalue().strip()

# provided sample
assert run(
"""4 5
1 1
3
1 1
1 1
0 -2
"""
) == "4", "sample 1"

# no movement possible
assert run(
"""3 3
1 1
1
-1 0
"""
) == "0", "blocked immediately"

# pure horizontal movement
assert run(
"""5 10
3 2
1
0 3
"""
) == "2", "horizontal only"

# negative diagonal movement
assert run(
"""5 5
4 4
1
-1 -2
"""
) == "1", "negative direction"

# huge values
assert run(
"""1000000000 1000000000
1 1
1
1 1
"""
) == "999999999", "large coordinates"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3x3`, di chuyển`(-1,0)`từ`(1,1)`|`0`| Va chạm ranh giới ngay lập tức | 
| Chuyển động chỉ theo chiều ngang |`2`| Xử lý đúng`dx = 0`| 
| Chuyển động chéo âm |`1`| Công thức đúng cho chiều âm | 
| Tọa độ tối đa |`999999999`| Số học lớn và an toàn 64-bit | 

## Vỏ cạnh 

Hãy xem xét chuyển động trong đó một tọa độ không bao giờ thay đổi. 

đầu vào:```
5 10
3 2
1
0 3
```Bộ thuật toán`steps_x = INF`bởi vì`dx = 0`. Đối với chuyển động của cột:```
(10 - 2) // 3 = 2
```Vậy câu trả lời cuối cùng là`2`. Các động thái là:```
(3,2) -> (3,5) -> (3,8)
```Một động thái khác sẽ đến cột`11`, bên ngoài lưới. 

Bây giờ hãy xem xét chuyển động tiêu cực. 

đầu vào:```
5 5
4 4
1
-1 -2
```Đối với hàng:```
(4 - 1) // 1 = 3
```Đối với cột:```
(4 - 1) // 2 = 1
```Tối thiểu là`1`, vì vậy chỉ có thể thực hiện một động tác. Sau khi chuyển đến`(3,2)`, một động thái khác sẽ cố gắng`(2,0)`, không hợp lệ. 

Cuối cùng, hãy xem xét một nước đi bị chặn hoàn toàn. 

đầu vào:```
3 3
1 1
1
-1 0
```Đối với hàng:```
(1 - 1) // 1 = 0
```Đối với cột, chuyển động không bị hạn chế vì`dy = 0`. 

Mức tối thiểu trở thành`0`, do đó thuật toán không thực hiện chuyển động nào và giữ nguyên câu trả lời.
