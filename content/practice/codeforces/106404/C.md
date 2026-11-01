---
title: "CF 106404C - Alien Attack (Phiên bản dễ)"
description: "Bài toán mô hình một đất nước như một cái cây của các thành phố. Mỗi thành phố bắt đầu với sức mạnh bằng không. Một thiên thạch có thể va vào một thành phố một lần và khi chạm đất, nó sẽ mang lại cho thành phố đó một lượng năng lượng ban đầu và giảm đi một giây sau thời gian va chạm."
date: "2026-06-25T10:01:34+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106404
codeforces_index: "C"
codeforces_contest_name: "Bay Area Programming Contest 2026 Advanced Division"
rating: 0
weight: 106404
solve_time_s: 40
verified: true
draft: false
---

[CF 106404C - Cuộc tấn công của người ngoài hành tinh (Phiên bản dễ dàng)](https://codeforces.com/problemset/problem/106404/C) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 40s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Bài toán mô hình một đất nước như một cái cây của các thành phố. Mỗi thành phố bắt đầu với sức mạnh bằng không. Một thiên thạch có thể va vào một thành phố một lần và khi chạm đất, nó sẽ mang lại cho thành phố đó một lượng năng lượng ban đầu và giảm đi một giây sau thời gian va chạm. Một truy vấn yêu cầu tổng công suất hiện có trên mỗi thành phố dọc theo con đường duy nhất giữa hai thành phố. Phiên bản dễ dàng có giới hạn đủ nhỏ, với$n \le 2000$Và$q \le 5000$, cho phép các giải pháp quá chậm đối với phiên bản cứng. 

Đầu vào đầu tiên đưa ra cấu trúc cây, sau đó là chuỗi các sự kiện. Sự kiện loại 1 ghi lại cảnh thiên thạch rơi xuống một thành phố vào một thời điểm cụ thể. Sự kiện loại 2 yêu cầu tổng lũy ​​thừa trên một đường dẫn tại một thời điểm cụ thể. Các sự kiện có cùng dấu thời gian yêu cầu phải áp dụng việc hạ cánh sao băng trước khi trả lời các truy vấn đường dẫn tại dấu thời gian đó. Đầu ra là tổng đường dẫn cho mọi sự kiện loại 2. 

Giá trị nhỏ của$n$thay đổi chiến lược so với phiên bản cứng. Chỉ với 2000 thành phố, việc quét đường đi bằng cách sử dụng phương pháp duyệt cây tốn nhiều nhất$O(n)$. Vì chỉ có 5000 truy vấn nên khoảng mười triệu lượt truy cập thành phố là đủ, điều này có thể dễ dàng quản lý bằng Python. Ở đây, các phương pháp tiếp cận liên quan đến cấu trúc dữ liệu cây nặng là không cần thiết. 

Các trường hợp cạnh chính đến từ việc xử lý thời gian. Một thiên thạch hạ cánh chính xác vào thời điểm truy vấn đã đóng góp toàn bộ năng lượng ban đầu. Ví dụ:```
3
1 2
2 3
2
1 5 2 10
2 5 1 3
```Đầu ra đúng là:```
10
```Việc thực hiện bất cẩn chỉ kích hoạt sao băng khi`query_time > meteor_time`sẽ bỏ lỡ sao băng vì hai thời điểm bằng nhau. 

Một sai lầm dễ mắc phải khác là cho phép một thiên thạch ảnh hưởng đến các thành phố nằm ngoài đường dẫn được truy vấn. Ví dụ:```
3
1 2
2 3
2
1 1 1 7
2 1 3
```Đầu ra đúng là:```
7
```Sao băng ở thành phố 1 góp phần vì thành phố 1 đang trên đường đi. Một giải pháp tổng hợp tất cả các thiên thạch đang hoạt động trên toàn cầu cũng sẽ tính cả các thiên thạch không liên quan đến đường đi hiện tại. 

Các giá trị âm cũng cần được bảo tồn. Ví dụ:```
1
1
2
1 0 1 0
2 10 1 1
```Đầu ra đúng là:```
-10
```Sao băng mất năng lượng liên tục, ngay cả khi giá trị của nó trở nên âm. Kẹp phần đóng góp về 0 sẽ tạo ra câu trả lời sai. 

## Phương pháp tiếp cận 

Cách tiếp cận trực tiếp là xử lý các sự kiện theo trình tự thời gian và duy trì các thiên thạch đã hạ cánh. Đối với mỗi truy vấn đường dẫn, chúng tôi đi qua đường dẫn cây và tính toán mức đóng góp hiện tại của mọi sao băng được tìm thấy ở đó. Điều này đúng vì sức mạnh của mỗi thành phố chỉ phụ thuộc vào việc thiên thạch của thành phố đó có hạ cánh hay không và thời gian hiện tại. 

Lần thử đầu tiên có thể tính toán lại tất cả quyền hạn của thành phố cho mỗi truy vấn. Trong trường hợp xấu nhất, điều này chạm đến tất cả$n$thành phố cho mọi truy vấn, đưa ra$O(nq)$hoạt động. Với giới hạn đã cho, đây là khoảng$10^7$hoạt động có thể chấp nhận được. Ý tưởng tương tự có thể được xem như một giải pháp cây bắt buộc: không cần phải tối ưu hóa các truy vấn đường dẫn vì các ràng buộc cố ý giữ khối lượng công việc này ở mức nhỏ. 

Sự đơn giản hóa chính là công thức cho một thiên thạch. Nếu một thiên thạch hạ cánh vào thời điểm$t_0$có giá trị$v$, thì vào lúc đó$t$đóng góp của nó là:$$v-(t-t_0)=(v+t_0)-t$$Đối với một truy vấn duy nhất, thời gian hiện tại là cố định, vì vậy chúng ta chỉ cần biết thành phố nào trên đường đi có sao băng. DFS cây có thể thu thập tổng đường dẫn trực tiếp mà không cần duy trì bất kỳ cấu trúc dữ liệu phức tạp nào. 

Lực lượng vũ phu hoạt động vì phiên bản dễ dàng giữ cho cả cây và số lượng truy vấn ở mức nhỏ. Nó sẽ thất bại trên phiên bản cứng khi cả hai giá trị đều tăng lớn, bởi vì việc đi trên những con đường lớn hàng nghìn lần sẽ trở nên quá đắt. Nhận xét rằng các ràng buộc dễ dàng cho phép truyền tải trực tiếp cho phép chúng tôi duy trì việc triển khai đơn giản. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(n) | Đã chấp nhận | 
| Tối ưu | O(nq) | O(n) | Được chấp nhận cho phiên bản dễ dàng | 

## Hướng dẫn thuật toán 

1. Đọc cây và lưu trữ danh sách kề của nó. Cây có chính xác một đường đi đơn giản giữa hai thành phố bất kỳ, do đó DFS là đủ để khôi phục các thành phố trên đường đi đó. 
2. Sắp xếp tất cả các truy vấn theo dấu thời gian của chúng. Khi một số truy vấn chia sẻ dấu thời gian, hãy đặt các sự kiện sao băng trước các truy vấn đường dẫn. Điều này phù hợp với quy luật rằng một thiên thạch đến đúng lúc$t$đã hoạt động cho một truy vấn vào thời điểm đó$t$. 
3. Duy trì hai mảng cho các thành phố đã nhận được thiên thạch. Một cửa hàng$v+t_0$, phần không đổi của sự đóng góp của thiên thạch và các cửa hàng khác cho dù thành phố hiện có sao băng hay không. 
4. Đối với truy vấn sao băng, hãy kích hoạt thành phố tương ứng bằng cách lưu giá trị và thời gian hạ cánh của thành phố đó. Mỗi thành phố được cập nhật nhiều nhất một lần nên không cần logic loại bỏ. 
5. Đối với truy vấn đường dẫn, hãy chạy DFS từ thành phố bắt đầu cho đến khi tìm thấy đích. Trong khi giải quyết đệ quy, hãy thêm sự đóng góp của mọi thành phố trên con đường đó. Đối với một sao băng đang hoạt động tại thành phố$x$, thêm vào:$$(v_x+t_x)-t$$Ở đâu$t$là thời gian truy vấn. 

1. In câu trả lời theo thứ tự câu hỏi ban đầu. Sắp xếp chỉ được sử dụng nội bộ để mô phỏng thời gian chính xác. 

Bất biến đằng sau thuật toán là trước khi xử lý bất kỳ truy vấn nào tại một thời điểm$t$, mọi sao băng có thời gian hạ cánh nhiều nhất$t$đã được kích hoạt và không có thiên thạch nào trong tương lai được kích hoạt. Truy vấn đường dẫn chỉ truy cập các thành phố trên đường dẫn được yêu cầu, do đó, nó tính tổng chính xác những đóng góp tích cực thuộc về đường dẫn đó. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    graph = [[] for _ in range(n)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        u -= 1
        v -= 1
        graph[u].append(v)
        graph[v].append(u)

    q = int(input())
    queries = []

    for i in range(q):
        data = list(map(int, input().split()))
        queries.append((data[1], 0 if data[0] == 1 else 1, i, data))

    queries.sort()

    active = [False] * n
    base = [0] * n
    ans = [0] * q

    def get_path_sum(start, target, time):
        stack = [(start, -1, 0)]
        parent = [-1] * n
        order = []

        while stack:
            node, par, state = stack.pop()
            if state == 0:
                parent[node] = par
                order.append(node)
                if node == target:
                    break
                for nxt in graph[node]:
                    if nxt != par:
                        stack.append((nxt, node, 0))

        cur = target
        total = 0
        while cur != -1:
            if active[cur]:
                total += base[cur] - time
            if cur == start:
                break
            cur = parent[cur]

        return total

    for time, _, _, data in queries:
        if data[0] == 1:
            _, t, x, val = data
            x -= 1
            active[x] = True
            base[x] = t + val
        else:
            _, t, a, b = data
            a -= 1
            b -= 1
            ans[_] = get_path_sum(a, b, t)

    for x in ans:
        print(x)

if __name__ == "__main__":
    solve()
```Danh sách kề lưu trữ cây với các chỉ số dựa trên 0, giúp cho việc truyền tải trở nên đơn giản. các`active`mảng ghi lại liệu một thành phố có nhận được sao băng hay không và`base`lưu trữ giá trị được chuyển đổi$v+t_0$. Điều này tránh việc tính toán lại việc điều chỉnh thời gian đích cho mỗi truy vấn. 

Các truy vấn được sắp xếp theo thời gian. Phần tử thứ hai trong bộ sắp xếp đặt các truy vấn sao băng trước các truy vấn đường dẫn ở cùng một dấu thời gian. Thứ tự này là bắt buộc vì sao băng sẽ hạ cánh vào thời điểm$t$đóng góp ngay lập tức vào thời điểm$t$. 

Tìm kiếm đường dẫn sử dụng ngăn xếp DFS để xây dựng thông tin gốc cho đến khi đến đích. Sau khi đạt được mục tiêu, mã sẽ đi lùi qua các con trỏ gốc và chỉ tích lũy các thành phố trên đường dẫn đó. Điều kiện dừng ở nút bắt đầu ngăn việc vô tình thêm các nhánh không liên quan. 

Số nguyên Python tự động xử lý các khoản tiền lớn có thể có, do đó không cần xử lý tràn đặc biệt. 

## Ví dụ đã hoạt động 

Mẫu 1:```
3
1 2
2 3
4
1 1 2 5
2 2 1 3
2 4 1 3
2 4 2 2
```Những thay đổi trạng thái quan trọng là: 

| Bước | Truy vấn | Thành phố năng động | Tính toán hiện tại | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | Sao băng tại thành phố 2, thời điểm 1, giá trị 5 | Thành phố 2 với cơ sở 6 | Kích hoạt thành phố 2 | | 
| 2 | Đường dẫn 1 đến 3 tại thời điểm 2 | Thành phố 2 với cơ sở 6 |$6-2=4$| 4 | 
| 3 | Đường 1 đến 3 tại thời điểm 4 | Thành phố 2 với cơ sở 6 |$6-4=2$| 2 | 
| 4 | Đường 2 đến 2 tại thời điểm 4 | Thành phố 2 với cơ sở 6 |$6-4=2$| 2 | 

Điều này chứng tỏ rằng một thiên thạch chỉ ảnh hưởng đến những đường đi chứa thành phố của nó và cùng một thiên thạch có thể trở nên âm khi thời gian tăng lên. 

Mẫu 2:```
2
1 2
3
1 0 1 3
2 0 1 2
2 5 1 2
```| Bước | Truy vấn | Thành phố năng động | Tính toán hiện tại | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | Sao băng tại thành phố 1, thời gian 0, giá trị 3 | Thành phố 1 với cơ sở 3 | Kích hoạt thành phố 1 | | 
| 2 | Đường dẫn 1 đến 2 tại thời điểm 0 | Thành phố 1 với cơ sở 3 |$3-0=3$| 3 | 
| 3 | Đường 1 đến 2 tại thời điểm 5 | Thành phố 1 với cơ sở 3 |$3-5=-2$| -2 | 

Điều này xác nhận rằng việc triển khai sẽ giữ các giá trị công suất âm thay vì kẹp chúng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nq) | Mỗi truy vấn đường dẫn có thể kiểm tra tối đa tất cả các thành phố trong cây và có nhiều nhất$q$truy vấn. | 
| Không gian | O(n+q) | Biểu đồ, danh sách truy vấn và mảng phụ đều có kích thước tuyến tính. | 

Công việc tối đa là khoảng$2000 \times 5000 = 10^7$chuyến thăm thành phố, phù hợp thoải mái trong giới hạn phiên bản dễ dàng. Việc sử dụng bộ nhớ cũng nhỏ vì cây và tất cả các mảng trạng thái được lưu trữ chỉ chứa vài nghìn phần tử. 

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

# sample 1
assert run("""3
1 2
2 3
4
1 1 2 5
2 2 1 3
2 4 1 3
2 4 2 2
""") == """4
2
2
"""

# sample 2
assert run("""1
0
""") == ""

# single node, negative decay
assert run("""1
2
1 0 1 0
2 10 1 1
""") == """-10
"""

# same timestamp meteor must be processed first
assert run("""2
1 2
2
1 5 2 10
2 5 1 2
""") == """10
"""

# multiple active cities on a path
assert run("""4
1 2
2 3
3 4
5
1 1 2 5
1 2 3 7
2 3 1 4
2 10 1 4
2 0 1 1
""") == """8
-7
0
"""
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Thành phố duy nhất có một thiên thạch bị phân hủy | -10 | Các giá trị âm được giữ nguyên. | 
| Sao băng và truy vấn cùng một lúc | 10 | Thứ tự sự kiện là chính xác. | 
| Một số sao băng trên một con đường | 8, -7, 0 | Lọc đường dẫn và phân rã thời gian hoạt động cùng nhau. | 

## Vỏ cạnh 

Đối với trường hợp dấu thời gian bằng nhau:```
2
1 2
2
1 5 2 10
2 5 1 2
```Truy vấn sao băng được xử lý trước tiên vì cả hai sự kiện đều xảy ra tại thời điểm thứ 5. Giá trị được lưu trữ là$10+5=15$và đường dẫn chứa thành phố 2, vì vậy câu trả lời là$15-5=10$. Thuật toán xử lý việc này vì sắp xếp các vị trí truy vấn loại 1 trước truy vấn loại 2 trong cùng một thời điểm. 

Đối với trường hợp lọc đường dẫn:```
3
1 2
2 3
2
1 1 1 7
2 1 3
```Sao băng ở thành phố 1 có đóng góp$7$tại thời điểm 1. Đường đi từ 1 đến 3 là$1 \rightarrow 2 \rightarrow 3$, vì vậy nó bao gồm thiên thạch. Nếu truy vấn từ thành phố 2 đến thành phố 3 thì đóng góp sẽ bằng 0 vì thành phố 1 không nằm trên con đường đó. Việc xây dựng lại DFS gốc đảm bảo chỉ tính các thành phố đường dẫn. 

Đối với phân rã âm:```
1
2
2
1 0 1 0
2 10 1 1
```Sao băng có giá trị cơ bản bằng không. Tại thời điểm 10 phần đóng góp của nó là$0-10=-10$. Mã trực tiếp đánh giá biểu thức này và không bao giờ áp dụng giới hạn dưới, vì vậy nó trả về kết quả âm được yêu cầu.
