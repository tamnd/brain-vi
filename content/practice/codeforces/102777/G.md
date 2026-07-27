---
title: "CF 102777G - \u0422\u043e\u0432\u0430\u0440\u0438\u0449 \u043c\u0430\u0439\u043e\u0440"
description: "Đầu vào mô tả một mạng lưới tội phạm. Mỗi tội phạm là một đỉnh của đồ thị vô hướng và mỗi kết nối được ghi lại là một cạnh giữa hai đỉnh. Cộng đồng tội phạm là một nhóm tội phạm có liên kết với nhau, nhưng một tội phạm riêng lẻ không được tính là một cộng đồng."
date: "2026-07-27T20:35:24+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102777
codeforces_index: "G"
codeforces_contest_name: "ICPC Central Russia Regional Contest (CRRC 19), \u0427\u0435\u043c\u043f\u0438\u043e\u043d\u0430\u0442 \u0426\u0435\u043d\u0442\u0440\u0430\u043b\u044c\u043d\u043e\u0439 \u0420\u043e\u0441\u0441\u0438\u0438, \u043a\u0432\u0430\u043b\u0438\u0444\u0438\u043a\u0430\u0446\u0438\u043e\u043d\u043d\u044b\u0439 \u0440\u0430\u0443\u043d\u0434"
rating: 0
weight: 102777
solve_time_s: 65
verified: true
draft: false
---

[CF 102777G - \u0422\u043e\u0432\u0430\u0440\u0438\u0449 \u043c\u0430\u0439\u043e\u0440](https://codeforces.com/problemset/problem/102777/G) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 5s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Đầu vào mô tả một mạng lưới tội phạm. Mỗi tội phạm là một đỉnh của đồ thị vô hướng và mỗi kết nối được ghi lại là một cạnh giữa hai đỉnh. Cộng đồng tội phạm là một nhóm tội phạm có liên kết với nhau, nhưng một tội phạm riêng lẻ không được tính là một cộng đồng. Nhiệm vụ là xác định có bao nhiêu thành phần liên thông của đồ thị chứa ít nhất hai đỉnh. 

Dòng đầu tiên cung cấp số lượng tội phạm và số lượng kết nối được ghi lại. Các cặp sau đây mô tả những tội phạm nào có liên quan trực tiếp. Các kết nối có thể tạo thành dây chuyền, vì vậy hai tên tội phạm thuộc cùng một cộng đồng ngay cả khi không có ranh giới trực tiếp giữa chúng, miễn là có một con đường xuyên qua những tên tội phạm khác. Đầu ra là số lượng các thành phần được kết nối không tầm thường như vậy. 

Số lượng tội phạm nhiều nhất là 100 và số lượng kết nối có thể lớn bằng bình phương số lượng tội phạm. Với những giới hạn này, ngay cả các phương pháp truyền tải đồ thị đơn giản cũng có thể đủ nhanh. Một giải pháp có độ phức tạp về thời gian khoảng O(N + M) là quá đủ. Các phương pháp tốn kém hơn, liên tục tìm kiếm tất cả các đường đi giữa tất cả các cặp tội phạm là không cần thiết và sẽ thực hiện nhiều công việc hơn so với cấu trúc của bài toán yêu cầu. 

Một số trường hợp cần được chăm sóc. Một biểu đồ không chứa cộng đồng thực sẽ tạo ra số 0, ngay cả khi các đỉnh tồn tại. 

Ví dụ:```
3 0
```Câu trả lời là:```
0
```Việc triển khai bất cẩn coi mọi đỉnh là một thành phần sẽ trả về sai 3. 

Một cặp tội phạm được kết nối phải được tính là một cộng đồng. 

Ví dụ:```
2 1
0 1
```Câu trả lời là:```
1
```Việc triển khai chỉ tìm kiếm các đỉnh có nhiều cạnh có thể bỏ sót điều này vì mỗi tội phạm chỉ có một kết nối. 

Sự tự kết nối không biến một tên tội phạm bị cô lập thành một cộng đồng. 

Ví dụ:```
3 1
1 1
```Câu trả lời là:```
0
```Đỉnh có một vòng lặp, nhưng thành phần được kết nối vẫn chỉ chứa một tội phạm. Giải pháp đếm mọi thành phần chứa cạnh sẽ cho kết quả sai. 

## Phương pháp tiếp cận 

Một ý tưởng bạo lực trực tiếp là bắt đầu từ mọi tội phạm và tìm kiếm tất cả tội phạm có thể tiếp cận được từ đó. Sau khi tìm thấy tập hợp có thể truy cập, chúng tôi có thể kiểm tra xem kích thước của nó có lớn hơn một hay không và tính nó là một cộng đồng. Để tránh tính cùng một cộng đồng nhiều lần, chúng tôi cần theo dõi xem tội phạm nào đã được xử lý. 

Nếu không có sự tối ưu hóa đó, thành phần tương tự sẽ được khám phá nhiều lần. Trong trường hợp xấu nhất, một biểu đồ dày đặc với 100 tội phạm có thể có khoảng 10.000 kết nối. Chạy toàn bộ đường truyền từ mọi đỉnh sẽ thực hiện khoảng O(N(N + M)), tức là khoảng một triệu phép tính ở đây. Nó vẫn sẽ vượt qua những ràng buộc này, nhưng nó bỏ qua cấu trúc đơn giản hơn của nhiệm vụ. 

Quan sát quan trọng là cộng đồng chính xác là một thành phần được kết nối của đồ thị vô hướng. Chúng ta không cần tìm đường đi giữa các cặp tội phạm cụ thể. Chúng ta chỉ cần khám phá mọi vùng được kết nối một lần và kiểm tra kích thước của nó. 

Việc duyệt đồ thị chẳng hạn như tìm kiếm theo chiều sâu hoặc tìm kiếm theo chiều rộng thực hiện điều này một cách tự nhiên. Bắt đầu từ một tên tội phạm không được viếng thăm, quá trình truyền tải sẽ đến thăm tất cả mọi người trong cùng một thành phần. Sau khi quá trình truyền tải kết thúc, chúng ta biết kích thước thành phần. Nếu kích thước đó ít nhất là hai thì nó sẽ đóng góp một sao. 

Brute-force hoạt động vì khả năng tiếp cận có thể được phát hiện bằng cách truyền tải đồ thị, nhưng về mặt khái niệm thì nó không thành công vì nó lặp lại cùng một công việc. Quan sát cho thấy mỗi tội phạm thuộc về chính xác một thành phần được kết nối cho phép chúng ta xử lý mỗi đỉnh và cạnh chỉ một lần. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(N(N + M)) | O(N) | Hoạt động ở đây nhưng không cần thiết | 
| Tối ưu | O(N + M) | O(N + M) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng danh sách kề cho đồ thị. Đối với mọi kết nối giữa hai tội phạm khác nhau, hãy lưu trữ từng điểm cuối trong danh sách của kẻ kia vì biểu đồ là vô hướng. Một vòng lặp tự có thể được lưu trữ hoặc bỏ qua, vì một thành phần có kích thước bằng một không bao giờ đóng góp vào câu trả lời. 
2. Tạo một mảng ghi lại từng tội phạm đã được truy cập hay chưa. Ban đầu, mọi tội phạm đều không được truy cập vì không có thành phần kết nối nào được khám phá. 
3. Lặp lại tất cả tội phạm. Khi tìm thấy một tên tội phạm chưa được truy cập, hãy khởi động DFS từ nó và đếm xem có bao nhiêu đỉnh đạt được. Quá trình truyền tải đánh dấu mọi tội phạm trong thành phần được kết nối này, vì vậy các lần lặp lại sau này sẽ bỏ qua cùng một cộng đồng. 
4. Sau khi DFS kết thúc, hãy kiểm tra kích thước của thành phần được phát hiện. Nếu nó có ít nhất hai tội phạm, hãy tăng câu trả lời lên một. Một đỉnh được viếng thăm chỉ là một tên tội phạm biệt lập, không phải một cộng đồng. 
5. In số thành phần cuối cùng được đếm. 

Tại sao nó hoạt động: 

Điều bất biến trong quá trình thuật toán là mỗi lần truyền tải DFS hoàn thành biểu thị chính xác một thành phần được kết nối. DFS chỉ di chuyển dọc theo các cạnh hiện có nên không thể rời khỏi thành phần đó. Đồng thời, mọi đỉnh có thể tiếp cận được thông qua một đường dẫn cuối cùng đều được truy cập, do đó không có thành viên nào của thành phần bị bỏ sót. Vì mỗi đỉnh chỉ bắt đầu một DFS khi nó chưa được nhìn thấy trước đó nên mọi thành phần được kết nối đều được tính chính xác một lần. Kiểm tra kích thước cuối cùng sẽ loại bỏ các thành phần một đỉnh, phù hợp với định nghĩa về cộng đồng tội phạm. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())

    graph = [[] for _ in range(n)]

    for _ in range(m):
        x, y = map(int, input().split())
        if x != y:
            graph[x].append(y)
            graph[y].append(x)

    visited = [False] * n
    answer = 0

    for start in range(n):
        if visited[start]:
            continue

        stack = [start]
        visited[start] = True
        size = 0

        while stack:
            v = stack.pop()
            size += 1

            for to in graph[v]:
                if not visited[to]:
                    visited[to] = True
                    stack.append(to)

        if size > 1:
            answer += 1

    print(answer)

if __name__ == "__main__":
    solve()
```Biểu đồ được lưu trữ dưới dạng danh sách kề vì việc duyệt qua chỉ cần biết tội phạm nào được kết nối trực tiếp. Việc lưu trữ một ma trận kề đầy đủ cũng sẽ phù hợp với các ràng buộc, nhưng nó sẽ sử dụng nhiều bộ nhớ hơn và sẽ làm cho giải pháp ít được kết nối trực tiếp hơn với ý tưởng truyền tải đồ thị. 

DFS lặp lại sử dụng ngăn xếp thay vì đệ quy. Python recursion depth can become a problem on long chains in larger graph tasks, so the explicit stack avoids that limitation.

 Biến`size`đếm các đỉnh trong thành phần được kết nối hiện tại. Nó chỉ được kiểm tra sau khi quá trình truyền tải kết thúc vì thuật toán phải biết toàn bộ kích thước thành phần trước khi quyết định xem nó có đại diện cho một cộng đồng hay không. 

Bỏ qua các vòng tự lặp khi xây dựng biểu đồ là an toàn. Vòng lặp tự không bao giờ tạo ra một đỉnh khác trong thành phần và câu hỏi duy nhất quan trọng là liệu kích thước thành phần có vượt quá một đỉnh hay không. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
10 4
1 2
1 3
0 2
0 1
```Dấu vết: 

| Bước | Đỉnh hiện tại | Các đỉnh đã truy cập | Kích thước thành phần hiện tại | Trả lời | 
| --- | --- | --- | --- | --- | 
| Bắt đầu DFS lúc 0 | 0 | {0} | 0 | 0 | 
| Thăm hàng xóm 1 | 1 | {0,1} | 1 | 0 | 
| Thăm hàng xóm 3 | 3 | {0,1,3} | 2 | 0 | 
| Thăm hàng xóm 2 | 2 | {0,1,2,3} | 3 | 0 | 
| Thành phần hoàn thiện | | {0,1,2,3} | 4 | 1 | 
| Kiểm tra các đỉnh còn lại | | {0,1,2,3} đã truy cập | | 1 | 

Bốn tên tội phạm có liên quan tạo thành một thành phần. Sáu tội phạm còn lại đang bị cô lập nên không tăng số lượng. 

### Ví dụ tùy chỉnh 2 

đầu vào:```
5 2
0 1
3 4
```Dấu vết: 

| Bước | Đỉnh hiện tại | Các đỉnh đã truy cập | Kích thước thành phần hiện tại | Trả lời | 
| --- | --- | --- | --- | --- | 
| Bắt đầu DFS lúc 0 | 0 | {0} | 0 | 0 | 
| Thăm 1 | 1 | {0,1} | 1 | 0 | 
| Thành phần hoàn thiện | | {0,1} | 2 | 1 | 
| Bắt đầu DFS lúc 2 | 2 | {2} | 1 | 1 | 
| Thành phần hoàn thiện | | {2} | 1 | 1 | 
| Bắt đầu DFS lúc 3 | 3 | {3} | 0 | 1 | 
| Visit 4 | 4 | {3,4} | 1 | 1 | 
| Thành phần hoàn thiện | | {3,4} | 2 | 2 | 

This example shows that isolated criminals are ignored while multiple separate communities are counted independently.

 ## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(N + M) | Mỗi tội phạm được truy cập một lần và mọi cạnh được lưu trữ đều được kiểm tra một lần. | 
| Không gian | O(N + M) | Danh sách kề lưu trữ biểu đồ và mảng truyền tải lưu trữ dữ liệu bổ sung O(N). | 

Đồ thị lớn nhất chỉ có 100 đỉnh nên việc truyền tuyến tính dễ dàng phù hợp trong giới hạn thời gian. Việc sử dụng bộ nhớ cũng thấp hơn nhiều so với giới hạn 256 MB. 

## Trường hợp thử nghiệm```python
import sys
import io

def solve():
    input = sys.stdin.readline

    n, m = map(int, input().split())
    graph = [[] for _ in range(n)]

    for _ in range(m):
        x, y = map(int, input().split())
        if x != y:
            graph[x].append(y)
            graph[y].append(x)

    visited = [False] * n
    ans = 0

    for i in range(n):
        if not visited[i]:
            stack = [i]
            visited[i] = True
            size = 0

            while stack:
                v = stack.pop()
                size += 1
                for u in graph[v]:
                    if not visited[u]:
                        visited[u] = True
                        stack.append(u)

            if size > 1:
                ans += 1

    return str(ans)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    try:
        sys.stdin = io.StringIO(inp)
        return solve()
    finally:
        sys.stdin = old_stdin

assert run("""10 4
1 2
1 3
0 2
0 1
""") == "1", "sample 1"

assert run("""5 2
0 1
3 4
""") == "2", "two communities"

assert run("""1 0
""") == "0", "minimum isolated graph"

assert run("""4 4
0 1
1 2
2 3
3 0
""") == "1", "cycle community"

assert run("""5 3
0 0
2 2
3 4
""") == "1", "self loops and one real community"

assert run("""100 99
""" + "\n".join(f"{i} {i+1}" for i in range(99)) + "\n") == "1", "maximum chain"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Biểu đồ mẫu | 1 | Ví dụ ban đầu với một thành phần không tầm thường | 
| Hai cặp ngắt kết nối | 2 | Nhiều cộng đồng được tính riêng | 
| Một tên tội phạm bị cô lập | 0 | Các đỉnh đơn bị bỏ qua | 
| Bốn chu kỳ | 1 | Cấu trúc được kết nối lớn hơn được tính một lần | 
| Tự vòng với một cặp | 1 | Vòng lặp không tạo ra cộng đồng giả mạo | 
| Chuỗi 100 tội phạm | 1 | Thành phần được kết nối lớn và độ sâu truyền tải | 

## Vỏ cạnh 

Một đầu vào chỉ có tội phạm bị cô lập:```
3 0
```bắt đầu một DFS từ mỗi đỉnh, nhưng mỗi lần duyệt sẽ truy cập chính xác một đỉnh. Việc kiểm tra kích thước sẽ loại bỏ cả ba thành phần, để lại câu trả lời là 0. Thuật toán xử lý việc này vì khả năng kết nối và quy mô cộng đồng được kiểm tra riêng biệt. 

Một cặp được kết nối:```
2 1
0 1
```khiến DFS đầu tiên truy cập vào cả hai đỉnh. Kích thước thành phần trở thành 2, do đó câu trả lời tăng lên 1. Thuật toán không yêu cầu một thành phần phải có nhiều cạnh, chỉ có nhiều đỉnh. 

Một vòng lặp tự:```
3 1
1 1
```không tạo ra một cộng đồng hợp lệ. Biểu diễn biểu đồ bỏ qua vòng lặp và DFS từ đỉnh 1 vẫn có kích thước 1. Câu trả lời vẫn là 0 vì một tội phạm không thể hình thành một cộng đồng. 

Một biểu đồ có một số nhóm bị ngắt kết nối:```
6 3
0 1
1 2
4 5
```được xử lý thành ba thành phần. Cái đầu tiên có kích thước 3 và đóng góp một ngôi sao, cái thứ hai bị cô lập và không đóng góp gì, cái thứ ba có kích thước 2 và đóng góp thêm một ngôi sao. Câu trả lời cuối cùng là 2. Điều này xác nhận rằng quá trình truyền tải đếm các thành phần một cách độc lập thay vì đếm các đỉnh hoặc cạnh.
