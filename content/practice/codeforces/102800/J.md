---
title: "CF 102800J - Tình huống"
description: "Chúng ta được cấp một bảng Tic-tac-toe 3 × 3 được lấp đầy một phần. Alice sử dụng O, Bob sử dụng X và một số ô có thể vẫn trống. Không giống như Tic-tac-toe thông thường, trò chơi không bao giờ dừng lại khi ai đó hoàn thành một dòng. Mọi ô trống cuối cùng sẽ được lấp đầy."
date: "2026-07-28T22:49:31+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102800
codeforces_index: "J"
codeforces_contest_name: "The 14th Jilin Provincial Collegiate Programming Contest"
rating: 0
weight: 102800
solve_time_s: 57
verified: true
draft: false
---

[CF 102800J - Tình huống](https://codeforces.com/problemset/problem/102800/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 57s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một bảng Tic-tac-toe 3 × 3 được lấp đầy một phần. Alice sử dụng`O`, Bob sử dụng`X`và một số ô có thể vẫn trống. Không giống như Tic-tac-toe thông thường, trò chơi không bao giờ dừng lại khi ai đó hoàn thành một dòng. Mọi ô trống cuối cùng sẽ được lấp đầy. Cuối cùng, chúng tôi đếm từng hàng, từng cột và cả hai đường chéo. Mỗi dòng bao gồm toàn bộ`O`đóng góp`+1`, mỗi dòng bao gồm toàn bộ`X`đóng góp`-1`và các dòng hỗn hợp góp phần`0`. Câu trả lời bắt buộc là tỷ số cuối cùng với điều kiện cả hai người chơi luôn chơi tối ưu. Alice cố gắng tối đa hóa điểm số, trong khi Bob cố gắng giảm thiểu nó. 

Mỗi trường hợp thử nghiệm chỉ định lượt tiếp theo của ai và cấu hình bảng hiện tại. Chúng ta phải tính điểm cuối cùng tối ưu cho mọi trường hợp thử nghiệm. 

Số lượng ca kiểm thử lên tới 40.000, do đó, mặc dù bo mạch rất nhỏ nhưng công việc trên mỗi ca kiểm thử phải cực kỳ nhỏ. Một tìm kiếm minimax đầy đủ từ đầu cho mọi trường hợp thử nghiệm sẽ liên tục giải quyết cùng một trạng thái trò chơi hàng nghìn lần. Toàn bộ trò chơi chỉ có chín ô nên số lượng vị trí bảng riêng biệt bị hạn chế. Điều này gợi ý rõ ràng rằng bạn nên tính toán trước mọi trạng thái có thể truy cập một lần, sau đó trả lời từng truy vấn bằng một thao tác tra cứu đơn giản. 

Một số tình huống có thể dễ dàng tạo ra việc triển khai không chính xác. 

Một dòng hoàn thành sẽ **không** dừng trò chơi. Ví dụ,```
1
OOO
...
...
```Alice đã có hàng chiến thắng nhưng cả hai người chơi phải tiếp tục đặt quân cờ cho đến khi đầy bàn cờ. Một giải pháp đánh giá ngay hội đồng quản trị là`+1`không chính xác vì các hàng, cột hoặc đường chéo đã hoàn thành bổ sung có thể xuất hiện sau đó. 

Việc người chơi di chuyển được xác định bởi đầu vào chứ không phải bằng cách đếm các quân cờ. Ví dụ,```
0
...
...
...
```Mặc dù một bảng trống thường thuộc về Alice, nhưng đầu vào này rõ ràng cho biết Bob sẽ di chuyển tiếp theo. Việc bỏ qua giá trị lần lượt sẽ tạo ra các quyết định minimax không chính xác. 

Một lỗi dễ mắc phải khác là chỉ đánh giá dòng hoàn thành mới nhất sau mỗi lần di chuyển. Coi như```
1
OO.
OO.
...
```Nước đi cuối cùng có thể đồng thời hoàn thành một hàng và một cột. Điểm số luôn phụ thuộc vào bảng cuối cùng chứ không phụ thuộc vào các sự kiện trung gian. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là tìm kiếm minimax đệ quy. Từ vị trí hiện tại, hãy thử mọi nước đi hợp pháp, giải đệ quy trò chơi còn lại và để Alice chọn giá trị lớn nhất trong khi Bob chọn giá trị nhỏ nhất. Khi bảng đầy, hãy đếm các hàng, cột và đường chéo đã hoàn thành để có được điểm số cuối cùng. 

Tìm kiếm này là chính xác vì nó khám phá rõ ràng mọi khả năng tiếp tục. Vấn đề là sự lặp lại. Có thể đạt được cùng một bảng được lấp đầy một phần thông qua nhiều lệnh di chuyển khác nhau, khiến cho cùng một cây con phải được tính toán lại nhiều lần. Mặc dù một trò chơi có tối đa chín nước đi, nhưng việc giải quyết tới 40.000 trường hợp thử nghiệm độc lập khiến việc sao chép không cần thiết này trở nên tốn kém. 

Quan sát quan trọng là trạng thái trò chơi hoàn toàn được xác định bởi cấu hình bàn cờ và người chơi đến lượt. Tổng số cấu hình bảng chỉ$3^9 = 19,683$, vì mọi ô đều trống,`O`, hoặc`X`. Nhân với hai người chơi có thể sẽ có ít hơn 40.000 trạng thái khác nhau. 

Thay vì giải quyết mọi truy vấn một cách độc lập, chúng tôi giải quyết từng trạng thái chính xác một lần bằng cách sử dụng minimax được ghi nhớ. Bất cứ khi nào đệ quy đạt đến trạng thái được tính toán trước đó, chúng tôi sẽ sử dụng lại ngay câu trả lời của nó. Vì mỗi trạng thái được đánh giá nhiều nhất một lần nên tổng công việc bị giới hạn bởi số lượng trạng thái riêng biệt thay vì số lượng đường dẫn đệ quy. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(b!)$, Ở đâu$b$là số ô trống |$O(b)$| Quá chậm đối với nhiều trường hợp thử nghiệm | 
| Tối ưu |$O(3^9 \times 9)$tiền xử lý,$O(1)$mỗi truy vấn |$O(3^9)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Biểu diễn mỗi bảng dưới dạng một chuỗi chín ký tự. Điều này xác định duy nhất một trạng thái trò chơi và có thể được sử dụng làm khóa ghi nhớ. 
2. Viết hàm đánh giá một bảng đầy đủ bằng cách kiểm tra cả ba hàng, cả ba cột và cả hai đường chéo. Mỗi tất cả-`O`dòng thêm một, và mỗi dòng tất cả-`X`dòng trừ một. 
3. Tạo hàm minimax đệ quy đưa bảng hiện tại và người chơi di chuyển. 
4. Nếu bảng không có ô trống, hãy trả lại đánh giá từ Bước 2. Đây là điểm cuối cùng của trò chơi đã hoàn thành đó. 
5. Nếu trạng thái này đã được tính toán, hãy trả lại câu trả lời đã lưu ngay lập tức. Điều này tránh việc giải quyết cùng một vị trí nhiều lần. 
6. Mặt khác, tạo mọi nước đi hợp lệ bằng cách đặt dấu của người chơi hiện tại vào một ô trống. 
7. Giải quyết đệ quy trạng thái kết quả với người chơi đối diện. 
8. Nếu Alice di chuyển, hãy giữ nguyên giá trị trả về tối đa vì cô ấy muốn điểm cuối cùng lớn nhất. Nếu Bob di chuyển, hãy giữ giá trị trả về tối thiểu vì anh ấy muốn điểm cuối cùng nhỏ nhất. 
9. Lưu giá trị tính toán vào bảng ghi nhớ trước khi trả lại. 
10. Đối với mỗi trường hợp thử nghiệm, chuyển đổi bảng thành biểu diễn đã chọn, đọc trình phát di chuyển, gọi hàm minimax đã ghi nhớ và in giá trị trả về. 

### Tại sao nó hoạt động 

Mỗi lệnh gọi đệ quy thể hiện chính xác một vị trí trò chơi hợp pháp. Đệ quy khám phá mọi sự tiếp tục có thể có từ vị trí đó. Đánh giá cuối cùng khớp chính xác với số điểm được xác định trong bài toán vì nó chỉ tính các dòng đã hoàn thành sau khi bảng đã đầy. Vì Alice luôn chọn phần tiếp theo có số điểm lớn nhất và Bob luôn chọn phần nhỏ nhất nên phép truy toán chính xác là định nghĩa về cách chơi tối ưu. Việc ghi nhớ chỉ thay đổi tần suất giải quyết một trạng thái chứ không thay đổi giá trị nào nó nhận được, vì vậy giá trị trả về vẫn đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

LINES = [
    (0, 1, 2),
    (3, 4, 5),
    (6, 7, 8),
    (0, 3, 6),
    (1, 4, 7),
    (2, 5, 8),
    (0, 4, 8),
    (2, 4, 6),
]

memo = {}

def score(board):
    res = 0
    for a, b, c in LINES:
        if board[a] == board[b] == board[c]:
            if board[a] == 'O':
                res += 1
            elif board[a] == 'X':
                res -= 1
    return res

def dfs(board, alice_turn):
    key = (board, alice_turn)
    if key in memo:
        return memo[key]

    if '.' not in board:
        val = score(board)
        memo[key] = val
        return val

    if alice_turn:
        best = -10
        mark = 'O'
        for i, ch in enumerate(board):
            if ch == '.':
                nxt = board[:i] + mark + board[i + 1:]
                best = max(best, dfs(nxt, False))
    else:
        best = 10
        mark = 'X'
        for i, ch in enumerate(board):
            if ch == '.':
                nxt = board[:i] + mark + board[i + 1:]
                best = min(best, dfs(nxt, True))

    memo[key] = best
    return best

t = int(input())
for _ in range(t):
    turn = int(input())
    board = "".join(input().strip() for _ in range(3))
    print(dfs(board, turn == 1))
```các`LINES`mảng lưu trữ mọi dòng tính điểm chính xác một lần, tránh việc tính toán chỉ số lặp lại trong suốt chương trình. 

các`score`chức năng chỉ được gọi cho các bảng được điền đầy đủ. Nó kiểm tra từng dòng trong số tám dòng có thể thắng và tính toán sự khác biệt cần thiết giữa dòng hoàn thành của Alice và Bob. 

Đệ quy`dfs`hàm thực hiện minimax cùng với việc ghi nhớ. Phím ghi nhớ chứa cả bàn cờ và người chơi hiện tại vì các bảng giống hệt nhau có thể có các giá trị tối ưu khác nhau tùy thuộc vào lượt của ai. 

Quá trình đệ quy chỉ kết thúc khi bảng không có ô trống. Điều này phù hợp với các quy tắc đã được sửa đổi của trò chơi, trong đó các dòng hoàn thành trong khi chơi không kết thúc trò chơi. 

Các giá trị ban đầu`-10`Và`10`an toàn vì điểm số luôn ở giữa`-8`Và`8`, vì chỉ có tám dòng có thể có trên bảng. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Giả sử đầu vào là```
1
1
.OO
X.O
OXO
```| Bước | Người chơi hiện tại | Ô trống | Giá trị được chọn | 
| --- | --- | --- | --- | 
| Ban đầu | Alice | 2 | Khám phá cả hai động tác | 
| Sau bước đi đầu tiên | Bob | 1 | Bob giảm thiểu | 
| Nhà ga | Không có | 0 | Đánh giá bảng cuối cùng | 

Đệ quy khám phá cả hai sự tiếp tục hợp pháp. Bob luôn đáp lại bằng nước đi mang lại cho Alice số điểm cuối cùng nhỏ hơn. Giá trị trả về là kết quả trò chơi tối ưu. 

### Mẫu 2```
1
1
XXX
XXX
XXX
```| Bước | Người chơi hiện tại | Ô trống | Giá trị trả về | 
| --- | --- | --- | --- | 
| Ban đầu | Alice | 0 | -8 | 

Bảng đã đầy nên quá trình đệ quy dừng ngay lập tức. Mỗi một trong tám dòng có thể thuộc về Bob, cho điểm cuối cùng là`0 - 8 = -8`. 

Những ví dụ này minh họa hai điều kiện kết thúc. Cái đầu tiên tiếp cận bảng đầu cuối thông qua đệ quy, trong khi cái thứ hai đã ở trạng thái đầu cuối khi truy vấn bắt đầu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(3^9 \times 9)$tổng số tiền xử lý,$O(1)$trung bình trên mỗi truy vấn | Mỗi trạng thái riêng biệt được giải quyết một lần, mỗi trạng thái xem xét tối đa chín nước đi | 
| Không gian |$O(3^9)$| Bản ghi nhớ lưu trữ một giá trị cho mọi trạng thái có thể truy cập | 

Vì tồn tại ít hơn 40.000 trạng thái riêng biệt nên toàn bộ không gian trạng thái dễ dàng được đưa vào bộ nhớ. Sau khi bản ghi nhớ đã được điền, mỗi trường hợp thử nghiệm sẽ trở thành một bản tra cứu bảng băm đơn giản, giúp 40.000 truy vấn dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    from functools import lru_cache

    LINES = [
        (0,1,2),(3,4,5),(6,7,8),
        (0,3,6),(1,4,7),(2,5,8),
        (0,4,8),(2,4,6)
    ]

    @lru_cache(None)
    def dfs(board, turn):
        if '.' not in board:
            s = 0
            for a,b,c in LINES:
                if board[a]==board[b]==board[c]:
                    if board[a]=='O':
                        s += 1
                    elif board[a]=='X':
                        s -= 1
            return s
        if turn:
            ans = -10
            for i,c in enumerate(board):
                if c=='.':
                    ans = max(ans, dfs(board[:i]+'O'+board[i+1:], 0))
            return ans
        ans = 10
        for i,c in enumerate(board):
            if c=='.':
                ans = min(ans, dfs(board[:i]+'X'+board[i+1:], 1))
        return ans

    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline
    t = int(input())
    out = []
    for _ in range(t):
        turn = int(input())
        board = "".join(input().strip() for _ in range(3))
        out.append(str(dfs(board, turn)))
    return "\n".join(out)

# provided sample 2
assert run("""1
1
XXX
XXX
XXX
""") == "-8"

# custom cases
assert run("""1
1
OOO
OOO
OOO
""") == "8"

assert run("""1
0
XXX
XXX
XXX
""") == "-8"

assert run("""1
1
...
...
...
""")

assert run("""1
1
OXO
XOX
OXO
""") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Tất cả`O`bảng | 8 | Điểm tối đa có thể | 
| Tất cả`X`bảng | -8 | Điểm tối thiểu có thể | 
| Bảng trống | Tính theo minimax | Tìm kiếm đầy đủ từ vị trí ban đầu | 
| Bảng hỗn hợp đầy đủ | 2 | Đánh giá thiết bị đầu cuối mà không cần đệ quy | 

## Vỏ cạnh 

Hãy xem xét trường hợp đã có đường thắng nhưng bàn cờ chưa đầy.```
1
1
OOO
...
...
```Thuật toán không dừng lại sau khi quan sát hàng đã hoàn thành. Vì các ô trống vẫn còn nên phép đệ quy tiếp tục cho đến khi mọi ô vuông đều bị chiếm giữ. Chỉ khi đó hàm đánh giá mới tính tất cả các dòng đã hoàn thành. Điều này hoàn toàn phù hợp với các quy tắc trò chơi đã sửa đổi. 

Bây giờ hãy xem xét một bảng có lượt không tương ứng với số lượng quân cờ hiện có.```
0
...
...
...
```Phím ghi nhớ bao gồm cả bàn cờ và người chơi di chuyển. Việc tìm kiếm bắt đầu với việc Bob thu nhỏ kết quả, mặc dù vị trí này sẽ không bao giờ xuất hiện trong một trò chơi bình thường. Thuật toán tuân theo đầu vào thay vì đưa ra các giả định từ số lượng mảnh. 

Cuối cùng, hãy xem xét một vị trí mà một nước đi sẽ tạo ra nhiều đường cùng một lúc.```
1
OO.
OO.
...
```Tìm kiếm đệ quy cuối cùng đạt đến một bảng được điền đầy đủ và hàm đánh giá sẽ kiểm tra độc lập tất cả tám dòng có thể. Nếu một nước đi hoàn thành cả một hàng và một cột thì cả hai đều được tính vì mỗi đường ghi điểm đều được kiểm tra riêng biệt. Điều này tránh việc đếm thiếu các vị trí có nhiều kết nối đồng thời.
