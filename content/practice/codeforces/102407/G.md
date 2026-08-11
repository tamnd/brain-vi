---
title: "CF 102407G - Domino điên cuồng"
description: "Chúng ta có một bàn cờ (n lần n). Chúng tôi có thể đặt tối đa (n) quân cờ trên từng ô riêng lẻ. Mỗi ô còn lại phải được bao phủ bởi chính xác một domino, trong đó một domino luôn bao phủ hai ô chung một bên."
date: "2026-08-10T16:15:55+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102407
codeforces_index: "G"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2019-2020, \u0412\u0442\u043e\u0440\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430, \u0443\u0441\u043b\u043e\u0436\u043d\u0435\u043d\u043d\u0430\u044f \u043d\u043e\u043c\u0438\u043d\u0430\u0446\u0438\u044f"
rating: 0
weight: 102407
solve_time_s: 321
verified: true
draft: false
---

[CF 102407G - Domino điên rồ](https://codeforces.com/problemset/problem/102407/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 5 phút 21s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một bàn cờ (n \times n). Chúng tôi có thể đặt tối đa (n) quân cờ trên từng ô riêng lẻ. Mỗi ô còn lại phải được bao phủ bởi chính xác một domino, trong đó một domino luôn bao phủ hai ô chung một bên. Việc sắp xếp các quân cờ phải được chọn sao cho bàn cờ còn lại có đúng một ô domino. 

Đầu vào chỉ chứa (n), với (2 \le n \le 100). Đầu ra là một lưới (n \times n) trong đó`#`đại diện cho một người kiểm tra và`.`đại diện cho một ô tự do. Công trình được đảm bảo tồn tại. Giới hạn chính thức là 2 giây và 512 MB. 

Giới hạn trên nhỏ của 100 hơi gây nhầm lẫn. Đây không phải là vấn đề mà chúng ta nên tìm kiếm thông qua bảng. Có (n^2) ô và thậm chí chỉ xem xét các sắp xếp chứa chính xác (n) bộ kiểm tra sẽ đưa ra (\binom{n^2}{n}) ứng cử viên. Với (n=100), giá trị này gần bằng (10^{242}), do đó việc xây dựng toàn diện là hoàn toàn không thể. Giải pháp dự định phải khai thác thực tế là chúng ta chỉ cần một mẫu được thiết kế cẩn thận. 

Có một số trường hợp đặc biệt mà việc xây dựng dựa trên tính chẵn lẻ chung có thể xử lý sai. Đối với (n=2), việc đặt quân cờ vào cả hai ô của hàng đầu tiên sẽ để lại hàng thứ hai, chính xác là một domino, vì vậy`##`theo sau là`..`đã hợp lệ rồi. Đối với (n=3), cấu trúc góc xen kẽ kích thước lẻ nói chung không hoạt động, do đó cần có một cấu trúc nhỏ riêng biệt. Bản thân mẫu đưa ra một cấu trúc như vậy:```
...
##.
#..
```Sáu ô trống có đúng một ô xếp. 

Với (n=4), công trình xây dựng chẵn, đơn giản:```
#..#
....
#..#
....
```Có chính xác bốn quân cờ, và các ô tự do bị ép làm quân domino từ hai bên. Việc quên đi sự phân biệt chẵn lẻ sẽ sử dụng quá nhiều quân cờ hoặc để lại một bảng có nhiều ô. 

## Phương pháp tiếp cận 

Một giải pháp brute-force có thể thử mọi tập hợp có thể có tối đa (n) ô kiểm tra. Đối với mỗi ứng cử viên, chúng tôi sẽ phải xác định xem liệu các ô còn lại có thể được xếp theo ô hay không và liệu ô đó có phải là duy nhất hay không. Chỉ riêng số lượng tập ứng cử viên ít nhất là (\binom{n^2}{n}). Tại (n=100), đây là khoảng (10^{242}), trước khi thực hiện bất kỳ công việc nào để kiểm tra ô xếp. Lực lượng vũ phu là chính xác vì nó kiểm tra rõ ràng định nghĩa, nhưng không gian tìm kiếm vượt xa mọi thứ khả thi. 

Quan sát hữu ích là chúng ta không cần phải khám phá cách xếp lớp. Chúng ta có thể xây dựng bàn cờ sao cho các quân cờ ở biên có tác dụng lần lượt từng quân domino. Khi một ô chỉ có một ô tự do lân cận, quân domino của ô đó sẽ bị ép buộc. Việc đặt quân domino đó có thể khiến ô khác chỉ có một đối tác khả thi, tạo ra một chuỗi các vị trí bắt buộc. 

Đối với số chẵn (n), đặt dấu kiểm vào cột đầu tiên và cột cuối cùng của mỗi hàng số lẻ. Điều này sử dụng chính xác (n) trình kiểm tra. Các ô ở cuối mỗi hàng chẵn sau đó được ép theo chiều ngang và điều này buộc các ô lân cận ở các hàng lẻ, tiếp tục hướng về phía giữa. 

Đối với số lẻ (n \ge 5), hãy luân phiên kiểm tra giữa hai cột cực trị. Các hàng lẻ nhận được một dấu kiểm ở cột đầu tiên, trong khi các hàng chẵn nhận được một dấu kiểm ở cột cuối cùng. Một lần nữa có chính xác (n) quân cờ. Ranh giới xen kẽ tạo ra một chuỗi domino cưỡng bức lan truyền khắp bàn cờ. Đây là cách xây dựng được đưa ra trong hướng dẫn chính thức. 

Giá trị ngoại lệ duy nhất là (n=3), giá trị này chúng tôi sử dụng cấu trúc nhỏ từ mẫu. Trường hợp (n=2) được xử lý trực tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (\Theta\left(\binom{n^2}{n}\right)) ứng viên trước khi kiểm tra xếp gạch | Ít nhất (O(n^2)) | Quá chậm | 
| Xây dựng | (O(n^2)) | (O(n^2)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc (n) và tạo một bảng (n \times n) chứa đầy`.`. Chúng ta sẽ chỉ cần quyết định ô nào sẽ trở thành ô kiểm tra. 
2. Nếu (n=2), đặt quân cờ vào cả hai ô của hàng đầu tiên. Hàng còn lại gồm đúng hai ô liền kề nên việc xếp gạch domino của nó là bắt buộc. 
3. Nếu (n=3), hãy sử dụng lưới```
...
##.
#..
```Có ba quân cờ, sáu ô còn lại được buộc thành ba quân domino. 

1. Nếu (n) chẵn thì đặt`#`trong cột (1) và (n) của mỗi hàng đánh số lẻ. Trong các chỉ mục Python dựa trên 0, điều này có nghĩa là các hàng`0, 2, 4, ...`nhận được một người kiểm tra tại các cột`0`Và`n - 1`. 
2. Nếu (n) là số lẻ và ít nhất (5), hãy đánh dấu vào cột (1) trên mỗi hàng đánh số lẻ và trong cột (n) trên mỗi hàng đánh số chẵn. Trong lập chỉ mục dựa trên 0, hàng`i`nhận được một người kiểm tra trong cột`0`khi`i`là số chẵn và cột`n - 1`khi`i`thật kỳ quặc. 
3. In bảng kết quả. Trong trường hợp chẵn có (2(n/2)=n) cờ. Trong trường hợp lẻ có ((n+1)/2+(n-1)/2=n) bộ kiểm tra, do đó giới hạn kiểm tra được thỏa mãn chính xác. 

**Tại sao nó hoạt động.** Điều bất biến chính là bộ kiểm tra ranh giới sẽ loại bỏ tất cả các đối tác thay thế cho các ô bên cạnh chúng. Khi một quân domino bắt buộc được đặt, một ô khác sẽ bị ép buộc và quá trình tiếp tục. Đối với kết cấu đồng đều, lực truyền truyền đối xứng từ hai cột cực trị. Đối với kết cấu lẻ, hai hướng cưỡng bức xen kẽ giữa các cột cực đoan khi chúng ta di chuyển qua các hàng. Do đó, mỗi ô tự do đều thuộc về một quân domino bắt buộc và không còn quyết định nào khi việc xếp ô hoàn tất. Hướng dẫn chính thức mô tả ý tưởng tương tự bằng cách quan sát rằng các quân cờ ở các cột cực đoan làm cho việc xếp gạch domino trở nên độc đáo. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())

    board = [['.' for _ in range(n)] for _ in range(n)]

    if n == 2:
        board[0][0] = '#'
        board[0][1] = '#'

    elif n == 3:
        board = [
            list("..."),
            list("##."),
            list("#.."),
        ]

    elif n % 2 == 0:
        for r in range(0, n, 2):
            board[r][0] = '#'
            board[r][n - 1] = '#'

    else:
        for r in range(n):
            if r % 2 == 0:
                board[r][0] = '#'
            else:
                board[r][n - 1] = '#'

    sys.stdout.write('\n'.join(''.join(row) for row in board))

if __name__ == "__main__":
    solve()
```Bảng được khởi tạo hoàn toàn bằng các ô trống, do đó mỗi phép gán của`#`là rõ ràng và không cần có số lượng người kiểm tra riêng biệt. 

Các trường hợp (n=2) và (n=3) được xử lý trước khi xây dựng tính chẵn lẻ vì chúng là các kích thước nhỏ duy nhất mà các mẫu chung không phù hợp. Với (n=3), việc xây dựng mẫu rõ ràng đặc biệt thuận tiện. 

Đối với chẵn (n), lặp lại`range(0, n, 2)`truy cập chính xác các hàng được đánh số lẻ trong lập chỉ mục dựa trên một. Cả hai cột cực trị đều nhận được một trình kiểm tra trong mỗi hàng như vậy. 

Đối với số lẻ (n \ge 5), mỗi hàng nhận được chính xác một bộ kiểm tra. Quân cờ xen kẽ giữa ranh giới bên trái và bên phải, vì vậy chính xác (n) quân cờ được sử dụng. 

Không có rủi ro số học hoặc tính toán ranh giới phức tạp. Các biểu thức nhạy cảm với ranh giới duy nhất là`0`Và`n - 1`, có giá trị với mọi (n) được phép. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, (n=3), thuật toán sẽ đi vào nhánh chữ nhỏ rõ ràng. 

| Bước | n | Hành động | Ban | 
| --- | --- | --- | --- | 
| 1 | 3 | Khởi tạo |`... / ... / ...`| 
| 2 | 3 | Sử dụng công trình đặc biệt |`... / ##. / #..`| 
| 3 | 3 | In |`... / ##. / #..`| 

Các ô tự do là ((1,1),(1,2),(1,3),(2,3),(3,2),(3,3)). Quân domino bao phủ hàng dưới cùng phải sử dụng ((3,2),(3,3)). Khi đó ((1,1),(1,2)) phải tạo thành một quân domino ngang, để lại ((1,3),(2,3)) là quân domino cuối cùng. Không có sự thay thế nào còn lại. 

Đối với Mẫu 2, (n=4), cấu trúc chẵn được sử dụng. 

| Hàng | n | Vị trí kiểm tra | Kết quả | 
| --- | --- | --- | --- | 
| 1 | 4 | cột 1 và 4 |`#..#`| 
| 2 | 4 | không |`....`| 
| 3 | 4 | cột 1 và 4 |`#..#`| 
| 4 | 4 | không |`....`| 

Bốn quân cờ chính xác là số lượng cho phép. Ở hàng 2, cả hai ô ranh giới chỉ có thể ghép theo chiều ngang vì các ô ngay phía trên và phía dưới chúng đều bị ô kiểm soát. Những quân domino bắt buộc đó sau đó buộc các ô còn lại ở hàng 1 và 3. Lý do tương tự sẽ kết thúc bàn cờ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n^2)) | Bảng chứa (n^2) ô và chúng tôi khởi tạo và xuất tất cả chúng. | 
| Không gian | (O(n^2)) | Bảng được xây dựng được lưu trữ dưới dạng lưới ký tự (n \times n). | 

Với (n \le 100), bảng chứa tối đa 10.000 ô. Thuật toán chỉ thực hiện một lượng công việc không đổi trên mỗi ô nên thấp hơn nhiều so với giới hạn 2 giây và sử dụng bộ nhớ không đáng kể so với giới hạn 512 MB. 

## Trường hợp thử nghiệm 

Các mẫu cho phép mọi cấu trúc hợp lệ, do đó, bộ khai thác thử nghiệm bên dưới sẽ xác nhận bảng được trả về thay vì yêu cầu một đầu ra mẫu cụ thể. Đối với các bảng nhỏ, nó cũng đếm tất cả các ô domino theo cách đệ quy và xác minh rằng có chính xác một ô tồn tại. Thử nghiệm kích thước tối đa kiểm tra trực tiếp các yêu cầu về cấu trúc.```python
# helper: run solution on input string, return output string
import sys
import io

def solve_data(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        n = int(sys.stdin.readline())
        board = [['.' for _ in range(n)] for _ in range(n)]

        if n == 2:
            board[0][0] = '#'
            board[0][1] = '#'

        elif n == 3:
            board = [
                list("..."),
                list("##."),
                list("#.."),
            ]

        elif n % 2 == 0:
            for r in range(0, n, 2):
                board[r][0] = '#'
                board[r][n - 1] = '#'

        else:
            for r in range(n):
                if r % 2 == 0:
                    board[r][0] = '#'
                else:
                    board[r][n - 1] = '#'

        sys.stdout.write('\n'.join(''.join(row) for row in board))
        return sys.stdout.getvalue()

    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

def run(inp: str) -> str:
    return solve_data(inp)

def count_tilings(board):
    n = len(board)
    free = []

    for r in range(n):
        for c in range(n):
            if board[r][c] == '.':
                free.append((r, c))

    index = {cell: i for i, cell in enumerate(free)}
    full = (1 << len(free)) - 1

    memo = {}

    def dfs(mask):
        if mask == full:
            return 1

        if mask in memo:
            return memo[mask]

        for i, (r, c) in enumerate(free):
            if mask & (1 << i):
                continue

            ways = 0

            for dr, dc in ((1, 0), (0, 1)):
                nr, nc = r + dr, c + dc
                j = index.get((nr, nc))

                if j is not None and not (mask & (1 << j)):
                    ways += dfs(mask | (1 << i) | (1 << j))

            memo[mask] = ways
            return ways

        return 0

    return dfs(0)

def validate_small(inp: str):
    n = int(inp)
    out = run(inp).strip().splitlines()

    assert len(out) == n
    assert all(len(row) == n for row in out)

    checkers = sum(row.count('#') for row in out)
    assert checkers <= n

    board = [list(row) for row in out]
    assert count_tilings(board) == 1

# Sample 1
validate_small("3\n")

# Sample 2
validate_small("4\n")

# Minimum size
validate_small("2\n")

# Small odd case that needs the special construction
validate_small("5\n")

# Boundary and parity case
validate_small("6\n")

# Maximum size
out = run("100\n").strip().splitlines()
assert len(out) == 100
assert all(len(row) == 100 for row in out)
assert all(ch in ".#" for row in out for ch in row)
assert sum(row.count('#') for row in out) <= 100
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`3`| Bất kỳ công trình xây dựng 3 x 3 hợp lệ nào | Cung cấp Mẫu 1 và trường hợp lẻ đặc biệt | 
|`4`| Bất kỳ công trình xây dựng 4 x 4 hợp lệ nào | Cung cấp Mẫu 2 và chẵn lẻ | 
|`2`|`## / ..`| Tối thiểu cho phép (n) và xử lý ranh giới | 
|`5`| Công cụ kiểm tra cột cực xen kẽ | Trường hợp lẻ nhỏ nhất sử dụng cấu trúc tổng quát | 
|`6`| Cờ đam ở cả hai đầu hàng lẻ | Ngay cả việc lập chỉ mục xây dựng và hàng | 
|`100`| Lưới 100 x 100 hợp lệ | Kích thước tối đa và giới hạn số lượng kiểm tra | 

## Vỏ cạnh 

Với (n=2), đầu vào chính xác là:```
2
```Thuật toán tạo ra:```
##
..
```Có hai quân cờ thỏa mãn giới hạn. Hai ô còn lại liền kề nhau nên chỉ có duy nhất một vị trí domino. 

Với (n=3), đầu vào chính xác là:```
3
```Thuật toán tạo ra:```
...
##.
#..
```Số lượng người kiểm tra là ba. Hai ô tự do phía dưới tạo thành một quân domino ngang bắt buộc, sau đó cặp ô trên cùng bên trái và cặp dọc ngoài cùng bên phải bị buộc. Đây là lý do tại sao cần có nhánh (n=3) rõ ràng. 

Đối với kích thước chẵn chẳng hạn như (n=6), việc xây dựng là:```
#....#
......
#....#
......
#....#
......
```Có sáu người kiểm tra. Mỗi ô ranh giới trong một hàng chẵn bị mắc kẹt giữa các ô kiểm tra theo chiều dọc, do đó nó phải ghép vào trong theo chiều ngang. Sau đó, các quân domino bắt buộc đó sẽ xác định các ô lân cận ở các hàng lẻ và quá trình này tiếp tục cho đến khi phủ hết mọi ô trống. 

Đối với một chiều lẻ chẳng hạn như (n=5), việc xây dựng là:```
#....
....#
#....
....#
#....
```Một lần nữa có đúng năm quân cờ. Các vị trí ranh giới xen kẽ nhau tạo thành chuỗi lực được mô tả trong công trình. Tính chẵn lẻ của (n) làm cho hai mặt trận cưỡng bức gặp nhau một cách nhất quán thay vì để lại một ô trung tâm chưa từng có. 

Đối với (n=100), cùng một cấu trúc chẵn sử dụng chính xác 100 quân cờ, một cặp trên mỗi hàng trong số 50 hàng số lẻ. Thuật toán không phụ thuộc vào bất kỳ thuộc tính số đặc biệt nào của 100, do đó ràng buộc tối đa được xử lý theo cách giống hệt như mọi giá trị chẵn khác.
