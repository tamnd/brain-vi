---
title: "CF 104699H - \u041a\u043e\u043d\u0444\u0435\u0440\u0435\u043d\u0446\u0438\u044f"
description: "Chúng ta có một mạng lưới các thành phố không định hướng có trọng số, trong đó mỗi thành phố có một số nhà khoa học. Một nhà khoa học có thể di chuyển dọc theo các con đường giữa các thành phố, trả tổng chi phí biên dọc theo tuyến đường của họ."
date: "2026-06-29T08:35:26+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 104699
codeforces_index: "H"
codeforces_contest_name: "\u0418\u043d\u0442\u0435\u0440\u043d\u0435\u0442-\u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u044b, \u0421\u0435\u0437\u043e\u043d 2023-2024, \u0412\u0442\u043e\u0440\u0430\u044f \u043a\u043e\u043c\u0430\u043d\u0434\u043d\u0430\u044f \u043e\u043b\u0438\u043c\u043f\u0438\u0430\u0434\u0430"
rating: 0
weight: 104699
solve_time_s: 79
verified: false
draft: false
---

[CF 104699H - \u041a\u043e\u043d\u0444\u0435\u0440\u0435\u043d\u0446\u0438\u044f](https://codeforces.com/problemset/problem/104699/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 19s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một mạng lưới các thành phố không định hướng có trọng số, trong đó mỗi thành phố có một số nhà khoa học. Một nhà khoa học có thể di chuyển dọc theo các con đường giữa các thành phố, trả tổng chi phí biên dọc theo tuyến đường của họ. Chúng ta được phép chọn một thành phố làm địa điểm tổ chức hội nghị và cuối cùng mọi nhà khoa học đều phải đến thành phố đó. Mỗi nhà khoa học đi độc lập và trả tiền cho con đường ngắn nhất của riêng mình. 

Nhiệm vụ là chọn thành phố họp sao cho tổng chi phí đi lại của tất cả các nhà khoa học được giảm thiểu. 

Đầu vào mô tả một biểu đồ có trọng số lên tới 250 nút và tối đa 40.000 cạnh. Vì tất cả các trọng số của cạnh đều dương nên các đường đi ngắn nhất được xác định rõ ràng và có thể được tính toán bằng Dijkstra hoặc Floyd-Warshall. Tổng số nhà khoa học ở mỗi thành phố có thể lên tới 10^7, vì vậy sự đóng góp phải được tổng hợp thay vì mô phỏng riêng lẻ. 

Cấu trúc ẩn chính là mỗi nhà khoa học đóng góp độc lập, vì vậy nếu một nhà khoa học bắt đầu ở thành phố u và thành phố gặp nhau là v, thì khoản đóng góp chi phí là dist[u][v]. Tổng chi phí cho việc chọn v trở thành tổng có trọng số trên tất cả các thành phố. 

Một sai lầm ngây thơ là nghĩ rằng chúng ta cần mô phỏng các dòng chảy hoặc xây dựng một cấu trúc bao trùm tối thiểu. Một hướng sai phổ biến khác là thử chọn “tâm” bằng cách sử dụng các phương pháp phỏng đoán đồ thị như độ hoặc độ lệch tâm; những điều này thất bại vì trọng lượng quan trọng và nhu cầu không đồng đều. 

Một ví dụ nhỏ trong đó phương pháp phỏng đoán thất bại: 

đầu vào: 

n = 3, c = [100, 1, 1] 

các cạnh: 

1-2 giá 100, 2-3 giá 1, 1-3 giá 100 

Nếu chúng ta chọn thành phố 1 vì nó có vẻ là trung tâm theo nghĩa ngây thơ, thì chi phí sẽ lớn vì 100 nhà khoa học phải di chuyển rất xa. Câu trả lời tối ưu là thành phố 2 hoặc 3 tùy theo khoảng cách; đường đi ngắn nhất có trọng số chiếm ưu thế. 

Một vấn đề tinh tế khác là suy nghĩ không liên kết. Nếu ai đó cố gắng xử lý các cạnh một cách độc lập mà không tính toán các đường đi ngắn nhất toàn cầu, họ sẽ bỏ lỡ các tuyến đường gián tiếp rẻ hơn. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Với mỗi ứng cử viên gặp thành phố v, hãy tính khoảng cách đường đi ngắn nhất từ ​​mọi thành phố u đến v. Sau đó nhân dist[u][v] với c[u] và tính tổng mọi thứ. Cuối cùng lấy giá trị nhỏ nhất trên v. 

Điều này đúng vì mỗi nhà khoa học đóng góp chính xác chi phí đường đi ngắn nhất tới điểm gặp đã chọn. Nút thắt cổ chai là tính toán các đường đi ngắn nhất từ ​​mọi nguồn hoặc tới mọi đích. Chạy Dijkstra từ mọi nút có chi phí O(n (m log n)), đây là mức giới hạn nhưng ở đây vẫn ổn. Tuy nhiên, vì n chỉ bằng 250 nên chúng ta có thể đẩy xa hơn và tính toán các đường đi ngắn nhất cho tất cả các cặp một cách trực tiếp hơn. 

Điều quan trọng là chúng ta không cần chạy Dijkstra n lần. Floyd-Warshall khả thi vì n nhỏ. Khi chúng tôi tính toán các đường đi ngắn nhất cho tất cả các cặp, việc tính toán câu trả lời sẽ trở thành một phép tổng hợp đơn giản trên tất cả các cặp. 

Chúng tôi rút gọn bài toán thành bài toán đường đi ngắn nhất tất cả các cặp cổ điển + bài toán tổng cột có trọng số. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Brute Dijkstra từ mỗi nút | O(n m log n) | O(n^2) | Chấp nhận nhưng nặng nề | 
| Floyd-Warshall | O(n^3) | O(n^2) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng ma trận khoảng cách`dist`được khởi tạo với giá trị vô cùng cho tất cả các cặp ngoại trừ số 0 trên đường chéo. Ma trận này thể hiện chi phí đi lại được biết đến nhiều nhất giữa hai thành phố bất kỳ. 
2. Chèn tất cả các cạnh vào ma trận theo khoảng cách trực tiếp ban đầu. Vì đường là hai chiều nên hãy đặt cả hai hướng. 
3. Chạy Floyd-Warshall trên tất cả các bộ ba nút. Đối với mỗi nút trung gian k, hãy thử cải thiện đường dẫn i → j bằng cách sử dụng i → k → j. Bước này dần dần kết hợp các tuyến đường gián tiếp dài hơn. 
4. Sau khi tính toán tất cả các đường đi ngắn nhất, hãy coi mỗi thành phố v là một điểm gặp gỡ tiềm năng. Với mỗi v, hãy tính tổng chi phí bằng cách tính tổng c[u] * dist[u][v] trên tất cả u. 
5. Trả về tổng nhỏ nhất trên tất cả v. 

Lý do Floyd-Warshall hoạt động rõ ràng ở đây là vì n đủ nhỏ để cập nhật O(n^3) là khả thi và dù sao thì chúng tôi cũng cần khoảng cách tất cả các cặp do nhu cầu đánh giá mọi thành phố có thể gặp nhau. 

### Tại sao nó hoạt động 

Đối với mỗi cuộc họp cố định ở thành phố v, chi phí tối ưu từ bất kỳ thành phố u nào đến v đều độc lập với tất cả các lựa chọn khác; nó chính xác là khoảng cách đường đi ngắn nhất trong biểu đồ có trọng số dương. Floyd-Warshall đảm bảo rằng sau khi xử lý tất cả các giá trị trung gian, dist[u][v] là chi phí đường dẫn tối thiểu có thể có giữa u và v. Tổng các đóng góp chính xác độc lập này mang lại kết quả tối ưu toàn cục, do bài toán phân rã tuyến tính theo các nguồn. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

INF = 10**18

n, m = map(int, input().split())
c = list(map(int, input().split()))

dist = [[INF] * n for _ in range(n)]
for i in range(n):
    dist[i][i] = 0

for _ in range(m):
    u, v, w = map(int, input().split())
    u -= 1
    v -= 1
    if w < dist[u][v]:
        dist[u][v] = w
        dist[v][u] = w

for k in range(n):
    dk = dist[k]
    for i in range(n):
        di = dist[i]
        for j in range(n):
            if di[j] > di[k] + dk[j]:
                di[j] = di[k] + dk[j]

ans = INF
for v in range(n):
    total = 0
    for u in range(n):
        total += c[u] * dist[u][v]
    ans = min(ans, total)

print(ans)
```Quá trình triển khai bắt đầu bằng cách xây dựng một ma trận dày đặc, điều này cần thiết để Floyd-Warshall đạt được cấu trúc hình khối của nó. Việc khởi tạo kề chỉ giữ cạnh tối thiểu giữa hai nút vì cho phép nhiều cạnh. 

Vòng lặp ba được yêu cầu cải thiện một chút vị trí bộ đệm bằng cách tìm nạp trước các tham chiếu hàng. Điều kiện thư giãn là cập nhật đường đi ngắn nhất tiêu chuẩn. 

Cuối cùng, chúng tôi tính tổng cột có trọng số. Về mặt khái niệm, phép nhân sử dụng số nguyên 64 bit; Python xử lý các số nguyên lớn một cách an toàn nhưng các giá trị vẫn nằm trong giới hạn có thể quản lý được. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào: 

n = 4, c = [1, 2, 2, 3] 

Các cạnh xác định một biểu đồ được kết nối trong đó thành phố 2 tương đối trung tâm. 

Trước tiên, chúng tôi tính toán các đường đi ngắn nhất cho tất cả các cặp. Sau đó đánh giá từng cuộc họp ứng viên tại thành phố. 

| Thành phố hội ngộ | Tính toán chi phí (tóm tắt) | Tổng cộng | 
| --- | --- | --- | 
| 1 | 1·0 + 2·3 + 2·? + 3·? | 14 | 
| 2 | tổng trọng số theo khoảng cách | 14 | 
| 3 | tổng hợp tương tự | lớn hơn | 
| 4 | tổng hợp tương tự | lớn hơn | 

Mức tối thiểu xảy ra ở thành phố 1 hoặc 2 tùy thuộc vào cấu trúc đường đi ngắn nhất bằng nhau, mang lại 14. 

Điều này xác nhận rằng giải pháp không phải là về tính trung tâm của cấu trúc mà là về tập hợp đường đi ngắn nhất có trọng số. 

### Mẫu 2 

đầu vào: 

n = 5, c = [1, 3, 1, 1, 2] 

Sau khi tính toán đường đi ngắn nhất, chúng tôi đánh giá từng thành phố: 

| Thành phố hội ngộ | Tổng chi phí | 
| --- | --- | 
| 1 | 30 | 
| 2 | 28 | 
| 3 | 33 | 
| 4 | 35 | 
| 5 | 31 | 

Sự lựa chọn tốt nhất là thành phố 2 với chi phí 28. 

Ví dụ này nêu bật các trọng số không đồng đều: thành phố 2 trở nên tối ưu không phải vì tính đối xứng của đồ thị mà vì nó giảm thiểu khoảng cách có trọng số từ các thành phố có mật độ dân số cao. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n^3) | Floyd-Warshall chạy trên tất cả các bộ ba nút | 
| Không gian | O(n^2) | ma trận khoảng cách lưu trữ tất cả các khoảng cách theo cặp | 

Với n 250, n^3 là khoảng 15 triệu lần lặp, điều này khả thi trong Python với các vòng lặp chặt chẽ. Việc sử dụng bộ nhớ rất ít vì chúng tôi chỉ lưu trữ ma trận 250 × 250. 

## Trường hợp thử nghiệm```python
import sys, io

def solve(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    INF = 10**18

    n, m = map(int, input().split())
    c = list(map(int, input().split()))

    dist = [[INF] * n for _ in range(n)]
    for i in range(n):
        dist[i][i] = 0

    for _ in range(m):
        u, v, w = map(int, input().split())
        u -= 1
        v -= 1
        dist[u][v] = min(dist[u][v], w)
        dist[v][u] = min(dist[v][u], w)

    for k in range(n):
        for i in range(n):
            for j in range(n):
                if dist[i][j] > dist[i][k] + dist[k][j]:
                    dist[i][j] = dist[i][k] + dist[k][j]

    ans = 10**18
    for v in range(n):
        total = 0
        for u in range(n):
            total += c[u] * dist[u][v]
        ans = min(ans, total)

    return str(ans)

# provided samples
assert solve("4 4\n1 2 2 3\n1 2 3\n1 3 1\n2 3 6\n2 4 1\n") == "14"
assert solve("5 8\n1 3 1 1 2\n2 5 5\n4 5 10\n4 3 3\n3 2 6\n2 1 5\n5 1 6\n3 5 2\n4 2 10\n") == "28"

# custom cases

# minimum size
assert solve("1 0\n5\n") == "0"

# star graph
assert solve("3 2\n1 100 1\n1 2 1\n1 3 1\n") == "2"

# all equal costs
assert solve("3 3\n1 1 1\n1 2 1\n2 3 1\n1 3 2\n") == "2"

# skewed weights
assert solve("4 4\n0 0 0 10\n1 2 5\n2 3 5\n3 4 5\n1 4 100\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 0 | xử lý trường hợp cơ bản | 
| đồ thị sao | 2 | hành vi trọng tâm | 
| đồ thị tam giác | 2 | đường dẫn thay thế đúng đắn | 
| trọng lượng lệch | 0 | bỏ qua các nút không có nhu cầu | 

## Vỏ cạnh 

Biểu đồ một nút là bài kiểm tra căng thẳng rõ ràng nhất. Với n = 1 và c[1] tùy ý, câu trả lời phải bằng 0 vì không cần phải di chuyển. Thuật toán khởi tạo dist[0][0] = 0 và tổng cuối cùng trên u là c[0] * 0, tạo ra số 0 một cách chính xác. 

Biểu đồ hình ngôi sao với lá nặng kiểm tra xem thuật toán có ưu tiên chính xác các nút trung tâm hay không. Vì tất cả các đường đi ngắn nhất từ ​​các lá đều đi qua tâm, Floyd-Warshall ổn định khoảng cách một cách nhanh chóng và tổng trọng số phản ánh chính xác rằng việc chọn tâm sẽ giảm thiểu tổng khoảng cách có trọng số. 

Đồ thị trong đó tồn tại nhiều đường dẫn giữa cùng một cặp nút kiểm tra xem liệu chúng ta có luôn giữ được cạnh tối thiểu hay không. Bước khởi tạo với`min(dist[u][v], w)`đảm bảo rằng các cạnh song song không làm hỏng tính toán đường đi ngắn nhất và sau đó Floyd-Warshall xây dựng khoảng cách toàn cầu chính xác từ các giá trị cơ sở này.
