---
title: "CF 920E - Các thành phần được kết nối?"
description: "Đồ thị trong bài toán này không được đưa ra theo cách thông thường. Thay vì liệt kê các cạnh tồn tại, đầu vào liệt kê các cặp đỉnh bị ngắt kết nối rõ ràng. Mỗi cặp đỉnh không xuất hiện trong danh sách này sẽ được coi là có một cạnh giữa chúng."
date: "2026-06-13T02:50:10+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dfs-and-similar", "dsu", "graphs"]
categories: ["algorithms"]
codeforces_contest: 920
codeforces_index: "E"
codeforces_contest_name: "Educational Codeforces Round 37 (Rated for Div. 2)"
rating: 2100
weight: 920
solve_time_s: 176
verified: true
draft: false
---

[CF 920E - Các thành phần được kết nối?](https://codeforces.com/problemset/problem/920/E) 

**Xếp hạng:** 2100 
**Thẻ:** cấu trúc dữ liệu, dfs và tương tự, dsu, đồ thị 
**Thời gian giải:** 2m 56s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Đồ thị trong bài toán này không được đưa ra theo cách thông thường. Thay vì liệt kê các cạnh tồn tại, đầu vào liệt kê các cặp đỉnh bị ngắt kết nối rõ ràng. Mỗi cặp đỉnh không xuất hiện trong danh sách này sẽ được coi là có một cạnh giữa chúng. 

Vì vậy, cấu trúc thực sự là một biểu đồ dày đặc trong đó hầu hết mọi thứ đều được kết nối, ngoại trừ một tập hợp các kết nối bị cấm. Nhiệm vụ là xác định cách biểu đồ ẩn này phân chia thành các thành phần được kết nối và báo cáo cả số lượng thành phần như vậy tồn tại và kích thước của mỗi thành phần. 

Một cách hữu ích để suy nghĩ về điều này là mọi đỉnh đều bắt đầu như có khả năng kết nối với tất cả các đỉnh khác và mỗi cặp bị cấm sẽ loại bỏ chính xác một cạnh khỏi biểu đồ hoàn chỉnh này. Do đó, khả năng kết nối được xác định bằng việc liệu các cạnh bị thiếu này có đủ để tách các đỉnh thành các nhóm hay không. 

Các ràng buộc đủ lớn nên bất kỳ giải pháp nào cố gắng xây dựng biểu đồ đầy đủ một cách rõ ràng đều không thể thực hiện được. Với tối đa 200000 đỉnh, một biểu đồ hoàn chỉnh sẽ có thứ tự n2 cạnh, vượt xa giới hạn bộ nhớ và thời gian. Ngay cả việc lặp lại trên tất cả các cặp cũng sẽ là các phép toán khoảng 4 × 10¹⁰, điều này không thể thực hiện được trong giới hạn 2 giây. Điều này ngay lập tức loại trừ các ma trận kề và BFS/DFS ngây thơ trên một biểu đồ được xây dựng rõ ràng. 

Khó khăn tinh tế là các cạnh bị thiếu rất thưa thớt so với biểu đồ đầy đủ ẩn. Cấu trúc này gần giống với bài toán đồ thị bù hơn là bài toán duyệt đồ thị chuẩn. 

Một vài trường hợp khó khăn rất dễ mắc sai lầm. 

Nếu không có cặp cấm thì đồ thị là đồ thị đầy đủ và do đó có đúng một thành phần có kích thước n. Một cách tiếp cận ngây thơ giả sử các cạnh đầu vào là các cạnh thực tế sẽ coi nó là biểu đồ trống và tạo ra n thành phần có kích thước 1. 

Nếu mọi cặp có thể đều bị cấm (điều này không thể xảy ra do các ràng buộc nhưng hữu ích về mặt khái niệm), thì đồ thị sẽ có n đỉnh cô lập. Một giải pháp chỉ theo dõi các cạnh bị thiếu mà không xử lý cách ly đúng cách có thể thất bại ở đây. 

Một trường hợp tinh tế khác là khi các cạnh bị cấm tạo thành một hình dạng giống như ngôi sao. Ví dụ: nếu tất cả các cặp liên quan đến đỉnh 1 bị thiếu, đỉnh 1 sẽ bị cô lập, trong khi phần còn lại vẫn được kết nối đầy đủ. Một phép duyệt đơn giản không tính đến cấu trúc phần bù có thể hợp nhất hoặc phân chia các nhóm này một cách không chính xác. 

## Phương pháp tiếp cận 

Ý tưởng mạnh mẽ là xây dựng rõ ràng các danh sách kề cho biểu đồ phần bù. Đối với mỗi cặp đỉnh, chúng tôi kiểm tra xem nó có bị cấm hay không. Nếu không, chúng tôi thêm một cạnh. Điều này ngay lập tức trở nên không thể thực hiện được vì việc lặp qua tất cả các cặp là O(n2) và lưu trữ kề cũng là O(n2) trong trường hợp xấu nhất. 

Quan sát quan trọng là chúng ta không bao giờ cần liệt kê rõ ràng các cạnh. Mỗi đỉnh ban đầu được kết nối với tất cả các đỉnh khác, vì vậy lý do duy nhất khiến hai đỉnh không được kết nối là nếu chúng ta có bằng chứng từ danh sách cấm rằng chúng bị ngắt kết nối. Điều này gợi ý việc duy trì, đối với mỗi đỉnh, tập hợp các đỉnh mà nó không được kết nối. 

Khi chúng tôi thực hiện BFS hoặc DFS từ đỉnh v, chúng tôi muốn di chuyển đến tất cả các đỉnh vẫn “có sẵn”, nghĩa là tất cả các đỉnh ngoại trừ những đỉnh bị cấm rõ ràng và ngoại trừ những đỉnh đã được truy cập. Thách thức ở đây là tìm ra các đỉnh sẵn có này một cách hiệu quả mà không cần quét tất cả n ứng cử viên. 

Đây là lúc cấu trúc theo tập hợp hoặc có trật tự trở nên cần thiết. Chúng ta duy trì một tập hợp các đỉnh chưa được thăm. Khi xử lý một đỉnh v, chúng tôi lặp qua tập hợp chưa được thăm, nhưng bỏ qua những tập hợp bị cấm với v. Các lân cận bị cấm của v được lưu trữ trong một tập hợp băm, vì vậy chúng tôi có thể kiểm tra tư cách thành viên một cách hiệu quả. Mỗi khi chúng tôi xác nhận một hàng xóm hợp lệ, chúng tôi sẽ xóa nó khỏi nhóm chưa được truy cập và tiếp tục.

Thủ thuật hiệu quả quan trọng là mỗi đỉnh được loại bỏ khỏi tập hợp chưa được thăm đúng một lần. Mặc dù chúng tôi có thể kiểm tra nó nhiều lần, tổng công việc trên tất cả các bước BFS vẫn tuyến tính theo n cộng m. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (biểu đồ rõ ràng) | O(n²) | O(n²) | Quá chậm | 
| Tối ưu (bộ + BFS trên phần bù) | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng một biểu diễn các cạnh bị cấm và sau đó chạy BFS trên biểu đồ phần bù ẩn. 

1. Lưu trữ tất cả các cặp bị cấm trong cấu trúc dựa trên hàm băm, nhóm chúng theo đỉnh. Điều này cho phép kiểm tra liên tục xem có cạnh nào bị thiếu giữa hai đỉnh hay không. Mục đích là để nhanh chóng kiểm tra tính kề cận trong đồ thị phần bù. 
2. Duy trì một tập hợp tất cả các đỉnh chưa được thăm. Tập hợp này đại diện cho biên giới của các đỉnh chưa được gán cho một thành phần. 
3. Trong khi tập hợp chưa được thăm không trống, hãy chọn một đỉnh v tùy ý từ nó và bắt đầu BFS cho thành phần mới. Kích thước của BFS này sẽ xác định kích thước của một thành phần được kết nối. 
4. Khởi tạo hàng đợi với v và xóa v khỏi tập hợp chưa được thăm. Điều này đảm bảo chúng ta không bao giờ xử lý cùng một đỉnh hai lần. 
5. Khi hàng đợi không trống, hãy lấy đỉnh x. Chúng ta muốn tìm tất cả các đỉnh vẫn chưa được thăm mà được kết nối với x trong đồ thị phần bù. Điều đó có nghĩa là tất cả các đỉnh ngoại trừ chính x và ngoại trừ những đỉnh bị cấm rõ ràng với x. 
6. Lặp lại tập hợp chưa được duyệt và xác định các đỉnh không nằm trong danh sách bị cấm của x. Mỗi đỉnh như vậy đều kề nhau trong biểu đồ phần bù, vì vậy chúng ta thêm nó vào hàng đợi và xóa nó khỏi tập hợp chưa được thăm. 
7. Tiếp tục cho đến khi hết hàng đợi. Tại thời điểm này, chúng tôi đã phát hiện ra thành phần được kết nối đầy đủ của v. 
8. Ghi lại kích thước của thành phần này và lặp lại quá trình với một đỉnh khác chưa được thăm. 

Lý do chính khiến điều này hoạt động hiệu quả là vì mỗi đỉnh được xóa khỏi tập hợp chưa được duyệt đúng một lần và mỗi lần xóa tương ứng với việc duyệt cạnh được xác nhận trong biểu đồ phần bù. 

### Tại sao nó hoạt động 

Tại bất kỳ thời điểm nào, tập hợp chưa được thăm sẽ chứa chính xác các đỉnh chưa được gán cho bất kỳ thành phần nào. Khi xử lý một đỉnh x, mọi đỉnh vẫn còn trong tập chưa được thăm sẽ là một lân cận tiềm năng trong đồ thị phần bù. Ngoại lệ duy nhất là những trường hợp bị cấm rõ ràng với x. 

Khi chúng ta di chuyển một đỉnh y từ đỉnh chưa được thăm vào hàng đợi, chúng ta đang khẳng định rằng không có cạnh bị cấm (x, y), nghĩa là có một cạnh tồn tại trong biểu đồ ẩn. Vì BFS khám phá tất cả các đỉnh có thể tiếp cận theo định nghĩa kề cận này, nên mọi đỉnh có thể tiếp cận thông qua các cạnh cho phép cuối cùng sẽ được phát hiện. Bởi vì chúng ta không bao giờ xem lại các đỉnh nên các thành phần vẫn rời rạc và mỗi đỉnh được gán chính xác một lần. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import defaultdict, deque

def solve():
    n, m = map(int, input().split())
    
    forbidden = defaultdict(set)
    for _ in range(m):
        x, y = map(int, input().split())
        forbidden[x].add(y)
        forbidden[y].add(x)
    
    unvisited = set(range(1, n + 1))
    components = []
    
    while unvisited:
        start = next(iter(unvisited))
        unvisited.remove(start)
        
        q = deque([start])
        size = 0
        
        while q:
            v = q.popleft()
            size += 1
            
            to_remove = []
            for u in unvisited:
                if u not in forbidden[v]:
                    q.append(u)
                    to_remove.append(u)
            
            for u in to_remove:
                unvisited.remove(u)
        
        components.append(size)
    
    components.sort()
    print(len(components))
    print(*components)

if __name__ == "__main__":
    solve()
```Vùng kề bị cấm được lưu dưới dạng từ điển của các tập hợp nên việc kiểm tra tư cách thành viên là O(1). Tập hợp chưa được duyệt cho phép chúng ta tránh phải xem lại các đỉnh và loại bỏ các đỉnh một cách lười biếng sau khi lặp để tránh sửa đổi một tập hợp trong quá trình lặp. 

Vòng lặp BFS mở rộng một thành phần bằng cách quét nhóm chưa được truy cập hiện tại và chỉ chọn những hàng xóm hợp lệ. Mặc dù điều này trông giống như một vòng lặp lồng nhau nhưng mỗi đỉnh được loại bỏ khỏi`unvisited`chính xác một lần, do đó tổng số lần chèn thành công vào các thành phần là tuyến tính theo n. 

Việc sắp xếp ở cuối là bắt buộc vì các thành phần phải được in theo thứ tự không giảm. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 5
1 2
3 4
3 2
4 2
2 5
```Chúng ta bắt đầu với unvisited = {1,2,3,4,5}. 

Chúng tôi chọn 1 là sự khởi đầu đầu tiên. 

| Bước | Hiện tại | Chưa từng ghé thăm trước đây | Kiểm tra bị cấm | Mới được thêm | Kích thước thành phần | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | {1,2,3,4,5} | 2 không được phép? vâng bị cấm? không | 2,3,4,5 trừ bị cấm | 4 | 

Đỉnh 1 được kết nối với tất cả ngoại trừ những điểm bị cấm rõ ràng. Sau BFS, chúng ta nhận được một thành phần có kích thước 4. 

Còn lại chưa được truy cập = {2}. 

Bây giờ bắt đầu lúc 2. 

| Bước | Hiện tại | Chưa từng ghé thăm trước đây | Kiểm tra bị cấm | Mới được thêm | Kích thước thành phần | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | {2} | không | không | 1 | 

Chúng tôi kết thúc bằng các thành phần [4,1], được sắp xếp là [1,4]. 

Điều này xác nhận rằng các cạnh cấm cô lập một đỉnh duy nhất khỏi lõi dày đặc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) khấu hao | mỗi đỉnh bị xóa một lần, mỗi cạnh cấm được lưu trữ một lần | 
| Không gian | O(n + m) | lưu trữ cho tập hợp chưa được truy cập và vùng lân cận bị cấm | 

Cấu trúc tránh liệt kê bậc hai bằng cách không bao giờ xây dựng các cạnh một cách rõ ràng. Mặc dù các bộ quét vòng lặp bên trong, mỗi quá trình chuyển đổi đỉnh từ chưa được thăm sang đã được truy cập đều xảy ra một lần, điều này giữ cho tổng công việc luôn tuyến tính trong thực tế. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    from collections import defaultdict, deque

    n, m = map(int, input().split())
    forbidden = defaultdict(set)

    for _ in range(m):
        x, y = map(int, input().split())
        forbidden[x].add(y)
        forbidden[y].add(x)

    unvisited = set(range(1, n + 1))
    components = []

    while unvisited:
        start = next(iter(unvisited))
        unvisited.remove(start)
        q = deque([start])
        size = 0

        while q:
            v = q.popleft()
            size += 1
            to_remove = []
            for u in unvisited:
                if u not in forbidden[v]:
                    q.append(u)
                    to_remove.append(u)
            for u in to_remove:
                unvisited.remove(u)

        components.append(size)

    components.sort()
    out = str(len(components)) + "\n" + " ".join(map(str, components))
    return out

# provided sample
assert run("""5 5
1 2
3 4
3 2
4 2
2 5
""") == "2\n1 4"

# minimum size
assert run("""1 0
""") == "1\n1"

# fully connected (no forbidden edges)
assert run("""4 0
""") == "1\n4"

# star forbidden structure
assert run("""5 4
1 2
1 3
1 4
1 5
""") == "2\n1 4"

# split into pairs
assert run("""4 2
1 2
3 4
""") == "1\n4"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1 nút | 1 thành phần | trường hợp tối thiểu | 
| không có cạnh cấm | kết nối đầy đủ | hành vi đồ thị hoàn chỉnh | 
| sao cấm | đỉnh cô lập | ngắt kết nối không đối xứng | 
| chia đôi | kết nối đầy đủ thông qua bổ sung | kết nối gián tiếp | 

## Vỏ cạnh 

Khi không có cạnh bị cấm, thuật toán sẽ tạo ngay một thành phần duy nhất chứa tất cả các đỉnh. Tập hợp chưa được truy cập bắt đầu với tất cả các nút và BFS đầu tiên bắt đầu từ bất kỳ nút nào sẽ chấp nhận mọi nút khác vì không có nút nào bị loại trừ bởi danh sách bị cấm. 

Khi một đỉnh bị cấm với tất cả các đỉnh khác, đỉnh đó sẽ bị cô lập trong khi đồ thị còn lại vẫn được kết nối hoàn toàn. BFS bắt đầu từ bất kỳ nút không bị cô lập nào sẽ không bao giờ bao gồm đỉnh bị cô lập vì nó luôn bị loại trừ trong quá trình kiểm tra kề. 

Khi các cạnh bị cấm hình thành các ràng buộc rời rạc giữa các cặp đỉnh, đồ thị phần bù sẽ được kết nối đầy đủ. BFS sẽ hợp nhất mọi thứ thành một thành phần duy nhất vì mọi đỉnh vẫn có thể truy cập được thông qua các cạnh được phép.
