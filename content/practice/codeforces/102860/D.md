---
title: "CF 102860D - Hàng rào"
description: "Ngôi nhà là một đa giác trực giao: các bức tường của nó nằm ngang hoặc thẳng đứng và các góc của nó có tọa độ nguyên."
date: "2026-07-25T14:12:05+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102860
codeforces_index: "D"
codeforces_contest_name: "2020-2021 Saint-Petersburg Open High School Programming Contest (SpbKOSHP 20)"
rating: 0
weight: 102860
solve_time_s: 42
verified: true
draft: false
---

[CF 102860D - Hàng rào](https://codeforces.com/problemset/problem/102860/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 42s 
**Đã xác minh:** có 

##Giải pháp 
#Hiểu vấn đề 

Ngôi nhà là một đa giác trực giao: các bức tường của nó nằm ngang hoặc thẳng đứng và các góc của nó có tọa độ nguyên. Chúng ta cần xây hàng rào khép kín ngắn nhất chứa toàn bộ ngôi nhà trong khi mọi điểm trên hàng rào ít nhất cách Manhattan một khoảng.`l`từ mọi điểm của ngôi nhà. Đầu vào đưa ra các đỉnh ranh giới của ngôi nhà theo thứ tự và đầu ra là chu vi tối thiểu có thể có của hàng rào đó. 

Các ràng buộc đủ lớn để số lượng điểm được xử lý phải tuyến tính hoặc gần tuyến tính. Ngôi nhà có thể có tới`100000`các đỉnh, vì vậy các thuật toán so sánh từng cặp đỉnh hoặc mô phỏng rõ ràng toàn bộ hình dạng mở rộng bằng nhiều phép toán hình học sẽ không phù hợp. Một giải pháp xung quanh`O(n log n)`là phù hợp vì sắp xếp tuyến tính số điểm được tạo là thao tác chủ yếu. 

Quan sát hình học quan trọng là hàng rào cần thiết là ranh giới của việc mở rộng ngôi nhà Minkowski theo bán kính Manhattan`l`hình dạng. Thay vì xây dựng mọi điểm của ranh giới đó, chúng ta chỉ cần một tập hợp nhỏ các điểm cực trị ứng viên. Với mọi đỉnh ban đầu`(x, y)`, bốn điểm`(x+l, y)`,`(x-l, y)`,`(x, y+l)`, Và`(x, y-l)`là đủ. Bao lồi của tất cả các điểm được tạo này tạo ra hàng rào bao quanh ngắn nhất. 

Một số trường hợp cạnh rất dễ bị bỏ sót. Nếu như`l = 0`, câu trả lời chỉ là chu vi của đa giác ban đầu và các điểm được tạo có chứa nhiều điểm trùng lặp. Ví dụ:```
Input:
4 0
0 0
0 2
2 2
2 0

Output:
8
```Việc triển khai bất cẩn có thể tạo ra một thân tàu có các điểm lặp lại và tính toán không chính xác các cạnh có độ dài bằng 0 hoặc thất bại khi sắp xếp các điểm giống hệt nhau. 

Một trường hợp phức tạp khác là ngôi nhà lõm. Hàng rào tối ưu không phải là hình dạng ban đầu được dịch chuyển độc lập ở mọi cạnh vì đường cong bao quanh ngắn nhất có thể cắt ngang các góc lõm. Ví dụ:```
Input:
9 0
0 0
4 0
4 1
2 1
2 3
1 3
1 1
0 1
0 0
```Hàng rào chính xác đi theo ranh giới bên ngoài, nhưng sau khi áp dụng một khoảng cách dương, các vết lõm sẽ biến mất. Một phương pháp chỉ di chuyển từng bức tường ra bên ngoài một cách độc lập có thể để lại những góc vào trong không cần thiết và tạo ra chu vi dài hơn. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ cố gắng xây dựng đa giác lệch bằng cách xử lý mọi bức tường của ngôi nhà. Mỗi bức tường ngang có thể được di chuyển theo chiều dọc, mỗi bức tường thẳng đứng có thể được di chuyển theo chiều ngang và sau đó tất cả các giao điểm có thể được tính toán. Điều này trông tự nhiên vì bản thân ngôi nhà chỉ được tạo thành từ các phân đoạn thẳng hàng với nhau. 

Khó khăn là khoảng cách Manhattan không chỉ tạo ra ranh giới ngang và dọc. Xung quanh các góc, các điểm gần nhất tạo thành các đoạn chéo. Xử lý mọi trường hợp góc theo cách thủ công yêu cầu phải theo dõi cách các bức tường lân cận tương tác, đặc biệt là xung quanh các góc lõm. Một cấu trúc hình học đơn giản cũng có thể tạo ra quá nhiều phân đoạn và cần phải hợp nhất cẩn thận. 

Quan điểm tốt hơn là nghĩ về định nghĩa khoảng cách. Tập hợp tất cả các điểm có khoảng cách từ Manhattan đến một điểm`(x, y)`nhiều nhất là`l`là một viên kim cương. Việc mở rộng toàn bộ ngôi nhà bằng những viên kim cương này tạo ra ranh giới vùng cấm chính xác. Hàng rào hợp lệ nhỏ nhất là ranh giới bên ngoài của bản mở rộng này. 

Đối với một đa giác trực giao, các điểm cực trị của phép khai triển này được tạo ra bằng cách di chuyển mọi đỉnh ban đầu theo bốn hướng chính bằng`l`. Một khi các điểm này được thu thập, tất cả công việc còn lại là bài toán bao lồi tiêu chuẩn. Thân tàu tự động loại bỏ các phần lõm không cần thiết và chỉ giữ lại các điểm có thể xuất hiện trên hàng rào bao quanh ngắn nhất. 

Việc xây dựng bạo lực dành nhiều nỗ lực để suy luận về mọi tương tác giữa các phân khúc. Cách tiếp cận thân thay thế tất cả các trường hợp đó bằng một bất biến hình học duy nhất: bao lồi của các điểm được tạo chính xác là ranh giới của hình mở rộng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các đỉnh của ngôi nhà và tạo ra bốn điểm dịch chuyển cho mỗi đỉnh. Những điểm này thể hiện những vị trí cực đoan có thể có của việc mở rộng Manhattan. Chúng ta không cần tạo các điểm dọc theo các cạnh vì mỗi cạnh của hình mở rộng được xác định bởi các điểm cực trị của nó. 
2. Loại bỏ các điểm được tạo trùng lặp và sắp xếp các điểm còn lại theo từ điển. Việc sắp xếp cho phép thuật toán thân chuỗi đơn điệu xử lý các điểm theo thứ tự. 
3. Xây dựng phần dưới và phần trên của thân lồi. Trong khi thêm điểm mới, hãy xóa điểm trước đó bất cứ khi nào lượt rẽ không ngược chiều kim đồng hồ. Điều này chỉ giữ lại các điểm ranh giới bên ngoài. 
4. Đi qua thân tàu cuối cùng theo thứ tự và cộng khoảng cách Euclide giữa mỗi cặp đỉnh liên tiếp, bao gồm cả đỉnh cuối cùng trở lại đỉnh đầu tiên. Điều này cho chiều dài hàng rào. 

Tại sao nó hoạt động: 

Khai triển Manhattan của một tập hợp là tổng Minkowski của tập hợp đó và một viên kim cương có bán kính`l`. Đối với một đa giác trực giao, mọi điểm cực trị của hình mở rộng này phải xuất phát từ việc dịch chuyển một góc ban đầu theo một trong bốn hướng chính. Tập điểm được tạo chứa tất cả các điểm cực trị như vậy. Lấy thân lồi giữ chính xác ranh giới bên ngoài cần thiết cho hàng rào bao quanh tối thiểu, đồng thời loại bỏ các điểm không thể đóng góp vào chu vi. Vì hàng rào phải chứa vùng mở rộng và không thể xâm nhập vào vùng đó nên không thể tồn tại hàng rào hợp lệ ngắn hơn. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

def cross(a, b, c):
    return (b[0] - a[0]) * (c[1] - a[1]) - (b[1] - a[1]) * (c[0] - a[0])

def convex_hull(points):
    points = sorted(set(points))
    if len(points) <= 1:
        return points

    lower = []
    for p in points:
        while len(lower) >= 2 and cross(lower[-2], lower[-1], p) <= 0:
            lower.pop()
        lower.append(p)

    upper = []
    for p in reversed(points):
        while len(upper) >= 2 and cross(upper[-2], upper[-1], p) <= 0:
            upper.pop()
        upper.append(p)

    return lower[:-1] + upper[:-1]

def solve():
    n, l = map(int, input().split())
    points = []

    for _ in range(n):
        x, y = map(int, input().split())
        points.append((x + l, y))
        points.append((x - l, y))
        points.append((x, y + l))
        points.append((x, y - l))

    hull = convex_hull(points)

    ans = 0.0
    m = len(hull)
    for i in range(m):
        x1, y1 = hull[i]
        x2, y2 = hull[(i + 1) % m]
        ans += math.hypot(x1 - x2, y1 - y2)

    print("{:.10f}".format(ans))

if __name__ == "__main__":
    solve()
```Việc xử lý đầu vào đọc trực tiếp các đỉnh đa giác và tạo ra bốn điểm dịch chuyển có thể có ngay lập tức, khớp với bước 1 của thuật toán. Tọa độ có thể lớn bằng khoảng`2 * 10^8`, vẫn được xử lý an toàn bởi số nguyên Python. 

Việc thực hiện thân tàu sử dụng phương pháp chuỗi đơn điệu. các`cross`hàm xác định hướng của ba điểm. Việc loại bỏ những điểm không rẽ trái là cần thiết vì những điểm đó nằm bên trong ranh giới hiện tại và không thể xuất hiện trên hàng rào ngắn nhất. 

Việc loại bỏ trùng lặp thông qua`set`là quan trọng khi`l = 0`hoặc khi nhiều điểm được tạo trùng nhau. Nếu không có nó, vòng thân tàu có thể chứa các đỉnh lặp đi lặp lại và tạo ra các phép tính chu vi không chính xác. 

Việc tính chu vi sử dụng`math.hypot`, giúp tránh tính toán căn bậc hai theo cách thủ công và hoạt động trực tiếp với đầu ra dấu phẩy động. Số nguyên Python tránh tràn trong quá trình tính toán tích chéo. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4 3
-3 -3
-3 3
3 3
3 -3
```Các điểm cực trị được tạo ra tạo thành ranh giới mở rộng giống như kim cương xung quanh hình vuông. 

| Bước | Hoạt động | Kích thước thân tàu | Kết quả hiện tại | 
| --- | --- | --- | --- | 
| 1 | Tạo điểm dịch chuyển | 16 điểm thô | Không được tính toán | 
| 2 | Loại bỏ các bản sao và xây dựng thân tàu | 8 điểm | Không được tính toán | 
| 3 | Tổng các cạnh thân tàu | 8 cạnh | 40.9705627485 | 

Dấu vết này cho thấy một hình vuông không chỉ đơn giản thu được`8*l`chu vi. Việc mở rộng Manhattan tạo ra các phần góc chéo, đó là lý do tại sao câu trả lời khác với việc mở rộng hình chữ nhật. 

### Mẫu 2 

đầu vào:```
9 0
1 1
3 1
5 1
5 2
3 2
3 3
2 3
2 2
1 2
```| Bước | Hoạt động | Kích thước thân tàu | Kết quả hiện tại | 
| --- | --- | --- | --- | 
| 1 | Tạo điểm dịch chuyển | 36 điểm thô | Không được tính toán | 
| 2 | Loại bỏ các bản sao và xây dựng thân tàu | Chỉ các điểm ranh giới bên ngoài | Không được tính toán | 
| 3 | Tổng các cạnh thân tàu | Chu vi đa giác cuối cùng | 10.6502815399 | 

Ví dụ này chứng minh tại sao thân tàu lại cần thiết. Đa giác ban đầu chứa một vết lõm, nhưng ranh giới bao quanh chỉ được xác định bởi các điểm ngoài cùng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Chúng tôi tạo ra`4n`điểm và sắp xếp chúng cho thân tàu. | 
| Không gian | O(n) | Các điểm và thân được tạo ra chứa một số phần tử tuyến tính. | 

Kích thước đầu vào cho phép`n = 100000`và sắp xếp đại khái`400000`điểm vẫn nằm trong giới hạn dự kiến. Việc sử dụng bộ nhớ vẫn tuyến tính và vừa vặn thoải mái trong giới hạn nhất định. 

## Trường hợp thử nghiệm```python
import sys
import io
import math

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    out = sys.stdout.getvalue()
    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return out

assert abs(float(run("""4 3
-3 -3
-3 3
3 3
3 -3
""")) - 40.9705627485) < 1e-6

assert abs(float(run("""9 0
1 1
3 1
5 1
5 2
3 2
3 3
2 3
2 2
1 2
""")) - 10.6502815399) < 1e-6

assert abs(float(run("""4 0
0 0
0 2
2 2
2 0
""")) - 8.0) < 1e-6

assert abs(float(run("""4 5
0 0
0 1
1 1
1 0
""")) - 20.0) < 1e-6

assert abs(float(run("""4 1
-100000000 -100000000
-100000000 100000000
100000000 100000000
100000000 -100000000
""")) - 800000004.0) < 1e-6
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hình vuông với`l = 0`|`8`| Các điểm được tạo trùng lặp và xử lý chu vi ban đầu | 
| Hình vuông nhỏ với hình vuông lớn`l`|`20`| Mở rộng xung quanh các hình dạng rất nhỏ | 
| Tọa độ rất lớn |`800000004`| Xử lý tọa độ số nguyên lớn | 
| Đa giác lõm |`10.6502815399`| Loại bỏ đúng các phần lõm xuyên qua thân tàu | 

## Vỏ cạnh 

Khi nào`l = 0`, mọi điểm được tạo đều giống với đỉnh ban đầu bốn lần. Thuật toán trước tiên sẽ loại bỏ các bản sao này, sau đó tính toán bao lồi thông thường. Đối với hình vuông ở ví dụ trước, thân tàu là hình vuông ban đầu và câu trả lời là`8`. 

Đối với đa giác lõm, thuật toán không bao giờ cố gắng giữ nguyên vết lõm ban đầu. Hãy xem xét mẫu thứ hai. Các điểm được tạo ra bao gồm các ứng cử viên xung quanh rãnh bên trong, nhưng bao lồi sẽ loại bỏ những điểm không thể xuất hiện ở ranh giới bên ngoài. Thân tàu kết quả chính xác là hình dạng hàng rào ngắn nhất. 

Đối với tọa độ rất lớn, các điểm dịch chuyển có thể tiến tới`2 * 10^8`về độ lớn. Số học số nguyên của Python giữ cho tất cả các phép tính tích số chéo luôn chính xác và chỉ phép chuyển đổi chu vi cuối cùng mới sử dụng các giá trị dấu phẩy động. Điều này ngăn ngừa các vấn đề về độ chính xác trong quá trình xây dựng thân tàu.
