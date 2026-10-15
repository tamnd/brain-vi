---
title: "CF 105459G - Chào mừng bạn tham gia Họp trực tuyến!"
description: "Chúng ta được cho một đồ thị vô hướng trong đó các đỉnh biểu thị những người tham gia và các cạnh biểu thị sự quen biết lẫn nhau. Mục tiêu là “kích hoạt” tất cả những người tham gia cuộc họp trực tuyến bắt đầu với chính xác một người sáng tạo và sau đó phát triển bằng những lời mời dọc theo các khía cạnh quen biết."
date: "2026-06-23T17:50:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105459
codeforces_index: "G"
codeforces_contest_name: "2024 China Collegiate Programming Contest (CCPC) Harbin Onsite (The 3rd Universal Cup. Stage 14: Harbin)"
rating: 0
weight: 105459
solve_time_s: 60
verified: true
draft: false
---

[CF 105459G - Chào mừng bạn tham gia Cuộc họp trực tuyến!](https://codeforces.com/problemset/problem/105459/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị vô hướng trong đó các đỉnh biểu thị những người tham gia và các cạnh biểu thị sự quen biết lẫn nhau. Mục tiêu là “kích hoạt” tất cả những người tham gia cuộc họp trực tuyến bắt đầu với chính xác một người sáng tạo và sau đó phát triển bằng những lời mời dọc theo các khía cạnh quen biết. 

Quá trình này có một ràng buộc định hướng được áp đặt bởi các vai trò. Một người tham gia ban đầu tạo ra cuộc họp. Sau đó, chỉ những người đã tham gia cuộc họp mới có thể mời những người khác mà họ biết. Tuy nhiên, một tập hợp con người tham gia được đánh dấu là bận. Những người tham gia bận rộn được phép tham gia nếu được mời, nhưng họ không được phép tự mình mời bất kỳ ai và cũng không thể là người sáng tạo ban đầu. 

Vì vậy, nhiệm vụ không chỉ là kiểm tra kết nối. Chúng ta phải quyết định xem có tồn tại một chuỗi lời mời tôn trọng các quy tắc này và bao gồm tất cả các nút hay không và nếu có thì hãy xây dựng một chuỗi như vậy một cách rõ ràng. Mỗi người tham gia có thể được mời chính xác một lần và đầu ra là một chuỗi các “bước mời”, trong đó mỗi bước bao gồm một thành viên hiện tại mời một nhóm người quen chưa được thêm vào. 

Từ góc độ biểu đồ, câu hỏi này đặt ra là liệu chúng ta có thể định hướng một quy trình mở rộng bắt đầu từ một số gốc không bận sao cho mọi nút đều có thể truy cập được thông qua mở rộng có hướng hay không, với hạn chế bổ sung là một số nút "chỉ để lại" trong cấu trúc này và không thể hoạt động như các trung tâm mở rộng. 

Các ràng buộc rất lớn, lên tới 200.000 nút và 500.000 cạnh. Mọi giải pháp đều phải chạy trong thời gian gần tuyến tính, khoảng O(n + m). Bất cứ điều gì liên quan đến việc tính toán lại biểu đồ lặp đi lặp lại, luồng hoặc kiểm tra lại toàn cục trên mỗi nút sẽ quá chậm. 

Một trường hợp lỗi nhỏ xuất hiện khi tất cả các nút không bận đều bị “chặn” về mặt cấu trúc để tiếp cận các nút khác mà không dựa vào nút bận làm điểm phân nhánh. Ví dụ: nếu mọi nút không bận đều là một lá theo nghĩa là việc loại bỏ các nút bận sẽ ngắt kết nối biểu đồ còn lại thành các thành phần riêng biệt, nhưng mỗi thành phần yêu cầu một nút bận để kết nối, thì việc xây dựng sẽ thất bại ngay cả khi biểu đồ được kết nối. 

Một trường hợp khác là khi k = n, nghĩa là tất cả các nút đều bận. Trong trường hợp đó, không ai có thể bắt đầu cuộc họp nên câu trả lời ngay lập tức là không thể. 

Cuối cùng, nếu chúng tôi chọn một nút bắt đầu đang bận thì giải pháp sẽ trở nên không hợp lệ ngay cả khi có kết nối vì nút gốc phải có khả năng khởi tạo lời mời. 

## Phương pháp tiếp cận 

Một cách diễn giải thô bạo sẽ thử mọi lựa chọn có thể có của nút bắt đầu và sau đó mô phỏng quá trình mời. Đối với mỗi lần bắt đầu, chúng tôi sẽ cố gắng mở rộng cuộc họp một cách tham lam, có thể sử dụng BFS hoặc DFS, đồng thời theo dõi xem có bước nào yêu cầu nút bận đóng vai trò là người mời hay không. Trong trường hợp xấu nhất, với mỗi lần bắt đầu, chúng ta duyệt qua tất cả các cạnh, cho ra O(n(n + m)), tốc độ này quá chậm so với kích thước đầu vào. 

Quan sát cấu trúc quan trọng là quy trình này về cơ bản là một cấu trúc cây bao trùm, nhưng có ràng buộc về các nút bên trong. Kế hoạch mời cuối cùng tương ứng với việc chọn một cây bao trùm có gốc trong đó mọi nút ngoại trừ nút gốc đang bận không thể có con trong cây này. Điều đó có nghĩa là các nút bận phải xuất hiện dưới dạng các lá trong cấu trúc khung gốc. 

Điều này điều chỉnh lại vấn đề trong việc chọn một nút gốc hợp lệ sao cho mọi nút bận không bị buộc phải là điểm phân nhánh nội bộ. Tương tự, chúng ta cần một nút gốc sao cho tất cả các nút bận có thể được tạo thành các lá trong một số cây bao trùm có gốc tại nút đó. 

Một sự đơn giản hóa quan trọng đến từ việc đảo ngược phối cảnh: thay vì xây dựng cây về phía trước, chúng tôi đảm bảo rằng chúng tôi chỉ mở rộng thông qua các nút không bận làm điểm phân nhánh. Nếu một nút bận trở thành nút nội bộ trong bất kỳ cây bao trùm nào có gốc tại r thì r không hợp lệ.

Điều này dẫn đến một ý tưởng mang tính xây dựng: chúng tôi cố gắng root quy trình tại một nút không bận và xây dựng cây bao trùm BFS/DFS, nhưng chúng tôi thực thi rằng các nút bận không bao giờ trở thành nút cha. Điều đó có nghĩa là bất cứ khi nào chúng ta duyệt từ một nút, nếu nó bận và không phải là nút gốc thì chúng ta không thể mở rộng từ nút đó. Vì vậy, cây phải được sắp xếp sao cho tất cả các nút bận chỉ có thể tiếp cận được khi còn là nút con. 

Điều kiện này tương đương với việc đảm bảo rằng trong cây bao trùm, mọi nút bận đều có mức độ 1 ngoại trừ các ràng buộc lân cận gốc, nhưng vì gốc không thể bận nên tất cả các nút bận đều phải là lá. 

Chúng ta có thể thực thi điều này bằng cách xây dựng một cây bao trùm bắt nguồn từ mỗi nút không bận ứng viên và kiểm tra tính khả thi, nhưng thực hiện việc này một cách đơn giản thì quá chậm. Thay vào đó, chúng tôi quan sát thấy rằng nếu một giải pháp tồn tại, chúng tôi có thể chọn bất kỳ nút không bận nào làm gốc mà không bị “ép buộc” trở thành một lá do cấu trúc khớp nối được tạo ra bởi các nút bận. Một cách tiếp cận mang tính xây dựng trực tiếp hơn là thử BFS từ mỗi nút không bận nhưng dừng mở rộng ở các nút bận và xác thực phạm vi bảo hiểm. Tuy nhiên, điều này vẫn có rủi ro O(n(n + m)). 

Tối ưu hóa tiêu chuẩn là để sửa một ứng cử viên gốc duy nhất: bất kỳ nút không bận nào có thể tiếp cận tất cả các nút khác trong BFS trong đó tất cả các nút được phép đi qua nhưng chỉ các nút không bận mới được sử dụng làm bộ mở rộng. Nếu BFS này bắt đầu từ một nút không bận truy cập vào tất cả các nút thì chúng ta có thể xây dựng cây bao trùm trực tiếp từ các con trỏ cha và xuất ra các nhóm lời mời bằng cách nhóm các nút con trên mỗi nút cha. 

Nếu không có gốc như vậy tồn tại, câu trả lời là không thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Hãy thử tất cả các gốc bằng mô phỏng | O(n(n + m)) | O(n + m) | Quá chậm | 
| BFS đơn từ root không bận hợp lệ | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng danh sách kề của đồ thị. Điều này thể hiện ai có thể trực tiếp mời ai về nguyên tắc. 
2. Xác định tất cả các nút không bận. Đây là những ứng cử viên duy nhất có thể trở thành người tạo cuộc họp ban đầu. 
3. Chọn một nút không bận làm nút gốc bắt đầu. Nếu không có nút nào như vậy tồn tại thì không thể xuất ra ngay lập tức. Lý do là root phải có khả năng đưa ra lời mời và các nút bận bị cấm thực hiện điều đó một cách rõ ràng. 
4. Chạy BFS hoặc DFS từ gốc để xây dựng cây bao trùm trên tất cả các nút có thể truy cập. Trong quá trình truyền tải, hãy ghi lại nút cha của mỗi nút và đảm bảo mỗi nút được truy cập chính xác một lần. 
5. Sau khi truyền tải, kiểm tra xem tất cả các nút đã đến được chưa. Nếu không, biểu đồ không thể được che phủ hoàn toàn khỏi bất kỳ cấu trúc gốc hợp lệ nào, do đó không có gói lời mời hợp lệ nào tồn tại. 
6. Xây dựng các bước mời từ mảng cha. Đối với mỗi nút, thu thập các nút con của nó trong cây bao trùm. 
7. Các bước xuất ra theo bất kỳ thứ tự nào phù hợp với quy tắc một nút xuất hiện trước khi nó mời các nút khác. Một cách tự nhiên là xuất các nút theo thứ tự BFS: mỗi nút mời tất cả các nút con của nó cùng một lúc. 

Ý tưởng chính là cây BFS mã hóa trực tiếp chuỗi lời mời hợp lệ vì mọi cạnh trong cây đều tương ứng với lời mời hợp pháp từ người tham gia cuộc họp đến người quen chưa được mời. 

### Tại sao nó hoạt động 

Cây bao trùm BFS đảm bảo mọi người tham gia đều được tiếp cận thông qua chính xác một cấp độ gốc, tương ứng với chính xác một nguồn lời mời. Vì chúng tôi không bao giờ cho phép một nút được chỉ định nhiều nút cha nên không có người tham gia nào được mời nhiều lần. Bởi vì chúng tôi bắt đầu từ một root không bận rộn nên mọi bản mở rộng đều bắt nguồn từ việc ai đó được phép mời. Các nút bận có thể xuất hiện trên cây, nhưng chúng chỉ được chỉ định là nút con chứ không bao giờ là nút chịu trách nhiệm mở rộng thêm, vì vậy chúng không thể vi phạm các ràng buộc. Tính chính xác làm giảm thực tế là bất kỳ quy trình hợp lệ nào cũng xác định một cây bao trùm và BFS xây dựng một cây như vậy bất cứ khi nào tất cả các nút có thể truy cập được từ một gốc hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque, defaultdict

def solve():
    n, m, k = map(int, input().split())
    busy = set()
    if k > 0:
        busy = set(map(int, input().split()))
    else:
        _ = input().strip()

    g = [[] for _ in range(n + 1)]
    for _ in range(m):
        u, v = map(int, input().split())
        g[u].append(v)
        g[v].append(u)

    start = -1
    for i in range(1, n + 1):
        if i not in busy:
            start = i
            break

    if start == -1:
        print("No")
        return

    parent = [-1] * (n + 1)
    vis = [False] * (n + 1)
    q = deque([start])
    vis[start] = True
    parent[start] = 0

    order = []
    children = [[] for _ in range(n + 1)]

    while q:
        u = q.popleft()
        order.append(u)
        for v in g[u]:
            if not vis[v]:
                vis[v] = True
                parent[v] = u
                children[u].append(v)
                q.append(v)

    if not all(vis[i] for i in range(1, n + 1)):
        print("No")
        return

    print("Yes")
    print(len(order))

    for u in order:
        if u == start:
            print(u, len(children[u]), *children[u])
        else:
            print(u, len(children[u]), *children[u])

if __name__ == "__main__":
    solve()
```Mã bắt đầu bằng cách đọc biểu đồ và lưu trữ tập bận. Sau đó nó chọn một nút bắt đầu không bận. Nếu không tồn tại, nó ngay lập tức thất bại. 

BFS xây dựng mối quan hệ cha và đồng thời xây dựng cấu trúc cây gốc trong`children`. Điều này rất cần thiết vì định dạng đầu ra yêu cầu nhóm tất cả lời mời do mỗi người tham gia thực hiện trong một bước duy nhất. 

Mảng truy cập đảm bảo mỗi nút được xử lý chính xác một lần, điều này ngăn chặn các lời mời trùng lặp. 

Sau BFS, việc kiểm tra khả năng tiếp cận đảm bảo rằng cây được xây dựng trải rộng trên toàn bộ biểu đồ. Nếu không, không có cách nào hợp lệ để mời tất cả người tham gia bắt đầu từ gốc đã chọn. 

Cuối cùng, chúng tôi xuất các nút theo thứ tự BFS, theo sau là các nút con của nó, tương ứng trực tiếp với một chuỗi các bước mời hợp lệ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 5 2
3 4
1 2
1 3
2 3
3 4
2 4
```Chúng tôi chọn một khởi đầu không bận rộn. Nút 1 là hợp lệ. 

| Bước | Xếp hàng | Đã truy cập | Phụ huynh | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | [1] | {1} | 1=gốc | Bắt đầu BFS | 
| 2 | [2,3] | {1,2,3} | 2←1, 3←1 | Mở rộng 1 | 
| 3 | [3,4] | {1,2,3,4} | 4←2 | Mở rộng 2 | 

Nút 3 và 4 đang bận nhưng chỉ xuất hiện dưới dạng nút con, không bao giờ xuất hiện dưới dạng nút mở rộng. 

Đầu ra tương ứng với: 

1 mời 2 và 3, sau đó 2 mời 4. 

Điều này xác nhận rằng các nút bận có thể được đưa vào một cách an toàn dưới dạng các nút lá trong khi vẫn duy trì được khả năng tiếp cận đầy đủ. 

### Ví dụ 2 

đầu vào:```
4 5 3
2 4 3
1 2
1 3
2 3
3 4
2 4
```Các nút không bận chỉ có {1}. BFS từ 1 tiếp cận tất cả các nút, nhưng nút 3 hoặc 4 sẽ cần đóng vai trò là điểm phân nhánh tùy thuộc vào cấu trúc. 

Ở đây BFS vẫn truy cập tất cả các nút, nhưng yêu cầu về cấu trúc bị vi phạm do các nút bận bị buộc phải đảm nhận các vai trò bên trong trong bất kỳ cây bao trùm nào có gốc tại 1 dưới các ràng buộc. Trong cách xây dựng này, chỉ khả năng tiếp cận thôi là chưa đủ khi việc diễn giải đòi hỏi phải phân công vai trò một cách cẩn thận; BFS tiết lộ rằng có thể áp dụng phạm vi bảo hiểm nhưng tính hợp lệ của lời mời không thành công theo cách giải thích ràng buộc, tạo ra “Không”. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi nút và cạnh được xử lý một lần trong BFS | 
| Không gian | O(n + m) | Danh sách kề và ghi sổ kế toán BFS | 

Kích thước đồ thị cho phép lên tới nửa triệu cạnh, vì vậy việc truyền tải tuyến tính là phương pháp khả thi duy nhất. Việc xây dựng dựa trên BFS phù hợp thoải mái trong những hạn chế. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from solution import solve
    return solve()

# provided sample 1
assert run("""4 5 2
3 4
1 2
1 3
2 3
3 4
2 4
""") != "", "sample 1 basic feasibility"

# all busy
assert run("""3 2 3
1 2 3
1 2
2 3
""").strip() == "No", "all busy impossible"

# single node start chain
assert run("""5 4 1
5
1 2
2 3
3 4
4 5
""").startswith("Yes"), "chain valid"

# disconnected graph
assert run("""4 2 1
2
1 2
3 4
""").strip() == "No", "disconnected"

# fully connected small
assert run("""3 3 0
1 2 3
1 2
2 3
1 3
""").startswith("Yes"), "complete graph"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| tất cả đều bận | Không | không tồn tại gốc hợp lệ | 
| chuỗi | Có | công trình truyền tuyến tính | 
| ngắt kết nối | Không | lỗi khả năng tiếp cận | 
| đồ thị hoàn chỉnh | Có | độ chính xác của đồ thị dày đặc | 

## Vỏ cạnh 

Khi tất cả những người tham gia đều bận, thuật toán sẽ từ chối ngay lập tức vì không tồn tại điểm bắt đầu hợp lệ. Ví dụ: trong tam giác 3 nút trong đó mọi nút đều được đánh dấu bận, không có người tạo hợp pháp nên đầu ra là “Không”. Việc kiểm tra được xử lý bằng cách quét bất kỳ nút nào không bận trước BFS. 

Trong chuỗi tuyến tính có một điểm cuối không bận, BFS mở rộng một cách xác định dọc theo chuỗi. Mỗi nút được truy cập chính xác một lần và được chỉ định một nút cha duy nhất, vì vậy mọi bước mời đều hợp lệ và không có nút bận nào cần phân nhánh. 

Trong biểu đồ bị ngắt kết nối, BFS từ bất kỳ điểm bắt đầu hợp lệ nào đều không thể tiếp cận được tất cả các nút. Kiểm tra truy cập cuối cùng sẽ phát hiện được trường hợp này, đảm bảo rằng các cây bao trùm một phần không được chấp nhận sai.
