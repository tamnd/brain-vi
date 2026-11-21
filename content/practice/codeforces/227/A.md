---
title: "CF 227A - Tôi rẽ ở đâu?"
description: "Người anh hùng bắt đầu từ điểm A, đi đến điểm B, sau đó phải tiếp tục đi về phía điểm C. Tại điểm B, anh ta đang hướng mặt về hướng từ A đến B. Chúng ta cần xác định xem đến C cần rẽ trái, rẽ phải hay tiếp tục đi thẳng."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "geometry"]
categories: ["algorithms"]
codeforces_contest: 227
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 140 (Div. 2)"
rating: 1300
weight: 227
solve_time_s: 155
verified: true
draft: false
---

[CF 227A - Tôi phải rẽ ở đâu?](https://codeforces.com/problemset/problem/227/A) 

**Đánh giá:** 1300 
**Thẻ:** hình học 
**Thời gian giải:** 2m 35s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Người anh hùng bắt đầu từ điểm`A`, đi đến điểm`B`, và sau đó phải tiếp tục hướng tới điểm`C`. Tại điểm`B`, anh ấy đang hướng mặt về hướng từ`A`ĐẾN`B`. Chúng ta cần xác định xem liệu đạt được`C`yêu cầu rẽ trái, rẽ phải hoặc tiếp tục đi thẳng. 

Các ràng buộc hình học đơn giản hóa vấn đề rất nhiều. Các con đường được đảm bảo song song, thẳng hàng hoặc vuông góc. Điều đó có nghĩa là sự chuyển động từ`B`ĐẾN`C`hoặc là cùng hướng với`A -> B`, theo hướng hoàn toàn ngược lại hoặc quay chính xác 90 độ. Tuyên bố còn đảm bảo rằng cả ba điểm đều thẳng hàng hoặc là góc`ABC`là một góc vuông 

Tọa độ có thể lớn bằng`10^9`, cả tích cực và tiêu cực. Điều đó loại trừ bất kỳ cách tiếp cận nào liên quan đến lưới, mô phỏng hoặc hình học dấu phẩy động. Chúng ta chỉ cần một vài phép tính số học trên tọa độ, do đó`O(1)`giải pháp là mục tiêu tự nhiên. 

Một lỗi phổ biến là trộn lẫn hướng định hướng. Người anh hùng đứng ở`B`trong khi quay mặt ra xa`A`, do đó vectơ chỉ phương tương ứng là:```
AB = B - A
```và không`BA = A - B`. 

Ví dụ:```
A = (0, 0)
B = (0, 1)
C = (1, 1)
```Người anh hùng di chuyển lên từ`A`ĐẾN`B`. Để đạt được`C`, anh ta phải rẽ phải. Sử dụng vectơ đảo ngược sẽ tạo ra không chính xác`LEFT`. 

Một sai lầm dễ dàng khác xuất hiện trong trường hợp đường thẳng. 

Ví dụ:```
A = (0, 0)
B = (1, 0)
C = (2, 0)
```Câu trả lời đúng là:```
TOWARDS
```Việc triển khai bất cẩn chỉ kiểm tra dấu của tích chéo có thể phân loại sai điều này vì tích chéo bằng 0. Chúng ta phải xử lý rõ ràng trường hợp cộng tuyến một cách riêng biệt. 

Tọa độ âm cũng có thể làm lộ ra lỗi dấu. 

Ví dụ:```
A = (0, 0)
B = (-1, 0)
C = (-1, -1)
```Anh hùng di chuyển sang trái rồi đi xuống. Đó là rẽ trái chứ không phải rẽ phải. Trực giác trực quan trở nên khó khăn hơn với các trục âm, đó là lý do tại sao việc dựa vào công thức định hướng sẽ an toàn hơn so với các trường hợp vẽ trong đầu. 

## Phương pháp tiếp cận 

Cách mạnh mẽ để giải quyết vấn đề này là mô hình hóa rõ ràng hướng đi của anh hùng và so sánh nó với tất cả các nước đi hợp lệ có thể có. Vì các quy tắc chuyển động chỉ cho phép rẽ thẳng, rẽ trái hoặc rẽ phải nên chúng ta có thể xây dựng vectơ chỉ phương từ`A`ĐẾN`B`, xoay thủ công theo cả hai hướng và so sánh từng kết quả với vectơ từ`B`ĐẾN`C`. 

Ví dụ: nếu anh hùng di chuyển theo vectơ`(dx, dy)`, sau đó:```
Left rotation  = (-dy, dx)
Right rotation = (dy, -dx)
```Chúng ta có thể so sánh`BC`chống lại các vectơ biến đổi này và quyết định câu trả lời. Điều này hoạt động chính xác vì vấn đề chỉ đảm bảo ba khả năng này. 

Phương pháp vũ phu đã có thời gian không đổi, do đó không có vấn đề về hiệu suất ở đây. Tuy nhiên, nó vẫn dựa vào việc xử lý thủ công các phép biến đổi hình học và các trường hợp đặc biệt về cộng tuyến. 

Một quan sát rõ ràng hơn là đây chính xác là bài kiểm tra định hướng tiêu chuẩn từ hình học tính toán. Cho hai vectơ:```
u = AB
v = BC
```tích chéo 2D của họ cho chúng ta biết hướng rẽ:```
cross = ux * vy - uy * vx
```Nếu tích chéo là dương thì phép quay từ`u`ĐẾN`v`ngược chiều kim đồng hồ, nghĩa là rẽ trái. 

Nếu âm thì rẽ theo chiều kim đồng hồ tức là rẽ phải. 

Nếu nó bằng 0 thì các vectơ thẳng hàng nên anh hùng tiếp tục đi thẳng. 

Điều này làm giảm toàn bộ vấn đề thành một công thức số học duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force với thao tác xoay thủ công | O(1) | O(1) | Đã chấp nhận | 
| Kiểm tra định hướng chéo sản phẩm | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tọa độ điểm`A`,`B`, Và`C`. 
2. Xây dựng vectơ chỉ phương từ`A`ĐẾN`B`.```
AB = (xb - xa, yb - ya)
```Điều này thể hiện hướng mà người anh hùng hiện đang phải đối mặt. 

1. Xây dựng vectơ chỉ hướng từ`B`ĐẾN`C`.```
BC = (xc - xb, yc - yb)
```Điều này thể hiện hướng mà anh hùng muốn di chuyển tiếp theo. 

1. Tính tích chéo 2D:```
cross = AB.x * BC.y - AB.y * BC.x
```Dấu của giá trị này xác định hướng giữa hai vectơ. 

1. Nếu`cross > 0`, in`"LEFT"`. 

Tích chéo dương có nghĩa là phép quay từ`AB`ĐẾN`BC`là ngược chiều kim đồng hồ. 

1. Nếu`cross < 0`, in`"RIGHT"`. 

Tích chéo âm có nghĩa là phép quay theo chiều kim đồng hồ. 

1. Nếu không, hãy in`"TOWARDS"`. 

Tích chéo bằng 0 có nghĩa là cả hai vectơ đều nằm trên cùng một đường thẳng. 

### Tại sao nó hoạt động 

Sản phẩm chéo đo diện tích và hướng đã ký trong hình học 2D. Đối với vectơ`u`Và`v`, dấu hiệu của:```
u.x * v.y - u.y * v.x
```cho biết liệu quay từ`u`ĐẾN`v`đi ngược chiều kim đồng hồ hoặc theo chiều kim đồng hồ. 

Hướng quay hiện tại của anh hùng chính xác là vector`AB`và hướng tiếp theo mong muốn là`BC`. Vì bài toán chỉ đảm bảo rẽ trái, rẽ phải hoặc đi thẳng nên tích chéo hoàn toàn xác định được đáp án đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

xa, ya = map(int, input().split())
xb, yb = map(int, input().split())
xc, yc = map(int, input().split())

abx = xb - xa
aby = yb - ya

bcx = xc - xb
bcy = yc - yb

cross = abx * bcy - aby * bcx

if cross > 0:
    print("LEFT")
elif cross < 0:
    print("RIGHT")
else:
    print("TOWARDS")
```Phần đầu tiên của mã đọc ba điểm và chuyển đổi chúng thành các vectơ chỉ hướng. Sử dụng vectơ thay vì tọa độ tuyệt đối là bước hình học quan trọng vì các vòng quay phụ thuộc vào chuyển động tương đối chứ không phải vị trí. 

Tính toán tích chéo là trọng tâm của giải pháp:```
cross = abx * bcy - aby * bcx
```Thứ tự quan trọng. Hoán đổi các vectơ sẽ đảo ngược dấu và lật các câu trả lời sang trái/phải. 

Số nguyên Python tự động xử lý các giá trị rất lớn, do đó tọa độ gần`10^9`được an toàn. Trong các ngôn ngữ như C++, sử dụng`long long`sẽ là cần thiết vì sản phẩm có thể tiếp cận khoảng`10^18`. 

Điều kiện cuối cùng ánh xạ trực tiếp hướng tới đầu ra được yêu cầu. Điểm 0 phải được kiểm tra riêng vì các vectơ thẳng hàng không rẽ trái cũng không rẽ phải. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
0 0
0 1
1 1
```| Biến | Giá trị | 
| --- | --- | 
| AB | (0, 1) | 
| BC | (1, 0) | 
| chéo | -1 | 
| Kết quả | QUYỀN | 

Anh hùng di chuyển lên trên trước, sau đó quay về phía trục x dương. Vòng quay đó theo chiều kim đồng hồ nên đáp án là`"RIGHT"`. 

### Ví dụ 2 

đầu vào:```
0 0
1 0
2 0
```| Biến | Giá trị | 
| --- | --- | 
| AB | (1, 0) | 
| BC | (1, 0) | 
| chéo | 0 | 
| Kết quả | HƯỚNG TỚI | 

Cả hai vectơ đều chỉ cùng một hướng. Tích chéo trở thành 0, xác nhận rằng anh hùng tiếp tục tiến thẳng về phía trước. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một số phép tính số học được thực hiện | 
| Không gian | O(1) | Không sử dụng cấu trúc dữ liệu bổ sung | 

Lời giải dễ dàng nằm trong giới hạn vì nó thực hiện số học số nguyên theo thời gian không đổi bất kể kích thước tọa độ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def solve():
    input = sys.stdin.readline

    xa, ya = map(int, input().split())
    xb, yb = map(int, input().split())
    xc, yc = map(int, input().split())

    abx = xb - xa
    aby = yb - ya

    bcx = xc - xb
    bcy = yc - yb

    cross = abx * bcy - aby * bcx

    if cross > 0:
        print("LEFT")
    elif cross < 0:
        print("RIGHT")
    else:
        print("TOWARDS")

def run(inp: str) -> str:
    backup_stdin = sys.stdin
    backup_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    output = sys.stdout.getvalue()

    sys.stdin = backup_stdin
    sys.stdout = backup_stdout

    return output.strip()

# provided sample
assert run(
"""0 0
0 1
1 1
"""
) == "RIGHT", "sample 1"

# straight line
assert run(
"""0 0
1 0
2 0
"""
) == "TOWARDS", "straight movement"

# left turn with negative coordinates
assert run(
"""0 0
-1 0
-1 -1
"""
) == "LEFT", "negative coordinate orientation"

# right turn
assert run(
"""0 0
1 0
1 -1
"""
) == "RIGHT", "clockwise turn"

# large coordinates
assert run(
"""1000000000 1000000000
999999999 1000000000
999999999 999999999
"""
) == "LEFT", "large values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`(0,0) -> (1,0) -> (2,0)`|`TOWARDS`| Chuyển động cộng tuyến | 
|`(0,0) -> (-1,0) -> (-1,-1)`|`LEFT`| Định hướng đúng với tọa độ âm | 
|`(0,0) -> (1,0) -> (1,-1)`|`RIGHT`| Xoay theo chiều kim đồng hồ | 
| Lớn`10^9`tọa độ |`LEFT`| Không có vấn đề tràn | 

## Vỏ cạnh 

Xét trường hợp thẳng hàng:```
0 0
1 0
2 0
```Các vectơ là:```
AB = (1, 0)
BC = (1, 0)
```Sản phẩm chéo trở thành:```
1 * 0 - 0 * 1 = 0
```Thuật toán in`"TOWARDS"`. Nếu không kiểm tra rõ ràng giá trị 0, việc triển khai có thể phân loại sai giá trị này thành trái hoặc phải. 

Bây giờ hãy xem xét chuyển động ngang đảo ngược:```
0 0
-1 0
-1 -1
```Các vectơ là:```
AB = (-1, 0)
BC = (0, -1)
```Sản phẩm chéo là:```
(-1) * (-1) - 0 * 0 = 1
```Vì kết quả là dương nên thuật toán in`"LEFT"`. Trường hợp này bắt lỗi dấu hiệu do trực giác về tọa độ màn hình hoặc thứ tự vectơ đảo ngược. 

Cuối cùng, hãy xem xét tọa độ rất lớn:```
1000000000 1000000000
999999999 1000000000
999999999 999999999
```Các vectơ là:```
AB = (-1, 0)
BC = (0, -1)
```Tích chéo vẫn được tính toán chính xác bằng số học số nguyên. Python xử lý các giá trị này một cách an toàn và kết quả đầu ra của thuật toán`"LEFT"`mà không có bất kỳ vấn đề chính xác.
