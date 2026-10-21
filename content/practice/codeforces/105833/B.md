---
title: "CF 105833B - Ánh sáng của đôi cánh"
description: "Chúng ta có hai cây khác nhau trên cùng một tập hợp N đỉnh. Cây đầu tiên là cấu trúc hiện tại và cây thứ hai là cấu trúc cuối cùng mong muốn. Một thao tác cắt một cạnh hiện có và sau đó thêm một cạnh khác để đồ thị vẫn là một cây."
date: "2026-06-26T09:32:41+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105833
codeforces_index: "B"
codeforces_contest_name: "NUS CS3233 Final Team Contest 2025"
rating: 0
weight: 105833
solve_time_s: 44
verified: true
draft: false
---

[CF 105833B - BrIllIance of Wings](https://codeforces.com/problemset/problem/105833/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có hai cây khác nhau trên cùng một bộ`N`đỉnh. Cây đầu tiên là cấu trúc hiện tại và cây thứ hai là cấu trúc cuối cùng mong muốn. Một thao tác cắt một cạnh hiện có và sau đó thêm một cạnh khác để đồ thị vẫn là một cây. 

Mục tiêu không chỉ là tìm ra số lượng thao tác tối thiểu mà còn đưa ra một chuỗi các thao tác thực tế đạt đến cây mục tiêu. 

Số lượng quan trọng là số cạnh được chia sẻ bởi cả hai cây. Mọi cạnh được chia sẻ đều đã chính xác và không bao giờ cần phải di chuyển. Mọi cạnh khác trong cây ban đầu phải biến mất và mọi cạnh bị thiếu của cây mục tiêu phải xuất hiện. Vì một thao tác thay thế chính xác một cạnh nên câu trả lời phải là số cạnh không chia sẻ trong cây ban đầu. 

Với`N`lên đến`100000`, bất kỳ giải pháp nào liên tục tìm kiếm trong toàn bộ cây cho mọi thao tác sẽ trở nên quá chậm. Một cách tiếp cận bậc hai sẽ yêu cầu khoảng`10^10`kiểm tra trong trường hợp xấu nhất, điều này là không thể trong giới hạn cuộc thi thông thường. Chúng ta cần một đường gần tuyến tính hoặc`N log N`sự thi công. 

Phần khó khăn không phải là tính toán câu trả lời mà là tạo ra các phép tính hợp lệ theo thứ tự trong đó mọi biểu đồ trung gian vẫn là một cây. 

Trường hợp cạnh nhỏ là khi cả hai cây đều giống nhau. 

Ví dụ:```
Input
3
1 2
2 3
1 2
2 3
```Đầu ra đúng là:```
0
```Một giải pháp bất cẩn luôn cố gắng thực hiện việc thay thế tất cả các cạnh ban đầu sẽ phá hủy các cạnh đúng và tạo ra các thao tác không cần thiết. 

Một trường hợp cạnh khác là khi cây đầu tiên chứa một cạnh không có trong cây mục tiêu nhưng việc loại bỏ nó sẽ tách các đỉnh theo cách mà chỉ một cạnh mục tiêu có thể kết nối lại các thành phần. 

Ví dụ:```
Input
4
1 2
2 3
3 4
1 3
3 4
2 4
```Thuật toán phải chọn một cạnh từ cây mục tiêu vượt qua vết cắt được tạo bằng cách loại bỏ cạnh sai. Việc chọn một cạnh mục tiêu bị thiếu tùy ý có thể ngắt kết nối biểu đồ. 

## Phương pháp tiếp cận 

Ý tưởng của Brute Force là liên tục tìm một cạnh khác nhau giữa cây hiện tại và cây mục tiêu, loại bỏ nó và tìm kiếm tất cả các cạnh mục tiêu cho đến khi tìm thấy một cạnh kết nối lại hai thành phần kết quả. Điều này đúng vì bất kỳ cạnh nào vượt qua vết cắt sẽ khôi phục lại cây. Tuy nhiên, nếu chúng ta quét tất cả các cạnh cho mỗi lần thay thế, trường hợp xấu nhất sẽ trở thành`O(N^2)`hoạt động quá chậm đối với`N = 100000`. 

Quan sát hữu ích là chúng ta chỉ cần thêm các cạnh mục tiêu cho đến khi tất cả các cạnh mục tiêu đều có mặt. Mỗi thao tác sẽ tăng số cạnh được chia sẻ lên đúng một. Thách thức trở thành việc tìm ra một cạnh mục tiêu hợp lệ vượt qua đường cắt hiện tại một cách hiệu quả. 

Chúng tôi xử lý cây ban đầu từ lá trở lên. Khi một cạnh ban đầu đã có trong cây mục tiêu, chúng ta giữ nó và hợp nhất hai cạnh. Nếu không, chúng tôi loại bỏ nó và tìm cạnh mục tiêu cắt cùng một vết cắt. Cạnh mục tiêu được đảm bảo tồn tại vì cây mục tiêu được kết nối. 

Để tìm các cạnh như vậy một cách hiệu quả, chúng tôi duy trì các cạnh mục tiêu để lại mọi thành phần hiện được hợp nhất. Các tập hợp được hợp nhất bằng kỹ thuật từ nhỏ đến lớn, do đó mỗi cạnh chỉ thay đổi các tập hợp`O(log N)`lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N2) | O(N) | Quá chậm | 
| Tối ưu | O(N log N) | O(N log N) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ tất cả các cạnh của cây mục tiêu trong một tập hợp để chúng ta có thể nhanh chóng kiểm tra xem một cạnh đã tồn tại trong cấu trúc cuối cùng hay chưa. 
2. Root cây ban đầu một cách tùy ý và thực hiện duyệt theo thứ tự sau. Xử lý phần tử con trước phần tử cha có nghĩa là khi chúng ta xử lý một cạnh đối với phần tử cha, phần con đã được xử lý xong. 
3. Duy trì cấu trúc DSU biểu diễn các thành phần được kết nối bởi các cạnh mục tiêu đã được thêm vào. Đối với mỗi thành phần DSU, hãy lưu trữ các cạnh mục tiêu rời khỏi thành phần đó. 
4. Đối với mỗi cạnh cây gốc`(u, parent[u])`, kiểm tra xem nó đã có trong cây mục tiêu chưa. 
5. Nếu cạnh được chia sẻ, hãy hợp nhất hai thành phần DSU vì kết nối này sẽ tồn tại mãi mãi. 
6. Nếu cạnh không được chia sẻ, hãy loại bỏ nó. Tìm kiếm các cạnh mục tiêu đi ra của thành phần chứa`u`cho đến khi tìm thấy một cạnh có điểm cuối khác thuộc về thành phần khác. Thêm cạnh đó làm cạnh thay thế và hợp nhất hai thành phần DSU. 
7. Tiếp tục cho đến khi tất cả các cạnh ban đầu đã được xử lý. Các hoạt động được ghi lại là trình tự tối thiểu cần thiết. 

Tại sao nó hoạt động: 

Mỗi khi chúng tôi xử lý một cạnh không chia sẻ, chúng tôi sẽ loại bỏ một cạnh sai và thêm một cạnh mục tiêu bị thiếu. Số cạnh được chia sẻ tăng thêm một. Cây mục tiêu luôn chứa một cạnh cắt qua bất kỳ vết cắt nào được tạo bằng cách loại bỏ một cạnh khỏi cây hiện tại, vì nếu không thì cây mục tiêu sẽ bị ngắt kết nối. Vì vậy mọi sự thay thế đều có hiệu lực. Vì mỗi thao tác sửa chính xác một cạnh sai nên số lượng thao tác là tối thiểu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())

    tree = [[] for _ in range(n)]
    edges1 = []

    for _ in range(n - 1):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        edges1.append((u, v))
        tree[u].append(v)
        tree[v].append(u)

    target_edges = []
    target_set = set()

    for _ in range(n - 1):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        target_edges.append((u, v))
        target_set.add((min(u, v), max(u, v)))

    parent = [-1] * n
    order = [0]
    parent[0] = -2

    for u in order:
        for v in tree[u]:
            if parent[v] == -1:
                parent[v] = u
                order.append(v)

    dsu = list(range(n))
    bag = [set() for _ in range(n)]

    for i, (u, v) in enumerate(target_edges):
        bag[u].add(i)
        bag[v].add(i)

    def find(x):
        while dsu[x] != x:
            dsu[x] = dsu[dsu[x]]
            x = dsu[x]
        return x

    def merge(a, b):
        a = find(a)
        b = find(b)
        if a == b:
            return a
        if len(bag[a]) < len(bag[b]):
            a, b = b, a
        dsu[b] = a
        bag[a].update(bag[b])
        bag[b].clear()
        return a

    def get_crossing(comp):
        comp = find(comp)
        s = bag[comp]
        while s:
            eid = next(iter(s))
            a, b = target_edges[eid]
            if find(a) == find(b):
                s.remove(eid)
            else:
                return eid
        return -1

    ans = []

    for u in reversed(order[1:]):
        p = parent[u]
        key = (min(u, p), max(u, p))

        if key in target_set:
            merge(u, p)
        else:
            eid = get_crossing(u)
            a, b = target_edges[eid]
            ans.append((u + 1, p + 1, a + 1, b + 1))
            merge(u, p)
            merge(a, b)

    print(len(ans))
    for a, b, c, d in ans:
        print(a, b, c, d)

if __name__ == "__main__":
    solve()
```Phần đầu tiên của quá trình triển khai xây dựng hai cây và lưu trữ các cạnh mục tiêu ở dạng chuẩn hóa`(min, max)`hình thức. Điều này tránh được những sai sót do một cạnh vô hướng có thể được viết theo hai hướng. 

DSU theo dõi các thành phần được hình thành bởi các cạnh đã được chỉnh sửa chính xác. các`bag`mảng lưu trữ các cạnh mục tiêu có thể kết nối một thành phần với thành phần khác. Quá trình hợp nhất luôn chuyển tập hợp nhỏ hơn thành tập hợp lớn hơn, điều này mang lại`O(N log N)`ràng buộc. 

Thứ tự duyệt bị đảo ngược vì cây ban đầu được xử lý từ lá về gốc. Khi một cạnh bị loại bỏ, thuật toán biết rằng phía con đã được đại diện bởi thành phần DSU hiện tại. 

## Ví dụ đã hoạt động 

Ví dụ 1:```
Input
4
1 2
2 3
3 4
3 1
4 1
2 4
```Việc thực hiện là: 

| Cạnh hiện tại | Được chia sẻ? | Hành động | 
| --- | --- | --- | 
| 3 4 | Có | Hợp nhất các thành phần | 
| 2 3 | Không | Thay thế bằng 2 4 | 
| 1 2 | Không | Thay thế bằng 1 3 | 

Các thao tác được tạo ra là hợp lệ vì mỗi cạnh bị loại bỏ sẽ được thay thế bằng cạnh đích đi qua cùng một vết cắt. Quá trình kết thúc với tất cả các cạnh mục tiêu hiện diện. 

Ví dụ 2:```
Input
2
1 2
1 2
```| Cạnh hiện tại | Được chia sẻ? | Hành động | 
| --- | --- | --- | 
| 1 2 | Có | Hợp nhất các thành phần | 

Câu trả lời là 0 vì các cây đã khớp nhau rồi. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N log N) | Mỗi cạnh mục tiêu chỉ di chuyển theo logarit giữa các bộ DSU nhiều lần | 
| Không gian | O(N) | Cây, mảng DSU và tập cạnh được lưu trữ chứa thông tin tuyến tính | 

Giải pháp phù hợp với`N = 100000`hạn chế vì nó tránh việc quét liên tục toàn bộ cây. Việc hợp nhất từ ​​nhỏ đến lớn giữ cho tổng số chuyển động được giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    oldout = sys.stdout
    sys.stdout = out
    solve()
    sys.stdin = old
    sys.stdout = oldout
    return out.getvalue()

assert run("""2
1 2
1 2
""").split()[0] == "0"

assert run("""4
1 2
2 3
3 4
3 1
4 1
2 4
""").split()[0] == "3"

assert run("""3
1 2
2 3
1 2
2 3
""").split()[0] == "0"

assert run("""5
1 2
1 3
1 4
4 5
2 3
3 4
4 5
1 2
""").split()[0] == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hai đỉnh giống hệt nhau | 0 | Xử lý cây đã được giải quyết | 
| Chuyển đổi mẫu | 3 | Kiểm tra công trình thay thế | 
| Cây lớn giống hệt nhau | 0 | Ngăn chặn các hoạt động không cần thiết | 
| Một số cạnh sai | 2 | Kiểm tra việc hợp nhất nhiều thành phần | 

## Vỏ cạnh 

Khi các cây giống hệt nhau, mọi cạnh ban đầu được nhận dạng là được chia sẻ. DSU chỉ đơn giản là hợp nhất các thành phần cho đến khi toàn bộ cây là một thành phần và không có hoạt động nào được tạo ra. 

Khi mỗi cạnh đều khác nhau thì mọi cạnh được xử lý đều cần được thay thế. Thuật toán vẫn hoạt động vì mỗi lần loại bỏ sẽ tạo ra một vết cắt và cây mục tiêu phải chứa một cạnh cắt qua vết cắt đó. Tìm kiếm DSU tìm thấy chính xác lợi thế đó và duy trì kết nối sau mỗi hoạt động. 

Để cây có hình dây chuyền thì việc xử lý lá trước tiên là điều cần thiết. Loại bỏ cạnh bên trong quá sớm mà không tôn trọng cấu trúc cây có thể khiến việc tìm thông tin thành phần chính xác trở nên khó khăn hơn. Việc duyệt theo thứ tự sau đảm bảo rằng thông tin thành phần đại diện cho phần đã được cố định của cây.
