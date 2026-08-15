---
title: "CF 102433M - Kết nối mê cung"
description: "Đầu vào là bản vẽ hình chữ nhật của một mê cung trực giao sau khi xoay 45 độ. Mỗi ký tự không phải dấu chấm đại diện cho một đoạn tường chéo bên trong ô đầu vào của nó."
date: "2026-08-12T07:41:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102433
codeforces_index: "M"
codeforces_contest_name: "2019-2020 ACM-ICPC Pacific Northwest Regional Contest (Div. 1)"
rating: 0
weight: 102433
solve_time_s: 100
verified: true
draft: false
---

[CF 102433M - Kết nối mê cung](https://codeforces.com/problemset/problem/102433/M) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 40s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Đầu vào là bản vẽ hình chữ nhật của một mê cung trực giao sau khi xoay 45 độ. Mỗi ký tự không phải dấu chấm đại diện cho một đoạn tường chéo bên trong ô đầu vào của nó. Dấu gạch chéo chiếm đường chéo từ góc trên bên phải đến góc dưới bên trái, trong khi dấu gạch chéo ngược chiếm đường chéo đối diện. Các dấu chấm không có tường. 

Nhiệm vụ không phải là tìm một con đường cụ thể. Chúng ta cần loại bỏ càng ít đoạn tường càng tốt để mọi vùng không gian trống đều có đường dẫn đến vùng bên ngoài không giới hạn. Mê cung có thể chứa một số vùng khép kín bị ngắt kết nối, do đó câu trả lời là số lần loại bỏ bức tường cần thiết để hợp nhất tất cả các thành phần không gian trống vào thành phần bên ngoài. 

Điều kiện bàn cờ trên các hướng gạch chéo đảm bảo rằng bản vẽ có dạng hình học mê cung như mong muốn. Quan trọng hơn đối với thuật toán, sau khi chia tỷ lệ hình ảnh theo hệ số hai, mỗi bức tường chéo có thể được biểu thị bằng chính xác hai vị trí lưới bị chặn. Sau đó chúng ta có thể coi mê cung liên tục như một vấn đề kết nối lưới thông thường. 

Với R,C<1000, đầu vào chứa tối đa 10 ô 6. Bất kỳ giải pháp nào kiểm tra số ô đầu vào bậc hai, chẳng hạn như O((RC) 2 ), đều có thể đạt tới 10 12 phép tính và quá chậm. Quét tuyến tính hoặc gần tuyến tính qua việc mở rộng hệ số không đổi của đầu vào là phù hợp. Lưới tỷ lệ có tối đa khoảng 4×10 6 vị trí, do đó việc triển khai cũng phải tránh các đối tượng Python có chi phí quá cao trên mỗi ô. 

Có một số trường hợp đặc biệt có thể đánh lừa việc triển khai chỉ dựa trên các ký tự gốc. Một bức tường không nhất thiết phải bao bọc bất cứ thứ gì. Ví dụ,```
1 1
/
```có đầu ra`0`, bởi vì đoạn chéo đạt đến ranh giới và không cô lập một vùng giới hạn. Việc coi mọi bức tường là vật cản sẽ trở lại không chính xác`1`. 

Mê cung hoàn toàn trống rỗng là một trường hợp biên khác:```
1 1
.
```Toàn bộ không gian đã được kết nối với bên ngoài rồi nên câu trả lời là`0`. Việc triển khai phải đếm các thành phần không gian trống được kết nối thay vì đếm các ô mê cung. 

Hướng của đường chéo cũng có vấn đề. Hãy xem xét mẫu 3:```
2 2
\/
/\
```Bốn bức tường không bao quanh một vùng nên kết quả đúng là`0`. Một giải pháp giả định rằng mọi cách sắp xếp 2×2 của bốn dấu gạch chéo đều tạo ra một hình vuông khép kín sẽ sai. 

Cuối cùng, một số vùng kèm theo cần được loại bỏ nhiều lần. Mẫu 2 có hai vùng khép kín nên câu trả lời của nó là`2`. Việc loại bỏ một bức tường khỏi mỗi khu vực là cần thiết vì việc loại bỏ một bức tường chỉ có thể hợp nhất hai thành phần không gian trống hiện đang tách biệt. 

## Phương pháp tiếp cận 

Một giải pháp brute-force có thể bắt đầu từ định nghĩa hình học. Đối với mọi bộ tường có thể cần loại bỏ, hãy xóa những bức tường đó, lấp đầy mê cung tạo thành và kiểm tra xem mọi thành phần không gian trống có vươn ra bên ngoài hay không. Điều này đúng vì nó kiểm tra rõ ràng điều kiện mà chúng ta được yêu cầu phải thỏa mãn, nhưng có thể có tới 10 6 bức tường. Việc liệt kê các tập hợp con sẽ yêu cầu 2 10 6 khả năng, điều này ngay lập tức là không thể. 

Một chiến lược ít bạo lực hơn là xác định các bức tường và liên tục mô phỏng việc loại bỏ chúng. Đối với mỗi bức tường ứng cử viên, chúng tôi có thể loại bỏ nó và chạy lũ trên toàn bộ lưới tỷ lệ để xác định xem còn lại bao nhiêu thành phần. Với tường W=O(RC) và công việc O(RC) cho mỗi lần lấp lũ, chi phí này đã là O((RC) 2 ). Ở kích thước đầu vào tối đa, giá trị này vào khoảng 10 x 12 lượt truy cập ô, trước khi tính đến các lần lặp lại. 

Quan sát hữu ích là chúng ta thực sự không cần phải quyết định loại bỏ bức tường nào. Chúng tôi chỉ cần biết có bao nhiêu thành phần không gian trống tồn tại trước khi loại bỏ. 

Giả sử có K thành phần được kết nối của không gian trống và một trong số đó là thành phần bên ngoài. Mọi thành phần khác cuối cùng phải được kết nối với nó. Việc loại bỏ một bức tường có thể hợp nhất nhiều nhất hai thành phần, do đó cần phải loại bỏ ít nhất K−1. 

Giới hạn dưới đó cũng có thể đạt được. Các thành phần được ngăn cách bởi các bức tường và đồ thị kề có các đỉnh là các thành phần không gian tự do và các cạnh của nó là các bức tường có thể tháo rời được kết nối thông qua toàn bộ mê cung phẳng. Chúng ta có thể chọn cây bao trùm của đồ thị kề này và loại bỏ các cạnh K−1 của nó. Vì vậy, câu trả lời chính xác là K−1. 

Vấn đề còn lại là đếm K. Cách biểu diễn rõ ràng nhất là chia tỷ lệ mọi ô đầu vào thành khối 2 × 2. Dấu gạch chéo trở thành hai vị trí bị chặn trên đường chéo phía trên bên phải đến đường chéo phía dưới bên trái và dấu gạch chéo ngược trở thành hai vị trí bị chặn trên đường chéo kia. Dấu chấm để lại tất cả bốn vị trí miễn phí. Sau sự chuyển đổi này, kết nối bốn hướng thông thường thể hiện chính xác khả năng kết nối của các khu vực trong mê cung. 

Sau đó chúng tôi lấp đầy mọi vị trí trống chưa được sử dụng. Mỗi lần lấp lũ sẽ khám phá một vùng, vì vậy số lượng lũ là K và câu trả lời là K−1. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O((RC) 2 ) cho việc lấp lũ lặp đi lặp lại | O(RC) | Quá chậm | 
| Tối ưu | O(RC) | O(RC) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc lưới ký tự R×C và tạo lưới tỷ lệ có kích thước khoảng 2R×2C. Chúng tôi sử dụng thêm một hàng và cột không gian trống xung quanh nó để vùng bên ngoài được thể hiện rõ ràng. 
2. Đối với mỗi ô đầu vào chứa`/`, đánh dấu hai vị trí tương ứng với các điểm phía trên bên phải và phía dưới bên trái của khối 2×2 được chia tỷ lệ của nó là bị chặn. MỘT`/`do đó trở thành một bức tường chéo hai ô. 
3. Đối với mỗi ô đầu vào chứa`\`, đánh dấu hai vị trí đối diện là bị chặn. Các dấu chấm để lại toàn bộ khối được chia tỷ lệ miễn phí. 
4. Quét mọi vị trí của lưới tỷ lệ. Bất cứ khi nào tìm thấy một vị trí trống chưa được truy cập, hãy tăng số lượng thành phần và lấp đầy từ vị trí đó, đánh dấu tất cả các vị trí trống có thể tiếp cận là đã truy cập. 
5. Đầu ra`components - 1`. Một trong những thành phần là vùng bên ngoài không giới hạn. Mọi thành phần khác cần phải được kết nối với nó và mỗi lần dỡ bỏ tường có thể làm giảm số lượng thành phần nhiều nhất là một thành phần. 

### Tại sao nó hoạt động 

Lưới tỷ lệ bảo tồn cấu trúc liên kết của mê cung ban đầu vì mỗi bức tường chéo được thể hiện bằng hai ô bị chặn liền kề tạo thành cùng một đoạn đường chéo. Do đó, hai điểm trong không gian trống ban đầu được kết nối chính xác khi vị trí tỷ lệ tương ứng của chúng thuộc cùng một thành phần được kết nối bốn hướng. 

Cho lưới tỷ lệ chứa K thành phần không gian trống. Một là vùng bên ngoài, trong khi các thành phần K−1 còn lại là các vùng khép kín hiện không thể thoát ra ngoài. Việc loại bỏ một bức tường có thể hợp nhất nhiều nhất hai thành phần, do đó, việc loại bỏ ít hơn K−1 không thể kết nối tất cả các vùng với bên ngoài. 

Ngược lại, bất cứ khi nào hai thành phần không gian trống có chung một bức tường, việc loại bỏ bức tường đó sẽ hợp nhất chúng. Cấu trúc liền kề thành phần được kết nối vì toàn bộ mặt phẳng được kết nối khi các bức tường bị bỏ qua. Cây bao trùm của cấu trúc đó có K−1 cạnh và việc loại bỏ các bức tường đó sẽ hợp nhất mọi thành phần vào thành phần bên ngoài. Do đó mức tối thiểu chính xác là K−1. 

## Giải pháp Python```python
import sys
from array import array

input = sys.stdin.readline

def solve():
    R, C = map(int, input().split())
    maze = [input().rstrip('\n') for _ in range(R)]

    H = 2 * R + 2
    W = 2 * C + 2
    total = H * W

    # 0 = free and unvisited
    # 1 = wall or already visited
    grid = bytearray(total)

    # Keep one-cell padding around the drawing. The padding represents
    # the unbounded outside region.
    for i in range(R):
        row = maze[i]
        base = (2 * i + 1) * W
        for j, ch in enumerate(row):
            if ch == '/':
                grid[base + 2 * j + 2] = 1
                grid[base + W + 2 * j + 1] = 1
            elif ch == '\\':
                grid[base + 2 * j + 1] = 1
                grid[base + W + 2 * j + 2] = 1

    components = 0
    stack = array('i')

    for start in range(total):
        if grid[start]:
            continue

        components += 1
        grid[start] = 1
        stack.append(start)

        while stack:
            v = stack.pop()
            r = v // W
            c = v - r * W

            if r > 0:
                u = v - W
                if not grid[u]:
                    grid[u] = 1
                    stack.append(u)

            if r + 1 < H:
                u = v + W
                if not grid[u]:
                    grid[u] = 1
                    stack.append(u)

            if c > 0:
                u = v - 1
                if not grid[u]:
                    grid[u] = 1
                    stack.append(u)

            if c + 1 < W:
                u = v + 1
                if not grid[u]:
                    grid[u] = 1
                    stack.append(u)

    print(components - 1)

if __name__ == "__main__":
    solve()
```Phần đầu tiên của quá trình triển khai xây dựng biểu diễn được chia tỷ lệ. Mảng được lưu trữ dưới dạng`bytearray`, thay vì danh sách số nguyên Python, vì lưới có tỷ lệ lớn nhất chứa khoảng bốn triệu vị trí và mỗi vị trí chỉ cần một bit thông tin logic, được biểu thị ở đây bằng một byte. 

Đối với dấu gạch chéo ở vị trí đầu vào`(i, j)`, các vị trí bị chặn là`(2i+1, 2j+2)`Và`(2i+2, 2j+1)`. Đối với dấu gạch chéo ngược, chúng là`(2i+1, 2j+1)`Và`(2i+2, 2j+2)`. các`+1`offset để lại một đường viền tự do xung quanh bản vẽ hoàn chỉnh. 

Flood fill sử dụng một chồng các chỉ số ô số nguyên thay vì lưu trữ`(row, column)`bộ dữ liệu. Mã hóa một vị trí như`row * W + column`làm cho mỗi mục ngăn xếp là một số nguyên duy nhất.`array('i')`giữ ngăn xếp này nhỏ hơn đáng kể so với danh sách Python chứa hàng triệu đối tượng số nguyên Python. 

Phép chia và số dư được sử dụng để khôi phục hàng và cột là an toàn vì kích thước lưới chỉ khoảng 2000. Số nguyên Python cũng loại bỏ mọi lo ngại về tràn. 

Việc kiểm tra ranh giới được thực hiện có chủ ý trước khi truy cập chỉ mục lân cận. Vì phần đệm bên ngoài là một phần của cùng một lưới tỷ lệ nên không có logic lấp đầy đặc biệt nào cho phần bên ngoài. Nó chỉ đơn giản là thành phần nào chứa các ô đệm. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
2 2
/\
\/
```Bốn bức tường chéo tạo thành một khu vực khép kín. Biểu diễn tỷ lệ chứa một thành phần không gian trống bị chặn và một thành phần bên ngoài. 

| Sân khấu | Hành động | Thành phần được tìm thấy | Trạng thái ngăn xếp | 
| --- | --- | --- | --- | 
| 1 | Bắt đầu ở phần đệm bên ngoài | 1 | Các tế bào bên ngoài được khám phá | 
| 2 | Hoàn thiện việc lấp lũ ngoài khu vực | 1 | Trống | 
| 3 | Tìm vùng tự do kèm theo đầu tiên | 2 | Các tế bào kèm theo được khám phá | 
| 4 | Lũ lụt hoàn thành khu vực khép kín | 2 | Trống | 
| 5 | Tính toán`components - 1`| 1 | Trống | 

Hai thành phần này chính xác như những gì hình học gợi ý: bên ngoài không giới hạn và hình vuông khép kín duy nhất. Việc dỡ bỏ một bức tường sẽ kết nối chúng lại, vì vậy câu trả lời là`1`. 

### Mẫu 2 

Đầu vào là```
4 4
/\..
\.\.
.\/\
..\/
```Mê cung tỷ lệ có ba thành phần không gian trống. Một cái ở bên ngoài và hai cái được bao bọc. 

| Sân khấu | Hành động | Thành phần được tìm thấy | Trạng thái ngăn xếp | 
| --- | --- | --- | --- | 
| 1 | Lũ lụt tràn ngập khu vực bên ngoài | 1 | Trống sau khi truyền tải | 
| 2 | Gặp phải vùng kín đầu tiên | 2 | Trống sau khi truyền tải | 
| 3 | Gặp phải vùng kín thứ hai | 3 | Trống sau khi truyền tải | 
| 4 | Tính toán`components - 1`| 2 | Trống | 

Hai thành phần kèm theo là độc lập nên mỗi thành phần đều yêu cầu kết nối với bên ngoài. Kết quả là`2`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(RC) | Lưới tỷ lệ có các ô O(RC) và mỗi ô được xây dựng và truy cập nhiều nhất một lần. | 
| Không gian | O(RC) | Cả lưới tỷ lệ và ngăn xếp tràn đều sử dụng không gian tuyến tính. | 

Đối với R,C<1000, lưới tỷ lệ chứa tối đa khoảng bốn triệu ô. Thuật toán thực hiện một lượng công việc không đổi trên mỗi ô, do đó nó tránh được hành vi bậc hai liên tục giải mê cung từ đầu. nhỏ gọn`bytearray`Việc biểu diễn đặc biệt hữu ích ở kích thước này, trong khi ngăn xếp số nguyên tránh được việc tốn quá nhiều bộ nhớ khi lưu trữ các bộ dữ liệu tọa độ. 

## Trường hợp thử nghiệm 

Các thử nghiệm sau đây sử dụng logic đếm thành phần giống như giải pháp đã gửi. Trình trợ giúp chấp nhận chuỗi đầu vào hoàn chỉnh và trả về kết quả đầu ra được tạo ra.```python
# helper: run solution on input string, return output string
import sys
import io
from array import array

def solve_io(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    R, C = map(int, sys.stdin.readline().split())
    maze = [sys.stdin.readline().rstrip('\n') for _ in range(R)]

    H = 2 * R + 2
    W = 2 * C + 2
    total = H * W

    grid = bytearray(total)

    for i in range(R):
        row = maze[i]
        base = (2 * i + 1) * W

        for j, ch in enumerate(row):
            if ch == '/':
                grid[base + 2 * j + 2] = 1
                grid[base + W + 2 * j + 1] = 1
            elif ch == '\\':
                grid[base + 2 * j + 1] = 1
                grid[base + W + 2 * j + 2] = 1

    components = 0
    stack = array('i')

    for start in range(total):
        if grid[start]:
            continue

        components += 1
        grid[start] = 1
        stack.append(start)

        while stack:
            v = stack.pop()
            r = v // W
            c = v - r * W

            if r > 0:
                u = v - W
                if not grid[u]:
                    grid[u] = 1
                    stack.append(u)

            if r + 1 < H:
                u = v + W
                if not grid[u]:
                    grid[u] = 1
                    stack.append(u)

            if c > 0:
                u = v - 1
                if not grid[u]:
                    grid[u] = 1
                    stack.append(u)

            if c + 1 < W:
                u = v + 1
                if not grid[u]:
                    grid[u] = 1
                    stack.append(u)

    sys.stdout.write(str(components - 1) + '\n')
    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

# Provided sample 1
assert solve_io(
    "2 2\n"
    "/\\\n"
    "\\/\n"
) == "1\n", "sample 1"

# Provided sample 2
assert solve_io(
    "4 4\n"
    "/\\..\n"
    "\\.\\.\n"
    ".\\/\\\n"
    "..\\/\n"
) == "2\n", "sample 2"

# Provided sample 3
assert solve_io(
    "2 2\n"
    "\\/\n"
    "/\\\n"
) == "0\n", "sample 3"

# Provided sample 4
assert solve_io(
    "8 20\n"
    "/\\/\\/\\/\\/\\/\\/\\/\\/\\/\\/\\\n"
    "\\../\\.\\/./././\\/\\/\\/\\\n"
    "/./\\.././\\/\\.\\/\\/\\/\\/\\\n"
    "\\/\\/\\.\\/\\/./\\/..\\../\n"
    "/\\/./\\/\\/./..\\/\\/..\\\n"
    "\\.\\.././\\.\\/\\/./\\.\\/\n"
    "/.../\\../..\\/./.../\\\n"
    "\\/\\/\\/\\/\\/\\/\\/\\/\\/\\/\\/\\\n"
) == "26\n", "sample 4"

# Minimum-size input, a single empty cell.
assert solve_io(
    "1 1\n"
    ".\n"
) == "0\n", "single empty cell"

# A single diagonal wall reaches the boundary and encloses nothing.
assert solve_io(
    "1 1\n"
    "/\n"
) == "0\n", "single boundary wall"

# A 2x2 diamond with the opposite orientation is open, not enclosed.
assert solve_io(
    "2 2\n"
    "\\/\n"
    "/\\\n"
) == "0\n", "open 2x2 arrangement"

# Maximum-size valid empty maze.
assert solve_io(
    "1000 1000\n" + (".\n" * 1000)
) == "0\n", "maximum-size empty maze"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 2 /\,\/`|`1`| Ánh xạ vùng và đường chéo cơ bản | 
|`4 4 /\.., \.\., .\/\, ..\/`|`2`| Nhiều thành phần kèm theo | 
|`2 2 \/, /\`|`0`| Định hướng không tạo thành vỏ bọc | 
|`1 1 .`|`0`| Mê cung mở hoàn toàn có kích thước tối thiểu | 
|`1 1 /`|`0`| Bức tường chạm ranh giới không bao bọc không gian | 
|`1000 1000`chứa đầy`.`|`0`| Kích thước tối đa và mức sử dụng bộ nhớ | 

## Vỏ cạnh 

Mê cung trống đơn ô```
1 1
.
```chỉ tạo ra một thành phần không gian trống. Quá trình lấp đầy bắt đầu từ ô đó, truy cập vào toàn bộ lưới được chia tỷ lệ bao gồm cả phần đệm của nó và thu được`components = 1`. Tính toán cuối cùng cho`1 - 1 = 0`. 

Bức tường đơn bào```
1 1
/
```được xử lý khác với một hình vuông bốn bức tường khép kín. Hai ô có tỷ lệ bị chặn của nó chỉ tạo thành một đoạn đạt đến ranh giới. Các vị trí trống còn lại đều thuộc về cùng một thành phần nên số lượng thành phần vẫn là`1`và câu trả lời là`0`. Đây là lý do tại sao việc đếm trực tiếp các ký tự trên tường sẽ không chính xác. 

Đối với mẫu 3,```
2 2
\/
/\
```các đoạn đường chéo chạm vào ranh giới theo cách để không gian trống được kết nối. Việc lấp đầy lũ theo tỷ lệ chỉ tìm thấy thành phần bên ngoài. Câu trả lời là do đó`0`, mặc dù có bốn ký tự trên tường. 

Đối với mẫu 2,```
4 4
/\..
\.\.
.\/\
..\/
```việc lấp lũ phát hiện ra ba thành phần. Cái đầu tiên ở bên ngoài, trong khi hai cái còn lại được bao bọc. Thuật toán trả về`3 - 1 = 2`, phù hợp với thực tế là hai khu vực độc lập cần được mở. 

Ở kích thước tối đa, lưới tỷ lệ là khoảng 2002×2002. Thuật toán vẫn truy cập từng vị trí một lần. Việc sử dụng`bytearray`cho lưới và`array('i')`đối với ngăn xếp DFS tránh được chi phí đối tượng lớn mà Python`list[list[int]]`hoặc một chồng`(row, column)`bộ dữ liệu sẽ giới thiệu.
