---
title: "CF 102419D - Xor đồ thị"
description: "Chúng ta có một đồ thị vô hướng và mỗi đỉnh đều mang một số nguyên nhỏ hơn (2^{20}). Chúng ta có thể chọn một tập hợp con các đỉnh và một giá trị XOR (x), sau đó thay thế mọi giá trị đã chọn (ai) bằng (ai oplus x). Mục tiêu là làm cho hai giá trị điểm cuối khác nhau ở mọi cạnh."
date: "2026-08-14T14:48:21+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102419
codeforces_index: "D"
codeforces_contest_name: "SPC 2019"
rating: 0
weight: 102419
solve_time_s: 262
verified: false
draft: false
---

[CF 102419D - Xor biểu đồ](https://codeforces.com/problemset/problem/102419/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 4 phút 22s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đồ thị vô hướng và mỗi đỉnh đều mang một số nguyên nhỏ hơn (2^{20}). Chúng ta có thể chọn một tập hợp con các đỉnh và một giá trị XOR (x), sau đó thay thế mọi giá trị đã chọn (a_i) bằng (a_i \oplus x). Mục tiêu là làm cho hai giá trị điểm cuối khác nhau ở mọi cạnh. 

Cách hữu ích để xem xét một cạnh là phân loại các điểm cuối của nó theo việc chúng có được chọn hay không. Nếu cả hai điểm cuối đều được chọn hoặc cả hai đều không được chọn, XOR sẽ được áp dụng như nhau cho cả hai giá trị, do đó mối quan hệ đẳng thức của chúng không thay đổi. Nếu chính xác một điểm cuối được chọn thì hai giá trị cuối cùng sẽ bằng nhau một cách chính xác khi 

[ 
x=a_u\oplus a_v. 
] 

Điều này đưa ra cấu trúc trung tâm của vấn đề. 

Đối với cạnh có điểm cuối ban đầu có giá trị bằng nhau, (a_u=a_v), cả hai điểm cuối không thể có cùng trạng thái lựa chọn. Một cạnh như vậy buộc hai đỉnh thuộc về các cạnh đối diện của tập hợp con đã chọn. Do đó, tất cả các cạnh kết nối các giá trị bằng nhau tạo thành một đồ thị phải là đồ thị lưỡng cực. 

Đối với một cạnh có giá trị điểm cuối khác nhau, không có hạn chế nào về việc cả hai điểm cuối được chọn hay cả hai đều không được chọn. Nếu chọn chính xác một điểm cuối, chúng ta chỉ phải tránh giá trị XOR duy nhất (a_u\oplus a_v). 

Các giới hạn này làm cho việc tìm kiếm bậc hai hoặc hàm mũ là không thể. Với tối đa (3\cdot10^5) đỉnh và cạnh và chỉ một giây, giải pháp mong muốn về cơ bản cần xử lý đồ thị tuyến tính. Phạm vi giá trị chứa (2^{20}=1,048,576) giá trị XOR có thể có, đủ lớn so với giá trị tối đa (m=3\cdot10^5) để đảm bảo rằng có thể tìm thấy một giá trị phù hợp bằng một lần quét ngắn. 

Có một số trường hợp đặc biệt có thể đánh lừa việc triển khai trực tiếp. Coi như```
3 3
1 1 1
1 2
2 3
1 3
```Mọi cạnh đều nối các giá trị bằng nhau, vì vậy mọi cạnh đều yêu cầu điểm cuối của nó nằm ở phía đối diện. Đây là một chu kỳ lẻ và không thể phân chia thành hai phần. Đầu ra đúng là`-1`. Một giải pháp bất cẩn chỉ kiểm tra các cạnh riêng lẻ có thể bỏ sót mâu thuẫn tổng thể. 

Một trường hợp khác là một biểu đồ có các giá trị ban đầu đã đúng:```
3 2
1 2 3
1 2
2 3
```Không có cạnh có giá trị bằng nhau nên không có ràng buộc lưỡng cực nào cả. Chúng ta có thể chọn một đỉnh và chọn một (x) không bằng hiệu XOR trên bất kỳ cạnh nào được chọn. Một giải pháp không được cho rằng tồn tại một số cạnh có giá trị bằng nhau. 

Nhiều cạnh cũng xứng đáng được chăm sóc. Ví dụ,```
2 3
7 7
1 2
1 2
1 2
```cả ba cạnh đều áp đặt chính xác cùng một điều kiện, (x\ne0) và đồ thị vẫn là đồ thị lưỡng cực. Việc xử lý đầu vào dưới dạng một biểu đồ đơn giản là không cần thiết và có thể gây ra lỗi, nhưng bản thân các ràng buộc trùng lặp không gây khó khăn gì. 

Cuối cùng, (x) đã chọn phải ở dưới (2^{20}). Chúng tôi sẽ chỉ kiểm tra các giá trị từ (1) đến (m+1) và (m+1\le300001<2^{20}), do đó ranh giới sẽ tự động được tôn trọng. 

## Phương pháp tiếp cận 

Lực lượng vũ phu trực tiếp nhất là thử mọi tập hợp con các đỉnh và mọi giá trị XOR có thể có. Có (2^n) tập hợp con và (2^{20}) giá trị có thể có của (x). Đối với mỗi cặp, việc kiểm tra tất cả các cạnh sẽ mất (O(m)), đưa ra 

[ 
O(2^n\cdot2^{20}\cdot m) 
] 

kiểm tra cạnh trong trường hợp xấu nhất. Tại (n=3\cdot10^5), điều này vượt xa mọi tính toán khả thi. 

Về cơ bản, chúng ta có thể cải thiện sức mạnh vũ phu bằng cách quan sát trước tiên rằng các cạnh có giá trị bằng nhau sẽ xác định liệu một tập hợp con có khả thi hay không. Đối với cạnh có giá trị bằng nhau, phải chọn chính xác một điểm cuối. Đó chính xác là một điều kiện tô màu lưỡng cực. Thay vì liệt kê (2^n) tập hợp con, chúng ta có thể xây dựng một tập hợp con hợp lệ với một BFS hoặc DFS duy nhất. 

Câu hỏi còn lại là chọn (x) như thế nào. Khi tập hợp con được cố định, chỉ các cạnh đi từ đỉnh được chọn đến đỉnh không được chọn mới ràng buộc (x). Mỗi cạnh như vậy cấm chính xác một giá trị, (a_u\oplus a_v). Có nhiều nhất (m) giá trị bị cấm, trong khi chúng ta có thể chọn trong số (m+1) số nguyên dương. Theo nguyên lý chuồng bồ câu, ít nhất một trong số 

[ 
1,2,\ldots,m+1 
] 

không bị cấm. Vì (m+1<2^{20}), giá trị đó luôn hợp pháp. 

Lực lượng vũ phu hoạt động vì nó trực tiếp kiểm tra mọi hoạt động có thể. Nó thất bại vì số lượng tập hợp con có thể có là theo cấp số nhân. Quan sát cho thấy chỉ riêng các cạnh có giá trị bằng nhau đã xác định được phép tô màu hai màu cần thiết cho phép chúng ta thay thế tìm kiếm hàm mũ bằng một lần duyệt đồ thị, sau đó tìm kiếm XOR khổng lồ sẽ thu gọn thành nhiều nhất (m+1) ứng cử viên. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(2^n\cdot2^{20}\cdot m)) | (O(n+m)) | Quá chậm | 
| Tối ưu | (O(n+m)) thời gian thực tế dự kiến ​​| (O(n+m)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng một biểu đồ riêng chỉ chứa các cạnh ((u,v)) mà (a_u=a_v). Các cạnh này là những cạnh yêu cầu điểm cuối của chúng có các trạng thái lựa chọn khác nhau. 
2. Phân chia biểu đồ có giá trị bằng nhau này bằng BFS. Gán mỗi đỉnh một trong hai màu (0) hoặc (1). Đối với mỗi cạnh có giá trị bằng nhau, điểm cuối của nó phải nhận được các màu khác nhau. Nếu chúng ta tìm thấy một cạnh mà điểm cuối của nó đã có cùng màu thì tập hợp con được yêu cầu không thể tồn tại, vì vậy chúng ta sẽ in`-1`. 
3. Chọn mọi đỉnh có màu (1) làm tập hợp đã chọn. Nếu đồ thị có giá trị bằng nhau có ít nhất một cạnh thì tập hợp này sẽ tự động rỗng. Nếu đồ thị có giá trị bằng nhau không có cạnh, hãy chọn đỉnh (1) theo cách thủ công. Điều này xử lý trường hợp tất cả các giá trị cạnh ban đầu đã khác nhau. 
4. Kiểm tra từng cạnh gốc. Chỉ một cạnh có điểm cuối có trạng thái lựa chọn khác nhau mới có thể thay đổi quan hệ đẳng thức của nó. Với mỗi cạnh như vậy, hãy tính (a_u\oplus a_v) và đánh dấu giá trị đó là bị cấm đối với (x). 
5. Tìm kiếm qua (x=1,2,\ldots,m+1) và lấy giá trị đầu tiên không bị cấm. Có nhiều nhất (m) giá trị bị cấm, vì vậy (m+1) ứng viên không thể bị cấm. Ngoài ra, (m+1\le300001<2^{20}), do đó giá trị được chọn thỏa mãn phạm vi được yêu cầu. 
6. Xuất ra các đỉnh đã chọn và (x). Mọi cạnh có giá trị bằng nhau đều đi qua phân vùng, do đó các điểm cuối của nó nhận được các giá trị cuối cùng khác nhau. Mọi cạnh có giá trị không bằng nhau đi qua tập hợp con sẽ tránh giá trị XOR bị cấm duy nhất của nó, trong khi cạnh có điểm cuối ở cùng một phía sẽ giữ nguyên bất đẳng thức ban đầu. 

Điều bất biến là sau khi tô màu hai bên, mọi cạnh có giá trị bằng nhau đều có điểm cuối với trạng thái lựa chọn ngược lại. Một cạnh như vậy ban đầu có các giá trị bằng nhau và bởi vì (x\ne0), việc áp dụng XOR cho chính xác một điểm cuối sẽ làm cho hai giá trị đó khác nhau. Đối với cạnh có giá trị không bằng nhau có các trạng thái trái ngược nhau, các giá trị cuối cùng chỉ có thể trở thành bằng nhau đối với một giá trị duy nhất (x=a_u\oplus a_v), mà cấu trúc loại trừ một cách rõ ràng. Cạnh có giá trị không bằng nhau có trạng thái lựa chọn bằng nhau được áp dụng XOR cho cả hai điểm cuối hoặc không áp dụng cho cả hai điểm cuối, do đó hai giá trị khác nhau của nó vẫn khác nhau. 

Vì vậy mọi cạnh đều hợp lệ cho phép toán cuối cùng. Nếu việc tô màu hai bên không thành công, một chu kỳ lẻ có giá trị bằng nhau sẽ tạo ra một mẫu lựa chọn xen kẽ không thể thực hiện được, do đó việc báo cáo`-1`cũng đúng. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))

    edges = []
    equal_adj = [[] for _ in range(n)]

    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        edges.append((u, v))

        if a[u] == a[v]:
            equal_adj[u].append(v)
            equal_adj[v].append(u)

    color = [-1] * n

    for start in range(n):
        if color[start] != -1:
            continue

        color[start] = 0
        stack = [start]

        while stack:
            u = stack.pop()

            for v in equal_adj[u]:
                if color[v] == -1:
                    color[v] = color[u] ^ 1
                    stack.append(v)
                elif color[v] == color[u]:
                    print(-1)
                    return

    selected = [i for i in range(n) if color[i] == 1]

    if not selected:
        selected = [0]

    forbidden = set()

    for u, v in edges:
        if (u in selected_set) != (v in selected_set):
            forbidden.add(a[u] ^ a[v])

    x = 1
    while x in forbidden:
        x += 1

    print(len(selected), x)
    print(*(v + 1 for v in selected))

if __name__ == "__main__":
    solve()
```Đoạn mã trên cần một chi tiết triển khai nhỏ để giúp việc kiểm tra tư cách thành viên trở nên hiệu quả. Xây dựng`selected_set`từ danh sách trước khi quét các cạnh sẽ tránh việc tìm kiếm liên tục trong danh sách Python. Việc thực hiện đầy đủ là:```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))

    edges = []
    equal_adj = [[] for _ in range(n)]

    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        edges.append((u, v))

        if a[u] == a[v]:
            equal_adj[u].append(v)
            equal_adj[v].append(u)

    color = [-1] * n

    for start in range(n):
        if color[start] != -1:
            continue

        color[start] = 0
        stack = [start]

        while stack:
            u = stack.pop()

            for v in equal_adj[u]:
                if color[v] == -1:
                    color[v] = color[u] ^ 1
                    stack.append(v)
                elif color[v] == color[u]:
                    print(-1)
                    return

    selected = [i for i in range(n) if color[i] == 1]

    if not selected:
        selected = [0]

    selected_set = set(selected)

    forbidden = set()

    for u, v in edges:
        if (u in selected_set) != (v in selected_set):
            forbidden.add(a[u] ^ a[v])

    x = 1
    while x in forbidden:
        x += 1

    print(len(selected), x)
    print(*(v + 1 for v in selected))

if __name__ == "__main__":
    solve()
```Bước đầu tiên lưu trữ mọi cạnh ban đầu vì giai đoạn thứ hai cần kiểm tra tất cả các cạnh sau khi tập lựa chọn được xác định. Đồng thời, chỉ các cạnh có giá trị bằng nhau mới được chèn vào`equal_adj`, giữ cho đồ thị lưỡng cực càng nhỏ càng tốt. 

Việc tô màu sử dụng ngăn xếp lặp thay vì DFS đệ quy. Một đường dẫn có thể chứa (3\cdot10^5) đỉnh, có thể vượt quá độ sâu đệ quy mặc định của Python, do đó, đệ quy sẽ yêu cầu cấu hình bổ sung và không cần thiết ở đây. 

các`selected_set`là rất quan trọng. Tư cách thành viên trong danh sách Python sẽ mất thời gian tuyến tính, biến lần quét cạnh cuối cùng thành (O(nm)) trong trường hợp xấu nhất. Một tập hợp đưa ra các kiểm tra thành viên dự kiến ​​(O(1)). 

Giá trị XOR bắt đầu tại (1), không phải (0). Điều này thuận tiện vì các cạnh có giá trị bằng nhau cấm (0) và nó cũng đảm bảo rằng thao tác thực sự thay đổi các giá trị đã chọn. Quan trọng hơn, có (m+1) ứng cử viên từ (1) đến (m+1), nhưng nhiều nhất (m) giá trị bị cấm riêng biệt, do đó vòng lặp phải kết thúc. 

Số nguyên Python không bị tràn và mọi giá trị đầu vào đều ở dưới (2^{20}), vì vậy`a[u] ^ a[v]`cũng ở bên dưới (2^{20}). 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
3 3
1 1 1
1 2
2 3
1 3
```Mọi cạnh đều có giá trị điểm cuối bằng nhau nên cả ba cạnh đều đi vào biểu đồ hai bên. 

| Đỉnh | Màu được chỉ định | Lý do | 
| --- | --- | --- | 
| 1 | 0 | BFS bắt đầu | 
| 2 | 1 | Liền kề 1 | 
| 3 | 0 | Liền kề 2 | 
| 1 và 3 | 0, 0 | Xung đột ở biên (1\text{-}3) | 

Cạnh (1\text{-}3) kết nối các đỉnh có cùng màu, do đó đồ thị có giá trị bằng nhau không phải là đồ thị lưỡng cực. Không có tập lựa chọn nào có thể tách cả ba cạnh bằng nhau và thuật toán sẽ in ra`-1`. 

Điều này chứng tỏ tại sao chỉ kiểm tra các cạnh cục bộ là không đủ. Sự mâu thuẫn chỉ xuất hiện sau khi tô màu xen kẽ xung quanh toàn bộ chu kỳ lẻ. 

### Mẫu 2 

Đầu vào là```
3 3
1 1 2
1 2
2 3
1 3
```Chỉ cạnh (1\text{-}2) có giá trị điểm cuối bằng nhau. 

| Đỉnh | Màu sắc | Đã chọn | Lý do | 
| --- | --- | --- | --- | 
| 1 | 0 | Không | BFS bắt đầu | 
| 2 | 1 | Có | Cạnh có giá trị bằng nhau yêu cầu màu đối diện | 
| 3 | 0 | Không | Đỉnh 3 không được nối bằng cạnh có giá trị bằng nhau | 

Tập được chọn là ({2}). Các cạnh giao nhau và các giá trị cấm của chúng là 

| Cạnh | Giá trị điểm cuối | XOR | Cấm? | 
| --- | --- | --- | --- | 
| (1,2) | (1,1) | 0 | Có | 
| (2,3) | (1,2) | 3 | Có | 
| (1,3) | (1,2) | 3 | Không, cả hai đều không được chọn | 

Ứng cử viên tích cực đầu tiên là (x=1), điều này không bị cấm. 

Sau thao tác, các giá trị trở thành (1,0,2). Tất cả ba cạnh kết nối các giá trị khác nhau, do đó việc xây dựng là hợp lệ. 

Dấu vết này cho thấy các cạnh có giá trị bằng nhau xác định tập hợp con như thế nào trong khi các cạnh có giá trị không bằng nhau chỉ loại bỏ các giá trị có thể có của (x). 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n+m)) dự kiến ​​| Đồ thị có giá trị bằng nhau được duyệt một lần, các cạnh ban đầu được quét một lần và tìm kiếm XOR kiểm tra tối đa (m+1) giá trị | 
| Không gian | (O(n+m)) | Biểu đồ, danh sách cạnh gốc, tô màu và tập hợp đã chọn yêu cầu bộ nhớ tuyến tính | 

Đầu vào tối đa có (3\cdot10^5) đỉnh và (3\cdot10^5) cạnh, do đó việc xử lý tuyến tính phù hợp với giới hạn một giây. Thuật toán không bao giờ thực hiện công việc tỷ lệ thuận với (2^n) hoặc (2^{20}) và mức sử dụng bộ nhớ của nó nằm trong khoảng 256 MB. 

## Trường hợp thử nghiệm 

Bởi vì một vấn đề mang tính xây dựng có thể có nhiều kết quả đầu ra chính xác nên bộ khai thác thử nghiệm hữu ích nhất sẽ phân tích câu trả lời được tạo ra và xác minh nó thay vì so sánh văn bản đầu ra thô. Các khẳng định dưới đây cũng kiểm tra rằng`-1`được báo cáo chính xác khi việc xây dựng là không thể.```python
import sys
import io

def solve_data(inp: str) -> str:
    data = list(map(int, inp.split()))
    it = iter(data)

    n = next(it)
    m = next(it)
    a = [next(it) for _ in range(n)]

    edges = []
    equal_adj = [[] for _ in range(n)]

    for _ in range(m):
        u = next(it) - 1
        v = next(it) - 1
        edges.append((u, v))
        if a[u] == a[v]:
            equal_adj[u].append(v)
            equal_adj[v].append(u)

    color = [-1] * n

    for start in range(n):
        if color[start] != -1:
            continue

        color[start] = 0
        stack = [start]

        while stack:
            u = stack.pop()

            for v in equal_adj[u]:
                if color[v] == -1:
                    color[v] = color[u] ^ 1
                    stack.append(v)
                elif color[v] == color[u]:
                    return "-1\n"

    selected = [i for i in range(n) if color[i] == 1]

    if not selected:
        selected = [0]

    selected_set = set(selected)

    forbidden = set()
    for u, v in edges:
        if (u in selected_set) != (v in selected_set):
            forbidden.add(a[u] ^ a[v])

    x = 1
    while x in forbidden:
        x += 1

    return str(len(selected)) + " " + str(x) + "\n" + \
           " ".join(str(v + 1) for v in selected) + "\n"

def validate(inp: str, out: str) -> bool:
    data = list(map(int, inp.split()))
    it = iter(data)

    n = next(it)
    m = next(it)
    a = [next(it) for _ in range(n)]

    edges = [(next(it) - 1, next(it) - 1) for _ in range(m)]

    tokens = out.split()

    if tokens[0] == "-1":
        return True

    k = int(tokens[0])
    x = int(tokens[1])

    if not (0 <= k <= n):
        return False
    if not (0 <= x < (1 << 20)):
        return False

    chosen = list(map(lambda z: int(z) - 1, tokens[2:2 + k]))

    if len(chosen) != k:
        return False
    if len(set(chosen)) != k:
        return False
    if any(v < 0 or v >= n for v in chosen):
        return False

    chosen_set = set(chosen)

    for u, v in edges:
        au = a[u] ^ x if u in chosen_set else a[u]
        av = a[v] ^ x if v in chosen_set else a[v]

        if au == av:
            return False

    return True

sample1 = """\
3 3
1 1 1
1 2
2 3
1 3
"""

sample2 = """\
3 3
1 1 2
1 2
2 3
1 3
"""

sample3 = """\
5 4
1 2 3 4 5
1 2
1 3
1 4
4 5
"""

assert solve_data(sample1).strip() == "-1", "sample 1"

assert validate(sample2, solve_data(sample2)), "sample 2"
assert validate(sample3, solve_data(sample3)), "sample 3"

single_edge = """\
2 1
0 0
1 2
"""
assert validate(single_edge, solve_data(single_edge)), "minimum valid graph"

all_equal_path = """\
4 3
7 7 7 7
1 2
2 3
3 4
"""
assert validate(all_equal_path, solve_data(all_equal_path)), "all equal values"

boundary_values = """\
3 2
0 1048575 1
1 2
2 3
"""
assert validate(boundary_values, solve_data(boundary_values)), "20-bit boundary values"

maximum_size = ["300000 299999", " ".join(["5"] * 300000)]
maximum_size.extend(f"{i} {i + 1}" for i in range(1, 300000))
maximum_size = "\n".join(maximum_size) + "\n"
assert validate(maximum_size, solve_data(maximum_size)), "maximum-size path"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 |`-1`| Chu kỳ lẻ trong đồ thị có giá trị bằng nhau | 
| Mẫu 2 | Bất kỳ công trình hợp lệ nào | Cạnh có giá trị bằng nhau cộng với cạnh cắt nhau có giá trị không bằng nhau | 
| Mẫu 3 | Bất kỳ công trình hợp lệ nào | Không có cạnh nào có giá trị bằng nhau nên tập hợp đã chọn phải được chọn thủ công | 
|`2 1 / 0 0 / 1 2`| Bất kỳ công trình hợp lệ nào | Đồ thị hợp lệ nhỏ nhất và thực tế là (x=0) bị cấm | 
| Bốn giá trị bằng nhau trên một đường dẫn | Bất kỳ công trình hợp lệ nào | Tô màu lưỡng cực trên một số cạnh có giá trị bằng nhau | 
| Giá trị`0`Và`1048575`| Bất kỳ công trình hợp lệ nào | Giá trị cực đại 20-bit | 
| Đường dẫn 300000 đỉnh | Bất kỳ công trình hợp lệ nào | Hành vi thời gian tuyến tính ở quy mô đỉnh và cạnh tối đa | 

## Vỏ cạnh 

Đối với tam giác đều bằng nhau```
3 3
1 1 1
1 2
2 3
1 3
```đồ thị có giá trị bằng nhau là toàn bộ tam giác. Bắt đầu bằng màu (0) ở đỉnh 1 buộc đỉnh 2 phải tô màu (1) và đỉnh 3 tô màu (0) đến cạnh (2\text{-}3). Sau đó, cạnh (1\text{-}3) nối hai đỉnh màu-(0) nên thuật toán trả về ngay`-1`. 

Đối với đồ thị không có cạnh có giá trị bằng nhau,```
3 2
1 2 3
1 2
2 3
```đồ thị có giá trị bằng nhau không có cạnh, vì vậy tất cả các đỉnh ban đầu đều nhận được màu (0). Mặt màu-(1) trống và thay vào đó, thuật toán chọn đỉnh 1. Các cạnh giao nhau bị cấm (1\oplus2=3), trong khi cạnh (2\text{-}3) không vượt qua tập hợp đã chọn. Do đó (x=1) là hợp lệ và thay đổi đỉnh 1 từ (1) thành (0), để lại các giá trị cạnh khác nhau. 

Đối với các cạnh trùng lặp,```
2 3
7 7
1 2
1 2
1 2
```cùng một cặp xuất hiện ba lần trong biểu đồ có giá trị bằng nhau. BFS vẫn gán hai đỉnh màu khác nhau. Mặt được chọn chứa một điểm cuối và mọi bản sao của cạnh đều cấm (7\oplus7=0). Ứng cử viên đầu tiên (x=1) có giá trị cho cả ba bản sao cùng một lúc. 

Đối với các giá trị đỉnh lớn nhất có thể,```
3 2
0 1048575 1
1 2
2 3
```XOR giữa hai giá trị đầu tiên là (1048575), vẫn nằm trong phạm vi 20 bit. Thuật toán không bao giờ giả định rằng các giá trị XOR là nhỏ về mặt số lượng, chỉ giả định rằng chúng ở dưới (2^{20}). Tìm kiếm ứng cử viên của nó sử dụng các giá trị bắt đầu từ (1) và đối số chuồng chim đảm bảo giá trị miễn phí rất lâu trước khi đạt đến (2^{20}). 

Việc lựa chọn (m+1) ứng viên cũng xử lý tập hợp các giá trị XOR bị cấm tệ nhất có thể. Ngay cả khi mọi một trong (1,\ldots,m) đều bị cấm thì (m+1) không thể bị cấm vì chỉ có (m) cạnh và do đó nhiều nhất là (m) giá trị bị cấm riêng biệt. Vì (m+1\le300001<2^{20}), tìm kiếm luôn tìm thấy giá trị XOR hợp pháp.
