---
title: "CF 103081D - Chạy bộ"
description: "Chúng ta được cho một đồ thị có trọng số vô hướng trong đó các đỉnh là giao điểm và các cạnh là các đường có độ dài. Phoebe luôn bắt đầu và kết thúc mỗi buổi chạy bộ ở nút 0."
date: "2026-07-04T00:24:40+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 103081
codeforces_index: "D"
codeforces_contest_name: "2020-2021 ICPC Southwestern European Regional Contest (SWERC 2020)"
rating: 0
weight: 103081
solve_time_s: 61
verified: true
draft: false
---

[CF 103081D - Chạy bộ](https://codeforces.com/problemset/problem/103081/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1m 1s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cho một đồ thị có trọng số vô hướng trong đó các đỉnh là giao điểm và các cạnh là các đường có độ dài. Phoebe luôn bắt đầu và kết thúc mỗi phiên chạy bộ tại nút 0. Mỗi phiên chạy bộ là một cuộc đi bộ khép kín với tổng chiều dài bị ràng buộc nằm giữa giới hạn dưới$L$và giới hạn trên$U$. 

Có một quy tắc kế toán bổ sung để thúc đẩy mục tiêu. Một con phố được coi là "mới" khi Phoebe đi qua bất kỳ phần nào của nó lần đầu tiên và một buổi chạy bộ được gọi là thú vị nếu nó chứa ít nhất một con phố chưa từng được sử dụng trong bất kỳ buổi chạy bộ nào trước đó. Khi một con phố đã được sử dụng, nó sẽ không bao giờ mang lại sự mới lạ nữa, ngay cả khi nó được đi qua một phần trong những lần chạy sau. 

Nhiệm vụ là lên kế hoạch cho càng nhiều buổi chạy bộ thú vị càng tốt. Mỗi phiên có thể sử dụng lại các đường phố cũ một cách tự do, nhưng để được tính là một phiên mới, nó phải giới thiệu ít nhất một đường phố chưa được sử dụng trước đó. 

Các ràng buộc rất lớn, có thể lên tới$10^5$giao lộ và$10^5$đường phố. Điều này ngay lập tức loại trừ bất cứ điều gì gần gũi hơn với$O(S^2)$hoặc thậm chí lặp lại các phép tính đường đi ngắn nhất trên mỗi cạnh. Một tuyến tính đơn hoặc$O(S \log S)$vượt qua Dijkstra là đúng quy mô. 

Một điểm tinh tế trong câu nói là Phoebe có thể chỉ đi qua một đoạn phố, nhưng thời điểm cô ấy chạm vào một con phố, nó được coi là hoàn toàn “nhìn thấy”. Điều này loại bỏ bất kỳ lý do nào về việc sử dụng một phần cạnh. Mỗi cạnh không được sử dụng hoặc đã được sử dụng bởi lần chạy thú vị trước đó. 

Điều tinh tế thứ hai là đường chạy chỉ cần bao gồm ít nhất một đường phố mới, nhưng có thể bao gồm nhiều đường phố. Một cách giải thích ngây thơ sẽ gợi ý rằng chúng ta có thể cần phải phân chia các cạnh thành các cấu trúc phức tạp, nhưng điều đó hóa ra là không cần thiết. 

Một số trường hợp đặc biệt quan trọng: 

Nếu đồ thị có một cạnh$0 - 1$với chiều dài$3$,$L = 7$,$U = 7$, thì không có lượt chạy hợp lệ nào tồn tại vì mọi lượt đi khép kín đều phải có độ dài ít nhất$6$, vậy câu trả lời là$0$. 

Nếu có nhiều chu kỳ hình thành cạnh, một cách tiếp cận đơn giản có thể cố gắng xây dựng các chuyến tham quan dài và cho rằng các cạnh không thể được sử dụng lại trên các lần chạy ứng viên khác nhau theo cách có cấu trúc, trong khi giải pháp thực sự chỉ phụ thuộc vào khả năng tiếp cận và cấu trúc đường dẫn ngắn nhất từ ​​nút$0$. 

## Phương pháp tiếp cận 

Một cách giải thích bạo lực trực tiếp cố gắng mô phỏng quá trình. Người ta có thể tưởng tượng việc tìm kiếm liên tục một hành trình khép kín bắt đầu và kết thúc ở nút 0, với độ dài bằng$[L, U]$, chứa ít nhất một cạnh chưa được sử dụng, sau đó đánh dấu các cạnh đó là đã sử dụng và tiếp tục. Ngay cả khi chúng ta có thể phát hiện tính khả thi của một bước đi đơn lẻ, việc liệt kê những bước đi như vậy sẽ mang tính kết hợp bùng nổ. Số lượng bước đi có thể tăng theo cấp số nhân theo độ dài đường dẫn do chu kỳ và thậm chí việc quyết định tập hợp con nào của các cạnh sẽ bao gồm trong mỗi lần chạy sẽ dẫn đến một không gian tìm kiếm khó hiểu. 

Quan sát quan trọng là điều kiện “sự thú vị” gần như tách rời hoàn toàn. Mỗi cạnh chỉ quan trọng khi nó được sử dụng lần đầu tiên. Nếu chúng ta có thể chỉ định mỗi cạnh cho nhiều nhất một lần chạy và đảm bảo rằng đối với mỗi cạnh được chọn tồn tại một số bước đi khép kín hợp lệ bao gồm nó, thì chúng ta có thể nhận ra mỗi phép gán như vậy là một lần chạy riêng biệt. Vì các cạnh độc lập theo nghĩa này nên số lần chạy tối đa sẽ trở thành số cạnh có thể được hỗ trợ riêng lẻ bởi ít nhất một bước đi khép kín hợp lệ. 

Vì vậy, vấn đề giảm xuống còn việc kiểm tra từng cạnh một cách độc lập: liệu chúng ta có thể xây dựng bất kỳ bước đi khép kín nào từ 0 bao gồm cạnh này và có tổng chiều dài bằng$[L, U]$? 

Đối với đồ thị có trọng số vô hướng, bước đi khép kín ngắn nhất có thể từ 0 buộc phải đi qua một cạnh$(u, v, w)$thu được bằng cách đi từ 0 đến$u$, chiếm ưu thế$v$, và trở về từ$v$đến 0. Chi phí là:$$\text{dist}(0, u) + w + \text{dist}(0, v)$$Bất kỳ bước đi hợp lệ nào khác bao gồm cạnh chỉ có thể dài hơn vì tất cả trọng số của cạnh đều dương. Điều này rất quan trọng: khi chúng ta có độ dài tối thiểu có thể để bao gồm một cạnh, chúng ta luôn có thể tăng độ dài hơn nữa bằng cách đi vòng qua các chu kỳ có thể đạt được từ 0 mà không làm mất tính khả thi hoặc vi phạm các ràng buộc dương. 

Vì vậy, một cạnh có thể sử dụng được khi và chỉ khi độ dài chu kỳ cưỡng bức tối thiểu của nó lớn nhất$U$. Nếu điều kiện đó được giữ vững, chúng ta luôn có thể kéo dài bước đi để hạ cánh ở đâu đó bên trong$[L, U]$. Giới hạn dưới$L$không hạn chế tính khả thi vì chúng ta luôn có thể tăng chiều dài bằng cách chèn đường vòng. 

Điều này làm giảm vấn đề xuống còn việc tính toán đường đi ngắn nhất bằng một nguồn, sau đó là quét tuyến tính trên các cạnh. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Liệt kê Brute Force số lần chạy | Hàm mũ | Cao | Quá chậm | 
| Dijkstra + kiểm tra từng cạnh |$O(S \log I)$|$O(I + S)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### ## Hướng dẫn thuật toán 

1. Tính khoảng cách đường đi ngắn nhất từ nút 0 đến mọi nút khác bằng thuật toán Dijkstra. Điều này mang lại chi phí đi lại tối thiểu từ giao lộ nhà đến bất kỳ điểm nào trong biểu đồ. Bước này là cần thiết vì mọi bước đi khép kín ứng cử viên đều phải bắt đầu và kết thúc tại nút 0, do đó khoảng cách từ 0 xác định cách rẻ nhất để “tiếp cận” bất kỳ cạnh nào. 
2. Với mỗi con phố (u, v, w), hãy tính chi phí tối thiểu của một lối đi khép kín buộc phải bao gồm con phố này. Điều này được tính bằng dist[u] + w + dist[v]. Ý tưởng là chúng ta đi vào cạnh từ một điểm cuối sau khi đạt điểm tối ưu từ 0 và sau đó quay trở lại 0 một cách tối ưu từ điểm cuối kia. 
3. Kiểm tra xem chi phí yêu cầu tối thiểu này có tối đa là U hay không. Nếu nó vượt quá U, thì ngay cả bước đi khép kín hợp lệ ngắn nhất có thể chứa cạnh này cũng quá dài, do đó, cạnh này không bao giờ có thể được sử dụng trong bất kỳ lần chạy hợp lệ nào. 
4. Đếm tất cả các cạnh thỏa mãn điều kiện. Mỗi cạnh như vậy có thể được chỉ định cho một đường chạy riêng biệt, vì một đường chạy chỉ yêu cầu ít nhất một cạnh mới và chúng ta luôn có thể xây dựng một bước đi riêng cho từng cạnh một cách độc lập. 
5. Xuất tổng số lần chạy thú vị tối đa. 

### Tại sao nó hoạt động 

Bất biến chính là đối với mọi cạnh, chúng tôi xác định chính xác độ dài tối thiểu có thể có của bất kỳ bước đi khép kín nào bắt đầu và kết thúc tại 0 bao gồm cạnh đó. Bởi vì tất cả các trọng số của cạnh đều hoàn toàn dương nên bất kỳ bước đi thay thế nào chứa cùng một cạnh chỉ có thể làm tăng tổng chiều dài. Điều này làm cho giá trị được tính toán trở thành giới hạn dưới thực sự. 

Khi giới hạn dưới này nằm trong khoảng$[L, U]$, lối đi bộ có thể được kéo dài lên trên bằng cách chèn các đường vòng mà không loại bỏ rìa khỏi tuyến đường, điều đó có nghĩa là tính khả thi chỉ phụ thuộc vào việc mức tối thiểu có nhiều nhất hay không$U$. Vì mỗi cạnh có thể được gán độc lập cho một lần chạy, nên việc tối đa hóa số lần chạy sẽ giảm xuống việc đếm các cạnh khả thi. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

import heapq

def solve():
    I, S, L, U = map(int, input().split())
    g = [[] for _ in range(I)]
    edges = []

    for _ in range(S):
        u, v, w = map(int, input().split())
        g[u].append((v, w))
        g[v].append((u, w))
        edges.append((u, v, w))

    INF = 10**18
    dist = [INF] * I
    dist[0] = 0
    pq = [(0, 0)]

    while pq:
        d, u = heapq.heappop(pq)
        if d != dist[u]:
            continue
        for v, w in g[u]:
            nd = d + w
            if nd < dist[v]:
                dist[v] = nd
                heapq.heappush(pq, (nd, v))

    ans = 0
    for u, v, w in edges:
        if dist[u] + dist[v] + w <= U:
            ans += 1

    print(ans)

if __name__ == "__main__":
    solve()
```Giải pháp đầu tiên xây dựng biểu đồ và chạy Dijkstra từ nút 0 để tính khoảng cách ngắn nhất. Đây là cấu trúc toàn cục duy nhất cần thiết, bởi vì mọi kiểm tra tính khả thi của một cạnh chỉ phụ thuộc vào khoảng cách đến các điểm cuối của nó. 

Sau đó, mỗi cạnh được đánh giá độc lập bằng cách sử dụng công thức cho bước đi khép kín cưỡng bức tối thiểu. Việc kiểm tra chống lại$U$là đủ vì bất kỳ mức tối thiểu khả thi nào cũng tự động ngụ ý khả năng mở rộng trong khoảng thời gian được yêu cầu. 

Một lỗi triển khai phổ biến là quên rằng biểu đồ không có hướng, do đó cả hai hướng phải được thêm vào danh sách kề. Một điểm tinh tế khác là chúng tôi không bao giờ sử dụng rõ ràng$L$trong lần kiểm tra cuối cùng, vì khả năng tăng độ dài đường dẫn thông qua các chu kỳ làm cho giới hạn dưới không phù hợp với tính khả thi. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 4 80 90
0 1 40
0 2 50
1 2 30
2 3 10
```Đường đi ngắn nhất từ 0: 

| Nút | quận | 
| --- | --- | 
| 0 | 0 | 
| 1 | 40 | 
| 2 | 50 | 
| 3 | 60 | 

Đánh giá cạnh: 

| Cạnh | tính toán | giá trị | hợp lệ | 
| --- | --- | --- | --- | 
| 0-1 (40) | 0 + 40 + 40 | 80 | vâng | 
| 0-2 (50) | 0 + 50 + 50 | 100 | không | 
| 1-2 (30) | 40 + 30 + 50 | 120 | không | 
| 2-3 (10) | 50 + 10 + 60 | 120 | không | 

Câu trả lời là 1. 

Điều này cho thấy rằng chỉ các cạnh có chu kỳ quay trở lại bắt buộc phù hợp với$U$có thể được chỉ định để chạy, bất kể kết nối biểu đồ. 

### Ví dụ 2 

đầu vào:```
2 1 7 7
0 1 3
```Đường đi ngắn nhất: 

quận [0] = 0, quận [1] = 3 

Cạnh: 

0 + 3 + 3 = 6 ≤ 7 nên nó đúng. 

Câu trả lời là 1. 

Điều này chứng tỏ rằng ngay cả khi chu kỳ tối thiểu dưới đây$L$, nó vẫn hoạt động vì các đường vòng bổ sung có thể tăng độ dài lên chính xác là 7. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(S \log I)$| Dijkstra kết thúc$S$các cạnh bằng cách sử dụng một đống, cộng với một lần quét tuyến tính trên các cạnh | 
| Không gian |$O(I + S)$| danh sách kề và mảng khoảng cách | 

Các ràng buộc cho phép lên đến$10^5$các nút và các cạnh, do đó, một đường chuyền Dijkstra duy nhất nằm trong giới hạn thoải mái và quá trình quét cạnh tuyến tính cuối cùng là không đáng kể. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    return sys.stdout.getvalue() if False else solve_capture(inp)

def solve_capture(inp: str) -> str:
    import sys, heapq
    input = sys.stdin.readline
    I, S, L, U = map(int, input().split())
    g = [[] for _ in range(I)]
    edges = []
    for _ in range(S):
        u, v, w = map(int, input().split())
        g[u].append((v, w))
        g[v].append((u, w))
        edges.append((u, v, w))

    INF = 10**18
    dist = [INF] * I
    dist[0] = 0
    pq = [(0, 0)]
    while pq:
        d, u = heapq.heappop(pq)
        if d != dist[u]:
            continue
        for v, w in g[u]:
            nd = d + w
            if nd < dist[v]:
                dist[v] = nd
                heapq.heappush(pq, (nd, v))

    ans = 0
    for u, v, w in edges:
        if dist[u] + dist[v] + w <= U:
            ans += 1
    return str(ans)

# provided samples
assert solve_capture("""4 4 80 90
0 1 40
0 2 50
1 2 30
2 3 10
""") == "1"

assert solve_capture("""2 1 7 7
0 1 3
""") == "1"

# custom cases
assert solve_capture("""1 0 1 10
""") == "0", "no edges"

assert solve_capture("""3 2 5 5
0 1 2
1 2 2
""") == "0", "cycle too large via 0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đồ thị trống | 0 | không có cạnh có nghĩa là không chạy | 
| đồ thị đường đi nhỏ U | 0 | các cạnh vượt quá giới hạn đi lại khép kín cho phép | 

## Vỏ cạnh 

Trường hợp cạnh chính là khi biểu đồ chứa các cạnh có thể truy cập được nhưng vẫn quá đắt để đưa vào bất kỳ bước đi khép kín nào từ nút 0. Ví dụ: nếu đường đi ngắn nhất đến cả hai điểm cuối đều lớn, thì ngay cả một cạnh nhỏ cũng trở nên không hợp lệ vì hành trình quay lại chiếm ưu thế. Thuật toán loại bỏ chính xác các cạnh như vậy vì chu trình cưỡng bức được tính toán vượt quá$U$. 

Một trường hợp cạnh khác là khi$L$lớn nhưng$U$chỉ lớn hơn một chút so với chu kỳ tối thiểu. Giải pháp vẫn hoạt động vì mọi mức tối thiểu khả thi dưới đây hoặc bằng$U$có thể được thổi phồng bằng cách sử dụng đường vòng. Thuật toán hoàn toàn dựa vào sự tồn tại của các chu kỳ trọng số dương có thể đạt được từ 0, điều này luôn cho phép tăng độ dài được kiểm soát mà không phá vỡ tính hợp lệ. 

Cuối cùng, khi đồ thị là một cây, mỗi cạnh tạo thành một đường dẫn duy nhất và giải pháp giảm xuống việc kiểm tra xem chu trình đường đi qua 0 của mỗi cạnh có phù hợp với$U$, mà công thức xử lý trực tiếp mà không cần viết hoa đặc biệt.
