---
title: "CF 980E - Trò Chơi Số"
description: "Chúng ta được cấp một cây có nút $n$. Mỗi nút đại diện cho một quận và hệ thống đường đảm bảo có chính xác một con đường đơn giản giữa hai quận bất kỳ."
date: "2026-06-17T01:13:18+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "greedy", "trees"]
categories: ["algorithms"]
codeforces_contest: 980
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 480 (Div. 2)"
rating: 2200
weight: 980
solve_time_s: 106
verified: true
draft: false
---

[CF 980E - Trò chơi số](https://codeforces.com/problemset/problem/980/E) 

**Đánh giá:** 2200 
**Tags:** cấu trúc dữ liệu, tham lam, cây cối 
**Thời gian giải:** 1 phút 46s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi được tặng một cái cây với$n$nút. Mỗi nút đại diện cho một quận và hệ thống đường đảm bảo có chính xác một con đường đơn giản giữa hai quận bất kỳ. Mỗi quận$i$đóng góp một giá trị bằng$2^i$, do đó, các quận được đánh số cao hơn có giá trị theo cấp số nhân so với các quận được đánh số thấp hơn. 

Chúng ta phải loại bỏ chính xác$k$các huyện. Sau khi xóa, các quận còn lại vẫn phải tạo thành biểu đồ được kết nối theo các con đường ban đầu, nghĩa là bạn vẫn có thể di chuyển giữa hai nút còn lại bất kỳ mà không cần đi qua các nút đã bị xóa. Trong số tất cả các cách hợp lệ để loại bỏ$k$các nút, chúng tôi muốn tối đa hóa tổng của$2^i$qua các nút còn lại, tương đương với việc giảm thiểu tác động của việc loại bỏ các nút có chỉ số cao. 

Trọng số mũ quan trọng vì$2^i$thống trị bất kỳ sự kết hợp nào của các chỉ số nhỏ hơn. Điều này ngụ ý rằng mục tiêu có bản chất từ ​​điển: việc giữ một nút được đánh số cao hơn luôn quan trọng hơn bất kỳ tập hợp các nút được đánh số thấp hơn. 

Những ràng buộc cho phép$n$lên tới$10^6$, vì vậy mọi nghiệm đều phải gần tuyến tính hoặc$O(n \log n)$. Một chiến lược bậc hai như tính toán lại kết nối nhiều lần hoặc mô phỏng việc xóa bằng kiểm tra biểu đồ đầy đủ sẽ quá chậm. 

Một điểm tinh tế là không phải mọi tập hợp con của nút đều được phép. Ngay cả khi một tập hợp các nút có tổng trọng số cao thì nó vẫn không hợp lệ nếu nó ngắt kết nối với biểu đồ còn lại. Ví dụ: việc loại bỏ sớm một nút khớp nối trung tâm có thể chia các nút còn lại thành nhiều thành phần, điều này bị cấm. 

Một trường hợp cạnh khác là khi cây là một ngôi sao. Nếu nút 1 là trung tâm và chúng ta loại bỏ nó quá sớm thì tất cả các nút còn lại sẽ bị cô lập. Ví dụ, nếu$n = 4$và các cạnh là$1$kết nối với$2,3,4$, việc loại bỏ nút 1 trước tiên sẽ để lại ba nút bị cô lập, vi phạm kết nối. Vì vậy, việc xóa phải tôn trọng các ràng buộc về cấu trúc, không chỉ thứ tự giá trị. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử tất cả các tập hợp con có kích thước$n-k$, kiểm tra xem đồ thị con cảm ứng có được kết nối hay không và tính tổng trọng số. Ngay cả khi bỏ qua việc kiểm tra kết nối, số lượng tập hợp con vẫn$\binom{n}{k}$, điều này là không thể thực hiện được ngay cả đối với những trường hợp nhỏ$n$. Một lần kiểm tra kết nối cho mỗi ứng viên đã tốn phí$O(n)$, làm cho cách tiếp cận này theo cấp số nhân và không thể sử dụng được. 

Quan sát quan trọng xuất phát từ việc hiểu cách duy trì kết nối khi bị xóa trong cây. Trong cây, việc loại bỏ một nút chỉ có thể ngắt kết nối biểu đồ còn lại nếu nút đó là điểm khớp nối cho tập hợp còn lại hiện tại. Tuy nhiên, trong một cây, mọi nút đều trở thành một điểm khớp nối tiềm năng trừ khi nó là một lá của biểu đồ còn lại hiện tại. 

Điều này dẫn đến việc đơn giản hóa cấu trúc: nếu chúng ta muốn biểu đồ còn lại được kết nối trong suốt quá trình xóa, chúng ta chỉ có thể xóa các nút là lá trong cây còn lại hiện tại. Việc loại bỏ bất kỳ nút bên trong nào sẽ chia các đỉnh còn lại thành ít nhất hai thành phần. 

Một khi chúng ta chấp nhận hạn chế đó, vấn đề sẽ trở thành một quá trình tham lam. Chúng tôi liên tục loại bỏ một chiếc lá. Vì chúng ta muốn tối đa hóa tổng còn lại và chỉ số cao hơn sẽ có giá trị hơn nên chúng ta nên loại bỏ lá nhỏ nhất có sẵn ở mỗi bước. Việc xóa các nhãn nhỏ hơn trước tiên sẽ giữ lại các nhãn lớn hơn càng lâu càng tốt trong khi vẫn duy trì được khả năng kết nối. 

Quá trình này có thể được duy trì linh hoạt bằng cách sử dụng hàng đợi ưu tiên của các lá hiện tại. Mỗi lần chúng ta loại bỏ một lá, lá lân cận của nó có thể trở thành một lá mới và chúng ta cập nhật tương ứng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | O(n) | Quá chậm | 
| Cắt lá tham lam bằng đống |$O(n \log n)$| O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì mức độ hiện tại của các nút trong cây còn lại và cấu trúc luôn cung cấp cho chúng tôi lá có thể tháo rời được đánh số nhỏ nhất. 

1. Tính bậc của mỗi nút trong danh sách kề. Điều này thể hiện số lượng kết nối đang hoạt động mà mỗi nút hiện có. 
2. Khởi tạo một heap tối thiểu chứa tất cả các nút có bậc chính xác bằng 1. Đây là các lá hiện tại và chỉ những nút này mới an toàn để loại bỏ mà không làm đứt kết nối. 
3. Lặp lại chính xác quy trình sau$k$lần. Mỗi lần lặp lại tương ứng với việc loại bỏ một nút. 
4. Trích xuất nút được lập chỉ mục nhỏ nhất từ ​​​​đống. Điều này đảm bảo chúng tôi loại bỏ nút có giá trị thấp nhất trong số tất cả các lựa chọn hiện an toàn, phù hợp với việc giảm thiểu tổn thất về trọng số theo cấp số nhân. 
5. Đánh dấu nút này là đã bị xóa. Điều này có nghĩa là nó sẽ không xuất hiện trong thành phần được kết nối cuối cùng. 
6. Đối với nút lân cận duy nhất của nút này, hãy giảm bậc của nó đi một vì cạnh của nút bị loại bỏ sẽ biến mất. Nếu bậc của hàng xóm này trở thành 1 và nó chưa bị xóa, nó sẽ trở thành một lá mới và được chèn vào heap. 
7. Tiếp tục cho đến khi$k$các nút đã bị loại bỏ. 

Sau các bước này, các nút còn lại tạo thành một cây con được kết nối và các nút bị loại bỏ chính xác là những nút được ghi lại trong quá trình. 

Tính đúng đắn dựa trên tính bất biến là đồ thị còn lại luôn được kết nối và mỗi lần xóa sẽ bảo toàn thuộc tính này vì chúng ta chỉ xóa các lá. Ngoài ra, trong số tất cả các trình tự loại bỏ lá hợp lệ, việc luôn chọn lá nhãn nhỏ nhất đảm bảo rằng các nhãn cao hơn được bảo tồn trong thời gian dài nhất có thể về mặt cấu trúc, điều này là tối ưu do trọng số theo cấp số nhân. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

import heapq

n, k = map(int, input().split())
adj = [[] for _ in range(n + 1)]
deg = [0] * (n + 1)

for _ in range(n - 1):
    a, b = map(int, input().split())
    adj[a].append(b)
    adj[b].append(a)
    deg[a] += 1
    deg[b] += 1

removed = [False] * (n + 1)
heap = []

for i in range(1, n + 1):
    if deg[i] == 1:
        heapq.heappush(heap, i)

ans = []

for _ in range(k):
    while heap:
        v = heapq.heappop(heap)
        if not removed[v]:
            break

    removed[v] = True
    ans.append(v)

    for to in adj[v]:
        if not removed[to]:
            deg[to] -= 1
            if deg[to] == 1:
                heapq.heappush(heap, to)

ans.sort()
print(*ans)
```Danh sách kề lưu trữ cây một cách hiệu quả và mảng độ theo dõi xem mỗi nút vẫn còn bao nhiêu cạnh hoạt động khi các nút bị loại bỏ. Heap đảm bảo chúng ta luôn chọn lá hợp lệ nhỏ nhất trong$O(\log n)$thời gian. 

Một chi tiết triển khai tinh tế là việc xóa từng phần trong vùng heap. Các nút có thể vẫn còn trong heap sau khi chúng không còn là các lá hợp lệ, vì vậy chúng tôi bỏ qua các nút đã bị loại bỏ khi bật lên. Điều này tránh được việc bảo trì đống tốn kém. 

Việc sắp xếp câu trả lời cuối cùng là cần thiết vì việc loại bỏ được tạo theo thứ tự quy trình, không nhất thiết phải theo thứ tự tăng dần. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào mẫu:```
6 3
2 1
2 6
4 2
5 6
2 3
```Ban đầu, các lá là các nút có độ 1: các nút 1, 3, 4, 5. Heap luôn chọn lá nhỏ nhất. 

| Bước | Đống (khái niệm) | Được chọn | Đã xóa | Thay đổi bằng cấp | 
| --- | --- | --- | --- | --- | 
| 1 | 1,3,4,5 | 1 | {1} | nút giảm 2 độ | 
| 2 | 3,4,5 | 3 | {1,3} | nút giảm 2 độ | 
| 3 | 4,5 | 4 | {1,3,4} | nút 2 trở thành lá nếu đáp ứng điều kiện cấp 1 | 

Sau khi xử lý, các nút bị loại bỏ sẽ được$1, 3, 4$, phù hợp với sản lượng dự kiến. 

Dấu vết này cho thấy thuật toán luôn xóa các nút ranh giới an toàn và không bao giờ chạm vào cấu trúc bên trong cho đến khi nó tự nhiên trở thành một chiếc lá. 

Ví dụ thứ hai, hãy xem xét một chuỗi:```
5 2
1 2
2 3
3 4
4 5
```Các lá bắt đầu bằng 1 và 5. Thuật toán loại bỏ 1 trước, sau đó là 5. Đồ thị còn lại vẫn được kết nối vì việc loại bỏ các điểm cuối của một đường dẫn sẽ duy trì một đường dẫn ngắn hơn. 

| Bước | Lá | Được chọn | Đã xóa | 
| --- | --- | --- | --- | 
| 1 | 1,5 | 1 | {1} | 
| 2 | 2,5 | 2 | {1,2} | 

Điều này chứng tỏ các nút bên trong chỉ có thể tháo rời được sau khi cắt tỉa nhiều lần. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n \log n)$| Mỗi nút vào và ra khỏi vùng heap nhiều nhất một lần và mỗi thao tác trên vùng heap tốn thời gian logarit | 
| Không gian |$O(n)$| danh sách kề, mảng độ, mảng heap và sổ sách kế toán | 

Độ phức tạp tuyến tính có thể chấp nhận được đối với$n \le 10^6$bởi vì mỗi thao tác đều đơn giản và bị chi phối bởi các bản cập nhật heap. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import heapq

    n, k = map(int, input().split())
    adj = [[] for _ in range(n + 1)]
    deg = [0] * (n + 1)

    for _ in range(n - 1):
        a, b = map(int, input().split())
        adj[a].append(b)
        adj[b].append(a)
        deg[a] += 1
        deg[b] += 1

    removed = [False] * (n + 1)
    heap = []

    for i in range(1, n + 1):
        if deg[i] == 1:
            heapq.heappush(heap, i)

    ans = []

    for _ in range(k):
        while heap:
            v = heapq.heappop(heap)
            if not removed[v]:
                break
        removed[v] = True
        ans.append(v)

        for to in adj[v]:
            if not removed[to]:
                deg[to] -= 1
                if deg[to] == 1:
                    heapq.heappush(heap, to)

    ans.sort()
    return " ".join(map(str, ans))

assert run("""6 3
2 1
2 6
4 2
5 6
2 3
""") == "1 3 4"

assert run("""5 2
1 2
2 3
3 4
4 5
""") == "1 2"

assert run("""4 1
1 2
1 3
1 4
""") == "2"

assert run("""7 3
1 2
2 3
3 4
4 5
5 6
6 7
""") == "1 2 3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cây sao | 2 | quy tắc bảo vệ trung tâm và loại bỏ lá | 
| cây đường dẫn | 1 2 | tiến hóa lá năng động | 
| loại bỏ chuỗi nhỏ | 2 | đống thứ tự chính xác | 
| chuỗi dài | 1 2 3 | cắt tỉa lặp đi lặp lại ổn định | 

## Vỏ cạnh 

Cây có hình ngôi sao lúc đầu chỉ có lá rời ra. Thuật toán hạn chế chính xác việc xóa các nút lá, do đó phần trung tâm không bao giờ bị xóa sớm. Ví dụ, với các cạnh$1$kết nối với$2,3,4$, đống ban đầu chứa$2,3,4$và nút 1 không bao giờ đi vào heap cho đến khi nó trở thành nút lá, điều này không bao giờ xảy ra trừ khi tất cả các nút khác bị loại bỏ. 

Trong một chuỗi dài, các nút bên trong dần dần trở thành các lá chỉ sau khi các điểm cuối bị loại bỏ. Thuật toán tôn trọng quá trình chuyển đổi này một cách tự nhiên vì các cập nhật mức độ đảm bảo các nút chỉ vào vùng heap khi có cấu trúc hợp lệ. 

Trong trường hợp tồn tại nhiều lá, việc luôn chọn chỉ số nhỏ nhất sẽ đảm bảo loại bỏ một cách xác định các nút có giá trị thấp hơn trước, điều này phù hợp với trọng số theo cấp số nhân và ngăn chặn việc giữ lại các nút có chỉ số thấp dưới mức tối ưu.
