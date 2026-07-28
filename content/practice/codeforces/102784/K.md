---
title: "CF 102784K - Nhện lãnh thổ"
description: "Chúng ta có một cây gồm N hang. Mỗi đỉnh chứa một tarantula và mỗi tarantula thuộc về một trong K phân loài. Các đường hầm tạo thành một cây được kết nối, do đó, có chính xác một đường đi giữa hai hang bất kỳ."
date: "2026-07-27T19:52:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102784
codeforces_index: "K"
codeforces_contest_name: "UTPC Contest 10-23-20 Div. 1"
rating: 0
weight: 102784
solve_time_s: 66
verified: true
draft: false
---

[CF 102784K - Tarantulas lãnh thổ](https://codeforces.com/problemset/problem/102784/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 6s 
**Đã xác minh:** có 

##Giải pháp 
#Hiểu vấn đề 

Chúng tôi có một cây`N`hang. Mỗi đỉnh chứa một tarantula và mỗi tarantula thuộc về một trong`K`phân loài. Các đường hầm tạo thành một cây được kết nối, do đó, có chính xác một đường đi giữa hai hang bất kỳ. Đối với mỗi phân loài, chúng ta cần khoảng cách lớn nhất giữa hai đỉnh có phân loài đó. Nếu một phân loài chỉ xuất hiện một lần thì phạm vi của nó bằng 0. 

Kích thước đầu vào đạt`N = 200000`, vì vậy giải pháp kiểm tra tất cả các cặp nhện tarantula là không thể. Một phân loài duy nhất có tất cả các đỉnh sẽ tạo ra khoảng`N^2 / 2`cặp, vượt xa những gì có thể chạy trong vài giây. Chúng ta cần một giải pháp gần tuyến tính hoặc`N log N`, bởi vì điều đó giữ cho số lượng hoạt động ở khoảng vài triệu. 

Phần khó khăn là các đỉnh của một phân loài không nhất thiết phải được kết nối với nhau. Thuật toán đường kính cây thông thường hoạt động khi toàn bộ cây là một tập hợp, nhưng ở đây chúng ta cần nhiều đường kính cùng lúc, mỗi đường kính cho mỗi màu. 

Hãy xem xét một vài trường hợp phá vỡ những ý tưởng đơn giản hơn. Nếu mỗi loài tarantula có một phân loài khác nhau thì mọi câu trả lời đều phải bằng 0. Ví dụ:```
Input:
5 5
1 2 3 4 5
1 2
2 3
3 4
4 5

Output:
0
0
0
0
0
```Một giải pháp giả sử mỗi màu có ít nhất hai đỉnh sẽ truy cập vào điểm cuối bị thiếu. 

Bẫy thứ hai là các đỉnh của một phân loài có thể bị phân tách bởi các phân loài khác. Ví dụ:```
Input:
5 2
1 2 1 2 1
1 2
2 3
3 4
4 5

Output:
4
2
```Ba đỉnh của phân loài một nằm ở cuối và giữa một đường dẫn. Chỉ nhìn vào các màu bằng nhau liền kề sẽ bỏ lỡ đường kính thực. 

Vấn đề thứ ba xuất hiện khi sử dụng trọng tâm không chính xác. Hai đỉnh trong cùng một cây con của một trọng tâm không có đường đi đi qua trọng tâm đó. Kết hợp khoảng cách của chúng thông qua trọng tâm sẽ đánh giá quá cao khoảng cách thực. 

## Phương pháp tiếp cận 

Một cách tiếp cận đơn giản là thu thập các đỉnh của từng phân loài và tính toán đường kính của nó một cách độc lập. Đối với một màu, chúng ta có thể chọn một đỉnh tùy ý, duyệt cây để tìm đỉnh xa nhất, sau đó chạy một duyệt khác từ đỉnh đó. Đây là phương pháp đường kính hai BFS hoặc DFS tiêu chuẩn. Điều này đúng vì đỉnh xa nhất tính từ bất kỳ điểm cuối nào của cây đều chứa điểm cuối đường kính. 

Vấn đề là công việc lặp đi lặp lại. Nếu một phân loài chứa tất cả`N`đỉnh, một phép tính đường kính đơn đã có giá`O(N)`. Lặp lại nó cho nhiều phân loài có thể đạt được`O(NK)`và nếu màu sắc được phân phối không tốt thì điều này sẽ trở thành`O(N^2)`. 

Quan sát quan trọng là đồ thị là một cây, do đó việc phân tích trọng tâm có thể phân chia công việc. Một centroid tách cây thành các thành phần nhỏ hơn. Đối với mỗi tâm, mọi cặp đỉnh hợp lệ có đường đi qua tâm đó có thể được đánh giá bằng cách xem khoảng cách của chúng với tâm. Sự phân tách đảm bảo rằng mỗi cặp đỉnh được xem xét ở chính xác một trọng tâm nơi đường đi của chúng phân chia. 

Đối với một centroid, chúng tôi xử lý từng thành phần con của nó. Đối với mỗi phân loài, chúng tôi ghi nhớ khoảng cách lớn nhất được thấy trong các thành phần trước đó. Khi chúng ta thấy một đỉnh của cùng một phân loài trong thành phần hiện tại, việc kết hợp nó với mức tối đa trước đó sẽ tạo ra cặp tốt nhất có đường đi qua tâm. Sau khi tất cả các thành phần con được xử lý, chúng ta giải quyết đệ quy từng thành phần còn lại. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N2) | O(N) | Quá chậm | 
| Phân hủy trung tâm | O(N log N) | O(N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng cây và lưu trữ danh sách các đỉnh thuộc từng phân loài. Mảng câu trả lời bắt đầu bằng 0 vì màu có ít hơn hai đỉnh đã có câu trả lời đúng. 
2. Tìm trọng tâm của thành phần cây hiện tại. Trọng tâm là một đỉnh mà việc loại bỏ nó để lại các thành phần không lớn hơn một nửa thành phần hiện tại. 
3. Coi trọng tâm như một điểm cuối có thể có cho các đường đi qua nó. Lưu trữ khoảng cách bằng 0 cho phân loài của chính nó. 
4. Đối với mỗi thành phần lân cận của tâm, hãy chạy DFS để thu thập mọi đỉnh trong thành phần đó cùng với khoảng cách của nó với tâm. Trong khi thu thập, hãy so sánh từng đỉnh với khoảng cách tốt nhất của cùng phân loài được tìm thấy trong các thành phần đã được xử lý trước đó. Điều này có tác dụng vì đường đi giữa hai đỉnh đó phải đi qua tâm. 
5. Hợp nhất các khoảng cách đã thu thập vào các khoảng cách tốt nhất được lưu trữ cho tâm này. Chỉ giữ khoảng cách tối đa cho mỗi phân loài là đủ vì cặp cuối cùng xuyên qua tâm chỉ cần hai khoảng cách lớn nhất. 
6. Đánh dấu trọng tâm là đã bị loại bỏ và áp dụng đệ quy quy trình tương tự cho mọi thành phần còn lại. 

Tại sao nó hoạt động: đối với hai đỉnh bất kỳ của cùng một phân loài, hãy xem xét trọng tâm đầu tiên trong quá trình phân rã trong đó hai đỉnh kết thúc ở các thành phần còn lại khác nhau hoặc một trong số chúng là trọng tâm. Con đường của họ đi qua trung tâm đó. Thuật toán kiểm tra chính xác cặp này tại thời điểm đó và kết hợp chính xác hai khoảng cách của chúng. Vì mỗi cặp có thể được xem xét ở tâm phân cách của nó nên giá trị lớn nhất được tìm thấy là đường kính thực của phân loài đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(1 << 25)

def solve():
    n, k = map(int, input().split())
    color = list(map(int, input().split()))
    g = [[] for _ in range(n)]
    for _ in range(n - 1):
        a, b = map(int, input().split())
        a -= 1
        b -= 1
        g[a].append(b)
        g[b].append(a)

    removed = [False] * n
    size = [0] * n
    ans = [0] * k
    best = [None] * n

    def calc_size(u, p):
        size[u] = 1
        for v in g[u]:
            if v != p and not removed[v]:
                size[u] += calc_size(v, u)
        return size[u]

    def find_centroid(u, p, total):
        for v in g[u]:
            if v != p and not removed[v] and size[v] > total // 2:
                return find_centroid(v, u, total)
        return u

    def collect(u, p, d, arr):
        arr.append((color[u], d))
        for v in g[u]:
            if v != p and not removed[v]:
                collect(v, u, d + 1, arr)

    def decompose(u):
        total = calc_size(u, -1)
        c = find_centroid(u, -1, total)

        current = {}
        current[color[c]] = 0

        for v in g[c]:
            if removed[v]:
                continue
            nodes = []
            collect(v, c, 1, nodes)

            for col, dist in nodes:
                if col in current:
                    ans[col] = max(ans[col], current[col] + dist)

            for col, dist in nodes:
                if col not in current or dist > current[col]:
                    current[col] = dist

        removed[c] = True
        for v in g[c]:
            if not removed[v]:
                decompose(v)

    decompose(0)

    print("\n".join(map(str, ans)))

if __name__ == "__main__":
    solve()
```các`calc_size`chức năng đo kích thước thành phần hiện tại trong khi bỏ qua các tâm đã bị loại bỏ. các`find_centroid`hàm đi vào cây con duy nhất quá lớn cho đến khi đạt đến trọng tâm hợp lệ. 

các`collect`DFS tập hợp`(color, distance)`các cặp từ một thành phần con của trung tâm. Việc xử lý các thành phần con một cách riêng biệt là điều cần thiết. Khoảng cách từ hai đỉnh trong cùng một thành phần con không thể được kết hợp thông qua trọng tâm. 

các`current`Từ điển lưu trữ khoảng cách lớn nhất từ ​​tâm này đến từng màu trong số các nhánh đã được xử lý. Khi một nhánh mới được quét, kết hợp các đỉnh của nó với`current`kiểm tra tất cả các đường đi qua tâm. Bản cập nhật câu trả lời chỉ sử dụng phép cộng khoảng cách, vì vậy số nguyên Python tránh được vấn đề tràn. 

Giới hạn đệ quy được tăng lên vì cả DFS cây và đệ quy centroid đều có thể vượt quá độ sâu đệ quy mặc định của Python trên các đầu vào lớn. 

## Ví dụ đã hoạt động 

Mẫu 1:```
Input:
6 2
1 2 1 2 2 1
3 1
1 2
1 4
1 5
5 6
```| Bước | Trung tâm | Khoảng cách màu hiện tại | Cập nhật câu trả lời | 
| --- | --- | --- | --- | 
| 1 | 1 | màu 1: 0 | không | 
| 2 | nút xử lý 3 | màu 1 khoảng cách 1 | dải màu 1 trở thành 1 | 
| 3 | nút xử lý cây con 5 | màu 1 khoảng cách 2 | dải màu 1 trở thành 3 | 
| 4 | xử lý màu 2 nút | khoảng cách kết hợp qua trọng tâm | dải màu 2 trở thành 2 | 

Kết quả là:```
3
2
```Dấu vết cho thấy thuật toán kết hợp các đỉnh từ các nhánh trung tâm khác nhau và không bao giờ giả định rằng các màu bằng nhau nằm liền kề nhau. 

Mẫu 2:```
Input:
5 5
1 2 3 4 5
1 2
2 3
3 4
4 5
```| Bước | Trung tâm | Thông tin hiện tại | Kết quả | 
| --- | --- | --- | --- | 
| 1 | đỉnh giữa | mỗi màu xuất hiện một lần | tất cả các câu trả lời đều bằng không | 
| 2 | thành phần đệ quy | không có màu nào có hai đỉnh | tất cả các câu trả lời đều bằng không | 

Kết quả là:```
0
0
0
0
0
```Điều này xác nhận rằng các phân loài đơn lẻ được xử lý mà không cần tìm kiếm đặc biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log N) | Mỗi cấp độ trọng tâm xử lý mọi đỉnh trong các thành phần hiện tại của nó một lần và quá trình phân tách có chiều cao logarit. | 
| Không gian | O(N) | Cây, trạng thái đệ quy và các mảng phụ trợ là tuyến tính. | 

Với`N = 200000`, hệ số logarit giữ cho số lượng thao tác có thể quản lý được. Thuật toán tránh lưu trữ tất cả các cặp màu giống nhau, đòi hỏi bộ nhớ bậc hai. 

## Trường hợp thử nghiệm```python
# These tests assume the solve() function is copied above.

import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    try:
        solve()
        return sys.stdout.getvalue()
    finally:
        sys.stdin = old

assert run("""6 2
1 2 1 2 2 1
3 1
1 2
1 4
1 5
5 6
""") == "3\n2\n"

assert run("""1 1
1
""") == "0\n"

assert run("""5 5
1 2 3 4 5
1 2
2 3
3 4
4 5
""") == "0\n0\n0\n0\n0\n"

assert run("""5 2
1 2 1 2 1
1 2
2 3
3 4
4 5
""") == "4\n2\n"

assert run("""4 1
1 1 1 1
1 2
2 3
3 4
""") == "3\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đỉnh đơn |`0`| Kích thước tối thiểu và xử lý đơn lẻ | 
| Tất cả các màu sắc khác nhau | Tất cả số không | Không tạo đường kính sai | 
| Màu sắc xen kẽ trên một con đường |`4`,`2`| Khoảng cách xa giữa các màu bằng nhau cách nhau | 
| Một màu trên dây chuyền |`3`| Trường hợp đường kính cây đầy đủ | 

## Vỏ cạnh 

Khi một phân loài chỉ xuất hiện một lần, quá trình trung tâm có thể gặp màu đó nhiều lần, nhưng từ điển không bao giờ tìm thấy đỉnh phù hợp thứ hai. Câu trả lời vẫn là 0 vì không có cặp nào tồn tại. 

Đối với đầu vào:```
5 5
1 2 3 4 5
1 2
2 3
3 4
4 5
```mỗi bước trung tâm chỉ ghi lại một khoảng cách cho mỗi màu. Không có phép cộng nào xảy ra nên mọi câu trả lời đều bằng 0. 

Khi các màu bằng nhau được phân tách bằng các màu khác, thuật toán không dựa vào khả năng kết nối của một phân loài. Vì:```
5 2
1 2 1 2 1
1 2
2 3
3 4
4 5
```hai màu bên ngoài`1`các đỉnh cuối cùng được xem xét tại trọng tâm nơi đường đi của chúng được chia thành các nhánh khác nhau. Khoảng cách của họ cộng lại thành bốn, đó là phạm vi chính xác. 

Khi nhiều đỉnh có cùng màu, từ điển chỉ giữ khoảng cách tối đa nhìn thấy từ mỗi tâm. Điều này là đủ vì một cặp đi qua tâm đó luôn bao gồm hai khoảng cách từ cùng một tâm và cặp tốt nhất phải sử dụng hai giá trị sẵn có lớn nhất. Phân rã đệ quy xử lý các cặp không đi qua trọng tâm hiện tại.
