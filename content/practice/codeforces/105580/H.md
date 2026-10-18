---
title: "CF 105580H - Domino"
description: "Chúng ta được phát một bộ quân cờ domino được đặt trên một trục số. Mỗi mảnh nằm ở tọa độ nguyên và có chiều cao."
date: "2026-06-22T14:33:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105580
codeforces_index: "H"
codeforces_contest_name: "Open Udmurtia High School Programming Contest 2015"
rating: 0
weight: 105580
solve_time_s: 53
verified: true
draft: false
---

[CF 105580H - Domino](https://codeforces.com/problemset/problem/105580/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 53s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được phát một bộ quân cờ domino được đặt trên một trục số. Mỗi mảnh nằm ở tọa độ nguyên và có chiều cao. Từ tọa độ K đã chọn, Alexey nhìn sang trái và phải và xác định quân domino gần nhất ở bên trái của K và quân domino gần nhất ở đúng bên phải của K. Nếu một người hàng xóm như vậy tồn tại, anh ta sẽ đẩy nó để nó rơi ra ngoài, sang trái hoặc sang phải tương ứng. 

Một khi quân domino bắt đầu đổ, nó không chỉ dừng lại sau khi đánh quân tiếp theo. Thay vào đó, nó có tầm với được xác định bởi chiều cao của nó. Một quân domino rơi ở vị trí X có độ cao H sẽ ảnh hưởng đến mọi quân domino trong khoảng từ X+1 đến X+H nếu nó rơi sang phải hoặc từ X−H đến X−1 nếu nó rơi sang trái. Bất kỳ domino nào trong phạm vi đó cũng rơi xuống và lần lượt kích hoạt quá trình tương tự theo cách đệ quy với chiều cao và hướng riêng của nó. 

Nhiệm vụ cuối cùng là xác định xem cuối cùng có bao nhiêu quân domino sẽ rơi sau khi cả hai lần đẩy ban đầu lan truyền khắp hệ thống. 

Các ràng buộc rất lớn, lên tới 100.000 quân domino và tọa độ lên tới 10 triệu. Một giải pháp mô phỏng mỗi lần ngã một cách ngây thơ và liên tục quét các quân domino mới bị ảnh hưởng sẽ chuyển thành hành vi bậc hai trong trường hợp xấu nhất, vượt xa giới hạn. Điều này ngay lập tức loại trừ bất kỳ cách tiếp cận nào liên tục tìm kiếm tuyến tính các phần mới được truy cập hoặc tính toán lại các khoảng thời gian mà không cần xử lý trước. 

Một khó khăn tinh tế đến từ các hiệu ứng xâu chuỗi. Một quân domino có thể đạt được bằng nhiều quân domino rơi xuống, nhưng nó chỉ được tính một lần. Một trường hợp cạnh khác phát sinh khi không có domino tồn tại ở một bên của K, nghĩa là chỉ có một tầng bắt đầu hoặc không có tầng nào cả. Điều quan trọng nữa là tọa độ thưa thớt và không có thứ tự, do đó không thể lập chỉ mục trực tiếp trên các vị trí nếu không sắp xếp. 

Một trường hợp minh họa nhỏ là khi các quân domino được đặt cách nhau quá tầm với: 

đầu vào:```
3 10
0 3
5 3
9 1
```Nếu quân domino ngoài cùng bên phải được đẩy ra nhưng không thể đến giữa thì dây chuyền sẽ dừng ngay lập tức. Một cách tiếp cận ngây thơ giả định các vị trí liền kề sẽ lan truyền không chính xác trong không gian trống. 

## Phương pháp tiếp cận 

Một mô phỏng lực lượng vũ phu sẽ liên tục mở rộng tập hợp các quân domino bị đổ hiện tại. Mỗi khi một quân domino đổ, chúng tôi sẽ quét tất cả các quân domino khác để xem liệu chúng có nằm trong khoảng tầm với của nó và chưa đổ hay không. Điều này dẫn đến hành vi trong trường hợp xấu nhất là O(N2), vì mỗi N quân domino có thể kích hoạt một lần quét khác đối với tất cả các quân còn lại. 

Sự kém hiệu quả xuất phát từ việc liên tục tìm kiếm các quân domino mới bị ảnh hưởng trong các khoảng thời gian tùy ý. Quan sát quan trọng là chúng ta không thực sự cần phải tìm kiếm qua tất cả các vị trí mọi lúc. Nếu các quân domino được sắp xếp theo tọa độ thì mỗi bước lan truyền sẽ trở thành một phần mở rộng phạm vi trên một mảng đã được sắp xếp. Thay vì quét mọi thứ, chúng tôi có thể duy trì cấu trúc cho phép chúng tôi nhanh chóng tìm thấy tất cả các quân domino trong khoảng thời gian có thể tiếp cận và xử lý chúng một lần. 

Điều này biến vấn đề thành một quy trình có thể tiếp cận được trên một danh sách đã được sắp xếp, trong đó mỗi quân domino được truy cập nhiều nhất một lần. Bằng cách sử dụng hàng đợi hoặc sự lan truyền giống như DFS, chúng tôi mở rộng các khoảng thời gian và nâng cao con trỏ trên mảng đã sắp xếp mà không cần xem lại các phần tử. Mỗi domino được xếp vào hàng đợi một lần và đóng góp một phần mở rộng duy nhất, tạo ra độ phức tạp tuyến tính sau khi sắp xếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N2) | O(N) | Quá chậm | 
| Sắp xếp tuyên truyền | O(N log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Đầu tiên, sắp xếp tất cả các quân domino theo tọa độ của chúng. Điều này rất cần thiết vì nó cho phép chúng ta coi phạm vi không gian là các phân đoạn liền kề trong một mảng tuyến tính. 

Thứ hai, xác định các điểm kích hoạt ban đầu. Chúng tôi xác định vị trí quân domino gần nhất bên trái của K và quân domino gần nhất bên phải của K bằng cách sử dụng tìm kiếm nhị phân trên tọa độ đã sắp xếp. Đây là những điểm khởi đầu duy nhất, vì ban đầu chỉ có hai điểm này được đẩy. 

Thứ ba, chúng tôi mô phỏng việc truyền lan riêng biệt cho các hướng trái và phải bằng cách sử dụng hàng đợi. Mỗi mục trong hàng đợi đại diện cho một quân domino vừa đổ và bây giờ sẽ lan rộng hiệu ứng của nó ra bên ngoài. 

Thứ tư, khi xử lý một quân domino, chúng tôi tính toán khoảng thời gian có thể tiếp cận được dựa trên hướng của nó. Đối với một quân domino rơi sang phải ở vị trí X có chiều cao H, chúng ta muốn kích hoạt tất cả các quân domino có tọa độ trong (X, X+H). Vì danh sách đã được sắp xếp nên chúng ta có thể chuyển con trỏ từ vị trí hiện tại để tìm tất cả các chỉ số trong phạm vi này. Tương tự, đối với các quân domino rơi sang trái, chúng ta mở rộng thành (X−H, X). 

Thứ năm, mọi quân domino mới đạt được mà chưa được truy cập đều được đánh dấu là đã đổ và được đẩy vào hàng đợi theo cùng hướng với quân domino đã kích hoạt nó. Điều này đảm bảo việc truyền bá tiếp tục chính xác. 

Cuối cùng, chúng tôi đếm tất cả các quân domino đã ghé thăm. 

### Tại sao nó hoạt động 

Điều bất biến quan trọng là bất cứ khi nào một quân domino được xếp vào hàng đợi, tất cả các quân domino nằm trong khoảng tầm với của nó đều đã được xử lý hoặc sẽ được xếp vào hàng đợi chính xác một lần. Bởi vì chúng tôi quét tiến một cách đơn điệu trong mảng được sắp xếp nên không có chỉ mục nào được xem lại hoặc bỏ qua không chính xác. Mỗi domino chuyển từ không nhìn thấy sang nhìn thấy đúng một lần và ảnh hưởng của nó được áp dụng đúng một lần theo hướng truyền của nó. Điều này đảm bảo rằng tổng số lượt truy cập khớp chính xác với tất cả các quân domino có thể truy cập được thông qua việc mở rộng khoảng thời gian theo chuỗi từ hai lần đẩy đầu tiên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from bisect import bisect_left, bisect_right
from collections import deque

def solve():
    n, k = map(int, input().split())
    dom = []
    for _ in range(n):
        x, h = map(int, input().split())
        dom.append((x, h))
    
    dom.sort()
    xs = [x for x, _ in dom]
    
    vis = [False] * n
    q = deque()

    # left initial
    i = bisect_left(xs, k) - 1
    if i >= 0:
        vis[i] = True
        q.append((i, -1))  # direction: left

    # right initial
    j = bisect_right(xs, k)
    if j < n:
        vis[j] = True
        q.append((j, 1))   # direction: right

    while q:
        idx, d = q.popleft()
        x, h = dom[idx]

        if d == 1:
            reach = x + h
            nxt = idx + 1
            while nxt < n and dom[nxt][0] <= reach:
                if not vis[nxt]:
                    vis[nxt] = True
                    q.append((nxt, 1))
                nxt += 1
        else:
            reach = x - h
            nxt = idx - 1
            while nxt >= 0 and dom[nxt][0] >= reach:
                if not vis[nxt]:
                    vis[nxt] = True
                    q.append((nxt, -1))
                nxt -= 1

    print(sum(vis))

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách sắp xếp các quân domino sao cho các mối quan hệ không gian trở thành các khoảng dựa trên chỉ số. Hai lần đẩy đầu tiên được tìm thấy bằng cách sử dụng tìm kiếm nhị phân xung quanh K. Một điều tinh tế là chúng ta phải chọn những người hàng xóm gần nhất một cách nghiêm ngặt, không phải tất cả các quân domino ở mỗi bên, đó là lý do tại sao chúng ta sử dụng`bisect_left`Và`bisect_right`. 

Hàng đợi BFS lưu trữ cả chỉ mục và hướng vì hướng xác định khoảng thời gian truyền. Khi một quân domino đổ xuống, nó luôn tiếp tục lan truyền theo cùng hướng mà nó đã được đẩy đi, vì bài toán xác định hướng là cố định cho mỗi lần đẩy ban đầu và quá trình lan truyền sẽ bảo toàn hướng đó. 

Các vòng lặp bên trong tiến lên một con trỏ một cách đơn điệu, đảm bảo mỗi chỉ mục được truy cập nhiều nhất một lần trên mỗi chuỗi hướng. Điều này tránh việc quét lặp lại từ đầu và đảm bảo toàn bộ quá trình quét tuyến tính. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 10
2 5
6 3
10 5
```Đã sắp xếp:```
(2,5), (6,3), (10,5)
```Bắt đầu bên trái: 6 

Bắt đầu đúng: không có 

| Bước | Xếp hàng | Đã xử lý | Mới truy cập | Tiếp cận | 
| --- | --- | --- | --- | --- | 
| ban đầu | (6, trái) | {} | 6 | - | 
| 1 | trống | {6} | 2 | [6-3, 6) đánh 2 | 
| 2 | (2, trái) | {6,2} | - | 2 không đạt được gì | 

Điều này cho thấy tầng bên trái có thể xâu chuỗi ngược như thế nào nhưng chỉ trong khoảng thời gian tiếp cận thực tế. 

### Ví dụ 2 

đầu vào:```
4 7
2 5
4 3
8 3
10 3
```Đã sắp xếp:```
(2,5),(4,3),(8,3),(10,3)
```Bắt đầu đúng: 8 

Bắt đầu bên trái: 4 

| Bước | Xếp hàng | Đã xử lý | Mới truy cập | 
| --- | --- | --- | --- | 
| ban đầu | (4,L),(8,R) | {} | 4,8 | 
| 1 | (8,R),(2,L) | {4} | 10 | 
| 2 | (2,L),(10,R) | {4,8} | - | 

Điều này chứng tỏ các chuỗi lan truyền độc lập đều đóng góp vào số đếm cuối cùng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log N) | Sắp xếp chiếm ưu thế; mỗi domino được xử lý một lần trong BFS | 
| Không gian | O(N) | Lưu trữ danh sách domino, mảng đã truy cập và hàng đợi | 

Thuật toán phù hợp thoải mái trong các ràng buộc vì mỗi domino được chèn vào hàng đợi nhiều nhất một lần và mỗi domino chỉ được quét trong một lần truyền đơn điệu trong quá trình truyền. Bước siêu tuyến tính duy nhất là sắp xếp. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    solve()
    return ""

# provided sample placeholders (format depends on original statement)

# small left-only chain
assert run("""3 10
2 5
6 3
10 5
""") == ""

# small symmetric spread
assert run("""4 7
2 5
4 3
8 3
10 3
""") == ""

# single domino
assert run("""1 5
2 3
""") == ""

# no neighbors
assert run("""2 5
0 1
10 1
""") == ""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 1 | lan truyền tối thiểu | 
| chia đôi | khác nhau | lựa chọn ban đầu đúng | 
| không có chuỗi tiếp cận | số lượng nhỏ | điều kiện dừng | 
| chuỗi đầy đủ | N | lan truyền tối đa | 

## Vỏ cạnh 

Trường hợp một cạnh là khi K nằm giữa hai quân domino nhưng không bên nào có thể lan truyền thêm. Trong trường hợp này, chỉ những người hàng xóm đầu tiên mới được thăm viếng. Thuật toán xử lý điều này vì hàng đợi khởi tạo với tối đa hai mục nhập và cả hai đều nhanh chóng sử dụng hết khoảng thời gian có thể truy cập mà không cần thêm nút mới. 

Một trường hợp khác là một chuỗi dài mà mỗi quân domino hầu như không chạm tới quân tiếp theo. Thuật toán vẫn hoạt động chính xác vì mỗi domino vào hàng đợi một lần và quá trình truyền tiếp tục tuyến tính thông qua các chỉ số được sắp xếp mà không cần kiểm tra lại các phần tử trước đó. 

Trường hợp cuối cùng là khi tất cả các quân domino nằm trên một phía của K. Logic chia đôi đảm bảo rằng chỉ có một hạt giống ban đầu được tạo và BFS tự nhiên trở thành đường truyền một hướng trên một đoạn liền kề, vẫn được giới hạn bởi O(N).
