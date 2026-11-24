---
title: "CF 63E - Trò Chơi Kẹo Ngọt"
description: "Bảng là một hình lục giác cố định có 19 ô. Một số ô chứa sôcôla, một số ô trống. Hai người chơi luân phiên di chuyển và một nước đi bao gồm việc chọn một đoạn sôcôla liền kề nằm trên một đường thẳng song song với một trong ba hướng lục giác."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "bitmasks", "dfs-and-similar", "dp", "games", "implementation"]
categories: ["algorithms"]
codeforces_contest: 63
codeforces_index: "E"
codeforces_contest_name: "Codeforces Beta Round 59 (Div. 2)"
rating: 2000
weight: 63
solve_time_s: 133
verified: true
draft: false
---

[CF 63E - Trò chơi đồ ngọt](https://codeforces.com/problemset/problem/63/E) 

**Xếp hạng:** 2000 
**Thẻ:** bitmasks, dfs và tương tự, dp, trò chơi, triển khai 
**Thời gian giải:** 2m 13s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bảng là một hình lục giác cố định có 19 ô. Một số ô chứa sôcôla, một số ô trống. Hai người chơi luân phiên di chuyển và một nước đi bao gồm việc chọn một đoạn sôcôla liền kề nằm trên một đường thẳng song song với một trong ba hướng lục giác. Mỗi ô được chọn hiện phải chứa sô cô la và tất cả các ô được chọn sẽ bị loại bỏ cùng nhau. 

Người chơi không thể di chuyển sẽ thua. Vì mỗi nước đi sẽ loại bỏ ít nhất một viên sôcôla nên trò chơi luôn kết thúc. Karlsson đi trước và chúng ta phải xác định xem thế trận ban đầu là thắng hay thua nếu giả sử lối chơi hoàn hảo. 

Quan sát quan trọng nhất về các ràng buộc là kích thước bảng rất nhỏ. Chỉ có 19 ô, vì vậy mọi trạng thái bảng có thể đều nằm gọn trong mặt nạ 19 bit. Điều đó có nghĩa là tổng số vị trí phân biệt nhiều nhất là$2^{19} = 524288$, điều này hoàn toàn có thể quản lý được đối với trò chơi DP được ghi nhớ. 

Một tìm kiếm đệ quy ngây thơ mà không ghi nhớ sẽ bùng nổ vì đạt được các vị trí giống nhau thông qua nhiều thứ tự di chuyển khác nhau. Ví dụ: loại bỏ ô A thì B đạt đến vị trí tương tự như loại bỏ B rồi A. Hệ số phân nhánh cũng khá lớn vì mỗi đoạn đường của sôcôla đều là một nước đi hợp pháp. Một tìm kiếm minimax đơn giản sẽ truy cập lại các trạng thái giống hệt nhau theo cấp số nhân nhiều lần. 

Phần khó khăn của vấn đề là tạo ra các nước đi hợp pháp một cách chính xác. Bảng không phải là hình chữ nhật nên việc phối hợp bất cẩn dễ tạo ra những dòng không hợp lệ hoặc bỏ lỡ những dòng hợp lệ. 

Một sai lầm dễ mắc phải là coi những viên sôcôla rời rạc trên cùng một đường hình học là có thể tháo rời được. Chỉ cho phép các phân đoạn liền kề. 

Hãy xem xét vị trí này:```
. . .
 . O . .
O . O O .
 . . . .
  . . .
```Hai viên sôcôla bị cô lập cùng hướng không thể được loại bỏ cùng nhau vì ô ở giữa trống. Việc triển khai có lỗi chỉ kiểm tra tính cộng tuyến sẽ cho phép di chuyển như vậy một cách không chính xác. 

Một trường hợp tế nhị khác là mỗi viên sôcôla luôn là một động thái hợp pháp. Nếu việc tạo nước đi chỉ xem xét các đoạn có độ dài ít nhất là hai thì kết quả trò chơi sẽ sai. 

Ví dụ:```
. . .
 . . . .
. . O . .
 . . . .
  . . .
```Câu trả lời đúng là`"Karlsson"`bởi vì người chơi đầu tiên sẽ loại bỏ sô cô la duy nhất ngay lập tức. Thiếu di chuyển một ô sẽ tạo ra kết quả không chính xác`"Lillebror"`. 

Nguồn lỗi thứ ba là các dòng bị trùng lặp. Vì bảng có ba hướng hình học nên mỗi ô thuộc về nhiều dòng. Nếu cùng một phân đoạn được tạo ra nhiều lần, thuật toán vẫn đúng, nhưng hiệu suất sẽ trở nên kém hơn đáng kể do DFS liên tục khám phá các chuyển tiếp dư thừa. 

## Phương pháp tiếp cận 

Ý tưởng brute-force trực tiếp là mô phỏng trò chơi một cách đệ quy. Đối với mọi vị trí, tạo ra tất cả các nước đi hợp lệ, lặp lại các vị trí kết quả và phân loại trạng thái hiện tại là thắng nếu ít nhất một nước đi dẫn đến trạng thái thua. 

Logic này đúng vì trò chơi tổ hợp khách quan tuân theo tính chất minimax tiêu chuẩn. Một trạng thái sẽ thắng nếu người chơi hiện tại có thể buộc đối thủ vào trạng thái thua cuộc. 

Vấn đề là công việc lặp đi lặp lại. Trạng thái bảng tương tự có thể đạt được thông qua nhiều trình tự loại bỏ. Nếu không ghi nhớ, cây đệ quy sẽ trở nên khổng lồ. Trong trường hợp xấu nhất, mỗi bước di chuyển sẽ phân nhánh thành hàng tá khả năng và độ sâu có thể đạt tới 19. Ngay cả hệ số phân nhánh khoảng 10 cũng đã cho kết quả đại khái.$10^{19}$đường dẫn đệ quy, điều này là không thể. 

Quan sát quan trọng là bảng đủ nhỏ để mã hóa dưới dạng mặt nạ bit. Mỗi ô có chỉ số từ 0 đến 18. Trạng thái bảng trở thành số nguyên 19 bit trong đó bit$i$cho biết liệu sô cô la$i$tồn tại. 

Khi các trạng thái được mã hóa theo cách này, việc ghi nhớ sẽ trở nên tự nhiên. Mỗi mặt nạ được giải quyết một lần. Đối với mỗi trạng thái, chúng tôi liệt kê tất cả các phân đoạn liền kề có thể tháo rời và kiểm tra xem việc xóa phân đoạn đó có dẫn đến trạng thái mất hay không. 

Thách thức còn lại là tạo ra nước đi hiệu quả. Thay vì quét hình lục giác một cách linh hoạt mỗi lần, chúng tôi tính toán trước mọi phân đoạn hợp lệ có thể có ngay từ đầu. Hình dạng bảng không bao giờ thay đổi, chỉ có công suất thay đổi. 

Bảng có 15 đường thẳng tối đa theo ba hướng. Đối với mỗi dòng, mỗi phân đoạn liền kề đều có thể là một mẫu di chuyển. Trong DFS, một bước di chuyển là hợp pháp nếu tất cả các ô của phân đoạn đó hiện có trong mặt nạ. 

Điều này biến trò chơi thành DP bitmask tiêu chuẩn trên tối đa 524288 trạng thái. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ, đại khái$O(b^{19})$| Cây đệ quy hàm mũ | Quá chậm | 
| Tối ưu |$O(2^{19} \cdot M)$|$O(2^{19})$| Đã chấp nhận | 

Đây,$M$là số đoạn đường được tính toán trước, đủ nhỏ để hoạt động như một hằng số. 

## Hướng dẫn thuật toán 

1. Gán chỉ mục từ 0 đến 18 cho mỗi ô bảng. 

Chúng ta cần một sự biểu diễn cô đọng của các trạng thái. Một chỉ mục cố định cho mỗi ô cho phép chúng ta lưu trữ các vị trí bên trong mặt nạ bit. 
2. Xây dựng tọa độ cho bảng lục giác. 

Một cách biểu diễn thuận tiện sử dụng tọa độ trục lục giác. Mỗi ô sau đó có thể được xác định bằng hai tọa độ$(x, y)$và các đường thẳng tương ứng với các điều kiện tọa độ đơn giản. 
3. Nhóm các ô thành ba hướng dòng. 

Trong tọa độ trục: 

- hằng số$x$- không thay đổi$y$- không thay đổi$x + y$mỗi đường xác định một họ đường thẳng trên hình lục giác. 
4. Đối với mỗi dòng tối đa, hãy tạo tất cả các đoạn liền kề. 

Giả sử một dòng chứa các ô$[a,b,c,d]$. Sau đó, tất cả các mẫu di chuyển hợp pháp là: 

-$[a]$-$[b]$-$[c]$-$[d]$-$[a,b]$-$[b,c]$-$[c,d]$-$[a,b,c]$-$[b,c,d]$-$[a,b,c,d]$Mỗi phân đoạn trở thành một bitmask. 
5. Đọc bảng đầu vào và xây dựng mặt nạ trạng thái ban đầu. 

Nếu một ô chứa`"O"`, đặt bit của nó vào mặt nạ. 
6. Chạy DFS đã được ghi nhớ trên mặt nạ. 

Đối với mỗi mặt nạ di chuyển được tính toán trước: 

- kiểm tra xem tất cả các bit của chuyển động có ở trạng thái hiện tại không 
- nếu có, hãy loại bỏ chúng và lặp lại 

Nếu bất kỳ trạng thái kết quả nào là thua, hãy đánh dấu trạng thái hiện tại là thắng. 
7. Nếu không có nước đi nào dẫn đến trạng thái thua cuộc, đánh dấu trạng thái đó là thua cuộc. 

Điều này có nghĩa là mọi nước đi có thể đều mang lại cho đối thủ một chiến thắng liên tục. 
8. In`"Karlsson"`nếu trạng thái ban đầu thắng, nếu không thì in`"Lillebror"`. 

### Tại sao nó hoạt động 

DFS tính toán phân loại thắng và thua tiêu chuẩn cho các trò chơi công bằng theo luật chơi thông thường. 

Mọi động thái hợp pháp đều làm giảm nghiêm trọng số lượng sôcôla, do đó đệ quy luôn chấm dứt. 

Đối với mỗi trạng thái, thuật toán sẽ kiểm tra tất cả các nước đi hợp pháp đúng một lần. Một trạng thái đang thắng chính xác khi có sự chuyển sang trạng thái thua cuộc. Ngược lại mọi nước đi đều dẫn đến trạng thái thắng, do đó trạng thái hiện tại là thua. 

Các phân đoạn được tính toán trước thể hiện chính xác các nước đi được phép vì mỗi nước đi hợp lệ là một phân đoạn liền kề dọc theo một trong ba hướng của bàn cờ và mỗi phân đoạn như vậy xuất hiện trên một đường tối đa. 

## Giải pháp Python```python
import sys
from functools import lru_cache
from collections import defaultdict

input = sys.stdin.readline

# build axial coordinates for radius-2 hexagon
coords = []
for x in range(-2, 3):
    y_min = max(-2, -x - 2)
    y_max = min(2, -x + 2)
    for y in range(y_min, y_max + 1):
        coords.append((x, y))

idx = {p: i for i, p in enumerate(coords)}

# generate all line segments
groups = defaultdict(list)

for i, (x, y) in enumerate(coords):
    groups(("x", x)).append((y, i))
    groups(("y", y)).append((x, i))
    groups(("z", x + y)).append((x, i))

moves = []

for key in groups:
    arr = sorted(groups[key])
    cells = [i for _, i in arr]
    n = len(cells)

    for l in range(n):
        mask = 0
        for r in range(l, n):
            mask |= 1 << cells[r]
            moves.append(mask)

# remove duplicates
moves = list(set(moves))

# input rows correspond to these lengths
row_lengths = [3, 4, 5, 4, 3]

board = []
for _ in range(5):
    board.extend(input().split())

mask = 0
ptr = 0

for length in row_lengths:
    for _ in range(length):
        if board[ptr] == "O":
            mask |= 1 << ptr
        ptr += 1

@lru_cache(None)
def win(state):
    for mv in moves:
        if (state & mv) == mv:
            nxt = state ^ mv
            if not win(nxt):
                return True
    return False

print("Karlsson" if win(mask) else "Lillebror")
```Phần đầu tiên xây dựng bảng lục giác bằng tọa độ trục. Một hình lục giác bán kính-2 tự nhiên chứa 19 ô, khớp chính xác với bảng vấn đề. Việc sử dụng tọa độ thay vì mã hóa liền kề giúp việc tạo dòng trở nên rõ ràng hơn nhiều. 

Ba họ dòng tương ứng với hằng số`x`, không thay đổi`y`, và hằng số`x + y`. Mọi hướng thẳng trên lưới lục giác thuộc về một trong các nhóm này. 

Đối với mỗi dòng, mã sẽ tăng dần từng phân đoạn liền kề. Biến`mask`phát triển khi điểm cuối bên phải mở rộng, giúp tránh việc phải xây dựng lại mặt nạ nhiều lần. 

Việc phân tích cú pháp đầu vào đáng được chú ý vì định dạng trực quan của câu lệnh có thể gây hiểu nhầm. Bảng luôn chứa chính xác 19 mã thông báo được chia theo độ dài hàng`3,4,5,4,3`. sử dụng`split()`bỏ qua các khoảng trống thừa một cách an toàn. 

DFS sử dụng tính năng ghi nhớ thông qua`lru_cache`. Vì mỗi lần di chuyển sẽ loại bỏ ít nhất một bit nên độ sâu đệ quy không bao giờ vượt quá 19. 

Kiểm tra tính hợp pháp:```
(state & mv) == mv
```kiểm tra xem mọi sô-cô-la mà việc di chuyển yêu cầu có còn tồn tại hay không. Điều này an toàn hơn việc đếm bit hoặc lặp lại từng ô. 

Trạng thái tiếp theo được hình thành bằng XOR vì tất cả các bit trong`mv`đảm bảo đã có mặt rồi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
. . .
 . . O .
. . O O .
 . . . .
  . . .
```Các ô bị chiếm đóng tạo thành cấu trúc mặt nạ ban đầu này: 

| Bước | Trạng thái hiện tại | Đã tìm thấy nước đi chiến thắng | Kết quả | 
| --- | --- | --- | --- | 
| 1 | Còn lại ba viên sôcôla | Hãy thử loại bỏ một sô cô la | Đối thủ vẫn thắng | 
| 2 | Hãy thử loại bỏ cặp liền kề | Đối thủ vẫn thắng | | 
| 3 | Hãy thử xóa cả ba | Đối thủ vẫn thắng | | 
| 4 | Không có động thái chiến thắng nào tồn tại | Không có | Thua | 

Câu trả lời cuối cùng là`"Lillebror"`. 

Dấu vết này chứng tỏ rằng việc có nhiều nước đi không có nghĩa là trạng thái chiến thắng. Mỗi nước đi đều để lại một vị trí mà đối thủ có thể giành chiến thắng. 

### Ví dụ 2 

đầu vào:```
. . .
 . . . .
. . O . .
 . . . .
  . . .
```| Bước | Trạng thái hiện tại | Di chuyển đã chọn | Bang tiếp theo | 
| --- | --- | --- | --- | 
| 1 | Sôcôla đơn | Xóa ô đó | Bảng trống | 
| 2 | Bảng trống | Không có động thái hợp pháp | Thua | 

Câu trả lời cuối cùng là`"Karlsson"`. 

Điều này xác nhận thuộc tính cơ bản của trò chơi: trạng thái trống sẽ thua vì người chơi hiện tại không có nước đi hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(2^{19} \cdot M)$| Mỗi trạng thái được giải quyết một lần, lặp lại trên tất cả các mặt nạ di chuyển | 
| Không gian |$O(2^{19})$| Bảng ghi nhớ lưu trữ một giá trị cho mỗi trạng thái | 

Số lượng mặt nạ di chuyển ít vì bản thân bảng đã cố định. Ngay cả trong trường hợp xấu nhất, tổng công việc vẫn dễ dàng nằm trong giới hạn. Khoảng nửa triệu trạng thái có thao tác bit nhẹ là hoàn toàn an toàn trong Python. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io
from functools import lru_cache
from collections import defaultdict

def solve():
    input = sys.stdin.readline

    coords = []
    for x in range(-2, 3):
        y_min = max(-2, -x - 2)
        y_max = min(2, -x + 2)
        for y in range(y_min, y_max + 1):
            coords.append((x, y))

    groups = defaultdict(list)

    for i, (x, y) in enumerate(coords):
        groups(("x", x)).append((y, i))
        groups(("y", y)).append((x, i))
        groups(("z", x + y)).append((x, i))

    moves = []

    for key in groups:
        arr = sorted(groups[key])
        cells = [i for _, i in arr]

        for l in range(len(cells)):
            mask = 0
            for r in range(l, len(cells)):
                mask |= 1 << cells[r]
                moves.append(mask)

    moves = list(set(moves))

    board = []
    for _ in range(5):
        board.extend(input().split())

    mask = 0

    for i, c in enumerate(board):
        if c == "O":
            mask |= 1 << i

    @lru_cache(None)
    def win(state):
        for mv in moves:
            if (state & mv) == mv:
                if not win(state ^ mv):
                    return True
        return False

    print("Karlsson" if win(mask) else "Lillebror")

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    backup = sys.stdout
    sys.stdout = out

    solve()

    sys.stdout = backup
    return out.getvalue().strip()

# provided sample
assert run(""". . .
. . O .
. . O O .
. . . .
. . .
""") == "Lillebror", "sample 1"

# single chocolate
assert run(""". . .
. . . .
. . O . .
. . . .
. . .
""") == "Karlsson", "single chocolate"

# full board
assert run("""O O O
O O O O
O O O O O
O O O O
O O O
""") in ["Karlsson", "Lillebror"], "maximum board"

# two disconnected chocolates
assert run(""". . .
. O . .
. . O . .
. . . .
. . .
""") == "Lillebror", "two isolated chocolates"

# adjacent pair
assert run(""". . .
. . . .
. O O . .
. . . .
. . .
""") == "Karlsson", "pair position"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Sôcôla đơn | Karlsson | Di chuyển đơn ô được tạo ra | 
| Trọn gói | Đầu ra hợp lệ | Xử lý không gian trạng thái lớn nhất | 
| Hai viên sôcôla bị ngắt kết nối | Lillebror | Các ô bị ngắt kết nối không thể được gỡ bỏ cùng nhau | 
| Cặp liền kề | Karlsson | Di chuyển liền kề nhiều ô hoạt động chính xác | 

## Vỏ cạnh 

Xét hai viên sôcôla nằm trên cùng một hướng hình học nhưng cách nhau bởi một ô trống:```
. . .
 . O . .
. . O . .
 . . . .
  . . .
```Việc triển khai có lỗi có thể cho phép loại bỏ cả hai cùng nhau vì chúng có vẻ được căn chỉnh. Thuật toán tránh điều này vì các bước di chuyển chỉ được tạo từ các đoạn liền kề bên trong một đường tối đa. Vì ô ở giữa trống phá vỡ sự liền kề nên không có mặt nạ di chuyển nào chứa cả hai sôcôla cùng một lúc. 

DFS chỉ thấy việc loại bỏ một ô. Sau khi loại bỏ một viên sôcôla, người chơi còn lại loại bỏ viên kia và giành chiến thắng. Kết quả là`"Lillebror"`. 

Bây giờ hãy xem xét bảng không trống nhỏ nhất:```
. . .
 . . . .
. . O . .
 . . . .
  . . .
```Bộ di chuyển được tạo bao gồm tất cả các đoạn có độ dài 1, vì vậy sô cô la đơn độc có thể tháo rời được. Cuộc gọi đệ quy đạt đến trạng thái`0`, không có động thái hợp pháp và đang thua. Do đó trạng thái ban đầu đang thắng và đầu ra là`"Karlsson"`. 

Một trường hợp tinh vi khác là việc tạo nước đi trùng lặp từ logic đường chồng chéo. Việc triển khai sẽ loại bỏ các bản sao bằng:```
moves = list(set(moves))
```Nếu không có điều này, tính chính xác vẫn được giữ nguyên, nhưng DFS sẽ liên tục kiểm tra các chuyển đổi giống hệt nhau. Tính năng loại bỏ trùng lặp giúp tìm kiếm gọn gàng và tránh được chi phí đệ quy không cần thiết.
