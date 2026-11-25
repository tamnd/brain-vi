---
problem: 977E
contest_id: 977
problem_index: E
name: "Cyclic Components"
contest_name: "Codeforces Round 479 (Div. 3)"
rating: 1500
tags: ["dfs and similar", "dsu", "graphs"]
answer: passed_samples
verified: true
solve_time_s: 61
date: 2026-06-18
model: gpt-5-3-mini
samples_passed: 1
samples_total: 1
share_url: https://chatgpt.com/share/6a33a501-2120-83ec-ad40-57e2d61c49fa
---

#CF 977E - Linh kiện tuần hoàn 

**Đánh giá:** 1500 
**Tags:** dfs và tương tự, dsu, đồ thị 
**Mẫu:** gpt-5-3-mini 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có (1/1 mẫu) 
**Chia sẻ:** https://chatgpt.com/share/6a33a501-2120-83ec-ad40-57e2d61c49fa 

--- 

## Giải pháp 

## Hiểu vấn đề 

Chúng ta được cho một đồ thị vô hướng và nhiệm vụ là đếm xem có bao nhiêu thành phần liên thông của nó tạo thành một chu trình đơn giản. Một thành phần được coi là một chu trình khi tất cả các đỉnh của nó có thể được sắp xếp thành một vòng sao cho mỗi đỉnh có đúng hai đỉnh lân cận bên trong thành phần đó và không có cạnh phụ nào ngoài các cạnh tạo thành vòng đó. 

Theo thuật ngữ đồ thị, điều này có nghĩa là chúng ta đang tìm kiếm các thành phần liên thông trong đó mọi đỉnh đều có bậc chính xác là hai trong thành phần đó. Một thành phần có các nhánh phụ hoặc hợp âm bên trong sẽ bị loại, ngay cả khi nó chứa một chu trình ở đâu đó bên trong nó. Các chu trình riêng biệt như hình tam giác hoặc hình vuông đều hợp lệ, nhưng bất kỳ cấu trúc “chu trình cộng đuôi” nào đều không hợp lệ. 

Các ràng buộc cho phép lên tới 200.000 đỉnh và cạnh. Điều này ngay lập tức loại trừ bất kỳ điều gì cố gắng kiểm tra tất cả các tập hợp con hoặc tính toán lại kết nối nhiều lần. Bất kỳ giải pháp nào cũng phải chạy trong thời gian tuyến tính hoặc gần tuyến tính, do đó, việc duyệt đồ thị như DFS, BFS hoặc nhóm dựa trên DSU là cần thiết. 

Một vài trường hợp phức tạp xuất hiện một cách tự nhiên. 

Một thành phần được kết nối duy nhất có đúng một chu kỳ nhưng các cạnh phụ sẽ phá vỡ điều kiện. Ví dụ: một tam giác có thêm một nút lá được đính kèm sẽ được kết nối và chứa một chu trình nhưng không phải là thành phần chu trình. Đóng góp của câu trả lời đúng là bằng không. 

Một chu trình thuần túy phải có ít nhất ba đỉnh. Thành phần có kích thước hai với một cạnh duy nhất là không hợp lệ, mặc dù cả hai đỉnh đều có bậc một, vì nó không thể tạo thành một vòng khép kín. 

Cuối cùng, các đỉnh hoặc cây bị cô lập luôn không hợp lệ vì chúng không chứa chu trình trong đó mọi đỉnh đều có bậc hai. 

## Phương pháp tiếp cận 

Một ý tưởng mạnh mẽ là tính toán các thành phần được kết nối và đối với mỗi thành phần, xác minh rõ ràng xem nó có tạo thành một chu trình hay không. Chúng ta có thể làm điều này bằng cách thu thập tất cả các đỉnh trong một thành phần, sau đó kiểm tra hai điều kiện: mỗi đỉnh có đúng hai bậc trong thành phần đó và số cạnh bằng số đỉnh. Điều này có hiệu quả vì trong bất kỳ chu trình đơn giản nào, các cạnh và đỉnh khớp với nhau trong một vòng khép kín. 

Vấn đề với cách tiếp cận ngây thơ chỉ xuất hiện ở việc thực hiện kém hiệu quả chứ không phải ở tính đúng đắn. Nếu chúng ta quét liên tục các danh sách kề hoặc tính toán lại tư cách thành viên của thành phần không hiệu quả, chúng ta có thể hướng tới hành vi bậc hai. Tuy nhiên, ngay cả một DFS đơn giản tập hợp các thành phần một lần và kiểm tra mức độ vẫn tuyến tính. 

Quan sát quan trọng là một thành phần liên thông là một chu trình khi và chỉ khi mọi đỉnh trong nó có đúng bằng hai bậc trong thành phần đó. Điều này loại bỏ nhu cầu đếm cạnh hoặc tái cấu trúc cấu trúc. Trong quá trình truyền tải, chúng ta có thể tính toán kích thước thành phần và tổng các mức độ bên trong hoặc chỉ cần xác minh điều kiện mức độ cho tất cả các nút trong thành phần. 

Chúng tôi sử dụng DFS hoặc BFS để khám phá từng thành phần một lần, đánh dấu các nút đã truy cập. Đối với mỗi thành phần, chúng tôi kiểm tra xem tất cả các đỉnh có thỏa mãn bậc bằng hai hay không. Nếu có, thành phần này là một chu trình. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (kiểm tra lại cấu trúc nhiều lần) | O(n²) | O(n) | Quá chậm | 
| DFS/BFS có xác nhận bằng cấp | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán

1. Xây dựng biểu diễn danh sách kề của đồ thị. Điều này cho phép lặp lại theo thời gian không đổi qua các lân cận của mỗi đỉnh trong quá trình truyền tải. 
2. Duy trì mảng đã truy cập được khởi tạo thành false cho tất cả các đỉnh. Điều này đảm bảo mỗi nút được xử lý chính xác một lần trên tất cả các thành phần. 
3. Lặp lại mọi đỉnh từ 1 đến n. Nếu một đỉnh chưa được thăm, nó sẽ trở thành điểm bắt đầu của một thành phần được kết nối mới. 
4. Chạy DFS hoặc BFS từ đỉnh này, đánh dấu tất cả các nút có thể truy cập là một phần của cùng một thành phần. Trong quá trình truyền tải, thu thập tất cả các đỉnh thuộc thành phần này vào danh sách. 
5. Sau khi kết thúc quá trình truyền tải một thành phần, hãy kiểm tra mọi đỉnh trong thành phần đó và xác minh rằng kích thước danh sách kề của nó bằng chính xác 2. Điều kiện này buộc mỗi đỉnh tham gia vào đúng hai cạnh bên trong thành phần, điều này cần thiết cho một chu trình. 
6. Nếu tất cả các đỉnh trong thành phần đều thỏa mãn điều kiện này và thành phần đó có ít nhất 3 đỉnh thì tăng dần câu trả lời. 
7. Tiếp tục cho đến khi tất cả các đỉnh đã được xử lý, sau đó xuất ra số đếm cuối cùng. 

### Tại sao nó hoạt động 

Trong bất kỳ thành phần liên thông nào, nếu mỗi đỉnh có đúng bậc hai thì cấu trúc không thể phân nhánh hoặc kết thúc. Bắt đầu từ bất kỳ đỉnh nào và các cạnh tiếp theo cuối cùng phải quay trở lại điểm bắt đầu mà không lặp lại, tạo thành một chu trình đơn giản. Ngược lại, bất kỳ đồ thị chu trình đơn giản nào cũng có mọi đỉnh bậc đúng bằng hai. Vì vậy, điều kiện bậc vừa cần vừa đủ để xác định các thành phần chu trình trong đồ thị vô hướng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**7)

n, m = map(int, input().split())
g = [[] for _ in range(n + 1)]

for _ in range(m):
    u, v = map(int, input().split())
    g[u].append(v)
    g[v].append(u)

visited = [False] * (n + 1)

def dfs(start):
    stack = [start]
    comp = []
    visited[start] = True

    while stack:
        u = stack.pop()
        comp.append(u)
        for v in g[u]:
            if not visited[v]:
                visited[v] = True
                stack.append(v)

    return comp

ans = 0

for i in range(1, n + 1):
    if not visited[i]:
        comp = dfs(i)

        ok = True
        if len(comp) < 3:
            ok = False
        else:
            for u in comp:
                if len(g[u]) != 2:
                    ok = False
                    break

        if ok:
            ans += 1

print(ans)
```Giải pháp này xây dựng danh sách lân cận và chạy DFS từ mỗi nút chưa được truy cập, đảm bảo mỗi thành phần được kết nối được trích xuất chính xác một lần. DFS thu thập tất cả các nút trong thành phần. 

Sau khi trích xuất một thành phần, đầu tiên mã sẽ kiểm tra kích thước của nó. Một chu trình phải chứa ít nhất ba đỉnh nên các thành phần nhỏ hơn sẽ bị loại bỏ ngay lập tức. Sau đó, nó xác nhận thuộc tính xác định của đồ thị chu trình: mỗi đỉnh phải có chính xác hai đỉnh lân cận trong đồ thị đầy đủ, điều này trong một thành phần liên thông đơn giản có nghĩa là nó nằm trên một chu trình. 

Một lỗi phổ biến là cố gắng đếm các cạnh bên trong thành phần và so sánh với các đỉnh mà không xử lý cẩn thận các bản sao hoặc trạng thái truyền tải. Việc sử dụng mức độ danh sách kề toàn cục sẽ tránh được việc tính toán lại và tránh các lỗi kế toán khó phát hiện. 

DFS được lặp đi lặp lại để tránh các vấn đề về độ sâu đệ quy với giới hạn trên là 200.000 nút. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 4
1 2
3 4
5 4
3 5
```Chúng tôi xử lý các thành phần từng bước. 

| Nút bắt đầu | Đã tìm thấy thành phần | Kích cỡ | Kết quả kiểm tra bằng cấp | 
| --- | --- | --- | --- | 
| 1 | [1, 2] | 2 | thất bại | 
| 3 | [3, 4, 5] | 3 | vượt qua | 
| 5 | đã ghé thăm | - | - | 

Thành phần đầu tiên là một cạnh đơn, không đạt yêu cầu về kích thước. Thành phần thứ hai tạo thành một hình tam giác trong đó mỗi nút có bậc 2 nên được tính. 

Câu trả lời cuối cùng là 1. 

### Ví dụ 2 

đầu vào:```
4 3
1 2
2 3
3 4
```Biểu đồ này là một đường dẫn đơn giản. 

| Nút bắt đầu | Đã tìm thấy thành phần | Kích cỡ | Kết quả kiểm tra bằng cấp | 
| --- | --- | --- | --- | 
| 1 | [1, 2, 3, 4] | 4 | thất bại | 

Mặc dù đồ thị được kết nối, các nút 1 và 4 có bậc 1 nên nó không phải là thành phần chu trình. 

Đầu ra là 0. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi đỉnh và cạnh được truy cập một lần trong quá trình kiểm tra DFS và kề | 
| Không gian | O(n + m) | Danh sách kề cộng với mảng đã truy cập và lưu trữ thành phần | 

Độ phức tạp tuyến tính phù hợp thoải mái trong giới hạn 200.000 đỉnh và cạnh. Cả bộ nhớ và thời gian chạy vẫn tỷ lệ thuận với kích thước đầu vào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n, m = map(int, input().split())
    g = [[] for _ in range(n + 1)]
    for _ in range(m):
        u, v = map(int, input().split())
        g[u].append(v)
        g[v].append(u)

    visited = [False] * (n + 1)

    def dfs(start):
        stack = [start]
        comp = []
        visited[start] = True
        while stack:
            u = stack.pop()
            comp.append(u)
            for v in g[u]:
                if not visited[v]:
                    visited[v] = True
                    stack.append(v)
        return comp

    ans = 0
    for i in range(1, n + 1):
        if not visited[i]:
            comp = dfs(i)
            if len(comp) >= 3 and all(len(g[u]) == 2 for u in comp):
                ans += 1
    return str(ans)

# provided sample
assert run("""5 4
1 2
3 4
5 4
3 5
""") == "1"

# single edge
assert run("""2 1
1 2
""") == "0"

# simple cycle
assert run("""3 3
1 2
2 3
3 1
""") == "1"

# chain
assert run("""5 4
1 2
2 3
3 4
4 5
""") == "0"

# two disjoint cycles
assert run("""6 6
1 2
2 3
3 1
4 5
5 6
6 4
""") == "2"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| cạnh đơn | 0 | thành phần không hợp lệ tối thiểu | 
| chu kỳ tam giác | 1 | chu kỳ hợp lệ nhỏ nhất | 
| đồ thị chuỗi | 0 | từ chối cấu trúc cây | 
| hai chu kỳ | 2 | đếm nhiều thành phần | 

## Vỏ cạnh 

Một thành phần cạnh đơn như`1 - 2`được xử lý chính xác vì kích thước của nó là 2, không đạt yêu cầu kích thước tối thiểu cho một chu kỳ. DFS thu thập cả hai nút và việc kiểm tra sẽ ngay lập tức từ chối nó. 

Một hình tam giác như`1 - 2 - 3 - 1`vượt qua vì tất cả các nút có kích thước danh sách lân cận là 2. DFS thu thập tất cả ba nút trong một thành phần và điều kiện giữ nguyên cho mỗi nút. 

Một đồ thị ngôi sao như`1 connected to all others`không thành công vì nút trung tâm có bậc lớn hơn 2, vi phạm điều kiện chu trình ngay cả khi đồ thị được kết nối.
