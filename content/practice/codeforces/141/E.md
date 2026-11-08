---
title: "CF 141E - Dọn dẹp"
description: "Chúng ta có một đồ thị vô hướng trong đó mọi cạnh đều thuộc về một trong hai loại. Những con đường được đánh dấu \"S\" là những con đường hẹp mà Yêu tinh dọn dẹp và những con đường được đánh dấu \"M\" là những con đường rộng mà ông già Noel dọn dẹp. Chúng ta phải chọn một tập hợp con các con đường thỏa mãn hai điều kiện cùng một lúc."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "dp", "dsu", "graphs"]
categories: ["algorithms"]
codeforces_contest: 141
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 101 (Div. 2)"
rating: 2300
weight: 141
solve_time_s: 171
verified: true
draft: false
---

[CF 141E - Đang dọn dẹp](https://codeforces.com/problemset/problem/141/E) 

**Đánh giá:** 2300 
**Tags:** thuật toán xây dựng, dp, dsu, đồ thị 
**Thời gian giải:** 2m 51s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị vô hướng trong đó mọi cạnh đều thuộc về một trong hai loại. Đường được đánh dấu`"S"`là những con đường hẹp mà Yêu tinh dọn dẹp và những con đường được đánh dấu`"M"`là những con đường rộng rãi mà ông già Noel đã dọn sạch. 

Chúng ta phải chọn một tập hợp con các con đường thỏa mãn hai điều kiện cùng một lúc. 

Đầu tiên, những con đường được chọn phải tạo thành một cái cây bao phủ tất cả các túp lều. Giữa mỗi cặp túp lều phải có chính xác một đường đi đơn giản, đó là thuộc tính xác định của cây bao trùm. Một cây bao trùm trên`n`đỉnh luôn chứa chính xác`n - 1`các cạnh. 

Thứ hai, số lượng`"S"`các cạnh và`"M"`các cạnh bên trong cây bao trùm đó phải bằng nhau. 

Điều đó ngay lập tức đưa ra một quan sát quan trọng. Vì cây có`n - 1`tổng số cạnh thì số này phải là số chẵn. Nếu không thì không thể chia đều các cạnh giữa hai loại. 

Đồ thị có số đỉnh nhỏ, chỉ tối đa`1000`, nhưng số cạnh có thể đạt tới`100000`. Điều này thay đổi hoàn toàn hình dạng của giải pháp. Các thuật toán thử nhiều tập con của các cạnh là vô vọng, nhưng các thuật toán dựa trên các phép toán DSU lặp lại là đủ nhanh vì DSU chạy gần như trong thời gian khấu hao không đổi. MỘT`O(m α(n))`hoặc`O(m log n)`giải pháp có thể dễ dàng được chấp nhận. 

Có một số trường hợp khó bỏ sót. 

Biểu đồ có thể đã bị ngắt kết nối ngay cả khi chúng ta bỏ qua các loại cạnh.```
4 2
1 2 S
3 4 M
```Không có cây bao trùm nào tồn tại ở đây nên câu trả lời đúng là`-1`. 

Một trường hợp tinh vi khác xuất hiện khi một loại cạnh không thể xuất hiện đủ số lần trong bất kỳ cây bao trùm nào.```
3 2
1 2 S
2 3 S
```Một cây bao trùm tồn tại nhưng nó chỉ chứa`"S"`các cạnh. Vì một câu trả lời hợp lệ cần một`"S"`và một`"M"`cạnh, câu trả lời vẫn là`-1`. 

Tự lặp lại là một cái bẫy khác.```
2 2
1 1 S
1 2 M
```Vòng lặp tự không bao giờ giúp kết nối các thành phần, do đó việc triển khai bất cẩn đếm các cạnh theo loại một cách mù quáng có thể chấp nhận nó một cách không chính xác. 

Các cạnh song song cũng có vấn đề.```
2 3
1 2 S
1 2 S
1 2 M
```Chỉ có một cạnh có thể xuất hiện trong cây bao trùm. Thuật toán phải xử lý mọi cạnh một cách độc lập và tránh thêm các bản sao tạo thành chu trình. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Một cây bao trùm trên`n`đỉnh chứa chính xác`n - 1`các cạnh, vì vậy chúng ta có thể thử mọi tập hợp con có kích thước đó, kiểm tra xem nó có tạo thành cây hay không và xác minh rằng số lượng`"S"`Và`"M"`các cạnh đều bằng nhau. 

Điều này hoạt động hợp lý vì các ràng buộc xác định một câu trả lời hợp lệ rất dễ kiểm tra. Cả khả năng kết nối và tính không tuần hoàn đều có thể được kiểm tra bằng DSU hoặc DFS. 

Vấn đề là số lượng tập hợp con. Trong trường hợp xấu nhất chúng ta sẽ kiểm tra$$\binom{100000}{999}$$tập hợp con, điều này hoàn toàn không thể. 

Quan sát quan trọng là phần khó khăn duy nhất là cân bằng số lượng loại cạnh. Bản thân cấu trúc cây bao trùm rất dễ quản lý bằng DSU. 

Giả định`k = (n - 1) / 2`. Bất kỳ cây hợp lệ nào cũng phải chứa chính xác`k` `"S"`các cạnh và chính xác`k` `"M"`các cạnh. 

Thay vì xây dựng toàn bộ cây cùng một lúc, chúng ta có thể nghĩ về phạm vi khả thi. 

Đầu tiên, số lượng tối thiểu là bao nhiêu`"S"`các cạnh mà bất kỳ cây khung nào cũng phải chứa? Nếu chúng ta tham lam kết nối các thành phần chỉ bằng cách sử dụng`"M"`các cạnh trước, sau đó mọi kết nối bắt buộc còn lại phải sử dụng`"S"`các cạnh. Điều này đưa ra số lượng tối thiểu có thể của`"S"`các cạnh. 

Tương tự như vậy, nếu chúng ta tham lam sử dụng`"S"`trước tiên, chúng ta nhận được số lượng tối đa có thể của`"S"`các cạnh. 

Nếu như`k`nằm ngoài khoảng này thì không có câu trả lời nào tồn tại. 

Tốt hơn nữa, một khi tính khả thi được xác nhận, chúng ta có thể xây dựng một cây như vậy một cách rõ ràng. Đầu tiên chúng tôi buộc chính xác`k` `"S"`các cạnh trong khi vẫn duy trì khả năng mở rộng cho cây bao trùm. Sau đó, chúng tôi hoàn thành các kết nối còn lại bằng cách sử dụng`"M"`các cạnh. 

Toàn bộ giải pháp trở thành một chuỗi DSU chuyển qua danh sách cạnh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Tối ưu | O(m α(n)) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc tất cả các cạnh và chia chúng thành hai danh sách,`"S"`các cạnh và`"M"`các cạnh. 
2. Nếu`n - 1`lạ nhỉ, in ngay`-1`. 

Một cây bao trùm hợp lệ sẽ cần số lượng cả hai loại cạnh bằng nhau, do đó tổng số cạnh phải chia hết cho hai. 
3. Hãy để`k = (n - 1) / 2`. 
4. Tính số lượng tối thiểu có thể của`"S"`các cạnh trong bất kỳ cây bao trùm nào. 

Bắt đầu một DSU và tham lam thêm tất cả những gì có thể`"M"`các cạnh đầu tiên. Sau đó, hãy thử thêm`"S"`các cạnh. Mọi`"S"`cạnh thực sự hợp nhất hai thành phần là không thể tránh khỏi, vì vậy hãy tính nó. 
5. Nếu mức tối thiểu này vượt quá`k`, in`-1`. 

Mỗi cây bao trùm sẽ chứa quá nhiều`"S"`các cạnh. 
6. Tính số lượng tối đa có thể của`"S"`các cạnh. 

Bắt đầu một DSU khác và tham lam thêm tất cả những gì có thể`"S"`các cạnh đầu tiên. Đếm xem có bao nhiêu đã được thêm vào. 
7. Nếu mức tối đa này nhỏ hơn`k`, in`-1`. 

Không có cây bao trùm nào có thể chứa đủ`"S"`các cạnh. 
8. Thi công chính xác`k` `"S"`các cạnh. 

Bắt đầu với một DSU mới. Đầu tiên tham lam thêm`"S"`các cạnh bất cứ khi nào chúng kết nối các thành phần khác nhau. Lưu trữ các cạnh này tạm thời. 

Điều này tạo ra một khu rừng tối đa bằng cách sử dụng`"S"`các cạnh. 
9. Một số trong số đó`"S"`các cạnh có thể không cần thiết. Chúng tôi chỉ cần chính xác`k`. 

Xây dựng lại DSU từ đầu. Lặp lại phần được lưu trữ`"S"`các cạnh và thêm chúng từng cái một cho đến khi chính xác`k`đã được chọn. 
10. Tiếp tục quét tất cả`"S"`các cạnh một lần nữa. 

Nếu chúng ta vẫn còn ít hơn`k`đã chọn`"S"`các cạnh, hãy thêm nhiều hơn bất cứ khi nào chúng kết nối các thành phần khác nhau. 

Tại thời điểm này chúng tôi có chính xác`k` `"S"`rìa tạo thành một khu rừng. 
11. Hoàn thiện cây bao trùm bằng cách sử dụng`"M"`các cạnh. 

Lặp lại tất cả`"M"`các cạnh và thêm bất kỳ cạnh nào có điểm cuối thuộc về các thành phần khác nhau. 
12. Nếu tổng số cạnh được chọn không bằng`n - 1`, in`-1`. 

Nếu không thì xuất ra các chỉ số cạnh đã chọn. 

### Tại sao nó hoạt động 

Việc kiểm tra tính khả thi tối thiểu và tối đa dựa trên đối số trao đổi cây bao trùm tiêu chuẩn. Tham lam giành lấy tất cả những gì có thể`"M"`các cạnh giảm thiểu số lượng yêu cầu`"S"`các cạnh vì mọi sự hợp nhất đều tránh được bằng cách sử dụng`"M"`sẽ chỉ ép buộc người khác`"S"`hợp nhất sau này. Đối số đối xứng cho mức tối đa. 

Trong quá trình thi công đã lựa chọn`"S"`các cạnh luôn tạo thành một khu rừng theo chu kỳ. Mỗi lần chúng ta thêm một thành phần, nó sẽ hợp nhất hai thành phần đã bị ngắt kết nối trước đó. Vì khoảng khả thi đã chứng minh chính xác rằng`k` `"S"`có thể đạt được, chúng ta có thể dừng lại một cách an toàn sau khi chọn`k`các cạnh đó và hoàn thành các kết nối còn lại bằng cách sử dụng`"M"`các cạnh. 

Trạng thái DSU cuối cùng đảm bảo cả tính không tuần hoàn và khả năng kết nối. Chúng tôi chỉ thêm các cạnh kết nối các thành phần khác nhau nên các chu trình không bao giờ xuất hiện. Một cây bao trùm trên`n`đỉnh với`n - 1`các cạnh được kết nối tự động và không theo chu kỳ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class DSU:
    def __init__(self, n):
        self.parent = list(range(n + 1))
        self.size = [1] * (n + 1)

    def find(self, x):
        while self.parent[x] != x:
            self.parent[x] = self.parent[self.parent[x]]
            x = self.parent[x]
        return x

    def union(self, a, b):
        a = self.find(a)
        b = self.find(b)

        if a == b:
            return False

        if self.size[a] < self.size[b]:
            a, b = b, a

        self.parent[b] = a
        self.size[a] += self.size[b]
        return True

def solve():
    n, m = map(int, input().split())

    edges_s = []
    edges_m = []

    all_edges = []

    for idx in range(1, m + 1):
        u, v, t = input().split()
        u = int(u)
        v = int(v)

        edge = (u, v, idx)

        all_edges.append((u, v, t, idx))

        if t == 'S':
            edges_s.append(edge)
        else:
            edges_m.append(edge)

    if (n - 1) % 2:
        print(-1)
        return

    k = (n - 1) // 2

    # minimum possible number of S edges
    dsu = DSU(n)

    for u, v, _ in edges_m:
        dsu.union(u, v)

    min_s = 0

    for u, v, _ in edges_s:
        if dsu.union(u, v):
            min_s += 1

    if min_s > k:
        print(-1)
        return

    # maximum possible number of S edges
    dsu = DSU(n)

    max_s = 0

    for u, v, _ in edges_s:
        if dsu.union(u, v):
            max_s += 1

    if max_s < k:
        print(-1)
        return

    # build exactly k S edges
    dsu = DSU(n)

    useful_s = []

    for u, v, idx in edges_s:
        if dsu.union(u, v):
            useful_s.append((u, v, idx))

    dsu = DSU(n)

    answer = []
    used_s = 0

    for u, v, idx in useful_s:
        if used_s == k:
            break

        if dsu.union(u, v):
            answer.append(idx)
            used_s += 1

    for u, v, idx in edges_s:
        if used_s == k:
            break

        if dsu.union(u, v):
            answer.append(idx)
            used_s += 1

    # finish with M edges
    for u, v, idx in edges_m:
        if dsu.union(u, v):
            answer.append(idx)

    if len(answer) != n - 1:
        print(-1)
        return

    print(len(answer))
    print(*answer)

solve()
```Chi tiết quan trọng đầu tiên là ngăn cách`"S"`Và`"M"`các cạnh. Toàn bộ công trình phụ thuộc vào việc xử lý hai loại một cách độc lập. 

Kiểm tra tính khả thi tối thiểu và tối đa sử dụng lại cấu trúc DSU tương tự nhưng theo thứ tự ngược lại. Thứ tự là toàn bộ đối số. Thêm tất cả`"M"`mép lá đầu tiên chỉ bị ép buộc`"S"`hợp nhất, trong khi thêm tất cả`"S"`đầu tiên các cạnh tối đa hóa số cạnh như vậy có thể tham gia. 

Giai đoạn xây dựng có cấu trúc hai giai đoạn tinh tế. Lần vượt qua đầu tiên`"S"`các cạnh tính toán một khu rừng tối đa có khả năng hữu ích`"S"`các cạnh. Lần thứ hai chỉ chọn cẩn thận`k`của họ. 

Nếu không xây dựng lại DSU trước giai đoạn thứ hai, chúng ta sẽ vô tình giữ lại quá nhiều`"S"`các cạnh đã được sáp nhập vào cấu trúc. 

Một sai lầm dễ mắc phải khác là cho rằng việc đạt được chính xác`k` `"S"`các cạnh tự động đảm bảo kết nối. Nó không. Lần vượt qua cuối cùng`"M"`các cạnh vẫn cần thiết để kết nối tất cả các thành phần còn lại. 

Kiểm tra cuối cùng`len(answer) != n - 1`bắt các trường hợp bị ngắt kết nối một cách sạch sẽ. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
1 2
1 1 S
1 1 M
```Đây`n - 1 = 0`, Vì thế`k = 0`. 

| Bước | Hành động | Các cạnh được chọn | Linh kiện | 
| --- | --- | --- | --- | 
| Ban đầu | Không cần cạnh | {} | 1 | 
| Cuối cùng | Cây đã hoàn thiện | {} | 1 | 

Đầu ra:```
0
```Ví dụ này xác nhận rằng đồ thị có một đỉnh không yêu cầu cạnh nào cả. Vòng lặp tự bị bỏ qua vì chúng không bao giờ hợp nhất các thành phần. 

### Ví dụ 2 

đầu vào:```
3 3
1 2 S
2 3 M
1 3 S
```Chúng tôi cần chính xác một`"S"`cạnh và một`"M"`bờ rìa. 

| Bước | Cạnh được xem xét | Đã chấp nhận | S đã qua sử dụng | thành phần DSU | 
| --- | --- | --- | --- | --- | 
| 1 | 1-2 giây | Có | 1 | {1,2}, {3} | 
| 2 | 1-3 giây | Bỏ qua tạm thời | 1 | {1,2}, {3} | 
| 3 | 2-3 triệu | Có | 1 | {1,2,3} | 

Các cạnh được chọn là`{1, 2}`. 

Dấu vết này thể hiện sự bất biến chính. các`"S"`các cạnh chỉ được chọn khi chúng mở rộng rừng và`"M"`edge kết thúc kết nối mà không tạo chu trình. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(m α(n)) | Mỗi cạnh tham gia vào một số lượng hoạt động DSU không đổi | 
| Không gian | O(n) | Mảng DSU và lưu trữ câu trả lời | 

Đồ thị chứa nhiều nhất`100000`các cạnh, do đó việc chuyển tuyến tính qua danh sách cạnh là hoàn toàn ổn. Hoạt động DSU có hiệu quả là thời gian không đổi vì tốc độ tăng trưởng nghịch đảo của Ackermann trong thực tế là rất nhỏ. Giải pháp dễ dàng phù hợp với cả giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    class DSU:
        def __init__(self, n):
            self.parent = list(range(n + 1))
            self.size = [1] * (n + 1)

        def find(self, x):
            while self.parent[x] != x:
                self.parent[x] = self.parent[self.parent[x]]
                x = self.parent[x]
            return x

        def union(self, a, b):
            a = self.find(a)
            b = self.find(b)

            if a == b:
                return False

            if self.size[a] < self.size[b]:
                a, b = b, a

            self.parent[b] = a
            self.size[a] += self.size[b]
            return True

    n, m = map(int, input().split())

    edges_s = []
    edges_m = []

    for idx in range(1, m + 1):
        u, v, t = input().split()
        u = int(u)
        v = int(v)

        if t == 'S':
            edges_s.append((u, v, idx))
        else:
            edges_m.append((u, v, idx))

    if (n - 1) % 2:
        return "-1"

    k = (n - 1) // 2

    dsu = DSU(n)

    for u, v, _ in edges_m:
        dsu.union(u, v)

    min_s = 0

    for u, v, _ in edges_s:
        if dsu.union(u, v):
            min_s += 1

    if min_s > k:
        return "-1"

    dsu = DSU(n)

    max_s = 0

    for u, v, _ in edges_s:
        if dsu.union(u, v):
            max_s += 1

    if max_s < k:
        return "-1"

    dsu = DSU(n)

    useful_s = []

    for u, v, idx in edges_s:
        if dsu.union(u, v):
            useful_s.append((u, v, idx))

    dsu = DSU(n)

    ans = []
    used_s = 0

    for u, v, idx in useful_s:
        if used_s == k:
            break

        if dsu.union(u, v):
            ans.append(idx)
            used_s += 1

    for u, v, idx in edges_s:
        if used_s == k:
            break

        if dsu.union(u, v):
            ans.append(idx)
            used_s += 1

    for u, v, idx in edges_m:
        if dsu.union(u, v):
            ans.append(idx)

    if len(ans) != n - 1:
        return "-1"

    return str(len(ans))

# provided sample
assert run(
"""1 2
1 1 S
1 1 M
"""
) == "0", "sample 1"

# impossible because n-1 is odd
assert run(
"""2 1
1 2 S
"""
) == "-1", "odd tree size"

# disconnected graph
assert run(
"""4 2
1 2 S
3 4 M
"""
) == "-1", "disconnected"

# only one edge type available
assert run(
"""3 2
1 2 S
2 3 S
"""
) == "-1", "cannot balance types"

# simple valid case
assert run(
"""3 3
1 2 S
2 3 M
1 3 S
"""
) == "2", "balanced spanning tree exists"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Nút đơn có vòng lặp | 0 | Vòng lặp tự được bỏ qua một cách chính xác | 
|`n - 1`lẻ | -1 | Không thể chia đều | 
| Đồ thị bị ngắt kết nối | -1 | Yêu cầu kết nối được thực thi | 
| Chỉ có một loại cạnh | -1 | Kiểm tra khoảng thời gian khả thi | 
| Đồ thị cân bằng nhỏ | 2 | Xây dựng thành công | 

## Vỏ cạnh 

Hãy xem xét đồ thị bị ngắt kết nối:```
4 2
1 2 S
3 4 M
```Tối thiểu`"S"`tính toán bắt đầu bằng cách thêm`"M"`cạnh, sản xuất linh kiện`{1}`,`{2}`,`{3,4}`. các`"S"`sáp nhập cạnh`{1}`Và`{2}`, để lại hai thành phần được kết nối tổng thể. Vì đồ thị cuối cùng không bao giờ có thể được kết nối nên giai đoạn xây dựng kết thúc với ít hơn`n - 1`các cạnh và in chính xác`-1`. 

Bây giờ hãy xem xét đồ thị chỉ có`"S"`các cạnh:```
3 2
1 2 S
2 3 S
```Đây`k = 1`. Số lượng tối đa có thể của`"S"`cạnh là`2`, nhưng số lượng tối thiểu có thể có của`"S"`các cạnh cũng vậy`2`bởi vì không có`"M"`các cạnh ở tất cả. Từ`min_s > k`, thuật toán sẽ ngay lập tức từ chối thể hiện đó. 

Cuối cùng, hãy xem xét các cạnh song song:```
2 3
1 2 S
1 2 S
1 2 M
```DSU chỉ chấp nhận cạnh đầu tiên hợp nhất hai đỉnh. Mọi cạnh song song sau đó đều bị từ chối vì cả hai điểm cuối đều thuộc về cùng một thành phần. Điều này đảm bảo câu trả lời cuối cùng vẫn không có tính tuần hoàn.
