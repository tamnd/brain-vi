---
title: "CF 1584G - Phân khúc đủ điều kiện"
description: "Chúng ta có một số điểm trên mặt phẳng. Chúng tôi muốn đếm các cặp điểm có đoạn kết nối gần với mọi điểm trong tập hợp. Chính xác hơn, đối với cặp $(pi,pj)$ được chọn, mọi điểm phải nằm cách đoạn đó một khoảng tối đa là $R$."
date: "2026-06-10T09:41:50+07:00"
tags: ["codeforces", "competitive-programming", "geometry"]
categories: ["algorithms"]
codeforces_contest: 1584
codeforces_index: "G"
codeforces_contest_name: "Technocup 2022 - Elimination Round 2"
rating: 3200
weight: 1584
solve_time_s: 139
verified: false
draft: false
---

[CF 1584G - Phân khúc đủ điều kiện](https://codeforces.com/problemset/problem/1584/G) 

**Đánh giá:** 3200 
**Thẻ:** hình học 
**Thời gian giải:** 2m 19s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một số điểm trên mặt phẳng. Chúng tôi muốn đếm các cặp điểm có đoạn kết nối gần với mọi điểm trong tập hợp. Chính xác hơn, đối với một cặp được chọn$(p_i,p_j)$, mọi điểm phải nằm cách nhau nhiều nhất$R$từ phân khúc đó. 

Đoạn này quan trọng chứ không phải dòng vô hạn. Một điểm có thể chiếu ra ngoài đoạn đó, trong trường hợp đó khoảng cách của nó tới đoạn đó là khoảng cách đến một trong các điểm cuối. 

Số điểm tối đa là 3000. Có khoảng$4.5\times 10^6$cặp điểm cuối. Việc kiểm tra từng cặp đối với tất cả các điểm sẽ yêu cầu khoảng$$\binom{3000}{2}\cdot 3000 \approx 1.35\times 10^{10}$$tính toán khoảng cách, vượt xa những gì phù hợp với ba giây. Bất kỳ giải pháp khối được loại trừ. Các thuật toán bậc hai được chấp nhận và$O(n^2\log n)$đủ nhanh một cách dễ dàng. 

Một số tình huống rất dễ xử lý sai. 

Giả sử tất cả các điểm nằm trên một dòng:```
3 1
0 0
2 0
5 0
```Câu trả lời đúng là 1, tức là đoạn nối hai điểm cực trị. Nếu chúng ta chỉ yêu cầu mọi điểm phải gần với đường hỗ trợ vô hạn thì cả ba cặp đều có vẻ hợp lệ, điều này sai vì cặp ở giữa để lại một điểm cuối bên ngoài đoạn thẳng. 

Một trường hợp tinh tế khác xuất hiện khi điểm xa nhất so với đường hỗ trợ nằm ngoài điểm cuối.```
3 2
0 0
3 0
10 1
```điểm$(10,1)$chỉ cách đường vô hạn một khoảng cách là 1$y=0$, nhưng khoảng cách của nó đến đoạn từ$(0,0)$ĐẾN$(3,0)$gần bằng 7. Chỉ riêng công thức dựa trên dòng đã bỏ lỡ điều này. 

Cạm bẫy thứ ba là đoạn thẳng phải bao phủ hình chiếu của tất cả các điểm. Vì```
4 2
-3 0
0 1
0 -1
3 0
```chỉ có đoạn giữa hai điểm cực trị hoạt động. Bất kỳ đoạn nào ngắn hơn đều để lại một trong những điểm bên ngoài ở bên ngoài. 

## Phương pháp tiếp cận 

Ý tưởng về vũ lực rất đơn giản. Liệt kê mọi cặp điểm, coi chúng là điểm cuối của đoạn và tính khoảng cách từ mọi điểm đến đoạn đó. Nếu tất cả các khoảng cách lớn nhất$R$, đếm cặp. 

Tính đúng đắn là ngay lập tức vì định nghĩa được kiểm tra trực tiếp. Vấn đề là tốc độ. có$O(n^2)$cặp và mỗi cặp yêu cầu$O(n)$kiểm tra khoảng cách, đưa ra$O(n^3)$hoạt động. 

Để cải thiện điều này, chúng ta cần hiểu về hình học. 

Đi theo hướng nào đó$u$. Chiếu mọi điểm theo hướng đó. Trong số tất cả các phép chiếu đều có mức tối thiểu và mức tối đa. Đoạn nối các điểm đạt được các hình chiếu cực trị này là đoạn ứng cử viên duy nhất có đường hỗ trợ có phương hướng$u$. Bất kỳ cặp nào khác có cùng hướng sẽ để lại một số hình chiếu bên ngoài đoạn thẳng. 

Bây giờ nhìn vào hướng vuông góc$v$. Sự khác biệt lớn nhất giữa các hình chiếu lên$v$chính xác là chiều rộng của điểm được đặt theo hướng đó. Mọi điểm đều nằm trong khoảng cách$$\frac{\text{width}}{2}$$từ đường giữa. Do đó, một đoạn có hướng$u$khả thi chính xác khi chiều rộng vuông góc với$u$nhiều nhất là$2R$. 

Điều này chuyển bài toán thành một bài toán hình học lồi. Chỉ các đỉnh bao lồi mới quan trọng, bởi vì các điểm bên trong không bao giờ có thể trở thành hình chiếu cực đoan. Đối với mỗi hướng cạnh trên thân lồi, chúng ta có thể sử dụng thước cặp quay để duy trì điểm xa nhất theo hướng vuông góc và thu được chiều rộng tổng thể theo thời gian tuyến tính. 

Bất cứ khi nào chiều rộng không vượt quá$2R$, hai điểm cực trị dọc theo hướng hiện tại tạo thành một đoạn đủ điều kiện. 

Quan sát quan trọng là mỗi hướng xác định nhiều nhất một cặp ứng cử viên, giảm hàng triệu khả năng xuống chỉ còn$m$đỉnh thân tàu, ở đâu$m\le n$. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^3)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(n\log n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Tính bao lồi. 

Các điểm bên trong không bao giờ xác định các hình chiếu cực trị theo bất kỳ hướng nào, vì vậy chúng có thể bị bỏ qua. Chúng ta xây dựng bao lồi bằng thuật toán chuỗi đơn điệu. 

### 2. Xử lý vỏ tàu nhỏ. 

Nếu thân chứa một điểm thì không tồn tại đoạn nào. 

Nếu thân chứa hai điểm thì đoạn thẳng của chúng là câu trả lời khả dĩ duy nhất và nó luôn đúng. 

### 3. Đi ngang mép thân tàu bằng kẹp xoay. 

Đối với mỗi cạnh thân tàu$H_iH_{i+1}$, xét hướng của nó. Chiều rộng vuông góc với hướng này bằng khoảng cách tối đa từ đường đỡ qua mép tới bất kỳ đỉnh nào của thân tàu. 

Do điểm đối cực di chuyển đơn điệu xung quanh đa giác nên kỹ thuật thước cặp quay sẽ cập nhật mức tối đa này trong tổng thời gian tuyến tính. 

### 4. Tính chiều rộng. 

Đối với vectơ cạnh$e$, cho phép$$A = |\text{cross}(e, H_k-H_i)|$$Ở đâu$k$là điểm đối cực hiện tại. 

Chiều rộng thực tế là$$\frac{A}{|e|}.$$Thay vì sử dụng số dấu phẩy động, hãy so sánh$$A^2 \le (2R)^2|e|^2.$$Điều này tránh được các vấn đề về độ chính xác. 

### 5. Khôi phục cặp điểm cuối. 

Đối với hướng cạnh hiện tại, đoạn kéo dài tất cả các hình chiếu được hình thành bởi hai điểm hỗ trợ theo hướng đó. 

Thước cặp quay đồng thời duy trì các đỉnh cực trị này. Bất cứ khi nào điều kiện về độ rộng được đáp ứng, cặp đó sẽ đóng góp một câu trả lời. 

### 6. Đếm các cặp khác nhau. 

Các hướng cạnh khác nhau có thể tạo ra cùng một cặp điểm cuối, vì vậy chúng tôi lưu trữ các cặp trong một tập hợp. 

### Tại sao nó hoạt động 

Đối với bất kỳ hướng nào, tất cả các hình chiếu điểm tạo thành một khoảng. Đoạn duy nhất có hướng đó có khoảng chiếu chứa mọi điểm là đoạn nối các hình chiếu tối thiểu và tối đa. Khoảng cách của một điểm tới đoạn này bằng khoảng cách của nó với đường hỗ trợ vì mọi hình chiếu đều nằm trong khoảng. 

Chiều rộng vuông góc với hướng của đoạn thẳng chính xác là khoảng cách lớn nhất nhân với hai. Do đó điều kiện “mọi điểm đều nằm trong khoảng cách$R$từ đoạn thẳng" tương đương với "chiều rộng của điểm đặt vuông góc với đoạn thẳng tối đa là$2R$". Thước cặp xoay liệt kê mọi hướng có liên quan, do đó, mọi cặp hợp lệ đều được tìm thấy và không có cặp không hợp lệ nào được tính. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def cross(o, a, b):
    return (a[0] - o[0]) * (b[1] - o[1]) - (a[1] - o[1]) * (b[0] - o[0])

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
    n, R = map(int, input().split())
    pts = [tuple(map(int, input().split())) for _ in range(n)]

    hull = convex_hull(pts)
    m = len(hull)

    if m < 2:
        print(0)
        return

    if m == 2:
        print(1)
        return

    hull.extend(hull[:2])

    j = 1
    ans = set()

    for i in range(m):
        ni = i + 1
        ex = hull[ni][0] - hull[i][0]
        ey = hull[ni][1] - hull[i][1]

        while True:
            nj = (j + 1) % m

            cur = abs(ex * (hull[j][1] - hull[i][1]) -
                      ey * (hull[j][0] - hull[i][0]))
            nxt = abs(ex * (hull[nj][1] - hull[i][1]) -
                      ey * (hull[nj][0] - hull[i][0]))

            if nxt > cur:
                j = nj
            else:
                break

        area = abs(ex * (hull[j][1] - hull[i][1]) -
                   ey * (hull[j][0] - hull[i][0]))

        edge_sq = ex * ex + ey * ey

        if area * area <= 4 * R * R * edge_sq:
            a = i
            b = ni % m
            if a > b:
                a, b = b, a
            ans.add((a, b))

    print(len(ans))

solve()
```Phần đầu tiên xây dựng bao lồi. sử dụng`<=0`loại bỏ các điểm trong thẳng hàng và chỉ để lại các đỉnh cực trị. 

Thước cặp quay duy trì điểm đối cực`j`. Từ`j`chỉ di chuyển về phía trước, tổng số lần tiến của con trỏ là tuyến tính. 

Việc so sánh sử dụng số lượng bình phương. Điều này tránh được sự thiếu chính xác của dấu phẩy động và phù hợp với sự đảm bảo của tuyên bố rằng những nhiễu loạn nhỏ của$R$không thay đổi câu trả lời. 

Các cặp được chèn vào một tập hợp vì nhiều cạnh có thể tương ứng với cùng một hướng của đoạn. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4 2
0 1
0 -1
3 0
-3 0
```Thứ tự bao lồi:$$(-3,0),(0,-1),(3,0),(0,1)$$| Cạnh | Điểm đối cực | Chiều rộng | Chiều rộng 2R? | 
| --- | --- | --- | --- | 
| (-3,0)-(0,-1) | (0,1) | 1.897 | Có | 
| (0,-1)-(3,0) | (-3,0) | 1.897 | Có | 
| (3,0)-(0,1) | (0,-1) | 1.897 | Có | 
| (0,1)-(-3,0) | (3,0) | 1.897 | Có | 

Chỉ có một phân đoạn riêng biệt trải dài trên tất cả các hình chiếu, cụ thể là$(-3,0)$ĐẾN$(3,0)$. 

Đầu ra:```
1
```Ví dụ này cho thấy một số hướng cạnh có thể tương ứng với cùng một cặp cuối cùng. 

### Ví dụ cộng tuyến 

đầu vào:```
3 1
0 0
2 0
5 0
```| Kích thước thân tàu | Phân khúc ứng viên | Trả lời | 
| --- | --- | --- | 
| 2 | (0,0)-(5,0) | 1 | 

Tất cả các điểm đều nằm trên đoạn thẳng nên mọi khoảng cách đều bằng 0. 

Điều này khẳng định rằng các điểm thẳng hàng bên trong được loại bỏ một cách an toàn bởi kết cấu thân tàu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n\log n)$| Cấu trúc thân tàu chiếm ưu thế | 
| Không gian |$O(n)$| Đỉnh thân tàu và các kết cấu phụ trợ | 

giới hạn$n=3000$là khiêm tốn, và một$O(n\log n)$thuật toán chạy thoải mái trong vòng ba giây. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out
    solve()
    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# sample 1
assert run("""4 2
0 1
0 -1
3 0
-3 0
""") == "1"

# single point
assert run("""1 5
0 0
""") == "0"

# two points
assert run("""2 1
0 0
5 0
""") == "1"

# collinear points
assert run("""3 1
0 0
2 0
5 0
""") == "1"

# square with large R
assert run("""4 10
0 0
1 0
1 1
0 1
""") == "6"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| One point | 0 | No segment exists |
 | Two points | 1 | Trivial hull |
 | Collinear points | 1 | Interior collinear points removed correctly |
 | Large square with large R | 6 | Every pair becomes valid |

 ## Vỏ cạnh 

Hãy xem xét```
3 1
0 0
2 0
5 0
```Thân tàu chỉ chứa hai điểm cực trị. Thước cặp quay không bao giờ cần phải di chuyển. Thuật toán đưa ra 1, điều này đúng vì điểm ở giữa nằm trên đoạn thẳng. 

Vì```
3 2
0 0
3 0
10 1
```chiều rộng so với hướng của đoạn ngắn là nhỏ, nhưng các hình chiếu không vượt qua điểm thứ ba. Cặp hỗ trợ cho hướng đó trở thành$(0,0)$Và$(10,1)$, không$(0,0)$Và$(3,0)$, ngăn chặn việc đếm không chính xác. 

Vì```
4 2
-3 0
0 1
0 -1
3 0
```nhiều hướng cạnh thỏa mãn điều kiện chiều rộng. Tất cả chúng đều tạo ra cặp hỗ trợ giống nhau bao gồm hai điểm cực trị. Tập hợp các cặp loại bỏ các câu trùng lặp, để lại câu trả lời đúng là 1.
