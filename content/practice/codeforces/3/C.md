---
title: "CF 3C - Tic-tac-toe"
description: "Chúng ta được cung cấp một ảnh chụp nhanh của bảng tic-tac-toe, được biểu thị dưới dạng lưới 3 × 3. Mỗi ô trống (.), chứa dấu thập (X) hoặc không chứa giá trị nào (0). Người chơi đầu tiên luôn đặt những quả tạt, còn người chơi thứ hai không đặt quả thánh giá nào."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "games", "implementation"]
categories: ["algorithms"]
codeforces_contest: 3
codeforces_index: "C"
codeforces_contest_name: "Codeforces Beta Round 3"
rating: 1800
weight: 3
solve_time_s: 277
verified: true
draft: false
---
[CF 3C - Tic-tac-toe](https://codeforces.com/problemset/problem/3/C) 

**Đánh giá:** 1800 
**Tags:** bạo lực, trò chơi, thực hiện 
**Thời gian giải:** 4 phút 37 giây 
**Đã xác minh:** có 

## Hiểu vấn đề 

Chúng ta được cung cấp một ảnh chụp nhanh của bảng tic-tac-toe, được biểu thị dưới dạng lưới 3 × 3. Mỗi ô đều trống (`.`), chứa một chữ thập (`X`) hoặc không chứa giá trị nào (`0`). Người chơi đầu tiên luôn đặt những quả tạt, còn người chơi thứ hai không đặt quả thánh giá nào. Bảng có thể được lấp đầy một phần, được lấp đầy hoàn toàn hoặc thậm chí ở trạng thái không thể. 

Nhiệm vụ là xác định trạng thái của trò chơi. Chúng ta có thể đưa ra một trong sáu kết quả có thể xảy ra: người chơi thứ nhất thắng, người chơi thứ hai thắng, đến lượt người chơi thứ nhất tiếp theo, đến lượt người chơi thứ hai tiếp theo, trò chơi hòa hoặc bàn cờ là bất hợp pháp. Các bảng bất hợp pháp xảy ra khi vị trí của X và 0 không phù hợp với quy tắc, chẳng hạn như nếu người chơi thứ hai đã chơi nhiều lần hơn người chơi đầu tiên hoặc cả hai người chơi đều có đường thắng cùng một lúc. 

Các ràng buộc rất nhỏ: một bảng 3 × 3 cố định. Điều này cho chúng ta biết rằng bất kỳ giải pháp nào lặp lại trên các ô với số lần không đổi sẽ chạy tốt trong giới hạn thời gian 1 giây. Không cần tối ưu hóa dựa trên n vì n luôn bằng 9 hoặc nhỏ hơn. Các trường hợp đặc biệt rất phức tạp vì việc kiểm tra ngây thơ “ai có nhiều X hơn” có thể không phát hiện được các trạng thái bất hợp pháp trong đó cả hai người chơi dường như đều có đường thắng hoặc khi số đếm không khớp với thứ tự lượt chơi. Ví dụ:```
XXX
000
...
```Ở đây cả hai người chơi dường như đều có đường thắng. Đầu ra đúng là`illegal`. Việc triển khai bất cẩn để kiểm tra chiến thắng một cách độc lập mà không xác thực số lượt quay sẽ trả về không chính xác`first player won`hoặc`second player won`. Một trường hợp tế nhị khác là:```
X0X
0X0
X..
```Ở đây người chơi đầu tiên có đường thắng dọc theo cột đầu tiên nhưng số lượng X và 0 vẫn phải phù hợp với người chơi đầu tiên đi trước. Kết quả đúng là`the first player won`, không`illegal`. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ xem xét việc tạo ra mọi chuỗi di chuyển có thể dẫn đến bàn cờ nhất định và kiểm tra xem nó có hợp pháp hay không. Mỗi lượt có tối đa 9 tùy chọn ban đầu, 8 tùy chọn ở lượt tiếp theo, v.v. Đây là độ phức tạp giai thừa (9!) Và không cần thiết do kích thước cố định nhỏ của bảng. Nó sẽ đúng nhưng quá cồng kềnh. 

Cái nhìn sâu sắc về cách tiếp cận tối ưu là tính hợp pháp và trạng thái trò chơi có thể được xác định hoàn toàn bởi hai yếu tố: số lượng X và 0, và sự tồn tại của các đường thắng. Một bảng là hợp pháp nếu số X bằng số 0 hoặc nhiều hơn số 0 một. Nếu có vạch thắng thì người thắng phải là người chơi cuối cùng và số đếm phải căn chỉnh: X chỉ thắng nếu X lớn hơn 0 một; 0 chỉ thắng nếu X bằng 0. Các bảng có cả X và 0 đều có đường thắng luôn là bất hợp pháp. Một khi tính hợp pháp đã được thiết lập, việc xác định lượt tiếp theo của ai là chuyện nhỏ bằng cách so sánh số lượng. Một trận hòa xảy ra nếu bảng đầy và không có người chơi nào thắng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(9!) | O(1) | Quá chậm/không cần thiết | 
| Tối ưu | O(1) | O(1) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đếm số chữ X và số 0 trên bảng. Hãy để những điều này được`count_X`Và`count_0`. Việc đếm xác định đến lượt ai. Nếu như`count_0 > count_X`hoặc`count_X - count_0 > 1`, hội đồng quản trị là bất hợp pháp. Điều này đảm bảo thứ tự rẽ được tôn trọng. 
2. Kiểm tra các đường thắng. Xác định đường chiến thắng là ba biểu tượng giống hệt nhau trong một hàng, cột hoặc đường chéo. Lặp lại trên tất cả các hàng, cột và cả hai đường chéo, đánh dấu xem X có thắng hay không và 0 có thắng hay không. 
3. Nếu cả X và 0 đều có đường thắng đồng thời thì bàn cờ đó là bất hợp pháp. Tic-tac-toe không thể đạt đến trạng thái cả hai người chơi đều hoàn thành đường thắng trên cùng một bảng vì trò chơi kết thúc ngay lập tức khi một người chơi thắng. 
4. Nếu X có đường thắng thì kiểm tra xem`count_X == count_0 + 1`. Nếu điều này là sai, hội đồng quản trị là bất hợp pháp. Nếu đúng thì xuất ra`the first player won`. 
5. Nếu số 0 có đường thắng thì kiểm tra xem`count_X == count_0`. Nếu điều này là sai, hội đồng quản trị là bất hợp pháp. Nếu đúng thì xuất ra`the second player won`. 
6. Nếu không có ai thắng, hãy kiểm tra xem bảng đã đầy chưa. Nếu đầy, xuất`draw`. Nếu chưa đầy, hãy xác định đến lượt của ai: nếu`count_X == count_0`, đến lượt người chơi đầu tiên; nếu như`count_X == count_0 + 1`, đến lượt người chơi thứ hai. 

Tại sao nó hoạt động: Thuật toán này duy trì tính bất biến rằng mọi quyết định đều phù hợp với quy tắc của tic-tac-toe: lượt luân phiên bắt đầu bằng X, trò chơi kết thúc ngay lập tức khi có người thắng và số lượng biểu tượng trên bảng nhất quán với người chiến thắng. Bằng cách kiểm tra số lượng và dòng chiến thắng theo thứ tự này, tất cả các cấu hình bất hợp pháp đều bị phát hiện. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def check_winner(board, symbol):
    for i in range(3):
        if all(board[i][j] == symbol for j in range(3)):
            return True
        if all(board[j][i] == symbol for j in range(3)):
            return True
    if all(board[i][i] == symbol for i in range(3)):
        return True
    if all(board[i][2-i] == symbol for i in range(3)):
        return True
    return False

board = [input().strip() for _ in range(3)]
count_X = sum(row.count('X') for row in board)
count_0 = sum(row.count('0') for row in board)

x_wins = check_winner(board, 'X')
o_wins = check_winner(board, '0')

if count_0 > count_X or count_X - count_0 > 1:
    print("illegal")
elif x_wins and o_wins:
    print("illegal")
elif x_wins:
    print("the first player won" if count_X == count_0 + 1 else "illegal")
elif o_wins:
    print("the second player won" if count_X == count_0 else "illegal")
elif count_X + count_0 == 9:
    print("draw")
else:
    print("first" if count_X == count_0 else "second")
```Giải pháp đầu tiên đếm X và 0 để xác thực thứ tự lần lượt. các`check_winner`hàm đánh giá tất cả các hàng, cột và đường chéo để xác định người chiến thắng. Mỗi quyết định sau đó sẽ trực tiếp thực hiện các quy tắc được mô tả trong phần hướng dẫn thuật toán. 

## Ví dụ đã hoạt động 

**Mẫu 1** 

đầu vào:```
X0X
.0.
.X.
```| Biến | Giá trị | 
| --- | --- | 
| đếm_X | 3 | 
| đếm_0 | 2 | 
| x_wins | Sai | 
| o_wins | Sai | 
| Bảng đầy đủ? | Sai | 

Chưa có người chiến thắng, số lượng là`count_X = count_0 + 1`, vậy là đến lượt người chơi thứ hai. Đầu ra là`second`. 

**Mẫu 2** 

đầu vào:```
XXX
0.0
..0
```| Biến | Giá trị | 
| --- | --- | 
| đếm_X | 3 | 
| đếm_0 | 3 | 
| x_wins | Đúng | 
| o_wins | Sai | 

X có đường thắng nhưng`count_X`không phải`count_0 + 1`. Đầu ra là`illegal`. 

Những dấu vết này cho thấy cách đếm và kiểm tra dòng thực thi tính hợp pháp và xác định lượt hay người chiến thắng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(1) | Đã sửa lỗi lưới 3 × 3, số lần kiểm tra không đổi (3 hàng + 3 cột + 2 đường chéo). | 
| Không gian | O(1) | Chỉ có quầy và bảng lưu trữ, không có cấu trúc bổ sung. | 

Giải pháp vừa vặn thoải mái trong thời gian 1 giây và giới hạn bộ nhớ 64 MB. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import builtins
    from contextlib import redirect_stdout
    out = io.StringIO()
    with redirect_stdout(out):
        # solution
        board = [input().strip() for _ in range(3)]
        count_X = sum(row.count('X') for row in board)
        count_0 = sum(row.count('0') for row in board)

        def check_winner(board, symbol):
            for i in range(3):
                if all(board[i][j] == symbol for j in range(3)):
                    return True
                if all(board[j][i] == symbol for j in range(3)):
                    return True
            if all(board[i][i] == symbol for i in range(3)):
                return True
            if all(board[i][2-i] == symbol for i in range(3)):
                return True
            return False

        x_wins = check_winner(board, 'X')
        o_wins = check_winner(board, '0')

        if count_0 > count_X or count_X - count_0 > 1:
            print("illegal")
        elif x_wins and o_wins:
            print("illegal")
        elif x_wins:
            print("the first player
```
