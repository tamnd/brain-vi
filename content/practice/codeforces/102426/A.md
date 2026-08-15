---
title: "CF 102426A - \u81ea\u7136\u8bed\u8a00\u5904\u7406"
description: "Mỗi văn bản đã được chuyển đổi thành một vectơ tần số. Như vậy phần xử lý chuỗi đã hoàn toàn biến mất. Đối với một trường hợp thử nghiệm, chúng ta chỉ cần kiểm tra một tập hợp (n) vectơ, mỗi vectơ có (m) tọa độ nguyên và quyết định xem các vectơ đó có phụ thuộc tuyến tính hay không."
date: "2026-08-12T19:20:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102426
codeforces_index: "A"
codeforces_contest_name: "The 7-th BIT Campus Programming Contest for Junior Grade Group"
rating: 0
weight: 102426
solve_time_s: 336
verified: true
draft: false
---

[CF 102426A - \u81ea\u7136\u8bed\u8a00\u5904\u7406](https://codeforces.com/problemset/problem/102426/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 5 phút 36 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Mỗi văn bản đã được chuyển đổi thành một vectơ tần số. Như vậy phần xử lý chuỗi đã hoàn toàn biến mất. Đối với một trường hợp thử nghiệm, chúng ta chỉ cần kiểm tra một tập hợp (n) vectơ, mỗi vectơ có (m) tọa độ nguyên và quyết định xem các vectơ đó có phụ thuộc tuyến tính hay không. 

Câu hỏi này tương đương với việc hỏi liệu có tồn tại các hệ số (c_1,c_2,\ldots,c_n) không, sao cho 

[ 
c_1A_1+c_2A_2+\cdots+c_nA_n=0. 
] 

Nếu các hệ số như vậy tồn tại thì các vectơ phụ thuộc tuyến tính và câu trả lời là`YES`. Ngược lại chúng độc lập tuyến tính và câu trả lời là`NO`. 

Kích thước rất nhỏ. Có nhiều nhất 10 vectơ và mỗi vectơ có nhiều nhất 4 tọa độ. Điều này ngay lập tức đưa ra một điều kiện cần thiết hữu ích: trong một không gian vectơ chiều (m), không quá (m) vectơ có thể độc lập tuyến tính. Như vậy, bất cứ khi nào (n>m), câu trả lời sẽ tự động được`YES`. 

Ngay cả khi (n\le m), chúng ta vẫn cần xác định xem các vectơ có thực sự có hạng đầy đủ hay không. Vì (m\le4), việc loại bỏ Gaussian là quá đủ nhanh. Có nhiều nhất (10\times4=40) số đầu vào cho mỗi trường hợp, do đó, ngay cả các thuật toán có độ phức tạp tiệm cận kém hơn đáng kể cũng sẽ phù hợp với các giới hạn đã nêu. Các giới hạn nhỏ rất hữu ích cho các lựa chọn triển khai nhưng chúng không làm thay đổi vấn đề toán học cơ bản: chúng ta cần tính thứ hạng của ma trận. 

Có hai chi tiết định dạng đầu vào đáng được xử lý cẩn thận. Đầu tiên, các vectơ 0 ngay lập tức làm cho một nhóm vectơ phụ thuộc tuyến tính. Ví dụ,```
1 2
0 0
```có câu trả lời`YES`, vì hệ số của vectơ 0 có thể được chọn là 1. Việc triển khai xếp hạng chỉ tìm kiếm các hàng trùng lặp có thể báo cáo tính độc lập không chính xác. 

Thứ hai, các vectơ trùng lặp hoặc tỷ lệ cũng tạo ra sự phụ thuộc. Ví dụ,```
2 2
1 2
2 4
```có câu trả lời`YES`, vì vectơ thứ hai gấp đôi vectơ thứ nhất. Kiểm tra xem các hàng có khác nhau hay không là chưa đủ. Điều quan trọng là liệu một hàng có thể được biểu diễn dưới dạng kết hợp tuyến tính của các hàng khác hay không. 

Câu lệnh được cung cấp cho biết giá trị đầu vào đầu tiên là (T), trong khi mẫu được hiển thị bắt đầu trực tiếp bằng`n m`. Hai phần này không nhất quán. Giải pháp bên dưới chấp nhận định dạng nhiều trường hợp thử nghiệm chính thức và cũng nhận dạng định dạng mẫu được hiển thị, do đó bản thân thuật toán không bị ảnh hưởng bởi sự khác biệt về định dạng này. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ trực tiếp là liệt kê mọi tập hợp con khác rỗng của vectơ và kiểm tra xem tập hợp con đó có phụ thuộc tuyến tính hay không. Nếu bất kỳ tập hợp con nào phụ thuộc thì toàn bộ nhóm vectơ đều phụ thuộc. Đối với mỗi tập hợp con, chúng ta có thể thực hiện phép loại bỏ Gaussian trên các hàng của nó. 

Cách tiếp cận này đúng vì một họ vectơ phụ thuộc tuyến tính chính xác khi nó chứa một họ con phụ thuộc tuyến tính khác rỗng. Độ phức tạp trong trường hợp xấu nhất của nó là (O(2^n m^3)), vì có (2^n-1) tập con khác rỗng và chi phí tính toán xếp hạng (O(m^3)). Với mức tối đa thực tế (n=10) và (m=4), đây nhiều nhất là khoảng (1024\cdot64=65536) các phép toán quy mô loại bỏ cơ bản cho mỗi trường hợp thử nghiệm, do đó, ngay cả cách tiếp cận bạo lực này cũng có thể vượt qua một cách thoải mái. 

Vấn đề với cách tiếp cận đó không phải là các giới hạn đã cho mà là sự phụ thuộc hàm mũ không cần thiết vào (n). Nếu cùng một nhiệm vụ cho phép hàng nghìn hoặc hàng trăm nghìn vectơ thì việc liệt kê các tập hợp con sẽ ngay lập tức trở nên bất khả thi. Cấu trúc của sự phụ thuộc tuyến tính cho chúng ta một lộ trình rõ ràng hơn nhiều: tất cả các vectơ có thể được đặt vào một ma trận và số lượng vectơ độc lập chính xác là thứ hạng của ma trận. 

Quan sát quan trọng là việc loại bỏ Gaussian sẽ tính toán thứ hạng này một cách trực tiếp. Mỗi trục xoay thành công xác định một hướng độc lập. Nếu chúng ta tìm thấy chính xác (n) điểm xoay, thì mỗi một trong số (n) vectơ đóng góp một hướng độc lập mới, do đó các vectơ độc lập. Nếu tồn tại ít hơn (n) trục xoay, một số vectơ là sự kết hợp của các hướng độc lập trước đó, do đó họ phụ thuộc. 

Vì (m) là số cột nên thứ hạng không bao giờ vượt quá (m). Điều này cũng giải thích trường hợp (n>m): không thể có (n) điểm xoay chỉ trong (m) cột. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(2^n m^3)) | (O(nm)) | Được chấp nhận ở đây, nhưng theo cấp số nhân không cần thiết | 
| Tối ưu | (O(nm^2)) | (O(nm)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc các vectơ (n), (m) và (n), coi các vectơ là các hàng của ma trận (n\times m). Điều này biến câu hỏi ban đầu trực tiếp thành vấn đề xếp hạng ma trận. 
2. Nếu (n>m), xuất ngay`YES`. Một không gian vectơ hai chiều (m) không thể chứa nhiều hơn (m) vectơ độc lập tuyến tính, do đó sự phụ thuộc được đảm bảo. 
3. Duy trì hàng trục hiện tại. Ban đầu nó là hàng 0. Đối với mỗi cột, tìm kiếm các hàng còn lại để tìm giá trị khác 0 trong cột này. Nếu không có hàng như vậy tồn tại thì cột này không thể cung cấp một hướng độc lập khác, vì vậy hãy chuyển sang cột tiếp theo. 
4. Khi tìm thấy một trục xoay khác 0, hãy hoán đổi hàng đó vào vị trí trục xoay hiện tại. Chúng ta có thể chia hàng trục cho giá trị trục của nó, làm cho trục xoay bằng 1. Ở đây sử dụng số học hợp lý chính xác để kết quả không phụ thuộc vào độ chính xác của dấu phẩy động. 
5. Loại bỏ cột xoay hiện tại khỏi mọi hàng khác. Sau khi loại bỏ, mọi cột trụ được xử lý trước đó đều có số 0 bên ngoài hàng trục của nó. Điều này đưa ra cách biểu diễn theo kiểu cấp bậc hàng trong đó mọi trục xoay thành công đều tương ứng với một chiều độc lập. 
6. Tăng thứ hạng và di chuyển hàng trục tới vị trí tiếp theo. Khi tất cả các cột đã được xử lý, số lượng trục xoay thành công là thứ hạng của ma trận. 
7. So sánh thứ hạng với (n). Nếu như`rank == n`, tất cả (n) vectơ độc lập tuyến tính, do đó đầu ra`NO`. Ngược lại, thứ hạng nhỏ hơn số lượng vectơ, do đó đầu ra`YES`. 

### Tại sao nó hoạt động 

Bất biến trung tâm là sau mỗi lần xoay thành công, các hàng trục được xử lý biểu thị các hướng độc lập lẫn nhau và mọi cột được xử lý đã bị loại bỏ khỏi các hàng khác. Một trục mới chỉ có thể tồn tại nếu vẫn còn một hàng chứa thông tin mà các hàng trục trước đó không thể tạo ra được. 

Do đó, mỗi trục sẽ tăng thứ hạng lên đúng một. Việc loại bỏ Gaussian kết thúc với số trục xoay chính xác bằng kích thước khoảng của các vectơ đầu vào. Một họ vectơ có kích thước (n) độc lập tuyến tính chính xác khi khoảng của nó có thứ nguyên (n), do đó`rank == n`chính xác là điều kiện cần thiết`NO`trả lời. 

## Giải pháp Python```python
import sys
from fractions import Fraction

input = sys.stdin.readline

def independent(vectors, m):
    n = len(vectors)

    if n > m:
        return False

    a = [[Fraction(x) for x in row] for row in vectors]

    rank = 0

    for col in range(m):
        pivot = -1

        for row in range(rank, n):
            if a[row][col] != 0:
                pivot = row
                break

        if pivot == -1:
            continue

        a[rank], a[pivot] = a[pivot], a[rank]

        pivot_value = a[rank][col]
        for j in range(col, m):
            a[rank][j] /= pivot_value

        for row in range(n):
            if row == rank:
                continue

            factor = a[row][col]
            if factor == 0:
                continue

            for j in range(col, m):
                a[row][j] -= factor * a[rank][j]

        rank += 1

        if rank == n:
            return True

    return False

def solve():
    data = sys.stdin.buffer.read().split()
    if not data:
        return

    # The formal statement uses T test cases.
    # The displayed sample omits T and starts directly with n m.
    # Detect both formats from the first input line.
    lines = sys.stdin.buffer.read().splitlines()

    # Re-read using the raw data above if possible.
    # For the formal format, the first line contains only T.
    first_line = lines[0].split()

    if len(first_line) == 1:
        t = int(first_line[0])
        pos = 1

        answers = []

        for _ in range(t):
            n = int(data[pos])
            m = int(data[pos + 1])
            pos += 2

            vectors = []
            for _ in range(n):
                vectors.append(list(map(int, data[pos:pos + m])))
                pos += m

            answers.append("NO" if independent(vectors, m) else "YES")

        sys.stdout.write("\n".join(answers))
    else:
        # Format used by the displayed sample: n m followed by n vectors.
        pos = 0
        n = int(data[pos])
        m = int(data[pos + 1])
        pos += 2

        vectors = []
        for _ in range(n):
            vectors.append(list(map(int, data[pos:pos + m])))
            pos += m

        sys.stdout.write("NO\n" if independent(vectors, m) else "YES\n")

if __name__ == "__main__":
    solve()
```các`independent`trước tiên, hàm xử lý đối số thứ nguyên. Khi (n>m), nó trả về`False`vì các vectơ không thể độc lập. Đây là lối tắt toán học tương tự được sử dụng trong hướng dẫn này. 

Ma trận được chuyển đổi thành`Fraction`giá trị trước khi loại bỏ. Mặc dù đầu vào chỉ bao gồm các số nguyên, phép chia trong quá trình loại bỏ Gaussian có thể tạo ra các giá trị hữu tỷ. Việc sử dụng số dấu phẩy động thường có tác dụng đối với các giới hạn nhỏ này, nhưng số học chính xác làm cho phép kiểm tra sự phụ thuộc mạnh mẽ về mặt toán học và tránh việc chọn một epsilon tùy ý để quyết định xem một giá trị có bằng 0 hay không. 

Vòng lặp bên ngoài xử lý từng cột như một vị trí xoay có thể. Việc tìm kiếm bắt đầu lúc`rank`, bởi vì các hàng phía trên vị trí đó đã chứa các điểm xoay được thiết lập. Khi tìm thấy giá trị khác 0, hàng tương ứng sẽ được hoán đổi vào vị trí. 

Hàng trục được chuẩn hóa để làm cho trục xoay bằng 1. Việc triển khai sau đó sẽ loại bỏ cột trục khỏi mọi hàng khác. Việc loại bỏ cả bên trên và bên dưới trục xoay tốn nhiều công sức hơn một chút so với mức tối thiểu cần thiết cho dạng cấp bậc hàng thông thường, nhưng nó giữ cho ma trận ở dạng rút gọn và làm cho tính bất biến thứ hạng trở nên đặc biệt đơn giản. 

Sớm`rank == n`lợi nhuận là an toàn vì thứ hạng chỉ có thể tăng lên. Khi đã có (n) điểm xoay, tất cả (n) vectơ đều độc lập và không cột nào sau này có thể thay đổi kết luận đó. 

Trình phân tích cú pháp chứa một lớp tương thích nhỏ vì câu lệnh được cung cấp và mẫu được hiển thị không thống nhất về việc liệu (T) có hiện diện hay không. Nếu dòng đầu tiên chứa một số nguyên thì nó được coi là (T). Nếu nó chứa hai số nguyên, chúng được coi là (n,m), khớp với mẫu được hiển thị. Đại số tuyến tính thực tế ở cả hai dạng đều giống nhau. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Mẫu được hiển thị chứa hai vectơ:```
2 2
1 1
0 1
```Ma trận là 

[ 
\bắt đầu{pmatrix} 
1&1\ 
0&1 
\end{pmatrix}. 
] 

Việc loại bỏ tiến hành như sau. 

| Cột | Hàng xoay | Trạng thái ma trận | Xếp hạng | 
| --- | --- | --- | --- | 
| 0 | 0 | (\begin{pmatrix}1&1\0&1\end{pmatrix}) | 1 | 
| 1 | 1 | (\begin{pmatrix}1&0\0&1\end{pmatrix}) | 2 | 

Hạng cuối cùng là 2, bằng số lượng vectơ. Do đó các vectơ độc lập tuyến tính và câu trả lời là`NO`. 

Ví dụ này thể hiện tính bất biến cơ bản: mỗi trục xoay thành công đều đóng góp một hướng độc lập. Vectơ thứ hai không phải là bội số của vectơ thứ nhất, do đó tồn tại trục xoay thứ hai. 

### Mẫu 2 

Xét hai vectơ tỷ lệ:```
1
2 2
1 2
2 4
```Ma trận là 

[ 
\bắt đầu{pmatrix} 
1&2\ 
2&4 
\end{pmatrix}. 
] 

| Cột | Hàng xoay | Trạng thái ma trận | Xếp hạng | 
| --- | --- | --- | --- | 
| 0 | 0 | (\begin{pmatrix}1&2\2&4\end{pmatrix}) | 1 | 
| 1 | không | (\begin{pmatrix}1&2\0&0\end{pmatrix}) | 1 | 

Chỉ có một trục được tìm thấy. Hàng thứ hai bằng 0 vì nó gấp đôi hàng đầu tiên. Do đó, hạng là 1, nhỏ hơn (n=2), nên câu trả lời là`YES`. 

Dấu vết này thực hiện trường hợp các vectơ khác biệt nhưng vẫn phụ thuộc tuyến tính. Chỉ kiểm tra xem hai hàng có bằng nhau hay không sẽ bỏ lỡ ví dụ này, trong khi xếp hạng sẽ phát hiện ra nó ngay lập tức. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(nm^2)) | Tối đa (m) cột trục được xử lý và loại bỏ các mục nhập ma trận chạm trục (O(nm)) | 
| Không gian | (O(nm)) | Ma trận lưu trữ tất cả (n) vectơ | 

Với (n\le10) và (m\le4), ma trận chứa tối đa 40 phần tử. Thậm chí chính xác`Fraction`số học dễ dàng đủ nhanh cho các giới hạn này và mức sử dụng bộ nhớ không đáng kể so với giới hạn 64 MB. 

Giới hạn tiệm cận cũng thích hợp ngoài những ràng buộc nhỏ này. Giải pháp tránh liệt kê các tập hợp con và tính toán toàn bộ thứ hạng trong một lần loại bỏ. 

## Trường hợp thử nghiệm```python
import sys
import io
from fractions import Fraction

def independent(vectors, m):
    n = len(vectors)

    if n > m:
        return False

    a = [[Fraction(x) for x in row] for row in vectors]
    rank = 0

    for col in range(m):
        pivot = -1

        for row in range(rank, n):
            if a[row][col] != 0:
                pivot = row
                break

        if pivot == -1:
            continue

        a[rank], a[pivot] = a[pivot], a[rank]

        p = a[rank][col]
        for j in range(col, m):
            a[rank][j] /= p

        for row in range(n):
            if row == rank:
                continue

            factor = a[row][col]
            if factor == 0:
                continue

            for j in range(col, m):
                a[row][j] -= factor * a[rank][j]

        rank += 1

        if rank == n:
            return True

    return False

def run(inp: str) -> str:
    lines = inp.strip().splitlines()
    data = inp.split()

    if not data:
        return ""

    first_line = lines[0].split()

    if len(first_line) == 1:
        t = int(first_line[0])
        pos = 1
        out = []

        for _ in range(t):
            n = int(data[pos])
            m = int(data[pos + 1])
            pos += 2

            vectors = []
            for _ in range(n):
                vectors.append(list(map(int, data[pos:pos + m])))
                pos += m

            out.append("NO" if independent(vectors, m) else "YES")

        return "\n".join(out) + "\n"

    n = int(data[0])
    m = int(data[1])
    pos = 2

    vectors = []
    for _ in range(n):
        vectors.append(list(map(int, data[pos:pos + m])))
        pos += m

    return ("NO\n" if independent(vectors, m) else "YES\n")

# Provided sample, whose displayed format omits T.
assert run("""\
2 2
1 1
0 1
""") == "NO\n", "sample 1"

# Minimum-size case: one nonzero vector is independent.
assert run("""\
1
1 1
7
""") == "NO\n", "minimum nonzero vector"

# Zero vector is always dependent.
assert run("""\
1
1 3
0 0 0
""") == "YES\n", "zero vector"

# Two proportional vectors are dependent.
assert run("""\
1
2 2
1 2
2 4
""") == "YES\n", "proportional vectors"

# Maximum dimensions, with four independent vectors.
assert run("""\
1
4 4
1 0 0 0
0 1 0 0
0 0 1 0
0 0 0 100
""") == "NO\n", "maximum-size independent case"

# More vectors than dimensions must be dependent.
assert run("""\
1
5 4
1 0 0 0
0 1 0 0
0 0 1 0
0 0 0 1
1 1 1 1
""") == "YES\n", "n greater than m"

# Several test cases in the formal format.
assert run("""\
3
2 2
1 1
0 1
2 2
1 2
2 4
3 2
1 0
0 1
1 1
""") == "NO\nYES\nYES\n", "multiple test cases"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 1 1 / 7`|`NO`| Trường hợp kích thước tối thiểu với một vectơ khác 0 | 
|`1 / 1 3 / 0 0 0`|`YES`| Không vectơ | 
|`1 / 2 2 / 1 2 / 2 4`|`YES`| Vectơ tỷ lệ | 
|`1 / 4 4 / ...`|`NO`| Kích thước tối đa với bốn vectơ độc lập | 
|`1 / 5 4 / ...`|`YES`| Điều kiện biên (n>m) | 
| Ba trường hợp thử nghiệm chính thức |`NO YES YES`| Nhiều kết quả phân tích cú pháp trường hợp thử nghiệm và phụ thuộc hỗn hợp | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là vectơ 0. Coi như```
1
1 3
0 0 0
```Thuật toán bắt đầu với thứ hạng 0. Trong cột 0 không có mục nhập nào khác 0 và điều tương tự cũng xảy ra ở cột 1 và 2. Không tìm thấy trục xoay nào, vì vậy thứ hạng cuối cùng vẫn là 0. Vì (0<1), các vectơ phụ thuộc và đầu ra là`YES`. Điều này hoạt động mà không cần bất kỳ kiểm tra vectơ 0 đặc biệt nào vì việc loại bỏ Gaussian tự nhiên coi hàng 0 là không đóng góp thứ hạng nào. 

Trường hợp cạnh thứ hai là một cặp vectơ khác nhau nhưng tỷ lệ:```
1
2 2
1 2
2 4
```Hàng đầu tiên cung cấp một điểm xoay trong cột 0, tăng thứ hạng lên 1. Việc loại bỏ cột 0 khỏi hàng thứ hai sẽ thay đổi nó từ`(2, 4)`ĐẾN`(0, 0)`. Cột thứ hai không còn ứng cử viên nào khác 0 nên hạng vẫn là 1. Vì có hai vectơ nhưng chỉ có một hướng độc lập nên câu trả lời là`YES`. 

Trường hợp cạnh thứ ba xảy ra khi có nhiều vectơ hơn tọa độ:```
1
5 4
1 0 0 0
0 1 0 0
0 0 1 0
0 0 0 1
1 1 1 1
```Thuật toán trả về`YES`ngay lập tức vì (5>4). Không cần loại bỏ. Chỉ có bốn hướng tọa độ nên năm vectơ không thể độc lập. 

Trường hợp cạnh thứ tư là ma trận vuông hạng đầy đủ:```
1
4 4
1 0 0 0
0 1 0 0
0 0 1 0
0 0 0 100
```Thuật toán tìm thấy một trục trong mỗi cột. Thứ hạng đạt tới 4, bằng số vectơ nên nó trả về`NO`. Giá trị 100 không gây ra sự xử lý đặc biệt nào vì việc loại bỏ sử dụng số học hữu tỉ chính xác. 

Những trường hợp này bao gồm những cách chính mà một giải pháp hời hợt có thể thất bại: nhầm lẫn các vectơ riêng biệt với các vectơ độc lập, bỏ qua vectơ 0, quên giới hạn kích thước (n>m) hoặc sử dụng số học gần đúng mà không xem xét việc phát hiện điểm 0 chính xác.
