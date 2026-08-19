---
title: "CF 102192E - Hình vuông kỳ diệu"
description: "Chúng ta có một mảng 3 × 3 chứa các chữ số từ 1 đến 9 đúng một lần. Mảng được chia thành bốn khối 2 × 2 chồng lên nhau: Khối 1 là khối 2 × 2 phía trên bên trái, khối 2 là phía trên bên phải, khối 3 là phía dưới bên trái và khối 4 là phía dưới bên phải."
date: "2026-08-18T02:00:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102192
codeforces_index: "E"
codeforces_contest_name: "2018 Chinese Multi-University Training, Nanjing U Contest"
rating: 0
weight: 102192
solve_time_s: 98
verified: true
draft: false
---

[CF 102192E - Hình vuông ma thuật](https://codeforces.com/problemset/problem/102192/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 38 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một mảng 3 × 3 chứa các chữ số từ 1 đến 9 đúng một lần. Mảng được chia thành bốn khối 2 × 2 chồng lên nhau:```
1 23 4
```Khối 1 là khối 2 × 2 phía trên bên trái, khối 2 là khối phía trên bên phải, khối 3 là khối phía dưới bên trái và khối 4 là phía dưới bên phải. 

Một lệnh như`1C`có nghĩa là xoay khối 1 theo chiều kim đồng hồ 90 độ. Một lệnh như`4R`có nghĩa là xoay khối 4 ngược chiều kim đồng hồ 90 độ. Chỉ có bốn ô bên trong khối 2 × 2 đã chọn di chuyển. Các tế bào còn lại giữ nguyên vị trí của chúng. 

Đối với mỗi trường hợp thử nghiệm, đầu vào cung cấp số lần quay, mảng 3 × 3 ban đầu và sau đó là các phép quay theo thứ tự chính xác của chúng. Chúng ta phải mô phỏng các phép quay đó và in mảng kết quả. 

Những hạn chế là rất nhỏ. Có tối đa 100 trường hợp thử nghiệm và mỗi trường hợp thử nghiệm chứa tối đa 100 phép quay. Vì bảng chỉ có chín ô nên ngay cả một lượng công việc liên tục nhiều lần trên mỗi vòng quay cũng đủ nhanh. Không cần đến thuật toán đồ thị, tìm kiếm, lập trình động hoặc bất kỳ kỹ thuật phức tạp tiệm cận nào khác. 

Nguồn lỗi chính không phải là hiệu suất mà là lập chỉ mục. Vòng quay 2 × 2 phải di chuyển bốn ô cụ thể theo đúng hướng và bốn khối chồng lên nhau. Ví dụ, xoay khối 1 theo chiều kim đồng hồ trong```
123456789
```cho```
413526789
```bởi vì khối```
1245
```trở thành```
4152
```Việc thực hiện bất cẩn có thể xoay bốn giá trị theo hướng ngược lại. Một lỗi phổ biến khác là sử dụng sai hàng hoặc cột bắt đầu cho khối 2, 3 và 4. Ví dụ: khối 4 bắt đầu ở hàng 1, cột 1 bằng cách sử dụng chỉ mục dựa trên 0, không phải ở hàng 2, cột 2. 

Ngoài ra còn có một quan sát giá trị hữu ích. Đầu vào chính thức luôn chứa mỗi chữ số chính xác một lần, vì vậy chúng ta không bao giờ cần kiểm tra xem một nước đi có tạo ra hình vuông ma thuật hợp lệ hay không. Mọi thao tác chỉ là một hoán vị của chín giá trị hiện có. 

## Phương pháp tiếp cận 

Cách tiếp cận theo nghĩa đen nhất là mô phỏng trực tiếp từng lệnh. Đối với mỗi lần quay, chúng ta có thể tạo một mảng 3 × 3 mới và sao chép tất cả chín ô vào vị trí mới của chúng. Vì có tối đa 100 phép quay nên thao tác này thực hiện tối đa 900 phép gán ô cho mỗi trường hợp kiểm thử hoặc nhiều nhất là 90.000 phép gán trên tất cả 100 trường hợp kiểm thử. Điều đó đã thoải mái trong giới hạn. 

Một triển khai nhỏ gọn hơn cho thấy rằng một phép quay ảnh hưởng đến chính xác bốn ô. Nếu khối 2 × 2 đã chọn bắt đầu tại`(r, c)`, tế bào của nó là```
a bc d
```Một vòng quay theo chiều kim đồng hồ tạo ra```
c ad b
```trong khi quay ngược chiều kim đồng hồ tạo ra```
b da c
```Vì vậy chúng ta chỉ cần cập nhật bốn vị trí cho mỗi lệnh. Cấu trúc của bài toán làm cho điều này có thể thực hiện được vì bàn cờ không bao giờ thay đổi kích thước và một nước đi có ảnh hưởng cục bộ cố định. 

Một cuộc tìm kiếm vũ lực giả định liệt kê mọi trình tự có thể có của tám nước đi có thể sẽ phải kiểm tra`8^n`trình tự. Ở mức tối đa`n = 100`, đó là`8^100`, điều đó hoàn toàn không thể thực hiện được. Việc tìm kiếm như vậy là không cần thiết vì trình tự di chuyển đã được cung cấp bởi đầu vào. Mô phỏng trực tiếp loại bỏ hoàn toàn sự phân nhánh theo cấp số nhân đó. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê tất cả các chuỗi di chuyển có thể | O(8^n) | O(9) trên mỗi trạng thái mô phỏng | Quá chậm | 
| Xây dựng lại toàn bộ bảng sau mỗi lần di chuyển | O(9n) = O(n) | O(9) | Đã chấp nhận | 
| Chỉ xoay bốn ô bị ảnh hưởng | O(n) | O(9) | Đã chấp nhận | 

Việc triển khai dự định sử dụng phương pháp cuối cùng. Vì kích thước bảng được cố định nên cả hai phương pháp được chấp nhận đều hoạt động ổn định trên mỗi lệnh một cách hiệu quả, nhưng việc chỉ cập nhật các ô bị ảnh hưởng sẽ đơn giản hơn sau khi ánh xạ xoay được viết chính xác. 

## Hướng dẫn thuật toán 

1. Đọc số lượng test case. Đối với mỗi trường hợp thử nghiệm, hãy đọc`n`và ba chuỗi đại diện cho bảng 3 × 3 hiện tại. Việc giữ mỗi hàng dưới dạng danh sách có thể thay đổi sẽ giúp việc cập nhật từng ô riêng lẻ trở nên thuận tiện. 
2. Đối với mỗi`n`lệnh, đọc số khối và chiều quay. Số khối xác định góc trên bên trái của khối 2 × 2 bị ảnh hưởng. 
3. Ánh xạ số khối tới tọa độ gốc. Khối 1 bắt đầu lúc`(0, 0)`, khối 2 tại`(0, 1)`, khối 3 tại`(1, 0)`và khối 4 tại`(1, 1)`. 
4. Đọc bốn giá trị từ khối đã chọn vào các biến cục bộ. Nếu tọa độ trên cùng bên trái của nó là`(r, c)`, đặt tên cho chúng```
a = board[r][c]b = board[r][c+1]c = board[r+1][c]d = board[r+1][c+1]
```Sử dụng các biến tạm thời sẽ an toàn hơn so với việc thực hiện các phép gán trực tiếp trên bảng, vì phép gán sớm có thể ghi đè lên một giá trị mà phép gán sau này vẫn cần. 
5. Nếu lệnh kết thúc bằng`C`, gán các giá trị theo phép biến đổi theo chiều kim đồng hồ```
a b    c ac d -> d b
```6. Nếu không, lệnh sẽ kết thúc bằng`R`, do đó hãy gán chúng theo phép biến đổi ngược chiều kim đồng hồ```
a b    b dc d -> a c
```7. Sau khi tất cả các phép quay đã được xử lý, hãy in ba hàng của bảng kết quả. 

### Tại sao nó hoạt động 

Điều bất biến là ngay trước khi xử lý mọi lệnh,`board`chính xác là bình phương thu được bằng cách áp dụng tất cả các lệnh đã xử lý trước đó vào bình phương ban đầu. Một lệnh chọn một trong bốn khối 2 × 2 có thể, đọc bốn giá trị hiện tại của nó và thay thế chúng bằng các giá trị chính xác được tạo ra bởi phép quay 90 độ tương ứng. Tất cả các tế bào khác không thay đổi. Do đó, bất biến vẫn đúng sau mỗi lệnh. Sau lệnh cuối cùng, bảng chính xác là trạng thái cuối cùng được yêu cầu. 

## Giải pháp Python```python
Pythonimport sysinput = sys.stdin.readline

def rotate(board, block, direction):    # Zero-based top-left corner of each 2 x 2 block.    positions = {        1: (0, 0),        2: (0, 1),        3: (1, 0),        4: (1, 1),    }
    r, c = positions[block]
    a = board[r][c]    b = board[r][c + 1]    d = board[r + 1][c + 1]    e = board[r + 1][c]
    if direction == 'C':        # a b      e a        # e d  ->  d b        board[r][c] = e        board[r][c + 1] = a        board[r + 1][c] = d        board[r + 1][c + 1] = b    else:        # a b      b d        # e d  ->  a e        board[r][c] = b        board[r][c + 1] = d        board[r + 1][c] = a        board[r + 1][c + 1] = e

def solve():    t = int(input())    output = []
    for _ in range(t):        n = int(input())        board = [list(input().strip()) for _ in range(3)]
        for _ in range(n):            command = input().strip()            block = int(command[0])            direction = command[1]
            rotate(board, block, direction)
        for row in board:            output.append(''.join(row))
    sys.stdout.write('\n'.join(output))

if __name__ == "__main__":    solve()
```các`positions`từ điển mã hóa hình học của bốn khối chồng lên nhau. Với lập chỉ mục dựa trên số 0, bốn góc trên cùng bên trái có thể có chính xác`(0, 0)`,`(0, 1)`,`(1, 0)`, Và`(1, 1)`. 

Các biến tạm thời giữ bốn giá trị ban đầu trước khi thực hiện bất kỳ phép gán nào. Điều này tránh được lỗi xoay cổ điển trong đó việc ghi một đích sẽ phá hủy giá trị mà đích khác cần. 

Phép gán theo chiều kim đồng hồ là```
old bottom-left -> new top-leftold top-left    -> new top-rightold bottom-right -> new bottom-leftold top-right   -> new bottom-right
```Việc gán ngược chiều kim đồng hồ sẽ đảo ngược chu trình đó. Các lệnh được xử lý tuần tự, do đó mỗi vòng quay sẽ hoạt động trên bảng được tạo ra bởi vòng quay trước đó. 

Không có số học số nguyên trong thuật toán nên việc tràn là không thể. Các hoạt động lập chỉ mục duy nhất sử dụng`r`,`r + 1`,`c`, Và`c + 1`; bởi vì mọi khối được chọn đều bắt đầu ở hàng và cột 0 hoặc 1, các chỉ số này luôn nằm trong bảng 3 × 3. 

Giải pháp tích lũy sản lượng trong`output`và viết nó một lần ở cuối. Điều này không cần thiết đối với đầu vào nhỏ như vậy nhưng nó giúp I/O đơn giản và hiệu quả. 

## Ví dụ đã hoạt động 

Mẫu trích xuất của câu lệnh hoàn tất sau khi khôi phục định dạng bị thiếu:```
121234567891C4R
```Sản lượng dự kiến ​​​​là```
413569728
```### Mẫu 1 

Bảng ban đầu là```
123456789
```Lệnh đầu tiên là`1C`, vì vậy chúng ta xoay khối phía trên bên trái. 

| Bước | Lệnh | Chặn trước | Chặn sau | Toàn bộ bảng | 
| --- | --- | --- | --- | --- | 
| 0 | Ban đầu |`12 / 45`|`12 / 45`|`123 / 456 / 789`| 
| 1 |`1C`|`12 / 45`|`41 / 52`|`413 / 526 / 789`| 
| 2 |`4R`|`26 / 89`|`68 / 29`|`413 / 569 / 728`| 

Sau đó`1C`, bốn ô phía trên bên trái trở thành`41 / 52`. Lệnh thứ hai hoạt động ở khối phía dưới bên phải của bảng cập nhật này, không phải trên bảng gốc. Sự khác biệt đó chính là lý do tại sao các lệnh phải được mô phỏng theo thứ tự. 

Bàn cuối cùng là`413 / 569 / 728`, phù hợp với đầu ra mẫu. 

### Ví dụ tùy chỉnh 2 

Hãy xem xét một vòng quay ngược chiều kim đồng hồ của khối phía trên bên phải:```
111234567892R
```Khối bị ảnh hưởng là```
2356
```và xoay nó ngược chiều kim đồng hồ sẽ cho```
3526
```| Bước | Lệnh | Chặn trước | Chặn sau | Toàn bộ bảng | 
| --- | --- | --- | --- | --- | 
| 0 | Ban đầu |`23 / 56`|`23 / 56`|`123 / 456 / 789`| 
| 1 |`2R`|`23 / 56`|`35 / 26`|`135 / 426 / 789`| 

Các ô bên ngoài khối 2 vẫn còn nguyên. Đặc biệt, cột đầu tiên giữ nguyên`1, 4, 7`, đây là một biện pháp kiểm tra hữu ích để đảm bảo rằng quá trình triển khai không vô tình xoay toàn bộ hàng hoặc cột. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) cho mỗi trường hợp thử nghiệm | Mỗi vòng quay đọc và ghi chính xác bốn ô | 
| Không gian | O(1) | Bảng chỉ chứa chín ô và các biến tạm thời có kích thước không đổi | 

Trên tất cả các trường hợp thử nghiệm, tổng công việc là O(Σn). Vì mọi`n`nhiều nhất là 100 và có nhiều nhất 100 ca kiểm thử, có nhiều nhất 10.000 phép quay. Mỗi vòng quay chỉ thực hiện một số thao tác không đổi, do đó giải pháp này thấp hơn nhiều so với giới hạn thời gian một giây và sử dụng bộ nhớ không đáng kể so với giới hạn 128 MB. 

## Trường hợp thử nghiệm 

Đầu vào chính thức yêu cầu mọi chữ số từ 1 đến 9 xuất hiện chính xác một lần, vì vậy bảng hoàn toàn bằng nhau không phải là đầu vào hợp lệ của cuộc thi. Nó vẫn hữu ích như một bài kiểm tra đơn vị cho trình trợ giúp xoay, vì nó xác minh rằng việc xoay một khối 2 × 2 đồng nhất không làm thay đổi nội dung của nó. Khai thác kiểm tra bên dưới giúp tách biệt kiểm tra đó với trình phân tích cú pháp chính thức.```python
Pythonimport sysimport io
input = sys.stdin.readline

def rotate(board, block, direction):    positions = {        1: (0, 0),        2: (0, 1),        3: (1, 0),        4: (1, 1),    }
    r, c = positions[block]
    a = board[r][c]    b = board[r][c + 1]    d = board[r + 1][c + 1]    e = board[r + 1][c]
    if direction == 'C':        board[r][c] = e        board[r][c + 1] = a        board[r + 1][c] = d        board[r + 1][c + 1] = b    else:        board[r][c] = b        board[r][c + 1] = d        board[r + 1][c] = a        board[r + 1][c + 1] = e

def solve():    t = int(input())    output = []
    for _ in range(t):        n = int(input())        board = [list(input().strip()) for _ in range(3)]
        for _ in range(n):            command = input().strip()            rotate(board, int(command[0]), command[1])
        for row in board:            output.append(''.join(row))
    return '\n'.join(output)

def run(inp: str) -> str:    global input    old_stdin = sys.stdin    old_input = input
    sys.stdin = io.StringIO(inp)    input = sys.stdin.readline
    try:        return solve()    finally:        sys.stdin = old_stdin        input = old_input

# Provided sampleassert run(    """121234567891C4R""") == """413569728""", "sample 1"

# Minimum number of rotationsassert run(    """111234567891C""") == """413526789""", "single clockwise rotation"

# Other corner block, counterclockwiseassert run(    """111234567894R""") == """123495786""", "bottom-right counterclockwise rotation"

# Four clockwise rotations restore the original boardassert run(    """141234567893C3C3C3C""") == """123456789""", "four rotations return to the initial state"

# Maximum n for one test case, with repeated inverse rotationscommands = "\n".join(["1C", "1R"] * 50)assert run(    "1\n100\n123\n456\n789\n" + commands + "\n") == """123456789""", "100 rotations with inverse pairs"

# Internal helper test with an all-equal board.uniform = [    list("111"),    list("111"),    list("111"),]rotate(uniform, 1, 'C')rotate(uniform, 2, 'R')rotate(uniform, 3, 'C')rotate(uniform, 4, 'R')assert uniform == [    list("111"),    list("111"),    list("111"),], "uniform block rotation"

print("All tests passed.")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1C`TRÊN`123/456/789`|`413/526/789`| Chuỗi lệnh kích thước tối thiểu và ánh xạ theo chiều kim đồng hồ | 
|`4R`TRÊN`123/456/789`|`123/495/786`| Ranh giới dưới cùng bên phải và ánh xạ ngược chiều kim đồng hồ | 
| bốn`3C`lệnh |`123/456/789`| Bốn vòng quay hình thành bản sắc | 
| 100 xen kẽ`1C`,`1R`lệnh |`123/456/789`| Số lượng lệnh tối đa và cập nhật trạng thái tuần tự | 
| Đồng phục`111/111/111`kiểm tra trợ giúp |`111/111/111`| Phép gán xoay không phụ thuộc vào các giá trị riêng biệt | 

## Vỏ cạnh 

Việc xoay khối ở góc là nơi dễ mắc lỗi lập chỉ mục nhất. Coi như```
111234567894R
```Khối 4 bắt đầu lúc`(1, 1)`, vậy giá trị của nó là```
5689
```Xoay ngược chiều kim đồng hồ tạo ra```
6859
```và bảng cuối cùng là```
123468759
```Bài kiểm tra này thực hiện các chỉ số bắt đầu hàng và cột tối đa. Việc triển khai vô tình sử dụng`(2, 2)`vì tọa độ bắt đầu của khối sẽ truy cập bên ngoài bảng. 

Trường hợp tinh tế thứ hai là sự khác biệt giữa xoay theo chiều kim đồng hồ và ngược chiều kim đồng hồ. Vì```
111234567892R
```khối được chọn là```
2356
```và kết quả ngược chiều kim đồng hồ của nó là```
3526
```vậy bảng cuối cùng là```
135426789
```Một lỗi phổ biến là sử dụng hoán vị theo chiều kim đồng hồ cho cả hai chữ cái, thay vào đó sẽ tạo ra`165 / 423 / 789`. 

Thứ tự trình tự cũng quan trọng vì các khối chồng lên nhau. Trong mẫu,```
1C4R
```thao tác thứ hai nhìn thấy bảng sau thao tác đầu tiên. Bắt đầu từ```
123456789
```

`1C`sản xuất```
413526789
```và do đó khối 4 là`26 / 89`, không phải bản gốc`56 / 89`. Xoay khối đó ngược chiều kim đồng hồ sẽ ra bảng cuối cùng```
413569728
```Việc kiểm tra độ chính xác hữu ích cho bất kỳ quá trình triển khai nào là bốn phép quay giống hệt nhau. Bất kỳ khối 2 × 2 nào cũng trở về vị trí ban đầu sau bốn lần quay 90 độ. Như vậy```
141234567893C3C3C3C
```phải sản xuất```
123456789
```Điều này gây ra nhiều lỗi gán tuần hoàn vì một hoán vị sai có thể có vẻ hợp lý sau một lần quay nhưng không thể quay lại trạng thái ban đầu sau bốn lần áp dụng.
