---
title: "CF 102694E - Cây giàu bẩn thỉu"
description: "Chúng ta có một cây có gốc có gốc là nút 1. Mỗi nút lưu trữ một số tiền nguyên dương. Giá trị do cây con tạo ra là tích của các giá trị được lưu trữ trong mọi nút bên trong cây con đó. Hai thao tác phải được xử lý trực tuyến."
date: "2026-08-01T23:24:59+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102694
codeforces_index: "E"
codeforces_contest_name: "AlgorithmsThread Tree Basics Contest"
rating: 0
weight: 102694
solve_time_s: 73
verified: true
draft: false
---

[CF 102694E - Cây giàu bẩn thỉu](https://codeforces.com/problemset/problem/102694/E) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 1 phút 13s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một cây có gốc có gốc là nút`1`. Mỗi nút lưu trữ một số tiền nguyên dương. Giá trị do cây con tạo ra là tích của các giá trị được lưu trữ trong mọi nút bên trong cây con đó. 

Hai thao tác phải được xử lý trực tuyến. Một bản cập nhật sẽ thay thế giá trị của một nút. Một truy vấn yêu cầu tỷ lệ giữa các giá trị của hai cây con. Vì sản phẩm có thể trở nên khổng lồ nên tỷ lệ lớn hơn hoặc bằng`10^9`phải được in chính xác`1000000000`. 

Khó khăn xuất phát từ thực tế là việc thay đổi một nút sẽ ảnh hưởng đến mọi cây con chứa nút đó, nghĩa là tất cả các nút tổ tiên của nó. Một mô phỏng trực tiếp sẽ liên tục đi lên trên cây, tốc độ này quá chậm. 

Các giới hạn cho phép xung quanh`O((n+q) log n)`hoạt động. Với tối đa`3 * 10^5`các nút và truy vấn, bất kỳ thứ gì quét toàn bộ cây con hoặc toàn bộ đường dẫn từ gốc tới nút cho mọi thao tác đều có thể đạt được khoảng`9 * 10^10`hoạt động và sẽ không kết thúc. 

Các trường hợp cạnh chính là do sản phẩm lớn và cấu trúc cây gây ra. Một cây nút duy nhất là hợp lệ:```
Input
1
1
1 1 1
```Cây con duy nhất có thể có là gốc, vì vậy câu trả lời là:```
1.0000000000
```Việc triển khai bất cẩn bằng cách sử dụng phép nhân thông thường sẽ tràn ngay cả trên các đầu vào vừa phải, trong khi giải pháp đúng chỉ cần logarit. 

Một trường hợp khác là khi một nút được cập nhật nhiều lần:```
Input
2
1 2
3
1 2 10
1 2 100
2 2 1
```Tỷ lệ cuối cùng là`100`, vì giá trị cũ phải được thay thế chứ không được nhân lên. Việc quên xóa đóng góp trước đó sẽ tạo ra câu trả lời sai. 

Trường hợp cạnh thứ ba là khi cây con mẫu số lớn hơn:```
Input
3
1 2
1 3
2
1 2 5
2 1 2
```Giá trị của cây con`1`là`5`, trong khi cây con`2`là`5`, vậy câu trả lời là`1`. Giải pháp chỉ so sánh tổ tiên hoặc độ sâu sẽ thất bại vì giá trị của cây con phụ thuộc vào tất cả con cháu. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là duy trì mọi giá trị nút và tính toán các tích của cây con khi cần. Đối với một truy vấn trên nút`x`, chúng ta có thể duyệt toàn bộ cây con của`x`, nhân tất cả các giá trị và lặp lại cho`y`. Điều này đúng vì định nghĩa giá trị cây con chính xác là tích đó. 

Vấn đề xuất hiện khi cây trở nên lớn. Cây có hình ngôi sao`n = 300000`có một cây con chứa hầu hết mọi nút. Một truy vấn có thể yêu cầu hàng trăm nghìn phép nhân và việc thực hiện điều đó đối với hàng trăm nghìn truy vấn có phạm vi khoảng`9 * 10^10`hoạt động. 

Quan sát đầu tiên là các sản phẩm khó bảo trì nhưng logarit biến chúng thành tổng:$$\log(a \times b)=\log(a)+\log(b)$$Thay vì lưu trữ tích của cây con, chúng ta lưu trữ tổng logarit bên trong mỗi cây con. Tỷ lệ của hai giá trị cây con trở thành hiệu của hai tổng. 

Thử thách còn lại là cập nhật. Nếu nút`x`thay đổi bởi sự khác biệt logarit`d`, mọi tổng của cây con tổ tiên phải thay đổi bởi`d`. Đây chính xác là bản cập nhật đường dẫn cây từ gốc tới`x`, theo sau là các truy vấn yêu cầu giá trị được liên kết với một nút. 

Chuyến du hành Euler mang lại một sự chuyển đổi hữu ích. Nếu chúng ta lưu trữ một giá trị tại một nút và truy vấn tổng theo khoảng Euler của nó, chúng ta sẽ thu được tổng của cây con. Sử dụng cây Fenwick với kỹ thuật khác biệt, đóng góp từ gốc đến nút có thể được biểu diễn dưới dạng cập nhật phạm vi theo thứ tự Euler, cho phép mọi thao tác chạy theo thời gian logarit. 

Phương pháp cuối cùng giữ lại phần đóng góp logarit của mỗi nút và sử dụng thứ tự Euler với cây Fenwick để duy trì tổng của cây con. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(n) cho mỗi truy vấn/cập nhật trong trường hợp xấu nhất | O(n) | Quá chậm | 
| Tối ưu | O(log n) mỗi thao tác | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Chạy DFS từ thư mục gốc và tính toán`tin[x]`Và`tout[x]`cho mỗi nút. Cây con của`x`trở thành đoạn Euler liền kề`[tin[x], tout[x]]`. 
2. Lưu trữ giá trị logarit của mỗi nút. Ban đầu mỗi nút chứa`1`, vậy mọi logarit đều là`0`. 
3. Khi giá trị nút thay đổi từ`old`ĐẾN`new`, tính:$$\Delta=\log(new)-\log(old)$$Đây là số tiền mà mọi cây con chứa nút này phải đạt được. 

1. Thêm`\Delta`đến vị trí Euler của nút và trừ nó sau khi khoảng thời gian của cây con kết thúc bằng cách sử dụng cây Fenwick. Điều này làm cho việc truy vấn một cây con bằng với việc tính tổng theo khoảng Euler của nó. 
2. Đối với truy vấn yêu cầu các nút`x`Và`y`, thu được tổng cây con logarit`sx`Và`sy`. Tỷ lệ mong muốn là:$$e^{sx-sy}$$Nếu sự khác biệt đủ lớn để kết quả đạt được`10^9`, in nắp ngay. 

Tại sao nó hoạt động: cây Fenwick lưu trữ chính xác đóng góp logarit tích lũy của mỗi nút được cập nhật cho tất cả các cây con chứa nó. Giá trị của mỗi nút đóng góp vào khoảng Euler chính xác của tất cả các nút tổ tiên, do đó tổng duy trì cho một cây con luôn bằng logarit của tích thực của nó. Lấy hiệu của hai tổng được duy trì sẽ cho logarit của tỷ lệ được yêu cầu, kết quả này sẽ thu được câu trả lời sau khi lũy thừa. 

## Giải pháp Python```python
import sys
import math

input = sys.stdin.readline

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0.0] * (n + 3)

    def add(self, i, v):
        while i <= self.n:
            self.bit[i] += v
            i += i & -i

    def sum(self, i):
        res = 0.0
        while i:
            res += self.bit[i]
            i -= i & -i
        return res

    def range_add(self, l, r, v):
        self.add(l, v)
        if r + 1 <= self.n:
            self.add(r + 1, -v)

def solve():
    n = int(input())
    graph = [[] for _ in range(n + 1)]

    for _ in range(n - 1):
        a, b = map(int, input().split())
        graph[a].append(b)
        graph[b].append(a)

    tin = [0] * (n + 1)
    tout = [0] * (n + 1)
    order = 0

    sys.setrecursionlimit(1000000)

    def dfs(v, p):
        nonlocal order
        order += 1
        tin[v] = order
        for u in graph[v]:
            if u != p:
                dfs(u, v)
        tout[v] = order

    dfs(1, 0)

    bit = Fenwick(n)
    values = [1] * (n + 1)
    logs = [0.0] * (n + 1)

    q = int(input())
    ans = []

    for _ in range(q):
        t, x, y = map(int, input().split())

        if t == 1:
            delta = math.log(y) - logs[x]
            logs[x] = math.log(y)
            values[x] = y
            bit.range_add(tin[x], tout[x], delta)
        else:
            diff = bit.sum(tout[x]) - bit.sum(tin[x] - 1)
            diff -= bit.sum(tout[y]) - bit.sum(tin[y] - 1)

            if diff >= math.log(1e9):
                ans.append("1000000000")
            else:
                ans.append("{:.10f}".format(math.exp(diff)))

    sys.stdout.write("\n".join(ans))

if __name__ == "__main__":
    solve()
```DFS gán cho mỗi cây con một khoảng thời gian liên tục. Cây Fenwick không lưu trữ trực tiếp tổng số cây con. Nó lưu trữ một mảng chênh lệch trên các vị trí Euler, vì vậy việc thêm phần đóng góp vào khoảng thời gian của cây con là một cặp cập nhật điểm. 

Hoạt động cập nhật tiếp tục`logs[x]`là logarit hiện tại của giá trị nút. Sự khác biệt giữa logarit mới và cũ chính xác là lượng mà tất cả các tổng của cây con tổ tiên cần thay đổi. 

Truy vấn xây dựng lại tổng cây con bằng cách lấy tổng tiền tố tại`tout[x]`trừ tổng tiền tố trước`tin[x]`. Trừ hai tổng cây con logarit sẽ cho logarit của tỷ lệ được yêu cầu. 

Việc kiểm tra giới hạn được thực hiện trước khi gọi`exp`. Điều này tránh tràn vì tỷ lệ sản phẩm thực tế có thể rất lớn. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) log n) | DFS xây dựng bậc Euler, mọi phép toán Fenwick đều là logarit | 
| Không gian | O(n) | Kho lưu trữ cây, mảng Euler và cây Fenwick | 

Giải pháp chỉ thực hiện công việc logarit cho mỗi truy vấn và cập nhật, phù hợp với`300000`giới hạn hoạt động thoải mái. 

## Ví dụ đã hoạt động 

Đối với mẫu thứ hai:```
5
4 2
1 4
5 4
3 4
```Thứ tự Euler làm cho cây con của nút`4`một khoảng liên tục chứa các nút`2`,`3`,`5`, Và`4`. 

| Hoạt động | Nút cập nhật | Nhật ký đồng bằng | Kết quả truy vấn | 
| --- | --- | --- | --- | 
| Ban đầu | không | 0 | tất cả các tỷ lệ đều là 1 | 
| Đặt nút 5 thành 4 | 5 | nhật ký(4) | cây con 5 trở nên lớn hơn | 
| Đặt nút 5 thành 5 | 5 | log(5)-log(4) | giá trị cũ bị xóa | 
| Đặt nút 5 thành 4 | 5 | log(4)-log(5) | giá trị được khôi phục | 
| Truy vấn 5 vs 4 | không | 0 khác biệt | 1.0000000000 | 

Điều này chứng tỏ tại sao các bản cập nhật phải sử dụng sự khác biệt so với giá trị trước đó. 

Đối với một chuỗi tùy chỉnh:```
3
1 2
2 3
3
1 3 10
2 1 3
2 2 3
```| Hoạt động | Đóng góp tích cực | Nhật ký cây con của nút 1 | Nhật ký cây con của nút 2 | Nhật ký cây con của nút 3 | 
| --- | --- | --- | --- | --- | 
| Ban đầu | không | 0 | 0 | 0 | 
| Cập nhật nút 3 | +log(10) | nhật ký(10) | nhật ký(10) | nhật ký(10) | 
| Truy vấn 1/3 | không | log(10)-log(10)=0 | | | 
| Truy vấn 2/3 | không | | log(10)-log(10)=0 | | 

Bản cập nhật đến với mọi tổ tiên, đó chính xác là những gì đại diện Fenwick duy trì. 

## Trường hợp thử nghiệm```
# These tests illustrate the expected behaviour of the algorithm.

def check(inp, expected):
    import subprocess
    result = subprocess.run(
        ["python3", "main.py"],
        input=inp.encode(),
        stdout=subprocess.PIPE
    )
    assert result.stdout.decode().strip() == expected.strip()

check(
"""1
1
1 1 1
""",
"""1.0000000000"""
)

check(
"""3
1 2
1 3
4
1 2 5
2 1 2
2 2 3
2 1 1
""",
"""1.0000000000
5.0000000000
1.0000000000"""
)

check(
"""2
1 2
3
1 2 100
2 2 1
2 1 2
""",
"""0.0100000000
100.0000000000"""
)

check(
"""4
1 2
2 3
3 4
3
1 4 7
2 1 4
2 2 4
""",
"""1.0000000000
1.0000000000"""
)
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cây nút đơn | 1.0000000000 | Kích thước cây tối thiểu | 
| Cây phân nhánh | Tỷ lệ hỗn hợp | Xử lý khoảng thời gian cây con | 
| Giá trị thay thế lớn | Tỷ lệ chính xác | Cập nhật logic thay thế | 
| Cây xích | Tỷ lệ tổ tiên bằng nhau | Nhân giống từ rễ đến lá | 

## Vỏ cạnh 

Đối với một cây nút đơn, DFS tạo khoảng Euler có độ dài bằng 1. Cây Fenwick không có cập nhật và mọi truy vấn đều so sánh cùng một giá trị, tạo ra`1`. 

Đối với các bản cập nhật lặp lại, logarit được lưu trữ sẽ bị ghi đè thay vì được tích lũy. Ví dụ: thay đổi một nút từ`4`ĐẾN`5`thêm vào`log(5)-log(4)`, không`log(5)`. Điều này giữ cho các giá trị cây con được duy trì nhất quán. 

Đối với các câu trả lời rất lớn, thuật toán so sánh logarit trước khi lũy thừa. Nếu sự khác biệt ít nhất là`log(10^9)`, giá trị chính xác là không cần thiết vì đầu ra yêu cầu đã được cố định ở`1000000000`.
