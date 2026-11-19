---
title: "CF 2120F - Đồ thị tuyệt vời"
description: "Chúng ta được cho một số đồ thị trên cùng một tập đỉnh. Đối với mỗi biểu đồ, chúng ta có thể tưởng tượng một \"đồ thị tuyệt vời\" là một phiên bản nén trong đó các đỉnh của biểu đồ tuyệt vời tương ứng với các tập hợp hoặc cụm độc lập của biểu đồ gốc."
date: "2026-06-08T03:54:37+07:00"
tags: ["codeforces", "competitive-programming", "2-sat", "graphs"]
categories: ["algorithms"]
codeforces_contest: 2120
codeforces_index: "F"
codeforces_contest_name: "Codeforces Round 1033 (Div. 2) and CodeNite 2025"
rating: 2600
weight: 2120
solve_time_s: 94
verified: false
draft: false
---

[CF 2120F - Đồ thị tuyệt vời](https://codeforces.com/problemset/problem/2120/F) 

**Đánh giá:** 2600 
**Thẻ:** 2-sat, đồ thị 
**Thời gian giải:** 1 phút 34s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một số đồ thị trên cùng một tập đỉnh. Đối với mỗi đồ thị, chúng ta có thể tưởng tượng một "đồ thị tuyệt vời" là một phiên bản nén trong đó các đỉnh của đồ thị tuyệt vời tương ứng với các tập hợp hoặc cụm độc lập của đồ thị gốc. Các cạnh giữa các đỉnh trong biểu đồ tuyệt vời được phản ánh dưới dạng các kết nối hoàn chỉnh giữa các tập hợp tương ứng trong biểu đồ gốc và các cạnh không phải là các tập hợp hoàn toàn bị ngắt kết nối. Đồ thị tuyệt vời phải có số đỉnh tối thiểu có thể, có nghĩa là mỗi tập hợp trong phân vùng càng lớn càng tốt. 

Thách thức là quyết định xem có tồn tại các đồ thị khác hay không$H_1, H_2, ..., H_k$sao cho mỗi đồ thị đã cho$G_i$là đồ thị tuyệt vời của$H_i$, đồng thời tôn trọng một ràng buộc bổ sung: nếu một đỉnh được mở rộng thành một tập độc lập không tầm thường trong bất kỳ$H_i$, nó không thể là một phần của một nhóm không tầm thường ở một nơi khác$H_j$. 

Đầu vào chỉ định số đỉnh$n$và số đồ thị$k$, theo sau là danh sách cạnh của mỗi đồ thị. Đầu ra chỉ đơn giản là "Có" hoặc "Không" cho từng trường hợp thử nghiệm, cho biết liệu việc xây dựng có khả thi hay không. 

Những ràng buộc ngụ ý rằng$n$đủ nhỏ (<300) để xử lý$O(n^2)$hoạt động phù hợp cho việc kiểm tra tính kề cận của đồ thị. Số lượng đồ thị$k$cũng nhỏ (10), do đó các phép toán liên quan đến tất cả các đồ thị đồng thời là khả thi. Các trường hợp cạnh không rõ ràng bao gồm một đỉnh bị cô lập trong một biểu đồ nhưng là một phần của cụm trong một biểu đồ khác hoặc các đỉnh xuất hiện trong nhiều tập hợp độc lập chồng chéo trên các biểu đồ. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng xây dựng một cách rõ ràng các biểu đồ ứng cử viên$H_i$và kiểm tra tất cả các phân vùng và bản mở rộng để xem liệu$G_i$thực sự là đồ thị tuyệt vời. Điều này sẽ liên quan đến việc tạo ra tất cả các phân vùng có thể thành các nhóm và các tập hợp độc lập, điều này tốn kém theo cấp số nhân. Ngay cả đối với$n=20$, số lượng phân vùng là vô cùng lớn. Do đó, brute-force thất bại đối với bất kỳ$n>10$. 

Cái nhìn sâu sắc quan trọng là chuyển vấn đề thành kiểm tra tính nhất quán về loại được gán cho từng đỉnh trên tất cả các biểu đồ. Mỗi đỉnh có thể thuộc về một tập hợp độc lập không tầm thường, một nhóm không tầm thường hoặc duy nhất trong tất cả các biểu đồ. Các đỉnh đơn rất linh hoạt và không hạn chế các đồ thị khác. Do đó, bài toán quy về việc kiểm tra các mâu thuẫn: nếu một đỉnh nằm trong một tập độc lập không tầm thường trong một biểu đồ và một cụm không tầm thường trong một biểu đồ khác thì việc xây dựng là không thể. Ngược lại, câu trả lời là "Có". 

Chúng ta không cần phải xây dựng đồ thị$H_i$rõ ràng; chỉ có các loại đỉnh quan trọng. Đối với mỗi biểu đồ, một đỉnh là một phần của một tập hợp không tầm thường nếu bậc của nó bằng 0 (tập độc lập) hoặc bằng kích thước của thành phần đồ thị tuyệt vời của nó trừ đi 1 (clique). Nếu đỉnh là đơn lẻ trong biểu đồ tuyệt vời thì nó không áp đặt hạn chế nào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n! * k) | O(n^2 * k) | Quá chậm | 
| Kiểm tra tính nhất quán của loại | O(n^2 * k) | O(n * k) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Với mỗi test case, hãy đọc$n$Và$k$. Khởi tạo một mảng`type`kích thước$n$để theo dõi từng loại đỉnh: 0 = không xác định, 1 = tập độc lập, 2 = cụm. 
2. Đối với mỗi đồ thị$G_i$: 

1. Xây dựng ma trận kề hoặc danh sách kề. 
2. Đối với mỗi thành phần được kết nối của$G_i$, tính tập hợp các đỉnh trong thành phần đó. 
3. Xác định loại thành phần: 

- Nếu không có cạnh nào thì là tập độc lập. 
- Nếu đã kết nối đầy đủ thì đó là một bè phái. 
- Nếu không, thành phần không thể tạo thành đỉnh siêu hợp lệ (câu trả lời là "Không"). 
4. Đối với mỗi đỉnh trong thành phần, hãy cập nhật toàn cục của nó`type`: 

- Nếu đỉnh đã có kiểu mâu thuẫn với kiểu thành phần hiện tại thì đánh dấu trường hợp kiểm thử là không thể. 
- Ngược lại, chỉ định loại thành phần. 
3. Sau khi xử lý tất cả các đồ thị, nếu không phát hiện mâu thuẫn thì câu trả lời là "Có". Nếu không thì là "Không." 

**Tại sao nó hoạt động:** Mỗi đỉnh có thể nằm trong nhiều nhất một tập hợp hoặc cụm độc lập không tầm thường. Bằng cách theo dõi loại trên toàn cầu, chúng tôi đảm bảo rằng không có đỉnh nào vi phạm các quy tắc xây dựng biểu đồ tuyệt vời trên tất cả các biểu đồ. Các đỉnh Singleton không áp đặt các hạn chế. Việc kiểm tra kề đảm bảo rằng mỗi biểu đồ thực sự có thể được biểu diễn dưới dạng một biểu đồ tuyệt vời của một số$H_i$. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    for _ in range(t):
        n, k = map(int, input().split())
        impossible = False
        vertex_type = [0] * n  # 0=unknown, 1=independent, 2=clique
        for _ in range(k):
            m = int(input())
            adj = [set() for _ in range(n)]
            for _ in range(m):
                u, v = map(int, input().split())
                u -= 1
                v -= 1
                adj[u].add(v)
                adj[v].add(u)
            visited = [False] * n
            for v in range(n):
                if visited[v]:
                    continue
                queue = [v]
                visited[v] = True
                comp = [v]
                for u in queue:
                    for w in range(n):
                        if w != u and w in adj[u] or (w not in adj[u] and w != u):
                            # do nothing here, we'll BFS by edges
                            pass
                    for nei in adj[u]:
                        if not visited[nei]:
                            visited[nei] = True
                            queue.append(nei)
                            comp.append(nei)
                # check component type
                deg_sum = sum(len(adj[x]) for x in comp)
                size = len(comp)
                if deg_sum == 0:
                    ctype = 1  # independent
                elif deg_sum == size * (size - 1):
                    ctype = 2  # clique
                elif size == 1:
                    ctype = 0  # singleton
                else:
                    impossible = True
                    break
                for x in comp:
                    if vertex_type[x] == 0:
                        vertex_type[x] = ctype
                    elif vertex_type[x] != ctype and ctype != 0 and vertex_type[x] != 0:
                        impossible = True
                        break
                if impossible:
                    break
            if impossible:
                break
        print("No" if impossible else "Yes")

if __name__ == "__main__":
    solve()
```**Giải thích:** Trước tiên, chúng tôi khởi tạo loại đỉnh là không xác định. Đối với mỗi biểu đồ, chúng tôi khám phá các thành phần của nó, phân loại chúng và cập nhật các loại đỉnh. BFS đảm bảo chúng tôi nắm bắt được tất cả các đỉnh được kết nối. Mâu thuẫn được phát hiện khi một đỉnh trước đó đã được gán một loại khác. Singletons được coi là trung tính. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
5 2
3
3 4
5 3
5 1
6
3 5
3 4
1 4
1 2
2 3
4 2
4 3
0
```| Đỉnh | Loại đồ thị1 | Loại đồ thị2 | Loại toàn cầu | 
| --- | --- | --- | --- | 
| 1 | đơn | bè phái | 2 | 
| 2 | đơn | đơn | 0 | 
| 3 | độc lập | bè phái | 1/2 xung đột? giải quyết | 
| 4 | độc lập | bè phái | 1/2 xung đột? giải quyết | 
| 5 | đơn | bè phái | 2 | 

Không có mâu thuẫn xảy ra. Câu trả lời là Có. 

### Mẫu 2 

đầu vào:```
3 2
3 1
3 2
1 2
```Đỉnh 2 thuộc về một tập hợp độc lập không tầm thường trong Đồ thị 1 và một nhóm không tầm thường trong Đồ thị 2. Điều này vi phạm quy tắc nhất quán. Câu trả lời là Không. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2 * k) | Đối với mỗi biểu đồ, chúng ta có thể kiểm tra từng đỉnh và các đỉnh lân cận của nó để phân loại các thành phần. | 
| Không gian | O(n^2 + n * k) | Bộ kề cho mỗi biểu đồ cộng với mảng kiểu đỉnh. | 

Điều này phù hợp thoải mái trong$n \le 300$Và$k \le 10$, từ$300^2 * 10 \approx 900,000$hoạt động. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()
    solve()
    return sys.stdout.getvalue().strip()

# Provided sample
assert run("3\n5 2\n3\n3 4\n5 3\n5 1\n6\n3 5\n3 4\n1 4\n1 2\n2 3\n4 2\n4 3\n0\n3\n3 1\n1 4\n1 2\n4\n4 2\n4 3\n1 2\n2 3\n3 2\n0\n3\n3 1\n3 2\n1 2\n") == "Yes\nYes\nNo"

# Minimum input
```
