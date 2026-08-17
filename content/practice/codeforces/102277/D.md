---
title: "CF 102277D - Vòng tròn gặp vuông"
description: "Chúng ta có một hình tròn và một hình vuông thẳng hàng với trục trong mặt phẳng Descartes. Đường tròn được mô tả bởi tâm (x, y) và bán kính r. Hình vuông được mô tả bởi góc dưới bên trái (tx, ty) và độ dài cạnh s, do đó góc đối diện của nó là (tx + s, ty + s)."
date: "2026-08-16T19:33:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102277
codeforces_index: "D"
codeforces_contest_name: "UCF Locals 2018"
rating: 0
weight: 102277
solve_time_s: 65
verified: true
draft: false
---

[CF 102277D - Vòng tròn gặp vuông](https://codeforces.com/problemset/problem/102277/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một hình tròn và một hình vuông thẳng hàng với trục trong mặt phẳng Descartes. Vòng tròn được mô tả bởi tâm của nó`(x, y)`và bán kính`r`. Hình vuông được mô tả bởi góc dưới bên trái của nó`(tx, ty)`và độ dài cạnh`s`, vậy góc đối diện của nó là`(tx + s, ty + s)`. Nhiệm vụ là phân loại mối quan hệ hình học của chúng. Chúng tôi in`0`khi chúng rời rạc,`1`khi phần chung của chúng gồm đúng một điểm và`2`khi chúng trùng với diện tích dương. Giới hạn chính thức chỉ là`1000`về độ lớn cho tọa độ và kích thước, với giới hạn thời gian 1 giây và bộ nhớ 256 MB. 

Giới hạn số lượng nhỏ nhưng chúng không gợi ý một mô phỏng hữu ích. Các đối tượng chứa vô số điểm và thậm chí việc lặp qua mọi tọa độ nguyên trong hộp giới hạn sẽ không phải là thuật toán hình học hợp lệ vì điểm tiếp tuyến có thể có tọa độ không nguyên. Giải pháp sạch nên sử dụng số lượng phép tính số học không đổi và tránh hoàn toàn hình học dấu phẩy động. 

Có một số trường hợp ranh giới trong đó việc triển khai chỉ kiểm tra xem một góc nào đó nằm bên trong đối tượng kia có thể bị lỗi hay không. Ví dụ,```
0 0 5
5 0 6
```có đầu ra`1`. Quảng trường bắt đầu lúc`(5, 0)`, cách tâm đường tròn đúng 5 đơn vị, nên hai vật chạm nhau tại`(5, 0)`. Một sự nghiêm khắc`< r`bài kiểm tra sẽ báo cáo sai`0`, vì điểm tiếp xúc nằm trên đường tròn chứ không phải bên trong đường tròn. 

Trường hợp thứ hai là một hình tròn nằm hoàn toàn bên trong hình vuông:```
0 0 1
-5 -5 10
```Đầu ra đúng là`0`. Hình tròn hoàn toàn không giao nhau với ranh giới của hình vuông, mặc dù hai vùng hình học có sự ngăn chặn. Một phương pháp giả định "một đối tượng nằm bên trong đối tượng kia nên chúng chồng lên nhau" sẽ gây nhầm lẫn giữa việc ngăn chặn với giao điểm ranh giới. Việc phân loại bắt buộc là xem hình tròn và hình vuông có một vùng chung hay một điểm chung duy nhất, do đó, đối với vùng đĩa và hình vuông, trường hợp này thực sự có giao điểm diện tích dương và phải được phân loại là`2`. Điều này minh họa tại sao ý nghĩa chính xác của các đối tượng lại quan trọng: vòng tròn thể hiện phần bên trong được lấp đầy của nó như một phần của khu vực được so sánh. 

Một trường hợp hữu ích hơn là ngăn chặn ngược:```
0 0 10
-1 -1 2
```Đầu ra đúng là`2`. Toàn bộ hình vuông nằm bên trong hình tròn nên giao điểm của chúng có diện tích dương. Việc chỉ kiểm tra xem điểm cực trị của đường tròn có nằm bên trong hình vuông hay không sẽ hoàn toàn bỏ sót điều này. 

Cách rõ ràng nhất để tránh tất cả các trường hợp đặc biệt này là giảm bài toán xuống một đại lượng: khoảng cách tối thiểu từ tâm hình tròn đến bất kỳ điểm nào của hình vuông. 

## Phương pháp tiếp cận 

Một lực lượng hình học trực tiếp có thể liệt kê bốn góc của hình vuông và bốn điểm chính của hình tròn, sau đó phân loại xem có bất kỳ điểm ứng cử viên nào nằm hoàn toàn bên trong hay chính xác trên hình dạng kia hay không. Đây là một tìm kiếm có kích thước không đổi và với đối số lồi thích hợp, nó có thể giải quyết được vấn đề. Nó thực hiện bốn lần kiểm tra từ góc tới vòng tròn và bốn lần kiểm tra theo vòng tròn, từ điểm cực đến hình vuông, vì vậy trường hợp xấu nhất của nó là tám lần kiểm tra khoảng cách hoặc ngăn chặn, cộng với phép tính số học tương ứng. Cách tiếp cận này được chấp nhận một cách tiệm cận vì vấn đề này chỉ có một trường hợp thử nghiệm, nhưng rất dễ làm cho nó không đầy đủ do quên cấu hình ngăn chặn hoặc các trường hợp biên. 

Quan sát tốt hơn là chúng ta không cần phải tìm kiếm điểm giao nhau. Đối với tâm đường tròn cố định, hãy tìm điểm của hình vuông gần tâm đó nhất. Nếu tâm nằm ngang ngoài hình vuông thì điểm gần nhất phải sử dụng cạnh dọc tương ứng. Nếu tâm nằm ngang bên trong hình vuông thì điểm gần nhất của nó có cùng điểm`x`điều phối. Lý do tương tự áp dụng độc lập cho`y`điều phối. 

Điều này mang lại điểm bình phương gần nhất bằng cách kẹp từng tọa độ một cách độc lập. Nếu trung tâm có tọa độ`x`, và hình vuông kéo dài`[tx, tx+s]`, hình vuông hợp lệ gần nhất`x`tọa độ là```
min(max(x, tx), tx+s)
```và tương tự cho`y`. 

Khi đã biết điểm gần nhất, hình học sẽ trở thành một phép so sánh khoảng cách đơn giản. Cho phép`d`là khoảng cách từ tâm đường tròn đến điểm đó. Nếu như`d < r`, hình tròn tiến vào hình vuông có diện tích dương. Nếu như`d = r`, đường tròn tiếp xúc với hình vuông tại đúng một điểm. Nếu như`d > r`, hình tròn và hình vuông rời nhau, trừ khi hình vuông nằm hoàn toàn bên trong hình tròn. Trường hợp sau đã được bao phủ bởi`d < r`, bởi vì tâm hình tròn khi đó nằm bên trong hình vuông hoặc đủ gần với ranh giới hình vuông để hình tròn chứa một phần của hình vuông. 

Sự đơn giản hóa quan trọng là việc so sánh có thể được thực hiện với khoảng cách bình phương. Vì cả hai`d`Và`r`không âm, so sánh`d²`với`r²`cho kết quả chính xác như nhau và tránh được các vấn đề về căn bậc hai và độ chính xác của dấu phẩy động. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Kiểm tra góc và điểm cực trị | O(1) | O(1) | Được chấp nhận, nhưng nặng ký tự hơn | 
| Điểm gần nhất bằng kẹp | O(1) | O(1) | Được chấp nhận, ưa thích | 

## Hướng dẫn thuật toán 

1. Đọc tâm vòng tròn`(x, y)`, bán kính`r`, và góc dưới bên trái của hình vuông`(tx, ty)`và độ dài cạnh`s`. 
2. Tính ranh giới bên phải và ranh giới trên cùng của hình vuông như sau:`right = tx + s`Và`top = ty + s`. Hình vuông chính xác là tích Descartes`[tx, right] × [ty, top]`. 
3. Kẹp tâm vòng tròn`x`tọa độ theo khoảng ngang của hình vuông. Tính toán`px = min(max(x, tx), right)`. Nếu như`x`đã ở trong khoảng,`px`bằng`x`. Nếu như`x`là ở bên trái,`px`trở thành`tx`, và nếu`x`là ở bên phải,`px`trở thành`right`. 
4. Thực hiện thao tác tương tự cho`y`, thu được`py = min(max(y, ty), top)`. điểm`(px, py)`là điểm gần tâm hình vuông nhất trong hình vuông. 
5. Tính bình phương khoảng cách từ`(x, y)`ĐẾN`(px, py)`BẰNG`dist2 = (x - px)^2 + (y - py)^2`. So sánh nó với`r^2`. 
6. In`2`nếu như`dist2 < r^2`, vì hình tròn kéo dài vào hình vuông và giao điểm có diện tích dương. In`1`nếu như`dist2 == r^2`, vì điểm vuông gần nhất nằm chính xác trên đường tròn. Nếu không thì in`0`, vì mọi điểm của hình vuông đều ở xa tâm đường tròn hơn bán kính. 

Bất biến đằng sau thuật toán là`(px, py)`luôn là điểm gần tâm hình vuông nhất của hình vuông. Mỗi điểm của hình vuông ít nhất cũng cách xa tâm điểm này. Do đó, nếu ngay cả điểm gần nhất này cũng nằm bên ngoài hình tròn thì toàn bộ hình vuông cũng nằm bên ngoài nó. Nếu nó nằm hoàn toàn bên trong đường tròn thì một lân cận nhỏ xung quanh điểm đó cũng nằm bên trong đường tròn, cho diện tích giao nhau dương. Nếu nó nằm chính xác trên hình tròn thì hình vuông và hình tròn gặp nhau ở điểm gần nhất và không thể xuyên qua nhau, tạo thành một điểm tiếp xúc duy nhất. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    x, y, r = map(int, input().split())
    tx, ty, s = map(int, input().split())

    right = tx + s
    top = ty + s

    px = min(max(x, tx), right)
    py = min(max(y, ty), top)

    dx = x - px
    dy = y - py

    dist2 = dx * dx + dy * dy
    r2 = r * r

    if dist2 < r2:
        print(2)
    elif dist2 == r2:
        print(1)
    else:
        print(0)

if __name__ == "__main__":
    solve()
```Hai dòng đầu vào mô tả chính xác hai đối tượng hình học như trong thuật toán. Chúng tôi ngay lập tức tính toán các ranh giới bên phải và trên cùng của hình vuông để các biểu thức tiếp theo có thể coi hình vuông là hai khoảng tọa độ khép kín. 

hai`min(max(...))`biểu thức thực hiện kẹp tọa độ. Ở đây, phép tính số nguyên của Python đặc biệt thuận tiện vì mọi đầu vào đều là tích phân và chênh lệch bình phương lớn nhất dễ dàng nằm trong phạm vi số nguyên có độ chính xác tùy ý của Python. 

Việc trừ được thực hiện có chủ ý sau khi kẹp. Điều này đưa ra các thành phần theo chiều ngang và chiều dọc của vectơ ngắn nhất từ ​​tâm hình tròn đến hình vuông. Bình phương và cộng sẽ cho khoảng cách bình phương tối thiểu. 

Sự so sánh chặt chẽ và không chặt chẽ đều cần thiết.`<`có nghĩa là điểm vuông gần nhất nằm bên trong đường tròn, trong khi`==`đại diện cho tiếp tuyến chính xác. Thay thế trường hợp đẳng thức bằng`<=`sẽ phân loại không chính xác mọi cấu hình tiếp tuyến thành sự chồng chéo. 

Không lấy căn bậc hai, do đó không có lỗi dấu phẩy động xung quanh các trường hợp tiếp tuyến chẳng hạn như khoảng cách chính xác`5`so với bán kính`5`. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Mẫu được cung cấp đầu tiên là:```
0 0 5
2 3 1
```Hình vuông trải dài`x = [2, 3]`Và`y = [3, 4]`. Tâm vòng tròn`(0, 0)`nằm bên dưới và bên trái, vì vậy điểm vuông gần nhất là góc dưới bên trái của nó`(2, 3)`. 

|`x`|`y`|`px`|`py`|`dist2`|`r2`| Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 2 | 3 | 13 | 25 | 2 | 

Từ`13 < 25`, điểm gần nhất của hình vuông nằm bên trong đường tròn. Do đó, giao lộ chứa một khu vực chứ không chỉ là một điểm ranh giới. Sản lượng dự kiến ​​​​là`2`. 

### Mẫu 2 

Mẫu được cung cấp thứ hai là:```
0 0 5
5 0 6
```Hình vuông trải dài`x = [5, 11]`Và`y = [0, 6]`. Tâm hình tròn nằm ngay bên trái hình vuông nên điểm gần nhất của nó là`(5, 0)`. 

|`x`|`y`|`px`|`py`|`dist2`|`r2`| Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 0 | 5 | 0 | 25 | 25 | 1 | 

Bình phương khoảng cách bằng nhau nên điểm gần nhất nằm chính xác trên đường tròn. Hai hình chạm nhau tại một điểm và kết quả mong đợi là`1`. 

### Mẫu 3 

Mẫu được cung cấp thứ ba là:```
0 5 4
-1 -1 1
```Hình vuông trải dài`x = [-1, 0]`Và`y = [-1, 0]`. Điểm gần tâm vòng tròn nhất`(0, 5)`là`(0, 0)`. 

|`x`|`y`|`px`|`py`|`dist2`|`r2`| Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| 0 | 5 | 0 | 0 | 25 | 16 | 0 | 

Đây`25 > 16`, do đó ngay cả điểm gần nhất của hình vuông cũng nằm ngoài hình tròn. Hình vuông và hình tròn rời nhau, cho ra kết quả`0`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ có một số lượng không đổi các phép toán tọa độ và so sánh số học được thực hiện. | 
| Không gian | O(1) | Chỉ có một số biến số nguyên cố định được lưu trữ. | 

Tọa độ và kích thước của bài toán được giới hạn bởi`1000`, nhưng thuật toán hoàn toàn không phụ thuộc vào các giới hạn đó. Thời gian chạy của nó không đổi ngay cả khi giới hạn tọa độ tăng lên đáng kể. Nó dễ dàng phù hợp với giới hạn 1 giây và 256 MB được chỉ định cho sự cố. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    x, y, r = map(int, input().split())
    tx, ty, s = map(int, input().split())

    right = tx + s
    top = ty + s

    px = min(max(x, tx), right)
    py = min(max(y, ty), top)

    dx = x - px
    dy = y - py

    dist2 = dx * dx + dy * dy
    r2 = r * r

    if dist2 < r2:
        print(2)
    elif dist2 == r2:
        print(1)
    else:
        print(0)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided samples
assert run("0 0 5\n2 3 1\n") == "2\n", "sample 1"
assert run("0 0 5\n5 0 6\n") == "1\n", "sample 2"
assert run("0 5 4\n-1 -1 1\n") == "0\n", "sample 3"

# Minimum-size objects, clearly separated
assert run("0 0 1\n2 2 1\n") == "0\n", "minimum sizes, disjoint"

# All values in the first and second descriptions are small and equal where possible.
# The circle center is inside the square.
assert run("0 0 1\n0 0 1\n") == "2\n", "center inside square"

# Exact corner tangency
assert run("0 0 5\n5 5 1\n") == "0\n", "corner is farther than radius"

# Exact side tangency, catches < versus <=
assert run("0 0 5\n5 0 1\n") == "1\n", "side tangency"

# Square completely inside the circle
assert run("0 0 10\n-1 -1 2\n") == "2\n", "square inside circle"

# Large coordinates and dimensions
assert run("1000 1000 1000\n-999 -999 1000\n") == "0\n", "large disjoint configuration"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`0 0 1 / 2 2 1`|`0`| Các đối tượng có kích thước tối thiểu và sự phân tách rõ ràng | 
|`0 0 1 / 0 0 1`|`2`| Vòng tròn tâm bên trong hình vuông | 
|`0 0 5 / 5 5 1`|`0`| Khoảng cách chéo và xử lý góc | 
|`0 0 5 / 5 0 1`|`1`| Tiếp tuyến bên chính xác và xử lý đẳng thức | 
|`0 0 10 / -1 -1 2`|`2`| Toàn bộ hình vuông chứa bên trong hình tròn | 
|`1000 1000 1000 / -999 -999 1000`|`0`| Giá trị biên lớn | 

## Vỏ cạnh 

Trường hợp tinh tế đầu tiên là tiếp tuyến chính xác. Với```
0 0 5
5 0 1
```điểm gần tâm nhất của hình vuông là`(5, 0)`. Bình phương khoảng cách là`25`, chính xác bằng`r² = 25`, do đó thuật toán in`1`. Kiểm tra bên trong nghiêm ngặt mà không có trường hợp đẳng thức riêng biệt sẽ in không chính xác`0`. 

Trường hợp thứ hai là tâm đường tròn nằm trong hình vuông. Vì```
0 0 1
-5 -5 10
```kẹp giữ cho tâm không thay đổi, vì vậy`(px, py) = (0, 0)`. Khoảng cách bình phương tối thiểu là`0`, nhỏ hơn`1`. Thuật toán in`2`, nhận biết chính xác rằng hình tròn và hình vuông có giao diện tích dương. 

Trường hợp thứ ba là hình vuông nằm hoàn toàn bên trong hình tròn:```
0 0 10
-1 -1 2
```Một lần nữa, tâm hình tròn nằm bên trong hình vuông, vì vậy điểm hình vuông gần tâm nhất chính là tâm và khoảng cách bình phương tối thiểu là`0`. Từ`0 < 100`, kết quả là`2`. Trường hợp này rất hữu ích vì chỉ kiểm tra các điểm ranh giới của hình tròn so với hình vuông sẽ hoàn toàn bỏ sót giao điểm. 

Trường hợp thứ tư là tách đường chéo:```
0 0 5
5 5 1
```Điểm gần nhất là`(5, 5)`, cho khoảng cách bình phương`25 + 25 = 50`. Từ`50 > 25`, kết quả là`0`. So sánh khoảng cách bình phương cũng xử lý đường chéo mà không đưa ra căn bậc hai. 

Trường hợp thứ năm là cấu hình ranh giới tọa độ lớn:```
1000 1000 1000
-999 -999 1000
```Hình vuông kết thúc tại`(1, 1)`, đó là điểm gần nhất`(1000, 1000)`. Bình phương khoảng cách là`999² + 999²`, lớn hơn`1000²`, do đó thuật toán in`0`. Điều này xác nhận rằng ranh giới tọa độ không yêu cầu bất kỳ xử lý đặc biệt nào.
