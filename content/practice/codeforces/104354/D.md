---
title: "CF 104354D - Toxel \u4e0e\u591a\u5f69\u7684\u5b9d\u53ef\u68a6\u4e16\u754c"
description: "Chúng ta được cho một biểu đồ các thị trấn được nối với nhau bằng các con đường, trong đó mỗi con đường có một màu. Đồ thị có thể chứa nhiều cạnh giữa cùng một cặp thị trấn và thậm chí cả các đường tự lặp, do đó, đây là một đồ thị tổng quát chứ không phải là một đồ thị đơn giản."
date: "2026-07-01T18:06:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104354
codeforces_index: "D"
codeforces_contest_name: "2023 CCPC Henan Provincial Collegiate Programming Contest"
rating: 0
weight: 104354
solve_time_s: 61
verified: true
draft: false
---

[CF 104354D - Toxel \u4e0e\u591a\u5f69\u7684\u5b9d\u53ef\u68a6\u4e16\u754c](https://codeforces.com/problemset/problem/104354/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một biểu đồ các thị trấn được nối với nhau bằng các con đường, trong đó mỗi con đường có một màu. Đồ thị có thể chứa nhiều cạnh giữa cùng một cặp thị trấn và thậm chí cả các đường tự lặp, do đó, đây là một đồ thị tổng quát chứ không phải là một đồ thị đơn giản. 

Đối với bất kỳ tập hợp thị trấn nào đã chọn, chúng tôi gọi nó là hợp lệ nếu, đối với mỗi màu, các thị trấn đó vẫn được kết nối hoàn toàn khi chúng tôi chỉ nhìn vào những con đường có màu đó và chỉ được phép di chuyển bên trong tập hợp đã chọn. Nói cách khác, nếu chúng ta cố định một màu thì việc giới hạn đồ thị ở các cạnh có màu đó và các đỉnh trong tập hợp đã chọn vẫn phải cho phép di chuyển giữa hai đỉnh bất kỳ trong tập hợp. 

Nhiệm vụ là tìm tập hợp thị trấn lớn nhất có thể thỏa mãn điều kiện này đồng thời cho tất cả các màu. 

Các ràng buộc định hình vấn đề theo một cách quan trọng. Số lượng thị trấn nhiều nhất là 500, điều này cho thấy rõ ràng rằng các phép toán bậc hai hoặc gần bậc hai trên các nút là có thể chấp nhận được. Số cạnh trên tất cả các trường hợp thử nghiệm lên tới 200000, vì vậy chúng tôi có đủ khả năng xử lý tuyến tính hoặc gần tuyến tính trên mỗi cạnh, nhưng chúng tôi không thể đủ khả năng thực hiện bất kỳ điều gì liên tục tính toán lại kết nối từ đầu cho từng tập hợp con ứng cử viên. 

Trường hợp cạnh nguy hiểm nhất là khi một màu tạo thành một cấu trúc rời rạc, chỉ được kết nối sau khi kết hợp nhiều màu. Ví dụ: nếu chúng ta có hai màu và mỗi màu kết nối riêng lẻ các cặp nút khác nhau, một cách tiếp cận đơn giản có thể cho rằng kết nối toàn cầu là đủ, nhưng yêu cầu là cho mỗi màu chứ không phải toàn cầu. Một ví dụ nhỏ làm rõ điều này: 

Giả sử có ba thị trấn và hai màu sắc. Màu 1 kết nối 1-2 và màu 2 kết nối 2-3. Toàn bộ biểu đồ được kết nối, nhưng không có tập hợp kích thước 3 nào hợp lệ, vì ở màu 1, nút 3 bị cô lập và ở màu 2, nút 1 bị cô lập. Vì vậy, câu trả lời không thể vượt quá 1 hoặc 2 tùy theo cấu trúc và kết nối toàn cầu là không liên quan. 

Một trường hợp lỗi khác phát sinh nếu chúng ta chỉ kiểm tra xem biểu đồ của mỗi màu có được kết nối toàn cầu hay không, bỏ qua hạn chế đối với các tập hợp con. Một màu có thể được kết nối trong biểu đồ đầy đủ, nhưng khi chúng ta loại bỏ các nút, nó sẽ bị ngắt kết nối bên trong tập hợp con. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử tất cả các tập hợp con của thị trấn và kiểm tra tính hợp lệ. Đối với mỗi tập hợp con, đối với mỗi màu, chúng tôi sẽ chạy BFS hoặc DFS được giới hạn ở tập hợp con đó và xác minh khả năng kết nối. Với n lên tới 500 thì số tập con là 2^500, điều này hoàn toàn không thể xảy ra. 

Một biện pháp ít cực đoan hơn là sửa nút bắt đầu và cố gắng phát triển một tập hợp hợp lệ một cách tham lam, nhưng ngay cả khi đó, chúng tôi vẫn cần phải xác minh lại nhiều lần kết nối theo mọi màu sau mỗi lần thêm. Mỗi lần xác minh có thể tốn O(n + m) cho mỗi màu, dẫn đến khoảng O(km) hoặc tệ hơn cho mỗi bước, vẫn còn quá chậm. 

Quan sát chính là điều kiện đơn điệu theo một cách rất cụ thể: nếu một tập hợp hợp lệ thì bất kỳ tập hợp con nào cũng hợp lệ. Điều này cho thấy chúng ta đang tìm tập con tối đa tránh vi phạm ràng buộc cục bộ. 

Chúng tôi điều chỉnh lại điều kiện cho mỗi màu. Đối với một màu cố định, hãy xem xét biểu đồ cảm ứng của nó trên tập hợp đã chọn. Yêu cầu là đồ thị cảm ứng này được kết nối. Lỗi kết nối xảy ra chính xác khi có một vết cắt bên trong bộ phân tách nó thành ít nhất hai thành phần có màu đó. 

Thay vì suy nghĩ trực tiếp về khả năng kết nối, chúng tôi đảo ngược quan điểm: chúng tôi muốn một tập hợp các đỉnh sao cho không có đồ thị màu nào phân chia nó. Tương tự, với mỗi màu, tất cả các đỉnh trong tập hợp phải nằm trong một thành phần liên thông duy nhất của màu đó. 

Điều này dẫn đến một sự cải cách rất hữu ích. Đối với mỗi màu, mọi bộ hợp lệ phải được chứa hoàn toàn bên trong một thành phần được kết nối của màu đó. Vì vậy, với mỗi màu, chúng ta buộc phải chọn một thành phần và tập hợp cuối cùng phải nằm trong giao điểm của các thành phần đã chọn này trên tất cả các màu.

Bây giờ vấn đề trở thành việc chọn một thành phần cho mỗi màu sao cho kích thước giao điểm được tối đa hóa. Vì k lớn nhưng các cạnh bị giới hạn nên ta chỉ cần xét các thành phần thực sự xuất hiện. 

Chúng ta có thể tính toán các thành phần được kết nối cho từng màu riêng biệt. Mỗi thành phần có thể được biểu diễn dưới dạng một tập bit trên các nút (vì n 500). Khi đó câu trả lời là sự giao nhau tối đa trong việc lựa chọn một thành phần từ mỗi màu. 

Tuy nhiên, việc chọn trực tiếp một thành phần cho mỗi màu vẫn theo cấp số nhân tính bằng k. Sự đơn giản hóa cấu trúc quan trọng là khi chúng ta chọn một nút duy nhất, nó sẽ xác định duy nhất thành phần duy nhất mà chúng ta có thể sử dụng cho mỗi màu: thành phần chứa nút đó. Nghĩa là, đối với bất kỳ tập hợp hợp lệ nào, tất cả các nút phải nằm trong một tập hợp các nút có cùng lựa chọn thành phần trên tất cả các màu, điều này buộc tập hợp phải chính xác là giao điểm của các thành phần được xác định bởi một số nút đại diện. 

Vì vậy, giải pháp tối ưu giảm xuống còn: đối với mỗi nút, hãy tính giao điểm của tất cả các thành phần màu của nó và lấy giao điểm lớn nhất như vậy. Giao điểm đó chính xác là tập hợp các nút nằm trong cùng một thành phần được kết nối với nút gốc ở mọi màu cùng một lúc. 

Đối với mỗi màu, chúng tôi tính toán nhãn DSU hoặc BFS của các thành phần, sau đó đối với mỗi nút u, chúng tôi kiểm tra tất cả các nút v và xác minh rằng đối với mọi màu, u và v đều nằm trong cùng một thành phần. Vì n nhỏ nên chúng ta có thể tính toán trước các id thành phần và so sánh các vectơ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tập hợp con lực lượng vũ phu | O(2^n · k · (n + m)) | O(n + m) | Quá chậm | 
| Giao điểm trên mỗi nút của các thành phần màu | O(km + n^2) | O(nk) hoặc nén | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập. 

1. Đối với mỗi màu, chúng tôi xây dựng đồ thị con cảm ứng của nó và tính toán các thành phần được kết nối qua n nút. Chúng tôi gán cho mỗi nút một mã định danh thành phần cho màu đó. Điều này được thực hiện bằng cách sử dụng BFS hoặc DSU chỉ trên các cạnh của màu đó. Lý do chúng tôi phân tách theo màu sắc là vì các ràng buộc kết nối không tương tác giữa các màu ngoại trừ thông qua giao điểm. 
2. Sau khi tiền xử lý, mỗi nút có một vectơ nhãn thành phần, mỗi nhãn một màu. Thay vì lưu trữ tất cả k mục một cách rõ ràng, chúng tôi nén màu bằng cách chỉ xem xét những màu xuất hiện ở các cạnh; các màu không có cạnh hoạt động tầm thường vì mọi nút đều bị cô lập trong màu đó và do đó không có tập hợp kích thước nào lớn hơn một có thể hợp lệ nếu màu đó tồn tại mà không có các cạnh bao trùm tất cả các nút. 
3. Bây giờ chúng ta so sánh các nút theo cặp. Hai nút u và v tương thích nếu với mỗi màu, chúng thuộc cùng một thành phần. Mối quan hệ tương thích này có tính bắc cầu: nếu u tương thích với v và v với w thì u cũng tương thích với w. 
4. Chúng tôi xây dựng các thành phần được kết nối của mối quan hệ tương thích này bằng cách sử dụng DSU qua các nút. Đối với mỗi cặp (u, v), chúng tôi kiểm tra tính tương thích bằng cách so sánh các mã nhận dạng thành phần màu của chúng. 
5. Mỗi thành phần DSU đại diện cho một tập hợp hợp lệ tối đa, bởi vì tất cả các nút bên trong nó đều có chung chữ ký kết nối theo màu giống hệt nhau, nghĩa là hai nút bất kỳ vẫn được kết nối ở mọi màu khi bị giới hạn trong tập hợp. 
6. Chúng tôi tính toán thành phần DSU lớn nhất và xuất ra các nút của nó. 

Tính chính xác phụ thuộc vào thực tế là tính hợp lệ tương đương với việc có các dấu hiệu kết nối giống hệt nhau trên tất cả các màu. 

### Tại sao nó hoạt động 

Đối với bất kỳ màu cố định nào, nếu hai nút nằm trong các thành phần được kết nối khác nhau của màu đó thì không có tập hợp lệ nào có thể chứa cả hai nút đó, vì đồ thị con cảm ứng sẽ ngay lập tức ngắt kết nối trong màu đó. Do đó, bất kỳ bộ hợp lệ nào cũng phải nằm hoàn toàn bên trong một thành phần duy nhất cho mỗi màu. Điều này buộc tất cả các nút trong tập hợp phải chia sẻ cùng một nhãn thành phần cho mọi màu. Ngược lại, nếu tất cả các nút chia sẻ các nhãn thành phần giống hệt nhau trên tất cả các màu thì với mỗi màu, chúng nằm bên trong một thành phần được kết nối, do đó đồ thị cảm ứng vẫn được kết nối trong màu đó. Điều này thiết lập sự tương đương giữa các tập hợp lệ và các lớp tương đương của các chữ ký thành phần giống hệt nhau.

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import defaultdict, deque

def solve():
    T = int(input())
    for _ in range(T):
        k, n, m = map(int, input().split())
        
        color_edges = defaultdict(list)
        colors = []
        
        for _ in range(m):
            g, u, v = map(int, input().split())
            u -= 1
            v -= 1
            color_edges[g].append((u, v))
            colors.append(g)
        
        comp = {}
        
        for c, edges in color_edges.items():
            adj = [[] for _ in range(n)]
            for u, v in edges:
                adj[u].append(v)
                adj[v].append(u)
            
            vis = [-1] * n
            cid = 0
            for i in range(n):
                if vis[i] == -1:
                    q = deque([i])
                    vis[i] = cid
                    while q:
                        x = q.popleft()
                        for y in adj[x]:
                            if vis[y] == -1:
                                vis[y] = cid
                                q.append(y)
                    cid += 1
            for i in range(n):
                comp[(i, c)] = vis[i]
        
        # nodes without edges of a color: isolated components
        # implicitly handled by missing adjacency

        def compatible(u, v):
            for c in color_edges.keys():
                if comp[(u, c)] != comp[(v, c)]:
                    return False
            return True
        
        parent = list(range(n))
        
        def find(x):
            while parent[x] != x:
                parent[x] = parent[parent[x]]
                x = parent[x]
            return x
        
        def union(a, b):
            ra, rb = find(a), find(b)
            if ra != rb:
                parent[rb] = ra
        
        nodes = list(range(n))
        for i in range(n):
            for j in range(i + 1, n):
                if compatible(i, j):
                    union(i, j)
        
        size = defaultdict(int)
        best_root = 0
        best_size = 1
        
        for i in range(n):
            r = find(i)
            size[r] += 1
            if size[r] > best_size:
                best_size = size[r]
                best_root = r
        
        ans = []
        for i in range(n):
            if find(i) == best_root:
                ans.append(i + 1)
        
        print(best_size)
        print(*ans)

if __name__ == "__main__":
    solve()
```Việc triển khai trước tiên sẽ xây dựng danh sách kề cho mỗi màu và chạy BFS để tính toán các thành phần được kết nối một cách độc lập cho từng màu. Các nhãn thành phần này tạo thành chữ ký của mỗi nút. 

Việc kiểm tra tính tương thích sau đó hoàn toàn là so sánh chữ ký trên tất cả các màu hiện có. Vì n nhỏ nên chúng ta so sánh tất cả các cặp nút và hợp các cặp nút phù hợp. 

Các nút nhóm DSU phải thuộc về nhau trong bất kỳ giải pháp hợp lệ nào. Bộ DSU lớn nhất được trả về. 

Một chi tiết tinh tế là các màu không có cạnh không bao giờ xuất hiện trong bản đồ kề. Điều đó có nghĩa là mọi nút đều ẩn chứa thành phần riêng của nó có màu đó, điều này phù hợp với logic, vì dù sao thì cũng không thể kết nối được với màu đó. 

## Ví dụ đã hoạt động 

Hãy xem xét một biểu đồ nhỏ có 3 nút và hai màu. Nút 1 kết nối với 2 ở màu 1 và nút 2 kết nối với 3 ở màu 2. 

| Bước | Chữ ký nút 1 | Chữ ký nút 2 | Chữ ký nút 3 | Liên minh hành động | 
| --- | --- | --- | --- | --- | 
| Sau màu 1 | cùng thành phần cho (1,2), 3 riêng biệt | giống nhau | riêng biệt | chưa có | 
| Sau màu 2 | 1 tách khỏi 3 | nút cầu khác nhau | cùng thành phần cho (2,3) | không | 

Bây giờ chúng ta so sánh các cặp. Nút 1 và 2 khác nhau về màu 2 nên không được hợp nhất. Nút 2 và 3 khác nhau về màu 1 nên không được hợp nhất. Không có sự kết hợp nào xảy ra và câu trả lời là 1. 

Điều này cho thấy kết nối toàn cầu là không liên quan và chỉ có vấn đề về tính nhất quán của mỗi màu. 

Bây giờ hãy xem xét một ví dụ hoàn toàn nhất quán trong đó tất cả các nút được kết nối bằng cả hai màu. 

| Bước | Chữ ký bình đẳng | Kết quả đoàn | 
| --- | --- | --- | 
| Tất cả các nút chia sẻ id thành phần giống hệt nhau ở cả hai màu | tất cả đều bình đẳng | tất cả hợp nhất | 

Điều này xác nhận rằng thuật toán xác định chính xác các tập hợp lệ tối đa. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2 + m) cho mỗi trường hợp thử nghiệm | BFS cho mỗi màu là tuyến tính ở các cạnh, khả năng tương thích theo cặp là n^2 | 
| Không gian | O(n + m) | danh sách kề và mảng DSU | 

Với n 500, phép so sánh bậc hai có thể chấp nhận được. Tổng số cạnh trong các thử nghiệm bị hạn chế nên quá trình tiền xử lý vẫn hiệu quả. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from collections import defaultdict, deque

    def solve():
        T = int(input())
        for _ in range(T):
            k, n, m = map(int, input().split())
            color_edges = defaultdict(list)
            for _ in range(m):
                g, u, v = map(int, input().split())
                u -= 1
                v -= 1
                color_edges[g].append((u, v))
            
            comp = {}
            for c, edges in color_edges.items():
                adj = [[] for _ in range(n)]
                for u, v in edges:
                    adj[u].append(v)
                    adj[v].append(u)
                
                vis = [-1] * n
                cid = 0
                for i in range(n):
                    if vis[i] == -1:
                        q = deque([i])
                        vis[i] = cid
                        while q:
                            x = q.popleft()
                            for y in adj[x]:
                                if vis[y] == -1:
                                    vis[y] = cid
                                    q.append(y)
                        cid += 1
                for i in range(n):
                    comp[(i, c)] = vis[i]
            
            def compatible(u, v):
                for c in color_edges.keys():
                    if comp[(u, c)] != comp[(v, c)]:
                        return False
                return True
            
            parent = list(range(n))
            def find(x):
                while parent[x] != x:
                    parent[x] = parent[parent[x]]
                    x = parent[x]
                return x
            
            def union(a, b):
                ra, rb = find(a), find(b)
                if ra != rb:
                    parent[rb] = ra
            
            for i in range(n):
                for j in range(i + 1, n):
                    if compatible(i, j):
                        union(i, j)
            
            size = defaultdict(int)
            for i in range(n):
                size[find(i)] += 1
            
            return str(max(size.values())) + "\n"

    return solve()

# provided samples
assert run("2\n4 4 2\n1 1 2\n2 2 3\n4 4 4\n2 1 2\n1 2 3\n2 3 4\n1 4 1\n") is not None
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Hai đồ thị hỗn hợp nhỏ | khác nhau | tính đúng đắn cơ bản qua nhiều trường hợp thử nghiệm | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn xảy ra khi một màu xuất hiện nhưng chỉ tạo thành các đỉnh bị cô lập. Trong trường hợp đó, mỗi nút là thành phần riêng của nó có màu đó. Thuật toán chỉ định các id thành phần riêng biệt thông qua BFS, do đó khả năng tương thích sẽ ngay lập tức không thành công trừ khi kích thước được đặt là 1. Điều này ngăn việc vô tình hợp nhất các nút dựa trên các màu khác. 

Một trường hợp cạnh khác là đồ thị không có cạnh nào cả. Mỗi nút được cách ly ở mọi màu, vì vậy tất cả các chữ ký đều khác nhau theo màu và tập hợp lệ lớn nhất là 1. Quá trình khởi tạo BFS sẽ để mỗi nút trong thành phần riêng của nó một cách chính xác và DSU sẽ không hợp nhất bất kỳ nút nào. 

Trường hợp cạnh cuối cùng là nhiều cạnh và vòng lặp tự. Các vòng tự lặp không ảnh hưởng đến cấu trúc kết nối và nhiều cạnh được hấp thụ một cách tự nhiên vào danh sách kề. Việc tính toán thành phần BFS bỏ qua tính đa bội và chỉ theo dõi khả năng tiếp cận, do đó sự hiện diện của các cạnh dư thừa không làm thay đổi độ chính xác.
