---
title: "CF 102672A - Lâu Đài Gỗ"
description: "Chiếc khóa là một cái cây có các đỉnh được sơn hai màu. Một bước di chuyển có thể sơn lại một đỉnh hoặc loại bỏ toàn bộ vùng được kết nối có các đỉnh hiện có cùng màu. Nhiệm vụ là tìm số bước di chuyển tối thiểu cần thiết cho đến khi không còn đỉnh nào."
date: "2026-08-01T23:42:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102672
codeforces_index: "A"
codeforces_contest_name: "Selection of tasks from Internet olympiads season 2019-20"
rating: 0
weight: 102672
solve_time_s: 75
verified: true
draft: false
---

[CF 102672A - Lâu đài bằng gỗ](https://codeforces.com/problemset/problem/102672/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 15s 
**Đã xác minh:** có 

##Giải pháp 
#Hiểu vấn đề 

Chiếc khóa là một cái cây có các đỉnh được sơn hai màu. Một bước di chuyển có thể sơn lại một đỉnh hoặc loại bỏ toàn bộ vùng được kết nối có các đỉnh hiện có cùng màu. Nhiệm vụ là tìm số bước di chuyển tối thiểu cần thiết cho đến khi không còn đỉnh nào. Đầu vào cung cấp số đỉnh, màu nhị phân ban đầu và các cạnh của cây. Đầu ra là số lượng hoạt động nhỏ nhất có thể. 

Chi tiết quan trọng là cấu trúc cây không yêu cầu chúng tôi mô phỏng việc xóa. Số đỉnh có thể lên tới 200.000, do đó, bất kỳ phương pháp nào thử nhiều lệnh xóa có thể hoặc duy trì cây sau mỗi thao tác sẽ không phù hợp. Một giải pháp phải gần với thời gian tuyến tính, bởi vì ngay cả một thuật toán bậc hai cũng đã yêu cầu khoảng 40 tỷ phép tính ở kích thước lớn nhất. 

Có hai chiến lược tự nhiên. Chúng ta có thể xóa từng thành phần đơn sắc ban đầu một cách riêng biệt hoặc có thể thực hiện các thao tác sơn lại các đỉnh để nhiều thành phần hợp nhất và biến mất cùng nhau. Thách thức là nhận ra rằng không có sự kết hợp phức tạp nào giữa việc sơn lại một phần và xóa một phần có thể đánh bại được ý tưởng tốt nhất trong hai ý tưởng này. 

Xét một cái cây có bốn đỉnh có hình ngôi sao:```
4
1000
1 2
1 3
1 4
```Ban đầu có bốn thành phần đơn sắc, vì vậy việc xóa từng thành phần một sẽ mất bốn lần di chuyển. Tuy nhiên, việc sơn lại phần trung tâm sẽ làm cho toàn bộ cây trở nên trắng và một lần xóa cuối cùng sẽ loại bỏ mọi thứ, đưa ra câu trả lời là hai. Một giải pháp chỉ đếm các thành phần không thành công ở đây. 

Một trường hợp quan trọng khác là khi tất cả các đỉnh đều có cùng màu. Ví dụ:```
3
000
1 2
2 3
```Câu trả lời là một, vì toàn bộ cây đã là một thành phần duy nhất có thể tháo rời được. Một công thức luôn thêm một lần sơn lại trước khi xóa sẽ trả về hai lần không chính xác. 

Thái cực ngược lại cũng rất quan trọng. Đối với một đỉnh duy nhất:```
1
1
```Câu trả lời là một. Không cần phải sơn lại nhưng đỉnh vẫn phải được loại bỏ bằng thao tác xóa. 

# Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ cố gắng quyết định thành phần nào cần xóa trước và đỉnh nào cần sơn lại. Đối với mọi trạng thái có thể có của cây, nó có thể thử mọi thao tác có sẵn và sử dụng đệ quy với khả năng ghi nhớ. Điều này đúng vì nó khám phá mọi chuỗi có thể, nhưng số lượng trạng thái tăng theo cấp số nhân theo số đỉnh. Ngay cả những cây nhỏ cũng có thể tạo ra quá nhiều màu sắc và tập hợp đỉnh còn lại, vì vậy phương pháp này không thể sử dụng được. 

Một ý tưởng đơn giản hơn là đếm liên tục các thành phần đơn sắc hiện tại và thử mọi cách sơn lại có thể. Điều này vẫn thất bại vì sau mỗi lần sơn lại số lượng lựa chọn có thể có vẫn còn lớn. Với 200.000 đỉnh, ngay cả việc kiểm tra từng lần sơn lại một đỉnh có thể cũng đã quá tốn kém. 

Quan sát quan trọng là việc sơn lại chỉ giúp ích bằng cách giảm số lượng các nhóm màu khác nhau. Chỉ có hai màu, vì vậy nếu chúng ta quyết định chọn toàn bộ cây một màu, lựa chọn rẻ nhất là sơn lại mọi đỉnh có màu ít xuất hiện hơn và sau đó xóa cây một lần. Chi phí này:```
min(number of white vertices, number of black vertices) + 1
```Tùy chọn khác là không sơn lại gì cả. Sau đó, mọi thành phần kết nối đơn sắc ban đầu cuối cùng phải bị xóa, do đó chi phí bằng số lượng thành phần đó. 

Điều đáng ngạc nhiên là việc kết hợp cả hai chiến lược này không thể cải thiện được câu trả lời. Nếu chúng ta sơn lại ít đỉnh hơn số lượng màu thiểu số thì vẫn còn một số đỉnh của cả hai màu. Các vùng có màu khác nhau còn lại vẫn yêu cầu xóa riêng biệt và tổng số không thể đánh bại việc xóa tất cả các thành phần ban đầu. Nếu chúng ta sơn lại đủ số đỉnh để loại bỏ hoàn toàn một màu, thì ít nhất chúng ta đã phải trả chi phí để làm cho toàn bộ cây trở thành đơn sắc. 

Do đó, giải pháp được rút gọn thành việc đếm hai giá trị trong một lần duyệt: số thành phần đơn sắc và số đỉnh của mỗi màu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

#Hướng dẫn thuật toán 

1. Đếm xem mỗi màu có bao nhiêu đỉnh. Điều này đưa ra cái giá của chiến lược trong đó chúng ta xóa một màu khỏi cây, sơn lại màu kia và thực hiện lần xóa cuối cùng. 
2. Duyệt cây một lần và đếm các thành phần đơn sắc được kết nối. Bắt đầu duyệt từ mọi đỉnh chưa được thăm. Tất cả các đỉnh có thể tiếp cận có cùng màu đều thuộc về cùng một thành phần. 
3. Tính toán hai câu trả lời có thể. Đầu tiên là số lượng thành phần đơn sắc. Thứ hai là số đỉnh có màu ít phổ biến hơn cộng với một thao tác xóa cuối cùng. 
4. Xuất ra giá trị nhỏ hơn trong hai giá trị này vì mọi chuỗi tối ưu đều thuộc một trong hai trường hợp này. 

Tại sao nó hoạt động: 

Mọi thao tác xóa sẽ loại bỏ một thành phần kết nối đơn sắc của cây hiện tại. Nếu chúng ta không bao giờ loại bỏ hoàn toàn một màu thì ranh giới màu còn lại buộc chúng ta phải trả ít nhất chi phí tương đương với việc xử lý các thành phần ban đầu. Cách duy nhất để tránh những ranh giới thành phần đó là sơn lại đủ số đỉnh để toàn bộ cây trở thành một màu. Phép biến đổi rẻ nhất như vậy sẽ sơn lại tất cả các đỉnh của lớp màu nhỏ hơn. Sau đó, một thao tác xóa sẽ loại bỏ mọi thứ. Vì mọi giải pháp tối ưu đều phải giữ cả hai màu hoặc loại bỏ hoàn toàn một màu nên mức tối thiểu của hai chi phí này luôn có thể đạt được và không thể cải thiện được. 

#Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    s = input().strip()

    graph = [[] for _ in range(n)]
    for _ in range(n - 1):
        a, b = map(int, input().split())
        a -= 1
        b -= 1
        graph[a].append(b)
        graph[b].append(a)

    count0 = s.count('0')
    count1 = n - count0

    components = 0
    visited = [False] * n

    for i in range(n):
        if not visited[i]:
            components += 1
            color = s[i]
            stack = [i]
            visited[i] = True

            while stack:
                v = stack.pop()
                for u in graph[v]:
                    if not visited[u] and s[u] == color:
                        visited[u] = True
                        stack.append(u)

    repaint_strategy = min(count0, count1) + 1
    print(min(components, repaint_strategy))

if __name__ == "__main__":
    solve()
```Phần đầu tiên của mã xây dựng cây bằng danh sách kề. Một cái cây có chính xác`n - 1`các cạnh, do đó việc lưu trữ mỗi cạnh hai lần sẽ sử dụng bộ nhớ tuyến tính. 

Số lượng màu đủ để đánh giá toàn bộ chiến lược sơn lại. Chúng ta không bao giờ cần biết đỉnh cụ thể nào được sơn lại vì chỉ tổng số đỉnh của mỗi màu mới quan trọng. 

DFS đếm các thành phần đơn sắc. Khi tìm thấy một đỉnh mới chưa được thăm, nó sẽ bắt đầu duyệt chính xác một thành phần. DFS chỉ đi theo các cạnh đến các đỉnh có cùng màu, do đó, các đỉnh có màu đối diện sẽ chia cây thành các thành phần khác nhau một cách tự nhiên. 

Không có một vấn đề nào xảy ra trong số lượng thành phần vì mỗi đỉnh được truy cập chính xác một lần và mỗi lần bắt đầu truyền tải tương ứng với một thành phần. Số nguyên Python cũng xử lý số lượng tối đa mà không có bất kỳ lo ngại nào về tràn. 

# Ví dụ đã hoạt động 

## Ví dụ 1 

đầu vào:```
4
1000
1 2
1 3
1 4
```Ở giữa có màu đen và lá có màu trắng. 

| Bước | Đỉnh hiện tại | Màu thành phần | Thành phần được tính | 
| --- | --- | --- | --- | 
| 1 | 1 | 1 | 1 | 
| 2 | 2 | 0 | 2 | 
| 3 | 3 | 0 | 3 | 
| 4 | 4 | 0 | 4 | 

Chiến lược thành phần có giá 4. Có một đỉnh màu đen và ba đỉnh màu trắng, do đó việc sơn lại màu sắc nhỏ hơn sẽ có giá`1 + 1 = 2`. Câu trả lời là 2. 

## Ví dụ 2 

đầu vào:```
5
01010
1 2
2 3
3 4
4 5
```Con đường xen kẽ màu sắc. 

| Bước | Đỉnh hiện tại | Màu thành phần | Thành phần được tính | 
| --- | --- | --- | --- | 
| 1 | 1 | 0 | 1 | 
| 2 | 2 | 1 | 2 | 
| 3 | 3 | 0 | 3 | 
| 4 | 4 | 1 | 4 | 
| 5 | 5 | 0 | 5 | 

Có năm thành phần. Màu nhỏ hơn xuất hiện hai lần, do đó chiến lược sơn lại sẽ tốn kém`2 + 1 = 3`. Thuật toán chọn 3, nghĩa là sơn lại hai đỉnh màu đen và xóa cây màu trắng thu được. 

# Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi đỉnh và cạnh được xử lý một lần trong quá trình truyền tải. | 
| Không gian | O(n) | Danh sách kề, mảng đã truy cập và ngăn xếp DFS lưu trữ thông tin tuyến tính. | 

Kích thước đầu vào có thể đạt tới 200.000 đỉnh và thuật toán chỉ thực hiện một lượng công việc không đổi trên mỗi đỉnh và cạnh. Nó phù hợp thoải mái trong giới hạn cần thiết. 

# Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    data = sys.stdin.readline
    n = int(data())
    s = data().strip()

    graph = [[] for _ in range(n)]
    for _ in range(n - 1):
        a, b = map(int, data().split())
        a -= 1
        b -= 1
        graph[a].append(b)
        graph[b].append(a)

    count0 = s.count("0")
    count1 = n - count0

    visited = [False] * n
    components = 0

    for i in range(n):
        if not visited[i]:
            components += 1
            color = s[i]
            stack = [i]
            visited[i] = True
            while stack:
                v = stack.pop()
                for u in graph[v]:
                    if not visited[u] and s[u] == color:
                        visited[u] = True
                        stack.append(u)

    ans = min(components, min(count0, count1) + 1)

    sys.stdin = old_stdin
    return str(ans) + "\n"

assert run("""4
1000
1 2
1 3
1 4
""") == "2\n", "sample 1"

assert run("""1
0
""") == "1\n", "single vertex"

assert run("""3
000
1 2
2 3
""") == "1\n", "already one component"

assert run("""5
01010
1 2
2 3
3 4
4 5
""") == "3\n", "alternating path"

assert run("""6
111111
1 2
2 3
3 4
4 5
5 6
""") == "1\n", "all equal values"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`4 / 1000`cây sao | 2 | Sơn lại một màu có thể đánh bại việc xóa các thành phần. | 
| Đỉnh đơn | 1 | Điều kiện biên kích thước tối thiểu. | 
| Tất cả cây không | 1 | Đã là cây đơn sắc. | 
| Con đường thay thế | 3 | Nhiều thành phần sơn lại sẽ tốt hơn. | 
| Tất cả một màu | 1 | Xử lý giá trị bằng nhau | 

# Vỏ cạnh 

Đối với cây hình ngôi sao:```
4
1000
1 2
1 3
1 4
```Thuật toán đếm bốn thành phần đơn sắc. Nó cũng đếm một đỉnh đen và ba đỉnh trắng, tạo ra chiến lược sơn lại gồm hai thao tác. Tối thiểu là hai, phù hợp với trình tự sơn lại phần trung tâm và xóa toàn bộ cây. 

Đối với một cây hoàn toàn đơn sắc:```
3
000
1 2
2 3
```Việc truyền tải tìm thấy một thành phần. Chiến lược sơn lại sẽ tốn bốn chi phí vì không có đỉnh màu đen để sơn lại và công thức trở thành`0 + 1 = 1`. Câu trả lời vẫn là một vì cây có thể bị xóa ngay lập tức. 

Đối với đầu vào nhỏ nhất có thể:```
1
1
```Việc truyền tải tạo ra chính xác một thành phần. Chiến lược sơn lại cũng đưa ra một, vì vậy câu trả lời cuối cùng là một. Thuật toán không vô tình trả về 0 vì việc xóa luôn được yêu cầu để loại bỏ đỉnh cuối cùng.
