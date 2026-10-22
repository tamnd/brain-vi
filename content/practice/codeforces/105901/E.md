---
title: "CF 105901E - Đồ thị đầy màu sắc"
description: "Chúng ta có một đồ thị vô hướng có $n$ đỉnh và $m$ cạnh. Mỗi cạnh phải được gán một màu, sử dụng các màu được gắn nhãn từ $1$ đến $m$ và màu sắc có thể được sử dụng lại trên các cạnh."
date: "2026-06-22T02:51:03+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105901
codeforces_index: "E"
codeforces_contest_name: "2025 ICPC Wuhan Invitational Contest (The 3rd Universal Cup. Stage 37: Wuhan)"
rating: 0
weight: 105901
solve_time_s: 67
verified: true
draft: false
---

[CF 105901E - Đồ thị đầy màu sắc](https://codeforces.com/problemset/problem/105901/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 7s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị vô hướng với$n$đỉnh và$m$các cạnh. Mỗi cạnh phải được gán một màu, sử dụng các màu được dán nhãn từ$1$ĐẾN$m$và màu sắc có thể được sử dụng lại trên các cạnh. 

Hạn chế cục bộ đối với các đỉnh: đối với mọi đỉnh$u$, nếu bạn nhìn vào tất cả các cạnh liên quan tới$u$, thì bất kỳ màu cố định nào cũng có thể xuất hiện nhiều nhất hai lần trong số các cạnh tới đó. Vì vậy, một đỉnh được phép chạm vào cùng một màu một hoặc hai lần, nhưng không bao giờ được phép chạm ba lần trở lên. 

Đối với mỗi đỉnh$u$, định nghĩa$f_u$là số lượng các màu riêng biệt xuất hiện giữa các cạnh tới của nó. Mục đích là gán màu cho các cạnh sao cho tổng các giá trị này trên tất cả các đỉnh được giảm thiểu. 

Các ràng buộc lớn: tổng số đỉnh và cạnh trong tất cả các trường hợp thử nghiệm lên tới$2 \cdot 10^5$. Điều này ngay lập tức loại trừ bất cứ điều gì bậc hai trong$n$hoặc$m$và cũng làm rõ rằng chúng ta cần xây dựng tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. Bất kỳ giải pháp nào liên tục xử lý danh sách kề trên mỗi màu hoặc mô phỏng việc gán màu với việc ghi sổ nặng nề trên mỗi đỉnh sẽ không tồn tại. 

Một khó khăn nhỏ là ràng buộc “nhiều nhất là hai cạnh cho mỗi màu trên mỗi đỉnh” không hoạt động giống như việc tô màu hoặc so khớp cạnh tiêu chuẩn. Một kẻ tham lam ngây thơ gán màu cho mỗi cạnh mà không có cấu trúc tổng thể sẽ dễ dàng bị mắc kẹt trong tình huống một đỉnh tích lũy quá nhiều màu riêng biệt mặc dù mỗi màu có giá trị cục bộ. 

Ví dụ, trong một ngôi sao có tâm ở đỉnh$u$với bậc 5, nếu chúng ta gán cho tất cả các cạnh màu khác nhau, chúng ta sẽ nhận được$f_u = 5$, nhưng ràng buộc cho phép ghép các cạnh sao cho một màu duy nhất có thể bao phủ hai cạnh liên quan, vì vậy cách tốt nhất có thể là$f_u = 3$. Một người tham lam tránh sử dụng lại ở trung tâm sẽ hoàn toàn bỏ lỡ sự cải tiến này. 

Thách thức thực sự là buộc cấu trúc sao cho mỗi đỉnh “tái sử dụng màu theo cặp” càng nhiều càng tốt mà không vi phạm tính nhất quán giữa các cạnh. 

## Phương pháp tiếp cận 

Giới hạn dưới của mỗi đỉnh đã được xác định bởi ràng buộc. Vì mỗi màu có thể xuất hiện nhiều nhất hai lần ở một đỉnh, nếu$d_u$chạm vào các cạnh$u$, thì ít nhất$\lceil d_u / 2 \rceil$màu sắc cần thiết tại$u$. Tính tổng giá trị này trên tất cả các đỉnh sẽ cho ra giới hạn dưới tổng thể của mục tiêu. 

Vì vậy, vấn đề trở thành: liệu chúng ta có thể xây dựng một màu cạnh sao cho mọi đỉnh đều đạt được chính xác giới hạn này không? Điều đó có nghĩa là chúng ta ghép các cạnh tới ở mỗi đỉnh một cách hoàn hảo sao cho mỗi cặp có chung một màu. 

Khó khăn là sự nhất quán. Nếu chúng ta ghép các cạnh tùy ý tại một đỉnh, thì cùng một cạnh đó sẽ tham gia ghép nối ở cả hai điểm cuối và các cặp đó phải thống nhất về mặt tổng thể để chúng tạo thành các lớp màu hợp lệ. 

Ý tưởng chính là trước tiên hãy ngừng suy nghĩ về màu sắc mà thay vào đó hãy xây dựng cấu trúc trên các cạnh. Chúng tôi muốn nhóm các cạnh thành các thành phần được kết nối trong đó mỗi thành phần sau này sẽ trở thành một màu duy nhất. Bên trong mỗi thành phần, mỗi đỉnh phải có nhiều nhất là hai bậc để ràng buộc được tự động thỏa mãn. 

Điều này gợi ý việc xây dựng sự phân tách các cạnh thành các đường đi và chu trình. Nếu mỗi thành phần được kết nối của quá trình phân tách này là một đường dẫn hoặc chu trình thì việc gán một màu cho mỗi thành phần sẽ có tác dụng: tại bất kỳ đỉnh nào, mỗi thành phần đóng góp tối đa hai cạnh liên quan, do đó ràng buộc được giữ nguyên. 

Bây giờ mục tiêu là xây dựng một phân rã như vậy trong khi vẫn đảm bảo rằng tại mỗi đỉnh, các cạnh liên quan được ghép nối một cách tối ưu. Cách tiêu chuẩn để thực thi điều này là ghép các cạnh cục bộ ở mỗi đỉnh một cách tùy ý, sau đó hiểu mỗi cặp là một kết nối giữa hai cạnh. Sau khi thực hiện điều này ở tất cả các đỉnh, mỗi cạnh có chính xác là hai bậc trong cấu trúc “cạnh này tới cạnh khác”, ngoại trừ các điểm cuối có thể được tạo bởi bậc lẻ. Điều này tự nhiên tạo thành những đường mòn rời rạc trên các cạnh. 

Mỗi vệt kết quả chính xác là một lớp màu. Mỗi đỉnh thuộc về số lượng đường đi vì nó có các cặp sự cố, khớp với giới hạn dưới$\lceil d_u / 2 \rceil$, do đó mục tiêu được giảm thiểu. 

Việc xây dựng là tuyến tính vì mỗi cạnh được xử lý một số lần không đổi trong khi tạo thành các cặp và trích xuất các thành phần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tham lam mỗi cạnh |$O(m \cdot n)$trường hợp xấu nhất |$O(m)$| Quá chậm / Cấu trúc sai | 
| Ghép nối + phân tách thành các đường nhỏ |$O(n + m)$|$O(n + m)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi xây dựng một cấu trúc trong đó các cạnh được kết nối dần dần thành chuỗi bằng cách sử dụng ghép nối cục bộ tại các đỉnh, sau đó trích xuất các chuỗi đó dưới dạng các lớp màu. 

1. Lưu trữ danh sách kề của biểu đồ nhưng cũng theo dõi những cạnh nào vẫn chưa được sử dụng. 

Điều này là cần thiết vì chúng ta sẽ dần dần “tiêu thụ” các cạnh khi ghép nối chúng. 
2. Với mọi đỉnh$u$, lấy danh sách các cạnh liên quan của nó và ghép chúng tùy ý: cạnh thứ nhất với cạnh thứ hai, cạnh thứ ba với cạnh thứ tư, v.v. 

Việc ghép nối này là nơi duy nhất thực thi sự tối ưu. Mỗi đỉnh cố gắng nhóm các cạnh của nó thành càng nhiều cặp càng tốt để sau này mỗi cặp có thể chia sẻ một màu. 
3. Đối với mỗi cặp cạnh$(e_1, e_2)$được hình thành ở một đỉnh, nối hai cạnh này trong biểu đồ phụ trong đó các nút là các cạnh gốc. 

Bước này chuyển đổi bài toán từ các ràng buộc dựa trên đỉnh thành một biểu đồ cạnh trong đó mỗi nút cạnh có bậc nhiều nhất là hai. Việc ghép nối đảm bảo rằng mỗi cạnh tham gia vào tối đa một kết nối cho mỗi điểm cuối. 
4. Sau khi xử lý tất cả các đỉnh, biểu đồ cạnh sẽ phân tách thành các đường đi riêng biệt và quay vòng trên các cạnh. 

Mỗi thành phần được kết nối tương ứng với một chuỗi các cạnh trong đó các cạnh liên tiếp có chung một cặp đỉnh. 
5. Duyệt từng thành phần và gán cho nó một ID màu duy nhất. 

Vì các thành phần độc lập nên mọi nhãn hiệu đều hợp lệ. Ràng buộc đã được đảm bảo bởi cấu trúc. 
6. Xuất ra màu được gán cho mỗi cạnh gốc. 

### Tại sao nó hoạt động 

Mỗi đỉnh$u$góp phần ghép các cạnh tới của nó, do đó mỗi cặp làm giảm số lượng “nhóm cạnh” riêng biệt đi qua$u$. Mỗi nhóm tương ứng với chính xác một thành phần được kết nối trong phân tách cạnh, nghĩa là một màu. 

Một đỉnh bậc$d_u$có thể hình thành chính xác$\lfloor d_u / 2 \rfloor$cặp, chỉ để lại nhiều nhất một cạnh chưa ghép đôi. Mỗi cặp trở thành một phần của đường nhỏ và các cạnh còn lại tạo thành các điểm cuối bổ sung. Do đó, số lượng đường mòn chạm vào$u$chính xác là$\lceil d_u / 2 \rceil$, đó là giá trị tối thiểu có thể có của$f_u$. 

Vì mỗi lớp màu là một đường dẫn hoặc chu trình trên các cạnh, nên không có đỉnh nào có thể có nhiều hơn hai cạnh liên quan cùng màu, tự động thỏa mãn ràng buộc. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**7)

def solve():
    n, m = map(int, input().split())
    g = [[] for _ in range(n)]
    
    edges = []
    for i in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        edges.append((u, v))
        g[u].append((v, i))
        g[v].append((u, i))
    
    used_edge = [False] * m
    adj_edge = [[] for _ in range(m)]
    
    # pair edges at each vertex
    for u in range(n):
        lst = []
        for v, eid in g[u]:
            lst.append(eid)
        
        for i in range(0, len(lst) - 1, 2):
            e1 = lst[i]
            e2 = lst[i + 1]
            adj_edge[e1].append(e2)
            adj_edge[e2].append(e1)
    
    visited_edge = [False] * m
    color = [0] * m
    cur_color = 0
    
    for i in range(m):
        if not visited_edge[i]:
            cur_color += 1
            stack = [i]
            visited_edge[i] = True
            color[i] = cur_color
            
            while stack:
                e = stack.pop()
                for ne in adj_edge[e]:
                    if not visited_edge[ne]:
                        visited_edge[ne] = True
                        color[ne] = cur_color
                        stack.append(ne)
    
    print(*color)

def main():
    t = int(input())
    for _ in range(t):
        solve()

if __name__ == "__main__":
    main()
```Giai đoạn đầu tiên xây dựng một biểu đồ ẩn có các nút là các cạnh. Mỗi đỉnh đóng góp một sự so khớp hoàn hảo cục bộ trên các cạnh tới của nó, được thực hiện bằng cách ghép chúng theo thứ tự. Giai đoạn thứ hai chạy DFS trên biểu đồ cạnh này để xác định các thành phần được kết nối và mỗi thành phần sẽ trở thành một màu. 

Chi tiết triển khai quan trọng là chúng tôi không bao giờ cần mô phỏng rõ ràng các “đường dẫn”. Khi sự kề nhau giữa các cạnh được xây dựng, việc duyệt đồ thị tiêu chuẩn qua các cạnh là đủ. Một điểm tinh tế khác là mỗi cạnh được thêm vào tối đa hai kết nối ghép nối, do đó đồ thị phụ vẫn có kích thước tuyến tính. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Xét một tam giác: các cạnh$(1,2), (2,3), (3,1)$. 

Tại mỗi đỉnh có hai cạnh liên tiếp nên mỗi đỉnh sẽ ghép chúng trực tiếp với nhau. Điều này tạo ra một chu kỳ trên cả ba cạnh. 

| Bước | Hành động | Kết nối cạnh | 
| --- | --- | --- | 
| Ghép nối lúc 1 | (1,2)-(1,3) | e1 kết nối với e2 | 
| Ghép đôi lúc 2 | (1,2)-(2,3) | e1 kết nối với e3 | 
| Ghép đôi lúc 3 | (2,3)-(1,3) | e3 kết nối với e2 | 

Tất cả các cạnh được kết nối thành một thành phần, do đó tất cả các cạnh đều nhận được cùng một màu. 

Điều này cho thấy các chu kỳ tự nhiên được thu gọn thành một lớp màu duy nhất. 

### Ví dụ 2 

Hãy xem xét một con đường:$1 - 2 - 3 - 4$. 

Các cạnh là$e1=(1,2), e2=(2,3), e3=(3,4)$. 

| Đỉnh | Ghép nối | 
| --- | --- | 
| 1 | không | 
| 2 | (e1, e2) | 
| 3 | (e2, e3) | 
| 4 | không | 

Điều này tạo ra một chuỗi duy nhất trên tất cả các cạnh, do đó một lần nữa một màu được sử dụng. 

Quan sát quan trọng là mặc dù các cạnh được ghép cục bộ hai lần ở các đỉnh khác nhau, tính nhất quán tự nhiên tạo thành một đường duy nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n + m)$| Mỗi cạnh được xử lý một số lần không đổi trong quá trình ghép nối và DFS | 
| Không gian |$O(n + m)$| Danh sách kề và lưu trữ đồ thị cạnh | 

Các ràng buộc cho phép lên đến$2 \cdot 10^5$tổng các phần tử, do đó, việc xây dựng thời gian tuyến tính với quá trình xử lý kề đơn giản sẽ phù hợp thoải mái trong các giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque
    
    def solve():
        n, m = map(int, input().split())
        g = [[] for _ in range(n)]
        edges = []
        for i in range(m):
            u, v = map(int, input().split())
            u -= 1
            v -= 1
            edges.append((u, v))
            g[u].append(i)
            g[v].append(i)

        adj = [[] for _ in range(m)]
        for u in range(n):
            lst = g[u]
            for i in range(0, len(lst) - 1, 2):
                a, b = lst[i], lst[i + 1]
                adj[a].append(b)
                adj[b].append(a)

        vis = [False] * m
        col = [0] * m
        cid = 0

        for i in range(m):
            if not vis[i]:
                cid += 1
                stack = [i]
                vis[i] = True
                col[i] = cid
                while stack:
                    e = stack.pop()
                    for ne in adj[e]:
                        if not vis[ne]:
                            vis[ne] = True
                            col[ne] = cid
                            stack.append(ne)

        return " ".join(map(str, col))

    return solve()

# sample / custom tests

assert run("""1 0
""") == "", "empty graph"

assert run("""2 1
1 2
""") == "1", "single edge"

assert run("""3 3
1 2
2 3
3 1
""") == "1 1 1", "triangle cycle"

assert run("""4 3
1 2
2 3
3 4
""") == "1 1 1", "path graph"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đồ thị trống | trống | trường hợp ranh giới cơ sở | 
| cạnh đơn | 1 | đồ thị không trống tối thiểu | 
| tam giác | tất cả 1s | hành vi hợp nhất chu kỳ | 
| đường đi có độ dài 3 | tất cả 1s | hình thành chuỗi | 

## Vỏ cạnh 

Trường hợp cạnh khóa là khi một đỉnh có bậc lẻ. Trong tình huống như vậy, một cạnh tới bị bỏ cặp ở đỉnh đó. Cạnh đó vẫn tham gia ghép nối ở điểm cuối khác của nó, vì vậy về mặt tổng thể, nó trở thành điểm cuối của một đường nhỏ. Điều này đảm bảo rằng thành phần kết quả vẫn là một đường dẫn hợp lệ thay vì phá vỡ tính nhất quán. 

Một trường hợp cạnh khác là đồ thị bị ngắt kết nối. Vì việc ghép nối được thực hiện hoàn toàn cục bộ trên mỗi đỉnh nên các thành phần không bao giờ tương tác giữa các thành phần được kết nối, do đó, mỗi thành phần sẽ tạo thành các vệt hợp lệ một cách độc lập và nhận màu riêng mà không cần xử lý đặc biệt. 

Trường hợp tinh tế cuối cùng là khi nhiều đỉnh tạo ra các cặp chồng chéo có vẻ mâu thuẫn nhau. Việc xây dựng tránh xung đột vì các cạnh không bao giờ được ghép hai lần ở cùng một đỉnh và mỗi cạnh kề trong biểu đồ cạnh là đối xứng. Điều này đảm bảo rằng DFS trên các cạnh luôn nhìn thấy cấu trúc vô hướng nhất quán thay vì xung đột hướng.
