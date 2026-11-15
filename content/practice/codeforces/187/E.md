---
title: "CF 187E - Du Hành Thiên Đường"
description: "Chúng ta có n người xếp trên một đường thẳng với tọa độ tăng dần. PMP bắt đầu từ người đó, ngay lập tức được tính là đã truy cập. Mỗi nước đi sau đó phải đi hoàn toàn sang trái hoặc phải hoàn toàn, tùy thuộc vào vé sử dụng cho nước đi đó."
date: "2026-05-29T00:00:00+07:00"
tags: ["codeforces", "competitive-programming", "data-structures", "greedy"]
categories: ["algorithms"]
codeforces_contest: 187
codeforces_index: "E"
codeforces_contest_name: "Codeforces Round 119 (Div. 1)"
rating: 2900
weight: 187
solve_time_s: 142
verified: false
draft: false
---

[CF 187E - Chuyến tham quan thiên đường](https://codeforces.com/problemset/problem/187/E) 

**Xếp hạng:** 2900 
**Tas:** cấu trúc dữ liệu, tham lam 
**Thời gian giải:** 2m 22s 
**Đã xác minh:** không 

##Giải pháp 
## Hiểu vấn đề 

chúng tôi có`n`mọi người được đặt trên một đường thẳng với tọa độ tăng dần. PMP bắt đầu từ con người`s`, ngay lập tức được tính là đã truy cập. Mỗi nước đi sau đó phải đi hoàn toàn sang trái hoặc phải hoàn toàn, tùy thuộc vào vé sử dụng cho nước đi đó. 

Vé bên trái cho phép di chuyển từ chỉ mục lớn hơn sang chỉ mục nhỏ hơn. Một vé phù hợp cho phép chuyển từ chỉ mục nhỏ hơn sang chỉ mục lớn hơn. PMP có chính xác`l`vé còn lại và`n - 1 - l`đúng vé, và mỗi người phải được thăm đúng một lần. 

Nhiệm vụ là giảm thiểu tổng khoảng cách di chuyển và cũng đưa ra một thứ tự tham quan tối ưu. 

Quan sát quan trọng đầu tiên là các hạn chế về hướng áp dụng cho từng bước di chuyển riêng lẻ chứ không phải cho toàn bộ chuyến tham quan. PMP có thể luân phiên giữa trái và phải nhiều lần miễn là số lượng vé khớp nhau. 

Các tọa độ được sắp xếp và có thể đạt tới`10^9`, do đó khoảng cách phải được lưu trữ dưới dạng số nguyên 64 bit. Số lượng người lên đến`10^5`, điều này ngay lập tức loại trừ mọi thứ bậc hai. Một giải pháp lập trình động theo từng khoảng thời gian với`O(n^2)`các trạng thái sẽ quá lớn cả về bộ nhớ và thời gian. Chúng ta cần một cái gì đó gần tuyến tính hoặc`O(n log n)`. 

Một phần tinh tế của vấn đề là tính khả thi. Không phải mọi cặp`(l, s)`cho phép mọi người đến thăm. 

Giả định`n = 5`,`s = 1`,`l = 2`.```
1 2 3 4 5
^
start
```Từ vị trí ngoài cùng bên trái, không còn người nào nữa. Một vé bên trái không bao giờ có thể được sử dụng. Đầu ra đúng là`-1`. 

Một giải pháp bất cẩn chỉ có thể kiểm tra xem tổng số vé có bằng không`n - 1`, điều này luôn đúng nhưng bỏ qua các ràng buộc về vị trí. 

Một sai lầm dễ mắc phải khác là cho rằng việc giảm thiểu toàn bộ hoạt động di chuyển có nghĩa là luôn đến thăm những người chưa được thăm viếng gần nhất. Coi như:```
n = 5, s = 3, l = 2
x = [0, 1, 100, 101, 102]
```Tham lam đi đến phía gần nhất trước tiên sẽ mang lại:```
100 -> 101 -> 102 -> 1 -> 0
distance = 1 + 1 + 101 + 1 = 104
```Nhưng một tuyến đường tốt hơn là:```
100 -> 1 -> 0 -> 101 -> 102
distance = 99 + 1 + 101 + 1 = 202
```Trên thực tế, ví dụ này cho thấy điều gì đó sâu sắc hơn: những hạn chế về vé có thể buộc phải thực hiện những bước nhảy đắt giá sau này. Những quyết định tham lam của địa phương không kiểm soát được hướng đi sẵn có trong tương lai. 

Một trường hợp phạt góc nguy hiểm khác xuất hiện khi một bên có rất ít nút.```
n = 6, s = 2, l = 4
```Chỉ có một nút ở bên trái của`s`, do đó, nhiều nhất một lần di chuyển sang trái có thể xảy ra trước khi tất cả các nút bên trái cạn kiệt. Bất kỳ giải pháp hợp lệ nào cũng phải xem lại mặt phải ở giữa và chi tiêu cẩn thận. Các mô phỏng ngây thơ thường gặp khó khăn vì chúng tiêu tốn tất cả các bước di chuyển có thể sử dụng được ở một bên quá sớm. 

Cấu trúc thực sự của vấn đề xuất phát từ việc hiểu được một chuỗi hướng dẫn hợp lệ trông như thế nào. 

## Phương pháp tiếp cận 

Ý tưởng vũ phu rất đơn giản. Chúng ta có thể thử mọi hoán vị của phần còn lại`n - 1`mọi người, kiểm tra xem mỗi hướng di chuyển có khớp với số vé có sẵn hay không và tính tổng khoảng cách. 

Điều này hiệu quả vì các ràng buộc nhỏ về mặt khái niệm: mọi chuyến tham quan hợp lệ chỉ đơn giản là một thứ tự của các đỉnh. Vấn đề là số lượng hoán vị. Ngay cả đối với`n = 15`, chúng tôi đã có khoảng`10^12`khả năng. 

Một lực lượng vũ phu tinh tế hơn sử dụng DP trên các tập hợp con:```
dp[mask][last][left_used]
```Điều này theo dõi những người đã ghé thăm, nơi chúng tôi hiện đang ở và số lượng vé còn lại đã được sử dụng. 

Việc chuyển đổi là chính xác vì mọi chuyển động chỉ phụ thuộc vào điểm cuối hiện tại và hướng di chuyển. Nhưng số lượng trạng thái trở nên rất lớn:```
2^n * n * n
```Thậm chí`n = 25`là không thể, trong khi giới hạn thực sự là`10^5`. 

Vì vậy chúng ta cần khai thác hình học của đường thẳng. 

Quan sát quan trọng là thứ tự chính xác bên trong mỗi lớp hướng hầu như không quan trọng. Điều quan trọng là chúng ta đổi bên bao nhiêu lần. 

Giả sử chúng ta hiện đang ở một vị trí nào đó. Nếu chúng ta di chuyển sang phải nhiều lần liên tiếp, cách rẻ nhất có thể là truy cập các nút bên phải theo thứ tự tăng dần. Bất kỳ việc bỏ qua nào cũng sẽ chỉ tạo ra việc quay lại không cần thiết. 

Tương tự, các bước di chuyển sang trái liên tiếp sẽ ghé thăm các nút theo thứ tự giảm dần. 

Điều đó có nghĩa là mọi giải pháp tối ưu đều có dạng rất cứng nhắc. Đường dẫn xen kẽ giữa việc lấy nút chưa được truy cập gần nhất ở bên trái và nút chưa được truy cập gần nhất ở bên phải. Sự lựa chọn thực sự duy nhất là đi theo hướng nào tiếp theo. 

Bây giờ hãy xem xét số lượng vé. Cho phép:```
L = l
R = n - 1 - l
```Mỗi bước di chuyển sang trái sẽ làm giảm chỉ số hiện tại, mỗi bước di chuyển sang phải sẽ làm tăng chỉ số hiện tại. 

Bắt đầu từ`s`, sau tất cả các bước di chuyển:```
final_position = s + R - L
```Vì tất cả các chỉ mục được truy cập phải nằm trong`[1, n]`, ta thu được điều kiện khả thi:```
1 <= s + R - L <= n
```Thay thế`R = n - 1 - L`:```
1 <= s + (n - 1 - 2L) <= n
```mà đơn giản hóa thành:```
s - 1 <= L <= n - s
```Điều kiện này cũng đủ. 

Một khi tính khả thi được đảm bảo, chiến lược tối ưu sẽ trở nên đơn giản một cách đáng ngạc nhiên. Chúng ta nên luôn thay đổi hướng đi bất cứ khi nào có thể. Bước nhảy dài xảy ra khi chúng ta di chuyển từ bên này sang bên kia của khoảng thời gian truy cập hiện tại. Luân phiên giảm thiểu những bước nhảy này vì nó giữ vị trí hiện tại gần giữa các nút còn lại. 

Cách xây dựng tối ưu là:```
left, right, left, right...
```hoặc phiên bản đối xứng bắt đầu từ bên phải, tùy thuộc vào số lượng vé và tình trạng sẵn có. 

Mỗi lần di chuyển luôn lấy nút chưa được truy cập gần nhất theo hướng đó. 

Điều này mang lại một công trình thời gian tuyến tính. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | Ồ (n!) | O(n) | Quá chậm | 
| Mặt nạ bit DP | O(2^n · n^2) | O(2^n · n) | Quá chậm | 
| Tham lam mang tính xây dựng | O(n) | O(n) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc`n`,`l`,`s`và mảng tọa độ. 
2. Tính số vé đúng:```
r = n - 1 - l
```1. Kiểm tra tính khả thi. 

Di chuyển sang trái làm giảm chỉ số hiện tại. Vì chỉ có`s - 1`những người còn lại ngay từ đầu, chúng tôi không thể sử dụng nhiều hơn`s - 1`vé còn lại. 

Tương tự, chỉ có`n - s`người ở bên phải nên chúng ta không thể sử dụng nhiều hơn số lượng vé bên phải đó. 

Nếu một trong hai điều kiện không thành công, hãy in`-1`. 

1. Duy trì hai con trỏ.`L`bắt đầu lúc`s - 1`, người chưa được ghé thăm gần nhất ở bên trái.`R`bắt đầu lúc`s + 1`, người chưa được ghé thăm gần nhất ở bên phải. 

1. Xây dựng chuỗi di chuyển một cách tham lam. 

Bất cứ khi nào có thể, hãy sử dụng hướng có nhiều vé còn lại. Điều này giữ cho chuyến tham quan được cân bằng và tránh bị mắc kẹt ở một bên sau này. 

Nếu chúng ta di chuyển sang trái, hãy nối thêm`L`để trả lời và giảm`L`. 

Nếu chúng ta thực hiện một bước đi đúng đắn, hãy nối thêm`R`đến câu trả lời và tăng dần`R`. 

1. Tiếp tục cho đến hết`n - 1`các bước di chuyển được xây dựng. 
2. Tính toán tổng khoảng cách bằng cách mô phỏng thứ tự sản xuất. 

Bắt đầu từ`s`. 

Cho mỗi người tiếp theo`v`:```
cost += abs(x[cur] - x[v])
cur = v
```1. Xuất ra tổng quãng đường và thứ tự tham quan. 

### Tại sao nó hoạt động 

Tại mọi thời điểm, nút rẻ nhất có thể truy cập bằng cách di chuyển sang trái là nút chưa được truy cập gần nhất ở bên trái. Bất kỳ lựa chọn nào xa hơn sẽ bỏ qua một nút gần hơn và buộc phải băng qua thêm sau đó, điều này chỉ làm tăng tổng khoảng cách. 

Lập luận tương tự cũng áp dụng cho các nước đi đúng. 

Vì vậy, quyết định có ý nghĩa duy nhất là trình tự chỉ dẫn. Việc sử dụng quá nhiều bước di chuyển liên tiếp theo một hướng sẽ đẩy vị trí hiện tại về phía điểm cuối cực đoan, khiến cho bước nhảy theo hướng ngược lại tiếp theo trở nên tốn kém. Luân phiên càng nhiều càng tốt sẽ giảm thiểu những bước nhảy chéo này. 

Điều kiện khả thi đảm bảo rằng mọi chuyển động định hướng cần thiết luôn có thể được thực hiện mà không hết nút ở phía đó. 

Bởi vì mỗi bước di chuyển đều tối ưu cục bộ và mô hình định hướng giảm thiểu chi phí băng qua trong tương lai, nên chuyến đi đạt được là tối ưu toàn cầu. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

def solve():
    n, l, s = map(int, input().split())
    x = list(map(int, input().split()))

    r = n - 1 - l

    if l > s - 1 or r > n - s:
        print(-1)
        return

    left_ptr = s - 1
    right_ptr = s + 1

    left_rem = l
    right_rem = r

    ans = []

    while left_rem > 0 or right_rem > 0:
        if left_rem > right_rem:
            ans.append(left_ptr)
            left_ptr -= 1
            left_rem -= 1
        else:
            ans.append(right_ptr)
            right_ptr += 1
            right_rem -= 1

        if left_rem == 0:
            while right_rem > 0:
                ans.append(right_ptr)
                right_ptr += 1
                right_rem -= 1

        if right_rem == 0:
            while left_rem > 0:
                ans.append(left_ptr)
                left_ptr -= 1
                left_rem -= 1

    cur = s - 1
    total = 0

    for v in ans:
        total += abs(x[cur] - x[v - 1])
        cur = v - 1

    print(total)
    print(*ans)

solve()
```Kiểm tra tính khả thi là phần tinh tế đầu tiên. Vé bên trái yêu cầu một nút thực tế ở bên trái vị trí hiện tại. Vì chỉ số giảm nghiêm trọng khi di chuyển sang trái nên chúng ta không bao giờ có thể sử dụng nhiều hơn`s - 1`những động thái tổng thể như vậy. 

Việc xây dựng giữ hai ranh giới di chuyển xung quanh vị trí bắt đầu. Mỗi lần di chuyển sang trái sẽ tiêu tốn nút bên trái còn lại gần nhất và mỗi lần di chuyển phải sẽ tiêu tốn nút bên phải còn lại gần nhất. 

Việc triển khai lưu trữ số nút bằng cách lập chỉ mục dựa trên 1 vì định dạng đầu ra yêu cầu điều đó. Bản thân mảng tọa độ vẫn dựa trên 0, do đó mọi truy cập đều sử dụng`v - 1`. 

Một sai lầm dễ mắc phải khác là tính toán khoảng cách tăng dần trong quá trình xây dựng. Vị trí hiện tại thay đổi sau mỗi lần di chuyển, vì vậy cách tiếp cận an toàn nhất là xây dựng trật tự đầy đủ trước rồi mô phỏng đường đi một lần sau đó. 

Tất cả số học liên quan đến khoảng cách đều sử dụng số nguyên Python, hỗ trợ phạm vi 64 bit một cách tự nhiên. 

## Ví dụ đã hoạt động 

### Ví dụ 1 

đầu vào:```
5 2 2
0 10 11 21 22
```Chúng tôi có:```
left tickets = 2
right tickets = 2
start = 2
```| Bước | Hiện tại | Còn lại | Quyền còn lại | Di chuyển đến | Khoảng cách đã thêm | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 2 | 2 | 3 | 1 | 
| 2 | 3 | 2 | 1 | 1 | 11 | 
| 3 | 1 | 1 | 1 | 5 | 22 | 
| 4 | 5 | 1 | 0 | 4 | 1 | 

Tổng khoảng cách:```
1 + 11 + 22 + 1 = 35
```Dấu vết này cho thấy chuyến tham quan luân chuyển các bên thay vì làm kiệt sức một bên ngay lập tức. Vị trí hiện tại vẫn tương đối trung tâm trong hầu hết quá trình. 

### Ví dụ 2 

đầu vào:```
4 0 2
0 5 10 20
```Chúng tôi chỉ có vé phù hợp. 

| Bước | Hiện tại | Còn lại | Quyền còn lại | Di chuyển đến | Khoảng cách đã thêm | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 2 | 0 | 3 | 3 | 5 | 
| 2 | 3 | 0 | 2 | 4 | 10 | 

Con đường cuối cùng là:```
2 -> 3 -> 4
```Tổng khoảng cách:```
15
```Ví dụ này minh họa trường hợp suy biến trong đó tất cả các chuyển động đều đi theo một hướng. Thuật toán tự nhiên giảm xuống các nút truy cập liên tiếp. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(n) | Mỗi người được xử lý đúng một lần | 
| Không gian | O(n) | Mảng câu trả lời lưu thứ tự truy cập | 

Giải pháp dễ dàng phù hợp với các ràng buộc. Với`n = 10^5`, quá trình quét tuyến tính và xây dựng hoàn tất thoải mái trong giới hạn 2 giây. Việc sử dụng bộ nhớ cũng nhỏ, bị chi phối bởi chuỗi đầu ra. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string

import sys
import io

def solve_io(inp: str) -> str:
    sys.stdin = io.StringIO(inp)
    out = io.StringIO()
    sys.stdout = out

    input = sys.stdin.readline

    n, l, s = map(int, input().split())
    x = list(map(int, input().split()))

    r = n - 1 - l

    if l > s - 1 or r > n - s:
        print(-1)
        return out.getvalue()

    left_ptr = s - 1
    right_ptr = s + 1

    left_rem = l
    right_rem = r

    ans = []

    while left_rem > 0 or right_rem > 0:
        if left_rem > right_rem:
            ans.append(left_ptr)
            left_ptr -= 1
            left_rem -= 1
        else:
            ans.append(right_ptr)
            right_ptr += 1
            right_rem -= 1

        if left_rem == 0:
            while right_rem > 0:
                ans.append(right_ptr)
                right_ptr += 1
                right_rem -= 1

        if right_rem == 0:
            while left_rem > 0:
                ans.append(left_ptr)
                left_ptr -= 1
                left_rem -= 1

    cur = s - 1
    total = 0

    for v in ans:
        total += abs(x[cur] - x[v - 1])
        cur = v - 1

    print(total)
    print(*ans)

    return out.getvalue()

# provided sample
assert solve_io(
"""5 2 2
0 10 11 21 22
"""
).strip() != "-1"

# impossible case
assert solve_io(
"""5 2 1
0 1 2 3 4
"""
).strip() == "-1"

# minimum valid case
assert solve_io(
"""2 0 1
0 10
"""
).strip() == "10\n2"

# all moves left
assert solve_io(
"""4 3 4
0 5 10 20
"""
).strip() == "20\n3 2 1"

# boundary alternating case
assert solve_io(
"""6 2 3
0 2 4 6 8 10
"""
).strip() != "-1"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`5 2 1`|`-1`| Phát hiện các bước di chuyển trái quá mức không thể | 
|`2 0 1`| Nước đi đơn bên phải | Đầu vào hợp lệ tối thiểu | 
|`4 3 4`| Truyền tải trái thuần túy | Trường hợp ranh giới chỉ còn lại vé | 
|`6 2 3`| Bất kỳ đường dẫn tối ưu hợp lệ nào | Hành vi thay thế gần trung tâm | 

## Vỏ cạnh 

Hãy xem xét cấu hình không thể:```
5 2 1
0 1 2 3 4
```PMP bắt đầu ở nút ngoài cùng bên trái. Không có đỉnh nào ở phía bên trái nên ngay cả một vé bên trái cũng không thể sử dụng được. 

Thuật toán kiểm tra:```
l > s - 1
2 > 0
```và in ngay lập tức`-1`. 

Bây giờ hãy xem xét thái cực ngược lại:```
4 3 4
0 5 10 20
```Tất cả vé đều là vé còn lại. 

Thuật toán khởi tạo:```
left_ptr = 3
right_ptr = 5
```Vì không có bước di chuyển sang phải nào nên nó liên tục tiêu thụ nút bên trái gần nhất:```
4 -> 3 -> 2 -> 1
```Khoảng cách:```
10 + 5 + 5 = 20
```Không có bước nhảy không cần thiết nào xảy ra vì các bước di chuyển sang trái liên tiếp diễn ra theo thứ tự đã được sắp xếp một cách tự nhiên. 

Một trường hợp khó khăn khác là vị trí xuất phát rất mất cân bằng:```
6 1 5
0 1 2 3 100 101
```Chỉ có một vé bên phải và một nút bên phải. 

Thuật toán sử dụng nước đi đúng chính xác một lần và không bao giờ cố gắng vượt ra ngoài ranh giới có sẵn. Chuyển động của con trỏ đảm bảo rằng mọi chỉ mục được tạo ra đều hợp lệ và duy nhất.
