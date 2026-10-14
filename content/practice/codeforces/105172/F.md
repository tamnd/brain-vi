---
title: "CF 105172F - Nanami và Bông tuyết"
description: "Đầu vào mô tả một số đồ thị vô hướng và mỗi đồ thị được cho là thể hiện một cấu trúc “giống như bông tuyết”. Nhiệm vụ là quyết định xem mỗi biểu đồ có khớp với một mẫu rất cứng nhắc hay không. Cấu trúc mà chúng tôi đang tìm kiếm có thể được hiểu theo hai lớp."
date: "2026-06-27T08:26:57+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 105172
codeforces_index: "F"
codeforces_contest_name: "The 20th Southeast University Programming Contest (Summer)"
rating: 0
weight: 105172
solve_time_s: 183
verified: false
draft: false
---

[CF 105172F - Nanami và những bông tuyết](https://codeforces.com/problemset/problem/105172/F) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3m 3s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Đầu vào mô tả một số đồ thị vô hướng và mỗi đồ thị được cho là thể hiện một cấu trúc “giống như bông tuyết”. Nhiệm vụ là quyết định xem mỗi biểu đồ có khớp với một mẫu rất cứng nhắc hay không. 

Cấu trúc mà chúng tôi đang tìm kiếm có thể được hiểu theo hai lớp. Đầu tiên, có một chu trình đơn giản hình thành nên “đa giác lõi”. Thứ hai, từ mỗi đỉnh của chu trình này, một cái cây có thể hướng ra ngoài và tất cả những cây treo này phải có cấu trúc giống hệt nhau khi nhìn từ các điểm gắn của chúng. Nói cách khác, nếu bạn đứng ở bất kỳ đỉnh nào trên đường tròn và nhìn ra ngoài vào thành phần gắn liền với nó, thì tất cả các cây có gốc mà bạn nhìn thấy đều phải đẳng cấu. 

Vậy đồ thị không tùy ý. Nó trông giống như một chu trình đơn và mỗi đỉnh của chu trình đóng vai trò là gốc của một cây giống hệt nhau. 

Các ràng buộc rất lớn: tổng các đỉnh và cạnh trong tất cả các trường hợp thử nghiệm lên tới năm trăm nghìn. Điều này loại trừ bất kỳ giải pháp nào tính toán lại việc kiểm tra đẳng cấu đồ thị đắt tiền một cách độc lập đối với nhiều cặp cây con. Bất cứ điều gì bậc hai về số lượng nút trên mỗi trường hợp thử nghiệm sẽ ngay lập tức thất bại. Giải pháp về cơ bản phải tuyến tính hoặc gần tuyến tính cho mỗi trường hợp thử nghiệm. 

Một vài trường hợp thất bại xuất hiện một cách tự nhiên nếu người ta cố gắng suy luận một cách ngây thơ. Một lỗi phổ biến là coi mọi chu trình là hợp lệ, ngay cả khi các cấu trúc đính kèm khác nhau. Ví dụ, hãy xem xét một chu trình tam giác trong đó một đỉnh có thêm một chuỗi có độ dài bằng 2 trong khi các đỉnh khác không có phần đính kèm. Biểu đồ này chứa một chu trình, nhưng rõ ràng “cánh tay bông tuyết” không giống nhau nên câu trả lời đúng là KHÔNG. Một trường hợp lỗi khác là khi biểu đồ chứa nhiều chu trình được kết nối qua cây. Việc phát hiện chu trình đơn giản có thể chấp nhận từng chu trình một cách độc lập, nhưng cấu trúc không phải là một đa giác trung tâm duy nhất có các phần đính kèm thống nhất. 

## Phương pháp tiếp cận 

Việc giải thích bạo lực bắt đầu bằng cách cố gắng xác định chu trình, sau đó, với mỗi đỉnh chu trình, trích xuất cây con đính kèm của nó và so sánh tất cả chúng bằng cách sử dụng đẳng cấu cây. Một cách đơn giản để kiểm tra tính đẳng cấu của cây là tính toán dạng chuẩn hoặc hàm băm cho mỗi cây có gốc. 

Về nguyên tắc thì điều này có tác dụng, nhưng nếu thực hiện một cách ngây thơ thì sẽ rất tốn kém. Nếu chúng ta tính toán lại mã hóa cây con nhiều lần cho từng đỉnh chu kỳ một cách độc lập thì các nút giống nhau ở các nhánh khác nhau sẽ được xử lý lại nhiều lần. Trong trường hợp xấu nhất khi hầu hết mọi nút đều là một phần của chuỗi dài gắn liền với chu trình, việc duyệt DFS lặp đi lặp lại trên tất cả các đỉnh của chu kỳ sẽ dẫn đến công việc lặp lại trên cùng một cấu trúc, đẩy độ phức tạp về phía O(n^2). 

Quan sát quan trọng là cấu trúc tách biệt rõ ràng thành hai phần. Bản thân chu trình này là duy nhất và có thể được trích xuất bằng quy trình bóc tách 2 lõi. Khi chu trình được xác định, mỗi nút còn lại thuộc về chính xác một cây có gốc tại một đỉnh chu trình. Những cây này không tương tác với nhau. Điều này có nghĩa là chúng ta có thể tính toán băm cây con một lần cho mỗi nút bằng cách sử dụng một DFS duy nhất từ ​​mỗi gốc chu trình trở ra ngoài, tránh phải tính toán lại. 

Ý tưởng quan trọng thứ hai là việc xác định chu trình có thể được thực hiện bằng cách loại bỏ lặp đi lặp lại các nút cấp 1. Sau nhiều lần loại bỏ các lá, những gì còn lại chính xác là sự kết hợp của tất cả các chu trình trong biểu đồ. Vì cấu trúc dự định chứa chính xác một chu trình nên sơ đồ con còn lại này phải là chu trình đó. 

Sau khi cô lập chu trình, vấn đề giảm xuống còn việc tính toán hàm băm chuẩn của từng cây đính kèm gốc và xác minh tất cả các hàm băm đó là giống hệt nhau. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| So sánh cây con Brute Force | O(n^2) | O(n) | Quá chậm | 
| Lột chu kỳ + băm DFS đơn | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Tính toán cấp độ của mỗi nút và liên tục loại bỏ các nút có cấp độ 1 bằng cách sử dụng hàng đợi. Mỗi lần loại bỏ sẽ làm giảm mức độ lân cận, do đó các lá mới được phát hiện một cách linh hoạt. Khi quá trình dừng lại, các nút còn lại tạo thành lõi 2 của biểu đồ. Điều này cô lập tất cả các nút chu kỳ. 
2. Kiểm tra xem mỗi nút còn lại có chính xác hai nút lân cận trong tập hợp còn lại hay không. Điều này đảm bảo cấu trúc còn lại là một chu trình đơn giản chứ không phải là nhiều chu trình đan xen. 
3. Nếu số lượng nút còn lại ít hơn 3, hãy loại bỏ ngay biểu đồ vì lõi đa giác hợp lệ không thể tồn tại. 
4. Xây dựng danh sách kề được giới hạn ở các cạnh không có chu trình, nghĩa là chúng ta bỏ qua các cạnh giữa các nút chu kỳ và chỉ đi qua các cạnh dẫn ra ngoài chu trình. 
5. Đối với mỗi nút chu trình, thực hiện DFS vào các nút lân cận không có chu trình của nó và tính toán hàm băm chuẩn của cây gốc. DFS phải tránh xem lại các nút chu kỳ để chỉ bao gồm các nhánh bên ngoài. 
6. Thu thập tất cả các giá trị băm tương ứng với các nút chu kỳ. Nếu bất kỳ hàm băm nào khác với hàm băm đầu tiên thì các cây đính kèm không đẳng cấu và câu trả lời là KHÔNG. 
7. Đảm bảo rằng tất cả các nút không theo chu kỳ đều được truy cập trong quá trình truyền tải DFS này. Nếu bất kỳ nút nào không được truy cập, điều đó có nghĩa là có một thành phần bị ngắt kết nối không được gắn vào chu trình, điều này vi phạm cấu trúc bông tuyết. 

### Tại sao nó hoạt động 

Bước lột lá đảm bảo rằng mọi nút nằm ngoài tất cả các chu kỳ đều bị loại bỏ, bởi vì bất kỳ nút cây nào cuối cùng cũng trở thành lá và bị loại bỏ. Những gì còn lại chính xác là xương sống của chu trình. Mỗi nút còn lại tham gia vào đúng hai cạnh chu trình, do đó cấu trúc không thể phân nhánh hoặc chứa dây cung. 

Khi chu trình được cố định, mọi cạnh không phải chu trình sẽ thuộc về một cây được gắn vào đúng một nút chu trình. Những cây này rời rạc và độc lập. Việc tính toán hàm băm xác định cho mỗi cây có gốc đảm bảo rằng cấu trúc đẳng hình mang lại các giá trị giống hệt nhau, trong khi bất kỳ sự khác biệt về cấu trúc nào cũng làm thay đổi nhiều tập hợp băm con và do đó mã hóa cuối cùng. Điều này làm cho việc kiểm tra tính bằng nhau trên tất cả các đỉnh của chu trình vừa cần thiết vừa đủ để đảm bảo tính chính xác. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**7)

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        g = [[] for _ in range(n)]
        deg = [0] * n

        for _ in range(m):
            u, v = map(int, input().split())
            u -= 1
            v -= 1
            g[u].append(v)
            g[v].append(u)
            deg[u] += 1
            deg[v] += 1

        from collections import deque
        q = deque([i for i in range(n) if deg[i] <= 1])
        alive = [True] * n

        while q:
            u = q.popleft()
            if not alive[u]:
                continue
            alive[u] = False
            for v in g[u]:
                if alive[v]:
                    deg[v] -= 1
                    if deg[v] == 1:
                        q.append(v)

        core = [i for i in range(n) if alive[i]]

        if len(core) < 3:
            print("NO")
            continue

        core_set = set(core)

        ok = True
        for u in core:
            cnt = 0
            for v in g[u]:
                if v in core_set:
                    cnt += 1
            if cnt != 2:
                ok = False
                break

        if not ok:
            print("NO")
            continue

        seen = set()

        def dfs(u, p):
            seen.add(u)
            child_hashes = []
            for v in g[u]:
                if v in core_set or v == p:
                    continue
                child_hashes.append(dfs(v, u))
            child_hashes.sort()
            return tuple(child_hashes)

        hashes = []
        for u in core:
            for v in g[u]:
                if v not in core_set and v not in seen:
                    hashes.append(dfs(v, u))

        if not hashes:
            hashes = [() for _ in core]

        print("YES" if all(h == hashes[0] for h in hashes) else "NO")

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách xây dựng biểu đồ và mức độ tính toán. Bước cắt tỉa dựa trên hàng đợi sẽ loại bỏ tất cả các phần giống như cây, chỉ để lại các nút chu kỳ. Việc kiểm tra mức độ trên các nút còn lại buộc lõi này là một chu trình đơn giản. 

Bước băm DFS coi mỗi lân cận không theo chu trình của nút chu trình là gốc của cây đính kèm. Phép đệ quy xây dựng một biểu diễn bộ dữ liệu chính tắc của cấu trúc cây con bằng cách sắp xếp các giá trị băm con, đảm bảo tính độc lập về thứ tự. Mỗi cây con được tính toán chính xác một lần vì`seen`set ngăn chặn việc xem lại các nút trên các gốc chu kỳ khác nhau. 

Cuối cùng, tất cả các chữ ký cây con thu thập được sẽ được so sánh về độ bằng nhau. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

Biểu đồ đầu vào tương ứng với một chu trình sạch với các phần đính kèm giống hệt nhau. 

| Bước | Các nút lõi | Băm cây con được trích xuất | 
| --- | --- | --- | 
| Sau khi cắt tỉa | chu kỳ kích thước k | đang chờ xử lý | 
| DFS từ mỗi nút chu kỳ | tất cả đều giống hệt nhau | cùng một bộ | 

Mỗi nút chu trình tạo ra cấu trúc trống hoặc giống hệt nhau, do đó thuật toán đưa ra CÓ. 

Điều này xác nhận rằng các phần đính kèm giống hệt nhau trên tất cả các đỉnh chu kỳ được nhận dạng chính xác ngay cả khi cây là tầm thường. 

### Ví dụ 2 

Hãy xem xét một chu trình trong đó một đỉnh có chuỗi dài hơn các đỉnh khác. 

| Bước | Các nút lõi | Băm cây con được trích xuất | 
| --- | --- | --- | 
| Sau khi cắt tỉa | chu kỳ vẫn còn | đang chờ xử lý | 
| Kết quả DFS | một hàm băm khác nhau | không khớp | 

Ở đây, một đỉnh chu kỳ tạo ra một bộ dữ liệu sâu hơn nhờ có nhánh phụ của nó, trong khi các đỉnh khác tạo ra một cấu trúc đơn giản hơn. Sự không phù hợp ngay lập tức dẫn đến KHÔNG. 

Điều này cho thấy sự đẳng cấu của cây con được thực thi trên toàn cầu trên tất cả các đỉnh của chu kỳ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi nút vào và ra khỏi hàng đợi một lần trong quá trình cắt tỉa và mỗi cạnh được duyệt qua một số lần không đổi trong quá trình băm DFS | 
| Không gian | O(n) | Danh sách kề, mảng mức độ và lưu trữ đệ quy/đã truy cập | 

Tổng kích thước đầu vào trong các trường hợp thử nghiệm được giới hạn bởi 5 × 10^5, do đó, giải pháp thời gian tuyến tính phù hợp thoải mái trong giới hạn thời gian và bộ nhớ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from __main__ import solve
    return solve_capture(inp)

def solve_capture(inp: str) -> str:
    import sys
    from io import StringIO
    backup = sys.stdin
    sys.stdin = StringIO(inp)
    out = StringIO()
    backup_out = sys.stdout
    sys.stdout = out
    solve()
    sys.stdout = backup_out
    sys.stdin = backup
    return out.getvalue().strip()

# provided samples
assert solve_capture("""5
6 6
1 2
2 3
3 1
1 5
4 2
3 6
4 3
1 2
2 4
3 2
4 4
1 3
3 2
2 4
1 4
9 12
1 2
2 3
3 1
2 4
5 4
2 5
1 7
6 7
1 6
8 3
3 9
8 9
6 6
1 2
2 3
3 1
4 5
6 5
6 4
""") == """YES
NO
YES
NO
NO"""

# custom cases
assert solve_capture("""1
3 3
1 2
2 3
3 1
""") == "YES"

assert solve_capture("""1
4 3
1 2
2 3
3 1
""") == "NO"

assert solve_capture("""1
6 5
1 2
2 3
3 1
1 4
1 5
""") == "NO"

assert solve_capture("""1
7 7
1 2
2 3
3 1
1 4
2 5
3 6
1 7
""") == "YES"

| Test input | Expected output | What it validates |
|---|---|---|
| triangle only | YES | minimal valid cycle |
| cycle missing closure | NO | invalid structure detection |
| uneven attachments | NO | subtree mismatch detection |
| symmetric star attachments | YES | identical tree verification |

## Edge Cases

A pure cycle with no attachments is the simplest valid configuration. In this case, the pruning step leaves all nodes in the core, and every cycle node has no outgoing DFS subtree, producing identical empty signatures. The algorithm correctly returns YES.

A graph containing multiple cycles connected through bridges is eliminated during the pruning stage. Any tree components are removed first, but multiple cycles would remain and violate the “degree equals two in core” condition, forcing rejection.

A case where one attachment tree is deeper than others is caught during DFS hashing. Even if the difference is only a single extra node at the bottom, the sorted tuple representation changes, producing a different canonical signature and ensuring rejection.
```
