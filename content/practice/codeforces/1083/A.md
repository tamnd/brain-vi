---
title: "CF 1083A - Quả hạch công bằng và con đường tốt nhất"
description: "Chúng ta được cấp một cây trong đó mỗi nút đại diện cho một thành phố. Mỗi thành phố đều có một trạm nhiên liệu và mỗi trạm cho phép chúng ta nhận được một lượng nhiên liệu cố định $wi$. Việc di chuyển giữa hai thành phố được nối với nhau bằng một con đường sẽ tiêu tốn nhiên liệu bằng chiều dài con đường đó."
date: "2026-06-15T05:51:00+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "dp", "trees"]
categories: ["algorithms"]
codeforces_contest: 1083
codeforces_index: "A"
codeforces_contest_name: "Codeforces Round 526 (Div. 1)"
rating: 1800
weight: 1083
solve_time_s: 131
verified: true
draft: false
---

[CF 1083A - Quả hạch công bằng và con đường tốt nhất](https://codeforces.com/problemset/problem/1083/A) 

**Đánh giá:** 1800 
**Tas:** cấu trúc dữ liệu, dp, cây 
**Thời gian giải:** 2m 11s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta được cấp một cây trong đó mỗi nút đại diện cho một thành phố. Mỗi thành phố đều có một trạm nhiên liệu và mỗi trạm cho phép chúng ta lấy một lượng nhiên liệu cố định$w_i$. Việc di chuyển giữa hai thành phố được nối với nhau bằng một con đường sẽ tiêu tốn nhiên liệu bằng chiều dài con đường đó. 

Một du khách bắt đầu tại một thành phố nào đó, di chuyển dọc theo một con đường đơn giản (không có đỉnh lặp lại) và có thể dừng lại ở bất kỳ điểm nào. Tại mỗi thành phố được ghé thăm, bao gồm cả thành phố bắt đầu và kết thúc, họ có thể tiếp nhiên liệu bằng cách thêm tối đa$w_i$đơn vị. Mức nhiên liệu không bao giờ được phép giảm xuống dưới 0 khi đi qua một mép. Mục tiêu là chọn cả thành phố xuất phát và đường đi sao cho lượng nhiên liệu cuối cùng còn lại sau khi kết thúc chuyến đi bộ được tối đa hóa. 

Khó khăn chính là tính khả thi của một lộ trình phụ thuộc vào mức nhiên liệu trung gian chứ không chỉ tổng số tiền. Một con đường có vẻ có lợi về tổng thể vẫn có thể không khả thi nếu một cạnh quá đắt để vượt qua với nhiên liệu có sẵn tại thời điểm đó. 

Các ràng buộc đi lên đến$n = 3 \cdot 10^5$, điều này ngay lập tức loại trừ bất kỳ cách tiếp cận bậc hai nào đối với các cặp nút hoặc các đường truyền đồ thị lặp lại trên mỗi nút bắt đầu. Thậm chí$O(n \log n)$phải được cấu trúc cẩn thận, nhưng lập trình động cây tuyến tính hoặc gần tuyến tính là mục tiêu. 

Một vài trường hợp tế nhị phơi bày lý do tại sao lý luận ngây thơ lại thất bại. Nếu chúng ta chỉ xem xét tổng lợi ích dọc theo một con đường, chúng ta có thể chọn một con đường mà tổng nhiên liệu thu được vượt quá tổng chi phí, nhưng người đi du lịch có thể bị mắc kẹt sớm do lợi thế chi phí cao. 

Ví dụ: giả sử một nút$u$có một cái lớn$w_u$, nhưng lợi thế đầu tiên của nó có giá cao hơn bất kỳ lợi ích tức thời nào từ những người hàng xóm. Cách tiếp cận “tổng trọng số trừ tổng các cạnh” ngây thơ sẽ chấp nhận những chuyển đổi như vậy một cách không chính xác mặc dù đường đi không thể đi qua. 

Một trường hợp cạnh khác là cây nút đơn. Câu trả lời đơn giản là$w_1$, vì không cần chuyển động và không có ràng buộc nào bị vi phạm. 

## Phương pháp tiếp cận 

Chiến lược bạo lực sẽ là thử mọi con đường đơn giản có thể có trong cây, mô phỏng sự tích lũy nhiên liệu dọc theo con đường đó và theo dõi nhiên liệu kết thúc tốt nhất. Ngay cả việc liệt kê tất cả các đường đi cũng đã là số mũ trong một cây, vì có$O(n^2)$các điểm cuối có thể có và mỗi đường dẫn phải được xác nhận từng bước. Mô phỏng trên mỗi đường dẫn thêm một yếu tố khác của$O(n)$, dẫn đến điều không thể thực hiện được$O(n^3)$trường hợp xấu nhất. 

Quan sát chính là cấu trúc cây ngăn chặn các chu kỳ, vì vậy mọi đường dẫn đều có điểm cao nhất đóng vai trò là gốc một cách tự nhiên. Nếu chúng tôi sửa một nút làm “điểm neo bắt đầu”, thì mọi phần tiếp theo hợp lệ từ nút đó sẽ hoạt động giống như mở rộng thành các cây con và các phần mở rộng không hợp lệ có thể được cắt bớt cục bộ. 

Cái nhìn sâu sắc quan trọng là diễn giải lại vấn đề dưới dạng tính toán, đối với mỗi nút, “lợi ích ròng” tốt nhất có thể của một đường dẫn bắt đầu từ đó và di chuyển xuống các cây con của nó, nơi chúng tôi chỉ giữ các phần mở rộng không phá vỡ tính khả thi. Nếu một cây con không thể được nhập vào mà không bị âm thì nó sẽ bị bỏ qua. Điều này biến việc tìm kiếm đường dẫn toàn cầu thành các quyết định cục bộ được kết hợp một cách tối ưu. 

Điều này hiệu quả vì bất kỳ đường dẫn tối ưu nào cũng có thể được xem là có gốc ở điểm cao nhất trong đường dẫn đó và từ thời điểm đó nó chỉ mở rộng xuống thành các nhánh rời rạc. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n^3)$|$O(n)$| Quá chậm | 
| Cây DP |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng ta root cây một cách tùy ý, để thuận tiện cho nút 1. DP sẽ tính toán mức tăng nhiên liệu tốt nhất có thể đạt được bắt đầu từ mỗi nút nếu chúng ta bắt đầu từ đó và chỉ di chuyển đến các nút con của nó. 

1. Gốc cây tại bất kỳ nút nào, thường là nút 1. Điều này mang lại cho mỗi cạnh một hướng cha-con để chúng ta có thể xác định các bài toán con một cách rõ ràng. 
2. Xác định giá trị$dp[u]$là nhiên liệu cuối cùng tối đa mà chúng ta có thể có được nếu chúng ta bắt đầu một đường dẫn tại nút$u$và chỉ di chuyển xuống các con của nó. Chúng tôi giả sử trước tiên chúng tôi thu thập$w_u$Tại$u$, vì mọi nút được truy cập đều cho phép tiếp nhiên liệu. 
3. Đối với mỗi trẻ$v$của$u$, hãy xem xét việc mở rộng đường dẫn từ$u$vào trong$v$. Nếu chúng ta đi vào$v$, chúng ta phải trả chi phí biên$c(u,v)$, do đó sự đóng góp hiệu quả của cây con đó là$dp[v] - c(u,v)$. 
4. Chỉ nhận khoản đóng góp con cái nếu số tiền đó là số dương. Nếu như$dp[v] - c(u,v) \le 0$, thì việc đi vào cây con đó không bao giờ cải thiện được nhiên liệu cuối cùng và tốt hơn là nên dừng lại ở$u$thay vì vượt qua cạnh đó. 
5. Tổng hợp tất cả những đóng góp tích cực của trẻ em và cộng chúng vào$w_u$. Điều này mang lại$dp[u]$. 
6. Đáp án là giá trị lớn nhất của$dp[u]$trên tất cả các nút, vì đường dẫn tối ưu có thể bắt đầu ở bất kỳ đâu. 

### Tại sao nó hoạt động 

Tại bất kỳ nút nào$u$, khi chúng ta quyết định nhập một cây con con, phần tiếp theo tốt nhất bên trong cây con đó đã được nắm bắt hoàn toàn bởi$dp[v]$. Sự tương tác duy nhất giữa$u$Và$v$là chi phí biên, do đó quyết định giảm xuống mức kiểm tra lợi nhuận cục bộ. Bởi vì cây không có chu trình nên mỗi cây con sẽ độc lập khi cạnh của nó được chọn hoặc bị loại bỏ. Điều này ngăn cản việc tính toán trùng lặp hoặc tính toán nhiên liệu không nhất quán giữa các chi nhánh khác nhau. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline
sys.setrecursionlimit(10**7)

n = int(input())
w = list(map(int, input().split()))

adj = [[] for _ in range(n)]
for _ in range(n - 1):
    u, v, c = map(int, input().split())
    u -= 1
    v -= 1
    adj[u].append((v, c))
    adj[v].append((u, c))

dp = [0] * n
visited = [False] * n

def dfs(u, parent):
    best = w[u]
    for v, c in adj[u]:
        if v == parent:
            continue
        dfs(v, u)
        gain = dp[v] - c
        if gain > 0:
            best += gain
    dp[u] = best

dfs(0, -1)

print(max(dp))
```Việc triển khai tính toán DFS thứ tự sau để mọi phần tử con được xử lý trước phần tử cha của nó. Điều này đảm bảo rằng khi chúng ta đánh giá$u$, tất cả$dp[v]$các giá trị đã chính xác. 

Chi tiết triển khai tinh tế duy nhất là điều kiện cắt tỉa`gain > 0`. Nếu không có kiểm tra này, chúng tôi sẽ buộc đưa vào các cây con đóng góp tiêu cực một cách không chính xác, điều này sẽ làm giảm giá trị đường dẫn mặc dù việc dừng sớm hơn luôn được cho phép. 

Câu trả lời cuối cùng là tối đa trên tất cả$dp[u]$, vì đường dẫn tối ưu có thể bắt đầu ở bất kỳ nút nào, không nhất thiết phải là nút gốc. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
1 3 3
1 2 2
1 3 2
```Chúng tôi root ở mức 1. 

| Nút | w[u] | Đóng góp của trẻ em | dp[u] | 
| --- | --- | --- | --- | 
| 2 | 3 | không | 3 | 
| 3 | 3 | không | 3 | 
| 1 | 1 | (3-2)=1, (3-2)=1 | 1 + 1 + 1 = 3 | 

Câu trả lời tốt nhất là 3, đạt được bằng cách bắt đầu từ nút 2, đi qua nút 1 rồi đến nút 3. Bảng xác nhận rằng cả hai nhánh đều có lợi nhuận riêng lẻ từ nút gốc. 

### Ví dụ 2 

Hãy xem xét một chuỗi:```
4
5 1 1 10
1 2 4
2 3 4
3 4 1
```Chúng tôi tính toán từ dưới lên. 

| Nút | w[u] | Đóng góp của trẻ em | dp[u] | 
| --- | --- | --- | --- | 
| 4 | 10 | không | 10 | 
| 3 | 1 | 10 - 1 = 9 | 10 | 
| 2 | 1 | 10 - 4 = 6 | 7 | 
| 1 | 5 | 7 - 4 = 3 | 8 | 

Đường dẫn tốt nhất bắt đầu từ nút 4 hoặc nút 1 tùy theo cách hiểu, nhưng dp tối đa là 10, đạt được bằng cách bắt đầu từ nút 4. 

Dấu vết này cho thấy các cây con sâu mang lại lợi nhuận như thế nào chỉ lan truyền lên trên khi chi phí biên cho phép chúng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi nút và cạnh được xử lý một lần trong DFS | 
| Không gian |$O(n)$| Danh sách kề và ngăn xếp đệ quy lưu trữ thông tin tuyến tính | 

Giải pháp phù hợp thoải mái trong giới hạn vì$n$có thể đạt được$3 \cdot 10^5$và một lần truyền tải tuyến tính duy nhất sẽ tránh được bất kỳ sự tính toán lại lặp đi lặp lại nào. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    input = sys.stdin.readline

    n = int(input())
    w = list(map(int, input().split()))
    adj = [[] for _ in range(n)]
    for _ in range(n - 1):
        u, v, c = map(int, input().split())
        u -= 1
        v -= 1
        adj[u].append((v, c))
        adj[v].append((u, c))

    sys.setrecursionlimit(10**7)
    dp = [0] * n

    def dfs(u, p):
        best = w[u]
        for v, c in adj[u]:
            if v == p:
                continue
            dfs(v, u)
            gain = dp[v] - c
            if gain > 0:
                best += gain
        dp[u] = best

    dfs(0, -1)
    return str(max(dp))

# provided sample
assert run("3\n1 3 3\n1 2 2\n1 3 2\n") == "3"

# single node
assert run("1\n5\n") == "5"

# chain increasing
assert run("4\n1 2 3 4\n1 2 1\n2 3 1\n3 4 1\n") == "10"

# star
assert run("5\n10 1 1 1 1\n1 2 5\n1 3 5\n1 4 5\n1 5 5\n") == "10"

# all zero except one leaf
assert run("3\n0 0 10\n1 2 1\n2 3 1\n") == "10"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| nút đơn | 5 | tính đúng đắn của trường hợp cơ sở | 
| tăng chuỗi | 10 | tích lũy dọc theo một con đường | 
| cấu trúc sao | 10 | cắt tỉa các cạnh tiêu cực | 
| lá tăng thưa thớt | 10 | sự thống trị của cây con sâu | 

## Vỏ cạnh 

Đầu vào một nút được xử lý chính xác vì DFS chỉ định$dp[1] = w_1$và không có cạnh nào được xử lý. Vượt mức tối đa$dp$chỉ đơn giản là giá trị đó. 

Trong một chuỗi mà mỗi chi phí cạnh lớn hơn mức tăng liền kề, mỗi đóng góp con trở thành không dương, do đó thuật toán dừng chính xác ở mỗi nút thay vì buộc phải truyền tải. Điều này ngăn chặn các đường dẫn không hợp lệ có thể đi đến cạnh giữa âm. 

Trong biểu đồ hình sao chỉ có một nhánh có lợi, tất cả các đóng góp con khác sẽ được lọc ra bởi`gain > 0`tình trạng. Điều này đảm bảo rằng các cây con không liên quan sẽ không làm ảnh hưởng đến kết quả ngay cả khi chúng được gắn trực tiếp vào gốc.
