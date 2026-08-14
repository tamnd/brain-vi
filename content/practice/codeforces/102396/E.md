---
title: "CF 102396E - Giải pháp độc đáo"
description: "Chúng ta được cấp một vectơ mục tiêu (a) có độ dài (n), trong đó mọi tọa độ là (-1), (0) hoặc (1) và ít nhất một tọa độ khác 0."
date: "2026-08-14T14:29:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102396
codeforces_index: "E"
codeforces_contest_name: "2019-2020 Saint-Petersburg Open High School Programming Contest (SpbKOSHP 19)"
rating: 0
weight: 102396
solve_time_s: 412
verified: false
draft: false
---

[CF 102396E - Giải pháp độc đáo](https://codeforces.com/problemset/problem/102396/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 6 phút 52 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một vectơ mục tiêu (a) có độ dài (n), trong đó mọi tọa độ là (-1), (0) hoặc (1) và ít nhất một tọa độ khác 0. Chúng ta phải xây dựng một vectơ số nguyên khác (x) và một mô đun (m) sao cho, trong số tất cả các vectơ khác 0 (b\in{-1,0,1}^n), sự đồng dư 

[ 
\sum_{i=1}^{n} b_i x_i \equiv 0 \pmod m 
] 

đúng với hai vectơ, đó là (b=a) và (b=-a). 

Khó khăn là chúng ta không được yêu cầu tìm nghiệm cho (x) và (m) cố định. Chúng ta đang thiết kế (x) và (m) sao cho một vectơ quy định trở thành giải pháp duy nhất cho việc thay đổi mọi dấu. 

Giới hạn (n\le 30) là ràng buộc số quan trọng. Nó cho phép chúng ta sử dụng các số có lũy thừa từ hai đến (2^{29}), vẫn nằm trong phạm vi cho phép đối với mọi (x_i). Đồng thời, mô đun phải nhỏ hơn (2^n), vì vậy chúng ta không thể đơn giản sử dụng mô đun lớn bằng tích của các cách xây dựng độc lập tùy ý. Công trình bên dưới nhận được chính xác số lượng phòng cần thiết bằng cách chia tọa độ thành hai hệ thống mô-đun độc lập. 

Có hai trường hợp đáng được quan tâm đặc biệt. Nếu (a) có chính xác một tọa độ khác 0, giả sử đầu vào là```
3
0 -1 0
```chúng ta không thể sử dụng cấu trúc tổng quát với (2^k-1), bởi vì (k=1) sẽ cho mô đun (1) và mọi số nguyên đều chia hết cho (1). Thay vào đó, chúng ta sử dụng (m=2^n-1), đặt (x_2=-m) và cấp cho các tọa độ khác lũy thừa (1,2). Tổng có dấu của họ có giá trị tuyệt đối nhỏ hơn (m) nên họ không thể tạo ra nghiệm khác 0. 

Trường hợp cạnh thứ hai là khi không có tọa độ bằng 0. Ví dụ,```
3
1 -1 1
```công trình vẫn hoạt động. Phần tọa độ 0 của công trình đơn giản biến mất, do đó hệ số mô đun tương ứng của nó là (1). Sức mạnh của hai modulo (2^n-1) sau đó buộc vectơ mục tiêu và giá trị âm của nó. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp sẽ liệt kê mọi câu trả lời có thể có của học sinh (b). Mỗi tọa độ có ba lựa chọn, do đó có (3^n-1) ứng cử viên khác 0. Đối với mỗi ứng cử viên, chúng tôi có thể tính tích vô hướng và kiểm tra tính chia hết. Điều này đúng vì nó kiểm tra mọi câu trả lời có thể theo đúng nghĩa đen, nhưng với (n=30) nó yêu cầu khoảng 

[ 
3^{30}\khoảng 2,06\cdot 10^{14} 
] 

các vectơ ứng cử viên, vượt xa giới hạn thời gian. 

Quan sát hữu ích là chúng ta kiểm soát cả (x) và (m). Chúng ta có thể hiển thị các nhóm tọa độ khác nhau thông qua các yếu tố khác nhau của mô đun. Giả sử tọa độ (k) của (a) khác 0 và tọa độ (z=n-k) bằng 0. Với (k\ge2), chọn 

[ 
M_1=2^k-1,\qquad M_2=2^z, 
] 

và sử dụng 

[ 
m=M_1M_2. 
] 

Vì (M_1<2^k) nên ta có 

[ 
m=(2^k-1)2^z<2^{k+z}=2^n. 
] 

Các tọa độ khác 0 sẽ là bội số của (M_2) nên chúng biến mất theo modulo (M_2). Tọa độ 0 sẽ là bội số của (M_1) nên chúng biến mất theo modulo (M_1). Điều kiện chia hết đơn modulo (m=M_1M_2) sau đó chia thành hai điều kiện độc lập. 

Đối với tọa độ khác 0, lũy thừa của hai mang lại tính chất duy nhất quan trọng. Nếu tọa độ (k) khác 0 được gán lũy thừa (1,2,\ldots,2^{k-1}), thì 

[ 
1+2+\cdots+2^{k-1}=2^k-1=M_1. 
] 

Do đó vectơ mong muốn tạo ra chính xác (M_1). Bất kỳ vectơ nào khác tạo ra bội số của (M_1) thực tế phải tạo ra (0), (M_1) hoặc (-M_1), vì giá trị tuyệt đối tối đa là (M_1). Tính duy nhất thông thường của biểu diễn nhị phân khi đó buộc vectơ hệ số là (0), tất cả (1) hoặc tất cả (-1). 

Đối với tọa độ 0, lũy thừa của hai modulo (2^z) có một thuộc tính hữu ích khác. Tổng có dấu tuyệt đối của họ nhiều nhất là 

[ 
1+2+\cdots+2^{z-1}=2^z-1, 
] 

nhỏ hơn (2^z). Do đó, tổng có dấu chỉ có thể chia hết cho (2^z) khi nó chính xác bằng 0. Tính duy nhất nhị phân sau đó buộc mọi hệ số tọa độ bằng 0 bằng 0. 

Cách tiếp cận bạo lực hoạt động vì tập hợp các vectơ có thể có là hữu hạn và rõ ràng, nhưng không thành công vì tập hợp đó có kích thước (3^n). Quan sát rằng mô đun có thể được phân tích thành thừa số cho phép chúng ta thay thế một tìm kiếm lớn bằng hai đối số duy nhất dựa trên biểu diễn nhị phân. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(3^n n)) | (O(n)) | Quá chậm | 
| Tối ưu | (O(n)) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm số (k) phần tử khác 0 của (a) và đặt (z=n-k). Chúng ta sẽ sử dụng tọa độ khác 0 để mã hóa các dấu hiệu quy định và tọa độ 0 để tạo ra một ràng buộc độc lập ngăn chúng thay đổi. 
2. Nếu (k=1) và (n=1), xuất ra (m=1) và (x_1=1). Các lựa chọn khác 0 duy nhất là (1) và (-1), vì vậy cả hai và chỉ những lựa chọn đó đều thỏa mãn điều kiện chia hết. 
3. Nếu (k=1) và (n>1), hãy chọn (m=2^n-1). Đặt (x_j=a_jm) ở tọa độ khác 0 duy nhất (j). Gán lũy thừa (1,2,\ldots,2^{n-2}) cho các tọa độ còn lại theo bất kỳ thứ tự nào. Tọa độ mong muốn đóng góp (\pm m), trong khi mọi kết hợp có dấu của các tọa độ khác có giá trị tuyệt đối nhiều nhất là (2^{n-1}-1<m). Do đó, tất cả các tọa độ khác đều phải bằng 0 trong bất kỳ nghiệm nào và tọa độ khác 0 duy nhất phải là (1) hoặc (-1). 
4. Với (k\ge2), xác định (M_1=2^k-1) và (M_2=2^z). Đặt (m=M_1M_2). Bất đẳng thức (m<2^n) suy ra trực tiếp từ (M_1<2^k). 
5. Liệt kê các vị trí khác 0 của (a). Nếu vị trí (r)-th như vậy chứa (a_i), hãy đặt 

[ 
x_i=a_iM_2 2^r. 
] 

Phép nhân với (M_2) làm cho tọa độ này trở nên vô hình modulo (M_2), trong khi modulo (M_1) nó hoạt động chính xác như trọng số nhị phân có dấu (a_i2^r). 

1. Liệt kê các vị trí 0 của (a). Đối với (các) vị trí như vậy, hãy đặt 

[ 
x_i=M_1 2^s. 
]

Các tọa độ này là modulo vô hình (M_1), trong khi modulo (M_2) chúng tạo thành các trọng số nhị phân thông thường (1,2,\ldots,2^{z-1}). 

1. Xét bất kỳ vectơ ứng viên nào (b). Vì (M_1) và (M_2) là nguyên tố cùng nhau nên khả năng chia hết cho tích của chúng tương đương với khả năng chia hết cho từng thừa số. Modulo (M_1), mọi đóng góp có tọa độ 0 đều biến mất, chỉ còn lại tọa độ khác 0. Modulo (M_2), mọi đóng góp có tọa độ khác 0 đều biến mất, chỉ còn lại tọa độ ở đó (a_i=0). 
2. Điều kiện modulo (M_2) buộc mọi mục nhập tọa độ 0 của (b) bằng 0. Điều kiện modulo (M_1) còn lại nói rằng các hệ số trên tọa độ khác 0 phải bằng 0, tất cả đều bằng các phần tử tương ứng của (a) hoặc tất cả đều bằng âm của chúng. Trường hợp hoàn toàn bằng 0 bị bài toán loại trừ, chỉ để lại chính xác (a) và (-a). 

### Tại sao nó hoạt động 

Bất biến trung tâm là hai nhóm tọa độ được phân tách bằng hệ số mô đun nguyên tố cùng nhau. Tọa độ thuộc hỗ trợ của (a) là bội số của (M_2) nên không ảnh hưởng đến điều kiện (M_2). Tọa độ bên ngoài hỗ trợ là bội số của (M_1) nên không thể ảnh hưởng đến điều kiện (M_1). Do đó, tọa độ 0 buộc phải giữ nguyên bằng 0, trong khi tọa độ khác 0 được giảm xuống biểu diễn nhị phân có dấu của (0), (M_1) hoặc (-M_1). Tính duy nhất nhị phân cho chính xác ba vectơ (0,a,-a) và vectơ 0 không phải là câu trả lời được phép của học sinh. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    nonzero = [i for i in range(n) if a[i] != 0]
    zero = [i for i in range(n) if a[i] == 0]

    k = len(nonzero)
    z = len(zero)

    # Special case: n = 1, necessarily k = 1.
    if n == 1:
        print(1)
        print(a[0])
        return

    # Special case: exactly one nonzero coordinate.
    if k == 1:
        m = (1 << n) - 1
        x = [0] * n

        j = nonzero[0]
        x[j] = a[j] * m

        value = 1
        for i in zero:
            x[i] = value
            value <<= 1

        print(m)
        print(*x)
        return

    # General case.
    m1 = (1 << k) - 1
    m2 = 1 << z
    m = m1 * m2

    x = [0] * n

    # Encode nonzero coordinates modulo m1.
    weight = 1
    for i in nonzero:
        x[i] = a[i] * m2 * weight
        weight <<= 1

    # Encode zero coordinates modulo m2.
    weight = 1
    for i in zero:
        x[i] = m1 * weight
        weight <<= 1

    print(m)
    print(*x)

if __name__ == "__main__":
    solve()
```Đầu vào trước tiên được phân chia thành độ hỗ trợ của (a) và phần bù của nó. Giữ các danh sách chỉ mục này là đủ để gán các trọng số nhị phân liên tiếp mà không thay đổi thứ tự ban đầu của mảng. 

Nhánh (k=1) là cần thiết vì cấu trúc chính sẽ sử dụng (M_1=2^1-1=1), không đưa ra hạn chế mô-đun hữu ích nào. Với (n=1), mô đun (1) thực sự là đủ vì các vectơ khác 0 duy nhất là (1) và (-1). 

Đối với trường hợp tổng quát,`m1`là (2^k-1) và`m2`là (2^z). Tích của họ hoàn toàn nhỏ hơn (2^n), thỏa mãn chính xác giới hạn mô đun. 

Các giá trị được gán cho tọa độ khác 0 được nhân với`m2`. Các giá trị được gán cho tọa độ 0 được nhân với`m1`. Đây là sự phân tách kiểu CRT được sử dụng trong chứng minh. 

Giá trị lớn nhất được gán cho tọa độ khác 0 nhiều nhất là 

[ 
2^z 2^{k-1}=2^{n-1}, 
] 

và giá trị tọa độ 0 lớn nhất nhỏ hơn 

[ 
(2^k-1)2^{z-1<2^{n-1}. 
] 

Do đó mọi (x_i) đều nằm trong khoảng (-2^{30}) và (2^{30}) khi (n\le30). Số nguyên Python không tràn, nhưng cấu trúc cũng nằm trong giới hạn số ban đầu thay vì dựa vào độ chính xác tùy ý của Python. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Xem xét đầu vào được cung cấp```
2
1 -1
```Cả hai tọa độ đều khác 0, vì vậy (k=2) và (z=0). Việc xây dựng mang lại 

[ 
M_1=2^2-1=3,\qquad M_2=1,\qquad m=3. 
] 

Các giá trị được gán là (1) và (-2). 

| Tọa độ | (a_i) | Trọng lượng nhị phân | (x_i) | 
| --- | --- | --- | --- | 
| 1 | 1 | (1) | (1) | 
| 2 | -1 | (2) | (-2) | 

Đối với ứng viên (b), tổng là (b_1-2b_2). Các giá trị có thể có của nó có giá trị tuyệt đối nhiều nhất là (3). Bội số của (3) trong phạm vi này là (-3,0,3). 

| (b_1) | (b_2) | Tổng hợp | Chia hết cho 3? | 
| --- | --- | --- | --- | 
| 1 | -1 | 3 | Có | 
| -1 | 1 | -3 | Có | 
| 0 | 0 | 0 | Bị loại trừ | 
| sự lựa chọn khác | | không (\pm3) hoặc (0) | Không | 

Hai vectơ khác 0 hợp lệ chính xác là ((1,-1)) và ((-1,1)). 

### Ví dụ 2 

Hãy xem xét```
4
1 0 -1 0
```Ở đây (k=2) và (z=2). Như vậy 

[ 
M_1=3,\qquad M_2=4,\qquad m=12. 
] 

Hai tọa độ khác 0 nhận trọng số (1) và (2), nhân với (M_2). Hai tọa độ 0 nhận (1) và (2), nhân với (M_1). 

| Tọa độ | (a_i) | Vai trò | (x_i) | 
| --- | --- | --- | --- | 
| 1 | 1 | khác không, trọng lượng (1) | (4) | 
| 2 | 0 | số không, trọng lượng (1) | (3) | 
| 3 | -1 | khác không, trọng lượng (2) | (-8) | 
| 4 | 0 | số không, trọng lượng (2) | (6) | 

Đối với vectơ mục tiêu, 

[ 
1\cdot4+0\cdot3+(-1)(-8)+0\cdot6=12, 
] 

vì vậy nó hợp lệ. Âm của nó tạo ra (-12), điều này cũng hợp lệ. 

Bây giờ hãy kiểm tra hai thành phần mô-đun một cách riêng biệt. Modulo (4), tọa độ khác 0 biến mất và tọa độ 0 góp phần 

[ 
3(b_2+2b_4). 
] 

Vì (3) là modulo khả nghịch (4), nên điều này đòi hỏi 

[ 
b_2+2b_4\equiv0\pmod4. 
] 

Giá trị tuyệt đối của nó nhiều nhất là (3), vì vậy nó thực sự phải bằng 0. Lựa chọn bậc ba duy nhất thỏa mãn điều này là (b_2=b_4=0). 

Modulo (3), chỉ còn lại tọa độ (1) và (3). Tình trạng của họ trở nên 

[ 
b_1-2b_3\equiv0\pmod3. 
] 

Các khả năng duy nhất là ((b_1,b_3)=(1,-1),(-1,1),(0,0)). Vectơ 0 bị loại trừ, để lại chính xác vectơ quy định và giá trị âm của nó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n)) | Mỗi tọa độ được phân loại và gán chính xác một lần | 
| Không gian | (O(n)) | Mảng giá trị đầu vào, chỉ số và giá trị đầu ra chứa (O(n)) số nguyên | 

Việc xây dựng chỉ thực hiện một lượng số học không đổi trên mỗi tọa độ. Với (n\le30), nó thấp hơn nhiều so với giới hạn thời gian 1 giây và sử dụng bộ nhớ không đáng kể. 

## Trường hợp thử nghiệm 

Vì đầu ra không phải là duy nhất nên bộ khai thác kiểm tra không được so sánh văn bản đầu ra được tạo với một câu trả lời cố định. Thay vào đó, nó nên phân tích cú pháp (m) và (x), liệt kê tất cả (3^n) vectơ ứng cử viên cho các kiểm tra nhỏ và xác minh rằng các ứng cử viên khác 0 duy nhất chia hết cho (m) là (a) và (-a). Đối với trường hợp kích thước tối đa, khai thác sẽ kiểm tra giới hạn số và mối quan hệ đích mà không liệt kê tất cả các ứng cử viên.```python
import sys
import io
from itertools import product

def solve_data(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        n = int(sys.stdin.readline())
        a = list(map(int, sys.stdin.readline().split()))

        nonzero = [i for i in range(n) if a[i] != 0]
        zero = [i for i in range(n) if a[i] == 0]

        k = len(nonzero)
        z = len(zero)

        if n == 1:
            print(1)
            print(a[0])
            return sys.stdout.getvalue()

        if k == 1:
            m = (1 << n) - 1
            x = [0] * n

            j = nonzero[0]
            x[j] = a[j] * m

            weight = 1
            for i in zero:
                x[i] = weight
                weight <<= 1

            print(m)
            print(*x)
            return sys.stdout.getvalue()

        m1 = (1 << k) - 1
        m2 = 1 << z
        m = m1 * m2

        x = [0] * n

        weight = 1
        for i in nonzero:
            x[i] = a[i] * m2 * weight
            weight <<= 1

        weight = 1
        for i in zero:
            x[i] = m1 * weight
            weight <<= 1

        print(m)
        print(*x)
        return sys.stdout.getvalue()

    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

def validate(inp: str):
    out = solve_data(inp)
    data = list(map(int, out.split()))

    lines = inp.split()
    n = int(lines[0])
    a = list(map(int, lines[1:n + 1]))

    m = data[0]
    x = data[1:1 + n]

    assert 1 <= m < (1 << n)
    assert len(x) == n
    assert all(-(1 << 30) < v < (1 << 30) for v in x)

    target = sum(ai * xi for ai, xi in zip(a, x))
    assert target % m == 0

    expected = {tuple(a), tuple(-v for v in a)}

    if n <= 10:
        solutions = set()

        for b in product((-1, 0, 1), repeat=n):
            if not any(b):
                continue

            value = sum(bi * xi for bi, xi in zip(b, x))
            if value % m == 0:
                solutions.add(b)

        assert solutions == expected

# Provided sample
validate(
    """2
1 -1
"""
)

# Minimum-size case
validate(
    """1
-1
"""
)

# Exactly one nonzero coordinate
validate(
    """4
0 1 0 0
"""
)

# Mixed zero and nonzero coordinates
validate(
    """4
1 0 -1 0
"""
)

# All coordinates nonzero
validate(
    """5
1 -1 1 -1 1
"""
)

# Maximum-size input, all coordinates nonzero
validate(
    "30\n" + " ".join(["1", "-1"] * 15) + "\n"
)

print("all tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / -1`| Bất kỳ (m,x) hợp lệ nào do công trình tạo ra | Kích thước tối thiểu | 
|`4 / 0 1 0 0`| Một công trình có chính xác hai vectơ khác 0 | Trường hợp đặc biệt (k=1) | 
|`4 / 1 0 -1 0`| Một công trình sử dụng cả hai thành phần mô đun | Tách CRT | 
|`5 / 1 -1 1 -1 1`| Một công trình không có tọa độ bằng 0 | Xây dựng tổng quát với (z=0) | 
|`30 / alternating ±1`| Bất kỳ đầu ra nào thỏa mãn giới hạn số | Số học tối đa (n) và biên | 

## Vỏ cạnh 

Đối với trường hợp tọa độ đơn, lấy```
1
-1
```Kết quả đầu ra của thuật toán (m=1) và (x_1=-1). Mọi số nguyên đều chia hết cho (1), nhưng các lựa chọn khác 0 duy nhất được phép cho hệ số đơn là (-1) và (1). Lựa chọn 0 bị cấm, do đó tập nghiệm cần thiết chính xác là hai vectơ (a) và (-a). 

Để có chính xác một tọa độ khác 0 với nhiều vị trí hơn, hãy xem xét```
4
0 1 0 0
```Thuật toán sử dụng (m=15), đưa ra tọa độ thứ hai (x_2=15) và đưa ra các tọa độ khác (1,2,4). Bất kỳ tổng có dấu nào chỉ sử dụng ba trọng số nhỏ hơn đó đều có giá trị tuyệt đối nhiều nhất là (7), vì vậy nó không thể là bội số khác 0 của (15). Do đó tất cả tọa độ bằng 0 phải nhận hệ số bằng 0. Hệ số thứ hai có thể là (1) hoặc (-1), cho ra chính xác hai vectơ cần tìm. 

Đối với đầu vào chứa cả tọa độ 0 và khác 0, chẳng hạn như```
4
1 0 -1 0
```công trình sử dụng (M_1=3), (M_2=4) và (m=12). Tọa độ tương ứng với các số 0 là bội số của (3) nên điều kiện modulo (3) không thể nhìn thấy chúng. Các tọa độ khác 0 là bội số của (4), do đó điều kiện modulo (4) không thể nhìn thấy chúng. Phương trình modulo (4) buộc cả hai vị trí 0 phải có hệ số 0, sau đó phương trình modulo (3) chỉ có mục tiêu và giá trị âm của nó là nghiệm khác không. 

Khi mọi tọa độ đều khác 0, chẳng hạn như```
5
1 -1 1 -1 1
```chúng ta có (z=0), vì vậy (M_2=1). Việc xây dựng giảm xuống lũy ​​thừa có dấu của hai modulo (M_1=31). Mục tiêu tạo ra 

[ 
1+2+4+8+16=31. 
] 

Bất kỳ ứng cử viên nào có giá trị tuyệt đối nhiều nhất (31). Do đó, giá trị chia hết là (0), (31) hoặc (-31). Tính duy nhất nhị phân buộc mẫu hệ số tương ứng phải hoàn toàn bằng 0, tất cả (1) hoặc tất cả (-1) sau khi tính đến các dấu được lưu trong (x_i). Do đó, nghiệm khác 0 duy nhất là (a) và (-a). 

Giá trị lớn nhất xảy ra khi (n=30). Trong cấu trúc chung, tọa độ khác 0 có độ lớn tối đa là (2^{n-1}), trong khi tọa độ 0 nhỏ hơn (2^{n-1}). Cả hai đều hoàn toàn dưới (2^{30}), vì vậy giới hạn đầu ra vẫn hợp lệ ngay cả ở kích thước đầu vào tối đa.:::
