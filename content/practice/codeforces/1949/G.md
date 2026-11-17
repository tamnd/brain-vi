---
title: "CF 1949G - Xe tay ga"
description: "Mỗi tòa nhà có hai phần thông tin độc lập. Chuỗi đầu tiên mô tả lớp nào được tổ chức ở đó. Một tòa nhà có thể cần một giáo sư toán học, một giáo sư khoa học máy tính hoặc không cần giáo sư nào cả. Chuỗi thứ hai mô tả giáo sư nào ban đầu ở đó."
date: "2026-05-31T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "graphs", "greedy"]
categories: ["algorithms"]
codeforces_contest: 1949
codeforces_index: "G"
codeforces_contest_name: "European Championship 2024 - Online Mirror (Unrated, ICPC Rules, Teams Preferred)"
rating: 2300
weight: 1949
solve_time_s: 195
verified: false
draft: false
---

[CF 1949G - Xe tay ga](https://codeforces.com/problemset/problem/1949/G) 

**Đánh giá:** 2300 
**Tags:** đồ thị, tham lam 
**Thời gian giải:** 3 phút 15s 
**Đã xác minh:** không 

## Giải pháp 
## Hiểu vấn đề 

Mỗi tòa nhà có hai phần thông tin độc lập. 

Chuỗi đầu tiên mô tả lớp nào được tổ chức ở đó. Một tòa nhà có thể cần một giáo sư toán học, một giáo sư khoa học máy tính hoặc không cần giáo sư nào cả. 

Chuỗi thứ hai mô tả giáo sư nào ban đầu ở đó. Một tòa nhà có thể có giáo sư toán học, giáo sư khoa học máy tính hoặc không ai cả. 

Bạn lái một chiếc xe máy có thể chở đúng một giáo sư hành khách. Bạn có thể ghé thăm mỗi tòa nhà nhiều nhất một lần. Tại một tòa nhà mà bạn đến thăm, trước tiên bạn có thể thả giáo sư hiện đang đi cùng bạn và sau đó đón giáo sư ban đầu đã ở đó. 

Mục tiêu không phải là giảm thiểu bất cứ điều gì. Chúng ta chỉ cần xuất ra một lộ trình hợp lệ và kết thúc với việc mỗi lớp có một giáo sư đúng chuyên ngành. 

Sự ràng buộc$n \le 2000$đủ nhỏ để công thức bậc hai hoàn toàn có thể chấp nhận được. Thách thức không phải là tính hiệu quả, mà là việc khám phá cấu trúc biểu đồ ẩn giúp dễ dàng xây dựng một hành trình hợp lệ. 

Một sai lầm phổ biến là nghĩ về các tòa nhà một cách riêng lẻ. Hạn chế rằng một tòa nhà chỉ có thể được ghé thăm một lần có nghĩa là một khi chúng ta rời khỏi tòa nhà, nhiệm vụ giáo sư cuối cùng của nó sẽ được ấn định mãi mãi. 

Coi như```
c = "M"
p = "M"
```Tòa nhà đã chính xác rồi. Đến thăm nó là không cần thiết. Một giải pháp cố gắng lay động mọi giáo sư sẽ tạo ra những tác phẩm không cần thiết phải tồn tại. 

Một trường hợp tế nhị khác là```
c = "CM-"
p = "-CM"
```Đây là mẫu. Tòa nhà không có hạng đóng vai trò là điểm khởi đầu của chuỗi vận tải. Nếu không có nguồn giáo sư như vậy, sẽ không có cách nào để bắt đầu di chuyển mọi người xung quanh. 

Quan sát quan trọng là chiếc xe tay ga luôn chở đúng một loại giáo sư và mỗi chiếc xe bán tải sẽ ngay lập tức thay thế giáo sư vừa được thả xuống. Hành vi đó tự nhiên hình thành sự chuyển đổi giữa các loại giáo sư. 

## Phương pháp tiếp cận 

Một cách tiếp cận bạo lực sẽ cố gắng mô phỏng tất cả các lệnh thăm quan có thể có của các tòa nhà. Ngay cả đối với$n=20$, có$20!$đơn đặt hàng, vốn đã lớn về mặt thiên văn. Không gian tìm kiếm bùng nổ rất lâu trước khi đạt đến giới hạn thực tế là 2000 tòa nhà. 

Quan sát hữu ích là chỉ tồn tại ba loại giáo sư:```
-
M
C
```Đối với tòa nhà có loại lớp$c_i$và loại giáo sư ban đầu$p_i$, tưởng tượng một cạnh có hướng$$c_i \rightarrow p_i$$Tại sao hướng này? 

Giả sử chúng ta đến mang theo một giáo sư thuộc loại$c_i$. Chúng ta có thể thả giáo sư đó xuống tòa nhà, thỏa mãn lớp học của nó. Sau đó, chúng tôi chọn loại giáo sư ban đầu$p_i$và tiếp tục mang loại đó. 

Vì vậy, việc tham quan tòa nhà này sẽ biến đổi loại hình mang theo từ$c_i$vào trong$p_i$. 

Bây giờ mọi tòa nhà đều trở thành một cạnh có hướng trong biểu đồ chỉ có ba đỉnh:```
-, M, C
```Một hành trình hợp lệ tương ứng với các cạnh đi ngang sao cho loại đầu ra của một tòa nhà bằng loại đầu vào bắt buộc của tòa nhà tiếp theo. Đó chính xác là định nghĩa của phép đi qua Euler. 

Bởi vì câu lệnh đảm bảo rằng một hành trình hợp lệ tồn tại nên đồ thị kết quả là Euler. Chúng ta chỉ cần tìm một chu trình Euler và sau đó chuyển các cạnh của nó thành phép toán xe tay ga. 

Vì đồ thị chỉ có ba đỉnh nên thuật toán Hierholzer là quá đủ. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu |$O(n!)$|$O(n)$| Quá chậm | 
| Xây dựng du lịch Euler |$O(n)$|$O(n)$| Đã chấp nhận | 

## Hướng dẫn thuật toán 

### 1. Xây dựng biểu đồ 

Tạo ba đỉnh tương ứng với:```
0 = -
1 = M
2 = C
```Đối với mỗi tòa nhà$i$, thêm một cạnh có hướng$$c_i \rightarrow p_i$$và lưu trữ chỉ số tòa nhà trên cạnh đó. 

### 2. Tìm mạch Euler 

Chạy thuật toán Hierholzer bắt đầu từ đỉnh`-`. 

Mỗi cạnh phải được sử dụng đúng một lần. 

Thứ tự các cạnh được Hierholzer tạo ra là thứ tự các tòa nhà sẽ tham gia vào chuỗi vận chuyển. 

### 3. Chuyển mạch Euler thành tòa nhà 

Cho mạch Euler trả về một dãy cạnh$$e_1, e_2, \dots, e_k$$trong đó mỗi cạnh tương ứng với một tòa nhà. 

Các cạnh liên tiếp thỏa mãn$$p(e_i)=c(e_{i+1})$$đó chính xác là điều kiện tương thích cần thiết cho xe tay ga. 

### 4. Tạo lệnh 

Lái xe đến tòa nhà ở cạnh đầu tiên. 

Nếu giáo sư ban đầu của nó không phải là`-`, vấn đề`PICKUP`. 

Đối với mỗi tòa nhà tiếp theo: 

1.`DRIVE`đến tòa nhà đó. 
2. Nếu chúng ta chở một giáo sư, hãy phát hành`DROPOFF`. 
3. Nếu ban đầu tòa nhà có giáo sư, hãy cấp`PICKUP`. 

Sau khi xử lý tòa nhà cuối cùng, nếu một giáo sư vẫn đang được mang theo, hãy đưa ra một bản cuối cùng`DROPOFF`. 

Mạch Euler đảm bảo rằng mọi giáo sư bỏ học đều có chuyên môn chính xác cho tòa nhà đó. 

### Tại sao nó hoạt động 

Mỗi cạnh đại diện cho một tòa nhà chuyển đổi loại giáo sư được bổ nhiệm từ loại lớp sang loại giáo sư ban đầu. 

Trong chu trình Euler, đỉnh cuối của mỗi cạnh bằng đỉnh đầu của cạnh tiếp theo. Do đó, loại giáo sư được chọn ở tòa nhà này chính xác là loại giáo sư được tòa nhà tiếp theo yêu cầu. 

Khi chúng tôi đưa một giáo sư đến một tòa nhà, chuyên môn của giáo sư đó phù hợp với loại lớp của tòa nhà đó, bởi vì lợi thế bắt đầu ở loại lớp đó. Sau đó, xe bán tải sẽ thay đổi loại được mang thành đỉnh đích của cạnh. 

Vì mỗi cạnh được sử dụng chính xác một lần nên mọi tòa nhà đều được xử lý chính xác một lần. Vì quá trình truyền tải là Euler nên tất cả các yêu cầu về loại đều phù hợp trong toàn bộ chuỗi. Mỗi lớp học kết thúc với một giáo sư đúng chuyên ngành. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n = int(input())
    c = input().strip()
    p = input().strip()

    mp = {'-': 0, 'M': 1, 'C': 2}
    rev = ['-', 'M', 'C']

    g = [[] for _ in range(3)]

    for i in range(n):
        u = mp[c[i]]
        v = mp[p[i]]
        g[u].append((v, i + 1))

    ptr = [0] * 3
    stack_v = [0]
    stack_e = []
    euler = []

    while stack_v:
        v = stack_v[-1]

        if ptr[v] < len(g[v]):
            to, idx = g[v][ptr[v]]
            ptr[v] += 1

            stack_v.append(to)
            stack_e.append(idx)
        else:
            stack_v.pop()
            if stack_e:
                euler.append(stack_e.pop())

    euler.reverse()

    ops = []

    if not euler:
        print(0)
        return

    first = euler[0]
    ops.append(f"DRIVE {first}")

    carrying = False

    if p[first - 1] != '-':
        ops.append("PICKUP")
        carrying = True

    for b in euler[1:]:
        ops.append(f"DRIVE {b}")

        if carrying:
            ops.append("DROPOFF")
            carrying = False

        if p[b - 1] != '-':
            ops.append("PICKUP")
            carrying = True

    if carrying:
        ops.append("DROPOFF")

    print(len(ops))
    print("\n".join(ops))

solve()
```Đồ thị chỉ chứa ba đỉnh nhưng có tới 2000 cạnh. Mỗi cạnh lưu trữ chỉ mục tòa nhà vì hành trình cuối cùng phải tham chiếu các tòa nhà chứ không phải các cạnh biểu đồ. 

Thuật toán Hierholzer được thực hiện lặp đi lặp lại. các`stack_v`ngăn xếp lưu trữ các đỉnh trên đường dẫn DFS hiện tại, trong khi`stack_e`lưu trữ các định danh cạnh tương ứng. Khi một đỉnh hết các cạnh đi ra, cạnh gần đây nhất sẽ trở thành một phần của chu trình Euler. 

Sau khi đảo ngược danh sách cạnh được tạo ra, chúng ta thu được thứ tự truyền tải. 

Giai đoạn tạo lệnh phản ánh trực tiếp việc giải thích một cạnh. Xe bán tải thay đổi loại giáo sư được chở đến đích cạnh. Một lần bỏ học sẽ tiêu tốn giáo sư được mang từ cạnh trước đó. Thuộc tính Euler đảm bảo rằng mọi chuyên môn bỏ học đều phù hợp với yêu cầu về hạng của tòa nhà tiếp theo. 

Chi tiết tinh tế nhất là thứ tự`DROPOFF`sau đó`PICKUP`. Tuyên bố rõ ràng cho phép tối đa một lần đưa đón và một lần đón theo thứ tự đó tại một tòa nhà. Đảo ngược chúng sẽ vi phạm các quy tắc. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
3
CM-
-CM
```Các cạnh của đồ thị là: 

| Tòa nhà | Cạnh | 
| --- | --- | 
| 1 | C → - | 
| 2 | M → C | 
| 3 | - → M | 

Chu trình Euler là:```
3 → 2 → 1
```| Bước | Tòa nhà | Mang theo trước | Hành động | Thực hiện sau | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | không | ĐÓN M | M | 
| 2 | 2 | M | ĐÓN, ĐÓN C | C | 
| 3 | 1 | C | GIẢI PHÁP | không | 

Các lệnh được tạo:```
DRIVE 3
PICKUP
DRIVE 2
DROPOFF
PICKUP
DRIVE 1
DROPOFF
```Điều này chứng tỏ chuyên môn hóa được thực hiện chảy qua mạch Euler như thế nào. 

### Ví dụ 2 

đầu vào:```
2
--
MC
```Các cạnh: 

| Tòa nhà | Cạnh | 
| --- | --- | 
| 1 | - → M | 
| 2 | - → C | 

Việc duyệt Euler chỉ đơn giản là thăm cả hai tòa nhà nguồn. 

| Bước | Tòa nhà | Mang theo trước | Hành động | Thực hiện sau | 
| --- | --- | --- | --- | --- | 
| 1 | 1 | không | ĐÓN M | M | 
| 2 | 2 | M | ĐÓN, ĐÓN C | C | 
| Kết thúc | - | C | GIẢI PHÁP | không | 

Ví dụ này cho thấy thuật toán xử lý các tòa nhà không có lớp một cách tự nhiên. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian |$O(n)$| Mỗi cạnh được xử lý một lần bằng thuật toán Hierholzer | 
| Không gian |$O(n)$| Lưu trữ đồ thị và ngăn xếp truyền tải Euler | 

Đồ thị chỉ chứa ba đỉnh và có chính xác$n$các cạnh. Mỗi cạnh được chèn một lần và đi qua một lần, do đó thời gian chạy là tuyến tính theo số lượng tòa nhà. Với$n \le 2000$, điều này dễ dàng nằm trong giới hạn. 

## Trường hợp thử nghiệm```python
# helper skeleton for local testing

import sys
import io

def run(inp: str) -> str:
    sys.stdin = io.StringIO(inp)

    # call solve() here
    return ""

# sample from statement
# output is not unique, so exact-output assertions are inappropriate.

# minimum size
inp = """\
1
-
-
"""

# already satisfied building
inp = """\
1
M
M
"""

# simple chain
inp = """\
3
CM-
-CM
"""

# multiple buildings of same type
inp = """\
4
MMCC
MMCC
"""

# larger balanced construction
inp = """\
6
MMC-C-
CMM--C
"""
```Vì đầu ra không phải là duy nhất nên thử nghiệm thực tế sẽ xác nhận hành trình đã tạo thay vì so sánh với một chuỗi đầu ra cố định duy nhất. 

| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`n=1`, không lớp, không giáo sư | Bất kỳ hành trình trống hoặc tầm thường hợp lệ nào | Kích thước tối thiểu | 
| Một tòa nhà có giáo sư phù hợp | Hành trình hợp lệ | Trạng thái đã hài lòng | 
| Chuỗi mẫu | Hành trình hợp lệ | Chuỗi vận chuyển cơ bản | 
| Tất cả các tòa nhà đã khớp | Hành trình hợp lệ | Đồ thị nặng tự lặp | 
| Phiên bản lớn hơn hỗn hợp | Hành trình hợp lệ | Truyền tải Euler tổng quát | 

## Vỏ cạnh 

Hãy xem xét```
1
M
M
```Tòa nhà đã có giáo sư chính xác. Theo thuật ngữ đồ thị, đây là một vòng lặp tự`M → M`. Hierholzer xử lý các vòng lặp tự động một cách tự nhiên. Việc tham quan tòa nhà chỉ cần thả và chọn cùng chuyên môn, duy trì tính chính xác. 

Coi như```
3
CM-
-CM
```Nguồn giáo sư duy nhất là tòa nhà không có lớp học. Cạnh của nó là`- → M`, trở thành điểm khởi đầu tự nhiên của chu trình Euler. Thuật toán bắt đầu từ đó, chọn giáo sư toán học và truyền bá các chuyên ngành thông qua chuỗi. 

Coi như```
4
MMCC
MMCC
```Mỗi cạnh là một vòng tự lặp. Không có giáo sư thực sự cần phải di chuyển. Việc truyền tải Euler vẫn tồn tại và vẫn hợp lệ vì mỗi tòa nhà đã đáp ứng yêu cầu về lớp của nó. Hành trình được tạo tuân thủ giới hạn một lần ghé thăm và không bao giờ tạo ra sự phân công không tương thích. 

Bất biến tới hạn trong tất cả các trường hợp này là không thay đổi: khi cạnh của tòa nhà`c → p`được xử lý, giáo sư bỏ ở đó có chuyên môn`c`, và giáo sư mang đi có chuyên môn`p`. Việc truyền tải Euler đảm bảo các loại này khớp với các tòa nhà liên tiếp.
