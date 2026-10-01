---
title: "CF 104855D - Những con đường đầy màu sắc"
description: "Chúng ta có một đồ thị có hướng trong đó mỗi cạnh mang một nhãn gọi là màu. Bắt đầu từ nút 1, chúng ta có thể đi dọc theo các cạnh được định hướng bao nhiêu lần tùy thích và chúng ta được phép xem lại các nút và sử dụng lại các cạnh."
date: "2026-06-28T11:01:37+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104855
codeforces_index: "D"
codeforces_contest_name: "TheForces Round #27(3^3-Forces)"
rating: 0
weight: 104855
solve_time_s: 84
verified: false
draft: false
---

[CF 104855D - Những con đường đầy màu sắc](https://codeforces.com/problemset/problem/104855/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 24s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị có hướng trong đó mỗi cạnh mang một nhãn gọi là màu. Bắt đầu từ nút 1, chúng ta có thể đi dọc theo các cạnh được định hướng bao nhiêu lần tùy thích và chúng ta được phép xem lại các nút và sử dụng lại các cạnh. Hạn chế duy nhất là khi đi qua một đường đi, chúng ta không được phép lấy hai cạnh liên tiếp cùng màu. Nhiệm vụ của chúng ta là xác định xem nút nào có thể truy cập được từ nút 1 theo ràng buộc này. 

Vì vậy, bản thân biểu đồ là tĩnh, nhưng tính hợp lệ của bước đi phụ thuộc vào lịch sử của cạnh cuối cùng được sử dụng. Điều này có nghĩa là khả năng tiếp cận không chỉ là thuộc tính của các nút mà còn là thuộc tính của cặp bao gồm một nút và màu cạnh cuối cùng được sử dụng để đến đó. 

Các ràng buộc rất lớn: tổng số nút và cạnh trong tất cả các trường hợp thử nghiệm có thể lên tới một triệu. Điều này ngay lập tức loại trừ bất kỳ giải pháp nào cố gắng mô phỏng đường dẫn một cách rõ ràng hoặc khám phá chuỗi các cạnh mà không cần ghi nhớ. Bất kỳ DFS hoặc BFS ngây thơ nào trên các đường dẫn sẽ phát nổ vì các đường dẫn có thể dài tùy ý do chu kỳ và cùng một nút có thể được xem lại trong các bối cảnh màu cuối cùng khác nhau. 

Trường hợp cạnh tinh tế xuất hiện khi nhiều cạnh có cùng màu tạo thành chu kỳ. Ví dụ: nếu nút 1 có cấu trúc tự lặp như 1 -> 2 (đỏ), 2 -> 3 (đỏ), 3 -> 1 (đỏ), thì mặc dù tất cả các nút đều có thể truy cập được về mặt cấu trúc theo nghĩa biểu đồ thông thường, nhưng chỉ có nút 2 là có thể truy cập được theo ràng buộc đầy màu sắc, vì sau khi lấy một cạnh màu đỏ, không thể sử dụng cạnh màu đỏ thứ hai ngay lập tức. 

Một trường hợp góc khác phát sinh khi một nút có thể truy cập được thông qua hai màu khác nhau, nhưng chỉ một trong số chúng cho phép tiến triển thêm. Ví dụ: nếu việc tiếp cận một nút thông qua cạnh màu đỏ sẽ chặn tất cả các cạnh đi ra vì chúng cũng có màu đỏ, trong khi việc tiếp cận nút đó qua cạnh màu xanh lam cho phép tiếp tục, thì cách tiếp cận đã truy cập [nút] ngây thơ sẽ hợp nhất các trạng thái này một cách không chính xác và đánh giá quá cao hoặc đánh giá thấp khả năng tiếp cận. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ cố gắng khám phá tất cả các bước đi có thể bắt đầu từ nút 1 trong khi theo dõi màu cạnh được sử dụng cuối cùng. Chúng ta có thể mô hình hóa điều này dưới dạng DFS hoặc BFS theo các trạng thái (nút, Last_color). Từ mỗi trạng thái, chúng tôi thử tất cả các cạnh đi ra có màu khác với Last_color. 

Cách tiếp cận này đúng vì nó thực thi ràng buộc một cách trực tiếp, nhưng nó gây ra sự bùng nổ cơ bản trong không gian trạng thái. Trong trường hợp xấu nhất, mỗi nút có thể có nhiều màu cuối cùng khác nhau và mỗi lần chuyển đổi cạnh sẽ tạo ra một trạng thái khác. Nếu chúng ta ký hiệu m là số cạnh thì số màu có thể có cũng có thể lên tới m, do đó đồ thị trạng thái có thể đạt tới O(nm) trong các trường hợp bệnh lý. Giá trị này quá lớn so với tổng kích thước đầu vào là 10^6. 

Quan sát quan trọng là mặc dù màu cuối cùng quan trọng đối với tính chính xác, nhưng chúng ta chỉ cần biết liệu một nút có thể truy cập được dưới bất kỳ màu cuối cùng hợp lệ nào hay không, chứ không phải tất cả các màu cuối cùng có thể có. Điều này cho thấy BFS trên các trạng thái vẫn khả thi nếu chúng ta có thể biểu diễn các chuyển đổi một cách hiệu quả và tránh xem lại các tình huống tương đương. 

Do đó, chúng tôi mở rộng biểu đồ thành biểu đồ trạng thái phân lớp trong đó mỗi trạng thái là (nút, Last_color), nhưng chúng tôi tránh lưu trữ rõ ràng tất cả các màu trên mỗi nút bằng cách lưu trữ ngầm các chuyển tiếp đã truy cập. Mỗi cạnh có hướng sẽ trở thành một chuyển tiếp từ (u, c_prev) sang (v, c_i) bất cứ khi nào c_prev != c_i. Bí quyết là chạy BFS bắt đầu từ (1, 0) trong đó 0 đại diện cho “không có màu trước đó” và đánh dấu các trạng thái là đã truy cập để tránh lặp lại. 

Bởi vì mỗi trạng thái được xử lý nhiều nhất một lần và mỗi cạnh tạo ra nhiều nhất một số lượng chuyển đổi không đổi giữa các trạng thái, nên tổng độ phức tạp trở thành tuyến tính theo số lượng chuyển đổi trạng thái hợp lệ thực sự được phát hiện, được giới hạn bởi O(n + m).

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (đường dẫn DFS qua màu sắc) | Hàm mũ trong trường hợp xấu nhất | O(nm) | Quá chậm | 
| Trạng thái BFS (nút, màu cuối cùng) | O(n + m) khấu hao | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi trình bày lại vấn đề dưới dạng tìm kiếm trong một không gian trạng thái mở rộng trong đó mỗi trạng thái ghi nhớ màu cạnh cuối cùng được sử dụng. 

1. Chúng ta bắt đầu từ trạng thái (1, 0), trong đó 0 biểu thị rằng chưa có cạnh nào được lấy. Điều này cho phép sử dụng bất kỳ cạnh đi nào từ nút 1. Việc khởi tạo này là cần thiết vì bước đầu tiên không có hạn chế về màu sắc. 
2. Chúng tôi xây dựng một danh sách kề trong đó mỗi mục lưu trữ các cặp (hàng xóm, màu sắc). Điều này cho phép chúng tôi liệt kê một cách hiệu quả tất cả các chuyển đổi đi từ một nút. 
3. Chúng tôi chạy BFS bằng cách sử dụng hàng trạng thái (nút, Last_color). Mỗi lần chúng ta loại bỏ một trạng thái (u, c_prev), chúng ta sẽ khám phá tất cả các cạnh đi ra (u -> v, c_i). Nếu c_i khác với c_prev thì quá trình chuyển đổi hợp lệ. 
4. Với mỗi lần chuyển đổi hợp lệ, chúng ta chuyển sang trạng thái (v, c_i). Nếu trạng thái này chưa được truy cập trước đó, chúng tôi đánh dấu nó đã truy cập và đẩy nó vào hàng đợi. Điều này đảm bảo chúng tôi không xử lý lại các tình huống giống hệt nhau, ngăn chặn sự bùng nổ theo cấp số nhân. 
5. Chúng tôi duy trì một mảng riêng biệt có thể truy cập [nút] để ghi lại xem một nút đã từng được truy cập ở bất kỳ trạng thái nào hay chưa. Mỗi khi chúng tôi đạt đến trạng thái mới (v, c_i), chúng tôi đánh dấu có thể truy cập[v] là đúng. 
6. Sau khi BFS hoàn tất, chúng tôi xuất ra tất cả các nút có thể truy cập được [nút] là đúng. 

Tính chính xác xoay quanh việc khám phá tất cả các bối cảnh màu cuối cùng hợp lệ có thể có chính xác một lần, đảm bảo rằng không bỏ sót bước đi hợp lệ nào đồng thời ngăn chặn số lần truy cập lại vô hạn do chu kỳ gây ra. 

### Tại sao nó hoạt động 

Bất biến cốt lõi là bất cứ khi nào một trạng thái (u, c_prev) được xử lý, BFS đã phát hiện ra tất cả các đường dẫn đầy màu sắc hợp lệ kết thúc tại u với màu cạnh cuối cùng là c_prev. Bất kỳ phần mở rộng nào của đường dẫn hợp lệ đều phải đến từ trạng thái như vậy và mọi phần mở rộng hợp lệ đều tương ứng chính xác với một cạnh đi ra có màu khác. Vì mỗi trạng thái được truy cập nhiều nhất một lần và tất cả các tiện ích mở rộng hợp lệ đều được khám phá nên không có nút nào có thể truy cập bị bỏ sót. Ngược lại, mọi trạng thái được truy cập đều tương ứng với một đường dẫn đầy màu sắc hợp lệ theo cách xây dựng, do đó không có nút không chính xác nào được đánh dấu là có thể truy cập được. 

## Giải pháp Python```python
import sys
from collections import deque

input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())

        g = [[] for _ in range(n + 1)]
        for _ in range(m):
            u, v, c = map(int, input().split())
            g[u].append((v, c))

        # visited states: (node, last_color)
        # since colors are up to m, we store dict per node
        visited = [set() for _ in range(n + 1)]

        q = deque()
        q.append((1, 0))
        visited[1].add(0)

        reachable = [False] * (n + 1)
        reachable[1] = True

        while q:
            u, last_c = q.popleft()

            for v, c in g[u]:
                if c == last_c:
                    continue
                if c in visited[v]:
                    continue
                visited[v].add(c)
                reachable[v] = True
                q.append((v, c))

        res = [str(i) for i in range(1, n + 1) if reachable[i]]
        print(" ".join(res))

if __name__ == "__main__":
    solve()
```Việc triển khai phản ánh chính xác trạng thái BFS. Danh sách kề lưu trữ cả điểm cuối và màu cạnh để có thể kiểm tra quá trình chuyển đổi trong thời gian không đổi trên mỗi cạnh. Cấu trúc đã truy cập được duy trì trên mỗi nút, được khóa theo màu được sử dụng lần cuối, điều này ngăn việc truy cập lại các cặp trạng thái giống hệt nhau. 

Một chi tiết quan trọng là khởi tạo BFS với màu 0. Trọng điểm này đảm bảo rằng cạnh đầu tiên luôn có thể được lấy bất kể màu của nó là gì. Một sự tinh tế khác là đánh dấu có thể truy cập[v] tại thời điểm chúng tôi khám phá một trạng thái mới, không phải khi chúng tôi loại bỏ nó, điều này tránh bị thiếu các nút được tiếp cận lần đầu ở trạng thái mới được phát hiện. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Đồ thị đầu vào: 1 -> 2 (đỏ), 2 -> 3 (xanh dương), 1 -> 3 (đỏ) 

Chúng tôi mô phỏng BFS qua các trạng thái. 

| Bước | Trạng thái (nút, màu) | Chuyển tiếp | Bang Mới | 
| --- | --- | --- | --- | 
| 1 | (1, 0) | từ 1 lấy đỏ xuống 2, đỏ xuống 3 | (2, đỏ), (3, đỏ) | 
| 2 | (2, đỏ) | đỏ -> cạnh xanh cho phép 3 | (3, màu xanh) | 
| 3 | (3, đỏ) | không có phần mở rộng hợp lệ gửi đi | không | 
| 4 | (3, màu xanh) | không có cạnh đi ra | không | 

Tất cả các nút 1, 2, 3 đều có thể truy cập được. 

Điều này cho thấy rằng có thể tiếp cận cùng một nút (3) trong nhiều ngữ cảnh màu và cả hai đều phải được theo dõi. 

### Ví dụ 2 

Đồ thị đầu vào: 1 -> 2 (đỏ), 2 -> 3 (đỏ), 3 -> 4 (đỏ), 2 -> 4 (xanh dương) 

| Bước | Tiểu bang | Chuyển tiếp | Bang Mới | 
| --- | --- | --- | --- | 
| 1 | (1, 0) | 1 -> 2 (đỏ) | (2, đỏ) | 
| 2 | (2, đỏ) | không dùng được cạnh đỏ đến 3, có thể dùng cạnh xanh đến 4 | (4, màu xanh) | 
| 3 | (4, màu xanh) | thiết bị đầu cuối | không | 

Nút 3 không bao giờ đạt tới được vì tất cả các đường dẫn đến nút đó đều tạo ra các cạnh màu đỏ liên tiếp. Nút 4 có thể truy cập được thông qua thay đổi màu sắc. 

Điều này chứng tỏ rằng khả năng tiếp cận phụ thuộc vào sự chuyển đổi màu sắc, không chỉ khả năng kết nối biểu đồ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) khấu hao | Mỗi trạng thái (nút, Last_color) được truy cập một lần và mỗi cạnh được xử lý tối đa một lần cho mỗi lần chuyển đổi màu hợp lệ | 
| Không gian | O(n + m) | danh sách lân cận cộng với theo dõi trạng thái đã truy cập trên mỗi nút | 

Tổng kích thước đầu vào trên các trường hợp thử nghiệm được giới hạn bởi một triệu nút và cạnh và mỗi lần chuyển đổi trạng thái là thời gian không đổi. Điều này đảm bảo giải pháp chạy thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io
from collections import deque

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output

    input = sys.stdin.readline

    def solve():
        t = int(input())
        for _ in range(t):
            n, m = map(int, input().split())
            g = [[] for _ in range(n + 1)]
            for _ in range(m):
                u, v, c = map(int, input().split())
                g[u].append((v, c))

            visited = [set() for _ in range(n + 1)]
            q = deque()
            q.append((1, 0))
            visited[1].add(0)

            reachable = [False] * (n + 1)
            reachable[1] = True

            while q:
                u, last_c = q.popleft()
                for v, c in g[u]:
                    if c == last_c:
                        continue
                    if c in visited[v]:
                        continue
                    visited[v].add(c)
                    reachable[v] = True
                    q.append((v, c))

            res = [str(i) for i in range(1, n + 1) if reachable[i]]
            print(" ".join(res))

    solve()
    sys.stdout = sys.__stdout__
    return output.getvalue().strip()

# provided samples (as given format)
assert run("""1
5 6
1 2 1
2 3 1
2 4 2
4 2 3
3 4 3
3 5 1
""") == "1 2 3 4"

# minimal case
assert run("""1
1 0
""") == "1"

# no outgoing edges
assert run("""1
3 0
""") == "1"

# all edges same color blocking chains
assert run("""1
4 3
1 2 1
2 3 1
3 4 1
""") == "1 2"

# alternating colors allowing full reach
assert run("""1
4 4
1 2 1
2 3 2
3 4 1
4 1 2
""") == "1 2 3 4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 1 | khả năng tiếp cận tầm thường | 
| dây chuyền cùng màu | 1 2 | lan truyền khối hạn chế màu sắc | 
| chu kỳ xen kẽ | 1 2 3 4 | chu kỳ luân phiên hợp lệ | 

## Vỏ cạnh 

Một trường hợp tinh tế xảy ra khi một nút có thể truy cập được thông qua nhiều màu đến, nhưng chỉ một màu cho phép truyền tải thêm. Hãy xem xét nút 2 đạt được thông qua màu đỏ từ nút 1 và qua màu xanh lam từ nút 1. Nếu tất cả các cạnh đi từ 2 đều có màu đỏ thì chỉ trạng thái (2, xanh lam) có thể tiếp tục, trong khi (2, đỏ) là ngõ cụt. Thuật toán phân tách chính xác các trạng thái này vì lượt truy cập được theo dõi trên mỗi nút và màu sắc, do đó cả hai trạng thái đều được khám phá độc lập. 

Một trường hợp khác là khi một nút lần đầu tiên được phát hiện ở trạng thái màu “chết”. Mặc dù khám phá đầu tiên đó không dẫn đến đâu, có thể truy cập [nút] vẫn được đặt thành đúng và sau đó trạng thái màu hiệu quả cũng có thể đạt đến trạng thái đó. Điều này đảm bảo tính chính xác vì khả năng tiếp cận cấp nút không phụ thuộc vào việc một trạng thái cụ thể có thể tiếp tục hay không.
