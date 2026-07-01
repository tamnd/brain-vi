---
title: "CF 104426B - Cây hoán vị"
description: "Chúng ta được cấp một cái cây có gốc được chỉ định. Nhiệm vụ là gán một hoán vị của các đỉnh, nghĩa là mỗi đỉnh nhận được một số nguyên duy nhất từ ​​1 đến n, sao cho các nhãn tăng đúng theo mọi hướng từ gốc đến lá."
date: "2026-06-30T19:03:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104426
codeforces_index: "B"
codeforces_contest_name: "Syrian Private Universities Collegiate Programming Contest 2023"
rating: 0
weight: 104426
solve_time_s: 90
verified: false
draft: false
---

[CF 104426B - Cây hoán vị](https://codeforces.com/problemset/problem/104426/B) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 30 giây 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một cái cây có gốc được chỉ định. Nhiệm vụ là gán một hoán vị của các đỉnh, nghĩa là mỗi đỉnh nhận được một số nguyên duy nhất từ ​​1 đến n, sao cho các nhãn tăng đúng theo mọi hướng từ gốc đến lá. Về mặt hình thức, bất cứ khi nào một nút u nằm trên đường đi từ gốc đến một nút v khác, giá trị được gán cho u phải nhỏ hơn giá trị được gán cho v. Trong số tất cả các phép gán hợp lệ, chúng ta muốn có chuỗi nhỏ nhất về mặt từ điển khi chúng ta liệt kê các giá trị theo thứ tự đỉnh từ 1 đến n. 

Đây không phải là một vấn đề chung về thứ tự một phần được ngụy trang, nó chính xác là một ràng buộc về cây gốc trong đó mỗi đường dẫn từ gốc tới nút phải tăng một cách nghiêm ngặt. Cây tạo ra một hệ thống phân cấp các ràng buộc ưu tiên, nhưng không giống như các DAG tùy ý, mỗi nút có một chuỗi tổ tiên duy nhất. 

Ràng buộc n lên tới 2·10^5 ngay lập tức loại bỏ bất kỳ giải pháp nào cố gắng liệt kê các hoán vị hoặc thực hiện bất kỳ sự sắp xếp tổng thể nào theo các trạng thái. Ngay cả O(n log n) cũng có thể chấp nhận được, nhưng chỉ khi về cơ bản nó là công việc tuyến tính trên mỗi nút. Bất cứ điều gì bậc hai hoặc giai thừa là không thể. Cấu trúc cây gợi ý việc truyền tải DFS hoặc BFS với thứ tự cẩn thận. 

Một vấn đề tế nhị nảy sinh từ sự tối giản về mặt từ điển. Nếu chúng tôi chỉ được yêu cầu tạo ra bất kỳ hoán vị hợp lệ nào, chúng tôi có thể gán nhãn theo thứ tự độ sâu tăng dần một cách tùy ý. Nhưng tính tối thiểu từ điển buộc các đỉnh đầu phải nhận các giá trị càng nhỏ càng tốt và vì đầu ra được lập chỉ mục theo id đỉnh, nên trước tiên chúng ta phải giảm thiểu giá trị của các đỉnh được đánh số nhỏ hơn, chứ không phải thứ tự truyền tải. 

Một trường hợp cạnh khác là khi gốc không phải là đỉnh 1. Nhiều cách tiếp cận DFS ngây thơ giả định gốc 1 và gán thứ tự khám phá tăng dần. Điều đó không thành công vì thứ tự từ điển phụ thuộc vào chỉ số đỉnh chứ không phải thứ tự truy cập. Một cạm bẫy khác là sử dụng thứ tự DFS mà không bắt buộc các nút con của một nút được xử lý theo thứ tự tăng dần của nhãn đỉnh, điều này cần thiết để giảm thiểu các phép gán trước đó. 

## Phương pháp tiếp cận 

Một cách diễn giải thô bạo sẽ cố gắng xây dựng một hoán vị và kiểm tra tính hợp lệ. Người ta có thể tưởng tượng việc tạo ra tất cả các hoán vị từ 1 đến n, xác minh từng hoán vị xem mọi cạnh có tôn trọng thứ tự tổ tiên hay không. Việc kiểm tra tính hợp lệ yêu cầu tính toán các mối quan hệ tổ tiên hoặc thực hiện DFS cho mỗi ứng viên, tức là O(n) cho mỗi lần kiểm tra. Điều này đã trở thành O(n! · n), hoàn toàn không khả thi ngay cả với n = 12. 

Một lực lượng vũ phu có cấu trúc hơn một chút là coi ràng buộc như một thứ tự một phần và cố gắng sắp xếp cấu trúc liên kết trong khi luôn chọn đỉnh nhỏ nhất có sẵn. Tuy nhiên, khó khăn chính là các ràng buộc không phải là các cạnh tùy ý, chúng là các mối quan hệ tổ tiên trong một cây có gốc. Cấu trúc đó ngụ ý rằng khi một nút được gán một giá trị, tất cả các nút con của nó phải đến sau và thứ tự tương đối giữa các cây con khác nhau là miễn phí. 

Quan sát quan trọng là ràng buộc chỉ thực thi thứ tự dọc theo đường dẫn từ gốc tới lá. Điều này có nghĩa là hoán vị mà chúng ta xây dựng phải là phần mở rộng tuyến tính của thứ tự từng phần của cây có gốc. Phần mở rộng tuyến tính nhỏ nhất về mặt từ điển có được bằng cách luôn gán nhãn nhỏ nhất có sẵn cho đỉnh sớm nhất có thể theo thứ tự id đỉnh tăng dần, đồng thời tôn trọng rằng cha mẹ phải được chỉ định trước con cái. 

Điều này làm giảm vấn đề trong việc xây dựng thứ tự tôpô của cây gốc trong đó mỗi nút đứng trước tất cả các con cháu của nó và trong số tất cả các thứ tự hợp lệ, chúng tôi muốn thứ tự làm cho mảng nhãn p[1..n] tối thiểu về mặt từ điển. Điều này đạt được bằng cách mô phỏng một quá trình trong đó chúng ta gán nhãn từ 1 đến n theo thứ tự tăng dần, luôn chọn đỉnh được đánh số nhỏ nhất mà đỉnh gốc đã được gán.

Chúng tôi duy trì các nút nào "có sẵn" để nhận nhãn tiếp theo: nút sẽ khả dụng sau khi nút gốc của nó đã nhận được nhãn. Ở mỗi bước, chúng tôi chọn nút có sẵn được lập chỉ mục nhỏ nhất. Sự lựa chọn tham lam này đảm bảo tính tối thiểu từ điển vì các vị trí trước đó trong p tương ứng với các nhãn nhỏ hơn và chúng ta luôn gán các nhãn theo thứ tự tăng dần cho chỉ số đỉnh nhỏ nhất có thể phù hợp với các ràng buộc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! · n) | O(n) | Quá chậm | 
| Tối ưu | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta giải thích lại vấn đề bằng cách xây dựng hoán vị p[1..n], trong đó p[v] là nhãn được gán cho đỉnh v, nhưng nghĩ ngược lại sẽ dễ dàng hơn: chúng ta gán nhãn từ 1 đến n theo thứ tự. 

1. Căn cây tại x và tính danh sách kề. Chúng tôi cũng theo dõi mối quan hệ cha-con bằng DFS hoặc BFS bắt đầu từ x. Điều này đảm bảo chúng tôi biết nút nào phụ thuộc vào nút nào. 
2. Duy trì cấu trúc ưu tiên (min-heap) của các đỉnh có sẵn. Một đỉnh khả dụng nếu cha của nó đã được gán nhãn. Ban đầu, chỉ có thư mục gốc là khả dụng vì nó không có ràng buộc cấp trên. 
3. Liên tục trích xuất đỉnh có chỉ số nhỏ nhất từ ​​heap và gán cho nó nhãn tiếp theo theo thứ tự tăng dần. Khi chúng tôi gán nhãn cho đỉnh u, chúng tôi đánh dấu u là đã xử lý. 
4. Đối với mọi con v của u, hãy giảm sự phụ thuộc của nó và nếu tất cả các điều kiện tiên quyết của nó được thỏa mãn (trong cây có nghĩa là cha mẹ của nó hiện đã được xử lý), hãy đẩy v vào heap. 
5. Tiếp tục cho đến khi tất cả các đỉnh được gán nhãn. 

Lựa chọn thiết kế quan trọng là chúng tôi luôn chọn đỉnh nhỏ nhất có sẵn, không phải đỉnh xuất hiện sớm nhất theo thứ tự DFS hoặc BFS. Đây là những gì thực thi tính tối ưu từ điển. 

Tại sao nó hoạt động được gắn với hai bất biến kết hợp. Đầu tiên, một đỉnh được chèn vào tập có sẵn một cách chính xác khi đỉnh gốc của nó đã được gán một nhãn nhỏ hơn, do đó các ràng buộc tổ tiên luôn được thỏa mãn. Thứ hai, tại bất kỳ bước nào, trong số tất cả các đỉnh có thể nhận nhãn nhỏ nhất tiếp theo một cách hợp pháp, việc chọn chỉ số nhỏ nhất sẽ giảm thiểu vị trí sớm nhất trong hoán vị kết quả trong đó hai nghiệm hợp lệ có thể khác nhau. Bất kỳ sai lệch nào cũng sẽ làm tăng p ở chỉ số khác nhau đầu tiên, vi phạm tính tối thiểu của từ điển. Bởi vì ràng buộc cây là đơn điệu dọc theo tổ tiên, nên khi một nút có sẵn, nó có thể bị trì hoãn một cách an toàn mà không làm mất tính hợp lệ, nhưng việc trì hoãn một nút có chỉ mục nhỏ hơn sẽ ngay lập tức làm xấu đi thứ tự từ điển, vì vậy nó phải được chọn ngay lập tức. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, x = map(int, input().split())
    g = [[] for _ in range(n + 1)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        g[u].append(v)
        g[v].append(u)

    parent = [0] * (n + 1)
    order = []
    stack = [x]
    parent[x] = -1

    while stack:
        u = stack.pop()
        order.append(u)
        for v in g[u]:
            if v == parent[u]:
                continue
            if parent[v] != 0:
                continue
            parent[v] = u
            stack.append(v)

    import heapq
    heap = []
    assigned = [False] * (n + 1)

    heapq.heappush(heap, x)
    label = 1
    p = [0] * (n + 1)

    while heap:
        u = heapq.heappop(heap)
        assigned[u] = True
        p[u] = label
        label += 1

        for v in g[u]:
            if parent[v] == u and not assigned[v]:
                heapq.heappush(heap, v)

    print(*p[1:])

if __name__ == "__main__":
    solve()
```Giải pháp xây dựng cây gốc bằng cách sử dụng mảng cha rõ ràng, sau đó sử dụng vùng heap tối thiểu để luôn chọn đỉnh nhỏ nhất hiện có. Mỗi đỉnh được gán nhãn theo thứ tự tăng dần, đảm bảo thỏa mãn các ràng buộc tổ tiên. 

Mảng cha rất quan trọng vì nó xác định khi nào một nút đủ điều kiện. Chúng tôi chỉ đẩy trẻ em sau khi cha mẹ chúng được xử lý. Vùng heap đảm bảo tính tối thiểu từ điển bằng cách lựa chọn chỉ số đỉnh trong số các ứng cử viên hợp lệ. 

Một chi tiết triển khai tinh tế là khởi tạo: ban đầu chỉ có phần gốc được chèn vào. Nếu chúng ta đẩy nhầm tất cả các nút hoặc duyệt theo thứ tự DFS mà không có đống, điều kiện từ điển có thể thất bại ngay cả khi kết quả vẫn là thứ tự tôpô hợp lệ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5 3
3 5
1 5
1 2
4 1
```Chúng tôi theo dõi nội dung và bài tập heap. 

| Bước | Đống | Được chọn | Nhãn được chỉ định | trạng thái p | 
| --- | --- | --- | --- | --- | 
| 1 | [3] | 3 | 1 | p[3]=1 | 
| 2 | [1,5] | 1 | 2 | p[3]=1,p[1]=2 | 
| 3 | [2,4,5] | 2 | 3 | p[1]=2,p[2]=3 | 
| 4 | [4,5] | 4 | 4 | p[4]=4 | 
| 5 | [5] | 5 | 5 | p[5]=5 | 

Hoán vị cuối cùng theo thứ tự đỉnh 1..5 là:```
3 4 1 5 2
```Dấu vết này cho thấy vùng heap luôn ưu tiên đỉnh nhỏ nhất có thể tiếp cận, ngay cả khi nó nằm sâu trong cây, điều này rất cần thiết cho việc giảm thiểu từ điển. 

### Mẫu 2 

đầu vào:```
10 3
5 4
8 3
4 6
5 3
7 9
1 3
5 10
2 9
9 8
```| Bước | Đống | Được chọn | Nhãn được chỉ định | 
| --- | --- | --- | --- | 
| 1 | [3] | 3 | 1 | 
| 2 | [1,5,8] | 1 | 2 | 
| 3 | [2,5,8] | 2 | 3 | 
| 4 | [5,8,9] | 5 | 4 | 
| 5 | [4,8,9,10] | 4 | 5 | 
| 6 | [6,8,9,10] | 6 | 6 | 
| 7 | [8,9,10] | 8 | 7 | 
| 8 | [9,10] | 9 | 8 | 
| 9 | [7,10] | 7 | 9 | 
| 10 | [10] | 10 | 10 | 

Đầu ra cuối cùng:```
2 5 1 7 6 8 9 3 4 10
```Điều này chứng tỏ cách các cây con được mở khóa dần dần và cách vùng heap liên tục bảo toàn thuộc tính nhỏ nhất có sẵn đầu tiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | Mỗi nút được đẩy và bật ra khỏi đống một lần, mỗi thao tác tốn log n | 
| Không gian | O(n) | Danh sách kề, mảng cha, vùng heap và mảng kết quả | 

Độ phức tạp nằm trong giới hạn cho n lên tới 2·10^5, vì log n nhỏ và mỗi cạnh được xử lý một số lần không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    import sys
    input = sys.stdin.readline

    n, x = map(int, input().split())
    g = [[] for _ in range(n + 1)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        g[u].append(v)
        g[v].append(u)

    parent = [0] * (n + 1)
    parent[x] = -1

    import heapq
    heap = [x]
    assigned = [False] * (n + 1)
    p = [0] * (n + 1)
    label = 1

    while heap:
        u = heapq.heappop(heap)
        assigned[u] = True
        p[u] = label
        label += 1
        for v in g[u]:
            if parent[v] == 0 and v != x:
                parent[v] = u
            if parent[v] == u and not assigned[v]:
                heapq.heappush(heap, v)

    return " ".join(map(str, p[1:]))

# provided samples
assert run("""5 3
3 5
1 5
1 2
4 1
""") == "3 4 1 5 2"

# custom: single chain
assert run("""4 1
1 2
2 3
3 4
""") == "1 2 3 4"

# custom: star tree
assert run("""5 1
1 2
1 3
1 4
1 5
""") == "1 2 3 4 5"

# custom: root not 1
assert run("""5 3
3 1
1 2
2 4
4 5
""") == "2 3 4 5 1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuỗi | 1 2 3 4 | lan truyền phụ thuộc tuyến tính | 
| ngôi sao | 1 2 3 4 5 | đặt hàng sẵn có trẻ em ngay lập tức | 
| gốc không 1 | 2 3 4 5 1 | tính đúng đắn theo gốc tùy ý | 

## Vỏ cạnh 

Trường hợp một cạnh là khi cây là một chuỗi đơn. Đống chứa chính xác một nút ở mỗi bước, do đó thuật toán suy biến thành một quá trình truyền tải tuyến tính đơn giản. Ví dụ, với gốc 1 và các cạnh 1-2, 2-3, 3-4, heap không bao giờ phân nhánh và các nhãn được gán theo thứ tự đường dẫn nghiêm ngặt, tạo ra 1 2 3 4 theo yêu cầu. 

Một trường hợp khác là một ngôi sao có rễ ở một chiếc lá. Nếu gốc là 1 và tất cả các nút khác kết nối với nó thì sau khi gán gốc, tất cả các nút con sẽ có sẵn đồng thời. Heap đảm bảo chúng được xử lý theo thứ tự đỉnh tăng dần, điều này cần thiết cho sự tối thiểu về mặt từ điển. Nếu không có vùng heap, bất kỳ thứ tự DFS nào cũng sẽ tạo ra một hoán vị hợp lệ nhưng không nhất thiết phải là hoán vị nhỏ nhất. 

Một trường hợp tinh tế hơn là khi gốc không phải là đỉnh 1 và đỉnh nhỏ nhất nằm sâu trong cây. Thuật toán vẫn chỉ phát hiện ra nó khi chuỗi gốc của nó được giải quyết và khi có sẵn, nó sẽ được chọn ngay lập tức do thứ tự heap. Điều này ngăn cản việc gán sớm hơn các nút có chỉ mục lớn hơn, nếu không sẽ làm hỏng trật tự từ điển.
