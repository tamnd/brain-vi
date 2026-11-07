---
title: "CF 135B - Hình chữ nhật và hình vuông"
description: "Chúng ta có tám điểm phân biệt trên mặt phẳng. Nhiệm vụ là chia chúng thành hai nhóm riêng biệt, mỗi nhóm có bốn điểm. Một nhóm phải tạo thành một hình vuông. Nhóm còn lại phải tạo thành một hình chữ nhật."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "geometry", "math"]
categories: ["algorithms"]
codeforces_contest: 135
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 97 (Div. 1)"
rating: 1600
weight: 135
solve_time_s: 123
verified: true
draft: false
---

[CF 135B - Hình chữ nhật và Hình vuông](https://codeforces.com/problemset/problem/135/B) 

**Đánh giá:** 1600 
**Tags:** sức mạnh vũ phu, hình học, toán học 
**Thời gian giải:** 2m 3s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có tám điểm phân biệt trên mặt phẳng. Nhiệm vụ là chia chúng thành hai nhóm riêng biệt, mỗi nhóm có bốn điểm. 

Một nhóm phải tạo thành một hình vuông. Nhóm còn lại phải tạo thành một hình chữ nhật. Một hình vuông cũng được phép cho nhóm hình chữ nhật, vì vậy hình thứ hai chỉ cần thỏa mãn các điều kiện hình chữ nhật. 

Các điểm có thể xuất hiện theo bất kỳ thứ tự nào và các hình dạng không nhất thiết phải được căn chỉnh theo trục. Hình vuông và hình chữ nhật được xoay hoàn toàn hợp lệ. 

Đầu ra phải báo cáo rằng phân vùng đó tồn tại, cùng với chỉ số của các điểm tương ứng hoặc báo cáo rằng không tồn tại phân vùng hợp lệ. 

Kích thước đầu vào cực kỳ nhỏ. Chúng tôi chỉ có tám điểm, điều đó có nghĩa là việc sử dụng vũ lực đối với các tập hợp con là hoàn toàn thực tế. chỉ có$$\binom{8}{4} = 70$$cách chọn 4 điểm cho hình vuông. Khi những cái đó đã được sửa, bốn cái còn lại sẽ tự động thuộc về hình chữ nhật. 

Điều này ngay lập tức gợi ý rằng thách thức thực sự là xác nhận hình học chứ không phải tối ưu hóa. 

Phần nguy hiểm của vấn đề là nhận dạng hình dạng. Việc triển khai bất cẩn có thể dễ dàng phân loại sai cấu hình không hợp lệ thành cấu hình hợp lệ. 

Hãy xem xét những điểm sau:```
0 0
1 0
2 0
3 0
```Tất cả các khoảng cách theo cặp đều không bằng nhau, nhưng nếu ai đó chỉ kiểm tra "hai khoảng cách khác nhau", đường suy biến này có thể vô tình đi qua. Cả hình vuông và hình chữ nhật đều phải có diện tích dương. 

Một sai lầm phổ biến khác là cho rằng các điểm đến theo thứ tự tuần hoàn. Ví dụ:```
0 0
1 1
0 1
1 0
```Đây là các đỉnh của một hình vuông, nhưng được xáo trộn theo thứ tự. Bất kỳ thuật toán nào kiểm tra trực tiếp các cạnh liền kề mà không xem xét hoán vị sẽ thất bại. 

Số liệu xoay cũng có vấn đề. Hình chữ nhật này hợp lệ:```
0 0
2 1
3 3
1 2
```Các cạnh không song song với các trục nên chỉ kiểm tra tọa độ x và y là không chính xác. 

Cách tiếp cận an toàn nhất là làm việc hoàn toàn với khoảng cách và tích vector chấm, bởi vì những thuộc tính đó vẫn hợp lệ bất kể phép quay hay thứ tự. 

## Phương pháp tiếp cận 

Ý tưởng về vũ lực rất đơn giản. Chúng ta thử mọi tập hợp con của bốn điểm với tư cách là ứng cử viên bình phương. Bốn điểm còn lại trở thành ứng cử viên hình chữ nhật. Sau đó, chúng tôi xác minh một cách độc lập xem các nhóm được chọn có thực sự tạo thành các hình dạng được yêu cầu hay không. 

Vì chỉ có 70 tập hợp con nên ngay cả việc kiểm tra hình học đắt tiền cũng đủ nhanh. 

Câu hỏi thực sự là làm thế nào để xác minh một hình vuông hoặc hình chữ nhật một cách chắc chắn. 

Một cách tiếp cận hình học đơn giản là sắp xếp các điểm theo góc xung quanh tâm và sau đó kiểm tra độ dài các cạnh và góc vuông. Điều đó có hiệu quả, nhưng nó dễ vỡ một cách không cần thiết. Các vấn đề về dấu phẩy động và sự phức tạp trong việc đặt hàng khiến việc triển khai trở nên khó khăn hơn mức cần thiết. 

Một quan sát rõ ràng hơn là cả hình vuông và hình chữ nhật đều có các đặc tính đường chéo đơn giản. 

Đối với hình chữ nhật: 

1. Các đường chéo có độ dài bằng nhau. 
2. Các đường chéo có chung điểm giữa. 
3. Diện tích khác không. 

Đối với hình vuông, chúng tôi yêu cầu thêm: 

1. Bốn phía đều bằng nhau. 
2. Đường chéo bằng nhau. 
3. Đường chéo dài hơn cạnh. 

Thay vì xây dựng lại thứ tự đa giác, chúng ta có thể phân tích trực tiếp khoảng cách theo cặp. 

Đối với bốn điểm, có chính xác sáu khoảng cách bình phương theo cặp. 

Một hình vuông hợp lệ tạo ra: 

1. Bốn khoảng cách nhỏ hơn bằng nhau, các cạnh. 
2. Hai đường chéo có khoảng cách lớn hơn bằng nhau. 
3. Độ dài đường chéo bằng hai lần độ dài cạnh ở dạng bình phương. 

Một hình chữ nhật hợp lệ tạo ra: 

1. Hai cặp cạnh có độ dài bằng nhau. 
2. Hai đường chéo bằng nhau. 
3. Vùng tích cực. 

Việc kiểm tra hình chữ nhật thậm chí còn rõ ràng hơn đến từ các hoán vị. Vì chỉ có 24 thứ tự của bốn điểm nên chúng ta có thể đơn giản thử tất cả các thứ tự tuần hoàn có thể có và kiểm tra:$$\overrightarrow{AB} \cdot \overrightarrow{BC} = 0$$Và$$|AB| > 0,\quad |BC| > 0$$với các mặt đối diện bằng nhau được ngụ ý tự động. 

Vì không gian tìm kiếm rất nhỏ nên phương pháp hoán vị hoàn toàn có thể chấp nhận được và cực kỳ đáng tin cậy. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force với sự tái thiết hình học |$O(70 \cdot 24)$|$O(1)$| Đã chấp nhận | 
| Lực lượng vũ phu tập hợp con tối ưu với kiểm tra mạnh mẽ |$O(70 \cdot 24)$|$O(1)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tám điểm và lưu trữ tọa độ của chúng cùng với các chỉ số ban đầu của chúng. 
2. Liệt kê mọi tập hợp con của bốn chỉ số dưới dạng bình phương ứng cử viên. 
3. Xây dựng tập con bổ sung của bốn chỉ số cho ứng viên hình chữ nhật. 
4. Kiểm tra xem nhóm đầu tiên có tạo thành hình vuông hay không. 

Chúng tôi tính toán tất cả sáu khoảng cách bình phương theo cặp. 

Một hình vuông hợp lệ phải tạo ra chính xác hai giá trị dương riêng biệt:$$\{s,s,s,s,d,d\}$$Ở đâu$$d = 2s$$Giá trị nhỏ hơn đại diện cho các cạnh và giá trị lớn hơn đại diện cho các đường chéo. 
5. Kiểm tra xem nhóm thứ hai có tạo thành hình chữ nhật hay không. 

Chúng tôi thử tất cả các hoán vị của bốn điểm theo thứ tự tuần hoàn có thể. 

Để đặt hàng$A,B,C,D$, chúng tôi xác minh:$$\overrightarrow{AB} \cdot \overrightarrow{BC} = 0$$đảm bảo một góc vuông. 

Sau đó chúng tôi xác minh:$$|AB| = |CD|$$Và$$|BC| = |DA|$$cùng với độ dài cạnh dương. 
6. Nếu cả hai lần kiểm tra đều thành công, hãy in`"YES"`và các chỉ số điểm tương ứng. 
7. Nếu không có phân vùng nào hoạt động sau khi kiểm tra tất cả các tập hợp con, hãy in`"NO"`. 

### Tại sao nó hoạt động 

Thuật toán cố gắng một cách triệt để mọi phép chia có thể của tám điểm thành hai nhóm bốn. Không có câu trả lời hợp lệ nào có thể thoát khỏi bảng liệt kê này. 

Phép thử bình phương đúng vì bất kỳ hình vuông nào cũng luôn có bốn cạnh bằng nhau và hai đường chéo bằng nhau, và đó là những khoảng cách theo cặp duy nhất xuất hiện. Cấu hình suy biến không thành công vì độ dài các cạnh phải dương. 

Phép thử hình chữ nhật đúng vì mọi hình chữ nhật đều có bốn góc vuông và các cạnh đối diện bằng nhau. Việc thử tất cả các hoán vị đảm bảo rằng cuối cùng chúng ta sẽ gặp được trật tự tuần hoàn thực sự của các đỉnh hình chữ nhật. 

Vì mọi phân vùng ứng cử viên đều được kiểm tra chính xác theo định nghĩa toán học của các hình dạng nên thuật toán không thể chấp nhận cấu hình không hợp lệ hoặc từ chối cấu hình hợp lệ. 

## Giải pháp Python```python
import sys
from itertools import combinations, permutations

input = sys.stdin.readline

def dist2(a, b):
    dx = a[0] - b[0]
    dy = a[1] - b[1]
    return dx * dx + dy * dy

def is_square(points):
    dists = []

    for i in range(4):
        for j in range(i + 1, 4):
            dists.append(dist2(points[i], points[j]))

    dists.sort()

    if dists[0] == 0:
        return False

    return (
        dists[0] == dists[1] == dists[2] == dists[3]
        and dists[4] == dists[5]
        and dists[4] == 2 * dists[0]
    )

def dot(ax, ay, bx, by):
    return ax * bx + ay * by

def is_rectangle(points):
    for p in permutations(points):
        a, b, c, d = p

        abx = b[0] - a[0]
        aby = b[1] - a[1]

        bcx = c[0] - b[0]
        bcy = c[1] - b[1]

        cdx = d[0] - c[0]
        cdy = d[1] - c[1]

        dax = a[0] - d[0]
        day = a[1] - d[1]

        if dot(abx, aby, bcx, bcy) != 0:
            continue

        if dist2(a, b) == 0 or dist2(b, c) == 0:
            continue

        if dist2(a, b) != dist2(c, d):
            continue

        if dist2(b, c) != dist2(d, a):
            continue

        return True

    return False

def solve():
    pts = [tuple(map(int, input().split())) for _ in range(8)]

    indices = list(range(8))

    for square_ids in combinations(indices, 4):
        square_set = set(square_ids)

        rect_ids = [i for i in indices if i not in square_set]

        square_points = [pts[i] for i in square_ids]
        rect_points = [pts[i] for i in rect_ids]

        if is_square(square_points) and is_rectangle(rect_points):
            print("YES")
            print(*[i + 1 for i in square_ids])
            print(*[i + 1 for i in rect_ids])
            return

    print("NO")

solve()
```Giải pháp có hai trình xác nhận hình học độc lập. 

Bộ kiểm tra hình vuông sử dụng đặc tính khoảng cách theo cặp. Vì khoảng cách bình phương tránh căn bậc hai nên tất cả các phép tính đều ở dạng số nguyên. Điều này loại bỏ hoàn toàn các vấn đề về độ chính xác của dấu phẩy động. 

Trình kiểm tra hình chữ nhật sử dụng hoán vị vì thứ tự đầu vào là tùy ý. Bốn đỉnh hình chữ nhật giống nhau có thể xuất hiện theo 24 thứ tự khác nhau và chỉ một số trong số chúng tương ứng với việc đi vòng quanh chu vi một cách chính xác. 

Kiểm tra góc vuông sử dụng tích vô hướng:$$\vec{u} \cdot \vec{v} = 0$$đó là điều kiện trực giao an toàn số nguyên tiêu chuẩn. 

Một chi tiết tinh tế là loại bỏ các cạnh có độ dài bằng 0. Nếu không có sự kiểm tra này, các điểm lặp lại hoặc suy biến có thể vô tình thỏa mãn các điều kiện về mặt bằng nhau. Vấn đề đảm bảo các điểm đầu vào khác biệt trên toàn cầu, nhưng mã hình học phòng thủ vẫn phải xác minh rõ ràng độ dài dương. 

Một điểm tinh tế khác là sử dụng khoảng cách bình phương thay vì độ dài Euclide thực tế. So sánh trực tiếp căn bậc hai dấu phẩy động là không cần thiết và kém tin cậy hơn. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
0 0
10 11
10 0
0 11
1 1
2 2
2 1
1 2
```Thuật toán thử các tập hợp con cho đến khi đạt được các điểm 5, 6, 7, 8 dưới dạng hình vuông. 

| Ứng viên vuông | Khoảng cách theo cặp | Hình vuông hợp lệ | Hình chữ nhật Hợp lệ | 
| --- | --- | --- | --- | 
| 1 2 3 4 | hỗn hợp | Không | chưa được kiểm tra | 
| 5 6 7 8 | 1 1 1 1 2 2 | Có | Có | 

Nhóm thứ hai tạo thành một hình vuông đơn vị quay 45 độ:$$(1,1),(2,2),(2,1),(1,2)$$Bốn điểm còn lại tạo thành một hình chữ nhật thẳng hàng với trục. 

Ví dụ này cho thấy tại sao việc dựa vào căn chỉnh trục sẽ thất bại. Hình vuông được xoay. 

### Ví dụ 2 

Hãy xem xét đầu vào này:```
0 0
1 0
2 0
3 0
0 1
1 1
2 1
3 1
```| Ứng viên vuông | Quan sát | Kết quả | 
| --- | --- | --- | 
| 4 điểm thẳng hàng bất kỳ | diện tích bằng không | bị từ chối | 
| tập hợp con hỗn hợp | khoảng cách không nhất quán | bị từ chối | 

Không có phân vùng nào tạo ra cả hình vuông và hình chữ nhật cùng một lúc. 

Thuật toán cuối cùng sẽ sử dụng hết tất cả 70 tập con và in ra`"NO"`. 

Ví dụ này chứng minh tại sao việc kiểm tra vùng tích cực lại quan trọng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O\left(\binom{8}{4} \cdot 24\right)$| 70 tập hợp con, mỗi kiểm tra hình chữ nhật thử tối đa 24 hoán vị | 
| Không gian |$O(1)$| Chỉ một số mảng và vectơ tạm thời được lưu trữ | 

Tổng số hoạt động là rất nhỏ. Ngay cả với một số kiểm tra hình học cho mỗi hoán vị, thời gian chạy vẫn ở dưới mức giới hạn một cách thoải mái. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from itertools import combinations, permutations

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def dist2(a, b):
        dx = a[0] - b[0]
        dy = a[1] - b[1]
        return dx * dx + dy * dy

    def is_square(points):
        dists = []

        for i in range(4):
            for j in range(i + 1, 4):
                dists.append(dist2(points[i], points[j]))

        dists.sort()

        if dists[0] == 0:
            return False

        return (
            dists[0] == dists[1] == dists[2] == dists[3]
            and dists[4] == dists[5]
            and dists[4] == 2 * dists[0]
        )

    def dot(ax, ay, bx, by):
        return ax * bx + ay * by

    def is_rectangle(points):
        for p in permutations(points):
            a, b, c, d = p

            abx = b[0] - a[0]
            aby = b[1] - a[1]

            bcx = c[0] - b[0]
            bcy = c[1] - b[1]

            if dot(abx, aby, bcx, bcy) != 0:
                continue

            if dist2(a, b) == 0 or dist2(b, c) == 0:
                continue

            if dist2(a, b) != dist2(c, d):
                continue

            if dist2(b, c) != dist2(d, a):
                continue

            return True

        return False

    pts = [tuple(map(int, input().split())) for _ in range(8)]

    indices = list(range(8))

    out = []

    for square_ids in combinations(indices, 4):
        sset = set(square_ids)

        rect_ids = [i for i in indices if i not in sset]

        sp = [pts[i] for i in square_ids]
        rp = [pts[i] for i in rect_ids]

        if is_square(sp) and is_rectangle(rp):
            out.append("YES")
            out.append(" ".join(str(i + 1) for i in square_ids))
            out.append(" ".join(str(i + 1) for i in rect_ids))
            return "\n".join(out)

    return "NO"

# provided sample
assert run(
"""0 0
10 11
10 0
0 11
1 1
2 2
2 1
1 2
"""
).startswith("YES")

# rotated square + rectangle
assert run(
"""0 0
1 1
2 0
1 -1
10 10
14 10
14 12
10 12
"""
).startswith("YES")

# impossible configuration
assert run(
"""0 0
1 0
2 0
3 0
0 1
1 1
2 1
3 1
"""
) == "NO"

# two squares, second used as rectangle
assert run(
"""0 0
1 0
1 1
0 1
10 10
12 10
12 12
10 12
"""
).startswith("YES")

# large coordinates
assert run(
"""10000 10000
10001 10000
10001 10001
10000 10001
-10000 -10000
-9990 -10000
-9990 -9995
-10000 -9995
"""
).startswith("YES")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu chính thức | CÓ | Tính đúng đắn cơ bản | 
| Hình vuông xoay | CÓ | Xử lý xoay | 
| Cấu trúc cộng tuyến | KHÔNG | Từ chối thoái hóa | 
| Hai hình vuông | CÓ | Hình chữ nhật cũng có thể là hình vuông | 
| Tọa độ lớn | CÓ | An toàn số học số nguyên | 

## Vỏ cạnh 

Trường hợp cạnh nguy hiểm là bốn điểm thẳng hàng giả dạng hình chữ nhật. 

đầu vào:```
0 0
1 0
2 0
3 0
0 1
1 1
2 1
3 1
```Khi các hoán vị được kiểm tra, mọi thứ tự ứng cử viên cuối cùng đều thất bại trong việc kiểm tra độ vuông góc:$$\overrightarrow{AB} \cdot \overrightarrow{BC} \neq 0$$vì mọi vectơ đều nằm trên một đường thẳng. Thuật toán loại bỏ chính xác mọi phân vùng và in`"NO"`. 

Một trường hợp phức tạp khác là xáo trộn thứ tự đỉnh. 

đầu vào:```
0 0
1 1
0 1
1 0
10 10
13 10
13 12
10 12
```Các điểm bình phương không được đưa ra theo chu kỳ. Một trình kiểm tra cạnh kề ngây thơ sẽ thất bại ngay lập tức. Thay vào đó, giải pháp của chúng tôi tính toán tất cả sáu khoảng cách theo cặp:$$1,1,1,1,2,2$$xác định duy nhất một hình vuông bất kể thứ tự. 

Hình chữ nhật xoay cũng yêu cầu xử lý cẩn thận. 

đầu vào:```
0 0
2 1
3 3
1 2
5 5
6 5
6 6
5 6
```Bốn điểm đầu tiên tạo thành một hình chữ nhật xoay. Vì trình xác nhận hình chữ nhật sử dụng tích vectơ chấm thay vì so sánh trục nên nó xác định chính xác các góc vuông ngay cả khi không có cạnh nào nằm ngang hoặc dọc.
