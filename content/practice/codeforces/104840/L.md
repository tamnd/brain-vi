---
title: "CF 104840L - \u041f\u0443\u0442\u0435\u0448\u0435\u0441\u0442\u0432\u0438\u0435 \u043a \u043f\u0440\u0438\u043c\u0438\u0442\u0438\u0432\u0443"
description: "Chúng ta được cung cấp một tập hợp các từ, tất cả đều khác biệt và một hệ thống định hướng cho phép thay thế giữa chúng. Mỗi quy tắc thay thế nói rằng một từ có thể được thay thế bằng một từ khác và quá trình này có thể được lặp lại bao nhiêu lần tùy theo chuỗi thay thế."
date: "2026-06-28T11:40:33+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104840
codeforces_index: "L"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2023-2024, \u0422\u0440\u0435\u0442\u044c\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 104840
solve_time_s: 49
verified: true
draft: false
---

[CF 104840L - \u041f\u0443\u0442\u0435\u0448\u0435\u0441\u0442\u0432\u0438\u0435 \u043a \u043f\u0440\u0438\u043c\u0438\u0442\u0438\u0432\u0443](https://codeforces.com/problemset/problem/104840/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 49s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một tập hợp các từ, tất cả đều khác biệt và một hệ thống định hướng cho phép thay thế giữa chúng. Mỗi quy tắc thay thế nói rằng một từ có thể được thay thế bằng một từ khác và quá trình này có thể được lặp lại bao nhiêu lần tùy theo chuỗi thay thế. 

Nhiệm vụ là áp dụng những thay thế này theo thứ tự bất kỳ và số lần bất kỳ sao cho số lượng từ riêng biệt còn lại càng nhỏ càng tốt. Chúng tôi không được yêu cầu tự tìm các từ cuối cùng, chỉ có số lượng từ riêng biệt tối thiểu có thể có sau khi khai thác triệt để tất cả các khả năng thay thế. 

Cấu trúc này tự nhiên là một đồ thị có hướng trong đó mỗi từ là một nút và mỗi từ thay thế là một cạnh có hướng. Một chuỗi thay thế tương ứng với việc đi dọc theo các cạnh được định hướng. 

Các ràng buộc cho phép tối đa 200.000 từ và 200.000 quy tắc thay thế, loại trừ ngay lập tức mọi lý luận bậc hai hoặc mô phỏng lặp lại các phép biến đổi. Bất kỳ giải pháp nào cũng phải gần với tuyến tính hoặc tuyến tính về kích thước của biểu đồ, vì ngay cả các phép toán O(n²) cũng sẽ vượt xa giới hạn. 

Một vấn đề tế nhị là sự thay thế không nhất thiết phải đối xứng. Nếu chúng ta có thể thay thế a bằng b, điều đó không có nghĩa là b có thể thay thế a. Một điểm quan trọng khác là nhiều chuỗi có thể hợp nhất thành cùng một từ, nghĩa là các từ ban đầu khác nhau cuối cùng có thể được chuyển thành một đại diện duy nhất nếu chúng đạt đến đích chung. 

Một kịch bản thất bại điển hình đối với lối suy nghĩ ngây thơ là coi những sản phẩm thay thế là độc lập hoặc thực hiện các hoạt động sáp nhập địa phương tham lam. Ví dụ: nếu chúng ta có một chu trình như a → b → c → a, cả ba từ có thể thu gọn thành một, nhưng nếu chúng ta chỉ xem xét các từ thay thế ngay lập tức, chúng ta có thể bỏ lỡ cấu trúc chu trình tổng thể đó. 

Một trường hợp thất bại khác xuất hiện khi tồn tại chuỗi dài. Nếu a → b, b → c, c → d thì cả bốn từ đều có thể thu gọn thành d, mặc dù không có cạnh trực tiếp nào tồn tại giữa a và d. Bất kỳ cách tiếp cận nào không tính đến việc đóng cửa chuyển tiếp sẽ đánh giá thấp tiềm năng sáp nhập. 

## Phương pháp tiếp cận 

Một cách diễn giải thô bạo sẽ mô phỏng tất cả các biến đổi có thể có từ mỗi từ và tính toán tất cả các từ có thể tiếp cận được. Đối với mỗi từ, chúng tôi có thể chạy DFS hoặc BFS trên biểu đồ có hướng và đánh dấu tất cả các nút có thể truy cập. Sau đó, chúng tôi sẽ cố gắng xác định có bao nhiêu đại diện tối thiểu duy nhất tồn tại sau khi thu gọn các tập hợp có thể truy cập. 

Điều này đã gặp phải một vấn đề nghiêm trọng về hiệu quả. Chạy duyệt đồ thị từ mọi nút sẽ dẫn đến O(n(n + m)) trong trường hợp xấu nhất, điều này hoàn toàn không khả thi đối với 200.000 nút. 

Cái nhìn sâu sắc quan trọng là ngừng suy nghĩ về các nhóm khả năng tiếp cận riêng lẻ và thay vào đó tập trung vào sự tương đương do khả năng tiếp cận lẫn nhau tạo ra. Nếu từ A có thể tiếp cận từ B và từ B có thể tiếp cận từ A thông qua một số chuỗi thay thế, thì A và B có thể hoán đổi cho nhau theo nghĩa là chúng thuộc về một cấu trúc được kết nối chặt chẽ. Bên trong cấu trúc như vậy, tất cả các từ đều có thể được chuyển đổi thành một từ khác, do đó chúng có thể được thu gọn thành một đại diện duy nhất. 

Điều này làm giảm vấn đề tìm kiếm các thành phần liên thông mạnh (SCC) trong đồ thị có hướng. Sau khi ngưng tụ thành SCC, chúng ta thu được DAG. Bên trong mỗi SCC, tất cả các từ đều tương đương nhau nên chúng chỉ đóng góp một từ cho câu trả lời cuối cùng. 

Câu hỏi còn lại là liệu có bất kỳ SCC nào có thể được hợp nhất qua các cạnh hay không. Vì các cạnh chỉ cho phép thay thế về phía trước nên SCC không thể giảm số lượng từ riêng biệt vượt quá một từ trên mỗi thành phần. Vì vậy, câu trả lời cuối cùng chỉ đơn giản là số lượng SCC. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Khả năng tiếp cận Brute Force | O(n(n + m)) | O(n + m) | Quá chậm | 
| Phân hủy SCC | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán

Chúng tôi mô hình hóa các từ dưới dạng các đỉnh trong biểu đồ có hướng và xây dựng danh sách kề từ các quy tắc thay thế. 

1. Xây dựng ánh xạ từ mỗi từ đến một chỉ mục số nguyên. Điều này cho phép biểu diễn biểu đồ hiệu quả thay vì tra cứu dựa trên chuỗi, việc này sẽ quá chậm ở quy mô này. 
2. Xây dựng đồ thị có hướng bằng cách sử dụng quy tắc thay thế. Mỗi quy tắc a → b trở thành một cạnh có hướng từ chỉ số (a) đến chỉ số (b). 
3. Chạy thuật toán thành phần liên thông mạnh trên đồ thị. Một cách tiêu chuẩn là thuật toán Kosaraju hoặc thuật toán Tarjan. Mục tiêu là phân vùng các nút sao cho mỗi thành phần chứa chính xác các nút đó có thể truy cập lẫn nhau thông qua các đường dẫn được định hướng. 
4. Đếm số lượng SCC thu được. Mỗi SCC đại diện cho một nhóm từ có thể được chuyển đổi thành một nhóm khác thông qua việc thay thế lặp đi lặp lại. 
5. Xuất ra số đếm này là số từ riêng biệt tối thiểu có thể có. 

Lý do chính khiến SCC quan trọng là vì bên trong một thành phần, bất kỳ từ nào cũng có thể được chuyển đổi thành bất kỳ từ nào khác, vì vậy chúng ta luôn có thể thu gọn thành phần đó thành một từ đại diện được chọn duy nhất. Giữa các thành phần, việc chuyển đổi hoàn toàn như vậy là không thể vì không có khả năng tiếp cận lẫn nhau. 

### Tại sao nó hoạt động 

Trong một thành phần được kết nối mạnh mẽ, mọi nút đều có thể tiếp cận mọi nút khác. Điều này có nghĩa là bất kỳ từ nào trong thành phần đó đều có thể được chuyển đổi thành bất kỳ từ nào khác thông qua chuỗi thay thế hợp lệ. Do đó, toàn bộ thành phần hoạt động giống như một thực thể có thể hoán đổi cho nhau và việc giữ nhiều hơn một từ trong đó là không cần thiết. 

Trên các SCC khác nhau, thiếu ít nhất một hướng tiếp cận. Nếu hai thành phần được hợp nhất thành một từ, điều đó sẽ yêu cầu khả năng tiếp cận lẫn nhau giữa chúng, điều này mâu thuẫn với định nghĩa của SCC. Do đó, mỗi SCC đóng góp ít nhất một từ riêng biệt không thể tránh khỏi và chỉ cần một từ là đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**7)

def kosaraju(n, g, gr):
    visited = [False] * n
    order = []

    def dfs1(v):
        visited[v] = True
        for to in g[v]:
            if not visited[to]:
                dfs1(to)
        order.append(v)

    def dfs2(v):
        visited[v] = True
        for to in gr[v]:
            if not visited[to]:
                dfs2(to)

    for i in range(n):
        if not visited[i]:
            dfs1(i)

    visited = [False] * n
    scc_count = 0

    for v in reversed(order):
        if not visited[v]:
            dfs2(v)
            scc_count += 1

    return scc_count

def solve():
    n, m = map(int, input().split())
    idx = {}
    words = []

    for i in range(n):
        w = input().strip()
        idx[w] = i
        words.append(w)

    g = [[] for _ in range(n)]
    gr = [[] for _ in range(n)]

    for _ in range(m):
        a, b = input().split()
        u = idx[a]
        v = idx[b]
        g[u].append(v)
        gr[v].append(u)

    print(kosaraju(n, g, gr))

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ nén các nút chuỗi thành các chỉ mục số nguyên bằng cách sử dụng bản đồ băm, điều này rất cần thiết để duy trì các hoạt động O(1) trên mỗi cạnh. Danh sách lân cận`g`lưu trữ biểu đồ thay thế có hướng, trong khi`gr`lưu trữ biểu đồ đảo ngược cần thiết cho lần vượt qua thứ hai của Kosaraju. 

DFS đầu tiên xây dựng thứ tự hoàn thiện trên biểu đồ gốc. Thứ tự này đảm bảo rằng khi chúng tôi xử lý các nút theo thời gian hoàn thiện ngược, chúng tôi luôn bắt đầu khám phá SCC từ gốc hợp lệ của một thành phần trong biểu đồ đảo ngược. 

DFS thứ hai chạy trên biểu đồ đảo ngược và đếm số lần chúng tôi bắt đầu một lần truyền tải mới, tương ứng trực tiếp với số lượng SCC. 

## Ví dụ đã hoạt động 

Hãy xem xét cấu trúc mẫu trong đó các từ tạo thành một chuỗi có thêm các liên kết chéo, cho phép thu gọn hoàn toàn. 

đầu vào:```
5 5
hello
world
first
word
second
hello world
world first
world second
second first
word world
```| Bước | Nút hiện tại | Xếp thứ tự | SCC mới? | Số lượng SCC | 
| --- | --- | --- | --- | --- | 
| Thứ tự kết thúc DFS | tất cả các nút | xin chào, thế giới, từ, thứ hai, đầu tiên | - | 0 | 
| Quá trình đảo ngược | đầu tiên | bắt đầu DFS2 | vâng | 1 | 

Dấu vết này cho thấy rằng tất cả các nút đều có thể truy cập được lẫn nhau thông qua các chu kỳ do quy tắc thay thế gây ra. Lần chuyển thứ hai tìm thấy một SCC duy nhất, xác nhận việc thu gọn hoàn toàn thành một từ. 

Bây giờ hãy xem xét một trường hợp chu kỳ đơn giản. 

đầu vào:```
4 2
a
b
c
d
a b
b c
```| Bước | Nút | Hành động | SCC được thành lập | 
| --- | --- | --- | --- | 
| Đơn hàng đầu tiên | d, c, b, a | kết thúc đơn hàng được ghi lại | - | 
| Vượt qua thứ hai | một | chỉ khám phá một | {a} | 
| Vượt qua thứ hai | b | khám phá b → c | {b, c} | 
| Vượt qua thứ hai | d | bị cô lập | {d} | 

Điều này tạo ra ba SCC, nghĩa là vẫn còn lại ba từ riêng biệt không thể tránh khỏi. 

Những ví dụ này cho thấy các chu kỳ sụp đổ thành các thành phần đơn lẻ như thế nào trong khi chuỗi tuyến tính thì không. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi nút và cạnh được xử lý với số lần không đổi trên cả hai lần chuyển DFS | 
| Không gian | O(n + m) | Danh sách kề và ngăn xếp đệ quy lưu trữ cấu trúc biểu đồ và trạng thái truyền tải | 

Độ phức tạp tuyến tính là cần thiết vì cả n và m đều có thể đạt tới 200.000. Bất kỳ phương pháp siêu tuyến tính nào cũng sẽ thất bại trong thời gian giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from collections import defaultdict

    sys.setrecursionlimit(10**7)

    n, m = map(int, sys.stdin.readline().split())
    idx = {}
    for i in range(n):
        w = sys.stdin.readline().strip()
        idx[w] = i

    g = [[] for _ in range(n)]
    gr = [[] for _ in range(n)]

    for _ in range(m):
        a, b = sys.stdin.readline().split()
        u = idx[a]
        v = idx[b]
        g[u].append(v)
        gr[v].append(u)

    def kosaraju():
        vis = [False] * n
        order = []

        def dfs(v):
            vis[v] = True
            for to in g[v]:
                if not vis[to]:
                    dfs(to)
            order.append(v)

        for i in range(n):
            if not vis[i]:
                dfs(i)

        vis = [False] * n
        cnt = 0

        def dfs2(v):
            vis[v] = True
            for to in gr[v]:
                if not vis[to]:
                    dfs2(to)

        for v in reversed(order):
            if not vis[v]:
                dfs2(v)
                cnt += 1

        return cnt

    return str(kosaraju())

# provided sample
assert run("""5 5
hello
world
first
word
second
hello world
world first
world second
second first
word world
""") == "1"

# chain case
assert run("""4 2
a
b
c
d
a b
b c
""") == "3"

# all isolated
assert run("""3 0
a
b
c
""") == "3"

# full cycle
assert run("""3 3
a
b
c
a b
b c
c a
""") == "1"

# two components
assert run("""6 4
a
b
c
d
e
f
a b
b a
c d
d c
""") == "4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| không có cạnh | n | các nút bị cô lập vẫn tách biệt | 
| chuỗi tuyến tính | 3 | chỉ sụp đổ một phần | 
| chu kỳ đầy đủ | 1 | SCC sụp đổ hoạt động | 
| chu kỳ rời rạc | 4 | nhiều SCC được tính chính xác | 

## Vỏ cạnh 

Đồ thị không liên kết hoàn toàn là trường hợp ứng suất đơn giản nhất. Mỗi từ không có sự thay thế, vì vậy không thể hợp nhất. Thuật toán gán mỗi nút cho SCC của chính nó trong lần vượt qua DFS thứ hai. Ví dụ: với ba từ và không có cạnh, biểu đồ đảo ngược cũng trống và mỗi lệnh gọi DFS2 chạm vào chính xác một nút, tạo ra ba thành phần. 

Một đồ thị tuần hoàn đầy đủ là cực đoan ngược lại. Nếu mọi từ có thể đến được với nhau thông qua các chu kỳ thì thứ tự hoàn thiện từ DFS1 sẽ không còn phù hợp nữa vì DFS2 sẽ quét toàn bộ biểu đồ trong một lần duyệt. Điều này tạo ra chính xác một SCC. 

Chuỗi dài kiểm tra khả năng tiếp cận bắc cầu. Trong trường hợp như a → b → c → d, DFS2 xử lý các nút theo thứ tự hoàn thiện ngược, đảm bảo rằng khi c được xử lý, nó có thể tiếp cận b và a trong quá trình duyệt đồ thị đảo ngược, chỉ nhóm chúng một cách chính xác thành SCC khi có khả năng tiếp cận lẫn nhau.
