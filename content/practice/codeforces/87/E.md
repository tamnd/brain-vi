---
title: "CF 87E - Thần Tượng Mogohu-Rea"
description: "Chúng ta có ba đa giác lồi trên mặt phẳng. Từ mỗi đa giác chúng ta phải chọn một điểm và trung bình của ba điểm được chọn này phải bằng vị trí của thần tượng."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "geometry"]
categories: ["algorithms"]
codeforces_contest: 87
codeforces_index: "E"
codeforces_contest_name: "Codeforces Beta Round 73 (Div. 1 Only)"
rating: 2600
weight: 87
solve_time_s: 196
verified: true
draft: false
---

[CF 87E - Thần tượng Mogohu-Rea](https://codeforces.com/problemset/problem/87/E) 

**Đánh giá:** 2600 
**Thẻ:** hình học 
**Thời gian giải:** 3 phút 16s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có ba đa giác lồi trên mặt phẳng. Từ mỗi đa giác chúng ta phải chọn một điểm và trung bình của ba điểm được chọn này phải bằng vị trí của thần tượng. 

Nếu các điểm bàn thờ được chọn là$A, B, C$, và thần tượng đang ở điểm$P$, điều kiện là$$\frac{A+B+C}{3}=P$$có thể được viết lại thành$$A+B+C=3P$$Đối với mỗi ngọn đồi một cách độc lập, chúng ta phải xác định xem liệu bộ ba điểm đó có tồn tại hay không. 

Các đa giác đều lồi và có thể có tới$5 \cdot 10^4$mỗi đỉnh. Số lượng đồi lên đến$10^5$. Các giới hạn này ngay lập tức loại trừ mọi thứ xử lý từng truy vấn đối với mọi cạnh đa giác trong thời gian tuyến tính. Thậm chí$O(nm)$sẽ ở xung quanh$10^{10}$hoạt động trong trường hợp xấu nhất. 

Cấu trúc hình học quan trọng hơn kích thước thô. Đa giác lồi hoạt động rất tốt dưới tổng Minkowski và điều kiện$$A+B+C=3P$$đề nghị chuyển vấn đề thành truy vấn điểm trong đa giác lồi. 

Điểm tinh tế đầu tiên là các vị trí bàn thờ không bị giới hạn ở các đỉnh đa giác. Chúng có thể nằm ở bất cứ đâu bên trong đa giác, kể cả trên đường biên. Giải pháp chỉ kiểm tra các đỉnh là không chính xác. 

Coi như:```
Triangle 1: (0,0) (2,0) (0,2)
Triangle 2: (0,0) (2,0) (0,2)
Triangle 3: (0,0) (2,0) (0,2)
Hill: (1,1)
```điểm$(1,1)$có thể đạt được vì chúng ta có thể chọn cả ba điểm bàn thờ để làm$(1,1)$, nằm bên trong tam giác. Việc giới hạn bản thân ở các đỉnh sẽ trả lời sai KHÔNG. 

Điểm tinh tế thứ hai là các đa giác có thể giao nhau hoặc thậm chí chứa lẫn nhau. Không có giả định tách biệt tồn tại. Bất kỳ cách tiếp cận nào dựa trên sự rời rạc đều thất bại ngay lập tức. 

Một sai lầm dễ mắc phải khác là quên rằng các điểm ranh giới được cho phép. Giả sử một đa giác đóng góp suy biến thông qua điểm cuối cạnh.```
Triangle 1: (0,0) (2,0) (0,2)
Triangle 2: (0,0) (2,0) (0,2)
Triangle 3: (0,0) (2,0) (0,2)
Hill: (0,0)
```Chọn cả ba bàn thờ tại$(0,0)$là hợp lệ, vì vậy câu trả lời đúng là CÓ. Một bài kiểm tra nội thất nghiêm ngặt sẽ từ chối nó một cách không chính xác. 

Khu vực nguy hiểm cuối cùng là độ chính xác. Tọa độ lớn như$5 \cdot 10^8$và tổng Minkowski có thể tạm thời tạo ra các giá trị xung quanh$1.5 \cdot 10^9$. Ở đây, hình học dấu phẩy động là không cần thiết và có nguy cơ xảy ra lỗi biên. Các sản phẩm chéo số nguyên vừa khít một cách an toàn bên trong các số nguyên có dấu 64 bit, vì vậy toàn bộ giải pháp chỉ nên ở dạng số nguyên. 

## Phương pháp tiếp cận 

Việc giải thích bạo lực tuân theo định nghĩa trực tiếp. Đối với mỗi điểm đồi$P$, chúng ta sẽ cố gắng xác định liệu có tồn tại điểm$A \in S_1$,$B \in S_2$, Và$C \in S_3$như vậy$$A+B+C=3P$$Nếu chúng ta giới hạn bản thân ở các đỉnh đa giác, chúng ta có thể liệt kê tất cả các bộ ba đỉnh, nhưng điều đó là không thể. Mỗi đa giác có thể có$5 \cdot 10^4$đỉnh, do đó số lượng bộ ba trở nên gần đúng$$(5 \cdot 10^4)^3 \approx 10^{14}$$và điều này thậm chí vẫn không giải quyết được vấn đề thực sự vì các điểm bàn thờ hợp lệ không bị giới hạn ở các đỉnh. 

Quan sát quan trọng là phương trình đã mô tả tổng Minkowski. 

Định nghĩa$$S = S_1 + S_2 + S_3$$trong đó tổng Minkowski chứa mọi điểm được biểu diễn dưới dạng$$A+B+C$$với$A \in S_1$,$B \in S_2$,$C \in S_3$. 

Sau đó, truy vấn trở nên cực kỳ đơn giản:$$3P \in S$$Vì vậy, thay vì giải một bài toán tồn tại hình học mới cho mỗi ngọn đồi, chúng ta có thể tính toán trước vùng kết hợp một lần và mỗi truy vấn sẽ giảm xuống thành một phép thử đa giác điểm trong lồi. 

Điều này hoạt động rất tốt vì đa giác lồi được đóng dưới tổng Minkowski. Tổng các đa giác lồi lại là đa giác lồi. Thậm chí còn tốt hơn nữa, tổng Minkowski của các đa giác lồi có thể được tính theo thời gian tuyến tính bằng cách sử dụng kỹ thuật hợp nhất góc cạnh. 

Giả sử ba đa giác chứa$n_1, n_2, n_3$đỉnh. Tổng Minkowski của họ có nhiều nhất$$n_1+n_2+n_3$$các đỉnh sau khi loại bỏ các dư thừa thẳng hàng. Điều đó có nghĩa là chúng ta có thể xây dựng đa giác cuối cùng một lần theo thời gian tuyến tính và sau đó trả lời từng truy vấn theo thời gian logarit bằng cách đưa vào điểm tìm kiếm nhị phân. 

Cách tiếp cận bạo lực không thành công vì nó liên tục tái tạo lại các mối quan hệ hình học một cách độc lập cho mọi truy vấn. Quan điểm tổng Minkowski nén toàn bộ vùng khả thi thành một đa giác lồi. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(m \cdot n_1 n_2 n_3)$|$O(1)$| Quá chậm | 
| Tối ưu |$O(n_1+n_2+n_3+m\log N)$|$O(N)$| Đã chấp nhận | 

Đây$N$là số đỉnh của đa giác tổng Minkowski cuối cùng. 

## Hướng dẫn thuật toán 

1. Đọc ba đa giác lồi. 

Các đỉnh đã được đưa ra theo thứ tự ngược chiều kim đồng hồ, đó chính xác là những gì thuật toán Minkowski-sum cần. 
2. Xoay từng đa giác sao cho đỉnh nhỏ nhất theo từ điển của nó xuất hiện trước. 

Điều này mang lại cho mỗi đa giác một vị trí bắt đầu chuẩn. Sau phép quay này, các vectơ cạnh xuất hiện theo thứ tự các góc được sắp xếp xung quanh đa giác. 
3. Chuyển đổi mỗi đa giác thành một dãy cạnh tuần hoàn. 

Nếu các đỉnh là$p_0, p_1, \dots, p_{n-1}$, thì các cạnh là$$p_{i+1}-p_i$$với sự bao quanh ở cuối. 
4. Tính tổng Minkowski của hai đa giác bằng cách hợp nhất các cạnh. 

Bắt đầu từ tổng của hai đỉnh nhỏ nhất. Sau đó đồng thời đi qua các chuỗi cạnh. 

Ở mỗi bước, hãy so sánh tích chéo của các vectơ cạnh hiện tại. 

Nếu tích chéo là dương, hãy nâng cao đa giác đầu tiên. 

Nếu âm, tiến lên đa giác thứ hai. 

Nếu không, tiến cả hai. 

Về mặt tinh thần, điều này giống hệt với việc hợp nhất hai mảng đã sắp xếp, ngoại trừ việc sắp xếp thứ tự theo góc cạnh. 
5. Tính tổng ba đa giác. 

Tính toán đầu tiên$$T=S_1+S_2$$sau đó tính toán$$S=T+S_3$$6. Loại bỏ các đỉnh thẳng hàng dư thừa khỏi đa giác thu được. 

Các cạnh thẳng hàng liên tiếp không ảnh hưởng đến hình dạng nhưng làm phức tạp các truy vấn điểm. 
7. Đối với mỗi điểm đồi$P$, chia tỷ lệ lên 3. 

Điều kiện khả thi là$$3P \in S$$vì vậy chúng tôi truy vấn điểm$$Q=(3x,3y)$$8. Thực hiện kiểm tra đa giác điểm trong lồi theo thời gian logarit. 

Cố định đỉnh đầu tiên làm gốc của tam giác hình quạt. 

Điểm loại bỏ đầu tiên nằm ngoài phạm vi góc được hình thành bởi cạnh đầu tiên và cuối cùng. 

Sau đó tìm kiếm nhị phân khu vực tam giác chứa điểm truy vấn. 

Cuối cùng kiểm tra xem điểm có nằm trong tam giác đó hay không bằng cách sử dụng tích chéo. 
9. In CÓ nếu điểm nằm bên trong hoặc trên ranh giới của đa giác tổng Minkowski, nếu không thì in NO. 

### Tại sao nó hoạt động 

Tổng Minkowski$$S_1+S_2+S_3$$chứa chính xác tất cả các điểm có thể biểu diễn dưới dạng$$A+B+C$$với một điểm được chọn từ mỗi đa giác. 

Điều kiện thần tượng đòi hỏi$$A+B+C=3P$$điều đó có nghĩa là ngọn đồi khả thi chính xác khi$3P$thuộc đa giác tổng Minkowski. 

Bởi vì mọi đa giác đều lồi nên tổng Minkowski của chúng cũng lồi. Thuật toán hợp nhất góc cạnh xây dựng ranh giới của nó một cách chính xác bằng cách đi qua các cạnh hỗ trợ theo thứ tự góc tăng dần. Các truy vấn điểm trong đa giác lồi là chính xác vì mọi đa giác lồi có thể được phân chia thành các hình tam giác không chồng lên nhau có chung một đỉnh neo. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def cross(a, b):
    return a[0] * b[1] - a[1] * b[0]

def sub(a, b):
    return (a[0] - b[0], a[1] - b[1])

def add(a, b):
    return (a[0] + b[0], a[1] + b[1])

def rotate_smallest(poly):
    idx = min(range(len(poly)), key=lambda i: (poly[i][0], poly[i][1]))
    return poly[idx:] + poly[:idx]

def compress(poly):
    res = []
    n = len(poly)

    for p in poly:
        while len(res) >= 2:
            a = res[-2]
            b = res[-1]
            if cross(sub(b, a), sub(p, b)) == 0:
                res.pop()
            else:
                break
        res.append(p)

    while len(res) >= 3:
        a = res[-2]
        b = res[-1]
        c = res[0]
        if cross(sub(b, a), sub(c, b)) == 0:
            res.pop()
        else:
            break

    return res

def minkowski(A, B):
    A = rotate_smallest(A)
    B = rotate_smallest(B)

    n = len(A)
    m = len(B)

    ea = [sub(A[(i + 1) % n], A[i]) for i in range(n)]
    eb = [sub(B[(i + 1) % m], B[i]) for i in range(m)]

    i = j = 0

    cur = add(A[0], B[0])
    res = [cur]

    while i < n or j < m:
        va = ea[i % n] if i < n else None
        vb = eb[j % m] if j < m else None

        if i == n:
            take_a = False
        elif j == m:
            take_a = True
        else:
            cr = cross(va, vb)
            if cr > 0:
                take_a = True
            elif cr < 0:
                take_a = False
            else:
                cur = add(cur, add(va, vb))
                res.append(cur)
                i += 1
                j += 1
                continue

        if take_a:
            cur = add(cur, va)
            i += 1
        else:
            cur = add(cur, vb)
            j += 1

        res.append(cur)

    res.pop()
    return compress(res)

def point_in_convex(poly, p):
    n = len(poly)

    if n == 1:
        return p == poly[0]

    if cross(sub(poly[1], poly[0]), sub(p, poly[0])) < 0:
        return False

    if cross(sub(poly[-1], poly[0]), sub(p, poly[0])) > 0:
        return False

    l = 1
    r = n - 1

    while r - l > 1:
        mid = (l + r) // 2

        if cross(sub(poly[mid], poly[0]), sub(p, poly[0])) >= 0:
            l = mid
        else:
            r = mid

    a = poly[0]
    b = poly[l]
    c = poly[(l + 1) % n]

    return cross(sub(b, a), sub(p, a)) >= 0 and \
           cross(sub(c, b), sub(p, b)) >= 0 and \
           cross(sub(a, c), sub(p, c)) >= 0

def read_poly():
    n = int(input())
    return [tuple(map(int, input().split())) for _ in range(n)]

p1 = read_poly()
input()

p2 = read_poly()
input()

p3 = read_poly()
input()

sum12 = minkowski(p1, p2)
total = minkowski(sum12, p3)

m = int(input())

out = []

for _ in range(m):
    x, y = map(int, input().split())
    q = (3 * x, 3 * y)

    if point_in_convex(total, q):
        out.append("YES")
    else:
        out.append("NO")

print("\n".join(out))
```Việc thực hiện tuân theo cấu trúc toán học trực tiếp. 

các`rotate_smallest`hàm bình thường hóa biểu diễn đa giác. Việc hợp nhất cạnh Minkowski chỉ hoạt động rõ ràng khi cả hai đa giác đều bắt đầu từ đỉnh nhỏ nhất về mặt từ điển của chúng. 

các`minkowski`hàm thực hiện hợp nhất tuyến tính. Mỗi đa giác đóng góp một chuỗi tuần hoàn các vectơ cạnh được sắp xếp theo góc. Logic hợp nhất tiến tới bất kỳ cạnh nào có góc cực nhỏ hơn, được phát hiện bằng cách sử dụng tích chéo. Khi hai cạnh song song và hướng về cùng một hướng, cả hai đều bị tiêu hao cùng nhau. 

Kết quả có thể chứa các đỉnh thẳng hàng không cần thiết. các`compress`chức năng loại bỏ chúng bằng cách sử dụng kiểm tra định hướng. Điều này quan trọng vì truy vấn điểm giả định cấu trúc hình quạt lồi hoàn toàn. 

Bài kiểm tra bao gồm điểm chỉ sử dụng số học số nguyên. Đa giác được coi như một hình quạt bắt nguồn từ`poly[0]`. Tìm kiếm nhị phân xác định khu vực tam giác duy nhất chứa điểm truy vấn, sau đó ba phép kiểm tra định hướng xác định xem điểm nằm bên trong hay trên đường biên. 

Một chi tiết tinh tế là việc sử dụng các bất đẳng thức không chặt chẽ. Các điểm ranh giới có giá trị theo tuyên bố, vì vậy mọi thử nghiệm ngăn chặn phải chấp nhận tích số chéo bằng 0. 

Một điểm tinh tế khác là an toàn tràn. Các số nguyên Python có độ chính xác tùy ý, nhưng ngay cả trong C++, tất cả các phép tính đều khớp với các số nguyên 64 bit có dấu vì cường độ tọa độ luôn ở dưới mức đại khái$10^{18}$sau khi nhân tích trong chéo. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3
0 0
1 0
1 1

4
8 8
5 5
6 4
8 4

3
-1 -1
-3 -1
-2 -2

5
0 0
2 1
7 1
1 1
5 3
```Thuật toán đầu tiên xây dựng đa giác tổng Minkowski. 

Một truy vấn thành công là ngọn đồi$(2,1)$. 

Chúng tôi kiểm tra điểm:$$Q=(6,3)$$Các điểm bàn thờ được chọn từ tuyên bố là:$$(1,0), (7,5), (-2,-2)$$Tổng của chúng là:$$(1,0)+(7,5)+(-2,-2)=(6,3)$$cái nào bằng$3(2,1)$. 

| Đồi truy vấn | Truy vấn theo tỷ lệ | Bên trong Minkowski Sum | Trả lời | 
| --- | --- | --- | --- | 
| (0,0) | (0,0) | Không | KHÔNG | 
| (2,1) | (6,3) | Có | CÓ | 
| (7,1) | (21,3) | Không | KHÔNG | 
| (1,1) | (3,3) | Có | CÓ | 
| (5,3) | (15,9) | Không | KHÔNG | 

Điều này chứng tỏ sự giảm nhẹ trọng tâm của vấn đề. Chúng tôi không bao giờ tìm kiếm bộ ba bàn thờ một cách rõ ràng. Toàn bộ vùng khả thi được mã hóa thành một đa giác lồi. 

### Ví dụ tùy chỉnh 

Hãy xem xét ba hình vuông đơn vị giống hệt nhau:```
4
0 0
1 0
1 1
0 1
```Tổng Minkowski của chúng trở thành bình phương:```
0 ≤ x ≤ 3
0 ≤ y ≤ 3
```Bây giờ hãy truy vấn ngọn đồi$(1,1)$. 

| Bước | Giá trị | 
| --- | --- | 
| Đồi | (1,1) | 
| Điểm tỷ lệ | (3,3) | 
| Giới hạn Minkowski | 0  x 3, 0  y  3 | 
| Kết quả đưa vào | Điểm ranh giới | 
| Trả lời | CÓ | 

Dấu vết này xác nhận rằng các điểm biên được chấp nhận chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n_1+n_2+n_3+m\log N)$| Truy vấn điểm logarit và xây dựng Minkowski tuyến tính | 
| Không gian |$O(N)$| Lưu trữ đa giác lồi cuối cùng | 

Tổng số đỉnh đa giác nhiều nhất là$1.5 \cdot 10^5$, do đó việc xây dựng Minkowski tuyến tính dễ dàng đủ nhanh. Mỗi trong số$10^5$truy vấn hill chỉ tốn thời gian logarit, phù hợp thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def cross(a, b):
        return a[0] * b[1] - a[1] * b[0]

    def sub(a, b):
        return (a[0] - b[0], a[1] - b[1])

    def add(a, b):
        return (a[0] + b[0], a[1] + b[1])

    def rotate_smallest(poly):
        idx = min(range(len(poly)), key=lambda i: (poly[i][0], poly[i][1]))
        return poly[idx:] + poly[:idx]

    def compress(poly):
        res = []

        for p in poly:
            while len(res) >= 2:
                a = res[-2]
                b = res[-1]

                if cross(sub(b, a), sub(p, b)) == 0:
                    res.pop()
                else:
                    break

            res.append(p)

        return res

    def minkowski(A, B):
        A = rotate_smallest(A)
        B = rotate_smallest(B)

        n = len(A)
        m = len(B)

        ea = [sub(A[(i + 1) % n], A[i]) for i in range(n)]
        eb = [sub(B[(i + 1) % m], B[i]) for i in range(m)]

        i = j = 0

        cur = add(A[0], B[0])
        res = [cur]

        while i < n or j < m:
            va = ea[i % n] if i < n else None
            vb = eb[j % m] if j < m else None

            if i == n:
                take_a = False
            elif j == m:
                take_a = True
            else:
                cr = cross(va, vb)

                if cr > 0:
                    take_a = True
                elif cr < 0:
                    take_a = False
                else:
                    cur = add(cur, add(va, vb))
                    res.append(cur)
                    i += 1
                    j += 1
                    continue

            if take_a:
                cur = add(cur, va)
                i += 1
            else:
                cur = add(cur, vb)
                j += 1

            res.append(cur)

        res.pop()
        return compress(res)

    def point_in_convex(poly, p):
        n = len(poly)

        if cross(sub(poly[1], poly[0]), sub(p, poly[0])) < 0:
            return False

        if cross(sub(poly[-1], poly[0]), sub(p, poly[0])) > 0:
            return False

        l, r = 1, n - 1

        while r - l > 1:
            mid = (l + r) // 2

            if cross(sub(poly[mid], poly[0]), sub(p, poly[0])) >= 0:
                l = mid
            else:
                r = mid

        a = poly[0]
        b = poly[l]
        c = poly[(l + 1) % n]

        return cross(sub(b, a), sub(p, a)) >= 0 and \
               cross(sub(c, b), sub(p, b)) >= 0 and \
               cross(sub(a, c), sub(p, c)) >= 0

    def read_poly():
        n = int(input())
        return [tuple(map(int, input().split())) for _ in range(n)]

    p1 = read_poly()
    input()

    p2 = read_poly()
    input()

    p3 = read_poly()
    input()

    total = minkowski(minkowski(p1, p2), p3)

    m = int(input())

    ans = []

    for _ in range(m):
        x, y = map(int, input().split())

        if point_in_convex(total, (3 * x, 3 * y)):
            ans.append("YES")
        else:
            ans.append("NO")

    return "\n".join(ans)

# sample 1
assert run("""3
0 0
1 0
1 1

4
8 8
5 5
6 4
8 4

3
-1 -1
-3 -1
-2 -2

5
0 0
2 1
7 1
1 1
5 3
""") == """NO
YES
NO
YES
NO"""

# minimum triangles
assert run("""3
0 0
1 0
0 1

3
0 0
1 0
0 1

3
0 0
1 0
0 1

1
0 0
""") == "YES"

# boundary point
assert run("""4
0 0
1 0
1 1
0 1

4
0 0
1 0
1 1
0 1

4
0 0
1 0
1 1
0 1

1
1 1
""") == "YES"

# outside point
assert run("""4
0 0
1 0
1 1
0 1

4
0 0
1 0
1 1
0 1

4
0 0
1 0
1 1
0 1

1
2 2
""") == "NO"

# negative coordinates
assert run("""3
-1 -1
0 -1
-1 0

3
-1 -1
0 -1
-1 0

3
-1 -1
0 -1
-1 0

1
-1 -1
""") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Ba hình tam giác tối thiểu giống hệt nhau | CÓ | Đa giác hợp lệ nhỏ nhất | 
| Truy vấn ranh giới trên hình vuông tổng hợp | CÓ | Bao gồm ranh giới | 
| Truy vấn bên ngoài ô vuông tổng hợp | KHÔNG | Từ chối đúng | 
| Tam giác tọa độ âm | CÓ | Tính đúng đắn của số học đã ký | 

## Vỏ cạnh 

Hãy xem xét trường hợp bao gồm ranh giới:```
3
0 0
1 0
0 1

3
0 0
1 0
0 1

3
0 0
1 0
0 1

1
0 0
```Điểm truy vấn được chia tỷ lệ vẫn là$(0,0)$. Điểm này nằm chính xác tại một đỉnh của đa giác tổng Minkowski. Kiểm tra điểm trong đa giác chấp nhận nó vì mọi kiểm tra hướng đều sử dụng`>= 0`thay vì`> 0`. Thuật toán in chính xác CÓ. 

Bây giờ hãy xem xét một ví dụ trong đó chỉ các điểm bên trong mới hoạt động:```
3
0 0
2 0
0 2

3
0 0
2 0
0 2

3
0 0
2 0
0 2

1
1 1
```Truy vấn được chia tỷ lệ trở thành$(3,3)$. Không có ba đỉnh nào có tổng bằng$(3,3)$, vì vậy mọi cách tiếp cận chỉ theo đỉnh đều thất bại. Nhưng tổng Minkowski chứa tất cả các tổ hợp lồi chứ không chỉ tổng các đỉnh. Từ$(1,1)$nằm bên trong mỗi hình tam giác, việc chọn cả ba điểm thờ ở đó sẽ mang lại$$(1,1)+(1,1)+(1,1)=(3,3)$$và thuật toán xuất ra chính xác CÓ. 

Cuối cùng, hãy xem xét các đa giác giao nhau:```
3
0 0
2 0
0 2

3
1 0
3 0
1 2

3
0 1
2 1
0 3

1
1 1
```Các đa giác chồng lên nhau rất nhiều. Thuật toán không dựa vào sự rời rạc ở bất cứ đâu. Tổng Minkowski vẫn hợp lệ đối với các tập lồi tùy ý, do đó, cấu trúc và logic truy vấn tương tự vẫn hoạt động không thay đổi.
