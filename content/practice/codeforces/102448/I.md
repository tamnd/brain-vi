---
title: "CF 102448I - Ivan và bể bơi"
description: "Chúng ta có một lưới (N lần M). Mỗi ô chứa độ sâu tối đa có thể được khai quật tại vị trí đó trước khi chạm vào đá. Chúng ta phải chọn chính xác (S) ô để tạo thành nhóm. Các ô đã chọn phải được kết nối thông qua các mặt được chia sẻ và nhóm có một độ sâu đồng đều."
date: "2026-08-09T14:31:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102448
codeforces_index: "I"
codeforces_contest_name: "UFPE Starters Final Try-Outs 2020"
rating: 0
weight: 102448
solve_time_s: 597
verified: true
draft: false
---

[CF 102448I - Ivan và bể bơi](https://codeforces.com/problemset/problem/102448/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 9 phút 57 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một lưới (N \times M). Mỗi ô chứa độ sâu tối đa có thể được khai quật tại vị trí đó trước khi chạm vào đá. Chúng ta phải chọn chính xác (S) ô để tạo thành nhóm. Các ô đã chọn phải được kết nối thông qua các mặt được chia sẻ và nhóm có một độ sâu đồng đều. 

Nếu ô được chọn chỉ cho phép đào đến độ sâu (7), toàn bộ bể không thể sâu hơn (7). Do đó, đối với bất kỳ tập hợp ô (S) nào được kết nối, độ sâu có thể đạt được của nó là độ sâu tối thiểu giữa các ô đó. Nhiệm vụ là tối đa hóa mức tối thiểu đó. 

Một cách hữu ích để suy nghĩ về độ sâu ứng viên (D) là loại bỏ mọi ô có giá trị nhỏ hơn (D). Các ô còn lại chính xác là những nơi có thể xây dựng một vùng sâu (D). Câu hỏi đặt ra là liệu các ô còn lại này có chứa thành phần được kết nối với ít nhất (S) ô hay không. 

Tổng số ô nhiều nhất là (10^6), do đó, thuật toán liên tục kiểm tra toàn bộ lưới để tìm nhiều độ sâu ứng cử viên là quá tốn kém. Có thể có (10^6) giá trị độ sâu khác nhau, thực hiện quét (O(NM)) cho mọi độ sâu có thể bằng thuật toán (O((NM)^2)), sẽ đạt được khoảng (10^{12}) kiểm tra ô trong trường hợp xấu nhất. Chúng ta cần thứ gì đó gần tuyến tính, ngoài chi phí sắp xếp các ô. 

Bản thân kích thước cũng có thể rất lớn. Lưới có (N=M=1000) đã chứa (10^6) ô và một trong hai chiều được phép lớn bằng (10^6). Điều này loại trừ các thuật toán tùy thuộc vào (N^2), (M^2) hoặc các lần duyệt toàn bộ lưới lặp lại. Giới hạn bộ nhớ cũng có nghĩa là nên tránh lưu trữ các đối tượng Python lớn cho mỗi ô khi có thể. 

Có một số trường hợp khó khăn mà giải pháp đúng đắn bề ngoài có thể thất bại. 

Đối với (S=1), không cần kết nối ngoài một ô. Ví dụ,```
1 1 1
42
```có câu trả lời (42). Một giải pháp luôn cố gắng kết nối một ô với một ô lân cận sẽ kết luận sai rằng không có nhóm nào tồn tại. 

Một trường hợp quan trọng khác là khi các ô có giá trị cao bị ngắt kết nối. Coi như```
2 1 3
10 1 10
```Câu trả lời là (1), không phải (10). Có hai ô có độ sâu (10), nhưng chúng được phân tách bằng ô độ sâu (1). Một giải pháp chỉ lấy (S) giá trị lớn nhất mà không xem xét khả năng kết nối sẽ trả về kết quả sai. 

Tình huống ngược lại cũng có vấn đề. Nếu một thành phần được kết nối chứa nhiều hơn (S) ô, chúng ta được phép sử dụng chính xác (S) trong số đó. Ví dụ,```
3 1 5
8 8 8 8 8
```có câu trả lời (8), mặc dù thành phần này chứa năm ô. Một giải pháp yêu cầu một thành phần phải có chính xác (S) ô sẽ từ chối thành phần đó một cách không chính xác. 

Cuối cùng, khi (S=N\cdot M), mọi ô phải thuộc về nhóm. Ví dụ,```
4 2 2
5 7
8 6
```có câu trả lời (5), vì độ sâu tối thiểu trên toàn bộ lưới là (5). Một phương thức dừng ngay khi tìm thấy một vùng lớn có giá trị cao có thể bỏ lỡ thực tế là nhóm phải chứa mọi ô khi (S) bằng toàn bộ kích thước lưới. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là thử mọi độ sâu có thể của bể bơi. Đối với độ sâu ứng cử viên (D), chúng tôi chỉ giữ lại các ô có giá trị ít nhất (D), chạy tràn trên các ô đó và kiểm tra xem liệu thành phần được kết nối nào có ít nhất (S) ô hay không. Điều này đúng vì mọi ô trong thành phần như vậy có thể hỗ trợ độ sâu (D) và bất kỳ thành phần được kết nối nào có ít nhất (S) ô đều chứa một tập hợp con được kết nối của các ô chính xác (S). 

Vấn đề là số lượng độ sâu ứng cử viên. Có thể có nhiều giá trị riêng biệt (NM). Trong trường hợp xấu nhất, một lần lấp lũ mất (O(NM)) thời gian, do đó, việc kiểm tra từng độ sâu riêng biệt sẽ mất (O((NM)^2)). Với (NM=10^6), đó có thể là (10^{12}) lượt truy cập ô. 

Chúng ta có thể cải thiện ý tưởng bằng cách xử lý độ sâu từ lớn nhất đến nhỏ nhất thay vì kiểm tra độc lập từng độ sâu. 

Giả sử chúng ta đã kích hoạt mọi ô có độ sâu ít nhất là (D). Tại thời điểm đó, các ô hiện hoạt tạo thành chính xác biểu đồ có liên quan đến nhóm độ sâu (D). Khi chúng tôi giảm ngưỡng, các ô mới được kích hoạt sẽ được thêm vào biểu đồ này. Khả năng kết nối chỉ phát triển nên chúng ta không bao giờ cần phải tính toán lại bất kỳ thành phần nào được kết nối từ đầu. 

Đây chính xác là mục đích mà cấu trúc Disjoint Set Union được thiết kế. Mỗi ô được kích hoạt bắt đầu như một thành phần riêng của nó. Bất cứ khi nào một ô được kích hoạt có một ô lân cận được kích hoạt, chúng tôi sẽ hợp nhất các thành phần của chúng. DSU lưu trữ kích thước của mọi thành phần, vì vậy sau khi kích hoạt một ô, chúng ta có thể kiểm tra ngay xem thành phần của nó đã đạt đến kích thước (S) hay chưa. 

Độ sâu đầu tiên mà tại đó một thành phần đạt đến kích thước (S) chính là câu trả lời. Xử lý theo thứ tự giảm dần có nghĩa là mọi ô được kích hoạt trước đó đều có độ sâu ít nhất bằng độ sâu hiện tại. Do đó, một thành phần đạt kích thước (S) ở độ sâu (D) sẽ cung cấp một nhóm độ sâu (D) hợp lệ, trong khi không có độ sâu nào lớn hơn có thể hỗ trợ các ô được kết nối (S) vì chúng tôi đã dừng trước đó. 

Có một chi tiết triển khai dành riêng cho Python đáng được xử lý cẩn thận. Lưới có thể chứa (10^6) ô, do đó việc lưu trữ các bộ dữ liệu như`(depth, row, column)`tạo ra chi phí bộ nhớ đáng kể. Thay vào đó, mỗi ô được biểu thị bằng một số nguyên chứa cả độ sâu và chỉ số ô được làm phẳng của nó. Vì (NM\le10^6<2^{20}), 20 bit thấp hơn là đủ cho chỉ mục. Các bit còn lại lưu trữ độ sâu. Sau đó, tính năng sắp xếp tích hợp của Python có thể sắp xếp các số nguyên được đóng gói này một cách hiệu quả. 

Mảng DSU được lưu trữ bằng cách sử dụng`array('i')`, sử dụng bốn byte cho mỗi mục nhập thay vì biểu diễn đối tượng trên mỗi số nguyên lớn hơn nhiều của Python. Các ô đang hoạt động được lưu trữ trong một`bytearray`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O((NM)^2)) | (O(NM)) | Quá chậm | 
| Tối ưu | (O(NM\log(NM))) | (O(NM)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Làm phẳng lưới hai chiều thành các chỉ số từ (0) đến (NM-1). Lưu trữ mỗi ô dưới dạng một số nguyên được đóng gói chứa chỉ số độ sâu và làm phẳng của nó. Sắp xếp các số nguyên này theo thứ tự giảm dần sẽ cho chúng ta các ô từ sâu nhất đến nông nhất. 
2. Tạo cấu trúc DSU với một thành phần riêng biệt ban đầu cho mỗi ô. Đồng thời tạo một`active`mảng. Một ô chỉ hoạt động sau khi nó được chạm tới trong quá trình quét giảm dần. 
3. Xử lý các ô theo thứ tự độ sâu giảm dần. Khi gặp một ô, hãy kích hoạt nó và làm cho kích thước thành phần của nó bằng một. 
4. Kiểm tra bốn hàng xóm lưới có thể có của ô. Đối với mọi hàng xóm đã hoạt động, hãy hợp nhất hai thành phần DSU. Chúng tôi chỉ hợp nhất các hàng xóm đang hoạt động vì hàng xóm không hoạt động có độ sâu nhỏ hơn ngưỡng hiện tại và không thể thuộc về nhóm có độ sâu hiện tại. 
5. Sau tất cả các kết hợp có thể có cho ô mới được kích hoạt, hãy kiểm tra kích thước của thành phần DSU của nó. Nếu kích thước đó ít nhất là (S), hãy trả về độ sâu của ô hiện tại. 
6. Tiếp tục cho đến khi một thành phần đạt đến ô (S). Thành phần như vậy bao gồm hoàn toàn các ô có độ sâu ít nhất bằng giá trị được báo cáo và được kết nối. Vì nó chứa ít nhất (S) ô nên chúng ta có thể chọn chính xác (S) ô được kết nối từ nó. 

Tại sao một thành phần được kết nối có nhiều hơn (S) ô luôn chứa một tập hợp con được kết nối gồm chính xác (S) ô? Bắt đầu từ bất kỳ đỉnh nào và liên tục thêm một đỉnh liền kề thuộc về thành phần. Bởi vì thành phần được kết nối, quá trình này có thể tiếp tục cho đến khi chính xác (S) đỉnh được chọn. 

Bất biến đằng sau toàn bộ quá trình quét là ngay trước khi xử lý độ sâu (D), mọi ô hoạt động đều có độ sâu ít nhất (D) và các thành phần DSU chính xác là các thành phần được kết nối của các ô hoạt động đó. Khi ô hiện tại được kích hoạt, chỉ cần thêm các cạnh của các ô lân cận đã hoạt động, do đó, bất biến vẫn đúng. Do đó, khi một thành phần lần đầu tiên tiếp cận các ô (S), nó đại diện cho một nhóm khả thi ở độ sâu đó. Vì độ sâu được xử lý từ lớn nhất đến nhỏ nhất nên độ sâu đầu tiên là lớn nhất. 

## Giải pháp Python```python
import sys
from array import array

input = sys.stdin.readline

SHIFT = 20
MASK = (1 << SHIFT) - 1

def solve():
    S, N, M = map(int, input().split())
    total = N * M

    # Pack (depth, index) into one integer.
    # The lower 20 bits store the index because total <= 10^6 < 2^20.
    cells = []
    append = cells.append

    idx = 0
    for _ in range(N):
        row = map(int, input().split())
        for depth in row:
            append((depth << SHIFT) | idx)
            idx += 1

    cells.sort(reverse=True)

    parent = array('i', range(total))
    size = array('i', [1]) * total
    active = bytearray(total)

    def find(x):
        while parent[x] != x:
            parent[x] = parent[parent[x]]
            x = parent[x]
        return x

    def union(a, b):
        a = find(a)
        b = find(b)

        if a == b:
            return a

        if size[a] < size[b]:
            a, b = b, a

        parent[b] = a
        size[a] += size[b]
        return a

    for code in cells:
        depth = code >> SHIFT
        idx = code & MASK

        active[idx] = 1
        root = idx

        row = idx // M
        col = idx - row * M

        if row > 0:
            other = idx - M
            if active[other]:
                root = union(root, other)

        if row + 1 < N:
            other = idx + M
            if active[other]:
                root = union(root, other)

        if col > 0:
            other = idx - 1
            if active[other]:
                root = union(root, other)

        if col + 1 < M:
            other = idx + 1
            if active[other]:
                root = union(root, other)

        root = find(root)

        if size[root] >= S:
            print(depth)
            return

if __name__ == "__main__":
    solve()
```Đầu vào được đọc từng hàng để chương trình không bao giờ tạo một danh sách khổng lồ gồm tất cả các mã thông báo đầu vào. Mỗi độ sâu ngay lập tức được đóng gói cùng với vị trí làm phẳng của nó và được gắn vào`cells`. 

biểu thức`(depth << SHIFT) | idx`lưu trữ hai mẩu thông tin trong một số nguyên. Vì số lượng ô lớn nhất có thể là (10^6), nên 20 bit là đủ cho`idx`. Việc sắp xếp các giá trị được đóng gói theo thứ tự giảm dần do đó chủ yếu sắp xếp theo độ sâu, đây chính xác là thứ tự được yêu cầu trong quá trình quét DSU. 

các`active`mảng byte tách biệt với DSU. Chỉ riêng giá trị gốc DSU không thể cho chúng ta biết liệu một ô đã được kích hoạt hay chưa, bởi vì mọi ô đều có giá trị gốc ngay từ đầu. Mảng byte cho phép chúng ta kiểm tra xem hàng xóm có thuộc biểu đồ ngưỡng hiện tại hay không. 

Lưới được làm phẳng theo từng hàng. Đối với chỉ mục`idx`, ô ở trên là`idx - M`, ô bên dưới là`idx + M`, ô bên trái là`idx - 1`, và ô bên phải là`idx + 1`. Việc kiểm tra hàng và cột ngăn chặn việc quấn quanh các cạnh. Đặc biệt, kiểm tra`col > 0`trước khi sử dụng`idx - 1`ngăn ô đầu tiên của hàng kết nối không chính xác với hàng trước đó. 

DSU sử dụng nén kết hợp theo kích thước và đường dẫn. Cả hai phép toán đều được khấu hao theo thời gian không đổi một cách hiệu quả, chính xác hơn là (O(\alpha(NM))), trong đó (\alpha) là hàm Ackermann nghịch đảo. 

Mã kiểm tra kích thước thành phần sau tất cả các kết hợp liên quan đến ô hiện tại. Thứ tự này quan trọng vì ô có thể kết nối một số thành phần riêng biệt trước đó và thành phần được hợp nhất cuối cùng là thành phần có kích thước phải được so sánh với (S). 

Số nguyên Python không bị tràn nên cách biểu diễn được đóng gói là an toàn. Giá trị được đóng gói lớn nhất nằm bên dưới (10^8\cdot2^{20}+2^{20}), dễ dàng nằm trong phạm vi số nguyên của Python. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
1 2 4
9 7 7 9
7 8 8 7
```Lưới phẳng là`[9, 7, 7, 9, 7, 8, 8, 7]`. Vì (S=1), ô được kích hoạt đầu tiên đã tạo thành thành phần hợp lệ. 

| Độ sâu kích hoạt | Chỉ số tế bào | Thành phần sau đoàn thể | Kích thước phù hợp lớn nhất | Quyết định | 
| --- | --- | --- | --- | --- | 
| 9 | 0 |`{0}`| 1 | Dừng lại | 

Ô đầu tiên có độ sâu (9), vì vậy câu trả lời là (9). 

Điều này thể hiện trường hợp ranh giới (S=1). Khả năng kết nối với các ô khác là không liên quan vì một ô đơn lẻ đã là một tập hợp được kết nối. 

### Mẫu 2 

Đầu vào là```
2 2 4
9 7 7 9
7 8 8 7
```Ở đây (S=2). Hai ô có độ sâu (9) là các chỉ số (0) và (3) và chúng không liền kề nhau. Hai ô có độ sâu (8) là các chỉ số (6) và (5) và chúng liền kề nhau. 

| Độ sâu kích hoạt | Chỉ số tế bào | Công đoàn mới | Kích thước thành phần chứa ô | Quyết định | 
| --- | --- | --- | --- | --- | 
| 9 | 3 | không | 1 | Tiếp tục | 
| 9 | 0 | không | 1 | Tiếp tục | 
| 8 | 6 | không | 1 | Tiếp tục | 
| 8 | 5 | (5\leftrightarrow6) | 2 | Dừng lại | 

Khi chỉ mục (5) được kích hoạt, nó liền kề với chỉ mục (6), do đó hai ô độ sâu-(8) tạo thành một thành phần có kích thước (2). Chúng tôi đã tìm thấy một nhóm hợp lệ ở độ sâu (8). 

Các ô độ sâu (9) không thể tạo ra một nhóm có kích thước (2), vì chúng thuộc về các thành phần khác nhau. Điều này chứng tỏ tại sao chỉ lấy các giá trị lớn nhất là không đủ và tại sao kết nối phải được duy trì một cách rõ ràng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(NM\log(NM))) | Các giá trị ô được đóng gói sắp xếp (NM) chiếm ưu thế; tất cả các hoạt động DSU gần như tuyến tính | 
| Không gian | (O(NM)) | Mỗi ô được đóng gói, mảng DSU và mảng hoạt động đều sử dụng không gian tuyến tính | 

Với tối đa (10^6) ô, thuật toán thực hiện một sắp xếp và số lượng thao tác DSU không đổi trên mỗi ô. Việc triển khai Python cũng tránh được việc sử dụng nhiều bộ nhớ trong Python và các số nguyên DSU trên mỗi ô bằng cách sử dụng các số nguyên được đóng gói và`array('i')`. Điều này giúp việc biểu diễn thoải mái trong giới hạn bộ nhớ 256 MB đồng thời tránh việc truyền tải toàn bộ lưới lặp đi lặp lại có thể khiến cách tiếp cận đơn giản trở nên quá chậm. 

## Trường hợp thử nghiệm```python
# The solution above is assumed to be available as solve().
# This helper redirects stdin/stdout so solve() can be tested directly.

import sys
import io
from contextlib import redirect_stdout

def run(inp: str) -> str:
    global input

    old_stdin = sys.stdin
    old_input = input

    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    out = io.StringIO()
    try:
        with redirect_stdout(out):
            solve()
    finally:
        sys.stdin = old_stdin
        input = old_input

    return out.getvalue().strip()

# Provided samples
assert run(
    """1 2 4
9 7 7 9
7 8 8 7
"""
) == "9", "sample 1"

assert run(
    """2 2 4
9 7 7 9
7 8 8 7
"""
) == "8", "sample 2"

assert run(
    """3 2 4
9 7 7 9
7 8 8 7
"""
) == "7", "sample 3"

# Minimum-size input
assert run(
    """1 1 1
42
"""
) == "42", "single cell"

# S = 1 must choose the deepest individual cell
assert run(
    """1 2 3
5 1 9
1 1 1
"""
) == "9", "S=1"

# High cells exist, but they are disconnected.
assert run(
    """2 1 3
10 1 10
"""
) == "1", "disconnected high cells"

# All cells have the same depth, and S equals the entire grid.
assert run(
    """6 2 3
7 7 7
7 7 7
"""
) == "7", "all equal and S=NM"

# Maximum number of cells: 1 x 1,000,000.
# Every cell has the same depth, so the whole row is one component.
max_case = "1000000 1 1000000\n" + ("7 " * 1000000)
assert run(max_case) == "7", "maximum-size input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1 / 42`| 42 | Kích thước lưới tối thiểu và nhóm ô đơn | 
|`1 2 3 / 5 1 9 / 1 1 1`| 9 | (S=1) nên không cần kết nối | 
|`2 1 3 / 10 1 10`| 1 | Ngăn chặn việc xử lý các giá trị lớn nhất (S) dưới dạng được kết nối tự động | 
|`6 2 3 / all 7`| 7 | Tất cả các giá trị bằng nhau và (S=N\cdot M) | 
|`1000000 1 1000000 / all 7`| 7 | Tổng số ô tối đa và đầu vào tỷ lệ ranh giới | 

## Vỏ cạnh 

Trường hợp ô đơn được xử lý ngay khi ô đầu tiên được kích hoạt. Vì```
1 1 1
42
```ô có độ sâu (42), trở thành thành phần hoạt động có kích thước (1) và vì (S=1), thuật toán trả về (42). Không cần hàng xóm. 

Đối với các ô có giá trị cao bị ngắt kết nối, hãy xem xét```
2 1 3
10 1 10
```Hai ô độ sâu (10) được kích hoạt trước, nhưng không có ô lân cận đang hoạt động nào, vì vậy cả hai thành phần đều giữ nguyên kích thước (1). Sau đó, ô ở giữa có độ sâu (1) được kích hoạt và nối cả hai bên, tạo ra một thành phần có kích thước (3). Tại thời điểm đó thành phần có kích thước ít nhất là (S=2), vì vậy câu trả lời là (1). Thuật toán loại bỏ chính xác cặp ô có độ sâu-(10) bị cô lập hấp dẫn nhưng không hợp lệ. 

Khi một thành phần lớn hơn (S), thuật toán sẽ cố tình kiểm tra`>= S`còn hơn là`== S`. Vì```
3 1 5
8 8 8 8 8
```ba ô liền kề đầu tiên đã tạo thành một thành phần được kết nối gồm chính xác ba ô, do đó thuật toán trả về (8). Nếu nhiều ô hơn đã được kích hoạt trước khi kiểm tra, thì thành phần có kích thước bốn hoặc năm vẫn hợp lệ vì chúng ta có thể lấy một tập hợp con được kết nối gồm chính xác ba ô. 

Khi (S=N\cdot M), ngưỡng không thể được chấp nhận cho đến khi mọi ô được kích hoạt. Vì```
4 2 2
5 7
8 6
```các ô có độ sâu (8), (7) và (6) được kích hoạt trước, nhưng thành phần của chúng chỉ có kích thước ba. Ô có độ sâu-(5) cuối cùng kết hợp với thành phần đó, tạo nên kích thước của nó là bốn. Thuật toán sau đó trả về (5), chính xác là độ sâu tối thiểu của toàn bộ lưới. 

Việc kiểm tra ranh giới lưới cũng rất cần thiết. Trong biểu diễn hàng chính, ô cuối cùng của một hàng liền kề về mặt số với ô đầu tiên của hàng tiếp theo, nhưng các ô đó không liền kề theo chiều ngang trong lưới. Các điều kiện trên`col`ngăn chặn những kết nối sai lầm như vậy. Tương tự,`row > 0`Và`row + 1 < N`ngăn chặn truy cập theo chiều dọc bên ngoài lưới điện. 

Thứ tự giảm dần là yếu tố làm cho thành phần DSU thành công đầu tiên trở nên tối ưu. Ở độ sâu (D), mọi ô hoạt động đều có dung lượng ít nhất (D). Nếu một thành phần đạt đến (S), sẽ tồn tại một vùng sâu (D). Bất kỳ độ sâu lớn hơn nào đã được xử lý trước đó và không thể tạo ra thành phần như vậy, vì vậy không có câu trả lời nào tốt hơn bị bỏ qua. 

Phiên bản này đã sẵn sàng để sử dụng làm bài xã luận cho cuộc thi. Nếu bạn muốn, tôi cũng có thể tạo một phiên bản kiểu Codeforces ngắn hơn tập trung vào hiểu biết sâu sắc và triển khai DSU cốt lõi.
