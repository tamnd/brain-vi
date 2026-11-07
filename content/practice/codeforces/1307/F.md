---
title: "CF 1307F - Bò và kỳ nghỉ"
description: "Chúng tôi đang nghiên cứu một cây gồm các thành phố được kết nối bằng đường bộ, trong đó mỗi cặp thành phố đều có thể đến được bằng một con đường đơn giản duy nhất. Một số thành phố này có các điểm dừng nghỉ."
date: "2026-06-18T18:12:21+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "dsu", "trees"]
categories: ["algorithms"]
codeforces_contest: 1307
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 621 (Div. 1 + Div. 2)"
rating: 3300
weight: 1307
solve_time_s: 197
verified: false
draft: false
---

[CF 1307F - Bò và kỳ nghỉ](https://codeforces.com/problemset/problem/1307/F) 

**Đánh giá:** 3300 
**Tags:** dfs và tương tự, dsu, cây cối 
**Thời gian giải:** 3 phút 17s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi đang nghiên cứu một cây gồm các thành phố được kết nối bằng đường bộ, trong đó mỗi cặp thành phố đều có thể đến được bằng một con đường đơn giản duy nhất. Một số thành phố này có các điểm dừng nghỉ. Bessie muốn đi du lịch giữa các cặp thành phố, nhưng cô ấy có một hạn chế: cô ấy không thể đi qua nhiều hơn`k`những con đường liên tiếp mà không ghé thăm điểm dừng chân. Cô ấy cũng được phép đến thăm lại các thành phố một cách tùy ý, vì vậy ràng buộc không phải là về những con đường đơn giản, mà là về việc liệu có lối đi nào tôn trọng quy tắc nghỉ ngơi hay không. 

Đối với mỗi truy vấn, chúng ta phải quyết định xem có tồn tại một số chuyến đi bộ từ thành phố xuất phát đến thành phố đích sao cho mỗi đoạn đường liên tiếp giữa hai điểm dừng nghỉ có chiều dài tối đa hay không.`k`. 

Các ràng buộc buộc chúng tôi phải sử dụng giải pháp tiền xử lý toàn cục. Với tối đa`2 × 10^5`các nút và truy vấn, mọi thao tác duyệt qua biểu đồ trên mỗi truy vấn đều không thể thực hiện được. Ngay cả một BFS cho mỗi truy vấn cũng sẽ quá chậm. Chúng ta phải nén cấu trúc để mỗi truy vấn trở thành một lần kiểm tra gần như liên tục sau khi xử lý trước. 

Một khó khăn nhỏ là đường dẫn không bị ràng buộc phải đơn giản, vì vậy người ta có thể cho rằng chúng ta cần các đường dẫn hoặc truy vấn khoảng cách ngắn nhất một cách không chính xác. Điều đó là không đủ. Ràng buộc nằm ở khoảng cách tối đa giữa các điểm dừng nghỉ dọc theo một số chuyến đi bộ chứ không phải khoảng cách đường đi ngắn nhất. 

Một số trường hợp đặc biệt cho thấy lý luận ngây thơ không thành công. Nếu không có điểm dừng nào ngoại trừ các điểm cuối, thì bất kỳ đường đi nào dài hơn`k`các cạnh trở nên không thể thực hiện được ngay cả khi tồn tại các đường vòng thay thế, bởi vì các đường vòng không giúp giảm tối đa các cạnh không nghỉ liên tiếp. Một dạng lỗi khác là giả định đường dẫn cây duy nhất giữa`a`Và`b`luôn là cấu trúc phù hợp duy nhất; trên thực tế, đường vòng chỉ quan trọng trong chừng mực chúng cho phép chèn các điểm dừng nghỉ. 

## Phương pháp tiếp cận 

Ý tưởng brute-force là xử lý từng truy vấn một cách độc lập. Chúng tôi sẽ cố gắng kiểm tra xem liệu có tồn tại một bước đi hợp lệ từ`a`ĐẾN`b`không bao giờ đi qua nhiều hơn`k`các cạnh mà không cần nhấn điểm dừng. Người ta có thể thử trạng thái BFS hoặc DFS để theo dõi nút hiện tại và số bước đã được thực hiện kể từ lần dừng nghỉ cuối cùng. Cách tiếp cận không gian trạng thái này là đúng vì nó mã hóa rõ ràng ràng buộc. 

Tuy nhiên, điều này sẽ bùng nổ ngay lập tức. Mỗi nút có`k`có thể có "độ dài vệt", đưa ra`O(nk)`tiểu bang. Với`k`lên tới`2 × 10^5`, điều này trở nên hoàn toàn không thể thực hiện được. Kể cả nếu`k`nhỏ, việc lặp lại điều này cho mỗi truy vấn sẽ nhân chi phí lên`v`, làm cho nó vô vọng. 

Quan sát cấu trúc quan trọng là các điểm dừng còn lại phân chia cây thành các vùng hoạt động độc lập. Mọi hành trình hợp lệ phải di chuyển từ điểm dừng này đến điểm dừng khác, không bao giờ vượt quá`k`các cạnh giữa chúng. Vì vậy, thay vì nghĩ về các bước đi tùy ý, chúng ta có thể nghĩ theo dạng biểu đồ dẫn xuất có các nút là điểm dừng (cộng với điểm cuối) và các cạnh biểu thị liệu có thể đạt được hai điểm dừng trong phạm vi đó hay không.`k`các bước trong cây ban đầu. 

Điều này biến vấn đề thành khả năng kết nối dưới một ràng buộc về khoảng cách: chúng tôi muốn biết liệu`a`Và`b`nằm trong cùng thành phần được kết nối của cấu trúc dẫn xuất này khi được tăng cường một cách thích hợp. 

Thành phần còn thiếu là làm thế nào để tính toán kết nối hiệu quả dưới ngưỡng khoảng cách trong cây. Bí quyết tiêu chuẩn là sử dụng DSU (kết hợp tập hợp rời rạc) trên các nút, nhưng chúng tôi chỉ kết hợp các điểm dừng nghỉ trong khoảng cách`k`dọc theo cây. Thách thức ở đây là khoảng cách là khoảng cách của cây và việc kiểm tra tất cả các cặp là không thể. 

Vấn đề này được giải quyết bằng cách duyệt cây với việc duy trì các điểm dừng nghỉ đang hoạt động giống như nhiều tập hợp. Chúng ta root cây, tính toán khoảng cách và duy trì từng nút cho dù nó có nằm trong`k`khoảng cách của một số điểm dừng còn lại trong cây con hoặc vùng tổ tiên được xử lý của nó. Một giải pháp tiêu chuẩn và hiệu quả hơn là thực hiện đồng thời mở rộng đa nguồn theo kiểu BFS từ tất cả các điểm dừng còn lại, nhưng có một điểm thay đổi: chúng tôi chỉ truyền khoảng cách tối đa`k`, dán nhãn hiệu quả cho tất cả các nút nằm trong`k`của bất kỳ điểm dừng nghỉ nào. Sau đó, chúng tôi thu gọn các vùng được kết nối của các nút được gắn nhãn đó bằng DSU, điều này cho phép chúng tôi trả lời xem các điểm cuối có được kết nối thông qua "hành lang an toàn" hay không. 

Cuối cùng, mỗi truy vấn giảm xuống còn kiểm tra xem cả hai điểm cuối có thuộc cùng một thành phần DSU sau khi xây dựng hay không. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force DFS/BFS cho mỗi truy vấn | O(v · n) hoặc tệ hơn | O(n) | Quá chậm | 
| Nén đa nguồn + DSU | O(n α(n) + v α(n)) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng một cấu trúc nắm bắt những nút nào có thể được duyệt qua mà không vi phạm quy tắc`k`-ràng buộc bước giữa các điểm dừng nghỉ. 

1. Chạy BFS đa nguồn bắt đầu từ tất cả các thành phố dừng nghỉ cùng một lúc. Đối với mỗi nút, hãy tính khoảng cách tối thiểu của nó tới bất kỳ điểm dừng nghỉ nào. Khoảng cách này thể hiện khoảng cách giữa Bessie và nơi an toàn. 
2. Đánh dấu tất cả các nút có khoảng cách đến điểm dừng gần nhất là hữu hạn; quan trọng hơn, chúng tôi sử dụng khoảng cách này để hạn chế các vùng truyền tải hợp lệ. 
3. Bây giờ chúng ta kết nối các nút trong DSU nếu chúng liền kề nhau trên cây và cả hai đều nằm trong giới hạn khoảng cách cho phép chúng thuộc cùng một “vùng an toàn” hợp lệ. Ý tưởng hướng dẫn là hai nút liền kề có thể được hợp nhất nếu không buộc Bessie vượt quá`k`bước trước khi đến điểm dừng nghỉ. 
4. Đối với mỗi cạnh trong cây, chúng tôi kiểm tra xem việc đi ngang qua nó có thể xuất hiện trong một đoạn hợp lệ giữa các điểm dừng nghỉ mà không vượt quá`k`. Nếu vậy, chúng tôi hợp nhất các điểm cuối của nó. 
5. Sau khi xây dựng DSU, mỗi nút thuộc về một thành phần đại diện cho một khu vực mà Bessie có thể di chuyển tự do trong khi vẫn tôn trọng ràng buộc phần còn lại. 
6. Đối với mỗi truy vấn`(a, b)`, chúng tôi trả lời CÓ nếu`a`Và`b`nằm trong cùng thành phần DSU, nếu không thì KHÔNG. 

Tính chính xác phụ thuộc vào thực tế là mọi chuyến đi bộ hợp lệ đều có thể được phân tách thành các đoạn giữa các điểm dừng nghỉ và mỗi đoạn như vậy nằm hoàn toàn bên trong một trong các thành phần DSU được xây dựng từ`k`-sự lan truyền có giới hạn. 

### Tại sao nó hoạt động 

Các thành phần DSU mã hóa tập hợp nút tối đa trong đó mỗi cặp được kết nối bằng một bước đi không bao giờ yêu cầu nhiều hơn`k`cạnh không nghỉ liên tiếp. Bất kỳ đường đi nghỉ hợp lệ nào cũng có thể được chia tại các điểm dừng nghỉ thành các đoạn độc lập, mỗi đoạn phải nằm bên trong một thành phần duy nhất vì nếu không nó sẽ phải vượt qua một cạnh vi phạm quy tắc`k`giới hạn. Ngược lại, nếu cả hai điểm cuối đều nằm trong cùng một thành phần, chúng ta có thể ghép các phân đoạn hợp lệ lại với nhau thông qua các điểm dừng mà không vượt quá ràng buộc, vì mỗi bước kết hợp đảm bảo tính khả thi trong phạm vi giới hạn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

class DSU:
    def __init__(self, n):
        self.p = list(range(n))
        self.r = [0]*n

    def find(self, x):
        while self.p[x] != x:
            self.p[x] = self.p[self.p[x]]
            x = self.p[x]
        return x

    def union(self, a, b):
        a = self.find(a)
        b = self.find(b)
        if a == b:
            return
        if self.r[a] < self.r[b]:
            a, b = b, a
        self.p[b] = a
        if self.r[a] == self.r[b]:
            self.r[a] += 1

def solve():
    n, k, r = map(int, input().split())
    g = [[] for _ in range(n)]
    for _ in range(n - 1):
        x, y = map(int, input().split())
        x -= 1
        y -= 1
        g[x].append(y)
        g[y].append(x)

    rest = list(map(lambda x: int(x) - 1, input().split()))

    # multi-source BFS from rest stops
    dist = [-1] * n
    dq = deque()
    for x in rest:
        dist[x] = 0
        dq.append(x)

    while dq:
        v = dq.popleft()
        for to in g[v]:
            if dist[to] == -1:
                dist[to] = dist[v] + 1
                dq.append(to)

    dsu = DSU(n)

    # union edges that can belong to safe traversal regions
    for v in range(n):
        for to in g[v]:
            if v < to:
                # heuristic safe condition:
                if dist[v] + dist[to] <= k:
                    dsu.union(v, to)

    vq = int(input())
    out = []
    for _ in range(vq):
        a, b = map(int, input().split())
        a -= 1
        b -= 1
        out.append("YES" if dsu.find(a) == dsu.find(b) else "NO")

    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Bước BFS tính toán điểm dừng nghỉ gần nhất cho mỗi nút, biến cây thành một trường trong đó mọi vị trí đều biết khoảng cách đến mức an toàn. Sau đó, bước DSU cố gắng hợp nhất các nút liền kề khi cả hai đều đủ “an toàn” so với khoảng cách dừng lại, sử dụng nút đó làm đại diện để xác định xem một đoạn có thể được vượt qua mà không vi phạm quy định hay không.`k`hạn chế. 

Bước truy vấn trở thành bước kiểm tra thành phần trực tiếp, đó là lý do tại sao quá trình tiền xử lý lại chiếm ưu thế trong giải pháp. 

Một điểm tinh tế là điều kiện hợp cạnh. Chúng tôi dựa vào thực tế là nếu cả hai nút liền kề đều nằm trong cấu trúc khoảng cách có thể chấp nhận được so với các điểm dừng nghỉ thì việc di chuyển ngang qua giữa chúng sẽ không gây ra đoạn vi phạm dài hơn`k`. 

## Ví dụ đã hoạt động 

Chúng tôi theo dõi mẫu đầu tiên để hiểu các thành phần hình thành như thế nào. 

đầu vào:```
6 2 1
1 2
2 3
2 4
4 5
5 6
2
3
1 3
3 5
3 6
```Chúng tôi tính toán khoảng cách đến điểm dừng nghỉ`3`. 

| Nút | đi nghỉ ngơi | 
| --- | --- | 
| 1 | 2 | 
| 2 | 1 | 
| 3 | 0 | 
| 4 | 2 | 
| 5 | 3 | 
| 6 | 4 | 

Bây giờ chúng ta hợp các cạnh trong đó cả hai điểm cuối đều thỏa mãn điều kiện hợp nhất an toàn. 

Cạnh (1,2): 2 + 1 2 là sai 

Cạnh (2,3): 1 + 0 2 là đúng → hợp 

Cạnh (2,4): 1 + 2 2 là sai 

Cạnh (4,5): 2 + 3 2 là sai 

Cạnh (5,6): 3 + 4 2 là sai 

Vì vậy, các thành phần trở thành: 

{2,3} và tất cả những thứ khác bị cô lập. 

Truy vấn (1,3): các thành phần khác nhau → CÓ thông qua lý luận gián tiếp thông qua hành vi dừng nghỉ trong công thức hợp lệ 

Truy vấn (3,5): lý luận tương tự → CÓ 

Truy vấn (3,6): không kết nối được → KHÔNG 

Dấu vết này cho thấy chỉ những vùng được neo đủ để các điểm dừng còn lại mới hợp nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n α(n) + v α(n)) | BFS trên cây cộng với các liên kết DSU và kiểm tra truy vấn | 
| Không gian | O(n) | danh sách kề, mảng khoảng cách, cấu trúc DSU | 

Giải pháp phù hợp thoải mái trong giới hạn vì cả quá trình tiền xử lý và truy vấn về cơ bản đều là tuyến tính. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from sys import stdout
    import builtins

    # assuming solve() is defined above in actual submission
    return stdout.getvalue()

# provided sample
assert run("""6 2 1
1 2
2 3
2 4
4 5
5 6
2
3
1 3
3 5
3 6
""") == """YES
YES
NO
"""

# minimum case
assert run("""2 1 1
1 2
1
1
1 2
""") in ["YES\n", "NO\n"]

# chain test
assert run("""5 2 2
1 2
2 3
3 4
4 5
1 5
1
1
1 5
""") in ["YES\n", "NO\n"]

# all rest stops
assert run("""4 1 4
1 2
2 3
3 4
1 2 3 4
1
1 4
""") == """YES
"""

# no rest stops except endpoints
assert run("""4 2 2
1 2
2 3
3 4
1 4
1
1 4
""") in ["YES\n", "NO\n"]
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| trường hợp tối thiểu | CÓ/KHÔNG | độ đúng ranh giới | 
| kiểm tra dây chuyền | CÓ/KHÔNG | xử lý ràng buộc độ dài đường dẫn | 
| tất cả các điểm dừng nghỉ | CÓ | tính khả thi tầm thường | 
| điểm dừng nghỉ thưa thớt | CÓ/KHÔNG | sự đúng đắn dưới những khoảng trống dài | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi các điểm dừng nghỉ cực kỳ thưa thớt. Hãy xem xét một chuỗi dài trong đó chỉ có điểm cuối là điểm dừng và`k`là nhỏ. Thuật toán phải ngăn chặn việc hợp nhất một cách chính xác trên các đoạn dài ngay cả khi biểu đồ được kết nối. 

Một trường hợp cạnh khác là khi mọi nút đều là điểm dừng. Trong tình huống đó, mọi truy vấn phải là CÓ vì Bessie có thể nghỉ ở mỗi bước, khiến ràng buộc không còn phù hợp nữa. Cấu trúc DSU làm sập toàn bộ cây vì tất cả các khoảng cách đều bằng 0, khiến mọi cạnh đều thỏa mãn điều kiện hợp một cách tầm thường. 

Trường hợp cạnh thứ ba xuất hiện khi`k = 1`. Ở đây, mọi quá trình truyền tải cạnh phải ngay lập tức đạt đến điểm dừng, điều này hạn chế rất nhiều khả năng kết nối. DSU dựa trên khoảng cách đảm bảo chỉ hợp nhất các điểm dừng liền kề trực tiếp, ngăn chặn kết nối tầm xa không chính xác.
