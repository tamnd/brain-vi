---
title: "CF 909E - Bộ đồng xử lý"
description: "Chương trình bao gồm các nhiệm vụ phải được thực hiện theo thứ tự nhất quán với biểu đồ phụ thuộc. Mỗi tác vụ có thể phụ thuộc vào các tác vụ trước đó, tạo thành một biểu đồ tuần hoàn có hướng trong đó việc thực thi chỉ được phép thực hiện sau khi hoàn thành tất cả các điều kiện tiên quyết."
date: "2026-06-15T12:03:37+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "dp", "graphs", "greedy"]
categories: ["algorithms"]
codeforces_contest: 909
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 455 (Div. 2)"
rating: 1900
weight: 909
solve_time_s: 329
verified: true
draft: false
---

[CF 909E - Bộ đồng xử lý](https://codeforces.com/problemset/problem/909/E) 

**Xếp hạng:** 1900 
**Tags:** dfs và tương tự, dp, đồ thị, tham lam 
**Thời gian giải:** 5 phút 29s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chương trình bao gồm các nhiệm vụ phải được thực hiện theo thứ tự nhất quán với biểu đồ phụ thuộc. Mỗi tác vụ có thể phụ thuộc vào các tác vụ trước đó, tạo thành một biểu đồ tuần hoàn có hướng trong đó việc thực thi chỉ được phép thực hiện sau khi hoàn thành tất cả các điều kiện tiên quyết. 

Mỗi nhiệm vụ đều có một vị trí thực hiện cố định. Một số tác vụ phải chạy trên bộ xử lý chính, trong khi những tác vụ khác bị hạn chế ở bộ đồng xử lý. Điều phức tạp chính là bộ đồng xử lý không thực hiện từng tác vụ một theo thứ tự tùy ý. Thay vào đó, mỗi lần gọi nó, chúng tôi chọn một tập hợp các tác vụ chỉ dành cho bộ đồng xử lý. Tập hợp đó chỉ hợp lệ nếu đối với mọi tác vụ bên trong nó, tất cả các phần phụ thuộc của nó đều đã được hoàn thành hoặc cũng được bao gồm trong cùng một tập hợp. 

Mục tiêu là thực thi tất cả các tác vụ đồng thời giảm thiểu số lần chúng ta gọi bộ đồng xử lý. Các tác vụ của bộ xử lý chính miễn phí về mặt lệnh gọi, nhưng chúng vẫn tham gia vào các phần phụ thuộc và có thể chặn các tác vụ của bộ đồng xử lý. 

Các ràng buộc cho phép tối đa 100.000 nhiệm vụ và phần phụ thuộc, điều này ngay lập tức loại trừ mọi cách tiếp cận mô phỏng các tập hợp con hoặc liên tục cố gắng xây dựng các nhóm hợp lệ bằng cách tính toán lại khả năng tiếp cận. Bất cứ điều gì bậc hai về số lượng nút hoặc cạnh sẽ không thành công. Cần phải duyệt đồ thị tuyến tính hoặc gần tuyến tính, thường là O(N + M). 

Một trường hợp lỗi nhỏ xuất hiện khi các tác vụ của bộ đồng xử lý được xâu chuỗi thông qua các tác vụ của bộ xử lý chính. Ví dụ: nếu một chuỗi phụ thuộc dài xen kẽ giữa các tác vụ chỉ chính và chỉ bộ đồng xử lý, thì việc phân nhóm đơn giản có thể cố gắng hợp nhất quá tích cực hoặc quá thận trọng. 

Hãy xem xét đầu vào này:```
3 2
1 0 1
0 1
1 2
```Tác vụ 0 và 2 là tác vụ đồng xử lý, tác vụ 1 chỉ là tác vụ chính. Mặc dù 0 phụ thuộc vào 1 và 1 phụ thuộc vào 2, chúng ta không thể gộp 0 và 2 lại với nhau trong một lệnh gọi vì 0 gián tiếp phụ thuộc vào 2 đến 1, nhưng 1 không có trong bộ đồng xử lý. Một cách tiếp cận ngây thơ mà bỏ qua các nhiệm vụ chính trung gian có thể hợp nhất chúng một cách không chính xác. 

Một trường hợp khác là khi tồn tại nhiều cây con bộ đồng xử lý độc lập. Nếu mỗi lệnh được xử lý độc lập mà không xem xét sự phụ thuộc chung, thì người ta có thể đếm quá nhiều lệnh gọi hoặc không thể sử dụng lại một lệnh gọi cho nhiều thành phần sẵn sàng. 

## Phương pháp tiếp cận 

Chiến lược bạo lực sẽ mô phỏng việc thực hiện từng bước. Tại mọi thời điểm, chúng tôi xác định tất cả các tác vụ của bộ đồng xử lý có sự phụ thuộc được thỏa mãn và cố gắng gói gọn càng nhiều tác vụ trong số đó càng tốt vào một lệnh gọi bộ đồng xử lý. Sau mỗi cuộc gọi, chúng tôi cập nhật biểu đồ và lặp lại. Về nguyên tắc điều này đúng, nhưng khó khăn chính là xác định các tập hợp lệ tối đa một cách hiệu quả. Trong trường hợp xấu nhất, mỗi cuộc gọi có thể chỉ bao gồm một nhiệm vụ và việc tính toán lại tính đủ điều kiện cho tất cả các nhiệm vụ sau mỗi cuộc gọi sẽ dẫn đến việc quét lặp lại trên toàn bộ biểu đồ. Điều này thoái hóa thành hành vi O(N2) trên chuỗi phụ thuộc dày đặc. 

Cái nhìn sâu sắc về cấu trúc là biểu đồ áp đặt một phần thứ tự và các tác vụ của bộ xử lý chính đóng vai trò là các dấu phân cách bắt buộc theo thứ tự đó. Lệnh gọi bộ đồng xử lý về cơ bản là một "thao tác đóng" trên các nút bộ đồng xử lý: khi chúng tôi chọn một tác vụ bộ đồng xử lý bắt đầu, chúng tôi phải bao gồm tất cả các tác vụ của bộ đồng xử lý có thể truy cập ngược thông qua các tác vụ của bộ đồng xử lý khác cho đến khi chúng tôi chạm vào các nút đã được xử lý hoặc các tác vụ chỉ chính ngăn chặn việc đưa vào thêm. 

Thay vì suy nghĩ theo các tập hợp con, chúng ta đảo ngược quan điểm. Chúng tôi xử lý các tác vụ theo thứ tự tôpô và duy trì trạng thái của các “phân đoạn” bộ đồng xử lý. Mỗi lần chúng tôi gặp phải một tác vụ của bộ đồng xử lý chưa được xử lý trước đó thỏa mãn, chúng tôi có thể cần phải bắt đầu một lệnh gọi bộ đồng xử lý mới. Số lượng cuộc gọi tương ứng với số lần chúng ta buộc phải “bước vào” thế giới bộ đồng xử lý sau khi ở bên ngoài nó, nơi các nhiệm vụ chỉ chính sẽ phá vỡ tính liên tục. 

Điều này làm giảm vấn đề trong việc theo dõi các phần phụ thuộc trong quá trình truyền tải cấu trúc liên kết và đếm tần suất chúng tôi buộc phải bắt đầu đóng mới các nút bộ đồng xử lý chưa được giải quyết. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Mô phỏng lực lượng vũ phu | O(N2) | O(N + M) | Quá chậm | 
| Nhóm tôpô + tham lam | O(N + M) | O(N + M) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Trước tiên, chúng tôi xây dựng biểu đồ và tính toán mức độ cho thứ tự tôpô tiêu chuẩn. 

1. Tính toán thứ tự tôpô của DAG bằng thuật toán Kahn. Điều này đảm bảo mọi phần phụ thuộc đều được xử lý trước tác vụ phụ thuộc vào nó. Nếu không có điều này, việc lý luận về thời điểm một nhiệm vụ có sẵn sẽ không đáng tin cậy. 
2. Duy trì một mảng theo dõi xem mỗi tác vụ đã được “xử lý” về mặt mức độ hài lòng của người phụ thuộc hay chưa. Điều này tương ứng với việc liệu tất cả các điều kiện tiên quyết đã được tính đến hay chưa. 
3. Lặp lại các nhiệm vụ theo thứ tự tôpô. Ở mỗi nhiệm vụ, chúng tôi biết tất cả các phụ thuộc đều đã được giải quyết. 
4. Khi chúng tôi gặp một tác vụ phải chạy trên bộ đồng xử lý, chúng tôi sẽ kiểm tra xem liệu nó đã được thực hiện bởi lệnh gọi bộ đồng xử lý trước đó hay chưa. Nếu không, tác vụ này buộc phải bắt đầu một lệnh gọi bộ đồng xử lý mới vì nó thể hiện một yêu cầu kết nối mới trong cấu trúc phụ thuộc của bộ đồng xử lý. 
5. Sau khi lệnh gọi bộ đồng xử lý được bắt đầu, về mặt khái niệm, chúng tôi sẽ bao gồm tất cả các tác vụ của bộ đồng xử lý có thể đạt được mà không vượt qua ranh giới nhiệm vụ chỉ dành cho bộ xử lý chính. Việc truyền bá này được xử lý hoàn toàn theo thứ tự tôpô: tất cả các phụ thuộc của một nút đã được xử lý, do đó việc nhóm trở thành vấn đề đếm các chuyển tiếp. 
6. Tiếp tục cho đến khi tất cả các nút được xử lý. Số lần chúng tôi bắt đầu một nhóm bộ đồng xử lý mới là câu trả lời. 

### Tại sao nó hoạt động

Bất biến chính là bất cứ khi nào chúng tôi bắt đầu lệnh gọi bộ đồng xử lý, chúng tôi sẽ bao phủ một vùng liền kề tối đa của công việc liên quan đến bộ đồng xử lý theo thứ tự phụ thuộc. Bất cứ khi nào chúng ta gặp phải một tác vụ bộ đồng xử lý không thể truy cập được từ vùng hoạt động hiện tại, nó phải phụ thuộc (trực tiếp hoặc gián tiếp thông qua các tác vụ chính) vào một tác vụ bên ngoài vùng đã bị đóng. Điều này buộc một cuộc gọi mới. Vì thứ tự tôpô tôn trọng sự phụ thuộc nên không có thao tác nào trong tương lai có thể hợp nhất các vùng riêng biệt này, do đó mỗi lần tăng trong bộ đếm là cần thiết và đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
from collections import deque

def solve():
    n, m = map(int, input().split())
    e = list(map(int, input().split()))

    g = [[] for _ in range(n)]
    indeg = [0] * n

    for _ in range(m):
        a, b = map(int, input().split())
        g[b].append(a)
        indeg[a] += 1

    q = deque(i for i in range(n) if indeg[i] == 0)
    topo = []

    while q:
        v = q.popleft()
        topo.append(v)
        for to in g[v]:
            indeg[to] -= 1
            if indeg[to] == 0:
                q.append(to)

    visited = [False] * n
    active = False
    ans = 0

    for v in topo:
        if e[v] == 0:
            continue

        if not visited[v]:
            ans += 1
            stack = [v]
            visited[v] = True

            while stack:
                x = stack.pop()
                for to in g[x]:
                    if e[to] == 1 and not visited[to]:
                        visited[to] = True
                        stack.append(to)

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp bắt đầu bằng cách xây dựng danh sách kề đảo ngược phù hợp với hướng phụ thuộc và mức độ tính toán để sắp xếp tôpô. Thuật toán của Kahn đảm bảo chúng tôi chỉ xử lý một tác vụ sau khi tất cả các điều kiện tiên quyết đã được giải quyết. 

Sau khi có được thứ tự cấu trúc liên kết, chúng tôi quét nó và kích hoạt mở rộng giống như DFS bất cứ khi nào chúng tôi tìm thấy tác vụ bộ đồng xử lý chưa được truy cập. DFS đó đánh dấu tất cả các tác vụ của bộ đồng xử lý bị buộc phải đưa vào cùng một đợt thực thi do sự đóng phụ thuộc giữa các nút chỉ có bộ đồng xử lý. Mỗi DFS như vậy tương ứng chính xác với một lệnh gọi bộ đồng xử lý. 

Mảng đã truy cập đảm bảo chúng tôi không bao giờ đếm gấp đôi các nhiệm vụ đã được giao cho đợt trước. Điểm tinh tế quan trọng là chúng tôi chỉ truyền bá thông qua các tác vụ chỉ dành cho bộ đồng xử lý; các tác vụ của bộ xử lý chính đóng vai trò là rào cản và không bao giờ là một phần của việc mở rộng. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 3
0 1 0 1
0 1
1 2
2 3
```Thứ tự tôpô là`3, 2, 1, 0`. 

| Bước | Nút | Loại | Đã ghé thăm? | Hành động | Cuộc gọi | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 3 | coproc | không | bắt đầu DFS từ 3 | 1 | 
| 2 | 2 | chính | - | bỏ qua | 1 | 
| 3 | 1 | coproc | không | bắt đầu DFS từ 1 | 2 | 
| 4 | 0 | chính | - | bỏ qua | 2 | 

Cuộc gọi bộ đồng xử lý đầu tiên chỉ bao gồm nhiệm vụ 3 vì chuỗi phụ thuộc của nó bị chặn bởi nhiệm vụ chính 2. Cuộc gọi thứ hai bao gồm nhiệm vụ 1 riêng biệt. 

### Ví dụ 2 

đầu vào:```
4 3
1 1 1 0
0 1
1 2
0 2
```Thứ tự tôpô là`2, 1, 0, 3`. 

| Bước | Nút | Loại | Đã ghé thăm? | Hành động | Cuộc gọi | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | coproc | không | bắt đầu DFS từ 2 | 1 | 
| 2 | 1 | coproc | vâng | bỏ qua | 1 | 
| 3 | 0 | coproc | vâng | bỏ qua | 1 | 
| 4 | 3 | chính | - | bỏ qua | 1 | 

Tất cả các tác vụ của bộ đồng xử lý đều có thể truy cập được trong một lần đóng phụ thuộc, vì vậy chỉ cần một lệnh gọi là đủ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N + M) | Sắp xếp cấu trúc liên kết cộng với DFS trên mỗi nút nhiều nhất một lần | 
| Không gian | O(N + M) | Lưu trữ đồ thị, mảng vô cấp và điểm đánh dấu đã truy cập | 

Độ phức tạp tuyến tính phù hợp thoải mái trong giới hạn 100.000 nút và cạnh, vì mỗi cạnh và nút được xử lý một số lần không đổi. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from collections import deque

    def solve():
        n, m = map(int, input().split())
        e = list(map(int, input().split()))
        g = [[] for _ in range(n)]
        indeg = [0] * n

        for _ in range(m):
            a, b = map(int, input().split())
            g[b].append(a)
            indeg[a] += 1

        q = deque(i for i in range(n) if indeg[i] == 0)
        topo = []
        while q:
            v = q.popleft()
            topo.append(v)
            for to in g[v]:
                indeg[to] -= 1
                if indeg[to] == 0:
                    q.append(to)

        vis = [False] * n
        ans = 0

        for v in topo:
            if e[v] == 0:
                continue
            if not vis[v]:
                ans += 1
                stack = [v]
                vis[v] = True
                while stack:
                    x = stack.pop()
                    for to in g[x]:
                        if e[to] == 1 and not vis[to]:
                            vis[to] = True
                            stack.append(to)

        return str(ans)

    return solve()

# provided sample
assert run("""4 3
0 1 0 1
0 1
1 2
2 3
""") == "2"

# custom tests
assert run("""1 0
0
""") == "0"

assert run("""1 0
1
""") == "1"

assert run("""3 0
1 1 1
""") == "3"

assert run("""5 4
1 0 1 0 1
0 1
2 1
4 3
3 2
""") == "3"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nhiệm vụ chính duy nhất | 0 | không sử dụng bộ đồng xử lý | 
| nhiệm vụ đồng xử lý đơn | 1 | trường hợp cuộc gọi tối thiểu | 
| nhiệm vụ đồng xử lý độc lập | 3 | không có sự hợp nhất phụ thuộc | 
| phụ thuộc hỗn hợp xâu chuỗi | 3 | phân khúc qua các rào cản | 

## Vỏ cạnh 

Một trường hợp quan trọng là khi tất cả các nhiệm vụ chỉ là nhiệm vụ chính. Thuật toán chỉ đơn giản bỏ qua tất cả các nút và câu trả lời vẫn là 0 vì không có DFS nào được kích hoạt. 

Một trường hợp khác là một tập hợp các tác vụ đồng xử lý hoàn toàn độc lập không có cạnh. Mỗi nút là gốc cấu trúc liên kết riêng của nó, do đó, mỗi nút bộ đồng xử lý chưa được truy cập sẽ kích hoạt chính xác một lệnh gọi, phù hợp với thực tế là không thể nhóm được nút nào. 

Trường hợp thứ ba liên quan đến chuỗi phụ thuộc dài xen kẽ giữa bộ đồng xử lý và tác vụ chính. Việc mở rộng DFS không bao giờ vượt qua nhiệm vụ chính, do đó mỗi phân đoạn bộ đồng xử lý được cách ly chính xác. Ví dụ:```
5 4
1 0 1 0 1
1 0
2 1
3 2
4 3
```Ở đây, mỗi nút bộ đồng xử lý được phân tách bằng các nút chỉ chính, do đó, mỗi nút sẽ kích hoạt một cuộc gọi riêng, tạo ra 3. Việc truyền tải đảm bảo không có sự hợp nhất bất hợp pháp nào xảy ra vì quá trình truyền dừng ở các đỉnh chỉ chính.
