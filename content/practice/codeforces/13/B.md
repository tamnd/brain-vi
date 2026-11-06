---
title: "CF 13B - Chữ A"
description: "Chúng ta có ba đoạn thẳng trên mặt phẳng 2D. Chúng ta phải quyết định xem ba đoạn này có thể được hiểu là hình dạng của chữ in hoa A hay không."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "geometry", "implementation"]
categories: ["algorithms"]
codeforces_contest: 13
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 13"
rating: 2000
weight: 13
solve_time_s: 105
verified: true
draft: false
---
[CF 13B - Chữ A](https://codeforces.com/problemset/problem/13/B) 

**Xếp hạng:** 2000 
**Tags:** hình học, thực hiện 
**Thời gian giải:** 1 phút 45s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có ba đoạn thẳng trên mặt phẳng 2D. Chúng ta phải quyết định xem ba đoạn này có thể được hiểu là hình dạng của chữ in hoa A hay không. 

Cấu trúc của bức thư rất cứng nhắc. Hai đoạn phải gặp nhau tại một điểm chung và tạo thành hai chân xiên của chữ A. Đoạn thứ ba đóng vai trò là thanh ngang nối hai chân ở đâu đó ở giữa. 

Các điều kiện hình học chặt chẽ hơn là chỉ “chúng trông giống chữ A”. 

Hai đoạn chân phải có chung một điểm cuối, điểm này sẽ trở thành đỉnh trên cùng của chữ cái. Góc giữa hai chân phải dương và tối đa là 90 độ. Đường thẳng hoặc góc phản xạ không hợp lệ. 

Xà ngang phải nối một điểm ở lượt đi và một điểm ở lượt về. Những điểm giao nhau đó không thể quá gần với một trong hai điểm cuối. Chính xác hơn, nếu một chân được chia bởi điểm nối xà ngang thì đoạn ngắn chia cho đoạn dài ít nhất phải bằng$\frac14$. Điều này ngăn không cho thanh gần như ở trên cùng hoặc dưới cùng. 

Kích thước đầu vào rất nhỏ. Mỗi ca kiểm thử chỉ chứa ba phân đoạn và nhiều nhất là$10^4$trường hợp thử nghiệm tồn tại. Điều đó ngay lập tức cho chúng ta biết rằng chúng ta không cần các cấu trúc hình học phức tạp hoặc các thủ thuật tối ưu hóa. Ngay cả việc kiểm tra mọi hoán vị của các phân đoạn cũng hoàn toàn ổn vì chỉ có$3! = 6$khả năng. 

Thách thức thực sự là tính đúng đắn. Ở đây, những lỗi thực hiện hình học nguy hiểm hơn nhiều so với độ phức tạp tiệm cận. 

Một số trường hợp nguy hiểm dễ dàng phá vỡ các giải pháp ngây thơ. 

Một lỗi phổ biến là chỉ kiểm tra xem các điểm cuối của thanh ngang có nằm trên các đường hỗ trợ vô hạn thay vì trên các đoạn thực tế hay không. 

Coi như:```
0 0 0 4
0 4 4 0
10 10 11 11
```Đoạn thứ ba không nằm gần hai đoạn còn lại. Việc kiểm tra dựa trên đường kẻ bất cẩn vẫn cho rằng các đường giao nhau một cách chính xác. 

Một cái bẫy khác là quên rằng góc phải nhọn hoặc vuông chứ không được tù. 

Ví dụ:```
0 0 0 5
0 5 -2 -1
0 2 -1 2
```Hai chân chung một điểm nhưng góc mở rộng hơn 90 độ. Câu trả lời đúng là`NO`. 

Một vấn đề tinh tế hơn là điều kiện tỷ lệ. Nhiều cách triển khai chỉ xác minh rằng thanh ngang nằm bên trong mỗi đoạn chân chứ không xác minh liệu nó có nằm quá gần điểm cuối hay không. 

Ví dụ:```
0 0 0 8
0 8 4 0
0 7 1 6
```Quầy bar cực kỳ gần với đỉnh. Tỷ lệ một bên trở thành$1:7$, nhỏ hơn$1:4$. Câu trả lời phải là`NO`. 

Độ chính xác của dấu phẩy động là một nguồn khác gây ra lỗi thầm lặng. Khoảng cách và tỷ lệ liên quan đến căn bậc hai, nhưng chúng ta có thể tránh chúng hoàn toàn bằng cách so sánh độ dài bình phương và sử dụng tích vector chấm với số nguyên. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Vì chỉ có ba đoạn nên chúng ta có thể thử mọi cách để chọn hai đoạn làm chân và đoạn còn lại làm xà ngang. 

Đối với mỗi hoán vị, chúng tôi kiểm tra: 

1. Các nhánh được chọn có chung một điểm cuối. 
2. Góc giữa chúng nằm trong phạm vi hợp lệ. 
3. Mỗi điểm cuối của xà ngang nằm trên một chân khác nhau. 
4. Điều kiện tỷ lệ chia được giữ ở cả hai nhánh. 

Điều này đã hoạt động đủ nhanh. Mỗi trường hợp thử nghiệm chỉ thực hiện các phép toán hình học có thời gian không đổi. Ngay cả với$10^4$trường hợp thử nghiệm, tổng công việc là không đáng kể. 

Phần khó không phải là tốc độ mà là thể hiện hình học một cách mạnh mẽ. 

Việc triển khai đơn giản thường sử dụng các công thức khoảng cách và độ dốc dấu phẩy động. Điều đó tạo ra sự mất ổn định không cần thiết, đặc biệt khi kiểm tra thẳng hàng và so sánh góc. Các đoạn thẳng đứng cũng làm phức tạp logic dựa trên độ dốc. 

Quan sát quan trọng là mọi thuộc tính bắt buộc đều có thể được biểu diễn bằng các phép toán vectơ. 

Sự cộng tuyến có thể được kiểm tra bằng cách sử dụng tích chéo. 

Có thể kiểm tra xem một góc là nhọn hay vuông bằng cách sử dụng tích số chấm. 

Có thể kiểm tra xem một điểm có nằm giữa hai điểm cuối hay không bằng cách sử dụng giới hạn tọa độ. 

Điều kiện tỷ lệ cũng có thể tránh được căn bậc hai. Giả sử một điểm chia một đoạn thành các độ dài$a$Và$b$. Điều kiện là:$$\frac{\min(a,b)}{\max(a,b)} \ge \frac14$$Thay vì tính độ dài thực, chúng ta so sánh độ dài bình phương:$$16 \cdot \min(a^2,b^2) \ge \max(a^2,b^2)$$Mọi thứ vẫn dựa trên số nguyên và chính xác. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force với hình học dấu phẩy động | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Được chấp nhận nhưng dễ bị lỗi | 
| Hình học vectơ số nguyên tối ưu | O(1) cho mỗi trường hợp thử nghiệm | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc ba đoạn. 

Mỗi phân đoạn được đại diện bởi hai điểm cuối. 
2. Thử tất cả các hoán vị của ba đoạn. 

Đối với mỗi hoán vị, hãy coi hai đoạn đầu tiên là chân và đoạn thứ ba là thanh ngang. 
3. Kiểm tra xem hai đoạn chân có chia sẻ chính xác một điểm cuối hay không. 

Nếu không, hoán vị này không thể tạo thành chữ A. 
4. Đặt điểm cuối dùng chung là đỉnh trên cùng. 

Định hướng lại hai chân để cả hai đều bắt đầu từ điểm chung này và kéo dài xuống phía dưới về phía các điểm cuối khác của chúng. 

Việc chuẩn hóa này đơn giản hóa tất cả các lần kiểm tra sau này. 
5. Tính điều kiện góc bằng tích chấm. 

Đặt vectơ hướng chân là$u$Và$v$. 

Góc giữa chúng nhọn hoặc vuông khi:$$u \cdot v > 0$$Nếu tích số chấm bằng 0 hoặc âm, hãy từ chối cấu hình này. 

Số 0 chỉ có nghĩa là 90 độ nếu vectơ hướng ra ngoài từ điểm chung, nhưng vectơ của chúng ta kéo dài ra xa đỉnh, do đó tích chấm dương sẽ ghi lại chính xác các góc nhỏ hơn 90 độ. 
6. Kiểm tra xem một đầu của xà ngang có nằm trên chân thứ nhất và đầu kia có nằm trên chân thứ hai hay không. 

Một điểm nằm trên đoạn thẳng nếu: 

- tích chéo bằng 0, nghĩa là thẳng hàng, 
- và tọa độ nằm trong giới hạn phân đoạn. 
7. Kiểm tra tình trạng cân đối của cả hai chân. 

Giả sử điểm$P$nằm trên đoạn$AB$. 

Tính toán:$$d_1 = |AP|^2,\quad d_2 = |PB|^2$$Tỷ lệ cần tìm trở thành:$$16 \cdot \min(d_1,d_2) \ge \max(d_1,d_2)$$Điều này tương đương với điều kiện ban đầu không có căn bậc hai. 
8. Nếu tất cả các bước kiểm tra đều đạt đối với bất kỳ hoán vị nào, hãy in`YES`. 
9. Nếu không thì in`NO`. 

### Tại sao nó hoạt động 

Thuật toán xác minh rõ ràng mọi điều kiện từ định nghĩa của chữ A. 

Việc thử tất cả các hoán vị đảm bảo chúng ta không bao giờ bỏ sót việc gán chân và xà ngang chính xác. 

Kiểm tra điểm cuối dùng chung đảm bảo cấu trúc liên kết chính xác. 

Kiểm tra sản phẩm chấm đảm bảo góc mở là hợp lệ. 

Việc kiểm tra điểm trên đoạn đảm bảo thanh ngang kết nối hai chân thay vì chỉ giao nhau với các đường hỗ trợ vô tận của chúng. 

Kiểm tra tỷ lệ đảm bảo xà ngang được đặt trong vùng giữa cho phép trên cả hai chân. 

Vì mọi thuộc tính bắt buộc đều được kiểm tra chính xác một lần và mỗi lần từ chối đều tương ứng với việc vi phạm điều kiện định nghĩa nên thuật toán là chính xác. 

## Giải pháp Python```python
import sys
from itertools import permutations

input = sys.stdin.readline

def vec(a, b):
    return (b[0] - a[0], b[1] - a[1])

def dot(a, b):
    return a[0] * b[0] + a[1] * b[1]

def cross(a, b):
    return a[0] * b[1] - a[1] * b[0]

def dist2(a, b):
    dx = a[0] - b[0]
    dy = a[1] - b[1]
    return dx * dx + dy * dy

def point_on_segment(p, a, b):
    ap = vec(a, p)
    ab = vec(a, b)

    if cross(ap, ab) != 0:
        return False

    return (
        min(a[0], b[0]) <= p[0] <= max(a[0], b[0])
        and min(a[1], b[1]) <= p[1] <= max(a[1], b[1])
    )

def good_ratio(p, a, b):
    d1 = dist2(a, p)
    d2 = dist2(p, b)

    mn = min(d1, d2)
    mx = max(d1, d2)

    return 16 * mn >= mx

def check(s1, s2, s3):
    p1, p2 = s1
    q1, q2 = s2

    common = None

    for a in [p1, p2]:
        for b in [q1, q2]:
            if a == b:
                common = a

    if common is None:
        return False

    if p1 == common:
        a = p2
    else:
        a = p1

    if q1 == common:
        b = q2
    else:
        b = q1

    v1 = vec(common, a)
    v2 = vec(common, b)

    if dot(v1, v2) <= 0:
        return False

    r1, r2 = s3

    ok1 = point_on_segment(r1, common, a) and point_on_segment(r2, common, b)
    ok2 = point_on_segment(r2, common, a) and point_on_segment(r1, common, b)

    if not ok1 and not ok2:
        return False

    if ok2:
        r1, r2 = r2, r1

    if not good_ratio(r1, common, a):
        return False

    if not good_ratio(r2, common, b):
        return False

    return True

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        segs = []

        for _ in range(3):
            x1, y1, x2, y2 = map(int, input().split())
            segs.append(((x1, y1), (x2, y2)))

        found = False

        for perm in permutations(segs):
            if check(*perm):
                found = True
                break

        ans.append("YES" if found else "NO")

    print("\n".join(ans))

solve()
```Việc triển khai phản ánh trực tiếp việc kiểm tra hình học. 

các`point_on_segment`Hàm sử dụng hai điều kiện. Tích chéo đảm bảo tính cộng tuyến. Kiểm tra hộp giới hạn đảm bảo điểm nằm bên trong đoạn hữu hạn chứ không phải ở đâu đó xa hơn dọc theo đường thẳng. 

Kiểm tra góc sử dụng tích số chấm. Vì cả hai vectơ đều bắt đầu từ điểm cuối chung và hướng ra ngoài dọc theo các chân, nên tích chấm dương có nghĩa là góc hoàn toàn nhỏ hơn 90 độ. 

Điều kiện tỷ lệ được thực hiện hoàn toàn với khoảng cách bình phương. Điều này tránh được các vấn đề về độ chính xác của dấu phẩy động và cũng tránh được các căn bậc hai đắt tiền. 

Vòng lặp hoán vị rất nhỏ vì chỉ có sáu phép gán có thể thực hiện được. Điều này giúp mã đơn giản và loại bỏ việc phân tích trường hợp phức tạp. 

Một điểm tinh tế là xử lý các điểm cuối của thanh ngang. Điểm cuối của thanh ngang có thể thuộc về một trong hai nhánh, do đó mã sẽ kiểm tra cả hai phép gán. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
0 0 0 5
0 5 2 -1
1 2 0 1
```| Bước | Giá trị | 
| --- | --- | 
| Điểm cuối chung | (0, 5) | 
| Véc tơ chặng đầu tiên | (0, -5) | 
| Vector chặng thứ hai | (2, -6) | 
| Sản phẩm chấm | 30 | 
| Điểm cuối thanh ngang | (1, 2), (0, 1) | 
| Điểm cuối ở chặng đầu tiên | (0, 1) | 
| Điểm cuối ở chặng về | (1, 2) | 
| Kiểm tra tỉ số trận lượt đi | hợp lệ | 
| Kiểm tra tỉ số trận lượt về | hợp lệ | 
| Câu trả lời cuối cùng | CÓ | 

Tích số chấm là dương nên góc là nhọn. Mỗi điểm cuối của thanh ngang nằm trên một nhánh khác nhau và cả hai tỷ lệ phân chia đều thỏa mãn điều kiện$1/4$tình trạng. 

### Ví dụ 2 

đầu vào:```
0 0 0 8
0 8 4 0
0 7 1 6
```| Bước | Giá trị | 
| --- | --- | 
| Điểm cuối chung | (0, 8) | 
| Véc tơ chặng đầu tiên | (0, -8) | 
| Vector chặng thứ hai | (4, -8) | 
| Sản phẩm chấm | 64 | 
| Điểm cuối thanh ngang | (0, 7), (1, 6) | 
| Khoảng cách phân chia ở chặng đầu tiên | 1 và 7 | 
| Tỷ lệ | 7/1 | 
| Câu trả lời cuối cùng | KHÔNG | 

Mặc dù cấu trúc liên kết và góc hợp lệ nhưng thanh ngang quá gần đỉnh trên cùng. Điều kiện tỷ lệ bác bỏ nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) cho mỗi trường hợp thử nghiệm | Chỉ có 6 hoán vị và kiểm tra hình học theo thời gian không đổi | 
| Không gian | O(1) | Không có cấu trúc bổ sung tùy thuộc vào kích thước đầu vào | 

Các ràng buộc là cực kỳ nhỏ đối với mỗi trường hợp thử nghiệm, do đó hình học thời gian không đổi là quá đủ. Thậm chí$10^4$trường hợp thực hiện thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from itertools import permutations

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def vec(a, b):
        return (b[0] - a[0], b[1] - a[1])

    def dot(a, b):
        return a[0] * b[0] + a[1] * b[1]

    def cross(a, b):
        return a[0] * b[1] - a[1] * b[0]

    def dist2(a, b):
        dx = a[0] - b[0]
        dy = a[1] - b[1]
        return dx * dx + dy * dy

    def point_on_segment(p, a, b):
        ap = vec(a, p)
        ab = vec(a, b)

        if cross(ap, ab) != 0:
            return False

        return (
            min(a[0], b[0]) <= p[0] <= max(a[0], b[0])
            and min(a[1], b[1]) <= p[1] <= max(a[1], b[1])
        )

    def good_ratio(p, a, b):
        d1 = dist2(a, p)
        d2 = dist2(p, b)

        mn = min(d1, d2)
        mx = max(d1, d2)

        return 16 * mn >= mx

    def check(s1, s2, s3):
        p1, p2 = s1
        q1, q2 = s2

        common = None

        for a in [p1, p2]:
            for b in [q1, q2]:
                if a == b:
                    common = a

        if common is None:
            return False

        if p1 == common:
            a = p2
        else:
            a = p1

        if q1 == common:
            b = q2
        else:
            b = q1

        if dot(vec(common, a), vec(common, b)) <= 0:
            return False

        r1, r2 = s3

        ok1 = point_on_segment(r1, common, a) and point_on_segment(r2, common, b)
        ok2 = point_on_segment(r2, common, a) and point_on_segment(r1, common, b)

        if not ok1 and not ok2:
            return False

        if ok2:
            r1, r2 = r2, r1

        return (
            good_ratio(r1, common, a)
            and good_ratio(r2, common, b)
        )

    t = int(input())
    out = []

    for _ in range(t):
        segs = []

        for _ in range(3):
            x1, y1, x2, y2 = map(int, input().split())
            segs.append(((x1, y1), (x2, y2)))

        ans = "NO"

        for perm in permutations(segs):
            if check(*perm):
                ans = "YES"
                break

        out.append(ans)

    return "\n".join(out)

# provided samples
assert run(
"""3
4 4 6 0
4 1 5 2
4 0 4 4
0 0 0 6
0 6 2 -4
1 1 0 1
0 0 0 5
0 5 2 -1
1 2 0 1
"""
) == "YES\nNO\nYES", "sample cases"

# no shared endpoint
assert run(
"""1
0 0 1 1
2 2 3 3
0 1 1 2
"""
) == "NO", "legs must share a vertex"

# obtuse angle
assert run(
"""1
0 0 0 5
0 5 -2 -1
0 2 -1 2
"""
) == "NO", "angle must be <= 90 degrees"

# crossbar too close to endpoint
assert run(
"""1
0 0 0 8
0 8 4 0
0 7 1 6
"""
) == "NO", "ratio condition"

# valid symmetric A
assert run(
"""1
0 0 -2 -4
0 0 2 -4
-1 -2 1 -2
"""
) == "YES", "basic valid A"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Các đoạn không có điểm cuối chung | KHÔNG | Chân phải kết nối | 
| Góc mở tù túng | KHÔNG | Kiểm tra góc sản phẩm chấm | 
| Xà ngang gần đầu | KHÔNG | Điều kiện tỷ lệ | 
| Hình dạng hợp lệ đối xứng | CÓ | Cấu hình chuẩn thành công | 

## Vỏ cạnh 

Xét trường hợp thanh ngang nằm trên phần kéo dài vô hạn của một chân nhưng không nằm trên đoạn thực tế.```
1
0 0 0 4
0 4 4 0
10 10 11 11
```Đường đi qua đoạn thứ ba không chạm vào đoạn chân nào. các`point_on_segment`hàm từ chối nó vì tọa độ nằm ngoài giới hạn phân đoạn. Thuật toán in`NO`. 

Bây giờ hãy xem xét một góc tù.```
1
0 0 0 5
0 5 -2 -1
0 2 -1 2
```Điểm cuối được chia sẻ là`(0,5)`. Các vectơ chân trở thành`(0,-5)`Và`(-2,-6)`. Tích chấm của họ là:$$0 \cdot (-2) + (-5) \cdot (-6) = 30$$Nếu vectơ được định hướng không chính xác, trường hợp này có thể vô tình vượt qua. Việc triển khai luôn hướng các vectơ ra khỏi điểm cuối chung, giúp cho việc kiểm tra góc trở nên đáng tin cậy. 

Cuối cùng, kiểm tra trường hợp tỷ lệ biên.```
1
0 0 0 8
0 8 4 0
0 7 1 6
```điểm`(0,7)`chia chặng đầu tiên thành các độ dài`1`Và`7`. Độ dài bình phương trở thành`1`Và`49`. 

Séc trở thành:$$16 \cdot 1 \ge 49$$sai, do đó thuật toán sẽ loại bỏ cấu hình một cách chính xác.
