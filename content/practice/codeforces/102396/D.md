---
title: "CF 102396D - Cắt Pizza"
description: "Chúng tôi có một chiếc bánh pizza hình tròn và (n) người. Người (i) cần một khu vực có góc chính xác là (alphai) độ. Các cung này có thể được đặt ở bất kỳ đâu trên bánh pizza và không cần phải xuất hiện theo thứ tự đầu vào. Bất kỳ phần nào chưa sử dụng của bánh pizza đều có thể được giữ lại trong hộp."
date: "2026-08-11T23:24:11+07:00"
tags: ["codeforces", "competitive-programming"]
categories: ["algorithms"]
codeforces_contest: 102396
codeforces_index: "D"
codeforces_contest_name: "2019-2020 Saint-Petersburg Open High School Programming Contest (SpbKOSHP 19)"
rating: 0
weight: 102396
solve_time_s: 662
verified: true
draft: false
---

[CF 102396D - Cắt Pizza](https://codeforces.com/problemset/problem/102396/D) 

**Đánh giá:** - 
**Thẻ:** - 
**Thời gian giải:** 11m 2s 
**Đã xác minh:** có 

##Giải pháp 
## Hiểu vấn đề 

Chúng tôi có một chiếc bánh pizza hình tròn và (n) người. Người (i) cần một khu vực có góc chính xác là (\alpha_i) độ. Các cung này có thể được đặt ở bất kỳ đâu trên bánh pizza và không cần phải xuất hiện theo thứ tự đầu vào. Bất kỳ phần nào chưa sử dụng của bánh pizza đều có thể được giữ lại trong hộp. 

Một vết cắt có thể là một bán kính tạo ra một tia biên hoặc một đường kính tạo ra hai tia biên đối diện cùng một lúc. Sau khi thực hiện tất cả các phần cắt, mọi khu vực được yêu cầu phải là một trong những khu vực kết quả, không có phần cắt bổ sung nào đi qua phần bên trong của nó. Nhiệm vụ là giảm thiểu số lần cắt và tạo ra một loạt các lần cắt thực tế để đạt được mức tối thiểu đó. Các ví dụ chính thức là (4) bản sao của (90^\circ), hai bản sao của (30^\circ) và (200^\circ,80^\circ,80^\circ). 

Giới hạn nhỏ (n\le16) là đầu mối thuật toán chính. Chúng ta không thể liệt kê tất cả các hoán vị vì (16!) bằng khoảng (2,09\cdot10^{13}). Khi chúng tôi cũng xem xét những phần nào thuộc về hai nửa đối diện của chiếc bánh pizza, việc tìm kiếm dựa trên hoán vị đơn giản vượt xa giới hạn một giây có thể chịu đựng được. Giải pháp dự định có thể cung cấp các thuật toán hàm mũ trong (n), nhưng nó cần một cái gì đó xung quanh (2^n) hoặc (3^n), chứ không phải thời gian giai thừa. Thực tế là mọi góc đều là số nguyên và toàn bộ chiếc bánh pizza chỉ có (360^\circ) cũng cho chúng ta một không gian trạng thái góc nhỏ, mặc dù giải pháp sạch hơn sử dụng tổng tập hợp con thay vì hình học trạng thái (360). 

Có một số trường hợp ranh giới có thể khiến việc triển khai có vẻ hợp lý trở nên sai lầm. Ví dụ: một yêu cầu duy nhất (180^\circ) chỉ cần một đường kính. đầu vào`1 / 180`có câu trả lời`1`, vì bản thân đường kính là hai ranh giới của nửa chiếc bánh pizza được yêu cầu. Việc coi mọi khu vực được yêu cầu là yêu cầu hai lần cắt bán kính độc lập sẽ tạo ra`2`. 

Một trường hợp quan trọng khác là khi tổng góc yêu cầu chính xác (360^\circ). Vì`3 / 200 80 80`, ba khu vực được yêu cầu có thể được đặt liên tiếp xung quanh toàn bộ chiếc bánh pizza. Ba tia biên là (0,200,280), do đó chỉ cần cắt ba bán kính. Một nghiệm luôn bắt đầu bằng (n+1) vết cắt sẽ tính không chính xác cả phần đầu và phần cuối là các ranh giới khác nhau, mặc dù chúng là cùng một tia sau một vòng quay hoàn toàn. 

Một trường hợp tế nhị hơn là`2 / 30 30`. Ba lần cắt bán kính là đủ nếu chúng ta đặt hai cung (30^\circ) cạnh nhau, nhưng hai lần cắt đường kính thì tốt hơn. Đường kính cắt tại (0^\circ) và (30^\circ) tạo ra các cung (30^\circ) ở cả (0\ldots30) và (180\ldots210). Hai lĩnh vực được yêu cầu là bản sao đối diện nhau, vì vậy câu trả lời là`2`. Đây chính xác là kiểu đối xứng mà sự sắp xếp tuyến tính thuần túy không có. 

Cuối cùng, một góc lớn hơn (180^\circ) sẽ ngăn cản việc sử dụng bất kỳ đường kính nào bên trong khu vực được yêu cầu đó. Ví dụ, trong`3 / 200 80 80`, một đường kính sẽ đặt một tia cắt bên trong khu vực (200^\circ) nếu khu vực đó vượt qua đường kính. Vì mỗi phần được yêu cầu phải là một khu vực hoàn chỉnh không có vết cắt bên trong, nên phần lớn buộc chúng tôi phải xây dựng chỉ có bán kính. 

## Phương pháp tiếp cận 

Lực lượng vũ phu trực tiếp nhất là quyết định cách các khu vực được yêu cầu được sắp xếp xung quanh chiếc bánh pizza, khu vực nào thuộc về hai hình bán nguyệt được tạo bởi đường kính có thể và nơi xảy ra ranh giới giữa các nhóm. Ngay cả khi chúng ta chỉ liệt kê các hoán vị và lựa chọn nhị phân cho hai vế, chúng ta cũng đã thu được đại khái 

[ 
n!,2^n 
] 

khả năng. Tại (n=16), đây là khoảng (1,37\cdot10^{18}) kết hợp. Việc kiểm tra từng cách sắp xếp cũng sẽ yêu cầu xây dựng các ranh giới của nó và đếm các cặp đường kính, vì vậy phương pháp này gần như không khả thi. 

Quan sát hữu ích là chúng ta không nên quan tâm đến danh tính chính xác của từng phần cắt trong khi xây dựng lời giải. Đường kính rất hữu ích vì nó có thể phục vụ hai tia biên đối diện trong một lần cắt. Cố định một đường kính và gọi hai tia của nó (0^\circ) và (180^\circ). Các lĩnh vực được yêu cầu sau đó có thể được phân phối giữa hai hình bán nguyệt. Bên trong hình bán nguyệt, một số lĩnh vực được yêu cầu có thể được đặt liên tiếp, với các khoảng trống tùy ý không được sử dụng giữa chúng. 

Giả sử chúng ta muốn hai nhóm các cung được yêu cầu, mỗi nhóm ở một bên của đường kính, bắt đầu ở cùng một ranh giới và kết thúc ở một ranh giới chung khác. Gọi tổng số góc yêu cầu của chúng là (x) và (y). Khoảng cách giữa hai ranh giới chung đó phải có độ dài ít nhất là (\max(x,y)). Nếu (x=y), cả hai nhóm có thể được xếp liên tiếp. Nếu (x<y), cạnh ngắn hơn vẫn có thể chạm vào cả hai ranh giới nếu nó chứa ít nhất hai cung được yêu cầu, vì chúng ta có thể chèn khoảng trống chưa sử dụng giữa các cung của nó. Nhóm một ngành không thể chạm vào cả hai điểm cuối trừ khi góc của nó bằng độ dài khoảng. 

Điều này mang lại cho chúng ta đối tượng tổ hợp trung tâm: một khối được ghép nối. Một khối được ghép nối bao gồm một tập hợp con (U) gồm những người được chia thành hai tập hợp con khác trống (A) và (B), một tập hợp con cho mỗi hình bán nguyệt. Độ dài cần thiết của nó là 

[ 
w(U)=\max\left(\sum_{i\in A}\alpha_i,\sum_{i\in B}\alpha_i\right). 
] 

Việc phân chia có hiệu lực khi cả hai nhóm có thể chạm vào cả hai đầu của khối. Vì một nhóm luôn có tổng (w(U)) nên chỉ có nhóm nhỏ hơn mới cần kiểm tra. Nếu tổng của nó hoàn toàn nhỏ hơn (w(U)), thì nó phải chứa ít nhất hai cung để góc bị thiếu có thể được chèn làm khoảng trống bên trong. Nếu hai tổng bằng nhau thì nhóm một ngành cũng được. 

Mỗi khối được ghép nối sẽ cho chúng ta thêm một ranh giới chung giữa hai hình bán nguyệt. Một ranh giới chung giúp tiết kiệm một lần cắt vì một đường kính có thể phục vụ cả hai mặt. Các khối được ghép nối được đặt liên tiếp từ (0^\circ) tới (180^\circ). 

Các lĩnh vực được yêu cầu còn lại không cần phải ghép nối. Sau khi tất cả các khối ghép nối đã được đặt, giả sử tổng chiều dài của chúng là (P). Có (180-P) độ còn lại trong mỗi hình bán nguyệt. Các cung còn lại phải được chia thành hai hình bán nguyệt sao cho tổng góc ấn định cho mỗi bên nhiều nhất bằng công suất còn lại này. Đây chỉ là một kiểm tra tổng hợp con. 

Còn một khoản tiết kiệm nữa cần xử lý. Nếu một hình bán nguyệt được lấp đầy chính xác tới (180^\circ), thì tia đầu và tia cuối của nó đối diện nhau và thực tế là một đường kính cắt. Vì vậy, ranh giới đầu và cuối không nên được tính riêng. Ý tưởng tương tự xử lý một sự sắp xếp hoàn chỉnh chỉ theo bán kính (360^\circ). 

Bởi vì (n) chỉ là (16), nên chúng ta có thể liệt kê mọi tập hợp con và tính toán cách phân chia theo cặp tốt nhất có thể cho tập hợp con đó. Sau đó, tập hợp con DP sẽ phân chia một số người thành các khối được ghép nối đồng thời giảm thiểu tổng chiều dài mà các khối đó tiêu thụ. Đối với mỗi số khối được ghép nối, chúng tôi giữ lại góc tiêu thụ tối thiểu có thể. Cuối cùng, chúng tôi kiểm tra xem những người chưa ghép đôi có thể lọt vào hai hình bán nguyệt còn lại hay không. 

Sự so sánh là:

| Tiếp cận | Độ phức tạp thời gian | Độ phức tạp của không gian | Phán quyết | 
| --- | --- | --- | --- | 
| Lực lượng vũ phu | (O(n!2^n)) | (O(n)) | Quá chậm | 
| Tập hợp con DP | (O(n3^n)) | (O(n2^n)) | Đã chấp nhận | 

## Hướng dẫn thuật toán 

1. Tính tổng góc yêu cầu (S=\sum\alpha_i). Giá trị (S) cho chúng ta biết không gian góc phải chiếm bao nhiêu và ngay lập tức đưa ra yêu cầu về dung tích của hai hình bán nguyệt. 
2. Với mỗi tập hợp con (U), hãy tính tổng góc của nó`sum[U]`. Chúng tôi cũng tính toán một tập hợp bit mô tả mọi tổng tập hợp con có thể đạt được từ (U), cùng với tập hợp bit thứ hai chứa các tổng có thể đạt được bằng cách sử dụng ít nhất hai phần tử. 
3. Với mỗi tập hợp con (U) chứa ít nhất hai người, hãy tìm cạnh nhỏ nhất có thể có của một phép chia hợp lệ (U=A\cup B). Nếu hai bên có số tiền bằng nhau thì việc chia đôi có hiệu lực ngay lập tức. Ngược lại, cạnh nhỏ hơn phải chứa ít nhất hai người, vì nó cần một khoảng trống bên trong để chạm vào cả hai đầu của khối ghép nối. 
4. Lưu trữ độ dài khối kết quả và phần phân chia thực tế. Nếu cạnh nhỏ có tổng (t) thì cạnh lớn có tổng`sum[U] - t`, do đó khối tiêu thụ`sum[U] - t`độ trong mỗi hình bán nguyệt. 
5. Chạy tập hợp con DP. Nhà nước`dp[mask][k]`lưu trữ tổng chiều dài tối thiểu được chiếm bởi chính xác (k) khối được ghép nối chỉ sử dụng những người từ`mask`. Những người không được bao gồm trong`mask`vẫn miễn phí và sẽ được đặt sau. 
6. Khi xử lý một chiếc mặt nạ, hãy nhìn vào người ít quan trọng nhất trên đó. Hoặc để nguyên người đó hoặc tạo một khối ghép nối có chứa người đó. Việc hạn chế mọi khối được chọn để chứa ít người còn lại nhất sẽ mang lại cho mỗi phân vùng một sự phân tách duy nhất và tránh việc đếm cùng một tập hợp các khối theo nhiều thứ tự. 
7. Đối với mỗi trạng thái DP, hãy để độ dài khối ghép đôi bị chiếm dụng là (P). Dung lượng còn lại của mỗi hình bán nguyệt là (C=180-P). Những người chưa ghép đôi có thể được phân bổ chính xác giữa hai bên khi một số tập hợp con trong số họ có tổng giữa`remaining_sum - C`Và`C`. Tập hợp bit tổng con của chúng cho phép chúng tôi kiểm tra điều này trong các hoạt động bit có thời gian không đổi sau khi biết trạng thái DP. 
8. Trong số tất cả các trạng thái khả thi, hãy tối đa hóa số lượng (k) khối được ghép nối. Nếu một số trạng thái có cùng (k), hãy ưu tiên một trạng thái trong đó hình bán nguyệt có thể được lấp đầy chính xác đến (180^\circ), vì khi đó hai tia điểm cuối của nó được cắt một đường kính và lưu thêm một vết cắt nữa. 
9. Xây dựng lại các khối được ghép nối từ con trỏ cha DP. Đối với mỗi khối được ghép nối, đặt nhóm đầu tiên của nó vào hình bán nguyệt phía trên và nhóm thứ hai ở hình bán nguyệt phía dưới. Cả hai nhóm đều bắt đầu tại ranh giới chung hiện tại và kết thúc ở ranh giới chung tiếp theo. Nếu tổng của một nhóm nhỏ hơn chiều dài khối và nó chứa ít nhất hai cung, hãy đặt góc không sử dụng giữa các cung của nó. 
10. Đặt những người còn lại sau các khối được ghép nối, chia chúng theo cách tái cấu trúc tổng tập hợp con. Mỗi bên bây giờ vừa với dung lượng còn lại (180^\circ). 
11. Thu thập mọi tia cần thiết làm ranh giới khu vực. Đối với mỗi cặp tia đối diện (x) và (x+180), phát ra một đường kính nếu cần cả hai tia. Nếu không thì phát ra bán kính cho tia cần thiết. Quá trình nén cuối cùng này cũng tự động xử lý các trường hợp ranh giới (180^\circ) và (360^\circ). 

### Tại sao nó hoạt động 

Mọi đường kính hữu ích đều tạo ra một ranh giới cần thiết cho cả hai hình bán nguyệt hoặc xác định hai đầu của hình bán nguyệt. Giữa hai ranh giới chung liên tiếp, các lĩnh vực được yêu cầu ở mỗi bên tạo thành hai nhóm. Tổng các góc của chúng xác định khoảng cách tối thiểu có thể có giữa các ranh giới chung, chính xác là khoảng cách lớn nhất của tổng hai nhóm. Điều kiện hợp lệ cho một nhóm ngắn hơn phụ thuộc vào việc liệu nó có thể chạm vào cả hai điểm cuối hay không. 

DP xem xét mọi nhóm ghép đôi có thể có vì mọi phân chia hợp lệ của một tập hợp con đều được biểu thị bằng tập hợp con (U) của nó. Nó cũng xem xét mọi tập hợp có thể có của các nhóm ghép đôi rời rạc vì phép lặp lại bit được đặt nhỏ nhất sẽ khiến người đầu tiên không được ghép đôi hoặc đặt người đó vào chính xác một khối đã chọn. Đối với mỗi số khối, nó giữ góc chiếm tối thiểu, do đó, không có trạng thái nào có cùng số người được sử dụng và số khối có thể phù hợp hơn với những người còn lại. 

Sau khi các khối ghép nối được cố định, câu hỏi duy nhất còn lại là liệu các khu vực không được sử dụng có phù hợp với hai dung lượng hình bán nguyệt còn lại hay không. Kiểm tra tổng tập hợp con chính xác là điều kiện đó. Do đó, mọi sự sắp xếp hình học khả thi đều được thể hiện bằng một số trạng thái DP và mọi trạng thái DP vượt qua bài kiểm tra năng lực đều có thể được xây dựng về mặt hình học. Tối đa hóa số lượng khối được ghép nối, sau đó thực hiện lưu điểm cuối (180^\circ) khi có sẵn, giảm thiểu số lần cắt. 

## Giải pháp Python```python
import sys
from array import array

input = sys.stdin.readline

def solve():
    n = int(input())
    a = list(map(int, input().split()))

    N = 1 << n
    ALL = N - 1
    INF = 10**9

    total = [0] * N
    reach = [0] * N
    reach2 = [0] * N
    popcnt = [0] * N

    # Bit i in reach[mask] means that this subset of mask
    # can have total angle i.
    reach[0] = 1

    for mask in range(1, N):
        bit = mask & -mask
        i = bit.bit_length() - 1
        rem = mask ^ bit

        total[mask] = total[rem] + a[i]
        popcnt[mask] = popcnt[rem] + 1

        r = reach[rem]
        reach[mask] = r | (r << a[i])

        # At least two elements:
        # either a >=2-element subset already exists in rem,
        # or we take i together with a nonempty subset of rem.
        nonempty = r & ~1
        reach2[mask] = reach2[rem] | (nonempty << a[i])

    # For each subset U:
    # weight[U] = minimum length of a paired block using U.
    # split[U] = one side of the corresponding split.
    weight = array('H', [0]) * N
    split = array('H', [0]) * N

    for mask in range(1, N):
        if popcnt[mask] < 2:
            continue

        s = total[mask]
        half = s // 2

        # First try a perfectly balanced split.
        if s % 2 == 0 and ((reach[mask] >> half) & 1):
            small = half
            need_two = False
        else:
            # Otherwise the smaller side must contain >= 2 elements.
            limited = reach2[mask] & ((1 << (half + 1)) - 1)
            if not limited:
                continue
            small = limited.bit_length() - 1
            need_two = True

        large = s - small

        # The paired block must fit into a semicircle.
        if large > 180:
            continue

        # Recover an actual subset having sum == small.
        x = mask
        target = small
        side = 0

        while target:
            bit = x & -x
            i = bit.bit_length() - 1
            rem = x ^ bit

            source = reach2[rem] if need_two else reach[rem]

            if (source >> target) & 1:
                x = rem
            else:
                side |= bit
                target -= a[i]
                x = rem

        if side == 0:
            continue

        weight[mask] = large
        split[mask] = side

    # dp[mask][k] = minimum total length of k paired blocks
    # using exactly the people in mask.
    K = n // 2
    W = K + 1

    dp = [None] * N
    dp[0] = [INF] * W
    dp[0][0] = 0

    # choice[mask * W + k] is the paired block used to obtain
    # the state. Zero means that the least significant person
    # was left unpaired.
    choice = array('H', [0]) * (N * W)

    for mask in range(1, N):
        bit = mask & -mask
        without = mask ^ bit

        cur = dp[without][:]

        sub = without
        while sub:
            block = sub | bit
            w = weight[block]

            if w:
                rem = mask ^ block
                prev = dp[rem]

                max_k = min(K - 1, popcnt[rem] // 2)

                for k in range(max_k + 1):
                    old = prev[k]
                    if old == INF:
                        continue

                    nw = old + w
                    if nw < cur[k + 1]:
                        cur[k + 1] = nw
                        choice[mask * W + k + 1] = block

            sub = (sub - 1) & without

        dp[mask] = cur

    # We need enough saving to fit everything into two semicircles.
    required = max(0, total[ALL] - 180)

    best_k = -1
    best_mask = -1
    best_p = INF
    best_e = False
    best_left = 0

    # Try the largest number of paired blocks first.
    for k in range(K, -1, -1):
        found = False
        found_e = False
        candidate = None

        for mask in range(N):
            p = dp[mask][k]
            if p == INF or p > 180:
                continue

            capacity = 180 - p
            rem = ALL ^ mask
            rs = total[rem]

            # The remaining people must be split between the
            # two semicircles, each with capacity 'capacity'.
            low = max(0, rs - capacity)
            high = min(capacity, rs)

            if low > high:
                continue

            bits = reach[rem]
            allowed = bits & ((1 << (high + 1)) - 1)

            if low:
                allowed &= ~((1 << low) - 1)

            if not allowed:
                continue

            # Prefer an exact capacity on one side.
            exact = (
                capacity <= high
                and capacity >= low
                and ((bits >> capacity) & 1)
            )

            if exact and not found_e:
                found_e = True
                candidate = (mask, p, capacity, rem, True)
            elif not found_e and candidate is None:
                target = allowed.bit_length() - 1
                candidate = (mask, p, capacity, rem, False)

            found = True

        if found:
            best_k = k
            best_mask, best_p, capacity, rem, best_e = candidate
            break

    # If no partition into two semicircles exists, no diameter can
    # be used without cutting through a requested sector.
    if best_k == -1:
        need = [False] * 360
        need[0] = True

        cur_angle = 0
        for x in a:
            cur_angle += x
            if cur_angle < 360:
                need[cur_angle] = True

        cuts = []
        for ang in range(180):
            x = need[ang]
            y = need[ang + 180]

            if x and y:
                cuts.append((ang, 1))
            elif x:
                cuts.append((ang, 0))
            elif y:
                cuts.append((ang + 180, 0))

        out = [str(len(cuts))]
        out.extend(f"{ang} {typ}" for ang, typ in cuts)
        sys.stdout.write("\n".join(out))
        return

    # Recover the paired blocks.
    blocks = []
    mask = best_mask
    k = best_k

    while mask:
        block = choice[mask * W + k]

        if block:
            blocks.append((block, split[block]))
            mask ^= block
            k -= 1
        else:
            bit = mask & -mask
            mask ^= bit

    blocks.reverse()

    # Recover the remaining people assigned to one semicircle.
    paired_mask = best_mask
    remaining = ALL ^ paired_mask
    capacity = 180 - best_p

    rs = total[remaining]
    low = max(0, rs - capacity)
    high = min(capacity, rs)

    bits = reach[remaining]

    if best_e and ((bits >> capacity) & 1):
        target = capacity
    else:
        allowed = bits & ((1 << (high + 1)) - 1)
        if low:
            allowed &= ~((1 << low) - 1)
        target = allowed.bit_length() - 1

    top_remaining = 0
    x = remaining
    t = target

    while t:
        bit = x & -x
        i = bit.bit_length() - 1
        rem = x ^ bit

        if (reach[rem] >> t) & 1:
            x = rem
        else:
            top_remaining |= bit
            t -= a[i]
            x = rem

    bottom_remaining = remaining ^ top_remaining

    need = [False] * 360
    need[0] = True

    def place_group(mask, start, length):
        if mask == 0:
            return

        ids = []
        x = mask
        while x:
            bit = x & -x
            ids.append(bit.bit_length() - 1)
            x ^= bit

        cur = start

        for j, i in enumerate(ids):
            if j + 1 == len(ids):
                end = start + length
            else:
                cur += a[i]
                end = cur

            need[end % 360] = True

    pos = 0

    # Paired blocks occupy the same interval in both semicircles.
    for block, side_a in blocks:
        side_b = block ^ side_a

        sa = total[side_a]
        sb = total[side_b]
        length = max(sa, sb)

        place_group(side_a, pos, length)
        place_group(side_b, 180 + pos, length)

        pos += length

    # Place unpaired people after all paired blocks.
    def place_consecutive(mask, start):
        cur = start
        x = mask

        while x:
            bit = x & -x
            i = bit.bit_length() - 1
            cur += a[i]
            need[cur % 360] = True
            x ^= bit

    place_consecutive(top_remaining, pos)
    place_consecutive(bottom_remaining, 180 + pos)

    # Compress opposite required rays into diameter cuts.
    cuts = []

    for ang in range(180):
        x = need[ang]
        y = need[ang + 180]

        if x and y:
            cuts.append((ang, 1))
        elif x:
            cuts.append((ang, 0))
        elif y:
            cuts.append((ang + 180, 0))

    out = [str(len(cuts))]
    out.extend(f"{ang} {typ}" for ang, typ in cuts)
    sys.stdout.write("\n".join(out))

if __name__ == "__main__":
    solve()
```Phần đầu tiên của quá trình triển khai xây dựng các tổng tập hợp con dưới dạng số nguyên Python được sử dụng làm tập hợp bit. Một bit ở vị trí (x) có nghĩa là có thể đạt được góc (x) bằng cách chọn một số người từ tập hợp con. Các số nguyên có độ chính xác tùy ý của Python làm cho các chuyển đổi tổng tập hợp con này trở nên rất nhỏ gọn và nhanh chóng. 

Bitset thứ hai,`reach2`, đại diện cho số tiền có thể đạt được bằng cách sử dụng ít nhất hai người. Sự phân biệt này là cần thiết cho một khối ghép đôi không bằng nhau. Nếu một bên có tổng (x) và bên kia có tổng (y>x), thì bên ngắn hơn cần ít nhất hai cung để chạm vào cả hai đầu của khoảng. Chỉ với một khu vực, nó sẽ phải có góc chính xác (y). 

các`weight`mảng lưu trữ độ dài khoảng tối thiểu của một khối được ghép nối hợp lệ. Nếu một tập hợp con có tổng (s) và cạnh nhỏ hơn của nó có thể có tổng (t), thì cạnh lớn hơn có tổng (s-t), do đó khoảng tiêu tốn`s - t`độ. Việc chọn (t) hợp lệ lớn nhất có thể sẽ giảm thiểu khoảng đó. 

Tập hợp con chính DP sử dụng người ít quan trọng nhất làm lựa chọn chính tắc. Để người đó không ghép nối, sao chép trạng thái từ mặt nạ mà không có người đó. Nếu không, khối được ghép nối chứa người đó sẽ bị xóa và trọng lượng của nó sẽ được thêm vào. Lựa chọn chuẩn này ngăn chặn việc tạo cùng một phân vùng khối theo mọi thứ tự khối có thể. 

Ở giai đoạn cuối,`dp[mask][k]`cho chúng ta biết bao nhiêu trong số hai hình bán nguyệt đã được các khối ghép nối (k) dành sẵn. Phần bổ sung chứa tất cả những người chưa ghép đôi. Tập hợp bit tổng con xác định liệu các cung còn lại đó có thể được phân chia giữa hai dung lượng còn lại hay không. 

Việc tái thiết phản ánh chính xác DP. Một khối ghép đôi được đặt trong cùng một khoảng góc trên cả hai hình bán nguyệt. Nếu một bên có góc được yêu cầu ít hơn độ dài khoảng, thì việc triển khai sẽ đặt góc không sử dụng ngay trước khu vực cuối cùng. Điều này đảm bảo rằng tia đầu tiên và tia cuối cùng của nhóm vẫn là ranh giới khu vực được yêu cầu thực tế. 

trận chung kết`need`mảng được xây dựng có chủ ý như một tập hợp các tia thay vì phát ra các vết cắt trực tiếp. Điều này tránh các trường hợp đặc biệt dễ vỡ xung quanh (0^\circ), (180^\circ) và (360^\circ). Một khi đã biết tất cả các tia cần thiết, mỗi cặp đối cực được biểu diễn một cách tự nhiên bằng một đường kính. 

Số nguyên Python không tràn ở đây và tổng tập hợp con lớn nhất chỉ là (360). Chi tiết triển khai chính cần được quan tâm là sự phân biệt giữa môđun góc (360) và hai tia khác nhau (0^\circ) và (360^\circ). Chúng là cùng một tia nên mã luôn lưu trữ các góc modulo (360). 

## Ví dụ đã hoạt động 

### Mẫu 1 

Đầu vào là:```
4
90 90 90 90
```Chúng ta có thể chia bốn yêu cầu thành hai khối ghép nối. Mỗi khối chứa một cung (90^\circ) ở mỗi bên, vì vậy mỗi khối có chiều dài (90^\circ). Hai khối chiếm toàn bộ hình bán nguyệt (180^\circ). 

| Bước | Khối ghép nối | Tổng trên | Tổng thấp hơn | Chiều dài khối | Vị trí | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 90/90 | 90 | 90 | 90 | 0 -> 90 | 
| 2 | 90/90 | 90 | 90 | 90 | 90 -> 180 | 

Các tia cần thiết là (0^\circ,90^\circ,180^\circ,270^\circ). Các cặp (0/180) và (90/270) đối cực nhau nên mỗi cặp trở thành một đường kính. 

Do đó, đầu ra chứa hai phần cắt, ví dụ:```
2
0 1
90 1
```Điều này thể hiện cả hai nguồn tiết kiệm. Mỗi khối được ghép nối đưa ra một ranh giới chung và vì toàn bộ hình bán nguyệt đạt tới (180^\circ), hai điểm cuối của hình bán nguyệt đó là một đường kính. 

### Mẫu 2 

Đầu vào là:```
2
30 30
```Hai yêu cầu tạo thành một khối ghép nối. 

| Bước | Khối ghép nối | Tổng trên | Tổng thấp hơn | Chiều dài khối | Vị trí | 
| --- | --- | --- | --- | --- | --- | 
| 1 | 30/30 | 30 | 30 | 30 | 0 -> 30 | 

Hai lĩnh vực được đặt tại (0\ldots30) và (180\ldots210). Các tia cần thiết là (0^\circ,30^\circ,180^\circ,210^\circ). 

| Cặp tia | Yêu cầu? | Cắt | 
| --- | --- | --- | 
| 0 và 180 | cả hai | đường kính tại 0 | 
| 30 và 210 | cả hai | đường kính 30 | 

Như vậy hai vết cắt là đủ. 

Ví dụ này chứng minh tại sao chỉ sắp xếp các yêu cầu một cách liên tục là không đủ. Vị trí liên tiếp sẽ sử dụng các tia (0,30,60), yêu cầu ba lần cắt. Việc phân chia các yêu cầu giữa các hình bán nguyệt đối diện sẽ tạo thêm một ranh giới chung và giảm câu trả lời xuống còn hai. 

## Phân tích độ phức tạp 

| Đo | Độ phức tạp | Giải thích | 
| --- | --- | --- | 
| Thời gian | (O(n3^n)) | Tập hợp con DP xem xét các mặt nạ con chứa phần tử tối thiểu chính tắc, với trạng thái đếm khối lên tới (O(n)). | 
| Không gian | (O(n2^n)) | Trạng thái DP, tập hợp bit tổng con và thông tin tái tạo được lưu trữ cho tất cả các mặt nạ. | 

Đối với (n\le16), (2^n=65536), do đó không gian trạng thái hàm mũ đủ nhỏ cho giải pháp mong muốn. Các phép toán tổng tập hợp con đặc biệt hiệu quả vì chúng sử dụng số nguyên Python làm tập hợp bit. Giới hạn chuyển tiếp (3^n) là phần chiếm ưu thế trong thời gian chạy, nhưng (3^{16}=43,046,721), rất thiết thực với việc biểu diễn tập hợp con nhỏ gọn và cắt tỉa theo số lượng khối ghép đôi có thể có. 

## Trường hợp thử nghiệm 

Các thử nghiệm sau đây xác nhận các vết cắt được tạo ra thay vì so sánh các góc chính xác được chương trình in ra, bởi vì bài toán cho phép bất kỳ cấu trúc tối ưu nào.```python
# Save the submitted solution as solution.py before running this file.

import subprocess

def run(inp: str) -> str:
    p = subprocess.run(
        ["python3", "solution.py"],
        input=inp,
        text=True,
        capture_output=True,
        check=True,
    )
    return p.stdout

def validate(inp: str, out: str, expected_min_cuts: int):
    data = list(map(int, inp.split()))
    n = data[0]
    a = data[1:1 + n]

    lines = out.strip().splitlines()
    m = int(lines[0])

    assert m == len(lines) - 1
    assert m == expected_min_cuts

    rays = set()

    for line in lines[1:]:
        angle, typ = map(int, line.split())
        assert 0 <= angle < 360
        assert typ in (0, 1)

        rays.add(angle)

        if typ == 1:
            rays.add((angle + 180) % 360)

    rays = sorted(rays)
    assert rays

    sectors = []
    for i in range(len(rays)):
        x = rays[i]
        y = rays[(i + 1) % len(rays)]
        if i + 1 == len(rays):
            y += 360
        sectors.append(y - x)

    sectors.sort()

    wanted = sorted(a)

    # Every requested sector must occur as a complete atomic sector.
    i = 0
    j = 0
    while i < len(wanted) and j < len(sectors):
        if wanted[i] == sectors[j]:
            i += 1
        j += 1

    assert i == len(wanted)

# Sample 1
sample1 = """\
4
90 90 90 90
"""
out = run(sample1)
validate(sample1, out, 2)

# Sample 2
sample2 = """\
2
30 30
"""
out = run(sample2)
validate(sample2, out, 2)

# Sample 3
sample3 = """\
3
200 80 80
"""
out = run(sample3)
validate(sample3, out, 3)

# Minimum-size input: one 180-degree sector is exactly one diameter.
case4 = """\
1
180
"""
out = run(case4)
validate(case4, out, 1)

# All equal values. Eight opposite pairs of 1-degree sectors
# require nine distinct boundary positions.
case5 = """\
16
1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1
"""
out = run(case5)
validate(case5, out, 9)

# Boundary case: the requested sectors fill exactly one semicircle.
case6 = """\
3
60 60 60
"""
out = run(case6)
validate(case6, out, 3)

# A 180-degree subset can be made into one side of a diameter,
# reducing the number of radius cuts.
case7 = """\
3
100 80 50
"""
out = run(case7)
validate(case7, out, 3)
```Các trường hợp tùy chỉnh bao gồm các ranh giới cấu trúc quan trọng: 

| Kiểm tra đầu vào | Sản lượng dự kiến ​​| Nó xác nhận những gì | 
| --- | --- | --- | 
|`1 / 180`|`1`| Một nửa chiếc bánh pizza được cắt một đường kính. | 
|`16 / sixteen 1s`|`9`| Tối đa (n), nhiều khối được ghép nối và các góc bằng nhau lặp lại. | 
|`3 / 60 60 60`|`3`| Tổng số chính xác (180^\circ) và trường hợp nhận dạng điểm cuối. | 
|`3 / 100 80 50`|`3`| Tập hợp con (180^\circ) có thể tạo đường kính hữu ích ngay cả khi không có hai nhóm đối xứng. | 

## Vỏ cạnh 

Đối với một yêu cầu (180^\circ), đầu vào là:```
1
180
```Việc xây dựng đặt khu vực từ (0^\circ) đến (180^\circ). Cả hai tia đều có cùng đường kính nên lần nén cuối cùng nhìn thấy cặp cần thiết (0/180) và phát ra một đường kính. Đầu ra có chính xác một lần cắt. 

Để sắp xếp đầy đủ (360^\circ), hãy xem xét:```
3
200 80 80
```Không có đường kính nào có thể vượt qua yêu cầu (200^\circ) một cách an toàn, do đó giải pháp quay trở lại các lần cắt bán kính liên tiếp. Các ranh giới là (0,200,280) và sau khu vực (80^\circ) cuối cùng, góc quay trở lại (360^\circ=0^\circ). Ranh giới đầu tiên và cuối cùng là cùng một tia, tạo ra chính xác ba đường cắt. 

Đối với các yêu cầu tương tự được lặp đi lặp lại, hãy xem xét:```
2
30 30
```DP tìm thấy một khối được ghép nối chứa cả hai người, được chia thành (30/30). Chiều dài của nó là (30^\circ). Các lĩnh vực được đặt từ (0) đến (30) và từ (180) đến (210). Cả hai cặp ranh giới đều đối cực, do đó, hai đường cắt cần thiết có đường kính tại (0^\circ) và (30^\circ). 

Đối với trường hợp ít rõ ràng hơn:```
3
100 80 50
```Các cung (100^\circ) và (80^\circ) có thể chiếm một hình bán nguyệt hoàn chỉnh, từ (0^\circ) đến (180^\circ). Khu vực (50^\circ) chiếm (180^\circ) đến (230^\circ) trong hình bán nguyệt còn lại. Các tia cần thiết là (0^\circ,100^\circ,180^\circ,230^\circ). Vì (0^\circ) và (180^\circ) đối nhau nên một đường kính thay thế hai lần cắt bán kính. Câu trả lời cuối cùng là ba vết cắt. 

DP xử lý trường hợp cuối cùng này thông qua phân vùng hai hình bán nguyệt cuối cùng thay vì yêu cầu DP khối ghép đôi tạo ra một cặp đối xứng các khu vực được yêu cầu. Sự khác biệt này quan trọng vì đường kính được phép tạo thêm ranh giới ở phía đối diện. Tia đối diện không nhất thiết phải là điểm cuối của khu vực được yêu cầu khác, miễn là phần cắt bổ sung không đi qua khu vực được yêu cầu. 

Đối với phiên bản có kích thước tối đa với 16 yêu cầu bằng nhau (1^\circ), DP có thể tạo tám khối được ghép nối. Mỗi cặp chiếm một độ trên các hình bán nguyệt đối diện, tạo ra các ranh giới chung tại (0,1,\ldots,8). Cần phải cắt 9 đường kính. Phần còn lại (172^\circ) ở mỗi bên đơn giản là không được sử dụng nên không cần cắt thêm.
