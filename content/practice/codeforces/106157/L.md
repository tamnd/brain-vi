---
title: "CF 106157L - Đơn hàng cuối cùng"
description: "Chúng tôi có một thị trấn với một quán rượu ở mỗi ngã ba của mạng lưới đường bộ. Di chuyển dọc các con đường cần có thời gian và quán rượu nào cũng có thời gian đóng cửa. Chúng tôi bắt đầu tại quán rượu 1 vào thời điểm 0. Thời lượng của các buổi uống rượu đã được ấn định trước."
date: "2026-06-25T11:20:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106157
codeforces_index: "L"
codeforces_contest_name: "2025 United Kingdom and Ireland Programming Contest (UKIEPC 2025)"
rating: 0
weight: 106157
solve_time_s: 48
verified: true
draft: false
---

[CF 106157L - Đơn hàng cuối cùng](https://codeforces.com/problemset/problem/106157/L) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 48s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một thị trấn với một quán rượu ở mỗi ngã ba của mạng lưới đường bộ. Di chuyển dọc các con đường cần có thời gian và quán rượu nào cũng có thời gian đóng cửa. Chúng ta bắt đầu ở quán rượu 1 vào thời điểm 0. 

Thời gian của các buổi uống rượu được ấn định trước. Pint đầu tiên mất$t_1$giây, giây thứ hai mất$t_2$, v.v. cho đến$t_r$. Thời lượng không nhất thiết phải tăng lên. 

Một pint chỉ được tính nếu uống hết trước khi quán rượu nơi nó say đóng cửa. Chúng ta có thể di chuyển qua mạng lưới đường bộ theo bất kỳ cách nào chúng ta thích, ghé lại các quán rượu nhiều lần và thậm chí đi qua các quán rượu mà không uống rượu. Hạn chế duy nhất là không được uống hai ly liên tiếp trong cùng một quán rượu. Mục tiêu là tối đa hóa số lượng pint được tiêu thụ. 

Giới hạn nhỏ về số lượng pint,$r \le 100$, và vừa phải về số lượng quán rượu,$k \le 300$. Đồ thị có thể khá dày đặc lên tới 90.000 con đường. 

Việc tìm kiếm thô bạo trên tất cả các chuỗi quán rượu có thể sẽ là vô vọng. Ngay cả việc chọn một quán rượu cho mỗi pint cũng đã mang lại hiệu quả tương đối.$k^r$khả năng. Với$k=300$Và$r=100$, đó là lớn về mặt thiên văn. 

Kích thước biểu đồ cho thấy rằng việc tính toán thời gian di chuyển ngắn nhất giữa mỗi cặp quán rượu là khả thi. Khi đã biết những con đường ngắn nhất đó, cấu trúc đường thực tế không còn quan trọng nữa mà chỉ còn thời gian di chuyển tối thiểu giữa các quán rượu. 

Một trường hợp phức tạp xuất hiện khi ghé thăm lại một quán rượu. Được phép thăm lại nhưng chỉ sau khi uống rượu ở một nơi khác ở giữa. 

Ví dụ:```
2
5 5
2
20 20
1
1 2 1
```Trình tự pub 1 → pub 1 là bất hợp pháp vì cùng một pub được sử dụng cho các panh liên tiếp. Giải pháp chỉ kiểm tra thời gian đóng cửa sẽ tính sai hai pint. 

Một trường hợp khác là pint đầu tiên không cần phải uống ở quán rượu 1. 

Ví dụ:```
1
10
2
100 15
1
1 2 1
```Di chuyển đến quán rượu 2 mất 1 giây nên pint đầu tiên kết thúc ở thời điểm 11 và có giá trị. Bất kỳ giải pháp nào buộc phải tiêu thụ pint đầu tiên ở quán rượu 1 sẽ bỏ lỡ khả năng này. 

Trường hợp cạnh thứ ba là biểu đồ có thể chứa nhiều con đường nằm giữa cùng một cặp nút giao. Chỉ có thời gian di chuyển ngắn nhất mới quan trọng. Việc sử dụng đường tùy ý thay vì đường tốt nhất có thể khiến lịch trình khả thi dường như không thể thực hiện được. 

## Phương pháp tiếp cận 

Ý tưởng trực tiếp nhất là thử mọi lựa chọn quán rượu có thể cho mỗi pint. Nếu pint hiện tại say trong quán rượu$u$, chúng tôi chọn một quán rượu$v \ne u$đối với lần tiếp theo, hãy thêm thời gian di chuyển và thời gian uống rượu, đồng thời kiểm tra xem thời gian đóng cửa có được tôn trọng hay không. 

Cách tiếp cận này đúng vì nó liệt kê rõ ràng tất cả các lịch trình hợp lệ. Vấn đề là số lượng khả năng. Thậm chí bỏ qua chi tiết du lịch, có khoảng$k$sự lựa chọn trên mỗi pint, sản xuất$k^r$tiểu bang. Với$k=300$Và$r=100$, điều này hoàn toàn không thể thực hiện được. 

Quan sát quan trọng là chỉ có thời gian hoàn thành sớm nhất có thể mới quan trọng. 

Giả sử chúng ta đã say rồi$j$pint và cốc cuối cùng đã được uống hết ở quán rượu$u$. Nếu hai lịch trình khác nhau đạt đến cùng một trạng thái$(j,u)$, lịch trình có thời gian hoàn thành nhỏ hơn ít nhất luôn tốt bằng lịch trình khác. Đến sớm hơn không bao giờ có thể làm giảm các lựa chọn trong tương lai vì tất cả các ràng buộc đều là giới hạn trên về thời gian. 

Điều này đưa ra một công thức lập trình động. 

Trước DP, chúng tôi tính toán các đường đi ngắn nhất cho tất cả các cặp. Bất kỳ tuyến đường nào giữa hai quán rượu đều có thể được thay thế bằng tuyến đường ngắn nhất, vì đến sớm hơn luôn tốt hơn. 

Định nghĩa:$$dp[j][u]$$là thời gian tuyệt đối tối thiểu có thể mà tại đó$j$-sơn hoàn thiện trong quán rượu$u$. 

Từ trạng thái như vậy, pint tiếp theo có thể được uống ở bất kỳ quán rượu nào$v \ne u$. Thời gian hoàn thành sẽ là:$$dp[j][u] + dist[u][v] + t_{j+1}$$và quá trình chuyển đổi chỉ có hiệu lực nếu thời gian kết thúc đó không vượt quá$c_v$. 

Số lượng các trạng thái chỉ là$r \times k$và mỗi tiểu bang thử tất cả các quán rượu tiếp theo có thể, đưa ra$O(rk^2)$chuyển tiếp. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(k^r)$|$O(r)$| Quá chậm | 
| Tối ưu |$O(k^3 + rk^2)$|$O(rk)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc thời gian uống rượu, thời gian đóng cửa quán rượu và mạng lưới đường bộ. 
2. Xây dựng ma trận khoảng cách được khởi tạo ở mức vô cùng. 
3. Đặt khoảng cách từ mỗi quán rượu đến chính nó là 0. 
4. Đối với mỗi con đường, hãy giữ trọng số tối thiểu giữa các điểm cuối của nó. Nhiều con đường có thể kết nối cùng một cặp quán rượu. 
5. Chạy Floyd-Warshall để tính thời gian di chuyển ngắn nhất giữa mỗi cặp quán rượu. 
6. Tạo bảng DP trong đó`dp[j][u]`lưu trữ chính xác thời gian kết thúc sớm nhất có thể sau khi uống rượu`j`pint và kết thúc trong quán rượu`u`. 
7. Khởi tạo pint đầu tiên. Cho mỗi quán rượu`u`, pint đầu tiên có thể được uống ở đó nếu$$dist[1][u] + t_1 \le c_u$$Khi điều này được giữ, hãy lưu thời gian hoàn thành đó vào`dp[1][u]`. 

1. Đối với mỗi số pint được tiêu thụ`j`từ 1 đến`r-1`, xử lý tất cả các quán rượu kết thúc có thể truy cập`u`. 
2. Đối với mọi quán rượu đích`v`khác với`u`, tính toán$$finish = dp[j][u] + dist[u][v] + t_{j+1}$$Nếu như`finish <= c[v]`, cập nhật$$dp[j+1][v]$$với thời gian hoàn thiện tối thiểu được tìm thấy cho đến nay. 

1. Đáp án lớn nhất`j`mà một số tiểu bang`dp[j][u]`có thể truy cập được. 

### Tại sao nó hoạt động 

Trạng thái DP ghi lại thời gian hoàn thành sớm nhất có thể đạt được cho một cặp cố định, bao gồm số lượng panh đã tiêu thụ và quán rượu nơi uống panh cuối cùng. 

Mọi quyết định trong tương lai chỉ phụ thuộc vào hai thông tin đó. Việc hoàn thành sớm ít nhất luôn tốt bằng việc hoàn thành sau vì tất cả các ràng buộc đều là thời hạn. Việc thay thế một lịch trình bằng một lịch trình khác có cùng trạng thái sớm hơn không thể làm vô hiệu bất kỳ chuỗi di chuyển nào trong tương lai. 

Floyd-Warshall đảm bảo rằng`dist[u][v]`là thời gian di chuyển tối thiểu có thể giữa các quán rượu. Bất kỳ lịch trình nào sử dụng tuyến đường dài hơn chỉ có thể kết thúc muộn hơn, vì vậy việc xem xét các tuyến đường ngắn nhất là đủ. 

Vì mỗi chuỗi quán rượu hợp lệ tương ứng với một chuỗi chuyển đổi DP và mỗi chuyển đổi DP đại diện cho một chuỗi hành động hợp lệ, nên DP sẽ khám phá chính xác tất cả các lịch trình khả thi. Số lượng pint có thể tiếp cận lớn nhất là số lượng tối ưu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10**18

r = int(input())
t = list(map(int, input().split()))

k = int(input())
c = list(map(int, input().split()))

m = int(input())

dist = [[INF] * k for _ in range(k)]

for i in range(k):
    dist[i][i] = 0

for _ in range(m):
    a, b, d = map(int, input().split())
    a -= 1
    b -= 1
    if d < dist[a][b]:
        dist[a][b] = d
        dist[b][a] = d

for mid in range(k):
    dm = dist[mid]
    for i in range(k):
        if dist[i][mid] == INF:
            continue
        via = dist[i][mid]
        di = dist[i]
        for j in range(k):
            nd = via + dm[j]
            if nd < di[j]:
                di[j] = nd

dp = [[INF] * k for _ in range(r + 1)]

start = 0

for u in range(k):
    if dist[start][u] == INF:
        continue
    finish = dist[start][u] + t[0]
    if finish <= c[u]:
        dp[1][u] = finish

answer = 0

for u in range(k):
    if dp[1][u] < INF:
        answer = 1

for drank in range(1, r):
    for u in range(k):
        cur = dp[drank][u]
        if cur == INF:
            continue

        for v in range(k):
            if v == u:
                continue
            if dist[u][v] == INF:
                continue

            finish = cur + dist[u][v] + t[drank]

            if finish <= c[v] and finish < dp[drank + 1][v]:
                dp[drank + 1][v] = finish
                answer = max(answer, drank + 1)

print(answer)
```Phần đầu tiên xây dựng ma trận đường đi ngắn nhất. Nhiều con đường giữa cùng một cặp quán rượu được xử lý bằng cách chỉ giữ trọng lượng cạnh tối thiểu. 

Việc triển khai Floyd-Warshall cập nhật ma trận tại chỗ. Từ$k \le 300$, cái$O(k^3)$sự phức tạp có thể chấp nhận được. 

Bảng DP lưu trữ thời gian hoàn thành sớm nhất thay vì khả năng tiếp cận boolean. Đây là sự tối ưu hóa quan trọng. Nếu hai lịch trình kết thúc tại cùng một quán rượu sau cùng một số pint thì chỉ có lịch trình trước đó là quan trọng. 

Pint đầu tiên cần được xử lý đặc biệt vì không có quán rượu nào trước đó. Chúng ta chỉ cần đi từ quán rượu số 1 đến quán rượu đã chọn và tiêu$t_1$giây uống rượu ở đó. 

Quá trình chuyển đổi bỏ qua`v == u`, thực thi quy định không được uống các panh liên tiếp trong cùng một quán rượu. 

## Ví dụ đã hoạt động 

### Ví dụ 1```
2
5 5
2
10 20
1
1 2 2
```Khoảng cách ngắn nhất: 

| Từ | Đến 1 | Đến 2 | 
| --- | --- | --- | 
| 1 | 0 | 2 | 
| 2 | 2 | 0 | 

Khởi tạo: 

| Quán rượu | Kết thúc pint đầu tiên | hợp lệ | 
| --- | --- | --- | 
| 1 | 5 | Có | 
| 2 | 7 | Có | 

Chuyển tiếp DP: 

| Pint | Quán rượu hiện tại | Thời gian | Quán rượu tiếp theo | Thời Gian Mới | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | 5 | 2 | 12 | 
| 1 | 2 | 7 | 1 | 14 | 

Chỉ lần chuyển tiếp đầu tiên mới tôn trọng thời gian đóng cửa của pub 2. 

Trả lời:`2`. 

Dấu vết này cho thấy lý do tại sao chúng tôi lưu trữ thời gian hoàn thiện sớm nhất. Kết thúc pint đầu tiên ở quán rượu 1 sẽ tốt hơn kết thúc ở quán rượu 2 vì nó để lại nhiều thời gian hơn cho những hành động sau này. 

### Ví dụ 2```
3
4 4 4
3
5 20 20
2
1 2 1
2 3 1
```Khởi tạo: 

| Quán rượu | Kết thúc pint đầu tiên | 
| --- | --- | 
| 1 | 4 | 
| 2 | 5 | 
| 3 | 6 | 

Sau một pint: 

| Pint | Quán rượu | Thời gian | 
| --- | --- | --- | 
| 1 | 1 | 4 | 
| 1 | 2 | 5 | 
| 1 | 3 | 6 | 

Sau hai pint: 

| Pint | Quán rượu | Thời Gian Tốt Nhất | 
| --- | --- | --- | 
| 2 | 2 | 9 | 
| 2 | 3 | 10 | 

Sau ba pint: 

| Pint | Quán rượu | Thời Gian Tốt Nhất | 
| --- | --- | --- | 
| 3 | 3 | 14 | 

Trả lời:`3`. 

Ví dụ này chứng minh rằng việc truy cập lại và di chuyển qua các quán rượu trung gian được xử lý một cách tự nhiên bằng khoảng cách đường đi ngắn nhất. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(k^3 + rk^2)$| Chuyển đổi Floyd-Warshall cộng với DP | 
| Không gian |$O(rk + k^2)$| Bảng DP và ma trận khoảng cách | 

Với$k \le 300$Và$r \le 100$, giai đoạn Floyd-Warshall thực hiện khoảng 27 triệu lượt cập nhật và DP thực hiện khoảng 9 triệu lượt chuyển tiếp, cả hai đều nằm trong giới hạn cuộc thi thông thường. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    input = sys.stdin.readline
    INF = 10**18

    r = int(input())
    t = list(map(int, input().split()))

    k = int(input())
    c = list(map(int, input().split()))

    m = int(input())

    dist = [[INF] * k for _ in range(k)]
    for i in range(k):
        dist[i][i] = 0

    for _ in range(m):
        a, b, d = map(int, input().split())
        a -= 1
        b -= 1
        dist[a][b] = min(dist[a][b], d)
        dist[b][a] = min(dist[b][a], d)

    for mid in range(k):
        for i in range(k):
            for j in range(k):
                dist[i][j] = min(
                    dist[i][j],
                    dist[i][mid] + dist[mid][j]
                )

    dp = [[INF] * k for _ in range(r + 1)]

    for u in range(k):
        finish = dist[0][u] + t[0]
        if finish <= c[u]:
            dp[1][u] = finish

    ans = 0
    for u in range(k):
        if dp[1][u] < INF:
            ans = 1

    for drank in range(1, r):
        for u in range(k):
            if dp[drank][u] == INF:
                continue
            for v in range(k):
                if v == u:
                    continue
                finish = dp[drank][u] + dist[u][v] + t[drank]
                if finish <= c[v]:
                    dp[drank + 1][v] = min(
                        dp[drank + 1][v],
                        finish
                    )
                    ans = max(ans, drank + 1)

    return str(ans) + "\n"

# minimum size
assert run("""1
5
1
5
1
1 1 1
""") == "1\n"

# cannot drink any pint
assert run("""1
10
1
5
1
1 1 1
""") == "0\n"

# alternating between two pubs
assert run("""2
5 5
2
10 20
1
1 2 2
""") == "2\n"

# revisiting allowed, consecutive not allowed
assert run("""3
1 1 1
2
100 100
1
1 2 1
""") == "3\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Quán rượu đơn lẻ, một pint khả thi | 1 | Phiên bản hợp lệ có kích thước tối thiểu | 
| Quán rượu duy nhất, đã lỡ thời hạn | 0 | Không có trạng thái có thể truy cập | 
| Hai quán rượu với một chuyển tiếp | 2 | Chuyển đổi DP cơ bản | 
| Luân phiên giữa hai quán rượu liên tục | 3 | Được phép xem lại | 

## Vỏ cạnh 

Hãy xem xét trường hợp cùng một quán rượu được sử dụng hai lần liên tiếp:```
2
1 1
1
100
1
1 1 1
```Sau pint đầu tiên, pub duy nhất vẫn là pub 1. Vòng chuyển tiếp từ chối`v == u`, do đó không có pint thứ hai nào được tạo ra. Câu trả lời vẫn là 1. 

Hãy xem xét trường hợp pint đầu tiên không được uống trong quán rượu 1:```
1
10
2
100 15
1
1 2 1
```Việc khởi tạo sẽ kiểm tra mọi quán rượu. Đối với pub 2 thì thời gian kết thúc là`1 + 10 = 11`, thỏa mãn thời gian đóng cửa. Nhà nước`dp[1][2] = 11`được tạo ra, đưa ra câu trả lời đúng là 1. 

Hãy xem xét nhiều con đường giữa các quán rượu giống nhau:```
1
5
2
100 100
2
1 2 100
1 2 1
```Thuật toán lưu trữ trọng số cạnh tối thiểu, cụ thể là 1. Khi đó, ma trận đường đi ngắn nhất sẽ ghi lại chính xác thời gian di chuyển là 1 thay vì 100. Bất kỳ việc triển khai nào bỏ qua các cạnh song song đều có thể loại bỏ không chính xác các lịch trình khả thi.
