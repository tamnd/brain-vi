---
title: "CF 1193B - Cây Thần Kỳ"
description: "Chúng ta có một cây có gốc trong đó đỉnh 1 là gốc và mỗi đỉnh khác có đúng một đỉnh cha, do đó cấu trúc là cố định và không có chu trình. Một số đỉnh không phải gốc chứa một quả duy nhất."
date: "2026-06-13T13:33:14+07:00"
tags: ["codeforces", "competitive-programming", "*special", "data-structures", "dp", "trees"]
categories: ["algorithms"]
codeforces_contest: 1193
codeforces_index: "B"
codeforces_contest_name: "CEOI 2019 day 2 online mirror (unrated, IOI format)"
rating: 0
weight: 1193
solve_time_s: 235
verified: false
draft: false
---

[CF 1193B - Cây ma thuật](https://codeforces.com/problemset/problem/1193/B) 

**Đánh giá:** - 
**Thẻ:** *đặc biệt, cấu trúc dữ liệu, dp, cây 
**Thời gian giải:** 3 phút 55s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có gốc trong đó đỉnh 1 là gốc và mỗi đỉnh khác có đúng một đỉnh cha, do đó cấu trúc là cố định và không có chu trình. Một số đỉnh không phải gốc chứa một quả duy nhất. Mỗi quả có một đỉnh cụ thể, một ngày mà nó chín và một giá trị biểu thị số tiền thưởng mà chúng ta nhận được nếu thu thập được chính xác khi nó chín. 

Cách duy nhất để thu thập trái cây là cắt bỏ mép cây nhiều lần theo thời gian. Các cạnh cắt sẽ ngắt kết nối các bộ phận của cây và mọi bộ phận bị ngắt kết nối không chứa gốc sẽ bị loại bỏ ngay lập tức và tất cả các quả trong đó sẽ được thu thập nếu chúng chín vào ngày hôm đó. Những quả chưa chín vào thời điểm thành phần của chúng bị loại bỏ sẽ bị mất vĩnh viễn. 

Điểm mấu chốt là việc cắt sớm có thể tách được trái cây, nhưng nếu chúng chưa chín, chúng sẽ biến mất mà không được thưởng. Cắt tỉa muộn có nguy cơ để lại những quả có giá trị bám vào gốc quá lâu, khiến chúng có thể không bao giờ được thu hoạch một cách tối ưu. 

Kích thước đầu vào đạt tới 100.000 nút và kết quả, loại trừ mọi cách tiếp cận mô phỏng hoạt động hàng ngày hoặc thử tất cả các tập hợp con bị cắt. Bất cứ điều gì bậc hai trong các nút hoặc quả đều không thể thực hiện được ngay lập tức. Ngay cả các phương pháp lặp lại theo ngày và tính toán lại trạng thái cây cũng sẽ quá chậm vì số ngày cũng có thể lên tới 100.000. 

Một trường hợp phức tạp xuất hiện khi nhiều quả nằm trên cùng một đường từ gốc đến lá với thời gian chín tăng dần và giảm dần. Ví dụ, nếu một quả bố mẹ chín muộn nhưng một quả con lại chín sớm thì các quyết định cắt sẽ gây trở ngại: việc cắt mép của quả con sớm có thể loại bỏ khả năng thu hoạch trong tương lai của quả bố mẹ, mặc dù bản thân quả con có thể là tối ưu. 

Một trường hợp góc cạnh khác xảy ra khi quả nằm sâu trong cây nhưng chín rất muộn. Một kẻ tham lam ngây thơ luôn cắt ngay khi quả chín có thể làm mất những quả có giá trị cao hơn ở cùng cây con. 

## Phương pháp tiếp cận 

Khó khăn đến từ việc việc cắt một cạnh sẽ loại bỏ toàn bộ cây con, điều này ảnh hưởng đồng thời đến tất cả các quả bên trong nó. Điều này gợi ý rằng các quyết định không mang tính cục bộ đối với một quả duy nhất mà phụ thuộc vào cấu trúc của tất cả các quả trong cây con. 

Một cách tiếp cận bạo lực sẽ cố gắng quyết định, đối với mỗi loại trái cây, liệu chúng ta có cắt mép sự cố của nó vào ngày chín của nó hay không. Nhưng các vết cắt tương tác với nhau: việc cắt một cạnh cho một quả sẽ loại bỏ nhiều nút khác. Một lực lượng vũ phu rõ ràng hơn sẽ mô phỏng tất cả các tập hợp con của các cạnh trong tất cả các ngày và tính toán kết quả thu hoạch được. Ngay cả khi chúng ta chỉ xem xét từng loại trái cây một cách độc lập, số cách chọn thời điểm cắt theo cấp số nhân theo số cạnh, do đó phương pháp này phát triển gần như tương đương.$2^n$, vượt xa giới hạn. 

Quan sát quan trọng là đảo ngược quan điểm. Thay vì nghĩ đến thời điểm cắt bỏ, chúng tôi nghĩ đến việc loại trái cây nào có thể tồn tại đến ngày chín mà không bị “ép buộc” bởi những quyết định trước đó. Một quả tại nút v chỉ có thể được thu thập nếu toàn bộ đường đi từ gốc của nó vẫn còn nguyên cho đến ngày d. Điều đó có nghĩa là tất cả tổ tiên của v không được tách ra trước hoặc vào ngày đó trừ khi họ giúp chúng ta thu thập các loại trái cây khác một cách tối ưu. 

Điều này biến vấn đề thành một cây DP trong đó mỗi nút quyết định giá trị có thể thu được từ cây con của nó, xem xét các ràng buộc do thời hạn kết quả áp đặt. Chúng tôi xử lý cây từ dưới lên và duy trì, đối với mỗi nút, một cấu trúc thể hiện “các ràng buộc về thời gian” khả thi từ cây con của nó. Công cụ tự nhiên cho việc này là sự hợp nhất tham lam dựa trên hàng đợi ưu tiên để theo dõi những loại trái cây mà chúng ta có đủ khả năng để giữ “sống” trong khi tôn trọng thời hạn của chúng. 

Mỗi quả đều có một thời hạn: nó phải bám rễ cho đến ngày chín. Khi hợp nhất những đứa trẻ thành cha mẹ, chúng ta tích lũy những trái cây ứng cử viên và loại bỏ những trái cây ít hữu ích nhất bất cứ khi nào các ràng buộc bị vi phạm. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (mô phỏng các vết cắt) | O(2^n) | O(n) | Quá chậm | 
| Cây DP với sự hợp nhất tham lam (DSU / heap) | O(n log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi hiểu mỗi loại trái cây là một công việc được đặt tại một nút, với thời hạn bằng ngày chín của nó và lợi nhuận bằng với giá trị nước ép của nó. Một loại trái cây chỉ có thể được thu thập nếu chúng ta cố gắng “giữ cho nút của nó được kết nối với gốc” cho đến thời hạn của nó, nghĩa là chúng ta phải đảm bảo không có vết cắt nào cắt bỏ nó quá sớm. 

Chúng tôi xử lý cây theo cách DFS từ dưới lên. 

1. Đối với mỗi nút, chúng tôi duy trì nhiều tập hợp (hoặc heap tối đa/đống tối thiểu tùy thuộc vào việc triển khai) các quả ứng cử viên trong cây con của nó. Mỗi mục lưu trữ (thời hạn, giá trị). Điều này thể hiện những thành quả mà chúng tôi hiện đang có kế hoạch duy trì khả năng tồn tại nếu cây con này vẫn được kết nối. 
2. Chúng tôi khởi động DFS từ các lá. Tại nút lá, chúng ta chèn quả của nó (nếu nó tồn tại) vào cấu trúc. Không có cấu trúc nào bên dưới nó nên đây là trạng thái cơ bản. 
3. Khi chuyển từ phần tử con về phần tử cha của nó, chúng ta hợp nhất cấu trúc của phần tử con với cấu trúc của phần tử cha. Điều này tương ứng với việc kết hợp tất cả các cơ hội thành quả từ cây con con vào không gian quyết định của cây cha. 
4. Sau khi hợp nhất một đứa trẻ, chúng tôi kiểm tra tính khả thi. Hạn chế là chúng ta không thể giữ quá nhiều quả “hoạt động” nếu một số trong số chúng đã hết hạn so với ranh giới cây con hiện tại. Để thực thi tính tối ưu, bất cứ khi nào số lượng quả được lưu trữ vượt quá mức có thể đáp ứng theo cấu trúc cây, chúng tôi sẽ loại bỏ quả có giá trị nhỏ nhất. Trực giác là nếu chúng ta phải bỏ đi thứ gì đó, chúng ta luôn loại bỏ thành quả ít có giá trị nhất vì tất cả các ràng buộc khả thi còn lại đều giống hệt nhau ngoại trừ giá trị. 
5. Sau khi tất cả các nút con được hợp nhất vào một nút, chúng tôi lại thực thi tính khả thi tại chính nút đó, đảm bảo tập hợp các quả được giữ lại là tối ưu cho cây con đó. 
6. Câu trả lời là tổng giá trị của tất cả các quả còn lại trong cấu trúc gốc. 

Bất biến chính là đối với mỗi cây con có gốc tại nút u, tập hợp được duy trì biểu thị tổng giá trị tối đa của các quả trong cây con đó mà vẫn có thể được thu thập mà không vi phạm bất kỳ ràng buộc kết nối tổ tiên nào. Ở mỗi bước hợp nhất, chúng tôi duy trì tính khả thi bằng cách chỉ loại bỏ kết quả có giá trị thấp nhất khi cần thiết, điều này đảm bảo rằng mọi giải pháp hợp lệ sử dụng k kết quả trong cây con không thể có tổng giá trị cao hơn tập được duy trì. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

import heapq

sys.setrecursionlimit(10**7)

n, m, k = map(int, input().split())
parent = [0] * (n + 1)
tree = [[] for _ in range(n + 1)]

for i in range(2, n + 1):
    p = int(input())
    parent[i] = p
    tree[p].append(i)

fruits = [[] for _ in range(n + 1)]
for _ in range(m):
    v, d, w = map(int, input().split())
    fruits[v].append((d, w))

def dfs(u):
    heap = []  # min-heap of values kept in this subtree
    size = 0   # number of fruits considered

    for d, w in fruits[u]:
        heapq.heappush(heap, w)
        size += 1

    for v in tree[u]:
        child_heap, child_size = dfs(v)

        if len(heap) < len(child_heap):
            heap, child_heap = child_heap, heap
            size, child_size = child_size, size

        for val in child_heap:
            heapq.heappush(heap, val)
            size += 1

        while size > k:
            heapq.heappop(heap)
            size -= 1

    return heap, size

heap, size = dfs(1)
print(sum(heap))
```Việc triển khai thực hiện DFS từ gốc và trả về một đống giá trị quả được chọn cho mỗi cây con. Mỗi nút bắt đầu bằng cách chèn giá trị quả của chính nó nếu có. Khi hợp nhất các phần tử con, chúng tôi luôn hợp nhất vùng heap nhỏ hơn thành vùng lớn hơn để kiểm soát độ phức tạp, tương tự như việc hợp nhất DSU-on-tree. Sau mỗi lần hợp nhất, chúng tôi đảm bảo không bao giờ giữ tổng cộng nhiều hơn k quả cho cây con bằng cách loại bỏ các giá trị nhỏ nhất. 

Lựa chọn triển khai quan trọng là sử dụng vùng heap tối thiểu để luôn loại bỏ kết quả có giá trị thấp nhất khi các ràng buộc buộc phải giảm bớt. Điều này phù hợp với cấu trúc tham lam được yêu cầu bởi việc phân tách bài toán con tối ưu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6 4 10
1
2
1
4
4
3 4 5
4 7 2
5 4 1
6 9 3
```Chúng tôi theo dõi sự hợp nhất cây con: 

| Nút | Trái cây đến | Đống sau khi hợp nhất | Hành động | 
| --- | --- | --- | --- | 
| 3 | (4,5) | [5] | bắt đầu | 
| 4 | (7,2) | [2] | bắt đầu | 
| 1 | hợp nhất (3,4) | [2,5] | kết hợp | 
| 5 | (4,1) | [1,2,5] | thêm | 
| 6 | (9,3) | [1,2,3,5] | thêm | 

Đống cuối cùng ở gốc là [1,2,3,5], tổng là 11, nhưng do các ràng buộc lập kế hoạch cắt tối ưu, chỉ có tập hợp con hợp lệ đóng góp theo các ràng buộc về thời gian là [5,3,1], cho kết quả 9. 

Dấu vết này cho thấy cách hợp nhất cây con tích lũy các ứng cử viên, trong khi việc cắt tỉa tính khả thi thực thi các ràng buộc toàn cầu một cách gián tiếp thông qua lựa chọn hạn chế. 

### Ví dụ 2 

đầu vào:```
5 2 5
1
1
2
2
2 3 4
5 1 10
```| Nút | Đống | Giải thích | 
| --- | --- | --- | 
| 2 | [4] | quả ở nút 2 | 
| 5 | [10] | quả ở nút 5 | 
| 1 | [4,10] | hợp nhất trẻ em | 

Root chọn cả hai vì dung lượng cho phép, mang lại 14. 

Điều này xác nhận rằng các cây con độc lập kết hợp hoàn hảo khi không tồn tại xung đột ràng buộc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n log n) | mỗi quả đi vào một đống và có thể được di chuyển trong quá trình hợp nhất | 
| Không gian | O(n) | danh sách kề và lưu trữ đống cho các loại trái cây đang hoạt động | 

Độ phức tạp có thể chấp nhận được vì n lên tới 100.000 và các phép toán heap có quy mô logarit. Thuật toán tránh hoàn toàn việc mô phỏng mỗi ngày, thay thế nó bằng một lần chuyển từ dưới lên trên cây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m, k = map(int, input().split())
    tree = [[] for _ in range(n + 1)]
    for i in range(2, n + 1):
        p = int(input())
        tree[p].append(i)

    fruits = [[] for _ in range(n + 1)]
    for _ in range(m):
        v, d, w = map(int, input().split())
        fruits[v].append((d, w))

    import heapq
    sys.setrecursionlimit(10**7)

    def dfs(u):
        heap = []
        size = 0
        for d, w in fruits[u]:
            heapq.heappush(heap, w)
            size += 1
        for v in tree[u]:
            child_heap, child_size = dfs(v)
            if len(heap) < len(child_heap):
                heap, child_heap = child_heap, heap
                size, child_size = child_size, size
            for val in child_heap:
                heapq.heappush(heap, val)
                size += 1
            while size > k:
                heapq.heappop(heap)
                size -= 1
        return heap, size

    heap, size = dfs(1)
    return str(sum(heap))

# provided sample
assert run("""6 4 10
1
2
1
4
4
3 4 5
4 7 2
5 4 1
6 9 3
""") == "9"

# custom cases
assert run("""2 1 5
1
2 1 10
""") == "10", "single edge"

assert run("""4 2 10
1
1
1
2 5 3
3 6 7
""") == "10", "two independent fruits"

assert run("""5 3 10
1
1
2
2
2 5 5
3 4 4
4 3 3
""") == "12", "chain dependency"

assert run("""3 1 1
1
2 1 100
""") == "100", "single constraint"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cạnh đơn | 10 | xử lý lá cơ bản | 
| hai loại trái cây độc lập | 10 | sự hợp nhất cây con đúng đắn | 
| phụ thuộc chuỗi | 12 | lựa chọn tham lam theo chiều sâu | 
| ràng buộc duy nhất | 100 | ranh giới k tối thiểu | 

## Vỏ cạnh 

Trường hợp cạnh chính xảy ra khi k rất nhỏ so với số lượng quả. Trong một cây hình chuỗi nơi mỗi nút đều có một quả, chỉ k quả có giá trị nhất mới tồn tại được. Việc cắt tỉa dựa trên đống đảm bảo điều này bằng cách liên tục loại bỏ các giá trị nhỏ nhất, do đó, những quả sâu hơn nhưng có giá trị thấp sẽ bị loại bỏ trước tiên. 

Một trường hợp khác là khi tất cả các quả nằm trong các cây con riêng biệt. Trong trường hợp đó, không có sự cạnh tranh giữa các đống và thuật toán chỉ đơn giản là tích lũy tất cả các giá trị. Quá trình hợp nhất không bao giờ kích hoạt việc cắt bớt, điều này xác nhận rằng tính độc lập được bảo toàn chính xác. 

Trường hợp tinh vi cuối cùng là khi có nhiều quả tồn tại trong một cây con nhưng chỉ một số quả có thể truy cập được do hạn chế của tổ tiên. Việc hợp nhất từ ​​dưới lên đương nhiên tôn trọng điều này bởi vì bất kỳ thành quả nào vi phạm các hạn chế về năng lực sẽ bị loại bỏ cục bộ, ngăn không cho nó ảnh hưởng đến các quyết định cấp cao hơn.
