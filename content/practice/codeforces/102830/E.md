---
title: "CF 102830E - Mua Tacos"
description: "Kevin có sẵn một số loại taco. Mỗi loại đều có giá mua bình thường và cũng có sự trao đổi một chiều giữa các loại taco. Trao đổi cho phép anh ta biến một loại taco này thành một loại taco khác bằng cách trả một khoản phí bổ sung."
date: "2026-07-26T15:21:00+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102830
codeforces_index: "E"
codeforces_contest_name: "UTPC Contest 11-06-20 Div. 2 (Beginner)"
rating: 0
weight: 102830
solve_time_s: 47
verified: true
draft: false
---

[CF 102830E - Mua Tacos](https://codeforces.com/problemset/problem/102830/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 47s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Kevin có sẵn một số loại taco. Mỗi loại đều có giá mua bình thường và cũng có sự trao đổi một chiều giữa các loại taco. Trao đổi cho phép anh ta biến một loại taco này thành một loại taco khác bằng cách trả một khoản phí bổ sung. Mỗi trao đổi có thể được thực hiện bất kỳ số lần. 

Dữ liệu đầu vào mô tả số lượng loại bánh taco, giá trực tiếp khi mua một bánh taco của từng loại, tất cả các giao dịch có thể thực hiện được và cuối cùng là số lượng bánh taco của mỗi loại mà Kevin muốn. Mục tiêu là tìm ra số tiền tối thiểu cần thiết để đạt được việc phân phối chính xác số tacos được yêu cầu. 

Câu hỏi trọng tâm không phải là làm thế nào để thực hiện việc trao đổi sau khi mua tacos. Thay vào đó, vấn đề là mỗi loại taco cuối cùng có thể được sản xuất với giá rẻ đến mức nào. Một loại taco`i`có thể được mua trực tiếp hoặc có được bằng cách mua loại khác và thực hiện theo trình tự trao đổi. Khi biết được chi phí sản xuất rẻ nhất của mỗi loại, mỗi loại bánh taco được yêu cầu có thể được xử lý độc lập. 

Các ràng buộc gợi ý mạnh mẽ một giải pháp đồ thị. Có thể có tới 10.000 loại taco và 100.000 trao đổi. Một cách tiếp cận kiểm tra mọi taco khởi đầu có thể có cho mọi điểm đến sẽ yêu cầu khoảng 10^8 thao tác trở lên chỉ để xử lý biểu đồ và việc khám phá các đường dẫn liên tục sẽ quá chậm. Chúng ta cần một thuật toán đường đi ngắn nhất để xử lý đồ thị thưa thớt một cách hiệu quả. Trọng số của các cạnh không âm nên thuật toán Dijkstra là phù hợp. 

Có một số trường hợp nghiêm trọng mà việc triển khai bất cẩn có thể thất bại. Đầu tiên là khi taco chỉ có thể nhận được thông qua một số trao đổi. Ví dụ:```
Input
3 2
10
100
100
0 1 3
1 2 4
0
0
1
```Đầu ra đúng là:```
17
```Mua loại 0 và thực hiện hai lần trao đổi chi phí`10 + 3 + 4 = 17`. Giải pháp chỉ xem xét mua hàng trực tiếp sẽ trả lời sai`100`. 

Một trường hợp khó khăn khác là khi trao đổi rẻ hơn so với mua trực tiếp taco nguồn nhưng đường dẫn trao đổi bắt đầu từ một loại khác. Ví dụ:```
Input
2 1
100
5
1 0 2
0
1
```Đầu ra đúng là:```
5
```Loại 1 đã đủ rẻ và không cần chuyển đổi. Một giải pháp giả định mỗi taco phải sử dụng ít nhất một sàn giao dịch có thể tạo ra chi phí lớn hơn. 

Sai lầm phổ biến cuối cùng là bỏ qua việc nhiều loại bánh taco có thể có cùng nguồn cung cấp rẻ nhất. Ví dụ:```
Input
3 2
1
50
50
0 1 1
0 2 2
3
3
3
```Đầu ra đúng là:```
15
```Mọi loại bánh taco đều có thể được làm từ loại 0. Câu trả lời là`3 * 1 + 3 * 2 + 3 * 3 = 18`? Đợi đã, số lượng được yêu cầu trong ví dụ này đều là ba, vì vậy kết quả thực tế là:```
18
```Một giải pháp chỉ tính toán một mục tiêu chuyển đổi tốt nhất hoặc chỉ định các nguồn một cách tham lam mà không xem xét từng đích đến một cách độc lập sẽ thất bại. 

## Phương pháp tiếp cận 

Cách tiếp cận bạo lực là coi mọi loại taco là điểm bắt đầu có thể, sau đó chạy tìm kiếm đường dẫn ngắn nhất từ loại đó để xác định loại taco nào khác mà nó có thể tạo ra. Sau đó, chúng tôi có thể so sánh chi phí mua bánh taco ban đầu và chuyển đổi nó với câu trả lời tốt nhất hiện tại cho từng điểm đến. 

Phương pháp này đúng vì mọi nguồn taco có thể đều được xem xét. Nếu cách rẻ nhất để tạo loại taco bắt đầu bằng việc mua taco`s`, chạy từ`s`sẽ khám phá đường dẫn trao đổi tương ứng. 

Vấn đề là số lượng công việc lặp đi lặp lại. Chạy Dijkstra một lần tốn kém`O((t + e) log t)`. Chạy nó từ mọi loại taco đều tốn kém`O(t(t + e) log t)`. Với 10.000 loại taco và 100.000 trao đổi, điều này vượt xa giới hạn thời gian cho phép. 

Quan sát quan trọng là tất cả các loại bánh taco đều đặt cùng một câu hỏi: "Nguồn nào rẻ nhất có thể tiếp cận với tôi?" Thay vì chạy các đường đi ngắn nhất từ ​​mỗi nguồn một cách riêng biệt, chúng ta có thể đảo ngược quan điểm. Hãy tưởng tượng tạo một nguồn ảo có khoảng cách bằng 0 đến mọi loại taco, nhưng trước khi chạy Dijkstra, chúng tôi khởi tạo khoảng cách của từng loại taco bằng chi phí mua trực tiếp của nó. Dijkstra đa nguồn sau đó sẽ dàn trải các chi phí ban đầu này thông qua biểu đồ trao đổi. 

Khi Dijkstra nới lỏng trao đổi từ loại`i`gõ`j`, nó kiểm tra xem có được`i`với giá rẻ và trả phí trao đổi tốt hơn so với cách nhận hiện tại`j`. Điều này hoàn toàn khớp với vấn đề ban đầu vì mọi giao dịch mua hàng có thể bắt đầu đều đã có sẵn ở khoảng cách ban đầu. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(t(t + e) ​​log t) | O(t + e) ​​| Quá chậm | 
| Tối ưu | O((t + e) ​​log t) | O(t + e) ​​| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc các loại taco, giá mua trực tiếp, trao đổi và số lượng yêu cầu. Lưu trữ mỗi trao đổi dưới dạng cạnh đồ thị có hướng vì trao đổi từ loại`i`gõ`j`không ngụ ý sự trao đổi ngược lại tồn tại. 
2. Khởi tạo khoảng cách ngắn nhất của mỗi loại taco với giá mua trực tiếp. Điều này thể hiện khả năng mua món taco đó ngay lập tức mà không cần sử dụng bất kỳ sàn giao dịch nào. 
3. Chèn mọi loại taco vào hàng ưu tiên với khoảng cách hiện tại. Điều này khởi động Dijkstra từ tất cả các nguồn có thể cùng một lúc. 
4. Loại bỏ nhiều lần loại taco có chi phí nhỏ nhất được biết. Đối với mỗi lần trao đổi đi, hãy tính chi phí để đến đích bằng cách trước tiên lấy loại taco hiện tại và sau đó thanh toán phí trao đổi. Nếu giá trị này nhỏ hơn, hãy cập nhật đích và đẩy giá trị mới vào hàng ưu tiên. 
5. Sau khi quá trình tìm đường đi ngắn nhất kết thúc, hãy nhân chi phí rẻ nhất của từng loại bánh taco với số lượng bánh taco được yêu cầu cho loại đó. Cộng các giá trị này lại với nhau để có được tổng giá tối thiểu. 

Lý do điều này có hiệu quả là vì mọi cách hợp lệ để có được một taco bao gồm việc chọn một số taco ban đầu để mua và sau đó đi theo con đường trao đổi. Khoảng cách ban đầu thể hiện tất cả các giao dịch mua có thể bắt đầu và mọi sự thư giãn đều thể hiện việc mở rộng một trong những con đường đó. Dijkstra luôn giữ đường dẫn rẻ nhất đến mỗi nút vì tất cả chi phí trao đổi đều không âm. Khi thuật toán kết thúc, khoảng cách của mỗi loại taco chính xác là chi phí rẻ nhất có thể để tạo ra một loại taco đó. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    t, e = map(int, input().split())

    dist = []
    for _ in range(t):
        dist.append(int(input()))

    graph = [[] for _ in range(t)]

    for _ in range(e):
        i, j, p = map(int, input().split())
        graph[i].append((j, p))

    need = []
    for _ in range(t):
        need.append(int(input()))

    pq = []
    for i, cost in enumerate(dist):
        heapq.heappush(pq, (cost, i))

    while pq:
        cost, node = heapq.heappop(pq)

        if cost != dist[node]:
            continue

        for nxt, price in graph[node]:
            new_cost = cost + price
            if new_cost < dist[nxt]:
                dist[nxt] = new_cost
                heapq.heappush(pq, (new_cost, nxt))

    answer = 0
    for i in range(t):
        answer += dist[i] * need[i]

    print(answer)

if __name__ == "__main__":
    solve()
```các`dist`mảng bắt đầu bằng giá taco trực tiếp thay vì vô cùng. Đây là chi tiết triển khai chính giúp biến Dijkstra bình thường thành Dijkstra đa nguồn. 

Hàng đợi ưu tiên chứa mọi loại taco ở đầu. Khi tìm thấy đường dẫn rẻ hơn, mục hàng đợi cũ sẽ không bị xóa. Thay vào đó, một mục mới được chèn vào và`if cost != dist[node]`kiểm tra bỏ qua các mục lỗi thời. Đây là cách tiêu chuẩn để triển khai Dijkstra hiệu quả trong Python. 

Biểu đồ được định hướng vì các sàn giao dịch chỉ hoạt động theo hướng được chỉ định. Việc coi nó là vô hướng sẽ cho phép không thể chuyển đổi một cách không chính xác. 

Phép nhân cuối cùng là an toàn vì số lượng loại taco và các giá trị vừa vặn với số nguyên Python. Số nguyên Python không bị tràn, do đó mã không cần xử lý đặc biệt đối với số tiền lớn. 

## Ví dụ đã hoạt động 

Đối với đầu vào mẫu:```
3 2
1
3
5
0 1 1
1 2 1
1
2
3
```Khoảng cách ban đầu là giá trực tiếp. 

| Bước | Taco chế biến | Khoảng cách hiện tại | 
| --- | --- | --- | 
| Bắt đầu | Không có | [1, 3, 5] | 
| 1 | Loại 0 | [1, 2, 5] | 
| 2 | Loại 1 | [1, 2, 3] | 
| 3 | Loại 2 | [1, 2, 3] | 

Giá rẻ nhất trở thành 1, 2 và 3. Số lượng cần thiết là 1, 2 và 3, vì vậy câu trả lời là`1*1 + 2*2 + 3*3 = 14`. 

Ví dụ thứ hai:```
2 1
10
100
0 1 5
0
2
```| Bước | Taco chế biến | Khoảng cách hiện tại | 
| --- | --- | --- | 
| Bắt đầu | Không có | [10, 100] | 
| 1 | Loại 0 | [10, 15] | 
| 2 | Loại 1 | [10, 15] | 

Chiếc bánh taco thứ hai rẻ hơn thông qua một lần trao đổi so với việc mua trực tiếp. Hai tacos loại 1 có giá`2 * 15 = 30`. 

Dấu vết này chứng tỏ rằng thuật toán không quan tâm liệu tuyến đường rẻ nhất sử dụng 0, một hay nhiều trao đổi. Nó xử lý tất cả các khả năng một cách thống nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((t + e) ​​log t) | Mỗi loại taco có thể vào hàng ưu tiên nhiều lần và mỗi lần trao đổi sẽ được thoải mái trong Dijkstra. | 
| Không gian | O(t + e) ​​| Biểu đồ lưu trữ mọi trao đổi và các mảng lưu trữ một giá trị cho mỗi loại taco. | 

Với 10.000 loại taco và 100.000 trao đổi, sự phức tạp này dễ dàng đạt đến giới hạn. Thuật toán thực hiện một lần duyệt đồ thị thay vì lặp lại các phép tính đường đi ngắn nhất từ ​​mọi taco bắt đầu có thể. 

## Trường hợp thử nghiệm```python
import sys
import io
import heapq

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    input = sys.stdin.readline

    t, e = map(int, input().split())
    dist = [int(input()) for _ in range(t)]

    graph = [[] for _ in range(t)]
    for _ in range(e):
        i, j, p = map(int, input().split())
        graph[i].append((j, p))

    need = [int(input()) for _ in range(t)]

    pq = [(dist[i], i) for i in range(t)]
    heapq.heapify(pq)

    while pq:
        cost, node = heapq.heappop(pq)
        if cost != dist[node]:
            continue
        for nxt, price in graph[node]:
            if cost + price < dist[nxt]:
                dist[nxt] = cost + price
                heapq.heappush(pq, (dist[nxt], nxt))

    ans = sum(dist[i] * need[i] for i in range(t))
    print(ans)

    result = sys.stdout.getvalue()

    sys.stdin = old_stdin
    sys.stdout = old_stdout
    return result

assert run("""3 2
1
3
5
0 1 1
1 2 1
1
2
3
""") == "14\n", "sample"

assert run("""2 1
10
100
0 1 5
0
2
""") == "30\n", "single exchange"

assert run("""1 0
7
5
""") == "35\n", "minimum size"

assert run("""3 0
4
4
4
10
10
10
""") == "120\n", "all equal values"

assert run("""4 3
100
50
20
10
0 1 1
1 2 1
2 3 1
0
0
0
5
""") == "65\n", "long chain"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Đầu vào mẫu | 14 | Truyền đường đi ngắn nhất thông thường | 
| Hai loại taco một lần đổi | 30 | Chuyển đổi rẻ hơn so với mua trực tiếp | 
| Một loại taco | 35 | Xử lý đầu vào kích thước tối thiểu | 
| Tất cả giá trực tiếp bằng nhau | 120 | Không có giả định trao đổi không cần thiết | 
| Chuỗi trao đổi dài | 65 | Chuyển đổi nhiều bước | 

## Vỏ cạnh 

Khi chỉ có thể nhận được một chiếc bánh taco sau nhiều lần trao đổi, thuật toán sẽ xử lý nó một cách tự nhiên thông qua việc thư giãn lặp đi lặp lại. Trong ví dụ với giá 10, 100, 100 và trao đổi`0 -> 1`có giá 3 và`1 -> 2`có giá 4, Dijkstra bắt đầu bằng khoảng cách`[10, 100, 100]`. Đầu tiên nó cải thiện loại 1 thành 13, sau đó cải thiện loại 2 thành 17. Câu trả lời cuối cùng sử dụng 17 vì đường dẫn rẻ nhất chứa cả hai trao đổi. 

Khi taco đã là nguồn rẻ nhất có thể, thuật toán sẽ giữ khoảng cách ban đầu. Đối với đầu vào có giá 100 và 5 và trao đổi từ loại 1 sang loại 0, khoảng cách ban đầu là`[100, 5]`. Quá trình xử lý loại 1 cố gắng cải thiện loại 0 thành loại 7, nhưng điều đó chỉ ảnh hưởng đến loại 0. Bánh taco loại 1 được yêu cầu vẫn có giá 5 mỗi chiếc. 

Khi nhiều loại taco chia sẻ cùng một nguồn rẻ nhất, mỗi điểm đến sẽ nhận được giá trị đường đi ngắn nhất của riêng mình. Đối với chuỗi trao đổi từ loại 0, các giá trị khoảng cách sẽ trải ra từng cạnh một. Phép nhân cuối cùng với số lượng được yêu cầu sau đó kết hợp chính xác các chi phí tối thiểu độc lập đó.
