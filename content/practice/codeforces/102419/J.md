---
title: "CF 102419J - Cảnh sát Jaber"
description: "Hãy coi mỗi ô được chiếu sáng là một cạnh giữa hàng và cột của nó. Một hàng là một đỉnh ở bên trái, một cột là một đỉnh ở bên phải và số 1 ở vị trí (i, j) có nghĩa là hàng và cột tương ứng được kết nối. Tổng số hàng cần thiết cho chúng ta biết bậc của mỗi đỉnh hàng."
date: "2026-08-15T08:56:00+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102419
codeforces_index: "J"
codeforces_contest_name: "SPC 2019"
rating: 0
weight: 102419
solve_time_s: 367
verified: false
draft: false
---

[CF 102419J - Cảnh sát Jaber](https://codeforces.com/problemset/problem/102419/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 6 phút 7 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Hãy coi mỗi ô được chiếu sáng là một cạnh giữa hàng và cột của nó. Một hàng là một đỉnh ở bên trái, một cột là một đỉnh ở bên phải và một`1`ở vị trí`(i, j)`có nghĩa là hàng và cột tương ứng được kết nối. 

Tổng số hàng cần thiết cho chúng ta biết bậc của mỗi đỉnh hàng. Chúng ta có thể tự do lựa chọn các độ cột và vị trí của tất cả các`1`S. Trong quá trình kiểm tra, việc loại bỏ một hàng hoặc một cột có nghĩa là xóa đỉnh đó và tất cả các cạnh liên quan. Jaber hài lòng chính xác khi đỉnh bị loại bỏ có nhiều nhất một cạnh phụ còn lại. 

Do đó, câu hỏi đặt ra là liệu chúng ta có thể xây dựng một đồ thị hai bên với các bậc quy định ở phía hàng hay không, cùng với thứ tự của tất cả các đỉnh trong đó mỗi đỉnh bị loại bỏ có nhiều nhất là một bậc hiện tại. 

Giới hạn`n, m <= 1000`có nghĩa là bản thân đầu ra có thể chứa tới một triệu ký tự ma trận, do đó`O(nm)`thuật toán đã gần tối ưu. Một thuật toán có công thức bậc hai ngoài việc xây dựng ma trận là không cần thiết và bất kỳ thuật toán nào có hàm mũ đều hoàn toàn không khả thi. Giới hạn bộ nhớ cũng cho phép thoải mái lưu trữ một`1000 x 1000`ma trận. 

Có một số trường hợp đặc biệt có thể đánh lừa một công trình chỉ dựa trên tổng số`1`S. Ví dụ,```
2 2
2 2
```có bốn`1`s, nhưng mỗi hàng phải chứa cả hai cột. Ma trận duy nhất có thể là```
11
11
```trong đó có chứa một chu trình. Mọi đỉnh trong chu trình đó đều có bậc hai, do đó không có phép kiểm tra đầu tiên hợp lệ. Câu trả lời đúng là`NO`. 

Trường hợp ranh giới thứ hai là```
2 2
2 0
```Đây là câu trả lời`YES`. Chúng ta có thể sử dụng```
11
00
```và kiểm tra cột 2 trước, sau đó là hàng 1, rồi hàng 2, rồi cột 1. Cấu trúc yêu cầu mỗi hàng phải có cột riêng sẽ từ chối trường hợp này một cách không chính xác, mặc dù một số hàng có thể chia sẻ một cột trung tâm một cách an toàn. 

Trường hợp hoàn toàn bằng 0 cũng cần xử lý riêng. Vì```
3 4
0 0 0
```câu trả lời là`YES`, bởi vì toàn bộ ma trận có thể bằng 0 và mọi hàng và cột có thể được kiểm tra theo bất kỳ thứ tự nào. Một công thức liên quan đến số hàng dương không được vô tình cho rằng ít nhất một hàng chứa một`1`. 

## Phương pháp tiếp cận 

Một giải pháp brute-force trực tiếp có thể liệt kê mọi ma trận nhị phân có tổng hàng khớp với đầu vào, sau đó tìm kiếm thứ tự kiểm tra. Đối với ma trận cố định, điều kiện kiểm tra có thể được kiểm tra bằng cách xem nó dưới dạng biểu đồ và liên tục loại bỏ nhiều nhất một đỉnh bậc. Tuy nhiên, số lượng ma trận có thể có đã 

[ 
\prod_{i=1}^{n} \binom{m}{a_i}. 
] 

Nếu chúng tôi thử thêm mọi lệnh kiểm tra có thể, sẽ có`(n+m)!`thứ tự cho mỗi ma trận. Do đó tổng số cặp thứ tự ma trận là 

[ 
\left(\prod_{i=1}^{n}\binom{m}{a_i}\right)(n+m)!. 
] 

Trong trường hợp xấu nhất`n = m = 1000`và mọi`a_i = 500`, do đó, ngay cả số lượng ma trận ứng cử viên cũng là 

[ 
\binom{1000}{500}^{1000}. 
] 

Không có khả năng khám phá không gian này. 

Quan sát hữu ích là lệnh kiểm tra hợp lệ có cách diễn giải biểu đồ rất đơn giản. Nếu đồ thị chứa một chu trình thì ban đầu mọi đỉnh trên chu trình đó đều có ít nhất bậc hai bên trong chu trình. Bất kỳ đỉnh nào của chu trình mà chúng ta cố gắng kiểm tra trước tiên sẽ có ít nhất hai cạnh còn lại, do đó việc kiểm tra không thành công. Ngược lại, mỗi khu rừng đều có một chiếc lá và việc xóa đi nhiều lần các lá cuối cùng sẽ loại bỏ toàn bộ khu rừng. Do đó lệnh kiểm tra hợp lệ tồn tại chính xác khi đồ thị của`1`tế bào là một khu rừng. 

Bây giờ vấn đề trở nên đơn giản hơn nhiều. Cho phép`S`là tổng số`1`s và để`p`là số hàng có bậc dương. Chúng tôi muốn một khu rừng có độ hàng chính xác bằng các giá trị đã cho trong khi sử dụng tối đa`m`các đỉnh cột. 

Chúng ta có thể xây dựng thành phần kết nối nhỏ nhất có thể chứa tất cả các hàng dương. Chọn một cột làm đỉnh trung tâm và kết nối mọi hàng dương với nó. Nếu hàng`i`cần bằng cấp`a_i`, cho nó cái khác`a_i - 1`cột riêng. Các cột riêng tư này có cấp độ một nên chúng là các lá. Thành phần kết quả là một cái cây. 

Số cột cần thiết là 

[ 
1+\sum_{a_i>0}(a_i-1) 
=1+S-p. 
] 

Con số này cũng cần thiết. Hãy xem xét bất kỳ khu rừng nào chứa tất cả các hàng dương. Với mỗi thành phần cây chứa các cạnh, số cạnh bằng số đỉnh trừ đi một. Nếu như`q`các cột tham gia vào các cạnh và có`c`các thành phần không trống thì 

[ 
S=p+q-c. 
] 

Do đó 

[ 
q=S-p+c\ge S-p+1. 
] 

Vậy nếu`S-p+1 > m`, không có khu rừng nào có thể nhận ra được độ hàng. Nếu nhiều nhất là`m`, việc xây dựng cột trung tâm của chúng tôi hiện thực hóa chúng. 

Brute-force hoạt động vì nó tìm kiếm biểu đồ và thứ tự của nó một cách rõ ràng, nhưng không thành công vì có rất nhiều khả năng theo cấp số nhân. Đặc tính rừng loại bỏ hoàn toàn việc tìm kiếm và việc xây dựng cột trung tâm đưa ra ma trận cần thiết một cách trực tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ, ít nhất là tỷ lệ thuận với`∏ C(m, a_i)`ma trận |`O(nm)`| Quá chậm | 
| Tối ưu |`O(nm)`|`O(nm)`| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính toán`S`, tổng của tất cả các độ hàng, và`p`, số hàng có bậc dương. Nếu như`p = 0`, mọi ô đều có thể bằng 0, do đó xuất ra ma trận 0 và thứ tự bất kỳ của tất cả các hàng và cột. 
2. Tính toán`needed = S - p + 1`. Đây là số cột cần thiết để xây dựng cột trung tâm. Nếu như`needed > m`, in`NO`. Đối số giới hạn dưới ở trên cho thấy rằng không có khu rừng hợp lệ nào có thể vừa với số lượng cột có sẵn. 
3. Sử dụng cột 1 làm cột trung tâm. Đối với mỗi hàng có`a_i > 0`, đặt một`1`trong cột 1. Điều này mang lại cho mỗi hàng dương một cạnh trong khi vẫn giữ cho tất cả các hàng như vậy được kết nối qua cùng một đỉnh. 
4. Đối với mỗi hàng dương`i`, cộng chính xác`a_i - 1`thêm vào`1`S. Cung cấp cho các ô này các cột liên tiếp bắt đầu từ cột 2 và không bao giờ sử dụng lại các cột riêng tư này. Mỗi cột như vậy có đúng một cạnh nên nó là một chiếc lá của rừng. 
5. Tất cả các ô ma trận còn lại đều bằng 0. Phần khác 0 được xây dựng là một cây: cột trung tâm được kết nối với mọi hàng dương và mọi cạnh bổ sung đi từ một hàng đến cột lá tươi. 
6. Đặt tất cả các cột riêng lên hàng đầu theo thứ tự kiểm tra. Mỗi người trong số họ hiện có chính xác một`1`, so inspecting it is safe and removes that edge.
 7. After the private columns of every positive row have been removed, each positive row has only its edge to the central column. Kiểm tra tất cả các hàng tích cực tiếp theo. Mỗi hàng như vậy bây giờ còn lại đúng một`1`. 
8. Kiểm tra tất cả các hàng bằng 0. Họ không có`1`không có gì, vì vậy họ luôn được an toàn. 
9. Kiểm tra cột trung tâm và sau đó kiểm tra từng cột không sử dụng. Đến thời điểm này mọi cạnh đã biến mất, vì vậy các cột này không còn lại`1`S. 

Tại sao nó hoạt động. Các ô khác 0 tạo thành một cây. Các cột riêng tư là các lá của nó và việc loại bỏ chúng sẽ làm cho mỗi hàng dương lần lượt trở thành một lá. Khi tất cả các hàng dương bị loại bỏ, cột trung tâm không còn cạnh nào nữa. Mọi đỉnh khác đều bị cô lập. Do đó, mỗi đỉnh được kiểm tra với mức độ không quá một, trong khi mỗi hàng nhận được chính xác số lượng quy định của nó.`1`S. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))

    total = sum(a)
    positive = sum(x > 0 for x in a)

    if positive == 0:
        out = ["YES"]
        out.extend(["0" * m for _ in range(n)])

        for i in range(1, n + 1):
            out.append(f"row {i}")
        for j in range(1, m + 1):
            out.append(f"col {j}")

        sys.stdout.write("\n".join(out))
        return

    needed = total - positive + 1

    if needed > m:
        sys.stdout.write("NO\n")
        return

    # bytearray keeps the matrix compact while allowing O(1) cell updates.
    matrix = [bytearray(b"0" * m) for _ in range(n)]

    # Column 1 is the central column.
    next_col = 1  # zero-based index of the next private column

    # The central column is column 0.
    for i, degree in enumerate(a):
        if degree == 0:
            continue

        matrix[i][0] = ord("1")

        # Use fresh private columns for the remaining degree.
        for _ in range(degree - 1):
            next_col += 1
            matrix[i][next_col - 1] = ord("1")

    private_columns = next_col - 1

    out = ["YES"]
    out.extend(row.decode() for row in matrix)

    # Every private column is a leaf.
    for j in range(2, private_columns + 1):
        out.append(f"col {j}")

    # Positive rows become leaves after their private columns disappear.
    for i, degree in enumerate(a):
        if degree > 0:
            out.append(f"row {i + 1}")

    # Zero rows are isolated.
    for i, degree in enumerate(a):
        if degree == 0:
            out.append(f"row {i + 1}")

    # The central column is now isolated.
    out.append("col 1")

    # All unused columns are isolated from the start.
    for j in range(private_columns + 1, m + 1):
        out.append(f"col {j}")

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Phần đầu tiên tính toán hai đại lượng đặc trưng cho tính khả thi.`total`là số cạnh mà đồ thị cuối cùng phải chứa, trong khi`positive`đếm các đỉnh hàng thực sự tham gia vào biểu đồ. 

biểu thức`total - positive + 1`là số cột được sử dụng bởi công trình. Cột 1 là đỉnh trung tâm chung. Mỗi hàng tích cực đều đóng góp`a_i - 1`cột riêng. các`needed > m`việc kiểm tra được thực hiện trước khi cấp phát ma trận, do đó một trường hợp không thể thực hiện được sẽ ngay lập tức tạo ra`NO`. 

Ma trận được lưu trữ dưới dạng`bytearray`đồ vật. Một chuỗi Python bình thường không thể được sửa đổi tại chỗ, trong khi danh sách các ký tự riêng lẻ sẽ tiêu tốn nhiều bộ nhớ hơn đáng kể. MỘT`bytearray`gán ô trực tiếp và giữ cho ma trận một triệu ô có kích thước nhỏ. 

Biến`next_col`sử dụng lập chỉ mục dựa trên 0 cho ma trận, trong khi đầu ra sử dụng số cột dựa trên một. Sau khi đặt trung tâm`1`trong chỉ mục 0, cột riêng đầu tiên là chỉ số ma trận một, tương ứng với cột đầu ra 2. Việc giữ chuyển đổi này ở một nơi sẽ tránh được lỗi từng cái một. 

Lệnh kiểm tra có chủ ý loại bỏ các cột riêng tư trước các hàng tương ứng của chúng. Nếu kiểm tra ngay một hàng cấp độ 4 thì vẫn có 4 đèn sáng. Sau khi ba cột riêng của nó đã được kiểm tra, chỉ còn lại cạnh trung tâm, vì vậy hàng này an toàn. 

Không cần xử lý tràn số nguyên trong Python. Ngay cả số tiền tối đa có thể cũng chỉ`n * m = 10^6`, mặc dù số nguyên Python cũng sẽ xử lý các giá trị lớn hơn một cách an toàn. 

## Ví dụ đã hoạt động 

Đối với mẫu 1,```
4 4
1 0 0 0
```chúng tôi có`total = 1`,`positive = 1`, Và`needed = 1`. Không có cột riêng tư là cần thiết. Hàng dương duy nhất kết nối trực tiếp với cột trung tâm. 

| Bước |`total`|`positive`|`needed`|`next_col`| Hành động | 
| --- | --- | --- | --- | --- | --- | 
| Đọc đầu vào | 1 | 1 | 1 | 1 | Hàng 1 cần một cạnh | 
| Xây dựng hàng 1 | 1 | 1 | 1 | 1 | Đặt`1`ở cột 1 | 
| Xây dựng hàng 2 đến 4 | 1 | 1 | 1 | 1 | Họ vẫn bằng không | 
| Đặt hàng | 1 | 1 | 1 | 1 | Kiểm tra các hàng, sau đó cột 1 | 

Một đầu ra hợp lệ được tạo ra bởi thuật toán là```
YES
1000
0000
0000
0000
row 1
row 2
row 3
row 4
col 1
col 2
col 3
col 4
```Hàng 1 có 1 đèn và được kiểm tra trước nên an toàn. Tất cả các hàng và cột khác đều bị cô lập khi kiểm tra. 

Đối với mẫu 2,```
4 4
2 1 1 1
```chúng tôi nhận được`total = 5`,`positive = 4`, Và`needed = 2`. Cột 1 là trung tâm, trong khi cột 2 trở thành lá riêng cho hàng 1. 

| Bước | Hàng độ | Cạnh trung tâm | Cột riêng |`next_col`| Hành động | 
| --- | --- | --- | --- | --- | --- | 
| Hàng 1 | 2 | Có | Cột 2 | 2 | Đặt`1`ở cột 1 và 2 | 
| Hàng 2 | 1 | Có | Không có | 2 | Đặt`1`ở cột 1 | 
| Hàng 3 | 1 | Có | Không có | 2 | Đặt`1`ở cột 1 | 
| Hàng 4 | 1 | Có | Không có | 2 | Đặt`1`ở cột 1 | 
| Kiểm tra cột 2 | 1 | Có | Đã xóa | 2 | Cột 2 là an toàn | 
| Kiểm tra hàng 1 | 1 | Có | Đã xóa | 2 | Chỉ còn lại cột 1 | 
| Kiểm tra hàng 2 đến 4 | 1 | Có | Không có | 2 | Mỗi cái chỉ có cột 1 | 
| Kiểm tra cột 1 | 0 | Đã xóa | Không có | 2 | Tất cả các cạnh đã biến mất | 

Ma trận kết quả là```
1100
1000
1000
1000
```Thứ tự là```
col 2
row 1
row 2
row 3
row 4
col 1
col 3
col 4
```Đầu ra mẫu sử dụng ma trận và thứ tự hợp lệ khác. Vấn đề cho phép bất kỳ cách xây dựng hợp lệ nào, do đó việc tạo ra một cách xây dựng khác là hoàn toàn có thể chấp nhận được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |`O(nm)`| Ma trận có`nm`các ô và bản thân đầu ra có kích thước này trong trường hợp xấu nhất | 
| Không gian |`O(nm)`| Ma trận được xây dựng chiếm`nm`tế bào | 

Với`n, m <= 1000`, có nhiều nhất một triệu ô ma trận. Thuật toán chỉ chạm vào ma trận để xây dựng đầu ra được yêu cầu và công việc còn lại của nó là tuyến tính theo`n + m`. Điều này phù hợp thoải mái trong giới hạn 1 giây và 256 MB trong Python với tính năng nhỏ gọn`bytearray`kho. 

## Trường hợp thử nghiệm 

Bởi vì vấn đề cho phép nhiều ma trận hợp lệ và lệnh kiểm tra khác nhau nên một bộ khai thác kiểm tra mạnh mẽ sẽ xác thực cấu trúc được trả về thay vì so sánh mọi câu trả lời thành công với một đầu ra cố định. Hai mẫu được cung cấp có tính xác định cho việc triển khai bên dưới, do đó, kết quả đầu ra chính xác của chúng cũng có thể được kiểm tra.```python
import sys
import io

def solve_data(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

def validate(inp: str, out: str) -> bool:
    data = inp.split()
    n = int(data[0])
    m = int(data[1])
    a = list(map(int, data[2:2 + n]))

    lines = out.strip().splitlines()

    if lines[0] == "NO":
        # Verify impossibility using the necessary condition.
        total = sum(a)
        positive = sum(x > 0 for x in a)
        if positive == 0:
            return False
        return total - positive + 1 > m

    if lines[0] != "YES":
        return False

    if len(lines) != 1 + n + n + m:
        return False

    matrix = lines[1:1 + n]
    order = lines[1 + n:]

    for i in range(n):
        if len(matrix[i]) != m:
            return False
        if any(c not in "01" for c in matrix[i]):
            return False
        if matrix[i].count("1") != a[i]:
            return False

    used_rows = set()
    used_cols = set()

    current = [list(row) for row in matrix]

    for command in order:
        kind, x = command.split()
        x = int(x)

        if kind == "row":
            if not 1 <= x <= n or x in used_rows:
                return False

            remaining = sum(current[x - 1][j] == "1"
                            for j in range(m)
                            if j not in used_cols)

            if remaining > 1:
                return False

            used_rows.add(x)

        elif kind == "col":
            if not 1 <= x <= m or x in used_cols:
                return False

            remaining = sum(current[i][x - 1] == "1"
                            for i in range(n)
                            if i not in used_rows)

            if remaining > 1:
                return False

            used_cols.add(x)

        else:
            return False

    return len(used_rows) == n and len(used_cols) == m

# Provided samples
sample1 = """\
4 4
1 0 0 0
"""

sample2 = """\
4 4
2 1 1 1
"""

assert validate(sample1, solve_data(sample1)), "sample 1"
assert validate(sample2, solve_data(sample2)), "sample 2"

# Minimum-size all-zero instance
case1 = """\
1 1
0
"""
assert validate(case1, solve_data(case1)), "minimum all-zero case"

# Minimum-size all-one instance
case2 = """\
1 1
1
"""
assert validate(case2, solve_data(case2)), "minimum all-one case"

# Impossible boundary case: both rows require both columns
case3 = """\
2 2
2 2
"""
assert solve_data(case3).strip() == "NO", "cycle boundary case"

# Large all-equal case
case4 = "1000 1000\n" + " ".join(["1"] * 1000) + "\n"
assert validate(case4, solve_data(case4)), "large all-equal case"

# Exactly enough columns for a valid tree
case5 = """\
3 4
3 1 1
"""
assert validate(case5, solve_data(case5)), "exact column bound"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 0`|`YES`| Kích thước tối thiểu và kết cấu hoàn toàn bằng không | 
|`1 1 / 1`|`YES`| Kích thước tối thiểu có một cạnh | 
|`2 2 / 2 2`|`NO`| Phát hiện chu kỳ không thể tránh khỏi | 
|`1000 1000 / 1 ... 1`|`YES`| Kích thước tối đa và nhiều độ hàng bằng nhau | 
|`3 4 / 3 1 1`|`YES`| Ranh giới chính xác nơi`S-p+1 = m`| 

## Vỏ cạnh 

Đối với trường hợp toàn bằng không```
3 4
0 0 0
```chúng tôi có`positive = 0`. Nhánh đặc biệt xây dựng ba hàng`0000`và sau đó liệt kê tất cả các hàng và cột. Mỗi dây chuyền được kiểm tra đều không có đèn nào nên mọi hoạt động kiểm tra đều hợp lệ. Nhánh cũng cần thiết vì cấu trúc cột trung tâm chung được thiết kế xung quanh ít nhất một hàng dương. 

Đối với trường hợp khác không tối thiểu```
1 1
1
```chúng tôi có`total = 1`,`positive = 1`, Và`needed = 1`. Ma trận chỉ đơn giản là`1`. Không có cột riêng tư. Hàng được kiểm tra trước tiên với một đèn còn lại và cột duy nhất được kiểm tra sau đó với không đèn còn lại. 

Đối với trường hợp chu trình không thể```
2 2
2 2
```chúng tôi có`total = 4`,`positive = 2`, Vì thế`needed = 4 - 2 + 1 = 3`. Vì chỉ tồn tại hai cột nên việc xây dựng bị từ chối. Giới hạn dưới chứng tỏ rằng đây không chỉ là sự thất bại trong cách xây dựng cụ thể của chúng ta. Bất kỳ khu rừng nào nhận ra hai độ hàng sẽ yêu cầu ít nhất ba đỉnh cột, trong khi lưới chỉ có hai. 

Đối với trường hợp ràng buộc chính xác```
3 4
3 1 1
```chúng tôi có`total = 5`,`positive = 3`, Và`needed = 3`. Từ`3 <= 4`, trường hợp này là khả thi. Việc xây dựng sử dụng cột 1 làm trung tâm, cột 2 và 3 làm hai ô riêng cho hàng đầu tiên và cột 4 không được sử dụng. Ma trận trở thành```
1110
1000
1000
```Thứ tự bắt đầu với cột 2 và 3, sau đó là hàng 1, rồi đến hàng 2 và 3, tiếp theo là cột trung tâm và cột không sử dụng. Mỗi đỉnh không cô lập sẽ bị loại bỏ như một chiếc lá. 

Đối với trường hợp mức độ bằng nhau có kích thước tối đa```
1000 1000
1 1 1 ... 1
```có`1000`hàng dương và`total = 1000`, cho`needed = 1`. Tất cả các hàng có thể chia sẻ cùng một cột trung tâm. Do đó, ma trận có một cột duy nhất`1`s và 999 cột 0. Mỗi hàng có cấp một nên tất cả các hàng đều có thể được kiểm tra an toàn trước cột trung tâm. Điều này chứng tỏ tại sao chia sẻ một cột là ý tưởng cấu trúc quan trọng và tại sao số lượng cột bắt buộc phụ thuộc vào`total - positive + 1`, chứ không phải trên tổng số`1`S.
