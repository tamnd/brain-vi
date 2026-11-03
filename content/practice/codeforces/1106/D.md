---
title: "CF 1106D - Tết Nguyên đán và một chuyến lang thang"
description: "Chúng ta có một đồ thị liên thông vô hướng với các nút $n$. Bob bắt đầu ở nút 1 và thực hiện việc đi dọc theo các cạnh. Mỗi khi anh ấy vào một nút chưa từng thấy trước đây, anh ấy sẽ viết nó ra."
date: "2026-06-13T08:10:50+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dfs-and-similar", "graphs", "greedy", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 1106
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 536 (Div. 2)"
rating: 1500
weight: 1106
solve_time_s: 280
verified: true
draft: false
---

[CF 1106D - Tết Nguyên đán và một chuyến lang thang](https://codeforces.com/problemset/problem/1106/D) 

**Đánh giá:** 1500 
**Thẻ:** cấu trúc dữ liệu, dfs và tương tự, đồ thị, tham lam, đường dẫn ngắn nhất 
**Thời gian giải:** 4 phút 40s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị liên thông vô hướng với$n$nút. Bob bắt đầu ở nút 1 và thực hiện việc đi dọc theo các cạnh. Mỗi khi anh ấy vào một nút chưa từng thấy trước đây, anh ấy sẽ viết nó ra. Quá trình đi tiếp tục cho đến khi tất cả các nút đã được truy cập ít nhất một lần và trình tự được viết chứa mỗi nút chính xác một lần. 

Nhiệm vụ không phải là đưa ra một bước đi cụ thể mà là xác định chuỗi lần truy cập đầu tiên nhỏ nhất có thể về mặt từ điển mà Bob có thể có được bằng cách tự do chọn đường đi ngang của mình. 

Vì vậy, vấn đề thực sự là việc kiểm soát thứ tự các nút được phát hiện trong quá trình truyền tải đồ thị, bắt đầu từ nút 1, với ràng buộc là chuyển động chỉ dọc theo các cạnh. 

Những hạn chế$n, m \le 10^5$ngay lập tức loại trừ bất kỳ cách tiếp cận nào cố gắng mô phỏng tất cả các bước đi có thể có hoặc thực hiện khám phá theo cấp số nhân trên các đường đi. Thậm chí$O(n^2)$không an toàn trong đồ thị dày đặc. Chúng ta cần một cái gì đó gần với thời gian tuyến tính hoặc tuyến tính, thông thường$O((n + m)\log n)$hoặc$O(n + m)$. 

Một cách tiếp cận đơn giản là thực hiện DFS hoặc BFS và luôn chọn hàng xóm chưa được thăm dò có số lượng nhỏ nhất. Tuy nhiên, điều này không thành công trong các biểu đồ nơi các lựa chọn tối ưu cục bộ sẽ chặn quyền truy cập vào các nút nhỏ hơn sau này. 

Một kịch bản thất bại cụ thể là một cấu trúc giống như chu kỳ, trong đó các quyết định cục bộ tham lam có thể cô lập các nút nhỏ hơn. Ví dụ: 

đầu vào:```
4 4
1 4
4 3
3 2
2 4
```Nếu chúng ta luôn chọn người hàng xóm nhỏ nhất một cách tham lam mà không có trật tự toàn cầu, chúng ta có thể đi$1 \to 4 \to 3 \to 2$, nhưng một chiến lược khám phá khác có thể cho phép tiền tố nhỏ hơn về mặt từ điển bằng cách trì hoãn một cách cẩn thận các cạnh nhất định. Khó khăn chính là khi một nút được truy cập theo thứ tự DFS, vị trí của nó trong chuỗi đầu ra sẽ được cố định. 

Vì vậy, chúng ta cần một chiến lược truyền tải để thực thi tính tối thiểu về từ điển toàn cầu, chứ không chỉ các lựa chọn tham lam cục bộ. 

## Phương pháp tiếp cận 

Cách giải thích thô bạo là xem xét tất cả các bước đi hợp lệ có thể bắt đầu từ nút 1, theo dõi thứ tự truy cập kết quả và lấy chuỗi nhỏ nhất về mặt từ điển. Điều này đúng về mặt lý thuyết vì nó khám phá tất cả các hoán vị khả thi do các bước đi trên đồ thị gây ra. Tuy nhiên, số lần đi bộ tăng theo cấp số nhân theo chu kỳ trong biểu đồ. Ngay cả một đồ thị đơn giản với hệ số phân nhánh 3 trên độ sâu 100 cũng trở nên lớn về mặt thiên văn. 

Thông tin chi tiết quan trọng là chúng tôi chỉ quan tâm đến thứ tự của những lần ghé thăm đầu tiên chứ không phải toàn bộ chuyến đi. Khi một nút được truy cập, nó sẽ không bao giờ liên quan đến các quyết định đặt hàng nữa. Điều này gợi ý rằng chúng ta đang xây dựng một cây giống DFS, nhưng với ưu tiên chung cho nút sẵn có nhỏ nhất tại mỗi điểm mở rộng. 

Nếu chúng tôi thử một DFS tiêu chuẩn với các danh sách kề được sắp xếp theo thứ tự tăng dần, nó vẫn thất bại trong trường hợp việc truy cập một hàng xóm nhỏ hơn quá sớm sẽ ngăn cản việc tiếp cận một nút thậm chí còn nhỏ hơn thông qua thứ tự nhánh khác. Ý tưởng đúng là mô phỏng DFS nhưng duy trì một tập hợp toàn cầu các nút hiện có thể truy cập được nhưng chưa được khám phá đầy đủ, luôn chọn nút nhỏ nhất trong số chúng có thể tiếp cận được từ biên giới truyền tải hiện tại. 

Điều này dẫn đến một DFS được sửa đổi bằng cách sử dụng hàng đợi ưu tiên (min-heap). Chúng tôi đẩy các nút khi chúng có thể truy cập được và luôn trích xuất nút nhỏ nhất có sẵn tiếp theo. Khi một nút được truy cập, chúng tôi sẽ mở rộng nút đó và chèn các nút lân cận chưa được truy cập của nó. Điều này hoạt động giống như một cuộc khám phá bị hạn chế về mặt từ điển về biên giới có thể tiếp cận. 

Thuộc tính cấu trúc quan trọng là tại bất kỳ thời điểm nào, bất kỳ nút nào đã được phát hiện nhưng chưa được xử lý đều là ứng cử viên hợp lệ cho vị trí đầu ra tiếp theo và việc chọn mức tối thiểu từ điển duy trì nhỏ nhất trên toàn cầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các bước đi) | hàm mũ | hàm mũ | Quá chậm | 
| DFS tham lam với biên giới heap tối thiểu |$O((n + m)\log n)$|$O(n + m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng danh sách kề cho đồ thị. 

Chúng ta cần truy cập nhanh vào các nút lân cận của mỗi nút vì việc mở rộng phụ thuộc vào cấu trúc biểu đồ. 
2. Duy trì mảng boolean`visited`để theo dõi xem một nút đã được ghi lại chưa. 

Điều này đảm bảo mỗi nút xuất hiện chính xác một lần trong đầu ra. 
3. Sử dụng min-heap (hàng đợi ưu tiên) được khởi tạo với nút 1. 

Đống này đại diện cho tất cả các nút có thể truy cập được từ quá trình truyền tải cho đến nay nhưng chưa được xử lý. 
4. Trích xuất liên tục nút nhỏ nhất`u`từ đống. 

Điều này đảm bảo rằng trong số tất cả các ứng viên hợp lệ, chúng ta luôn chọn phần tử tiếp theo nhỏ nhất có thể có trong dãy. 
5. Nếu`u`đã được truy cập, bỏ qua nó. 

Điều này có thể xảy ra vì các nút có thể được chèn nhiều lần từ các nút lân cận khác nhau. 
6. Nếu không, hãy đánh dấu`u`như đã truy cập và thêm nó vào chuỗi câu trả lời. 
7. Đẩy tất cả những người hàng xóm chưa ghé thăm của`u`vào đống. 

Đây trở thành những ứng cử viên mới có thể tiếp cận được cho các bước đi trong tương lai. 
8. Tiếp tục cho đến khi heap trống. 

Bước lý luận chính là chúng ta không bao giờ truy cập lại một nút theo thứ tự đầu ra, nhưng chúng ta có thể chèn lại nút đó vào vùng nhớ heap nhiều lần; trùng lặp không thành vấn đề bởi vì`visited`lọc chúng. 

### Tại sao nó hoạt động 

Ở mỗi bước, vùng heap chứa chính xác tập hợp các nút có thể truy cập được từ các nút đã được truy cập thông qua ít nhất một cạnh, nhưng chưa được đưa vào đầu ra. Bất kỳ bước đi hợp lệ nào cuối cùng đều phải truy cập vào một trong các nút này tiếp theo. Việc chọn giá trị nhỏ nhất trong số chúng đảm bảo rằng không có giao dịch truyền tải hợp lệ thay thế nào có thể tạo ra tiền tố nhỏ hơn, bởi vì bất kỳ nút nhỏ hơn nào cũng đã được sử dụng hoặc không thể truy cập được nếu không đi qua các nút đã cố định. Điều này tạo ra một bất biến tham lam: sau khi tạo ra cái đầu tiên$k$các nút, thuật toán đã tạo ra tiền tố nhỏ nhất về mặt từ điển có thể đạt được bằng bất kỳ bước đi hợp lệ nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

import heapq

def solve():
    n, m = map(int, input().split())
    g = [[] for _ in range(n + 1)]

    for _ in range(m):
        u, v = map(int, input().split())
        g[u].append(v)
        g[v].append(u)

    visited = [False] * (n + 1)
    heap = []

    heapq.heappush(heap, 1)

    res = []

    while heap:
        u = heapq.heappop(heap)
        if visited[u]:
            continue

        visited[u] = True
        res.append(u)

        for v in g[u]:
            if not visited[v]:
                heapq.heappush(heap, v)

    print(*res)

if __name__ == "__main__":
    solve()
```Danh sách kề lưu trữ cấu trúc biểu đồ đầy đủ để chúng tôi có thể mở rộng từng nút được truy cập một cách hiệu quả. Heap đảm bảo rằng trong số tất cả các nút biên giới, chúng tôi luôn chọn ứng cử viên nhỏ nhất có sẵn. Mảng đã truy cập đảm bảo tính chính xác bất chấp sự trùng lặp trong vùng heap, vì một nút có thể được phát hiện từ nhiều nút cha. 

Một điểm tinh tế là chúng tôi không sắp xếp danh sách kề. Việc sắp xếp là không cần thiết vì thứ tự chung được thực thi theo vùng heap và việc sắp xếp sẽ chỉ tăng thêm chi phí mà không cải thiện tính chính xác. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2
1 2
1 3
```| Bước | Đống | Đã truy cập | Đầu ra | 
| --- | --- | --- | --- | 
| 1 | [1] | {} | [] | 
| 2 | [] | {1} | [1] | 
| 3 | [2, 3] | {1} | [1] | 
| 4 | [3] | {1,2} | [1,2] | 
| 5 | [] | {1,2,3} | [1,2,3] | 

Điều này cho thấy rằng sau khi mở rộng nút 1, cả hai hàng xóm đều trở thành ứng cử viên và vùng heap đảm bảo 2 được chọn trước 3, tạo ra chuỗi nhỏ nhất về mặt từ điển. 

### Ví dụ 2 

đầu vào:```
5 5
1 4
4 3
3 2
2 4
4 5
```| Bước | Đống | Đã truy cập | Đầu ra | 
| --- | --- | --- | --- | 
| 1 | [1] | {} | [] | 
| 2 | [] | {1} | [1] | 
| 3 | [4] | {1} | [1] | 
| 4 | [3,5] | {1,4} | [1,4] | 
| 5 | [2,5] | {1,3,4} | [1,4,3] | 
| 6 | [5] | {1,2,3,4} | [1,4,3,2] | 
| 7 | [] | {1,2,3,4,5} | [1,4,3,2,5] | 

Dấu vết cho thấy cách vùng nhớ duy trì đường biên một cách linh hoạt và luôn chọn nút chưa được truy cập nhỏ nhất có thể truy cập được. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O((n + m)\log n)$| mỗi thao tác chèn cạnh và trích xuất nút sử dụng thao tác heap | 
| Không gian |$O(n + m)$| danh sách kề cộng với mảng heap và mảng đã truy cập | 

Kích thước đồ thị lên tới$10^5$các nút và các cạnh vừa vặn thoải mái trong các giới hạn này vì các phép toán heap là logarit và tổng số phép toán tỷ lệ tuyến tính với các cạnh. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import heapq

    def solve():
        n, m = map(int, input().split())
        g = [[] for _ in range(n + 1)]
        for _ in range(m):
            u, v = map(int, input().split())
            g[u].append(v)
            g[v].append(u)

        visited = [False] * (n + 1)
        heap = [1]
        res = []

        while heap:
            u = heapq.heappop(heap)
            if visited[u]:
                continue
            visited[u] = True
            res.append(u)
            for v in g[u]:
                if not visited[v]:
                    heapq.heappush(heap, v)

        return " ".join(map(str, res))

    return solve()

# provided sample
assert run("""3 2
1 2
1 3
""") == "1 2 3"

# all connected line
assert run("""4 3
1 2
2 3
3 4
""") == "1 2 3 4"

# star graph
assert run("""5 4
1 2
1 3
1 4
1 5
""") == "1 2 3 4 5"

# cycle
assert run("""4 4
1 2
2 3
3 4
4 1
""") == "1 2 3 4"

# duplicated edges
assert run("""3 4
1 2
1 2
2 3
1 3
""") == "1 2 3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đồ thị đường | 1 2 3 4 | truyền tải chuỗi đơn giản | 
| đồ thị sao | 1 2 3 4 5 | ưu tiên hàng xóm | 
| đồ thị chu trình | 1 2 3 4 | chu trình xử lý | 
| cạnh trùng lặp | 1 2 3 | độ bền đa cạnh | 

## Vỏ cạnh 

Một trường hợp tinh tế là các cạnh trùng lặp và các vòng tự lặp. Ví dụ:```
3 3
1 1
1 2
2 3
```Vòng lặp tự trên 1 khiến nút 1 bị đẩy vào heap nhiều lần, nhưng việc kiểm tra đã truy cập đảm bảo nó chỉ được xử lý một lần. Thuật toán vẫn tiến hành như sau: 

heap bắt đầu bằng [1], xử lý 1, đẩy 1,2,2,3, sau đó liên tục bỏ qua 1 đã truy cập và sao chép 2 mục, cuối cùng tạo ra 1 2 3. 

Một trường hợp khác là khi nút được đánh số nhỏ nhất không thể truy cập được ngay lập tức nếu không đi qua các nút lớn hơn. Cách tiếp cận heap giải quyết vấn đề này một cách tự nhiên vì khả năng tiếp cận chỉ mở rộng thông qua các nút đã truy cập, đảm bảo chúng tôi không bao giờ xem xét các nút thực sự không thể truy cập và trong số các nút có thể truy cập, chúng tôi luôn chọn ứng cử viên nhỏ nhất hiện có.
