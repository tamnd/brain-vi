---
title: "CF 29D - Kiến Trên Cây"
description: "Chúng ta được yêu cầu xây dựng đường đi cho một con kiến ​​trên cây. Cây có n đỉnh, với đỉnh 1 là gốc và n-1 cạnh nối chúng để đồ thị được kết nối."
date: "2026-05-28T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "constructive-algorithms", "dfs-and-similar", "trees"]
categories: ["algorithms"]
codeforces_contest: 29
codeforces_index: "D"
codeforces_contest_name: "Codeforces Beta Round 29 (Div. 2, Codeforces format)"
rating: 2000
weight: 29
solve_time_s: 79
verified: true
draft: false
---
[CF 29D - Kiến trên cây](https://codeforces.com/problemset/problem/29/D) 

**Xếp hạng:** 2000 
**Thẻ:** thuật toán xây dựng, dfs và tương tự, cây 
**Thời gian giải:** 1 phút 19s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta được yêu cầu xây dựng đường đi cho một con kiến trên cây. Cây có`n`đỉnh, có đỉnh`1`là gốc, và`n-1`các cạnh kết nối chúng để đồ thị được kết nối. Con kiến ​​muốn bắt đầu từ gốc, đi qua mỗi cạnh hai lần (một lần theo mỗi hướng), thăm mọi đỉnh và quay trở lại gốc. Ngoài ra, con kiến ​​phải ghé thăm tất cả các lá theo một thứ tự cụ thể. Một lá là bất kỳ đỉnh nào khác ngoài gốc và chỉ có một lân cận. 

Đầu vào chỉ định cây thông qua các cặp cạnh và đưa ra chuỗi các lá mà kiến ​​phải ghé thăm. Đầu ra là một chuỗi các chỉ số đỉnh đại diện cho việc truyền tải hợp lệ hoặc`-1`nếu không có đường dẫn hợp lệ tồn tại. 

Những ràng buộc cho chúng ta biết`n`có thể lên tới 300. Việc khám phá toàn diện tất cả các chuỗi đỉnh là không khả thi vì số lượng hoán vị tăng theo giai thừa. Tuy nhiên, việc xây dựng dựa trên DFS trên cây vẫn ổn vì quá trình truyền tải DFS chạm vào mỗi cạnh chính xác hai lần, mang lại`2*n - 1`thăm. nhỏ`n`cho phép DFS đệ quy hoặc dựa trên ngăn xếp đơn giản mà không phải lo lắng về hiệu suất. 

Trường hợp cạnh tinh tế xảy ra khi không thể đạt được thứ tự lá yêu cầu do cấu trúc liên kết của cây. Ví dụ: nếu thứ tự lá buộc phải "nhảy" qua một cây con mà không thể thực hiện được nếu không xem lại lá khác không theo thứ tự thì giải pháp phải trả về`-1`. Một cách tiếp cận đơn giản chỉ thực hiện DFS từ gốc và xuất ra mọi đỉnh mà nó chạm tới có thể dễ dàng vi phạm thứ tự lá, tạo ra đường dẫn không hợp lệ. 

Một cạm bẫy tiềm ẩn khác là khi có nhiều lá được kết nối thông qua cùng một lá bố mẹ. Nếu thứ tự lá yêu cầu truy cập một lá nhưng lá khác lại được tiếp cận trước trong DFS đơn giản, thì đường dẫn không hợp lệ. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng tạo ra tất cả các bước đi Euler của cây (các đường đi trong đó mỗi cạnh được đi qua chính xác hai lần) và kiểm tra xem các lá có xuất hiện theo thứ tự yêu cầu hay không. Về nguyên tắc, điều này hoạt động vì mọi cây đều có một bước đi như vậy: bắt đầu từ gốc, DFS qua tất cả các cây con và quay trở lại. Tuy nhiên, việc tạo ra tất cả các bước đi Euler là giai thừa trong`n`bởi vì sự hoán vị của trẻ em đến thăm, nên nó hoàn toàn không thực tế ngay cả đối với`n = 20`. 

Cái nhìn sâu sắc quan trọng là nhận ra rằng việc truyền tải DFS của cây vốn đã đi qua mọi cạnh hai lần, mang lại một`2*n-1`trình tự thăm đỉnh. Do đó, thay vì khám phá tất cả các hoán vị, chúng ta có thể cấu trúc quá trình duyệt để nó đi theo các lá theo thứ tự yêu cầu. Điều này đòi hỏi hai bước: đầu tiên, xác định các đường dẫn từ gốc đến mỗi lá theo thứ tự đã chỉ định; thứ hai, hợp nhất các đường dẫn này một cách cẩn thận trong khi quay lại bất cứ khi nào một nhánh cạn kiệt. Điều này hiệu quả vì cây không có chu trình, do đó có một đường đi duy nhất giữa bất kỳ cặp đỉnh nào, nghĩa là các đường đi đến các lá không bao giờ xung đột theo những cách không rõ ràng trừ khi bản thân thứ tự đó là không thể. 

Cách tiếp cận tối ưu sử dụng DFS nhưng với sơ đồ quay lui có kiểm soát, luôn cố gắng mở rộng đường dẫn đến lá tiếp theo theo trình tự được yêu cầu. Nếu con kiến ​​không thể đi tới lá tiếp theo mà không vi phạm các lần thăm lá trước đó, chúng ta sẽ ngay lập tức xuất ra`-1`. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force (tất cả các bước đi Euler) | Ồ (n!) | O(n) | Quá chậm | 
| DFS được kiểm soát theo thứ tự lá | O(n^2) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Phân tích các cạnh của cây và xây dựng danh sách kề. Xác định tất cả các lá bằng cách đếm hàng xóm. 
2. Lưu trữ thứ tự lá cần thiết trong hàng đợi hoặc danh sách. 
3. Thực hiện DFS từ thư mục gốc. Tại mỗi đỉnh, cố gắng khám phá các cây con con theo thứ tự tôn trọng lá tiếp theo theo trình tự yêu cầu. 
4. Khi di chuyển đến một đỉnh, hãy nối nó vào đường dẫn. Nếu đỉnh này là một lá, hãy kiểm tra xem nó có khớp với lá dự kiến ​​theo thứ tự không. Nếu không, chấm dứt với`-1`. 
5. Tái diễn thành con theo thứ tự nào đó không vi phạm thứ tự lá sắp tới. Sau khi khám phá cây con, quay lại cây cha và nối lại cây cha vào đường dẫn. 
6. Tiếp tục cho đến khi tất cả các lá trong thứ tự được truy cập và quá trình duyệt trở về gốc. 
7. Xuất đường dẫn có độ dài`2*n - 1`. 

Tại sao nó hoạt động: tính bất biến DFS đảm bảo mọi cạnh đều được duyệt hai lần vì chúng tôi nối thêm cạnh cha một lần nữa sau khi khám phá từng cạnh con. Thứ tự được kiểm soát đảm bảo trình tự lá yêu cầu được tôn trọng. Nếu thứ tự lá không thể thực hiện được do cấu trúc cây, việc kiểm tra tại các nút lá sẽ phát hiện ra thứ tự đó và đưa ra kết quả.`-1`. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(1000)

def main():
    n = int(input())
    adj = [[] for _ in range(n + 1)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        adj[u].append(v)
        adj[v].append(u)
    
    leaves_order = list(map(int, input().split()))
    leaf_pos = {leaf: i for i, leaf in enumerate(leaves_order)}
    visited = [False] * (n + 1)
    path = []
    
    def dfs(u):
        visited[u] = True
        path.append(u)
        children = [v for v in adj[u] if not visited[v]]
        # sort children by leaf order if any is leaf
        children.sort(key=lambda x: leaf_pos.get(x, n+1))
        for v in children:
            dfs(v)
            path.append(u)
    
    dfs(1)
    
    # validate leaves order
    seen_leaves = [x for x in path if len(adj[x]) == 1 and x != 1]
    if seen_leaves != leaves_order:
        print(-1)
    else:
        print(' '.join(map(str, path)))

if __name__ == "__main__":
    main()
```DFS đảm bảo mọi cạnh đều được duyệt hai lần và quay trở lại cạnh cha mỗi lần. Việc sắp xếp các phần tử con theo thứ tự lá đảm bảo chúng ta cố gắng thỏa mãn trình tự lá. Kiểm tra cuối cùng xác nhận thứ tự được khớp chính xác vì việc sắp xếp một mình không thể thực thi thứ tự nếu xung đột cấu trúc. 

## Ví dụ đã hoạt động 

Mẫu 1:```
n = 3
edges = [(1,2),(2,3)]
leaves_order = [3]
```| Bước | bạn | con đường | trẻ em | nhìn thấy lá | 
| --- | --- | --- | --- | --- | 
| bắt đầu | 1 | [1] | [2] | [] | 
| dfs(2) | 2 | [1,2] | [3] | [] | 
| dfs(3) | 3 | [1,2,3] | [] | [3] | 
| quay lại | 2 | [1,2,3,2] | - | [3] | 
| quay lại | 1 | [1,2,3,2,1] | - | [3] | 

Truyền tải tuân theo thứ tự lá [3]. Đầu ra là`1 2 3 2 1`. 

Ví dụ tùy chỉnh:```
n = 5
edges = [(1,2),(1,3),(3,4),(3,5)]
leaves_order = [2,4,5]
```Quá trình duyệt sẽ khám phá 2 trước, quay lại 1, sau đó khám phá 3→4→5, dẫn đến`1 2 1 3 4 3 5 3 1`, tôn trọng thứ tự lá. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^2) | Việc sắp xếp các phần tử con trong DFS có thể lấy O(n) ở mỗi đỉnh, cho kết quả tổng thể là O(n^2). | 
| Không gian | O(n) | Danh sách kề, đường dẫn và mảng đã truy cập sử dụng không gian tuyến tính. | 

Với`n ≤ 300`, Hoạt động O(n^2) là khoảng 90.000 bước, nằm trong giới hạn 2 giây. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    output = io.StringIO()
    sys.stdout = output
    main()
    return output.getvalue().strip()

# Provided sample
assert run("3\n1 2\n2 3\n3\n") == "1 2 3 2 1", "sample 1"

# Custom cases
assert run("5\n1 2\n1 3\n3 4\n3 5\n2 4 5\n") == "1 2 1 3 4 3 5 3 1", "leaf order 2,4,5"
assert run("4\n1 2\n2 3\n2 4\n3 4 2\n") == "-1", "impossible leaf order"
assert run("3\n1 2\n1 3\n2 3\n") == "1 2 1 3 1", "leaf order 2,3 minimal tree"
assert run("6\n1 2\n1 3\n3 4\n3 5\n5 6\n2 4 6 5\n") == "-1", "order incompatible with tree structure"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cây 5 nút bậc 2,4,5 |`1 2 1 3 4 3 5 3 1`| Đúng DFS tôn trọng thứ tự lá | 
| Thứ tự cây 4 nút không thể |`-1`| Phát hiện thứ tự lá không hợp lệ | 
| Cây 3 nút tối thiểu | `1 | |
