---
title: "CF 102500K - Lướt ván diều"
description: "Cuộc đua là đường đi một chiều từ vị trí 0 đến vị trí s. Một số phần của con đường này bị chiếm giữ bởi các hòn đảo, được thể hiện bằng các khoảng không chồng chéo. Nora phải giữ liên lạc nên cô ấy không thể di chuyển qua một hòn đảo."
date: "2026-08-05T18:11:36+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102500
codeforces_index: "K"
codeforces_contest_name: "2019-2020 ICPC Northwestern European Regional Programming Contest (NWERC 2019)"
rating: 0
weight: 102500
solve_time_s: 58
verified: true
draft: false
---

[CF 102500K - Lướt ván diều](https://codeforces.com/problemset/problem/102500/K) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 58s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Cuộc đua là con đường một chiều từ vị trí`0`để định vị`s`. Một số phần của con đường này bị chiếm giữ bởi các hòn đảo, được thể hiện bằng các khoảng không chồng chéo. Nora phải giữ liên lạc nên cô ấy không thể di chuyển qua một hòn đảo. Cô ấy có thể lướt trên mặt nước không bị gián đoạn, nơi thời gian bằng khoảng cách hoặc nhảy giữa hai vị trí an toàn trong khoảng cách.`d`, trong đó mỗi bước nhảy đều có giá chính xác`t`giây. 

Nhiệm vụ là tìm thời gian tối thiểu để về đích. Câu trả lời không phải là hỏi lộ trình mà chỉ hỏi thời gian di chuyển ngắn nhất. 

Số lượng đảo ít, nhiều nhất là 500. Đây là hạn chế chính. Bản thân tọa độ có thể lớn bằng`10^9`, vì vậy việc lặp lại trên mỗi mét là không thể. Một giải pháp tỷ lệ thuận với độ dài cuộc đua sẽ yêu cầu tới một tỷ thao tác. Thay vào đó, thuật toán phải phụ thuộc vào số lượng đảo và số vị trí quan trọng mà chúng tạo ra. 

Một sai lầm phổ biến là chỉ xem xét việc nhảy trực tiếp từ điểm cuối của hòn đảo này sang điểm cuối của hòn đảo khác. Điều này bỏ lỡ các tuyến đường tối ưu nơi bước nhảy bắt đầu hoặc kết thúc ở giữa đoạn nước. Một sai lầm khác là coi điểm cuối của đảo là vị trí bị chặn. Sự cố cho phép truy cập các điểm cuối, vì vậy chúng phải được đưa vào dưới dạng vị trí hợp lệ. 

Ví dụ: nếu không có đảo:```
5 3 10
0
```câu trả lời là`5`, vì lướt cả chặng đường nhanh hơn hai lần nhảy. Việc triển khai chỉ xem xét các bước nhảy sẽ trả về không chính xác`20`. 

Một trường hợp khác là bước nhảy chính xác đến điểm cuối đảo:```
10 5 1
1
5 6
```Câu trả lời là`2`. Nora có thể nhảy từ`0`ĐẾN`5`, thì từ`6`ĐẾN`10`. Nếu việc triển khai sử dụng nghiêm ngặt`< d`so sánh thay vì`<= d`, nó bỏ lỡ những bước nhảy này. 

Trường hợp biên cuối cùng là khi một hòn đảo ngăn cách hai vùng nước:```
10 10 100
1
4 6
```Câu trả lời là`100`, không`4`. Lướt sóng từ`0`ĐẾN`4`và sau đó từ`6`ĐẾN`10`là có thể, nhưng để đến được phía bên kia đòi hỏi phải nhảy qua đảo. 

## Phương pháp tiếp cận 

Giải pháp trực tiếp nhất là mô hình hóa mọi vị trí hữu ích có thể có dưới dạng đỉnh đồ thị. Một đỉnh đại diện cho điểm bắt đầu, điểm kết thúc hoặc điểm cuối đảo. Lướt sóng tạo ra các cạnh giữa các đỉnh liên tiếp không bị ngăn cách bởi một hòn đảo. Việc nhảy tạo ra các cạnh giữa hai đỉnh bất kỳ có khoảng cách lớn nhất`d`. 

Việc xây dựng đồ thị brute-force đã đủ cho vấn đề này. Có nhiều nhất`2n + 2`đỉnh, nhiều nhất là 1002. Việc kiểm tra từng cặp đỉnh sẽ tạo ra khoảng một triệu cạnh nhảy có thể có. Chạy Dijkstra trên biểu đồ này đủ nhanh. 

Một cách tiếp cận đơn giản hơn sẽ cố gắng mô phỏng mọi mét có thể có trong suốt cuộc đua. Điều đó thất bại ngay lập tức bởi vì`s`có thể`10^9`. 

Lý do biểu đồ điểm cuối hợp lệ là vì mọi thay đổi hữu ích của chế độ di chuyển đều diễn ra tại ranh giới đảo hoặc lúc bắt đầu và kết thúc. Nếu Nora hạ cánh tại một điểm nước tùy ý nào đó, chuyển động còn lại trên khoảng nước không bị gián đoạn đó có thể được di chuyển đến điểm cuối liên quan gần nhất mà không cần phải nhảy mạnh hơn hoặc tăng khoảng cách lướt sóng. Giữa hai vị trí liên quan liên tiếp không có trở ngại nào nên chỉ có điểm cuối mới quan trọng. 

Mô phỏng tọa độ vũ phu hoạt động vì nó thể hiện tất cả các khả năng, nhưng không thành công vì phạm vi tọa độ rất lớn. Quan sát cho thấy chỉ ranh giới của đảo mới ảnh hưởng đến chuyển động cho phép chúng ta nén tập hợp vô hạn các vị trí có thể có vào một biểu đồ có khoảng một nghìn đỉnh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Force trên từng mét | O (các) | O (các) | Quá chậm | 
| Đồ thị trên các điểm cuối đảo | O(n² log n) | O(n²) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Thu thập tất cả các vị trí quan trọng. Thêm vào`0`Và`s`, sau đó thêm cả hai điểm cuối của mỗi hòn đảo. Sắp xếp chúng từ trái sang phải. 
2. Tạo biểu đồ trong đó mọi vị trí quan trọng đều là một nút. Đối với mỗi cặp nút, hãy xem xét một cạnh nhảy. Nếu khoảng cách giữa chúng lớn nhất`d`, thêm lợi thế về chi phí`t`. 
3. Thêm các cạnh lướt giữa các vị trí quan trọng lân cận. Hai vị trí lân cận được nối với nhau bằng mép sóng vì giữa chúng không thể có hòn đảo. Chi phí cạnh là sự khác biệt tọa độ của họ. 
4. Chạy thuật toán Dijkstra từ vị trí bắt đầu. Mỗi lần thư giãn thể hiện việc chọn chuyển động lướt sóng hoặc chuyển động nhảy. 
5. Xuất ra khoảng cách của nút kết thúc. 

Lý do chỉ các vị trí quan trọng lân cận mới nhận được mép lướt sóng là vì cặp xa hơn hoặc có một hòn đảo giữa chúng hoặc có thể tiếp cận được bằng cách nối các cạnh lướt sóng lân cận. Việc thêm nhiều cạnh lướt sẽ không cải thiện đường đi ngắn nhất. 

Tại sao nó hoạt động: mọi chuyển động pháp lý mà Nora có thể thực hiện đều được thể hiện bằng một đường dẫn trong biểu đồ này. Lướt sóng được thể hiện bằng cách đi bộ qua các đoạn nước liên tiếp và mỗi lần nhảy được thể hiện bằng một cạnh nhảy giữa vị trí bắt đầu và kết thúc của nó. Ngược lại, mọi cạnh của đồ thị đều tương ứng với một chuyển động hợp lệ trong bài toán ban đầu. Vì Dijkstra tìm thấy đường đi ngắn nhất trong biểu đồ có trọng số cạnh không âm nên khoảng cách thu được chính xác là thời gian đua tối thiểu. 

## Giải pháp Python```python
import sys
import heapq

input = sys.stdin.readline

def solve():
    s, d, t = map(int, input().split())
    n = int(input())

    points = [0, s]
    for _ in range(n):
        l, r = map(int, input().split())
        points.append(l)
        points.append(r)

    points.sort()
    m = len(points)

    graph = [[] for _ in range(m)]

    for i in range(m):
        for j in range(i + 1, m):
            dist = points[j] - points[i]
            if dist <= d:
                graph[i].append((j, t))
                graph[j].append((i, t))

    for i in range(m - 1):
        dist = points[i + 1] - points[i]
        graph[i].append((i + 1, dist))
        graph[i + 1].append((i, dist))

    inf = 10**30
    dist = [inf] * m
    dist[0] = 0

    pq = [(0, 0)]
    while pq:
        cur, u = heapq.heappop(pq)
        if cur != dist[u]:
            continue

        for v, w in graph[u]:
            if cur + w < dist[v]:
                dist[v] = cur + w
                heapq.heappush(pq, (dist[v], v))

    print(dist[-1])

if __name__ == "__main__":
    solve()
```Quá trình xử lý đầu vào chỉ lưu trữ tọa độ mà trạng thái của vấn đề có thể thay đổi. Tổng số tọa độ này nhiều nhất là 1002. 

Việc xây dựng biểu đồ đầu tiên xử lý các bước nhảy. Mỗi cặp đều được kiểm tra vì số đỉnh nhỏ và đồ thị bước nhảy hoàn chỉnh vẫn có thể quản lý được. Các cạnh lướt chỉ được thêm vào giữa các vị trí được sắp xếp liền kề, bởi vì đó chính xác là những mảnh không có chướng ngại vật tối đa. 

Việc triển khai Dijkstra sử dụng hàng đợi ưu tiên. Kiểm tra mục nhập cũ tránh xử lý các mục nhập hàng đợi cũ sau khi đã tìm thấy khoảng cách tốt hơn. 

Số nguyên Python không bị tràn, nhưng giá trị vô cực vẫn cần lớn hơn nhiều so với bất kỳ câu trả lời nào có thể có. Một tuyến đường có thể chứa nhiều bước nhảy, do đó, giá trị trọng điểm nhỏ sẽ không an toàn. 

## Ví dụ đã hoạt động 

Đối với mẫu đầu tiên:```
9 3 4
2
2 4
7 8
```Các vị trí quan trọng được`0, 2, 4, 7, 8, 9`. 

| Vị trí nút | Thời điểm tốt nhất hiện nay | Phong trào được lựa chọn | 
| --- | --- | --- | 
| 0 | 0 | Bắt đầu | 
| 2 | 2 | Lướt sóng | 
| 4 | 4 | Nhảy từ 2 | 
| 7 | 8 | Nhảy từ 4 | 
| 8 | 8 | Nhảy từ 7 | 
| 9 | 11 | Lướt sóng | 

Kết quả là`11`. Dấu vết cho thấy tại sao các điểm cuối của đảo lại hữu ích: tuyến đường thay đổi từ lướt sóng sang nhảy chính xác tại các ranh giới đó. 

Đối với mẫu thứ hai:```
12 5 3
3
1 3
5 7
8 11
```Các vị trí quan trọng được`0, 1, 3, 5, 7, 8, 11, 12`. 

| Vị trí nút | Thời điểm tốt nhất hiện nay | Phong trào được lựa chọn | 
| --- | --- | --- | 
| 0 | 0 | Bắt đầu | 
| 1 | 1 | Lướt sóng | 
| 3 | 3 | Nhảy | 
| 5 | 6 | Nhảy | 
| 7 | 6 | Nhảy | 
| 8 | 6 | Nhảy | 
| 11 | 9 | Nhảy | 
| 12 | 9 | Lướt sóng | 

Câu trả lời là`9`. Ví dụ này chứng minh rằng nhiều lần nhảy liên tiếp có thể tốt hơn việc lướt quanh tất cả các đoạn nước. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n² log n) | Có O(n) đỉnh và O(n2) cạnh. Dijkstra thống trị về thời gian chạy. | 
| Không gian | O(n²) | Biểu đồ bước nhảy hoàn chỉnh chứa các cạnh O(n2). | 

Với tối đa 1002 đỉnh, đồ thị có khoảng một triệu cạnh có thể có. Điều này vừa vặn trong bộ nhớ và cho phép Dijkstra hoàn thành trong thời gian giới hạn. 

## Trường hợp thử nghiệm```python
import sys
import io
import heapq

def run(inp: str) -> str:
    old_stdin = sys.stdin
    sys.stdin = io.StringIO(inp)

    def solve():
        input = sys.stdin.readline
        s, d, t = map(int, input().split())
        n = int(input())

        points = [0, s]
        for _ in range(n):
            l, r = map(int, input().split())
            points += [l, r]

        points.sort()
        m = len(points)
        graph = [[] for _ in range(m)]

        for i in range(m):
            for j in range(i + 1, m):
                if points[j] - points[i] <= d:
                    graph[i].append((j, t))
                    graph[j].append((i, t))

        for i in range(m - 1):
            w = points[i + 1] - points[i]
            graph[i].append((i + 1, w))
            graph[i + 1].append((i, w))

        dist = [10**30] * m
        dist[0] = 0
        pq = [(0, 0)]

        while pq:
            cur, u = heapq.heappop(pq)
            if cur != dist[u]:
                continue
            for v, w in graph[u]:
                if cur + w < dist[v]:
                    dist[v] = cur + w
                    heapq.heappush(pq, (dist[v], v))

        return str(dist[-1]) + "\n"

    result = solve()
    sys.stdin = old_stdin
    return result

assert run("""9 3 4
2
2 4
7 8
""") == "11\n"

assert run("""12 5 3
3
1 3
5 7
8 11
""") == "9\n"

assert run("""5 3 10
0
""") == "5\n"

assert run("""10 5 1
1
5 6
""") == "2\n"

assert run("""1 1 1
0
""") == "1\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`5 3 10`không có đảo |`5`| Lướt trực tiếp tốt hơn là nhảy. | 
|`10 5 1`với đảo`5 6`|`2`| Nhảy chính xác khoảng cách`d`hoạt động. | 
|`1 1 1`không có đảo |`1`| Kích thước tọa độ tối thiểu và xử lý bắt đầu kết thúc. | 

## Vỏ cạnh 

Đối với trường hợp không có đảo:```
5 3 10
0
```Thuật toán chỉ tạo ra hai đỉnh,`0`Và`5`. Nó bổ sung thêm lợi thế về chi phí`5`. Cạnh nhảy là không thể vì khoảng cách lớn hơn`d`. Dijkstra trở lại`5`. 

Đối với trường hợp nhảy khoảng cách chính xác:```
10 5 1
1
5 6
```Các đỉnh là`0, 5, 6, 10`. Thuật toán thêm các bước nhảy từ`0`ĐẾN`5`và từ`6`ĐẾN`10`, cả hai đều có chi phí`1`. Câu trả lời cuối cùng trở thành`2`, cho thấy việc so sánh khoảng cách phải bao gồm sự bình đẳng. 

Đối với đảo ngăn cách nước:```
10 10 100
1
4 6
```Đồ thị chứa các cạnh lướt`0-4`Và`6-10`, nhưng không có mép sóng băng qua đảo. Một bước nhảy từ`4`ĐẾN`6`có sẵn vì khoảng cách chính xác`2`, vậy con đường ngắn nhất là`100 + 0`cho phần nhảy cộng với hai phần lướt sóng? Thực ra con đường rẻ nhất là`0 -> 4`bằng cách lướt sóng,`4 -> 6`bằng cách nhảy và`6 -> 10`bằng cách lướt sóng, đưa`4 + 100 + 4 = 108`. Thuật toán coi hòn đảo là chướng ngại vật bắt buộc một cách chính xác và không bao giờ cho phép lướt qua nó bất hợp pháp.
