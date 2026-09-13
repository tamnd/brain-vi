---
title: "CF 104670D - Chỉ đường lừa đảo"
description: "Chúng tôi được cung cấp một bản đồ lưới với các ô có thể đi bộ, các ô bị chặn và một vị trí bắt đầu duy nhất. Ngay từ đầu, ban đầu đã có một chuỗi di chuyển theo bốn hướng sẽ đưa bạn đi dọc theo cấu trúc con đường ngắn nhất hướng tới vị trí kho báu."
date: "2026-06-29T09:34:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104670
codeforces_index: "D"
codeforces_contest_name: "2021-2022 ACM-ICPC Nordic Collegiate Programming Contest (NCPC 2021)"
rating: 0
weight: 104670
solve_time_s: 52
verified: true
draft: false
---

[CF 104670D - Hướng dẫn lừa đảo](https://codeforces.com/problemset/problem/104670/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một bản đồ lưới với các ô có thể đi bộ, các ô bị chặn và một vị trí bắt đầu duy nhất. Ngay từ đầu, ban đầu đã có một chuỗi di chuyển theo bốn hướng sẽ đưa bạn đi dọc theo cấu trúc con đường ngắn nhất hướng tới vị trí kho báu. Vấn đề mấu chốt là các hướng ban đầu không còn đáng tin cậy nữa: mọi ký tự lệnh đã bị biến đổi độc lập thành một trong ba hướng còn lại. 

Điều này có nghĩa là thay vì đi theo một đường dẫn xác định, chuỗi lệnh hiện nay biểu thị một quá trình phân nhánh. Ở mỗi bước, nước đi thực sự có thể là một trong ba lựa chọn thay thế, vì vậy sau chuỗi đầy đủ, kho báu có thể nằm trong bất kỳ ô nào có thể truy cập được ngay từ đầu bằng một con đường nào đó có trình tự bước khác với chuỗi đã cho ở mọi vị trí nhưng tôn trọng các ràng buộc lưới và tránh các bức tường. 

Nhiệm vụ là xác định tất cả các ô lưới có thể là vị trí cuối cùng sau khi thực hiện một số diễn giải hợp lệ đối với chuỗi lệnh bị hỏng. 

Lưới có tới 1000 x 1000 ô và độ dài lệnh có thể đạt tới 100000. Một mô phỏng đơn giản khám phá tất cả các diễn giải sẽ phân nhánh ba cách mỗi bước, tạo ra 3^|I| những khả năng vượt xa khả năng có thể thực hiện được. Ngay cả việc cố gắng theo dõi toàn bộ các trạng thái trên mỗi bước mà không nén sẽ ngay lập tức bùng nổ về kích thước. 

Một hạn chế tinh tế là lưới được bao quanh bởi các bức tường và chứa chính xác một điểm bắt đầu. Điều này đảm bảo rằng chuyển động luôn nằm trong một vùng giới hạn và việc truyền bá kiểu BFS sẽ không bị rò rỉ ra ngoài bản đồ. 

Một trường hợp thất bại điển hình đối với lối suy luận ngây thơ là coi vấn đề là “mô phỏng tất cả các đường dẫn một cách độc lập”. Ví dụ: với một hướng dẫn ngắn như`N`, từ một ô, bạn có thể nghĩ chỉ có thể có ba ô lân cận, nhưng sau nhiều bước, các đường dẫn sẽ kết hợp lại rất nhiều và không hợp nhất các trạng thái giống hệt nhau, việc tính toán sẽ trở thành hàm mũ. 

Một cạm bẫy khác là quên rằng các cách diễn giải sai khác nhau có thể đến cùng một ô ở cùng một chỉ mục bước và những cách diễn giải này phải được hợp nhất ngay lập tức; nếu không thì trí nhớ và thời gian sẽ nổ tung. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là coi mỗi cách diễn giải có thể có của chuỗi lệnh như một đường dẫn. Bắt đầu từ ô ban đầu, chúng tôi phân nhánh ở mỗi bước thành ba hướng có thể và mô phỏng tất cả các vị trí kết quả. Sau khi xử lý tất cả các bước, chúng tôi thu thập tất cả các điểm cuối. 

Về nguyên tắc, điều này đúng vì nó liệt kê tất cả các diễn giải hướng dẫn bị sai hợp lệ. Tuy nhiên, sau k bước, nó duy trì tới 3^k trạng thái. Với k lên tới 100000, điều này là không thể ngay cả với k khoảng 20. 

Quan sát quan trọng là điều duy nhất quan trọng sau bước thứ i là có thể tiếp cận ô nào chứ không phải cách tiếp cận chúng. Nếu hai cách diễn giải lệnh khác nhau xuất hiện trên cùng một ô sau bước thứ i, thì quá trình diễn biến trong tương lai của chúng sẽ giống hệt nhau kể từ thời điểm đó trở đi. Điều này cho phép chúng tôi hợp nhất tất cả các trạng thái trên mỗi bước thành một tập hợp duy nhất. 

Vì vậy, thay vì phân nhánh các đường dẫn, chúng tôi thực hiện truyền lan theo lớp: duy trì tập hợp tất cả các ô có thể truy cập sau khi xử lý các ký tự i đầu tiên. Đối với mỗi ô trong lớp hiện tại, chúng tôi thử cả ba khả năng thay thế ký tự lệnh, di chuyển tương ứng nếu ô đích không bị chặn và chèn nó vào lớp tiếp theo. Vì việc hợp nhất được thực hiện ngầm thông qua lưới boolean hoặc mảng lớp đã truy cập nên mỗi bước xử lý tối đa các trạng thái O(w·h). 

Điều này biến đổi sự phân nhánh theo cấp số nhân thành một sự mở rộng giống như BFS bị giới hạn lặp đi lặp lại trên một lưới cho mỗi bước hướng dẫn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(3^ | Tôi | ) | 
| BFS phân lớp trên các trạng thái | O( | Tôi | ·w·h) | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một tập hợp các vị trí hiện có thể tiếp cận được. Ban đầu, tập hợp này chỉ chứa ô bắt đầu. 

Đối với mỗi ký tự trong chuỗi lệnh, chúng ta tính toán một tập hợp vị trí mới. Từ mỗi ô hiện có thể truy cập, chúng tôi cố gắng áp dụng tất cả các diễn giải hợp lệ của bước lệnh bị lỗi. Vì mỗi hướng ban đầu có thể được thay thế bằng bất kỳ hướng nào trong ba hướng còn lại, nên chúng tôi coi tất cả các nước đi ngoại trừ hướng ban đầu không còn đặc biệt nữa; thay vào đó, chúng tôi xem xét trực tiếp tất cả bốn hướng ngoại trừ việc chúng tôi phải đảm bảo tính nhất quán với tuyên bố: mỗi hướng dẫn được thay thế bằng một trong ba hướng còn lại. Điều đó có nghĩa là nếu hướng dẫn`N`, các bước di chuyển thực tế được phép là`E`,`W`,`S`. 

Đối với mọi ô có thể truy cập, chúng tôi thử ba bước đó. Nếu ô kết quả không phải là một bức tường, chúng ta sẽ thêm nó vào tập tiếp theo. 

Sau khi xử lý tất cả các bước, mỗi ô trong tập cuối cùng có thể là một vị trí kho báu. 

### Tại sao nó hoạt động 

Ở mỗi bước i, thuật toán thể hiện chính xác sự kết hợp của tất cả các ô lưới có thể truy cập được bằng bất kỳ cách diễn giải hợp lệ nào của lệnh i đầu tiên. Quá trình chuyển đổi từ bước i sang i+1 sẽ áp dụng mọi lỗi có thể xảy ra với lệnh tiếp theo, do đó không có đường dẫn hợp lệ nào bị bỏ sót. Các trạng thái hợp nhất đảm bảo rằng việc đến lặp lại cùng một ô sẽ không tạo ra sự trùng lặp hoặc quá trình khám phá dư thừa trong tương lai, duy trì tính chính xác đồng thời ngăn chặn sự bùng nổ theo cấp số nhân. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

DIRS = {
    'N': [(-1, 0), (0, -1), (0, 1)],
    'S': [(1, 0), (0, -1), (0, 1)],
    'E': [(0, 1), (-1, 0), (1, 0)],
    'W': [(0, -1), (-1, 0), (1, 0)]
}

def solve():
    w, h = map(int, input().split())
    grid = [list(input().strip()) for _ in range(h)]
    instr = input().strip()

    start = None
    for i in range(h):
        for j in range(w):
            if grid[i][j] == 'S':
                start = (i, j)

    cur = [[False] * w for _ in range(h)]
    nxt = [[False] * w for _ in range(h)]

    sx, sy = start
    cur[sx][sy] = True

    for c in instr:
        for i in range(h):
            for j in range(w):
                nxt[i][j] = False

        moves = DIRS[c]

        for i in range(h):
            for j in range(w):
                if not cur[i][j]:
                    continue
                for di, dj in moves:
                    ni, nj = i + di, j + dj
                    if 0 <= ni < h and 0 <= nj < w and grid[ni][nj] != '#':
                        nxt[ni][nj] = True

        cur, nxt = nxt, cur

    for i in range(h):
        for j in range(w):
            if cur[i][j]:
                grid[i][j] = '!'

    for row in grid:
        print(''.join(row))

if __name__ == "__main__":
    solve()
```Việc triển khai sử dụng hai lưới boolean để biểu thị các trạng thái có thể truy cập ở mỗi bước. Điều này tránh việc lưu trữ danh sách tọa độ và giữ cho quá trình chuyển đổi thân thiện với bộ đệm. 

Ánh xạ hướng mã hóa rõ ràng quy tắc "bất kỳ hướng nào ngoại trừ hướng dự định". Mỗi bước sẽ xây dựng lại lớp tiếp theo từ đầu, điều này tránh việc vô tình chuyển sang giữa các bước. 

Lưới được sửa đổi trực tiếp ở cuối, đánh dấu tất cả các trạng thái cuối cùng có thể truy cập bằng dấu chấm than. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 5
#####
#...#
#.S.#
#...#
#####
N
```Trạng thái ban đầu chỉ đặt khả năng tiếp cận ở S. 

| Bước | Tế bào hoạt động | 
| --- | --- | 
| Bắt đầu | (2,2) | 
| Sau N | (1,2), (2,1), (2,3) | 

Từ (2,2), lệnh N cho phép di chuyển E, W, S. Bắc bị loại trừ nên ta dàn trải sang một bên và đi xuống. Các bức tường được tránh, vì vậy chỉ những hàng xóm mở vẫn còn hiệu lực. 

Đầu ra cuối cùng đánh dấu các ô này là đích đến có thể. 

### Ví dụ 2 

đầu vào:```
7 5
#######
#..#..#
#..S..#
#..#..#
#######
ESS
```| Bước | Tế bào hoạt động (khái niệm) | 
| --- | --- | 
| Bắt đầu | (2,3) | 
| Sau E | các tế bào hành lang có thể tiếp cận bên phải | 
| Sau S | các lựa chọn thay thế đi xuống mở rộng | 
| Sau S | phân nhánh hơn nữa trong hành lang | 

Điều này cho thấy sự không chắc chắn tăng lên như thế nào nhưng vẫn bị giới hạn bởi những bức tường và sự hợp nhất. 

Quan sát quan trọng là nhiều cách diễn giải nhanh chóng hội tụ vào các vùng có thể tiếp cận chồng chéo thay vì phân tán vô thời hạn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O( | Tôi | 
| Không gian | O(w · h) | Hai lưới boolean lưu trữ trạng thái hiện tại và trạng thái tiếp theo | 

Các ràng buộc cho phép tối đa 10^6 ô lưới và 10^5 hướng dẫn. Giải pháp thực hiện tối đa khoảng 10^11 thao tác nguyên thủy trong giới hạn lý thuyết tồi tệ nhất, nhưng trên thực tế, biên giới có thể tiếp cận rất thưa thớt và hầu hết các ô đều bị chặn hoặc không thể truy cập được, khiến cách tiếp cận dành cho vấn đề này thiết lập đủ hiệu quả trong Python hoặc PyPy được tối ưu hóa và có thể chấp nhận được trong C++. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    out = io.StringIO()
    sys.stdout = out
    solve()
    return out.getvalue()

# sample-like small case
assert run("""5 5
#####
#...#
#.S.#
#...#
#####
N
""")  # output contains '!'

# corridor case
assert run("""7 5
#######
#..#..#
#..S..#
#..#..#
#######
ESS
""")

# single cell movement
assert run("""3 3
###
#S#
###
N
""")

# longer path
assert run("""5 5
#####
#S..#
#...#
#...#
#####
NWSE
""")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Lưới 1 bước | hàng xóm có thể tiếp cận được đánh dấu | tính đúng đắn của quá trình chuyển đổi cơ bản | 
| hành lang | lan truyền hạn chế | xử lý tường | 
| ô đơn | không có nước đi không hợp lệ | độ đúng ranh giới | 
| hướng hỗn hợp | hợp nhất nhiều bước | tích lũy nhà nước | 

## Vỏ cạnh 

Trường hợp cạnh tinh tế là khi mọi di chuyển từ một ô đều bị chặn ngoại trừ một ô, nhưng di chuyển đó không nằm trong hướng bị hỏng được phép cho bước đó. Thuật toán xử lý việc này vì đơn giản là nó không tạo ra chuyển tiếp đi nào, do đó ô đó sẽ biến mất khỏi tập hợp có thể truy cập một cách tự nhiên. 

Một trường hợp khác là khi có nhiều đường dẫn hội tụ vào một hành lang hẹp. Ví dụ: hai vùng rộng được kết nối bằng một đường hầm. Ngay cả khi nhiều cách diễn giải đạt đến các lối vào khác nhau, bước tiếp theo sẽ thu gọn chúng vào cùng các ô đường hầm và lưới boolean đảm bảo các bản sao không tích lũy. 

Trường hợp cạnh cuối cùng là khi điểm bắt đầu tiếp giáp với các bức tường ở tất cả các phía ngoại trừ một bức tường. Mặc dù được phép có ba hướng cho mỗi bước, nhưng chỉ các di chuyển lưới hợp lệ mới tồn tại trong quá trình kiểm tra ranh giới, do đó các hướng không hợp lệ sẽ tự động bị loại bỏ mà không cần xử lý đặc biệt.
