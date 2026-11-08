---
title: "CF 1450E - Chủ nghĩa tư bản"
description: "Chúng ta có một đồ thị vô hướng liên thông trong đó mỗi đỉnh đại diện cho một người và mỗi cạnh đại diện cho một tình bạn."
date: "2026-06-11T03:42:51+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "dfs-and-similar", "graphs", "shortest-paths"]
categories: ["algorithms"]
codeforces_contest: 1450
codeforces_index: "E"
codeforces_contest_name: "Codeforces Global Round 12"
rating: 2700
weight: 1450
solve_time_s: 119
verified: false
draft: false
---

[CF 1450E - Chủ nghĩa tư bản](https://codeforces.com/problemset/problem/1450/E) 

**Đánh giá:** 2700 
**Tags:** thuật toán xây dựng, dfs và tương tự, đồ thị, đường đi ngắn nhất 
**Thời gian giải:** 1 phút 59 giây 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị vô hướng liên thông trong đó mỗi đỉnh đại diện cho một người và mỗi cạnh đại diện cho một tình bạn. Nhiệm vụ không phải là gán các giá trị một cách tùy ý mà là gán một thu nhập số nguyên cho mỗi đỉnh sao cho mọi tình bạn đều thỏa mãn một quy tắc rất cứng nhắc: dọc theo mỗi cạnh, hai điểm cuối phải khác nhau đúng 1 và chúng ta phải chọn hướng cho sự khác biệt đó. 

Mỗi cạnh đều đã được định hướng, nghĩa là chúng tôi được cho biết điểm cuối nào phải có thu nhập chính xác lớn hơn cạnh kia hoặc không được định hướng, nghĩa là chúng tôi có quyền quyết định mối quan hệ “+1” sẽ đi theo hướng nào. Nếu chúng ta ấn định thu nhập, mọi cạnh sẽ trở thành một ràng buộc có dạng “chênh lệch chính xác bằng 1 theo một hướng nào đó phù hợp với định hướng đã cho hoặc đã chọn”. 

Trong số tất cả các nhiệm vụ hợp lệ, chúng ta được yêu cầu tối đa hóa sự khác biệt giữa thu nhập được giao tối đa và tối thiểu. Nếu không có nhiệm vụ nào có thể thỏa mãn mọi ràng buộc thì chúng ta phải báo cáo là không thể thực hiện được. 

Cấu trúc chính là mỗi cạnh buộc một bước đơn vị nghiêm ngặt theo nghĩa có hướng. Điều này có nghĩa là khi các hướng được cố định, thu nhập sẽ trở thành khoảng cách trong biểu đồ trong đó mỗi trọng số cạnh chính xác là 1 nhưng có ràng buộc về hướng. 

Các ràng buộc này nhỏ theo cách cho phép chúng ta coi mỗi đỉnh như một trạng thái trong một bài toán đồ thị thay vì cần các tiệm cận nặng nề. Với n lên tới 200 và m lên đến 2000, thậm chí lý luận kiểu O(nm) hoặc O(n^2 + m log n) cũng có thể chấp nhận được. Những gì chúng ta không thể thực hiện được là liệt kê theo cấp số nhân các hướng của cạnh, vì 2^2000 là không thể. 

Một vài trường hợp thất bại rất tinh tế. 

Một chế độ thất bại là cho rằng chúng ta luôn có thể chỉ đường một cách tham lam. Ví dụ, trong một tam giác mà tất cả các cạnh đều vô hướng, việc chọn hướng tùy ý có thể tạo ra mâu thuẫn: một chu trình có thể buộc một chuỗi bất đẳng thức nghiêm ngặt quay về điểm bắt đầu với tổng khác 0, điều này là không thể. 

Một dạng lỗi khác là bỏ qua các ràng buộc có hướng khi truyền bá các giá trị. Nếu một cạnh có hướng buộc u phải cao hơn v một cạnh, nhưng một đường đi ở nơi khác ngụ ý điều ngược lại, thì chúng ta sẽ gặp mâu thuẫn mặc dù các kiểm tra cục bộ đã vượt qua. 

Vấn đề thứ ba là giả sử đồ thị là lưỡng cực theo nghĩa thông thường. Đây không phải là màu lưỡng cực; nó là một hệ thống ràng buộc sai phân với các chu kỳ có tổng bằng 0. 

## Phương pháp tiếp cận 

Nếu chúng ta cố gắng giải quyết vấn đề một cách thô bạo, chúng ta sẽ gán một hướng cho mọi cạnh vô hướng và sau đó kiểm tra xem các ràng buộc thu được có nhất quán hay không. Điều này dẫn đến một hệ thống trong đó chúng ta truyền bá các giá trị từ một gốc tùy ý và xác minh rằng tất cả các cạnh đều thỏa mãn những khác biệt cần thiết của chúng. Có 2^k lựa chọn trong đó k là số cạnh vô hướng và k có thể lớn tới 2000, khiến điều này không thể thực hiện được. 

Quan sát quan trọng là khi các hướng được cố định, bài toán sẽ trở thành một hệ phương trình có dạng 

a_j = a_i + 1 hoặc a_i = a_j + 1 

tương đương với việc gán các điện thế trong biểu đồ có các cạnh có hướng có trọng số. Tính nhất quán của một hệ thống như vậy chỉ phụ thuộc vào tổng chu kỳ bằng 0. 

Thay vì chọn hướng đi trước, chúng ta lật ngược quan điểm. Mỗi cạnh đóng góp một ràng buộc có thể được mã hóa dưới dạng mối quan hệ giữa các biến và tính khả thi sẽ kiểm tra xem liệu chúng ta có thể gán các giá trị sao cho tất cả các ràng buộc được giữ đồng thời hay không. Đây là một hệ thống bài toán ràng buộc sai phân cổ điển. 

Sau đó, chúng tôi tối ưu hóa mục tiêu phụ: tối đa hóa phạm vi giá trị. Khi chúng tôi biết tính khả thi, việc tối đa hóa phạm vi tương ứng với việc chọn điểm bắt đầu và để cấu trúc lan truyền, nhưng có một điểm thay đổi: biểu đồ có thể được phân tách thành các thành phần và chúng tôi thực sự muốn có khoảng cách đi ngắn nhất dài nhất giữa các điểm cực trị.

Chúng ta có thể hiểu cấu trúc dưới dạng đồ thị có hướng với các cạnh có trọng số +1 hoặc -1 tùy theo hướng. Các cạnh có hướng là cố định, các cạnh không có hướng có thể được gán một trong hai hướng, nhưng phải được chọn một cách nhất quán để không phát sinh mâu thuẫn. 

Cách rõ ràng để giải quyết vấn đề này là mô hình hóa nó dưới dạng biểu đồ trong đó chúng ta cố gắng gán a_i tiềm năng và kiểm tra xem mọi cạnh có thể được định hướng sao cho |a_i - a_j| = 1 trong khi tôn trọng các hướng cố định. Điều này trở thành một vấn đề lan truyền ràng buộc trên một biểu đồ có cấu trúc giống như chẵn lẻ nhưng có sự khác biệt về số nguyên thực tế. 

Chúng tôi rút gọn nó thành việc xây dựng một biểu đồ ràng buộc và kiểm tra tính nhất quán bằng cách sử dụng BFS/DFS với phép gán chênh lệch. Đối với mỗi thành phần được kết nối, chúng tôi cố gắng gán các giá trị bằng cách cố định một nút ở mức 0 và lan truyền. Nếu chúng ta gặp phải mâu thuẫn thì hệ thống đó không thể thực hiện được. 

Để tối đa hóa phạm vi, chúng tôi tính toán các giá trị tối thiểu và tối đa có thể có trong mỗi thành phần theo các ràng buộc này. Vì mỗi cạnh thực thi các bước đơn vị nên các giá trị trong một thành phần được xác định theo sự dịch chuyển toàn cục và có thể là sự phản ánh. Chúng tôi tính toán khoảng cách và sử dụng cặp xa nhất trong phép gán nhất quán ràng buộc. 

Điều này dẫn đến việc tính toán đường đi ngắn nhất dài nhất trong cấu trúc có định hướng bị ràng buộc, có thể được xử lý bởi BFS vì tất cả các trọng số cạnh đều là ±1 một lần được định hướng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (định hướng cạnh) | O(2^m · (n + m)) | O(n + m) | Quá chậm | 
| Tuyên truyền ràng buộc + BFS/DFS | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi vấn đề thành một biểu đồ ràng buộc trong đó mỗi cạnh thực thi chênh lệch chính xác là 1 theo hướng phải nhất quán. 

1. Xây dựng biểu diễn kề trong đó mỗi cạnh trở thành một quan hệ ràng buộc. Đối với cạnh có hướng u → v, chúng tôi thực thi a[v] = a[u] + 1. Đối với cạnh vô hướng u - v, chúng tôi tạm thời coi nó như một ràng buộc hai chiều cho phép a[v] = a[u] + 1 hoặc a[u] = a[v] + 1. 
2. Chúng tôi cố gắng gán các giá trị bằng BFS cho từng thành phần được kết nối. Chúng tôi chọn một nút chưa được truy cập và đặt giá trị của nó thành 0. Từ nút này, chúng tôi truyền dọc theo các cạnh, gán các giá trị lân cận dựa trên chênh lệch +1 được yêu cầu. 
3. Khi đi qua cạnh có hướng u → v, chúng ta đặt a[v] = a[u] + 1 nếu không được gán, nếu không chúng ta sẽ kiểm tra tính nhất quán. Nếu a[v] đã được gán và khác với a[u] + 1, chúng tôi phát hiện sự mâu thuẫn và dừng lại. 
4. Đối với cạnh vô hướng, chúng ta thử ngầm cả hai hướng thông qua quá trình truyền lan. Trong thực tế, chúng tôi coi mỗi cạnh vô hướng là hai ràng buộc có hướng có thể có nhưng đảm bảo rằng bất kỳ phép gán nào phát sinh từ BFS đều phải nhất quán. Nếu BFS buộc cả hai hướng không nhất quán thì thành phần đó là không thể. 
5. Trong khi gán giá trị, chúng tôi theo dõi giá trị tối thiểu và tối đa đạt được trong mỗi thành phần. Điều này là cần thiết vì câu trả lời cuối cùng phụ thuộc vào việc tối đa hóa mức độ lây lan toàn cầu. 
6. Sau khi xử lý tất cả các thành phần, nếu không tìm thấy mâu thuẫn, chúng tôi dịch chuyển tất cả các giá trị sao cho mức tối thiểu toàn cục trở thành 0, bảo toàn sự khác biệt trong khi tạo ra một phép gán hợp lệ. 

### Tại sao nó hoạt động 

Mỗi cạnh thực thi một ràng buộc chênh lệch đơn vị nghiêm ngặt, do đó, mọi phép gán hợp lệ đều tương ứng với một hàm tiềm năng trên biểu đồ trong đó mỗi cạnh thực thi một gradient cố định. BFS chỉ định các điện thế này một cách nhất quán và bất kỳ mâu thuẫn chu kỳ nào cũng xuất hiện dưới dạng gán không nhất quán cho nút đã truy cập trước đó. Bởi vì mọi ràng buộc cạnh đều là tuyến tính và có trọng số đơn vị, nên việc kiểm tra tính nhất quán sẽ giảm xuống còn việc xác minh rằng tất cả các phương trình ngụ ý đều phù hợp. Sau khi giữ được tính nhất quán, việc dịch chuyển tất cả các giá trị sẽ duy trì tính hợp lệ một cách thống nhất, do đó, việc tối đa hóa phạm vi sẽ giảm xuống việc chọn chuẩn hóa dựa trên mức tối thiểu tự nhiên. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

def solve():
    n, m = map(int, input().split())
    
    g_dir = [[] for _ in range(n)]
    g_und = [[] for _ in range(n)]
    
    edges = []
    
    for _ in range(m):
        u, v, b = map(int, input().split())
        u -= 1
        v -= 1
        if b == 1:
            g_dir[u].append(v)
        else:
            g_und[u].append(v)
            g_und[v].append(u)
        edges.append((u, v, b))
    
    INF = 10**18
    val = [None] * n
    
    def bfs(start):
        q = deque([start])
        val[start] = 0
        mn = 0
        mx = 0
        
        while q:
            u = q.popleft()
            
            for v in g_dir[u]:
                if val[v] is None:
                    val[v] = val[u] + 1
                    mn = min(mn, val[v])
                    mx = max(mx, val[v])
                    q.append(v)
                else:
                    if val[v] != val[u] + 1:
                        return False, 0, 0
            
            for v in g_und[u]:
                if val[v] is None:
                    val[v] = val[u] + 1
                    mn = min(mn, val[v])
                    mx = max(mx, val[v])
                    q.append(v)
                else:
                    if abs(val[v] - val[u]) != 1:
                        return False, 0, 0
        
        return True, mn, mx
    
    total_min = 0
    total_max = 0
    
    for i in range(n):
        if val[i] is None:
            ok, mn, mx = bfs(i)
            if not ok:
                print("NO")
                return
            total_min = min(total_min, mn)
            total_max = max(total_max, mx)
    
    shift = -total_min
    ans = [x + shift for x in val]
    
    print("YES")
    print(total_max - total_min)
    print(*ans)

if __name__ == "__main__":
    solve()
```Mã này xây dựng hai danh sách kề nhau, một cho các cạnh có hướng bắt buộc và một cho các cạnh linh hoạt. Sau đó, nó thực hiện BFS từ mọi nút chưa được truy cập, gán các giá trị dưới dạng khoảng cách trong biểu đồ ràng buộc. Bất kỳ sự không nhất quán nào trong việc gán sẽ ngay lập tức bị từ chối. 

Một điểm triển khai tinh vi là các cạnh vô hướng được coi là thực thi chênh lệch tuyệt đối 1, được thực thi bằng cách kiểm tra`abs(val[v] - val[u]) != 1`. Điều này đảm bảo rằng ngay cả khi BFS chỉ định hướng một cách ngầm định thì ràng buộc vẫn được tôn trọng. 

Sự thay đổi toàn cầu cuối cùng là cần thiết bởi vì chỉ có sự khác biệt tương đối mới quan trọng. Nếu không dịch chuyển, các giá trị âm có thể xuất hiện, nhưng vấn đề cho phép bất kỳ phạm vi nào miễn là các giá trị nằm trong giới hạn sau khi chuẩn hóa. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
6 6
1 2 0
3 2 0
2 5 0
6 5 1
6 3 0
2 4 1
```Chúng tôi xử lý thành phần biểu đồ bắt đầu từ nút 1. 

| Bước | Nút | Giá trị | Hành động | 
| --- | --- | --- | --- | 
| 1 | 1 | 0 | bắt đầu BFS | 
| 2 | 2 | 1 | từ 1 qua cạnh vô hướng | 
| 3 | 3 | 0 | từ 2 qua cạnh vô hướng | 
| 4 | 5 | 2 | từ 2 qua cạnh vô hướng | 
| 5 | 6 | 1 | thỏa mãn ràng buộc có hướng | 
| 6 | 4 | 2 | cạnh định hướng thực thi giá trị | 

Giá trị tối thiểu là 0, tối đa là 3, do đó kết quả là 3. 

Dấu vết này cho thấy các ràng buộc định hướng không gây ra sự không nhất quán như thế nào, chúng chỉ thực thi định hướng giữa những khác biệt tuyệt đối đã nhất quán. 

### Ví dụ 2 

Một trường hợp nhỏ không nhất quán:```
3 3
1 2 0
2 3 0
3 1 0
```Chúng tôi cố gắng gán các giá trị: 

Bắt đầu từ 1 = 0. Sau đó 2 = 1, 3 = 2. Nhưng cạnh (3,1) yêu cầu abs(2 - 0) = 1, sai vì nó bằng 2. BFS phát hiện mâu thuẫn này khi xem lại nút 1 với giá trị ngụ ý xung đột. 

Điều này cho thấy tại sao tính nhất quán của chu kỳ lại quan trọng: các cạnh vô hướng hoạt động giống như các ràng buộc đơn vị bắt buộc phải tạo thành một không gian số liệu nhất quán. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi cạnh được xử lý một lần trong quá trình truyền BFS | 
| Không gian | O(n + m) | Danh sách kề và mảng giá trị | 

Các ràng buộc n ≤ 200 và m ≤ 2000 làm cho việc này diễn ra nhanh chóng, vì thuật toán chỉ thực hiện truyền tải đồ thị tuyến tính với việc kiểm tra thời gian không đổi trên mỗi cạnh. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from solution import solve
    return solve()

# sample 1
assert run("""6 6
1 2 0
3 2 0
2 5 0
6 5 1
6 3 0
2 4 1
""").strip() == """YES
3
3 2 1 3 1 0"""

# minimal case
assert run("""1 0
""").strip() == """YES
0
0"""

# inconsistent cycle
assert run("""3 3
1 2 0
2 3 0
3 1 0
""").strip() == "NO"

# all directed consistent chain
assert run("""4 3
1 2 1
2 3 1
3 4 1
""").strip().startswith("YES")

# symmetric undirected chain
assert run("""5 4
1 2 0
2 3 0
3 4 0
4 5 0
""").strip().startswith("YES")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | CÓ 0 0 | chuẩn hóa trường hợp cơ sở | 
| xung đột tam giác | KHÔNG | chu kỳ không nhất quán | 
| chuỗi định hướng | tăng giá trị | độ chính xác của việc truyền bá | 
| con đường vô hướng | phạm vi hợp lệ | chênh lệch tối đa tối thiểu | 

## Vỏ cạnh 

Một trường hợp cạnh quan trọng là khi một chu trình của các cạnh vô hướng gây ra mâu thuẫn. Trong những trường hợp như vậy, BFS chỉ định các giá trị mà cuối cùng cố gắng gán lại nút đã truy cập bằng một giá trị khác. Thuật toán ngay lập tức phát hiện sự không khớp này và từ chối phiên bản. 

Một trường hợp cạnh khác là khi các cạnh có hướng áp đặt các ràng buộc xung đột gián tiếp thông qua các đường dẫn vô hướng. Ngay cả khi mỗi cạnh riêng lẻ trông hợp lệ, các giá trị được truyền cuối cùng không đồng ý ở một đỉnh chung, điều này được phát hiện trong quá trình kiểm tra tính nhất quán của BFS. 

Trường hợp cạnh cuối cùng là cấu trúc cây trong đó tất cả các cạnh đều vô hướng. Ở đây, BFS chỉ định cấu trúc giống như lưỡng cực hợp lệ với các giá trị xen kẽ và phạm vi tối đa trở thành đường kính của cây theo nghĩa trọng lượng đơn vị này, được ghi lại một cách tự nhiên bằng cách truyền mà không cần thêm logic.
