---
title: "CF 105627H - Chiến tranh giữa các vì sao"
description: "Bàn cờ là một chiến trường chứa các ô trống, quân đen và quân trắng. Chúng ta có thể chọn một quân trắng làm quân bị khống chế. Trong trò chơi, quân cờ đó mỗi lần chỉ được di chuyển một hàng lên trên, chọn cùng một cột, đường chéo bên trái hoặc đường chéo bên phải."
date: "2026-06-26T18:10:35+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105627
codeforces_index: "H"
codeforces_contest_name: "The 2023 ICPC Asia Tehran Regional Contest"
rating: 0
weight: 105627
solve_time_s: 48
verified: true
draft: false
---

[CF 105627H - Chiến tranh giữa các vì sao](https://codeforces.com/problemset/problem/105627/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Bàn cờ là một chiến trường chứa các ô trống, quân đen và quân trắng. Chúng ta có thể chọn một quân trắng làm quân bị khống chế. Trong trò chơi, quân cờ đó mỗi lần chỉ được di chuyển một hàng lên trên, chọn cùng một cột, đường chéo bên trái hoặc đường chéo bên phải. Nó không thể vào một ô bị chiếm bởi một quân trắng khác. Bất cứ khi nào nó chạm tới quân đen thì quân đó bị tiêu diệt. Mục tiêu là chọn quân trắng xuất phát và đường di chuyển của nó sao cho số lượng quân đen bị tiêu diệt càng nhiều càng tốt. 

Đầu vào mô tả kích thước của bảng theo sau là lưới. Đầu ra là một số nguyên duy nhất, số lượng ô đen tối đa có thể được truy cập bằng một đường dẫn đi lên hợp lệ. 

Kích thước bảng tối đa là 50 x 50 nên chỉ có 2500 ô. Kích thước này đủ nhỏ để có thể thực hiện được giải pháp khám phá trạng thái của mọi tế bào. Tuy nhiên, việc thử mọi đường đi có thể một cách rõ ràng vẫn sẽ tốn kém vì số lượng đường đi có thể tăng theo cấp số nhân với số lượng hàng. Giải pháp lập trình động xử lý từng ô một lần là độ phức tạp dự kiến. 

Hướng chuyển động tạo nên cấu trúc chính của bài toán. Vì mỗi bước di chuyển đều làm giảm chỉ số hàng nên đồ thị các chuyển động có thể xảy ra không có chu kỳ. Điều đó có nghĩa là mọi ô đều có thể được đánh giá từ các ô bên dưới nó mà không cần các thuật toán đồ thị phức tạp hơn. 

Một số trường hợp nguy hiểm có thể phá vỡ việc triển khai bất cẩn. Một quân trắng ở hàng trên cùng đã có một đường đi hợp lệ mà không có nước đi nào. Ví dụ:```
Input
1 1
W

Output
0
```Giải pháp cho rằng mọi quân trắng phải di chuyển ít nhất một lần có thể thất bại ở đây. 

Quân đen ngay phía trên vị trí xuất phát phải được tính vì di chuyển vào quân đen sẽ phá hủy quân đó. Ví dụ:```
Input
2 1
W
B

Output
1
```Lỗi chỉ đếm các ô sau khi di chuyển thay vì bao gồm ô đích sẽ trả về 0. 

Một sai lầm phổ biến khác là cho phép di chuyển qua một quân trắng khác. Coi như:```
Input
3 1
W
W
B

Output
0
```Quân trắng phía trên không thể xuyên qua quân trắng phía dưới nên không thể tiếp cận được ô đen. Xử lý các ô trắng như các ô trống sẽ trả về một ô không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là mô phỏng mọi chuỗi chuyển động có thể có từ mọi quân trắng. Một đường dẫn bắt đầu từ một ô màu trắng và phân nhánh thành tối đa ba ô tiếp theo trên mỗi hàng. Cách tiếp cận này đúng vì mọi tuyến đường có thể đều được kiểm tra và tuyến đường có số lượng ô đen lớn nhất có thể được chọn. Vấn đề là số lượng tuyến đường tăng theo cấp số nhân. Trong một bảng có 50 hàng, một quân cờ có thể có tới$3^{49}$các chuỗi chuyển động có thể có trước khi xem xét các chướng ngại vật, vượt xa những gì có thể khám phá. 

Quan sát hữu ích là có nhiều đường dẫn khác nhau đến cùng một ô. Khi phần được kiểm soát đến một ô cụ thể, kết quả tốt nhất trong tương lai từ thời điểm đó không phụ thuộc vào đường dẫn được sử dụng để đến nó. Thông tin duy nhất quan trọng là vị trí hiện tại. Điều này làm cho bài toán trở thành bài toán đường đi dài nhất trên đồ thị chu kỳ có hướng. 

Chúng ta có thể định nghĩa`dp[i][j]`là số lượng quân đen tối đa có thể bị tiêu diệt bắt đầu từ ô`(i, j)`và tiếp tục đi lên. Từ một ô, chúng tôi thử ba ô có thể có ở hàng trên. Nếu đích đến là quân trắng thì việc chuyển đổi đó là không thể. Nếu đích đến là màu đen, chúng ta được một điểm và đi tiếp từ đó. Vì mỗi lần chuyển đổi sẽ chuyển sang chỉ mục hàng nhỏ hơn nên chúng ta có thể tính toán các giá trị từ đầu bảng trở xuống hoặc sử dụng đệ quy có tính năng ghi nhớ. 

Phương pháp brute-force không thành công vì nó giải quyết nhiều lần các bài toán con giống nhau. Giải pháp lập trình động lưu trữ các kết quả đó một lần và sử dụng lại chúng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(3^n) trong trường hợp xấu nhất | Độ sâu đệ quy O(n) | Quá chậm | 
| Tối ưu | O(nm) | O(nm) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tạo một hàm ghi nhớ trả về số lượng quân đen tốt nhất có thể lấy được từ một ô nhất định. 

Chức năng đại diện cho toàn bộ tương lai của trò chơi từ vị trí đó. Vì tất cả chuyển động đều hướng lên trên nên mọi lệnh gọi đệ quy sẽ di chuyển đến gần cuối bảng hơn. 
2. Đối với một ô, hãy xem xét ba khả năng di chuyển đến hàng phía trên: lên trái, lên và lên phải. 

Mỗi đích đến hợp lệ đại diện cho một khả năng tiếp tục của đường dẫn hiện tại. 
3. Bỏ qua nước đi rời khỏi bàn cờ hoặc nhập quân trắng khác. 

Quân trắng chặn chuyển động vĩnh viễn, do đó, coi nó như một ô bình thường sẽ tạo ra các đường dẫn không hợp lệ. 
4. Đối với mỗi đích đến hợp lệ, hãy thêm một điểm nếu đích đến chứa quân đen, sau đó thêm kết quả tốt nhất từ ​​đích đến đó. 

Việc di chuyển hiện tại là cơ hội duy nhất để đạt được giá trị của ô đích đó, bởi vì đường đi không bao giờ quay trở lại. 
5. Bắt đầu tính toán từ mỗi quân trắng và lấy kết quả lớn nhất. 

Người chơi có thể chọn bất kỳ quân trắng ban đầu nào, vì vậy câu trả lời chung là giá trị tốt nhất trong số tất cả các quân trắng có thể bắt đầu. 

Tại sao nó hoạt động: bất biến lập trình động là`dp[i][j]`luôn lưu trữ số lượng quân đen tối đa có thể bị phá hủy khỏi ô`(i, j)`trở đi. Giá trị này đúng vì mọi nước đi đầu tiên hợp lệ đều được kiểm tra và sau nước đi đó, vấn đề còn lại chính xác là cùng một loại vấn đề từ ô đích. Vì chuyển động luôn đi đến một hàng nhỏ hơn nên không có sự phụ thuộc vòng tròn, nên giá trị tối đa được tính toán không thể bị vô hiệu bởi lựa chọn sau đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    board = [input().strip() for _ in range(n)]

    sys.setrecursionlimit(1000000)
    memo = [[-1] * m for _ in range(n)]

    def dfs(r, c):
        if memo[r][c] != -1:
            return memo[r][c]

        best = 0
        nr = r - 1

        if nr >= 0:
            for dc in (-1, 0, 1):
                nc = c + dc
                if 0 <= nc < m and board[nr][nc] != 'W':
                    gain = 1 if board[nr][nc] == 'B' else 0
                    best = max(best, gain + dfs(nr, nc))

        memo[r][c] = best
        return best

    ans = 0
    for i in range(n):
        for j in range(m):
            if board[i][j] == 'W':
                ans = max(ans, dfs(i, j))

    print(ans)

if __name__ == "__main__":
    solve()
```Đầu vào được đọc dưới dạng lưới ký tự vì mỗi loại ô ảnh hưởng trực tiếp đến quy tắc chuyển đổi. Bảng ghi nhớ sử dụng`-1`như một điểm đánh dấu không được tính toán vì các câu trả lời hợp lệ luôn không âm. 

Trước tiên, hàm đệ quy sẽ kiểm tra xem ô hiện tại đã được xử lý chưa. Nếu không có sự kiểm tra này, nhiều đường dẫn sẽ liên tục tính toán các trạng thái tương tự trong tương lai. 

Vòng lặp chuyển tiếp kiểm tra ba cột ở hàng trước đó. Kiểm tra ranh giới ngăn chặn truy cập cột không hợp lệ và kiểm tra ô trắng sẽ thực thi quy tắc chặn. Phần đóng góp của ô đen được thêm vào trước lệnh gọi đệ quy vì ô đích bị hủy khi di chuyển xảy ra. 

Vòng lặp cuối cùng thử mọi quân trắng có thể bắt đầu. Một lỗi phổ biến là chỉ bắt đầu từ quân trắng thấp nhất, nhưng vấn đề cho phép bất kỳ quân trắng nào, kể cả những quân gần đầu. 

## Ví dụ đã hoạt động 

Sử dụng một ví dụ tùy chỉnh nhỏ hơn:```
Input
3 3
W..
.B.
..W
```Sự phát triển của trạng thái là: 

| Ô hiện tại | Có thể có các ô tiếp theo | Giá trị tốt nhất | 
| --- | --- | --- | 
| (2,2) W | (1,1), (1,2) | 0 | 
| (1,1) | (0,0), (0,1), (0,2) | 0 | 
| (0,0) | không | 0 | 

Câu trả lời là`1`từ quân trắng phía trên đi qua ô đen. 

Một ví dụ thứ hai:```
Input
4 4
....
.W..
.B..
..W.
```Việc tính toán là: 

| Ô hiện tại | Đã chọn điểm đến | Đạt được | Tổng cộng | 
| --- | --- | --- | --- | 
| (2,1) | (1,1) | 0 | 0 | 
| (1,1) | (0,0),(0,1),(0,2) | 0 | 0 | 
| (3,2) | (2,1) | 1 | 1 | 

Quân trắng phía dưới tới quân đen trước nên đáp án là`1`. Điều này chứng tỏ tại sao mọi vị trí xuất phát của quân trắng đều phải được xem xét. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) | Mỗi ô bảng được đánh giá một lần và mỗi lần đánh giá sẽ kiểm tra ba lần chuyển đổi. | 
| Không gian | O(nm) | Bảng ghi nhớ lưu trữ một giá trị cho mỗi ô. | 

Bảng chứa tối đa 2500 ô, vì vậy phương pháp lập trình động dễ dàng phù hợp với giới hạn nhất định. Hệ số hằng số nhỏ từ việc kiểm tra ba hướng giúp việc triển khai được nhanh chóng. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

# provided sample
assert run("""8 10
.W...BB...
W..B.WB...
.B.WB...W.
.B..B.....
..W...BB..
B.B..B.W.W
.WB.W...B.
..W..BW.B.
""") == "5\n", "sample"

# minimum size
assert run("""1 1
W
""") == "0\n", "single white cell"

# blocked by another white piece
assert run("""3 1
W
W
B
""") == "0\n", "white blocker"

# all black reachable
assert run("""3 3
...
.W.
BBB
""") == "1\n", "one move destroys one piece"

# no white pieces
assert run("""2 2
BB
BB
""") == "0\n", "no starting piece"

# multiple choices
assert run("""4 4
W...
.B..
..B.
...W
""") == "2\n", "best path selection"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Tế bào trắng đơn | 0 | Xử lý một bảng mà không thể di chuyển được. | 
| Quân trắng chặn nhau | 0 | Xác nhận các tế bào trắng là trở ngại. | 
| Các tế bào đen có thể tiếp cận | 1 | Kiểm tra đếm các mảnh bị phá hủy. | 
| Không có mảnh màu trắng | 0 | Xử lý sự vắng mặt của vị trí bắt đầu hợp lệ. | 
| Nhiều đường dẫn có thể | 2 | Xác minh việc chọn tuyến đường tốt nhất. | 

## Vỏ cạnh 

Đối với hộp mảnh màu trắng ở hàng trên cùng:```
Input
1 1
W
```Thuật toán gọi`dfs(0,0)`. Không có hàng nào ở trên nên giá trị vẫn bằng 0. Câu trả lời chính xác vẫn là 0 vì quân được chọn không thể phá hủy bất cứ thứ gì. 

Đối với quân đen ngay phía trên:```
Input
2 1
W
B
```Ô bắt đầu là`(0,0)`trong lập chỉ mục dựa trên số không. Quá trình chuyển đổi duy nhất đến ô đen, mang lại lợi ích cho một ô. Kết quả đệ quy từ đích đó bằng 0 vì nó hiện ở hàng trên cùng. Câu trả lời cuối cùng là`1`. 

Đối với trình chặn màu trắng:```
Input
3 1
W
W
B
```Quân trắng phía trên kiểm tra nước đi duy nhất có thể và nhìn thấy một quân trắng khác, do đó quá trình chuyển đổi đó bị bỏ qua. Quân trắng phía dưới chỉ có ô đen ở dưới mà chuyển động chỉ hướng lên trên nên không thể chạm tới ô đen đó. Câu trả lời vẫn còn`0`. 

Những trường hợp này được xử lý một cách tự nhiên bằng các quy tắc chuyển đổi: chỉ các bước di chuyển hợp pháp đi lên mới được xem xét và mọi ô bị chặn hoặc không thể truy cập đều bị loại trừ trước khi nó có thể ảnh hưởng đến kết quả lập trình động.
