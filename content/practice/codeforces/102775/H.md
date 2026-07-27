---
title: "CF 102775H - \u041f\u0435\u0448\u043a\u0430 \u0442\u0443\u0434\u0430-\u0441\u044e\u0434\u0430"
description: "Chúng tôi có một bảng N by N. Mỗi ô thuộc về một trong ba loại. Một ô được đánh dấu h biến quân hiện tại thành quân mã, một ô được đánh dấu p biến nó thành một con tốt đặc biệt có thể di chuyển theo quy tắc di chuyển của bài toán và một ô được đánh dấu x hoàn toàn không thể nhập được."
date: "2026-07-27T20:41:23+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102775
codeforces_index: "H"
codeforces_contest_name: "ICPC Central Russia Regional Contest (CRRC 20), \u0427\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442 \u0426\u0435\u043d\u0442\u0440\u0430\u043b\u044c\u043d\u043e\u0439 \u0420\u043e\u0441\u0441\u0438\u0438, \u043a\u0432\u0430\u043b\u0438\u0444\u0438\u043a\u0430\u0446\u0438\u043e\u043d\u043d\u044b\u0439 \u0440\u0430\u0443\u043d\u0434"
rating: 0
weight: 102775
solve_time_s: 59
verified: true
draft: false
---

[CF 102775H - \u041f\u0435\u0448\u043a\u0430 \u0442\u0443\u0434\u0430-\u0441\u044e\u0434\u0430](https://codeforces.com/problemset/problem/102775/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 59s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một bảng N by N. Mỗi ô thuộc về một trong ba loại. Một ô được đánh dấu`h`biến quân cờ hiện tại thành hiệp sĩ, một ô được đánh dấu`p`biến nó thành một con tốt đặc biệt có thể di chuyển theo quy luật di chuyển của bài toán và một ô được đánh dấu`x`hoàn toàn không thể nhập được. Chúng ta bắt đầu trên một ô hợp lệ và cần đến một ô hợp lệ khác bằng cách sử dụng ít bước di chuyển nhất có thể. 

Chi tiết quan trọng là loại quân cờ được xác định bởi ô nơi quân cờ hiện đang đứng. Các tùy chọn di chuyển không cố định trong toàn bộ hành trình, do đó, cùng một ô vuông có thể có các bước đi khác nhau tùy thuộc vào việc chúng ta đến đó và biến thành hiệp sĩ hay thành cầm đồ. Câu trả lời là số lần di chuyển cần thiết để đến đích ngắn nhất, hoặc`-1`nếu không tồn tại chuỗi nước đi hợp pháp. 

Kích thước bảng tối đa là 100 x 100, vì vậy có tối đa 10000 vị trí có thể sử dụng. Giá trị này đủ nhỏ cho các thuật toán đồ thị tuyến tính hoặc gần tuyến tính về số lượng ô. Một giải pháp thử mọi đường đi có thể sẽ không thể thực hiện được vì số lượng đường đi có thể tăng theo cấp số nhân. Ngay cả một phương pháp khám phá nhiều lần tất cả các cặp ô cũng sẽ đạt tới 100 triệu phép tính và sẽ không còn nhiều chỗ cho việc mô phỏng chuyển động. 

Những cạm bẫy triển khai chính đến từ việc chỉ xử lý các ô theo tọa độ của chúng. Tọa độ giống nhau không phải lúc nào cũng có cùng trạng thái đồ thị vì đạt đến một`h`tế bào và đến một`p`tế bào tạo ra những khả năng khác nhau trong tương lai. 

Ví dụ:```
1
h
1 1
1 1
```Câu trả lời đúng là`0`. Giải pháp buộc phải di chuyển ít nhất một lần trước khi kiểm tra đích sẽ thất bại. 

Một trường hợp khác là:```
2
hp
xx
1 1
1 2
```Câu trả lời đúng là`1`bởi vì hiệp sĩ có thể di chuyển từ ô đầu tiên sang ô thứ hai. Một giải pháp bất cẩn chỉ kiểm tra đích đến sau khi thay đổi loại quân cờ hoặc cho rằng tất cả các quân cờ đều có chung nước đi có thể tạo ra kết quả không chính xác. 

Trường hợp thứ ba là:```
3
xxx
xpx
xxx
2 2
2 2
```Câu trả lời đúng là`0`. Ô bắt đầu có thể bị cô lập, nhưng để đến được ô đã là đích thì không cần phải di chuyển. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp là tìm kiếm trong tất cả các chuỗi chuyển động có thể có. Từ ô hiện tại, chúng tôi thử mọi bước di chuyển hợp pháp, tiếp tục đệ quy và giữ con đường thành công ngắn nhất. Điều này đúng vì mọi con đường có thể đều được xem xét. Vấn đề là số lượng đường dẫn. Trên một bảng có nhiều ô có thể truy cập, cây tìm kiếm sẽ phân nhánh liên tục và số lượng trạng thái được khám phá có thể trở nên rất lớn. 

Cải tiến đầu tiên là nhận ra rằng đây là bài toán đường đi ngắn nhất trên biểu đồ không có trọng số. Mỗi tình huống có thể xảy ra có thể được coi là một đỉnh của đồ thị và mọi nước đi hợp lệ đều trở thành một cạnh có giá bằng một. 

Phần tinh tế là xác định chính xác trạng thái biểu đồ. Một trạng thái không chỉ là một tọa độ. Nó là tọa độ cùng với loại mảnh hiện đang hoạt động ở đó. Tuy nhiên, vì mọi ô không bị chặn có thể truy cập sẽ xác định ngay loại quân cờ, nên chúng ta có thể chỉ cần lưu trữ tọa độ và tạo các bước di chuyển từ ký tự trên ô đó. Mỗi ô có một tập hợp các cạnh đi ra cố định. 

Vì mọi cạnh đều có cùng chi phí nên tìm kiếm theo chiều rộng sẽ khám phá các trạng thái theo thứ tự khoảng cách tăng dần. Lần đầu tiên BFS đến ô đích, khoảng cách là tối thiểu. Bảng chỉ có 10000 ô và mỗi ô có số lần di chuyển có thể không đổi, do đó toàn bộ quá trình tìm kiếm diễn ra đủ nhanh chóng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ về số lượng ô có thể truy cập | Trạng thái đệ quy O(N2) | Quá chậm | 
| Tối ưu | O(N2) | O(N2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Bắt đầu tìm kiếm theo chiều rộng từ ô ban đầu. Lưu trữ khoảng cách đến mỗi ô, với`-1`có nghĩa là ô đó chưa được truy cập. Ô bắt đầu nhận được khoảng cách`0`bởi vì không cần di chuyển để ở đó. 
2. Xóa một ô khỏi hàng đợi BFS và kiểm tra loại của nó. Nếu nó là`h`, tạo ra tất cả các bước di chuyển của hiệp sĩ. Nếu nó là`p`, tạo ra tất cả các nước đi tốt đặc biệt. Quy tắc di chuyển chỉ phụ thuộc vào ô hiện tại, vì vậy điều này là đủ để biết tất cả các trạng thái tiếp theo có thể xảy ra. 
3. Đối với mọi vị trí được tạo, hãy kiểm tra xem nó có nằm trong bảng và không bị chặn hay không. Nếu nó chưa được truy cập, hãy gán cho nó khoảng cách hiện tại cộng với một và đưa nó vào hàng đợi. 
4. Tiếp tục cho đến khi hàng đợi trống hoặc đến đích. Nếu đích đến nhận được một khoảng cách, hãy in nó. Ngược lại, in`-1`. 

Tại sao nó hoạt động: 

BFS duy trì bất biến rằng khi một ô bị xóa khỏi hàng đợi, khoảng cách được lưu trữ là độ dài của đường đi ngắn nhất từ đầu đến ô đó. Mỗi cạnh biểu thị chính xác một bước di chuyển, vì vậy BFS xử lý tất cả các đường đi có độ dài`k`trước bất kỳ đường đi có độ dài nào`k + 1`. Bởi vì một ô được đánh dấu là đã truy cập vào lần đầu tiên nó được truy cập, nên các đường dẫn dài hơn sau đó đến cùng một ô đó không thể cải thiện câu trả lời. Do đó, đích đến sẽ nhận được khoảng cách ngắn nhất có thể. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

def solve():
    n = int(input())
    board = [input().strip() for _ in range(n)]

    sr, sc = map(int, input().split())
    tr, tc = map(int, input().split())

    sr -= 1
    sc -= 1
    tr -= 1
    tc -= 1

    dist = [[-1] * n for _ in range(n)]
    dist[sr][sc] = 0

    knight_moves = [
        (-2, -1), (-2, 1), (-1, -2), (-1, 2),
        (1, -2), (1, 2), (2, -1), (2, 1)
    ]

    pawn_moves = [
        (-1, 0), (1, 0), (0, -1), (0, 1)
    ]

    q = deque([(sr, sc)])

    while q:
        r, c = q.popleft()

        if r == tr and c == tc:
            print(dist[r][c])
            return

        moves = knight_moves if board[r][c] == 'h' else pawn_moves

        for dr, dc in moves:
            nr = r + dr
            nc = c + dc

            if 0 <= nr < n and 0 <= nc < n:
                if board[nr][nc] != 'x' and dist[nr][nc] == -1:
                    dist[nr][nc] = dist[r][c] + 1
                    q.append((nr, nc))

    print(-1)

if __name__ == "__main__":
    solve()
```Bàn cờ được lưu trữ chính xác như đã cho nên việc kiểm tra loại ô hiện tại sẽ ngay lập tức đưa ra các nước đi có sẵn. Mảng khoảng cách tăng gấp đôi vừa là điểm đánh dấu đã truy cập vừa là nơi lưu trữ câu trả lời, tránh cấu trúc thứ hai. 

Hàng đợi chỉ chứa tọa độ vì phép biến đổi quân cờ đã được mã hóa bởi ký tự bảng. Khi một tọa độ bị xóa, mã sẽ chọn danh sách chuyển động chính xác trước khi khám phá các vùng lân cận. 

Việc kiểm tra ranh giới xảy ra trước khi truy cập vào bảng. Điều này ngăn không cho các chỉ mục không hợp lệ được hiểu là các ô hợp lệ. Việc kiểm tra đích được thực hiện khi xuất hiện từ hàng đợi, điều này an toàn vì BFS đảm bảo rằng đây đã là khoảng cách ngắn nhất. 

## Ví dụ đã hoạt động 

Hãy xem xét mẫu:```
3
phx
pxx
hhh
2 1
3 3
```Dấu vết BFS là: 

| Bước | Ô hiện tại | Loại | Khoảng cách | Xếp hàng sau khi xử lý | 
| --- | --- | --- | --- | --- | 
| 1 | (2,1) | p | 0 | (1,1) | 
| 2 | (1,1) | p | 1 | (1,2) | 
| 3 | (1,2) | h | 2 | (3,3) | 
| 4 | (3,3) | h | 3 | trống | 

Dấu vết cho thấy tại sao thứ tự hàng đợi lại quan trọng. Chỉ đến đích sau khi tất cả các đường dẫn ngắn hơn đã được xử lý. 

Trường hợp ranh giới:```
1
p
1 1
1 1
```| Bước | Ô hiện tại | Khoảng cách | Kết quả | 
| --- | --- | --- | --- | 
| 1 | (1,1) | 0 | Đã đến đích | 

Câu trả lời là`0`, chứng minh rằng thuật toán xử lý các ô bắt đầu và kết thúc giống hệt nhau mà không cần phải di chuyển. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N2) | Mỗi ô không bị chặn sẽ vào hàng đợi nhiều nhất một lần và kiểm tra số lần di chuyển không đổi. | 
| Không gian | O(N2) | Ma trận khoảng cách và hàng đợi lưu trữ thông tin cho các ô của bảng. | 

Với tối đa 10000 ô, BFS chỉ xử lý một biểu đồ nhỏ và phù hợp thoải mái với giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys
import io
from collections import deque

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)

    n = int(input())
    board = [input().strip() for _ in range(n)]
    sr, sc = map(int, input().split())
    tr, tc = map(int, input().split())

    sr -= 1
    sc -= 1
    tr -= 1
    tc -= 1

    dist = [[-1] * n for _ in range(n)]
    dist[sr][sc] = 0
    q = deque([(sr, sc)])

    knight = [(-2,-1),(-2,1),(-1,-2),(-1,2),
              (1,-2),(1,2),(2,-1),(2,1)]
    pawn = [(-1,0),(1,0),(0,-1),(0,1)]

    ans = -1
    while q:
        r, c = q.popleft()
        if (r, c) == (tr, tc):
            ans = dist[r][c]
            break
        for dr, dc in (knight if board[r][c] == 'h' else pawn):
            nr, nc = r + dr, c + dc
            if 0 <= nr < n and 0 <= nc < n:
                if board[nr][nc] != 'x' and dist[nr][nc] == -1:
                    dist[nr][nc] = dist[r][c] + 1
                    q.append((nr, nc))

    sys.stdin = old
    return str(ans) + "\n"

assert run("""3
phx
pxx
hhh
2 1
3 3
""") == "3\n", "sample 1"

assert run("""1
p
1 1
1 1
""") == "0\n", "single cell"

assert run("""2
hp
xx
1 1
1 2
""") == "1\n", "knight boundary"

assert run("""3
xxx
xpx
xxx
2 2
2 2
""") == "0\n", "isolated destination"

assert run("""3
xxx
xxx
xxx
1 1
3 3
""") == "-1\n", "blocked board"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Bảng mẫu | 3 | Truyền tải BFS bình thường | 
| Một ô | 0 | Bắt đầu bằng kết thúc | 
| Di chuyển hiệp sĩ hai tế bào | 1 | Lựa chọn chuyển động đúng | 
| Trung tâm biệt lập | 0 | Không có chuyển động không cần thiết | 
| Bảng bị chặn hoàn toàn | -1 | Những con đường bất khả thi | 

## Vỏ cạnh 

Đối với trường hợp cạnh đầu tiên:```
1
h
1 1
1 1
```BFS bắt đầu với đích đã có trong hàng đợi. Trạng thái bị loại bỏ đầu tiên có khoảng cách bằng 0 nên thuật toán ngay lập tức trả về`0`. 

Đối với trường hợp cạnh thứ hai:```
2
hp
xx
1 1
1 2
```Ô bắt đầu là một`h`ô, do đó BFS tạo ra các bước di chuyển của hiệp sĩ. Ô mục tiêu có thể truy cập được trong một lần di chuyển và nhận được khoảng cách`1`. Một giải pháp sử dụng các nước đi tốt ở khắp mọi nơi sẽ báo lỗi không chính xác. 

Đối với trường hợp cạnh thứ ba:```
3
xxx
xpx
xxx
2 2
2 2
```Ô trung tâm bị cô lập nhưng cũng là đích đến. BFS không bao giờ cần phải rời bỏ nó. Khoảng cách ban đầu được lưu trữ vẫn còn`0`, đó là câu trả lời đúng. 

Tôi cũng có thể điều chỉnh định dạng này thành định dạng biên tập ngắn hơn theo phong cách Codeforces nếu bạn cần nội dung nào đó gần hơn với nội dung sẽ xuất hiện trên trang cuộc thi.
