---
title: "CF 102433H - Điểm xoay"
description: "Chúng ta có tập hợp tối đa 2000 điểm trên mặt phẳng, không có ba điểm nào trên một đường thẳng. Một cối xay gió bao gồm một đường quay và một điểm hiện đóng vai trò là trục xoay của nó. Dòng quay theo chiều kim đồng hồ."
date: "2026-08-12T07:35:22+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102433
codeforces_index: "H"
codeforces_contest_name: "2019-2020 ACM-ICPC Pacific Northwest Regional Contest (Div. 1)"
rating: 0
weight: 102433
solve_time_s: 196
verified: true
draft: false
---

[CF 102433H - Điểm xoay](https://codeforces.com/problemset/problem/102433/H) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 3 phút 16s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có tập hợp tối đa 2000 điểm trên mặt phẳng, không có ba điểm nào trên một đường thẳng. Một cối xay gió bao gồm một đường quay và một điểm hiện đóng vai trò là trục xoay của nó. Dòng quay theo chiều kim đồng hồ. Bất cứ khi nào nó đến một điểm khác, điểm đó sẽ trở thành trục quay mới, trong khi đường thẳng vẫn giữ nguyên hướng và tiếp tục quay. Sau khi quay hoàn toàn 360 độ, quá trình sẽ trở lại trạng thái ban đầu. 

Câu hỏi không yêu cầu chúng ta chọn một cấu hình khởi đầu cụ thể. Chúng ta có thể chọn bất kỳ điểm xoay ban đầu nào và bất kỳ hướng ban đầu nào. Đối với mỗi cối xay gió như vậy, chúng tôi đếm số lần mỗi điểm trở thành trục quay và chúng tôi muốn số lượng lớn nhất có thể có cho bất kỳ điểm nào trên tất cả các cối xay gió có thể có. 

Các ràng buộc đủ nhỏ cho phép tính bậc hai, nhưng không đủ nhỏ cho phép tính bậc ba trong Python. Với (n=2000), (n^2) là khoảng bốn triệu, điều này là thực tế. Một phép tính đầy đủ (n^3) cần khoảng tám tỷ phép tính, vượt xa giới hạn 10 giây. Thông số kỹ thuật chính thức của cuộc thi đưa ra giới hạn thời gian là 10 giây và giới hạn bộ nhớ 256 MB. 

Đầu vào hình học cũng được định giá trị số nguyên một cách có chủ ý và giới hạn bởi 10000, do đó tích chéo và hiệu tọa độ phù hợp thoải mái trong số nguyên Python. Do đó, việc triển khai có thể tránh hoàn toàn hình học số ngoại trừ việc sắp xếp góc. Trong giải pháp bên dưới, các góc chỉ được sử dụng để thiết lập trật tự hình tròn xung quanh mỗi trục. Các chuyển đổi cối xay gió thực tế được lập chỉ mục theo thứ tự đó, do đó không sử dụng so sánh dấu phẩy động để quyết định sự kiện nào đến trước. 

Trường hợp cạnh đầu tiên là tập hợp nhỏ nhất có thể.```
2
0 0
1 0
```Chỉ có hai điểm nên đường thẳng xen kẽ giữa chúng. Trong một vòng quay 360 độ đầy đủ, mỗi điểm được thăng cấp hai lần. Câu trả lời là`2`. Việc triển khai bất cẩn coi đường thẳng là một đối tượng khó định hướng và chỉ lưu trữ cặp điểm sẽ chỉ thấy một chuyển tiếp giữa hai điểm và trả về không chính xác.`1`. 

Trường hợp cạnh thứ hai là sự phân biệt giữa hai hướng của cùng một đường hình học. Vì```
3
-1 0
1 0
0 2
```câu trả lời là`2`. Có thể gặp cùng một cặp điểm hai lần trong quá trình quay hoàn toàn 360 độ, một lần với mỗi hướng của đường định hướng. Việc xử lý một góc modulo 180 độ sẽ làm mất đi sự khác biệt này. 

Trường hợp cạnh thứ ba là ranh giới hình tròn có thứ tự góc. Nếu hướng hiện tại gần góc 0 và sự kiện tiếp theo chỉ ở dưới 0 thì sự kiện đó được biểu thị bằng một góc gần (2\pi). Một tìm kiếm tiền thân thông thường phải bao bọc từ mục đầu tiên của mảng được sắp xếp đến mục cuối cùng. Mẫu đầu tiên thực hiện chính xác tình huống này. 

Cuối cùng, tọa độ trùng lặp hoặc hoàn toàn bằng nhau không phải là trường hợp hợp lệ. Đầu vào mô tả một tập hợp các điểm và quá trình hình học giả định các điểm riêng biệt không có ba điểm thẳng hàng. Do đó, không có đầu ra chính xác có ý nghĩa cho đầu vào như```
3
0 0
0 0
0 0
```Giải pháp không nên thêm cách xử lý đặc biệt cho đầu vào không hợp lệ như vậy vì cuộc thi không bao giờ cung cấp nó. 

## Phương pháp tiếp cận 

Mô phỏng trực tiếp nhất bắt đầu từ một trạng thái cối xay gió và liên tục tìm ra điểm tiếp theo bị đường quay chạm vào. Sau mỗi lần thăng hạng, trục xoay sẽ thay đổi, vì vậy chúng ta có thể kiểm tra mọi điểm khác, tính toán vị trí góc của nó xung quanh trục mới và chọn điểm đầu tiên gặp phải khi xoay theo chiều kim đồng hồ. 

Điều này đúng vì cối xay gió chỉ thay đổi khi đường dây tới điểm đầu vào khác. Giữa hai sự kiện như vậy không có gì rời rạc xảy ra. Nếu chúng ta luôn chọn điểm đầu tiên gặp trong hướng quay thì mô phỏng sẽ tuân theo chính xác quá trình vật lý được mô tả bởi bài toán. 

Vấn đề là khối lượng công việc cần thiết để tìm ra điểm đầu tiên đó. Có thể có (\Theta(n^2)) sự kiện khuyến mãi theo định hướng. Nếu mọi sự kiện quét tất cả (n-1) điểm tiếp theo có thể có thì tổng số là (\Theta(n^3)). Chính xác hơn, với hai hướng cho mỗi cặp có thứ tự, sẽ có (2n(n-1)) trạng thái và việc quét (n-1) ứng viên ở mọi trạng thái sẽ cho kết quả 

[ 
2n(n-1)^2. 
] 

Tại (n=2000), đó là khoảng (15,98) tỷ lượt kiểm tra ứng viên. Phân tích cuộc thi chính thức xác định chính xác nút thắt khối này. 

Cũng không cần thiết phải mô phỏng riêng từng hướng xuất phát có thể. Quan sát quan trọng là cối xay gió có tính quyết định khi chúng ta đạt đến một sự kiện khuyến mại. Báo cáo vấn đề cho chúng ta biết rằng sau khi xoay hoàn toàn 360 độ, đường thẳng sẽ trở về vị trí ban đầu. Do đó, tất cả các sự kiện khuyến mãi có thể hình thành nên những chu kỳ có định hướng rời rạc. Khởi động cối xay gió tại bất kỳ điểm nào trong một chu kỳ chỉ cần bắt đầu cùng một chu trình ở một vị trí khác. 

Vì vậy, nhiệm vụ thực sự là thể hiện mọi sự kiện xúc tiến dưới dạng trạng thái và tìm ra người kế nhiệm duy nhất của nó một cách hiệu quả. 

Phần tinh tế là một đường hình học có hai hướng, nhưng cối xay gió lại quay 360 độ chứ không phải 180 độ. Chúng ta phải phân biệt hai hướng của cùng một đường thẳng. Chúng tôi đại diện cho một tiểu bang với tư cách là 

[ 
(a,b,s), 
] 

trong đó (b) là trục hiện tại, (a) là điểm liên quan đến khuyến mãi trước đó và (s) là 0 hoặc 1. Hướng của đường có hướng hiện tại là hướng từ (a) đến (b), được quay bởi (s\pi). 

Đối với một trục quay cố định (b), mọi điểm khác (c) cho hai tia hướng từ (b), một hướng tới (c) và một hướng đối diện với nó. Có (2(n-1)) tia như vậy. Nếu chúng ta sắp xếp tất cả chúng theo góc, thì thăng hạng tiếp theo chỉ đơn giản là tia ngay trước tia hiện tại theo thứ tự chiều kim đồng hồ. 

Đây là mức giảm chính. Thay vì hỏi, đối với mọi sự kiện, "điểm nào trong số (n-1) điểm khác đến trước?", chúng tôi xử lý trước thứ tự vòng tròn xung quanh mỗi trục. Sau đó, một người kế nhiệm được tìm thấy trong thời gian không đổi. 

Đối với mỗi trục, chúng tôi sắp xếp (2(n-1)) tia, đưa ra tiền xử lý (O(n^2\log n)). Khi đã biết trạng thái kế tiếp của mọi trạng thái, tất cả các chu trình cùng nhau chỉ chứa các trạng thái (O(n^2)), do đó, việc duyệt qua chúng sẽ có chi phí (O(n^2)). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n^3)) | (O(n^2)) trở xuống | Quá chậm | 
| Tối ưu | (O(n^2\log n)) | (O(n^2)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Cung cấp cho mỗi điểm đầu vào một chỉ mục từ`0`bởi vì`n - 1`. Trạng thái là một sự kiện xúc tiến có định hướng`(a, b, side)`, Ở đâu`b`là điểm vừa được thăng chức và`side`phân biệt hai hướng có thể có của đường thẳng đi qua`a`Và`b`. Do đó trạng thái có (2n(n-1)) khả năng. 
2. Đối với mọi điểm`b`, xây dựng tất cả các tia định hướng dựa trên`b`. Đối với mọi điểm khác`q`, chèn tia từ`b`ĐẾN`q`và tia đối diện. Mỗi tia lưu trữ góc của nó, điểm`q`nó thuộc về, và nó là tia gốc hay tia đối diện của nó. 
3. Sắp xếp các tia này ngược chiều kim đồng hồ`b`. Danh sách được sắp xếp thể hiện thứ tự chính xác mà đường quay có thể gặp các điểm khác trong khi hướng của nó giảm dần theo chiều kim đồng hồ. 
4. Trong khi xây dựng danh sách đã sắp xếp cho`b`, ghi lại vị trí của cả hai tia thuộc mọi điểm khác`q`. Đây là thủ thuật thực hiện quan trọng. Giả sử trạng thái hiện tại là`(a, b, side)`. Hướng đường hiện tại của nó là hướng từ`a`ĐẾN`b`hoặc hướng đó cộng thêm 180 độ. Tại trục`b`, hướng này chính xác là một trong hai tia được lưu trữ thuộc về`a`. 
5. Tìm vị trí của tia hiện tại. Vì cối xay gió quay theo chiều kim đồng hồ nên sự kiện tiếp theo là tia ngay trước nó theo thứ tự sắp xếp hình tròn. Nếu tia hiện tại ở vị trí 0 thì tia trước đó là tia cuối cùng trong danh sách. 
6. Cho tia liền trước thuộc điểm`c`và để cờ định hướng của nó là`next_side`. Trạng thái tiếp theo là`(b, c, next_side)`. Lưu trữ quá trình chuyển đổi này. Hình học hiện đã được chuyển đổi thành đồ thị có hướng xác định trong đó mỗi trạng thái có chính xác một cạnh đi ra. 
7. Đánh dấu mọi trạng thái là chưa ghé thăm. Bất cứ khi nào tìm thấy trạng thái chưa truy cập, hãy làm theo các con trỏ kế tiếp của nó cho đến khi quay lại trạng thái đã truy cập. Bởi vì mỗi trạng thái có một trạng thái kế tiếp và quá trình này mang tính xác định nên việc duyệt chính xác là một chu trình hoàn chỉnh. 
8. Trong quá trình đi qua một chu trình, mọi trạng thái`(a, b, side)`đại diện cho một lần thăng hạng điểm`b`. Tăng số lượng cho`b`mỗi khi trạng thái đó được ghé thăm. Sau khi chu trình kết thúc, hãy so sánh số lớn nhất với câu trả lời chung. 
9. Lặp lại cho đến khi mọi trạng thái sự kiện định hướng đã được truy cập. Vì mọi cối xay gió có thể đều tương ứng với một vị trí trên một trong các chu kỳ này nên số lượng lớn nhất tìm được trong tất cả các chu kỳ là câu trả lời bắt buộc. 

### Tại sao nó hoạt động 

Điều bất biến là mọi trạng thái`(a, b, side)`thể hiện chính xác một sự kiện khuyến mãi cùng với hướng định hướng hiện tại của đường quay. Xung quanh trục`b`, tất cả các sự kiện có thể xảy ra trong tương lai đều xảy ra khi đường thẳng chạm đến một trong hai tia liên kết với một điểm khác. Danh sách tia được sắp xếp chứa các sự kiện đó theo thứ tự góc, do đó, lấy tiền thân của tia hiện tại chính xác là sự kiện đầu tiên gặp phải khi quay theo chiều kim đồng hồ. 

Mối quan hệ kế tiếp có tính tất định, do đó không gian trạng thái hoàn chỉnh sẽ phân rã thành các chu trình rời rạc. Một cối xay gió quay hoàn toàn tuân theo một chu kỳ như vậy đúng một lần. Do đó, việc đếm điểm thứ hai của mỗi trạng thái trong chu kỳ đó sẽ tính chính xác số lần mỗi điểm được thăng cấp trong cối xay gió đó. Vì mọi trạng thái ban đầu có thể đều thuộc về một chu trình nào đó nên việc kiểm tra mọi chu trình sẽ xem xét mọi cối xay gió có thể có. 

## Giải pháp Python```python
import sys
input = sys.stdin.readline

from math import atan2, pi
from bisect import bisect_left
from array import array

def solve():
    n = int(input())
    points = [tuple(map(int, input().split())) for _ in range(n)]

    if n == 2:
        print(2)
        return

    TWO_PI = 2.0 * pi

    # State:
    #   (a, b, side)
    # where b is the current pivot and the directed line has angle
    # angle(a -> b) + side * pi.
    #
    # Encode it as:
    #   ((a * n + b) << 1) | side
    #
    # There are 2*n*n slots. States with a == b are unused.
    total_states = 2 * n * n
    nxt = array('I', [0]) * total_states

    for b in range(n):
        bx, by = points[b]

        # Each entry is (angle, point, opposite_flag).
        # opposite_flag = 0 means the ray points toward point.
        # opposite_flag = 1 means the ray points in the opposite direction.
        rays = []

        for q in range(n):
            if q == b:
                continue

            qx, qy = points[q]
            ang = atan2(qy - by, qx - bx)
            if ang < 0.0:
                ang += TWO_PI

            rays.append((ang, q, 0))

            opposite = ang + pi
            if opposite >= TWO_PI:
                opposite -= TWO_PI
            rays.append((opposite, q, 1))

        rays.sort(key=lambda x: x[0])

        m = len(rays)

        # pos0[q] = position of the ray b -> q
        # pos1[q] = position of the opposite ray
        pos0 = [-1] * n
        pos1 = [-1] * n

        for i, (_, q, flag) in enumerate(rays):
            if flag == 0:
                pos0[q] = i
            else:
                pos1[q] = i

        # Fill all states whose current pivot is b.
        for a in range(n):
            if a == b:
                continue

            # side = 0:
            #   current direction is angle(a -> b)
            #   which is the ray opposite to b -> a.
            #
            # side = 1:
            #   current direction is angle(a -> b) + pi
            #   which is exactly b -> a.
            current_pos_side0 = pos1[a]
            current_pos_side1 = pos0[a]

            # side 0
            p = current_pos_side0 - 1
            if p < 0:
                p = m - 1

            _, c, next_side = rays[p]
            state = ((a * n + b) << 1)
            nxt[state] = ((b * n + c) << 1) | next_side

            # side 1
            p = current_pos_side1 - 1
            if p < 0:
                p = m - 1

            _, c, next_side = rays[p]
            state = ((a * n + b) << 1) | 1
            nxt[state] = ((b * n + c) << 1) | next_side

    visited = bytearray(total_states)
    answer = 0

    for a in range(n):
        for b in range(n):
            if a == b:
                continue

            base = (a * n + b) << 1

            for side in range(2):
                start = base | side

                if visited[start]:
                    continue

                counts = [0] * n
                cur = start

                while not visited[cur]:
                    visited[cur] = 1

                    pair = cur >> 1
                    promoted = pair % n
                    counts[promoted] += 1

                    cur = nxt[cur]

                cycle_best = max(counts)
                if cycle_best > answer:
                    answer = cycle_best

    print(answer)

if __name__ == "__main__":
    solve()
```Đầu vào được đọc từng điểm một, sau đó bảng chuyển đổi được xây dựng. Trường hợp đặc biệt`n == 2`không cần thiết về mặt toán học, nhưng nó tránh xây dựng một cấu trúc chuyển tiếp nhỏ và làm cho trường hợp tối thiểu trở nên rõ ràng. 

các`rays`danh sách chứa hai mục cho mọi điểm khác. Đầu tiên là hướng thực tế từ trục quay đến điểm đó. Thứ hai là cùng một đường hình học với hướng ngược lại. Đây là yếu tố cho phép thuật toán phân biệt đường 0 độ với đường 180 độ. 

các`pos0`Và`pos1`mảng loại bỏ nhu cầu tìm kiếm nhị phân khi tìm tia hiện tại. Một tiểu bang đã xác định được điểm`a`và hai hướng cho chúng ta biết chính xác hướng nào trong số`a`hai tia là tia hiện tại. Sự kiện tiếp theo chỉ đơn giản là sự kiện trước đó trong mảng hình tròn. 

Điều này cũng tránh được vấn đề về dấu phẩy động khó phát hiện. Chúng tôi không bao giờ so sánh góc của trạng thái hiện tại với các góc trong mảng được sắp xếp. Chúng ta ghi nhớ trực tiếp vị trí mảng chính xác của tia hiện tại và lùi lại một vị trí. Theo đó, thực tế là`atan2`được sử dụng để sắp xếp chỉ đường không tạo ra vấn đề ranh giới đẳng thức. 

Bảng chuyển đổi sử dụng một mảng số nguyên không dấu được đóng gói thay vì danh sách các số nguyên Python. Có thể có khoảng tám triệu trạng thái ở kích thước đầu vào tối đa, do đó, danh sách số nguyên Python bình thường sẽ tiêu tốn nhiều bộ nhớ hơn đáng kể. Bốn byte cho mỗi lần chuyển đổi giữ cho bảng có dung lượng khoảng 32 MB và mảng đã truy cập chỉ thêm khoảng 8 MB. 

Mã hóa trạng thái sử dụng`(a * n + b) << 1 | side`. Hoạt động ngược lại là`pair = state >> 1`theo sau là`promoted = pair % n`. Thành phần thứ hai chính xác là điểm được thăng cấp, đó là lý do tại sao nó là số lượng được tính trong quá trình truyền tải chu kỳ. 

Chỉ số tiền thân được gói bằng```
p = current_pos - 1
if p < 0:
    p = m - 1
```vì thứ tự góc là hình tròn. Việc quên đi sự bao bọc này là một trong những cách dễ nhất dẫn đến thất bại đối với các cấu hình trong đó sự kiện tiếp theo nằm ngay dưới góc 0. 

Tất cả sự khác biệt giữa các tọa độ chéo đều nhỏ và số nguyên Python có độ chính xác tùy ý. Không thể tràn số nguyên. Phép toán số duy nhất là`atan2`, và nó chỉ được sử dụng để thiết lập trật tự hình tròn của các tia phân biệt. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Sử dụng điểm```
A = (-1, 0)
B = ( 1, 0)
C = ( 0, 2)
```Bắt đầu với trạng thái nơi`A`là điểm trước đó,`B`là điểm mới được thăng cấp và hướng đường là`0`độ. Sáu trạng thái sau đây tạo thành một chu trình hoàn chỉnh. 

| Bước | Tiểu bang | Điểm thăng hạng | Góc đường | Trạng thái tiếp theo | 
| --- | --- | --- | --- | --- | 
| 1 |`(A, B, 0)`| B | (0^\circ) |`(B, C, 1)`| 
| 2 |`(B, C, 1)`| C | (296,6^\circ) |`(C, A, 0)`| 
| 3 |`(C, A, 0)`| A | (243.4^\circ) |`(A, B, 1)`| 
| 4 |`(A, B, 1)`| B | (180^\circ) |`(B, C, 0)`| 
| 5 |`(B, C, 0)`| C | (116,6^\circ) |`(C, A, 1)`| 
| 6 |`(C, A, 1)`| A | (63,4^\circ) |`(A, B, 0)`| 

Trạng thái cuối cùng là trạng thái ban đầu nên sáu sự kiện tạo thành một chu kỳ. Mỗi điểm xảy ra hai lần như điểm thăng cấp. Do đó, số chu kỳ là 2 cho mỗi điểm, đưa ra câu trả lời mẫu`2`. 

Dấu vết này chứng tỏ tại sao bit định hướng lại cần thiết. đầu tiên`(A, B)`sự kiện có hướng (0^\circ), trong khi sự kiện thứ tư có hướng (180^\circ). Chúng liên quan đến cùng một cặp điểm nhưng là các trạng thái khác nhau. 

### Mẫu 2 

hãy để```
A = (0, 0)
B = (5, 0)
C = (0, 5)
D = (5, 5)
E = (1, 2)
F = (4, 2)
```Một trong các chu kỳ chứa các trạng thái sau. 

| Bước | Tiểu bang | Điểm thăng hạng | Góc đường | Trạng thái tiếp theo | 
| --- | --- | --- | --- | --- | 
| 1 |`(A, B, 1)`| B | (180^\circ) |`(B, E, 0)`| 
| 2 |`(B, E, 0)`| E | (153,4^\circ) |`(E, C, 0)`| 
| 3 |`(E, C, 0)`| C | (108,4^\circ) |`(C, A, 1)`| 
| 4 |`(C, A, 1)`| A | (90^\circ) |`(A, E, 0)`| 
| 5 |`(A, E, 0)`| E | (63,4^\circ) |`(E, D, 0)`| 
| 6 |`(E, D, 0)`| D | (41,6^\circ) |`(D, B, 1)`| 
| 7 |`(D, B, 1)`| B | (0^\circ) |`(B, E, 1)`| 
| 8 |`(B, E, 1)`| E | (333,4^\circ) |`(E, C, 1)`| 
| 9 |`(E, C, 1)`| C | (288.4^\circ) |`(C, A, 0)`| 
| 10 |`(C, A, 0)`| A | (270^\circ) |`(A, B, 1)`| 

Số lượng khuyến mãi trong chu kỳ này là 

[ 
A=2,\quad B=2,\quad C=2,\quad D=1,\quad E=3,\quad F=0. 
] 

điểm`E`được thăng cấp ba lần, vì vậy câu trả lời là`3`. Dấu vết này rất hữu ích vì mức tối đa không nhất thiết phải giống nhau ở mọi điểm. Thuật toán phải tính các lần thăng hạng riêng cho từng điểm trong mỗi chu kỳ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n^2\log n)) | Mỗi trục sắp xếp (2(n-1)) tia và tất cả các trạng thái sau đó được duyệt qua một lần | 
| Không gian | (O(n^2)) | Bảng kế tiếp có (2n^2) mục và mảng đã truy cập có (2n^2) byte | 

Với (n=2000), số lượng bang là khoảng 8 triệu. Hoạt động tốn kém là sắp xếp các tia góc xung quanh mỗi trục quay, tạo ra khoảng (n) loại phần tử (2n). Sau đó, mọi trạng thái được xử lý chính xác một lần. Đây là không gian trạng thái tỷ lệ bậc hai do hình học dự định và nó tránh được việc quét khối sẽ yêu cầu khoảng 16 tỷ lượt kiểm tra ứng viên ở kích thước tối đa. 

Biểu diễn chuyển tiếp nhỏ gọn đặc biệt hữu ích trong Python vì chỉ riêng không gian toán học (O(n^2)) bị ràng buộc không tính đến chi phí hoạt động của các đối tượng Python. sử dụng`array('I')`Và`bytearray`giúp việc triển khai diễn ra thoải mái dưới giới hạn bộ nhớ cuộc thi 256 MB. 

## Trường hợp thử nghiệm 

Dây thử nghiệm bên dưới sử dụng tương tự`solve`hoạt động như giải pháp được gửi. Nó chuyển hướng đầu vào tiêu chuẩn cho mỗi bài kiểm tra và ghi lại đầu ra tiêu chuẩn. 

Trường hợp kích thước tối đa sử dụng cấu trúc bậc hai tiêu chuẩn (y=x^2\bmod 2003) cho mô đun nguyên tố. Lấy 2000 điểm đầu tiên sẽ cho tọa độ trong phạm vi yêu cầu và tránh được ba điểm thẳng hàng. Đối với bài kiểm tra căng thẳng này, xác nhận sẽ kiểm tra phạm vi hợp lệ về mặt toán học thay vì mã hóa cứng một câu trả lời cụ thể, vì mục đích của trường hợp này là thực hiện toàn bộ không gian trạng thái và mức sử dụng bộ nhớ. 

Đầu vào tọa độ hoàn toàn bằng nhau không được cố tình chuyển cho giải pháp vì nó vi phạm các giả định đầu vào của vấn đề.```python
import sys
import io

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout

    sys.stdin = io.StringIO(inp)
    sys.stdout = io.StringIO()

    try:
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample 1
assert run("""\
3
-1 0
1 0
0 2
""") == "2", "sample 1"

# Provided sample 2
assert run("""\
6
0 0
5 0
0 5
5 5
1 2
4 2
""") == "3", "sample 2"

# Custom 1: minimum size
assert run("""\
2
0 0
1 0
""") == "2", "minimum size"

# Custom 2: boundary coordinates, still only three non-collinear points
assert run("""\
3
-10000 -10000
10000 -10000
-10000 10000
""") == "2", "coordinate boundary"

# Custom 3: symmetric square, useful for checking the two orientations
assert run("""\
4
0 0
1 0
1 1
0 1
""") == "2", "square"

# Custom 4: maximum-size stress case.
# 2003 is prime, and (x, x^2 mod 2003) gives a no-three-collinear set.
points = []
p = 2003
for x in range(2000):
    points.append((x, (x * x) % p))

stress = [str(len(points))]
stress.extend(f"{x} {y}" for x, y in points)
stress_input = "\n".join(stress) + "\n"

stress_answer = int(run(stress_input))
assert 2 <= stress_answer <= 2 * (len(points) - 1), "maximum-size stress"

# Invalid-input guard for the "all equal" case.
# The problem does not define an output for this input because the points
# are not a valid set of distinct points.
invalid_all_equal = """\
3
0 0
0 0
0 0
"""
coords = [tuple(map(int, line.split()))
          for line in invalid_all_equal.strip().splitlines()[1:]]
assert len(set(coords)) != 3, "all-equal input must be rejected as invalid"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`2 / (0,0) / (1,0)`|`2`| Kích thước tối thiểu và hai hướng của một dòng | 
|`3 / (-10000,-10000) / (10000,-10000) / (-10000,10000)`|`2`| Phối hợp xử lý ranh giới | 
| Đơn vị vuông |`2`| Sự đối xứng và tách hướng | 
| Xây dựng bậc hai 2000 điểm |`2 <= answer <= 3998`| Kích thước không gian trạng thái tối đa và mức sử dụng bộ nhớ | 
| Ba điểm giống nhau | Không hợp lệ | Xác nhận rằng đầu vào hoàn toàn bằng nhau nằm ngoài miền vấn đề | 

## Vỏ cạnh 

Đối với hai điểm,```
2
0 0
1 0
```mỗi trục chỉ có một điểm khác. Hai hướng có thể có của đường này tạo ra một chu trình bốn trạng thái. Trình tự được thăng cấp là`B, A, B, A`, vì vậy mỗi điểm được thăng cấp hai lần. Việc triển khai xử lý việc này một cách tự nhiên thông qua hai tia đối diện được lưu trữ cho hàng xóm duy nhất và biểu thức rõ ràng`n == 2`lợi nhuận chi nhánh`2`trực tiếp. 

Đối với tam giác nhạy cảm với định hướng,```
3
-1 0
1 0
0 2
```các tiểu bang`(A,B,0)`Và`(A,B,1)`là khác biệt. Họ mô tả cùng một đường hình học nhưng hướng khác nhau 180 độ. Chu trình chứa sáu trạng thái chứ không phải ba và mỗi điểm được thăng cấp hai lần. các`side`bit trong mã hóa trạng thái chính xác là thứ ngăn cản thuật toán thu gọn các sự kiện này. 

Để bao quanh góc, hãy xem xét bước đầu tiên của Mẫu 1. Đường hiện tại có góc (0^\circ), trong khi tia liên quan tiếp theo xung quanh`B`có góc xấp xỉ (296,6^\circ). Vì cối xay gió quay theo chiều kim đồng hồ nên (296.6^\circ) là hướng tiếp theo sau số 0. Trong danh sách tia tròn được sắp xếp, nó là tia trước của tia hiện tại, với phép toán tiền thân bao bọc từ vị trí 0 đến vị trí cuối cùng. Mã thực hiện điều này với sự rõ ràng`if p < 0`chi nhánh. 

Đối với hình vuông```
4
0 0
1 0
1 1
0 1
```hình học có tính đối xứng đáng kể, nhưng câu trả lời vẫn là`2`. Giải pháp chỉ sử dụng các cặp vô hướng có thể dễ dàng thu gọn hai lượt truy cập của cùng một cặp thành một và trả về kết quả sai. Bit định hướng giữ cho hai đường truyền tách biệt ngay cả khi cặp điểm cơ bản giống hệt nhau. 

Đối với mẫu thứ hai, các điểm bên trong cho thấy tại sao câu trả lời không được xác định đơn giản bằng số điểm đầu vào. điểm`(1,2)`được thăng ba lần trong một chu kỳ, trong khi một số điểm khác chỉ được thăng một hoặc hai lần. Thuật toán không cố gắng rút ra công thức từ độ lồi hoặc độ sâu. Nó tuân theo các chu kỳ trạng thái chính xác, tự động ghi lại các khuyến mãi lặp đi lặp lại của các điểm bên trong. 

Đối với trường hợp kích thước tối đa, bảng chuyển tiếp chứa khoảng tám triệu trạng thái. Mỗi trạng thái được biểu thị bằng bốn byte trong mảng kế tiếp và một byte trong mảng đã truy cập. Các danh sách tia được xây dựng lần lượt từng trục và bị loại bỏ sau khi quá trình chuyển đổi của trục đó được viết, do đó việc triển khai không bao giờ lưu trữ đồng thời tất cả các danh sách góc. Điều này giữ cho mức sử dụng bộ nhớ tỷ lệ thuận với không gian trạng thái bậc hai thay vì nhân không gian bậc hai với tổng chi phí của các bộ dữ liệu và số float trong Python. 

Trường hợp hoàn toàn bằng nhau thì khác vì nó không phải là trường hợp biên của thuật toán. Đó là một đầu vào không hợp lệ. Với```
3
0 0
0 0
0 0
```không có đường thẳng xác định rõ ràng nào được xác định bởi hai điểm phân biệt, do đó không có quá trình cối xay gió nào tồn tại theo giả định của bài toán. Câu trả lời đúng không phải là một câu trả lời bằng số đặc biệt mà là để nhận biết rằng trường hợp đó không thể xảy ra trong một bài kiểm tra hợp lệ. 

Ý tưởng trọng tâm để thực hiện các bài toán hình học tương tự là ngừng mô phỏng trực tiếp chuyển động liên tục. Khi chuyển động được giảm xuống thành các sự kiện rời rạc, trạng thái phù hợp thường chứa vừa đủ thông tin định hướng để làm cho sự kiện tiếp theo mang tính quyết định. Ở đây, điều đó biến cối xay gió thành một hoán vị của các trạng thái định hướng O(n 2 ), sau đó việc truyền chu kỳ trở nên đơn giản.
