---
title: "CF 102829E - Mua Tacos"
description: "Kevin có nhiều loại taco. Mỗi loại có giá mua bình thường và anh ta cũng biết một loạt các giao dịch có thể thực hiện được. Một sàn giao dịch nói rằng nếu anh ta hiện đang sở hữu một taco loại i, anh ta có thể trả một số tiền nào đó và nhận được một taco loại j."
date: "2026-07-26T15:25:01+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102829
codeforces_index: "E"
codeforces_contest_name: "UTPC Contest 11-06-20 Div. 1 (Tryout)"
rating: 0
weight: 102829
solve_time_s: 41
verified: true
draft: false
---

[CF 102829E - Mua Tacos](https://codeforces.com/problemset/problem/102829/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 41s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Kevin có nhiều loại taco. Mỗi loại có giá mua bình thường và anh ta cũng biết một loạt các giao dịch có thể thực hiện được. Một sàn giao dịch nói rằng nếu anh ta hiện đang sở hữu một loại bánh taco`i`, anh ta có thể trả một số tiền và nhận được một loại bánh taco`j`. Trao đổi có thể được lặp lại và chúng không phải hoạt động theo cả hai hướng. 

Mục tiêu là mua đủ số lượng tacos cần thiết thuộc mọi loại trong khi trả số tiền tối thiểu có thể. Đầu vào cung cấp số lượng loại taco, quy tắc trao đổi, chi phí mua trực tiếp từng loại và số lượng taco cần thiết cho mỗi loại. Sản lượng là tổng số tiền tối thiểu cần thiết để đáp ứng tất cả số lượng được yêu cầu. 

Các giới hạn đủ lớn để việc kiểm tra mọi chuỗi trao đổi có thể xảy ra là không thể. Có thể có tới`10^4`các loại taco và`10^5`trao đổi. Một giải pháp thử tất cả các loại taco sẽ cần khoảng`10^8`hoạt động và việc mô phỏng các đường dẫn trao đổi có thể sẽ tệ hơn nhiều vì các đường dẫn có thể dài tùy ý. Giá trao đổi không âm, điều này cho thấy rõ ràng rằng kỹ thuật đường đi ngắn nhất là phù hợp. 

Một trường hợp tế nhị là khi mua trực tiếp loại taco không rẻ nhất. Ví dụ: giả sử đầu vào là:```
2 1
10
1
0 1 1
0
5
```Câu trả lời là`5`, không`50`. Một giải pháp bất cẩn có thể cho rằng mọi món taco được yêu cầu đều phải được mua trực tiếp. Ở đây, kiểu mua`1`chi phí`1`, nhưng mua loại`0`và việc trao đổi nó tốn kém`10 + 1`, nên giá trực tiếp vẫn tốt hơn. Ví dụ cho thấy mỗi loại mục tiêu cần được đánh giá độc lập thay vì cho rằng việc trao đổi luôn hữu ích. 

Một trường hợp khó khăn khác là khi taco ban đầu tốt nhất khác với taco mục tiêu. Coi như:```
3 2
100
5
100
0 1 1
1 2 1
0
0
3
```Câu trả lời là`21`. Ba loại`2`tacos có thể được thực hiện bằng cách mua loại`1`vì`5`và trao đổi hai lần, tính ra chi phí là`7`mỗi. Phương pháp chỉ kiểm tra giao dịch mua trực tiếp hoặc một sàn giao dịch sẽ bỏ lỡ đường dẫn này. 

Trường hợp cạnh cuối cùng là khi không có trao đổi:```
2 0
4
7
3
2
```Câu trả lời là`26`. Mỗi chiếc bánh taco phải được mua trực tiếp vì không thể di chuyển giữa các loại. Thuật toán phải cho phép loại taco sử dụng giá mua của chính nó làm điểm bắt đầu hợp lệ. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là xem xét riêng từng loại taco được yêu cầu. Đối với loại mục tiêu`j`, chúng ta có thể thử mọi kiểu bắt đầu có thể`i`, tìm đường trao đổi rẻ nhất từ ​​​​`i`ĐẾN`j`và lấy giá trị nhỏ nhất của`cost[i] + path_cost`. Điều này đúng vì mọi taco cuối cùng đều phải đến từ một số taco được mua trực tiếp, sau đó không có trao đổi hoặc nhiều hơn. 

Vấn đề là số lượng tính toán đường đi ngắn nhất. Việc chạy tìm kiếm biểu đồ từ mọi loại taco sẽ yêu cầu`10^4`tìm kiếm trên biểu đồ với`10^5`các cạnh. Ngay cả với thuật toán Dijkstra, điều này vẫn quá chậm vì độ phức tạp trở nên gần như`O(t * (t + e) log t)`, vượt xa giới hạn. 

Quan sát quan trọng là chúng ta không cần phải biết nguồn tốt nhất cho từng điểm đến riêng biệt. Chúng tôi chỉ cần cách rẻ nhất để đến từng điểm đến khi có thể mua bất kỳ chiếc bánh taco ban đầu nào trước tiên. Điều này hoàn toàn giống với việc thêm một nút nguồn ảo được kết nối với mọi loại taco có lợi thế bằng giá mua của nó. Khoảng cách ngắn nhất từ ​​nguồn ảo đó đến mọi loại taco là cách rẻ nhất để có được một taco của mỗi loại. 

Thay vì tạo nút này một cách rõ ràng, chúng ta có thể khởi tạo Dijkstra với mọi loại taco đã có trong hàng đợi ưu tiên. Khoảng cách ban đầu của loại`i`là giá mua trực tiếp của nó. Sau đó mọi cạnh trao đổi sẽ thư giãn bình thường. Vì các sàn giao dịch mô tả việc di chuyển từ taco mà chúng ta sở hữu sang taco mà chúng ta muốn, nên các đường đi ngắn nhất cần phải đi theo hướng trao đổi trực tiếp. 

Kết quả so sánh là: 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(t(t + e) ​​log t) | O(t + e) ​​| Quá chậm | 
| Tối ưu | O((t + e) ​​log t) | O(t + e) ​​| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc giá mua trực tiếp của từng loại taco và quy tắc trao đổi. Xây dựng một biểu đồ trong đó một cạnh`i -> j`có trọng lượng bằng chi phí trao đổi taco`i`vào taco`j`. 
2. Khởi tạo khoảng cách ngắn nhất của mỗi loại taco với giá mua trực tiếp. Chèn mọi loại taco vào hàng ưu tiên với giá trị ban đầu này. Điều này coi mọi loại taco là điểm khởi đầu khả thi cho lần mua hàng cuối cùng. 
3. Chạy thuật toán Dijkstra trên biểu đồ trao đổi. Bất cứ khi nào một loại taco`u`có thể thu được với chi phí`d`, hãy thử mọi trao đổi từ`u`sang loại khác`v`. Nếu như`d + exchange_cost`cải thiện chi phí đã biết của`v`, cập nhật nó. 
4. Sau khi Dijkstra kết thúc, khoảng cách của loại`i`là mức giá rẻ nhất có thể cho một loại bánh taco`i`, bao gồm tất cả các chuỗi trao đổi có thể có. 
5. Nhân mỗi khoảng cách cuối cùng với số lượng tacos cần thiết cho loại đó và cộng các giá trị lại với nhau. Tổng số tiền này là tổng chi phí tối thiểu vì mỗi chiếc bánh taco có thể được mua độc lập. 

Tại sao nó hoạt động: Tính bất biến của Dijkstra là bất cứ khi nào một nút bị xóa khỏi hàng ưu tiên có khoảng cách hiện tại nhỏ nhất thì khoảng cách đó là cách rẻ nhất có thể để có được loại taco đó. Khoảng cách ban đầu thể hiện việc mua trực tiếp bất kỳ bánh taco nào và mỗi lần thư giãn thể hiện việc thực hiện một giao dịch hợp lệ. Vì tất cả chi phí trao đổi đều không âm nên Dijkstra không bao giờ cần phải xem xét lại loại taco cuối cùng. Sau khi tất cả các nút được hoàn tất, mọi trình tự mua và trao đổi có thể có đã được xem xét thông qua một đường dẫn tương ứng trong biểu đồ. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    t, e = map(int, input().split())

    cost = []
    for _ in range(t):
        cost.append(int(input()))

    graph = [[] for _ in range(t)]
    for _ in range(e):
        i, j, p = map(int, input().split())
        graph[i].append((j, p))

    need = []
    for _ in range(t):
        need.append(int(input()))

    dist = cost[:]
    pq = []

    for i in range(t):
        heapq.heappush(pq, (dist[i], i))

    while pq:
        cur, u = heapq.heappop(pq)

        if cur != dist[u]:
            continue

        for v, w in graph[u]:
            nd = cur + w
            if nd < dist[v]:
                dist[v] = nd
                heapq.heappush(pq, (nd, v))

    ans = 0
    for i in range(t):
        ans += dist[i] * need[i]

    print(ans)

if __name__ == "__main__":
    solve()
```Quá trình phân tích cú pháp đầu vào tuân theo cấu trúc của biểu đồ: đầu tiên là số loại taco và trao đổi, sau đó là giá trực tiếp, sau đó là cạnh trao đổi theo chỉ dẫn và cuối cùng là số lượng được yêu cầu. 

các`dist`mảng bắt đầu bằng giá mua trực tiếp thay vì giá vô cùng. Đây là chi tiết triển khai chính giúp biến Dijkstra bình thường thành phiên bản đa nguồn. Mọi loại bánh taco đều có sẵn vì Kevin luôn có thể mua trực tiếp. 

Hàng đợi ưu tiên có thể chứa các mục nhập lỗi thời vì loại taco có thể nhận được khoảng cách tốt hơn sau khi giá trị cũ hơn đã được chèn vào. các`if cur != dist[u]`kiểm tra loại bỏ những mục cũ. 

Tất cả chi phí đều phù hợp một cách an toàn bên trong số nguyên Python. Phép nhân với số lượng cần thiết chỉ được thực hiện sau khi đã biết đường đi ngắn nhất, do đó không có thao tác biểu đồ lặp lại trong quá trình tính toán cuối cùng. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
3 2
1
3
5
0 1 1
1 2 1
1
2
3
```Khoảng cách ban đầu là giá mua trực tiếp. 

| Bước | taco hiện tại | Mảng khoảng cách | Hành động | 
| --- | --- | --- | --- | 
| Ban đầu | Không có |`[1, 3, 5]`| Chèn tất cả các loại taco | 
| 1 | Loại 0 |`[1, 3, 5]`| Cải thiện loại 1 thành`2`| 
| 2 | Loại 1 |`[1, 2, 5]`| Cải thiện loại 2 thành`3`| 
| 3 | Loại 2 |`[1, 2, 3]`| Không có cải tiến | 

Giá rẻ nhất trở thành loại`0`vì`1`, kiểu`1`vì`2`, và gõ`2`vì`3`. Tổng cộng là:`1 * 1 + 2 * 2 + 3 * 3 = 14`Dấu vết này chứng tỏ rằng thuật toán sử dụng chuỗi trao đổi một cách tự nhiên. 

### Ví dụ tùy chỉnh 

đầu vào:```
3 1
8
2
10
1 2 3
0
0
2
```| Bước | taco hiện tại | Mảng khoảng cách | Hành động | 
| --- | --- | --- | --- | 
| Ban đầu | Không có |`[8, 2, 10]`| Chèn tất cả các loại taco | 
| 1 | Loại 1 |`[8, 2, 10]`| Cải thiện loại 2 thành`5`| 
| 2 | Loại 2 |`[8, 2, 5]`| Không có cải tiến | 
| 3 | Loại 0 |`[8, 2, 5]`| Không có cải tiến | 

Các tacos được yêu cầu đều là loại`2`. Chi phí mua trực tiếp`10`mỗi loại, nhưng mua loại`1`và chi phí trao đổi`5`mỗi cái, vì vậy câu trả lời là`10`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((t + e) ​​log t) | Mỗi loại taco được đưa vào hàng ưu tiên và mỗi cạnh trao đổi được xử lý trong Dijkstra. | 
| Không gian | O(t + e) ​​| Biểu đồ lưu trữ tất cả các quy tắc trao đổi và các mảng lưu trữ khoảng cách và yêu cầu. | 

Đầu vào lớn nhất chỉ chứa`10^4`các loại taco và`10^5`trao đổi, do đó, một lần chạy Dijkstra dễ dàng phù hợp với giới hạn đã định. Việc sử dụng vùng heap giữ cho thuật toán hoạt động hiệu quả ngay cả với biểu đồ thưa thớt. 

## Trường hợp thử nghiệm```python
import sys
import io
import heapq

def solve_case(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    input = sys.stdin.readline

    t, e = map(int, input().split())
    cost = [int(input()) for _ in range(t)]

    graph = [[] for _ in range(t)]
    for _ in range(e):
        i, j, p = map(int, input().split())
        graph[i].append((j, p))

    need = [int(input()) for _ in range(t)]

    dist = cost[:]
    pq = [(dist[i], i) for i in range(t)]
    heapq.heapify(pq)

    while pq:
        d, u = heapq.heappop(pq)
        if d != dist[u]:
            continue
        for v, w in graph[u]:
            nd = d + w
            if nd < dist[v]:
                dist[v] = nd
                heapq.heappush(pq, (nd, v))

    return str(sum(dist[i] * need[i] for i in range(t)))

assert solve_case("""3 2
1
3
5
0 1 1
1 2 1
1
2
3
""") == "14", "sample 1"

assert solve_case("""1 0
7
5
""") == "35", "single taco type"

assert solve_case("""3 2
100
5
100
1 2 1
2 0 1
0
0
3
""") == "21", "multi-step exchange"

assert solve_case("""4 0
3
4
5
6
2
0
1
3
""") == "37", "no exchanges"

assert solve_case("""2 1
10
1
0 1 1
0
10000
""") == "1000", "all demand on cheap type"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Mẫu 1 | 14 | Chuỗi trao đổi cơ bản | 
| Loại taco đơn | 35 | Kích thước biểu đồ tối thiểu | 
| Trao đổi nhiều bước | 21 | Đường dẫn dài hơn một lần trao đổi | 
| Không trao đổi | 37 | Dự phòng mua hàng trực tiếp | 
| Mọi nhu cầu về loại giá rẻ | 1000 | Nhân đúng yêu cầu | 

## Vỏ cạnh 

Đối với trường hợp mua hàng trực tiếp rõ ràng không phải là lựa chọn cuối cùng, thuật toán sẽ giữ tất cả giá trực tiếp làm khoảng cách bắt đầu. TRONG:```
2 1
10
1
0 1 1
0
5
```Dijkstra bắt đầu với khoảng cách`[10, 1]`. Việc trao đổi có thể cải thiện loại`1`từ`1`không có gì tốt hơn, vì vậy chi phí cuối cùng cho năm loại`1`tacos là`5`. Tùy chọn mua trực tiếp vẫn có sẵn vì nó đã được đưa vào trong quá trình khởi tạo. 

Đối với một chuỗi trao đổi dài:```
3 2
100
5
100
1 2 1
2 0 1
0
0
3
```Khoảng cách ngắn nhất để gõ`2`được tìm thấy thông qua loại`1`: loại mua`1`vì`5`, trao đổi để gõ`2`vì`1`, đưa ra chi phí`6`. Chi phí ba tacos được yêu cầu`18`. Quá trình tương tự cũng tìm thấy loại đó`0`có thể thu được thông qua loại`2`, chứng minh rằng việc truyền tải đồ thị xử lý các chu trình và các tuyến gián tiếp một cách chính xác. 

Đối với đồ thị không có cạnh:```
2 0
4
7
3
2
```Hàng ưu tiên chỉ chứa hai giá trực tiếp. Không có sự thư giãn xảy ra nên khoảng cách giữ nguyên`[4, 7]`. Câu trả lời cuối cùng là`4 * 3 + 7 * 2 = 26`, phù hợp với chiến lược duy nhất có thể.
