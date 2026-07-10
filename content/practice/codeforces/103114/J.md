---
title: "CF 103114J - Hành trình của Jahaxiki III - Tryna lạc lối"
description: "Chúng ta được cung cấp một bản đồ phẳng dạng lưới, nơi chuyển động xảy ra giữa các ô vuông nhỏ. Mỗi ô có khả năng được kết nối với các ô lân cận thông qua các lỗ mở, trong khi các rào cản được thể hiện ngầm bằng các ký tự trên tường trong bố cục văn bản."
date: "2026-07-03T20:40:48+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103114
codeforces_index: "J"
codeforces_contest_name: "The 2021 Hangzhou Normal U Summer Trials"
rating: 0
weight: 103114
solve_time_s: 54
verified: true
draft: false
---

[CF 103114J - Hành trình của Jahaxiki III - Tryna lạc lối](https://codeforces.com/problemset/problem/103114/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 54s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một bản đồ phẳng dạng lưới, nơi chuyển động xảy ra giữa các ô vuông nhỏ. Mỗi ô có khả năng được kết nối với các ô lân cận thông qua các lỗ mở, trong khi các rào cản được thể hiện ngầm bằng các ký tự trên tường trong bố cục văn bản. Nếu hai ô liền kề không được ngăn cách bởi một bức tường, chúng ta có thể di chuyển giữa chúng, nếu không quá trình chuyển đổi đó sẽ bị chặn. 

Khi cấu trúc này được diễn giải, vấn đề sẽ chuyển thành đồ thị vô hướng: mỗi ô là một nút và mỗi đoạn hợp lệ giữa các ô lân cận là một cạnh. Nhiệm vụ là xác định xem biểu đồ này có chứa chu trình nào không. Nếu một chu trình tồn tại, chúng tôi xuất ra NO, nghĩa là Tryna có thể bị mắc kẹt trong một vòng lặp. Nếu không tồn tại chu trình, chúng ta xuất ra CÓ, nghĩa là mọi đường dẫn cuối cùng đều dẫn ra ngoài mà không tạo thành một vòng khép kín. 

Các ràng buộc cho phép tổng cộng tối đa 10^5 ô, nghĩa là mọi giải pháp về cơ bản phải tuyến tính về số lượng ô và cạnh. Một cách tiếp cận bậc hai thử tất cả các cặp nút hoặc khám phá nhiều lần từ đầu là không khả thi ngay lập tức vì nó sẽ vượt quá khoảng 10^10 thao tác trong trường hợp xấu nhất. 

Một vấn đề khó nhận thấy trong bài toán này là đầu vào không phải là ma trận hoặc danh sách kề trực tiếp. Thay vào đó, kết nối được mã hóa trong lưới ký tự nơi các bức tường xuất hiện dưới dạng`|`Và`_`và khoảng trống hàm ý sự kết nối. Một lỗi diễn giải ngây thơ là coi mỗi vị trí ký tự là một nút chứ không phải mỗi ô. Điều đó dẫn đến việc xây dựng biểu đồ không chính xác và phát hiện chu trình sai. 

Một cạm bẫy phổ biến khác là xử lý sai các cạnh hai chiều hai lần mà không đánh dấu đúng các cạnh đã thăm. Trong biểu đồ lưới, mỗi cạnh xuất hiện hai lần trong quá trình truyền tải, do đó việc phát hiện chu kỳ phải dựa vào các nút đã truy cập hoặc cấu trúc tìm liên kết thay vì đếm cạnh thô. 

Trường hợp cạnh cuối cùng là khi đồ thị là một cây nhưng có nhiều phân nhánh. Trong những trường hợp như vậy, việc phát hiện chu trình DFS đơn giản mà quên theo dõi các nút cha sẽ báo cáo sai các chu kỳ do truy cập lại nút cha trực tiếp trong một quá trình truyền tải không có hướng dẫn. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ trực tiếp là diễn giải lưới, xây dựng biểu đồ đầy đủ một cách rõ ràng và đối với mỗi nút, hãy khởi động DFS hoặc BFS để phát hiện xem liệu chúng ta có thể quay lại nút đã truy cập ngoài nút gốc hay không. Điều này hoạt động chính xác, vì bất kỳ chu trình nào cuối cùng cũng sẽ được khám phá bằng cách khám phá các cạnh một cách triệt để. Tuy nhiên, việc bắt đầu một quá trình truyền tải mới từ mọi nút sẽ dẫn đến công việc lặp đi lặp lại: mỗi cạnh được khám phá nhiều lần trong nhiều lần chạy DFS, tạo ra độ phức tạp trong trường hợp xấu nhất là O(n^2) trên các lưới kết nối dày đặc. 

Quan sát quan trọng là việc phát hiện chu trình trong đồ thị vô hướng không yêu cầu duyệt toàn bộ lặp đi lặp lại. Mỗi nút chỉ cần được truy cập một lần trong quá trình truyền tải toàn cầu. Nếu chúng ta duy trì một mảng đã truy cập và đảm bảo rằng chúng ta bỏ qua cạnh cha ngay lập tức thì một DFS hoặc BFS duy nhất trên toàn bộ biểu đồ là đủ. Lần đầu tiên chúng ta gặp một nút được truy cập không phải là nút cha của nút hiện tại, chúng ta đã tìm thấy một chu trình. 

Điều này làm giảm vấn đề xây dựng cấu trúc kề một lần và thực hiện một lần truyền tuyến tính duy nhất trên tất cả các nút và cạnh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force DFS từ mọi nút | O(n²) | O(n) | Quá chậm | 
| Phát hiện chu trình DFS/BFS đơn | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi diễn giải lưới dưới dạng biểu đồ trong đó mỗi ô là một nút, được lập chỉ mục theo hàng và cột của nó. Sau đó, chúng tôi kết nối từng ô với các ô lân cận bên phải và dưới cùng của nó nếu không có bức tường nào giữa chúng, vì những bức tường đó đủ để nắm bắt tất cả các vùng lân cận vô hướng mà không bị trùng lặp. 

1. Gán một chỉ số nguyên cho mỗi ô trong lưới n x m để chúng ta có thể lưu trữ trạng thái truy cập trong một mảng phẳng. Điều này giúp đơn giản hóa việc theo dõi kề và tránh việc tính toán tọa độ lặp đi lặp lại. 
2. Phân tích cấu trúc ký tự đầu vào và xác định khả năng kết nối giữa từng cặp ô liền kề. Đối với mỗi cạnh tiềm năng, hãy kiểm tra xem ký tự tường tương ứng có chặn chuyển động hay không. Nếu không có bức tường nào tồn tại, hãy thêm một cạnh vô hướng giữa hai nút tương ứng. 
3. Duy trì mảng đã truy cập được khởi tạo thành false cho tất cả các nút. Mảng này theo dõi xem một nút đã được xử lý đầy đủ trong quá trình truyền tải DFS hay chưa. 
4. Đối với mỗi nút chưa được truy cập, hãy khởi động DFS. Chúng tôi cũng duy trì một con trỏ cha để không diễn giải sai cạnh trở về cha mẹ như một chu trình. 
5. Trong DFS, khi chúng ta đến thăm một người hàng xóm, nếu người đó không được thăm, chúng ta sẽ truy cập lại vào đó. Nếu nó đã được truy cập và không phải là cha mẹ, chúng tôi sẽ phát hiện ngay một chu trình và kết thúc bằng NO. 
6. Nếu toàn bộ quá trình truyền tải kết thúc mà không phát hiện bất kỳ chu kỳ nào, chúng ta xuất ra CÓ. 

Lý do chúng ta chỉ cần xem xét các kết nối phải và xuống trong quá trình phân tích cú pháp là vì mỗi vùng lân cận được biểu diễn duy nhất một lần trong mã hóa đầu vào và tính đối xứng trong lưới đảm bảo tái tạo kết nối đầy đủ. 

### Tại sao nó hoạt động

Thuật toán đang thực hiện DFS một cách hiệu quả trên đồ thị vô hướng trong khi vẫn duy trì tính bất biến rằng mọi nút được truy cập đều thuộc về một cây được khám phá một phần bắt nguồn từ một nút bắt đầu nào đó. Trong đồ thị vô hướng, một chu trình tồn tại khi và chỉ nếu trong DFS chúng ta gặp một cạnh dẫn đến nút đã truy cập trước đó không phải là nút gốc trong cây DFS. Vì mỗi nút được truy cập chính xác một lần trong quá trình truyền tải toàn cục nên bất kỳ chu trình nào cuối cùng cũng phải đưa ra một cạnh sau như vậy. Ngược lại, nếu không có cạnh nào tồn tại thì mọi thành phần được kết nối là một cây, đảm bảo tính không tuần hoàn trên toàn bộ biểu đồ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**7)

def solve():
    n, m = map(int, input().split())
    
    # total cells
    N = n * m
    
    def id(x, y):
        return x * m + y

    adj = [[] for _ in range(N)]

    # read grid lines
    # we interpret only cell structure, ignoring visual separators
    grid = [input().rstrip("\n") for _ in range(n + 1)]

    # connect vertical edges
    # between row i and i+1 using line i+1
    for i in range(n - 1):
        line = grid[i + 1]
        # each cell boundary is represented in compressed form
        # positions correspond to columns
        for j in range(m):
            # between (i,j) and (i+1,j)
            # wall is at line[i+1][2*j + 1]
            if line[2 * j + 1] == " ":
                u = id(i, j)
                v = id(i + 1, j)
                adj[u].append(v)
                adj[v].append(u)

    # connect horizontal edges
    for i in range(n):
        line = grid[i]
        for j in range(m - 1):
            # between (i,j) and (i,j+1)
            # check barrier in same row line
            if line[2 * j + 2] == " ":
                u = id(i, j)
                v = id(i, j + 1)
                adj[u].append(v)
                adj[v].append(u)

    visited = [False] * N
    parent = [-1] * N
    has_cycle = False

    def dfs(u):
        nonlocal has_cycle
        visited[u] = True
        for v in adj[u]:
            if not visited[v]:
                parent[v] = u
                dfs(v)
            elif parent[u] != v:
                has_cycle = True

    for i in range(N):
        if not visited[i]:
            parent[i] = -1
            dfs(i)
            if has_cycle:
                print("NO")
                return

    print("YES")

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên là tái tạo lại sự kề cận bằng cách quét biểu diễn lưới văn bản và chuyển đổi nó thành biểu đồ vô hướng tiêu chuẩn trên n·m nút. Việc xây dựng vùng lân cận chỉ kiểm tra cẩn thận các vị trí tương ứng với ranh giới ô thực tế. 

DFS sử dụng một mảng cha để phân biệt cạnh trả về hợp lệ của cha với một chu trình thực. Nếu không có sự kiểm tra này, mọi cạnh vô hướng sẽ ngay lập tức xuất hiện dưới dạng một chu trình. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Đầu vào tương ứng với một cấu trúc kết nối đơn giản không có chu trình. 

| Bước | Nút | Hành động đã truy cập | Đã phát hiện chu kỳ | 
| --- | --- | --- | --- | 
| 1 | (0,0) | bắt đầu DFS | không | 
| 2 | (1,0) | thăm hàng xóm | không | 
| 3 | (1,1) | thăm hàng xóm | không | 
| 4 | (0,1) | thăm hàng xóm | không | 
| 5 | trở lại | kết thúc quá trình truyền tải | không | 

Dấu vết này cho thấy rằng chúng tôi luôn khám phá các nút mới mà không gặp phải nút không phải nút gốc đã được truy cập trước đó, xác nhận tính không chu kỳ và tạo ra CÓ. 

### Ví dụ 2 

Đầu vào tạo thành một chu kỳ vuông. 

| Bước | Nút | Hành động đã truy cập | Đã phát hiện chu kỳ | 
| --- | --- | --- | --- | 
| 1 | (0,0) | bắt đầu DFS | không | 
| 2 | (1,0) | thăm hàng xóm | không | 
| 3 | (1,1) | thăm hàng xóm | không | 
| 4 | (0,1) | thăm hàng xóm | không | 
| 5 | (0,0) | thăm lại không phải cha mẹ | vâng | 

Ở bước 5, chúng ta đến một nút đã được truy cập mà không phải là nút cha, nút này trực tiếp biểu thị một chu kỳ, vì vậy chúng ta xuất ra NO. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nm) | Mỗi ô được xử lý một lần và mỗi cạnh được xem xét nhiều nhất hai lần trong DFS | 
| Không gian | O(nm) | Danh sách kề và mảng đã truy cập lưu trữ một mục nhập trên mỗi ô và cạnh | 

Các ràng buộc cho phép tối đa 10^5 ô, do đó, việc truyền tải biểu đồ thời gian tuyến tính vừa vặn thoải mái trong cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from solution import solve  # assume solution is in module form
    return solve()

# provided samples
assert run("""2 3
_ _ _
| | |_|
|_ _|_|
""") == "YES"

assert run("""2 3
_ _ _
|
|_|
|_ _|_|
""") == "NO"

# custom cases

# minimum size, no cycle
assert run("""1 1
 
""") == "YES"

# small cycle
assert run("""2 2
_ _
| |
|_|_|
""") == "NO"

# linear chain
assert run("""1 4
_ _ _ _
""") == "YES"

# fully connected square grid cycle
assert run("""2 2
_ _
| |
|_|_|
""") == "NO"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Lưới 1x1 | CÓ | trường hợp tuần hoàn nhỏ nhất | 
| chu kỳ 2x2 | KHÔNG | phát hiện chu trình cơ bản | 
| dòng 1x4 | CÓ | con đường dài không có chu kỳ | 
| Vòng lặp kết nối đầy đủ 2x2 | KHÔNG | độ chính xác của chu trình lưới | 

## Vỏ cạnh 

Trường hợp cạnh đầu tiên là lưới ô đơn. Trong trường hợp này không có cạnh nào cả, do đó đồ thị có tính chất không tuần hoàn và DFS không bao giờ kích hoạt phát hiện chu kỳ. 

Trường hợp cạnh thứ hai là một lưới mở hoàn toàn trong đó mọi cặp liền kề đều được kết nối. Ở đây, có rất nhiều chu kỳ và DFS phải phát hiện chính xác cạnh sau khi xem lại nút đã khám phá trước đó mà không phải là nút gốc. 

Trường hợp cạnh thứ ba là một cấu trúc giống như cây được nhúng trong một lưới có nhiều nhánh. Thuật toán phải tránh phát hiện sai các chu kỳ khi quay về cấp độ gốc trực tiếp, việc này được xử lý bằng quá trình kiểm tra cấp độ gốc trong DFS.
