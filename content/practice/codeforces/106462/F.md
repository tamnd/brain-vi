---
title: "CF 106462F - \u041b\u0430\u0431\u0438\u0440\u0438\u043d\u0442"
description: "Lưới mô tả một mê cung hình chữ nhật được tạo thành từ các ô, trong đó một số ô bị chặn bởi các bức tường và phần còn lại có thể đi bộ được. Trong số các ô có thể đi bộ, có chính xác một vị trí bắt đầu được đánh dấu là A và đúng một ô mục tiêu được đánh dấu là G."
date: "2026-06-25T08:58:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106462
codeforces_index: "F"
codeforces_contest_name: "\u041f\u0435\u0440\u0432\u0435\u043d\u0441\u0442\u0432\u043e \u0421\u0432\u0435\u0440\u0434\u043b\u043e\u0432\u0441\u043a\u043e\u0439 \u043e\u0431\u043b\u0430\u0441\u0442\u0438 \u043f\u043e \u043f\u0440\u043e\u0433\u0440\u0430\u043c\u043c\u0438\u0440\u043e\u0432\u0430\u043d\u0438\u044e \u0441\u0440\u0435\u0434\u0438 \u043d\u0430\u0447\u0438\u043d\u0430\u044e\u0449\u0438\u0445 2026"
rating: 0
weight: 106462
solve_time_s: 36
verified: true
draft: false
---

[CF 106462F - \u041b\u0430\u0431\u0438\u0440\u0438\u043d\u0442](https://codeforces.com/problemset/problem/106462/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 36s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Lưới mô tả một mê cung hình chữ nhật được tạo thành từ các ô, trong đó một số ô bị chặn bởi các bức tường và phần còn lại có thể đi bộ được. Trong số các ô có thể đi bộ, có chính xác một vị trí bắt đầu được đánh dấu là A và đúng một ô đích được đánh dấu là G. Từ ô bắt đầu, được phép di chuyển từng bước một theo bốn hướng chính, nhưng chỉ lên các ô không phải là tường. Nhiệm vụ là xác định xem có tồn tại chuỗi nước đi hợp lệ nào bắt đầu từ A và cuối cùng đến G hay không. 

Mặc dù mê cung có thể trông phức tạp nhưng vấn đề hoàn toàn nằm ở khả năng kết nối trong biểu đồ lưới. Mỗi ô tự do hoạt động như một nút và các cạnh tồn tại giữa các ô tự do liền kề trực giao. 

Các ràng buộc rất nhỏ: lưới tối đa là 100 x 100, do đó có tối đa 10.000 nút. Kích thước này đã gợi ý rằng mọi giải pháp khám phá toàn bộ lưới nhiều lần vẫn đủ nhanh. Việc truyền tải chạm vào mỗi ô với số lần không đổi là nằm trong giới hạn, trong khi mọi thứ siêu tuyến tính trên mỗi cặp ô, chẳng hạn như kiểm tra rõ ràng tất cả các đường dẫn, là không cần thiết. 

Một số tình huống khó khăn quan trọng hơn những gì chúng xuất hiện ngay từ cái nhìn đầu tiên. Một là khi A và G được bao bọc trong các vùng riêng biệt bằng tường. Ví dụ: một hành lang hẹp được phân chia bởi một ô chặn duy nhất:```
A . #
# # #
. . G
```Mặc dù cả A và G đều tồn tại nhưng chúng nằm trong những thành phần không liên kết với nhau nên câu trả lời phải là KHÔNG. Một cách tiếp cận ngây thơ cho rằng “không gian mở ngụ ý khả năng tiếp cận” sẽ thất bại ở đây. 

Một trường hợp khác là khi có đường đi ngắn nhất nhưng không rõ ràng vì nó đòi hỏi phải đi vòng quanh chướng ngại vật:```
A . # . G
# . # . #
# . . . #
```Ở đây, sự chuyển động tham lam về phía G có thể thất bại vì những lựa chọn cục bộ không đảm bảo sự tiến bộ trong mê cung. Bất kỳ giải pháp đúng đắn nào cũng phải khám phá các giải pháp thay thế thay vì cam kết theo một hướng duy nhất. 

Cuối cùng, có thể A và G ở gần nhau nhưng vẫn bị chặn theo đường chéo hoặc bị tường bao quanh cản trở sự di chuyển. Chỉ có các cạnh được chia sẻ mới quan trọng; khoảng cách chéo không cho phép di chuyển. 

## Phương pháp tiếp cận 

Một cách tiếp cận trực tiếp là coi đây là một vấn đề tìm đường và cố gắng xây dựng một cách rõ ràng một tuyến đường từ A đến G. Người ta có thể tưởng tượng việc thử đệ quy tất cả các chuỗi chuyển động có thể, đánh dấu các ô đã truy cập để tránh chu kỳ. Điều này hiệu quả vì lưới là hữu hạn và chuyển động bị hạn chế, do đó cuối cùng tất cả các trạng thái có thể tiếp cận đều được khám phá. Tuy nhiên, trong trường hợp xấu nhất, số lượng đường dẫn có thể tăng theo cấp số nhân với số lượng ô mở. Trong lưới 100 x 100 có nhiều ô trống, phương pháp này sẽ thử một số lượng lớn các đường dẫn từng phần dư thừa, xem lại cùng các vùng thông qua các bước khác nhau. 

Quan sát quan trọng là thứ tự truy cập các ô không quan trọng đối với khả năng tiếp cận. Điều quan trọng chỉ là liệu G có nằm trong cùng thành phần liên thông với A trong biểu đồ ẩn của các ô lưới hay không. Khi điều này được nhìn thấy, vấn đề sẽ giảm xuống còn việc truyền tải biểu đồ tiêu chuẩn: bắt đầu từ A, chúng tôi khám phá tất cả các ô có thể truy cập bằng cách sử dụng tìm kiếm theo chiều rộng hoặc tìm kiếm theo chiều sâu. Mỗi ô được xử lý nhiều nhất một lần và mọi cạnh chỉ được kiểm tra khi chúng ta mở rộng một nút. 

Điều này loại bỏ hoàn toàn việc liệt kê đường dẫn. Thay vì lý luận về trình tự di chuyển, chúng tôi lý luận về khả năng tiếp cận như một thuộc tính của biểu đồ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Liệt kê đường dẫn Brute Force | Hàm mũ trong các ô mở | Ngăn xếp đệ quy O(H·W) | Quá chậm | 
| Khả năng tiếp cận BFS / DFS | O(H·W) | O(H·W) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi mô hình hóa lưới dưới dạng biểu đồ trong đó mỗi ô tự do là một nút và các cạnh kết nối các ô tự do liền kề. 

1. Xác định tọa độ của A và G khi đọc lưới. Điều này cung cấp các nút bắt đầu và mục tiêu trong biểu đồ. 
2. Khởi tạo một hàng đợi cho BFS (hoặc một ngăn xếp cho DFS) và chèn vị trí bắt đầu A. Đồng thời duy trì một mảng đã truy cập có cùng kích thước với lưới để tránh phải xem lại các ô. 
3. Trong khi cấu trúc không trống, hãy xóa ô tiếp theo và coi nó là vị trí hiện tại. 
4. Nếu vị trí hiện tại là G, chúng ta có thể dừng ngay lập tức vì chúng ta đã chứng minh được rằng G có thể tới được từ A. 
5. Nếu không, hãy cố gắng di chuyển theo bốn hướng: lên, xuống, trái, phải. Đối với mỗi hàng xóm, hãy kiểm tra xem nó có nằm trong lưới, không phải tường và chưa được ghé thăm hay không. Nếu tất cả các điều kiện đều đúng, hãy đánh dấu nó đã truy cập và thêm nó vào hàng đợi. 
6. Nếu quá trình truyền tải kết thúc mà không đến được G thì A không thể truy cập được G. 

Tính đúng đắn đến từ tính bất biến là hàng đợi luôn chứa chính xác tập hợp các ô lưới có thể truy cập được từ A thông qua một số đường dẫn nhưng chưa được xử lý. Mỗi khi chúng tôi bật một ô, chúng tôi sẽ khám phá đầy đủ mọi cách để mở rộng đường dẫn hợp lệ từ A đến ô đó. Vì mọi ô có thể truy cập cuối cùng đều được phát hiện nên nếu G có thể truy cập được thì cuối cùng nó phải được xếp vào hàng đợi và xử lý. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

def solve():
    H, W = map(int, input().split())
    grid = []
    start = goal = None

    for i in range(H):
        row = list(input().strip())
        grid.append(row)
        for j, c in enumerate(row):
            if c == 'A':
                start = (i, j)
            elif c == 'G':
                goal = (i, j)

    sx, sy = start
    gx, gy = goal

    q = deque()
    q.append((sx, sy))
    visited = [[False] * W for _ in range(H)]
    visited[sx][sy] = True

    dirs = [(1, 0), (-1, 0), (0, 1), (0, -1)]

    while q:
        x, y = q.popleft()

        if (x, y) == (gx, gy):
            print("YES")
            return

        for dx, dy in dirs:
            nx, ny = x + dx, y + dy
            if 0 <= nx < H and 0 <= ny < W:
                if not visited[nx][ny] and grid[nx][ny] != '#':
                    visited[nx][ny] = True
                    q.append((nx, ny))

    print("NO")

if __name__ == "__main__":
    solve()
```Giải pháp giữ cho lưới không thay đổi và chỉ theo dõi khả năng tiếp cận. Mảng đã truy cập là cần thiết vì nếu không có nó, BFS sẽ liên tục nhập lại các chu kỳ trong các vùng mở và chuyển sang trạng thái lặp lại theo cấp số nhân. 

Việc thoát sớm khi đạt G sẽ tránh được việc di chuyển không cần thiết khi câu trả lời đã được xác định. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 5
#####
#...#
#.#.#
#A#G#
#####
```| Bước | Xếp hàng | Đã truy cập (một phần) | Hiện tại | Hành động | 
| --- | --- | --- | --- | --- | 
| 0 | (3,1) | A | (3,1) | Bắt đầu từ A | 
| 1 | hàng xóm | A + có thể truy cập | (2,1) | Mở rộng | 
| 2 | ... | ... | (3,3)=G | Tìm thấy | 

Việc di chuyển nhanh chóng đến G qua hành lang hẹp duy nhất. Điều này xác nhận rằng BFS đi theo đúng các đường dẫn bắt buộc khi chúng tồn tại. 

### Ví dụ 2 

đầu vào:```
5 5
#####
#.G.#
#####
#A..#
#####
```| Bước | Xếp hàng | Đã truy cập | Hiện tại | Hành động | 
| --- | --- | --- | --- | --- | 
| 0 | (3,1) | A | (3,1) | Bắt đầu | 
| 1 | (3,2) | A + hàng dưới cùng | (3,2) | Mở rộng | 
| 2 | vùng trống | chỉ vùng dưới cùng | - | G không thể truy cập | 

Hàng tường ngăn cách hoàn toàn A với G, vì vậy BFS không bao giờ vượt lên vùng phía trên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(H·W) | Mỗi ô được xếp hàng và xử lý nhiều nhất một lần và mỗi cạnh trong số bốn cạnh của nó được kiểm tra một lần | 
| Không gian | O(H·W) | Đã truy cập mảng và hàng đợi lưu trữ ở hầu hết các ô lưới | 

Cho H, W ≤ 100, tổng số phép toán tối đa là vài nghìn, nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io
from collections import deque

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    old_stdout = sys.stdout
    sys.stdout = out
    solve()
    sys.stdout = old_stdout
    return out.getvalue().strip()

# sample 1
assert run("""5 5
#####
#...#
#.#.#
#A#G#
#####""") == "YES"

# sample 2
assert run("""5 5
#####
#.G.#
#####
#A..#
#####""") == "NO"

# minimum grid
assert run("""5 5
#####
#A..#
#.###
#..G#
#####""") == "YES"

# fully blocked separation
assert run("""5 5
#####
#A###
#####
###G#
#####""") == "NO"

# long corridor
assert run("""5 7
#######
#A....#
#.###.#
#....G#
#######""") == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| mẫu 1 | CÓ | kết nối cơ bản | 
| mẫu 2 | KHÔNG | thành phần riêng biệt | 
| lưới tối thiểu | CÓ | con đường không tầm thường ngắn nhất | 
| ngăn chặn hoàn toàn | KHÔNG | không có khu vực có thể truy cập | 
| hành lang dài | CÓ | xử lý đường vòng | 

## Vỏ cạnh 

Trường hợp A và G ở gần nhau nhưng được ngăn cách bằng một bức tường sẽ kiểm tra rằng chỉ sự kề cận thôi là chưa đủ. Trong một đầu vào như:```
#####
#AG##
#####
#...#
#####
```BFS bắt đầu từ A, đánh dấu nó đã truy cập và kiểm tra các hàng xóm. Ô chứa G nằm liền kề, nhưng nếu đó là một bức tường hoặc vị trí bị chặn (theo quy tắc, nó sẽ là '#' hoặc không thể truy cập được), thì ô đó sẽ không bao giờ được xếp vào hàng đợi. Thuật toán chỉ thêm các ô thỏa mãn điều kiện có thể đi được, do đó G bị bỏ qua trừ khi nó thực sự có thể truy cập được. 

Trường hợp mê cung là một hành lang dài ngoằn ngoèo đảm bảo rằng việc thăm lại là cần thiết. Nếu không có mảng đã truy cập, thuật toán sẽ chuyển qua lại giữa hai ô vô thời hạn. Với nó, mỗi ô hành lang được xử lý một lần và quá trình truyền tải diễn ra tuyến tính cho đến khi đạt được mục tiêu hoặc dùng hết hành lang.
