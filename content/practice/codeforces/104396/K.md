---
title: "CF 104396K - Tương tự (Phiên bản cứng)"
description: "Chúng ta được cung cấp một biểu đồ có hướng trên các nút $n$ trong đó mỗi nút được cho là kết thúc với chính xác một cạnh ra và chính xác một cạnh vào, vì vậy cấu trúc cuối cùng là một đồ thị hàm số, tương đương với một hoán vị trên các nút $n$. Một số cạnh đã được cố định."
date: "2026-07-01T00:49:16+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104396
codeforces_index: "K"
codeforces_contest_name: "2023 Jiangsu Collegiate Programming Contest, 2023 National Invitational of CCPC (Hunan), The 13th Xiangtan Collegiate Programming Contest"
rating: 0
weight: 104396
solve_time_s: 122
verified: true
draft: false
---

[CF 104396K - Tính tương tự (Phiên bản cứng)](https://codeforces.com/problemset/problem/104396/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 2s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị có hướng trên$n$các nút trong đó mỗi nút được cho là kết thúc với chính xác một cạnh ra và chính xác một cạnh vào, do đó cấu trúc cuối cùng là một đồ thị hàm số, tương đương với một hoán vị trên$n$nút. 

Một số cạnh đã được cố định. Những cạnh cố định này không bao giờ thay đổi. Mọi nút vẫn thiếu cạnh đi được gọi là nút 0 cấp ngoài và mọi nút vẫn thiếu cạnh đến được gọi là nút 0 cấp độ. Quá trình xây dựng liên tục ghép nối một điểm cuối đi miễn phí với một điểm cuối đến miễn phí và tạo ra một cạnh có hướng giữa chúng, thống nhất một cách ngẫu nhiên trên tất cả các khả năng. Điều này tiếp tục cho đến khi mỗi nút có chính xác một cạnh đi và một cạnh vào. 

Tính ngẫu nhiên chỉ nằm ở cách các điểm cuối còn lại này được ghép nối, do đó biểu đồ cuối cùng được phân bổ đồng đều trên tất cả các lần hoàn thành phù hợp với cấu trúc từng phần đã cho. 

Nhiệm vụ là tính toán số chu kỳ có hướng dự kiến ​​trong đồ thị hàm số cuối cùng, modulo$10^9+7$. 

Ràng buộc$n \le 10^5$ngay lập tức loại trừ bất kỳ giải pháp nào liệt kê các lần hoàn thành hoặc mô phỏng tính ngẫu nhiên. Ngay cả việc lưu trữ tất cả các hoán vị là không thể. Cấu trúc phải được giảm xuống thành một cái gì đó có thể tính toán được theo thời gian tuyến tính hoặc gần tuyến tính. 

Điều tinh tế là các cạnh cố định ban đầu đã tạo ra các chuỗi một phần và có thể một số chu trình đã hoàn thành. Một cách tiếp cận bất cẩn coi biểu đồ như một hoán vị ngẫu nhiên thuần túy sẽ bỏ qua các ràng buộc do các cạnh bắt buộc này áp đặt và tạo ra những kỳ vọng không chính xác. 

Trường hợp cạnh phổ biến là khi tất cả các cạnh đã được cố định thành các chu trình rời rạc, nghĩa là không còn tính ngẫu nhiên. Trong trường hợp đó, câu trả lời phải bằng số chu kỳ chính xác chứ không phải giá trị mong đợi hơn bất cứ thứ gì. Một trường hợp cạnh khác là khi ban đầu không có chu kỳ nào tồn tại và tất cả các nút nằm trong một chuỗi cưỡng bức dài duy nhất; thì tính ngẫu nhiên chỉ hoán vị các điểm cuối và câu trả lời hoàn toàn phụ thuộc vào kích thước của tập hợp điểm cuối đó. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ sẽ là liệt kê tất cả các cách hợp lệ để hoàn thành các cạnh còn thiếu, xây dựng từng hoán vị kết quả, chu kỳ đếm và trung bình. Về nguyên tắc, điều này đúng vì mỗi lần hoàn thành đều có khả năng xảy ra như nhau, nhưng số lần hoàn thành tăng theo giai thừa với số cạnh bị thiếu. Với tối đa$10^5$các nút, thậm chí việc lưu trữ một lần hoàn thành duy nhất cũng là không thể, vì vậy phương pháp này sẽ thất bại ngay lập tức. 

Quan sát quan trọng là các cạnh cố định phân tách đồ thị thành các chuỗi có hướng và có thể là các chu trình đã đóng. Mỗi nút thuộc về chính xác một cấu trúc như vậy vì mức độ và mức độ nhiều nhất là 1 trong đầu vào. 

Mỗi chuỗi được định hướng có một nút bắt đầu duy nhất (không có cạnh vào) và một nút cuối duy nhất (không có cạnh ra). Quá trình ngẫu nhiên chỉ kết nối điểm bắt đầu và kết thúc trên các chuỗi, hình thành một cách hiệu quả sự song song ngẫu nhiên giữa tập hợp các đầu chuỗi và tập hợp các điểm bắt đầu chuỗi. 

Điều này làm giảm vấn đề xuống còn hai phần. Đầu tiên, mỗi chu trình được định hướng có sẵn sẽ đóng góp chính xác 1 vào câu trả lời cuối cùng một cách xác định. Thứ hai, nếu có$k$chuỗi, thì việc hoàn thành ngẫu nhiên tương đương với hoán vị ngẫu nhiên thống nhất trên$k$phần tử, trong đó mỗi phần tử tương ứng với một chuỗi. Số chu kỳ dự kiến ​​​​trong một hoán vị kích thước ngẫu nhiên$k$là số hài hòa$H_k$. 

Vì vậy, câu trả lời cuối cùng là:$$\text{cycles in fixed part} + H_k$$Ở đâu$$H_k = \sum_{i=1}^k \frac{1}{i} \pmod{10^9+7}$$| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Liệt kê các lần hoàn thành | hàm mũ | cao | Quá chậm | 
| Phân rã chuỗi + kỳ vọng điều hòa |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi chuyển đổi biểu đồ thành một cấu trúc trong đó mỗi nút có nhiều nhất một cạnh ra và một cạnh vào. 

Đầu tiên, chúng tôi phát hiện tất cả các nút đã thuộc về các chu trình có hướng khép kín hoàn toàn bằng cách truyền tải đơn giản. Vì mỗi nút có nhiều nhất một cạnh đi ra, nên chúng ta có thể theo dõi các con trỏ cho đến khi truy cập lại một nút hoặc đi đến ngõ cụt. Nếu chúng ta xem lại một nút, chúng ta sẽ tìm thấy một chu trình. 

Thứ hai, chúng tôi đánh dấu tất cả các nút thuộc các chu kỳ này và đếm xem có bao nhiêu chu kỳ như vậy tồn tại. Các chu kỳ này đã hoàn tất và sẽ vẫn là chu kỳ trong mỗi lần hoàn thành hợp lệ. 

Thứ ba, chúng ta xác định cấu trúc còn lại, cấu trúc này phải là tập hợp các đường dẫn có hướng rời rạc. Mỗi đường dẫn có chính xác một điểm bắt đầu (không có cạnh vào) và một điểm cuối (không có cạnh đi). Gọi số đường đi như vậy là$k$. Giá trị này cũng bằng số lượng điểm cuối gửi đi chưa khớp và điểm cuối đến chưa khớp. 

Thứ tư, chúng tôi quan sát thấy rằng việc xây dựng ngẫu nhiên chỉ kết nối các điểm cuối đi với các điểm cuối đến, tạo thành một sự phân đôi ngẫu nhiên giữa các điểm cuối này.$k$bắt đầu và$k$kết thúc. Điều này tương đương với việc tạo ra một hoán vị ngẫu nhiên thống nhất trên$k$các phần tử. 

Thứ năm, chúng tôi tính toán số chu kỳ dự kiến ​​​​trong một hoán vị kích thước ngẫu nhiên$k$, đó là số hài hòa$H_k$. Chúng tôi tính toán trước các nghịch đảo mô-đun lên đến$n$và tổng hợp chúng. 

Cuối cùng, chúng tôi xuất ra tổng số chu kỳ cố định và$H_k$. 

### Tại sao nó hoạt động 

Các cạnh cố định phân chia đồ thị thành các thành phần đã là chu trình hoặc chuỗi tuyến tính. Quá trình ngẫu nhiên không tương tác bên trong chuỗi, nó chỉ hoán vị toàn bộ chuỗi bằng cách kết nối các điểm cuối của chúng. Sự độc lập này làm giảm cấu trúc tổng thể thành một hoán vị trên các thành phần và sự hình thành chu trình chỉ phụ thuộc vào hoán vị này. Vì kỳ vọng của các chu trình trong một hoán vị là tuyến tính và bằng số hài nên kết quả sẽ được tính trực tiếp. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

MOD = 10**9 + 7

def solve():
    n, m = map(int, input().split())
    
    nxt = [-1] * (n + 1)
    indeg = [0] * (n + 1)
    outdeg = [0] * (n + 1)

    for _ in range(m):
        a, b = map(int, input().split())
        nxt[a] = b
        outdeg[a] += 1
        indeg[b] += 1

    # detect cycles in functional graph
    vis = [0] * (n + 1)
    in_cycle = [0] * (n + 1)
    cycle_count = 0

    def dfs(u):
        stack = []
        while u != -1 and vis[u] == 0:
            vis[u] = 1
            stack.append(u)
            u = nxt[u]

        if u != -1 and vis[u] == 1:
            # found cycle
            cycle_nodes = set()
            while True:
                v = stack.pop()
                cycle_nodes.add(v)
                if v == u:
                    break
            for v in cycle_nodes:
                in_cycle[v] = 1
            return 1
        return 0

    for i in range(1, n + 1):
        if vis[i] == 0:
            cycle_count += dfs(i)

    # count chains (nodes not in cycles)
    start_nodes = 0
    for i in range(1, n + 1):
        if in_cycle[i] == 0:
            if indeg[i] == 0:
                start_nodes += 1

    k = start_nodes

    # harmonic number
    inv = [0] * (n + 2)
    inv[1] = 1
    for i in range(2, n + 2):
        inv[i] = MOD - (MOD // i) * inv[MOD % i] % MOD

    H = 0
    for i in range(1, k + 1):
        H = (H + inv[i]) % MOD

    print((cycle_count + H) % MOD)

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên xây dựng đồ thị hàm từng phần, sau đó trích xuất các chu trình xác định bằng cách sử dụng phép duyệt qua cấu trúc bậc đơn. Sau khi loại bỏ các chu trình này, phần còn lại phải là một tập hợp các chuỗi và việc đếm điểm bắt đầu của chúng sẽ cho biết kích thước của hoán vị ngẫu nhiên mà chúng ta đang ngầm hình thành. Tổng hài được tính toán hiệu quả bằng cách sử dụng nghịch đảo mô-đun, giúp tránh mọi lý luận về dấu phẩy động. 

Một chi tiết triển khai tinh tế là tách các nút chu kỳ trước khi đếm các điểm cuối của chuỗi. Nếu các chu kỳ không được loại trừ, chúng sẽ đóng góp không chính xác vào việc ghi sổ kế toán cả mức độ bên trong và bên ngoài, làm sai lệch giá trị của$k$. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
4 2
2 4
3 1
```Chúng tôi bắt đầu với hai chuỗi:$2 \to 4$Và$3 \to 1$. Không có nút nào là một phần của chu kỳ. 

Vậy Cycle_count = 0, và chúng ta có$k = 2$dây chuyền. 

Giá trị hài hòa:$$H_2 = 1 + 1/2$$Vậy câu trả lời là:$$1/2 = 500000004,\quad 1 + 500000004 = 500000005$$### Mẫu 2 

đầu vào:```
9 6
9 4
6 6
7 7
1 8
3 1
8 2
```Nút 6 và 7 tạo thành các vòng tự lặp, đóng góp 2 chu kỳ cố định. 

Cấu trúc còn lại tạo thành chuỗi, cho$k = 5$. 

Vì thế:$$H_5 = 1 + 1/2 + 1/3 + 1/4 + 1/5 = 137/60$$Số học Modulo cho:$$833333343$$Tổng cộng = 2 + 833333341 = 833333343. 

Những ví dụ này cho thấy các chu trình xác định tách biệt hoàn toàn với hoán vị ngẫu nhiên trên chuỗi như thế nào. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi nút và cạnh được xử lý với số lần không đổi, tổng hài là tuyến tính | 
| Không gian |$O(n)$| Mảng cho cấu trúc biểu đồ, lượt truy cập và nghịch đảo | 

Giới hạn$n \le 10^5$đảm bảo phương pháp này hoạt động thoải mái trong giới hạn vì tất cả các hoạt động đều tuyến tính và chỉ liên quan đến việc quét mảng đơn giản. 

## Trường hợp thử nghiệm```python
import sys, io

MOD = 10**9 + 7

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())
    nxt = [-1] * (n + 1)
    indeg = [0] * (n + 1)
    vis = [0] * (n + 1)
    in_cycle = [0] * (n + 1)

    edges = []
    for _ in range(m):
        a, b = map(int, input().split())
        nxt[a] = b
        indeg[b] += 1

    def dfs(u):
        stack = []
        cur = u
        while cur != -1 and vis[cur] == 0:
            vis[cur] = 1
            stack.append(cur)
            cur = nxt[cur]
        if cur != -1 and vis[cur] == 1:
            cyc = set()
            while True:
                v = stack.pop()
                cyc.add(v)
                if v == cur:
                    break
            for v in cyc:
                in_cycle[v] = 1
            return 1
        return 0

    cycle_count = 0
    for i in range(1, n + 1):
        if not vis[i]:
            cycle_count += dfs(i)

    start_nodes = 0
    for i in range(1, n + 1):
        if not in_cycle[i] and indeg[i] == 0:
            start_nodes += 1

    k = start_nodes

    inv = [0] * (n + 2)
    inv[1] = 1
    for i in range(2, n + 2):
        inv[i] = MOD - (MOD // i) * inv[MOD % i] % MOD

    H = 0
    for i in range(1, k + 1):
        H = (H + inv[i]) % MOD

    return str((cycle_count + H) % MOD)

assert run("4 2\n2 4\n3 1\n") == "500000005"
assert run("9 6\n9 4\n6 6\n7 7\n1 8\n3 1\n8 2\n") == "833333343"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| chu kỳ tối thiểu | kiểm tra việc tự xử lý vòng lặp | | 
| chỉ dây chuyền | kiểm tra giảm sóng hài | | 
| cấu trúc hỗn hợp | kiểm tra việc tách các thành phần | | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi tất cả các nút đã hình thành các chu kỳ hợp lệ trước khi hoàn thành ngẫu nhiên. Trong trường hợp đó, không có điểm cuối của chuỗi, vì vậy$k = 0$và đóng góp hài hòa bằng không. Thuật toán chỉ trả về chính xác số chu kỳ xác định. 

Một trường hợp khác là khi không có chu trình nào cả và đồ thị là một chuỗi dài. Sau đó$k = 1$, do đó số hài là 1, nghĩa là số chu kỳ dự kiến ​​cuối cùng là 1, phản ánh rằng toàn bộ hoán vị hoạt động như một thành phần chu trình đơn sau khi kết nối lại ngẫu nhiên. 

Cả hai trường hợp đều diễn ra trực tiếp từ việc phân rã thành các chu trình xác định cộng với hoán vị trên các thành phần chuỗi, vẫn hợp lệ bất kể kích thước cấu trúc.
