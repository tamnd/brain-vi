---
title: "CF 106430I - Bessie và XOR"
description: "Nhiệm vụ có thể được coi là làm việc trên một hệ thống các ràng buộc trên các giá trị XOR, nhưng các ràng buộc này không được viết trực tiếp dưới dạng các cạnh và nút. Thay vào đó, chúng bắt nguồn từ các phép toán trên các phân đoạn của một mảng, có thể được diễn giải lại thành một bài toán đồ thị."
date: "2026-06-20T03:51:18+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106430
codeforces_index: "I"
codeforces_contest_name: "2026 USACO.Guide Informatics Tournament"
rating: 0
weight: 106430
solve_time_s: 52
verified: true
draft: false
---

[CF 106430I - Bessie và XOR](https://codeforces.com/problemset/problem/106430/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 52s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ có thể được coi là làm việc trên một hệ thống các ràng buộc trên các giá trị XOR, nhưng các ràng buộc này không được viết trực tiếp dưới dạng các cạnh và nút. Thay vào đó, chúng bắt nguồn từ các phép toán trên các phân đoạn của một mảng, có thể được diễn giải lại thành một bài toán đồ thị. 

Chúng tôi bắt đầu với cấu trúc giống như mảng trong đó mỗi thao tác sẽ sửa đổi một phân đoạn liền kề bằng XOR. Quan sát chính là các phép toán phân đoạn có thể được chuyển đổi thành các ràng buộc trên biểu diễn sai phân của mảng. Khi chúng ta chuyển sang một mảng sai phân, mỗi phép toán chỉ ảnh hưởng đến hai vị trí, nghĩa là mỗi phép toán sẽ trở thành mối quan hệ giữa chính xác hai biến. 

Sau quá trình chuyển đổi này, chúng ta kết thúc với một cấu trúc giống như biểu đồ trong đó mỗi vị trí có một giá trị XOR bắt buộc và mỗi cạnh tương ứng với một thao tác gán một giá trị đóng góp cho cả hai điểm cuối. Vấn đề trở thành việc gán giá trị cho các cạnh sao cho đối với mỗi nút, XOR của các giá trị cạnh sự cố khớp với một mục tiêu nhất định. 

Đầu ra là một phép gán đầy đủ các trọng số cạnh thỏa mãn tất cả các ràng buộc của nút hoặc một tuyên bố rằng không có phép gán nào tồn tại. 

Các ràng buộc ngụ ý rằng ít nhất chúng ta cần xử lý thời gian tuyến tính về số lượng nút và cạnh. Bất kỳ giải pháp nào liên quan đến việc thử tất cả các phép gán hoặc lặp qua các tập con của các cạnh đều không thể ngay lập tức vì số lượng khả năng tăng theo cấp số nhân với số lượng cạnh. Ngay cả những cách tiếp cận cố gắng giải quyết các hệ thống một cách độc lập trên mỗi nút cũng thất bại vì các cạnh có nhiều ràng buộc trên biểu đồ. 

Một trường hợp cạnh tinh tế phát sinh khi một thành phần được kết nối có tổng giá trị nút XOR khác 0. Ví dụ: nếu ba nút trong một thành phần có giá trị 1, 2 và 4 thì XOR của chúng là 7, khác 0. Trong trường hợp như vậy, không có phép gán nào có thể thỏa mãn đồng thời tất cả các ràng buộc vì mỗi cạnh đóng góp vào chính xác hai nút, nghĩa là tổng XOR trên một thành phần phải luôn bị loại bỏ. Một người giải ngây thơ bỏ qua điều kiện tổng thể này vẫn có thể cố gắng xây dựng và tạo ra kết quả không nhất quán. 

Một dạng lỗi khác xảy ra khi đồ thị không phải là cây. Nếu các chu trình tồn tại và chúng ta không lựa chọn chiến lược xây dựng một cách cẩn thận, việc gán tùy ý có thể tạo ra mâu thuẫn khi xem lại các cạnh. Đây là lý do tại sao việc phân rã có cấu trúc giống như cây bao trùm là điều cần thiết. 

## Phương pháp tiếp cận 

Một nỗ lực trực tiếp sẽ coi mỗi cạnh là một biến và cố gắng gán các giá trị thỏa mãn tất cả các phương trình XOR nút cùng một lúc. Điều này trở thành một hệ phương trình tuyến tính trên trường GF(2), nhưng được viết dưới dạng XOR. Giải quyết nó trực tiếp bằng cách sử dụng phép loại bỏ Gaussian sẽ có tác dụng về mặt lý thuyết. Tuy nhiên, cấu trúc đồ thị có thể giúp việc xây dựng đơn giản hơn. 

Phối cảnh brute-force là gán từng giá trị cho từng cạnh một và kiểm tra xem cuối cùng tất cả các ràng buộc nút có được thỏa mãn hay không. Điều này đòi hỏi phải thử kết hợp các trọng số cạnh, trong trường hợp xấu nhất sẽ tăng theo cấp số nhân với số lượng cạnh. Ngay cả khi chúng tôi hạn chế các giá trị được lấy từ các yêu cầu của nút, các quyết định cục bộ có thể truyền bá sự không nhất quán vào sâu trong biểu đồ, buộc phải quay lui. 

Thông tin chi tiết về cấu trúc quan trọng là mỗi cạnh đóng góp chính xác hai lần, một lần cho mỗi điểm cuối. Điều này ngụ ý rằng trong bất kỳ thành phần được kết nối nào, XOR của tất cả các giá trị nút phải bằng 0. Khi điều kiện đó được giữ, chúng ta có thể ngừng suy nghĩ theo các biểu đồ tùy ý và thay vào đó giảm cấu trúc thành một cây bao trùm, vì các chu kỳ không cung cấp thêm sự tự do ngoài tính nhất quán.

Trên một cái cây, ta có thể giải bài toán từ lá trở lên. Một nút lá có chính xác một cạnh tới, do đó XOR yêu cầu của nó xác định trực tiếp giá trị của cạnh đó. Khi cạnh đó được cố định, chúng ta có thể loại bỏ chiếc lá và truyền tác dụng của nó đến chiếc lá lân cận. Việc lặp lại quá trình này sẽ loại bỏ mọi sự mơ hồ và đảm bảo mọi ràng buộc đều được thỏa mãn chính xác một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Phân công vũ lực | Hàm mũ | O(n + m) | Quá chậm | 
| Loại bỏ lá dựa trên cây | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng biểu đồ các nút và cạnh từ đầu vào, diễn giải mỗi cạnh là một ràng buộc kết nối hai nút. Mỗi nút mang một giá trị XOR bắt buộc phải bằng XOR của trọng số cạnh tới. 
2. Đối với mỗi thành phần được kết nối, hãy tính XOR của tất cả các giá trị nút. Nếu XOR này khác 0, ngay lập tức kết luận rằng không có phép gán hợp lệ nào tồn tại cho thành phần đó. Điều này xuất phát từ thực tế là mọi cạnh đều đóng góp vào hai nút, do đó tất cả các đóng góp đều bị hủy bỏ. 
3. Nếu thành phần hợp lệ, hãy trích xuất bất kỳ cây bao trùm nào của thành phần đó. Điều này làm giảm cấu trúc về dạng trong đó mọi nút ngoại trừ nút gốc đều có chính xác một cạnh cha mà qua đó ràng buộc của nó có thể được giải quyết. 
4. Root cây bao trùm tùy ý và duy trì một bộ lá. Lá là nút có bậc 1 trong cấu trúc cây hiện tại. 
5. Liên tục lấy một nút lá không phải là nút gốc, đọc giá trị XOR yêu cầu hiện tại của nó và gán giá trị này làm trọng số của cạnh tới duy nhất của nó. Phép gán này đảm bảo ràng buộc của lá được thỏa mãn ngay lập tức. 
6. Sau khi gán cạnh, hãy loại bỏ lá khỏi cây và truyền giá trị của nó cho hàng xóm bằng cách XOR nó theo yêu cầu của hàng xóm. Điều này duy trì tính chính xác vì cạnh bị loại bỏ trước đó đã góp phần vào cả hai điểm cuối. 
7. Tiếp tục quá trình này cho đến khi chỉ còn lại phần gốc. Tại thời điểm này, điều kiện XOR toàn cục đảm bảo rằng yêu cầu của nút gốc được tự động thỏa mãn. 

### Tại sao nó hoạt động 

Cấu trúc duy trì một bất biến: ở mỗi bước, yêu cầu XOR của mỗi nút còn lại bằng với đóng góp XOR vẫn cần được cung cấp bởi các cạnh chưa được xử lý trong cây con còn lại. Việc loại bỏ một lá sẽ giải quyết chính xác một ràng buộc bằng cách sử dụng chính xác một bậc tự do và bước truyền bá sẽ duy trì tính nhất quán cho cấu trúc còn lại. Vì mỗi cạnh được xử lý chính xác một lần và mỗi ràng buộc nút được tính chính xác một lần nên không thể nảy sinh mâu thuẫn sau lần kiểm tra tính khả thi ban đầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    val = list(map(int, input().split()))
    
    g = [[] for _ in range(n)]
    edges = []
    
    for i in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append((v, i))
        g[v].append((u, i))
        edges.append((u, v))
    
    vis = [False] * n
    ans = [0] * m
    
    from collections import deque
    
    for s in range(n):
        if vis[s]:
            continue
        
        # gather component
        comp = []
        stack = [s]
        vis[s] = True
        
        while stack:
            u = stack.pop()
            comp.append(u)
            for v, _ in g[u]:
                if not vis[v]:
                    vis[v] = True
                    stack.append(v)
        
        # check XOR condition
        x = 0
        for u in comp:
            x ^= val[u]
        
        if x != 0:
            print("NO")
            return
        
        # build parent structure via BFS tree
        parent = [-1] * n
        parent_edge = [-1] * n
        order = []
        
        dq = deque([s])
        parent[s] = s
        
        while dq:
            u = dq.popleft()
            order.append(u)
            for v, ei in g[u]:
                if parent[v] == -1:
                    parent[v] = u
                    parent_edge[v] = ei
                    dq.append(v)
        
        # process in reverse BFS (leaf-like removal)
        for u in reversed(order):
            if u == s:
                continue
            p = parent[u]
            ei = parent_edge[u]
            ans[ei] = val[u]
            val[p] ^= val[u]
        
        # root handled automatically
    
    print("YES")
    print(*ans)

if __name__ == "__main__":
    solve()
```Mã đầu tiên phân tách biểu đồ thành các thành phần được kết nối. Đối với mỗi thành phần, nó xác minh điều kiện cần thiết là XOR của các giá trị nút phải bằng 0. Nếu điều này không thành công, chức năng sẽ kết thúc sớm. 

Sau khi tính khả thi được xác nhận, nó sẽ xây dựng cây bao trùm BFS bắt nguồn từ một nút tùy ý. Mảng cha mã hóa cấu trúc cây trong đó mỗi nút (trừ nút gốc) có chính xác một cạnh cha sẽ được sử dụng để gán trọng số cạnh của nó. 

Thứ tự BFS ngược mô phỏng việc loại bỏ lá. Giá trị của mỗi nút được gán cho cạnh kết nối nó với nút cha của nó, giá trị này sẽ thỏa mãn nút đó ngay lập tức. Sau đó, giá trị của nó được truyền lên trên bằng XOR, cập nhật yêu cầu của cha mẹ. 

Một chi tiết tinh tế là chúng tôi sửa đổi`val`tại chỗ. Điều này là cần thiết vì nó tích lũy các yêu cầu còn lại khi xử lý các phần tử con. Nếu không có sự lan truyền này, các nút cha sẽ không phản ánh chính xác các ràng buộc còn lại. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Hãy xem xét một thành phần nhỏ có ba nút trong một chuỗi. 

Giá trị nút ban đầu:`[3, 5, 6]`Các cạnh:`1-2`,`2-3`| Bước | Nút được xử lý | Đã gán cạnh | Giá trị sử dụng | Cập nhật dành cho phụ huynh | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | (2,3) | 6 | val[2] ^= 6 | 
| 2 | 2 | (1,2) | giá trị được cập nhật[2] | giá trị [1] ^= giá trị [2] | 

Sau khi xử lý nút 3, cạnh (2,3) được cố định thành 6 và yêu cầu của nút 2 trở thành`5 XOR 6 = 3`. Sau đó nút 2 gán cạnh (1,2) = 3 và nút 1 trở nên nhất quán. 

Điều này cho thấy cách loại bỏ lá giải quyết các ràng buộc một cách tự nhiên từ dưới lên. 

### Ví dụ 2 

Đồ thị hình ngôi sao có nút trung tâm là 1 và các lá 2, 3, 4. 

Giá trị nút:`[0, 1, 2, 3]`| Bước | Nút được xử lý | Đã gán cạnh | Giá trị sử dụng | Cập nhật dành cho phụ huynh | 
| --- | --- | --- | --- | --- | 
| 1 | 4 | (1,4) | 3 | val[1] ^= 3 | 
| 2 | 3 | (1,3) | 2 | giá trị [1] ^= 2 | 
| 3 | 2 | (1,2) | 1 | val[1] ^= 1 | 

Cuối cùng, nút trung tâm tự động trở thành 0 do tính nhất quán XOR ban đầu, xác nhận tính chính xác. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi nút và cạnh được truy cập với số lần không đổi trong quá trình BFS và xử lý ngược | 
| Không gian | O(n + m) | Biểu diễn đồ thị và mảng phụ trợ để theo dõi phụ huynh | 

Độ phức tạp tuyến tính phù hợp với các ràng buộc điển hình của các vấn đề tái cấu trúc đồ thị có tới hàng trăm nghìn nút và cạnh, đảm bảo giải pháp chạy thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from solution import solve  # assume wrapped
    return solve()

# minimal valid tree
assert run("""3 2
1 2 3
1 2
2 3
""") == "YES\n...\n", "simple chain"

# impossible XOR condition
assert run("""3 2
1 2 4
1 2
2 3
""") == "NO", "bad XOR"

# single node no edges
assert run("""1 0
0
""") == "YES\n", "single node"

# star graph
assert run("""4 3
1 2 3 0
1 2
1 3
1 4
""") == "YES\n...\n", "star case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuỗi 3 nút | CÓ + cạnh | nhân giống cơ bản | 
| XOR thành phần khác 0 | KHÔNG | kiểm tra tính khả thi | 
| nút đơn | CÓ | thành phần tầm thường | 
| đồ thị sao | CÓ + bài tập | cập nhật nhiều lá | 

## Vỏ cạnh 

Trường hợp một cạnh là một biểu đồ hoàn toàn bị ngắt kết nối trong đó mỗi nút được cô lập. Trong trường hợp này, mỗi nút tạo thành thành phần riêng của nó và điều kiện XOR được thỏa mãn một cách đơn giản. Thuật toán xử lý từng nút một cách độc lập, không gán cạnh nào và trả về thành công ngay lập tức. 

Một trường hợp khác là thành phần có một chu trình đơn. Cây bao trùm BFS loại bỏ chính xác một cạnh khỏi chu trình, biến nó thành cây. Cạnh chu kỳ bị loại bỏ là không cần thiết để đảm bảo tính chính xác, vì việc xây dựng lại dựa trên lá đã đảm bảo tính nhất quán trên cây bao trùm. 

Trường hợp cuối cùng là khi tất cả các giá trị nút bằng 0. BFS vẫn chạy, nhưng mọi cạnh được chỉ định đều nhận được trọng số bằng 0 và việc truyền bá không thay đổi tất cả các giá trị. Đầu ra vẫn hợp lệ vì các ràng buộc XOR được thỏa mãn ở mọi nút.
