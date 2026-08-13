---
title: "CF 102284I - OpenStreetMap"
description: "Chúng tôi có bản đồ chiều cao (n lần m). Trình duyệt có thể hiển thị bất kỳ hình chữ nhật nào chứa chính xác (a) các hàng liên tiếp và (b) các cột liên tiếp. Đối với mọi vị trí có thể có của hình chữ nhật đó, chúng ta cần chiều cao tối thiểu của nó và cuối cùng chúng ta cần tổng của tất cả các điểm tối thiểu đó."
date: "2026-08-13T08:53:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102284
codeforces_index: "I"
codeforces_contest_name: "\u041b\u041a\u0428 2019, \u0418\u044e\u043b\u044c, \u041c\u0438\u043a\u0441 \u0441\u0442\u0430\u0440\u0448\u0435\u0439 \u0438 \u043c\u043b\u0430\u0434\u0448\u0435\u0439 \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434"
rating: 0
weight: 102284
solve_time_s: 134
verified: true
draft: false
---

[CF 102284I - OpenStreetMap](https://codeforces.com/problemset/problem/102284/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 14s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có bản đồ chiều cao (n \times m). Trình duyệt có thể hiển thị bất kỳ hình chữ nhật nào chứa chính xác (a) các hàng liên tiếp và (b) các cột liên tiếp. Đối với mọi vị trí có thể có của hình chữ nhật đó, chúng ta cần chiều cao tối thiểu của nó và cuối cùng chúng ta cần tổng của tất cả các điểm tối thiểu đó. 

Chiều cao không được đưa ra một cách rõ ràng. Thay vào đó, chúng được tạo ra bởi sự tái phát 

[ 
g_i=(g_{i-1}x+y)\bmod z, 
] 

và ma trận được điền từng hàng từ chuỗi này. Do đó, ô ((r,c)) chứa phần tử chuỗi có chỉ số (rm+c) khi sử dụng tọa độ dựa trên 0. 

Các ràng buộc cho phép cả hai chiều đạt tới (3000), do đó ma trận có thể chứa (9) triệu ô. Một thuật toán kiểm tra từng ô của mọi hình chữ nhật (a\times b) là quá đắt. Ngay cả khi (a=b=1500), vẫn có khoảng (2,25) triệu hình chữ nhật có thể có, mỗi hình chữ nhật chứa (2,25) triệu ô, cho kết quả so sánh khoảng (5\times10^{12}). Giải pháp phải xử lý mỗi ô ma trận chỉ với một số lần không đổi. 

Ngoài ra còn có hai cân nhắc về số lượng. Chiều cao được tạo thấp hơn (10^9), nhưng câu trả lời có thể chứa gần (9) triệu giá trị như vậy, vì vậy câu trả lời có thể đạt tới khoảng (9\times10^{15}). Số nguyên Python tự động xử lý việc này, trong khi biểu diễn 32 bit nhỏ gọn là đủ cho độ cao được lưu trữ vì mọi chiều cao đều dưới (10^9). 

Trường hợp cạnh nhỏ là màn hình một ô. Vì```
1 1 1 1
5 0 0 10
```hình chữ nhật được hiển thị duy nhất có chiều cao (5), vì vậy câu trả lời là (5). Một giải pháp giả sử một cửa sổ có ít nhất hai ô hoặc trì hoãn việc ghi giá trị cực tiểu cho đến khi vị trí thứ hai xuất hiện, có thể vô tình tạo ra số 0. 

Một trường hợp ranh giới khác xảy ra khi cửa sổ có chiều rộng tối đa. Vì```
2 3 1 3
1 1 0 100
```ma trận được tạo ra là 

[ 
\bắt đầu{pmatrix} 
1&1&1\ 
1&1&1 
\end{pmatrix}, 
] 

vậy có đúng hai hình chữ nhật được hiển thị và đáp án là (2). Việc triển khai cửa sổ trượt ngang bắt đầu tạo ra câu trả lời ở cột (b) nhưng sử dụng chỉ mục đầu ra sai có thể làm mất cửa sổ đầu tiên. 

Vấn đề tương tự xảy ra theo chiều dọc khi (a=n). Vì```
3 1 3 1
4 1 0 100
```mỗi ô là (4), chỉ có một hình chữ nhật (3\times1) và đáp án là (4). Hàng đợi dọc phải chứa chính xác các hàng (a) hiện tại, không phải (a+1). 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là liệt kê mọi góc trên bên trái có thể có và quét tất cả (a b) ô bên trong hình chữ nhật của nó. Có ((n-a+1)(m-b+1)) hình chữ nhật, nên thời gian chạy của nó là 

[ 
O((n-a+1)(m-b+1)ab), 
] 

đó là (O(n^2m^2)) trong trường hợp xấu nhất. Với (n=m=3000), điều này hoàn toàn không thể thực hiện được. 

Quan sát hữu ích đầu tiên là mức tối thiểu hai chiều có thể được tách thành hai phép toán tối thiểu một chiều. Hãy xem xét một hàng cố định. Nếu chúng ta biết giá trị nhỏ nhất của mỗi đoạn có độ dài-(b) trong hàng đó thì hình chữ nhật (a\times b) có thể được giảm xuống thành một cửa sổ dọc của (a) giá trị cực tiểu của hàng đó. Việc lấy mức tối thiểu của các giá trị (a) đó sẽ cho ra chính xác mức tối thiểu của toàn bộ hình chữ nhật. 

Câu hỏi còn lại là làm thế nào để tính toán tất cả các cực tiểu của cửa sổ trượt một chiều một cách hiệu quả. Hàng đợi đơn điệu giữ các vị trí ứng cử viên theo thứ tự giá trị tăng dần. Khi một giá trị mới được chèn vào, mọi giá trị lớn hơn đằng sau nó có thể bị loại bỏ vì giá trị mới vừa nhỏ hơn vừa mới hơn, do đó giá trị bị loại bỏ không bao giờ có thể trở thành giá trị tối thiểu của một khoảng thời gian trong tương lai trước khi giá trị mới hết hạn. Do đó, mặt trước của hàng đợi là mức tối thiểu của cửa sổ hiện tại. 

Chúng tôi áp dụng điều này một lần theo chiều ngang và một lần theo chiều dọc. Đường ngang tạo ra (n(m-b+1)) cực tiểu trung gian. Đường chuyền dọc sử dụng các giá trị đó và thêm trực tiếp mọi cửa sổ phần tử (a) đã hoàn thành vào câu trả lời. 

Việc triển khai C++ tiêu chuẩn có thể lưu trữ ma trận trung gian dưới dạng số nguyên. Trong Python, việc lưu trữ hàng triệu giá trị dưới dạng số nguyên thông thường sẽ sử dụng nhiều bộ nhớ hơn, do đó cách triển khai bên dưới sẽ sử dụng`array('I')`, một mảng số nguyên không dấu 32 bit nhỏ gọn. Điều này giữ cho ma trận trung gian ở mức khoảng 36 MB trong trường hợp xấu nhất trong khi vẫn giữ nguyên thuật toán (O(nm)). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O((n-a+1)(m-b+1)ab)) | (O(1)) bên cạnh việc tạo đầu vào | Quá chậm | 
| Hai đường chuyền đơn điệu | (O(nm)) | (O(n(m-b+1))) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo từng hàng ma trận từ phép lặp. Chúng ta không bao giờ cần toàn bộ ma trận gốc cùng một lúc, vì mỗi hàng có thể ngay lập tức được giảm xuống mức cực tiểu cửa sổ ngang của nó. 
2. Đối với mỗi hàng, hãy duy trì một hàng chỉ số cột tăng dần đơn điệu. Trước khi chèn cột (c), hãy xóa các chỉ mục nằm ngoài cửa sổ độ dài-(b) hiện tại. Sau đó xóa các chỉ số ở phía sau trong khi chiều cao của chúng ít nhất bằng chiều cao mới. Mặt trước bây giờ xác định mức tối thiểu của cửa sổ ngang hiện tại. 
3. Lưu trữ cực tiểu theo chiều ngang trong một mảng nhỏ gọn. Đối với hàng (r), giá trị ở vị trí nằm ngang (c) biểu thị giá trị tối thiểu của các ô ban đầu từ cột (c) đến (c+b-1). Có (m-b+1) các giá trị như vậy trên mỗi hàng. 
4. Xử lý từng cột của mảng trung gian bằng một hàng đợi đơn điệu khác. Hàng đợi hiện chứa chỉ mục hàng thay vì chỉ mục cột. Khi hàng (r) xuất hiện, hãy xóa các hàng cũ hơn (a), loại bỏ các giá trị lớn hơn ở phía sau và giữ ứng cử viên nhỏ nhất ở phía trước. 
5. Khi (a) hàng đã được xử lý, giá trị ở đầu hàng đợi là giá trị nhỏ nhất của hình chữ nhật (a\times b) tương ứng. Thêm nó ngay vào câu trả lời. Không cần lưu trữ ma trận cuối cùng của hình chữ nhật cực tiểu. 
6. In câu trả lời tích lũy. Kiểu số nguyên của Python được sử dụng cho biến này vì tổng có thể lớn hơn nhiều so với (2^{32}). 

### Tại sao nó hoạt động 

Đối với mỗi hàng, hàng đợi ngang duy trì chính xác mức tối thiểu của khoảng ô (b) hiện tại ở phía trước của nó. Do đó, mỗi giá trị trung gian là giá trị tối thiểu của một dải ngang hoàn chỉnh của hình chữ nhật cuối cùng. Đối với một cột cố định chứa các giá trị trung gian này, mức tối thiểu của (a) dải ngang liên tiếp bằng mức tối thiểu của tất cả các ô trong hình chữ nhật (a\times b) tương ứng. Hàng đợi đơn điệu thứ hai duy trì chính xác mức tối thiểu theo chiều dọc đó, vì vậy mỗi hình chữ nhật đóng góp mức tối thiểu thực sự của nó một lần và chỉ một lần cho câu trả lời. 

Bất biến của hàng đợi là các chỉ số của nó nằm bên trong cửa sổ hiện tại và các giá trị tương ứng của chúng không giảm. Việc xóa các giá trị lớn hơn ở phía sau không thể làm mất giá trị tối thiểu trong tương lai vì giá trị mới được chèn nhỏ hơn và không hết hạn sớm hơn. Việc loại bỏ các phần tử phía trước đã hết hạn là an toàn vì chúng không còn thuộc về cửa sổ hiện tại nữa. Do đó mặt trước luôn đại diện cho mức tối thiểu cần thiết. 

## Giải pháp Python```python
import sys
from array import array

input = sys.stdin.readline

def solve():
    n, m, a, b = map(int, input().split())
    g, x, y, z = map(int, input().split())

    w = m - b + 1

    # Horizontal minima. Heights are < 1e9, so unsigned 32-bit
    # integers are sufficient. Preallocating avoids the memory
    # overhead of millions of Python int objects.
    horizontal = array('I', [0]) * (n * w)

    # Monotonic queue of column indices for one row.
    q = [0] * m

    for r in range(n):
        row = [0] * m
        for c in range(m):
            row[c] = g
            g = (g * x + y) % z

        head = 0
        tail = 0
        base = r * w

        for c in range(m):
            # Remove columns that are left of the current
            # length-b window.
            while head < tail and q[head] <= c - b:
                head += 1

            # Keep values in increasing order.
            value = row[c]
            while head < tail and row[q[tail - 1]] >= value:
                tail -= 1

            q[tail] = c
            tail += 1

            if c >= b - 1:
                horizontal[base + c - b + 1] = row[q[head]]

    # Vertical pass. The queue contains row indices.
    q = [0] * n
    answer = 0

    for c in range(w):
        head = 0
        tail = 0

        for r in range(n):
            pos = r * w + c
            value = horizontal[pos]

            # Remove rows outside the current length-a window.
            while head < tail and q[head] <= r - a:
                head += 1

            # Remove values that cannot become a future minimum.
            while head < tail:
                back_row = q[tail - 1]
                back_value = horizontal[back_row * w + c]
                if back_value < value:
                    break
                tail -= 1

            q[tail] = r
            tail += 1

            if r >= a - 1:
                answer += horizontal[q[head] * w + c]

    print(answer)

if __name__ == "__main__":
    solve()
```Phần đầu tiên đọc các tham số của trình tạo và tính toán`w`, số lượng cửa sổ ngang ở mỗi hàng. Từ`w = m-b+1`, mảng trung gian chứa chính xác các giá trị mà lượt thứ hai cần. 

các`horizontal`mảng sử dụng mã loại 32-bit không dấu`I`. Mọi chiều cao được tạo đều nằm trong phạm vi từ (0) đến (z-1) và (z\le10^9), do đó không có chiều cao được lưu trữ nào có thể vượt qua biểu diễn này. Mảng nhỏ gọn rất hữu ích trong Python vì một danh sách thông thường lên tới chín triệu số nguyên Python sẽ tiêu tốn nhiều bộ nhớ hơn đáng kể. 

Đối với mỗi hàng,`q`là một mảng được sử dụng như một deque được triển khai thủ công.`head`trỏ đến phần tử hoạt động đầu tiên và`tail`trỏ một vị trí sau phần tử hoạt động cuối cùng. Sử dụng chỉ số thay vì gọi liên tục`popleft`tránh tạo nhiều đối tượng Python và giữ cho các vòng lặp nóng đơn giản. 

Điều kiện hết hạn`q[head] <= c-b`tương đương với việc nói rằng chỉ số ít nhất phải là`c-b+1`, là điểm cuối bên trái của cửa sổ hiện tại. Câu trả lời theo chiều ngang đầu tiên xuất hiện khi`c == b-1`, bởi vì đó là cột đầu tiên tồn tại khoảng phần tử (b) đầy đủ. 

Thẻ dọc sử dụng cùng một ý tưởng hàng đợi, nhưng hàng đợi lưu trữ các chỉ mục hàng. điều kiện`q[head] <= r-a`xóa các hàng có nhiều hơn (a-1) vị trí phía sau hàng hiện tại. Một cửa sổ dọc hoàn chỉnh lần đầu tiên tồn tại khi`r == a-1`. 

Quá trình cập nhật trình tạo diễn ra ngay sau khi gán ô hiện tại. Điều này phù hợp với định nghĩa của chuỗi: ô đầu tiên nhận (g_0) và ô tiếp theo nhận giá trị được tạo bởi một bước lặp lại. 

phép nhân`g * x`có thể đạt tới gần (10^{18}), nằm trong phạm vi số nguyên có độ chính xác tùy ý của Python. Câu trả lời cuối cùng cũng có thể đạt khoảng (9\time10^{15}), do đó không cần xử lý tràn rõ ràng. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
3 4 2 1
1 2 3 59
```Ma trận được tạo ra là 

[ 
\bắt đầu{pmatrix} 
1&5&13&29\ 
2&7&17&37\ 
18&39&22&47 
\end{pmatrix}. 
] 

Bởi vì (b=1), mỗi cửa sổ ngang chứa một ô, do đó, giá trị ngang không thay đổi. 

| Hàng | Giá trị được tạo | Cực tiểu ngang | 
| --- | --- | --- | 
| 0 | 1, 5, 13, 29 | 1, 5, 13, 29 | 
| 1 | 2, 7, 17, 37 | 2, 7, 17, 37 | 
| 2 | 18, 39, 22, 47 | 18, 39, 22, 47 | 

Bây giờ (a=2), vậy các cửa sổ dọc chứa hai hàng liên tiếp. 

| Cột | Hàng tối thiểu 0-1 | Hàng tối thiểu 1-2 | Đóng góp | 
| --- | --- | --- | --- | 
| 0 | 1 | 2 | 3 | 
| 1 | 5 | 7 | 12 | 
| 2 | 13 | 17 | 30 | 
| 3 | 29 | 37 | 66 | 

Câu trả lời tích lũy là 

[ 
3+12+30+66=111. 
] 

Dấu vết này chứng tỏ rằng bước thứ hai hoạt động trên các cực tiểu của hàng thay vì các ô ban đầu, nhưng vẫn đạt được mức tối thiểu hình chữ nhật chính xác. 

### Ví dụ tùy chỉnh 

Hãy xem xét```
2 3 2 2
1 1 0 100
```Dãy số là (1,1,1,1,1,1), nên ma trận là tất cả một. Chỉ có một (2\times2) hình chữ nhật theo chiều dọc và hai vị trí nằm ngang. 

| Hàng | Cửa sổ ngang | Tối thiểu | 
| --- | --- | --- | 
| 0 | cột 0-1 | 1 | 
| 0 | cột 1-2 | 1 | 
| 1 | cột 0-1 | 1 | 
| 1 | cột 1-2 | 1 | 

Đường dọc xử lý từng cột trung gian riêng biệt. 

| Cột trung gian | Hàng 0-1 | Hình chữ nhật tối thiểu | Trả lời sau cột | 
| --- | --- | --- | --- | 
| 0 | 1, 1 | 1 | 1 | 
| 1 | 1, 1 | 1 | 2 | 

Câu trả lời là (2), khớp hai hình chữ nhật có thể có (2\times2). Ví dụ này thực hiện cả hai chiều của thao tác cửa sổ trượt và cũng kiểm tra các giá trị bằng nhau, trong đó hàng đợi phải duy trì chính xác khi xảy ra sự trùng lặp về chiều cao. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(nm)) | Mỗi ô ma trận vào và rời khỏi hàng đợi ngang nhiều nhất một lần và mọi giá trị trung gian vào và rời khỏi hàng đợi dọc nhiều nhất một lần. | 
| Không gian | (O(n(m-b+1)+m+n)) | Cực tiểu theo chiều ngang yêu cầu một giá trị thu gọn trên mỗi hàng và cửa sổ ngang, trong khi hai hàng đợi và hàng hiện tại chỉ yêu cầu không gian phụ trợ tuyến tính. | 

Với (n,m\le3000), có nhiều nhất (9) triệu ô được tạo. Thuật toán thực hiện một lượng công việc hàng đợi phân bổ không đổi trên mỗi ô, thay vì quét mọi ô hình chữ nhật (a\times b). Mảng trung gian nhỏ gọn duy trì việc triển khai Python trong giới hạn bộ nhớ 256 MB, mặc dù giới hạn Codeforces ban đầu đủ chặt chẽ để việc triển khai được biên dịch có biên độ hiệu suất lớn hơn nhiều. 

## Trường hợp thử nghiệm```python
# The solution function from above is assumed to be present.
# For local testing, run() temporarily replaces stdin and captures stdout.

import sys
import io
from array import array

def solve():
    input = sys.stdin.readline

    n, m, a, b = map(int, input().split())
    g, x, y, z = map(int, input().split())

    w = m - b + 1
    horizontal = array('I', [0]) * (n * w)

    q = [0] * m

    for r in range(n):
        row = [0] * m

        for c in range(m):
            row[c] = g
            g = (g * x + y) % z

        head = 0
        tail = 0
        base = r * w

        for c in range(m):
            while head < tail and q[head] <= c - b:
                head += 1

            value = row[c]
            while head < tail and row[q[tail - 1]] >= value:
                tail -= 1

            q[tail] = c
            tail += 1

            if c >= b - 1:
                horizontal[base + c - b + 1] = row[q[head]]

    q = [0] * n
    answer = 0

    for c in range(w):
        head = 0
        tail = 0

        for r in range(n):
            value = horizontal[r * w + c]

            while head < tail and q[head] <= r - a:
                head += 1

            while head < tail:
                br = q[tail - 1]
                if horizontal[br * w + c] < value:
                    break
                tail -= 1

            q[tail] = r
            tail += 1

            if r >= a - 1:
                answer += horizontal[q[head] * w + c]

    print(answer)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample
assert run(
    "3 4 2 1\n"
    "1 2 3 59\n"
) == "111", "sample 1"

# Minimum-size input
assert run(
    "1 1 1 1\n"
    "5 0 0 10\n"
) == "5", "single cell"

# All values equal, with several rectangles
assert run(
    "2 3 2 2\n"
    "7 1 0 10\n"
) == "14", "all equal values"

# Horizontal windows, catches the first and last horizontal positions
assert run(
    "2 3 1 2\n"
    "1 2 1 100\n"
) == "50", "horizontal boundaries"

# Vertical window spanning the complete height
assert run(
    "3 1 3 1\n"
    "4 1 0 100\n"
) == "4", "full-height window"

# Maximum-size dimensions with a full matrix window.
# All generated values are zero, so the only 3000x3000
# rectangle has minimum zero.
assert run(
    "3000 3000 3000 3000\n"
    "0 0 0 1\n"
) == "0", "maximum-size input"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 1 1 / 5 0 0 10`| 5 | Kích thước tối thiểu và cửa sổ một ô | 
|`2 3 2 2 / 7 1 0 10`| 14 | Giá trị bằng nhau và nhiều cửa sổ hai chiều | 
|`2 3 1 2 / 1 2 1 100`| 50 | Ranh giới cửa sổ ngang | 
|`3 1 3 1 / 4 1 0 100`| 4 | Ranh giới dọc trong đó (a=n) | 
|`3000 3000 3000 3000 / 0 0 0 1`| 0 | Kích thước ma trận tối đa và biểu diễn bộ nhớ nhỏ gọn | 

## Vỏ cạnh 

### Ma trận một ô 

cho```
1 1 1 1
5 0 0 10
```cột xử lý vượt qua ngang (0), ngay lập tức tạo ra mức tối thiểu theo chiều ngang duy nhất (5) và hàng xử lý vượt qua dọc (0), thêm ngay (5) vì (r=a-1=0). Câu trả lời cuối cùng là (5). Không cần trường hợp đặc biệt nào trong quá trình triển khai vì điều kiện hoàn thành cửa sổ tương tự sẽ hoạt động khi kích thước cửa sổ là một. 

### Một cửa sổ có chiều rộng đầy đủ 

cho```
2 3 1 3
1 1 0 100
```các giá trị được tạo đều là (1). Vì (b=3), hàng đợi ngang không tạo ra giá trị trung gian cho đến cột (2). Tại thời điểm đó, cửa sổ ngang duy nhất có mức tối thiểu (1). Đường dọc có (a=1), vì vậy mỗi hàng trong số hai hàng đóng góp một giá trị. Kết quả là (1+1=2). 

biểu hiện`c >= b - 1`là điều làm cho cửa sổ ngang hoàn chỉnh đầu tiên xuất hiện chính xác ở ranh giới bên phải. 

### Một cửa sổ có chiều cao đầy đủ 

cho```
3 1 3 1
4 1 0 100
```cả ba giá trị được tạo ra là (4). Đường chuyền ngang tạo ra một giá trị trên mỗi hàng. Trong quá trình di chuyển theo chiều dọc, các hàng (0) và (1) chưa tạo thành một cửa sổ (a=3) hoàn chỉnh. Tại hàng (2), hàng đợi chứa cả ba hàng, do đó giá trị tối thiểu (4) được thêm đúng một lần. Câu trả lời là (4). 

biểu hiện`r >= a - 1`ngăn thuật toán tạo ra kết quả trước khi tồn tại một cửa sổ dọc hoàn chỉnh. 

### Chiều cao bằng nhau trong hàng đợi 

Giả sử một số ô liên tiếp có cùng mức tối thiểu. Việc triển khai sẽ loại bỏ các giá trị bằng cách sử dụng`>=`, thay vì chỉ`>`. Giữ giá trị bằng nhau mới nhất là đủ vì nó hết hạn sau giá trị bằng cũ hơn. Bản thân mức tối thiểu không thay đổi, trong khi hàng đợi trở nên ngắn hơn. Đây là lý do tại sao phép kiểm tra hoàn toàn bằng nhau vẫn đúng mà không yêu cầu quy tắc giá trị trùng lặp riêng biệt. 

### Câu trả lời lớn 

Có thể có tới ((n-a+1)(m-b+1)) hình chữ nhật, tối đa là (9) triệu. Mỗi mức tối thiểu đều dưới (10^9), vì vậy câu trả lời có thể đạt đến (9\times10^{15}). Các giá trị ma trận được lưu trữ sử dụng số nguyên 32 bit, nhưng`answer`cố tình vẫn là một số nguyên Python bình thường. Việc sử dụng bộ tích lũy 32 bit cho câu trả lời sẽ âm thầm tràn vào các ngôn ngữ có độ rộng số nguyên được cố định. 

### Kích thước tối đa 

Khi (n=m=3000), ma trận chứa (9) triệu ô. Mảng trung gian theo chiều ngang chứa tối đa cùng số lượng giá trị. Danh sách Python sẽ lưu trữ các tham chiếu cộng với các đối tượng số nguyên riêng biệt và có thể tiêu thụ nhiều bộ nhớ hơn nhiều lần so với các giá trị thô.`array('I')`lưu trữ mỗi chiều cao bằng bốn byte, do đó ma trận trung gian trong trường hợp xấu nhất chiếm khoảng (36) MB. Hàng đợi và hàng hiện tại chỉ có (O(n+m)), để lại khoảng trống đáng kể dưới giới hạn 256 MB.
