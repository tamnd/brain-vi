---
title: "CF 1806E - Chủ Cây"
description: "Cây có gốc ở đỉnh 1. Mỗi đỉnh có giá trị a[v] và mọi đỉnh ngoại trừ gốc đều có đỉnh gốc. Đối với truy vấn (x, y), cả hai đỉnh được đảm bảo nằm ở cùng độ sâu. Bắt đầu từ hai đỉnh này, chúng ta liên tục di chuyển cả hai đỉnh lên trên cùng một lúc."
date: "2026-06-09T09:10:00+07:00"
tags: ["codeforces", "competitive-programming", "brute-force", "data-structures", "dfs-and-similar", "trees"]
categories: ["algorithms"]
codeforces_contest: 1806
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 858 (Div. 2)"
rating: 2200
weight: 1806
solve_time_s: 92
verified: true
draft: false
---

[CF 1806E - Bậc thầy về cây](https://codeforces.com/problemset/problem/1806/E) 

**Xếp hạng:** 2200 
**Thẻ:** bạo lực, cấu trúc dữ liệu, dfs và tương tự, cây cối 
**Thời gian giải:** 1m 32s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Cây có gốc ở đỉnh 1. Mỗi đỉnh đều có một giá trị`a[v]`, và mọi đỉnh ngoại trừ gốc đều có đỉnh gốc. 

Đối với một truy vấn`(x, y)`, cả hai đỉnh đều được đảm bảo nằm ở cùng độ sâu. Bắt đầu từ hai đỉnh này, chúng ta liên tục di chuyển cả hai đỉnh lên trên cùng một lúc. Ở mỗi bước, chúng ta nhân các giá trị trên cặp đỉnh hiện tại và cộng tích đó vào câu trả lời. 

Nếu chúng ta viết chuỗi tổ tiên là`x = x0, x1 = parent(x0), x2 = parent(x1), ...`Và`y = y0, y1 = parent(y0), y2 = parent(y1), ...`thì câu trả lời là`a[x0]a[y0] + a[x1]a[y1] + a[x2]a[y2] + ...`cho đến khi cả hai chuỗi đều tới gốc. 

Phép truy toán ẩn bên trong định nghĩa là$$f(x,y)=a_xa_y+f(p_x,p_y)$$với$$f(0,0)=0.$$Những hạn chế là thách thức thực sự. Cả hai`n`Và`q`có thể đạt được`10^5`. Một truy vấn có thể liên quan đến một đường dẫn có độ dài`10^5`, do đó, việc đi thẳng tới thư mục gốc cho mọi truy vấn có thể yêu cầu khoảng`10^10`hoạt động trong trường hợp xấu nhất. Bất kỳ giải pháp nào xử lý toàn bộ chuỗi tổ tiên cho mỗi truy vấn sẽ bị loại trừ ngay lập tức. 

Giới hạn bộ nhớ rất hào phóng, 1024 MB. Đây là một gợi ý mạnh mẽ rằng chúng tôi được phép lưu trữ một lượng lớn thông tin vào bộ nhớ đệm, nhưng không phải tất cả`n²`các cặp đỉnh có thể có. 

Trường hợp cạnh tinh tế xuất hiện khi cả hai đỉnh được truy vấn đều giống nhau. 

Ví dụ:```
3 1
2 3 5
1 2
3 3
```Câu trả lời đúng là$$5^2+3^2+2^2=38.$$Việc triển khai bất cẩn cố gắng khai thác tính đối xứng mà không xử lý chính xác các đỉnh giống hệt nhau có thể vô tình đếm gấp đôi hoặc bỏ qua các số hạng. 

Một sai lầm dễ mắc phải khác là cho rằng việc ghi nhớ tất cả các cặp là khả thi. 

Hãy xem xét một ngôi sao:```
1
├── 2
├── 3
├── ...
└── 100000
```Tất cả các lá đều có độ sâu như nhau. Hầu như có`10^10`cặp lá có thể. Lưu trữ mọi giá trị cặp là không thể. 

Cạm bẫy thứ ba đến từ độ sâu đệ quy. Sự lặp lại gợi ý một cách tự nhiên việc triển khai đệ quy, nhưng một chuỗi độ dài`10^5`vượt quá giới hạn đệ quy của Python. Một giải pháp Python được chấp nhận phải tăng giới hạn một cách cẩn thận hoặc sử dụng công thức lặp lại. 

## Phương pháp tiếp cận 

Giải pháp vũ phu tuân theo định nghĩa trực tiếp. Đối với mỗi truy vấn, hãy thêm liên tục`a[x] * a[y]`, sau đó thay thế cả hai đỉnh bằng cha mẹ của chúng. 

Sự tái diễn$$f(x,y)=a_xa_y+f(p_x,p_y)$$cho thấy điều này là đúng. Thật không may, nếu cây là một chuỗi dài thì chi phí cho một truy vấn`O(n)`, Và`10^5`truy vấn trở thành`O(nq)=10^{10}`hoạt động. 

Sự tái diễn cũng tiết lộ điều gì đó hữu ích. Nhiều truy vấn cuối cùng đạt đến cùng một cặp`(u,v)`. Một khi chúng ta biết`f(u,v)`, mọi tính toán trong tương lai đạt đến trạng thái đó đều có thể dừng ngay lập tức. 

Vấn đề là có quá nhiều cặp có thể xảy ra. Chúng ta không thể ghi nhớ tất cả chúng. 

Quan sát chính là về độ sâu. 

Cho phép`cnt[d]`là số đỉnh ở độ sâu`d`. 

Chọn một ngưỡng$$B \approx \sqrt n.$$Độ sâu tối đa`B`đỉnh được gọi là độ sâu nhỏ. 

Đối với mỗi độ sâu nhỏ, có nhiều nhất`B`các đỉnh có thể có ở độ sâu đó. Việc lưu trữ các câu trả lời liên quan đến các đỉnh đó trở nên hợp lý hơn. 

Giả sử chúng ta gán cho mỗi đỉnh một vị trí bên trong mức độ sâu của nó. Đối với một đỉnh`y`ở độ sâu nhỏ, chúng tôi có thể lưu trữ`f(x,y)`cho mọi đỉnh`x`. 

Số lượng trạng thái được lưu trữ là$$\sum_{\text{small depths}} n \cdot cnt[d].$$Vì mỗi độ sâu nhỏ chứa nhiều nhất`B`đỉnh thì tổng số nhiều nhất là`nB`. 

Bây giờ hãy xem xét một truy vấn. Khi leo lên trên, nếu độ sâu hiện tại lớn thì độ sâu đó chứa nhiều hơn`B`đỉnh. Vì tổng số đỉnh là`n`, có thể có nhiều nhất`n/B`độ sâu lớn như vậy trên bất kỳ đường dẫn gốc nào. 

Sau nhiều nhất`n/B`di chuyển lên trên, chúng ta đạt đến một độ sâu nhỏ. Tại thời điểm đó, chúng ta có thể truy xuất ngay giá trị được tính toán trước. 

Điều này mang lại$$O\!\left(\frac{n}{B}\right)$$làm việc theo mỗi truy vấn và$$O(nB)$$tổng số trạng thái được lưu trữ 

Lựa chọn$$B \approx \sqrt n$$cân bằng cả kỳ hạn và lợi suất$$O(n\sqrt n)$$sự phức tạp tổng thể. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nq) | O(1) | Quá chậm | 
| Tối ưu | O(n√n + q√n) | O(n√n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Xây dựng cây có gốc và tính độ sâu của mỗi đỉnh. 
2. Đối với mọi độ sâu`d`, đếm xem có bao nhiêu đỉnh thuộc về độ sâu đó. 
3. Bên trong mỗi độ sâu, chỉ định một chỉ mục cục bộ`h[v]`tới mọi đỉnh. Chỉ mục này được sử dụng để giải quyết các trạng thái được ghi nhớ. 
4. Hãy để`B = 320`(khoảng`√100000`). 
5. Xác định hàm ghi nhớ:$$f(x,y)=a_xa_y+f(p_x,p_y).$$Trường hợp cơ bản là`f(0,0)=0`. 

1. Bất cứ khi nào độ sâu hiện tại của`y`nhỏ, có nghĩa là`cnt[depth[y]] <= B`, lưu trữ câu trả lời trong một bảng`dp[x][h[y]]`. 

Chỉ số cục bộ là đủ vì tất cả các đỉnh ở cùng độ sâu đều có vị trí riêng biệt. 

1. Để trả lời một câu hỏi`(x,y)`, đánh giá sự tái phát. Nếu giá trị được ghi nhớ đã tồn tại, hãy trả lại giá trị đó ngay lập tức. 
2. Ngược lại tính$$f(x,y)=a_xa_y+f(p_x,p_y),$$lưu trữ nó nếu độ sâu nhỏ và trả lại. 

### Tại sao nó hoạt động 

Sự lặp lại khớp chính xác với định nghĩa của truy vấn. Mỗi bước đệ quy sẽ loại bỏ một cặp đỉnh khỏi tổng và ủy quyền hậu tố còn lại cho cặp cha. 

Ghi nhớ không bao giờ thay đổi giá trị được trả về. Nó chỉ tránh việc tính toán lại trạng thái đã được giải quyết trước đó. 

Đối với độ sâu chứa nhiều nhất`B`đỉnh, mọi trạng thái`(x,y)`được lưu trữ sau lần tính toán đầu tiên. Đối với độ sâu lớn hơn`B`, có rất ít cấp độ như vậy trên bất kỳ đường dẫn gốc nào, bởi vì mỗi cấp độ lớn chứa nhiều hơn`B`đỉnh và cây chỉ có`n`tổng số đỉnh. 

Do đó, mọi truy vấn chỉ thực hiện một số lượng giới hạn các chuyển tiếp không được lưu vào bộ đệm trước khi đạt đến trạng thái được lưu vào bộ đệm và mọi giá trị được trả về chính xác là giá trị lặp lại được xác định bởi sự cố. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, q = map(int, input().split())
    a = [0] + list(map(int, input().split()))

    parent = [0] * (n + 1)
    children = [[] for _ in range(n + 1)

]
    p = list(map(int, input().split()))

    for i in range(2, n + 1):
        parent[i] = p[i - 2]
        children[parent[i]].append(i)

    depth = [0] * (n + 1)

    cnt = [0] * (n + 1)
    pos = [0] * (n + 1)

    stack = [(1, 0)]
    while stack:
        v, d = stack.pop()
        depth[v] = d
        cnt[d] += 1
        pos[v] = cnt[d]

        for to in children[v]:
            stack.append((to, d + 1))

    B = 320

    dp = [[0] * (B + 2) for _ in range(n + 1)]

    sys.setrecursionlimit(300000)

    def ask(x, y):
        if x == 0:
            return 0

        if cnt[depth[y]] <= B:
            cached = dp[x][pos[y]]
            if cached:
                return cached

        ans = ask(parent[x], parent[y]) + a[x] * a[y]

        if cnt[depth[y]] <= B:
            dp[x][pos[y]] = ans

        return ans

    out = []
    for _ in range(q):
        x, y = map(int, input().split())
        out.append(str(ask(x, y)))

    sys.stdout.write("\n".join(out))

solve()
```Phần đầu tiên xây dựng cây có gốc và tính toán độ sâu. Khi duyệt cây, mỗi đỉnh đều nhận được một vị trí bên trong mức độ sâu của chính nó. Nếu độ sâu chứa các đỉnh`{v1, v2, ..., vk}`, vị trí của chúng là`1..k`. 

cái bàn`dp[x][pos[y]]`chỉ được sử dụng khi độ sâu của`y`là nhỏ. Vì độ sâu nhỏ chứa nhiều nhất`B`đỉnh,`pos[y]`không bao giờ vượt quá chiều rộng của bảng. 

Sự tái diễn`ask(parent[x], parent[y]) + a[x] * a[y]`phản ánh trực tiếp định nghĩa toán học của câu trả lời. 

Việc tra cứu bộ đệm xảy ra trước cuộc gọi đệ quy. Khi một trạng thái từ độ sâu nhỏ đã được tính toán, mọi truy vấn trong tương lai đạt tới trạng thái đó sẽ dừng ngay lập tức. 

Một chi tiết thực hiện là quan trọng. Bộ đệm sử dụng`0`làm điểm đánh dấu "không được tính toán". Điều này là an toàn vì mọi giá trị đỉnh đều dương, vì vậy mọi câu trả lời hợp lệ đều hoàn toàn dương. 

Một chi tiết khác là giới hạn đệ quy. Cây hình dây chuyền có thể có chiều sâu`100000`, do đó giới hạn đệ quy Python mặc định là không đủ. 

## Ví dụ đã hoạt động 

### Mẫu 1 

đầu vào:```
6 2
1 5 2 3 1 1
1 2 3 3 2
4 5
6 6
```Đối với truy vấn`(4,5)`: 

| x | y | Giá trị gia tăng | Tổng chạy | 
| --- | --- | --- | --- | 
| 4 | 5 | 3×1=3 | 3 | 
| 3 | 3 | 2×2=4 | 7 | 
| 2 | 2 | 5×5=25 | 32 | 
| 1 | 1 | 1×1=1 | 33 | 

Đáp án = 33. 

Đối với truy vấn`(6,6)`: 

| x | y | Giá trị gia tăng | Tổng chạy | 
| --- | --- | --- | --- | 
| 6 | 6 | 1×1=1 | 1 | 
| 2 | 2 | 5×5=25 | 26 | 
| 1 | 1 | 1×1=1 | 27 | 

Đáp án = 27. 

Ví dụ này cho thấy sự tái phát trực tiếp. Mỗi hàng tương ứng với một chuyển đổi đệ quy. 

### Ví dụ tùy chỉnh```
4 1
1 2 3 4
1 2 3
4 4
```Cây là một chuỗi. 

| x | y | Giá trị gia tăng | Tổng chạy | 
| --- | --- | --- | --- | 
| 4 | 4 | 16 | 16 | 
| 3 | 3 | 9 | 25 | 
| 2 | 2 | 4 | 29 | 
| 1 | 1 | 1 | 30 | 

Đáp án = 30. 

Ví dụ này thực hiện hành vi có độ sâu tối đa. Thuật toán lặp đi lặp lại áp dụng phép truy toán tương tự cho đến khi đạt được`(0,0)`. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n√n + q√n) | Mỗi truy vấn vượt qua tối đa O(√n) cấp độ chưa được lưu trong bộ nhớ đệm | 
| Không gian | O(n√n) | Các trạng thái được ghi nhớ cho mọi độ sâu nhỏ | 

Với`n = 10^5`,`√n`đại khái là`316`. Số lượng thao tác thu được nằm trong giới hạn thoải mái và mức sử dụng bộ nhớ nằm trong mức 1024 MB khả dụng. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys, io

def run(inp: str) -> str:
    input_data = io.StringIO(inp)

    n, q = map(int, input_data.readline().split())
    a = [0] + list(map(int, input_data.readline().split()))

    parent = [0] * (n + 1)
    children = [[] for _ in range(n + 1)]

    p = list(map(int, input_data.readline().split()))
    for i in range(2, n + 1):
        parent[i] = p[i - 2]
        children[parent[i]].append(i)

    depth = [0] * (n + 1)
    cnt = [0] * (n + 1)
    pos = [0] * (n + 1)

    stack = [(1, 0)]
    while stack:
        v, d = stack.pop()
        depth[v] = d
        cnt[d] += 1
        pos[v] = cnt[d]
        for to in children[v]:
            stack.append((to, d + 1))

    B = 320
    dp = [[0] * (B + 2) for _ in range(n + 1)]

    sys.setrecursionlimit(300000)

    def ask(x, y):
        if x == 0:
            return 0
        if cnt[depth[y]] <= B and dp[x][pos[y]]:
            return dp[x][pos[y]]
        ans = ask(parent[x], parent[y]) + a[x] * a[y]
        if cnt[depth[y]] <= B:
            dp[x][pos[y]] = ans
        return ans

    out = []
    for _ in range(q):
        x, y = map(int, input_data.readline().split())
        out.append(str(ask(x, y)))

    return "\n".join(out)

# sample 1
assert run(
"""6 2
1 5 2 3 1 1
1 2 3 3 2
4 5
6 6
"""
) == "33\n27"

# minimum tree
assert run(
"""2 1
1 2
1
1 1
"""
) == "1"

# chain, same vertex
assert run(
"""4 1
1 2 3 4
1 2 3
4 4
"""
) == "30"

# star tree
assert run(
"""5 1
1 2 3 4 5
1 1 1 1
2 3
"""
) == "7"

# repeated query checks memoization path
assert run(
"""3 3
1 2 3
1 2
3 3
3 3
3 3
"""
) == "14\n14\n14"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
| Cây tối thiểu | 1 | Xử lý gốc và trường hợp cơ sở | 
| Chuỗi, cùng một đỉnh | 30 | Con đường đệ quy sâu | 
| Cây sao | 7 | Lá cùng độ sâu với gốc chung | 
| Truy vấn giống hệt nhau lặp đi lặp lại | 14 mỗi lần | Các trạng thái được ghi nhớ được sử dụng lại chính xác | 

## Vỏ cạnh 

Xét trường hợp có cùng đỉnh:```
4 1
1 2 3 4
1 2 3
4 4
```Thuật toán tính toán`f(4,4) = 16 + f(3,3)`

`f(3,3) = 9 + f(2,2)`

`f(2,2) = 4 + f(1,1)`

`f(1,1) = 1 + f(0,0)`sản xuất`30`. Không có gì đặc biệt được yêu cầu vì phép truy toán xử lý các đỉnh bằng nhau một cách tự nhiên. 

Bây giờ hãy xem xét một ngôi sao:```
5 1
1 2 3 4 5
1 1 1 1
2 3
```Việc thực thi là`f(2,3) = 2*3 + f(1,1)`

`f(1,1) = 1`cho đi`7`. 

Điều này xác nhận rằng thuật toán sẽ tiếp tục chính xác đến tận gốc ngay cả khi các đỉnh được truy vấn phân kỳ ngay lập tức. 

Cuối cùng, hãy xem xét nhiều đỉnh có cùng độ sâu. Một ngôi sao với`99999`lá tạo ra một số lượng lớn các cặp tiềm năng. Thuật toán không bao giờ lưu trữ tất cả các giá trị cặp. Nó chỉ lưu trữ các trạng thái liên quan đến độ sâu có kích thước tối đa`B`, giữ bộ nhớ ở`O(n√n)`còn hơn là`O(n²)`. Đây chính xác là tình huống mà đối số phân tách căn bậc hai được thiết kế để xử lý.
