---
title: "CF 32E - Trốn Tìm"
description: "Chúng ta có hai người được biểu diễn dưới dạng các điểm trên mặt phẳng, Victor và Peter. Căn phòng có đúng hai đồ vật, một đoạn tường kiên cố và một đoạn gương hai mặt. Victor muốn biết liệu anh ấy có thể nhìn thấy Peter trực tiếp hay qua một hình ảnh phản chiếu duy nhất trong gương."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "geometry", "implementation"]
categories: ["algorithms"]
codeforces_contest: 32
codeforces_index: "E"
codeforces_contest_name: "Codeforces Beta Round 32 (Div. 2, Codeforces format)"
rating: 2400
weight: 32
solve_time_s: 145
verified: true
draft: false
---
[CF 32E - Trốn tìm](https://codeforces.com/problemset/problem/32/E) 

**Đánh giá:** 2400 
**Tags:** hình học, thực hiện 
**Thời gian giải:** 2m 25s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai người được biểu diễn dưới dạng các điểm trên mặt phẳng, Victor và Peter. Căn phòng có đúng hai đồ vật, một đoạn tường kiên cố và một đoạn gương hai mặt. Victor muốn biết liệu anh ấy có thể nhìn thấy Peter trực tiếp hay qua một hình ảnh phản chiếu duy nhất trong gương. 

Đường ngắm trực tiếp sẽ bị chặn nếu đoạn nối Victor và Peter chạm vào tường dù chỉ một điểm. Chiếc gương hoạt động khác hẳn. Nếu đường ngắm chạm vào gương thì sự phản xạ có thể xảy ra theo quang học hình học. Sự phản xạ chỉ có hiệu lực khi cả hai người ở cùng một phía của đường vô hạn chứa gương và tia nhìn không song song với gương. 

Đầu vào đưa ra tọa độ của hai người, sau đó là điểm cuối của đoạn tường, sau đó là điểm cuối của đoạn gương. Đầu ra chỉ đơn giản là CÓ hoặc KHÔNG. 

Các hạn chế là rất nhỏ từ góc độ phức tạp. Tọa độ được giới hạn bởi$10^4$, và chỉ có một bức tường và một tấm gương. Điều này có nghĩa là thách thức hoàn toàn nằm ở tính chính xác về hình học chứ không phải hiệu suất. Bất kỳ thuật toán nào thực hiện một số phép toán hình học không đổi đều dễ dàng nằm trong giới hạn. Khó khăn thực sự là xử lý chính xác tất cả các tình huống suy biến: chạm vào điểm cuối, sự thẳng hàng, các tia song song và các quy tắc hiển thị gương. 

Một số trường hợp cạnh rất dễ xử lý sai. 

Giả sử đoạn trực tiếp chỉ chạm vào điểm cuối của bức tường.```
0 0
4 0
2 -1 2 1
10 0 11 0
```Câu trả lời đúng là`NO`. Tuyên bố nói rằng ngay cả một điểm chung cũng chặn khả năng hiển thị. Việc triển khai bất cẩn bằng cách sử dụng các bài kiểm tra giao lộ nghiêm ngặt sẽ cho phép hiển thị không chính xác. 

Một trường hợp nguy hiểm khác là khi đoạn nhìn nằm dọc theo đường gương.```
0 0
4 0
10 0 11 0
2 0 3 0
```Câu trả lời đúng là`YES`chỉ khi bức tường không chặn đường đi trực tiếp. Sự phản xạ không xảy ra vì đường ngắm song song với gương. Gương cũng không được coi là chướng ngại vật trong tình huống này. Việc triển khai ngây thơ “bất kỳ thao tác chạm nào với gương đều có nghĩa là phản chiếu” sẽ cho kết quả sai. 

Một trường hợp tinh vi hơn xuất phát từ điều kiện cùng phía.```
-1 1
1 -1
10 0 11 0
0 -2 0 2
```Câu trả lời đúng là`NO`. Mặc dù cấu trúc phản xạ về mặt hình học tạo ra một đường đi qua gương, nhưng hai người nằm ở hai phía đối diện của đường gương, do đó việc phát biểu bài toán cấm sự phản chiếu. 

Phản ánh điểm cuối cũng hợp lệ. Nếu tia phản xạ chạm gương chính xác tại một điểm cuối thì khả năng nhìn vẫn có tác dụng vì gương có một điểm chung với đường nhìn. 

## Phương pháp tiếp cận 

Một cách tiếp cận hình học mạnh mẽ sẽ cố gắng mô phỏng tất cả các tia nhìn thấy được. Để nhìn trực tiếp, chúng ta chỉ cần kiểm tra xem đoạn thẳng giữa Victor và Peter có giao nhau với bức tường hay không. Sự phản ánh khó khăn hơn. Một ý tưởng ngây thơ là tham số hóa các điểm trên gương, coi mỗi điểm là một điểm phản xạ ứng cử viên và xác minh điều kiện góc bằng bằng số. Ngay cả khi được rời rạc hóa một cách tinh vi, điều này vẫn trở nên không đáng tin cậy vì độ chính xác của dấu phẩy động xác định tính đúng đắn. Các bài toán hình học chính xác hiếm khi chấp nhận được việc lấy mẫu gần đúng. 

Quan sát quan trọng là sự phản chiếu qua một đường sẽ chuyển đổi điều kiện góc bằng thành khả năng hiển thị đường thẳng. Đây là thủ thuật gương cổ điển từ quang học. 

Phản chiếu Peter qua dòng vô hạn chứa gương, nhận được một điểm mới$P'$. Bất kỳ đường phản xạ hợp lệ nào từ Victor đến Peter qua gương đều tương ứng chính xác với đoạn thẳng từ Victor đến$P'$. Điểm giao nhau giữa đoạn này và đường gương là điểm phản chiếu. 

Điều này biến vấn đề thành một tập hợp nhỏ các kiểm tra hình học xác định: 

Đầu tiên, hãy kiểm tra khả năng hiển thị trực tiếp. Nếu đoạn VP không cắt tường thì câu trả lời ngay là CÓ. 

Nếu không, hãy thử khả năng hiển thị gương. Phản chiếu Peter qua đường gương. Vẽ đoạn thẳng từ Victor đến điểm phản ánh. Nếu đoạn này cắt đoạn gương tại đúng một điểm thì hình học phản chiếu là hợp lệ. Sau đó kiểm tra xem hai người có ở cùng một phía của đường gương không và đảm bảo rằng không có nửa đường phản chiếu nào bị tường chặn lại. 

Toàn bộ giải pháp trở thành một số lượng không đổi các hoạt động giao cắt và định hướng phân đoạn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Không thể ràng buộc một cách đáng tin cậy | O(1) | Không chính xác / không thực tế | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tọa độ của Victor, Peter, đoạn tường và đoạn gương. 
2. Kiểm tra tầm nhìn trực tiếp bằng cách kiểm tra xem đoạn VP có giao nhau với đoạn tường hay không. 

Nếu chúng không giao nhau, Victor đã có thể nhìn thấy trực tiếp Peter, vì vậy in YES. 
3. Tính đường thẳng vô hạn chứa gương. 
4. Xác minh rằng Victor và Peter nằm trên cùng một phía của đường gương. 

Sự phản chiếu chỉ được phép trong điều kiện này. Nếu chúng ở phía đối diện thì không thể phản xạ được. 
5. Phản chiếu Peter qua đường gương. 

Điều này chuyển bài toán phản xạ thành bài toán đường thẳng. Đoạn từ Victor đến điểm phản xạ biểu thị tia phản xạ chưa được trải ra. 
6. Tính giao điểm giữa đoạn thẳng$V \to P'$và đường gương. 

Điểm này là điểm phản ánh duy nhất có thể. 
7. Kiểm tra xem giao điểm này có nằm trên đoạn gương hay không. 

Sự phản xạ không thể xảy ra bên ngoài gương hữu hạn. 
8. Đảm bảo tia nhìn không song song với đường gương. 

Nếu đoạn$V \to P'$song song với gương, tuyên bố rõ ràng cấm sự phản chiếu. 
9. Chia đường đi phản chiếu thành hai đoạn: Victor đến điểm phản chiếu và điểm phản chiếu tới Peter. 
10. Kiểm tra xem một trong hai đoạn có giao nhau với tường không. 

Nếu bức tường chạm vào một nửa đường phản xạ, tầm nhìn sẽ bị chặn. 
11. Nếu tất cả các bước kiểm tra đều đạt, hãy in CÓ. Ngược lại in ra KHÔNG. 

### Tại sao nó hoạt động 

Sự phản chiếu qua một đường thẳng bảo toàn các góc. Bằng cách phản chiếu Peter qua đường gương, điều kiện phản xạ góc bằng nhau trở thành đoạn thẳng từ Victor đến ảnh phản chiếu. Bất kỳ tia phản xạ hợp lệ nào cũng phải tương ứng với đoạn đó và mọi đoạn như vậy đi qua đoạn gương đều mang lại một điểm phản xạ hợp lệ về mặt vật lý. 

Thuật toán kiểm tra chính xác các ràng buộc hình học do câu lệnh áp đặt: 

Tầm nhìn trực tiếp không yêu cầu giao nhau với tường. 

Khả năng hiển thị của gương yêu cầu: 

- định vị cùng phía, 
- tia không song song, 
- giao điểm với đoạn gương hữu hạn, 
- và không có bức tường nào chặn cả nửa con đường. 

Vì mọi cách hợp lệ để xem Peter phải thỏa mãn một trong hai cấu hình này nên thuật toán hoàn chỉnh và chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

EPS = 1e-9

class Point:
    def __init__(self, x, y):
        self.x = float(x)
        self.y = float(y)

    def __add__(self, other):
        return Point(self.x + other.x, self.y + other.y)

    def __sub__(self, other):
        return Point(self.x - other.x, self.y - other.y)

    def __mul__(self, k):
        return Point(self.x * k, self.y * k)

def dot(a, b):
    return a.x * b.x + a.y * b.y

def cross(a, b):
    return a.x * b.y - a.y * b.x

def orient(a, b, c):
    return cross(b - a, c - a)

def on_segment(a, b, p):
    if abs(orient(a, b, p)) > EPS:
        return False

    return (
        min(a.x, b.x) - EPS <= p.x <= max(a.x, b.x) + EPS
        and min(a.y, b.y) - EPS <= p.y <= max(a.y, b.y) + EPS
    )

def segments_intersect(a, b, c, d):
    o1 = orient(a, b, c)
    o2 = orient(a, b, d)
    o3 = orient(c, d, a)
    o4 = orient(c, d, b)

    if o1 * o2 < -EPS and o3 * o4 < -EPS:
        return True

    if abs(o1) <= EPS and on_segment(a, b, c):
        return True
    if abs(o2) <= EPS and on_segment(a, b, d):
        return True
    if abs(o3) <= EPS and on_segment(c, d, a):
        return True
    if abs(o4) <= EPS and on_segment(c, d, b):
        return True

    return False

def line_intersection(a, b, c, d):
    ab = b - a
    cd = d - c
    denom = cross(ab, cd)

    if abs(denom) <= EPS:
        return None

    t = cross(c - a, cd) / denom
    return a + ab * t

def reflect_point(p, a, b):
    ab = b - a
    ap = p - a

    t = dot(ap, ab) / dot(ab, ab)
    proj = a + ab * t

    return proj * 2 - p

def same_side(p1, p2, a, b):
    o1 = orient(a, b, p1)
    o2 = orient(a, b, p2)

    return o1 * o2 > EPS

def solve():
    xv, yv = map(int, input().split())
    xp, yp = map(int, input().split())

    xw1, yw1, xw2, yw2 = map(int, input().split())
    xm1, ym1, xm2, ym2 = map(int, input().split())

    V = Point(xv, yv)
    P = Point(xp, yp)

    W1 = Point(xw1, yw1)
    W2 = Point(xw2, yw2)

    M1 = Point(xm1, ym1)
    M2 = Point(xm2, ym2)

    # Direct visibility
    if not segments_intersect(V, P, W1, W2):
        print("YES")
        return

    # Reflection impossible if on opposite sides
    if not same_side(V, P, M1, M2):
        print("NO")
        return

    # Reflect Peter
    PR = reflect_point(P, M1, M2)

    # Parallel to mirror
    if abs(cross(PR - V, M2 - M1)) <= EPS:
        print("NO")
        return

    # Reflection point
    R = line_intersection(V, PR, M1, M2)

    if R is None or not on_segment(M1, M2, R):
        print("NO")
        return

    # Wall blocks either half
    if segments_intersect(V, R, W1, W2):
        print("NO")
        return

    if segments_intersect(R, P, W1, W2):
        print("NO")
        return

    print("YES")

solve()
```Việc triển khai được xây dựng xung quanh các nguyên tắc hình học tính toán tiêu chuẩn. 

các`orient`hàm tính toán diện tích đã ký, xác định hướng tương đối. Mọi bài kiểm tra giao lộ đều phụ thuộc vào nó. Mã này sử dụng một epsilon nhỏ vì các phép tính phản xạ liên quan đến phép chia và phép chiếu dấu phẩy động. 

Thử nghiệm khả năng hiển thị trực tiếp diễn ra đầu tiên vì đây là điều kiện thành công đơn giản nhất. Nếu bức tường không giao với đoạn trực tiếp thì logic phản chiếu không có vấn đề gì. 

các`reflect_point`Hàm thực hiện phép chiếu trực giao lên đường gương và sau đó phản chiếu điểm trên hình chiếu. Điều này tránh hoàn toàn việc tính toán góc. 

các`line_intersection`tính toán điểm giao nhau duy nhất giữa tia nhìn mở ra và đường gương. Nếu mẫu số bằng 0 thì các đường thẳng song song và sự phản chiếu không hợp lệ. 

Việc kiểm tra giao lộ trên tường sau khi tìm thấy điểm phản chiếu rất tinh tế. Cấu trúc phản chiếu đảm bảo các góc bằng nhau, nhưng nó không tự động đảm bảo đường đi không bị cản trở. Mỗi nửa tia vật lý vẫn phải tránh bức tường. 

Một sai lầm phổ biến là quên rằng việc chạm vào được coi là chặn. Quy trình giao cắt đoạn xử lý rõ ràng điểm tiếp xúc điểm cuối và sự chồng chéo thẳng hàng như là giao điểm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
-1 3
1 3
0 2 0 4
0 0 0 1
```| Bước | Giá trị | 
| --- | --- | 
| Phân khúc trực tiếp | từ (-1,3) đến (1,3) | 
| Ngã tư tường | Có | 
| Cùng một mặt gương | Có | 
| Peter phản ánh | (-1,3) | 
| Tia song song với gương | Có | 

Đầu ra:```
NO
```Con đường trực tiếp xuyên qua bức tường. Phản xạ cũng không thành công vì tia nhìn song song với đường gương$x=0$. Tuyên bố rõ ràng cấm phản ánh trong trường hợp này. 

### Ví dụ 2```
0 0
4 0
2 -1 2 1
1 -2 1 2
```| Bước | Giá trị | 
| --- | --- | 
| Phân khúc trực tiếp | từ (0,0) đến (4,0) | 
| Ngã tư tường | Có | 
| Cùng một mặt gương | Có | 
| Peter phản ánh | (-2,0) | 
| Điểm phản chiếu | (1,0) | 
| Điểm phản chiếu trên gương | Có | 
| Khối tường phản ánh đường đi | Không | 

Đầu ra:```
YES
```Bức tường chặn tầm nhìn trực tiếp. Sau khi phản xạ qua đường gương$x=1$, Peter ánh xạ tới$(-2,0)$. Đoạn thẳng từ Victor đến điểm phản xạ này chạm vào gương đúng lúc$(1,0)$, tạo ra một đường dẫn phản ánh hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Chỉ một số phép toán hình học cố định được thực hiện | 
| Không gian | O(1) | Không có cấu trúc dữ liệu phụ thuộc vào kích thước đầu vào | 

Bài toán chỉ chứa hai điểm và hai đoạn, do đó thuật toán thực hiện công không đổi bất kể độ lớn tọa độ. Giải pháp dễ dàng phù hợp với giới hạn 2 giây và 256 MB. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    EPS = 1e-9

    class Point:
        def __init__(self, x, y):
            self.x = float(x)
            self.y = float(y)

        def __add__(self, other):
            return Point(self.x + other.x, self.y + other.y)

        def __sub__(self, other):
            return Point(self.x - other.x, self.y - other.y)

        def __mul__(self, k):
            return Point(self.x * k, self.y * k)

    def dot(a, b):
        return a.x * b.x + a.y * b.y

    def cross(a, b):
        return a.x * b.y - a.y * b.x

    def orient(a, b, c):
        return cross(b - a, c - a)

    def on_segment(a, b, p):
        if abs(orient(a, b, p)) > EPS:
            return False

        return (
            min(a.x, b.x) - EPS <= p.x <= max(a.x, b.x) + EPS
            and min(a.y, b.y) - EPS <= p.y <= max(a.y, b.y) + EPS
        )

    def segments_intersect(a, b, c, d):
        o1 = orient(a, b, c)
        o2 = orient(a, b, d)
        o3 = orient(c, d, a)
        o4 = orient(c, d, b)

        if o1 * o2 < -EPS and o3 * o4 < -EPS:
            return True

        if abs(o1) <= EPS and on_segment(a, b, c):
            return True
        if abs(o2) <= EPS and on_segment(a, b, d):
            return True
        if abs(o3) <= EPS and on_segment(c, d, a):
            return True
        if abs(o4) <= EPS and on_segment(c, d, b):
            return True

        return False

    def line_intersection(a, b, c, d):
        ab = b - a
        cd = d - c
        denom = cross(ab, cd)

        if abs(denom) <= EPS:
            return None

        t = cross(c - a, cd) / denom
        return a + ab * t

    def reflect_point(p, a, b):
        ab = b - a
        ap = p - a

        t = dot(ap, ab) / dot(ab, ab)
        proj = a + ab * t

        return proj * 2 - p

    def same_side(p1, p2, a, b):
        o1 = orient(a, b, p1)
        o2 = orient(a, b, p2)

        return o1 * o2 > EPS

    input = sys.stdin.readline

    xv, yv = map(int, input().split())
    xp, yp = map(int, input().split())

    xw1, yw1, xw2, yw2 = map(int, input().split())
    xm1, ym1, xm2, ym2 = map(int, input().split())

    V = Point(xv, yv)
    P = Point(xp, yp)

    W1 = Point(xw1, yw1)
    W2 = Point(xw2, yw2)

    M1 = Point(xm1, ym1)
    M2 = Point(xm2, ym2)

    if not segments_intersect(V, P, W1, W2):
        return "YES\n"

    if not same_side(V, P, M1, M2):
        return "NO\n"

    PR = reflect_point(P, M1, M2)

    if abs(cross(PR - V, M2 - M1)) <= EPS:
        return "NO\n"

    R = line_intersection(V, PR, M1, M2)

    if R is None or not on_segment(M1, M2, R):
        return "NO\n"

    if segments_intersect(V, R, W1, W2):
        return "NO\n"

    if segments_intersect(R, P, W1, W2):
        return "NO\n"

    return "YES\n"

# provided sample
assert run(
"""-1 3
1 3
0 2 0 4
0 0 0 1
"""
) == "NO\n", "sample 1"

# direct visibility
assert run(
"""0 0
4 0
10 0 11 0
2 -1 2 1
"""
) == "YES\n", "direct visibility"

# valid reflection
assert run(
"""0 0
4 0
2 -1 2 1
1 -2 1 2
"""
) == "YES\n", "reflection works"

# opposite sides of mirror
assert run(
"""-1 1
1 -1
10 0 11 0
0 -2 0 2
"""
) == "NO\n", "same-side condition"

# touching wall endpoint blocks
assert run(
"""0 0
4 0
2 -1 2 0
10 0 11 0
"""
) == "NO\n", "endpoint touching blocks"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Trường hợp hiển thị trực tiếp | CÓ | Bức tường vắng mặt ở đoạn trực tiếp | 
| Trường hợp công trình phản ánh | CÓ | Xây dựng phản ánh đúng | 
| Trường hợp bên đối diện | KHÔNG | Hạn chế gương cùng phía | 
| Trường hợp chạm vào điểm cuối | KHÔNG | Giao lộ ranh giới được tính là chặn | 

## Vỏ cạnh 

Hãy xem xét kịch bản chạm vào điểm cuối:```
0 0
4 0
2 -1 2 0
10 0 11 0
```Đoạn trực tiếp từ$(0,0)$ĐẾN$(4,0)$chạm vào tường lúc$(2,0)$. Các thử nghiệm định hướng tạo ra một cấu hình thẳng hàng, và`on_segment`trả về đúng. Bởi vì việc triển khai coi liên hệ điểm cuối là giao điểm nên câu trả lời sẽ trở thành`NO`, phù hợp với tuyên bố. 

Bây giờ hãy xem xét trường hợp song song với gương:```
0 0
4 0
2 -1 2 1
1 0 3 0
```Gương nằm ngang trên cùng một đường thẳng với đoạn quan sát. Cấu trúc điểm phản xạ tạo ra một tia song song với gương. Tích số chéo giữa hướng nhìn và hướng gương trở thành 0, gây ra tình trạng loại bỏ rõ ràng. Sự phản ánh bị cấm ở đây. 

Cuối cùng, hãy xem xét các cạnh đối diện của gương:```
-1 1
1 -1
10 0 11 0
0 -2 0 2
```Victor và Peter nằm đối diện nhau trên đường gương thẳng đứng$x=0$. Các dấu hiệu định hướng khác nhau, vì vậy`same_side`trả về sai ngay lập tức. Thuật toán từ chối chính xác nỗ lực phản ánh trước khi tính toán thêm.
