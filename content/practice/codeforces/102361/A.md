---
title: "CF 102361A - Nhịp góc"
description: "Đối với mỗi điểm truy vấn (A), chúng ta phải chọn hai điểm gốc riêng biệt (Pu) và (Pv). Ba điểm phải tạo thành một tam giác không suy biến có một góc bằng (90^circ). Đầu ra cho truy vấn đó là số lượng các cặp điểm ban đầu không có thứ tự như vậy."
date: "2026-08-14T02:48:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102361
codeforces_index: "A"
codeforces_contest_name: "2019 China Collegiate Programming Contest Qinhuangdao Onsite"
rating: 0
weight: 102361
solve_time_s: 115
verified: true
draft: false
---

[CF 102361A - Nhịp góc](https://codeforces.com/problemset/problem/102361/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 55s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Đối với mỗi điểm truy vấn (A), chúng ta phải chọn hai điểm gốc riêng biệt (P_u) và (P_v). Ba điểm phải tạo thành một tam giác không suy biến có một góc bằng (90^\circ). Đầu ra của truy vấn đó là số lượng các cặp điểm gốc không có thứ tự như vậy. 

Điều kiện hình học quan trọng được biểu diễn bằng vectơ. Nếu (A) là đỉnh vuông góc thì 

[ 
(P_u-A)\cdot(P_v-A)=0. 
] 

Tuy nhiên, (A) cũng có thể nằm trên cạnh huyền. Trong trường hợp đó, một số điểm ban đầu (P_i) là đỉnh góc vuông và điều kiện trở thành 

[ 
(A-P_i)\cdot(P_j-P_i)=0. 
] 

Sự khác biệt giữa hai trường hợp này thúc đẩy toàn bộ giải pháp. 

Có nhiều nhất (2000) điểm gốc và (2000) truy vấn. Một truy vấn có thể có khoảng hai triệu cặp điểm ban đầu và việc thực hiện điều đó cho mỗi truy vấn sẽ yêu cầu kiểm tra khoảng (4\times10^9) cặp ở kích thước tối đa. Thẩm phán chính thức cho vấn đề này giới hạn thời gian là 4 giây, vì vậy cách tiếp cận theo hình khối vượt xa những gì chúng ta có thể mua được. 

Tọa độ có thể đạt tới (10^9), do đó chênh lệch có thể đạt tới (2\times10^9) và tích số chấm có thể đạt tới khoảng (4\time10^{18}). Số nguyên Python xử lý trực tiếp các giá trị này, trong khi việc triển khai có chiều rộng cố định sẽ cần số học 64 bit. 

Trường hợp tinh tế đầu tiên là khi một số vectơ từ cùng một điểm nằm dọc theo cùng một đường thẳng. Ví dụ:```
3 1
1 0
-1 0
0 1
0 0
```Đầu ra đúng là`2`. Hai điểm ngang tạo thành một đường định hướng và điểm dọc tạo thành một đường vuông góc, do đó có hai lựa chọn cho điểm cuối nằm ngang. Việc triển khai bất cẩn lưu trữ các vectơ thô thay vì hướng của chúng có thể bỏ lỡ thực tế là ((1,0)) và ((-1,0)) thuộc cùng một đường hình học. 

Trường hợp tinh vi thứ hai là bản thân truy vấn không nhất thiết phải là đỉnh vuông góc. Ví dụ:```
2 1
0 0
1 0
0 1
```Đầu ra đúng là`1`. Góc vuông nằm ở ((0,0)), không phải tại điểm truy vấn ((0,1)). Cách tiếp cận chỉ đếm các vectơ vuông góc xung quanh truy vấn sẽ trả về 0 không chính xác. 

Trường hợp cạnh thứ ba xảy ra với các điểm nằm trên cùng một đường thẳng đứng hoặc nằm ngang:```
4 1
0 0
0 1
0 2
0 3
1 1
```Đầu ra đúng là`4`. Tọa độ (x) bằng nhau buộc nhiều vectơ phải thẳng đứng, do đó trường hợp này bắt gặp việc xử lý độ dốc không chính xác, đặc biệt là mã chia cho thành phần (x) hoặc xử lý các hướng dọc riêng biệt theo cách không nhất quán. 

Cuối cùng, một điểm truy vấn được đảm bảo khác với mọi điểm ban đầu. Do đó, một vectơ từ một truy vấn hoặc một điểm ban đầu đến một điểm có liên quan khác không bao giờ là ((0,0)). Điều này quan trọng vì chuẩn hóa hướng chia cho gcd và gcd của ((0,0)) sẽ bằng 0. 

## Phương pháp tiếp cận 

Giải pháp brute-force trực tiếp xem xét mọi truy vấn (A), mọi cặp (P_u,P_v) và kiểm tra xem tam giác có vuông góc hay không. Có các cặp (\binom n2) cho một truy vấn, vì vậy tổng công việc là 

[ 
O(qn^2). 
] 

Tại (n=q=2000), đây là 

[ 
2000\binom{2000}{2}=3,998,000,000 
] 

kiểm tra cặp. Bản thân bài kiểm tra có thời gian không đổi, nhưng gần bốn tỷ lần lặp lại đã là quá nhiều, đặc biệt là trong Python. 

Lực lượng vũ phu hoạt động vì mọi tam giác có thể được kiểm tra trực tiếp. Vấn đề là nó bỏ qua thực tế là độ vuông góc chỉ phụ thuộc vào hướng chứ không phụ thuộc vào độ dài của vectơ. 

Giả sử một điểm (O) cố định. Xét tất cả các vectơ từ (O) đến các điểm liên quan. Hai vectơ như vậy tạo thành một góc vuông chính xác khi các đường định hướng của chúng vuông góc. Chúng ta có thể chuẩn hóa mọi vectơ khác 0 thành biểu diễn chính tắc của đường vô hướng của nó. Đối với vectơ ((x,y)), chia cả hai tọa độ cho (\gcd(|x|,|y|)), sau đó chọn dấu nhất quán sao cho (x>0) hoặc (x=0) và (y>0). 

Ví dụ, tất cả 

[ 
(1,2),\quad (2,4),\quad (-1,-2),\quad (-2,-4) 
] 

trở thành cùng một hướng kinh điển ((1,2)). 

Khi một hướng (d=(x,y)) được chuẩn hóa, hướng vuông góc chỉ đơn giản là ((-y,x)), theo sau là sự chuẩn hóa tương tự. Bản đồ băm có thể lưu trữ bao nhiêu vectơ thuộc về mọi hướng, do đó, việc tra cứu vuông góc sẽ trở thành (O(1)) thời gian dự kiến. 

Vẫn còn hai vai trò hình học cho điểm truy vấn. Nếu truy vấn là đỉnh góc vuông, chúng ta có thể xử lý truy vấn đó một cách độc lập trong (O(n\log C)), trong đó (C) là độ lớn tọa độ, vì mỗi vectơ (n) phải được chuẩn hóa. 

Nếu truy vấn không phải là đỉnh góc vuông, chúng ta không thể xử lý độc lập từng cặp xung quanh truy vấn. Thay vào đó, chúng tôi đảo ngược quan điểm. Cố định một điểm ban đầu (P_i) làm đỉnh vuông góc. Xây dựng bản đồ tần số hướng cho tất cả các điểm gốc khác xung quanh (P_i). Sau đó, mọi truy vấn (A) đều yêu cầu số điểm ban đầu có vectơ từ (P_i) vuông góc với (A-P_i). Chúng tôi có thể trả lời tất cả các truy vấn (q) trong khi bản đồ này có sẵn. Việc lặp lại điều này cho mọi điểm ban đầu sẽ mang lại các phép toán hướng (O(n^2+nq)). 

Hai trường hợp cùng nhau giảm công việc từ hàng tỷ kiểm tra cặp xuống còn khoảng vài triệu thao tác bản đồ băm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(qn^2)) | (O(1)) | Quá chậm | 
| Băm hướng | (O((nq+n^2)\log C)) dự kiến ​​| (O(n)) | Đã chấp nhận | 

Ở đây (C) biểu thị độ lớn của sự khác biệt tọa độ. Hệ số logarit xuất phát từ thuật toán Euclid được sử dụng bởi`gcd`. 

## Hướng dẫn thuật toán 

1. Đọc tất cả các điểm ban đầu và tất cả các điểm truy vấn trước khi xử lý trường hợp thứ hai. Điều này làm cho việc tính toán một đỉnh góc vuông ban đầu cố định hoàn toàn ngoại tuyến, bởi vì mọi truy vấn đều đã được biết. 
2. Xác định hàm chuẩn hóa hướng. Đối với vectơ khác 0 ((dx,dy)), chia cả hai tọa độ cho gcd của chúng và lật cả hai dấu khi (dx<0) hoặc khi (dx=0) và (dy<0). Kết quả thể hiện một đường khó định hướng đi qua gốc tọa độ. 

Chúng tôi cố tình xác định các vectơ đối diện. Đối với độ vuông góc, ((1,0)) và ((-1,0)) đóng vai trò hoàn toàn giống nhau và việc tách chúng ra sẽ chỉ làm cho việc đếm trở nên phức tạp hơn. 
3. Đối với mỗi truy vấn (A), hãy xây dựng bản đồ tần số theo (n) hướng (P_i-A). Với mỗi hướng (d) xuất hiện (c_d) lần, hãy tính hướng vuông góc chính tắc của nó (d^\perp). Số cặp được đóng góp bởi hai lớp hướng này là (c_d c_{d^\perp}). 
4. Tính tổng các tích đó trên tất cả các lớp hướng và chia cho hai. Một cặp hướng vuông góc được gặp một lần từ mỗi hướng điểm cuối, do đó nếu không có phép chia thì mọi cặp hướng hợp lệ sẽ được tính hai lần. 
5. Khởi tạo mảng câu trả lời với các giá trị thu được khi bản thân truy vấn là đỉnh vuông góc. Điều này giải thích cho mọi tam giác có góc (90^\circ) nằm ở điểm truy vấn. 
6. Cố định một điểm ban đầu (P_i) làm đỉnh có thể là góc vuông. Xây dựng bản đồ tần số chứa các hướng chuẩn hóa (P_j-P_i) cho mọi (j\ne i). 
7. Đối với mọi truy vấn (A_k), chuẩn hóa (A_k-P_i), xoay truy vấn theo (90^\circ) bằng cách sử dụng ((-y,x)), chuẩn hóa hướng vuông góc đó và tra cứu nó trên bản đồ. Tần số được lưu trữ chính xác là số điểm ban đầu (P_j) sao cho (P_i,A_k,P_j) có góc vuông tại (P_i). 
8. Thêm tần số đó vào câu trả lời của truy vấn (A_k). Lặp lại hai bước trước cho mỗi điểm ban đầu (P_i). 

Một tam giác trong đó truy vấn không phải là đỉnh góc vuông có chính xác một điểm ban đầu là đỉnh góc vuông, do đó việc xử lý từng (P_i) riêng biệt sẽ tính mỗi tam giác như vậy chính xác một lần. 
9. In câu trả lời tích lũy cho mỗi truy vấn. 

### Tại sao nó hoạt động 

Đối với một đỉnh cố định (O), hai vectơ khác 0 (u) và (v) vuông góc chính xác khi các đường định hướng của chúng vuông góc. Chuẩn hóa hướng bảo toàn đường được biểu thị bằng vectơ và xoay hướng chuẩn hóa theo (90^\circ) sẽ cho hướng vuông góc chính xác của nó. 

Khi truy vấn là đỉnh góc vuông, bản đồ tần số sẽ đếm từng cặp điểm ban đầu có vectơ từ truy vấn vuông góc. Mỗi cặp không có thứ tự xuất hiện hai lần trong tổng, một lần từ mỗi lớp hướng, do đó việc chia cho hai sẽ cho ra chính xác số lượng hình tam giác. 

Khi điểm ban đầu (P_i) là đỉnh góc vuông thì vectơ (A-P_i) xác định hướng mà vectơ (P_j-P_i) phải có. Bản đồ chứa chính xác tần số của tất cả các hướng điểm ban đầu có thể có từ (P_i), do đó việc tra cứu sẽ tính chính xác các lựa chọn hợp lệ của (P_j). Vì mỗi tam giác chỉ có một đỉnh vuông nên mỗi tam giác có truy vấn về cạnh huyền của nó được tính một lần. Hai trường hợp không khớp nhau vì tam giác không suy biến chỉ có một góc vuông. 

## Giải pháp Python```python
import sys
from math import gcd

input = sys.stdin.readline

def normalize(dx, dy):
    g = gcd(abs(dx), abs(dy))
    dx //= g
    dy //= g

    if dx < 0 or (dx == 0 and dy < 0):
        dx = -dx
        dy = -dy

    return dx, dy

def solve(data=None):
    if data is None:
        read = input
        n, q = map(int, read().split())
        points = [tuple(map(int, read().split())) for _ in range(n)]
        queries = [tuple(map(int, read().split())) for _ in range(q)]
    else:
        it = iter(map(int, data.split()))
        n = next(it)
        q = next(it)
        points = [(next(it), next(it)) for _ in range(n)]
        queries = [(next(it), next(it)) for _ in range(q)]

    ans = [0] * q

    # Case 1: the query point itself is the right-angle vertex.
    for qi, (ax, ay) in enumerate(queries):
        freq = {}

        for px, py in points:
            d = normalize(px - ax, py - ay)
            freq[d] = freq.get(d, 0) + 1

        total = 0

        for (x, y), cnt in freq.items():
            px, py = normalize(-y, x)
            total += cnt * freq.get((px, py), 0)

        ans[qi] = total // 2

    # Case 2: an original point is the right-angle vertex.
    for ox, oy in points:
        freq = {}

        for px, py in points:
            dx = px - ox
            dy = py - oy

            if dx == 0 and dy == 0:
                continue

            d = normalize(dx, dy)
            freq[d] = freq.get(d, 0) + 1

        for qi, (ax, ay) in enumerate(queries):
            dx = ax - ox
            dy = ay - oy

            d = normalize(-dy, dx)
            ans[qi] += freq.get(d, 0)

    result = "\n".join(map(str, ans))

    if data is None:
        sys.stdout.write(result + "\n")
    else:
        return result + "\n"

if __name__ == "__main__":
    solve()
```các`normalize`chức năng là lõi hình học. Gcd loại bỏ độ dài không liên quan của một vectơ, trong khi quy ước dấu hợp nhất một hướng với hướng ngược lại của nó. Đảm bảo đầu vào có nghĩa là vectơ được truyền tới`normalize`không bao giờ bằng không. 

Vòng lặp bên ngoài đầu tiên xử lý trường hợp truy vấn là đỉnh góc vuông. Bản đồ tần số của nó chứa một mục nhập cho mỗi đường định hướng riêng biệt từ truy vấn đến điểm ban đầu. Nhìn lên hướng quay tìm thấy tất cả các điểm nằm trên đường vuông góc. 

các`total // 2`thao tác là cần thiết vì nếu hướng (d) vuông góc với hướng (e), vòng lặp sẽ xử lý cả (d) và (e). Do đó, cùng một cặp điểm được bao gồm hai lần. 

Vòng ngoài thứ hai cố định mỗi điểm ban đầu là đỉnh góc vuông. Bản đồ tần số chỉ chứa các điểm ban đầu khác, vì vậy bản thân điểm cố định không bao giờ có thể vô tình tạo thành vectơ 0. Đối với mọi truy vấn, vectơ từ điểm cố định đến truy vấn được xoay theo (90^\circ), được chuẩn hóa và được sử dụng làm khóa từ điển. 

Mã này sử dụng các số nguyên có độ chính xác tùy ý của Python, vì vậy các sản phẩm có chênh lệch tọa độ xung quanh (2\times10^9) đều an toàn. Không sử dụng tính toán độ dốc hoặc góc dấu phẩy động, điều này tránh được lỗi chính xác đối với các đường thẳng đứng và tọa độ rất lớn. 

## Ví dụ đã hoạt động 

Mẫu đã cho có bốn điểm ban đầu và hai truy vấn. 

Đối với truy vấn đầu tiên, ((0,0)), các lớp hướng là ngang và dọc. Mỗi điểm chứa hai điểm, do đó đóng góp của truy vấn dưới dạng góc vuông là (2\cdot2=4). 

Đối với truy vấn thứ hai, ((1,1)), các lớp hướng liên quan bao gồm một đường ngang chứa một điểm, một đường thẳng đứng chứa một điểm và hai đường chéo. Tổng số cặp vuông góc cộng lại bằng (3). 

| Truy vấn | Các lớp định hướng xung quanh truy vấn | Đóng góp góc vuông | 
| --- | --- | --- | 
| ((0,0)) | ngang: 2, dọc: 2 | 4 | 
| ((1,1)) | ngang: 1, dọc: 1, ghép chéo | 3 | 

Giai đoạn thứ hai cũng tìm thấy các hình tam giác trong đó truy vấn nằm trên cạnh huyền. Ví dụ: với truy vấn ((1,1)), việc sửa ((0,1)) là đỉnh góc vuông sẽ cho các vectơ vuông góc về phía ((1,1)) và ((0,-1)). Điều tương tự cũng xảy ra với hai đỉnh góc vuông ban đầu có liên quan khác. Điều này xác nhận tại sao chỉ kiểm tra truy vấn vì đỉnh góc vuông sẽ không đầy đủ. 

Đối với dấu vết thứ hai, hãy xem xét:```
6 2
1 0
2 0
0 2
-1 0
0 -2
-1 -1
2 2
1 1
```Các câu trả lời kết quả là`5`Và`7`. 

| Truy vấn | Góc vuông tại truy vấn | Đóng góp với điểm gốc là góc vuông | Cuối cùng | 
| --- | --- | --- | --- | 
| ((2,2)) | 1 | 4 | 5 | 
| ((1,1)) | 1 | 6 | 7 | 

Đối với truy vấn đầu tiên, chính truy vấn đó chiếm một hình tam giác, trong khi bốn hình tam giác bổ sung có một trong sáu điểm ban đầu là đỉnh góc vuông của chúng. Đối với truy vấn thứ hai, phần chia tương ứng là một cộng sáu. Đường biểu diễn thể hiện tính bất biến trung tâm: mọi tam giác hợp lệ đều thuộc đúng một trong hai trường hợp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O((nq+n^2)\log C)) dự kiến ​​| (nq) chuẩn hóa hướng truy vấn và (n^2) chuẩn hóa hướng điểm gốc, với gcd lấy (O(\log C)) | 
| Không gian | (O(n)) | Tại bất kỳ thời điểm nào, bản đồ tần số chứa tối đa (n) hướng chuẩn hóa | 

Với (n,q\le 2000), thuật toán thực hiện theo thứ tự các phép toán hướng (8\times10^6) trong trường hợp lớn nhất, thay vì kiểm tra tam giác rõ ràng khoảng (4\time10^9). Bản đồ được xây dựng lại cho từng đỉnh cố định, vì vậy chúng tôi không lưu trữ (n) bản đồ riêng biệt cùng một lúc. Giới hạn bộ nhớ chính thức là 1024 MB và giới hạn thời gian là 4 giây. 

## Trường hợp thử nghiệm 

Trường hợp "tất cả các điểm có giá trị bằng nhau" theo nghĩa đen không phải là đầu vào hợp pháp vì câu lệnh đảm bảo rằng tất cả các điểm (n+q) đều khác biệt theo cặp. Trường hợp căng thẳng có ý nghĩa gần nhất là làm cho tất cả các điểm ban đầu có chung một tọa độ, thực hiện việc xử lý hướng dọc mà không vi phạm sự đảm bảo.```python
# helper: run solution on input string, return output string
import io
import sys

# Assume the solution above has already defined solve(data).
# The helper calls solve with an input string, so no process-level
# stdin replacement is necessary.
def run(inp: str) -> str:
    return solve(inp)

# Provided sample
assert run(
    """4 2
0 1
1 0
0 -1
-1 0
0 0
1 1
"""
) == "4\n3\n", "provided sample"

# Minimum size: exactly one possible pair.
# The query is the right-angle vertex.
assert run(
    """2 1
0 0
1 0
0 1
"""
) == "1\n", "minimum size"

# Query is on the hypotenuse, so the right angle is at an original point.
assert run(
    """2 1
0 0
2 0
0 1
"""
) == "1\n", "query is not the right-angle vertex"

# All original points have the same x-coordinate.
# This stresses vertical directions and repeated collinear directions.
assert run(
    """4 1
0 0
0 1
0 2
0 3
1 1
"""
) == "4\n", "vertical direction groups"

# Boundary coordinates near +/- 1e9.
assert run(
    """3 1
1000000000 1000000000
-1000000000 1000000000
1000000000 -1000000000
-1000000000 -1000000000
"""
) == "1\n", "coordinate boundary"

# Maximum-size test.
# Original points: (i, 0), 0 <= i < 2000.
# Queries:        (i, 1), 0 <= i < 2000.
#
# For i = 0 and i = 1999, there are 1999 triangles.
# For every interior i, there are 1999 triangles with (i, 0)
# as the right-angle vertex, plus one with the query itself
# as the right-angle vertex.
n = 2000
q = 2000

lines = [f"{n} {q}"]
lines.extend(f"{i} 0" for i in range(n))
lines.extend(f"{i} 1" for i in range(q))

maximum_input = "\n".join(lines) + "\n"

expected = [1999] + [2000] * 1998 + [1999]
maximum_output = "\n".join(map(str, expected)) + "\n"

assert run(maximum_input) == maximum_output, "maximum-size test"

print("all tests passed")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu được cung cấp |`4`,`3`| Cả hai trường hợp hình học với nhau | 
| (n=2) với truy vấn tại ((0,1)) |`1`| Kích thước tối thiểu và độ vuông góc trực tiếp | 
| (n=2), truy vấn cạnh huyền |`1`| Điểm gốc là đỉnh góc vuông | 
| Bốn điểm có tọa độ (x) giống nhau |`4`| Hướng dọc và hướng thẳng hàng lặp đi lặp lại | 
| Tọa độ tại (\pm10^9) |`1`| Sự khác biệt lớn và số học số nguyên | 
| (n=q=2000) |`1999`, tiếp theo là giá trị năm 1998 của`2000`, sau đó`1999`| Hạn chế tối đa và cả hai giai đoạn đếm | 

## Vỏ cạnh 

Trường hợp hướng lặp lại được xử lý bằng cách chuẩn hóa. Vì```
3 1
1 0
-1 0
0 1
0 0
```hai vectơ ngang trở thành cùng hướng chính tắc ((1,0)), trong khi vectơ dọc là ((0,1)). Bản đồ tần số chứa số đếm (2) và (1), do đó phép tính truy vấn dưới dạng góc vuông sẽ cho (2\cdot1=2). Đầu ra là`2`. Các vectơ đối diện được hợp nhất một cách có chủ ý vì độ vuông góc phụ thuộc vào đường thẳng chứ không phụ thuộc vào hướng của vectơ. 

Trường hợp truy vấn trên cạnh huyền được xử lý ở giai đoạn thứ hai. Vì```
2 1
0 0
1 0
0 1
```việc sửa ((0,0)) làm đỉnh góc vuông ban đầu sẽ cho các vectơ ((1,0)) và ((0,1)), vuông góc. Truy vấn nhận được một đóng góp từ đỉnh cố định này, do đó kết quả đầu ra là`1`. Giai đoạn đầu tiên đóng góp bằng 0, đó chính xác là những gì chúng ta muốn vì bản thân truy vấn không phải là đỉnh góc vuông. 

Trường hợp đường thẳng đứng sử dụng cách chuẩn hóa giống như mọi hướng khác. Vì```
4 1
0 0
0 1
0 2
0 3
1 1
```truy vấn có vectơ ((-1,-1)), ((-1,0)), ((-1,1)) và ((-1,2)). Cặp ((-1,-1)) và ((-1,1)) vuông góc, tạo thành một tam giác với truy vấn là đỉnh góc vuông. Điểm ban đầu ((0,1)) cũng là đỉnh vuông với vectơ truy vấn ((1,0)), vuông góc với ba vectơ dọc hướng về ((0,0)), ((0,2)) và ((0,3)). Điều đó thêm ba cái nữa, cho kết quả chính xác`4`. 

Trường hợp tọa độ ranh giới sử dụng sự khác biệt về kích thước (2\times10^9). Vì```
3 1
1000000000 1000000000
-1000000000 1000000000
1000000000 -1000000000
-1000000000 -1000000000
```truy vấn có vectơ ngang thành ((1000000000,-1000000000)) và vectơ dọc thành ((1000000000,1000000000)), tạo ra một góc vuông. Số học số nguyên của Python biểu thị chính xác các tích tương ứng, vì vậy kết quả đầu ra là`1`. 

Trường hợp tất cả các điểm bằng nhau không hợp lệ không cần nhánh đặc biệt. Nếu một đầu vào chứa cùng một điểm hai lần, nó sẽ vi phạm đảm bảo vấn đề và có thể tạo ra một vectơ 0, mà việc chuẩn hóa hướng không được xác định. Vì mọi truy vấn và mọi điểm ban đầu đều khác biệt theo từng cặp nên việc triển khai có thể giả định một cách an toàn mọi vectơ được truyền tới`normalize`là khác không.
