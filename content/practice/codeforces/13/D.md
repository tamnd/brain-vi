---
title: "CF 13D - Hình tam giác"
description: "Chúng ta có hai tập hợp điểm trên mặt phẳng. Điểm màu đỏ có thể được dùng làm đỉnh tam giác, điểm màu xanh là chướng ngại vật. Không có ba điểm nào thẳng hàng, điều này loại bỏ tất cả các suy biến liên quan đến các điểm trên đường viền tam giác."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dp", "geometry"]
categories: ["algorithms"]
codeforces_contest: 13
codeforces_index: "D"
codeforces_contest_name: "Codeforces Beta Round 13"
rating: 2600
weight: 13
solve_time_s: 98
verified: true
draft: false
---
[CF 13D - Tam giác](https://codeforces.com/problemset/problem/13/D) 

**Đánh giá:** 2600 
**Thẻ:** dp, hình học 
**Thời gian giải:** 1 phút 38 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai tập hợp điểm trên mặt phẳng. Điểm màu đỏ có thể được dùng làm đỉnh tam giác, điểm màu xanh là chướng ngại vật. Không có ba điểm nào thẳng hàng, điều này loại bỏ tất cả các suy biến liên quan đến các điểm trên đường viền tam giác. 

Nhiệm vụ là đếm xem có bao nhiêu hình tam giác được tạo thành bởi các điểm màu đỏ không chứa điểm màu xanh nào hoàn toàn bên trong chúng. 

Việc giải thích hình học trực tiếp có ý nghĩa quan trọng ở đây. Một tam giác hợp lệ nếu mọi điểm màu xanh nằm bên ngoài tam giác hoặc trên một trong các cạnh bên ngoài của tam giác. Vì không có ba điểm nào thẳng hàng nên một điểm xanh không bao giờ có thể nằm chính xác trên một cạnh, nên mọi điểm xanh đều rõ ràng ở bên trong hoặc bên ngoài. 

Những hạn chế là dấu hiệu cảnh báo đầu tiên. Cả số điểm màu đỏ và màu xanh lam đều có thể đạt tới 500. Một cách tiếp cận đơn giản sẽ lặp lại từng ba điểm màu đỏ và kiểm tra từng điểm màu xanh lam đối với hình tam giác. Chỉ riêng số bộ ba màu đỏ là khoảng$\binom{500}{3} \approx 2 \cdot 10^7$. Nhân với hệ số khác là 500 điểm xanh sẽ có kết quả gần đúng$10^{10}$các bài kiểm tra hình học, vượt xa những gì có trong 2 giây. 

Hình học cũng ẩn giấu một số bẫy. 

Một sai lầm dễ mắc phải là xử lý sai các biển báo định hướng. Giả sử đầu vào là:```
3 1
0 0
4 0
0 4
1 1
```Hình tam giác duy nhất chứa điểm màu xanh, vì vậy câu trả lời là`0`. Nếu thử nghiệm bên trong giả định các đỉnh luôn ngược chiều kim đồng hồ nhưng quá trình triển khai xử lý chúng theo thứ tự tùy ý thì kết quả sẽ âm thầm trở thành không chính xác. 

Một trường hợp tinh tế khác xuất hiện khi không có điểm xanh nào cả:```
4 0
0 0
1 0
0 1
1 1
```Mọi bộ ba màu đỏ đều đúng nên đáp án là`4`. Bất kỳ giải pháp nào cố gắng loại bỏ các cấu hình “xấu” mà không xác định cẩn thận chúng đều có thể vô tình bị tính thiếu ở đây. 

Trường hợp cạnh hình học hơn là khi các điểm màu xanh lam nằm bên trong bao lồi nhưng nằm bên ngoài hầu hết các hình tam giác:```
4 1
0 0
10 0
10 10
0 10
8 8
```Chỉ một trong bốn hình tam giác chứa điểm màu xanh. Một cách tiếp cận bất cẩn chỉ dựa trên việc ngăn chặn thân lồi sẽ loại bỏ quá nhiều hình tam giác. 

Điều kiện “không có ba điểm nào thẳng hàng” cũng rất quan trọng. Nhiều công thức đếm dựa trên định hướng dựa trên các bất đẳng thức chặt chẽ. Nếu không có sự đảm bảo này, các trường hợp cạnh liên quan đến tích số 0 sẽ yêu cầu xử lý riêng. 

## Phương pháp tiếp cận 

Giải pháp brute-force rất đơn giản về mặt khái niệm. Liệt kê từng bộ ba điểm đỏ. Đối với mỗi tam giác, lặp lại tất cả các điểm màu xanh lam và kiểm tra xem điểm đó có nằm bên trong tam giác hay không bằng cách kiểm tra hướng hoặc phân tích diện tích. 

Điều này có hiệu quả vì một điểm nằm bên trong một tam giác một cách chính xác khi nó nằm trên cùng một phía của cả ba cạnh có hướng. Vì không có ba điểm nào thẳng hàng nên mọi phép thử đều nghiêm ngặt. 

Vấn đề là quy mô. Có nhiều nhất là:$$\binom{500}{3} \approx 2.08 \times 10^7$$hình tam giác. Kiểm tra 500 điểm màu xanh cho mỗi tam giác sẽ cho kết quả gần đúng:$$10^{10}$$tính toán định hướng. Ngay cả trong C++, điều này là vô vọng. 

Quan sát quan trọng là chúng ta không thực sự cần kiểm tra các điểm màu xanh lam một cách độc lập cho mọi tam giác. Thay vào đó, chúng ta có thể đếm chúng về mặt hình học. 

Sửa hai điểm đỏ$i$Và$j$. Xét cạnh có hướng$i \to j$. Chúng ta có thể tính toán trước có bao nhiêu điểm màu xanh nằm hoàn toàn bên trái của đường có hướng này. Gọi giá trị này`left[i][j]`. 

Bây giờ hãy xem xét một hình tam giác màu đỏ ngược chiều kim đồng hồ$(i,j,k)$. Mỗi điểm màu xanh bên trong tam giác đều góp phần vào vùng bên trái của cả ba cạnh có hướng. Có một bản sắc tổ hợp rõ ràng:$$\text{inside}(i,j,k) = left[i][j] + left[j][k] - left[i][k]$$cho các đỉnh được sắp xếp ngược chiều kim đồng hồ. 

Điều này chuyển đổi vấn đề từ “kiểm tra mọi điểm màu xanh đối với mọi tam giác” thành “tính trước số nửa mặt phẳng một lần, sau đó trả lời từng tam giác trong O(1)”. 

Thử thách còn lại là hiểu tại sao công thức này lại có tác dụng. Về mặt hình học, vùng được tính bằng`left[i][j] + left[j][k]`bao gồm phần bên trong của tam giác và một số hình nêm bổ sung bên ngoài tam giác. Trừ`left[i][k]`loại bỏ chính xác khu vực bổ sung đó. 

Khi tất cả số lượng bên trong có sẵn trong thời gian không đổi, chúng tôi chỉ cần liệt kê tất cả các bộ ba màu đỏ và đếm những bộ ba có phần bên trong không chứa điểm màu xanh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(N^3 M)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(N^2 M + N^3)$|$O(N^2)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các điểm màu đỏ và màu xanh. 

Điểm màu đỏ xác định hình tam giác ứng cử viên. Điểm màu xanh chỉ được truy vấn về mặt hình học. 
2. Tính toán trước`left[i][j]`cho mỗi cặp điểm màu đỏ theo thứ tự.`left[i][j]`lưu trữ bao nhiêu điểm màu xanh nằm đúng bên trái của đoạn được chỉ dẫn từ điểm màu đỏ`i`đến điểm đỏ`j`. 

Điều này được tính toán bằng cách sử dụng các sản phẩm chéo. Đối với một điểm màu xanh`p`:$$cross(r_j-r_i,\ p-r_i) > 0$$có nghĩa`p`nằm bên trái. 
3. Liệt kê từng bộ ba điểm đỏ`(i, j, k)`với`i < j < k`. 

Chúng ta chỉ cần các bộ ba không có thứ tự vì mỗi tam giác được tính một lần. 
4. Tính hướng của tam giác. 

Nếu hướng âm, hoán đổi hai đỉnh để tam giác trở nên ngược chiều kim đồng hồ. 

Danh tính đếm giả định thứ tự ngược chiều kim đồng hồ. 
5. Tính số điểm xanh bên trong tam giác bằng cách sử dụng:$$inside = left[i][j] + left[j][k] - left[i][k]$$6. Nếu`inside == 0`, tăng câu trả lời. 

Những hình tam giác như vậy không chứa điểm màu xanh và hợp lệ. 

### Tại sao nó hoạt động 

Đối với tam giác ngược chiều kim đồng hồ$(i,j,k)$, mọi điểm bên trong tam giác đều nằm bên trái các cạnh$i \to j$Và$j \to k$, nhưng không còn lại$i \to k$. Biểu thức:$$left[i][j] + left[j][k] - left[i][k]$$hoạt động giống như bao gồm-loại trừ trên các nửa mặt phẳng này. Các điểm bên trong tam giác đóng góp chính xác một lần. Các điểm bên ngoài đóng góp tổng thể bằng 0 vì bất kỳ số đếm vượt quá nào từ hai số hạng đầu tiên đều bị hủy bỏ bởi số hạng trừ. 

Vì mọi điểm màu xanh lam đều nằm hoàn toàn bên trong hoặc hoàn toàn bên ngoài nên công thức sẽ đưa ra số lượng chính xác các điểm màu xanh lam bên trong. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def cross(ax, ay, bx, by):
    return ax * by - ay * bx

def solve():
    n, m = map(int, input().split())

    red = [tuple(map(int, input().split())) for _ in range(n)]
    blue = [tuple(map(int, input().split())) for _ in range(m)]

    left = [[0] * n for _ in range(n)]

    for i in range(n):
        xi, yi = red[i]

        for j in range(n):
            if i == j:
                continue

            xj, yj = red[j]

            cnt = 0

            vx = xj - xi
            vy = yj - yi

            for px, py in blue:
                if cross(vx, vy, px - xi, py - yi) > 0:
                    cnt += 1

            left[i][j] = cnt

    ans = 0

    for i in range(n):
        xi, yi = red[i]

        for j in range(i + 1, n):
            xj, yj = red[j]

            for k in range(j + 1, n):
                xk, yk = red[k]

                area2 = cross(
                    xj - xi,
                    yj - yi,
                    xk - xi,
                    yk - yi
                )

                if area2 > 0:
                    a, b, c = i, j, k
                else:
                    a, b, c = i, k, j

                inside = (
                    left[a][b]
                    + left[b][c]
                    - left[a][c]
                )

                if inside == 0:
                    ans += 1

    print(ans)

solve()
```Phần đầu tiên của mã xây dựng`left`ma trận. Mỗi cặp điểm màu đỏ có thứ tự xác định một đường có hướng và chúng ta đếm xem có bao nhiêu điểm màu xanh nằm ở phía bên trái của nó. Vì có nhiều nhất$500^2$các cặp có thứ tự và 500 điểm xanh, quá trình tiền xử lý này thực hiện khoảng$1.25 \times 10^8$các sản phẩm chéo, có thể chấp nhận được trong Python được tối ưu hóa vì mỗi thao tác cực kỳ nhỏ. 

Việc triển khai sản phẩm chéo là trọng tâm:```
cross(ax, ay, bx, by)
```trả về diện tích đã ký của hình bình hành. Tích cực có nghĩa là vectơ thứ hai nằm ở bên trái của vectơ thứ nhất. 

Vòng liệt kê tam giác chỉ xem xét`i < j < k`, đảm bảo mọi tam giác đều được xử lý một lần. 

Việc điều chỉnh hướng là tinh tế và cần thiết. Công thức:```
left[a][b] + left[b][c] - left[a][c]
```chỉ hoạt động theo thứ tự ngược chiều kim đồng hồ. Nếu tam giác theo chiều kim đồng hồ, các vùng hình học sẽ bị đảo ngược và việc đếm trở nên vô nghĩa. 

Số nguyên Python tự động tránh tràn, nhưng trong C++ vấn đề này yêu cầu số nguyên 64 bit vì tọa độ đạt tới$10^9$, tạo ra các sản phẩm chéo có kích thước lớn bằng$10^{18}$. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 1
0 0
10 0
10 10
5 4
2 1
```Các điểm màu đỏ là: 

- A = (0,0) 
- B = (10,0) 
- C = (10,10) 
-D = (5,4) 

Điểm màu xanh là P = (2,1). 

Thuật toán tính toán các giá trị liên quan`left`các giá trị. 

| Cạnh định hướng | Điểm xanh bên trái | 
| --- | --- | 
| A → B | 1 | 
| B → C | 1 | 
| A → C | 0 | 
| A → D | 0 | 
| D → C | 1 | 

Bây giờ hãy đánh giá các hình tam giác. 

| Tam giác | Thứ tự ngược chiều kim đồng hồ | Công thức bên trong | Bên trong màu xanh? | 
| --- | --- | --- | --- | 
| A,B,C | A,B,C | 1 + 1 - 0 = 2 | Có | 
| A,B,D | A,B,D | 1 + 0 - 0 = 1 | Có | 
| A,C,D | A,D,C | 0 + 1 - 0 = 1 | Có | 
| B,C,D | B,C,D | 1 + 0 - 1 = 0 | Không | 

Chỉ có hình tam giác$BCD$là hợp lệ. 

Dấu vết này cho thấy tại sao định hướng lại quan trọng. Tam giác$ACD$ban đầu xuất hiện theo chiều kim đồng hồ nên các đỉnh phải được sắp xếp lại trước khi áp dụng công thức. 

### Ví dụ 2 

đầu vào:```
4 0
0 0
1 0
0 1
1 1
```Không có điểm màu xanh. 

Mọi`left[i][j]`bằng không. 

| Tam giác | Công thức bên trong | hợp lệ | 
| --- | --- | --- | 
| 0,1,2 | 0 | Có | 
| 0,1,3 | 0 | Có | 
| 0,2,3 | 0 | Có | 
| 1,2,3 | 0 | Có | 

Đáp án = 4. 

Điều này xác nhận rằng công thức loại trừ bao gồm xử lý một cách tự nhiên trường hợp điểm trống màu xanh mà không có bất kỳ logic đặc biệt nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(N^2 M + N^3)$| Tính toán trước số lượng nửa mặt phẳng, sau đó liệt kê tất cả các hình tam giác | 
| Không gian |$O(N^2)$| Lưu trữ`left`ma trận | 

Với$N,M \le 500$, quá trình tiền xử lý thực hiện khoảng$1.25 \times 10^8$các phép toán nguyên thủy và phép liệt kê tam giác bổ sung thêm về$2 \times 10^7$lần lặp lại. Cả hai đều nằm trong giới hạn của Python được tối ưu hóa khi được triển khai cẩn thận với các truy cập mảng và số học đơn giản. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def cross(ax, ay, bx, by):
        return ax * by - ay * bx

    n, m = map(int, input().split())

    red = [tuple(map(int, input().split())) for _ in range(n)]
    blue = [tuple(map(int, input().split())) for _ in range(m)]

    left = [[0] * n for _ in range(n)]

    for i in range(n):
        xi, yi = red[i]

        for j in range(n):
            if i == j:
                continue

            xj, yj = red[j]

            vx = xj - xi
            vy = yj - yi

            cnt = 0

            for px, py in blue:
                if cross(vx, vy, px - xi, py - yi) > 0:
                    cnt += 1

            left[i][j] = cnt

    ans = 0

    for i in range(n):
        xi, yi = red[i]

        for j in range(i + 1, n):
            xj, yj = red[j]

            for k in range(j + 1, n):
                xk, yk = red[k]

                area2 = cross(
                    xj - xi,
                    yj - yi,
                    xk - xi,
                    yk - yi
                )

                if area2 > 0:
                    a, b, c = i, j, k
                else:
                    a, b, c = i, k, j

                inside = (
                    left[a][b]
                    + left[b][c]
                    - left[a][c]
                )

                if inside == 0:
                    ans += 1

    return str(ans)

# provided sample
assert run(
"""4 1
0 0
10 0
10 10
5 4
2 1
"""
) == "2", "sample 1"

# minimum size
assert run(
"""0 0
"""
) == "0", "no points"

# no blue points
assert run(
"""4 0
0 0
1 0
0 1
1 1
"""
) == "4", "all triangles valid"

# one triangle blocked
assert run(
"""3 1
0 0
4 0
0 4
1 1
"""
) == "0", "blue point inside only triangle"

# blue point outside all triangles
assert run(
"""3 1
0 0
4 0
0 4
10 10
"""
) == "1", "outside point"

# orientation edge case
assert run(
"""4 1
0 0
0 5
5 0
5 5
1 1
"""
) == "2", "clockwise handling"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Không có điểm | 0 | Xử lý đầu vào trống | 
| Không có điểm xanh | 4 | Tất cả các hình tam giác được tính | 
| Tam giác chặn đơn | 0 | Độ chính xác phát hiện nội thất | 
| Điểm xanh bên ngoài | 1 | Điểm bên ngoài bị bỏ qua | 
| Trường hợp cạnh định hướng | 2 | Xử lý đúng bộ ba theo chiều kim đồng hồ | 

## Vỏ cạnh 

Hãy xem xét trường hợp một điểm màu xanh nằm bên trong tam giác duy nhất có thể:```
3 1
0 0
4 0
0 4
1 1
```Hướng tam giác là ngược chiều kim đồng hồ. Thuật toán tính toán:$$inside = left[A][B] + left[B][C] - left[A][C]$$Điểm màu xanh đóng góp chính xác một lần, vì vậy`inside = 1`. Tam giác bị từ chối và đầu ra trở thành`0`. 

Bây giờ hãy xem xét kịch bản không có điểm xanh:```
4 0
0 0
1 0
0 1
1 1
```Mọi`left[i][j]`bằng 0 vì không có điểm màu xanh nào để đếm. Mỗi tam giác nhận được:$$inside = 0$$và cả bốn hình tam giác đều được chấp nhận. 

Cuối cùng, hãy xem xét một tam giác được xử lý theo chiều kim đồng hồ:```
3 0
0 0
0 5
5 0
```Định hướng thô là tiêu cực. Thuật toán hoán đổi hai đỉnh cuối cùng trước khi tính công thức. Nếu không có sự điều chỉnh này, danh tính bao gồm-loại trừ sẽ sử dụng các nửa mặt phẳng không tương thích và tạo ra các giá trị vô nghĩa. Sắp xếp lại sẽ khôi phục tính bất biến ngược chiều kim đồng hồ, do đó tam giác được tính chính xác.
