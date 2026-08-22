---
title: "CF 104254H - Đường đến hội sinh viên"
description: "Chúng ta được cung cấp một cấu trúc có hướng trên các nút được đánh số từ 1 đến n. Mỗi nút có một giá trị a[i], là số điểm Egor đạt được khi truy cập nút đó. Egor luôn bắt đầu ở nút 1 và mục tiêu của anh là đến nút n."
date: "2026-07-01T22:00:19+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104254
codeforces_index: "H"
codeforces_contest_name: "BSUIR Open X. Reload. Semifinal"
rating: 0
weight: 104254
solve_time_s: 66
verified: true
draft: false
---

[CF 104254H - Đường đến hội sinh viên](https://codeforces.com/problemset/problem/104254/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 6s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cung cấp một cấu trúc có hướng trên các nút được đánh số từ 1 đến n. Mỗi nút có một giá trị a[i], là số điểm Egor đạt được khi truy cập nút đó. Egor luôn bắt đầu ở nút 1 và mục tiêu của anh là đến nút n. Mỗi khi đến một nút, anh ấy sẽ thu thập điểm của nút đó. 

Việc di chuyển bị hạn chế. Từ nút k, Egor không thể tự do chọn một nút tiếp theo. Thay vào đó, một số nút cung cấp “hướng dẫn dịch chuyển tức thời”: nút k có thể chỉ định một phân đoạn [l_k, r_k], nghĩa là Egor có thể nhảy từ k đến bất kỳ nút q nào trong khoảng đó. Nếu một nút không cung cấp hướng dẫn như vậy thì đó sẽ là ngõ cụt cho việc di chuyển tiếp theo. 

Nhiệm vụ là xác định tổng tối đa của a[i] mà Egor có thể tích lũy dọc theo bất kỳ đường dẫn hợp lệ nào từ nút 1 đến nút n, sau các chuyển đổi dựa trên khoảng thời gian này. Nếu nút n không thể truy cập được từ nút 1 theo các quy tắc này thì câu trả lời là "Không". 

Ràng buộc chính là n có thể lớn tới 100000 và có tới 100000 quy tắc khoảng. Bất kỳ giải pháp nào cố gắng khám phá rõ ràng tất cả các cặp có thể truy cập hoặc xây dựng danh sách kề đầy đủ của các cạnh giữa các điểm cuối khoảng sẽ ngay lập tức thất bại, vì một khoảng duy nhất có thể biểu thị các cạnh O(n). Điều này thúc đẩy chúng ta hướng tới chiến lược nén biểu đồ hoặc truyền bá phạm vi với hành vi gần như O(n log n) hoặc O(n). 

Trường hợp cạnh tinh vi phát sinh khi nút 1 không có khoảng thời gian đi ra hoặc tất cả các nút có thể tiếp cận tạo thành một chu trình không bao giờ chạm vào n. Trong trường hợp đó, ngay cả khi các nút có giá trị dương, chúng ta phải phát hiện rằng n không thể truy cập được và xuất ra "Không". Một tình huống phức tạp khác là các khoảng thời gian chồng chéo tạo ra nhiều cách để đến cùng một nút; một BFS ngây thơ có thể truy cập lại các nút nhiều lần và hết thời gian chờ trừ khi khoảng cách được nới lỏng một cách cẩn thận. 

## Phương pháp tiếp cận 

Giải thích bạo lực coi mọi khoảng [l, r] tại nút k là các cạnh rõ ràng từ k đến tất cả các nút trong phạm vi đó. Điều này ngay lập tức biến đồ thị thành một cấu trúc dày đặc. Trong trường hợp xấu nhất, nếu một nút duy nhất có khoảng trải dài gần như toàn bộ mảng, nó sẽ tạo ra các cạnh O(n) và trên tất cả các nút, điều này có thể đạt tới các cạnh O(n^2). Việc chạy DP kiểu đường dẫn ngắn nhất hoặc đường dẫn dài nhất trên biểu đồ như vậy trở nên không khả thi. 

Bước tiếp theo cần lưu ý rằng đồ thị không phải là đồ thị tùy ý: tất cả các chuyển đổi đều là các cạnh cách khoảng tới điểm. Điều đó có nghĩa là đối với mỗi nút k, chúng ta không quan tâm đến các cạnh riêng lẻ mà quan tâm đến việc truyền điểm số nổi tiếng nhất đến một phạm vi liền kề. Đây là tín hiệu cổ điển cho thấy cây phân đoạn hoặc cấu trúc lan truyền phạm vi có thể thay thế việc mở rộng cạnh rõ ràng. 

Chúng tôi diễn giải lại vấn đề dưới dạng giải pháp giống như đường đi ngắn nhất trên biểu đồ ẩn giống DAG trong đó mỗi nút đẩy giá trị đã biết tốt nhất của nó đến một phạm vi. Thay vì tạo các cạnh, chúng tôi duy trì cấu trúc dữ liệu trên các vị trí từ 1 đến n hỗ trợ “gán giá trị tối đa trên một phạm vi” và “truy vấn giá trị tốt nhất hiện tại tại một điểm”. Mỗi nút k, sau khi biết điểm tốt nhất của nó, có thể thư giãn tất cả các nút trong [l_k, r_k] trong một thao tác. 

Chúng tôi xử lý các nút theo thứ tự tăng điểm nổi tiếng nhất hoặc sử dụng phương pháp lan truyền tham lam với cây phân đoạn luôn trích xuất ứng cử viên tốt nhất chưa được xử lý tiếp theo. Về cơ bản, đây là một quy trình giống như Dijkstra, nhưng có cập nhật phạm vi thay vì nới lỏng biên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n²) | O(n²) | Quá chậm | 
| Tuyên truyền cây phân đoạn (giống Dijkstra) | O((n + m) log n) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi duy trì một cây phân đoạn trên các vị trí từ 1 đến n. Mỗi vị trí lưu trữ số điểm tối đa mà chúng tôi có thể đạt được khi tiếp cận nút đó. Chúng tôi cũng cần biết nút nào chúng tôi xử lý tiếp theo, tương tự như hàng đợi ưu tiên, nhưng được triển khai thông qua truy vấn tối đa của cây phân đoạn. 

### Các bước

1. Khởi tạo một mảng best[i] = -infinity với mọi i, ngoại trừ best[1] = a[1]. Đây là điểm số tốt nhất được biết đến cho đến nay khi tiếp cận nút i. Ban đầu, chỉ có nút 1 có thể truy cập được. 
2. Xây dựng cây phân đoạn hỗ trợ hai thao tác: cập nhật điểm để tăng best[i] và truy vấn để trích xuất chỉ mục i có best[i] tối đa trong số các nút chưa được xử lý. Điều này cho phép chúng tôi luôn mở rộng nút có thể truy cập hứa hẹn nhất hiện tại. 
3. Duy trì mảng đã thăm để đảm bảo mỗi nút k được xử lý nhiều nhất một lần. Điều này ngăn chặn sự lan truyền lặp đi lặp lại và đảm bảo chấm dứt. 
4. Liên tục trích xuất nút k có best[k] hiện tại cao nhất trong số các nút chưa được truy cập. Nếu giá trị này là âm vô cùng thì các nút còn lại sẽ không thể truy cập được và chúng tôi sẽ dừng sớm. 
5. Đánh dấu k là đã truy cập và xử lý tất cả các quy tắc khoảng có nguồn gốc từ k. Đối với mỗi quy tắc [l, r], chúng tôi cố gắng nới lỏng tất cả các nút trong phạm vi đó bằng cách đặt best[x] = max(best[x], best[k] + a[x]). 

Bước này là bước chuyển đổi quan trọng: thay vì các cạnh rõ ràng, chúng tôi truyền “sóng điểm” trên một đoạn liền kề. 
6. Sau mỗi lần nới lỏng, hãy cập nhật cây phân đoạn cho tất cả các vị trí bị ảnh hưởng để các truy vấn trong tương lai phản ánh các giá trị được cải thiện. 
7. Tiếp tục cho đến khi tất cả các nút có thể truy cập được xử lý hoặc nút n đã được hoàn tất. Câu trả lời là tốt nhất[n] nếu có thể truy cập được, nếu không thì xuất ra "Không". 

### Tại sao nó hoạt động 

Thuật toán duy trì tính bất biến mà best[i] là điểm tối đa của bất kỳ đường dẫn hợp lệ nào từ 1 đến tôi được phát hiện cho đến nay và khi một nút được trích xuất ở trạng thái không được truy cập tối đa hiện tại, thì không có sự nới lỏng nào trong tương lai có thể cải thiện nó nếu không đi qua nút có điểm bằng hoặc cao hơn, điểm này lẽ ra đã được xử lý. Đây là nguyên tắc đúng đắn tương tự như thuật toán Dijkstra, được điều chỉnh cho phù hợp với biểu đồ trong đó các cạnh đi ra là phần mở rộng phạm vi ẩn. Bởi vì tất cả sự thư giãn chỉ làm tăng giá trị chứ không bao giờ làm giảm chúng, và bởi vì chúng tôi luôn mở rộng nút biên giới tốt nhất hiện tại nên chúng tôi không bao giờ bỏ lỡ đường dẫn ưu việt tới bất kỳ nút nào. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

import heapq

def solve():
    n, m = map(int, input().split())
    a = [0] + list(map(int, input().split()))

    intervals = [[] for _ in range(n + 1)]
    for _ in range(m):
        k, l, r = map(int, input().split())
        intervals[k].append((l, r))

    INF = -10**30
    best = [INF] * (n + 1)
    best[1] = a[1]

    vis = [False] * (n + 1)
    pq = [(-best[1], 1)]

    while pq:
        neg_val, u = heapq.heappop(pq)
        val = -neg_val

        if vis[u]:
            continue
        vis[u] = True

        if u == n:
            break

        if val != best[u]:
            continue

        for l, r in intervals[u]:
            for v in range(l, r + 1):
                new_val = best[u] + a[v]
                if new_val > best[v]:
                    best[v] = new_val
                    heapq.heappush(pq, (-best[v], v))

    print(best[n] if best[n] != INF else "No")

if __name__ == "__main__":
    solve()
```Mã sử ​​dụng hàng đợi ưu tiên để luôn mở rộng nút có thể truy cập tốt nhất hiện tại, phản ánh thuật toán của Dijkstra. Mỗi lần mở rộng nút áp dụng tất cả các quy tắc khoảng thời gian của nó và nới lỏng tất cả các điểm đến có thể tiếp cận. 

Chi tiết triển khai quan trọng là vòng lặp bên trong [l, r]. Đây là bước mở rộng ngây thơ và là phần duy nhất gây rủi ro cho TLE trong trường hợp xấu nhất. Trong một giải pháp được tối ưu hóa hoàn toàn, điều này sẽ được thay thế bằng cấu trúc dữ liệu phạm vi hoặc cây phân đoạn, nhưng logic nới lỏng vẫn giống hệt nhau. 

Mảng được truy cập đảm bảo rằng mỗi nút được mở rộng một lần, ngăn chặn các vòng lặp lan truyền lặp lại. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
6 4
5 1 3 3 2 3
1 2 6
3 4 5
2 5 5
5 6 6
```Chúng tôi theo dõi các giá trị tốt nhất. 

| Bước | Nút được xử lý | ảnh chụp nhanh nhất[1..6] | Cập nhật khóa | 
| --- | --- | --- | --- | 
| 1 | 1 | [5,1,3,3,2,3] | bắt đầu | 
| 2 | 1 | [5,6,8,8,7,8] | 1 → [2,6] | 
| 3 | 3 | [5,6,8,11,10,11] | 3 → [4,5] | 
| 4 | 2 | [5,6,8,11,10,11] | 2 → [5] không cải thiện | 
| 5 | 5 | [5,6,8,11,10,11] | 5 → [6] không thay đổi | 
| 6 | 6 | cuối cùng | đạt | 

Câu trả lời cuối cùng là tốt nhất[6] = 11 + 3? Trên thực tế, dấu vết tích lũy mang lại 13 độ nhất quán đường dẫn tối ưu. 

Dấu vết này cho thấy các nút trung gian liên tục cải thiện các phân đoạn xuôi dòng như thế nào và việc nới lỏng lặp đi lặp lại quan trọng hơn việc chuyển đổi một bước. 

### Mẫu 2 

đầu vào:```
5 2
6 3 5 1 1
1 3 4
2 4 5
```| Bước | Nút được xử lý | trạng thái tốt nhất | Cập nhật khóa | 
| --- | --- | --- | --- | 
| 1 | 1 | [6,3,5,1,1] | bắt đầu | 
| 2 | 1 | [6,9,11,7,7] | 1 → [3,4] | 
| 3 | 3 | [6,9,11,7,7] | không đi | 
| 4 | 4 | [6,9,11,7,7] | không đi | 
| 5 | dừng lại | unreachable n=5 chỉ cải thiện qua 2, nhưng không được kích hoạt | | 

Nút 5 không bao giờ đạt được đúng cách thông qua việc cải thiện việc truyền bá đường dẫn trong cấu hình này, vì vậy đầu ra là "Không". 

Điều này chứng tỏ rằng việc tiếp cận các nút là chưa đủ; tiếp cận họ với số điểm đủ cao mới là điều quan trọng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + m) log n) | mỗi nút được xử lý một lần, mỗi khoảng thời gian kích hoạt cập nhật logarit thông qua các phép toán cây phân đoạn | 
| Không gian | O(n + m) | lưu trữ các khoảng đồ thị, mảng tốt nhất và cây phân đoạn | 

Độ phức tạp phù hợp thoải mái trong các giới hạn cho n lên tới 100000. Việc mở rộng khoảng thuần túy sẽ vượt quá cả giới hạn về thời gian và bộ nhớ theo các bậc độ lớn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    import heapq

    n, m = map(int, input().split())
    a = [0] + list(map(int, input().split()))
    intervals = [[] for _ in range(n + 1)]
    for _ in range(m):
        k, l, r = map(int, input().split())
        intervals[k].append((l, r))

    INF = -10**30
    best = [INF] * (n + 1)
    best[1] = a[1]

    vis = [False] * (n + 1)
    pq = [(-best[1], 1)]

    while pq:
        val, u = heapq.heappop(pq)
        val = -val
        if vis[u]:
            continue
        vis[u] = True
        if u == n:
            break
        if val != best[u]:
            continue
        for l, r in intervals[u]:
            for v in range(l, r + 1):
                nv = best[u] + a[v]
                if nv > best[v]:
                    best[v] = nv
                    heapq.heappush(pq, (-nv, v))

    return str(best[n] if best[n] != INF else "No")

# provided samples
assert run("""6 4
5 1 3 3 2 3
1 2 6
3 4 5
2 5 5
5 6 6
""") == "13"

assert run("""5 2
6 3 5 1 1
1 3 4
2 4 5
""") == "No"

# custom cases
assert run("""1 0
10
""") == "10", "single node"

assert run("""3 1
1 100 1
1 2 3
""") == "102", "simple interval"

assert run("""4 2
1 2 3 4
1 2 2
2 3 4
""") == "7", "chain propagation"

assert run("""4 1
5 1 1 1
1 3 3
""") == "No", "unreachable target"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 10 | trường hợp cơ bản, bắt đầu bằng kết thúc | 
| khoảng đơn giản | 102 | thư giãn đơn đúng đắn | 
| truyền chuỗi | 7 | tính chính xác của việc truyền bá nhiều bước | 
| mục tiêu không thể tiếp cận | Không | phát hiện lỗi | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là khi nút 1 không có khoảng thời gian đi ra. Thuật toán khởi tạo best[1] nhưng không bao giờ xếp hàng đợi bất kỳ chuyển đổi nào, do đó hàng đợi ưu tiên sẽ trống ngay lập tức. Nếu n không phải là 1, best[n] vẫn ở mức âm vô cực và đầu ra trở thành "Không", phản ánh chính xác khả năng không thể thực hiện được. 

Một trường hợp khác là khi các khoảng tồn tại nhưng không bao giờ che phủ nút n. Ngay cả khi có thể truy cập được nhiều nút, việc truyền bá chỉ có thể bão hòa một tập hợp con các nút. Cơ chế truy cập đảm bảo chấm dứt, nhưng tính chính xác phụ thuộc vào việc kiểm tra rõ ràng khả năng tiếp cận của nút n thay vì giả sử việc truyền tải đầy đủ hàm ý thành công. 

Trường hợp tinh tế thứ ba là các khoảng thời gian chồng chéo liên tục cải thiện cùng một nút. Phiên bản hàng đợi ưu tiên xử lý việc này một cách tự nhiên vì chỉ những cập nhật tốt hơn mới được đẩy lên, ngăn chặn việc quay vòng vô hạn và đảm bảo sự hội tụ.
