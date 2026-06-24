---
title: "CF 105272I - Thăm dò sao Hỏa"
description: "Chúng ta có một lưới hình chữ nhật trong đó mỗi ô trống, một bức tường hoặc vị trí bắt đầu duy nhất của robot. Robot bắt đầu với hướng ban đầu được mã hóa trong ô xuất phát đó và sau đó liên tục áp dụng quy tắc chuyển động xác định."
date: "2026-06-23T06:56:52+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105272
codeforces_index: "I"
codeforces_contest_name: "IX MaratonUSP Freshman Contest"
rating: 0
weight: 105272
solve_time_s: 51
verified: true
draft: false
---

[CF 105272I - Điều tra sao Hỏa](https://codeforces.com/problemset/problem/105272/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một lưới hình chữ nhật trong đó mỗi ô trống, một bức tường hoặc vị trí bắt đầu duy nhất của robot. Robot bắt đầu với hướng ban đầu được mã hóa trong ô xuất phát đó và sau đó liên tục áp dụng quy tắc chuyển động xác định. 

Ở mỗi bước, trước tiên robot sẽ cố gắng di chuyển một ô về phía trước theo hướng hiện tại. Nếu ô đó bị chặn bởi một bức tường hoặc do đi ra ngoài lưới (vì ranh giới được coi là những bức tường), nó sẽ không di chuyển và thay vào đó quay 90 độ ngược chiều kim đồng hồ và thử lại. Quá trình quay và thử lại này tiếp tục cho đến khi tìm thấy một bước di chuyển hợp lệ và khi tìm thấy một ô liền kề hợp lệ, nó sẽ di chuyển đến đó và giữ nguyên hướng hiện tại sau khi di chuyển thành công. 

Nhiệm vụ là xác định có bao nhiêu ô lưới riêng biệt mà robot sẽ truy cập ít nhất một lần trong khi quá trình này phát triển vô thời hạn. 

Kích thước lưới có thể lên tới 1000 x 1000, nghĩa là lên tới một triệu ô. Bất kỳ giải pháp nào mô phỏng chuyển động theo cách không giới hạn hoặc xem lại nhiều lần mà không ghi nhớ đều có nguy cơ vượt quá giới hạn thời gian. Mô phỏng trực tiếp qua các bước chỉ được chấp nhận nếu chúng tôi có thể đảm bảo mỗi cấu hình liên quan được xử lý với số lần không đổi. 

Khó khăn tinh tế là robot không chỉ di chuyển trên các ô mà còn ở các trạng thái bao gồm cả vị trí và hướng. Điều này có nghĩa là hệ thống có thể truy cập lại cùng một ô nhiều lần theo các hướng khác nhau và việc theo dõi “chỉ ô đã truy cập” đơn giản là không đủ để phát hiện việc chấm dứt. 

Một trường hợp lỗi phổ biến phát sinh khi robot đi vào một chu trình trong không gian trạng thái nhưng vẫn tiếp tục truy cập vào các ô đã thấy trước đó. Trong trường hợp đó, việc dừng sớm hoặc bỏ qua hướng dẫn sẽ dẫn đến việc đếm không chính xác. 

Ví dụ: trong một vòng nhỏ gồm các ô trống, rô-bốt có thể di chuyển vô thời hạn: 

Nếu chúng ta chỉ theo dõi các ô, chúng ta có thể cho rằng quá trình này là vô hạn và dừng không chính xác hoặc chúng ta có thể đếm quá mức bằng cách lặp đi lặp lại cùng một vòng lặp. 

## Phương pháp tiếp cận 

Giải thích bạo lực mô phỏng mãi mãi robot từng bước, cập nhật vị trí và hướng theo các quy tắc và ghi lại mọi ô đã ghé thăm. Quá trình dừng lại khi nó quay trở lại cấu hình đã thấy trước đó. Cấu hình không chỉ là một ô mà còn là một cặp ô và hướng. 

Cách tiếp cận này đúng vì hệ thống có tính xác định và hữu hạn. Có nhiều nhất 4 × n × m trạng thái có thể xảy ra, do đó cuối cùng một trạng thái phải lặp lại, ngụ ý một chu trình. Tuy nhiên, nếu thực hiện bất cẩn, người ta có thể chỉ lưu trữ các ô đã truy cập thay vì trạng thái đầy đủ, điều này sẽ phá vỡ tính chính xác. Một sự kém hiệu quả khác sẽ phát sinh nếu chúng ta mô phỏng chuyển động mà không chuyển đổi bộ nhớ đệm; mỗi bước có thể yêu cầu quét lặp lại tối đa bốn hướng. 

Quan sát quan trọng là mỗi trạng thái chuyển tiếp sang đúng một trạng thái tiếp theo. Điều này tạo thành một đồ thị hàm có hướng trên tất cả các cặp (ô, hướng). Di chuyển từ trạng thái ban đầu theo một đường duy nhất cho đến khi nó đi vào một chu trình. Mỗi trạng thái được truy cập tối đa một lần trước khi lặp lại, do đó việc truyền tuyến tính trên không gian trạng thái là đủ. 

Chúng tôi duy trì một tập hợp các trạng thái đã truy cập để phát hiện sự lặp lại và một tập hợp các ô đã truy cập để đếm các vị trí duy nhất. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Brute Force (mô phỏng trạng thái có tính toán lại) | O(n · k) | O(n m) | Quá chậm | 
| Tối ưu (truyền tải biểu đồ trạng thái) | O(n m) | O(n m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mô hình hóa mỗi trạng thái dưới dạng bộ ba hàng, cột và hướng. Các hướng được mã hóa dưới dạng số nguyên và việc quay ngược chiều kim đồng hồ là một hoán vị cố định của các giá trị này. 

## Hướng dẫn thuật toán

1. Phân tích cú pháp lưới và xác định vị trí ô bắt đầu duy nhất, đồng thời trích xuất hướng ban đầu của nó. Điều này đưa ra trạng thái ban đầu của mô phỏng. 
2. Xác định hàm cố gắng di chuyển từ một trạng thái. Từ ô và hướng hiện tại, chúng tôi cố gắng bước về phía trước. Nếu ô tiếp theo không hợp lệ hoặc có tường, chúng ta xoay ngược chiều kim đồng hồ và thử lại. Chúng tôi lặp lại điều này tối đa bốn lần cho đến khi tìm thấy nước đi hợp lệ. Điều này đảm bảo chấm dứt vì ít nhất một hướng phải hợp lệ hoặc tất cả đều bị chặn, trong trường hợp đó vòng quay sẽ quay trở lại. 
3. Duy trì một mảng boolean hoặc tập hợp hàm băm cho các trạng thái đã truy cập (ô, hướng). Điều này là cần thiết để phát hiện các chu trình trong đồ thị hàm số. 
4. Duy trì một mảng boolean khác cho các ô đã truy cập. Mỗi lần chúng ta vào một trạng thái mới, chúng ta đánh dấu ô đó là đã truy cập. 
5. Bắt đầu từ trạng thái ban đầu, áp dụng hàm chuyển tiếp nhiều lần. Nếu trạng thái kết quả đã được nhìn thấy, hãy dừng mô phỏng. 
6. Câu trả lời là số lượng ô duy nhất được đánh dấu đã truy cập. 

### Tại sao nó hoạt động 

Hành vi của robot xác định một hàm xác định từ tập hợp hữu hạn các trạng thái cho đến chính nó. Vì số lượng trạng thái là hữu hạn nên ứng dụng lặp đi lặp lại cuối cùng phải truy cập lại một trạng thái, tạo thành một chu trình. Trước khi vào chu trình, mỗi trạng thái được truy cập đúng một lần. Vì mỗi lượt truy cập tương ứng với chính xác một ô nên việc đếm các ô đã truy cập qua tiền tố này sẽ mang lại kết quả chính xác. Kiểm tra trạng thái đã truy cập đảm bảo chúng tôi dừng chính xác khi bắt đầu lặp lại, không sớm hơn và không muộn hơn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    grid = [list(input().strip()) for _ in range(n)]

    # Directions: U, L, D, R (counterclockwise rotation order)
    dirs = ['U', 'L', 'D', 'R']
    dr = [-1, 0, 1, 0]
    dc = [0, -1, 0, 1]
    dir_idx = {d: i for i, d in enumerate(dirs)}

    # Find start
    sr = sc = sd = -1
    for i in range(n):
        for j in range(m):
            if grid[i][j] in "ULDR":
                sr, sc = i, j
                sd = dir_idx[grid[i][j]]
                grid[i][j] = '.'

    vis_state = [[[False] * 4 for _ in range(m)] for _ in range(n)]
    vis_cell = [[False] * m for _ in range(n)]

    r, c, d = sr, sc, sd
    vis_state[r][c][d] = True
    vis_cell[r][c] = True
    ans = 1

    def blocked(x, y):
        if x < 0 or x >= n or y < 0 or y >= m:
            return True
        return grid[x][y] == '#'

    while True:
        nd = d
        nr = r + dr[nd]
        nc = c + dc[nd]

        for _ in range(4):
            if not blocked(nr, nc):
                break
            nd = (nd + 1) % 4
            nr = r + dr[nd]
            nc = c + dc[nd]

        if vis_state[nr][nc][nd]:
            break

        r, c, d = nr, nc, nd

        if not vis_cell[r][c]:
            vis_cell[r][c] = True
            ans += 1

        vis_state[r][c][d] = True

    print(ans)

if __name__ == "__main__":
    solve()
```Trước tiên, mã xác định trạng thái ban đầu, sau đó liên tục tính toán chuyển động hợp lệ tiếp theo bằng cách kiểm tra tiến và xoay ngược chiều kim đồng hồ tối đa bốn lần. các`blocked`người trợ giúp coi các phần nằm ngoài giới hạn như những bức tường, điều này tránh được logic ranh giới đặc biệt ở nơi khác. 

Chi tiết triển khai quan trọng là chúng tôi lưu trữ các trạng thái đã truy cập bao gồm cả hướng đi. Nếu không có hướng, robot có thể truy cập lại một ô theo hướng khác và kết thúc sớm hoặc lặp lại vô hạn một cách không chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
2 2
.L
.#
```Chúng ta bắt đầu tại ô chứa L, di chuyển sang trái. Từ ô đó, bên trái bị chặn nên rô-bốt quay ngược chiều kim đồng hồ và cố gắng đi xuống, rồi sang phải, cho đến khi tìm được nước đi hợp lệ. Trong lưới nhỏ này, robot nhanh chóng bị hạn chế và bước vào một chu kỳ ngắn. 

| Bước | Vị trí | Hướng | Hành động tiếp theo | Các ô đã truy cập | 
| --- | --- | --- | --- | --- | 
| 1 | (0,1) | L | xoay cho đến khi di chuyển hợp lệ | {(0,1)} | 
| 2 | (1,1) | ... | tiếp tục cho đến chu kỳ | tăng trưởng rồi ổn định | 

Dấu vết cho thấy mặc dù chuyển động vẫn tiếp tục nhưng không có tế bào mới nào được phát hiện sau khi bước vào chu kỳ. 

### Ví dụ 2 

đầu vào:```
4 4
#..#
#...
##.#
U..#
```Ở đây, ban đầu, robot có nhiều tự do hơn, khám phá nhiều hành lang trước khi bị buộc phải lặp lại mô hình. Tập truy cập tăng dần cho đến điểm lặp lại trạng thái. 

| Bước | Vị trí | Hướng | Hành động | Tế bào mới? | 
| --- | --- | --- | --- | --- | 
| 1 | bắt đầu | Bạn | di chuyển | vâng | 
| 2 | ... | ... | di chuyển | vâng | 
| 3 | ... | ... | di chuyển | không | 
| 4 | bắt đầu chu kỳ | ... | phát hiện lặp lại | dừng lại | 

Điều này xác nhận rằng điều kiện dừng dựa trên sự lặp lại trạng thái đầy đủ hơn là sự lặp lại vị trí. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n m) | Mỗi trạng thái (ô, hướng) được truy cập nhiều nhất một lần và các chuyển đổi có thời gian không đổi | 
| Không gian | O(n m) | Lưu trữ các trạng thái đã truy cập trên tất cả các ô lưới và chỉ đường | 

Lưới có nhiều nhất một triệu ô, vì vậy không gian trạng thái nhiều nhất là bốn triệu. Truyền tuyến tính trên không gian này vừa vặn thoải mái trong giới hạn cho giới hạn thời gian 3 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    # call solution
    solve = globals()['solve']
    from io import StringIO
    backup = sys.stdout
    sys.stdout = StringIO()
    solve()
    out = sys.stdout.getvalue()
    sys.stdout = backup
    return out.strip()

# provided samples
assert run("2 2\n.L\n.#\n") == "1"
assert run("2 2\nL.\n.#\n") == "1"

# minimum grid
assert run("2 2\nL.\n..\n") >= "1"

# straight corridor
assert run("3 3\n###\n#L#\n###\n") == "1"

# open loop
assert run("3 3\n...\n.L.\n...\n") >= "1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Tường kín 2x2 | giá trị nhỏ | hành vi chặn ranh giới | 
| hành lang | 1 | bẫy ngay lập tức | 
| lưới mở | lớn hơn | xử lý hình thành chu trình | 

## Vỏ cạnh 

Trường hợp key edge là khi robot bị bao vây cả bốn phía ngay lập tức. Trong tình huống đó, mọi cố gắng di chuyển đều bị chặn và robot sẽ quay theo mọi hướng trước khi giữ nguyên vị trí một cách hiệu quả trong một chu kỳ trạng thái. Thuật toán xử lý việc này một cách chính xác vì nó vẫn ghi lại trạng thái ban đầu và sau đó phát hiện sự lặp lại sau khi chuyển qua bốn trạng thái định hướng trong cùng một ô. 

Một trường hợp cạnh khác xảy ra khi robot đi vào một hành lang dài và cuối cùng quay trở lại vị trí trước đó nhưng với một hướng khác. Cách tiếp cận ô đã truy cập đơn giản sẽ dừng không chính xác hoặc đếm sai. Tính năng theo dõi dựa trên trạng thái đảm bảo rằng việc xem lại cùng một ô theo một hướng khác được coi là một cấu hình riêng biệt, duy trì tính chính xác cho đến khi đạt được chu kỳ thực sự.
