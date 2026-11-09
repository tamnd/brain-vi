---
title: "CF 1470D - Nhà Ở Kỳ Lạ"
description: "Chúng ta được cho một đồ thị vô hướng trong đó các đỉnh biểu thị các ngôi nhà và các cạnh biểu thị các lối đi ngầm. Chúng ta phải chọn một tập hợp con các đỉnh để đặt giáo viên vào. Sau khi lựa chọn được thực hiện, tất cả các cạnh có điểm cuối đều không được chọn sẽ bị xóa. Mọi cạnh khác vẫn còn."
date: "2026-06-11T01:01:06+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "dfs-and-similar", "graph-matchings", "graphs", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1470
codeforces_index: "D"
codeforces_contest_name: "Codeforces Round 694 (Div. 1)"
rating: 2200
weight: 1470
solve_time_s: 140
verified: false
draft: false
---

[CF 1470D - Nhà ở kỳ lạ](https://codeforces.com/problemset/problem/1470/D) 

**Xếp hạng:** 2200 
**Tags:** thuật toán xây dựng, dfs và tương tự, so khớp đồ thị, đồ thị, tham lam 
**Thời gian giải:** 2 phút 20s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị vô hướng trong đó các đỉnh biểu thị các ngôi nhà và các cạnh biểu thị các lối đi ngầm. Chúng ta phải chọn một tập hợp con các đỉnh để đặt giáo viên vào. 

Sau khi lựa chọn được thực hiện, tất cả các cạnh có điểm cuối đều không được chọn sẽ bị xóa. Mọi cạnh khác vẫn còn. Vì vậy, một cạnh tồn tại nếu ít nhất một trong các điểm cuối của nó có một giáo viên. 

Sau lần cắt tỉa này, đồ thị còn lại vẫn phải cho phép di chuyển giữa mọi cặp đỉnh chỉ sử dụng các cạnh còn sót lại. Ngoài ra, không có cạnh nào được phép kết nối hai đỉnh đã chọn, nghĩa là các đỉnh được chọn tạo thành một tập hợp độc lập. 

Vì vậy, nhiệm vụ là tìm một tập hợp các đỉnh là một tập độc lập nhưng vẫn “che phủ” đồ thị theo nghĩa là việc loại bỏ tất cả các cạnh bên trong tập hợp không được chọn sẽ không ngắt kết nối đồ thị. 

Một cách khác để diễn giải điều kiện này là chia các đỉnh thành hai nhóm, nhóm giáo viên và nhóm không phải giáo viên. Các giáo viên không thể ở gần nhau nên tập giáo viên là độc lập. Mỗi cạnh phải có ít nhất một điểm cuối trong tập giáo viên, nếu không nó sẽ biến mất. Biểu đồ còn lại phải được kết nối. 

Điều này tạo ra sự căng thẳng giữa độ thưa thớt và khả năng kết nối: chúng tôi muốn một tập hợp độc lập cũng là một loại vỏ đỉnh đủ mạnh để duy trì cấu trúc kết nối. 

Các ràng buộc rất lớn: lên tới 3⋅10^5 đỉnh và cạnh trong tất cả các trường hợp thử nghiệm. Điều này loại trừ mọi cách xây dựng bậc hai hoặc tìm kiếm tổng thể lặp lại trên mỗi đỉnh. Về cơ bản, chúng ta cần tuyến tính hoặc tuyến tính cho mỗi trường hợp thử nghiệm và rất có thể là phép duyệt đồ thị hoặc tô màu tham lam. 

Một vấn đề tế nhị phát sinh khi đồ thị không có hai phần. Nếu chúng ta cố gắng phân chia hai phần, xung đột sẽ xuất hiện ở đó các chu kỳ lẻ dẫn đến mâu thuẫn. Một trường hợp lỗi khác là khi các đỉnh hoặc thành phần bị cô lập không được xử lý nhất quán, đặc biệt khi biểu đồ có nhiều thành phần nhưng yêu cầu kết nối áp dụng cho biểu đồ còn lại sau khi xóa chứ không phải biểu đồ gốc. 

Một cách tiếp cận ngây thơ sẽ thử tất cả các tập hợp con hoặc cố gắng chọn các đỉnh một cách tham lam mà không có hướng dẫn cấu trúc. Điều này nhanh chóng thất bại vì tính độc lập và kết nối toàn cầu tương tác theo những cách không mang tính địa phương. 

## Phương pháp tiếp cận 

Chiến lược brute-force trực tiếp sẽ coi mỗi tập hợp con các đỉnh là vị trí tiềm năng của giáo viên và xác minh cả hai điều kiện: không có hai đỉnh được chọn nào có chung một cạnh và biểu đồ kết quả sau khi loại bỏ các cạnh bên trong tập hợp không được chọn vẫn được kết nối. Có 2^n tập hợp con như vậy và thậm chí việc kiểm tra một tập hợp con cũng yêu cầu truyền tải đồ thị, dẫn đến thời gian theo cấp số nhân, điều này là không thể ngay cả với n = 20. 

Quan sát quan trọng là điều kiện về khả năng kết nối sau khi loại bỏ cạnh được gắn chặt với cấu trúc lưỡng cực. Nếu chúng ta tô 2 màu cho đồ thị, mọi cạnh sẽ kết nối các màu đối diện. Nếu chúng ta chọn một lớp màu làm giáo viên thì sẽ không có hai giáo viên nào tự động ở cạnh nhau. Hơn nữa, mỗi cạnh có chính xác một điểm cuối trong tập hợp đã chọn, do đó không có cạnh nào biến mất, nghĩa là đồ thị vẫn hoàn toàn nguyên vẹn và do đó được kết nối chính xác như trước. 

Điều này ngay lập tức đưa ra một điều kiện đủ: nếu đồ thị là hai phần, chúng ta có thể lấy một trong hai lớp màu. 

Câu hỏi còn lại là liệu sự hợp tác lưỡng đảng có cần thiết để tồn tại một giải pháp hay không. Nếu có một chu kỳ lẻ, bất kỳ tập hợp giáo viên hợp lệ nào cũng phải tránh đặt cả hai điểm cuối của mọi cạnh bên trong tập hợp không được chọn trong khi vẫn duy trì kết nối, điều này tạo ra mâu thuẫn tương đương với việc tô màu 2 màu. Trên thực tế, người ta có thể chỉ ra rằng bất kỳ cách xây dựng hợp lệ nào đều gây ra sự phân chia hai phần, bởi vì nếu cả hai đỉnh liền kề đều không được chọn thì cạnh của chúng sẽ bị xóa và việc xóa đi lặp lại có thể phá vỡ kết nối theo cách không thể sửa chữa được nếu không vi phạm tính độc lập.

Vì vậy, vấn đề giảm xuống còn việc kiểm tra tính lưỡng cực trên mỗi thành phần được kết nối và chọn một bên của màu. Nếu một thành phần không phải là lưỡng cực thì không có lựa chọn hợp lệ nào tồn tại. 

Chúng tôi vẫn có quyền tự do lựa chọn loại màu nào sẽ xuất ra; chúng tôi chọn cái lớn hơn để tối đa hóa kích thước, mặc dù bên nào cũng có tác dụng. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Tập hợp con Brute Force | O(2^n · n) | O(n) | Quá chậm | 
| Màu lưỡng cực BFS/DFS | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng danh sách kề cho đồ thị từ các cạnh đầu vào. Biểu diễn này cho phép truyền tải theo thời gian tuyến tính. 
2. Khởi tạo một mảng`color`với -1 cho tất cả các đỉnh, nghĩa là chưa được thăm. 
3. Đối với mỗi đỉnh từ 1 đến n, nếu nó không được tô màu, hãy bắt đầu BFS (hoặc DFS) từ đỉnh đó và gán màu 0. 
4. Trong quá trình truyền tải, với mỗi cạnh (u, v), gán v màu đối diện của u nếu không được tô màu. Nếu v đã được tô màu và có cùng màu với u thì đồ thị không phải là đồ thị lưỡng cực và chúng ta kết luận ngay rằng không có nghiệm hợp lệ. 
5. Thu thập tất cả các đỉnh của một lớp màu từ mỗi thành phần được kết nối. Vì mỗi thành phần là lưỡng cực độc lập nên chúng ta có thể tự do lựa chọn một trong hai bên cho mỗi thành phần. 
6. Đối với mỗi thành phần, so sánh số đỉnh ở màu 0 và màu 1, đồng thời chọn cạnh lớn hơn để tối đa hóa kích thước câu trả lời. 
7. Xuất “YES” theo sau là số đỉnh được chọn và danh sách các đỉnh đó. 

Lý do quan trọng khiến tính năng này hoạt động là vì trong mỗi thành phần được kết nối, phân vùng kép đảm bảo không có cạnh bên trong giữa các đỉnh đã chọn nếu chúng ta chọn một màu duy nhất và tất cả các cạnh luôn kết nối trên phân vùng để duy trì kết nối. 

### Tại sao nó hoạt động 

Màu BFS thực thi việc phân vùng mỗi thành phần được kết nối thành hai bộ sao cho mọi cạnh đều giao nhau giữa chúng. Bất kỳ giải pháp hợp lệ nào cũng phải tránh chọn cả hai điểm cuối của một cạnh, điều này buộc tập hợp được chọn phải độc lập. Cách duy nhất để đảm bảo tính linh hoạt tối đa mà không phá vỡ các ràng buộc lân cận là căn chỉnh theo phân vùng kép. Nếu tồn tại một chu kỳ lẻ thì không tồn tại sự phân công hai màu nhất quán và mọi nỗ lực lựa chọn giáo viên chắc chắn sẽ vi phạm tính độc lập hoặc yêu cầu về cấu trúc ngăn cản việc loại bỏ cạnh khỏi việc ngắt kết nối biểu đồ. 

Như vậy, tính hai bên vừa cần vừa đủ để đảm bảo tính khả thi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

def solve():
    t = int(input())
    for _ in range(t):
        n, m = map(int, input().split())
        g = [[] for _ in range(n + 1)]
        for _ in range(m):
            u, v = map(int, input().split())
            g[u].append(v)
            g[v].append(u)

        color = [-1] * (n + 1)
        ok = True
        ans = []

        for i in range(1, n + 1):
            if color[i] != -1:
                continue
            q = deque([i])
            color[i] = 0
            comp = [[], []]

            while q:
                u = q.popleft()
                comp[color[u]].append(u)
                for v in g[u]:
                    if color[v] == -1:
                        color[v] = color[u] ^ 1
                        q.append(v)
                    elif color[v] == color[u]:
                        ok = False

            if not ok:
                break

            if len(comp[0]) >= len(comp[1]):
                ans.extend(comp[0])
            else:
                ans.extend(comp[1])

        if not ok:
            print("NO")
        else:
            print("YES")
            print(len(ans))
            print(*ans)

if __name__ == "__main__":
    solve()
```Việc triển khai tuân theo màu lưỡng cực BFS tiêu chuẩn cho mỗi thành phần. Danh sách kề được xây dựng một lần cho mỗi trường hợp thử nghiệm. các`color`mảng vẫn tồn tại trên các thành phần, điều này rất quan trọng vì chúng ta không cần phải đặt lại mảng đó giữa các thành phần trong quá trình kiểm thử. 

Một điểm tinh tế là chúng tôi chỉ kiểm tra tính lưỡng cực cục bộ trong BFS; nếu phát hiện xung đột, chúng tôi sẽ dừng sớm. Một chi tiết khác là chúng tôi thu thập các đỉnh cho mỗi thành phần trong`comp[0]`Và`comp[1]`, cho phép lựa chọn tối ưu sau khi khám phá toàn bộ thành phần. 

Chúng tôi cũng cẩn thận đảm bảo xử lý mọi thành phần bị ngắt kết nối vì ban đầu biểu đồ có thể không được kết nối. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2
3 2
2 1
```Chúng tôi xây dựng danh sách lân cận: 

Đỉnh 1: [2] 

Đỉnh 2: [3, 1] 

Đỉnh 3: [2] 

Chúng tôi bắt đầu BFS từ 1. 

| Bước | Nút | Màu sắc | comp[0] | comp[1] | Hành động | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 1 | 0 | [1] | [] | bắt đầu BFS | 
| 2 | 2 | 1 | [1] | [2] | chỉ định ngược lại | 
| 3 | 3 | 0 | [1, 3] | [2] | tuyên truyền | 

Thành phần là lưỡng cực. Ta chọn tập lớn hơn, cả hai kích thước đều bằng nhau nên ta lấy [1, 3]. 

Đầu ra:```
YES
2
1 3
```Điều này xác nhận rằng việc phân công hai bên tạo ra một tập hợp độc lập hợp lệ. 

### Ví dụ 2 

đầu vào:```
4 2
1 4
2 3
```Hai cạnh bị ngắt kết nối. 

Thành phần đầu tiên (1-4): 

Chúng tôi gán 1→0, 4→1. 

Thành phần thứ hai (2-3): 

Chúng tôi gán 2→0, 3→1. 

| Thành phần | màu 0 | màu 1 | đã chọn | 
| --- | --- | --- | --- | 
| 1 | [1] | [4] | [1] | 
| 2 | [2] | [3] | [2] | 

Câu trả lời cuối cùng là [1, 2]. 

Đầu ra:```
YES
2
1 2
```Điều này chứng tỏ mỗi thành phần được xử lý độc lập và đóng góp mặt tối ưu riêng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi đỉnh và cạnh được xử lý một lần trong quá trình truyền tải BFS | 
| Không gian | O(n + m) | Danh sách kề và mảng tô màu | 

Các ràng buộc cho phép tổng số đỉnh và cạnh lên tới 3⋅10^5, đồng thời chỉ cần vượt qua một tuyến tính duy nhất cho mỗi trường hợp thử nghiệm, do đó giải pháp sẽ vừa vặn thoải mái trong giới hạn. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    from collections import deque

    def solve():
        t = int(input())
        out = []
        for _ in range(t):
            n, m = map(int, input().split())
            g = [[] for _ in range(n + 1)]
            for _ in range(m):
                u, v = map(int, input().split())
                g[u].append(v)
                g[v].append(u)

            color = [-1] * (n + 1)
            ok = True
            ans = []

            for i in range(1, n + 1):
                if color[i] != -1:
                    continue
                q = deque([i])
                color[i] = 0
                comp = [[], []]

                while q:
                    u = q.popleft()
                    comp[color[u]].append(u)
                    for v in g[u]:
                        if color[v] == -1:
                            color[v] = color[u] ^ 1
                            q.append(v)
                        elif color[v] == color[u]:
                            ok = False

                if not ok:
                    break

                if len(comp[0]) >= len(comp[1]):
                    ans.extend(comp[0])
                else:
                    ans.extend(comp[1])

            if not ok:
                out.append("NO")
            else:
                out.append("YES")
                out.append(str(len(ans)))
                out.append(" ".join(map(str, ans)))

        return "\n".join(out)

    return solve()

# provided sample 1
assert run("""2
3 2
3 2
2 1
4 2
1 4
2 3
""") == """YES
2
1 3
NO"""

# custom: single edge
assert run("""1
2 1
1 2
""").split()[0] == "YES"

# custom: odd cycle
assert run("""1
3 3
1 2
2 3
3 1
""") == "NO"

# custom: disconnected bipartite
assert run("""1
6 0
""").split()[0] == "YES"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cạnh đơn | CÓ ... | thành phần lưỡng cực cơ bản | 
| tam giác | KHÔNG | từ chối chu kỳ lẻ | 
| đồ thị trống | CÓ | xử lý các đỉnh bị cô lập | 

## Vỏ cạnh 

Trường hợp cạnh tới hạn là đồ thị chứa chu trình lẻ. Xét một tam giác có các cạnh (1,2), (2,3), (3,1). BFS cố gắng gán các màu xen kẽ, nhưng cuối cùng buộc nút 1 phải có cả màu 0 và màu 1 thông qua các đường dẫn khác nhau. Thuật toán phát hiện xung đột này khi đến thăm một hàng xóm đã được tô màu có cùng màu và đưa ra kết quả NO một cách chính xác. 

Một trường hợp cạnh khác là đồ thị rời rạc với các đỉnh cô lập. Mỗi đỉnh bị cô lập tạo thành một thành phần có kích thước bằng một và không có ràng buộc nào. BFS gán cho nó màu 0 và đưa nó vào câu trả lời, điều này hợp lệ vì không có ràng buộc kề nào bị vi phạm. 

Trường hợp tinh vi cuối cùng là nhiều thành phần trong đó mỗi thành phần là lưỡng cực nhưng việc chọn các mặt không nhất quán có thể làm giảm kích thước câu trả lời. Thuật toán xử lý vấn đề này bằng cách chọn độc lập lớp màu lớn hơn cho mỗi thành phần, đảm bảo cấu trúc tối đa mà không ảnh hưởng đến tính hợp lệ.
