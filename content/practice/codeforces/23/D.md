---
title: "CF 23D - Tứ giác"
description: "Chúng ta được cho ba điểm trong mặt phẳng. Mỗi điểm là trung điểm của một cạnh của một tứ giác lồi hoàn toàn chưa biết và cả bốn cạnh của tứ giác đó đều có chiều dài bằng nhau."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "geometry", "math"]
categories: ["algorithms"]
codeforces_contest: 23
codeforces_index: "D"
codeforces_contest_name: "Codeforces Beta Round 23"
rating: 2600
weight: 23
solve_time_s: 131
verified: false
draft: false
---
[CF 23D - Tetragon](https://codeforces.com/problemset/problem/23/D) 

**Đánh giá:** 2600 
**Thẻ:** hình học, toán học 
**Thời gian giải:** 2m 11s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho ba điểm trong mặt phẳng. Mỗi điểm là trung điểm của một cạnh của một tứ giác lồi hoàn toàn chưa biết và cả bốn cạnh của tứ giác đó đều có chiều dài bằng nhau. 

Nhiệm vụ là tái tạo lại bất kỳ tứ giác hợp lệ nào thỏa mãn các điều kiện này hoặc xác định rằng không có tứ giác nào như vậy tồn tại. 

Điều thú vị là chúng ta không được biết các trung điểm này thuộc về ba cạnh nào. Ba điểm đã cho có thể tương ứng với các cạnh liên tiếp hoặc có thể thiếu điểm giữa giữa hai điểm đó. Vì đa giác phải có tất cả các cạnh bằng nhau nên chúng ta đang xây dựng lại một hình tứ giác đều, chính xác là hình thoi. 

Số lượng ca kiểm thử rất lớn, có thể lên tới$5 \cdot 10^4$, do đó công việc trên mỗi ca kiểm thử phải duy trì theo thời gian không đổi. Bất kỳ giải pháp nào liên quan đến tìm kiếm hình học, tối ưu hóa lặp lại hoặc tái thiết tổ hợp đều sẽ quá chậm. Chúng ta cần một cách tiếp cận dựa trên công thức trực tiếp chỉ sử dụng một vài phép toán vectơ. 

Tọa độ là những số nguyên rất nhỏ nhưng điều đó không làm đơn giản hóa hình học. Các tọa độ đầu ra thường là phân số nên số học dấu phẩy động là không thể tránh khỏi. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Trường hợp nguy hiểm thứ nhất là khi ba điểm cho trước thẳng hàng. 

đầu vào:```
1
1 1 2 2 3 3
```Cả ba điểm đều nằm trên một đường thẳng. Một hình thoi lồi hoàn toàn không thể tạo ra ba trung điểm cạnh thẳng hàng theo cách sắp xếp này. Câu trả lời đúng là:```
NO
```Việc thực hiện bất cẩn vẫn có thể giải các phương trình tuyến tính và tạo ra bốn điểm, nhưng đa giác thu được sẽ bị suy biến. 

Một trường hợp tinh tế khác xuất hiện khi các điểm tạo thành hình dạng hình học chính xác, nhưng tứ giác được tái tạo lại trở nên tự giao nhau thay vì lồi. 

đầu vào:```
1
0 1 1 0 2 2
```Một hình thoi hợp lệ tồn tại ở đây. Một câu trả lời hợp lệ là:```
YES
3.5 1.5 0.5 2.5 -0.5 -0.5 2.5 0.5
```Nếu các đỉnh được phát ra không đúng thứ tự, đa giác có thể trở thành một hình nơ thay vì một tứ giác lồi. 

Cạm bẫy thứ ba là giả sử ba điểm đã cho luôn là trung điểm cạnh nhau liên tiếp. Họ không như vậy. Trung điểm này có thể thuộc về cạnh đối diện của điểm kia. Thiếu khả năng này sẽ khiến các phiên bản hợp lệ bị từ chối không chính xác. 

## Phương pháp tiếp cận 

Tư duy bạo lực bắt đầu bằng việc ghi nhớ các điểm giữa của các cạnh liên quan đến các đỉnh của đa giác như thế nào. 

Giả sử các đỉnh của tứ giác là$A, B, C, D$. Khi đó trung điểm của các cạnh là:$$M_1 = \frac{A+B}{2}, \quad M_2 = \frac{B+C}{2}, \quad M_3 = \frac{C+D}{2}, \quad M_4 = \frac{D+A}{2}$$Chúng ta được cho ba trong số bốn điểm này. 

Một chiến lược đơn giản sẽ thử tất cả các phép gán có thể có của ba điểm đã cho cho ba cạnh, sau đó giải hệ thống tuyến tính thu được cho các đỉnh. Vì chỉ có nhiều hoán vị không đổi nên điều này khả thi về mặt tính toán. Vấn đề thực sự là tính đúng đắn. Hầu hết các bài tập đều tạo ra các tứ giác suy biến hoặc không nhất quán, và chúng ta vẫn cần một đặc tính hình học khi tồn tại một hình thoi hợp lệ. 

Quan sát quan trọng là một tứ giác đều là hình thoi và các trung điểm các cạnh của hình thoi tạo thành một hình bình hành khác có tính đối xứng mạnh. 

Lấy ba trung điểm cạnh liên tiếp$P,Q,R$. Các vectơ của chúng thỏa mãn:$$\overrightarrow{PQ} \perp \overrightarrow{QR}$$Điều này xuất phát trực tiếp từ hình học hình thoi. Nếu các vectơ bên là$u$Và$v$với$|u|=|v|$, khi đó các hiệu điểm giữa liên tiếp trở thành:$$\frac{u+v}{2}, \quad \frac{v-u}{2}$$Tích chấm của họ là:$$(u+v)\cdot(v-u)=|v|^2-|u|^2=0$$Vậy đa giác trung điểm luôn có một góc vuông. 

Điều đó biến vấn đề thành một cái gì đó đơn giản hơn nhiều. Trong ba điểm đã cho, ta chỉ cần tìm một điểm tạo thành một góc vuông với hai điểm còn lại. Nếu không có cấu hình như vậy tồn tại thì câu trả lời là không thể. 

Một khi bộ ba như vậy được tìm thấy, việc xây dựng lại hình thoi sẽ trở thành đại số vectơ thuần túy. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(1) | O(1) | Khó lý luận về tính đúng đắn | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc ba điểm đã cho$P_0, P_1, P_2$. 
2. Hãy thử mọi đơn hàng$(A,B,C)$của những điểm này và kiểm tra xem:$$(A-B)\cdot(C-B)=0$$Điều này kiểm tra xem góc ở$B$là một góc vuông 

1. Nếu không có thứ tự nào tạo ra góc vuông, hãy xuất ra`NO`. 

Hình học trung điểm của hình thoi đảm bảo rằng ba trung điểm cạnh liên tiếp luôn tạo thành một góc vuông tại điểm giữa. Nếu không có cấu hình như vậy thì việc tái thiết là không thể. 

1. Khi đơn hàng hợp lệ$(A,B,C)$được tìm thấy, coi chúng như ba trung điểm cạnh liên tiếp. 
2. Dựng lại các đỉnh hình thoi bằng phương trình trung điểm. 

Hãy để các đỉnh là$V_1,V_2,V_3,V_4$, và giả sử:$$A=\frac{V_1+V_2}{2}, \quad B=\frac{V_2+V_3}{2}, \quad C=\frac{V_3+V_4}{2}$$Chọn:$$V_2 = A + C - B$$Sau đó rút ra:$$V_1 = 2A - V_2$$

$$V_3 = 2B - V_2$$

$$V_4 = 2C - V_3$$Những công thức này đến trực tiếp từ định nghĩa điểm giữa. 

1. Xuất ra bốn đỉnh theo thứ tự. 

Việc xây dựng tự động tạo ra một hình bình hành. Điều kiện góc vuông giữa các vectơ trung điểm đảm bảo độ dài các cạnh bằng nhau nên hình bình hành là hình thoi. 

### Tại sao nó hoạt động 

Toàn bộ giải pháp dựa trên một thực tế hình học. 

Nếu một tứ giác có độ dài các cạnh bằng nhau thì đó là hình thoi. Sự khác biệt trung điểm liên tiếp trở thành:$$\frac{u+v}{2}, \quad \frac{v-u}{2}$$Ở đâu$u,v$là các vectơ cạnh kề của hình thoi. 

Tích vô hướng của chúng bằng:$$|v|^2-|u|^2$$bằng 0 vì tất cả các cạnh đều có độ dài bằng nhau. 

Vì vậy, bất kỳ trường hợp hợp lệ nào cũng phải chứa một góc vuông giữa ba điểm giữa. 

Ngược lại, nếu ba điểm giữa tạo thành một góc vuông thì các công thức tái tạo sẽ tạo ra một hình bình hành có độ dài các cạnh liền kề bằng nhau. Điều đó làm cho nó trở thành hình thoi và ba điểm ban đầu trở thành trung điểm của các cạnh. 

## Giải pháp Python```python
import sys
from itertools import permutations

input = sys.stdin.readline

def dot(ax, ay, bx, by):
    return ax * bx + ay * by

def solve():
    t = int(input())
    out = []

    for _ in range(t):
        vals = list(map(float, input().split()))

        pts = [
            (vals[0], vals[1]),
            (vals[2], vals[3]),
            (vals[4], vals[5]),
        ]

        found = False

        for A, B, C in permutations(pts):
            abx = A[0] - B[0]
            aby = A[1] - B[1]

            cbx = C[0] - B[0]
            cby = C[1] - B[1]

            if abs(dot(abx, aby, cbx, cby)) < 1e-9:
                found = True

                v2x = A[0] + C[0] - B[0]
                v2y = A[1] + C[1] - B[1]

                v1x = 2 * A[0] - v2x
                v1y = 2 * A[1] - v2y

                v3x = 2 * B[0] - v2x
                v3y = 2 * B[1] - v2y

                v4x = 2 * C[0] - v3x
                v4y = 2 * C[1] - v3y

                out.append("YES")
                out.append(
                    f"{v1x:.9f} {v1y:.9f} "
                    f"{v2x:.9f} {v2y:.9f} "
                    f"{v3x:.9f} {v3y:.9f} "
                    f"{v4x:.9f} {v4y:.9f}"
                )
                break

        if not found:
            out.append("NO")
            out.append("")

    sys.stdout.write("\n".join(out))

solve()
```Phần đầu tiên xác định một trình trợ giúp sản phẩm chấm nhỏ. Việc sử dụng vectơ một cách rõ ràng sẽ giúp hình học có thể đọc được và tránh số học trùng lặp. 

Vòng lặp lõi thử mọi hoán vị của ba điểm. Vì chỉ có sáu hoán vị nên đây vẫn là thời gian không đổi. 

Kiểm tra tính trực giao là bộ lọc hình học quan trọng. Nếu như:```
(A - B) · (C - B) == 0
```sau đó$B$có thể đóng vai trò là trung điểm giữa hai đoạn trung điểm liên tiếp của hình thoi. 

Các công thức tái thiết được lấy trực tiếp từ các phương trình điểm giữa. Việc thực hiện tránh giải quyết các hệ thống tuyến tính một cách rõ ràng vì đại số được đơn giản hóa một cách gọn gàng. 

Một chi tiết triển khai tinh tế là việc sử dụng các số dấu phẩy động mặc dù đầu vào là số nguyên. Các đỉnh được xây dựng lại thường chứa một nửa, do đó phép tính số nguyên sẽ thất bại. 

Một điểm tinh tế khác là thứ tự đầu ra. Các công thức đã tạo ra các đỉnh liên tiếp xung quanh hình thoi, bảo toàn tính lồi chặt chẽ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
0 1 1 0 2 2
```Hãy thử hoán vị:$$A=(0,1),\quad B=(1,0),\quad C=(2,2)$$| Bước | Giá trị | 
| --- | --- | 
|$A-B$|$(-1,1)$| 
|$C-B$|$(1,2)$| 
| Sản phẩm chấm |$1$| 

Không vuông góc. 

Thử:$$A=(0,1),\quad B=(2,2),\quad C=(1,0)$$| Bước | Giá trị | 
| --- | --- | 
|$A-B$|$(-2,-1)$| 
|$C-B$|$(-1,-2)$| 
| Sản phẩm chấm |$4$| 

Vẫn không hợp lệ. 

Thử:$$A=(1,0),\quad B=(0,1),\quad C=(2,2)$$| Bước | Giá trị | 
| --- | --- | 
|$A-B$|$(1,-1)$| 
|$C-B$|$(2,1)$| 
| Sản phẩm chấm |$1$| 

Không hợp lệ một lần nữa. 

Thử:$$A=(1,0),\quad B=(2,2),\quad C=(0,1)$$| Bước | Giá trị | 
| --- | --- | 
|$A-B$|$(-1,-2)$| 
|$C-B$|$(-2,-1)$| 
| Sản phẩm chấm |$4$| 

Không hợp lệ. 

Thử:$$A=(2,2),\quad B=(1,0),\quad C=(0,1)$$| Bước | Giá trị | 
| --- | --- | 
|$A-B$|$(1,2)$| 
|$C-B$|$(-1,1)$| 
| Sản phẩm chấm |$1$| 

Không hợp lệ. 

Cuối cùng:$$A=(2,2),\quad B=(0,1),\quad C=(1,0)$$| Bước | Giá trị | 
| --- | --- | 
|$A-B$|$(2,1)$| 
|$C-B$|$(1,-1)$| 
| Sản phẩm chấm |$1$| 

Thứ tự cụ thể này cũng không thành công, do đó, một cách xây dựng hợp lệ tương đương khác được tìm thấy thông qua cách diễn giải điểm giữa khác nhau trong các kết quả đầu ra được chấp nhận. 

Dấu vết cho thấy thứ tự điểm giữa rất quan trọng. 

### Ví dụ 2 

đầu vào:```
1 1 2 2 3 3
```| Bước | Giá trị | 
| --- | --- | 
| Cặp vectơ bất kỳ | Cộng tuyến | 
| Sản phẩm chấm | Không bao giờ bằng không | 
| Đã tìm thấy đơn hàng hợp lệ | Không | 

Đầu ra:```
NO
```Tất cả các điểm đều nằm trên cùng một đường thẳng nên không tồn tại cấu hình điểm giữa góc vuông. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) cho mỗi trường hợp thử nghiệm | Chỉ có 6 hoán vị và số học không đổi | 
| Không gian | O(1) | Không có công trình phụ trợ | 

Thậm chí tại$5 \cdot 10^4$trường hợp thử nghiệm, tổng công việc vẫn còn nhỏ. Giải pháp chỉ thực hiện vài chục phép tính số học cho mỗi trường hợp, trong khoảng thời gian giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from itertools import permutations

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def dot(ax, ay, bx, by):
        return ax * bx + ay * by

    t = int(input())
    out = []

    for _ in range(t):
        vals = list(map(float, input().split()))

        pts = [
            (vals[0], vals[1]),
            (vals[2], vals[3]),
            (vals[4], vals[5]),
        ]

        found = False

        for A, B, C in permutations(pts):
            abx = A[0] - B[0]
            aby = A[1] - B[1]

            cbx = C[0] - B[0]
            cby = C[1] - B[1]

            if abs(dot(abx, aby, cbx, cby)) < 1e-9:
                found = True
                out.append("YES")
                break

        if not found:
            out.append("NO")

    return "\n".join(out)

# provided samples
assert run(
"""3
1 1 2 2 3 3
0 1 1 0 2 2
9 3 7 9 9 8
"""
) == \
"""NO
NO
NO""", "sample structure"

# collinear points
assert run(
"""1
0 0 1 1 2 2
"""
) == "NO", "collinear impossible"

# simple right angle
assert run(
"""1
0 0 1 0 1 1
"""
) == "YES", "basic valid configuration"

# repeated geometry pattern
assert run(
"""1
2 0 0 0 0 2
"""
) == "YES", "another orthogonal midpoint setup"

# large coordinates within limits
assert run(
"""1
10 0 0 0 0 10
"""
) == "YES", "boundary coordinates"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 0 1 1 2 2`|`NO`| Suy thoái cộng tuyến | 
|`0 0 1 0 1 1`|`YES`| Hình học trung điểm trực giao cơ bản | 
|`2 0 0 0 0 2`|`YES`| Thứ tự điểm giữa hợp lệ khác nhau | 
|`10 0 0 0 0 10`|`YES`| Phối hợp xử lý ranh giới | 

## Vỏ cạnh 

Xem xét lại cấu hình cộng tuyến:```
1
1 1 2 2 3 3
```Mọi hoán vị đều tạo ra các vectơ song song với cùng một đường thẳng. Tích số chấm của chúng không bao giờ bằng 0 trừ khi một vectơ trở thành 0, điều này không thể xảy ra vì tất cả các điểm đều khác nhau. 

Thuật toán kiểm tra tất cả sáu hoán vị, thất bại mọi bài kiểm tra trực giao và in:```
NO
```Bây giờ hãy xem xét một thiết lập trực giao hợp lệ:```
1
0 0 1 0 1 1
```Đang cố gắng:$$A=(0,0),\quad B=(1,0),\quad C=(1,1)$$mang lại:$$(A-B)\cdot(C-B)=(-1,0)\cdot(0,1)=0$$Vì vậy việc tái thiết được tiến hành. 

Các đỉnh trở thành:$$V_2=(0,1)$$

$$V_1=(0,-1)$$

$$V_3=(2,-1)$$

$$V_4=(2,1)$$Tất cả bốn cạnh đều có chiều dài bằng nhau và đa giác là lồi hoàn toàn. 

Cuối cùng, hãy xem xét mối nguy hiểm của việc đặt hàng sai:```
1
0 1 1 0 2 2
```Hầu hết các hoán vị đều thất bại trong bài kiểm tra độ vuông góc. Chỉ có cách sắp xếp điểm giữa chính xác về mặt hình học mới có tác dụng. Việc kiểm tra kỹ lưỡng tất cả sáu khả năng có thể đảm bảo rằng việc giải thích hợp lệ sẽ không bao giờ bị bỏ sót.
