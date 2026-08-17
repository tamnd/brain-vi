---
title: "CF 102279B - Bắt đầu cho một nút"
description: "Chúng ta có một cây có tới 200.000 đỉnh và một đỉnh chưa biết chứa bí mật ẩn giấu. Chương trình có thể giao tiếp với người tương tác bằng hai truy vấn. Truy vấn loại 1 yêu cầu khoảng cách từ đỉnh được chọn đến đỉnh ẩn."
date: "2026-08-16T19:10:56+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102279
codeforces_index: "B"
codeforces_contest_name: "HCW 19 Team Round (ICPC format)"
rating: 0
weight: 102279
solve_time_s: 129
verified: true
draft: false
---

[CF 102279B - Bắt đầu cho một nút](https://codeforces.com/problemset/problem/102279/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 9s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có tới 200.000 đỉnh và một đỉnh chưa biết chứa bí mật ẩn giấu. Chương trình có thể giao tiếp với người tương tác bằng hai truy vấn. Truy vấn loại 1 yêu cầu khoảng cách từ đỉnh được chọn đến đỉnh ẩn. Truy vấn loại 2 yêu cầu hàng xóm của đỉnh được chọn nằm trên đường dẫn tới đỉnh ẩn. Nếu đỉnh được chọn đã là đỉnh ẩn thì loại 2 trả về 0. Mục tiêu là xác định đỉnh ẩn bằng cách sử dụng tối đa 36 truy vấn. Vấn đề thực sự mang tính tương tác, do đó đầu vào chỉ chứa cây, trong khi câu trả lời cho các truy vấn sẽ đến trong quá trình thực thi. 

Cây có 200.000 đỉnh, vì vậy việc quét tất cả các đỉnh bằng một truy vấn là không thể ngay lập tức dưới giới hạn 36 truy vấn. Mặc dù việc xử lý trước tuyến tính trên cây là hoàn toàn hợp lý nhưng việc yêu cầu một truy vấn trên mỗi đỉnh sẽ yêu cầu 200.000 truy vấn trong trường hợp xấu nhất. Giải pháp dự định phải làm cho tập hợp các đỉnh ẩn có thể co lại về mặt hình học sau mỗi truy vấn. 

Có ba trường hợp đặc biệt dễ xử lý sai. Đầu tiên là cây một đỉnh:```
1
```Đỉnh duy nhất nhất thiết phải là câu trả lời, vì vậy kết quả đầu ra đúng là`1`. Việc triển khai bất cẩn giả định mọi câu trả lời loại 2 đều là hàng xóm hợp lệ sẽ thất bại vì trình tương tác trả về 0 khi truy vấn chính đỉnh ẩn. 

Trường hợp thứ hai là cây hai đỉnh:```
2
1 2
```Nếu đỉnh ẩn là 2, truy vấn đỉnh 1 với loại 2 trả về 2. Sau đó, thuật toán phải tiếp tục bên trong thành phần một đỉnh chứa 2 và truy vấn lại nó, nhận được 0. Việc dừng sau phản hồi khác 0 đầu tiên sẽ tạo ra đỉnh lân cận chứ không phải đỉnh ẩn. 

Trường hợp thứ ba là một ngôi sao:```
5
1 2
1 3
1 4
1 5
```Nếu đỉnh ẩn là 5 thì đỉnh 1 là tâm. Truy vấn loại 2 ở mức 1 ngay lập tức xác định thành phần duy nhất có liên quan, cụ thể là singleton chứa 5. Thuật toán phải cắt trọng tâm khỏi cây còn lại thay vì loại bỏ toàn bộ hàng xóm được trả về hoặc coi phản hồi là câu trả lời cuối cùng. 

Bài xã luận chính thức đưa ra giải pháp LCA, DFS và tìm kiếm nhị phân, đồng thời đề cập đến việc phân tách trọng tâm như một giải pháp thay thế. Cách tiếp cận bên dưới sử dụng trực tiếp ý tưởng trung tâm, điều này làm cho truy vấn bị ràng buộc đặc biệt rõ ràng. 

## Phương pháp tiếp cận 

Một chiến lược đơn giản là đặt truy vấn loại 1 cho mọi đỉnh cho đến khi khoảng cách trả về bằng 0. Điều này đúng vì có đúng một đỉnh có khoảng cách bằng 0 so với đỉnh ẩn. Vấn đề là giới hạn truy vấn. Trong trường hợp xấu nhất, đỉnh ẩn là đỉnh cuối cùng được kiểm tra, yêu cầu 200.000 truy vấn, trong khi bộ tương tác chỉ cho phép 36 truy vấn. 

Cách tiếp cận brute-force hoạt động vì truy vấn khoảng cách cung cấp thông tin đầy đủ về việc liệu đỉnh được kiểm tra có phải là câu trả lời hay không. Nó thất bại vì nó dành một truy vấn để phân biệt một ứng cử viên. Chúng ta cần một truy vấn mà câu trả lời của nó có thể loại bỏ một phần lớn ứng viên cùng một lúc. 

Quan sát quan trọng nhất là định nghĩa về trọng tâm của cây. Mỗi cây có một đỉnh sao cho sau khi loại bỏ đỉnh đó, mọi thành phần liên thông thu được sẽ chứa nhiều nhất một nửa số đỉnh ban đầu. Giả sử tập hợp các đỉnh ẩn có thể có hiện tại tạo thành một thành phần liên thông và chúng ta chọn trọng tâm của nó`c`. 

Truy vấn loại 2 tại`c`đưa ra chính xác cạnh đầu tiên trên đường đi từ`c`đến đỉnh ẩn. Nếu câu trả lời là 0,`c`chính nó bị ẩn đi và chúng ta đã kết thúc. Ngược lại, giả sử câu trả lời là`v`. Vì đỉnh ẩn nằm đâu đó ngoài`v`, nó phải thuộc thành phần chứa`v`sau đó`c`được gỡ bỏ. Chúng ta có thể loại bỏ hoàn toàn mọi thành phần khác. 

Phần quan trọng là thành phần còn lại có số đỉnh nhiều nhất bằng một nửa so với thành phần trước. Chúng tôi lặp lại thao tác tương tự trên thành phần đó. Đây chính xác là mẫu truy vấn đằng sau việc phân tách trọng tâm và nó làm giảm số lượng đỉnh có thể có xuống khoảng hai hệ số sau mỗi truy vấn. Một giải pháp dựa trên centroid cho vấn đề này cũng được mô tả trong tài liệu cuộc thi bên ngoài như một cách tự nhiên để giải quyết nhiệm vụ. 

Đối với 200.000 đỉnh, 18 lần chia đôi là đủ vì`2^18 = 262144`. Do đó, thuật toán sử dụng tối đa 18 truy vấn loại 2, thấp hơn giới hạn 36. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Truy vấn O(n) và công việc cục bộ O(n) | O(n) | Quá chậm | 
| Tìm kiếm trung tâm | O(n log n) công việc cục bộ và truy vấn O(log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng danh sách kề của cây. Chúng tôi sẽ liên tục kiểm tra các thành phần được kết nối thu được bằng cách cắt các centroid đã chọn trước đó, do đó bản thân cây ban đầu không bao giờ cần phải sửa đổi. 
2. Duy trì Boolean`blocked`mảng. Đỉnh bị chặn đại diện cho tâm đã bị xóa khỏi không gian tìm kiếm hiện tại. Thành phần chứa đỉnh ẩn luôn được biểu thị bằng một đỉnh bắt đầu được bỏ chặn. 
3. Bắt đầu từ đỉnh hiện tại, chỉ đi qua các đỉnh không bị chặn và thu thập toàn bộ thành phần hiện tại. Trong quá trình duyệt này, lưu trữ đỉnh cha cho mỗi đỉnh được thăm. Việc truyền tải lặp đi lặp lại được ưu tiên hơn trong Python vì một cây có thể là một đường dẫn có độ dài 200.000, vượt quá giới hạn đệ quy thông thường. 
4. Tính toán kích thước cây con bên trong thành phần được thu thập bằng cách xử lý ngược lại thứ tự duyệt. Đối với mỗi đỉnh, kích thước của thành phần ở phía cha mẹ của nó là`total_size - subtree_size[v]`, trong khi mỗi phần tử con đóng góp kích thước cây con của riêng mình. 
5. Tìm một đỉnh`c`mà mọi thành phần kết quả có kích thước tối đa`total_size / 2`. Một đỉnh như vậy là một trọng tâm. Chúng ta có thể chỉ cần quét tất cả các đỉnh trong thành phần hiện tại và kiểm tra điều kiện này bằng cách sử dụng kích thước cây con. 
6. Hỏi người tương tác`? 2 c`. Nếu câu trả lời bằng 0 thì bản thân trọng tâm là đỉnh ẩn, vì vậy hãy in`! c`và chấm dứt. 
7. Nếu đáp án là một đỉnh`v`, khối`c`và làm`v`đỉnh bắt đầu cho lần lặp tiếp theo. Cạnh từ`c`theo hướng`v`bây giờ đã được cắt giảm một cách hiệu quả. Bởi vì`v`nằm trên đường đi từ`c`đến đỉnh ẩn thì đỉnh ẩn được đảm bảo nằm bên trong thành phần mới này. 
8. Lặp lại cho đến khi truy vấn loại 2 trả về 0. Mỗi lần lặp làm giảm số đỉnh có thể có ít nhất một nửa, do đó cần tối đa 18 truy vấn để`n <= 200000`. 

### Tại sao nó hoạt động 

Duy trì bất biến rằng thành phần được bỏ chặn hiện tại có chứa đỉnh ẩn. Ban đầu toàn bộ cây là thành phần hiện tại nên bất biến là đúng. Khi trọng tâm của nó`c`được truy vấn, một câu trả lời khác không`v`là hàng xóm của`c`trên con đường duy nhất từ`c`đến đỉnh ẩn. Đang xóa`c`do đó để lại đỉnh ẩn trong chính xác thành phần chứa`v`. Thuật toán giữ chính xác thành phần đó nên bất biến vẫn đúng. Nếu truy vấn trả về 0, thì trình tương tác đã thiết lập rằng`c`là đỉnh ẩn nên thuật toán có thể xuất nó một cách an toàn. 

Thuộc tính centroid đảm bảo rằng thành phần được giữ lại chứa tối đa một nửa thành phần trước đó. Bắt đầu từ tối đa 200.000 đỉnh, sau 18 lần giảm như vậy có thể vẫn còn ít hơn một đỉnh, do đó, đỉnh ẩn phải được xác định trước khi vượt quá giới hạn 36 truy vấn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(1_000_000)

def find_centroid(start, graph, blocked, parent, size):
    order = [start]
    parent[start] = 0

    for v in order:
        pv = parent[v]
        for to in graph[v]:
            if blocked[to] or to == pv:
                continue
            parent[to] = v
            order.append(to)

    total = len(order)

    for v in reversed(order):
        size[v] = 1
        pv = parent[v]
        for to in graph[v]:
            if blocked[to] or parent[to] != v:
                continue
            size[v] += size[to]

    for v in order:
        largest = total - size[v]

        for to in graph[v]:
            if blocked[to] or parent[to] != v:
                continue
            if size[to] > largest:
                largest = size[to]

        if largest * 2 <= total:
            return v

    return start

def ask(t, v):
    print("?", t, v, flush=True)
    ans = int(input())
    if ans == -1:
        sys.exit(0)
    return ans

def main():
    n = int(input())

    graph = [[] for _ in range(n + 1)]

    for _ in range(n - 1):
        u, v = map(int, input().split())
        graph[u].append(v)
        graph[v].append(u)

    blocked = [False] * (n + 1)
    parent = [0] * (n + 1)
    size = [0] * (n + 1)

    current = 1
    queries = 0

    while True:
        centroid = find_centroid(
            current,
            graph,
            blocked,
            parent,
            size
        )

        queries += 1
        if queries > 36:
            sys.exit(0)

        nxt = ask(2, centroid)

        if nxt == 0:
            print("!", centroid, flush=True)
            return

        blocked[centroid] = True
        current = nxt

if __name__ == "__main__":
    main()
```Danh sách kề lưu trữ cây ban đầu. Không có cạnh nào bị xóa về mặt vật lý vì làm như vậy sẽ yêu cầu cập nhật một số danh sách kề. Thay vì,`blocked[v]`làm cho trọng tâm được chọn trước đó trở nên ẩn đối với tất cả các lần duyệt thành phần trong tương lai.`find_centroid`đầu tiên đi qua thành phần được kết nối hiện tại. các`order`danh sách chứa các đỉnh của nó theo thứ tự cha trước con. Việc xử lý ngược danh sách này sẽ cho ra mọi kích thước cây con mà không cần đệ quy. Điều này đặc biệt hữu ích cho Python vì một cây hình đường dẫn có thể chứa 200.000 đỉnh lồng nhau. 

Đối với một đỉnh`v`,`size[v]`là kích thước cây con của nó đối với gốc tạm thời`start`. Đang xóa`v`tạo một thành phần có thể có ở phía cha mẹ với kích thước`total - size[v]`, cộng thêm một thành phần cho mỗi đứa trẻ có kích thước`size[child]`. Đỉnh chính xác là trọng tâm khi giá trị lớn nhất trong số này lớn nhất bằng một nửa`total`. 

các`ask`hàm in truy vấn và ngay lập tức xóa đầu ra tiêu chuẩn. Việc xóa là bắt buộc trong một vấn đề tương tác vì trình tương tác không thể trả lời một truy vấn vẫn được lưu vào bộ đệm. Nếu người tương tác quay trở lại`-1`, giao thức cho biết chương trình phải chấm dứt. 

Chỉ cần truy vấn loại 2. Một câu trả lời khác 0 bản thân nó không phải là đỉnh ẩn. Nó là đỉnh đầu tiên sau tâm hiện tại trên đường dẫn đến đỉnh ẩn, vì vậy nó cho chúng ta biết thành phần nào có kích thước bằng một nửa cần giữ lại. 

Về mặt kỹ thuật, bộ đếm truy vấn không cần thiết để đảm bảo tính chính xác vì đối số centroid đã chứng minh rằng có nhiều nhất 18 truy vấn được thực hiện cho kích thước đầu vào tối đa. Việc duy trì nó trong quá trình triển khai sẽ khiến việc vô tình vi phạm giao thức không thành công một cách an toàn. 

Không có sự đệ quy trong tính toán truyền tải thành phần hoặc tính toán centroid. Điều này tránh tràn ngăn xếp Python trên cây có hình đường dẫn. Số nguyên Python cũng không gặp vấn đề tràn đối với kích thước cây con, mặc dù tất cả các kích thước có liên quan dù sao cũng chỉ tối đa là 200.000. 

## Ví dụ đã hoạt động 

Tuyên bố cung cấp một bản ghi tương tác. Cây của nó là```
7
2 1
2 4
3 5
6 2
1 3
2 7
```và bản ghi cho thấy đỉnh ẩn là 3. 

Trọng tâm của cây bảy đỉnh đầy đủ là đỉnh 2. Loại bỏ 2 thành phần lá có kích thước 1, 1, 1 và 3 để mỗi thành phần có kích thước tối đa là 3. 

| Thành phần hiện tại | Kích thước | Trung tâm | Đáp án loại 2 | Thành phần tiếp theo | Kích thước sau khi cắt | 
| --- | --- | --- | --- | --- | --- | 
|`{1,2,3,4,5,6,7}`| 7 | 2 | 1 |`{1,3,5}`| 3 | 
|`{1,3,5}`| 3 | 3 | 0 | Đã hoàn thành | 1 | 

Truy vấn đầu tiên có hiệu quả`? 2 2`, và trình tương tác trả về 1 vì đường đi từ 2 đến đỉnh ẩn 3 bắt đầu bằng cạnh`2 -> 1`. Sau khối 2, thành phần liên quan duy nhất là`{1,3,5}`. Trọng tâm của nó là 3. Truy vấn 3 trả về 0, vì vậy câu trả lời là 3. Điều này thể hiện tính bất biến trung tâm: sau mỗi câu trả lời khác 0, đỉnh ẩn vẫn nằm trong thành phần được chọn. 

Đối với ví dụ thứ hai, hãy xem xét một ngôi sao có tâm là đỉnh 1, với đỉnh ẩn là 5.```
6
1 2
1 3
1 4
1 5
1 6
```Bản thân trung tâm là một trọng tâm vì việc loại bỏ nó sẽ để lại năm thành phần có kích thước một. 

| Thành phần hiện tại | Kích thước | Trung tâm | Đáp án loại 2 | Thành phần tiếp theo | Kích thước sau khi cắt | 
| --- | --- | --- | --- | --- | --- | 
|`{1,2,3,4,5,6}`| 6 | 1 | 5 |`{5}`| 1 | 
|`{5}`| 1 | 5 | 0 | Đã hoàn thành | 1 | 

Truy vấn đầu tiên xác định ngay nhánh chứa đỉnh ẩn. Truy vấn thứ hai được thực hiện trên thành phần đơn lẻ đó và trả về 0. Ví dụ này cho thấy tại sao câu trả lời loại 2 khác 0 nên được hiểu là một hướng đi chứ không phải là câu trả lời cuối cùng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi lần lặp tìm kiếm centroid sẽ quét thành phần hiện tại của nó, có kích thước giảm dần về mặt hình học | 
| Không gian | O(n) | Mỗi danh sách kề và mảng phụ đều yêu cầu không gian tuyến tính | 
| Truy vấn | O(log n) | Mỗi truy vấn sẽ giảm thành phần ứng cử viên xuống tối đa một nửa kích thước trước đó của nó | 

Vì`n = 200000`, có thể có tối đa 18 truy vấn centroid vì`2^18 = 262144`. Quá trình xử lý cây cục bộ thực hiện một chuỗi quét hình học, được giới hạn bởi`O(n log n)`, dễ dàng tương thích với giới hạn 2 giây và 256 MB trong quá trình triển khai được biên dịch và cũng thực tế trong Python với tính năng truyền tải lặp lại. 

## Trường hợp thử nghiệm 

Vì đây là một vấn đề tương tác nên mẫu được xuất bản là bản ghi tương tác chứ không phải là cặp đầu vào/đầu ra xác định thông thường. Một điều bình thường`run(input)`người trợ giúp không thể tái tạo trình tương tác. Khai thác kiểm tra sau đây sử dụng một đỉnh ẩn cố định và mô phỏng các câu trả lời loại 2. Bộ giải ngoại tuyến phản ánh thuật toán trọng tâm đã gửi, trong khi trình mô phỏng bắt nguồn từ cây ở đỉnh ẩn sao cho đỉnh gốc của đỉnh được truy vấn chính xác là phản hồi loại 2 của trình tương tác.```python
import io
import sys

def solve_offline(inp: str, hidden: int):
    data = list(map(int, inp.split()))
    it = iter(data)

    n = next(it)
    graph = [[] for _ in range(n + 1)]

    for _ in range(n - 1):
        u = next(it)
        v = next(it)
        graph[u].append(v)
        graph[v].append(u)

    # Simulate the interactor's type-2 answers.
    parent_hidden = [0] * (n + 1)
    order = [hidden]

    for v in order:
        for to in graph[v]:
            if to == parent_hidden[v]:
                continue
            parent_hidden[to] = v
            order.append(to)

    blocked = [False] * (n + 1)
    parent = [0] * (n + 1)
    size = [0] * (n + 1)

    def find_centroid(start):
        order = [start]
        parent[start] = 0

        for v in order:
            for to in graph[v]:
                if blocked[to] or to == parent[v]:
                    continue
                parent[to] = v
                order.append(to)

        total = len(order)

        for v in reversed(order):
            size[v] = 1
            for to in graph[v]:
                if blocked[to] or parent[to] != v:
                    continue
                size[v] += size[to]

        for v in order:
            largest = total - size[v]

            for to in graph[v]:
                if blocked[to] or parent[to] != v:
                    continue
                largest = max(largest, size[to])

            if largest * 2 <= total:
                return v

        return start

    current = 1
    queries = 0

    while True:
        centroid = find_centroid(current)
        queries += 1

        if centroid == hidden:
            return centroid, queries

        nxt = parent_hidden[centroid]
        blocked[centroid] = True
        current = nxt

# Provided sample tree. The interaction transcript establishes hidden = 3.
sample = """\
7
2 1
2 4
3 5
6 2
1 3
2 7
"""
assert solve_offline(sample, 3) == (3, 2), "sample"

# Minimum-size tree.
case_min = """\
1
"""
assert solve_offline(case_min, 1) == (1, 1), "minimum tree"

# Two vertices, hidden at the second vertex.
case_two = """\
2
1 2
"""
assert solve_offline(case_two, 2) == (2, 2), "two-vertex boundary"

# Star, testing a highly branching tree and an immediate singleton component.
case_star = """\
5
1 2
1 3
1 4
1 5
"""
assert solve_offline(case_star, 5) == (5, 2), "star"

# Maximum-size path, hidden at the final vertex.
n = 200000
case_max = str(n) + "\n" + "\n".join(
    f"{i} {i + 1}" for i in range(1, n)
) + "\n"

answer, queries = solve_offline(case_max, n)
assert answer == n, "maximum-size path answer"
assert queries <= 18, "centroid query bound"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`7`với sáu cạnh mẫu, ẩn`3`|`3`| Cung cấp ví dụ tương tác và giảm trọng tâm bình thường | 
|`1`|`1`| Truy vấn đầu vào tối thiểu và không có phản hồi | 
|`2`có cạnh`1 2`, ẩn giấu`2`|`2`| Thành phần Singleton sau lần cắt đầu tiên | 
| Ngôi sao năm đỉnh có tâm ở`1`, ẩn giấu`5`|`5`| Cây phân nhánh cao và phản ứng định hướng loại 2 | 
| Đường dẫn có 200.000 đỉnh, ẩn`200000`|`200000`| Kích thước tối đa, cây sâu, duyệt lặp và giới hạn truy vấn | 

Thử nghiệm kích thước tối đa có chủ ý sử dụng một đường dẫn vì đây là hình dạng tồi tệ nhất đối với các thuật toán cây đệ quy. Giải pháp đã gửi không bao giờ lặp lại qua đường dẫn và quy trình centroid sẽ giảm độ dài còn lại xuống gần một nửa ở mỗi truy vấn. 

## Vỏ cạnh 

Đối với cây một đỉnh,```
1
```thành phần hiện tại duy nhất có thể là`{1}`, có trọng tâm là 1. Truy vấn loại 2`? 2 1`trả về 0 vì đỉnh 1 là đỉnh ẩn. Chương trình in ngay lập tức`! 1`. Không có nỗ lực diễn giải số 0 là hàng xóm, do đó trường hợp ranh giới được xử lý chính xác. 

Đối với cây hai đỉnh```
2
1 2
```với đỉnh ẩn 2, trọng tâm ban đầu là 1. Truy vấn loại 2 tại 1 trả về 2. Sau đó, đỉnh 1 bị chặn, để lại thành phần đơn lẻ`{2}`. Trọng tâm của nó là 2 và truy vấn loại 2 tiếp theo trả về 0. Thuật toán in 2 sau đúng hai truy vấn. Đây là ví dụ nhỏ nhất cho thấy tại sao phản hồi loại 2 khác 0 có nghĩa là "tiếp tục theo hướng này" thay vì "trả lời đỉnh này". 

Đối với ngôi sao năm đỉnh```
5
1 2
1 3
1 4
1 5
```có đỉnh ẩn 5 thì đỉnh 1 là tâm. Truy vấn nó trả về 5, vì vậy tất cả các lá khác có thể bị loại bỏ ngay lập tức. Thành phần tiếp theo chỉ chứa đỉnh 5, truy vấn của nó trả về 0. Thuật toán sử dụng hai truy vấn và không bao giờ cần thông tin về khoảng cách. 

Đối với đường dẫn có kích thước tối đa, trọng tâm đầu tiên nằm gần giữa. Nếu đỉnh ẩn nằm ở một điểm cuối thì đáp án loại 2 sẽ chọn nửa chứa điểm cuối đó. Trọng tâm tiếp theo lại nằm gần giữa nửa đó và quá trình tương tự vẫn tiếp tục. Do đó, đường dẫn 200.000 đỉnh yêu cầu tối đa 18 truy vấn mặc dù có độ sâu 199.999. Việc truyền tải lặp đi lặp lại là điều ngăn cản độ sâu của đường dẫn gây ra lỗi đệ quy Python. 

Trường hợp cạnh trung tâm trong tất cả các ví dụ này là thời điểm thành phần còn lại có một đỉnh. Trọng tâm nhất thiết phải là đỉnh đó và loại 2 trả về số 0. Quá trình triển khai sẽ kiểm tra phản hồi này trước khi chặn trọng tâm, do đó nó không bao giờ vô tình xóa câu trả lời đúng khỏi không gian tìm kiếm.
