---
title: "CF 114B - PFAST Inc."
description: "Chúng tôi được giao cho một nhóm nhỏ người và danh sách các cặp không thể làm việc cùng nhau. Chúng ta cần chọn tập hợp con lớn nhất có thể sao cho mọi cặp trong nhóm được chọn đều tương thích. Đây tự nhiên là một vấn đề đồ thị. Hãy coi mỗi tình nguyện viên là một đỉnh."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "brute-force", "graphs"]
categories: ["algorithms"]
codeforces_contest: 114
codeforces_index: "B"
codeforces_contest_name: "Codeforces Beta Round 86 (Div. 2 Only)"
rating: 1500
weight: 114
solve_time_s: 139
verified: true
draft: false
---

[CF 114B - PFAST Inc.](https://codeforces.com/problemset/problem/114/B) 

**Đánh giá:** 1500 
**Tags:** bitmasks, vũ lực, đồ thị 
**Thời gian giải:** 2m 19s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được giao cho một nhóm nhỏ người và danh sách các cặp không thể làm việc cùng nhau. Chúng ta cần chọn tập hợp con lớn nhất có thể sao cho mọi cặp trong nhóm được chọn đều tương thích. 

Đây tự nhiên là một vấn đề đồ thị. Hãy coi mỗi tình nguyện viên là một đỉnh. Nếu hai người không ưa nhau, chúng ta sẽ tạo thêm khoảng cách giữa họ. Nhiệm vụ trở thành tìm tập hợp con lớn nhất của các đỉnh không có cạnh nào giữa hai đỉnh bất kỳ. Trong lý thuyết đồ thị, đây được gọi là tập độc lập cực đại. 

Số lượng tình nguyện viên nhiều nhất là 16, điều này thay đổi hoàn toàn cách chúng ta nên nghĩ về vấn đề này. Bài toán tập độc lập cực đại tổng quát là NP-hard, nhưng chỉ với 16 đỉnh chúng ta có thể tìm kiếm toàn diện trên tất cả các tập con. chỉ có$2^{16} = 65536$tập hợp con, rất nhỏ đối với phần cứng hiện đại. Ngay cả khi chúng ta dành vài trăm thao tác để kiểm tra từng tập hợp con, tổng công việc vẫn dễ dàng phù hợp với thời hạn. 

Một cách tiếp cận quay lui đệ quy đơn giản mà không cắt tỉa vẫn có thể trở nên lộn xộn vì nó liên tục tính toán lại các kiểm tra tương thích giữa các trạng thái chồng chéo. Vì không gian tìm kiếm đã đủ nhỏ nên việc lặp trực tiếp qua từng bitmask sẽ đơn giản và an toàn hơn. 

Có một số trường hợp nguy hiểm có thể âm thầm phá vỡ quá trình triển khai. 

Một sai lầm phổ biến là xử lý mối quan hệ không tương thích theo chỉ đạo thay vì vô hướng. 

đầu vào:```
2 1
Alice
Bob
Alice Bob
```Câu trả lời đúng là bất kỳ người nào. Nếu chúng ta chỉ đánh dấu`Alice -> Bob`nhưng quên`Bob -> Alice`, thì tập con`{Bob, Alice}`có thể xuất hiện không chính xác hợp lệ tùy thuộc vào thứ tự lặp lại. 

Một lỗi dễ xảy ra khác là quên tên có phân biệt chữ hoa chữ thường. 

đầu vào:```
2 0
Petya
petya
```Đây là những người khác nhau. Câu trả lời đúng có chứa cả hai tên. Việc chuyển đổi tên thành chữ thường sẽ hợp nhất chúng không chính xác. 

Một vấn đề phức tạp hơn xuất hiện khi kiểm tra xem một tập hợp con có hợp lệ hay không. Giả sử chúng ta chỉ xác minh rằng mọi nút được chọn đều có ít nhất một nút lân cận tương thích. Điều kiện đó là vô nghĩa vì khả năng tương thích phải được duy trì cho mọi cặp. 

đầu vào:```
3 2
A
B
C
A B
B C
```Đội tối đa hợp lệ là`{A, C}`với kích thước 2. Việc kiểm tra cục bộ bất cẩn có thể cho phép sai cả ba. 

Cuối cùng, đầu ra phải được sắp xếp theo từ điển. Thứ tự lập chỉ mục nội bộ là tùy ý và phụ thuộc vào thứ tự đầu vào, do đó việc in các tên đã chọn trực tiếp từ các chỉ mục có thể tạo ra định dạng sai. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là dùng vũ lực đối với tất cả các nhóm nhỏ người. 

Đối với mỗi tập hợp con, chúng tôi kiểm tra xem tất cả các thành viên được chọn có tương thích theo cặp hay không. Nếu có, chúng tôi so sánh kích thước của nó với câu trả lời tốt nhất được tìm thấy cho đến nay. 

Việc kiểm tra tính tương thích đơn giản nhất sẽ kiểm tra từng cặp bên trong tập hợp con. Với$n = 16$, có$2^{16}$tập hợp con và nhiều nhất$16^2$kiểm tra cặp cho mỗi tập hợp con. Điều đó mang lại khoảng:$$2^{16} \times 16^2 \approx 1.6 \times 10^7$$các hoạt động đã được chấp nhận trong Python. 

Lực lượng vũ phu hoạt động vì các ràng buộc cực kỳ nhỏ. Vấn đề không phải là tính không khả thi tiệm cận mà là sự sạch sẽ trong việc triển khai và tránh việc kiểm tra lặp đi lặp lại không cần thiết. 

Quan sát quan trọng là các tập hợp con được biểu diễn tự nhiên dưới dạng mặt nạ bit. Mỗi người tương ứng với một bit. Đối với mỗi người, chúng tôi cũng có thể lưu trữ một bitmask của những người mà họ có xung đột. 

Bây giờ việc xác thực một tập hợp con trở nên rất nhanh. Vì một người được chọn`i`, nếu như:$$mask \& bad[i] \neq 0$$sau đó tập hợp con chứa ai đó không tương thích với`i`, vì vậy tập hợp con không hợp lệ. 

Điều này chuyển đổi việc kiểm tra theo cặp thành các hoạt động bit nhỏ gọn. 

Cấu trúc vấn đề làm cho bitmasking trở nên lý tưởng vì: 

1. Kích thước vũ trụ rất nhỏ. 
2. Dù thế nào đi nữa, mọi tập hợp con đều phải được xem xét. 
3. Mối quan hệ tương thích là tĩnh. 
4. Hoạt động của bit cực kỳ nhanh chóng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force với tính năng kiểm tra cặp |$O(2^n \cdot n^2)$|$O(n^2)$| Đã chấp nhận | 
| Tối ưu hóa mặt nạ bit |$O(2^n \cdot n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả tên và gán cho mỗi người một chỉ mục từ`0`ĐẾN`n-1`. 

Làm việc với các chỉ số nguyên cho phép chúng ta sử dụng mặt nạ bit nhỏ gọn thay vì chuỗi trong quá trình tính toán. 
2. Xây dựng ánh xạ từ tên đến chỉ mục. 

Điều này cho phép chúng tôi chuyển đổi các cặp không tương thích từ chuỗi thành ID đỉnh số một cách hiệu quả. 
3. Tạo một mảng`bad`Ở đâu`bad[i]`là một bitmask của mọi người không tương thích với con người`i`. 

Nếu người`i`không thích người`j`, sau đó đặt bit`j`bên trong`bad[i]`, và cũng đặt bit`i`bên trong`bad[j]`. 
4. Lặp lại qua từng tập hợp con`mask`từ`0`ĐẾN`(1 << n) - 1`. 

Mỗi bitmask đại diện cho một đội có thể. 
5. Đối với mỗi tập hợp con, hãy kiểm tra xem nó có hợp lệ không. 

Lặp lại qua tất cả mọi người`i`. Nếu người`i`nằm trong tập hợp con và:$$mask \& bad[i] \neq 0$$thì một số đồng đội không tương thích cũng nằm trong tập hợp con nên không thể sử dụng tập hợp con này. 
6. Nếu tập hợp con hợp lệ, hãy so sánh kích thước của nó với câu trả lời đúng nhất hiện tại. 

Sử dụng`mask.bit_count()`để tính toán quy mô nhóm một cách hiệu quả. 
7. Lưu tập hợp con tốt nhất được tìm thấy. 

Bất kỳ giải pháp tối đa nào cũng có thể chấp nhận được, vì vậy sự ràng buộc không thành vấn đề. 
8. Trích xuất các tên thuộc tập hợp con tốt nhất. 

Sắp xếp chúng theo từ điển trước khi in vì vấn đề yêu cầu đầu ra được sắp xếp. 

### Tại sao nó hoạt động 

Một tập hợp con hợp lệ chính xác khi không có cặp không tương thích nào xuất hiện cùng nhau bên trong nó. 

Mảng`bad[i]`chứa mọi người xung đột với`i`. Trong quá trình xác thực, biểu thức:$$mask \& bad[i]$$kiểm tra xem tập hợp con hiện tại có chứa bất kỳ đối tác không tương thích nào của`i`. 

Nếu biểu thức này khác 0 đối với một số người được chọn`i`, thì tập hợp con vi phạm yêu cầu và phải bị từ chối. 

Nếu biểu thức bằng 0 đối với mọi người được chọn thì không có cặp xung đột nào tồn tại ở bất kỳ đâu trong tập hợp con, do đó tập hợp con đó hợp lệ. 

Vì thuật toán kiểm tra mọi tập hợp con có thể nên tập hợp con lớn nhất được tìm thấy được đảm bảo là tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())

    names = [input().strip() for _ in range(n)]

    idx = {}
    for i, name in enumerate(names):
        idx[name] = i

    bad = [0] * n

    for _ in range(m):
        a, b = input().split()

        u = idx[a]
        v = idx[b]

        bad[u] |= (1 << v)
        bad[v] |= (1 << u)

    best_mask = 0
    best_size = 0

    for mask in range(1 << n):
        ok = True

        for i in range(n):
            if (mask >> i) & 1:
                if mask & bad[i]:
                    ok = False
                    break

        if ok:
            size = mask.bit_count()

            if size > best_size:
                best_size = size
                best_mask = mask

    ans = []

    for i in range(n):
        if (best_mask >> i) & 1:
            ans.append(names[i])

    ans.sort()

    print(best_size)
    print(*ans, sep="\n")

solve()
```Phần đầu tiên đọc tên và chuyển đổi chúng thành các chỉ số nguyên. Điều này rất cần thiết vì các hoạt động của mặt nạ bit chỉ hoạt động rõ ràng với ID số. 

các`bad`mảng lưu trữ mặt nạ không tương thích. Nếu như`bad[3]`có bit 1 và 5 được đặt thì người 3 không thể làm việc với người 1 và 5. 

Vòng lặp tập hợp con liệt kê mọi đội có thể. Từ`n <= 16`, tổng số tập con chỉ là 65536. 

Bước xác nhận là cốt lõi của giải pháp. Đối với mỗi người được chọn`i`, chúng tôi kiểm tra xem có đồng đội bị cấm nào cũng tồn tại trong tập hợp con hay không. Biểu thức:```
mask & bad[i]
```trả về một số nguyên khác 0 chính xác khi có ít nhất một người xung đột. 

Việc triển khai sẽ kiểm tra tất cả các đỉnh được chọn một cách độc lập. Mặc dù điều này có thể phát hiện cùng một cặp xấu hai lần, nhưng hệ số không đổi rất nhỏ và logic vẫn rất rõ ràng. 

Giai đoạn đầu ra thu thập các tên đã chọn và sắp xếp chúng theo từ điển. Chi tiết này rất dễ bị quên vì thứ tự tập hợp con bên trong tuân theo thứ tự đầu vào chứ không phải thứ tự từ điển. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 1
Petya
Vasya
Masha
Petya Vasya
```Chỉ định chỉ số: 

| Tên | Chỉ mục | 
| --- | --- | 
| Petya | 0 | 
| Vasya | 1 | 
| Masha | 2 | 

Mặt nạ xung đột: 

| Người | mặt nạ xấu | Ý nghĩa | 
| --- | --- | --- | 
| Petya | 010 | xung đột với Vasya | 
| Vasya | 001 | xung đột với Petya | 
| Masha | 000 | mâu thuẫn với ai | 

Dấu vết tập hợp con: 

| Mặt nạ | Thành viên | hợp lệ | Kích thước | 
| --- | --- | --- | --- | 
| 000 | {} | Có | 0 | 
| 001 | {Petya} | Có | 1 | 
| 010 | {Vasya} | Có | 1 | 
| 011 | {Petya, Vasya} | Không | - | 
| 100 | {Masha} | Có | 1 | 
| 101 | {Petya, Masha} | Có | 2 | 
| 110 | {Vasya, Masha} | Có | 2 | 
| 111 | {Petya, Vasya, Masha} | Không | - | 

Kích thước hợp lệ tối đa là 2. Một câu trả lời có thể chấp nhận được là`{Masha, Petya}`. 

Dấu vết này cho thấy các tập hợp con không hợp lệ sẽ bị loại bỏ ngay lập tức khi một cặp xung đột xuất hiện. 

### Ví dụ 2 

đầu vào:```
4 3
A
B
C
D
A B
B C
C D
```Mặt nạ xung đột: 

| Người | Xung đột | 
| --- | --- | 
| A | B | 
| B | A, C | 
| C | B, D | 
| D | C | 

Dấu vết tập hợp con cho các ứng cử viên lớn hơn: 

| Mặt nạ | Thành viên | hợp lệ | Kích thước | 
| --- | --- | --- | --- | 
| 0011 | {A, B} | Không | - | 
| 0101 | {A, C} | Có | 2 | 
| 1001 | {A, D} | Có | 2 | 
| 1010 | {B, D} | Có | 2 | 
| 1100 | {C, D} | Không | - | 
| 1111 | {A, B, C, D} | Không | - | 

Không tồn tại tập hợp con hợp lệ có kích thước 3, vì vậy kích thước câu trả lời là 2. 

Ví dụ này cho thấy sự không tương thích hoàn toàn là theo cặp. Mặc dù`A`Và`D`đều được kết nối gián tiếp thông qua người khác, họ vẫn có thể thuộc cùng một đội. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(2^n \cdot n)$| Mọi tập hợp con đều được kiểm tra đối với tất cả mọi người | 
| Không gian |$O(n)$| Chúng tôi lưu trữ một mặt nạ xung đột cho mỗi người | 

Với$n \le 16$, thuật toán thực hiện đại khái:$$65536 \times 16 \approx 10^6$$các bước lặp cơ bản, nằm trong giới hạn một cách thoải mái. Việc sử dụng bộ nhớ là không đáng kể. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    def solve():
        n, m = map(int, input().split())

        names = [input().strip() for _ in range(n)]

        idx = {}
        for i, name in enumerate(names):
            idx[name] = i

        bad = [0] * n

        for _ in range(m):
            a, b = input().split()

            u = idx[a]
            v = idx[b]

            bad[u] |= (1 << v)
            bad[v] |= (1 << u)

        best_mask = 0
        best_size = 0

        for mask in range(1 << n):
            ok = True

            for i in range(n):
                if (mask >> i) & 1:
                    if mask & bad[i]:
                        ok = False
                        break

            if ok:
                size = mask.bit_count()

                if size > best_size:
                    best_size = size
                    best_mask = mask

        ans = []

        for i in range(n):
            if (best_mask >> i) & 1:
                ans.append(names[i])

        ans.sort()

        out = [str(best_size)]
        out.extend(ans)

        return "\n".join(out)

    return solve()

# provided sample
assert run(
"""3 1
Petya
Vasya
Masha
Petya Vasya
"""
) == "2\nMasha\nPetya"

# minimum size
assert run(
"""1 0
Solo
"""
) == "1\nSolo"

# everyone compatible
assert run(
"""3 0
A
B
C
"""
) == "3\nA\nB\nC"

# complete conflict graph
out = run(
"""4 6
A
B
C
D
A B
A C
A D
B C
B D
C D
"""
)

assert out in {
    "1\nA",
    "1\nB",
    "1\nC",
    "1\nD"
}

# case-sensitive names
assert run(
"""2 0
Petya
petya
"""
) == "2\nPetya\npetya"

# chain conflicts
out = run(
"""4 3
A
B
C
D
A B
B C
C D
"""
)

assert out in {
    "2\nA\nC",
    "2\nA\nD",
    "2\nB\nD"
}
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Người độc thân | Quy mô đội 1 | Xử lý ràng buộc tối thiểu | 
| Không xung đột | Mọi người đã chọn | Lựa chọn tập hợp con đầy đủ | 
| Đồ thị hoàn chỉnh | Chỉ một người | Xung đột kiểm tra tính chính xác | 
|`Petya`vs`petya`| Cả hai đã chọn | Tên phân biệt chữ hoa chữ thường | 
| Xung đột dây chuyền | Bộ độc lập tối đa | Cấu trúc đồ thị không cần thiết | 

## Vỏ cạnh 

Hãy xem xét trường hợp tính không tương thích bị xử lý nhầm theo hướng dẫn. 

đầu vào:```
2 1
Alice
Bob
Alice Bob
```Thuật toán lưu trữ cả hai:```
bad[Alice] |= Bob
bad[Bob] |= Alice
```Bây giờ khi kiểm tra tập hợp con`{Alice, Bob}`, cả hai người đều phát hiện ra xung đột nên tập hợp con bị từ chối một cách chính xác. 

Hãy xem xét các tên phân biệt chữ hoa chữ thường. 

đầu vào:```
2 0
Petya
petya
```Từ điển ánh xạ lưu trữ hai khóa riêng biệt:```
{
    "Petya": 0,
    "petya": 1
}
```Vì không có xung đột tồn tại nên tập hợp con`11`là hợp lệ và cả hai tên đều xuất hiện trong câu trả lời. 

Hãy xem xét một biểu đồ trong đó các kết nối gián tiếp không quan trọng. 

đầu vào:```
3 2
A
B
C
A B
B C
```Tập hợp con`{A, C}`tương ứng với mặt nạ`101`. 

Vì`A`:```
mask & bad[A] = 101 & 010 = 0
```Vì`C`:```
mask & bad[C] = 101 & 010 = 0
```Tập hợp con hợp lệ vì chỉ có vấn đề không tương thích trực tiếp. 

Cuối cùng, hãy xem xét đồ thị bị ngắt kết nối hoàn toàn. 

đầu vào:```
4 0
A
B
C
D
```Tất cả`bad[i]`mặt nạ bằng không. Mọi tập hợp con đều vượt qua quá trình xác thực và cuối cùng thuật toán sẽ chọn mặt nạ`1111`, chứa tất cả bốn người.
