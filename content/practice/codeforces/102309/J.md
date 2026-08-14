---
title: "CF 102309J - Gấu trúc Orz thất nghiệp"
description: "Chúng ta được cho một ma trận nguyên (ntimes n) (A) và các số nguyên dương (b1,ldots,bn). Đối với một vectơ (x), hãy xác định (y=Ax). Tích phân yêu cầu thể tích (n) chiều của tất cả các vectơ (x) có ảnh (y) nằm bên trong hộp thẳng hàng với trục [ 0le yile bi."
date: "2026-08-13T07:06:17+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102309
codeforces_index: "J"
codeforces_contest_name: "The 2019 \u201cOrz Panda\u201d Cup Programming Contest"
rating: 0
weight: 102309
solve_time_s: 377
verified: true
draft: false
---

[CF 102309J - Gấu trúc Orz thất nghiệp](https://codeforces.com/problemset/problem/102309/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 6 phút 17s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một ma trận số nguyên (n\times n) (A) và các số nguyên dương (b_1,\ldots,b_n). Đối với một vectơ (x), hãy xác định (y=Ax). Tích phân yêu cầu thể tích (n) chiều của tất cả các vectơ (x) có ảnh (y) nằm bên trong hộp thẳng hàng với trục 

[ 
0\le y_i\le b_i. 
] 

Câu trả lời bắt buộc là bình phương của thể tích đó. 

Do đó, câu hỏi hình học then chốt không thực sự là về tích phân. Tập hợp giá trị (y) chỉ đơn giản là một hộp hình chữ nhật, trong khi (x) được lấy từ (y) thông qua phép biến đổi tuyến tính (A). Nếu (A) khả nghịch, hộp sẽ biến đổi trở lại thành hình bình hành, có thể tích được xác định bởi định thức của (A). Nếu (A) là số ít thì một số hướng trong không gian (x) hoàn toàn không nhìn thấy được đối với (Ax), do đó tập hợp khả thi sẽ kéo dài vô tận theo hướng đó. 

Khi (A) khả nghịch, 

# \frac{\operatorname{Vol}(y)}{|\det A|} 

\frac{\prod_i b_i}{|\det A|}. 
] 

Do đó, giá trị được yêu cầu là 

[ 
\đóng hộp{ 
\frac{\left(\prod_i b_i\right)^2}{(\det A)^2} 
}. 
] 

Các ràng buộc có (n\le300), do đó thuật toán (O(n^3)) là phù hợp. Một thuật toán bậc ba thực hiện khoảng (300^3=27) triệu phép tính ma trận cơ bản, trong khi bất kỳ phép toán hàm mũ hoặc giai thừa nào đều hoàn toàn không khả thi. Các mục nhập ma trận và giá trị (b_i) lớn bằng (10^9), do đó việc tính định thức như một số nguyên máy thông thường cũng không an toàn. Các số nguyên có độ chính xác tùy ý của Python tránh bị tràn, nhưng việc loại bỏ Gaussian theo mô-đun sẽ sạch hơn vì bản thân kết quả được yêu cầu là modulo một số nguyên tố. 

Có hai trường hợp thất bại riêng biệt mà việc triển khai bất cẩn phải phân biệt. Đầu tiên, ma trận số ít làm cho tích phân trở thành vô hạn. Ví dụ,```
2
1 1
1 1
2 2
```có định thức bằng 0 và kết quả đúng là`Orz`. Tập hợp (x) khả thi chứa hướng không hạn chế, do đó, việc coi định thức chỉ là mẫu số sẽ gợi ý phép chia cho 0 một cách không chính xác. 

Thứ hai, định thức số nguyên khác 0 có thể chia hết cho (M=10^9+7). Ví dụ,```
2
1 2
-500000003 1
1 1
```có yếu tố quyết định 

[ 
1+2\cdot500000003=1000000007=M. 
] 

Tích phân là hữu hạn, nhưng mẫu số rút gọn của nó vẫn chứa (M), vì mọi (b_i\le10^9<M), nên (\prod b_i) không thể chứa thừa số nguyên tố (M). Nghịch đảo mô-đun cần thiết không tồn tại, vì vậy câu trả lời đúng lại là`Orz`. Một giải pháp chỉ kiểm tra xem định thức có khác 0 trên các số nguyên hay không nhưng sau đó gọi một cách mù quáng một nghịch đảo mô đun sẽ tạo ra kết quả không hợp lệ. 

Mẫu với```
2
1 1
1 -1
4 5
```có định thức (-2). Hộp được chuyển đổi có thể tích (4\cdot5=20), do đó, vùng (x) khả thi có thể tích (20/2=10) và hình vuông được yêu cầu là (100). 

## Phương pháp tiếp cận 

Một cách suy nghĩ trực tiếp và thô bạo về định thức là công thức Leibniz của nó, 

[ 
\det A=\sum_{\pi} \operatorname{sgn}(\pi) 
\prod_i A_{i,\pi(i)}. 
] 

Nó kiểm tra một thuật ngữ cho mỗi hoán vị của (n) cột. Điều đó có nghĩa là tích (n!) và các phép toán vô hướng gần đúng (n\cdot n!) nếu các tích được hình thành độc lập. Tại (n=300), thậm chí (300!) lớn hơn về mặt thiên văn so với bất kỳ thứ gì có thể được xử lý trong bốn giây. Tích phân số trực tiếp thậm chí còn ít hữu ích hơn, vì vùng tích phân là một đa giác nhiều chiều tùy ý trong tọa độ ban đầu. 

Quan sát hình học loại bỏ hoàn toàn sự tích phân. Các ràng buộc đã là một hộp ở tọa độ (y=Ax). Đối với bản đồ tuyến tính khả nghịch, âm lượng thay đổi theo đúng hệ số (|\det A|). Do đó, toàn bộ tích phân có thể được biểu diễn bằng cách sử dụng một định thức và tích của (b_i). 

Thách thức còn lại là tính toán định thức đó một cách hiệu quả và ở dạng tương thích với phép chia mô-đun. Việc loại bỏ Gaussian tính toán định thức trong (O(n^3)). Vì mô đun (M=1000000007) là số nguyên tố nên mọi mô đun đầu vào ma trận khác 0 (M) đều có mô đun nghịch đảo. Do đó, chúng ta có thể thực hiện loại bỏ hoàn toàn modulo (M). 

Có một điểm tinh tế về ý nghĩa của modulo định thức bằng 0 (M). Định thức bằng 0 trên các số nguyên chắc chắn có nghĩa là ma trận là số ít, nhưng modulo ngược (M) hơi khác một chút: định thức nguyên có thể khác 0 khi chia hết cho (M). Vụ đó vẫn phải sản xuất`Orz`, vì câu trả lời hợp lý có mẫu số chứa (M). Vì tất cả (b_i<M), tử số ((\prod b_i)^2) không chia hết cho (M), nên không có thừa số (M) nào có thể biến mất trong quá trình rút gọn phân số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Mở rộng yếu tố quyết định Brute Force | (O(n\cdot n!)) | (O(n)) | Quá chậm | 
| Loại bỏ Gaussian mô-đun tối ưu | (O(n^3)) | (O(n^2)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc (A) và (b), và giảm mọi modulo mục nhập ma trận (M=1000000007). Các mục âm được chuyển đổi thành phần dư không âm tương đương của chúng. 
2. Tính toán (\det A\bmod M) bằng phép loại trừ Gaussian. Tại cột (i), tìm kiếm các hàng (i,\ldots,n-1) để tìm trục xoay khác 0. Nếu không tồn tại thì định thức bằng 0 modulo (M), do đó đầu ra`Orz`. 

Nếu định thức bằng 0 modulo (M), thì (A) thực sự là số ít hoặc định thức nguyên khác 0 của nó chia hết cho (M). Cả hai trường hợp đều dẫn đến`Orz`, vì những lý do toán học khác nhau. 
3. Khi tìm thấy trục xoay bên dưới hàng hiện tại, hãy hoán đổi hai hàng. Hoán đổi hàng làm thay đổi dấu của định thức, do đó nhân định thức tích lũy với (-1). 
4. Nhân tích lũy định thức với giá trị trục. Sau đó loại bỏ các mục bên dưới trục đó. Nếu trục xoay là (p) và mục bị loại bỏ là (v), hãy sử dụng 

[ 
f=v p^{-1}\pmod M 
] 

và thay thế hàng bằng 

[ 
R_j\mũi tên trái R_j-fR_i. 
] 

Chỉ các cột ở bên phải trục xoay mới cần được cập nhật vì cột hiện tại trở thành số 0. 

1. Sau khi tất cả các cột đã được xử lý, bộ tích lũy định thức là (\det A\bmod M). Nếu nó bằng 0, xuất ra`Orz`. Ngược lại thì tính 

[ 
B=\prod_i b_i\pmod M. 
] 

1. Giá trị được yêu cầu là 

[ 
\frac{B^2}{(\det A)^2}. 
] 

Vì định thức là khác 0 modulo (M), nên tồn tại nghịch đảo mô đun của nó. Tính toán 

[ 
B^2\cdot(\det A)^{-2}\pmod M. 
] 

của Python`pow(x, M-2, M)`cho nghịch đảo mô đun vì (M) là số nguyên tố. 

### Tại sao nó hoạt động 

Đặt (S={x:0\le (Ax)_i\le b_i\text{ với mọi }i}). Nếu (A) khả nghịch, bản đồ (x\mapsto Axe) ánh xạ phỏng đoán (S) vào hộp (B=[0,b_1]\times\cdots\times[0,b_n]). Thay đổi tuyến tính của biến có tỷ lệ thể tích theo (|\det A|), do đó 

# \frac{\operatorname{Vol}(B)}{|\det A|} 

\frac{\prod_i b_i}{|\det A|}. 
] 

Bình phương loại bỏ dấu của định thức và đưa ra công thức được thuật toán sử dụng. 

Nếu (A) là số ít thì tồn tại một vectơ khác 0 (v) với (Av=0). Bắt đầu từ bất kỳ điểm khả thi nào, di chuyển dọc theo (v) để lại (Ax) không thay đổi, do đó vùng khả thi chứa một đường không giới hạn và có thể tích vô hạn. Do đó, ma trận số ít phải tạo ra`Orz`. 

Cuối cùng, giả sử (\det A\ne0) là số nguyên nhưng (\det A\equiv0\pmod M). Vì (M) là số nguyên tố và mọi (b_i<M) nên tử số ((\prod b_i)^2) không chia hết cho (M). Do đó mẫu số rút gọn vẫn chứa (M), nên nghịch đảo môđun của nó không tồn tại. Giống nhau`Orz`quyết định là đúng đắn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 1000000007

def determinant_mod(a):
    n = len(a)
    det = 1

    for col in range(n):
        pivot = col
        while pivot < n and a[pivot][col] == 0:
            pivot += 1

        if pivot == n:
            return 0

        if pivot != col:
            a[col], a[pivot] = a[pivot], a[col]
            det = (-det) % MOD

        p = a[col][col]
        det = det * p % MOD

        inv_p = pow(p, MOD - 2, MOD)

        pivot_row = a[col]

        for row in range(col + 1, n):
            value = a[row][col]
            if value == 0:
                continue

            factor = value * inv_p % MOD
            current = a[row]

            for j in range(col + 1, n):
                current[j] = (current[j] - factor * pivot_row[j]) % MOD

            current[col] = 0

    return det

def solve():
    out = []

    for line in sys.stdin:
        line = line.strip()
        if not line:
            continue

        n = int(line)

        a = []
        for _ in range(n):
            row = list(map(int, input().split()))
            a.append([x % MOD for x in row])

        b = list(map(int, input().split()))

        det = determinant_mod(a)

        if det == 0:
            out.append("Orz")
            continue

        product_b = 1
        for x in b:
            product_b = product_b * (x % MOD) % MOD

        inv_det = pow(det, MOD - 2, MOD)
        ans = product_b * product_b % MOD
        ans = ans * inv_det % MOD
        ans = ans * inv_det % MOD

        out.append(str(ans))

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Thủ tục xác định sửa đổi ma trận tại chỗ, tránh việc cấp phát một ma trận (n\times n) khác. Mọi mục nhập đều được giữ theo modulo (M), vì vậy tất cả số học vẫn bị giới hạn bởi khoảng (M^2) trước phép toán modulo. 

Việc tìm kiếm trục quay là cần thiết vì việc loại bỏ Gaussian không thể chia cho trục quay bằng 0. Hoán đổi hàng làm thay đổi dấu định thức, đó là lý do tại sao`det`bị phủ định khi một hàng trục khác được chọn. 

Hệ số loại bỏ được tính là`value * inv_p % MOD`. Cột trụ hiện tại sau đó được đặt rõ ràng về 0. Vòng lặp bên trong bắt đầu tại`col + 1`, thay vì tại`col`, bởi vì giá trị ở cột hiện tại đã được xác định là bằng 0. Điều này giúp tiết kiệm công việc và tránh việc vô tình sửa đổi cột trụ trước khi nó được các hàng sau sử dụng. 

Yếu tố quyết định được nhân với mỗi trục trước khi loại bỏ. Phép loại bỏ Gaussian bảo toàn định thức cho đến khi hoán đổi hàng, bởi vì việc trừ bội số của hàng này với hàng khác không làm thay đổi định thức. Do đó, tích của các trục quay, cùng với các dấu hiệu từ việc hoán đổi hàng, chính xác là modulo xác định (M). 

Python không có lỗi tràn số nguyên có chiều rộng cố định, nhưng việc triển khai vẫn thực hiện tất cả các phép toán theo modulo (M) vì đầu ra toán học là modulo (M) và việc loại bỏ Gaussian theo mô-đun sẽ tránh được các giá trị xác định rất lớn. 

Hai cuộc gọi đến`inv_det`trong biểu thức cuối cùng tương ứng với định thức bình phương ở mẫu số. Không cần phải tính định thức tuyệt đối vì hình vuông làm cho dấu của nó không liên quan. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Trường hợp mẫu đầu tiên là```
2
1 1
1 -1
4 5
```Định thức ma trận là 

[ 
1\cdot(-1)-1\cdot1=-2. 
] 

Modulo (M), đây là (M-2=1000000005). 

| Bước | Xoay vòng | Giá trị xoay | Modulo xác định (M) | 
| --- | --- | --- | --- | 
| Bắt đầu | không | không | 1 | 
| Cột 0 | 0 | 1 | 1 | 
| Cột 1 | 1 | (-2) | (1000000005) | 

Tích độ dài các cạnh của hộp là (4\cdot5=20). Do đó 

# \frac{400}{4} 

1. 

] 

Đầu ra là`100`. 

Dấu vết này thể hiện trường hợp nghịch đảo thông thường. Định thức khác 0 nên vùng khả thi có thể tích hữu hạn và tồn tại nghịch đảo mô đun của định thức. 

### Mẫu 2 

Trường hợp mẫu thứ hai là```
2
1 1
1 1
2 2
```Cả hai hàng đều giống nhau nên định thức bằng 0. 

| Bước | Cột xoay | Đã tìm thấy trục xoay? | Trạng thái quyết định | 
| --- | --- | --- | --- | 
| Bắt đầu | 0 | Có, giá trị 1 | Khác không | 
| Loại bỏ hàng 1 | 0 | Hàng trở thành`0 0`| Vẫn theo dõi việc loại bỏ | 
| Cột 1 | 1 | Không | Không | 

Tại cột 1 không có điểm xoay nào khác 0. Định thức bằng 0 modulo (M), và trong trường hợp này nó cũng bằng 0 dưới dạng số nguyên. Ma trận có hướng rỗng nên tập khả thi không bị chặn và tích phân là vô hạn. 

Đầu ra là`Orz`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n^3)) | Các mục nhập cập nhật loại bỏ Gaussian (O(n^2)) cho mỗi (n) trục | 
| Không gian | (O(n^2)) | Bản thân ma trận chiếm (n^2) mục | 

Với (n=300), khối công việc là khoảng 27 triệu bản cập nhật mục nhập ma trận. Ma trận chỉ yêu cầu (300^2=90000) số nguyên được lưu trữ, thoải mái trong phạm vi 256 MB. Không có phép toán nào phụ thuộc theo cấp số nhân vào (n), do đó thuật toán phù hợp với quy mô dự định của bài toán. 

## Trường hợp thử nghiệm```python
import sys
import io

MOD = 1000000007

def determinant_mod(a):
    n = len(a)
    det = 1

    for col in range(n):
        pivot = col
        while pivot < n and a[pivot][col] == 0:
            pivot += 1

        if pivot == n:
            return 0

        if pivot != col:
            a[col], a[pivot] = a[pivot], a[col]
            det = (-det) % MOD

        p = a[col][col]
        det = det * p % MOD
        inv_p = pow(p, MOD - 2, MOD)

        pivot_row = a[col]

        for row in range(col + 1, n):
            value = a[row][col]
            if value == 0:
                continue

            factor = value * inv_p % MOD
            current = a[row]

            for j in range(col + 1, n):
                current[j] = (
                    current[j] - factor * pivot_row[j]
                ) % MOD

            current[col] = 0

    return det

def solve_string(inp: str) -> str:
    data = inp.split()
    pos = 0
    ans = []

    while pos < len(data):
        n = int(data[pos])
        pos += 1

        a = []
        for _ in range(n):
            row = [int(data[pos + j]) % MOD for j in range(n)]
            pos += n
            a.append(row)

        b = [int(data[pos + i]) for i in range(n)]
        pos += n

        det = determinant_mod(a)

        if det == 0:
            ans.append("Orz")
            continue

        prod = 1
        for x in b:
            prod = prod * x % MOD

        inv_det = pow(det, MOD - 2, MOD)
        value = prod * prod % MOD
        value = value * inv_det % MOD
        value = value * inv_det % MOD

        ans.append(str(value))

    return "\n".join(ans)

def run(inp: str) -> str:
    return solve_string(inp)

sample = """\
2
1 1
1 -1
4 5
2
1 1
1 1
2 2
"""
assert run(sample) == "100\nOrz", "provided samples"

assert run("""\
1
1
1
""") == "1", "minimum size"

assert run("""\
2
2 0
0 2
3 3
""") == "81", "diagonal matrix and equal b"

assert run("""\
2
1 1
1 1
7 7
""") == "Orz", "singular all-equal rows"

assert run("""\
2
1 2
-500000003 1
1 1
""") == "Orz", "determinant divisible by MOD"

# Maximum-size structural case: identity matrix of size 300.
n = 300
lines = [str(n)]
for i in range(n):
    row = ["0"] * n
    row[i] = "1"
    lines.append(" ".join(row))
lines.append(" ".join(["1"] * n))
assert run("\n".join(lines)) == "1", "maximum-size identity matrix"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 / 1`|`1`| Kích thước ma trận tối thiểu và tích phân hữu hạn đơn giản nhất | 
|`[[2,0],[0,2]]`,`b=[3,3]`|`81`| Tỷ lệ xác định và độ dài cạnh bằng nhau | 
|`[[1,1],[1,1]]`,`b=[7,7]`|`Orz`| Phát hiện ma trận số ít | 
|`[[1,2],[-500000003,1]]`,`b=[1,1]`|`Orz`| Định thức số nguyên khác 0 chia hết cho (M) | 
| (300\times300) danh tính, tất cả (b_i=1) |`1`| Kích thước ma trận tối đa và triển khai khối | 

## Vỏ cạnh 

Một ma trận số ít phải bị bác bỏ mặc dù bản thân các bất đẳng thức ban đầu có vẻ bị giới hạn trong mọi (y_i). Vì```
2
1 1
1 1
7 7
```vectơ ((1,-1)^T) thuộc về không gian rỗng của (A). Nếu (x) thỏa mãn các ràng buộc thì (x+t(1,-1)) thỏa mãn chính xác các ràng buộc tương tự đối với mọi thực (t). Do đó, vùng khả thi là không bị giới hạn. Việc loại bỏ Gaussian biến hàng thứ hai thành 0 và không tìm thấy điểm xoay nào ở cột cuối cùng, do đó thuật toán xuất ra`Orz`. 

Một định thức triệt tiêu modulo (M) cũng bị bác bỏ. Coi như```
2
1 2
-500000003 1
1 1
```Yếu tố quyết định là 

[ 
1\cdot1-2(-500000003)=1000000007=M. 
] 

Ma trận khả nghịch đối với các số thực nên bản thân tích phân là hữu hạn. Giá trị của nó là 

[ 
\frac{1}{M^2}. 
] 

Vì (M) là số nguyên tố và tử số là (1) nên mẫu số rút gọn không có modulo nghịch đảo môđun (M). Thủ tục xác định thu được modulo bằng 0 (M), do đó chương trình in chính xác`Orz`. 

Trường hợp nhỏ nhất là```
1
1
1
```Ở đây (Ax=x), do đó khoảng cho phép của (x) là ([0,1]), có âm lượng là (1). Bình phương cho (1). Thuật toán xác định có một trục duy nhất bằng (1) và công thức cuối cùng cho ra (1^2/1^2=1). 

Việc hoán đổi hàng cũng phải được xử lý chính xác. Ví dụ,```
2
0 1
1 0
2 3
```có định thức (-1). Cột đầu tiên không có trục xoay ở hàng đầu tiên nên thuật toán hoán đổi các hàng và ghi lại sự thay đổi dấu. Thể tích của hộp là (6), do đó câu trả lời bắt buộc là (6^2=36). Việc quên dấu hiệu định thức sẽ không ảnh hưởng đến câu trả lời cuối cùng cụ thể này vì định thức là bình phương, nhưng việc xử lý hoán đổi hàng vẫn cần thiết cho chính phép tính định thức và để duy trì một bất biến tổng quát chính xác. 

Cuối cùng, trường hợp mọi (b_i) bằng nhau không phải là trường hợp đặc biệt về mặt toán học. Vì```
2
2 0
0 2
3 3
```hộp chuyển đổi có thể tích (9), trong khi định thức có giá trị tuyệt đối (4). Do đó, vùng khả thi có khối lượng (9/4) và câu trả lời được yêu cầu là (81/16). Modulo (M), chương trình tính toán (81\cdot16^{-1}). Điều này thực hiện yêu cầu đầu ra hợp lý thực tế thay vì chỉ các trường hợp mẫu số chia tử số.
