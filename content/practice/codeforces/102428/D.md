---
title: "CF 102428D - Ngôi sao rực rỡ"
description: "Mỗi ngôi sao có một vị trí cố định (X, Y) và độ sáng B. Bernie có thể xoay toàn bộ bức ảnh, sau đó máy in sẽ quét từ trên xuống dưới. Sau khi quay, một ngôi sao có tọa độ Y biến đổi lớn hơn sẽ được in trước đó."
date: "2026-08-10T08:36:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102428
codeforces_index: "D"
codeforces_contest_name: "2019-2020 ACM-ICPC Latin American Regional Programming Contest"
rating: 0
weight: 102428
solve_time_s: 214
verified: true
draft: false
---

[CF 102428D - Những ngôi sao rực rỡ](https://codeforces.com/problemset/problem/102428/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 34 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi ngôi sao có một vị trí cố định`(X, Y)`và độ sáng`B`. Bernie có thể xoay toàn bộ bức ảnh, sau đó máy in sẽ quét từ trên xuống dưới. Sau khi quay, một ngôi sao có chuyển dạng lớn hơn`Y`tọa độ được in trước đó. Những ngôi sao có sự biến đổi giống nhau`Y`tọa độ được in đồng thời. 

Sự quay phải làm cho mọi ngôi sao sáng hơn có vẻ không thấp hơn mọi ngôi sao mờ hơn. Nếu sao`i`Và`j`có`B_i > B_j`, điều kiện cần là`Y'_i >= Y'_j`. 

Tâm xoay không quan trọng. Dịch mọi ngôi sao theo cùng một vectơ sẽ thay đổi mỗi lần chuyển đổi`Y`với cùng một lượng, do đó chỉ có hướng của trục tung mới là quan trọng. 

Chúng ta có thể mô tả hướng đó bằng một vectơ đơn vị`d`. Tọa độ thẳng đứng được biến đổi của một điểm`p`là hình chiếu của nó lên`d`, cụ thể là`p · d`. Như vậy toàn bộ vấn đề trở thành việc tìm ra hướng đi`d`như vậy`(p_i - p_j) · d >= 0`cho mỗi cặp với`B_i > B_j`. 

Có nhiều nhất`1000`sao, vì vậy có thể có nhiều nhất`1000 * 999 / 2 = 499500`cặp. MỘT`O(N^2)`thuật toán là thực tế vì điều này có nghĩa là chỉ có khoảng nửa triệu ràng buộc về cặp. MỘT`O(N^3)`Cách tiếp cận này sẽ thực hiện khoảng một tỷ thao tác cơ bản, trong khi một thuật toán liên tục kiểm tra tất cả các cặp cho nhiều phép quay ứng cử viên có thể đạt được khoảng`O(N^4)`và nó quá lớn. 

Một số chi tiết rất dễ xử lý sai. Đầu tiên, độ sáng bằng nhau không yêu cầu đặt hàng. Ví dụ,```
3
0 0 2
10 0 2
0 10 1
```có câu trả lời`Y`. Hai ngôi sao có độ sáng-2 có thể được in cùng nhau và một hướng phù hợp có thể đặt cả hai lên trên ngôi sao có độ sáng-1. Một giải pháp xử lý độ sáng bằng nhau theo yêu cầu thứ tự nghiêm ngặt sẽ từ chối trường hợp này một cách không chính xác. 

Thứ hai, sự bình đẳng trong phép chiếu được cho phép. Coi như```
3
1 0 2
-1 0 2
0 0 1
```Hướng hữu ích duy nhất là theo chiều dọc. Cả ba ngôi sao sau đó đều có sự biến đổi giống nhau`Y`, do đó, hai ngôi sao sáng hơn được in đồng thời với ngôi sao tối hơn, điều này được cho phép. Câu trả lời là`Y`. Sử dụng một cách nghiêm ngặt`>`thay vì`>=`sẽ sản xuất không chính xác`N`. 

Thứ ba, tất cả các ngôi sao có thể có độ sáng như nhau. Ví dụ,```
3
0 0 7
1 2 7
4 -3 7
```có câu trả lời`Y`, vì không có sự so sánh độ sáng nào cả. Mọi vòng quay đều hợp lệ. 

Cuối cùng, một điểm có thể nằm bên trong bao lồi của các điểm sáng hơn hoặc tối hơn và khiến câu trả lời là không thể. Ví dụ,```
4
0 0 1
2 0 1
0 2 1
1 1 2
```có câu trả lời`N`. Ngôi sao độ sáng-2 nằm hoàn toàn bên trong tam giác được hình thành bởi ba ngôi sao độ sáng-1. Không có phép chiếu tuyến tính nào có thể làm cho một điểm bên trong cao hơn mọi đỉnh. Một phương pháp chỉ kiểm tra một vài ngôi sao lân cận có thể bỏ sót mâu thuẫn toàn cầu này. 

## Phương pháp tiếp cận 

Một phương pháp cưỡng bức trực tiếp có thể được xây dựng từ hình học của các ràng buộc. Mỗi cặp sao có độ sáng khác nhau sẽ cho một tập hợp các hướng quay hợp lệ. Giao của tất cả các tập hợp đó chính là tập hợp các phép quay thỏa mãn bài toán. Vì mỗi ràng buộc có hai hướng biên, nên người ta có thể tạo ra mọi hướng biên làm ứng cử viên và kiểm tra mọi ràng buộc độ sáng theo hướng đó. Nếu có`K`các cặp liên quan thì có nhiều nhất`2K`ranh giới ứng cử viên và kiểm tra một ứng cử viên với tất cả các chi phí ràng buộc`O(K)`. Với`K = 499500`, điều này có thể yêu cầu khoảng`2K^2`, gần với`5 * 10^11`kiểm tra, tốc độ này quá chậm. 

Lý do mà vũ lực hoạt động về mặt khái niệm là vì vấn đề thực sự là sự giao nhau của các phạm vi góc. Sai lầm là tính toán lại toàn bộ giao lộ từ đầu cho mọi ranh giới có thể. 

Quan sát quan trọng là một cặp sao không đưa ra một hạn chế tùy ý nào đối với chuyển động quay. Hãy để ngôi sao sáng hơn`A`, ngôi sao càng tối`C`, và xác định`v = A - C`. 

Để có một hướng đi`d`, ngôi sao sáng hơn được in không thấp hơn ngôi sao tối hơn một cách chính xác khi`v · d >= 0`. 

Nếu như`a`là góc của`v`Và`θ`là góc của`d`, điều này trở thành`cos(θ - a) >= 0`. 

Vì vậy, các hướng hợp lệ tạo thành một hình bán nguyệt khép kín:`a - π/2 <= θ <= a + π/2`. 

Do đó, mọi ràng buộc về độ sáng đều chính xác là một khoảng góc có chiều rộng`π`. 

Vấn đề bây giờ là giao nhau tới khoảng nửa triệu hình bán nguyệt khép kín. Giao lộ có thể được duy trì trực tiếp. Chúng tôi giữ khoảng thời gian khả thi hiện tại, mở nó ra trên dòng số thực và với mỗi hình bán nguyệt mới, hãy chọn bản sao được dịch chuyển theo bội số của`2π`đó là gần nhất với khoảng thời gian hiện tại. Vì khoảng khả thi hiện tại có độ dài tối đa`π`, bản sao này chứa mọi phần trùng lặp có thể có với khoảng thời gian hiện tại. Sau đó chúng ta giao nhau giữa hai khoảng thông thường. 

Điều này tránh việc sắp xếp tất cả các điểm cuối trong khoảng thời gian và tránh việc kiểm tra mọi ứng cử viên theo mọi ràng buộc. Thuật toán kết quả là`O(N^2)`thời gian và`O(1)`thêm không gian ngoài đầu vào. Công thức ràng buộc góc theo cặp tương tự cũng được mô tả trong phần thảo luận của cuộc thi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N^4) | O(N^2) | Quá chậm | 
| Tối ưu | O(N^2) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các ngôi sao và lưu trữ tọa độ cũng như độ sáng của chúng. Các tọa độ này đủ để xác định mọi ràng buộc hình học, trong khi độ sáng cho chúng ta biết hướng mà vectơ hiệu phải chỉ. 
2. Kiểm tra từng cặp sao riêng biệt. Nếu giá trị độ sáng của chúng bằng nhau, hãy bỏ qua cặp vì cho phép một trong hai thứ tự in. Mặt khác, định hướng cặp từ ngôi sao sáng hơn về phía ngôi sao tối hơn và gọi vectơ đó là`v`. 
3. Tính góc`a = atan2(v_y, v_x)`. Hướng thẳng đứng ứng viên có góc`θ`thỏa mãn cặp chính xác khi`v · d >= 0`, tương đương với`θ`thuộc khoảng đóng`[a - π/2, a + π/2]`. 
4. Sử dụng ràng buộc không cần thiết đầu tiên để khởi tạo khoảng khả thi hiện tại`[L, R]`. Chúng tôi cố tình không mở khoảng thời gian này để các điểm cuối của nó có thể nằm ngoài`[0, 2π)`. 
5. Đối với mỗi ràng buộc tiếp theo, hãy xem xét các bản sao định kỳ của nó`[l + 2kπ, r + 2kπ]`. Chọn bản sao có tâm gần nhất với tâm của`[L, R]`. Bởi vì cả khoảng thời gian hiện tại và mọi khoảng thời gian ràng buộc đều có độ dài tối đa`π`, bất kỳ bản sao nào có thể giao nhau với khoảng hiện tại đều phải có tâm gần nó nhất. Cắt bản sao đã chọn với`[L, R]`. 
6. Nếu giao lộ mới trống, hãy in`N`ngay lập tức. Không thể có một phép quay hợp lệ vì mọi hướng hợp lệ sẽ phải thuộc về cả hai khoảng. 
7. Sau khi tất cả các cặp đã được xử lý, nếu khoảng khả thi vẫn khác trống, hãy in`Y`. Nếu tất cả các giá trị độ sáng bằng nhau thì không có ràng buộc nào được tạo ra, do đó mọi hướng đều hợp lệ và chúng tôi cũng in`Y`. 

### Tại sao nó hoạt động 

Tính bất biến đó là`[L, R]`biểu thị chính xác tập hợp các hướng quay thỏa mãn mọi ràng buộc về độ sáng được xử lý cho đến nay, với không gian góc tròn được mở trên đường thực. 

Đối với một cặp sao, bất đẳng thức`v · d >= 0`chính xác là một hình bán nguyệt khép kín có các hướng hợp lệ. Việc giao tập hợp khả thi hiện tại với hình bán nguyệt đó sẽ loại bỏ chính xác các hướng vi phạm cặp mới. Việc chọn bản sao định kỳ gần nhất sẽ không làm mất bất kỳ giải pháp khả thi nào vì khoảng thời gian hiện tại có độ dài tối đa`π`và hai hình bán nguyệt chỉ có thể chồng lên nhau khi tâm của chúng lớn nhất là`π`riêng biệt. Do đó, khoảng thời gian được duy trì không trống sau tất cả các ràng buộc chính xác khi tồn tại một phép quay hợp lệ. 

Việc sử dụng các khoảng thời gian khép kín cũng xử lý các ngôi sao trở nên thẳng hàng theo chiều ngang sau khi quay. Sự bình đẳng như vậy được tuyên bố cho phép, do đó, một khoảng khả thi bao gồm một hướng duy nhất phải được chấp nhận. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from math import atan2, pi

def solve():
    input = sys.stdin.readline

    n = int(input())
    stars = [tuple(map(int, input().split())) for _ in range(n)]

    half_pi = pi / 2.0
    two_pi = 2.0 * pi
    eps = 1e-12

    left = None
    right = None

    for i in range(n):
        xi, yi, bi = stars[i]

        for j in range(i):
            xj, yj, bj = stars[j]

            if bi == bj:
                continue

            if bi > bj:
                dx = xi - xj
                dy = yi - yj
            else:
                dx = xj - xi
                dy = yj - yi

            angle = atan2(dy, dx)
            l = angle - half_pi
            r = angle + half_pi

            if left is None:
                left = l
                right = r
                continue

            mid = (left + right) * 0.5

            # Shift the new semicircle by a multiple of 2*pi
            # so that its center is closest to the current interval.
            k = round((mid - angle) / two_pi)
            l += k * two_pi
            r += k * two_pi

            new_left = max(left, l)
            new_right = min(right, r)

            if new_left > new_right + eps:
                print("N")
                return

            left = new_left
            right = new_right

    print("Y")

if __name__ == "__main__":
    solve()
```Đầu vào được lưu trữ dưới dạng bộ ba`(x, y, b)`. Các vòng lặp lồng nhau kiểm tra từng cặp không có thứ tự chính xác một lần, do đó không có cặp nào có thể bị trùng lặp một cách vô tình. 

Đối với mỗi cặp, mã đầu tiên bỏ qua độ sáng bằng nhau. Ngược lại, nó làm cho vectơ sai phân hướng từ ngôi sao tối hơn về phía ngôi sao sáng hơn. Hướng này quan trọng vì việc đảo ngược vectơ sẽ đảo ngược nửa mặt phẳng cần thiết.`atan2`cho biết hướng của vectơ đó. Trừ và cộng`π/2`đưa ra hai ranh giới của hình bán nguyệt hợp lệ. Khoảng thời gian này không được cố ý chuẩn hóa thành`[0, 2π)`, bởi vì khoảng không được bao bọc sẽ dễ giao nhau hơn nhiều. 

biểu thức```
k = round((mid - angle) / two_pi)
```chọn bản sao định kỳ của khoảng mới có tâm gần nhất với tâm của khoảng khả thi hiện tại. con trăn`round`hành vi ở một nửa số nguyên chính xác là vô hại ở đây, bởi vì một trong hai bản sao gần giống nhau đều cho cùng một giao điểm ranh giới có thể có. 

Việc so sánh sử dụng một epsilon nhỏ vì thuật toán thực hiện số học góc với các số dấu phẩy động. Tọa độ được giới hạn bởi`10^4`, do đó các vectơ hiệu số nguyên được giới hạn bởi`2 * 10^4`ở mỗi tọa độ. Các hướng nguyên riêng biệt không thể đóng tùy ý, tạo ra dung sai`10^-12`nhỏ hơn một cách an toàn so với các khoảng cách hình học quan trọng. 

Không thể tràn số nguyên trong Python và trên thực tế việc triển khai không bao giờ cần số học số nguyên lớn. Các phép toán số duy nhất là`atan2`, phép cộng, phép chia và so sánh các góc. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Các cặp liên quan là các cặp có độ sáng khác nhau. Bảng sử dụng độ để làm cho hình học dễ đọc hơn. Việc triển khai thực tế hoạt động theo radian. 

| Cặp, sáng hơn đến tối hơn | Vectơ | Khoảng thời gian hợp lệ | Giao lộ hiện tại | 
| --- | --- | --- | --- | 
| 2 ăn 1 |`(1,-3)`|`[-161.565°, 18.435°]`|`[-161.565°, 18.435°]`| 
| 3 ăn 1 |`(3,1)`|`[-71.565°, 108.435°]`|`[-71.565°, 18.435°]`| 
| 3 đến 2 |`(2,4)`|`[-26.565°, 153.435°]`|`[-26.565°, 18.435°]`| 
| 3 đến 4 |`(-1,3)`|`[18.435°, 198.435°]`|`[18.435°, 18.435°]`| 
| 4 đến 1 |`(4,-2)`|`[-116.565°, 63.435°]`|`[18.435°, 18.435°]`| 

Tập khả thi cuối cùng là một hướng duy nhất, xấp xỉ`18.435°`. Một hướng duy nhất là đủ vì sự bất bình đẳng không nghiêm ngặt. Theo hướng đó, một số ngôi sao có giá trị độ sáng khác nhau nằm trên cùng một đường in, điều này được cho phép rõ ràng. Thuật toán giữ khoảng rộng bằng 0 thay vì loại bỏ nó, do đó kết quả là`Y`. 

### Mẫu 2 

Một lần nữa, các góc được thể hiện bằng độ. 

| Cặp, sáng hơn đến tối hơn | Vectơ | Khoảng thời gian hợp lệ | Giao lộ hiện tại | 
| --- | --- | --- | --- | 
| 5 ăn 1 |`(3,-4)`|`[-143.130°, 36.870°]`|`[-143.130°, 36.870°]`| 
| 5 đến 2 |`(1,-4)`|`[-165.964°, 14.036°]`|`[-143.130°, 14.036°]`| 
| 5 đến 3 |`(0,-7)`|`[-180°, 0°]`|`[-143.130°, 0°]`| 
| 5 đến 4 |`(-1,-4)`|`[-194.036°, -14.036°]`|`[-143.130°, -14.036°]`| 
| 1 đến 2 |`(-2,0)`| chuyển sang`[-270°,-90°]`|`[-143.130°, -90°]`| 
| 1 đến 3 |`(-3,-3)`|`[-225°,-45°]`|`[-143.130°, -90°]`| 
| 4 đến 2 |`(2,0)`|`[-90°,90°]`|`[-90°,-90°]`| 
| 4 đến 3 |`(1,-3)`|`[-161.565°,18.435°]`|`[-90°,-90°]`| 
| 2 đến 3 |`(-1,-3)`|`[-198.435°,-18.435°]`|`[-90°,-90°]`| 

Bước quan trọng là ghép cặp từ sao 1 đến sao 2. Khoảng cách tự nhiên của nó tập trung ở`180°`, nhưng khoảng thời gian khả thi hiện tại là khoảng`-100°`, do đó thuật toán dịch chuyển khoảng đó bằng`-360°`. Đây chính xác là lý do tại sao các khoảng phải được coi là bản sao định kỳ thay vì được chuẩn hóa độc lập. 

Tập khả thi cuối cùng lại là một hướng duy nhất,`-90°`. Giao lộ không bao giờ vắng nên câu trả lời là`Y`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N2) | Mỗi cặp không có thứ tự được xử lý một lần, với công việc không đổi trên mỗi cặp. | 
| Không gian | O(N) | Chỉ các ngôi sao đầu vào và số lượng biến góc không đổi được lưu trữ. | 

Vì`N <= 1000`, có nhiều nhất`499500`cặp. Thuật toán thực hiện một`atan2`và một vài phép tính số học cho mỗi cặp có liên quan, không có sự sắp xếp và không có tập hợp các khoảng có kích thước bậc hai. Điều này giữ cho cả thời gian chạy và mức sử dụng bộ nhớ trong giới hạn dự định. 

## Trường hợp thử nghiệm```python
import sys
import io
from math import atan2, pi

def solve():
    input = sys.stdin.readline

    n = int(input())
    stars = [tuple(map(int, input().split())) for _ in range(n)]

    half_pi = pi / 2.0
    two_pi = 2.0 * pi
    eps = 1e-12

    left = None
    right = None

    for i in range(n):
        xi, yi, bi = stars[i]

        for j in range(i):
            xj, yj, bj = stars[j]

            if bi == bj:
                continue

            if bi > bj:
                dx = xi - xj
                dy = yi - yj
            else:
                dx = xj - xi
                dy = yj - yi

            angle = atan2(dy, dx)
            l = angle - half_pi
            r = angle + half_pi

            if left is None:
                left = l
                right = r
                continue

            mid = (left + right) * 0.5
            k = round((mid - angle) / two_pi)

            l += k * two_pi
            r += k * two_pi

            left = max(left, l)
            right = min(right, r)

            if left > right + eps:
                print("N")
                return

    print("Y")

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output

    try:
        solve()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

    return output.getvalue()

# Provided sample 1
assert run("""\
4
0 2 1
1 -1 2
3 3 5
4 0 2
""") == "Y\n", "sample 1"

# Provided sample 2
assert run("""\
5
0 4 6
2 4 5
3 7 2
4 4 6
3 0 8
""") == "Y\n", "sample 2"

# Provided sample 3
assert run("""\
4
-1 2 5
0 0 2
3 4 1
4 2 4
""") == "N\n", "sample 3"

# Custom 1: minimum N, all brightness equal
assert run("""\
3
0 0 7
1 2 7
4 -3 7
""") == "Y\n", "all equal brightness"

# Custom 2: minimum N, different brightness values
assert run("""\
3
0 0 1
1 0 2
0 1 3
""") == "Y\n", "minimum-size instance"

# Custom 3: feasible set is exactly one direction
assert run("""\
3
1 0 2
-1 0 2
0 0 1
""") == "Y\n", "single-direction boundary case"

# Custom 4: maximum N, all points collinear and brightness increases with x
max_case = "1000\n" + "\n".join(
    f"{x} 0 {x + 1}" for x in range(1000)
) + "\n"

assert run(max_case) == "Y\n", "maximum-size instance"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Ba sao có độ sáng bằng nhau |`Y`| Không có ràng buộc nào được tạo ra. | 
| Ba ngôi sao với độ sáng`1, 2, 3`|`Y`| Kích thước đầu vào tối thiểu được phép và các ràng buộc cặp thông thường. | 
| Hai ngôi sao sáng như nhau xung quanh một tâm tối hơn |`Y`| Khoảng khả thi có độ rộng bằng 0 và các bất đẳng thức không nghiêm ngặt. | 
| 1000 ngôi sao thẳng hàng với độ sáng ngày càng tăng |`Y`| Kích thước đầu vào tối đa và đầy đủ`O(N²)`vòng lặp cặp. | 

## Vỏ cạnh 

### Độ sáng bằng nhau 

cho```
3
0 0 7
1 2 7
4 -3 7
```mọi cặp đều có độ sáng bằng nhau nên thuật toán sẽ bỏ qua từng cặp. Khoảng khả thi vẫn chưa được đặt, có nghĩa là hệ thống ràng buộc trống. Thuật toán in`Y`, xử lý chính xác mọi phép quay là hợp lệ. 

### In đồng thời 

cho```
3
1 0 2
-1 0 2
0 0 1
```hai ngôi sao có độ sáng-2 tạo ra các ràng buộc đòi hỏi hướng phải có cả không âm và không dương`x`thành phần. Giao điểm của chúng chính xác là hướng thẳng đứng. Khoảng thời gian được duy trì cuối cùng có điểm cuối bên trái và bên phải bằng nhau, thay vì trở nên trống rỗng. Thuật toán in`Y`. 

Trường hợp này đặc biệt hữu ích để nắm bắt một triển khai sử dụng các so sánh nghiêm ngặt. Vấn đề này cho phép một ngôi sao sáng hơn và một ngôi sao tối hơn chia sẻ cùng một biến đổi`Y`. 

### Nội thất không thể tối đa 

cho```
4
0 0 1
2 0 1
0 2 1
1 1 2
```ngôi sao độ sáng-2 nằm bên trong tam giác được tạo thành bởi ba ngôi sao độ sáng-1. Mọi hướng đều chiếu điểm bên trong vào giữa các hình chiếu cực trị của tam giác nên ít nhất nó không thể cao bằng cả ba đỉnh. Các ràng buộc hình bán nguyệt theo cặp có giao điểm trống và thuật toán cuối cùng tạo ra`left > right`, sản xuất`N`. 

### Khoảng thời gian bao quanh 

Một khoảng như`[90°,270°]`có thể cần phải được đại diện như`[-270°,-90°]`khi khoảng khả thi hiện tại nằm gần`-100°`. Thuật toán xử lý việc này bằng cách cộng bội số thích hợp của`2π`thay vì buộc mọi khoảng thời gian vào`[0,2π)`. Nếu không có bước này, giao lộ hợp lệ băng qua đường`0°/360°`ranh giới có thể được báo cáo không chính xác là trống. 

Nếu bạn muốn, tôi cũng có thể biến nó thành một bài xã luận kiểu Codeforces nhỏ gọn hơn, giữ nguyên bằng chứng và cách triển khai nhưng giảm mức trình bày khoảng 40%.
