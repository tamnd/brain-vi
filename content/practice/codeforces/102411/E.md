---
title: "CF 102411E - Cách đều"
description: "Mạng lưới đường sắt là một cái cây. Mỗi thành phố là một đỉnh, mỗi đường ray là một cạnh và việc đi qua một cạnh mất một giờ. Một tập hợp con của các đỉnh chứa các thành phố nơi các đội tọa lạc."
date: "2026-08-12T00:12:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102411
codeforces_index: "E"
codeforces_contest_name: "ICPC 2019-2020 North-Western Russia Regional Contest"
rating: 0
weight: 102411
solve_time_s: 167
verified: true
draft: false
---

[CF 102411E - Cách đều](https://codeforces.com/problemset/problem/102411/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 47s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Mạng lưới đường sắt là một cái cây. Mỗi thành phố là một đỉnh, mỗi đường ray là một cạnh và việc đi qua một cạnh mất một giờ. Một tập hợp con của các đỉnh chứa các thành phố nơi các đội tọa lạc. Chúng ta cần tìm một thành phố có khoảng cách từ cây đến mọi thành phố của đội là hoàn toàn giống nhau. Nếu một thành phố như vậy tồn tại thì bất kỳ thành phố nào hợp lệ đều có thể được in ra. 

Thực tế là đồ thị là một cái cây là thuộc tính cấu trúc quan trọng. Giữa hai thành phố có chính xác một con đường, do đó không có sự mơ hồ về khoảng cách hoặc nơi hai con đường gặp nhau. Số lượng thành phố có thể đạt tới (2 \cdot 10^5), và số đội cũng có thể đạt tới (2 \cdot 10^5). Với giới hạn 2 giây, thuật toán (O(n^2)) hoặc (O(nm)) sẽ yêu cầu khoảng (4 \cdot 10^{10}) thao tác trong trường hợp xấu nhất, vượt xa những gì thực tế. Chúng ta cần một thuật toán gần tuyến tính theo kích thước của cây. 

Có một số trường hợp đặc biệt có thể khiến một giải pháp tưởng chừng như hợp lý lại thất bại. Nếu chỉ có một đội thì mọi thành phố đều cách xa thành phố đó như nhau chỉ theo nghĩa tầm thường là chỉ có một khoảng cách để so sánh, nên thành phố nào cũng hợp lệ. Ví dụ,```
1 1
1
```có câu trả lời`YES`với thành phố`1`. Một giải pháp mù quáng sử dụng hai thành phố của nhóm sẽ tiếp cận được yếu tố thứ hai không tồn tại. 

Với hai đội, đường đi của họ phải có độ dài chẵn. Ví dụ,```
2 2
1 2
1 2
```chỉ có một cạnh giữa các đội, vì vậy không có đỉnh nào nằm chính xác giữa chúng. Câu trả lời đúng là`NO`. Một giải pháp bất cẩn có thể sử dụng phép chia số nguyên cho khoảng cách và chọn một điểm cuối, nhưng điểm cuối đó có khoảng cách bằng 0 với đội này và đội này với đội kia. 

Ngoài ra còn có chế độ thất bại thứ hai khi có ít nhất ba đội. Chỉ kiểm tra hai đội đầu tiên là không đủ. Coi như```
5 3
1 2
2 3
3 4
4 5
1 5 2
```Hai đội đầu tiên ở thành phố 1 và 5 nên trung điểm của họ là thành phố 3. Khoảng cách đến thành phố 1 và 5 của đội đó đều là 2, nhưng khoảng cách đến đội thứ 3 ở thành phố 2 chỉ là 1. Đáp án đúng là`NO`. Việc xác minh cuối cùng đối với mọi đội là điều cần thiết. 

## Phương pháp tiếp cận 

Một giải pháp bạo lực trực tiếp có thể lấy mọi thành phố làm địa điểm cuối cùng. Đối với mỗi thành phố ứng cử viên, nó có thể duyệt cây một lần để tính khoảng cách và sau đó kiểm tra xem tất cả các thành phố được đánh dấu có cùng khoảng cách hay không. Chi phí cho một lần di chuyển (O(n)) và có thể có (n) thành phố ứng cử viên, cho (O(n^2)) thời gian. Với (n=2\cdot10^5), đây là khoảng (4\cdot10^{10}) lượt truy cập đỉnh trong trường hợp xấu nhất. Lực lượng vũ phu là chính xác vì nó kiểm tra rõ ràng mọi câu trả lời có thể, nhưng nó quá chậm. 

Quan sát hữu ích đến từ việc chỉ xem xét hai thành phố của đội. Giả sử tồn tại một thành phố cuối cùng hợp lệ (x) và có hai đội ở (a) và (b). Vì (d(x,a)=d(x,b)), đường đi duy nhất từ ​​(a) đến (b) phải đi qua (x) và hai phần của đường đi đó phải có cùng độ dài. Do đó (x) chính là trung điểm của đường đi từ (a) đến (b). 

Điều này hoàn toàn quyết định ứng viên. Nếu khoảng cách giữa (a) và (b) là số lẻ thì không có đỉnh nào ở trung điểm nên câu trả lời ngay lập tức là không thể. Nếu khoảng cách là chẵn thì có đúng một đỉnh ở giữa. Chúng ta có thể tìm thấy nó bằng cách duyệt cây và sau đó kiểm tra xem mọi đội có cùng khoảng cách đến đỉnh đó hay không. 

Giải pháp vũ phu hoạt động vì nó kiểm tra tất cả các trung tâm có thể, nhưng không thành công vì có quá nhiều trung tâm. Nhận xét rằng bất kỳ trung tâm hợp lệ nào cũng phải là điểm giữa của đường đi giữa hai nhóm bất kỳ cho phép chúng tôi giảm việc tìm kiếm từ tất cả (n) thành phố xuống còn một ứng cử viên, sau đó là một lần duyệt qua xác minh. 

Đối với một đội, không có cặp nào có sẵn, vì vậy chúng tôi xử lý trường hợp đó một cách riêng biệt và chỉ cần trả lại chính thành phố của đội. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n^2)) | (O(n)) | Quá chậm | 
| Tối ưu | (O(n)) | (O(n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc cây và danh sách các thành phố của đội. Nếu chỉ có một đội, hãy xuất thành phố đó ngay lập tức. Không cần phải so sánh với đội khác vì chỉ có một khoảng cách. 
2. Chọn hai thành phố đầu tiên của đội, gọi chúng là (a) và (b). Root cây tại (a) và chạy DFS lặp để tính toán`parent[v]`Và`dist[v]`cho mỗi thành phố. Mảng cha ghi lại đường đi duy nhất từ ​​mỗi thành phố trở về (a), trong khi`dist[b]`cho biết độ dài của đường đi từ (a) đến (b). 
3. Đặt (D=dist[b]). Nếu (D) lẻ, xuất ra`NO`. Một đường đi có độ dài lẻ có một cạnh ở giữa chứ không phải ở đỉnh, vì vậy không có thành phố nào có thể cách xa cả hai điểm cuối như nhau. 
4. Nếu (D) chẵn, hãy tìm trung điểm bằng cách bắt đầu từ (b) và di chuyển lên trên qua`parent`chính xác (D/2) lần. Gọi thành phố kết quả (x). Khoảng cách của nó với cả (a) và (b) chính xác là (D/2). 
5. Chạy một đường truyền khác bắt đầu từ (x), tính khoảng cách từ (x) đến mọi thành phố. Cho phép`target`là khoảng cách từ (x) đến thành phố của đội đầu tiên. 
6. Kiểm tra từng thành phố của đội. Nếu khoảng cách của nó với (x) khác với`target`, đầu ra`NO`. Ngược lại, xuất ra`YES`và (x). 
7. Thí sinh không thể sai sau lần xác minh này. Mọi câu trả lời hợp lệ đều phải là điểm giữa của hai thành phố của đội đầu tiên và chúng tôi đã kiểm tra rõ ràng rằng điểm giữa này có cùng khoảng cách với mọi đội còn lại. 

Tại sao nó hoạt động: giả sử một thành phố hợp lệ (y) tồn tại. Đối với hai thành phố đầu tiên của đội (a) và (b), đẳng thức (d(y,a)=d(y,b)) buộc (y) nằm trên đường đi duy nhất của chúng và là điểm giữa của nó. Do đó, nếu đường đi đó có độ dài lẻ thì không tồn tại thành phố hợp lệ, trong khi nếu nó có độ dài chẵn thì câu trả lời duy nhất có thể là điểm giữa (x) được xây dựng bằng thuật toán. Lần duyệt cuối cùng kiểm tra chính xác điều kiện ban đầu cho mỗi nhóm, do đó, thuật toán sẽ chấp nhận chính xác khi ứng viên duy nhất đó hợp lệ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())

    graph = [[] for _ in range(n + 1)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        graph[u].append(v)
        graph[v].append(u)

    teams = list(map(int, input().split()))

    if m == 1:
        print("YES")
        print(teams[0])
        return

    a = teams[0]
    b = teams[1]

    parent = [0] * (n + 1)
    dist = [-1] * (n + 1)

    dist[a] = 0
    stack = [a]

    while stack:
        v = stack.pop()
        for u in graph[v]:
            if u == parent[v]:
                continue
            parent[u] = v
            dist[u] = dist[v] + 1
            stack.append(u)

    d = dist[b]

    if d % 2 == 1:
        print("NO")
        return

    center = b
    for _ in range(d // 2):
        center = parent[center]

    center_dist = [-1] * (n + 1)
    center_dist[center] = 0
    stack = [center]

    while stack:
        v = stack.pop()
        for u in graph[v]:
            if u == parent[v]:
                continue
            if center_dist[u] != -1:
                continue
            center_dist[u] = center_dist[v] + 1
            stack.append(u)

    target = center_dist[a]

    for city in teams:
        if center_dist[city] != target:
            print("NO")
            return

    print("YES")
    print(center)

if __name__ == "__main__":
    solve()
```Chuyến đi đầu tiên bắt nguồn từ thành phố của đội đầu tiên.`dist[b]`đưa ra khoảng cách chính xác giữa hai đội đầu tiên, trong khi`parent`hãy để chúng tôi đi lùi dọc theo con đường độc đáo của họ. Chúng ta không cần một cấu trúc tổ tiên chung thấp nhất riêng biệt vì một điểm cuối là gốc, do đó toàn bộ đường dẫn đến điểm cuối khác đã được lưu trữ trong chuỗi gốc. 

Việc tính điểm giữa sử dụng`d // 2`cha mẹ chuyển từ`b`. Nếu đường đi có độ dài (d=2k), thành phố`b`là (2k) cạnh từ`a`và di chuyển các cạnh (k) đi lên sẽ để lại chính xác các cạnh (k) cho một trong hai điểm cuối. Việc kiểm tra khoảng cách lẻ phải diễn ra trước phép tính này, vì đường đi có độ dài lẻ không có điểm giữa đỉnh. 

Quá trình truyền tải thứ hai bắt đầu từ trung tâm được đề xuất. Mục đích của nó không phải là tìm kiếm ứng viên khác mà chỉ để xác minh điều kiện ban đầu. Khoảng cách tham chiếu có thể được lấy từ`a`, và mỗi đội phải có chính xác khoảng cách đó. 

Việc truyền tải được triển khai bằng một ngăn xếp rõ ràng thay vì DFS đệ quy. Một cây có thể là một chuỗi gồm (2\cdot10^5), sẽ vượt quá độ sâu đệ quy thông thường của Python nếu sử dụng DFS đệ quy. Ngăn xếp rõ ràng tránh được vấn đề đó. 

các`center_dist[u] != -1`kiểm tra lần duyệt thứ hai sẽ ngăn việc truy cập lại cha mẹ. Trong lần duyệt đầu tiên, kiểm tra`u == parent[v]`là đủ vì mỗi đỉnh mới đạt tới chỉ có một đỉnh gốc trong cây có gốc. 

Tất cả các khoảng cách tối đa là (n-1), vì vậy số nguyên Python dễ dàng xử lý chúng. Không cần chuyển đổi lập chỉ mục cho các thành phố vì đầu vào sử dụng số đỉnh dựa trên một và các mảng được phân bổ bằng`n + 1`các vị trí. 

## Ví dụ đã hoạt động 

Đối với Mẫu 1, cây là```
1 - 2 - 3 - 4 - 5
            |
            6
```và các đội ở các thành phố 1, 5 và 6. 

| Bước |`a`|`b`|`dist[b]`|`center`|`target`| Kết quả | 
| --- | --- | --- | --- | --- | --- | --- | 
| Đọc đội | 1 | 5 | 4 | chưa được đặt | chưa được đặt | Tiếp tục | 
| Tìm trung điểm | 1 | 5 | 4 | 3 | chưa được đặt | Khoảng cách chẵn | 
| Xác minh đội | 1 | 5 | 4 | 3 | 2 | Kiểm tra tất cả | 
| Đội 1 | 1 | 5 | 4 | 3 | 2 | Khoảng cách 2 | 
| Đội 2 | 1 | 5 | 4 | 3 | 2 | Khoảng cách 2 | 
| Đội 3 | 1 | 5 | 4 | 3 | 2 | Khoảng cách 2 | 
| Đầu ra | 1 | 5 | 4 | 3 | 2 |`YES 3`| 

Đường đi từ thành phố 1 đến thành phố 5 có độ dài 4 nên thành phố 3 là điểm giữa của nó. Thành phố 6 cũng cách thành phố 3 hai cạnh nên cả ba đội đều cách nhau đúng hai giờ. Bất biến của thuật toán được hiển thị ở đây: sau khi xây dựng điểm giữa, câu hỏi duy nhất còn lại là liệu mọi thành phố được đánh dấu có cùng khoảng cách với nó hay không. 

Đối với Mẫu 2, cây chỉ chứa cạnh (1-2) và cả hai thành phố đều chứa các đội. 

| Bước |`a`|`b`|`dist[b]`|`dist[b] % 2`| Kết quả | 
| --- | --- | --- | --- | --- | --- | 
| Đọc đội | 1 | 2 | chưa được đặt | chưa được đặt | Tiếp tục | 
| Tìm khoảng cách | 1 | 2 | 1 | 1 | lẻ | 
| Kiểm tra tính chẵn lẻ | 1 | 2 | 1 | 1 |`NO`| 

Con đường duy nhất có một cạnh. Không có thành phố nào cách cả hai điểm cuối nửa cạnh, vì vậy thuật toán dừng lại một cách chính xác trước khi cố gắng xây dựng điểm giữa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n)) | Hai lần duyệt sẽ kiểm tra mọi đỉnh và mọi cạnh của cây với số lần không đổi. | 
| Không gian | (O(n)) | Danh sách kề, mảng cha, mảng khoảng cách và ngăn xếp truyền tải đều sử dụng không gian tuyến tính. | 

Cây chỉ chứa (n-1) cạnh, do đó hai phép duyệt cùng nhau thực hiện công việc (O(n)). Đối với (n=2\cdot10^5), điều này nằm trong thang đo dự định cho giới hạn 2 giây, trong khi mức sử dụng bộ nhớ tuyến tính cũng dễ dàng nằm gọn trong 512 MB. 

## Trường hợp thử nghiệm 

Dây thử nghiệm bên dưới giữ nguyên`solve()`thực hiện và tạm thời thay thế đầu vào và đầu ra tiêu chuẩn. Thử nghiệm quy mô tối đa sẽ xây dựng một ngôi sao với 200.000 thành phố và 199.999 đội. Mỗi đội cách thành phố 1 một cạnh, vì vậy thành phố 1 là trung tâm bắt buộc.```python
import sys
import io

input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())

    graph = [[] for _ in range(n + 1)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        graph[u].append(v)
        graph[v].append(u)

    teams = list(map(int, input().split()))

    if m == 1:
        print("YES")
        print(teams[0])
        return

    a = teams[0]
    b = teams[1]

    parent = [0] * (n + 1)
    dist = [-1] * (n + 1)

    dist[a] = 0
    stack = [a]

    while stack:
        v = stack.pop()
        for u in graph[v]:
            if u == parent[v]:
                continue
            parent[u] = v
            dist[u] = dist[v] + 1
            stack.append(u)

    d = dist[b]

    if d % 2 == 1:
        print("NO")
        return

    center = b
    for _ in range(d // 2):
        center = parent[center]

    center_dist = [-1] * (n + 1)
    center_dist[center] = 0
    stack = [center]

    while stack:
        v = stack.pop()
        for u in graph[v]:
            if u == parent[v]:
                continue
            if center_dist[u] != -1:
                continue
            center_dist[u] = center_dist[v] + 1
            stack.append(u)

    target = center_dist[a]

    for city in teams:
        if center_dist[city] != target:
            print("NO")
            return

    print("YES")
    print(center)

def run(inp: str) -> str:
    global input
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    input = sys.stdin.readline

    try:
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample 1
assert run(
    """6 3
1 2
2 3
3 4
4 5
4 6
1 5 6
"""
) == "YES\n3", "sample 1"

# Provided sample 2
assert run(
    """2 2
1 2
1 2
"""
) == "NO", "sample 2"

# Minimum-size input
assert run(
    """1 1

1
"""
) == "YES\n1", "single city and single team"

# Even path, midpoint is an internal vertex
assert run(
    """5 2
1 2
2 3
3 4
4 5
1 5
"""
) == "YES\n3", "even path midpoint"

# First two teams have a midpoint, but the third team breaks equality
assert run(
    """5 3
1 2
2 3
3 4
4 5
1 5 2
"""
) == "NO", "must verify every team"

# Maximum-size input with all team distances equal to the center
n = 200000
edges = "\n".join(f"1 {v}" for v in range(2, n + 1))
teams = " ".join(str(v) for v in range(2, n + 1))
maximum_case = f"{n} {n - 1}\n{edges}\n{teams}\n"

assert run(maximum_case) == "YES\n1", "maximum-size star"

print("All tests passed.")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1`, với thành phố duy nhất là thành phố nhóm |`YES`, thành phố`1`| Kích thước tối thiểu và trường hợp đặc biệt của một đội | 
| Con đường năm thành phố với đội 1 và 5 |`YES`, thành phố`3`| Tính toán chính xác điểm giữa và khoảng cách ranh giới | 
| Con đường năm thành phố với các đội 1, 5 và 2 |`NO`| Xác minh lần cuối của mỗi đội | 
| Ngôi sao 200.000 thành phố với tất cả 199.999 rời đi theo đội |`YES`, thành phố`1`| Kích thước đầu vào tối đa, khoảng cách bằng nhau và hiệu suất tuyến tính | 

## Vỏ cạnh 

Trường hợp một đội được xử lý trước bất kỳ lý luận dựa trên cặp nào. Vì```
1 1
1
```thuật toán nhìn thấy`m == 1`và in ngay lập tức`YES`Và`1`. Không cần xác định điểm giữa vì không có đội thứ hai. Một giải pháp luôn đọc`teams[1]`sẽ thất bại ở đây. 

Trường hợp khoảng cách lẻ bị bác bỏ trước khi xây dựng điểm giữa. Vì```
2 2
1 2
1 2
```lần truyền tải đầu tiên mang lại`dist[2] = 1`. Từ`1 % 2 == 1`, thuật toán in`NO`. Không có thành phố có giá trị nguyên nằm giữa hai thành phố của đội. 

Trường hợp cặp đầu tiên có vẻ hợp lệ nhưng đội khác không ở giữa sẽ được xử lý bằng lần duyệt thứ hai. Vì```
5 3
1 2
2 3
3 4
4 5
1 5 2
```khoảng cách từ 1 đến 5 là 4 nên thuật toán chọn thành phố 3. Mảng khoảng cách từ thành phố 3 cho`d(3,1)=2`,`d(3,5)=2`, Và`d(3,2)=1`. Do giá trị thứ ba khác với khoảng cách tham chiếu 2 nên thuật toán sẽ in`NO`. Điều này ngăn ngừa lỗi phổ biến khi chỉ kiểm tra cặp được sử dụng để xây dựng ứng cử viên. 

Cây có độ sâu tối đa là một trường hợp thực tế khác của Python. Một đường dẫn có 200.000 đỉnh có thể khiến DFS đệ quy vượt quá giới hạn đệ quy của Python. Giải pháp này sử dụng các ngăn xếp rõ ràng cho cả hai lần duyệt, do đó, một cây như```
1 - 2 - 3 - ... - 200000
```được xử lý mà không cần đệ quy. Ngăn xếp chứa các đỉnh đang chờ xử lý, trong khi mảng gốc và mảng khoảng cách giữ lại thông tin cây cần thiết cho giai đoạn giữa và giai đoạn xác minh. 

Cuối cùng, một tập hợp lớn các đội đều có thể ở xa một thành phố như nhau. Ở ngôi sao có kích thước tối đa, thành phố 1 được kết nối trực tiếp với mọi thành phố khác và mỗi thành phố từ 2 đến 200.000 đều có một đội. Hai đội đầu tiên cách nhau khoảng cách 2 qua thành phố 1 nên điểm giữa là thành phố 1. Lần duyệt thứ hai tìm khoảng cách 1 cho mọi đội và mọi so sánh đều thành công. Thuật toán in`YES`Và`1`, chứng tỏ rằng số lượng đội không làm thay đổi độ phức tạp tuyến tính.
