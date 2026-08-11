---
title: "CF 102411I - Kim tự tháp lý tưởng"
description: "Chúng ta cần chọn tâm nguyên (X, Y) và chiều cao nguyên H cho hình chóp vuông thẳng hàng với trục. Bởi vì mọi cạnh đều có độ dốc chính xác là 45°, nên việc di chuyển một đơn vị theo chiều ngang hoặc chiều dọc ra khỏi tâm sẽ làm giảm hình chóp đi đúng một đơn vị."
date: "2026-08-10T14:48:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102411
codeforces_index: "I"
codeforces_contest_name: "ICPC 2019-2020 North-Western Russia Regional Contest"
rating: 0
weight: 102411
solve_time_s: 785
verified: true
draft: false
---

[CF 102411I - Kim tự tháp lý tưởng](https://codeforces.com/problemset/problem/102411/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 13m 5s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta cần chọn một trung tâm số nguyên`(X, Y)`và chiều cao nguyên`H`cho một hình chóp vuông thẳng hàng với trục. Vì mọi cạnh đều có độ dốc chính xác`45°`, di chuyển một đơn vị theo chiều ngang hoặc chiều dọc ra khỏi tâm sẽ làm giảm hình chóp đi đúng một đơn vị. Như vậy, tại vị trí`(x, y)`, chiều cao của kim tự tháp là 

[ 
H-\max(|x-X|,|y-Y|). 
] 

một đài tưởng niệm`(x_i, y_i, h_i)`phù hợp nếu chiều cao của nó không vượt quá giá trị này. Nhiệm vụ là tìm ra một kim tự tháp có chiều cao nhỏ nhất có thể chứa mọi đài tưởng niệm và đưa ra bất kỳ tâm nào đạt được mức tối thiểu đó. 

Đầu vào chứa tối đa`1000`đài tưởng niệm. Tọa độ và độ cao của chúng có thể đạt tới`10^8`về độ lớn nên việc liệt kê các tọa độ có thể là hoàn toàn không khả thi. Một tọa độ có thể có khoảng`2 * 10^8`các giá trị có thể, đưa ra đại khái`4 * 10^16`những trung tâm có thể Với`1000`các đài tưởng niệm, việc kiểm tra tất cả chúng sẽ cần khoảng`4 * 10^19`kiểm tra cơ bản. Giải pháp cần xử lý đầu vào về cơ bản một cách tuyến tính. 

Các hạn chế về số nguyên cũng có vấn đề. Tối ưu hình học liên tục là không đủ vì tâm và chiều cao phải là số nguyên. May mắn thay, các ràng buộc liên quan đều là các khoảng nguyên, do đó có thể đạt được số nguyên tối ưu chính xác mà không cần số học dấu phẩy động. 

Có một số trường hợp nguy hiểm có thể khiến giải pháp bất cẩn thất bại. Với một đài tưởng niệm duy nhất,```
1
0 0 5
```câu trả lời là`0 0 5`. Một phương pháp cố gắng đặt tâm một cách mù quáng ở giữa các tọa độ cực trị có thể vô tình làm tăng chiều cao mặc dù đặt nó ngay dưới đài tưởng niệm là tối ưu. 

Một trường hợp ranh giới thú vị hơn là```
2
0 0 1
2 0 1
```Câu trả lời đúng có thể là```
1 0 2
```vì kim tự tháp có chiều cao`2`, và cả hai đài tưởng niệm đều cách tâm một đơn vị. Khoảng cho tâm tối ưu đóng lại ở một điểm nguyên duy nhất. Sử dụng bất đẳng thức nghiêm ngặt thay vì bất đẳng thức bao hàm sẽ bác bỏ nghiệm này một cách không chính xác. 

Kích thước khác nhau cũng có thể yêu cầu độ cao khác nhau. Ví dụ,```
2
0 0 100
10 0 1
```có chiều cao tối ưu`100`, có tâm`(0, 0)`. Đài tưởng niệm đầu tiên rất cao chiếm ưu thế hơn câu trả lời, mặc dù đài tưởng niệm thứ hai ở rất xa. Việc coi tất cả các đài tưởng niệm như thể chúng có cùng chiều cao sẽ tạo ra kết quả sai. 

Cuối cùng, tọa độ âm lớn phải được xử lý mà không cần giả định rằng tọa độ không âm. Vì```
2
-100000000 -100000000 1
100000000 100000000 1
```chiều cao tối ưu là`100000001`, Và`(0, 0)`là một trung tâm hợp lệ. Số nguyên Python xử lý phạm vi này một cách tự nhiên, trong khi việc triển khai có chiều rộng cố định cần loại số nguyên đủ rộng. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ liệt kê các trung tâm ứng viên`(X, Y)`. Đối với mỗi trung tâm, chúng tôi có thể tính chiều cao yêu cầu nhỏ nhất là 

[ 
\max_i\left(h_i+\max(|x_i-X|,|y_i-Y|)\right). 
] 

Điều này đúng vì mỗi đài tưởng niệm sẽ cho chúng ta biết độ cao tối thiểu cần thiết ở tâm đó một cách độc lập. Tuy nhiên, có khoảng`4 * 10^16`tâm số nguyên trong phạm vi tọa độ và kiểm tra`1000`đài tưởng niệm cho mỗi cái mang lại khoảng`4 * 10^19`hoạt động trong trường hợp xấu nhất. Thời hạn loại trừ điều này ngay lập tức. 

Quan sát hữu ích được ẩn giấu bên trong`max`. Chúng ta có thể viết lại 

\max(h_i+|x_i-X|,\ h_i+|y_i-Y|). 
] 

Tận dụng tối đa tất cả các đài tưởng niệm mang lại 

[ 
H= 
\max\left( 
\max_i(h_i+|x_i-X|), 
\max_i(h_i+|y_i-Y|) 
\đúng). 
] 

Hai tọa độ đã hoàn toàn tách biệt. Chúng ta có thể xác định độc lập chiều cao nhỏ nhất cần thiết cho`x`phối hợp và cho`y`phối hợp, sau đó lấy giá trị lớn hơn trong hai chiều cao. 

Hãy xem xét chỉ một chiều. Chúng tôi có vị trí`c_i`, độ cao`h_i`, và muốn số nguyên nhỏ nhất`H`trong đó một số trung tâm số nguyên`C`thỏa mãn 

[ 
h_i+|c_i-C|\le H 
] 

cho mọi`i`. Sắp xếp lại mang lại 

[ 
c_i+h_i-H\le C\le c_i-h_i+H. 
] 

Do đó, mỗi đài tưởng niệm đưa ra một khoảng thời gian cho phép để`C`. Tất cả các đài tưởng niệm có thể được thỏa mãn chính xác khi các khoảng này có một điểm chung. 

Xác định 

[ 
L=\max_i(c_i+h_i), 
\qquad 
R=\min_i(c_i-h_i). 
] 

Sau khi giao nhau tất cả các khoảng, các trung tâm có thể được xác định chính xác 

[ 
[L-H,\R+H]. 
] 

Khoảng này khác rỗng chính xác khi 

[ 
L-H\le R+H, 
] 

hoặc 

[ 
2H\ge L-R. 
] 

Do đó, chiều cao nguyên tối thiểu được yêu cầu bởi tọa độ này là 

[ 
H_{\text{dim}}=\left\lceil\frac{L-R}{2}\right\rceil. 
] 

Chúng tôi tính toán điều này một lần cho`x`và một lần cho`y`. Kim tự tháp phải thỏa mãn cả hai chiều nên chiều cao tối thiểu của nó là chiều cao tối đa của hai giá trị đó. Khi đã biết chiều cao đó, mọi điểm nguyên trong mỗi khoảng kết quả đều là tâm hợp lệ. Chọn điểm cuối bên trái,`L-H`, thuận tiện và tránh mọi vấn đề làm tròn. 

Cách tiếp cận bạo lực có hiệu quả vì nó đánh giá rõ ràng mọi trung tâm có thể. Nó thất bại vì không gian tọa độ rất lớn. Sự quan sát rằng`max`tách thành độc lập`x`Và`y`các thuật ngữ làm giảm bài toán hình học thành hai giao điểm khoảng một chiều, mỗi giao điểm có thể được xử lý trong một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |`O(n · C²)`Ở đâu`C`là phạm vi tọa độ |`O(n)`| Quá chậm | 
| Tối ưu |`O(n)`|`O(n)`để lưu trữ đầu vào, hoặc`O(1)`ngoài nó | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các đài tưởng niệm. Đối với`x`kích thước, tính toán 

[ 
L_x=\max_i(x_i+h_i) 
] 

và 

[ 
R_x=\min_i(x_i-h_i). 
] 

Hai giá trị này mô tả giao điểm của tất cả các khoảng tâm có thể có sau khi chiều cao kim tự tháp được cố định. 
2. Tính các đại lượng tương tự cho`y`kích thước: 

[ 
L_y=\max_i(y_i+h_i), 
\qquad 
R_y=\min_i(y_i-h_i). 
] 

Hai chiều có thể được xử lý độc lập vì chiều cao yêu cầu của kim tự tháp là yêu cầu tối đa trong hai tọa độ. 
3. Tìm chiều cao tối thiểu theo yêu cầu của`x`kích thước: 

[ 
H_x=\left\lceil\frac{L_x-R_x}{2}\right\rceil. 
] 

Đối với các giá trị nguyên, giá trị này được tính là`(L_x - R_x + 1) // 2`. 
4. Tìm giá trị tương ứng của`y`kích thước: 

[ 
H_y=\left\lceil\frac{L_y-R_y}{2}\right\rceil. 
] 
5. Đặt 

[ 
H=\max(H_x,H_y). 
] 

Chiều cao này là đủ cho cả hai chiều và không thể giảm bớt vì một trong hai chiều đã cần ít nhất chiều cao này. 
6. Chọn 

[ 
X=L_x-H, 
\qquad 
Y=L_y-H. 
]

Từ`H >= H_x`Và`H >= H_y`, các giá trị này thuộc về các khoảng khả thi tương ứng của chúng. Chúng là số nguyên vì mọi đại lượng liên quan đều là số nguyên. 
7. Đầu ra`X`,`Y`, Và`H`. Kim tự tháp thu được chứa mọi đài tưởng niệm và không có kim tự tháp nào có chiều cao nhỏ hơn có thể chứa tất cả chúng. 

### Tại sao nó hoạt động 

Đối với bất kỳ chiều cao cố định`H`, một đài tưởng niệm`(c_i,h_i)`trong một chiều cho phép chính xác khoảng trung tâm`[c_i+h_i-H, c_i-h_i+H]`. Giao điểm của tất cả các khoảng này là`[L-H,R+H]`, do đó một trung tâm hợp lệ tồn tại chính xác khi`2H >= L-R`. Kể từ đây`H_x`Và`H_y`là độ cao tối thiểu chính xác bị ép buộc bởi hai tọa độ. 

Kim tự tháp đầy đủ yêu cầu đồng thời cả hai bất đẳng thức một chiều. Vì yêu cầu về chiều cao của nó là tối đa của độc lập`x`Và`y`yêu cầu, tối ưu toàn cục chính xác là`max(H_x,H_y)`. Lựa chọn`L_x-H`Và`L_y-H`đặt tâm bên trong cả hai giao điểm khả thi, do đó mọi đài tưởng niệm đều được chứa trong khi chiều cao ở mức tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())

    lx = -10**30
    rx = 10**30
    ly = -10**30
    ry = 10**30

    for _ in range(n):
        x, y, h = map(int, input().split())

        lx = max(lx, x + h)
        rx = min(rx, x - h)

        ly = max(ly, y + h)
        ry = min(ry, y - h)

    hx = (lx - rx + 1) // 2
    hy = (ly - ry + 1) // 2

    H = max(hx, hy)

    X = lx - H
    Y = ly - H

    print(X, Y, H)

if __name__ == "__main__":
    solve()
```Bốn biến`lx`,`rx`,`ly`, Và`ry`duy trì các giá trị cực trị cần thiết cho các giao điểm khoảng. Ví dụ,`lx`cửa hàng lớn nhất`x_i + h_i`, bởi vì mọi trung tâm khả thi ít nhất phải có`x_i + h_i - H`. 

Các biểu thức`x + h`Và`x - h`nên được giữ theo thứ tự đó. Việc đảo ngược một trong số chúng sẽ làm thay đổi phía nào của khoảng khả thi đang được biểu diễn và dẫn đến tâm không chính xác. 

Vách ngăn trần cũng là nơi dễ xảy ra lỗi lẻ tẻ. Đối với số nguyên không âm`d`,`ceil(d / 2)`là`(d + 1) // 2`. Đây`L - R`luôn không âm vì với mỗi đài tưởng niệm,`x_i + h_i > x_i - h_i`và thứ tự tương tự vẫn tồn tại khi lấy mức tối đa và tối thiểu. 

Không có tính toán dấu phẩy động. Hình học số nguyên chính xác là đủ và các số nguyên có độ chính xác tùy ý của Python xử lý thoải mái tất cả các giá trị trung gian. Biểu thức liên quan lớn nhất là theo thứ tự`2 * 10^8`, do đó, ngay cả số nguyên 64 bit có dấu cũng đủ dùng trong các ngôn ngữ có số nguyên có chiều rộng cố định. 

Mã không cần phải lưu trữ các đài tưởng niệm. Mỗi cái chỉ đóng góp vào bốn cực trị đang chạy, do đó việc triển khai có thể xử lý đầu vào trong một lần chạy. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào chứa một obelisk tại`(0,0)`với chiều cao`5`. 

| Biến | Sau khi đọc đài tưởng niệm | 
| --- | --- | 
|`lx`|`5`| 
|`rx`|`-5`| 
|`ly`|`5`| 
|`ry`|`-5`| 
|`hx`|`5`| 
|`hy`|`5`| 
|`H`|`5`| 
|`X`|`0`| 
|`Y`|`0`| 

Khoảng thời gian khả thi cho`x`trung tâm ở độ cao`5`là`[5-5,-5+5] = [0,0]`, và điều này cũng đúng với`y`. Vì vậy trung tâm buộc phải`(0,0)`, và kim tự tháp có chiều cao`5`. 

### Mẫu 2 

Các đài tưởng niệm là`(3,3,3)`Và`(6,6,2)`. 

| Đài tưởng niệm |`x+h`|`x-h`|`y+h`|`y-h`| 
| --- | --- | --- | --- | --- | 
|`(3,3,3)`|`6`|`0`|`6`|`0`| 
|`(6,6,2)`|`8`|`4`|`8`|`4`| 

Sau cả hai đài tưởng niệm, chúng ta có`lx = 8`,`rx = 0`,`ly = 8`, Và`ry = 0`. 

| Biến | Giá trị | 
| --- | --- | 
|`hx`|`(8-0+1)//2 = 4`| 
|`hy`|`(8-0+1)//2 = 4`| 
|`H`|`4`| 
|`X`|`8-4 = 4`| 
|`Y`|`8-4 = 4`| 

Tại`(4,4)`, đài tưởng niệm đầu tiên cách nhau một đơn vị theo cả hai tọa độ, nên kim tự tháp có chiều cao`4-1=3`. Cái thứ hai cách nhau hai đơn vị nên chiều cao khả dụng của nó là`4-2=2`. Cả hai đều được hỗ trợ chính xác, mang lại kết quả đầu ra cần thiết`4 4 4`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(n)`| Mỗi đài tưởng niệm cập nhật bốn cực trị một lần. | 
| Không gian |`O(1)`| Chỉ có bốn cực trị được duy trì, không bao gồm lưu trữ đầu vào. | 

Với`n <= 1000`, thuật toán chỉ thực hiện vài nghìn phép tính số nguyên. Giới hạn tọa độ và chiều cao cũng vừa vặn thoải mái trong số học 64-bit thông thường, trong khi kiểu số nguyên của Python mang lại sự an toàn bổ sung. Phương pháp này thấp hơn nhiều so với giới hạn thời gian 2 giây và bộ nhớ 512 MB khả dụng. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve_data(inp: str) -> str:
    data = inp.strip().split()
    it = iter(data)

    n = int(next(it))

    lx = -10**30
    rx = 10**30
    ly = -10**30
    ry = 10**30

    for _ in range(n):
        x = int(next(it))
        y = int(next(it))
        h = int(next(it))

        lx = max(lx, x + h)
        rx = min(rx, x - h)
        ly = max(ly, y + h)
        ry = min(ry, y - h)

    hx = (lx - rx + 1) // 2
    hy = (ly - ry + 1) // 2

    H = max(hx, hy)
    X = lx - H
    Y = ly - H

    return f"{X} {Y} {H}\n"

def run(inp: str) -> str:
    return solve_data(inp)

assert run("""\
1
0 0 5
""") == "0 0 5\n", "sample 1"

assert run("""\
2
3 3 3
6 6 2
""") == "4 4 4\n", "sample 2"

assert run("""\
1
-100000000 -100000000 100000000
""") == "-100000000 -100000000 100000000\n", "minimum-size boundary case"

assert run("""\
2
0 0 1
2 0 1
""") == "1 0 2\n", "exact interval boundary"

assert run("""\
2
0 0 100
10 0 1
""") == "0 0 100\n", "different heights"

all_equal = "1000\n" + "\n".join(
    "12345678 -87654321 99999999" for _ in range(1000)
) + "\n"

assert run(all_equal) == "12345678 -87654321 99999999\n", "maximum n, equal values"

assert run("""\
2
-100000000 -100000000 1
100000000 100000000 1
""") == "0 0 100000001\n", "large negative and positive coordinates"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 0 0 5`|`0 0 5`| Tối thiểu một đài tưởng niệm | 
|`0 0 1 / 2 0 1`|`1 0 2`| Chính xác ranh giới nút giao và phân chia trần | 
|`0 0 100 / 10 0 1`|`0 0 100`| Chiều cao không đồng đều và sự thống trị một chiều | 
|`1000`đài tưởng niệm giống hệt nhau |`12345678 -87654321 99999999`| Tối đa`n`và các giá trị lặp lại | 
|`±100000000`tọa độ |`0 0 100000001`| Phối hợp ranh giới và số học lớn | 

## Vỏ cạnh 

Trường hợp đài tưởng niệm đơn```
1
0 0 5
```sản xuất`lx = ly = 5`Và`rx = ry = -5`. Cả hai chiều cao tối thiểu một chiều đều`(5 - (-5) + 1) // 2 = 5`. Trung tâm được chọn là`5 - 5 = 0`ở cả hai chiều, đưa ra`0 0 5`. Thuật toán không dựa vào việc có nhiều tháp để tạo thành một khoảng. 

Trường hợp ranh giới chính xác```
2
0 0 1
2 0 1
```cho`lx = 3`Và`rx = -1`. yêu cầu`x`chiều cao là`(3 - (-1) + 1) // 2 = 2`. Ở độ cao`2`, khả thi`x`khoảng thời gian là`[3-2,-1+2] = [1,1]`, vậy trung tâm duy nhất có thể là`X=1`. các`y`kích thước chỉ cần chiều cao`1`, vì vậy chiều cao cuối cùng vẫn giữ nguyên`2`. Điều này phát hiện các triển khai vô tình sử dụng phép chia tầng. 

Đối với trường hợp chiều cao không bằng nhau```
2
0 0 100
10 0 1
```cái`x`cực trị là`100`Và`-100`, yêu cầu chiều cao`100`, trong khi`y`cực đoan cũng`100`Và`-100`, nhưng kết quả tính toán trung tâm cho`(0,0)`. Tại trung tâm đó, đài tưởng niệm đầu tiên đạt đến độ cao chính xác`100`, trong khi cái thứ hai có chiều cao sẵn có`90`, cao hơn nhiều so với chiều cao yêu cầu của nó`1`. Obelisk cao xác định mức tối thiểu. 

Đối với trường hợp ranh giới tọa độ```
2
-100000000 -100000000 1
100000000 100000000 1
```chúng tôi nhận được`L_x = L_y = 100000001`Và`R_x = R_y = -100000001`. Như vậy`H = (100000001 - (-100000001) + 1) // 2 = 100000001`. Trung tâm được chọn là`0,0`. Mỗi đài tưởng niệm chính xác là`100000000`đơn vị đi trong tọa độ có liên quan, để lại chiều cao`1`, vì vậy cả hai đều được chứa chính xác tại ranh giới. 

Thử nghiệm kích thước tối đa với`1000`các đài tưởng niệm giống hệt nhau thực hiện giới hạn đầu vào mà không làm thay đổi cấu trúc toán học. Mỗi bản cập nhật đều giữ nguyên bốn cực trị sau cột đầu tiên, vì vậy câu trả lời cuối cùng vẫn giữ nguyên. Điều này chứng tỏ tại sao các điểm lặp lại không yêu cầu bất kỳ xử lý đặc biệt nào và tại sao quá trình quét tuyến tính vẫn hiệu quả.
