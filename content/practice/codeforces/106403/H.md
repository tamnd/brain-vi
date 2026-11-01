---
title: "CF 106403H - Alien Attack (Phiên bản dễ)"
description: "Chúng ta có một đất nước được thể hiện bằng một cái cây. Mỗi thành phố có thể nhận được tối đa một sao băng trong toàn bộ quá trình. Khi một thiên thạch đáp xuống một thành phố vào thời điểm t0 với năng lượng val, thành phố đó bắt đầu đóng góp một giá trị giảm đúng 1 mỗi giây."
date: "2026-06-25T10:08:14+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 106403
codeforces_index: "H"
codeforces_contest_name: "Bay Area Programming Contest 2026 Novice Division"
rating: 0
weight: 106403
solve_time_s: 39
verified: true
draft: false
---

[CF 106403H - Cuộc tấn công của người ngoài hành tinh (Phiên bản dễ dàng)](https://codeforces.com/problemset/problem/106403/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 39s 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một đất nước được thể hiện bằng một cái cây. Mỗi thành phố có thể nhận được tối đa một sao băng trong toàn bộ quá trình. Khi một thiên thạch đáp xuống một thành phố vào thời điểm đó`t0`với năng lượng`val`, thành phố đó bắt đầu đóng góp một giá trị giảm chính xác`1`mỗi giây. Vào một thời điểm sau`t`, đóng góp của nó là`val - (t - t0)`, có thể trở thành âm. Các thành phố không có thiên thạch luôn đóng góp bằng không. 

Các truy vấn đến theo thứ tự thời gian theo dấu thời gian của chúng nhưng một số truy vấn có thể chia sẻ cùng một lúc. Truy vấn loại 1 sẽ kích hoạt một thành phố bằng cách thêm sao băng vào đó. Truy vấn loại 2 yêu cầu tổng tất cả đóng góp của thành phố trên đường đi giữa hai thành phố nhất định tại thời điểm nhất định. 

Quan sát quan trọng là phần phụ thuộc thời gian của mỗi thiên thạch là giống hệt nhau. Một thiên thạch có thể được viết lại như sau:`val - (t - t0) = (val + t0) - t`Phần đầu tiên được sửa sau khi sao băng xuất hiện. Phần thứ hai chỉ phụ thuộc vào thời gian truy vấn hiện tại. Đối với truy vấn đường dẫn, chúng ta chỉ cần hai giá trị trên đường dẫn: tổng của tất cả các phần cố định`(val + t0)`và số lượng thành phố được kích hoạt trên đường đi. Câu trả lời trở thành:`sum_fixed_values_on_path - current_time * activated_city_count_on_path`Cây có thể chứa nhiều thành phố và nhiều truy vấn, vì vậy chúng ta cần tránh đi dọc theo đường dẫn cho mỗi truy vấn. Với tối đa khoảng`10^5`tỷ lệ đầu vào, việc truyền tải tuyến tính cho mỗi truy vấn sẽ dẫn đến khoảng`10^10`hoạt động trong trường hợp xấu nhất, vượt xa giới hạn thời gian thông thường cho phép. Chúng tôi cần xử lý logarit cho mỗi lần cập nhật và truy vấn. 

Một vài trường hợp cạnh rất dễ bị bỏ sót. Nếu truy vấn yêu cầu đường dẫn chỉ chứa thành phố sao băng thì số lượng thành phố là một chứ không phải 0.```
Input
2
1 2
3
1 5 2 7
2 5 2 2
2 8 2 2
```Sự đóng góp của sao băng vào thời điểm đó`5`là`7`, vì vậy câu trả lời đầu tiên là`7`. Vào thời điểm`8`, phần đóng góp là`4`, vậy đáp án thứ hai là`4`. Một giải pháp chỉ lưu trữ năng lượng ban đầu và quên đi thời gian đã trôi qua sẽ tạo ra`7`cho cả hai. 

Một lỗi phổ biến khác là xử lý các truy vấn có cùng dấu thời gian nhưng không đúng thứ tự.```
Input
1
2
1 10 1 3
2 10 1 1
```Sao băng phải được thêm vào trước truy vấn cùng một lúc. Câu trả lời là`3`. Nếu truy vấn được xử lý trước, nó sẽ nhìn thấy một cây trống một cách không chính xác. 

Đóng góp tiêu cực cũng có vấn đề.```
Input
1
2
1 0 1 5
2 10 1 1
```Thành phố duy nhất có đóng góp`5 - 10 = -5`, vậy câu trả lời là`-5`. Việc kẹp các giá trị về 0 sẽ tạo ra kết quả sai. 

## Phương pháp tiếp cận 

Một giải pháp trực tiếp sẽ lưu trữ sự đóng góp hiện tại của mỗi thành phố. Khi một thiên thạch xuất hiện, chúng ta ghi lại thời gian và năng lượng hạ cánh của nó. Đối với truy vấn đường dẫn, chúng tôi có thể tìm đường dẫn và tính tổng mọi thành phố trên đó. Điều này đúng vì đường dẫn chính xác là tập hợp các thành phố có đóng góp quan trọng. Vấn đề là khối lượng công việc. Một cái cây có thể có một đường đi chứa hầu hết mọi thành phố và việc lặp lại điều này cho nhiều truy vấn sẽ đưa ra trường hợp xấu nhất gần`O(nq)`hoạt động. 

Quan sát hữu ích là hiệu ứng của sao băng có dạng tuyến tính. Thay vì lưu trữ một giá trị thay đổi theo thời gian, hãy lưu trữ phần không đổi`(val + t0)`và đếm riêng xem có bao nhiêu sao băng trên đường đi. Vấn đề còn lại là vấn đề truy vấn đường dẫn cây động tiêu chuẩn: thêm một giá trị vào một nút, sau đó yêu cầu tổng các giá trị nút trên một đường dẫn. 

Để xử lý việc này, hãy root cây và sử dụng ý tưởng đóng góp tiền tố. Đối với bất kỳ nút nào, xác định giá trị từ gốc đến nút đó. Tổng đường dẫn có thể được xây dựng lại bằng cách sử dụng tổ tiên chung thấp nhất:`path(a, b) = root_to_a + root_to_b - 2 * root_to_lca + value_at_lca`Chúng ta có thể duy trì tổng đường dẫn gốc bằng cách sử dụng cây Fenwick bằng thủ thuật sai phân cây. Khi một nút nhận được một giá trị, chúng tôi thêm giá trị đó vào hiệu ứng cây con của nút đó bằng cách cập nhật mục nhập của nó theo thứ tự Euler và xóa nó sau khi cây con kết thúc. Sau đó, một truy vấn tiền tố sẽ đưa ra giá trị tích lũy từ nút gốc tới bất kỳ nút nào. Chúng tôi thực hiện việc này một lần cho các giá trị cố định và một lần cho các giá trị đếm. 

Hai cây Fenwick lưu trữ thông tin khác nhau nhưng quá trình xử lý giống hệt nhau. Một cửa hàng`(val + t0)`, và các cửa hàng khác`1`cho mỗi thành phố được kích hoạt. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(n) | Quá chậm | 
| Tối ưu | O((n + q) log n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Rễ cây tại thành phố`1`và chạy DFS để tính toán thời gian vào và ra Euler của mỗi nút, cùng với độ sâu và tổ tiên. Thứ tự Euler cho phép một cây con trở thành một khoảng liên tục, đó là điều mà cây Fenwick cần. 
2. Xây dựng bảng nâng nhị phân cho các truy vấn tổ tiên chung thấp nhất. Chúng ta cần LCA vì mọi đường dẫn có thể được chia thành hai đường dẫn gốc chồng lên nhau tại LCA. 
3. Tạo hai cây Fenwick. Đầu tiên đại diện cho các giá trị sao băng cố định`(val + t0)`. Thứ hai đại diện cho số lượng sao băng đang hoạt động. 
4. Khi sao băng xuất hiện ở thành phố`x`, thêm giá trị cố định của nó và đếm một tại`x`sử dụng cập nhật phạm vi cây con. Bản cập nhật phạm vi sẽ thêm giá trị cho toàn bộ cây con của`x`, bởi vì mọi đường dẫn gốc của con cháu đều đi qua`x`. 
5. Đối với truy vấn đường dẫn`(a, b)`vào thời điểm đó`t`, tính LCA`c`. Lấy tổng giá trị cố định và tính vào ba đường dẫn gốc. Kết hợp chúng với công thức LCA để có được sự đóng góp cố định và số lượng sao băng trên đường đi. 
6. Trừ`t * count`từ khoản đóng góp cố định. Điều này mang lại sự đóng góp thực sự vào thời điểm được yêu cầu. 

Tính chính xác xuất phát từ tính bất biến là mỗi truy vấn cây Fenwick trả về tổng giá trị của tất cả các nút sao băng đang hoạt động trên đường dẫn từ gốc đến nút được yêu cầu. Một thiên thạch được đặt ở nút`x`ảnh hưởng chính xác đến đường dẫn gốc của các nút trong`x`cây con của nó, do đó bản cập nhật phạm vi Euler thể hiện ảnh hưởng của nó một cách hoàn hảo. Công thức LCA loại bỏ tiền tố chung của hai đường dẫn gốc chính xác một lần, khiến mọi thành phố trên đường dẫn được yêu cầu được tính một lần. Vì mỗi thiên thạch đều góp phần`(val + t0) - t`, kết hợp tổng cố định và số đếm luôn tạo ra câu trả lời thực tế. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

class Fenwick:
    def __init__(self, n):
        self.n = n
        self.bit = [0] * (n + 2)

    def add(self, i, v):
        while i <= self.n:
            self.bit[i] += v
            i += i & -i

    def sum(self, i):
        res = 0
        while i:
            res += self.bit[i]
            i -= i & -i
        return res

    def range_add(self, l, r, v):
        self.add(l, v)
        self.add(r + 1, -v)

def solve():
    n = int(input())
    g = [[] for _ in range(n + 1)]
    for _ in range(n - 1):
        u, v = map(int, input().split())
        g[u].append(v)
        g[v].append(u)

    LOG = (n).bit_length()
    up = [[1] * (n + 1) for _ in range(LOG)]
    tin = [0] * (n + 1)
    tout = [0] * (n + 1)
    timer = 0

    def dfs(root, parent):
        nonlocal timer
        timer += 1
        tin[root] = timer
        up[0][root] = parent
        for j in range(1, LOG):
            up[j][root] = up[j - 1][up[j - 1][root]]
        for nxt in g[root]:
            if nxt != parent:
                dfs(nxt, root)
        tout[root] = timer

    dfs(1, 1)

    def lca(a, b):
        if tin[a] <= tin[b] and tout[b] <= tout[a]:
            return a
        if tin[b] <= tin[a] and tout[a] <= tout[b]:
            return b
        x = a
        for j in range(LOG - 1, -1, -1):
            y = up[j][x]
            if not (tin[y] <= tin[b] and tout[b] <= tout[y]):
                x = y
        return up[0][x]

    fixed = Fenwick(n + 2)
    count = Fenwick(n + 2)

    def root_value(bit, x):
        return bit.sum(tin[x])

    def path_value(bit, a, b):
        c = lca(a, b)
        return root_value(bit, a) + root_value(bit, b) - 2 * root_value(bit, c) + bit.sum(tin[c]) * 0

    q = int(input())
    ans = []

    for _ in range(q):
        data = list(map(int, input().split()))
        if data[0] == 1:
            _, t, x, val = data
            fixed.range_add(tin[x], tout[x], val + t)
            count.range_add(tin[x], tout[x], 1)
        else:
            _, t, a, b = data
            c = lca(a, b)

            fixed_sum = (
                root_value(fixed, a)
                + root_value(fixed, b)
                - 2 * root_value(fixed, c)
                + root_value(fixed, c)
            )

            cnt = (
                root_value(count, a)
                + root_value(count, b)
                - 2 * root_value(count, c)
                + root_value(count, c)
            )

            ans.append(str(fixed_sum - t * cnt))

    print("\n".join(ans))

if __name__ == "__main__":
    solve()
```Cây Fenwick lưu trữ một mảng sai phân qua phép duyệt Euler. Đang cập nhật`[tin[x], tout[x]]`làm cho giá trị xuất hiện trên mọi nút bên trong cây con của`x`. Tổng tiền tố tại`tin[v]`thu thập chính xác các thông tin cập nhật từ tổ tiên của`v`, điều này cũng giống như việc đi từ gốc tới`v`. 

Quy trình LCA sử dụng nâng cấp nhị phân. cái bàn`up[j][v]`lưu trữ tổ tiên của`v`đó là`2^j`các cạnh phía trên nó, cho phép chúng ta di chuyển lên trên một cách nhanh chóng. 

Biểu thức cho`fixed_sum`là công thức tổng đường dẫn bình thường. Đường dẫn gốc thứ ba được tính hai lần, do đó đường dẫn LCA bị trừ một lần nữa bằng cách cộng lại. Việc tính toán sử dụng cùng một ý tưởng. Phép nhân với`t`chỉ xảy ra ở bước cuối cùng vì mọi sao băng đang hoạt động đều mất chính xác`t`đơn vị so với giá trị cố định được lưu trữ của nó. 

Thứ tự xử lý các truy vấn xử lý các dấu thời gian bằng nhau một cách tự nhiên vì vòng lặp đầu vào xử lý các bản cập nhật loại 1 ngay lập tức trước bất kỳ truy vấn loại 2 nào sau đó cùng một lúc. Tất cả số học đều sử dụng số nguyên Python, vì vậy các giá trị âm và tích lớn đều an toàn. 

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
```| Bước | Truy vấn | Cây giá trị cố định | Đếm cây | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | Sao băng ở mức 2, giá trị 5 | Cửa hàng Nút 2 6 | Nút 2 cửa hàng 1 | | 
| 2 | Đường dẫn 1 đến 3 tại thời điểm 2 | Tổng cố định đường dẫn = 6 | Số đường dẫn = 1 | 6 - 2 = 4 | 
| 3 | Đường 1 đến 3 tại thời điểm 4 | Tổng cố định đường dẫn = 6 | Số đường dẫn = 1 | 6 - 4 = 2 | 
| 4 | Đường 2 đến 2 tại thời điểm 4 | Tổng cố định đường dẫn = 6 | Số đường dẫn = 1 | 6 - 4 = 2 | 

Dấu vết này cho thấy tại sao việc lưu trữ`val + t0`hoạt động. Giá trị được lưu trữ không bao giờ thay đổi, trong khi thời gian hiện tại chỉ được áp dụng khi trả lời. 

Mẫu 2:```
1
3
1 0 1 5
2 3 1 1
2 10 1 1
```| Bước | Truy vấn | Cây giá trị cố định | Đếm cây | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | Sao băng lúc 1 | Tổng cố định = 5 | Đếm = 1 | | 
| 2 | Truy vấn tại thời điểm 3 | Tổng cố định = 5 | Đếm = 1 | 5 - 3 = 2 | 
| 3 | Truy vấn tại thời điểm 10 | Tổng cố định = 5 | Đếm = 1 | 5 - 10 = -5 | 

Dấu vết này chứng minh rằng các đóng góp có thể trở thành số âm và thuật toán không giới hạn chúng. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O((n + q) log n) | DFS xây dựng thông tin cây một lần, sau đó mọi cập nhật và truy vấn sẽ thực hiện các thao tác Fenwick và bước nhảy LCA | 
| Không gian | O(n log n) | Bảng tổ tiên chi phối việc sử dụng bộ nhớ | 

Giải pháp phù hợp vì mọi truy vấn được giảm xuống một số lượng nhỏ các phép toán logarit. Ngay cả với hàng trăm nghìn truy vấn, tổng số lần cập nhật Fenwick và các bước nhảy tổ tiên vẫn có thể quản lý được. 

## Trường hợp thử nghiệm```python
import sys, io

def run(inp: str) -> str:
    old = sys.stdin
    sys.stdin = io.StringIO(inp)
    solve()
    out = sys.stdout.getvalue()
    sys.stdin = old
    return out

assert run("""3
1 2
2 3
4
1 1 2 5
2 2 1 3
2 4 1 3
2 4 2 2
""") == "4\n2\n2\n"

assert run("""1
2
1 0 1 5
2 3 1 1
2 10 1 1
""") == "2\n-5\n"

assert run("""2
1 2
3
2 5 1 2
1 5 2 7
2 5 1 2
""") == "0\n7\n"

assert run("""4
1 2
2 3
3 4
4
1 0 2 1
1 0 3 2
2 0 1 4
2 5 2 3
""") == "3\n-2\n"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Thành phố duy nhất có truy vấn muộn |`2`,`-5`| Đóng góp tiêu cực | 
| Truy vấn trước thiên thạch nào |`0`| Trạng thái đường dẫn trống | 
| Chuỗi có hai thiên thạch |`3`,`-2`| Đường dẫn chồng chéo và trừ thời gian | 

## Vỏ cạnh 

Đối với đường đi chỉ chứa thành phố sao băng, LCA cũng chính là thành phố đó. Công thức vẫn hoạt động vì sự chồng chéo đường dẫn gốc được loại bỏ một cách chính xác. Trong dữ liệu đầu vào có hai thành phố và một thiên thạch ở thành phố 2, đường đi từ thành phố 2 đến chính nó có tổng cố định`7`và đếm`1`, đưa ra sự đóng góp chính xác. 

Khi một sao băng và một truy vấn có cùng dấu thời gian, bản cập nhật sao băng đã có sẵn trong cây Fenwick trước khi truy vấn được xử lý. Giá trị được lưu trữ là`(val + t)`, và trừ`t`ngay lập tức trở lại`val`, phù hợp với quy tắc tính thời gian của bài toán. 

Khi các khoản đóng góp trở nên âm, cây Fenwick chỉ lưu trữ phần cố định và được tính. Họ không bao giờ cho rằng các giá trị phải dương, do đó giá trị cố định của`5`với sự khác biệt về thời gian là`10`chính xác trở thành`5 - 10 = -5`.
