---
title: "CF 104308J - Người ngoài hành tinh du hành Masud"
description: "Bản đồ trái đất có thể được mô hình hóa dưới dạng biểu đồ có hướng trong đó mỗi thành phố là một nút và mỗi đường một chiều là một cạnh có hướng."
date: "2026-07-01T20:03:38+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104308
codeforces_index: "J"
codeforces_contest_name: "Mirror of Independence Day Programming Contest 2023 by MIST Computer Club"
rating: 0
weight: 104308
solve_time_s: 51
verified: true
draft: false
---

[CF 104308J - Masud du hành ngoài hành tinh](https://codeforces.com/problemset/problem/104308/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bản đồ trái đất có thể được mô hình hóa dưới dạng biểu đồ có hướng trong đó mỗi thành phố là một nút và mỗi đường một chiều là một cạnh có hướng. Hai thành phố thuộc về cùng một quốc gia khi chúng có thể tiếp cận được lẫn nhau, nghĩa là có một đường đi có hướng từ thành phố thứ nhất đến thành phố thứ hai và cũng có một đường đi có hướng ngược lại. Đây chính xác là định nghĩa của một thành phần được kết nối chặt chẽ, do đó biểu đồ được phân chia một cách tự nhiên thành các SCC và mỗi SCC hoạt động như một quốc gia duy nhất. 

Sau khi nén biểu đồ thành SCC, chúng ta nhận được biểu đồ chu kỳ có hướng trong đó các nút là các quốc gia và các cạnh thể hiện rằng có ít nhất một con đường tồn tại từ thành phố này ở quốc gia này đến thành phố ở quốc gia khác. 

Masud chỉ có thể chuẩn bị hồ sơ cho tối đa hai quốc gia, nhưng anh được phép đi lại tự do trên các con đường. Mục tiêu là tối đa hóa số lượng thành phố riêng biệt mà anh ta có thể ghé thăm trong khi không bao giờ rời khỏi liên minh của tối đa hai SCC. 

Vì vậy, vấn đề giảm xuống còn việc chọn một hoặc hai SCC sao cho tất cả các thành phố được ghé thăm đều nằm hoàn toàn bên trong các SCC đó và có thể đến được bằng các con đường có chỉ dẫn. 

Điều tinh tế quan trọng là việc chọn hai SCC chỉ hữu ích khi có thể di chuyển từ SCC này sang SCC kia thông qua các cạnh có hướng trong biểu đồ gốc. Nếu không có con đường được định hướng kết nối chúng theo hướng có thể sử dụng được, anh ta không thể đi qua giữa chúng trong một kế hoạch du hành. 

Các ràng buộc rất lớn, lên tới 100.000 thành phố và 100.000 cạnh cho mỗi trường hợp thử nghiệm. Điều này ngay lập tức loại trừ mọi cách tiếp cận bậc hai đối với các nút hoặc cặp SCC. Ngay cả việc lặp lại tất cả các cặp thành phần cũng sẽ quá chậm. Cần có thuật toán đồ thị tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm, chẳng hạn như phân tách SCC trong O(n + m). 

Một vài trường hợp cạnh rất dễ bị bỏ sót. Nếu không có cạnh nào, mỗi thành phố là quốc gia riêng của nó và câu trả lời đơn giản là kích thước SCC lớn nhất, bằng 1. Nếu biểu đồ đã được kết nối mạnh, câu trả lời là tất cả các nút vì chỉ tồn tại một quốc gia. Một trường hợp phức tạp khác là khi các SCC tạo thành chuỗi A → B → C. Việc chọn A và C cùng nhau là không thể vì việc đi lại sẽ phải đi qua B, điều này sẽ dẫn đến một quốc gia thứ ba, vi phạm ràng buộc. 

## Phương pháp tiếp cận 

Một cách giải thích mạnh mẽ sẽ là xem xét mọi thành phố có thể bắt đầu, mô phỏng tất cả các chuyến đi bộ có thể và theo dõi số lượng thành phố có thể được ghé thăm trong khi đảm bảo rằng cuộc đi bộ chỉ đi vào tối đa hai SCC. Điều này sẽ liên quan đến việc khám phá các đường dẫn trong biểu đồ gốc và duy trì một tập hợp các thành phần đã truy cập. Trong trường hợp xấu nhất, mỗi lần truyền tải sẽ phân nhánh qua nhiều cạnh, dẫn đến hành vi hàm mũ. Ngay cả việc hạn chế SCC cũng không giúp ích nhiều nếu chúng ta vẫn thử tất cả các cặp thành phần và kiểm tra khả năng tiếp cận giữa chúng, điều này sẽ dẫn đến việc kiểm tra O(k²) trong đó k có thể lên tới n. 

Cấu trúc của bài toán trở nên đơn giản hơn nhiều khi chúng ta nén SCC. Sau khi nén, chúng ta có DAG. Bất kỳ chuyến đi hợp lệ nào sử dụng tối đa hai quốc gia đều phải tương ứng với việc ở trong một SCC hoặc di chuyển dọc theo một cạnh có hướng duy nhất từ ​​SCC A đến SCC B rồi dừng lại. Lý do là khi chúng tôi vào SCC thứ ba, chúng tôi sẽ vượt quá số lượng quốc gia được phép. 

Quan sát này làm giảm vấn đề xuống chỉ còn đánh giá hai loại lựa chọn. Đầu tiên là kích thước của từng SCC riêng lẻ. Thứ hai là tổng kích thước của hai SCC được kết nối bằng một cạnh có hướng trong biểu đồ ngưng tụ. 

Chúng ta không cần xem xét các đường dẫn dài hơn trong DAG vì bất kỳ đường dẫn nào có độ dài ít nhất là hai sẽ nhất thiết phải liên quan đến ba SCC, điều này bị cấm. Điều này thu gọn vấn đề từ việc truyền tải đồ thị toàn cục thành một vấn đề tổng hợp dựa trên cạnh đơn giản trên các kích thước SCC.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (đường dẫn / cặp thành phố hoặc SCC) | O(n²) hoặc tệ hơn | O(n + m) | Quá chậm | 
| Kiểm tra cặp SCC + Edge | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi bắt đầu bằng cách xác định các thành phần được kết nối mạnh mẽ bằng thuật toán SCC tiêu chuẩn như Kosaraju hoặc Tarjan. Mỗi SCC được chỉ định một id và chúng tôi cũng tính toán kích thước của nó, đại diện cho số lượng thành phố bên trong quốc gia đó. 

Tiếp theo, chúng tôi quét tất cả các cạnh trong biểu đồ gốc. Đối với mỗi cạnh có hướng u → v, nếu u và v thuộc các SCC khác nhau, chúng ta ghi lại một kết nối từ thành phần cu đến thành phần cv. Chúng tôi không cần lưu trữ cấu trúc DAG đầy đủ; chúng ta chỉ cần biết cặp thành phần nào được kết nối trực tiếp. 

Sau đó, chúng tôi tính toán hai loại ứng cử viên cho câu trả lời. Đầu tiên, chúng tôi xem xét từng SCC riêng lẻ và lấy kích thước của nó làm câu trả lời khả thi. Điều này tương ứng với việc chỉ đến thăm một quốc gia. 

Thứ hai, đối với mọi cạnh có hướng giữa các SCC cu → cv, chúng ta xem xét tổng size[cu] + size[cv]. Điều này tương ứng với việc bắt đầu ở một quốc gia và di chuyển vào đúng một quốc gia láng giềng, sau đó dừng lại. 

Cuối cùng, chúng tôi lấy mức tối đa trên tất cả các kích thước SCC đơn lẻ và tất cả các cặp cạnh SCC hợp lệ. 

### Tại sao nó hoạt động 

Bất kỳ kế hoạch du lịch hợp lệ nào cũng chỉ có thể bao gồm các thành phố từ tối đa hai SCC. Nếu gói chỉ sử dụng một SCC thì gói đó sẽ được ghi lại hoàn toàn theo kích thước SCC. Nếu nó sử dụng hai SCC thì phải tồn tại ít nhất một cạnh cho phép di chuyển từ SCC này sang SCC kia theo hướng di chuyển. Khi quá trình chuyển đổi đó xảy ra, việc truy cập bất kỳ SCC thứ ba nào sẽ yêu cầu một quá trình chuyển đổi khác, điều này không được phép. Do đó, mọi giải pháp hợp lệ đều tương ứng chính xác với một cặp SCC hoặc một cặp SCC được kết nối bởi ít nhất một cạnh có hướng và việc đánh giá các cạnh này một cách toàn diện bao gồm tất cả các trường hợp khả thi. 

## Giải pháp Python```python
import sys
sys.setrecursionlimit(10**7)
input = sys.stdin.readline

def kosaraju(n, adj, radj):
    visited = [False] * n
    order = []

    def dfs1(u):
        visited[u] = True
        for v in adj[u]:
            if not visited[v]:
                dfs1(v)
        order.append(u)

    for i in range(n):
        if not visited[i]:
            dfs1(i)

    comp = [-1] * n
    cid = 0

    def dfs2(u):
        comp[u] = cid
        for v in radj[u]:
            if comp[v] == -1:
                dfs2(v)

    for u in reversed(order):
        if comp[u] == -1:
            dfs2(u)
            cid += 1

    return comp, cid

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        adj = [[] for _ in range(n)]
        radj = [[] for _ in range(n)]

        edges = []
        for _ in range(m):
            u, v = map(int, input().split())
            u -= 1
            v -= 1
            adj[u].append(v)
            radj[v].append(u)
            edges.append((u, v))

        comp, k = kosaraju(n, adj, radj)

        size = [0] * k
        for i in range(n):
            size[comp[i]] += 1

        ans = max(size)

        for u, v in edges:
            cu, cv = comp[u], comp[v]
            if cu != cv:
                ans = max(ans, size[cu] + size[cv])

        print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp trước tiên là xây dựng danh sách lân cận thuận và ngược để hỗ trợ DFS hai lượt của Kosaraju. DFS đầu tiên xây dựng thứ tự hoàn thiện và DFS thứ hai gán id thành phần trên biểu đồ đảo ngược. 

Sau khi nén SCC, mảng kích thước sẽ theo dõi xem mỗi quốc gia có bao nhiêu thành phố. Câu trả lời ban đầu được đặt thành SCC lớn nhất vì việc đến thăm một quốc gia luôn được phép. 

Sau đó chúng tôi lặp qua tất cả các cạnh ban đầu. Mỗi cạnh vượt qua ranh giới SCC thể hiện một hành trình tiềm năng giữa hai quốc gia. Chúng tôi tổng hợp kích thước của các thành phần điểm cuối của nó và cập nhật câu trả lời. Các cạnh trùng lặp hoặc nhiều cạnh giữa cùng một cặp SCC không ảnh hưởng đến tính chính xác vì việc lấy mức tối đa là không có giá trị. 

## Ví dụ đã hoạt động 

Xét một đồ thị có hai chu trình được nối với nhau bằng một cạnh: 1 → 2 → 3 → 1 và 4 → 5 → 6 → 4, cộng với một cạnh 3 → 4. 

Sau khi nén SCC, chúng ta có hai thành phần: C1 với kích thước 3 và C2 với kích thước 3 và một cạnh C1 → C2. 

| Bước | Hành động | cỡ C1 | Kích thước C2 | Câu trả lời hay nhất | 
| --- | --- | --- | --- | --- | 
| 1 | Xây dựng SCC | 3 | 3 | 3 | 
| 2 | Cạnh xử lý C1 → C2 | 3 | 3 | 6 | 

Điều này cho thấy chiến lược tốt nhất là đi từ quốc gia đầu tiên sang quốc gia thứ hai và dừng lại ở đó. 

Bây giờ hãy xem xét một chuỗi gồm ba SCC: A → B → C với kích thước 2, 5 và 4. 

| Bước | Hành động | Ứng viên | 
| --- | --- | --- | 
| 1 | SCC đơn lẻ | tối đa là 5 | 
| 2 | Cạnh A → B | 7 | 
| 3 | Cạnh B → C | 9 | 

Chúng tôi không bao giờ xem xét A → C vì không tồn tại cạnh trực tiếp và việc sử dụng B làm trung gian sẽ vượt quá giới hạn hai quốc gia. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Kosaraju chạy theo thời gian tuyến tính và mỗi cạnh được xử lý một lần | 
| Không gian | O(n + m) | Danh sách kề, đồ thị ngược và mảng SCC | 

Giải pháp phù hợp thoải mái trong giới hạn vì cả n và m đều lên tới 100.000 cho mỗi trường hợp thử nghiệm và tất cả các hoạt động đều là quét tuyến tính hoặc truyền tải DFS. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    # assume solve() is defined above
    solve()

    sys.stdout = sys.__stdout__
    return out.getvalue().strip()

# minimum graph
assert run("1\n1 0\n") == "1"

# simple two-node chain
assert run("1\n2 1\n1 2\n") == "2"

# strongly connected cycle
assert run("1\n3 3\n1 2\n2 3\n3 1\n") == "3"

# chain of SCCs
assert run("1\n4 3\n1 2\n2 3\n3 4\n") == "3"

# two separate cycles with connection
assert run("1\n6 7\n1 2\n2 1\n3 4\n4 3\n2 3\n") == "6"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 1 | xử lý SCC tối thiểu | 
| cạnh đơn | 2 | chuyển đổi hai nước | 
| chu kỳ đầy đủ | n | đã là một nước | 
| chuỗi tuyến tính | chỉ cặp liền kề tốt nhất | cấm bỏ qua SCC | 
| hai chu kỳ được kết nối | tổng cặp tốt nhất | Tính chính xác của tổng hợp SCC | 

## Vỏ cạnh 

Một biểu đồ không có cạnh sẽ tạo ra n SCC riêng biệt, mỗi SCC có kích thước 1. Thuật toán coi mỗi nút là thành phần riêng của nó và câu trả lời tốt nhất sẽ trở thành kích thước SCC lớn nhất, bằng 1. Vì không tồn tại cạnh thành phần chéo nên giai đoạn thứ hai không bao giờ cập nhật câu trả lời ngoài giá trị này. 

Trong đồ thị được kết nối mạnh đầy đủ, tất cả các nút thuộc về một SCC. Sự phân rã SCC tạo ra một thành phần duy nhất có kích thước n. Bước xử lý cạnh không có ích gì vì không có cạnh liên thành phần nào. Câu trả lời cuối cùng vẫn là n, phù hợp với thực tế là chỉ có một quốc gia tồn tại. 

Trong cấu trúc chuỗi như 1 → 2 → 3 → 4, SCC đều là các nút đơn. Các cặp hợp lệ duy nhất được xem xét là (1,2), (2,3) và (3,4). Thuật toán không bao giờ xem xét (1,3) hoặc (1,4), điều này ngăn chặn chính xác các giao dịch truyền tải đa quốc gia không hợp lệ đòi hỏi phải đi qua các thành phần trung gian.
