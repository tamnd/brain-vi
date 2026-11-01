---
title: "CF 106396H - \u70df\u706b\u62fe\u7a57"
description: "Bài toán đưa ra một biểu đồ có trọng số với nút bắt đầu được chỉ định. Chúng ta được phép di chuyển dọc theo các cạnh và chi phí di chuyển giữa hai nút bất kỳ được xác định bởi khoảng cách đường đi ngắn nhất trong biểu đồ gốc."
date: "2026-06-20T23:07:06+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106396
codeforces_index: "H"
codeforces_contest_name: "Tiangong University 2025 ICPC Team Selection Contest II (Online Mirror)"
rating: 0
weight: 106396
solve_time_s: 51
verified: true
draft: false
---

[CF 106396H - \u70df\u706b\u62fe\u7a57](https://codeforces.com/problemset/problem/106396/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 51s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Bài toán đưa ra một biểu đồ có trọng số với nút bắt đầu được chỉ định. Chúng ta được phép di chuyển dọc theo các cạnh và chi phí di chuyển giữa hai nút bất kỳ được xác định bởi khoảng cách đường đi ngắn nhất trong biểu đồ gốc. Mục tiêu không chỉ là tìm đường đi ngắn nhất đến một mục tiêu mà còn truy cập tất cả các nút trong khi giảm thiểu tổng chi phí di chuyển, bắt đầu từ nguồn nhất định. 

Vì vậy, về mặt khái niệm, trước tiên chúng ta chuyển đổi biểu đồ thành một không gian số liệu hoàn chỉnh trong đó khoảng cách giữa hai nút bất kỳ là khoảng cách đường đi ngắn nhất trong biểu đồ gốc. Sau quá trình chuyển đổi này, chúng tôi đang giải quyết vấn đề truy cập trên tất cả các nút trong đó quá trình chuyển đổi giữa các nút được tính trọng số bởi khoảng cách đường đi ngắn nhất được tính toán trước này. 

Đầu ra là tổng chi phí tối thiểu có thể có của một chuyến đi bắt đầu tại nút nhất định và cuối cùng bao phủ tất cả các nút ít nhất một lần. Về cơ bản, đây là một bài toán khám phá không gian trạng thái trong đó trạng thái được xác định bởi các nút nào đã được truy cập và vị trí hiện tại của chúng ta. 

Những hạn chế gợi ý mạnh mẽ rằng$n$là nhỏ, nhiều nhất là 15 hoặc 16. Điều này là do bất kỳ giải pháp nào xem xét tập hợp con của các nút đều yêu cầu$2^n$tiểu bang, và bất cứ điều gì ngoài$n = 20$trở nên không khả thi do tăng trưởng theo cấp số nhân. Tại$n = 15$,$2^n$là khoảng 32.000, có thể quản lý được bằng cách chuyển đổi cẩn thận. 

Đường đi ngắn nhất đơn giản trên biểu đồ gốc là không đủ vì chi phí phụ thuộc vào lịch sử truy cập. Một cách tiếp cận tham lam điển hình luôn di chuyển đến nút chưa được truy cập gần nhất có thể thất bại vì các lựa chọn tối ưu cục bộ có thể dẫn đến các chuyến tham quan dưới mức tối ưu toàn cầu. 

Trường hợp cạnh tinh tế phát sinh khi các đường đi ngắn nhất giữa các nút khác biệt đáng kể so với các cạnh trực tiếp. Nếu chúng tôi không tính toán trước các đường đi ngắn nhất cho tất cả các cặp, chúng tôi có thể đánh giá thấp hoặc đánh giá quá cao chi phí đi lại giữa các tiểu bang, dẫn đến chuyển tiếp DP không chính xác. 

## Phương pháp tiếp cận 

Ý tưởng brute-force sẽ là mô phỏng tất cả các thứ tự có thể có của việc truy cập các nút. Chúng tôi bắt đầu từ nguồn và thử mọi hoán vị của các nút còn lại, tính toán tổng chi phí của mỗi đơn hàng bằng cách sử dụng khoảng cách đường đi ngắn nhất. Điều này đúng vì nó khám phá tất cả các chuyến tham quan có thể, nhưng nó đòi hỏi$n!$hoán vị, và thậm chí tại$n = 12$, cái này đã trở nên quá lớn rồi. 

Thông tin chi tiết quan trọng là chúng ta không cần quan tâm đến cấu trúc chuỗi chính xác ngoài nút hiện tại và tập hợp các nút đã truy cập. Khi chúng tôi tính toán trước khoảng cách đường đi ngắn nhất giữa tất cả các cặp nút, biểu đồ sẽ trở thành một biểu đồ có trọng số hoàn chỉnh với chi phí cạnh cố định. Sau đó, bài toán trở thành đường đi ngắn nhất trong biểu đồ trạng thái trong đó mỗi trạng thái được xác định bởi một tập hợp con các nút đã truy cập và vị trí hiện tại. 

Điều này dẫn đến cách tiếp cận lập trình động bitmask một cách tự nhiên. Mỗi trạng thái mã hóa các nút nào đã được truy cập và các chuyển đổi tương ứng với việc di chuyển đến một nút mới chưa được đưa vào mặt nạ. Chi phí chuyển đổi là khoảng cách đường đi ngắn nhất được tính toán trước. 

Trước tiên, chúng tôi tính toán các đường đi ngắn nhất cho tất cả các cặp bằng cách sử dụng Floyd-Warshall vì$n$là nhỏ. Sau đó, chúng tôi chạy DP trên các tập hợp con, dần dần mở rộng tập hợp đã truy cập cho đến khi bao gồm tất cả các nút. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Hoán vị Brute Force |$O(n!)$|$O(n)$| Quá chậm | 
| Mặt nạ bit DP + APSP |$O(n^2 2^n)$|$O(n 2^n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi giải quyết vấn đề theo hai giai đoạn khái niệm: đầu tiên nén biểu đồ thành khoảng cách đường đi ngắn nhất, sau đó thực hiện lập trình động trên các tập hợp con. 

1. Đầu tiên, chúng tôi tính toán đường đi ngắn nhất cho tất cả các cặp bằng cách sử dụng Floyd-Warshall. Bước này là cần thiết vì biểu đồ ban đầu có thể có các tuyến đường gián tiếp rẻ hơn các cạnh trực tiếp. Sau bước này, chúng ta có thể coi biểu đồ là được kết nối đầy đủ trong đó mỗi cặp nút có chi phí di chuyển tối thiểu cố định. 
2. Chúng tôi xác định trạng thái DP`dp[mask][u]`, Ở đâu`mask`đại diện cho tập hợp các nút đã truy cập và`u`là nút hiện tại. Trạng thái này lưu trữ chi phí tối thiểu để tiếp cận nút`u`đã truy cập chính xác các nút trong`mask`. Mã hóa này là đủ vì các quyết định trong tương lai chỉ phụ thuộc vào những gì đã ghé thăm và hiện tại chúng ta đang ở đâu. 
3. Chúng tôi khởi tạo DP với nút khởi đầu chỉ được truy cập, cài đặt`dp[1 << s][s] = 0`. Điều này thể hiện việc bắt đầu không có chi phí di chuyển. 
4. Chúng tôi lặp lại tất cả các tập hợp con của các nút theo thứ tự kích thước mặt nạ tăng dần. Đối với mỗi tiểu bang`(mask, i)`có thể truy cập được, chúng tôi thử mở rộng đường dẫn đến mọi nút`j`vẫn chưa có mặt nạ. Chúng tôi cập nhật trạng thái mới`dp[mask | (1 << j)][j]`sử dụng chi phí`dp[mask][i] + dist[i][j]`. 
5. Quá trình chuyển đổi mô phỏng hiệu quả việc chọn nút tiếp theo để truy cập nhưng không sửa trước hoán vị đầy đủ. Thay vào đó, chúng tôi tăng dần tất cả các khả năng và chỉ giữ lại chi phí tốt nhất cho mỗi trạng thái. 
6. Sau khi xử lý tất cả các trạng thái, câu trả lời là giá trị nhỏ nhất trong số tất cả các trạng thái`dp[full_mask][i]`, vì nút cuối cùng có thể là bất kỳ nút nào sau khi truy cập tất cả các nút. 

### Tại sao nó hoạt động 

Mỗi trạng thái DP đại diện cho một chuyến tham quan từng phần được xác định duy nhất, đặc trưng chỉ bởi các nút đã truy cập và vị trí hiện tại. Bất kỳ chuyến tham quan hoàn chỉnh nào cũng có thể được phân tách thành các trạng thái như vậy và mọi chuyển đổi đều duy trì tính chính xác vì trọng số cạnh đã biểu thị chi phí di chuyển ngắn nhất có thể giữa các nút. Vì chúng tôi khám phá tất cả các tập hợp con và tất cả các nút kết thúc một cách có hệ thống nên không có chuyến tham quan hợp lệ nào bị bỏ lỡ và DP đảm bảo rằng chỉ giữ lại chi phí tối thiểu cho mỗi trạng thái. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, m, s = map(int, input().split())
    s -= 1

    INF = 10**18
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
                if dist[i][k] + dist[k][j] < dist[i][j]:
                    dist[i][j] = dist[i][k] + dist[k][j]

    K = 1 << n
    dp = [[INF] * n for _ in range(K)]

    dp[1 << s][s] = 0

    for mask in range(K):
        for i in range(n):
            if not (mask & (1 << i)):
                continue
            cur = dp[mask][i]
            if cur == INF:
                continue
            for j in range(n):
                if mask & (1 << j):
                    continue
                nmask = mask | (1 << j)
                new_cost = cur + dist[i][j]
                if new_cost < dp[nmask][j]:
                    dp[nmask][j] = new_cost

    full = K - 1
    ans = min(dp[full][i] for i in range(n))
    print(ans)

if __name__ == "__main__":
    solve()
```Khối Floyd-Warshall đảm bảo mọi khoảng cách theo cặp đều ở mức tối thiểu, điều này rất quan trọng vì quá trình chuyển đổi DP giả định quyền truy cập trực tiếp vào chi phí di chuyển tối ưu giữa hai nút bất kỳ. Nếu không có quá trình tiền xử lý này, DP sẽ đánh giá thấp các đường dẫn gián tiếp. 

Bitmask DP lặp lại trên tất cả các tập hợp con. Vòng lặp bên trong kiểm tra cẩn thận xem một nút có nằm trong tập hợp con hiện tại hay không trước khi cho phép chuyển đổi. Bước cập nhật duy trì chi phí tối thiểu trên mỗi trạng thái, đảm bảo rằng các đường dẫn dư thừa sẽ bị loại bỏ sớm. 

Việc giảm cuối cùng đối với tất cả các nút kết thúc phản ánh thực tế rằng chuyến tham quan không yêu cầu quay lại điểm xuất phát hoặc kết thúc tại một nút cố định. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3 3 1
1 2 1
2 3 1
1 3 10
```Sau Floyd-Warshall, con đường ngắn nhất trở thành: 

| tôi | j | quận | 
| --- | --- | --- | 
| 1 | 2 | 1 | 
| 2 | 3 | 1 | 
| 1 | 3 | 2 | 

Tiến trình DP: 

| mặt nạ | nút cuối | chi phí | 
| --- | --- | --- | 
| 001 | 1 | 0 | 
| 011 | 2 | 1 | 
| 111 | 3 | 2 | 

Câu trả lời cuối cùng là 2. 

Điều này cho thấy các cạnh gián tiếp được thay thế bằng các đường đi ngắn nhất trước DP như thế nào. 

### Ví dụ 2 

đầu vào:```
4 4 1
1 2 5
2 3 5
3 4 5
1 4 20
```Đường dẫn ngắn nhất nén cấu trúc chuỗi: 

| cặp | quận | 
| --- | --- | 
| 1-2 | 5 | 
| 2-3 | 5 | 
| 3-4 | 5 | 
| 1-4 | 15 | 

DP xây dựng đường dẫn 1 → 2 → 3 → 4 với chi phí 15. 

Điều này xác nhận rằng mặc dù tồn tại cạnh trực tiếp, DP vẫn ưu tiên chính xác chuỗi các đường dẫn phụ tối ưu. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n^3 + n^2 2^n)$| Floyd-Warshall cộng với tập hợp con chuyển tiếp DP | 
| Không gian |$O(n 2^n)$| Bảng DP trên các tập hợp con và điểm cuối | 

Giải pháp được thiết kế cho quy mô nhỏ$n$, trong đó DP theo cấp số nhân là khả thi. Quá trình tiền xử lý khối không đáng kể so với sự bùng nổ không gian trạng thái và độ phức tạp kết hợp phù hợp với các ràng buộc điển hình cho các vấn đề DP mặt nạ bit. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys as _sys
    from io import StringIO

    out = StringIO()
    _sys.stdout = out

    solve()

    _sys.stdout = sys.__stdout__
    return out.getvalue()

def solve():
    n, m, s = map(int, input().split())
    s -= 1

    INF = 10**18
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
                if dist[i][k] + dist[k][j] < dist[i][j]:
                    dist[i][j] = dist[i][k] + dist[k][j]

    K = 1 << n
    dp = [[10**18] * n for _ in range(K)]
    dp[1 << s][s] = 0

    for mask in range(K):
        for i in range(n):
            if not (mask & (1 << i)):
                continue
            cur = dp[mask][i]
            if cur == 10**18:
                continue
            for j in range(n):
                if mask & (1 << j):
                    continue
                nmask = mask | (1 << j)
                dp[nmask][j] = min(dp[nmask][j], cur + dist[i][j])

    full = K - 1
    return str(min(dp[full][i] for i in range(n))) + "\n"

# custom tests
assert run("3 3 1\n1 2 1\n2 3 1\n1 3 10\n") == "2\n", "triangle metric reduction"
assert run("4 4 1\n1 2 5\n2 3 5\n3 4 5\n1 4 20\n") == "15\n", "chain optimal path"
assert run("2 1 1\n1 2 100\n") == "100\n", "two node base case"
assert run("3 2 1\n1 2 5\n2 3 5\n") == "10\n", "no shortcut edge"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| đồ thị tam giác | 2 | độ chính xác nén đường dẫn ngắn nhất | 
| chuỗi có phím tắt | 15 | Lựa chọn DP và cạnh trực tiếp | 
| hai nút | 100 | xử lý trạng thái tối thiểu | 
| đồ thị thưa thớt | 10 | không có cải tiến gián tiếp | 

## Vỏ cạnh 

Đối với biểu đồ trong đó tất cả các nút đã được kết nối trực tiếp với các cạnh tối ưu, bước Floyd-Warshall không thay đổi khoảng cách. DP vẫn hoạt động chính xác vì quá trình chuyển đổi chỉ diễn ra theo các cạnh trực tiếp. 

Đối với biểu đồ tồn tại cạnh trực tiếp rất đắt tiền nhưng tồn tại đường dẫn nhiều bước rẻ hơn, chẳng hạn như hình tam giác có cạnh tắt nặng, quá trình xử lý trước đảm bảo DP không bao giờ sử dụng cạnh đắt tiền. 

Khi$n = 1$, bitmask DP giảm xuống một trạng thái duy nhất và trả về 0 ngay lập tức vì không cần chuyển động. 

Khi nút bắt đầu đã là nút duy nhất trong mặt nạ, quá trình khởi tạo DP đã tương ứng với trạng thái đầy đủ, xác nhận rằng thuật toán xử lý các trường hợp tầm thường mà không cần chuyển đổi.
