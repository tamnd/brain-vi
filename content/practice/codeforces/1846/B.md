---
title: "CF 1846B - Rudolph và Tic-Tac-Toe"
description: "Chúng ta được cung cấp trạng thái cuối cùng là một bảng rất nhỏ, luôn là 3 x 3, dành cho biến thể tic-tac-toe đơn giản hóa với ba ký hiệu khác nhau. Một người chơi đặt X, người khác đặt O và người thứ ba đặt dấu cộng. Một số ô có thể vẫn trống."
date: "2026-06-09T05:53:38+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "implementation", "strings"]
categories: ["algorithms"]
codeforces_contest: 1846
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 883 (Div. 3)"
rating: 800
weight: 1846
solve_time_s: 308
verified: false
draft: false
---

[CF 1846B – Rudolph và Tic-Tac-Toe](https://codeforces.com/problemset/problem/1846/B) 

**Đánh giá:** 800 
**Tags:** vũ lực, thực hiện, dây 
**Thời gian giải:** 5 phút 8 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp trạng thái cuối cùng là một bảng rất nhỏ, luôn là 3 x 3, dành cho biến thể tic-tac-toe đơn giản hóa với ba ký hiệu khác nhau. Một người chơi đặt X, người khác đặt O và người thứ ba đặt dấu cộng. Một số ô có thể vẫn trống. Bàn cờ đã được cấu hình hoàn thiện và chúng tôi được yêu cầu xác định người chơi nào, nếu có, đã tạo thành đường chiến thắng. 

Đường chiến thắng có nghĩa là ba biểu tượng giống hệt nhau xếp thành một hàng thẳng, theo chiều ngang, chiều dọc hoặc đường chéo. Chỉ có một người chiến thắng hoặc không có ai thắng, trong trường hợp đó kết quả là hòa. Chúng tôi cũng đảm bảo rằng hai biểu tượng khác nhau sẽ không bao giờ đồng thời tạo thành các đường chiến thắng trên cùng một bảng. 

Kích thước đầu vào cực lớn trong số trường hợp thử nghiệm, lên tới mười nghìn bảng, nhưng mỗi bảng có kích thước không đổi. Điều này ngay lập tức loại bỏ mọi nhu cầu tối ưu hóa ngoài thời gian cố định trên mỗi bảng. Bất cứ điều gì thực hiện nhiều hơn mức quét cố định 9 ô cho mỗi trường hợp thử nghiệm đều đã quá mức cần thiết. 

Trường hợp cạnh tinh tế đến từ các ô trống và một phần dòng. Ví dụ: một hàng như`X X .`không phải là một chiến thắng mặc dù nó gần như tạo thành một chiến thắng. Một cách khác là kiểm tra chéo, thường bị bỏ qua khi triển khai bất cẩn. Ví dụ, một bảng như```
X . .
. X .
. . X
```phải trả lại X. 

Cạm bẫy triển khai chính là giả định không chính xác rằng chỉ cần đếm các ký hiệu là đủ. Ví dụ: có ba chữ X trên bảng không đảm bảo chiến thắng trừ khi chúng thẳng hàng. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là kiểm tra rõ ràng mọi dòng có thể cho mọi trường hợp thử nghiệm. Vì bàn cờ chỉ có 3 x 3 nên có đúng 8 đường thắng: 3 hàng, 3 cột và 2 đường chéo. Đối với mỗi dòng, chúng tôi kiểm tra xem cả ba ô có chứa cùng một ký hiệu và không trống hay không. Đây là công việc liên tục trên mỗi dòng, vì vậy mỗi bảng mất thời gian không đổi. 

Một cách tiếp cận đơn giản hơn nhưng ít cấu trúc hơn là thử tất cả các tập hợp con của ba ô và xác minh xem chúng có tạo thành một dòng hay không. Điều đó là không cần thiết vì hình dạng của lưới đã được cố định và biết trước. Cách tiếp cận đó vẫn có hiệu quả nhưng đưa ra những kiểm tra dư thừa và việc triển khai khó khăn hơn. 

Quan sát quan trọng là tập hợp các đường thắng là cố định và rất nhỏ. Điều này chuyển đổi vấn đề từ mô phỏng trò chơi thành nhiệm vụ khớp mẫu trực tiếp. Chúng tôi không mô phỏng các bước di chuyển hoặc kiểm tra tính hợp lệ mà chỉ nhận dạng một mẫu tĩnh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (kiểm tra tất cả các bộ ba) | O(t) với hằng số lớn hoặc O(t * 27 kiểm tra) | O(1) | Được chấp nhận nhưng quá phức tạp | 
| Tối ưu (kiểm tra 8 dòng) | O(t) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xác định trước tất cả 8 dòng chiến thắng khi chỉ số tăng gấp ba lần trên một lưới 3 x 3 phẳng. 

1. Chuyển đổi mỗi bảng thành một cấu trúc mà chúng ta có thể lập chỉ mục nhanh chóng, chẳng hạn như danh sách 9 ký tự. Điều này làm cho việc kiểm tra dòng trở nên đơn giản và tránh logic lập chỉ mục chuỗi lặp lại. Bước này quan trọng vì nó giữ cho dòng kiểm tra thống nhất và dễ đọc. 
2. Xác định 8 mô hình chiến thắng: ba hàng, ba cột và hai đường chéo. Mỗi mẫu chỉ là một bộ ba chỉ số trong bảng phẳng. 
3. Đối với mỗi ký hiệu trong`X`,`O`, Và`+`, hãy kiểm tra xem có bất kỳ mẫu nào trong số 8 mẫu có bị biểu tượng đó chiếm giữ hoàn toàn hay không. Điều này được thực hiện bằng cách xác minh rằng cả ba vị trí trong mẫu đều khớp với ký hiệu. 
4. Nếu có chính xác một ký hiệu thỏa mãn điều kiện thắng, hãy xuất ký hiệu đó. 
5. Nếu không có ký hiệu nào thỏa mãn, xuất ra`DRAW`. 

Lý do chúng tôi kiểm tra rõ ràng cả ba ký hiệu một cách riêng biệt là vì không có sự đảm bảo chồng chéo nào mà chúng tôi có thể khai thác ngoài báo cáo vấn đề. Mặc dù nó đảm bảo không có người chiến thắng đồng thời, chúng tôi vẫn phải xác minh từng người một cách độc lập. 

### Tại sao nó hoạt động 

Mọi cấu hình chiến thắng có thể có trong bảng 3 x 3 tic-tac-toe đều tương ứng chính xác với một trong 8 dòng được xác định trước. Không có sự sắp xếp hình học nào khác tạo nên chiến thắng. Vì chúng tôi kiểm tra tất cả các ký hiệu trên tất cả các dòng hợp lệ nên bất kỳ bảng chiến thắng nào cũng phải được phát hiện bằng ít nhất một lần kiểm tra. Ngược lại, một bàn cờ không thắng không thể đáp ứng bất kỳ điều kiện dòng đầy đủ nào, vì vậy nó không thể bị phân loại sai là bàn thắng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

LINES = [
    (0, 1, 2), (3, 4, 5), (6, 7, 8),  # rows
    (0, 3, 6), (1, 4, 7), (2, 5, 8),  # cols
    (0, 4, 8), (2, 4, 6)              # diagonals
]

def winner(board, ch):
    return any(board[a] == board[b] == board[c] == ch for a, b, c in LINES)

def solve():
    t = int(input())
    out = []
    for _ in range(t):
        grid = []
        for _ in range(3):
            grid.append(input().strip())
        board = ''.join(grid)

        if winner(board, 'X'):
            out.append('X')
        elif winner(board, 'O'):
            out.append('O')
        elif winner(board, '+'):
            out.append('+')
        else:
            out.append('DRAW')

    print('\n'.join(out))

if __name__ == "__main__":
    solve()
```Giải pháp làm phẳng mỗi lưới thành một chuỗi có độ dài 9 để việc kiểm tra dòng trở thành so sánh chỉ mục đơn giản. các`LINES`mảng mã hóa tất cả các cấu hình chiến thắng có thể có, loại bỏ mọi nhu cầu về số học tọa độ trong quá trình thực thi. 

các`winner`chức năng là logic cốt lõi. Nó quét tất cả 8 dòng và kiểm tra xem cả ba vị trí có chứa cùng một biểu tượng mục tiêu hay không. Đây là kết quả khớp mẫu trực tiếp, không phải tìm kiếm hay mô phỏng. 

Thứ tự kiểm tra chỉ quan trọng khi hòa, nhưng vấn đề đảm bảo không có người chiến thắng đồng thời, vì vậy việc kiểm tra X, rồi O, rồi + là an toàn và mang tính quyết định. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Bảng đầu vào:```
X O X
O X O
O + X
```Tấm phẳng:`XOXOXOO+X`Chúng tôi đánh giá từng biểu tượng. 

| Biểu tượng | Đã kiểm tra dòng | Kết quả | 
| --- | --- | --- | 
| X | (0,4,8) | X == X == X đúng | 
| Ồ | bất kỳ dòng nào | không có trận đấu đầy đủ | 
| + | bất kỳ dòng nào | không có trận đấu đầy đủ | 

Đầu ra là X. 

Điều này cho thấy cách phát hiện đường chéo được xử lý một cách tự nhiên bởi danh sách dòng được tính toán trước mà không cần viết hoa đặc biệt. 

### Ví dụ 2 

Bảng đầu vào:```
O O O
X + X
. X +
```Tấm phẳng:`OOO X+X .X+`| Biểu tượng | Đã kiểm tra dòng | Kết quả | 
| --- | --- | --- | 
| Ồ | hàng (0,1,2) | tất cả O | 
| X | bất kỳ dòng nào | không có trận đấu đầy đủ | 
| + | bất kỳ dòng nào | không có trận đấu đầy đủ | 

Đầu ra là O. 

Ví dụ này xác nhận rằng sự can thiệp một phần từ các ký hiệu khác không ảnh hưởng đến tính chính xác vì chúng tôi chỉ xác thực tính đồng nhất trên mỗi dòng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(t) | Mỗi test case kiểm tra 8 dòng cố định trên 3 ô | 
| Không gian | O(1) | Chỉ lưu trữ một số lượng chỉ mục và chuỗi tạm thời không đổi | 

Công việc liên tục trên mỗi trường hợp thử nghiệm là cực kỳ nhỏ, do đó, ngay cả mười nghìn bảng cũng được xử lý ngay lập tức trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    LINES = [
        (0, 1, 2), (3, 4, 5), (6, 7, 8),
        (0, 3, 6), (1, 4, 7), (2, 5, 8),
        (0, 4, 8), (2, 4, 6)
    ]

    def winner(board, ch):
        return any(board[a] == board[b] == board[c] == ch for a, b, c in LINES)

    t = int(input())
    out = []
    for _ in range(t):
        grid = [input().strip() for _ in range(3)]
        board = ''.join(grid)

        if winner(board, 'X'):
            out.append('X')
        elif winner(board, 'O'):
            out.append('O')
        elif winner(board, '+'):
            out.append('+')
        else:
            out.append('DRAW')

    return "\n".join(out)

# provided sample (single test case extracted style check omitted full sample due to formatting)
assert run("1\nXOX\nOXO\nOXO\n") == "X"

# custom: empty board
assert run("1\n...\n...\n...\n") == "DRAW"

# custom: X wins row
assert run("1\nXXX\n.O.\n+.+\n") == "X"

# custom: + wins diagonal
assert run("1\n+O.\nO+O\n.O+\n") == "+"

# custom: O wins column
assert run("1\nXO.\nXO.\nXO.\n") == "O"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| bảng trống | VẼ | không phát hiện chiến thắng ngẫu nhiên | 
| hàng thắng | X | phát hiện ngang | 
| thắng chéo | + | độ đúng đường chéo | 
| cột thắng | Ồ | độ chính xác theo chiều dọc | 

## Vỏ cạnh 

Trường hợp bảng trống chứng tỏ rằng thuật toán không suy ra chiến thắng từ số lượng ký hiệu. bảng`"..."`lặp lại ba lần sẽ không tạo ra đường khớp nào trong bất kỳ mẫu nào trong số 8 mẫu, vì vậy tất cả`winner`kiểm tra trả về sai và đầu ra là`DRAW`. 

Một chiến thắng chỉ theo đường chéo chẳng hạn như`+O., O+O, .O+`xác nhận rằng mô hình đường chéo`(0,4,8)`được bao gồm một cách chính xác và kiểm tra đối xứng với`(2,4,6)`. Nếu không có hai mô hình này, chiến thắng của người chơi cộng sẽ bị bỏ lỡ một cách có hệ thống. 

Một cột thắng như:```
X O .
X O .
X O .
```kiểm tra tính nhất quán của chỉ mục theo chiều dọc sau khi làm phẳng. Các chỉ số`(0,3,6)`thể hiện chính xác cột đầu tiên và kiểm tra tính bằng nhau đảm bảo cả ba hàng đều đóng góp chính xác vào cùng một dòng.
