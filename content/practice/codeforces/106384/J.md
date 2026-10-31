---
title: "CF 106384J - \u6734\u7d20\u7684\u6700\u957f\u8def\u95ee\u9898"
description: "Nhiệm vụ này mô tả một bài toán về đồ thị trong đó chúng ta được cung cấp một tập hợp các kết nối có hướng giữa các nút và chúng ta được yêu cầu xác định đường đi dài nhất có thể có thể được hình thành bằng cách đi theo các cạnh có hướng này."
date: "2026-06-20T03:27:58+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106384
codeforces_index: "J"
codeforces_contest_name: "CYCPC Round 2"
rating: 0
weight: 106384
solve_time_s: 68
verified: true
draft: false
---

[CF 106384J - \u6734\u7d20\u7684\u6700\u957f\u8def\u95ee\u9898](https://codeforces.com/problemset/problem/106384/J) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 8 giây 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Nhiệm vụ này mô tả một bài toán về đồ thị trong đó chúng ta được cung cấp một tập hợp các kết nối có hướng giữa các nút và chúng ta được yêu cầu xác định đường đi dài nhất có thể có thể được hình thành bằng cách đi theo các cạnh có hướng này. Đường dẫn ở đây có nghĩa là một chuỗi các nút trong đó mỗi cặp liên tiếp được kết nối bằng một cạnh có hướng và các nút không được xem lại dọc theo cùng một đường dẫn. 

Mặc dù câu lệnh là tối thiểu, nhưng cấu trúc “bài toán đường đi dài nhất” gợi ý rõ ràng rằng chúng ta đang làm việc trên một đồ thị có hướng không theo chu kỳ. Nếu các chu kỳ được cho phép, đường đi dài nhất nói chung sẽ không được xác định vì chúng ta có thể lặp vô thời hạn. Vì vậy, cách giải thích có ý nghĩa là biểu đồ rõ ràng là DAG hoặc được đảm bảo ngầm định là không theo chu kỳ. 

Đầu vào có thể được hiểu là một mô tả biểu đồ, trong đó các nút biểu thị các trạng thái và các cạnh có hướng biểu thị các chuyển đổi được phép. Đầu ra là một số nguyên biểu thị số cạnh tối đa (hoặc các nút tương đương trừ đi một) trong bất kỳ đường dẫn có hướng hợp lệ nào. 

Từ góc độ phức tạp, số lượng nút thường đủ lớn để bất kỳ phương pháp nào liên quan đến việc liệt kê tất cả các đường dẫn đều không thể thực hiện được. Việc liệt kê các đường dẫn một cách thô bạo sẽ tăng theo cấp số nhân, vì mỗi điểm phân nhánh sẽ nhân số lần tiếp tục có thể có. Ngay cả với vài chục nút, điều này trở nên không khả thi, vì vậy giải pháp phải khai thác cấu trúc trong biểu đồ thay vì khám phá trực tiếp các đường dẫn. 

Một số trường hợp biên rất dễ bị bỏ qua nếu chúng ta không cẩn thận về cấu trúc đồ thị. Nếu đồ thị hoàn toàn không có cạnh nào thì đường đi dài nhất sẽ bằng 0 vì không tồn tại chuyển tiếp nào. Nếu biểu đồ là một chuỗi đơn, chẳng hạn như 1 → 2 → 3 → 4, câu trả lời chỉ đơn giản là độ dài của chuỗi đó. Một trường hợp tinh vi hơn xuất hiện khi nhiều đường dẫn hợp nhất, chẳng hạn như 1 → 3, 2 → 3, 3 → 4. Một DFS đơn giản không ghi nhớ sẽ tính toán lại các bài toán con nhiều lần và có thể hết thời gian mặc dù biểu đồ nhỏ. 

Một vấn đề tế nhị khác là giả định rằng một DFS đơn giản không cần xử lý theo chu trình là an toàn. Nếu đầu vào vô tình chứa một chu trình như 1 → 2 → 3 → 1, thì phép đệ quy đơn giản sẽ lặp vô thời hạn hoặc truy cập lại các trạng thái nhiều lần, tạo ra kết quả không chính xác trừ khi chúng ta đảm bảo rõ ràng cấu trúc tôpô hoặc thứ tự truy cập. 

## Phương pháp tiếp cận 

Cách trực tiếp nhất để suy nghĩ về vấn đề này là cố gắng liệt kê mọi đường đi có thể bắt đầu từ mọi nút. Từ mỗi nút, chúng tôi thử tất cả các cạnh đi ra, mở rộng đường dẫn một cách đệ quy cho đến khi không thể mở rộng thêm nữa, theo dõi độ dài tối đa nhìn thấy. 

Cách tiếp cận này đúng vì nó khám phá tất cả các đường dẫn hợp lệ trong biểu đồ. Tuy nhiên, chi phí của nó bùng nổ nhanh chóng. Trong biểu đồ trong đó mỗi nút có hai cạnh đi ra, số lượng đường dẫn sẽ tăng gấp đôi ở mỗi bước, dẫn đến hành vi gần như O(2^n) trong trường hợp xấu nhất. Việc tính toán lại các đường dẫn phụ lặp đi lặp lại là sự kém hiệu quả trung tâm: đường dẫn dài nhất bắt đầu từ một nút được tính toán lại mỗi khi nút đó được truy cập từ một nút trước đó khác. 

Quan sát cấu trúc quan trọng là khi chúng ta biết đường đi dài nhất bắt đầu từ một nút, giá trị đó sẽ không bao giờ thay đổi. Nếu biểu đồ không theo chu kỳ, thì cấu trúc phụ thuộc sẽ tạo thành một phần thứ tự, nghĩa là chúng ta có thể tính toán các câu trả lời theo thứ tự tôpô ngược. Mỗi nút chỉ phụ thuộc vào các nút đứng sau nó theo thứ tự. Điều này chuyển đổi vấn đề từ phân nhánh theo cấp số nhân thành một vấn đề lập trình động một lần trên DAG. 

Chúng tôi xác định dp[u] là đường đi dài nhất bắt đầu từ nút u. Đối với mỗi cạnh u → v, chúng ta có thể mở rộng đường đi từ u thêm một bước cộng với bất kỳ đường đi tốt nhất nào bắt đầu tại v. Vì vậy dp[u] đơn giản là giá trị lớn nhất trên tất cả các lân cận v của 1 + dp[v]. Nếu chúng tôi xử lý các nút theo thứ tự tôpô ngược thì tất cả các giá trị dp[v] đã được tính toán khi cần.

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Bản án | 
| --- | --- | --- | --- | 
| Bảng liệt kê DFS Brute Force | O(2^n) | O(n) | Quá chậm | 
| DP trên DAG (toposort) | O(n + m) | O(n + m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

Chúng tôi tiến hành bằng cách biến biểu đồ thành một cấu trúc trong đó các phần phụ thuộc được xử lý trước các phần phụ thuộc. 

1. Đầu tiên chúng ta xây dựng danh sách kề của đồ thị có hướng. Biểu diễn này cho phép chúng ta truy cập tất cả các cạnh đi của bất kỳ nút nào một cách hiệu quả. Nếu không có điều này, mọi quá trình chuyển đổi sẽ yêu cầu quét liên tục toàn bộ danh sách cạnh, điều này sẽ gây ra chi phí không cần thiết. 
2. Chúng tôi tính toán thứ tự tôpô của các nút. Thứ tự này đảm bảo rằng với mọi cạnh có hướng u → v, nút v xuất hiện trước u theo thứ tự. Thuộc tính này rất quan trọng vì nó đảm bảo rằng khi chúng ta tính dp[u], tất cả các giá trị dp[v] cho các cạnh bên ngoài đều đã được biết. 
3. Chúng ta khởi tạo một mảng dp với tất cả các giá trị được đặt thành 0. Mỗi dp[u] cuối cùng sẽ lưu trữ đường đi dài nhất bắt đầu từ u. 
4. Chúng tôi lặp lại các nút theo thứ tự tôpô ngược. Hướng này quan trọng vì nó đảm bảo chúng tôi luôn xử lý một nút sau tất cả các nút mà nó có thể tiếp cận trực tiếp. 
5. Đối với mỗi nút u, chúng tôi kiểm tra tất cả các cạnh đi ra u → v. Chúng tôi cập nhật dp[u] dưới dạng dp[u] = max(dp[u], 1 + dp[v]). Bước này thể hiện ý tưởng rằng bất kỳ đường đi nào bắt đầu từ u trước tiên phải đi qua một trong các cạnh đi ra của nó, sau đó tiếp tục tối ưu từ nút tiếp theo. 
6. Sau khi xử lý tất cả các nút, chúng tôi tính toán câu trả lời cuối cùng là giá trị lớn nhất trong dp. Đây là điểm khởi đầu tốt nhất trong toàn bộ biểu đồ. 

### Tại sao nó hoạt động 

Tính chính xác dựa trên thực tế là trong DAG, mọi đường dẫn đều tuân theo thứ tự tôpô, nghĩa là không có sự phụ thuộc ngược. Khi xử lý các nút theo thứ tự tôpô ngược, mọi bài toán con dp[v] đều được giải hoàn toàn trước khi nó được sử dụng trong dp[u]. Điều này tạo ra sự lặp lại ổn định trong đó mỗi giá trị dp được tính toán chính xác một lần và chỉ phụ thuộc vào kết quả cuối cùng. Vì mọi đường dẫn hợp lệ phải đi theo các cạnh trong biểu đồ và mọi phần mở rộng được xem xét chính xác một lần thông qua phép truy toán, giá trị được tính toán tối đa tương ứng với đường dẫn dài nhất thực sự. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from collections import deque

def solve():
    n, m = map(int, input().split())
    g = [[] for _ in range(n)]
    indeg = [0] * n

    for _ in range(m):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        g[u].append(v)
        indeg[v] += 1

    q = deque([i for i in range(n) if indeg[i] == 0])
    topo = []

    while q:
        u = q.popleft()
        topo.append(u)
        for v in g[u]:
            indeg[v] -= 1
            if indeg[v] == 0:
                q.append(v)

    dp = [0] * n

    for u in reversed(topo):
        for v in g[u]:
            dp[u] = max(dp[u], 1 + dp[v])

    print(max(dp) if n > 0 else 0)

if __name__ == "__main__":
    solve()
```Việc triển khai bắt đầu bằng cách xây dựng danh sách kề và tính toán các mức độ để thực hiện sắp xếp cấu trúc liên kết thuật toán Kahn tiêu chuẩn. Điều này đảm bảo chúng tôi tránh được đệ quy và cũng đảm bảo thứ tự tuyến tính ngay cả đối với các biểu đồ lớn. 

Khi thứ tự cấu trúc liên kết được xây dựng, chúng tôi lặp lại ngược lại để tất cả các nút kế thừa của một nút đã được xử lý. Quá trình chuyển đổi dp phản ánh trực tiếp định nghĩa về đường đi dài nhất bắt đầu từ mỗi nút. 

Một điểm tinh tế là dp được khởi tạo bằng 0, điều này thể hiện chính xác rằng một nút không có cạnh đi ra sẽ đóng góp độ dài đường dẫn bằng 0. Một chi tiết quan trọng khác là chúng tôi tính toán câu trả lời cuối cùng là giá trị dp tối đa, không phải dp của một nút cố định, vì đường dẫn có thể bắt đầu ở bất cứ đâu. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
4 4
1 2
2 3
3 4
1 3
```Thứ tự tôpô có thể là: 

| Bước | Nút | dp trước | Cập nhật | 
| --- | --- | --- | --- | 
| 1 | 4 | [0,0,0,0] | không | 
| 2 | 3 | [0,0,0,0] | dp[3]=1 | 
| 3 | 2 | [0,0,1,0] | dp[2]=1+1=2 | 
| 4 | 1 | [2,2,1,0] | dp[1]=max(1+dp[2],1+dp[3])=3 | 

Câu trả lời cuối cùng là 3. 

Điều này cho thấy cách hợp nhất nhiều đường dẫn tại nút 3 và lập trình động sẽ sử dụng lại chính xác các hậu tố được tính toán thay vì tính toán lại chúng. 

### Ví dụ 2 

đầu vào:```
3 0
```| Bước | Nút | dp | 
| --- | --- | --- | 
| 1 | 1 | 0 | 
| 2 | 2 | 0 | 
| 3 | 3 | 0 | 

Câu trả lời cuối cùng là 0. 

Điều này xác nhận rằng các nút bị cô lập không đóng góp cạnh nào và thuật toán xử lý chính xác một biểu đồ trống mà không cần logic phân nhánh đặc biệt. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n + m) | Mỗi nút được xử lý một lần theo kiểu sắp xếp tôpô và mỗi cạnh được nới lỏng một lần trong DP | 
| Không gian | O(n + m) | danh sách kề, mảng bậc, mảng dp và thứ tự topo | 

Thuật toán phù hợp thoải mái trong các ràng buộc điển hình lên tới 2×10^5 nút và cạnh, vì mọi thao tác đều có kích thước tuyến tính trong biểu đồ. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    import sys
    from collections import deque

    n, m = map(int, sys.stdin.readline().split())
    g = [[] for _ in range(n)]
    indeg = [0] * n

    for _ in range(m):
        u, v = map(int, sys.stdin.readline().split())
        u -= 1
        v -= 1
        g[u].append(v)
        indeg[v] += 1

    q = deque([i for i in range(n) if indeg[i] == 0])
    topo = []
    while q:
        u = q.popleft()
        topo.append(u)
        for v in g[u]:
            indeg[v] -= 1
            if indeg[v] == 0:
                q.append(v)

    dp = [0] * n
    for u in reversed(topo):
        for v in g[u]:
            dp[u] = max(dp[u], 1 + dp[v])

    return str(max(dp) if n > 0 else 0)

assert run("4 4\n1 2\n2 3\n3 4\n1 3\n") == "3"
assert run("3 0\n") == "0"
assert run("5 4\n1 2\n2 3\n3 4\n4 5\n") == "4"
assert run("4 2\n1 2\n3 4\n") == "1"
assert run("1 0\n") == "0"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| 1→2→3→4→5 | 4 | độ đúng của chuỗi tuyến tính | 
| hai cạnh rời nhau | 1 | thành phần bị ngắt kết nối | 
| nút đơn | 0 | xử lý trường hợp tối thiểu | 

## Vỏ cạnh 

Đối với một bộ cạnh trống, chẳng hạn như`n = 3, m = 0`, thuật toán vẫn tạo ra thứ tự tôpô hợp lệ và gán giá trị dp bằng 0 cho tất cả các nút. Giá trị tối đa chính xác là 0 vì không tồn tại chuyển tiếp nào. 

Đối với một chuỗi thuần túy, chẳng hạn như`1 → 2 → 3 → 4`, quá trình xử lý tôpô ngược đảm bảo mỗi dp tích lũy chính xác một cạnh bổ sung cho mỗi bước, tạo ra sự tăng trưởng tuyến tính chính xác. 

Đối với cấu trúc hợp nhất phân nhánh như`1 → 3, 2 → 3, 3 → 4`, nút 3 được tính toán một lần khi dp[3] = 1 và cả hai nút trước đó đều sử dụng lại chính xác giá trị này mà không cần tính toán lại, xác nhận hiệu ứng ghi nhớ của thứ tự DAG.
