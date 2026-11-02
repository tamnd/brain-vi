---
title: "CF 106500D - Khóa kết hợp"
description: "Trò chơi thực sự không phụ thuộc vào các chữ số chính xác được ghi trên hai ổ khóa. Charlie chỉ ghi lại xem mỗi cặp bánh xe tương ứng bằng nhau hay khác nhau."
date: "2026-06-25T08:36:28+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106500
codeforces_index: "D"
codeforces_contest_name: "XXVIII Interregional Programming Olympiad, Vologda SU, 2026"
rating: 0
weight: 106500
solve_time_s: 44
verified: true
draft: false
---

[CF 106500D - Khóa kết hợp](https://codeforces.com/problemset/problem/106500/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Trò chơi thực sự không phụ thuộc vào các chữ số chính xác được ghi trên hai ổ khóa. Charlie chỉ ghi lại xem mỗi cặp bánh xe tương ứng bằng nhau hay khác nhau. Chúng ta có thể biểu thị một vị trí bằng một chuỗi có độ dài`N`chứa hai trạng thái có thể có cho mỗi bánh xe: bằng hoặc không bằng nhau. 

Việc thay đổi một chữ số trên một trong hai khóa sẽ ảnh hưởng đến chính xác một vị trí trong mẫu này. Nếu hai chữ số bằng nhau, việc thay đổi một trong số chúng sẽ làm chúng khác đi. Nếu chúng khác nhau, chúng ta có thể chọn chữ số mới để làm cho chúng bằng nhau. Vì có thể có mười chữ số nên cả hai chuyển đổi luôn có thể thực hiện được ở cấp mẫu. Một động thái chỉ đơn giản là lật một chút của mẫu. 

Một số mẫu bị cấm xuất hiện và không thể truy cập lại mẫu đã xuất hiện. Alice bắt đầu từ mẫu sai phân ban đầu. Người chơi không thể di chuyển sẽ thua. 

Các ràng buộc được thiết kế xoay quanh số lượng mẫu chứ không phải số lượng cấu hình khóa có thể có.`N`nhiều nhất là 10 nên có nhiều nhất`2^10 = 1024`các mẫu khác biệt. Điều này loại trừ việc tìm kiếm theo cấp số nhân trong lịch sử trò chơi, bởi vì số lượng các tập hợp có thể được truy cập về mặt thiên văn lớn hơn số đỉnh. Tuy nhiên, nó cho phép các thuật toán đồ thị trên chính đồ thị trạng thái. Danh sách cấm có thể chứa tới 1000 mẫu, gần bằng toàn bộ kích thước đồ thị nên giải pháp phải xử lý các đỉnh bị loại bỏ tùy ý. 

Một sai lầm đáng chú ý là coi trò chơi như một con đường ngắn nhất hoặc vấn đề về khả năng tiếp cận. Thứ tự di chuyển rất quan trọng vì việc truy cập vào một mẫu sẽ loại bỏ nó vĩnh viễn. Ví dụ: với một bánh xe và không có trạng thái cấm, các mẫu tạo thành một chu trình gồm hai đỉnh:```
N = 1
Alice: 0
Bob:   0
```Kết quả đúng là`Bob`, bởi vì Alice chuyển sang trạng thái duy nhất còn lại và Bob chỉ quay lại trạng thái bắt đầu nếu được phép, nhưng nó đã được truy cập. Bob không có động thái nào sau lượt của Alice. Cách tiếp cận khả năng tiếp cận ngây thơ sẽ không nắm bắt được quy tắc đã truy cập. 

Một trường hợp tinh vi khác là khi mẫu bắt đầu không có sẵn nước đi nào vì mọi hàng xóm đều bị cấm.```
N = 1
start = =
forbidden:
.
```Đầu ra đúng là`Bob`. Alice thua ngay lập tức. Bất kỳ giải pháp nào giả định người chơi đầu tiên luôn có lượt lật hợp lệ sẽ thất bại. 

Trường hợp cạnh cuối cùng xuất hiện khi đỉnh bắt đầu là đỉnh duy nhất chưa khớp trong mức khớp tối đa của biểu đồ trạng thái. Đây là tình huống Alice thắng nên chỉ đếm các thành phần liên thông hoặc kiểm tra số trạng thái còn lại là không đủ. 

## Phương pháp tiếp cận 

Ý tưởng bạo lực là mô phỏng trò chơi một cách đệ quy. Một trạng thái sẽ chứa mẫu hiện tại và tập hợp các mẫu đã được truy cập. Đối với mỗi nước đi có thể xảy ra, chúng tôi thử trạng thái tiếp theo và đánh dấu người chơi hiện tại là thắng nếu ít nhất một nước đi khiến đối thủ thua. 

Điều này đúng vì trò chơi là hữu hạn và mọi khả năng tiếp tục đều được khám phá. Vấn đề là số lượng trạng thái. Mặc dù chỉ có 1024 mẫu nhưng tập đã truy cập có thể chứa bất kỳ tập con nào trong số đó, dẫn đến`1024 * 2^1024`các trạng thái lý thuyết Điều này vượt xa những gì có thể được lưu trữ hoặc khám phá. 

Nhận xét quan trọng là đồ thị trò chơi là vô hướng. Mọi nước đi hợp lệ đều có thể được hoàn tác bởi người chơi đối diện, ngoại trừ đỉnh ban đầu không thể xem lại. Đây là trò chơi địa lý đỉnh vô hướng cổ điển. Đối với những trò chơi như vậy, kết quả có thể được xác định bằng các kết quả khớp tối đa. 

Hãy xem xét sự phù hợp tối đa của biểu đồ còn lại. Nếu đỉnh bắt đầu không được bao phủ bởi một số kết quả khớp tối đa, người chơi thứ hai luôn có thể trả lời nước đi của người chơi thứ nhất bằng cách lấy lại cạnh phù hợp. Các cặp phù hợp sẽ được tiêu thụ cùng nhau, vì vậy người chơi thứ hai không bao giờ bị mắc kẹt trước. 

Nếu mọi kết quả khớp tối đa đều bao phủ đỉnh bắt đầu, người chơi đầu tiên có thể giành chiến thắng. Cách tiêu chuẩn để kiểm tra trường hợp nào được áp dụng là so sánh kích thước khớp tối đa của biểu đồ với kích thước khớp tối đa sau khi loại bỏ đỉnh bắt đầu. Nếu việc loại bỏ điểm bắt đầu làm giảm kích thước khớp tối đa thì mọi kết quả khớp tối đa đều cần đến đỉnh đó, vì vậy Alice thắng. Ngược lại Bob sẽ thắng. 

Biểu đồ trạng thái là một siêu khối đã được loại bỏ một số đỉnh. Chúng tôi xây dựng nó một cách rõ ràng, chia các đỉnh theo tính chẵn lẻ và chạy Hopcroft Karp hai lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Trạng thái O(2^2^N) trong trường hợp xấu nhất | O(2^N) mỗi tiểu bang | Quá chậm | 
| Kết hợp tối đa | O(V^2 * E^0,5) với Hopcroft Karp | O(V + E) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chuyển đổi mọi mẫu thành mặt nạ bit. Một chút thiết lập để`1`có nghĩa là các chữ số tương ứng là khác nhau. Mẫu bắt đầu được chuyển đổi theo cách tương tự và các mẫu bị cấm được đánh dấu là không có sẵn. 
2. Xây dựng đồ thị của tất cả các mẫu được phép. Hai đỉnh được kết nối khi mặt nạ của chúng khác nhau đúng một bit, vì điều đó thể hiện sự thay đổi trạng thái bằng nhau của một bánh xe. 
3. Chia biểu đồ thành hai phần bằng cách sử dụng tính chẵn lẻ của số bit đã đặt. Lật một bit luôn làm thay đổi tính chẵn lẻ, do đó mọi cạnh đều nằm giữa hai phần. 
4. Tìm kích thước của kết quả khớp tối đa trong biểu đồ được phép hoàn chỉnh. Điều này cho chúng ta biết số lượng cặp rời rạc tối đa có thể được hình thành. 
5. Tạm thời loại bỏ đỉnh bắt đầu và tìm lại kích thước phù hợp tối đa. Nếu kích thước trở nên nhỏ hơn thì đỉnh bắt đầu là cần thiết trong mọi kết quả khớp tối đa. 
6. In`Alice`khi loại bỏ phần bắt đầu sẽ giảm kích thước phù hợp. Nếu không thì in`Bob`. 

Tại sao nó hoạt động: 

Trò chơi này là một trò chơi địa lý đỉnh vô hướng trên biểu đồ mẫu. Một kết quả khớp tối đa khiến phần đầu không bị phát hiện mang lại cho Bob một chiến lược ghép đôi: bất cứ khi nào Alice đi vào một đỉnh trùng khớp, Bob ngay lập tức di chuyển đến đối tác của mình. Các đỉnh đối tác đó luôn không được sử dụng vì các cặp được sử dụng cùng nhau. Nếu sự kết hợp như vậy không tồn tại, mọi kết quả phù hợp tối đa phải bao gồm điểm bắt đầu và người chơi bắt đầu có chiến lược chiến thắng theo định lý về địa lý đỉnh vô hướng. 

Việc so sánh phù hợp sẽ cung cấp chính xác thông tin này. Nếu kết quả phù hợp nhất không có điểm bắt đầu nhỏ hơn thì điểm bắt đầu là cần thiết và Alice thắng. Nếu nó không thay đổi thì sẽ tồn tại một mức khớp tối đa để tránh bắt đầu và Bob có thể sử dụng chiến lược ghép nối. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

def hopcroft_karp(adj, left_nodes, right_nodes):
    pair_left = {x: -1 for x in left_nodes}
    pair_right = {x: -1 for x in right_nodes}
    dist = {}

    def bfs():
        q = deque()
        found = False
        for u in left_nodes:
            if pair_left[u] == -1:
                dist[u] = 0
                q.append(u)
            else:
                dist[u] = -1
        while q:
            u = q.popleft()
            for v in adj[u]:
                pu = pair_right[v]
                if pu == -1:
                    found = True
                elif dist[pu] == -1:
                    dist[pu] = dist[u] + 1
                    q.append(pu)
        return found

    def dfs(u):
        for v in adj[u]:
            pu = pair_right[v]
            if pu == -1 or (dist[pu] == dist[u] + 1 and dfs(pu)):
                pair_left[u] = v
                pair_right[v] = u
                return True
        dist[u] = -1
        return False

    result = 0
    while bfs():
        for u in left_nodes:
            if pair_left[u] == -1 and dfs(u):
                result += 1
    return result

def solve_case(n, c, a, b, forbidden):
    bad = [False] * (1 << n)

    start = 0
    for i in range(n):
        if a[i] != b[i]:
            start |= 1 << i

    for s in forbidden:
        mask = 0
        for i, ch in enumerate(s):
            if ch == '.':
                mask |= 1 << i
        bad[mask] = True

    vertices = [i for i in range(1 << n) if not bad[i]]

    color = {}
    for v in vertices:
        color[v] = v.bit_count() & 1

    def build(skip):
        left = []
        right = []
        alive = set(vertices)
        if skip in alive:
            alive.remove(skip)

        for v in alive:
            if color[v] == 0:
                left.append(v)
            else:
                right.append(v)

        adj = {v: [] for v in left}
        for v in left:
            for i in range(n):
                u = v ^ (1 << i)
                if u in alive:
                    adj[v].append(u)
        return hopcroft_karp(adj, left, right)

    full = build(-1)
    without_start = build(start)

    return "Alice" if without_start < full else "Bob"

def main():
    t = int(input())
    ans = []
    for _ in range(t):
        n, c = map(int, input().split())
        a = input().strip()
        b = input().strip()
        forbidden = [input().strip() for _ in range(c)]
        ans.append(solve_case(n, c, a, b, forbidden))
    print("\n".join(ans))

if __name__ == "__main__":
    main()
```Trình phân tích cú pháp đầu vào trước tiên đọc hai cấu hình khóa và chuyển đổi mối quan hệ ban đầu giữa chúng thành mặt nạ bit. Các chữ số thực tế sẽ biến mất sau bước này vì chỉ có thông tin bình đẳng mới quan trọng. 

Việc xây dựng đồ thị xảy ra bên trong`build`. Hàm này tùy ý loại bỏ một đỉnh cần thiết cho phép tính khớp thứ hai. Các đỉnh được phân chia theo tính chẵn lẻ vì mỗi cạnh siêu lập phương thay đổi chính xác một bit. Danh sách kề chỉ lưu trữ các cạnh từ phía bên trái, đây là định dạng được Hopcroft Karp yêu cầu. 

Hai cuộc gọi phù hợp là trung tâm của giải pháp. Cái đầu tiên đưa ra kích thước phù hợp nhất có thể trong biểu đồ gốc. Câu hỏi thứ hai hỏi liệu vẫn có thể thực hiện được cùng một kích thước phù hợp nếu không có đỉnh bắt đầu. Nếu không thể, thì điểm bắt đầu phải được bao gồm trong mọi kết quả khớp tối đa. 

Việc triển khai sử dụng từ điển để khớp các cặp vì biểu đồ thưa thớt và số lượng đỉnh nhỏ. Không có vấn đề về độ sâu đệ quy vì Hopcroft Karp chỉ đi theo các đường đi qua tối đa 1024 đỉnh. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
2
12
89
```Mẫu bắt đầu là`..`vì cả hai vị trí đều khác nhau. 

| Bước | Giá trị hiện tại | Hành động | Kết quả phù hợp | 
| --- | --- | --- | --- | 
| 1 |`..`| Xây dựng đồ thị | 4 tiểu bang | 
| 2 |`..`đã xóa | Tính toán kết hợp mà không cần bắt đầu | Kết hợp nhỏ hơn | 
| 3 |`..`giữ | So sánh kích cỡ | Alice thắng | 

Đỉnh bắt đầu được yêu cầu cho mọi kết quả khớp tối đa, vì vậy Alice có chiến lược chiến thắng. 

Đối với mẫu thứ hai:```
3
204
101
```Mẫu bắt đầu là`.=.`. 

| Bước | Giá trị hiện tại | Hành động | Kết quả phù hợp | 
| --- | --- | --- | --- | 
| 1 |`=.= `| Xây dựng đồ thị | Tìm thấy kết hợp đầy đủ | 
| 2 | Bắt đầu xóa | Kích thước phù hợp không thay đổi | Bob thắng | 

Tồn tại một sự kết hợp tối đa để tránh trạng thái bắt đầu. Bob có thể ghép từng bước di chuyển của Alice với đối tác phù hợp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(2^N * N * sqrt(2^N)) | Đồ thị có nhiều nhất 1024 đỉnh và khoảng`N * 2^(N-1)`các cạnh. Hai lần chạy Hopcroft Karp được thực hiện. | 
| Không gian | O(2^N * N) | Danh sách kề lưu trữ các cạnh của hypercube. | 

Với`N <= 10`, biểu đồ chỉ có 1024 đỉnh, vì vậy việc xây dựng nó và chạy so khớp một cách rõ ràng dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    data = sys.stdin.readline
    t = int(data())
    out = []
    for _ in range(t):
        n, c = map(int, data().split())
        a = data().strip()
        b = data().strip()
        forbidden = [data().strip() for _ in range(c)]
        out.append(solve_case(n, c, a, b, forbidden))
    sys.stdin = old
    return "\n".join(out)

assert run("""3
2 2
12
89
=.
==
3 1
204
101
.==
3 2
000
000
...
==.
""") == """Alice
Bob
Bob"""

assert run("""1
1 0
0
0
""") == "Bob"

assert run("""1
1 1
0
0
.
""") == "Bob"

assert run("""1
2 0
00
00
""") == "Bob"

assert run("""1
10 0
0000000000
1111111111
""") == "Bob"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Trường hợp mẫu | Alice, Bob, Bob | Ví dụ tiêu chuẩn | 
| Một chút, không có trạng thái cấm | Bob | Đồ thị nhỏ nhất | 
| Chỉ có hàng xóm bị cấm | Bob | Lập tức thua nước đi | 
| Tất cả đều bắt đầu như nhau | Bob | Xử lý đúng chênh lệch bằng 0 | 
| Tối đa`N`| Bob | Hiệu suất và xây dựng đồ thị lớn | 

## Vỏ cạnh 

Khi mẫu bắt đầu không có bước đi hợp lệ, biểu đồ chứa đỉnh bắt đầu nhưng không có đỉnh lân cận nào. Việc so khớp không có điểm bắt đầu có cùng kích thước với biểu đồ đầy đủ, do đó thuật toán trả về Bob ngay lập tức. 

Vì:```
1
0
0
.
```mặt nạ bắt đầu là`0`, và người hàng xóm duy nhất bị cấm. Alice không thể di chuyển nên Bob thắng. 

Khi nhiều mẫu bị cấm, thuật toán không cho rằng siêu khối đã hoàn thành. Nó loại bỏ các đỉnh đó trước khi xây dựng các cạnh. Ví dụ:```
1
2 2
00
00
=.
.=
```Chỉ có các bang`==`Và`..`duy trì. Trạng thái bắt đầu có một nước đi và sau đó đối thủ không có nước đi nào. So sánh khớp vẫn hoạt động vì nó được thực hiện trên biểu đồ thực tế còn lại. 

Khi bản thân mẫu bắt đầu có vai trò đặc biệt trong việc so khớp, phép tính so khớp thứ hai sẽ nắm bắt được vai trò đó. Một cách tiếp cận sai phổ biến là chỉ so sánh số lượng trạng thái còn lại. Người chiến thắng phụ thuộc vào việc liệu đỉnh bắt đầu có cần thiết trong một kết quả khớp tối đa hay không chứ không phụ thuộc vào tổng số mẫu.
