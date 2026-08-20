---
title: "CF 102185I - \u0425\u0430\u043e\u0442\u0438\u0447\u043d\u044b\u0435 \u043f\u043b\u044e\u043c\u0431\u0443\u0441\u044b"
description: "Chúng ta có một hàng chứa chính xác N quả mận với mỗi K màu, vì vậy tổng chiều dài là N nhân K. Thao tác duy nhất được phép lấy một quả mận hiện có ra khỏi hàng và chèn nó ở bên trái hoặc bên phải."
date: "2026-08-19T06:43:45+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102185
codeforces_index: "I"
codeforces_contest_name: "Southern Russia Open Championship \u2013 ContestSFedU 2019, Team Final."
rating: 0
weight: 102185
solve_time_s: 303
verified: true
draft: false
---

[CF 102185I - \u0425\u0430\u043e\u0442\u0438\u0447\u043d\u044b\u0435 \u043f\u043b\u044e\u043c\u0431\u0443\u0441\u044b](https://codeforces.com/problemset/problem/102185/I) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 5 phút 3 giây 
**Đã xác minh:** có 

## Giải pháp 
## Hiểu vấn đề 

Chúng ta có một hàng chứa chính xác N quả mận với mỗi K màu, vì vậy tổng chiều dài là N nhân K. Thao tác duy nhất được phép lấy một quả mận hiện có ra khỏi hàng và chèn nó ở bên trái hoặc bên phải. Mục tiêu là làm cho mỗi màu chiếm một khối liền kề. Thứ tự của các khối màu này là tùy ý. 

Khó khăn chính là hoạt động không cho phép hoán đổi tùy ý. Mỗi quả mận mà chúng ta không bao giờ di chuyển sẽ giữ nguyên trật tự tương đối của nó với mọi quả mận không bao giờ di chuyển khác. Cuối cùng, các dây dọi đã di chuyển có thể được bố trí ở hai đầu, trong khi các dây dọi chưa được chạm tới sẽ tạo thành phần giữa của hàng cuối cùng. 

Câu trả lời là số lượng ống dẫn được di chuyển tối thiểu. Tương tự, chúng tôi muốn tối đa hóa số lượng dây dọi có thể giữ nguyên vị trí của chúng. 

Các ràng buộc cung cấp N,K nhiều nhất là 1000, trong khi bản thân mảng có thể chứa 1.000.000 phần tử. Một thuật toán thực hiện công việc thậm chí O(NK nhân K) đã có khoảng 10^9 thao tác trong trường hợp xấu nhất và không thể sử dụng được. Về cơ bản, chúng ta cần công việc tuyến tính trong mảng cộng với hầu hết các công việc bậc hai về số lượng màu. O(NK + K^2) đủ nhỏ và giới hạn bộ nhớ cũng cho phép lưu trữ mảng ban đầu. 

Có một số trường hợp ranh giới có thể đánh lừa việc triển khai. 

Với N=1 và K=1,```
1 1
1
```câu trả lời là 0. Chỉ có một màu duy nhất và một màu duy nhất của nó đã tạo thành một nhóm hợp lệ. Việc triển khai giả định tồn tại hai màu điểm cuối khác nhau có thể tính sai một bước di chuyển. 

Với N=2 và K=2,```
2 2
1 2 1 2
```câu trả lời là 2. Chỉ giữ lại một màu hoàn chỉnh để giữ nguyên hai quả mận, vì vậy hai lần di chuyển là đủ. Một giải pháp bất cẩn chỉ xem xét các khối màu liền kề nhau có thể bỏ lỡ khả năng này. 

Một trường hợp tế nhị hơn là```
3 2
1 1 2 2 1 2
```có câu trả lời là 2. Hai số 1 đầu tiên và hai số 2 đầu tiên đã được nhóm lại và việc di chuyển số 1 và 2 còn lại sang đầu thích hợp sẽ hoàn thành cả hai nhóm. Một giải pháp nhấn mạnh rằng một số màu phải được giữ hoàn toàn có thể bỏ lỡ loại tối ưu này, bởi vì ở đây phần giữa không bị ảnh hưởng tốt nhất có thể bao gồm hai màu điểm cuối một phần. 

## Phương pháp tiếp cận 

Một lực lượng vũ phu trực tiếp sẽ chọn những chiếc mận nào được di chuyển và những chiếc mận nào còn lại, sau đó kiểm tra xem những chiếc mận còn lại có thể hình thành ở giữa một sự sắp xếp cuối cùng hợp lệ hay không. Điều này đúng vì mọi chuỗi hoạt động hợp pháp đều được xác định hoàn toàn, đối với các phần tử chưa được chạm tới, bởi tập hợp các phần tử đã được di chuyển. Tuy nhiên, có thể có 2^(NK) tập hợp con. Với NK lớn tới 1.000.000 thì việc ghi hết tất cả các ứng viên là điều không thể. Việc liệt kê các hoán vị của khối màu K cũng là vô vọng, vì K! đã là rất lớn với K=1000. 

Quan sát hữu ích là quan sát các dây dọi không bị dịch chuyển. Thứ tự tương đối của chúng không bao giờ thay đổi và tất cả các phần tử được di chuyển đều nằm ngoài chúng. Do đó, các dây dọi chưa được chạm tới phải xuất hiện ở hàng cuối cùng dưới dạng một chuỗi ở giữa, trong đó mỗi màu tạo thành nhiều nhất một lần chạy. 

Giả sử một màu xuất hiện ở đâu đó bên trong dãy nguyên sơ đó. Nếu một trong N dây dọi của nó đã được di chuyển thì quả mận đã di chuyển đó sẽ phải được đặt ở một trong hai đầu của toàn bộ hàng. Nó không thể trở lại bên cạnh nhóm nội thất này. Do đó, màu nội thất phải có tất cả N lần xuất hiện nguyên vẹn. 

Chỉ nhóm màu đầu tiên và cuối cùng của chuỗi màu chưa được chạm tới mới có thể là một phần. Chúng có thể có một số lần xuất hiện được chuyển đến đầu bên ngoài tương ứng. Như vậy mọi giải pháp tối ưu đều có cấu trúc như sau:```
partial left color
complete color
complete color
...
complete color
partial right color
```Hai màu một phần cũng có thể vắng mặt. Nếu không có màu hoàn chỉnh nào cả, chuỗi không được chạm tới sẽ bao gồm một phần màu bên trái, sau đó là một phần màu bên phải. 

Bây giờ hãy xem xét màu c được giữ nguyên hoàn toàn. Đặt first[c] và Last[c] là vị trí đầu tiên và cuối cùng trong mảng ban đầu. Nếu một màu khác d cũng được giữ hoàn toàn trước c thì mỗi lần xuất hiện của d phải trước mỗi lần xuất hiện của c. Điều kiện chính xác là```
last[d] < first[c].
```Do đó, các màu hoàn chỉnh tạo thành một chuỗi các khoảng không chồng chéo [đầu tiên [c], cuối cùng [c]]. Vì tất cả các màu hoàn chỉnh đóng góp chính xác N dây dọi chưa được chạm tới, nên bài toán trở thành bài toán lập trình động trên các khoảng màu này. 

Còn một chi tiết nữa. Nếu c là màu hoàn chỉnh đầu tiên, chúng tôi có thể giữ thêm một số lần xuất hiện của màu khác trước first[c]. Màu tốt nhất như vậy chỉ đơn giản là màu xuất hiện thường xuyên nhất trong tiền tố đó. Tương tự như vậy, sau màu hoàn chỉnh cuối cùng, chúng ta có thể giữ lại màu thường xuyên nhất ở hậu tố. 

Chúng ta phải nhớ danh tính của các màu điểm cuối này, không chỉ số lượng của chúng. Không thể sử dụng cùng một màu cho cả hai mặt vì khi đó những lần xuất hiện được giữ lại của nó sẽ tạo thành hai nhóm riêng biệt. Giữ hai ứng cử viên tốt nhất ở mỗi bên là đủ để giải quyết xung đột này. 

Trường hợp không có màu sắc hoàn chỉnh sẽ được xử lý riêng. Đối với mỗi vết cắt giữa hai vị trí, chúng tôi tìm thấy hai màu riêng biệt tốt nhất bao gồm màu tiền tố ở bên trái và màu hậu tố ở bên phải. Quét chuyển tiếp có thể duy trì hai màu tiền tố thường xuyên nhất. Quét ngược có thể duy trì hai màu hậu tố thường xuyên nhất. Vì thông tin tiền tố là cần thiết khi quét ngược đạt đến cùng một vết cắt nên nó được lưu trữ trong một mảng nhỏ gọn. 

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | O(2^(NK) · NK) | O(NK) | Quá chậm | 
| Tối ưu | O(NK + K2) | O(NK) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Đọc mảng và ghi lại`first[c]`Và`last[c]`cho mọi màu sắc. Hai vị trí này mô tả chính xác vị trí có thể xuất hiện màu hoàn toàn nguyên vẹn. 
2. Quét mảng từ trái sang phải trong khi vẫn duy trì số lần xuất hiện của mỗi màu trong tiền tố. Khi đạt đến lần xuất hiện đầu tiên của màu c, hãy lưu hai màu thường xuyên nhất vào các vị trí trước đó.`first[c]`. Đây là những ứng cử viên duy nhất có thể đóng vai trò là màu một phần ở bên trái của khối c hoàn chỉnh. 
3. Trong cùng quá trình quét tiến, hãy lưu hai màu thường xuyên nhất sau mỗi lần cắt có thể. Chỉ cần có hai ứng cử viên xuất sắc nhất vì tình huống bị cấm duy nhất là chọn hai bên cùng màu. 
4. Quét mảng từ phải sang trái. Trước khi xử lý lần xuất hiện cuối cùng của màu c, số lượng hiện tại mô tả chính xác hậu tố sau`last[c]`. Lưu hai màu tốt nhất của nó làm màu điểm cuối phù hợp có thể có cho chuỗi kết thúc tại c. 
5. Sắp xếp tất cả các màu theo lần xuất hiện đầu tiên của chúng. Đối với mỗi màu c, tạo trạng thái DP đại diện cho chuỗi màu hoàn chỉnh kết thúc tại c. Trạng thái ban đầu bao gồm chính c cộng với một trong hai ứng cử viên điểm cuối bên trái tốt nhất trước đó`first[c]`. 
6. Để mở rộng chuỗi kết thúc tại d với màu c, yêu cầu`last[d] < first[c]`. Điều này có nghĩa là mọi lần xuất hiện của d đều nằm trước mỗi lần xuất hiện của c, vì vậy cả hai màu có thể vẫn hoàn toàn không bị ảnh hưởng. Việc thêm c sẽ làm tăng số dây dọi chưa được chạm tới đúng N. 
7. Đối với mỗi màu kết thúc c, hãy giữ lại hai trạng thái DP tốt nhất với các màu điểm cuối bên trái khác nhau. Hai trạng thái là đủ vì khi điểm cuối bên phải được chọn, chỉ một màu bên trái có thể bị cấm. 
8. Kết hợp mọi trạng thái DP kết thúc ở c với hai ứng cử viên hậu tố tốt nhất sau`last[c]`. Chỉ từ chối sự kết hợp khi cả hai màu điểm cuối đều có cùng màu khác 0. Giá trị thu được là số lượng lớn nhất các dây dọi chưa được chạm tới trong dung dịch chứa ít nhất một màu hoàn chỉnh. 
9. Xử lý riêng biệt mọi lần cắt với các ứng cử viên tiền tố được lưu trữ và các ứng cử viên hậu tố được duy trì động. Điều này xử lý các giải pháp không chứa màu hoàn chỉnh, trong đó toàn bộ chuỗi chưa được xử lý bao gồm hai màu điểm cuối một phần. 
10. Hãy để`best`là số lượng lớn nhất các dây dọi chưa được chạm tới trong cả hai trường hợp. Câu trả lời bắt buộc là`N*K - best`, bởi vì mỗi quả mận không được tính là chưa chạm tới phải được di chuyển đúng một lần. 

### Tại sao nó hoạt động 

Hãy xem xét bất kỳ chuỗi hoạt động tối ưu nào và chỉ nhìn vào các dây dọi không bao giờ được di chuyển. Thứ tự tương đối của chúng không thay đổi nên chúng xuất hiện dưới dạng một chuỗi ở giữa trong cách sắp xếp cuối cùng. Mỗi màu xuất hiện bên trong chuỗi đó phải có tất cả N lần xuất hiện ban đầu của nó ở đó, nếu không, sự xuất hiện của màu đó sẽ phải vượt qua một nhóm không liên quan để trở về nhóm của chính nó. Chỉ nhóm đầu tiên và nhóm cuối cùng của dãy giữa mới có thể là một phần. 

Do đó, mọi giải pháp tối ưu đều được thể hiện bằng một chuỗi các màu hoàn chỉnh với nhiều nhất một phần màu ở mỗi bên hoặc bằng hai phần màu không có màu hoàn chỉnh. Điều kiện khoảng`last[d] < first[c]`mô tả chính xác khi nào hai màu có thể hoàn chỉnh và xuất hiện theo thứ tự đó. DP liệt kê mọi chuỗi có thể thỏa mãn điều kiện này, trong khi các ứng cử viên điểm cuối được lưu trữ liệt kê các màu từng phần tốt nhất có thể xung quanh nó. Quét cắt riêng biệt bao gồm trường hợp không có màu hoàn chỉnh còn lại. Vì mọi cấu hình chưa được chạm tới hợp lệ đều thuộc về một trong hai dạng này nên số lượng tối đa các đường thẳng chưa được chạm tới mà thuật toán tìm thấy là tối ưu. 

## Giải pháp Python```python
import sys
from array import array

input = sys.stdin.readline

BASE = 1024
BITS = 10
MASK = BASE - 1
PAIR_BITS = 20

def solve():
    n, k = map(int, input().split())
    a = list(map(int, input().split()))
    m = n * k

    first = [m] * k
    last = [-1] * k

    for i, x in enumerate(a):
        c = x - 1
        if first[c] == m:
            first[c] = i
        last[c] = i

    order = sorted(range(k), key=first.__getitem__)

    pref1 = [0] * k
    pref2 = [0] * k

    cnt = [0] * k
    t1 = 0
    id1 = 0
    t2 = 0
    id2 = 0

    # packed[i] stores the two best prefix candidates before position i.
    # Each candidate is encoded as count * BASE + color.
    packed = array('Q')
    packed.append(0)

    for i, x in enumerate(a):
        c = x - 1
        cid = c + 1

        if i == first[c]:
            pref1[c] = t1 * BASE + id1
            pref2[c] = t2 * BASE + id2

        cnt[c] += 1
        v = cnt[c]

        if cid == id1:
            t1 = v
        elif cid == id2:
            t2 = v
            if t2 > t1:
                t1, t2 = t2, t1
                id1, id2 = id2, id1
        elif v > t1:
            t2, id2 = t1, id1
            t1, id1 = v, cid
        elif v > t2:
            t2, id2 = v, cid

        e1 = t1 * BASE + id1
        e2 = t2 * BASE + id2
        packed.append((e1 << PAIR_BITS) | e2)

    # Suffix candidates for every color.
    suf1 = [0] * k
    suf2 = [0] * k

    cnt = [0] * k
    t1 = 0
    id1 = 0
    t2 = 0
    id2 = 0

    # No-complete-color case.
    best_no_full = 0

    # Cut m, where the suffix is empty.
    p = packed[m]
    pe1 = p >> PAIR_BITS
    pe2 = p & ((1 << PAIR_BITS) - 1)

    pc1 = pe1 >> BITS
    pi1 = pe1 & MASK
    pc2 = pe2 >> BITS
    pi2 = pe2 & MASK

    best_no_full = max(
        pc1,
        pc2,
    )

    for i in range(m - 1, -1, -1):
        c = a[i] - 1
        cid = c + 1

        if i == last[c]:
            suf1[c] = t1 * BASE + id1
            suf2[c] = t2 * BASE + id2

        cnt[c] += 1
        v = cnt[c]

        if cid == id1:
            t1 = v
        elif cid == id2:
            t2 = v
            if t2 > t1:
                t1, t2 = t2, t1
                id1, id2 = id2, id1
        elif v > t1:
            t2, id2 = t1, id1
            t1, id1 = v, cid
        elif v > t2:
            t2, id2 = v, cid

        # The current suffix is [i, m).
        p = packed[i]
        pe1 = p >> PAIR_BITS
        pe2 = p & ((1 << PAIR_BITS) - 1)

        pc1 = pe1 >> BITS
        pi1 = pe1 & MASK
        pc2 = pe2 >> BITS
        pi2 = pe2 & MASK

        # Combine the two best prefix and suffix colors.
        if pi1 == 0 or id1 == 0 or pi1 != id1:
            best_no_full = max(best_no_full, pc1 + t1)

        if pi1 == 0 or id2 == 0 or pi1 != id2:
            best_no_full = max(best_no_full, pc1 + t2)

        if pi2 != 0 and pi2 != id1:
            best_no_full = max(best_no_full, pc2 + t1)

        if pi2 != 0 and pi2 != id2:
            best_no_full = max(best_no_full, pc2 + t2)

    # DP states:
    # dp1[c], dp2[c] are the best two states ending with color c.
    # The second component is the color used as the left partial endpoint.
    dp1_val = [0] * k
    dp1_id = [0] * k
    dp2_val = [0] * k
    dp2_id = [0] * k

    for c in order:
        e1 = pref1[c]
        e2 = pref2[c]

        v1 = n + (e1 >> BITS)
        q1 = e1 & MASK

        v2 = n + (e2 >> BITS)
        q2 = e2 & MASK

        b1v = v1
        b1q = q1
        b2v = -1
        b2q = -1

        if q2 != q1:
            if v2 > b1v:
                b2v, b2q = b1v, b1q
                b1v, b1q = v2, q2
            else:
                b2v, b2q = v2, q2

        # Try every earlier complete color.
        for d in order:
            if first[d] >= first[c]:
                break
            if last[d] >= first[c]:
                continue

            v = dp1_val[d] + n
            q = dp1_id[d]

            if q == b1q:
                if v > b1v:
                    b1v = v
            elif q == b2q:
                if v > b2v:
                    b2v = v
            elif v > b1v:
                b2v, b2q = b1v, b1q
                b1v, b1q = v, q
            elif v > b2v:
                b2v, b2q = v, q

            v = dp2_val[d] + n
            q = dp2_id[d]

            if q == 0 and dp2_val[d] == 0:
                continue

            if q == b1q:
                if v > b1v:
                    b1v = v
            elif q == b2q:
                if v > b2v:
                    b2v = v
            elif v > b1v:
                b2v, b2q = b1v, b1q
                b1v, b1q = v, q
            elif v > b2v:
                b2v, b2q = v, q

        dp1_val[c] = b1v
        dp1_id[c] = b1q
        dp2_val[c] = b2v if b2v >= 0 else 0
        dp2_id[c] = b2q if b2v >= 0 else 0

    best = best_no_full

    for c in range(k):
        s1 = suf1[c]
        s2 = suf2[c]

        sc1 = s1 >> BITS
        si1 = s1 & MASK
        sc2 = s2 >> BITS
        si2 = s2 & MASK

        for dv, di in (
            (dp1_val[c], dp1_id[c]),
            (dp2_val[c], dp2_id[c]),
        ):
            if dv == 0:
                continue

            if di == 0 or si1 == 0 or di != si1:
                best = max(best, dv + sc1)

            if di == 0 or si2 == 0 or di != si2:
                best = max(best, dv + sc2)

    answer = m - best
    return str(answer)

if __name__ == "__main__":
    print(solve())
```Lần quét đầu tiên tính toán lần xuất hiện đầu tiên và cuối cùng của mỗi màu. Những ranh giới đó là thông tin duy nhất cần thiết để quyết định liệu cả hai màu có thể được giữ nguyên hoàn toàn hay không. 

Việc duy trì số chuyển tiếp phục vụ hai mục đích. Ở lần xuất hiện đầu tiên của một màu, số lượng hiện tại mô tả chính xác tiền tố trước màu đó. Ở mỗi lần cắt, giá trị đóng gói sẽ lưu trữ hai màu tiền tố tốt nhất, sau này cần thiết cho trường hợp không có màu hoàn chỉnh. 

Việc đóng gói sử dụng mười bit cho mã nhận dạng màu vì K nhiều nhất là 1000. Mười bit khác lưu trữ số đếm của nó, nhiều nhất là N. Hai ứng cử viên như vậy vừa khít với số nguyên 64 bit. sử dụng`array('Q')`giữ mức sử dụng bộ nhớ ở mức nhỏ mặc dù có thể có một triệu lần cắt. 

Quá trình quét ngược sẽ tính toán các ứng cử viên hậu tố. Trạng thái hậu tố trước khi xử lý vị trí i mô tả các vị trí sau i, đây chính xác là điều cần thiết khi`i`là lần xuất hiện cuối cùng của một màu. Sau khi thêm vị trí i, trạng thái hậu tố tương ứng với phần cắt trước i và có thể được kết hợp với trạng thái tiền tố đã được lưu trữ. 

DP lưu trữ hai trạng thái cho mỗi màu kết thúc thay vì một trạng thái. Lý do là màu điểm cuối bên trái tốt nhất có thể bằng màu điểm cuối bên phải tốt nhất. Giữ hai màu bên trái khác nhau đảm bảo rằng chúng ta có thể loại bỏ trạng thái xung đột khi chọn điểm cuối bên phải. 

Số nguyên Python không tràn ở đây. Số lượng mận chưa được chạm tới tối đa là N lần K, nhiều nhất là 1.000.000. Các giá trị ứng cử viên được mã hóa cũng thoải mái bên trong các số nguyên 64 bit. 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là```
3 3
1 2 3 3 2 1 1 2 3
```Các khoảng màu có liên quan là```
color 1: [1, 7]
color 2: [2, 8]
color 3: [3, 9]
```Không có hai khoảng nào rời nhau nên không có hai màu nào có thể trọn vẹn. Một lựa chọn hữu ích là màu 2 là nhóm ở giữa hoàn chỉnh. Một màu 1 có thể được giữ lại trước nó và một màu 3 có thể được giữ lại sau nó. 

| Tiểu bang | Giá trị | 
| --- | --- | 
| Màu hoàn chỉnh 2 | 3 | 
| Màu một phần bên trái tốt nhất 1 | 1 | 
| Màu một phần bên phải tốt nhất 3 | 1 | 
| Tổng số chưa được chạm tới | 5 | 
| Tổng số ống dẫn nước | 9 | 
| Di chuyển | 4 | 

Năm chiếc mận chưa được chạm tới có thể xuất hiện dưới dạng`1 2 2 2 3`. Tất cả bốn còn lại có thể được chuyển đến hai đầu. DP tìm thấy năm phần tử chưa được chạm tới, vì vậy câu trả lời là 9 trừ 5, bằng 4. 

### Mẫu 2 

Đầu vào là```
2 4
3 3 1 1 4 4 2 2
```Mỗi màu đã tạo thành một khối hoàn chỉnh. 

| Màu sắc | Đầu tiên | Cuối cùng | Vai trò DP | 
| --- | --- | --- | --- | 
| 3 | 1 | 2 | Màu hoàn chỉnh đầu tiên | 
| 1 | 3 | 4 | Mở rộng chuỗi | 
| 4 | 5 | 6 | Mở rộng chuỗi | 
| 2 | 7 | 8 | Mở rộng chuỗi | 

Điều kiện khoảng được giữ ở mỗi lần chuyển đổi, vì vậy tất cả bốn màu có thể được giữ nguyên hoàn toàn. 

| Tiểu bang | Giá trị | 
| --- | --- | 
| Màu sắc hoàn chỉnh | 4 | 
| Plumbuses mỗi màu | 2 | 
| Tổng số chưa được chạm tới | 8 | 
| Tổng số ống dẫn nước | 8 | 
| Di chuyển | 0 | 

Kết quả bằng 0, điều này cũng xác nhận rằng DP không buộc di chuyển điểm cuối không cần thiết khi sự sắp xếp ban đầu đã hợp lệ. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | O(NK + K2) | Mảng được quét với số lần không đổi và DP màu sẽ kiểm tra hầu hết các cặp K². | 
| Không gian | O(NK + K) | Mảng ban đầu và một trạng thái tiền tố đóng gói được lưu trữ cùng với dữ liệu DP và màu O(K). | 

Mảng lớn nhất chứa 1.000.000 dây dọi, do đó phần tuyến tính chỉ thực hiện vài triệu thao tác đơn giản. DP bậc hai chứa tối đa 1.000.000 kiểm tra cặp màu. Thuật toán tránh mọi sự phụ thuộc vào hoán vị của màu sắc hoặc tập hợp con của các dây dọi riêng lẻ. 

## Trường hợp thử nghiệm```python
import sys
import io
from array import array

# Put the submitted solve() implementation above this test section.

def run(inp: str) -> str:
    global input
    old_stdin = sys.stdin
    old_input = input
    try:
        sys.stdin = io.StringIO(inp)
        input = sys.stdin.readline
        return solve().strip()
    finally:
        sys.stdin = old_stdin
        input = old_input

# Provided samples
assert run(
    "3 3\n"
    "1 2 3 3 2 1 1 2 3\n"
) == "4", "sample 1"

assert run(
    "2 4\n"
    "3 3 1 1 4 4 2 2\n"
) == "0", "sample 2"

# Minimum-size input
assert run(
    "1 1\n"
    "1\n"
) == "0", "minimum size"

# All plumbuses already have one color group
assert run(
    "5 1\n"
    "1 1 1 1 1\n"
) == "0", "all equal"

# Alternating colors, so neither color can be kept as a complete
# group together with the other, but keeping one complete color
# still leaves an optimal solution.
assert run(
    "2 2\n"
    "1 2 1 2\n"
) == "2", "alternating boundary case"

# Two partial endpoint groups are optimal here.
assert run(
    "3 2\n"
    "1 1 2 2 1 2\n"
) == "2", "two partial endpoint colors"

# Maximum-size case: 1000 copies of each of two alternating colors.
# Keeping all 1000 occurrences of either color is optimal.
a = " ".join("1 2" for _ in range(1000))
assert run(
    "1000 2\n" + a + "\n"
) == "1000", "maximum size"
```| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 1 / 1`| 0 | Kích thước tối thiểu và xử lý một màu | 
|`5 1 / 1 1 1 1 1`| 0 | Tất cả các phần tử đã tạo thành một nhóm | 
|`2 2 / 1 2 1 2`| 2 | Các khoảng màu chồng chéo và DP màu hoàn chỉnh | 
|`3 2 / 1 1 2 2 1 2`| 2 | Giải pháp tối ưu sử dụng màu một phần điểm cuối | 
|`1000 2 / 1 2 ... 1 2`| 1000 | Kích thước đầu vào tối đa và xử lý DP/đầu vào lớn | 

## Vỏ cạnh 

Đối với trường hợp một màu```
1 1
1
```DP coi màu 1 là một nhóm hoàn chỉnh và giữ N=1 dây dọi. Không cần phân màu một phần bên trái hoặc bên phải, vì vậy số dây dọi chưa chạm tới tối đa là 1 và câu trả lời là 0. 

Đối với trường hợp luân phiên```
2 2
1 2 1 2
```các khoảng là`[1,3]`cho màu 1 và`[2,4]`đối với màu 2. Chúng trùng nhau nên không thể cả hai đều hoàn chỉnh. DP giữ lại màu sắc hoàn chỉnh, bảo toàn hai dây dọi và hai dây còn lại được chuyển đến các đầu. Câu trả lời là 2. 

Đối với trường hợp hai màu một phần```
3 2
1 1 2 2 1 2
```có một vết cắt sau bốn yếu tố đầu tiên. Tiền tố chứa hai số 1 và hậu tố chứa một số 1 và một số 2, nhưng sự sắp xếp hợp lệ nhất đạt được bằng cách di chuyển số 1 cuối cùng và số 2 cuối cùng, để lại`1 1 2 2`không bị ảnh hưởng. Đây đã là hai nhóm hoàn chỉnh nên chỉ cần hai thao tác là đủ. DP không có màu đầy đủ cũng được kiểm tra, nhưng biểu diễn toàn nhóm cho giá trị tối ưu là 2. 

Đối với trường hợp xen kẽ kích thước tối đa, mảng chứa 2000 phần tử khi N=1000 và K=2. Hai khoảng màu chồng lên nhau hoàn toàn nên chúng không thể giữ nguyên cả hai khoảng màu. Việc giữ một trong hai màu sẽ bảo toàn hoàn toàn 1000 quả mận, trong khi 1000 quả còn lại phải được di chuyển. Thuật toán trả về 1000 mà không thực hiện bất kỳ thao tác nào tỷ lệ với K lần độ dài mảng. 

Trường hợp xung đột điểm cuối đáng được quan tâm đặc biệt. Giả sử màu tiền tố tốt nhất và màu hậu tố tốt nhất đều là màu 1. Giữ cả hai sẽ tạo ra hai nhóm màu 1 riêng biệt, điều này là bất hợp pháp. Thuật toán lưu trữ hai ứng cử viên tốt nhất ở mỗi bên và thử cả hai lựa chọn. Nếu các ứng cử viên tốt nhất va chạm, ứng cử viên tốt thứ hai ở một bên có thể được chọn. Nếu không có ứng cử viên riêng biệt nào tồn tại thì điểm cuối tương ứng sẽ trống.
