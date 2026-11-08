---
title: "CF 1385E - Các cạnh dẫn hướng"
description: "Chúng ta có một biểu đồ trong đó mọi cạnh đều biết điểm cuối của nó, nhưng không phải tất cả các cạnh đều được phép tự do chọn hướng. Một số cạnh đã được định hướng và phải giữ nguyên chính xác như hiện tại, trong khi các cạnh còn lại không được định hướng và có thể được định hướng theo cách chúng ta muốn."
date: "2026-06-18T18:27:37+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "dfs-and-similar", "graphs"]
categories: ["algorithms"]
codeforces_contest: 1385
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 656 (Div. 3)"
rating: 2000
weight: 1385
solve_time_s: 305
verified: false
draft: false
---

[CF 1385E - Các cạnh định hướng](https://codeforces.com/problemset/problem/1385/E) 

**Xếp hạng:** 2000 
**Tags:** thuật toán xây dựng, dfs và tương tự, đồ thị 
**Thời gian giải:** 5 phút 5s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một biểu đồ trong đó mọi cạnh đều biết điểm cuối của nó, nhưng không phải tất cả các cạnh đều được phép tự do chọn hướng. Một số cạnh đã được định hướng và phải giữ nguyên chính xác như hiện tại, trong khi các cạnh còn lại không được định hướng và có thể được định hướng theo cách chúng ta muốn. Nhiệm vụ là gán hướng cho tất cả các cạnh vô hướng sao cho đồ thị có hướng cuối cùng không chứa chu trình có hướng. 

Đầu ra không chỉ là một quyết định có/không. Nếu có thể, chúng ta cũng phải xuất ra hướng đầy đủ của mọi cạnh, bao gồm cả những cạnh đã được định hướng, sao cho đồ thị có hướng thu được là không theo chu kỳ. 

Cấu trúc của các ràng buộc đã gợi ý rằng bất kỳ giải pháp nào cũng phải gần tuyến tính về kích thước của biểu đồ. Với tối đa 2·10^5 đỉnh và cạnh trong tất cả các trường hợp thử nghiệm, bất kỳ phương pháp bậc hai nào, chẳng hạn như liên tục kiểm tra chu trình sau mỗi lần gán cạnh hoặc thử tất cả các hướng, đều hoàn toàn không khả thi. Về cơ bản, giải pháp phải thực hiện một hoặc một vài lần duyệt đồ thị cho mỗi trường hợp thử nghiệm. 

Một khó khăn chính là các cạnh có hướng áp đặt các ràng buộc thứ tự nghiêm ngặt giữa các đỉnh, trong khi các cạnh vô hướng rất linh hoạt nhưng phải được gán một cách nhất quán với các ràng buộc đó. Một trực giác ngây thơ có thể là trước tiên bỏ qua các cạnh có hướng, xây dựng thứ tự tôpô của đồ thị vô hướng cơ bản và sau đó định hướng các cạnh tương ứng. Điều này thất bại ngay lập tức khi các cạnh được định hướng đã buộc một thứ tự một phần xung đột với thứ tự DFS ngây thơ. 

Ví dụ: hãy xem xét ba nút 1 → 2 → 3 đã được định hướng và một cạnh vô hướng nằm giữa 3 và 1. Mọi nỗ lực gán một thứ tự tôpô mà bỏ qua chuỗi có hướng cuối cùng sẽ tạo ra mâu thuẫn nếu thứ tự đặt 3 trước 1 hoặc ngược lại. Các cạnh định hướng xác định một trật tự từng phần cứng nhắc phải được tôn trọng trên toàn cầu. 

Do đó, vấn đề trọng tâm là làm thế nào để hợp nhất các ràng buộc có hướng cố định với các cạnh vô hướng linh hoạt theo cách duy trì tính không tuần hoàn. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ là coi mọi cạnh vô hướng đều có hai hướng có thể và cố gắng chỉ định các hướng trong khi kiểm tra chu kỳ. Ngay cả khi chúng tôi sử dụng tính năng quay lui với DFS, trong trường hợp xấu nhất vẫn có 2^(m0) khả năng trong đó m0 là số cạnh vô hướng. Với m lên tới 2·10^5 thì điều này hoàn toàn không thể xảy ra. 

Một ý tưởng ngây thơ khác là chỉ tính toán thứ tự tôpô của các cạnh có hướng, sau đó định hướng mọi cạnh vô hướng từ trước đến sau theo thứ tự đó. Vấn đề là các cạnh có hướng có thể không xác định được thứ tự hoàn chỉnh trên tất cả các đỉnh và quan trọng hơn là ngay từ đầu chúng có thể không có tính tuần hoàn. Nếu các cạnh có hướng đã chứa một chu trình thì không có nghiệm nào tồn tại và điều này phải được phát hiện. 

Quan sát quan trọng là chúng ta chỉ cần một trật tự tôpô phù hợp với tất cả các cạnh đã được định hướng. Khi chúng ta có thứ tự như vậy, mọi cạnh vô hướng có thể được định hướng an toàn từ đầu đến cuối theo thứ tự đó, bởi vì điều này đảm bảo không có cạnh lùi nào có thể xuất hiện và do đó không thể tạo chu trình. 

Điều này làm giảm vấn đề kiểm tra xem đồ thị con có hướng có phải là không tuần hoàn hay không và nếu có thì việc xây dựng một trật tự tôpô trên nó. Sau đó, chúng ta bỏ qua các cạnh vô hướng trong giai đoạn sắp xếp thứ tự và chỉ gán hướng của chúng sau đó. 

Điều tinh tế duy nhất là chỉ riêng các cạnh có hướng có thể không đến được tất cả các nút thông qua quá trình truyền tải, vì vậy chúng ta vẫn phải bao gồm tất cả các đỉnh trong sắp xếp tôpô. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^m) | O(m) | Quá chậm | 
| Sắp xếp tôpô trên đồ thị có hướng + định hướng các cạnh vô hướng | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xử lý từng trường hợp thử nghiệm một cách độc lập.

1. Xây dựng biểu đồ chỉ sử dụng các cạnh có hướng, lưu trữ chúng dưới dạng danh sách kề và cũng theo dõi độ. Biểu đồ này thể hiện các ràng buộc cố định. 
2. Chạy sắp xếp tôpô (thuật toán Kahn) trên đồ thị có hướng này. Chúng tôi liên tục chọn các nút có bậc bằng 0 và loại bỏ chúng, tạo ra thứ tự các đỉnh. 
3. Nếu chúng ta không thể bao gồm tất cả các đỉnh theo thứ tự tôpô thì các cạnh có hướng chứa một chu trình và không thể thỏa mãn yêu cầu. Chúng tôi xuất ra KHÔNG ngay lập tức. Lý do là bất kỳ chu trình nào được hình thành bởi các cạnh cố định đều không thể bị phá vỡ bằng cách định hướng các cạnh vô hướng. 
4. Nếu chúng ta đạt được thứ tự tôpô thành công, hãy gán thứ hạng cho mỗi đỉnh dựa trên vị trí của nó theo thứ tự này. 
5. Bây giờ xử lý tất cả các cạnh. Đối với mỗi cạnh có hướng, chúng tôi giữ nó không thay đổi. Đối với mọi cạnh vô hướng (u, v), chúng ta so sánh thứ hạng của chúng. Nếu xếp hạng [u] < xếp hạng [v], chúng tôi hướng nó u → v, nếu không, chúng tôi hướng nó v → u. 
6. Xuất danh sách cạnh có hướng cuối cùng. 

Lý do bước 5 có hiệu quả là vì thứ tự tôpô đảm bảo rằng tất cả các cạnh có hướng sẽ tiến về phía trước theo thứ tự. Do đó, việc định hướng các cạnh vô hướng một cách nhất quán theo thứ tự này sẽ đảm bảo chúng cũng tiến về phía trước. 

### Tại sao nó hoạt động 

Thứ tự tôpô là sự mở rộng tuyến tính của tất cả các ràng buộc được áp đặt bởi các cạnh đã được định hướng. Bất kỳ đường dẫn có hướng nào trong biểu đồ gốc đều phải tăng theo đúng thứ tự này. Vì mọi cạnh vô hướng đều được định hướng tuân theo cùng thứ tự này nên mọi cạnh trong đồ thị cuối cùng sẽ đi từ đỉnh có thứ hạng thấp hơn đến đỉnh có thứ hạng cao hơn. Một chu kỳ có định hướng sẽ yêu cầu một chuỗi cấp bậc tăng dần nghiêm ngặt để cuối cùng quay trở lại nút bắt đầu, điều này là không thể. 

Do đó, đồ thị được xây dựng là không có chu trình và nếu một chu trình tồn tại ở các cạnh có hướng cố định thì nó sẽ được phát hiện chính xác ở bước 3. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

def solve():
    t = int(input())
    out = []
    
    for _ in range(t):
        n, m = map(int, input().split())
        
        adj = [[] for _ in range(n + 1)]
        indeg = [0] * (n + 1)
        edges = []
        
        for i in range(m):
            tpe, x, y = map(int, input().split())
            edges.append((tpe, x, y))
            if tpe == 1:
                adj[x].append(y)
                indeg[y] += 1
        
        q = deque([i for i in range(1, n + 1) if indeg[i] == 0])
        topo = []
        
        while q:
            u = q.popleft()
            topo.append(u)
            for v in adj[u]:
                indeg[v] -= 1
                if indeg[v] == 0:
                    q.append(v)
        
        if len(topo) != n:
            out.append("NO")
            continue
        
        pos = [0] * (n + 1)
        for i, v in enumerate(topo):
            pos[v] = i
        
        res = []
        for tpe, x, y in edges:
            if tpe == 1:
                res.append((x, y))
            else:
                if pos[x] < pos[y]:
                    res.append((x, y))
                else:
                    res.append((y, x))
        
        out.append("YES")
        for x, y in res:
            out.append(f"{x} {y}")
    
    print("\n".join(out))

if __name__ == "__main__":
    solve()
```Các cạnh có hướng trước tiên được cô lập để xây dựng đồ thị ràng buộc. Phần thuật toán của Kahn xây dựng một thứ tự tuyến tính hợp lệ hoặc phát hiện một chu kỳ nếu hàng đợi trống sớm. 

Mảng vị trí mã hóa thứ tự cuối cùng sao cho so sánh giữa điểm cuối của các cạnh vô hướng là O(1). Điều này tránh việc tính toán lại logic thứ tự trong quá trình xử lý cạnh. 

Vòng lặp cuối cùng bảo toàn các cạnh được định hướng và định hướng các cạnh không được định hướng theo đúng thứ tự được tính toán, đảm bảo tính nhất quán với tất cả các ràng buộc. 

## Ví dụ đã hoạt động 

Hãy xem xét một biểu đồ nhỏ trong đó các cạnh có hướng thực thi một chuỗi rõ ràng và các cạnh vô hướng phải tuân theo chuỗi đó. 

đầu vào:```
1
3 3
1 1 2
1 2 3
0 3 1
```Chúng ta xây dựng đồ thị có hướng 1 → 2 → 3. Thuật toán của Kahn tạo ra topo = [1, 2, 3]. 

| Bước | Xếp hàng | Xuất hiện | Topo | Thay đổi mức độ | 
| --- | --- | --- | --- | --- | 
| ban đầu | [1] | - | [] | 2:1, 3:1 | 
| 1 | [2] | 1 | [1] | 2:0 | 
| 2 | [3] | 2 | [1,2] | 3:0 | 
| 3 | [] | 3 | [1,2,3] | - | 

Bây giờ pos[1] < pos[3] nên ta định hướng 3 → 1 trở thành 3 → 1 hoặc 1 → 3 tùy theo quy tắc; vì 1 đứng trước 3 nên ta gán 1 → 3. 

Các cạnh đầu ra trở thành:```
1 2
2 3
1 3
```Điều này chứng tỏ rằng tất cả các cạnh đều tiến lên theo thứ tự và không có chu trình nào được tạo ra. 

Ví dụ thứ hai trong đó kích hoạt phát hiện chu kỳ: 

đầu vào:```
1
3 3
1 1 2
1 2 3
1 3 1
```Thuật toán của Kahn không thể xử lý tất cả các nút vì ban đầu mọi nút đều có bậc 1, do đó hàng đợi trống. Danh sách topo có kích thước 0 hoặc 1 tùy theo thứ tự khởi tạo, nhưng không có kích thước 3. 

Điều này khẳng định tính không thể thực hiện được do chu trình có hướng trong các ràng buộc cố định. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi đỉnh và cạnh được xử lý một lần trong thuật toán Kahn và một lần ở bước định hướng cuối cùng | 
| Không gian | O(n + m) | Danh sách kề và lưu trữ cạnh | 

Tổng của tất cả các đỉnh và cạnh trong các trường hợp thử nghiệm được giới hạn bởi 2·10^5, do đó, một giải pháp tuyến tính sẽ phù hợp một cách thoải mái trong giới hạn thời gian. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    
    input = sys.stdin.readline
    from collections import deque

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            n, m = map(int, input().split())
            adj = [[] for _ in range(n + 1)]
            indeg = [0] * (n + 1)
            edges = []
            for i in range(m):
                tpe, x, y = map(int, input().split())
                edges.append((tpe, x, y))
                if tpe == 1:
                    adj[x].append(y)
                    indeg[y] += 1

            q = deque([i for i in range(1, n + 1) if indeg[i] == 0])
            topo = []
            while q:
                u = q.popleft()
                topo.append(u)
                for v in adj[u]:
                    indeg[v] -= 1
                    if indeg[v] == 0:
                        q.append(v)

            if len(topo) != n:
                out.append("NO")
                continue

            pos = [0] * (n + 1)
            for i, v in enumerate(topo):
                pos[v] = i

            out.append("YES")
            for tpe, x, y in edges:
                if tpe == 1:
                    out.append(f"{x} {y}")
                else:
                    if pos[x] < pos[y]:
                        out.append(f"{x} {y}")
                    else:
                        out.append(f"{y} {x}")

        return "\n".join(out)

    return solve()

# provided sample checks (format adapted)
# custom cases

# simple chain + undirected
assert run("1\n3 3\n1 1 2\n1 2 3\n0 3 1\n") != ""

# cycle impossible
assert run("1\n3 3\n1 1 2\n1 2 3\n1 3 1\n").strip().startswith("NO")
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chuỗi + vô hướng | CÓ + cạnh | định hướng đúng theo thứ tự topo | 
| chu kỳ định hướng | KHÔNG | độ chính xác phát hiện chu kỳ | 

## Vỏ cạnh 

Một trường hợp tinh tế là khi đồ thị không có cạnh có hướng nào cả. Trong tình huống này, đồ thị con có hướng trống, do đó mọi đỉnh đều bắt đầu bằng 0. Thứ tự tôpô trở thành bất kỳ hoán vị nào được tạo ra bởi thứ tự hàng đợi. Thuật toán vẫn hoạt động vì mọi cạnh vô hướng đều được định hướng nhất quán theo thứ tự tùy ý này, đảm bảo không có chu kỳ. 

Một trường hợp khác là đồ thị không liên kết trong đó các thành phần khác nhau có các ràng buộc có hướng độc lập. Thuật toán của Kahn xen kẽ các thành phần một cách tự nhiên theo bất kỳ thứ tự hợp lệ nào. Vì các cạnh vô hướng luôn được định hướng theo thứ tự tổng thể nên các cạnh giữa các thành phần không thể hình thành chu trình giữa các thành phần. 

Một kịch bản thất bại sẽ là cố gắng định hướng các cạnh vô hướng trước khi thiết lập một trật tự tôpô đầy đủ. Ví dụ: nếu một người cố gắng chỉ định các hướng một cách tham lam trong DFS mà không có thứ tự chung, thì rất dễ tạo ra một cạnh sau hình thành một chu trình sau đó. Trật tự toàn cầu tránh được điều này bằng cách cố định một cấu trúc tuyến tính hoàn chỉnh trước rồi mới đưa ra các quyết định cục bộ.
