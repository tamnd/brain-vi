---
title: "CF 102407E - \u0421\u0442\u0440\u0430\u043d\u043d\u0430\u044f \u0438\u0433\u0440\u0430 \u043d\u0430 \u0433\u0440\u0430\u0444\u0435"
description: "Bảng là một đồ thị đơn giản vô hướng. Một nước đi không loại bỏ một đỉnh mà nó loại bỏ một cạnh và nước đi tiếp theo phải sử dụng một cạnh có chung điểm cuối với cạnh bị loại bỏ ngay trước nó. Một cạnh chỉ có thể được sử dụng một lần vì nó sẽ biến mất sau khi được chọn."
date: "2026-08-11T16:14:44+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102407
codeforces_index: "E"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2019-2020, \u0412\u0442\u043e\u0440\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430, \u0443\u0441\u043b\u043e\u0436\u043d\u0435\u043d\u043d\u0430\u044f \u043d\u043e\u043c\u0438\u043d\u0430\u0446\u0438\u044f"
rating: 0
weight: 102407
solve_time_s: 148
verified: true
draft: false
---

[CF 102407E - \u0421\u0442\u0440\u0430\u043d\u043d\u0430\u044f \u0438\u0433\u0440\u0430 \u043d\u0430 \u0433\u0440\u0430\u0444\u0435](https://codeforces.com/problemset/problem/102407/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 28s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bảng là một đồ thị đơn giản vô hướng. Một nước đi không loại bỏ một đỉnh mà nó loại bỏ một cạnh và nước đi tiếp theo phải sử dụng một cạnh có chung điểm cuối với cạnh bị loại bỏ ngay trước nó. Một cạnh chỉ có thể được sử dụng một lần vì nó sẽ biến mất sau khi được chọn. 

Một cách hữu ích để diễn giải lại trò chơi là quên đi các đỉnh ban đầu trong giây lát. Tạo một đồ thị mới có các đỉnh là các cạnh ban đầu. Kết nối chính xác hai đỉnh mới khi các cạnh ban đầu tương ứng có chung điểm cuối. Đây là **biểu đồ đường** của biểu đồ gốc. Trò chơi bây giờ như sau: người chơi đầu tiên chọn bất kỳ đỉnh nào của biểu đồ đường và mỗi bước đi tiếp theo sẽ chọn một đỉnh liền kề chưa sử dụng. Đây là phiên bản đỉnh vô hướng của Địa lý. Người chơi đầu tiên thắng nếu họ có thể chọn đỉnh ban đầu để lối chơi tối ưu cuối cùng khiến người chơi thứ hai không di chuyển. Đặc tính khớp của trò chơi này cho biết người chơi thứ hai sẽ thắng từ mọi đỉnh xuất phát có thể có một cách chính xác khi biểu đồ đường có sự khớp hoàn hảo. 

Đầu vào chứa tối đa (10^4) đỉnh gốc và (10^4) cạnh gốc. Giới hạn chính thức là 2 giây và 512 MB. Việc xây dựng trực tiếp biểu đồ đường đã bị nghi ngờ vì một đỉnh bậc cao duy nhất trong biểu đồ ban đầu tạo ra một cụm chứa tất cả các cạnh liên quan của nó. Với (10^4) cạnh, cụm đó có thể chứa gần (10^4) đỉnh và xung quanh (5\cdot10^7) cặp kề. Quan trọng hơn, tìm kiếm trò chơi brute-force có tính cấp số nhân hoặc tệ hơn, vì vậy giải pháp dự định phải khai thác cấu trúc của biểu đồ đường thay vì liệt kê các trạng thái trò chơi. 

Có một số trường hợp đặc biệt bộc lộ những cách hiểu sai phổ biến. Đầu tiên, tính chẵn lẻ của **tổng** số cạnh là không đủ. Coi như```
4 2
1 2
3 4
```Tổng số cạnh là 2, nhưng mỗi thành phần liên thông có một cạnh. Hai cạnh không liền nhau nên sau khi người chơi thứ nhất chọn một trong hai thì người chơi thứ hai không được di chuyển. Câu trả lời là`YES`. Một giải pháp chỉ kiểm tra`m % 2`sẽ in sai`NO`. 

Thứ hai, các đỉnh bị cô lập không có tác dụng. Vì```
4 1
1 2
```đỉnh 3 và 4 không thể tham gia bất kỳ nước đi nào. Cạnh duy nhất tạo thành thành phần một đỉnh trong biểu đồ đường nên Arthur thắng ngay và đáp án là`YES`. Việc đếm các đỉnh thay vì các cạnh hoặc yêu cầu mọi đỉnh ban đầu thuộc về một thành phần không tầm thường sẽ cho kết quả sai. 

Thứ ba, một thành phần liên thông có số cạnh chẵn sẽ cho kết quả ngược lại. Vì```
4 3
1 2
1 3
1 4
```ba cạnh ban đầu đều liền kề nhau nên đồ thị đường là (K_3). Arthur chọn một cạnh, đối thủ chọn cạnh khác, Arthur chọn cạnh cuối cùng nên Arthur thắng. Câu trả lời là`YES`. Một lập luận bất cẩn chỉ dựa trên thực tế là mọi thành phần được kết nối đều được kết nối sẽ bỏ lỡ tính chẵn lẻ của số cạnh của nó. 

## Phương pháp tiếp cận 

Cách tiếp cận brute-force là coi cạnh bị xóa hiện tại và tập hợp các cạnh đã bị xóa là trạng thái trò chơi hoàn chỉnh. Đối với mỗi cạnh hợp lệ tiếp theo, hãy xác định đệ quy xem đối thủ có thể thắng hay không và tuyên bố trạng thái hiện tại thắng nếu ít nhất một nước đi dẫn đến trạng thái thua cho đối thủ. Điều này đúng vì trò chơi là hữu hạn và mọi khả năng tiếp tục đều được xem xét. 

Vấn đề là số lần tiếp tục. Lấy một ngôi sao ban đầu có (m) cạnh. Hai cạnh của ngôi sao có chung tâm nên biểu đồ đường của nó là (K_m). Sau khi cạnh đầu tiên được chọn, mọi cạnh còn lại đều hợp lệ, sau đó là mọi cạnh còn lại sau đó, v.v. Do đó, đệ quy có thể kiểm tra thứ tự của (m!) Trình tự chơi khác nhau. Với (m) gần với (10^4), điều này hoàn toàn không khả thi. Ngay cả việc ghi nhớ cũng không làm cho không gian trạng thái chung trở nên thực tế, bởi vì một trạng thái về cơ bản là một tập hợp con của các cạnh được sử dụng cùng với cạnh hiện tại, tạo ra (O(m2^m)) trạng thái có thể. 

Nhận xét quan trọng là trò chơi vô hướng cụ thể này có đặc điểm phù hợp. Trong Địa lý đỉnh vô hướng, một biểu đồ có sự kết hợp hoàn hảo sẽ mang lại cho người chơi thứ hai một chiến lược phản ứng đơn giản. Bất cứ khi nào người chơi đầu tiên đi vào một đỉnh, người chơi thứ hai sẽ di chuyển đến đối tác của mình theo cách khớp hoàn hảo cố định. Ngược lại, nếu kết quả khớp tối đa để lại một số đỉnh không khớp, người chơi đầu tiên có thể bắt đầu từ đó và sử dụng các cạnh khớp làm chiến lược phản hồi. Nếu chiến lược từng thất bại do một đỉnh chưa từng có có thể tiếp cận được không đúng lúc, thì đường đi xen kẽ sẽ là đường dẫn tăng cường, mâu thuẫn với tính cực đại của kết quả khớp. Đây là đặc tính khớp tiêu chuẩn của Địa lý đỉnh vô hướng. 

Người chơi đầu tiên của chúng tôi được phép tự do chọn đỉnh bắt đầu. Do đó, biểu đồ đường sẽ thua đối với người chơi đầu tiên khi nó có kết quả khớp hoàn hảo. Do đó, chúng ta chỉ cần quyết định xem biểu đồ đường của biểu đồ đã cho có khớp hoàn hảo hay không. 

Có một sự đơn giản hóa cấu trúc khác. Xét một thành phần liên thông của đồ thị gốc chứa (k) cạnh. Đồ thị đường của nó được kết nối và có chính xác (k) đỉnh. Biểu đồ đường liên thông có số đỉnh chẵn luôn có sự kết hợp hoàn hảo. Tương tự, các cạnh của mọi đồ thị liên thông có số cạnh chẵn có thể được phân chia thành các cặp sao cho hai cạnh trong mỗi cặp có chung một đỉnh. Đây là thuộc tính tiêu chuẩn của biểu đồ đường. 

Điều ngược lại là ngay lập tức vì sự so khớp hoàn hảo chỉ có thể tồn tại trong đồ thị có số đỉnh chẵn. Do đó, một thành phần được kết nối của biểu đồ gốc tạo ra một thành phần hoàn toàn có thể khớp của biểu đồ đường một cách chính xác khi số cạnh ban đầu của nó là chẵn. 

Vì vậy, toàn bộ trò chơi quy về một điều kiện rất đơn giản: Arthur thua chính xác khi **mọi thành phần liên thông của đồ thị ban đầu đều chứa số cạnh chẵn**. Nếu ít nhất một thành phần được kết nối có số cạnh lẻ thì thành phần biểu đồ đường của nó có kích thước lẻ và không có sự kết hợp hoàn hảo, mang lại cho Arthur lợi thế khởi đầu chiến thắng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(m!)) | (O(m)) độ sâu đệ quy | Quá chậm | 
| Tối ưu | (O(n+m)) | (O(n)) | Đã chấp nhận | 

Thuật toán tối ưu không cần xây dựng biểu đồ đường và không cần biết chuỗi chiến thắng thực tế của các cạnh bị xóa. Nó chỉ cần tính chẵn lẻ của số cạnh trong mỗi thành phần được kết nối. 

## Hướng dẫn thuật toán

1. Xây dựng các thành phần được kết nối của biểu đồ gốc bằng cấu trúc Disjoint Set Union. Đối với mọi cạnh đầu vào ((u,v)), hợp nhất (u) và (v). Thông tin duy nhất cần có từ cấu trúc thành phần là thành phần nào chứa mỗi cạnh. 
2. Sau khi hoàn tất tất cả các công đoàn, hãy quét các cạnh ban đầu một lần nữa. Đối với một cạnh ((u,v)), hãy tìm đại diện của (u), cũng là đại diện của (v) và tăng số cạnh của thành phần đó. Việc đếm các cạnh sau tất cả các phép hợp sẽ tránh mọi sự phụ thuộc vào thứ tự xuất hiện của các cạnh đầu vào. 
3. Kiểm tra mọi thành phần có chứa ít nhất một cạnh. Nếu số cạnh của nó là số lẻ, hãy in`YES`. Thành phần như vậy tạo ra thành phần biểu đồ đường có số đỉnh là số lẻ, do đó thành phần đó không thể có kết quả khớp hoàn hảo. Arthur có thể bắt đầu với lợi thế từ đó và có chiến lược chiến thắng. 
4. Nếu mọi thành phần không trống đều có số cạnh chẵn, hãy in`NO`. Mỗi thành phần biểu đồ đường tương ứng có số đỉnh chẵn và do đó có sự kết hợp hoàn hảo. Sự kết hợp rời rạc của những kết hợp hoàn hảo này là sự kết hợp hoàn hảo của toàn bộ biểu đồ đường, do đó người chơi thứ hai có thể trả lời mọi lựa chọn ban đầu bằng cách sử dụng chiến lược kết hợp. 

### Tại sao nó hoạt động 

Bất biến trung tâm là sự tương ứng giữa các thành phần được kết nối của biểu đồ gốc và các thành phần được kết nối của biểu đồ đường. Hai cạnh ban đầu có thể được kết nối thông qua một chuỗi các cạnh ban đầu liền kề một cách chính xác khi chúng thuộc về cùng một thành phần được kết nối, do đó không có động thái trò chơi nào có thể chuyển từ thành phần ban đầu này sang thành phần ban đầu khác. 

Bên trong một thành phần được kết nối có (k) cạnh, biểu đồ đường có (k) đỉnh. Nếu (k) là số lẻ thì nó không thể có sự kết hợp hoàn hảo, do đó, trò chơi Địa lý vô hướng bắt đầu tự do trên thành phần đó sẽ giành chiến thắng cho người chơi đầu tiên. Nếu (k) chẵn thì biểu đồ đường có sự khớp hoàn hảo, do đó người chơi thứ hai luôn có thể trả lời dọc theo cạnh khớp. Biểu đồ đường toàn cục có sự khớp hoàn hảo một cách chính xác khi mọi thành phần ban đầu không trống đều có số cạnh chẵn. Do đó thuật toán in`YES`chính xác là khi Arthur có thành phần chiến thắng để bắt đầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())

    parent = list(range(n))
    size = [1] * n
    edges = []

    def find(x):
        while parent[x] != x:
            parent[x] = parent[parent[x]]
            x = parent[x]
        return x

    def union(a, b):
        a = find(a)
        b = find(b)
        if a == b:
            return

        if size[a] < size[b]:
            a, b = b, a

        parent[b] = a
        size[a] += size[b]

    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        edges.append((u, v))
        union(u, v)

    edge_count = [0] * n

    for u, _ in edges:
        root = find(u)
        edge_count[root] += 1

    for v in range(n):
        if edge_count[v] % 2 == 1:
            print("YES")
            return

    print("NO")

if __name__ == "__main__":
    solve()
```Vòng lặp đầu tiên lưu trữ mọi cạnh và nối các điểm cuối của nó trong DSU. Nén đường dẫn trong`find`và sự kết hợp theo kích thước thành phần làm cho tổng DSU hoạt động tuyến tính một cách hiệu quả đối với những ràng buộc này. 

Vòng lặp thứ hai đếm các cạnh theo đại diện thành phần cuối cùng của chúng. Sử dụng điểm cuối đầu tiên là đủ vì mọi cạnh đã được hợp nhất nên cả hai điểm cuối đều thuộc về cùng một thành phần DSU. Chúng tôi cố tình thực hiện việc đếm này sau tất cả các công đoàn. Nếu việc này được thực hiện trong khi đọc đầu vào, ban đầu một cạnh có thể được gán cho một đại diện mà sau đó sẽ được hợp nhất thành một thành phần khác. 

Vòng lặp cuối cùng chỉ kiểm tra tính chẵn lẻ tại các đại diện DSU có số đếm khác 0. Các cạnh không đại diện có số lượng bằng 0 vì mọi cạnh được chỉ định bằng cách sử dụng đại diện cuối cùng của nó. Các đỉnh biệt lập cũng có số đếm bằng 0, điều này khiến chúng không liên quan đến trò chơi. 

Không có vấn đề tràn số nguyên trong Python và số lượng thành phần lớn nhất có thể chỉ là (10^4). Việc lập chỉ mục được chuyển đổi ngay lập tức từ các đỉnh dựa trên một của bài toán sang mảng Python dựa trên 0, do đó DSU có chính xác (n) phần tử. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đồ thị là```
7 5
1 2
5 1
5 6
3 2
2 4
```Tất cả năm cạnh thuộc về cùng một thành phần được kết nối. Các phép toán DSU hợp nhất tất cả các đỉnh xuất hiện trong biểu đồ và số cạnh cuối cùng của thành phần đó là 5. 

| Đại diện thành phần | Số cạnh | Chẵn lẻ | Quyết định | 
| --- | --- | --- | --- | 
| thành phần chứa 1 | 5 | lẻ |`YES`| 

Biểu đồ đường tương ứng có năm đỉnh. Vì số đỉnh của nó là số lẻ nên nó không thể có sự kết hợp hoàn hảo. Arthur có thể chọn một lợi thế trong phần này làm nước đi đầu tiên và buộc phải thắng. Điều này phù hợp với đầu ra mẫu`YES`. 

### Mẫu 2 

Đồ thị là```
3 2
1 2
2 3
```Cả hai cạnh đều thuộc về cùng một thành phần được kết nối, có số cạnh là 2. 

| Đại diện thành phần | Số cạnh | Chẵn lẻ | Quyết định | 
| --- | --- | --- | --- | 
| thành phần chứa 1 | 2 | thậm chí | tiếp tục | 
| tất cả các thành phần | thậm chí | thậm chí |`NO`| 

Hai cạnh ban đầu có chung đỉnh 2 nên đồ thị đường chứa hai đỉnh liền kề. Người chơi đầu tiên chọn một cái và người chơi thứ hai chọn cái còn lại. Người chơi đầu tiên sau đó không có động thái nào. Như vậy Arthur thua và câu trả lời là`NO`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n+m)) | Hoạt động DSU và hai lần quét tuyến tính trên các đỉnh và cạnh | 
| Không gian | (O(n+m)) | Mảng DSU cộng với bộ lưu trữ cho các cạnh gốc | 

Với (n,m\le 10^4), điều này nằm trong giới hạn 2 giây và 512 MB chính thức. Quan trọng hơn, thuật toán không bao giờ xây dựng biểu đồ đường có mật độ dày đặc và không bao giờ khám phá các trạng thái trò chơi. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def solve():
    input = sys.stdin.readline

    n, m = map(int, input().split())
    parent = list(range(n))
    size = [1] * n
    edges = []

    def find(x):
        while parent[x] != x:
            parent[x] = parent[parent[x]]
            x = parent[x]
        return x

    def union(a, b):
        a = find(a)
        b = find(b)

        if a == b:
            return

        if size[a] < size[b]:
            a, b = b, a

        parent[b] = a
        size[a] += size[b]

    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        edges.append((u, v))
        union(u, v)

    edge_count = [0] * n

    for u, _ in edges:
        edge_count[find(u)] += 1

    for count in edge_count:
        if count % 2 == 1:
            return "YES"

    return "NO"

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)
    try:
        return solve()
    finally:
        sys.stdin = old_stdin

# Provided samples
assert run(
    """7 5
1 2
5 1
5 6
3 2
2 4
"""
) == "YES", "sample 1"

assert run(
    """3 2
1 2
2 3
"""
) == "NO", "sample 2"

# Minimum-size graph: one edge is an odd component.
assert run(
    """2 1
1 2
"""
) == "YES", "minimum-size graph"

# Two separate one-edge components. Total m is even, but
# each component is odd, so checking only m % 2 would fail.
assert run(
    """4 2
1 2
3 4
"""
) == "YES", "disconnected odd components"

# Four edges incident to the same center. The line graph is K4,
# which has a perfect matching.
assert run(
    """5 4
1 2
1 3
1 4
1 5
"""
) == "NO", "even star"

# Three edges incident to the same center. The line graph is K3,
# which has odd size and no perfect matching.
assert run(
    """4 3
1 2
1 3
1 4
"""
) == "YES", "odd star"

# Maximum-size instance: n = m = 10000.
# A path with 9999 edges plus edge (1, 3) has 10000 edges
# and remains connected, so the answer is NO.
n = 10000
edges = [(i, i + 1) for i in range(1, n)]
edges.append((1, 3))

inp = f"{n} {len(edges)}\n"
inp += "\n".join(f"{u} {v}" for u, v in edges) + "\n"

assert run(inp) == "NO", "maximum-size connected even-edge graph"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 1 / 1 2`|`YES`| Đồ thị tối thiểu có thể và thành phần lẻ | 
|`4 2 / 1 2 / 3 4`|`YES`| Các thành phần bị ngắt kết nối và thực tế là tính chẵn lẻ của cạnh toàn cầu là không đủ | 
|`5 4 / 1 2 / 1 3 / 1 4 / 1 5`|`NO`| Bốn cạnh liền kề nhau và một thành phần chẵn | 
|`4 3 / 1 2 / 1 3 / 1 4`|`YES`| Thành phần lẻ có nhiều cạnh liền kề | 
|`10000 10000`xây dựng kết nối |`NO`| Giới hạn tối đa và thành phần được kết nối lớn | 

## Vỏ cạnh 

Đối với trường hợp ngắt kết nối```
4 2
1 2
3 4
```DSU sản xuất hai thành phần. Hình đầu tiên chứa một cạnh và hình thứ hai cũng chứa một cạnh, do đó số lượng của chúng là`1`Và`1`. Thuật toán gặp số lẻ ngay lập tức và in`YES`. Đây chính xác là tình huống chỉ kiểm tra tính chẵn lẻ của`m`không thành công, vì (1+1=2) chẵn trong khi không thành phần nào có kết quả khớp hoàn hảo trong biểu đồ đường của nó. 

Đối với các đỉnh bị cô lập, hãy xem xét```
4 1
1 2
```DSU tạo ra một thành phần chứa các đỉnh 1 và 2 có một cạnh, trong khi các đỉnh 3 và 4 vẫn bị cô lập với số cạnh bằng 0. Thành phần khác 0 có kích thước lẻ nên thuật toán in ra`YES`. Các thành phần không có cạnh nào không quan trọng vì không có cạnh nào để chọn từ chúng và do đó không có vị trí trò chơi nào liên quan đến chúng. 

Vì ngôi sao lẻ loi```
4 3
1 2
1 3
1 4
```cả ba cạnh đều thuộc một thành phần, do đó số cạnh là 3. Biểu đồ đường là (K_3). Arthur có thể xóa bất kỳ cạnh nào trước, đối thủ của anh ta có thể xóa một trong hai cạnh còn lại và Arthur xóa cạnh cuối cùng. Thuật toán in`YES`mà không bao giờ xây dựng hình tam giác đó. 

Đối với ngôi sao chẵn```
5 4
1 2
1 3
1 4
1 5
```thành phần chứa bốn cạnh và mọi cặp cạnh đó đều liền kề nhau. Biểu đồ đường là (K_4), có sự kết hợp hoàn hảo có thể ghép bốn đỉnh thành hai cặp. Người chơi thứ hai luôn có thể trả lời cạnh đã chọn của người chơi đầu tiên bằng đối tác phù hợp của mình, vì vậy Arthur thua và thuật toán in ra`NO`. 

Lỗi triển khai nguy hiểm nhất là đếm các cạnh trước khi DSU hoàn thành việc hợp nhất các thành phần. Ví dụ,```
4 2
1 2
2 3
```đầu tiên nối 1 với 2, sau đó nối 2 với 3. Cuối cùng, cả hai cạnh đều thuộc về một thành phần có số đếm là 2, vì vậy câu trả lời là`NO`. Việc đếm các đại diện cũ trong quá trình xử lý đầu vào có thể phân chia hai cạnh đó thành các ID thành phần tạm thời và tạo ra tính chẵn lẻ sai. Giải pháp tránh điều đó bằng cách lưu trữ các cạnh và chỉ đếm chúng sau khi tất cả các phép hợp hoàn tất. 

Trường hợp ranh giới cuối cùng là một đồ thị có (10^4) đỉnh và (10^4) cạnh. Một công trình được kết nối được lấy từ đường dẫn```
1-2-3-...-10000
```với một cạnh phụ`1 3`. Nó có chính xác 10000 cạnh, tất cả trong một thành phần được kết nối, do đó tính chẵn lẻ của thành phần là chẵn và câu trả lời là`NO`. DSU xử lý toàn bộ phiên bản có cấu trúc tuyến tính giống như một biểu đồ nhỏ, đó là lý do tại sao giải pháp vẫn nhanh ở giới hạn trên.
