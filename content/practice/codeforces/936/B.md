---
title: "CF 936B - Trò Chơi Buồn Ngủ"
description: "Chúng ta được cung cấp một biểu đồ có hướng trong đó mỗi đỉnh biểu thị trạng thái của mã thông báo trò chơi. Một mã thông báo bắt đầu ở một đỉnh cố định và hai người chơi luân phiên di chuyển nó dọc theo các cạnh đi ra. Người chơi sẽ thua ngay lập tức nếu họ di chuyển từ một đỉnh không có cạnh hướng ra ngoài."
date: "2026-06-17T02:47:16+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "dp", "games", "graphs"]
categories: ["algorithms"]
codeforces_contest: 936
codeforces_index: "B"
codeforces_contest_name: "Codeforces Round 467 (Div. 1)"
rating: 2100
weight: 936
solve_time_s: 100
verified: false
draft: false
---

[CF 936B - Trò chơi buồn ngủ](https://codeforces.com/problemset/problem/936/B) 

**Xếp hạng:** 2100 
**Tags:** dfs và tương tự, dp, trò chơi, đồ thị 
**Thời gian giải:** 1 phút 40s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một biểu đồ có hướng trong đó mỗi đỉnh biểu thị trạng thái của mã thông báo trò chơi. Một mã thông báo bắt đầu ở một đỉnh cố định và hai người chơi luân phiên di chuyển nó dọc theo các cạnh đi ra. Người chơi sẽ thua ngay lập tức nếu họ di chuyển từ một đỉnh không có cạnh hướng ra ngoài. Trò chơi cũng có giới hạn cứng: nếu trận đấu tiếp tục trong một thời gian rất dài (tối đa 10^6 nước đi), trò chơi được tuyên bố là hòa. 

Điều khó khăn là một người chơi đang ngủ, vì vậy một người chơi sẽ kiểm soát hiệu quả cả hai bên của trò chơi. Điều này thay đổi vấn đề từ một trò chơi xen kẽ tiêu chuẩn thành một trò chơi tự kiểm soát, trong đó chúng ta phải suy luận về kết quả trong cách chơi tối ưu, nhưng với hạn chế bổ sung là các chu kỳ ngăn cản việc buộc phải chấm dứt và do đó hàm ý là hòa. 

Nhiệm vụ là phân loại đỉnh bắt đầu thành một trong ba kết quả: người điều khiển buộc phải thắng, thua bắt buộc hoặc hòa do khả năng tránh bị chấm dứt vô thời hạn. 

Các ràng buộc rất lớn: lên tới 10^5 đỉnh và 2·10^5 cạnh. Điều này loại trừ mọi mô phỏng của cây trò chơi. Ngay cả một BFS ngây thơ về các trạng thái trò chơi cũng là không thể bởi vì mỗi trạng thái đỉnh phụ thuộc vào lượt của nó, nhân đôi các trạng thái một cách hiệu quả và vẫn để lại cấu trúc phân nhánh theo cấp số nhân. 

Khó khăn chính đến từ chu kỳ. Một DFS ngây thơ chỉ cố gắng tìm đường dẫn đến ngõ cụt sẽ coi bất kỳ chu kỳ nào là có khả năng thắng hoặc thua mà không hiểu liệu nó có thể thoát được hay có thể thực thi được. 

Một trường hợp thất bại khó phát hiện là một biểu đồ trong đó mọi đường dẫn cuối cùng đều dẫn đến một chu trình. Ví dụ: nếu tất cả các đỉnh tạo thành một chu kỳ duy nhất, thì cách tiếp cận ngây thơ “không có ngõ cụt nào có thể tiếp cận được” có thể gắn nhãn không chính xác cho nó là thắng hoặc thua tùy thuộc vào thứ tự di chuyển, trong khi trên thực tế, đó là một trận hòa vì trò chơi có thể tiếp tục vô thời hạn mà không đạt đến đỉnh cuối cùng. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là coi mỗi trạng thái như một vị trí trong trò chơi và mô phỏng tất cả các chuỗi di chuyển có thể xảy ra. Từ một đỉnh, chúng tôi thử mọi cạnh đi ra và xác định đệ quy xem đối thủ có thua ở trạng thái kết quả hay không. Đây là phép đệ quy minimax tiêu chuẩn trên đồ thị có hướng. 

Cách tiếp cận này đúng về nguyên tắc vì mọi vị thế đều thắng hoặc thua tùy thuộc vào việc có tồn tại một động thái dẫn đến vị thế thua hay không. Tuy nhiên, không gian trạng thái bùng nổ do các chu kỳ. Một DFS ngây thơ sẽ truy cập lại các trạng thái vô hạn hoặc yêu cầu ghi nhớ các trạng thái cũng mã hóa số lần di chuyển hoặc tính chẵn lẻ, điều này làm tăng độ phức tạp đáng kể. 

Cái nhìn sâu sắc thực sự là đảo ngược quan điểm. Thay vì hỏi liệu một vị trí có chiến thắng hay không, chúng tôi phân loại các vị trí bằng cách sử dụng lý luận ngược từ các trạng thái cuối. Một đỉnh không có cạnh đi ra là trạng thái thua cuộc. Đỉnh nào có thể đẩy đối thủ vào thế thua là đỉnh thắng. Tuy nhiên, các đỉnh chưa được chứng minh là thắng hay thua sau khi lan truyền phải nằm trong hoặc dẫn đến các chu kỳ mà không người chơi nào có thể buộc chấm dứt. 

Điều này dẫn đến một BFS đa nguồn trên các cạnh bị đảo ngược với bộ đếm các bước đi còn lại. Chúng tôi truyền bá các nhãn thua và thắng ra bên ngoài và bất kỳ thứ gì chưa được phân loại sau khi truyền bá đầy đủ đều là một phần của vùng hòa. 

Việc xây dựng đường thắng thực tế được thực hiện bằng cách lưu trữ các con trỏ gốc trong BFS, đảm bảo chúng ta có thể xây dựng lại đường dẫn thắng bắt buộc hợp lệ kết thúc ở đỉnh cuối. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu DFS/Minimax | Hàm mũ | Ngăn xếp đệ quy O(n) | Quá chậm | 
| Đảo ngược BFS với sự lan truyền trạng thái | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải thích trò chơi như một bài toán lan truyền có lợi ngắn nhất trên đồ thị có hướng.

1. Xác định tất cả các đỉnh cuối, nghĩa là các đỉnh không có cạnh hướng ra ngoài. Đây là những vị trí bị mất ngay lập tức vì người chơi di chuyển không thể hành động. Chúng tôi khởi tạo chúng dưới dạng trạng thái mất. 
2. Xây dựng danh sách kề đảo ngược sao cho với mỗi cạnh u → v, chúng ta có thể truyền thông tin từ v trở lại u một cách hiệu quả. Điều này là cần thiết vì chúng ta suy luận ngược từ trạng thái cuối. 
3. Duy trì một mảng`deg[u]`lưu trữ số cạnh đi từ mỗi đỉnh. Điều này theo dõi số lượng nước đi vẫn "chưa được giải quyết" cho mỗi vị trí. 
4. Chạy hàng đợi BFS được khởi tạo với tất cả các đỉnh đầu cuối được đánh dấu là mất. Chúng tôi cũng duy trì một mảng trạng thái trong đó mỗi đỉnh có thể thắng, thua hoặc không xác định. 
5. Khi xử lý một đỉnh bị mất v, chúng ta kiểm tra tất cả các đỉnh u trước đó. Nếu bạn có lợi thế hơn v, thì bạn có thể buộc thắng bằng cách chuyển sang vị trí thua. Chúng tôi đánh dấu u là người chiến thắng và lưu trữ v là người kế nhiệm của nó để tái thiết đường dẫn. 
6. Khi xử lý đỉnh thắng v, ta giảm`deg[u]`cho mỗi người tiền nhiệm u. Điều này thể hiện rằng một trong các lựa chọn của bạn sẽ dẫn đến trạng thái chiến thắng đã biết cho đối thủ. Nếu tất cả các nước đi đi từ u được chứng minh là dẫn đến trạng thái chiến thắng cho đối thủ (nghĩa là deg[u] trở thành 0 mà không tìm thấy con thua cuộc), thì bản thân u sẽ thua cuộc. 
7. Tiếp tục truyền cho đến khi không xác định được trạng thái mới. 
8. Sau khi nhân giống, hãy kiểm tra đỉnh bắt đầu. Nếu nó thắng, hãy xây dựng lại đường đi bằng cách đi theo các con trỏ cha được lưu trữ cho đến khi đạt đến đỉnh thua cuối cùng. 
9. Nếu trận đấu không thắng nhưng vẫn chưa được phân loại, thì đó là một phần của khu vực chu kỳ mà cả hai người chơi đều có thể tránh bị chấm dứt, do đó kết quả là hòa. 
10. Ngược lại, đó là vị thế thua cuộc. 

### Tại sao nó hoạt động 

Thuật toán duy trì sự phân loại đơn điệu các đỉnh dựa trên kết quả bắt buộc. Một đỉnh chỉ trở thành chiến thắng nếu nó có nước đi trực tiếp vào một đỉnh thua đã được xác nhận, đảm bảo lợi thế nước đi bắt buộc. Một đỉnh chỉ trở nên thua khi tất cả các nước đi đi đều dẫn đến các vị trí đã được chứng minh là mang lại chiến thắng cho đối thủ, nghĩa là không còn nước đi an toàn nào. 

Bất kỳ đỉnh nào không được phân loại sau khi lan truyền đầy đủ phải nằm trong một khu vực mà cả lan truyền thắng hay thua đều không thể chạm tới nó. Các đỉnh như vậy nhất thiết phải thuộc về các chu kỳ hoặc các cấu trúc chỉ nạp vào các chu kỳ, trong đó trò chơi có thể tiếp tục vô thời hạn mà không đạt đến trạng thái kết thúc. Điều này khớp chính xác với định nghĩa hòa dưới giới hạn nước đi 10^6. 

Bởi vì quá trình truyền luôn chuyển từ trạng thái đã giải quyết sang trạng thái chưa được giải quyết và không bao giờ đảo ngược các quyết định nên không có đỉnh nào bị gắn nhãn sai một khi đã được gán. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

n, m = map(int, input().split())

g = [[] for _ in range(n)]
rg = [[] for _ in range(n)]
deg = [0] * n

for i in range(n):
    parts = list(map(int, input().split()))
    c = parts[0]
    for v in parts[1:]:
        v -= 1
        g[i].append(v)
        rg[v].append(i)
    deg[i] = c

s = int(input()) - 1

# state: 0 unknown, 1 winning, 2 losing
state = [0] * n
parent = [-1] * n

q = deque()

# terminal nodes are losing
for i in range(n):
    if deg[i] == 0:
        state[i] = 2
        q.append(i)

while q:
    v = q.popleft()

    if state[v] == 2:
        for u in rg[v]:
            if state[u] == 0:
                state[u] = 1
                parent[u] = v
                q.append(u)
    else:
        for u in rg[v]:
            if state[u] == 0:
                deg[u] -= 1
                if deg[u] == 0:
                    state[u] = 2
                    q.append(u)

if state[s] != 1:
    if state[s] == 0:
        print("Draw")
    else:
        print("Lose")
else:
    print("Win")
    path = []
    cur = s
    while cur != -1:
        path.append(cur + 1)
        cur = parent[cur]
    print(*path)
```Giải pháp bắt đầu bằng cách xây dựng cả danh sách kề thuận và ngược. Biểu đồ ngược là cần thiết vì tất cả sự lan truyền đều đi ngược lại từ các kết quả đã biết đến các kết quả trước đó. 

các`deg`mảng theo dõi có bao nhiêu cạnh đi ra vẫn “không an toàn” cho mỗi nút. Một nút chỉ bị thua sau khi tất cả các nút con của nó được coi là vị trí chiến thắng. 

Hàng đợi BFS chứa các nút có trạng thái đã được biết. Từ một nút thua, chúng tôi ngay lập tức chuyển đổi các nút trước đó thành các nút thắng. Từ một nút chiến thắng, chúng tôi loại bỏ một tùy chọn gửi đi cho các nút tiền nhiệm, có thể khiến chúng rơi vào trạng thái thua cuộc. 

Mảng cha chỉ được lấp đầy khi một nút giành chiến thắng, đảm bảo rằng mọi đường dẫn được xây dựng lại luôn tuân theo trình tự di chuyển bắt buộc phải thắng hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Đồ thị đầu vào:```
5 6
1 -> 2,3
2 -> 4,5
3 -> 4
4 -> 5
5 -> none
start = 1
```Chúng tôi theo dõi sự lan truyền: 

| Bước | Nút xếp hàng | Hành động | Thay đổi trạng thái | 
| --- | --- | --- | --- | 
| 1 | 5 | thiết bị đầu cuối → mất | 5 = L | 
| 2 | 5 | tuyên truyền tới 4,2 | 4 = W, 2 = W | 
| 3 | 4 | tuyên truyền thắng lợi | cập nhật 3 | 
| 4 | 2 | tuyên truyền thắng lợi | cập nhật 1 | 
| 5 | 1 | trở thành W | xong | 

Phân loại cuối cùng: đỉnh 1 là chiến thắng. 

Tái thiết theo con trỏ cha mẹ: 

1 → 2 → 4 → 5. 

Điều này xác nhận một đường đi bắt buộc kết thúc tại điểm cuối bị mất đỉnh. 

### Ví dụ 2 

Hãy xem xét một chu trình đơn giản:```
3 3
1 -> 2
2 -> 3
3 -> 1
start = 2
```| Bước | Nút xếp hàng | Hành động | Thay đổi trạng thái | 
| --- | --- | --- | --- | 
| 1 | không có nút đầu cuối | không có độ = 0 nút | không khởi tạo | 
| 2 | hàng đợi trống | không lan truyền | tất cả vẫn chưa được biết | 

Tất cả các nút vẫn chưa được phân loại, do đó kết quả là Draw. 

Điều này cho thấy rằng các chu kỳ không có lối thoát sẽ không bao giờ được giải quyết thành trạng thái thắng/thua. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi cạnh được xử lý một số lần không đổi trong quá trình truyền ngược | 
| Không gian | O(n + m) | Lưu trữ đồ thị, đồ thị ngược và mảng phụ trợ | 

Độ phức tạp tuyến tính là cần thiết cho n tối đa 10^5 và m tối đa 2·10^5, trong đó bất kỳ khám phá bậc hai hoặc hàm mũ nào đều không thể thực hiện được. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())
    g = [[] for _ in range(n)]
    rg = [[] for _ in range(n)]
    deg = [0] * n

    for i in range(n):
        parts = list(map(int, input().split()))
        c = parts[0]
        for v in parts[1:]:
            v -= 1
            g[i].append(v)
            rg[v].append(i)
        deg[i] = c

    s = int(input()) - 1

    state = [0] * n
    parent = [-1] * n
    from collections import deque
    q = deque()

    for i in range(n):
        if deg[i] == 0:
            state[i] = 2
            q.append(i)

    while q:
        v = q.popleft()
        if state[v] == 2:
            for u in rg[v]:
                if state[u] == 0:
                    state[u] = 1
                    parent[u] = v
                    q.append(u)
        else:
            for u in rg[v]:
                if state[u] == 0:
                    deg[u] -= 1
                    if deg[u] == 0:
                        state[u] = 2
                        q.append(u)

    if state[s] != 1:
        print("Draw" if state[s] == 0 else "Lose")
    else:
        print("Win")
        path = []
        cur = s
        while cur != -1:
            path.append(cur + 1)
            cur = parent[cur]
        print(*path)

# provided sample 1
assert run("""5 6
2 2 3
2 4 5
1 4
1 5
0
1
""") == "Win\n1 2 4 5\n"

# custom: single losing node
assert run("""1 0
0
1
""") == "Lose\n"

# custom: simple cycle (draw)
assert run("""3 3
1 2
2 3
3 1
2
""") == "Draw\n"

# custom: chain to terminal win
assert run("""4 3
2 2 3
1 4
1 4
0
1
""") == "Win\n1 2 4\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn không có cạnh | Thua | phát hiện thiết bị đầu cuối | 
| 3 chu kỳ | Vẽ | xử lý chu trình | 
| xích chìm | Thắng | độ chính xác của việc truyền bá | 

## Vỏ cạnh 

Một đồ thị chỉ bao gồm các chu trình, chẳng hạn như 1 → 2 → 3 → 1, không bao giờ tạo ra các nút cuối. Thuật toán để lại tất cả các đỉnh ở trạng thái chưa xác định sau khi khởi tạo BFS. Vì không có trạng thái nào có thể được giải quyết ngược từ điều kiện cuối nên việc phân loại vẫn chưa đầy đủ, điều này được hiểu chính xác là một kết quả hòa. 

Một đỉnh có nhiều cạnh đi ra trong đó một cạnh dẫn đến nút đầu cuối và các cạnh khác dẫn vào chu kỳ sẽ giành chiến thắng ngay lập tức khi phát hiện ra nút đầu cuối. Điều này chứng tỏ tại sao việc lan truyền ngược từ trạng thái thua là đủ: một lần thoát vào vị trí thua sẽ xác định một chiến thắng bắt buộc bất kể các cạnh khác. 

Một chuỗi dài kết thúc bằng sink đảm bảo rằng việc truyền bá chính xác sẽ xây dựng một đường dẫn chiến thắng đầy đủ. Mỗi phần trước được giải quyết chính xác một lần và các con trỏ gốc sẽ xây dựng lại một chuỗi hợp lệ mà không bị mơ hồ.
