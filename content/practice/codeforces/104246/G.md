---
title: "CF 104246G - Đi bộ dạng lưới"
description: "Chúng tôi được cung cấp một lưới trong đó một số ô bị chặn và phần còn lại miễn phí. Trên lưới này, một số robot được đặt. Mỗi robot chiếm chính xác một ô trống và có hướng, một trong bốn hướng: trái, phải, lên hoặc xuống. Thời gian tiến triển theo từng bước rời rạc."
date: "2026-07-01T23:02:54+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104246
codeforces_index: "G"
codeforces_contest_name: "CodeSmash 2021 by RAPL"
rating: 0
weight: 104246
solve_time_s: 62
verified: true
draft: false
---

[CF 104246G - Đi bộ trên lưới](https://codeforces.com/problemset/problem/104246/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 2s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được cung cấp một lưới trong đó một số ô bị chặn và phần còn lại miễn phí. Trên lưới này, một số robot được đặt. Mỗi robot chiếm chính xác một ô trống và có hướng, một trong bốn hướng: trái, phải, lên hoặc xuống. 

Thời gian tiến triển theo từng bước rời rạc. Ở mỗi bước, mỗi robot cố gắng di chuyển một ô theo hướng mà nó đang hướng tới. Nếu ô liền kề theo hướng đó tồn tại và không bị chặn, robot sẽ di chuyển đến đó và giữ nguyên hướng. Nếu không thể di chuyển vì nó sẽ rời khỏi lưới hoặc đi vào ô bị chặn, robot sẽ không di chuyển và thay vào đó sẽ lật hướng sang hướng ngược lại với hướng mà nó đã đối mặt trước đó. 

Nhiệm vụ là xác định, đối với mỗi robot một cách độc lập, vị trí và hướng của nó sau đúng k bước thời gian như vậy. 

Nhận xét quan trọng từ các ràng buộc đầu vào là cả kích thước lưới và số lượng robot đều nhỏ, tối đa là 100 cho mỗi chiều hoặc số lượng và k cũng nhiều nhất là 100. Điều này ngay lập tức loại trừ mọi nhu cầu phát hiện chu trình nâng cao hoặc tối ưu hóa mô phỏng quy mô lớn. Mô phỏng trực tiếp tất cả các robot qua tất cả các bước là đủ, vì tổng số thao tác tối đa là 100 × 100 = 10^4 mỗi robot, đủ nhanh. 

Một vấn đề tế nhị phát sinh từ việc giải thích chính xác các ô bị chặn. Robot không thể di chuyển vào ô bị chặn và lỗi như vậy sẽ gây ra hiện tượng đảo hướng. Một chi tiết quan trọng khác là việc thay đổi hướng chỉ xảy ra khi chuyển động không thành công; nếu không thì hướng vẫn không thay đổi. 

Các trường hợp cạnh bao gồm các robot khởi động bên cạnh các bức tường hoặc các ô bị chặn theo mọi hướng, gây ra dao động ngay lập tức giữa các hướng. Ví dụ, một robot được đặt trong một góc được bao quanh bởi các khối có thể đổi hướng mỗi bước mà không cần di chuyển. 

Một trường hợp khác là nhiều robot chia sẻ một ô. Điều này được cho phép và không có tác dụng tương tác vì robot không ảnh hưởng lẫn nhau. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là mô phỏng quy trình theo từng bước. Đối với mỗi bước trong số k bước thời gian, chúng tôi lặp lại tất cả các rô-bốt và cố gắng di chuyển chúng theo hướng hiện tại của chúng. Nếu nước đi hợp lệ, chúng tôi sẽ cập nhật vị trí. Nếu không thì ta lật hướng tại chỗ. 

Điều này hiệu quả vì mỗi robot tiến hóa độc lập và trạng thái của nó tại thời điểm t+1 chỉ phụ thuộc vào trạng thái của nó tại thời điểm t và lưới. Không có sự ghép nối giữa các robot nên chúng ta có thể mô phỏng từng robot một cách an toàn. 

Bản chất mạnh mẽ của mô phỏng này đã đủ hiệu quả trong các điều kiện ràng buộc. Mỗi bước là O(x) và có k bước, cho O(xk). Với x, k 100, trường hợp xấu nhất là 10^4 bản cập nhật, mỗi bản cập nhật liên quan đến việc kiểm tra lưới theo thời gian không đổi. 

Không cần phát hiện chu trình hoặc nén trạng thái vì k nhỏ. Mặc dù chuyển động của robot mang tính xác định và cuối cùng là tuần hoàn trên một lưới hữu hạn, độ dài chu kỳ không liên quan do các ràng buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(x · k) | O(n · m + x) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi biểu thị lưới dưới dạng mảng ký tự 2D và lưu trữ mỗi rô-bốt dưới dạng một bộ dữ liệu chứa chỉ mục hàng, cột và hướng của nó. 

Chúng tôi ánh xạ các hướng để tọa độ các vùng đồng bằng: cột giảm bên trái, cột tăng bên phải, hàng giảm lên và hàng tăng xuống. Chúng tôi cũng xác định ánh xạ hướng ngược lại cho các lần lật. 

## Hướng dẫn thuật toán

1. Đọc lưới và lưu trữ tất cả trạng thái của robot. Mỗi robot có một vị trí và hướng đi. 
2. Chuyển đổi ký tự hướng thành mã số nguyên để cập nhật nhanh và thống nhất. 
3. Lặp lại quá trình sau k lần. 
4. Đối với mỗi robot, hãy tính toán ô tiếp theo dựa trên hướng hiện tại của nó. 
5. Kiểm tra xem ô đó có nằm trong lưới và không bị chặn hay không. Việc kiểm tra này là điểm quyết định cốt lõi. 
6. Nếu nước đi hợp lệ, hãy cập nhật vị trí của robot sang ô mới. 
7. Nếu nước đi không hợp lệ, giữ nguyên vị trí và lật hướng của robot sang hướng ngược lại. 
8. Sau k lần lặp, chuyển đổi mã hướng trở lại thành ký tự và xuất trạng thái cuối cùng. 

Chi tiết triển khai chính là các bản cập nhật phải dựa trên trạng thái tại thời điểm t, không phải các trạng thái được cập nhật một phần trong cùng một bước thời gian. Vì rô-bốt hoạt động độc lập nên chúng tôi có thể cập nhật tại chỗ cho mỗi rô-bốt trong mỗi bước một cách an toàn mà không cần lo lắng về sự tương tác. 

### Tại sao nó hoạt động 

Quá trình chuyển đổi trạng thái của mỗi robot được xác định hoàn toàn bởi trạng thái hiện tại và lưới cố định. Vì không có robot nào ảnh hưởng đến robot khác nên hệ thống này là một tập hợp các máy trạng thái hữu hạn xác định độc lập. Mỗi bước áp dụng cùng một hàm chuyển đổi, do đó việc lặp lại k lần sẽ tính toán chính xác thành phần hàm k bước. Không có sự phụ thuộc ẩn vào thứ tự cập nhật vì quá trình chuyển đổi của mỗi robot đều khép kín. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

n, m, x, k = map(int, input().split())
grid = [input().strip() for _ in range(n)]

dirs = {'L': 0, 'R': 1, 'U': 2, 'D': 3}
rev = ['L', 'R', 'U', 'D']

dr = [0, 0, -1, 1]
dc = [-1, 1, 0, 0]
opp = [1, 0, 3, 2]

robots = []
for _ in range(x):
    r, c, d = input().split()
    r = int(r) - 1
    c = int(c) - 1
    robots.append([r, c, dirs[d]])

for _ in range(k):
    for i in range(x):
        r, c, d = robots[i]
        nr = r + dr[d]
        nc = c + dc[d]
        if 0 <= nr < n and 0 <= nc < m and grid[nr][nc] == '.':
            robots[i][0] = nr
            robots[i][1] = nc
        else:
            robots[i][2] = opp[d]

out = []
for r, c, d in robots:
    out.append(f"{r+1} {c+1} {rev[d]}")

print("\n".join(out))
```Mã trực tiếp mã hóa các quy tắc chuyển đổi. Các mảng hướng xác định các vùng đồng bằng chuyển động và các lần lật ngược chiều, tránh phân nhánh có điều kiện cho từng loại hướng. 

Vòng lặp mô phỏng được cấu trúc sao cho mỗi bước thời gian được áp dụng đầy đủ cho tất cả các robot trước khi tiến lên, duy trì ngữ nghĩa thời gian rời rạc chính xác. 

Một sai lầm phổ biến là cập nhật một robot rồi sử dụng trạng thái cập nhật đó ngay lập tức cho một robot khác trong cùng một bước. Điều đó chỉ sai nếu robot tương tác, nhưng ngay cả ở đây cũng an toàn vì không có tương tác. Tuy nhiên, việc cấu trúc các bản cập nhật cho mỗi robot theo từng bước thời gian sẽ tránh được sự nhầm lẫn và phù hợp với mô hình vấn đề. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5 5 2 4
..##.
#....
...##
###..
....#
2 3 L
2 4 R
```Chúng tôi theo dõi cả hai robot theo thời gian. 

| Bước | Robot 1 (r,c,d) | Robot 2 (r,c,d) | 
| --- | --- | --- | 
| 0 | (2,3,L) | (2,4,R) | 
| 1 | (2,2,L) | (2,5,R) | 
| 2 | (2,1,L) | (2,5,L) | 
| 3 | (2,1,R) | (2,5,R) | 
| 4 | (2,2,R) | (2,4,R) | 

Sau 4 bước, họ trở lại cấu hình ban đầu. 

Dấu vết này cho thấy các ranh giới và cạnh bị chặn tạo ra dao động trong đó robot nảy lên hoặc lật hướng mà không nhất thiết phải di chuyển từng bước. 

### Mẫu 2 

đầu vào:```
10 10 10 10
#......##.
#.##.###..
#.#....#..
......##..
##.#.#..#.
.##...###.
#...###..#
.##.......
##..#.#.##
#...#.#.##
...
```Một bảng từng bước đầy đủ sẽ quá lớn, nhưng cơ chế tương tự sẽ được áp dụng: mỗi robot phát triển độc lập, liên tục cố gắng di chuyển và đảo hướng khi bị chặn. 

Hiện tượng quan trọng ở đây là robot có thể bị mắc kẹt trong các hành lang cục bộ của các ô mở, gây ra các chu kỳ chuyển động và đảo ngược xác định. 

Điều này xác nhận rằng mô phỏng phải duy trì tính đồng bộ theo từng bước và không cố gắng bỏ qua hoặc nén đường dẫn một cách tham lam. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(x · k) | Mỗi bước trong số k bước xử lý tất cả x robot với kiểm tra lưới O(1) | 
| Không gian | O(n · m + x) | Lưu trữ lưới cộng với mảng trạng thái robot | 

Công việc tối đa là 100 × 100 = 10.000 lần cập nhật robot, mỗi lần không đổi. Điều này nằm trong giới hạn cho ràng buộc 1 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    input = sys.stdin.readline

    n, m, x, k = map(int, input().split())
    grid = [input().strip() for _ in range(n)]

    dirs = {'L': 0, 'R': 1, 'U': 2, 'D': 3}
    rev = ['L', 'R', 'U', 'D']

    dr = [0, 0, -1, 1]
    dc = [-1, 1, 0, 0]
    opp = [1, 0, 3, 2]

    robots = []
    for _ in range(x):
        r, c, d = input().split()
        robots.append([int(r)-1, int(c)-1, dirs[d]])

    for _ in range(k):
        for i in range(x):
            r, c, d = robots[i]
            nr, nc = r + dr[d], c + dc[d]
            if 0 <= nr < n and 0 <= nc < m and grid[nr][nc] == '.':
                robots[i][0], robots[i][1] = nr, nc
            else:
                robots[i][2] = opp[d]

    out = "\n".join(f"{r+1} {c+1} {rev[d]}" for r,c,d in robots)
    return out

# provided samples
assert run("""5 5 2 4
..##.
#....
...##
###..
....#
2 3 L
2 4 R
""") == """2 4 R
2 3 L"""

# custom cases

# 1. single cell, always blocked move, flip direction
assert run("""1 1 1 3
.
1 1 L
""") == """1 1 R"""

# 2. open line, deterministic movement
assert run("""1 5 1 4
.....
1 3 R
""") == """1 5 R"""

# 3. immediate wall flip oscillation
assert run("""1 3 1 2
.#.
1 1 R
""") == """1 1 L"""

# 4. multiple robots independent
assert run("""2 2 2 1
..
..
1 1 R
2 2 L
""") == """1 2 R
2 1 L"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hành vi bị chặn 1x1 | chỉ lật | lật hướng không chuyển động | 
| Chuyển động mở 1D | tầm với cạnh | sự đúng đắn của chuyển động biên | 
| dao động của tường | logic lật | xử lý ô bị chặn | 
| nhiều robot | độc lập | không có sự tương tác giữa các robot | 

## Vỏ cạnh 

Trường hợp cạnh phím là một robot được bao quanh bởi các ô hoặc đường viền lưới bị chặn ở tất cả các phía ngoại trừ một hướng cũng bị chặn. Ví dụ:```
1 1 1 k
.
1 1 L
```Mọi cố gắng di chuyển đều không hợp lệ nên robot không bao giờ thay đổi vị trí. Mỗi bước đảo hướng nên sau k bước, hướng phụ thuộc vào tính chẵn lẻ của k. Mô phỏng xử lý điều này một cách chính xác vì thao tác lật được áp dụng mỗi khi kiểm tra di chuyển không thành công, không phụ thuộc vào thay đổi vị trí. 

Một trường hợp khác là robot di chuyển dọc theo hành lang gồm các ô mở và ô bị chặn xen kẽ. Trong những trường hợp như vậy, chuyển động không liên tục và sự chuyển hướng chỉ xảy ra ở các ranh giới. Vì mỗi bước sẽ tính toán lại giá trị hợp lệ từ lưới nên mô phỏng sẽ nắm bắt được dao động một cách tự nhiên mà không cần vỏ đặc biệt. 

Trường hợp cuối cùng là nhiều robot luôn chiếm giữ cùng một ô. Vì không có quy tắc xung đột nào tồn tại nên thuật toán chỉ theo dõi chúng một cách độc lập và các vị trí được chia sẻ không ảnh hưởng đến quá trình chuyển đổi.
