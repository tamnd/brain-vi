---
title: "CF 102279D - Thược Dược Nhà Vô Địch"
description: "Dahlia đứng ở một điểm cố định ((x0,y0)) và có thể chọn bất kỳ hướng nào để phát huy khả năng của mình. Một mục tiêu có thể bị kéo nếu nó ở khoảng cách tối đa (R), nhưng có thêm một hạn chế: dọc theo hướng đã chọn, Dahlia tiếp cận mục tiêu đầu tiên trên tia đó."
date: "2026-08-16T19:12:09+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102279
codeforces_index: "D"
codeforces_contest_name: "HCW 19 Team Round (ICPC format)"
rating: 0
weight: 102279
solve_time_s: 71
verified: true
draft: false
---

[CF 102279D - Nhà vô địch Dahlia](https://codeforces.com/problemset/problem/102279/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 11 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Dahlia đứng ở một điểm cố định ((x_0,y_0)) và có thể chọn bất kỳ hướng nào để kích hoạt khả năng của mình. Một mục tiêu có thể bị kéo nếu nó ở khoảng cách tối đa (R), nhưng có thêm một hạn chế: dọc theo hướng đã chọn, Dahlia tiếp cận mục tiêu đầu tiên trên tia đó. Nếu một mục tiêu khác nằm giữa Dahlia và mục tiêu đã chọn, mục tiêu xa hơn không thể được chọn trực tiếp. 

Do đó, nhiệm vụ không phải là đếm mọi mục tiêu bên trong vòng tròn bán kính (R). Các mục tiêu nằm trên cùng một tia sẽ cạnh tranh với nhau và chỉ mục tiêu gần nhất trên tia đó mới có thể bị kéo. Chúng ta cần đếm xem có bao nhiêu tia khác nhau từ cây thược dược chứa ít nhất một mục tiêu trong khoảng cách (R). 

Có thể có tới (5\cdot10^5) mục tiêu. Tọa độ và (R) có thể lớn bằng (10^9), do đó, thuật toán so sánh mọi cặp mục tiêu sẽ thực hiện khoảng (2,5\cdot10^{11}) so sánh trong trường hợp xấu nhất. Điều đó vượt xa những gì giới hạn lập trình cạnh tranh 1,5 giây có thể hỗ trợ. Chúng ta cần một cách tiếp cận gần với (O(N\log N)), hoặc tốt hơn là (O(N)) ngoài chi phí logarit nhỏ của các phép tính gcd số nguyên. 

Tọa độ có thể âm nên các hướng phải bảo toàn dấu của chúng. Các tia ((1,2)) và ((-1,-2)) có hướng khác nhau mặc dù chúng nằm trên cùng một đường vô hạn. Đồng thời, ((1,2)), ((2,4)) và ((-1,-2)) đại diện cho hai tia khác nhau chứ không phải ba. 

Ngoài ra còn có vấn đề về ranh giới ở khoảng cách chính xác (R). Mục tiêu có khoảng cách bình phương chính xác (R^2) có thể tiếp cận được và phải được tính. Ví dụ, với```
0 0 5 1
3 4
```câu trả lời là`1`, vì khoảng cách chính xác là (5). Một triển khai sử dụng`< R * R`thay vì`<= R * R`sẽ quay lại không chính xác`0`. 

Trường hợp cạnh thứ hai xảy ra khi một số mục tiêu nằm trên cùng một tia. Ví dụ,```
0 0 10 3
1 0
2 0
3 0
```có câu trả lời`1`, không`3`. Cả ba mục tiêu đều nằm trong tầm bắn, nhưng bắn về phía đông luôn trúng đích`(1,0)`Đầu tiên. Đếm mọi mục tiêu bên trong vòng tròn mà không hợp nhất các hướng sẽ là sai. 

Trường hợp ngược chiều cũng dễ xử lý sai. Vì```
0 0 10 2
3 0
-3 0
```câu trả lời là`2`. Hai mục tiêu nằm trên các tia đối diện nên Dahlia có thể chọn một trong hai. Chuẩn hóa các hướng bằng ước số chung lớn nhất của chúng sẽ bảo toàn các dấu hiệu và giữ cho hai hướng này khác biệt. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là kiểm tra mọi mục tiêu bên trong bán kính và xác định xem có mục tiêu nào gần hơn trên cùng một tia hay không. Đối với mỗi mục tiêu, chúng tôi có thể so sánh nó với mọi mục tiêu khác và kiểm tra xem hai vectơ từ Dahlia có phải là bội số dương của nhau hay không và liệu mục tiêu kia có gần hơn hay không. Điều này đúng vì mục tiêu có thể được chọn chính xác khi không có mục tiêu nào khác chặn mục tiêu đó trên tia của nó. 

Vấn đề là số bậc hai của so sánh. Với (N=5\cdot10^5), trường hợp xấu nhất chứa khoảng (N(N-1)/2) hoặc khoảng (1,25\cdot10^{11}), các cặp. Ngay cả một bài kiểm tra thời gian liên tục rất rẻ cũng không thể làm được điều đó. 

Quan sát quan trọng là chúng ta không thực sự cần tìm mục tiêu gần nhất trên mỗi tia. Giả sử một tia chứa mục tiêu có khoảng cách tới Dahlia tối đa là (R). Trong số tất cả các mục tiêu trên tia đó, mục tiêu gần nhất cũng có nhiều nhất là (R) và mục tiêu gần nhất đó chính xác là mục tiêu mà Dahlia có thể kéo. Ngược lại, nếu một tia không có mục tiêu trong (R) thì không thể kéo được gì trên tia đó. 

Vì vậy, câu trả lời chỉ đơn giản là số hướng riêng biệt được biểu thị bằng các mục tiêu có khoảng cách tối đa (R). 

Một hướng có thể được biểu diễn chính xác bằng cách sử dụng số học số nguyên. Dịch mọi mục tiêu để Dahlia trở thành gốc, cho vector 

[ 
(dx,dy)=(x_i-x_0,y_i-y_0). 
] 

Chia cả hai thành phần cho 

[ 
g=\gcd(|dx|,|dy|). 
] 

Cặp kết quả 

[ 
\left(\frac{dx}{g},\frac{dy}{g}\right) 
] 

là biểu diễn số nguyên nguyên thủy duy nhất của tia đó. Các điểm như ((2,6)), ((1,3)) và ((10,30)) đều trở thành ((1,3)), trong khi ((-1,-3)) vẫn khác nhau vì nó chỉ theo hướng ngược lại. 

Sau đó chúng ta có thể chèn từng hướng chuẩn hóa vào một tập hợp. Mỗi cặp riêng biệt trong tập hợp này tương ứng với chính xác một tia có thể chạm tới. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(N^2)) | (O(1)) | Quá chậm | 
| Tối ưu | (O(N\log C)) | (O(N)) | Đã chấp nhận | 

Ở đây (C) là độ lớn của chênh lệch tọa độ, nhiều nhất là (2\cdot10^9). Hệ số logarit xuất phát từ thuật toán gcd Euclide. 

## Hướng dẫn thuật toán 

1. Đọc vị trí của Dahlia, khoảng cách tối đa (R) và số lượng mục tiêu. Tính toán (R^2), vì so sánh khoảng cách bình phương sẽ tránh căn bậc hai và đưa ra so sánh số nguyên chính xác. 
2. Với mỗi mục tiêu, hãy tính vectơ tương đối của nó 
[ 
dx=x_i-x_0,\qquad dy=y_i-y_0. 
] 
Điều này thay đổi bài toán từ hình học xung quanh một điểm tùy ý thành các hướng bắt đầu từ gốc tọa độ. 
3. Tính toán 
[ 
d^2=dx^2+dy^2. 
] 
Nếu (d^2>R^2), bỏ qua mục tiêu này. Nó không thể bị kéo và bởi vì mọi mục tiêu khác trên cùng một tia sẽ ở xa hơn nếu nó ở phía sau mục tiêu này, mục tiêu này không thể đóng góp một tia có thể tiếp cận được. 
4. Với mỗi mục tiêu sống sót sau khi kiểm tra khoảng cách, hãy tính 
[ 
g=\gcd(|dx|,|dy|) 
] 
và thay thế vector bằng 
[ 
(dx/g,dy/g). 
] 
Điều này loại bỏ độ lớn của vectơ trong khi vẫn giữ nguyên hướng chính xác của nó. 
5. Chèn cặp đã chuẩn hóa vào một bộ. Các cặp chuẩn hóa bằng nhau có nghĩa là các mục tiêu nằm trên cùng một tia. Các cung khác nhau vẫn khác nhau, do đó các tia đối diện không vô tình hợp nhất với nhau. 
6. Sau khi tất cả các mục tiêu đã được xử lý, hãy xuất kích thước của tập hợp. Mỗi hướng được lưu trữ tương ứng với chính xác một mục tiêu mà Dahlia có thể chọn kéo, cụ thể là mục tiêu gần nhất trên tia đó. 

### Tại sao nó hoạt động 

Xét bất kỳ tia nào chứa ít nhất một mục tiêu trong khoảng cách (R). Trong số những mục tiêu đó, có một mục tiêu gần nhất. Vì khoảng cách của nó tối đa là (R) nên Dahlia có thể chọn tia đó và ngay lập tức kéo mục tiêu gần nhất này. Do đó, mỗi tia có thể tiếp cận đều đóng góp một câu trả lời. 

Bây giờ hãy xem xét một tia không chứa mục tiêu trong khoảng cách (R). Mọi mục tiêu trên tia đó đều ở quá xa nên Dahlia không thể kéo bất cứ thứ gì ra khỏi nó. Vì thế không có tia nào khác đóng góp vào câu trả lời.

Vectơ chuẩn hóa xác định duy nhất một tia vì hai vectơ nguyên khác 0 hướng dọc theo cùng một tia chính xác khi một vectơ là bội số nguyên dương của vectơ nguyên thủy của vectơ kia. Chia cho gcd tạo ra vectơ nguyên thủy đó, trong khi giữ lại các dấu hiệu để phân biệt các hướng ngược nhau. Do đó, bộ này chứa chính xác một mục nhập cho mỗi tia mà từ đó có thể tiếp cận được ít nhất một mục tiêu, đây chính xác là câu trả lời bắt buộc. 

## Giải pháp Python```python
import sys
from math import gcd

input = sys.stdin.readline

def solve():
    x0, y0, R, N = map(int, input().split())

    r2 = R * R
    directions = set()

    for _ in range(N):
        x, y = map(int, input().split())

        dx = x - x0
        dy = y - y0

        if dx * dx + dy * dy > r2:
            continue

        g = gcd(abs(dx), abs(dy))
        directions.add((dx // g, dy // g))

    print(len(directions))

if __name__ == "__main__":
    solve()
```Phần đầu tiên đọc vị trí cố định và tính toán`r2`. Khoảng cách bình phương được sử dụng vì việc lấy căn bậc hai sẽ thêm công việc về dấu phẩy động không cần thiết và có thể gây ra những lo ngại về độ chính xác. 

Đối với mỗi mục tiêu,`dx`Và`dy`được đo tương đối với Dahlia. Việc so sánh khoảng cách được thực hiện trước khi chuẩn hóa vì các mục tiêu bên ngoài vòng tròn hoàn toàn không cần phải biểu diễn. 

Cuộc gọi đến`gcd(abs(dx), abs(dy))`tạo ra vectơ số nguyên nhỏ nhất có cùng hướng. Các giá trị tuyệt đối được truyền cho`gcd`, nhưng dấu ban đầu được giữ lại khi chia nên`(1, 2)`Và`(-1, -2)`vẫn giữ nguyên các mục được thiết lập khác nhau. 

Không có vấn đề chia cho 0 vì câu nói đảm bảo rằng không có mục tiêu nào chiếm giữ vị trí của Dahlia. Như vậy`(dx, dy)`không bao giờ`(0, 0)`, và gcd của nó luôn dương. 

Số nguyên Python có độ chính xác tùy ý, vì vậy các biểu thức như`dx * dx + dy * dy`vẫn an toàn mặc dù chênh lệch tọa độ bình phương có thể đạt tới (4\cdot10^{18}). 

Bộ này tự động loại bỏ tất cả các hướng trùng lặp. Kích thước cuối cùng của nó là câu trả lời. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, Dahlia ở`(0, 0)`và có thể đạt được khoảng cách`10`. Mọi mục tiêu đều nằm trong vòng tròn. 

| Mục tiêu | Vectơ tương đối | Khoảng cách bình phương | Hướng chuẩn hóa | Đặt sau khi chèn | 
| --- | --- | --- | --- | --- | 
|`(1, 2)`|`(1, 2)`|`5`|`(1, 2)`|`{(1,2)}`| 
|`(4, 1)`|`(4, 1)`|`17`|`(4, 1)`|`{(1,2),(4,1)}`| 
|`(-1, -2)`|`(-1, -2)`|`5`|`(-1, -2)`|`{(1,2),(4,1),(-1,-2)}`| 
|`(3, -4)`|`(3, -4)`|`25`|`(3, -4)`|`{(1,2),(4,1),(-1,-2),(3,-4)}`| 

Bốn phương đều khác biệt, gồm có`(1,2)`Và`(-1,-2)`, là các tia đối nhau. Tập cuối cùng chứa bốn mục, vì vậy câu trả lời là`4`. 

Đối với Mẫu 2, Dahlia ở`(1,2)`và có bán kính`5`. 

| Mục tiêu | Vectơ tương đối | Khoảng cách bình phương | Bán kính bên trong? | Hướng chuẩn hóa | Đặt sau khi chèn | 
| --- | --- | --- | --- | --- | --- | 
|`(-2,-2)`|`(-3,-4)`|`25`| Có |`(-3,-4)`|`{(-3,-4)}`| 
|`(6,2)`|`(5,0)`|`25`| Có |`(1,0)`|`{(-3,-4),(1,0)}`| 
|`(10,1)`|`(9,-1)`|`82`| Không | Chưa được chèn |`{(-3,-4),(1,0)}`| 
|`(-2,1)`|`(-3,-1)`|`10`| Có |`(-3,-1)`|`{(-3,-4),(1,0),(-3,-1)}`| 

Vẫn còn ba hướng tiếp cận riêng biệt, đưa ra câu trả lời`3`. 

Dấu vết cũng thực hiện ranh giới bán kính chính xác. Cả hai`(-2,-2)`Và`(6,2)`đang ở khoảng cách bình phương`25`, chính xác là (R^2), vì vậy cả hai đều phải được đưa vào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(N\log C)) | Mỗi mục tiêu yêu cầu số học không đổi cộng với một phép tính gcd trên các số nguyên giới hạn bởi (2\cdot10^9). | 
| Không gian | (O(N)) | Nhiều nhất một hướng chuẩn hóa được lưu trữ cho mỗi mục tiêu. | 

Với (N\le5\cdot10^5), thuật toán thực hiện một lần vượt qua các mục tiêu và một số lượng nhỏ các phép toán số nguyên cho mỗi mục tiêu. Bộ này có thể chứa tối đa (N) cặp, nằm trong giới hạn bộ nhớ 256 MB cho một triển khai Python thông thường, mặc dù chi phí đối tượng của Python khiến mức sử dụng bộ nhớ lớn hơn đáng kể so với biểu diễn toán học thô. 

## Trường hợp thử nghiệm```python
import sys
import io
from math import gcd

def solution():
    input = sys.stdin.readline

    x0, y0, R, N = map(int, input().split())
    r2 = R * R
    directions = set()

    for _ in range(N):
        x, y = map(int, input().split())

        dx = x - x0
        dy = y - y0

        if dx * dx + dy * dy > r2:
            continue

        g = gcd(abs(dx), abs(dy))
        directions.add((dx // g, dy // g))

    print(len(directions))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solution()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample 1
assert run(
    """0 0 10 4
1 2
4 1
-1 -2
3 -4
"""
) == "4\n", "sample 1"

# Provided sample 2
assert run(
    """1 2 5 4
-2 -2
6 2
10 1
-2 1
"""
) == "3\n", "sample 2"

# Minimum-size input, target exactly at the boundary.
assert run(
    """0 0 1 1
1 0
"""
) == "1\n", "minimum size and boundary"

# Same ray, with the farther targets blocked by the nearest one.
assert run(
    """0 0 10 4
1 0
2 0
3 0
-5 0
"""
) == "2\n", "same and opposite rays"

# Boundary plus an outside target, and multiple representations of one ray.
assert run(
    """0 0 5 5
3 4
6 8
1 2
2 4
-3 -4
"""
) == "2\n", "boundary, outside point, duplicate directions"

# Large input, all targets on one ray.
# Positions are distinct and all are within R.
n = 500000
large_input = "0 0 500000 500000\n" + "".join(
    f"{i} 0\n" for i in range(1, 500001)
)
assert run(large_input) == "1\n", "maximum-size same-ray case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 0 1 1 / 1 0`|`1`| Bao gồm đầu vào tối thiểu và bán kính chính xác | 
|`0 0 10 4 / 1 0, 2 0, 3 0, -5 0`|`2`| Nhiều mục tiêu trên một tia và các tia đối diện | 
|`0 0 5 5 / 3 4, 6 8, 1 2, 2 4, -3 -4`|`2`| Ranh giới chính xác, mục tiêu bên ngoài và hướng trùng lặp được chuẩn hóa | 
|`0 0 500000 500000 / (i,0)`|`1`| Tối đa (N) và trường hợp mọi mục tiêu đều có cùng hướng | 

Cuộc thử nghiệm lớn có chủ ý đặt nửa triệu mục tiêu riêng biệt trên trục (x) dương. Mọi mục tiêu đều có thể tiếp cận được, nhưng tất cả chúng đều tương ứng với cùng một tia. Điều này nắm bắt các triển khai tính số điểm có thể tiếp cận thay vì chỉ đường có thể tiếp cận. 

## Vỏ cạnh 

Trường hợp bán kính chính xác được xử lý bằng cách so sánh`dx * dx + dy * dy`với`R * R`sử dụng`>`. Vì```
0 0 5 1
3 4
```khoảng cách bình phương là`25`Và`R * R`cũng là`25`. điều kiện`25 > 25`là sai, vì vậy hướng`(3,4)`được chèn vào và câu trả lời là`1`. Một triển khai sử dụng`<`vì bài kiểm tra đưa vào sẽ loại bỏ nó một cách không chính xác. 

Khi một số mục tiêu chia sẻ một tia, quá trình chuẩn hóa sẽ thu gọn chúng thành một mục nhập đã đặt. Vì```
0 0 10 3
1 0
2 0
3 0
```ba vectơ chuẩn hóa thành`(1,0)`. Do đó, tập hợp chỉ chứa một hướng, cho kết quả đầu ra`1`. Mục tiêu gần nhất`(1,0)`là thứ mà Dahlia thực sự kéo. 

Các hướng đối diện giữ lại các dấu hiệu khác nhau. Vì```
0 0 10 2
3 0
-3 0
```các vectơ chuẩn hóa là`(1,0)`Và`(-1,0)`. Chúng là hai mục riêng biệt, vì vậy câu trả lời là`2`. Giải pháp chuẩn hóa vectơ bằng tọa độ tuyệt đối sẽ hợp nhất hai tia một cách không chính xác. 

Các mục tiêu bên ngoài vòng tròn sẽ bị loại bỏ trước khi vào bộ. Coi như```
0 0 5 2
1 0
10 0
```Mục tiêu đầu tiên có khoảng cách bình phương`1`, trong khi thứ hai có khoảng cách bình phương`100`. Chỉ một`(1,0)`đóng góp, và câu trả lời là`1`. Mục tiêu xa hơn không thể trở nên phù hợp vì cùng một hướng đã có mục tiêu có thể tiếp cận được ở phía trước nó. 

Cuối cùng, các mục tiêu có tọa độ tỷ lệ phải được chuẩn hóa bằng cách sử dụng gcd thay vì so sánh độ dốc với dấu phẩy động. TRONG```
0 0 10 4
1 2
2 4
3 6
-1 -2
```ba mục tiêu đầu tiên đều bình thường hóa thành`(1,2)`, trong khi cái cuối cùng bình thường hóa thành`(-1,-2)`. Tập hợp có đúng hai hướng, vì vậy đầu ra là`2`. Biểu diễn số nguyên này tránh được cả các vấn đề về độ chính xác của dấu phẩy động và việc xử lý đặc biệt cần thiết cho các đường thẳng đứng.
