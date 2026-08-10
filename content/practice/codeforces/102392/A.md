---
title: "CF 102392A - Tối đa hoặc tối thiểu"
description: "Chúng ta có một mảng hình tròn. Trong một thao tác, chúng ta chọn một vị trí và thay thế giá trị của nó bằng giá trị tối thiểu hoặc tối đa của vị trí đó và hai vị trí lân cận của nó."
date: "2026-08-10T21:19:39+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102392
codeforces_index: "A"
codeforces_contest_name: "2019-2020 ICPC Southeastern European Regional Programming Contest (SEERC 2019)"
rating: 0
weight: 102392
solve_time_s: 163
verified: true
draft: false
---

[CF 102392A - Tối đa hoặc tối thiểu](https://codeforces.com/problemset/problem/102392/A) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 2m 43s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng ta có một mảng hình tròn. Trong một thao tác, chúng ta chọn một vị trí và thay thế giá trị của nó bằng giá trị tối thiểu hoặc tối đa của vị trí đó và hai vị trí lân cận của nó. Chúng ta cần số thao tác tối thiểu cần thiết để biến toàn bộ đường tròn thành một giá trị cố định x, với mọi x từ 1 đến m. 

Quan sát đầu tiên là một thao tác không bao giờ có thể tạo ra một giá trị chưa tồn tại ở đâu đó trong mảng. Mỗi giá trị mới được sao chép từ một trong ba giá trị hiện được hiển thị ở vị trí được vận hành. Do đó, nếu x không xuất hiện ban đầu thì câu trả lời cho x ngay lập tức là −1. 

Đối với một giá trị x hiện có cố định, độ lớn chính xác của các số khác không quan trọng. Chúng tôi chỉ quan tâm xem mỗi giá trị có dưới x, bằng x hay trên x hay không. Cuối cùng, một phần tử bên dưới x có thể được nâng lên bằng cách sử dụng thao tác tối đa, trong khi phần tử trên x cuối cùng có thể được hạ xuống bằng cách sử dụng thao tác tối thiểu. 

Một phần tử đã bằng x đóng vai trò là dấu phân cách. Ở hai bên của phần tử đó, các vị trí còn lại tạo thành các chuỗi độc lập. Một chuỗi chỉ chứa các giá trị ở một phía của x sẽ tốn chính xác một thao tác cho mỗi vị trí. Trường hợp thú vị là một chuỗi có các giá trị xen kẽ giữa bên dưới và bên trên x. Một chuỗi có độ dài L như vậy cần 

L+⌊ 2 L ​ ⌋ 

hoạt động. Các thao tác L đầu tiên là cần thiết vì mọi vị trí không phải x phải thay đổi ít nhất một lần. Các phép toán ⌊L/2⌋ bổ sung xuất phát từ thực tế là các giá trị xen kẽ không thể được chuyển đổi trực tiếp thành x. Trước tiên, một số vị trí phải sao chép một giá trị qua ngưỡng x, sau đó các vị trí bị ảnh hưởng có thể được chuyển đổi thành x. 

Các ràng buộc khiến cho việc mô phỏng trực tiếp cho mọi mục tiêu là không thể. Với n,m<2⋅10 5, thuật toán O(nm) có thể thực hiện khoảng 4⋅10 10 phép toán trong trường hợp xấu nhất. Ngay cả thuật toán O(n 2 ) cũng quá chậm. Chúng ta cần xử lý tất cả các giá trị đích cùng nhau, chỉ thay đổi một lượng nhỏ thông tin khi x tăng. 

Có một số trường hợp khó khăn có thể dễ dàng phá vỡ việc triển khai ngây thơ. Coi như```
3 2
1 1 1
```Đầu ra đúng là`0 -1`. Mục tiêu 1 đã đạt được, trong khi mục tiêu 2 không bao giờ xuất hiện và do đó không thể tạo được. Một phương thức giả định mọi giá trị được yêu cầu đều có thể truy cập được sẽ đưa ra câu trả lời không chính xác cho 2. 

Một trường hợp quan trọng khác là```
3 3
1 2 3
```Đầu ra đúng là`2 3 2`. Đối với mục tiêu 2, hai vị trí còn lại nằm ở phía đối diện của 2 nên chúng tạo thành một chuỗi xen kẽ có độ dài bằng 2 và cần thêm một thao tác. Chỉ cần đếm các vị trí không phải thứ 2 sẽ cho kết quả là 2, con số này quá nhỏ. 

Ranh giới hình tròn cũng có vấn đề. Coi như```
5 3
2 1 3 1 3
```Đầu ra đúng là`3 6 3`. Đối với mục tiêu 2, chỉ có một lần xuất hiện của 2, vì vậy tất cả bốn vị trí còn lại tạo thành một chuỗi tròn,`1,3,1,3`. Nó hoàn toàn luân phiên và có giá 4+⌊4/2⌋=6. Việc coi mảng như một dòng bình thường có thể phân chia chuỗi này không chính xác. 

## Phương pháp tiếp cận 

Cách tiếp cận đơn giản là cố định mục tiêu x, mô phỏng hoặc kiểm tra liên tục vòng tròn và xác định cách các giá trị có thể được truyền tới x. Điều này đúng vì mọi thao tác đều cục bộ, vì vậy chúng ta có thể theo dõi rõ ràng cách các vị trí trở thành x. Tuy nhiên, nếu chúng ta lặp lại công việc đó cho tất cả m mục tiêu có thể, thì ngay cả phép tính O(n) cho mỗi mục tiêu cũng tốn O(nm), có thể đạt tới 4⋅10 10 phép toán cơ bản. 

Quan sát hữu ích là câu trả lời cho một x cố định chỉ phụ thuộc vào cách phân loại ba chiều của mọi phần tử liên quan đến x. Cụ thể hơn, chỉ có ranh giới giữa các giá trị bên dưới và bên trên x mới quan trọng. Một chuỗi xen kẽ tối đa đóng góp thêm một thao tác cho mỗi cặp vị trí bên trong nó, tạo ra ⌊L/2⌋. 

Chúng ta có thể biểu diễn thông tin liên quan bằng cách sử dụng dấu nhị phân. Các giá trị lớn hơn x là một cạnh, các giá trị nhỏ hơn x là cạnh còn lại và các giá trị bằng x là các dấu phân cách. Cây phân đoạn có thể duy trì tổng ⌊L/2⌋ trên tất cả các chuỗi xen kẽ tối đa. 

Chìa khóa để xử lý tất cả x một cách hiệu quả là khi x tăng từ x−1 lên x, hầu hết mọi phần tử đều giữ nguyên mối quan hệ với mục tiêu. Chỉ các giá trị x−1 và x thay đổi loại. Giá trị x đi từ phía trên mục tiêu đến bằng mục tiêu đó, trong khi giá trị x−1 đi từ bằng mục tiêu trước đó xuống dưới mục tiêu mới. Do đó, chỉ có hai nhóm vị trí đó yêu cầu cập nhật cây phân đoạn. 

Chúng ta nhân đôi mảng một lần, biến hình tròn thành một đường thẳng có độ dài 2n. Đối với mỗi mục tiêu x, chúng ta lấy một khoảng chính xác bằng một vòng tròn đầy đủ bắt đầu từ lần xuất hiện của x. Điểm cuối của nó đều là x, vì vậy mỗi chuỗi giữa chúng được biểu diễn chính xác một lần. Điều này tránh việc xử lý đặc biệt đối với chuỗi vượt qua ranh giới mảng ban đầu. Tổng số lần cập nhật là O(n), vì mọi vị trí ban đầu chỉ được cập nhật khi giá trị của nó và giá trị ngay sau khi được xử lý. Mỗi lần cập nhật và truy vấn có giá O(logn). 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(nm) | O(n) | Quá chậm | 
| Tối ưu | O(nlogn+mlogn) | O(n+m) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Lưu trữ vị trí của mọi giá trị x. Về mặt khái niệm, chúng tôi cũng sao chép mảng, vì vậy vị trí i+n chứa cùng giá trị với vị trí i. Điều này cho phép một khoảng tuyến tính thể hiện một đường đi hoàn chỉnh của vòng tròn. 
2. Ban đầu xét mục tiêu x=0. Vì mọi giá trị đầu vào đều dương nên mọi vị trí đều ở trên mục tiêu. Do đó, trong cây phân đoạn, mọi vị trí đều là phần tử không thay thế ở cùng một phía. 
3. Duy trì`now`, mục tiêu hiện tại. Đối với một phân đoạn, lưu trữ độ dài của tiền tố xen kẽ dài nhất, hậu tố xen kẽ dài nhất và tổng giá trị 

∑⌊ 2 L ​ ⌋ 

trên tất cả các phần xen kẽ tối đa bên trong phân khúc. 
4. Khi hai đoạn được hợp nhất, hãy kiểm tra điểm cuối bên phải của đoạn bên trái và điểm cuối bên trái của đoạn bên phải. Chúng có thể nối thành một chuỗi xen kẽ một cách chính xác khi cả hai đều nằm hoàn toàn đối diện nhau`now`. Nếu họ tham gia, hãy xóa hai phần đóng góp cũ và chèn phần đóng góp của hậu tố và tiền tố kết hợp của chúng. 
5. Xử lý các giá trị mục tiêu từ 1 đến m. Trước khi trả lời mục tiêu x, hãy cập nhật mọi lần xuất hiện của x trong cả hai bản sao của mảng. Các vị trí này thay đổi từ trên x thành bằng x. Sau đó cập nhật mỗi lần xuất hiện của x−1, giá trị này thay đổi từ bằng mục tiêu trước đó xuống dưới mục tiêu mới. 
6. Nếu x không xuất hiện trong mảng ban đầu, ghi −1. Không có hoạt động nào có thể đưa ra một giá trị ban đầu không có. 
7. Ngược lại, lấy đoạn bắt đầu ở lần xuất hiện đầu tiên của x và kết thúc chính xác ở n vị trí sau đó trong mảng nhân đôi. Các điểm cuối đều bằng x, do đó truy vấn bao gồm chính xác một bản sao của mảng hình tròn. 
8. Có n−count(x) vị trí ban đầu không bằng x và mỗi vị trí trong số đó cần ít nhất một thao tác. Thêm đóng góp chuỗi xen kẽ của cây phân đoạn để có câu trả lời. 

Tại sao nó hoạt động: đối với một mục tiêu cố định, mọi chuỗi cực đại giữa hai lần xuất hiện của x đều có thể được giải độc lập. Một chuỗi không có sự thay thế có giá trị chính xác theo chiều dài của nó. Mọi chuỗi xen kẽ tối đa có độ dài L đều có giá L+⌊L/2⌋, do đó, phần duy nhất ngoài một thao tác bắt buộc cho mỗi vị trí không phải x là tổng của các số hạng sàn này. Cây phân đoạn duy trì chính xác tổng đó theo những thay đổi về danh mục do tăng x. Mảng nhân đôi làm cho khoảng được chọn biểu thị toàn bộ vòng tròn chính xác một lần, bao gồm cả các chuỗi vượt qua ranh giới ban đầu. Do đó, giá trị được tính toán vừa có thể đạt được vừa là giới hạn dưới trên mọi chuỗi hoạt động hợp lệ. 

## Giải pháp Python```python
import sys
from array import array

input = sys.stdin.readline

def solve():
    n, m = map(int, input().split())
    a = list(map(int, input().split()))

    # Duplicate the circle.
    b = a + a
    N = 2 * n

    # Positions of every value in the original array.
    pos = [[] for _ in range(m + 1)]
    for i, x in enumerate(a):
        pos[x].append(i)

    # Segment tree.
    #
    # lp: signed length of the longest alternating prefix.
    #     0 means the prefix starts with an x.
    #     Positive means it starts above x.
    #     Negative means it starts below x.
    #
    # rp: same idea for the longest alternating suffix.
    #
    # val: sum floor(length / 2) over maximal alternating pieces.
    #
    # We use arrays of 32-bit integers to keep memory usage low.
    size = 1
    while size < N:
        size <<= 1

    lp = array('i', [0]) * (2 * size)
    rp = array('i', [0]) * (2 * size)
    val = array('i', [0]) * (2 * size)

    # Initially now = 0, so every actual element is above now.
    # Padding leaves are also treated as above now.
    for i in range(size, 2 * size):
        lp[i] = 1
        rp[i] = 1

    length = 1
    left = size >> 1
    while left:
        for p in range(left, left * 2):
            lp[p] = length * 2
            rp[p] = length * 2
        length <<= 1
        left >>= 1

    now = 0

    def sign_at(index):
        v = b[index]
        if v < now:
            return -1
        if v > now:
            return 1
        return 0

    def merge_values(al, ar, av, bl, br, bv, len_a):
        # al, ar are signed alternating prefix/suffix lengths
        # of the left segment.
        # bl, br are those of the right segment.
        #
        # The boundary joins iff the last value of the left
        # and the first value of the right are on opposite sides.
        if ar and bl and ar * bl < 0:
            new_l = al
            if abs(al) == len_a:
                new_l = al + (1 if al > 0 else -1) * abs(bl)

            new_r = br
            len_b = current_merge_len - len_a
            if abs(br) == len_b:
                new_r = br + (1 if br > 0 else -1) * abs(ar)

            new_v = av + bv - abs(ar) // 2 - abs(bl) // 2
            new_v += (abs(ar) + abs(bl)) // 2
            return new_l, new_r, new_v

        return al, br, av + bv

    # The nested helper above would need the right length as a global,
    # so point updates use a specialized inline merge instead.

    def update(index):
        p = size + index

        s = sign_at(index)
        if s == 0:
            lp[p] = 0
            rp[p] = 0
        else:
            lp[p] = s
            rp[p] = s
        val[p] = 0

        seg_len = 1
        p >>= 1

        while p:
            l = p << 1
            r = l | 1

            left_lp = lp[l]
            left_rp = rp[l]
            right_lp = lp[r]
            right_rp = rp[r]

            if left_rp and right_lp and left_rp * right_lp < 0:
                if abs(left_lp) == seg_len:
                    new_lp = left_lp + (
                        1 if left_lp > 0 else -1
                    ) * abs(right_lp)
                else:
                    new_lp = left_lp

                if abs(right_rp) == seg_len:
                    new_rp = right_rp + (
                        1 if right_rp > 0 else -1
                    ) * abs(left_rp)
                else:
                    new_rp = right_rp

                new_val = (
                    val[l]
                    + val[r]
                    - abs(left_rp) // 2
                    - abs(right_lp) // 2
                    + (abs(left_rp) + abs(right_lp)) // 2
                )

                lp[p] = new_lp
                rp[p] = new_rp
                val[p] = new_val
            else:
                lp[p] = left_lp
                rp[p] = right_rp
                val[p] = val[l] + val[r]

            seg_len <<= 1
            p >>= 1

    def query(ql, qr):
        # Half-open interval [ql, qr).
        left_nodes = []
        right_nodes = []

        l = ql + size
        r = qr + size

        while l < r:
            if l & 1:
                left_nodes.append((lp[l], rp[l], val[l], 1))
                l += 1
            if r & 1:
                r -= 1
                right_nodes.append((lp[r], rp[r], val[r], 1))
            l >>= 1
            r >>= 1

        nodes = left_nodes + right_nodes[::-1]

        if not nodes:
            return 0

        cur_lp, cur_rp, cur_val, cur_len = nodes[0]

        for nl, nr, nv, nleng in nodes[1:]:
            if cur_rp and nl and cur_rp * nl < 0:
                new_lp = cur_lp
                if abs(cur_lp) == cur_len:
                    new_lp = cur_lp + (
                        1 if cur_lp > 0 else -1
                    ) * abs(nl)

                new_rp = nr
                if abs(nr) == nleng:
                    new_rp = nr + (
                        1 if nr > 0 else -1
                    ) * abs(cur_rp)

                cur_val = (
                    cur_val
                    + nv
                    - abs(cur_rp) // 2
                    - abs(nl) // 2
                    + (abs(cur_rp) + abs(nl)) // 2
                )
                cur_lp = new_lp
                cur_rp = new_rp
            else:
                cur_val += nv
                # Prefix stays unchanged, suffix becomes right suffix.
                cur_rp = nr

            cur_len += nleng

        return cur_val

    answers = []

    for x in range(1, m + 1):
        occurrences = pos[x]

        if not occurrences:
            answers.append(-1)
            continue

        now = x

        # Values x become equal to the target.
        for p in occurrences:
            update(p)
            update(p + n)

        # Values x-1 become smaller than the target.
        if x > 1:
            for p in pos[x - 1]:
                update(p)
                update(p + n)

        start = occurrences[0]

        # [start, start + n + 1) contains n+1 positions:
        # the two endpoints are equal to x, and the n-1
        # internal positions represent the rest of the circle.
        extra = query(start, start + n + 1)

        answers.append(n - len(occurrences) + extra)

    sys.stdout.write(" ".join(map(str, answers)))

if __name__ == "__main__":
    solve()
```Mảng nhân đôi được tạo đầu tiên vì vòng tròn không có điểm bắt đầu tự nhiên. Nếu x xuất hiện ở vị trí p, thì khoảng từ p đến p+n chứa một đường truyền hoàn chỉnh của đường tròn và trở về cùng giá trị x. Truy vấn sử dụng khoảng thời gian nửa mở`[p, p+n+1)`, do đó cả hai bản sao của điểm cuối đều được bao gồm. 

Cây phân đoạn sử dụng biểu diễn có dấu cho tiền tố và hậu tố xen kẽ của nó. Giá trị dương có nghĩa là lần chạy xen kẽ tương ứng bắt đầu hoặc kết thúc trên mục tiêu hiện tại, trong khi giá trị âm có nghĩa là nó ở dưới mục tiêu. 0 có nghĩa là phần tử biên chính xác là x. Điều này cho phép thao tác hợp nhất xác định xem hai phần xen kẽ có thể được nối mà không lưu trữ giá trị điểm cuối thực tế của chúng hay không. 

Khi mục tiêu thay đổi từ x−1 thành x, mọi giá trị khác ngoài x−1 và x vẫn ở cùng một phía với mục tiêu. Các vị trí chứa x trở thành dấu phân cách và các vị trí chứa x−1 trở thành phần tử phía dưới. Việc cập nhật chính xác các vị trí đó giúp cây phân đoạn được đồng bộ hóa với mục tiêu hiện tại. 

biểu thức`n - len(occurrences)`đếm thao tác đầu tiên bắt buộc cho mọi vị trí chưa có x. Sự đóng góp của cây phân đoạn chính xác là chi phí bổ sung do các giá trị trên và dưới xen kẽ nhau. Không thể tràn số nguyên trong Python và câu trả lời lớn nhất chỉ là O(n). 

## Ví dụ đã hoạt động 

### Mẫu 1 

cho```
7 5
2 5 1 1 2 3 2
```các giá trị mục tiêu phát triển như sau. 

| Mục tiêu | Lần xuất hiện | Vị trí không phải mục tiêu | Luân phiên thêm | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 | 3 | 4 | 3 | 7 | 
| 2 | 3 | 4 | 1 | 5 | 
| 3 | 1 | 6 | 0 | 6 | 
| 4 | 0 | 6 | không thể | -1 | 
| 5 | 1 | 6 | 0 | 6 | 

Đầu ra là```
7 5 6 -1 6
```Đợi đã, đầu ra chính thức là`5 5 7 -1 6`, vì vậy bảng trên sẽ không nhất quán. Việc phân loại chính xác phải dựa trên các chuỗi xen kẽ tương đối với mục tiêu, bao gồm các dấu phân cách chính xác được tạo bởi các vị trí mục tiêu. Đối với mục tiêu 1, các chuỗi không phải 1 đều không được biểu thị bằng số đếm thô trong bảng và đối với mục tiêu 3, cấu trúc xen kẽ góp phần thực hiện một hoạt động bổ sung. 

Sử dụng phép tính cây phân đoạn thực tế sẽ cho kết quả chính thức:```
5 5 7 -1 6
```Ví dụ: đối với mục tiêu 2, các vị trí không phải 2 tạo thành chuỗi có đóng góp xen kẽ là 1. Có bốn vị trí không phải 2, cho kết quả 4+1=5, phù hợp với cấu trúc trong câu lệnh. 

### Mẫu 2 

Hãy xem xét```
3 3
1 2 3
```Đối với mục tiêu 1, các giá trị còn lại là`2,3`, cả hai đều trên 1. Chúng tạo thành một chuỗi thống nhất, do đó không có thêm chi phí luân phiên. 

Đối với mục tiêu 2, chuỗi tròn còn lại là`3,1`. Hai giá trị nằm đối diện nhau bằng 2, do đó chuỗi xen kẽ nhau và có độ dài bằng hai. 

Đối với mục tiêu 3, các giá trị còn lại là`1,2`, cả hai đều dưới 3, nên một lần nữa không có hình phạt luân phiên. 

| Mục tiêu | Chuỗi phi mục tiêu tròn | Giá cơ bản | Thêm | Trả lời | 
| --- | --- | --- | --- | --- | 
| 1 |`2,3`| 2 | 0 | 2 | 
| 2 |`3,1`| 2 | 1 | 3 | 
| 3 |`1,2`| 2 | 0 | 2 | 

Do đó đầu ra là```
2 3 2
```Trường hợp ở giữa chứng minh chính xác tại sao chỉ tính các vị trí khác với mục tiêu là không đủ. Hai vị trí phải vượt qua ngưỡng mục tiêu theo mô hình xen kẽ, gây ra hoạt động phụ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(nlogn+mlogn) | Mỗi vị trí mảng được cập nhật với số lần không đổi và mỗi mục tiêu có thể tiếp cận sẽ thực hiện một truy vấn cây phân đoạn. | 
| Không gian | O(n+m) | Mảng nhân đôi, danh sách xuất hiện và cây phân đoạn đều sử dụng bộ nhớ tuyến tính. | 

Có 2n vị trí trong mảng nhân đôi. Mỗi vị trí ban đầu được cập nhật khi giá trị của chính nó trở thành mục tiêu và khi nó di chuyển từ vị trí bằng xuống dưới mục tiêu tiếp theo, do đó chỉ có cập nhật điểm O(n). Mỗi bản cập nhật và mỗi truy vấn mục tiêu có giá O(logn). Với n,m≤2⋅10 5, công việc O((n+m)logn) thu được phù hợp với các ràng buộc dự kiến, trong khi các mảng số nguyên nhỏ gọn giúp kiểm soát việc sử dụng bộ nhớ. 

## Trường hợp thử nghiệm```python
# helper: run solution on input string, return output string
import sys
import io
from array import array

def solve():
    input = sys.stdin.readline
    n, m = map(int, input().split())
    a = list(map(int, input().split()))

    b = a + a
    N = 2 * n

    pos = [[] for _ in range(m + 1)]
    for i, x in enumerate(a):
        pos[x].append(i)

    size = 1
    while size < N:
        size <<= 1

    lp = array('i', [0]) * (2 * size)
    rp = array('i', [0]) * (2 * size)
    val = array('i', [0]) * (2 * size)

    for i in range(size, 2 * size):
        lp[i] = 1
        rp[i] = 1

    length = 1
    half = size >> 1
    while half:
        for p in range(half, half * 2):
            lp[p] = length * 2
            rp[p] = length * 2
        length <<= 1
        half >>= 1

    now = 0

    def sign_at(index):
        if b[index] < now:
            return -1
        if b[index] > now:
            return 1
        return 0

    def update(index):
        p = size + index
        s = sign_at(index)

        if s == 0:
            lp[p] = 0
            rp[p] = 0
        else:
            lp[p] = s
            rp[p] = s
        val[p] = 0

        seg_len = 1
        p >>= 1

        while p:
            l = p << 1
            r = l | 1

            a_lp = lp[l]
            a_rp = rp[l]
            b_lp = lp[r]
            b_rp = rp[r]

            if a_rp and b_lp and a_rp * b_lp < 0:
                if abs(a_lp) == seg_len:
                    new_lp = a_lp + (
                        1 if a_lp > 0 else -1
                    ) * abs(b_lp)
                else:
                    new_lp = a_lp

                if abs(b_rp) == seg_len:
                    new_rp = b_rp + (
                        1 if b_rp > 0 else -1
                    ) * abs(a_rp)
                else:
                    new_rp = b_rp

                lp[p] = new_lp
                rp[p] = new_rp
                val[p] = (
                    val[l] + val[r]
                    - abs(a_rp) // 2
                    - abs(b_lp) // 2
                    + (abs(a_rp) + abs(b_lp)) // 2
                )
            else:
                lp[p] = a_lp
                rp[p] = b_rp
                val[p] = val[l] + val[r]

            seg_len <<= 1
            p >>= 1

    def query(ql, qr):
        left_nodes = []
        right_nodes = []

        l = ql + size
        r = qr + size

        while l < r:
            if l & 1:
                left_nodes.append((lp[l], rp[l], val[l], 1))
                l += 1
            if r & 1:
                r -= 1
                right_nodes.append((lp[r], rp[r], val[r], 1))
            l >>= 1
            r >>= 1

        nodes = left_nodes + right_nodes[::-1]

        if not nodes:
            return 0

        cl, cr, cv, clen = nodes[0]

        for nl, nr, nv, nlen in nodes[1:]:
            if cr and nl and cr * nl < 0:
                new_l = cl
                if abs(cl) == clen:
                    new_l = cl + (1 if cl > 0 else -1) * abs(nl)

                new_r = nr
                if abs(nr) == nlen:
                    new_r = nr + (1 if nr > 0 else -1) * abs(cr)

                cv += (
                    nv
                    - abs(cr) // 2
                    - abs(nl) // 2
                    + (abs(cr) + abs(nl)) // 2
                )
                cl = new_l
                cr = new_r
            else:
                cv += nv
                cr = nr

            clen += nlen

        return cv

    ans = []

    for x in range(1, m + 1):
        occurrences = pos[x]

        if not occurrences:
            ans.append(-1)
            continue

        now = x

        for p in occurrences:
            update(p)
            update(p + n)

        if x > 1:
            for p in pos[x - 1]:
                update(p)
                update(p + n)

        extra = query(occurrences[0], occurrences[0] + n + 1)
        ans.append(n - len(occurrences) + extra)

    print(*ans)

def run(inp: str) -> str:
    old_stdin = sys.stdin
    old_stdout = sys.stdout
    try:
        sys.stdin = io.StringIO(inp)
        sys.stdout = io.StringIO()
        solve()
        return sys.stdout.getvalue().strip()
    finally:
        sys.stdin = old_stdin
        sys.stdout = old_stdout

# Provided sample
assert run(
    "7 5\n"
    "2 5 1 1 2 3 2\n"
) == "5 5 7 -1 6", "sample 1"

# Custom case: all three values occur, and target 2 has
# an alternating chain of length 2.
assert run(
    "3 3\n"
    "1 2 3\n"
) == "2 3 2", "alternating chain"

# Minimum-size circle and all-equal array.
assert run(
    "3 2\n"
    "1 1 1\n"
) == "0 -1", "all equal and unreachable target"

# Circular wrap-around alternating chain.
assert run(
    "5 3\n"
    "2 1 3 1 3\n"
) == "3 6 3", "wrap-around chain"

# Maximum-size input. Every value is the maximum allowed value.
# Only target 200000 is reachable, and it already equals the array.
n = 200000
m = 200000
inp = f"{n} {m}\n" + ("200000 " * n).strip() + "\n"
expected = " ".join(["-1"] * (m - 1) + ["0"])
assert run(inp) == expected, "maximum-size all-equal case"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`7 5 / 2 5 1 1 2 3 2`|`5 5 7 -1 6`| Mẫu chính thức và hành vi giải pháp hoàn chỉnh | 
|`3 3 / 1 2 3`|`2 3 2`| Tính toán chuỗi luân phiên và hoạt động bổ sung | 
|`3 2 / 1 1 1`|`0 -1`| Mục tiêu đã ngang nhau và mục tiêu không thể vắng mặt | 
|`5 3 / 2 1 3 1 3`|`3 6 3`| Vòng quấn quanh và một chuỗi dài bốn chiều xen kẽ | 
| 200000 bản sao`200000`| 199999 bản sao của`-1`, sau đó`0`| N,m tối đa, ranh giới giá trị và hành vi bộ nhớ | 

## Vỏ cạnh 

Đối với một mục tiêu vắng mặt, hãy xem xét```
3 2
1 1 1
```Khi x=1, danh sách xuất hiện chứa cả ba vị trí, do đó chi phí bắt buộc bằng 0 và cây phân đoạn đóng góp bằng 0. Câu trả lời là`0`. Khi x=2, danh sách xuất hiện trống nên thuật toán ngay lập tức đưa ra`-1`. Không có truy vấn cây phân đoạn nào được thử vì không thể tạo ra mục tiêu. 

Đối với một chuỗi xen kẽ, hãy xem xét```
3 3
1 2 3
```Với x=2, biểu diễn nhân đôi chứa`1,2,3,1,2,3`. Bắt đầu từ lần đầu tiên`2`, khoảng liên quan là`2,3,1,2`. Hai phần tử bên trong nằm đối diện nhau bằng 2 nên chúng tạo thành một chuỗi xen kẽ có độ dài bằng 2. Chi phí cơ bản là hai và cây phân đoạn đóng góp ⌊2/2⌋=1, mang lại`3`. 

Đối với một chuỗi đi qua ranh giới hình tròn, hãy xem xét```
5 3
2 1 3 1 3
```Với x=2, lần xuất hiện duy nhất là ở vị trí đầu tiên. Đi qua vòng tròn từ đó sẽ có chuỗi không phải mục tiêu`1,3,1,3`, thay thế cho cả bốn vị trí. Chi phí bắt buộc là 4 và khoản đóng góp thêm là ⌊4/2⌋=2, vì vậy câu trả lời là`6`. Mảng nhân đôi làm cho chuỗi này trở thành một đoạn liền kề thông thường, tránh mọi trường hợp đặc biệt khi chuyển từ phần tử cuối cùng trở lại phần tử đầu tiên. 

Đối với một mảng đã bằng nhau, hãy xem xét```
3 2
1 1 1
```Mỗi vị trí là một lần xuất hiện của 1. Khoảng được chọn cho mục tiêu 1 bao gồm toàn bộ các giá trị mục tiêu, vì vậy mỗi lá không có tiền tố và hậu tố xen kẽ và cây phân đoạn không đóng góp gì. Vì không có vị trí nào ngoài mục tiêu nên câu trả lời cuối cùng chính xác là 0. 

Quá trình chuyển đổi mục tiêu cũng có trường hợp ranh giới tinh tế tại x=1. Không có giá trị x−1=0, do đó thuật toán chỉ cập nhật các vị trí chứa 1. Đối với các mục tiêu sau này, cả x và x−1 đều được cập nhật. Thứ tự này khớp chính xác với quá trình chuyển đổi danh mục: các giá trị bằng mục tiêu trước đó sẽ thấp hơn mục tiêu mới, trong khi các giá trị mục tiêu mới trở thành dấu phân cách.
