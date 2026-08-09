---
title: "CF 102431I - Ông Panda và Khối"
description: "Có (n) màu sắc. Đối với mỗi cặp màu không có thứ tự ((i,j)), bao gồm cả cặp màu tự ghép ((i,i)), có chính xác một khối hình domino có hai khối lập phương đơn vị có các màu đó. Do đó, đầu vào không mô tả sự sắp xếp hiện có."
date: "2026-08-09T12:33:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102431
codeforces_index: "I"
codeforces_contest_name: "2019 China Collegiate Programming Contest Final (CCPC-Final 2019)"
rating: 0
weight: 102431
solve_time_s: 337
verified: true
draft: false
---

[CF 102431I - Ông Panda và Blocks](https://codeforces.com/problemset/problem/102431/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 5 phút 37 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Có (n) màu sắc. Đối với mỗi cặp màu không có thứ tự ((i,j)), bao gồm cả cặp màu tự ghép ((i,i)), có chính xác một khối hình domino có hai khối lập phương đơn vị có các màu đó. Do đó, đầu vào không mô tả sự sắp xếp hiện có. Nó chỉ cung cấp (n) và nhiệm vụ là xuất tọa độ cho mọi khối màu được yêu cầu. 

Hai khối được kết nối khi chúng có chung một mặt và kết nối có tính bắc cầu. Chúng ta cần toàn bộ tập hợp các hình khối để tạo thành một cấu trúc được kết nối. Đồng thời, nếu chúng ta loại bỏ mọi màu ngoại trừ một số màu cố định (c), thì tất cả các khối màu (c) vẫn phải tạo thành một cấu trúc được kết nối. 

Số khối là 

[ 
1+2+\cdots+n=\frac{n(n+1)}2. 
] 

Với (n\le 200), trường hợp thử nghiệm lớn nhất chứa (20100) khối và (40200) khối. Bản thân đầu ra đã có dạng bậc hai theo (n), do đó, cấu trúc (O(n^2)) là mục tiêu tự nhiên. Bất cứ điều gì đắt hơn đáng kể, chẳng hạn như kiểm tra vị trí (O(n^3)) hoặc quay lui theo cấp số nhân, đều không cần thiết và sẽ không phù hợp với giới hạn một giây. Các ràng buộc chính thức là (T\le10) và (n\le200), với giới hạn bộ nhớ 256 MB. 

Có một vài trường hợp nhỏ trong đó việc triển khai có thể âm thầm gặp trục trặc. Đối với (n=1), khối duy nhất là ((1,1)), vì vậy câu trả lời phải chứa hai khối màu 1 liền kề. Một cấu trúc chỉ xử lý các cặp có (i<j) sẽ không tạo ra kết quả gì và thất bại ngay lập tức. Ví dụ, đầu vào`1`phải sản xuất một`YES`câu trả lời theo sau là một khối như`1 1 1 1 1 1 2 1`. 

Các cặp tự cũng quan trọng đối với mọi (n) lớn hơn. Đối với (n=2), các khối bắt buộc là ((1,1),(1,2),(2,2)), không chỉ ((1,2)). Việc quên đường chéo chỉ cung cấp một bản sao của màu sẽ nhận được hai khối từ khối tự của nó, do đó đối số kết nối của nó không còn mô tả tập hợp các khối thực tế. 

Một sai lầm phổ biến khác là làm cho mỗi quân domino có giá trị riêng lẻ nhưng lại quên rằng các khối cùng màu phải kết nối giữa các lớp khác nhau. Ví dụ: đặt các khối ((1,2)) và ((1,3)) cách xa nhau có thể đáp ứng cả hai ràng buộc domino trong khi vẫn giữ hai khối màu-1 bị ngắt kết nối. Cấu trúc bên dưới tránh được điều đó bằng cách đặt tất cả sự xuất hiện trong tương lai của một màu cố định trên một đường thẳng đứng. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ coi đây là một tìm kiếm hình học. Sau khi chọn vị trí cho một khối, nó có thể thử các vị trí lân cận có thể có cho khối tiếp theo và quay lại bất cứ khi nào một màu bị ngắt kết nối hoặc một khối chồng lên một khối hiện có. Ngay cả khi chúng tôi bỏ qua phạm vi tọa độ không hạn chế và chỉ xem xét sáu hướng mặt cho mỗi khối mới được gắn, tìm kiếm theo độ sâu (m) có chuỗi vị trí lên tới (6^m), trong đó 

[ 
m=\frac{n(n+1)}2. 
] 

Tại (n=200), điều này có nghĩa là có tối đa (6^{20100}) ứng viên. Không có cách hữu ích nào để loại bỏ một tìm kiếm như vậy vì các ràng buộc mang tính tổng thể và đầu ra không yêu cầu phải giống với một hình dạng cụ thể. Ý tưởng brute-force đúng ở chỗ nó tìm kiếm tọa độ hợp lệ, nhưng nó tấn công sai phần của vấn đề. 

Quan sát hữu ích là cấu trúc cặp có hình tam giác. Khi chúng ta giới thiệu một màu (i), nó phải tạo thành các khối có màu (1,2,\ldots,i). Chúng ta có thể đặt tất cả các khối đó lên một lớp ngang mới (z=i). Khối lập phương thuộc màu cũ hơn (j) có thể được đặt tại 

[ 
(j,1,i), 
] 

trong khi khối lập phương có màu mới (i) được đặt ngay bên cạnh nó tại 

[ 
(j,2,i). 
] 

Đối với lớp cố định (i), các khối thứ hai có cùng màu (i) và tọa độ (x) của chúng là (1,2,\ldots,i). Do đó, chúng tạo thành một đường đi nằm ngang. 

Bây giờ hãy xem xét một màu cũ cố định (j). hình khối đầu tiên của nó xảy ra tại 

[ 
(j,1,j),(j,1,j+1),\ldots,(j,1,n). 
] 

Các khối này tạo thành một đường thẳng đứng vì các lớp liên tiếp chỉ khác nhau ở (z). Việc tự ghép ((j,j)) còn cho ra khối lập phương ((j,2,j)), nằm liền kề với ((j,1,j)). Các khối màu còn lại (j), cụ thể là ((1,2,j),\ldots,(j-1,2,j)), tạo thành một đường ngang dẫn đến cùng một khối tự ghép đó. 

Do đó, mọi màu sắc được kết nối bằng cấu trúc hình chữ L đơn giản. Mỗi cặp được yêu cầu cũng là một quân domino chính hãng vì hai tọa độ của nó khác nhau đúng một ở tọa độ (y). 

Ý tưởng phân lớp này loại bỏ hoàn toàn việc tìm kiếm. Chúng tôi chỉ cần liệt kê từng cặp và gán tọa độ của nó bằng cách sử dụng hai chỉ số màu của nó. Việc xây dựng chính xác là mẫu (j,i,j,1,i,j,2,i) được sử dụng trong giải pháp đã công bố của vấn đề. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(6^{n(n+1)/2})) ứng viên | Hàm mũ trong độ sâu tìm kiếm | Quá chậm | 
| Tối ưu | (O(n^2)) | (O(n^2)) cho đầu ra | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Xử lý màu sắc theo từng lớp. Với mọi (i) từ (1) đến (n), coi (z=i) là lớp chịu trách nhiệm cho tất cả các cặp có màu lớn hơn là (i). 
2. Trên lớp (i), liệt kê (j=1,2,\ldots,i). Điều này sẽ truy cập vào mọi cặp bắt buộc ((j,i)) chính xác một lần, bao gồm cả cặp đường chéo ((i,i)). 
3. Với cặp ((j,i)), đặt khối màu (j) tại ((j,1,i)) và khối màu (i) tại ((j,2,i)). Tọa độ của chúng chỉ khác nhau một trong tọa độ (y), do đó chúng có chung một mặt và tạo thành khối (1\times1\times2) cần thiết. 
4. Xem xét bất kỳ màu cố định nào (c). Sự xuất hiện của nó dưới dạng màu nhỏ hơn là các hình khối ((c,1,c),(c,1,c+1),\ldots,(c,1,n)). Các khối liên tiếp có tọa độ khác nhau một phần (z), vì vậy chúng tạo thành một đường thẳng đứng. 
5. Sự xuất hiện của nó dưới dạng màu lớn hơn là ((1,2,c),(2,2,c),\ldots,(c,2,c)). Các hình khối này tạo thành một đường nằm ngang vì các tọa độ liên tiếp khác nhau một phần (x). Khối cuối cùng ((c,2,c)) liền kề với ((c,1,c)), khối đầu tiên của đường thẳng đứng. Do đó tất cả các khối màu (c) thuộc về một thành phần được kết nối. 
6. Mỗi cặp màu riêng biệt đều có một khối ngăn cách giữa chúng. Vì mỗi màu riêng lẻ được kết nối bên trong nên các khối cặp này kết nối tất cả các thành phần màu thành một cấu trúc được kết nối toàn cầu. Do đó toàn bộ lâu đài được kết nối. 

### Tại sao nó hoạt động 

Bất biến trung tâm là sau khi xây dựng tất cả các lớp thông qua (i), mọi màu (c\le i) có tất cả các khối hiện được tạo của nó được kết nối. Khi một lớp (i) mới được thêm vào, mỗi màu cũ (j<i) sẽ nhận được một khối mới tại ((j,1,i)), ngay phía trên khối trước đó tại ((j,1,i-1)), do đó thành phần của nó vẫn được kết nối. Màu mới (i) nhận chuỗi ngang ((1,2,i),\ldots,(i,2,i)) và tự ghép nối của nó kết nối chuỗi đó với ((i,1,i)). Do đó, bất biến đúng cho mọi lớp. Vì mỗi cặp màu có một khối mà hai khối chạm nhau nên các thành phần màu được kết nối đều được liên kết với nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())

    out = []

    for case in range(1, t + 1):
        n = int(input())

        out.append(f"Case #{case}:")
        out.append("YES")

        # Layer i contains all pairs (j, i), 1 <= j <= i.
        for i in range(1, n + 1):
            for j in range(1, i + 1):
                # Color j at (j, 1, i)
                # Color i at (j, 2, i)
                out.append(
                    f"{j} {i} {j} 1 {i} {j} 2 {i}"
                )

    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Vòng ngoài chọn lớp và màu lớn hơn của cặp. Vòng lặp bên trong chọn màu nhỏ hơn, do đó cặp ((j,i)) được tạo chính xác khi (j\le i). Không có cách xử lý riêng cho việc tự ghép đôi vì (j=i) tạo ra khối đường chéo một cách tự nhiên. 

Các tọa độ có chủ ý sử dụng chỉ số màu làm tọa độ (x). Điều này làm cho tất cả các khối của một màu thẳng hàng theo chiều dọc khi màu đó xuất hiện dưới dạng điểm cuối nhỏ hơn. Tọa độ (y) 1 và 2 cố định làm cho mọi khối được tạo thành một cặp liền kề mặt hợp lệ. 

Tọa độ lớn nhất là (n), tối đa là 200, thấp hơn nhiều so với giới hạn trên bắt buộc của (10^9). Số nguyên Python cũng không có vấn đề tràn ở đây, mặc dù việc xây dựng không bao giờ cần số học vượt quá kích thước đầu vào. 

Danh sách đầu ra chứa (n(n+1)/2) mô tả khối cho mỗi trường hợp kiểm thử. Việc lưu trữ nó trong bộ nhớ vẫn còn nhỏ đối với (n=200), nhưng cấu trúc tương tự cũng có thể in trực tiếp. Việc sử dụng một bộ đệm đầu ra sẽ tránh thực hiện hàng nghìn lệnh gọi đầu ra riêng lẻ và thuận tiện cho việc I/O nhanh. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, (n=3), có sáu khối bắt buộc. Việc xây dựng tạo ra ba lớp. 

| Lớp (i) | (j) | Cặp | Khối màu (j) | Khối màu (i) | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | ((1,1)) | ((1,1,1)) | ((1,2,1)) | 
| 2 | 1 | ((1,2)) | ((1,1,2)) | ((1,2,2)) | 
| 2 | 2 | ((2,2)) | ((2,1,2)) | ((2,2,2)) | 
| 3 | 1 | ((1,3)) | ((1,1,3)) | ((1,2,3)) | 
| 3 | 2 | ((2,3)) | ((2,1,3)) | ((2,2,3)) | 
| 3 | 3 | ((3,3)) | ((3,1,3)) | ((3,2,3)) | 

Màu 1 có các hình khối tại ((1,1,1),(1,2,1),(1,1,2),(1,1,3),(1,2,2),(1,2,3)). Phần dọc kết nối tọa độ đầu tiên của nó ở mỗi lớp, trong khi các khối ngang lớp 1 và sau này kết nối thông qua tự ghép nối. Màu 2 hoạt động theo cách tương tự bắt đầu từ lớp 2 và màu 3 chỉ xuất hiện trên lớp 3. Mỗi lớp cũng chứa các khối liền kề của cặp tương ứng, vì vậy tất cả sáu khối đều hợp lệ. 

Đối với Mẫu 2, (n=4), có 10 khối. Trạng thái của các vòng lặp lồng nhau là: 

| Lớp (i) | Giá trị của (j) được tạo ra | Các cặp được tạo | 
| --- | --- | --- | 
| 1 | 1 | ((1,1)) | 
| 2 | 1, 2 | ((1,2),(2,2)) | 
| 3 | 1, 2, 3 | ((1,3),(2,3),(3,3)) | 
| 4 | 1, 2, 3, 4 | ((1,4),(2,4),(3,4),(4,4)) | 

Ví dụ: màu 2 xuất hiện tại ((2,1,2)), ((2,1,3)) và ((2,1,4)) là điểm cuối nhỏ hơn. Đây là những liền kề theo chiều dọc. Trên lớp 2, nó cũng xuất hiện tại ((1,2,2)) và ((2,2,2)), nằm liền kề theo chiều ngang và ((2,2,2)) chạm vào ((2,1,2)). Lập luận tương tự áp dụng cho tất cả bốn màu. 

Đầu ra mẫu trong câu lệnh sử dụng cách sắp xếp hợp lệ khác, điều này được mong đợi cho một vấn đề mang tính xây dựng. Thẩm phán kiểm tra các thuộc tính cần thiết thay vì yêu cầu các tọa độ chính xác đó. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n^2)) | Chính xác (n(n+1)/2) khối được tạo ra. | 
| Không gian | (O(n^2)) | Bộ đệm đầu ra lưu trữ (n(n+1)/2) dòng. | 

Đối với (n=200), chỉ có (20100) dòng khối được tạo ra cho mỗi trường hợp thử nghiệm. Trong tối đa mười trường hợp thử nghiệm, đây là khoảng (201000) dòng, do đó, cấu trúc bậc hai được căn chỉnh thoải mái với kích thước đầu ra và các ràng buộc của bài toán. Giới hạn thời gian chính thức là một giây và giới hạn bộ nhớ là 256 MB. 

## Trường hợp thử nghiệm 

Đối với một bài toán mang tính xây dựng, việc so sánh chuỗi đầu ra hoàn chỉnh với đầu ra mẫu không phải là phép kiểm tra hợp lý vì nhiều phép gán tọa độ khác nhau đều hợp lệ. Bộ khai thác thử nghiệm sau đây sẽ chạy công trình đã được gửi và xác minh các điều kiện hình học thực tế.```python
# helper: run solution on input string, return output string
import sys
import io
from collections import defaultdict, deque

def solution():
    input = sys.stdin.readline

    t = int(input())
    out = []

    for case in range(1, t + 1):
        n = int(input())

        out.append(f"Case #{case}:")
        out.append("YES")

        for i in range(1, n + 1):
            for j in range(1, i + 1):
                out.append(f"{j} {i} {j} 1 {i} {j} 2 {i}")

    sys.stdout.write("\n".join(out))

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solution()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

def adjacent(a, b):
    return (
        abs(a[0] - b[0])
        + abs(a[1] - b[1])
        + abs(a[2] - b[2])
        == 1
    )

def validate_output(inp: str, output: str):
    input_lines = inp.strip().splitlines()
    t = int(input_lines[0])
    ns = list(map(int, input_lines[1:]))

    lines = output.strip().splitlines()
    pos = 0

    for case in range(1, t + 1):
        n = ns[case - 1]
        m = n * (n + 1) // 2

        assert lines[pos] == f"Case #{case}:"
        pos += 1

        assert lines[pos] == "YES"
        pos += 1

        pairs = set()
        colors = defaultdict(list)
        used_coordinates = set()

        for _ in range(m):
            values = list(map(int, lines[pos].split()))
            pos += 1

            assert len(values) == 8

            i, j = values[0], values[1]
            a = tuple(values[2:5])
            b = tuple(values[5:8])

            assert 1 <= i <= j <= n
            assert (i, j) not in pairs
            pairs.add((i, j))

            assert all(0 <= x <= 10**9 for x in a)
            assert all(0 <= x <= 10**9 for x in b)

            assert adjacent(a, b)
            assert a != b

            assert a not in used_coordinates
            assert b not in used_coordinates
            used_coordinates.add(a)
            used_coordinates.add(b)

            colors[i].append(a)
            colors[j].append(b)

        assert len(pairs) == m

        expected_pairs = {
            (i, j)
            for i in range(1, n + 1)
            for j in range(i, n + 1)
        }
        assert pairs == expected_pairs

        # Verify that every color induces a connected set.
        for color in range(1, n + 1):
            cells = set(colors[color])
            assert len(cells) == n + 1

            start = next(iter(cells))
            q = deque([start])
            seen = {start}

            while q:
                x, y, z = q.popleft()

                for dx, dy, dz in (
                    (1, 0, 0),
                    (-1, 0, 0),
                    (0, 1, 0),
                    (0, -1, 0),
                    (0, 0, 1),
                    (0, 0, -1),
                ):
                    nxt = (x + dx, y + dy, z + dz)
                    if nxt in cells and nxt not in seen:
                        seen.add(nxt)
                        q.append(nxt)

            assert len(seen) == len(cells)

    assert pos == len(lines)

# Provided samples, validated structurally rather than compared
# against one particular valid construction.
sample_input = """2
3
4
"""
assert validate_output(sample_input, run(sample_input)) is None

# Minimum size: the only block is (1, 1), and its two cubes
# must be adjacent.
case_n1 = """1
1
"""
assert validate_output(case_n1, run(case_n1)) is None

# Smallest case containing both a diagonal pair and a distinct pair.
case_n2 = """1
2
"""
assert validate_output(case_n2, run(case_n2)) is None

# Near the upper boundary, useful for catching off-by-one errors
# in the nested loops.
case_n199 = """1
199
"""
assert validate_output(case_n199, run(case_n199)) is None

# Maximum allowed n.
case_n200 = """1
200
"""
assert validate_output(case_n200, run(case_n200)) is None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 3 4`|`YES`với sáu và mười khối hợp lệ | Cả hai đều cung cấp cỡ mẫu và cấu trúc chung | 
|`1 1`|`YES`với 1 khối tự liền kề | Vỏ có kích thước tối thiểu và xử lý theo đường chéo | 
|`1 2`|`YES`với ba khối hợp lệ | Cấu trúc cặp không cần thiết đầu tiên | 
|`1 199`|`YES`với 19900 khối hợp lệ | Hành vi vòng lặp giới hạn trên | 
|`1 200`|`YES`với 20100 khối hợp lệ | Hạn chế tối đa và tạo đầu ra | 

Trình xác nhận kiểm tra nhiều hơn định dạng văn bản được yêu cầu. Nó xác minh rằng mỗi cặp xảy ra chính xác một lần, rằng mỗi domino bao gồm các khối liền kề nhau, không có hai khối nào chiếm cùng tọa độ, các khối của mỗi màu được kết nối và số lượng khối của mỗi màu là chính xác. 

## Vỏ cạnh 

Đối với (n=1), vòng lặp bên ngoài chạy một lần với (i=1) và vòng lặp bên trong cũng chạy một lần với (j=1). Nó xuất ra khối ((1,1)) bằng cách sử dụng ((1,1,1)) và ((1,2,1)). Các hình khối này có chung một mặt và vì không có hình khối nào khác nên cả điều kiện kết nối theo màu sắc cụ thể và toàn bộ lâu đài đều được giữ nguyên. 

Với (n=2), các cặp được tạo là ((1,1),(1,2),(2,2)). Màu 1 có ((1,1,1)), ((1,2,1)), và ((1,1,2)), được kết nối thông qua tự khối và cạnh dọc. Màu 2 có ((1,2,2),(2,1,2),(2,2,2)), trong đó ((1,2,2)) kết nối với ((2,2,2)) và ((2,2,2)) kết nối với ((2,1,2)). Khối ((1,2)) sau đó nối hai thành phần màu. 

Với (n=200), lớp cuối cùng là (z=200) và chứa chính xác 200 khối, được lập chỉ mục bởi (j=1,\ldots,200). Đối với mỗi màu (j<200), khối lập phương mới của nó tại ((j,1,200)) nằm ngay phía trên ((j,1,199)). Đối với màu 200, tất cả 200 khối ở hàng thứ hai của lớp 200 tạo thành một đường dẫn ngang, với sự tự ghép nối đường dẫn đó với ((200,1,200)). Không có tọa độ nào tiếp cận ranh giới (10^9), do đó không có vấn đề về phạm vi hoặc tràn tọa độ. 

Trường hợp đường chéo (j=i) đáng được chú ý đặc biệt vì hai hình lập phương có cùng màu. Đầu ra vẫn chứa hai tọa độ riêng biệt, ((i,1,i)) và ((i,2,i)), vì vậy chúng là một quân domino thực sự chứ không phải là một khối có độ dài bằng 0 ngẫu nhiên. Đồng thời, khối tự đó chính là thứ kết nối phần ngang của cấu trúc màu (i) với phần dọc của nó. 

Nếu bạn muốn, tôi cũng có thể biến bài viết này thành một bài xã luận kiểu Codeforces nhỏ gọn hơn, phù hợp cho blog cuộc thi hoặc kho lưu trữ giải pháp.
