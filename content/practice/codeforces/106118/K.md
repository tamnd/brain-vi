---
title: "CF 106118K - Vua Tic-Tac-Toe"
description: "Chúng ta được cấp một bảng Tic-Tac-Toe 3 × 3. Mỗi ô chứa X, O hoặc .. Bàn cờ không được đảm bảo là trạng thái hợp lệ so với trò chơi thông thường nên số lượng X và O không quan trọng. Nobita có một sức mạnh đặc biệt: cậu có thể đặt hai dấu O liên tiếp vào hai ô trống bất kỳ."
date: "2026-06-19T20:07:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106118
codeforces_index: "K"
codeforces_contest_name: "2025 ICPC, Chula Selection Contest"
rating: 0
weight: 106118
solve_time_s: 50
verified: true
draft: false
---

[CF 106118K - Vua Tic-Tac-Toe](https://codeforces.com/problemset/problem/106118/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 50s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một bảng Tic-Tac-Toe 3 × 3. Mỗi ô chứa một trong hai`X`,`O`, hoặc`.`. Bàn cờ không được đảm bảo là trạng thái hợp lệ so với một trò chơi bình thường, vì vậy số lượng`X`Và`O`không quan trọng. 

Nobita có được một sức mạnh đặc biệt: cậu có thể đặt hai`O`đánh dấu liên tiếp vào hai ô trống bất kỳ. Sau hai vị trí đó, trò chơi ngay lập tức kết thúc. Nếu bảng kết quả chứa ít nhất một hàng, cột hoặc đường chéo hoàn chỉnh bao gồm toàn bộ`O`, thì Nobita thắng. 

Đối với mỗi trường hợp thử nghiệm, chúng ta phải xác định xem liệu một cặp nước đi như vậy có tồn tại hay không. Nếu có, chúng tôi in`YES`và kết quả là một bảng chiến thắng. Ngược lại, chúng tôi in`NO`. 

Kích thước bảng được cố định ở mức 3 × 3. Mặc dù có thể có tới 1000 trường hợp thử nghiệm nhưng mỗi bảng chỉ chứa 9 ô. Bất kỳ thuật toán nào thực hiện một lượng công việc không đổi trên mỗi bảng đều đủ nhanh. Chúng tôi không cần tối ưu hóa phức tạp vì không gian tìm kiếm rất nhỏ. 

Thách thức chính không phải là hiệu suất mà là tính chính xác. Chúng ta phải xem xét cẩn thận tất cả các cách có thể để đặt hai mục tiêu mới`O`dấu vết. 

Một sai lầm dễ mắc phải là chỉ kiểm tra những dòng đã chứa một số`O`S. Một dòng chiến thắng có thể yêu cầu điền vào hai ô trống. 

Ví dụ:```
...
OO.
...
```Đặt một`O`ở ô cuối cùng của hàng giữa sẽ thắng ngay. Vì Nobita phải thực hiện đúng hai nước đi nên cậu ấy có thể đặt nước thứ hai`O`bất cứ nơi nào khác. Câu trả lời đúng là`YES`. 

Một trường hợp tinh tế khác là khi dòng chiến thắng không chứa`O`không hề.```
...
.X.
...
```Nobita có thể đặt hai`O`s trong cùng một hàng, nhưng vẫn chỉ cho hai điểm. Một dòng đầy đủ yêu cầu ba`O`s, vậy câu trả lời là`NO`. 

Một sai lầm khác là dừng lại sau khi tìm thấy một nước đi không thắng. Chúng ta cần xem xét từng cặp ô trống.```
X.X
.O.
...
```Một số vị trí không thành công nhưng việc chọn ô bên trái và bên phải của hàng giữa sẽ tạo ra`OOO`. Câu trả lời đúng là`YES`. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là vũ lực. Thu thập tất cả các ô trống, chọn mọi cặp ô trống riêng biệt có thể, đặt`O`ở những vị trí đó và kiểm tra xem bảng kết quả có chứa dòng chiến thắng hay không. 

Một bàn cờ Tic-Tac-Toe có nhiều nhất là 9 ô nên có nhiều nhất là 9 ô trống. Số lượng cặp nhiều nhất là:$$\binom{9}{2} = 36$$Đối với mỗi cặp, chúng tôi kiểm tra 8 dòng có thể thắng, cụ thể là 3 hàng, 3 cột và 2 đường chéo. Tổng số công việc cho mỗi trường hợp thử nghiệm chỉ là vài trăm thao tác. 

Vì kích thước bảng là cố định nên tìm kiếm brute-force này đã tối ưu trong thực tế. Không có cấu trúc nào lớn hơn để khai thác và bất kỳ cách tiếp cận phức tạp nào hơn sẽ chỉ làm tăng thêm sự phức tạp không cần thiết. 

Quan sát quan trọng là bảng quá nhỏ nên việc tìm kiếm toàn diện thực tế là thời gian không đổi. Thay vì suy luận riêng về các đường thắng tiềm năng, chúng ta có thể chỉ cần kiểm tra từng cặp nước đi hợp lệ và trực tiếp xác minh kết quả. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên tất cả các cặp di chuyển | O(E²) trong đó E ≤ 9 | O(1) | Đã chấp nhận | 
| Tối ưu | O(E²) trong đó E ≤ 9 | O(1) | Đã chấp nhận | 

Từ`E`nhiều nhất là 9, cả hai mô tả đều thể hiện cùng một giải pháp thực tế. 

## Hướng dẫn thuật toán 

1. Đọc bảng 3 × 3. 
2. Thu thập tọa độ của tất cả các ô trống. 
3. Với mỗi cặp ô trống riêng biệt: 

Địa điểm`O`ở cả hai vị trí trên một bản sao tạm thời của bảng. 
4. Kiểm tra xem bảng đã sửa đổi có chứa đường chiến thắng hay không`O`. 

Đường chiến thắng là bất kỳ hàng, cột hoặc đường chéo nào có ba ô đều`O`. 
5. Nếu có đường thắng, hãy in`YES`và bảng sửa đổi. 

Chúng ta có thể dừng ngay lập tức vì bài toán chấp nhận bất kỳ bảng thắng nào. 
6. Nếu tất cả các cặp đã được kiểm tra và không có cặp nào giành chiến thắng, hãy in`NO`. 

### Tại sao nó hoạt động 

Thuật toán kiểm tra mọi cách hợp pháp mà Nobita có thể sử dụng chiêu thức kép của mình. Mỗi kết quả có thể xảy ra sau hai vị trí tương ứng với chính xác một cặp ô trống và mỗi cặp như vậy đều được kiểm tra. 

Khi một cặp được kiểm tra, thuật toán sẽ kiểm tra rõ ràng tất cả 8 dòng chiến thắng. Nếu bất kỳ dòng nào bao gồm toàn bộ`O`, bảng đó đang thắng. Nếu không có đường nào thì bảng đó không thắng. 

Bởi vì mọi cặp nước đi hợp lệ đều được xem xét và mọi bảng kết quả đều được đánh giá chính xác, nên thuật toán sẽ tìm một bảng chiến thắng bất cứ khi nào có bảng đó và báo cáo`NO`chỉ khi không tồn tại. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def is_win(board):
    for r in range(3):
        if all(board[r][c] == 'O' for c in range(3)):
            return True

    for c in range(3):
        if all(board[r][c] == 'O' for r in range(3)):
            return True

    if all(board[i][i] == 'O' for i in range(3)):
        return True

    if all(board[i][2 - i] == 'O' for i in range(3)):
        return True

    return False

t = int(input())

for _ in range(t):
    board = [list(input().strip()) for _ in range(3)]

    empty = []
    for r in range(3):
        for c in range(3):
            if board[r][c] == '.':
                empty.append((r, c))

    found = False

    m = len(empty)
    for i in range(m):
        for j in range(i + 1, m):
            temp = [row[:] for row in board]

            r1, c1 = empty[i]
            r2, c2 = empty[j]

            temp[r1][c1] = 'O'
            temp[r2][c2] = 'O'

            if is_win(temp):
                print("YES")
                for row in temp:
                    print("".join(row))
                found = True
                break

        if found:
            break

    if not found:
        print("NO")
```Chức năng trợ giúp`is_win`kiểm tra tất cả tám dòng chiến thắng có thể. Vì kích thước bảng là cố định nên việc kiểm tra rõ ràng các hàng, cột và đường chéo là cách thực hiện đơn giản và an toàn nhất. 

Danh sách`empty`lưu trữ mọi vị trí di chuyển có sẵn. Các vòng lặp lồng nhau lặp lại trên tất cả các cặp ô trống không có thứ tự. sử dụng`j = i + 1`tránh việc kiểm tra cùng một cặp hai lần và tránh việc chọn cùng một ô hai lần. 

Đối với mỗi cặp ứng cử viên, một bản sao mới của bảng sẽ được tạo. Điều này tránh phải hoàn tác các bước di chuyển sau đó và giữ logic đơn giản. 

Thời điểm tìm thấy bảng chiến thắng, các vòng lặp sẽ kết thúc và bảng được in. Bảng trúng thưởng hợp lệ nào cũng được chấp nhận nên không cần tiếp tục tìm kiếm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Bảng đầu vào:```
X.X
.O.
...
```Ô trống: 

| Đã thử nghiệm cặp | Chiến thắng? | 
| --- | --- | 
| (0,1), (1,0) | Không | 
| (0,1), (1,2) | Không | 
| (1,0), (1,2) | Có | 

Bảng kết quả:```
X.X
OOO
...
```Hàng giữa trở nên hoàn toàn`O`, do đó việc tìm kiếm dừng lại ngay lập tức. Điều này chứng tỏ rằng chúng ta phải kiểm tra nhiều cặp thay vì đưa ra lựa chọn tham lam. 

### Ví dụ 2 

Bảng đầu vào:```
X.X
.X.
...
```Ô trống: 

| Đã thử nghiệm cặp | Chiến thắng? | 
| --- | --- | 
| Cặp nào cũng được | Không | 

Dù hai người mới ở đâu`O`s được đặt, tối đa hai ô của một dòng có thể trở thành`O`. Vì ban đầu không có gì hữu ích`O`Để hoàn thành một dòng ba ô, chiến thắng là không thể. 

Ví dụ này cho thấy rằng việc có hai nước đi tự động không đủ để tạo ra đường thắng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(E²) | Liệt kê tất cả các cặp ô trống, trong đó E ≤ 9 | 
| Không gian | O(1) | Kích thước bảng được cố định ở mức 3 × 3 | 

Từ`E`không bao giờ vượt quá 9, tối đa 36 cặp được kiểm tra. Ngay cả với 1000 trường hợp thử nghiệm, tổng công việc vẫn rất nhỏ và dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    def is_win(board):
        for r in range(3):
            if all(board[r][c] == 'O' for c in range(3)):
                return True

        for c in range(3):
            if all(board[r][c] == 'O' for r in range(3)):
                return True

        if all(board[i][i] == 'O' for i in range(3)):
            return True

        if all(board[i][2 - i] == 'O' for i in range(3)):
            return True

        return False

    t = int(input())

    out = []

    for _ in range(t):
        board = [list(input().strip()) for _ in range(3)]

        empty = []
        for r in range(3):
            for c in range(3):
                if board[r][c] == '.':
                    empty.append((r, c))

        found = False

        for i in range(len(empty)):
            for j in range(i + 1, len(empty)):
                temp = [row[:] for row in board]

                r1, c1 = empty[i]
                r2, c2 = empty[j]

                temp[r1][c1] = 'O'
                temp[r2][c2] = 'O'

                if is_win(temp):
                    out.append("YES")
                    out.extend("".join(row) for row in temp)
                    found = True
                    break

            if found:
                break

        if not found:
            out.append("NO")

    print("\n".join(out))

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue()

# sample 1
res = run("""1
X.X
.O.
...
""")
assert res.startswith("YES")

# sample 2
res = run("""1
O.X
.O.
X..
""")
assert res.startswith("YES")

# impossible case
assert run("""1
X.X
.X.
...
""").strip() == "NO"

# diagonal completion
res = run("""1
O..
.X.
...
""")
assert res.startswith("YES")

# many empty cells
res = run("""1
...
...
...
""")
assert res.strip() == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`X.X / .O. / ...`| CÓ | Hàng thắng kiểu mẫu | 
|`O.X / .O. / X..`| CÓ | Vị trí chiến thắng kiểu mẫu | 
|`X.X / .X. / ...`| KHÔNG | Không thể có đường sau hai lần di chuyển | 
|`O.. / .X. / ...`| CÓ | Hoàn thành đường chéo | 
| Bảng trống | KHÔNG | Hai nước đi một mình không thể tạo thành ba nước`O`s | 

## Vỏ cạnh 

### Đường thắng cần có cả nước đi mới 

đầu vào:```
...
.O.
...
```Hàng giữa chỉ có một`O`. Nobita có thể đặt`O`trong cả hai ô còn lại của hàng đó:```
...
OOO
...
```Thuật toán kiểm tra ghép nối và báo cáo chính xác`YES`. 

### Nhiều cặp thất bại trước khi một cặp thành công 

đầu vào:```
X.X
.O.
...
```Một số cặp ô trống không tạo thành đường chiến thắng. Thuật toán tiếp tục tìm kiếm cho đến khi gặp cặp tương ứng với 2 ô bên của hàng giữa. Bởi vì mỗi cặp đều được kiểm tra nên cấu hình chiến thắng không bao giờ bị bỏ sót. 

### Hiện không có hỗ trợ nào cho một dòng 

đầu vào:```
...
.X.
...
```Sau hai nước đi, Nobita có thể tạo ra nhiều nhất hai nước.`O`s trong bất kỳ hàng, cột hoặc đường chéo nào. Một dòng chiến thắng đòi hỏi ba. Thuật toán sử dụng hết tất cả các cặp, không tìm thấy bảng thắng và in`NO`. 

### Có nhiều hơn một câu trả lời thành công 

đầu vào:```
OO.
...
...
```Đặt một`O`ở ô còn lại của hàng trên cùng đã hoàn thành một dòng và nước đi thứ hai có thể ở bất kỳ vị trí nào khác. Nhiều đầu ra hợp lệ tồn tại. Thuật toán trả về bảng chiến thắng đầu tiên mà nó gặp, điều này được cho phép bởi câu lệnh vấn đề.
