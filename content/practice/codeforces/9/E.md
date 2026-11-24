---
title: "CF 9E - Biểu đồ thú vị và Táo"
description: "Chúng ta bắt đầu với một đa đồ thị vô hướng. Nhiều cạnh được cho phép và các vòng lặp cũng được cho phép. Chúng tôi có thể thêm các cạnh mới và mục tiêu là chuyển đổi biểu đồ thành một cấu trúc rất cụ thể."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "dfs-and-similar", "dsu", "graphs"]
categories: ["algorithms"]
codeforces_contest: 9
codeforces_index: "E"
codeforces_contest_name: "Codeforces Beta Round 9 (Div. 2 Only)"
rating: 2300
weight: 9
solve_time_s: 108
verified: true
draft: false
---
[CF 9E - Biểu đồ thú vị và những quả táo](https://codeforces.com/problemset/problem/9/E) 

**Đánh giá:** 2300 
**Tags:** dfs và tương tự, dsu, đồ thị 
**Thời gian giải:** 1 phút 48s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta bắt đầu với một đa đồ thị vô hướng. Nhiều cạnh được cho phép và các vòng lặp cũng được cho phép. Chúng tôi có thể thêm các cạnh mới và mục tiêu là chuyển đổi biểu đồ thành một cấu trúc rất cụ thể. 

Một đồ thị được gọi là thú vị khi mọi đỉnh đều thuộc đúng một chu trình. Vì các thành phần được kết nối được cho phép nên mỗi thành phần được kết nối phải trông giống như một chu trình đơn giản, có thể có độ dài bằng một nếu nó là một vòng lặp. 

Một cách khác để nói điều tương tự là mọi thành phần được kết nối phải là một vòng. Không cho phép cây vì một số đỉnh sẽ không thuộc về chu trình và không cho phép một thành phần có hai chu trình khác nhau vì các đỉnh trên phần chồng lên nhau sẽ thuộc về nhiều hơn một chu trình. 

Chúng tôi được yêu cầu thêm càng ít cạnh càng tốt. Trong số tất cả các câu trả lời tối ưu, chúng ta phải xuất ra tập hợp các cạnh được thêm vào nhỏ nhất về mặt từ điển. 

Đồ thị có tối đa 50 đỉnh, nhưng có tới 2500 cạnh vì cho phép có nhiều cạnh và vòng lặp. Giá trị nhỏ của`n`có nghĩa là thậm chí`O(n^3)`hoặc`O(n^4)`giải pháp hoàn toàn an toàn. Phần khó hơn không phải là hiệu suất mà là lý luận về cấu trúc và tính tối giản của từ điển. 

Phần khó khăn là hiểu những cấu hình nào là không thể thực hiện được. 

Giả sử một thành phần được kết nối đã chứa hai chu kỳ khác nhau. Thế thì thêm cạnh bao nhiêu cũng không khắc phục được, vì chúng ta chỉ được phép thêm cạnh chứ không bao giờ được bỏ đi. Ví dụ:```
4 4
1 2
2 3
3 1
3 4
```Đỉnh`1,2,3`đã hình thành một chu kỳ. đỉnh`4`được đính kèm dưới dạng nút cây. Thành phần này vẫn hợp lệ vì chúng ta có thể thêm cạnh`(4,4)`và khi đó mọi đỉnh đều thuộc đúng một chu trình. 

Bây giờ so sánh với:```
4 5
1 2
2 3
3 1
2 4
4 3
```Thành phần này đã chứa hai chu kỳ:`1-2-3-1`Và`2-4-3-2`. 

Không có cạnh nào được thêm vào có thể loại bỏ sự chồng chéo này, vì vậy câu trả lời đúng là`NO`. 

Vòng lặp cũng cần được chăm sóc. Một vòng lặp đã được tính là một chu trình chỉ liên quan đến đỉnh đó. Nếu một đỉnh có vòng lặp tham gia vào bất kỳ chu trình nào khác, biểu đồ sẽ ngay lập tức trở nên không hợp lệ. Ví dụ:```
2 2
1 1
1 2
```Biểu đồ này vẫn có thể sửa chữa được. Chúng ta có thể thêm`(2,2)`. 

Nhưng:```
2 3
1 1
1 2
2 1
```chứa cả vòng lặp tại`1`và chu kỳ`1-2-1`, vậy đỉnh`1`thuộc về hai chu kỳ. Câu trả lời là`NO`. 

Một trường hợp tinh tế khác là các cạnh song song. Hai cạnh song song giữa`u`Và`v`tạo thành một chu trình có độ dài hai. Ví dụ:```
2 2
1 2
1 2
```đã là một biểu đồ thú vị. Mỗi đỉnh thuộc đúng một chu trình. 

Một DFS bất cẩn giả định các biểu đồ đơn giản sẽ bỏ lỡ điều này. 

Cuối cùng, thứ tự từ điển đóng vai trò quan trọng trên toàn cầu chứ không phải riêng từng cạnh một. Nếu tồn tại hai cấu trúc tối ưu khác nhau, chúng ta phải ưu tiên cấu trúc có chuỗi điểm cuối được làm phẳng nhỏ hơn về mặt từ điển. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực sẽ thử tất cả các tập cạnh được thêm vào có thể và kiểm tra xem biểu đồ kết quả có trở nên thú vị hay không. Vì có`n(n+1)/2`các cạnh có thể bao gồm các vòng lặp, ngay cả đối với`n = 50`không gian tìm kiếm là thiên văn. Tìm kiếm toàn diện là hoàn toàn không khả thi. 

Quan sát hữu ích đầu tiên là về cấu trúc. Trong một thành phần được kết nối với`v`đỉnh và`e`cạnh thì số chu trình độc lập bằng:```
e - v + 1
```Để có một thành phần thú vị, mỗi đỉnh phải thuộc đúng một chu trình. Điều đó có nghĩa là thành phần phải chứa chính xác một chu trình, bởi vì hai chu trình khác nhau sẽ buộc một số đỉnh thuộc về nhiều chu trình hoặc tạo ra các vùng tuần hoàn bị ngắt kết nối. 

Vì vậy mọi thành phần được kết nối phải thỏa mãn:```
e = v
```Một thành phần được kết nối với`e = v - 1`là một cái cây. Nó chưa chứa chu trình nào, nhưng chúng ta có thể tạo chính xác một chu trình bằng cách thêm một cạnh. 

Một thành phần được kết nối với`e > v`đã có ít nhất hai chu kỳ nên không thể thực hiện được mãi mãi vì không thể loại bỏ các cạnh. 

Điều này biến vấn đề thành một cái gì đó đơn giản hơn nhiều. 

Mỗi thành phần thuộc một trong ba loại: 

Đầu tiên, các thành phần có`e > v`ngay lập tức là không thể. 

Thứ hai, các thành phần có`e = v`đã tốt rồi. Chúng ta không nên chạm vào chúng. 

Thứ ba, các thành phần có`e = v - 1`là cây cối. Chúng ta cần chính xác một cạnh được thêm vào bên trong mỗi thành phần như vậy để tạo một chu trình duy nhất. 

Bây giờ nhiệm vụ trở thành: 

Đối với mỗi thành phần của cây, hãy thêm một cạnh vào giữa hai đỉnh chưa được nối với nhau bằng một cạnh. 

Tại sao các điểm cuối phải không liền kề nhau? Bởi vì việc thêm một cạnh khác giữa các đỉnh liền kề sẽ tạo ra một chu trình sử dụng lại đường đi có độ dài bằng một hiện có, tạo ra một chu trình nhiều cạnh. Điều đó thực sự hợp lệ. Hạn chế thực sự là khác nhau. 

Vấn đề là việc thêm bất kỳ cạnh nào vào trong cây sẽ tạo ra chính xác một chu trình, điều này luôn được chấp nhận. Ngay cả các cạnh và vòng lặp song song cũng được cho phép. Vì vậy, mọi thành phần của cây luôn có thể được cố định bằng một cạnh. 

Thách thức còn lại là sự tối giản về mặt từ điển. 

Giả sử chúng ta có một thành phần cây. Trong số tất cả các cạnh có thể được thêm vào bên trong nó, chúng ta muốn cặp cạnh nhỏ nhất về mặt từ điển`(u,v)`với`u <= v`. 

Vì các thành phần là độc lập nên việc chọn cạnh hợp lệ nhỏ nhất bên trong mỗi thành phần một cách độc lập cũng sẽ giảm thiểu trình tự toàn cục sau khi sắp xếp. 

Chúng ta có thể đơn giản thử tất cả các cặp`(u,v)`theo thứ tự từ điển và chọn cái đầu tiên mà phép cộng tạo ra đúng một chu trình mà không vi phạm điều kiện. 

Đối với một cái cây, mọi cạnh được thêm vào đều có tác dụng. Vì vậy, cạnh hợp lệ nhỏ nhất về mặt từ điển chỉ đơn giản là cặp nhỏ nhất bên trong thành phần. 

Một vòng lặp luôn được cho phép trong thành phần cây vì đỉnh trước đây không thuộc về chu trình. Thêm`(u,u)`tạo ra một chu kỳ chỉ cho`u`, trong khi tất cả các đỉnh khác vẫn nằm ngoài chu trình, điều đó là chưa đủ. Vì vậy, vòng lặp không hợp lệ trừ khi thành phần có kích thước bằng một. 

Đây là sự tinh tế quan trọng. 

Để làm cho mọi đỉnh thuộc cùng một chu trình, cạnh được thêm vào phải nối hai đỉnh phân biệt. Chu trình được tạo sau đó bao gồm toàn bộ đường dẫn giữa chúng. 

Trong cây, thêm cạnh`(u,v)`tạo ra một chu trình bao gồm chính xác đường dẫn từ`u`ĐẾN`v`. 

Để mọi đỉnh trong thành phần thuộc về chu trình đó, đường đi giữa`u`Và`v`phải chứa mọi đỉnh của thành phần. Điều đó có nghĩa là bản thân cái cây phải là một đường đi đơn giản và`u,v`phải là điểm cuối của nó. 

Đây là đặc điểm thực sự. 

Vì vậy, một thành phần có thể sửa chữa được nếu:```
e = v
```hoặc```
e = v - 1
```và trong trường hợp thứ hai, thành phần phải là một đường dẫn. 

Cây phân nhánh không bao giờ có thể trở nên thú vị khi có thêm một cạnh vì một số đỉnh nhánh sẽ nằm ngoài chu trình. 

Ví dụ:```
4 3
1 2
2 3
2 4
```Không có cạnh nào được thêm vào có thể làm cho cả bốn đỉnh thuộc đúng một chu trình. 

Quan sát mang tính quyết định là mọi thành phần được kết nối hợp lệ đều phải có:```
a cycle
```hoặc```
a path
```bởi vì độ trong biểu đồ cuối cùng phải trở thành chính xác hai chu trình bên trong không vòng lặp. 

Như vậy: 

Nếu một thành phần đã có đỉnh có bậc lớn hơn hai thì không thể. 

Nếu một thành phần có nhiều hơn hai đỉnh bậc một thì không thể. 

Nếu một thành phần là một đường dẫn, hãy kết nối hai điểm cuối của nó. 

Nếu một thành phần là một đỉnh bị cô lập, hãy thêm một vòng lặp. 

Trong số tất cả các câu trả lời tối ưu, cấu trúc tham lam này tự động nhỏ nhất về mặt từ điển vì mỗi thành phần chỉ có một phần hoàn thành hợp lệ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Hàm mũ | Hàm mũ | Quá chậm | 
| Tối ưu | O(n²) | O(n²) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng đa đồ thị chính xác như đã cho, bao gồm các vòng và các cạnh song song. 
2. Tính toán cẩn thận bậc của từng đỉnh. 

Một vòng lặp đóng góp`2`theo mức độ trong đồ thị vô hướng. 
3. Tìm các thành phần được kết nối bằng DFS hoặc DSU. 
4. Đối với mỗi thành phần, hãy đếm: 

số đỉnh, 

số cạnh, 

và bậc của mỗi đỉnh bên trong nó. 
5. Nếu bất kỳ đỉnh nào có bậc lớn hơn`2`, đầu ra`NO`. 

Trong một đồ thị thú vị, mọi đỉnh đều thuộc đúng một chu trình, do đó mọi đỉnh phải có bậc chính xác`2`. 
6. Nếu thành phần đã đáp ứng`edges = vertices`, thì đó đã là một chu trình. 

Không nên thêm cạnh nào. 
7. Nếu không thì thành phần phải đáp ứng`edges = vertices - 1`. 

Nếu không thì xuất`NO`. 
8. Trong thành phần đó, hãy đếm các đỉnh bậc`1`. 

Thành phần đường dẫn hợp lệ phải có chính xác hai điểm cuối. 
9. Nếu thành phần có đúng một đỉnh và không có cạnh nào, hãy thêm một vòng lặp`(v,v)`. 
10. Ngược lại nối hai đỉnh bậc 1. 

Điều này sẽ đóng đường dẫn thành một chu trình chứa mỗi đỉnh đúng một lần. 
11. Lưu trữ tất cả các cạnh được thêm vào và sắp xếp chúng theo từ điển. 
12. Xuất danh sách kết quả. 

### Tại sao nó hoạt động 

Bên trong một thành phần liên thông thú vị, mỗi đỉnh phải nằm trên một chu trình đơn giản. Điều đó buộc mọi đỉnh phải có đúng bậc hai. 

Một đồ thị liên thông trong đó mỗi đỉnh có bậc hai chính xác là một thành phần chu trình, bao gồm các vòng lặp và 2 chu trình từ các cạnh song song. 

Nếu một thành phần đã thỏa mãn điều này, chúng tôi sẽ giữ nguyên nó. 

Nếu một thành phần có ít cạnh hơn đỉnh thì đó là cây. Những cây duy nhất có thể trở thành chu trình sau khi thêm một cạnh là các đường đi, bởi vì mọi đỉnh đều phải kết thúc bằng bậc hai. Việc kết nối hai điểm cuối sẽ biến đường dẫn thành một chu trình chứa tất cả các đỉnh. 

Bất kỳ thành phần nào vi phạm các đặc tính cấu trúc này không bao giờ có thể được sửa chữa bằng cách chỉ thêm các cạnh. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

sys.setrecursionlimit(10**6)

def solve():
    n, m = map(int, input().split())

    adj = [[] for _ in range(n)]
    deg = [0] * n
    edges = []

    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1

        adj[u].append(v)
        if u != v:
            adj[v].append(u)

        deg[u] += 1
        deg[v] += 1

        edges.append((u, v))

    visited = [False] * n
    answer = []

    def dfs(start, comp):
        stack = [start]
        visited[start] = True

        while stack:
            u = stack.pop()
            comp.append(u)

            for v in adj[u]:
                if not visited[v]:
                    visited[v] = True
                    stack.append(v)

    for i in range(n):
        if visited[i]:
            continue

        comp = []
        dfs(i, comp)

        comp_set = set(comp)

        edge_count = 0
        for u, v in edges:
            if u in comp_set and v in comp_set:
                edge_count += 1

        size = len(comp)

        bad = False
        leaves = []

        for v in comp:
            if deg[v] > 2:
                bad = True
                break

            if deg[v] == 1:
                leaves.append(v)

        if bad:
            print("NO")
            return

        if edge_count == size:
            for v in comp:
                if deg[v] != 2:
                    print("NO")
                    return

        elif edge_count == size - 1:
            if size == 1:
                if deg[comp[0]] != 0:
                    print("NO")
                    return

                answer.append((comp[0] + 1, comp[0] + 1))

            else:
                if len(leaves) != 2:
                    print("NO")
                    return

                u, v = sorted([leaves[0] + 1, leaves[1] + 1])
                answer.append((u, v))

        else:
            print("NO")
            return

    answer.sort()

    print("YES")
    print(len(answer))

    for u, v in answer:
        print(u, v)

solve()
```Lời giải bắt đầu bằng việc xây dựng đồ thị chính xác như đã cho. Các cạnh và vòng song song được giữ nguyên vì chúng ảnh hưởng đến cấu trúc chu trình. 

Xử lý mức độ là tinh tế. Một vòng lặp đóng góp hai lần vào bậc của đỉnh của nó trong đồ thị vô hướng. Việc triển khai đạt được điều này một cách tự động vì cả hai điểm cuối đều được tăng lên ngay cả khi`u == v`. 

Các thành phần được kết nối được phát hiện bằng DFS. Từ`n <= 50`, hiệu quả không phải là vấn đề đáng lo ngại, nhưng DFS lặp lại hoàn toàn tránh được các vấn đề về độ sâu đệ quy. 

Đối với mỗi thành phần, chúng tôi tính toán lại số cạnh bằng cách quét danh sách cạnh ban đầu. Điều này hoạt động trơn tru ngay cả với các vòng lặp và các cạnh song song. 

Xác nhận trung tâm là điều kiện mức độ. Bất kỳ đỉnh nào có bậc lớn hơn 2 đều không thể đưa ra câu trả lời ngay lập tức. 

Một thành phần với`edges == vertices`phải là một chu kỳ. Mỗi đỉnh phải có đúng bằng hai. 

Một thành phần với`edges == vertices - 1`là một cái cây. Cây hợp lệ duy nhất là đường dẫn. Đối với một đường đi, hai đỉnh bậc 1 được kết nối để đóng chu trình. 

Trường hợp đỉnh cô lập cần được xử lý đặc biệt. Việc kết nối đỉnh với chính nó sẽ tạo ra một chu trình vòng lặp có độ dài bằng một. 

Cuối cùng, tất cả các cạnh được thêm vào sẽ được sắp xếp theo thứ tự từ điển trước khi xuất ra. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 2
1 2
2 3
```Đồ thị là một đường dẫn duy nhất. 

| Bước | Thành phần | Bằng cấp | Đếm cạnh | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | {1,2,3} | [1,2,1] | 2 | Đường dẫn được phát hiện | 
| 2 | điểm cuối = (1,3) | - | - | Cộng (1,3) | 

Các cạnh được thêm cuối cùng:```
1 3
```Đồ thị mới trở thành chu trình`1-2-3-1`và mọi đỉnh đều thuộc đúng một chu trình. 

### Ví dụ 2 

đầu vào:```
4 3
1 2
2 3
2 4
```Biểu đồ này là một cây phân nhánh. 

| Bước | Thành phần | Bằng cấp | Đếm cạnh | Hành động | 
| --- | --- | --- | --- | --- | 
| 1 | {1,2,3,4} | [1,3,1,1] | 3 | Bằng cấp 3 được tìm thấy | 

Vì đỉnh`2`đã có bậc lớn hơn hai thì không thể có được đồ thị thú vị cuối cùng. 

Đầu ra:```
NO
```Dấu vết này thể hiện tính bất biến chính: mọi đỉnh trong đồ thị cuối cùng phải có đúng hai bậc. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n²) | DFS cộng với quét cạnh lặp đi lặp lại trên các ràng buộc nhỏ | 
| Không gian | O(n²) | Lưu trữ kề trong trường hợp xấu nhất 

Chỉ với 50 đỉnh và tối đa 2500 cạnh, giải pháp này dễ dàng nằm gọn trong giới hạn. Ngay cả một giải pháp khối cũng có thể trôi qua một cách thoải mái. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline

    n, m = map(int, input().split())

    adj = [[] for _ in range(n)]
    deg = [0] * n
    edges = []

    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1

        adj[u].append(v)
        if u != v:
            adj[v].append(u)

        deg[u] += 1
        deg[v] += 1

        edges.append((u, v))

    visited = [False] * n
    answer = []

    def dfs(start, comp):
        stack = [start]
        visited[start] = True

        while stack:
            u = stack.pop()
            comp.append(u)

            for v in adj[u]:
                if not visited[v]:
                    visited[v] = True
                    stack.append(v)

    out = []

    for i in range(n):
        if visited[i]:
            continue

        comp = []
        dfs(i, comp)

        comp_set = set(comp)

        edge_count = 0
        for u, v in edges:
            if u in comp_set and v in comp_set:
                edge_count += 1

        size = len(comp)

        leaves = []

        for v in comp:
            if deg[v] > 2:
                return "NO\n"

            if deg[v] == 1:
                leaves.append(v)

        if edge_count == size:
            for v in comp:
                if deg[v] != 2:
                    return "NO\n"

        elif edge_count == size - 1:
            if size == 1:
                answer.append((comp[0] + 1, comp[0] + 1))
            else:
                if len(leaves) != 2:
                    return "NO\n"

                u, v = sorted([leaves[0] + 1, leaves[1] + 1])
                answer.append((u, v))

        else:
            return "NO\n"

    answer.sort()

    out.append("YES")
    out.append(str(len(answer)))

    for u, v in answer:
        out.append(f"{u} {v}")

    return "\n".join(out) + "\n"

# provided sample
assert run(
"""3 2
1 2
2 3
"""
) == (
"""YES
1
1 3
"""
), "sample 1"

# isolated vertex
assert run(
"""1 0
"""
) == (
"""YES
1
1 1
"""
), "single vertex loop"

# already valid cycle
assert run(
"""3 3
1 2
2 3
3 1
"""
) == (
"""YES
0
"""
), "already interesting"

# branching tree impossible
assert run(
"""4 3
1 2
2 3
2 4
"""
) == (
"""NO
"""
), "degree > 2 impossible"

# parallel edges form cycle
assert run(
"""2 2
1 2
1 2
"""
) == (
"""YES
0
"""
), "2-cycle with parallel edges"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 0`| thêm vòng lặp | xử lý đỉnh bị cô lập | 
| đồ thị tam giác |`YES 0`| chu kỳ đã hợp lệ | 
| cây phân nhánh |`NO`| độ lớn hơn hai | 
| các cạnh song song |`YES 0`| xử lý chu trình đa đồ thị | 

## Vỏ cạnh 

Xét trường hợp đỉnh bị cô lập:```
1 0
```Thành phần này có một đỉnh và không có cạnh nào. Về mặt kỹ thuật, nó là một cái cây nhưng việc kết nối các điểm cuối là không thể vì chỉ có một đỉnh. 

Thuật toán phát hiện`size == 1`và thêm vòng lặp`(1,1)`. 

Đồ thị cuối cùng có một chu trình chứa đỉnh duy nhất nên câu trả lời là hợp lệ. 

Bây giờ hãy xem xét các cạnh song song:```
2 2
1 2
1 2
```Bằng cấp trở thành`[2,2]`. Thành phần có`2`đỉnh và`2`các cạnh, vì vậy nó đã là một thành phần chu trình. 

Việc triển khai biểu đồ đơn giản có thể từ chối điều này một cách không chính xác, nhưng thuật toán chấp nhận nó một cách chính xác. 

Cuối cùng, hãy xem xét một thành phần có chu kỳ chồng chéo:```
4 5
1 2
2 3
3 1
2 4
4 3
```Độ đỉnh là`[2,3,3,2]`. 

Thuật toán ngay lập tức bác bỏ đồ thị vì các đỉnh`2`Và`3`đã có bằng cấp lớn hơn hai. 

Không có cạnh nào được thêm vào có thể sửa chữa được cấu trúc này vì việc loại bỏ các chu trình là không thể.
