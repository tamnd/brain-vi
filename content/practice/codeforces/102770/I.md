---
title: "CF 102770I - Kêu gọi phép thuật"
description: "Sau giai đoạn giặt đầu tiên, mỗi màu tất sẽ xuất hiện đúng hai lần, nhưng hai chiếc tất cùng màu có thể được đặt ở các cặp khác nhau. Chúng ta có thể xem mỗi cặp ban đầu là sự kết nối giữa hai màu."
date: "2026-07-28T23:12:50+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102770
codeforces_index: "I"
codeforces_contest_name: "The 17th Zhejiang Provincial Collegiate Programming Contest"
rating: 0
weight: 102770
solve_time_s: 44
verified: true
draft: false
---

[CF 102770I - Kêu gọi phép thuật](https://codeforces.com/problemset/problem/102770/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 44s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Sau giai đoạn giặt đầu tiên, mỗi màu tất sẽ xuất hiện đúng hai lần, nhưng hai chiếc tất cùng màu có thể được đặt ở các cặp khác nhau. Chúng ta có thể xem mỗi cặp ban đầu là sự kết nối giữa hai màu. Một cặp chứa màu sắc`a`Và`b`trở thành một cạnh giữa các đỉnh`a`Và`b`trong một biểu đồ. 

Vì mỗi màu xuất hiện trong đúng hai chiếc tất nên mỗi đỉnh đều có đúng hai bậc. Do đó, đồ thị được tạo thành từ các chu trình độc lập. Một đôi tất cùng màu là trường hợp đặc biệt: đó là một chu kỳ có chiều dài bằng một, được biểu thị bằng một vòng tự vòng. 

Chiếc chậu ma thuật chỉ chấp nhận một bộ sưu tập các cặp khi mỗi màu bên trong bộ sưu tập đó xuất hiện với số lần chẵn. Trong thuật ngữ đồ thị, các cạnh được chọn phải tạo cho mỗi đỉnh một bậc chẵn. Tập hợp như vậy là một đồ thị con Euler. 

Nhiệm vụ là tìm dung tích nhỏ nhất có thể có của lưu vực, trong đó dung tích nghĩa là số lượng cặp ban đầu tối đa được sử dụng trong một thao tác ma thuật. 

Số lượng cặp có thể đạt tới$10^5$. Một thuật toán thử nhiều tập hợp con có thể có hoặc mô phỏng các nhóm tùy ý sẽ nhanh chóng trở nên không thể thực hiện được vì số lượng các lựa chọn cạnh có thể tăng lên theo cấp số nhân. Ngay cả các thuật toán liên tục quét toàn bộ biểu đồ để tìm mọi nhóm có thể cũng sẽ quá tốn kém. Cấu trúc của biểu đồ phải được sử dụng sao cho mỗi cặp chỉ được xử lý với số lần không đổi. 

Phần khó khăn là nhận ra rằng các chu trình không thể được chia thành các nhóm hợp lệ nhỏ hơn. Ví dụ: một chu trình có ba cạnh:```
1 2
2 3
3 1
```có câu trả lời`3`. Cố gắng chỉ sử dụng hai trong số các cạnh này sẽ khiến một trong các màu có số lượng lẻ, do đó chiếc chậu sẽ không thành công. 

Một trường hợp cạnh khác là tự lặp. đầu vào```
1
1
1 1
```có câu trả lời`1`. Việc triển khai biểu đồ bất cẩn có thể bỏ qua các vòng tự lặp vì chúng không kết nối hai đỉnh khác nhau, nhưng một cặp màu giống hệt nhau đã tự tạo thành một nhóm hợp lệ. 

Một trường hợp nữa là nhiều chu kỳ tồn tại độc lập. Ví dụ:```
1
4
1 2
2 1
3 4
4 3
```có câu trả lời`2`. Hai chu trình cạnh song song có thể được xử lý riêng biệt. Coi toàn bộ biểu đồ là một chu kỳ sẽ trả về không chính xác`4`. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực trực tiếp sẽ cố gắng tìm các nhóm cặp có màu sắc xuất hiện với số lần chẵn. Nó có thể tạo ra các tập hợp con có thể có của các cạnh, kiểm tra xem mỗi màu có xuất hiện với số lần chẵn hay không, sau đó tìm kiếm phân vùng giảm thiểu kích thước tập hợp con lớn nhất. Điều này đúng vì nó xem xét mọi cách có thể để sử dụng lưu vực. Tuy nhiên, ngay cả việc kiểm tra tất cả các tập hợp con cũng đã yêu cầu$2^n$khả năng. Với$n=10^5$, điều này vượt xa những gì bất kỳ triển khai nào có thể xử lý. 

Quan sát hữu ích đến từ biểu diễn đồ thị. Vì mọi đỉnh đều có bậc hai nên mọi thành phần liên thông đều là một chu trình. Một chu trình có một thuộc tính đặc biệt: đồ thị con Euler không trống duy nhất được tạo từ các cạnh của nó chính là toàn bộ chu trình. Nếu chúng ta loại bỏ ngay cả một cạnh khỏi một chu trình, thì hai điểm cuối của cạnh bị loại bỏ đó sẽ trở thành điểm cuối của đường dẫn và các đỉnh đó có bậc lẻ bên trong các cạnh được chọn. 

Điều này có nghĩa là mỗi chu kỳ phải được đặt vào một hoạt động ma thuật tổng thể. Không có lợi ích gì khi cố gắng kết hợp các chu trình vì việc kết hợp hai chu trình chỉ làm tăng công suất cần thiết cho hoạt động đó. Do đó, dung lượng nhỏ nhất có thể là kích thước của chu kỳ lớn nhất trong biểu đồ. 

Giải pháp tối ưu đơn giản là tìm tất cả các thành phần được kết nối và ghi lại số cạnh lớn nhất trong bất kỳ thành phần nào. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^n) | O(n) | Quá chậm | 
| Tối ưu | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng biểu đồ từ các cặp tất. Mỗi màu là một đỉnh và mỗi cặp đóng góp một cạnh. Các vòng lặp tự được lưu trữ dưới dạng các cạnh bình thường vì một cặp cùng màu đã là một chu trình hoàn chỉnh. 
2. Duyệt qua mọi màu chưa được xem bằng cách sử dụng tìm kiếm theo chiều sâu. Trong quá trình truyền tải, hãy đếm xem có bao nhiêu cạnh thuộc về thành phần được kết nối đó. Bởi vì đồ thị chỉ được tạo thành từ các chu trình nên số đếm này chính xác là số cặp phải được đặt cùng nhau. 
3. Giữ số cạnh thành phần tối đa được tìm thấy trong tất cả các lần truyền tải. Giá trị đó là dung tích lưu vực tối thiểu có thể. 
4. Xuất kích thước thành phần tối đa. 

Tại sao nó hoạt động: 

Mọi thành phần liên thông là một chu trình vì mọi đỉnh đều có bậc hai. Một chu trình không thể được tách thành các phép toán ma thuật hợp lệ nhỏ hơn, vì việc loại bỏ bất kỳ cạnh nào sẽ tạo ra các đỉnh có bậc lẻ. Do đó, mỗi thành phần đóng góp một giới hạn dưới không thể tránh khỏi bằng số cạnh của nó. Việc xử lý từng thành phần riêng biệt sẽ đạt được giới hạn đó, do đó kích thước thành phần lớn nhất vừa cần thiết vừa đủ. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    t = int(input())
    ans = []

    for _ in range(t):
        n = int(input())
        graph = {}

        for _ in range(n):
            a, b = map(int, input().split())
            if a not in graph:
                graph[a] = []
            if b not in graph:
                graph[b] = []
            graph[a].append(b)
            graph[b].append(a)

        visited = set()
        best = 0

        for start in graph:
            if start in visited:
                continue

            stack = [start]
            visited.add(start)
            vertices = 0

            while stack:
                u = stack.pop()
                vertices += 1
                for v in graph[u]:
                    if v not in visited:
                        visited.add(v)
                        stack.append(v)

            best = max(best, vertices)

        ans.append(str(best))

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Biểu đồ được lưu dưới dạng danh sách kề. Khi một cạnh được thêm vào, nó sẽ được thêm theo cả hai hướng, kể cả trường hợp tự lặp. Đối với một vòng tự lặp, cùng một đỉnh nhận được hai mục, đây chính xác là mức đóng góp của nó. 

Tìm kiếm theo chiều sâu sẽ đếm các đỉnh trong thành phần được kết nối. Trong họ đồ thị này, mọi thành phần đều là một chu trình nên số đỉnh bằng số cạnh. Điều này cũng xử lý thành phần tự lặp một cách chính xác vì nó chứa một đỉnh và một cạnh. 

Việc duyệt đánh dấu các đỉnh ngay lập tức khi chúng được đẩy vào ngăn xếp. Điều này tránh việc truy cập cùng một chu kỳ nhiều lần và giữ cho tổng công việc được tuyến tính. 

## Ví dụ đã hoạt động 

Hãy xem xét đầu vào này:```
1
3
1 2
2 3
3 1
```Quá trình truyền tải bắt đầu ở màu sắc`1`. 

| Đỉnh bắt đầu | Các đỉnh đã truy cập | Kích thước thành phần hiện tại | Trả lời | 
| --- | --- | --- | --- | 
| 1 | {1} | 1 | 0 | 
| 2 | {1,2} | 2 | 0 | 
| 3 | {1,2,3} | 3 | 3 | 

Ba màu tạo thành một chu kỳ. Toàn bộ chu trình phải được xử lý cùng nhau nên công suất yêu cầu là`3`. 

Một ví dụ khác:```
1
5
1 2
2 1
3 4
4 3
5 5
```Biểu đồ chứa ba thành phần riêng biệt. 

| Thành phần | Các đỉnh được phát hiện | Kích thước thành phần | Câu trả lời hiện tại | 
| --- | --- | --- | --- | 
| Chu kỳ đầu tiên | {1,2} | 2 | 2 | 
| Chu kỳ thứ hai | {3,4} | 2 | 2 | 
| Tự lặp | {5} | 1 | 2 | 

Chu kỳ lớn nhất chứa hai cặp nên dung lượng tối thiểu là`2`. Vòng lặp tự đã là hoạt động một cặp hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi cặp tất tạo ra một cạnh và mỗi đỉnh và cạnh đều được truy cập với số lần không đổi. | 
| Không gian | O(n) | Danh sách kề lưu trữ tất cả các kết nối đồ thị và quá trình truyền tải lưu trữ các trạng thái đã truy cập. | 

Kích thước đầu vào tối đa là$10^5$cặp, do đó, một giải pháp tuyến tính dễ dàng phù hợp với giới hạn lập trình cạnh tranh điển hình. 

## Trường hợp thử nghiệm```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    solve()

    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

# provided sample
assert run("""1
5
1 2
2 3
1 3
4 5
4 5
""") == "3\n", "sample"

# single self-loop
assert run("""1
1
7 7
""") == "1\n", "self loop"

# two separate cycles
assert run("""1
4
1 2
2 1
3 4
4 3
""") == "2\n", "multiple components"

# one large cycle
assert run("""1
6
1 2
2 3
3 4
4 5
5 6
6 1
""") == "6\n", "large cycle"

# repeated two-color cycle
assert run("""1
2
10 20
10 20
""") == "2\n", "parallel edges"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Trường hợp mẫu | 3 | Một chu kỳ bình thường trộn lẫn với thành phần khác | 
| Một vòng tự | 1 | Các cặp cùng màu được xử lý chính xác | 
| Hai chu kỳ riêng biệt | 2 | Các thành phần phải được xem xét độc lập | 
| Một chu kỳ sáu nút | 6 | Toàn bộ chu kỳ không thể được chia nhỏ | 
| Hai cạnh song song | 2 | Chiều dài hai chu kỳ được tính chính xác | 

## Vỏ cạnh 

Đối với một vòng lặp tự duy nhất:```
1
1
7 7
```Đồ thị có một đỉnh với một vòng tự lặp. Danh sách kề chứa hai tham chiếu đến đỉnh`7`và quá trình duyệt sẽ tìm thấy một đỉnh trong thành phần. Vì một vòng tự đại diện cho một cặp nên kích thước thành phần là`1`, vậy câu trả lời là`1`. 

Đối với một chu kỳ có thể chia được:```
1
3
1 2
2 3
3 1
```Một cách tiếp cận tham lam có thể cố gắng lấy hai cặp trước, nhưng hai cạnh đó để lại một màu xuất hiện một lần. Quá trình duyệt nhìn thấy một thành phần liên thông chứa cả ba đỉnh, đưa ra câu trả lời cần thiết`3`. 

Đối với nhiều thành phần bị ngắt kết nối:```
1
4
1 2
2 1
3 4
4 3
```Thành phần đầu tiên chứa màu sắc`1`Và`2`, và cái thứ hai chứa màu sắc`3`Và`4`. Mỗi thành phần có thể được xử lý độc lập với dung lượng`2`. Thuật toán không bao giờ hợp nhất chúng, vì vậy nó không đánh giá quá cao câu trả lời vì`4`.
